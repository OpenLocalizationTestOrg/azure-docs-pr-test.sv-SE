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
# <a name="u-sql-local-run-and-local-debug-with-visual-studio-code"></a>Lokal U-SQL kör och lokala debug med Visual Studio Code

## <a name="prerequisites"></a>Krav
Kontrollera att du har följande krav är på plats innan du påbörjar de här procedurerna hello:
- Azure Data Lake-verktyg för Visual Studio-koden. Instruktioner finns i [Använd Azure Data Lake-verktyg för Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).
- C# för Visual Studio Code (om du vill tooperform U-SQL lokalt debug).

   ![Installera C# i Data Lake-verktyg för Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-install-ms-vscodecsharp.png)
   
   > [!NOTE]
   > Hej lokal U-SQL-körning och debug-funktioner för närvarande stöder endast Windows-användare. 


## <a name="set-up-hello-u-sql-local-run-environment"></a>Konfigurera hello U-SQL lokalt kör miljö

1. Välj Ctrl + Skift + P tooopen hello kommandot paletten och ange **ADL: hämta LocalRun beroende** toodownload hello paket.  

   ![Ladda ned hello ADL LocalRun beroende paket](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. Leta upp hello beroende paket från hello sökvägen som visas i hello **utdata** rutan, och sedan installera BuildTools och Win10SDK 10240. Här är en exempelsökväg:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency
`  
  ![Leta upp hello beroende paket](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   a. tooinstall BuildTools, följ instruktionerna i guiden hello.   

  ![Installera BuildTools](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   b. tooinstall Win10SDK 10240, följ instruktionerna i guiden hello.  

  ![Installera Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Ställ in hello miljövariabeln. Ange hello **SCOPE_CPP_SDK** miljövariabeln:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency\CppSDK_3rdparty
`  
4. Starta om hello OS toomake att gälla hello miljövariabelinställningar.  

   ![Kontrollera hello SCOPE_CPP_SDK miljövariabeln är installerad](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/ConfigScopeCppSDk.png)

## <a name="start-hello-local-run-service-and-submit-hello-u-sql-job-tooa-local-account"></a>Starta hello lokala kör tjänsten och skicka hello U-SQL-jobbet tooa lokalt konto 
Hello första gången användaren du är begärd toodownload hello ADL: hämta LocalRun beroende paket om de inte redan är installerat.
1. Välj Ctrl + Skift + P tooopen hello kommandot paletten och ange **ADL: starta tjänsten för lokala kör**.
2. Välj **acceptera** tooaccept hello licensvillkoren för hello första gången. 

   ![Acceptera licensvillkoren för hello](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. hello cmd-konsolen öppnas. För första gången en användare, behöver du tooenter **3**, och leta sedan upp hello lokal mappsökväg för dina data som indata och utdata. Du kan använda hello standardvärden för andra alternativ. 

   ![Data Lake-verktyg för Visual Studio Code lokala kör cmd](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Välj Ctrl + Skift + P tooopen hello kommandot paletten, ange **ADL: skicka jobbet**, och välj sedan **lokala** toosubmit hello jobbet tooyour lokalt konto.

   ![Data Lake-verktyg för Visual Studio Code Välj lokala](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. Du kan visa hello skickas information när du skickar hello jobb. tooview hello skickas information väljer **jobUrl** i hello **utdata** fönster. Du kan också visa hello skicka jobbstatus från hello cmd-konsolen. Ange **7** i hello cmd-konsolen om du vill tooknow mer Jobbdetaljer.

   ![Data Lake-verktyg för Visual Studio Code lokal körning utdata](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Data Lake-verktyg för Visual Studio Code lokal körning cmd-status](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-hello-u-sql-job"></a>Starta en lokal debug för hello U-SQL-jobb  
Hello första gången användaren du är begärd toodownload hello ADL: hämta LocalRun beroende paket om de inte redan är installerat.
  
1. Välj Ctrl + Skift + P tooopen hello kommandot paletten och ange **ADL: starta tjänsten för lokala kör**. hello cmd-konsolen öppnas. Kontrollera att hello **DataRoot** har angetts.
3. Ange en brytpunkt i ditt C# bakomliggande kod.
4. I hello Skriptredigerare, markerar du Ctrl + Skift + P tooopen hello kommandokonsolen och ange sedan **lokala felsöka** toostart lokala debug-tjänsten.

![Data Lake-verktyg för Visual Studio Code lokala debug-resultat](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Nästa steg
- För att använda Azure Data Lake-verktyg för Visual Studio Code, se [Använd Azure Data Lake-verktyg för Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).
- Komma igång information om Data Lake Analytics finns [Självstudier: Kom igång med Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Information om Data Lake-verktyg för Visual Studio finns [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Hello information om hur du utvecklar sammansättningar finns [utveckla U-SQL-sammansättningar för Azure Data Lake Analytics-jobb](data-lake-analytics-u-sql-develop-assemblies.md).
