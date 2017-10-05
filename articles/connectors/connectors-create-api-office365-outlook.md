---
title: "Lägg till Office 365 Outlook connector i dina Logic Apps | Microsoft Docs"
description: "Skapa logikappar med Office 365-kopplingen för att aktivera samverkan med Office 365. Exempel: skapa, redigera och uppdatera kontakter och Kalender-objekt."
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
ms.openlocfilehash: 5335dae62e61659b68e8befb4ed0d404dffb800c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-office-365-outlook-connector"></a><span data-ttu-id="2a44d-104">Kom igång med Office 365 Outlook connector</span><span class="sxs-lookup"><span data-stu-id="2a44d-104">Get started with the Office 365 Outlook connector</span></span>
<span data-ttu-id="2a44d-105">Office 365 Outlook connector kan interaktion med Outlook i Office 365.</span><span class="sxs-lookup"><span data-stu-id="2a44d-105">The Office 365 Outlook connector enables interaction with Outlook in Office 365.</span></span> <span data-ttu-id="2a44d-106">Använd den här anslutningen för att skapa, redigera och uppdatera kontakter och kalenderobjekt, och också få, skicka och svara på e-post.</span><span class="sxs-lookup"><span data-stu-id="2a44d-106">Use this connector to create, edit, and update contacts and calendar items, and also get, send, and reply to email.</span></span>

<span data-ttu-id="2a44d-107">Med Office 365 Outlook kan du:</span><span class="sxs-lookup"><span data-stu-id="2a44d-107">With Office 365 Outlook, you:</span></span>

* <span data-ttu-id="2a44d-108">Skapa arbetsflödet med hjälp av funktionerna i Office 365 e-post och kalender.</span><span class="sxs-lookup"><span data-stu-id="2a44d-108">Build your workflow using the email and calendar features within Office 365.</span></span> 
* <span data-ttu-id="2a44d-109">Använd utlösare för att starta arbetsflödet när det finns en ny e-post, när ett kalenderobjekt uppdateras med mera.</span><span class="sxs-lookup"><span data-stu-id="2a44d-109">Use triggers to start your workflow when there is a new email, when a calendar item is updated, and more.</span></span>
* <span data-ttu-id="2a44d-110">Använd åtgärder för att skicka ett e-postmeddelande, skapa en ny händelse och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="2a44d-110">Use actions to send an email, create a new calendar event, and more.</span></span> <span data-ttu-id="2a44d-111">Till exempel när det finns ett nytt objekt i Salesforce (en utlösare), skicka ett e-postmeddelande till din Office 365 Outlook (en åtgärd).</span><span class="sxs-lookup"><span data-stu-id="2a44d-111">For example, when there is a new object in Salesforce (a trigger), send an email to your Office 365 Outlook (an action).</span></span> 

<span data-ttu-id="2a44d-112">Det här avsnittet beskrivs hur du använder Office 365 Outlook-anslutningen i en logikapp och visar också utlösare och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2a44d-112">This topic shows you how to use the Office 365 Outlook connector in a logic app, and also lists the triggers and actions.</span></span>

> [!NOTE]
> <span data-ttu-id="2a44d-113">Den här versionen av artikeln gäller för Logic Apps allmän tillgänglighet (GA).</span><span class="sxs-lookup"><span data-stu-id="2a44d-113">This version of the article applies to Logic Apps general availability (GA).</span></span>
> 
> 

<span data-ttu-id="2a44d-114">Läs mer om Logic Apps i [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="2a44d-114">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-office-365"></a><span data-ttu-id="2a44d-115">Ansluta till Office 365</span><span class="sxs-lookup"><span data-stu-id="2a44d-115">Connect to Office 365</span></span>
<span data-ttu-id="2a44d-116">Innan din logikapp får åtkomst till alla tjänster måste du först skapa en *anslutning* till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2a44d-116">Before your logic app can access any service, you first create a *connection* to the service.</span></span> <span data-ttu-id="2a44d-117">En anslutning kan du ansluta en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="2a44d-117">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="2a44d-118">Till exempel för att ansluta till Office 365 Outlook, måste du först en Office 365 *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="2a44d-118">For example, to connect to Office 365 Outlook, you first need an Office 365 *connection*.</span></span> <span data-ttu-id="2a44d-119">Ange de autentiseringsuppgifter som du vanligtvis använder för att få åtkomst till tjänsten som du vill ansluta till om du vill skapa en anslutning.</span><span class="sxs-lookup"><span data-stu-id="2a44d-119">To create a connection, enter the credentials you normally use to access the service you wish to connect to.</span></span> <span data-ttu-id="2a44d-120">Så med Office 365 Outlook ange autentiseringsuppgifterna för ditt Office 365-konto för att skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="2a44d-120">So with Office 365 Outlook, enter the credentials to your Office 365 account to create the connection.</span></span>

## <a name="create-the-connection"></a><span data-ttu-id="2a44d-121">Skapa anslutningen</span><span class="sxs-lookup"><span data-stu-id="2a44d-121">Create the connection</span></span>
> [!INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="2a44d-122">Använda en utlösare</span><span class="sxs-lookup"><span data-stu-id="2a44d-122">Use a trigger</span></span>
<span data-ttu-id="2a44d-123">En utlösare är en händelse som kan användas för att starta arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="2a44d-123">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="2a44d-124">Utlösare avsöker ”” tjänsten på ett intervall och frekvens som du vill.</span><span class="sxs-lookup"><span data-stu-id="2a44d-124">Triggers "poll" the service at an interval and frequency that you want.</span></span> <span data-ttu-id="2a44d-125">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="2a44d-125">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="2a44d-126">I logikappen, skriver du ”office 365” om du vill hämta en lista över utlösare:</span><span class="sxs-lookup"><span data-stu-id="2a44d-126">In the logic app, type "office 365" to get a list of the triggers:</span></span>  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. <span data-ttu-id="2a44d-127">Välj **Office 365 Outlook - när en kommande händelse startas snart**.</span><span class="sxs-lookup"><span data-stu-id="2a44d-127">Select **Office 365 Outlook - When an upcoming event is starting soon**.</span></span> <span data-ttu-id="2a44d-128">Om det finns redan en anslutning, väljer du en kalender från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="2a44d-128">If a connection already exists, then select a calendar from the drop-down list.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    <span data-ttu-id="2a44d-129">Om du uppmanas att logga in, anger du tecknet i informationen för att skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="2a44d-129">If you are prompted to sign in, then enter the sign in details to create the connection.</span></span> <span data-ttu-id="2a44d-130">[Skapa anslutningen](connectors-create-api-office365-outlook.md#create-the-connection) innehåller stegen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2a44d-130">[Create the connection](connectors-create-api-office365-outlook.md#create-the-connection) in this topic lists the steps.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="2a44d-131">I det här exemplet körs logikappen när en händelse har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="2a44d-131">In this example, the logic app runs when a calendar event is updated.</span></span> <span data-ttu-id="2a44d-132">Lägg till en annan åtgärd som skickar ett SMS till dig om du vill se resultatet av den här utlösaren.</span><span class="sxs-lookup"><span data-stu-id="2a44d-132">To see the results of this trigger, add another action that sends you a text message.</span></span> <span data-ttu-id="2a44d-133">Till exempel lägga till Twilio *skickar ett meddelande* åtgärd som texter när händelse startar i 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="2a44d-133">For example, add the Twilio *Send message* action that texts you when the calendar event is starting in 15 minutes.</span></span> 
   > 
   > 
3. <span data-ttu-id="2a44d-134">Välj den **redigera** knappen och ange den **frekvens** och **intervall** värden.</span><span class="sxs-lookup"><span data-stu-id="2a44d-134">Select the **Edit** button and set the **Frequency** and **Interval** values.</span></span> <span data-ttu-id="2a44d-135">Till exempel om du vill att utlösaren ska avsöka var 15: e minut, anger du den **frekvens** till **minut**, och ange den **intervall** till **15**.</span><span class="sxs-lookup"><span data-stu-id="2a44d-135">For example, if you want the trigger to poll every 15 minutes, then set the **Frequency** to **Minute**, and set the **Interval** to **15**.</span></span> 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. <span data-ttu-id="2a44d-136">**Spara** dina ändringar (övre vänstra hörnet i verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="2a44d-136">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="2a44d-137">Din logikapp sparas och aktiveras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2a44d-137">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="2a44d-138">Använda en åtgärd</span><span class="sxs-lookup"><span data-stu-id="2a44d-138">Use an action</span></span>
<span data-ttu-id="2a44d-139">En åtgärd är en åtgärd som utförs av arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="2a44d-139">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="2a44d-140">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="2a44d-140">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="2a44d-141">Klicka på plustecknet.</span><span class="sxs-lookup"><span data-stu-id="2a44d-141">Select the plus sign.</span></span> <span data-ttu-id="2a44d-142">Du ser flera alternativ: **lägga till en åtgärd**, **Lägg till ett villkor**, eller en av de **mer** alternativ.</span><span class="sxs-lookup"><span data-stu-id="2a44d-142">You see several choices: **Add an action**, **Add a condition**, or one of the **More** options.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. <span data-ttu-id="2a44d-143">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="2a44d-143">Choose **Add an action**.</span></span>
3. <span data-ttu-id="2a44d-144">Skriv ”office 365” om du vill hämta en lista över alla tillgängliga åtgärder i textrutan.</span><span class="sxs-lookup"><span data-stu-id="2a44d-144">In the text box, type “office 365” to get a list of all the available actions.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. <span data-ttu-id="2a44d-145">I vårt exempel väljer **Office 365 Outlook - skapa kontakt**.</span><span class="sxs-lookup"><span data-stu-id="2a44d-145">In our example, choose **Office 365 Outlook - Create contact**.</span></span> <span data-ttu-id="2a44d-146">Om det finns redan en anslutning, väljer du den **mappen ID**, **Förnamn**, och andra egenskaper:</span><span class="sxs-lookup"><span data-stu-id="2a44d-146">If a connection already exists, then choose the **Folder ID**, **Given Name**, and other properties:</span></span>  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    <span data-ttu-id="2a44d-147">Om du uppmanas att ange anslutningsinformation anger du information för att skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="2a44d-147">If you are prompted for the connection information, then enter the details to create the connection.</span></span> <span data-ttu-id="2a44d-148">[Skapa anslutningen](connectors-create-api-office365-outlook.md#create-the-connection) i det här avsnittet beskriver dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="2a44d-148">[Create the connection](connectors-create-api-office365-outlook.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="2a44d-149">I det här exemplet skapar vi en ny kontakt i Office 365 Outlook.</span><span class="sxs-lookup"><span data-stu-id="2a44d-149">In this example, we create a new contact in Office 365 Outlook.</span></span> <span data-ttu-id="2a44d-150">Du kan använda utdata från en annan utlösare för att skapa kontakten.</span><span class="sxs-lookup"><span data-stu-id="2a44d-150">You can use output from another trigger to create the contact.</span></span> <span data-ttu-id="2a44d-151">Lägg exempelvis till SalesForce *när ett objekt skapas* utlösare.</span><span class="sxs-lookup"><span data-stu-id="2a44d-151">For example, add the SalesForce *When an object is created* trigger.</span></span> <span data-ttu-id="2a44d-152">Lägg sedan till Office 365 Outlook *skapa kontakt* åtgärd som SalesForce-fälten används för att skapa ny ny kontakt i Office 365.</span><span class="sxs-lookup"><span data-stu-id="2a44d-152">Then add the Office 365 Outlook *Create contact* action that uses the SalesForce fields to create the new new contact in Office 365.</span></span> 
   > 
   > 
5. <span data-ttu-id="2a44d-153">**Spara** dina ändringar (övre vänstra hörnet i verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="2a44d-153">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="2a44d-154">Din logikapp sparas och aktiveras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2a44d-154">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="2a44d-155">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="2a44d-155">Connector-specific details</span></span>

<span data-ttu-id="2a44d-156">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/office365connector/).</span><span class="sxs-lookup"><span data-stu-id="2a44d-156">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/office365connector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2a44d-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2a44d-157">Next Steps</span></span>
<span data-ttu-id="2a44d-158">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="2a44d-158">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="2a44d-159">Utforska andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="2a44d-159">Explore the other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

