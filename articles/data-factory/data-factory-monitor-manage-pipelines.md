---
title: aaaMonitor och hantera pipelines med hello Azure portal och PowerShell | Microsoft Docs
description: "Lär dig hur toouse hello Azure-portalen och Azure PowerShell toomonitor och hantera hello Azure datafabriker och pipelines som du har skapat."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 9b0fdc59-5bbe-44d1-9ebc-8be14d44def9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: spelluru
ms.openlocfilehash: a8d3c7943e79450895ff754f06a37fdad1cbef92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-hello-azure-portal-and-powershell"></a>Övervaka och hantera Azure Data Factory pipelines med hello Azure portal och PowerShell
> [!div class="op_single_selector"]
> * [Med hjälp av Azure portal/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
> * [Med hjälp av övervakning och Management-appen](data-factory-monitor-manage-app.md)


> [!IMPORTANT]
> hello övervakning och hantering av programmet ger ett bättre stöd för att övervaka och hantera dina data pipelines och felsöka eventuella problem. Mer information om hur du använder programmet hello finns [övervaka och hantera Data Factory pipelines genom att använda appen för övervakning och hantering av hello](data-factory-monitor-manage-app.md). 


Den här artikeln beskrivs hur toomonitor, hantera och felsöka din pipelines med hjälp av Azure portal och PowerShell. hello artikeln innehåller även information om hur toocreate aviseringar och få ett meddelande om misslyckade.

## <a name="understand-pipelines-and-activity-states"></a>Förstå pipelines och aktivitet tillstånd
Med hjälp av hello Azure-portalen kan du:

* Visa din data factory som ett diagram.
* Visa aktiviteter i en pipeline.
* Visa inkommande och utgående datauppsättningar.

Det här avsnittet beskriver också hur en datamängdssektor övergångar från ett tillstånd tooanother tillstånd.   

### <a name="navigate-tooyour-data-factory"></a>Navigera tooyour data factory
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **datafabriker** på hello menyn hello vänster. Om du inte ser det klickar du på **fler tjänster >**, och klicka sedan på **datafabriker** under hello **INTELLIGENCE + analys** kategori.

   ![Bläddra igenom alla > datafabriker](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. På hello **datafabriker** bladet, Välj hello data factory som du är intresserad av.

    ![Välj data factory](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   Du bör se hello startsidan för hello data factory.

   ![Data factory-bladet](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>Diagramvy i din data factory
Hej **Diagram** vy av en datafabrik som innehåller en enda om toomonitor och hantera hello data factory och dess tillgångar. toosee hello **Diagram** visa i din data factory, klickar du på **Diagram** på hello startsida för hello data factory.

![Diagramvy](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

Du kan zooma in, Zooma in, Zooma toofit, Zooma too100%, lås hello layout hello diagram och automatiskt placera pipelines och datauppsättningar. Du kan också se hello härkomst informationen (det vill säga visa överordnade och underordnade objekt av valda objekt).

### <a name="activities-inside-a-pipeline"></a>Aktiviteter i en pipeline
1. Högerklicka på hello pipeline och klicka sedan på **öppna pipeline** toosee alla aktiviteter i hello pipeline tillsammans med indata- och datauppsättningar för hello aktiviteter. Den här funktionen är användbart när din pipeline innehåller mer än en aktivitet och du vill toounderstand hello operativa övriga en enkel rörledning.

    ![Menyn Öppna pipeline](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. I följande exempel hello, ser du en kopia aktivitet i pipelinen hello med indata och utdata. 

    ![Aktiviteter i en pipeline](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. Du kan gå tillbaka toohello startsidan i hello data factory genom att klicka på hello **datafabriken** länken i hello spåret hello övre vänstra hörn.

    ![Gå tillbaka toodata fabriken](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-hello-state-of-each-activity-inside-a-pipeline"></a>Hello visningsstatus för varje aktivitet i en pipeline
Du kan visa hello aktuell status för en aktivitet genom att visa hello status för hello datauppsättningar som produceras av hello-aktivitet.

Genom att dubbelklicka på hello **OutputBlobTable** i hello **Diagram**, du kan se alla hello-segment som genereras av en annan aktivitet körs i en pipeline. Du kan se att hello kopieringsaktiviteten för hello har körts senaste åtta timmar och producerade hello segment i hello **klar** tillstånd.  

![Tillstånd för hello pipeline](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

hello dataset segment i hello data factory kan ha något av följande statusar hello:

<table>
<tr>
    <th align="left">Status</th><th align="left">Undertillstånden</th><th align="left">Beskrivning</th>
</tr>
<tr>
    <td rowspan="8">Väntar</td><td>ScheduleTime</td><td>hello tiden har inte inne för hello sektorn toorun.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Hej uppströmsberoendena är inte redo.</td>
</tr>
<tr>
<td>ComputeResources</td><td>hello beräkningsresurser är inte tillgängliga.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Alla instanser av hello aktivitet är upptaget med att köra andra sektorer.</td>
</tr>
<tr>
<td>ActivityResume</td><td>hello aktiviteten har pausats och kan inte köras hello segment förrän hello aktivitet återupptas.</td>
</tr>
<tr>
<td>Försök igen</td><td>Aktivitetskörningen försöks.</td>
</tr>
<tr>
<td>Validering</td><td>Verifieringen har inte startat ännu.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Verifieringen är väntar toobe igen.</td>
</tr>
<tr>
<tr>
<td rowspan="2">InProgress</td><td>Verifiera</td><td>Verifiering pågår.</td>
</tr>
<td>-</td>
<td>hello sektorn behandlas.</td>
</tr>
<tr>
<td rowspan="4">Det gick inte</td><td>För lång tid</td><td>hello aktivitetskörningen tog längre tid än vad som tillåts av hello-aktivitet.</td>
</tr>
<tr>
<td>Avbrutna</td><td>hello sektorn avbröts av en användare.</td>
</tr>
<tr>
<td>Validering</td><td>Verifieringen misslyckades.</td>
</tr>
<tr>
<td>-</td><td>Det gick inte att toobe genereras och/eller verifiera hello sektorn.</td>
</tr>
<td>Redo</td><td>-</td><td>hello sektorn är klar för användning.</td>
</tr>
<tr>
<td>Hoppades över</td><td>Ingen</td><td>hello segment bearbetas inte.</td>
</tr>
<tr>
<td>Ingen</td><td>-</td><td>En sektor används tooexist med en annan status, men den har återställts.</td>
</tr>
</table>



Du kan visa hello information om en sektor genom att klicka på en post i segment på hello **nyligen uppdaterat segment** bladet.

![Sektorn information](./media/data-factory-monitor-manage-pipelines/slice-details.png)

Om hello segment har körts flera gånger, ser du flera rader i hello **aktiviteten körs** lista. Du kan visa information om en aktivitet som kör genom att klicka på Kör hello post i hello **aktiviteten körs** lista. hello i listan visas alla hello-loggfiler, tillsammans med ett felmeddelande om det finns en. Den här funktionen är användbart tooview och felsökningsloggar loggar utan tooleave din data factory.

![Aktivitetskörningsinformation](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Om det inte finns i hello hello sektorn **klar** tillstånd, som du kan se hello överordnade sektorer som inte är klar och blockerar hello aktuellt segment från att köras i hello **sektorer uppströms som inte är redo** lista. Den här funktionen är användbart när din sektorn är i **väntar på** tillstånd och du vill att toounderstand hello uppströmsberoendena som hello sektorn väntar.

![Sektorer uppströms som inte är klara](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>DataSet tillståndsdiagram
När du distribuerar en datafabrik och hello pipelines har en ogiltig aktiv period, sektorer övergång från ett tillstånd tooanother hello dataset. För närvarande följande hello sektorn status hello följande tillståndsdiagram:

![Tillståndsdiagram](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

hello dataset tillstånd övergången flödet i data factory är hello följande: väntar -> förlopp/i-pågående (verifierar) -> klar/misslyckades.

hello sektorn startar i en **väntar på** tillstånd, väntar på villkor toobe uppfyllda innan den körs. Körningen av hello aktiviteten startar sedan och hello sektorn försätts i ett **pågående** tillstånd. Hej aktivitetskörningen kan lyckas eller misslyckas. hello segment har markerats som **klar** eller **misslyckades**, baserat på hello resultatet av hello körning.

Du kan återställa hello sektorn toogo tillbaka från hello **klar** eller **misslyckades** tillstånd toohello **väntar på** tillstånd. Du kan också markera hello sektorn tillstånd för**hoppa över**, vilket förhindrar hello aktivitet från körs och inte behandlar hello sektorn.

## <a name="pause-and-resume-pipelines"></a>Pausa och återuppta pipelines
Du kan hantera dina pipelines med hjälp av Azure PowerShell. Du kan till exempel pausa och återuppta pipelines genom att köra Azure PowerShell-cmdlets. 

> [!NOTE] 
> hello diagramvyn stöder inte pausa och återuppta pipelines. Om du vill toouse ett användargränssnitt, Använd hello övervaka och hantera program. Mer information om hur du använder programmet hello finns [övervaka och hantera Data Factory pipelines genom att använda appen för övervakning och hantering av hello](data-factory-monitor-manage-app.md) artikel. 

Du kan Pausa/Pausa pipelines med hjälp av hello **pausa AzureRmDataFactoryPipeline** PowerShell-cmdlet. Denna cmdlet är användbar när du inte vill toorun din pipelines förrän problemet har åtgärdats. 

```powershell
Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Exempel:

```powershell
Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

Efter hello problemet har korrigerats med hello pipeline, kan du återuppta hello avbruten pipeline genom att köra följande PowerShell-kommando hello:

```powershell
Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Exempel:

```powershell
Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a>Felsöka pipelines
Azure Data Factory ger omfattande funktioner för du toodebug och felsöka pipelines med hjälp av hello Azure-portalen och Azure PowerShell.

> [! Observera} är det mycket enklare tootroubleshot fel med hello övervakning & Management-appen. Mer information om hur du använder programmet hello finns [övervaka och hantera Data Factory pipelines genom att använda appen för övervakning och hantering av hello](data-factory-monitor-manage-app.md) artikel. 

### <a name="find-errors-in-a-pipeline"></a>Sök efter fel i en pipeline
Om hello aktiviteten körs inte i en pipeline, är hello dataset som produceras av hello pipeline i ett feltillstånd på grund av hello-fel. Du kan felsöka och felsöka i Azure Data Factory med hjälp av följande metoder hello.

#### <a name="use-hello-azure-portal-toodebug-an-error"></a>Använd hello Azure portal toodebug ett fel
1. På hello **tabell** bladet, klickar du på hello problemet sektor som har hello **Status** ställa in också**misslyckades**.

   ![Tabell bladet med problem](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. På hello **datasektorn** bladet, klickar du på hello-aktivitet som körs som misslyckades.

   ![Datasektorn med ett fel](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. På hello **aktivitet köras information** bladet som du kan hämta hello-filer som är associerade med hello HDInsight bearbetning. Klicka på **hämta** för Status/stderr toodownload hello felloggen som innehåller information om hello felet.

   ![Aktiviteten kör informationsbladet med fel](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-toodebug-an-error"></a>Använd PowerShell toodebug ett fel
1. Starta **PowerShell**.
2. Kör hello **Get-AzureRmDataFactorySlice** kommandot toosee hello segment och deras status. Du bör se ett segment med hello status **misslyckades**.        

    ```powershell   
    Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   Exempel:

    ```powershell   
    Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   Ersätt **StartDateTime** med starttiden för din pipeline. 
3. Kör nu hello **Get-AzureRmDataFactoryRun** cmdlet tooget information om hello aktiviteter körs för hello sektorn.

    ```powershell   
    Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    Exempel:

    ```powershell   
    Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    hello-värdet för StartDateTime är hello starttiden för hello fel/problem sektor som du antecknade från hello föregående steg. hello tid ska omges av dubbla citattecken.
4. Du bör se utdata med information om felet i hello som är liknande toohello följande:

    ```   
    Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
    ResourceGroupName       : ADF
    DataFactoryName         : LogProcessingFactory3
    DatasetName               : EnrichedGameEventsTable
    ProcessingStartTime     : 10/10/2014 3:04:52 AM
    ProcessingEndTime       : 10/10/2014 3:06:49 AM
    PercentComplete         : 0
    DataSliceStart          : 5/5/2014 12:00:00 AM
    DataSliceEnd            : 5/6/2014 12:00:00 AM
    Status                  : FailedExecution
    Timestamp               : 10/10/2014 3:04:52 AM
    RetryAttempt            : 0
    Properties              : {}
    ErrorMessage            : Pig script failed with exit code '5'. See wasb://        adfjobs@spestore.blob.core.windows.net/PigQuery
                                    Jobs/841b77c9-d56c-48d1-99a3-
                8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                more details.
    ActivityName            : PigEnrichLogs
    PipelineName            : EnrichGameLogsPipeline
    Type                    :
    ```
5. Du kan köra hello **spara AzureRmDataFactoryLog** med hello ID-värde som du ser från hello-utdata och hämta hello loggfiler med hjälp av hello **- DownloadLogsoption** för hello cmdlet.

    ```powershell
    Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a>Kör fel i en pipeline

> [!IMPORTANT]
> Det är enklare tootroubleshoot fel och kör misslyckade segment med hjälp av hello övervakning & Management-appen. Mer information om hur du använder programmet hello finns [övervaka och hantera Data Factory pipelines genom att använda appen för övervakning och hantering av hello](data-factory-monitor-manage-app.md). 

### <a name="use-hello-azure-portal"></a>Använd hello Azure-portalen
När du felsöker och felsöka fel i en pipeline, kan du köra fel genom att gå toohello fel segment och klicka på hello **kör** hello kommandofältet-knappen.

![Kör en misslyckad sektor](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

I fallet hello sektorn valideringen misslyckades på grund av en princip-fel (till exempel om data inte är tillgängligt), kan du åtgärda hello fel och validera igen genom att klicka på hello **verifiera** hello kommandofältet-knappen.

![Åtgärda felen och validera](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a>Använda Azure PowerShell
Du kan köra fel med hjälp av hello **Set AzureRmDataFactorySliceStatus** cmdlet. Se hello [Set AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) avsnittet annan information om hello-cmdlet och syntax.

**Exempel:**

hello följande exempel anger hello status för alla segment för hello tabell 'DAWikiAggregatedData' too'Waiting' i hello Azure data factory 'WikiADF'.

hello 'Uppdateringstyp' anges too'UpstreamInPipeline ', vilket innebär att statusen för varje segment för hello tabellen och alla hello beroende (överordnad) tabeller är inställda too'Waiting'. hello är andra möjliga värde för den här parametern 'Person'.

```powershell
Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```

## <a name="create-alerts"></a>Skapa aviseringar
Användaren Azure loggar händelser när en Azure-resurs (till exempel data factory) skapas, uppdateras eller tas bort. Du kan skapa aviseringar på dessa händelser. Du kan använda Data Factory toocapture olika mått och skapa aviseringar på mått. Vi rekommenderar att du använder händelser för övervakning i realtid och använda mått i historiksyfte.

### <a name="alerts-on-events"></a>Aviseringar om händelser
Azure händelser ger användbar insikter om vad som händer i Azure-resurser. När du använder Azure Data Factory händelser genereras när:

* En datafabrik har skapats, uppdateras eller tas bort.
* Databearbetning (som ”kör”) har startats eller slutförts.
* Ett HDInsight-kluster på begäran skapas eller tas bort.

Du kan skapa varningar på dessa användarhändelser och konfigurera dem toosend e-postaviseringar toohello administratör och coadministrators hello prenumeration. Dessutom kan du ange ytterligare e-postadresserna för användare som behöver tooreceive e-postaviseringar när hello villkor är uppfyllda. Den här funktionen är användbart när du vill meddelas när fel tooget och inte vill toocontinuously övervaka din data factory.

> [!NOTE]
> För närvarande visar hello portal inte aviseringar om händelser. Använd hello [övervakning och hantering av](data-factory-monitor-manage-app.md) toosee alla aviseringar.


#### <a name="specify-an-alert-definition"></a>Ange en definition av varning
toospecify en varningsdefinitionen du skapa en JSON-fil som beskriver hello-åtgärder som du vill att ett meddelande om toobe. I följande exempel hello, skickar hello avisering ett e-postmeddelande för hello RunFinished igen. toobe specifika, ett e-postmeddelande skickas när en körning i hello data factory har slutförts och inte det gick att köra hello (Status = FailedExecution).

```JSON
{
    "contentVersion": "1.0.0.0",
     "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters": {},
    "resources":
    [
        {
            "name": "ADFAlertsSlice",
            "type": "microsoft.insights/alertrules",
            "apiVersion": "2014-04-01",
            "location": "East US",
            "properties":
            {
                "name": "ADFAlertsSlice",
                "description": "One or more of hello data slices for hello Azure Data Factory has failed processing.",
                "isEnabled": true,
                "condition":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                    "dataSource":
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                        "operationName": "RunFinished",
                        "status": "Failed",
                        "subStatus": "FailedExecution"   
                    }
                },
                "action":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails": [ "<your alias>@contoso.com" ]
                }
            }
        }
    ]
}
```

Du kan ta bort **subStatus** från hello JSON-definitionen om du inte vill toobe ett meddelande om ett specifikt fel.

Det här exemplet konfigurerar hello varning för alla datafabriker i din prenumeration. Om du vill hello avisering toobe har ställts in för en viss datafabrik, kan du ange data factory **resourceUri** i hello **dataSource**:

```JSON
"resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"
```

hello innehåller följande tabell hello lista över tillgängliga åtgärder och status (och underordnad status).

| Åtgärdsnamn | Status | Substatus |
| --- | --- | --- |
| RunStarted |Igång |Startar |
| RunFinished |Misslyckades / lyckades |FailedResourceAllocation<br/><br/>Lyckades<br/><br/>FailedExecution<br/><br/>För lång tid<br/><br/>< avbröts<br/><br/>FailedValidation<br/><br/>Avbrutna |
| OnDemandClusterCreateStarted |Igång | |
| OnDemandClusterCreateSuccessful |Lyckades | |
| OnDemandClusterDeleted |Lyckades | |

Se [skapa Varningsregeln](https://msdn.microsoft.com/library/azure/dn510366.aspx) för ytterligare information om hello JSON-element som används i hello exempel.

#### <a name="deploy-hello-alert"></a>Distribuera hello avisering
toodeploy hello avisering, Använd hello Azure PowerShell-cmdleten **ny AzureRmResourceGroupDeployment**som visas i följande exempel hello:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  
```

När hello resurs distribution har slutförts visas hello följande meddelanden:

```
VERBOSE: 7:00:48 PM - Template is valid.
WARNING: 7:00:48 PM - hello StorageAccountName parameter is no longer used and will be removed in a future release.
Please update scripts tooremove this parameter.
VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded

DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

> [!NOTE]
> Du kan använda hello [Skapa regel för varning](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API toocreate en aviseringsregel. hello JSON-nyttolast är liknande toohello JSON-exempel.  


#### <a name="retrieve-hello-list-of-azure-resource-group-deployments"></a>Hämta hello lista över distributionen av Azure resursgrupper
tooretrieve hello lista över distribuerade distributionen av Azure resursgrupper använder hello cmdlet **Get-AzureRmResourceGroupDeployment**som visas i följande exempel hello:

```powershell
Get-AzureRmResourceGroupDeployment -ResourceGroupName adf
```

```
DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

#### <a name="troubleshoot-user-events"></a>Felsöka användarhändelser
1. Du kan se alla hello-händelser som genereras när du klickar på hello **mått och åtgärder** panelen.

    ![Panelen mått och åtgärder](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)
2. Klicka på hello **händelser** panelen toosee hello händelser.

    ![Panelen händelser](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. På hello **händelser** bladet hittar du information om händelser, filtreras händelser och så vidare.

    ![Händelser bladet](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. Klicka på en **åtgärden** i hello operations lista som orsakar ett fel.

    ![Välj en åtgärd](./media/data-factory-monitor-manage-pipelines/select-operation.png)
5. Klicka på en **fel** toosee händelseinformationen om hello felet.

    ![Händelsefel](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)

Se [Azure Insight cmdlets](https://msdn.microsoft.com/library/mt282452.aspx) för PowerShell-cmdlets som du kan använda tooadd, hämta eller ta bort aviseringar. Här följer några exempel på användning av hello **Get-AlertRule** cmdlet:

```powershell
get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
```

```
Properties :
Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
Condition   :
DataSource :
EventName             :
Category              :
Level                 :
OperationName         : RunFinished
ResourceGroupName     :
ResourceProviderName  :
ResourceId            :
Status                : Failed
SubStatus             : FailedExecution
Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
Condition      :
Description : One or more of hello data slices for hello Azure Data Factory has failed processing.
Status      : Enabled
Name:       : ADFAlertsSlice
Tags       :
$type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
Location   : West US
Name       : ADFAlertsSlice
```

```powershell
Get-AlertRule -res $resourceGroup
```
```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0

Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
Location   : West US
Name       : FailedExecutionRunsWest3
```

```powershell
Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
```

```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0
```

Kör hello följande get-help kommandon toosee information och exempel för hello Get-AlertRule cmdlet.

```powershell
get-help Get-AlertRule -detailed
```

```powershell
get-help Get-AlertRule -examples
```


Om du ser hello varningsgenereringen händelser på hello portalbladet men du ta emot inte e-postaviseringar, kontrollera om tooreceive e-post från externa avsändare anges hello e-postadress som har angetts. hello aviseringsmeddelanden kanske har blockerats av din e-postinställningar.

### <a name="alerts-on-metrics"></a>Aviseringar om mått
I Data Factory du samla in olika mått och skapa aviseringar på mått. Du kan övervaka och skapa aviseringar på följande mätvärden för hello segment i din data factory hello:

* **Misslyckade körs**
* **Lyckad körs**

De här måtten är användbara och hjälpa dig en översikt över övergripande misslyckade och lyckade körs i hello data factory tooget. Mått är orsakat varje gång det är ett segment kör. De här måtten aggregeras hello början av hello timme och pushas tooyour storage-konto. tooenable statistik, och skapa ett lagringskonto.

#### <a name="enable-metrics"></a>Aktivera mätvärden
tooenable statistik, och klicka på hello följande från hello **Data Factory** bladet:

**Övervaka** > **mått** > **diagnostikinställningar** > **diagnostik**

![Diagnostik-länk](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

På hello **diagnostik** bladet, klickar du på **på**, Välj hello lagringskonto och på **spara**.

![Diagnostik-bladet](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

Det kan ta upp tooone timme för hello mått toobe synliga på hello **övervakning** bladet eftersom mått aggregering sker varje timme.

### <a name="set-up-an-alert-on-metrics"></a>Skapa en avisering om mått
Klicka på hello **Data Factory mått** panelen:

![Data factory mått sida vid sida](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

På hello **mått** bladet, klickar du på **+ Lägg till avisering** hello i verktygsfältet.
![Data factory mått bladet > Lägg till avisering](./media/data-factory-monitor-manage-pipelines/add-alert.png)

På hello **lägga till en varningsregel** gör hello följande steg, och klicka på **OK**.

* Ange ett namn för aviseringen hello (exempel: ”misslyckades aviseringen”).
* Ange en beskrivning för hello varning (exempel: ”skicka ett e-postmeddelande när ett fel uppstår”).
* Markera ett mått (vs ”kunde inte körs”. ”Lyckades körs”).
* Ange ett villkor och ett tröskelvärde.   
* Ange hello tidsperiod.
* Ange om ett e-postmeddelande ska skickas tooowners, deltagare och läsare.

![Data factory mått bladet > Lägg till regel för varning](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

När hello varningsregeln har lagts till, hello blad stängs och du ser hello ny avisering på hello **mått** bladet.

![Data factory mått bladet > Ny avisering som lagts till](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

Du bör också se hello antal varningar i hello **Varna regler** panelen. Klicka på hello **Varna regler** panelen.

![Data factory mått-bladet – Varningsregler](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

På hello **aviseringar regler** bladet visas alla befintliga varningar. tooadd en avisering, klickar du på **Lägg till avisering** hello i verktygsfältet.

![Varningsregler bladet](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a>Varningsmeddelanden
När hello varningsregeln matchar hello villkor, bör du få ett e-postmeddelande som säger hello aviseringen har aktiverats. När hello problemet har lösts och hello varningsvillkor matchar inte längre kan få du ett e-postmeddelande som säger hello varningen har åtgärdats.

Det här skiljer sig händelser där skickas ett meddelande på alla fel som en aviseringsregel är berättigad till.

### <a name="deploy-alerts-by-using-powershell"></a>Distribuera aviseringar med hjälp av PowerShell
Du kan distribuera aviseringar för mått hello samma sätt som för händelser.

**Varningsdefinitionen**

```JSON
{
    "contentVersion" : "1.0.0.0",
    "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters" : {},
    "resources" : [
    {
            "name" : "FailedRunsGreaterThan5",
            "type" : "microsoft.insights/alertrules",
            "apiVersion" : "2014-04-01",
            "location" : "East US",
            "properties" : {
                "name" : "FailedRunsGreaterThan5",
                "description" : "Failed Runs greater than 5",
                "isEnabled" : true,
                "condition" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                    "dataSource" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                        "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                        "metricName" : "FailedRuns"
                    },
                    "threshold" : 5.0,
                    "windowSize" : "PT3H",
                    "timeAggregation" : "Total"
                },
                "action" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails" : ["abhinav.gpt@live.com"]
                }
            }
        }
    ]
}
```

Ersätt *subscriptionId*, *resourceGroupName*, och *dataFactoryName* i hello exemplet med lämpliga värden.

*metricName* stöder för närvarande två värden:

* FailedRuns
* SuccessfulRuns

**Distribuera hello avisering**

toodeploy hello avisering, Använd hello Azure PowerShell-cmdleten **ny AzureRmResourceGroupDeployment**som visas i följande exempel hello:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json
```

Du bör se följande meddelande efter en lyckad distribution:

```
VERBOSE: 12:52:47 PM - Template is valid.
VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded


DeploymentName    : FailedRunsGreaterThan5
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 7/27/2015 7:52:56 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           
```

Du kan också använda hello **Lägg till AlertRule** cmdlet toodeploy en aviseringsregel. Se hello [Lägg till AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) avsnittet information och exempel.  

## <a name="move-a-data-factory-tooa-different-resource-group-or-subscription"></a>Flytta en data factory tooa annan resursgrupp eller prenumeration
Du kan flytta data factory tooa olika resursgrupper eller en annan prenumeration med hjälp av hello **flytta** kommandot liggande hello startsidan i din data factory-knappen.

![Flytta data factory](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Du kan också flytta alla relaterade resurser (till exempel aviseringar som är associerade med hello data factory), tillsammans med hello data factory.

![Flytta dialogrutan resurser](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
