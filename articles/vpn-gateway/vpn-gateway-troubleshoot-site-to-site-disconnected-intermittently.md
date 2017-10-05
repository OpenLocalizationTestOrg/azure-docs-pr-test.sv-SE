---
title: "Felsöka Azure plats-till-plats VPN kopplar från periodvis | Microsoft Docs"
description: "Lär dig att felsöka problemet plats-till-plats VPN-anslutningen frånkopplad regelbundet."
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
ms.openlocfilehash: 99a790617baa65116bfba976cd9279627e8775f3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-azure-site-to-site-vpn-disconnects-intermittently"></a><span data-ttu-id="a00fe-103">Felsöka: Azure plats-till-plats-VPN kopplar från periodvis</span><span class="sxs-lookup"><span data-stu-id="a00fe-103">Troubleshooting: Azure Site-to-Site VPN disconnects intermittently</span></span>

<span data-ttu-id="a00fe-104">Du kan få problem att en ny eller befintlig Microsoft Azure punkt-till-plats VPN-anslutning inte är stabilt eller kopplar från regelbundet.</span><span class="sxs-lookup"><span data-stu-id="a00fe-104">You might experience the problem that a new or existing Microsoft Azure Point-to-Site VPN connection is not stable or disconnects regularly.</span></span> <span data-ttu-id="a00fe-105">Den här artikeln innehåller felsökning som hjälper dig att identifiera och åtgärda orsaken till problemet.</span><span class="sxs-lookup"><span data-stu-id="a00fe-105">This article provides troubleshoot steps to help you identify and resolve the cause of the problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a><span data-ttu-id="a00fe-106">Felsökningssteg</span><span class="sxs-lookup"><span data-stu-id="a00fe-106">Troubleshooting steps</span></span>

### <a name="prerequisite-step"></a><span data-ttu-id="a00fe-107">Innan du börjar:</span><span class="sxs-lookup"><span data-stu-id="a00fe-107">Prerequisite step</span></span>

<span data-ttu-id="a00fe-108">Kontrollera vilken typ av Azure virtuella nätverkets gateway:</span><span class="sxs-lookup"><span data-stu-id="a00fe-108">Check the type of Azure  virtual network gateway:</span></span>

1. <span data-ttu-id="a00fe-109">Gå till [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a00fe-109">Go to [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a00fe-110">Kontrollera den **översikt** sida av den virtuella nätverksgatewayen för informationen.</span><span class="sxs-lookup"><span data-stu-id="a00fe-110">Check the **Overview** page of the virtual network gateway for the type information.</span></span>
    
    ![Översikt över gatewayen](media\vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently\gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a><span data-ttu-id="a00fe-112">Steg 1 Kontrollera om lokala VPN-enhet har verifierats</span><span class="sxs-lookup"><span data-stu-id="a00fe-112">Step 1 Check whether the on-premises VPN device is validated</span></span>

1. <span data-ttu-id="a00fe-113">Kontrollera om du använder en [verifiera VPN-enhet och operativsystemversion](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="a00fe-113">Check whether you are using a [validated VPN device and operating system version](vpn-gateway-about-vpn-devices.md#devicetable).</span></span> <span data-ttu-id="a00fe-114">Om VPN-enheten inte har verifierats, kan du behöva kontakta tillverkaren av enheten för att se om det finns några kompatibilitetsproblem.</span><span class="sxs-lookup"><span data-stu-id="a00fe-114">If the VPN device is not validated, you may have to contact the device manufacturer to see if there is any compatibility issue.</span></span>
2. <span data-ttu-id="a00fe-115">Kontrollera att VPN-enheten är korrekt konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="a00fe-115">Make sure that the VPN device is correctly configured.</span></span> <span data-ttu-id="a00fe-116">Mer information finns i [redigera enheten configuration exempel](vpn-gateway-about-vpn-devices.md#editing).</span><span class="sxs-lookup"><span data-stu-id="a00fe-116">For more information, see [Editing device configuration samples](vpn-gateway-about-vpn-devices.md#editing).</span></span>

### <a name="step-2-check-the-security-association-settingsfor-policy-based-azure-virtual-network-gateways"></a><span data-ttu-id="a00fe-117">Steg 2 Kontrollera inställningarna för säkerhetsassociation (för gruppolicy-baserad Azure virtuella nätverksgatewayer)</span><span class="sxs-lookup"><span data-stu-id="a00fe-117">Step 2 Check the Security Association settings(for policy-based Azure virtual network gateways)</span></span>

1. <span data-ttu-id="a00fe-118">Se till att det virtuella nätverket, undernät och områden i den **lokal nätverksgateway** definition i Microsoft Azure är samma som konfigurationen på den lokala VPN-enheten.</span><span class="sxs-lookup"><span data-stu-id="a00fe-118">Make sure that the virtual network, subnets and, ranges in the **Local network gateway** definition in Microsoft Azure are same as the configuration on the on-premises VPN device.</span></span>
2. <span data-ttu-id="a00fe-119">Kontrollera att säkerhetsassociation stämmer.</span><span class="sxs-lookup"><span data-stu-id="a00fe-119">Verify that the Security Association settings match.</span></span>

### <a name="step-3-check-for-user-defined-routes-or-network-security-groups-on-gateway-subnet"></a><span data-ttu-id="a00fe-120">Steg 3-kontroll för användardefinierade vägar eller Nätverkssäkerhetsgrupper på Gateway-undernät</span><span class="sxs-lookup"><span data-stu-id="a00fe-120">Step 3 Check for User-Defined Routes or Network Security Groups on Gateway Subnet</span></span>

<span data-ttu-id="a00fe-121">En användardefinierad väg på gateway-undernätet kan begränsa viss trafik och tillåta andra trafik.</span><span class="sxs-lookup"><span data-stu-id="a00fe-121">A user-defined route on the gateway subnet may be restricting some traffic and allowing other traffic.</span></span> <span data-ttu-id="a00fe-122">Detta gör det visas att VPN-anslutningen är instabilt för viss trafik och bra för andra.</span><span class="sxs-lookup"><span data-stu-id="a00fe-122">This makes it appear that the VPN connection is unreliable for some traffic and good for others.</span></span> 

### <a name="step-4-check-the-one-vpn-tunnel-per-subnet-pair-setting-for-policy-based-virtual-network-gateways"></a><span data-ttu-id="a00fe-123">Steg 4 Kontrollera ”en VPN-tunneln undernät par” (för principbaserad virtuella nätverks-gateway)</span><span class="sxs-lookup"><span data-stu-id="a00fe-123">Step 4 Check the "one VPN Tunnel per Subnet Pair" setting (for policy-based virtual network gateways)</span></span>

<span data-ttu-id="a00fe-124">Se till att den lokala VPN-enheten har **en VPN-tunnel undernät par** för principbaserad virtuella nätverksgatewayer.</span><span class="sxs-lookup"><span data-stu-id="a00fe-124">Make sure that the on-premises VPN device is set to have **one VPN tunnel per subnet pair** for policy-based virtual network gateways.</span></span>

### <a name="step-5-check-for-security-association-limitation-for-policy-based-virtual-network-gateways"></a><span data-ttu-id="a00fe-125">Steg 5 Kontrollera säkerhet Association begränsningen (för principbaserad virtuella nätverksgatewayer)</span><span class="sxs-lookup"><span data-stu-id="a00fe-125">Step 5 Check for Security Association Limitation (for policy-based virtual network gateways)</span></span>

<span data-ttu-id="a00fe-126">Policy-baserad virtuell nätverksgateway har högst 200 undernät säkerhetsassociation par.</span><span class="sxs-lookup"><span data-stu-id="a00fe-126">The Policy-based virtual network gateway has limit of 200 subnet Security Association pairs.</span></span> <span data-ttu-id="a00fe-127">Om antalet virtuella Azure-nätverket undernät multiplicerat med antalet lokala undernät är större än 200 ser du sporadiska undernät som kopplar från.</span><span class="sxs-lookup"><span data-stu-id="a00fe-127">If the number of Azure virtual network subnets multiplied times by the number of local subnets is greater than 200, you see sporadic subnets disconnecting.</span></span>

### <a name="step-6-check-on-premises-vpn-device-external-interface-address"></a><span data-ttu-id="a00fe-128">Steg 6 Kontrollera lokalt externa gränssnittet för VPN-enhetsadress</span><span class="sxs-lookup"><span data-stu-id="a00fe-128">Step 6 Check on-premises VPN device external interface address</span></span>

- <span data-ttu-id="a00fe-129">Om IP-adressen för VPN-enhet för Internetuppkopplad ingår i den **lokal nätverksgateway** definition i Azure, sporadiska frånkopplingar kan uppstå.</span><span class="sxs-lookup"><span data-stu-id="a00fe-129">If the Internet facing IP address of the VPN device is included in the **Local network gateway** definition in Azure, you may experience sporadic disconnections.</span></span>
- <span data-ttu-id="a00fe-130">Enhetens externa gränssnittet måste vara direkt på Internet.</span><span class="sxs-lookup"><span data-stu-id="a00fe-130">The device's external interface must be directly on the Internet.</span></span> <span data-ttu-id="a00fe-131">Det bör finnas några NAT (Network Address Translation) eller en brandvägg mellan Internet och enheten.</span><span class="sxs-lookup"><span data-stu-id="a00fe-131">There should be no Network Address Translation (NAT) or firewall between the Internet and the device.</span></span>
-  <span data-ttu-id="a00fe-132">Om du konfigurerar brandväggen kluster om du vill att en virtuell IP-adress, måste du bryta klustret och exponera VPN-enhet direkt till ett offentligt gränssnitt som gatewayen kan samverka med.</span><span class="sxs-lookup"><span data-stu-id="a00fe-132">If you configure Firewall Clustering to have a virtual IP, you must break the cluster and expose the VPN appliance directly to a public interface that the gateway can interface with.</span></span>

### <a name="step-7-check-whether-the-on-premises-vpn-device-has-perfect-forward-secrecy-enabled"></a><span data-ttu-id="a00fe-133">Steg 7 Kontrollera om lokala VPN-enhet har Perfect Forward Secrecy aktiverad</span><span class="sxs-lookup"><span data-stu-id="a00fe-133">Step 7 Check whether the on-premises VPN device has Perfect Forward Secrecy enabled</span></span>

<span data-ttu-id="a00fe-134">Den **Perfect Forward Secrecy** funktion kan orsaka frånkoppling problem.</span><span class="sxs-lookup"><span data-stu-id="a00fe-134">The **Perfect Forward Secrecy** feature can cause the disconnection problems.</span></span> <span data-ttu-id="a00fe-135">Om VPN-enhet har **perfekt forward Secrecy** aktiverat, inaktiveras funktionen.</span><span class="sxs-lookup"><span data-stu-id="a00fe-135">If the VPN device has **Perfect forward Secrecy** enabled, disable the feature.</span></span> <span data-ttu-id="a00fe-136">Sedan [uppdatera den virtuella nätverksgatewayen IPsec-princip](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).</span><span class="sxs-lookup"><span data-stu-id="a00fe-136">Then [update the virtual network gateway IPsec policy](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a00fe-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a00fe-137">Next steps</span></span>

- [<span data-ttu-id="a00fe-138">Konfigurera en plats-till-plats-anslutning till ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="a00fe-138">Configure a Site-to-Site connection to a virtual network</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [<span data-ttu-id="a00fe-139">Konfigurera IPsec/IKE-princip för plats-till-plats VPN-anslutningar</span><span class="sxs-lookup"><span data-stu-id="a00fe-139">Configure IPsec/IKE policy for Site-to-Site VPN connections</span></span>](vpn-gateway-ipsecikepolicy-rm-powershell.md)

