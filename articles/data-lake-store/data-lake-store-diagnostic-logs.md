---
title: "aaaViewing diagnostikloggarna för Azure Data Lake Store | Microsoft Docs"
description: "Förstå hur toosetup och åtkomst till diagnostikloggarna för Azure Data Lake Store "
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: f6e75eb1-d0ae-47cf-bdb8-06684b7c0a94
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 11fbf7f517f97abdcaf809c1ebeeb51424ab2c1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Åtkomst till diagnostikloggarna för Azure Data Lake Store
Läs mer om hur tooenable diagnostikloggning för ditt Data Lake Store-konto och hur tooview hello loggar samlas in för ditt konto.

Organisationer kan aktivera diagnostikloggning för sina Azure Data Lake Store konto toocollect data granskningsspår från filåtkomstförsök som ger information, till exempel listan över användare som ansluter till hello data, hur ofta hello data används, hur mycket data lagras i hello konto, osv.

## <a name="prerequisites"></a>Krav
* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Store-konto**. Följ instruktionerna för hello på [Kom igång med Azure Data Lake Store med hjälp av hello Azure Portal](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Aktivera diagnostikloggning för ditt Data Lake Store-konto
1. Logga in på nytt toohello [Azure Portal](https://portal.azure.com).
2. Öppna Data Lake Store-konto och ditt Data Lake Store-kontoblad klickar du på **inställningar**, och klicka sedan på **diagnostikloggar**.
3. I hello **diagnostik loggar** bladet, klickar du på **aktivera diagnostiken**.

    ![Aktivera diagnostikloggning](./media/data-lake-store-diagnostic-logs/turn-on-diagnostics.png "aktivera diagnostikloggar")

3. I hello **diagnostiska** bladet gör följande ändringar tooconfigure diagnostikloggning hello.
   
    ![Aktivera diagnostikloggning](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "aktivera diagnostikloggar")
   
   * Ange **Status** för**på** tooenable diagnostikloggning.
   * Du kan välja toostore/bearbeta hello data på olika sätt.
     
        * Välj alternativet för hello för**Arkivera tooa lagringskonto** toostore loggar tooan Azure Storage-konto. Du kan använda det här alternativet om du vill tooarchive hello data som ska bearbetas i batch vid ett senare tillfälle. Om du väljer det här alternativet måste du ange ett Azure Storage-konto toosave hello loggarna.
        
        * Välj alternativet för hello för**dataströmmen tooan händelsehubb** toostream logga data tooan Azure Event Hub. Du kommer troligen använda det här alternativet om du har en nedströms bearbetning pipeline tooanalyze inkommande loggar i realtid. Om du väljer det här alternativet måste du ange hello information för hello Azure Event Hub du vill toouse.

        * Välj alternativet för hello för**skicka tooLog Analytics** toouse hello Azure logganalys tooanalyze hello genereras tjänstlogginformation. Om du väljer det här alternativet måste du ange hello information för hello Operations Management Suite-arbetsyta som du skulle använda hello utföra logganalys.
     
   * Ange om du vill tooget granskningsloggar eller begäran loggar eller båda.
   * Ange hello antal dagar som hello data måste behållas. Kvarhållning gäller endast om du använder Azure storage-konto tooarchive loggdata.
   * Klicka på **Spara**.

När du har aktiverat diagnostikinställningar, kan du titta på hello loggar in hello **diagnostikloggar** fliken.

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>Visa diagnostikloggar för ditt Data Lake Store-konto
Det finns två sätt tooview hello loggdata för ditt Data Lake Store-konto.

* Inställningar för Visa från hello Data Lake Store-konto
* Från hello Azure Storage-konto som innehåller hello-data

### <a name="using-hello-data-lake-store-settings-view"></a>Med hjälp av hello Visa inställningar för Data Lake Store
1. Från ditt Data Lake Store-konto **inställningar** bladet, klickar du på **diagnostikloggar**.
   
    ![Visa diagnostikloggning](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Visa diagnostikloggar") 
2. I hello **diagnostikloggar** bladet bör du se hello loggar kategoriserade efter **granskningsloggar** och **begära loggar**.
   
   * Begäran loggar avbilda alla API-begäranden som görs på hello Data Lake Store-konto.
   * Granskningsloggar är liknande toorequest loggar men ger en mycket mer detaljerad uppdelning av hello åtgärder som utförs på hello Data Lake Store-konto. En enskild överföring API-anrop i begäran loggar kan exempelvis resultera i flera ”tilläggsåtgärder” i hello granskningsloggar.
3. Klicka på hello **hämta** länken mot varje logga post toodownload hello loggar.

### <a name="from-hello-azure-storage-account-that-contains-log-data"></a>Från hello Azure Storage-konto innehåller som loggdata
1. Öppna hello Azure Storage-konto bladet som är associerade med Data Lake Store loggning och klicka sedan på Blobbar. Hej **Blob-tjänst** bladet visar två behållare.
   
    ![Visa diagnostikloggning](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Visa diagnostikloggar")
   
   * hello behållaren **insikter loggar granskning** innehåller hello granskningsloggar.
   * hello behållaren **insikter loggar begäran** innehåller hello begäran loggar.
2. I dessa behållare lagras hello loggar under hello följande struktur.
   
    ![Visa diagnostikloggning](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Visa diagnostikloggar")
   
    Exempelvis kunde hello fullständig sökväg tooan granskningsloggen`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`
   
    Similary, hello fullständig sökväg tooa loggen kan vara`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-hello-structure-of-hello-log-data"></a>Förstå hello strukturen för hello loggdata
hello finns gransknings-och begäran i en JSON-format. I det här avsnittet ska vi titta på hello struktur JSON för begäran och granskningsloggar.

### <a name="request-logs"></a>Begäran loggar
Här är ett exempel i hello JSON-formaterad loggen. Varje blobb har en rotobjektet kallas **poster** som innehåller en matris med objekt.

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Schemat för begäran-logg
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| time |Sträng |hello tidsstämpeln (i UTC) för hello logg |
| resourceId |Sträng |hello-ID för hello-resurs som åtgärden tog placera på |
| category |Sträng |hello loggen kategori. Till exempel **begäranden**. |
| operationName |Sträng |Namnet på hello-åtgärd som loggas. Till exempel getfilestatus. |
| resultType |Sträng |hello status för hello åtgärden, till exempel 200. |
| callerIpAddress |Sträng |hello hello klientens hello begäran IP-adress |
| correlationId |Sträng |hello-id för hello-logg som kan användas toogroup tillsammans en uppsättning relaterade loggposter |
| identity |Objekt |hello-identitet som genererade hello logg |
| properties |JSON |Mer information finns nedan |

#### <a name="request-log-properties-schema"></a>Begäran loggen egenskaper schema
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| HttpMethod |Sträng |hello HTTP-metod som används för hello åtgärden. Till exempel få. |
| Sökväg |Sträng |hello sökväg hello åtgärden utfördes på |
| RequestContentLength |int |hello innehållslängden för hello HTTP-begäran |
| ClientRequestId |Sträng |hello-Id som unikt identifierar den här begäran |
| StartTime |Sträng |hello tid på vilka hello servern tar emot hello begäran |
| Sluttid |Sträng |hello tid vid vilken hello servern skickade ett svar |

### <a name="audit-logs"></a>Granskningsloggar
Här är ett exempel i hello JSON-formaterad granskningslogg. Varje blobb har en rotobjektet kallas **poster** som innehåller en uppsättning objekt

    {
    "records": 
      [        
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Granska loggen schema
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| time |Sträng |hello tidsstämpeln (i UTC) för hello logg |
| resourceId |Sträng |hello-ID för hello-resurs som åtgärden tog placera på |
| category |Sträng |hello loggen kategori. Till exempel **Audit**. |
| operationName |Sträng |Namnet på hello-åtgärd som loggas. Till exempel getfilestatus. |
| resultType |Sträng |hello status för hello åtgärden, till exempel 200. |
| correlationId |Sträng |hello-id för hello-logg som kan användas toogroup tillsammans en uppsättning relaterade loggposter |
| identity |Objekt |hello-identitet som genererade hello logg |
| properties |JSON |Mer information finns nedan |

#### <a name="audit-log-properties-schema"></a>Granska loggen egenskaper schema
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| StreamName |Sträng |hello sökväg hello åtgärden utfördes på |

## <a name="samples-tooprocess-hello-log-data"></a>Exempel tooprocess hello loggdata
Azure Data Lake Store ger ett exempel på hur tooprocess och analysera hello loggdata. Du kan hitta hello exempelprogram på [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 

## <a name="see-also"></a>Se även
* [Översikt över Azure Data Lake Store](data-lake-store-overview.md)
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)

