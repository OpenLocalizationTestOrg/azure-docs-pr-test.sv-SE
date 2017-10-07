---
title: "lösningar för aaaCreate B2B - Azure Logic Apps | Microsoft Docs"
description: "Ta emot data i logikappar med hjälp av hello B2B-funktioner i hello Enterprise-Integrationspaket"
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
ms.openlocfilehash: 8f01318a0415d81c37b216f9b991c060edec2053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="receive-data-in-logic-apps-with-hello-b2b-features-in-hello-enterprise-integration-pack"></a><span data-ttu-id="ba5cb-103">Ta emot data i logikappar med hello B2B-funktioner i hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="ba5cb-103">Receive data in logic apps with hello B2B features in hello Enterprise Integration Pack</span></span>

<span data-ttu-id="ba5cb-104">När du har skapat ett konto för integrering med partners och avtal du är klar toocreate ett företag toobusiness (B2B) arbetsflöde för din logikapp med hello [Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ba5cb-104">After you create an integration account that has partners and agreements, you are ready toocreate a business toobusiness (B2B) workflow for your logic app with hello [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba5cb-105">Krav</span><span class="sxs-lookup"><span data-stu-id="ba5cb-105">Prerequisites</span></span>

<span data-ttu-id="ba5cb-106">toouse hello AS2 och X12 åtgärder, du måste ha ett Enterprise Integration-konto.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-106">toouse hello AS2 and X12 actions, you must have an Enterprise Integration Account.</span></span> <span data-ttu-id="ba5cb-107">Läs [hur toocreate en Enterprise-Integration kontot](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="ba5cb-107">Learn [how toocreate an Enterprise Integration Account](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span></span>

## <a name="create-a-logic-app-with-b2b-connectors"></a><span data-ttu-id="ba5cb-108">Skapa en logikapp med B2B-kopplingar</span><span class="sxs-lookup"><span data-stu-id="ba5cb-108">Create a logic app with B2B connectors</span></span>

<span data-ttu-id="ba5cb-109">Följ dessa steg toocreate en B2B-logikappen som använder hello AS2 och X12 åtgärder tooreceive data från en handelspartner:</span><span class="sxs-lookup"><span data-stu-id="ba5cb-109">Follow these steps toocreate a B2B logic app that uses hello AS2 and X12 actions tooreceive data from a trading partner:</span></span>

1. <span data-ttu-id="ba5cb-110">Skapa en logikapp sedan [länka ditt konto med app tooyour integrering](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="ba5cb-110">Create a logic app, then [link your app tooyour integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md).</span></span>

2. <span data-ttu-id="ba5cb-111">Lägg till en **begäran - när en HTTP-begäran tas emot** utlösaren tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-111">Add a **Request - When an HTTP request is received** trigger tooyour logic app.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. <span data-ttu-id="ba5cb-112">tooadd hello **avkoda AS2** åtgärd, Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-112">tooadd hello **Decode AS2** action, select **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. <span data-ttu-id="ba5cb-113">toofilter alla åtgärder toohello som du vill ange hello word **as2** i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-113">toofilter all actions toohello one that you want, enter hello word **as2** in hello search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. <span data-ttu-id="ba5cb-114">Välj hello **AS2 - avkoda AS2-meddelandet** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-114">Select hello **AS2 - Decode AS2 message** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. <span data-ttu-id="ba5cb-115">Lägg till hello **brödtext** som du vill toouse som indata.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-115">Add hello **Body** that you want toouse as input.</span></span> <span data-ttu-id="ba5cb-116">I det här exemplet väljer du hello brödtext hello HTTP-begäran att utlösare hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-116">In this example, select hello body of hello HTTP request that triggers hello logic app.</span></span> <span data-ttu-id="ba5cb-117">Eller ange ett uttryck som anger hello huvuden i hello **HUVUDEN** fält:</span><span class="sxs-lookup"><span data-stu-id="ba5cb-117">Or enter an expression that inputs hello headers in hello **HEADERS** field:</span></span>

    <span data-ttu-id="ba5cb-118">@triggerOutputs() [huvuden]</span><span class="sxs-lookup"><span data-stu-id="ba5cb-118">@triggerOutputs()['headers']</span></span>

7. <span data-ttu-id="ba5cb-119">Lägg till hello krävs **huvuden** för AS2-, som du hittar i hello HTTP-huvuden för begäran.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-119">Add hello required **Headers** for AS2, which you can find in hello HTTP request headers.</span></span> <span data-ttu-id="ba5cb-120">I det här exemplet väljer du hello-huvuden hello HTTP-begäran som utlösare hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-120">In this example, select hello headers of hello HTTP request that trigger hello logic app.</span></span>

8. <span data-ttu-id="ba5cb-121">Nu ska du lägga till åtgärden för hello avkoda X12 meddelande.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-121">Now add hello Decode X12 message action.</span></span> <span data-ttu-id="ba5cb-122">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-122">Select **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. <span data-ttu-id="ba5cb-123">toofilter alla åtgärder toohello som du vill ange hello word **x12** i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-123">toofilter all actions toohello one that you want, enter hello word **x12** in hello search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. <span data-ttu-id="ba5cb-124">Välj hello **X12-avkoda X12 meddelandet** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-124">Select hello **X12 - Decode X12 message** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. <span data-ttu-id="ba5cb-125">Nu måste du ange hello inkommande toothis åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-125">Now you must specify hello input toothis action.</span></span> <span data-ttu-id="ba5cb-126">Den här indata är hello utdata från hello föregående AS2-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-126">This input is hello output from hello previous AS2 action.</span></span>

    <span data-ttu-id="ba5cb-127">hello faktiska meddelandeinnehåll är base64-kodad, så du måste ange ett uttryck som indata för hello är i ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-127">hello actual message content is in a JSON object and is base64 encoded, so you must specify an expression as hello input.</span></span> 
    <span data-ttu-id="ba5cb-128">Ange följande uttryck i hello hello **X12 FLAT fil meddelandet tooDECODE** inmatningsfältet:</span><span class="sxs-lookup"><span data-stu-id="ba5cb-128">Enter hello following expression in hello **X12 FLAT FILE MESSAGE tooDECODE** input field:</span></span>
    
    <span data-ttu-id="ba5cb-129">@base64ToString(body('Decode_AS2_message')? ['AS2Message']? ['Content'])</span><span class="sxs-lookup"><span data-stu-id="ba5cb-129">@base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])</span></span>

    <span data-ttu-id="ba5cb-130">Nu ska du lägga till steg toodecode hello X12 data togs emot från hello handelspartner och utdata objekt i en JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-130">Now add steps toodecode hello X12 data received from hello trading partner and output items in a JSON object.</span></span> 
    <span data-ttu-id="ba5cb-131">toonotify hello partner som hello data togs emot, kan du skicka tillbaka ett svar som innehåller hello AS2 meddelandet Disposition meddelande (MDN) i en åtgärd för HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-131">toonotify hello partner that hello data was received, you can send back a response containing hello AS2 Message Disposition Notification (MDN) in an HTTP Response Action.</span></span>

12. <span data-ttu-id="ba5cb-132">tooadd hello **svar** åtgärd, Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-132">tooadd hello **Response** action, choose **Add an action**.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. <span data-ttu-id="ba5cb-133">toofilter alla åtgärder toohello som du vill ange hello word **svar** i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-133">toofilter all actions toohello one that you want, enter hello word **response** in hello search box.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. <span data-ttu-id="ba5cb-134">Välj hello **svar** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-134">Select hello **Response** action.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. <span data-ttu-id="ba5cb-135">tooaccess hello MDN från hello utdata från hello **avkoda X12 meddelandet** åtgärd, ange hello svar **BRÖDTEXT** med det här uttrycket:</span><span class="sxs-lookup"><span data-stu-id="ba5cb-135">tooaccess hello MDN from hello output of hello **Decode X12 message** action, set hello response **BODY** field with this expression:</span></span>

    <span data-ttu-id="ba5cb-136">@base64ToString(body('Decode_AS2_message')? ['OutgoingMdn']? ['Content'])</span><span class="sxs-lookup"><span data-stu-id="ba5cb-136">@base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. <span data-ttu-id="ba5cb-137">Spara ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-137">Save your work.</span></span>

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

<span data-ttu-id="ba5cb-138">Du är nu klar att konfigurera logikappen B2B.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-138">You are now done setting up your B2B logic app.</span></span> <span data-ttu-id="ba5cb-139">I ett verkligt program, kan du toostore hello avkodas X12 data i en line-of-business (LOB) app eller data store.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-139">In a real world application, you might want toostore hello decoded X12 data in a line-of-business (LOB) app or data store.</span></span> <span data-ttu-id="ba5cb-140">tooconnect egna LOB-appar och använda dessa API: er i din logikapp kan du lägga till ytterligare åtgärder eller skriva anpassade API: er.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-140">tooconnect your own LOB apps and use these APIs in your logic app, you can add further actions or write custom APIs.</span></span>

## <a name="features-and-use-cases"></a><span data-ttu-id="ba5cb-141">Funktioner och användningsområden</span><span class="sxs-lookup"><span data-stu-id="ba5cb-141">Features and use cases</span></span>

* <span data-ttu-id="ba5cb-142">hello AS2 X12 avkoda och koda åtgärder kan du utbyta data mellan handelspartner med standardprotokollen i logikappar.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-142">hello AS2 and X12 decode and encode actions let you exchange data between trading partners by using industry standard protocols in logic apps.</span></span>
* <span data-ttu-id="ba5cb-143">tooexchange data med handelspartner, du kan använda AS2 och X12 med eller utan varandra.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-143">tooexchange data with trading partners, you can use AS2 and X12 with or without each other.</span></span>
* <span data-ttu-id="ba5cb-144">hello B2B-åtgärder kan du enkelt skapa partners och avtal i kontot för integrering och använda dem i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-144">hello B2B actions help you create partners and agreements easily in your integration account and consume them in a logic app.</span></span>
* <span data-ttu-id="ba5cb-145">När du utökar din logikapp med andra åtgärder kan du skicka och ta emot data mellan andra appar och tjänster som SalesForce.</span><span class="sxs-lookup"><span data-stu-id="ba5cb-145">When you extend your logic app with other actions, you can send and receive data between other apps and services like SalesForce.</span></span>

## <a name="learn-more"></a><span data-ttu-id="ba5cb-146">Läs mer</span><span class="sxs-lookup"><span data-stu-id="ba5cb-146">Learn more</span></span>
[<span data-ttu-id="ba5cb-147">Mer information om hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="ba5cb-147">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md)
