
Diagnostisera problem med en Microsoft Azure-molntjänst kräver att samla in hello service-loggfilerna på virtuella datorer som hello problem uppstå. Du kan använda hello AzureLogCollector tillägg på begäran tooperfom enstaka samlingen loggar från en eller flera moln virtuella datorer för tjänsten (från både webb- och arbetsroller) och överföring hello insamlade filer tooan Azure storage-konto – utan inloggning via fjärranslutning tooany av hello virtuella datorer.

> [!NOTE]
> Beskrivningar för de flesta hello loggas information finns på http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.
> 
> 

Det finns två lägen mängden beroende hello typer av filer toobe samlas in.

* Azure gäst-agenten loggar endast (GA). Läget för den här samlingen innehåller alla hello loggar relaterade tooAzure gästagenter och andra Azure-komponenter.
* Alla loggar (fullständig). Den här samlingsläget samlar in alla filer i GA läge plus:
  
  * händelseloggarna system och program
  * Felloggarna för HTTP
  * IIS-loggar
  * Installationsloggarna
  * andra systemloggar

I båda lägena samling kan ytterligare data collection mappar anges med hjälp av en samling hello följande struktur:

* **Namnet**: hello namnet hello mängden som ska användas som hello namnet på undermappar hello zip-filen toobe samlas in.
* **Plats**: hello toohello sökväg på hello virtuell dator där filen kommer att samlas in.
* **SearchPattern**: hello mönster av hello namnen på filerna toobe samlas in. Standardvärdet är ”*”
* **Rekursiva**: om hello filer ska samlas in rekursivt hello i mappen.

## <a name="prerequisites"></a>Krav
* Du behöver toohave ett lagringskonto för tillägget toosave genereras zip-filer.
* Du måste se till att du använder Azure PowerShell-Cmdlets V0.8.0 eller senare. Mer information finns i [Azure hämtar](https://azure.microsoft.com/downloads/).

## <a name="add-hello-extension"></a>Lägga till hello-tillägget
Du kan använda [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) cmdlets eller [Service Management REST API: er](https://msdn.microsoft.com/library/ee460799.aspx) tooadd hello AzureLogCollector tillägg.

Hej befintliga Azure Powershell-cmdleten för molntjänster, **Set AzureServiceExtension**, kan vara används tooenable hello-tillägget för rollinstanser för Molntjänsten. Varje gång det här tillägget aktiveras via denna cmdlet, utlöses Logginsamling på hello valt rollinstanser av valda roller.

Hej befintliga Azure Powershell-cmdleten för virtuella datorer, **Set AzureVMExtension**, kan vara används tooenable hello tillägg på virtuella datorer. Varje gång det här tillägget aktiveras via hello-cmdletar, utlöses Logginsamling på varje-instans.

Det här tillägget används internt, hello JSON-baserade PublicConfiguration och PrivateConfiguration. hello följer hello layout för ett exempel JSON för offentliga och privata konfiguration.

### <a name="publicconfiguration"></a>PublicConfiguration
    {
        "Instances":  "*",
        "Mode":  "Full",
        "SasUri":  "SasUri tooyour storage account with sp=wl",
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

### <a name="privateconfiguration"></a>PrivateConfiguration
    {

    }

> [!NOTE]
> Det här tillägget behöver inte **privateConfiguration**. Du kan bara ange en tom struktur för hello **– PrivateConfiguration** argumentet.
> 
> 

Du kan följa en hello två följande steg tooadd hello AzureLogCollector tooone eller flera instanser av en molnbaserad tjänst eller en virtuell dator för valda roller, vilka utlösare hello samlingar på varje VM-toorun och skicka hello insamlade filer tooAzure konto Ange.

## <a name="adding-as-a-service-extension"></a>Lägga till som ett tillägg för tjänsten
1. Följ hello instruktioner tooconnect Azure PowerShell tooyour prenumeration.
2. Ange hello tjänstens namn, plats, roller och rollen instanser toowhich du vill använda tooadd och aktivera hello AzureLogCollector tillägg.
   
        #Specify your cloud service name
        $ServiceName = 'extensiontest2'
   
        #Specify hello slot. 'Production' or 'Staging'
        $slot = 'Production'
   
        #Specified hello roles on which hello extension will be installed and enabled
        $roles = @("WorkerRole1","WebRole1")
   
        #Specify hello instances on which extension will be installed and enabled.  Use wildcard * for all instances
        $instances = @("*")
   
        #Specify hello collection mode, "Full" or "GA"
        $mode = "GA"
3. Ange mappen för hello ytterligare data som samlas in filer (det här steget är valfritt).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
   
   > [!NOTE]
   > Du kan använda token `%roleroot%` toospecify hello rollen rotenhet eftersom det inte använder en fast enhet.
   > 
   > 
4. Ange hello Azure storage-kontonamnet och nyckeln toowhich insamlade filer överförs.
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
5. Anropa hello SetAzureServiceLogCollector.ps1 (ingår i hello slutet av artikeln hello) som följer tooenable hello AzureLogCollector tillägg för en tjänst i molnet. När hello körningen har slutförts hittar hello överföra filen under`https://YouareStorageAccountName.blob.core.windows.net/vmlogs`
   
        .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList

hello följande är hello definition av hello parametrar toohello skript. (Detta kopieras nedan samt.)

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

* *ServiceName*: din molntjänstnamnet.
* *Roller*: en lista över roller, till exempel ”WebRole1” eller ”WorkerRole1”.
* *Instanser*: en lista över hello namnen på rollinstanser avgränsade med kommatecken--Använd hello jokerteckensträng (”*”) för alla rollinstanser.
* *Fack*: platsnamnet. ”Produktion” eller ”mellanlagring”.
* *Läget*: samlingsläget. ”Full” eller ”GA”.
* *StorageAccountName*: Name Azure storage-konto för att lagra insamlade data.
* *StorageAccountKey*: Name Azure lagringskontonyckel.
* *AdditionalDataLocationList*: en lista över hello följande struktur:
  
      {
      String Name,
      String Location,
      String SearchPattern,
      Bool   Recursive
      }

## <a name="adding-as-a-vm-extension"></a>Lägga till som en VM-tillägget
Följ hello instruktioner tooconnect Azure PowerShell tooyour prenumeration.

1. Ange hello tjänstnamn och VM hello samlingsläget.
   
        #Specify your cloud service name
        $ServiceName = 'YourCloudServiceName'
   
        #Specify hello VM name
        $VMName = "'YourVMName'"
   
        #Specify hello collection mode, "Full" or "GA"
        $mode = "GA"
   
        Specify hello additional data folder for which files will be collected (this step is optional).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
2. Ange hello Azure storage-kontonamnet och nyckeln toowhich insamlade filer överförs.
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
3. Anropa hello SetAzureVMLogCollector.ps1 (ingår i hello slutet av artikeln hello) som följer tooenable hello AzureLogCollector tillägg för en tjänst i molnet. När hello körningen har slutförts hittar hello överföra filen under https://YouareStorageAccountName.blob.core.windows.net/vmlogs

hello följande är hello definition av hello parametrar toohello skript. (Detta kopieras nedan samt.)

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

* Namn på tjänst: Din molntjänstnamnet.
* VMName hello namnet på hello VM.
* Läge: Läget samlingen. ”Full” eller ”GA”.
* StorageAccountName: Namnet på Azure storage-konto för att lagra data som samlats in.
* StorageAccountKey: Namnet på nyckeln för Azure storage-konto.
* AdditionalDataLocationList: En lista över hello följande struktur:

```
      {
        String Name,
        String Location,
        String SearchPattern,
        Bool   Recursive
      }
```

## <a name="extention-powershell-script-files"></a>Tillägget PowerShell-skript filer
SetAzureServiceLogCollector.ps1

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
    else  #For all instances if not specified.  hello value should be a space or *
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
    #we need tooget hello Sasuri from StorageAccount and containers
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
    #Add AdditionalData toocollect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it tooJSON format
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
    #This is an optional step: generate a sasUri toohello container so it can be shared with other people if nened
    #
    $SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
    $SasUri = $SasUri + "&restype=container&comp=list"
    Write-Output "hello container for uploaded file can be accessed using this link:`r`n$sasuri"


SetAzureVMLogCollector.ps1

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
    #we need tooget hello Sasuri from StorageAccount and containers
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
    #Add AdditionalData toocollect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it tooJSON format
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
                #We will check hello VM status toofind if operation by extension has been completed or not. hello completion of hello operation,either succeed or fail, can be indicated by
                #hello presence of SubstatusList field.
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
                              Write-Output "Waiting for operation toocomplete..."
                        }
                        else
                        {
                              $Completed = $true
                              Write-Output "Operation completed."

                        $UploadedFileUri = $Status.ExtensionSettingStatus.SubStatusList[0].FormattedMessage.Message
                              $blob = New-Object Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob($UploadedFileUri)

                      #
                            # This is an optional step:  For easier access toohello file, we can generate a read-only SasUri directly toohello file
                              #
                              $ExpiryTimeRead =  [DateTime]::Now.AddMinutes(120).ToString("o")
                              $ReadSasUri = New-AzureStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

                            Write-Output "hello uploaded file can be accessed using this link: $ReadSasUri"

                              #
                              #This is an optional step:  Remove hello extension after we are done
                              #
                              Get-AzureVM -ServiceName $ServiceName -Name $VMName | Set-AzureVMExtension -Publisher Microsoft.WindowsAzure.Compute -ExtensionName "AzureLogCollector" -Version 1.* -Uninstall | Update-AzureVM -Verbose

                        }
                        Start-Sleep -s 5
                }
          }
          else
          {
              Write-Output "VM OS Type is not Windows, hello extension cannot be enabled"
          }

    }
    else
    {
      Write-Output "VM name is not specified, hello extension cannot be enabled"
    }

## <a name="next-steps"></a>Nästa steg
Nu kan du undersöka eller kopiera loggar från en väldigt enkel plats.

