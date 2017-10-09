---
title: "aaaDebug Azure mikrotjänster i Windows | Microsoft Docs"
description: "Lär dig hur toomonitor och diagnostisera dina tjänster som skrivits med Microsoft Azure Service Fabric på en dator för lokal utveckling."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: edcc0631-ed2d-45a3-851d-2c4fa0f4a326
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2017
ms.author: dekapur
ms.openlocfilehash: 24868aa194b8a28fa3e6de95c1de5506d912a544
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Övervaka och diagnostisera tjänster i en inställning för utveckling av lokal dator
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
> 
> 

Övervaka, identifiera, diagnostisera och felsöka tillåta tjänster toocontinue med minimala störningar toohello användarupplevelsen. Övervakning och diagnostik är viktiga i en verklig distribuerade produktionsmiljön, beror hello effektivitet på införandet av en liknande modell under utvecklingen av tjänster tooensure de fungerar när du flyttar tooa verkliga installationen. Service Fabric gör det enkelt för tjänsten utvecklare tooimplement diagnostik fungerar sömlöst över både inställningar för enskild dator lokal utveckling och produktion verkliga klustret installationsprogram.

## <a name="event-tracing-for-windows"></a>Händelsespårning för Windows
[Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) är hello rekommenderas teknik för spårning av meddelanden i Service Fabric. Några av fördelarna med att använda ETW är:

* **ETW är snabbt.** Det har skapats som en spårning-teknik som har minimal inverkan på koden körningstider.
* **ETW-spårning fungerar sömlöst över lokala utvecklingsmiljöer och verkliga klustret inställningar.** Det innebär att du inte har toorewrite din spårning code när du är klar toodeploy kod tooa verkliga klustret.
* **Service Fabric system koden används även ETW för interna spårning.** Detta ger dig tooview programspårningar överlagrat med Service Fabric system spårningar. Det hjälper även toomore enkelt förstå hello sekvenser och relationer mellan din programkod och händelser i hello underliggande system.
* **Det finns inbyggt stöd i Service Fabric Visual Studio tools tooview ETW-händelser.** ETW-händelser som visas i hello diagnostiska händelser vy av Visual Studio när Visual Studio är korrekt konfigurerad med Service Fabric. 

## <a name="view-service-fabric-system-events-in-visual-studio"></a>Visa händelser för Service Fabric-system i Visual Studio
Service Fabric avger ETW-händelser toohelp programutvecklare förstå vad som händer i hello-plattformen. Om du inte redan gjort det. Gå vidare och följer hello stegen i [skapa ditt första program i Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md). Den här informationen hjälper du får ett program körs med hello diagnostik Loggboken visar hello trace meddelanden.

1. Om hello diagnostik händelser fönstret inte visas automatiskt går toohello **visa** i Visual Studio, Välj **andra fönster** och sedan **diagnostiska Loggboken**.
2. Varje händelse har standard metadata-information som talar om hello nod, och en tjänst hello händelsen kommer från. Du kan också filtrera hello lista över händelser med hjälp av hello **Filtrera händelser** rutan hello överst i fönstret för hello-händelser. Du kan till exempel filtrera på **nodnamnet** eller **tjänstnamn.** Och när du tittar på händelseinformationen, du kan också pausa med hjälp av hello **pausa** knappen hello överst i fönstret för hello händelser och återuppta senare utan att förlora händelser.
   
   ![Visual Studio diagnostik Loggboken](./media/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally/DiagEventsExamples2.png)

## <a name="add-your-own-custom-traces-toohello-application-code"></a>Lägga till egna anpassade spårningar toohello programkoden
hello Service Fabric Visual Studio-projektmallar innehåller exempelkod. hello-kod visar hur tooadd anpassad programkod ETW spårar som visar upp i hello Visual Studio ETW viewer tillsammans med system spår från Service Fabric. hello nytta av den här metoden är att metadata läggs till automatiskt tootraces och hello Visual Studio diagnostiska Loggboken är redan har konfigurerat toodisplay dem.

För projekt som skapas från hello **tjänstmallar** (tillståndslösa och tillståndskänsliga) bara söka efter hello `RunAsync` implementering:

1. Hej anrop för`ServiceEventSource.Current.ServiceMessage` i hello `RunAsync` metoden visar ett exempel på en anpassad ETW-spårning från hello programkod.
2. I hello **ServiceEventSource.cs** filen hittar du en överlagring för hello `ServiceEventSource.ServiceMessage` metod som ska användas för hög frekvens händelser på grund av tooperformance skäl.

För projekt som skapas från hello **aktören mallar** (tillståndslösa och tillståndskänsliga):

1. Öppna hello **”projektnamn” .cs** filen var *projektnamn* är hello namn du angav för Visual Studio-projekt.  
2. Hitta hello kod `ActorEventSource.Current.ActorMessage(this, "Doing Work");` i hello *DoWorkAsync* metod.  Detta är ett exempel på en anpassad ETW-spårning som skrivs från programkod.  
3. I filen **ActorEventSource.cs**, hittar du en överlagring för hello `ActorEventSource.ActorMessage` metod som ska användas för hög frekvens händelser på grund av tooperformance skäl.

När du lägger till anpassade ETW tracing tooyour service-koden kan du skapa, distribuera och köra programmet hello igen toosee din händelse(r) i hello diagnostiska Loggboken. Om du felsöker hello program med **F5**, hello diagnostiska Loggboken öppnas automatiskt.

## <a name="next-steps"></a>Nästa steg
hello fungerar samma spårning kod som du har lagt till tooyour program ovan för lokala diagnostik med verktyg som du kan använda tooview händelserna när du kör programmet på ett Azure-kluster. Gå igenom dessa artiklar som hello olika användningsalternativ för hello verktyg och beskriver hur du kan konfigurera dem.

* [Hur toocollect loggar med Azure-diagnostik](service-fabric-diagnostics-how-to-setup-wad.md)
* [Aggregering av händelse och med EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md)

