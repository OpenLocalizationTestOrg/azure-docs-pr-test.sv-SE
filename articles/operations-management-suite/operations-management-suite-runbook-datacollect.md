---
title: aaaCollecting Log Analytics-data med en runbook i Azure Automation | Microsoft Docs
description: "Stegvis självstudiekurs som går igenom hur du skapar en runbook i Azure Automation toocollect data i hello OMS-databas för analys av logganalys."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: operations-management-suite
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2017
ms.author: bwren
ms.openlocfilehash: e644dc3ef20fb1e930cae02e0fd44ccca31dc13d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collect-data-in-log-analytics-with-an-azure-automation-runbook"></a>Samla in data i logganalys med en Azure Automation-runbook
Du kan samla in data i logganalys mycket från olika källor, till exempel [datakällor](../log-analytics/log-analytics-data-sources.md) på agenter och även [data som samlas in från Azure](../log-analytics/log-analytics-azure-storage.md).  Det finns en scenarier men där du behöver toocollect data som inte är tillgängligt via dessa källor som standard.  I dessa fall kan du använda hello [HTTP Data Collector API: et](../log-analytics/log-analytics-data-collector-api.md) toowrite data tooLog Analytics från valfri REST API-klient.  En gemensam metoden tooperform Datasamlingen använder en runbook i Azure Automation.   

Den här kursen går igenom hello processen för att skapa och schemalägga en runbook i Azure Automation toowrite data tooLog Analytics.


## <a name="prerequisites"></a>Krav
Det här scenariot kräver hello efter resurser som konfigurerats i din Azure-prenumeration.  Båda kan vara ett kostnadsfritt konto.

- [Logga Analytics-arbetsyta](../log-analytics/log-analytics-get-started.md).
- [Azure automation-konto](../automation/automation-offering-get-started.md).

## <a name="overview-of-scenario"></a>Översikt över scenario
Den här självstudiekursen skriver du en runbook som samlar in information om Automation-jobb.  Azure Automation-Runbooks implementeras med PowerShell, så att du ska börja med att skriva och testa ett skript i hello Azure Automation-redigeraren.  När du har kontrollerat att du har samlat hello krävs information kan du skriva den data tooLog Analytics och verifiera hello anpassade datatypen.  Slutligen ska du skapa ett schema toostart hello runbook med jämna mellanrum.

> [!NOTE]
> Du kan konfigurera Azure Automation toosend jobbet information tooLog Analytics utan denna runbook.  Det här scenariot är främst används toosupport hello självstudiekursen och det rekommenderas att skicka hello tooa test arbetsyta.  


## <a name="1-install-data-collector-api-module"></a>1. Installera Data Collector API-modulen
Varje [begäran från hello HTTP Data Collector API: et](../log-analytics/log-analytics-data-collector-api.md#create-a-request) måste formateras på rätt sätt och innehålla ett authorization-huvud.  Du kan göra detta i din runbook, men du kan minska hello kod krävs med hjälp av en modul som förenklar processen.  En modul som du kan använda är [OMSIngestionAPI modulen](https://www.powershellgallery.com/packages/OMSIngestionAPI) i hello PowerShell-galleriet.

toouse en [modulen](../automation/automation-integration-modules.md) i en runbook måste den installeras i ditt Automation-konto.  Varje runbook i samma konto kan sedan använda hello hello funktioner i hello modul.  Du kan installera en ny modul genom att välja **tillgångar** > **moduler** > **lägga till en modul** i Automation-kontot.  

hello PowerShell-galleriet men ger dig en snabb alternativet toodeploy en modul direkt tooyour automation-konto så att du kan använda alternativet för den här självstudiekursen.  

![OMSIngestionAPI modul](media/operations-management-suite-runbook-datacollect/OMSIngestionAPI.png)

1. Gå för[PowerShell-galleriet](https://www.powershellgallery.com/).
2. Sök efter **OMSIngestionAPI**.
3. Klicka på hello **distribuera tooAzure Automation** knappen.
4. Välj ditt automation-konto och klicka på **OK** tooinstall hello modulen.


## <a name="2-create-automation-variables"></a>2. Skapa variabler för Automation
[Automationsvariabler](..\automation\automation-variables.md) innehålla värden som kan användas av alla runbooks i ditt Automation-konto.  De gör runbooks mer flexibel genom att låta dig toochange dessa värden utan att redigera hello faktiska runbook. Varje begäran från hello HTTP Data Collector API kräver hello-ID och nyckeln för hello OMS-arbetsytan och variabeln tillgångar är perfekt toostore informationen.  

![Variabler](media/operations-management-suite-runbook-datacollect/variables.png)

1. Navigera tooyour Automation-konto i hello Azure-portalen.
2. Välj **variabler** under **delade resurser**.
2. Klicka på **lägga till en variabel** och skapa hello två variabler i hello i den följande tabellen.

| Egenskap | Arbetsytan ID-värde | Nyckelvärdet för arbetsytan |
|:--|:--|:--|
| Namn | WorkspaceId | WorkspaceKey |
| Typ | Sträng | Sträng |
| Värde | Klistra in i hello arbetsyte-ID för logganalys-arbetsytan. | Klistra in med hello primära eller sekundärnyckeln i logganalys-arbetsytan. |
| Krypterade | Nej | Ja |



## <a name="3-create-runbook"></a>3. Skapa runbook

Azure Automation har en redigerare i hello portal där du kan redigera och testa din runbook.  Du har hello alternativet toouse hello skript editor toowork med [PowerShell direkt](../automation/automation-edit-textual-runbook.md) eller [skapar en grafisk runbook](../automation/automation-graphical-authoring-intro.md).  För den här självstudiekursen kommer du arbeta med ett PowerShell-skript. 

![Redigera runbooken](media/operations-management-suite-runbook-datacollect/edit-runbook.png)

1. Navigera tooyour Automation-konto.  
2. Klicka på **Runbooks** > **lägga till en runbook** > **skapa en ny runbook**.
3. Hej runbook-namn, Skriv **samla in-Automation-jobb**.  Hej runbooktyp Välj **PowerShell**.
4. Klicka på **skapa** toocreate hello runbook och starta hello redigeraren.
5. Kopiera och klistra in följande kod i hello runbook hello.  Läs toohello kommentarer i hello skript förklaring av hello kod.
    
        # Get information required for hello automation account from parameter values when hello runbook is started.
        Param
        (
            [Parameter(Mandatory = $True)]
            [string]$resourceGroupName,
            [Parameter(Mandatory = $True)]
            [string]$automationAccountName
        )
        
        # Authenticate toohello Automation account using hello Azure connection created when hello Automation account was created.
        # Code copied from hello runbook AzureAutomationTutorial.
        $connectionName = "AzureRunAsConnection"
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
        Add-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
        
        # Set hello $VerbosePreference variable so that we get verbose output in test environment.
        $VerbosePreference = "Continue"
        
        # Get information required for Log Analytics workspace from Automation variables.
        $customerId = Get-AutomationVariable -Name 'WorkspaceID'
        $sharedKey = Get-AutomationVariable -Name 'WorkspaceKey'
        
        # Set hello name of hello record type.
        $logType = "AutomationJob"
        
        # Get hello jobs from hello past hour.
        $jobs = Get-AzureRmAutomationJob -ResourceGroupName $resourceGroupName -AutomationAccountName $automationAccountName -StartTime (Get-Date).AddHours(-1)
        
        if ($jobs -ne $null) {
            # Convert hello job data toojson
            $body = $jobs | ConvertTo-Json
        
            # Write hello body tooverbose output so we can inspect it if verbose logging is on for hello runbook.
            Write-Verbose $body
        
            # Send hello data tooLog Analytics.
            Send-OMSAPIIngestionFile -customerId $customerId -sharedKey $sharedKey -body $body -logType $logType -TimeStampField CreationTime
        }


## <a name="4-test-runbook"></a>4. Testa runbook
Azure Automation innehåller en miljö för[testa din runbook](../automation/automation-testing-runbook.md) innan du publicerar den.  Du kan inspektera hello data som samlas in av hello runbook och verifiera att skrivs tooLog Analytics som förväntat innan du publicerar den tooproduction. 
 
![Testa runbook](media/operations-management-suite-runbook-datacollect/test-runbook.png)

6. Klicka på **spara** toosave hello runbook.
1. Klicka på **Test fönstret** tooopen hello runbook i hello testmiljö.
3. Eftersom din runbook har parametrar, är du tillfrågas tooenter värden för dessa.  Ange hello namnet på resursgruppen hello hello automation-kontot som kommer toocollect jobbinformation från.
4. Klicka på **starta** toohello starta hello runbook.
3. Hej runbook startas med statusen **i kö** innan den försätts för**kör**.  
3. Hej runbook ska visa utförliga utdata med hello jobb som samlas in i json-format.  Om inga jobb visas sedan kanske har inga jobb skapas hello automation-konto i hello senaste timmen.  Försök att starta en runbook i hello automation-kontot och utför sedan hello test igen.
4. Se till att hello utdata inte visar eventuella fel i hello efter kommandot tooLog Analytics.  Du bör ha ett meddelande liknande toohello följande.

    ![Post-utdata](media/operations-management-suite-runbook-datacollect/post-output.png)

## <a name="5-verify-records-in-log-analytics"></a>5. Kontrollera posterna i logganalys
När hello runbook har slutförts i test och du har kontrollerat att hello utdata har tagits emot kan du kontrollera att hello poster har skapats med en [loggen Sök i logganalys](../log-analytics/log-analytics-log-searches.md).

![Loggutdata saknas](media/operations-management-suite-runbook-datacollect/log-output.png)

1. Välj logganalys-arbetsytan i hello Azure-portalen.
2. Klicka på **logga Sök**.
3. Typen hello följande kommando `Type=AutomationJob_CL` och klicka på sökknappen hello. Observera att hello posttyp innehåller _CL som inte anges i hello skript.  Det suffixet är automatiskt tillagda toohello loggen typen tooindicate att det är en typ av anpassad logg.
4. Du bör se en eller flera poster returneras som anger att hello runbook fungerar som förväntat.


## <a name="6-publish-hello-runbook"></a>6. Publicera hello runbook
När du har kontrollerat att hello runbooken fungerar korrekt, måste toopublish den så att du kan köra den i produktion.  Du kan fortsätta tooedit och testa hello runbook utan att ändra hello publicerad version.  

![Publicera runbook](media/operations-management-suite-runbook-datacollect/publish-runbook.png)

1. Returnera tooyour automation-konto.
2. Klicka på **Runbooks** och välj **samla in-Automation-jobb**.
3. Klicka på **redigera** och sedan **publicera**.
4. Klicka på **Ja** när och tooverify som du vill toooverwrite hello tidigare publicerade versionen.

## <a name="7-set-logging-options"></a>7. Ange alternativ för loggning 
Testet du var kan tooview [utförlig utdata](../automation/automation-runbook-output-and-messages.md#message-streams) eftersom hello $VerbosePreference variabeln har angetts i hello skript.  För produktion behöver du tooset hello Loggningsegenskaper för hello runbook om du vill tooview utförlig utdata.  För hello-runbook som används i den här självstudiekursen visas hello json-data skickas tooLog Analytics.

![Loggning och spårning](media/operations-management-suite-runbook-datacollect/logging.png)

1. Välj i hello egenskaperna för din runbook **loggning och spårning** under **Runbook-inställningar**.
2. Ändra hello inställningen för **logga utförliga poster** för**på**.
3. Klicka på **Spara**.

## <a name="8-schedule-runbook"></a>8. Schemalägg runbook
hello vanligaste sättet toostart en runbook som samlar in övervakningsdata är tooschedule den toorun automatiskt.  Det gör du genom att skapa en [schema i Azure Automation](../automation/automation-schedules.md) och kopplar den tooyour runbook.

![Schemalägg runbook](media/operations-management-suite-runbook-datacollect/schedule-runbook.png)

1. I hello egenskaper för din runbook, Välj **scheman** under **resurser**.
2. Klicka på **lägga till ett schema** > **länka ett schema tooyour runbook** > **skapa ett nytt schema**.
5. Typen i hello följande värden för hello schema och klickar på **skapa**.

| Egenskap | Värde |
|:--|:--|
| Namn | AutomationJobs-varje timme |
| Startar | Välj helst minst 5 minuter senaste hello aktuell tid. |
| Upprepning | Återkommande |
| Upprepas var | 1 timme |
| Ange förfallodatum | Nej |

När hello schema har skapats måste tooset hello parametervärden som kommer att användas varje gång det här schemat startar hello runbook.

6. Klicka på **konfigurera parametrar och körningsinställningar**.
7. Fyll i värden för din **ResourceGroupName** och **AutomationAccountName**.
8. Klicka på **OK**. 

## <a name="9-verify-runbook-starts-on-schedule"></a>9. Kontrollera runbook startar enligt schema
Varje gång en runbook startas [ett jobb skapas](../automation/automation-runbook-execution.md) och all utdata som loggas.  Detta är i själva verket hello samlar in samma jobb som hello runbook.  Du kan kontrollera att hello runbook startar som förväntat genom att kontrollera hello jobb för runbook hello när hello starttiden för hello schema har överskridits.

![Jobb](media/operations-management-suite-runbook-datacollect/jobs.png)

1. I hello egenskaper för din runbook, Välj **jobb** under **resurser**.
2. Du bör se en lista över jobb för varje gång hello runbook startades.
3. Klicka på någon av hello jobb tooview information.
4. Klicka på **alla loggar** tooview hello loggar och utdata från hello runbook.
5. Rulla toohello nedre toofind en post liknande toohello bilden nedan.<br>![Utförlig](media/operations-management-suite-runbook-datacollect/verbose.png)
6. Klicka på den här posten tooview hello detaljerad json-data som skickades tooLog Analytics.



## <a name="next-steps"></a>Nästa steg
- Använd [Vydesigner](../log-analytics/log-analytics-view-designer.md) toocreate en vy som visar hello data som du har samlat in toohello logganalys-databasen.
- Paketera din runbook i en [hanteringslösning](operations-management-suite-solutions-creating.md) toodistribute toocustomers.
- Lär dig mer om [logganalys](https://docs.microsoft.com/azure/log-analytics/).
- Lär dig mer om [Azure Automation](https://docs.microsoft.com/azure/automation/).
- Mer information om hello [HTTP Data Collector API: et](../log-analytics/log-analytics-data-collector-api.md).
