---
title: "Introduktion till App Service miljö v1"
description: "Mer information om funktionen Apptjänstmiljö v1 som tillhandahåller säker, ansluten till VNet, dedikerad skalningsenheter för att köra alla dina appar."
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
ms.openlocfilehash: 38cb79eb32bd61cdbfb6da91d50e6713d71a2b0d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="introduction-to-app-service-environment-v1"></a><span data-ttu-id="4fb60-103">Introduktion till App Service miljö v1</span><span class="sxs-lookup"><span data-stu-id="4fb60-103">Introduction to App Service Environment v1</span></span>

> [!NOTE]
> <span data-ttu-id="4fb60-104">Den här artikeln handlar om Apptjänstmiljö v1.</span><span class="sxs-lookup"><span data-stu-id="4fb60-104">This article is about the App Service Environment v1.</span></span>  <span data-ttu-id="4fb60-105">Det finns en nyare version av Apptjänst-miljön som är enklare att använda och körs på mer kraftfulla infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="4fb60-105">There is a newer version of the App Service Environment that is easier  to use and runs on more powerful infrastructure.</span></span> <span data-ttu-id="4fb60-106">Mer information om den nya versionen start med den [introduktion till Apptjänst-miljön](../app-service/app-service-environment/intro.md).</span><span class="sxs-lookup"><span data-stu-id="4fb60-106">To learn more about the new version start with the [Introduction to the App  Service Environment](../app-service/app-service-environment/intro.md).</span></span>
> 

## <a name="overview"></a><span data-ttu-id="4fb60-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="4fb60-107">Overview</span></span>
<span data-ttu-id="4fb60-108">En Apptjänst-miljö är en [Premium] [ PremiumTier] service plan alternativet för Azure App Service som tillhandahåller en helt isolerad och dedikerad miljö för Azure App Service-program som körs på ett säkert sätt i hög skala inklusive [Web Apps][WebApps], [Mobilappar][MobileApps], och [API Apps] [ APIApps].</span><span class="sxs-lookup"><span data-stu-id="4fb60-108">An App Service Environment is a [Premium][PremiumTier] service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps at high scale, including [Web Apps][WebApps], [Mobile Apps][MobileApps], and [API Apps][APIApps].</span></span>  

<span data-ttu-id="4fb60-109">Apptjänstmiljöer är perfekt för programarbetsbelastningar som kräver:</span><span class="sxs-lookup"><span data-stu-id="4fb60-109">App Service Environments are ideal for application workloads requiring:</span></span>

* <span data-ttu-id="4fb60-110">Mycket hög skala</span><span class="sxs-lookup"><span data-stu-id="4fb60-110">Very high scale</span></span>
* <span data-ttu-id="4fb60-111">Isolering och säker nätverksåtkomst</span><span class="sxs-lookup"><span data-stu-id="4fb60-111">Isolation and secure network access</span></span>

<span data-ttu-id="4fb60-112">Kunder kan skapa flera Apptjänstmiljöer inom en enda Azure region, samt över flera Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="4fb60-112">Customers can create multiple App Service Environments within a single Azure region, as well as across multiple Azure regions.</span></span>  <span data-ttu-id="4fb60-113">Detta gör Apptjänstmiljöer idealiskt för vågrätt skalning statuslösa programnivåerna stöd för hög RPS arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="4fb60-113">This makes App Service Environments ideal for horizontally scaling state-less application tiers in support of high RPS workloads.</span></span>

<span data-ttu-id="4fb60-114">Apptjänstmiljöer är isolerad för att endast en enskild kund program som körs och alltid har distribuerats till ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="4fb60-114">App Service Environments are isolated to running only a single customer's applications, and are always deployed into a virtual network.</span></span>  <span data-ttu-id="4fb60-115">Kunder har detaljerad kontroll över både inkommande och utgående nätverkstrafik och program kan upprätta säkra höghastighetsanslutning över virtuella nätverk till lokala företagsresurser.</span><span class="sxs-lookup"><span data-stu-id="4fb60-115">Customers have fine-grained control over both inbound and outbound application network traffic, and applications can establish high-speed secure connections over virtual networks to on-premises corporate resources.</span></span>

<span data-ttu-id="4fb60-116">Alla artiklar och hur-handlar om att Apptjänstmiljöer finns tillgängliga i den [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="4fb60-116">All articles and How-To's about App Service Environments are available in the [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="4fb60-117">En översikt över hur Apptjänstmiljöer Aktivera hög skala och säker nätverksåtkomst, finns det [AzureCon ingående] [ AzureConDeepDive] på Apptjänstmiljöer!</span><span class="sxs-lookup"><span data-stu-id="4fb60-117">For an overview of how App Service Environments enable high scale and secure network access, see the [AzureCon Deep Dive][AzureConDeepDive] on App Service Environments!</span></span>

<span data-ttu-id="4fb60-118">En djupdykning i vågrätt skalning med flera Apptjänstmiljöer finns i artikeln på hur du konfigurerar en [geodistribuerad app storleken][GeodistributedAppFootprint].</span><span class="sxs-lookup"><span data-stu-id="4fb60-118">For a deep-dive on horizontally scaling using multiple App Service Environments see the article on how to setup a [geo-distributed app footprint][GeodistributedAppFootprint].</span></span>

<span data-ttu-id="4fb60-119">Om du vill se hur säkerhetsarkitekturen som visas i AzureCon ingående har konfigurerats, finns i artikeln om hur du implementerar en [lager säkerhetsarkitekturen](app-service-app-service-environment-layered-security.md) med Apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="4fb60-119">To see how the security architecture shown in the AzureCon Deep Dive was configured, see the article on implementing a [layered security architecture](app-service-app-service-environment-layered-security.md) with App Service Environments.</span></span>

<span data-ttu-id="4fb60-120">Appar som körs på Apptjänstmiljöer kan behörighet gated av överordnad enheter, till exempel web application brandväggar (Brandvägg).</span><span class="sxs-lookup"><span data-stu-id="4fb60-120">Apps running on App Service Environments can have their access gated by upstream devices such as web application firewalls (WAF).</span></span>  <span data-ttu-id="4fb60-121">Artikel om [konfigurera en Brandvägg för Apptjänstmiljöer](app-service-app-service-environment-web-application-firewall.md) omfattar det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="4fb60-121">The article on [configuring a WAF for App Service Environments](app-service-app-service-environment-web-application-firewall.md) covers this scenario.</span></span> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a><span data-ttu-id="4fb60-122">Dedikerad beräkningsresurser</span><span class="sxs-lookup"><span data-stu-id="4fb60-122">Dedicated Compute Resources</span></span>
<span data-ttu-id="4fb60-123">Alla beräkningsresurser i en Apptjänst-miljö är avsedda enbart för en enda prenumeration och en Apptjänst-miljö kan konfigureras med upp till 50 (50)-beräkningsresurser för exklusiv användning av ett program.</span><span class="sxs-lookup"><span data-stu-id="4fb60-123">All of the compute resources in an App Service Environment are dedicated exclusively to a single subscription, and an App Service Environment can be configured with up to fifty (50) compute resources for exclusive use by a single application.</span></span>

<span data-ttu-id="4fb60-124">En Apptjänst-miljö består av en resurspool för frontend beräkning, samt en till tre worker beräkning resurspooler.</span><span class="sxs-lookup"><span data-stu-id="4fb60-124">An App Service Environment is composed of a front-end compute resource pool, as well as one to three worker compute resource pools.</span></span> 

<span data-ttu-id="4fb60-125">Frontend poolen innehåller beräkningsresurser som är ansvarig för SSL-avslutning som också automatisk belastningsbalansering av app-förfrågningar inom en Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="4fb60-125">The front-end pool contains compute resources responsible for SSL termination as well automatic load balancing of app requests within an App Service Environment.</span></span> 

<span data-ttu-id="4fb60-126">Varje arbetspool innehåller beräkningsresurser som allokeras till [Apptjänstplaner][AppServicePlan], som i sin tur innehåller en eller flera appar i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="4fb60-126">Each worker pool contains compute resources allocated to [App Service Plans][AppServicePlan], which in turn contain one or more Azure App Service apps.</span></span>  <span data-ttu-id="4fb60-127">Eftersom det kan vara upp till tre olika arbetarpooler i en Apptjänst-miljö, har du möjlighet att välja olika beräkningsresurser för varje worker-pool.</span><span class="sxs-lookup"><span data-stu-id="4fb60-127">Since there can be up to three different worker pools in an App Service Environment, you have the flexibility to choose different compute resources for each worker pool.</span></span>  

<span data-ttu-id="4fb60-128">T.ex, kan detta du skapa en arbetspool med mindre kraftfulla beräkningsresurser för Apptjänstplaner avsett för utveckling eller testning appar.</span><span class="sxs-lookup"><span data-stu-id="4fb60-128">For example, this allows you to create one worker pool with less powerful compute resources for App Service Plans intended for development or test apps.</span></span>  <span data-ttu-id="4fb60-129">En andra (eller även tredje) arbetspool kan använda mer kraftfulla beräkningsresurser som är avsedd för Apptjänstplaner produktion program som körs.</span><span class="sxs-lookup"><span data-stu-id="4fb60-129">A second (or even third) worker pool could use more powerful compute resources intended for App Service Plans running production apps.</span></span>

<span data-ttu-id="4fb60-130">Mer information om mängden beräkningsresurser som är tillgängliga på frontend- och arbetsroller lagringspooler finns [så här konfigurerar du en Apptjänst-miljö][HowToConfigureanAppServiceEnvironment].</span><span class="sxs-lookup"><span data-stu-id="4fb60-130">For more details on the quantity of compute resources available to the front-end and worker pools, see [How To Configure an App Service Environment][HowToConfigureanAppServiceEnvironment].</span></span>  

<span data-ttu-id="4fb60-131">Mer information om tillgängliga beräknings resurs storlekar som stöds i en Apptjänst-miljö finns i [priser för Apptjänst] [ AppServicePricing] sidan och granska de tillgängliga alternativen för Apptjänstmiljöer i den Premium-prisnivån.</span><span class="sxs-lookup"><span data-stu-id="4fb60-131">For details on the available compute resource sizes supported in an App Service Environment, consult the [App Service Pricing][AppServicePricing] page and review the available options for App Service Environments in the Premium pricing tier.</span></span>

## <a name="virtual-network-support"></a><span data-ttu-id="4fb60-132">Stöd för virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="4fb60-132">Virtual Network Support</span></span>
<span data-ttu-id="4fb60-133">En Apptjänst-miljö kan skapas i **antingen** ett virtuellt nätverk i Azure Resource Manager **eller** klassisk distribution modellen virtuella nätverk ([mer information om virtuella nätverk] [MoreInfoOnVirtualNetworks]).</span><span class="sxs-lookup"><span data-stu-id="4fb60-133">An App Service Environment can be created in **either** an Azure Resource Manager virtual network, **or** a classic deployment model virtual network ([more info on virtual networks][MoreInfoOnVirtualNetworks]).</span></span>  <span data-ttu-id="4fb60-134">Eftersom en Apptjänst-miljö finns alltid i ett virtuellt nätverk och mer exakt inom ett undernät i ett virtuellt nätverk, kan du utnyttja säkerhetsfunktionerna i virtuella nätverk för att styra både inkommande och utgående nätverkskommunikation.</span><span class="sxs-lookup"><span data-stu-id="4fb60-134">Since an App Service Environment always exists in a virtual network, and more precisely within a subnet of a virtual network, you can leverage the security features of virtual networks to control both inbound and outbound network communications.</span></span>  

<span data-ttu-id="4fb60-135">En Apptjänst-miljö kan vara antingen mot med en offentlig IP-adress eller intern med endast en Azure interna belastningen belastningsutjämnare (ILB)-adress mot Internet.</span><span class="sxs-lookup"><span data-stu-id="4fb60-135">An App Service Environment can be either Internet facing with a public IP address, or internal facing with only an Azure Internal Load Balancer (ILB) address.</span></span>

<span data-ttu-id="4fb60-136">Du kan använda [nätverkssäkerhetsgrupper] [ NetworkSecurityGroups] att begränsa inkommande nätverkskommunikationen till undernätet där det finns en Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="4fb60-136">You can use [network security groups][NetworkSecurityGroups] to restrict inbound network communications to the subnet where an App Service Environment resides.</span></span>  <span data-ttu-id="4fb60-137">På så sätt kan du köra bakom överordnade enheter och tjänster som web application brandväggar och nätverksprovider för SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="4fb60-137">This allows you to run apps behind upstream devices and services such as web application firewalls, and network SaaS providers.</span></span>

<span data-ttu-id="4fb60-138">Apparna måste också ofta att komma åt företagets resurser, till exempel interna databaser och webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="4fb60-138">Apps also frequently need to access corporate resources such as internal databases and web services.</span></span>  <span data-ttu-id="4fb60-139">En vanlig metod är att göra dessa slutpunkter som är tillgänglig endast för interna nätverkstrafik som går inom ett virtuellt Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="4fb60-139">A common approach is to make these endpoints available only to internal network traffic flowing within an Azure virtual network.</span></span>  <span data-ttu-id="4fb60-140">När en Apptjänst-miljö är ansluten till samma virtuella nätverk som interna tjänster, appar som körs i miljön kan komma åt dem, inklusive slutpunkter kan nås via [plats-till-plats] [ SiteToSite] och [Azure ExpressRoute] [ ExpressRoute] anslutningar.</span><span class="sxs-lookup"><span data-stu-id="4fb60-140">Once an App Service Environment is joined to the same virtual network as the internal services, apps running in the environment can access them, including endpoints reachable via [Site-to-Site][SiteToSite] and [Azure ExpressRoute][ExpressRoute] connections.</span></span>

<span data-ttu-id="4fb60-141">För mer information om hur Apptjänstmiljöer fungerar med virtuella nätverk och lokala nätverk finns i följande artiklar på [nätverksarkitektur][NetworkArchitectureOverview], [styra inkommande Trafik][ControllingInboundTraffic], och [säker anslutning till serverdelar][SecurelyConnectingToBackends].</span><span class="sxs-lookup"><span data-stu-id="4fb60-141">For more details on how App Service Environments work with virtual networks and on-premises networks consult the following articles on [Network Architecture][NetworkArchitectureOverview], [Controlling Inbound Traffic][ControllingInboundTraffic], and [Securely Connecting to Backends][SecurelyConnectingToBackends].</span></span> 

## <a name="getting-started"></a><span data-ttu-id="4fb60-142">Komma igång</span><span class="sxs-lookup"><span data-stu-id="4fb60-142">Getting started</span></span>
<span data-ttu-id="4fb60-143">Kom igång med Apptjänstmiljöer finns [hur att skapa en Apptjänst-miljö][HowToCreateAnAppServiceEnvironment]</span><span class="sxs-lookup"><span data-stu-id="4fb60-143">To get started with App Service Environments, see [How To Create An App Service Environment][HowToCreateAnAppServiceEnvironment]</span></span>

<span data-ttu-id="4fb60-144">Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i den [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="4fb60-144">All articles and How-To's for App Service Environments are available in the [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="4fb60-145">Mer information om plattformen Azure App Service finns [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="4fb60-145">For more information about the Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

<span data-ttu-id="4fb60-146">En översikt över nätverksarkitektur Apptjänst-miljö finns i [arkitektur, översikt] [ NetworkArchitectureOverview] artikel.</span><span class="sxs-lookup"><span data-stu-id="4fb60-146">For an overview of the App Service Environment network architecture, see the [Network Architecture Overview][NetworkArchitectureOverview] article.</span></span>

<span data-ttu-id="4fb60-147">Mer information om hur du använder en Apptjänst-miljö med ExpressRoute finns i följande artikel på [Express Route och Apptjänstmiljöer][NetworkConfigDetailsForExpressRoute].</span><span class="sxs-lookup"><span data-stu-id="4fb60-147">For details on using an App Service Environment with ExpressRoute, see the following article on [Express Route and App Service Environments][NetworkConfigDetailsForExpressRoute].</span></span>

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


