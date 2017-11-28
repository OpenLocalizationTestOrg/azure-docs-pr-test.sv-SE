---
title: "aaaCapture data från Händelsehubbar i Azure Data Lake Store | Microsoft Docs"
description: "Använd Azure Data Lake Store toocapture data från Händelsehubbar"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 09b17bd0b47043bd2c83dba72c01a8064f206a0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-lake-store-toocapture-data-from-event-hubs"></a><span data-ttu-id="e72de-103">Använd Azure Data Lake Store toocapture data från Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="e72de-103">Use Azure Data Lake Store toocapture data from Event Hubs</span></span>

<span data-ttu-id="e72de-104">Lär dig hur toouse Azure Data Lake Store toocapture data tas emot av Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="e72de-104">Learn how toouse Azure Data Lake Store toocapture data received by Azure Event Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e72de-105">Krav</span><span class="sxs-lookup"><span data-stu-id="e72de-105">Prerequisites</span></span>

* <span data-ttu-id="e72de-106">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="e72de-106">**An Azure subscription**.</span></span> <span data-ttu-id="e72de-107">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e72de-107">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="e72de-108">**Ett Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="e72de-108">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="e72de-109">Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e72de-109">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md).</span></span>

*  <span data-ttu-id="e72de-110">**Ett namnområde för Händelsehubbar**.</span><span class="sxs-lookup"><span data-stu-id="e72de-110">**An Event Hubs namespace**.</span></span> <span data-ttu-id="e72de-111">Instruktioner finns i [skapa ett namnområde för Händelsehubbar](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace).</span><span class="sxs-lookup"><span data-stu-id="e72de-111">For instructions, see [Create an Event Hubs namespace](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace).</span></span> <span data-ttu-id="e72de-112">Kontrollera hello Data Lake Store-konto och hello Händelsehubbar namnområde i hello samma Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e72de-112">Make sure hello Data Lake Store account and hello Event Hubs namespace are in hello same Azure subscription.</span></span>


## <a name="assign-permissions-tooevent-hubs"></a><span data-ttu-id="e72de-113">Tilldela behörigheter tooEvent hubbar</span><span class="sxs-lookup"><span data-stu-id="e72de-113">Assign permissions tooEvent Hubs</span></span>

<span data-ttu-id="e72de-114">I det här avsnittet skapar du en mapp i hello konto där du vill toocapture hello data från Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="e72de-114">In this section, you create a folder within hello account where you want toocapture hello data from Event Hubs.</span></span> <span data-ttu-id="e72de-115">Du också tilldela behörigheter tooEvent Hubs så att det kan skriva data till ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="e72de-115">You also assign permissions tooEvent Hubs so that it can write data into a Data Lake Store account.</span></span> 

1. <span data-ttu-id="e72de-116">Öppna hello Data Lake Store-konto där du vill toocapture data från Händelsehubbar och klicka sedan på **Data Explorer**.</span><span class="sxs-lookup"><span data-stu-id="e72de-116">Open hello Data Lake Store account where you want toocapture data from Event Hubs and then click on **Data Explorer**.</span></span>

    <span data-ttu-id="e72de-117">![Data Lake Store data explorer](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Datasjölager data explorer")</span><span class="sxs-lookup"><span data-stu-id="e72de-117">![Data Lake Store data explorer](./media/data-lake-store-archive-eventhub-capture/data-lake-store-open-data-explorer.png "Data Lake Store data explorer")</span></span>

2.  <span data-ttu-id="e72de-118">Klicka på **ny mapp** och ange sedan ett namn på mappen där du vill toocapture hello data.</span><span class="sxs-lookup"><span data-stu-id="e72de-118">Click **New Folder** and then enter a name for folder where you want toocapture hello data.</span></span>

    <span data-ttu-id="e72de-119">![Skapa en ny mapp i Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "skapa en ny mapp i Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="e72de-119">![Create a new folder in Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-new-folder.png "Create a new folder in Data Lake Store")</span></span>

3. <span data-ttu-id="e72de-120">Tilldela behörigheter hello roten av hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="e72de-120">Assign permissions at hello root of hello Data Lake Store.</span></span> 

    <span data-ttu-id="e72de-121">a.</span><span class="sxs-lookup"><span data-stu-id="e72de-121">a.</span></span> <span data-ttu-id="e72de-122">Klicka på **Data Explorer**, Välj hello roten för hello Data Lake Store-konto och klicka sedan på **åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="e72de-122">Click **Data Explorer**, select hello root of hello Data Lake Store account, and then click **Access**.</span></span>

    <span data-ttu-id="e72de-123">![Tilldela behörigheter för Data Lake Store roten](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "tilldela behörigheter för Data Lake Store-rot")</span><span class="sxs-lookup"><span data-stu-id="e72de-123">![Assign permissions for Data Lake Store root](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-root.png "Assign permissions for Data Lake Store root")</span></span>

    <span data-ttu-id="e72de-124">b.</span><span class="sxs-lookup"><span data-stu-id="e72de-124">b.</span></span> <span data-ttu-id="e72de-125">Under **åtkomst**, klickar du på **Lägg till**, klickar du på **Välj användare eller grupp**, och sök sedan efter `Microsoft.EventHubs`.</span><span class="sxs-lookup"><span data-stu-id="e72de-125">Under **Access**, click **Add**, click **Select User or Group**, and then search for `Microsoft.EventHubs`.</span></span> 

    <span data-ttu-id="e72de-126">![Tilldela behörigheter för Data Lake Store roten](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "tilldela behörigheter för Data Lake Store-rot")</span><span class="sxs-lookup"><span data-stu-id="e72de-126">![Assign permissions for Data Lake Store root](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Assign permissions for Data Lake Store root")</span></span>
    
    <span data-ttu-id="e72de-127">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="e72de-127">Click **Select**.</span></span>

    <span data-ttu-id="e72de-128">c.</span><span class="sxs-lookup"><span data-stu-id="e72de-128">c.</span></span> <span data-ttu-id="e72de-129">Under **tilldela behörigheter**, klickar du på **Välj behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="e72de-129">Under **Assign Permissions**, click **Select Permissions**.</span></span> <span data-ttu-id="e72de-130">Ange **behörigheter** för**kör**.</span><span class="sxs-lookup"><span data-stu-id="e72de-130">Set **Permissions** too**Execute**.</span></span> <span data-ttu-id="e72de-131">Ange **lägga till** för**den här mappen och alla underordnade**.</span><span class="sxs-lookup"><span data-stu-id="e72de-131">Set **Add to** too**This folder and all children**.</span></span> <span data-ttu-id="e72de-132">Ange **lägga till som** för**en behörighetspost för åtkomst och en standard behörighetspost**.</span><span class="sxs-lookup"><span data-stu-id="e72de-132">Set **Add as** too**An access permission entry and a default permission entry**.</span></span>

    <span data-ttu-id="e72de-133">![Tilldela behörigheter för Data Lake Store roten](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "tilldela behörigheter för Data Lake Store-rot")</span><span class="sxs-lookup"><span data-stu-id="e72de-133">![Assign permissions for Data Lake Store root](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp1.png "Assign permissions for Data Lake Store root")</span></span>

    <span data-ttu-id="e72de-134">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e72de-134">Click **OK**.</span></span>

4. <span data-ttu-id="e72de-135">Tilldela behörigheter för hello mapp under Data Lake Store-konto där du vill toocapture data.</span><span class="sxs-lookup"><span data-stu-id="e72de-135">Assign permissions for hello folder under Data Lake Store account where you want toocapture data.</span></span>

    <span data-ttu-id="e72de-136">a.</span><span class="sxs-lookup"><span data-stu-id="e72de-136">a.</span></span> <span data-ttu-id="e72de-137">Klicka på **Data Explorer**, Välj hello mapp i hello Data Lake Store-konto och klicka sedan på **åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="e72de-137">Click **Data Explorer**, select hello folder in hello Data Lake Store account, and then click **Access**.</span></span>

    <span data-ttu-id="e72de-138">![Tilldela behörigheter för Data Lake Store-mapp](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "tilldela behörigheter för Data Lake Store-mapp")</span><span class="sxs-lookup"><span data-stu-id="e72de-138">![Assign permissions for Data Lake Store folder](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-permissions-to-folder.png "Assign permissions for Data Lake Store folder")</span></span>

    <span data-ttu-id="e72de-139">b.</span><span class="sxs-lookup"><span data-stu-id="e72de-139">b.</span></span> <span data-ttu-id="e72de-140">Under **åtkomst**, klickar du på **Lägg till**, klickar du på **Välj användare eller grupp**, och sök sedan efter `Microsoft.EventHubs`.</span><span class="sxs-lookup"><span data-stu-id="e72de-140">Under **Access**, click **Add**, click **Select User or Group**, and then search for `Microsoft.EventHubs`.</span></span> 

    <span data-ttu-id="e72de-141">![Tilldela behörigheter för Data Lake Store-mapp](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "tilldela behörigheter för Data Lake Store-mapp")</span><span class="sxs-lookup"><span data-stu-id="e72de-141">![Assign permissions for Data Lake Store folder](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp.png "Assign permissions for Data Lake Store folder")</span></span>
    
    <span data-ttu-id="e72de-142">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="e72de-142">Click **Select**.</span></span>

    <span data-ttu-id="e72de-143">c.</span><span class="sxs-lookup"><span data-stu-id="e72de-143">c.</span></span> <span data-ttu-id="e72de-144">Under **tilldela behörigheter**, klickar du på **Välj behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="e72de-144">Under **Assign Permissions**, click **Select Permissions**.</span></span> <span data-ttu-id="e72de-145">Ange **behörigheter** för**läsa, skriva,** och **kör**.</span><span class="sxs-lookup"><span data-stu-id="e72de-145">Set **Permissions** too**Read, Write,** and **Execute**.</span></span> <span data-ttu-id="e72de-146">Ange **lägga till** för**den här mappen och alla underordnade**.</span><span class="sxs-lookup"><span data-stu-id="e72de-146">Set **Add to** too**This folder and all children**.</span></span> <span data-ttu-id="e72de-147">Slutligen, Ställ **lägga till som** för**en behörighetspost för åtkomst och en standard behörighetspost**.</span><span class="sxs-lookup"><span data-stu-id="e72de-147">Finally, set **Add as** too**An access permission entry and a default permission entry**.</span></span>

    <span data-ttu-id="e72de-148">![Tilldela behörigheter för Data Lake Store-mapp](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "tilldela behörigheter för Data Lake Store-mapp")</span><span class="sxs-lookup"><span data-stu-id="e72de-148">![Assign permissions for Data Lake Store folder](./media/data-lake-store-archive-eventhub-capture/data-lake-store-assign-eventhub-sp-folder.png "Assign permissions for Data Lake Store folder")</span></span>
    
    <span data-ttu-id="e72de-149">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e72de-149">Click **OK**.</span></span> 

## <a name="configure-event-hubs-toocapture-data-toodata-lake-store"></a><span data-ttu-id="e72de-150">Konfigurera Händelsehubbar toocapture tooData Datasjölager</span><span class="sxs-lookup"><span data-stu-id="e72de-150">Configure Event Hubs toocapture data tooData Lake Store</span></span>

<span data-ttu-id="e72de-151">I det här avsnittet skapar du en Händelsehubb i ett namnområde för Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="e72de-151">In this section, you create an Event Hub within an Event Hubs namespace.</span></span> <span data-ttu-id="e72de-152">Du kan också konfigurera hello Event Hub toocapture data tooan Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="e72de-152">You also configure hello Event Hub toocapture data tooan Azure Data Lake Store account.</span></span> <span data-ttu-id="e72de-153">Det här avsnittet förutsätter att du redan har skapat ett namnområde för Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="e72de-153">This section assumes that you have already created an Event Hubs namespace.</span></span>

2. <span data-ttu-id="e72de-154">Från hello **översikt** i hello Händelsehubbar namnområde, klickar du på **+ Event Hub**.</span><span class="sxs-lookup"><span data-stu-id="e72de-154">From hello **Overview** pane of hello Event Hubs namespace, click **+ Event Hub**.</span></span>

    <span data-ttu-id="e72de-155">![Skapa Händelsehubb](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "skapar Händelsehubb")</span><span class="sxs-lookup"><span data-stu-id="e72de-155">![Create Event Hub](./media/data-lake-store-archive-eventhub-capture/data-lake-store-create-event-hub.png "Create Event Hub")</span></span>

3. <span data-ttu-id="e72de-156">Ange hello följande värden tooconfigure Händelsehubbar toocapture tooData Datasjölager.</span><span class="sxs-lookup"><span data-stu-id="e72de-156">Provide hello following values tooconfigure Event Hubs toocapture data tooData Lake Store.</span></span>

    <span data-ttu-id="e72de-157">![Skapa Händelsehubb](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "skapar Händelsehubb")</span><span class="sxs-lookup"><span data-stu-id="e72de-157">![Create Event Hub](./media/data-lake-store-archive-eventhub-capture/data-lake-store-configure-eventhub.png "Create Event Hub")</span></span>

    <span data-ttu-id="e72de-158">a.</span><span class="sxs-lookup"><span data-stu-id="e72de-158">a.</span></span> <span data-ttu-id="e72de-159">Ange ett namn för hello Event Hub.</span><span class="sxs-lookup"><span data-stu-id="e72de-159">Provide a name for hello Event Hub.</span></span>
    
    <span data-ttu-id="e72de-160">b.</span><span class="sxs-lookup"><span data-stu-id="e72de-160">b.</span></span> <span data-ttu-id="e72de-161">Den här självstudien anges **antalet partitioner** och **meddelandet kvarhållning** toohello standardvärden.</span><span class="sxs-lookup"><span data-stu-id="e72de-161">For this tutorial, set **Partition Count** and **Message Retention** toohello default values.</span></span>
    
    <span data-ttu-id="e72de-162">c.</span><span class="sxs-lookup"><span data-stu-id="e72de-162">c.</span></span> <span data-ttu-id="e72de-163">Ange **avbilda** för**på**.</span><span class="sxs-lookup"><span data-stu-id="e72de-163">Set **Capture** too**On**.</span></span> <span data-ttu-id="e72de-164">Ange hello **tidsfönstret** (hur ofta toocapture) och **storlek fönstret** (data storlek toocapture).</span><span class="sxs-lookup"><span data-stu-id="e72de-164">Set hello **Time Window** (how frequently toocapture) and **Size Window** (data size toocapture).</span></span> 
    
    <span data-ttu-id="e72de-165">d.</span><span class="sxs-lookup"><span data-stu-id="e72de-165">d.</span></span> <span data-ttu-id="e72de-166">För **avbilda providern**väljer **Azure Data Lake Store** och hello väljer hello Data Lake Store som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="e72de-166">For **Capture Provider**, select **Azure Data Lake Store** and hello select hello Data Lake Store you created earlier.</span></span> <span data-ttu-id="e72de-167">För **Data Lake sökvägen**, ange hello namnet på hello-mappen som du skapade i hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="e72de-167">For **Data Lake Path**, enter hello name of hello folder you created in hello Data Lake Store account.</span></span> <span data-ttu-id="e72de-168">Du behöver bara tooprovide hello relativ sökväg toohello mapp.</span><span class="sxs-lookup"><span data-stu-id="e72de-168">You only need tooprovide hello relative path toohello folder.</span></span>

    <span data-ttu-id="e72de-169">e.</span><span class="sxs-lookup"><span data-stu-id="e72de-169">e.</span></span> <span data-ttu-id="e72de-170">Lämna hello **exempel avbilda filformat namn** toohello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="e72de-170">Leave hello **Sample capture file name formats** toohello default value.</span></span> <span data-ttu-id="e72de-171">Det här alternativet styr hello mappstruktur som skapas under hello avbilda mapp.</span><span class="sxs-lookup"><span data-stu-id="e72de-171">This option governs hello folder structure that is created under hello capture folder.</span></span>

    <span data-ttu-id="e72de-172">f.</span><span class="sxs-lookup"><span data-stu-id="e72de-172">f.</span></span> <span data-ttu-id="e72de-173">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e72de-173">Click **Create**.</span></span>

## <a name="test-hello-setup"></a><span data-ttu-id="e72de-174">Inställningar för hello</span><span class="sxs-lookup"><span data-stu-id="e72de-174">Test hello setup</span></span>

<span data-ttu-id="e72de-175">Nu kan du testa hello lösning genom att skicka data toohello Azure Event Hub.</span><span class="sxs-lookup"><span data-stu-id="e72de-175">You can now test hello solution by sending data toohello Azure Event Hub.</span></span> <span data-ttu-id="e72de-176">Följ instruktionerna för hello på [skicka händelser tooAzure Händelsehubbar](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).</span><span class="sxs-lookup"><span data-stu-id="e72de-176">Follow hello instructions at [Send events tooAzure Event Hubs](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).</span></span> <span data-ttu-id="e72de-177">När du börjar skicka hello data Se du hello om data som visas i Data Lake Store använder hello mappstruktur som du angav.</span><span class="sxs-lookup"><span data-stu-id="e72de-177">Once you start sending hello data, you see hello data reflected in Data Lake Store using hello folder structure you specified.</span></span> <span data-ttu-id="e72de-178">Till exempel finns en mappstruktur som visas i följande skärmbild i Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="e72de-178">For example, you see a folder structure, as shown in hello following screenshot, in your Data Lake Store.</span></span>

<span data-ttu-id="e72de-179">![Exempel på EventHub-data i Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "exempel EventHub-data i Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="e72de-179">![Sample EventHub data in Data Lake Store](./media/data-lake-store-archive-eventhub-capture/data-lake-store-eventhub-data-sample.png "Sample EventHub data in Data Lake Store")</span></span>

> [!NOTE]
> <span data-ttu-id="e72de-180">Även om du inte har meddelanden som skickas till Händelsehubbar skriver Händelsehubbar tom filer med bara hello huvuden i hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="e72de-180">Even if you do not have messages coming into Event Hubs, Event Hubs writes empty files with just hello headers into hello Data Lake Store account.</span></span> <span data-ttu-id="e72de-181">hello skrivs på hello samma tidsintervall som du angav när du skapar hello Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="e72de-181">hello files are written at hello same time interval that you provided while creating hello Event Hubs.</span></span>
> 
>

## <a name="analyze-data-in-data-lake-store"></a><span data-ttu-id="e72de-182">Analysera data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e72de-182">Analyze data in Data Lake Store</span></span>

<span data-ttu-id="e72de-183">När hello data i Data Lake Store kan köra du analytiska jobb tooprocess och matar hello-data.</span><span class="sxs-lookup"><span data-stu-id="e72de-183">Once hello data is in Data Lake Store, you can run analytical jobs tooprocess and crunch hello data.</span></span> <span data-ttu-id="e72de-184">Se [USQL Avro exempel](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) om hur toodo detta med hjälp av Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="e72de-184">See [USQL Avro Example](https://github.com/Azure/usql/tree/master/Examples/AvroExamples) on how toodo this using Azure Data Lake Analytics.</span></span>
  

## <a name="see-also"></a><span data-ttu-id="e72de-185">Se även</span><span class="sxs-lookup"><span data-stu-id="e72de-185">See also</span></span>
* [<span data-ttu-id="e72de-186">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e72de-186">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="e72de-187">Kopiera data från Azure Storage BLOB tooData Datasjölager</span><span class="sxs-lookup"><span data-stu-id="e72de-187">Copy data from Azure Storage Blobs tooData Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
