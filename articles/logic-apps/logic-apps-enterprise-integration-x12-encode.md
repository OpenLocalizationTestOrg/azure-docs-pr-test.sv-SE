---
title: Koda X12 meddelanden - Azure Logic Apps | Microsoft Docs
description: "Validera EDI och konvertera XML-kodade meddelanden med X12 meddelande kodare i Enterprise-Integrationspaket för Logikappar i Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: a01e9ca9-816b-479e-ab11-4a984f10f62d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 29d19364b9a98e351c95f13e68a2e63b9f6439f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="encode-x12-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="bcaf8-103">Koda X12 meddelanden för Azure Logikappar med Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="bcaf8-103">Encode X12 messages for Azure Logic Apps with the Enterprise Integration Pack</span></span>

<span data-ttu-id="bcaf8-104">Med koda X12 meddelande-anslutningen kan du verifiera EDI och partner-specifika egenskaper, konvertera XML-kodade meddelanden till uppsättningar för EDI-transaktion i utbyte och begär en teknisk bekräftelse eller den funktionella bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-104">With the Encode X12 message connector, you can validate EDI and partner-specific properties, convert XML-encoded messages into EDI transaction sets in the interchange, and request a Technical Acknowledgement, Functional Acknowledgment, or both.</span></span>
<span data-ttu-id="bcaf8-105">Om du vill använda denna anslutning måste du lägga till anslutningen till en befintlig utlösare i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-105">To use this connector, you must add the connector to an existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="bcaf8-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="bcaf8-106">Before you start</span></span>

<span data-ttu-id="bcaf8-107">Här är de objekt som du behöver:</span><span class="sxs-lookup"><span data-stu-id="bcaf8-107">Here's the items you need:</span></span>

* <span data-ttu-id="bcaf8-108">Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="bcaf8-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="bcaf8-109">En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="bcaf8-110">Du måste ha ett integration konto att använda anslutningstjänsten koda X12 meddelande.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-110">You must have an integration account to use the Encode X12 message connector.</span></span>
* <span data-ttu-id="bcaf8-111">Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="bcaf8-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="bcaf8-112">En [X12 avtal](logic-apps-enterprise-integration-x12.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="bcaf8-112">An [X12 agreement](logic-apps-enterprise-integration-x12.md) that's already defined in your integration account</span></span>

## <a name="encode-x12-messages"></a><span data-ttu-id="bcaf8-113">Koda X12 meddelanden</span><span class="sxs-lookup"><span data-stu-id="bcaf8-113">Encode X12 messages</span></span>

1. <span data-ttu-id="bcaf8-114">[Skapa en logikapp](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="bcaf8-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="bcaf8-115">Koda X12 meddelande kopplingen har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-115">The Encode X12 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="bcaf8-116">Lägg till en utlösare i logik App Designer och sedan lägga till en åtgärd i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3.  <span data-ttu-id="bcaf8-117">I sökrutan anger du ”x12” för filtret.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-117">In the search box, enter "x12" for your filter.</span></span> <span data-ttu-id="bcaf8-118">Välj antingen **X12-koda till X12 meddelandet genom avtalsnamn** eller **X12-koda till X12 meddelandet genom identiteter**.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-118">Select either **X12 - Encode to X12 message by agreement name** or **X12 - Encode to X12 message by identities**.</span></span>
   
    ![Sök efter ”x12”](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. <span data-ttu-id="bcaf8-120">Om du inte tidigare skapade alla anslutningar till ditt konto integration, uppmanas du att skapa anslutningen nu.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="bcaf8-121">Namnge din anslutning och välj integration kontot som du vill ansluta till.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-121">Name your connection, and select the integration account that you want to connect.</span></span> 
   
    ![Integration konto anslutning](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    <span data-ttu-id="bcaf8-123">Egenskaper med en asterisk krävs.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="bcaf8-124">Egenskap</span><span class="sxs-lookup"><span data-stu-id="bcaf8-124">Property</span></span> | <span data-ttu-id="bcaf8-125">Information</span><span class="sxs-lookup"><span data-stu-id="bcaf8-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="bcaf8-126">Anslutningsnamn *</span><span class="sxs-lookup"><span data-stu-id="bcaf8-126">Connection Name *</span></span> |<span data-ttu-id="bcaf8-127">Ange ett namn för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="bcaf8-128">Integration konto *</span><span class="sxs-lookup"><span data-stu-id="bcaf8-128">Integration Account *</span></span> |<span data-ttu-id="bcaf8-129">Ange ett namn för ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-129">Enter a name for your integration account.</span></span> <span data-ttu-id="bcaf8-130">Se till att appen integration konto och logik finns i samma Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

5.  <span data-ttu-id="bcaf8-131">När du är klar bör din anslutningsinformation likna exemplet.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-131">When you're done, your connection details should look similar to this example.</span></span> <span data-ttu-id="bcaf8-132">Slutför din anslutning väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-132">To finish creating your connection, choose **Create**.</span></span>

    ![Integration konto anslutning som skapats](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    <span data-ttu-id="bcaf8-134">Anslutningen har skapats.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-134">Your connection is now created.</span></span>

    ![Integration kontoinformation för anslutning](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a><span data-ttu-id="bcaf8-136">Koda X12 meddelanden efter avtalsnamn</span><span class="sxs-lookup"><span data-stu-id="bcaf8-136">Encode X12 messages by agreement name</span></span>

<span data-ttu-id="bcaf8-137">Om du väljer att koda X12 meddelanden efter avtalsnamn, öppna den **namnet på X12 avtal** listan, ange eller välj en befintlig X12 avtal.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-137">If you chose to encode X12 messages by agreement name, open the **Name of X12 agreement** list, enter or select your existing X12 agreement.</span></span> <span data-ttu-id="bcaf8-138">Ange den XML-meddelanden om att koda.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-138">Enter the XML message to encode.</span></span>

![Ange X12 avtalets namn och XML-meddelande att koda](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a><span data-ttu-id="bcaf8-140">Koda X12 meddelanden med identiteter</span><span class="sxs-lookup"><span data-stu-id="bcaf8-140">Encode X12 messages by identities</span></span>

<span data-ttu-id="bcaf8-141">Om du väljer att koda X12 meddelanden med identiteter, ange ingen avsändaridentifierare, kvalificerare avsändaren, mottagaren identifierare och mottagaren kvalificeraren som konfigurerats i din X12 avtal.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-141">If you choose to encode X12 messages by identities, enter the sender identifier, sender qualifier, receiver identifier, and receiver qualifier as configured in your X12 agreement.</span></span> <span data-ttu-id="bcaf8-142">Välj XML-meddelandet för att koda.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-142">Select the XML message to encode.</span></span>
   
![Ange identiteter för avsändare och mottagare, Välj XML-meddelande att koda](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a><span data-ttu-id="bcaf8-144">X12 koda information</span><span class="sxs-lookup"><span data-stu-id="bcaf8-144">X12 Encode details</span></span>

<span data-ttu-id="bcaf8-145">X12 koda connector utför dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="bcaf8-145">The X12 Encode connector performs these tasks:</span></span>

* <span data-ttu-id="bcaf8-146">Avtalet lösning genom att matcha avsändare och mottagare kontextegenskaper.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-146">Agreement resolution by matching sender and receiver context properties.</span></span>
* <span data-ttu-id="bcaf8-147">Serialiserar EDI-utbyte, konvertera XML-kodade meddelanden till uppsättningar för EDI-transaktion i utbyte.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-147">Serializes the EDI interchange, converting XML-encoded messages into EDI transaction sets in the interchange.</span></span>
* <span data-ttu-id="bcaf8-148">Gäller transaktion set-huvudet och trailern segment</span><span class="sxs-lookup"><span data-stu-id="bcaf8-148">Applies transaction set header and trailer segments</span></span>
* <span data-ttu-id="bcaf8-149">Genererar ett utbyte kontrollen Antal, gruppnumret kontroll och en uppsättning kontrollen transaktionsnumret för varje utgående utbyte</span><span class="sxs-lookup"><span data-stu-id="bcaf8-149">Generates an interchange control number, a group control number, and a transaction set control number for each outgoing interchange</span></span>
* <span data-ttu-id="bcaf8-150">Ersätter avgränsare i nyttolasten</span><span class="sxs-lookup"><span data-stu-id="bcaf8-150">Replaces separators in the payload data</span></span>
* <span data-ttu-id="bcaf8-151">Verifierar EDI och partner-specifika egenskaper</span><span class="sxs-lookup"><span data-stu-id="bcaf8-151">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="bcaf8-152">Schemavalideringen av transaktionen set dataelement mot meddelandet Schema</span><span class="sxs-lookup"><span data-stu-id="bcaf8-152">Schema validation of the transaction-set data elements against the message Schema</span></span>
  * <span data-ttu-id="bcaf8-153">EDI-verifiering utförs på dataelement för transaktionen uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-153">EDI validation performed on transaction-set data elements.</span></span>
  * <span data-ttu-id="bcaf8-154">Utökad verifiering utförs på transaktion set dataelement</span><span class="sxs-lookup"><span data-stu-id="bcaf8-154">Extended validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="bcaf8-155">Begär en tekniska och/eller funktionella bekräftelse (om konfigurerad).</span><span class="sxs-lookup"><span data-stu-id="bcaf8-155">Requests a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="bcaf8-156">En teknisk bekräftelse genererar på grund av huvud-verifiering.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-156">A Technical Acknowledgment generates as a result of header validation.</span></span> <span data-ttu-id="bcaf8-157">Tekniska bekräftelsen rapporterar status för bearbetning av ett utbyte huvudet och trailern av adress-mottagare</span><span class="sxs-lookup"><span data-stu-id="bcaf8-157">The technical acknowledgment reports the status of the processing of an interchange header and trailer by the address receiver</span></span>
  * <span data-ttu-id="bcaf8-158">En funktionell bekräftelse genererar på grund av brödtext validering.</span><span class="sxs-lookup"><span data-stu-id="bcaf8-158">A Functional Acknowledgment generates as a result of body validation.</span></span> <span data-ttu-id="bcaf8-159">Funktionella bekräftelsen rapporterar varje fel uppstod under bearbetning av Mottaget dokument</span><span class="sxs-lookup"><span data-stu-id="bcaf8-159">The functional acknowledgment reports each error encountered while processing the received document</span></span>

## <a name="view-the-swagger"></a><span data-ttu-id="bcaf8-160">Visa swagger</span><span class="sxs-lookup"><span data-stu-id="bcaf8-160">View the swagger</span></span>
<span data-ttu-id="bcaf8-161">Finns det [swagger information](/connectors/x12/).</span><span class="sxs-lookup"><span data-stu-id="bcaf8-161">See the [swagger details](/connectors/x12/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="bcaf8-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bcaf8-162">Next steps</span></span>
[<span data-ttu-id="bcaf8-163">Mer information om Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="bcaf8-163">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

