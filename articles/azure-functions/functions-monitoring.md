---
title: "Övervaka Azure Functions | Microsoft Docs"
description: "Lär dig hur du övervakar dina Azure-funktioner."
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
ms.openlocfilehash: b70214593b1417265387f42306a633bb0df2920e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitoring-azure-functions"></a>Övervaka Azure Functions

## <a name="overview"></a>Översikt 


Den **övervakaren** för varje funktion kan du granska varje körning av en funktion.

![Azure fliken för övervakning av funktioner](./media/functions-monitoring/monitor-tab.png) 

Klicka på en körning kan du granska varaktighet, indata, fel och associerade loggfilerna. Detta är användbart felsökning och prestandajustering dina funktioner.


> [!IMPORTANT]
> När du använder den [förbrukning som värd för planen](functions-overview.md#pricing) för Azure Functions i **övervakning** panelen i bladet Funktionsapp översikt visas inte några data. Detta beror på att plattformen dynamiskt skalar och hanterar compute-instanser för dig, så de här måtten inte meningsfull på en plan för användning. För att övervaka användningen av funktionen-appar, bör du istället använda riktlinjerna i den här artikeln.
> 
> Följande skärmbild visar ett exempel:
> 
> ![Övervakning på huvudsakliga resursbladet](./media/functions-monitoring/app-service-overview-monitoring.png)



## <a name="real-time-monitoring"></a>Realtidsövervakning

Realtidsövervakning är tillgänglig genom att klicka på **live händelseströmmen** enligt nedan. 

![Direktsänd händelse dataströmmen alternativ för övervakning](./media/functions-monitoring/monitor-tab-live-event-stream.png)

Dataströmmen direktsänd händelse ska visas i diagram registreringen i en ny webbläsarflik enligt nedan. 

![Direktsänd händelse stream-exempel](./media/functions-monitoring/live-event-stream.png)


> [!NOTE]
> Det finns ett känt problem som kan orsaka att data ska kunna fyllas i. Om du får detta kan du behöva Stäng fliken som innehåller dataströmmen direktsänd händelse och klicka sedan på **live händelseströmmen** igen för att göra det möjligt att fylla i din händelsedata dataströmmen korrekt. 

Live händelseströmmen kommer kurva följande statistik för din funktion:

* Körningar som startas per sekund
* Körningar som slutförs varje sekund
* Körningar som misslyckas per sekund
* Genomsnittlig körningstid i millisekunder.

Statistiken är realtid men den faktiska grafiska Körningsdata kanske cirka 10 sekunder svarstid.






## <a name="monitoring-log-files-from-a-command-line"></a>Övervaka loggfilerna från en kommandorad


Du kan strömma loggfiler till en session på kommandoraden på en lokal arbetsstation med Azure-kommandoradsgränssnittet (CLI) eller PowerShell.

### <a name="monitoring-function-app-log-files-with-the-azure-cli"></a>Övervakning av funktionen app-loggfiler med Azure CLI

Du kommer igång [installerar Azure CLI](../cli-install-nodejs.md)

Logga in på ditt Azure-konto med hjälp av följande kommando, eller några av de alternativ som beskrivs i, [logga in till Azure från Azure CLI](../xplat-cli-connect.md).

    azure login

Använd följande kommando för att aktivera Azure CLI Service Management (ASM) läge:.

    azure config mode asm

Om du har flera prenumerationer, använder du följande kommandon lista dina prenumerationer och ange den aktuella prenumerationen till den prenumeration som innehåller funktionsapp.

    azure account list
    azure account set <subscriptionNameOrId>

Följande kommando direktuppspelas loggfilerna för funktionen appen till kommandoraden:

    azure site log tail -v <function app name>

### <a name="monitoring-function-app-log-files-with-powershell"></a>Övervakning av funktionen app-loggfiler med PowerShell

Du kommer igång [installera och konfigurera Azure PowerShell](/powershell/azure/overview).

Lägg till ditt Azure-konto genom att köra följande kommando:

    PS C:\> Add-AzureAccount

Om du har flera prenumerationer, kan du visa dem med namn med följande kommando för att se om korrekt prenumeration är den markerade baserat på `IsCurrent` egenskapen:

    PS C:\> Get-AzureSubscription

Om du behöver ange aktiv prenumeration till den som innehåller appen funktionen använder du följande kommando:

    PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription

Strömma loggar i PowerShell-sessionen med följande kommando:

    PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail

Mer information finns i [så här: strömma loggar för web apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs). 

## <a name="next-steps"></a>Nästa steg
Mer information finns i följande resurser:

* [Testa en funktion](functions-test-a-function.md)
* [Skala en funktion](functions-scale.md)

