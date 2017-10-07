---
title: "aaaGet igång med Azure Data Lake Analytics med hjälp av Azure PowerShell | Microsoft Docs"
description: "Använda Azure PowerShell toocreate ett Data Lake Analytics-konto, skapa ett Data Lake Analytics-jobb med hjälp av U-SQL och skicka hello-jobbet. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 8a4e901e-9656-4a60-90d0-d78ff2f00656
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: edmaca
ms.openlocfilehash: cb9b35352d1cc9a78337448b1d6835875a212e08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Kom igång med Azure Data Lake Analytics med hjälp av Azure PowerShell
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Lär dig hur toouse Azure PowerShell toocreate Azure Data Lake Analytics-konton och sedan skicka och köra U-SQL-jobb. Mer information om Data Lake Analytics finns i [Översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Krav

Innan du påbörjar den här självstudien måste du ha hello följande information:

* **Ett Azure Data Lake Analytics-konto**. Se [Kom igång med Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).
* **En arbetsstation med Azure PowerShell**. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

## <a name="log-in-tooazure"></a>Logga in tooAzure

Den här kursen förutsätter att du redan är bekant med Azure PowerShell. I synnerhet måste tooknow hur toolog i tooAzure. Se hello [Kom igång med Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps) om du behöver hjälp.

toolog in med ett prenumerationsnamn:

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

Du kan också använda en prenumerations-id toolog i i stället för hello prenumerationsnamn:

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

Om detta lyckas hello utdata från kommandot ser ut så hello följande text:

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

## <a name="preparing-for-hello-tutorial"></a>Förbereda hello genomgång

hello PowerShell kodavsnitt i den här kursen använder dessa variabler toostore informationen:

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="get-information-about-a-data-lake-analytics-account"></a>Hämta information om ett Data Lake Analytics-konto

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a>Skicka ett U-SQL-jobb

Skapa ett PowerShell variabeln toohold hello U-SQL-skript.

```
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();

"@
```

Skicka hello skript.

```
$job = Submit-AdlJob -AccountName $adla –Script $script
```

Du kan också spara hello skript som en fil och skicka med hello följande kommando:

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -AccountName $adla –ScriptPath $filename
```


Hämta hello status för ett visst jobb. Fortsätta att använda denna cmdlet tills du ser hello jobbet är klart.

```
$job = Get-AdlJob -AccountName $adla -JobId $job.JobId
```

Du kan använda hello vänta AdlJob cmdlet i stället för att anropa Get-AdlAnalyticsJob flera gånger tills ett jobb har slutförts.

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

Hämta hello utdatafilen.

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "C:\data.csv"
```

## <a name="see-also"></a>Se även
* toosee hello samma självstudier med andra verktyg klickar du på hello flikväljarna överst hello hello-sidan.
* toolearn U-SQL finns [Kom igång med Azure Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md).
* Information om hanteringsuppgifter finns i [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md).
