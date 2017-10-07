---
title: aaaUse Azure Automation tootrigger ett jobb | Microsoft Docs
description: "Lär dig hur toouse Azure Automation för att utlösa StorSimple Data Manager-jobb (privat förhandsvisning)"
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
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 0d9d6e5e6d52ed27ca759ed7e949b5f5dfd319c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-automation-tootrigger-a-job-private-preview"></a><span data-ttu-id="f579e-103">Använd Azure Automation tootrigger ett jobb (privat förhandsvisning)</span><span class="sxs-lookup"><span data-stu-id="f579e-103">Use Azure Automation tootrigger a job (Private Preview)</span></span>

<span data-ttu-id="f579e-104">Den här artikeln beskrivs hur toouse Azure Automation tootrigger ett StorSimple Data Manager-jobb.</span><span class="sxs-lookup"><span data-stu-id="f579e-104">This articles describes how toouse Azure Automation tootrigger a StorSimple Data Manager job.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f579e-105">Krav</span><span class="sxs-lookup"><span data-stu-id="f579e-105">Prerequisites</span></span>

<span data-ttu-id="f579e-106">Innan du börjar bör du kontrollera att du har:</span><span class="sxs-lookup"><span data-stu-id="f579e-106">Before you begin, ensure that you have:</span></span>

*   <span data-ttu-id="f579e-107">Azure Powershell har installerats.</span><span class="sxs-lookup"><span data-stu-id="f579e-107">Azure Powershell installed.</span></span> <span data-ttu-id="f579e-108">[Hämta Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="f579e-108">[Download Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span></span>
*   <span data-ttu-id="f579e-109">Konfiguration av inställningar tooinitialize hello Data Transformation jobbet (instruktioner tooobtain inställningarna ingår).</span><span class="sxs-lookup"><span data-stu-id="f579e-109">Configuration settings tooinitialize hello Data Transformation job (instructions tooobtain these settings are included here).</span></span>
*   <span data-ttu-id="f579e-110">En jobbdefinition av som har konfigurerats på rätt sätt i en Hybrid Dataresurs i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f579e-110">A job definition that has been correctly configured in a Hybrid Data Resource within a resource group.</span></span>
*   <span data-ttu-id="f579e-111">Hämta `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) filen från hello github-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="f579e-111">Download `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) file from hello github repository.</span></span>
*   <span data-ttu-id="f579e-112">Hämta `Get-ConfigurationParams.ps1` [skriptet](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) från hello github-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="f579e-112">Download `Get-ConfigurationParams.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) from hello github repository.</span></span>
*   <span data-ttu-id="f579e-113">Hämta `Trigger-DataTransformation-Job.ps1` [skriptet](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) från hello github-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="f579e-113">Download `Trigger-DataTransformation-Job.ps1` [script](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) from hello github repository.</span></span>

## <a name="step-by-step"></a><span data-ttu-id="f579e-114">Steg för steg</span><span class="sxs-lookup"><span data-stu-id="f579e-114">Step-by-step</span></span>

### <a name="get-azure-active-directory-permissions-for-hello-automation-job-toorun-hello-job-definition"></a><span data-ttu-id="f579e-115">Hämta Azure Active Directory-behörigheter för hello automation jobbet toorun hello jobbdefinitionen</span><span class="sxs-lookup"><span data-stu-id="f579e-115">Get Azure Active Directory permissions for hello automation job toorun hello job definition</span></span>

1. <span data-ttu-id="f579e-116">tooretrieve hello konfigurationsparametrar för Active Directory, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f579e-116">tooretrieve hello configuration parameters for Active Directory, do hello following steps:</span></span>

    1. <span data-ttu-id="f579e-117">Öppna Windows PowerShell i din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="f579e-117">Open Windows PowerShell in your local machine.</span></span> <span data-ttu-id="f579e-118">Se till att [Azure PowerShell](https://azure.microsoft.com/downloads/) är installerad.</span><span class="sxs-lookup"><span data-stu-id="f579e-118">Ensure that [Azure PowerShell](https://azure.microsoft.com/downloads/) is installed.</span></span>
    1. <span data-ttu-id="f579e-119">Kör hello `Get-ConfigurationParams.ps1` skript (i hello-mappen som du hämtade ovan).</span><span class="sxs-lookup"><span data-stu-id="f579e-119">Run hello `Get-ConfigurationParams.ps1` script (in hello folder you downloaded above).</span></span> <span data-ttu-id="f579e-120">Skriv följande kommando i hello PowerShell-fönster hello:</span><span class="sxs-lookup"><span data-stu-id="f579e-120">Type hello following command in hello PowerShell window:</span></span>

        ```
        .\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```

        <span data-ttu-id="f579e-121">Hej ActiveDirectoryKey är ett lösenord som du använder senare.</span><span class="sxs-lookup"><span data-stu-id="f579e-121">hello ActiveDirectoryKey is a password that you use later.</span></span> <span data-ttu-id="f579e-122">Ange ett lösenord som du väljer.</span><span class="sxs-lookup"><span data-stu-id="f579e-122">Enter a password of your choice.</span></span> <span data-ttu-id="f579e-123">Programnamn kan vara en sträng.</span><span class="sxs-lookup"><span data-stu-id="f579e-123">AppName can be any string.</span></span>

2. <span data-ttu-id="f579e-124">Det här skriptet matar ut hello följande värden som ska användas när utlösa hello automation-runbook.</span><span class="sxs-lookup"><span data-stu-id="f579e-124">This script outputs hello following values that should be used while triggering hello automation runbook.</span></span> <span data-ttu-id="f579e-125">Anteckna dessa värden.</span><span class="sxs-lookup"><span data-stu-id="f579e-125">Make a note of these values.</span></span>

    - <span data-ttu-id="f579e-126">Klient-ID</span><span class="sxs-lookup"><span data-stu-id="f579e-126">Client ID</span></span>
    - <span data-ttu-id="f579e-127">Klient-ID:t</span><span class="sxs-lookup"><span data-stu-id="f579e-127">Tenant ID</span></span>
    - <span data-ttu-id="f579e-128">Active Directory-nyckel (samma som hello något som anges ovan)</span><span class="sxs-lookup"><span data-stu-id="f579e-128">Active Directory key (same as hello one entered above)</span></span>
    - <span data-ttu-id="f579e-129">Prenumerations-ID:t</span><span class="sxs-lookup"><span data-stu-id="f579e-129">Subscription ID</span></span>

### <a name="set-up-hello-automation-account"></a><span data-ttu-id="f579e-130">Ställ in hello Automation-konto</span><span class="sxs-lookup"><span data-stu-id="f579e-130">Set up hello Automation Account</span></span>

1. <span data-ttu-id="f579e-131">Logga in tooAzure och öppna ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="f579e-131">Log on tooAzure and open your Automation account.</span></span>
2. <span data-ttu-id="f579e-132">Klicka på **tillgångar** panelen tooopen hello lista över tillgångar.</span><span class="sxs-lookup"><span data-stu-id="f579e-132">Click **Assets** tile tooopen hello list of assets.</span></span>
3. <span data-ttu-id="f579e-133">Klicka på **moduler** panelen tooopen hello listan över moduler.</span><span class="sxs-lookup"><span data-stu-id="f579e-133">Click **Modules** tile tooopen hello list of modules.</span></span>
4. <span data-ttu-id="f579e-134">Klicka på **+ Lägg till en modul** knappen och hello Lägg till modulen bladet har startats.</span><span class="sxs-lookup"><span data-stu-id="f579e-134">Click **+ Add a module** button and hello Add module blade is launched.</span></span>

    ![Inställningarna för Automation-konto](./media/storsimple-data-manager-job-using-automation/add-module1m.png)

5. <span data-ttu-id="f579e-136">När du har valt hello `DataTransformationApp.zip` filen från den lokala datorn, klickar på **OK** tooimport hello modulen.</span><span class="sxs-lookup"><span data-stu-id="f579e-136">After you have selected hello `DataTransformationApp.zip` file from your local computer, click **OK** tooimport hello module.</span></span>

   <span data-ttu-id="f579e-137">När Azure Automation importerar en modul tooyour konto, extraherar metadata om hello modul.</span><span class="sxs-lookup"><span data-stu-id="f579e-137">When Azure Automation imports a module tooyour account, it extracts metadata about hello module.</span></span> <span data-ttu-id="f579e-138">Den här åtgärden kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="f579e-138">This operation may take a couple of minutes.</span></span>

   ![Inställningarna för Automation-konto](./media/storsimple-data-manager-job-using-automation/add-module2m.png)

   

6. <span data-ttu-id="f579e-140">Du får ett meddelande att hello modulen distribueras och ett annat meddelande när hello processen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="f579e-140">You receive a notification that hello module is being deployed and another notification when hello process is complete.</span></span>  <span data-ttu-id="f579e-141">Du kan också kontrollera hello status **moduler** panelen.</span><span class="sxs-lookup"><span data-stu-id="f579e-141">You can also check hello status in **Modules** tile.</span></span>

### <a name="tooimport-hello-runbook-that-triggers-hello-job-definition"></a><span data-ttu-id="f579e-142">tooimport hello runbook som utlöser hello jobbdefinitionen</span><span class="sxs-lookup"><span data-stu-id="f579e-142">tooimport hello runbook that triggers hello job definition</span></span>

1. <span data-ttu-id="f579e-143">Hello Azure-portalen, öppna ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="f579e-143">In hello Azure portal, open your Automation account.</span></span>
2. <span data-ttu-id="f579e-144">Klicka på **Runbooks** panelen tooopen hello lista över runbooks.</span><span class="sxs-lookup"><span data-stu-id="f579e-144">Click **Runbooks** tile tooopen hello list of runbooks.</span></span>
3. <span data-ttu-id="f579e-145">Klicka på **+ Lägg till en runbook** och sedan **importera en befintlig runbook**.</span><span class="sxs-lookup"><span data-stu-id="f579e-145">Click **+ Add a runbook** and then **Import an existing runbook**.</span></span>

   ![Importera en befintlig runbook](./media/storsimple-data-manager-job-using-automation/import-a-runbook.png)

4. <span data-ttu-id="f579e-147">Klicka på **Runbook-filen** och välj hello filen tooimport `Trigger-DataTransformation-Job.ps1`.</span><span class="sxs-lookup"><span data-stu-id="f579e-147">Click **Runbook file** and select hello file tooimport `Trigger-DataTransformation-Job.ps1`.</span></span>
5. <span data-ttu-id="f579e-148">Klicka på **skapa** tooimport hello runbook.</span><span class="sxs-lookup"><span data-stu-id="f579e-148">Click **Create** tooimport hello runbook.</span></span> <span data-ttu-id="f579e-149">hello ny runbook visas i hello runbooks för hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="f579e-149">hello new runbook appears in hello list of runbooks for hello Automation account.</span></span>
7. <span data-ttu-id="f579e-150">Klicka på **utlösaren DataTransformation jobbet** runbook och klicka sedan på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="f579e-150">Click **Trigger-DataTransformation-Job** runbook and then click **Edit**.</span></span>
8. <span data-ttu-id="f579e-151">Klicka på **publicera** och sedan **Ja** när du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="f579e-151">Click **Publish** and then **Yes** when prompted for confirmation.</span></span>


### <a name="toorun-hello-runbook"></a><span data-ttu-id="f579e-152">toorun hello runbook:</span><span class="sxs-lookup"><span data-stu-id="f579e-152">toorun hello runbook:</span></span>
1. <span data-ttu-id="f579e-153">Hello Azure-portalen, öppna ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="f579e-153">In hello Azure portal, open your Automation account.</span></span>
2. <span data-ttu-id="f579e-154">Klicka på hello **Runbooks** panelen tooopen hello lista över runbooks.</span><span class="sxs-lookup"><span data-stu-id="f579e-154">Click hello **Runbooks** tile tooopen hello list of runbooks.</span></span>
3. <span data-ttu-id="f579e-155">Klicka på **utlösaren DataTransformation jobbet**.</span><span class="sxs-lookup"><span data-stu-id="f579e-155">Click **Trigger-DataTransformation-Job**.</span></span>
4. <span data-ttu-id="f579e-156">Klicka på **starta** toostart hello runbook.</span><span class="sxs-lookup"><span data-stu-id="f579e-156">Click **Start** toostart hello runbook.</span></span>

   ![Starta runbooken](./media/storsimple-data-manager-job-using-automation/run-runbook1m.png)

5. <span data-ttu-id="f579e-158">I hello **startar runbook** bladet ange alla hello-parametrar.</span><span class="sxs-lookup"><span data-stu-id="f579e-158">In hello **Start runbook** blade, enter all hello parameters.</span></span> <span data-ttu-id="f579e-159">Klicka på **OK** toosubmit hello Data Transformation jobb.</span><span class="sxs-lookup"><span data-stu-id="f579e-159">Click **OK** toosubmit hello Data Transformation job.</span></span>

   ![Starta runbooken](./media/storsimple-data-manager-job-using-automation/run-runbook2m.png)


## <a name="next-steps"></a><span data-ttu-id="f579e-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f579e-161">Next steps</span></span>

<span data-ttu-id="f579e-162">[Använda StorSimple Data-hanterarens användargränssnitt tootransform dina data](storsimple-data-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="f579e-162">[Use StorSimple Data Manager UI tootransform your data](storsimple-data-manager-ui.md).</span></span>
