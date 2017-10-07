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
# <a name="get-started-with-hello-azure-service-bus-connector"></a><span data-ttu-id="adf73-105">Kom igång med hello Azure Service Bus-koppling</span><span class="sxs-lookup"><span data-stu-id="adf73-105">Get started with hello Azure Service Bus connector</span></span>
<span data-ttu-id="adf73-106">Anslut tooAzure Service Bus toosend och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="adf73-106">Connect tooAzure Service Bus toosend and receive messages.</span></span> <span data-ttu-id="adf73-107">Du kan utföra åtgärder, till exempel skicka tooqueue, skicka tootopic, ta emot från kön och tar emot från prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="adf73-107">You can perform actions such as send tooqueue, send tootopic, receive from queue, and receive from subscription.</span></span>

<span data-ttu-id="adf73-108">toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp.</span><span class="sxs-lookup"><span data-stu-id="adf73-108">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="adf73-109">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="adf73-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooservice-bus"></a><span data-ttu-id="adf73-110">Ansluta tooService Bus</span><span class="sxs-lookup"><span data-stu-id="adf73-110">Connect tooService Bus</span></span>
<span data-ttu-id="adf73-111">Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en anslutning toohello tjänst.</span><span class="sxs-lookup"><span data-stu-id="adf73-111">Before your logic app can access any service, you first need toocreate a connection toohello service.</span></span> <span data-ttu-id="adf73-112">En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="adf73-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

> [!INCLUDE [Steps toocreate a connection tooAzure Service Bus](../../includes/connectors-create-api-servicebus.md)]
> 
> 

## <a name="use-a-service-bus-trigger"></a><span data-ttu-id="adf73-113">Använda en Service Bus-utlösare</span><span class="sxs-lookup"><span data-stu-id="adf73-113">Use a Service Bus trigger</span></span>
<span data-ttu-id="adf73-114">En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="adf73-114">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="adf73-115">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="adf73-115">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps toocreate a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]
> 
> 

## <a name="use-a-service-bus-action"></a><span data-ttu-id="adf73-116">Använda en Service Bus-åtgärd</span><span class="sxs-lookup"><span data-stu-id="adf73-116">Use a Service Bus action</span></span>
<span data-ttu-id="adf73-117">En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="adf73-117">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="adf73-118">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="adf73-118">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

[!INCLUDE [Steps toocreate a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]

## <a name="connector-specific-details"></a><span data-ttu-id="adf73-119">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="adf73-119">Connector-specific details</span></span>

<span data-ttu-id="adf73-120">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/servicebus/).</span><span class="sxs-lookup"><span data-stu-id="adf73-120">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/servicebus/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="adf73-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="adf73-121">Next steps</span></span>
<span data-ttu-id="adf73-122">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="adf73-122">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

