---
title: aaaDecode EDIFACT-meddelanden - Azure Logic Apps | Microsoft Docs
description: "Validera EDI och generera bekräftelser med hello EDIFACT meddelandet avkodarens i hello Enterprise-Integrationspaket för Logikappar i Azure"
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
ms.openlocfilehash: 94faebdec4e4ffc8ad76ad1609495ddf9f002250
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="c75ef-103">Avkoda EDIFACT-meddelanden för Logikappar i Azure med hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="c75ef-103">Decode EDIFACT messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="c75ef-104">Med hello avkoda EDIFACT-meddelande anslutningen kan du verifiera EDI och partner-specifika egenskaper, dela externa utbyten i transaktioner uppsättningar eller bevara hela externa utbyten och generera bekräftelser för bearbetade transaktioner.</span><span class="sxs-lookup"><span data-stu-id="c75ef-104">With hello Decode EDIFACT message connector, you can validate EDI and partner-specific properties, split interchanges into transactions sets or preserve entire interchanges, and generate acknowledgments for processed transactions.</span></span> <span data-ttu-id="c75ef-105">toouse den här kopplingen måste du lägga till hello connector tooan befintliga utlösaren i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="c75ef-105">toouse this connector, you must add hello connector tooan existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="c75ef-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c75ef-106">Before you start</span></span>

<span data-ttu-id="c75ef-107">Här är hello-objekt som du behöver:</span><span class="sxs-lookup"><span data-stu-id="c75ef-107">Here's hello items you need:</span></span>

* <span data-ttu-id="c75ef-108">Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="c75ef-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="c75ef-109">En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c75ef-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="c75ef-110">Du måste ha en integration konto toouse hello avkoda EDIFACT meddelande-koppling.</span><span class="sxs-lookup"><span data-stu-id="c75ef-110">You must have an integration account toouse hello Decode EDIFACT message connector.</span></span> 
* <span data-ttu-id="c75ef-111">Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="c75ef-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="c75ef-112">En [EDIFACT-avtal](logic-apps-enterprise-integration-edifact.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="c75ef-112">An [EDIFACT agreement](logic-apps-enterprise-integration-edifact.md) that's already defined in your integration account</span></span>

## <a name="decode-edifact-messages"></a><span data-ttu-id="c75ef-113">Avkoda EDIFACT-meddelanden</span><span class="sxs-lookup"><span data-stu-id="c75ef-113">Decode EDIFACT messages</span></span>

1. <span data-ttu-id="c75ef-114">[Skapa en logikapp](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c75ef-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="c75ef-115">hello avkoda EDIFACT meddelandet connector har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran.</span><span class="sxs-lookup"><span data-stu-id="c75ef-115">hello Decode EDIFACT message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="c75ef-116">Lägg till en utlösare hello logik App Designer, och sedan lägga till en åtgärd tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="c75ef-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3. <span data-ttu-id="c75ef-117">Ange ”EDIFACT” i sökrutan hello som filter.</span><span class="sxs-lookup"><span data-stu-id="c75ef-117">In hello search box, enter "EDIFACT" as your filter.</span></span> <span data-ttu-id="c75ef-118">Välj **avkoda meddelandet EDIFACT**.</span><span class="sxs-lookup"><span data-stu-id="c75ef-118">Select **Decode EDIFACT Message**.</span></span>
   
    ![Sök EDIFACT](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. <span data-ttu-id="c75ef-120">Om du tidigare inte har skapat alla anslutningar tooyour integrering konto uppmanas du toocreate anslutningen nu.</span><span class="sxs-lookup"><span data-stu-id="c75ef-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="c75ef-121">Namnge din anslutning och välj hello integration konto som du vill tooconnect.</span><span class="sxs-lookup"><span data-stu-id="c75ef-121">Name your connection, and select hello integration account that you want tooconnect.</span></span>
   
    ![Skapa integration konto](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    <span data-ttu-id="c75ef-123">Egenskaper med en asterisk krävs.</span><span class="sxs-lookup"><span data-stu-id="c75ef-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="c75ef-124">Egenskap</span><span class="sxs-lookup"><span data-stu-id="c75ef-124">Property</span></span> | <span data-ttu-id="c75ef-125">Information</span><span class="sxs-lookup"><span data-stu-id="c75ef-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="c75ef-126">Anslutningsnamn *</span><span class="sxs-lookup"><span data-stu-id="c75ef-126">Connection Name *</span></span> |<span data-ttu-id="c75ef-127">Ange ett namn för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="c75ef-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="c75ef-128">Integration konto *</span><span class="sxs-lookup"><span data-stu-id="c75ef-128">Integration Account *</span></span> |<span data-ttu-id="c75ef-129">Ange ett namn för ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="c75ef-129">Enter a name for your integration account.</span></span> <span data-ttu-id="c75ef-130">Se till att appen integration konto och logik finns i hello samma Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="c75ef-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

4. <span data-ttu-id="c75ef-131">När du är klar att skapa anslutningen toofinish välja **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c75ef-131">When you're done toofinish creating your connection, choose **Create**.</span></span> <span data-ttu-id="c75ef-132">Information om anslutningen ska se liknande toothis exempel:</span><span class="sxs-lookup"><span data-stu-id="c75ef-132">Your connection details should look similar toothis example:</span></span>

    ![Integration kontoinformation](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. <span data-ttu-id="c75ef-134">När anslutningen har skapats, som visas i det här exemplet, Välj hello EDIFACT flat fil meddelandet toodecode.</span><span class="sxs-lookup"><span data-stu-id="c75ef-134">After your connection is created, as shown in this example, select hello EDIFACT flat file message toodecode.</span></span>

    ![Integration konto anslutning som skapats](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    <span data-ttu-id="c75ef-136">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c75ef-136">For example:</span></span>

    ![Välj EDIFACT flat fil meddelande för avkodning av](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a><span data-ttu-id="c75ef-138">EDIFACT avkodarens information</span><span class="sxs-lookup"><span data-stu-id="c75ef-138">EDIFACT decoder details</span></span>

<span data-ttu-id="c75ef-139">hello avkoda EDIFACT connector utför dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="c75ef-139">hello Decode EDIFACT connector performs these tasks:</span></span> 

* <span data-ttu-id="c75ef-140">Verifierar hello kuvert mot handel partneravtalet.</span><span class="sxs-lookup"><span data-stu-id="c75ef-140">Validates hello envelope against trading partner agreement.</span></span>
* <span data-ttu-id="c75ef-141">Löser hello avtalet genom att matcha hello avsändaren kvalificerare identifierare och mottagaren kvalificerare & identifierare.</span><span class="sxs-lookup"><span data-stu-id="c75ef-141">Resolves hello agreement by matching hello sender qualifier & identifier and receiver qualifier & identifier.</span></span>
* <span data-ttu-id="c75ef-142">Delar en interchange i flera transaktioner när hello interchange har fler än en transaktion baserat på hello avtal ta emot konfigurationen av prenumerationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="c75ef-142">Splits an interchange into multiple transactions when hello interchange has more than one transaction based on hello agreement's receive settings configuration.</span></span>
* <span data-ttu-id="c75ef-143">Disassemblerar hello utbyte.</span><span class="sxs-lookup"><span data-stu-id="c75ef-143">Disassembles hello interchange.</span></span>
* <span data-ttu-id="c75ef-144">Verifierar EDI och partner-specifika egenskaper, inklusive:</span><span class="sxs-lookup"><span data-stu-id="c75ef-144">Validates EDI and partner-specific properties including:</span></span>
  * <span data-ttu-id="c75ef-145">Validering av hello interchange kuvert struktur</span><span class="sxs-lookup"><span data-stu-id="c75ef-145">Validation of hello interchange envelope structure</span></span>
  * <span data-ttu-id="c75ef-146">Schemavalideringen hello Envelope mot hello kontrollen schema</span><span class="sxs-lookup"><span data-stu-id="c75ef-146">Schema validation of hello envelope against hello control schema</span></span>
  * <span data-ttu-id="c75ef-147">Schemavalideringen av hello transaktion set dataelement mot hello Meddelandeschema</span><span class="sxs-lookup"><span data-stu-id="c75ef-147">Schema validation of hello transaction-set data elements against hello message schema</span></span>
  * <span data-ttu-id="c75ef-148">EDI-verifiering utförs på transaktion set dataelement</span><span class="sxs-lookup"><span data-stu-id="c75ef-148">EDI validation performed on transaction-set data elements</span></span>
* <span data-ttu-id="c75ef-149">Verifierar att hello interchange, grupp och transaktionen set kontrollen siffror inte är dubbletter (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="c75ef-149">Verifies that hello interchange, group, and transaction set control numbers are not duplicates (if configured)</span></span> 
  * <span data-ttu-id="c75ef-150">Kontrollerar hello interchange kontrollnummer mot tidigare mottagna externa utbyten.</span><span class="sxs-lookup"><span data-stu-id="c75ef-150">Checks hello interchange control number against previously received interchanges.</span></span> 
  * <span data-ttu-id="c75ef-151">Kontrollerar hello kontrollen gruppnumret mot andra grupp kontrollen talen i hello utbyte.</span><span class="sxs-lookup"><span data-stu-id="c75ef-151">Checks hello group control number against other group control numbers in hello interchange.</span></span> 
  * <span data-ttu-id="c75ef-152">Kontrollerar hello transaktion ange kontroll många mot andra transaktion set kontrollen talen i den gruppen.</span><span class="sxs-lookup"><span data-stu-id="c75ef-152">Checks hello transaction set control number against other transaction set control numbers in that group.</span></span>
* <span data-ttu-id="c75ef-153">Delar upp hello interchange i transaktionen uppsättningar eller bevarar hello hela interchange:</span><span class="sxs-lookup"><span data-stu-id="c75ef-153">Splits hello interchange into transaction sets, or preserves hello entire interchange:</span></span>
  * <span data-ttu-id="c75ef-154">Dela Interchange som transaktionen uppsättningar - inaktivera transaktion anger vid fel: delningar interchange i transaktion anger och Parsar varje transaktion uppsättning.</span><span class="sxs-lookup"><span data-stu-id="c75ef-154">Split Interchange as transaction sets - suspend transaction sets on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="c75ef-155">Hej X12 avkoda åtgärd matar ut bara de transaktion filer som inte kan valideras för`badMessages`, och anger hello återstående transaktioner anger för`goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="c75ef-155">hello X12 Decode action outputs only those transaction sets that fail validation too`badMessages`, and outputs hello remaining transactions sets too`goodMessages`.</span></span>
  * <span data-ttu-id="c75ef-156">Dela Interchange som transaktionen uppsättningar - inaktivera utbyte vid fel: delningar interchange i transaktion anger och Parsar varje transaktion uppsättning.</span><span class="sxs-lookup"><span data-stu-id="c75ef-156">Split Interchange as transaction sets - suspend interchange on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="c75ef-157">Om en eller flera transaktion anger i hello interchange misslyckas verifieringen, hello X12 avkoda åtgärd matar ut alla hello transaktion anger i den interchange för`badMessages`.</span><span class="sxs-lookup"><span data-stu-id="c75ef-157">If one or more transaction sets in hello interchange fail validation, hello X12 Decode action outputs all hello transaction sets in that interchange too`badMessages`.</span></span>
  * <span data-ttu-id="c75ef-158">Bevara Interchange – inaktivera transaktion anger vid fel: Preserve hello utbyte och processen hello hela gruppbaserad utbyte.</span><span class="sxs-lookup"><span data-stu-id="c75ef-158">Preserve Interchange - suspend transaction sets on error: Preserve hello interchange and process hello entire batched interchange.</span></span> 
  <span data-ttu-id="c75ef-159">Hej X12 avkoda åtgärd matar ut bara de transaktion filer som inte kan valideras för`badMessages`, och anger hello återstående transaktioner anger för`goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="c75ef-159">hello X12 Decode action outputs only those transaction sets that fail validation too`badMessages`, and outputs hello remaining transactions sets too`goodMessages`.</span></span>
  * <span data-ttu-id="c75ef-160">Bevara Interchange – inaktivera utbyte vid fel: Preserve hello utbyte och processen hello hela gruppbaserad utbyte.</span><span class="sxs-lookup"><span data-stu-id="c75ef-160">Preserve Interchange - suspend interchange on error: Preserve hello interchange and process hello entire batched interchange.</span></span> 
  <span data-ttu-id="c75ef-161">Om en eller flera transaktion anger i hello interchange misslyckas verifieringen, hello X12 avkoda åtgärd matar ut alla hello transaktion anger i den interchange för`badMessages`.</span><span class="sxs-lookup"><span data-stu-id="c75ef-161">If one or more transaction sets in hello interchange fail validation, hello X12 Decode action outputs all hello transaction sets in that interchange too`badMessages`.</span></span>
* <span data-ttu-id="c75ef-162">Genererar en Technical (kontroll) och/eller funktionella bekräftelse (om konfigurerad).</span><span class="sxs-lookup"><span data-stu-id="c75ef-162">Generates a Technical (control) and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="c75ef-163">En teknisk bekräftelse eller hello CONTRL ACK rapporterar hello resultaten av en syntaktiska kontroll av hello fullständig emot utbyte.</span><span class="sxs-lookup"><span data-stu-id="c75ef-163">A Technical Acknowledgment or hello CONTRL ACK reports hello results of a syntactical check of hello complete received interchange.</span></span>
  * <span data-ttu-id="c75ef-164">En funktionell bekräftelse om godkänna eller avvisa en mottagna utbyte eller en grupp</span><span class="sxs-lookup"><span data-stu-id="c75ef-164">A functional acknowledgment acknowledges accept or reject a received interchange or a group</span></span>

## <a name="view-swagger-file"></a><span data-ttu-id="c75ef-165">Visa Swagger-fil</span><span class="sxs-lookup"><span data-stu-id="c75ef-165">View Swagger file</span></span>
<span data-ttu-id="c75ef-166">tooview hello Swagger information för hello EDIFACT-anslutningen finns [EDIFACT](/connectors/edifact/).</span><span class="sxs-lookup"><span data-stu-id="c75ef-166">tooview hello Swagger details for hello EDIFACT connector, see [EDIFACT](/connectors/edifact/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c75ef-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c75ef-167">Next steps</span></span>
[<span data-ttu-id="c75ef-168">Mer information om hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="c75ef-168">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

