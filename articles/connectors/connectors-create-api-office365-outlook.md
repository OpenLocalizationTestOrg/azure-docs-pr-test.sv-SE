---
title: aaaAdd hello Office 365 Outlook connector i dina Logic Apps | Microsoft Docs
description: 'Skapa logikappar med Office 365 connector tooenable interaktion med Office 365. Exempel: skapa, redigera och uppdatera kontakter och Kalender-objekt.'
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2f6cc2c-bba2-493a-b0ba-841785462a80
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 86a573c9c54701de3d3f0500d19eaf545e0710ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-office-365-outlook-connector"></a><span data-ttu-id="17761-104">Kom igång med hello Office 365 Outlook connector</span><span class="sxs-lookup"><span data-stu-id="17761-104">Get started with hello Office 365 Outlook connector</span></span>
<span data-ttu-id="17761-105">hello Office 365 Outlook connector kan interaktion med Outlook i Office 365.</span><span class="sxs-lookup"><span data-stu-id="17761-105">hello Office 365 Outlook connector enables interaction with Outlook in Office 365.</span></span> <span data-ttu-id="17761-106">Använd den här kopplingen toocreate, redigera, och uppdatera kontakter och kalenderobjekt, och även hämta, skicka och svara tooemail.</span><span class="sxs-lookup"><span data-stu-id="17761-106">Use this connector toocreate, edit, and update contacts and calendar items, and also get, send, and reply tooemail.</span></span>

<span data-ttu-id="17761-107">Med Office 365 Outlook kan du:</span><span class="sxs-lookup"><span data-stu-id="17761-107">With Office 365 Outlook, you:</span></span>

* <span data-ttu-id="17761-108">Skapa arbetsflödet använder hello e-post och kalender funktioner i Office 365.</span><span class="sxs-lookup"><span data-stu-id="17761-108">Build your workflow using hello email and calendar features within Office 365.</span></span> 
* <span data-ttu-id="17761-109">Använd utlöser toostart arbetsflödet när det finns ett nytt e-postmeddelande när ett kalenderobjekt uppdateras med mera.</span><span class="sxs-lookup"><span data-stu-id="17761-109">Use triggers toostart your workflow when there is a new email, when a calendar item is updated, and more.</span></span>
* <span data-ttu-id="17761-110">Använd åtgärder toosend ett e-postmeddelande, skapa en ny händelse och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="17761-110">Use actions toosend an email, create a new calendar event, and more.</span></span> <span data-ttu-id="17761-111">Till exempel när det finns ett nytt objekt i Salesforce (en utlösare), skicka ett e-postmeddelande tooyour Office 365 Outlook (en åtgärd).</span><span class="sxs-lookup"><span data-stu-id="17761-111">For example, when there is a new object in Salesforce (a trigger), send an email tooyour Office 365 Outlook (an action).</span></span> 

<span data-ttu-id="17761-112">Det här avsnittet visar hur toouse hello Outlook för Office 365-kopplingen i en logikapp och även visar hello utlösare och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="17761-112">This topic shows you how toouse hello Office 365 Outlook connector in a logic app, and also lists hello triggers and actions.</span></span>

> [!NOTE]
> <span data-ttu-id="17761-113">Den här versionen av hello artikeln gäller tooLogic appar allmän tillgänglighet (GA).</span><span class="sxs-lookup"><span data-stu-id="17761-113">This version of hello article applies tooLogic Apps general availability (GA).</span></span>
> 
> 

<span data-ttu-id="17761-114">toolearn mer om Logic Apps finns [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="17761-114">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toooffice-365"></a><span data-ttu-id="17761-115">Ansluta tooOffice 365</span><span class="sxs-lookup"><span data-stu-id="17761-115">Connect tooOffice 365</span></span>
<span data-ttu-id="17761-116">Innan din logikapp får åtkomst till alla tjänster måste du först skapa en *anslutning* toohello service.</span><span class="sxs-lookup"><span data-stu-id="17761-116">Before your logic app can access any service, you first create a *connection* toohello service.</span></span> <span data-ttu-id="17761-117">En anslutning kan du ansluta en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="17761-117">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="17761-118">Till exempel tooconnect tooOffice 365 Outlook, måste du först en Office 365 *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="17761-118">For example, tooconnect tooOffice 365 Outlook, you first need an Office 365 *connection*.</span></span> <span data-ttu-id="17761-119">toocreate en anslutning, ange hello autentiseringsuppgifter som du vanligtvis använder tooaccess Hej tjänst du vill tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="17761-119">toocreate a connection, enter hello credentials you normally use tooaccess hello service you wish tooconnect to.</span></span> <span data-ttu-id="17761-120">Ange så hello autentiseringsuppgifter tooyour Office 365-konto toocreate hello anslutning med Office 365 Outlook.</span><span class="sxs-lookup"><span data-stu-id="17761-120">So with Office 365 Outlook, enter hello credentials tooyour Office 365 account toocreate hello connection.</span></span>

## <a name="create-hello-connection"></a><span data-ttu-id="17761-121">Skapa hello-anslutning</span><span class="sxs-lookup"><span data-stu-id="17761-121">Create hello connection</span></span>
> [!INCLUDE [Steps toocreate a connection tooOffice 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="17761-122">Använda en utlösare</span><span class="sxs-lookup"><span data-stu-id="17761-122">Use a trigger</span></span>
<span data-ttu-id="17761-123">En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="17761-123">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="17761-124">Utlösare avsöker ”” hello-tjänsten på ett intervall och frekvens som du vill.</span><span class="sxs-lookup"><span data-stu-id="17761-124">Triggers "poll" hello service at an interval and frequency that you want.</span></span> <span data-ttu-id="17761-125">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="17761-125">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="17761-126">Skriv ”office 365” tooget en lista över hello utlösare i hello logikapp:</span><span class="sxs-lookup"><span data-stu-id="17761-126">In hello logic app, type "office 365" tooget a list of hello triggers:</span></span>  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. <span data-ttu-id="17761-127">Välj **Office 365 Outlook - när en kommande händelse startas snart**.</span><span class="sxs-lookup"><span data-stu-id="17761-127">Select **Office 365 Outlook - When an upcoming event is starting soon**.</span></span> <span data-ttu-id="17761-128">Om det finns redan en anslutning, väljer du en kalender hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="17761-128">If a connection already exists, then select a calendar from hello drop-down list.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    <span data-ttu-id="17761-129">Om du tillfrågas toosign i kan sedan ange hello inloggningsnamnet information toocreate hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="17761-129">If you are prompted toosign in, then enter hello sign in details toocreate hello connection.</span></span> <span data-ttu-id="17761-130">[Skapa hello anslutning](connectors-create-api-office365-outlook.md#create-the-connection) i det här avsnittet listar hello steg.</span><span class="sxs-lookup"><span data-stu-id="17761-130">[Create hello connection](connectors-create-api-office365-outlook.md#create-the-connection) in this topic lists hello steps.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="17761-131">I det här exemplet hello logikapp körs när en händelse har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="17761-131">In this example, hello logic app runs when a calendar event is updated.</span></span> <span data-ttu-id="17761-132">toosee hello resultaten av den här utlösaren Lägg till en annan åtgärd som skickar ett SMS.</span><span class="sxs-lookup"><span data-stu-id="17761-132">toosee hello results of this trigger, add another action that sends you a text message.</span></span> <span data-ttu-id="17761-133">Lägg exempelvis till hello Twilio *skickar ett meddelande* åtgärd som texter när hello kalender händelse startar i 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="17761-133">For example, add hello Twilio *Send message* action that texts you when hello calendar event is starting in 15 minutes.</span></span> 
   > 
   > 
3. <span data-ttu-id="17761-134">Välj hello **redigera** knappen och ange hello **frekvens** och **intervall** värden.</span><span class="sxs-lookup"><span data-stu-id="17761-134">Select hello **Edit** button and set hello **Frequency** and **Interval** values.</span></span> <span data-ttu-id="17761-135">Till exempel om du vill hello utlösaren toopoll var 15: e minut, ange hello **frekvens** för**minut**, och ange hello **intervall** för**15**.</span><span class="sxs-lookup"><span data-stu-id="17761-135">For example, if you want hello trigger toopoll every 15 minutes, then set hello **Frequency** too**Minute**, and set hello **Interval** too**15**.</span></span> 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. <span data-ttu-id="17761-136">**Spara** dina ändringar (övre vänstra hörnet av hello verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="17761-136">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="17761-137">Din logikapp sparas och aktiveras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="17761-137">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="17761-138">Använda en åtgärd</span><span class="sxs-lookup"><span data-stu-id="17761-138">Use an action</span></span>
<span data-ttu-id="17761-139">En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="17761-139">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="17761-140">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="17761-140">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="17761-141">Välj hello plustecken.</span><span class="sxs-lookup"><span data-stu-id="17761-141">Select hello plus sign.</span></span> <span data-ttu-id="17761-142">Du ser flera alternativ: **lägga till en åtgärd**, **Lägg till ett villkor**, eller en av hello **mer** alternativ.</span><span class="sxs-lookup"><span data-stu-id="17761-142">You see several choices: **Add an action**, **Add a condition**, or one of hello **More** options.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. <span data-ttu-id="17761-143">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="17761-143">Choose **Add an action**.</span></span>
3. <span data-ttu-id="17761-144">Skriv ”office 365” tooget en lista över alla tillgängliga åtgärder för hello hello i textrutan.</span><span class="sxs-lookup"><span data-stu-id="17761-144">In hello text box, type “office 365” tooget a list of all hello available actions.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. <span data-ttu-id="17761-145">I vårt exempel väljer **Office 365 Outlook - skapa kontakt**.</span><span class="sxs-lookup"><span data-stu-id="17761-145">In our example, choose **Office 365 Outlook - Create contact**.</span></span> <span data-ttu-id="17761-146">Om det finns redan en anslutning, och välj sedan hello **mappen ID**, **Förnamn**, och andra egenskaper:</span><span class="sxs-lookup"><span data-stu-id="17761-146">If a connection already exists, then choose hello **Folder ID**, **Given Name**, and other properties:</span></span>  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    <span data-ttu-id="17761-147">Om du uppmanas att ange anslutningsinformation för hello ange hello information toocreate hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="17761-147">If you are prompted for hello connection information, then enter hello details toocreate hello connection.</span></span> <span data-ttu-id="17761-148">[Skapa hello anslutning](connectors-create-api-office365-outlook.md#create-the-connection) i det här avsnittet beskriver dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="17761-148">[Create hello connection](connectors-create-api-office365-outlook.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="17761-149">I det här exemplet skapar vi en ny kontakt i Office 365 Outlook.</span><span class="sxs-lookup"><span data-stu-id="17761-149">In this example, we create a new contact in Office 365 Outlook.</span></span> <span data-ttu-id="17761-150">Du kan använda utdata från en annan utlösare toocreate hello kontakt.</span><span class="sxs-lookup"><span data-stu-id="17761-150">You can use output from another trigger toocreate hello contact.</span></span> <span data-ttu-id="17761-151">Lägg exempelvis till hello SalesForce *när ett objekt skapas* utlösare.</span><span class="sxs-lookup"><span data-stu-id="17761-151">For example, add hello SalesForce *When an object is created* trigger.</span></span> <span data-ttu-id="17761-152">Lägg sedan till hello Office 365 Outlook *skapa kontakt* åtgärd som använder hello SalesForce fält toocreate hello ny nya kontakt i Office 365.</span><span class="sxs-lookup"><span data-stu-id="17761-152">Then add hello Office 365 Outlook *Create contact* action that uses hello SalesForce fields toocreate hello new new contact in Office 365.</span></span> 
   > 
   > 
5. <span data-ttu-id="17761-153">**Spara** dina ändringar (övre vänstra hörnet av hello verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="17761-153">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="17761-154">Din logikapp sparas och aktiveras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="17761-154">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="17761-155">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="17761-155">Connector-specific details</span></span>

<span data-ttu-id="17761-156">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/office365connector/).</span><span class="sxs-lookup"><span data-stu-id="17761-156">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/office365connector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="17761-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="17761-157">Next Steps</span></span>
<span data-ttu-id="17761-158">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="17761-158">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="17761-159">Utforska hello andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="17761-159">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

