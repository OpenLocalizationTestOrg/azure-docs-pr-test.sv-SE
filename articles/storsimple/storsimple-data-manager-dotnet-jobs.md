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
# <a name="use-hello-net-sdk-tooinitiate-data-transformation-private-preview"></a>Använd hello .net SDK tooinitiate DTS (privat förhandsvisning)

## <a name="overview"></a>Översikt

Den här artikeln förklarar hur du kan använda hello data transformation-funktion i hello StorSimple Data Manager-tjänsten tootransform StorSimple data på enheten. hello används transformerade data av andra Azure-tjänster i hello molnet. hello artikeln har även en genomgång toohelp och skapa en exempel .NET konsolen programmet tooinitiate ett jobb för omvandling av data och spåra det för slutförande.

## <a name="prerequisites"></a>Krav

Innan du börjar bör du kontrollera att du har:
*   Ett system med Visual Studio 2012, 2013 eller 2015 installerat 2017.
*   Azure Powershell har installerats. [Hämta Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Konfiguration av inställningar tooinitialize hello Data Transformation jobbet (instruktioner tooobtain inställningarna ingår).
*   En jobbdefinition av som har konfigurerats på rätt sätt i en Hybrid Dataresurs i en resursgrupp.
*   Alla hello krävs DLL-filer. Ladda ned dessa DLL-filer från hello [GitHub-lagringsplatsen](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).
*   `Get-ConfigurationParams.ps1`[skriptet](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) från hello github-lagringsplatsen.

## <a name="step-by-step"></a>Steg för steg

Utföra hello följande steg toouse .NET toolaunch ett jobb för omvandling av data.

1. konfigurationsparametrar för tooretrieve hello hello gör du följande steg:
    1. Hämta hello `Get-ConfigurationParams.ps1` från hello github-lagringsplatsen skriptet i `C:\DataTransformation` plats.
    1. Kör hello `Get-ConfigurationParams.ps1` skriptet från hello github-lagringsplatsen. Skriv följande kommando hello:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        Du kan skicka in värden för hello ActiveDirectoryKey och AppName.


2. Det här skriptet matar ut hello följande värden:
    * Klient-ID
    * Klient-ID:t
    * Active Directory-nyckel (samma som hello något som anges ovan)
    * Prenumerations-ID:t

3. Med Visual Studio 2012, 2013 eller 2015, skapa ett C# .NET-konsolprogram.

    1. Starta **Visual Studio 2012/2013/2015**.
    1. Klicka på **filen**, peka för**ny**, och klicka på **projekt**.
    2. Expandera **Mallar** och välj **Visual C#**.
    3. Välj **konsolprogram** hello listan över projekttyper på hello rätt.
    4. Ange **DataTransformationApp** för hello **namn**.
    5. Välj **C:\DataTransformation** för hello **plats**.
    6. Klicka på **OK** toocreate hello projektet.

4.  Lägg till alla DLL: er finns i hello [DLL: er](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) mapp som **referenser** i hello-projekt som du skapade. toodownload hello dll-filer, hello följande:

    1. I Visual Studio, gå för**Visa > Solution Explorer**.
    1. Klicka på hello pilen toohello till vänster i Data Transformation App-projekt. Klicka på **referenser** och högerklickar för**Lägg till referens**.
    2. Bläddra toohello var hello paket mapp, markera alla hello DLL: er och på **Lägg till**, och klicka sedan på **OK**.

5. Lägg till följande hello **med** instruktioner toohello källfilen (Program.cs) i hello-projekt.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```


6. följande kod hello initierar hello data transformation jobbinstans. Lägg till denna i hello **Main-metoden**. Ersätt hello värden konfigurationsparametrar som hämtats tidigare. Anslut hello värdena för **resursgruppens namn** och **Hybrid Data resursnamnet**. Hej **resursgruppens namn** är hello en som är värd för hello Hybrid Dataresurs på vilka hello jobbdefinitionen konfigurerades.

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

7. Ange hello parametrar med vilka hello jobbdefinitionen måste toobe kör

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);

    ```

    (ELLER)

    Om du vill toochange hello definition jobbparametrar under körning och Lägg sedan till följande kod hello:

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

8. Lägg till följande kod tootrigger ett jobb för omvandling av data på hello jobbdefinitionen hello efter hello initiering. Anslut hello lämpliga **Definition jobbnamn**.

    ```
    // Trigger a job, retrieve hello jobId and hello retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);

    ```

9. Det här jobbet överför hello matchade filer finns under hello rotkatalogen på hello StorSimple toohello angivna volymbehållare. När en fil har överförts visas ett meddelande har släppts i hello kön (i hello samma lagringskonto som hello behållare) med hello samma namn som hello jobbdefinitionen. Det här meddelandet kan användas som en utlösare tooinitiate ytterligare bearbetning av hello-filen.

10. Lägg till hello följande kod tootrack hello jobb för slutförande när hello jobbet har utlösts.

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


## <a name="next-steps"></a>Nästa steg

[Använda StorSimple Data-hanterarens användargränssnitt tootransform dina data](storsimple-data-manager-ui.md).
