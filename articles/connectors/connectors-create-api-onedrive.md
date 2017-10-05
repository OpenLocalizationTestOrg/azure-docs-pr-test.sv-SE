---
title: "Lägg till OneDrive-koppling i dina Logic Apps | Microsoft Docs"
description: "Översikt över OneDrive anslutningen med REST API-parametrar"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 47a8582a-1b1a-4fc3-beb5-97c60c4306fe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 63bd33bf4e09b98aa53dcfec9fcc4a0109204952
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-onedrive-connector"></a><span data-ttu-id="31bb2-103">Kom igång med OneDrive-koppling</span><span class="sxs-lookup"><span data-stu-id="31bb2-103">Get started with the OneDrive connector</span></span>
<span data-ttu-id="31bb2-104">Ansluta till OneDrive för att hantera dina filer, inklusive överföring, hämta, ta bort filer och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="31bb2-104">Connect to OneDrive to manage your files, including upload, get, delete files, and more.</span></span> 

<span data-ttu-id="31bb2-105">Med OneDrive kan du:</span><span class="sxs-lookup"><span data-stu-id="31bb2-105">With OneDrive, you:</span></span> 

* <span data-ttu-id="31bb2-106">Skapa ditt arbetsflöde genom att lagra filer i OneDrive eller uppdatera befintliga filer i OneDrive.</span><span class="sxs-lookup"><span data-stu-id="31bb2-106">Build your workflow by storing files in OneDrive, or update existing files in OneDrive.</span></span> 
* <span data-ttu-id="31bb2-107">Använd utlösare för att starta arbetsflödet när en fil skapas eller uppdateras i OneDrive.</span><span class="sxs-lookup"><span data-stu-id="31bb2-107">Use triggers to start your workflow when a file is created or updated within your OneDrive.</span></span>
* <span data-ttu-id="31bb2-108">Använd åtgärder för att skapa en fil, ta bort en fil med mera.</span><span class="sxs-lookup"><span data-stu-id="31bb2-108">Use actions to create a file, delete a file, and more.</span></span> <span data-ttu-id="31bb2-109">Till exempel när en ny Office 365 e-post tas emot med en bifogad fil (en utlösare), skapa en ny fil i OneDrive (en åtgärd).</span><span class="sxs-lookup"><span data-stu-id="31bb2-109">For example, when a new Office 365 email is received with an attachment (a trigger), create a new file in OneDrive (an action).</span></span>

<span data-ttu-id="31bb2-110">Det här avsnittet beskrivs hur du använder OneDrive-anslutningen i en logikapp och visar också utlösare och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="31bb2-110">This topic shows you how to use the OneDrive connector in a logic app, and also lists the triggers and actions.</span></span>

<span data-ttu-id="31bb2-111">Läs mer om Logic Apps i [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="31bb2-111">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-onedrive"></a><span data-ttu-id="31bb2-112">Ansluta till OneDrive</span><span class="sxs-lookup"><span data-stu-id="31bb2-112">Connect to OneDrive</span></span>
<span data-ttu-id="31bb2-113">Innan din logikapp får åtkomst till alla tjänster måste du först skapa en *anslutning* till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="31bb2-113">Before your logic app can access any service, you first create a *connection* to the service.</span></span> <span data-ttu-id="31bb2-114">En anslutning kan du ansluta en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="31bb2-114">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="31bb2-115">Till exempel för att ansluta till OneDrive, måste du först en OneDrive *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="31bb2-115">For example, to connect to OneDrive, you first need a OneDrive *connection*.</span></span> <span data-ttu-id="31bb2-116">Ange de autentiseringsuppgifter som du vanligtvis använder för att få åtkomst till tjänsten som du vill ansluta till om du vill skapa en anslutning.</span><span class="sxs-lookup"><span data-stu-id="31bb2-116">To create a connection, enter the credentials you normally use to access the service you wish to connect to.</span></span> <span data-ttu-id="31bb2-117">Så, OneDrive, ange autentiseringsuppgifterna till OneDrive-konto för att skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="31bb2-117">So, with OneDrive, enter the credentials to your OneDrive account  to create the connection.</span></span>

### <a name="create-the-connection"></a><span data-ttu-id="31bb2-118">Skapa anslutningen</span><span class="sxs-lookup"><span data-stu-id="31bb2-118">Create the connection</span></span>
> [!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="31bb2-119">Använda en utlösare</span><span class="sxs-lookup"><span data-stu-id="31bb2-119">Use a trigger</span></span>
<span data-ttu-id="31bb2-120">En utlösare är en händelse som kan användas för att starta arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="31bb2-120">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="31bb2-121">Utlösare avsöker ”” tjänsten på ett intervall och frekvens som du vill.</span><span class="sxs-lookup"><span data-stu-id="31bb2-121">Triggers "poll" the service at an interval and frequency that you want.</span></span> <span data-ttu-id="31bb2-122">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="31bb2-122">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="31bb2-123">Skriv ”onedrive” i logikappen, om du vill hämta en lista över utlösare:</span><span class="sxs-lookup"><span data-stu-id="31bb2-123">In the logic app, type "onedrive" to get a list of the triggers:</span></span>  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. <span data-ttu-id="31bb2-124">Välj **när en fil ändras**.</span><span class="sxs-lookup"><span data-stu-id="31bb2-124">Select **When a file is modified**.</span></span> <span data-ttu-id="31bb2-125">Om det finns redan en anslutning, väljer du knappen Visa objektväljaren och väljer en mapp.</span><span class="sxs-lookup"><span data-stu-id="31bb2-125">If a connection already exists, then select the Show Picker button to select a folder.</span></span>
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    <span data-ttu-id="31bb2-126">Om du uppmanas att logga in, anger du tecknet i informationen för att skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="31bb2-126">If you are prompted to sign in, then enter the sign in details to create the connection.</span></span> <span data-ttu-id="31bb2-127">[Skapa anslutningen](connectors-create-api-onedrive.md#create-the-connection) innehåller stegen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="31bb2-127">[Create the connection](connectors-create-api-onedrive.md#create-the-connection) in this topic lists the steps.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="31bb2-128">I det här exemplet körs logikappen när en fil i mappen som du väljer uppdateras.</span><span class="sxs-lookup"><span data-stu-id="31bb2-128">In this example, the logic app runs when a file in the folder you choose is updated.</span></span> <span data-ttu-id="31bb2-129">Lägg till en annan åtgärd som skickar ett e-postmeddelande om du vill se resultatet av den här utlösaren.</span><span class="sxs-lookup"><span data-stu-id="31bb2-129">To see the results of this trigger, add another action that sends you an email.</span></span> <span data-ttu-id="31bb2-130">Till exempel lägga till Office 365 Outlook *skickar ett e-* åtgärd som e-postmeddelanden när en fil har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="31bb2-130">For example, add the Office 365 Outlook *Send an email* action that emails you when a file is updated.</span></span> 

3. <span data-ttu-id="31bb2-131">Välj den **redigera** knappen och ange den **frekvens** och **intervall** värden.</span><span class="sxs-lookup"><span data-stu-id="31bb2-131">Select the **Edit** button and set the **Frequency** and **Interval** values.</span></span> <span data-ttu-id="31bb2-132">Till exempel om du vill att utlösaren ska avsöka var 15: e minut, anger du den **frekvens** till **minut**, och ange den **intervall** till **15**.</span><span class="sxs-lookup"><span data-stu-id="31bb2-132">For example, if you want the trigger to poll every 15 minutes, then set the **Frequency** to **Minute**, and set the **Interval** to **15**.</span></span> 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. <span data-ttu-id="31bb2-133">**Spara** dina ändringar (övre vänstra hörnet i verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="31bb2-133">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="31bb2-134">Din logikapp sparas och aktiveras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="31bb2-134">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="31bb2-135">Använda en åtgärd</span><span class="sxs-lookup"><span data-stu-id="31bb2-135">Use an action</span></span>
<span data-ttu-id="31bb2-136">En åtgärd är en åtgärd som utförs av arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="31bb2-136">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="31bb2-137">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="31bb2-137">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="31bb2-138">Klicka på plustecknet.</span><span class="sxs-lookup"><span data-stu-id="31bb2-138">Select the plus sign.</span></span> <span data-ttu-id="31bb2-139">Du ser flera alternativ: **lägga till en åtgärd**, **Lägg till ett villkor**, eller en av de **mer** alternativ.</span><span class="sxs-lookup"><span data-stu-id="31bb2-139">You see several choices: **Add an action**, **Add a condition**, or one of the **More** options.</span></span>
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. <span data-ttu-id="31bb2-140">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="31bb2-140">Choose **Add an action**.</span></span>
3. <span data-ttu-id="31bb2-141">I rutan skriver du ”onedrive” om du vill hämta en lista över alla tillgängliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="31bb2-141">In the text box, type “onedrive” to get a list of all the available actions.</span></span>
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. <span data-ttu-id="31bb2-142">I vårt exempel väljer **OneDrive – skapa filen**.</span><span class="sxs-lookup"><span data-stu-id="31bb2-142">In our example, choose **OneDrive - Create file**.</span></span> <span data-ttu-id="31bb2-143">Om det finns redan en anslutning, väljer du den **mappsökväg** för att placera filen, ange den **filnamn**, och välj den **filinnehåll** du vill:</span><span class="sxs-lookup"><span data-stu-id="31bb2-143">If a connection already exists, then select the **Folder Path** to put the file, enter the **File Name**, and choose the **File Content** you want:</span></span>  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    <span data-ttu-id="31bb2-144">Om du uppmanas att ange anslutningsinformation anger du information för att skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="31bb2-144">If you are prompted for the connection information, then enter the details to create the connection.</span></span> <span data-ttu-id="31bb2-145">[Skapa anslutningen](connectors-create-api-onedrive.md#create-the-connection) i det här avsnittet beskriver dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="31bb2-145">[Create the connection](connectors-create-api-onedrive.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="31bb2-146">I det här exemplet skapar vi en ny fil i en mapp i OneDrive.</span><span class="sxs-lookup"><span data-stu-id="31bb2-146">In this example, we create a new file in a OneDrive folder.</span></span> <span data-ttu-id="31bb2-147">Du kan använda utdata från en annan utlösare för att skapa filen OneDrive.</span><span class="sxs-lookup"><span data-stu-id="31bb2-147">You can use output from another trigger to create the OneDrive file.</span></span> <span data-ttu-id="31bb2-148">Till exempel lägga till Office 365 Outlook *när en ny e-postmeddelandet* utlösare.</span><span class="sxs-lookup"><span data-stu-id="31bb2-148">For example, add the Office 365 Outlook *When a new email arrives* trigger.</span></span> <span data-ttu-id="31bb2-149">Lägg sedan till OneDrive *skapa fil* åtgärd som använder bilagor och Content-Type fält inom en ForEach att skapa den nya filen i OneDrive.</span><span class="sxs-lookup"><span data-stu-id="31bb2-149">Then add the OneDrive *Create file* action that uses the Attachments and Content-Type fields within a ForEach to create the new file in OneDrive.</span></span> 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. <span data-ttu-id="31bb2-150">**Spara** dina ändringar (övre vänstra hörnet i verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="31bb2-150">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="31bb2-151">Din logikapp sparas och aktiveras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="31bb2-151">Your logic app is saved and may be automatically enabled.</span></span>


## <a name="connector-specific-details"></a><span data-ttu-id="31bb2-152">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="31bb2-152">Connector-specific details</span></span>

<span data-ttu-id="31bb2-153">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/onedriveconnector/).</span><span class="sxs-lookup"><span data-stu-id="31bb2-153">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/onedriveconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="31bb2-154">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="31bb2-154">More connectors</span></span>
<span data-ttu-id="31bb2-155">Gå tillbaka till den [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="31bb2-155">Go back to the [APIs list](apis-list.md).</span></span>