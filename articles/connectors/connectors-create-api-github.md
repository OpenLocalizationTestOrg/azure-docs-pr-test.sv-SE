---
title: GitHub-anslutningen i Azure Logic Apps | Microsoft Docs
description: "Skapa logikappar med Azure App service. GitHub är en webbaserad Git-lagringsplats värd för tjänsten. Den erbjuder alla distribuerade revision kontroll- och koden management (SCM) funktioner av Git samt lägga till egna funktioner."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: d4614b0ceff0ec0d36dbb1a136551f985f2fc1a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-github-connector"></a><span data-ttu-id="a6f81-105">Kom igång med GitHub-koppling</span><span class="sxs-lookup"><span data-stu-id="a6f81-105">Get started with the GitHub connector</span></span>
<span data-ttu-id="a6f81-106">GitHub är en webbaserad Git-lagringsplats värd för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a6f81-106">GitHub is a web-based Git repository hosting service.</span></span> <span data-ttu-id="a6f81-107">Den erbjuder alla distribuerade revision kontroll- och koden management (SCM) funktioner av Git samt lägga till egna funktioner.</span><span class="sxs-lookup"><span data-stu-id="a6f81-107">It offers all of the distributed revision control and source code management (SCM) functionality of Git as well as adding its own features.</span></span>

<span data-ttu-id="a6f81-108">Du kan komma igång genom att skapa en logikapp nu, se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="a6f81-108">You can get started by creating a Logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-github"></a><span data-ttu-id="a6f81-109">Skapa en anslutning till GitHub</span><span class="sxs-lookup"><span data-stu-id="a6f81-109">Create a connection to GitHub</span></span>
<span data-ttu-id="a6f81-110">För att skapa logikappar med GitHub, måste du först skapa en **anslutning** ange detaljer för följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="a6f81-110">To create Logic apps with GitHub, you must first create a **connection** then provide the details for the following properties:</span></span> 

| <span data-ttu-id="a6f81-111">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a6f81-111">Property</span></span> | <span data-ttu-id="a6f81-112">Krävs</span><span class="sxs-lookup"><span data-stu-id="a6f81-112">Required</span></span> | <span data-ttu-id="a6f81-113">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a6f81-113">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a6f81-114">Token</span><span class="sxs-lookup"><span data-stu-id="a6f81-114">Token</span></span> |<span data-ttu-id="a6f81-115">Ja</span><span class="sxs-lookup"><span data-stu-id="a6f81-115">Yes</span></span> |<span data-ttu-id="a6f81-116">Ange autentiseringsuppgifter för GitHub</span><span class="sxs-lookup"><span data-stu-id="a6f81-116">Provide GitHub Credentials</span></span> |

<span data-ttu-id="a6f81-117">Du kan använda den köra åtgärder och lyssna för utlösare som beskrivs i den här artikeln när du har skapat anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a6f81-117">After you create the connection, you can use it to execute the actions and listen for the triggers described in this article.</span></span> 

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="a6f81-118">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="a6f81-118">Connector-specific details</span></span>

<span data-ttu-id="a6f81-119">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/github/).</span><span class="sxs-lookup"><span data-stu-id="a6f81-119">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/github/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="a6f81-120">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="a6f81-120">More connectors</span></span>
<span data-ttu-id="a6f81-121">Gå tillbaka till den [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="a6f81-121">Go back to the [APIs list](apis-list.md).</span></span>