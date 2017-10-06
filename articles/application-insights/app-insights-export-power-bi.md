---
title: "aaaExport tooPower BI från Application Insights | Microsoft Docs"
description: "Analytics-frågor kan visas i Power BI."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: 6668cd7f4e0fbf41695972617f5f8ec207356659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="feed-power-bi-from-application-insights"></a>Powerbi-feed från Application Insights
[Power BI](http://www.powerbi.com/) är en uppsättning verktyg för business analytics som hjälper dig att analysera data och dela information. Omfattande instrumentpaneler är tillgängliga på varje enhet. Du kan kombinera data från flera källor, inklusive Analytics-frågor från [Azure Application Insights](app-insights-overview.md).

Det finns tre rekommenderade metoder för att exportera Application Insights data tooPower BI. Du kan använda dem enskilt eller tillsammans.

* [**Power BI kortet** ](#power-pi-adapter) -Ställ in en komplett instrumentpanel för telemetri från din app. hello uppsättning diagram är fördefinierade, men du kan lägga till egna frågor från andra källor.
* [**Exportera Analytics frågor** ](#export-analytics-queries) -skriva någon fråga som du vill med hjälp av Analytics och exportera det tooPower BI. Du kan placera den här frågan på en instrumentpanel tillsammans med andra data.
* [**Den löpande exporten och Stream Analytics** ](app-insights-export-stream-analytics.md) -detta innebär mer arbete tooset upp. Det är användbart om du vill tookeep dina data under långa perioder. Annars rekommenderas hello andra metoder som.

## <a name="power-bi-adapter"></a>Power BI-kort
Den här metoden skapas en komplett instrumentpanel för telemetri. hello inledande datauppsättning är fördefinierade, men du kan lägga till mer data tooit.

### <a name="get-hello-adapter"></a>Hämta hello nätverkskort
1. Logga in för[Power BI](https://app.powerbi.com/).
2. Öppna **hämta Data**, **Services**, **Application Insights**
   
    ![Hämta från Application Insights-datakälla](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. Ange hello information om Application Insights-resurs.
   
    ![Hämta från Application Insights-datakälla](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. Vänta en minut eller två för hello data toobe importeras.
   
    ![Power BI-kort](./media/app-insights-export-power-bi/010.png)

Du kan redigera hello instrumentpanelen kombinera hello Application Insights diagram med de andra källor och med Analytics-frågor. Det finns ett visualiseringen galleri där du kan få fler diagram och varje diagram har en parametrar som du kan ange.

Efter hello fortsätter importen, hello instrumentpanel och rapporter för hello tooupdate dagligen. Du kan styra hello uppdateringsschema för hello datauppsättningen.

## <a name="export-analytics-queries"></a>Exportera Analytics-frågor
Den här vägen gör toowrite alla Analytics fråga du och sedan exportera den tooa Power BI-instrumentpanelen. (Du kan lägga till toohello instrumentpanel som har skapats av hello-kort).

### <a name="one-time-install-power-bi-desktop"></a>En gång: Installera Power BI Desktop
tooimport Application Insights-frågan du använder hello skrivbordsversionen av Power BI. Men sedan kan du publicera den toohello web eller tooyour Power BI molnet arbetsytan. 

Installera [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Exportera en Analytics-fråga
1. [Öppna Analytics och skriva en fråga](app-insights-analytics-tour.md).
2. Testa och finjustera hello frågan tills du är nöjd med resultaten hello.

   **Kontrollera att den hello-frågan körs korrekt i Analytics innan du exporterar den.**
3. På hello **exportera** -menyn, Välj **Power BI (M)**. Spara hello textfil.
   
    ![Exportera Power BI-fråga](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. I Power BI Desktop select **hämta Data, tom fråga** och sedan i hello-frågan editor under **visa** Välj **avancerade frågeredigeraren**.

    Klistra in hello exporteras M språk skript i hello avancerade frågeredigeraren.

    ![Avancerade frågeredigeraren](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. Du kanske tooprovide autentiseringsuppgifter tooallow Power BI tooaccess Azure. Använd 'organisationskonto' toosign in med ditt Microsoft-konto.
   
    ![Ange autentiseringsuppgifter för Azure tooenable Power BI toorun Application Insights-fråga](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    (Om du behöver tooverify hello autentiseringsuppgifter kommandot hello inställningar för datakälla-menyn i hello frågeredigeraren. Var noggrann toospecify hello autentiseringsuppgifter som du använder för Azure, vilket kan skilja sig från dina autentiseringsuppgifter för Power BI.)
2. Välj en visualisering för din fråga och välj hello fält för x-axeln, y-axeln och segmentera dimension.
   
    ![Välj visualiseringen](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. Publicera din tooyour Power BI molnet rapportarbetsytan. Du kan bädda in en synkroniserade version till andra webbsidor därifrån.
   
    ![Välj visualiseringen](./media/app-insights-export-power-bi/publish-power-bi.png)
4. Uppdatera hello rapporten manuellt med intervall, eller ställa in en schemalagd uppdatering på alternativsidan för hello.

## <a name="troubleshooting"></a>Felsökning

### <a name="401-or-403-unauthorized"></a>401 eller 403 obehörig 
Detta kan inträffa om refesh-token inte har uppdaterats. Försök dessa steg tooensure fortfarande ha åtkomst. Öppna ett supportärende om du har åtkomst och refershing hello autentiseringsuppgifter fungerar inte.

1. Logga in på hello Azure-portalen och kontrollera att du kan komma åt hello resurs
2. Försök toorefresh hello autentiseringsuppgifter för hello instrumentpanelen

### <a name="502-bad-gateway"></a>502 felaktig Gateway
Detta orsakas normalt av en Analytics-fråga som returnerar för mycket data. Du bör försöka använda en mindre tidsintervall eller genom att använda hello [sedan](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) eller [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) fungerar endast [projekt](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) hello fält som du behöver.

Om att minska hello datauppsättningen kommer från hello Analytics-fråga inte uppfyller dina krav bör du använda hello [API](https://dev.applicationinsights.io/documentation/overview) toopull en större datamängd. Här följer instruktioner för hur tooconvert hello M-Query exportera toouse hello API.

1. Skapa en [API-nyckel](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)
2. Uppdatera hello Power BI M-skript som du exporterade från Analytics genom att ersätta hello ARM-URL: en med AI-API (se exemplet nedan)
   * Ersätt **https://management.azure.com/subscriptions/...**
   * med, **https://api.applicationinsights.io/beta/apps/...**
3. Slutligen uppdatera autentiseringsuppgifterna toobasic och Använd API-nyckel
  

**Befintligt skript**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Uppdaterat skript**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>Om provtagning
Om ditt program skickar stora mängder data, hello anpassningsbar provtagning funktionen fungerar och skicka endast en del av din telemetri. Detsamma gäller om du har manuellt provtagning i hello SDK eller på införandet hello. [Läs mer om sampling.](app-insights-sampling.md)


## <a name="next-steps"></a>Nästa steg
* [Power BI - Läs](http://www.powerbi.com/learning/)
* [Analytics-självstudier](app-insights-analytics-tour.md)

