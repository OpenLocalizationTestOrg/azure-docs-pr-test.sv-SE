---
title: "aaaWeb övervakning av programprestanda - Azure Application Insights | Microsoft Docs"
description: Hur Application Insights passar in i hello devOps cykel
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 479522a9-ff5c-471e-a405-b8fa221aedb3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: bba2d6c59de1794adcbf8e298d0ef4f0dbaa700f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deep-diagnostics-for-web-apps-and-services-with-application-insights"></a>Djup diagnostik för webbappar och tjänster med Application Insights
## <a name="why-do-i-need-application-insights"></a>Varför behöver Application Insights?
Application Insights övervakar webbappen körs. Det finns information om fel och problem med prestanda och hjälper dig att analysera hur kunder använder din app. Den fungerar för appar som körs på flera olika plattformar (ASP.NET, J2EE, Node.js,...) och finns i hello molnet eller lokalt. 

![Aspekter av hello komplexiteten med att leverera webbprogram](./media/app-insights-devops/010.png)

Det är viktigt toomonitor moderna program när den körs. Viktigast av allt du toodetect fel innan de flesta av dina kunder göra. Du vill toodiscover och åtgärda problem med prestanda som, när det är inte allvarligt kanske saker går långsammare eller göra vissa besvär tooyour användare. Och när hello system som utför tooyour tillfredsställande sätt kan du vill tooknow vad hello användare gör med den: kan de använda hello senaste funktionen? De lyckas med den?

Moderna program utvecklas i en cykel med kontinuerlig leverans: släpp en ny funktion eller förbättring; Se hur det fungerar hello användare. Planera hello nästa ökning av utveckling baserat på denna kunskap. En viktig del av den här cykeln är hello observationsintervallet fasen. Application Insights innehåller hello verktyg toomonitor ett webbprogram för prestanda och användning.

hello viktigaste aspekt av den här processen är diagnostik- och diagnos. Om programmet hello misslyckas är business förlorad. hello primära roll av ett ramverk för övervakning är tillförlitligt därför toodetect fel, meddelar du omedelbart och toopresent du med hello information behövs toodiagnose hello problem. Detta är exakt det Application Insights.

### <a name="where-do-bugs-come-from"></a>Varifrån kommer buggar?
Fel i web system uppkommer vanligtvis konfigurationsproblem eller felaktiga samverkan mellan många komponenter. hello första uppgiften när hantera en liveplatsen incident är därför tooidentify hello lokus hello problemet: vilken komponent eller relationen är hello orsak?

En del av oss dem med grå hår kan komma ihåg en enklare era som körde ett program i en dator. hello utvecklare skulle testa den noggrant innan du levererar. och har levererats, skulle sällan se eller göra det igen. hello användare skulle ha tooput med hello kvarvarande buggar i många år. 

Vad är nu så mycket annat. Appen har en mängd olika toorun för olika enheter på och det kan vara svårt tooguarantee hello exakt samma beteende på var och en. Värd för appar i molnet hello innebär buggar kan åtgärdas snabbt, men det innebär också kontinuerlig konkurrens och hello förväntningen på nya funktioner med återkommande intervall. 

I dessa villkor automatiserad hello endast sätt tookeep en fast kontroll på hello bugg antal enhet testning. Är det omöjligt toomanually testa allt på varje leverans. Enhet testet är nu en tar över som standard en del av hello Byggprocessen. Verktyg, till exempel hello Xamarin Test molnet hjälpa genom att tillhandahålla automatisk UI tester på flera versioner av webbläsaren. Dessa tester systemen kan vi toohope som hello frekvensen för fel som hittas i en app kan hållas tooa minsta.

Vanliga webbprogram har många live komponenter. I tillägg toohello klient (i en webbläsare och enheter appen) och hello webbserver har troligen toobe betydande backend-bearbetning. Hello backend är kanske en pipeline-komponenter eller en glesare samling samverkande delar. Och många av dem kommer inte att påverka - de är externa tjänster som du är beroende av.

I konfigurationer som dessa det vara svårt och uneconomical tootest för, eller vill, var möjligt felläget, utom hello bor. själva systemet. 

### <a name="questions-"></a>Frågor...
Några frågor som ber vi när vi utvecklar web systemet:

* Min app kraschar? 
* Vad hände exakt? – Om en begäran misslyckades, vill jag tooknow hur den har fått det. Vi behöver en spårning av händelser...
* Är min app tillräckligt snabbt? Hur lång tid tar det toorespond tootypical begäranden?
* Kan hello server referensen hello ladda? När hello hastigheten för förfrågningar stiger hello svarstid håller konstant?
* Hur responsiv är min beroenden - hello REST API: er, databaser och andra komponenter som anropar min app. I synnerhet om hello system är långsam, är den här komponenten eller får jag långsamt svar från någon annan?
* Är min app uppåt eller nedåt? Kan det ses från runt hello world? Meddela mig om avbryts...
* Vad är hello grundorsaken? Var hello fel i min-komponent eller ett beroende? Är det ett kommunikationsproblem?
* Hur många användare påverkas? Om jag har fler än en problemet tootackle, vilket är den viktigaste hello?

## <a name="what-is-application-insights"></a>Vad är Application Insights?
![Grundläggande arbetsflöde för Application Insights](./media/app-insights-devops/020.png)

1. Application Insights instruments din app och skickar telemetri om den medan hello appen körs. Du kan skapa hello Application Insights SDK i hello app, eller du kan använda instrumentation vid körning. hello tidigare metoden är mer flexibel som du kan lägga till egna telemetri toohello vanliga moduler.
2. hello telemetri skickas toohello Application Insights-portalen, där den lagras och bearbetas. (Även om Application Insights finns i Microsoft Azure, kan övervaka alla webbappar – inte bara Azure apps.)
3. hello telemetri visas tooyou i hello form av diagram och tabeller av händelser.

Det finns två typer av telemetri: aggregerade och raw-instanser. 

* Instansdata innehåller till exempel en rapport med en begäran som har tagits emot av ditt webbprogram. Du kan hitta för och inspektera hello information om en begäran med hello sökverktyg i hello Application Insights-portalen. hello instans skulle innehålla data, till exempel hur länge din app tog toorespond toohello begäran, samt hello begärd URL, ungefärliga plats av hello-klienten och andra data.
* Sammanställda data innehåller antalet händelser per tidsenhet, så att du kan jämföra hello andelen begäranden med hello svarstider. Den omfattar också medelvärden av mätvärden, till exempel svarstid för begäran.

hello huvudkategorier av data är:

* Begäranden tooyour app (vanligtvis HTTP-begäranden) med data på URL: en, svarstid, och har lyckats eller misslyckats.
* Beroenden - REST- och SQL-anrop gjorda av din app, även med URI, svarstider och lyckades
* Undantag, inklusive stackspår.
* Sidan Visa data som kommer från hello användarens webbläsare.
* Mått till exempel prestandaräknare, samt mått som du skriver själv. 
* Anpassade händelser som du kan använda tootrack affärshändelser
* Loggspårningar som används för felsökning.

## <a name="case-study-real-madrid-fc"></a>Fallstudie: Verkliga Madrid F.C.
Hej webbtjänsten av [verkliga Madrid Football en](http://www.realmadrid.com/) fungerar ungefär 450 miljoner fläktar kring hälsningsmeddelande. Fläktar åtkomst till den både genom webbläsare och hello föreningens mobila appar. Fläktar kan inte bara bok biljetter, men också komma åt information och videon klipp på resultat, spelare och kommande spel. De kan söka med filter som antal mål bedömas. Det finns även länkar toosocial media. hello användarupplevelsen är mycket personlig och är utformad som en dubbelriktad kommunikation tooengage fläktar.

Hej lösning [är ett system med tjänster och program i Microsoft Azure](https://www.microsoft.com/en-us/enterprise/microsoftcloud/realmadrid.aspx). Skalbarhet är ett krav: trafiken variabeln och kan nå mycket höga volymer under och runt matchar.

För riktiga Madrid är viktiga toomonitor hello systemets prestanda. Azure Application Insights ger en omfattande vy över hello system, säkerställer en tillförlitlig och hög servicenivå. 

hello en också hämtar djup förståelse av dess fläktar: där de är (endast 3% är i Spanien), vilka intresse som de har i spelare, historiska resultat och kommande spel och hur de svara toomatch resultat.

De flesta av dessa telemetridata samlas in automatiskt med ingen lagts till kod som förenklad hello lösning och minskar operativa komplikationer.  För riktiga Madrid, Programinsikter behandlar 3.8 miljarder telemetri punkter varje månad.

Verklig Madrid använder hello Power BI-modulen tooview sina telemetri.

![Power BI vy av Application Insights telemetri](./media/app-insights-devops/080.png)

## <a name="smart-detection"></a>Smart identifiering
[Proaktiv diagnostik](app-insights-proactive-diagnostics.md) är en ny funktion. Utan någon specialkonfiguration av du Application Insights automatiskt identifierar och varnar dig om ovanliga ökar i fel priser i din app. Det är tillräckligt smart tooignore bakgrund tillfälliga fel och också ökar som är helt enkelt proportion till omständigheterna tooa ökning av begäranden. Exempelvis fungerar om det finns ett fel i någon av hello-tjänster som du är beroende av, eller om hello nya bygger du precis distribuerade inte så bra sedan vet du om den när du tittar på din e-post. (Och det finns webhooks så att du kan utlösa andra appar.)

En annan aspekt av den här funktionen utför en daglig djupgående analys av dina telemetri som söker efter ovanliga mönster av prestanda som hårda toodiscover. Det kan till exempel hitta långsamma prestanda som är associerad med ett visst geografiskt område eller med en viss webbläsarversion.

I båda fallen hello aviseringen inte endast anger du hello symptom det upptäcks, men även hello ger du data som du behöver toohelp diagnostisera problem, till exempel relevant undantag rapporter.

![E-post från proaktiv diagnostik](./media/app-insights-devops/030.png)

Kunden Samtec säger: ”under de senaste funktionen cutover påträffades under skalas databasen som träffa resurs gränsen och orsakar timeout. Proaktiv identifiering aviseringar har tagits emot bokstavligt som vi triaging hello problemet mycket nära realtid som annonseras. Den här aviseringen tillsammans med hello Azure-plattformen aviseringar hjälp oss nästan omedelbart åtgärda hello problem. Total avbrottstid < 10 minuter ”.

## <a name="live-metrics-stream"></a>Direktsänd dataström mått
Distribuera hello senaste versionen kan vara en angelägna upplevelse. Om det finns problem vill du tooknow om dem direkt, så att du kan säkerhetskopiera om det behövs. Direktsänd dataström med mått ger nyckelvärden med en fördröjning på ca en sekund.

![Live mått](./media/app-insights-devops/040.png)

Och du kan inspektera omedelbart ett exempel på några fel och undantag.

![Live felhändelser](./media/app-insights-devops/live-stream-failures.png)

## <a name="application-map"></a>Programkarta
Programavbildningen upptäcker automatiskt topologi för tjänstprogram om hello prestandainformation ovanpå det toolet du enkelt identifiera prestandaflaskhalsar och problematiska flöden i din miljö. Det gör du toodiscover programberoenden på Azure-tjänster. Du kan prioritering hello problemet genom att förstå om det är koden relaterade eller beroende relaterade och från en enda plats detaljerat relaterade diagnostik upplevelse. Programmet kan till exempel att misslyckas på grund av tooperformance försämring i SQL-nivå. Med programavbildningen, kan du se direkt och hello SQL Index Advisor eller fråga insikter erfarenhet mer detaljerat.

![Programkarta](./media/app-insights-devops/050.png)

## <a name="application-insights-analytics"></a>Application Insights Analytics
Med [Analytics](app-insights-analytics.md), du kan skriva godtycklig frågor i ett kraftfullt SQL-liknande språk.  Diagnostisera över hello hela appen stack blir enkelt som olika perspektiv Anslut och du kan be hello rätt frågor toocorrelate Service prestanda med affärsmått och Customer Experience. 

Du kan fråga telemetri instans och mått rådata som lagras i hello-portalen. hello språk innehåller filter, koppling, sammanställning och andra åtgärder. Du kan beräkna fält och utföra statistiska analyser. Det finns både tabular och grafiska visualiseringar.

![Analytics-fråga och resultat diagram](./media/app-insights-devops/025.png)

Exempelvis är det enkelt att:

* Segmentera ditt programs begäran om prestandadata av kunden nivåer toounderstand sina erfarenheter.
* Söka efter specifika felkoder eller anpassade händelsenamn under liveplatsen undersökningar.
* Öka detaljnivån hello appanvändningen av vissa kunder toounderstand hur funktioner förvärvas och antas.
* Spåra sessioner och svarstider för specifika användare tooenable support och åtgärder team tooprovide instant teknisk support.
* Fastställa vanliga app funktioner tooanswer funktionen prioritering frågor.

Kunden DNN säger: ”Application Insights har gett oss hello saknar en del av hello Ekvationen för att kunna toocombine, sortera, fråga och filtrera data efter behov. Vårt team toouse sina egna ingenuity och erfarenhet toofind data med ett kraftfullt frågespråk kan vi toofind insikter och lösa problem som vi visste vi hade. Mycket intressant svar kommer från hello frågor från och med *”jag konstigt om '.*”

## <a name="development-tools-integration"></a>Development tools integrering
### <a name="configuring-application-insights"></a>Konfigurera Application Insights
Visual Studio och Eclipse har verktyg tooconfigure hello rätt SDK paket för hello-projekt som du utvecklar. Det finns en menyn kommandot tooadd Application Insights.

Om du råkar toobe med hjälp av ett ramverk för loggning av spårning som Log4N, NLog eller System.Diagnostics.Trace får hello alternativet toosend hello loggar tooApplication insikter tillsammans med hello andra telemetri så att du enkelt kan jämföra hello spårningar med begäranden , beroendeanrop och undantag.

### <a name="search-telemetry-in-visual-studio"></a>Sök telemetri i Visual Studio
När du utvecklar och felsökning av en funktion kan du visa och söka hello telemetri direkt i Visual Studio med hello samma söka verksamhet som i hello webbportalen.

Och när Application Insights loggar ett undantag, kan du visa hello datapunkt i Visual Studio och hoppa raka toohello relevant kod.

![Visual Studio-sökning](./media/app-insights-devops/060.png)

Under felsökning har hello alternativet tookeep hello telemetri i utvecklingsdatorn, visar den i Visual Studio men utan att skicka det toohello portal. Det här alternativet för lokal undviker blanda felsökning med telemetri för produktion.

### <a name="build-annotations"></a>Skapa anteckningar
Om du använder Visual Studio Team Services toobuild och distribuera din app visas distribution anteckningar i diagram i hello-portalen. Om den senaste versionen har någon effekt på hello mått, blir det uppenbara.

![Skapa anteckningar](./media/app-insights-devops/070.png)

### <a name="work-items"></a>Arbetsobjekt
När en varning utlöses kan Application Insights automatiskt skapa ett arbetsobjekt i ditt arbete system för uppföljning.

## <a name="but-what-about"></a>Men vad händer om...?
* [Sekretess- och](app-insights-data-retention-privacy.md) -din telemetri sparas på Azure säkra servrar.
* Prestanda - hello påverkan är mycket låg. Telemetri batchar.
* [Priser](app-insights-pricing.md) – du kan komma igång gratis och som fortsätter när du använder låg volym.


## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Nästa steg
Det är lätt att komma igång med Application Insights. hello huvudsakliga alternativ är:

* Instrumentera en webbapp körs redan. Detta ger dig all hello inbyggda prestanda telemetri. Den är tillgänglig för [Java](app-insights-java-live.md) och [IIS-servrar](app-insights-monitor-performance-live-website-now.md), och också för [Azure-webbappar](app-insights-azure.md).
* Instrumentera ditt projekt under utveckling. Du kan göra detta för [ASP.NET](app-insights-asp-net.md) eller [Java](app-insights-java-get-started.md) appar, samt [Node.js](app-insights-nodejs.md) och [andra typer](app-insights-platforms.md). 
* Instrument [webbsidor](app-insights-javascript.md) genom att lägga till ett kort kodfragment.

