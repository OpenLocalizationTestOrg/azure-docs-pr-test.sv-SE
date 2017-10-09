---
title: aaaEncode X12 meddelanden - Azure Logic Apps | Microsoft Docs
description: "Validera EDI och konvertera XML-kodade meddelanden med X12 meddelande kodare i hello Enterprise-Integrationspaket för Logikappar i Azure"
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
ms.openlocfilehash: 785dbd2c7c82463154732921e07e3586d2307840
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encode-x12-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="3d40c-103">Koda X12 meddelanden för Logikappar i Azure med hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="3d40c-103">Encode X12 messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="3d40c-104">Med hello koda X12 meddelande-anslutningen kan du verifiera EDI och partner-specifika egenskaper, konvertera XML-kodade meddelanden till EDI transaktion uppsättningar i hello utbyte och begär en teknisk bekräftelse eller den funktionella bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="3d40c-104">With hello Encode X12 message connector, you can validate EDI and partner-specific properties, convert XML-encoded messages into EDI transaction sets in hello interchange, and request a Technical Acknowledgement, Functional Acknowledgment, or both.</span></span>
<span data-ttu-id="3d40c-105">toouse den här kopplingen måste du lägga till hello connector tooan befintliga utlösaren i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="3d40c-105">toouse this connector, you must add hello connector tooan existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="3d40c-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="3d40c-106">Before you start</span></span>

<span data-ttu-id="3d40c-107">Här är hello-objekt som du behöver:</span><span class="sxs-lookup"><span data-stu-id="3d40c-107">Here's hello items you need:</span></span>

* <span data-ttu-id="3d40c-108">Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="3d40c-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="3d40c-109">En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3d40c-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="3d40c-110">Du måste ha en integration konto toouse hello koda X12 meddelande-koppling.</span><span class="sxs-lookup"><span data-stu-id="3d40c-110">You must have an integration account toouse hello Encode X12 message connector.</span></span>
* <span data-ttu-id="3d40c-111">Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="3d40c-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="3d40c-112">En [X12 avtal](logic-apps-enterprise-integration-x12.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="3d40c-112">An [X12 agreement](logic-apps-enterprise-integration-x12.md) that's already defined in your integration account</span></span>

## <a name="encode-x12-messages"></a><span data-ttu-id="3d40c-113">Koda X12 meddelanden</span><span class="sxs-lookup"><span data-stu-id="3d40c-113">Encode X12 messages</span></span>

1. <span data-ttu-id="3d40c-114">[Skapa en logikapp](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="3d40c-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="3d40c-115">hello koda X12 meddelande connector har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran.</span><span class="sxs-lookup"><span data-stu-id="3d40c-115">hello Encode X12 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="3d40c-116">Lägg till en utlösare hello logik App Designer, och sedan lägga till en åtgärd tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="3d40c-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="3d40c-117">Ange ”x12” i sökrutan hello för filtret.</span><span class="sxs-lookup"><span data-stu-id="3d40c-117">In hello search box, enter "x12" for your filter.</span></span> <span data-ttu-id="3d40c-118">Välj antingen **X12-koda tooX12 meddelande av avtalsnamn** eller **X12-koda tooX12 meddelande av identiteter**.</span><span class="sxs-lookup"><span data-stu-id="3d40c-118">Select either **X12 - Encode tooX12 message by agreement name** or **X12 - Encode tooX12 message by identities**.</span></span>
   
    ![Sök efter ”x12”](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. <span data-ttu-id="3d40c-120">Om du tidigare inte har skapat alla anslutningar tooyour integrering konto uppmanas du toocreate anslutningen nu.</span><span class="sxs-lookup"><span data-stu-id="3d40c-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="3d40c-121">Namnge din anslutning och välj hello integration konto som du vill tooconnect.</span><span class="sxs-lookup"><span data-stu-id="3d40c-121">Name your connection, and select hello integration account that you want tooconnect.</span></span> 
   
    ![Integration konto anslutning](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    <span data-ttu-id="3d40c-123">Egenskaper med en asterisk krävs.</span><span class="sxs-lookup"><span data-stu-id="3d40c-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="3d40c-124">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3d40c-124">Property</span></span> | <span data-ttu-id="3d40c-125">Information</span><span class="sxs-lookup"><span data-stu-id="3d40c-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="3d40c-126">Anslutningsnamn *</span><span class="sxs-lookup"><span data-stu-id="3d40c-126">Connection Name *</span></span> |<span data-ttu-id="3d40c-127">Ange ett namn för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="3d40c-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="3d40c-128">Integration konto *</span><span class="sxs-lookup"><span data-stu-id="3d40c-128">Integration Account *</span></span> |<span data-ttu-id="3d40c-129">Ange ett namn för ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="3d40c-129">Enter a name for your integration account.</span></span> <span data-ttu-id="3d40c-130">Se till att appen integration konto och logik finns i hello samma Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="3d40c-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="3d40c-131">När du är klar din innehållet bör se ut liknande toothis exempel.</span><span class="sxs-lookup"><span data-stu-id="3d40c-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="3d40c-132">toofinish skapa din anslutning och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="3d40c-132">toofinish creating your connection, choose **Create**.</span></span>

    ![Integration konto anslutning som skapats](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    <span data-ttu-id="3d40c-134">Anslutningen har skapats.</span><span class="sxs-lookup"><span data-stu-id="3d40c-134">Your connection is now created.</span></span>

    ![Integration kontoinformation för anslutning](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a><span data-ttu-id="3d40c-136">Koda X12 meddelanden efter avtalsnamn</span><span class="sxs-lookup"><span data-stu-id="3d40c-136">Encode X12 messages by agreement name</span></span>

<span data-ttu-id="3d40c-137">Om du har valt tooencode X12 meddelanden av avtalsnamn öppna hello **namnet på X12 avtal** listan, ange eller välj en befintlig X12 avtal.</span><span class="sxs-lookup"><span data-stu-id="3d40c-137">If you chose tooencode X12 messages by agreement name, open hello **Name of X12 agreement** list, enter or select your existing X12 agreement.</span></span> <span data-ttu-id="3d40c-138">Ange hello XML-meddelande tooencode.</span><span class="sxs-lookup"><span data-stu-id="3d40c-138">Enter hello XML message tooencode.</span></span>

![Ange X12 avtal namn och en XML-meddelandet tooencode](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a><span data-ttu-id="3d40c-140">Koda X12 meddelanden med identiteter</span><span class="sxs-lookup"><span data-stu-id="3d40c-140">Encode X12 messages by identities</span></span>

<span data-ttu-id="3d40c-141">Om du väljer tooencode X12 meddelanden av identiteter ange hello ingen avsändaridentifierare, kvalificerare avsändaren, mottagaren identifierare och mottagaren kvalificerare som konfigurerats i din X12 avtal.</span><span class="sxs-lookup"><span data-stu-id="3d40c-141">If you choose tooencode X12 messages by identities, enter hello sender identifier, sender qualifier, receiver identifier, and receiver qualifier as configured in your X12 agreement.</span></span> <span data-ttu-id="3d40c-142">Välj hello XML-meddelande tooencode.</span><span class="sxs-lookup"><span data-stu-id="3d40c-142">Select hello XML message tooencode.</span></span>
   
![Ange identiteter för avsändare och mottagare, Välj XML-meddelandet tooencode](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a><span data-ttu-id="3d40c-144">X12 koda information</span><span class="sxs-lookup"><span data-stu-id="3d40c-144">X12 Encode details</span></span>

<span data-ttu-id="3d40c-145">Hej X12 koda connector utför dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="3d40c-145">hello X12 Encode connector performs these tasks:</span></span>

* <span data-ttu-id="3d40c-146">Avtalet lösning genom att matcha avsändare och mottagare kontextegenskaper.</span><span class="sxs-lookup"><span data-stu-id="3d40c-146">Agreement resolution by matching sender and receiver context properties.</span></span>
* <span data-ttu-id="3d40c-147">Serialiserar hello EDI interchange, konvertera XML-kodade meddelanden till EDI transaktion uppsättningar i hello utbyte.</span><span class="sxs-lookup"><span data-stu-id="3d40c-147">Serializes hello EDI interchange, converting XML-encoded messages into EDI transaction sets in hello interchange.</span></span>
* <span data-ttu-id="3d40c-148">Gäller transaktion set-huvudet och trailern segment</span><span class="sxs-lookup"><span data-stu-id="3d40c-148">Applies transaction set header and trailer segments</span></span>
* <span data-ttu-id="3d40c-149">Genererar ett utbyte kontrollen Antal, gruppnumret kontroll och en uppsättning kontrollen transaktionsnumret för varje utgående utbyte</span><span class="sxs-lookup"><span data-stu-id="3d40c-149">Generates an interchange control number, a group control number, and a transaction set control number for each outgoing interchange</span></span>
* <span data-ttu-id="3d40c-150">Ersätter avgränsare i hello nyttolasten</span><span class="sxs-lookup"><span data-stu-id="3d40c-150">Replaces separators in hello payload data</span></span>
* <span data-ttu-id="3d40c-151">Verifierar EDI och partner-specifika egenskaper</span><span class="sxs-lookup"><span data-stu-id="3d40c-151">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="3d40c-152">Schemavalideringen hello transaktion set dataelement mot hello-meddelande schemat för</span><span class="sxs-lookup"><span data-stu-id="3d40c-152">Schema validation of hello transaction-set data elements against hello message Schema</span></span>
  * <span data-ttu-id="3d40c-153">EDI-verifiering utförs på dataelement för transaktionen uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="3d40c-153">EDI validation performed on transaction-set data elements.</span></span>
  * <span data-ttu-id="3d40c-154">Utökad verifiering utförs på transaktion set dataelement</span><span class="sxs-lookup"><span data-stu-id="3d40c-154">Extended validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="3d40c-155">Begär en tekniska och/eller funktionella bekräftelse (om konfigurerad).</span><span class="sxs-lookup"><span data-stu-id="3d40c-155">Requests a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="3d40c-156">En teknisk bekräftelse genererar på grund av huvud-verifiering.</span><span class="sxs-lookup"><span data-stu-id="3d40c-156">A Technical Acknowledgment generates as a result of header validation.</span></span> <span data-ttu-id="3d40c-157">hello tekniska bekräftelse rapporter hello status med hello bearbetning av ett utbyte huvudet och trailern av hello adress mottagare</span><span class="sxs-lookup"><span data-stu-id="3d40c-157">hello technical acknowledgment reports hello status of hello processing of an interchange header and trailer by hello address receiver</span></span>
  * <span data-ttu-id="3d40c-158">En funktionell bekräftelse genererar på grund av brödtext validering.</span><span class="sxs-lookup"><span data-stu-id="3d40c-158">A Functional Acknowledgment generates as a result of body validation.</span></span> <span data-ttu-id="3d40c-159">hello rapporterar funktionella bekräftelse varje fel uppstod under bearbetning av hello emot dokumentet</span><span class="sxs-lookup"><span data-stu-id="3d40c-159">hello functional acknowledgment reports each error encountered while processing hello received document</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="3d40c-160">Visa hello swagger</span><span class="sxs-lookup"><span data-stu-id="3d40c-160">View hello swagger</span></span>
<span data-ttu-id="3d40c-161">Se hello [swagger information](/connectors/x12/).</span><span class="sxs-lookup"><span data-stu-id="3d40c-161">See hello [swagger details](/connectors/x12/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3d40c-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3d40c-162">Next steps</span></span>
[<span data-ttu-id="3d40c-163">Mer information om hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="3d40c-163">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

