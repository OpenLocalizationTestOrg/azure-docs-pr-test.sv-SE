---
title: Outlook.com-anslutningen i Azure Logic Apps | Microsoft Docs
description: "Skapa logikappar med Azure App service. Outlook.com-anslutningen kan du hantera din e-post, kalender och kontakter. Du kan utföra olika åtgärder, till exempel skicka e-post, schemalägga möten, Lägg till kontakter, osv."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 87113c85-d158-4dd5-9ed5-5748130003d6
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: bde1629504c97cf6706b42219570ffa6243073dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-outlookcom-connector"></a><span data-ttu-id="2dac6-105">Kom igång med Outlook.com-koppling</span><span class="sxs-lookup"><span data-stu-id="2dac6-105">Get started with the Outlook.com connector</span></span>
<span data-ttu-id="2dac6-106">Outlook.com-anslutningen kan du hantera din e-post, kalender och kontakter.</span><span class="sxs-lookup"><span data-stu-id="2dac6-106">Outlook.com connector allows you to manage your mail, calendars, and contacts.</span></span> <span data-ttu-id="2dac6-107">Du kan utföra olika åtgärder, till exempel skicka e-post, schemalägga möten, Lägg till kontakter, osv.</span><span class="sxs-lookup"><span data-stu-id="2dac6-107">You can perform various actions such as send mail, schedule meetings, add contacts, etc.</span></span>

<span data-ttu-id="2dac6-108">Du kan komma igång genom att skapa en logikapp nu, se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="2dac6-108">You can get started by creating a Logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-outlookcom"></a><span data-ttu-id="2dac6-109">Skapa en anslutning till Outlook.com</span><span class="sxs-lookup"><span data-stu-id="2dac6-109">Create a connection to Outlook.com</span></span>
<span data-ttu-id="2dac6-110">För att skapa logikappar med Outlook.com, måste du först skapa en **anslutning** ange detaljer för följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="2dac6-110">To create Logic apps with Outlook.com, you must first create a **connection** then provide the details for the following properties:</span></span>

| <span data-ttu-id="2dac6-111">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2dac6-111">Property</span></span> | <span data-ttu-id="2dac6-112">Krävs</span><span class="sxs-lookup"><span data-stu-id="2dac6-112">Required</span></span> | <span data-ttu-id="2dac6-113">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2dac6-113">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2dac6-114">Token</span><span class="sxs-lookup"><span data-stu-id="2dac6-114">Token</span></span> |<span data-ttu-id="2dac6-115">Ja</span><span class="sxs-lookup"><span data-stu-id="2dac6-115">Yes</span></span> |<span data-ttu-id="2dac6-116">Ange autentiseringsuppgifter för Outlook.com</span><span class="sxs-lookup"><span data-stu-id="2dac6-116">Provide Outlook.com Credentials</span></span> |

<span data-ttu-id="2dac6-117">Du kan använda den köra åtgärder och lyssna för utlösare som beskrivs i den här artikeln när du har skapat anslutningen.</span><span class="sxs-lookup"><span data-stu-id="2dac6-117">After you create the connection, you can use it to execute the actions and listen for the triggers described in this article.</span></span>

> [!INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)]
>

## <a name="connector-specific-details"></a><span data-ttu-id="2dac6-118">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="2dac6-118">Connector-specific details</span></span>

<span data-ttu-id="2dac6-119">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/outlook/).</span><span class="sxs-lookup"><span data-stu-id="2dac6-119">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/outlook/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="2dac6-120">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="2dac6-120">More connectors</span></span>
<span data-ttu-id="2dac6-121">Gå tillbaka till den [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="2dac6-121">Go back to the [APIs list](apis-list.md).</span></span>