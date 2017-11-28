---
title: "Skapa och ändra en ExpressRoute-krets: PowerShell: klassiska Azure-portalen | Microsoft Docs"
description: "Den här artikeln vägleder dig genom stegen för att skapa och etablera en ExpressRoute-krets. Den här artikeln visar även hur att kontrollera status, uppdatera eller ta bort och ta bort etableringen kretsen."
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
ms.openlocfilehash: 3b12bbb21ebf6a0160227c4a281c420cf192d6f7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a><span data-ttu-id="dbe17-104">Skapa och ändra en ExpressRoute-krets med hjälp av PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="dbe17-104">Create and modify an ExpressRoute circuit using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dbe17-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dbe17-105">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="dbe17-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="dbe17-106">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="dbe17-107">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="dbe17-107">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="dbe17-108">Video - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dbe17-108">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="dbe17-109">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="dbe17-109">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="dbe17-110">Den här artikeln vägleder dig genom stegen för att skapa en Azure ExpressRoute-krets med hjälp av PowerShell-cmdlets och den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="dbe17-110">This article walks you through the steps to create an Azure ExpressRoute circuit by using PowerShell cmdlets and the classic deployment model.</span></span> <span data-ttu-id="dbe17-111">Den här artikeln visar även hur att kontrollera status, uppdatera eller ta bort och ta bort etableringen av en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="dbe17-111">This article also shows you how to check the status, update, or delete and deprovision an ExpressRoute circuit.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


<span data-ttu-id="dbe17-112">**Om distributionsmodeller för Azure**</span><span class="sxs-lookup"><span data-stu-id="dbe17-112">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a><span data-ttu-id="dbe17-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="dbe17-113">Before you begin</span></span>
### <a name="step-1-review-the-prerequisites-and-workflow-articles"></a><span data-ttu-id="dbe17-114">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="dbe17-114">Step 1.</span></span> <span data-ttu-id="dbe17-115">Granska krav och arbetsflöde artiklar</span><span class="sxs-lookup"><span data-stu-id="dbe17-115">Review the prerequisites and workflow articles</span></span>
<span data-ttu-id="dbe17-116">Kontrollera att du har granskat den [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="dbe17-116">Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>  

### <a name="step-2-install-the-latest-versions-of-the-azure-service-management-sm-powershell-modules"></a><span data-ttu-id="dbe17-117">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="dbe17-117">Step 2.</span></span> <span data-ttu-id="dbe17-118">Installera de senaste versionerna av Azure Service Management (SM) PowerShell-moduler</span><span class="sxs-lookup"><span data-stu-id="dbe17-118">Install the latest versions of the Azure Service Management (SM) PowerShell modules</span></span>
<span data-ttu-id="dbe17-119">Följ instruktionerna i [komma igång med Azure PowerShell-cmdlets](/powershell/azure/overview) stegvisa instruktioner om hur du konfigurerar din dator om du vill använda Azure PowerShell-moduler.</span><span class="sxs-lookup"><span data-stu-id="dbe17-119">Follow the instructions in [Getting started with Azure PowerShell cmdlets](/powershell/azure/overview) for step-by-step guidance on how to configure your computer to use the Azure PowerShell modules.</span></span>

### <a name="step-3-log-in-to-your-azure-account-and-select-a-subscription"></a><span data-ttu-id="dbe17-120">Steg 3.</span><span class="sxs-lookup"><span data-stu-id="dbe17-120">Step 3.</span></span> <span data-ttu-id="dbe17-121">Logga in på ditt Azure-konto och välja en prenumeration</span><span class="sxs-lookup"><span data-stu-id="dbe17-121">Log in to your Azure account and select a subscription</span></span>
1. <span data-ttu-id="dbe17-122">Öppna PowerShell-konsolen med utökade rättigheter och anslut till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="dbe17-122">Open your PowerShell console with elevated rights and connect to your account.</span></span> <span data-ttu-id="dbe17-123">Använd följande exempel för att ansluta:</span><span class="sxs-lookup"><span data-stu-id="dbe17-123">Use the following example to help you connect:</span></span>

        Login-AzureRmAccount

2. <span data-ttu-id="dbe17-124">Kontrollera prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="dbe17-124">Check the subscriptions for the account.</span></span>

        Get-AzureRmSubscription

3. <span data-ttu-id="dbe17-125">Om du har mer än en prenumeration väljer du den du vill använda.</span><span class="sxs-lookup"><span data-stu-id="dbe17-125">If you have more than one subscription, select the subscription that you want to use.</span></span>

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. <span data-ttu-id="dbe17-126">Därefter kan du använda följande cmdlet för att lägga till din Azure-prenumeration i PowerShell för den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="dbe17-126">Next, use the following cmdlet to add your Azure subscription to PowerShell for the classic deployment model.</span></span>

        Add-AzureAccount

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="dbe17-127">Skapa och etablera en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="dbe17-127">Create and provision an ExpressRoute circuit</span></span>
### <a name="step-1-import-the-powershell-modules-for-expressroute"></a><span data-ttu-id="dbe17-128">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="dbe17-128">Step 1.</span></span> <span data-ttu-id="dbe17-129">Importera PowerShell-moduler för ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="dbe17-129">Import the PowerShell modules for ExpressRoute</span></span>
 <span data-ttu-id="dbe17-130">Om du inte redan har gjort det, måste du importera Azure och ExpressRoute-moduler i PowerShell-sessionen för att börja använda ExpressRoute-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="dbe17-130">If you have not already done so, you must import the Azure and ExpressRoute modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="dbe17-131">Du importerar moduler från den plats som de installerades för att lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="dbe17-131">You import the modules from the location that they were installed to on your local computer.</span></span> <span data-ttu-id="dbe17-132">Beroende på vilken metod som du använde för att installera modulerna vara platsen densamma som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="dbe17-132">Depending on the method you used to install the modules, the location may be different than the following example shows.</span></span> <span data-ttu-id="dbe17-133">Ändra exemplet om det behövs.</span><span class="sxs-lookup"><span data-stu-id="dbe17-133">Modify the example if necessary.</span></span>  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-the-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="dbe17-134">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="dbe17-134">Step 2.</span></span> <span data-ttu-id="dbe17-135">Hämta en lista över providers som stöds, platser och bandbredder</span><span class="sxs-lookup"><span data-stu-id="dbe17-135">Get the list of supported providers, locations, and bandwidths</span></span>
<span data-ttu-id="dbe17-136">Innan du skapar en ExpressRoute-krets måste listan över providers stöds anslutning, platser och alternativ för bandbredd.</span><span class="sxs-lookup"><span data-stu-id="dbe17-136">Before you create an ExpressRoute circuit, you need the list of supported connectivity providers, locations, and bandwidth options.</span></span>

<span data-ttu-id="dbe17-137">PowerShell-cmdleten `Get-AzureDedicatedCircuitServiceProvider` returnerar informationen som du ska använda i senare steg:</span><span class="sxs-lookup"><span data-stu-id="dbe17-137">The PowerShell cmdlet `Get-AzureDedicatedCircuitServiceProvider` returns this information, which you’ll use in later steps:</span></span>

    Get-AzureDedicatedCircuitServiceProvider

<span data-ttu-id="dbe17-138">Kontrollera om anslutningsleverantören finns med i listan.</span><span class="sxs-lookup"><span data-stu-id="dbe17-138">Check to see if your connectivity provider is listed there.</span></span> <span data-ttu-id="dbe17-139">Anteckna följande information eftersom du behöver det senare när du skapar en krets:</span><span class="sxs-lookup"><span data-stu-id="dbe17-139">Make a note of the following information because you'll need it later when you create a circuit:</span></span>

* <span data-ttu-id="dbe17-140">Namn</span><span class="sxs-lookup"><span data-stu-id="dbe17-140">Name</span></span>
* <span data-ttu-id="dbe17-141">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="dbe17-141">PeeringLocations</span></span>
* <span data-ttu-id="dbe17-142">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="dbe17-142">BandwidthsOffered</span></span>

<span data-ttu-id="dbe17-143">Nu är du redo att skapa en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="dbe17-143">You're now ready to create an ExpressRoute circuit.</span></span>         

### <a name="step-3-create-an-expressroute-circuit"></a><span data-ttu-id="dbe17-144">Steg 3.</span><span class="sxs-lookup"><span data-stu-id="dbe17-144">Step 3.</span></span> <span data-ttu-id="dbe17-145">Skapa en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="dbe17-145">Create an ExpressRoute circuit</span></span>
<span data-ttu-id="dbe17-146">I följande exempel visas hur du skapar en 200 Mbit/s ExpressRoute-krets via Equinix i Silicon dal.</span><span class="sxs-lookup"><span data-stu-id="dbe17-146">The following example shows how to create a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="dbe17-147">Om du använder en annan provider och andra inställningar, i stället informationen när du gör din begäran.</span><span class="sxs-lookup"><span data-stu-id="dbe17-147">If you're using a different provider and different settings, substitute that information when you make your request.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dbe17-148">ExpressRoute-krets kommer att debiteras från den tidpunkt då en nyckel har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="dbe17-148">Your ExpressRoute circuit will be billed from the moment a service key is issued.</span></span> <span data-ttu-id="dbe17-149">Se till att utföra den här åtgärden när anslutningen providern är redo att etablera kretsen.</span><span class="sxs-lookup"><span data-stu-id="dbe17-149">Ensure that you perform this operation when the connectivity provider is ready to provision the circuit.</span></span>
> 
> 

<span data-ttu-id="dbe17-150">Följande är ett exempelbegäran om en ny nyckel:</span><span class="sxs-lookup"><span data-stu-id="dbe17-150">The following is an example request for a new service key:</span></span>

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

<span data-ttu-id="dbe17-151">Eller, om du vill skapa en ExpressRoute-krets med premium-tillägg kan du använda följande exempel.</span><span class="sxs-lookup"><span data-stu-id="dbe17-151">Or, if you want to create an ExpressRoute circuit with the premium add-on, use the following example.</span></span> <span data-ttu-id="dbe17-152">Referera till den [ExpressRoute vanliga frågor och svar](expressroute-faqs.md) för mer information om premium-tillägg.</span><span class="sxs-lookup"><span data-stu-id="dbe17-152">Refer to the [ExpressRoute FAQ](expressroute-faqs.md) for more details about the premium add-on.</span></span>

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


<span data-ttu-id="dbe17-153">Svaret innehåller nyckeln för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dbe17-153">The response will contain the service key.</span></span> <span data-ttu-id="dbe17-154">Du kan få en detaljerad beskrivning av alla parametrar genom att köra följande:</span><span class="sxs-lookup"><span data-stu-id="dbe17-154">You can get detailed descriptions of all the parameters by running the following:</span></span>

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-the-expressroute-circuits"></a><span data-ttu-id="dbe17-155">Steg 4.</span><span class="sxs-lookup"><span data-stu-id="dbe17-155">Step 4.</span></span> <span data-ttu-id="dbe17-156">Visa en lista med alla ExpressRoute-kretsar</span><span class="sxs-lookup"><span data-stu-id="dbe17-156">List all the ExpressRoute circuits</span></span>
<span data-ttu-id="dbe17-157">Du kan köra den `Get-AzureDedicatedCircuit` kommando för att hämta en lista över alla ExpressRoute-kretsar som du skapade:</span><span class="sxs-lookup"><span data-stu-id="dbe17-157">You can run the `Get-AzureDedicatedCircuit` command to get a list of all the ExpressRoute circuits that you created:</span></span>

    Get-AzureDedicatedCircuit

<span data-ttu-id="dbe17-158">Svaret blir något som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="dbe17-158">The response will be something similar to the following example:</span></span>

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="dbe17-159">Du kan hämta den här informationen när som helst genom att använda den `Get-AzureDedicatedCircuit` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="dbe17-159">You can retrieve this information at any time by using the `Get-AzureDedicatedCircuit` cmdlet.</span></span> <span data-ttu-id="dbe17-160">Att göra anropet utan några parametrar visas alla kretsar.</span><span class="sxs-lookup"><span data-stu-id="dbe17-160">Making the call without any parameters lists all the circuits.</span></span> <span data-ttu-id="dbe17-161">Nyckeln för tjänsten visas i den *ServiceKey* fältet.</span><span class="sxs-lookup"><span data-stu-id="dbe17-161">Your service key will be listed in the *ServiceKey* field.</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="dbe17-162">Du kan få en detaljerad beskrivning av alla parametrar genom att köra följande:</span><span class="sxs-lookup"><span data-stu-id="dbe17-162">You can get detailed descriptions of all the parameters by running the following:</span></span>

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a><span data-ttu-id="dbe17-163">Steg 5.</span><span class="sxs-lookup"><span data-stu-id="dbe17-163">Step 5.</span></span> <span data-ttu-id="dbe17-164">Skicka tjänstnyckeln till anslutningsleverantören för etablering</span><span class="sxs-lookup"><span data-stu-id="dbe17-164">Send the service key to your connectivity provider for provisioning</span></span>
<span data-ttu-id="dbe17-165">*ServiceProviderProvisioningState* innehåller information om det aktuella tillståndet för etablering på sida-leverantör.</span><span class="sxs-lookup"><span data-stu-id="dbe17-165">*ServiceProviderProvisioningState* provides information on the current state of provisioning on the service-provider side.</span></span> <span data-ttu-id="dbe17-166">*Status för* ger tillståndet på Microsoft-sida.</span><span class="sxs-lookup"><span data-stu-id="dbe17-166">*Status* provides the state on the Microsoft side.</span></span> <span data-ttu-id="dbe17-167">Mer information om krets etablering tillstånd finns det [arbetsflöden](expressroute-workflows.md#expressroute-circuit-provisioning-states) artikel.</span><span class="sxs-lookup"><span data-stu-id="dbe17-167">For more information about circuit provisioning states, see the [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="dbe17-168">När du skapar en ny ExpressRoute-krets blir kretsen i följande tillstånd:</span><span class="sxs-lookup"><span data-stu-id="dbe17-168">When you create a new ExpressRoute circuit, the circuit will be in the following state:</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="dbe17-169">Kretsen kommer att gå till följande tillstånd när anslutningen providern håller på att du:</span><span class="sxs-lookup"><span data-stu-id="dbe17-169">The circuit will go to the following state when the connectivity provider is in the process of enabling it for you:</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

<span data-ttu-id="dbe17-170">En ExpressRoute-krets måste vara i följande läge att kunna använda den:</span><span class="sxs-lookup"><span data-stu-id="dbe17-170">An ExpressRoute circuit must be in the following state for you to be able to use it:</span></span>

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a><span data-ttu-id="dbe17-171">Steg 6.</span><span class="sxs-lookup"><span data-stu-id="dbe17-171">Step 6.</span></span> <span data-ttu-id="dbe17-172">Regelbundet kontrollera status och tillståndet för nyckeln krets</span><span class="sxs-lookup"><span data-stu-id="dbe17-172">Periodically check the status and the state of the circuit key</span></span>
<span data-ttu-id="dbe17-173">På så sätt får du reda på om din leverantör har aktiverat kretsen.</span><span class="sxs-lookup"><span data-stu-id="dbe17-173">This lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="dbe17-174">När kretsen har konfigurerats, *ServiceProviderProvisioningState* visas som *etablerad* som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="dbe17-174">After the circuit has been configured, *ServiceProviderProvisioningState* will display as *Provisioned* as shown in the following example:</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a><span data-ttu-id="dbe17-175">Steg 7.</span><span class="sxs-lookup"><span data-stu-id="dbe17-175">Step 7.</span></span> <span data-ttu-id="dbe17-176">Skapa din routningskonfiguration</span><span class="sxs-lookup"><span data-stu-id="dbe17-176">Create your routing configuration</span></span>
<span data-ttu-id="dbe17-177">Referera till den [ExpressRoute-krets routningskonfiguration (skapa och ändra krets peerkopplingar)](expressroute-howto-routing-classic.md) artikel stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="dbe17-177">Refer to the [ExpressRoute circuit routing configuration (create and modify circuit peerings)](expressroute-howto-routing-classic.md) article for step-by-step instructions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dbe17-178">Dessa anvisningar gäller endast för kretsar som skapats med leverantörer som erbjuder layer 2 tjänster.</span><span class="sxs-lookup"><span data-stu-id="dbe17-178">These instructions only apply to circuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="dbe17-179">Om du använder en tjänstprovider som erbjuder Hanterade layer 3-tjänster (vanligtvis en IP VPN, t.ex. MPLS), anslutningsleverantören kan konfigurera och hantera routning av du.</span><span class="sxs-lookup"><span data-stu-id="dbe17-179">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="step-8-link-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="dbe17-180">Steg 8.</span><span class="sxs-lookup"><span data-stu-id="dbe17-180">Step 8.</span></span> <span data-ttu-id="dbe17-181">Länka ett virtuellt nätverk till en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="dbe17-181">Link a virtual network to an ExpressRoute circuit</span></span>
<span data-ttu-id="dbe17-182">Därefter länka ett virtuellt nätverk till ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="dbe17-182">Next, link a virtual network to your ExpressRoute circuit.</span></span> <span data-ttu-id="dbe17-183">Referera till [länka ExpressRoute-kretsar till virtuella nätverk](expressroute-howto-linkvnet-classic.md) stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="dbe17-183">Refer to [Linking ExpressRoute circuits to virtual networks](expressroute-howto-linkvnet-classic.md) for step-by-step instructions.</span></span> <span data-ttu-id="dbe17-184">Om du behöver skapa ett virtuellt nätverk med den klassiska distributionsmodellen expressroute, se [skapa ett virtuellt nätverk för ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span><span class="sxs-lookup"><span data-stu-id="dbe17-184">If you need to create a virtual network using the classic deployment model for ExpressRoute, see [Create a virtual network for ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span></span>

## <a name="getting-the-status-of-an-expressroute-circuit"></a><span data-ttu-id="dbe17-185">Hämta status för en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="dbe17-185">Getting the status of an ExpressRoute circuit</span></span>
<span data-ttu-id="dbe17-186">Du kan hämta den här informationen när som helst genom att använda den `Get-AzureCircuit` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="dbe17-186">You can retrieve this information at any time by using the `Get-AzureCircuit` cmdlet.</span></span> <span data-ttu-id="dbe17-187">Att göra anropet utan några parametrar visas alla kretsar.</span><span class="sxs-lookup"><span data-stu-id="dbe17-187">Making the call without any parameters lists all the circuits.</span></span>

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

<span data-ttu-id="dbe17-188">Du kan få information om en specifik ExpressRoute-krets genom att överföra nyckeln för tjänsten som en parameter till anropet.</span><span class="sxs-lookup"><span data-stu-id="dbe17-188">You can get information on a specific ExpressRoute circuit by passing the service key as a parameter to the call.</span></span>

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


<span data-ttu-id="dbe17-189">Du kan få en detaljerad beskrivning av alla parametrar genom att köra följande exempel:</span><span class="sxs-lookup"><span data-stu-id="dbe17-189">You can get detailed descriptions of all the parameters by running the following example:</span></span>

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a><span data-ttu-id="dbe17-190">Ändra en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="dbe17-190">Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="dbe17-191">Du kan ändra vissa egenskaper för en ExpressRoute-krets utan att påverka anslutningen.</span><span class="sxs-lookup"><span data-stu-id="dbe17-191">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="dbe17-192">Du kan göra följande med utan avbrott:</span><span class="sxs-lookup"><span data-stu-id="dbe17-192">You can do the following with no downtime:</span></span>

* <span data-ttu-id="dbe17-193">Aktivera eller inaktivera tillägget ExpressRoute premium för ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="dbe17-193">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="dbe17-194">Öka bandbredden för ExpressRoute-krets under förutsättning att det finns tillgänglig kapacitet på porten.</span><span class="sxs-lookup"><span data-stu-id="dbe17-194">Increase the bandwidth of your ExpressRoute circuit provided there is capacity available on the port.</span></span> <span data-ttu-id="dbe17-195">Observera att nedgradera bandbredden för en krets inte stöds.</span><span class="sxs-lookup"><span data-stu-id="dbe17-195">Note that downgrading the bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="dbe17-196">Ändra avläsning planen från förbrukade Data till obegränsad.</span><span class="sxs-lookup"><span data-stu-id="dbe17-196">Change the metering plan from Metered Data to Unlimited Data.</span></span> <span data-ttu-id="dbe17-197">Observera att om du ändrar avläsning planen från obegränsad till förbrukade Data inte stöds.</span><span class="sxs-lookup"><span data-stu-id="dbe17-197">Note that changing the metering plan from Unlimited Data to Metered Data is not supported.</span></span>
* <span data-ttu-id="dbe17-198">Du kan aktivera och inaktivera *Tillåt klassiska åtgärder*.</span><span class="sxs-lookup"><span data-stu-id="dbe17-198">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="dbe17-199">Referera till den [ExpressRoute vanliga frågor och svar](expressroute-faqs.md) mer information om gränser och begränsningar.</span><span class="sxs-lookup"><span data-stu-id="dbe17-199">Refer to the [ExpressRoute FAQ](expressroute-faqs.md) for more information on limits and limitations.</span></span>

### <a name="to-enable-the-expressroute-premium-add-on"></a><span data-ttu-id="dbe17-200">Så här aktiverar du ExpressRoute premium-tillägg</span><span class="sxs-lookup"><span data-stu-id="dbe17-200">To enable the ExpressRoute premium add-on</span></span>
<span data-ttu-id="dbe17-201">Du kan aktivera ExpressRoute premium-tillägg för en befintlig krets med hjälp av följande PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="dbe17-201">You can enable the ExpressRoute premium add-on for your existing circuit by using the following PowerShell cmdlet:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

<span data-ttu-id="dbe17-202">Kretsen har nu tilläggsfunktioner för ExpressRoute premium aktiverat.</span><span class="sxs-lookup"><span data-stu-id="dbe17-202">Your circuit will now have the ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="dbe17-203">Observera att vi startar fakturering för premium-tillägg-funktionen när kommandot har körts.</span><span class="sxs-lookup"><span data-stu-id="dbe17-203">Note that we will start billing you for the premium add-on capability as soon as the command has successfully run.</span></span>

### <a name="to-disable-the-expressroute-premium-add-on"></a><span data-ttu-id="dbe17-204">Att inaktivera tillägget ExpressRoute premium</span><span class="sxs-lookup"><span data-stu-id="dbe17-204">To disable the ExpressRoute premium add-on</span></span>
> [!IMPORTANT]
> <span data-ttu-id="dbe17-205">Den här åtgärden kan misslyckas om du använder resurser som är större än vad som tillåts för standard-kretsen.</span><span class="sxs-lookup"><span data-stu-id="dbe17-205">This operation can fail if you're using resources that are greater than what is permitted for the standard circuit.</span></span>
> 
> 

#### <a name="considerations"></a><span data-ttu-id="dbe17-206">Överväganden</span><span class="sxs-lookup"><span data-stu-id="dbe17-206">Considerations</span></span>

* <span data-ttu-id="dbe17-207">Du måste se till att antalet virtuella nätverk som är kopplad till kretsen är mindre än 10 innan du Nedgradera från premium till standard.</span><span class="sxs-lookup"><span data-stu-id="dbe17-207">You must ensure that the number of virtual networks linked to the circuit is less than 10 before you downgrade from premium to standard.</span></span> <span data-ttu-id="dbe17-208">Om du inte gör det, uppdateringsbegäran misslyckas och du kommer att debiteras premium priser.</span><span class="sxs-lookup"><span data-stu-id="dbe17-208">If you don't do this, your update request will fail, and you'll be billed the premium rates.</span></span>
* <span data-ttu-id="dbe17-209">Du måste ta bort länken alla virtuella nätverk i andra geopolitiska regioner.</span><span class="sxs-lookup"><span data-stu-id="dbe17-209">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="dbe17-210">Om du inte gör det, uppdateringsbegäran misslyckas och du kommer att debiteras premium priser.</span><span class="sxs-lookup"><span data-stu-id="dbe17-210">If you don't do this, your update request will fail, and you'll be billed the premium rates.</span></span>
* <span data-ttu-id="dbe17-211">Din routningstabellen måste vara mindre än 4 000 vägar för privat peering.</span><span class="sxs-lookup"><span data-stu-id="dbe17-211">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="dbe17-212">Om din väg tabell är större än 4 000 vägar, kommer BGP-sessionen och reenabled inte tills antalet annonserade prefix hamnar under 4 000.</span><span class="sxs-lookup"><span data-stu-id="dbe17-212">If your route table size is greater than 4,000 routes, the BGP session will drop and won't be reenabled until the number of advertised prefixes goes below 4,000.</span></span>

#### <a name="disable-the-premium-add-on"></a><span data-ttu-id="dbe17-213">Inaktivera tillägget premium</span><span class="sxs-lookup"><span data-stu-id="dbe17-213">Disable the premium add-on</span></span>
<span data-ttu-id="dbe17-214">Du kan inaktivera ExpressRoute premium-tillägg för en befintlig krets med hjälp av följande PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="dbe17-214">You can disable the ExpressRoute premium add-on for your existing circuit by using the following PowerShell cmdlet:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a><span data-ttu-id="dbe17-215">Uppdatera bandbredd för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="dbe17-215">To update the ExpressRoute circuit bandwidth</span></span>
<span data-ttu-id="dbe17-216">Kontrollera den [ExpressRoute vanliga frågor och svar](expressroute-faqs.md) stöd för alternativ för bandbredd för leverantören.</span><span class="sxs-lookup"><span data-stu-id="dbe17-216">Check the [ExpressRoute FAQ](expressroute-faqs.md) for supported bandwidth options for your provider.</span></span> <span data-ttu-id="dbe17-217">Du kan välja valfri storlek som är större än storleken på befintliga kretsen som gör att den fysiska porten (där kretsen skapas).</span><span class="sxs-lookup"><span data-stu-id="dbe17-217">You can pick any size that is greater than the size of your existing circuit as long as the physical port (on which your circuit is created) allows.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dbe17-218">Du kan behöva återskapa ExpressRoute-kretsen om det finns för lite kapacitet på befintliga porten.</span><span class="sxs-lookup"><span data-stu-id="dbe17-218">You may have to recreate the ExpressRoute circuit if there is inadequate capacity on the existing port.</span></span> <span data-ttu-id="dbe17-219">Du kan inte uppgradera kretsen om det finns inga ytterligare kapacitet på den platsen.</span><span class="sxs-lookup"><span data-stu-id="dbe17-219">You cannot upgrade the circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="dbe17-220">Du kan minska bandbredden för en ExpressRoute-krets utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="dbe17-220">You cannot reduce the bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="dbe17-221">Nedgradera bandbredd måste du ta bort etableringen av ExpressRoute-kretsen och etablera en ny ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="dbe17-221">Downgrading bandwidth requires you to deprovision the ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 
> 

#### <a name="resize-a-circuit"></a><span data-ttu-id="dbe17-222">Ändra storlek på en krets</span><span class="sxs-lookup"><span data-stu-id="dbe17-222">Resize a circuit</span></span>

<span data-ttu-id="dbe17-223">När du har bestämt vilken storlek måste använda du följande kommando för att ändra storlek på kretsen:</span><span class="sxs-lookup"><span data-stu-id="dbe17-223">After you decide what size you need, you can use the following command to resize your circuit:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="dbe17-224">Kretsen kommer har tagits stora på Microsoft-sida.</span><span class="sxs-lookup"><span data-stu-id="dbe17-224">Your circuit will have been sized up on the Microsoft side.</span></span> <span data-ttu-id="dbe17-225">Du måste kontakta anslutningsleverantören om du vill uppdatera konfigurationer på sidan så att den matchar den här ändringen.</span><span class="sxs-lookup"><span data-stu-id="dbe17-225">You must contact your connectivity provider to update configurations on their side to match this change.</span></span> <span data-ttu-id="dbe17-226">Observera att vi startar debitering du för alternativet uppdaterade bandbredd från och med nu.</span><span class="sxs-lookup"><span data-stu-id="dbe17-226">Note that we will start billing you for the updated bandwidth option from this point on.</span></span>

<span data-ttu-id="dbe17-227">Om du ser följande fel om du vill öka krets bandbredd, innebär det inte finns inte tillräckligt med bandbredd kvar på den fysiska porten där befintliga kretsen har skapats.</span><span class="sxs-lookup"><span data-stu-id="dbe17-227">If you see the following error when increasing the circuit bandwidth, it means there is no sufficient bandwidth left on the physical port where your existing circuit is created.</span></span> <span data-ttu-id="dbe17-228">Du måste ta bort den här kretsen och skapa en ny krets storlek som du behöver.</span><span class="sxs-lookup"><span data-stu-id="dbe17-228">You have to delete this circuit and create a new circuit of the size you need.</span></span> 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="dbe17-229">Borttagning och tar bort en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="dbe17-229">Deprovisioning and deleting an ExpressRoute circuit</span></span>

### <a name="considerations"></a><span data-ttu-id="dbe17-230">Överväganden</span><span class="sxs-lookup"><span data-stu-id="dbe17-230">Considerations</span></span>

* <span data-ttu-id="dbe17-231">Du måste ta bort länken alla virtuella nätverk från ExpressRoute-kretsen för den här åtgärden ska lyckas.</span><span class="sxs-lookup"><span data-stu-id="dbe17-231">You must unlink all virtual networks from the ExpressRoute circuit for this operation to succeed.</span></span> <span data-ttu-id="dbe17-232">Kontrollera om du har några virtuella nätverk som är länkade till kretsen om åtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="dbe17-232">Check to see if you have any virtual networks that are linked to the circuit if this operation fails.</span></span>
* <span data-ttu-id="dbe17-233">Om leverantören för ExpressRoute-kretsen Etableringsläge är **etablering** eller **etablerad** du måste arbeta med tjänstleverantören för att ta bort etableringen krets på sidan.</span><span class="sxs-lookup"><span data-stu-id="dbe17-233">If the ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider to deprovision the circuit on their side.</span></span> <span data-ttu-id="dbe17-234">Vi kommer att fortsätta att reservera resurser och debitera dig tills tjänstleverantör har slutförts avetablering kretsen och meddela oss.</span><span class="sxs-lookup"><span data-stu-id="dbe17-234">We will continue to reserve resources and bill you until the service provider completes deprovisioning the circuit and notifies us.</span></span>
* <span data-ttu-id="dbe17-235">Om tjänstleverantör har avetableras kretsen (Etableringsstatus tjänstleverantör har angetts till **inte etablerats**) du kan sedan ta bort kretsen.</span><span class="sxs-lookup"><span data-stu-id="dbe17-235">If the service provider has deprovisioned the circuit (the service provider provisioning state is set to **Not provisioned**) you can then delete the circuit.</span></span> <span data-ttu-id="dbe17-236">Detta förhindrar att faktureringen för kretsen.</span><span class="sxs-lookup"><span data-stu-id="dbe17-236">This will stop billing for the circuit.</span></span>

#### <a name="delete-a-circuit"></a><span data-ttu-id="dbe17-237">Ta bort en krets</span><span class="sxs-lookup"><span data-stu-id="dbe17-237">Delete a circuit</span></span>

<span data-ttu-id="dbe17-238">Du kan ta bort ExpressRoute-krets genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="dbe17-238">You can delete your ExpressRoute circuit by running the following command:</span></span>

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a><span data-ttu-id="dbe17-239">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dbe17-239">Next steps</span></span>
<span data-ttu-id="dbe17-240">När du har skapat kretsen, kontrollera att du gör följande:</span><span class="sxs-lookup"><span data-stu-id="dbe17-240">After you create your circuit, make sure that you do the following:</span></span>

* [<span data-ttu-id="dbe17-241">Skapa och ändra routning för ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="dbe17-241">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-classic.md)
* [<span data-ttu-id="dbe17-242">Länka ditt virtuella nätverk till ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="dbe17-242">Link your virtual network to your ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-classic.md)

