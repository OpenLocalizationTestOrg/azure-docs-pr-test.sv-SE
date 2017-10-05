---
title: Koda AS2 - meddelanden i Azure Logic Apps | Microsoft Docs
description: "Hur du använder AS2-kodaren i Enterprise-Integrationspaket för Logikappar i Azure"
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
ms.openlocfilehash: 7889bf9e4e02143b6bb4c797531afa54f8647ce5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="encode-as2-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="c8496-103">Koda AS2-meddelanden för Azure Logikappar med Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="c8496-103">Encode AS2 messages for Azure Logic Apps with the Enterprise Integration Pack</span></span>

<span data-ttu-id="c8496-104">Använd koda AS2 meddelandet kopplingen för att upprätta säkerheten och pålitligheten vid överföring av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c8496-104">To establish security and reliability while transmitting messages, use the Encode AS2 message connector.</span></span> <span data-ttu-id="c8496-105">Den här anslutningen ger digital signering, kryptering och bekräftelser via meddelandet Disposition meddelanden (MDN), vilket leder också till stöd för icke-Repudiation.</span><span class="sxs-lookup"><span data-stu-id="c8496-105">This connector provides digital signing, encryption, and acknowledgements through Message Disposition Notifications (MDN), which also leads to support for Non-Repudiation.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="c8496-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c8496-106">Before you start</span></span>

<span data-ttu-id="c8496-107">Här är de objekt som du behöver:</span><span class="sxs-lookup"><span data-stu-id="c8496-107">Here's the items you need:</span></span>

* <span data-ttu-id="c8496-108">Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="c8496-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="c8496-109">En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c8496-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="c8496-110">Du måste ha ett integration konto att använda anslutningstjänsten AS2 koda meddelandet.</span><span class="sxs-lookup"><span data-stu-id="c8496-110">You must have an integration account to use the Encode AS2 message connector.</span></span>
* <span data-ttu-id="c8496-111">Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="c8496-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="c8496-112">En [AS2-avtal](logic-apps-enterprise-integration-as2.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="c8496-112">An [AS2 agreement](logic-apps-enterprise-integration-as2.md) that's already defined in your integration account</span></span>

## <a name="encode-as2-messages"></a><span data-ttu-id="c8496-113">Koda AS2-meddelanden</span><span class="sxs-lookup"><span data-stu-id="c8496-113">Encode AS2 messages</span></span>

1. <span data-ttu-id="c8496-114">[Skapa en logikapp](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c8496-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="c8496-115">Koda AS2 meddelande-koppling har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran.</span><span class="sxs-lookup"><span data-stu-id="c8496-115">The Encode AS2 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="c8496-116">Lägg till en utlösare i logik App Designer och sedan lägga till en åtgärd i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="c8496-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3.  <span data-ttu-id="c8496-117">I sökrutan anger du ”AS2” för filtret.</span><span class="sxs-lookup"><span data-stu-id="c8496-117">In the search box, enter "AS2" for your filter.</span></span> <span data-ttu-id="c8496-118">Välj **AS2 - koda AS2-meddelandet**.</span><span class="sxs-lookup"><span data-stu-id="c8496-118">Select **AS2 - Encode AS2 message**.</span></span>
   
    ![Sök efter ”AS2”](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. <span data-ttu-id="c8496-120">Om du inte tidigare skapade alla anslutningar till ditt konto integration, uppmanas du att skapa anslutningen nu.</span><span class="sxs-lookup"><span data-stu-id="c8496-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="c8496-121">Namnge din anslutning och välj integration kontot som du vill ansluta till.</span><span class="sxs-lookup"><span data-stu-id="c8496-121">Name your connection, and select the integration account that you want to connect.</span></span> 
   
    ![Skapa anslutning till kontot på integrering](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    <span data-ttu-id="c8496-123">Egenskaper med en asterisk krävs.</span><span class="sxs-lookup"><span data-stu-id="c8496-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="c8496-124">Egenskap</span><span class="sxs-lookup"><span data-stu-id="c8496-124">Property</span></span> | <span data-ttu-id="c8496-125">Information</span><span class="sxs-lookup"><span data-stu-id="c8496-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="c8496-126">Anslutningsnamn *</span><span class="sxs-lookup"><span data-stu-id="c8496-126">Connection Name *</span></span> |<span data-ttu-id="c8496-127">Ange ett namn för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="c8496-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="c8496-128">Integration konto *</span><span class="sxs-lookup"><span data-stu-id="c8496-128">Integration Account *</span></span> |<span data-ttu-id="c8496-129">Ange ett namn för ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="c8496-129">Enter a name for your integration account.</span></span> <span data-ttu-id="c8496-130">Se till att appen integration konto och logik finns i samma Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="c8496-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

5.  <span data-ttu-id="c8496-131">När du är klar bör din anslutningsinformation likna exemplet.</span><span class="sxs-lookup"><span data-stu-id="c8496-131">When you're done, your connection details should look similar to this example.</span></span> <span data-ttu-id="c8496-132">Slutför din anslutning väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c8496-132">To finish creating your connection, choose **Create**.</span></span>
   
    ![Integration anslutningsinformation](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. <span data-ttu-id="c8496-134">När anslutningen har skapats, som visas i det här exemplet, ange information för **AS2-från**, **AS2-att identifierare** som konfigurerats i ditt avtal och **brödtext**, vilket är nyttolast meddelande.</span><span class="sxs-lookup"><span data-stu-id="c8496-134">After your connection is created, as shown in this example, provide details for **AS2-From**, **AS2-To identifiers** as configured in your agreement, and **Body**, which is the message payload.</span></span>
   
    ![Ange obligatoriska fält](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a><span data-ttu-id="c8496-136">AS2-kodaren information</span><span class="sxs-lookup"><span data-stu-id="c8496-136">AS2 encoder details</span></span>

<span data-ttu-id="c8496-137">Koda AS2-koppling utför dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="c8496-137">The Encode AS2 connector performs these tasks:</span></span> 

* <span data-ttu-id="c8496-138">Gäller AS2/HTTP-huvuden</span><span class="sxs-lookup"><span data-stu-id="c8496-138">Applies AS2/HTTP headers</span></span>
* <span data-ttu-id="c8496-139">Loggar utgående meddelanden (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="c8496-139">Signs outgoing messages (if configured)</span></span>
* <span data-ttu-id="c8496-140">Krypterar utgående meddelanden (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="c8496-140">Encrypts outgoing messages (if configured)</span></span>
* <span data-ttu-id="c8496-141">Komprimerar meddelandet (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="c8496-141">Compresses the message (if configured)</span></span>

## <a name="try-this-sample"></a><span data-ttu-id="c8496-142">Försök med det här exemplet</span><span class="sxs-lookup"><span data-stu-id="c8496-142">Try this sample</span></span>

<span data-ttu-id="c8496-143">Om du vill prova att distribuera ett fullt fungerande logik appen och exempel AS2 scenario, finns det [AS2 logik appmallen och scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span><span class="sxs-lookup"><span data-stu-id="c8496-143">To try deploying a fully operational logic app and sample AS2 scenario, see the [AS2 logic app template and scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span></span>

## <a name="view-the-swagger"></a><span data-ttu-id="c8496-144">Visa swagger</span><span class="sxs-lookup"><span data-stu-id="c8496-144">View the swagger</span></span>
<span data-ttu-id="c8496-145">Finns det [swagger information](/connectors/as2/).</span><span class="sxs-lookup"><span data-stu-id="c8496-145">See the [swagger details](/connectors/as2/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="c8496-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c8496-146">Next steps</span></span>
[<span data-ttu-id="c8496-147">Mer information om Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="c8496-147">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

