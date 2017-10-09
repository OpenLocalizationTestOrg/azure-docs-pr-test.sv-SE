---
title: "aaaUse .NET SDK för Microsoft Azure StorSimple Data Manager-jobb | Microsoft Docs"
description: "Lär dig hur toouse .NET SDK toolaunch StorSimple Data Manager jobb (privat förhandsvisning)"
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
ms.openlocfilehash: b07fe64369574c994fd28d42786aa02dca435ccc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-net-sdk-tooinitiate-data-transformation-private-preview"></a><span data-ttu-id="d1021-103">Använd hello .net SDK tooinitiate DTS (privat förhandsvisning)</span><span class="sxs-lookup"><span data-stu-id="d1021-103">Use hello .Net SDK tooinitiate data transformation (Private Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="d1021-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="d1021-104">Overview</span></span>

<span data-ttu-id="d1021-105">Den här artikeln förklarar hur du kan använda hello data transformation-funktion i hello StorSimple Data Manager-tjänsten tootransform StorSimple data på enheten.</span><span class="sxs-lookup"><span data-stu-id="d1021-105">This article explains how you can use hello data transformation feature within hello StorSimple Data Manager service tootransform StorSimple device data.</span></span> <span data-ttu-id="d1021-106">hello används transformerade data av andra Azure-tjänster i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="d1021-106">hello transformed data is then consumed by other Azure services in hello cloud.</span></span> <span data-ttu-id="d1021-107">hello artikeln har även en genomgång toohelp och skapa en exempel .NET konsolen programmet tooinitiate ett jobb för omvandling av data och spåra det för slutförande.</span><span class="sxs-lookup"><span data-stu-id="d1021-107">hello article also has a walkthrough toohelp create a sample .NET console application tooinitiate a data transformation job and then track it for completion.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1021-108">Krav</span><span class="sxs-lookup"><span data-stu-id="d1021-108">Prerequisites</span></span>

<span data-ttu-id="d1021-109">Innan du börjar bör du kontrollera att du har:</span><span class="sxs-lookup"><span data-stu-id="d1021-109">Before you begin, ensure that you have:</span></span>
*   <span data-ttu-id="d1021-110">Ett system med Visual Studio 2012, 2013 eller 2015 installerat 2017.</span><span class="sxs-lookup"><span data-stu-id="d1021-110">A system with Visual Studio 2012, 2013, 2015, or 2017 installed.</span></span>
*   <span data-ttu-id="d1021-111">Azure Powershell har installerats.</span><span class="sxs-lookup"><span data-stu-id="d1021-111">Azure Powershell installed.</span></span> <span data-ttu-id="d1021-112">[Hämta Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="d1021-112">[Download Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span></span>
*   <span data-ttu-id="d1021-113">Konfiguration av inställningar tooinitialize hello Data Transformation jobbet (instruktioner tooobtain inställningarna ingår).</span><span class="sxs-lookup"><span data-stu-id="d1021-113">Configuration settings tooinitialize hello Data Transformation job (instructions tooobtain these settings are included here).</span></span>
*   <span data-ttu-id="d1021-114">En jobbdefinition av som har konfigurerats på rätt sätt i en Hybrid Dataresurs i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d1021-114">A job definition that has been correctly configured in a Hybrid Data Resource within a Resource Group.</span></span>
*   <span data-ttu-id="d1021-115">Alla hello krävs DLL-filer.</span><span class="sxs-lookup"><span data-stu-id="d1021-115">All hello required dlls.</span></span> <span data-ttu-id="d1021-116">Ladda ned dessa DLL-filer från hello [GitHub-lagringsplatsen](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).</span><span class="sxs-lookup"><span data-stu-id="d1021-116">Download these dlls from hello [GitHub repository](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).</span></span>
*   <span data-ttu-id="d1021-117">`Get-ConfigurationParams.ps1`[skriptet](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) från hello github-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="d1021-117">`Get-ConfigurationParams.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) from hello github repository.</span></span>

## <a name="step-by-step"></a><span data-ttu-id="d1021-118">Steg för steg</span><span class="sxs-lookup"><span data-stu-id="d1021-118">Step-by-step</span></span>

<span data-ttu-id="d1021-119">Utföra hello följande steg toouse .NET toolaunch ett jobb för omvandling av data.</span><span class="sxs-lookup"><span data-stu-id="d1021-119">Perform hello following steps toouse .NET toolaunch a data transformation job.</span></span>

1. <span data-ttu-id="d1021-120">konfigurationsparametrar för tooretrieve hello hello gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="d1021-120">tooretrieve hello configuration parameters, do hello following steps:</span></span>
    1. <span data-ttu-id="d1021-121">Hämta hello `Get-ConfigurationParams.ps1` från hello github-lagringsplatsen skriptet i `C:\DataTransformation` plats.</span><span class="sxs-lookup"><span data-stu-id="d1021-121">Download hello `Get-ConfigurationParams.ps1` from hello github repository script in `C:\DataTransformation` location.</span></span>
    1. <span data-ttu-id="d1021-122">Kör hello `Get-ConfigurationParams.ps1` skriptet från hello github-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="d1021-122">Run hello `Get-ConfigurationParams.ps1` script from hello github repository.</span></span> <span data-ttu-id="d1021-123">Skriv följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="d1021-123">Type hello following command:</span></span>

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        <span data-ttu-id="d1021-124">Du kan skicka in värden för hello ActiveDirectoryKey och AppName.</span><span class="sxs-lookup"><span data-stu-id="d1021-124">You can pass in any values for hello ActiveDirectoryKey and AppName.</span></span>


2. <span data-ttu-id="d1021-125">Det här skriptet matar ut hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="d1021-125">This script outputs hello following values:</span></span>
    * <span data-ttu-id="d1021-126">Klient-ID</span><span class="sxs-lookup"><span data-stu-id="d1021-126">Client ID</span></span>
    * <span data-ttu-id="d1021-127">Klient-ID:t</span><span class="sxs-lookup"><span data-stu-id="d1021-127">Tenant ID</span></span>
    * <span data-ttu-id="d1021-128">Active Directory-nyckel (samma som hello något som anges ovan)</span><span class="sxs-lookup"><span data-stu-id="d1021-128">Active Directory key (same as hello one entered above)</span></span>
    * <span data-ttu-id="d1021-129">Prenumerations-ID:t</span><span class="sxs-lookup"><span data-stu-id="d1021-129">Subscription ID</span></span>

3. <span data-ttu-id="d1021-130">Med Visual Studio 2012, 2013 eller 2015, skapa ett C# .NET-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="d1021-130">Using Visual Studio 2012, 2013 or 2015, create a C# .NET console application.</span></span>

    1. <span data-ttu-id="d1021-131">Starta **Visual Studio 2012/2013/2015**.</span><span class="sxs-lookup"><span data-stu-id="d1021-131">Launch **Visual Studio 2012/2013/2015**.</span></span>
    1. <span data-ttu-id="d1021-132">Klicka på **filen**, peka för**ny**, och klicka på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="d1021-132">Click **File**, point too**New**, and click **Project**.</span></span>
    2. <span data-ttu-id="d1021-133">Expandera **Mallar** och välj **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="d1021-133">Expand **Templates**, and select **Visual C#**.</span></span>
    3. <span data-ttu-id="d1021-134">Välj **konsolprogram** hello listan över projekttyper på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="d1021-134">Select **Console Application** from hello list of project types on hello right.</span></span>
    4. <span data-ttu-id="d1021-135">Ange **DataTransformationApp** för hello **namn**.</span><span class="sxs-lookup"><span data-stu-id="d1021-135">Enter **DataTransformationApp** for hello **Name**.</span></span>
    5. <span data-ttu-id="d1021-136">Välj **C:\DataTransformation** för hello **plats**.</span><span class="sxs-lookup"><span data-stu-id="d1021-136">Select **C:\DataTransformation** for hello **Location**.</span></span>
    6. <span data-ttu-id="d1021-137">Klicka på **OK** toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="d1021-137">Click **OK** toocreate hello project.</span></span>

4.  <span data-ttu-id="d1021-138">Lägg till alla DLL: er finns i hello [DLL: er](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) mapp som **referenser** i hello-projekt som du skapade.</span><span class="sxs-lookup"><span data-stu-id="d1021-138">Now, add all DLLs present in hello [dlls](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) folder as **References** in hello project that you created.</span></span> <span data-ttu-id="d1021-139">toodownload hello dll-filer, hello följande:</span><span class="sxs-lookup"><span data-stu-id="d1021-139">toodownload hello dll files, do hello following:</span></span>

    1. <span data-ttu-id="d1021-140">I Visual Studio, gå för**Visa > Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="d1021-140">In Visual Studio, go too**View > Solution Explorer**.</span></span>
    1. <span data-ttu-id="d1021-141">Klicka på hello pilen toohello till vänster i Data Transformation App-projekt.</span><span class="sxs-lookup"><span data-stu-id="d1021-141">Click hello arrow toohello left of Data Transformation App project.</span></span> <span data-ttu-id="d1021-142">Klicka på **referenser** och högerklickar för**Lägg till referens**.</span><span class="sxs-lookup"><span data-stu-id="d1021-142">Click **References** and then right-click too**Add Reference**.</span></span>
    2. <span data-ttu-id="d1021-143">Bläddra toohello var hello paket mapp, markera alla hello DLL: er och på **Lägg till**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d1021-143">Browse toohello location of hello packages folder, select all hello DLLs and click **Add**, and then click **OK**.</span></span>

5. <span data-ttu-id="d1021-144">Lägg till följande hello **med** instruktioner toohello källfilen (Program.cs) i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="d1021-144">Add hello following **using** statements toohello source file (Program.cs) in hello project.</span></span>

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```


6. <span data-ttu-id="d1021-145">följande kod hello initierar hello data transformation jobbinstans.</span><span class="sxs-lookup"><span data-stu-id="d1021-145">hello following code initializes hello data transformation job instance.</span></span> <span data-ttu-id="d1021-146">Lägg till denna i hello **Main-metoden**.</span><span class="sxs-lookup"><span data-stu-id="d1021-146">Add this in hello **Main method**.</span></span> <span data-ttu-id="d1021-147">Ersätt hello värden konfigurationsparametrar som hämtats tidigare.</span><span class="sxs-lookup"><span data-stu-id="d1021-147">Replace hello values of configuration parameters as obtained earlier.</span></span> <span data-ttu-id="d1021-148">Anslut hello värdena för **resursgruppens namn** och **Hybrid Data resursnamnet**.</span><span class="sxs-lookup"><span data-stu-id="d1021-148">Plug in hello values of **Resource Group Name** and **Hybrid Data Resource name**.</span></span> <span data-ttu-id="d1021-149">Hej **resursgruppens namn** är hello en som är värd för hello Hybrid Dataresurs på vilka hello jobbdefinitionen konfigurerades.</span><span class="sxs-lookup"><span data-stu-id="d1021-149">hello **Resource Group Name** is hello one that hosts hello Hybrid Data Resource on which hello job definition was configured.</span></span>

    ```
    // Setup hello configuration parameters.
    var configParams = new ConfigurationParams
    {
        ClientId = "client-id",
        TenantId = "tenant-id",
        ActiveDirectoryKey = "active-directory-key",
        SubscriptionId = "subscription-id",
        ResourceGroupName = "resource-group-name",
        ResourceName = "resource-name"
    };

    // Initialize hello Data Transformation Job instance.
    DataTransformationJob dataTransformationJob = new DataTransformationJob(configParams);

    ```

7. <span data-ttu-id="d1021-150">Ange hello parametrar med vilka hello jobbdefinitionen måste toobe kör</span><span class="sxs-lookup"><span data-stu-id="d1021-150">Specify hello parameters with which hello job definition needs toobe run</span></span>

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);

    ```

    <span data-ttu-id="d1021-151">(ELLER)</span><span class="sxs-lookup"><span data-stu-id="d1021-151">(OR)</span></span>

    <span data-ttu-id="d1021-152">Om du vill toochange hello definition jobbparametrar under körning och Lägg sedan till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="d1021-152">If you want toochange hello job definition parameters during run time, then add hello following code:</span></span>

    ```
    string jobDefinitionName = "job-definition-name";
    // Must start with a '\'
    var rootDirectories = new List<string> {@"\root"};

    // Name of hello volume on hello StorSimple device.
    var volumeNames = new List<string> {"volume-name"};

    var dataTransformationInput = new DataTransformationInput
    {
        // If you require hello latest existing backup toobe picked else use TakeNow tootrigger a new backup.
        BackupChoice = BackupChoice.UseExistingLatest.ToString(),
        // Name of hello StorSimple device.
        DeviceName = "device-name",
        // Name of hello container in Azure storage where hello files will be placed after execution.
        ContainerName = "container-name",
        // File name filter (search pattern) toobe applied on files under hello root directory. * - Match all files.
        FileNameFilter = "*",
        // List of root directories.
        RootDirectories = rootDirectories,
        // Name of hello volume on StorSimple device on which hello relevant data is present. 
        VolumeNames = volumeNames
    };
    
    ```

8. <span data-ttu-id="d1021-153">Lägg till följande kod tootrigger ett jobb för omvandling av data på hello jobbdefinitionen hello efter hello initiering.</span><span class="sxs-lookup"><span data-stu-id="d1021-153">After hello initialization, add hello following code tootrigger a data transformation job on hello job definition.</span></span> <span data-ttu-id="d1021-154">Anslut hello lämpliga **Definition jobbnamn**.</span><span class="sxs-lookup"><span data-stu-id="d1021-154">Plug in hello appropriate **Job Definition Name**.</span></span>

    ```
    // Trigger a job, retrieve hello jobId and hello retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);

    ```

9. <span data-ttu-id="d1021-155">Det här jobbet överför hello matchade filer finns under hello rotkatalogen på hello StorSimple toohello angivna volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="d1021-155">This job uploads hello matched files present under hello root directory on hello StorSimple volume toohello specified container.</span></span> <span data-ttu-id="d1021-156">När en fil har överförts visas ett meddelande har släppts i hello kön (i hello samma lagringskonto som hello behållare) med hello samma namn som hello jobbdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="d1021-156">When a file is uploaded, a message is dropped in hello queue (in hello same storage account as hello container) with hello same name as hello job definition.</span></span> <span data-ttu-id="d1021-157">Det här meddelandet kan användas som en utlösare tooinitiate ytterligare bearbetning av hello-filen.</span><span class="sxs-lookup"><span data-stu-id="d1021-157">This message can be used as a trigger tooinitiate any further processing of hello file.</span></span>

10. <span data-ttu-id="d1021-158">Lägg till hello följande kod tootrack hello jobb för slutförande när hello jobbet har utlösts.</span><span class="sxs-lookup"><span data-stu-id="d1021-158">Once hello job has been triggered, add hello following code tootrack hello job for completion.</span></span>

    ```
    Job jobDetails = null;

    // Poll hello job.
    do
    {
        jobDetails = dataTransformationJob.GetJob(jobDefinitionName, jobId);

        // Wait before polling for hello status again.
        Thread.Sleep(TimeSpan.FromSeconds(retryAfter));

    } while (jobDetails.Status == JobStatus.InProgress);

    // Completion status of hello job.
    Console.WriteLine("JobStatus: {0}", jobDetails.Status);
    
    // toohold hello console before exiting.
    Console.Read();

    ```


## <a name="next-steps"></a><span data-ttu-id="d1021-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d1021-159">Next steps</span></span>

<span data-ttu-id="d1021-160">[Använda StorSimple Data-hanterarens användargränssnitt tootransform dina data](storsimple-data-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="d1021-160">[Use StorSimple Data Manager UI tootransform your data](storsimple-data-manager-ui.md).</span></span>
