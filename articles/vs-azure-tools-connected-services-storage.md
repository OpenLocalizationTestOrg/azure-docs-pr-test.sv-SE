---
title: "Lägg till Azure Storage med hjälp av anslutna Services i Visual Studio | Microsoft Docs"
description: "Lägg till Azure Storage i appen med hjälp av dialogrutan Visual Studio Lägg till anslutna tjänster"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 521ec044-ad4b-4828-8864-01decde2e758
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2017
ms.author: kraigb
ms.openlocfilehash: 35638083cd75e1b751d00a9c8163a3bc7480f0cd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a><span data-ttu-id="497ac-103">Lägga till Azure storage med hjälp av Visual Studio anslutna Services</span><span class="sxs-lookup"><span data-stu-id="497ac-103">Adding Azure storage by using Visual Studio Connected Services</span></span>
<span data-ttu-id="497ac-104">Med Visual Studio, du kan ansluta något av följande till Azure Storage med hjälp av den **Lägg till anslutna tjänster** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="497ac-104">With Visual Studio, you can connect any of the following to Azure Storage by using the **Add Connected Services** dialog:</span></span>

- <span data-ttu-id="497ac-105">C#-Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="497ac-105">C# cloud service</span></span>
- <span data-ttu-id="497ac-106">.NET-serverdel Mobiltjänst</span><span class="sxs-lookup"><span data-stu-id="497ac-106">.NET backend mobile service</span></span>
- <span data-ttu-id="497ac-107">ASP.NET-webbplats eller tjänst</span><span class="sxs-lookup"><span data-stu-id="497ac-107">ASP.NET website or service</span></span>
- <span data-ttu-id="497ac-108">ASP.NET Core service</span><span class="sxs-lookup"><span data-stu-id="497ac-108">ASP.NET Core service</span></span>
- <span data-ttu-id="497ac-109">Azure Webjobs-tjänst</span><span class="sxs-lookup"><span data-stu-id="497ac-109">Azure WebJob service</span></span> 

<span data-ttu-id="497ac-110">Funktionen anslutna tjänsten lägger till alla nödvändiga referenser och anslutningen kod i ditt projekt och ändrar konfigurationsfilerna på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="497ac-110">The connected service functionality adds all the needed references and connection code to your project, and modifies your configuration files appropriately.</span></span> 

<span data-ttu-id="497ac-111">När du har slutfört den **Lägg till anslutna tjänster** dialogrutan visar dokumentation med de steg som krävs för att börja arbeta med blob storage, köer och tabeller automatiskt.</span><span class="sxs-lookup"><span data-stu-id="497ac-111">After completion, the **Add Connected Services** dialog automatically displays documentation detailing the steps required to start working with blob storage, queues, and tables.</span></span>

## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a><span data-ttu-id="497ac-112">Ansluta till Azure Storage genom att använda dialogrutan anslutna tjänster</span><span class="sxs-lookup"><span data-stu-id="497ac-112">Connect to Azure Storage using the Connected Services dialog</span></span>
1. <span data-ttu-id="497ac-113">Öppna projektet i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="497ac-113">Open your project in Visual Studio</span></span>

1. <span data-ttu-id="497ac-114">I **Solution Explorer**, högerklicka på den **anslutna tjänster** nod, och från snabbmenyn och välj **Lägg till ansluten tjänst**.</span><span class="sxs-lookup"><span data-stu-id="497ac-114">In **Solution Explorer**, right-click the **Connected Services** node, and, from the context menu, and select **Add Connected Service**.</span></span>
   
    ![Lägg till Azure ansluten tjänst](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. <span data-ttu-id="497ac-116">I den **anslutna tjänster** väljer **lagringsutrymmet i molnet med Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="497ac-116">In the **Connected Services** page, select **Cloud Storage with Azure Storage**.</span></span>
   
    ![Lägg till Azure Storage](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. <span data-ttu-id="497ac-118">I den **Azure Storage** dialogrutan, Välj ett befintligt lagringskonto och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="497ac-118">In the **Azure Storage** dialog, select an existing storage account, and select **Add**.</span></span>
   
    <span data-ttu-id="497ac-119">Om du behöver skapa ett lagringskonto går du till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="497ac-119">If you need to create a storage account, go to the next step.</span></span> <span data-ttu-id="497ac-120">Annars fortsätter du till steg 6.</span><span class="sxs-lookup"><span data-stu-id="497ac-120">Otherwise, skip to step 6.</span></span>
    
    ![Lägg till befintligt lagringskonto i projekt](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. <span data-ttu-id="497ac-122">Skapa ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="497ac-122">To create a storage account:</span></span> 
   
   1. <span data-ttu-id="497ac-123">Välj **skapa ett nytt Lagringskonto** längst ned i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="497ac-123">Select **Create a New Storage Account** at the bottom of the dialog.</span></span>

   1. <span data-ttu-id="497ac-124">Fyll i den **skapa Lagringskonto** dialogrutan och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="497ac-124">Fill out the **Create Storage Account** dialog, and select **Create**.</span></span>
      
       ![Nya Azure storage-konto](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. <span data-ttu-id="497ac-126">När den **Azure Storage** dialogrutan visas, visas det nya lagringskontot i listan.</span><span class="sxs-lookup"><span data-stu-id="497ac-126">When the **Azure Storage** dialog is displayed, the new storage account appears in the list.</span></span> <span data-ttu-id="497ac-127">Markera det nya lagringskontot i listan och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="497ac-127">Select the new storage account in the list, and select **Add**.</span></span>

1. <span data-ttu-id="497ac-128">Lagring ansluten visas under den **referenser** nod i projektet.</span><span class="sxs-lookup"><span data-stu-id="497ac-128">The storage connected service appears under the **Service References** node of your project.</span></span>
   
## <a name="how-your-project-is-modified"></a><span data-ttu-id="497ac-129">Hur projektet har ändrats</span><span class="sxs-lookup"><span data-stu-id="497ac-129">How your project is modified</span></span>
<span data-ttu-id="497ac-130">När du avslutar dialogrutan lägger till referenser i Visual Studio och ändrar vissa konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="497ac-130">When you finish the dialog, Visual Studio adds references and modifies certain configuration files.</span></span> <span data-ttu-id="497ac-131">Vissa ändringar beror på projekttypen:</span><span class="sxs-lookup"><span data-stu-id="497ac-131">The specific changes depend on the project type:</span></span> 

- <span data-ttu-id="497ac-132">ASP.NET-projekt - [vad hände – ASP.NET-projekt](http://go.microsoft.com/fwlink/p/?LinkId=513126)</span><span class="sxs-lookup"><span data-stu-id="497ac-132">ASP.NET project - [What happened – ASP.NET Projects](http://go.microsoft.com/fwlink/p/?LinkId=513126)</span></span>
- <span data-ttu-id="497ac-133">ASP.NET Core projekt - [vad hände – ASP.NET 5 projekt](http://go.microsoft.com/fwlink/p/?LinkId=513124)</span><span class="sxs-lookup"><span data-stu-id="497ac-133">ASP.NET Core project - [What happened – ASP.NET 5 Projects](http://go.microsoft.com/fwlink/p/?LinkId=513124)</span></span> 
- <span data-ttu-id="497ac-134">Molntjänstprojekt (webb- och arbetsroller) - [vad hände – Cloud Service-projekt](http://go.microsoft.com/fwlink/p/?LinkId=516965)</span><span class="sxs-lookup"><span data-stu-id="497ac-134">Cloud service project (web roles and worker roles) - [What happened – Cloud Service projects](http://go.microsoft.com/fwlink/p/?LinkId=516965)</span></span>
- <span data-ttu-id="497ac-135">Webbjobbet projekt - [vad hände - Webbjobb projekt](visual-studio/vs-storage-webjobs-what-happened.md)</span><span class="sxs-lookup"><span data-stu-id="497ac-135">WebJob project - [What happened - WebJob projects](visual-studio/vs-storage-webjobs-what-happened.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="497ac-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="497ac-136">Next steps</span></span>
- [<span data-ttu-id="497ac-137">MSDN-Forum: Azure Storage</span><span class="sxs-lookup"><span data-stu-id="497ac-137">MSDN Forum: Azure Storage</span></span>](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [<span data-ttu-id="497ac-138">Microsoft Azure Storage-teamets blogg</span><span class="sxs-lookup"><span data-stu-id="497ac-138">Microsoft Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
- [<span data-ttu-id="497ac-139">Azure Storage-dokumentation</span><span class="sxs-lookup"><span data-stu-id="497ac-139">Azure Storage documentation</span></span>](https://docs.microsoft.com/azure/storage/)
