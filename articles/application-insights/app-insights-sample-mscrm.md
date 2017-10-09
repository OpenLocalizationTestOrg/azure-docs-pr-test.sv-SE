---
title: aaaMicrosoft Dynamics CRM och Azure Application Insights | Microsoft Docs
description: "Hämta telemetri från Microsoft Dynamics CRM Online med hjälp av Application Insights. Genomgång av installationen, som hämtar data, visualisering och export."
services: application-insights
documentationcenter: 
author: mazharmicrosoft
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: bwren
ms.openlocfilehash: a39398060d6553fb18a26c101f085b7d87443636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Genomgång: Aktivera telemetri för Microsoft Dynamics CRM Online med hjälp av Application Insights
Den här artikeln beskrivs hur du tooget telemetridata från [Microsoft Dynamics CRM Online](https://www.dynamics.com/) med [Azure Application Insights](https://azure.microsoft.com/services/application-insights/). Vi går igenom hello Slutför process för att lägga till Application Insights skriptet tooyour program, data och datavisualisering.

> [!NOTE]
> [Bläddra hello exempellösning](https://dynamicsandappinsights.codeplex.com/).
> 
> 

## <a name="add-application-insights-toonew-or-existing-crm-online-instance"></a>Lägg till Application Insights toonew eller befintliga CRM Online-instans
toomonitor ditt program kan du lägga till en Application Insights SDK tooyour program. hello SDK skickar telemetri toohello [Application Insights-portalen](https://portal.azure.com), där du kan använda våra kraftfulla analys och diagnosverktyg eller exportera hello data toostorage.

### <a name="create-an-application-insights-resource-in-azure"></a>Skapa en Application Insights-resurs i Azure
1. Hämta [ett konto i Microsoft Azure](http://azure.com/pricing). 
2. Logga in på hello [Azure-portalen](https://portal.azure.com) och lägga till en ny Application Insights-resurs. Detta är där dina data bearbetas och visas.
   
    ![Klicka på +, Developer Services Application Insights.](./media/app-insights-sample-mscrm/01.png)
   
    Välj ASP.NET som hello programtyp.
3. Öppna hello komma igång-sidan och öppna ”övervaka och diagnostisera på klientsidan”.
   
    ![Kodstycke för infogning i din webbsida](./media/app-insights-sample-mscrm/03.png)

**Håll hello teckentabellen öppen** medan du hello nästa steg i ett nytt webbläsarfönster. Du behöver hello kod snart. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Skapa en webbresurs JavaScript i Microsoft Dynamics CRM
1. Öppna din CRM Online-instans och logga in med administratörsbehörighet.
2. Öppna Microsoft Dynamics CRM inställningar, anpassningar, anpassa hello System
   
    ![Microsoft Dynamics CRM-inställningar](./media/app-insights-sample-mscrm/04.png)
   
    ![Inställningar > anpassningar](./media/app-insights-sample-mscrm/05.png)

    ![Anpassa hello system alternativet](./media/app-insights-sample-mscrm/06.png)

1. Skapa en JavaScript-resurs.
   
    ![Dialogrutan Ny webbresurs](./media/app-insights-sample-mscrm/07.png)
   
    Ge det ett namn, Välj **skript (JScript)** och öppna hello textredigerare.
   
    ![Öppna hello textredigerare](./media/app-insights-sample-mscrm/08.png)
2. Kopiera hello koden från Application Insights. Se till att tooignore skripttaggar vid kopiering. Se nedan skärmbild:
   
    ![Ange din instrumentation nyckel](./media/app-insights-sample-mscrm/09.png)
   
    hello-koden innehåller hello instrumentation nyckel som identifierar din Application insights-resurs.
3. Spara och publicera.
   
    ![Spara och publicera](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Betalningsinstrument formulär
1. Öppna formuläret för hello-konto i Microsoft CRM Online
   
    ![Kontoformuläret](./media/app-insights-sample-mscrm/11.png)
2. Öppna hello formuläregenskaper
   
    ![Egenskaper för formulär](./media/app-insights-sample-mscrm/12.png)
3. Lägg till hello JavaScript webbresurs som du skapade
   
    ![Menyn Lägg till](./media/app-insights-sample-mscrm/13.png)
   
    ![Lägg till hello webbresurs](./media/app-insights-sample-mscrm/14.png)
4. Spara och publicera dina formuläranpassningar.

## <a name="metrics-captured"></a>Mått som har hämtats
Du har nu konfigurerat telemetriinsamling för hello formulär. När den används kan data skickas som tooyour Application Insights-resurs.

Här följer exempel på hello data som visas.

#### <a name="application-health"></a>Programmets hälsotillstånd
![Exempel sidinläsningstiden](./media/app-insights-sample-mscrm/15.png)

![Exempeldiagram sidan vyer](./media/app-insights-sample-mscrm/16.png)

Webbläsarundantag:

![Webbläsaren undantag diagram](./media/app-insights-sample-mscrm/17.png)

Klicka på hello diagram tooget detalj:

![Listan över undantag](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Användning
![Användare, sessioner och sidvyer](./media/app-insights-sample-mscrm/19.png)

![Sesion diagram](./media/app-insights-sample-mscrm/20.png)

![Webbläsarversioner](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Webbläsare
![Uppdelning av sidinläsningstiden](./media/app-insights-sample-mscrm/22.png)

![Antal sessioner efter webbläsarversion](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Geoplats
![Sessionsantal efter land](./media/app-insights-sample-mscrm/24.png)

![Sessioner och användare per land](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Inuti vyn begäran
![Sidan Visa sammanfattning](./media/app-insights-sample-mscrm/26.png)

![Söka på sidan Visa händelser](./media/app-insights-sample-mscrm/27.png)

![Liknande sidvisningar](./media/app-insights-sample-mscrm/28.png)

![Egenskaper för sidvisning](./media/app-insights-sample-mscrm/29.png)

![Sidor per session](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Exempelkod
[Bläddra hello exempelkod](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI
Du kan göra djupare analys om du [exportera hello data tooMicrosoft Power BI](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Exempel Microsoft Dynamics CRM-lösning
[Här är hello exempellösning implementerad i Microsoft Dynamics CRM](https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Läs mer
* [Vad är Application Insights?](app-insights-overview.md)
* [Application Insights för webbsidor](app-insights-javascript.md)
* [Fler exempel och genomgång](app-insights-code-samples.md)
