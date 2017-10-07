---
title: "aaaApplication insikter för Azure-molntjänster | Microsoft Docs"
description: "Övervaka webb- och arbetsroller effektivt med Application Insights"
services: application-insights
documentationcenter: 
keywords: WAD2AI, Azure Diagnostics
author: CFreemanwa
manager: carmonm
editor: alancameronwills
ms.assetid: 5c7a5b34-329e-42b7-9330-9dcbb9ff1f88
ms.service: application-insights
ms.devlang: na
ms.tgt_pltfrm: ibiza
ms.topic: get-started-article
ms.workload: tbd
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: 6956ce423eea1e2cf387bd98250bae32d9501ed0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-azure-cloud-services"></a>Application Insights för Azure Cloud Services
Du kan övervaka [Microsoft Azure Cloud-tjänstapparnas](https://azure.microsoft.com/services/cloud-services/) tillgänglighet, prestanda, fel och användning med [Application Insights][start] genom att kombinera data från Application Insights SDK:er med data från [Azure Diagnotics](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/azure-diagnostics) från Cloud Services. Du kan göra välgrundade beslut om hello riktning hello design med hello feedback du använda om hello prestanda och effektivitet för din app i hello, i varje utvecklingslivscykeln för.

![Exempel](./media/app-insights-cloudservices/sample.png)

## <a name="before-you-start"></a>Innan du börjar
Du behöver:

* En prenumeration med [Microsoft Azure](http://azure.com). Logga in med ett Microsoft-konto som du kanske har för Windows, XBox Live eller andra Microsoft-molntjänster. 
* Microsoft Azure Tools 2.9 eller senare
* Developer Analytics Tools 7.10 eller senare

## <a name="quick-start"></a>Snabbstart
Hej snabbaste och enklaste sättet toomonitor Molntjänsten med Application Insights är toochoose när du publicerar service-tooAzure.

![Exempel](./media/app-insights-cloudservices/azure-cloud-application-insights.png)

Det här alternativet instrument appen vid körning, ger alla hello telemetri om du behöver toomonitor begäranden, undantag och beroenden i din webbroll, samt prestanda prestandaräknare från worker-roller. Alla diagnostikspår som genererats av din app skickas även tooApplication insikter.

Om det är allt du behöver så är du klar! Nästa steg är [att visa mätvärden från appen](app-insights-metrics-explorer.md), [att köra frågor mot dina data med Analytics](app-insights-analytics.md) och eventuellt att konfigurera en [instrumentpanel](app-insights-dashboards.md). Du kanske vill att tooset [tillgänglighetstester](app-insights-monitor-web-app-availability.md) och [lägga till kod tooyour webbsidor](app-insights-javascript.md) toomonitor prestanda i hello webbläsare.

Men du har fler alternativ:

* Skicka data från olika komponenter och skapa konfigurationer tooseparate resurser.
* Lägg till anpassad telemetri från din app.

Om dessa alternativ är av intresse tooyou Läs på.

## <a name="sample-application-instrumented-with-application-insights"></a>Exempelprogram som instrumenteras med Application Insights
Ta en titt på den här [exempelprogram](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) som Application Insights läggs tooa Molntjänsten med två arbetsroller i Azure. 

Följande får du reda på hur tooadapt egna Molntjänsten projektet i hello samma sätt.

## <a name="plan-resources-and-resource-groups"></a>Planera resurser och resursgrupper
hello telemetri från din app lagras, analyseras och visas i en Azure-resurs av typen Application Insights. 

Varje resurs tillhör tooa resursgruppen. Resursgrupper används för att hantera kostnader för att bevilja åtkomst tooteam medlemmar och toodeploy uppdateras i en enda, samordnad transaktion. Du kan till exempel [skriva ett skript toodeploy](../azure-resource-manager/resource-group-template-deploy.md) en Azure-Molntjänsten och dess Application Insights-övervakning resurser i en åtgärd.

### <a name="resources-for-components"></a>Resurser för komponenter
hello bör schemat är toocreate en separat resurs för varje komponent i ditt program – det vill säga varje webbroll och en arbetsroll. Du kan analysera varje komponent separat, men du kan skapa en [instrumentpanelen](app-insights-dashboards.md) som sammanför hello viktiga diagram från alla hello-komponenter, så att du kan jämföra och övervaka dem tillsammans. 

Ett alternativ är toosend hello telemetri från mer än en roll toohello samma resurs, men [lägger till en dimension egenskapen tooeach telemetri](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer) som identifierar källroll. Mått diagram, till exempel undantag visas normalt en sammanställning av hello antal från hello olika roller i det här schemat, men du kan också segmentera hello diagram av hello Rollidentifierare vid behov. Du kan även filtrera sökningar av hello samma dimension. Detta alternativ gör det lite enklare tooview allt på hello samma tid, men även leda toosome förvirring mellan hello roller.

Webbläsaren telemetri ingår ofta i hello samma resurs som dess webbroll på serversidan.

Placera hello Application Insights-resurser för hello olika komponenter i en resursgrupp. Detta gör det enkelt toomanage dem tillsammans. 

### <a name="separating-development-test-and-production"></a>Avgränsa utveckling, testning och produktion
Om du utvecklar anpassade händelser för din nästa funktionen medan hello tidigare version är live, vill du toosend hello development telemetri tooa separat Application Insights-resurs. Annars blir hårda toofind din test telemetri bland alla hello trafik från hello liveplatsen.

tooavoid den här situationen kan skapa separata resurser för varje versionskonfiguration eller 'stämpeln' (utveckling, test, produktion,...) på datorn. Placera hello resurser för varje build-konfiguration i en separat resursgrupp. 

toosend hello telemetri toohello lämpliga resurser, du kan ställa in hello Application Insights SDK så att den hämtar en annan instrumentation nyckel beroende på hello build-konfiguration. 

## <a name="create-an-application-insights-resource-for-each-role"></a>Skapa en Application Insights-resurs för varje roll
Om du har valt toocreate en separat resurs för varje roll - och kanske en separat ange för varje build-konfiguration – så är det enklaste toocreate dem i hello Application Insights-portalen. (Om du skapar mycket resurser, kan du [automatisera hello](app-insights-powershell.md).

1. I hello [Azure-portalen][portal], skapa en ny Application Insights-resurs. Välj ASP.NET-app för programtyp. 

    ![Klicka på Nytt, Application Insights](./media/app-insights-cloudservices/01-new.png)
2. Observera att varje resurs identifieras av en instrumenteringsnyckel. Du kan behöva det senare om du vill toomanually konfigurera eller kontrollera hello konfigurationen av hello SDK.

    ![Klicka på egenskaper, markera hello nyckeln och tryck på ctrl + C](./media/app-insights-cloudservices/02-props.png) 

## <a name="set-up-azure-diagnostics-for-each-role"></a>Konfigurera Azure Diagnostics för varje roll
Ange det här alternativet toomonitor din app med Application Insights. För webbroller tillhandahåller det här alternativet prestandaövervakning, aviseringar och diagnostik, samt användningsanalys. Du kan söka och övervaka Azure diagnostics, till exempel omstart, prestandaräknare och anrop tooSystem.Diagnostics.Trace för andra roller. 

1. I Visual Studio Solution Explorer under &lt;YourCloudService&gt;, roller, öppna hello egenskaper för varje roll.
2. I **Configuration**, ange **skicka diagnostik data tooApplication insikter** och välj hello lämplig Application Insights-resurs som du skapade tidigare.

Om du har valt toouse en separat Application Insights-resurs för varje build-konfiguration, väljer du först hello konfiguration.

![Konfigurera Application Insights i hello-egenskaperna för varje roll för Azure](./media/app-insights-cloudservices/configure-azure-diagnostics.png)

Detta påverkar hello för att lägga till Application Insights instrumentation nycklarna i hello filer med namnet `ServiceConfiguration.*.cscfg`. ([Exempelkod](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/AzureEmailService/ServiceConfiguration.Cloud.cscfg)).

Om du vill toovary hello andelen diagnostisk information skickas tooApplication insikter kan du göra det [genom att redigera hello `.cscfg` filer direkt](app-insights-azure-diagnostics.md).

## <a name="sdk"></a>Installera hello SDK i varje projekt
Det här alternativet lägger till hello möjlighet tooadd anpassad telemetri tooany roll, för en närmare analys av hur programmet används och fungerar.

Konfigurera hello Application Insights SDK för varje moln app-projekt i Visual Studio.

1. **Web roller**: Högerklicka på hello-projektet och välj **konfigurera Application Insights** eller **Lägg till > Application Insights telemetri**.

2. **Arbetsroller**: 
 * Högerklicka på hello-projektet och välj **hantera Nuget-paket**.
 * Lägg till [Application Insights för Windows Server](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/).

    ![Sök efter ”Application Insights”](./media/app-insights-cloudservices/04-ai-nuget.png)

3. Konfigurera hello SDK toosend data toohello Application Insights-resurs.

    Ange hello instrumentation nyckel i en lämplig Start-funktion från hello Konfigurationsinställningen i hello .cscfg-filen:
 
    ```C#
   
     TelemetryConfiguration.Active.InstrumentationKey = RoleEnvironment.GetConfigurationSettingValue("APPINSIGHTS_INSTRUMENTATIONKEY");
    ```
   
    Gör detta för varje roll i programmet. Visa hello exempel:
   
   * [Webbroll](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Global.asax.cs#L27)
   * [Arbetsroll](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L232)
   * [För webbsidor](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Views/Shared/_Layout.cshtml#L13) 
4. Ange hello ApplicationInsights.config-filen toobe kopieras alltid toohello målkatalogen. 
   
    (I hello .config-fil visas meddelandena ber du tooplace hello instrumentation nyckeln det. Men för molnprogram är det bättre tooset från hello .cscfg-filen. Detta säkerställer hello rollen identifieras korrekt i hello-portalen.)

#### <a name="run-and-publish-hello-app"></a>Kör och publicera hello app
Kör appen och logga in i Azure. Öppna hello Application Insights-resurser som du har skapat och ser du enskilda datapunkter som visas i [Sök](app-insights-diagnostic-search.md), och aggregerade data i [mått Explorer](app-insights-metrics-explorer.md). 

Lägg till mer telemetri - finns hello avsnitten nedan - och sedan publicera din app tooget live diagnostik- och användningsdata feedback. 

#### <a name="no-data"></a>Ser du inga data?
* Öppna hello [Sök] [ diagnostic] panelen toosee enskilda händelser.
* Använda programmet hello, öppna olika sidor så att den genererar vissa telemetri.
* Vänta några sekunder och klicka på Uppdatera.
* Mer information finns i [Felsökning][qna].

## <a name="view-azure-diagnostic-events"></a>Visa Azure Diagnostics-händelser
Där toofind hello [Azure Diagnostics](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/azure-diagnostics) information i Application Insights:

* Prestandaräknare visas som anpassade mått. 
* Windows-händelseloggar visas som spårningar och anpassade händelser.
* Programloggar, ETW-loggar och diagnostikinfrastrukturloggar visas som spårningar.

toosee prestandaräknare och antal händelser, öppna [Metrics Explorer](app-insights-metrics-explorer.md) och Lägg till ett nytt diagram:

![Azure-diagnostikdata](./media/app-insights-cloudservices/23-wad.png)

Använd [Sök](app-insights-diagnostic-search.md) eller en [Analytics query](app-insights-analytics-tour.md) toosearch över hello olika spårningsloggar som skickas av Azure-diagnostik. Anta att du har ett ohanterat undantag som orsakade en roll toocrash och återvinning. Denna information kan visas i hello kanal i Windows händelselogg. Du kan använda Sök toolook på hello händelseloggen fel och hämta hello fullständig stack-spårning för hello undantag. Som hjälper dig att hitta hello grundorsaken till problemet hello.

![Söka i Azure Diagnostics](./media/app-insights-cloudservices/25-wad.png)

## <a name="more-telemetry"></a>Mer telemetri
Hej avsnitt nedan visar hur tooget ytterligare telemetri från olika aspekter av ditt program.

## <a name="track-requests-from-worker-roles"></a>Spåra begäranden från arbetsroller
I webbroller hello begäranden modulen samlar automatiskt in om HTTP-förfrågningar. Se hello [exempel MVCWebRole](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole) exempel på hur du kan åsidosätta hello standardbeteendet för samlingen. 

Du kan avbilda hello prestanda för anrop tooworker roller genom att följa upp dem i hello samma sätt som HTTP-begäranden. I Application Insights mäter hello telemetri Begärandetypen en arbetsenhet namngivna serversidan som kan vara tidsgränsen och kan oberoende lyckas eller misslyckas. När HTTP-begäranden som avbildas automatiskt av hello SDK, kan du infoga en egen kod tootrack begäranden tooworker roller.

Visa hello två exempel worker roller instrumenterade tooreport begäranden: [WorkerRoleA](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleA) och [WorkerRoleB](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleB)

## <a name="exceptions"></a>Undantag
Information om hur du kan samla in ohanterade undantag från olika webbprogramtyper finns i [Monitoring Exceptions in Application Insights](app-insights-asp-net-exceptions.md) (Övervaka undantag i Application Insights).

hello exempel webbroll har MVC5 och Web API 2 styrenheter. hello samlas ohanterade undantag från hello två in med följande hanterare hello:

* [AiHandleErrorAttribute](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiHandleErrorAttribute.cs)-konfigurationen finns [här](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/FilterConfig.cs#L12) för MVC5-styrenheter
* [AiWebApiExceptionLogger](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiWebApiExceptionLogger.cs)-konfigurationen finns [här](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/WebApiConfig.cs#L25) för Web API 2-styrenheter

För arbetsroller finns det två sätt tootrack undantag:

* TrackException(ex)
* Om du har lagt till hello Application Insights trace listener NuGet-paketet, kan du använda **System.Diagnostics.Trace** toolog undantag. [Kodexempel.](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L107)

## <a name="performance-counters"></a>Prestandaräknare
hello följande räknare samlas in som standard:

    * \Process(??APP_WIN32_PROC??)\% Processortid
    * \Memory\Tillgängliga byte
    * \.NET CLR-undantag(??APP_CLR_PROC??)\# undantag som utlöses/sekund
    * \Process(??APP_WIN32_PROC??)\Privata byte
    * \Process(??APP_WIN32_PROC??)\Byte i I/O-data per sekund
    * \Processor(_Total)\% processortid

För webbroller samlas även dessa räknare in:

    * \ASP.NET-program(??APP_W3SVC_PROC??)\Begäranden/sek
    * \ASP.NET-program(??APP_W3SVC_PROC??)\Körningstid för begäran
    * \ASP.NET-program(??APP_W3SVC_PROC??)\Begäranden i tillämpningskö

Du kan ange fler anpassade prestandaräknare eller andra Windows-prestandaräknare genom att redigera ApplicationInsights.config [som i det här exemplet](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/ApplicationInsights.config#L14).

  ![Prestandaräknare](./media/app-insights-cloudservices/OLfMo2f.png)

## <a name="correlated-telemetry-for-worker-roles"></a>Korrelerad telemetri för arbetsroller
Det är en diagnostiska innehållsrik när du kan se vilka lampa tooa misslyckades eller hög fördröjning begäran. Med web-roller hello SDK ställer automatiskt in sambandet mellan relaterade telemetri. För arbetsroller, kan du använda en anpassad telemetri initieraren tooset en gemensam Operation.Id kontextattribut för alla hello telemetri tooachieve den. Detta kan du toosee om hello latens/felet orsakades på grund av tooa beroende eller din kod i korthet! 

Så här gör du:

* Ange hello korrelation Id till en CallContext enligt [här](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L36). I detta fall använder vi hello ID för begäran som hello Korrelations-id
* Lägg till en anpassad TelemetryInitializer implementering tooset hello Operation.Id toohello correlationId som ovan. Ett exempel finns här: [ItemCorrelationTelemetryInitializer](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/Telemetry/ItemCorrelationTelemetryInitializer.cs#L13)
* Lägg till hello telemetri om anpassade initieraren. Du kan göra det i hello ApplicationInsights.config-filen eller i koden som visas [här](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L233)

Klart! hello-portaler har redan kabelanslutet in toohelp du se alla associerade telemetri i korthet:

![Korrelerad telemetri](./media/app-insights-cloudservices/bHxuUhd.png)

## <a name="client-telemetry"></a>Klienttelemetri
[Lägg till hello JavaScript SDK tooyour webbsidor] [ client] tooget webbläsarbaserad telemetri som sidan Visa antal, sidinläsningstider, skript undantag och toolet som du kan skriva anpassad telemetri i page-skript.

## <a name="availability-tests"></a>Tillgänglighetstester
[Ställ in webbtester] [ availability] toomake säker tillämpningsprogrammet är live och svarstid.

## <a name="display-everything-together"></a>Visa allt tillsammans
tooget en övergripande bild av systemet som du kan ta hello key som övervakning diagram tillsammans på en [instrumentpanelen](app-insights-dashboards.md). Du kan fästa hello begäran t.ex fel räknar för varje roll. 

Om systemet använder andra Azure-tjänster, till exempel Stream Analytics, kan du även lägga till övervakningsdiagrammen för dem. 

Om du har en mobil klientapp infoga vissa kod toosend anpassade händelser om viktiga åtgärder och skapa en [HockeyApp bridge](app-insights-hockeyapp-bridge-app.md). Skapa frågor i [Analytics](app-insights-analytics.md) toodisplay hello händelse antal och fästa dem toohello instrumentpanelen.

## <a name="example"></a>Exempel
[hello exempel](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) övervakar en tjänst som har en webbroll och två arbetsroller.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Undantaget ”metoden hittades inte” vid körning i Azure Cloud Services
Utvecklade du för .NET 4.6? 4.6 stöds inte automatiskt i Azure Cloud Services-roller. [Installera 4.6 för varje roll](../cloud-services/cloud-services-dotnet-install-dotnet.md) innan du kör din app.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Nästa steg
* [Konfigurera skickar Azure-diagnostik tooApplication insikter](app-insights-azure-diagnostics.md)
* [Automatisera genereringen av Application Insights-resurser](app-insights-powershell.md)
* [Automatisera Azure-diagnostik](app-insights-powershell-azure-diagnostics.md)
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample)

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[azure]: app-insights-azure.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[netlogs]: app-insights-asp-net-trace-logs.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md 
