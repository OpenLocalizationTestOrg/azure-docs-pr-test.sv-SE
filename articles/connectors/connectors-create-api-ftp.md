---
title: "Lär dig hur du använder FTP-anslutningen i logikappar | Microsoft Docs"
description: "Skapa logikappar med Azure App service. Anslut till FTP-server för att hantera dina filer. Du kan utföra olika åtgärder, till exempel ladda upp, uppdatera, hämta och ta bort filer i FTP-servern."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: d83c55fe-eb59-4b7b-a5ec-afac5c772616
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/22/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 61bfbedfd4f1e84b6976099323a32f3a720634c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-ftp-connector"></a><span data-ttu-id="734ca-105">Kom igång med FTP-anslutningen</span><span class="sxs-lookup"><span data-stu-id="734ca-105">Get started with the FTP connector</span></span>
<span data-ttu-id="734ca-106">Använd FTP-anslutningen för att övervaka, hantera och skapa filer på en FTP-server.</span><span class="sxs-lookup"><span data-stu-id="734ca-106">Use the FTP connector to monitor, manage and create files on an  FTP server.</span></span> 

<span data-ttu-id="734ca-107">Att använda [alla anslutningar](apis-list.md), måste du först skapa en logikapp.</span><span class="sxs-lookup"><span data-stu-id="734ca-107">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="734ca-108">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="734ca-108">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-ftp"></a><span data-ttu-id="734ca-109">Anslut till FTP</span><span class="sxs-lookup"><span data-stu-id="734ca-109">Connect to FTP</span></span>
<span data-ttu-id="734ca-110">Innan din logikapp kan komma åt någon tjänst, måste du först skapa en *anslutning* till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="734ca-110">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="734ca-111">En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="734ca-111">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-to-ftp"></a><span data-ttu-id="734ca-112">Skapa en anslutning till FTP</span><span class="sxs-lookup"><span data-stu-id="734ca-112">Create a connection to FTP</span></span>
> [!INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a><span data-ttu-id="734ca-113">Använda en FTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="734ca-113">Use a FTP trigger</span></span>
<span data-ttu-id="734ca-114">En utlösare är en händelse som kan användas för att starta arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="734ca-114">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="734ca-115">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="734ca-115">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="734ca-116">FTP-anslutningen kräver en FTP-server som är tillgänglig från Internet och är konfigurerad för att fungera med PASSIVT läge.</span><span class="sxs-lookup"><span data-stu-id="734ca-116">The FTP connector requires an FTP server that  is accessible from the Internet and is configured to operate with PASSIVE mode.</span></span> <span data-ttu-id="734ca-117">FTP-anslutningen är också **inte kompatibel med implicit FTPS (FTP över SSL)**.</span><span class="sxs-lookup"><span data-stu-id="734ca-117">Also, the FTP connector is **not compatible with implicit FTPS (FTP over SSL)**.</span></span> <span data-ttu-id="734ca-118">FTP-anslutningen har endast stöd för explicit FTPS (FTP över SSL).</span><span class="sxs-lookup"><span data-stu-id="734ca-118">The FTP connector only supports explicit FTPS (FTP over SSL).</span></span>  
> 
> 

<span data-ttu-id="734ca-119">I det här exemplet jag visar hur du använder den **FTP - när en fil har lagts till eller ändrats** utlöser ett arbetsflöde för logik app att starta när en fil har lagts till eller ändrats på en FTP-server.</span><span class="sxs-lookup"><span data-stu-id="734ca-119">In this example, I will show you how to use the **FTP - When a file is added or modified** trigger to initiate a logic app workflow when a file is added to, or modified on, an FTP server.</span></span> <span data-ttu-id="734ca-120">Du kan använda den här utlösaren för att övervaka en FTP-mapp för nya filer som representerar order från kunder i ett enterprise-exempel.</span><span class="sxs-lookup"><span data-stu-id="734ca-120">In an enterprise example, you could use this trigger to monitor an FTP folder for new files that represent orders from customers.</span></span>  <span data-ttu-id="734ca-121">Du kan sedan använda en åtgärd för FTP-anslutningen som **hämta filinnehåll** att hämta innehållet i ordningen för vidare bearbetning och lagring i order-databasen.</span><span class="sxs-lookup"><span data-stu-id="734ca-121">You could then use an FTP connector action such as **Get file content** to get the contents of the order for further processing and storage in your orders database.</span></span>

1. <span data-ttu-id="734ca-122">Ange *ftp* i sökrutan på logic apps designer väljer den **FTP - när en fil har lagts till eller ändrats** utlösare</span><span class="sxs-lookup"><span data-stu-id="734ca-122">Enter *ftp* in the search box on the logic apps designer then select the **FTP - When a file is added or modified**  trigger</span></span>   
   <span data-ttu-id="734ca-123">![FTP-utlösarbild 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)</span><span class="sxs-lookup"><span data-stu-id="734ca-123">![FTP trigger image 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)</span></span>  
   <span data-ttu-id="734ca-124">Den **när en fil har lagts till eller ändrats** kontrollen öppnas</span><span class="sxs-lookup"><span data-stu-id="734ca-124">The **When a file is added or modified** control opens up</span></span>  
   <span data-ttu-id="734ca-125">![Bild 2 till FTP-utlösare](./media/connectors-create-api-ftp/ftp-trigger-2.png)</span><span class="sxs-lookup"><span data-stu-id="734ca-125">![FTP trigger image 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)</span></span>  
2. <span data-ttu-id="734ca-126">Välj den **...**  finns på höger sida av kontrollen.</span><span class="sxs-lookup"><span data-stu-id="734ca-126">Select the **...** located on the right side of the control.</span></span> <span data-ttu-id="734ca-127">Då öppnas väljarkontrollen mapp</span><span class="sxs-lookup"><span data-stu-id="734ca-127">This opens the folder picker control</span></span>  
   <span data-ttu-id="734ca-128">![Bild 3 till FTP-utlösare](./media/connectors-create-api-ftp/ftp-trigger-3.png)</span><span class="sxs-lookup"><span data-stu-id="734ca-128">![FTP trigger image 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)</span></span>  
3. <span data-ttu-id="734ca-129">Välj den  **>**  (HÖGERPIL) och bläddra till mappen som du vill övervaka för nya eller ändrade filer.</span><span class="sxs-lookup"><span data-stu-id="734ca-129">Select the **>** (right arrow) and browse to find the folder that you want to monitor for new or modified files.</span></span> <span data-ttu-id="734ca-130">Välj mapp och mappen visas nu i meddelande i **mappen** kontroll.</span><span class="sxs-lookup"><span data-stu-id="734ca-130">Select the folder and notice the folder is now displayed in the **Folder** control.</span></span>  
   <span data-ttu-id="734ca-131">![Bild 4 till FTP-utlösare](./media/connectors-create-api-ftp/ftp-trigger-4.png)</span><span class="sxs-lookup"><span data-stu-id="734ca-131">![FTP trigger image 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)</span></span>   

<span data-ttu-id="734ca-132">Din logikapp har nu konfigurerats med en utlösare som börjar den andra utlösare och åtgärder i arbetsflödet körs när en fil ändras eller skapas i mappen specifika FTP.</span><span class="sxs-lookup"><span data-stu-id="734ca-132">At this point, your logic app has been configured with a trigger that will begin a run of the other triggers and actions in the workflow when a file is either modified or created in the specific FTP folder.</span></span> 

> [!NOTE]
> <span data-ttu-id="734ca-133">Det måste innehålla minst en utlösare och en åtgärd för en logikapp ska fungera.</span><span class="sxs-lookup"><span data-stu-id="734ca-133">For a logic app to be functional, it must contain at least one trigger and one action.</span></span> <span data-ttu-id="734ca-134">Följ stegen i nästa avsnitt för att lägga till en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="734ca-134">Follow the steps in the next section to add an action.</span></span>  
> 
> 

## <a name="use-a-ftp-action"></a><span data-ttu-id="734ca-135">Använda en FTP-åtgärd</span><span class="sxs-lookup"><span data-stu-id="734ca-135">Use a FTP action</span></span>
<span data-ttu-id="734ca-136">En åtgärd är en åtgärd som utförs av arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="734ca-136">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="734ca-137">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="734ca-137">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="734ca-138">Nu när du har lagt till en utlösare, Följ dessa steg för att lägga till en åtgärd som kommer att hämta innehållet i den nya eller ändrade filen som hittas av utlösaren.</span><span class="sxs-lookup"><span data-stu-id="734ca-138">Now that you have added a trigger, follow these steps to add an action that will get the contents of the new or modified file found by the trigger.</span></span>    

1. <span data-ttu-id="734ca-139">Välj **+ nytt steg** att lägga till den åtgärden för att hämta innehållet i filen på FTP-servern</span><span class="sxs-lookup"><span data-stu-id="734ca-139">Select **+ New step** to add the the action to get the contents of the file on the FTP server</span></span>  
2. <span data-ttu-id="734ca-140">Välj den **lägga till en åtgärd** länk.</span><span class="sxs-lookup"><span data-stu-id="734ca-140">Select the **Add an action** link.</span></span>  
   <span data-ttu-id="734ca-141">![Bild 1 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-1.png)</span><span class="sxs-lookup"><span data-stu-id="734ca-141">![FTP action image 1](./media/connectors-create-api-ftp/ftp-action-1.png)</span></span>  
3. <span data-ttu-id="734ca-142">Ange *FTP* att söka efter alla åtgärder som rör FTP.</span><span class="sxs-lookup"><span data-stu-id="734ca-142">Enter *FTP* to search for all actions related to FTP.</span></span>
4. <span data-ttu-id="734ca-143">Välj **FTP - hämta filinnehåll** som åtgärden som ska vidtas när en nya eller ändrade filen finns i FTP-mappen.</span><span class="sxs-lookup"><span data-stu-id="734ca-143">Select **FTP - Get file content**  as the action to take when a new or modified file is found in the FTP folder.</span></span>      
   <span data-ttu-id="734ca-144">![Bild 2 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-2.png)</span><span class="sxs-lookup"><span data-stu-id="734ca-144">![FTP action image 2](./media/connectors-create-api-ftp/ftp-action-2.png)</span></span>  
   <span data-ttu-id="734ca-145">Den **hämta filinnehåll** styra öppnas.</span><span class="sxs-lookup"><span data-stu-id="734ca-145">The **Get file content** control opens.</span></span> <span data-ttu-id="734ca-146">**Obs**: uppmanas du att godkänna din logikapp för att komma åt ditt konto för FTP-servern om du inte har gjort det tidigare.</span><span class="sxs-lookup"><span data-stu-id="734ca-146">**Note**: you will be prompted to authorize your logic app to access your FTP server account if you have not done so previously.</span></span>  
   <span data-ttu-id="734ca-147">![Bild 3 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-3.png)</span><span class="sxs-lookup"><span data-stu-id="734ca-147">![FTP action image 3](./media/connectors-create-api-ftp/ftp-action-3.png)</span></span>   
5. <span data-ttu-id="734ca-148">Välj den **filen** kontrollen (det tomma utrymmet som finns under **filen***).</span><span class="sxs-lookup"><span data-stu-id="734ca-148">Select the **File** control (the white space located below **FILE***).</span></span> <span data-ttu-id="734ca-149">Här kan använda du någon av de olika egenskaperna från den nya eller ändrade filen som hittas på FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="734ca-149">Here, you can use any of the various properties from the new or modified file found on the FTP server.</span></span>  
6. <span data-ttu-id="734ca-150">Välj den **filen innehåll** alternativet.</span><span class="sxs-lookup"><span data-stu-id="734ca-150">Select the **File content** option.</span></span>  
   <span data-ttu-id="734ca-151">![Bild 4 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-4.png)</span><span class="sxs-lookup"><span data-stu-id="734ca-151">![FTP action image 4](./media/connectors-create-api-ftp/ftp-action-4.png)</span></span>   
7. <span data-ttu-id="734ca-152">Kontrollen har uppdaterats, vilket betyder att den **FTP - hämta filinnehåll** åtgärd får den *filen innehåll* av nya eller ändrade filen på FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="734ca-152">The control is updated, indicating that the **FTP - Get file content** action will get the *file content* of the new or modified file on the FTP server.</span></span>      
   <span data-ttu-id="734ca-153">![Bild 5 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-5.png)</span><span class="sxs-lookup"><span data-stu-id="734ca-153">![FTP action image 5](./media/connectors-create-api-ftp/ftp-action-5.png)</span></span>     
8. <span data-ttu-id="734ca-154">Spara ditt arbete och sedan lägga till en fil i mappen FTP för att testa ditt arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="734ca-154">Save your work then add a file to the FTP folder to test your workflow.</span></span>    

<span data-ttu-id="734ca-155">Logikappen har nu konfigurerats med en utlösare för att övervaka en mapp på en FTP-server och starta arbetsflödet när den hittar en ny fil eller en fil på FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="734ca-155">At this point, the logic app has been configured with a trigger to monitor a folder on an FTP server and initiate the workflow when it finds either a new file or a modified file on the FTP server.</span></span> 

<span data-ttu-id="734ca-156">Logikappen har också konfigurerats med en åtgärd för att hämta innehållet i den nya eller ändrade filen.</span><span class="sxs-lookup"><span data-stu-id="734ca-156">The logic app also has been configured with an action to get the contents of the new or modified file.</span></span>

<span data-ttu-id="734ca-157">Du kan nu lägga till en annan åtgärd som den [SQL Server - infogningsraden](connectors-create-api-sqlazure.md) åtgärder för att infoga innehållet i den nya eller ändrade filen i en SQL-databastabell.</span><span class="sxs-lookup"><span data-stu-id="734ca-157">You can now add another action such as the [SQL Server - insert row](connectors-create-api-sqlazure.md) action to insert the contents of the new or modified file into a SQL database table.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="734ca-158">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="734ca-158">Connector-specific details</span></span>

<span data-ttu-id="734ca-159">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/ftpconnector/).</span><span class="sxs-lookup"><span data-stu-id="734ca-159">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/ftpconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="734ca-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="734ca-160">Next Steps</span></span>
[<span data-ttu-id="734ca-161">Skapa en logikapp</span><span class="sxs-lookup"><span data-stu-id="734ca-161">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

