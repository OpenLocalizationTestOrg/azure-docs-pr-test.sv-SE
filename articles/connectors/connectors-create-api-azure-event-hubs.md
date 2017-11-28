---
title: "aaaSet in Händelseövervakare med Händelsehubbar i Azure för Logikappar i Azure | Microsoft Docs"
description: "Övervaka dataströmmar tooreceive händelser och skicka händelser för Logikappar i Azure med Azure Event Hubs"
services: logic-apps
keywords: "dataströmmen, övervakning, händelsehubbar"
author: ecfan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: estfan; LADocs
ms.openlocfilehash: 4aad2c2ac1134b4d4d440019b4773559e49be122
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-receive-and-send-events-with-hello-event-hubs-connector"></a><span data-ttu-id="879a7-104">Övervaka, ta emot och skicka händelser med hello Händelsehubbar connector</span><span class="sxs-lookup"><span data-stu-id="879a7-104">Monitor, receive, and send events with hello Event Hubs connector</span></span>

<span data-ttu-id="879a7-105">tooset upp en Händelseövervakare så att din logikapp kan identifiera händelser, ta emot händelser och skicka händelser, ansluta tooan [Azure Event Hub](https://azure.microsoft.com/services/event-hubs) från din logikapp.</span><span class="sxs-lookup"><span data-stu-id="879a7-105">tooset up an event monitor so that your logic app can detect events, receive events, and send events, connect tooan [Azure Event Hub](https://azure.microsoft.com/services/event-hubs) from your logic app.</span></span> <span data-ttu-id="879a7-106">Lär dig mer om [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="879a7-106">Learn more about [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>

## <a name="requirements"></a><span data-ttu-id="879a7-107">Krav</span><span class="sxs-lookup"><span data-stu-id="879a7-107">Requirements</span></span>

* <span data-ttu-id="879a7-108">Du har toohave en [Händelsehubbar namnområde och Event Hub](../event-hubs/event-hubs-create.md) i Azure.</span><span class="sxs-lookup"><span data-stu-id="879a7-108">You have toohave an [Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md) in Azure.</span></span> <span data-ttu-id="879a7-109">Läs [hur toocreate en Händelsehubbar namnområde och Event Hub](../event-hubs/event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="879a7-109">Learn [how toocreate an Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md).</span></span> 

* <span data-ttu-id="879a7-110">toouse [alla anslutningar](https://docs.microsoft.com/azure/connectors/apis-list) i din logikapp, har du toocreate en logikapp först.</span><span class="sxs-lookup"><span data-stu-id="879a7-110">toouse [any connector](https://docs.microsoft.com/azure/connectors/apis-list) in your logic app, you have toocreate a logic app first.</span></span> <span data-ttu-id="879a7-111">Läs [hur toocreate en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="879a7-111">Learn [how toocreate a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

<a name="permissions-connection-string"></a>
## <a name="check-event-hubs-namespace-permissions-and-find-hello-connection-string"></a><span data-ttu-id="879a7-112">Kontrollera behörigheter för Händelsehubbar namnområde och hitta hello-anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="879a7-112">Check Event Hubs namespace permissions and find hello connection string</span></span>

<span data-ttu-id="879a7-113">För dina logic app tooaccess tjänsten något du har toocreate en [ *anslutning* ](./connectors-overview.md) mellan logik appen och hello tjänsten om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="879a7-113">For your logic app tooaccess any service, you have toocreate a [*connection*](./connectors-overview.md) between your logic app and hello service, if you haven't already.</span></span> <span data-ttu-id="879a7-114">Den här anslutningen auktoriserar tooaccess för logik appdata.</span><span class="sxs-lookup"><span data-stu-id="879a7-114">This connection authorizes your logic app tooaccess data.</span></span>
<span data-ttu-id="879a7-115">Din logik app tooaccess din Händelsehubb har toohave **hantera** behörigheter och hello anslutningssträngen för namnområdet Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="879a7-115">For your logic app tooaccess your Event Hub, you have toohave **Manage** permissions and hello connection string for your Event Hubs namespace.</span></span>

<span data-ttu-id="879a7-116">toocheck dina behörigheter och hämta hello anslutningssträngen, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="879a7-116">toocheck your permissions and get hello connection string, follow these steps.</span></span>

1.  <span data-ttu-id="879a7-117">Logga in toohello [Azure-portalen](https://portal.azure.com "Azure-portalen").</span><span class="sxs-lookup"><span data-stu-id="879a7-117">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span> 

2.  <span data-ttu-id="879a7-118">Gå tooyour Händelsehubbar *namnområde*, inte hello specifik Händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="879a7-118">Go tooyour Event Hubs *namespace*, not hello specific Event Hub.</span></span> <span data-ttu-id="879a7-119">Hello namnområde bladet under **inställningar**, Välj **principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="879a7-119">On hello namespace blade, under **Settings**, choose **Shared access policies**.</span></span> <span data-ttu-id="879a7-120">Under **anspråk**, kontrollera att du har **hantera** behörigheter för det namnområdet.</span><span class="sxs-lookup"><span data-stu-id="879a7-120">Under **Claims**, check that you have **Manage** permissions for that namespace.</span></span>

    ![Hantera behörigheter för namnområdet Event Hub](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3.  <span data-ttu-id="879a7-122">toocopy hello anslutningssträngen för hello Händelsehubbar namnområde, Välj **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="879a7-122">toocopy hello connection string for hello Event Hubs namespace, choose **RootManageSharedAccessKey**.</span></span> <span data-ttu-id="879a7-123">Nästa tooyour primära nyckeln anslutningssträngen, Välj hello kopia.</span><span class="sxs-lookup"><span data-stu-id="879a7-123">Next tooyour primary key connection string, choose hello copy button.</span></span>

    ![Kopiera anslutningssträngen för Händelsehubbar namnområde](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > <span data-ttu-id="879a7-125">tooconfirm om anslutningssträngen är associerad med Händelsehubbar namnområdet eller med en specifik Händelsehubb Kontrollera hello anslutningssträngen för hello `EntityPath` parameter.</span><span class="sxs-lookup"><span data-stu-id="879a7-125">tooconfirm whether your connection string is associated with your Event Hubs namespace or with a specific Event Hub, check hello connection string for hello `EntityPath` parameter.</span></span> <span data-ttu-id="879a7-126">Om du hittar den här parametern hello anslutningssträngen är för en specifik Händelsehubb ”enhet” och är inte hello rätt sträng toouse med din logikapp.</span><span class="sxs-lookup"><span data-stu-id="879a7-126">If you find this parameter, hello connection string is for a specific Event Hub "entity", and is not hello correct string toouse with your logic app.</span></span>

4.  <span data-ttu-id="879a7-127">Nu när du uppmanas ange autentiseringsuppgifter när du lägger till en Händelsehubbar utlösare eller åtgärd tooyour logikapp, kan du ansluta tooyour Händelsehubbar namnområde.</span><span class="sxs-lookup"><span data-stu-id="879a7-127">Now when you're prompted for credentials after adding an Event Hubs trigger or action tooyour logic app, you can connect tooyour Event Hubs namespace.</span></span> <span data-ttu-id="879a7-128">Namnge din anslutning, ange hello anslutningssträng som du kopierade och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="879a7-128">Give your connection a name, enter hello connection string that you copied, and choose **Create**.</span></span>

    ![Ange anslutningssträng för Händelsehubbar namnområde](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    <span data-ttu-id="879a7-130">När du har skapat din anslutning ska hello anslutningsnamn visas i hello Händelsehubbar utlösare eller åtgärd.</span><span class="sxs-lookup"><span data-stu-id="879a7-130">After you create your connection, hello connection name should appear in hello Event Hubs trigger or action.</span></span> 
    <span data-ttu-id="879a7-131">Du kan sedan fortsätta med hello andra steg i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="879a7-131">You can then continue with hello other steps in your logic app.</span></span>

    ![Event Hubs namnområde anslutning som skapats](./media/connectors-create-api-azure-event-hubs/event-hubs-connection-created.png)

## <a name="start-workflow-when-your-event-hub-receives-new-events"></a><span data-ttu-id="879a7-133">Starta arbetsflödet när din Event Hub tar emot nya händelser</span><span class="sxs-lookup"><span data-stu-id="879a7-133">Start workflow when your Event Hub receives new events</span></span>

<span data-ttu-id="879a7-134">En [ *utlösaren* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) är en händelse som startar ett arbetsflöde i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="879a7-134">A [*trigger*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) is an event that starts a workflow in your logic app.</span></span> <span data-ttu-id="879a7-135">Följ dessa steg för att lägga till hello utlösare som identifierar den här händelsen om du vill starta ett arbetsflöde när nya händelser skickas tooyour Event Hub.</span><span class="sxs-lookup"><span data-stu-id="879a7-135">To start a workflow when new events are sent tooyour Event Hub, follow these steps for adding hello trigger that detects this event.</span></span>

1.  <span data-ttu-id="879a7-136">I hello [Azure-portalen](https://portal.azure.com "Azure-portalen"), gå tooyour befintliga logikapp eller skapa en tom logikapp.</span><span class="sxs-lookup"><span data-stu-id="879a7-136">In hello [Azure portal](https://portal.azure.com "Azure portal"), go tooyour existing logic app or create a blank logic app.</span></span>

2.  <span data-ttu-id="879a7-137">Skriv i sökrutan för hello logik App Designer hello `event hubs` för filtret.</span><span class="sxs-lookup"><span data-stu-id="879a7-137">In hello search box for hello Logic App Designer, enter `event hubs` for your filter.</span></span> <span data-ttu-id="879a7-138">Välj den här utlösaren: **när händelser är tillgängliga i Event Hub**</span><span class="sxs-lookup"><span data-stu-id="879a7-138">Select this trigger: **When events are available in Event Hub**</span></span>

    ![Välj utlösare för när din Event Hub tar emot nya händelser](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

    <span data-ttu-id="879a7-140">Om du inte redan har en anslutning tooyour Händelsehubbar namnrymd, uppmanas du toocreate nu den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="879a7-140">If you don't already have a connection tooyour Event Hubs namespace, you're prompted toocreate this connection now.</span></span> <span data-ttu-id="879a7-141">Namnge din anslutning och ange hello anslutningssträng för namnområdet Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="879a7-141">Give your connection a name, and enter hello connection string for your Event Hubs namespace.</span></span> 
    <span data-ttu-id="879a7-142">Lär dig mer om det behövs [hur toofind anslutningssträngen](#permissions-connection-string).</span><span class="sxs-lookup"><span data-stu-id="879a7-142">If necessary, learn [how toofind your connection string](#permissions-connection-string).</span></span>

    ![Ange anslutningssträng för Händelsehubbar namnområde](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    <span data-ttu-id="879a7-144">När du har skapat hello anslutning hello inställningar för hello **när en händelse i finns i en Händelsehubb** utlösaren visas.</span><span class="sxs-lookup"><span data-stu-id="879a7-144">After you create hello connection, hello settings for hello **When an event in available in an Event Hub** trigger appear.</span></span>

    ![Med hjälp av inställningar för när din Event Hub tar emot nya händelser](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

3.  <span data-ttu-id="879a7-146">Ange eller välj hello namn för hello Event Hub som du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="879a7-146">Enter or select hello name for hello Event Hub that you want toomonitor.</span></span> <span data-ttu-id="879a7-147">Välj hello frekvensen och intervall för hur ofta du vill att toocheck hello Event Hub.</span><span class="sxs-lookup"><span data-stu-id="879a7-147">Select hello frequency and interval for how often you want toocheck hello Event Hub.</span></span>

    > [!TIP]
    > <span data-ttu-id="879a7-148">toooptionally markerar du en konsumentgrupp för att läsa händelser, väljer **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="879a7-148">toooptionally select a consumer group for reading events, choose **Show advanced options**.</span></span> 

    ![Ange Händelsehubb eller konsumentgrupp](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-details.png)

    <span data-ttu-id="879a7-150">Du har nu skapat en utlösare toostart ett arbetsflöde för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="879a7-150">You've now set up a trigger toostart a workflow for your logic app.</span></span> 
    <span data-ttu-id="879a7-151">Din logikapp kontrollerar hello angetts Event Hub baserat på hello-schema som du har angett.</span><span class="sxs-lookup"><span data-stu-id="879a7-151">Your logic app checks hello specified Event Hub based on hello schedule that you set.</span></span> 
    <span data-ttu-id="879a7-152">Om din app hittar nya händelser i hello Event Hub, kör hello trigger andra åtgärder eller utlösare i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="879a7-152">If your app finds new events in hello Event Hub, hello trigger runs other actions or triggers in your logic app.</span></span>

## <a name="send-events-tooyour-event-hub-from-your-logic-app"></a><span data-ttu-id="879a7-153">Skicka händelser tooyour Händelsehubb från din logikapp</span><span class="sxs-lookup"><span data-stu-id="879a7-153">Send events tooyour Event Hub from your logic app</span></span>

<span data-ttu-id="879a7-154">En [*åtgärd*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) är en aktivitet som utförs av logikapparbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="879a7-154">An [*action*](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) is a task performed by your logic app workflow.</span></span> <span data-ttu-id="879a7-155">När du lägger till en utlösare tooyour logikapp, du kan lägga till en åtgärd tooperform åtgärder med data som genereras av den här utlösaren.</span><span class="sxs-lookup"><span data-stu-id="879a7-155">After you add a trigger tooyour logic app, you can add an action tooperform operations with data generated by that trigger.</span></span> <span data-ttu-id="879a7-156">toosend händelse-tooyour Händelsehubb från din logikapp, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="879a7-156">toosend an event tooyour Event Hub from your logic app, follow these steps.</span></span>

1.  <span data-ttu-id="879a7-157">I logik App Designer under din logik apputlösare väljer **nytt steg** > **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="879a7-157">In Logic App Designer, under your logic app trigger, choose **New step** > **Add an action**.</span></span>

    ![Sedan Välj ”nytt steg”, ”Lägg till en åtgärd”](./media/connectors-create-api-azure-event-hubs/add-action.png)

    <span data-ttu-id="879a7-159">Nu kan du hitta och välja en åtgärd tooperform.</span><span class="sxs-lookup"><span data-stu-id="879a7-159">Now you can find and select an action tooperform.</span></span> 
    <span data-ttu-id="879a7-160">Men du kan välja en åtgärd, t.ex, vill vi hello Händelsehubbar åtgärd toosend händelser.</span><span class="sxs-lookup"><span data-stu-id="879a7-160">Although you can select any action, for this example, we want hello Event Hubs action toosend events.</span></span>

2.  <span data-ttu-id="879a7-161">Skriv i sökrutan hello `event hubs` för filtret.</span><span class="sxs-lookup"><span data-stu-id="879a7-161">In hello search box, enter `event hubs` for your filter.</span></span>
<span data-ttu-id="879a7-162">Välj den här åtgärden: **överföringshändelse**</span><span class="sxs-lookup"><span data-stu-id="879a7-162">Select this action: **Send event**</span></span>

    ![Välj ”Händelsehubbar - överföringshändelse” åtgärd](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

3.  <span data-ttu-id="879a7-164">Ange hello krävs information om hello händelse, till exempel hello namn för hello Event Hub där du vill att toosend hello händelsen.</span><span class="sxs-lookup"><span data-stu-id="879a7-164">Enter hello required details for hello event, such as hello name for hello Event Hub where you want toosend hello event.</span></span> <span data-ttu-id="879a7-165">Ange andra valfria information om hello-händelse, t.ex innehåll för händelsen.</span><span class="sxs-lookup"><span data-stu-id="879a7-165">Enter any other optional details about hello event, such as content for that event.</span></span>

    > [!TIP]
    > <span data-ttu-id="879a7-166">toooptionally ange hello Event Hub partition där toosend hello-händelse, Välj **visa avancerade alternativ**.</span><span class="sxs-lookup"><span data-stu-id="879a7-166">toooptionally specify hello Event Hub partition where toosend hello event, choose **Show advanced options**.</span></span> 

    ![Ange Event Hub-namn och valfri händelseinformation](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

6.  <span data-ttu-id="879a7-168">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="879a7-168">Save your changes.</span></span>

    ![Spara din logikapp](./media/connectors-create-api-azure-event-hubs/save-logic-app.png)

    <span data-ttu-id="879a7-170">Du har nu skapat en åtgärd toosend händelser från din logikapp.</span><span class="sxs-lookup"><span data-stu-id="879a7-170">You've now set up an action toosend events from your logic app.</span></span> 

## <a name="connector-specific-details"></a><span data-ttu-id="879a7-171">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="879a7-171">Connector-specific details</span></span>

<span data-ttu-id="879a7-172">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/eventhubs/).</span><span class="sxs-lookup"><span data-stu-id="879a7-172">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/eventhubs/).</span></span> 

## <a name="get-help"></a><span data-ttu-id="879a7-173">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="879a7-173">Get help</span></span>

<span data-ttu-id="879a7-174">tooask frågor, besvara frågor och se vilka andra användare i Azure Logic Apps gör finns hello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="879a7-174">tooask questions, answer questions, and see what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="879a7-175">toohelp förbättra Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Logic Apps användaren feedbackwebbplats](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="879a7-175">toohelp improve Logic Apps and connectors, vote on or submit ideas at hello [Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="879a7-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="879a7-176">Next steps</span></span>

*  [<span data-ttu-id="879a7-177">Sök efter andra kopplingar till Azure logikappar</span><span class="sxs-lookup"><span data-stu-id="879a7-177">Find other connectors for Azure Logic apps</span></span>](./apis-list.md)