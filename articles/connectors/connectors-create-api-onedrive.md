---
title: aaaAdd hello OneDrive connector i dina Logic Apps | Microsoft Docs
description: "Översikt över hello OneDrive koppling med REST API-parametrar"
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
ms.openlocfilehash: 8303794bb3c2844de288f87f40639abb84c160fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-onedrive-connector"></a><span data-ttu-id="c9fdd-103">Kom igång med hello OneDrive-koppling</span><span class="sxs-lookup"><span data-stu-id="c9fdd-103">Get started with hello OneDrive connector</span></span>
<span data-ttu-id="c9fdd-104">Ansluta tooOneDrive toomanage filerna, inklusive överföring, get, ta bort filer och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-104">Connect tooOneDrive toomanage your files, including upload, get, delete files, and more.</span></span> 

<span data-ttu-id="c9fdd-105">Med OneDrive kan du:</span><span class="sxs-lookup"><span data-stu-id="c9fdd-105">With OneDrive, you:</span></span> 

* <span data-ttu-id="c9fdd-106">Skapa ditt arbetsflöde genom att lagra filer i OneDrive eller uppdatera befintliga filer i OneDrive.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-106">Build your workflow by storing files in OneDrive, or update existing files in OneDrive.</span></span> 
* <span data-ttu-id="c9fdd-107">Använd utlösare toostart arbetsflödet när en fil skapas eller uppdateras i OneDrive.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-107">Use triggers toostart your workflow when a file is created or updated within your OneDrive.</span></span>
* <span data-ttu-id="c9fdd-108">Använda åtgärder toocreate en fil, ta bort en fil med mera.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-108">Use actions toocreate a file, delete a file, and more.</span></span> <span data-ttu-id="c9fdd-109">Till exempel när en ny Office 365 e-post tas emot med en bifogad fil (en utlösare), skapa en ny fil i OneDrive (en åtgärd).</span><span class="sxs-lookup"><span data-stu-id="c9fdd-109">For example, when a new Office 365 email is received with an attachment (a trigger), create a new file in OneDrive (an action).</span></span>

<span data-ttu-id="c9fdd-110">Det här avsnittet visar hur toouse hello OneDrive kopplingen i en logikapp och även visar hello utlösare och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-110">This topic shows you how toouse hello OneDrive connector in a logic app, and also lists hello triggers and actions.</span></span>

<span data-ttu-id="c9fdd-111">toolearn mer om Logic Apps finns [vad är logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c9fdd-111">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooonedrive"></a><span data-ttu-id="c9fdd-112">Ansluta tooOneDrive</span><span class="sxs-lookup"><span data-stu-id="c9fdd-112">Connect tooOneDrive</span></span>
<span data-ttu-id="c9fdd-113">Innan din logikapp får åtkomst till alla tjänster måste du först skapa en *anslutning* toohello service.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-113">Before your logic app can access any service, you first create a *connection* toohello service.</span></span> <span data-ttu-id="c9fdd-114">En anslutning kan du ansluta en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-114">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="c9fdd-115">Till exempel tooconnect tooOneDrive, måste du först en OneDrive *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-115">For example, tooconnect tooOneDrive, you first need a OneDrive *connection*.</span></span> <span data-ttu-id="c9fdd-116">toocreate en anslutning, ange hello autentiseringsuppgifter som du vanligtvis använder tooaccess Hej tjänst du vill tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-116">toocreate a connection, enter hello credentials you normally use tooaccess hello service you wish tooconnect to.</span></span> <span data-ttu-id="c9fdd-117">Så, OneDrive, ange hello autentiseringsuppgifter tooyour OneDrive-konto toocreate hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-117">So, with OneDrive, enter hello credentials tooyour OneDrive account  toocreate hello connection.</span></span>

### <a name="create-hello-connection"></a><span data-ttu-id="c9fdd-118">Skapa hello-anslutning</span><span class="sxs-lookup"><span data-stu-id="c9fdd-118">Create hello connection</span></span>
> [!INCLUDE [Steps toocreate a connection tooOneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="c9fdd-119">Använda en utlösare</span><span class="sxs-lookup"><span data-stu-id="c9fdd-119">Use a trigger</span></span>
<span data-ttu-id="c9fdd-120">En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-120">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="c9fdd-121">Utlösare avsöker ”” hello-tjänsten på ett intervall och frekvens som du vill.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-121">Triggers "poll" hello service at an interval and frequency that you want.</span></span> <span data-ttu-id="c9fdd-122">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="c9fdd-122">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="c9fdd-123">Skriv ”onedrive” tooget en lista över hello utlösare i hello logikapp:</span><span class="sxs-lookup"><span data-stu-id="c9fdd-123">In hello logic app, type "onedrive" tooget a list of hello triggers:</span></span>  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. <span data-ttu-id="c9fdd-124">Välj **när en fil ändras**.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-124">Select **When a file is modified**.</span></span> <span data-ttu-id="c9fdd-125">Om det finns redan en anslutning, väljer du hello visa Väljaren knappen tooselect en mapp.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-125">If a connection already exists, then select hello Show Picker button tooselect a folder.</span></span>
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    <span data-ttu-id="c9fdd-126">Om du tillfrågas toosign i kan sedan ange hello inloggningsnamnet information toocreate hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-126">If you are prompted toosign in, then enter hello sign in details toocreate hello connection.</span></span> <span data-ttu-id="c9fdd-127">[Skapa hello anslutning](connectors-create-api-onedrive.md#create-the-connection) i det här avsnittet listar hello steg.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-127">[Create hello connection](connectors-create-api-onedrive.md#create-the-connection) in this topic lists hello steps.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="c9fdd-128">I det här exemplet hello logikapp körs när en fil i hello-mapp som du väljer uppdateras.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-128">In this example, hello logic app runs when a file in hello folder you choose is updated.</span></span> <span data-ttu-id="c9fdd-129">toosee hello resultaten av den här utlösaren Lägg till en annan åtgärd som skickar ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-129">toosee hello results of this trigger, add another action that sends you an email.</span></span> <span data-ttu-id="c9fdd-130">Lägg exempelvis till hello Office 365 Outlook *skickar ett e-* åtgärd som e-postmeddelanden när en fil har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-130">For example, add hello Office 365 Outlook *Send an email* action that emails you when a file is updated.</span></span> 

3. <span data-ttu-id="c9fdd-131">Välj hello **redigera** knappen och ange hello **frekvens** och **intervall** värden.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-131">Select hello **Edit** button and set hello **Frequency** and **Interval** values.</span></span> <span data-ttu-id="c9fdd-132">Till exempel om du vill hello utlösaren toopoll var 15: e minut, ange hello **frekvens** för**minut**, och ange hello **intervall** för**15**.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-132">For example, if you want hello trigger toopoll every 15 minutes, then set hello **Frequency** too**Minute**, and set hello **Interval** too**15**.</span></span> 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. <span data-ttu-id="c9fdd-133">**Spara** dina ändringar (övre vänstra hörnet av hello verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="c9fdd-133">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="c9fdd-134">Din logikapp sparas och aktiveras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-134">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="c9fdd-135">Använda en åtgärd</span><span class="sxs-lookup"><span data-stu-id="c9fdd-135">Use an action</span></span>
<span data-ttu-id="c9fdd-136">En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-136">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="c9fdd-137">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="c9fdd-137">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="c9fdd-138">Välj hello plustecken.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-138">Select hello plus sign.</span></span> <span data-ttu-id="c9fdd-139">Du ser flera alternativ: **lägga till en åtgärd**, **Lägg till ett villkor**, eller en av hello **mer** alternativ.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-139">You see several choices: **Add an action**, **Add a condition**, or one of hello **More** options.</span></span>
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. <span data-ttu-id="c9fdd-140">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-140">Choose **Add an action**.</span></span>
3. <span data-ttu-id="c9fdd-141">Skriv ”onedrive” tooget en lista över alla tillgängliga åtgärder för hello hello i textrutan.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-141">In hello text box, type “onedrive” tooget a list of all hello available actions.</span></span>
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. <span data-ttu-id="c9fdd-142">I vårt exempel väljer **OneDrive – skapa filen**.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-142">In our example, choose **OneDrive - Create file**.</span></span> <span data-ttu-id="c9fdd-143">Om det finns redan en anslutning, och välj sedan hello **mappsökväg** tooput hello fil, ange hello **filnamn**, och välj hello **filinnehåll** du vill:</span><span class="sxs-lookup"><span data-stu-id="c9fdd-143">If a connection already exists, then select hello **Folder Path** tooput hello file, enter hello **File Name**, and choose hello **File Content** you want:</span></span>  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    <span data-ttu-id="c9fdd-144">Om du uppmanas att ange anslutningsinformation för hello ange hello information toocreate hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-144">If you are prompted for hello connection information, then enter hello details toocreate hello connection.</span></span> <span data-ttu-id="c9fdd-145">[Skapa hello anslutning](connectors-create-api-onedrive.md#create-the-connection) i det här avsnittet beskriver dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-145">[Create hello connection](connectors-create-api-onedrive.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="c9fdd-146">I det här exemplet skapar vi en ny fil i en mapp i OneDrive.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-146">In this example, we create a new file in a OneDrive folder.</span></span> <span data-ttu-id="c9fdd-147">Du kan använda utdata från en annan utlösare toocreate hello OneDrive fil.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-147">You can use output from another trigger toocreate hello OneDrive file.</span></span> <span data-ttu-id="c9fdd-148">Lägg exempelvis till hello Office 365 Outlook *när en ny e-postmeddelandet* utlösare.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-148">For example, add hello Office 365 Outlook *When a new email arrives* trigger.</span></span> <span data-ttu-id="c9fdd-149">Lägg sedan till hello OneDrive *skapa fil* åtgärd som använder hello bilagor och Content-Type-fält i en ForEach toocreate hello ny fil i OneDrive.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-149">Then add hello OneDrive *Create file* action that uses hello Attachments and Content-Type fields within a ForEach toocreate hello new file in OneDrive.</span></span> 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. <span data-ttu-id="c9fdd-150">**Spara** dina ändringar (övre vänstra hörnet av hello verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="c9fdd-150">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="c9fdd-151">Din logikapp sparas och aktiveras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c9fdd-151">Your logic app is saved and may be automatically enabled.</span></span>


## <a name="connector-specific-details"></a><span data-ttu-id="c9fdd-152">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="c9fdd-152">Connector-specific details</span></span>

<span data-ttu-id="c9fdd-153">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/onedriveconnector/).</span><span class="sxs-lookup"><span data-stu-id="c9fdd-153">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/onedriveconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="c9fdd-154">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="c9fdd-154">More connectors</span></span>
<span data-ttu-id="c9fdd-155">Gå tillbaka toohello [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="c9fdd-155">Go back toohello [APIs list](apis-list.md).</span></span>
