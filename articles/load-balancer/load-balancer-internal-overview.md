---
title: "aaaInternal belastningsutjämnaren översikt | Microsoft Docs"
description: "Översikt för intern belastningsutjämnare och dess funktioner. Så här fungerar en belastningsutjämnare för Azure och möjliga scenarier tooconfigure interna slutpunkter"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 36065bfe-0ef1-46f9-a9e1-80b229105c85
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 9a901aad224d8821c154e130e142699d57282b25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="internal-load-balancer-overview"></a><span data-ttu-id="35242-103">Översikt över interna belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="35242-103">Internal load balancer overview</span></span>

<span data-ttu-id="35242-104">Till skillnad från hello Internet belastningsutjämnare dirigerar hello intern belastningsutjämnare (ILB) trafik endast tooresources i hello molnbaserad tjänst eller med hjälp av VPN-tooaccess hello Azure-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="35242-104">Unlike hello Internet facing load balancer, hello internal load balancer (ILB) directs traffic only tooresources inside hello cloud service or using VPN tooaccess hello Azure infrastructure.</span></span> <span data-ttu-id="35242-105">hello infrastruktur begränsar åtkomst toohello belastningsutjämnade virtuella IP-adresser (VIP) för en molnbaserad tjänst eller ett virtuellt nätverk så att de blir aldrig direkt exponerade tooan Internet slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="35242-105">hello infrastructure restricts access toohello load balanced virtual IP addresses (VIPs) of a Cloud Service or a Virtual Network so that they will never be directly exposed tooan Internet endpoint.</span></span> <span data-ttu-id="35242-106">Det interna branschspecifika toorun för business (LOB)-program i Azure och nås från hello molntjänster eller från resurser på lokalt.</span><span class="sxs-lookup"><span data-stu-id="35242-106">This enables internal line of business (LOB) applications toorun in Azure and be accessed from within hello cloud or from resources on-premises.</span></span>

## <a name="why-you-may-need-an-internal-load-balancer"></a><span data-ttu-id="35242-107">Varför du kanske behöver en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="35242-107">Why you may need an internal load balancer</span></span>

<span data-ttu-id="35242-108">Azure interna läsa in belastningsutjämning (ILB) tillhandahåller belastningsutjämning mellan virtuella datorer som finns inuti en molnbaserad tjänst eller ett virtuellt nätverk med ett regionalt omfång.</span><span class="sxs-lookup"><span data-stu-id="35242-108">Azure Internal Load Balancing (ILB) provides load balancing between virtual machines that reside inside of a cloud service or a virtual network with a regional scope.</span></span> <span data-ttu-id="35242-109">Information om hello användning och konfiguration av virtuellt nätverk med ett regionalt omfång finns [regionala virtuella nätverk](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) i hello Azure blogg.</span><span class="sxs-lookup"><span data-stu-id="35242-109">For information about hello use and configuration of virtual networks with a regional scope, see [Regional Virtual Networks](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) in hello Azure blog.</span></span> <span data-ttu-id="35242-110">Befintliga virtuella nätverk som har konfigurerats för en tillhörighetsgrupp kan inte använda ILB.</span><span class="sxs-lookup"><span data-stu-id="35242-110">Existing virtual networks that have been configured for an affinity group cannot use ILB.</span></span>

<span data-ttu-id="35242-111">ILB kan hello följande typer av nätverksbelastning:</span><span class="sxs-lookup"><span data-stu-id="35242-111">ILB enables hello following types of load balancing:</span></span>

* <span data-ttu-id="35242-112">I en molnbaserad tjänst från virtuella datorer tooa uppsättning virtuella datorer som finns på hello samma molntjänst (se bild 1).</span><span class="sxs-lookup"><span data-stu-id="35242-112">Within a cloud service, from virtual machines tooa set of virtual machines that reside within hello same cloud service (see Figure 1).</span></span>
* <span data-ttu-id="35242-113">Ett virtuellt nätverk från virtuella datorer i hello virtuellt nätverk tooa uppsättning virtuella datorer som finns på hello samma molntjänst för hello virtuella nätverk (se bild 2).</span><span class="sxs-lookup"><span data-stu-id="35242-113">Within a virtual network, from virtual machines in hello virtual network tooa set of virtual machines that reside within hello same cloud service of hello virtual network (see Figure 2).</span></span>
* <span data-ttu-id="35242-114">För ett virtuellt nätverk mellan platser, från lokala datorer tooa uppsättning virtuella datorer som finns på hello samma molntjänst för hello virtuella nätverk (se bild 3).</span><span class="sxs-lookup"><span data-stu-id="35242-114">For a cross-premises virtual network, from on-premises computers tooa set of virtual machines that reside within hello same cloud service of hello virtual network (see Figure 3).</span></span>
* <span data-ttu-id="35242-115">Internet-riktade och flera nivåer program där hello backend-nivåer är inte mot Internet men kräver belastningsutjämning för trafik från hello mot Internet-nivå.</span><span class="sxs-lookup"><span data-stu-id="35242-115">Internet-facing, multi-tier applications in which hello back-end tiers are not Internet-facing but require load balancing for traffic from hello Internet-facing tier.</span></span>
* <span data-ttu-id="35242-116">Belastningsutjämning för LOB-program som finns i Azure utan ytterligare belastningen belastningsutjämnaren maskinvara eller programvara.</span><span class="sxs-lookup"><span data-stu-id="35242-116">Load balancing for LOB applications hosted in Azure without requiring additional load balancer hardware or software.</span></span> <span data-ttu-id="35242-117">Inklusive lokala servrar i hello uppsättning datorer vars trafik är belastningen balanserade.</span><span class="sxs-lookup"><span data-stu-id="35242-117">Including on-premises servers in hello set of computers whose traffic is load balanced.</span></span>

## <a name="internet-facing-multi-tier-applications"></a><span data-ttu-id="35242-118">Flera nivåer program mot Internet</span><span class="sxs-lookup"><span data-stu-id="35242-118">Internet facing multi-tier applications</span></span>

<span data-ttu-id="35242-119">Hej webbnivå har internetuppkopplade slutpunkter för Internet-klienter och är en del av en belastningsutjämnad uppsättning.</span><span class="sxs-lookup"><span data-stu-id="35242-119">hello web tier has Internet facing endpoints for Internet clients and is part of a load-balanced set.</span></span> <span data-ttu-id="35242-120">hello belastningsutjämnare distribuerar inkommande trafik från webbklienter för TCP-port 443 (HTTPS) toohello webbservrar.</span><span class="sxs-lookup"><span data-stu-id="35242-120">hello load balancer  distributes incoming traffic from web clients for TCP port 443 (HTTPS) toohello web servers.</span></span>

<span data-ttu-id="35242-121">hello databasservrar finns bakom en ILB-slutpunkt som hello webbservrar använder för lagring.</span><span class="sxs-lookup"><span data-stu-id="35242-121">hello database servers are behind an ILB endpoint which hello web servers use for storage.</span></span> <span data-ttu-id="35242-122">Den här databasen service belastningsutjämnade slutpunkt, vilken trafik är belastningsutjämnas mellan hello databasservrar i hello ILB uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="35242-122">This database service load balanced endpoint, which traffic is load balanced across hello database servers in hello ILB set.</span></span>

<span data-ttu-id="35242-123">Följande bild visar hello hello Internetuppkopplad flernivåapp inom hello samma molntjänst.</span><span class="sxs-lookup"><span data-stu-id="35242-123">hello following image shows hello Internet facing multi-tier application within hello same cloud service.</span></span>

![Intern belastningsutjämning enda Molntjänsten](./media/load-balancer-internal-overview/IC736321.png)

<span data-ttu-id="35242-125">Bild 1 - flernivåapp mot Internet</span><span class="sxs-lookup"><span data-stu-id="35242-125">Figure 1 - Internet facing multi-tier application</span></span>

<span data-ttu-id="35242-126">Kan också använda en flernivåapp är när hello ILB distribuerade tooa annan molntjänst än hello en lång hello-tjänst för hello ILB.</span><span class="sxs-lookup"><span data-stu-id="35242-126">Another possible use for a multi-tier application is when hello ILB deployed tooa different cloud service than hello one consuming hello service for hello ILB.</span></span>

<span data-ttu-id="35242-127">Cloud services med hello samma virtuella nätverk har åtkomst till toohello ILB-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="35242-127">Cloud services using hello same virtual network will have access toohello ILB endpoint.</span></span> <span data-ttu-id="35242-128">hello följande bild visar frontend-webbservrar som i en annan molntjänst från hello-databas och använder hello ILB-slutpunkten inom hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="35242-128">hello following image shows front-end web servers are in a different cloud service from hello database back-end and using hello ILB endpoint within hello same virtual network.</span></span>

![Intern belastningsutjämning mellan molntjänster](./media/load-balancer-internal-overview/IC744147.png)

<span data-ttu-id="35242-130">Bild 2 - servrar i en annan molntjänst</span><span class="sxs-lookup"><span data-stu-id="35242-130">Figure 2 - Front-end servers in a different cloud service</span></span>

## <a name="intranet-line-of-business-applications"></a><span data-ttu-id="35242-131">Intranät verksamhetsspecifika program</span><span class="sxs-lookup"><span data-stu-id="35242-131">Intranet line of business applications</span></span>

<span data-ttu-id="35242-132">Trafik från klienter på hello lokalt nätverk hämta belastningsutjämnade över hello uppsättning LOB-servrar med hjälp av VPN-anslutning tooAzure nätverk.</span><span class="sxs-lookup"><span data-stu-id="35242-132">Traffic from clients on hello on-premises network get load-balanced across hello set of LOB servers using VPN connection tooAzure network.</span></span>

<span data-ttu-id="35242-133">hello klientdatorn ska ha åtkomst tooan IP-adress från Azure VPN-tjänsten med hjälp av VPN för punkt toosite.</span><span class="sxs-lookup"><span data-stu-id="35242-133">hello client machine will have access tooan IP address from Azure VPN service using point toosite VPN.</span></span> <span data-ttu-id="35242-134">Det gör hello Använd hello LOB-program som finns bakom hello ILB-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="35242-134">It allows hello use hello LOB application hosted behind hello ILB endpoint.</span></span>

![Intern belastningsutjämning med hjälp av VPN för punkt toosite](./media/load-balancer-internal-overview/IC744148.png)

<span data-ttu-id="35242-136">Bild 3 - LOB-program som finns bakom hello LB-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="35242-136">Figure 3 - LOB applications hosted behind hello LB endpoint</span></span>

<span data-ttu-id="35242-137">Ett annat scenario för hello LOB är toohave toosite VPN toohello virtuella nätverk på en plats där hello ILB-slutpunkten är konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="35242-137">Another scenario for hello LOB is toohave a site toosite VPN toohello virtual network where hello ILB endpoint is configured.</span></span> <span data-ttu-id="35242-138">Detta gör att lokalt nätverk trafik dirigeras toobe toohello ILB-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="35242-138">This allows on-premises network traffic toobe routed toohello ILB endpoint.</span></span>

![Intern belastningsutjämning med hjälp av webbplatsen toosite VPN](./media/load-balancer-internal-overview/IC744150.png)

<span data-ttu-id="35242-140">Bild 4 - lokal trafik dirigeras toohello ILB-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="35242-140">Figure 4 - On-premises network traffic routed toohello ILB endpoint</span></span>

## <a name="limitations"></a><span data-ttu-id="35242-141">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="35242-141">Limitations</span></span>

<span data-ttu-id="35242-142">Den interna belastningsutjämnaren konfigurationer stöder inte SNAT.</span><span class="sxs-lookup"><span data-stu-id="35242-142">Internal Load Balancer configurations do not support SNAT.</span></span> <span data-ttu-id="35242-143">Hello gäller det här dokumentet är refererar SNAT tooport imiterade källa nätverksadresser.</span><span class="sxs-lookup"><span data-stu-id="35242-143">In hello context of this document, SNAT refers tooport masquerading source  network address translation.</span></span>  <span data-ttu-id="35242-144">Detta gäller tooscenarios där en virtuell dator i en pool för belastningsutjämnare belastningen måste tooreach hello respektive interna Belastningsutjämnares klientdelens IP-adress.</span><span class="sxs-lookup"><span data-stu-id="35242-144">This applies tooscenarios where a VM in a load balancer pool needs tooreach hello respective internal Load Balancer's frontend IP address.</span></span> <span data-ttu-id="35242-145">Det här scenariot stöds inte för intern belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="35242-145">This scenario is not supported for internal Load Balancer.</span></span> <span data-ttu-id="35242-146">Anslutningsfel inträffar när hello flödet är belastningsutjämnad toohello VM ursprung hello flödet.</span><span class="sxs-lookup"><span data-stu-id="35242-146">Connection failures will occur when hello flow is load balanced toohello VM which originated hello flow.</span></span> <span data-ttu-id="35242-147">Du måste använda en belastningsutjämnare för proxy-format för sådana scenarier.</span><span class="sxs-lookup"><span data-stu-id="35242-147">You must use a proxy style load balancer for such scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="35242-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="35242-148">Next Steps</span></span>

[<span data-ttu-id="35242-149">Azure Resource Manager-stöd för Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="35242-149">Azure Resource Manager support for Azure Load Balancer</span></span>](load-balancer-arm.md)

[<span data-ttu-id="35242-150">Kom igång med att konfigurera en Internetuppkopplad belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="35242-150">Get started configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="35242-151">Kom igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="35242-151">Get started configuring an Internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="35242-152">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="35242-152">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="35242-153">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="35242-153">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
