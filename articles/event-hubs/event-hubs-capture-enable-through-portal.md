---
title: aaaAzure Event Hubs avbilda aktivera via portalen | Microsoft Docs
description: "Aktivera hello Event Hubs Capture-funktionen med hjälp av hello Azure-portalen."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: sethm
ms.openlocfilehash: 27c7528552c497a4d98873a22bd56a991c66247c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-event-hubs-capture-using-hello-azure-portal"></a><span data-ttu-id="ddc32-103">Aktivera Event Hubs hämtar med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ddc32-103">Enable Event Hubs Capture using hello Azure portal</span></span>

<span data-ttu-id="ddc32-104">Du kan konfigurera avbilda vid hello event hub skapandet med hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ddc32-104">You can configure Capture at hello event hub creation time using hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="ddc32-105">Du kan antingen avbildning hello data tooan Azure [Blob storage](https://azure.microsoft.com/services/storage/blobs/) behållare eller tooan [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) konto.</span><span class="sxs-lookup"><span data-stu-id="ddc32-105">You can either capture hello data tooan Azure [Blob storage](https://azure.microsoft.com/services/storage/blobs/) container, or tooan [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) account.</span></span>

## <a name="capture-data-tooan-azure-storage-account"></a><span data-ttu-id="ddc32-106">Samla in data tooan Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="ddc32-106">Capture data tooan Azure Storage account</span></span>  

<span data-ttu-id="ddc32-107">När du skapar en händelsehubb kan du aktivera avbildning genom att klicka på hello **på** knapp i hello **skapa Event Hub** portalblad.</span><span class="sxs-lookup"><span data-stu-id="ddc32-107">When you create an event hub, you can enable Capture by clicking hello **On** button in hello **Create Event Hub** portal blade.</span></span> <span data-ttu-id="ddc32-108">Sedan anger du ett Lagringskonto och behållare genom att klicka på **Azure Storage** i hello **avbilda providern** rutan.</span><span class="sxs-lookup"><span data-stu-id="ddc32-108">You then specify a Storage Account and container by clicking **Azure Storage** in hello **Capture Provider** box.</span></span> <span data-ttu-id="ddc32-109">Eftersom Event Hubs avbilda använder tjänst-till-tjänst-autentisering med lagring, behöver inte toospecify anslutningssträngen för lagring.</span><span class="sxs-lookup"><span data-stu-id="ddc32-109">Because Event Hubs Capture uses service-to-service authentication with storage, you do not need toospecify a storage connection string.</span></span> <span data-ttu-id="ddc32-110">hello resurs Väljaren väljs automatiskt hello resurs-URI för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ddc32-110">hello resource picker selects hello resource URI for your storage account automatically.</span></span> <span data-ttu-id="ddc32-111">Om du använder Azure Resource Manager måste du ange den här URI:n uttryckligen som en sträng.</span><span class="sxs-lookup"><span data-stu-id="ddc32-111">If you use Azure Resource Manager, you must supply this URI explicitly as a string.</span></span>

<span data-ttu-id="ddc32-112">hello standard tidsfönstret är 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="ddc32-112">hello default time window is 5 minutes.</span></span> <span data-ttu-id="ddc32-113">hello lägsta värdet är 1, hello maximala 15.</span><span class="sxs-lookup"><span data-stu-id="ddc32-113">hello minimum value is 1, hello maximum 15.</span></span> <span data-ttu-id="ddc32-114">Hej **storlek** fönstret har en uppsättning 10 500 MB.</span><span class="sxs-lookup"><span data-stu-id="ddc32-114">hello **Size** window has a range of 10-500 MB.</span></span>

![][1]

## <a name="capture-data-tooan-azure-data-lake-store-account"></a><span data-ttu-id="ddc32-115">Samla in data tooan Azure Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="ddc32-115">Capture data tooan Azure Data Lake Store account</span></span>

<span data-ttu-id="ddc32-116">toocapture data tooAzure Datasjölager du skapa ett Data Lake Store-konto och en händelsehubb:</span><span class="sxs-lookup"><span data-stu-id="ddc32-116">toocapture data tooAzure Data Lake Store, you create a Data Lake Store account, and an event hub:</span></span>

### <a name="create-an-azure-data-lake-store-account-and-folders"></a><span data-ttu-id="ddc32-117">Skapa ett Azure Data Lake Store-konto och mappar</span><span class="sxs-lookup"><span data-stu-id="ddc32-117">Create an Azure Data Lake Store account and folders</span></span>

1. <span data-ttu-id="ddc32-118">Skapa ett Data Lake Store-konto hello instruktionerna i [Kom igång med Azure Data Lake Store med hjälp av hello Azure-portalen](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ddc32-118">Create a Data Lake Store account, following hello instructions in [Get started with Azure Data Lake Store using hello Azure portal](../data-lake-store/data-lake-store-get-started-portal.md).</span></span> 
2. <span data-ttu-id="ddc32-119">Skapa en mapp under det här kontot hello instruktionerna i hello [skapa mappar i Azure Data Lake Store-konto](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ddc32-119">Create a folder under this account, following hello instructions in hello [Create folders in Azure Data Lake Store account](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) section.</span></span>
3. <span data-ttu-id="ddc32-120">I hello Data Lake Store-kontoblad klickar du på **Data Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ddc32-120">In hello Data Lake Store account blade, click **Data Explorer**.</span></span>
4. <span data-ttu-id="ddc32-121">Klicka på **Åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="ddc32-121">Click **Access**.</span></span>
5. <span data-ttu-id="ddc32-122">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ddc32-122">Click **Add**.</span></span>
6. <span data-ttu-id="ddc32-123">I hello **Sök efter namn eller e-** skriver **Microsoft.EventHubs** och välj sedan det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="ddc32-123">In hello **Search by name or email** box type **Microsoft.EventHubs** and then select this option.</span></span> 
7. <span data-ttu-id="ddc32-124">Hej **behörigheter** visas.</span><span class="sxs-lookup"><span data-stu-id="ddc32-124">hello **Permissions** tab appears.</span></span> <span data-ttu-id="ddc32-125">Ange hello behörigheter som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="ddc32-125">Set hello permissions as shown in hello following figure:</span></span>

    ![][6]

8. <span data-ttu-id="ddc32-126">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ddc32-126">Click **OK**.</span></span>
9. <span data-ttu-id="ddc32-127">Nu skapa en mapp i rotmappen för hello genom att bläddra toohello målmappen och klicka på hello mappnamn.</span><span class="sxs-lookup"><span data-stu-id="ddc32-127">Now, create a folder in hello root folder by browsing toohello target folder and clicking on hello folder name.</span></span>
10. <span data-ttu-id="ddc32-128">Klicka på **Åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="ddc32-128">Click **Access**.</span></span>
11. <span data-ttu-id="ddc32-129">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ddc32-129">Click **Add**.</span></span>
12. <span data-ttu-id="ddc32-130">I hello **Sök efter namn eller e-** skriver **Microsoft.EventHubs** och välj sedan det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="ddc32-130">In hello **Search by name or email** box type **Microsoft.EventHubs** and then select this option.</span></span>
13. <span data-ttu-id="ddc32-131">Hej **behörigheter** visas igen.</span><span class="sxs-lookup"><span data-stu-id="ddc32-131">hello **Permissions** tab appears again.</span></span> <span data-ttu-id="ddc32-132">Ange hello behörigheter som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="ddc32-132">Set hello permissions as shown in hello following figure:</span></span>

    ![][5]

### <a name="create-an-event-hub"></a><span data-ttu-id="ddc32-133">Skapa en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="ddc32-133">Create an event hub</span></span>

1. <span data-ttu-id="ddc32-134">Observera att hello händelsehubb måste vara i hello samma Azure-prenumeration som hello Azure Data Lake Store som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="ddc32-134">Note that hello event hub must be in hello same Azure subscription as hello Azure Data Lake Store you just created.</span></span> <span data-ttu-id="ddc32-135">Skapa hello händelsehubb, klicka på hello **på** knappen **avbilda** i hello **skapa Event Hub** portalblad.</span><span class="sxs-lookup"><span data-stu-id="ddc32-135">Create hello event hub, clicking hello **On** button under **Capture** in hello **Create Event Hub** portal blade.</span></span> 
2. <span data-ttu-id="ddc32-136">I hello **skapa Event Hub** portalbladet och välj **Azure Data Lake Store** från hello **avbilda providern** rutan.</span><span class="sxs-lookup"><span data-stu-id="ddc32-136">In hello **Create Event Hub** portal blade, select **Azure Data Lake Store** from hello **Capture Provider** box.</span></span>
3. <span data-ttu-id="ddc32-137">I **Välj Data Lake Store**, ange hello Data Lake Store-konto som du skapade tidigare och i hello **Data Lake sökvägen** anger hello sökvägen toohello datamapp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="ddc32-137">In **Select Data Lake Store**, specify hello Data Lake Store account you created previously, and in hello **Data Lake Path** field, enter hello path toohello data folder you created.</span></span>

    ![][3]

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a><span data-ttu-id="ddc32-138">Lägga till eller konfigurera avbildningsfunktionen i en befintlig händelsehubb</span><span class="sxs-lookup"><span data-stu-id="ddc32-138">Add or configure Capture on an existing event hub</span></span>

<span data-ttu-id="ddc32-139">Du kan konfigurera avbildningsfunktionen i befintliga händelsehubbar som finns i namnområden för Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="ddc32-139">You can configure Capture on existing event hubs that are in Event Hubs namespaces.</span></span> <span data-ttu-id="ddc32-140">tooenable på ett befintligt händelsehubb eller toochange avbilda dina hämtningsinställningar klickar du på hello namnområde tooload hello **Essentials** bladet Klicka hello händelsehubb som du vill tooenable eller ändra inställningen för hello avbildning.</span><span class="sxs-lookup"><span data-stu-id="ddc32-140">tooenable Capture on an existing event hub, or toochange your Capture settings, click hello namespace tooload hello **Essentials** blade, then click hello event hub for which you want tooenable or change hello Capture setting.</span></span> <span data-ttu-id="ddc32-141">Klicka slutligen på hello **egenskaper** avsnitt i hello öppna bladet och redigera hello hämtningsinställningar enligt hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="ddc32-141">Finally, click hello **Properties** section of hello open blade and then edit hello Capture settings, as shown in hello following figures:</span></span>

### <a name="azure-blob-storage"></a><span data-ttu-id="ddc32-142">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="ddc32-142">Azure Blob Storage</span></span>

![][2]

### <a name="azure-data-lake-store"></a><span data-ttu-id="ddc32-143">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ddc32-143">Azure Data Lake Store</span></span>

![][4]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png
[5]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture5.png
[6]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture6.png

## <a name="next-steps"></a><span data-ttu-id="ddc32-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ddc32-144">Next steps</span></span>

<span data-ttu-id="ddc32-145">Du kan också konfigurera avbildningsfunktionen i Event Hubs med hjälp av Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="ddc32-145">You can also configure Event Hubs Capture using Azure Resource Manager templates.</span></span> <span data-ttu-id="ddc32-146">Mer information finns i [Skapa ett namnområde för Event Hubs med en händelsehubb och aktivera avbildning med hjälp av en Azure Resource Manager-mall](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).</span><span class="sxs-lookup"><span data-stu-id="ddc32-146">For more information, see [Enable Capture using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).</span></span>
