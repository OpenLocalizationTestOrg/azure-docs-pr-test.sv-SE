---
title: "Konfigurera IPsec/IKE-princip för S2S VPN- eller VNet-till-VNet-anslutningar: Azure Resource Manager: PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur du konfigurerar IPsec/IKE-principen för S2S eller VNet-till-VNet-anslutningar med Azure VPN-gatewayer med Azure Resource Manager och PowerShell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: yushwang
ms.openlocfilehash: f8d2e29276efdec7071f2aa0d463b1abd64a5253
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections"></a><span data-ttu-id="23db5-103">Konfigurera IPsec/IKE-princip för S2S VPN- eller VNet-till-VNet-anslutningar</span><span class="sxs-lookup"><span data-stu-id="23db5-103">Configure IPsec/IKE policy for S2S VPN or VNet-to-VNet connections</span></span>

<span data-ttu-id="23db5-104">Den här artikeln vägleder dig genom hello steg tooconfigure IPsec/IKE-principen för plats-till-plats-VPN eller VNet-till-VNet-anslutningar med hello Resource Manager-distributionsmodellen och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="23db5-104">This article walks you through hello steps tooconfigure IPsec/IKE policy for Site-to-Site VPN or VNet-to-VNet connections using hello Resource Manager deployment model and PowerShell.</span></span>

## <span data-ttu-id="23db5-105"><a name="about"></a>Om IPsec och IKE principparametrar för Azure VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="23db5-105"><a name="about"></a>About IPsec and IKE policy parameters for Azure VPN gateways</span></span>
<span data-ttu-id="23db5-106">IPsec och IKE protokollet som standard stöder en mängd olika krypteringsalgoritmer i olika kombinationer.</span><span class="sxs-lookup"><span data-stu-id="23db5-106">IPsec and IKE protocol standard supports a wide range of cryptographic algorithms in various combinations.</span></span> <span data-ttu-id="23db5-107">Se för[om kryptografiska krav och Azure VPN-gatewayer](vpn-gateway-about-compliance-crypto.md) toosee hur detta kan hjälpa att säkerställa mellan platser och VNet-till-VNet-anslutningen uppfyller dina krav efterlevnad och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="23db5-107">Refer too[About cryptographic requirements and Azure VPN gateways](vpn-gateway-about-compliance-crypto.md) toosee how this can help ensuring cross-premises and VNet-to-VNet connectivity satisfy your compliance or security requirements.</span></span>

<span data-ttu-id="23db5-108">Den här artikeln innehåller anvisningar toocreate och konfigurera en princip för IPsec/IKE och tillämpa tooa ny eller befintlig anslutning:</span><span class="sxs-lookup"><span data-stu-id="23db5-108">This article provides instructions toocreate and configure an IPsec/IKE policy and apply tooa new or existing connection:</span></span>

* [<span data-ttu-id="23db5-109">Del 1 - arbetsflöde toocreate och ange IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="23db5-109">Part 1 - Workflow toocreate and set IPsec/IKE policy</span></span>](#workflow)
* [<span data-ttu-id="23db5-110">Del 2 - stöd för kryptografiska algoritmer och viktiga fördelar</span><span class="sxs-lookup"><span data-stu-id="23db5-110">Part 2 - Supported cryptographic algorithms and key strengths</span></span>](#params)
* [<span data-ttu-id="23db5-111">Del 3 – skapa en ny S2S VPN-anslutning med IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="23db5-111">Part 3 - Create a new S2S VPN connection with IPsec/IKE policy</span></span>](#crossprem)
* [<span data-ttu-id="23db5-112">Del 4 – skapa en ny VNet-till-VNet-anslutning med IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="23db5-112">Part 4 - Create a new VNet-to-VNet connection with IPsec/IKE policy</span></span>](#vnet2vnet)
* [<span data-ttu-id="23db5-113">En del 5 – hantera (skapa, lägga till, ta bort) IPsec/IKE-princip för en anslutning</span><span class="sxs-lookup"><span data-stu-id="23db5-113">Part 5 - Manage (create, add, remove) IPsec/IKE policy for a connection</span></span>](#managepolicy)

> [!IMPORTANT]
> 1. <span data-ttu-id="23db5-114">Observera att IPsec/princip bara fungerar på hello efter gateway-SKU: er:</span><span class="sxs-lookup"><span data-stu-id="23db5-114">Note that IPsec/IKE policy only works on hello following gateway SKUs:</span></span>
>    * <span data-ttu-id="23db5-115">***VpnGw1 VpnGw2, VpnGw3*** (route-baserade)</span><span class="sxs-lookup"><span data-stu-id="23db5-115">***VpnGw1, VpnGw2, VpnGw3*** (route-based)</span></span>
>    * <span data-ttu-id="23db5-116">***Standard*** och ***HighPerformance*** (route-baserade)</span><span class="sxs-lookup"><span data-stu-id="23db5-116">***Standard*** and ***HighPerformance*** (route-based)</span></span>
> 2. <span data-ttu-id="23db5-117">Du kan bara ange ***en*** principkombination för en viss anslutning.</span><span class="sxs-lookup"><span data-stu-id="23db5-117">You can only specify ***one*** policy combination for a given connection.</span></span>
> 3. <span data-ttu-id="23db5-118">Du måste ange alla algoritmer och parametrar för IKE (Main-läge) och IPsec (snabbläge).</span><span class="sxs-lookup"><span data-stu-id="23db5-118">You must specify all algorithms and parameters for both IKE (Main Mode) and IPsec (Quick Mode).</span></span> <span data-ttu-id="23db5-119">Partiell principspecifikationen tillåts inte.</span><span class="sxs-lookup"><span data-stu-id="23db5-119">Partial policy specification is not allowed.</span></span>
> 4. <span data-ttu-id="23db5-120">Kontakta din VPN-leverantör enhetsspecifikationerna tooensure hello principen stöds på din lokala VPN-enheter.</span><span class="sxs-lookup"><span data-stu-id="23db5-120">Consult with your VPN device vendor specifications tooensure hello policy is supported on your on-premises VPN devices.</span></span> <span data-ttu-id="23db5-121">S2S eller VNet-till-VNet-anslutningar kan inte upprätta om hello principer är inkompatibla.</span><span class="sxs-lookup"><span data-stu-id="23db5-121">S2S or VNet-to-VNet connections cannot establish if hello policies are incompatible.</span></span>

## <span data-ttu-id="23db5-122"><a name ="workflow"></a>Del 1 - arbetsflöde toocreate och ange IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="23db5-122"><a name ="workflow"></a>Part 1 - Workflow toocreate and set IPsec/IKE policy</span></span>
<span data-ttu-id="23db5-123">Det här avsnittet beskrivs hello arbetsflöde toocreate och uppdatera IPsec/IKE-principen på en S2S VPN- eller VNet-till-VNet-anslutning:</span><span class="sxs-lookup"><span data-stu-id="23db5-123">This section outlines hello workflow toocreate and update IPsec/IKE policy on a S2S VPN or VNet-to-VNet connection:</span></span>
1. <span data-ttu-id="23db5-124">Skapa ett virtuellt nätverk och en VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="23db5-124">Create a virtual network and a VPN gateway</span></span>
2. <span data-ttu-id="23db5-125">Skapa en lokal nätverksgateway för mellan lokala anslutningen eller ett annat virtuellt nätverk och gateway för VNet-till-VNet-anslutningen</span><span class="sxs-lookup"><span data-stu-id="23db5-125">Create a local network gateway for cross premises connection, or another virtual network and gateway for VNet-to-VNet connection</span></span>
3. <span data-ttu-id="23db5-126">Skapa en IPsec/IKE-princip med valda algoritmer och parametrar</span><span class="sxs-lookup"><span data-stu-id="23db5-126">Create an IPsec/IKE policy with selected algorithms and parameters</span></span>
4. <span data-ttu-id="23db5-127">Skapa en anslutning (IPSec- eller VNet2VNet) med hello IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="23db5-127">Create a connection (IPsec or VNet2VNet) with hello IPsec/IKE policy</span></span>
5. <span data-ttu-id="23db5-128">Lägg till/Uppdatera/ta bort en IPsec/IKE-princip för en befintlig anslutning</span><span class="sxs-lookup"><span data-stu-id="23db5-128">Add/update/remove an IPsec/IKE policy for an existing connection</span></span>

<span data-ttu-id="23db5-129">hello anvisningarna i den här artikeln hjälper dig att installera och konfigurera IPsec/IKE-principer som visas i hello diagram:</span><span class="sxs-lookup"><span data-stu-id="23db5-129">hello instructions in this article helps you set up and configure IPsec/IKE policies as shown in hello diagram:</span></span>

![IPSec-princip](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)

## <span data-ttu-id="23db5-131"><a name ="params"></a>Del 2 - stöd för kryptografiska algoritmer & viktiga fördelar</span><span class="sxs-lookup"><span data-stu-id="23db5-131"><a name ="params"></a>Part 2 - Supported cryptographic algorithms & key strengths</span></span>

<span data-ttu-id="23db5-132">hello följande tabell visas hello stöds kryptografiska algoritmer och viktiga styrkor konfigureras av hello-kunder:</span><span class="sxs-lookup"><span data-stu-id="23db5-132">hello following table lists hello supported cryptographic algorithms and key strengths configurable by hello customers:</span></span>

| <span data-ttu-id="23db5-133">**IPsec/IKEv2**</span><span class="sxs-lookup"><span data-stu-id="23db5-133">**IPsec/IKEv2**</span></span>  | <span data-ttu-id="23db5-134">**Alternativ**</span><span class="sxs-lookup"><span data-stu-id="23db5-134">**Options**</span></span>    |
| ---  | --- 
| <span data-ttu-id="23db5-135">IKEv2-kryptering</span><span class="sxs-lookup"><span data-stu-id="23db5-135">IKEv2 Encryption</span></span> | <span data-ttu-id="23db5-136">AES256, AES192, AES128, DES3, DES</span><span class="sxs-lookup"><span data-stu-id="23db5-136">AES256, AES192, AES128, DES3, DES</span></span>  
| <span data-ttu-id="23db5-137">IKEv2 Integrity</span><span class="sxs-lookup"><span data-stu-id="23db5-137">IKEv2 Integrity</span></span>  | <span data-ttu-id="23db5-138">SHA384, SHA256, SHA1, MD5</span><span class="sxs-lookup"><span data-stu-id="23db5-138">SHA384, SHA256, SHA1, MD5</span></span>  |
| <span data-ttu-id="23db5-139">DH-grupp</span><span class="sxs-lookup"><span data-stu-id="23db5-139">DH Group</span></span>         | <span data-ttu-id="23db5-140">DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, ingen</span><span class="sxs-lookup"><span data-stu-id="23db5-140">DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None</span></span> |
| <span data-ttu-id="23db5-141">IPsec-kryptering</span><span class="sxs-lookup"><span data-stu-id="23db5-141">IPsec Encryption</span></span> | <span data-ttu-id="23db5-142">GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None</span><span class="sxs-lookup"><span data-stu-id="23db5-142">GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None</span></span>    |
| <span data-ttu-id="23db5-143">IPsec Integrity</span><span class="sxs-lookup"><span data-stu-id="23db5-143">IPsec Integrity</span></span>  | <span data-ttu-id="23db5-144">GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5</span><span class="sxs-lookup"><span data-stu-id="23db5-144">GCMASE256, GCMAES192, GCMAES128, SHA256, SHA1, MD5</span></span> |
| <span data-ttu-id="23db5-145">PFS-grupp</span><span class="sxs-lookup"><span data-stu-id="23db5-145">PFS Group</span></span>        | <span data-ttu-id="23db5-146">PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, None</span><span class="sxs-lookup"><span data-stu-id="23db5-146">PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, None</span></span> 
| <span data-ttu-id="23db5-147">QM SA-livstid</span><span class="sxs-lookup"><span data-stu-id="23db5-147">QM SA Lifetime</span></span>   | <span data-ttu-id="23db5-148">(**Valfritt**: standardvärdena är används om inget anges)</span><span class="sxs-lookup"><span data-stu-id="23db5-148">(**Optional**: default values are used if not specified)</span></span><br><span data-ttu-id="23db5-149">Sekunder (heltal. **min. 300**/standard 27 000 sekunder)</span><span class="sxs-lookup"><span data-stu-id="23db5-149">Seconds (integer; **min. 300**/default 27000 seconds)</span></span><br><span data-ttu-id="23db5-150">Kilobyte (heltal. **min. 1024**/standard 102400000 kilobyte)</span><span class="sxs-lookup"><span data-stu-id="23db5-150">KBytes (integer; **min. 1024**/default 102400000 KBytes)</span></span>   |
| <span data-ttu-id="23db5-151">Trafikväljare</span><span class="sxs-lookup"><span data-stu-id="23db5-151">Traffic Selector</span></span> | <span data-ttu-id="23db5-152">UsePolicyBasedTrafficSelectors ** ($True/$False; **Valfritt**, $False som standard om inget anges)</span><span class="sxs-lookup"><span data-stu-id="23db5-152">UsePolicyBasedTrafficSelectors** ($True/$False; **Optional**, default $False if not specified)</span></span>    |
|  |  |

> [!IMPORTANT]
> 1. <span data-ttu-id="23db5-153">**Om GCMAES används för IPSec-krypteringsalgoritmen, måste du välja hello samma GCMAES algoritmen och nyckellängd IPsec integritet; till exempel med hjälp av GCMAES128 för både**</span><span class="sxs-lookup"><span data-stu-id="23db5-153">**If GCMAES is used as for IPsec Encryption algorithm, you must select hello same GCMAES algorithm and key length for IPsec Integrity; for example, using GCMAES128 for both**</span></span>
> 2. <span data-ttu-id="23db5-154">IKEv2 Main Säkerhetsassociation livslängd vara högst 28 800 sekunder på hello Azure VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="23db5-154">IKEv2 Main Mode SA lifetime is fixed at 28,800 seconds on hello Azure VPN gateways</span></span>
> 3. <span data-ttu-id="23db5-155">Inställningen ”UsePolicyBasedTrafficSelectors” för$ True i en anslutning konfigurerar hello Azure VPN gateway tooconnect toopolicy-baserad VPN-brandväggen lokalt.</span><span class="sxs-lookup"><span data-stu-id="23db5-155">Setting "UsePolicyBasedTrafficSelectors" too$True on a connection will configure hello Azure VPN gateway tooconnect toopolicy-based VPN firewall on premises.</span></span> <span data-ttu-id="23db5-156">Om du aktiverar PolicyBasedTrafficSelectors måste tooensure VPN-enheten har hello matchande trafik väljare som definierats med alla kombinationer av lokalt nätverk (lokal nätverksgateway)-prefix till eller från hello virtuella Azure-nätverksprefix i stället för alla-till-alla.</span><span class="sxs-lookup"><span data-stu-id="23db5-156">If you enable PolicyBasedTrafficSelectors, you need tooensure your VPN device has hello matching traffic selectors defined with all combinations of your on-premises network (local network gateway) prefixes to/from hello Azure virtual network prefixes, instead of any-to-any.</span></span> <span data-ttu-id="23db5-157">Om din prefix för det lokala nätverket är 10.1.0.0/16 och 10.2.0.0/16 och din prefix för det virtuella nätverket är 192.168.0.0/16 och 172.16.0.0/16, måste till exempel toospecify hello följande trafik väljare:</span><span class="sxs-lookup"><span data-stu-id="23db5-157">For example, if your on-premises network prefixes are 10.1.0.0/16 and 10.2.0.0/16, and your virtual network prefixes are 192.168.0.0/16 and 172.16.0.0/16, you need toospecify hello following traffic selectors:</span></span>
>    * <span data-ttu-id="23db5-158">10.1.0.0/16 <====> 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="23db5-158">10.1.0.0/16 <====> 192.168.0.0/16</span></span>
>    * <span data-ttu-id="23db5-159">10.1.0.0/16 <====> 172.16.0.0/16</span><span class="sxs-lookup"><span data-stu-id="23db5-159">10.1.0.0/16 <====> 172.16.0.0/16</span></span>
>    * <span data-ttu-id="23db5-160">10.2.0.0/16 <====> 192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="23db5-160">10.2.0.0/16 <====> 192.168.0.0/16</span></span>
>    * <span data-ttu-id="23db5-161">10.2.0.0/16 <====> 172.16.0.0/16</span><span class="sxs-lookup"><span data-stu-id="23db5-161">10.2.0.0/16 <====> 172.16.0.0/16</span></span>

<span data-ttu-id="23db5-162">Mer information om principbaserad trafik väljare finns [ansluta flera lokala principbaserad VPN-enheter](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="23db5-162">For more information regarding policy-based traffic selectors, see [Connect multiple on-premises policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span></span>

<span data-ttu-id="23db5-163">följande tabell visar hello hello motsvarande Diffie-Hellman-grupp som stöds av hello anpassad princip:</span><span class="sxs-lookup"><span data-stu-id="23db5-163">hello following table lists hello corresponding Diffie-Hellman Groups supported by hello custom policy:</span></span>

| <span data-ttu-id="23db5-164">**Diffie-Hellman-grupp**</span><span class="sxs-lookup"><span data-stu-id="23db5-164">**Diffie-Hellman Group**</span></span>  | <span data-ttu-id="23db5-165">**DHGroup**</span><span class="sxs-lookup"><span data-stu-id="23db5-165">**DHGroup**</span></span>              | <span data-ttu-id="23db5-166">**PFSGroup**</span><span class="sxs-lookup"><span data-stu-id="23db5-166">**PFSGroup**</span></span> | <span data-ttu-id="23db5-167">**Nyckellängd**</span><span class="sxs-lookup"><span data-stu-id="23db5-167">**Key length**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="23db5-168">1</span><span class="sxs-lookup"><span data-stu-id="23db5-168">1</span></span>                         | <span data-ttu-id="23db5-169">DHGroup1</span><span class="sxs-lookup"><span data-stu-id="23db5-169">DHGroup1</span></span>                 | <span data-ttu-id="23db5-170">PFS1</span><span class="sxs-lookup"><span data-stu-id="23db5-170">PFS1</span></span>         | <span data-ttu-id="23db5-171">768-bitars MODP</span><span class="sxs-lookup"><span data-stu-id="23db5-171">768-bit MODP</span></span>   |
| <span data-ttu-id="23db5-172">2</span><span class="sxs-lookup"><span data-stu-id="23db5-172">2</span></span>                         | <span data-ttu-id="23db5-173">DHGroup2</span><span class="sxs-lookup"><span data-stu-id="23db5-173">DHGroup2</span></span>                 | <span data-ttu-id="23db5-174">PFS2</span><span class="sxs-lookup"><span data-stu-id="23db5-174">PFS2</span></span>         | <span data-ttu-id="23db5-175">1024-bitars MODP</span><span class="sxs-lookup"><span data-stu-id="23db5-175">1024-bit MODP</span></span>  |
| <span data-ttu-id="23db5-176">14</span><span class="sxs-lookup"><span data-stu-id="23db5-176">14</span></span>                        | <span data-ttu-id="23db5-177">DHGroup14</span><span class="sxs-lookup"><span data-stu-id="23db5-177">DHGroup14</span></span><br><span data-ttu-id="23db5-178">DHGroup2048</span><span class="sxs-lookup"><span data-stu-id="23db5-178">DHGroup2048</span></span> | <span data-ttu-id="23db5-179">PFS2048</span><span class="sxs-lookup"><span data-stu-id="23db5-179">PFS2048</span></span>      | <span data-ttu-id="23db5-180">2048-bitars MODP</span><span class="sxs-lookup"><span data-stu-id="23db5-180">2048-bit MODP</span></span>  |
| <span data-ttu-id="23db5-181">19</span><span class="sxs-lookup"><span data-stu-id="23db5-181">19</span></span>                        | <span data-ttu-id="23db5-182">ECP256</span><span class="sxs-lookup"><span data-stu-id="23db5-182">ECP256</span></span>                   | <span data-ttu-id="23db5-183">ECP256</span><span class="sxs-lookup"><span data-stu-id="23db5-183">ECP256</span></span>       | <span data-ttu-id="23db5-184">256-bitars ECP</span><span class="sxs-lookup"><span data-stu-id="23db5-184">256-bit ECP</span></span>    |
| <span data-ttu-id="23db5-185">20</span><span class="sxs-lookup"><span data-stu-id="23db5-185">20</span></span>                        | <span data-ttu-id="23db5-186">ECP384</span><span class="sxs-lookup"><span data-stu-id="23db5-186">ECP384</span></span>                   | <span data-ttu-id="23db5-187">ECP284</span><span class="sxs-lookup"><span data-stu-id="23db5-187">ECP284</span></span>       | <span data-ttu-id="23db5-188">384-bitars ECP</span><span class="sxs-lookup"><span data-stu-id="23db5-188">384-bit ECP</span></span>    |
| <span data-ttu-id="23db5-189">24</span><span class="sxs-lookup"><span data-stu-id="23db5-189">24</span></span>                        | <span data-ttu-id="23db5-190">DHGroup24</span><span class="sxs-lookup"><span data-stu-id="23db5-190">DHGroup24</span></span>                | <span data-ttu-id="23db5-191">PFS24</span><span class="sxs-lookup"><span data-stu-id="23db5-191">PFS24</span></span>        | <span data-ttu-id="23db5-192">2048-bitars MODP</span><span class="sxs-lookup"><span data-stu-id="23db5-192">2048-bit MODP</span></span>  |

<span data-ttu-id="23db5-193">Se för[RFC3526](https://tools.ietf.org/html/rfc3526) och [RFC5114](https://tools.ietf.org/html/rfc5114) för mer information.</span><span class="sxs-lookup"><span data-stu-id="23db5-193">Refer too[RFC3526](https://tools.ietf.org/html/rfc3526) and [RFC5114](https://tools.ietf.org/html/rfc5114) for more details.</span></span>

## <span data-ttu-id="23db5-194"><a name ="crossprem"></a>Del 3 – skapa en ny S2S VPN-anslutning med IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="23db5-194"><a name ="crossprem"></a>Part 3 - Create a new S2S VPN connection with IPsec/IKE policy</span></span>

<span data-ttu-id="23db5-195">Det här avsnittet vägleder dig genom hello stegen för att skapa en S2S VPN-anslutning med ett IPsec/IKE-principen.</span><span class="sxs-lookup"><span data-stu-id="23db5-195">This section walks you through hello steps of creating a S2S VPN connection with an IPsec/IKE policy.</span></span> <span data-ttu-id="23db5-196">hello följande steg skapar hello anslutning som visas i hello diagram:</span><span class="sxs-lookup"><span data-stu-id="23db5-196">hello following steps create hello connection as shown in hello diagram:</span></span>

![s2s-princip](./media/vpn-gateway-ipsecikepolicy-rm-powershell/s2spolicy.png)

<span data-ttu-id="23db5-198">Se [skapa en S2S VPN-anslutning](vpn-gateway-create-site-to-site-rm-powershell.md) mer detaljerad stegvisa instruktioner för att skapa en S2S VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="23db5-198">See [Create a S2S VPN connection](vpn-gateway-create-site-to-site-rm-powershell.md) for more detailed step-by-step instructions for creating a S2S VPN connection.</span></span>

### <span data-ttu-id="23db5-199"><a name="before"></a>Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="23db5-199"><a name="before"></a>Before you begin</span></span>

* <span data-ttu-id="23db5-200">Kontrollera att du har en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="23db5-200">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="23db5-201">Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="23db5-201">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="23db5-202">Installera hello Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="23db5-202">Install hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="23db5-203">Se [översikt av Azure PowerShell](/powershell/azure/overview) för mer information om hur du installerar hello PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="23db5-203">See [Overview of Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span>

### <span data-ttu-id="23db5-204"><a name="createvnet1"></a>Steg 1 – Skapa hello virtuellt nätverk, VPN-gateway och lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="23db5-204"><a name="createvnet1"></a>Step 1 - Create hello virtual network, VPN gateway, and local network gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="23db5-205">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="23db5-205">1. Declare your variables</span></span>

<span data-ttu-id="23db5-206">Den här övningen starta genom att deklarera våra variabler.</span><span class="sxs-lookup"><span data-stu-id="23db5-206">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="23db5-207">Vara säker på att tooreplace hello värden med dina egna när du konfigurerar för produktion.</span><span class="sxs-lookup"><span data-stu-id="23db5-207">Be sure tooreplace hello values with your own when configuring for production.</span></span>

```powershell
$Sub1          = "<YourSubscriptionName>"
$RG1           = "TestPolicyRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GW1IPName1    = "VNet1GWIP1"
$GW1IPconf1    = "gw1ipconf1"
$Connection16  = "VNet1toSite6"

$LNGName6      = "Site6"
$LNGPrefix61   = "10.61.0.0/16"
$LNGPrefix62   = "10.62.0.0/16"
$LNGIP6        = "131.107.72.22"
```

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="23db5-208">2. Anslut tooyour prenumeration och skapa en ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="23db5-208">2. Connect tooyour subscription and create a new resource group</span></span>

<span data-ttu-id="23db5-209">Kontrollera att du växla tooPowerShell läge toouse hello Resource Manager-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="23db5-209">Make sure you switch tooPowerShell mode toouse hello Resource Manager cmdlets.</span></span> <span data-ttu-id="23db5-210">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="23db5-210">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="23db5-211">Öppna PowerShell-konsolen och Anslut tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="23db5-211">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="23db5-212">Använd hello följande exempel toohelp du ansluta:</span><span class="sxs-lookup"><span data-stu-id="23db5-212">Use hello following sample toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a><span data-ttu-id="23db5-213">3. Skapa hello virtuellt nätverk, VPN-gateway och lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="23db5-213">3. Create hello virtual network, VPN gateway, and local network gateway</span></span>

<span data-ttu-id="23db5-214">hello följande exempel skapar hello virtuellt nätverk, TestVNet1, med tre undernät och hello VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="23db5-214">hello following sample creates hello virtual network, TestVNet1, with three subnets, and hello VPN gateway.</span></span> <span data-ttu-id="23db5-215">När du ersätter värden är det viktigt att du alltid namnger gateway-undernätet specifikt till GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="23db5-215">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="23db5-216">Om du ger det något annat namn går det inte att skapa gatewayen.</span><span class="sxs-lookup"><span data-stu-id="23db5-216">If you name it something else, your gateway creation fails.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <span data-ttu-id="23db5-217"><a name="s2sconnection"></a>Steg 2 – Skapa en S2S VPN-anslutning med en IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="23db5-217"><a name="s2sconnection"></a>Step 2 - Create a S2S VPN connection with an IPsec/IKE policy</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="23db5-218">1. Skapa en IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="23db5-218">1. Create an IPsec/IKE policy</span></span>

<span data-ttu-id="23db5-219">Följande exempelskript hello skapar en IPsec/IKE-princip med hello följande algoritmer och parametrar:</span><span class="sxs-lookup"><span data-stu-id="23db5-219">hello following sample script creates an IPsec/IKE policy with hello following algorithms and parameters:</span></span>

* <span data-ttu-id="23db5-220">IKEv2: AES256, SHA384 DHGroup24</span><span class="sxs-lookup"><span data-stu-id="23db5-220">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="23db5-221">IPsec: AES256, SHA256, PFS24 SA livstid 7200 sekunder & 2048KB</span><span class="sxs-lookup"><span data-stu-id="23db5-221">IPsec: AES256, SHA256, PFS24, SA Lifetime 7200 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

<span data-ttu-id="23db5-222">Om du använder GCMAES för IPsec, måste du använda hello samma GCMAES algoritmen och nyckellängd för både IPSec-kryptering och integritet, till exempel:</span><span class="sxs-lookup"><span data-stu-id="23db5-222">If you use GCMAES for IPsec, you must use hello same GCMAES algorithm and key length for both IPsec encryption and integrity, for example:</span></span>

* <span data-ttu-id="23db5-223">IKEv2: AES256, SHA384 DHGroup24</span><span class="sxs-lookup"><span data-stu-id="23db5-223">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="23db5-224">IPsec: **GCMAES256, GCMAES256**, PFS24 SA livstid 7200 sekunder & 2048 KB</span><span class="sxs-lookup"><span data-stu-id="23db5-224">IPsec: **GCMAES256, GCMAES256**, PFS24, SA Lifetime 7200 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption GCMAES256 -IpsecIntegrity GCMAES256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

#### <a name="2-create-hello-s2s-vpn-connection-with-hello-ipsecike-policy"></a><span data-ttu-id="23db5-225">2. Skapa hello S2S VPN-anslutning med hello IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="23db5-225">2. Create hello S2S VPN connection with hello IPsec/IKE policy</span></span>

<span data-ttu-id="23db5-226">Skapa en S2S VPN-anslutning och tillämpa hello IPsec/princip skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="23db5-226">Create an S2S VPN connection and apply hello IPsec/IKE policy created earlier.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

<span data-ttu-id="23db5-227">Du kan lägga till ”-UsePolicyBasedTrafficSelectors $True” toohello Skapa anslutning cmdlet tooenable Azure VPN gateway tooconnect toopolicy-baserade VPN-enheter lokalt, enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="23db5-227">You can optionally add "-UsePolicyBasedTrafficSelectors $True" toohello create connection cmdlet tooenable Azure VPN gateway tooconnect toopolicy-based VPN devices on premises, as described above.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="23db5-228">När en IPsec/IKE-princip har angetts på en anslutning, skickar eller acceptera hello IPsec/IKE förslag med angivna kryptografiska algoritmer och viktiga fördelar för viss anslutningen endast hello Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="23db5-228">Once an IPsec/IKE policy is specified on a connection, hello Azure VPN gateway will only send or accept hello IPsec/IKE proposal with specified cryptographic algorithms and key strengths on that particular connection.</span></span> <span data-ttu-id="23db5-229">Kontrollera att din lokala VPN-enhet för hello anslutningen använder eller accepterar hello exakt princip kombination, annars hello S2S VPN-tunnel inte att upprätta.</span><span class="sxs-lookup"><span data-stu-id="23db5-229">Make sure your on-premises VPN device for hello connection uses or accepts hello exact policy combination, otherwise hello S2S VPN tunnel will not establish.</span></span>


## <span data-ttu-id="23db5-230"><a name ="vnet2vnet"></a>Del 4 – skapa en ny VNet-till-VNet-anslutning med IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="23db5-230"><a name ="vnet2vnet"></a>Part 4 - Create a new VNet-to-VNet connection with IPsec/IKE policy</span></span>

<span data-ttu-id="23db5-231">hello stegen för att skapa en VNet-till-VNet-anslutning med ett IPsec/IKE-principen är liknande toothat av en S2S VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="23db5-231">hello steps of creating a VNet-to-VNet connection with an IPsec/IKE policy are similar toothat of a S2S VPN connection.</span></span> <span data-ttu-id="23db5-232">hello skapa följande exempelskript hello anslutning som visas i hello diagram:</span><span class="sxs-lookup"><span data-stu-id="23db5-232">hello following sample scripts create hello connection as shown in hello diagram:</span></span>

![v2v-princip](./media/vpn-gateway-ipsecikepolicy-rm-powershell/v2vpolicy.png)

<span data-ttu-id="23db5-234">Se [skapa en VNet-till-VNet-anslutning](vpn-gateway-vnet-vnet-rm-ps.md) mer detaljerade anvisningar för att skapa en VNet-till-VNet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="23db5-234">See [Create a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) for more detailed steps for creating a VNet-to-VNet connection.</span></span> <span data-ttu-id="23db5-235">Du måste slutföra [del 3](#crossprem) toocreate TestVNet1 och konfigurera hello VPN-Gateway.</span><span class="sxs-lookup"><span data-stu-id="23db5-235">You must complete [Part 3](#crossprem) toocreate and configure TestVNet1 and hello VPN Gateway.</span></span>

### <span data-ttu-id="23db5-236"><a name="createvnet2"></a>Steg 1 – Skapa hello andra virtuella nätverk och VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="23db5-236"><a name="createvnet2"></a>Step 1 - Create hello second virtual network and VPN gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="23db5-237">1. Deklarera dina variabler</span><span class="sxs-lookup"><span data-stu-id="23db5-237">1. Declare your variables</span></span>

<span data-ttu-id="23db5-238">Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="23db5-238">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

```powershell
$RG2          = "TestPolicyRG2"
$Location2    = "East US 2"
$VNetName2    = "TestVNet2"
$FESubName2   = "FrontEnd"
$BESubName2   = "Backend"
$GWSubName2   = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$DNS2         = "8.8.8.8"
$GWName2      = "VNet2GW"
$GW2IPName1   = "VNet2GWIP1"
$GW2IPconf1   = "gw2ipconf1"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-hello-second-virtual-network-and-vpn-gateway-in-hello-new-resource-group"></a><span data-ttu-id="23db5-239">2. Skapa hello andra virtuella nätverk och VPN-gateway i hello ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="23db5-239">2. Create hello second virtual network and VPN gateway in hello new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

$gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1

New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance
```

### <a name="step-2---create-a-vnet-tovnet-connection-with-hello-ipsecike-policy"></a><span data-ttu-id="23db5-240">Steg 2 – Skapa en VNet toVNet anslutning med hello IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="23db5-240">Step 2 - Create a VNet-toVNet connection with hello IPsec/IKE policy</span></span>

<span data-ttu-id="23db5-241">Liknande toohello S2S VPN-anslutningen, skapa en IPsec/IKE-princip och sedan tillämpa toopolicy toohello ny anslutning.</span><span class="sxs-lookup"><span data-stu-id="23db5-241">Similar toohello S2S VPN connection, create an IPsec/IKE policy then apply toopolicy toohello new connection.</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="23db5-242">1. Skapa en IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="23db5-242">1. Create an IPsec/IKE policy</span></span>

<span data-ttu-id="23db5-243">Följande exempelskript hello skapar en annan IPsec/IKE-princip med hello följande algoritmer och parametrar:</span><span class="sxs-lookup"><span data-stu-id="23db5-243">hello following sample script creates a different IPsec/IKE policy with hello following algorithms and parameters:</span></span>
* <span data-ttu-id="23db5-244">IKEv2: AES128, SHA1, DHGroup14</span><span class="sxs-lookup"><span data-stu-id="23db5-244">IKEv2: AES128, SHA1, DHGroup14</span></span>
* <span data-ttu-id="23db5-245">IPsec: GCMAES128, GCMAES128, PFS14, SA livstid 7200 sekunder och 4096KB</span><span class="sxs-lookup"><span data-stu-id="23db5-245">IPsec: GCMAES128, GCMAES128, PFS14, SA Lifetime 7200 seconds & 4096KB</span></span>

```powershell
$ipsecpolicy2 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup PFS14 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 4096
```

#### <a name="2-create-vnet-to-vnet-connections-with-hello-ipsecike-policy"></a><span data-ttu-id="23db5-246">2. Skapa VNet-till-VNet-anslutningar med hello IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="23db5-246">2. Create VNet-to-VNet connections with hello IPsec/IKE policy</span></span>

<span data-ttu-id="23db5-247">Skapa en VNet-till-VNet-anslutning och tillämpa hello IPsec/IKE-principen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="23db5-247">Create a VNet-to-VNet connection and apply hello IPsec/IKE policy you created.</span></span> <span data-ttu-id="23db5-248">I det här exemplet är både gateways i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="23db5-248">In this example, both gateways are in hello same subscription.</span></span> <span data-ttu-id="23db5-249">Hej samma IPsec/IKE-princip i hello så att det är möjligt toocreate och konfigurera både anslutningar med samma PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="23db5-249">So it is possible toocreate and configure both connections with hello same IPsec/IKE policy in hello same PowerShell session.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2  -ResourceGroupName $RG2

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'
```

> [!IMPORTANT]
> <span data-ttu-id="23db5-250">När en IPsec/IKE-princip har angetts på en anslutning, skickar eller acceptera hello IPsec/IKE förslag med angivna kryptografiska algoritmer och viktiga fördelar för viss anslutningen endast hello Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="23db5-250">Once an IPsec/IKE policy is specified on a connection, hello Azure VPN gateway will only send or accept hello IPsec/IKE proposal with specified cryptographic algorithms and key strengths on that particular connection.</span></span> <span data-ttu-id="23db5-251">Kontrollera att hello IPsec-principer för både anslutningar är hello samma, annars VNet-till-VNet-anslutningen inte upprättas.</span><span class="sxs-lookup"><span data-stu-id="23db5-251">Make sure hello IPsec policies for both connections are hello same, otherwise the VNet-to-VNet connection will not establish.</span></span>

<span data-ttu-id="23db5-252">Hello-anslutningen upprättas om några minuter när du har slutfört de här stegen, och du måste hello efter nätverkets topologi som visas i början av hello:</span><span class="sxs-lookup"><span data-stu-id="23db5-252">After completing these steps, hello connection is established in a few minutes, and you will have hello following network topology as shown in hello beginning:</span></span>

![IPSec-princip](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)


## <span data-ttu-id="23db5-254"><a name ="managepolicy"></a>En del 5 - uppdatering IPsec/IKE-princip för en anslutning</span><span class="sxs-lookup"><span data-stu-id="23db5-254"><a name ="managepolicy"></a>Part 5 - Update IPsec/IKE policy for a connection</span></span>

<span data-ttu-id="23db5-255">hello sista avsnittet visas hur toomanage IPsec/IKE-princip för en befintlig S2S eller VNet-till-VNet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="23db5-255">hello last section shows you how toomanage IPsec/IKE policy for an existing S2S or VNet-to-VNet connection.</span></span> <span data-ttu-id="23db5-256">hello Övning nedan vägleder dig genom följande åtgärder på en anslutning hello:</span><span class="sxs-lookup"><span data-stu-id="23db5-256">hello exercise below walks you through hello following operations on a connection:</span></span>

1. <span data-ttu-id="23db5-257">Visa hello IPsec/IKE-princip för en anslutning</span><span class="sxs-lookup"><span data-stu-id="23db5-257">Show hello IPsec/IKE policy of a connection</span></span>
2. <span data-ttu-id="23db5-258">Lägg till eller uppdatera hello IPsec/IKE tooa anslutning</span><span class="sxs-lookup"><span data-stu-id="23db5-258">Add or update hello IPsec/IKE policy tooa connection</span></span>
3. <span data-ttu-id="23db5-259">Ta bort hello IPsec/IKE-princip från en anslutning</span><span class="sxs-lookup"><span data-stu-id="23db5-259">Remove hello IPsec/IKE policy from a connection</span></span>

<span data-ttu-id="23db5-260">hello samma steg gäller tooboth S2S och VNet-till-VNet-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="23db5-260">hello same steps apply tooboth S2S and VNet-to-VNet connections.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="23db5-261">IPsec/IKE-principen stöds på *Standard* och *HighPerformance* ruttbaserade VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="23db5-261">IPsec/IKE policy is supported on *Standard* and *HighPerformance* route-based VPN gateways only.</span></span> <span data-ttu-id="23db5-262">Det fungerar inte på hello grundläggande gateway SKU eller hello principbaserad VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="23db5-262">It does not work on hello Basic gateway SKU or hello policy-based VPN gateway.</span></span>

#### <a name="1-show-hello-ipsecike-policy-of-a-connection"></a><span data-ttu-id="23db5-263">1. Visa hello IPsec/IKE-princip för en anslutning</span><span class="sxs-lookup"><span data-stu-id="23db5-263">1. Show hello IPsec/IKE policy of a connection</span></span>

<span data-ttu-id="23db5-264">hello som följande exempel visar hur tooget hello IPsec/IKE-princip som konfigurerats på en anslutning.</span><span class="sxs-lookup"><span data-stu-id="23db5-264">hello following example shows how tooget hello IPsec/IKE policy configured on a connection.</span></span> <span data-ttu-id="23db5-265">hello skript kan också fortsätta från hello övningarna ovan.</span><span class="sxs-lookup"><span data-stu-id="23db5-265">hello scripts also continue from hello exercises above.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

<span data-ttu-id="23db5-266">hello senaste kommando visar hello aktuella IPsec/IKE-princip konfigurerats hello anslutning, om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="23db5-266">hello last command lists hello current IPsec/IKE policy configured on hello connection, if there is any.</span></span> <span data-ttu-id="23db5-267">hello följande exempel på utdata är för hello-anslutningen:</span><span class="sxs-lookup"><span data-stu-id="23db5-267">hello following sample output is for hello connection:</span></span>

```powershell
SALifeTimeSeconds   : 3600
SADataSizeKilobytes : 2048
IpsecEncryption     : AES256
IpsecIntegrity      : SHA256
IkeEncryption       : AES256
IkeIntegrity        : SHA384
DhGroup             : DHGroup24
PfsGroup            : PFS24
```

<span data-ttu-id="23db5-268">Om det finns ingen IPsec/IKE-princip som har konfigurerats, hello kommando (PS > $connection6.policy) hämtar det returnerade en tom.</span><span class="sxs-lookup"><span data-stu-id="23db5-268">If there is no IPsec/IKE policy configured, hello command (PS> $connection6.policy) gets an empty return.</span></span> <span data-ttu-id="23db5-269">Det innebär inte IPsec/IKE inte har konfigurerats på hello anslutning, men att det finns inga anpassade IPsec/IKE-princip.</span><span class="sxs-lookup"><span data-stu-id="23db5-269">It does not mean IPsec/IKE is not configured on hello connection, but that there is no custom IPsec/IKE policy.</span></span> <span data-ttu-id="23db5-270">hello faktiska anslutningen använder hello standardprincipen förhandlats mellan din lokala VPN-enhet och hello Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="23db5-270">hello actual connection uses hello default policy negotiated between your on-premises VPN device and hello Azure VPN gateway.</span></span>

#### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a><span data-ttu-id="23db5-271">2. Lägg till eller uppdatera en IPsec/IKE-princip för en anslutning</span><span class="sxs-lookup"><span data-stu-id="23db5-271">2. Add or update an IPsec/IKE policy for a connection</span></span>

<span data-ttu-id="23db5-272">hello steg tooadd en ny princip eller uppdatera en befintlig princip på en anslutning är hello samma: skapa en ny princip och sedan använda hello ny princip toohello anslutning.</span><span class="sxs-lookup"><span data-stu-id="23db5-272">hello steps tooadd a new policy or update an existing policy on a connection are hello same: create a new policy then apply hello new policy toohello connection.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6   = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup None -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

<span data-ttu-id="23db5-273">tooenable ”UsePolicyBasedTrafficSelectors” när du ansluter tooan lokalt principbaserad VPN-enhet, lägga till hello ”-UsePolicyBaseTrafficSelectors” parametern toohello cmdlet, eller ändra värdet för$ False toodisable hello alternativet:</span><span class="sxs-lookup"><span data-stu-id="23db5-273">tooenable "UsePolicyBasedTrafficSelectors" when connecting tooan on-premises policy-based VPN device, add hello "-UsePolicyBaseTrafficSelectors" parameter toohello cmdlet, or set it too$False toodisable hello option:</span></span>

```powershell
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6 -UsePolicyBasedTrafficSelectors $True
```

<span data-ttu-id="23db5-274">Du kan hämta hello anslutningen igen toocheck om hello principen uppdateras.</span><span class="sxs-lookup"><span data-stu-id="23db5-274">You can get hello connection again toocheck if hello policy is updated.</span></span>

```powershell
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

<span data-ttu-id="23db5-275">Du bör se hello utdata från hello sista raden, som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="23db5-275">You should see hello output from hello last line, as shown in hello following example:</span></span>

```powershell
SALifeTimeSeconds   : 3600
SADataSizeKilobytes : 2048
IpsecEncryption     : GCMAES128
IpsecIntegrity      : GCMAES128
IkeEncryption       : AES128
IkeIntegrity        : SHA1
DhGroup             : DHGroup14--
PfsGroup            : None
```

#### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a><span data-ttu-id="23db5-276">3. Ta bort en IPsec/IKE-princip från en anslutning</span><span class="sxs-lookup"><span data-stu-id="23db5-276">3. Remove an IPsec/IKE policy from a connection</span></span>

<span data-ttu-id="23db5-277">När du tar bort hello anpassad princip från en anslutning hello Azure VPN-gateway återgår tillbaka toohello [standardlistan med IPsec/IKE förslag](vpn-gateway-about-vpn-devices.md) och gör en omförhandling igen med din lokala VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="23db5-277">Once you remove hello custom policy from a connection, hello Azure VPN gateway reverts back toohello [default list of IPsec/IKE proposals](vpn-gateway-about-vpn-devices.md) and renegotiates again with your on-premises VPN device.</span></span>

```powershell
$RG1           = "TestPolicyRG1"
$Connection16  = "VNet1toSite6"
$connection6   = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

<span data-ttu-id="23db5-278">Du kan använda hello samma skript toocheck om hello princip har tagits bort från hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="23db5-278">You can use hello same script toocheck if hello policy has been removed from hello connection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="23db5-279">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="23db5-279">Next steps</span></span>

<span data-ttu-id="23db5-280">Se [ansluta flera lokala principbaserad VPN-enheter](vpn-gateway-connect-multiple-policybased-rm-ps.md) för mer information om principbaserad trafik väljare.</span><span class="sxs-lookup"><span data-stu-id="23db5-280">See [Connect multiple on-premises policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md) for more details regarding policy-based traffic selectors.</span></span>

<span data-ttu-id="23db5-281">När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="23db5-281">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="23db5-282">Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.</span><span class="sxs-lookup"><span data-stu-id="23db5-282">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>
