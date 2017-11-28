---
title: aaaDecode AS2 - meddelanden i Azure Logic Apps | Microsoft Docs
description: "Hur toouse hello AS2 avkodarens i hello Enterprise-Integrationspaket för Logikappar i Azure"
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
ms.openlocfilehash: 2406e5ec68e0906700fad97d60cb83ef0d106cd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="decode-as2-messages-for-azure-logic-apps-with-hello-enterprise-integration-pack"></a><span data-ttu-id="998f7-103">Avkoda AS2-meddelanden för Logikappar i Azure med hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="998f7-103">Decode AS2 messages for Azure Logic Apps with hello Enterprise Integration Pack</span></span> 

<span data-ttu-id="998f7-104">tooestablish säkerheten och pålitligheten vid överföring av meddelanden, Använd hello avkoda AS2-koppling för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="998f7-104">tooestablish security and reliability while transmitting messages, use hello Decode AS2 message connector.</span></span> <span data-ttu-id="998f7-105">Den här anslutningen ger digital signering dekryptering och bekräftelser via meddelandet Disposition meddelanden (MDN).</span><span class="sxs-lookup"><span data-stu-id="998f7-105">This connector provides digital signing, decryption, and acknowledgements through Message Disposition Notifications (MDN).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="998f7-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="998f7-106">Before you start</span></span>

<span data-ttu-id="998f7-107">Här är hello-objekt som du behöver:</span><span class="sxs-lookup"><span data-stu-id="998f7-107">Here's hello items you need:</span></span>

* <span data-ttu-id="998f7-108">Ett Azure-konto; Du kan skapa en [kostnadsfritt konto](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="998f7-108">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="998f7-109">En [integrering konto](logic-apps-enterprise-integration-create-integration-account.md) som redan har definierat och som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="998f7-109">An [integration account](logic-apps-enterprise-integration-create-integration-account.md) that's already defined and associated with your Azure subscription.</span></span> <span data-ttu-id="998f7-110">Du måste ha en integration konto toouse hello avkoda AS2 meddelande-koppling.</span><span class="sxs-lookup"><span data-stu-id="998f7-110">You must have an integration account toouse hello Decode AS2 message connector.</span></span>
* <span data-ttu-id="998f7-111">Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="998f7-111">At least two [partners](logic-apps-enterprise-integration-partners.md) that are already defined in your integration account</span></span>
* <span data-ttu-id="998f7-112">En [AS2-avtal](logic-apps-enterprise-integration-as2.md) som redan har definierats i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="998f7-112">An [AS2 agreement](logic-apps-enterprise-integration-as2.md) that's already defined in your integration account</span></span>

## <a name="decode-as2-messages"></a><span data-ttu-id="998f7-113">Avkoda AS2-meddelanden</span><span class="sxs-lookup"><span data-stu-id="998f7-113">Decode AS2 messages</span></span>

1. <span data-ttu-id="998f7-114">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="998f7-114">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="998f7-115">hello avkoda AS2-koppling för meddelandet har utlösare, så måste du lägga till en utlösare för att starta logikappen som en utlösare för begäran.</span><span class="sxs-lookup"><span data-stu-id="998f7-115">hello Decode AS2 message connector doesn't have triggers, so you must add a trigger for starting your logic app, like a Request trigger.</span></span> <span data-ttu-id="998f7-116">Lägg till en utlösare hello logik App Designer, och sedan lägga till en åtgärd tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="998f7-116">In hello Logic App Designer, add a trigger, and then add an action tooyour logic app.</span></span>

3.  <span data-ttu-id="998f7-117">Ange ”AS2” i sökrutan hello för filtret.</span><span class="sxs-lookup"><span data-stu-id="998f7-117">In hello search box, enter "AS2" for your filter.</span></span> <span data-ttu-id="998f7-118">Välj **AS2 - avkoda AS2-meddelandet**.</span><span class="sxs-lookup"><span data-stu-id="998f7-118">Select **AS2 - Decode AS2 message**.</span></span>
   
    ![Sök efter ”AS2”](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)

4. <span data-ttu-id="998f7-120">Om du tidigare inte har skapat alla anslutningar tooyour integrering konto uppmanas du toocreate anslutningen nu.</span><span class="sxs-lookup"><span data-stu-id="998f7-120">If you didn't previously create any connections tooyour integration account, you're prompted toocreate that connection now.</span></span> <span data-ttu-id="998f7-121">Namnge din anslutning och välj hello integration konto som du vill tooconnect.</span><span class="sxs-lookup"><span data-stu-id="998f7-121">Name your connection, and select hello integration account that you want tooconnect.</span></span>
   
    ![Skapa integration anslutning](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)

    <span data-ttu-id="998f7-123">Egenskaper med en asterisk krävs.</span><span class="sxs-lookup"><span data-stu-id="998f7-123">Properties with an asterisk are required.</span></span>

    | <span data-ttu-id="998f7-124">Egenskap</span><span class="sxs-lookup"><span data-stu-id="998f7-124">Property</span></span> | <span data-ttu-id="998f7-125">Information</span><span class="sxs-lookup"><span data-stu-id="998f7-125">Details</span></span> |
    | --- | --- |
    | <span data-ttu-id="998f7-126">Anslutningsnamn *</span><span class="sxs-lookup"><span data-stu-id="998f7-126">Connection Name *</span></span> |<span data-ttu-id="998f7-127">Ange ett namn för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="998f7-127">Enter any name for your connection.</span></span> |
    | <span data-ttu-id="998f7-128">Integration konto *</span><span class="sxs-lookup"><span data-stu-id="998f7-128">Integration Account *</span></span> |<span data-ttu-id="998f7-129">Ange ett namn för ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="998f7-129">Enter a name for your integration account.</span></span> <span data-ttu-id="998f7-130">Se till att appen integration konto och logik finns i hello samma Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="998f7-130">Make sure that your integration account and logic app are in hello same Azure location.</span></span> |

5.  <span data-ttu-id="998f7-131">När du är klar din innehållet bör se ut liknande toothis exempel.</span><span class="sxs-lookup"><span data-stu-id="998f7-131">When you're done, your connection details should look similar toothis example.</span></span> <span data-ttu-id="998f7-132">toofinish skapa din anslutning och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="998f7-132">toofinish creating your connection, choose **Create**.</span></span>

    ![Integration anslutningsinformation](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)

6. <span data-ttu-id="998f7-134">När anslutningen har skapats, som visas i det här exemplet, Välj **brödtext** och **huvuden** från hello begära utdata.</span><span class="sxs-lookup"><span data-stu-id="998f7-134">After your connection is created, as shown in this example, select **Body** and **Headers** from hello Request outputs.</span></span>
   
    ![Integration anslutning som har skapats](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 

    <span data-ttu-id="998f7-136">Exempel:</span><span class="sxs-lookup"><span data-stu-id="998f7-136">For example:</span></span>

    ![Välj brödtext och rubriker från utdata för begäran](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 

## <a name="as2-decoder-details"></a><span data-ttu-id="998f7-138">AS2 avkodarens information</span><span class="sxs-lookup"><span data-stu-id="998f7-138">AS2 decoder details</span></span>

<span data-ttu-id="998f7-139">hello avkoda AS2-koppling utför dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="998f7-139">hello Decode AS2 connector performs these tasks:</span></span> 

* <span data-ttu-id="998f7-140">Bearbetar AS2/HTTP-huvuden</span><span class="sxs-lookup"><span data-stu-id="998f7-140">Processes AS2/HTTP headers</span></span>
* <span data-ttu-id="998f7-141">Verifierar hello signatur (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="998f7-141">Verifies hello signature (if configured)</span></span>
* <span data-ttu-id="998f7-142">Dekrypterar hälsningsmeddelande (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="998f7-142">Decrypts hello messages (if configured)</span></span>
* <span data-ttu-id="998f7-143">Expanderar hello-meddelande (om konfigurerad)</span><span class="sxs-lookup"><span data-stu-id="998f7-143">Decompresses hello message (if configured)</span></span>
* <span data-ttu-id="998f7-144">Synkroniserar en mottagna MDN med ursprungliga utgående hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="998f7-144">Reconciles a received MDN with hello original outbound message</span></span>
* <span data-ttu-id="998f7-145">Uppdateringar och korrelerar poster i hello oavvislighet databas</span><span class="sxs-lookup"><span data-stu-id="998f7-145">Updates and correlates records in hello non-repudiation database</span></span>
* <span data-ttu-id="998f7-146">Skriver poster för AS2 statusrapportering</span><span class="sxs-lookup"><span data-stu-id="998f7-146">Writes records for AS2 status reporting</span></span>
* <span data-ttu-id="998f7-147">hello utdata nyttolast innehållet är base64-kodade</span><span class="sxs-lookup"><span data-stu-id="998f7-147">hello output payload contents are base64 encoded</span></span>
* <span data-ttu-id="998f7-148">Anger om en MDN krävs, och om hello MDN ska synkron eller asynkron baserat på konfigurationen i AS2-avtal</span><span class="sxs-lookup"><span data-stu-id="998f7-148">Determines whether an MDN is required, and whether hello MDN should be synchronous or asynchronous based on configuration in AS2 agreement</span></span>
* <span data-ttu-id="998f7-149">Genererar en synkron eller asynkron MDN (baserat på avtal konfigurationer)</span><span class="sxs-lookup"><span data-stu-id="998f7-149">Generates a synchronous or asynchronous MDN (based on agreement configurations)</span></span>
* <span data-ttu-id="998f7-150">Anger hello korrelation token och egenskaper för hello MDN</span><span class="sxs-lookup"><span data-stu-id="998f7-150">Sets hello correlation tokens and properties on hello MDN</span></span>

## <a name="try-this-sample"></a><span data-ttu-id="998f7-151">Försök med det här exemplet</span><span class="sxs-lookup"><span data-stu-id="998f7-151">Try this sample</span></span>

<span data-ttu-id="998f7-152">distribuera ett fullt fungerande logik appen och exempel AS2-scenario tootry finns hello [AS2 logik appmallen och scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span><span class="sxs-lookup"><span data-stu-id="998f7-152">tootry deploying a fully operational logic app and sample AS2 scenario, see hello [AS2 logic app template and scenario](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="998f7-153">Visa hello swagger</span><span class="sxs-lookup"><span data-stu-id="998f7-153">View hello swagger</span></span>
<span data-ttu-id="998f7-154">Se hello [swagger information](/connectors/as2/).</span><span class="sxs-lookup"><span data-stu-id="998f7-154">See hello [swagger details](/connectors/as2/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="998f7-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="998f7-155">Next steps</span></span>
[<span data-ttu-id="998f7-156">Mer information om hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="998f7-156">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md) 

