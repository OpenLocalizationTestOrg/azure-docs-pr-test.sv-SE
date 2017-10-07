---
title: "aaaAzure Automation Källkontrollintegrering med GitHub Enterprise | Microsoft Docs"
description: "Beskriver hello information om hur tooconfigure integrering med GitHub Enterprise för källkontroll Automation-runbooks."
services: automation
documentationCenter: 
authors: mgoedtel
manager: jwhit
editor: 
ms.assetid: e01d817c-7d38-421c-adf5-647a4b526eb4
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: magoedte
ms.openlocfilehash: 915d36ccabb72fdee1dba663049a0b331249cd73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-github-enterprise"></a>Azure Automation-scenario – Automation källkontrollintegrering med GitHub-Enterprise

Automation stöder för närvarande källkontrollintegrering, vilket gör att du tooassociate runbooks i ditt Automation-konto tooa GitHub för källkontroll.  Dock kunder som har distribuerat [GitHub Enterprise](https://enterprise.github.com/home) toosupport sina DevOps-metoder, även vill toouse den toomanage hello livscykeln för runbooks som utvecklats tooautomate affärsprocesser och servicehantering åtgärder.  

I det här scenariot har du en Windows-dator i ditt datacenter som konfigurerats som en Hybrid Runbook Worker med hello Azure Resource Manager-moduler och Git-verktygen som installeras.  hello Hybrid worker datorn har en klon av hello lokal Git-lagringsplats.  När hello runbook körs på hello hybrid worker hello Git directory synkroniseras och hello runbook-filens innehåll som har importerats till hello Automation-konto.

Den här artikeln beskriver hur tooset in den här konfigurationen i Azure Automation-miljö. Vi börjar med att konfigurera Automation med hello säkerhetsreferenser runbooks krävs toosupport för det här scenariot och distribution av en Hybrid Runbook Worker i dina data center toorun hello runbooks och få åtkomst till dina GitHub Enterprise databasen toosynchronize runbooks med ditt Automation-konto.  


## <a name="getting-hello-scenario"></a>Hämta hello scenario

Det här scenariot består av två PowerShell-runbooks som kan importeras direkt från hello [Runbook-galleriet](automation-runbook-gallery.md) hello Azure-portalen eller ladda ned från hello [PowerShell-galleriet](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooks

Runbook | Beskrivning| 
--------|------------|
Export-RunAsCertificateToHybridWorker | Runbook exporterar ett RunAs-certifikat från en Automation-konto tooa hybrid worker så att runbooks på hello worker kan autentisera med Azure i ordning tooimport runbooks i hello Automation-konto.| 
Synkronisera LocalGitFolderToAutomationAccount | Runbook synkroniseringar hello lokal Git-mapp på hello hybrid datorn och sedan importera hello runbook-filer (*.ps1) till hello Automation-konto.|

### <a name="credentials"></a>Autentiseringsuppgifter

Autentiseringsuppgift | Beskrivning|
-----------|------------|
GitHRWCredential | Autentiseringsuppgiftstillgång skapar du toocontain hello användarnamn och lösenord för en användare med behörighet toohello hybrid worker.|

## <a name="installing-and-configuring-this-scenario"></a>Installera och konfigurera det här scenariot

### <a name="prerequisites"></a>Krav

1. hello Sync LocalGitFolderToAutomationAccount runbook autentiserar med hjälp av hello [Azure kör som-konto](automation-sec-configure-azure-runas-account.md). 

2. Microsoft Operations Management Suite (OMS) arbetsytan med hello Azure Automation-lösningen aktiverad och konfigurerad krävs också.  Om du inte har någon som är associerad med hello Automation-konto som används för tooinstall och konfigurera det här scenariot, det har skapats och konfigurerats för dig när du kör hello **ny OnPremiseHybridWorker.ps1** skriptet från hello hybrid runbook worker.        

    > [!NOTE]
    > För närvarande hello följande regioner endast stöd för Automation integrering med OMS - **Australien sydost**, **östra USA 2**, **Sydostasien**, och **Väst Europa**. 

3. En dator som kan fungera som en dedikerad Hybrid Runbook Worker där också hello GitHub programvara och underhåll hello runbook-filer (*runbook*.ps1) i en katalog på hello filen system toosynchronize mellan GitHub och Automation-konto.

### <a name="import-and-publish-hello-runbooks"></a>Importera och publicera hello runbooks

tooimport hello *Export RunAsCertificateToHybridWorker* och *Sync LocalGitFolderToAutomationAccount* runbooks från hello Runbook-galleriet från ditt Automation-konto i hello Azure-portalen Följ procedurerna hello i [importera Runbook från hello Runbook-galleriet](automation-runbook-gallery.md#to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal). Publicera hello runbooks när de har importerats till ditt Automation-konto.

### <a name="deploy-and-configure-hybrid-runbook-worker"></a>Distribuera och konfigurera Hybrid Runbook Worker

Om du inte har en Hybrid Runbook Worker som redan har distribuerats i ditt datacenter du bör granska hello krav och gör hello automatiserad installation med hjälp av hello proceduren i [Azure Automation Hybrid Runbook Worker - automatisera installera Konfiguration och](automation-hybrid-runbook-worker.md#automated-deployment).  När du har installerat hello worker-hybrid på en dator, utföra hello följande steg toocomplete dess konfiguration toosupport det här scenariot.

1. Inloggning toohello datorn värd hello Hybrid Runbook Worker-rollen med ett konto som har lokala administrativa rättigheter och skapa en katalog toohold hello Git runbook filer.  Klona hello internt Git-lagringsplatsen toohello directory.
2. Om du inte redan har ett konto skapas eller om du vill toocreate en ny avsedd för detta ändamål, hello Azure-portalen navigera tooAutomation konton, Välj ditt Automation-konto och skapa en [autentiseringsuppgiftstillgång](automation-credentials.md) som innehåller hello användarnamn och lösenord för en användare med behörighet toohello hybrid worker.  
3. Från ditt Automation-konto [redigera hello runbook](automation-edit-textual-runbook.md)**Export RunAsCertificateToHybridWorker** och ändra hello hello variabels värde *$Password* med ett starkt lösenordet.    När du har ändrat hello värde klickar du på **publicera** toohave hello utkastversionen för hello runbook publiceras. 
5. Starta hello runbook **Export RunAsCertificateToHybridWorker**, och i hello **starta Runbook** bladet under hello alternativet **körningsinställningar** alternativet hello **Hybrid Worker** och i hello listrutan väljer hello Hybrid worker-grupp du skapade tidigare i det här scenariot.  

    Detta exporterar en certifikatet toohello hybrid worker så att runbooks på hello worker kan autentisera med Azure med hjälp av hello-kör som-anslutning i ordning toomanage Azure-resurser (särskilt för det här scenariot - importera runbooks toohello Automation-konto).

4. Från ditt Automation-konto, Välj hello Hybrid worker-gruppen som skapades tidigare och [ange ett RunAs-konto](automation-hrw-run-runbooks.md#runas-account) för hello Hybrid worker-gruppen och valt hello autentiseringsuppgiftstillgång du bara eller redan har skapat.  Detta säkerställer att hello Sync runbook kan köra Git-kommandon. 
5. Starta hello runbook **Sync LocalGitFolderToAutomationAccount**, ange hello Indataparametern värden för obligatoriska och i hello **starta Runbook** bladet under hello alternativet **kör inställningar för** alternativet hello **Hybrid Worker** och i hello listrutan väljer hello Hybrid worker-grupp du skapade tidigare i det här scenariot:
    * *ResourceGroup* - hello namnet på resursgruppen som är associerade med ditt Automation-konto
    * *AutomationAccountName* - hello namnet på ditt Automation-konto
    * *GitPath* - hello lokal mapp eller fil på hello Hybrid Runbook Worker där Git ställs in toopull senaste ändringarna i

    Detta synkronisera hello lokal Git-mapp på hello hybrid worker-dator och importera sedan hello .ps1-filer från hello källan directory toohello Automation-konto.

    ![Starta Sync LocalGitFolderToAutomationAccount Runbook](media/automation-scenario-source-control-integration-with-github-ent/start-runbook-synclocalgitfoldertoautoacct.png)<br>

7. Visa jobb sammanfattningsinformation för hello runbook genom att välja den hello **Runbooks** bladet i Automation-konto och välj sedan hello **jobb** panelen.  Bekräfta att den har slutförts genom att välja hello **alla loggar** panelen och granska hello detaljerad loggström.  

## <a name="next-steps"></a>Nästa steg

-  tooknow mer om runbook-typer, fördelar och begränsningar finns [typer för Azure Automation-runbook](automation-runbook-types.md)
-  Mer information om PowerShell-skriptstöd finns i [Inbyggt PowerShell-skriptstöd i Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)
