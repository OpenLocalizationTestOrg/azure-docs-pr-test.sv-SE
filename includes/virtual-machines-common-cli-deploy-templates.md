
* [<span data-ttu-id="9ce1a-101">Snabbskapa en virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="9ce1a-101">Quick-create a virtual machine in Azure</span></span>](#quick-create-a-vm-in-azure)
* [<span data-ttu-id="9ce1a-102">Distribuera en virtuell dator i Azure från en mall</span><span class="sxs-lookup"><span data-stu-id="9ce1a-102">Deploy a virtual machine in Azure from a template</span></span>](#deploy-a-vm-in-azure-from-a-template)
* [<span data-ttu-id="9ce1a-103">Skapa en virtuell dator från en anpassad avbildning</span><span class="sxs-lookup"><span data-stu-id="9ce1a-103">Create a virtual machine from a custom image</span></span>](#create-a-custom-vm-image)
* [<span data-ttu-id="9ce1a-104">Distribuera en virtuell dator som använder ett virtuellt nätverk och en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="9ce1a-104">Deploy a virtual machine that uses a virtual network and a load balancer</span></span>](#deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer)
* [<span data-ttu-id="9ce1a-105">Ta bort en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="9ce1a-105">Remove a resource group</span></span>](#remove-a-resource-group)
* [<span data-ttu-id="9ce1a-106">Visa hello logg för en resurs för gruppdistributionen</span><span class="sxs-lookup"><span data-stu-id="9ce1a-106">Show hello log for a resource group deployment</span></span>](#show-the-log-for-a-resource-group-deployment)
* [<span data-ttu-id="9ce1a-107">Visa information om en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9ce1a-107">Display information about a virtual machine</span></span>](#display-information-about-a-virtual-machine)
* [<span data-ttu-id="9ce1a-108">Ansluta tooa Linux-baserade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="9ce1a-108">Connect tooa Linux-based virtual machine</span></span>](#log-on-to-a-linux-based-virtual-machine)
* [<span data-ttu-id="9ce1a-109">Stoppa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9ce1a-109">Stop a virtual machine</span></span>](#stop-a-virtual-machine)
* [<span data-ttu-id="9ce1a-110">Starta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9ce1a-110">Start a virtual machine</span></span>](#start-a-virtual-machine)
* [<span data-ttu-id="9ce1a-111">Anslut en datadisk</span><span class="sxs-lookup"><span data-stu-id="9ce1a-111">Attach a data disk</span></span>](#attach-a-data-disk)

## <a name="getting-ready"></a><span data-ttu-id="9ce1a-112">Komma igång</span><span class="sxs-lookup"><span data-stu-id="9ce1a-112">Getting ready</span></span>
<span data-ttu-id="9ce1a-113">Innan du kan använda hello Azure CLI med Azure-resursgrupper, behöver du toohave hello rätt Azure CLI version och ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-113">Before you can use hello Azure CLI with Azure resource groups, you need toohave hello right Azure CLI version and an Azure account.</span></span> <span data-ttu-id="9ce1a-114">Om du inte har hello Azure CLI [installera den](../articles/cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-114">If you don't have hello Azure CLI, [install it](../articles/cli-install-nodejs.md).</span></span>

### <a name="update-your-azure-cli-version-too090-or-later"></a><span data-ttu-id="9ce1a-115">Uppdatera din Azure CLI version too0.9.0 eller senare</span><span class="sxs-lookup"><span data-stu-id="9ce1a-115">Update your Azure CLI version too0.9.0 or later</span></span>
<span data-ttu-id="9ce1a-116">Typen `azure --version` toosee om du redan har installerat version 0.9.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-116">Type `azure --version` toosee whether you have already installed version 0.9.0 or later.</span></span>

```azurecli
azure --version
0.9.0 (node: 0.10.25)
```

<span data-ttu-id="9ce1a-117">Om din version är inte 0.9.0 eller senare behöver du tooupdate den med något av hello interna installationsprogram eller via **npm** genom att skriva `npm update -g azure-cli`.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-117">If your version is not 0.9.0 or later, you need tooupdate it by using one of hello native installers or through **npm** by typing `npm update -g azure-cli`.</span></span>

<span data-ttu-id="9ce1a-118">Du kan också köra Azure CLI som en dockerbehållare med hjälp av följande hello [Docker bild](https://registry.hub.docker.com/u/microsoft/azure-cli/).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-118">You can also run Azure CLI as a Docker container by using hello following [Docker image](https://registry.hub.docker.com/u/microsoft/azure-cli/).</span></span> <span data-ttu-id="9ce1a-119">Kör hello följande kommando från en Docker-värd:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-119">From a Docker host, run hello following command:</span></span>

```bash
docker run -it microsoft/azure-cli
```

### <a name="set-your-azure-account-and-subscription"></a><span data-ttu-id="9ce1a-120">Konfigurera Azure-kontot och Azure-prenumerationen</span><span class="sxs-lookup"><span data-stu-id="9ce1a-120">Set your Azure account and subscription</span></span>
<span data-ttu-id="9ce1a-121">Om du inte har någon Azure-prenumeration men en MSDN-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-121">If you don't already have an Azure subscription but you do have an MSDN subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span> <span data-ttu-id="9ce1a-122">Du kan även anmäla dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-122">Or you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

<span data-ttu-id="9ce1a-123">Nu [logga in tooyour Azure-konto interaktivt](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login) genom att skriva `azure login` och följa hello efterfrågas en interaktiv inloggning upplevelse tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-123">Now [log in tooyour Azure account interactively](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login) by typing `azure login` and following hello prompts for an interactive login experience tooyour Azure account.</span></span> 

> [!NOTE]
> <span data-ttu-id="9ce1a-124">Om du har ett arbets- eller skolkonto ID och du vet du inte har tvåfaktorsautentisering aktiverad, kan du **också** använder `azure login -u` tillsammans med hello arbetsplats eller skola ID toolog i *utan* en interaktiv session.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-124">If you have a work or school ID and you know you do not have two-factor authentication enabled, you can **also** use `azure login -u` along with hello work or school ID toolog in *without* an interactive session.</span></span> <span data-ttu-id="9ce1a-125">Om du inte har ett arbets- eller skolkonto ID, kan du [skapar ett arbets- eller skolkonto id från ditt personliga Microsoft-konto](../articles/virtual-machines/windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toolog i hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-125">If you don't have a work or school ID, you can [create a work or school id from your personal Microsoft account](../articles/virtual-machines/windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toolog in hello same way.</span></span>
>
>

<span data-ttu-id="9ce1a-126">Ditt konto kan innehålla fler än en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-126">Your account may have more than one subscription.</span></span> <span data-ttu-id="9ce1a-127">Du kan visa prenumerationerna genom att skriva `azure account list`, som kan se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-127">You can list your subscriptions by typing `azure account list`, which might look something like this:</span></span>

```azurecli
azure account list
info:    Executing command account list
data:    Name                              Id                                    Tenant Id                            Current
data:    --------------------------------  ------------------------------------  ------------------------------------  -------
data:    Contoso Admin                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  true
data:    Fabrikam dev                      xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Fabrikam test                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Contoso production                xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
```

<span data-ttu-id="9ce1a-128">Du kan ange hello aktuella Azure-prenumeration genom att skriva följande hello.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-128">You can set hello current Azure subscription by typing hello following.</span></span> <span data-ttu-id="9ce1a-129">Använd hello prenumeration namn eller hello-ID som har hello-resurser som du vill toomanage.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-129">Use hello subscription name or hello ID that has hello resources you want toomanage.</span></span>

```azurecli
azure account set <subscription name or ID> true
```

### <a name="switch-toohello-azure-cli-resource-group-mode"></a><span data-ttu-id="9ce1a-130">Växla toohello Azure CLI resurs Grupperingsläge</span><span class="sxs-lookup"><span data-stu-id="9ce1a-130">Switch toohello Azure CLI resource group mode</span></span>
<span data-ttu-id="9ce1a-131">Som standard startar hello Azure CLI i läget för hello-hantering (**asm** läge).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-131">By default, hello Azure CLI starts in hello service management mode (**asm** mode).</span></span> <span data-ttu-id="9ce1a-132">Ange hello följande tooswitch tooresource Grupperingsläge.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-132">Type hello following tooswitch tooresource group mode.</span></span>

```azurecli
azure config mode arm
```

## <a name="understanding-azure-resource-templates-and-resource-groups"></a><span data-ttu-id="9ce1a-133">Förstå Azure-resursmallar och -resursgrupper</span><span class="sxs-lookup"><span data-stu-id="9ce1a-133">Understanding Azure resource templates and resource groups</span></span>
<span data-ttu-id="9ce1a-134">De flesta program skapas med en kombination av olika resurstyper (t.ex. en eller flera virtuella datorer och lagringskonton, en SQL-databas, ett virtuellt nätverk eller ett innehållsleveransnätverk).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-134">Most applications are built from a combination of different resource types (such as one or more VMs and storage accounts, a SQL database, a virtual network, or a content delivery network).</span></span> <span data-ttu-id="9ce1a-135">Hej standard Azure service management API och hello klassiska Azure-portalen representeras dessa objekt med hjälp av en metod av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-135">hello default Azure service management API and hello Azure classic portal represented these items by using a service-by-service approach.</span></span> <span data-ttu-id="9ce1a-136">Den här metoden kräver toodeploy och hantera hello enskilda tjänster individuellt (eller andra verktyg som gör att hitta), och inte som en logisk enhet för distributionen.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-136">This approach requires you toodeploy and manage hello individual services individually (or find other tools that do so), and not as a single logical unit of deployment.</span></span>

<span data-ttu-id="9ce1a-137">*Azure Resource Manager-mallar*, men gör det möjligt för du toodeploy och hantera dessa olika resurser som en logisk distributionsenhet deklarativ överskrids.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-137">*Azure Resource Manager templates*, however, make it possible for you toodeploy and manage these different resources as one logical deployment unit in a declarative fashion.</span></span> <span data-ttu-id="9ce1a-138">I stället för imperatively uppmanar Azure vilka toodeploy ett kommando efter en annan, beskrivs i en JSON-fil – alla hello resurser och associerade konfigurationen och distributionen parametrar--hela distributionen och berätta Azure toodeploy resurserna som en grupp.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-138">Instead of imperatively telling Azure what toodeploy one command after another, you describe your entire deployment in a JSON file -- all of hello resources and associated configuration and deployment parameters -- and tell Azure toodeploy those resources as one group.</span></span>

<span data-ttu-id="9ce1a-139">Du kan sedan hantera hello övergripande livscykel hello gruppera resurser med hjälp av Azure CLI resurs management kommandon för att:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-139">You can then manage hello overall life cycle of hello group's resources by using Azure CLI resource management commands to:</span></span>

* <span data-ttu-id="9ce1a-140">Stoppa, starta eller ta bort alla hello resurser inom hello grupp samtidigt.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-140">Stop, start, or delete all of hello resources within hello group at once.</span></span>
* <span data-ttu-id="9ce1a-141">Använda rollbaserad åtkomstkontroll (RBAC) regler toolock ned säkerhetsbehörigheter på dem.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-141">Apply Role-Based Access Control (RBAC) rules toolock down security permissions on them.</span></span>
* <span data-ttu-id="9ce1a-142">Granskningsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-142">Audit operations.</span></span>
* <span data-ttu-id="9ce1a-143">Tagga resurser med ytterligare metadata för bättre spårning.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-143">Tag resources with additional metadata for better tracking.</span></span>

<span data-ttu-id="9ce1a-144">Du kan lära dig mer om Azure-resursgrupper och vad de kan du göra i hello många [översikt över Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-144">You can learn lots more about Azure resource groups and what they can do for you in hello [Azure Resource Manager overview](../articles/azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="9ce1a-145">Om du är intresserad av att skapa mallar, se [Skapa Azure Resource Manager-mallar](../articles/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-145">If you're interested in authoring templates, see [Authoring Azure Resource Manager templates](../articles/resource-group-authoring-templates.md).</span></span>

## <span data-ttu-id="9ce1a-146"><a id="quick-create-a-vm-in-azure"></a>Uppgift: Snabbt skapa en virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="9ce1a-146"><a id="quick-create-a-vm-in-azure"></a>Task: Quick-create a VM in Azure</span></span>
<span data-ttu-id="9ce1a-147">Du vet ibland vilken bild som du behöver, och du behöver en virtuell dator från den avbildningen just nu och du bryr dig inte så mycket om hello infrastruktur – du kanske har tootest något på en ren VM.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-147">Sometimes you know what image you need, and you need a VM from that image right now and you don't care too much about hello infrastructure -- maybe you have tootest something on a clean VM.</span></span> <span data-ttu-id="9ce1a-148">Det är då du vill toouse hello `azure vm quick-create` kommandot och skicka hello argument nödvändiga toocreate en virtuell dator och sin infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-148">That's when you want toouse hello `azure vm quick-create` command, and pass hello arguments necessary toocreate a VM and its infrastructure.</span></span>

<span data-ttu-id="9ce1a-149">Skapa först en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-149">First, create your resource group.</span></span>

```azurecli
azure group create coreos-quick westus
info:    Executing command group create
+ Getting resource group coreos-quick
+ Creating resource group coreos-quick
info:    Created resource group coreos-quick
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick
data:    Name:                coreos-quick
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

<span data-ttu-id="9ce1a-150">Sedan behöver du en avbildning.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-150">Second, you'll need an image.</span></span> <span data-ttu-id="9ce1a-151">toofind en bild med hello Azure CLI, se [navigering och välja avbildningar för virtuella Azure-datorn med PowerShell och hello Azure CLI](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-151">toofind an image with hello Azure CLI, see [Navigating and selecting Azure virtual machine images with PowerShell and hello Azure CLI](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="9ce1a-152">Men här är en kort lista med populära avbildningar för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-152">But for this article, here's a short list of popular images.</span></span> <span data-ttu-id="9ce1a-153">Vi använder CoreOS stabila avbildning för den här snabbövningen.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-153">We'll use CoreOS's Stable image for this quick-create.</span></span>

> [!NOTE]
> <span data-ttu-id="9ce1a-154">För ComputeImageVersion, du kan också bara kan ange ”senaste” som hello parameter i båda hello mallspråk och hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-154">For ComputeImageVersion, you can also simply supply 'latest' as hello parameter in both hello template language and in hello Azure CLI.</span></span> <span data-ttu-id="9ce1a-155">Detta gör att du tooalways använder hello senaste och uppdaterad version av hello bild utan toomodify skript och mallar.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-155">This will allow you tooalways use hello latest and patched version of hello image without having toomodify your scripts or templates.</span></span> <span data-ttu-id="9ce1a-156">Detta visas nedan.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-156">This is shown below.</span></span>
>
>

| <span data-ttu-id="9ce1a-157">PublisherName</span><span class="sxs-lookup"><span data-stu-id="9ce1a-157">PublisherName</span></span> | <span data-ttu-id="9ce1a-158">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="9ce1a-158">Offer</span></span> | <span data-ttu-id="9ce1a-159">Sku</span><span class="sxs-lookup"><span data-stu-id="9ce1a-159">Sku</span></span> | <span data-ttu-id="9ce1a-160">Version</span><span class="sxs-lookup"><span data-stu-id="9ce1a-160">Version</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="9ce1a-161">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="9ce1a-161">OpenLogic</span></span> |<span data-ttu-id="9ce1a-162">CentOS</span><span class="sxs-lookup"><span data-stu-id="9ce1a-162">CentOS</span></span> |<span data-ttu-id="9ce1a-163">7</span><span class="sxs-lookup"><span data-stu-id="9ce1a-163">7</span></span> |<span data-ttu-id="9ce1a-164">7.0.201503</span><span class="sxs-lookup"><span data-stu-id="9ce1a-164">7.0.201503</span></span> |
| <span data-ttu-id="9ce1a-165">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="9ce1a-165">OpenLogic</span></span> |<span data-ttu-id="9ce1a-166">CentOS</span><span class="sxs-lookup"><span data-stu-id="9ce1a-166">CentOS</span></span> |<span data-ttu-id="9ce1a-167">7.1</span><span class="sxs-lookup"><span data-stu-id="9ce1a-167">7.1</span></span> |<span data-ttu-id="9ce1a-168">7.1.201504</span><span class="sxs-lookup"><span data-stu-id="9ce1a-168">7.1.201504</span></span> |
| <span data-ttu-id="9ce1a-169">CoreOS</span><span class="sxs-lookup"><span data-stu-id="9ce1a-169">CoreOS</span></span> |<span data-ttu-id="9ce1a-170">CoreOS</span><span class="sxs-lookup"><span data-stu-id="9ce1a-170">CoreOS</span></span> |<span data-ttu-id="9ce1a-171">Beta</span><span class="sxs-lookup"><span data-stu-id="9ce1a-171">Beta</span></span> |<span data-ttu-id="9ce1a-172">647.0.0</span><span class="sxs-lookup"><span data-stu-id="9ce1a-172">647.0.0</span></span> |
| <span data-ttu-id="9ce1a-173">CoreOS</span><span class="sxs-lookup"><span data-stu-id="9ce1a-173">CoreOS</span></span> |<span data-ttu-id="9ce1a-174">CoreOS</span><span class="sxs-lookup"><span data-stu-id="9ce1a-174">CoreOS</span></span> |<span data-ttu-id="9ce1a-175">Stable</span><span class="sxs-lookup"><span data-stu-id="9ce1a-175">Stable</span></span> |<span data-ttu-id="9ce1a-176">633.1.0</span><span class="sxs-lookup"><span data-stu-id="9ce1a-176">633.1.0</span></span> |
| <span data-ttu-id="9ce1a-177">MicrosoftDynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="9ce1a-177">MicrosoftDynamicsNAV</span></span> |<span data-ttu-id="9ce1a-178">DynamicsNAV</span><span class="sxs-lookup"><span data-stu-id="9ce1a-178">DynamicsNAV</span></span> |<span data-ttu-id="9ce1a-179">2015</span><span class="sxs-lookup"><span data-stu-id="9ce1a-179">2015</span></span> |<span data-ttu-id="9ce1a-180">8.0.40459</span><span class="sxs-lookup"><span data-stu-id="9ce1a-180">8.0.40459</span></span> |
| <span data-ttu-id="9ce1a-181">MicrosoftSharePoint</span><span class="sxs-lookup"><span data-stu-id="9ce1a-181">MicrosoftSharePoint</span></span> |<span data-ttu-id="9ce1a-182">MicrosoftSharePointServer</span><span class="sxs-lookup"><span data-stu-id="9ce1a-182">MicrosoftSharePointServer</span></span> |<span data-ttu-id="9ce1a-183">2013</span><span class="sxs-lookup"><span data-stu-id="9ce1a-183">2013</span></span> |<span data-ttu-id="9ce1a-184">1.0.0</span><span class="sxs-lookup"><span data-stu-id="9ce1a-184">1.0.0</span></span> |
| <span data-ttu-id="9ce1a-185">msopentech</span><span class="sxs-lookup"><span data-stu-id="9ce1a-185">msopentech</span></span> |<span data-ttu-id="9ce1a-186">Oracle-Database-12c-Weblogic-Server-12c</span><span class="sxs-lookup"><span data-stu-id="9ce1a-186">Oracle-Database-12c-Weblogic-Server-12c</span></span> |<span data-ttu-id="9ce1a-187">Standard</span><span class="sxs-lookup"><span data-stu-id="9ce1a-187">Standard</span></span> |<span data-ttu-id="9ce1a-188">1.0.0</span><span class="sxs-lookup"><span data-stu-id="9ce1a-188">1.0.0</span></span> |
| <span data-ttu-id="9ce1a-189">msopentech</span><span class="sxs-lookup"><span data-stu-id="9ce1a-189">msopentech</span></span> |<span data-ttu-id="9ce1a-190">Oracle-Database-12c-Weblogic-Server-12c</span><span class="sxs-lookup"><span data-stu-id="9ce1a-190">Oracle-Database-12c-Weblogic-Server-12c</span></span> |<span data-ttu-id="9ce1a-191">Enterprise</span><span class="sxs-lookup"><span data-stu-id="9ce1a-191">Enterprise</span></span> |<span data-ttu-id="9ce1a-192">1.0.0</span><span class="sxs-lookup"><span data-stu-id="9ce1a-192">1.0.0</span></span> |
| <span data-ttu-id="9ce1a-193">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="9ce1a-193">MicrosoftSQLServer</span></span> |<span data-ttu-id="9ce1a-194">SQL2014-WS2012R2</span><span class="sxs-lookup"><span data-stu-id="9ce1a-194">SQL2014-WS2012R2</span></span> |<span data-ttu-id="9ce1a-195">Enterprise-Optimized-for-DW</span><span class="sxs-lookup"><span data-stu-id="9ce1a-195">Enterprise-Optimized-for-DW</span></span> |<span data-ttu-id="9ce1a-196">12.0.2430</span><span class="sxs-lookup"><span data-stu-id="9ce1a-196">12.0.2430</span></span> |
| <span data-ttu-id="9ce1a-197">MicrosoftSQLServer</span><span class="sxs-lookup"><span data-stu-id="9ce1a-197">MicrosoftSQLServer</span></span> |<span data-ttu-id="9ce1a-198">SQL2014-WS2012R2</span><span class="sxs-lookup"><span data-stu-id="9ce1a-198">SQL2014-WS2012R2</span></span> |<span data-ttu-id="9ce1a-199">Enterprise-Optimized-for-OLTP</span><span class="sxs-lookup"><span data-stu-id="9ce1a-199">Enterprise-Optimized-for-OLTP</span></span> |<span data-ttu-id="9ce1a-200">12.0.2430</span><span class="sxs-lookup"><span data-stu-id="9ce1a-200">12.0.2430</span></span> |
| <span data-ttu-id="9ce1a-201">Canonical</span><span class="sxs-lookup"><span data-stu-id="9ce1a-201">Canonical</span></span> |<span data-ttu-id="9ce1a-202">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="9ce1a-202">UbuntuServer</span></span> |<span data-ttu-id="9ce1a-203">12.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="9ce1a-203">12.04.5-LTS</span></span> |<span data-ttu-id="9ce1a-204">12.04.201504230</span><span class="sxs-lookup"><span data-stu-id="9ce1a-204">12.04.201504230</span></span> |
| <span data-ttu-id="9ce1a-205">Canonical</span><span class="sxs-lookup"><span data-stu-id="9ce1a-205">Canonical</span></span> |<span data-ttu-id="9ce1a-206">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="9ce1a-206">UbuntuServer</span></span> |<span data-ttu-id="9ce1a-207">14.04.2-LTS</span><span class="sxs-lookup"><span data-stu-id="9ce1a-207">14.04.2-LTS</span></span> |<span data-ttu-id="9ce1a-208">14.04.201503090</span><span class="sxs-lookup"><span data-stu-id="9ce1a-208">14.04.201503090</span></span> |
| <span data-ttu-id="9ce1a-209">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="9ce1a-209">MicrosoftWindowsServer</span></span> |<span data-ttu-id="9ce1a-210">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="9ce1a-210">WindowsServer</span></span> |<span data-ttu-id="9ce1a-211">2012-Datacenter</span><span class="sxs-lookup"><span data-stu-id="9ce1a-211">2012-Datacenter</span></span> |<span data-ttu-id="9ce1a-212">3.0.201503</span><span class="sxs-lookup"><span data-stu-id="9ce1a-212">3.0.201503</span></span> |
| <span data-ttu-id="9ce1a-213">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="9ce1a-213">MicrosoftWindowsServer</span></span> |<span data-ttu-id="9ce1a-214">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="9ce1a-214">WindowsServer</span></span> |<span data-ttu-id="9ce1a-215">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="9ce1a-215">2012-R2-Datacenter</span></span> |<span data-ttu-id="9ce1a-216">4.0.201503</span><span class="sxs-lookup"><span data-stu-id="9ce1a-216">4.0.201503</span></span> |
| <span data-ttu-id="9ce1a-217">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="9ce1a-217">MicrosoftWindowsServer</span></span> |<span data-ttu-id="9ce1a-218">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="9ce1a-218">WindowsServer</span></span> |<span data-ttu-id="9ce1a-219">Windows-Server-Technical-Preview</span><span class="sxs-lookup"><span data-stu-id="9ce1a-219">Windows-Server-Technical-Preview</span></span> |<span data-ttu-id="9ce1a-220">5.0.201504</span><span class="sxs-lookup"><span data-stu-id="9ce1a-220">5.0.201504</span></span> |
| <span data-ttu-id="9ce1a-221">MicrosoftWindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="9ce1a-221">MicrosoftWindowsServerEssentials</span></span> |<span data-ttu-id="9ce1a-222">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="9ce1a-222">WindowsServerEssentials</span></span> |<span data-ttu-id="9ce1a-223">WindowsServerEssentials</span><span class="sxs-lookup"><span data-stu-id="9ce1a-223">WindowsServerEssentials</span></span> |<span data-ttu-id="9ce1a-224">1.0.141204</span><span class="sxs-lookup"><span data-stu-id="9ce1a-224">1.0.141204</span></span> |
| <span data-ttu-id="9ce1a-225">MicrosoftWindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="9ce1a-225">MicrosoftWindowsServerHPCPack</span></span> |<span data-ttu-id="9ce1a-226">WindowsServerHPCPack</span><span class="sxs-lookup"><span data-stu-id="9ce1a-226">WindowsServerHPCPack</span></span> |<span data-ttu-id="9ce1a-227">2012R2</span><span class="sxs-lookup"><span data-stu-id="9ce1a-227">2012R2</span></span> |<span data-ttu-id="9ce1a-228">4.3.4665</span><span class="sxs-lookup"><span data-stu-id="9ce1a-228">4.3.4665</span></span> |

<span data-ttu-id="9ce1a-229">Skapa den virtuella datorn genom att ange hello `azure vm quick-create` kommandot och är redo för hello anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-229">Just create your VM by entering hello `azure vm quick-create` command and being ready for hello prompts.</span></span> <span data-ttu-id="9ce1a-230">Det bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-230">It should look something like this:</span></span>

```azurecli
azure vm quick-create
info:    Executing command vm quick-create
Resource group name: coreos-quick
Virtual machine name: coreos
Location name: westus
Operating system Type [Windows, Linux]: linux
ImageURN (format: "publisherName:offer:skus:version"): coreos:coreos:stable:latest
User name: ops
Password: *********
Confirm password: *********
+ Looking up hello VM "coreos"
info:    Using hello VM Size "Standard_A1"
info:    hello [OS, Data] Disk or image configuration requires storage account
+ Retrieving storage accounts
info:    Could not find any storage accounts in hello region "westus", trying toocreate new one
+ Creating storage account "cli9fd3fce49e9a9b3d14302" in "westus"
+ Looking up hello storage account cli9fd3fce49e9a9b3d14302
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
info:    An nic with given name "coreo-westu-1430261891570-nic" not found, creating a new one
+ Looking up hello virtual network "coreo-westu-1430261891570-vnet"
info:    Preparing toocreate new virtual network and subnet
/ Creating a new virtual network "coreo-westu-1430261891570-vnet" [address prefix: "10.0.0.0/16"] with subnet "coreo-westu-1430261891570-sne+" [address prefix: "10.0.1.0/24"]
+ Looking up hello virtual network "coreo-westu-1430261891570-vnet"
+ Looking up hello subnet "coreo-westu-1430261891570-snet" under hello virtual network "coreo-westu-1430261891570-vnet"
info:    Found public ip parameters, trying toosetup PublicIP profile
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
info:    PublicIP with given name "coreo-westu-1430261891570-pip" not found, creating a new one
+ Creating public ip "coreo-westu-1430261891570-pip"
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
+ Creating NIC "coreo-westu-1430261891570-nic"
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
+ Creating VM "coreos"
+ Looking up hello VM "coreos"
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Compute/virtualMachines/coreos
data:    ProvisioningState               :Succeeded
data:    Name                            :coreos
data:    Location                        :westus
data:    FQDN                            :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :coreos
data:        Offer                       :coreos
data:        Sku                         :stable
data:        Version                     :633.1.0
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :cli9fd3fce49e9a9b3d-os-1430261892283
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli9fd3fce49e9a9b3d14302.blob.core.windows.net/vhds/cli9fd3fce49e9a9b3d-os-1430261892283.vhd
data:
data:    OS Profile:
data:      Computer Name                 :coreos
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :false
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Network/networkInterfaces/coreo-westu-1430261891570-nic
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-30-72-E3
data:          Provisioning State        :Succeeded
data:          Name                      :coreo-westu-1430261891570-nic
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.1.4
data:            Public IP address       :104.40.24.124
data:            FQDN                    :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
info:    vm quick-create command OK
```

<span data-ttu-id="9ce1a-231">Och så fortsätter du med din nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-231">And away you go with your new VM.</span></span>

## <span data-ttu-id="9ce1a-232"><a id="deploy-a-vm-in-azure-from-a-template"></a>Uppgift: Distribuera en virtuell dator i Azure från en mall</span><span class="sxs-lookup"><span data-stu-id="9ce1a-232"><a id="deploy-a-vm-in-azure-from-a-template"></a>Task: Deploy a VM in Azure from a template</span></span>
<span data-ttu-id="9ce1a-233">Använd hello instruktioner i dessa avsnitt toodeploy en ny virtuell Azure-dator med hjälp av en mall med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-233">Use hello instructions in these sections toodeploy a new Azure VM by using a template with hello Azure CLI.</span></span> <span data-ttu-id="9ce1a-234">Den här mallen skapar en virtuell dator i ett nytt virtuellt nätverk med ett enda undernät och till skillnad från `azure vm quick-create`, aktiverar du toodescribe vad du vill exakt och upprepa utan fel.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-234">This template creates a single virtual machine in a new virtual network with a single subnet, and unlike `azure vm quick-create`, enables you toodescribe what you want precisely and repeat it without errors.</span></span> <span data-ttu-id="9ce1a-235">Mallen skapar följande:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-235">Here's what this template creates:</span></span>

![](./media/virtual-machines-common-cli-deploy-templates/new-vm.png)

### <a name="step-1-examine-hello-json-file-for-hello-template-parameters"></a><span data-ttu-id="9ce1a-236">Steg 1: Kontrollera hello JSON-filen för hello mallparametrar</span><span class="sxs-lookup"><span data-stu-id="9ce1a-236">Step 1: Examine hello JSON file for hello template parameters</span></span>
<span data-ttu-id="9ce1a-237">Här följer hello innehållet i hello JSON-fil för hello mall.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-237">Here are hello contents of hello JSON file for hello template.</span></span> <span data-ttu-id="9ce1a-238">(hello mall finns också i [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json).)</span><span class="sxs-lookup"><span data-stu-id="9ce1a-238">(hello template is also located in [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json).)</span></span>

<span data-ttu-id="9ce1a-239">Mallar är flexibla, så kan ha valt toogive många parametrar eller valt toooffer endast några få genom att skapa en mall som är mer fast hello designer.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-239">Templates are flexible, so hello designer may have chosen toogive you lots of parameters or chosen toooffer only a few by creating a template that is more fixed.</span></span> <span data-ttu-id="9ce1a-240">Öppna hello mallfilen (det här avsnittet innehåller en mall infogad nedan) i ordning toocollect hello information du behöver toopass hello mall som parametrar och undersöka hello **parametrar** värden.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-240">In order toocollect hello information you need toopass hello template as parameters, open hello template file (this topic has a template inline, below) and examine hello **parameters** values.</span></span>

<span data-ttu-id="9ce1a-241">I det här fallet att hello mallen nedan uppmanas:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-241">In this case, hello template below will ask for:</span></span>

* <span data-ttu-id="9ce1a-242">Ett unikt namn på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-242">A unique storage account name.</span></span>
* <span data-ttu-id="9ce1a-243">En administratör användarnamn för hello VM.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-243">An admin user name for hello VM.</span></span>
* <span data-ttu-id="9ce1a-244">Ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-244">A password.</span></span>
* <span data-ttu-id="9ce1a-245">Ett domännamn för hello utanför world toouse.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-245">A domain name for hello outside world toouse.</span></span>
* <span data-ttu-id="9ce1a-246">Ett versionsnummer till Ubuntu Server – men den accepterar endast en i en lista.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-246">An Ubuntu Server version number -- but it will accept only one of a list.</span></span>

<span data-ttu-id="9ce1a-247">Läs mer om [krav för användarnamn och lösenord](../articles/virtual-machines/linux/faq.md#what-are-the-username-requirements-when-creating-a-vm).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-247">See more about [username and password requirements](../articles/virtual-machines/linux/faq.md#what-are-the-username-requirements-when-creating-a-vm).</span></span>

<span data-ttu-id="9ce1a-248">När du har bestämt på dessa värden och är redo toocreate en grupp för och distribuera den här mallen till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-248">Once you decide on these values, you're ready toocreate a group for and deploy this template into your Azure subscription.</span></span>

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "newStorageAccountName": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for hello storage account where hello virtual machine's disks will be placed."
    }
    },
    "adminUsername": {
    "type": "string",
    "metadata": {
        "description": "User name for hello virtual machine."
    }
    },
    "adminPassword": {
    "type": "securestring",
    "metadata": {
        "description": "Password for hello virtual machine."
    }
    },
    "dnsNameForPublicIP": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for hello public IP used tooaccess hello virtual machine."
    }
    },
    "ubuntuOSVersion": {
    "type": "string",
    "defaultValue": "14.04.2-LTS",
    "allowedValues": [
        "12.04.5-LTS",
        "14.04.2-LTS",
        "15.04"
    ],
    "metadata": {
        "description": "hello Ubuntu version for hello VM. This will pick a fully patched image of this given Ubuntu version. Allowed values: 12.04.5-LTS, 14.04.2-LTS, 15.04."
    }
    }
},
"variables": {
    "location": "West US",
    "imagePublisher": "Canonical",
    "imageOffer": "UbuntuServer",
    "OSDiskName": "osdiskforlinuxsimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyUbuntuVM",
    "vmSize": "Standard_D1",
    "virtualNetworkName": "MyVNET",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
},
"resources": [
    {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('newStorageAccountName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[variables('location')]",
    "properties": {
        "accountType": "[variables('storageAccountType')]"
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[variables('publicIPAddressName')]",
    "location": "[variables('location')]",
    "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
        "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
        }
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
    "location": "[variables('location')]",
    "properties": {
        "addressSpace": {
        "addressPrefixes": [
            "[variables('addressPrefix')]"
        ]
        },
        "subnets": [
        {
            "name": "[variables('subnetName')]",
            "properties": {
            "addressPrefix": "[variables('subnetPrefix')]"
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('nicName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
    ],
    "properties": {
        "ipConfigurations": [
        {
            "name": "ipconfig1",
            "properties": {
            "privateIPAllocationMethod": "Dynamic",
            "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
            },
            "subnet": {
                "id": "[variables('subnetRef')]"
            }
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', parameters('newStorageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
        "computername": "[variables('vmName')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
        "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('ubuntuOSVersion')]",
            "version": "latest"
        },
        "osDisk": {
            "name": "osdisk",
            "vhd": {
            "uri": "[concat('http://',parameters('newStorageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
        }
        },
        "networkProfile": {
        "networkInterfaces": [
            {
            "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
        ]
        }
    }
    }
]
}
```

### <a name="step-2-create-hello-virtual-machine-by-using-hello-template"></a><span data-ttu-id="9ce1a-249">Steg 2: Skapa hello virtuell dator med hjälp av hello mall</span><span class="sxs-lookup"><span data-stu-id="9ce1a-249">Step 2: Create hello virtual machine by using hello template</span></span>
<span data-ttu-id="9ce1a-250">När du har din parametervärden som är klar måste du skapa en resursgrupp för distributionen mall och sedan distribuera hello mallen.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-250">Once you have your parameter values ready, you must create a resource group for your template deployment and then deploy hello template.</span></span>

<span data-ttu-id="9ce1a-251">toocreate hello resursgrupp, typen `azure group create <group name> <location>` med hello namnet hello-grupp som du vill och hello datacenter plats som du vill toodeploy.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-251">toocreate hello resource group, type `azure group create <group name> <location>` with hello name of hello group you want and hello datacenter location into which you want toodeploy.</span></span> <span data-ttu-id="9ce1a-252">Detta händer snabbt:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-252">This happens quickly:</span></span>

```azurecli
azure group create myResourceGroup westus
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

<span data-ttu-id="9ce1a-253">Nu toocreate hello distribution, anropet `azure group deployment create` och skicka:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-253">Now toocreate hello deployment, call `azure group deployment create` and pass:</span></span>

* <span data-ttu-id="9ce1a-254">hello mallfilen (om du har sparat hello ovan JSON tooa lokala mallfilen).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-254">hello template file (if you saved hello above JSON template tooa local file).</span></span>
* <span data-ttu-id="9ce1a-255">En mall URI (om du vill toopoint på hello-filen i GitHub eller några andra webbadressen).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-255">A template URI (if you want toopoint at hello file in GitHub or some other web address).</span></span>
* <span data-ttu-id="9ce1a-256">hello resursgrupp som du vill toodeploy.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-256">hello resource group into which you want toodeploy.</span></span>
* <span data-ttu-id="9ce1a-257">Ett valfritt distributionsnamn.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-257">An optional deployment name.</span></span>

<span data-ttu-id="9ce1a-258">Du kommer att tillfrågas toosupply hello värdena för parametrarna i hello ”parametrar” avsnittet i hello JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-258">You will be prompted toosupply hello values of parameters in hello "parameters" section of hello JSON file.</span></span> <span data-ttu-id="9ce1a-259">När du har angett alla hello parametervärden börjar din distribution.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-259">When you have specified all hello parameter values, your deployment will begin.</span></span>

<span data-ttu-id="9ce1a-260">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-260">Here is an example:</span></span>

```azurecli
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json myResourceGroup firstDeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
newStorageAccountName: storageaccount
adminUsername: ops
adminPassword: password
dnsNameForPublicIP: newdomainname
```

<span data-ttu-id="9ce1a-261">Du får hello efter typ av information:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-261">You will receive hello following type of information:</span></span>

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "firstDeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment toocomplete
data:    DeploymentName     : firstDeployment
data:    ResourceGroupName  : myResourceGroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T07:53:55.1828878Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  -------------
data:    newStorageAccountName  String        storageaccount
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameForPublicIP     String        newdomainname
data:    ubuntuOSVersion        String        14.10
info:    group deployment create command OK
```


## <span data-ttu-id="9ce1a-262"><a id="create-a-custom-vm-image"></a>Uppgift: Skapa en anpassad avbildning av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9ce1a-262"><a id="create-a-custom-vm-image"></a>Task: Create a custom VM image</span></span>
<span data-ttu-id="9ce1a-263">Du har sett hello grundläggande användningen av mallar som ovan, så vi kan nu använda liknande instruktioner toocreate en egen virtuell dator från en viss VHD-fil i Azure med hjälp av en mall via hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-263">You've seen hello basic usage of templates above, so now we can use similar instructions toocreate a custom VM from a specific .vhd file in Azure by using a template via hello Azure CLI.</span></span> <span data-ttu-id="9ce1a-264">här hello skillnaden är att den här mallen skapar en virtuell dator från en angiven virtuell hårddisk (VHD).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-264">hello difference here is that this template creates a single virtual machine from a specified virtual hard disk (VHD).</span></span>

### <a name="step-1-examine-hello-json-file-for-hello-template"></a><span data-ttu-id="9ce1a-265">Steg 1: Granska hello JSON-fil för hello mall</span><span class="sxs-lookup"><span data-stu-id="9ce1a-265">Step 1: Examine hello JSON file for hello template</span></span>
<span data-ttu-id="9ce1a-266">Här följer hello innehållet i hello JSON-fil för hello-mall som det här avsnittet används som exempel.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-266">Here are hello contents of hello JSON file for hello template that this section uses as an example.</span></span> <span data-ttu-id="9ce1a-267">(hello mall finns också i [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).)</span><span class="sxs-lookup"><span data-stu-id="9ce1a-267">(hello template is also located in [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).)</span></span>

<span data-ttu-id="9ce1a-268">Igen, måste toofind hello värden tooenter för hello-parametrar som inte har standardvärden.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-268">Again, you will need toofind hello values you want tooenter for hello parameters that do not have default values.</span></span> <span data-ttu-id="9ce1a-269">När du kör hello `azure group deployment create` kommandot hello Azure CLI uppmanas du tooenter dessa värden.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-269">When you run hello `azure group deployment create` command, hello Azure CLI will prompt you tooenter those values.</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters" : {
        "userImageStorageAccountName": {
            "type": "string",
            "defaultValue" : "userImageStorageAccountName"
        },
        "userImageStorageContainerName" : {
            "type" : "string",
            "defaultValue" : "userImageStorageContainerName"
        },
        "userImageVhdName" : {
            "type" : "string",
            "defaultValue" : "userImageVhdName"
        },
        "dnsNameForPublicIP" : {
            "type" : "string",
            "defaultValue": "uniqueDnsNameForPublicIP"
        },
        "adminUserName": {
            "type": "string"
        },
        "adminPassword": {
            "type": "securestring"
        },
        "osType" : {
            "type" : "string",
            "allowedValues" : [
                "windows",
                "linux"
            ]
        },
        "subscriptionId":{
            "type" : "string"
        },
        "location": {
            "type": "String",
            "defaultValue" : "West US"
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A2"
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue" : "myPublicIP"
        },
        "vmName": {
            "type": "string",
            "defaultValue" : "myVM"
        },
        "virtualNetworkName":{
            "type" : "string",
            "defaultValue" : "myVNET"
        },
        "nicName":{
            "type" : "string",
            "defaultValue":"myNIC"
        }
    },
    "variables": {
        "addressPrefix":"10.0.0.0/16",
        "subnet1Name": "Subnet-1",
        "subnet2Name": "Subnet-2",
        "subnet1Prefix" : "10.0.0.0/24",
        "subnet2Prefix" : "10.0.1.0/24",
        "publicIPAddressType" : "Dynamic",
        "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
        "subnet1Ref" : "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
        "userImageName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('userImageVhdName'))]",
        "osDiskVhdName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
    },
    "resources": [
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[parameters('publicIPAddressName')]",
        "location": "[parameters('location')]",
        "properties": {
            "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
            }
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/virtualNetworks",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "properties": {
        "addressSpace": {
            "addressPrefixes": [
            "[variables('addressPrefix')]"
            ]
        },
        "subnets": [
            {
            "name": "[variables('subnet1Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet1Prefix')]"
            }
            },
            {
            "name": "[variables('subnet2Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet2Prefix')]"
            }
            }
        ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[parameters('nicName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
            "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
        ],
        "properties": {
            "ipConfigurations": [
            {
                "name": "ipconfig1",
                "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                        "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                    },
                    "subnet": {
                        "id": "[variables('subnet1Ref')]"
                    }
                }
            }
            ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[parameters('vmName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
        ],
        "properties": {
            "hardwareProfile": {
                "vmSize": "[parameters('vmSize')]"
            },
            "osProfile": {
                "computername": "[parameters('vmName')]",
                "adminUsername": "[parameters('adminUsername')]",
                "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
                "osDisk" : {
                    "name" : "[concat(parameters('vmName'),'-osDisk')]",
                    "osType" : "[parameters('osType')]",
                    "caching" : "ReadWrite",
                    "image" : {
                        "uri" : "[variables('userImageName')]"
                    },
                    "vhd" : {
                        "uri" : "[variables('osDiskVhdName')]"
                    }
                }
            },
            "networkProfile": {
                "networkInterfaces" : [
                {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                }
                ]
            }
        }
    }
    ]
}
```

### <a name="step-2-obtain-hello-vhd"></a><span data-ttu-id="9ce1a-270">Steg 2: Hämta hello VHD</span><span class="sxs-lookup"><span data-stu-id="9ce1a-270">Step 2: Obtain hello VHD</span></span>
<span data-ttu-id="9ce1a-271">Du behöver givetvis ha en VHD-fil för detta.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-271">Obviously, you'll need a .vhd for this.</span></span> <span data-ttu-id="9ce1a-272">Du kan använda en som du redan har i Azure eller också kan du ladda upp en.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-272">You can use one you already have in Azure, or you can upload one.</span></span>

<span data-ttu-id="9ce1a-273">En Windows-baserad virtuell dator, se [skapa och ladda upp en Windows Server VHD-tooAzure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-273">For a Windows-based virtual machine, see [Create and upload a Windows Server VHD tooAzure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="9ce1a-274">En Linux-baserade virtuella datorer, se [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-274">For a Linux-based virtual machine, see [Creating and uploading a virtual hard disk that contains hello Linux operating system](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

### <a name="step-3-create-hello-virtual-machine-by-using-hello-template"></a><span data-ttu-id="9ce1a-275">Steg 3: Skapa hello virtuell dator med hjälp av hello mall</span><span class="sxs-lookup"><span data-stu-id="9ce1a-275">Step 3: Create hello virtual machine by using hello template</span></span>
<span data-ttu-id="9ce1a-276">Nu är du redo toocreate en ny virtuell dator baserat på hello VHD.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-276">Now you're ready toocreate a new virtual machine based on hello .vhd.</span></span> <span data-ttu-id="9ce1a-277">Skapa en grupp toodeploy, med hjälp av `azure group create <location>`:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-277">Create a group toodeploy into, by using `azure group create <location>`:</span></span>

```azurecli
azure group create myResourceGroupUser eastus
info:    Executing command group create
+ Getting resource group myResourceGroupUser
+ Creating resource group myResourceGroupUser
info:    Created resource group myResourceGroupUser
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupUser
data:    Name:                myResourceGroupUser
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

<span data-ttu-id="9ce1a-278">Sedan skapar hello distribution med hjälp av hello `--template-uri` alternativet toocall i direkt hello-mall (eller så kan du använda hello `--template-file` alternativet toouse en fil som du har sparat lokalt).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-278">Then create hello deployment by using hello `--template-uri` option toocall in hello template directly (or you can use hello `--template-file` option toouse a file that you have saved locally).</span></span> <span data-ttu-id="9ce1a-279">Observera att eftersom hello mall har standardvärden som angetts kan du tillfrågas om endast några saker.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-279">Note that because hello template has defaults specified, you are prompted for only a few things.</span></span> <span data-ttu-id="9ce1a-280">Om du distribuerar hello mallen på olika ställen hända att vissa namngivning kollisioner inträffar med standardvärden för hello (särskilt hello DNS-namn du skapar).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-280">If you deploy hello template in different places, you may find that some naming collisions occur with hello default values (particularly hello DNS name you create).</span></span>

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json \
> myResourceGroup \
> customVhdDeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
adminUserName: ops
adminPassword: password
osType: linux
subscriptionId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

<span data-ttu-id="9ce1a-281">Utdata ser ut ungefär så hello följande:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-281">Output looks something like hello following:</span></span>

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "customVhdDeployment"
+ Registering providers
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment toocomplete
error:   Deployment provisioning state was not successful
data:    DeploymentName     : customVhdDeployment
data:    ResourceGroupName  : myResourceGroupUser
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T14:55:48.0963829Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                           Type          Value
data:    -----------------------------  ------------  ------------------------------------
data:    userImageStorageAccountName    String        userImageStorageAccountName
data:    userImageStorageContainerName  String        userImageStorageContainerName
data:    userImageVhdName               String        userImageVhdName
data:    dnsNameForPublicIP             String        uniqueDnsNameForPublicIP
data:    adminUserName                  String        ops
data:    adminPassword                  SecureString  undefined
data:    osType                         String        linux
data:    subscriptionId                 String        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
data:    location                       String        West US
data:    vmSize                         String        Standard_A2
data:    publicIPAddressName            String        myPublicIP
data:    vmName                         String        myVM
data:    virtualNetworkName             String        myVNET
data:    nicName                        String        myNIC
info:    group deployment create command OK
```

## <span data-ttu-id="9ce1a-282"><a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Uppgift: Distribuera ett VM-multiprogram som använder ett virtuellt nätverk och en extern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="9ce1a-282"><a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Task: Deploy a multi-VM application that uses a virtual network and an external load balancer</span></span>
<span data-ttu-id="9ce1a-283">Den här mallen kan du toocreate två virtuella datorer under en belastningsutjämnare och konfigurera en regel för belastningsutjämning på Port 80.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-283">This template allows you toocreate two virtual machines under a load balancer and configure a load-balancing rule on Port 80.</span></span> <span data-ttu-id="9ce1a-284">Den här mallen distribuerar också ett lagringskonto, ett virtuellt nätverk, en offentlig IP-adress, en tillgänglighetsuppsättning och nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-284">This template also deploys a storage account, virtual network, public IP address, availability set, and network interfaces.</span></span>

![](./media/virtual-machines-common-cli-deploy-templates/multivmextlb.png)

<span data-ttu-id="9ce1a-285">Följ dessa steg toodeploy en multi-VM-program som använder ett virtuellt nätverk och en belastningsutjämnare med hjälp av en Resource Manager-mall i hello GitHub mall databasen via Azure PowerShell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-285">Follow these steps toodeploy a multi-VM application that uses a virtual network and a load balancer by using a Resource Manager template in hello GitHub template repository via Azure PowerShell commands.</span></span>

### <a name="step-1-examine-hello-json-file-for-hello-template"></a><span data-ttu-id="9ce1a-286">Steg 1: Granska hello JSON-fil för hello mall</span><span class="sxs-lookup"><span data-stu-id="9ce1a-286">Step 1: Examine hello JSON file for hello template</span></span>
<span data-ttu-id="9ce1a-287">Här följer hello innehållet i hello JSON-fil för hello mall.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-287">Here are hello contents of hello JSON file for hello template.</span></span> <span data-ttu-id="9ce1a-288">Om du vill att hello senaste versionen, det finns [på hello GitHub-lagringsplatsen för mallar](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-288">If you want hello most recent version, it's located [at hello GitHub repository for templates](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json).</span></span> <span data-ttu-id="9ce1a-289">Det här avsnittet använder hello `--template-uri` växel toocall i hello mallen, men du kan också använda hello `--template-file` växla toopass en lokal version.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-289">This topic uses hello `--template-uri` switch toocall in hello template, but you can also use hello `--template-file` switch toopass a local version.</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of resources"
            }
        },
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "Name of storage account"
            }
        },
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "Admin user name"
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Admin password"
            }
        },
        "dnsNameforLBIP": {
            "type": "string",
            "metadata": {
                "description": "DNS for load balancer IP"
            }
        },
        "backendPort": {
            "type": "int",
            "defaultValue": 3389,
            "metadata": {
                "description": "Back-end port"
            }
        },
        "vmNamePrefix": {
            "type": "string",
            "defaultValue": "myVM",
            "metadata": {
                "description": "Prefix toouse for VM names"
            }
        },
        "vmSourceImageName": {
            "type": "string",
            "defaultValue": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201412.01-en.us-127GB.vhd"
        },
        "lbName": {
            "type": "string",
            "defaultValue": "myLB",
            "metadata": {
                "description": "Load balancer name"
            }
        },
        "nicNamePrefix": {
            "type": "string",
            "defaultValue": "nic",
            "metadata": {
                "description": "Network interface name prefix"
            }
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue": "myPublicIP",
            "metadata": {
                "description": "Public IP name"
            }
        },
        "vnetName": {
            "type": "string",
            "defaultValue": "myVNET",
            "metadata": {
                "description": "Virtual network name"
            }
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A1",
            "metadata": {
                "description": "Size of hello VM"
            }
        }
    },
    "variables": {
        "storageAccountType": "Standard_LRS",
        "vmStorageAccountContainerName": "vhds",
        "availabilitySetName": "myAvSet",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "Subnet-1",
        "subnetPrefix": "10.0.0.0/24",
        "publicIPAddressType": "Dynamic",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
        "numberOfInstances": 2,
        "nicId1": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 0))]",
        "nicId2": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 1))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LBFE')]",
        "backEndIPConfigID1": "[concat(variables('nicId1'),'/ipConfigurations/ipconfig1')]",
        "backEndIPConfigID2": "[concat(variables('nicId2'),'/ipConfigurations/ipconfig1')]",
        "sourceImageName": "[concat('/', subscription().subscriptionId,'/services/images/',parameters('vmSourceImageName'))]",
        "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/LBBE')]",
        "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": { }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('dnsNameforLBIP')]"
                }
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('vnetName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnetName')]",
                        "properties": {
                            "addressPrefix": "[variables('subnetPrefix')]"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/networkInterfaces",
            "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
            "location": "[parameters('location')]",
            "copy": {
                "name": "nicLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "subnet": {
                                "id": "[variables('subnetRef')]"
                            }
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/backendAddressPools/LBBE')]"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/inboundNatRule/RDP-VM', copyindex())]"
                            }
                        ]


                    }
                ]

            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "name": "[parameters('lbName')]",
            "type": "Microsoft.Network/loadBalancers",
            "location": "[parameters('location')]",
            "dependsOn": [
                "nicLoop",
                "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
            ],
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "LBFE",
                        "properties": {
                            "publicIPAddress": {
                                "id": "[variables('publicIPAddressID')]"
                            }
                        }
                    }
                ],
                "backendAddressPools": [
                    {
                        "name": "LBBE"

                    }
                ],
                "inboundNatRules": [
                    {
                        "name": "RDP-VM1",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50001,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    },
                    {
                        "name": "RDP-VM2",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50002,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "LBRule",
                        "properties": {
                            "frontendIPConfiguration": {
                                "id": "[variables('frontEndIPConfigID')]"
                            },
                            "backendAddressPool": {
                                "id": "[variables('lbPoolID')]"
                            },
                            "protocol": "tcp",
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 5,
                            "probe": {
                                "id": "[variables('lbProbeID')]"
                            }
                        }
                    }
                ],
                "probes": [
                    {
                        "name": "tcpProbe",
                        "properties": {
                            "protocol": "tcp",
                            "port": 80,
                            "intervalInSeconds": "5",
                            "numberOfProbes": "2"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
            "copy": {
                "name": "virtualMachineLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
                "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
                "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
            ],
            "properties": {
                "availabilitySet": {
                    "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
                },
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computername": "[concat(parameters('vmNamePrefix'), copyIndex())]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "sourceImage": {
                        "id": "[variables('sourceImageName')]"
                    },
                    "destinationVhdsContainer": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/')]"
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="step-2-create-hello-deployment-by-using-hello-template"></a><span data-ttu-id="9ce1a-290">Steg 2: Skapa hello distribution med hjälp av hello mall</span><span class="sxs-lookup"><span data-stu-id="9ce1a-290">Step 2: Create hello deployment by using hello template</span></span>
<span data-ttu-id="9ce1a-291">Skapa en resursgrupp för hello mallen med hjälp av `azure group create <location>`.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-291">Create a resource group for hello template by using `azure group create <location>`.</span></span> <span data-ttu-id="9ce1a-292">Skapa sedan en distribution i den resursgruppen med hjälp av `azure group deployment create` och skicka hello resursgrupp, skickar du ett distributionsnamn och svara på hello anvisningarna för parametrar i hello-mall som inte har standardvärden.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-292">Then, create a deployment into that resource group by using `azure group deployment create` and passing hello resource group, passing a deployment name, and answering hello prompts for parameters in hello template that did not have default values.</span></span>

```azurecli
azure group create lbgroup westus
info:    Executing command group create
+ Getting resource group lbgroup
+ Creating resource group lbgroup
info:    Created resource group lbgroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/lbgroup
data:    Name:                lbgroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

<span data-ttu-id="9ce1a-293">Nu använda hello `azure group deployment create` kommandot och hello `--template-uri` alternativet toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-293">Now use hello `azure group deployment create` command and hello `--template-uri` option toodeploy hello template.</span></span> <span data-ttu-id="9ce1a-294">Vara beredd med dina parametervärden när du uppmanas till det, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-294">Be ready with your parameter values when it prompts you, as shown below.</span></span>

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json \
> lbgroup \
> newdeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
location: westus
newStorageAccountName: storagename
adminUsername: ops
adminPassword: password
dnsNameforLBIP: lbdomainname
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "newdeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.compute
info:    Registering provider microsoft.network
+ Waiting for deployment toocomplete
data:    DeploymentName     : newdeployment
data:    ResourceGroupName  : lbgroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T20:58:40.1678876Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  ----------------------
data:    location               String        westus
data:    newStorageAccountName  String        storagename
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameforLBIP         String        lbdomainname
data:    backendPort            Int           3389
data:    vmNamePrefix           String        myVM
data:    imagePublisher         String        MicrosoftWindowsServer
data:    imageOffer             String        WindowsServer
data:    imageSKU               String        2012-R2-Datacenter
data:    lbName                 String        myLB
data:    nicNamePrefix          String        nic
data:    publicIPAddressName    String        myPublicIP
data:    vnetName               String        myVNET
data:    vmSize                 String        Standard_A1
info:    group deployment create command OK
```

<span data-ttu-id="9ce1a-295">Observera att den här mallen distribuerar en Windows Server-avbildning. Den kan dock enkelt ersättas av en Linux-avbildning.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-295">Note that this template deploys a Windows Server image; however, it could easily be replaced by any Linux image.</span></span> <span data-ttu-id="9ce1a-296">Vill toocreate ett Docker-kluster med flera swarm cheferna?</span><span class="sxs-lookup"><span data-stu-id="9ce1a-296">Want toocreate a Docker cluster with multiple swarm managers?</span></span> <span data-ttu-id="9ce1a-297">[Det kan du göra](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-297">[You can do it](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).</span></span>

## <span data-ttu-id="9ce1a-298"><a id="remove-a-resource-group"></a>Uppgift: Ta bort en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="9ce1a-298"><a id="remove-a-resource-group"></a>Task: Remove a resource group</span></span>
<span data-ttu-id="9ce1a-299">Kom ihåg att du kan distribuera tooa resursgrupp, men om du är klar med en kan du ta bort den med hjälp av `azure group delete <group name>`.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-299">Remember that you can redeploy tooa resource group, but if you are done with one, you can delete it by using `azure group delete <group name>`.</span></span>

```azurecli
azure group delete myResourceGroup
info:    Executing command group delete
Delete resource group myResourceGroup? [y/n] y
+ Deleting resource group myResourceGroup
info:    group delete command OK
```

## <span data-ttu-id="9ce1a-300"><a id="show-the-log-for-a-resource-group-deployment"></a>Uppgift: Visa hello logg för en resurs för gruppdistributionen</span><span class="sxs-lookup"><span data-stu-id="9ce1a-300"><a id="show-the-log-for-a-resource-group-deployment"></a>Task: Show hello log for a resource group deployment</span></span>
<span data-ttu-id="9ce1a-301">Det här är vanligt när man skapar eller använder mallar.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-301">This one is common while you're creating or using templates.</span></span> <span data-ttu-id="9ce1a-302">hello anropet toodisplay hello-distributionsloggar för en grupp är `azure group log show <groupname>`, som visar lite information som är användbar för att förstå varför något hände--inte eller.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-302">hello call toodisplay hello deployment logs for a group is `azure group log show <groupname>`, which displays quite a bit of information that's useful for understanding why something happened -- or didn't.</span></span> <span data-ttu-id="9ce1a-303">(Mer information om hur du felsöker dina distributioner, samt annan information om problem finns i avsnittet [Troubleshoot common Azure deployment errors with Azure Resource Manager (Felsöka vanliga Azure-distributionsfel med Azure Resource Manager)](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md).)</span><span class="sxs-lookup"><span data-stu-id="9ce1a-303">(For more information on troubleshooting your deployments, as well as other information about issues, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md).)</span></span>

<span data-ttu-id="9ce1a-304">tootarget specifika problem, till exempel kan du använda verktyg som **jq** tooquery saker lite mer exakt, till exempel vilka enskilda fel måste toocorrect.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-304">tootarget specific failures, for example, you might use tools like **jq** tooquery things a bit more precisely, such as which individual failures you need toocorrect.</span></span> <span data-ttu-id="9ce1a-305">hello följande exempel används **jq** tooparse en distribution i felloggen **lbgroup**, som söker efter fel.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-305">hello following example uses **jq** tooparse a deployment log for **lbgroup**, looking for failures.</span></span>

```azurecli
azure group log show lbgroup -l --json | jq '.[] | select(.status.value == "Failed") | .properties'
```
<span data-ttu-id="9ce1a-306">Du kan snabbt identifiera vad som har gått fel, åtgärda felet och försöka igen.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-306">You can discover very quickly what went wrong, fix, and retry.</span></span> <span data-ttu-id="9ce1a-307">I följande fall hello, hello mallen hade skapar två virtuella datorer på hello samtidigt som skapas ett lås på hello VHD.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-307">In hello following case, hello template had been creating two VMs at hello same time, which created a lock on hello .vhd.</span></span> <span data-ttu-id="9ce1a-308">(När vi har ändrats hello mallen hello distributionen lyckades snabbt.)</span><span class="sxs-lookup"><span data-stu-id="9ce1a-308">(After we modified hello template, hello deployment succeeded quickly.)</span></span>

```json
{
    "statusCode": "Conflict",
    "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"hello resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"AcquireDiskLeaseFailed\",\"message\":\"Failed tooacquire lease while creating disk 'osdisk' using blob with URI http://storage.blob.core.windows.net/vhds/osdisk.vhd.\"}]}}"
}
```

## <span data-ttu-id="9ce1a-309"><a id="display-information-about-a-virtual-machine"></a>Uppgift: Visa information om en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9ce1a-309"><a id="display-information-about-a-virtual-machine"></a>Task: Display information about a virtual machine</span></span>
<span data-ttu-id="9ce1a-310">Du kan se information om specifika virtuella datorer i resursgruppen med hjälp av hello `azure vm show <groupname> <vmname>` kommando.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-310">You can see information about specific VMs in your resource group by using hello `azure vm show <groupname> <vmname>` command.</span></span> <span data-ttu-id="9ce1a-311">Om du har mer än en virtuell dator i din grupp, måste du kanske först toolist hello virtuella datorer i en grupp med hjälp av `azure vm list <groupname>`.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-311">If you have more than one VM in your group, you might first need toolist hello VMs in a group by using `azure vm list <groupname>`.</span></span>

```azurecli
azure vm list zoo
info:    Executing command vm list
+ Getting virtual machines
data:    Name   ProvisioningState  Location  Size
data:    -----  -----------------  --------  -----------
data:    myVM0  Succeeded          westus    Standard_A1
data:    myVM1  Failed             westus    Standard_A1
```

<span data-ttu-id="9ce1a-312">Och sedan söka efter myVM1:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-312">And then, looking up myVM1:</span></span>

```azurecli
azure vm show zoo myVM1
info:    Executing command vm show
+ Looking up hello VM "myVM1"
+ Looking up hello NIC "nic1"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Failed
data:    Name                            :myVM1
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :MicrosoftWindowsServer
data:        Offer                       :WindowsServer
data:        Sku                         :2012-R2-Datacenter
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Windows
data:        Name                        :osdisk
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :http://zoostorageralph.blob.core.windows.net/vhds/osdisk.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Windows Configuration:
data:        Provision VM Agent          :true
data:        Enable automatic updates    :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Network/networkInterfaces/nic1
data:          Primary                   :false
data:          Provisioning State        :Succeeded
data:          Name                      :nic1
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.0.5
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/availabilitySets/MYAVSET
info:    vm show command OK
```

> [!NOTE]
> <span data-ttu-id="9ce1a-313">Om du vill tooprogrammatically store och manipulera hello utdata från konsolkommandon du toouse parsning av JSON-verktyget som  **[jq](https://github.com/stedolan/jq)**  eller  **[jsawk](https://github.com/micha/jsawk)** , eller språk bibliotek som är bra för hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-313">If you want tooprogrammatically store and manipulate hello output of your console commands, you may want toouse a JSON parsing tool such as **[jq](https://github.com/stedolan/jq)** or **[jsawk](https://github.com/micha/jsawk)**, or language libraries that are good for hello task.</span></span>
>
>

## <span data-ttu-id="9ce1a-314"><a id="log-on-to-a-linux-based-virtual-machine"></a>Uppgift: Logga in tooa Linux-baserade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="9ce1a-314"><a id="log-on-to-a-linux-based-virtual-machine"></a>Task: Log on tooa Linux-based virtual machine</span></span>
<span data-ttu-id="9ce1a-315">Linux-datorer är vanligtvis anslutna toothrough SSH.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-315">Typically Linux machines are connected toothrough SSH.</span></span> <span data-ttu-id="9ce1a-316">Mer information finns i [hur toouse SSH med Linux på Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-316">For more information, see [How toouse SSH with Linux on Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <span data-ttu-id="9ce1a-317"><a id="stop-a-virtual-machine"></a>Uppgift: Stoppa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9ce1a-317"><a id="stop-a-virtual-machine"></a>Task: Stop a VM</span></span>
<span data-ttu-id="9ce1a-318">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-318">Run this command:</span></span>

```azurecli
azure vm stop <group name> <virtual machine name>
```

> [!IMPORTANT]
> <span data-ttu-id="9ce1a-319">Använd den här parametern tookeep hello virtuella IP-Adressen (VIP) för hello vnet om det är hello sista virtuella datorn i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-319">Use this parameter tookeep hello virtual IP (VIP) of hello vnet in case it's hello last VM in that vnet.</span></span> <br><br> <span data-ttu-id="9ce1a-320">Om du använder hello `StayProvisioned` parametern du faktureras fortfarande för hello VM.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-320">If you use hello `StayProvisioned` parameter, you'll still be billed for hello VM.</span></span>
>
>

## <span data-ttu-id="9ce1a-321"><a id="start-a-virtual-machine"></a>Uppgift: Starta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9ce1a-321"><a id="start-a-virtual-machine"></a>Task: Start a VM</span></span>
<span data-ttu-id="9ce1a-322">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-322">Run this command:</span></span>

```azurecli
azure vm start <group name> <virtual machine name>
```

## <span data-ttu-id="9ce1a-323"><a id="attach-a-data-disk"></a>Uppgift: Anslut en datadisk</span><span class="sxs-lookup"><span data-stu-id="9ce1a-323"><a id="attach-a-data-disk"></a>Task: Attach a data disk</span></span>
<span data-ttu-id="9ce1a-324">Du måste också toodecide om tooattach en ny disk eller en som innehåller data.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-324">You'll also need toodecide whether tooattach a new disk or one that contains data.</span></span> <span data-ttu-id="9ce1a-325">För en ny disk hello skapar hello VHD-filen och bifogar i hello samma kommando.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-325">For a new disk, hello command creates hello .vhd file and attaches it in hello same command.</span></span>

<span data-ttu-id="9ce1a-326">tooattach kör det här kommandot för en ny disk:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-326">tooattach a new disk, run this command:</span></span>

```azurecli
    azure vm disk attach-new <resource-group> <vm-name> <size-in-gb>
```

<span data-ttu-id="9ce1a-327">tooattach en befintlig datadisk kör detta kommando:</span><span class="sxs-lookup"><span data-stu-id="9ce1a-327">tooattach an existing data disk, run this command:</span></span>

```azurecli
azure vm disk attach <resource-group> <vm-name> [vhd-url]
```

<span data-ttu-id="9ce1a-328">Sedan måste toomount hello disk som vanligt i Linux.</span><span class="sxs-lookup"><span data-stu-id="9ce1a-328">Then you'll need toomount hello disk, as you normally would in Linux.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ce1a-329">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9ce1a-329">Next steps</span></span>
<span data-ttu-id="9ce1a-330">Långt fler exempel på användning av Azure CLI med hello **arm** läge, se [Using hello Azure CLI för Mac, Linux och Windows med Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-330">For far more examples of Azure CLI usage with hello **arm** mode, see [Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md).</span></span> <span data-ttu-id="9ce1a-331">toolearn mer om Azure-resurser och deras begrepp finns [översikt över Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-331">toolearn more about Azure resources and their concepts, see [Azure Resource Manager overview](../articles/azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="9ce1a-332">Fler mallar som du kan använda finns i [Azure Quickstart-mallar](https://azure.microsoft.com/documentation/templates/) och [Programramverk med hjälp av mallar](../articles/virtual-machines/linux/app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9ce1a-332">For more templates you can use, see [Azure Quickstart templates](https://azure.microsoft.com/documentation/templates/) and [Application frameworks using templates](../articles/virtual-machines/linux/app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
