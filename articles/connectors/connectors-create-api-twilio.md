---
title: aaaAdd hello Twilio-anslutningen i dina logikappar i Azure | Microsoft Docs
description: "Översikt över hello Twilio-anslutningen med REST API-parametrar"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/19/2016
ms.author: mandia; ladocs
ms.openlocfilehash: b2b487f34bc76bee24b4237a71ee767d0d22ff7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-twilio-connector"></a><span data-ttu-id="382a9-103">Kom igång med hello Twilio-koppling</span><span class="sxs-lookup"><span data-stu-id="382a9-103">Get started with hello Twilio connector</span></span>
<span data-ttu-id="382a9-104">Anslut tooTwilio toosend och ta emot globala IP, SMS och MMS-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="382a9-104">Connect tooTwilio toosend and receive global SMS, MMS, and IP messages.</span></span> <span data-ttu-id="382a9-105">Med Twilio kan du:</span><span class="sxs-lookup"><span data-stu-id="382a9-105">With Twilio, you can:</span></span>

* <span data-ttu-id="382a9-106">Skapa ditt företag flödet baserat på hello data du får från Twilio.</span><span class="sxs-lookup"><span data-stu-id="382a9-106">Build your business flow based on hello data you get from Twilio.</span></span> 
* <span data-ttu-id="382a9-107">Använd åtgärder som får ett meddelande och listmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="382a9-107">Use actions that get a message, list messages, and more.</span></span> <span data-ttu-id="382a9-108">De här åtgärderna få svar och hello utdata gör tillgängligt för andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="382a9-108">These actions get a response, and then make hello output available for other actions.</span></span> <span data-ttu-id="382a9-109">När du får ett nytt Twilio-meddelande kan du exempelvis ta meddelandet och använda den ett Service Bus-arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="382a9-109">For example, when  you get a new Twilio message, you can take this message and use it a Service Bus workflow.</span></span> 

<span data-ttu-id="382a9-110">Kom igång genom att skapa en logikapp; Se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="382a9-110">Get started by creating a logic app; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-tootwilio"></a><span data-ttu-id="382a9-111">Skapa en anslutning tooTwilio</span><span class="sxs-lookup"><span data-stu-id="382a9-111">Create a connection tooTwilio</span></span>
<span data-ttu-id="382a9-112">När du lägger till den här anslutningen tooyour logikappar ange hello följande Twilio värden:</span><span class="sxs-lookup"><span data-stu-id="382a9-112">When you add this Connector tooyour logic apps, enter hello following Twilio values:</span></span>

| <span data-ttu-id="382a9-113">Egenskap</span><span class="sxs-lookup"><span data-stu-id="382a9-113">Property</span></span> | <span data-ttu-id="382a9-114">Krävs</span><span class="sxs-lookup"><span data-stu-id="382a9-114">Required</span></span> | <span data-ttu-id="382a9-115">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="382a9-115">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="382a9-116">Konto-ID</span><span class="sxs-lookup"><span data-stu-id="382a9-116">Account ID</span></span> |<span data-ttu-id="382a9-117">Ja</span><span class="sxs-lookup"><span data-stu-id="382a9-117">Yes</span></span> |<span data-ttu-id="382a9-118">Ange ditt Twilio-konto-ID</span><span class="sxs-lookup"><span data-stu-id="382a9-118">Enter your Twilio account ID</span></span> |
| <span data-ttu-id="382a9-119">Åtkomst-Token</span><span class="sxs-lookup"><span data-stu-id="382a9-119">Access Token</span></span> |<span data-ttu-id="382a9-120">Ja</span><span class="sxs-lookup"><span data-stu-id="382a9-120">Yes</span></span> |<span data-ttu-id="382a9-121">Ange ditt Twilio-åtkomsttoken</span><span class="sxs-lookup"><span data-stu-id="382a9-121">Enter your Twilio access token</span></span> |

> [!INCLUDE [Steps toocreate a connection tooTwilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

<span data-ttu-id="382a9-122">Om du inte har en Twilio-åtkomsttoken, se [användaridentitet & åtkomsttoken](https://www.twilio.com/docs/api/chat/guides/identity).</span><span class="sxs-lookup"><span data-stu-id="382a9-122">If you don't have a Twilio access token, see [User Identity & Access Tokens](https://www.twilio.com/docs/api/chat/guides/identity).</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="382a9-123">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="382a9-123">Connector-specific details</span></span>

<span data-ttu-id="382a9-124">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/twilio/).</span><span class="sxs-lookup"><span data-stu-id="382a9-124">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/twilio/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="382a9-125">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="382a9-125">More connectors</span></span>
<span data-ttu-id="382a9-126">Gå tillbaka toohello [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="382a9-126">Go back toohello [APIs list](apis-list.md).</span></span>
