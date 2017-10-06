---
title: "aaaIntroduction tooApp v1-miljö"
description: "Läs mer om hello Apptjänstmiljö v1-funktion som tillhandahåller säker, ansluten till VNet, dedikerad skalningsenheter för att köra alla dina appar."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 78e6d4f5-da46-4eb5-a632-b5fdc17d2394
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: 6e3cd1909b241887b5ec19412b9f7884d870cc3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooapp-service-environment-v1"></a><span data-ttu-id="ac1aa-103">Introduktion tooApp v1-miljö</span><span class="sxs-lookup"><span data-stu-id="ac1aa-103">Introduction tooApp Service Environment v1</span></span>

> [!NOTE]
> <span data-ttu-id="ac1aa-104">Den här artikeln handlar om hello Apptjänstmiljö v1.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-104">This article is about hello App Service Environment v1.</span></span>  <span data-ttu-id="ac1aa-105">Det finns en nyare version av hello Apptjänst-miljö som är enklare toouse och körs på mer kraftfulla infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-105">There is a newer version of hello App Service Environment that is easier  toouse and runs on more powerful infrastructure.</span></span> <span data-ttu-id="ac1aa-106">Mer om hello nya versionen börjar med hello toolearn [introduktion toohello Apptjänstmiljö](../app-service/app-service-environment/intro.md).</span><span class="sxs-lookup"><span data-stu-id="ac1aa-106">toolearn more about hello new version start with hello [Introduction toohello App  Service Environment](../app-service/app-service-environment/intro.md).</span></span>
> 

## <a name="overview"></a><span data-ttu-id="ac1aa-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="ac1aa-107">Overview</span></span>
<span data-ttu-id="ac1aa-108">En Apptjänst-miljö är en [Premium] [ PremiumTier] service plan alternativet för Azure App Service som tillhandahåller en helt isolerad och dedikerad miljö för Azure App Service-program som körs på ett säkert sätt i hög skala inklusive [Web Apps][WebApps], [Mobilappar][MobileApps], och [API Apps] [ APIApps].</span><span class="sxs-lookup"><span data-stu-id="ac1aa-108">An App Service Environment is a [Premium][PremiumTier] service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps at high scale, including [Web Apps][WebApps], [Mobile Apps][MobileApps], and [API Apps][APIApps].</span></span>  

<span data-ttu-id="ac1aa-109">Apptjänstmiljöer är perfekt för programarbetsbelastningar som kräver:</span><span class="sxs-lookup"><span data-stu-id="ac1aa-109">App Service Environments are ideal for application workloads requiring:</span></span>

* <span data-ttu-id="ac1aa-110">Mycket hög skala</span><span class="sxs-lookup"><span data-stu-id="ac1aa-110">Very high scale</span></span>
* <span data-ttu-id="ac1aa-111">Isolering och säker nätverksåtkomst</span><span class="sxs-lookup"><span data-stu-id="ac1aa-111">Isolation and secure network access</span></span>

<span data-ttu-id="ac1aa-112">Kunder kan skapa flera Apptjänstmiljöer inom en enda Azure region, samt över flera Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-112">Customers can create multiple App Service Environments within a single Azure region, as well as across multiple Azure regions.</span></span>  <span data-ttu-id="ac1aa-113">Detta gör Apptjänstmiljöer idealiskt för vågrätt skalning statuslösa programnivåerna stöd för hög RPS arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-113">This makes App Service Environments ideal for horizontally scaling state-less application tiers in support of high RPS workloads.</span></span>

<span data-ttu-id="ac1aa-114">Apptjänstmiljöer finns isolerade toorunning endast en enskild kund program och alltid har distribuerats till ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-114">App Service Environments are isolated toorunning only a single customer's applications, and are always deployed into a virtual network.</span></span>  <span data-ttu-id="ac1aa-115">Kunder har detaljerad kontroll över både inkommande och utgående nätverkstrafik och program kan upprätta säkra höghastighetsanslutning över virtuella nätverk tooon lokala företagets resurser.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-115">Customers have fine-grained control over both inbound and outbound application network traffic, and applications can establish high-speed secure connections over virtual networks tooon-premises corporate resources.</span></span>

<span data-ttu-id="ac1aa-116">Alla artiklar och hur-handlar om att Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="ac1aa-116">All articles and How-To's about App Service Environments are available in hello [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="ac1aa-117">En översikt över hur Apptjänstmiljöer Aktivera hög skala och säker nätverksåtkomst, se hello [AzureCon ingående] [ AzureConDeepDive] på Apptjänstmiljöer!</span><span class="sxs-lookup"><span data-stu-id="ac1aa-117">For an overview of how App Service Environments enable high scale and secure network access, see hello [AzureCon Deep Dive][AzureConDeepDive] on App Service Environments!</span></span>

<span data-ttu-id="ac1aa-118">En djupdykning i vågrätt skalning med flera Apptjänstmiljöer finns hello artikel om hur toosetup en [geodistribuerad app storleken][GeodistributedAppFootprint].</span><span class="sxs-lookup"><span data-stu-id="ac1aa-118">For a deep-dive on horizontally scaling using multiple App Service Environments see hello article on how toosetup a [geo-distributed app footprint][GeodistributedAppFootprint].</span></span>

<span data-ttu-id="ac1aa-119">toosee hur hello-säkerhetsarkitekturen som visas i hello AzureCon ingående har konfigurerats, finns hello artikel om hur du implementerar en [lager säkerhetsarkitekturen](app-service-app-service-environment-layered-security.md) med Apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-119">toosee how hello security architecture shown in hello AzureCon Deep Dive was configured, see hello article on implementing a [layered security architecture](app-service-app-service-environment-layered-security.md) with App Service Environments.</span></span>

<span data-ttu-id="ac1aa-120">Appar som körs på Apptjänstmiljöer kan behörighet gated av överordnad enheter, till exempel web application brandväggar (Brandvägg).</span><span class="sxs-lookup"><span data-stu-id="ac1aa-120">Apps running on App Service Environments can have their access gated by upstream devices such as web application firewalls (WAF).</span></span>  <span data-ttu-id="ac1aa-121">hello artikel på [konfigurera en Brandvägg för Apptjänstmiljöer](app-service-app-service-environment-web-application-firewall.md) omfattar det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-121">hello article on [configuring a WAF for App Service Environments](app-service-app-service-environment-web-application-firewall.md) covers this scenario.</span></span> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a><span data-ttu-id="ac1aa-122">Dedikerad beräkningsresurser</span><span class="sxs-lookup"><span data-stu-id="ac1aa-122">Dedicated Compute Resources</span></span>
<span data-ttu-id="ac1aa-123">Alla hello beräkning resurser i en Apptjänst-miljö är dedikerad enbart tooa enda prenumeration och en Apptjänst-miljö kan konfigureras med upp toofifty (50) beräkningsresurser för exklusiv användning av ett enda program.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-123">All of hello compute resources in an App Service Environment are dedicated exclusively tooa single subscription, and an App Service Environment can be configured with up toofifty (50) compute resources for exclusive use by a single application.</span></span>

<span data-ttu-id="ac1aa-124">En Apptjänst-miljö består av en resurspool för frontend beräkning, samt en toothree worker beräkning resurspooler.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-124">An App Service Environment is composed of a front-end compute resource pool, as well as one toothree worker compute resource pools.</span></span> 

<span data-ttu-id="ac1aa-125">hello frontend poolen innehåller beräkningsresurser som är ansvarig för SSL-avslutning som också automatisk belastningsbalansering av app-förfrågningar inom en Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-125">hello front-end pool contains compute resources responsible for SSL termination as well automatic load balancing of app requests within an App Service Environment.</span></span> 

<span data-ttu-id="ac1aa-126">Varje arbetspool innehåller beräkningsresurser som allokeras för[Apptjänstplaner][AppServicePlan], som i sin tur innehåller en eller flera appar i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-126">Each worker pool contains compute resources allocated too[App Service Plans][AppServicePlan], which in turn contain one or more Azure App Service apps.</span></span>  <span data-ttu-id="ac1aa-127">Eftersom det kan vara upp toothree olika arbetarpooler i en Apptjänst-miljö, ha hello flexibilitet toochoose olika beräkningsresurser för varje worker-pool.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-127">Since there can be up toothree different worker pools in an App Service Environment, you have hello flexibility toochoose different compute resources for each worker pool.</span></span>  

<span data-ttu-id="ac1aa-128">T.ex, kan detta du toocreate en arbetspool med mindre kraftfulla beräkningsresurser för Apptjänstplaner avsett för utveckling eller testning appar.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-128">For example, this allows you toocreate one worker pool with less powerful compute resources for App Service Plans intended for development or test apps.</span></span>  <span data-ttu-id="ac1aa-129">En andra (eller även tredje) arbetspool kan använda mer kraftfulla beräkningsresurser som är avsedd för Apptjänstplaner produktion program som körs.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-129">A second (or even third) worker pool could use more powerful compute resources intended for App Service Plans running production apps.</span></span>

<span data-ttu-id="ac1aa-130">Mer information om hello mängden beräkning resurser tillgängliga toohello frontend och arbetarpooler finns [hur tooConfigure en Apptjänstmiljö][HowToConfigureanAppServiceEnvironment].</span><span class="sxs-lookup"><span data-stu-id="ac1aa-130">For more details on hello quantity of compute resources available toohello front-end and worker pools, see [How tooConfigure an App Service Environment][HowToConfigureanAppServiceEnvironment].</span></span>  

<span data-ttu-id="ac1aa-131">För information om tillgängliga hello compute-resurs-storlekar som stöds i en Apptjänst-miljö, se hello [priser för Apptjänst] [ AppServicePricing] sidan och granska hello tillgängliga alternativ för Apptjänstmiljöer i hello Premium-prisnivån.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-131">For details on hello available compute resource sizes supported in an App Service Environment, consult hello [App Service Pricing][AppServicePricing] page and review hello available options for App Service Environments in hello Premium pricing tier.</span></span>

## <a name="virtual-network-support"></a><span data-ttu-id="ac1aa-132">Stöd för virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="ac1aa-132">Virtual Network Support</span></span>
<span data-ttu-id="ac1aa-133">En Apptjänst-miljö kan skapas i **antingen** ett virtuellt nätverk i Azure Resource Manager **eller** klassisk distribution modellen virtuella nätverk ([mer information om virtuella nätverk] [MoreInfoOnVirtualNetworks]).</span><span class="sxs-lookup"><span data-stu-id="ac1aa-133">An App Service Environment can be created in **either** an Azure Resource Manager virtual network, **or** a classic deployment model virtual network ([more info on virtual networks][MoreInfoOnVirtualNetworks]).</span></span>  <span data-ttu-id="ac1aa-134">Eftersom en Apptjänst-miljö finns alltid i ett virtuellt nätverk och mer exakt inom ett undernät i ett virtuellt nätverk, kan du utnyttja hello säkerhetsfunktionerna i virtuella nätverk toocontrol både inkommande och utgående nätverkskommunikation.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-134">Since an App Service Environment always exists in a virtual network, and more precisely within a subnet of a virtual network, you can leverage hello security features of virtual networks toocontrol both inbound and outbound network communications.</span></span>  

<span data-ttu-id="ac1aa-135">En Apptjänst-miljö kan vara antingen mot med en offentlig IP-adress eller intern med endast en Azure interna belastningen belastningsutjämnare (ILB)-adress mot Internet.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-135">An App Service Environment can be either Internet facing with a public IP address, or internal facing with only an Azure Internal Load Balancer (ILB) address.</span></span>

<span data-ttu-id="ac1aa-136">Du kan använda [nätverkssäkerhetsgrupper] [ NetworkSecurityGroups] toorestrict inkommande kommunikation toohello undernät där det finns en Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-136">You can use [network security groups][NetworkSecurityGroups] toorestrict inbound network communications toohello subnet where an App Service Environment resides.</span></span>  <span data-ttu-id="ac1aa-137">Detta ger dig toorun appar bakom överordnade enheter och tjänster som web application brandväggar och SaaS-leverantörer för nätverk.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-137">This allows you toorun apps behind upstream devices and services such as web application firewalls, and network SaaS providers.</span></span>

<span data-ttu-id="ac1aa-138">Apparna måste också ofta tooaccess företagets resurser, till exempel interna databaser och webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-138">Apps also frequently need tooaccess corporate resources such as internal databases and web services.</span></span>  <span data-ttu-id="ac1aa-139">En vanlig metod är toomake dessa slutpunkter finns endast toointernal-nätverkstrafik som går inom ett virtuellt Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-139">A common approach is toomake these endpoints available only toointernal network traffic flowing within an Azure virtual network.</span></span>  <span data-ttu-id="ac1aa-140">När en Apptjänst-miljö är domänansluten toohello samma virtuella nätverk som hello interna tjänster, appar som körs i hello miljö kan komma åt dem, inklusive slutpunkter kan nås via [plats-till-plats] [ SiteToSite]och [Azure ExpressRoute] [ ExpressRoute] anslutningar.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-140">Once an App Service Environment is joined toohello same virtual network as hello internal services, apps running in hello environment can access them, including endpoints reachable via [Site-to-Site][SiteToSite] and [Azure ExpressRoute][ExpressRoute] connections.</span></span>

<span data-ttu-id="ac1aa-141">För mer information om hur Apptjänstmiljöer fungerar med virtuella nätverk och lokala nätverk finns i följande artiklar hello [nätverksarkitektur][NetworkArchitectureOverview], [styra inkommande Trafik][ControllingInboundTraffic], och [på ett säkert sätt ansluta tooBackends][SecurelyConnectingToBackends].</span><span class="sxs-lookup"><span data-stu-id="ac1aa-141">For more details on how App Service Environments work with virtual networks and on-premises networks consult hello following articles on [Network Architecture][NetworkArchitectureOverview], [Controlling Inbound Traffic][ControllingInboundTraffic], and [Securely Connecting tooBackends][SecurelyConnectingToBackends].</span></span> 

## <a name="getting-started"></a><span data-ttu-id="ac1aa-142">Komma igång</span><span class="sxs-lookup"><span data-stu-id="ac1aa-142">Getting started</span></span>
<span data-ttu-id="ac1aa-143">tooget igång med Apptjänstmiljöer, se [hur tooCreate en Apptjänst-miljö][HowToCreateAnAppServiceEnvironment]</span><span class="sxs-lookup"><span data-stu-id="ac1aa-143">tooget started with App Service Environments, see [How tooCreate An App Service Environment][HowToCreateAnAppServiceEnvironment]</span></span>

<span data-ttu-id="ac1aa-144">Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="ac1aa-144">All articles and How-To's for App Service Environments are available in hello [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="ac1aa-145">Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="ac1aa-145">For more information about hello Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

<span data-ttu-id="ac1aa-146">En översikt över hello Apptjänstmiljö nätverksarkitektur finns hello [arkitektur, översikt] [ NetworkArchitectureOverview] artikel.</span><span class="sxs-lookup"><span data-stu-id="ac1aa-146">For an overview of hello App Service Environment network architecture, see hello [Network Architecture Overview][NetworkArchitectureOverview] article.</span></span>

<span data-ttu-id="ac1aa-147">Mer information om hur du använder en Apptjänst-miljö med ExpressRoute finns hello följande artikel [Express Route och Apptjänstmiljöer][NetworkConfigDetailsForExpressRoute].</span><span class="sxs-lookup"><span data-stu-id="ac1aa-147">For details on using an App Service Environment with ExpressRoute, see hello following article on [Express Route and App Service Environments][NetworkConfigDetailsForExpressRoute].</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->


