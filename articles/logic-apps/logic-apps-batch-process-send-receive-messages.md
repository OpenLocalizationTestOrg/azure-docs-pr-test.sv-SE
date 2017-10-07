---
title: aaaBatch bearbeta meddelanden som en grupp eller samling - Azure Logic Apps | Microsoft Docs
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
ms.openlocfilehash: 2603db71ee0659d5b6bf5ce3d32f1b0d13c34194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-receive-and-batch-process-messages-in-logic-apps"></a><span data-ttu-id="72e43-104">Skicka, ta emot och batch-bearbeta meddelanden i logikappar</span><span class="sxs-lookup"><span data-stu-id="72e43-104">Send, receive, and batch process messages in logic apps</span></span>

<span data-ttu-id="72e43-105">tooprocess meddelanden tillsammans i grupper som du kan skicka data objekt eller meddelanden, tooa *batch*, och sedan bearbeta de objekt som en batch.</span><span class="sxs-lookup"><span data-stu-id="72e43-105">tooprocess messages together in groups, you can send data items, or messages, tooa *batch*, and then process those items as a batch.</span></span> <span data-ttu-id="72e43-106">Den här metoden är användbar när du vill toomake att dataobjekt grupperas på ett visst sätt och behandlas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="72e43-106">This approach is useful when you want toomake sure data items are grouped in a specific way and are processed together.</span></span> 

<span data-ttu-id="72e43-107">Du kan skapa logikappar som tar emot objekt som en batch med hjälp av hello **Batch** utlösare.</span><span class="sxs-lookup"><span data-stu-id="72e43-107">You can create logic apps that receive items as a batch by using hello **Batch** trigger.</span></span> <span data-ttu-id="72e43-108">Du kan sedan skapa logikappar som skickar objekt tooa batch med hjälp av hello **Batch** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="72e43-108">You can then create logic apps that send items tooa batch by using hello **Batch** action.</span></span>

<span data-ttu-id="72e43-109">Det här avsnittet visar hur du kan skapa en lösning för batching genom att utföra dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="72e43-109">This topic shows how you can build a batching solution by performing these tasks:</span></span> 

* <span data-ttu-id="72e43-110">[Skapa en logikapp som tar emot och samlar in objekt som en batch](#batch-receiver).</span><span class="sxs-lookup"><span data-stu-id="72e43-110">[Create a logic app that receives and collects items as a batch](#batch-receiver).</span></span> <span data-ttu-id="72e43-111">Den här ”batch mottagare” logikapp anger hello batch namn och version kriterier toomeet innan hello mottagaren logikapp släpper och behandlar objekt.</span><span class="sxs-lookup"><span data-stu-id="72e43-111">This "batch receiver" logic app specifies hello batch name and release criteria toomeet before hello receiver logic app releases and processes items.</span></span> 

* <span data-ttu-id="72e43-112">[Skapa en logikapp som skickar objekt tooa batch](#batch-sender).</span><span class="sxs-lookup"><span data-stu-id="72e43-112">[Create a logic app that sends items tooa batch](#batch-sender).</span></span> <span data-ttu-id="72e43-113">Den här ”batch avsändaren” logikapp anger var toosend objekt som måste vara en befintlig logikapp för batch-mottagare.</span><span class="sxs-lookup"><span data-stu-id="72e43-113">This "batch sender" logic app specifies where toosend items, which must be an existing batch receiver logic app.</span></span> <span data-ttu-id="72e43-114">Du kan också ange en unik nyckel som ett kundnummer, för ”partition” eller dela upp hello mål batch i delmängder baserat på nyckeln.</span><span class="sxs-lookup"><span data-stu-id="72e43-114">You can also specify a unique key, like a customer number, too"partition", or divide, hello target batch into subsets based on that key.</span></span> <span data-ttu-id="72e43-115">På så sätt kan alla artiklar med nyckeln samlas in och bearbetas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="72e43-115">That way, all items with that key are collected and processed together.</span></span> 

## <a name="requirements"></a><span data-ttu-id="72e43-116">Krav</span><span class="sxs-lookup"><span data-stu-id="72e43-116">Requirements</span></span>

<span data-ttu-id="72e43-117">toofollow det här exemplet är vad du behöver:</span><span class="sxs-lookup"><span data-stu-id="72e43-117">toofollow this example, you need these items:</span></span>

* <span data-ttu-id="72e43-118">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="72e43-118">An Azure subscription.</span></span> <span data-ttu-id="72e43-119">Om du inte har en prenumeration kan du [börja med ett kostnadsfritt Azure-konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="72e43-119">If you don't have a subscription, you can [start with a free Azure account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="72e43-120">Annars kan du [registrera dig för en prenumeration enligt principen Betala per användning](https://azure.microsoft.com/pricing/purchase-options/).</span><span class="sxs-lookup"><span data-stu-id="72e43-120">Otherwise, you can [sign up for a Pay-As-You-Go subscription](https://azure.microsoft.com/pricing/purchase-options/).</span></span>

* <span data-ttu-id="72e43-121">Grundläggande kunskaper om [hur toocreate logikappar](../logic-apps/logic-apps-create-a-logic-app.md)</span><span class="sxs-lookup"><span data-stu-id="72e43-121">Basic knowledge about [how toocreate logic apps](../logic-apps/logic-apps-create-a-logic-app.md)</span></span> 

* <span data-ttu-id="72e43-122">Ett e-postkonto med alla [e-providern stöds av Azure Logic Apps](../connectors/apis-list.md)</span><span class="sxs-lookup"><span data-stu-id="72e43-122">An email account with any [email provider supported by Azure Logic Apps](../connectors/apis-list.md)</span></span>

<a name="batch-receiver"></a>

## <a name="create-logic-apps-that-receive-messages-as-a-batch"></a><span data-ttu-id="72e43-123">Skapa logikappar som tar emot meddelanden som en batch</span><span class="sxs-lookup"><span data-stu-id="72e43-123">Create logic apps that receive messages as a batch</span></span>

<span data-ttu-id="72e43-124">Innan du kan skicka meddelanden tooa batch, måste du först skapa en logikapp ”batch mottagare” med hello **Batch** utlösare.</span><span class="sxs-lookup"><span data-stu-id="72e43-124">Before you can send messages tooa batch, you must first create a "batch receiver" logic app with hello **Batch** trigger.</span></span> <span data-ttu-id="72e43-125">På så sätt kan du välja den här mottagaren logikappen när du skapar hello avsändaren logikapp.</span><span class="sxs-lookup"><span data-stu-id="72e43-125">That way, you can select this receiver logic app when you create hello sender logic app.</span></span> <span data-ttu-id="72e43-126">För hello mottagare anger du hello batch-namn, version villkor och andra inställningar.</span><span class="sxs-lookup"><span data-stu-id="72e43-126">For hello receiver, you specify hello batch name, release criteria, and other settings.</span></span> 

<span data-ttu-id="72e43-127">Avsändaren logikappar måste veta där toosend objekt när mottagaren logic apps inte behöver tooknow något annat om hello avsändare.</span><span class="sxs-lookup"><span data-stu-id="72e43-127">Sender logic apps need know where toosend items, while receiver logic apps don't need tooknow anything about hello senders.</span></span>

1. <span data-ttu-id="72e43-128">I hello [Azure-portalen](https://portal.azure.com), skapa en logikapp med detta namn: ”BatchReceiver”</span><span class="sxs-lookup"><span data-stu-id="72e43-128">In hello [Azure portal](https://portal.azure.com), create a logic app with this name: "BatchReceiver"</span></span> 

2. <span data-ttu-id="72e43-129">Lägg till hello i Logic Apps Designer **Batch** utlösaren som startar arbetsflödet logik app.</span><span class="sxs-lookup"><span data-stu-id="72e43-129">In Logic Apps Designer, add hello **Batch** trigger, which starts your logic app workflow.</span></span> <span data-ttu-id="72e43-130">Ange ”batch” i sökrutan hello som filter.</span><span class="sxs-lookup"><span data-stu-id="72e43-130">In hello search box, enter "batch" as your filter.</span></span> <span data-ttu-id="72e43-131">Välj den här utlösaren: **Batch – Batch-meddelanden**</span><span class="sxs-lookup"><span data-stu-id="72e43-131">Select this trigger: **Batch – Batch messages**</span></span>

   ![Lägga till Batch-utlösare](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. <span data-ttu-id="72e43-133">Ange ett namn för hello batch och ange villkor för att frisläppa hello batchen, till exempel:</span><span class="sxs-lookup"><span data-stu-id="72e43-133">Provide a name for hello batch, and specify criteria for releasing hello batch, for example:</span></span>

   * <span data-ttu-id="72e43-134">**Batch-namnet**: hello namn som används för tooidentify hello batch, vilket är ”TestBatch” i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="72e43-134">**Batch Name**: hello name used tooidentify hello batch, which is "TestBatch" in this example.</span></span>
   * <span data-ttu-id="72e43-135">**Antalet meddelanden**: hello antalet meddelanden toohold som en batch innan du släpper för bearbetning, vilket är ”5” i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="72e43-135">**Message Count**: hello number of messages toohold as a batch before releasing for processing, which is "5" in this example.</span></span>

   ![Ange information för Batch-utlösare](./media/logic-apps-batch-process-send-receive-messages/receive-batch-trigger-details.png)

4. <span data-ttu-id="72e43-137">Lägg till en annan åtgärd som skickar ett e-postmeddelande när hello batch utlösare utlöses.</span><span class="sxs-lookup"><span data-stu-id="72e43-137">Add another action that sends an email when hello batch trigger fires.</span></span> <span data-ttu-id="72e43-138">Varje gång hello batch har fem artiklar, hello logikapp skickar ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="72e43-138">Each time hello batch has five items, hello logic app sends an email.</span></span>

   1. <span data-ttu-id="72e43-139">Välj under hello batch utlösaren **+ nytt steg** > **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="72e43-139">Under hello batch trigger, choose **+ New Step** > **Add an action**.</span></span>

   2. <span data-ttu-id="72e43-140">Ange ”e” i sökrutan hello som filter.</span><span class="sxs-lookup"><span data-stu-id="72e43-140">In hello search box, enter "email" as your filter.</span></span>
   <span data-ttu-id="72e43-141">Baserat på din e-leverantör, Välj en e-anslutning.</span><span class="sxs-lookup"><span data-stu-id="72e43-141">Based on your email provider, select an email connector.</span></span>
   
      <span data-ttu-id="72e43-142">Om du har ett arbets- eller skolkonto konto kan du till exempel välja hello Office 365 Outlook connector.</span><span class="sxs-lookup"><span data-stu-id="72e43-142">For example, if you have a work or school account, select hello Office 365 Outlook connector.</span></span> 
      <span data-ttu-id="72e43-143">Om du har en Gmail-konto, Välj hello Gmail-anslutning.</span><span class="sxs-lookup"><span data-stu-id="72e43-143">If you have a Gmail account, select hello Gmail connector.</span></span>

   3. <span data-ttu-id="72e43-144">Väljer den här åtgärden för din connector: * *{*e-providern*} – skicka ett e-post **</span><span class="sxs-lookup"><span data-stu-id="72e43-144">Select this action for your connector: **{*email provider*} - Send an email**</span></span>

      <span data-ttu-id="72e43-145">Exempel:</span><span class="sxs-lookup"><span data-stu-id="72e43-145">For example:</span></span>

      ![Välj ”Skicka ett e-postmeddelande” åtgärd för din e-provider](./media/logic-apps-batch-process-send-receive-messages/add-send-email-action.png)

5. <span data-ttu-id="72e43-147">Om du inte tidigare skapade en anslutning för e-post-providern, ange din e-post-autentiseringsuppgifter för autentisering när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="72e43-147">If you didn't previously create a connection for your email provider, provide your email credentials for authentication when prompted.</span></span> <span data-ttu-id="72e43-148">Lär dig mer om [autentisera dina autentiseringsuppgifter för e-](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="72e43-148">Learn more about [authenticating your email credentials](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

6. <span data-ttu-id="72e43-149">Ange hello egenskaper för hello-åtgärd som du just lagt till.</span><span class="sxs-lookup"><span data-stu-id="72e43-149">Set hello properties for hello action you just added.</span></span>

   * <span data-ttu-id="72e43-150">I hello **till** ange hello mottagarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="72e43-150">In hello **To** box, enter hello recipient's email address.</span></span> 
   <span data-ttu-id="72e43-151">I testsyfte kan använda du din egen e-postadress.</span><span class="sxs-lookup"><span data-stu-id="72e43-151">For testing purposes, you can use your own email address.</span></span>

   * <span data-ttu-id="72e43-152">I hello **ämne** rutan när hello **dynamiskt innehåll** lista visas, väljer du hello **partitionsnamnet** fältet.</span><span class="sxs-lookup"><span data-stu-id="72e43-152">In hello **Subject** box, when hello **Dynamic content** list appears, select hello **Partition Name** field.</span></span>

     ![Välj ”partitionsnamnet” hello ”dynamiskt innehåll” listan](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     <span data-ttu-id="72e43-154">Du kan ange en unik partitionsnyckel att dividerar hello mål batch i logiska anger toowhere som du kan skicka meddelanden i ett senare avsnitt.</span><span class="sxs-lookup"><span data-stu-id="72e43-154">In a later section, you can specify a unique partition key that divides hello target batch into logical sets toowhere you can send messages.</span></span> 
     <span data-ttu-id="72e43-155">Varje uppsättning har ett unikt nummer som genereras av hello avsändaren logikapp.</span><span class="sxs-lookup"><span data-stu-id="72e43-155">Each set has a unique number that's generated by hello sender logic app.</span></span> 
     <span data-ttu-id="72e43-156">Den här funktionen kan du använda en enda grupp med flera delar och definiera varje uppsättning med hello-namn som du anger.</span><span class="sxs-lookup"><span data-stu-id="72e43-156">This capability lets you use a single batch with multiple subsets and define each subset with hello name that you provide.</span></span>

   * <span data-ttu-id="72e43-157">I hello **brödtext** rutan när hello **dynamiskt innehåll** lista visas, väljer du hello **meddelande-Id** fältet.</span><span class="sxs-lookup"><span data-stu-id="72e43-157">In hello **Body** box, when hello **Dynamic content** list appears, select hello **Message Id** field.</span></span>

     ![Välj för ”text”, ”meddelande-Id”](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

     <span data-ttu-id="72e43-159">Eftersom hello indata för åtgärden för hello skicka e-post är en matris, hello designer lägger automatiskt till en **för varje** cirkel runt hello **skickar ett e-** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="72e43-159">Because hello input for hello send email action is an array, hello designer automatically adds a **For each** loop around hello **Send an email** action.</span></span> 
     <span data-ttu-id="72e43-160">Denna loop utför hello inre åtgärd på varje objekt i hello batch.</span><span class="sxs-lookup"><span data-stu-id="72e43-160">This loop performs hello inner action on each item in hello batch.</span></span> 
     <span data-ttu-id="72e43-161">Därför med hello batch utlösaren toofive uppsättningsobjekt får du fem e-post varje gång hello utlösare utlöses.</span><span class="sxs-lookup"><span data-stu-id="72e43-161">So, with hello batch trigger set toofive items, you get five emails each time hello trigger fires.</span></span>

7.  <span data-ttu-id="72e43-162">Nu när du har skapat en batch mottagaren logikapp spara din logikapp.</span><span class="sxs-lookup"><span data-stu-id="72e43-162">Now that you created a batch receiver logic app, save your logic app.</span></span>

    ![Spara din logikapp](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

<a name="batch-sender"></a>

## <a name="create-logic-apps-that-send-messages-tooa-batch"></a><span data-ttu-id="72e43-164">Skapa logikappar som skickar meddelanden tooa batch</span><span class="sxs-lookup"><span data-stu-id="72e43-164">Create logic apps that send messages tooa batch</span></span>

<span data-ttu-id="72e43-165">Skapa en eller flera logikappar som skickar objekt toohello batch definieras av hello mottagaren logikapp nu.</span><span class="sxs-lookup"><span data-stu-id="72e43-165">Now create one or more logic apps that send items toohello batch defined by hello receiver logic app.</span></span> <span data-ttu-id="72e43-166">För hello avsändaren anger du hello mottagaren logikapp och batch-namn, meddelandeinnehåll och andra inställningar.</span><span class="sxs-lookup"><span data-stu-id="72e43-166">For hello sender, you specify hello receiver logic app and batch name, message content, and any other settings.</span></span> <span data-ttu-id="72e43-167">Du kan också medföra en unik partition viktiga toodivide hello batch i delmängder toocollect objekt med nyckeln.</span><span class="sxs-lookup"><span data-stu-id="72e43-167">You can optionally provide a unique partition key toodivide hello batch into subsets toocollect items with that key.</span></span>

<span data-ttu-id="72e43-168">Avsändaren logikappar måste veta där toosend objekt när mottagaren logic apps inte behöver tooknow något annat om hello avsändare.</span><span class="sxs-lookup"><span data-stu-id="72e43-168">Sender logic apps need know where toosend items, while receiver logic apps don't need tooknow anything about hello senders.</span></span>

1. <span data-ttu-id="72e43-169">Skapa en annan logikapp med detta namn: ”BatchSender”</span><span class="sxs-lookup"><span data-stu-id="72e43-169">Create another logic app with this name: "BatchSender"</span></span>

   1. <span data-ttu-id="72e43-170">Ange ”återkommande” i sökrutan hello som filter.</span><span class="sxs-lookup"><span data-stu-id="72e43-170">In hello search box, enter "recurrence" as your filter.</span></span> 
   <span data-ttu-id="72e43-171">Välj den här utlösaren: **schema - upprepning**</span><span class="sxs-lookup"><span data-stu-id="72e43-171">Select this trigger: **Schedule - Recurrence**</span></span>

      ![Lägga till hello ”Schemalägg återkommande” utlösare](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-receiver.png)

   2. <span data-ttu-id="72e43-173">Ange hello frekvensen och intervall toorun hello avsändaren logikapp varje minut.</span><span class="sxs-lookup"><span data-stu-id="72e43-173">Set hello frequency and interval toorun hello sender logic app every minute.</span></span>

      ![Ange frekvensen och intervall för upprepning utlösare](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-receiver-details.png)

2. <span data-ttu-id="72e43-175">Lägg till ett nytt steg för att skicka meddelanden tooa batch.</span><span class="sxs-lookup"><span data-stu-id="72e43-175">Add a new step for sending messages tooa batch.</span></span>

   1. <span data-ttu-id="72e43-176">Välj under hello upprepning utlösaren **+ nytt steg** > **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="72e43-176">Under hello recurrence trigger, choose **+ New Step** > **Add an action**.</span></span>

   2. <span data-ttu-id="72e43-177">Ange ”batch” i sökrutan hello som filter.</span><span class="sxs-lookup"><span data-stu-id="72e43-177">In hello search box, enter "batch" as your filter.</span></span> 

   3. <span data-ttu-id="72e43-178">Välj den här åtgärden: **skicka meddelanden toobatch – Välj ett arbetsflöde för Logic Apps med batch-utlösare**</span><span class="sxs-lookup"><span data-stu-id="72e43-178">Select this action: **Send messages toobatch – Choose a Logic Apps workflow with batch trigger**</span></span>

      ![Välj ”Skicka meddelanden toobatch”](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   4. <span data-ttu-id="72e43-180">Nu välja ”BatchReceiver” logikappen som du skapade tidigare, som nu visas som en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="72e43-180">Now select your "BatchReceiver" logic app that you previously created, which now appears as an action.</span></span>

      ![Välj ”batch mottagare” logikapp](./media/logic-apps-batch-process-send-receive-messages/send-batch-select-batch-receiver.png)

      > [!NOTE]
      > <span data-ttu-id="72e43-182">hello listan visas även andra logikappar som har batch-utlösare.</span><span class="sxs-lookup"><span data-stu-id="72e43-182">hello list also shows any other logic apps that have batch triggers.</span></span>

3. <span data-ttu-id="72e43-183">Ange egenskaper för hello batch.</span><span class="sxs-lookup"><span data-stu-id="72e43-183">Set hello batch properties.</span></span>

   * <span data-ttu-id="72e43-184">**Batch-namnet**: hello batch-namn som definieras av hello mottagaren logikapp, som är ”TestBatch” i det här exemplet och verifieras vid körning.</span><span class="sxs-lookup"><span data-stu-id="72e43-184">**Batch Name**: hello batch name defined by hello receiver logic app, which is "TestBatch" in this example and is validated at runtime.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="72e43-185">Kontrollera att du inte ändrar hello batch-namn, vilket måste matcha hello batch-namn som anges av hello mottagaren logikapp.</span><span class="sxs-lookup"><span data-stu-id="72e43-185">Make sure that you don't change hello batch name, which must match hello batch name that's specified by hello receiver logic app.</span></span>
     > <span data-ttu-id="72e43-186">Ändra namnet på hello batch gör hello avsändaren logik app toofail.</span><span class="sxs-lookup"><span data-stu-id="72e43-186">Changing hello batch name causes hello sender logic app toofail.</span></span>

   * <span data-ttu-id="72e43-187">**Meddelande innehåll**: hello meddelandeinnehåll som du vill toosend.</span><span class="sxs-lookup"><span data-stu-id="72e43-187">**Message Content**: hello message content that you want toosend.</span></span> 
   <span data-ttu-id="72e43-188">I det här exemplet lägger du till det här uttrycket att infogningar hello aktuellt datum och tid till innehåll som du skickar toohello batch hello-meddelande:</span><span class="sxs-lookup"><span data-stu-id="72e43-188">For this example, add this expression that inserts hello current date and time into hello message content that you send toohello batch:</span></span>

     1. <span data-ttu-id="72e43-189">När hello **dynamiskt innehåll** lista visas, väljer **uttryck**.</span><span class="sxs-lookup"><span data-stu-id="72e43-189">When hello **Dynamic content** list appears, choose **Expression**.</span></span> 
     2. <span data-ttu-id="72e43-190">Ange hello uttryck **utcnow()**, och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="72e43-190">Enter hello expression **utcnow()**, and choose **OK**.</span></span> 

        ![Välj ”uttryck” i ”meddelande innehåll”.](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details.png)

4. <span data-ttu-id="72e43-193">Nu ställa in en partition för hello batch.</span><span class="sxs-lookup"><span data-stu-id="72e43-193">Now set up a partition for hello batch.</span></span> <span data-ttu-id="72e43-194">Välj i hello ”BatchReceiver”-åtgärd, **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="72e43-194">In hello "BatchReceiver" action, choose **Show advanced options**.</span></span>

   * <span data-ttu-id="72e43-195">**Partitionera namnet**: ett valfritt unikt partition viktiga toouse för att dela hello mål batch.</span><span class="sxs-lookup"><span data-stu-id="72e43-195">**Partition Name**: An optional unique partition key toouse for dividing hello target batch.</span></span> <span data-ttu-id="72e43-196">Lägg till ett uttryck som genererar ett slumptal mellan en och fem i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="72e43-196">For this example, add an expression that generates a random number between one and five.</span></span>
   
     1. <span data-ttu-id="72e43-197">När hello **dynamiskt innehåll** lista visas, väljer **uttryck**.</span><span class="sxs-lookup"><span data-stu-id="72e43-197">When hello **Dynamic content** list appears, choose **Expression**.</span></span>
     2. <span data-ttu-id="72e43-198">Ange det här uttrycket: **rand(1,6)**</span><span class="sxs-lookup"><span data-stu-id="72e43-198">Enter this expression: **rand(1,6)**</span></span>

        ![Skapa en partition för mål-batch](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-partition-advanced-options.png)

        <span data-ttu-id="72e43-200">Detta **SLUMP** funktionen genererar ett tal mellan en och fem.</span><span class="sxs-lookup"><span data-stu-id="72e43-200">This **rand** function generates a number between one and five.</span></span> 
        <span data-ttu-id="72e43-201">Du är så att dela den här batchen i fem numrerade partitioner, som det här uttrycket anger dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="72e43-201">So you are dividing this batch into five numbered partitions, which this expression dynamically sets.</span></span>

   * <span data-ttu-id="72e43-202">**Meddelande-Id**: ett valfritt meddelande-ID och ett GUID som genererats när den är tom.</span><span class="sxs-lookup"><span data-stu-id="72e43-202">**Message Id**: An optional message identifier and is a generated GUID when empty.</span></span> 
   <span data-ttu-id="72e43-203">I det här exemplet lämnar du rutan tom.</span><span class="sxs-lookup"><span data-stu-id="72e43-203">For this example, leave this box blank.</span></span>

5. <span data-ttu-id="72e43-204">Spara din logikapp.</span><span class="sxs-lookup"><span data-stu-id="72e43-204">Save your logic app.</span></span> <span data-ttu-id="72e43-205">Logikappen avsändaren ser nu liknande toothis exempel:</span><span class="sxs-lookup"><span data-stu-id="72e43-205">Your sender logic app now looks similar toothis example:</span></span>

   ![Spara logikappen avsändaren](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details-finished.png)

## <a name="test-your-logic-apps"></a><span data-ttu-id="72e43-207">Testa dina logic apps</span><span class="sxs-lookup"><span data-stu-id="72e43-207">Test your logic apps</span></span>

<span data-ttu-id="72e43-208">tootest din batchbearbetning lösning, lämna dina logic apps kör några minuter.</span><span class="sxs-lookup"><span data-stu-id="72e43-208">tootest your batching solution, leave your logic apps running for a few minutes.</span></span> <span data-ttu-id="72e43-209">Snart du startar hämtning av e-post i grupper med fem, allt med hello partitions samma nyckel.</span><span class="sxs-lookup"><span data-stu-id="72e43-209">Soon, you start getting emails in groups of five, all with hello same partition key.</span></span>

<span data-ttu-id="72e43-210">Logikappen BatchSender körs varje minut, genererar ett slumptal mellan en och fem och använder den här genererat nummer som hello partitionsnyckel för hello mål batch där meddelanden skickas.</span><span class="sxs-lookup"><span data-stu-id="72e43-210">Your BatchSender logic app runs every minute, generates a random number between one and five, and uses this generated number as hello partition key for hello target batch where messages are sent.</span></span> <span data-ttu-id="72e43-211">Varje gång hello batch har fem artiklar med hello samma partitionsnyckel logikappen BatchReceiver utlöses och skickar e-post för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="72e43-211">Each time hello batch has five items with hello same partition key, your BatchReceiver logic app fires and sends mail for each message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="72e43-212">När du är klar testning, se till att du inaktiverar hello BatchSender logik app toostop skickar meddelanden och undvika överbelastning av din inkorg.</span><span class="sxs-lookup"><span data-stu-id="72e43-212">When you're done testing, make sure that you disable hello BatchSender logic app toostop sending messages and avoid overloading your inbox.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72e43-213">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="72e43-213">Next steps</span></span>

* [<span data-ttu-id="72e43-214">Skapa på definitioner för logic appen med hjälp av JSON</span><span class="sxs-lookup"><span data-stu-id="72e43-214">Build on logic app definitions by using JSON</span></span>](../logic-apps/logic-apps-author-definitions.md)
* [<span data-ttu-id="72e43-215">Skapa en serverlösa app i Visual Studio med Azure Logikappar och funktioner</span><span class="sxs-lookup"><span data-stu-id="72e43-215">Build a serverless app in Visual Studio with Azure Logic Apps and Functions</span></span>](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [<span data-ttu-id="72e43-216">Undantagshantering och felloggning för logic apps</span><span class="sxs-lookup"><span data-stu-id="72e43-216">Exception handling and error logging for logic apps</span></span>](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
