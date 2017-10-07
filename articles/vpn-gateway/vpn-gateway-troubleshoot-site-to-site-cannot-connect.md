---
title: "aaaTroubleshoot en Azure VPN-anslutning för plats-till-plats som inte kan ansluta | Microsoft Docs"
description: "Lär dig hur tootroubleshoot en plats-till-plats VPN-anslutning som plötsligt slutar fungera och kan inte återanslutas."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/21/2017
ms.author: genli
ms.openlocfilehash: 632c75bfcfb93a532eeead2855d43e6614569a99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-an-azure-site-to-site-vpn-connection-cannot-connect-and-stops-working"></a><span data-ttu-id="08a3c-103">Felsökning: Azure plats-till-plats VPN-anslutningen kan inte ansluta och slutar fungera</span><span class="sxs-lookup"><span data-stu-id="08a3c-103">Troubleshooting: An Azure site-to-site VPN connection cannot connect and stops working</span></span>

<span data-ttu-id="08a3c-104">När du har konfigurerat en plats-till-plats VPN-anslutning mellan ett lokalt nätverk och Azure-nätverk slutar fungera plötsligt hello VPN-anslutning och kan inte återanslutas.</span><span class="sxs-lookup"><span data-stu-id="08a3c-104">After you configure a site-to-site VPN connection between an on-premises network and an Azure virtual network, hello VPN connection suddenly stops working and cannot be reconnected.</span></span> <span data-ttu-id="08a3c-105">Den här artikeln innehåller felsökning steg toohelp du lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="08a3c-105">This article provides troubleshooting steps toohelp you resolve this problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a><span data-ttu-id="08a3c-106">Felsökningssteg</span><span class="sxs-lookup"><span data-stu-id="08a3c-106">Troubleshooting steps</span></span>

<span data-ttu-id="08a3c-107">tooresolve hello problemet först försöka för[Återställ hello Azure VPN-gateway](vpn-gateway-resetgw-classic.md) och återställa hello tunnel från hello lokala VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="08a3c-107">tooresolve hello problem, first try too[reset hello Azure VPN gateway](vpn-gateway-resetgw-classic.md) and reset hello tunnel from hello on-premises VPN device.</span></span> <span data-ttu-id="08a3c-108">Följ dessa steg tooidentify hello orsaken hello problemet om hello problemet kvarstår.</span><span class="sxs-lookup"><span data-stu-id="08a3c-108">If hello problem persists, follow these steps tooidentify hello cause of hello problem.</span></span>

### <a name="prerequisite-step"></a><span data-ttu-id="08a3c-109">Innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="08a3c-109">Prerequisite step</span></span>

<span data-ttu-id="08a3c-110">Kontrollera hello typ av hello Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="08a3c-110">Check hello type of hello Azure VPN gateway.</span></span>

1. <span data-ttu-id="08a3c-111">Gå toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="08a3c-111">Go toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="08a3c-112">Kontrollera hello **översikt** sidan hello VPN-gateway för hello typinformation.</span><span class="sxs-lookup"><span data-stu-id="08a3c-112">Check hello **Overview** page of hello VPN gateway for hello type information.</span></span>
    
    ![Översikt över hello gateway](media\vpn-gateway-troubleshoot-site-to-site-cannot-connect\gatewayoverview.png)

### <a name="step-1-check-whether-hello-on-premises-vpn-device-is-validated"></a><span data-ttu-id="08a3c-114">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="08a3c-114">Step 1.</span></span> <span data-ttu-id="08a3c-115">Kontrollera om hello lokala VPN-enhet har verifierats</span><span class="sxs-lookup"><span data-stu-id="08a3c-115">Check whether hello on-premises VPN device is validated</span></span>

1. <span data-ttu-id="08a3c-116">Kontrollera om du använder en [verifiera VPN-enhet och operativsystemversion](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="08a3c-116">Check whether you are using a [validated VPN device and operating system version](vpn-gateway-about-vpn-devices.md#devicetable).</span></span> <span data-ttu-id="08a3c-117">Om hello enheten inte är en verifierad VPN-enhet, kanske toocontact hello enhetens tillverkare toosee om det finns ett kompatibilitetsproblem.</span><span class="sxs-lookup"><span data-stu-id="08a3c-117">If hello device is not a validated VPN device, you might have toocontact hello device manufacturer toosee if there is a compatibility issue.</span></span>

2. <span data-ttu-id="08a3c-118">Kontrollera att hello VPN-enheten är korrekt konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="08a3c-118">Make sure that hello VPN device is correctly configured.</span></span> <span data-ttu-id="08a3c-119">Mer information finns i [redigera enheten configuration exempel](/vpn-gateway-about-vpn-devices.md#editing).</span><span class="sxs-lookup"><span data-stu-id="08a3c-119">For more information, see [Edit device configuration samples](/vpn-gateway-about-vpn-devices.md#editing).</span></span>

### <a name="step-2-verify-hello-shared-key"></a><span data-ttu-id="08a3c-120">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="08a3c-120">Step 2.</span></span> <span data-ttu-id="08a3c-121">Kontrollera hello delad nyckel</span><span class="sxs-lookup"><span data-stu-id="08a3c-121">Verify hello shared key</span></span>

<span data-ttu-id="08a3c-122">Jämför hello delad nyckel för hello lokala VPN-enhet toohello VPN för Azure Virtual Network toomake att hello nycklar matchar.</span><span class="sxs-lookup"><span data-stu-id="08a3c-122">Compare hello shared key for hello on-premises VPN device toohello Azure Virtual Network VPN toomake sure that hello keys match.</span></span> 

<span data-ttu-id="08a3c-123">tooview hello delad nyckel för hello Azure VPN-anslutning med någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="08a3c-123">tooview hello shared key for hello Azure VPN connection, use one of hello following methods:</span></span>

<span data-ttu-id="08a3c-124">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="08a3c-124">**Azure portal**</span></span>

1. <span data-ttu-id="08a3c-125">Gå toohello VPN plats-till-plats-gatewayanslutning som du skapade.</span><span class="sxs-lookup"><span data-stu-id="08a3c-125">Go toohello VPN gateway site-to-site connection that you created.</span></span>

2. <span data-ttu-id="08a3c-126">I hello **inställningar** klickar du på **delad nyckel**.</span><span class="sxs-lookup"><span data-stu-id="08a3c-126">In hello **Settings** section, click **Shared key**.</span></span>
    
    ![Delad nyckel](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/sharedkey.png)

<span data-ttu-id="08a3c-128">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="08a3c-128">**Azure PowerShell**</span></span>

<span data-ttu-id="08a3c-129">För hello Azure Resource Manager-distributionsmodellen:</span><span class="sxs-lookup"><span data-stu-id="08a3c-129">For hello Azure Resource Manager deployment model:</span></span>

    Get-AzureRmVirtualNetworkGatewayConnectionSharedKey -Name <Connection name> -ResourceGroupName <Resource group name>

<span data-ttu-id="08a3c-130">För hello klassiska distributionsmodellen:</span><span class="sxs-lookup"><span data-stu-id="08a3c-130">For hello classic deployment model:</span></span>

    Get-AzureVNetGatewayKey -VNetName -LocalNetworkSiteName

### <a name="step-3-verify-hello-vpn-peer-ips"></a><span data-ttu-id="08a3c-131">Steg 3.</span><span class="sxs-lookup"><span data-stu-id="08a3c-131">Step 3.</span></span> <span data-ttu-id="08a3c-132">Kontrollera hello VPN-peer IP-adresser</span><span class="sxs-lookup"><span data-stu-id="08a3c-132">Verify hello VPN peer IPs</span></span>

-   <span data-ttu-id="08a3c-133">Hej IP-definition i hello **lokala nätverksgateway** objektet i Azure måste matcha hello lokala enhet IP.</span><span class="sxs-lookup"><span data-stu-id="08a3c-133">hello IP definition in hello **Local Network Gateway** object in Azure should match hello on-premises device IP.</span></span>
-   <span data-ttu-id="08a3c-134">hello Azure gateway IP-definition som är inställd på hello lokal enhet ska matcha hello Azure gateway IP.</span><span class="sxs-lookup"><span data-stu-id="08a3c-134">hello Azure gateway IP definition that is set on hello on-premises device should match hello Azure gateway IP.</span></span>

### <a name="step-4-check-udr-and-nsgs-on-hello-gateway-subnet"></a><span data-ttu-id="08a3c-135">Steg 4.</span><span class="sxs-lookup"><span data-stu-id="08a3c-135">Step 4.</span></span> <span data-ttu-id="08a3c-136">Kontrollera UDR och NSG: er på hello gateway-undernät</span><span class="sxs-lookup"><span data-stu-id="08a3c-136">Check UDR and NSGs on hello gateway subnet</span></span>

<span data-ttu-id="08a3c-137">Sök efter och ta bort användardefinierade routning (UDR) eller Nätverkssäkerhetsgrupper (NSG: er) på hello gateway-undernät och testa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="08a3c-137">Check for and remove user-defined routing (UDR) or Network Security Groups (NSGs) on hello gateway subnet, and then test hello result.</span></span> <span data-ttu-id="08a3c-138">Validera hello-inställningar som UDR eller NSG tillämpas om hello problemet har lösts.</span><span class="sxs-lookup"><span data-stu-id="08a3c-138">If hello problem is resolved, validate hello settings that UDR or NSG applied.</span></span>

### <a name="step-5-check-hello-on-premises-vpn-device-external-interface-address"></a><span data-ttu-id="08a3c-139">Steg 5.</span><span class="sxs-lookup"><span data-stu-id="08a3c-139">Step 5.</span></span> <span data-ttu-id="08a3c-140">Kontrollera externa gränssnittet för hello lokala VPN-enhetsadress</span><span class="sxs-lookup"><span data-stu-id="08a3c-140">Check hello on-premises VPN device external interface address</span></span>

- <span data-ttu-id="08a3c-141">Om hello mot Internet IP-adressen för VPN-enhet för hello ingår i hello **lokala nätverket** definition i Azure, kan du uppleva sporadiska frånkopplingar.</span><span class="sxs-lookup"><span data-stu-id="08a3c-141">If hello Internet-facing IP address of hello VPN device is included in hello **Local network** definition in Azure, you might experience sporadic disconnections.</span></span>
- <span data-ttu-id="08a3c-142">hello måste externa enhetsgränssnittet vara direkt på hello Internet.</span><span class="sxs-lookup"><span data-stu-id="08a3c-142">hello device's external interface must be directly on hello Internet.</span></span> <span data-ttu-id="08a3c-143">Det bör finnas några nätverksadresser eller brandvägg mellan hello Internet och hello enhet.</span><span class="sxs-lookup"><span data-stu-id="08a3c-143">There should be no network address translation or firewall between hello Internet and hello device.</span></span>
- <span data-ttu-id="08a3c-144">tooconfigure klustring toohave en virtuell IP-adress för brandvägg måste du bryta hello kluster och exponera hello VPN-enhet direkt tooa offentliga gränssnittet som hello-gateway kan samverka med.</span><span class="sxs-lookup"><span data-stu-id="08a3c-144">tooconfigure firewall clustering toohave a virtual IP, you must break hello cluster and expose hello VPN appliance directly tooa public interface that hello gateway can interface with.</span></span>

### <a name="step-6-verify-that-hello-subnets-match-exactly-azure-policy-based-gateways"></a><span data-ttu-id="08a3c-145">Steg 6.</span><span class="sxs-lookup"><span data-stu-id="08a3c-145">Step 6.</span></span> <span data-ttu-id="08a3c-146">Kontrollera att hello undernät matchar exakt (Azure policy-baserad gateway)</span><span class="sxs-lookup"><span data-stu-id="08a3c-146">Verify that hello subnets match exactly (Azure policy-based gateways)</span></span>

-   <span data-ttu-id="08a3c-147">Kontrollera att hello undernät matchar exakt mellan hello Azure-nätverk och lokala definitioner för hello virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="08a3c-147">Verify that hello subnets match exactly between hello Azure virtual network and on-premises definitions for hello Azure virtual network.</span></span>
-   <span data-ttu-id="08a3c-148">Kontrollera att hello undernät matchar exakt mellan hello **lokala nätverksgateway** och lokala definitioner för hello lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="08a3c-148">Verify that hello subnets match exactly between hello **Local Network Gateway** and on-premises definitions for hello on-premises network.</span></span>

### <a name="step-7-verify-hello-azure-gateway-health-probe"></a><span data-ttu-id="08a3c-149">Steg 7.</span><span class="sxs-lookup"><span data-stu-id="08a3c-149">Step 7.</span></span> <span data-ttu-id="08a3c-150">Kontrollera hello Azure gateway hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="08a3c-150">Verify hello Azure gateway health probe</span></span>

1. <span data-ttu-id="08a3c-151">Gå toohello [hälsoavsökningen](https://&lt;YourVirtualNetworkGatewayIP&gt;:8081/healthprobe).</span><span class="sxs-lookup"><span data-stu-id="08a3c-151">Go toohello [health probe](https://&lt;YourVirtualNetworkGatewayIP&gt;:8081/healthprobe).</span></span>

2. <span data-ttu-id="08a3c-152">Klicka dig igenom hello certifikatvarning.</span><span class="sxs-lookup"><span data-stu-id="08a3c-152">Click through hello certificate warning.</span></span>
3. <span data-ttu-id="08a3c-153">Om du får svar anses hello VPN-gateway felfritt.</span><span class="sxs-lookup"><span data-stu-id="08a3c-153">If you receive a response, hello VPN gateway is considered healthy.</span></span> <span data-ttu-id="08a3c-154">Om du inte får ett svar, hello gateway kanske inte är felfri eller hello problemet beror på en NSG på hello gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="08a3c-154">If you don't receive a response, hello gateway might not be healthy or an NSG on hello gateway subnet is causing hello problem.</span></span> <span data-ttu-id="08a3c-155">hello efter texten är en exempelsvar:</span><span class="sxs-lookup"><span data-stu-id="08a3c-155">hello following text is a sample response:</span></span>

    <span data-ttu-id="08a3c-156">&lt;? xml-version = ”1.0”? > <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">primära-instansen: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6 < / sträng&gt;</span><span class="sxs-lookup"><span data-stu-id="08a3c-156">&lt;?xml version="1.0"?>  <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Primary Instance: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6</string&gt;</span></span>

### <a name="step-8-check-whether-hello-on-premises-vpn-device-has-hello-perfect-forward-secrecy-feature-enabled"></a><span data-ttu-id="08a3c-157">Steg 8.</span><span class="sxs-lookup"><span data-stu-id="08a3c-157">Step 8.</span></span> <span data-ttu-id="08a3c-158">Kontrollera om hello lokala VPN-enhet har hello PFS-funktionen aktiverad</span><span class="sxs-lookup"><span data-stu-id="08a3c-158">Check whether hello on-premises VPN device has hello perfect forward secrecy feature enabled</span></span>

<span data-ttu-id="08a3c-159">hello PFS-funktionen kan orsaka problem för frånkoppling.</span><span class="sxs-lookup"><span data-stu-id="08a3c-159">hello perfect forward secrecy feature can cause disconnection problems.</span></span> <span data-ttu-id="08a3c-160">Om hello VPN-enhet har PFS aktiverat, inaktiveras hello.</span><span class="sxs-lookup"><span data-stu-id="08a3c-160">If hello VPN device has perfect forward secrecy enabled, disable hello feature.</span></span> <span data-ttu-id="08a3c-161">Uppdatera hello VPN-gateway IPsec-princip.</span><span class="sxs-lookup"><span data-stu-id="08a3c-161">Then update hello VPN gateway IPsec policy.</span></span>

## <a name="next-steps"></a><span data-ttu-id="08a3c-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="08a3c-162">Next steps</span></span>

-   [<span data-ttu-id="08a3c-163">Konfigurera ett virtuellt nätverk tooa för plats-till-plats-anslutning</span><span class="sxs-lookup"><span data-stu-id="08a3c-163">Configure a site-to-site connection tooa virtual network</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
-   [<span data-ttu-id="08a3c-164">Konfigurera en princip för IPsec/IKE för plats-till-plats VPN-anslutningar</span><span class="sxs-lookup"><span data-stu-id="08a3c-164">Configure an IPsec/IKE policy for site-to-site VPN connections</span></span>](vpn-gateway-ipsecikepolicy-rm-powershell.md)
