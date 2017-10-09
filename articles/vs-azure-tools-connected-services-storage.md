---
title: "aaaAdd Azure Storage med hjälp av anslutna Services i Visual Studio | Microsoft Docs"
description: "Lägg till Azure Storage tooyour app med hjälp av dialogrutan för hello Visual Studio Lägg till anslutna tjänster"
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
ms.openlocfilehash: 56b42063d86510b330e405108e28d50e6ba4da05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a><span data-ttu-id="91bae-103">Lägga till Azure storage med hjälp av Visual Studio anslutna Services</span><span class="sxs-lookup"><span data-stu-id="91bae-103">Adding Azure storage by using Visual Studio Connected Services</span></span>
<span data-ttu-id="91bae-104">Med Visual Studio kan du ansluta någon av följande tooAzure lagring med hjälp av hello hello **Lägg till anslutna tjänster** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="91bae-104">With Visual Studio, you can connect any of hello following tooAzure Storage by using hello **Add Connected Services** dialog:</span></span>

- <span data-ttu-id="91bae-105">C#-Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="91bae-105">C# cloud service</span></span>
- <span data-ttu-id="91bae-106">.NET-serverdel Mobiltjänst</span><span class="sxs-lookup"><span data-stu-id="91bae-106">.NET backend mobile service</span></span>
- <span data-ttu-id="91bae-107">ASP.NET-webbplats eller tjänst</span><span class="sxs-lookup"><span data-stu-id="91bae-107">ASP.NET website or service</span></span>
- <span data-ttu-id="91bae-108">ASP.NET Core service</span><span class="sxs-lookup"><span data-stu-id="91bae-108">ASP.NET Core service</span></span>
- <span data-ttu-id="91bae-109">Azure Webjobs-tjänst</span><span class="sxs-lookup"><span data-stu-id="91bae-109">Azure WebJob service</span></span> 

<span data-ttu-id="91bae-110">hello anslutna ytterligare funktioner läggs alla hello behövs referenser och anslutningen Kodprojekt tooyour och ändrar konfigurationsfilerna på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="91bae-110">hello connected service functionality adds all hello needed references and connection code tooyour project, and modifies your configuration files appropriately.</span></span> 

<span data-ttu-id="91bae-111">När du har slutfört hello **Lägg till anslutna tjänster** dialogrutan visar dokumentationen om hello steg krävs toostart arbetar med blob storage, köer och tabeller automatiskt.</span><span class="sxs-lookup"><span data-stu-id="91bae-111">After completion, hello **Add Connected Services** dialog automatically displays documentation detailing hello steps required toostart working with blob storage, queues, and tables.</span></span>

## <a name="connect-tooazure-storage-using-hello-connected-services-dialog"></a><span data-ttu-id="91bae-112">Ansluta tooAzure lagring med hjälp av hello anslutna tjänster dialogrutan</span><span class="sxs-lookup"><span data-stu-id="91bae-112">Connect tooAzure Storage using hello Connected Services dialog</span></span>
1. <span data-ttu-id="91bae-113">Öppna projektet i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91bae-113">Open your project in Visual Studio</span></span>

1. <span data-ttu-id="91bae-114">I **Solution Explorer**, högerklicka på hello **anslutna tjänster** nod, och hello snabbmenyn och välj **Lägg till ansluten tjänst**.</span><span class="sxs-lookup"><span data-stu-id="91bae-114">In **Solution Explorer**, right-click hello **Connected Services** node, and, from hello context menu, and select **Add Connected Service**.</span></span>
   
    ![Lägg till Azure ansluten tjänst](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. <span data-ttu-id="91bae-116">I hello **anslutna tjänster** väljer **lagringsutrymmet i molnet med Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="91bae-116">In hello **Connected Services** page, select **Cloud Storage with Azure Storage**.</span></span>
   
    ![Lägg till Azure Storage](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. <span data-ttu-id="91bae-118">I hello **Azure Storage** dialogrutan, Välj ett befintligt lagringskonto och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="91bae-118">In hello **Azure Storage** dialog, select an existing storage account, and select **Add**.</span></span>
   
    <span data-ttu-id="91bae-119">Om du behöver toocreate ett lagringskonto kan du gå toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="91bae-119">If you need toocreate a storage account, go toohello next step.</span></span> <span data-ttu-id="91bae-120">Annars fortsätter toostep 6.</span><span class="sxs-lookup"><span data-stu-id="91bae-120">Otherwise, skip toostep 6.</span></span>
    
    ![Lägg till befintlig lagring konto tooproject](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. <span data-ttu-id="91bae-122">toocreate ett lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="91bae-122">toocreate a storage account:</span></span> 
   
   1. <span data-ttu-id="91bae-123">Välj **skapa ett nytt Lagringskonto** längst hello hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91bae-123">Select **Create a New Storage Account** at hello bottom of hello dialog.</span></span>

   1. <span data-ttu-id="91bae-124">Fyll i hello **skapa Lagringskonto** dialogrutan och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="91bae-124">Fill out hello **Create Storage Account** dialog, and select **Create**.</span></span>
      
       ![Nya Azure storage-konto](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. <span data-ttu-id="91bae-126">När hello **Azure Storage** dialogrutan visas, visas hello nytt lagringskonto i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="91bae-126">When hello **Azure Storage** dialog is displayed, hello new storage account appears in hello list.</span></span> <span data-ttu-id="91bae-127">Välj hello nytt lagringskonto hello listan och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="91bae-127">Select hello new storage account in hello list, and select **Add**.</span></span>

1. <span data-ttu-id="91bae-128">Hej lagring ansluten visas under hello **referenser** nod i projektet.</span><span class="sxs-lookup"><span data-stu-id="91bae-128">hello storage connected service appears under hello **Service References** node of your project.</span></span>
   
## <a name="how-your-project-is-modified"></a><span data-ttu-id="91bae-129">Hur projektet har ändrats</span><span class="sxs-lookup"><span data-stu-id="91bae-129">How your project is modified</span></span>
<span data-ttu-id="91bae-130">När du är klar hello dialogrutan lägger till referenser i Visual Studio och ändrar vissa konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="91bae-130">When you finish hello dialog, Visual Studio adds references and modifies certain configuration files.</span></span> <span data-ttu-id="91bae-131">vissa ändringar för hello beror på hello projekttypen:</span><span class="sxs-lookup"><span data-stu-id="91bae-131">hello specific changes depend on hello project type:</span></span> 

- <span data-ttu-id="91bae-132">ASP.NET-projekt - [vad hände – ASP.NET-projekt](http://go.microsoft.com/fwlink/p/?LinkId=513126)</span><span class="sxs-lookup"><span data-stu-id="91bae-132">ASP.NET project - [What happened – ASP.NET Projects](http://go.microsoft.com/fwlink/p/?LinkId=513126)</span></span>
- <span data-ttu-id="91bae-133">ASP.NET Core projekt - [vad hände – ASP.NET 5 projekt](http://go.microsoft.com/fwlink/p/?LinkId=513124)</span><span class="sxs-lookup"><span data-stu-id="91bae-133">ASP.NET Core project - [What happened – ASP.NET 5 Projects](http://go.microsoft.com/fwlink/p/?LinkId=513124)</span></span> 
- <span data-ttu-id="91bae-134">Molntjänstprojekt (webb- och arbetsroller) - [vad hände – Cloud Service-projekt](http://go.microsoft.com/fwlink/p/?LinkId=516965)</span><span class="sxs-lookup"><span data-stu-id="91bae-134">Cloud service project (web roles and worker roles) - [What happened – Cloud Service projects](http://go.microsoft.com/fwlink/p/?LinkId=516965)</span></span>
- <span data-ttu-id="91bae-135">Webbjobbet projekt - [vad hände - Webbjobb projekt](visual-studio/vs-storage-webjobs-what-happened.md)</span><span class="sxs-lookup"><span data-stu-id="91bae-135">WebJob project - [What happened - WebJob projects](visual-studio/vs-storage-webjobs-what-happened.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="91bae-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91bae-136">Next steps</span></span>
- [<span data-ttu-id="91bae-137">MSDN-Forum: Azure Storage</span><span class="sxs-lookup"><span data-stu-id="91bae-137">MSDN Forum: Azure Storage</span></span>](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [<span data-ttu-id="91bae-138">Microsoft Azure Storage-teamets blogg</span><span class="sxs-lookup"><span data-stu-id="91bae-138">Microsoft Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
- [<span data-ttu-id="91bae-139">Azure Storage-dokumentation</span><span class="sxs-lookup"><span data-stu-id="91bae-139">Azure Storage documentation</span></span>](https://docs.microsoft.com/azure/storage/)
