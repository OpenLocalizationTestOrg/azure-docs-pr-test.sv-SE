---
title: "Azure Data Lake-verktyg: Lokal U-SQL kör och lokala debug med Visual Studio Code | Microsoft Docs"
description: "Lär dig hur toouse Azure Data Lake-verktyg för Visual Studio Code toolocal kör och lokala felsöka."
Keywords: "VScode, Azure Data Lake-verktyg, lokal körning lokala felsökning lokala Debug Förhandsgranska lagringsfil överför toostorage sökväg"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: DJ
editor: jejiang
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: fb152f07fe8c4b03dde8fb8e62c7475eccda0578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="u-sql-local-run-and-local-debug-with-visual-studio-code"></a><span data-ttu-id="20b87-104">Lokal U-SQL kör och lokala debug med Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="20b87-104">U-SQL local run and local debug with Visual Studio Code</span></span>

## <a name="prerequisites"></a><span data-ttu-id="20b87-105">Krav</span><span class="sxs-lookup"><span data-stu-id="20b87-105">Prerequisites</span></span>
<span data-ttu-id="20b87-106">Kontrollera att du har följande krav är på plats innan du påbörjar de här procedurerna hello:</span><span class="sxs-lookup"><span data-stu-id="20b87-106">Make sure you have hello following prerequisites in place before you start these procedures:</span></span>
- <span data-ttu-id="20b87-107">Azure Data Lake-verktyg för Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="20b87-107">Azure Data Lake Tool for Visual Studio Code.</span></span> <span data-ttu-id="20b87-108">Instruktioner finns i [Använd Azure Data Lake-verktyg för Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="20b87-108">For instructions, see [Use Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>
- <span data-ttu-id="20b87-109">C# för Visual Studio Code (om du vill tooperform U-SQL lokalt debug).</span><span class="sxs-lookup"><span data-stu-id="20b87-109">C# for Visual Studio Code (if you want tooperform a U-SQL local debug).</span></span>

   ![Installera C# i Data Lake-verktyg för Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-install-ms-vscodecsharp.png)
   
   > [!NOTE]
   > <span data-ttu-id="20b87-111">Hej lokal U-SQL-körning och debug-funktioner för närvarande stöder endast Windows-användare.</span><span class="sxs-lookup"><span data-stu-id="20b87-111">hello U-SQL local run and debug features currently only support Windows users.</span></span> 


## <a name="set-up-hello-u-sql-local-run-environment"></a><span data-ttu-id="20b87-112">Konfigurera hello U-SQL lokalt kör miljö</span><span class="sxs-lookup"><span data-stu-id="20b87-112">Set up hello U-SQL local run environment</span></span>

1. <span data-ttu-id="20b87-113">Välj Ctrl + Skift + P tooopen hello kommandot paletten och ange **ADL: hämta LocalRun beroende** toodownload hello paket.</span><span class="sxs-lookup"><span data-stu-id="20b87-113">Select Ctrl+Shift+P tooopen hello command palette, and then enter **ADL: Download LocalRun Dependency** toodownload hello packages.</span></span>  

   ![Ladda ned hello ADL LocalRun beroende paket](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. <span data-ttu-id="20b87-115">Leta upp hello beroende paket från hello sökvägen som visas i hello **utdata** rutan, och sedan installera BuildTools och Win10SDK 10240.</span><span class="sxs-lookup"><span data-stu-id="20b87-115">Locate hello dependency packages from hello path shown in hello **Output** pane, and then install BuildTools and Win10SDK 10240.</span></span> <span data-ttu-id="20b87-116">Här är en exempelsökväg:</span><span class="sxs-lookup"><span data-stu-id="20b87-116">Here is an example path:</span></span>  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency
`  
  <span data-ttu-id="20b87-117">![Leta upp hello beroende paket](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)</span><span class="sxs-lookup"><span data-stu-id="20b87-117">![Locate hello dependency packages](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)</span></span>

   <span data-ttu-id="20b87-118">a.</span><span class="sxs-lookup"><span data-stu-id="20b87-118">a.</span></span> <span data-ttu-id="20b87-119">tooinstall BuildTools, följ instruktionerna i guiden hello.</span><span class="sxs-lookup"><span data-stu-id="20b87-119">tooinstall BuildTools, follow hello wizard instructions.</span></span>   

  ![Installera BuildTools](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   <span data-ttu-id="20b87-121">b.</span><span class="sxs-lookup"><span data-stu-id="20b87-121">b.</span></span> <span data-ttu-id="20b87-122">tooinstall Win10SDK 10240, följ instruktionerna i guiden hello.</span><span class="sxs-lookup"><span data-stu-id="20b87-122">tooinstall Win10SDK 10240, follow hello wizard instructions.</span></span>  

  ![Installera Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. <span data-ttu-id="20b87-124">Ställ in hello miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="20b87-124">Set up hello environment variable.</span></span> <span data-ttu-id="20b87-125">Ange hello **SCOPE_CPP_SDK** miljövariabeln:</span><span class="sxs-lookup"><span data-stu-id="20b87-125">Set hello **SCOPE_CPP_SDK** environment variable to:</span></span>  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency\CppSDK_3rdparty
`  
4. <span data-ttu-id="20b87-126">Starta om hello OS toomake att gälla hello miljövariabelinställningar.</span><span class="sxs-lookup"><span data-stu-id="20b87-126">Restart hello OS toomake sure that hello environment variable settings take effect.</span></span>  

   ![Kontrollera hello SCOPE_CPP_SDK miljövariabeln är installerad](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/ConfigScopeCppSDk.png)

## <a name="start-hello-local-run-service-and-submit-hello-u-sql-job-tooa-local-account"></a><span data-ttu-id="20b87-128">Starta hello lokala kör tjänsten och skicka hello U-SQL-jobbet tooa lokalt konto</span><span class="sxs-lookup"><span data-stu-id="20b87-128">Start hello local run service and submit hello U-SQL job tooa local account</span></span> 
<span data-ttu-id="20b87-129">Hello första gången användaren du är begärd toodownload hello ADL: hämta LocalRun beroende paket om de inte redan är installerat.</span><span class="sxs-lookup"><span data-stu-id="20b87-129">For hello first-time user, you are prompted toodownload hello ADL: Download LocalRun Dependency packages if they are not already installed.</span></span>
1. <span data-ttu-id="20b87-130">Välj Ctrl + Skift + P tooopen hello kommandot paletten och ange **ADL: starta tjänsten för lokala kör**.</span><span class="sxs-lookup"><span data-stu-id="20b87-130">Select Ctrl+Shift+P tooopen hello command palette, and then enter **ADL: Start Local Run Service**.</span></span>
2. <span data-ttu-id="20b87-131">Välj **acceptera** tooaccept hello licensvillkoren för hello första gången.</span><span class="sxs-lookup"><span data-stu-id="20b87-131">Select **Accept** tooaccept hello Microsoft Software License Terms for hello first time.</span></span> 

   ![Acceptera licensvillkoren för hello](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. <span data-ttu-id="20b87-133">hello cmd-konsolen öppnas.</span><span class="sxs-lookup"><span data-stu-id="20b87-133">hello cmd console opens.</span></span> <span data-ttu-id="20b87-134">För första gången en användare, behöver du tooenter **3**, och leta sedan upp hello lokal mappsökväg för dina data som indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="20b87-134">For first-time users, you need tooenter **3**, and then locate hello local folder path for your data input and output.</span></span> <span data-ttu-id="20b87-135">Du kan använda hello standardvärden för andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="20b87-135">For other options, you can use hello default values.</span></span> 

   ![Data Lake-verktyg för Visual Studio Code lokala kör cmd](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. <span data-ttu-id="20b87-137">Välj Ctrl + Skift + P tooopen hello kommandot paletten, ange **ADL: skicka jobbet**, och välj sedan **lokala** toosubmit hello jobbet tooyour lokalt konto.</span><span class="sxs-lookup"><span data-stu-id="20b87-137">Select Ctrl+Shift+P tooopen hello command palette, enter **ADL: Submit Job**, and then select **Local** toosubmit hello job tooyour local account.</span></span>

   ![Data Lake-verktyg för Visual Studio Code Välj lokala](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. <span data-ttu-id="20b87-139">Du kan visa hello skickas information när du skickar hello jobb.</span><span class="sxs-lookup"><span data-stu-id="20b87-139">After you submit hello job, you can view hello submission details.</span></span> <span data-ttu-id="20b87-140">tooview hello skickas information väljer **jobUrl** i hello **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="20b87-140">tooview hello submission details select **jobUrl** in hello **Output** window.</span></span> <span data-ttu-id="20b87-141">Du kan också visa hello skicka jobbstatus från hello cmd-konsolen.</span><span class="sxs-lookup"><span data-stu-id="20b87-141">You can also view hello job submission status from hello cmd console.</span></span> <span data-ttu-id="20b87-142">Ange **7** i hello cmd-konsolen om du vill tooknow mer Jobbdetaljer.</span><span class="sxs-lookup"><span data-stu-id="20b87-142">Enter **7** in hello cmd console if you want tooknow more job details.</span></span>

   <span data-ttu-id="20b87-143">![Data Lake-verktyg för Visual Studio Code lokal körning utdata](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Data Lake-verktyg för Visual Studio Code lokal körning cmd-status](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png)</span><span class="sxs-lookup"><span data-stu-id="20b87-143">![Data Lake Tools for Visual Studio Code local run output](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
![Data Lake Tools for Visual Studio Code local run cmd status](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png)</span></span> 


## <a name="start-a-local-debug-for-hello-u-sql-job"></a><span data-ttu-id="20b87-144">Starta en lokal debug för hello U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="20b87-144">Start a local debug for hello U-SQL job</span></span>  
<span data-ttu-id="20b87-145">Hello första gången användaren du är begärd toodownload hello ADL: hämta LocalRun beroende paket om de inte redan är installerat.</span><span class="sxs-lookup"><span data-stu-id="20b87-145">For hello first-time user, you are prompted toodownload hello ADL: Download LocalRun Dependency packages if they are not already installed.</span></span>
  
1. <span data-ttu-id="20b87-146">Välj Ctrl + Skift + P tooopen hello kommandot paletten och ange **ADL: starta tjänsten för lokala kör**.</span><span class="sxs-lookup"><span data-stu-id="20b87-146">Select Ctrl+Shift+P tooopen hello command palette, and then enter **ADL: Start Local Run Service**.</span></span> <span data-ttu-id="20b87-147">hello cmd-konsolen öppnas.</span><span class="sxs-lookup"><span data-stu-id="20b87-147">hello cmd console opens.</span></span> <span data-ttu-id="20b87-148">Kontrollera att hello **DataRoot** har angetts.</span><span class="sxs-lookup"><span data-stu-id="20b87-148">Make sure that hello **DataRoot** is set.</span></span>
3. <span data-ttu-id="20b87-149">Ange en brytpunkt i ditt C# bakomliggande kod.</span><span class="sxs-lookup"><span data-stu-id="20b87-149">Set a breakpoint in your C# code-behind.</span></span>
4. <span data-ttu-id="20b87-150">I hello Skriptredigerare, markerar du Ctrl + Skift + P tooopen hello kommandokonsolen och ange sedan **lokala felsöka** toostart lokala debug-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="20b87-150">Back in hello script editor, select Ctrl+Shift+P tooopen hello command console, and then enter **Local Debug** toostart your local debug service.</span></span>

![Data Lake-verktyg för Visual Studio Code lokala debug-resultat](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a><span data-ttu-id="20b87-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="20b87-152">Next steps</span></span>
- <span data-ttu-id="20b87-153">För att använda Azure Data Lake-verktyg för Visual Studio Code, se [Använd Azure Data Lake-verktyg för Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="20b87-153">For using Azure Data Lake Tools for Visual Studio Code, see [Use Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>
- <span data-ttu-id="20b87-154">Komma igång information om Data Lake Analytics finns [Självstudier: Kom igång med Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="20b87-154">For getting started information on Data Lake Analytics, see [Tutorial: Get started with Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span></span>
- <span data-ttu-id="20b87-155">Information om Data Lake-verktyg för Visual Studio finns [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="20b87-155">For information about Data Lake Tools for Visual Studio, see [Tutorial: Develop U-SQL scripts by using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
- <span data-ttu-id="20b87-156">Hello information om hur du utvecklar sammansättningar finns [utveckla U-SQL-sammansättningar för Azure Data Lake Analytics-jobb](data-lake-analytics-u-sql-develop-assemblies.md).</span><span class="sxs-lookup"><span data-stu-id="20b87-156">For hello information on developing assemblies, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md).</span></span>
