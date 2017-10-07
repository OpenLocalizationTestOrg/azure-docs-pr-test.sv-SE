---
title: "aaaGet igång med Data Lake Analytics med hjälp av REST API | Microsoft Docs"
description: "Använd WebHDFS REST API: er tooperform åtgärder på Data Lake Analytics"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 5e133d92-baaa-44c9-890c-ab2d85c91122
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/03/2017
ms.author: jgao
ms.openlocfilehash: a0b13d521821fd2d74716cc52485585feb7c51b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Kom igång med Azure Data Lake Analytics med hjälp av REST API:er | Azure
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Lär dig hur toouse WebHDFS REST API: er och Data Lake Analytics REST API: er toomanage Data Lake Analytics-konton, jobb och katalogen. 

## <a name="prerequisites"></a>Krav
* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Skapa ett program i Azure Active Directory**. Du kan använda hello Azure AD application tooauthenticate hello Data Lake Analytics-program med Azure AD. Det finns olika sätt tooauthenticate med Azure AD, som är **slutanvändarens autentisering** eller **tjänst-till-tjänst autentisering**. Anvisningar och mer information om hur tooauthenticate, se [autentisera med Data Lake Analytics med hjälp av Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).
* [cURL](http://curl.haxx.se/). Den här artikeln använder cURL toodemonstrate hur toomake REST API-anrop mot ett Data Lake Analytics-konto.

## <a name="authenticate-with-azure-active-directory"></a>Autentisera med hjälp av Azure Active Directory
Det finns två metoder för hur du autentiserar med Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Slutanvändarautentisering (interaktiv)
Med den här metoden uppmanar programmet hello användaren toolog i och alla hello åtgärder utförs i hello hello användarens kontext. 

Följ de här stegen för interaktiv autentisering:

1. Omdirigera hello användaren toohello följande URL: en via ditt program:
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > \<OMDIRIGERINGS-URI > måste toobe kodad för användning i en URL. Alltså ska `https%3A%2F%2Flocalhost` användas för https://localhost)
   > 
   > 
   
    För hello syftet med den här självstudiekursen, du ersätta hello platshållarvärdena i URL: en för hello ovan och klistra in den i webbläsarens adressfält. Du kommer att omdirigerade tooauthenticate med Azure-autentiseringsuppgifter. När du har loggat in visas svaret hello i hello webbläsarens adressfält. hello svaret ska ha hello följande format:
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. Avbilda hello Auktoriseringskoden från hello svar. För den här självstudiekursen kan du kopiera hello Auktoriseringskoden från adressfältet i hello av hello webbläsare och skicka den i hello POST-begäran toohello tokenslutpunkten, enligt nedan:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > I det här fallet hello \<OMDIRIGERINGS-URI > måste inte vara kodad.
   > 
   > 
3. hello svaret är ett JSON-objekt som innehåller en åtkomsttoken (t.ex. `"access_token": "<ACCESS_TOKEN>"`) och en uppdateringstoken (t.ex. `"refresh_token": "<REFRESH_TOKEN>"`). Programmet använder hello åtkomsttoken vid åtkomst till Azure Data Lake Store och hello uppdatera token tooget en annan åtkomsttoken när en åtkomst-token upphör att gälla.
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. När hello åtkomst-token upphör att gälla, kan du begära en ny åtkomsttoken med hjälp av hello uppdateringstoken som visas nedan:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<CLIENT-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Mer information om interaktiv användarautentisering finns i [Flöde beviljat med auktoriseringskod](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Tjänst-till-tjänst-autentisering (icke-interaktivt)
Programmet tillhandahåller använder den här metoden egna autentiseringsuppgifter tooperform hello åtgärder. För att göra detta måste du skicka en POST-begäran som hello som visas nedan: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

hello utdata för begäran innehåller en autentiseringstoken (betecknas med `access-token` i hello utdata nedan) som du sedan skickar med REST API-anrop. Spara den här autentiseringstoken i en textfil; du behöver den senare i den här artikeln.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Den här artikeln använder hello **icke-interaktiv** metod. Mer information om icke-interaktivt (service to service anrop) finns [tjänsten tooservice anrop med autentiseringsuppgifter](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-analytics-account"></a>Skapa ett Data Lake Analytics-konto
Du måste skapa en Azure-resursgrupp och ett Data Lake Store-konto innan du kan skapa ett Data Lake Analytics-konto.  Se [Skapa ett Data Lake Store-konto](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

Hej följande Curl-kommando visar hur toocreate ett konto:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Ersätt \< `REDACTED` \> med hello autentiseringstoken \< `AzureSubscriptionID` \> med ditt prenumerations-ID \< `AzureResourceGroupName` \> med en befintlig Azure-resurs Gruppnamn och \< `NewAzureDataLakeAnalyticsAccountName` \> med ett nytt namn för Data Lake Analytics-konto. Hej begärannyttolast för det här kommandot finns i hello **CreateDatalakeAnalyticsAccountRequest.json** fil som har angetts för hello `-d` parametern ovan. hello innehållet i input.json-filen för hello likna hello följande:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Lista över Data Lake Analytics-konton i en prenumeration
hello följande Curl-kommando visar hur toolist konton i en prenumeration:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Ersätt \< `REDACTED` \> med hello autentiseringstoken \< `AzureSubscriptionID` \> med ditt prenumerations-ID. hello utdata liknar:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Hämta information om ett Data Lake Analytics-konto
Hej följande Curl-kommando visar hur tooget en kontoinformation:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Ersätt \< `REDACTED` \> med hello autentiseringstoken \< `AzureSubscriptionID` \> med ditt prenumerations-ID \< `AzureResourceGroupName` \> med en befintlig Azure-resurs Gruppnamn och \< `DataLakeAnalyticsAccountName` \> med hello namnet på ett befintligt Data Lake Analytics-konto. hello utdata liknar:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Ange Data Lake Stores för ett Data Lake Analytics-konto i en lista
hello följande Curl-kommando visar hur toolist Data Lake lagrar för ett konto:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Ersätt \< `REDACTED` \> med hello autentiseringstoken \< `AzureSubscriptionID` \> med ditt prenumerations-ID \< `AzureResourceGroupName` \> med en befintlig Azure-resurs Gruppnamn och \< `DataLakeAnalyticsAccountName` \> med hello namnet på ett befintligt Data Lake Analytics-konto. hello utdata liknar:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>Skicka U-SQL-jobb
Hej följande Curl-kommando visar hur toosubmit U-SQL-jobb:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Ersätt \< `REDACTED` \> med hello autentiseringstoken \< `DataLakeAnalyticsAccountName` \> med hello namnet på ett befintligt Data Lake Analytics-konto. Hej begärannyttolast för det här kommandot finns i hello **SubmitADLAJob.json** fil som har angetts för hello `-d` parametern ovan. hello innehållet i input.json-filen för hello likna hello följande:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    too\"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

hello utdata liknar:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>Lista U-SQL-jobb
Hej följande Curl-kommando visar hur toolist U-SQL-jobb:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Ersätt \< `REDACTED` \> med hello autentiseringstoken och \< `DataLakeAnalyticsAccountName` \> med hello namnet på ett befintligt Data Lake Analytics-konto. 

hello utdata liknar:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Hämta katalogobjekt
hello följande Curl-kommando visar hur tooget hello databaser från hello katalog:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

hello utdata liknar:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Se även
* toosee en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* tooget utveckla U-SQL-program, se [utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* toolearn U-SQL finns [Kom igång med Azure Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md).
* Information om hanteringsuppgifter finns i [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md).
* tooget en översikt över Data Lake Analytics finns [översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).
* toosee hello samma självstudier med andra verktyg klickar du på hello flikväljarna överst hello hello-sidan.

