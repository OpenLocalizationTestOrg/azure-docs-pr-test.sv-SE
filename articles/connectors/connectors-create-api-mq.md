---
title: "Lär dig hur du använder MQ-anslutningen i Azure Logic Apps | Microsoft Docs"
description: "Ansluta till en lokal eller Azure MQ-server från logik app arbetsflödet att bläddra, ta emot och skicka meddelanden till WebSphere MQ"
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
ms.openlocfilehash: 17c651585b56dae186286f5d8c68c363ae9c524d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="connect-to-an-ibm-mq-server-from-logic-apps-using-the-mq-connector"></a><span data-ttu-id="b022b-103">Ansluta till en IBM MQ-server från logikappar med MQ-koppling</span><span class="sxs-lookup"><span data-stu-id="b022b-103">Connect to an IBM MQ server from logic apps using the MQ connector</span></span> 

<span data-ttu-id="b022b-104">Microsoft Connector för MQ skickar och tar emot meddelanden som lagras i ett MQ-Server lokalt eller i Azure.</span><span class="sxs-lookup"><span data-stu-id="b022b-104">Microsoft Connector for MQ sends and retrieves messages stored in an MQ Server on-premises, or in Azure.</span></span> <span data-ttu-id="b022b-105">Denna koppling inkluderar en Microsoft MQ-klient som kommunicerar med en fjärransluten IBM MQ-server i ett TCP/IP-nätverk.</span><span class="sxs-lookup"><span data-stu-id="b022b-105">This connector includes a Microsoft MQ client that communicates with a remote IBM MQ server across a TCP/IP network.</span></span> <span data-ttu-id="b022b-106">Det här dokumentet är en start-guide för att använda anslutningstjänsten MQ.</span><span class="sxs-lookup"><span data-stu-id="b022b-106">This document is a starter guide to use the MQ connector.</span></span> <span data-ttu-id="b022b-107">Vi rekommenderar att du börjar genom att bläddra ett enda meddelande i en kö och försök andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b022b-107">We recommended you begin by browsing a single message on a queue, and then trying the other actions.</span></span>    

<span data-ttu-id="b022b-108">MQ-koppling inkluderar följande åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b022b-108">The MQ connector includes the following actions.</span></span> <span data-ttu-id="b022b-109">Det finns inga utlösare.</span><span class="sxs-lookup"><span data-stu-id="b022b-109">There are no triggers.</span></span>

-   <span data-ttu-id="b022b-110">Bläddra i ett enda meddelande utan att ta bort meddelandet från IBM MQ-Server</span><span class="sxs-lookup"><span data-stu-id="b022b-110">Browse a single message without deleting the message from the IBM MQ Server</span></span>
-   <span data-ttu-id="b022b-111">Bläddra i en grupp med meddelanden utan att ta bort meddelandena från IBM MQ-Server</span><span class="sxs-lookup"><span data-stu-id="b022b-111">Browse a batch of messages without deleting the messages from the IBM MQ Server</span></span>
-   <span data-ttu-id="b022b-112">Ett enda meddelande och ta bort meddelandet från IBM MQ-Server</span><span class="sxs-lookup"><span data-stu-id="b022b-112">Receive a single message and delete the message from the IBM MQ Server</span></span>
-   <span data-ttu-id="b022b-113">Ta emot en grupp med meddelanden och ta bort meddelandena från IBM MQ-Server</span><span class="sxs-lookup"><span data-stu-id="b022b-113">Receive a batch of messages and delete the messages from the IBM MQ Server</span></span>
-   <span data-ttu-id="b022b-114">Skicka ett meddelande till IBM MQ-Server</span><span class="sxs-lookup"><span data-stu-id="b022b-114">Send a single message to the IBM MQ Server</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b022b-115">Krav</span><span class="sxs-lookup"><span data-stu-id="b022b-115">Prerequisites</span></span>

* <span data-ttu-id="b022b-116">Om du använder en lokal MQ-server [installera lokala datagateway](../logic-apps/logic-apps-gateway-install.md) på en server i nätverket.</span><span class="sxs-lookup"><span data-stu-id="b022b-116">If using an on-premises MQ server, [install the on-premises data gateway](../logic-apps/logic-apps-gateway-install.md) on a server within your network.</span></span> <span data-ttu-id="b022b-117">Om MQ-Server är offentligt tillgänglig, eller tillgängliga i Azure, sedan datagateway inte används eller krävs.</span><span class="sxs-lookup"><span data-stu-id="b022b-117">If the MQ Server is publicly available, or available within Azure, then the data gateway is not used or required.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b022b-118">Den server där lokala Data Gateway har installerats måste också ha .net Framework 4.6 installerad för MQ-anslutningen ska fungera.</span><span class="sxs-lookup"><span data-stu-id="b022b-118">The server where the On-Premises Data Gateway is installed must also have .Net Framework 4.6 installed for the MQ Connector to function.</span></span>

* <span data-ttu-id="b022b-119">Skapa Azure-resurs för lokala datagateway - [konfigurera dataanslutningen gateway](../logic-apps/logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="b022b-119">Create the Azure resource for the on-premises data gateway - [Set up the data gateway connection](../logic-apps/logic-apps-gateway-connection.md).</span></span>

* <span data-ttu-id="b022b-120">Officiellt IBM WebSphere MQ-versioner som stöds:</span><span class="sxs-lookup"><span data-stu-id="b022b-120">Officially supported IBM WebSphere MQ versions:</span></span>
   * <span data-ttu-id="b022b-121">MQ 7.5</span><span class="sxs-lookup"><span data-stu-id="b022b-121">MQ 7.5</span></span>
   * <span data-ttu-id="b022b-122">MQ 8.0</span><span class="sxs-lookup"><span data-stu-id="b022b-122">MQ 8.0</span></span>

## <a name="create-a-logic-app"></a><span data-ttu-id="b022b-123">Skapa en logikapp</span><span class="sxs-lookup"><span data-stu-id="b022b-123">Create a logic app</span></span>

1. <span data-ttu-id="b022b-124">I den **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="b022b-124">In the **Azure start board**, select **+** (plus sign), **Web + Mobile**, and then **Logic App**.</span></span> 
2. <span data-ttu-id="b022b-125">Ange den **namn**, till exempel MQTestApp, **prenumeration**, **resursgruppen**, och **plats** (använda den plats där den lokala Data Gateway-anslutningen har konfigurerats).</span><span class="sxs-lookup"><span data-stu-id="b022b-125">Enter the **Name**, such as MQTestApp, **Subscription**, **Resource group**, and **Location** (use the location where the on-premises Data Gateway connection is configured).</span></span> <span data-ttu-id="b022b-126">Välj **fäst på instrumentpanelen**, och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b022b-126">Select **Pin to dashboard**, and select **Create**.</span></span>  
<span data-ttu-id="b022b-127">![Skapa Logikapp](media/connectors-create-api-mq/Create_Logic_App.png)</span><span class="sxs-lookup"><span data-stu-id="b022b-127">![Create Logic App](media/connectors-create-api-mq/Create_Logic_App.png)</span></span>

## <a name="add-a-trigger"></a><span data-ttu-id="b022b-128">Lägg till en utlösare</span><span class="sxs-lookup"><span data-stu-id="b022b-128">Add a trigger</span></span>

> [!NOTE]
> <span data-ttu-id="b022b-129">MQ-anslutningen har inte utlösare.</span><span class="sxs-lookup"><span data-stu-id="b022b-129">The MQ Connector does not have any triggers.</span></span> <span data-ttu-id="b022b-130">Använda en annan utlösare så för att starta din logikapp som den **återkommande** utlösare.</span><span class="sxs-lookup"><span data-stu-id="b022b-130">So, use another trigger to start your logic app, such as the **Recurrence** trigger.</span></span> 

1. <span data-ttu-id="b022b-131">Den **Logic Apps Designer** öppnas väljer **återkommande** i listan över vanliga utlösare.</span><span class="sxs-lookup"><span data-stu-id="b022b-131">The **Logic Apps Designer** opens, select **Recurrence** in the list of common triggers.</span></span>
2. <span data-ttu-id="b022b-132">Välj **redigera** i återkommande utlösare.</span><span class="sxs-lookup"><span data-stu-id="b022b-132">Select **Edit** within the Recurrence Trigger.</span></span> 
3. <span data-ttu-id="b022b-133">Ange den **frekvens** till **dag**, och ange den **intervall** till **7**.</span><span class="sxs-lookup"><span data-stu-id="b022b-133">Set the **Frequency** to **Day**, and set the **Interval** to **7**.</span></span> 

## <a name="browse-a-single-message"></a><span data-ttu-id="b022b-134">Bläddra i ett enda meddelande</span><span class="sxs-lookup"><span data-stu-id="b022b-134">Browse a single message</span></span>
1. <span data-ttu-id="b022b-135">Välj **+ nytt steg**, och välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="b022b-135">Select **+ New step**, and select **Add an action**.</span></span>
2. <span data-ttu-id="b022b-136">I sökrutan skriver `mq`, och välj sedan **MQ - Bläddra meddelandet**.</span><span class="sxs-lookup"><span data-stu-id="b022b-136">In the search box, type `mq`, and then select **MQ - Browse message**.</span></span>  
<span data-ttu-id="b022b-137">![Bläddra meddelande](media/connectors-create-api-mq/Browse_message.png)</span><span class="sxs-lookup"><span data-stu-id="b022b-137">![Browse message](media/connectors-create-api-mq/Browse_message.png)</span></span>

3. <span data-ttu-id="b022b-138">Om det inte finns en befintlig MQ-anslutning, skapar du anslutningen:</span><span class="sxs-lookup"><span data-stu-id="b022b-138">If there isn't an existing MQ connection, then create the connection:</span></span>  

    1. <span data-ttu-id="b022b-139">Välj **Anslut via lokala datagateway**, och ange egenskaperna för MQ-server.</span><span class="sxs-lookup"><span data-stu-id="b022b-139">Select **Connect via on-premises data gateway**, and enter the properties of your MQ server.</span></span>  
    <span data-ttu-id="b022b-140">För **Server**, du kan ange namnet på MQ-server eller ange IP-adressen följt av ett kolon och portnumret.</span><span class="sxs-lookup"><span data-stu-id="b022b-140">For **Server**, you can enter the MQ server name, or enter the IP address followed by a colon and the port number.</span></span> 
    2. <span data-ttu-id="b022b-141">Den **gateway** listan visar alla befintliga gateway-anslutningar som har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="b022b-141">The **gateway** dropdown lists any existing gateway connections that have been configured.</span></span> <span data-ttu-id="b022b-142">Välj din gateway.</span><span class="sxs-lookup"><span data-stu-id="b022b-142">Select your gateway.</span></span>
    3. <span data-ttu-id="b022b-143">Välj **skapa** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="b022b-143">Select **Create** when finished.</span></span> <span data-ttu-id="b022b-144">Anslutningen ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="b022b-144">Your connection looks similar to the following:</span></span>   
    <span data-ttu-id="b022b-145">![Anslutningsegenskaper](media/connectors-create-api-mq/Connection_Properties.png)</span><span class="sxs-lookup"><span data-stu-id="b022b-145">![Connection Properties](media/connectors-create-api-mq/Connection_Properties.png)</span></span>

4. <span data-ttu-id="b022b-146">I Egenskaper för åtgärden kan du:</span><span class="sxs-lookup"><span data-stu-id="b022b-146">In the action properties, you can:</span></span>  

    * <span data-ttu-id="b022b-147">Använd den **kön** egenskapen åtkomst till en annan kö namn än vad som har definierats i anslutningen</span><span class="sxs-lookup"><span data-stu-id="b022b-147">Use the **Queue** property to access a different queue name than what is defined in the connection</span></span>
    * <span data-ttu-id="b022b-148">Använd den **MessageId**, **CorrelationId**, **GroupId**, och andra egenskaper för att leta efter ett meddelande utifrån olika egenskaper för MQ-meddelande</span><span class="sxs-lookup"><span data-stu-id="b022b-148">Use the **MessageId**, **CorrelationId**, **GroupId**, and other properties to browse for a message based on the different MQ message properties</span></span>
    * <span data-ttu-id="b022b-149">Ange **IncludeInfo** till **SANT** att inkludera ytterligare meddelandeinformation i utdata.</span><span class="sxs-lookup"><span data-stu-id="b022b-149">Set **IncludeInfo** to **True** to include additional message information in the output.</span></span> <span data-ttu-id="b022b-150">Eller anger **FALSKT** att inte inkludera ytterligare meddelandeinformation i utdata.</span><span class="sxs-lookup"><span data-stu-id="b022b-150">Or, set it to **False** to not include additional message information in the output.</span></span>
    * <span data-ttu-id="b022b-151">Ange en **Timeout** värde för att bestämma hur lång tid att vänta på ett meddelande som ska tas emot i en tom kö.</span><span class="sxs-lookup"><span data-stu-id="b022b-151">Enter a **Timeout** value to determine how long to wait for a message to arrive in an empty queue.</span></span> <span data-ttu-id="b022b-152">Om inget anges det första meddelandet i kön har hämtats och det finns ingen tid väntar på att ett meddelande ska visas.</span><span class="sxs-lookup"><span data-stu-id="b022b-152">If nothing is entered, the first message in the queue is retrieved, and there is no time spent waiting for a message to appear.</span></span>  
    <span data-ttu-id="b022b-153">![Bläddra meddelandeegenskaper](media/connectors-create-api-mq/Browse_message_Props.png)</span><span class="sxs-lookup"><span data-stu-id="b022b-153">![Browse Message Properties](media/connectors-create-api-mq/Browse_message_Props.png)</span></span>

5. <span data-ttu-id="b022b-154">**Spara** dina ändringar och sedan **kör** logikappen:</span><span class="sxs-lookup"><span data-stu-id="b022b-154">**Save** your changes, and then **Run** your logic app:</span></span>  
<span data-ttu-id="b022b-155">![Spara och köra](media/connectors-create-api-mq/Save_Run.png)</span><span class="sxs-lookup"><span data-stu-id="b022b-155">![Save and run](media/connectors-create-api-mq/Save_Run.png)</span></span>

6. <span data-ttu-id="b022b-156">Efter några sekunder visas hur du kör och titta på utdata.</span><span class="sxs-lookup"><span data-stu-id="b022b-156">After a few seconds, the steps of the run are shown, and you can look at the output.</span></span> <span data-ttu-id="b022b-157">Välj en grön markering visas detaljerad information om varje steg.</span><span class="sxs-lookup"><span data-stu-id="b022b-157">Select the green checkmark to see details of each step.</span></span> <span data-ttu-id="b022b-158">Välj **finns rådata utdata** vill visa ytterligare information om utdata.</span><span class="sxs-lookup"><span data-stu-id="b022b-158">Select **See raw outputs** to see additional details on the output data.</span></span>  
<span data-ttu-id="b022b-159">![Bläddra meddelandet utdata](media/connectors-create-api-mq/Browse_message_output.png)</span><span class="sxs-lookup"><span data-stu-id="b022b-159">![Browse message output](media/connectors-create-api-mq/Browse_message_output.png)</span></span>  

    <span data-ttu-id="b022b-160">Rådata utdata:</span><span class="sxs-lookup"><span data-stu-id="b022b-160">Raw output:</span></span>  
    ![Bläddra meddelandet raw-utdata](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. <span data-ttu-id="b022b-162">När den **IncludeInfo** alternativet är inställt på true, visas följande utdata:</span><span class="sxs-lookup"><span data-stu-id="b022b-162">When the **IncludeInfo** option is set to true, the following output is displayed:</span></span>  
<span data-ttu-id="b022b-163">![Bläddra meddelandet innehålla info](media/connectors-create-api-mq/Browse_message_Include_Info.png)</span><span class="sxs-lookup"><span data-stu-id="b022b-163">![Browse message include info](media/connectors-create-api-mq/Browse_message_Include_Info.png)</span></span>

## <a name="browse-multiple-messages"></a><span data-ttu-id="b022b-164">Bläddra flera meddelanden</span><span class="sxs-lookup"><span data-stu-id="b022b-164">Browse multiple messages</span></span>
<span data-ttu-id="b022b-165">Den **Bläddra meddelanden** åtgärden inkluderar en **BatchSize** alternativ för att ange hur många meddelanden som ska returneras från kön.</span><span class="sxs-lookup"><span data-stu-id="b022b-165">The **Browse messages** action includes a **BatchSize** option to indicate how many messages should be returned from the queue.</span></span>  <span data-ttu-id="b022b-166">Om **BatchSize** har ingen post returneras alla meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b022b-166">If **BatchSize** has no entry, all messages are returned.</span></span> <span data-ttu-id="b022b-167">Det returnerade resultatet är en matris av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b022b-167">The returned output is an array of messages.</span></span>

1. <span data-ttu-id="b022b-168">När du lägger till den **Bläddra meddelanden** åtgärd, den första anslutningen har konfigurerats är markerad som standard.</span><span class="sxs-lookup"><span data-stu-id="b022b-168">When adding the **Browse messages** action, the first connection that is configured is selected by default.</span></span> <span data-ttu-id="b022b-169">Välj **ändra anslutningen** att skapa en ny anslutning eller välj en annan anslutning.</span><span class="sxs-lookup"><span data-stu-id="b022b-169">Select **Change connection** to create a new connection, or select a different connection.</span></span>

2. <span data-ttu-id="b022b-170">Utdata från Bläddra meddelanden visas:</span><span class="sxs-lookup"><span data-stu-id="b022b-170">The output of Browse messages shows:</span></span>  
![Bläddra meddelanden utdata](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a><span data-ttu-id="b022b-172">Ett enda meddelande</span><span class="sxs-lookup"><span data-stu-id="b022b-172">Receive a single message</span></span>
<span data-ttu-id="b022b-173">Den **ta emot meddelandet** åtgärden har samma indata och utdata som den **Bläddra meddelandet** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="b022b-173">The **Receive message** action has the same inputs and outputs as the **Browse message** action.</span></span> <span data-ttu-id="b022b-174">När du använder **ta emot meddelandet**, tas meddelandet bort från kön.</span><span class="sxs-lookup"><span data-stu-id="b022b-174">When using **Receive message**, the message is deleted from the queue.</span></span>

## <a name="receive-multiple-messages"></a><span data-ttu-id="b022b-175">Ta emot flera meddelanden</span><span class="sxs-lookup"><span data-stu-id="b022b-175">Receive multiple messages</span></span>
<span data-ttu-id="b022b-176">Den **ta emot meddelanden** åtgärden har samma indata och utdata som den **Bläddra meddelanden** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="b022b-176">The **Receive messages** action has the same inputs and outputs as the **Browse messages** action.</span></span> <span data-ttu-id="b022b-177">När du använder **ta emot meddelanden**, meddelandena tas bort från kön.</span><span class="sxs-lookup"><span data-stu-id="b022b-177">When using **Receive messages**, the messages are deleted from the queue.</span></span>

<span data-ttu-id="b022b-178">Om det finns inga meddelanden i kön när du gör en Bläddra eller en receive, misslyckas steget med följande utdata:</span><span class="sxs-lookup"><span data-stu-id="b022b-178">If there are no messages in the queue when doing a browse or a receive, the step fails with the following output:</span></span>  
![MQ Nej visas fel](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a><span data-ttu-id="b022b-180">Skicka ett meddelande</span><span class="sxs-lookup"><span data-stu-id="b022b-180">Send a message</span></span>
1. <span data-ttu-id="b022b-181">När du lägger till den **skickar ett meddelande** åtgärd, den första anslutningen har konfigurerats är markerad som standard.</span><span class="sxs-lookup"><span data-stu-id="b022b-181">When adding the **Send message** action, the first connection that is configured is selected by default.</span></span> <span data-ttu-id="b022b-182">Välj **ändra anslutningen** att skapa en ny anslutning eller välj en annan anslutning.</span><span class="sxs-lookup"><span data-stu-id="b022b-182">Select **Change connection** to create a new connection, or select a different connection.</span></span> <span data-ttu-id="b022b-183">Det giltiga **meddelandetyper** är **Datagram**, **svar**, eller **begära**.</span><span class="sxs-lookup"><span data-stu-id="b022b-183">The valid **Message Types** are **Datagram**, **Reply**, or **Request**.</span></span>  
<span data-ttu-id="b022b-184">![Skicka meddelande sammanställer](media/connectors-create-api-mq/Send_Msg_Props.png)</span><span class="sxs-lookup"><span data-stu-id="b022b-184">![Send Msg Props](media/connectors-create-api-mq/Send_Msg_Props.png)</span></span>

2. <span data-ttu-id="b022b-185">Utdata för Skicka meddelande ser ut som följande:</span><span class="sxs-lookup"><span data-stu-id="b022b-185">The output of Send message looks like the following:</span></span>  
![Skicka meddelande](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a><span data-ttu-id="b022b-187">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="b022b-187">Connector-specific details</span></span>

<span data-ttu-id="b022b-188">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/mq/).</span><span class="sxs-lookup"><span data-stu-id="b022b-188">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/mq/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b022b-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b022b-189">Next steps</span></span>
<span data-ttu-id="b022b-190">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="b022b-190">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="b022b-191">Utforska andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="b022b-191">Explore the other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
