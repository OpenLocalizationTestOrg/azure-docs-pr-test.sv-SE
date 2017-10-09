---
title: "aaaLog analys för Azure CDN | Microsoft Docs"
description: "Kunden kan aktivera logganalys för Azure CDN."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: v-semcev
ms.openlocfilehash: 56e5a4fec46fd156cf38252732afb4522741d009
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-logs-for-azure-cdn"></a>Diagnostik-loggarna för Azure CDN

När du aktiverar CDN för ditt program, kommer du förmodligen vill toomonitor hello CDN användning, kontrollera din leverans hello hälsa och felsöka problem. Azure CDN ger dessa funktioner med [CDN Core Analytics](cdn-analyze-usage-patterns.md) och [diagnostikloggar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)

## <a name="cdn-core-analytics"></a>CDN Core Analytics
Som en aktuell Azure CDN-användare med Verizon standard eller premium-profil har du redan kan tooview core analytics hello kompletterande portalen tillgänglig via hello ”hantera” alternativet från hello Azure-portalen. 

## <a name="azure-diagnostic-logs"></a>Azure diagnostikloggar

Azure med denna nya funktion, du kan nu visa core analytics och spara dem i en eller flera mål, inklusive:

 - Azure-lagringskonto
 - Azure Event Hubs
 - [OMS Log Analytics-databasen](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
 Den här funktionen är tillgänglig för alla CDN-slutpunkter som hör tooVerizon (Standard och Premium) och Akamai (Standard) CDN profiler.

Diagnostik loggar Tillåt tooexport grundläggande användningsstatistik från din CDN-slutpunkten tooa olika källor så att du kan använda dem i ett anpassat sätt. Du kan till exempel göra hello följande typer av export av data:

- Exportera tooblob datalagring, exportera tooCSV och skapa diagram i excel.
- Exportera data tooevent NAV och korrelera med data från andra azure-tjänster.
- Exportera data toolog analytics och visa data i din egen OMS-arbetsyta

hello visar följande bild en typisk CDN Core Analytics överblick över data.

![Portal - diagnostik loggar](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*Bild 1 - CDN Core Analytics visa*

hello efter genomgången genomgår hello schemat för hello core analysdata, steg som ingår i hello funktionen aktiveras och leverera dem toovarious mål och förbrukar från dessa mål.

## <a name="enable-logging-with-azure-portal"></a>Aktivera loggning med Azure-portalen

> [!NOTE]
> hello diagnostik loggar aktiveras **av** som standard. 

Så hello nedan tooenable loggning med CDN Core Analytics:

Logga in toohello [Azure-portalen](http://portal.azure.com). Om du inte redan har CDN som aktiverats för ditt arbetsflöde [aktivera Azure CDN](cdn-create-new-endpoint.md) innan du fortsätter.

1. I hello portal, navigerar för**CDN-profilen**.
2. Välj en CDN-profil och sedan hello CDN-slutpunkt som du vill tooenable **diagnostik loggar**.

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. Gå för**diagnostik loggar** bladet Under **övervakning** och sedan ändra hello status för**på**.

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Aktivera loggning med Azure Storage
    
toouse Azure Storage toostore hello loggar, Välj **Arkivera tooa lagringskonto**, Välj kvarhållning dagar och på **CoreAnalytics** under **loggen**.

![Portal - diagnostik loggar](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png)

*Bild 2 - loggning med Azure Storage*

### <a name="logging-with-oms-log-analytics"></a>Loggning med OMS logganalys

toouse OMS logganalys toostore hello loggar så här:

1. Från hello **diagnostik loggar** bladet Under **övervakning**väljer **skicka tooLog Analytics** från 

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Konfigurera hello logganalys loggning genom att klicka på Konfigurera. Då kommer du tooa dialogrutan där du kan välja en tidigare arbetsyta eller skapa en ny.

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. Klicka på **Skapa ny arbetsyta**.

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/07_Create-new.png)

4. Därefter måste du välja ett nytt Arbetsytenamn, befintliga prenumeration, resursgrupp (ny eller befintlig), plats och prisnivå. Du har hello möjlighet att fästa den här konfigurationen tooyour instrumentpanelen. Klicka på OK toocomplete hello konfiguration.

    Därefter bör du se din arbetsyta med din OMS-arbetsytan och resurs gruppnamn. Namn måste vara unikt och kan bara använda bokstäver, siffror och bindestreck. Blanksteg och understreck tillåts inte. 

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/08_Workspace-resource.png)

5. Du får sedan ett kort meddelande om att ditt arbetsområde har skapats och du kommer tillbaka tooyour loggning skärm för konfiguration. Du kan bekräfta hello namnet på logganalys-arbetsytan.

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/09_Return-to-logging.png)

    När du har konfigurerat hello konfiguration för logganalys se till att du även kryssrutan hello CoreAnalytics för CDN-loggning.

6. Om allt tooyour önskemål klickar du på hello **spara** knappen hello överst i hello konfigurationsdialogruta.

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/10_Save-me.png)

    Hej **spara** knappen inte längre är aktiv och att hello på/av-knappen är nu vidare men blå lila.

7. Om du vill toosee din nya OMS-arbetsyta, gå tooyour Azure-portalen instrumentpanelen, klickar du på hello för logganalys-arbetsytan. Nu visas ditt arbetsområde (se till att OMS-arbetsyta är markerad hello vänster). Klicka på hello OMS-portalen panelen toosee arbetsytan i hello OMS-databas. 

    ![Portal - diagnostik loggar](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    OMS-databasen är nu redo toolog data. Ordna tooconsume data måste du använda en [OMS lösningen](#consuming-oms-log-analytics-data), tas upp senare i den här artikeln.

Mer information om logga data fördröjningar [här](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>Aktivera loggning med PowerShell

Nedan finns ett exempel på hur tooenable och få diagnostikloggar via hello Azure PowerShell-Cmdlets.

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a>Aktivera diagnostik loggar i ett Lagringskonto

Först logga in och välj en prenumeration:

    Login-AzureRmAccount 

    Select-AzureSubscription -SubscriptionId 


tooEnable diagnostikloggar i ett Lagringskonto, Använd följande kommando:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```
tooEnable diagnostik loggar i OMS-arbetsyta, Använd följande kommando:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
```



## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Förbrukar diagnostik loggar från Azure Storage
Det här avsnittet beskriver hello schemat för hello CDN core analytics, hur de är uppdelade i ett Azure Storage-konto och ger exempel kod toodownload hello loggar i tooa CSV-filen.

### <a name="using-microsoft-azure-storage-explorer"></a>Med Microsoft Azure Lagringsutforskaren
Innan du kan komma åt hello core analysdata från hello Azure Storage-konto, måste du först verktyget tooaccess hello innehållet i ett lagringskonto. Det finns flera verktyg i hello marknaden, är hello något som vi rekommenderar hello Microsoft Azure Lagringsutforskaren. Du kan hämta verktyget hello från [här](http://storageexplorer.com/). När du hämtar och installerar programmet hello, konfigurerar den hello toouse samma Azure Storage-konto som har konfigurerats som en destination toohello CDN diagnostik loggar.

1.  Öppna **Lagringsutforskaren för Microsoft Azure**
2.  Leta upp hello storage-konto
3.  Gå toohello **”Blob-behållare”** nod under lagringen kontot och expandera hello nod
4.  Välj hello behållare med namnet **”insikter-loggar-coreanalytics”** och dubbelklicka på det.
5.  Resultatet visar upp i hello högra fönstret från och med hello först nivå, som ser ut så **”resourceId =”**. Klickar du på alla hello sätt tills du ser hello filen **PT1H.json**. Se hello följande Obs förklaring av hello sökväg.
6.  Varje blobb **PT1H.json** representerar hello analytics loggar under en timme för en specifik CDN-slutpunkten eller den anpassa domänen.
7.  hello schemat för hello innehållet i JSON-fil beskrivs i hello schemat i hello Core Analytics loggar


> [!NOTE]
> **Formatet för BLOB-sökväg**
> 
> Core Analytics loggar skapas varje timme. Alla data för en timme som samlas in och lagras i en enda Azure-Blob som en JSON-nyttolast. hello sökvägen toothis Azure Blob visas som om det är en hierarkisk struktur. Detta är eftersom hello lagring explorer verktyget tolkar '/' som en katalogavgränsare som visar hello hierarki i informationssyfte. Hello hela sökvägen representerar egentligen, just hello blob-namnet. Det här namnet på hello blob följer hello följande namngivningskonvention 
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

**Beskrivning av fält:**

|värde|description|
|-------|---------|
|Prenumerations-ID:t    |ID för hello Azure-prenumeration. Detta är i hello Guid-format.|
|Resurs |Gruppnamn namn hello resurs grupp toowhich hello CDN resurser tillhör.|
|Profilnamn |Namnet på hello CDN-profilen|
|Namnet på slutpunkten |Namnet på hello CDN-slutpunkten|
|År|  4-siffrig representation av hello år exempelvis 2017|
|Månad| 2-siffrig representation av hello månadsnummer. 01 = januari... 12 = December|
|Dag|   2 siffra representation av hello dagen i månaden för hello|
|PT1H.JSON| Faktiska JSON-fil som innehåller hello analytics-data|

### <a name="exporting-hello-core-analytics-data-tooa-csv-file"></a>Exportera hello Core analysdata tooa CSV-fil

toomake it lätt tooaccess hello Core Analytics vi erbjuder en exempelkod för ett verktyg som gör att ladda ned hello JSON-filer till en platt CSV-format, vilket kan vara används tooeasily skapa diagram eller andra aggregeringar.

Här är hur du kan använda verktyget hello:

1.  Gå hello github länk: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )
2.  Hämta hello koden
3.  Följ anvisningarna toocompile och konfigurera
4.  Kör verktyget hello
5.  Resulterande CSV-fil innehåller hello analytics-data i en enkel platt hierarki.

## <a name="consuming-diagnostics-logs-from-an-oms-log-analytics-repository"></a>Förbrukar diagnostik loggar från en OMS Log Analytics-databas
Log Analytics är en tjänst i Operations Management Suite (OMS) och som övervakar dina molntjänster och lokala miljöer toomaintain deras tillgänglighet och prestanda. Den samlar in data som genereras av resurser i dina miljöer i molnet och lokalt och från andra övervakning verktyg tooprovide analys över flera källor. 

toouse Log Analytics, måste du [aktivera loggning](#enable-logging-with-azure-storage) toohello Azure logganalys för OMS-lagringsplatsen, vilket beskrivs tidigare i den här artikeln.

### <a name="using-hello-oms-repository"></a>Med hjälp av hello OMS-databasen

 följande diagram visar hello arkitekturen för hello indata och utdata för hello databasen hello:

![OMS Log Analytics-databasen](./media/cdn-diagnostics-log/12_Repo-overview.png)

*Bild 3 - Log Analytics-databasen*

Du kan visa hello data i en mängd olika sätt med hjälp av lösningar för hantering. Du kan få hanteringslösningar från hello [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Du kan installera hanteringslösningar från Azure marketplace genom att klicka på hello **blir det nu** länken längst ned hello i varje lösning.

### <a name="adding-an-oms-cdn-management-solution"></a>Lägga till en OMS CDN hanteringslösning

Följ dessa steg tooadd en lösning:

1.   Om du inte redan har gjort logga in toohello Azure-portalen med din Azure-prenumeration och gå tooyour instrumentpanelen.
    ![Instrumentpanelen för Azure](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. I hello **ny** bladet under **Marketplace**väljer **övervakning + management**.

    ![Marketplace](./media/cdn-diagnostics-log/14_Marketplace.png)

3. I hello **övervakning + management** bladet, klickar du på **se alla**.

    ![Se alla](./media/cdn-diagnostics-log/15_See-all.png)

4.  Sök efter CDN i hello sökrutan.

    ![Se alla](./media/cdn-diagnostics-log/16_Search-for.png)

5.  Välj **Azure CDN Core Analytics**. 

    ![Se alla](./media/cdn-diagnostics-log/17_Core-analytics.png)

6.  När du klickar på **skapa**, kommer du att uppmanas ange toocreate en ny OMS-arbetsyta eller använda en befintlig. 

    ![Se alla](./media/cdn-diagnostics-log/18_Adding-solution.png)

7.  Välj hello arbetsytans innan. Du måste sedan tooadd ett automation-konto.

    ![Se alla](./media/cdn-diagnostics-log/19_Add-automation.png)

8. hello visar följande skärmbild hello automation kontoformuläret måste du fylla ut. 

    ![Se alla](./media/cdn-diagnostics-log/20_Automation.png)

9. När du har skapat hello automation-konto, är du redo tooadd din lösning. Klicka på hello **skapa** knappen.

    ![Se alla](./media/cdn-diagnostics-log/21_Ready.png)

10. Din lösning har nu lagts tooyour arbetsytan. Gå tillbaka tooyour Azure-portalen instrumentpanelen.

    ![Se alla](./media/cdn-diagnostics-log/22_Dashboard.png)

    Klicka på hello logganalys-arbetsytan som du skapade toogo tooyour arbetsytan. 

11. Klicka på hello **OMS-portalen** panelen toosee den nya lösningen i hello OMS-portalen.

    ![Se alla](./media/cdn-diagnostics-log/23_workspace.png)

12. OMS-portalen bör nu se ut som följande skärmbild hello:

    ![Se alla](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Klicka på en hello paneler toosee flera vyer i dina data.

    ![Se alla](./media/cdn-diagnostics-log/25_Interior-view.png)

    Du kan rulla åt vänster eller höger toosee ytterligare rutor som representerar enskilda vyer i hello data. 

    Klicka på någon av hello paneler får du mer information om dina data.

     ![Se alla](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Erbjudanden och prisnivåer

Du kan se erbjudanden och prisnivåer för OMS hanteringslösningar [här](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).

### <a name="customizing-views"></a>Anpassa vyer

Du kan anpassa hello vyn i dina data med hjälp av hello **Vydesigner**. Gå tooyour OMS-arbetsytan och börjar designa genom att klicka på hello **Vydesigner** panelen.

![Vydesigner](./media/cdn-diagnostics-log/27_Designer.png)

Du kan dra diagramtyper från hello vänster och Fyll i hello information om du vill tooanalyze på hello kvar.

![Vydesigner](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Logga data fördröjningar

Verizon logga data fördröjningar | Akamai logga data fördröjningar
--- | ---
Loggdata för Verizon försenas 1 timme och tar upp too2 timmar toostart visas efter att endpoint-spridningen har slutförts. | Loggdata för Akamai är 24 timmar fördröjd och tar upp too2 timmar toostart visas om det skapades mer än 24 timmar sedan. Om den nyligen har skapats kan det ta upp too25 timmar för hello loggar toostart visas.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>Typer av diagnostiska loggen för CDN Core Analytics

Vi har för närvarande endast Core Analytics loggarna, vilket innehåller mått som visar statistik för HTTP-svar och utgång sett från hello CDN POP/kanter.

### <a name="core-analytics-metrics-details"></a>Core Analytics mätvärden information
hello följande tabell visar en lista över tillgängliga i hello Core Analytics loggar mått. Inte alla mått är tillgängliga från alla leverantörer, även om dessa skillnader är minimal. hello också följande tabell visar om ett visst mått är tillgänglig från en leverantör. Observera att hello mått är tillgängliga för bara de CDN-slutpunkter som har trafik på dem..


|Mått                     | Beskrivning   | Verizon  | Akamai 
|---------------------------|---------------|---|---|
| RequestCountTotal         |Totalt antal träffar för begäran under denna period| Ja  |Ja   |
| RequestCountHttpStatus2xx |Antal alla begäranden som resulterade i en 2xx http-kod (t.ex. 200, 202)              | Ja  |Ja   |
| RequestCountHttpStatus3xx | Antal alla begäranden som resulterade i en 3xx http-kod (t.ex. 300, 302)              | Ja  |Ja   |
| RequestCountHttpStatus4xx |Antal alla begäranden som resulterade i en 4xx http-kod (t.ex. 400, 404)               | Ja   |Ja   |
| RequestCountHttpStatus5xx | Antal alla begäranden som resulterade i en 5xx http-kod (t.ex. 500, 504)              | Ja  |Ja   |
| RequestCountHttpStatusOthers |  Uppräkning av alla andra HTTP-koder (utanför 2xx 5xx) | Ja  |Ja   |
| RequestCountHttpStatus200 | Antal alla begäranden som resulterade i en 200 HTTP-svar för kod              |Nej   |Ja   |
| RequestCountHttpStatus206 | Antal alla begäranden som resulterade i ett 206 HTTP-svar för kod              |Nej   |Ja   |
| RequestCountHttpStatus302 | Antal alla begäranden som resulterade i en 302 HTTP-svar för kod              |Nej   |Ja   |
| RequestCountHttpStatus304 |  Antal alla begäranden som resulterade i ett 304 HTTP-svar för kod             |Nej   |Ja   |
| RequestCountHttpStatus404 | Antal alla begäranden som resulterade i ett 404 HTTP-svar för kod              |Nej   |Ja   |
| RequestCountCacheHit |Antal alla begäranden som resulterade i ett antal träffar. Det innebär att hello tillgången behandlades direkt från hello POP toohello klienten.               | Ja  |Nej   |
| RequestCountCacheMiss | Antal alla begäranden som resulterade i en Cache-Miss. Detta innebär hello tillgången hittades inte på hello POP närmaste toohello klient och därför har hämtats från hello ursprung.              |Ja   | Nej  |
| RequestCountCacheNoCache | Antalet begäranden tooan tillgång som hindras från att cachelagras på grund av tooa Användarkonfiguration hello kant.              |Ja   | Nej  |
| RequestCountCacheUncacheable | Antalet begäranden tooassets hindras från att cachelagras av hello tillgången Cache-Control och upphör att gälla rubriker, vilket indikerar att det inte ska cachelagras på en POP eller av hello HTTP-klienten                |Ja   |Nej   |
| RequestCountCacheOthers | Antal begäranden med cachen inte omfattas av ovan.              |Ja   | Nej  |
| EgressTotal | Utgående dataöverföring i GB              |Ja   |Ja   |
| EgressHttpStatus2xx | Utgående data transfer * för svar med 2xx HTTP-statuskoder i GB            |Ja   |Nej   |
| EgressHttpStatus3xx | Utgående dataöverföring för svar med 3xx HTTP-statuskoder i GB              |Ja   |Nej   |
| EgressHttpStatus4xx | Utgående dataöverföring för svar med 4xx HTTP-statuskoder i GB               |Ja   | Nej  |
| EgressHttpStatus5xx | Utgående dataöverföring för svar med 5xx HTTP-statuskoder i GB               |Ja   |  Nej |
| EgressHttpStatusOthers | Utgående dataöverföring för svar med andra HTTP-statuskoder i GB                |Ja   |Nej   |
| EgressCacheHit |  Utgående dataöverföring för svar som har levererats direkt från hello CDN cache på hello CDN POP/kanter  |Ja   |  Nej |
| EgressCacheMiss | Utgående dataöverföring för svar som inte hittades på hello närmaste POP-server, och hämtas från hello ursprungsservern              |Ja   |  Nej |
| EgressCacheNoCache | Utgående dataöverföring för tillgångar som hindras från att cachelagras på grund av tooa Användarkonfiguration hello kant.                |Ja   |Nej   |
| EgressCacheUncacheable | Utgående dataöverföring för tillgångar som hindras från att cachelagras av hello tillgången Cache-Control eller Expires-huvuden som indikerar att det inte ska cachelagras på en POP eller av hello HTTP-klienten                    |Ja   | Nej  |
| EgressCacheOthers |  Utgående dataöverföringar för andra cache-scenarier.             |Ja   | Nej  |

* Utgående dataöverföring refererar tootraffic levereras från CDN POP servrar toohello klienten.


### <a name="schema-of-hello-core-analytics-logs"></a>Schemat för hello Core Analytics loggar 

Alla loggar lagras i JSON-format och varje post innehåller strängfält följande hello nedan schemat:

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of hello CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of hello domain for which hello statistics is reported>",
                "RequestCountTotal": integer value,
                "RequestCountHttpStatus2xx": integer value,
                "RequestCountHttpStatus3xx": integer value,
                "RequestCountHttpStatus4xx": integer value,
                "RequestCountHttpStatus5xx": integer value,
                "RequestCountHttpStatusOthers": integer value,
                "RequestCountHttpStatus200": integer value,
                "RequestCountHttpStatus206": integer value,
                "RequestCountHttpStatus302": integer value,
                "RequestCountHttpStatus304": integer value,
                "RequestCountHttpStatus404": integer value,
                "RequestCountCacheHit": integer value,
                "RequestCountCacheMiss": integer value,
                "RequestCountCacheNoCache": integer value,
                "RequestCountCacheUncacheable": integer value,
                "RequestCountCacheOthers": integer value,
                "EgressTotal": double value,
                "EgressHttpStatus2xx": double value,
                "EgressHttpStatus3xx": double value,
                "EgressHttpStatus4xx": double value,
                "EgressHttpStatus5xx": double value,
                "EgressHttpStatusOthers": double value,
                "EgressCacheHit": double value,
                "EgressCacheMiss": double value,
                "EgressCacheNoCache": double value,
                "EgressCacheUncacheable": double value,
                "EgressCacheOthers": double value,
            }
        }

    ]
}
```

Där representerar hello ”time” hello starttiden för hello timme gräns som hello statistik rapporteras. När ett mått inte stöds av en CDN-providern, i stället för ett värde för double eller heltal, ska det finnas ett null-värde. Null-värde som anger hello avsaknaden av ett mått och detta skiljer sig från värdet 0. Observera också att en uppsättning av de här måtten per domän som konfigurerats på hello slutpunkt.

Exempel egenskaper nedan:

```json
{
     "DomainName": "manlingakamaitest2.azureedge.net",
     "RequestCountTotal": 480,
     "RequestCountHttpStatus2xx": 480,
     "RequestCountHttpStatus3xx": 0,
     "RequestCountHttpStatus4xx": 0,
     "RequestCountHttpStatus5xx": 0,
     "RequestCountHttpStatusOthers": 0,
     "RequestCountHttpStatus200": 480,
     "RequestCountHttpStatus206": 0,
     "RequestCountHttpStatus302": 0,
     "RequestCountHttpStatus304": 0,
     "RequestCountHttpStatus404": 0,
     "RequestCountCacheHit": null,
     "RequestCountCacheMiss": null,
     "RequestCountCacheNoCache": null,
     "RequestCountCacheUncacheable": null,
     "RequestCountCacheOthers": null,
     "EgressTotal": 0.09,
     "EgressHttpStatus2xx": null,
     "EgressHttpStatus3xx": null,
     "EgressHttpStatus4xx": null,
     "EgressHttpStatus5xx": null,
     "EgressHttpStatusOthers": null,
     "EgressCacheHit": null,
     "EgressCacheMiss": null,
     "EgressCacheNoCache": null,
     "EgressCacheUncacheable": null,
     "EgressCacheOthers": null
}

```

## <a name="additional-resources"></a>Ytterligare resurser

* [Azure diagnostikloggar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Core analytics via Azure CDN kompletterande portalen](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [Azure OMS logganalys](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-overview)
* [Azure logganalys REST-API](https://docs.microsoft.com/en-us/rest/api/loganalytics)







