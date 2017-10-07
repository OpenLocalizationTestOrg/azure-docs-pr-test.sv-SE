---
title: aaaStreaming loggar och konsolen
description: "Direktuppspelningsloggar och översikt över administratörskonsolen"
author: btardif
manager: erikre
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: 3e50a287-781b-4c6a-8c53-eec261889d7a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/12/2016
ms.author: byvinyal
ms.openlocfilehash: bb4b8ce5358da12041e164dfff8f43790dd67924
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="streaming-logs-and-hello-console"></a>Direktuppspelningsloggar och hello konsolen
## <a name="streaming-logs"></a>Direktuppspelningsloggar
Hej **Azure-portalen** ger en integrerad strömmande Loggvisaren där du kan visa spårninghändelser från din **Apptjänst** appar i realtid.  

Ställa in den här funktionen kräver några enkla steg:

* Skriva spårningar i koden
* Aktivera programmet **diagnostikloggar** för din app
* Visa hello ström från hello inbyggda **strömning loggar** Användargränssnittet hello **Azure-portalen**.

### <a name="how-toowrite-traces-in-your-code"></a>Hur toowrite spårar i koden
Det är enkelt att skriva spårningar i koden.  Det är lika enkelt som att skriva följande kod hello i C#:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

hello Trace klassen bor i hello System.Diagnostics namnområde.

I en node.js-app kan du skriva koden tooachieve hello samma resultat:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-tooenable-and-view-hello-streaming-logs"></a>Hur tooenable och visa hello direktuppspelningsloggar
![][BrowseSitesScreenshot]Diagnostik är aktiverade på grundval av per app. Starta genom att bläddra toohello plats som tooenable denna funktion på.  

![][DiagnosticsLogs]Från inställningsmenyn rullar toohello **övervakning** avsnittet och klicka på **(1) diagnostikloggar**. Sedan **(2) Aktivera** **programloggning (filsystem)** eller **programloggning (blob)** hello **nivå** alternativet kan du ändra hello allvarlighetsgraden spårningar toocapture. Om du försöker bara tooget bekant med hello funktion, ange hello nivå för**utförlig** tooensure alla trace-satser som samlas in.

Klicka på **spara** hello överst på bladet hello och du är redo tooview loggar.

> [!NOTE]
> hello högre hello **allvarlighetsgrad** hello mer resurser är förbrukade toolog och hello mer spårningar produceras. Kontrollera att **allvarlighetsgrad** är konfigurerade toohello rätt detaljnivå för en produktion eller hög belastning på platsen. 
> 
> 

![][StreamingLogsScreenshot]tooview hello **direktuppspelningsloggar** från inom hello Azure-portalen klickar du på **(1) loggström** även i hello **övervakning** hello Inställningar-menyn. Om din app aktivt skriver trace-satser, så du bör se dem i hello **(2) strömning loggar UI** i nära realtid.

## <a name="console"></a>Konsolen
Hej **Azure-portalen** ger tooyour åtkomst-konsolapp. Du kan utforska appens filsystemet och köra powershell/cmd-skript. Du är bundna av hello samma behörigheter som angetts som din Appkod som körs när konsolkommandon. Åtkomst tooprotected kataloger eller skript som körs som kräver utökade behörigheter har blockerats.  

![][ConsoleScreenshot]Från inställningsmenyn rulla nedåt för**utvecklingsverktyg** avsnittet och klicka på **(1) konsolen** och hello **(2) konsolen** UI öppnar toohello höger.

Bekanta dig med hello tooget **konsolen**, försök grundläggande kommandon som:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
