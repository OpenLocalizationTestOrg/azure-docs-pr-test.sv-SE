---
title: "Azure Apptjänst-miljö viktigt"
description: "Visar en lista över dokumentationen som beskriver Azure Apptjänst-miljö"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 77452413-5193-4762-8b3d-5fa8e4edf1ca
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 5b1362854dbc3b0098718bd2ea3cffb06366000c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="app-service-environment-documentation"></a>Dokumentation för Apptjänst-miljö
 Azure Apptjänst-miljö är en funktion i Azure App Service som tillhandahåller en helt isolerad och dedikerad miljö för säker körning av Apptjänst-appar i hög skala. Den här funktionen kan vara värd för din [webbappar][webapps], [mobilappar][mobileapps], [API apps][APIApps], och [funktioner][Functions].

Apptjänstmiljöer (ASEs) är perfekt för programarbetsbelastningar som kräver:

* Mycket hög skala.
* Isolering och säker nätverksåtkomst.

Kunder kan skapa flera ASEs inom ett enda Azure-region och över flera Azure-regioner. Den här flexibilitet gör ASEs idealiskt för vågrätt skalning tillståndslös programnivåerna stöd för hög RPS arbetsbelastningar.

ASEs är isolerad för att endast en enskild kund program som körs och alltid har distribuerats till Azure-nätverk. Kunder har detaljerad kontroll över inkommande och utgående nätverkstrafik genom att använda [Nätverkssäkerhetsgrupper][NSGs]. Program kan också etablera snabb säkra anslutningar över virtuella nätverk till lokala företagsresurser.

Appar behöver ofta åtkomst till företagets resurser, till exempel interna databaser och webbtjänster. Appar som körs på ASEs kan komma åt resurser via [plats-till-plats] [ SiteToSite] VPN och [Azure ExpressRoute] [ ExpressRoute] anslutningar.

* [Vad är en Apptjänst-miljö?][Intro]
* [Skapa en Apptjänst-miljö][MakeExternalASE]
* [Skapa en intern belastningsutjämnare Apptjänst-miljö][MakeILBASE]
* [Använd en Apptjänst-miljö][UsingASE]
* [Överväganden för nätverk och Apptjänst-miljö][ASENetwork]
* [Skapa en Apptjänst-miljö från en mall][MakeASEfromTemplate]


## <a name="videos"></a>Videoklipp
Hantera modern PaaS för företaget med Azure App Service
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2016/BRK3205/player]

Distribuera säkra appar med hög skalbarhet
>[!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON325/player]

Köra företagswebbappar och mobilappar på Azure App Service
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3715/player]

## <a name="app-service-environment-v1"></a>App Service Environment v1 ##
Det finns två versioner av Apptjänstmiljö: ASEv1 och ASEv2. Information om ASEv1 finns [Apptjänstmiljö v1 dokumentationen][ASEv1README].


<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[ASEv1README]: ../app-service-app-service-environments-readme.md
[SiteToSite]: ../../vpn-gateway/vpn-gateway-site-to-site-create.md
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
