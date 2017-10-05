---
title: "Web App kloning med hjälp av Azure Portal"
description: "Lär dig mer om att klona ditt webbprogram till nya webbprogram med hjälp av Azure Portal."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 20b0ae4e-67e8-4bae-9d74-8a24dc445cce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2016
ms.author: aelnably
ms.openlocfilehash: 9ebfa91c7972ab3c264032ead8376c23c1197a4b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Azure Apptjänst-App kloning med hjälp av Azure-portalen
Funktionen kloning i [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) kan du enkelt klona befintliga webbappar till en nyligen skapade appen i en annan region eller i samma region. Detta gör att kunder att distribuera flera appar över olika regioner, snabbt och enkelt.

Kloning av appen är för närvarande stöds endast för apptjänstplaner för premium-nivån. Ny funktion använder samma begränsningar som Web Apps Backup-funktionen, se [säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Kloning av en befintlig App
Webbprogrammet måste köras den **Premium** läge för att du kan skapa en klon för webbprogrammet.

1. I den [Azure Portal](https://portal.azure.com/), öppna ditt webbprogram bladet.
2. Klicka på **verktyg**. I den **verktyg** bladet, klickar du på **klona App**.
   
    ![][1]
   
   > [!NOTE]
   > Om webbappen inte är redan i den **Premium** läge, visas ett meddelande som anger stöds lägen för kloning av app. Nu har du möjlighet att välja **uppgradera**.
   > 
   > 
3. I den **klona App** bladet ange ett namn på ny webbapp, resursgruppen och App Service-Plan. Användaren kommer även att kunna välja om du vill klona ett antal inställningar för datakälla web app eller inte.
   
    ![][2]
4. När du klickar på **skapa** plattformen börjar skapa en klon av webbprogram källa.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Kloning av en befintlig App som en Apptjänst-miljö
I den **klona App** bladet kunden har möjlighet att välja en programpool i en befintlig Apptjänst-miljö.

## <a name="current-restrictions"></a>Aktuella begränsningar
Den här funktionen är för närvarande under förhandsgranskning, vi arbetar för att lägga till nya funktioner över tiden, i följande lista finns kända begränsningar för det aktuella stödet för kloning av app i Azure Portal:

* Klonas inte Azure Traffic Manager-inställningar
* Inställningar för automatisk skalning klonas inte
* Schemat för säkerhetskopiering inställningar klonas inte
* VNET-inställningarna klonas inte
* App Insights konfigureras inte automatiskt på mål-webbprogram
* Enkelt autentiseringsinställningarna klonas inte
* Kudu-tillägget klonas inte
* Tips regler klonas inte
* Databasen innehåll klonas inte

### <a name="references"></a>Referenser
* [Web App kloning med hjälp av PowerShell](app-service-web-app-cloning.md)
* [Säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md)
* [Skapa en App Service Environment](app-service-web-how-to-create-an-app-service-environment.md)
* [Skapa en webbapp i en App Service Environment](app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Introduktion till App Service-miljöer](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png
