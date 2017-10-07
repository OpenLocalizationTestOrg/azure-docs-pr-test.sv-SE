---
title: "aaaSeparating telemetri från utveckling, testa och släpp i Azure Application Insights | Microsoft Docs"
description: "Direkt telemetri toodifferent resurser för utveckling, test- och stämplar."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 578e30f0-31ed-4f39-baa8-01b4c2f310c9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: a294c8c70f46d7c29b460461c3494c83e13a0cbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="separating-telemetry-from-development-test-and-production"></a>Avgränsa telemetri från utveckling, Test och produktion

När du utvecklar hello nästa version av ett program måste du inte vill toomix in hello [Programinsikter](app-insights-overview.md) telemetri från hello nya och hello redan släppts-versionen. tooavoid förvirring skicka hello telemetri från olika development skapar tooseparate Application Insights-resurser med separat instrumentation nycklar (ikeys). toomake den enklare toochange hello instrumentation nyckeln som en version som flyttas från en fas tooanother det kan vara användbart tooset hello ikey i koden i stället för i hello konfigurationsfilen. 

(Om datorn är en Azure-molntjänst är [en annan metod för att separat ikeys](app-insights-cloudservices.md).)

## <a name="about-resources-and-instrumentation-keys"></a>Om resurser och instrumentation nycklar

När du ställer in Application Insights-övervakning för webbappen kan du skapa en Application Insights *resurs* i Microsoft Azure. Du öppnar den här resursen i hello Azure-portalen i ordning toosee och analysera hello telemetri som samlats in från din app. hello resurs har identifierats av en *instrumentation nyckeln* (ikey). När du installerar hello Application Insights paketet toomonitor din app kan du konfigurera den med hello instrumentation nyckel så att den vet där toosend hello telemetri.

Du vanligtvis välja toouse separata resurser eller en enda delad resurs i olika scenarier:

* Olika, oberoende program - använda en separat resurs och ikey för varje app.
* Flera komponenter eller roller från en affärsprogram - använda en [enkel delad resurs](app-insights-monitor-multi-role-apps.md) för alla hello komponenten appar. Telemetri kan filtreras eller segmenterade av hello cloud_RoleName-egenskapen.
* Utveckling och Test Release - använda en separat resurs och ikey för versioner av hello i 'i stämpel' eller steget i produktion.
* EN | B-testning - använda en enskild resurs. Skapa en TelemetryInitializer tooadd en egenskapen toohello telemetri som identifierar hello varianter.


## <a name="dynamic-ikey"></a>Dynamisk instrumentation nyckel

toomake underlättar toochange hello ikey som hello kod flyttar mellan stegen i produktion, ange den i koden i stället för i hello konfigurationsfil.

Ange hello nyckeln i en initieringsmetod, till exempel global.aspx.cs i en ASP.NET-tjänst:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

I det här exemplet placeras hello ikeys för hello olika resurser i olika versioner av hello Webbkonfigurationsfilen. Växlar hello Webbkonfigurationsfilen - som du kan göra i hello versionen skript - ska växla hello målresursen.

### <a name="web-pages"></a>Webbsidor
Hej iKey används också i din app webbsidor i hello [skript som du har fått från hello Snabbstart-bladet](app-insights-javascript.md). I stället för att koda den bokstavligt hello skript, generera den från hello Servertillstånd. Till exempel i en ASP.NET-app:

*JavaScript i Razor*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a>Skapa ytterligare Application Insights-resurser
tooseparate telemetri för olika programkomponenter eller annan stämplar (dev/test/produktion) av hello samma komponent och du har toocreate en ny Application Insights-resurs.

I hello [portal.azure.com](https://portal.azure.com), Lägg till Application Insights-resurs:

![Klicka på Nytt, Application Insights](./media/app-insights-separate-resources/01-new.png)

* **Programtyp** påverkar vad som visas på hello översikt bladet och hello-egenskaper som är tillgängliga i [mått explorer](app-insights-metrics-explorer.md). Välj ett av hello web typer för webbsidor om du inte ser typen av app.
* **Resursgruppen** är i syfte att underlätta för att hantera egenskaper som [åtkomstkontroll](app-insights-resources-roles-access-control.md). Du kan använda separata resursgrupper för utveckling, testning och produktion.
* **Prenumerationen** är din betalningskonto i Azure.
* **Plats** är där vi behåller dina data. För närvarande kan den inte ändras. 
* **Lägg till toodashboard** placerar en snabb åtkomst-panelen för din resurs på Azure-startsidan. 

Att skapa hello resurs tar några sekunder. En varning visas när du är klar.

(Du kan skriva en [PowerShell-skript](app-insights-powershell-script-create-resource.md) toocreate en resurs automatiskt.)

### <a name="getting-hello-instrumentation-key"></a>Hämta hello instrumentation nyckel
hello instrumentation nyckel identifierar hello-resurs som du skapade. 

![Klicka på Essentials, hello Instrumentation nyckeln CTRL + C](./media/app-insights-separate-resources/02-props.png)

Du behöver hello instrumentation nycklar för alla hello resurser toowhich appen skickar data.

## <a name="filter-on-build-number"></a>Filtrera efter build-nummer
När du publicerar en ny version av din app, ska du toobe kan tooseparate hello telemetri från olika versioner.

Du kan ange hello programversion egenskapen så att du kan filtrera [Sök](app-insights-diagnostic-search.md) och [mått explorer](app-insights-metrics-explorer.md) resultat.

![Filtrering på en egenskap](./media/app-insights-separate-resources/050-filter.png)

Det finns flera olika metoder för att egenskapen hello-programversionen.

* Ange direkt:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* Omsluta den raden i en [telemetri initieraren](app-insights-api-custom-events-metrics.md#defaults) tooensure som alla TelemetryClient instanser är konsekvent.
* [ASP.NET] Ange hello version i `BuildInfo.config`. hello webbmodul ska hämta hello-version från hello BuildLabel nod. Inkludera den här filen i projektet och Kom ihåg tooset hello alltid kopiera egenskapen i Solution Explorer.

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] Generera BuildInfo.config automatiskt i MSBuild. toodo, lägga till några rader tooyour `.csproj` fil:

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Detta genererar en fil med namnet *yourProjectName*. BuildInfo.config. hello publiceringsprocessen byter namn på den tooBuildInfo.config.

    hello build etiketten innehåller platshållare (AutoGen_...) när du skapar med Visual Studio. Men när byggts med MSBuild det fylls i med hello rätt versionsnumret.

    tooallow MSBuild toogenerate versionsnummer, ange hello version som `1.0.*` i AssemblyReference.cs

## <a name="version-and-release-tracking"></a>Spårning av versionen och utgåva
tootrack hello programversion, se till att `buildinfo.config` har genererats av Microsoft skapa Engine-processen. I filen .csproj lägger du till:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

När den har hello build info hello Application Insights webbmodulen lägger automatiskt till **programversion** som ett objekt med egenskapen tooevery av telemetri. Gör att du toofilter av version när du utför [diagnostiska sökningar](app-insights-diagnostic-search.md), eller när du [utforska mått](app-insights-metrics-explorer.md).

Observera dock att hello build-versionsnummer genereras bara av hello Microsoft skapa Engine, inte av hello utvecklare skapar i Visual Studio.

### <a name="release-annotations"></a>Versionsanteckningar
Om du använder Visual Studio Team Services, kan du [få en anteckning markör](app-insights-annotations.md) till tooyour diagram när du släpper en ny version. hello följande bild visar hur den här markören visas.

![Skärmbild av exempel på versionsanteckning i ett diagram](./media/app-insights-asp-net/release-annotation.png)
## <a name="next-steps"></a>Nästa steg

* [Delade resurser för flera roller](app-insights-monitor-multi-role-apps.md)
* [Skapa en telemetri initieraren toodistinguish A | B varianter](app-insights-api-filtering-sampling.md#add-properties)
