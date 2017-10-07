---
title: "aaaAzure Application Insights för Windows server- och arbetsroller roller | Microsoft Docs"
description: "Manuellt lägga till hello Application Insights SDK tooyour ASP.NET tooanalyze programanvändning, tillgänglighet och prestanda."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 106ba99b-b57a-43b8-8866-e02f626c8190
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: 64643ef637195d10f87fc6020a77169bca66c1f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manually-configure-application-insights-for-net-applications"></a>Konfigurera Application Insights för .NET manuellt

Du kan konfigurera [Programinsikter](app-insights-overview.md) toomonitor en mängd olika program eller programroller, komponenter eller mikrotjänster. Visual Studio erbjuder [enstegskonfiguration](app-insights-asp-net.md) för webbappar och tjänster. För andra typer av .NET-program, som till exempel roller för serverdelar eller skrivbordsprogram, kan du konfigurera Application Insights manuellt.

![Exempel på prestandaövervakningsdiagram](./media/app-insights-windows-services/10-perf.png)

#### <a name="before-you-start"></a>Innan du börjar

Du behöver:

* En prenumeration för[Microsoft Azure](http://azure.com). Om din grupp eller organisation har en Azure-prenumeration, hello ägare kan lägga till dig tooit, med hjälp av din [Microsoft-konto](http://live.com).
* Visual Studio 2013 eller senare.

## <a name="add"></a>1. Välj en Application Insights-resurs

hello resursen är där dina data samlas in och visas i hello Azure-portalen. Du behöver toodecide om toocreate en ny eller en befintlig resurs.

### <a name="part-of-a-larger-app-use-existing-resource"></a>En del av en större app: Använd en befintlig resurs

Om ditt webbprogram har flera komponenter – till exempel en frontend-webbprogram och en eller flera backend-tjänster – så att du ska skicka telemetri från alla hello komponenter toohello samma resurs. Detta kommer att de visas på en enda programavbildningen toobe och gör det möjligt tootrace en begäran från en komponent tooanother.

Så om du redan övervakar andra komponenter i den här appen, sedan bara Använd hello samma resurs.

Öppna hello resurs i hello [Azure-portalen](https://portal.azure.com/). 

### <a name="self-contained-app-create-a-new-resource"></a>Fristående app: Skapa en ny resurs

Om hello nya appen är inte relaterat tooother program, bör den ha en egen resurs.

Logga in toohello [Azure-portalen](https://portal.azure.com/), och skapa en ny Application Insights-resurs. Välj ASP.NET som hello programtyp.

![Klicka på Nytt, Application Insights](./media/app-insights-windows-services/01-new-asp.png)

hello val av programtyp anger hello standardinnehåll hello resurs blad.

## <a name="2-copy-hello-instrumentation-key"></a>2. Kopiera hello Instrumentation nyckel
hello nyckel identifierar hello resurs. Du måste installera den snart i hello SDK, i ordning toodirect data toohello resursen.

![Klicka på egenskaper, markera hello nyckeln och tryck på ctrl + C](./media/app-insights-windows-services/02-props-asp.png)

## <a name="sdk"></a>3. Installationspaket som hello Application Insights i ditt program
Installera och konfigurera hello Application Insights varierar paketet beroende på hello-plattform som du arbetar med. 

1. I Visual Studio högerklickar du på projektet och väljer **Hantera Nuget-paket**.
   
    ![Högerklicka på hello-projektet och välj Hantera Nuget-paket](./media/app-insights-windows-services/03-nuget.png)
2. Installera hello Application Insights-paketet för Windows server-appar, ”Microsoft.ApplicationInsights.WindowsServer”.
   
    ![Sök efter ”Application Insights”](./media/app-insights-windows-services/04-ai-nuget.png)
   
    *Vilken version?*

    Kontrollera **inkludera förhandsversion** om du vill tootry våra senaste funktioner. hello relevanta dokument eller bloggar du Observera att om du behöver en förhandsversion.
    
    *Kan jag använda andra paket?*
   
    Ja. Välj ”Microsoft.ApplicationInsights” om du bara vill toouse hello API toosend egna telemetri. hello Windows Server-paketet innehåller hello API plus ett antal andra paket, till exempel beroende övervakning och insamling av räknare för prestanda. 

### <a name="tooupgrade-toofuture-package-versions"></a>tooupgrade toofuture paketversionerna
Vi använda en ny version av hello SDK från tid tootime.

tooupgrade tooa [ny version av paketet hello](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/), öppna NuGet-Pakethanteraren igen och filtrera efter installerade paket. Markera **Microsoft.ApplicationInsights.WindowsServer** och välj **Uppgradera**.

Om du har gjort alla anpassningar tooApplicationInsights.config spara en kopia av den innan du uppgraderar och därefter sammanfoga ändringarna till hello ny version.

## <a name="4-send-telemetry"></a>4. Skicka telemetri
**Om du har installerat hello API-paketet:**

* Ange hello instrumentation nyckeln i koden, till exempel i `main()`: 
  
    `TelemetryConfiguration.Active.InstrumentationKey = "` *din nyckel* `";` 
* [Skriva egna telemetri med hello API](app-insights-api-custom-events-metrics.md#ikey).

**Om du har installerat andra Application Insights-paket** du kan om du föredrar att använda hello .config-fil tooset hello instrumentation nyckel:

* Redigera ApplicationInsights.config (som har lagts till av hello installera NuGet). Infoga det precis innan hello avslutande tagg:
  
    `<InstrumentationKey>`*hello instrumentation nyckel som du kopierade*`</InstrumentationKey>`
* Se till att hello egenskaper för ApplicationInsights.config i Solution Explorer är inställda för**Skapa åtgärd = innehåll, kopiera tooOutput Directory = kopiera**.

Det är användbart tooset hello instrumentation nyckel i kod om du vill använda för[växel hello nyckel för olika build konfigurationer](app-insights-separate-resources.md). Om du anger hello nyckel i koden kan du inte har tooset den i hello `.config` fil.

## <a name="run"></a> Köra projektet
Använd hello **F5** toorun programmet och prova: öppna olika sidor toogenerate vissa telemetri.

I Visual Studio visas antalet hello-händelser som har skickats.

![Antal händelser i Visual Studio](./media/app-insights-windows-services/appinsights-09eventcount.png)

## <a name="monitor"></a> Visa telemetrin
Returnera toohello [Azure-portalen](https://portal.azure.com/) och bläddra tooyour Application Insights-resurs.

Leta efter data i hello översikt över diagram. Först ser du bara en eller två punkter. Exempel:

![Klicka dig igenom toomore data](./media/app-insights-windows-services/12-first-perf.png)

Klicka på via alla diagram toosee mer detaljerad mått. [Lär dig mer om mätvärden.](app-insights-web-monitor-performance.md)

### <a name="no-data"></a>Ser du inga data?
* Använda programmet hello, öppna olika sidor så att den genererar vissa telemetri.
* Öppna hello [Sök](app-insights-diagnostic-search.md) panelen toosee enskilda händelser. Ibland tar händelser lite medan längre tooget via hello mått pipeline.
* Vänta några sekunder och klicka på **Uppdatera**. Diagram uppdatera själva regelbundet, men du kan uppdatera manuellt om du väntar vissa data tooshow.
* Mer information finns i [Felsökning](app-insights-troubleshoot-faq.md).

## <a name="publish-your-app"></a>Publicera appen
Nu distribuera programservern tooyour eller tooAzure och titta på hello data ackumuleras.

![Använd Visual Studio toopublish din app](./media/app-insights-windows-services/15-publish.png)

När du kör i felsökningsläge expedierade telemetri via hello pipelinen, så att du bör se data som visas inom några sekunder. När du distribuerar appen i en publiceringskonfiguration ackumuleras data långsammare.

### <a name="no-data-after-you-publish-tooyour-server"></a>Inga data när du har publicerat tooyour server?
Öppna portar för utgående trafik i serverns brandvägg. Se [den här sidan](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) hello lista över begärda adresser 

### <a name="trouble-on-your-build-server"></a>Har du problem på build-servern?
Mer information finns i [det här felsökningsavsnittet](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [!NOTE]
> Om din app genererar mycket telemetri, minska hello anpassningsbar provtagning modulen automatiskt hello volymen som skickas toohello portal genom att skicka en representativ del av händelser. Dock händelser som är relaterade toohello samma begäran ska markeras eller avmarkeras som en grupp, så att du kan navigera mellan relaterade händelser. 
> [Lär dig mer om insamling](app-insights-sampling.md).
> 
> 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Nästa steg
* [Lägg till mer telemetri](app-insights-asp-net-more.md) tooget hello 360 graders vy av ditt program.

