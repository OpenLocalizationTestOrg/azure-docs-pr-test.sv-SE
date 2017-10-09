---
title: aaaSCOM integrering med Application Insights | Microsoft Docs
description: "Om du använder en SCOM, övervaka prestanda och diagnostisera problem med Application Insights. Omfattande instrumentpaneler, smart aviseringar, kraftfulla verktyg för Nätverksdiagnostik och analys frågor."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/12/2016
ms.author: bwren
ms.openlocfilehash: ee9ad462610fd916098a0e292c5bd44f2a873c10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a>Prestandaövervakning av program med Application Insights för SCOM
Om du använder System Center Operations Manager (SCOM) toomanage dina servrar, kan du övervaka prestanda och diagnostisera prestandaproblem med hello [Azure Application Insights](app-insights-asp-net.md). Application Insights övervakar ditt webbprogram inkommande begäranden, utgående REST och SQL-anrop, undantag och loggspårningar. Det ger instrumentpaneler med mått diagram och smarta aviseringar, samt kraftfulla diagnostiska Sök och analytiska frågor via den här telemetri. 

Du kan växla på Application Insights-övervakning med hjälp av ett hanteringspaket för SCOM.

## <a name="before-you-start"></a>Innan du börjar
Vi förutsätter:

* Du är bekant med SCOM och att du använder SCOM 2012 R2 eller 2016 toomanage din IIS-webbservrar.
* Du har redan installerat på dina servrar för ett webbprogram som du vill toomonitor med Application Insights.
* Appen framework-version är .NET 4.5 eller senare.
* Du har åtkomst tooa prenumeration [Microsoft Azure](https://azure.com) och kan logga in toohello [Azure-portalen](https://portal.azure.com). Din organisation kan ha en prenumeration och kan lägga till ditt Microsoft-konto tooit.

(hello Utvecklingsteamet kan skapa hello [Application Insights SDK](app-insights-asp-net.md) i hello webbapp. Den här byggning instrumentation ger dem större flexibilitet vid skrivning anpassad telemetri. Men det spelar ingen roll: du kan följa hello steg som beskrivs här med eller utan hello SDK inbyggda.)

## <a name="one-time-install-application-insights-management-pack"></a>(En gång) Installera Application Insights management pack
På hello datorn där du kör Operations Manager:

1. Avinstallera en tidigare version av hello management pack:
   1. Öppna Administration Management Packs i Operations Manager. 
   2. Ta bort gamla hello-versionen.
2. Hämta och installera hello hanteringspaket hello-katalogen.
3. Starta om Operations Manager.

## <a name="create-a-management-pack"></a>Skapa ett hanteringspaket
1. Öppna i Operations Manager, **redigering**, **.NET... med Application Insights**, **guiden Lägg till övervakning**, och välj igen **.NET med program... Insikter**.
   
    ![](./media/app-insights-scom/020.png)
2. Konfiguration av hello efter din app. (Du har tooinstrument en app åt gången.)
   
    ![](./media/app-insights-scom/030.png)
3. Hej på samma sida i guiden Skapa ett nytt management pack, eller välj ett paket som du tidigare skapade för Application Insights.
   
     (hello Application Insights [hanteringspaket](https://technet.microsoft.com/library/cc974491.aspx) är en mall som du skapar en instans. Du kan återanvända samma instans senare hello.)

    ![Skriv hello namnet på hello app i hello fliken allmänna egenskaper. Klicka på nytt och ange ett namn för ett management pack. Klicka på OK och klicka på Nästa.](./media/app-insights-scom/040.png)

1. Välj en app som du vill toomonitor. hello sökfunktionen söker bland appar som är installerade på servrarna.
   
    ![Vilka tooMonitor på fliken klickar du på Lägg till, skriver du en del av hello programnamn, klicka på Sök, väljer hello appen och sedan lägga till, OK.](./media/app-insights-scom/050.png)
   
    hello övervakning scope Fritextfält kan vara används toospecify en delmängd av dina servrar, om du inte vill toomonitor hello app i alla servrar.
2. Du måste ange dina autentiseringsuppgifter toosign i tooMicrosoft Azure på hello nästa sida i guiden.
   
    Välj hello Application Insights-resurs där du vill att hello telemetri data toobe analyseras och visas på den här sidan. 
   
   * Om programmet hello konfigurerades för Application Insights under utvecklingen, väljer du den befintliga resursen.
   * Annars skapar du en ny resurs med namnet för hello app. Om det finns andra appar som är komponenter av hello samma system placeras i hello samma resursgrupp, toomake åtkomst toohello telemetri enklare toomanage.
     
     Du kan ändra inställningarna senare.
     
     ![På fliken för Application Insights-inställningar, klicka på Logga in och ange autentiseringsuppgifterna för ditt Microsoft-konto för Azure. Välj en prenumeration, resursgrupp och resurs.](./media/app-insights-scom/060.png)
3. Fullständig hello guiden.
   
    ![Klicka på Skapa](./media/app-insights-scom/070.png)

Upprepa proceduren för varje app som du vill toomonitor.

Om du senare behöver toochange inställningar, övervaka hello öppnar egenskaperna för hello från hello redigering fönster.

![I redigering, Välj .NET Application Performance Monitoring med Application Insights, Välj din Övervakare och klickar på Egenskaper.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a>Kontrollera övervakning
hello Övervakare för att du har installerat sökningar för din app på varje server. Om den hittar hello app konfigurerar den Application Insights Status Monitor toomonitor hello app. Om det behövs installerar först Status Monitor på hello-servern.

Du kan kontrollera vilka instanser av hello app har identifierats:

![Öppna i övervakning, Programinsikter](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a>Visa telemetri i Application Insights
I hello [Azure-portalen](https://portal.azure.com), bläddra toohello resursen för din app. Du [Visa diagram som visar telemetri](app-insights-dashboards.md) från din app. (Om den inte visas på huvudsidan för hello ännu, klickar du på direktsänd dataström med mått.)

## <a name="next-steps"></a>Nästa steg
* [Ställ in en instrumentpanel](app-insights-dashboards.md) toobring tillsammans hello viktigaste diagram övervakning detta och andra appar.
* [Lär dig mer om mått](app-insights-metrics-explorer.md)
* [Konfigurera aviseringar](app-insights-alerts.md)
* [Diagnostisera prestandaproblem](app-insights-detect-triage-diagnose.md)
* [Kraftfulla Analytics-frågor](app-insights-analytics.md)
* [Tillgänglighetstester för webbprogram](app-insights-monitor-web-app-availability.md)

