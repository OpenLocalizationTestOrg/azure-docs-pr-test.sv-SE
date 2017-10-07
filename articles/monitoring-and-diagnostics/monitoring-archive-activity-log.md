---
title: aaaArchive hello Azure-aktivitetsloggen | Microsoft Docs
description: "Lär dig hur tooarchive din Azure aktivitet loggar för långsiktig kvarhållning i ett lagringskonto."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: johnkem
ms.openlocfilehash: 58c6d3a3a31398287f66f76999d48f2942ab5109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="archive-hello-azure-activity-log"></a>Arkivera hello Azure-aktivitetsloggen
I den här artikeln visar vi hur du kan använda hello Azure-portalen, PowerShell-Cmdlets och plattformsoberoende CLI tooarchive din [ **Azure-aktivitetsloggen** ](monitoring-overview-activity-logs.md) i ett lagringskonto. Det här alternativet är användbart om du vill att tooretain din aktivitetsloggen som är längre än 90 dagar (med fullständig kontroll över hello bevarandeprincip) för granska statiska analys eller säkerhetskopiering. Om du bara behöver tooretain behöver inte händelserna i 90 dagar eller mindre du tooset in arkivering tooa lagringskontot eftersom aktivitetsloggen händelser är kvar i hello Azure-plattformen i 90 dagar utan att aktivera arkivering.

## <a name="prerequisites"></a>Krav
Innan du börjar behöver du för[skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) toowhich som du kan arkivera dina aktivitetsloggen. Vi rekommenderar starkt att du inte använder ett befintligt lagringskonto som har andra, icke-övervakning data som lagras i den så att du bättre kan styra åtkomst till toomonitoring data. Men om du även arkiverar diagnostiska loggar och mått tooa storage-konto, kan det vara meningsfullt toouse detta lagringskonto för dina aktiviteter logga samt tookeep alla övervakningsdata på en central plats. hello storage-konto du använder måste vara ett allmänt lagringskonto inte ett blob storage-konto. hello storage-konto har inte toobe i hello samma prenumeration som hello prenumeration avger loggar så länge hello användare som konfigurerar hello inställningen har lämplig RBAC åtkomst tooboth prenumerationer.

## <a name="log-profile"></a>Log-profil
tooarchive hello aktivitetsloggen med hjälp av hello metoderna nedan, ange hello **loggen profil** för en prenumeration. hello loggen profil definierar hello typ av händelser som lagras eller strömmas och hello utdata – lagring konto och/eller event hub. Den definierar även hello bevarandeprincip (antal dagar tooretain) för händelser som lagras i ett lagringskonto. Om hello bevarandeprincip anges toozero lagras händelser på obestämd tid. Detta kan annars ställas in tooany värde mellan 1 och 2147483647. Bevarandeprinciper är tillämpade per dag, så vid hello slutet på dagen (UTC) loggar från hello dag som inte har nu hello bevarandeprincip kommer att tas bort. Till exempel om du har en bevarandeprincip på en dag, skulle hello början av hello dagen idag hello loggar från hello dag före igår tas bort. [Du kan läsa mer om loggen profiler här](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile). 

## <a name="archive-hello-activity-log-using-hello-portal"></a>Arkivera hello aktivitetsloggen med hello-portalen
1. I hello-portalen klickar du på hello **aktivitetsloggen** länk på hello vänstra navigeringsfönstret. Om du inte ser en länk för hello aktivitetsloggen, klickar du på hello **fler tjänster** länka först.
   
    ![Navigera tooActivity loggen bladet](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. Hello överkant hello-bladet, klickar du på **exportera**.
   
    ![Klicka på hello Export](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. Hello-bladet som visas kryssrutan hello för **exportera lagringskontot tooa** och välj ett lagringskonto.
   
    ![Ange ett lagringskonto](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Använder hello skjutreglaget eller textruta, definiera ett antal dagar som aktiviteten logghändelser ska behållas i ditt lagringskonto. Om du föredrar toohave data kvar i hello storage-konto på obestämd tid genom att ange det här antalet toozero.
5. Klicka på **Spara**.

## <a name="archive-hello-activity-log-via-powershell"></a>Arkivera hello aktivitetsloggen via PowerShell
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Egenskap | Krävs | Beskrivning |
| --- | --- | --- |
| StorageAccountId |Nej |Resurs-ID för hello Lagringskonto toowhich aktivitetsloggar ska sparas. |
| Platser |Ja |Kommaavgränsad lista över regioner som du vill att toocollect aktivitetsloggen händelser. Du kan visa en lista över alla regioner [genom att besöka sidan](https://azure.microsoft.com/en-us/regions) eller genom att använda [hello Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| retentionInDays |Ja |Antal dagar för vilka händelser som ska behållas, mellan 1 och 2147483647. Värdet noll lagrar hello loggar på obestämd tid (alltid). |
| Kategorier |Ja |Kommaavgränsad lista över kategorier som ska samlas in. Möjliga värden är skriva och ta bort åtgärd. |

## <a name="archive-hello-activity-log-via-cli"></a>Arkivera hello aktivitetsloggen via CLI
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Egenskap | Krävs | Beskrivning |
| --- | --- | --- |
| namn |Ja |Namnet på loggen profilen. |
| storageId |Nej |Resurs-ID för hello Lagringskonto toowhich aktivitetsloggar ska sparas. |
| Platser |Ja |Kommaavgränsad lista över regioner som du vill att toocollect aktivitetsloggen händelser. Du kan visa en lista över alla regioner [genom att besöka sidan](https://azure.microsoft.com/en-us/regions) eller genom att använda [hello Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| retentionInDays |Ja |Antal dagar för vilka händelser som ska behållas, mellan 1 och 2147483647. Värdet noll lagrar hello loggar på obestämd tid (alltid). |
| Kategorier |Ja |Kommaavgränsad lista över kategorier som ska samlas in. Möjliga värden är skriva och ta bort åtgärd. |

## <a name="storage-schema-of-hello-activity-log"></a>Schemat för lagring av hello aktivitetsloggen
När du har konfigurerat arkivering, skapas en lagringsbehållare i hello storage-konto så snart aktivitetsloggen händelse inträffar. Hej blobbar i behållaren hello Följ hello samma format över hello aktivitetsloggen och diagnostikloggar. hello strukturen för de här blobbar är:

> insikter-operativa-loggar/name = standard/resourceId = / PRENUMERATIONER / {prenumerations-ID} / y = {numeriskt årtal} / m = {tvåsiffrig numeriska month} / d = {tvåsiffrig kalenderdag} / tim = {tvåsiffrig 24-timmarsklocka hour}/m=00/PT1H.json
> 
> 

Till exempel kan en blobbnamnet vara:

> Insights-Operational-Logs/Name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON
> 
> 

Varje PT1H.json blobb innehåller en JSON-blob av händelser som inträffade inom hello timme som anges i hello blob-URL (t.ex. h = 12). Under hello finns timme är händelser tillagda toohello PT1H.json filen när de inträffar. Hej minuten (m = 00) är alltid 00, eftersom aktiviteten logghändelser delas upp i enskilda blobbar per timme.

I hello PT1H.json filen lagras varje händelse i hello ”innehåller” matris, efter det här formatet:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
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
| category |Kategori av hello åtgärder, t.ex. Skrivning, Läs, åtgärden. |
| resultType |Hej typ av hello resultat, t.ex. Lyckades, fel, Start |
| resultSignature |Beror på hello resurstypen. |
| durationMs |Varaktighet för hello-åtgärden i millisekunder |
| callerIpAddress |IP-adressen för hello-användare som har utfört hello operation, UPN-anspråk eller SPN-anspråk baserat på tillgänglighet. |
| correlationId |Vanligtvis ett GUID i hello-strängformat. Händelser som delar en correlationId tillhör toohello samma uber åtgärd. |
| identity |JSON-blob som beskriver hello auktoriserings- och anspråk. |
| Auktorisering |BLOB RBAC egenskaper för hello-händelse. Normalt innehåller hello ””, ”roll” och ”omfattning” egenskaper. |
| nivå |Nivå av hello-händelse. En av hello följande värden: ”kritiska”, ”Error”, ”varning”, ”information” och ”utförlig” |
| location |Region i vilken hello plats uppstod (eller global). |
| properties |En uppsättning `<Key, Value>` par (d.v.s. ordlista) som beskriver hello information om hello-händelse. |

> [!NOTE]
> hello egenskaper och användning av dessa egenskaper kan variera beroende på hello resurs.
> 
> 

## <a name="next-steps"></a>Nästa steg
* [Ladda ned blobbar för analys](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
* [Strömma hello aktivitetsloggen tooEvent hubbar](monitoring-stream-activity-logs-event-hubs.md)
* [Läs mer om hello aktivitetsloggen](monitoring-overview-activity-logs.md)

