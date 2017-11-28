---
title: aaaLearn toouse hello Salesforce-anslutningsprogrammet i dina logic apps | Microsoft Docs
description: "Skapa logikappar med Azure App service. hello Salesforce-anslutningsprogrammet tillhandahåller ett API-toowork med Salesforce-objekt."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 54fe5af8-7d2a-4da8-94e7-15d029e029bf
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/05/2016
ms.author: mandia; ladocs
ms.openlocfilehash: b14b41fa8a4648b4f0090472dc0f9575bf13a2ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-salesforce-connector"></a><span data-ttu-id="6c08e-104">Kom igång med hello Salesforce-anslutningsprogrammet</span><span class="sxs-lookup"><span data-stu-id="6c08e-104">Get started with hello Salesforce connector</span></span>
<span data-ttu-id="6c08e-105">hello Salesforce-anslutningsprogrammet tillhandahåller ett API-toowork med Salesforce-objekt.</span><span class="sxs-lookup"><span data-stu-id="6c08e-105">hello Salesforce Connector provides an API toowork with Salesforce objects.</span></span>

<span data-ttu-id="6c08e-106">toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp.</span><span class="sxs-lookup"><span data-stu-id="6c08e-106">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="6c08e-107">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="6c08e-107">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toosalesforce-connector"></a><span data-ttu-id="6c08e-108">Ansluta tooSalesforce koppling</span><span class="sxs-lookup"><span data-stu-id="6c08e-108">Connect tooSalesforce connector</span></span>
<span data-ttu-id="6c08e-109">Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en *anslutning* toohello service.</span><span class="sxs-lookup"><span data-stu-id="6c08e-109">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="6c08e-110">En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="6c08e-110">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-toosalesforce-connector"></a><span data-ttu-id="6c08e-111">Skapa en anslutning tooSalesforce koppling</span><span class="sxs-lookup"><span data-stu-id="6c08e-111">Create a connection tooSalesforce connector</span></span>
> [!INCLUDE [Steps toocreate a connection tooSalesforce Connector](../../includes/connectors-create-api-salesforce.md)]
> 
> 

## <a name="use-a-salesforce-connector-trigger"></a><span data-ttu-id="6c08e-112">Använda en utlösare för Salesforce-koppling</span><span class="sxs-lookup"><span data-stu-id="6c08e-112">Use a Salesforce connector trigger</span></span>
<span data-ttu-id="6c08e-113">En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="6c08e-113">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="6c08e-114">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="6c08e-114">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

> [!INCLUDE [Steps toocreate a Salesforce trigger](../../includes/connectors-create-api-salesforce-trigger.md)]
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="6c08e-115">Lägg till ett villkor</span><span class="sxs-lookup"><span data-stu-id="6c08e-115">Add a condition</span></span>
> [!INCLUDE [Steps toocreate a Salesforce condition](../../includes/connectors-create-api-salesforce-condition.md)]
> 
> 

## <a name="use-a-salesforce-connector-action"></a><span data-ttu-id="6c08e-116">Använda en åtgärd för Salesforce-koppling</span><span class="sxs-lookup"><span data-stu-id="6c08e-116">Use a Salesforce connector action</span></span>
<span data-ttu-id="6c08e-117">En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="6c08e-117">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="6c08e-118">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="6c08e-118">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

> [!INCLUDE [Steps toocreate a Salesforce action](../../includes/connectors-create-api-salesforce-action.md)]
> 
> 

## <a name="connector-specific-details"></a><span data-ttu-id="6c08e-119">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="6c08e-119">Connector-specific details</span></span>

<span data-ttu-id="6c08e-120">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/salesforce/).</span><span class="sxs-lookup"><span data-stu-id="6c08e-120">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/salesforce/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="6c08e-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c08e-121">Next steps</span></span>
[<span data-ttu-id="6c08e-122">Skapa en logikapp</span><span class="sxs-lookup"><span data-stu-id="6c08e-122">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

