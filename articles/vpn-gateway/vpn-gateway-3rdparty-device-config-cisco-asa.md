---
title: "Exempel på konfiguration - Cisco ASA-enhet som ansluter till Azure VPN-gatewayer | Microsoft Docs"
description: "Den här artikeln innehåller ett exempel på konfiguration för Cisco ASA-enhet som ansluter till Azure VPN-gatewayer."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: 10466b8928e2cd687f7961a2956b6d60823b82be
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a><span data-ttu-id="ffc2f-103">Exempel på konfiguration: Cisco ASA enhet (IKEv2/Nej BGP)</span><span class="sxs-lookup"><span data-stu-id="ffc2f-103">Sample configuration: Cisco ASA device (IKEv2/no BGP)</span></span>
<span data-ttu-id="ffc2f-104">Den här artikeln innehåller exempel konfigurationer för Cisco ASA-enheter som ansluter till Azure VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-104">This article provides sample configurations for Cisco ASA devices connecting to Azure VPN gateways.</span></span>

## <a name="device-at-a-glance"></a><span data-ttu-id="ffc2f-105">Enheten i korthet</span><span class="sxs-lookup"><span data-stu-id="ffc2f-105">Device at a glance</span></span>

|                        |                                   |
| ---                    | ---                               |
| <span data-ttu-id="ffc2f-106">Leverantören av enheten</span><span class="sxs-lookup"><span data-stu-id="ffc2f-106">Device vendor</span></span>          | <span data-ttu-id="ffc2f-107">Cisco</span><span class="sxs-lookup"><span data-stu-id="ffc2f-107">Cisco</span></span>                             |
| <span data-ttu-id="ffc2f-108">Enhetsmodell</span><span class="sxs-lookup"><span data-stu-id="ffc2f-108">Device model</span></span>           | <span data-ttu-id="ffc2f-109">ASA (anpassningsbar postsäkerhet)</span><span class="sxs-lookup"><span data-stu-id="ffc2f-109">ASA (Adaptive Security Appliance)</span></span> |
| <span data-ttu-id="ffc2f-110">Målversionen</span><span class="sxs-lookup"><span data-stu-id="ffc2f-110">Target version</span></span>         | <span data-ttu-id="ffc2f-111">8.4+</span><span class="sxs-lookup"><span data-stu-id="ffc2f-111">8.4+</span></span>                              |
| <span data-ttu-id="ffc2f-112">Testad modellen</span><span class="sxs-lookup"><span data-stu-id="ffc2f-112">Tested model</span></span>           | <span data-ttu-id="ffc2f-113">ASA 5505</span><span class="sxs-lookup"><span data-stu-id="ffc2f-113">ASA 5505</span></span>                          |
| <span data-ttu-id="ffc2f-114">Testad version</span><span class="sxs-lookup"><span data-stu-id="ffc2f-114">Tested version</span></span>         | <span data-ttu-id="ffc2f-115">9.2</span><span class="sxs-lookup"><span data-stu-id="ffc2f-115">9.2</span></span>                               |
| <span data-ttu-id="ffc2f-116">IKE-version</span><span class="sxs-lookup"><span data-stu-id="ffc2f-116">IKE version</span></span>            | <span data-ttu-id="ffc2f-117">**IKEv2**</span><span class="sxs-lookup"><span data-stu-id="ffc2f-117">**IKEv2**</span></span>                         |
| <span data-ttu-id="ffc2f-118">BGP</span><span class="sxs-lookup"><span data-stu-id="ffc2f-118">BGP</span></span>                    | <span data-ttu-id="ffc2f-119">**Nej**</span><span class="sxs-lookup"><span data-stu-id="ffc2f-119">**No**</span></span>                            |
| <span data-ttu-id="ffc2f-120">Azure VPN gateway-typ</span><span class="sxs-lookup"><span data-stu-id="ffc2f-120">Azure VPN gateway type</span></span> | <span data-ttu-id="ffc2f-121">**Ruttbaserad** VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="ffc2f-121">**Route-based** VPN gateway</span></span>       |
|                        |                                   |

> [!NOTE]
> 1. <span data-ttu-id="ffc2f-122">Konfigurationen nedan ansluter en Cisco ASA-enhet till en Azure **ruttbaserad** VPN-gateway med hjälp av anpassade IPsec/IKE-princip med alternativet ”UserPolicyBasedTrafficSelectors”, enligt beskrivningen i [i den här artikeln](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ffc2f-122">The configuration below connects a Cisco ASA device to an Azure **route-based** VPN gateway using custom IPsec/IKE policy with "UserPolicyBasedTrafficSelectors" option, as described in [this article](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span></span>
> 2. <span data-ttu-id="ffc2f-123">Det krävs ASA enheter ska kunna använda **IKEv2** med åtkomst-list-baserade konfigurationer inte VTI-baserade.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-123">It requires ASA devices to use **IKEv2** with access-list-based configurations, not VTI-based.</span></span>
> 3. <span data-ttu-id="ffc2f-124">Kontakta din VPN-leverantör enhetsspecifikationerna så principen stöds på din lokala VPN-enheter.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-124">Please consult with your VPN device vendor specifications to ensure the policy is supported on your on-premises VPN devices.</span></span>

## <a name="vpn-device-requirements"></a><span data-ttu-id="ffc2f-125">Krav för VPN-enhet</span><span class="sxs-lookup"><span data-stu-id="ffc2f-125">VPN device requirements</span></span>
<span data-ttu-id="ffc2f-126">Azure VPN-gatewayer använder standard IPsec/IKE-protokoll-paket för att upprätta S2S VPN-tunnlar.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-126">Azure VPN gateways use standard IPsec/IKE protocol suites to establish S2S VPN tunnels.</span></span> <span data-ttu-id="ffc2f-127">Referera till [om VPN-enheter](vpn-gateway-about-vpn-devices.md) för detaljerad IPsec/IKE-protokollparametrar och standard kryptografiska algoritmer för Azure VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-127">Refer to [About VPN devices](vpn-gateway-about-vpn-devices.md) for the detailed IPsec/IKE protocol parameters and default cryptographic algorithms for Azure VPN gateways.</span></span> <span data-ttu-id="ffc2f-128">Du kan också ange den exakta kombinationen av kryptografiska algoritmer och viktiga fördelar för en viss anslutning enligt beskrivningen i [om krav för kryptografiska](vpn-gateway-about-compliance-crypto.md).</span><span class="sxs-lookup"><span data-stu-id="ffc2f-128">You can optionally specify the exact combination of cryptographic algorithms and key strengths for a specific connection as described in [About cryptographic requirements](vpn-gateway-about-compliance-crypto.md).</span></span> <span data-ttu-id="ffc2f-129">Om du väljer en specifik kombination av kryptografiska algoritmer och viktiga styrkor, kontrollera att du använder de motsvarande specifikationerna på VPN-enheter.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-129">If you select a specific combination of cryptographic algorithms and key strengths, please make sure you use the corresponding specifications on your VPN devices.</span></span>

## <a name="single-vpn-tunnel"></a><span data-ttu-id="ffc2f-130">En VPN-tunnel</span><span class="sxs-lookup"><span data-stu-id="ffc2f-130">Single VPN tunnel</span></span>
<span data-ttu-id="ffc2f-131">Den här topologin består av en S2S VPN-tunnel mellan en Azure VPN-gateway och din lokala VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-131">This topology consists of a single S2S VPN tunnel between an Azure VPN gateway and your on-premises VPN device.</span></span> <span data-ttu-id="ffc2f-132">Du kan också konfigurera BGP över VPN-tunnel.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-132">You can optionally configure BGP across the VPN tunnel.</span></span>

![en tunnel](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

<span data-ttu-id="ffc2f-134">Referera till [en tunnel installationsprogrammet](vpn-gateway-3rdparty-device-config-overview.md#singletunnel) för detaljerad, stegvisa anvisningar att skapa Azure-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-134">Refer to [Single tunnel setup](vpn-gateway-3rdparty-device-config-overview.md#singletunnel) for detailed, step-by-step instructions to build the Azure configurations.</span></span>

### <a name="network-and-vpn-gateway-information"></a><span data-ttu-id="ffc2f-135">Information om nätverk och VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="ffc2f-135">Network and VPN gateway information</span></span>
<span data-ttu-id="ffc2f-136">Det här avsnittet listas parametrarna för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-136">This section list the parameters for the this sample.</span></span>

| <span data-ttu-id="ffc2f-137">**Parametern**</span><span class="sxs-lookup"><span data-stu-id="ffc2f-137">**Parameter**</span></span>                | <span data-ttu-id="ffc2f-138">**Värde**</span><span class="sxs-lookup"><span data-stu-id="ffc2f-138">**Value**</span></span>                    |
| ---                          | ---                          |
| <span data-ttu-id="ffc2f-139">VNet-adressprefixer</span><span class="sxs-lookup"><span data-stu-id="ffc2f-139">VNet address prefixes</span></span>        | <span data-ttu-id="ffc2f-140">10.11.0.0/16</span><span class="sxs-lookup"><span data-stu-id="ffc2f-140">10.11.0.0/16</span></span><br><span data-ttu-id="ffc2f-141">10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="ffc2f-141">10.12.0.0/16</span></span> |
| <span data-ttu-id="ffc2f-142">Azure VPN gateway-IP</span><span class="sxs-lookup"><span data-stu-id="ffc2f-142">Azure VPN gateway IP</span></span>         | <span data-ttu-id="ffc2f-143">Azure_Gateway_Public_IP</span><span class="sxs-lookup"><span data-stu-id="ffc2f-143">Azure_Gateway_Public_IP</span></span>      |
| <span data-ttu-id="ffc2f-144">Lokala adressprefix</span><span class="sxs-lookup"><span data-stu-id="ffc2f-144">On-premises address prefixes</span></span> | <span data-ttu-id="ffc2f-145">10.51.0.0/16</span><span class="sxs-lookup"><span data-stu-id="ffc2f-145">10.51.0.0/16</span></span><br><span data-ttu-id="ffc2f-146">10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="ffc2f-146">10.52.0.0/16</span></span> |
| <span data-ttu-id="ffc2f-147">Lokala VPN-enhetens IP</span><span class="sxs-lookup"><span data-stu-id="ffc2f-147">On-premises VPN device IP</span></span>    | <span data-ttu-id="ffc2f-148">OnPrem_Device_Public_IP</span><span class="sxs-lookup"><span data-stu-id="ffc2f-148">OnPrem_Device_Public_IP</span></span>     |
| <span data-ttu-id="ffc2f-149">* VNet BGP ASN</span><span class="sxs-lookup"><span data-stu-id="ffc2f-149">*VNet BGP ASN</span></span>                | <span data-ttu-id="ffc2f-150">65010</span><span class="sxs-lookup"><span data-stu-id="ffc2f-150">65010</span></span>                        |
| <span data-ttu-id="ffc2f-151">* Azure BGP-peer-IP</span><span class="sxs-lookup"><span data-stu-id="ffc2f-151">*Azure BGP peer IP</span></span>           | <span data-ttu-id="ffc2f-152">10.12.255.30</span><span class="sxs-lookup"><span data-stu-id="ffc2f-152">10.12.255.30</span></span>                 |
| <span data-ttu-id="ffc2f-153">* Lokala BGP ASN</span><span class="sxs-lookup"><span data-stu-id="ffc2f-153">*On-premises BGP ASN</span></span>         | <span data-ttu-id="ffc2f-154">65050</span><span class="sxs-lookup"><span data-stu-id="ffc2f-154">65050</span></span>                        |
| <span data-ttu-id="ffc2f-155">* Lokala BGP-peer-IP</span><span class="sxs-lookup"><span data-stu-id="ffc2f-155">*On-premises BGP peer IP</span></span>     | <span data-ttu-id="ffc2f-156">10.52.255.254</span><span class="sxs-lookup"><span data-stu-id="ffc2f-156">10.52.255.254</span></span>                |
|                              |                              |

* <span data-ttu-id="ffc2f-157">(*) Valfria parametrar för BGP endast.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-157">(*) Optional parameters for BGP only.</span></span>

### <a name="ipsecike-policy--parameters"></a><span data-ttu-id="ffc2f-158">IPSec-/ princip & parametrar</span><span class="sxs-lookup"><span data-stu-id="ffc2f-158">IPsec/IKE policy & parameters</span></span>

<span data-ttu-id="ffc2f-159">I tabellen nedan visas de IPsec/IKE-algoritmer och parametrar som används i exemplet.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-159">The table below lists the IPsec/IKE algorithms and parameters used in the sample.</span></span> <span data-ttu-id="ffc2f-160">Kontakta din VPN-enhetsspecifikationer se till alla algoritmer som anges ovan stöds av din VPN-enhetsmodeller och inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-160">Please consult your VPN device specifications to make sure all algorithms listed above are supported by your VPN device models and firmware versions.</span></span>

| <span data-ttu-id="ffc2f-161">**IPsec/IKEv2**</span><span class="sxs-lookup"><span data-stu-id="ffc2f-161">**IPsec/IKEv2**</span></span>  | <span data-ttu-id="ffc2f-162">**Värde**</span><span class="sxs-lookup"><span data-stu-id="ffc2f-162">**Value**</span></span>                            |
| ---              | ---                                  |
| <span data-ttu-id="ffc2f-163">IKEv2-kryptering</span><span class="sxs-lookup"><span data-stu-id="ffc2f-163">IKEv2 Encryption</span></span> | <span data-ttu-id="ffc2f-164">AES256</span><span class="sxs-lookup"><span data-stu-id="ffc2f-164">AES256</span></span>                               |
| <span data-ttu-id="ffc2f-165">IKEv2 Integrity</span><span class="sxs-lookup"><span data-stu-id="ffc2f-165">IKEv2 Integrity</span></span>  | <span data-ttu-id="ffc2f-166">SHA384</span><span class="sxs-lookup"><span data-stu-id="ffc2f-166">SHA384</span></span>                               |
| <span data-ttu-id="ffc2f-167">DH-grupp</span><span class="sxs-lookup"><span data-stu-id="ffc2f-167">DH Group</span></span>         | <span data-ttu-id="ffc2f-168">DHGroup24</span><span class="sxs-lookup"><span data-stu-id="ffc2f-168">DHGroup24</span></span>                            |
| <span data-ttu-id="ffc2f-169">IPsec-kryptering</span><span class="sxs-lookup"><span data-stu-id="ffc2f-169">IPsec Encryption</span></span> | <span data-ttu-id="ffc2f-170">AES256</span><span class="sxs-lookup"><span data-stu-id="ffc2f-170">AES256</span></span>                               |
| <span data-ttu-id="ffc2f-171">IPsec Integrity</span><span class="sxs-lookup"><span data-stu-id="ffc2f-171">IPsec Integrity</span></span>  | <span data-ttu-id="ffc2f-172">SHA1</span><span class="sxs-lookup"><span data-stu-id="ffc2f-172">SHA1</span></span>                                 |
| <span data-ttu-id="ffc2f-173">PFS-grupp</span><span class="sxs-lookup"><span data-stu-id="ffc2f-173">PFS Group</span></span>        | <span data-ttu-id="ffc2f-174">PFS24</span><span class="sxs-lookup"><span data-stu-id="ffc2f-174">PFS24</span></span>                                |
| <span data-ttu-id="ffc2f-175">QM SA-livstid</span><span class="sxs-lookup"><span data-stu-id="ffc2f-175">QM SA Lifetime</span></span>   | <span data-ttu-id="ffc2f-176">7200 sekunder</span><span class="sxs-lookup"><span data-stu-id="ffc2f-176">7200 seconds</span></span>                         |
| <span data-ttu-id="ffc2f-177">Trafikväljare</span><span class="sxs-lookup"><span data-stu-id="ffc2f-177">Traffic Selector</span></span> | <span data-ttu-id="ffc2f-178">UsePolicyBasedTrafficSelectors $True</span><span class="sxs-lookup"><span data-stu-id="ffc2f-178">UsePolicyBasedTrafficSelectors $True</span></span> |
| <span data-ttu-id="ffc2f-179">I förväg delad nyckel</span><span class="sxs-lookup"><span data-stu-id="ffc2f-179">Pre-Shared Key</span></span>   | <span data-ttu-id="ffc2f-180">PreSharedKey</span><span class="sxs-lookup"><span data-stu-id="ffc2f-180">PreSharedKey</span></span>                         |
|                  |                                      |

- <span data-ttu-id="ffc2f-181">(*) IPSec-integritet måste vara ”null” om GCM-AES används som IPSec-krypteringsalgoritmen på vissa enheter.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-181">(*) On some device, IPsec integrity must be "null" if GCM-AES is used as IPsec encryption algorithm.</span></span>

### <a name="device-notes"></a><span data-ttu-id="ffc2f-182">Anteckningar för enhet</span><span class="sxs-lookup"><span data-stu-id="ffc2f-182">Device notes</span></span>

>[!NOTE]
>
> 1. <span data-ttu-id="ffc2f-183">Stödet för IKEv2 kräver ASA version 8.4 och högre.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-183">IKEv2 support requires ASA version 8.4 and above.</span></span>
> 2. <span data-ttu-id="ffc2f-184">Stöd för högre DH och PFS (utöver gruppen 5) kräver ASA version 9.x.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-184">Higher DH and PFS group support (beyond Group 5) requires ASA version 9.x.</span></span>
> 3. <span data-ttu-id="ffc2f-185">IPSec-kryptering med AES-GCM och IPsec integritet med SHA-256, SHA-384, stöd för SHA-512 kräver ASA version 9.x på nyare ASA maskinvara; 5505 ASA, 5510, 5520, 5540, 5550, 5580 är **inte** stöds.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-185">IPsec encryption with AES-GCM and IPsec integrity with SHA-256, SHA-384, SHA-512 support requires ASA version 9.x on newer ASA hardware; ASA 5505, 5510, 5520, 5540, 5550, 5580 are **not** supported.</span></span> <span data-ttu-id="ffc2f-186">(Kontrollera Leverantörsspecifikationer för att bekräfta.)</span><span class="sxs-lookup"><span data-stu-id="ffc2f-186">(Please check the vendor specifications to confirm.)</span></span>
>


### <a name="sample-device-configurations"></a><span data-ttu-id="ffc2f-187">Exempel enhetskonfigurationer</span><span class="sxs-lookup"><span data-stu-id="ffc2f-187">Sample device configurations</span></span>
<span data-ttu-id="ffc2f-188">Skriptet nedan innehåller en exempelkonfiguration baserat på topologin och parametrarna som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-188">The script below provides a sample configuration based on the topology and parameters listed above.</span></span> <span data-ttu-id="ffc2f-189">S2S VPN-tunnel konfigurationen består av följande delar:</span><span class="sxs-lookup"><span data-stu-id="ffc2f-189">The S2S VPN tunnel configuration consists of the following parts:</span></span>

1. <span data-ttu-id="ffc2f-190">Gränssnitt & vägar</span><span class="sxs-lookup"><span data-stu-id="ffc2f-190">Interfaces & routes</span></span>
2. <span data-ttu-id="ffc2f-191">Listor</span><span class="sxs-lookup"><span data-stu-id="ffc2f-191">Access lists</span></span>
3. <span data-ttu-id="ffc2f-192">IKE-principen och parametrar (fas 1 eller Main-läge)</span><span class="sxs-lookup"><span data-stu-id="ffc2f-192">IKE policy and parameters (Phase 1 or Main Mode)</span></span>
4. <span data-ttu-id="ffc2f-193">IPSec-principen och parametrar (fas 2 eller snabbläge)</span><span class="sxs-lookup"><span data-stu-id="ffc2f-193">IPsec policy and parameters (Phase 2 or Quick Mode)</span></span>
5. <span data-ttu-id="ffc2f-194">Andra parametrar (TCP-MSS minskning, etc.)</span><span class="sxs-lookup"><span data-stu-id="ffc2f-194">Other parameters (TCP MSS clamping, etc.)</span></span>

>[!IMPORTANT] 
><span data-ttu-id="ffc2f-195">Kontrollera att du slutföra ytterligare konfigurationen nedan och ersätta platshållarna med faktiska värden:</span><span class="sxs-lookup"><span data-stu-id="ffc2f-195">Please make sure you complete the additional configuration listed below and replace the placeholders with the actual values:</span></span>
> 
> - <span data-ttu-id="ffc2f-196">Gränssnittskonfiguration för både inom och utanför gränssnitt</span><span class="sxs-lookup"><span data-stu-id="ffc2f-196">Interface configuration for both inside and outside interfaces</span></span>
> - <span data-ttu-id="ffc2f-197">Vägar för din innanför/privat och utanför och offentliga nätverk</span><span class="sxs-lookup"><span data-stu-id="ffc2f-197">Routes for your inside/private and outside/public networks</span></span>
> - <span data-ttu-id="ffc2f-198">Se till att alla namn och nummer är unika på enheten</span><span class="sxs-lookup"><span data-stu-id="ffc2f-198">Ensure all names and policy numbers are unique on the device</span></span>
> - <span data-ttu-id="ffc2f-199">Se till att de kryptografiska algoritmerna stöds på enheten</span><span class="sxs-lookup"><span data-stu-id="ffc2f-199">Ensure the cryptographic algorithms are supported on your device</span></span>
> - <span data-ttu-id="ffc2f-200">Ersätt följande plats innehavare med de faktiska värdena</span><span class="sxs-lookup"><span data-stu-id="ffc2f-200">Replace the following place holders with the actual values</span></span>
>   - <span data-ttu-id="ffc2f-201">Utanför gränssnittsnamn: ”externa”</span><span class="sxs-lookup"><span data-stu-id="ffc2f-201">Outside interface name: "outside"</span></span>
>   - <span data-ttu-id="ffc2f-202">Azure_Gateway_Public_IP</span><span class="sxs-lookup"><span data-stu-id="ffc2f-202">Azure_Gateway_Public_IP</span></span>
>   - <span data-ttu-id="ffc2f-203">OnPrem_Device_Public_IP</span><span class="sxs-lookup"><span data-stu-id="ffc2f-203">OnPrem_Device_Public_IP</span></span>
>   - <span data-ttu-id="ffc2f-204">IKE-Pre_Shared_Key</span><span class="sxs-lookup"><span data-stu-id="ffc2f-204">IKE Pre_Shared_Key</span></span>
>   - <span data-ttu-id="ffc2f-205">Virtuella nätverk och lokala gateway nätverksnamn (VNetName, LNGName)</span><span class="sxs-lookup"><span data-stu-id="ffc2f-205">VNet and local network gateway names (VNetName, LNGName)</span></span>
>   - <span data-ttu-id="ffc2f-206">Virtuella nätverk och lokala nätverks-adressprefix</span><span class="sxs-lookup"><span data-stu-id="ffc2f-206">VNet and on-premises network address prefixes</span></span>
>   - <span data-ttu-id="ffc2f-207">Rätt nätmasker</span><span class="sxs-lookup"><span data-stu-id="ffc2f-207">Proper netmasks</span></span>

#### <a name="sample-configuration"></a><span data-ttu-id="ffc2f-208">Exempel på konfiguration</span><span class="sxs-lookup"><span data-stu-id="ffc2f-208">Sample configuration</span></span>

```
! Sample ASA configuration for connecting to Azure VPN gateway
!
! Tested hardware: ASA 5505
! Tested version:  ASA version 9.2(4)
!
! Replace the following place holders with your actual values:
!   - Interface names - default are "outside" and "inside"
!   - <Azure_Gateway_Public_IP>
!   - <OnPrem_Device_Public_IP>
!   - <Pre_Shared_Key>
!   - <VNetName>*
!   - <LNGName>* ==> LocalNetworkGateway - the Azure resource that represents the
!     on-premises network, specifies network prefixes, device public IP, BGP info, etc.
!   - <PrivateIPAddress> ==> Replace it with a private IP address if applicable
!   - <Netmask> ==> Replace it with appropriate netmasks
!   - <Nexthop> ==> Replace it with the actual nexthop IP address
!
! (*) Must be unique names in the device configuration
!
! ==> Interface & route configurations
!
!     > <OnPrem_Device_Public_IP> address on the outside interface or vlan
!     > <PrivateIPAddress> on the inside interface or vlan; e.g., 10.51.0.1/24
!     > Route to connect to <Azure_Gateway_Public_IP> address
!
!     > Example:
!
!       interface Ethernet0/0
!        switchport access vlan 2
!       exit
!
!       interface vlan 1
!        nameif inside
!        security-level 100
!        ip address <PrivateIPAddress> <Netmask>
!       exit
!
!       interface vlan 2
!        nameif outside
!        security-level 0
!        ip address <OnPrem_Device_Public_IP> <Netmask>
!       exit
!
!       route outside 0.0.0.0 0.0.0.0 <NextHop IP> 1
!
! ==> Access lists
!
!     > Most firewall devices deny all traffic by default. Create access lists to
!       (1) Allow S2S VPN tunnels between the ASA and the Azure gateway public IP address
!       (2) Construct traffic selectors as part of IPsec policy or proposal
!
access-list outside_access_in extended permit ip host <Azure_Gateway_Public_IP> host <OnPrem_Device_Public_IP>
!
!     > Object group that consists of all VNet prefixes (e.g., 10.11.0.0/16 &
!       10.12.0.0/16)
!
object-group network Azure-<VNetName>
 description Azure virtual network <VNetName> prefixes
 network-object 10.11.0.0 255.255.0.0
 network-object 10.12.0.0 255.255.0.0
exit
!
!     > Object group that corresponding to the <LNGName> prefixes.
!       E.g., 10.51.0.0/16 and 10.52.0.0/16. Note that LNG = "local network gateway".
!       In Azure network resource, a local network gateway defines the on-premises
!       network properties (address prefixes, VPN device IP, BGP ASN, etc.)
!
object-group network <LNGName>
 description On-Premises network <LNGName> prefixes
 network-object 10.51.0.0 255.255.0.0
 network-object 10.52.0.0 255.255.0.0
exit
!
!     > Specify the access-list between the Azure VNet and your on-premises network.
!       This access list defines the IPsec SA traffic selectors.
!
access-list Azure-<VNetName>-acl extended permit ip object-group <LNGName> object-group Azure-<VNetName>
!
!     > No NAT required between the on-premises network and Azure VNet
!
nat (inside,outside) source static <LNGName> <LNGName> destination static Azure-<VNetName> Azure-<VNetName>
!
! ==> IKEv2 configuration
!
!     > General IKEv2 configuration - enable IKEv2 for VPN
!
group-policy DfltGrpPolicy attributes
 vpn-tunnel-protocol ikev1 ikev2
exit
!
crypto isakmp identity address
crypto ikev2 enable outside
!
!     > Define IKEv2 Phase 1/Main Mode policy
!       - Make sure the policy number is not used
!       - integrity and prf must be the same
!       - DH group 14 and above require ASA version 9.x.
!
crypto ikev2 policy 1
 encryption       aes-256
 integrity        sha384
 prf              sha384
 group            24
 lifetime seconds 86400
exit
!
!     > Set connection type and pre-shared key
!
tunnel-group <Azure_Gateway_Public_IP> type ipsec-l2l
tunnel-group <Azure_Gateway_Public_IP> ipsec-attributes
 ikev2 remote-authentication pre-shared-key <Pre_Shared_Key> 
 ikev2 local-authentication  pre-shared-key <Pre_Shared_Key> 
exit
!
! ==> IPsec configuration
!
!     > IKEv2 Phase 2/Quick Mode proposal
!       - AES-GCM and SHA-2 requires ASA version 9.x on newer ASA models. ASA
!         5505, 5510, 5520, 5540, 5550, 5580 are not supported.
!       - ESP integrity must be null if AES-GCM is configured as ESP encryption
!
crypto ipsec ikev2 ipsec-proposal AES-256
 protocol esp encryption aes-256
 protocol esp integrity  sha-1
exit
!
!     > Set access list & traffic selectors, PFS, IPsec protposal, SA lifetime
!       - This sample uses "Azure-<VNetName>-map" as the crypto map name
!       - ASA supports only one crypto map per interface, if you already have
!         an existing crypto map assigned to your outside interface, you must use
!         the same crypto map name, but with a different sequence number for
!         this policy
!       - "match address" policy uses the access-list "Azure-<VNetName>-acl" defined 
!         previously
!       - "ipsec-proposal" uses the proposal "AES-256" defined previously 
!       - PFS groups 14 and beyond requires ASA version 9.x.
!
crypto map Azure-<VNetName>-map 1 match address Azure-<VNetName>-acl
crypto map Azure-<VNetName>-map 1 set pfs group24
crypto map Azure-<VNetName>-map 1 set peer <Azure_Gateway_Public_IP>
crypto map Azure-<VNetName>-map 1 set ikev2 ipsec-proposal AES-256
crypto map Azure-<VNetName>-map 1 set security-association lifetime seconds 7200
crypto map Azure-<VNetName>-map interface outside
!
! ==> Set TCP MSS to 1350
!
sysopt connection tcpmss 1350
!
```

## <a name="simple-debugging-commands"></a><span data-ttu-id="ffc2f-209">Enkla kommandon för felsökning</span><span class="sxs-lookup"><span data-stu-id="ffc2f-209">Simple debugging commands</span></span>

<span data-ttu-id="ffc2f-210">Här följer några ASA-kommandon för felsökning:</span><span class="sxs-lookup"><span data-stu-id="ffc2f-210">Here are some ASA commands for debugging purposes:</span></span>

1. <span data-ttu-id="ffc2f-211">Visa IPsec- och IKE SA</span><span class="sxs-lookup"><span data-stu-id="ffc2f-211">Show the IPsec and IKE SA's</span></span>
    - <span data-ttu-id="ffc2f-212">”Visa kryptografi ipsec sa”</span><span class="sxs-lookup"><span data-stu-id="ffc2f-212">"show crypto ipsec sa"</span></span>
    - <span data-ttu-id="ffc2f-213">”Visa crypto ikev2 sa”</span><span class="sxs-lookup"><span data-stu-id="ffc2f-213">"show crypto ikev2 sa"</span></span>
2. <span data-ttu-id="ffc2f-214">Anger felsökningsläge - detta kan få väldigt mycket brus på konsolen</span><span class="sxs-lookup"><span data-stu-id="ffc2f-214">Entering debug mode - this can get very noisy on the console</span></span>
    - <span data-ttu-id="ffc2f-215">”debug kryptografi ikev2 plattform <level>”</span><span class="sxs-lookup"><span data-stu-id="ffc2f-215">"debug crypto ikev2 platform <level>"</span></span>
    - <span data-ttu-id="ffc2f-216">”Felsöka kryptografi ikev2-protokoll <level>”</span><span class="sxs-lookup"><span data-stu-id="ffc2f-216">"debug crypto ikev2 protocol <level>"</span></span>
3. <span data-ttu-id="ffc2f-217">Lista över aktuella konfigurationer</span><span class="sxs-lookup"><span data-stu-id="ffc2f-217">List current configurations</span></span>
    - <span data-ttu-id="ffc2f-218">”Visa kör” - visar aktuella konfigurationen på enhet; Du kan använda olika underordnade kommandon för att lista vissa delar av konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-218">"show run" - shows the current configurations on the device; you can use the various sub-commands to list specific parts of the configuration.</span></span> <span data-ttu-id="ffc2f-219">Till exempel ”visa kör crypto”, ”visa kör åtkomstlista”, ”visa kör tunnel-grupp” osv.</span><span class="sxs-lookup"><span data-stu-id="ffc2f-219">E.g., "show run crypto", "show run access-list", "show run tunnel-group", etc.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ffc2f-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ffc2f-220">Next steps</span></span>
<span data-ttu-id="ffc2f-221">Anvisningar om hur du konfigurerar aktiv-aktiv VPN-gateways för flera platser och VNet-till-VNet-anslutningar finns i [Konfigurera aktiv-aktiv VPN-gateways för flera platser och VNet-till-VNet-anslutningar](vpn-gateway-activeactive-rm-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="ffc2f-221">See [Configuring Active-Active VPN Gateways for Cross-Premises and VNet-to-VNet Connections](vpn-gateway-activeactive-rm-powershell.md) for steps to configure active-active cross-premises and VNet-to-VNet connections.</span></span>

