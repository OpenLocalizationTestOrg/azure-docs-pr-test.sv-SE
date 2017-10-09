---
title: aaaView analytiska data i Azure Web Apps | Microsoft Docs
description: "Du kan använda hello Azure Web Apps Analytics lösning toogain insikter om dina Azure-webbprogram genom att samla in olika mått för alla webbprogram i Azure-resurser."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 20ff337f-b1a3-4696-9b5a-d39727a94220
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: banders
ms.openlocfilehash: 7e9725f95c9faf01da89184975ad5444dd19ff95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-analytic-data-for-metrics-across-all-your-azure-web-app-resources"></a>Visa analytiska data för mått för alla webbprogram i Azure-resurser

![Web Apps symbol](./media/log-analytics-azure-web-apps-analytics/azure-web-apps-analytics-symbol.png)  
hello Azure Web Apps Analytics (förhandsgranskning) lösningen ger insikter om din [Azure Web Apps](../app-service-web/app-service-web-overview.md) genom att samla in olika mått för alla webbprogram i Azure-resurser. Du kan analysera och Sök efter web app resurs mått data med hello-lösning.

Använda hello-lösning kan du visa den:

- Den översta Web Apps med hello högsta svarstid
- Antalet begäranden i dina webbprogram, inklusive lyckade och misslyckade begäranden
- Den översta Web Apps med högsta inkommande och utgående trafik
- Övre serviceplaner med hög användning av CPU och minne
- Loggen åtgärder i Azure Web Apps aktivitet

## <a name="connected-sources"></a>Anslutna källor

Till skillnad från de flesta andra logganalys-lösningar är inte samlas in för Azure Web Apps av agenter. Alla data som används av hello lösningen kommer direkt från Azure.

| Ansluten källa | Stöds | Beskrivning |
| --- | --- | --- |
| [Windows-agenter](log-analytics-windows-agents.md) | Nej | hello lösningen samlar inte in information från Windows-agenter. |
| [Linux-agenter](log-analytics-linux-agents.md) | Nej | hello lösningen samlar inte in information från Linux-agenter. |
| [SCOM-hanteringsgrupp](log-analytics-om-agents.md) | Nej | hello lösningen samlar inte in information från agenter i en ansluten SCOM-hanteringsgrupp. |
| [Azure Storage-konto](log-analytics-azure-storage.md) | Nej | hello-lösningen stöder inte samlingsinformation från Azure storage. |

## <a name="prerequisites"></a>Krav

- Du måste ha en Azure-prenumeration tooaccess mått resursinformation Azure Web App.

## <a name="configuration"></a>Konfiguration

Utför följande steg tooconfigure hello Azure Web Apps Analytics lösningen för dina arbetsytor hello.

1. Aktivera hello Azure Web Apps Analytics lösningar från [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureWebAppsAnalyticsOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).
2. [Aktivera loggning tooOMS med Azure-resurs mått med hjälp av PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell).

hello Azure Web Apps Analytics lösningen samlar in två olika mått från Azure:

- Azure Web Apps-mått
  - Genomsnittlig minne arbetsminne
  - Genomsnittlig svarstid
  - Byte mottagna/skickas
  - CPU-tid
  - Begäranden
  - Minne arbetsminne
  - Httpxxx
- App Service-Plan mått
  - Byte mottagna/skickas
  - CPU-procent
  - Diskkölängd
  - HTTP-Kölängd
  - Minnesprocent

App Service-Plan mått samlas endast in om du använder en särskild service-plan. Detta gäller inte toofree eller delade App Service-planer.

Om du lägger till hello-lösning med hjälp av hello OMS-portalen visas hello följande panelen. Du behöver för[aktivera Azure-resurs mått loggning tooOMS med hjälp av PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell).

![Utför Assessment meddelande](./media/log-analytics-azure-web-apps-analytics/performing-assessment.png)

När du har konfigurerat hello lösning data ska börja flödar tooyour arbetsytan inom 15 minuter.

## <a name="using-hello-solution"></a>Med hello-lösning

När du lägger till hello Azure Web Apps Analytics lösning tooyour arbetsytan hello **Azure Web Apps Analytics** panel har lagts tooyour översikt över instrumentpanelen. Den här panelen visar antalet hello antal Azure Web Apps att hello-lösningen har åtkomst tooin din Azure-prenumeration.

![Azure Web Apps Analytics sida vid sida](./media/log-analytics-azure-web-apps-analytics/azure-web-apps-analytics-tile.png)

### <a name="view-azure-web-apps-analytics-information"></a>Visa information om Azure Web Apps Analytics

Klicka på hello **Azure Web Apps Analytics** panelen tooopen hello **Azure Web Apps Analytics** instrumentpanelen. hello instrumentpanelen innehåller hello blad i hello i den följande tabellen. Varje bladet visar upp tooten objekt som matchar att bladets kriterier för hello angetts scope och ett tidsintervall. Du kan köra en sökning i loggen som returnerar alla poster genom att klicka på **se alla** längst ned hello hello bladet eller genom att klicka på hello bladet sidhuvud.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

| Kolumn | Beskrivning |
| --- | --- |
| Azure Webbappar |   |
| Trender för Web Apps begäran | Visar ett linjediagram för hello Web Apps begäran trend för hello datumintervallet som du har valt och visar en lista över hello översta tio webbegäranden. Klicka på hello rad diagram toorun en logg sökning efter<code>Type=AzureMetrics ResourceId=*"/MICROSOFT.WEB/SITES/"* (MetricName=Requests OR MetricName=Http*) &#124; measure avg(Average) by MetricName interval 1HOUR</code> <br>Klicka på en webbserver begäran objektet toorun en logg-sökning för hello web begäran mått trender som begär. |
| Svarstid för Web Apps | Visar ett linjediagram för hello Web Apps svarstiden för hello datumintervall som du har valt. Dessutom visas en lista över hello översta tio Web Apps svar gånger. Klicka på hello diagram toorun en logg sökning efter<code>Type:AzureMetrics ResourceId=*"/MICROSOFT.WEB/SITES/"* MetricName="AverageResponseTime" &#124; measure avg(Average) by Resource interval 1HOUR</code><br> Klicka på en Web App toorun en logg sökning returnerar svarstider för hello Web App. |
| Appar webbtrafik | Visar ett linjediagram för Web Apps trafik i MB och visar hello upp Web Apps trafik. Klicka på hello diagram toorun en logg sökning efter<code>Type:AzureMetrics ResourceId=*"/MICROSOFT.WEB/SITES/"*  MetricName=BytesSent OR BytesReceived &#124; measure sum(Average) by Resource interval 1HOUR</code><br> Den visar alla webbprogram med trafik för hello senaste minuten. Klicka på en Web App toorun en logg sökning visar antal byte som tagits emot och skickats för hello Web App. |
| Azure App Service-planer |   |
| App Service-planer med CPU-användning &gt; 80% | Visar hello Totalt antal App Service-planer som processoranvändningen som är större än 80% och visar hello topp 10 Apptjänstplaner per CPU-användning. Klicka på hello Totalt område toorun en logg sökning efter<code>Type=AzureMetrics ResourceId=*"/MICROSOFT.WEB/SERVERFARMS/"* MetricName=CpuPercentage &#124; measure Avg(Average) by Resource</code><br> Den visar en lista med din App Service-planer och deras Genomsnittlig CPU-belastning. Klicka på en Apptjänstplan toorun en logg sökning visar den genomsnittliga processoranvändningen. |
| App Service-planer med minnesanvändning &gt; 80% | Visar hello Totalt antal App Service-planer som minnesanvändningen är större än 80% och visar hello topp 10 Apptjänstplaner av minnesanvändning. Klicka på hello Totalt område toorun en logg sökning efter<code>Type=AzureMetrics ResourceId=*"/MICROSOFT.WEB/SERVERFARMS/"* MetricName=MemoryPercentage &#124; measure Avg(Average) by Resource</code><br> Den visar en lista med din App Service-planer och deras genomsnittliga minnesanvändningen. Klicka på en Apptjänstplan toorun en logg sökning visar den genomsnittliga minnesanvändningen. |
| Azure Web Apps aktivitetsloggar |   |
| Azure Web Apps aktivitet granskning | Visar hello Totalt antal Web Apps med [aktivitetsloggar](log-analytics-activity.md) och visar hello topp 10 aktivitet loggen operations. Klicka på hello Totalt område toorun en logg sökning efter<code>Type=AzureActivity ResourceProvider= "Azure Web Sites" &#124; measure count() by OperationName</code><br> Den visar en lista över hello aktivitet loggen åtgärder. Klicka på en aktivitet loggen igen toorun en logg sökning som visar hello poster för hello igen. |



### <a name="azure-web-apps"></a>Azure Web Apps

I instrumentpanelen för hello detaljnivån tooget större insyn i Web Apps-mått. Den här första uppsättning blad visar hello trend hello Web Apps begäranden, antalet fel (till exempel HTTP404), trafik och genomsnittlig svarstid över tid. Den visar också en sammanställning av dessa mått för olika Web Apps.

![Blad för Azure Webbappar](./media/log-analytics-azure-web-apps-analytics/web-apps-dash01.png)

En primär orsak visar att data är så att du kan identifiera en Webbapp med hög svarstid och undersöka toofind hello orsaken. En tröskelvärdet är också tillämpade toohelp mer enkelt identifiera hello viktiga problem.

- Webbprogram som visas i rött har svarstid som är högre än 1 sekund.
- Web Apps visas med orange färg har en svarstid som är högre än 0.7 sekund och mindre än 1 sekund.
- Webbprogram som visas i grönt har en svarstid som är mindre än 0,7 andra.

I följande Exempelbild för sökning i loggen hello, ser du att hello *anugup3* webbprogrammet hade en mycket högre svarstid än hello andra webbprogram.

![loggen Sök-exempel](./media/log-analytics-azure-web-apps-analytics/web-app-search-example.png)

### <a name="app-service-plans"></a>Apptjänstplaner

Om du använder dedicerade Service-planer, du kan också samla in mått för din App Service-planer. Den här vyn visas i din App Service-planer med hög CPU eller minne användning (&gt; 80%). Där visas också hello översta apptjänster med hög minne eller CPU-användning. På samma sätt är en tröskelvärdet tillämpade toohelp mer enkelt identifiera hello viktiga problem.

- Apptjänstplaner visas i rött har en processor/minnesanvändning högre än 80%.
- Programtjänstplaner visas med orange färg har en processor/minnesanvändning högre än 60% och lägre än 80%.
- Apptjänstplaner visas i grönt har en processor/minnesanvändning lägre än 60%.

![Blad för Azure App Service-planer](./media/log-analytics-azure-web-apps-analytics/web-apps-dash02.png)

## <a name="azure-web-apps-log-searches"></a>Azure Web Apps loggen sökningar

Hej **lista av populära Azure Web Apps sökfrågor** visar alla hello relaterade aktivitetsloggar för webbprogram som tillhandahåller insikter om hello-åtgärder som utfördes på resurserna i Web Apps. Dessutom visas alla hello relaterade åtgärder och hello antalet gånger som de har inträffat.

Använder hello loggen sökfrågor som utgångspunkt, kan du enkelt skapa en avisering. Du kanske exempelvis vill toocreate en avisering när genomsnittlig svarstid för ett mått som är större än 1 gång i sekunden.

## <a name="next-steps"></a>Nästa steg

- Skapa en [avisering](log-analytics-alerts-creating.md) för ett specifikt mått.
- Använd [loggen Sök](log-analytics-log-searches.md) tooview detaljerad information från din aktivitetsloggar.
