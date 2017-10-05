---
title: "Ansluta till lokalt filsystem från Azure Logic Apps | Microsoft Docs"
description: "Ansluta till lokalt filsystem från logik app arbetsflödet via lokala datagateway och filsystemet connector"
keywords: Filsystem
services: logic-apps
author: derek1ee
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/27/2017
ms.author: LADocs; deli
ms.openlocfilehash: f33e7c58103c57e17e4e273caba1ab9b83f0cd2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-on-premises-file-systems-from-logic-apps-with-the-file-system-connector"></a><span data-ttu-id="b9c9a-104">Ansluta till lokalt filsystem från logikappar med filsystemet connector</span><span class="sxs-lookup"><span data-stu-id="b9c9a-104">Connect to on-premises file systems from logic apps with the File System connector</span></span>

<span data-ttu-id="b9c9a-105">Hybrid är cloud centrala för logic apps så för att hantera data och säker åtkomst till lokala resurser, dina logic apps kan använda lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-105">Hybrid cloud connectivity is central to logic apps, so to manage data and securely access on-premises resources, your logic apps can use the on-premises data gateway.</span></span> <span data-ttu-id="b9c9a-106">I den här artikeln visar vi hur du ansluter till ett lokalt filsystem med ett enkelt scenario: kopiera en fil som överförs till Dropbox till en filresurs och sedan skicka ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-106">In this article, we show how to connect to an on-premises file system with a basic scenario: copy a file that's uploaded to Dropbox to a file share, then send an email.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9c9a-107">Krav</span><span class="sxs-lookup"><span data-stu-id="b9c9a-107">Prerequisites</span></span>

- <span data-ttu-id="b9c9a-108">Installera och konfigurera senast [lokala datagateway](https://www.microsoft.com/download/details.aspx?id=53127).</span><span class="sxs-lookup"><span data-stu-id="b9c9a-108">Install and configure the latest [on-premises data gateway](https://www.microsoft.com/download/details.aspx?id=53127).</span></span>
- <span data-ttu-id="b9c9a-109">Installera den senaste version av lokala datagatewayen, version 1.15.6150.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-109">Install the latest on-premises data gateway, version 1.15.6150.1 or above.</span></span> <span data-ttu-id="b9c9a-110">[Ansluta till lokala datagateway](http://aka.ms/logicapps-gateway) innehåller stegen.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-110">[Connect to the on-premises data gateway](http://aka.ms/logicapps-gateway) lists the steps.</span></span> <span data-ttu-id="b9c9a-111">Gatewayen måste installeras på en lokal dator innan du fortsätter med resten av stegen.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-111">The gateway must be installed on an on-premises machine before you can continue with the rest of the steps.</span></span>

## <a name="add-trigger-and-actions-for-connecting-to-your-file-system"></a><span data-ttu-id="b9c9a-112">Lägga till utlösare och åtgärder för att ansluta till filsystemet</span><span class="sxs-lookup"><span data-stu-id="b9c9a-112">Add trigger and actions for connecting to your file system</span></span>

1. <span data-ttu-id="b9c9a-113">Skapa en logikapp och lägga till den här Dropbox-utlösare: **när en fil skapas**</span><span class="sxs-lookup"><span data-stu-id="b9c9a-113">Create a logic app, and add this Dropbox trigger: **When a file is created**</span></span> 
2. <span data-ttu-id="b9c9a-114">Välj under utlösare, **nästa steg** > **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-114">Under the trigger, choose **Next Step** > **Add an action**.</span></span> 
3. <span data-ttu-id="b9c9a-115">I sökrutan anger `file system` så att du kan visa alla stöds åtgärder för kopplingen filsystem.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-115">In the search box, enter `file system` so you can view all supported actions for the File System connector.</span></span>

   ![Sök efter filen connector](media/logic-apps-using-file-connector/search-file-connector.png)

2. <span data-ttu-id="b9c9a-117">Välj den **skapa fil** åtgärd, och skapa en anslutning till filsystemet.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-117">Choose the **Create file** action, and create a connection to your file system.</span></span>

   <span data-ttu-id="b9c9a-118">Om du inte har en befintlig anslutning, uppmanas du att skapa en.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-118">If you don't have an existing connection, you are prompted to create one.</span></span>

   1. <span data-ttu-id="b9c9a-119">Välj **Anslut via lokala datagateway**.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-119">Choose **Connect via on-premises data gateway**.</span></span> <span data-ttu-id="b9c9a-120">Fler egenskaper visas.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-120">More properties appear.</span></span>
   2. <span data-ttu-id="b9c9a-121">Välj din rotmapp för filsystemet.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-121">Select your root folder for your file system.</span></span>
      
       > [!NOTE]
       > <span data-ttu-id="b9c9a-122">Rotmappen är den viktigaste överordnade mappen som används för relativa sökvägar för alla fil-relaterade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-122">The root folder is the main parent folder, which is used for relative paths for all file-related actions.</span></span> <span data-ttu-id="b9c9a-123">Du kan ange en lokal mapp på datorn där lokala datagateway har installerats eller mappen kan vara en nätverksresurs som datorn kan komma åt.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-123">You can specify a local folder on the machine where the on-premises data gateway is installed, or the folder can be a network share that the machine can access.</span></span>

   3. <span data-ttu-id="b9c9a-124">Ange användarnamn och lösenord för gatewayen.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-124">Enter the username and password for the gateway.</span></span>
   4. <span data-ttu-id="b9c9a-125">Välj den gateway som du tidigare har installerat.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-125">Select the gateway that you previously installed.</span></span>

       ![Konfigurera anslutning](media/logic-apps-using-file-connector/create-file.png)

3. <span data-ttu-id="b9c9a-127">När du har angett alla detaljer väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-127">After you provide all the details, choose **Create**.</span></span> 

   <span data-ttu-id="b9c9a-128">Logic Apps konfigurerar och testar anslutningen, se till att anslutningen inte fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-128">Logic Apps configures and tests your connection, making sure that the connection works properly.</span></span> 
   <span data-ttu-id="b9c9a-129">Om anslutningen har konfigurerats på rätt sätt finns alternativ för den åtgärd som du valde tidigare.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-129">If the connection is set up correctly, you see options for the action that you previously selected.</span></span> 
   <span data-ttu-id="b9c9a-130">Filen system koppling är nu redo för användning.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-130">The file system connector is now ready for use.</span></span>

4. <span data-ttu-id="b9c9a-131">Ange att du vill kopiera filer från Dropbox till rotmappen för din lokala filresurs.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-131">Specify that you want to copy files from Dropbox to the root folder for your on-premises file share.</span></span>

   ![Skapa filen åtgärd](media/logic-apps-using-file-connector/create-file-filled.png)

5. <span data-ttu-id="b9c9a-133">När logikappen kopieras filen, kan du lägga till en Outlook-åtgärd som skickar ett e-postmeddelande så relevanta användarna vet om den nya filen.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-133">After your logic app copies the file, add an Outlook action that sends an email so relevant users know about the new file.</span></span> <span data-ttu-id="b9c9a-134">Ange mottagare, titel och brödtexten i e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-134">Enter the recipients, title, and body of the email.</span></span> 

   <span data-ttu-id="b9c9a-135">I den dynamiska innehåll selector du utdata från filen-anslutningen så att du kan lägga till mer information i e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-135">In the dynamic content selector, you can choose data outputs from the file connector so you can add more details to the email.</span></span>

   ![Skicka e-åtgärd](media/logic-apps-using-file-connector/send-email.png)

6. <span data-ttu-id="b9c9a-137">Spara din logikapp.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-137">Save your logic app.</span></span> <span data-ttu-id="b9c9a-138">Testa din app genom att överföra en fil till Dropbox.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-138">Test your app by uploading a file to Dropbox.</span></span> <span data-ttu-id="b9c9a-139">Filen ska kopieras till filresursen lokalt och du får ett e-postmeddelande om åtgärden.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-139">The file should get copied to the on-premises file share, and you should receive an email about the operation.</span></span>

   > [!TIP] 
   > <span data-ttu-id="b9c9a-140">Lär dig hur du [övervaka dina logikappar](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="b9c9a-140">Learn how to [monitor your logic apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span></span>

<span data-ttu-id="b9c9a-141">Grattis, du har nu en fungerande logikapp som kan ansluta till det lokala filsystemet.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-141">Congratulations, you now have a working logic app that can connect to your on-premises file system.</span></span> <span data-ttu-id="b9c9a-142">Försök att utforska andra funktioner som kopplingen erbjuder:</span><span class="sxs-lookup"><span data-stu-id="b9c9a-142">Try exploring other functionalities that the connector offers, for example:</span></span>

- <span data-ttu-id="b9c9a-143">Skapa fil</span><span class="sxs-lookup"><span data-stu-id="b9c9a-143">Create file</span></span>
- <span data-ttu-id="b9c9a-144">Listar filer i mappen</span><span class="sxs-lookup"><span data-stu-id="b9c9a-144">List files in folder</span></span>
- <span data-ttu-id="b9c9a-145">Lägg till fil</span><span class="sxs-lookup"><span data-stu-id="b9c9a-145">Append file</span></span>
- <span data-ttu-id="b9c9a-146">Ta bort fil</span><span class="sxs-lookup"><span data-stu-id="b9c9a-146">Delete file</span></span>
- <span data-ttu-id="b9c9a-147">Hämta innehåll</span><span class="sxs-lookup"><span data-stu-id="b9c9a-147">Get file content</span></span>
- <span data-ttu-id="b9c9a-148">Hämta filinnehåll med hjälp av sökväg</span><span class="sxs-lookup"><span data-stu-id="b9c9a-148">Get file content using path</span></span>
- <span data-ttu-id="b9c9a-149">Hämta filens metadata</span><span class="sxs-lookup"><span data-stu-id="b9c9a-149">Get file metadata</span></span>
- <span data-ttu-id="b9c9a-150">Hämta metadata för fil med hjälp av sökväg</span><span class="sxs-lookup"><span data-stu-id="b9c9a-150">Get file metadata using path</span></span>
- <span data-ttu-id="b9c9a-151">Listar filer i rotmappen</span><span class="sxs-lookup"><span data-stu-id="b9c9a-151">List files in root folder</span></span>
- <span data-ttu-id="b9c9a-152">Uppdatera-filen</span><span class="sxs-lookup"><span data-stu-id="b9c9a-152">Update file</span></span>

## <a name="view-the-swagger"></a><span data-ttu-id="b9c9a-153">Visa swagger</span><span class="sxs-lookup"><span data-stu-id="b9c9a-153">View the swagger</span></span>
<span data-ttu-id="b9c9a-154">Finns det [swagger information](/connectors/fileconnector/).</span><span class="sxs-lookup"><span data-stu-id="b9c9a-154">See the [swagger details](/connectors/fileconnector/).</span></span> 

## <a name="get-help"></a><span data-ttu-id="b9c9a-155">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="b9c9a-155">Get help</span></span>

<span data-ttu-id="b9c9a-156">I [Azure Logic Apps-forumet](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) kan du ställa frågor, besvara frågor och se vad andra Azure Logic Apps-användare håller på med.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-156">To ask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="b9c9a-157">På [webbplatsen för Azure Logic Apps-användarfeedback](http://aka.ms/logicapps-wish) kan du hjälpa till med att förbättra Azure Logic Apps och anslutningsapparna genom att rösta på förslag eller komma med egna förslag på förbättringar.</span><span class="sxs-lookup"><span data-stu-id="b9c9a-157">To help improve Azure Logic Apps and connectors, vote on or submit ideas at the [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9c9a-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9c9a-158">Next steps</span></span>

- <span data-ttu-id="b9c9a-159">[Ansluta till lokala data](../logic-apps/logic-apps-gateway-connection.md) från logikappar</span><span class="sxs-lookup"><span data-stu-id="b9c9a-159">[Connect to on-premises data](../logic-apps/logic-apps-gateway-connection.md) from logic apps</span></span>
- <span data-ttu-id="b9c9a-160">Lär dig mer om [enterprise integration](../logic-apps/logic-apps-enterprise-integration-overview.md)</span><span class="sxs-lookup"><span data-stu-id="b9c9a-160">Learn about [enterprise integration](../logic-apps/logic-apps-enterprise-integration-overview.md)</span></span>
