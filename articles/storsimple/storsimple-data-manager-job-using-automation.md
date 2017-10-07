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
# <a name="use-azure-automation-tootrigger-a-job-private-preview"></a>Använd Azure Automation tootrigger ett jobb (privat förhandsvisning)

Den här artikeln beskrivs hur toouse Azure Automation tootrigger ett StorSimple Data Manager-jobb.

## <a name="prerequisites"></a>Krav

Innan du börjar bör du kontrollera att du har:

*   Azure Powershell har installerats. [Hämta Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Konfiguration av inställningar tooinitialize hello Data Transformation jobbet (instruktioner tooobtain inställningarna ingår).
*   En jobbdefinition av som har konfigurerats på rätt sätt i en Hybrid Dataresurs i en resursgrupp.
*   Hämta `DataTransformationApp.zip` [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip) filen från hello github-lagringsplatsen.
*   Hämta `Get-ConfigurationParams.ps1` [skriptet](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1) från hello github-lagringsplatsen.
*   Hämta `Trigger-DataTransformation-Job.ps1` [skriptet](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1) från hello github-lagringsplatsen.

## <a name="step-by-step"></a>Steg för steg

### <a name="get-azure-active-directory-permissions-for-hello-automation-job-toorun-hello-job-definition"></a>Hämta Azure Active Directory-behörigheter för hello automation jobbet toorun hello jobbdefinitionen

1. tooretrieve hello konfigurationsparametrar för Active Directory, hello följande steg:

    1. Öppna Windows PowerShell i din lokala dator. Se till att [Azure PowerShell](https://azure.microsoft.com/downloads/) är installerad.
    1. Kör hello `Get-ConfigurationParams.ps1` skript (i hello-mappen som du hämtade ovan). Skriv följande kommando i hello PowerShell-fönster hello:

        ```
        .\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```

        Hej ActiveDirectoryKey är ett lösenord som du använder senare. Ange ett lösenord som du väljer. Programnamn kan vara en sträng.

2. Det här skriptet matar ut hello följande värden som ska användas när utlösa hello automation-runbook. Anteckna dessa värden.

    - Klient-ID
    - Klient-ID:t
    - Active Directory-nyckel (samma som hello något som anges ovan)
    - Prenumerations-ID:t

### <a name="set-up-hello-automation-account"></a>Ställ in hello Automation-konto

1. Logga in tooAzure och öppna ditt Automation-konto.
2. Klicka på **tillgångar** panelen tooopen hello lista över tillgångar.
3. Klicka på **moduler** panelen tooopen hello listan över moduler.
4. Klicka på **+ Lägg till en modul** knappen och hello Lägg till modulen bladet har startats.

    ![Inställningarna för Automation-konto](./media/storsimple-data-manager-job-using-automation/add-module1m.png)

5. När du har valt hello `DataTransformationApp.zip` filen från den lokala datorn, klickar på **OK** tooimport hello modulen.

   När Azure Automation importerar en modul tooyour konto, extraherar metadata om hello modul. Den här åtgärden kan ta några minuter.

   ![Inställningarna för Automation-konto](./media/storsimple-data-manager-job-using-automation/add-module2m.png)

   

6. Du får ett meddelande att hello modulen distribueras och ett annat meddelande när hello processen har slutförts.  Du kan också kontrollera hello status **moduler** panelen.

### <a name="tooimport-hello-runbook-that-triggers-hello-job-definition"></a>tooimport hello runbook som utlöser hello jobbdefinitionen

1. Hello Azure-portalen, öppna ditt Automation-konto.
2. Klicka på **Runbooks** panelen tooopen hello lista över runbooks.
3. Klicka på **+ Lägg till en runbook** och sedan **importera en befintlig runbook**.

   ![Importera en befintlig runbook](./media/storsimple-data-manager-job-using-automation/import-a-runbook.png)

4. Klicka på **Runbook-filen** och välj hello filen tooimport `Trigger-DataTransformation-Job.ps1`.
5. Klicka på **skapa** tooimport hello runbook. hello ny runbook visas i hello runbooks för hello Automation-konto.
7. Klicka på **utlösaren DataTransformation jobbet** runbook och klicka sedan på **redigera**.
8. Klicka på **publicera** och sedan **Ja** när du uppmanas att bekräfta.


### <a name="toorun-hello-runbook"></a>toorun hello runbook:
1. Hello Azure-portalen, öppna ditt Automation-konto.
2. Klicka på hello **Runbooks** panelen tooopen hello lista över runbooks.
3. Klicka på **utlösaren DataTransformation jobbet**.
4. Klicka på **starta** toostart hello runbook.

   ![Starta runbooken](./media/storsimple-data-manager-job-using-automation/run-runbook1m.png)

5. I hello **startar runbook** bladet ange alla hello-parametrar. Klicka på **OK** toosubmit hello Data Transformation jobb.

   ![Starta runbooken](./media/storsimple-data-manager-job-using-automation/run-runbook2m.png)


## <a name="next-steps"></a>Nästa steg

[Använda StorSimple Data-hanterarens användargränssnitt tootransform dina data](storsimple-data-manager-ui.md).
