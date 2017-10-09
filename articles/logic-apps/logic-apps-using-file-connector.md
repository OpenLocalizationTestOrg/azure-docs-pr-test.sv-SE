---
title: "aaaConnect tooon lokala filsystem från Azure Logic Apps | Microsoft Docs"
description: "Ansluta tooon lokala filsystem från logik app arbetsflödet via hello lokala datagateway och filsystemet connector"
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
ms.openlocfilehash: beb5565293def4aba81f63f19e77d7498aac38c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooon-premises-file-systems-from-logic-apps-with-hello-file-system-connector"></a><span data-ttu-id="141e3-104">Ansluta tooon lokala filsystem från logikappar med hello filsystemet connector</span><span class="sxs-lookup"><span data-stu-id="141e3-104">Connect tooon-premises file systems from logic apps with hello File System connector</span></span>

<span data-ttu-id="141e3-105">Molnet hybridanslutning är central toologic appar, så toomanage data och säker åtkomst till lokala resurser, dina logic apps kan använda hello lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="141e3-105">Hybrid cloud connectivity is central toologic apps, so toomanage data and securely access on-premises resources, your logic apps can use hello on-premises data gateway.</span></span> <span data-ttu-id="141e3-106">I den här artikeln visar vi hur tooconnect tooan lokalt filsystem med ett enkelt scenario: kopiera en fil som överförda tooDropbox tooa filresurs och sedan skicka ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="141e3-106">In this article, we show how tooconnect tooan on-premises file system with a basic scenario: copy a file that's uploaded tooDropbox tooa file share, then send an email.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="141e3-107">Krav</span><span class="sxs-lookup"><span data-stu-id="141e3-107">Prerequisites</span></span>

- <span data-ttu-id="141e3-108">Installera och konfigurera hello senaste [lokala datagateway](https://www.microsoft.com/download/details.aspx?id=53127).</span><span class="sxs-lookup"><span data-stu-id="141e3-108">Install and configure hello latest [on-premises data gateway](https://www.microsoft.com/download/details.aspx?id=53127).</span></span>
- <span data-ttu-id="141e3-109">Installera hello senaste lokala datagateway, version 1.15.6150.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="141e3-109">Install hello latest on-premises data gateway, version 1.15.6150.1 or above.</span></span> <span data-ttu-id="141e3-110">[Ansluta toohello lokala datagateway](http://aka.ms/logicapps-gateway) visar hello steg.</span><span class="sxs-lookup"><span data-stu-id="141e3-110">[Connect toohello on-premises data gateway](http://aka.ms/logicapps-gateway) lists hello steps.</span></span> <span data-ttu-id="141e3-111">hello gateway måste installeras på en lokal dator innan du kan fortsätta med hello resten av hello steg.</span><span class="sxs-lookup"><span data-stu-id="141e3-111">hello gateway must be installed on an on-premises machine before you can continue with hello rest of hello steps.</span></span>

## <a name="add-trigger-and-actions-for-connecting-tooyour-file-system"></a><span data-ttu-id="141e3-112">Lägg till utlösare och åtgärder för att ansluta tooyour filsystem</span><span class="sxs-lookup"><span data-stu-id="141e3-112">Add trigger and actions for connecting tooyour file system</span></span>

1. <span data-ttu-id="141e3-113">Skapa en logikapp och lägga till den här Dropbox-utlösare: **när en fil skapas**</span><span class="sxs-lookup"><span data-stu-id="141e3-113">Create a logic app, and add this Dropbox trigger: **When a file is created**</span></span> 
2. <span data-ttu-id="141e3-114">Välj under hello utlösaren **nästa steg** > **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="141e3-114">Under hello trigger, choose **Next Step** > **Add an action**.</span></span> 
3. <span data-ttu-id="141e3-115">Skriv i sökrutan hello `file system` så att du kan visa alla stöds åtgärder för hello filsystemet connector.</span><span class="sxs-lookup"><span data-stu-id="141e3-115">In hello search box, enter `file system` so you can view all supported actions for hello File System connector.</span></span>

   ![Sök efter filen connector](media/logic-apps-using-file-connector/search-file-connector.png)

2. <span data-ttu-id="141e3-117">Välj hello **skapa fil** åtgärd, och skapa en anslutning tooyour filsystem.</span><span class="sxs-lookup"><span data-stu-id="141e3-117">Choose hello **Create file** action, and create a connection tooyour file system.</span></span>

   <span data-ttu-id="141e3-118">Om du inte har en befintlig anslutning, kan du ange toocreate en.</span><span class="sxs-lookup"><span data-stu-id="141e3-118">If you don't have an existing connection, you are prompted toocreate one.</span></span>

   1. <span data-ttu-id="141e3-119">Välj **Anslut via lokala datagateway**.</span><span class="sxs-lookup"><span data-stu-id="141e3-119">Choose **Connect via on-premises data gateway**.</span></span> <span data-ttu-id="141e3-120">Fler egenskaper visas.</span><span class="sxs-lookup"><span data-stu-id="141e3-120">More properties appear.</span></span>
   2. <span data-ttu-id="141e3-121">Välj din rotmapp för filsystemet.</span><span class="sxs-lookup"><span data-stu-id="141e3-121">Select your root folder for your file system.</span></span>
      
       > [!NOTE]
       > <span data-ttu-id="141e3-122">hello rotmappen är hello huvudsakliga överordnade mappen som används för relativa sökvägar för alla fil-relaterade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="141e3-122">hello root folder is hello main parent folder, which is used for relative paths for all file-related actions.</span></span> <span data-ttu-id="141e3-123">Du kan ange en lokal mapp på hello datorn där hello lokala datagateway har installerats eller hello-mappen kan vara en nätverksresurs som hello datorn kan komma åt.</span><span class="sxs-lookup"><span data-stu-id="141e3-123">You can specify a local folder on hello machine where hello on-premises data gateway is installed, or hello folder can be a network share that hello machine can access.</span></span>

   3. <span data-ttu-id="141e3-124">Ange hello användarnamn och lösenord för hello gateway.</span><span class="sxs-lookup"><span data-stu-id="141e3-124">Enter hello username and password for hello gateway.</span></span>
   4. <span data-ttu-id="141e3-125">Välj hello-gateway som du tidigare har installerat.</span><span class="sxs-lookup"><span data-stu-id="141e3-125">Select hello gateway that you previously installed.</span></span>

       ![Konfigurera anslutning](media/logic-apps-using-file-connector/create-file.png)

3. <span data-ttu-id="141e3-127">När du har angett alla hello information, Välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="141e3-127">After you provide all hello details, choose **Create**.</span></span> 

   <span data-ttu-id="141e3-128">Logic Apps konfigurerar och testar anslutningen, se till att hello anslutning fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="141e3-128">Logic Apps configures and tests your connection, making sure that hello connection works properly.</span></span> 
   <span data-ttu-id="141e3-129">Om hello anslutningen har konfigurerats på rätt sätt finns alternativ för hello-åtgärd som du valde tidigare.</span><span class="sxs-lookup"><span data-stu-id="141e3-129">If hello connection is set up correctly, you see options for hello action that you previously selected.</span></span> 
   <span data-ttu-id="141e3-130">hello filen system koppling är nu redo för användning.</span><span class="sxs-lookup"><span data-stu-id="141e3-130">hello file system connector is now ready for use.</span></span>

4. <span data-ttu-id="141e3-131">Ange att du vill toocopy filer från Dropbox toohello rotmappen för din lokala filresurs.</span><span class="sxs-lookup"><span data-stu-id="141e3-131">Specify that you want toocopy files from Dropbox toohello root folder for your on-premises file share.</span></span>

   ![Skapa filen åtgärd](media/logic-apps-using-file-connector/create-file-filled.png)

5. <span data-ttu-id="141e3-133">Lägga till en Outlook-åtgärd som skickar ett e-postmeddelande så relevanta användarna vet om hello ny fil efter din logik app kopior hello-fil.</span><span class="sxs-lookup"><span data-stu-id="141e3-133">After your logic app copies hello file, add an Outlook action that sends an email so relevant users know about hello new file.</span></span> <span data-ttu-id="141e3-134">Ange hello mottagare, titel och brödtext hello e-post.</span><span class="sxs-lookup"><span data-stu-id="141e3-134">Enter hello recipients, title, and body of hello email.</span></span> 

   <span data-ttu-id="141e3-135">I hello dynamiskt innehåll selector du utdata från hello filen anslutningen så att du kan lägga till mer information om toohello e-post.</span><span class="sxs-lookup"><span data-stu-id="141e3-135">In hello dynamic content selector, you can choose data outputs from hello file connector so you can add more details toohello email.</span></span>

   ![Skicka e-åtgärd](media/logic-apps-using-file-connector/send-email.png)

6. <span data-ttu-id="141e3-137">Spara din logikapp.</span><span class="sxs-lookup"><span data-stu-id="141e3-137">Save your logic app.</span></span> <span data-ttu-id="141e3-138">Testa din app genom att överföra en fil tooDropbox.</span><span class="sxs-lookup"><span data-stu-id="141e3-138">Test your app by uploading a file tooDropbox.</span></span> <span data-ttu-id="141e3-139">hello-fil får kopieras toohello lokalt filresursen och du får ett e-postmeddelande om hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="141e3-139">hello file should get copied toohello on-premises file share, and you should receive an email about hello operation.</span></span>

   > [!TIP] 
   > <span data-ttu-id="141e3-140">Lär dig hur för[övervaka dina logikappar](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="141e3-140">Learn how too[monitor your logic apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span></span>

<span data-ttu-id="141e3-141">Grattis, du har nu en fungerande logikappen som kan ansluta tooyour lokala filsystem.</span><span class="sxs-lookup"><span data-stu-id="141e3-141">Congratulations, you now have a working logic app that can connect tooyour on-premises file system.</span></span> <span data-ttu-id="141e3-142">Försök att utforska andra funktioner som hello connector erbjudanden, till exempel:</span><span class="sxs-lookup"><span data-stu-id="141e3-142">Try exploring other functionalities that hello connector offers, for example:</span></span>

- <span data-ttu-id="141e3-143">Skapa fil</span><span class="sxs-lookup"><span data-stu-id="141e3-143">Create file</span></span>
- <span data-ttu-id="141e3-144">Listar filer i mappen</span><span class="sxs-lookup"><span data-stu-id="141e3-144">List files in folder</span></span>
- <span data-ttu-id="141e3-145">Lägg till fil</span><span class="sxs-lookup"><span data-stu-id="141e3-145">Append file</span></span>
- <span data-ttu-id="141e3-146">Ta bort fil</span><span class="sxs-lookup"><span data-stu-id="141e3-146">Delete file</span></span>
- <span data-ttu-id="141e3-147">Hämta innehåll</span><span class="sxs-lookup"><span data-stu-id="141e3-147">Get file content</span></span>
- <span data-ttu-id="141e3-148">Hämta filinnehåll med hjälp av sökväg</span><span class="sxs-lookup"><span data-stu-id="141e3-148">Get file content using path</span></span>
- <span data-ttu-id="141e3-149">Hämta filens metadata</span><span class="sxs-lookup"><span data-stu-id="141e3-149">Get file metadata</span></span>
- <span data-ttu-id="141e3-150">Hämta metadata för fil med hjälp av sökväg</span><span class="sxs-lookup"><span data-stu-id="141e3-150">Get file metadata using path</span></span>
- <span data-ttu-id="141e3-151">Listar filer i rotmappen</span><span class="sxs-lookup"><span data-stu-id="141e3-151">List files in root folder</span></span>
- <span data-ttu-id="141e3-152">Uppdatera-filen</span><span class="sxs-lookup"><span data-stu-id="141e3-152">Update file</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="141e3-153">Visa hello swagger</span><span class="sxs-lookup"><span data-stu-id="141e3-153">View hello swagger</span></span>
<span data-ttu-id="141e3-154">Se hello [swagger information](/connectors/fileconnector/).</span><span class="sxs-lookup"><span data-stu-id="141e3-154">See hello [swagger details](/connectors/fileconnector/).</span></span> 

## <a name="get-help"></a><span data-ttu-id="141e3-155">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="141e3-155">Get help</span></span>

<span data-ttu-id="141e3-156">tooask frågor besvara frågor och lär dig vilka andra Azure Logikappar användarna gör besök hello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="141e3-156">tooask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="141e3-157">toohelp förbättra Azure Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Azure Logikappar användare feedbackwebbplats](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="141e3-157">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="141e3-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="141e3-158">Next steps</span></span>

- <span data-ttu-id="141e3-159">[Ansluta tooon lokala data](../logic-apps/logic-apps-gateway-connection.md) från logikappar</span><span class="sxs-lookup"><span data-stu-id="141e3-159">[Connect tooon-premises data](../logic-apps/logic-apps-gateway-connection.md) from logic apps</span></span>
- <span data-ttu-id="141e3-160">Lär dig mer om [enterprise integration](../logic-apps/logic-apps-enterprise-integration-overview.md)</span><span class="sxs-lookup"><span data-stu-id="141e3-160">Learn about [enterprise integration](../logic-apps/logic-apps-enterprise-integration-overview.md)</span></span>
