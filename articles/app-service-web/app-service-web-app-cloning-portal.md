---
title: "aaaWeb App kloning med hjälp av Azure Portal"
description: "Lär dig hur tooclone din Web Apps toonew webbprogram med hjälp av Azure Portal."
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
ms.openlocfilehash: 605c4879f34d568e9981c34109f9496731c9ed18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Azure Apptjänst-App kloning med hjälp av Azure-portalen
hello funktionen i kloning [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) kan du enkelt klona befintliga appar tooa nyskapad webbprogrammet i en annan region eller hello samma region. Detta gör att kunder toodeploy ett antal appar över olika regioner, snabbt och enkelt.

Kloning av appen är för närvarande stöds endast för apptjänstplaner för premium-nivån. hello nya funktion använder hello samma begränsningar som Web Apps Backup-funktionen finns [säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Kloning av en befintlig App
hello webbprogram måste köras i hello **Premium** läge för toocreate en klon för hello webbprogrammet.

1. I hello [Azure Portal](https://portal.azure.com/), öppna ditt webbprogram bladet.
2. Klicka på **verktyg**. Sedan hello **verktyg** bladet, klickar du på **klona App**.
   
    ![][1]
   
   > [!NOTE]
   > Om hello webbapp inte är redan i hello **Premium** läge, visas ett meddelande som anger hello stöds lägen för kloning av app. Nu har du hello alternativet tooselect **uppgradera**.
   > 
   > 
3. I hello **klona App** bladet anger du ett namn av hello nytt webbprogram, resursgruppen och App Service-Plan. Även hello användaren kommer att kunna toochoose om tooclone ett antal inställningar för datakälla web app eller inte.
   
    ![][2]
4. När du klickar på **skapa** hello plattform kommer att börja arbeta om hur du skapar en klon av hello käll-webbprogram.

## <a name="cloning-an-existing-app-tooan-app-service-environment"></a>Kloning av en befintlig App tooan Apptjänstmiljö
I hello **klona App** bladet hello kunden måste hello alternativet toochoose en programpool i en befintlig Apptjänst-miljö.

## <a name="current-restrictions"></a>Aktuella begränsningar
Den här funktionen är för närvarande under förhandsgranskning, vi arbetar tooadd nya funktioner över tid, hello följande lista är hello hello stöder kända begränsningar av app kloning i Azure-portalen:

* Klonas inte Azure Traffic Manager-inställningar
* Inställningar för automatisk skalning klonas inte
* Schemat för säkerhetskopiering inställningar klonas inte
* VNET-inställningarna klonas inte
* App Insights konfigureras inte automatiskt på hello mål för webbprogram
* Enkelt autentiseringsinställningarna klonas inte
* Kudu-tillägget klonas inte
* Tips regler klonas inte
* Databasen innehåll klonas inte

### <a name="references"></a>Referenser
* [Web App kloning med hjälp av PowerShell](app-service-web-app-cloning.md)
* [Säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md)
* [Hur tooCreate en Apptjänst-miljö](app-service-web-how-to-create-an-app-service-environment.md)
* [Skapa en webbapp i en App Service Environment](app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Introduktion tooApp-miljö](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png
