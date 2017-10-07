---
title: "aaaTroubleshoot Azure plats-till-plats-VPN kopplar från periodvis | Microsoft Docs"
description: "Lär dig hur tootroubleshoot hello problem i vilka hello plats-till-plats VPN-anslutningen kopplas från regelbundet."
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
ms.openlocfilehash: 1ce3c4ff9d8f650312e45f33b760ebcc6597fc13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-site-to-site-vpn-disconnects-intermittently"></a><span data-ttu-id="c335a-103">Felsöka: Azure plats-till-plats-VPN kopplar från periodvis</span><span class="sxs-lookup"><span data-stu-id="c335a-103">Troubleshooting: Azure Site-to-Site VPN disconnects intermittently</span></span>

<span data-ttu-id="c335a-104">Du kan uppleva hello problem att en ny eller befintlig Microsoft Azure punkt-till-plats VPN-anslutning inte är stabilt eller kopplar från regelbundet.</span><span class="sxs-lookup"><span data-stu-id="c335a-104">You might experience hello problem that a new or existing Microsoft Azure Point-to-Site VPN connection is not stable or disconnects regularly.</span></span> <span data-ttu-id="c335a-105">Den här artikeln innehåller felsöka steg toohelp du identifiera och lösa hello orsaken hello problemet.</span><span class="sxs-lookup"><span data-stu-id="c335a-105">This article provides troubleshoot steps toohelp you identify and resolve hello cause of hello problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a><span data-ttu-id="c335a-106">Felsökningssteg</span><span class="sxs-lookup"><span data-stu-id="c335a-106">Troubleshooting steps</span></span>

### <a name="prerequisite-step"></a><span data-ttu-id="c335a-107">Innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="c335a-107">Prerequisite step</span></span>

<span data-ttu-id="c335a-108">Kontrollera hello typ av Azure virtuella nätverkets gateway:</span><span class="sxs-lookup"><span data-stu-id="c335a-108">Check hello type of Azure  virtual network gateway:</span></span>

1. <span data-ttu-id="c335a-109">Gå för[Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c335a-109">Go too[Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c335a-110">Kontrollera hello **översikt** sidan hello virtuell nätverksgateway hello typen information.</span><span class="sxs-lookup"><span data-stu-id="c335a-110">Check hello **Overview** page of hello virtual network gateway for hello type information.</span></span>
    
    ![hello översikt över hello gateway](media\vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently\gatewayoverview.png)

### <a name="step-1-check-whether-hello-on-premises-vpn-device-is-validated"></a><span data-ttu-id="c335a-112">Steg 1 Kontrollera om hello lokala VPN-enhet har verifierats</span><span class="sxs-lookup"><span data-stu-id="c335a-112">Step 1 Check whether hello on-premises VPN device is validated</span></span>

1. <span data-ttu-id="c335a-113">Kontrollera om du använder en [verifiera VPN-enhet och operativsystemversion](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="c335a-113">Check whether you are using a [validated VPN device and operating system version](vpn-gateway-about-vpn-devices.md#devicetable).</span></span> <span data-ttu-id="c335a-114">Om hello VPN-enhet inte har verifierats, kanske toocontact hello enhetens tillverkare toosee om det inte finns några kompatibilitetsproblem.</span><span class="sxs-lookup"><span data-stu-id="c335a-114">If hello VPN device is not validated, you may have toocontact hello device manufacturer toosee if there is any compatibility issue.</span></span>
2. <span data-ttu-id="c335a-115">Kontrollera att hello VPN-enheten är korrekt konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="c335a-115">Make sure that hello VPN device is correctly configured.</span></span> <span data-ttu-id="c335a-116">Mer information finns i [redigera enheten configuration exempel](vpn-gateway-about-vpn-devices.md#editing).</span><span class="sxs-lookup"><span data-stu-id="c335a-116">For more information, see [Editing device configuration samples](vpn-gateway-about-vpn-devices.md#editing).</span></span>

### <a name="step-2-check-hello-security-association-settingsfor-policy-based-azure-virtual-network-gateways"></a><span data-ttu-id="c335a-117">Steg 2 Kontrollera hello säkerhetsassociation inställningar (för gruppolicy-baserad Azure virtuella nätverks-gateway)</span><span class="sxs-lookup"><span data-stu-id="c335a-117">Step 2 Check hello Security Association settings(for policy-based Azure virtual network gateways)</span></span>

1. <span data-ttu-id="c335a-118">Se till att hello virtuellt nätverk, undernät och intervall hello **lokal nätverksgateway** definition i Microsoft Azure är samma som hello konfiguration på hello lokala VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="c335a-118">Make sure that hello virtual network, subnets and, ranges in hello **Local network gateway** definition in Microsoft Azure are same as hello configuration on hello on-premises VPN device.</span></span>
2. <span data-ttu-id="c335a-119">Kontrollera som överensstämmer med hello säkerhetsassociation inställningar.</span><span class="sxs-lookup"><span data-stu-id="c335a-119">Verify that hello Security Association settings match.</span></span>

### <a name="step-3-check-for-user-defined-routes-or-network-security-groups-on-gateway-subnet"></a><span data-ttu-id="c335a-120">Steg 3-kontroll för användardefinierade vägar eller Nätverkssäkerhetsgrupper på Gateway-undernät</span><span class="sxs-lookup"><span data-stu-id="c335a-120">Step 3 Check for User-Defined Routes or Network Security Groups on Gateway Subnet</span></span>

<span data-ttu-id="c335a-121">En användardefinierad väg på hello gateway-undernätet kan begränsa viss trafik och tillåta andra trafik.</span><span class="sxs-lookup"><span data-stu-id="c335a-121">A user-defined route on hello gateway subnet may be restricting some traffic and allowing other traffic.</span></span> <span data-ttu-id="c335a-122">Detta gör det visas att hello VPN-anslutningen är instabilt för viss trafik och bra för andra.</span><span class="sxs-lookup"><span data-stu-id="c335a-122">This makes it appear that hello VPN connection is unreliable for some traffic and good for others.</span></span> 

### <a name="step-4-check-hello-one-vpn-tunnel-per-subnet-pair-setting-for-policy-based-virtual-network-gateways"></a><span data-ttu-id="c335a-123">Steg 4 Kontrollera Hej ”en VPN-Tunnel undernät par” (för principbaserad virtuella nätverks-gateway)</span><span class="sxs-lookup"><span data-stu-id="c335a-123">Step 4 Check hello "one VPN Tunnel per Subnet Pair" setting (for policy-based virtual network gateways)</span></span>

<span data-ttu-id="c335a-124">Kontrollera att hello lokala VPN-enheten anges toohave **en VPN-tunnel undernät par** för principbaserad virtuella nätverksgatewayer.</span><span class="sxs-lookup"><span data-stu-id="c335a-124">Make sure that hello on-premises VPN device is set toohave **one VPN tunnel per subnet pair** for policy-based virtual network gateways.</span></span>

### <a name="step-5-check-for-security-association-limitation-for-policy-based-virtual-network-gateways"></a><span data-ttu-id="c335a-125">Steg 5 Kontrollera säkerhet Association begränsningen (för principbaserad virtuella nätverksgatewayer)</span><span class="sxs-lookup"><span data-stu-id="c335a-125">Step 5 Check for Security Association Limitation (for policy-based virtual network gateways)</span></span>

<span data-ttu-id="c335a-126">hello Policy-baserad virtuell nätverksgateway har högst 200 undernät säkerhetsassociation par.</span><span class="sxs-lookup"><span data-stu-id="c335a-126">hello Policy-based virtual network gateway has limit of 200 subnet Security Association pairs.</span></span> <span data-ttu-id="c335a-127">Om hello antalet undernät som virtuella Azure-nätverket multiplicerat gånger av hello antalet lokala undernät är större än 200 ser du sporadiska undernät som kopplar från.</span><span class="sxs-lookup"><span data-stu-id="c335a-127">If hello number of Azure virtual network subnets multiplied times by hello number of local subnets is greater than 200, you see sporadic subnets disconnecting.</span></span>

### <a name="step-6-check-on-premises-vpn-device-external-interface-address"></a><span data-ttu-id="c335a-128">Steg 6 Kontrollera lokalt externa gränssnittet för VPN-enhetsadress</span><span class="sxs-lookup"><span data-stu-id="c335a-128">Step 6 Check on-premises VPN device external interface address</span></span>

- <span data-ttu-id="c335a-129">Om hello mot IP-adressen för VPN-enhet för hello Internet ingår i hello **lokal nätverksgateway** definition i Azure, sporadiska frånkopplingar kan uppstå.</span><span class="sxs-lookup"><span data-stu-id="c335a-129">If hello Internet facing IP address of hello VPN device is included in hello **Local network gateway** definition in Azure, you may experience sporadic disconnections.</span></span>
- <span data-ttu-id="c335a-130">hello måste externa enhetsgränssnittet vara direkt på hello Internet.</span><span class="sxs-lookup"><span data-stu-id="c335a-130">hello device's external interface must be directly on hello Internet.</span></span> <span data-ttu-id="c335a-131">Det bör finnas några NAT (Network Address Translation) eller brandvägg mellan hello Internet och hello enhet.</span><span class="sxs-lookup"><span data-stu-id="c335a-131">There should be no Network Address Translation (NAT) or firewall between hello Internet and hello device.</span></span>
-  <span data-ttu-id="c335a-132">Om du konfigurerar brandväggen Clustering toohave en virtuell IP-adress, måste du bryta hello kluster och exponera hello VPN-enhet direkt tooa offentliga gränssnittet som hello gateway kan samverka med.</span><span class="sxs-lookup"><span data-stu-id="c335a-132">If you configure Firewall Clustering toohave a virtual IP, you must break hello cluster and expose hello VPN appliance directly tooa public interface that hello gateway can interface with.</span></span>

### <a name="step-7-check-whether-hello-on-premises-vpn-device-has-perfect-forward-secrecy-enabled"></a><span data-ttu-id="c335a-133">Steg 7 Kontrollera om hello lokala VPN-enhet har Perfect Forward Secrecy aktiverad</span><span class="sxs-lookup"><span data-stu-id="c335a-133">Step 7 Check whether hello on-premises VPN device has Perfect Forward Secrecy enabled</span></span>

<span data-ttu-id="c335a-134">Hej **Perfect Forward Secrecy** funktion kan orsaka hello frånkoppling problem.</span><span class="sxs-lookup"><span data-stu-id="c335a-134">hello **Perfect Forward Secrecy** feature can cause hello disconnection problems.</span></span> <span data-ttu-id="c335a-135">Om hello VPN-enhet har **perfekt forward Secrecy** aktiverat inaktivera hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="c335a-135">If hello VPN device has **Perfect forward Secrecy** enabled, disable hello feature.</span></span> <span data-ttu-id="c335a-136">Sedan [uppdatera hello virtuellt gateway IPsec-princip](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).</span><span class="sxs-lookup"><span data-stu-id="c335a-136">Then [update hello virtual network gateway IPsec policy](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c335a-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c335a-137">Next steps</span></span>

- [<span data-ttu-id="c335a-138">Konfigurera ett virtuellt nätverk för plats-till-plats-anslutning tooa</span><span class="sxs-lookup"><span data-stu-id="c335a-138">Configure a Site-to-Site connection tooa virtual network</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [<span data-ttu-id="c335a-139">Konfigurera IPsec/IKE-princip för plats-till-plats VPN-anslutningar</span><span class="sxs-lookup"><span data-stu-id="c335a-139">Configure IPsec/IKE policy for Site-to-Site VPN connections</span></span>](vpn-gateway-ipsecikepolicy-rm-powershell.md)

