
<span data-ttu-id="0a186-101">Diagnostisera problem med en Microsoft Azure-molntjänst kräver att samla in hello service-loggfilerna på virtuella datorer som hello problem uppstå.</span><span class="sxs-lookup"><span data-stu-id="0a186-101">Diagnosing issues with an Microsoft Azure cloud service requires collecting hello service’s log files on virtual machines as hello issues occur.</span></span> <span data-ttu-id="0a186-102">Du kan använda hello AzureLogCollector tillägg på begäran tooperfom enstaka samlingen loggar från en eller flera moln virtuella datorer för tjänsten (från både webb- och arbetsroller) och överföring hello insamlade filer tooan Azure storage-konto – utan inloggning via fjärranslutning tooany av hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0a186-102">You can use hello AzureLogCollector extension on-demand tooperfom one-time collection of logs from one or more Cloud Service VMs (from both web roles and worker roles) and transfer hello collected files tooan Azure storage account – all without remotely logging on tooany of hello VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="0a186-103">Beskrivningar för de flesta hello loggas information finns på http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.</span><span class="sxs-lookup"><span data-stu-id="0a186-103">Descriptions for most of hello logged information can be found at http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.</span></span>
> 
> 

<span data-ttu-id="0a186-104">Det finns två lägen mängden beroende hello typer av filer toobe samlas in.</span><span class="sxs-lookup"><span data-stu-id="0a186-104">There are two modes of collection dependent on hello types of files toobe collected.</span></span>

* <span data-ttu-id="0a186-105">Azure gäst-agenten loggar endast (GA).</span><span class="sxs-lookup"><span data-stu-id="0a186-105">Azure Guest Agent Logs only (GA).</span></span> <span data-ttu-id="0a186-106">Läget för den här samlingen innehåller alla hello loggar relaterade tooAzure gästagenter och andra Azure-komponenter.</span><span class="sxs-lookup"><span data-stu-id="0a186-106">This collection mode includes all hello logs related tooAzure guest agents and other Azure components.</span></span>
* <span data-ttu-id="0a186-107">Alla loggar (fullständig).</span><span class="sxs-lookup"><span data-stu-id="0a186-107">All Logs (Full).</span></span> <span data-ttu-id="0a186-108">Den här samlingsläget samlar in alla filer i GA läge plus:</span><span class="sxs-lookup"><span data-stu-id="0a186-108">This collection mode will collect all files in GA mode plus:</span></span>
  
  * <span data-ttu-id="0a186-109">händelseloggarna system och program</span><span class="sxs-lookup"><span data-stu-id="0a186-109">system and application event logs</span></span>
  * <span data-ttu-id="0a186-110">Felloggarna för HTTP</span><span class="sxs-lookup"><span data-stu-id="0a186-110">HTTP error logs</span></span>
  * <span data-ttu-id="0a186-111">IIS-loggar</span><span class="sxs-lookup"><span data-stu-id="0a186-111">IIS Logs</span></span>
  * <span data-ttu-id="0a186-112">Installationsloggarna</span><span class="sxs-lookup"><span data-stu-id="0a186-112">Setup logs</span></span>
  * <span data-ttu-id="0a186-113">andra systemloggar</span><span class="sxs-lookup"><span data-stu-id="0a186-113">other system logs</span></span>

<span data-ttu-id="0a186-114">I båda lägena samling kan ytterligare data collection mappar anges med hjälp av en samling hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="0a186-114">In both collection modes, additional data collection folders can be specified by using a collection of hello following structure:</span></span>

* <span data-ttu-id="0a186-115">**Namnet**: hello namnet hello mängden som ska användas som hello namnet på undermappar hello zip-filen toobe samlas in.</span><span class="sxs-lookup"><span data-stu-id="0a186-115">**Name**: hello name of hello collection, which will be used as hello name of subfolder inside hello zip file toobe collected.</span></span>
* <span data-ttu-id="0a186-116">**Plats**: hello toohello sökväg på hello virtuell dator där filen kommer att samlas in.</span><span class="sxs-lookup"><span data-stu-id="0a186-116">**Location**: hello path toohello folder on hello virtual machine where file will be collected.</span></span>
* <span data-ttu-id="0a186-117">**SearchPattern**: hello mönster av hello namnen på filerna toobe samlas in.</span><span class="sxs-lookup"><span data-stu-id="0a186-117">**SearchPattern**: hello pattern of hello names of files toobe collected.</span></span> <span data-ttu-id="0a186-118">Standardvärdet är ”*”</span><span class="sxs-lookup"><span data-stu-id="0a186-118">Default is “*”</span></span>
* <span data-ttu-id="0a186-119">**Rekursiva**: om hello filer ska samlas in rekursivt hello i mappen.</span><span class="sxs-lookup"><span data-stu-id="0a186-119">**Recursive**: if hello files will be collected recursively under hello folder.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a186-120">Krav</span><span class="sxs-lookup"><span data-stu-id="0a186-120">Prerequisites</span></span>
* <span data-ttu-id="0a186-121">Du behöver toohave ett lagringskonto för tillägget toosave genereras zip-filer.</span><span class="sxs-lookup"><span data-stu-id="0a186-121">You need toohave a storage account for extension toosave generated zip files.</span></span>
* <span data-ttu-id="0a186-122">Du måste se till att du använder Azure PowerShell-Cmdlets V0.8.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="0a186-122">You must make sure that you are using Azure PowerShell Cmdlets V0.8.0 or above.</span></span> <span data-ttu-id="0a186-123">Mer information finns i [Azure hämtar](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="0a186-123">For more information, see [Azure Downloads](https://azure.microsoft.com/downloads/).</span></span>

## <a name="add-hello-extension"></a><span data-ttu-id="0a186-124">Lägga till hello-tillägget</span><span class="sxs-lookup"><span data-stu-id="0a186-124">Add hello extension</span></span>
<span data-ttu-id="0a186-125">Du kan använda [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) cmdlets eller [Service Management REST API: er](https://msdn.microsoft.com/library/ee460799.aspx) tooadd hello AzureLogCollector tillägg.</span><span class="sxs-lookup"><span data-stu-id="0a186-125">You can use [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) cmdlets or [Service Management REST APIs](https://msdn.microsoft.com/library/ee460799.aspx) tooadd hello AzureLogCollector extension.</span></span>

<span data-ttu-id="0a186-126">Hej befintliga Azure Powershell-cmdleten för molntjänster, **Set AzureServiceExtension**, kan vara används tooenable hello-tillägget för rollinstanser för Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="0a186-126">For Cloud Services, hello existing Azure Powershell cmdlet, **Set-AzureServiceExtension**, can be used tooenable hello extension on Cloud Service role instances.</span></span> <span data-ttu-id="0a186-127">Varje gång det här tillägget aktiveras via denna cmdlet, utlöses Logginsamling på hello valt rollinstanser av valda roller.</span><span class="sxs-lookup"><span data-stu-id="0a186-127">Every time this extension is enabled through this cmdlet, log collection is triggered on hello selected role instances of selected roles.</span></span>

<span data-ttu-id="0a186-128">Hej befintliga Azure Powershell-cmdleten för virtuella datorer, **Set AzureVMExtension**, kan vara används tooenable hello tillägg på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0a186-128">For Virtual Machines, hello existing Azure Powershell cmdlet, **Set-AzureVMExtension**, can be used tooenable hello extension on Virtual Machines.</span></span> <span data-ttu-id="0a186-129">Varje gång det här tillägget aktiveras via hello-cmdletar, utlöses Logginsamling på varje-instans.</span><span class="sxs-lookup"><span data-stu-id="0a186-129">Every time this extension is enabled through hello cmdlets, log collection is triggered on each instance.</span></span>

<span data-ttu-id="0a186-130">Det här tillägget används internt, hello JSON-baserade PublicConfiguration och PrivateConfiguration.</span><span class="sxs-lookup"><span data-stu-id="0a186-130">Internally, this extension uses hello JSON-based PublicConfiguration and PrivateConfiguration.</span></span> <span data-ttu-id="0a186-131">hello följer hello layout för ett exempel JSON för offentliga och privata konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0a186-131">hello following is hello layout of a sample JSON for public and private configuration.</span></span>

### <a name="publicconfiguration"></a><span data-ttu-id="0a186-132">PublicConfiguration</span><span class="sxs-lookup"><span data-stu-id="0a186-132">PublicConfiguration</span></span>
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

### <a name="privateconfiguration"></a><span data-ttu-id="0a186-133">PrivateConfiguration</span><span class="sxs-lookup"><span data-stu-id="0a186-133">PrivateConfiguration</span></span>
    {

    }

> [!NOTE]
> <span data-ttu-id="0a186-134">Det här tillägget behöver inte **privateConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="0a186-134">This extension doesn’t need **privateConfiguration**.</span></span> <span data-ttu-id="0a186-135">Du kan bara ange en tom struktur för hello **– PrivateConfiguration** argumentet.</span><span class="sxs-lookup"><span data-stu-id="0a186-135">You can just provide an empty structure for hello **–PrivateConfiguration** argument.</span></span>
> 
> 

<span data-ttu-id="0a186-136">Du kan följa en hello två följande steg tooadd hello AzureLogCollector tooone eller flera instanser av en molnbaserad tjänst eller en virtuell dator för valda roller, vilka utlösare hello samlingar på varje VM-toorun och skicka hello insamlade filer tooAzure konto Ange.</span><span class="sxs-lookup"><span data-stu-id="0a186-136">You can follow one of hello two following steps tooadd hello AzureLogCollector tooone or more instances of a Cloud Service or Virtual Machine of selected roles, which triggers hello collections on each VM toorun and send hello collected files tooAzure account specified.</span></span>

## <a name="adding-as-a-service-extension"></a><span data-ttu-id="0a186-137">Lägga till som ett tillägg för tjänsten</span><span class="sxs-lookup"><span data-stu-id="0a186-137">Adding as a Service Extension</span></span>
1. <span data-ttu-id="0a186-138">Följ hello instruktioner tooconnect Azure PowerShell tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0a186-138">Follow hello instructions tooconnect Azure PowerShell tooyour subscription.</span></span>
2. <span data-ttu-id="0a186-139">Ange hello tjänstens namn, plats, roller och rollen instanser toowhich du vill använda tooadd och aktivera hello AzureLogCollector tillägg.</span><span class="sxs-lookup"><span data-stu-id="0a186-139">Specify hello service name, slot, roles, and role instances toowhich you want tooadd and enable hello AzureLogCollector extension.</span></span>
   
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
3. <span data-ttu-id="0a186-140">Ange mappen för hello ytterligare data som samlas in filer (det här steget är valfritt).</span><span class="sxs-lookup"><span data-stu-id="0a186-140">Specify hello additional data folder for which files will be collected (this step is optional).</span></span>
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
   
   > [!NOTE]
   > <span data-ttu-id="0a186-141">Du kan använda token `%roleroot%` toospecify hello rollen rotenhet eftersom det inte använder en fast enhet.</span><span class="sxs-lookup"><span data-stu-id="0a186-141">You can use token `%roleroot%` toospecify hello role root drive since it doesn’t use a fixed drive.</span></span>
   > 
   > 
4. <span data-ttu-id="0a186-142">Ange hello Azure storage-kontonamnet och nyckeln toowhich insamlade filer överförs.</span><span class="sxs-lookup"><span data-stu-id="0a186-142">Provide hello Azure storage account name and key toowhich collected files will be uploaded.</span></span>
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
5. <span data-ttu-id="0a186-143">Anropa hello SetAzureServiceLogCollector.ps1 (ingår i hello slutet av artikeln hello) som följer tooenable hello AzureLogCollector tillägg för en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="0a186-143">Call hello SetAzureServiceLogCollector.ps1 (included at hello end of hello article) as follows tooenable hello AzureLogCollector extension for a Cloud Service.</span></span> <span data-ttu-id="0a186-144">När hello körningen har slutförts hittar hello överföra filen under`https://YouareStorageAccountName.blob.core.windows.net/vmlogs`</span><span class="sxs-lookup"><span data-stu-id="0a186-144">Once hello execution is completed, you can find hello uploaded file under `https://YouareStorageAccountName.blob.core.windows.net/vmlogs`</span></span>
   
        .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList

<span data-ttu-id="0a186-145">hello följande är hello definition av hello parametrar toohello skript.</span><span class="sxs-lookup"><span data-stu-id="0a186-145">hello following is hello definition of hello parameters passed toohello script.</span></span> <span data-ttu-id="0a186-146">(Detta kopieras nedan samt.)</span><span class="sxs-lookup"><span data-stu-id="0a186-146">(This is copied below as well.)</span></span>

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

* <span data-ttu-id="0a186-147">*ServiceName*: din molntjänstnamnet.</span><span class="sxs-lookup"><span data-stu-id="0a186-147">*ServiceName*: Your cloud service name.</span></span>
* <span data-ttu-id="0a186-148">*Roller*: en lista över roller, till exempel ”WebRole1” eller ”WorkerRole1”.</span><span class="sxs-lookup"><span data-stu-id="0a186-148">*Roles*: A list of roles, such as “WebRole1” or ”WorkerRole1”.</span></span>
* <span data-ttu-id="0a186-149">*Instanser*: en lista över hello namnen på rollinstanser avgränsade med kommatecken--Använd hello jokerteckensträng (”*”) för alla rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="0a186-149">*Instances*: A list of hello names of role instances separated by comma -- use hello wildcard string (“*”) for all role instances.</span></span>
* <span data-ttu-id="0a186-150">*Fack*: platsnamnet.</span><span class="sxs-lookup"><span data-stu-id="0a186-150">*Slot*: Slot name.</span></span> <span data-ttu-id="0a186-151">”Produktion” eller ”mellanlagring”.</span><span class="sxs-lookup"><span data-stu-id="0a186-151">“Production” or “Staging”.</span></span>
* <span data-ttu-id="0a186-152">*Läget*: samlingsläget.</span><span class="sxs-lookup"><span data-stu-id="0a186-152">*Mode*: Collection mode.</span></span> <span data-ttu-id="0a186-153">”Full” eller ”GA”.</span><span class="sxs-lookup"><span data-stu-id="0a186-153">“Full” or “GA”.</span></span>
* <span data-ttu-id="0a186-154">*StorageAccountName*: Name Azure storage-konto för att lagra insamlade data.</span><span class="sxs-lookup"><span data-stu-id="0a186-154">*StorageAccountName*: Name of Azure storage account for storing collected data.</span></span>
* <span data-ttu-id="0a186-155">*StorageAccountKey*: Name Azure lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="0a186-155">*StorageAccountKey*: Name of Azure storage account key.</span></span>
* <span data-ttu-id="0a186-156">*AdditionalDataLocationList*: en lista över hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="0a186-156">*AdditionalDataLocationList*: A list of hello following structure:</span></span>
  
      {
      String Name,
      String Location,
      String SearchPattern,
      Bool   Recursive
      }

## <a name="adding-as-a-vm-extension"></a><span data-ttu-id="0a186-157">Lägga till som en VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="0a186-157">Adding as a VM Extension</span></span>
<span data-ttu-id="0a186-158">Följ hello instruktioner tooconnect Azure PowerShell tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0a186-158">Follow hello instructions tooconnect Azure PowerShell tooyour subscription.</span></span>

1. <span data-ttu-id="0a186-159">Ange hello tjänstnamn och VM hello samlingsläget.</span><span class="sxs-lookup"><span data-stu-id="0a186-159">Specify hello service name, VM, and hello collection mode.</span></span>
   
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
2. <span data-ttu-id="0a186-160">Ange hello Azure storage-kontonamnet och nyckeln toowhich insamlade filer överförs.</span><span class="sxs-lookup"><span data-stu-id="0a186-160">Provide hello Azure storage account name and key toowhich collected files will be uploaded.</span></span>
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
3. <span data-ttu-id="0a186-161">Anropa hello SetAzureVMLogCollector.ps1 (ingår i hello slutet av artikeln hello) som följer tooenable hello AzureLogCollector tillägg för en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="0a186-161">Call hello SetAzureVMLogCollector.ps1 (included at hello end of hello article) as follows tooenable hello AzureLogCollector extension for a Cloud Service.</span></span> <span data-ttu-id="0a186-162">När hello körningen har slutförts hittar hello överföra filen under https://YouareStorageAccountName.blob.core.windows.net/vmlogs</span><span class="sxs-lookup"><span data-stu-id="0a186-162">Once hello execution is completed, you can find hello uploaded file under https://YouareStorageAccountName.blob.core.windows.net/vmlogs</span></span>

<span data-ttu-id="0a186-163">hello följande är hello definition av hello parametrar toohello skript.</span><span class="sxs-lookup"><span data-stu-id="0a186-163">hello following is hello definition of hello parameters passed toohello script.</span></span> <span data-ttu-id="0a186-164">(Detta kopieras nedan samt.)</span><span class="sxs-lookup"><span data-stu-id="0a186-164">(This is copied below as well.)</span></span>

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

* <span data-ttu-id="0a186-165">Namn på tjänst: Din molntjänstnamnet.</span><span class="sxs-lookup"><span data-stu-id="0a186-165">ServiceName: Your cloud service name.</span></span>
* <span data-ttu-id="0a186-166">VMName hello namnet på hello VM.</span><span class="sxs-lookup"><span data-stu-id="0a186-166">VMName hello name of hello VM.</span></span>
* <span data-ttu-id="0a186-167">Läge: Läget samlingen.</span><span class="sxs-lookup"><span data-stu-id="0a186-167">Mode: Collection mode.</span></span> <span data-ttu-id="0a186-168">”Full” eller ”GA”.</span><span class="sxs-lookup"><span data-stu-id="0a186-168">“Full” or “GA”.</span></span>
* <span data-ttu-id="0a186-169">StorageAccountName: Namnet på Azure storage-konto för att lagra data som samlats in.</span><span class="sxs-lookup"><span data-stu-id="0a186-169">StorageAccountName: Name of Azure storage account for storing collected data.</span></span>
* <span data-ttu-id="0a186-170">StorageAccountKey: Namnet på nyckeln för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0a186-170">StorageAccountKey: Name of Azure storage account key.</span></span>
* <span data-ttu-id="0a186-171">AdditionalDataLocationList: En lista över hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="0a186-171">AdditionalDataLocationList: A list of hello following structure:</span></span>

```
      {
        String Name,
        String Location,
        String SearchPattern,
        Bool   Recursive
      }
```

## <a name="extention-powershell-script-files"></a><span data-ttu-id="0a186-172">Tillägget PowerShell-skript filer</span><span class="sxs-lookup"><span data-stu-id="0a186-172">Extention PowerShell Script files</span></span>
<span data-ttu-id="0a186-173">SetAzureServiceLogCollector.ps1</span><span class="sxs-lookup"><span data-stu-id="0a186-173">SetAzureServiceLogCollector.ps1</span></span>

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


<span data-ttu-id="0a186-174">SetAzureVMLogCollector.ps1</span><span class="sxs-lookup"><span data-stu-id="0a186-174">SetAzureVMLogCollector.ps1</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="0a186-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0a186-175">Next Steps</span></span>
<span data-ttu-id="0a186-176">Nu kan du undersöka eller kopiera loggar från en väldigt enkel plats.</span><span class="sxs-lookup"><span data-stu-id="0a186-176">Now you can examine or copy your logs from one very simple location.</span></span>

