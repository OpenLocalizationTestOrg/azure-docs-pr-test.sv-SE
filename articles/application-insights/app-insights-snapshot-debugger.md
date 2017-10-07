---
title: "aaaAzure Application Insights ögonblicksbild felsökare för .NET-appar | Microsoft Docs"
description: "Felsöka ögonblicksbilder automatiskt samlas in när undantag utlöstes i produktion .NET appar"
services: application-insights
documentationcenter: 
author: qubitron
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: bwren
ms.openlocfilehash: f0173a752b5795d934fbab1bd53eb077433edc90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a>Felsöka ögonblicksbilder på undantag i .NET-appar

När ett undantag inträffar kan du automatiskt samla in en debug ögonblicksbild webbtillämpningen live. hello ögonblicksbild visar hello tillstånd av källkoden och variabler vid hello ögonblick hello undantag utlöstes. hello ögonblicksbild felsökare (förhandsgranskning) i [Azure Application Insights](app-insights-overview.md) övervakar undantagstelemetri från ditt webbprogram. Den samlar in ögonblicksbilder på de översta utlösande undantag så att du har hello information du behöver toodiagnose problem i produktion. Inkludera hello [ögonblicksbild insamlaren NuGet-paketet](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) i ditt program och du kan också konfigurera samlingen parametrar i [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Ögonblicksbilder visas på [undantag](app-insights-asp-net-exceptions.md) i hello Application Insights-portalen.

Du kan visa debug ögonblicksbilder i anropsstacken för hello portal toosee hello och inspektera variabler vid varje anropsstacken.. tooget en kraftigare felsökning upplevelse med källkoden, öppna ögonblicksbilder med Visual Studio 2017 Enterprise av [hämtar hello ögonblicksbild felsökare tillägget för Visual Studio](https://aka.ms/snapshotdebugger).

Ögonblicksbild samlingen är tillgänglig för:
* .NET framework och ASP.NET-program med .NET Framework 4.5 eller senare.
* .NET core 2.0 och ASP.NET Core 2.0 program som körs på Windows.

### <a name="configure-snapshot-collection-for-aspnet-applications"></a>Konfigurera ögonblicksbild samling för ASP.NET-program

1. [Aktivera Application Insights i ditt webbprogram](app-insights-asp-net.md), om du inte gjort det ännu.

2. Inkludera hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-paketet i din app. 

3. Granska hello standardalternativen som hello paket som lagts till för[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- hello default is true, but you can disable Snapshot Debugging by setting it toofalse -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this tootrue. -->
        <!-- DeveloperMode is a property on hello active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need toosee an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>5</ThresholdForSnapshotting>
        <!-- hello maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- hello maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often tooreset problem counters. -->
        <ProblemCounterResetInterval>06:00:00</ProblemCounterResetInterval>
        <!-- hello maximum number of snapshots allowed in one minute. -->
        <SnapshotsPerMinuteLimit>2</SnapshotsPerMinuteLimit>
        <!-- hello maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>50</SnapshotsPerDayLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. Ögonblicksbilder samlas endast för undantag som har rapporterat tooApplication insikter. I vissa fall (till exempel äldre versioner av hello .NET-plattformen) kan du behöva för[konfigurera undantag samling](app-insights-asp-net-exceptions.md#exceptions) toosee undantag med ögonblicksbilder i hello-portalen.


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a>Konfigurera ögonblicksbild samlingen för ASP.NET Core 2.0-program

1. [Aktivera Application Insights i ditt webbprogram för ASP.NET Core](app-insights-asp-net-core.md), om du inte gjort det ännu.

2. Inkludera hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-paketet i din app.

3. Ändra hello `ConfigureServices` metod i ditt program `Startup` klassen tooadd hello ögonblicksbild insamlarens telemetri processor. hello-kod som du bör lägga till beror på hello refererade version av hello Microsoft.ApplicationInsights.ASPNETCore NuGet-paketet.

   Lägg till för Microsoft.ApplicationInsights.AspNetCore 2.1.0:
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       // This method is called by hello runtime. Use it tooadd services toohello container.
       public void ConfigureServices(IServiceCollection services)
       {
           services.AddSingleton<Func<ITelemetryProcessor, ITelemetryProcessor>>(next => new SnapshotCollectorTelemetryProcessor(next));
           // TODO: Add any other services your application needs here.
       }
   }
   ```

   Lägg till för Microsoft.ApplicationInsights.AspNetCore 2.1.1:
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           public ITelemetryProcessor Create(ITelemetryProcessor next) =>
               new SnapshotCollectorTelemetryProcessor(next);
       }

       // This method is called by hello runtime. Use it tooadd services toohello container.
       public void ConfigureServices(IServiceCollection services)
       {
            services.AddSingleton<ITelemetryProcessorFactory>(new SnapshotCollectorTelemetryProcessorFactory());
           // TODO: Add any other services your application needs here.
       }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a>Konfigurera ögonblicksbild samling för andra .NET-program

1. Om programmet inte är redan instrumenterats med Application Insights, Kom igång genom att [aktivera Application Insights och inställningen hello instrumentation nyckeln](app-insights-windows-desktop.md).

2. Lägg till hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-paketet i din app.

3. Ögonblicksbilder samlas endast för undantag som har rapporterat tooApplication insikter. Du kan behöva toomodify din kod tooreport dem. hello undantagshantering kod beror på hello strukturen för ditt program, men ett exempel är lägre än:
    ```C#
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle hello request.
        }
        catch (Exception ex)
        {
            // Report hello exception tooApplication Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow hello exception if desired.
        }
   }
    ```
    
## <a name="grant-permissions"></a>Bevilja behörighet

Ägarna av hello Azure-prenumeration kan inspektera ögonblicksbilder. Andra användare måste beviljas behörighet genom en ägare.

toogrant behörighet, tilldela hello `Application Insights Snapshot Debugger` rollen toousers som ska granska ögonblicksbilder. Den här rollen kan tilldelas tooindividual användare eller grupper av prenumeration ägare för hello mål Application Insights-resurs eller dess resursgrupp eller prenumeration.

1. Öppna bladet för hello Access Control (IAM).
1. Klicka på hello + Lägg till.
1. Välj Application Insights ögonblicksbild felsökare hello roller nedrullningsbara listan.
1. Sök efter och ange ett namn för hello användaren tooadd.
1. Klicka på hello spara knappen tooadd hello toohello användarroll.


[!IMPORTANT]
    Ögonblicksbilder kan innehålla personliga och annan känslig information i variabeln och parametervärden.

## <a name="debug-snapshots-in-hello-application-insights-portal"></a>Felsöka ögonblicksbilder i hello Application Insights-portalen

Om en ögonblicksbild som är tillgänglig för en viss undantag eller problem-ID, en **öppna Debug ögonblicksbild** visas knappen på hello [undantag](app-insights-asp-net-exceptions.md) i hello Application Insights-portalen.

![Öppna Debug ögonblicksbild knappen undantag](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

I hello Debug ögonblicksbild vy visas en anropsstacken och en variabler rutan. När du väljer ramar av hello anropet stacken i hello anropet stack rutan, kan du visa lokala variabler och parametrar för funktionen anropa i hello variabler rutan.

![Visa Debug ögonblicksbild i hello-portalen](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

Ögonblicksbilder kan innehålla känslig information, och som standard är de inte kan visas. tooview ögonblicksbilder, måste du ha hello `Application Insights Snapshot Debugger` tooyou tilldelats rollen.

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a>Felsöka ögonblicksbilder med Visual Studio 2017 Enterprise
1. Klicka på hello **hämta ögonblicksbild** knappen toodownload en `.diagsession` fil som kan öppnas i Visual Studio 2017 Enterprise. 

2. tooopen hello `.diagsession` -fil, måste du först [ladda ned och installera hello ögonblicksbild felsökare tillägget för Visual Studio](https://aka.ms/snapshotdebugger).

3. När du har öppnat hello snapshot-fil visas hello Minidump felsökning i Visual Studio. Klicka på **Debug förvaltad kod** toostart felsökning hello ögonblicksbild. hello ögonblicksbild öppnas toohello kodrad där hello undantag utlöstes så att du kan felsöka hello hello processen aktuella tillstånd.

    ![Visa debug ögonblicksbild i Visual Studio](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

hello hämtade ögonblicksbild innehåller symbolfiler som hittades på webbservern för programmet. Symbolen är obligatoriska tooassociate ögonblicksbilddata med källkod. För Apptjänst-appar kontrollerar du att tooenable symbol distribution när du publicerar dina webbprogram.

## <a name="how-snapshots-work"></a>Så här fungerar ögonblicksbilder

När programmet startas skapas en separat ögonblicksbild överföring process som övervakar ditt program för snapshot-begäranden. När en ögonblicksbild har begärts görs en skuggkopia av hello körs i cirka 10 too20 minuter. hello shadow processen analyseras och en ögonblicksbild skapas när hello huvudsakliga processen fortsätter toorun och ger toousers trafik. hello ögonblicksbild är överförda tooApplication insikter tillsammans med symbolfiler som är relevanta (.pdb) behövs tooview hello ögonblicksbild.

## <a name="current-limitations"></a>Aktuella begränsningar

### <a name="publish-symbols"></a>Publicera symboler
hello ögonblicksbild felsökare kräver symbolfiler på hello produktion toodecode servervariabler och tooprovide Avbrottsfritt felsökning i Visual Studio. hello 15,2 versionen av Visual Studio 2017 publicerar symboler för versionen versioner som standard när den publicerar tooApp Service. I tidigare versioner måste tooadd hello följande rad tooyour publiceringsprofil `.pubxml` så att symboler publiceras i versionsläge:

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

För Azure Compute och andra typer, se till att hello symbolfiler hello samma mapp för hello programmets DLL (vanligtvis `wwwroot/bin`) eller som är tillgängliga på hello aktuella sökvägen.

### <a name="optimized-builds"></a>Optimerad versioner
I vissa fall, kan lokala variabler inte visas i versionen versioner på grund av optimeringar som tillämpas under hello build-processen.

## <a name="troubleshooting"></a>Felsökning

De här tipsen hjälper dig att felsöka problem med hello ögonblicksbild felsökare.

### <a name="verify-hello-instrumentation-key"></a>Verifiera hello instrumentation nyckeln

Kontrollera att du använder hello rätt instrumentation nyckel i ditt publicerade program. Application Insights läser vanligtvis hello instrumentation nyckeln från hello ApplicationInsights.config-filen. Kontrollera att hello värdet hello samma som hello instrumentation nyckel för hello Application Insights-resurs som du ser i hello-portalen.

### <a name="check-hello-uploader-logs"></a>Loggarna hello-överförare

När du har skapat en ögonblicksbild skapas en minidumpfil (.dmp) på disken. En separat överförare-process tar minidump filen och överförs, tillsammans med alla associerade PDB-filer, tooApplication insikter ögonblicksbild felsökare lagring. När hello minidump har laddats bort den från disken. hello loggfiler för hello minidump överföring finns kvar på disken. I en Apptjänst-miljö kan du hitta dessa loggar i `D:\Home\LogFiles\Uploader_*.log`. Använd hello Kudu hanteringswebbplats för Apptjänst toofind dessa loggfiler.

1. Öppna din App-tjänstprogrammet i hello Azure-portalen.

2. Välj hello **avancerade verktyg** bladet eller söka efter **Kudu**.
3. Klicka på **Gå**.
4. I hello **Felsökningskonsolen** listrutan markerar **CMD**.
5. Klicka på **loggfiler**.

Du bör se minst en fil med ett namn som börjar med `Uploader_` och en `.log` tillägg. Klicka på hello önskad ikon toodownload alla loggfiler eller öppna dem i en webbläsare.
hello-filnamnet innehåller hello datornamnet. Om din App Service-instans finns på flera datorer, finns separata loggfiler för varje dator. När hello överföring upptäcker en ny minidumpfil, registreras den i hello loggfilen. Här är ett exempel på uppladdningen lyckas:

```
MinidumpUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:08.0349846Z
MinidumpUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 329.12 MB
    DateTime=2017-05-25T14:25:16.0145444Z
MinidumpUploader.exe Information: 0 : Upload successful.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2017-05-25T14:25:44.2310982Z
MinidumpUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2017-05-25T14:25:44.5435948Z
MinidumpUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:44.6095821Z
```

I föregående exempel hello hello instrumentation nyckeln är `c12a605e73c44346a984e00000000000`. Det här värdet ska matcha hello instrumentation nyckeln för ditt program.
hello minidump är associerad med en ögonblicksbild med hello ID `139e411a23934dc0b9ea08a626db16c5`. Du kan använda detta ID senare toolocate hello associerade undantagstelemetri i Application Insights Analytics.

Hej överförare söker efter nya PDB-filer om var 15: e minut. Här är ett exempel:

```
MinidumpUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\home\site\wwwroot\ for local PDBs.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\local\Temporary ASP.NET Files\root\a6554c94\e3ad6f22\assembly\dl3\81d5008b\00b93cc8_dec5d201 for local PDBs.
    DateTime=2017-05-25T15:11:38.8160276Z
MinidumpUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2017-05-25T15:11:38.8316450Z
MinidumpUploader.exe Information: 0 : Deleted PDB scan marker D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\.pdbscan.
    DateTime=2017-05-25T15:11:38.8316450Z
```

För program som är _inte_ finns i App Service, hello överföring loggar finns i hello samma mapp som hello minidumpar: `%TEMP%\Dumps\<ikey>` (där `<ikey>` är instrumentation-nyckel).

### <a name="use-application-insights-search-toofind-exceptions-with-snapshots"></a>Använd Application Insights söka toofind undantag med ögonblicksbilder

När en ögonblicksbild skapas är hello att undantag märkta med en ögonblicksbild-ID. När hello undantagstelemetri rapporterade tooApplication insikter ingår som ögonblicksbilds-ID som en anpassad egenskap. Använder hello Sök bladet i Application Insights, du kan hitta all telemetri med hello `ai.snapshot.id` anpassad egenskap.

1. Bläddra tooyour Application Insights-resurs i hello Azure-portalen.
2. Klicka på **Sök**.
3. Typen `ai.snapshot.id` i hello sökrutan och tryck på RETUR.

![Sök efter telemetri med en ögonblicksbild ID i hello-portalen](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

Om den här sökningen returnerar inga resultat har inga rapporterade tooApplication insikter för ditt program i hello valt tidsintervall.

toosearch för en specifik ögonblicksbilds-ID från hello överföring loggar skriver detta ID i hello sökrutan. Om du inte hittar telemetri för en ögonblicksbild som du vet överfördes, gör du följande:

1. Kontrollera att du tittar på hello rätt Application Insights-resursen genom att verifiera hello instrumentation nyckel.

2. Med hello tidsstämpel från hello överföring loggen kan justera hello tidsintervall filter för hello Sök toocover att tidsintervallet för rapporten.

Om du fortfarande inte ser ett undantag med detta ID ögonblicksbild inte hello undantagstelemetri rapporterade tooApplication insikter. Detta kan inträffa om programmet kraschade när det tog hello ögonblicksbild men innan den rapporterades hello undantagstelemetri. I det här fallet Kontrollera hello Apptjänst loggar under `Diagnose and solve problems` toosee om det fanns oväntat startar om eller ohanterade undantag.

## <a name="next-steps"></a>Nästa steg

* [Ange snappoints i koden](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/) tooget ögonblicksbilder utan att vänta på ett undantag.
* [Diagnostisera undantag i web apps](app-insights-asp-net-exceptions.md) förklarar hur toomake flera undantag synliga tooApplication insikter. 
* [Identifiering för smartkort](app-insights-proactive-diagnostics.md) upptäcker automatiskt prestandaavvikelser.
