---
title: Direktuppspelningsloggar och konsolen
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
ms.openlocfilehash: 120ce6b115820728b9f853e9ff349beb0ef29c9d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="streaming-logs-and-the-console"></a>Direktuppspelningsloggar och konsolen
## <a name="streaming-logs"></a>Direktuppspelningsloggar
Den **Azure-portalen** ger en integrerad strömmande Loggvisaren där du kan visa spårninghändelser från din **Apptjänst** appar i realtid.  

Ställa in den här funktionen kräver några enkla steg:

* Skriva spårningar i koden
* Aktivera programmet **diagnostikloggar** för din app
* Visa ström från inbyggt **strömning loggar** Användargränssnittet i den **Azure-portalen**.

### <a name="how-to-write-traces-in-your-code"></a>Hur du skriver spårningar i koden
Det är enkelt att skriva spårningar i koden.  Det är lika enkelt som att skriva följande kod i C#:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Klassen Trace bor i namnområdet System.Diagnostics.

Du kan skriva koden för att uppnå samma resultat i en node.js-app:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>Aktivera och visa strömning loggar
![][BrowseSitesScreenshot]Diagnostik är aktiverade på grundval av per app. Starta genom att bläddra till platsen som du vill aktivera den här funktionen på.  

![][DiagnosticsLogs]Från inställningsmenyn, bläddra till den **övervakning** avsnittet och klicka på **(1) diagnostikloggar**. Sedan **(2) Aktivera** **programloggning (filsystem)** eller **programloggning (blob)** den **nivå** alternativet kan du ändra den allvarlighetsgraden spårningar att avbilda. Om du försöker bara bekanta dig med funktionen, ställa in på **utförlig** så alla trace-satser som samlas in.

Klicka på **spara** längst upp på bladet och du är redo att visa loggar.

> [!NOTE]
> Det högre av **allvarlighetsgrad** mer spåren produceras och mer resurser används för att logga. Kontrollera att **allvarlighetsgrad** konfigureras till att rätt detaljnivå för en produktion eller hög belastning på platsen. 
> 
> 

![][StreamingLogsScreenshot]Visa den **direktuppspelningsloggar** från inom Azure-portalen klickar du på **(1) loggström** även i den **övervakning** avsnitt i inställningsmenyn. Om din app aktivt skriver trace-satser, så du bör se dem i den **(2) strömning loggar UI** i nära realtid.

## <a name="console"></a>Konsolen
Den **Azure-portalen** ger åtkomst till din app. Du kan utforska appens filsystemet och köra powershell/cmd-skript. Du är bundna av samma behörigheter som din Appkod som körs när konsolkommandon. Åtkomst till skyddade kataloger eller skript som körs som kräver förhöjd behörighet har blockerats.  

![][ConsoleScreenshot]Rulla ned till Inställningar-menyn **utvecklingsverktyg** avsnittet och klicka på **(1) konsolen** och **(2) konsolen** UI öppnas till höger.

Att bekanta dig med de **konsolen**, försök grundläggande kommandon som:

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
