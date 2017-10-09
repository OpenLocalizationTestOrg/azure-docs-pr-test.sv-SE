---
title: aaaDecode X12 meddelanden - Azure Logic Apps | Microsoft Docs
description: "Validera EDI och generera bekräftelser med hello X12 meddelande avkodarens i hello Enterprise-Integrationspaket för Logikappar i Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4fd48d2d-2008-4080-b6a1-8ae183b48131
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 1ffececca1ff835b319b64c85f86c421395833c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="decode-x12-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="808c5-103">Avkoda X12 meddelanden för Logikappar i Azure med hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="808c5-103">Decode X12 messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span>

<span data-ttu-id="808c5-104">Med hello avkoda X12 meddelande-anslutningen kan kan du verifiera hello kuvert mot ett handelspartneravtal, verifiera EDI och partner-specifika egenskaper, dela externa utbyten i transaktioner uppsättningar eller bevara hela externa utbyten och generera bekräftelser för bearbetade transaktioner.</span><span class="sxs-lookup"><span data-stu-id="808c5-104">With hello Decode X12 message connector, you can validate hello envelope against a trading partner agreement, validate EDI and partner-specific properties, split interchanges into transactions sets or preserve entire interchanges, and generate acknowledgments for processed transactions.</span></span> <span data-ttu-id="808c5-105">toouse den här kopplingen måste du lägga till hello connector tooan befintliga utlösaren i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="808c5-105">toouse this connector, you must add hello connector tooan existing trigger in your logic app.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="808c5-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="808c5-106">Before you start</span></span>

<span data-ttu-id="808c5-107">Här är hello-objekt som du behöver:</span><span class="sxs-lookup"><span data-stu-id="808c5-107">Here's hello items you need:</span></span>

* <span data-ttu-id="808c5-108">Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="808c5-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="808c5-109">En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="808c5-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="808c5-110">Du måste ha en integration konto toouse hello avkoda X12 meddelande-koppling.</span><span class="sxs-lookup"><span data-stu-id="808c5-110">You must have an integration account toouse hello Decode X12 message connector.</span></span>
* <span data-ttu-id="808c5-111">Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="808c5-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="808c5-112">En [X12 avtal](logic-apps-enterprise-integration-x12.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="808c5-112">An [X12 agreement](logic-apps-enterprise-integration-x12.md) that's already defined in your integration account</span></span>

## <a name="decode-x12-messages"></a><span data-ttu-id="808c5-113">Avkoda X12 meddelanden</span><span class="sxs-lookup"><span data-stu-id="808c5-113">Decode X12 messages</span></span>

1. <span data-ttu-id="808c5-114">[Skapa en logikapp](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="808c5-114">[Create a logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="808c5-115">hello avkoda X12 meddelande connector har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran.</span><span class="sxs-lookup"><span data-stu-id="808c5-115">hello Decode X12 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="808c5-116">Lägg till en utlösare hello logik App Designer, och sedan lägga till en åtgärd tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="808c5-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="808c5-117">Ange ”x12” i sökrutan hello för filtret.</span><span class="sxs-lookup"><span data-stu-id="808c5-117">In hello search box, enter "x12" for your filter.</span></span> <span data-ttu-id="808c5-118">Välj **X12-avkoda X12 meddelandet**.</span><span class="sxs-lookup"><span data-stu-id="808c5-118">Select **X12 - Decode X12 message**.</span></span>
   
    ![Sök efter ”x12”](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. <span data-ttu-id="808c5-120">Om du tidigare inte har skapat alla anslutningar tooyour integrering konto uppmanas du toocreate anslutningen nu.</span><span class="sxs-lookup"><span data-stu-id="808c5-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="808c5-121">Namnge din anslutning och välj hello integration konto som du vill tooconnect.</span><span class="sxs-lookup"><span data-stu-id="808c5-121">Name your connection, and select hello integration account that you want tooconnect.</span></span> 

    ![Ange integration kontoinformation för anslutning](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    <span data-ttu-id="808c5-123">Egenskaper med en asterisk krävs.</span><span class="sxs-lookup"><span data-stu-id="808c5-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="808c5-124">Egenskap</span><span class="sxs-lookup"><span data-stu-id="808c5-124">Property</span></span> | <span data-ttu-id="808c5-125">Information</span><span class="sxs-lookup"><span data-stu-id="808c5-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="808c5-126">Anslutningsnamn *</span><span class="sxs-lookup"><span data-stu-id="808c5-126">Connection Name *</span></span> |<span data-ttu-id="808c5-127">Ange ett namn för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="808c5-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="808c5-128">Integration konto *</span><span class="sxs-lookup"><span data-stu-id="808c5-128">Integration Account *</span></span> |<span data-ttu-id="808c5-129">Ange ett namn för ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="808c5-129">Enter a name for your integration account.</span></span> <span data-ttu-id="808c5-130">Se till att appen integration konto och logik finns i hello samma Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="808c5-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="808c5-131">När du är klar din innehållet bör se ut liknande toothis exempel.</span><span class="sxs-lookup"><span data-stu-id="808c5-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="808c5-132">toofinish skapa din anslutning och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="808c5-132">toofinish creating your connection, choose **Create**.</span></span>
   
    ![Integration kontoinformation för anslutning](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. <span data-ttu-id="808c5-134">När anslutningen har skapats, som visas i det här exemplet, Välj hello X12 flat fil meddelandet toodecode.</span><span class="sxs-lookup"><span data-stu-id="808c5-134">After your connection is created, as shown in this example, select hello X12 flat file message toodecode.</span></span>

    ![Integration konto anslutning som skapats](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    <span data-ttu-id="808c5-136">Exempel:</span><span class="sxs-lookup"><span data-stu-id="808c5-136">For example:</span></span>

    ![Välj X12 flat fil meddelande för avkodning av](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

## <a name="x12-decode-details"></a><span data-ttu-id="808c5-138">X12 avkoda information</span><span class="sxs-lookup"><span data-stu-id="808c5-138">X12 Decode details</span></span>

<span data-ttu-id="808c5-139">Hej X12 avkoda connector utför dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="808c5-139">hello X12 Decode connector performs these tasks:</span></span>

* <span data-ttu-id="808c5-140">Verifierar hello kuvert mot handel partneravtalet</span><span class="sxs-lookup"><span data-stu-id="808c5-140">Validates hello envelope against trading partner agreement</span></span>
* <span data-ttu-id="808c5-141">Verifierar EDI och partner-specifika egenskaper</span><span class="sxs-lookup"><span data-stu-id="808c5-141">Validates EDI and partner-specific properties</span></span>
  * <span data-ttu-id="808c5-142">Strukturella EDI-validering och utökade schemavalidering</span><span class="sxs-lookup"><span data-stu-id="808c5-142">EDI structural validation, and extended schema validation</span></span>
  * <span data-ttu-id="808c5-143">Validering av hello interchange kuvert hello struktur.</span><span class="sxs-lookup"><span data-stu-id="808c5-143">Validation of hello structure of hello interchange envelope.</span></span>
  * <span data-ttu-id="808c5-144">Schemavalideringen hello Envelope mot hello kontrollen schema.</span><span class="sxs-lookup"><span data-stu-id="808c5-144">Schema validation of hello envelope against hello control schema.</span></span>
  * <span data-ttu-id="808c5-145">Schemavalideringen av hello transaktion set dataelement mot hello Meddelandeschema.</span><span class="sxs-lookup"><span data-stu-id="808c5-145">Schema validation of hello transaction-set data elements against hello message schema.</span></span>
  * <span data-ttu-id="808c5-146">EDI-verifiering utförs på transaktion set dataelement</span><span class="sxs-lookup"><span data-stu-id="808c5-146">EDI validation performed on transaction-set data elements</span></span> 
* <span data-ttu-id="808c5-147">Verifierar att hello interchange, grupp och transaktionen set kontrollen siffror inte är dubbletter</span><span class="sxs-lookup"><span data-stu-id="808c5-147">Verifies that hello interchange, group, and transaction set control numbers are not duplicates</span></span>
  * <span data-ttu-id="808c5-148">Kontrollerar hello interchange kontrollnummer mot tidigare mottagna externa utbyten.</span><span class="sxs-lookup"><span data-stu-id="808c5-148">Checks hello interchange control number against previously received interchanges.</span></span>
  * <span data-ttu-id="808c5-149">Kontrollerar hello kontrollen gruppnumret mot andra grupp kontrollen talen i hello utbyte.</span><span class="sxs-lookup"><span data-stu-id="808c5-149">Checks hello group control number against other group control numbers in hello interchange.</span></span>
  * <span data-ttu-id="808c5-150">Kontrollerar hello transaktion ange kontroll många mot andra transaktion set kontrollen talen i den gruppen.</span><span class="sxs-lookup"><span data-stu-id="808c5-150">Checks hello transaction set control number against other transaction set control numbers in that group.</span></span>
* <span data-ttu-id="808c5-151">Delar upp hello interchange i transaktionen uppsättningar eller bevarar hello hela interchange:</span><span class="sxs-lookup"><span data-stu-id="808c5-151">Splits hello interchange into transaction sets, or preserves hello entire interchange:</span></span>
  * <span data-ttu-id="808c5-152">Dela Interchange som transaktionen uppsättningar - inaktivera transaktion anger vid fel: delningar interchange i transaktion anger och Parsar varje transaktion uppsättning.</span><span class="sxs-lookup"><span data-stu-id="808c5-152">Split Interchange as transaction sets - suspend transaction sets on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="808c5-153">Hej X12 avkoda åtgärd matar ut bara de transaktion filer som inte kan valideras för`badMessages`, och anger hello återstående transaktioner anger för`goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="808c5-153">hello X12 Decode action outputs only those transaction sets that fail validation too`badMessages`, and outputs hello remaining transactions sets too`goodMessages`.</span></span>
  * <span data-ttu-id="808c5-154">Dela Interchange som transaktionen uppsättningar - inaktivera utbyte vid fel: delningar interchange i transaktion anger och Parsar varje transaktion uppsättning.</span><span class="sxs-lookup"><span data-stu-id="808c5-154">Split Interchange as transaction sets - suspend interchange on error: Splits interchange into transaction sets and parses each transaction set.</span></span> 
  <span data-ttu-id="808c5-155">Om en eller flera transaktion anger i hello interchange misslyckas verifieringen, hello X12 avkoda åtgärd matar ut alla hello transaktion anger i den interchange för`badMessages`.</span><span class="sxs-lookup"><span data-stu-id="808c5-155">If one or more transaction sets in hello interchange fail validation, hello X12 Decode action outputs all hello transaction sets in that interchange too`badMessages`.</span></span>
  * <span data-ttu-id="808c5-156">Bevara Interchange – inaktivera transaktion anger vid fel: Preserve hello utbyte och processen hello hela gruppbaserad utbyte.</span><span class="sxs-lookup"><span data-stu-id="808c5-156">Preserve Interchange - suspend transaction sets on error: Preserve hello interchange and process hello entire batched interchange.</span></span> 
  <span data-ttu-id="808c5-157">Hej X12 avkoda åtgärd matar ut bara de transaktion filer som inte kan valideras för`badMessages`, och anger hello återstående transaktioner anger för`goodMessages`.</span><span class="sxs-lookup"><span data-stu-id="808c5-157">hello X12 Decode action outputs only those transaction sets that fail validation too`badMessages`, and outputs hello remaining transactions sets too`goodMessages`.</span></span>
  * <span data-ttu-id="808c5-158">Bevara Interchange – inaktivera utbyte vid fel: Preserve hello utbyte och processen hello hela gruppbaserad utbyte.</span><span class="sxs-lookup"><span data-stu-id="808c5-158">Preserve Interchange - suspend interchange on error: Preserve hello interchange and process hello entire batched interchange.</span></span> 
  <span data-ttu-id="808c5-159">Om en eller flera transaktion anger i hello interchange misslyckas verifieringen, hello X12 avkoda åtgärd matar ut alla hello transaktion anger i den interchange för`badMessages`.</span><span class="sxs-lookup"><span data-stu-id="808c5-159">If one or more transaction sets in hello interchange fail validation, hello X12 Decode action outputs all hello transaction sets in that interchange too`badMessages`.</span></span> 
* <span data-ttu-id="808c5-160">Genererar en tekniska och/eller funktionella bekräftelse (om konfigurerad).</span><span class="sxs-lookup"><span data-stu-id="808c5-160">Generates a Technical and/or Functional acknowledgment (if configured).</span></span>
  * <span data-ttu-id="808c5-161">En teknisk bekräftelse genererar på grund av huvud-verifiering.</span><span class="sxs-lookup"><span data-stu-id="808c5-161">A Technical Acknowledgment generates as a result of header validation.</span></span> <span data-ttu-id="808c5-162">Hej tekniska bekräftelse rapporter hello status med hello bearbetning av ett utbyte huvud och inkluderande av hello adress mottagare.</span><span class="sxs-lookup"><span data-stu-id="808c5-162">hello technical acknowledgment reports hello status of hello processing of an interchange header and trailer by hello address receiver.</span></span>
  * <span data-ttu-id="808c5-163">En funktionell bekräftelse genererar på grund av brödtext validering.</span><span class="sxs-lookup"><span data-stu-id="808c5-163">A Functional Acknowledgment generates as a result of body validation.</span></span> <span data-ttu-id="808c5-164">hello rapporterar funktionella bekräftelse varje fel uppstod under bearbetning av hello emot dokumentet</span><span class="sxs-lookup"><span data-stu-id="808c5-164">hello functional acknowledgment reports each error encountered while processing hello received document</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="808c5-165">Visa hello swagger</span><span class="sxs-lookup"><span data-stu-id="808c5-165">View hello swagger</span></span>
<span data-ttu-id="808c5-166">Se hello [swagger information](/connectors/x12/).</span><span class="sxs-lookup"><span data-stu-id="808c5-166">See hello [swagger details](/connectors/x12/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="808c5-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="808c5-167">Next steps</span></span>
[<span data-ttu-id="808c5-168">Mer information om hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="808c5-168">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket") 

