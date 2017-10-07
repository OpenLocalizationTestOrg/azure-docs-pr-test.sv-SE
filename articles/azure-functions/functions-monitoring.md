---
title: aaaMonitoring Azure Functions | Microsoft Docs
description: "Lär dig hur toomonitor Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "azure-funktioner, funktioner, händelsebearbetning, webhooks, dynamisk beräkning, serverlös arkitektur"
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/03/2016
ms.author: wesmc
ms.openlocfilehash: 254348d1cefce925654bd9532715b6def571e0ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-azure-functions"></a>Övervaka Azure Functions

## <a name="overview"></a>Översikt 


Hej **övervakaren** för varje funktion kan du tooreview varje körning av en funktion.

![Azure fliken för övervakning av funktioner](./media/functions-monitoring/monitor-tab.png) 

Om du klickar på en körning kan du tooreview hello varaktighet, indata, fel och associerade loggfilerna. Detta är användbart felsökning och prestandajustering dina funktioner.


> [!IMPORTANT]
> När du använder hello [förbrukning som värd för planen](functions-overview.md#pricing) Azure Functions hello **övervakning** panelen i hello Funktionsapp översikt bladet visas inte några data. Detta beror på att hello plattform dynamiskt skalar och hanterar compute-instanser för dig, så de här måtten inte meningsfull på en plan för användning. toomonitor hello användning av funktionen-appar, bör du istället använda hello vägledning i den här artikeln.
> 
> hello följande skärmbild som visar ett exempel:
> 
> ![Övervakning på hello huvudsakliga resursbladet](./media/functions-monitoring/app-service-overview-monitoring.png)



## <a name="real-time-monitoring"></a>Realtidsövervakning

Realtidsövervakning är tillgänglig genom att klicka på **live händelseströmmen** enligt nedan. 

![Live händelse dataströmmen alternativ för hello övervakningsfliken](./media/functions-monitoring/monitor-tab-live-event-stream.png)

hello direktsänd händelse dataströmmen ska visas i diagram registreringen i en ny webbläsarflik enligt nedan. 

![Direktsänd händelse stream-exempel](./media/functions-monitoring/live-event-stream.png)


> [!NOTE]
> Det finns ett känt problem som kan medföra att dina data toofail toobe fylls i. Om du får detta kan du behöva tooclose hello webbläsare fliken som innehåller hello live händelseströmmen och klicka sedan på **live händelseströmmen** igen tooallow den tooproperly fylla din händelsedata för dataströmmen. 

hello direktsänd händelse dataströmmen kommer kurva hello följande statistik för din funktion:

* Körningar som startas per sekund
* Körningar som slutförs varje sekund
* Körningar som misslyckas per sekund
* Genomsnittlig körningstid i millisekunder.

Statistiken är realtid men hello faktiska rita in av hello Körningsdata kanske cirka 10 sekunder svarstid.






## <a name="monitoring-log-files-from-a-command-line"></a>Övervaka loggfilerna från en kommandorad


Du kan strömma loggen filer tooa kommandoraden session på en lokal arbetsstation med hello Azure-kommandoradsgränssnittet (CLI) eller PowerShell.

### <a name="monitoring-function-app-log-files-with-hello-azure-cli"></a>Övervakning av funktionen app-loggfiler med hello Azure CLI

tooget har startats [installera hello Azure CLI](../cli-install-nodejs.md)

Logga in på ditt Azure-konto med hjälp av hello följande kommando, eller andra hello andra alternativ som beskrivs i, [logga in tooAzure från hello Azure CLI](../xplat-cli-connect.md).

    azure login

Använd hello följande kommando tooenable Azure CLI Service Management (ASM) läge:.

    azure config mode asm

Om du har flera prenumerationer använder du följande kommandon toolist hello dina prenumerationer och ange hello aktuell prenumeration toohello prenumeration som innehåller appen funktionen.

    azure account list
    azure account set <subscriptionNameOrId>

hello direktuppspelas följande kommando hello loggfiler funktionen app toohello kommandoraden:

    azure site log tail -v <function app name>

### <a name="monitoring-function-app-log-files-with-powershell"></a>Övervakning av funktionen app-loggfiler med PowerShell

tooget har startats [installera och konfigurera Azure PowerShell](/powershell/azure/overview).

Lägg till ditt Azure-konto genom att köra följande kommando hello:

    PS C:\> Add-AzureAccount

Om du har flera prenumerationer, kan du visa dem efter namn med hello efter kommandot toosee om hello rätt prenumerationen är hello markerade baserat på `IsCurrent` egenskapen:

    PS C:\> Get-AzureSubscription

Om du behöver tooset hello aktiv prenumeration toohello ett som innehåller appen funktionen använder du följande kommando hello:

    PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription

Dataströmmen hello loggar tooyour PowerShell-session med hello följande kommando:

    PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail

Mer information finns för[så här: strömma loggar för web apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs). 

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello följande resurser:

* [Testa en funktion](functions-test-a-function.md)
* [Skala en funktion](functions-scale.md)

