---
title: aaaArchive Azure diagnostikloggar | Microsoft Docs
description: "Lär dig hur tooarchive din Azure diagnostikloggar för långsiktig kvarhållning i ett lagringskonto."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 3a55c73f-2ef3-45f3-8956-bcf9c0cb7e05
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: bc9edbd3a649023a728b7fe77130dba2b6e6370d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="archive-azure-diagnostic-logs"></a>Arkivera Azure diagnostikloggar
I den här artikeln visar vi hur du kan använda hello Azure-portalen PowerShell-Cmdlets, CLI eller REST API tooarchive din [Azure diagnostikloggar](monitoring-overview-of-diagnostic-logs.md) i ett lagringskonto. Det här alternativet är användbart om du vill att tooretain diagnostikloggar med ett valfritt bevarandeprincipen för granskning, statiska analys eller säkerhetskopiering. hello storage-konto har inte toobe i hello samma prenumeration som hello resurs avger loggar så länge hello användare som konfigurerar hello inställningen har lämplig RBAC åtkomst tooboth prenumerationer.

## <a name="prerequisites"></a>Krav
Innan du börjar behöver du för[skapa ett lagringskonto](../storage/storage-create-storage-account.md) toowhich som du kan arkivera dina diagnostikloggar. Vi rekommenderar starkt att du inte använder ett befintligt lagringskonto som har andra, icke-övervakning data som lagras i den så att du bättre kan styra åtkomst till toomonitoring data. Men om du arkiverar även dina aktivitetsloggen och diagnostik mått tooa storage-konto, kan det vara klokt toouse detta lagringskonto för dina diagnostikloggar samt tookeep alla övervakningsdata på en central plats. hello storage-konto du använder måste vara ett allmänt lagringskonto inte ett blob storage-konto.

## <a name="diagnostic-settings"></a>Diagnostikinställningar
tooarchive dina diagnostikloggar med hjälp av hello metoderna nedan, som du ställer in en **diagnostikinställningen** för en viss resurs. Diagnostikinställningen för en resurs definierar hello kategorier av loggar och mätvärden skickas tooa destination (storage-konto, Händelsehubbar namnområde eller logganalys). Den definierar även hello bevarandeprincip (antal dagar tooretain) för händelser för varje logg kategori och mått data som lagras i ett lagringskonto. Om en bevarandeprincip anges toozero lagras händelser för log kategorin på obestämd tid (d.v.s. toosay, oändligt). En bevarandeprincip kan annars vara valfritt antal dagar mellan 1 och 2147483647. [Du kan läsa mer om diagnostikinställningar här](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings). Bevarandeprinciper är tillämpade per dag, så vid hello slutet på dagen (UTC) loggar från hello dag som inte har nu hello bevarandeprincip kommer att tas bort. Till exempel om du har en bevarandeprincip på en dag, skulle hello början av hello dagen idag hello loggar från hello dag före igår tas bort

## <a name="archive-diagnostic-logs-using-hello-portal"></a>Arkivera diagnostikloggar med hello-portalen
1. Navigera tooAzure Övervakare i hello-portalen och klicka på **diagnostikinställningar**

    ![Avsnittet av Azure-Monitor övervakning](media/monitoring-archive-diagnostic-logs/diagnostic-settings-blade.png)

2. Du kan också hello-filterlista av resursgruppen eller resursen, och klicka sedan på hello resursen som du vill att tooset en diagnostikinställningen.

3. Om det finns inga inställningar på hello resursen som du har valt, kan du ange toocreate en inställning. Klicka på ”Aktivera diagnostik”.

   ![Lägg till diagnostikinställningen - inga befintliga inställningar](media/monitoring-archive-diagnostic-logs/diagnostic-settings-none.png)

   Om det finns befintliga inställningarna på hello resurs, visas en lista över inställningar som redan har konfigurerats på den här resursen. Klicka på ”Lägg till diagnostikinställningen”.

   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. Ge ange ett namn och kryssrutan för hello **exportera tooStorage konto**, Välj ett lagringskonto. Du kan också ange ett antal dagar tooretain loggarna för med hello **bevarande (dagar)** skjutreglagen. En kvarhållning av noll dagar lagrar hello loggar under obestämd tid.
   
   ![Lägg till diagnostikinställningen - befintliga inställningar](media/monitoring-archive-diagnostic-logs/diagnostic-settings-configure.png)
    
4. Klicka på **Spara**.

Efter en liten stund hello nya inställningen som visas i din lista över inställningar för den här resursen och diagnostikloggar är arkiverade toothat lagring kontot så snart nya händelsedata genereras.

## <a name="archive-diagnostic-logs-via-azure-powershell"></a>Arkivera diagnostikloggar via Azure PowerShell
```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Egenskap | Krävs | Beskrivning |
| --- | --- | --- |
| Resurs-ID |Ja |Resurs-ID för hello resursen som du vill tooset en diagnostikinställningen. |
| StorageAccountId |Nej |Resurs-ID för hello Lagringskonto toowhich diagnostikloggar ska sparas. |
| Kategorier |Nej |Kommaavgränsad lista över loggen kategorier tooenable. |
| Enabled |Ja |Booleskt värde som anger om diagnostik är aktiverade eller inaktiverade på den här resursen. |
| RetentionEnabled |Nej |Booleskt värde som anger om en bevarandeprincip är aktiverade på den här resursen. |
| retentionInDays |Nej |Antal dagar som händelser ska behållas mellan 1 och 2147483647. Värdet noll lagrar hello loggar under obestämd tid. |

## <a name="archive-diagnostic-logs-via-hello-cross-platform-cli"></a>Arkivera diagnostikloggar via hello plattformsoberoende CLI
```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Egenskap | Krävs | Beskrivning |
| --- | --- | --- |
| resourceId |Ja |Resurs-ID för hello resursen som du vill tooset en diagnostikinställningen. |
| storageId |Nej |Resurs-ID för Lagringskontot hello toowhich diagnostikloggar ska sparas. |
| Kategorier |Nej |Kommaavgränsad lista över loggen kategorier tooenable. |
| aktiverad |Ja |Booleskt värde som anger om diagnostik är aktiverade eller inaktiverade på den här resursen. |

## <a name="archive-diagnostic-logs-via-hello-rest-api"></a>Arkivera diagnostikloggar via hello REST API
[Det här dokumentet finns](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) information om hur du ställer in en diagnostikinställningen med hello Azure övervakaren REST API.

## <a name="schema-of-diagnostic-logs-in-hello-storage-account"></a>Schemat för diagnostikloggar i hello storage-konto
När du har konfigurerat arkivering, skapas en lagringsbehållare i hello storage-konto när en händelse inträffar i en av hello loggen kategorier som du har aktiverat. Hej blobbar i behållaren hello följa hello samma format över diagnostikloggar och hello aktivitetsloggen. hello strukturen för de här blobbar är:

> insikter - loggar-{kategori loggnamn} / resourceId = / PRENUMERATIONER / {prenumerations-ID} /RESOURCEGROUPS/ {resursgruppens namn} /PROVIDERS/ {resurs providernamn} / {resurstyp} / {resursnamn} / y = {numeriskt årtal} / m = {tvåsiffrig numeriska month} / d = {tvåsiffrig kalenderdag} / tim = {tvåsiffrig 24-timmarsklocka hour}/m=00/PT1H.json
> 
> 

Eller fler bara

> insikter - loggar-{kategori loggnamn} / resourceId = / {resursens Id} / y = {numeriskt årtal} / m = {tvåsiffrig numeriska month} / d = {tvåsiffrig kalenderdag} / tim = {tvåsiffrig 24-timmarsklocka hour}/m=00/PT1H.json
> 
> 

Till exempel kan en blobbnamnet vara:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
> 
> 

Varje PT1H.json blobb innehåller en JSON-blob av händelser som inträffade inom hello timme som anges i hello blob-URL (till exempel h = 12). Under hello finns timme är händelser tillagda toohello PT1H.json filen när de inträffar. Hej minuten (m = 00) är alltid 00, eftersom diagnostiska logghändelser delas upp i enskilda blobbar per timme.

I hello PT1H.json filen lagras varje händelse i hello ”innehåller” matris, efter det här formatet:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Elementnamn | Beskrivning |
| --- | --- |
| time |Tidsstämpel när hello händelse har genererats av hello Azure service bearbetning hello begära motsvarande hello-händelse. |
| resourceId |Resurs-ID för hello påverkas resurs. |
| operationName |Namnet på hello igen. |
| category |Loggen kategori hello-händelse. |
| properties |En uppsättning `<Key, Value>` par (d.v.s. ordlista) som beskriver hello information om hello-händelse. |

> [!NOTE]
> hello egenskaper och användning av dessa egenskaper kan variera beroende på hello resurs.
> 
> 

## <a name="next-steps"></a>Nästa steg
* [Ladda ned blobbar för analys](../storage/storage-dotnet-how-to-use-blobs.md)
* [Dataströmmen diagnostiska loggar tooan Händelsehubbar namnområde](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Läs mer om diagnostikloggar](monitoring-overview-of-diagnostic-logs.md)
