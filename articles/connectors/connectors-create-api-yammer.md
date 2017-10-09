---
title: aaaAdd hello Yammer-anslutningen i dina Logic Apps i Azure | Microsoft Docs
description: "Översikt över hello Yammer-anslutningen med REST API-parametrar"
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
ms.openlocfilehash: be75df770a8062d839926dff8c0195d2647f78d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-yammer-connector"></a><span data-ttu-id="de27f-103">Kom igång med hello Yammer-koppling</span><span class="sxs-lookup"><span data-stu-id="de27f-103">Get started with hello Yammer connector</span></span>
<span data-ttu-id="de27f-104">Ansluta tooYammer tooaccess konversationer i företagsnätverket.</span><span class="sxs-lookup"><span data-stu-id="de27f-104">Connect tooYammer tooaccess conversations in your enterprise network.</span></span> <span data-ttu-id="de27f-105">Med Yammer kan du:</span><span class="sxs-lookup"><span data-stu-id="de27f-105">With Yammer, you can:</span></span>

* <span data-ttu-id="de27f-106">Skapa ditt företag flödet baserat på hello data du får från Yammer.</span><span class="sxs-lookup"><span data-stu-id="de27f-106">Build your business flow based on hello data you get from Yammer.</span></span> 
* <span data-ttu-id="de27f-107">Använd utlöser för när det finns ett nytt meddelande i en grupp eller en feed din följande.</span><span class="sxs-lookup"><span data-stu-id="de27f-107">Use triggers for when there is a new message in a group, or a feed your following.</span></span>
* <span data-ttu-id="de27f-108">Använd åtgärder toopost ett meddelande, få alla meddelanden.</span><span class="sxs-lookup"><span data-stu-id="de27f-108">Use actions toopost a message, get all messages, and more.</span></span> <span data-ttu-id="de27f-109">De här åtgärderna få svar och hello utdata gör tillgängligt för andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="de27f-109">These actions get a response, and then make hello output available for other actions.</span></span> <span data-ttu-id="de27f-110">När ett nytt meddelande visas, kan du skicka ett e-postmeddelande med hjälp av Office 365.</span><span class="sxs-lookup"><span data-stu-id="de27f-110">For example, when a new message appears, you can send an email using Office 365.</span></span>

<span data-ttu-id="de27f-111">Kom igång genom att skapa en logikapp nu. Se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="de27f-111">Get started by creating a logic app now; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-tooyammer"></a><span data-ttu-id="de27f-112">Skapa en anslutning tooYammer</span><span class="sxs-lookup"><span data-stu-id="de27f-112">Create a connection tooYammer</span></span>
<span data-ttu-id="de27f-113">toouse hello Yammer-anslutningen kan du först skapa en **anslutning** ange hello information för dessa egenskaper:</span><span class="sxs-lookup"><span data-stu-id="de27f-113">toouse hello Yammer connector, you first create a **connection** then provide hello details for these properties:</span></span> 

| <span data-ttu-id="de27f-114">Egenskap</span><span class="sxs-lookup"><span data-stu-id="de27f-114">Property</span></span> | <span data-ttu-id="de27f-115">Krävs</span><span class="sxs-lookup"><span data-stu-id="de27f-115">Required</span></span> | <span data-ttu-id="de27f-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="de27f-116">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="de27f-117">Token</span><span class="sxs-lookup"><span data-stu-id="de27f-117">Token</span></span> |<span data-ttu-id="de27f-118">Ja</span><span class="sxs-lookup"><span data-stu-id="de27f-118">Yes</span></span> |<span data-ttu-id="de27f-119">Ange autentiseringsuppgifter för Yammer</span><span class="sxs-lookup"><span data-stu-id="de27f-119">Provide Yammer Credentials</span></span> |

> [!INCLUDE [Steps toocreate a connection tooYammer](../../includes/connectors-create-api-yammer.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="de27f-120">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="de27f-120">Connector-specific details</span></span>

<span data-ttu-id="de27f-121">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/yammer/).</span><span class="sxs-lookup"><span data-stu-id="de27f-121">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/yammer/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="de27f-122">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="de27f-122">More connectors</span></span>
<span data-ttu-id="de27f-123">Gå tillbaka toohello [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="de27f-123">Go back toohello [APIs list](apis-list.md).</span></span>
