---
title: Batch-bearbeta meddelanden som en grupp eller samling - Azure Logic Apps | Microsoft Docs
description: "Skicka och ta emot meddelanden för batchbearbetning i logikappar"
keywords: batch batchprocess
author: jonfancey
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/7/2017
ms.author: LADocs; estfan; jonfan
ms.openlocfilehash: 480ffce5dbe7c25181bb0ba5639de884e98ff4e6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="send-receive-and-batch-process-messages-in-logic-apps"></a><span data-ttu-id="1b5dd-104">Skicka, ta emot och batch-bearbeta meddelanden i logikappar</span><span class="sxs-lookup"><span data-stu-id="1b5dd-104">Send, receive, and batch process messages in logic apps</span></span>

<span data-ttu-id="1b5dd-105">Om du vill bearbeta meddelanden tillsammans i grupper du kan skicka dataobjekt eller meddelanden, till en *batch*, och sedan bearbeta de objekt som en batch.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-105">To process messages together in groups, you can send data items, or messages, to a *batch*, and then process those items as a batch.</span></span> <span data-ttu-id="1b5dd-106">Den här metoden är användbar när du vill kontrollera dataobjekt grupperas på ett visst sätt och behandlas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-106">This approach is useful when you want to make sure data items are grouped in a specific way and are processed together.</span></span> 

<span data-ttu-id="1b5dd-107">Du kan skapa logikappar som tar emot objekt som en batch med hjälp av den **Batch** utlösare.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-107">You can create logic apps that receive items as a batch by using the **Batch** trigger.</span></span> <span data-ttu-id="1b5dd-108">Du kan sedan skapa logikappar som skickar objekt till en batch med hjälp av den **Batch** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-108">You can then create logic apps that send items to a batch by using the **Batch** action.</span></span>

<span data-ttu-id="1b5dd-109">Det här avsnittet visar hur du kan skapa en lösning för batching genom att utföra dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="1b5dd-109">This topic shows how you can build a batching solution by performing these tasks:</span></span> 

* <span data-ttu-id="1b5dd-110">[Skapa en logikapp som tar emot och samlar in objekt som en batch](#batch-receiver).</span><span class="sxs-lookup"><span data-stu-id="1b5dd-110">[Create a logic app that receives and collects items as a batch](#batch-receiver).</span></span> <span data-ttu-id="1b5dd-111">Den här ”batch mottagare” logikapp anger batch namn och version villkor att uppfylla innan logikappen mottagare släpper och behandlar objekt.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-111">This "batch receiver" logic app specifies the batch name and release criteria to meet before the receiver logic app releases and processes items.</span></span> 

* <span data-ttu-id="1b5dd-112">[Skapa en logikapp som skickar objekt till en batch](#batch-sender).</span><span class="sxs-lookup"><span data-stu-id="1b5dd-112">[Create a logic app that sends items to a batch](#batch-sender).</span></span> <span data-ttu-id="1b5dd-113">Den här ”batch avsändaren” logikapp anger var att skicka objekt som måste vara en befintlig logikapp för batch-mottagare.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-113">This "batch sender" logic app specifies where to send items, which must be an existing batch receiver logic app.</span></span> <span data-ttu-id="1b5dd-114">Du kan också ange en unik nyckel som ett kundnummer ”partition” eller dela upp mål batch i delmängder baserat på nyckeln.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-114">You can also specify a unique key, like a customer number, to "partition", or divide, the target batch into subsets based on that key.</span></span> <span data-ttu-id="1b5dd-115">På så sätt kan alla artiklar med nyckeln samlas in och bearbetas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-115">That way, all items with that key are collected and processed together.</span></span> 

## <a name="requirements"></a><span data-ttu-id="1b5dd-116">Krav</span><span class="sxs-lookup"><span data-stu-id="1b5dd-116">Requirements</span></span>

<span data-ttu-id="1b5dd-117">Om du vill följa det här exemplet är vad du behöver:</span><span class="sxs-lookup"><span data-stu-id="1b5dd-117">To follow this example, you need these items:</span></span>

* <span data-ttu-id="1b5dd-118">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-118">An Azure subscription.</span></span> <span data-ttu-id="1b5dd-119">Om du inte har en prenumeration kan du [börja med ett kostnadsfritt Azure-konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="1b5dd-119">If you don't have a subscription, you can [start with a free Azure account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="1b5dd-120">Annars kan du [registrera dig för en prenumeration enligt principen Betala per användning](https://azure.microsoft.com/pricing/purchase-options/).</span><span class="sxs-lookup"><span data-stu-id="1b5dd-120">Otherwise, you can [sign up for a Pay-As-You-Go subscription](https://azure.microsoft.com/pricing/purchase-options/).</span></span>

* <span data-ttu-id="1b5dd-121">Grundläggande kunskaper om [skapa logikappar](../logic-apps/logic-apps-create-a-logic-app.md)</span><span class="sxs-lookup"><span data-stu-id="1b5dd-121">Basic knowledge about [how to create logic apps](../logic-apps/logic-apps-create-a-logic-app.md)</span></span> 

* <span data-ttu-id="1b5dd-122">Ett e-postkonto med alla [e-providern stöds av Azure Logic Apps](../connectors/apis-list.md)</span><span class="sxs-lookup"><span data-stu-id="1b5dd-122">An email account with any [email provider supported by Azure Logic Apps](../connectors/apis-list.md)</span></span>

<a name="batch-receiver"></a>

## <a name="create-logic-apps-that-receive-messages-as-a-batch"></a><span data-ttu-id="1b5dd-123">Skapa logikappar som tar emot meddelanden som en batch</span><span class="sxs-lookup"><span data-stu-id="1b5dd-123">Create logic apps that receive messages as a batch</span></span>

<span data-ttu-id="1b5dd-124">Innan du kan skicka meddelanden till en grupp, måste du först skapa en logikapp för ”batch mottagare” med den **Batch** utlösare.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-124">Before you can send messages to a batch, you must first create a "batch receiver" logic app with the **Batch** trigger.</span></span> <span data-ttu-id="1b5dd-125">På så sätt kan du välja den här mottagaren logikappen när du skapar avsändaren logikappen.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-125">That way, you can select this receiver logic app when you create the sender logic app.</span></span> <span data-ttu-id="1b5dd-126">För mottagaren anger du batch-namn, version villkor och andra inställningar.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-126">For the receiver, you specify the batch name, release criteria, and other settings.</span></span> 

<span data-ttu-id="1b5dd-127">Avsändaren logikappar måste veta var att skicka objekt när mottagaren logic apps inte behöver känna till något om avsändare.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-127">Sender logic apps need know where to send items, while receiver logic apps don't need to know anything about the senders.</span></span>

1. <span data-ttu-id="1b5dd-128">I den [Azure-portalen](https://portal.azure.com), skapa en logikapp med detta namn: ”BatchReceiver”</span><span class="sxs-lookup"><span data-stu-id="1b5dd-128">In the [Azure portal](https://portal.azure.com), create a logic app with this name: "BatchReceiver"</span></span> 

2. <span data-ttu-id="1b5dd-129">Lägg till i Logic Apps Designer i **Batch** utlösaren som startar arbetsflödet logik app.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-129">In Logic Apps Designer, add the **Batch** trigger, which starts your logic app workflow.</span></span> <span data-ttu-id="1b5dd-130">I sökrutan anger du ”batch” som filter.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-130">In the search box, enter "batch" as your filter.</span></span> <span data-ttu-id="1b5dd-131">Välj den här utlösaren: **Batch – Batch-meddelanden**</span><span class="sxs-lookup"><span data-stu-id="1b5dd-131">Select this trigger: **Batch – Batch messages**</span></span>

   ![Lägga till Batch-utlösare](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. <span data-ttu-id="1b5dd-133">Ange ett namn för gruppen och ange villkor för att frisläppa batchen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="1b5dd-133">Provide a name for the batch, and specify criteria for releasing the batch, for example:</span></span>

   * <span data-ttu-id="1b5dd-134">**Batch-namnet**: namnet som används för att identifiera gruppen, som är ”TestBatch” i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-134">**Batch Name**: The name used to identify the batch, which is "TestBatch" in this example.</span></span>
   * <span data-ttu-id="1b5dd-135">**Antalet meddelanden**: antal meddelanden till håller som en batch innan du släpper för bearbetning, vilket är ”5” i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-135">**Message Count**: The number of messages to hold as a batch before releasing for processing, which is "5" in this example.</span></span>

   ![Ange information för Batch-utlösare](./media/logic-apps-batch-process-send-receive-messages/receive-batch-trigger-details.png)

4. <span data-ttu-id="1b5dd-137">Lägg till en annan åtgärd som skickar ett e-postmeddelande när batch-utlösaren utlöses.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-137">Add another action that sends an email when the batch trigger fires.</span></span> <span data-ttu-id="1b5dd-138">Varje gång som gruppen har fem objekt, skickar logikappen ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-138">Each time the batch has five items, the logic app sends an email.</span></span>

   1. <span data-ttu-id="1b5dd-139">Välj under batch-utlösaren **+ nytt steg** > **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-139">Under the batch trigger, choose **+ New Step** > **Add an action**.</span></span>

   2. <span data-ttu-id="1b5dd-140">I sökrutan anger du ”e-post” som filter.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-140">In the search box, enter "email" as your filter.</span></span>
   <span data-ttu-id="1b5dd-141">Baserat på din e-leverantör, Välj en e-anslutning.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-141">Based on your email provider, select an email connector.</span></span>
   
      <span data-ttu-id="1b5dd-142">Om du har ett arbets- eller skolkonto konto väljer du exempelvis Office 365 Outlook connector.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-142">For example, if you have a work or school account, select the Office 365 Outlook connector.</span></span> 
      <span data-ttu-id="1b5dd-143">Om du har en Gmail-konto, Välj den Gmail-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-143">If you have a Gmail account, select the Gmail connector.</span></span>

   3. <span data-ttu-id="1b5dd-144">Väljer den här åtgärden för din connector:  **{*e-providern*} – skicka ett e-post **</span><span class="sxs-lookup"><span data-stu-id="1b5dd-144">Select this action for your connector: **{*email provider*} - Send an email**</span></span>

      <span data-ttu-id="1b5dd-145">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1b5dd-145">For example:</span></span>

      ![Välj ”Skicka ett e-postmeddelande” åtgärd för din e-provider](./media/logic-apps-batch-process-send-receive-messages/add-send-email-action.png)

5. <span data-ttu-id="1b5dd-147">Om du inte tidigare skapade en anslutning för e-post-providern, ange din e-post-autentiseringsuppgifter för autentisering när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-147">If you didn't previously create a connection for your email provider, provide your email credentials for authentication when prompted.</span></span> <span data-ttu-id="1b5dd-148">Lär dig mer om [autentisera dina autentiseringsuppgifter för e-](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1b5dd-148">Learn more about [authenticating your email credentials](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

6. <span data-ttu-id="1b5dd-149">Ange egenskaper för den åtgärd som du just lagt till.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-149">Set the properties for the action you just added.</span></span>

   * <span data-ttu-id="1b5dd-150">I den **till** Ange mottagarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-150">In the **To** box, enter the recipient's email address.</span></span> 
   <span data-ttu-id="1b5dd-151">I testsyfte kan använda du din egen e-postadress.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-151">For testing purposes, you can use your own email address.</span></span>

   * <span data-ttu-id="1b5dd-152">I den **ämne** rutan när den **dynamiskt innehåll** lista visas, väljer du den **partitionsnamnet** fältet.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-152">In the **Subject** box, when the **Dynamic content** list appears, select the **Partition Name** field.</span></span>

     ![Välj i listan ”dynamiskt innehåll” ”partitionsnamnet”](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     <span data-ttu-id="1b5dd-154">Du kan ange en unik partitionsnyckel som dividerar mål batch i logiska grupper där du kan skicka meddelanden i ett senare avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-154">In a later section, you can specify a unique partition key that divides the target batch into logical sets to where you can send messages.</span></span> 
     <span data-ttu-id="1b5dd-155">Varje uppsättning har ett unikt nummer som genereras av avsändaren logikappen.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-155">Each set has a unique number that's generated by the sender logic app.</span></span> 
     <span data-ttu-id="1b5dd-156">Den här funktionen kan du använda en enda grupp med flera delar och definiera varje uppsättning med det namn som du anger.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-156">This capability lets you use a single batch with multiple subsets and define each subset with the name that you provide.</span></span>

   * <span data-ttu-id="1b5dd-157">I den **brödtext** rutan när den **dynamiskt innehåll** lista visas, väljer du den **meddelande-Id** fältet.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-157">In the **Body** box, when the **Dynamic content** list appears, select the **Message Id** field.</span></span>

     ![Välj för ”text”, ”meddelande-Id”](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

     <span data-ttu-id="1b5dd-159">Eftersom indata för åtgärden Skicka e-post är en matris med designer lägger automatiskt till en **för varje** Slinga runt den **skickar ett e-** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-159">Because the input for the send email action is an array, the designer automatically adds a **For each** loop around the **Send an email** action.</span></span> 
     <span data-ttu-id="1b5dd-160">Denna loop utför åtgärden inre på varje objekt i gruppen.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-160">This loop performs the inner action on each item in the batch.</span></span> 
     <span data-ttu-id="1b5dd-161">Så med batch-utlösare är inställd på fem artiklar får du gång fem e-post varje utlösare utlöses.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-161">So, with the batch trigger set to five items, you get five emails each time the trigger fires.</span></span>

7.  <span data-ttu-id="1b5dd-162">Nu när du har skapat en batch mottagaren logikapp spara din logikapp.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-162">Now that you created a batch receiver logic app, save your logic app.</span></span>

    ![Spara din logikapp](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

<a name="batch-sender"></a>

## <a name="create-logic-apps-that-send-messages-to-a-batch"></a><span data-ttu-id="1b5dd-164">Skapa logikappar som skickar meddelanden till en grupp</span><span class="sxs-lookup"><span data-stu-id="1b5dd-164">Create logic apps that send messages to a batch</span></span>

<span data-ttu-id="1b5dd-165">Nu ska du skapa en eller flera logikappar som skickar objekt i gruppen som definieras av logikappen mottagare.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-165">Now create one or more logic apps that send items to the batch defined by the receiver logic app.</span></span> <span data-ttu-id="1b5dd-166">För avsändaren anger du logikappen mottagare och batch-namn, meddelandeinnehåll och andra inställningar.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-166">For the sender, you specify the receiver logic app and batch name, message content, and any other settings.</span></span> <span data-ttu-id="1b5dd-167">Alternativt kan du ange en unik partitionsnyckel om du vill dela upp gruppen i delmängder att samla in objekt med nyckeln.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-167">You can optionally provide a unique partition key to divide the batch into subsets to collect items with that key.</span></span>

<span data-ttu-id="1b5dd-168">Avsändaren logikappar måste veta var att skicka objekt när mottagaren logic apps inte behöver känna till något om avsändare.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-168">Sender logic apps need know where to send items, while receiver logic apps don't need to know anything about the senders.</span></span>

1. <span data-ttu-id="1b5dd-169">Skapa en annan logikapp med detta namn: ”BatchSender”</span><span class="sxs-lookup"><span data-stu-id="1b5dd-169">Create another logic app with this name: "BatchSender"</span></span>

   1. <span data-ttu-id="1b5dd-170">I sökrutan anger du ”återkommande” som filter.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-170">In the search box, enter "recurrence" as your filter.</span></span> 
   <span data-ttu-id="1b5dd-171">Välj den här utlösaren: **schema - upprepning**</span><span class="sxs-lookup"><span data-stu-id="1b5dd-171">Select this trigger: **Schedule - Recurrence**</span></span>

      ![Lägga till ”Schemalägg återkommande”-utlösare](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-receiver.png)

   2. <span data-ttu-id="1b5dd-173">Ange frekvensen och intervall för att köra avsändaren logikapp varje minut.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-173">Set the frequency and interval to run the sender logic app every minute.</span></span>

      ![Ange frekvensen och intervall för upprepning utlösare](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-receiver-details.png)

2. <span data-ttu-id="1b5dd-175">Lägg till ett nytt steg för att skicka meddelanden till en batch.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-175">Add a new step for sending messages to a batch.</span></span>

   1. <span data-ttu-id="1b5dd-176">Välj under utlösaren återkommande **+ nytt steg** > **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-176">Under the recurrence trigger, choose **+ New Step** > **Add an action**.</span></span>

   2. <span data-ttu-id="1b5dd-177">I sökrutan anger du ”batch” som filter.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-177">In the search box, enter "batch" as your filter.</span></span> 

   3. <span data-ttu-id="1b5dd-178">Välj den här åtgärden: **skicka meddelanden till batch – Välj ett arbetsflöde för Logic Apps med batch-utlösare**</span><span class="sxs-lookup"><span data-stu-id="1b5dd-178">Select this action: **Send messages to batch – Choose a Logic Apps workflow with batch trigger**</span></span>

      ![Välj ”Skicka meddelanden till batch”](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   4. <span data-ttu-id="1b5dd-180">Nu välja ”BatchReceiver” logikappen som du skapade tidigare, som nu visas som en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-180">Now select your "BatchReceiver" logic app that you previously created, which now appears as an action.</span></span>

      ![Välj ”batch mottagare” logikapp](./media/logic-apps-batch-process-send-receive-messages/send-batch-select-batch-receiver.png)

      > [!NOTE]
      > <span data-ttu-id="1b5dd-182">I listan visas också andra logikappar som har batch-utlösare.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-182">The list also shows any other logic apps that have batch triggers.</span></span>

3. <span data-ttu-id="1b5dd-183">Ange Batchegenskaper.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-183">Set the batch properties.</span></span>

   * <span data-ttu-id="1b5dd-184">**Batch-namnet**: batch-namn som definierats av logikappen mottagare som är ”TestBatch” i det här exemplet och verifieras vid körning.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-184">**Batch Name**: The batch name defined by the receiver logic app, which is "TestBatch" in this example and is validated at runtime.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="1b5dd-185">Kontrollera att du inte ändra namnet, vilket måste matcha namnet som anges av logikappen mottagare.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-185">Make sure that you don't change the batch name, which must match the batch name that's specified by the receiver logic app.</span></span>
     > <span data-ttu-id="1b5dd-186">Ändra namnet på batch gör att avsändaren logikapp misslyckas.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-186">Changing the batch name causes the sender logic app to fail.</span></span>

   * <span data-ttu-id="1b5dd-187">**Meddelande innehåll**: meddelandeinnehåll som du vill skicka.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-187">**Message Content**: The message content that you want to send.</span></span> 
   <span data-ttu-id="1b5dd-188">I det här exemplet lägger du till det här uttrycket som infogar aktuellt datum och tid i meddelandeinnehåll som du skickar till gruppen:</span><span class="sxs-lookup"><span data-stu-id="1b5dd-188">For this example, add this expression that inserts the current date and time into the message content that you send to the batch:</span></span>

     1. <span data-ttu-id="1b5dd-189">När den **dynamiskt innehåll** lista visas, väljer **uttryck**.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-189">When the **Dynamic content** list appears, choose **Expression**.</span></span> 
     2. <span data-ttu-id="1b5dd-190">Anger uttrycket **utcnow()**, och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-190">Enter the expression **utcnow()**, and choose **OK**.</span></span> 

        ![Välj ”uttryck” i ”meddelande innehåll”.](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details.png)

4. <span data-ttu-id="1b5dd-193">Nu ställa in en partition för gruppen.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-193">Now set up a partition for the batch.</span></span> <span data-ttu-id="1b5dd-194">I ”BatchReceiver”-åtgärd väljer **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-194">In the "BatchReceiver" action, choose **Show advanced options**.</span></span>

   * <span data-ttu-id="1b5dd-195">**Partitionera namnet**: ett valfritt unikt partitionsnyckel ska användas för att dela mål batch.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-195">**Partition Name**: An optional unique partition key to use for dividing the target batch.</span></span> <span data-ttu-id="1b5dd-196">Lägg till ett uttryck som genererar ett slumptal mellan en och fem i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-196">For this example, add an expression that generates a random number between one and five.</span></span>
   
     1. <span data-ttu-id="1b5dd-197">När den **dynamiskt innehåll** lista visas, väljer **uttryck**.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-197">When the **Dynamic content** list appears, choose **Expression**.</span></span>
     2. <span data-ttu-id="1b5dd-198">Ange det här uttrycket: **rand(1,6)**</span><span class="sxs-lookup"><span data-stu-id="1b5dd-198">Enter this expression: **rand(1,6)**</span></span>

        ![Skapa en partition för mål-batch](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-partition-advanced-options.png)

        <span data-ttu-id="1b5dd-200">Detta **SLUMP** funktionen genererar ett tal mellan en och fem.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-200">This **rand** function generates a number between one and five.</span></span> 
        <span data-ttu-id="1b5dd-201">Du är så att dela den här batchen i fem numrerade partitioner, som det här uttrycket anger dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-201">So you are dividing this batch into five numbered partitions, which this expression dynamically sets.</span></span>

   * <span data-ttu-id="1b5dd-202">**Meddelande-Id**: ett valfritt meddelande-ID och ett GUID som genererats när den är tom.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-202">**Message Id**: An optional message identifier and is a generated GUID when empty.</span></span> 
   <span data-ttu-id="1b5dd-203">I det här exemplet lämnar du rutan tom.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-203">For this example, leave this box blank.</span></span>

5. <span data-ttu-id="1b5dd-204">Spara din logikapp.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-204">Save your logic app.</span></span> <span data-ttu-id="1b5dd-205">Logikappen avsändaren nu ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="1b5dd-205">Your sender logic app now looks similar to this example:</span></span>

   ![Spara logikappen avsändaren](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details-finished.png)

## <a name="test-your-logic-apps"></a><span data-ttu-id="1b5dd-207">Testa dina logic apps</span><span class="sxs-lookup"><span data-stu-id="1b5dd-207">Test your logic apps</span></span>

<span data-ttu-id="1b5dd-208">Lämna dina logic apps kör några minuter för att testa din batching lösning.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-208">To test your batching solution, leave your logic apps running for a few minutes.</span></span> <span data-ttu-id="1b5dd-209">Du startar snart få e-post i grupper med fem, alla med samma partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-209">Soon, you start getting emails in groups of five, all with the same partition key.</span></span>

<span data-ttu-id="1b5dd-210">Logikappen BatchSender körs varje minut, genererar ett slumptal mellan en och fem och använder den här genererat nummer som partitionsnyckel för mål-batch där meddelanden skickas.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-210">Your BatchSender logic app runs every minute, generates a random number between one and five, and uses this generated number as the partition key for the target batch where messages are sent.</span></span> <span data-ttu-id="1b5dd-211">Varje gång som gruppen har fem objekt med samma partitionsnyckel logikappen BatchReceiver utlöses och skickar e-post för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-211">Each time the batch has five items with the same partition key, your BatchReceiver logic app fires and sends mail for each message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1b5dd-212">När du är klar testning, se till att du inaktiverar BatchSender logikappen så att stoppa meddelanden och undvika överbelastning av din inkorg.</span><span class="sxs-lookup"><span data-stu-id="1b5dd-212">When you're done testing, make sure that you disable the BatchSender logic app to stop sending messages and avoid overloading your inbox.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b5dd-213">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1b5dd-213">Next steps</span></span>

* [<span data-ttu-id="1b5dd-214">Skapa på definitioner för logic appen med hjälp av JSON</span><span class="sxs-lookup"><span data-stu-id="1b5dd-214">Build on logic app definitions by using JSON</span></span>](../logic-apps/logic-apps-author-definitions.md)
* [<span data-ttu-id="1b5dd-215">Skapa en serverlösa app i Visual Studio med Azure Logikappar och funktioner</span><span class="sxs-lookup"><span data-stu-id="1b5dd-215">Build a serverless app in Visual Studio with Azure Logic Apps and Functions</span></span>](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [<span data-ttu-id="1b5dd-216">Undantagshantering och felloggning för logic apps</span><span class="sxs-lookup"><span data-stu-id="1b5dd-216">Exception handling and error logging for logic apps</span></span>](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
