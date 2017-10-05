---
title: Avkoda meddelanden med EDIFACT - Azure Logic Apps | Microsoft Docs
description: "Validera EDI och generera bekräftelser med EDIFACT meddelandet avkodaren i Enterprise-Integrationspaket för Logikappar i Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 0e61501d-21a2-4419-8c6c-88724d346e81
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: e3787b48037360bf6066ddce2bacba6842213b2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a><span data-ttu-id="74b98-103">Avkoda EDIFACT-meddelanden för Azure Logikappar med Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="74b98-103">Decode EDIFACT messages for Azure Logic Apps with the Enterprise Integration Pack</span></span>

<span data-ttu-id="74b98-104">Med meddelandet avkoda EDIFACT-anslutningen kan du verifiera EDI och partner-specifika egenskaper, dela externa utbyten i transaktioner uppsättningar eller bevara hela externa utbyten och generera bekräftelser för bearbetade transaktioner.</span><span class="sxs-lookup"><span data-stu-id="74b98-104">With the Decode EDIFACT message connector, you can validate EDI and partner-specific properties, split interchanges into transactions sets or preserve entire interchanges, and generate acknowledgments for processed transactions.</span></span> <span data-ttu-id="74b98-105">Om du vill använda denna anslutning måste du lägga till anslutningen till en befintlig utlösare i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="74b98-105">To use this connector, you must add the connector to an existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="74b98-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="74b98-106">Before you start</span></span>

<span data-ttu-id="74b98-107">Här är de objekt som du behöver:</span><span class="sxs-lookup"><span data-stu-id="74b98-107">Here's the items you need:</span></span>

* <span data-ttu-id="74b98-108">Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="74b98-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="74b98-109">En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="74b98-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="74b98-110">Du måste ha ett integration konto att använda anslutningstjänsten EDIFACT avkoda meddelandet.</span><span class="sxs-lookup"><span data-stu-id="74b98-110">You must have an integration account to use the Decode EDIFACT message connector.</span></span> 
* <span data-ttu-id="74b98-111">Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="74b98-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="74b98-112">En [EDIFACT-avtal](logic-apps-enterprise-integration-edifact.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="74b98-112">An [EDIFACT agreement](logic-apps-enterprise-integration-edifact.md) that's already defined in your integration account</span></span>

## <a name="decode-edifact-messages"></a><span data-ttu-id="74b98-113">Avkoda EDIFACT-meddelanden</span><span class="sxs-lookup"><span data-stu-id="74b98-113">Decode EDIFACT messages</span></span>

1. <span data-ttu-id="74b98-114">[Skapa en logikapp](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="74b98-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="74b98-115">Avkoda EDIFACT meddelandet kopplingen har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran.</span><span class="sxs-lookup"><span data-stu-id="74b98-115">The Decode EDIFACT message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="74b98-116">Lägg till en utlösare i logik App Designer och sedan lägga till en åtgärd i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="74b98-116">In the Logic App Designer, add a trigger, and then add an action to your logic app.</span></span>

3. <span data-ttu-id="74b98-117">I sökrutan anger du ”EDIFACT” som filter.</span><span class="sxs-lookup"><span data-stu-id="74b98-117">In the search box, enter "EDIFACT" as your filter.</span></span> <span data-ttu-id="74b98-118">Välj **avkoda meddelandet EDIFACT**.</span><span class="sxs-lookup"><span data-stu-id="74b98-118">Select **Decode EDIFACT Message**.</span></span>
   
    ![Sök EDIFACT](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. <span data-ttu-id="74b98-120">Om du inte tidigare skapade alla anslutningar till ditt konto integration, uppmanas du att skapa anslutningen nu.</span><span class="sxs-lookup"><span data-stu-id="74b98-120">If you didn't previously create any connections to your integration account, you're prompted to create that connection now.</span></span> <span data-ttu-id="74b98-121">Namnge din anslutning och välj integration kontot som du vill ansluta till.</span><span class="sxs-lookup"><span data-stu-id="74b98-121">Name your connection, and select the integration account that you want to connect.</span></span>
   
    ![Skapa integration konto](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    <span data-ttu-id="74b98-123">Egenskaper med en asterisk krävs.</span><span class="sxs-lookup"><span data-stu-id="74b98-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="74b98-124">Egenskap</span><span class="sxs-lookup"><span data-stu-id="74b98-124">Property</span></span> | <span data-ttu-id="74b98-125">Information</span><span class="sxs-lookup"><span data-stu-id="74b98-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="74b98-126">Anslutningsnamn *</span><span class="sxs-lookup"><span data-stu-id="74b98-126">Connection Name *</span></span> |<span data-ttu-id="74b98-127">Ange ett namn för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="74b98-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="74b98-128">Integration konto *</span><span class="sxs-lookup"><span data-stu-id="74b98-128">Integration Account *</span></span> |<span data-ttu-id="74b98-129">Ange ett namn för ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="74b98-129">Enter a name for your integration account.</span></span> <span data-ttu-id="74b98-130">Se till att appen integration konto och logik finns i samma Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="74b98-130">Make sure that your integration account and logic app are in the same Azure location.</span></span> |

4. <span data-ttu-id="74b98-131">När du är klar att anslutningen är klar, Välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="74b98-131">When you're done to finish creating your connection, choose **Create**.</span></span> <span data-ttu-id="74b98-132">Information om anslutningen bör likna exemplet:</span><span class="sxs-lookup"><span data-stu-id="74b98-132">Your connection details should look similar to this example:</span></span>

    ![Integration kontoinformation](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. <span data-ttu-id="74b98-134">När anslutningen har skapats, som visas i det här exemplet, Välj EDIFACT flat fil meddelandet att avkoda.</span><span class="sxs-lookup"><span data-stu-id="74b98-134">After your connection is created, as shown in this example, select the EDIFACT flat file message to decode.</span></span>

    ![Integration konto anslutning som skapats](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    <span data-ttu-id="74b98-136">Exempel:</span><span class="sxs-lookup"><span data-stu-id="74b98-136">For example:</span></span>

    ![Välj EDIFACT flat fil meddelande för avkodning av](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a><span data-ttu-id="74b98-138">EDIFACT avkodarens information</span><span class="sxs-lookup"><span data-stu-id="74b98-138">EDIFACT decoder details</span></span>

<span data-ttu-id="74b98-139">Avkoda EDIFACT-kopplingen utför dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="74b98-139">The Decode EDIFACT connector performs these tasks:</span></span> 

* <span data-ttu-id="74b98-140">Verifierar kuvertet mot handel partneravtalet.</span><span class="sxs-lookup"><span data-stu-id="74b98-140">Validates the envelope against trading partner agreement.</span></span>
* <span data-ttu-id="74b98-141">Löser avtalet genom att matcha avsändaren kvalificerare och identifierare och mottagaren kvalificerare och identifierare.</span><span class="sxs-lookup"><span data-stu-id="74b98-141">Resolves the agreement by matching the sender qualifier & identifier and receiver qualifier & identifier.</span></span>
* <span data-ttu-id="74b98-142">Delar en interchange i flera transaktioner när utbyte har fler än en transaktion baserat på avtalet ta emot konfigurationen av prenumerationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="74b98-142">Splits an interchange into multiple transactions when the interchange has more than one transaction based on the agreement's receive settings configuration.</span></span>
* <span data-ttu-id="74b98-143">Disassemblerar utbyte.</span><span class="sxs-lookup"><span data-stu-id="74b98-143">Disassembles the interchange.</span></span>
* <span data-ttu-id="74b98-144">Verifierar EDI och partner-specifika egenskaper, inklusive:</span><span class="sxs-lookup"><span data-stu-id="74b98-144">Validates EDI and partner-specific properties including:</span></span>
  * <span data-ttu-id="74b98-145">Validering av interchange kuvert struktur</span><span class="sxs-lookup"><span data-stu-id="74b98-145">Validation of the interchange envelope structure</span></span>
  * <span data-ttu-id="74b98-146">Schemavalideringen Envelope mot kontroll schemat</span><span class="sxs-lookup"><span data-stu-id="74b98-146">Schema validation of the envelope against the control schema</span></span>
  * <span data-ttu-id="74b98-147">Schemavalideringen av transaktionen set dataelement mot meddelandet schemat</span><span class="sxs-lookup"><span data-stu-id="74b98-147">Schema validation of the transaction-set data elements against the message schema</span></span>
  * <span data-ttu-id="74b98-148">EDI-verifiering utförs på transaktion set dataelement</span><span class="sxs-lookup"><span data-stu-id="74b98-148">EDI validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="74b98-149">Verifierar att interchange, grupp och transaktionen set kontrollen talen som inte är dubbletter (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="74b98-149">Verifies that the interchange, group, and transaction set control numbers are not duplicates (if configured)</span></span> 
  * <span data-ttu-id="74b98-150">Kontrollerar antalet interchange kontroll mot tidigare mottagna externa utbyten.</span><span class="sxs-lookup"><span data-stu-id="74b98-150">Checks the interchange control number against previously received interchanges.</span></span> 
  * <span data-ttu-id="74b98-151">Kontrollerar kontrollen gruppnumret mot andra grupp kontrollen talen i utbyte.</span><span class="sxs-lookup"><span data-stu-id="74b98-151">Checks the group control number against other group control numbers in the interchange.</span></span> 
  * <span data-ttu-id="74b98-152">Kontrollerar transaktionen ange kontroll många mot andra transaktion set kontrollen talen i den gruppen.</span><span class="sxs-lookup"><span data-stu-id="74b98-152">Checks the transaction set control number against other transaction set control numbers in that group.</span></span>
* <span data-ttu-id="74b98-153">Delar upp interchange i transaktionen uppsättningar eller bevarar hela interchange:</span><span class="sxs-lookup"><span data-stu-id="74b98-153">Splits the interchange into transaction sets, or preserves the entire interchange:</span></span>
  * <span data-ttu-id="74b98-154">Dela Interchange som transaktionen uppsättningar - inaktivera transaktion anger vid fel: delningar interchange i transaktion anger och Parsar varje transaktion uppsättning.</span><span class="sxs-lookup"><span data-stu-id="74b98-154">Split Interchange as transaction sets - suspend transaction sets on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="74b98-155">X12 avkoda åtgärd matar ut bara dessa transaktion anger som inte kan valideras till `badMessages`, och utdata återstående transaktioner anger till `goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="74b98-155">The X12 Decode action outputs only those transaction sets that fail validation to `badMessages`, and outputs the remaining transactions sets to `goodMessages`.</span></span>
  * <span data-ttu-id="74b98-156">Dela Interchange som transaktionen uppsättningar - inaktivera utbyte vid fel: delningar interchange i transaktion anger och Parsar varje transaktion uppsättning.</span><span class="sxs-lookup"><span data-stu-id="74b98-156">Split Interchange as transaction sets - suspend interchange on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="74b98-157">Om en eller flera transaktion anger i utbyte inte verifiering, X12 avkoda åtgärd matar ut alla transaktionen anger i den interchange till `badMessages`.</span><span class="sxs-lookup"><span data-stu-id="74b98-157">If one or more transaction sets in the interchange fail validation, the X12 Decode action outputs all the transaction sets in that interchange to `badMessages`.</span></span>
  * <span data-ttu-id="74b98-158">Bevara Interchange – inaktivera transaktion anger vid fel: bevara utbyte och bearbeta hela gruppbaserad utbyte.</span><span class="sxs-lookup"><span data-stu-id="74b98-158">Preserve Interchange - suspend transaction sets on error: Preserve the interchange and process the entire batched interchange.</span></span> 
  <span data-ttu-id="74b98-159">X12 avkoda åtgärd matar ut bara dessa transaktion anger som inte kan valideras till `badMessages`, och utdata återstående transaktioner anger till `goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="74b98-159">The X12 Decode action outputs only those transaction sets that fail validation to `badMessages`, and outputs the remaining transactions sets to `goodMessages`.</span></span>
  * <span data-ttu-id="74b98-160">Bevara Interchange – inaktivera utbyte vid fel: bevara utbyte och bearbeta hela gruppbaserad utbyte.</span><span class="sxs-lookup"><span data-stu-id="74b98-160">Preserve Interchange - suspend interchange on error: Preserve the interchange and process the entire batched interchange.</span></span> 
  <span data-ttu-id="74b98-161">Om en eller flera transaktion anger i utbyte inte verifiering, X12 avkoda åtgärd matar ut alla transaktionen anger i den interchange till `badMessages`.</span><span class="sxs-lookup"><span data-stu-id="74b98-161">If one or more transaction sets in the interchange fail validation, the X12 Decode action outputs all the transaction sets in that interchange to `badMessages`.</span></span>
* <span data-ttu-id="74b98-162">Genererar en Technical (kontroll) och/eller funktionella bekräftelse (om konfigurerad).</span><span class="sxs-lookup"><span data-stu-id="74b98-162">Generates a Technical (control) and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="74b98-163">En teknisk bekräftelse eller CONTRL ACK rapporterar resultatet av en syntaktiska kontroll av fullständig mottagna utbyte.</span><span class="sxs-lookup"><span data-stu-id="74b98-163">A Technical Acknowledgment or the CONTRL ACK reports the results of a syntactical check of the complete received interchange.</span></span>
  * <span data-ttu-id="74b98-164">En funktionell bekräftelse om godkänna eller avvisa en mottagna utbyte eller en grupp</span><span class="sxs-lookup"><span data-stu-id="74b98-164">A functional acknowledgment acknowledges accept or reject a received interchange or a group</span></span>

## <a name="view-swagger-file"></a><span data-ttu-id="74b98-165">Visa Swagger-fil</span><span class="sxs-lookup"><span data-stu-id="74b98-165">View Swagger file</span></span>
<span data-ttu-id="74b98-166">Swagger-information för EDIFACT-anslutningen finns i [EDIFACT](/connectors/edifact/).</span><span class="sxs-lookup"><span data-stu-id="74b98-166">To view the Swagger details for the EDIFACT connector, see [EDIFACT](/connectors/edifact/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="74b98-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="74b98-167">Next steps</span></span>
[<span data-ttu-id="74b98-168">Mer information om Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="74b98-168">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

