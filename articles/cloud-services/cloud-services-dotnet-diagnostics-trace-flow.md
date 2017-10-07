---
title: "aaaTrace hello flödet i Cloud Services-program med Azure-diagnostik | Microsoft Docs"
description: "Lägg till spårning meddelanden tooan Azure-program toohelp felsökning, mäta prestanda, övervakning, trafik analys och mer."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: 
ms.assetid: 09934772-cc07-4fd2-ba88-b224ca192f8e
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/20/2016
ms.author: robb
ms.openlocfilehash: d2ed7b5997ae1d298115b4ce593bb5051a9a0c75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="trace-hello-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Spåra hello flödet av ett Cloud Services-program med Azure-diagnostik
Spårning är ett sätt för du toomonitor hello körningen av program när den körs. Du kan använda hello [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), och [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) klasserna toorecord information om fel och program som körs i loggar, textfiler eller andra enheter för senare analys. Mer information om spårning finns [spårning och instrumentering program](https://msdn.microsoft.com/library/zs6s4h68.aspx).

## <a name="use-trace-statements-and-trace-switches"></a>Använda trace-satser och spåra växlar
Implementera spårning i tillämpningsprogrammet molntjänster genom att lägga till hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) toohello programkonfigurationen och gör anrop tooSystem.Diagnostics.Trace eller System.Diagnostics.Debug i din programkod. Använd hello konfigurationsfilen *app.config* för arbetsroller och hello *web.config* för webbroller. När du skapar en ny värdbaserad tjänst med en mall för Visual Studio toohello projektet läggs automatiskt till Azure-diagnostik och hello DiagnosticMonitorTraceListener läggs toohello lämplig konfigurationsfilen för hello roller som du lägger till.

Information om att placera trace-satser finns [så här: Lägg till Trace-satser tooApplication koden](https://msdn.microsoft.com/library/zd83saa2.aspx).

Genom att placera [Trace växlar](https://msdn.microsoft.com/library/3at424ac.aspx) i koden, du kan styra om spårning sker och hur omfattande är. På så sätt kan du övervaka hello status för ditt program i en produktionsmiljö. Detta är särskilt viktigt i ett affärsprogram som använder flera komponenter som körs på flera datorer. Mer information finns i [så här: konfigurera spårning växlar](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-hello-trace-listener-in-an-azure-application"></a>Konfigurera hello spårningslyssnaren i ett Azure-program
Spåra, felsökning och TraceSource, måste du ställa in ”lyssnare” toocollect och registrera hello-meddelanden som skickas. Lyssnare samla in, lagra och vidarebefordra spårning av meddelanden. De direkt hello spårning utdata tooan lämpliga mål, till exempel en logg, fönster eller textfil. Azure Diagnostics använder hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) klass.

Innan du slutför hello följer proceduren måste du initiera hello Azure diagnostikövervakare. toodo detta, se [aktiverar diagnostik i Microsoft Azure](cloud-services-dotnet-diagnostics.md).

Observera att om du använder hello-mallar som tillhandahålls av Visual Studio hello konfigurationen av hello lyssnare läggs automatiskt åt dig.

### <a name="add-a-trace-listener"></a>Lägga till en spårningsavlyssningen
1. Öppna hello web.config eller app.config-filen för din roll.
2. Lägg till följande kod toohello hello. Ändra hello attributet toouse hello versionsnummer hello-paketet som du refererar till. hello Sammansättningsversionen ändras inte nödvändigtvis i varje version av Azure SDK om det finns uppdateringar tooit.
   
    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                    <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
   > [!IMPORTANT]
   > Kontrollera att du har ett projekt referens toohello Microsoft.WindowsAzure.Diagnostics sammansättning. Uppdatera hello versionsnumret i hello xml ovan toomatch hello version av hello refereras Microsoft.WindowsAzure.Diagnostics sammansättning.
   > 
   > 
3. Spara hello config-fil.

Läs mer om lyssnare [Samlingsspårlyssnarna](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Du kan lägga till Spårningskod instruktioner tooyour när du har slutfört hello steg tooadd hello lyssnare.

### <a name="tooadd-trace-statement-tooyour-code"></a>tooadd Spårningskod instruktionen tooyour
1. Öppna en källfil för programmet. Till exempel hello <RoleName>.cs filen för hello worker-rollen eller webbroll.
2. Lägg till hello följande med instruktionen om det inte redan lagts:
    ```
        using System.Diagnostics;
    ```
3. Lägg till Trace-satser där du vill toocapture information om hello tillståndet för programmet. Du kan använda olika metoder tooformat hello utdata från hello spårningsinstruktionen. Mer information finns i [så här: Lägg till Trace-satser tooApplication koden](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Spara hello källfilen.

