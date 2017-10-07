---
title: "aaaViewing diagnostikloggarna för Azure Data Lake Analytics | Microsoft Docs"
description: "Förstå hur toosetup och åtkomst diagnostiska loggar för Azure Data Lake analytics "
services: data-lake-analytics
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf5633d4-bc43-444e-90fc-f90fbd0b7935
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 4cd1eb6f585c1ef96c358340232ef85721a972b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Åtkomst till diagnostikloggarna för Azure Data Lake Analytics

Diagnostikloggning kan toocollect data granskningsspår från filåtkomstförsök. Dessa loggar finns information som:

* En lista över användare som har öppnat hello data.
* Hur ofta hello data används.
* Hur mycket data som lagras i hello-konto.

## <a name="enable-logging"></a>Aktivera loggning

1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. Öppna Data Lake Analytics-kontot och markera **diagnostikloggar** från hello __övervakaren__ avsnitt. Välj därefter __aktivera diagnostiken__.

    ![Aktivera diagnostik toocollect granskning och begära loggar](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. Från __diagnostikinställningarna__, ange hello status too__On__ och välja alternativ för loggning.

    ![Aktivera diagnostik toocollect granskning och begära loggar](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "aktivera diagnostikloggar")

   * Ange **Status** för**på** tooenable diagnostikloggning.

   * Du kan välja toostore/bearbeta hello data på tre olika sätt.

     * Välj __Arkivera tooa lagringskonto__ toostore loggar i ett Azure storage-konto. Använd det här alternativet om du vill tooarchive hello data. Om du väljer det här alternativet måste du ange ett Azure storage-konto toosave hello loggarna.

     * Välj **strömma tooan Händelsehubb** toostream logga data tooan Azure Event Hub. Använd det här alternativet om du har en pipeline för nedströms bearbetning som analysera inkommande loggarna i realtid. Om du väljer det här alternativet måste du ange hello information för hello Azure Event Hub du vill toouse.

     * Välj __skicka tooLog Analytics__ toosend hello data toohello logganalys-tjänsten. Använd det här alternativet om du vill toouse logganalys toogather och analysera loggfiler.
   * Ange om du vill tooget granskningsloggar eller begäran loggar eller båda.  En Begärandelogg samlar in varje API-begäran. En granskningslogg registrerar alla åtgärder som utlöses av denna API-begäran.

   * För __Arkivera tooa lagringskonto__, ange hello antal dagar tooretain hello data.

   * Klicka på __Spara__.

        > [!NOTE]
        > Du måste välja antingen __Arkivera tooa lagringskonto__, __strömma tooan Händelsehubb__ eller __skicka tooLog Analytics__ innan du klickar på hello __spara__knappen.

När du har aktiverat diagnostikinställningar kan du gå tillbaka toohello __diagnostik loggar__ bladet tooview hello loggar.

## <a name="view-logs"></a>Visa loggfiler

### <a name="use-hello-data-lake-analytics-view"></a>Använd hello Data Lake Analytics visa

1. Från Data Lake Analytics-kontot bladet under **övervakning**väljer **diagnostikloggar** och välj sedan en post toodisplay loggar för.

    ![Visa diagnostikloggning](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Visa diagnostikloggar")

2. hello loggar kategoriseras efter **granskningsloggar** och **begära loggar**.

    ![loggposter](./media/data-lake-analytics-diagnostic-logs/diagnostic-log-entries.png)

   * Begäran loggar avbilda alla API-begäranden som görs på hello Data Lake Analytics-konto.
   * Granskningsloggar är liknande toorequest loggar men ger en mycket mer detaljerad uppdelning av hello åtgärder. En enskild överföring API-anrop i loggen för begäran kan medföra flera ”tilläggsåtgärder” i dess granskningslogg.

3. Klicka på hello **hämta** länk för en logg post toodownload loggar.

### <a name="use-hello-azure-storage-account-that-contains-log-data"></a>Använd hello Azure Storage-konto som innehåller loggdata

1. Öppna hello Azure Storage-konto bladet som är associerade med Data Lake Analytics loggning och klicka sedan på __Blobbar__. Hej **Blob-tjänst** bladet visar två behållare.

    ![Visa diagnostikloggning](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Visa diagnostikloggar")

   * hello behållaren **insikter loggar granskning** innehåller hello granskningsloggar.
   * hello behållaren **insikter loggar begäran** innehåller hello begäran loggar.
2. I dessa behållare lagras hello loggar under hello följande struktur:

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json

   > [!NOTE]
   > Hej `##` posterna i hello sökvägen innehåller hello år, månad, dag och timme i vilka hello loggen skapades. Data Lake Analytics skapar en fil varje timme så `m=` alltid innehåller värdet `00`.

    Exempelvis kan hello fullständig sökväg tooan granskningsloggen vara:

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    På liknande sätt kan hello fullständig sökväg tooa loggen vara:

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Loggen struktur

hello är gransknings-och begäran i ett strukturerat JSON-format.

### <a name="request-logs"></a>Begäran loggar

Här är ett exempel i hello JSON-formaterad loggen. Varje blobb har en rotobjektet kallas **poster** som innehåller en matris med objekt.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Schemat för begäran-logg

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| time |Sträng |hello tidsstämpeln (i UTC) för hello logg |
| resourceId |Sträng |hello identifierare för hello-resurs som åtgärden tog placera på |
| category |Sträng |hello loggen kategori. Till exempel **begäranden**. |
| operationName |Sträng |Namnet på hello-åtgärd som loggas. Till exempel GetAggregatedJobHistory. |
| resultType |Sträng |hello status för hello åtgärden, till exempel 200. |
| callerIpAddress |Sträng |hello hello klientens hello begäran IP-adress |
| correlationId |Sträng |hello identifierare för hello-loggen. Det här värdet kan vara används toogroup en uppsättning relaterade loggposter. |
| identity |Objekt |hello-identitet som genererade hello logg |
| properties |JSON |Se nedan hello (begäran loggen egenskaper schema) mer information |

#### <a name="request-log-properties-schema"></a>Begäran loggen egenskaper schema

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| HttpMethod |Sträng |hello HTTP-metod som används för hello åtgärden. Till exempel få. |
| Sökväg |Sträng |hello sökväg hello åtgärden utfördes på |
| RequestContentLength |int |hello innehållslängden för hello HTTP-begäran |
| ClientRequestId |Sträng |hello-identifierare som unikt identifierar den här begäran |
| StartTime |Sträng |hello tid på vilka hello servern tar emot hello begäran |
| Sluttid |Sträng |hello tid vid vilken hello servern skickade ett svar |

### <a name="audit-logs"></a>Granskningsloggar

Här är ett exempel i hello JSON-formaterad granskningslogg. Varje blobb har en rotobjektet kallas **poster** som innehåller en matris med objekt.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job",
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Granska loggen schema

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| time |Sträng |hello tidsstämpeln (i UTC) för hello logg |
| resourceId |Sträng |hello identifierare för hello-resurs som åtgärden tog placera på |
| category |Sträng |hello loggen kategori. Till exempel **Audit**. |
| operationName |Sträng |Namnet på hello-åtgärd som loggas. Till exempel JobSubmitted. |
| resultType |Sträng |En substatus för jobbstatus för hello (operationName). |
| resultSignature |Sträng |Ytterligare information om hello jobbets status (operationName). |
| identity |Sträng |hello-användaren som begärde hello-åtgärd. Till exempel susan@contoso.com. |
| properties |JSON |Se nedan hello (Granska loggen egenskaper schema) mer information |

> [!NOTE]
> **resultType** och **resultSignature** innehåller information om hello resultatet av en åtgärd och bara innehålla ett värde om en åtgärd har slutförts. Till exempel som endast innehåller ett värde när **operationName** innehåller värdet **JobStarted** eller **JobEnded**.
>
>

#### <a name="audit-log-properties-schema"></a>Granska loggen egenskaper schema

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| JobId |Sträng |hello-ID tilldelade toohello jobb |
| Jobbnamn |Sträng |hello-namnet som angavs för hello jobb |
| JobRunTime |Sträng |hello runtime används tooprocess hello jobb |
| SubmitTime |Sträng |hello tid (i UTC) hello jobbet har skickats |
| StartTime |Sträng |hello hello jobbet började köras efter att (i UTC) |
| Sluttid |Sträng |hello hello jobbet avslutades |
| Parallellitet |Sträng |hello antalet Data Lake Analytics-enheter som krävs för det här jobbet under överföring |

> [!NOTE]
> **SubmitTime**, **StartTime**, **EndTime**, och **parallellitet** innehåller information om en åtgärd. Dessa poster endast innehåller ett värde om som åtgärden har startats eller slutförts. Till exempel **SubmitTime** endast innehåller ett värde efter **operationName** har hello värde **JobSubmitted**.

## <a name="process-hello-log-data"></a>Bearbeta hello loggdata

Azure Data Lake Analytics innehåller ett exempel på hur tooprocess och analysera hello loggdata. Du kan hitta hello exempelprogram på [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).

## <a name="next-steps"></a>Nästa steg
* [Översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md)
