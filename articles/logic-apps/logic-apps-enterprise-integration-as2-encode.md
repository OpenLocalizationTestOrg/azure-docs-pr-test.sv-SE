---
title: aaaEncode AS2 - meddelanden i Azure Logic Apps | Microsoft Docs
description: "Hur toouse hello AS2-kodaren i hello Enterprise-Integrationspaket för Logikappar i Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 332fb9e3-576c-4683-bd10-d177a0ebe9a3
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 2b372c416512ffa9ea5dc50ce0f767bfd8aefbc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encode-as2-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="b6b45-103">Koda AS2-meddelanden för Logikappar i Azure med hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="b6b45-103">Encode AS2 messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="b6b45-104">tooestablish säkerheten och pålitligheten vid överföring av meddelanden, Använd hello koda AS2-koppling för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="b6b45-104">tooestablish security and reliability while transmitting messages, use hello Encode AS2 message connector.</span></span> <span data-ttu-id="b6b45-105">Den här anslutningen ger digital signering, kryptering och bekräftelser via meddelandet Disposition meddelanden (MDN), vilket leder också toosupport för icke-Repudiation.</span><span class="sxs-lookup"><span data-stu-id="b6b45-105">This connector provides digital signing, encryption, and acknowledgements through Message Disposition Notifications (MDN), which also leads toosupport for Non-Repudiation.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="b6b45-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="b6b45-106">Before you start</span></span>

<span data-ttu-id="b6b45-107">Här är hello-objekt som du behöver:</span><span class="sxs-lookup"><span data-stu-id="b6b45-107">Here's hello items you need:</span></span>

* <span data-ttu-id="b6b45-108">Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="b6b45-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="b6b45-109">En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b6b45-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="b6b45-110">Du måste ha en integration konto toouse hello koda AS2 meddelande-koppling.</span><span class="sxs-lookup"><span data-stu-id="b6b45-110">You must have an integration account toouse hello Encode AS2 message connector.</span></span>
* <span data-ttu-id="b6b45-111">Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="b6b45-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="b6b45-112">En [AS2-avtal](logic-apps-enterprise-integration-as2.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="b6b45-112">An [AS2 agreement](logic-apps-enterprise-integration-as2.md) that's already defined in your integration account</span></span>

## <a name="encode-as2-messages"></a><span data-ttu-id="b6b45-113">Koda AS2-meddelanden</span><span class="sxs-lookup"><span data-stu-id="b6b45-113">Encode AS2 messages</span></span>

1. <span data-ttu-id="b6b45-114">[Skapa en logikapp](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="b6b45-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="b6b45-115">hello koda AS2-koppling för meddelandet har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran.</span><span class="sxs-lookup"><span data-stu-id="b6b45-115">hello Encode AS2 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="b6b45-116">Lägg till en utlösare hello logik App Designer, och sedan lägga till en åtgärd tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="b6b45-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="b6b45-117">Ange ”AS2” i sökrutan hello för filtret.</span><span class="sxs-lookup"><span data-stu-id="b6b45-117">In hello search box, enter "AS2" for your filter.</span></span> <span data-ttu-id="b6b45-118">Välj **AS2 - koda AS2-meddelandet**.</span><span class="sxs-lookup"><span data-stu-id="b6b45-118">Select **AS2 - Encode AS2 message**.</span></span>
   
    ![Sök efter ”AS2”](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. <span data-ttu-id="b6b45-120">Om du tidigare inte har skapat alla anslutningar tooyour integrering konto uppmanas du toocreate anslutningen nu.</span><span class="sxs-lookup"><span data-stu-id="b6b45-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="b6b45-121">Namnge din anslutning och välj hello integration konto som du vill tooconnect.</span><span class="sxs-lookup"><span data-stu-id="b6b45-121">Name your connection, and select hello integration account that you want tooconnect.</span></span> 
   
    ![Skapa toointegration anslutningskonto](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    <span data-ttu-id="b6b45-123">Egenskaper med en asterisk krävs.</span><span class="sxs-lookup"><span data-stu-id="b6b45-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="b6b45-124">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b6b45-124">Property</span></span> | <span data-ttu-id="b6b45-125">Information</span><span class="sxs-lookup"><span data-stu-id="b6b45-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="b6b45-126">Anslutningsnamn *</span><span class="sxs-lookup"><span data-stu-id="b6b45-126">Connection Name *</span></span> |<span data-ttu-id="b6b45-127">Ange ett namn för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="b6b45-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="b6b45-128">Integration konto *</span><span class="sxs-lookup"><span data-stu-id="b6b45-128">Integration Account *</span></span> |<span data-ttu-id="b6b45-129">Ange ett namn för ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="b6b45-129">Enter a name for your integration account.</span></span> <span data-ttu-id="b6b45-130">Se till att appen integration konto och logik finns i hello samma Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="b6b45-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="b6b45-131">När du är klar din innehållet bör se ut liknande toothis exempel.</span><span class="sxs-lookup"><span data-stu-id="b6b45-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="b6b45-132">toofinish skapa din anslutning och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b6b45-132">toofinish creating your connection, choose **Create**.</span></span>
   
    ![Integration anslutningsinformation](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. <span data-ttu-id="b6b45-134">När anslutningen har skapats, som visas i det här exemplet, ange information för **AS2-från**, **AS2-tooidentifiers** som konfigurerats i ditt avtal och **brödtext**, vilket är nyttolast för hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="b6b45-134">After your connection is created, as shown in this example, provide details for **AS2-From**, **AS2-tooidentifiers** as configured in your agreement, and **Body**, which is hello message payload.</span></span>
   
    ![Ange obligatoriska fält](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a><span data-ttu-id="b6b45-136">AS2-kodaren information</span><span class="sxs-lookup"><span data-stu-id="b6b45-136">AS2 encoder details</span></span>

<span data-ttu-id="b6b45-137">hello koda AS2-koppling utför dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="b6b45-137">hello Encode AS2 connector performs these tasks:</span></span> 

* <span data-ttu-id="b6b45-138">Gäller AS2/HTTP-huvuden</span><span class="sxs-lookup"><span data-stu-id="b6b45-138">Applies AS2/HTTP headers</span></span>
* <span data-ttu-id="b6b45-139">Loggar utgående meddelanden (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="b6b45-139">Signs outgoing messages (if configured)</span></span>
* <span data-ttu-id="b6b45-140">Krypterar utgående meddelanden (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="b6b45-140">Encrypts outgoing messages (if configured)</span></span>
* <span data-ttu-id="b6b45-141">Komprimerar hello-meddelande (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="b6b45-141">Compresses hello message (if configured)</span></span>

## <a name="try-this-sample"></a><span data-ttu-id="b6b45-142">Försök med det här exemplet</span><span class="sxs-lookup"><span data-stu-id="b6b45-142">Try this sample</span></span>

<span data-ttu-id="b6b45-143">distribuera ett fullt fungerande logik appen och exempel AS2-scenario tootry finns hello [AS2 logik appmallen och scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span><span class="sxs-lookup"><span data-stu-id="b6b45-143">tootry deploying a fully operational logic app and sample AS2 scenario, see hello [AS2 logic app template and scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="b6b45-144">Visa hello swagger</span><span class="sxs-lookup"><span data-stu-id="b6b45-144">View hello swagger</span></span>
<span data-ttu-id="b6b45-145">Se hello [swagger information](/connectors/as2/).</span><span class="sxs-lookup"><span data-stu-id="b6b45-145">See hello [swagger details](/connectors/as2/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b6b45-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b6b45-146">Next steps</span></span>
[<span data-ttu-id="b6b45-147">Mer information om hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="b6b45-147">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

