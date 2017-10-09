---
title: "aaaUse hello REST API tooget igång med Data Lake Store | Microsoft Docs"
description: "Använd WebHDFS REST API: er tooperform åtgärder på Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.openlocfilehash: 62fce8293dfee730a61f2a3d37fc138ce7c3afdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Kom igång med Azure Data Lake Store med hjälp av REST API:er
> [!div class="op_single_selector"]
> * [Portalen](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

I den här artikeln får du lära dig hur toouse WebHDFS REST API: er och Data Lake Store REST API: er tooperform konto management samt filsystemsåtgärder på Azure Data Lake Store. Azure Data Lake Store visar sina egna REST API:er för kontohanteringsåtgärder. Men eftersom Data Lake Store är kompatibelt med HDFS och Hadoop-ekosystemet, stöder det användning av WebHDFS REST API:er för filsystemsåtgärder.

> [!NOTE]
> Detaljerad information om hello REST API-stöd för Data Lake Store finns [Azure Data Lake Store REST API-referens](https://msdn.microsoft.com/library/mt693424.aspx).
> 
> 

## <a name="prerequisites"></a>Krav
* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Skapa ett program i Azure Active Directory**. Du kan använda hello Azure AD application tooauthenticate hello Data Lake Store-program med Azure AD. Det finns olika sätt tooauthenticate med Azure AD, som är **slutanvändarens autentisering** eller **tjänst-till-tjänst autentisering**. Anvisningar och mer information om hur tooauthenticate, se [slutanvändarens autentisering](data-lake-store-end-user-authenticate-using-active-directory.md) eller [tjänst-till-tjänst autentisering](data-lake-store-authenticate-using-active-directory.md).
* [cURL](http://curl.haxx.se/). Den här artikeln använder cURL toodemonstrate hur toomake REST API-anrop mot ett Data Lake Store-konto.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hur autentiserar jag med Azure Active Directory?
Du kan använda två metoder tooauthenticate med Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Slutanvändarautentisering (interaktiv)
I det här scenariot uppmanar hello programmet hello användaren toolog i och alla hello åtgärder utförs i hello hello användarens kontext. Utför följande steg för interaktiv autentisering hello.

1. Omdirigera hello användaren toohello följande URL: en via ditt program:
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > \<OMDIRIGERINGS-URI > måste toobe kodad för användning i en URL. Alltså ska `https%3A%2F%2Flocalhost` användas för https://localhost)
   > 
   > 
   
    För hello syftet med den här självstudiekursen, du ersätta hello platshållarvärdena i URL: en för hello ovan och klistra in den i webbläsarens adressfält. Du kommer att omdirigerade tooauthenticate med Azure-autentiseringsuppgifter. När du har loggar in visas svaret hello i hello webbläsarens adressfält. hello svaret ska ha hello följande format:
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. Avbilda hello Auktoriseringskoden från hello svar. För den här självstudiekursen kan du kopiera hello Auktoriseringskoden från adressfältet i hello av hello webbläsare och skicka den i hello POST-begäran toohello tokenslutpunkten, enligt nedan:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<APPLICATION-ID> \
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
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Mer information om interaktiv användarautentisering finns i [Flöde beviljat med auktoriseringskod](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Tjänst-till-tjänst-autentisering (icke-interaktivt)
I det här scenariot hello hello programmet tillhandahåller egna autentiseringsuppgifter tooperform hello åtgärder. För att göra detta måste du skicka en POST-begäran som hello som visas nedan. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

hello utdata för begäran innehåller en autentiseringstoken (betecknas med `access-token` i hello utdata nedan) som du sedan skickar med REST API-anrop. Spara den här autentiseringstoken i en textfil; du behöver den senare i den här artikeln.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Den här artikeln använder hello **icke-interaktiv** metod. Mer information om icke-interaktivt (service to service anrop) finns [tjänsten tooservice anrop med autentiseringsuppgifter](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-store-account"></a>Skapa ett Data Lake Store-konto
Den här åtgärden är baserad på hello REST API-anrop som definierats [här](https://msdn.microsoft.com/library/mt694078.aspx).

Använd hello följande cURL-kommando. Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

I hello ovanstående kommando ersätter \< `REDACTED` \> med hello autentiseringstoken som hämtats tidigare. Hej begärannyttolast för det här kommandot finns i hello **input.json** fil som har angetts för hello `-d` parametern ovan. hello innehållet i input.json-filen för hello likna hello följande:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="create-folders-in-a-data-lake-store-account"></a>Skapa mappar i ett Data Lake Store-konto
Den här åtgärden är baserad på hello WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Använd hello följande cURL-kommando. Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

I hello ovanstående kommando ersätter \< `REDACTED` \> med hello autentiseringstoken som hämtats tidigare. Det här kommandot skapar en katalog med namnet **mytempdir** under hello rotmappen för ditt Data Lake Store-konto.

Om hello åtgärden slutförs bör du se ett svar som detta:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Lista mappar i ett Data Lake Store-konto
Den här åtgärden är baserad på hello WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Använd hello följande cURL-kommando. Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

I hello ovanstående kommando ersätter \< `REDACTED` \> med hello autentiseringstoken som hämtats tidigare.

Om hello åtgärden slutförs bör du se ett svar som detta:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Ladda upp data till ett Data Lake Store-konto
Den här åtgärden är baserad på hello WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Använd hello följande cURL-kommando. Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

I hello ovan syntax **-T** parametern är hello platsen för hello-filen som du överför.

hello-utdata är liknande toohello följande:
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data-from-a-data-lake-store-account"></a>Läs data från ett Data Lake Store-konto
Den här åtgärden är baserad på hello WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Att läsa data från ett Data Lake Store-konto är en tvåstegsprocess.

* Du skickar först en GET-begäran mot slutpunkten hello `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Detta returnerar en plats toosubmit hello nästa GET-begäran.
* Du skickar hello GET-begäran mot slutpunkten hello `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Detta visar hello innehållet i hello-fil.

Men eftersom det inte finns någon skillnad i hello indataparametrarna mellan hello först och hello andra steget kan du använda hello `-L` parametern toosubmit hello första begäran. `-L`alternativet kombinerar två begäranden till ett i princip och gör att cURL gör om hello begäran på hello ny plats. Slutligen hello utdata från alla anrop för hello begäran visas som visas nedan. Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

Du bör se en utdata liknande toohello följande:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Byt namn på en fil i ett Data Lake Store-konto
Den här åtgärden är baserad på hello WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Använd hello följande cURL-kommando toorename en fil. Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

Du bör se en utdata liknande toohello följande:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Ta bort en fil från ett Data Lake Store-konto
Den här åtgärden är baserad på hello WebHDFS REST API-anrop som definierats [här](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Använd hello följande cURL-kommando toodelete en fil. Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

Du bör se utdata som liknar hello följande:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Ta bort ett Data Lake Store-konto
Den här åtgärden är baserad på hello REST API-anrop som definierats [här](https://msdn.microsoft.com/library/mt694075.aspx).

Använd hello följande cURL-kommando toodelete ett Data Lake Store-konto. Ersätt **\<yourstorename>** med ditt Data Lake Store-namn.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Du bör se utdata som liknar hello följande:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Se även
* [Stordataprogram med öppen källkod som är kompatibla med Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md)

