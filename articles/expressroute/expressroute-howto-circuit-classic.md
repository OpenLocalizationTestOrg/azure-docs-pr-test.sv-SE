---
title: "Skapa och ändra en ExpressRoute-krets: PowerShell: klassiska Azure-portalen | Microsoft Docs"
description: "Den här artikeln vägleder dig genom hello steg för att skapa och etablera en ExpressRoute-krets. Den här artikeln beskriver också hur toocheck hello status, uppdatera, eller ta bort och ta bort etableringen kretsen."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0134d242-6459-4dec-a2f1-4657c3bc8b23
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 9897c88776a2153ba22aa9ff328becb9f12b660b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a><span data-ttu-id="7747a-104">Skapa och ändra en ExpressRoute-krets med hjälp av PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="7747a-104">Create and modify an ExpressRoute circuit using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7747a-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7747a-105">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="7747a-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7747a-106">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="7747a-107">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7747a-107">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="7747a-108">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7747a-108">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="7747a-109">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="7747a-109">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="7747a-110">Den här artikeln vägleder dig genom hello steg toocreate Azure ExpressRoute-kretsen med hjälp av PowerShell-cmdlets och hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="7747a-110">This article walks you through hello steps toocreate an Azure ExpressRoute circuit by using PowerShell cmdlets and hello classic deployment model.</span></span> <span data-ttu-id="7747a-111">Den här artikeln beskriver också hur toocheck hello status, uppdatera, eller ta bort och ta bort etableringen av en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="7747a-111">This article also shows you how toocheck hello status, update, or delete and deprovision an ExpressRoute circuit.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


<span data-ttu-id="7747a-112">**Om distributionsmodeller för Azure**</span><span class="sxs-lookup"><span data-stu-id="7747a-112">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a><span data-ttu-id="7747a-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="7747a-113">Before you begin</span></span>
### <a name="step-1-review-hello-prerequisites-and-workflow-articles"></a><span data-ttu-id="7747a-114">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="7747a-114">Step 1.</span></span> <span data-ttu-id="7747a-115">Granska hello krav och arbetsflöde artiklar</span><span class="sxs-lookup"><span data-stu-id="7747a-115">Review hello prerequisites and workflow articles</span></span>
<span data-ttu-id="7747a-116">Kontrollera att du har granskat hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="7747a-116">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>  

### <a name="step-2-install-hello-latest-versions-of-hello-azure-service-management-sm-powershell-modules"></a><span data-ttu-id="7747a-117">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="7747a-117">Step 2.</span></span> <span data-ttu-id="7747a-118">Installera hello senaste versionerna av hello Azure Service Management (SM) PowerShell-moduler</span><span class="sxs-lookup"><span data-stu-id="7747a-118">Install hello latest versions of hello Azure Service Management (SM) PowerShell modules</span></span>
<span data-ttu-id="7747a-119">Följ anvisningarna för hello i [komma igång med Azure PowerShell-cmdlets](/powershell/azure/overview) stegvisa instruktioner om hur tooconfigure din dator toouse hello Azure PowerShell-moduler.</span><span class="sxs-lookup"><span data-stu-id="7747a-119">Follow hello instructions in [Getting started with Azure PowerShell cmdlets](/powershell/azure/overview) for step-by-step guidance on how tooconfigure your computer toouse hello Azure PowerShell modules.</span></span>

### <a name="step-3-log-in-tooyour-azure-account-and-select-a-subscription"></a><span data-ttu-id="7747a-120">Steg 3.</span><span class="sxs-lookup"><span data-stu-id="7747a-120">Step 3.</span></span> <span data-ttu-id="7747a-121">Logga in tooyour Azure-konto och välj en prenumeration</span><span class="sxs-lookup"><span data-stu-id="7747a-121">Log in tooyour Azure account and select a subscription</span></span>
1. <span data-ttu-id="7747a-122">Öppna PowerShell-konsol med utökade behörigheter och Anslut tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="7747a-122">Open your PowerShell console with elevated rights and connect tooyour account.</span></span> <span data-ttu-id="7747a-123">Använd hello följande exempel toohelp du ansluta:</span><span class="sxs-lookup"><span data-stu-id="7747a-123">Use hello following example toohelp you connect:</span></span>

        Login-AzureRmAccount

2. <span data-ttu-id="7747a-124">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="7747a-124">Check hello subscriptions for hello account.</span></span>

        Get-AzureRmSubscription

3. <span data-ttu-id="7747a-125">Om du har mer än en prenumeration väljer du hello prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="7747a-125">If you have more than one subscription, select hello subscription that you want toouse.</span></span>

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. <span data-ttu-id="7747a-126">Använd sedan följande cmdlet tooadd hello tooPowerShell din Azure-prenumeration för hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="7747a-126">Next, use hello following cmdlet tooadd your Azure subscription tooPowerShell for hello classic deployment model.</span></span>

        Add-AzureAccount

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="7747a-127">Skapa och etablera en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="7747a-127">Create and provision an ExpressRoute circuit</span></span>
### <a name="step-1-import-hello-powershell-modules-for-expressroute"></a><span data-ttu-id="7747a-128">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="7747a-128">Step 1.</span></span> <span data-ttu-id="7747a-129">Importera hello PowerShell-moduler för ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="7747a-129">Import hello PowerShell modules for ExpressRoute</span></span>
 <span data-ttu-id="7747a-130">Om du inte redan har gjort det, måste du importera hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen i ordning toostart med hello ExpressRoute-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="7747a-130">If you have not already done so, you must import hello Azure and ExpressRoute modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="7747a-131">Du importerar hello moduler från hello plats som de var installerat tooon den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="7747a-131">You import hello modules from hello location that they were installed tooon your local computer.</span></span> <span data-ttu-id="7747a-132">Beroende på hello metod du använde tooinstall hello moduler, hello plats kan skilja sig hello som följande exempel visar.</span><span class="sxs-lookup"><span data-stu-id="7747a-132">Depending on hello method you used tooinstall hello modules, hello location may be different than hello following example shows.</span></span> <span data-ttu-id="7747a-133">Ändra hello exempel om det behövs.</span><span class="sxs-lookup"><span data-stu-id="7747a-133">Modify hello example if necessary.</span></span>  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="7747a-134">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="7747a-134">Step 2.</span></span> <span data-ttu-id="7747a-135">Hämta hello lista över providers som stöds, platser och bandbredder</span><span class="sxs-lookup"><span data-stu-id="7747a-135">Get hello list of supported providers, locations, and bandwidths</span></span>
<span data-ttu-id="7747a-136">Innan du skapar en ExpressRoute-krets måste hello lista över providers stöds anslutning, platser och alternativ för bandbredd.</span><span class="sxs-lookup"><span data-stu-id="7747a-136">Before you create an ExpressRoute circuit, you need hello list of supported connectivity providers, locations, and bandwidth options.</span></span>

<span data-ttu-id="7747a-137">Hej PowerShell-cmdleten `Get-AzureDedicatedCircuitServiceProvider` returnerar informationen som du ska använda i senare steg:</span><span class="sxs-lookup"><span data-stu-id="7747a-137">hello PowerShell cmdlet `Get-AzureDedicatedCircuitServiceProvider` returns this information, which you’ll use in later steps:</span></span>

    Get-AzureDedicatedCircuitServiceProvider

<span data-ttu-id="7747a-138">Kontrollera toosee om anslutningsleverantören finns med i listan.</span><span class="sxs-lookup"><span data-stu-id="7747a-138">Check toosee if your connectivity provider is listed there.</span></span> <span data-ttu-id="7747a-139">Anteckna hello följande information eftersom du behöver det senare när du skapar en krets:</span><span class="sxs-lookup"><span data-stu-id="7747a-139">Make a note of hello following information because you'll need it later when you create a circuit:</span></span>

* <span data-ttu-id="7747a-140">Namn</span><span class="sxs-lookup"><span data-stu-id="7747a-140">Name</span></span>
* <span data-ttu-id="7747a-141">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="7747a-141">PeeringLocations</span></span>
* <span data-ttu-id="7747a-142">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="7747a-142">BandwidthsOffered</span></span>

<span data-ttu-id="7747a-143">Nu är du redo toocreate en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="7747a-143">You're now ready toocreate an ExpressRoute circuit.</span></span>         

### <a name="step-3-create-an-expressroute-circuit"></a><span data-ttu-id="7747a-144">Steg 3.</span><span class="sxs-lookup"><span data-stu-id="7747a-144">Step 3.</span></span> <span data-ttu-id="7747a-145">Skapa en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="7747a-145">Create an ExpressRoute circuit</span></span>
<span data-ttu-id="7747a-146">hello som följande exempel visar hur toocreate en 200 Mbit/s-ExpressRoute-krets via Equinix i Silicon dal.</span><span class="sxs-lookup"><span data-stu-id="7747a-146">hello following example shows how toocreate a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="7747a-147">Om du använder en annan provider och andra inställningar, i stället informationen när du gör din begäran.</span><span class="sxs-lookup"><span data-stu-id="7747a-147">If you're using a different provider and different settings, substitute that information when you make your request.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7747a-148">ExpressRoute-krets kommer att debiteras från hello tidpunkt då en nyckel har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="7747a-148">Your ExpressRoute circuit will be billed from hello moment a service key is issued.</span></span> <span data-ttu-id="7747a-149">Se till att utföra den här åtgärden när hello anslutningen providern är klar tooprovision hello krets.</span><span class="sxs-lookup"><span data-stu-id="7747a-149">Ensure that you perform this operation when hello connectivity provider is ready tooprovision hello circuit.</span></span>
> 
> 

<span data-ttu-id="7747a-150">hello följande är ett exempelbegäran om en ny nyckel:</span><span class="sxs-lookup"><span data-stu-id="7747a-150">hello following is an example request for a new service key:</span></span>

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

<span data-ttu-id="7747a-151">Eller, om du vill toocreate en ExpressRoute-krets med hello premium-tillägg, Använd hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="7747a-151">Or, if you want toocreate an ExpressRoute circuit with hello premium add-on, use hello following example.</span></span> <span data-ttu-id="7747a-152">Se toohello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md) för mer information om hello premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="7747a-152">Refer toohello [ExpressRoute FAQ](expressroute-faqs.md) for more details about hello premium add-on.</span></span>

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


<span data-ttu-id="7747a-153">hello svaret innehåller hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="7747a-153">hello response will contain hello service key.</span></span> <span data-ttu-id="7747a-154">Du kan få en detaljerad beskrivning av alla hello parametrar genom att köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="7747a-154">You can get detailed descriptions of all hello parameters by running hello following:</span></span>

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-hello-expressroute-circuits"></a><span data-ttu-id="7747a-155">Steg 4.</span><span class="sxs-lookup"><span data-stu-id="7747a-155">Step 4.</span></span> <span data-ttu-id="7747a-156">Visa en lista med alla hello ExpressRoute-kretsar</span><span class="sxs-lookup"><span data-stu-id="7747a-156">List all hello ExpressRoute circuits</span></span>
<span data-ttu-id="7747a-157">Du kan köra hello `Get-AzureDedicatedCircuit` kommandot tooget en lista över alla hello ExpressRoute-kretsar som du skapade:</span><span class="sxs-lookup"><span data-stu-id="7747a-157">You can run hello `Get-AzureDedicatedCircuit` command tooget a list of all hello ExpressRoute circuits that you created:</span></span>

    Get-AzureDedicatedCircuit

<span data-ttu-id="7747a-158">hello svaret blir något liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="7747a-158">hello response will be something similar toohello following example:</span></span>

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="7747a-159">Du kan hämta den här informationen när som helst genom att använda hello `Get-AzureDedicatedCircuit` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7747a-159">You can retrieve this information at any time by using hello `Get-AzureDedicatedCircuit` cmdlet.</span></span> <span data-ttu-id="7747a-160">Gör hello anropa utan parametrar visar alla hello kretsar.</span><span class="sxs-lookup"><span data-stu-id="7747a-160">Making hello call without any parameters lists all hello circuits.</span></span> <span data-ttu-id="7747a-161">Nyckeln för tjänsten visas i hello *ServiceKey* fältet.</span><span class="sxs-lookup"><span data-stu-id="7747a-161">Your service key will be listed in hello *ServiceKey* field.</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="7747a-162">Du kan få en detaljerad beskrivning av alla hello parametrar genom att köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="7747a-162">You can get detailed descriptions of all hello parameters by running hello following:</span></span>

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a><span data-ttu-id="7747a-163">Steg 5.</span><span class="sxs-lookup"><span data-stu-id="7747a-163">Step 5.</span></span> <span data-ttu-id="7747a-164">Skicka hello-nyckel tooyour anslutning leverantör för etablering</span><span class="sxs-lookup"><span data-stu-id="7747a-164">Send hello service key tooyour connectivity provider for provisioning</span></span>
<span data-ttu-id="7747a-165">*ServiceProviderProvisioningState* innehåller information om hello aktuell status för etablering på hello-leverantör sida.</span><span class="sxs-lookup"><span data-stu-id="7747a-165">*ServiceProviderProvisioningState* provides information on hello current state of provisioning on hello service-provider side.</span></span> <span data-ttu-id="7747a-166">*Status för* innehåller hello tillstånd om hello Microsoft sida.</span><span class="sxs-lookup"><span data-stu-id="7747a-166">*Status* provides hello state on hello Microsoft side.</span></span> <span data-ttu-id="7747a-167">Mer information om krets etablering tillstånd finns hello [arbetsflöden](expressroute-workflows.md#expressroute-circuit-provisioning-states) artikel.</span><span class="sxs-lookup"><span data-stu-id="7747a-167">For more information about circuit provisioning states, see hello [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="7747a-168">När du skapar en ny ExpressRoute-krets blir hello krets i hello följande tillstånd:</span><span class="sxs-lookup"><span data-stu-id="7747a-168">When you create a new ExpressRoute circuit, hello circuit will be in hello following state:</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="7747a-169">hello krets hamnar toohello efter tillstånd när hello anslutningen leverantören är i hello innebär att aktivera den åt dig:</span><span class="sxs-lookup"><span data-stu-id="7747a-169">hello circuit will go toohello following state when hello connectivity provider is in hello process of enabling it for you:</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

<span data-ttu-id="7747a-170">En ExpressRoute-krets måste vara i följande tillstånd för du toobe kan toouse hello den:</span><span class="sxs-lookup"><span data-stu-id="7747a-170">An ExpressRoute circuit must be in hello following state for you toobe able toouse it:</span></span>

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a><span data-ttu-id="7747a-171">Steg 6.</span><span class="sxs-lookup"><span data-stu-id="7747a-171">Step 6.</span></span> <span data-ttu-id="7747a-172">Kontrollera regelbundet hello status och hello tillståndet för hello krets nyckel</span><span class="sxs-lookup"><span data-stu-id="7747a-172">Periodically check hello status and hello state of hello circuit key</span></span>
<span data-ttu-id="7747a-173">På så sätt får du reda på om din leverantör har aktiverat kretsen.</span><span class="sxs-lookup"><span data-stu-id="7747a-173">This lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="7747a-174">När hello kretsen har konfigurerats, *ServiceProviderProvisioningState* visas som *etablerad* som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="7747a-174">After hello circuit has been configured, *ServiceProviderProvisioningState* will display as *Provisioned* as shown in hello following example:</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a><span data-ttu-id="7747a-175">Steg 7.</span><span class="sxs-lookup"><span data-stu-id="7747a-175">Step 7.</span></span> <span data-ttu-id="7747a-176">Skapa din routningskonfiguration</span><span class="sxs-lookup"><span data-stu-id="7747a-176">Create your routing configuration</span></span>
<span data-ttu-id="7747a-177">Se toohello [ExpressRoute-krets routningskonfiguration (skapa och ändra krets peerkopplingar)](expressroute-howto-routing-classic.md) artikel stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="7747a-177">Refer toohello [ExpressRoute circuit routing configuration (create and modify circuit peerings)](expressroute-howto-routing-classic.md) article for step-by-step instructions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7747a-178">Dessa anvisningar gäller endast toocircuits som skapas med leverantörer som erbjuder layer 2 tjänster.</span><span class="sxs-lookup"><span data-stu-id="7747a-178">These instructions only apply toocircuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="7747a-179">Om du använder en tjänstprovider som erbjuder Hanterade layer 3-tjänster (vanligtvis en IP VPN, t.ex. MPLS), anslutningsleverantören kan konfigurera och hantera routning av du.</span><span class="sxs-lookup"><span data-stu-id="7747a-179">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="step-8-link-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="7747a-180">Steg 8.</span><span class="sxs-lookup"><span data-stu-id="7747a-180">Step 8.</span></span> <span data-ttu-id="7747a-181">Länka ett virtuellt nätverk tooan ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="7747a-181">Link a virtual network tooan ExpressRoute circuit</span></span>
<span data-ttu-id="7747a-182">Därefter länka ett virtuellt nätverk tooyour ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="7747a-182">Next, link a virtual network tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="7747a-183">Se för[länka ExpressRoute-kretsar toovirtual nätverk](expressroute-howto-linkvnet-classic.md) stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="7747a-183">Refer too[Linking ExpressRoute circuits toovirtual networks](expressroute-howto-linkvnet-classic.md) for step-by-step instructions.</span></span> <span data-ttu-id="7747a-184">Om du behöver toocreate ett virtuellt nätverk med hello klassiska distributionsmodellen expressroute finns [skapa ett virtuellt nätverk för ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span><span class="sxs-lookup"><span data-stu-id="7747a-184">If you need toocreate a virtual network using hello classic deployment model for ExpressRoute, see [Create a virtual network for ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span></span>

## <a name="getting-hello-status-of-an-expressroute-circuit"></a><span data-ttu-id="7747a-185">Hämta hello status för en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="7747a-185">Getting hello status of an ExpressRoute circuit</span></span>
<span data-ttu-id="7747a-186">Du kan hämta den här informationen när som helst genom att använda hello `Get-AzureCircuit` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7747a-186">You can retrieve this information at any time by using hello `Get-AzureCircuit` cmdlet.</span></span> <span data-ttu-id="7747a-187">Gör hello anropa utan parametrar visar alla hello kretsar.</span><span class="sxs-lookup"><span data-stu-id="7747a-187">Making hello call without any parameters lists all hello circuits.</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="7747a-188">Du kan hämta information om en specifik ExpressRoute-krets genom att skicka hello nyckel som en parameter toohello anrop.</span><span class="sxs-lookup"><span data-stu-id="7747a-188">You can get information on a specific ExpressRoute circuit by passing hello service key as a parameter toohello call.</span></span>

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


<span data-ttu-id="7747a-189">Du kan få en detaljerad beskrivning av alla hello parametrar genom att köra följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="7747a-189">You can get detailed descriptions of all hello parameters by running hello following example:</span></span>

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a><span data-ttu-id="7747a-190">Ändra en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="7747a-190">Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="7747a-191">Du kan ändra vissa egenskaper för en ExpressRoute-krets utan att påverka anslutningen.</span><span class="sxs-lookup"><span data-stu-id="7747a-191">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="7747a-192">Du kan göra hello följande utan avbrott:</span><span class="sxs-lookup"><span data-stu-id="7747a-192">You can do hello following with no downtime:</span></span>

* <span data-ttu-id="7747a-193">Aktivera eller inaktivera tillägget ExpressRoute premium för ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="7747a-193">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="7747a-194">Öka hello bandbredden för ExpressRoute-krets såvida det inte finns tillgänglig kapacitet på hello port.</span><span class="sxs-lookup"><span data-stu-id="7747a-194">Increase hello bandwidth of your ExpressRoute circuit provided there is capacity available on hello port.</span></span> <span data-ttu-id="7747a-195">Observera att nedgradera hello bandbredden för en krets inte stöds.</span><span class="sxs-lookup"><span data-stu-id="7747a-195">Note that downgrading hello bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="7747a-196">Ändra hello avläsning plan från förbrukade Data tooUnlimited Data.</span><span class="sxs-lookup"><span data-stu-id="7747a-196">Change hello metering plan from Metered Data tooUnlimited Data.</span></span> <span data-ttu-id="7747a-197">Observera att ändra hello avläsning plan från obegränsad tooMetered Data inte stöds.</span><span class="sxs-lookup"><span data-stu-id="7747a-197">Note that changing hello metering plan from Unlimited Data tooMetered Data is not supported.</span></span>
* <span data-ttu-id="7747a-198">Du kan aktivera och inaktivera *Tillåt klassiska åtgärder*.</span><span class="sxs-lookup"><span data-stu-id="7747a-198">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="7747a-199">Se toohello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md) mer information om gränser och begränsningar.</span><span class="sxs-lookup"><span data-stu-id="7747a-199">Refer toohello [ExpressRoute FAQ](expressroute-faqs.md) for more information on limits and limitations.</span></span>

### <a name="tooenable-hello-expressroute-premium-add-on"></a><span data-ttu-id="7747a-200">tooenable hello ExpressRoute premium-tillägg</span><span class="sxs-lookup"><span data-stu-id="7747a-200">tooenable hello ExpressRoute premium add-on</span></span>
<span data-ttu-id="7747a-201">Du kan aktivera hello ExpressRoute premium-tillägg för en befintlig krets med hjälp av hello följande PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="7747a-201">You can enable hello ExpressRoute premium add-on for your existing circuit by using hello following PowerShell cmdlet:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

<span data-ttu-id="7747a-202">Kretsen har nu hello ExpressRoute premium tilläggsfunktioner aktiverad.</span><span class="sxs-lookup"><span data-stu-id="7747a-202">Your circuit will now have hello ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="7747a-203">Observera att vi startar fakturering för hello premium-tillägg-funktionen som hello-kommandot har körts.</span><span class="sxs-lookup"><span data-stu-id="7747a-203">Note that we will start billing you for hello premium add-on capability as soon as hello command has successfully run.</span></span>

### <a name="toodisable-hello-expressroute-premium-add-on"></a><span data-ttu-id="7747a-204">toodisable hello ExpressRoute premium-tillägg</span><span class="sxs-lookup"><span data-stu-id="7747a-204">toodisable hello ExpressRoute premium add-on</span></span>
> [!IMPORTANT]
> <span data-ttu-id="7747a-205">Den här åtgärden kan misslyckas om du använder resurser som är större än vad som tillåts för standard hello-kretsen.</span><span class="sxs-lookup"><span data-stu-id="7747a-205">This operation can fail if you're using resources that are greater than what is permitted for hello standard circuit.</span></span>
> 
> 

#### <a name="considerations"></a><span data-ttu-id="7747a-206">Överväganden</span><span class="sxs-lookup"><span data-stu-id="7747a-206">Considerations</span></span>

* <span data-ttu-id="7747a-207">Du måste se till att hello antalet virtuella nätverk länkade toohello kretsen är mindre än 10 innan du Nedgradera från premium toostandard.</span><span class="sxs-lookup"><span data-stu-id="7747a-207">You must ensure that hello number of virtual networks linked toohello circuit is less than 10 before you downgrade from premium toostandard.</span></span> <span data-ttu-id="7747a-208">Om du inte gör det, uppdateringsbegäran misslyckas och du kommer att fakturerade hello premium priser.</span><span class="sxs-lookup"><span data-stu-id="7747a-208">If you don't do this, your update request will fail, and you'll be billed hello premium rates.</span></span>
* <span data-ttu-id="7747a-209">Du måste ta bort länken alla virtuella nätverk i andra geopolitiska regioner.</span><span class="sxs-lookup"><span data-stu-id="7747a-209">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="7747a-210">Om du inte gör det, uppdateringsbegäran misslyckas och du kommer att fakturerade hello premium priser.</span><span class="sxs-lookup"><span data-stu-id="7747a-210">If you don't do this, your update request will fail, and you'll be billed hello premium rates.</span></span>
* <span data-ttu-id="7747a-211">Din routningstabellen måste vara mindre än 4 000 vägar för privat peering.</span><span class="sxs-lookup"><span data-stu-id="7747a-211">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="7747a-212">Om din väg tabell är större än 4 000 vägar, släpper hello BGP-sessionen och reenabled inte tills hello antalet annonserade prefix hamnar under 4 000.</span><span class="sxs-lookup"><span data-stu-id="7747a-212">If your route table size is greater than 4,000 routes, hello BGP session will drop and won't be reenabled until hello number of advertised prefixes goes below 4,000.</span></span>

#### <a name="disable-hello-premium-add-on"></a><span data-ttu-id="7747a-213">Inaktivera hello premium-tillägg</span><span class="sxs-lookup"><span data-stu-id="7747a-213">Disable hello premium add-on</span></span>
<span data-ttu-id="7747a-214">Du kan inaktivera hello ExpressRoute premium-tillägg för en befintlig krets med hjälp av hello följande PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="7747a-214">You can disable hello ExpressRoute premium add-on for your existing circuit by using hello following PowerShell cmdlet:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a><span data-ttu-id="7747a-215">tooupdate hello ExpressRoute-kretsen bandbredd</span><span class="sxs-lookup"><span data-stu-id="7747a-215">tooupdate hello ExpressRoute circuit bandwidth</span></span>
<span data-ttu-id="7747a-216">Kontrollera hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md) stöd för alternativ för bandbredd för leverantören.</span><span class="sxs-lookup"><span data-stu-id="7747a-216">Check hello [ExpressRoute FAQ](expressroute-faqs.md) for supported bandwidth options for your provider.</span></span> <span data-ttu-id="7747a-217">Du kan välja valfri storlek som är större än hello storleken på befintliga kretsen som gör att hello fysiska port (där kretsen skapas).</span><span class="sxs-lookup"><span data-stu-id="7747a-217">You can pick any size that is greater than hello size of your existing circuit as long as hello physical port (on which your circuit is created) allows.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7747a-218">Du kan ha toorecreate hello ExpressRoute-krets om det finns för lite kapacitet på befintlig hello-port.</span><span class="sxs-lookup"><span data-stu-id="7747a-218">You may have toorecreate hello ExpressRoute circuit if there is inadequate capacity on hello existing port.</span></span> <span data-ttu-id="7747a-219">Du kan inte uppgradera hello krets om det finns inga ytterligare kapacitet på den platsen.</span><span class="sxs-lookup"><span data-stu-id="7747a-219">You cannot upgrade hello circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="7747a-220">Du kan minska hello bandbredden för en ExpressRoute-krets utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="7747a-220">You cannot reduce hello bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="7747a-221">Nedgradera bandbredd kräver toodeprovision hello ExpressRoute-krets och etablera en ny ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="7747a-221">Downgrading bandwidth requires you toodeprovision hello ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 
> 

#### <a name="resize-a-circuit"></a><span data-ttu-id="7747a-222">Ändra storlek på en krets</span><span class="sxs-lookup"><span data-stu-id="7747a-222">Resize a circuit</span></span>

<span data-ttu-id="7747a-223">När du har bestämt vilken storlek du behöver använda du följande kommando tooresize hello kretsen:</span><span class="sxs-lookup"><span data-stu-id="7747a-223">After you decide what size you need, you can use hello following command tooresize your circuit:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="7747a-224">Kretsen kommer har tagits stora på hello Microsoft sida.</span><span class="sxs-lookup"><span data-stu-id="7747a-224">Your circuit will have been sized up on hello Microsoft side.</span></span> <span data-ttu-id="7747a-225">Du måste kontakta din anslutning providern tooupdate konfigurationer på deras sida toomatch ändringen.</span><span class="sxs-lookup"><span data-stu-id="7747a-225">You must contact your connectivity provider tooupdate configurations on their side toomatch this change.</span></span> <span data-ttu-id="7747a-226">Observera att vi startar fakturering du för hello uppdateras bandbredd alternativet från och med nu på.</span><span class="sxs-lookup"><span data-stu-id="7747a-226">Note that we will start billing you for hello updated bandwidth option from this point on.</span></span>

<span data-ttu-id="7747a-227">Om du ser följande fel om du vill öka hello kretsbandbredden hello innebär det inte är inte tillräckligt med bandbredd åt vänster på hello fysiska port där befintliga kretsen har skapats.</span><span class="sxs-lookup"><span data-stu-id="7747a-227">If you see hello following error when increasing hello circuit bandwidth, it means there is no sufficient bandwidth left on hello physical port where your existing circuit is created.</span></span> <span data-ttu-id="7747a-228">Du har toodelete den här kretsen och skapa en ny krets hello storlek som du behöver.</span><span class="sxs-lookup"><span data-stu-id="7747a-228">You have toodelete this circuit and create a new circuit of hello size you need.</span></span> 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available tooperform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="7747a-229">Borttagning och tar bort en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="7747a-229">Deprovisioning and deleting an ExpressRoute circuit</span></span>

### <a name="considerations"></a><span data-ttu-id="7747a-230">Överväganden</span><span class="sxs-lookup"><span data-stu-id="7747a-230">Considerations</span></span>

* <span data-ttu-id="7747a-231">Du måste ta bort länken alla virtuella nätverk från hello ExpressRoute-krets för den här åtgärden toosucceed.</span><span class="sxs-lookup"><span data-stu-id="7747a-231">You must unlink all virtual networks from hello ExpressRoute circuit for this operation toosucceed.</span></span> <span data-ttu-id="7747a-232">Kontrollera toosee om du har några virtuella nätverk som är länkade toohello krets om åtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="7747a-232">Check toosee if you have any virtual networks that are linked toohello circuit if this operation fails.</span></span>
* <span data-ttu-id="7747a-233">Om hello ExpressRoute-kretsen tjänstleverantör Etableringsstatus **etablering** eller **etablerad** du måste arbeta med service provider toodeprovision hello kretsen på sidan.</span><span class="sxs-lookup"><span data-stu-id="7747a-233">If hello ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider toodeprovision hello circuit on their side.</span></span> <span data-ttu-id="7747a-234">Kommer att fortsätta tooreserve resurser och debitera dig tills hello-leverantör har slutförts avställningsskript hello-krets och meddela oss.</span><span class="sxs-lookup"><span data-stu-id="7747a-234">We will continue tooreserve resources and bill you until hello service provider completes deprovisioning hello circuit and notifies us.</span></span>
* <span data-ttu-id="7747a-235">Om hello-leverantör har avetableras hello krets (hello-leverantör Etableringsstatus har angetts för**inte etablerats**) du kan sedan ta bort hello krets.</span><span class="sxs-lookup"><span data-stu-id="7747a-235">If hello service provider has deprovisioned hello circuit (hello service provider provisioning state is set too**Not provisioned**) you can then delete hello circuit.</span></span> <span data-ttu-id="7747a-236">Detta förhindrar fakturering för hello krets.</span><span class="sxs-lookup"><span data-stu-id="7747a-236">This will stop billing for hello circuit.</span></span>

#### <a name="delete-a-circuit"></a><span data-ttu-id="7747a-237">Ta bort en krets</span><span class="sxs-lookup"><span data-stu-id="7747a-237">Delete a circuit</span></span>

<span data-ttu-id="7747a-238">Du kan ta bort ExpressRoute-krets genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7747a-238">You can delete your ExpressRoute circuit by running hello following command:</span></span>

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a><span data-ttu-id="7747a-239">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7747a-239">Next steps</span></span>
<span data-ttu-id="7747a-240">Kontrollera att du hello följande när du har skapat kretsen:</span><span class="sxs-lookup"><span data-stu-id="7747a-240">After you create your circuit, make sure that you do hello following:</span></span>

* [<span data-ttu-id="7747a-241">Skapa och ändra routning för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="7747a-241">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-classic.md)
* [<span data-ttu-id="7747a-242">Länka ditt virtuella nätverk tooyour ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="7747a-242">Link your virtual network tooyour ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-classic.md)

