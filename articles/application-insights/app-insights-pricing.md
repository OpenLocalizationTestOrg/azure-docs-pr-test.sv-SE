---
title: "aaaManage priser och data volymen för Azure Application Insights | Microsoft Docs"
description: "Hantera telemetri volymer och övervaka kostnader i Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: bwren
ms.openlocfilehash: 4621c989cd467735aefc48ec9547fcbe1b1ae41b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Hantera priser och data volym i Application Insights


Priser för [Azure Application Insights] [ start] baseras på datavolym per program. Låg användning under utvecklingen eller för en liten app är sannolikt toobe ledigt, eftersom det inte finns en 1 GB månatlig ersättning av telemetridata.

Varje Application Insights-resurs debiteras som en separat tjänst och bidrar toohello faktura för din prenumeration tooAzure.

Det finns två prisnivå planer. hello standardplanen kallas Basic. Du kan välja hello Enterprise plan som har en daglig kostnad, men gör att ytterligare funktioner som [löpande export](app-insights-export-telemetry.md).

Om du har frågor om hur priser fungerar för Application Insights känna sig fria toopost en fråga i vår [forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights). 

## <a name="hello-price-plans"></a>hello pris planer

Se hello [Application Insights sida med priser] [ pricing] för aktuella priser i din valuta.

### <a name="basic-plan"></a>Grundläggande planen

hello grundläggande planen är hello standard när en ny Application Insights-resurs skapas och räcker för de flesta kunder.

* I hello grundläggande planen, debiteras du av datavolym: antal byte av telemetri som tagits emot av Application Insights. Datavolymen mäts som hello storleken på hello okomprimerade JSON datapaketet som tagits emot av Application Insights från ditt program.
För [tabelldata importeras till Analytics](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-import), hello datavolym mäts som hello okomprimerade storleken för filer som skickats tooApplication insikter.  
* Din första 1 GB för varje app är ledigt, så om du bara experimentera eller utveckla, du är inte troligt toohave toopay.
* [Live mått dataströmmen](app-insights-live-stream.md) data inte räknas för prissättning.
* [Löpande Export](app-insights-export-telemetry.md) är tillgänglig för en extra per GB kostnad i hello grundläggande planen.

### <a name="enterprise-plan"></a>Företagsplan

* I hello företagsplan kan din app använda alla hello Application Insights. [Löpande Export](app-insights-export-telemetry.md) och 

[Logga Analytics connector](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) är tillgängliga utan någon extra kostnad i hello företagsplan.
* Du betalar per nod som skickar telemetri för alla program i hello företagsplan. 
 * En *nod* är en fysisk eller virtuell server-dator eller en plattform som en tjänst-rollinstans som är värd för din app.
 * Datorer för utveckling, klientwebbläsare och mobila enheter räknas inte som noder.
 * Om din app har flera komponenter som skickar telemetri, till exempel en webbtjänst och en serverdel worker räknas de separat.
 * [Live mått dataströmmen](app-insights-live-stream.md) data inte räknas för priser purposes.* över en prenumeration är dina avgifter per nod inte per app. Om du har fem noder skicka telemetri för 12 appar och sedan hello debiterar är de fem noder.
* Även om avgifter citattecken per månad, använder du debiteras endast för varje timme som en nod skickar telemetri från en app. Hej timvisa kostnad är hello citattecken månadskostnaden / 744 (hello antal timmar i 31 dagar i månaden).
* En data-volym tilldelning av 200 MB per dag anges för varje nod som identifierades (med timme). Allokering av oanvända data överförs inte från en dag toohello nästa.
 * Om du väljer hello Enterprise prisnivå alternativet får varje prenumeration en daglig ersättning av data baserat på hello antalet noder skicka telemetri toohello Application Insights-resurser i den prenumerationen. Så om du har 5 noder och skicka data hela dagen, har du en grupperad justering med 1 GB tillämpas tooall hello Application Insights-resurser i den prenumerationen. Det spelar ingen roll om vissa noder skickar mer data än andra noder eftersom hello inkluderat data delas mellan alla noder. Om en viss dag hello Application Insights-resurser visas mer data än vad som ingår i hello tilldelning av dagliga data för den här prenumerationen gäller hello per GB överförbrukning data avgifter. 
 * hello dagliga data ersättning beräknas som hello antalet timmar hello dag (med UTC) att varje nod skickar telemetri divideras med 24 gånger 200 MB. Så om du har 4 noder skicka telemetri under 15 i hello 24 timmar hello dag hello ingår data för den dagen skulle vara ((4 x 15) / 24) x 200 MB = 500 MB. Till hello-pris 2.30 USD per GB för data överförbrukning är hello kostnad för 1,15 USD om hello noder skickar 1 GB data den dagen.
 * Observera att hello Enterprise plan dagliga ersättningen inte delas med program som du har valt grundläggande hello-alternativet och oanvända ersättning överförs inte från dagliga. 
* Här följer några exempel för att avgöra antalet distinkta noder:
| Scenario                               | Totalt antal per dag nod |
|:---------------------------------------|:----------------:|
| 1 program använder 3 Azure App Service-instanser och 1 virtuell server | 4 |
| 3 program som körs på 2 virtuella datorer och hello Application Insights-resurser för dessa program är i hello samma prenumeration och i hello företaget planerar | 2 | 
| 4 program vars program Insights-resurser finns i hello samma prenumeration. Alla program körs 2 instanser 16 utanför arbetstid och 4 instanser under 8 användningsnivå. | 13.33 | 
| Molntjänster med 1 Worker-rollen och 1 webbrollen, var 2 instanser körs | 4 | 
| 5-nod Service Fabric-kluster som kör 50 micro-tjänster, varje micro-tjänsten körs 3 instanser | 5|

* hello beror exakt nod cykliska som Application Insights SDK programmet används. 
  * I SDK-versioner 2.2 och och senare, både hello Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) eller [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) ska rapportera varje värd för programmet som en nod, till exempel hello datornamn för fysiska servrar och värdar för Virtuella datorer eller hello instansnamnet hello gäller molntjänster.  Hej undantag är program endast med hjälp av [.NET Core](https://dotnet.github.io/) och hello Application Insights Core SDK, där case endast en nod ska rapporteras för alla värdar, eftersom hello värdnamn inte är tillgänglig. 
  * Tidigare versioner av hello SDK hello [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) fungerar precis som hello nyare versioner av SDK, men hello [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) rapporterar endast en nod oavsett hello antalet faktiska programvärdar. 
  * Observera att om programmet använder hello SDK tooset roleInstance tooa anpassat värde, som standard samma värde används toodetermine hello antal noder. 
  * Om du använder en ny version av SDK med en app som körs från klientdatorer eller mobila enheter, är det möjligt att hello antal noder kan returnera ett tal som är mycket stora (från hello stort antal klientdatorer eller mobilenheter). 

### <a name="multi-step-web-tests"></a>Webbtester med flera steg

Det finns en extra kostnad för [webbtester med flera steg](app-insights-monitor-web-app-availability.md#multi-step-web-tests). Detta refererar tooweb tester som utför en sekvens med åtgärder. 

Det kostar ingenting separat för Pingtest av en enstaka sida. Telemetri från både Pingtest och flera steg tester debiteras tillsammans med andra telemetri från din app.
 
## <a name="operations-management-suite-subscription-entitlement"></a>Operations Management Suite-prenumeration rätt

Som [presenterade nyligen](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/), kunder som köpa Microsoft Operations Management Suite E1 och E2 är kan tooget Application Insights Enterprise som en ytterligare komponent utan extra kostnad. Varje enhet Operations Management Suite E1 och E2 innehåller främst en rätt too1 nod i hello företagsplan av Application Insights. Som nämnts ovan innehåller varje nod för Application Insights in too200 MB data som inhämtas per dag (separat från logganalys datapåfyllning) med 90 dagars data bevaras utan extra kostnad. 

> [!NOTE]
> tooensure att du får denna rättighet, måste du ha Application Insights-resurser i hello företag priser plan. Denna rättighet gäller endast som noder, så inga fördelar inte uppnår med Application Insights-resurser i hello grundläggande planen. Observera att denna rättighet visas inte på hello beräknade kostnader som visas på hello funktioner + prisnivå bladet. 
>
 
## <a name="review-pricing-plans-and-estimate-costs"></a>Granska prisnivå planer och uppskatta kostnader

Applicaition Insights gör det enkelt toounderstand hello prissättning som är tillgängliga och vilka hello är sannolikt att kostnaderna baseras på de senaste användningsmönster. Starta genom att öppna hello **funktioner + priser** bladet i hello Application Insights-resurs i hello Azure-portalen:

![Välj prisnivå.](./media/app-insights-pricing/01-pricing.png)

**a.** Granska dina datavolym för hello månad. Detta omfattar alla hello data tas emot och behålls (efter något [provtagning](app-insights-sampling.md) från din server och -klientprogram och tillgänglighetstester.

**b.** En separat kostnad görs efter [webbtester med flera steg](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (Detta innehåller enkla tillgänglighetstester som ingår i hello data volym kostnad.)

**c.** Aktivera hello företagsplan.

**d.** Klicka dig igenom datavolymen för toodata management alternativ tooview för hello förra månaden, ange en daglig fjärrskrivbordsanslutning eller ange införandet provtagning.

Application Insights avgifter läggs tooyour Azure faktura. Du kan se information om din Azure debiterar på hello fakturering avsnitt av hello Azure-portalen eller i hello [Azure Billing Portal](https://account.windowsazure.com/Subscriptions). 

![Välj fakturering på hello sida-menyn.](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Datahastighet
Det finns tre sätt i vilka hello volymen som du skickar data är begränsad:

* **Provtagning:** kan användas för den här mekanismen minska hello telemetri som skickas från din server och klient appar, med minimal förvrängning av mått. Detta är hello primära verktyget tootune hello mängder data. Lär dig mer om [provtagning funktioner](app-insights-sampling.md). 
* **Dagliga cap:** om att skapa en Application Insights-resurs från hello Azure-portalen detta anges too500 GB/dag. Hej standard när du skapar en resurs för Application Insights från Visual Studio, är liten (endast 32,3 MB/dag) som är avsedd endast toofaciliate testning. I det här fallet är avsedda hello användaren höjer hello dagliga cap innan du distribuerar hello app till produktionen. hello högsta kapacitet är 500 GB/dag om du har begärt en högre maximal för ett program med hög trafik. Försiktig när du ställer in hello dagliga taket, som du vill ska vara **aldrig toohit hello dagliga cap**, eftersom du kommer att förlora data för hello resten av hello dag och att toomonitor ditt program. toochange länkas den Använd hello daglig volym cap bladet från hello volym datahantering bladet (se nedan). Observera att vissa prenumerationstyper av kredit som inte kan användas för Application Insights. Om hello prenumerationen har en utgifter begränsa, hello dagligen cap bladet kommer att ha anvisningar hur tooremove den och aktivera hello dagliga cap toobe aktiveras utöver 32,3 MB per dag.  
* **Begränsning:** denna gränser hello data hastighet too32 k händelser per sekund, var i genomsnitt över 1 minut. 


*Vad händer om min app överskrider hello begränsning hastighet?*

* hello mängden data som appen skickar utvärderas varje minut. Om den överskrider hello per sekund hastighet ett genomsnitt över hello minut hello-servern kan inte vissa begäranden. hello SDK buffrar hello data och sedan försöker tooresend, sprida en ökning i över flera minuter. Om din app konsekvent skickar data på ovan hello begränsning hastighet, vissa data att tas bort. (hello ASP.NET, Java och JavaScript SDK försök tooresend på detta sätt och andra SDK: er kan bara ta bort begränsas data.) Om begränsning inträffar, visas ett meddelande som varning detta har skett.

*Hur vet hur mycket data som skickar min app?*

* Öppna hello **volym datahantering** bladet toosee hello-volymer med dagliga data. 
* I Metrics Explorer lägga till ett nytt schema eller välj **datapunktsvolym** som dess mått. Växla på gruppering och gruppera efter **datatyp**.

## <a name="tooreduce-your-data-rate"></a>tooreduce vilken takt dina data
Här följer några saker du kan göra tooreduce datavolymen:

* Använd [provtagning](app-insights-sampling.md). Den här tekniken minskar datahastighet utan skeva din mått och utan att störa hello möjlighet toonavigate mellan relaterade objekt i sökningen. I server appar fungerar den automatiskt.
* [Begränsa hello antalet Ajax-anrop som kan rapporteras](app-insights-javascript.md#detailed-configuration) i varje vyn sida, eller Byt ut Ajax-rapportering.
* Stäng av samlingen moduler som du inte behöver genom [redigera ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Du kan till exempel bestämma att prestandaräknare eller beroendedata är inessential.
* Dela dina telemetri tooseparate instrumentation nycklar. 
* Före aggregera mått. Om du har placerat anrop tooTrackMetric i din app, kan du minska trafik med hjälp av hello-överlagring som accepterar beräkningen av hello medelvärde och standardavvikelsen för en grupp med mått. Du kan också använda en [före sammanställa paketet](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="managing-hello-maximum-daily-data-volume"></a>Hantera hello maximala dagliga datavolym

Du kan använda hello daglig volym cap toolimit hello data som samlas in, men om hello fästpunkten är uppfyllt, det leder till förlust av alla telemetery som skickas från ditt program för hello resten av hello dag. Det är **inte tillrådligt** toohave ditt program toohit hello dagliga cap efter att du har tootrack hello hälsotillstånd och prestanda för ditt program när den med samma namn. 

Använd i stället [provtagning](app-insights-sampling.md) tootune hello data toohello volymnivå du skulle och hello dagliga cap endast som en ”sista utväg” om ditt program börjar skicka mycket större mängder telemetery oväntat. 

toochange hello dagliga fästpunkten, i hello konfigurationsavsnittet av ditt program Insihgts resurs, klickar du på **volym datahantering** sedan **dagliga Cap**.

![Justera hello dagliga telemetri volym linjeslut](./media/app-insights-pricing/daily-cap.png) 

## <a name="sampling"></a>Samling
[Provtagning](app-insights-sampling.md) är en metod för att minska hello hastighet som telemetri skickas tooyour app när bevaras hello möjlighet toofind relaterade händelser under diagnostiska sökningar och räknar fortfarande behåller rätt händelse. 

Sampling är ett effektivt sätt tooreduce avgifter och hålla sig inom den månatliga kvoten. hello provtagning algoritmen behåller relaterade objekt för telemetri, så att till exempel när du använder Sök hittar du hello begäran relaterade tooa särskilda undantag. hello algoritmen behåller även rätt antal så att du ser hello rätt värden i måttet Explorer för begäran frekvens, undantag priser och andra antal.

Det finns flera typer av provtagning.

* [Anpassningsbar provtagning](app-insights-sampling.md) är hello standardvärde för hello SDK för ASP.NET, som justerar toohello telemetrivolym som appen skickar automatiskt. Det körs automatiskt i hello SDK i ditt webbprogram, så att hello telemetri trafik i hello nätverk minskar. 
* *Införandet provtagning* är ett alternativ som fungerar på hello punkt där telemetri från din app kommer in hello Application Insights-tjänsten. Det påverkar inte hello telemetrivolym skickas från din app, men det minskar hello volymen som behålls av hello-tjänsten. Du kan använda den tooreduce hello kvot förbrukats av telemetri från webbläsare och andra SDK: er.

tooset införandet provtagning, ange hello kontrollen i hello priser blad:

![Från hello kvoten och prissättning bladet, klickar hello exempel sida vid sida och väljer en provtagning bråkdel.](./media/app-insights-pricing/04.png)

> [!WARNING]
> hello Data provtagning bladet styr endast hello värde för provtagning införandet. Hello samplingsfrekvensen som används av hello Application Insights SDK i din app visas inte. Om hello inkommande telemetri redan samlats in vid hello SDK, tillämpas inte införandet provtagning.
> 

toodiscover hello faktiska samplingsfrekvens oavsett om den har tillämpats, använda en [Analytics query](app-insights-analytics.md) sådana här:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

Bevaras i varje post, `itemCount` anger hello antal ursprungliga poster som den representerar lika too1 + hello antal tidigare borttagna poster. 


## <a name="automation"></a>Automation

Du kan skriva ett skript tooset hello prisplan, med hjälp av Azure Resource Manager. [Lär dig hur](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Sammanfattning av gränser
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Nästa steg

* [Sampling](app-insights-sampling.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/

