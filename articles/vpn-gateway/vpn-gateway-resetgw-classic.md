---
title: "Återställa en Azure VPN gateway tooreestablish IPsec-tunnlar | Microsoft Docs"
description: "Den här artikeln beskriver hur du återställer Azure VPN-Gateway tooreestablish IPsec-tunnlar. hello artikeln gäller tooVPN gateways i både hello klassiska och hello Resource Manager distributionsmodellerna."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 79d77cb8-d175-4273-93ac-712d7d45b1fe
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: cherylmc
ms.openlocfilehash: 84dd741f0bebd6b18cb235216a68a88da5fe17b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reset-a-vpn-gateway"></a><span data-ttu-id="820ba-104">Återställ en VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="820ba-104">Reset a VPN Gateway</span></span>

<span data-ttu-id="820ba-105">Du kan behöva återställa en Azure VPN-gateway om VPN-anslutningen mellan flera platser i en eller flera VPN-tunnlar för plats-till-plats bryts.</span><span class="sxs-lookup"><span data-stu-id="820ba-105">Resetting an Azure VPN gateway is helpful if you lose cross-premises VPN connectivity on one or more Site-to-Site VPN tunnels.</span></span> <span data-ttu-id="820ba-106">I så fall kan din lokala VPN-enheter är alla fungerar, men är inte tooestablish IPsec-tunnlar med hello Azure VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="820ba-106">In this situation, your on-premises VPN devices are all working correctly, but are not able tooestablish IPsec tunnels with hello Azure VPN gateways.</span></span> <span data-ttu-id="820ba-107">Den här artikeln hjälper dig att återställa din VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="820ba-107">This article helps you reset your VPN gateway.</span></span>

### <a name="what-happens-during-a-reset"></a><span data-ttu-id="820ba-108">Vad som händer under en återställning?</span><span class="sxs-lookup"><span data-stu-id="820ba-108">What happens during a reset?</span></span>

<span data-ttu-id="820ba-109">En VPN-gateway består av två VM-instanser som körs i en konfiguration i aktivt vänteläge.</span><span class="sxs-lookup"><span data-stu-id="820ba-109">A VPN gateway is composed of two VM instances running in an active-standby configuration.</span></span> <span data-ttu-id="820ba-110">När du återställer hello gateway hello gateway startas och sedan återställer hello mellan lokala konfigurationer tooit.</span><span class="sxs-lookup"><span data-stu-id="820ba-110">When you reset hello gateway, it reboots hello gateway, and then reapplies hello cross-premises configurations tooit.</span></span> <span data-ttu-id="820ba-111">hello gateway behåller hello offentliga IP-adressen har redan.</span><span class="sxs-lookup"><span data-stu-id="820ba-111">hello gateway keeps hello public IP address it already has.</span></span> <span data-ttu-id="820ba-112">Det innebär att du inte behöver tooupdate hello VPN routerkonfiguration med en ny offentlig IP-adress för Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="820ba-112">This means you won’t need tooupdate hello VPN router configuration with a new public IP address for Azure VPN gateway.</span></span>

<span data-ttu-id="820ba-113">När du skickar hello kommandot tooreset hello gateway startas hello aktuella aktiva instansen av hello Azure VPN-gateway omedelbart.</span><span class="sxs-lookup"><span data-stu-id="820ba-113">When you issue hello command tooreset hello gateway, hello current active instance of hello Azure VPN gateway is rebooted immediately.</span></span> <span data-ttu-id="820ba-114">Det blir ett kort mellanrum under hello växling vid fel från hello aktiva instansen (startas), toohello vänteläge instans.</span><span class="sxs-lookup"><span data-stu-id="820ba-114">There will be a brief gap during hello failover from hello active instance (being rebooted), toohello standby instance.</span></span> <span data-ttu-id="820ba-115">hello mellanrummet bör vara mindre än en minut.</span><span class="sxs-lookup"><span data-stu-id="820ba-115">hello gap should be less than one minute.</span></span>

<span data-ttu-id="820ba-116">Om inte hello anslutningen återställs efter hello första omstarten problemet hello samma kommando igen tooreboot hello andra VM-instans (hello nya aktiva gateway).</span><span class="sxs-lookup"><span data-stu-id="820ba-116">If hello connection is not restored after hello first reboot, issue hello same command again tooreboot hello second VM instance (hello new active gateway).</span></span> <span data-ttu-id="820ba-117">Om hello två omstarter begärda tillbaka tooback kan finnas det en längre period där båda VM-instanser (aktiva och vänteläge) startas.</span><span class="sxs-lookup"><span data-stu-id="820ba-117">If hello two reboots are requested back tooback, there will be a slightly longer period where both VM instances (active and standby) are being rebooted.</span></span> <span data-ttu-id="820ba-118">Detta innebär att ett längre uppehåll hello VPN-anslutning, in too2 too4 minuter för virtuella datorer toocomplete hello omstarter.</span><span class="sxs-lookup"><span data-stu-id="820ba-118">This will cause a longer gap on hello VPN connectivity, up too2 too4 minutes for VMs toocomplete hello reboots.</span></span>

<span data-ttu-id="820ba-119">Efter två omstarter, om du fortfarande har problem med nätverksanslutningen mellan platser, öppnar du en supportbegäran från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="820ba-119">After two reboots, if you are still experiencing cross-premises connectivity problems, please open a support request from hello Azure portal.</span></span>

## <span data-ttu-id="820ba-120"><a name="before"></a>Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="820ba-120"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="820ba-121">Innan du återställer din gateway, kontrollera hello huvudpunkter som anges nedan för varje IPsec plats-till-plats (S2S) VPN-tunnel.</span><span class="sxs-lookup"><span data-stu-id="820ba-121">Before you reset your gateway, verify hello key items listed below for each IPsec Site-to-Site (S2S) VPN tunnel.</span></span> <span data-ttu-id="820ba-122">Alla matchning av datatyp i hello objekt leder hello att koppla från S2S VPN-tunnlar.</span><span class="sxs-lookup"><span data-stu-id="820ba-122">Any mismatch in hello items will result in hello disconnect of S2S VPN tunnels.</span></span> <span data-ttu-id="820ba-123">Kontrollera och korrigera hello konfigurationer för lokalt och Azure VPN-gatewayer behöver du inte onödiga omstarter och avbrott för hello andra fungerande anslutningar på hello gateways.</span><span class="sxs-lookup"><span data-stu-id="820ba-123">Verifying and correcting hello configurations for your on-premises and Azure VPN gateways saves you from unnecessary reboots and disruptions for hello other working connections on hello gateways.</span></span>

<span data-ttu-id="820ba-124">Kontrollera hello följande objekt innan du återställer din gateway:</span><span class="sxs-lookup"><span data-stu-id="820ba-124">Verify hello following items before resetting your gateway:</span></span>

* <span data-ttu-id="820ba-125">hello Internet IP-adresser (VIP) för både hello Azure VPN-gateway och hello lokala VPN-gateway har konfigurerats korrekt i båda hello Azure och hello lokala VPN-principer.</span><span class="sxs-lookup"><span data-stu-id="820ba-125">hello Internet IP addresses (VIPs) for both hello Azure VPN gateway and hello on-premises VPN gateway are configured correctly in both hello Azure and hello on-premises VPN policies.</span></span>
* <span data-ttu-id="820ba-126">hello i förväg delad nyckel måste hello samma på både Azure och lokala VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="820ba-126">hello pre-shared key must be hello same on both Azure and on-premises VPN gateways.</span></span>
* <span data-ttu-id="820ba-127">Om du tillämpar specifika IPsec/IKE-konfiguration, som kryptering, hash-algoritmer och PFS (Perfect Forward Secrecy), se till att både hello Azure och lokala VPN-gatewayer har hello samma konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="820ba-127">If you apply specific IPsec/IKE configuration, such as encryption, hashing algorithms, and PFS (Perfect Forward Secrecy), ensure both hello Azure and on-premises VPN gateways have hello same configurations.</span></span>

## <span data-ttu-id="820ba-128"><a name="portal"></a>Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="820ba-128"><a name="portal"></a>Azure portal</span></span>

<span data-ttu-id="820ba-129">Du kan återställa en Resource Manager VPN-gateway med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="820ba-129">You can reset a Resource Manager VPN gateway using hello Azure portal.</span></span> <span data-ttu-id="820ba-130">Om du vill tooreset klassiska gateway, se hello [PowerShell](#resetclassic) steg.</span><span class="sxs-lookup"><span data-stu-id="820ba-130">If you want tooreset a classic gateway, see hello [PowerShell](#resetclassic) steps.</span></span>

### <a name="resource-manager-deployment-model"></a><span data-ttu-id="820ba-131">Distributionsmodell med Resource Manager</span><span class="sxs-lookup"><span data-stu-id="820ba-131">Resource Manager deployment model</span></span>

1. <span data-ttu-id="820ba-132">Öppna hello [Azure-portalen](https://portal.azure.com) och navigera toohello Resource Manager virtuell nätverksgateway som du vill tooreset.</span><span class="sxs-lookup"><span data-stu-id="820ba-132">Open hello [Azure portal](https://portal.azure.com) and navigate toohello Resource Manager virtual network gateway that you want tooreset.</span></span>
2. <span data-ttu-id="820ba-133">Klicka på ”Återställ' hello bladet för hello virtuell nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="820ba-133">On hello blade for hello virtual network gateway, click 'Reset'.</span></span>

  ![Återställ VPN-Gateway-bladet](./media/vpn-gateway-howto-reset-gateway/reset-vpn-gateway-portal.png)
3. <span data-ttu-id="820ba-135">I hello återställa bladet, klicka på hello **återställa** knappen.</span><span class="sxs-lookup"><span data-stu-id="820ba-135">On hello Reset blade, click hello **Reset** button.</span></span>

## <span data-ttu-id="820ba-136"><a name="ps"></a>PowerShell</span><span class="sxs-lookup"><span data-stu-id="820ba-136"><a name="ps"></a>PowerShell</span></span>

### <a name="resource-manager-deployment-model"></a><span data-ttu-id="820ba-137">Distributionsmodell med Resource Manager</span><span class="sxs-lookup"><span data-stu-id="820ba-137">Resource Manager deployment model</span></span>

<span data-ttu-id="820ba-138">Hej cmdlet för att återställa en gateway är **Återställ AzureRmVirtualNetworkGateway**.</span><span class="sxs-lookup"><span data-stu-id="820ba-138">hello cmdlet for resetting a gateway is **Reset-AzureRmVirtualNetworkGateway**.</span></span> <span data-ttu-id="820ba-139">Innan du utför en återställning, kontrollera att du har hello senaste versionen av hello [Resource Manager PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span><span class="sxs-lookup"><span data-stu-id="820ba-139">Before performing a reset, make sure you have hello latest version of hello [Resource Manager PowerShell cmdlets](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).</span></span> <span data-ttu-id="820ba-140">hello återställer följande exempel en virtuell nätverksgateway med namnet VNet1GW i hello TestRG1 resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="820ba-140">hello following example resets a virtual network gateway named VNet1GW in hello TestRG1 resource group:</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw
```

<span data-ttu-id="820ba-141">Resultat:</span><span class="sxs-lookup"><span data-stu-id="820ba-141">Result:</span></span>

<span data-ttu-id="820ba-142">När du får ett returnerade resultat, kan du anta hello gateway återställning har slutförts.</span><span class="sxs-lookup"><span data-stu-id="820ba-142">When you receive a return result, you can assume hello gateway reset was successful.</span></span> <span data-ttu-id="820ba-143">Men finns det inget i hello returnerade resultat att uttryckligen att hello återställning har slutförts.</span><span class="sxs-lookup"><span data-stu-id="820ba-143">However, there is nothing in hello return result that indicates explicitly that hello reset was successful.</span></span> <span data-ttu-id="820ba-144">Om du vill toolook nära på hello historik toosee exakt när hello gateway återställa uppstod, du kan visa informationen i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="820ba-144">If you want toolook closely at hello history toosee exactly when hello gateway reset occurred, you can view that information in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="820ba-145">I hello portal, navigerar för**'GatewayName' -> Resource Health**.</span><span class="sxs-lookup"><span data-stu-id="820ba-145">In hello portal, navigate too**'GatewayName' -> Resource Health**.</span></span>

### <span data-ttu-id="820ba-146"><a name="resetclassic"></a>Klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="820ba-146"><a name="resetclassic"></a>Classic deployment model</span></span>

<span data-ttu-id="820ba-147">Hej cmdlet för att återställa en gateway är **Återställ AzureVNetGateway**.</span><span class="sxs-lookup"><span data-stu-id="820ba-147">hello cmdlet for resetting a gateway is **Reset-AzureVNetGateway**.</span></span> <span data-ttu-id="820ba-148">Innan du utför en återställning, kontrollera att du har hello senaste versionen av hello [Service Management (SM) PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="820ba-148">Before performing a reset, make sure you have hello latest version of hello [Service Management (SM) PowerShell cmdlets](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0).</span></span> <span data-ttu-id="820ba-149">hello återställs följande exempel hello-gateway för virtuellt nätverk med namnet ”ContosoVNet”:</span><span class="sxs-lookup"><span data-stu-id="820ba-149">hello following example resets hello gateway for a virtual network named "ContosoVNet":</span></span>

```powershell
Reset-AzureVNetGateway –VnetName “ContosoVNet”
```

<span data-ttu-id="820ba-150">Resultat:</span><span class="sxs-lookup"><span data-stu-id="820ba-150">Result:</span></span>

```powershell
Error          :
HttpStatusCode : OK
Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
Status         : Successful
RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
StatusCode     : OK
```

## <span data-ttu-id="820ba-151"><a name="cli"></a>Azure CLI</span><span class="sxs-lookup"><span data-stu-id="820ba-151"><a name="cli"></a>Azure CLI</span></span>

<span data-ttu-id="820ba-152">Använd hello-tooreset hello gateway [az vnet-nätverksgateway återställa](https://docs.microsoft.com/cli/azure/network/vnet-gateway#reset) kommando.</span><span class="sxs-lookup"><span data-stu-id="820ba-152">tooreset hello gateway, use hello [az network vnet-gateway reset](https://docs.microsoft.com/cli/azure/network/vnet-gateway#reset) command.</span></span> <span data-ttu-id="820ba-153">hello återställer följande exempel en virtuell nätverksgateway med namnet VNet5GW i hello TestRG5 resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="820ba-153">hello following example resets a virtual network gateway named VNet5GW in hello TestRG5 resource group:</span></span>

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

<span data-ttu-id="820ba-154">Resultat:</span><span class="sxs-lookup"><span data-stu-id="820ba-154">Result:</span></span>

<span data-ttu-id="820ba-155">När du får ett returnerade resultat, kan du anta hello gateway återställning har slutförts.</span><span class="sxs-lookup"><span data-stu-id="820ba-155">When you receive a return result, you can assume hello gateway reset was successful.</span></span> <span data-ttu-id="820ba-156">Men finns det inget i hello returnerade resultat att uttryckligen att hello återställning har slutförts.</span><span class="sxs-lookup"><span data-stu-id="820ba-156">However, there is nothing in hello return result that indicates explicitly that hello reset was successful.</span></span> <span data-ttu-id="820ba-157">Om du vill toolook nära på hello historik toosee exakt när hello gateway återställa uppstod, du kan visa informationen i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="820ba-157">If you want toolook closely at hello history toosee exactly when hello gateway reset occurred, you can view that information in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="820ba-158">I hello portal, navigerar för**'GatewayName' -> Resource Health**.</span><span class="sxs-lookup"><span data-stu-id="820ba-158">In hello portal, navigate too**'GatewayName' -> Resource Health**.</span></span>
