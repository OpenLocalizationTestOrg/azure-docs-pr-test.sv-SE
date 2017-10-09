---
title: "aaaCreate partners för business-to-business (B2B) meddelanden - Azure Logic Apps | Microsoft Docs"
description: "Lär dig hur tooadd partners tooyour integrering konto med hello Enterprise-Integrationspaket och Logic Apps"
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
ms.openlocfilehash: 8dc70a8f441fcf228ed178029dcdbac940d794b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a><span data-ttu-id="d6026-103">Lägg till eller uppdatera partner i business-to-business-avtalen i arbetsflödet</span><span class="sxs-lookup"><span data-stu-id="d6026-103">Add or update partners in business-to-business agreements in your workflow</span></span>

<span data-ttu-id="d6026-104">Partners är enheter som deltar i business-to-business (B2B) transaktioner och utbyte av meddelanden mellan varandra.</span><span class="sxs-lookup"><span data-stu-id="d6026-104">Partners are entities that participate in business-to-business (B2B) transactions and exchange messages between each other.</span></span> <span data-ttu-id="d6026-105">Innan du kan skapa partners som representerar du och en annan organisation i dessa transaktioner, måste du både dela information som identifierar och validerar meddelanden som skickas av varandra.</span><span class="sxs-lookup"><span data-stu-id="d6026-105">Before you can create partners that represent you and another organization in these transactions, you must both share information that identifies and validates messages sent by each other.</span></span> <span data-ttu-id="d6026-106">När du diskutera dessa uppgifter och är redo toostart business-relation, kan du skapa partners i din integrering konto toorepresent du båda.</span><span class="sxs-lookup"><span data-stu-id="d6026-106">After you discuss these details and are ready toostart your business relationship, you can create partners in your integration account toorepresent you both.</span></span>

## <a name="what-roles-do-partners-have-in-your-integration-account"></a><span data-ttu-id="d6026-107">Vilka roller har partners i integration kontot?</span><span class="sxs-lookup"><span data-stu-id="d6026-107">What roles do partners have in your integration account?</span></span>

<span data-ttu-id="d6026-108">toodefine information om hello-meddelanden som utbyts mellan partner kan du skapa avtal mellan dessa partner.</span><span class="sxs-lookup"><span data-stu-id="d6026-108">toodefine details about hello messages exchanged between partners, you create agreements between those partners.</span></span> <span data-ttu-id="d6026-109">Men innan du kan skapa ett avtal, du måste ha lagt till minst två parter tooyour integrering konto.</span><span class="sxs-lookup"><span data-stu-id="d6026-109">However, before you can create an agreement, you must have added at least two partners tooyour integration account.</span></span> <span data-ttu-id="d6026-110">Din organisation måste vara en del av hello avtal som hello **värden partner**.</span><span class="sxs-lookup"><span data-stu-id="d6026-110">Your organization must be part of hello agreement as hello **host partner**.</span></span> <span data-ttu-id="d6026-111">Hej andra partnern eller **gäst partner** representerar hello organisation som utbyter meddelanden med din organisation.</span><span class="sxs-lookup"><span data-stu-id="d6026-111">hello other partner, or **guest partner** represents hello organization that exchanges messages with your organization.</span></span> <span data-ttu-id="d6026-112">hello gäst partner kan vara ett annat företag eller även en avdelning i din organisation.</span><span class="sxs-lookup"><span data-stu-id="d6026-112">hello guest partner can be another company, or even a department in your own organization.</span></span>

<span data-ttu-id="d6026-113">När du lägger till dessa tillverkare, skapar du ett avtal.</span><span class="sxs-lookup"><span data-stu-id="d6026-113">After you add these partners, you can create an agreement.</span></span>

<span data-ttu-id="d6026-114">Ta emot och skicka inställningarna går från hello synsätt av hello Hosted Partner.</span><span class="sxs-lookup"><span data-stu-id="d6026-114">Receive and Send settings are oriented from hello point of view of hello Hosted Partner.</span></span> <span data-ttu-id="d6026-115">Hello får till exempel inställningar i ett avtal avgöra hur hello finns partner tar emot meddelanden som skickas från en gäst-partner.</span><span class="sxs-lookup"><span data-stu-id="d6026-115">For example, hello receive settings in an agreement determine how hello hosted partner receives messages sent from a guest partner.</span></span> <span data-ttu-id="d6026-116">På samma sätt anger hello skicka inställningarna på hello avtal hur hello finns kontopartner skickar meddelanden toohello gäst partner.</span><span class="sxs-lookup"><span data-stu-id="d6026-116">Likewise, hello send settings on hello agreement indicate how hello hosted partner sends messages toohello guest partner.</span></span>

## <a name="how-toocreate-a-partner"></a><span data-ttu-id="d6026-117">Hur toocreate en partner?</span><span class="sxs-lookup"><span data-stu-id="d6026-117">How toocreate a partner?</span></span>

1. <span data-ttu-id="d6026-118">Välj i hello Azure-portalen, **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="d6026-118">In hello Azure portal, select **Browse**.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. <span data-ttu-id="d6026-119">Skriv i sökrutan för hello filter **integrering**och välj **Integrationskonton** i hello resultatlistan.</span><span class="sxs-lookup"><span data-stu-id="d6026-119">In hello filter search box, enter **integration**, then select **Integration Accounts** in hello results list.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. <span data-ttu-id="d6026-120">Välj hello integration konto där du vill tooadd dina partners.</span><span class="sxs-lookup"><span data-stu-id="d6026-120">Select hello integration account where you want tooadd your partners.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. <span data-ttu-id="d6026-121">Välj hello **Partners** panelen.</span><span class="sxs-lookup"><span data-stu-id="d6026-121">Select hello **Partners** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. <span data-ttu-id="d6026-122">Hello Partners bladet välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d6026-122">In hello Partners blade, choose **Add**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. <span data-ttu-id="d6026-123">Ange ett namn för din partner och välj sedan en **kvalificerare**.</span><span class="sxs-lookup"><span data-stu-id="d6026-123">Enter a name for your partner, then select a **Qualifier**.</span></span> <span data-ttu-id="d6026-124">Du kan använda en **värdet** toohelp identifiera dokument som finns i dina appar.</span><span class="sxs-lookup"><span data-stu-id="d6026-124">Finally, enter a **Value** toohelp identify documents that come into your apps.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. <span data-ttu-id="d6026-125">toosee hello förloppet för ditt partner-processen, Välj hello *bell* meddelandeikonen.</span><span class="sxs-lookup"><span data-stu-id="d6026-125">toosee hello progress for your partner creation process, select hello *bell* notification icon.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. <span data-ttu-id="d6026-126">tooconfirm som din nya partner har lagts till, Välj hello **Partners** panelen.</span><span class="sxs-lookup"><span data-stu-id="d6026-126">tooconfirm that your new partners were successfully added, select hello **Partners** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    <span data-ttu-id="d6026-127">När du har valt hello Partners panelen visas också nytillagda partners i hello Partners bladet.</span><span class="sxs-lookup"><span data-stu-id="d6026-127">After you select hello Partners tile, you'll also see  newly added partners in hello Partners blade.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-tooedit-existing-partners-in-your-integration-account"></a><span data-ttu-id="d6026-128">Hur tooedit befintliga partners i ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="d6026-128">How tooedit existing partners in your integration account</span></span>

1. <span data-ttu-id="d6026-129">Välj hello **Partners** panelen.</span><span class="sxs-lookup"><span data-stu-id="d6026-129">Select hello **Partners** tile.</span></span>
2. <span data-ttu-id="d6026-130">När hello Partners blad öppnas, Välj hello-partner som du vill tooedit.</span><span class="sxs-lookup"><span data-stu-id="d6026-130">After hello Partners blade opens, select hello partner you want tooedit.</span></span>
3. <span data-ttu-id="d6026-131">På hello **uppdatering Partner** panelen, gör ändringarna.</span><span class="sxs-lookup"><span data-stu-id="d6026-131">On hello **Update Partner** tile, make your changes.</span></span>
4. <span data-ttu-id="d6026-132">När du är klar väljer **spara**, eller toocancel ändringarna, Välj **Ignorera**.</span><span class="sxs-lookup"><span data-stu-id="d6026-132">After you're done, choose **Save**, or toocancel your changes, select **Discard**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-toodelete-a-partner"></a><span data-ttu-id="d6026-133">Hur toodelete en partner</span><span class="sxs-lookup"><span data-stu-id="d6026-133">How toodelete a partner</span></span>

1. <span data-ttu-id="d6026-134">Välj hello **Partners** panelen.</span><span class="sxs-lookup"><span data-stu-id="d6026-134">Select hello **Partners** tile.</span></span>
2. <span data-ttu-id="d6026-135">Välj hello-partner som du vill toodelete efter hello Partner blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="d6026-135">After hello Partner blade opens, select hello partner that you want toodelete.</span></span>
3. <span data-ttu-id="d6026-136">Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="d6026-136">Choose **Delete**.</span></span>

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a><span data-ttu-id="d6026-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6026-137">Next steps</span></span>
* [<span data-ttu-id="d6026-138">Mer information om avtalen</span><span class="sxs-lookup"><span data-stu-id="d6026-138">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")  

