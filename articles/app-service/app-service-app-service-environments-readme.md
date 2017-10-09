---
title: "aaaApp miljö | Microsoft Docs"
description: "Vad är en Azure Apptjänst-miljö? En introduktion tooApp-miljö."
keywords: "Azure apptjänst-miljön, virtuella nätverk, säkra nätverk"
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 1db5c057-3c56-4537-b580-cdd21fe3f3a7
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: stefsch
ms.openlocfilehash: 1b59fed4e5a72d4c4805e1dca203747e07e77103
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-environment-documentation"></a>Dokumentation för Apptjänst-miljö
En Apptjänst-miljö är en [Premium] [ PremiumTier] service plan alternativet för Azure App Service som tillhandahåller en helt isolerad och dedikerad miljö för Azure App Service-program som körs på ett säkert sätt i hög skala inklusive [Web Apps][WebApps], [Mobilappar][MobileApps], och [API Apps] [ APIApps].  

Apptjänstmiljöer är perfekt för programarbetsbelastningar som kräver:

* Mycket hög skala
* Isolering och säker nätverksåtkomst

Kunder kan skapa flera Apptjänstmiljöer inom en enda Azure region, samt över flera Azure-regioner.  Detta gör Apptjänstmiljöer idealiskt för vågrätt skalning statuslösa programnivåerna stöd för hög RPS arbetsbelastningar.

Apptjänstmiljöer finns isolerade toorunning endast en enskild kund program och alltid har distribuerats till ett virtuellt nätverk.  Kunder har detaljerad kontroll över både inkommande och utgående trafik med [nätverkssäkerhetsgrupper][NetworkSecurityGroups].  Program kan också etablera snabb säkra anslutningar över virtuella nätverk tooon lokala företagets resurser.

Appar måste ofta tooaccess företagets resurser, till exempel interna databaser och webbtjänster.  Appar som körs på Apptjänstmiljöer kan komma åt resurser som nås via [plats-till-plats] [ SiteToSite] VPN och [Azure ExpressRoute] [ ExpressRoute] anslutningar.

* [Vad är en App Service-miljö (ASE)?](../app-service-web/app-service-app-service-environment-intro.md)
* [Skapa en App Service-miljö](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Skapa appar i en App Service-miljö](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Skapa och använda en intern belastningsutjämnare med Apptjänstmiljöer](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Konfigurera en App Service-miljö](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Skala appar i en App Service-miljö](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [Nätverkssäkerhet och arkitektur](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>Så här
[!INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]

## <a name="videos"></a>Videoklipp
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2016/BRK3205/player]

>[!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON325/player]

>[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3715/player]



<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
