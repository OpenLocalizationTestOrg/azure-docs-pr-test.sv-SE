---
title: aaaEncode EDIFACT-meddelanden - Azure Logic Apps | Microsoft Docs
description: "Validera EDI och generera XML med EDIFACT meddelandekodare i hello Enterprise-Integrationspaket för Logikappar i Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 974ac339-d97a-4715-bc92-62d02281e900
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: b3799dbd2492adef597022d017cf28f5ceb1085c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="b7bd5-103">Koda EDIFACT-meddelanden för Logikappar i Azure med hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="b7bd5-103">Encode EDIFACT messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="b7bd5-104">Med hello koda EDIFACT-meddelande anslutningen kan du verifiera EDI och partner-specifika egenskaper, generera ett XML-dokument för varje transaktion och begär en teknisk bekräftelse eller den funktionella bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-104">With hello Encode EDIFACT message connector, you can validate EDI and partner-specific properties, generate an XML document for each transaction set, and request a Technical Acknowledgement, Functional Acknowledgment, or both.</span></span>
<span data-ttu-id="b7bd5-105">toouse den här kopplingen måste du lägga till hello connector tooan befintliga utlösaren i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-105">toouse this connector, you must add hello connector tooan existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="b7bd5-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="b7bd5-106">Before you start</span></span>

<span data-ttu-id="b7bd5-107">Här är hello-objekt som du behöver:</span><span class="sxs-lookup"><span data-stu-id="b7bd5-107">Here's hello items you need:</span></span>

* <span data-ttu-id="b7bd5-108">Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="b7bd5-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="b7bd5-109">En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="b7bd5-110">Du måste ha en integration konto toouse hello koda EDIFACT meddelande-koppling.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-110">You must have an integration account toouse hello Encode EDIFACT message connector.</span></span> 
* <span data-ttu-id="b7bd5-111">Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="b7bd5-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="b7bd5-112">En [EDIFACT-avtal](logic-apps-enterprise-integration-edifact.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="b7bd5-112">An [EDIFACT agreement](logic-apps-enterprise-integration-edifact.md) that's already defined in your integration account</span></span>

## <a name="encode-edifact-messages"></a><span data-ttu-id="b7bd5-113">Koda EDIFACT-meddelanden</span><span class="sxs-lookup"><span data-stu-id="b7bd5-113">Encode EDIFACT messages</span></span>

1. <span data-ttu-id="b7bd5-114">[Skapa en logikapp](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="b7bd5-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="b7bd5-115">hello koda EDIFACT meddelandet connector har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-115">hello Encode EDIFACT message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="b7bd5-116">Lägg till en utlösare hello logik App Designer, och sedan lägga till en åtgärd tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="b7bd5-117">Ange ”EDIFACT” i sökrutan hello som filter.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-117">In hello search box, enter "EDIFACT" as your filter.</span></span> <span data-ttu-id="b7bd5-118">Välj antingen **koda EDIFACT-meddelandet genom avtalsnamn** eller **koda tooEDIFACT meddelandet genom identiteter**.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-118">Select either **Encode EDIFACT Message by agreement name** or **Encode tooEDIFACT message by identities**.</span></span>
   
    ![Sök EDIFACT](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. <span data-ttu-id="b7bd5-120">Om du tidigare inte har skapat alla anslutningar tooyour integrering konto uppmanas du toocreate anslutningen nu.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="b7bd5-121">Namnge din anslutning och välj hello integration konto som du vill tooconnect.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-121">Name your connection, and select hello integration account that you want tooconnect.</span></span>

    ![Skapa integration konto anslutning](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    <span data-ttu-id="b7bd5-123">Egenskaper med en asterisk krävs.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="b7bd5-124">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b7bd5-124">Property</span></span> | <span data-ttu-id="b7bd5-125">Information</span><span class="sxs-lookup"><span data-stu-id="b7bd5-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="b7bd5-126">Anslutningsnamn *</span><span class="sxs-lookup"><span data-stu-id="b7bd5-126">Connection Name *</span></span> |<span data-ttu-id="b7bd5-127">Ange ett namn för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="b7bd5-128">Integration konto *</span><span class="sxs-lookup"><span data-stu-id="b7bd5-128">Integration Account *</span></span> |<span data-ttu-id="b7bd5-129">Ange ett namn för ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-129">Enter a name for your integration account.</span></span> <span data-ttu-id="b7bd5-130">Se till att appen integration konto och logik finns i hello samma Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="b7bd5-131">När du är klar din innehållet bör se ut liknande toothis exempel.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="b7bd5-132">toofinish skapa din anslutning och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-132">toofinish creating your connection, choose **Create**.</span></span>

    ![Integration kontoinformation för anslutning](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    <span data-ttu-id="b7bd5-134">Anslutningen har skapats.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-134">Your connection is now created.</span></span>

    ![Integration konto anslutning som skapats](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a><span data-ttu-id="b7bd5-136">Koda EDIFACT-meddelande med avtalsnamn</span><span class="sxs-lookup"><span data-stu-id="b7bd5-136">Encode EDIFACT Message by agreement name</span></span>

<span data-ttu-id="b7bd5-137">Om du väljer tooencode EDIFACT meddelanden avtalsnamn, öppna hello **namn EDIFACT-avtal** listan, ange eller välj namnet på din EDIFACT-avtal.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-137">If you chose tooencode EDIFACT messages by agreement name, open hello **Name of EDIFACT agreement** list, enter or select your EDIFACT agreement name.</span></span> <span data-ttu-id="b7bd5-138">Ange hello XML-meddelande tooencode.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-138">Enter hello XML message tooencode.</span></span>

![Ange EDIFACT avtalets namn och XML-meddelandet tooencode](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a><span data-ttu-id="b7bd5-140">Koda EDIFACT-meddelande med identiteter</span><span class="sxs-lookup"><span data-stu-id="b7bd5-140">Encode EDIFACT Message by identities</span></span>

<span data-ttu-id="b7bd5-141">Om du väljer tooencode EDIFACT meddelanden identiteter, ange hello ingen avsändaridentifierare, avsändaren kvalificerare, mottagaren identifierare och mottagaren kvalificerare som konfigurerats i EDIFACT-avtal.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-141">If you choose tooencode EDIFACT messages by identities, enter hello sender identifier, sender qualifier, receiver identifier, and receiver qualifier as configured in your EDIFACT agreement.</span></span> <span data-ttu-id="b7bd5-142">Välj hello XML-meddelande tooencode.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-142">Select hello XML message tooencode.</span></span>

![Ange identiteter för avsändare och mottagare, Välj XML-meddelandet tooencode](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a><span data-ttu-id="b7bd5-144">EDIFACT koda information</span><span class="sxs-lookup"><span data-stu-id="b7bd5-144">EDIFACT Encode details</span></span>

<span data-ttu-id="b7bd5-145">hello koda EDIFACT connector utför dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="b7bd5-145">hello Encode EDIFACT connector performs these tasks:</span></span> 

* <span data-ttu-id="b7bd5-146">Lös hello avtalet genom att matcha hello avsändaren kvalificerare & identifierare och mottagaren kvalificerare och identifierare</span><span class="sxs-lookup"><span data-stu-id="b7bd5-146">Resolve hello agreement by matching hello sender qualifier & identifier and receiver qualifier and identifier</span></span>
* <span data-ttu-id="b7bd5-147">Serialiserar hello EDI interchange, konvertera XML-kodade meddelanden till EDI transaktion uppsättningar i hello utbyte.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-147">Serializes hello EDI interchange, converting XML-encoded messages into EDI transaction sets in hello interchange.</span></span>
* <span data-ttu-id="b7bd5-148">Gäller transaktion set-huvudet och trailern segment</span><span class="sxs-lookup"><span data-stu-id="b7bd5-148">Applies transaction set header and trailer segments</span></span>
* <span data-ttu-id="b7bd5-149">Genererar ett utbyte kontrollen Antal, gruppnumret kontroll och en uppsättning kontrollen transaktionsnumret för varje utgående utbyte</span><span class="sxs-lookup"><span data-stu-id="b7bd5-149">Generates an interchange control number, a group control number, and a transaction set control number for each outgoing interchange</span></span>
* <span data-ttu-id="b7bd5-150">Ersätter avgränsare i hello nyttolasten</span><span class="sxs-lookup"><span data-stu-id="b7bd5-150">Replaces separators in hello payload data</span></span>
* <span data-ttu-id="b7bd5-151">Verifierar EDI och partner-specifika egenskaper</span><span class="sxs-lookup"><span data-stu-id="b7bd5-151">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="b7bd5-152">Schemavalideringen av hello transaktion set dataelement mot hello Meddelandeschema.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-152">Schema validation of hello transaction-set data elements against hello message schema.</span></span>
  * <span data-ttu-id="b7bd5-153">EDI-verifiering utförs på dataelement för transaktionen uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-153">EDI validation performed on transaction-set data elements.</span></span>
  * <span data-ttu-id="b7bd5-154">Utökad verifiering utförs på transaktion set dataelement</span><span class="sxs-lookup"><span data-stu-id="b7bd5-154">Extended validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="b7bd5-155">Genererar XML-dokument för varje transaktion.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-155">Generates an XML document for each transaction set.</span></span>
* <span data-ttu-id="b7bd5-156">Begär en tekniska och/eller funktionella bekräftelse (om konfigurerad).</span><span class="sxs-lookup"><span data-stu-id="b7bd5-156">Requests a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="b7bd5-157">Som en teknisk bekräftelse anger hälsningsmeddelande CONTRL mottagit ett utbyte.</span><span class="sxs-lookup"><span data-stu-id="b7bd5-157">As a technical acknowledgment, hello CONTRL message indicates receipt of an interchange.</span></span>
  * <span data-ttu-id="b7bd5-158">Som en funktionell bekräftelse anger CONTRL hälsningsmeddelande godkännande eller underkännande hello emot interchange, grupp eller meddelandet med en lista över fel eller funktioner som inte stöds</span><span class="sxs-lookup"><span data-stu-id="b7bd5-158">As a functional acknowledgment, hello CONTRL message indicates acceptance or rejection of hello received interchange, group, or message, with a list of errors or unsupported functionality</span></span>

## <a name="view-swagger-file"></a><span data-ttu-id="b7bd5-159">Visa Swagger-fil</span><span class="sxs-lookup"><span data-stu-id="b7bd5-159">View Swagger file</span></span>
<span data-ttu-id="b7bd5-160">tooview hello Swagger information för hello EDIFACT-anslutningen finns [EDIFACT](/connectors/edifact/).</span><span class="sxs-lookup"><span data-stu-id="b7bd5-160">tooview hello Swagger details for hello EDIFACT connector, see [EDIFACT](/connectors/edifact/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7bd5-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7bd5-161">Next steps</span></span>
[<span data-ttu-id="b7bd5-162">Mer information om hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="b7bd5-162">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

