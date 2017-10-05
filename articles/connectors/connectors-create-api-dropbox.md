---
title: Dropbox-anslutningen i Azure Logic Apps | Microsoft Docs
description: "Skapa logikappar med Azure App service. Ansluta till Dropbox att hantera dina filer. Du kan utföra olika åtgärder, till exempel ladda upp, uppdatera, hämta och ta bort filer i Dropbox."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: cb0ae033-aba7-4ac9-beaa-be561a0f0cac
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 0d09580c60fd620811b539147439d0922839fe7e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-dropbox-connector"></a><span data-ttu-id="9c070-105">Kom igång med Dropbox-koppling</span><span class="sxs-lookup"><span data-stu-id="9c070-105">Get started with the Dropbox connector</span></span>
<span data-ttu-id="9c070-106">Ansluta till Dropbox att hantera dina filer.</span><span class="sxs-lookup"><span data-stu-id="9c070-106">Connect to Dropbox to manage your files.</span></span> <span data-ttu-id="9c070-107">Du kan utföra olika åtgärder, till exempel ladda upp, uppdatera, hämta och ta bort filer i Dropbox.</span><span class="sxs-lookup"><span data-stu-id="9c070-107">You can perform various actions such as upload, update, get, and delete files in Dropbox.</span></span>

<span data-ttu-id="9c070-108">Att använda [alla anslutningar](apis-list.md), måste du först skapa en logikapp.</span><span class="sxs-lookup"><span data-stu-id="9c070-108">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="9c070-109">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9c070-109">You can get started by [creating a Logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-dropbox"></a><span data-ttu-id="9c070-110">Ansluta till Dropbox</span><span class="sxs-lookup"><span data-stu-id="9c070-110">Connect to Dropbox</span></span>
<span data-ttu-id="9c070-111">Innan din logikapp kan komma åt någon tjänst, måste du först skapa en *anslutning* till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c070-111">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="9c070-112">En anslutning kan du ansluta en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="9c070-112">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="9c070-113">Till exempel för att ansluta till Dropbox, måste du först en Dropbox *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="9c070-113">For example, in order to connect to Dropbox, you first need a Dropbox *connection*.</span></span> <span data-ttu-id="9c070-114">Om du vill skapa en anslutning skulle du behöva ange autentiseringsuppgifter som du vanligtvis använder för att få åtkomst till tjänsten som du vill ansluta till.</span><span class="sxs-lookup"><span data-stu-id="9c070-114">To create a connection, you would need to provide the credentials you normally use to access the service you wish to connect to.</span></span> <span data-ttu-id="9c070-115">Så i Dropbox-exemplet skulle du behöva autentiseringsuppgifterna Dropbox-konto för att skapa anslutningen till Dropbox.</span><span class="sxs-lookup"><span data-stu-id="9c070-115">So, in the Dropbox example, you would need the credentials to your Dropbox account in order to create the connection to Dropbox.</span></span> [<span data-ttu-id="9c070-116">Mer information om anslutningar</span><span class="sxs-lookup"><span data-stu-id="9c070-116">Learn more about connections</span></span>]()

### <a name="create-a-connection-to-dropbox"></a><span data-ttu-id="9c070-117">Skapa en anslutning till Dropbox</span><span class="sxs-lookup"><span data-stu-id="9c070-117">Create a connection to Dropbox</span></span>
> [!INCLUDE [Steps to create a connection to Dropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a><span data-ttu-id="9c070-118">Använda en Dropbox-utlösare</span><span class="sxs-lookup"><span data-stu-id="9c070-118">Use a Dropbox trigger</span></span>
<span data-ttu-id="9c070-119">En utlösare är en händelse som kan användas för att starta arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="9c070-119">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="9c070-120">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="9c070-120">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="9c070-121">I det här exemplet ska vi använda den **när en fil skapas** utlösare.</span><span class="sxs-lookup"><span data-stu-id="9c070-121">In this example, we will use the **When a file is created** trigger.</span></span> <span data-ttu-id="9c070-122">När den här utlösaren uppstår kan vi ringer den **hämta innehåll med sökvägen** Dropbox-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="9c070-122">When this trigger occurs, we will call the **Get file content using path** Dropbox action.</span></span> 

1. <span data-ttu-id="9c070-123">Ange *dropbox* i sökrutan på Logic Apps designer väljer den **Dropbox - när en fil skapas** utlösare.</span><span class="sxs-lookup"><span data-stu-id="9c070-123">Enter *dropbox* in the search box on the Logic Apps designer, then select the **Dropbox - When a file is created** trigger.</span></span>      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. <span data-ttu-id="9c070-124">Välj den mapp där du vill spåra skapas.</span><span class="sxs-lookup"><span data-stu-id="9c070-124">Select the folder in which you want to track file creation.</span></span> <span data-ttu-id="9c070-125">Välj... (som identifieras i den röda rutan) och bläddra till mappen som du vill välja för utlösaren input.</span><span class="sxs-lookup"><span data-stu-id="9c070-125">Select ... (identified in the red box) and browse to the folder you wish to select for the trigger's input.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a><span data-ttu-id="9c070-126">Använda en Dropbox-åtgärd</span><span class="sxs-lookup"><span data-stu-id="9c070-126">Use a Dropbox action</span></span>
<span data-ttu-id="9c070-127">En åtgärd är en åtgärd som utförs av arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="9c070-127">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="9c070-128">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="9c070-128">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="9c070-129">Nu när utlösaren har lagts till, Följ dessa steg för att lägga till en åtgärd som får den nya filen innehåll.</span><span class="sxs-lookup"><span data-stu-id="9c070-129">Now that the trigger has been added, follow these steps to add an action that will get the new file's content.</span></span>

1. <span data-ttu-id="9c070-130">Välj **+ nytt steg** att lägga till den åtgärd som ska vidtas när en ny fil skapas.</span><span class="sxs-lookup"><span data-stu-id="9c070-130">Select **+ New Step** to add the action you would like to take when a new file is created.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. <span data-ttu-id="9c070-131">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="9c070-131">Select **Add an action**.</span></span> <span data-ttu-id="9c070-132">Den här öppnas sökrutan där du kan söka efter något du vill göra.</span><span class="sxs-lookup"><span data-stu-id="9c070-132">This opens the search box where you can search for any action you would like to take.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. <span data-ttu-id="9c070-133">Ange *dropbox* att söka efter åtgärder som rör Dropbox.</span><span class="sxs-lookup"><span data-stu-id="9c070-133">Enter *dropbox* to search for actions related to Dropbox.</span></span>  
4. <span data-ttu-id="9c070-134">Välj **Dropbox - Get-filinnehåll med sökvägen till** som åtgärden som ska vidtas när en ny fil skapas i den valda Dropbox-mappen.</span><span class="sxs-lookup"><span data-stu-id="9c070-134">Select **Dropbox - Get file content using path** as the action to take when a new file is created in the selected Dropbox folder.</span></span> <span data-ttu-id="9c070-135">Åtgärden kontrollen block öppnas.</span><span class="sxs-lookup"><span data-stu-id="9c070-135">The action control block opens.</span></span> <span data-ttu-id="9c070-136">Du uppmanas att godkänna logikappen åtkomst till Dropbox-konto om du inte har gjort det tidigare.</span><span class="sxs-lookup"><span data-stu-id="9c070-136">You will be prompted to authorize your logic app to access your Dropbox account if you have not done so previously.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. <span data-ttu-id="9c070-137">Välj... (på höger sida av den **filsökväg** kontroll) och bläddra till sökvägen till filen som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="9c070-137">Select ... (located at the right side of the **File Path** control) and browse to the file path you would like to use.</span></span> <span data-ttu-id="9c070-138">Eller Använd den **filsökväg** token för att påskynda genereringen av din app logik.</span><span class="sxs-lookup"><span data-stu-id="9c070-138">Or, use the **file path** token to speed up your logic app creation.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. <span data-ttu-id="9c070-139">Spara ditt arbete och skapa en ny fil i Dropbox aktivera arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="9c070-139">Save your work and create a new file in Dropbox to activate your workflow.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="9c070-140">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="9c070-140">Connector-specific details</span></span>

<span data-ttu-id="9c070-141">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/dropbox/).</span><span class="sxs-lookup"><span data-stu-id="9c070-141">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/dropbox/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="9c070-142">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="9c070-142">More connectors</span></span>
<span data-ttu-id="9c070-143">Gå tillbaka till den [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="9c070-143">Go back to the [APIs list](apis-list.md).</span></span>