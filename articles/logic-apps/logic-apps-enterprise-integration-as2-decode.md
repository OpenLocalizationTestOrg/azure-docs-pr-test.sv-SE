---
title: Avkoda AS2 - meddelanden i Azure Logic Apps | Microsoft Docs
description: "Hur du använder AS2-avkodarens i Enterprise-Integrationspaket för Logikappar i Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: a7920b2509fe368c6f7d55e17fe0bf0020c4562c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="decode-as2-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="3e556-103">Avkoda AS2-meddelanden för Azure Logikappar med Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="3e556-103">Decode AS2 messages for Azure Logic Apps with the Enterprise Integration Pack</span></span> 

<span data-ttu-id="3e556-104">Använd avkoda AS2 meddelandet kopplingen för att upprätta säkerheten och pålitligheten vid överföring av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3e556-104">To establish security and reliability while transmitting messages, use the Decode AS2 message connector.</span></span> <span data-ttu-id="3e556-105">Den här anslutningen ger digital signering dekryptering och bekräftelser via meddelandet Disposition meddelanden (MDN).</span><span class="sxs-lookup"><span data-stu-id="3e556-105">This connector provides digital signing, decryption, and acknowledgements through Message Disposition Notifications (MDN).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="3e556-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="3e556-106">Before you start</span></span>

<span data-ttu-id="3e556-107">Här är de objekt som du behöver:</span><span class="sxs-lookup"><span data-stu-id="3e556-107">Here's the items you need:</span></span>

* <span data-ttu-id="3e556-108">Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="3e556-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="3e556-109">En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3e556-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="3e556-110">Du måste ha ett integration konto att använda anslutningstjänsten AS2 avkoda meddelandet.</span><span class="sxs-lookup"><span data-stu-id="3e556-110">You must have an integration account to use the Decode AS2 message connector.</span></span>
* <span data-ttu-id="3e556-111">Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="3e556-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="3e556-112">En [AS2-avtal](logic-apps-enterprise-integration-as2.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="3e556-112">An [AS2 agreement](logic-apps-enterprise-integration-as2.md) that's already defined in your integration account</span></span>

## <a name="decode-as2-messages"></a><span data-ttu-id="3e556-113">Avkoda AS2-meddelanden</span><span class="sxs-lookup"><span data-stu-id="3e556-113">Decode AS2 messages</span></span>

1. <span data-ttu-id="3e556-114">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="3e556-114">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="3e556-115">Avkoda AS2 meddelande-koppling har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran.</span><span class="sxs-lookup"><span data-stu-id="3e556-115">The Decode AS2 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="3e556-116">Lägg till en utlösare i logik App Designer och sedan lägga till en åtgärd i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="3e556-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3.  <span data-ttu-id="3e556-117">I sökrutan anger du ”AS2” för filtret.</span><span class="sxs-lookup"><span data-stu-id="3e556-117">In the search box, enter "AS2" for your filter.</span></span> <span data-ttu-id="3e556-118">Välj **AS2 - avkoda AS2-meddelandet**.</span><span class="sxs-lookup"><span data-stu-id="3e556-118">Select **AS2 - Decode AS2 message**.</span></span>
   
    ![Sök efter ”AS2”](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)

4. <span data-ttu-id="3e556-120">Om du inte tidigare skapade alla anslutningar till ditt konto integration, uppmanas du att skapa anslutningen nu.</span><span class="sxs-lookup"><span data-stu-id="3e556-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="3e556-121">Namnge din anslutning och välj integration kontot som du vill ansluta till.</span><span class="sxs-lookup"><span data-stu-id="3e556-121">Name your connection, and select the integration account that you want to connect.</span></span>
   
    ![Skapa integration anslutning](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)

    <span data-ttu-id="3e556-123">Egenskaper med en asterisk krävs.</span><span class="sxs-lookup"><span data-stu-id="3e556-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="3e556-124">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3e556-124">Property</span></span> | <span data-ttu-id="3e556-125">Information</span><span class="sxs-lookup"><span data-stu-id="3e556-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="3e556-126">Anslutningsnamn *</span><span class="sxs-lookup"><span data-stu-id="3e556-126">Connection Name *</span></span> |<span data-ttu-id="3e556-127">Ange ett namn för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="3e556-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="3e556-128">Integration konto *</span><span class="sxs-lookup"><span data-stu-id="3e556-128">Integration Account *</span></span> |<span data-ttu-id="3e556-129">Ange ett namn för ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="3e556-129">Enter a name for your integration account.</span></span> <span data-ttu-id="3e556-130">Se till att appen integration konto och logik finns i samma Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="3e556-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

5.  <span data-ttu-id="3e556-131">När du är klar bör din anslutningsinformation likna exemplet.</span><span class="sxs-lookup"><span data-stu-id="3e556-131">When you're done, your connection details should look similar to this example.</span></span> <span data-ttu-id="3e556-132">Slutför din anslutning väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="3e556-132">To finish creating your connection, choose **Create**.</span></span>

    ![Integration anslutningsinformation](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)

6. <span data-ttu-id="3e556-134">När anslutningen har skapats, som visas i det här exemplet, Välj **brödtext** och **huvuden** från utdata för begäran.</span><span class="sxs-lookup"><span data-stu-id="3e556-134">After your connection is created, as shown in this example, select **Body** and **Headers** from the Request outputs.</span></span>
   
    ![Integration anslutning som har skapats](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 

    <span data-ttu-id="3e556-136">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3e556-136">For example:</span></span>

    ![Välj brödtext och rubriker från utdata för begäran](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 

## <a name="as2-decoder-details"></a><span data-ttu-id="3e556-138">AS2 avkodarens information</span><span class="sxs-lookup"><span data-stu-id="3e556-138">AS2 decoder details</span></span>

<span data-ttu-id="3e556-139">Avkoda AS2-koppling utför dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="3e556-139">The Decode AS2 connector performs these tasks:</span></span> 

* <span data-ttu-id="3e556-140">Bearbetar AS2/HTTP-huvuden</span><span class="sxs-lookup"><span data-stu-id="3e556-140">Processes AS2/HTTP headers</span></span>
* <span data-ttu-id="3e556-141">Verifierar signaturen (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="3e556-141">Verifies the signature (if configured)</span></span>
* <span data-ttu-id="3e556-142">Dekrypterar meddelanden (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="3e556-142">Decrypts the messages (if configured)</span></span>
* <span data-ttu-id="3e556-143">Dekomprimerar meddelandet (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="3e556-143">Decompresses the message (if configured)</span></span>
* <span data-ttu-id="3e556-144">Synkroniserar en mottagna MDN med ursprungliga utgående meddelande</span><span class="sxs-lookup"><span data-stu-id="3e556-144">Reconciles a received MDN with the original outbound message</span></span>
* <span data-ttu-id="3e556-145">Uppdateringar och korrelerar poster i oavvislighet databasen</span><span class="sxs-lookup"><span data-stu-id="3e556-145">Updates and correlates records in the non-repudiation database</span></span>
* <span data-ttu-id="3e556-146">Skriver poster för AS2 statusrapportering</span><span class="sxs-lookup"><span data-stu-id="3e556-146">Writes records for AS2 status reporting</span></span>
* <span data-ttu-id="3e556-147">Innehållet i utdata-nyttolasten är base64-kodade</span><span class="sxs-lookup"><span data-stu-id="3e556-147">The output payload contents are base64 encoded</span></span>
* <span data-ttu-id="3e556-148">Anger om en MDN krävs, och om MDN ska synkron eller asynkron baserat på konfigurationen i AS2-avtal</span><span class="sxs-lookup"><span data-stu-id="3e556-148">Determines whether an MDN is required, and whether the MDN should be synchronous or asynchronous based on configuration in AS2 agreement</span></span>
* <span data-ttu-id="3e556-149">Genererar en synkron eller asynkron MDN (baserat på avtal konfigurationer)</span><span class="sxs-lookup"><span data-stu-id="3e556-149">Generates a synchronous or asynchronous MDN (based on agreement configurations)</span></span>
* <span data-ttu-id="3e556-150">Ställer in token Korrelations och egenskaper på MDN</span><span class="sxs-lookup"><span data-stu-id="3e556-150">Sets the correlation tokens and properties on the MDN</span></span>

## <a name="try-this-sample"></a><span data-ttu-id="3e556-151">Försök med det här exemplet</span><span class="sxs-lookup"><span data-stu-id="3e556-151">Try this sample</span></span>

<span data-ttu-id="3e556-152">Om du vill prova att distribuera ett fullt fungerande logik appen och exempel AS2 scenario, finns det [AS2 logik appmallen och scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span><span class="sxs-lookup"><span data-stu-id="3e556-152">To try deploying a fully operational logic app and sample AS2 scenario, see the [AS2 logic app template and scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span></span>

## <a name="view-the-swagger"></a><span data-ttu-id="3e556-153">Visa swagger</span><span class="sxs-lookup"><span data-stu-id="3e556-153">View the swagger</span></span>
<span data-ttu-id="3e556-154">Finns det [swagger information](/connectors/as2/).</span><span class="sxs-lookup"><span data-stu-id="3e556-154">See the [swagger details](/connectors/as2/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3e556-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e556-155">Next steps</span></span>
[<span data-ttu-id="3e556-156">Mer information om Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="3e556-156">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md) 

