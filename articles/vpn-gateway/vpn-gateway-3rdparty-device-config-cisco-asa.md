---
title: "aaaSample-konfiguration – Cisco ASA-enhet som ansluter tooAzure VPN-gatewayer | Microsoft Docs"
description: "Den här artikeln innehåller ett exempel på konfiguration för Cisco ASA-enhet som ansluter tooAzure VPN-gatewayer."
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
ms.openlocfilehash: dad13e02afe8dad2379db750eb09602e08e8ea99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a><span data-ttu-id="0ec98-103">Exempel på konfiguration: Cisco ASA enhet (IKEv2/Nej BGP)</span><span class="sxs-lookup"><span data-stu-id="0ec98-103">Sample configuration: Cisco ASA device (IKEv2/no BGP)</span></span>
<span data-ttu-id="0ec98-104">Den här artikeln innehåller exempel konfigurationer för Cisco ASA-enheter som ansluter tooAzure VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="0ec98-104">This article provides sample configurations for Cisco ASA devices connecting tooAzure VPN gateways.</span></span>

## <a name="device-at-a-glance"></a><span data-ttu-id="0ec98-105">Enheten i korthet</span><span class="sxs-lookup"><span data-stu-id="0ec98-105">Device at a glance</span></span>

|                        |                                   |
| ---                    | ---                               |
| <span data-ttu-id="0ec98-106">Leverantören av enheten</span><span class="sxs-lookup"><span data-stu-id="0ec98-106">Device vendor</span></span>          | <span data-ttu-id="0ec98-107">Cisco</span><span class="sxs-lookup"><span data-stu-id="0ec98-107">Cisco</span></span>                             |
| <span data-ttu-id="0ec98-108">Enhetsmodell</span><span class="sxs-lookup"><span data-stu-id="0ec98-108">Device model</span></span>           | <span data-ttu-id="0ec98-109">ASA (anpassningsbar postsäkerhet)</span><span class="sxs-lookup"><span data-stu-id="0ec98-109">ASA (Adaptive Security Appliance)</span></span> |
| <span data-ttu-id="0ec98-110">Målversionen</span><span class="sxs-lookup"><span data-stu-id="0ec98-110">Target version</span></span>         | <span data-ttu-id="0ec98-111">8.4+</span><span class="sxs-lookup"><span data-stu-id="0ec98-111">8.4+</span></span>                              |
| <span data-ttu-id="0ec98-112">Testad modellen</span><span class="sxs-lookup"><span data-stu-id="0ec98-112">Tested model</span></span>           | <span data-ttu-id="0ec98-113">ASA 5505</span><span class="sxs-lookup"><span data-stu-id="0ec98-113">ASA 5505</span></span>                          |
| <span data-ttu-id="0ec98-114">Testad version</span><span class="sxs-lookup"><span data-stu-id="0ec98-114">Tested version</span></span>         | <span data-ttu-id="0ec98-115">9.2</span><span class="sxs-lookup"><span data-stu-id="0ec98-115">9.2</span></span>                               |
| <span data-ttu-id="0ec98-116">IKE-version</span><span class="sxs-lookup"><span data-stu-id="0ec98-116">IKE version</span></span>            | <span data-ttu-id="0ec98-117">**IKEv2**</span><span class="sxs-lookup"><span data-stu-id="0ec98-117">**IKEv2**</span></span>                         |
| <span data-ttu-id="0ec98-118">BGP</span><span class="sxs-lookup"><span data-stu-id="0ec98-118">BGP</span></span>                    | <span data-ttu-id="0ec98-119">**Nej**</span><span class="sxs-lookup"><span data-stu-id="0ec98-119">**No**</span></span>                            |
| <span data-ttu-id="0ec98-120">Azure VPN gateway-typ</span><span class="sxs-lookup"><span data-stu-id="0ec98-120">Azure VPN gateway type</span></span> | <span data-ttu-id="0ec98-121">**Ruttbaserad** VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="0ec98-121">**Route-based** VPN gateway</span></span>       |
|                        |                                   |

> [!NOTE]
> 1. <span data-ttu-id="0ec98-122">hello configuration nedan ansluter en Cisco ASA enheten tooan Azure **ruttbaserad** VPN-gateway med hjälp av anpassade IPsec/IKE-princip med alternativet ”UserPolicyBasedTrafficSelectors”, enligt beskrivningen i [i den här artikeln](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="0ec98-122">hello configuration below connects a Cisco ASA device tooan Azure **route-based** VPN gateway using custom IPsec/IKE policy with "UserPolicyBasedTrafficSelectors" option, as described in [this article](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span></span>
> 2. <span data-ttu-id="0ec98-123">Det krävs ASA enheter toouse **IKEv2** med åtkomst-list-baserade konfigurationer inte VTI-baserade.</span><span class="sxs-lookup"><span data-stu-id="0ec98-123">It requires ASA devices toouse **IKEv2** with access-list-based configurations, not VTI-based.</span></span>
> 3. <span data-ttu-id="0ec98-124">Kontakta din VPN-leverantör enhetsspecifikationerna tooensure hello principen stöds på din lokala VPN-enheter.</span><span class="sxs-lookup"><span data-stu-id="0ec98-124">Please consult with your VPN device vendor specifications tooensure hello policy is supported on your on-premises VPN devices.</span></span>

## <a name="vpn-device-requirements"></a><span data-ttu-id="0ec98-125">Krav för VPN-enhet</span><span class="sxs-lookup"><span data-stu-id="0ec98-125">VPN device requirements</span></span>
<span data-ttu-id="0ec98-126">Azure VPN-gatewayer Använd standard IPsec/IKE-protokoll-paket tooestablish S2S VPN-tunnlar.</span><span class="sxs-lookup"><span data-stu-id="0ec98-126">Azure VPN gateways use standard IPsec/IKE protocol suites tooestablish S2S VPN tunnels.</span></span> <span data-ttu-id="0ec98-127">Se för[om VPN-enheter](vpn-gateway-about-vpn-devices.md) hello mer IPsec/IKE protokollparametrar och standard kryptografiska algoritmer för Azure VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="0ec98-127">Refer too[About VPN devices](vpn-gateway-about-vpn-devices.md) for hello detailed IPsec/IKE protocol parameters and default cryptographic algorithms for Azure VPN gateways.</span></span> <span data-ttu-id="0ec98-128">Du kan också ange hello exakta kombinationen av kryptografiska algoritmer och viktiga fördelar för en viss anslutning enligt beskrivningen i [om krav för kryptografiska](vpn-gateway-about-compliance-crypto.md).</span><span class="sxs-lookup"><span data-stu-id="0ec98-128">You can optionally specify hello exact combination of cryptographic algorithms and key strengths for a specific connection as described in [About cryptographic requirements](vpn-gateway-about-compliance-crypto.md).</span></span> <span data-ttu-id="0ec98-129">Om du väljer en specifik kombination av kryptografiska algoritmer och viktiga styrkor, kontrollera att du använder hello motsvarande specifikationer på VPN-enheter.</span><span class="sxs-lookup"><span data-stu-id="0ec98-129">If you select a specific combination of cryptographic algorithms and key strengths, please make sure you use hello corresponding specifications on your VPN devices.</span></span>

## <a name="single-vpn-tunnel"></a><span data-ttu-id="0ec98-130">En VPN-tunnel</span><span class="sxs-lookup"><span data-stu-id="0ec98-130">Single VPN tunnel</span></span>
<span data-ttu-id="0ec98-131">Den här topologin består av en S2S VPN-tunnel mellan en Azure VPN-gateway och din lokala VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="0ec98-131">This topology consists of a single S2S VPN tunnel between an Azure VPN gateway and your on-premises VPN device.</span></span> <span data-ttu-id="0ec98-132">Du kan också konfigurera BGP över hello VPN-tunnel.</span><span class="sxs-lookup"><span data-stu-id="0ec98-132">You can optionally configure BGP across hello VPN tunnel.</span></span>

![en tunnel](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

<span data-ttu-id="0ec98-134">Se för[en tunnel installationsprogrammet](vpn-gateway-3rdparty-device-config-overview.md#singletunnel) detaljerade stegvisa anvisningar toobuild hello Azure-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="0ec98-134">Refer too[Single tunnel setup](vpn-gateway-3rdparty-device-config-overview.md#singletunnel) for detailed, step-by-step instructions toobuild hello Azure configurations.</span></span>

### <a name="network-and-vpn-gateway-information"></a><span data-ttu-id="0ec98-135">Information om nätverk och VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="0ec98-135">Network and VPN gateway information</span></span>
<span data-ttu-id="0ec98-136">Det här avsnittet listas hello parametrar för hello det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="0ec98-136">This section list hello parameters for hello this sample.</span></span>

| <span data-ttu-id="0ec98-137">**Parametern**</span><span class="sxs-lookup"><span data-stu-id="0ec98-137">**Parameter**</span></span>                | <span data-ttu-id="0ec98-138">**Värde**</span><span class="sxs-lookup"><span data-stu-id="0ec98-138">**Value**</span></span>                    |
| ---                          | ---                          |
| <span data-ttu-id="0ec98-139">VNet-adressprefixer</span><span class="sxs-lookup"><span data-stu-id="0ec98-139">VNet address prefixes</span></span>        | <span data-ttu-id="0ec98-140">10.11.0.0/16</span><span class="sxs-lookup"><span data-stu-id="0ec98-140">10.11.0.0/16</span></span><br><span data-ttu-id="0ec98-141">10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="0ec98-141">10.12.0.0/16</span></span> |
| <span data-ttu-id="0ec98-142">Azure VPN gateway-IP</span><span class="sxs-lookup"><span data-stu-id="0ec98-142">Azure VPN gateway IP</span></span>         | <span data-ttu-id="0ec98-143">Azure_Gateway_Public_IP</span><span class="sxs-lookup"><span data-stu-id="0ec98-143">Azure_Gateway_Public_IP</span></span>      |
| <span data-ttu-id="0ec98-144">Lokala adressprefix</span><span class="sxs-lookup"><span data-stu-id="0ec98-144">On-premises address prefixes</span></span> | <span data-ttu-id="0ec98-145">10.51.0.0/16</span><span class="sxs-lookup"><span data-stu-id="0ec98-145">10.51.0.0/16</span></span><br><span data-ttu-id="0ec98-146">10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="0ec98-146">10.52.0.0/16</span></span> |
| <span data-ttu-id="0ec98-147">Lokala VPN-enhetens IP</span><span class="sxs-lookup"><span data-stu-id="0ec98-147">On-premises VPN device IP</span></span>    | <span data-ttu-id="0ec98-148">OnPrem_Device_Public_IP</span><span class="sxs-lookup"><span data-stu-id="0ec98-148">OnPrem_Device_Public_IP</span></span>     |
| <span data-ttu-id="0ec98-149">* VNet BGP ASN</span><span class="sxs-lookup"><span data-stu-id="0ec98-149">*VNet BGP ASN</span></span>                | <span data-ttu-id="0ec98-150">65010</span><span class="sxs-lookup"><span data-stu-id="0ec98-150">65010</span></span>                        |
| <span data-ttu-id="0ec98-151">* Azure BGP-peer-IP</span><span class="sxs-lookup"><span data-stu-id="0ec98-151">*Azure BGP peer IP</span></span>           | <span data-ttu-id="0ec98-152">10.12.255.30</span><span class="sxs-lookup"><span data-stu-id="0ec98-152">10.12.255.30</span></span>                 |
| <span data-ttu-id="0ec98-153">* Lokala BGP ASN</span><span class="sxs-lookup"><span data-stu-id="0ec98-153">*On-premises BGP ASN</span></span>         | <span data-ttu-id="0ec98-154">65050</span><span class="sxs-lookup"><span data-stu-id="0ec98-154">65050</span></span>                        |
| <span data-ttu-id="0ec98-155">* Lokala BGP-peer-IP</span><span class="sxs-lookup"><span data-stu-id="0ec98-155">*On-premises BGP peer IP</span></span>     | <span data-ttu-id="0ec98-156">10.52.255.254</span><span class="sxs-lookup"><span data-stu-id="0ec98-156">10.52.255.254</span></span>                |
|                              |                              |

* <span data-ttu-id="0ec98-157">(*) Valfria parametrar för BGP endast.</span><span class="sxs-lookup"><span data-stu-id="0ec98-157">(*) Optional parameters for BGP only.</span></span>

### <a name="ipsecike-policy--parameters"></a><span data-ttu-id="0ec98-158">IPSec-/ princip & parametrar</span><span class="sxs-lookup"><span data-stu-id="0ec98-158">IPsec/IKE policy & parameters</span></span>

<span data-ttu-id="0ec98-159">hello tabellen nedan visar hello IPsec/IKE algoritmer och parametrar som används i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="0ec98-159">hello table below lists hello IPsec/IKE algorithms and parameters used in hello sample.</span></span> <span data-ttu-id="0ec98-160">Kontakta din VPN-enhet specifikationer toomake att alla algoritmer som anges ovan stöds av din VPN-enhetsmodeller och inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="0ec98-160">Please consult your VPN device specifications toomake sure all algorithms listed above are supported by your VPN device models and firmware versions.</span></span>

| <span data-ttu-id="0ec98-161">**IPsec/IKEv2**</span><span class="sxs-lookup"><span data-stu-id="0ec98-161">**IPsec/IKEv2**</span></span>  | <span data-ttu-id="0ec98-162">**Värde**</span><span class="sxs-lookup"><span data-stu-id="0ec98-162">**Value**</span></span>                            |
| ---              | ---                                  |
| <span data-ttu-id="0ec98-163">IKEv2-kryptering</span><span class="sxs-lookup"><span data-stu-id="0ec98-163">IKEv2 Encryption</span></span> | <span data-ttu-id="0ec98-164">AES256</span><span class="sxs-lookup"><span data-stu-id="0ec98-164">AES256</span></span>                               |
| <span data-ttu-id="0ec98-165">IKEv2 Integrity</span><span class="sxs-lookup"><span data-stu-id="0ec98-165">IKEv2 Integrity</span></span>  | <span data-ttu-id="0ec98-166">SHA384</span><span class="sxs-lookup"><span data-stu-id="0ec98-166">SHA384</span></span>                               |
| <span data-ttu-id="0ec98-167">DH-grupp</span><span class="sxs-lookup"><span data-stu-id="0ec98-167">DH Group</span></span>         | <span data-ttu-id="0ec98-168">DHGroup24</span><span class="sxs-lookup"><span data-stu-id="0ec98-168">DHGroup24</span></span>                            |
| <span data-ttu-id="0ec98-169">IPsec-kryptering</span><span class="sxs-lookup"><span data-stu-id="0ec98-169">IPsec Encryption</span></span> | <span data-ttu-id="0ec98-170">AES256</span><span class="sxs-lookup"><span data-stu-id="0ec98-170">AES256</span></span>                               |
| <span data-ttu-id="0ec98-171">IPsec Integrity</span><span class="sxs-lookup"><span data-stu-id="0ec98-171">IPsec Integrity</span></span>  | <span data-ttu-id="0ec98-172">SHA1</span><span class="sxs-lookup"><span data-stu-id="0ec98-172">SHA1</span></span>                                 |
| <span data-ttu-id="0ec98-173">PFS-grupp</span><span class="sxs-lookup"><span data-stu-id="0ec98-173">PFS Group</span></span>        | <span data-ttu-id="0ec98-174">PFS24</span><span class="sxs-lookup"><span data-stu-id="0ec98-174">PFS24</span></span>                                |
| <span data-ttu-id="0ec98-175">QM SA-livstid</span><span class="sxs-lookup"><span data-stu-id="0ec98-175">QM SA Lifetime</span></span>   | <span data-ttu-id="0ec98-176">7200 sekunder</span><span class="sxs-lookup"><span data-stu-id="0ec98-176">7200 seconds</span></span>                         |
| <span data-ttu-id="0ec98-177">Trafikväljare</span><span class="sxs-lookup"><span data-stu-id="0ec98-177">Traffic Selector</span></span> | <span data-ttu-id="0ec98-178">UsePolicyBasedTrafficSelectors $True</span><span class="sxs-lookup"><span data-stu-id="0ec98-178">UsePolicyBasedTrafficSelectors $True</span></span> |
| <span data-ttu-id="0ec98-179">I förväg delad nyckel</span><span class="sxs-lookup"><span data-stu-id="0ec98-179">Pre-Shared Key</span></span>   | <span data-ttu-id="0ec98-180">PreSharedKey</span><span class="sxs-lookup"><span data-stu-id="0ec98-180">PreSharedKey</span></span>                         |
|                  |                                      |

- <span data-ttu-id="0ec98-181">(*) IPSec-integritet måste vara ”null” om GCM-AES används som IPSec-krypteringsalgoritmen på vissa enheter.</span><span class="sxs-lookup"><span data-stu-id="0ec98-181">(*) On some device, IPsec integrity must be "null" if GCM-AES is used as IPsec encryption algorithm.</span></span>

### <a name="device-notes"></a><span data-ttu-id="0ec98-182">Anteckningar för enhet</span><span class="sxs-lookup"><span data-stu-id="0ec98-182">Device notes</span></span>

>[!NOTE]
>
> 1. <span data-ttu-id="0ec98-183">Stödet för IKEv2 kräver ASA version 8.4 och högre.</span><span class="sxs-lookup"><span data-stu-id="0ec98-183">IKEv2 support requires ASA version 8.4 and above.</span></span>
> 2. <span data-ttu-id="0ec98-184">Stöd för högre DH och PFS (utöver gruppen 5) kräver ASA version 9.x.</span><span class="sxs-lookup"><span data-stu-id="0ec98-184">Higher DH and PFS group support (beyond Group 5) requires ASA version 9.x.</span></span>
> 3. <span data-ttu-id="0ec98-185">IPSec-kryptering med AES-GCM och IPsec integritet med SHA-256, SHA-384, stöd för SHA-512 kräver ASA version 9.x på nyare ASA maskinvara; 5505 ASA, 5510, 5520, 5540, 5550, 5580 är **inte** stöds.</span><span class="sxs-lookup"><span data-stu-id="0ec98-185">IPsec encryption with AES-GCM and IPsec integrity with SHA-256, SHA-384, SHA-512 support requires ASA version 9.x on newer ASA hardware; ASA 5505, 5510, 5520, 5540, 5550, 5580 are **not** supported.</span></span> <span data-ttu-id="0ec98-186">(Kontrollera hello leverantör specifikationer tooconfirm.)</span><span class="sxs-lookup"><span data-stu-id="0ec98-186">(Please check hello vendor specifications tooconfirm.)</span></span>
>


### <a name="sample-device-configurations"></a><span data-ttu-id="0ec98-187">Exempel enhetskonfigurationer</span><span class="sxs-lookup"><span data-stu-id="0ec98-187">Sample device configurations</span></span>
<span data-ttu-id="0ec98-188">hello-skriptet nedan ger en exempelkonfiguration baserat på hello topologi och parametrarna som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="0ec98-188">hello script below provides a sample configuration based on hello topology and parameters listed above.</span></span> <span data-ttu-id="0ec98-189">Hej S2S VPN-tunnel konfigurationen består av hello följande delar:</span><span class="sxs-lookup"><span data-stu-id="0ec98-189">hello S2S VPN tunnel configuration consists of hello following parts:</span></span>

1. <span data-ttu-id="0ec98-190">Gränssnitt & vägar</span><span class="sxs-lookup"><span data-stu-id="0ec98-190">Interfaces & routes</span></span>
2. <span data-ttu-id="0ec98-191">Listor</span><span class="sxs-lookup"><span data-stu-id="0ec98-191">Access lists</span></span>
3. <span data-ttu-id="0ec98-192">IKE-principen och parametrar (fas 1 eller Main-läge)</span><span class="sxs-lookup"><span data-stu-id="0ec98-192">IKE policy and parameters (Phase 1 or Main Mode)</span></span>
4. <span data-ttu-id="0ec98-193">IPSec-principen och parametrar (fas 2 eller snabbläge)</span><span class="sxs-lookup"><span data-stu-id="0ec98-193">IPsec policy and parameters (Phase 2 or Quick Mode)</span></span>
5. <span data-ttu-id="0ec98-194">Andra parametrar (TCP-MSS minskning, etc.)</span><span class="sxs-lookup"><span data-stu-id="0ec98-194">Other parameters (TCP MSS clamping, etc.)</span></span>

>[!IMPORTANT] 
><span data-ttu-id="0ec98-195">Kontrollera att du slutför hello ytterligare konfiguration nedan och ersätta hello platshållarna med hello faktiska värden:</span><span class="sxs-lookup"><span data-stu-id="0ec98-195">Please make sure you complete hello additional configuration listed below and replace hello placeholders with hello actual values:</span></span>
> 
> - <span data-ttu-id="0ec98-196">Gränssnittskonfiguration för både inom och utanför gränssnitt</span><span class="sxs-lookup"><span data-stu-id="0ec98-196">Interface configuration for both inside and outside interfaces</span></span>
> - <span data-ttu-id="0ec98-197">Vägar för din innanför/privat och utanför och offentliga nätverk</span><span class="sxs-lookup"><span data-stu-id="0ec98-197">Routes for your inside/private and outside/public networks</span></span>
> - <span data-ttu-id="0ec98-198">Se till att alla namn och nummer är unika på hello-enhet</span><span class="sxs-lookup"><span data-stu-id="0ec98-198">Ensure all names and policy numbers are unique on hello device</span></span>
> - <span data-ttu-id="0ec98-199">Se till att hello kryptografiska algoritmer stöds på enheten</span><span class="sxs-lookup"><span data-stu-id="0ec98-199">Ensure hello cryptographic algorithms are supported on your device</span></span>
> - <span data-ttu-id="0ec98-200">Ersätt hello följande platshållare med hello faktiska värden</span><span class="sxs-lookup"><span data-stu-id="0ec98-200">Replace hello following place holders with hello actual values</span></span>
>   - <span data-ttu-id="0ec98-201">Utanför gränssnittsnamn: ”externa”</span><span class="sxs-lookup"><span data-stu-id="0ec98-201">Outside interface name: "outside"</span></span>
>   - <span data-ttu-id="0ec98-202">Azure_Gateway_Public_IP</span><span class="sxs-lookup"><span data-stu-id="0ec98-202">Azure_Gateway_Public_IP</span></span>
>   - <span data-ttu-id="0ec98-203">OnPrem_Device_Public_IP</span><span class="sxs-lookup"><span data-stu-id="0ec98-203">OnPrem_Device_Public_IP</span></span>
>   - <span data-ttu-id="0ec98-204">IKE-Pre_Shared_Key</span><span class="sxs-lookup"><span data-stu-id="0ec98-204">IKE Pre_Shared_Key</span></span>
>   - <span data-ttu-id="0ec98-205">Virtuella nätverk och lokala gateway nätverksnamn (VNetName, LNGName)</span><span class="sxs-lookup"><span data-stu-id="0ec98-205">VNet and local network gateway names (VNetName, LNGName)</span></span>
>   - <span data-ttu-id="0ec98-206">Virtuella nätverk och lokala nätverks-adressprefix</span><span class="sxs-lookup"><span data-stu-id="0ec98-206">VNet and on-premises network address prefixes</span></span>
>   - <span data-ttu-id="0ec98-207">Rätt nätmasker</span><span class="sxs-lookup"><span data-stu-id="0ec98-207">Proper netmasks</span></span>

#### <a name="sample-configuration"></a><span data-ttu-id="0ec98-208">Exempel på konfiguration</span><span class="sxs-lookup"><span data-stu-id="0ec98-208">Sample configuration</span></span>

```
! Sample ASA configuration for connecting tooAzure VPN gateway
!
! Tested hardware: ASA 5505
! Tested version:  ASA version 9.2(4)
!
! Replace hello following place holders with your actual values:
!   - Interface names - default are "outside" and "inside"
!   - <Azure_Gateway_Public_IP>
!   - <OnPrem_Device_Public_IP>
!   - <Pre_Shared_Key>
!   - <VNetName>*
!   - <LNGName>* ==> LocalNetworkGateway - hello Azure resource that represents the
!     on-premises network, specifies network prefixes, device public IP, BGP info, etc.
!   - <PrivateIPAddress> ==> Replace it with a private IP address if applicable
!   - <Netmask> ==> Replace it with appropriate netmasks
!   - <Nexthop> ==> Replace it with hello actual nexthop IP address
!
! (*) Must be unique names in hello device configuration
!
! ==> Interface & route configurations
!
!     > <OnPrem_Device_Public_IP> address on hello outside interface or vlan
!     > <PrivateIPAddress> on hello inside interface or vlan; e.g., 10.51.0.1/24
!     > Route tooconnect too<Azure_Gateway_Public_IP> address
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
!       (1) Allow S2S VPN tunnels between hello ASA and hello Azure gateway public IP address
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
!     > Object group that corresponding toohello <LNGName> prefixes.
!       E.g., 10.51.0.0/16 and 10.52.0.0/16. Note that LNG = "local network gateway".
!       In Azure network resource, a local network gateway defines hello on-premises
!       network properties (address prefixes, VPN device IP, BGP ASN, etc.)
!
object-group network <LNGName>
 description On-Premises network <LNGName> prefixes
 network-object 10.51.0.0 255.255.0.0
 network-object 10.52.0.0 255.255.0.0
exit
!
!     > Specify hello access-list between hello Azure VNet and your on-premises network.
!       This access list defines hello IPsec SA traffic selectors.
!
access-list Azure-<VNetName>-acl extended permit ip object-group <LNGName> object-group Azure-<VNetName>
!
!     > No NAT required between hello on-premises network and Azure VNet
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
!       - Make sure hello policy number is not used
!       - integrity and prf must be hello same
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
!       - This sample uses "Azure-<VNetName>-map" as hello crypto map name
!       - ASA supports only one crypto map per interface, if you already have
!         an existing crypto map assigned tooyour outside interface, you must use
!         hello same crypto map name, but with a different sequence number for
!         this policy
!       - "match address" policy uses hello access-list "Azure-<VNetName>-acl" defined 
!         previously
!       - "ipsec-proposal" uses hello proposal "AES-256" defined previously 
!       - PFS groups 14 and beyond requires ASA version 9.x.
!
crypto map Azure-<VNetName>-map 1 match address Azure-<VNetName>-acl
crypto map Azure-<VNetName>-map 1 set pfs group24
crypto map Azure-<VNetName>-map 1 set peer <Azure_Gateway_Public_IP>
crypto map Azure-<VNetName>-map 1 set ikev2 ipsec-proposal AES-256
crypto map Azure-<VNetName>-map 1 set security-association lifetime seconds 7200
crypto map Azure-<VNetName>-map interface outside
!
! ==> Set TCP MSS too1350
!
sysopt connection tcpmss 1350
!
```

## <a name="simple-debugging-commands"></a><span data-ttu-id="0ec98-209">Enkla kommandon för felsökning</span><span class="sxs-lookup"><span data-stu-id="0ec98-209">Simple debugging commands</span></span>

<span data-ttu-id="0ec98-210">Här följer några ASA-kommandon för felsökning:</span><span class="sxs-lookup"><span data-stu-id="0ec98-210">Here are some ASA commands for debugging purposes:</span></span>

1. <span data-ttu-id="0ec98-211">Visa hello IPsec och IKE SA</span><span class="sxs-lookup"><span data-stu-id="0ec98-211">Show hello IPsec and IKE SA's</span></span>
    - <span data-ttu-id="0ec98-212">”Visa kryptografi ipsec sa”</span><span class="sxs-lookup"><span data-stu-id="0ec98-212">"show crypto ipsec sa"</span></span>
    - <span data-ttu-id="0ec98-213">”Visa crypto ikev2 sa”</span><span class="sxs-lookup"><span data-stu-id="0ec98-213">"show crypto ikev2 sa"</span></span>
2. <span data-ttu-id="0ec98-214">Anger felsökningsläge - detta kan få väldigt mycket brus på hello-konsolen</span><span class="sxs-lookup"><span data-stu-id="0ec98-214">Entering debug mode - this can get very noisy on hello console</span></span>
    - <span data-ttu-id="0ec98-215">”debug kryptografi ikev2 plattform <level>”</span><span class="sxs-lookup"><span data-stu-id="0ec98-215">"debug crypto ikev2 platform <level>"</span></span>
    - <span data-ttu-id="0ec98-216">”Felsöka kryptografi ikev2-protokoll <level>”</span><span class="sxs-lookup"><span data-stu-id="0ec98-216">"debug crypto ikev2 protocol <level>"</span></span>
3. <span data-ttu-id="0ec98-217">Lista över aktuella konfigurationer</span><span class="sxs-lookup"><span data-stu-id="0ec98-217">List current configurations</span></span>
    - <span data-ttu-id="0ec98-218">”Visa kör” - visar hello aktuella konfigurationer på hello enhet. Du kan använda hello olika underordnade kommandon toolist vissa delar av hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0ec98-218">"show run" - shows hello current configurations on hello device; you can use hello various sub-commands toolist specific parts of hello configuration.</span></span> <span data-ttu-id="0ec98-219">Till exempel ”visa kör crypto”, ”visa kör åtkomstlista”, ”visa kör tunnel-grupp” osv.</span><span class="sxs-lookup"><span data-stu-id="0ec98-219">E.g., "show run crypto", "show run access-list", "show run tunnel-group", etc.</span></span>


## <a name="next-steps"></a><span data-ttu-id="0ec98-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ec98-220">Next steps</span></span>
<span data-ttu-id="0ec98-221">Se [konfigurera aktiv-aktiv VPN-gatewayer för anslutningar mellan platser och VNet-till-VNet-anslutningar](vpn-gateway-activeactive-rm-powershell.md) åtgärder tooconfigure aktiv-aktiv mellan platser och VNet-till-VNet-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="0ec98-221">See [Configuring Active-Active VPN Gateways for Cross-Premises and VNet-to-VNet Connections](vpn-gateway-activeactive-rm-powershell.md) for steps tooconfigure active-active cross-premises and VNet-to-VNet connections.</span></span>

