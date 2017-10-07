---
title: aaaSource kontroll integrering i Azure Automation | Microsoft Docs
description: "Den här artikeln beskriver källkontrollintegrering med GitHub i Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 224d7375-9887-44dd-b137-06ffe396a4b4
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: magoedte;sngun
ms.openlocfilehash: 6ceee1de8065fafe85a13bbd7f585e74dbc96b47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="source-control-integration-in-azure-automation"></a>Källkontrollintegrering i Azure Automation
Källkontrollintegrering kan tooassociate runbooks i ditt Automation-konto tooa GitHub för källkontroll. Källkontrollen kan du tooeasily samarbeta med din grupp, spåra ändringar och återställa tooearlier versioner av dina runbooks. Exempelvis kan källkontrollen du toosync olika filialer i källkontrollen tooyour utveckling, test- eller produktionsmiljö Automation-konton, vilket gör det enkelt toopromote kod som har testats i produktionsmiljön development environment tooyour Automation konto.

Källkontrollen kan du toopush kod från Azure Automation toosource kontroll eller hämtar runbooks från källan kontrollen tooAzure Automation. Den här artikeln beskriver hur tooset in källa styra i Azure Automation-miljö. Vi börjar genom att konfigurera Azure Automation tooaccess GitHub-lagringsplatsen och gå igenom olika åtgärder som kan utföras med hjälp av källkontrollintegrering. 

> [!NOTE]
> Källkontrollen har stöd för dra och push-överföring [PowerShell-arbetsflöde runbooks](automation-runbook-types.md#powershell-workflow-runbooks) samt [PowerShell runbooks](automation-runbook-types.md#powershell-runbooks). [Grafiska runbook-flöden](automation-runbook-types.md#graphical-runbooks) stöds inte ännu.<br><br>
> 
> 

Det finns två enkla steg krävs tooconfigure källkontrollen för Automation-konto och bara om du redan har en GitHub-konto. De är:

## <a name="step-1--create-a-github-repository"></a>Steg 1 – skapa en GitHub-databas
Om du redan har en GitHub-konto och en databas som du vill toolink tooAzure Automation och sedan logga in tooyour befintliga kontot och starta från steg 2 nedan. Annars navigerar för[GitHub](https://github.com/), registrera dig för ett nytt konto och [skapar en ny lagringsplats](https://help.github.com/articles/create-a-repo/).

## <a name="step-2--set-up-source-control-in-azure-automation"></a>Steg 2 – konfigurera källkontroll i Azure Automation
1. Hello Automation-konto bladet i hello Azure-portalen klickar du på **Ange källkontroll.** 
   
    ![Konfigurera källkontroll](media/automation-source-control-integration/automation_01_SetUpSourceControl.png)
2. Hej **källkontrollen** blad öppnas, där du kan konfigurera din kontoinformation för GitHub. Nedan finns hello lista över parametrar tooconfigure:  
   
   | **Parametern** | **Beskrivning** |
   |:--- |:--- |
   | Välj källa |Välj hello källa. För närvarande endast **GitHub** stöds. |
   | Auktorisering |Klicka på hello **auktorisera** knappen toogrant Azure Automation åtkomst tooyour GitHub-lagringsplatsen. Om du redan har loggat in tooyour GitHub-konto i ett annat fönster hello används autentiseringsuppgifterna för kontot. När tillståndet är klar hello bladet visar ditt GitHub-användarnamn under **auktorisering egenskapen**. |
   | Välj databasen |Välj en GitHub-databas hello listan över tillgängliga databaser. |
   | Välj filialen |Välj en gren hello listan över tillgängliga filialer. Endast hello **master** grenen visas om du inte har skapat några filialer. |
   | Runbook-sökvägen för mappen |Hej runbook mappsökväg anger hello sökväg i hello GitHub-lagringsplats som du vill toopush eller hämtar din kod. Det måste anges i formatet hello **/mappnamn/undermappsnamn**. Endast runbookar i mappsökvägen för hello runbook kommer att vara synkroniserade tooyour Automation-konto. Runbooks i undermappar hello på hello runbook sökväg kommer **inte** synkroniseras. Använd  **/**  toosync alla hello runbooks under hello-databasen. |
3. Till exempel om du har en databas med namnet **PowerShellScripts** som innehåller en mapp med namnet **RootFolder så**, som innehåller en mapp med namnet **undermapp**. Du kan använda följande strängar toosync hello varje mappnivå:
   
   1. toosync runbooks från **databasen**, mappsökväg för runbook*/*
   2. toosync runbooks från **RootFolder så**, runbook mappsökväg */RootFolder*
   3. toosync runbooks från **undermapp**, runbook mappsökväg */RootFolder så/undermapp*.
4. När du har konfigurerat hello parametrar visas de på hello **ange källkontrollen bladet.**  
   
    ![Konfigurera bladet](media/automation-source-control-integration/automation_02_SourceControlConfigure.png)
5. När du klickar på OK källkontrollintegrering har nu konfigurerats för ditt Automation-konto och ska uppdateras med din GitHub-information. Nu kan du klicka på den här delen tooview synkronisera alla dina källkontrollen jobbhistorik.  
   
    ![Värden för databasen](media/automation-source-control-integration/automation_03_RepoValues.png)
6. När du har skapat källkontrollen kommer hello följande Automation resurser att skapas i ditt Automation-konto:  
   Två [variabeln tillgångar](automation-variables.md) skapas.  
   
   * hello variabeln **Microsoft.Azure.Automation.SourceControl.Connection** innehåller hello värden med hello anslutningssträngen, enligt nedan.  
     
     | **Parametern** | **Värde** |
     |:--- |:--- |
     | Namn |Microsoft.Azure.Automation.SourceControl.Connection |
     | Typ |Sträng |
     | Värde |{”Gren”:\<*namnet på din gren*>, ”RunbookFolderPath”:\<*Runbook mappsökväg*>, ”providertyp”:\<*har värdet 1 för GitHub*>, ”databas”:\<*namnet på databasen*>, ”användarnamn”:\<*din GitHub-användarnamn*>} |

    * hello variabeln **Microsoft.Azure.Automation.SourceControl.OAuthToken**, innehåller hello säker krypterade värdet för din OAuthToken.  

    |**Parametern**            |**Värde** |
    |:---|:---|
    | Namn  | Microsoft.Azure.Automation.SourceControl.OAuthToken |
    | Typ | Unknown(Encrypted) |
    | Värde | <*Krypterade OAuthToken*> |  

    ![Variabler](media/automation-source-control-integration/automation_04_Variables.png)  

    * **Automation källkontrollen** läggs till som en auktoriserad programmet tooyour GitHub-konto. tooview hello program: navigera från startsidan GitHub tooyour **profil** > **inställningar** > **program**. Det här programmet kan Azure Automation toosync dina GitHub-lagringsplatsen tooan Automation-konto.  

    ![Git-program](media/automation-source-control-integration/automation_05_GitApplication.png)


## <a name="using-source-control-in-automation"></a>Använda källkontrollen i Automation
### <a name="check-in-a-runbook-from-azure-automation-toosource-control"></a>Kontrollera i en runbook från Azure Automation toosource kontroll
Checka in Runbook kan du toopush hello ändringar du har gjort tooa runbook i Azure Automation i din källkontroll. Nedan visas hello stegen toocheck i en runbook:

1. Från ditt Automation-konto [skapa en ny textrepresentation runbook](automation-first-runbook-textual.md), eller [redigera en befintlig, textrepresentation runbook](automation-edit-textual-runbook.md). Denna runbook kan vara ett PowerShell-arbetsflöde eller en runbook för PowerShell-skript.  
2. När du redigerar en runbook, spara den och klickar på **checka in** från hello **redigera** bladet.  
   
    ![Knappen incheckning](media/automation-source-control-integration/automation_06_CheckinButton.png)

     > [!NOTE] 
     > Checka in från Azure Automation skrivs hello-kod som för närvarande finns i din källkontroll. Hej Git motsvarande kommandoraden anvisningarna toocheck i är **git Lägg till + git-incheckning + git push**  

1. När du klickar på **checka in**, du uppmanas att ange ett bekräftelsemeddelande klickar du på Ja toocontinue.  
   
    ![Checka in meddelande](media/automation-source-control-integration/automation_07_CheckinMessage.png)
2. Checka in startar hello källa kontrollen runbook: **Sync MicrosoftAzureAutomationAccountToGitHubV1**. Denna runbook ansluter tooGitHub och skickar ändringarna från Azure Automation tooyour databasen. tooview hello checka in jobbhistorik, gå tillbaka toohello **Källkontrollintegrering** och klicka på tooopen hello databasen synkronisering bladet. Det här bladet visas alla dina källkontrolljobb.  Välj hello jobb tooview och på tooview hello information.  
   
    ![Checka in Runbook](media/automation-source-control-integration/automation_08_CheckinRunbook.png)
   
   > [!NOTE]
   > Källan kontrollen runbooks är särskilda Automation-runbooks som du inte kan visa eller redigera. Även om de inte visas i listan runbook, ser du synkroniseringsjobb visas på listan jobb.
   > 
   > 
3. hello namnet på hello ändrade runbook skickas som en indataparameter toohello checka in runbook. Du kan [visa hello jobbinformation](automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) genom att expandera runbook i **databasen synkronisering** bladet.  
   
    ![Checka in indata](media/automation-source-control-integration/automation_09_CheckinInput.png)
4. Uppdatera dina GitHub-lagringsplatsen när hello jobbet slutförs tooview hello ändringar.  Det bör finnas ett genomförande i databasen med ett genomförande meddelande: **uppdaterade *Runbooknamnet* i Azure Automation.**  

### <a name="sync-runbooks-from-source-control-tooazure-automation"></a>Synkronisera runbooks från källan kontrollen tooAzure Automation
hello sync-knappen på hello databasen synkronisering bladet kan du toopull alla hello runbooks i hello runbook mappsökväg för din databas tooyour Automation-konto. hello kan samma databas vara synkroniserade toomore än en Automation-konto. Nedan visas hello steg toosync en runbook:

1. Hello Automation-konto där du ställa in källkontroll, öppna hello **källa kontrollen Integration/databasen synkronisering bladet** och på **Sync** sedan uppmanas du att med en bekräftelse klickar du på **Ja** toocontinue.  
   
    ![Synkronisera knappen](media/automation-source-control-integration/automation_10_SyncButtonwithMessage.png)
2. Synkroniseringen hello runbook: **Sync MicrosoftAzureAutomationAccountFromGitHubV1**. Denna runbook ansluter tooGitHub och hämtar hello ändringar från din lagringsplats tooAzure Automation. Du bör se ett nytt jobb på hello **databasen synkronisering** bladet för den här åtgärden. tooview information om hello synkroniseringsjobb Klicka tooopen hello jobbet informationsbladet.  
   
    ![Synkronisera Runbook](media/automation-source-control-integration/automation_11_SyncRunbook.png)

    > [!NOTE] 
    > En Synkronisera från källkontroll skriver över hello Utkastversionen av hello runbooks som för närvarande finns i ditt Automation-konto för **alla** datakälla runbooks som för närvarande. Hej Git motsvarande kommandoraden instruktion toosync är **git, pull**


## <a name="troubleshooting-source-control-problems"></a>Felsökning för käll-kontroll
Om det finns några fel med ett checka in eller synkronisering jobb, hello jobbstatus bör pausas och du kan visa mer information om felet hello hello jobb-bladet.  Hej **alla loggar** del visas alla hello PowerShell strömmar som associeras med jobbet. Detta ger dig med hello information behövs toohelp du åtgärda eventuella problem med din kontakt eller synkronisering. Den visas också hello sekvens med åtgärder som uppstod när synkroniseringen eller kontroll i en runbook.  

![AllLogs bild](media/automation-source-control-integration/automation_13_AllLogs.png)

## <a name="disconnecting-source-control"></a>Kopplar från källkontrollen
toodisconnect från GitHub-konto öppnar hello databasen synkronisering bladet och klickar på **frånkoppling**. När du kopplar från källkontrollen runbooks som har synkroniserats tidigare finns kvar i ditt Automation-konto men hello databasen synkronisering bladet aktiveras inte.  

  ![Koppla från](media/automation-source-control-integration/automation_12_Disconnect.png)

## <a name="next-steps"></a>Nästa steg
Mer information om källkontrollintegrering finns hello följande resurser:  

* [Azure Automation: Källkontrollintegrering i Azure Automation](https://azure.microsoft.com/blog/azure-automation-source-control-13/)  
* [Rösta på din favorit källkontrollsystem](https://www.surveymonkey.com/r/?sm=2dVjdcrCPFdT0dFFI8nUdQ%3d%3d)  
* [Azure Automation: Integrera Runbook källkontrollen med hjälp av Visual Studio Team Services](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)  

