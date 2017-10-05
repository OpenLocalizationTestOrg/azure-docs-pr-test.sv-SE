---
title: Koda meddelanden med EDIFACT - Azure Logic Apps | Microsoft Docs
description: "Validera EDI och generera XML med EDIFACT meddelandekodare i Enterprise-Integrationspaket för Logikappar i Azure"
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
ms.openlocfilehash: b8d577326d23ec45cb4a9ec0e450ebf7afd945f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="95d10-103">Koda EDIFACT-meddelanden för Azure Logikappar med Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="95d10-103">Encode EDIFACT messages for Azure Logic Apps with the Enterprise Integration Pack</span></span>

<span data-ttu-id="95d10-104">Med meddelandet koda EDIFACT-anslutningen kan du verifiera EDI och partner-specifika egenskaper, generera ett XML-dokument för varje transaktion och begär en teknisk bekräftelse eller den funktionella bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="95d10-104">With the Encode EDIFACT message connector, you can validate EDI and partner-specific properties, generate an XML document for each transaction set, and request a Technical Acknowledgement, Functional Acknowledgment, or both.</span></span>
<span data-ttu-id="95d10-105">Om du vill använda denna anslutning måste du lägga till anslutningen till en befintlig utlösare i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="95d10-105">To use this connector, you must add the connector to an existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="95d10-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="95d10-106">Before you start</span></span>

<span data-ttu-id="95d10-107">Här är de objekt som du behöver:</span><span class="sxs-lookup"><span data-stu-id="95d10-107">Here's the items you need:</span></span>

* <span data-ttu-id="95d10-108">Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="95d10-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="95d10-109">En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="95d10-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="95d10-110">Du måste ha ett integration konto att använda anslutningstjänsten EDIFACT koda meddelandet.</span><span class="sxs-lookup"><span data-stu-id="95d10-110">You must have an integration account to use the Encode EDIFACT message connector.</span></span> 
* <span data-ttu-id="95d10-111">Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="95d10-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="95d10-112">En [EDIFACT-avtal](logic-apps-enterprise-integration-edifact.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="95d10-112">An [EDIFACT agreement](logic-apps-enterprise-integration-edifact.md) that's already defined in your integration account</span></span>

## <a name="encode-edifact-messages"></a><span data-ttu-id="95d10-113">Koda EDIFACT-meddelanden</span><span class="sxs-lookup"><span data-stu-id="95d10-113">Encode EDIFACT messages</span></span>

1. <span data-ttu-id="95d10-114">[Skapa en logikapp](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="95d10-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="95d10-115">Koda EDIFACT meddelandet kopplingen har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran.</span><span class="sxs-lookup"><span data-stu-id="95d10-115">The Encode EDIFACT message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="95d10-116">Lägg till en utlösare i logik App Designer och sedan lägga till en åtgärd i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="95d10-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3.  <span data-ttu-id="95d10-117">I sökrutan anger du ”EDIFACT” som filter.</span><span class="sxs-lookup"><span data-stu-id="95d10-117">In the search box, enter "EDIFACT" as your filter.</span></span> <span data-ttu-id="95d10-118">Välj antingen **koda EDIFACT-meddelandet genom avtalsnamn** eller **koda på EDIFACT meddelandet identiteter**.</span><span class="sxs-lookup"><span data-stu-id="95d10-118">Select either **Encode EDIFACT Message by agreement name** or **Encode to EDIFACT message by identities**.</span></span>
   
    ![Sök EDIFACT](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. <span data-ttu-id="95d10-120">Om du inte tidigare skapade alla anslutningar till ditt konto integration, uppmanas du att skapa anslutningen nu.</span><span class="sxs-lookup"><span data-stu-id="95d10-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="95d10-121">Namnge din anslutning och välj integration kontot som du vill ansluta till.</span><span class="sxs-lookup"><span data-stu-id="95d10-121">Name your connection, and select the integration account that you want to connect.</span></span>

    ![Skapa integration konto anslutning](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    <span data-ttu-id="95d10-123">Egenskaper med en asterisk krävs.</span><span class="sxs-lookup"><span data-stu-id="95d10-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="95d10-124">Egenskap</span><span class="sxs-lookup"><span data-stu-id="95d10-124">Property</span></span> | <span data-ttu-id="95d10-125">Information</span><span class="sxs-lookup"><span data-stu-id="95d10-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="95d10-126">Anslutningsnamn *</span><span class="sxs-lookup"><span data-stu-id="95d10-126">Connection Name *</span></span> |<span data-ttu-id="95d10-127">Ange ett namn för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="95d10-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="95d10-128">Integration konto *</span><span class="sxs-lookup"><span data-stu-id="95d10-128">Integration Account *</span></span> |<span data-ttu-id="95d10-129">Ange ett namn för ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="95d10-129">Enter a name for your integration account.</span></span> <span data-ttu-id="95d10-130">Se till att appen integration konto och logik finns i samma Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="95d10-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

5.  <span data-ttu-id="95d10-131">När du är klar bör din anslutningsinformation likna exemplet.</span><span class="sxs-lookup"><span data-stu-id="95d10-131">When you're done, your connection details should look similar to this example.</span></span> <span data-ttu-id="95d10-132">Slutför din anslutning väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="95d10-132">To finish creating your connection, choose **Create**.</span></span>

    ![Integration kontoinformation för anslutning](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    <span data-ttu-id="95d10-134">Anslutningen har skapats.</span><span class="sxs-lookup"><span data-stu-id="95d10-134">Your connection is now created.</span></span>

    ![Integration konto anslutning som skapats](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a><span data-ttu-id="95d10-136">Koda EDIFACT-meddelande med avtalsnamn</span><span class="sxs-lookup"><span data-stu-id="95d10-136">Encode EDIFACT Message by agreement name</span></span>

<span data-ttu-id="95d10-137">Om du vill koda EDIFACT-meddelanden med avtalsnamn, öppna den **namn EDIFACT-avtal** listan, ange eller välj namnet på din EDIFACT-avtal.</span><span class="sxs-lookup"><span data-stu-id="95d10-137">If you chose to encode EDIFACT messages by agreement name, open the **Name of EDIFACT agreement** list, enter or select your EDIFACT agreement name.</span></span> <span data-ttu-id="95d10-138">Ange den XML-meddelanden om att koda.</span><span class="sxs-lookup"><span data-stu-id="95d10-138">Enter the XML message to encode.</span></span>

![Ange EDIFACT avtalets namn och XML-meddelande att koda](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a><span data-ttu-id="95d10-140">Koda EDIFACT-meddelande med identiteter</span><span class="sxs-lookup"><span data-stu-id="95d10-140">Encode EDIFACT Message by identities</span></span>

<span data-ttu-id="95d10-141">Om du vill koda EDIFACT-meddelanden med identiteter Ange ingen avsändaridentifierare, kvalificerare för avsändaren, mottagaren identifierare och mottagaren kvalificeraren som konfigurerats i EDIFACT-avtal.</span><span class="sxs-lookup"><span data-stu-id="95d10-141">If you choose to encode EDIFACT messages by identities, enter the sender identifier, sender qualifier, receiver identifier, and receiver qualifier as configured in your EDIFACT agreement.</span></span> <span data-ttu-id="95d10-142">Välj XML-meddelandet för att koda.</span><span class="sxs-lookup"><span data-stu-id="95d10-142">Select the XML message to encode.</span></span>

![Ange identiteter för avsändare och mottagare, Välj XML-meddelande att koda](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a><span data-ttu-id="95d10-144">EDIFACT koda information</span><span class="sxs-lookup"><span data-stu-id="95d10-144">EDIFACT Encode details</span></span>

<span data-ttu-id="95d10-145">Koda EDIFACT-kopplingen utför dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="95d10-145">The Encode EDIFACT connector performs these tasks:</span></span> 

* <span data-ttu-id="95d10-146">Lös avtalet genom att matcha avsändaren kvalificerare & identifierare och mottagaren kvalificerare och identifierare</span><span class="sxs-lookup"><span data-stu-id="95d10-146">Resolve the agreement by matching the sender qualifier & identifier and receiver qualifier and identifier</span></span>
* <span data-ttu-id="95d10-147">Serialiserar EDI-utbyte, konvertera XML-kodade meddelanden till uppsättningar för EDI-transaktion i utbyte.</span><span class="sxs-lookup"><span data-stu-id="95d10-147">Serializes the EDI interchange, converting XML-encoded messages into EDI transaction sets in the interchange.</span></span>
* <span data-ttu-id="95d10-148">Gäller transaktion set-huvudet och trailern segment</span><span class="sxs-lookup"><span data-stu-id="95d10-148">Applies transaction set header and trailer segments</span></span>
* <span data-ttu-id="95d10-149">Genererar ett utbyte kontrollen Antal, gruppnumret kontroll och en uppsättning kontrollen transaktionsnumret för varje utgående utbyte</span><span class="sxs-lookup"><span data-stu-id="95d10-149">Generates an interchange control number, a group control number, and a transaction set control number for each outgoing interchange</span></span>
* <span data-ttu-id="95d10-150">Ersätter avgränsare i nyttolasten</span><span class="sxs-lookup"><span data-stu-id="95d10-150">Replaces separators in the payload data</span></span>
* <span data-ttu-id="95d10-151">Verifierar EDI och partner-specifika egenskaper</span><span class="sxs-lookup"><span data-stu-id="95d10-151">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="95d10-152">Schemavalideringen av transaktionen set dataelement mot meddelandet schemat.</span><span class="sxs-lookup"><span data-stu-id="95d10-152">Schema validation of the transaction-set data elements against the message schema.</span></span>
  * <span data-ttu-id="95d10-153">EDI-verifiering utförs på dataelement för transaktionen uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="95d10-153">EDI validation performed on transaction-set data elements.</span></span>
  * <span data-ttu-id="95d10-154">Utökad verifiering utförs på transaktion set dataelement</span><span class="sxs-lookup"><span data-stu-id="95d10-154">Extended validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="95d10-155">Genererar XML-dokument för varje transaktion.</span><span class="sxs-lookup"><span data-stu-id="95d10-155">Generates an XML document for each transaction set.</span></span>
* <span data-ttu-id="95d10-156">Begär en tekniska och/eller funktionella bekräftelse (om konfigurerad).</span><span class="sxs-lookup"><span data-stu-id="95d10-156">Requests a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="95d10-157">Som en teknisk bekräftelse anger CONTRL meddelandet har mottagit ett utbyte.</span><span class="sxs-lookup"><span data-stu-id="95d10-157">As a technical acknowledgment, the CONTRL message indicates receipt of an interchange.</span></span>
  * <span data-ttu-id="95d10-158">Som en funktionell bekräftelse anger CONTRL meddelandet godkännande eller underkännande mottagna interchange, grupp eller meddelandet med en lista över fel eller funktioner som inte stöds</span><span class="sxs-lookup"><span data-stu-id="95d10-158">As a functional acknowledgment, the CONTRL message indicates acceptance or rejection of the received interchange, group, or message, with a list of errors or unsupported functionality</span></span>

## <a name="view-swagger-file"></a><span data-ttu-id="95d10-159">Visa Swagger-fil</span><span class="sxs-lookup"><span data-stu-id="95d10-159">View Swagger file</span></span>
<span data-ttu-id="95d10-160">Swagger-information för EDIFACT-anslutningen finns i [EDIFACT](/connectors/edifact/).</span><span class="sxs-lookup"><span data-stu-id="95d10-160">To view the Swagger details for the EDIFACT connector, see [EDIFACT](/connectors/edifact/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="95d10-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="95d10-161">Next steps</span></span>
[<span data-ttu-id="95d10-162">Mer information om Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="95d10-162">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

