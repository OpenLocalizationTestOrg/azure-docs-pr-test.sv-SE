


## <a name="using-vm-extensions"></a><span data-ttu-id="9af23-101">Med hjälp av VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="9af23-101">Using VM Extensions</span></span>
<span data-ttu-id="9af23-102">Azure VM-tillägg implementerar beteenden eller funktioner som antingen hjälper andra program som fungerar på virtuella Azure-datorer (till exempel den **WebDeployForVSDevTest** tillägget kan Visual Studio Web Deploy Solutions på Azure VM) eller ange den möjlighet att interagera med den virtuella datorn att stödja vissa andra uppträdande (du kan till exempel använda VM-Access-tillägg från PowerShell, Azure CLI och REST-klienter att återställa eller ändra värden för fjärråtkomst på Azure VM).</span><span class="sxs-lookup"><span data-stu-id="9af23-102">Azure VM Extensions implement behaviors or features that either help other programs work on Azure VMs (for example, the **WebDeployForVSDevTest** extension allows Visual Studio to Web Deploy solutions on your Azure VM) or provide the ability for you to interact with the VM to support some other behavior (for example, you can use the VM Access extensions from PowerShell, the Azure CLI, and REST clients to reset or modify remote access values on your Azure VM).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9af23-103">En fullständig lista över tillägg av funktioner som de stöder finns i [Azure VM-tillägg och funktioner](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9af23-103">For a complete list of extensions by the features they support, see [Azure VM Extensions and Features](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="9af23-104">Eftersom varje VM-tillägget har stöd för en specifik funktion, beroende exakt vad du kan och inte kan göra med ett tillägg av tillägget.</span><span class="sxs-lookup"><span data-stu-id="9af23-104">Because each VM extension supports a specific feature, exactly what you can and cannot do with an extension depends on the extension.</span></span> <span data-ttu-id="9af23-105">Kontrollera därför att du har läs i dokumentationen för VM-tillägget som du vill använda innan du ändrar den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9af23-105">Therefore, before modifying your VM, make sure you have read the documentation for the VM Extension you want to use.</span></span> <span data-ttu-id="9af23-106">Det går inte att ta bort vissa VM-tillägg. andra har egenskaper som kan anges som ändrar VM beteende radikalt.</span><span class="sxs-lookup"><span data-stu-id="9af23-106">Removing some VM Extensions is not supported; others have properties that can be set that change VM behavior radically.</span></span>
> 
> 

<span data-ttu-id="9af23-107">De vanligaste uppgifterna är:</span><span class="sxs-lookup"><span data-stu-id="9af23-107">The most common tasks are:</span></span>

1. <span data-ttu-id="9af23-108">Söka efter tillgängliga tillägg</span><span class="sxs-lookup"><span data-stu-id="9af23-108">Finding Available Extensions</span></span>
2. <span data-ttu-id="9af23-109">Uppdatering av inlästa tillägg</span><span class="sxs-lookup"><span data-stu-id="9af23-109">Updating Loaded Extensions</span></span>
3. <span data-ttu-id="9af23-110">Lägga till tillägg</span><span class="sxs-lookup"><span data-stu-id="9af23-110">Adding Extensions</span></span>
4. <span data-ttu-id="9af23-111">Ta bort tillägg</span><span class="sxs-lookup"><span data-stu-id="9af23-111">Removing Extensions</span></span>

## <a name="find-available-extensions"></a><span data-ttu-id="9af23-112">Hitta tillgängliga tillägg</span><span class="sxs-lookup"><span data-stu-id="9af23-112">Find Available Extensions</span></span>
<span data-ttu-id="9af23-113">Du kan hitta tillägget och utökad information med hjälp av:</span><span class="sxs-lookup"><span data-stu-id="9af23-113">You can locate extension and extended information using:</span></span>

* <span data-ttu-id="9af23-114">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9af23-114">PowerShell</span></span>
* <span data-ttu-id="9af23-115">Kommandoraden för Azure plattformsoberoende gränssnitt (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="9af23-115">Azure Cross-Platform Command Line Interface (Azure CLI)</span></span>
* <span data-ttu-id="9af23-116">REST API för Service Management</span><span class="sxs-lookup"><span data-stu-id="9af23-116">Service Management REST API</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="9af23-117">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9af23-117">Azure PowerShell</span></span>
<span data-ttu-id="9af23-118">Vissa tillägg har PowerShell-cmdlets som är specifika för dem, som kan underlätta konfigurationen från PowerShell; men följande cmdlets fungerar för alla VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="9af23-118">Some extensions have PowerShell cmdlets that are specific to them, which may make their configuration from PowerShell easier; but the following cmdlets work for all VM extensions.</span></span>

<span data-ttu-id="9af23-119">Du kan använda följande cmdletar för att få information om tillgängliga tillägg:</span><span class="sxs-lookup"><span data-stu-id="9af23-119">You can use the following cmdlets to obtain information about available extensions:</span></span>

* <span data-ttu-id="9af23-120">För instanser av webbroller eller arbetsroller som du kan använda den [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9af23-120">For instances of web roles or worker roles, you can use the [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet.</span></span>
* <span data-ttu-id="9af23-121">För instanser av virtuella datorer som du kan använda den [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9af23-121">For instances of Virtual Machines, you can use the [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet.</span></span>
  
   <span data-ttu-id="9af23-122">Till exempel följande kodexempel visar hur du vill visa information för den **IaaSDiagnostics** tillägget med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9af23-122">For example, the following code example shows how to list the information for the **IaaSDiagnostics** extension using PowerShell.</span></span>
  
      PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
  
      Publisher                   : Microsoft.Azure.Diagnostics
      ExtensionName               : IaaSDiagnostics
      Version                     : 1.2
      Label                       : Microsoft Monitoring Agent Diagnostics
      Description                 : Microsoft Monitoring Agent Extension
      PublicConfigurationSchema   :
      PrivateConfigurationSchema  :
      IsInternalExtension         : False
      SampleConfig                :
      ReplicationCompleted        : True
      Eula                        :
      PrivacyUri                  :
      HomepageUri                 :
      IsJsonExtension             : True
      DisallowMajorVersionUpgrade : False
      SupportedOS                 :
      PublishedDate               :
      CompanyName                 :

### <a name="azure-command-line-interface-azure-cli"></a><span data-ttu-id="9af23-123">Azure Command Line Interface (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="9af23-123">Azure Command Line Interface (Azure CLI)</span></span>
<span data-ttu-id="9af23-124">Vissa tillägg har Azure CLI-kommandon som är specifika för dem (Docker VM-tillägget är ett exempel), som kan underlätta konfigurationen; men följande kommandon som fungerar för alla VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="9af23-124">Some extensions have Azure CLI commands that are specific to them (the Docker VM Extension is one example), which may make their configuration easier; but the following commands work for all VM extensions.</span></span>

<span data-ttu-id="9af23-125">Du kan använda den **azure vm tilläggslistan** kommando för att hämta information om tillgängliga tillägg och använda den **–-json** alternativet för att visa all tillgänglig information om en eller flera filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="9af23-125">You can use the **azure vm extension list** command to obtain information about available extensions, and use the **–-json** option to display all available information about one or more extensions.</span></span> <span data-ttu-id="9af23-126">Om du inte använder en Tilläggsnamn returnerar kommandot en JSON-beskrivning av alla tillgängliga tillägg.</span><span class="sxs-lookup"><span data-stu-id="9af23-126">If you do not use an extension name, the command returns a JSON description of all available extensions.</span></span>

<span data-ttu-id="9af23-127">Till exempel följande kodexempel visar hur du vill visa information för den **IaaSDiagnostics** tillägget med hjälp av Azure CLI **azure vm tilläggslistan** kommandot och använder den **–-json**  alternativet för att returnera fullständig information.</span><span class="sxs-lookup"><span data-stu-id="9af23-127">For example, the following code example shows how to list the information for the **IaaSDiagnostics** extension using the Azure CLI **azure vm extension list** command and uses the **–-json** option to return complete information.</span></span>

    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



### <a name="service-management-rest-apis"></a><span data-ttu-id="9af23-128">REST API:er för tjänsthantering</span><span class="sxs-lookup"><span data-stu-id="9af23-128">Service Management REST APIs</span></span>
<span data-ttu-id="9af23-129">Du kan använda följande REST API: er för att få information om tillgängliga tillägg:</span><span class="sxs-lookup"><span data-stu-id="9af23-129">You can use the following REST APIs to obtain information about available extensions:</span></span>

* <span data-ttu-id="9af23-130">För instanser av webbroller eller arbetsroller som du kan använda den [lista över tillgängliga tillägg](https://msdn.microsoft.com/library/dn169559.aspx) igen.</span><span class="sxs-lookup"><span data-stu-id="9af23-130">For instances of web roles or worker roles, you can use the [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) operation.</span></span> <span data-ttu-id="9af23-131">Om du vill visa en lista med versionerna av tillgängliga tillägg, kan du använda [lista tillägget versioner](https://msdn.microsoft.com/library/dn495437.aspx).</span><span class="sxs-lookup"><span data-stu-id="9af23-131">To list the versions of available extensions, you can use [List Extension Versions](https://msdn.microsoft.com/library/dn495437.aspx).</span></span>
* <span data-ttu-id="9af23-132">För instanser av virtuella datorer som du kan använda den [lista Resurstillägg](https://msdn.microsoft.com/library/dn495441.aspx) igen.</span><span class="sxs-lookup"><span data-stu-id="9af23-132">For instances of Virtual Machines, you can use the [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) operation.</span></span> <span data-ttu-id="9af23-133">Om du vill visa en lista med versionerna av tillgängliga tillägg, kan du använda [lista resurs tillägget versioner](https://msdn.microsoft.com/library/dn495440.aspx).</span><span class="sxs-lookup"><span data-stu-id="9af23-133">To list the versions of available extensions, you can use [List Resource Extension Versions](https://msdn.microsoft.com/library/dn495440.aspx).</span></span>

## <a name="add-update-or-disable-extensions"></a><span data-ttu-id="9af23-134">Lägga till, uppdatera, eller inaktivera tillägg</span><span class="sxs-lookup"><span data-stu-id="9af23-134">Add, Update, or Disable Extensions</span></span>
<span data-ttu-id="9af23-135">Tillägg kan läggas till när en instans skapas eller de kan läggas till en instans som körs.</span><span class="sxs-lookup"><span data-stu-id="9af23-135">Extensions can be added when an instance is created or they can be added to a running instance.</span></span> <span data-ttu-id="9af23-136">Tillägg kan uppdateras, inaktiveras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="9af23-136">Extensions can be updated, disabled, or removed.</span></span> <span data-ttu-id="9af23-137">Du kan utföra dessa åtgärder med hjälp av Azure PowerShell-cmdlets eller med hjälp av Service Management REST API-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="9af23-137">You can perform these actions by using Azure PowerShell cmdlets or by using the Service Management REST API operations.</span></span> <span data-ttu-id="9af23-138">Parametrar som krävs för att installera och konfigurera vissa tillägg.</span><span class="sxs-lookup"><span data-stu-id="9af23-138">Parameters are required to install and set up some extensions.</span></span> <span data-ttu-id="9af23-139">Offentliga och privata parametrar stöds för tillägg.</span><span class="sxs-lookup"><span data-stu-id="9af23-139">Public and private parameters are supported for extensions.</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="9af23-140">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9af23-140">Azure PowerShell</span></span>
<span data-ttu-id="9af23-141">Med hjälp av Azure PowerShell-cmdlets är det enklaste sättet att lägga till och uppdatera tillägg.</span><span class="sxs-lookup"><span data-stu-id="9af23-141">Using Azure PowerShell cmdlets is the easiest way to add and update extensions.</span></span> <span data-ttu-id="9af23-142">När du använder cmdlets tillägg görs merparten av konfigurationen av tillägget för dig.</span><span class="sxs-lookup"><span data-stu-id="9af23-142">When you use the extension cmdlets, most of the configuration of the extension is done for you.</span></span> <span data-ttu-id="9af23-143">Ibland kan behöva du lägga till ett tillägg.</span><span class="sxs-lookup"><span data-stu-id="9af23-143">At times, you may need to programmatically add an extension.</span></span> <span data-ttu-id="9af23-144">När du behöver göra detta måste du ange konfigurationen för tillägget.</span><span class="sxs-lookup"><span data-stu-id="9af23-144">When you need to do this, you must provide the configuration of the extension.</span></span>

<span data-ttu-id="9af23-145">Du kan använda följande cmdletar för att veta om ett tillägg kräver en konfiguration av offentliga och privata parametrar:</span><span class="sxs-lookup"><span data-stu-id="9af23-145">You can use the following cmdlets to know whether an extension requires a configuration of public and private parameters:</span></span>

* <span data-ttu-id="9af23-146">För instanser av webbroller eller arbetsroller som du kan använda den **Get-AzureServiceAvailableExtension** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9af23-146">For instances of web roles or worker roles, you can use the **Get-AzureServiceAvailableExtension** cmdlet.</span></span>
* <span data-ttu-id="9af23-147">För instanser av virtuella datorer som du kan använda den **Get-AzureVMAvailableExtension** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9af23-147">For instances of Virtual Machines, you can use the **Get-AzureVMAvailableExtension** cmdlet.</span></span>

### <a name="service-management-rest-apis"></a><span data-ttu-id="9af23-148">REST API:er för tjänsthantering</span><span class="sxs-lookup"><span data-stu-id="9af23-148">Service Management REST APIs</span></span>
<span data-ttu-id="9af23-149">När du hämtar en lista över tillgängliga tillägg med hjälp av REST-API: er, får du information om hur tillägget ska konfigureras.</span><span class="sxs-lookup"><span data-stu-id="9af23-149">When you retrieve a listing of available extensions by using the REST APIs, you receive information about how the extension is to be configured.</span></span> <span data-ttu-id="9af23-150">Den information som returneras kan visa parameterinformation som representeras av ett schema med offentliga och privata schemat.</span><span class="sxs-lookup"><span data-stu-id="9af23-150">The information that is returned might show parameter information represented by a public schema and private schema.</span></span> <span data-ttu-id="9af23-151">Gemensamma parametervärden returneras i frågor om instanserna.</span><span class="sxs-lookup"><span data-stu-id="9af23-151">Public parameter values are returned in queries about the instances.</span></span> <span data-ttu-id="9af23-152">Privata parametervärden returneras inte.</span><span class="sxs-lookup"><span data-stu-id="9af23-152">Private parameter values are not returned.</span></span>

<span data-ttu-id="9af23-153">Du kan använda följande REST API: er för att veta om ett tillägg kräver en konfiguration av offentliga och privata parametrar:</span><span class="sxs-lookup"><span data-stu-id="9af23-153">You can use the following REST APIs to know whether an extension requires a configuration of public and private parameters:</span></span>

* <span data-ttu-id="9af23-154">För instanser av webbroller eller arbetsroller, den **PublicConfigurationSchema** och **PrivateConfigurationSchema** elementen innehåller informationen i svaret från den [lista tillgängliga Tillägg](https://msdn.microsoft.com/library/dn169559.aspx) igen.</span><span class="sxs-lookup"><span data-stu-id="9af23-154">For instances of web roles or worker roles, the **PublicConfigurationSchema** and **PrivateConfigurationSchema** elements contain the information in the response from the [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) operation.</span></span>
* <span data-ttu-id="9af23-155">För instanser av virtuella datorer i **PublicConfigurationSchema** och **PrivateConfigurationSchema** elementen innehåller informationen i svaret från den [lista resurs Tillägg](https://msdn.microsoft.com/library/dn495441.aspx) igen.</span><span class="sxs-lookup"><span data-stu-id="9af23-155">For instances of Virtual Machines, the **PublicConfigurationSchema** and **PrivateConfigurationSchema** elements contain the information in the response from the [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) operation.</span></span>

> [!NOTE]
> <span data-ttu-id="9af23-156">Tillägg kan också använda konfigurationer som har definierats med JSON.</span><span class="sxs-lookup"><span data-stu-id="9af23-156">Extensions can also use configurations that are defined with JSON.</span></span> <span data-ttu-id="9af23-157">När dessa typer av tillägg används endast den **SampleConfig** elementet används.</span><span class="sxs-lookup"><span data-stu-id="9af23-157">When these types of extensions are used, only the **SampleConfig** element is used.</span></span>
> 
> 

