---
title: "Gränssnitt för Microsoft Azure StorSimple Data Manager | Microsoft Docs"
description: "Beskriver hur du använder StorSimple Data Manager-tjänsten Användargränssnittet (privat förhandsvisning)"
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
ms.openlocfilehash: 53a8599df2c647613122cd791b680e2e658586b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-using-the-storsimple-data-manager-service-ui-private-preview"></a><span data-ttu-id="1d48a-103">Hantera med hjälp av StorSimple Data Manager-tjänsten Användargränssnittet (privat förhandsvisning)</span><span class="sxs-lookup"><span data-stu-id="1d48a-103">Manage using the StorSimple Data Manager service UI (Private Preview)</span></span>

<span data-ttu-id="1d48a-104">Den här artikeln förklarar hur du kan använda StorSimple Data Manager-UI för att utföra Dataomvandling på data som finns på StorSimple 8000-serien enheter.</span><span class="sxs-lookup"><span data-stu-id="1d48a-104">This article explains how you can use the StorSimple Data Manager UI to perform data transformation on data residing on the StorSimple 8000 series devices.</span></span> <span data-ttu-id="1d48a-105">Omvandlade data kan sedan användas av andra Azure-tjänster, till exempel Azure Media Services, Azure HDInsight, Azure Machine Learning och Azure Search.</span><span class="sxs-lookup"><span data-stu-id="1d48a-105">The transformed data can then be consumed by other Azure services such as Azure Media Services, Azure HDInsight, Azure Machine Learning, and Azure Search.</span></span> 


## <a name="use-storsimple-data-transformation"></a><span data-ttu-id="1d48a-106">Använda StorSimple Data Transformation</span><span class="sxs-lookup"><span data-stu-id="1d48a-106">Use StorSimple Data Transformation</span></span>

<span data-ttu-id="1d48a-107">Datahanteraren StorSimple är resursen inom vilken Data Transformation kan initieras.</span><span class="sxs-lookup"><span data-stu-id="1d48a-107">The StorSimple Data Manager is the resource within which Data Transformation can be instantiated.</span></span> <span data-ttu-id="1d48a-108">DTS-tjänsten kan du flytta data från din lokala StorSimple-enhet till blobbar i Azure storage.</span><span class="sxs-lookup"><span data-stu-id="1d48a-108">The Data Transformation service lets you move data from your StorSimple on-premises device to blobs in Azure storage.</span></span> <span data-ttu-id="1d48a-109">Därför i arbetsflödet måste du ange information om din StorSimple-enhet och data av intresse som du vill flytta till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="1d48a-109">Hence, in workflow you need to specify the details about your StorSimple device and the data of interest that you want to move to the storage account.</span></span>

### <a name="create-a-storsimple-data-manager-service"></a><span data-ttu-id="1d48a-110">Skapa en StorSimple Data Manager-tjänst</span><span class="sxs-lookup"><span data-stu-id="1d48a-110">Create a StorSimple Data Manager service</span></span>

<span data-ttu-id="1d48a-111">Utför följande steg för att skapa en StorSimple Data Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1d48a-111">Perform the following steps to create a StorSimple Data Manager service.</span></span>

1. <span data-ttu-id="1d48a-112">Om du vill skapa en StorSimple Data Manager-tjänst, gå till [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)</span><span class="sxs-lookup"><span data-stu-id="1d48a-112">To create a StorSimple Data Manager service, go to [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)</span></span>

2. <span data-ttu-id="1d48a-113">Klicka på den  **+**  ikon och sökning för StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="1d48a-113">Click the **+** icon and search for StorSimple Data Manager.</span></span> <span data-ttu-id="1d48a-114">Klicka på din StorSimple Data Manager-tjänst och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="1d48a-114">Click your StorSimple Data Manager service and then click **Create**.</span></span>

3. <span data-ttu-id="1d48a-115">Om din prenumeration aktiveras för att skapa den här tjänsten kan se du följande bladet.</span><span class="sxs-lookup"><span data-stu-id="1d48a-115">If your subscription is enabled for creating this service, you see the following blade.</span></span>

    ![Skapa en resurs för StorSimple Data chefer](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. <span data-ttu-id="1d48a-117">Ange indata och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="1d48a-117">Enter the inputs and click **Create**.</span></span> <span data-ttu-id="1d48a-118">Den angivna platsen bör vara det som innehåller dina lagringskonton och StorSimple Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1d48a-118">The specified location should be the one that houses your storage accounts and your StorSimple Manager service.</span></span> <span data-ttu-id="1d48a-119">För närvarande stöds endast västra USA och Västeuropa regioner.</span><span class="sxs-lookup"><span data-stu-id="1d48a-119">Currently, only West US and West Europe regions are supported.</span></span> <span data-ttu-id="1d48a-120">Därför ska StorSimple Manager-tjänsten, Data Manager-tjänsten och det associerade lagringskontot vara i de föregående regionerna som stöds.</span><span class="sxs-lookup"><span data-stu-id="1d48a-120">Hence, your StorSimple Manager service, Data Manager service, and the associated storage account should all be in the preceding supported regions.</span></span> <span data-ttu-id="1d48a-121">Det tar ungefär en minut att skapa tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1d48a-121">It takes about a minute to create the service.</span></span>

### <a name="create-a-data-transformation-job-definition"></a><span data-ttu-id="1d48a-122">Skapa en data transformation jobbdefinition</span><span class="sxs-lookup"><span data-stu-id="1d48a-122">Create a data transformation job definition</span></span>

<span data-ttu-id="1d48a-123">Inom en StorSimple Data Manager-tjänsten måste du skapa en data transformation jobbdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="1d48a-123">Within a StorSimple Data Manager service, you need to create a data transformation job definition.</span></span> <span data-ttu-id="1d48a-124">En jobbdefinition anger information om de data som du vill flytta till ett lagringskonto i native-format.</span><span class="sxs-lookup"><span data-stu-id="1d48a-124">A job definition specifies details of the data that you are interested in moving into a storage account in the native format.</span></span> 

<span data-ttu-id="1d48a-125">Utför följande steg för att skapa en ny data transformation jobbdefinition.</span><span class="sxs-lookup"><span data-stu-id="1d48a-125">Perform the following steps to create a new data transformation job definition.</span></span>

1.  <span data-ttu-id="1d48a-126">Navigera till den tjänst som du skapade.</span><span class="sxs-lookup"><span data-stu-id="1d48a-126">Navigate to the service that you created.</span></span> <span data-ttu-id="1d48a-127">Klicka på **+ jobbet Definition**.</span><span class="sxs-lookup"><span data-stu-id="1d48a-127">Click **+ Job Definition**.</span></span>

    ![Klicka på + jobbdefinitionen](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. <span data-ttu-id="1d48a-129">Nytt jobb definition blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="1d48a-129">The new job definition blade opens up.</span></span> <span data-ttu-id="1d48a-130">Namnge din jobbdefinitionen och klicka på **källa**.</span><span class="sxs-lookup"><span data-stu-id="1d48a-130">Give your job definition a name and click **Source**.</span></span> <span data-ttu-id="1d48a-131">I den **konfigurera datakällan** bladet, ange information om din StorSimple-enhet och data av intresse.</span><span class="sxs-lookup"><span data-stu-id="1d48a-131">In the **Configure data source** blade, specify the details of your StorSimple device and the data of interest.</span></span>

    ![Skapa jobbdefinitionen](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. <span data-ttu-id="1d48a-133">Eftersom det är en ny Data Manager-tjänst har inga databaser konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="1d48a-133">Since this is a new Data Manager service, no data repositories are configured.</span></span> <span data-ttu-id="1d48a-134">Om du vill lägga till din StorSimple Manager som en data-databas, klickar du på **Lägg till ny** i data databasen listrutan och klicka sedan på **Lägg till Data databas**.</span><span class="sxs-lookup"><span data-stu-id="1d48a-134">To add your StorSimple Manager as a data repository, click **Add new** in the data repository dropdown and then click **Add Data Repository**.</span></span>

4. <span data-ttu-id="1d48a-135">Välj **StorSimple 8000-serien Manager** som databasen skriver och anger egenskaperna för din **StorSimple Manager**.</span><span class="sxs-lookup"><span data-stu-id="1d48a-135">Choose **StorSimple 8000 series Manager** as the repository type and enter the properties of your **StorSimple Manager**.</span></span> <span data-ttu-id="1d48a-136">För den **resurs-Id** fältet, måste du ange hur många innan den **:** i Registreringsnyckeln för din StorSimple manager.</span><span class="sxs-lookup"><span data-stu-id="1d48a-136">For the **Resource Id** field, you need to enter the number before the **:** in the registration key of your StorSimple manager.</span></span>

    ![Skapa datakälla](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  <span data-ttu-id="1d48a-138">Klicka på **OK** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="1d48a-138">Click **OK** when done.</span></span> <span data-ttu-id="1d48a-139">Detta sparar data databasen och den här StorSimple Manager kan återanvändas i andra jobbdefinitioner utan att ange parametrarna igen.</span><span class="sxs-lookup"><span data-stu-id="1d48a-139">This saves your data repository and this StorSimple Manager can be reused in other job definitions without entering these parameters again.</span></span> <span data-ttu-id="1d48a-140">Det tar några sekunder efter att du klickar på **OK** för StorSimple Manager visas i listrutan.</span><span class="sxs-lookup"><span data-stu-id="1d48a-140">It takes a few seconds after you click **OK** for the StorSimple Manager to show up in the dropdown.</span></span>

6.  <span data-ttu-id="1d48a-141">I den **konfigurera datakällan** bladet anger du namnet på en enhet och namn på volymer som innehåller dina data av intresse.</span><span class="sxs-lookup"><span data-stu-id="1d48a-141">In the **Configure data source** blade, enter the device name and the volume name that has your data of interest.</span></span>

7.  <span data-ttu-id="1d48a-142">I den **Filter** underavsnittets, ange rotkatalogen som innehåller dina data av intresse (det här fältet måste börja med en `\`).</span><span class="sxs-lookup"><span data-stu-id="1d48a-142">In the **Filter** subsection, enter the root directory that contains your data of interest (this field should start with a `\`).</span></span> <span data-ttu-id="1d48a-143">Du kan också lägga till alla filfilter.</span><span class="sxs-lookup"><span data-stu-id="1d48a-143">You can also add any file filters here.</span></span>

8.  <span data-ttu-id="1d48a-144">Tjänsten data transformation fungerar på de data som skickas till Azure via ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="1d48a-144">The data transformation service works on the data that is pushed up to the Azure via snapshots.</span></span> <span data-ttu-id="1d48a-145">När du kör det här jobbet kan du ta en säkerhetskopia varje gång det här jobbet körs (för att arbeta med senaste data) eller använda den senaste säkerhetskopian i molnet (om du arbetar med vissa arkiverade data).</span><span class="sxs-lookup"><span data-stu-id="1d48a-145">When running this job, you can choose to take a backup every time this job is run (to work on latest data) or to use the last existing backup in the cloud (if you are working on some archived data).</span></span>

    ![Information om ny datakälla](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. <span data-ttu-id="1d48a-147">Därefter måste mål-inställningar konfigureras.</span><span class="sxs-lookup"><span data-stu-id="1d48a-147">Next, the Target settings need to be configured.</span></span> <span data-ttu-id="1d48a-148">Det finns 2 typer stöds mål – Azure Media Services och Azure Storage-konton.</span><span class="sxs-lookup"><span data-stu-id="1d48a-148">There are 2 types of supported targets – Azure Storage accounts and Azure Media Services accounts.</span></span> <span data-ttu-id="1d48a-149">Välj storage-konton för att placera filer i BLOB på det kontot.</span><span class="sxs-lookup"><span data-stu-id="1d48a-149">Choose storage accounts to put files into blobs in that account.</span></span> <span data-ttu-id="1d48a-150">Välj media services-konto för att placera filer i tillgångar i kontot.</span><span class="sxs-lookup"><span data-stu-id="1d48a-150">Choose media services account to put files into assets in that account.</span></span> <span data-ttu-id="1d48a-151">Vi behöver igen och Lägg till en databas.</span><span class="sxs-lookup"><span data-stu-id="1d48a-151">Again, we need to add a repository.</span></span> <span data-ttu-id="1d48a-152">Välj i listrutan, **Lägg till ny** och sedan **konfigurera inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="1d48a-152">In the dropdown, select **Add new** and then **Configure settings**.</span></span>

    ![Skapa data sink](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. <span data-ttu-id="1d48a-154">Här, kan du välja vilken typ av databasen som du vill lägga till och de andra parametrarna som är kopplade till databasen.</span><span class="sxs-lookup"><span data-stu-id="1d48a-154">Here, you can select the type of repository you want to add and the other parameters associated with the repository.</span></span> <span data-ttu-id="1d48a-155">I båda fallen skapas en kö för lagring när jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="1d48a-155">In both cases, a storage queue is created when the job runs.</span></span> <span data-ttu-id="1d48a-156">Den här kön fylls med meddelanden om transformerade blobbar i takt med att de är klara.</span><span class="sxs-lookup"><span data-stu-id="1d48a-156">This queue is populated with messages about transformed blobs as they are ready.</span></span> <span data-ttu-id="1d48a-157">Namnet på den här kön är detsamma som namnet på jobbdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="1d48a-157">The name of this queue is the same as the name of the job definition.</span></span> <span data-ttu-id="1d48a-158">Om du väljer **Media Services** som lagringsplatsen typ, sedan kan du också ange lagringskontouppgifter där kön skapas.</span><span class="sxs-lookup"><span data-stu-id="1d48a-158">If you select **Media Services** as the repo type, then you can also enter storage account credentials where the queue is created.</span></span>

    ![Nya data sink information](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. <span data-ttu-id="1d48a-160">När du lägger till data-databasen (som tar några sekunder) hittar du lagringsplatsen i listrutan i den **mål kontonamn**.</span><span class="sxs-lookup"><span data-stu-id="1d48a-160">After adding the data repository (which takes a few seconds), you will find the repo in the dropdown in the **Target account name**.</span></span>  <span data-ttu-id="1d48a-161">Välj det mål som du behöver.</span><span class="sxs-lookup"><span data-stu-id="1d48a-161">Choose the target that you need.</span></span>

12. <span data-ttu-id="1d48a-162">Klicka på **OK** att skapa jobbdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="1d48a-162">Click **OK** to create the job definition.</span></span> <span data-ttu-id="1d48a-163">Din jobbdefinition har nu konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="1d48a-163">Your job definition is now set up.</span></span> <span data-ttu-id="1d48a-164">Du kan använda den här jobbdefinitionen flera gånger via Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="1d48a-164">You can use this job definition multiple times via the UI.</span></span>

    ![Lägg till ny jobbdefinition](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-the-job-definition"></a><span data-ttu-id="1d48a-166">Kör jobbdefinitionen</span><span class="sxs-lookup"><span data-stu-id="1d48a-166">Run the job definition</span></span>

<span data-ttu-id="1d48a-167">När du behöver flytta data från StorSimple till lagringskontot som du har angett i jobbdefinitionen behöver du anropar den.</span><span class="sxs-lookup"><span data-stu-id="1d48a-167">Whenever you need to move data from StorSimple to the storage account that you have specified in the job definition, you will need to invoke it.</span></span> <span data-ttu-id="1d48a-168">Det finns vissa flexibilitet i varje gång du anropar jobbet att ändra parametrarna.</span><span class="sxs-lookup"><span data-stu-id="1d48a-168">There is some flexibility in changing the parameters every time you invoke the job.</span></span> <span data-ttu-id="1d48a-169">Stegen är följande:</span><span class="sxs-lookup"><span data-stu-id="1d48a-169">The steps are as follows:</span></span>

1. <span data-ttu-id="1d48a-170">Välj din StorSimple Data Manager-tjänsten och gå till **övervakning**.</span><span class="sxs-lookup"><span data-stu-id="1d48a-170">Select your StorSimple Data Manager service and go to **Monitoring**.</span></span> <span data-ttu-id="1d48a-171">Klicka på **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="1d48a-171">Click **Run Now**.</span></span>

    ![Utlösaren jobbdefinitionen](./media/storsimple-data-manager-ui/run-now.png)

2. <span data-ttu-id="1d48a-173">Välj jobbdefinitionen som du vill köra.</span><span class="sxs-lookup"><span data-stu-id="1d48a-173">Choose the job definition that you want to run.</span></span> <span data-ttu-id="1d48a-174">Klicka på **körningsinställningar** att ändra alla inställningar som du kanske vill ändra för det här jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="1d48a-174">Click **Run settings** to modify any settings that you might want to change for this job run.</span></span>

    ![Kör jobbinställningar](./media/storsimple-data-manager-ui/run-settings.png)

3. <span data-ttu-id="1d48a-176">Klicka på **OK** och klicka sedan på **kör** att starta utskriften.</span><span class="sxs-lookup"><span data-stu-id="1d48a-176">Click **OK** and then click **Run** to launch your job.</span></span> <span data-ttu-id="1d48a-177">För att övervaka jobbet, gå till den **jobb** sidan i din StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="1d48a-177">To monitor this job, go to the **Jobs** page in your StorSimple Data Manager.</span></span>

    ![Lista över jobb och status](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. <span data-ttu-id="1d48a-179">Förutom övervakning i den **jobb** bladet du kan också lyssna på lagring kön där ett meddelande läggs varje gång en fil har flyttats från StorSimple till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="1d48a-179">In addition to monitoring in the **Jobs** blade, you can also listen on the storage queue where a message is added every time a file is moved from StorSimple to the storage account.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1d48a-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d48a-180">Next steps</span></span>

<span data-ttu-id="1d48a-181">[Använd .NET SDK för att starta jobb StorSimple Data Manager](storsimple-data-manager-dotnet-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="1d48a-181">[Use .NET SDK to launch StorSimple Data Manager jobs](storsimple-data-manager-dotnet-jobs.md).</span></span>