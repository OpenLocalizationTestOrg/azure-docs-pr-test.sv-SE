---
title: aaaLearn hur toouse hello MQ-anslutningen i Azure Logic Apps | Microsoft Docs
description: "Ansluta tooan lokal eller Azure MQ-server från din logik app arbetsflöde toobrowse, ta emot och skicka meddelanden tooWebSphere MQ"
services: logic-apps
author: valthom
manager: anneta
documentationcenter: 
editor: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 06/01/2017
ms.author: valthom; ladocs
ms.openlocfilehash: 8b36d53b457ced1a7461c229aecfcf8e4ae668a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-ibm-mq-server-from-logic-apps-using-hello-mq-connector"></a><span data-ttu-id="cf59e-103">Ansluta tooan IBM MQ-server från logikappar med hjälp av hello MQ connector</span><span class="sxs-lookup"><span data-stu-id="cf59e-103">Connect tooan IBM MQ server from logic apps using hello MQ connector</span></span> 

<span data-ttu-id="cf59e-104">Microsoft Connector för MQ skickar och tar emot meddelanden som lagras i ett MQ-Server lokalt eller i Azure.</span><span class="sxs-lookup"><span data-stu-id="cf59e-104">Microsoft Connector for MQ sends and retrieves messages stored in an MQ Server on-premises, or in Azure.</span></span> <span data-ttu-id="cf59e-105">Denna koppling inkluderar en Microsoft MQ-klient som kommunicerar med en fjärransluten IBM MQ-server i ett TCP/IP-nätverk.</span><span class="sxs-lookup"><span data-stu-id="cf59e-105">This connector includes a Microsoft MQ client that communicates with a remote IBM MQ server across a TCP/IP network.</span></span> <span data-ttu-id="cf59e-106">Det här dokumentet är en starter guiden toouse hello MQ-koppling.</span><span class="sxs-lookup"><span data-stu-id="cf59e-106">This document is a starter guide toouse hello MQ connector.</span></span> <span data-ttu-id="cf59e-107">Vi rekommenderar att du börjar genom att bläddra i ett enda meddelande i en kö och försök sedan hello andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="cf59e-107">We recommended you begin by browsing a single message on a queue, and then trying hello other actions.</span></span>    

<span data-ttu-id="cf59e-108">hello MQ koppling inkluderar hello följande åtgärder.</span><span class="sxs-lookup"><span data-stu-id="cf59e-108">hello MQ connector includes hello following actions.</span></span> <span data-ttu-id="cf59e-109">Det finns inga utlösare.</span><span class="sxs-lookup"><span data-stu-id="cf59e-109">There are no triggers.</span></span>

-   <span data-ttu-id="cf59e-110">Bläddra i ett enda meddelande utan att ta bort hello-meddelande från hello IBM MQ-Server</span><span class="sxs-lookup"><span data-stu-id="cf59e-110">Browse a single message without deleting hello message from hello IBM MQ Server</span></span>
-   <span data-ttu-id="cf59e-111">Bläddra i en grupp med meddelanden utan att ta bort hälsningsmeddelande från hello IBM MQ-Server</span><span class="sxs-lookup"><span data-stu-id="cf59e-111">Browse a batch of messages without deleting hello messages from hello IBM MQ Server</span></span>
-   <span data-ttu-id="cf59e-112">Ett enda meddelande och ta bort hello-meddelande från hello IBM MQ-Server</span><span class="sxs-lookup"><span data-stu-id="cf59e-112">Receive a single message and delete hello message from hello IBM MQ Server</span></span>
-   <span data-ttu-id="cf59e-113">Ta emot en grupp med meddelanden och ta bort hälsningsmeddelande från hello IBM MQ-Server</span><span class="sxs-lookup"><span data-stu-id="cf59e-113">Receive a batch of messages and delete hello messages from hello IBM MQ Server</span></span>
-   <span data-ttu-id="cf59e-114">Skicka ett enda meddelande toohello IBM MQ-Server</span><span class="sxs-lookup"><span data-stu-id="cf59e-114">Send a single message toohello IBM MQ Server</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="cf59e-115">Krav</span><span class="sxs-lookup"><span data-stu-id="cf59e-115">Prerequisites</span></span>

* <span data-ttu-id="cf59e-116">Om du använder en lokal MQ-server [installera hello lokala datagateway](../logic-apps/logic-apps-gateway-install.md) på en server i nätverket.</span><span class="sxs-lookup"><span data-stu-id="cf59e-116">If using an on-premises MQ server, [install hello on-premises data gateway](../logic-apps/logic-apps-gateway-install.md) on a server within your network.</span></span> <span data-ttu-id="cf59e-117">Om hello MQ-Server är offentligt tillgänglig, eller tillgängliga i Azure, sedan hello datagateway inte används eller krävs.</span><span class="sxs-lookup"><span data-stu-id="cf59e-117">If hello MQ Server is publicly available, or available within Azure, then hello data gateway is not used or required.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cf59e-118">hello-server där hello lokala Data Gateway har installerats måste ha .net Framework 4.6 installerats för hello MQ Connector toofunction.</span><span class="sxs-lookup"><span data-stu-id="cf59e-118">hello server where hello On-Premises Data Gateway is installed must also have .Net Framework 4.6 installed for hello MQ Connector toofunction.</span></span>

* <span data-ttu-id="cf59e-119">Skapa hello Azure-resurs för hello lokala datagateway - [konfigurera hello data gateway-anslutningen](../logic-apps/logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="cf59e-119">Create hello Azure resource for hello on-premises data gateway - [Set up hello data gateway connection](../logic-apps/logic-apps-gateway-connection.md).</span></span>

* <span data-ttu-id="cf59e-120">Officiellt IBM WebSphere MQ-versioner som stöds:</span><span class="sxs-lookup"><span data-stu-id="cf59e-120">Officially supported IBM WebSphere MQ versions:</span></span>
   * <span data-ttu-id="cf59e-121">MQ 7.5</span><span class="sxs-lookup"><span data-stu-id="cf59e-121">MQ 7.5</span></span>
   * <span data-ttu-id="cf59e-122">MQ 8.0</span><span class="sxs-lookup"><span data-stu-id="cf59e-122">MQ 8.0</span></span>

## <a name="create-a-logic-app"></a><span data-ttu-id="cf59e-123">Skapa en logikapp</span><span class="sxs-lookup"><span data-stu-id="cf59e-123">Create a logic app</span></span>

1. <span data-ttu-id="cf59e-124">I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="cf59e-124">In hello **Azure start board**, select **+** (plus sign), **Web + Mobile**, and then **Logic App**.</span></span> 
2. <span data-ttu-id="cf59e-125">Ange hello **namn**, till exempel MQTestApp, **prenumeration**, **resursgruppen**, och **plats** (använda hello plats där hello lokala Data Gateway-anslutningen har konfigurerats).</span><span class="sxs-lookup"><span data-stu-id="cf59e-125">Enter hello **Name**, such as MQTestApp, **Subscription**, **Resource group**, and **Location** (use hello location where hello on-premises Data Gateway connection is configured).</span></span> <span data-ttu-id="cf59e-126">Välj **PIN-kod toodashboard**, och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="cf59e-126">Select **Pin toodashboard**, and select **Create**.</span></span>  
<span data-ttu-id="cf59e-127">![Skapa Logikapp](media/connectors-create-api-mq/Create_Logic_App.png)</span><span class="sxs-lookup"><span data-stu-id="cf59e-127">![Create Logic App](media/connectors-create-api-mq/Create_Logic_App.png)</span></span>

## <a name="add-a-trigger"></a><span data-ttu-id="cf59e-128">Lägg till en utlösare</span><span class="sxs-lookup"><span data-stu-id="cf59e-128">Add a trigger</span></span>

> [!NOTE]
> <span data-ttu-id="cf59e-129">hello MQ Connector har inte utlösare.</span><span class="sxs-lookup"><span data-stu-id="cf59e-129">hello MQ Connector does not have any triggers.</span></span> <span data-ttu-id="cf59e-130">Så, Använd en annan utlösare toostart din logikapp, till exempel hello **återkommande** utlösare.</span><span class="sxs-lookup"><span data-stu-id="cf59e-130">So, use another trigger toostart your logic app, such as hello **Recurrence** trigger.</span></span> 

1. <span data-ttu-id="cf59e-131">Hej **Logic Apps Designer** öppnas väljer **återkommande** i hello lista över vanliga utlösare.</span><span class="sxs-lookup"><span data-stu-id="cf59e-131">hello **Logic Apps Designer** opens, select **Recurrence** in hello list of common triggers.</span></span>
2. <span data-ttu-id="cf59e-132">Välj **redigera** inom hello upprepning utlösare.</span><span class="sxs-lookup"><span data-stu-id="cf59e-132">Select **Edit** within hello Recurrence Trigger.</span></span> 
3. <span data-ttu-id="cf59e-133">Ange hello **frekvens** för**dag**, och ange hello **intervall** för**7**.</span><span class="sxs-lookup"><span data-stu-id="cf59e-133">Set hello **Frequency** too**Day**, and set hello **Interval** too**7**.</span></span> 

## <a name="browse-a-single-message"></a><span data-ttu-id="cf59e-134">Bläddra i ett enda meddelande</span><span class="sxs-lookup"><span data-stu-id="cf59e-134">Browse a single message</span></span>
1. <span data-ttu-id="cf59e-135">Välj **+ nytt steg**, och välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="cf59e-135">Select **+ New step**, and select **Add an action**.</span></span>
2. <span data-ttu-id="cf59e-136">Skriv i sökrutan hello `mq`, och välj sedan **MQ - Bläddra meddelandet**.</span><span class="sxs-lookup"><span data-stu-id="cf59e-136">In hello search box, type `mq`, and then select **MQ - Browse message**.</span></span>  
<span data-ttu-id="cf59e-137">![Bläddra meddelande](media/connectors-create-api-mq/Browse_message.png)</span><span class="sxs-lookup"><span data-stu-id="cf59e-137">![Browse message](media/connectors-create-api-mq/Browse_message.png)</span></span>

3. <span data-ttu-id="cf59e-138">Om det inte finns en befintlig MQ-anslutning, skapar du hello-anslutningen:</span><span class="sxs-lookup"><span data-stu-id="cf59e-138">If there isn't an existing MQ connection, then create hello connection:</span></span>  

    1. <span data-ttu-id="cf59e-139">Välj **Anslut via lokala datagateway**, och ange egenskaper för hello MQ-Server.</span><span class="sxs-lookup"><span data-stu-id="cf59e-139">Select **Connect via on-premises data gateway**, and enter hello properties of your MQ server.</span></span>  
    <span data-ttu-id="cf59e-140">För **Server**, du kan ange hello MQ-servernamn eller ange hello IP-adressen följt av ett kolon och hello portnummer.</span><span class="sxs-lookup"><span data-stu-id="cf59e-140">For **Server**, you can enter hello MQ server name, or enter hello IP address followed by a colon and hello port number.</span></span> 
    2. <span data-ttu-id="cf59e-141">Hej **gateway** listan visar alla befintliga gateway-anslutningar som har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="cf59e-141">hello **gateway** dropdown lists any existing gateway connections that have been configured.</span></span> <span data-ttu-id="cf59e-142">Välj din gateway.</span><span class="sxs-lookup"><span data-stu-id="cf59e-142">Select your gateway.</span></span>
    3. <span data-ttu-id="cf59e-143">Välj **skapa** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="cf59e-143">Select **Create** when finished.</span></span> <span data-ttu-id="cf59e-144">Anslutningen ser ut ungefär toohello följande:</span><span class="sxs-lookup"><span data-stu-id="cf59e-144">Your connection looks similar toohello following:</span></span>   
    <span data-ttu-id="cf59e-145">![Anslutningsegenskaper](media/connectors-create-api-mq/Connection_Properties.png)</span><span class="sxs-lookup"><span data-stu-id="cf59e-145">![Connection Properties](media/connectors-create-api-mq/Connection_Properties.png)</span></span>

4. <span data-ttu-id="cf59e-146">I egenskaperna för hello-åtgärd kan du:</span><span class="sxs-lookup"><span data-stu-id="cf59e-146">In hello action properties, you can:</span></span>  

    * <span data-ttu-id="cf59e-147">Använd hello **kön** egenskapen tooaccess en annan kö än vad som har definierats i hello-anslutning</span><span class="sxs-lookup"><span data-stu-id="cf59e-147">Use hello **Queue** property tooaccess a different queue name than what is defined in hello connection</span></span>
    * <span data-ttu-id="cf59e-148">Använd hello **MessageId**, **CorrelationId**, **GroupId**, och andra egenskaper toobrowse för ett meddelande utifrån hello olika MQ meddelandeegenskaper</span><span class="sxs-lookup"><span data-stu-id="cf59e-148">Use hello **MessageId**, **CorrelationId**, **GroupId**, and other properties toobrowse for a message based on hello different MQ message properties</span></span>
    * <span data-ttu-id="cf59e-149">Ange **IncludeInfo** för**SANT** tooinclude ytterligare meddelandeinformation i hello utdata.</span><span class="sxs-lookup"><span data-stu-id="cf59e-149">Set **IncludeInfo** too**True** tooinclude additional message information in hello output.</span></span> <span data-ttu-id="cf59e-150">Eller ändra värdet för**FALSKT** toonot inkludera information om ytterligare meddelande i hello utdata.</span><span class="sxs-lookup"><span data-stu-id="cf59e-150">Or, set it too**False** toonot include additional message information in hello output.</span></span>
    * <span data-ttu-id="cf59e-151">Ange en **Timeout** värdet toodetermine hur länge toowait för ett meddelande tooarrive i en tom kö.</span><span class="sxs-lookup"><span data-stu-id="cf59e-151">Enter a **Timeout** value toodetermine how long toowait for a message tooarrive in an empty queue.</span></span> <span data-ttu-id="cf59e-152">Om inget anges hello första meddelandet i kön hello hämtas och det finns ingen tid väntar på ett meddelande tooappear.</span><span class="sxs-lookup"><span data-stu-id="cf59e-152">If nothing is entered, hello first message in hello queue is retrieved, and there is no time spent waiting for a message tooappear.</span></span>  
    <span data-ttu-id="cf59e-153">![Bläddra meddelandeegenskaper](media/connectors-create-api-mq/Browse_message_Props.png)</span><span class="sxs-lookup"><span data-stu-id="cf59e-153">![Browse Message Properties](media/connectors-create-api-mq/Browse_message_Props.png)</span></span>

5. <span data-ttu-id="cf59e-154">**Spara** dina ändringar och sedan **kör** logikappen:</span><span class="sxs-lookup"><span data-stu-id="cf59e-154">**Save** your changes, and then **Run** your logic app:</span></span>  
<span data-ttu-id="cf59e-155">![Spara och köra](media/connectors-create-api-mq/Save_Run.png)</span><span class="sxs-lookup"><span data-stu-id="cf59e-155">![Save and run](media/connectors-create-api-mq/Save_Run.png)</span></span>

6. <span data-ttu-id="cf59e-156">Hello stegen för att köra hello visas efter några sekunder och titta på hello utdata.</span><span class="sxs-lookup"><span data-stu-id="cf59e-156">After a few seconds, hello steps of hello run are shown, and you can look at hello output.</span></span> <span data-ttu-id="cf59e-157">Välj hello grön bockmarkering toosee information för varje steg.</span><span class="sxs-lookup"><span data-stu-id="cf59e-157">Select hello green checkmark toosee details of each step.</span></span> <span data-ttu-id="cf59e-158">Välj **finns rådata utdata** toosee ytterligare information om hello utdata.</span><span class="sxs-lookup"><span data-stu-id="cf59e-158">Select **See raw outputs** toosee additional details on hello output data.</span></span>  
<span data-ttu-id="cf59e-159">![Bläddra meddelandet utdata](media/connectors-create-api-mq/Browse_message_output.png)</span><span class="sxs-lookup"><span data-stu-id="cf59e-159">![Browse message output](media/connectors-create-api-mq/Browse_message_output.png)</span></span>  

    <span data-ttu-id="cf59e-160">Rådata utdata:</span><span class="sxs-lookup"><span data-stu-id="cf59e-160">Raw output:</span></span>  
    ![Bläddra meddelandet raw-utdata](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. <span data-ttu-id="cf59e-162">När hello **IncludeInfo** alternativuppsättning tootrue, hello följande utdata visas:</span><span class="sxs-lookup"><span data-stu-id="cf59e-162">When hello **IncludeInfo** option is set tootrue, hello following output is displayed:</span></span>  
<span data-ttu-id="cf59e-163">![Bläddra meddelandet innehålla info](media/connectors-create-api-mq/Browse_message_Include_Info.png)</span><span class="sxs-lookup"><span data-stu-id="cf59e-163">![Browse message include info](media/connectors-create-api-mq/Browse_message_Include_Info.png)</span></span>

## <a name="browse-multiple-messages"></a><span data-ttu-id="cf59e-164">Bläddra flera meddelanden</span><span class="sxs-lookup"><span data-stu-id="cf59e-164">Browse multiple messages</span></span>
<span data-ttu-id="cf59e-165">Hej **Bläddra meddelanden** åtgärden inkluderar en **BatchSize** alternativet tooindicate hur många meddelanden ska returneras från hello kö.</span><span class="sxs-lookup"><span data-stu-id="cf59e-165">hello **Browse messages** action includes a **BatchSize** option tooindicate how many messages should be returned from hello queue.</span></span>  <span data-ttu-id="cf59e-166">Om **BatchSize** har ingen post returneras alla meddelanden.</span><span class="sxs-lookup"><span data-stu-id="cf59e-166">If **BatchSize** has no entry, all messages are returned.</span></span> <span data-ttu-id="cf59e-167">hello returnerade utdata är en matris av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="cf59e-167">hello returned output is an array of messages.</span></span>

1. <span data-ttu-id="cf59e-168">När du lägger till hello **Bläddra meddelanden** åtgärd hello första anslutningen har konfigurerats är markerad som standard.</span><span class="sxs-lookup"><span data-stu-id="cf59e-168">When adding hello **Browse messages** action, hello first connection that is configured is selected by default.</span></span> <span data-ttu-id="cf59e-169">Välj **ändra anslutningen** toocreate en ny anslutning eller välj en annan anslutning.</span><span class="sxs-lookup"><span data-stu-id="cf59e-169">Select **Change connection** toocreate a new connection, or select a different connection.</span></span>

2. <span data-ttu-id="cf59e-170">hello utdata från Bläddra meddelanden visas:</span><span class="sxs-lookup"><span data-stu-id="cf59e-170">hello output of Browse messages shows:</span></span>  
![Bläddra meddelanden utdata](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a><span data-ttu-id="cf59e-172">Ett enda meddelande</span><span class="sxs-lookup"><span data-stu-id="cf59e-172">Receive a single message</span></span>
<span data-ttu-id="cf59e-173">Hej **ta emot meddelandet** åtgärd har hello samma in- och utgångar som hello **Bläddra meddelandet** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="cf59e-173">hello **Receive message** action has hello same inputs and outputs as hello **Browse message** action.</span></span> <span data-ttu-id="cf59e-174">När du använder **ta emot meddelandet**, hello-meddelande tas bort från kön hello.</span><span class="sxs-lookup"><span data-stu-id="cf59e-174">When using **Receive message**, hello message is deleted from hello queue.</span></span>

## <a name="receive-multiple-messages"></a><span data-ttu-id="cf59e-175">Ta emot flera meddelanden</span><span class="sxs-lookup"><span data-stu-id="cf59e-175">Receive multiple messages</span></span>
<span data-ttu-id="cf59e-176">Hej **ta emot meddelanden** åtgärd har hello samma in- och utgångar som hello **Bläddra meddelanden** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="cf59e-176">hello **Receive messages** action has hello same inputs and outputs as hello **Browse messages** action.</span></span> <span data-ttu-id="cf59e-177">När du använder **ta emot meddelanden**, hälsningsmeddelande tas bort från kön hello.</span><span class="sxs-lookup"><span data-stu-id="cf59e-177">When using **Receive messages**, hello messages are deleted from hello queue.</span></span>

<span data-ttu-id="cf59e-178">Om det finns inga meddelanden i kö för hello när du gör en Bläddra eller en receive, misslyckas hello steg med hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="cf59e-178">If there are no messages in hello queue when doing a browse or a receive, hello step fails with hello following output:</span></span>  
![MQ Nej visas fel](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a><span data-ttu-id="cf59e-180">Skicka ett meddelande</span><span class="sxs-lookup"><span data-stu-id="cf59e-180">Send a message</span></span>
1. <span data-ttu-id="cf59e-181">När du lägger till hello **skickar ett meddelande** åtgärd hello första anslutningen har konfigurerats är markerad som standard.</span><span class="sxs-lookup"><span data-stu-id="cf59e-181">When adding hello **Send message** action, hello first connection that is configured is selected by default.</span></span> <span data-ttu-id="cf59e-182">Välj **ändra anslutningen** toocreate en ny anslutning eller välj en annan anslutning.</span><span class="sxs-lookup"><span data-stu-id="cf59e-182">Select **Change connection** toocreate a new connection, or select a different connection.</span></span> <span data-ttu-id="cf59e-183">hello giltig **meddelandetyper** är **Datagram**, **svar**, eller **begära**.</span><span class="sxs-lookup"><span data-stu-id="cf59e-183">hello valid **Message Types** are **Datagram**, **Reply**, or **Request**.</span></span>  
<span data-ttu-id="cf59e-184">![Skicka meddelande sammanställer](media/connectors-create-api-mq/Send_Msg_Props.png)</span><span class="sxs-lookup"><span data-stu-id="cf59e-184">![Send Msg Props](media/connectors-create-api-mq/Send_Msg_Props.png)</span></span>

2. <span data-ttu-id="cf59e-185">hello utdata för Skicka meddelande ser ut så hello följande:</span><span class="sxs-lookup"><span data-stu-id="cf59e-185">hello output of Send message looks like hello following:</span></span>  
![Skicka meddelande](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a><span data-ttu-id="cf59e-187">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="cf59e-187">Connector-specific details</span></span>

<span data-ttu-id="cf59e-188">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/mq/).</span><span class="sxs-lookup"><span data-stu-id="cf59e-188">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/mq/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf59e-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cf59e-189">Next steps</span></span>
<span data-ttu-id="cf59e-190">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="cf59e-190">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="cf59e-191">Utforska hello andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="cf59e-191">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
