---
title: "Skapa partners för business-to-business (B2B) meddelanden - Azure Logic Apps | Microsoft Docs"
description: "Lär dig hur du lägger till partners till ditt konto för integrering med Logic Apps och Enterprise-Integrationspaket"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 950cb449b53f400f0f0f860caf5415bbb5212269
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a><span data-ttu-id="f372b-103">Lägg till eller uppdatera partner i business-to-business-avtalen i arbetsflödet</span><span class="sxs-lookup"><span data-stu-id="f372b-103">Add or update partners in business-to-business agreements in your workflow</span></span>

<span data-ttu-id="f372b-104">Partners är enheter som deltar i business-to-business (B2B) transaktioner och utbyte av meddelanden mellan varandra.</span><span class="sxs-lookup"><span data-stu-id="f372b-104">Partners are entities that participate in business-to-business (B2B) transactions and exchange messages between each other.</span></span> <span data-ttu-id="f372b-105">Innan du kan skapa partners som representerar du och en annan organisation i dessa transaktioner, måste du både dela information som identifierar och validerar meddelanden som skickas av varandra.</span><span class="sxs-lookup"><span data-stu-id="f372b-105">Before you can create partners that represent you and another organization in these transactions, you must both share information that identifies and validates messages sent by each other.</span></span> <span data-ttu-id="f372b-106">Du kan skapa partners i ditt konto integration som representerar du båda när du diskutera dessa uppgifter och är redo att börja business-relation.</span><span class="sxs-lookup"><span data-stu-id="f372b-106">After you discuss these details and are ready to start your business relationship, you can create partners in your integration account to represent you both.</span></span>

## <a name="what-roles-do-partners-have-in-your-integration-account"></a><span data-ttu-id="f372b-107">Vilka roller har partners i integration kontot?</span><span class="sxs-lookup"><span data-stu-id="f372b-107">What roles do partners have in your integration account?</span></span>

<span data-ttu-id="f372b-108">Om du vill definiera information om meddelanden som utbyts mellan partners, kan du skapa avtal mellan dessa partner.</span><span class="sxs-lookup"><span data-stu-id="f372b-108">To define details about the messages exchanged between partners, you create agreements between those partners.</span></span> <span data-ttu-id="f372b-109">Men innan du kan skapa ett avtal, du måste ha lagt till minst två parter till ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="f372b-109">However, before you can create an agreement, you must have added at least two partners to your integration account.</span></span> <span data-ttu-id="f372b-110">Din organisation måste vara en del av avtal som den **värden partner**.</span><span class="sxs-lookup"><span data-stu-id="f372b-110">Your organization must be part of the agreement as the **host partner**.</span></span> <span data-ttu-id="f372b-111">Den andra partnern eller **gäst partner** representerar den organisation som utbyter meddelanden med din organisation.</span><span class="sxs-lookup"><span data-stu-id="f372b-111">The other partner, or **guest partner** represents the organization that exchanges messages with your organization.</span></span> <span data-ttu-id="f372b-112">Gäst-partner kan vara ett annat företag eller även en avdelning i din organisation.</span><span class="sxs-lookup"><span data-stu-id="f372b-112">The guest partner can be another company, or even a department in your own organization.</span></span>

<span data-ttu-id="f372b-113">När du lägger till dessa tillverkare, skapar du ett avtal.</span><span class="sxs-lookup"><span data-stu-id="f372b-113">After you add these partners, you can create an agreement.</span></span>

<span data-ttu-id="f372b-114">Ta emot och skicka inställningarna går från synvinkel för den Partner som värd.</span><span class="sxs-lookup"><span data-stu-id="f372b-114">Receive and Send settings are oriented from the point of view of the Hosted Partner.</span></span> <span data-ttu-id="f372b-115">Exempelvis avgör ta emot inställningarna i ett avtal hur värdbaserade partnern tar emot meddelanden som skickas från en gäst-partner.</span><span class="sxs-lookup"><span data-stu-id="f372b-115">For example, the receive settings in an agreement determine how the hosted partner receives messages sent from a guest partner.</span></span> <span data-ttu-id="f372b-116">På samma sätt anger skicka inställningarna i avtalet hur värdbaserade partnern skickar meddelanden till gäst-partner.</span><span class="sxs-lookup"><span data-stu-id="f372b-116">Likewise, the send settings on the agreement indicate how the hosted partner sends messages to the guest partner.</span></span>

## <a name="how-to-create-a-partner"></a><span data-ttu-id="f372b-117">Så här skapar du en partner?</span><span class="sxs-lookup"><span data-stu-id="f372b-117">How to create a partner?</span></span>

1. <span data-ttu-id="f372b-118">Välj i Azure-portalen **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="f372b-118">In the Azure portal, select **Browse**.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. <span data-ttu-id="f372b-119">Skriv i sökrutan filtrera **integrering**och välj **Integrationskonton** i resultatlistan.</span><span class="sxs-lookup"><span data-stu-id="f372b-119">In the filter search box, enter **integration**, then select **Integration Accounts** in the results list.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. <span data-ttu-id="f372b-120">Välj integration konto där du vill lägga till dina partners.</span><span class="sxs-lookup"><span data-stu-id="f372b-120">Select the integration account where you want to add your partners.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. <span data-ttu-id="f372b-121">Välj den **Partners** panelen.</span><span class="sxs-lookup"><span data-stu-id="f372b-121">Select the **Partners** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. <span data-ttu-id="f372b-122">I bladet Partners väljer **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f372b-122">In the Partners blade, choose **Add**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. <span data-ttu-id="f372b-123">Ange ett namn för din partner och välj sedan en **kvalificerare**.</span><span class="sxs-lookup"><span data-stu-id="f372b-123">Enter a name for your partner, then select a **Qualifier**.</span></span> <span data-ttu-id="f372b-124">Du kan använda en **värdet** för att identifiera dokument som finns i dina appar.</span><span class="sxs-lookup"><span data-stu-id="f372b-124">Finally, enter a **Value** to help identify documents that come into your apps.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. <span data-ttu-id="f372b-125">Om du vill se förloppet för ditt partner-processen, Välj den *bell* meddelandeikonen.</span><span class="sxs-lookup"><span data-stu-id="f372b-125">To see the progress for your partner creation process, select the *bell* notification icon.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. <span data-ttu-id="f372b-126">För att bekräfta att din nya partner har lagts till, Välj den **Partners** panelen.</span><span class="sxs-lookup"><span data-stu-id="f372b-126">To confirm that your new partners were successfully added, select the **Partners** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    <span data-ttu-id="f372b-127">När du har valt Partners panelen visas också nytillagda partners i bladet Partners.</span><span class="sxs-lookup"><span data-stu-id="f372b-127">After you select the Partners tile, you'll also see  newly added partners in the Partners blade.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-to-edit-existing-partners-in-your-integration-account"></a><span data-ttu-id="f372b-128">Hur du redigerar befintliga partners i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="f372b-128">How to edit existing partners in your integration account</span></span>

1. <span data-ttu-id="f372b-129">Välj den **Partners** panelen.</span><span class="sxs-lookup"><span data-stu-id="f372b-129">Select the **Partners** tile.</span></span>
2. <span data-ttu-id="f372b-130">När det öppnas bladet Partners, markerar du den partner som du vill redigera.</span><span class="sxs-lookup"><span data-stu-id="f372b-130">After the Partners blade opens, select the partner you want to edit.</span></span>
3. <span data-ttu-id="f372b-131">På den **uppdatering Partner** panelen, gör ändringarna.</span><span class="sxs-lookup"><span data-stu-id="f372b-131">On the **Update Partner** tile, make your changes.</span></span>
4. <span data-ttu-id="f372b-132">När du är klar väljer **spara**, eller om du vill avbryta ändringarna, Välj **Ignorera**.</span><span class="sxs-lookup"><span data-stu-id="f372b-132">After you're done, choose **Save**, or to cancel your changes, select **Discard**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-to-delete-a-partner"></a><span data-ttu-id="f372b-133">Hur du tar bort en partner</span><span class="sxs-lookup"><span data-stu-id="f372b-133">How to delete a partner</span></span>

1. <span data-ttu-id="f372b-134">Välj den **Partners** panelen.</span><span class="sxs-lookup"><span data-stu-id="f372b-134">Select the **Partners** tile.</span></span>
2. <span data-ttu-id="f372b-135">När det öppnas bladet Partner, markerar du den partner som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="f372b-135">After the Partner blade opens, select the partner that you want to delete.</span></span>
3. <span data-ttu-id="f372b-136">Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f372b-136">Choose **Delete**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a><span data-ttu-id="f372b-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f372b-137">Next steps</span></span>
* [<span data-ttu-id="f372b-138">Mer information om avtalen</span><span class="sxs-lookup"><span data-stu-id="f372b-138">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")  

