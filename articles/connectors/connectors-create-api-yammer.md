---
title: "Lägg till Yammer-koppling i dina Azure Logic Apps | Microsoft Docs"
description: "Översikt över Yammer-anslutningen med REST API-parametrar"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5ae0827-fbb3-45ec-8f45-ad1cc2e7eccc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: c7a213343b4fb2b5a89a5052a459061b404a431c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-yammer-connector"></a><span data-ttu-id="447a0-103">Kom igång med Yammer-koppling</span><span class="sxs-lookup"><span data-stu-id="447a0-103">Get started with the Yammer connector</span></span>
<span data-ttu-id="447a0-104">Ansluta till Yammer till access konversationer i företagsnätverket.</span><span class="sxs-lookup"><span data-stu-id="447a0-104">Connect to Yammer to access conversations in your enterprise network.</span></span> <span data-ttu-id="447a0-105">Med Yammer kan du:</span><span class="sxs-lookup"><span data-stu-id="447a0-105">With Yammer, you can:</span></span>

* <span data-ttu-id="447a0-106">Skapa ditt företag flödet som baseras på de data som du får från Yammer.</span><span class="sxs-lookup"><span data-stu-id="447a0-106">Build your business flow based on the data you get from Yammer.</span></span> 
* <span data-ttu-id="447a0-107">Använd utlöser för när det finns ett nytt meddelande i en grupp eller en feed din följande.</span><span class="sxs-lookup"><span data-stu-id="447a0-107">Use triggers for when there is a new message in a group, or a feed your following.</span></span>
* <span data-ttu-id="447a0-108">Använd åtgärder för att skicka ett meddelande får alla meddelanden med mera.</span><span class="sxs-lookup"><span data-stu-id="447a0-108">Use actions to post a message, get all messages, and more.</span></span> <span data-ttu-id="447a0-109">De här åtgärderna få svar och utdata gör tillgängligt för andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="447a0-109">These actions get a response, and then make the output available for other actions.</span></span> <span data-ttu-id="447a0-110">När ett nytt meddelande visas, kan du skicka ett e-postmeddelande med hjälp av Office 365.</span><span class="sxs-lookup"><span data-stu-id="447a0-110">For example, when a new message appears, you can send an email using Office 365.</span></span>

<span data-ttu-id="447a0-111">Kom igång genom att skapa en logikapp nu. Se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="447a0-111">Get started by creating a logic app now; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-yammer"></a><span data-ttu-id="447a0-112">Skapa en anslutning till Yammer</span><span class="sxs-lookup"><span data-stu-id="447a0-112">Create a connection to Yammer</span></span>
<span data-ttu-id="447a0-113">Om du vill använda Yammer-anslutningstjänsten måste du först skapa en **anslutning** ange detaljer för dessa egenskaper:</span><span class="sxs-lookup"><span data-stu-id="447a0-113">To use the Yammer connector, you first create a **connection** then provide the details for these properties:</span></span> 

| <span data-ttu-id="447a0-114">Egenskap</span><span class="sxs-lookup"><span data-stu-id="447a0-114">Property</span></span> | <span data-ttu-id="447a0-115">Krävs</span><span class="sxs-lookup"><span data-stu-id="447a0-115">Required</span></span> | <span data-ttu-id="447a0-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="447a0-116">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="447a0-117">Token</span><span class="sxs-lookup"><span data-stu-id="447a0-117">Token</span></span> |<span data-ttu-id="447a0-118">Ja</span><span class="sxs-lookup"><span data-stu-id="447a0-118">Yes</span></span> |<span data-ttu-id="447a0-119">Ange autentiseringsuppgifter för Yammer</span><span class="sxs-lookup"><span data-stu-id="447a0-119">Provide Yammer Credentials</span></span> |

> [!INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="447a0-120">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="447a0-120">Connector-specific details</span></span>

<span data-ttu-id="447a0-121">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/yammer/).</span><span class="sxs-lookup"><span data-stu-id="447a0-121">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/yammer/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="447a0-122">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="447a0-122">More connectors</span></span>
<span data-ttu-id="447a0-123">Gå tillbaka till den [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="447a0-123">Go back to the [APIs list](apis-list.md).</span></span>