---
title: "aaaProfiling live webbappar på Azure med Application Insights | Microsoft Docs"
description: "Identifiera hello varm sökväg i koden web server med en låg storleken profiler."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: bwren
ms.openlocfilehash: 3c7f21076f19335e0f006327932e13623ec9526b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="profiling-live-azure-web-apps-with-application-insights"></a>Profilering live Azure-webbappar med Application Insights

*Den här funktionen av Application Insights är GA för Apptjänster och förhandsgranskning för beräkning.*

Ta reda på hur mycket tid läggs varje metod i webbprogrammet live med hjälp av hello profilering verktyg [Azure Application Insights](app-insights-overview.md). Den visas detaljerad profiler för live-frågor som har hanteras av din app och markeras hello 'varm path' som använder hello de flesta tid. Väljs automatiskt exempel som har olika svarstider. hello-profiler använder olika tekniker toominimize kostnader.

hello profiler för närvarande fungerar för ASP.NET webb-appar som körs på Azure App Service i minst hello Basic prisnivån. 

<a id="installation"></a>
## <a name="enable-hello-profiler"></a>Aktivera hello profiler

[Installera Application Insights](app-insights-asp-net.md) i koden. Om den redan är installerad, kontrollera att du har hello senaste versionen. (toodo, högerklicka på projektet i Solution Explorer och välj Hantera NuGet-paket. Välj uppdateringar och alla uppdateringspaket.) Omdistribuera din app.

*Med hjälp av ASP.NET Core? [Markera den här kryssrutan](#aspnetcore).*

I [https://portal.azure.com](https://portal.azure.com), öppna hello Application Insights-resurs för webbappen. Öppna **prestanda** och på **aktivera Application Insights Profiler...** .

![Klicka på hello aktivera profileraren banderoll][enable-profiler-banner]

Du kan alltid Klicka **konfigurera** tooview status, aktivera eller inaktivera hello Profiler.

![Klicka på Konfigurera i hello prestanda bladet][performance-blade]

Webbprogram som är konfigurerade med Application Insights visas på Konfigurera bladet. Följ anvisningarna tooinstall hello profileraren agenten om det behövs. Om inget webbprogram har konfigurerats med Application Insights ännu, klickar du på *Lägg till länkade appar*.

Använd hello *aktivera profileraren* eller *inaktivera profileraren* knapparna i hello konfigurera bladet toocontrol hello Profiler på alla dina länkade webbprogram.



![Konfigurera bladet][linked app services]

toostop eller omstart hello profiler för en enskild App Service-instans, hittar du den **i hello App tjänstresurs**i **Webjobs**. toodelete utseende under den **tillägg**.

![Inaktivera profiler för en webb-jobb][disable-profiler-webjob]

Vi rekommenderar att du har hello Profiler som är aktiverad på alla dina toodiscover om web apps som alla prestandaproblem så snart som möjligt.

Om du använder WebDeploy toodeploy ändringar tooyour webbprogram, kontrollera att du utesluter hello **App_Data** mappen tas bort under distributionen. Annars hello profileraren tillägget filer tas bort när du sedan distribuerar hello web application tooAzure.

### <a name="using-profiler-with-azure-vms-and-compute-resources-preview"></a>Med hjälp av profiler med Azure Virtual Machines och beräkningsresurser (förhandsgranskning)

När du [aktivera Application Insights för Azure app Service vid körning](app-insights-azure-web-apps.md#run-time-instrumentation-with-application-insights), Profiler är automatiskt tillgänglig. (Om du redan har aktiverat Programinsikter för hello resursen måste kanske tooupdate toohello lates version via hello **konfigurera** guiden.)

Det finns en [förhandsversionen av hello Profiler för Azure Compute resurser](https://go.microsoft.com/fwlink/?linkid=848155).


## <a name="limits"></a>Begränsningar

hello standard datalagring är 5 dagar. Maximal 10 GB inhämtas per dag.

Det är gratis för hello profileraren tjänsten. Ditt webbprogram måste finnas i minst hello grundläggande nivån av App-tjänster.

## <a name="viewing-profiler-data"></a>Visa profiler data

Öppna hello prestanda bladet och rulla ned toohello åtgärden listan.




![Application Insights prestanda bladet exempel kolumn][performance-blade-examples]

hello kolumner i tabellen hello är:

* **Antal** -hello antal dessa begäranden i hello tidsintervall för hello-bladet.
* **Medianvärdet** -hello vanliga överföringstiden för din app toorespond tooa begäran. Hälften av alla svar var snabbare än så.
* **95: e percentilen** 95% av svar var snabbare än så. Om denna bild är bland annat hello median kan finnas det ett tillfälligt problem med din app. (Eller det kan förklaras av en design-funktion, till exempel cachelagring.)
* **Exempel** -en ikon som indikerar att hello profileraren har hämtats stackspår för den här åtgärden.

Klicka på hello exempel ikonen tooopen hello trace explorer. hello explorer visar några exempel som hello profileraren har hämtats klassificeras efter svarstid.

Välj en exempel-tooshow en kod på objektnivå uppdelning av tid som går åt verkställande hello-begäran.

![Application Insights Trace Explorer][trace-explorer]

**Visa varm sökvägen** öppnas hello största lövnod eller stänga minst något. I de flesta fall inte den här noden intilliggande tooa flaskhalsar.



* **Etiketten**: hello namnet på funktionen hello eller händelse. hello trädet innehåller en blandning av koden och händelser som inträffade (till exempel SQL- och HTTP-händelser). hello översta händelsen representerar hello övergripande varaktighet för begäran.
* **Förfluten tid**: hello tidsintervallet mellan hello operation hello startas och hello slut.
* **När**: Visar när hello funktionen/händelse kördes i relationen tooother funktioner.

## <a name="how-tooread-performance-data"></a>Hur tooread prestandadata

Microsoft service profiler använder en kombination av provtagning metoden och instrumentation tooanalyze hello prestanda för ditt program.
När detaljerad samling pågår hello service profiler-exempel instruktion pekare på var och en av hello datorns processor i varje millisekunder.
Varje prov avbildar hello fullständig anropsstacken för hello tråd för närvarande körs, ger detaljerad och användbar information om vad som tråden som utfördes på båda hög och låg gjord. Service profiler samlar även in andra händelser som kontexten växling händelser, TPL händelser och trådpoolen händelser tootrack aktivitet korrelation och orsakssamband.

hello anropsstacken visas i hello tidslinjevyn beror hello hello ovan provtagning och instrumentation. Eftersom varje prov fångar hello fullständig anropsstacken för hello tråd, inkluderar den kod från hello .NET framework, samt andra ramverk som du refererar till.

### <a id="jitnewobj"></a>Objekt-allokering (`clr!JIT\_New or clr!JIT\_Newarr1`)
`clr!JIT\_New and clr!JIT\_Newarr1`är hjälpfunktioner i .NET framework som allokerar minne från hanterad heap. `clr!JIT\_New`anropas när ett objekt har allokerats. `clr!JIT\_Newarr1`anropas när en objektmatris allokeras. Dessa två funktioner är vanligtvis mycket snabbt och bör ta relativt små lång tid. Om du ser `clr!JIT\_New` eller `clr!JIT\_Newarr1` ta en lång tid i tidslinjen, det är en indikation på att hello kod fördela många objekt och förbruka mycket minne.

### <a id="theprestub"></a>Läser in kod (`clr!ThePreStub`)
`clr!ThePreStub`är en hjälpfunktion i .NET framework som förbereder hello kod tooexecute för hello första gången. Detta innefattar vanligtvis sådana, men inte begränsat till, JIT (Just i tid)-kompilering. För varje C#-metoden `clr!ThePreStub` ska anropas en gång under hello livslängden för en process.

Om du ser `clr!ThePreStub` tar lång tid för en begäran, betyder det att begäran är hello förstnämnda som utför den metoden och hello tid för .NET framework runtime tooload att metoden är betydande. Du kan överväga en värmts process som körs som del av hello koden innan användarna åtkomst till den eller Överväg att köra NGen på din sammansättningar.

### <a id="lockcontention"></a>Låskonkurrens (`clr!JITutil\_MonContention` eller `clr!JITutil\_MonEnterWorker`)
`clr!JITutil\_MonContention`eller `clr!JITutil\_MonEnterWorker` ange hello aktuella tråden väntar på ett lås toobe släpps. Detta normalt visas när du kör en C# lock-instruktion, startar Monitor.Enter metod eller anropa en metod med attributet MethodImplOptions.Synchronized. Låskonkurrens inträffar oftast när tråden A hämtar ett lås och tråd B försöker tooacquire hello samma låsa innan tråd A släpper den.

### <a id="ngencold"></a>Läser in kod (`[COLD]`)
Om hello metodnamnet innehåller `[COLD]`, som `mscorlib.ni![COLD]System.Reflection.CustomAttribute.IsDefined`, innebär det att hello .NET framework-körningen kör kod som inte är optimerad av <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">profil interaktiv optimering</a> för hello första gången. För varje metod ska det visas högst en gång under hello livstid hello-processen.

Om inläsning av koden tar lång tid en begäran, betyder det att begäran är hello första en tooexecute hello optimerat delen av hello-metoden. Du kan överväga en varmt in process som körs som del av hello koden innan användarna åtkomst till den.

### <a id="httpclientsend"></a>Skicka HTTP-begäran
Metoder som `HttpClient.Send` ange hello kod väntar på att en HTTP-begäran toocomplete.

### <a id="sqlcommand"></a>Databasåtgärden
Metoden, till exempel SqlCommand.Execute anger hello kod väntar på en databas åtgärden toocomplete.

### <a id="await"></a>Väntar på (`AWAIT\_TIME`)
`AWAIT\_TIME`Anger hello kod väntar på en annan aktivitet toocomplete. Detta inträffar vanligtvis med C#-await-instruktionen. När hello koden gör en C# 'await', hello tråd unwinds returnerar kontrollen toohello trådpoolen och det finns ingen tråd blockeras väntar hello 'await' toofinish. Logiskt hello men tråden som gjorde hello await 'blockeras' hello åtgärden toocomplete väntar. Den `AWAIT\_TIME` anger hello blockeras tidpunkt väntar hello uppgiften toocomplete.

### <a id="block"></a>Blockerade tid
`BLOCKED_TIME`Anger hello kod väntar på en annan resurs toobe som är tillgängliga, till exempel väntar på ett synkroniseringsobjekt, väntar på att en tråd toobe tillgängliga eller väntar på att en begäran toofinish.

### <a id="cpu"></a>CPU-tid
hello CPU är upptagen med att köra hello-instruktioner.

### <a id="disk"></a>Disktid i procent
hello program utför diskåtgärder.

### <a id="network"></a>Network Time
hello program utför nätverksåtgärder.

### <a id="when"></a>När kolumnen
Det här är en visualisering av hur hello INKLUSIVA prover samlas in för en nod variera över tid. hello totala intervallet för hello-begäran är uppdelat i 32 tid buckets och hello inklusiva prover för noden ackumuleras till dessa 32 buckets. Varje bucket sedan representeras som en stapel vars höjd representerar ett skalade värde. För markerade noder `CPU_TIME` eller `BLOCKED_TIME`, eller om det finns en tydlig relationen mellan förbrukar en resurs (cpu, disk, tråd), hello liggande representerar förbrukar en av resurserna för hello tidsperiod som bucket. De här måtten kan du få mer än 100% genom att använda flera resurser. Till exempel om i genomsnitt du använder två processorer via ett intervall som får sedan du 200%.


## <a id="troubleshooting"></a>Felsökning

### <a name="how-can-i-know-whether-application-insights-profiler-is-running"></a>Hur vet jag om Application Insights profiler körs?

hello profileraren körs som en kontinuerlig web-jobb i webbprogrammet. Du kan öppna hello webbprogrammet resurs i https://portal.azure.com och kontrollera statusen för ”ApplicationInsightsProfiler” Hej WebJobs-bladet. Om den inte körs, öppna **loggar** toofind mer.

### <a name="why-cant-i-find-any-stack-examples-even-though-hello-profiler-is-running"></a>Varför hittar inte någon stack-exempel trots att hello profileraren körs?

Här följer några saker du kan kontrollera.

1. Se till att din Web App Service-Plan grundläggande nivån och högre.
2. Kontrollera att ditt webbprogram har Application Insights SDK 2.2 Beta och ovan aktiverad.
3. Kontrollera att ditt webbprogram har hello APPINSIGHTS_INSTRUMENTATIONKEY setting hello samma instrumentation nyckel som används av Application Insights SDK.
4. Kontrollera att ditt webbprogram som körs på .net Framework 4.6.
5. Om det är ett ASP.NET Core-program kan också kontrollera [hello krävs beroenden](#aspnetcore).

Finns en kort värmts period när hello profileraren aktivt samlar in spårningar för flera prestanda när hello profileraren har startats. Samlar in prestanda spårningar i två minuter i varje timme efter det hello profiler.  

### <a name="i-was-using-azure-service-profiler-what-happened-tooit"></a>Jag använde Azure Service Profiler. Vilka happened tooit?  

När du aktiverar Application Insights profileraren har Azure Service Profiler-agenten inaktiverats.

### <a id="double-counting"></a>Dubbel inventering i parallella trådar

I vissa fall hello är total tidsmått i hello stack viewer mer än hello verkliga varaktigheten av hello-begäran.

Detta kan inträffa när det finns två eller flera trådar som är associerade med en begäran körs parallellt. hello totala tråd tid är sedan större än hello förfluten tid. I många fall kan väntar på en tråd på hello andra toocomplete. Hej viewer försöker toodetect detta och utelämna hello ointressanta vänta, men överför längs hello visar för mycket i stället för att utelämna vad som kan vara viktig information.  

När du ser parallella trådar i dina spår måste toodetermine som trådar som väntar på för att se hello kritiska för hello-begäran. I de flesta fall hello hello tråd som snabbt att försätta datorn i tillståndet vänta bara väntar på andra trådar. Koncentrera dig på hello andra och ignorera hello tid i hello väntande trådar.

### <a id="issue-loading-trace-in-viewer"></a>Inga profildata

1. Om hello data som du försöker tooview är äldre än några veckor, begränsa tiden filtret och försök igen.

2. Kontrollera att proxyservrar eller en brandvägg har inte blockerat åtkomsten toohttps://gateway.azureserviceprofiler.net.

3. Kontrollera att hello hello Application Insights instrumentation nyckeln som du använder i din app är samma som hello Application Insights-resurs som du har aktiverat profilering med. hello nyckeln finns vanligtvis i ApplicationInsights.config men finns också i web.config eller app.config.

### <a name="error-report-in-hello-profiling-viewer"></a>Felrapport i hello profilering viewer

Filen ett supportärende från hello-portalen. Ange hello Korrelations-ID från hello felmeddelande.

## <a name="manual-installation"></a>Manuell installation

När du konfigurerar hello profileraren görs hello följande uppdateringar toohello Web App-inställningar. Du kan göra dem själv manuellt om din miljö kräver, till exempel om programmet körs i Azure App Service miljö (ASE):

1. Öppna inställningar i hello web app kontroll-bladet.
2. Ange ”.net Framework-version” toov4.6.
3. Ange ”alltid på” tooOn.
4. Lägga till appinställningen ”__APPINSIGHTS_INSTRUMENTATIONKEY__” och ange hello värdet toohello samma instrumentation nyckel som används av hello SDK.
5. Öppna avancerade verktyg.
6. Klicka på ”Gå” tooopen hello Kudu webbplats.
7. I hello Kudu-webbplatsen, väljer du ”webbplatstillägg”.
8. Installera ”__Programinsikter__” från galleriet.
9. Starta om hello webbprogram.

## <a id="aspnetcore"></a>ASP.NET Core-stöd

ASP.NET Core programmet måste tooinstall Microsoft.ApplicationInsights.AspNetCore Nuget-paketet 2.1.0-beta6 eller högre toowork med hello Profiler. Vi stöder inte längre hello lägre versioner efter 6/27/2017.

## <a name="next-steps"></a>Nästa steg

* [Arbeta med Application Insights i Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png
