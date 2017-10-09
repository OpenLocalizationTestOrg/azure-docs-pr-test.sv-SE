---
title: aaaMicrosoft Azure StorSimple Data Manager UI | Microsoft Docs
description: "Beskriver hur toouse StorSimple Data Manager-tjänsten Användargränssnittet (privat förhandsvisning)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: b0ee12b3e495400b54e48eb1a98c68b1af2e5f7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-using-hello-storsimple-data-manager-service-ui-private-preview"></a><span data-ttu-id="0df2b-103">Hantera med hjälp av hello StorSimple Data Manager-tjänsten Användargränssnittet (privat förhandsvisning)</span><span class="sxs-lookup"><span data-stu-id="0df2b-103">Manage using hello StorSimple Data Manager service UI (Private Preview)</span></span>

<span data-ttu-id="0df2b-104">Den här artikeln förklarar hur du kan använda hello omvandling av StorSimple Data Manager UI tooperform data på data som finns på hello StorSimple 8000-serien enheter.</span><span class="sxs-lookup"><span data-stu-id="0df2b-104">This article explains how you can use hello StorSimple Data Manager UI tooperform data transformation on data residing on hello StorSimple 8000 series devices.</span></span> <span data-ttu-id="0df2b-105">hello kan transformerade data sedan användas av andra Azure-tjänster, till exempel Azure Media Services, Azure HDInsight, Azure Machine Learning och Azure Search.</span><span class="sxs-lookup"><span data-stu-id="0df2b-105">hello transformed data can then be consumed by other Azure services such as Azure Media Services, Azure HDInsight, Azure Machine Learning, and Azure Search.</span></span> 


## <a name="use-storsimple-data-transformation"></a><span data-ttu-id="0df2b-106">Använda StorSimple Data Transformation</span><span class="sxs-lookup"><span data-stu-id="0df2b-106">Use StorSimple Data Transformation</span></span>

<span data-ttu-id="0df2b-107">Hej StorSimple Data Manager är hello resurs inom vilken Data Transformation kan initieras.</span><span class="sxs-lookup"><span data-stu-id="0df2b-107">hello StorSimple Data Manager is hello resource within which Data Transformation can be instantiated.</span></span> <span data-ttu-id="0df2b-108">hello Data Transformation service kan du flytta data från din StorSimple lokala enhet tooblobs i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="0df2b-108">hello Data Transformation service lets you move data from your StorSimple on-premises device tooblobs in Azure storage.</span></span> <span data-ttu-id="0df2b-109">Därför i arbetsflödet måste toospecify hello information om dina StorSimple-enheten och hello data av intresse som du vill toomove toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0df2b-109">Hence, in workflow you need toospecify hello details about your StorSimple device and hello data of interest that you want toomove toohello storage account.</span></span>

### <a name="create-a-storsimple-data-manager-service"></a><span data-ttu-id="0df2b-110">Skapa en StorSimple Data Manager-tjänst</span><span class="sxs-lookup"><span data-stu-id="0df2b-110">Create a StorSimple Data Manager service</span></span>

<span data-ttu-id="0df2b-111">Utföra hello följande steg toocreate en StorSimple Data Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0df2b-111">Perform hello following steps toocreate a StorSimple Data Manager service.</span></span>

1. <span data-ttu-id="0df2b-112">Gå toocreate StorSimple Data Manager-tjänsten för[https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)</span><span class="sxs-lookup"><span data-stu-id="0df2b-112">toocreate a StorSimple Data Manager service, go too[https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)</span></span>

2. <span data-ttu-id="0df2b-113">Klicka på hello  **+**  ikon och sökning för StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="0df2b-113">Click hello **+** icon and search for StorSimple Data Manager.</span></span> <span data-ttu-id="0df2b-114">Klicka på din StorSimple Data Manager-tjänst och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="0df2b-114">Click your StorSimple Data Manager service and then click **Create**.</span></span>

3. <span data-ttu-id="0df2b-115">Om din prenumeration är aktiverad för att skapa den här tjänsten finns hello följande bladet.</span><span class="sxs-lookup"><span data-stu-id="0df2b-115">If your subscription is enabled for creating this service, you see hello following blade.</span></span>

    ![Skapa en resurs för StorSimple Data chefer](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. <span data-ttu-id="0df2b-117">Ange hello indata och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="0df2b-117">Enter hello inputs and click **Create**.</span></span> <span data-ttu-id="0df2b-118">hello anges måste vara hello en som innehåller dina lagringskonton och StorSimple Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0df2b-118">hello specified location should be hello one that houses your storage accounts and your StorSimple Manager service.</span></span> <span data-ttu-id="0df2b-119">För närvarande stöds endast västra USA och Västeuropa regioner.</span><span class="sxs-lookup"><span data-stu-id="0df2b-119">Currently, only West US and West Europe regions are supported.</span></span> <span data-ttu-id="0df2b-120">Därför måste din StorSimple Manager-tjänsten, Data Manager-tjänsten och hello tillhörande storage-konto ska i hello föregående stöds regioner.</span><span class="sxs-lookup"><span data-stu-id="0df2b-120">Hence, your StorSimple Manager service, Data Manager service, and hello associated storage account should all be in hello preceding supported regions.</span></span> <span data-ttu-id="0df2b-121">Det tar en minut toocreate hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="0df2b-121">It takes about a minute toocreate hello service.</span></span>

### <a name="create-a-data-transformation-job-definition"></a><span data-ttu-id="0df2b-122">Skapa en data transformation jobbdefinition</span><span class="sxs-lookup"><span data-stu-id="0df2b-122">Create a data transformation job definition</span></span>

<span data-ttu-id="0df2b-123">Du måste toocreate data transformation jobbdefinitionen inom en StorSimple Data Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0df2b-123">Within a StorSimple Data Manager service, you need toocreate a data transformation job definition.</span></span> <span data-ttu-id="0df2b-124">En jobbdefinition anger information om hello data som du vill flytta till ett lagringskonto i hello native-format.</span><span class="sxs-lookup"><span data-stu-id="0df2b-124">A job definition specifies details of hello data that you are interested in moving into a storage account in hello native format.</span></span> 

<span data-ttu-id="0df2b-125">Utföra hello följande steg toocreate jobbdefinitionen för en ny omvandling.</span><span class="sxs-lookup"><span data-stu-id="0df2b-125">Perform hello following steps toocreate a new data transformation job definition.</span></span>

1.  <span data-ttu-id="0df2b-126">Navigera toohello tjänsten som du skapade.</span><span class="sxs-lookup"><span data-stu-id="0df2b-126">Navigate toohello service that you created.</span></span> <span data-ttu-id="0df2b-127">Klicka på **+ jobbet Definition**.</span><span class="sxs-lookup"><span data-stu-id="0df2b-127">Click **+ Job Definition**.</span></span>

    ![Klicka på + jobbdefinitionen](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. <span data-ttu-id="0df2b-129">hello nytt jobb definition blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="0df2b-129">hello new job definition blade opens up.</span></span> <span data-ttu-id="0df2b-130">Namnge din jobbdefinitionen och klicka på **källa**.</span><span class="sxs-lookup"><span data-stu-id="0df2b-130">Give your job definition a name and click **Source**.</span></span> <span data-ttu-id="0df2b-131">I hello **konfigurera datakällan** bladet ange hello information om din StorSimple-enhet och hello data av intresse.</span><span class="sxs-lookup"><span data-stu-id="0df2b-131">In hello **Configure data source** blade, specify hello details of your StorSimple device and hello data of interest.</span></span>

    ![Skapa jobbdefinitionen](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. <span data-ttu-id="0df2b-133">Eftersom det är en ny Data Manager-tjänst har inga databaser konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="0df2b-133">Since this is a new Data Manager service, no data repositories are configured.</span></span> <span data-ttu-id="0df2b-134">tooadd din StorSimple Manager som en data-databas, klicka på **Lägg till ny** i hello data databasen listrutan och klicka sedan på **Lägg till Data databas**.</span><span class="sxs-lookup"><span data-stu-id="0df2b-134">tooadd your StorSimple Manager as a data repository, click **Add new** in hello data repository dropdown and then click **Add Data Repository**.</span></span>

4. <span data-ttu-id="0df2b-135">Välj **StorSimple 8000-serien Manager** som hello databasen skriver och anger hello egenskaperna för din **StorSimple Manager**.</span><span class="sxs-lookup"><span data-stu-id="0df2b-135">Choose **StorSimple 8000 series Manager** as hello repository type and enter hello properties of your **StorSimple Manager**.</span></span> <span data-ttu-id="0df2b-136">För hello **resurs-Id** fält måste tooenter hello nummer före hello **:** i hello registreringsnyckel av din StorSimple manager.</span><span class="sxs-lookup"><span data-stu-id="0df2b-136">For hello **Resource Id** field, you need tooenter hello number before hello **:** in hello registration key of your StorSimple manager.</span></span>

    ![Skapa datakälla](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  <span data-ttu-id="0df2b-138">Klicka på **OK** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="0df2b-138">Click **OK** when done.</span></span> <span data-ttu-id="0df2b-139">Detta sparar data databasen och den här StorSimple Manager kan återanvändas i andra jobbdefinitioner utan att ange parametrarna igen.</span><span class="sxs-lookup"><span data-stu-id="0df2b-139">This saves your data repository and this StorSimple Manager can be reused in other job definitions without entering these parameters again.</span></span> <span data-ttu-id="0df2b-140">Det tar några sekunder efter att du klickar på **OK** för hello StorSimple Manager tooshow upp i hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="0df2b-140">It takes a few seconds after you click **OK** for hello StorSimple Manager tooshow up in hello dropdown.</span></span>

6.  <span data-ttu-id="0df2b-141">I hello **konfigurera datakällan** bladet ange hello enhetsnamnet och hello volymnamn som innehåller dina data av intresse.</span><span class="sxs-lookup"><span data-stu-id="0df2b-141">In hello **Configure data source** blade, enter hello device name and hello volume name that has your data of interest.</span></span>

7.  <span data-ttu-id="0df2b-142">I hello **Filter** underavsnittets, ange hello rotkatalog som innehåller dina data av intresse (det här fältet måste börja med en `\`).</span><span class="sxs-lookup"><span data-stu-id="0df2b-142">In hello **Filter** subsection, enter hello root directory that contains your data of interest (this field should start with a `\`).</span></span> <span data-ttu-id="0df2b-143">Du kan också lägga till alla filfilter.</span><span class="sxs-lookup"><span data-stu-id="0df2b-143">You can also add any file filters here.</span></span>

8.  <span data-ttu-id="0df2b-144">hello data transformation-tjänsten fungerar på hello data som skickas in toohello Azure via ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="0df2b-144">hello data transformation service works on hello data that is pushed up toohello Azure via snapshots.</span></span> <span data-ttu-id="0df2b-145">När du kör det här jobbet kan välja du tootake en säkerhetskopia varje gång det här jobbet körs (toowork på senaste data) eller toouse hello senaste säkerhetskopian i hello molnet (om du arbetar med vissa arkiverade data).</span><span class="sxs-lookup"><span data-stu-id="0df2b-145">When running this job, you can choose tootake a backup every time this job is run (toowork on latest data) or toouse hello last existing backup in hello cloud (if you are working on some archived data).</span></span>

    ![Information om ny datakälla](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. <span data-ttu-id="0df2b-147">Därefter måste hello Målinställningar toobe konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="0df2b-147">Next, hello Target settings need toobe configured.</span></span> <span data-ttu-id="0df2b-148">Det finns 2 typer stöds mål – Azure Media Services och Azure Storage-konton.</span><span class="sxs-lookup"><span data-stu-id="0df2b-148">There are 2 types of supported targets – Azure Storage accounts and Azure Media Services accounts.</span></span> <span data-ttu-id="0df2b-149">Välj lagringskonton tooput filer i BLOB på det kontot.</span><span class="sxs-lookup"><span data-stu-id="0df2b-149">Choose storage accounts tooput files into blobs in that account.</span></span> <span data-ttu-id="0df2b-150">Välj media services-konto tooput filer i tillgångar i kontot.</span><span class="sxs-lookup"><span data-stu-id="0df2b-150">Choose media services account tooput files into assets in that account.</span></span> <span data-ttu-id="0df2b-151">Vi behöver igen, tooadd en databas.</span><span class="sxs-lookup"><span data-stu-id="0df2b-151">Again, we need tooadd a repository.</span></span> <span data-ttu-id="0df2b-152">Välj i listrutan hello **Lägg till ny** och sedan **konfigurera inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="0df2b-152">In hello dropdown, select **Add new** and then **Configure settings**.</span></span>

    ![Skapa data sink](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. <span data-ttu-id="0df2b-154">Här, kan du välja hello typ av databasen som du vill att tooadd och hello andra parametrar som är associerade med hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="0df2b-154">Here, you can select hello type of repository you want tooadd and hello other parameters associated with hello repository.</span></span> <span data-ttu-id="0df2b-155">I båda fallen skapas en kö för lagring när hello jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="0df2b-155">In both cases, a storage queue is created when hello job runs.</span></span> <span data-ttu-id="0df2b-156">Den här kön fylls med meddelanden om transformerade blobbar i takt med att de är klara.</span><span class="sxs-lookup"><span data-stu-id="0df2b-156">This queue is populated with messages about transformed blobs as they are ready.</span></span> <span data-ttu-id="0df2b-157">hello namnet på den här kön är hello samma som hello namnet på hello jobbdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="0df2b-157">hello name of this queue is hello same as hello name of hello job definition.</span></span> <span data-ttu-id="0df2b-158">Om du väljer **Media Services** som hello lagringsplatsen typ, sedan kan du också ange lagringskontouppgifter där hello kön skapas.</span><span class="sxs-lookup"><span data-stu-id="0df2b-158">If you select **Media Services** as hello repo type, then you can also enter storage account credentials where hello queue is created.</span></span>

    ![Nya data sink information](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. <span data-ttu-id="0df2b-160">När du lägger till hello data databasen (som tar några sekunder) hittar du hello lagringsplatsen i hello listrutan hello **mål kontonamn**.</span><span class="sxs-lookup"><span data-stu-id="0df2b-160">After adding hello data repository (which takes a few seconds), you will find hello repo in hello dropdown in hello **Target account name**.</span></span>  <span data-ttu-id="0df2b-161">Välj hello-mål som du behöver.</span><span class="sxs-lookup"><span data-stu-id="0df2b-161">Choose hello target that you need.</span></span>

12. <span data-ttu-id="0df2b-162">Klicka på **OK** toocreate hello jobbdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="0df2b-162">Click **OK** toocreate hello job definition.</span></span> <span data-ttu-id="0df2b-163">Din jobbdefinition har nu konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="0df2b-163">Your job definition is now set up.</span></span> <span data-ttu-id="0df2b-164">Du kan använda den här jobbdefinitionen flera gånger via hello Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="0df2b-164">You can use this job definition multiple times via hello UI.</span></span>

    ![Lägg till ny jobbdefinition](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-hello-job-definition"></a><span data-ttu-id="0df2b-166">Kör hello jobbdefinitionen</span><span class="sxs-lookup"><span data-stu-id="0df2b-166">Run hello job definition</span></span>

<span data-ttu-id="0df2b-167">När du behöver toomove data från StorSimple toohello storage-konto som du har angett i hello jobbdefinitionen måste tooinvoke den.</span><span class="sxs-lookup"><span data-stu-id="0df2b-167">Whenever you need toomove data from StorSimple toohello storage account that you have specified in hello job definition, you will need tooinvoke it.</span></span> <span data-ttu-id="0df2b-168">Det finns vissa flexibilitet ändring hello parametrar varje gång du anropar hello jobb.</span><span class="sxs-lookup"><span data-stu-id="0df2b-168">There is some flexibility in changing hello parameters every time you invoke hello job.</span></span> <span data-ttu-id="0df2b-169">hello stegen är följande:</span><span class="sxs-lookup"><span data-stu-id="0df2b-169">hello steps are as follows:</span></span>

1. <span data-ttu-id="0df2b-170">Välj din StorSimple Data Manager-tjänsten och gå för**övervakning**.</span><span class="sxs-lookup"><span data-stu-id="0df2b-170">Select your StorSimple Data Manager service and go too**Monitoring**.</span></span> <span data-ttu-id="0df2b-171">Klicka på **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="0df2b-171">Click **Run Now**.</span></span>

    ![Utlösaren jobbdefinitionen](./media/storsimple-data-manager-ui/run-now.png)

2. <span data-ttu-id="0df2b-173">Välj hello jobbdefinitionen som du vill toorun.</span><span class="sxs-lookup"><span data-stu-id="0df2b-173">Choose hello job definition that you want toorun.</span></span> <span data-ttu-id="0df2b-174">Klicka på **körningsinställningar** toomodify alla inställningar som du kan toochange för det här jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="0df2b-174">Click **Run settings** toomodify any settings that you might want toochange for this job run.</span></span>

    ![Kör jobbinställningar](./media/storsimple-data-manager-ui/run-settings.png)

3. <span data-ttu-id="0df2b-176">Klicka på **OK** och klicka sedan på **kör** toolaunch ditt jobb.</span><span class="sxs-lookup"><span data-stu-id="0df2b-176">Click **OK** and then click **Run** toolaunch your job.</span></span> <span data-ttu-id="0df2b-177">toomonitor detta jobb, gå toohello **jobb** sidan i din StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="0df2b-177">toomonitor this job, go toohello **Jobs** page in your StorSimple Data Manager.</span></span>

    ![Lista över jobb och status](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. <span data-ttu-id="0df2b-179">I tillägg toomonitoring i hello **jobb** bladet du kan också lyssna på hello storage-kö där ett meddelande läggs varje gång en fil flyttas från StorSimple toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0df2b-179">In addition toomonitoring in hello **Jobs** blade, you can also listen on hello storage queue where a message is added every time a file is moved from StorSimple toohello storage account.</span></span>


## <a name="next-steps"></a><span data-ttu-id="0df2b-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0df2b-180">Next steps</span></span>

<span data-ttu-id="0df2b-181">[Använder .NET SDK toolaunch StorSimple Data Manager-jobb](storsimple-data-manager-dotnet-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="0df2b-181">[Use .NET SDK toolaunch StorSimple Data Manager jobs](storsimple-data-manager-dotnet-jobs.md).</span></span>
