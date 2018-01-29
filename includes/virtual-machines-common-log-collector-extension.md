
Diagnostisera problem med en Microsoft Azure-molntjänst kräver att samla in tjänstens loggfiler på virtuella datorer som problem uppstå. Du kan använda den AzureLogCollector tillägg på begäran för att utföra en samling av loggar från en eller flera moln virtuella datorer för tjänsten (från både webb- och arbetsroller) och överföring av insamlade filer till ett Azure storage-konto – utan att behöva logga in via en fjärranslutning till någon på de virtuella datorerna.

> [!NOTE]
> Beskrivningar för de flesta loggade informationen finns på http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.
> 
> 

Det finns två lägen mängden beroende på vilka typer av filer ska samlas in.

* **Azure gäst-agenten loggar endast (GA)**. Läget för den här samlingen innehåller alla loggar som rör Azure gästagenter och andra Azure-komponenter.
* **Alla loggar (fullständig)**. Den här samlingsläget samlar in alla filer i GA läge plus:
  
  * händelseloggarna system och program
  * Felloggarna för HTTP
  * IIS-loggar
  * Installationsloggarna
  * andra systemloggar

I båda lägena samling, kan ytterligare data collection mappar anges med hjälp av en samling med följande struktur:

* **Namnet**: namnet på samlingen som namnet på undermappen i zip-filen med insamlade filer.
* **Plats**: sökvägen till mappen på den virtuella datorn där filer ska samlas in.
* **SearchPattern**: mönstret för namnen på filer som ska samlas in. Standardvärdet är ”\*”
* **Rekursiva**: om filer ska samlas in hitta rekursivt under den angivna platsen.

## <a name="prerequisites"></a>Förutsättningar
* Har ett lagringskonto för tillägg att spara genererade zip-filer.
* Använda Azure PowerShell-Cmdlets v0.8.0 eller senare. Mer information finns i [Azure hämtar](https://azure.microsoft.com/downloads/).

## <a name="add-the-extension"></a>Lägg till tillägget
Du kan använda [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) cmdlets eller [Service Management REST API: er](https://msdn.microsoft.com/library/ee460799.aspx) lägga till filnamnstillägget AzureLogCollector.

För molntjänster, befintliga Azure Powershell-cmdleten **Set AzureServiceExtension**, kan användas för att aktivera tillägget för rollinstanser för Molntjänsten. Varje gång det här tillägget aktiveras via denna cmdlet, utlöses Logginsamling på de valda rollinstanserna av valda roller.

För virtuella datorer, den befintliga Azure Powershell-cmdleten **Set AzureVMExtension**, kan användas för att aktivera tillägget på virtuella datorer. Varje gång det här tillägget aktiveras via cmdlets, utlöses Logginsamling på varje-instans.

Det här tillägget används internt, JSON-baserade PublicConfiguration och PrivateConfiguration. Följande är layouten för ett exempel JSON för offentliga och privata konfiguration.

### <a name="publicconfiguration"></a>PublicConfiguration

```json
{
    "Instances":  "*",
    "Mode":  "Full",
    "SasUri":  "SasUri to your storage account with sp=wl",
    "AdditionalData":
    [
      {
              "Name":  "StorageData",
              "Location":  "%roleroot%storage",
              "SearchPattern":  "*.*",
              "Recursive":  "true"
      },
      {
            "Name":  "CustomDataFolder2",
            "Location":  "c:\customFolder",
            "SearchPattern":  "*.log",
            "Recursive":  "false"
      },
    ]
}
```

### <a name="privateconfiguration"></a>PrivateConfiguration

```json
{

}
```

> [!NOTE]
> Det här tillägget behöver inte **privateConfiguration**. Du kan bara ange en tom struktur för den **– PrivateConfiguration** argumentet.
> 
> 

Du kan följa en av två följande steg för att lägga till AzureLogCollector till en eller flera instanser av en molnbaserad tjänst eller en virtuell dator för valda roller, som utlöser samlingar på varje virtuell dator kör och skicka de insamlade filerna till Azure-konto har angetts.

## <a name="adding-as-a-service-extension"></a>Lägga till som ett tillägg för tjänsten
1. Följ instruktionerna för att ansluta Azure PowerShell till din prenumeration.
2. Ange namn, plats, roller och rollen tjänstinstanser som du vill lägga till och aktivera tillägget AzureLogCollector.

  ```powershell
  #Specify your cloud service name
  $ServiceName = 'extensiontest2'

  #Specify the slot. 'Production' or 'Staging'
  $slot = 'Production'

  #Specified the roles on which the extension will be installed and enabled
  $roles = @("WorkerRole1","WebRole1")

  #Specify the instances on which extension will be installed and enabled.  Use wildcard * for all instances
  $instances = @("*")

  #Specify the collection mode, "Full" or "GA"
  $mode = "GA"
  ```

3. Ange mappen ytterligare data som samlas in filer (det här steget är valfritt).

  ```powershell
  #add one location
  $a1 = New-Object PSObject

  $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
  $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
  $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
  $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"

  $AdditionalDataList+= $a1
  #more locations can be added....
  ```

   > [!NOTE]
   > Du kan använda token `%roleroot%` att ange rotenheten roll eftersom den inte använder en fast enhet.
   > 
   > 
4. Ange det Azure storage-kontonamnet och nyckeln insamlade filer överförs.

  ```powershell
  $StorageAccountName = 'YourStorageAccountName'
  $StorageAccountKey  = 'YourStorageAccountKey'
  ```

5. Anropa SetAzureServiceLogCollector.ps1 (ingår i slutet av artikeln) på följande sätt om du vill aktivera tillägget AzureLogCollector för en tjänst i molnet. När körningen har slutförts, kan du hitta den överförda filen under`https://YourStorageAccountName.blob.core.windows.net/vmlogs`

  ```powershell
  .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList
  ```

Följande är definitionen av parametrar för skriptet. (Detta kopieras nedan samt.)

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
  [Parameter(Mandatory=$true)]
  [string]   $ServiceName,

  [Parameter(Mandatory=$false)]
  [string[]] $Roles ,

  [Parameter(Mandatory=$false)]
  [string[]] $Instances,

  [Parameter(Mandatory=$false)]
  [string]   $Slot = 'Production',

  [Parameter(Mandatory=$false)]
  [string]   $Mode = 'Full',

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountName,

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountKey,

  [Parameter(Mandatory=$false)]
  [PSObject[]] $AdditionDataLocationList = $null
)
```

* **ServiceName**: din molntjänstnamnet.
* **Roller**: en lista över roller, till exempel ”WebRole1” eller ”WorkerRole1”.
* **Instanser**: en lista med namnen på rollinstanser avgränsade med kommatecken--Använd strängen med jokertecken (”*”) för alla rollinstanser.
* **Fack**: platsnamnet. ”Produktion” eller ”mellanlagring”.
* **Läget**: samlingsläget. ”Full” eller ”GA”.
* **StorageAccountName**: Name Azure storage-konto för att lagra insamlade data.
* **StorageAccountKey**: Name Azure lagringskontonyckel.
* **AdditionalDataLocationList**: en lista med följande struktur:

  ```powershell
  {
    String Name,
    String Location,
    String SearchPattern,
    Bool   Recursive
  }
  ```

## <a name="adding-as-a-vm-extension"></a>Lägga till som en VM-tillägget
Följ instruktionerna för att ansluta Azure PowerShell till din prenumeration.

1. Ange namnet på tjänsten och Virtuella samlingsläget.

  ```powershell
  #Specify your cloud service name
  $ServiceName = 'YourCloudServiceName'

  #Specify the VM name
  $VMName = "'YourVMName'"

  #Specify the collection mode, "Full" or "GA"
  $mode = "GA"

  Specify the additional data folder for which files will be collected (this step is optional).

  #add one location
  $a1 = New-Object PSObject

  $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
  $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
  $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
  $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"

  $AdditionalDataList+= $a1
        #more locations can be added....
  ```
  
2. Ange det Azure storage-kontonamnet och nyckeln insamlade filer överförs.

  ```powershell
  $StorageAccountName = 'YourStorageAccountName'
  $StorageAccountKey  = 'YourStorageAccountKey'
  ```

3. Anropa SetAzureVMLogCollector.ps1 (ingår i slutet av artikeln) på följande sätt om du vill aktivera tillägget AzureLogCollector för en tjänst i molnet. När körningen har slutförts, kan du hitta den överförda filen under`https://YourStorageAccountName.blob.core.windows.net/vmlogs`

Följande är definitionen av parametrar för skriptet. (Detta kopieras nedan samt.)

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
  [Parameter(Mandatory=$true)]
  [string]   $ServiceName,

  [Parameter(Mandatory=$false)]
  [string] $VMName ,

  [Parameter(Mandatory=$false)]
  [string]   $Mode = 'Full',

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountName,

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountKey,

  [Parameter(Mandatory=$false)]
  [PSObject[]] $AdditionDataLocationList = $null
)
```

* **ServiceName**: din molntjänstnamnet.
* **VMName**: namnet på den virtuella datorn.
* **Läget**: samlingsläget. ”Full” eller ”GA”.
* **StorageAccountName**: Name Azure storage-konto för att lagra insamlade data.
* **StorageAccountKey**: Name Azure lagringskontonyckel.
* **AdditionalDataLocationList**: en lista med följande struktur:

  ```
  {
    String Name,
    String Location,
    String SearchPattern,
    Bool   Recursive
  }
  ```

## <a name="extention-powershell-script-files"></a>Tillägget PowerShell-skript filer
### <a name="setazureservicelogcollectorps1"></a>SetAzureServiceLogCollector.ps1

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
  [Parameter(Mandatory=$true)]
  [string]   $ServiceName,

  [Parameter(Mandatory=$false)]
  [string[]] $Roles ,

  [Parameter(Mandatory=$false)]
  [string[]] $Instances = '*',

  [Parameter(Mandatory=$false)]
  [string]   $Slot = 'Production',

  [Parameter(Mandatory=$false)]
  [string]   $Mode = 'Full',

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountName,

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountKey,

  [Parameter(Mandatory=$false)]
  [PSObject[]] $AdditionDataLocationList = $null
)

$publicConfig = New-Object PSObject

if ($Instances -ne $null -and $Instances.Count -gt 0)  #Instances should be seperated by ,
{
  $instanceText = $Instances[0]
  for ($i = 1;$i -lt $Instances.Count;$i++)
  {
    $instanceText = $instanceText+ "," + $Instances[$i]
  }
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value $instanceText
}
else  #For all instances if not specified.  The value should be a space or *
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value " "
}

if ($Mode -ne $null )
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
}
else
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
}

#
#we need to get the Sasuri from StorageAccount and containers
#
$context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

$ContainerName = "azurelogcollectordata"
$existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
if ($existingContainer -eq $null)
{
  "Container ($ContainerName) doesn't exist. Creating it now.."
  New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
}

$ExpiryTime =  [DateTime]::Now.AddMinutes(120).ToString("o")
$SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
$publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

#
#Add AdditionalData to collect data from additional folders
#
if ($AdditionDataLocationList -ne $null )
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
}

#
# Convert it to JSON format
#
$publicConfigJSON = $publicConfig | ConvertTo-Json
"publicConfig is:  $publicConfigJSON"

#we just provide a empty privateConfig object
$privateconfig = "{
}"

if ($Roles -ne $null)
{
  Set-AzureServiceExtension -Service $ServiceName -Slot $Slot -Role $Roles -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
}
else
{
  Set-AzureServiceExtension -Service $ServiceName -Slot $Slot  -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
}

#
#This is an optional step: generate a sasUri to the container so it can be shared with other people if nened
#
$SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
$SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
$SasUri = $SasUri + "&restype=container&comp=list"
Write-Output "The container for uploaded file can be accessed using this link:`r`n$sasuri"
```

### <a name="setazurevmlogcollectorps1"></a>SetAzureVMLogCollector.ps1

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
              [Parameter(Mandatory=$true)]
              [string]   $ServiceName,

              [Parameter(Mandatory=$false)]
              [string] $VMName ,

              [Parameter(Mandatory=$false)]
              [string]   $Mode = 'Full',

              [Parameter(Mandatory=$false)]
              [string]   $StorageAccountName,

              [Parameter(Mandatory=$false)]
              [string]   $StorageAccountKey,

              [Parameter(Mandatory=$false)]
              [PSObject[]] $AdditionDataLocationList = $null
        )

$publicConfig = New-Object PSObject
$publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value "*"

if ($Mode -ne $null )
{
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
}
else
{
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
}

#
#we need to get the Sasuri from StorageAccount and containers
#
$context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

$ContainerName = "azurelogcollectordata"
$existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
if ($existingContainer -eq $null)
{
    "Container ($ContainerName) doesn't exist. Creating it now.."
    New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
}

$ExpiryTime =  [DateTime]::Now.AddMinutes(90).ToString("o")
$SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
$publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

#
#Add AdditionalData to collect data from additional folders
#
if ($AdditionDataLocationList -ne $null )
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
}

#
# Convert it to JSON format
#
$publicConfigJSON = $publicConfig | ConvertTo-Json

Write-Output "PublicConfigurtion is: \r\n$publicConfigJSON"

#
#we just provide a empty privateConfig object
#
$privateconfig = "{
}"

if ($VMName -ne $null )
{
      $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
      $VM.VM.OSVirtualHardDisk.OS

      if ($VM.VM.OSVirtualHardDisk.OS -like '*Windows*')
      {
            Set-AzureVMExtension -VM $VM -ExtensionName "AzureLogCollector" -Publisher Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.* | Update-AzureVM -Verbose

            #
            #We will check the VM status to find if operation by extension has been completed or not. The completion of the operation,either succeed or fail, can be indicated by
            #the presence of SubstatusList field.
            #
            $Completed = $false
            while ($Completed -ne $true)
            {
                    $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
                    $status = $VM.ResourceExtensionStatusList | Where-Object {$_.HandlerName -eq "Microsoft.WindowsAzure.Compute.AzureLogCollector"}

                    if ( ($status.Code -ne 0) -and ($status.Status -like '*error*'))
                    {
                        Write-Output "Error status is returned: $($Status.ExtensionSettingStatus.FormattedMessage.Message)."
                          $Completed = $true
                    }
                    elseif (($status.ExtensionSettingStatus.SubstatusList -eq $null -or $status.ExtensionSettingStatus.SubstatusList.Count -lt 1))
                    {
                          $Completed = $false
                          Write-Output "Waiting for operation to complete..."
                    }
                    else
                    {
                          $Completed = $true
                          Write-Output "Operation completed."

                    $UploadedFileUri = $Status.ExtensionSettingStatus.SubStatusList[0].FormattedMessage.Message
                          $blob = New-Object Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob($UploadedFileUri)

                  #
                        # This is an optional step:  For easier access to the file, we can generate a read-only SasUri directly to the file
                          #
                          $ExpiryTimeRead =  [DateTime]::Now.AddMinutes(120).ToString("o")
                          $ReadSasUri = New-AzureStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

                        Write-Output "The uploaded file can be accessed using this link: $ReadSasUri"

                          #
                          #This is an optional step:  Remove the extension after we are done
                          #
                          Get-AzureVM -ServiceName $ServiceName -Name $VMName | Set-AzureVMExtension -Publisher Microsoft.WindowsAzure.Compute -ExtensionName "AzureLogCollector" -Version 1.* -Uninstall | Update-AzureVM -Verbose

                    }
                    Start-Sleep -s 5
            }
      }
      else
      {
          Write-Output "VM OS Type is not Windows, the extension cannot be enabled"
      }

}
else
{
  Write-Output "VM name is not specified, the extension cannot be enabled"
}
```

## <a name="next-steps"></a>Nästa steg
Nu kan du undersöka eller kopiera loggar från en enkel plats.

