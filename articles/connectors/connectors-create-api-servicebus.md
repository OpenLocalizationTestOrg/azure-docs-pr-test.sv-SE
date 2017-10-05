---
title: "Lär dig hur du använder Azure Service Bus-anslutningen i dina logic apps | Microsoft Docs"
description: "Skapa logikappar med Azure App service. Ansluta till Azure Service Bus skickar och tar emot meddelanden. Du kan utföra åtgärder, till exempel skicka kö, skickar till avsnitt, tar emot från kön och tar emot från prenumerationen."
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
ms.openlocfilehash: 1e2ce06f5993280dbdb67121849591e67f7979e9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-azure-service-bus-connector"></a><span data-ttu-id="30a5e-105">Kom igång med Azure Service Bus-koppling</span><span class="sxs-lookup"><span data-stu-id="30a5e-105">Get started with the Azure Service Bus connector</span></span>
<span data-ttu-id="30a5e-106">Ansluta till Azure Service Bus skickar och tar emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="30a5e-106">Connect to Azure Service Bus to send and receive messages.</span></span> <span data-ttu-id="30a5e-107">Du kan utföra åtgärder, till exempel skicka kö, skickar till avsnitt, tar emot från kön och tar emot från prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="30a5e-107">You can perform actions such as send to queue, send to topic, receive from queue, and receive from subscription.</span></span>

<span data-ttu-id="30a5e-108">Att använda [alla anslutningar](apis-list.md), måste du först skapa en logikapp.</span><span class="sxs-lookup"><span data-stu-id="30a5e-108">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="30a5e-109">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="30a5e-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-service-bus"></a><span data-ttu-id="30a5e-110">Ansluta till Service Bus</span><span class="sxs-lookup"><span data-stu-id="30a5e-110">Connect to Service Bus</span></span>
<span data-ttu-id="30a5e-111">Innan din logikapp kan komma åt någon tjänst, måste du först skapa en anslutning till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="30a5e-111">Before your logic app can access any service, you first need to create a connection to the service.</span></span> <span data-ttu-id="30a5e-112">En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="30a5e-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

> [!INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]
> 
> 

## <a name="use-a-service-bus-trigger"></a><span data-ttu-id="30a5e-113">Använda en Service Bus-utlösare</span><span class="sxs-lookup"><span data-stu-id="30a5e-113">Use a Service Bus trigger</span></span>
<span data-ttu-id="30a5e-114">En utlösare är en händelse som kan användas för att starta arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="30a5e-114">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="30a5e-115">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="30a5e-115">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]
> 
> 

## <a name="use-a-service-bus-action"></a><span data-ttu-id="30a5e-116">Använda en Service Bus-åtgärd</span><span class="sxs-lookup"><span data-stu-id="30a5e-116">Use a Service Bus action</span></span>
<span data-ttu-id="30a5e-117">En åtgärd är en åtgärd som utförs av arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="30a5e-117">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="30a5e-118">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="30a5e-118">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

[!INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]

## <a name="connector-specific-details"></a><span data-ttu-id="30a5e-119">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="30a5e-119">Connector-specific details</span></span>

<span data-ttu-id="30a5e-120">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/servicebus/).</span><span class="sxs-lookup"><span data-stu-id="30a5e-120">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/servicebus/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="30a5e-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="30a5e-121">Next steps</span></span>
<span data-ttu-id="30a5e-122">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="30a5e-122">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

