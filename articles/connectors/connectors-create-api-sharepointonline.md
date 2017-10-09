---
title: aaaLearn hur toouse hello SharePoint Online connector i logikappar | Microsoft Docs
description: Skapa logikappar med hello SharePoint Online connector toomange listor i SharePoint.
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: e0ec3149-507a-409d-8e7b-d5fbded006ce
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/19/2016
ms.author: mandia; ladocs
ms.openlocfilehash: fc2f2745f0d174eae6165e84fd8b6656b0aac44b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sharepoint-online-connector"></a><span data-ttu-id="53f08-103">Kom igång med SharePoint Online hello-koppling</span><span class="sxs-lookup"><span data-stu-id="53f08-103">Get started with hello SharePoint Online connector</span></span>
<span data-ttu-id="53f08-104">Använd hello SharePoint Online connector toomanage SharePoint-listor.</span><span class="sxs-lookup"><span data-stu-id="53f08-104">Use hello SharePoint Online connector toomanage SharePoint lists.</span></span>  

<span data-ttu-id="53f08-105">toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp.</span><span class="sxs-lookup"><span data-stu-id="53f08-105">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="53f08-106">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="53f08-106">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toosharepoint-online"></a><span data-ttu-id="53f08-107">Ansluta tooSharePoint Online</span><span class="sxs-lookup"><span data-stu-id="53f08-107">Connect tooSharePoint Online</span></span>
<span data-ttu-id="53f08-108">Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en *anslutning* toohello service.</span><span class="sxs-lookup"><span data-stu-id="53f08-108">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="53f08-109">En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="53f08-109">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-toosharepoint-online"></a><span data-ttu-id="53f08-110">Skapa en anslutning tooSharePoint Online</span><span class="sxs-lookup"><span data-stu-id="53f08-110">Create a connection tooSharePoint Online</span></span>
> [!INCLUDE [Steps toocreate a connection tooSharePoint](../../includes/connectors-create-api-sharepointonline.md)]


## <a name="use-a-sharepoint-online-trigger"></a><span data-ttu-id="53f08-111">Använda en SharePoint Online utlösare</span><span class="sxs-lookup"><span data-stu-id="53f08-111">Use a SharePoint Online trigger</span></span>
<span data-ttu-id="53f08-112">En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="53f08-112">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="53f08-113">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="53f08-113">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps toocreate a SharePoint Online trigger](../../includes/connectors-create-api-sharepointonline-trigger.md)]


## <a name="use-a-sharepoint-online-action"></a><span data-ttu-id="53f08-114">Använd en SharePoint Online-åtgärd</span><span class="sxs-lookup"><span data-stu-id="53f08-114">Use a SharePoint Online action</span></span>
<span data-ttu-id="53f08-115">En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="53f08-115">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="53f08-116">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="53f08-116">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps toocreate a SharePoint Online action](../../includes/connectors-create-api-sharepointonline-action.md)]


## <a name="connector-specific-details"></a><span data-ttu-id="53f08-117">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="53f08-117">Connector-specific details</span></span>

<span data-ttu-id="53f08-118">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/sharepoint/).</span><span class="sxs-lookup"><span data-stu-id="53f08-118">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/sharepoint/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="53f08-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53f08-119">Next Steps</span></span>
[<span data-ttu-id="53f08-120">Skapa en logikapp</span><span class="sxs-lookup"><span data-stu-id="53f08-120">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

