---
title: aaaLearn toouse hello Azure Service Bus-anslutningen i dina logic apps | Microsoft Docs
description: "Skapa logikappar med Azure App service. Anslut tooAzure Service Bus toosend och ta emot meddelanden. Du kan utföra åtgärder, till exempel skicka tooqueue, skicka tootopic, ta emot från kön och tar emot från prenumerationen."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/02/2016
ms.author: mandia; ladocs
ms.openlocfilehash: c815ac167c3106ade470ce139d119085558a9497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-azure-service-bus-connector"></a>Kom igång med hello Azure Service Bus-koppling
Anslut tooAzure Service Bus toosend och ta emot meddelanden. Du kan utföra åtgärder, till exempel skicka tooqueue, skicka tootopic, ta emot från kön och tar emot från prenumerationen.

toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp. Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-tooservice-bus"></a>Ansluta tooService Bus
Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en anslutning toohello tjänst. En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.  

> [!INCLUDE [Steps toocreate a connection tooAzure Service Bus](../../includes/connectors-create-api-servicebus.md)]
> 
> 

## <a name="use-a-service-bus-trigger"></a>Använda en Service Bus-utlösare
En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden. [Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!INCLUDE [Steps toocreate a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]
> 
> 

## <a name="use-a-service-bus-action"></a>Använda en Service Bus-åtgärd
En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp. [Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

[!INCLUDE [Steps toocreate a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/servicebus/). 

## <a name="next-steps"></a>Nästa steg
[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

