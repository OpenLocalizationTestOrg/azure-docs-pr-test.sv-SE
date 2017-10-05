---
title: "Lär dig hur du använder SharePoint Online-anslutningen i logikappar | Microsoft Docs"
description: "Skapa logikappar med SharePoint Online kopplingen för att hantera listor i SharePoint."
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
ms.openlocfilehash: 96fc1347604c0c6cc2c2463a5dbd83b560183a16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-sharepoint-online-connector"></a><span data-ttu-id="06bf3-103">Kom igång med SharePoint Online connector</span><span class="sxs-lookup"><span data-stu-id="06bf3-103">Get started with the SharePoint Online connector</span></span>
<span data-ttu-id="06bf3-104">Använda SharePoint Online-anslutningen för att hantera SharePoint-listor.</span><span class="sxs-lookup"><span data-stu-id="06bf3-104">Use the SharePoint Online connector to manage SharePoint lists.</span></span>  

<span data-ttu-id="06bf3-105">Att använda [alla anslutningar](apis-list.md), måste du först skapa en logikapp.</span><span class="sxs-lookup"><span data-stu-id="06bf3-105">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="06bf3-106">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="06bf3-106">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-sharepoint-online"></a><span data-ttu-id="06bf3-107">Anslut till SharePoint Online</span><span class="sxs-lookup"><span data-stu-id="06bf3-107">Connect to SharePoint Online</span></span>
<span data-ttu-id="06bf3-108">Innan din logikapp kan komma åt någon tjänst, måste du först skapa en *anslutning* till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="06bf3-108">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="06bf3-109">En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="06bf3-109">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-to-sharepoint-online"></a><span data-ttu-id="06bf3-110">Skapa en anslutning till SharePoint Online</span><span class="sxs-lookup"><span data-stu-id="06bf3-110">Create a connection to SharePoint Online</span></span>
> [!INCLUDE [Steps to create a connection to SharePoint](../../includes/connectors-create-api-sharepointonline.md)]


## <a name="use-a-sharepoint-online-trigger"></a><span data-ttu-id="06bf3-111">Använda en SharePoint Online utlösare</span><span class="sxs-lookup"><span data-stu-id="06bf3-111">Use a SharePoint Online trigger</span></span>
<span data-ttu-id="06bf3-112">En utlösare är en händelse som kan användas för att starta arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="06bf3-112">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="06bf3-113">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="06bf3-113">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps to create a SharePoint Online trigger](../../includes/connectors-create-api-sharepointonline-trigger.md)]


## <a name="use-a-sharepoint-online-action"></a><span data-ttu-id="06bf3-114">Använd en SharePoint Online-åtgärd</span><span class="sxs-lookup"><span data-stu-id="06bf3-114">Use a SharePoint Online action</span></span>
<span data-ttu-id="06bf3-115">En åtgärd är en åtgärd som utförs av arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="06bf3-115">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="06bf3-116">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="06bf3-116">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!INCLUDE [Steps to create a SharePoint Online action](../../includes/connectors-create-api-sharepointonline-action.md)]


## <a name="connector-specific-details"></a><span data-ttu-id="06bf3-117">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="06bf3-117">Connector-specific details</span></span>

<span data-ttu-id="06bf3-118">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/sharepoint/).</span><span class="sxs-lookup"><span data-stu-id="06bf3-118">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/sharepoint/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="06bf3-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="06bf3-119">Next Steps</span></span>
[<span data-ttu-id="06bf3-120">Skapa en logikapp</span><span class="sxs-lookup"><span data-stu-id="06bf3-120">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

