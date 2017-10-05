---
title: "Skapa lösningar för B2B - Azure Logic Apps | Microsoft Docs"
description: "Ta emot data i logikappar med hjälp av B2B-funktioner i Enterprise-Integrationspaket"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 20fc3722-6f8b-402f-b391-b84e9df6fcff
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 0625787ddcbc0091e70b111f687e25929720ad15
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="receive-data-in-logic-apps-with-the-b2b-features-in-the-enterprise-integration-pack"></a><span data-ttu-id="c42de-103">Ta emot data i logikappar med B2B-funktioner i Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="c42de-103">Receive data in logic apps with the B2B features in the Enterprise Integration Pack</span></span>

<span data-ttu-id="c42de-104">När du har skapat ett konto för integrering med partners och avtal du är redo att skapa ett företag att (B2B) arbetsflöde för din logikapp med den [Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c42de-104">After you create an integration account that has partners and agreements, you are ready to create a business to business (B2B) workflow for your logic app with the [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c42de-105">Krav</span><span class="sxs-lookup"><span data-stu-id="c42de-105">Prerequisites</span></span>

<span data-ttu-id="c42de-106">Att använda AS2 och X12 åtgärder, du måste ha ett Enterprise Integration-konto.</span><span class="sxs-lookup"><span data-stu-id="c42de-106">To use the AS2 and X12 actions, you must have an Enterprise Integration Account.</span></span> <span data-ttu-id="c42de-107">Läs [hur du skapar ett konto för Enterprise Integration](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="c42de-107">Learn [how to create an Enterprise Integration Account](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span></span>

## <a name="create-a-logic-app-with-b2b-connectors"></a><span data-ttu-id="c42de-108">Skapa en logikapp med B2B-kopplingar</span><span class="sxs-lookup"><span data-stu-id="c42de-108">Create a logic app with B2B connectors</span></span>

<span data-ttu-id="c42de-109">Följ dessa steg om du vill skapa en B2B-logikapp som använder AS2 och X12 åtgärder för att ta emot data från en handelspartner:</span><span class="sxs-lookup"><span data-stu-id="c42de-109">Follow these steps to create a B2B logic app that uses the AS2 and X12 actions to receive data from a trading partner:</span></span>

1. <span data-ttu-id="c42de-110">Skapa en logikapp sedan [länka din app till ditt konto för integrering](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="c42de-110">Create a logic app, then [link your app to your integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span></span>

2. <span data-ttu-id="c42de-111">Lägg till en **begäran - när en HTTP-begäran tas emot** utlösaren till din logikapp.</span><span class="sxs-lookup"><span data-stu-id="c42de-111">Add a **Request - When an HTTP request is received** trigger to your logic app.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. <span data-ttu-id="c42de-112">Att lägga till den **avkoda AS2** åtgärd, Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="c42de-112">To add the **Decode AS2** action, select **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. <span data-ttu-id="c42de-113">Om du vill filtrera alla åtgärder för att det som du vill ange ordet **as2** i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="c42de-113">To filter all actions to the one that you want, enter the word **as2** in the search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. <span data-ttu-id="c42de-114">Välj den **AS2 - avkoda AS2-meddelandet** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="c42de-114">Select the **AS2 - Decode AS2 message** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. <span data-ttu-id="c42de-115">Lägg till den **brödtext** som du vill använda som indata.</span><span class="sxs-lookup"><span data-stu-id="c42de-115">Add the **Body** that you want to use as input.</span></span> <span data-ttu-id="c42de-116">I det här exemplet väljer du innehållet i HTTP-begäran som utlöser logikappen.</span><span class="sxs-lookup"><span data-stu-id="c42de-116">In this example, select the body of the HTTP request that triggers the logic app.</span></span> <span data-ttu-id="c42de-117">Eller ange ett uttryck som anger huvudena i den **HUVUDEN** fält:</span><span class="sxs-lookup"><span data-stu-id="c42de-117">Or enter an expression that inputs the headers in the **HEADERS** field:</span></span>

    <span data-ttu-id="c42de-118">@triggerOutputs() [huvuden]</span><span class="sxs-lookup"><span data-stu-id="c42de-118">@triggerOutputs()['headers']</span></span>

7. <span data-ttu-id="c42de-119">Lägg till de nödvändiga **huvuden** för AS2-, som du hittar i HTTP-huvuden för begäran.</span><span class="sxs-lookup"><span data-stu-id="c42de-119">Add the required **Headers** for AS2, which you can find in the HTTP request headers.</span></span> <span data-ttu-id="c42de-120">I det här exemplet väljer du till HTTP-huvuden som utlöser logikappen.</span><span class="sxs-lookup"><span data-stu-id="c42de-120">In this example, select the headers of the HTTP request that trigger the logic app.</span></span>

8. <span data-ttu-id="c42de-121">Nu ska du lägga till åtgärden för avkoda X12-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c42de-121">Now add the Decode X12 message action.</span></span> <span data-ttu-id="c42de-122">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="c42de-122">Select **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. <span data-ttu-id="c42de-123">Om du vill filtrera alla åtgärder för att det som du vill ange ordet **x12** i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="c42de-123">To filter all actions to the one that you want, enter the word **x12** in the search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. <span data-ttu-id="c42de-124">Välj den **X12-avkoda X12 meddelandet** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="c42de-124">Select the **X12 - Decode X12 message** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. <span data-ttu-id="c42de-125">Nu måste du ange indata för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="c42de-125">Now you must specify the input to this action.</span></span> <span data-ttu-id="c42de-126">Den här informationen är utdata från den föregående AS2-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="c42de-126">This input is the output from the previous AS2 action.</span></span>

    <span data-ttu-id="c42de-127">Det faktiska meddelandeinnehållet är i ett JSON-objekt och är base64-kodad, så du måste ange ett uttryck som indata.</span><span class="sxs-lookup"><span data-stu-id="c42de-127">The actual message content is in a JSON object and is base64 encoded, so you must specify an expression as the input.</span></span> 
    <span data-ttu-id="c42de-128">Ange följande uttryck i den **X12 FLAT fil meddelandet till avkoda** inmatningsfältet:</span><span class="sxs-lookup"><span data-stu-id="c42de-128">Enter the following expression in the **X12 FLAT FILE MESSAGE TO DECODE** input field:</span></span>
    
    <span data-ttu-id="c42de-129">@base64ToString(body('Decode_AS2_message')? ['AS2Message']? ['Content'])</span><span class="sxs-lookup"><span data-stu-id="c42de-129">@base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])</span></span>

    <span data-ttu-id="c42de-130">Nu ska du lägga till stegen för att avkoda X12 data togs emot från handelspartner och utdata objekt i en JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="c42de-130">Now add steps to decode the X12 data received from the trading partner and output items in a JSON object.</span></span> 
    <span data-ttu-id="c42de-131">För att meddela partnern att data har tagits emot, kan du skicka tillbaka ett svar som innehåller Disposition Notification AS2 meddelande (MDN) i en åtgärd för HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="c42de-131">To notify the partner that the data was received, you can send back a response containing the AS2 Message Disposition Notification (MDN) in an HTTP Response Action.</span></span>

12. <span data-ttu-id="c42de-132">Att lägga till den **svar** åtgärd, Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="c42de-132">To add the **Response** action, choose **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. <span data-ttu-id="c42de-133">Om du vill filtrera alla åtgärder för att det som du vill ange ordet **svar** i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="c42de-133">To filter all actions to the one that you want, enter the word **response** in the search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. <span data-ttu-id="c42de-134">Välj den **svar** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="c42de-134">Select the **Response** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. <span data-ttu-id="c42de-135">Åtkomst till MDN från utdata från den **avkoda X12 meddelandet** åtgärd, ange svaret **BRÖDTEXT** med det här uttrycket:</span><span class="sxs-lookup"><span data-stu-id="c42de-135">To access the MDN from the output of the **Decode X12 message** action, set the response **BODY** field with this expression:</span></span>

    <span data-ttu-id="c42de-136">@base64ToString(body('Decode_AS2_message')? ['OutgoingMdn']? ['Content'])</span><span class="sxs-lookup"><span data-stu-id="c42de-136">@base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. <span data-ttu-id="c42de-137">Spara ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="c42de-137">Save your work.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

<span data-ttu-id="c42de-138">Du är nu klar att konfigurera logikappen B2B.</span><span class="sxs-lookup"><span data-stu-id="c42de-138">You are now done setting up your B2B logic app.</span></span> <span data-ttu-id="c42de-139">I ett verkligt program kanske du vill lagra den avkodade X12 data i en line-of-business (LOB) app eller data store.</span><span class="sxs-lookup"><span data-stu-id="c42de-139">In a real world application, you might want to store the decoded X12 data in a line-of-business (LOB) app or data store.</span></span> <span data-ttu-id="c42de-140">Du kan lägga till ytterligare åtgärder för att ansluta dina egna LOB-appar och använda dessa API: er i din logikapp, eller skriva anpassade API: er.</span><span class="sxs-lookup"><span data-stu-id="c42de-140">To connect your own LOB apps and use these APIs in your logic app, you can add further actions or write custom APIs.</span></span>

## <a name="features-and-use-cases"></a><span data-ttu-id="c42de-141">Funktioner och användningsområden</span><span class="sxs-lookup"><span data-stu-id="c42de-141">Features and use cases</span></span>

* <span data-ttu-id="c42de-142">AS2 X12 avkoda och koda åtgärder kan du utbyta data mellan handelspartner med standardprotokollen i logikappar.</span><span class="sxs-lookup"><span data-stu-id="c42de-142">The AS2 and X12 decode and encode actions let you exchange data between trading partners by using industry standard protocols in logic apps.</span></span>
* <span data-ttu-id="c42de-143">Om du vill utbyta data med handelspartner kan du använda AS2 och X12 med eller utan varandra.</span><span class="sxs-lookup"><span data-stu-id="c42de-143">To exchange data with trading partners, you can use AS2 and X12 with or without each other.</span></span>
* <span data-ttu-id="c42de-144">B2B-åtgärder kan du enkelt skapa partners och avtal i kontot för integrering och använda dem i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="c42de-144">The B2B actions help you create partners and agreements easily in your integration account and consume them in a logic app.</span></span>
* <span data-ttu-id="c42de-145">När du utökar din logikapp med andra åtgärder kan du skicka och ta emot data mellan andra appar och tjänster som SalesForce.</span><span class="sxs-lookup"><span data-stu-id="c42de-145">When you extend your logic app with other actions, you can send and receive data between other apps and services like SalesForce.</span></span>

## <a name="learn-more"></a><span data-ttu-id="c42de-146">Läs mer</span><span class="sxs-lookup"><span data-stu-id="c42de-146">Learn more</span></span>
[<span data-ttu-id="c42de-147">Mer information om Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="c42de-147">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md)
