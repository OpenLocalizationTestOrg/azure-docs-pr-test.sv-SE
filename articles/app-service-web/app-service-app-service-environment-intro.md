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
# <a name="introduction-tooapp-service-environment-v1"></a>Introduktion tooApp v1-miljö

> [!NOTE]
> Den här artikeln handlar om hello Apptjänstmiljö v1.  Det finns en nyare version av hello Apptjänst-miljö som är enklare toouse och körs på mer kraftfulla infrastruktur. Mer om hello nya versionen börjar med hello toolearn [introduktion toohello Apptjänstmiljö](../app-service/app-service-environment/intro.md).
> 

## <a name="overview"></a>Översikt
En Apptjänst-miljö är en [Premium] [ PremiumTier] service plan alternativet för Azure App Service som tillhandahåller en helt isolerad och dedikerad miljö för Azure App Service-program som körs på ett säkert sätt i hög skala inklusive [Web Apps][WebApps], [Mobilappar][MobileApps], och [API Apps] [ APIApps].  

Apptjänstmiljöer är perfekt för programarbetsbelastningar som kräver:

* Mycket hög skala
* Isolering och säker nätverksåtkomst

Kunder kan skapa flera Apptjänstmiljöer inom en enda Azure region, samt över flera Azure-regioner.  Detta gör Apptjänstmiljöer idealiskt för vågrätt skalning statuslösa programnivåerna stöd för hög RPS arbetsbelastningar.

Apptjänstmiljöer finns isolerade toorunning endast en enskild kund program och alltid har distribuerats till ett virtuellt nätverk.  Kunder har detaljerad kontroll över både inkommande och utgående nätverkstrafik och program kan upprätta säkra höghastighetsanslutning över virtuella nätverk tooon lokala företagets resurser.

Alla artiklar och hur-handlar om att Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).

En översikt över hur Apptjänstmiljöer Aktivera hög skala och säker nätverksåtkomst, se hello [AzureCon ingående] [ AzureConDeepDive] på Apptjänstmiljöer!

En djupdykning i vågrätt skalning med flera Apptjänstmiljöer finns hello artikel om hur toosetup en [geodistribuerad app storleken][GeodistributedAppFootprint].

toosee hur hello-säkerhetsarkitekturen som visas i hello AzureCon ingående har konfigurerats, finns hello artikel om hur du implementerar en [lager säkerhetsarkitekturen](app-service-app-service-environment-layered-security.md) med Apptjänstmiljöer.

Appar som körs på Apptjänstmiljöer kan behörighet gated av överordnad enheter, till exempel web application brandväggar (Brandvägg).  hello artikel på [konfigurera en Brandvägg för Apptjänstmiljöer](app-service-app-service-environment-web-application-firewall.md) omfattar det här scenariot. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a>Dedikerad beräkningsresurser
Alla hello beräkning resurser i en Apptjänst-miljö är dedikerad enbart tooa enda prenumeration och en Apptjänst-miljö kan konfigureras med upp toofifty (50) beräkningsresurser för exklusiv användning av ett enda program.

En Apptjänst-miljö består av en resurspool för frontend beräkning, samt en toothree worker beräkning resurspooler. 

hello frontend poolen innehåller beräkningsresurser som är ansvarig för SSL-avslutning som också automatisk belastningsbalansering av app-förfrågningar inom en Apptjänst-miljö. 

Varje arbetspool innehåller beräkningsresurser som allokeras för[Apptjänstplaner][AppServicePlan], som i sin tur innehåller en eller flera appar i Azure App Service.  Eftersom det kan vara upp toothree olika arbetarpooler i en Apptjänst-miljö, ha hello flexibilitet toochoose olika beräkningsresurser för varje worker-pool.  

T.ex, kan detta du toocreate en arbetspool med mindre kraftfulla beräkningsresurser för Apptjänstplaner avsett för utveckling eller testning appar.  En andra (eller även tredje) arbetspool kan använda mer kraftfulla beräkningsresurser som är avsedd för Apptjänstplaner produktion program som körs.

Mer information om hello mängden beräkning resurser tillgängliga toohello frontend och arbetarpooler finns [hur tooConfigure en Apptjänstmiljö][HowToConfigureanAppServiceEnvironment].  

För information om tillgängliga hello compute-resurs-storlekar som stöds i en Apptjänst-miljö, se hello [priser för Apptjänst] [ AppServicePricing] sidan och granska hello tillgängliga alternativ för Apptjänstmiljöer i hello Premium-prisnivån.

## <a name="virtual-network-support"></a>Stöd för virtuella nätverk
En Apptjänst-miljö kan skapas i **antingen** ett virtuellt nätverk i Azure Resource Manager **eller** klassisk distribution modellen virtuella nätverk ([mer information om virtuella nätverk] [MoreInfoOnVirtualNetworks]).  Eftersom en Apptjänst-miljö finns alltid i ett virtuellt nätverk och mer exakt inom ett undernät i ett virtuellt nätverk, kan du utnyttja hello säkerhetsfunktionerna i virtuella nätverk toocontrol både inkommande och utgående nätverkskommunikation.  

En Apptjänst-miljö kan vara antingen mot med en offentlig IP-adress eller intern med endast en Azure interna belastningen belastningsutjämnare (ILB)-adress mot Internet.

Du kan använda [nätverkssäkerhetsgrupper] [ NetworkSecurityGroups] toorestrict inkommande kommunikation toohello undernät där det finns en Apptjänst-miljö.  Detta ger dig toorun appar bakom överordnade enheter och tjänster som web application brandväggar och SaaS-leverantörer för nätverk.

Apparna måste också ofta tooaccess företagets resurser, till exempel interna databaser och webbtjänster.  En vanlig metod är toomake dessa slutpunkter finns endast toointernal-nätverkstrafik som går inom ett virtuellt Azure-nätverk.  När en Apptjänst-miljö är domänansluten toohello samma virtuella nätverk som hello interna tjänster, appar som körs i hello miljö kan komma åt dem, inklusive slutpunkter kan nås via [plats-till-plats] [ SiteToSite]och [Azure ExpressRoute] [ ExpressRoute] anslutningar.

För mer information om hur Apptjänstmiljöer fungerar med virtuella nätverk och lokala nätverk finns i följande artiklar hello [nätverksarkitektur][NetworkArchitectureOverview], [styra inkommande Trafik][ControllingInboundTraffic], och [på ett säkert sätt ansluta tooBackends][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Komma igång
tooget igång med Apptjänstmiljöer, se [hur tooCreate en Apptjänst-miljö][HowToCreateAnAppServiceEnvironment]

Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).

Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service][AzureAppService].

En översikt över hello Apptjänstmiljö nätverksarkitektur finns hello [arkitektur, översikt] [ NetworkArchitectureOverview] artikel.

Mer information om hur du använder en Apptjänst-miljö med ExpressRoute finns hello följande artikel [Express Route och Apptjänstmiljöer][NetworkConfigDetailsForExpressRoute].

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


