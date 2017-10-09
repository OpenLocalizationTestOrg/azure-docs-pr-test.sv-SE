---
title: aaaAbout kryptografiska krav och Azure VPN-gatewayer | Microsoft Docs
description: "Den här artikeln beskrivs kraven för kryptografiska och Azure VPN-gatewayer"
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
ms.date: 05/22/2017
ms.author: yushwang
ms.openlocfilehash: af5f14d66beeea5316218f9788c4ad7876826162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a><span data-ttu-id="30f8c-103">Om kryptografiska krav och Azure VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="30f8c-103">About cryptographic requirements and Azure VPN gateways</span></span>

<span data-ttu-id="30f8c-104">Den här artikeln beskrivs hur du kan konfigurera Azure VPN-gatewayer toosatisfy dina kryptografiska krav för både anslutningar mellan lokala S2S VPN-tunnlar och VNet-till-VNet-anslutningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="30f8c-104">This article discusses how you can configure Azure VPN gateways toosatisfy your cryptographic requirements for both cross-premises S2S VPN tunnels and VNet-to-VNet connections within Azure.</span></span> 

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a><span data-ttu-id="30f8c-105">Om IPsec och IKE principparametrar för Azure VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="30f8c-105">About IPsec and IKE policy parameters for Azure VPN gateways</span></span>
<span data-ttu-id="30f8c-106">IPsec och IKE protokollet som standard stöder en mängd olika krypteringsalgoritmer i olika kombinationer.</span><span class="sxs-lookup"><span data-stu-id="30f8c-106">IPsec and IKE protocol standard supports a wide range of cryptographic algorithms in various combinations.</span></span> <span data-ttu-id="30f8c-107">Om kunder inte begär en specifik kombination av kryptografiska algoritmer och parametrar, Använd Azure VPN-gatewayer för förslag som standard.</span><span class="sxs-lookup"><span data-stu-id="30f8c-107">If customers do not request a specific combination of cryptographic algorithms and parameters, Azure VPN gateways use a set of default proposals.</span></span> <span data-ttu-id="30f8c-108">hello standard principen anger har valt toomaximize samverkan med en mängd olika tredje parts VPN-enheter i standardkonfigurationerna.</span><span class="sxs-lookup"><span data-stu-id="30f8c-108">hello default policy sets were chosen toomaximize interoperability with a wide range of third-party VPN devices in default configurations.</span></span> <span data-ttu-id="30f8c-109">Därför kan inte hello principer och hello antal förslag täcka alla möjliga kombinationer av tillgängliga kryptografiska algoritmer och viktiga fördelar.</span><span class="sxs-lookup"><span data-stu-id="30f8c-109">As a result, hello policies and hello number of proposals cannot cover all possible combinations of available cryptographic algorithms and key strengths.</span></span>

<span data-ttu-id="30f8c-110">Hej standardprincipen som angetts för Azure VPN-gateway finns i dokumentet hello: [om VPN-enheter och IPsec/IKE-parametrar för plats-till-plats VPN-Gateway-anslutningar](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="30f8c-110">hello default policy set for Azure VPN gateway is listed in hello document: [About VPN devices and IPsec/IKE parameters for Site-to-Site VPN Gateway connections](vpn-gateway-about-vpn-devices.md).</span></span>

## <a name="cryptographic-requirements"></a><span data-ttu-id="30f8c-111">Kryptografiska krav</span><span class="sxs-lookup"><span data-stu-id="30f8c-111">Cryptographic requirements</span></span>
<span data-ttu-id="30f8c-112">För kommunikation som kräver särskilda kryptografiska algoritmer eller parametrar, kan vanligtvis på grund av toocompliance eller säkerhet krav kunder nu konfigurera sina Azure VPN-gatewayer toouse en anpassad IPsec/IKE-princip med specifika kryptografiska algoritmer och viktiga styrkor i stället hello Azure standard principen anger.</span><span class="sxs-lookup"><span data-stu-id="30f8c-112">For communications that require specific cryptographic algorithms or parameters, typically due toocompliance or security requirements, customers can now configure their Azure VPN gateways toouse a custom IPsec/IKE policy with specific cryptographic algorithms and key strengths, rather than hello Azure default policy sets.</span></span>

<span data-ttu-id="30f8c-113">Till exempel använda hello IKEv2 huvudlägesprinciper för Azure VPN-gatewayer endast Diffie-Hellman-Grupp2 (1024 bitar) medan kunderna behöva toospecify starkare grupper toobe som används i IKE, till exempel grupp 14 (2048-bitars), grupp 24 (2048-bitars MODP grupp) eller ECP (elliptic kurva grupper) 256 eller 384 bitar (gruppen 19 och grupp 20 respektive).</span><span class="sxs-lookup"><span data-stu-id="30f8c-113">For example, hello IKEv2 main mode policies for Azure VPN gateways utilize only Diffie-Hellman Group 2 (1024 bits), whereas customers may need toospecify stronger groups toobe used in IKE, such as Group 14 (2048-bit), Group 24 (2048-bit MODP Group), or ECP (elliptic curve groups) 256 or 384 bit (Group 19 and Group 20, respectively).</span></span> <span data-ttu-id="30f8c-114">Liknande krav tillämpa tooIPsec snabbläge samt principer.</span><span class="sxs-lookup"><span data-stu-id="30f8c-114">Similar requirements apply tooIPsec quick mode policies as well.</span></span>

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a><span data-ttu-id="30f8c-115">Anpassad IPsec/IKE-princip med Azure VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="30f8c-115">Custom IPsec/IKE policy with Azure VPN gateways</span></span>
<span data-ttu-id="30f8c-116">Azure VPN-gatewayer har nu stöd för varje anslutning, anpassad IPsec/IKE-princip.</span><span class="sxs-lookup"><span data-stu-id="30f8c-116">Azure VPN gateways now support per-connection, custom IPsec/IKE policy.</span></span> <span data-ttu-id="30f8c-117">För en plats-till-plats eller VNet-till-VNet-anslutningen, kan du välja en specifik kombination av kryptografiska algoritmer för IPSec- och IKE med hello önskad nyckellängd, som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="30f8c-117">For a Site-to-Site or VNet-to-VNet connection, you can choose a specific combination of cryptographic algorithms for IPsec and IKE with hello desired key strength, as shown in hello following example:</span></span>

![IPSec-princip](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

<span data-ttu-id="30f8c-119">Du kan skapa en princip för IPsec/IKE och använda tooa ny eller befintlig anslutning.</span><span class="sxs-lookup"><span data-stu-id="30f8c-119">You can create an IPsec/IKE policy and apply tooa new or existing connection.</span></span> 

### <a name="workflow"></a><span data-ttu-id="30f8c-120">Arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="30f8c-120">Workflow</span></span>

1. <span data-ttu-id="30f8c-121">Skapa hello virtuella nätverk, VPN-gatewayer och lokala nätverksgatewayer för anslutningstopologin enligt beskrivningen i andra hur toodocuments</span><span class="sxs-lookup"><span data-stu-id="30f8c-121">Create hello virtual networks, VPN gateways, or local network gateways for your connectivity topology as described in other how-toodocuments</span></span>
2. <span data-ttu-id="30f8c-122">Skapa en IPsec/IKE-princip</span><span class="sxs-lookup"><span data-stu-id="30f8c-122">Create an IPsec/IKE policy</span></span>
3. <span data-ttu-id="30f8c-123">Du kan använda hello princip när du skapar en S2S eller VNet-till-VNet-anslutning</span><span class="sxs-lookup"><span data-stu-id="30f8c-123">You can apply hello policy when you create a S2S or VNet-to-VNet connection</span></span>
4. <span data-ttu-id="30f8c-124">Om hello anslutning redan har skapats, kan du tillämpa eller uppdatera hello princip tooan befintlig anslutning</span><span class="sxs-lookup"><span data-stu-id="30f8c-124">If hello connection is already created, you can apply or update hello policy tooan existing connection</span></span>


## <a name="ipsecike-policy-faq"></a><span data-ttu-id="30f8c-125">IPSec-/ princip vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="30f8c-125">IPsec/IKE policy FAQ</span></span>

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-ipsecikepolicy-faq-include.md)]


## <a name="next-steps"></a><span data-ttu-id="30f8c-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="30f8c-126">Next steps</span></span>
<span data-ttu-id="30f8c-127">Se [konfigurera IPSec-/ princip](vpn-gateway-ipsecikepolicy-rm-powershell.md) stegvisa instruktioner om hur du konfigurerar anpassade IPsec/IKE-principen på en anslutning.</span><span class="sxs-lookup"><span data-stu-id="30f8c-127">See [Configure IPsec/IKE policy](vpn-gateway-ipsecikepolicy-rm-powershell.md) for step-by-step instructions on configuring custom IPsec/IKE policy on a connection.</span></span>

<span data-ttu-id="30f8c-128">Se även [ansluta flera principbaserad VPN-enheter](vpn-gateway-connect-multiple-policybased-rm-ps.md) toolearn mer om hello UsePolicyBasedTrafficSelectors alternativet.</span><span class="sxs-lookup"><span data-stu-id="30f8c-128">See also [Connect multiple policy-based VPN devices](vpn-gateway-connect-multiple-policybased-rm-ps.md) toolearn more about hello UsePolicyBasedTrafficSelectors option.</span></span>
