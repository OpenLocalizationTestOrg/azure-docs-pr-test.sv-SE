---
title: "Källkontrollintegrering i Azure Automation med GitHub Enterprise | Microsoft Docs"
description: "Innehåller information om hur du konfigurerar integrering med GitHub Enterprise för källkontroll Automation-runbooks."
services: automation
documentationCenter: 
authors: georgewallace
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
ms.openlocfilehash: cf72c6d05e2872bea84b8a7218bd318d5b8c9694
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/14/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-github-enterprise"></a>Azure Automation-scenario – Automation källkontrollintegrering med GitHub-Enterprise

Automation stöder för närvarande källkontrollintegrering där du kan associera runbooks i ditt Automation-konto till en GitHub-källkontroll.  Dock kunder som har distribuerat [GitHub Enterprise](https://enterprise.github.com/home) för att stödja sina DevOps-metoder, även vill använda den för att hantera livscykeln för runbooks som utvecklats för att automatisera affärsprocesser och tjänsten hanteringsåtgärder.  

I det här scenariot har du en Windows-dator i ditt datacenter som konfigurerats som en Hybrid Runbook Worker med Azure Resource Manager-moduler och Git-verktygen som installeras.  Hybrid worker-datorn har en klon av lokal Git-lagringsplats.  När runbook körs på worker-hybriden Git-katalogen har synkroniserats och runbook-filens innehåll som har importerats till Automation-kontot.

Den här artikeln beskriver hur du ställer in den här konfigurationen i Azure Automation-miljö. Vi börjar med att konfigurera Automation med säkerhetsreferenser runbooks som krävs för att stödja det här scenariot och distribution av en Hybrid Runbook Worker i ditt datacenter att köra runbooks och få åtkomst till dina GitHub Enterprise-databasen för att synkronisera runbooks med ditt Automation-konto.  


## <a name="getting-the-scenario"></a>Hämta scenariot

Det här scenariot består av två PowerShell-runbooks som kan importeras direkt från den [Runbook-galleriet](automation-runbook-gallery.md) i Azure-portalen eller hämtas från den [PowerShell-galleriet](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooks

Runbook | Beskrivning| 
--------|------------|
Export-RunAsCertificateToHybridWorker | Runbook exporterar ett RunAs-certifikat från en Automation-konto till en hybrid worker så att runbooks på worker kan autentisera med Azure för att kunna importera runbooks till Automation-kontot.| 
Synkronisera LocalGitFolderToAutomationAccount | Synkroniserar lokala Git-mappen på hybrid-datorn och sedan importera runbook-filer (*.ps1) till Automation-kontot.|

### <a name="credentials"></a>Autentiseringsuppgifter

Autentiseringsuppgift | Beskrivning|
-----------|------------|
GitHRWCredential | Autentiseringsuppgiftstillgång som du skapar för att innehålla användarnamn och lösenord för en användare med behörighet för worker-hybriden.|

## <a name="installing-and-configuring-this-scenario"></a>Installera och konfigurera det här scenariot

### <a name="prerequisites"></a>Krav

1. Synkronisera LocalGitFolderToAutomationAccount runbook autentiserar med hjälp av den [Azure kör som-konto](automation-sec-configure-azure-runas-account.md). 

2. Det krävs också en arbetsyta för Microsoft Operations Management Suite (OMS) med Azure Automation-lösningen aktiverad och konfigurerad.  Om du inte har någon som är kopplat till Automation-kontot som används för att installera och konfigurera det här scenariot, det har skapats och konfigurerats för dig när du kör den **ny OnPremiseHybridWorker.ps1** skriptet från runbook worker-hybriden.        

    > [!NOTE]
    > För närvarande följande regioner endast stöd för Automation integrering med OMS - **Australien sydost**, **östra USA 2**, **Sydostasien**, och **Västeuropa**. 

3. En dator som kan fungera som en dedikerad Hybrid Runbook Worker där även GitHub-programvara och underhålla runbook-filer (*runbook*.ps1) i en källkatalog i filsystemet synkroniseras mellan GitHub och ditt Automation-konto.

### <a name="import-and-publish-the-runbooks"></a>Importera och publicera runbooks

Så här importerar du den *Export RunAsCertificateToHybridWorker* och *Sync LocalGitFolderToAutomationAccount* runbooks från galleriet Runbook från ditt Automation-konto i Azure-portalen, följer du procedurerna i [importera Runbook från galleriet Runbook](automation-runbook-gallery.md#to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal). Publicera runbooks när de har importerats till ditt Automation-konto.

### <a name="deploy-and-configure-hybrid-runbook-worker"></a>Distribuera och konfigurera Hybrid Runbook Worker

Om du inte har en Hybrid Runbook Worker som redan har distribuerats i ditt datacenter, bör du granska kraven och gör automatiserad installation med hjälp av proceduren i [Azure Automation Hybrid Runbook Worker - automatisera installera och konfigurera](automation-hybrid-runbook-worker.md#automated-deployment).  När du har installerat worker-hybrid på en dator, utför följande steg för att slutföra sin konfiguration för att stödja det här scenariot.

1. Logga in på datorn som är värd för Hybrid Runbook Worker-rollen med ett konto som har lokala administrativa rättigheter och skapa en katalog för att lagra filer för Git-runbook.  Klona den interna Git-lagringsplatsen till katalogen.
2. Om du inte redan har ett konto skapas eller om du vill skapa en ny en dedikerad för detta ändamål, från Azure portal går du till Automation-konton, Välj ditt Automation-konto och skapa en [autentiseringsuppgiftstillgång](automation-credentials.md) som innehåller användarnamn och lösenord för en användare med behörighet för worker-hybriden.  
3. Från ditt Automation-konto [redigera runbook](automation-edit-textual-runbook.md)**Export RunAsCertificateToHybridWorker** och ändra värdet för variabeln *$Password* med ett starkt lösenord.  När du har ändrat värdet klickar du på **publicera** ha utkastet för runbook publiceras. 
5. Starta runbook **Export RunAsCertificateToHybridWorker**, och i den **starta Runbook** bladet med alternativet **körningsinställningar** väljer alternativet **Hybrid Worker** och välj Hybrid worker-grupp som du skapade tidigare i det här scenariot i den nedrullningsbara listan.  

    Detta exporterar ett certifikat till hybrid worker så att runbooks på worker kan autentisera med Azure med hjälp av kör som-anslutningen för att hantera Azure-resurser (särskilt för det här scenariot - import av runbooks Automation-kontot).

4. Välj Hybrid worker-gruppen som skapades tidigare från ditt Automation-konto och [ange ett RunAs-konto](automation-hrw-run-runbooks.md#runas-account) för Hybrid worker-gruppen och välj autentiseringsuppgiftstillgång du bara eller redan har skapat.  Detta säkerställer att synkronisera runbook kan köra Git-kommandon. 
5. Starta runbook **Sync LocalGitFolderToAutomationAccount**, ange följande obligatoriska Indataparametern värden och i den **starta Runbook** bladet med alternativet **körningsinställningar** väljer alternativet **Hybrid Worker** och välj Hybrid worker-grupp som du skapade tidigare i det här scenariot i den nedrullningsbara listan:
    * *ResourceGroup* -namnet på resursgruppen som är associerade med ditt Automation-konto
    * *AutomationAccountName* -namnet på ditt Automation-konto
    * *GitPath* -lokal mapp eller fil på där Git ställs in för att hämta senaste ändringarna till Hybrid Runbook Worker

    Detta synkroniserar den lokala Git-mappen på hybrid worker-dator och sedan importera .ps1-filer från källkatalogen till Automation-kontot.

    ![Starta Sync LocalGitFolderToAutomationAccount Runbook](media/automation-scenario-source-control-integration-with-github-ent/start-runbook-synclocalgitfoldertoautoacct.png)<br>

7. Visa sammanfattningsinformation för jobb för runbook genom att välja det från den **Runbooks** bladet i Automation-konto och välj sedan den **jobb** panelen.  Bekräfta att den har slutförts genom att välja den **alla loggar** panelen och granska detaljerad logg-dataströmmen.  

## <a name="next-steps"></a>Nästa steg

-  Mer information om typer av runbooks, och om deras fördelar och begränsningar, finns i [Typer av Azure Automation-runbooks](automation-runbook-types.md)
-  Mer information om PowerShell-skriptstöd finns i [Inbyggt PowerShell-skriptstöd i Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)
