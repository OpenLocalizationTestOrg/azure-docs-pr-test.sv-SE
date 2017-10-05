---
title: Distribuera program till Azure App Service
description: "Lär dig hur du distribuera program till Apptjänst fungerar"
keywords: App service, azure app service distribuera distribution
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: 
ms.assetid: de12cd6e-e124-4e48-90bc-c3a3801305da
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 347e8b5177eac8e08ab0dea701b736b86d23904a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-deployment-overview"></a>Översikt över Azure App Service-distribution
Azure App Service tillhandahåller en omfattande och integrerade funktioner för att skapa kraftfulla distributionsarbetsflöden. App-distribution kan utnyttja alternativ som omfattar kontinuerlig integrering eller lokala källkontrollen publicering, WebDeploy- och FTP. Den rekommenderade metoden för Produktionsdistribution app är distributionen fack växlingen. Distributionsplatser representerar mellanlagrings- och integrering miljöer som är kopplade till produktion appar. Distributionsplatser kan konfigureras och mål för webbtrafik för verifiering och trafik kan växlas på begäran för distribution till tillverkning med ingen nertid och automatisk värmts. Stegen i ett arbetsflöde för distribution kan enkelt automatiskt via versionen management produkter som släpper hantering av Visual Studio. Detta är användbart för tillsammans med andra lösning resurser (t.ex. datalager), återkommande och replikering över flera enheter i distributionen. 

[!INCLUDE [app-service-blueprint-deployment](../../includes/app-service-blueprint-deployment.md)]

