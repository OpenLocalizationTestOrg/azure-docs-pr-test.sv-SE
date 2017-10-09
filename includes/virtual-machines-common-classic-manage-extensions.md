


## <a name="using-vm-extensions"></a><span data-ttu-id="8e6a3-101">Med hjälp av VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="8e6a3-101">Using VM Extensions</span></span>
<span data-ttu-id="8e6a3-102">Azure VM-tillägg implementerar beteenden eller funktioner som antingen hjälper andra program som fungerar på virtuella Azure-datorer (till exempel hello **WebDeployForVSDevTest** tillägget kan Visual Studio tooWeb distribuera lösningar på Azure VM) eller ange Hej möjligheten för toointeract med hello VM toosupport vissa andra beteende (t.ex, du kan använda hello VM tillägg från PowerShell, hello Azure CLI och RESTEN klienter tooreset eller ändra värden för fjärråtkomst på Azure VM).</span><span class="sxs-lookup"><span data-stu-id="8e6a3-102">Azure VM Extensions implement behaviors or features that either help other programs work on Azure VMs (for example, hello **WebDeployForVSDevTest** extension allows Visual Studio tooWeb Deploy solutions on your Azure VM) or provide hello ability for you toointeract with hello VM toosupport some other behavior (for example, you can use hello VM Access extensions from PowerShell, hello Azure CLI, and REST clients tooreset or modify remote access values on your Azure VM).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8e6a3-103">En fullständig lista över tillägg av hello funktioner de stöder finns i [Azure VM-tillägg och funktioner](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8e6a3-103">For a complete list of extensions by hello features they support, see [Azure VM Extensions and Features](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="8e6a3-104">Eftersom varje VM-tillägget har stöd för en specifik funktion, beroende exakt vad du kan och inte kan göra med ett tillägg hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-104">Because each VM extension supports a specific feature, exactly what you can and cannot do with an extension depends on hello extension.</span></span> <span data-ttu-id="8e6a3-105">Kontrollera därför att du har läst hello dokumentationen för hello VM-tillägget som du vill toouse innan du ändrar den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-105">Therefore, before modifying your VM, make sure you have read hello documentation for hello VM Extension you want toouse.</span></span> <span data-ttu-id="8e6a3-106">Det går inte att ta bort vissa VM-tillägg. andra har egenskaper som kan anges som ändrar VM beteende radikalt.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-106">Removing some VM Extensions is not supported; others have properties that can be set that change VM behavior radically.</span></span>
> 
> 

<span data-ttu-id="8e6a3-107">hello vanligaste uppgifterna är:</span><span class="sxs-lookup"><span data-stu-id="8e6a3-107">hello most common tasks are:</span></span>

1. <span data-ttu-id="8e6a3-108">Söka efter tillgängliga tillägg</span><span class="sxs-lookup"><span data-stu-id="8e6a3-108">Finding Available Extensions</span></span>
2. <span data-ttu-id="8e6a3-109">Uppdatering av inlästa tillägg</span><span class="sxs-lookup"><span data-stu-id="8e6a3-109">Updating Loaded Extensions</span></span>
3. <span data-ttu-id="8e6a3-110">Lägga till tillägg</span><span class="sxs-lookup"><span data-stu-id="8e6a3-110">Adding Extensions</span></span>
4. <span data-ttu-id="8e6a3-111">Ta bort tillägg</span><span class="sxs-lookup"><span data-stu-id="8e6a3-111">Removing Extensions</span></span>

## <a name="find-available-extensions"></a><span data-ttu-id="8e6a3-112">Hitta tillgängliga tillägg</span><span class="sxs-lookup"><span data-stu-id="8e6a3-112">Find Available Extensions</span></span>
<span data-ttu-id="8e6a3-113">Du kan hitta tillägget och utökad information med hjälp av:</span><span class="sxs-lookup"><span data-stu-id="8e6a3-113">You can locate extension and extended information using:</span></span>

* <span data-ttu-id="8e6a3-114">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8e6a3-114">PowerShell</span></span>
* <span data-ttu-id="8e6a3-115">Kommandoraden för Azure plattformsoberoende gränssnitt (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="8e6a3-115">Azure Cross-Platform Command Line Interface (Azure CLI)</span></span>
* <span data-ttu-id="8e6a3-116">REST API för Service Management</span><span class="sxs-lookup"><span data-stu-id="8e6a3-116">Service Management REST API</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="8e6a3-117">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8e6a3-117">Azure PowerShell</span></span>
<span data-ttu-id="8e6a3-118">Vissa tillägg har PowerShell-cmdlets som är specifika toothem som kan underlätta konfigurationen från PowerShell; men hello följande cmdlets fungerar för alla VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-118">Some extensions have PowerShell cmdlets that are specific toothem, which may make their configuration from PowerShell easier; but hello following cmdlets work for all VM extensions.</span></span>

<span data-ttu-id="8e6a3-119">Du kan använda följande cmdlet: ar tooobtain information om tillgängliga tillägg hello:</span><span class="sxs-lookup"><span data-stu-id="8e6a3-119">You can use hello following cmdlets tooobtain information about available extensions:</span></span>

* <span data-ttu-id="8e6a3-120">För instanser av webbroller eller arbetsroller kan du använda hello [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-120">For instances of web roles or worker roles, you can use hello [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) cmdlet.</span></span>
* <span data-ttu-id="8e6a3-121">För instanser av virtuella datorer, kan du använda hello [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-121">For instances of Virtual Machines, you can use hello [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) cmdlet.</span></span>
  
   <span data-ttu-id="8e6a3-122">Till exempel hello följande exempel visas hur toolist information för hello **IaaSDiagnostics** tillägget med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-122">For example, hello following code example shows how toolist the information for hello **IaaSDiagnostics** extension using PowerShell.</span></span>
  
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

### <a name="azure-command-line-interface-azure-cli"></a><span data-ttu-id="8e6a3-123">Azure Command Line Interface (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="8e6a3-123">Azure Command Line Interface (Azure CLI)</span></span>
<span data-ttu-id="8e6a3-124">Vissa tillägg har Azure CLI-kommandon som är specifika toothem (hello Docker VM-tillägget är ett exempel), som kan underlätta konfigurationen; men hello följande kommandon för alla VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-124">Some extensions have Azure CLI commands that are specific toothem (hello Docker VM Extension is one example), which may make their configuration easier; but hello following commands work for all VM extensions.</span></span>

<span data-ttu-id="8e6a3-125">Du kan använda hello **azure vm tilläggslistan** kommandot tooobtain information om tillgängliga tillägg och använda hello **–-json** alternativet toodisplay all tillgänglig information om en eller flera filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-125">You can use hello **azure vm extension list** command tooobtain information about available extensions, and use hello **–-json** option toodisplay all available information about one or more extensions.</span></span> <span data-ttu-id="8e6a3-126">Om du inte använder en Tilläggsnamn returnerar hello kommando en JSON-beskrivning av alla tillgängliga tillägg.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-126">If you do not use an extension name, hello command returns a JSON description of all available extensions.</span></span>

<span data-ttu-id="8e6a3-127">Till exempel hello följande kodexempel visar hur toolist hello information för hello **IaaSDiagnostics** tillägget med hjälp av hello Azure CLI **azure vm tilläggslistan** kommandot och använder hello **–-json** alternativet tooreturn fullständig information.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-127">For example, hello following code example shows how toolist hello information for hello **IaaSDiagnostics** extension using hello Azure CLI **azure vm extension list** command and uses hello **–-json** option tooreturn complete information.</span></span>

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



### <a name="service-management-rest-apis"></a><span data-ttu-id="8e6a3-128">REST API:er för tjänsthantering</span><span class="sxs-lookup"><span data-stu-id="8e6a3-128">Service Management REST APIs</span></span>
<span data-ttu-id="8e6a3-129">Du kan använda följande REST API: er tooobtain information om tillgängliga tillägg hello:</span><span class="sxs-lookup"><span data-stu-id="8e6a3-129">You can use hello following REST APIs tooobtain information about available extensions:</span></span>

* <span data-ttu-id="8e6a3-130">För instanser av webbroller eller arbetsroller kan du använda hello [lista över tillgängliga tillägg](https://msdn.microsoft.com/library/dn169559.aspx) igen.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-130">For instances of web roles or worker roles, you can use hello [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) operation.</span></span> <span data-ttu-id="8e6a3-131">toolist hello versioner av tillgängliga tillägg kan du använda [lista tillägget versioner](https://msdn.microsoft.com/library/dn495437.aspx).</span><span class="sxs-lookup"><span data-stu-id="8e6a3-131">toolist hello versions of available extensions, you can use [List Extension Versions](https://msdn.microsoft.com/library/dn495437.aspx).</span></span>
* <span data-ttu-id="8e6a3-132">För instanser av virtuella datorer, kan du använda hello [lista Resurstillägg](https://msdn.microsoft.com/library/dn495441.aspx) igen.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-132">For instances of Virtual Machines, you can use hello [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) operation.</span></span> <span data-ttu-id="8e6a3-133">toolist hello versioner av tillgängliga tillägg kan du använda [lista resurs tillägget versioner](https://msdn.microsoft.com/library/dn495440.aspx).</span><span class="sxs-lookup"><span data-stu-id="8e6a3-133">toolist hello versions of available extensions, you can use [List Resource Extension Versions](https://msdn.microsoft.com/library/dn495440.aspx).</span></span>

## <a name="add-update-or-disable-extensions"></a><span data-ttu-id="8e6a3-134">Lägga till, uppdatera, eller inaktivera tillägg</span><span class="sxs-lookup"><span data-stu-id="8e6a3-134">Add, Update, or Disable Extensions</span></span>
<span data-ttu-id="8e6a3-135">Tillägg kan läggas till när en instans skapas eller de kan läggas till tooa kör-instansen.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-135">Extensions can be added when an instance is created or they can be added tooa running instance.</span></span> <span data-ttu-id="8e6a3-136">Tillägg kan uppdateras, inaktiveras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-136">Extensions can be updated, disabled, or removed.</span></span> <span data-ttu-id="8e6a3-137">Du kan utföra dessa åtgärder med hjälp av Azure PowerShell-cmdlets eller genom att använda hello Service Management REST API-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-137">You can perform these actions by using Azure PowerShell cmdlets or by using hello Service Management REST API operations.</span></span> <span data-ttu-id="8e6a3-138">Parametrar krävs tooinstall och konfigurera vissa tillägg.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-138">Parameters are required tooinstall and set up some extensions.</span></span> <span data-ttu-id="8e6a3-139">Offentliga och privata parametrar stöds för tillägg.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-139">Public and private parameters are supported for extensions.</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="8e6a3-140">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8e6a3-140">Azure PowerShell</span></span>
<span data-ttu-id="8e6a3-141">Använda Azure PowerShell-cmdlets är hello enklaste sättet tooadd och uppdatera tillägg.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-141">Using Azure PowerShell cmdlets is hello easiest way tooadd and update extensions.</span></span> <span data-ttu-id="8e6a3-142">När du använder hello tillägget cmdlets görs största delen av hello konfiguration hello-tillägget för dig.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-142">When you use hello extension cmdlets, most of hello configuration of hello extension is done for you.</span></span> <span data-ttu-id="8e6a3-143">Ibland kan du behöva tooprogrammatically lägga till ett tillägg.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-143">At times, you may need tooprogrammatically add an extension.</span></span> <span data-ttu-id="8e6a3-144">När du behöver toodo detta, måste du ange hello konfigurationen av hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-144">When you need toodo this, you must provide hello configuration of hello extension.</span></span>

<span data-ttu-id="8e6a3-145">Du kan använda följande cmdlets tooknow om filnamnstillägget kräver en konfiguration av offentliga och privata parametrar hello:</span><span class="sxs-lookup"><span data-stu-id="8e6a3-145">You can use hello following cmdlets tooknow whether an extension requires a configuration of public and private parameters:</span></span>

* <span data-ttu-id="8e6a3-146">För instanser av webbroller eller arbetsroller kan du använda hello **Get-AzureServiceAvailableExtension** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-146">For instances of web roles or worker roles, you can use hello **Get-AzureServiceAvailableExtension** cmdlet.</span></span>
* <span data-ttu-id="8e6a3-147">För instanser av virtuella datorer, kan du använda hello **Get-AzureVMAvailableExtension** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-147">For instances of Virtual Machines, you can use hello **Get-AzureVMAvailableExtension** cmdlet.</span></span>

### <a name="service-management-rest-apis"></a><span data-ttu-id="8e6a3-148">REST API:er för tjänsthantering</span><span class="sxs-lookup"><span data-stu-id="8e6a3-148">Service Management REST APIs</span></span>
<span data-ttu-id="8e6a3-149">När du hämtar en lista över tillgängliga tillägg genom att använda hello REST API: er, får du information om hur hello tillägget är toobe konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-149">When you retrieve a listing of available extensions by using hello REST APIs, you receive information about how hello extension is toobe configured.</span></span> <span data-ttu-id="8e6a3-150">hello information som returneras kan visa parameterinformation som representeras av ett schema med offentliga och privata schemat.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-150">hello information that is returned might show parameter information represented by a public schema and private schema.</span></span> <span data-ttu-id="8e6a3-151">Gemensamma parametervärden returneras i frågor om hello instanser.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-151">Public parameter values are returned in queries about hello instances.</span></span> <span data-ttu-id="8e6a3-152">Privata parametervärden returneras inte.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-152">Private parameter values are not returned.</span></span>

<span data-ttu-id="8e6a3-153">Du kan använda hello följande REST API: er tooknow om filnamnstillägget kräver en konfiguration av offentliga och privata parametrar:</span><span class="sxs-lookup"><span data-stu-id="8e6a3-153">You can use hello following REST APIs tooknow whether an extension requires a configuration of public and private parameters:</span></span>

* <span data-ttu-id="8e6a3-154">För instanser av webbroller eller arbetsroller, hello **PublicConfigurationSchema** och **PrivateConfigurationSchema** element innehåller hello information i hello-svar från hello [lista Tillgängliga tillägg](https://msdn.microsoft.com/library/dn169559.aspx) igen.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-154">For instances of web roles or worker roles, hello **PublicConfigurationSchema** and **PrivateConfigurationSchema** elements contain hello information in hello response from hello [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) operation.</span></span>
* <span data-ttu-id="8e6a3-155">För instanser av virtuella datorer, hello **PublicConfigurationSchema** och **PrivateConfigurationSchema** element innehåller hello information i hello-svar från hello [lista Resurstillägg](https://msdn.microsoft.com/library/dn495441.aspx) igen.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-155">For instances of Virtual Machines, hello **PublicConfigurationSchema** and **PrivateConfigurationSchema** elements contain hello information in hello response from hello [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) operation.</span></span>

> [!NOTE]
> <span data-ttu-id="8e6a3-156">Tillägg kan också använda konfigurationer som har definierats med JSON.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-156">Extensions can also use configurations that are defined with JSON.</span></span> <span data-ttu-id="8e6a3-157">När dessa typer av tillägg används endast hello **SampleConfig** elementet används.</span><span class="sxs-lookup"><span data-stu-id="8e6a3-157">When these types of extensions are used, only hello **SampleConfig** element is used.</span></span>
> 
> 

