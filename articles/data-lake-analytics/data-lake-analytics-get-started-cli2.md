---
title: "aaaGet igång med Azure Data Lake Analytics med hjälp av Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur toouse hello Azure kommandoradsgränssnittet 2.0 toocreate ett Data Lake Analytics-konto, skapa ett Data Lake Analytics-jobb med hjälp av U-SQL och skicka hello-jobbet. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: jgao
ms.openlocfilehash: c4e91c0d3526e4932c2948c0a326d4cedc985791
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli-20"></a>Kom igång med Azure Data Lake Analytics med hjälp av Azure CLI 2.0
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

I de här självstudierna utvecklar du ett jobb som läser en fil med tabbavgränsade värden (TVS) och konverterar den till en fil med kommaavgränsade värden (CSV). toogo via hello stöds samma självstudier med andra verktyg, Använd hello listrutan hello längst upp i det här avsnittet.

## <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du ha hello följande objekt:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure CLI 2.0**. Se [Installera och konfigurera Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="log-in-tooazure"></a>Logga in tooAzure

toolog i tooyour Azure-prenumeration:

```
azurecli
az login
```

Du har begärt toobrowse tooa URL och ange en Autentiseringskod.  Och följ hello instruktioner tooenter dina autentiseringsuppgifter.

När du har loggat in visar hello inloggningen kommando dina prenumerationer.

toouse en viss prenumeration:

```
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a>Skapa ett Data Lake Analytics-konto
Du måste ha ett Data Lake Analytics-konto innan du kan köra några jobb. toocreate ett Data Lake Analytics-konto, måste du ange hello följande objekt:

* **Azure-resursgrupp**. Ett Data Lake Analytics-konto måste skapas i en Azure-resursgrupp. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kan du toowork med hello resurser i ditt program som en grupp. Du kan distribuera, uppdatera eller ta bort alla hello resurser i ditt program i en enda, samordnad åtgärd.  

toolist hello befintliga resursgrupper i din prenumeration:

```
az group list
```

toocreate en ny resursgrupp:

```
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* **Data Lake Analytics-kontonamn**. Varje Data Lake Analytics-konto har ett namn.
* **Plats**. Använd någon av hello Azure-datacenter som stöder Data Lake Analytics.
* **Data Lake Store-standardkonto**: Varje Data Lake Analytics-konto har ett Data Lake Store-standardkonto.

toolist hello befintliga Data Lake Store-konto:

```
az dls account list
```

toocreate ett nytt Data Lake Store-konto:

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

Använd följande syntax toocreate ett Data Lake Analytics-konto hello:

```
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

Du kan använda följande kommandon toolist hello konton hello och visa kontoinformation när du har skapat ett konto:

```
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-toodata-lake-store"></a>Överför tooData Datasjölager
I den här självstudien bearbetar du vissa sökloggar.  Hej sökloggen kan lagras i Data Lake store eller Azure Blob storage.

hello Azure-portalen innehåller ett användargränssnitt för att kopiera vissa exempel data filer toohello Data Lake Store standardkontot, bland annat en sökloggfil. Se [förbereda källdata](data-lake-analytics-get-started-portal.md) tooupload hello data toohello Data Lake Store-standardkontot.

tooupload filer med hjälp av CLI 2.0, Använd hello följande kommandon:

```
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

Data Lake Analytics kan också använda Azure Blob-lagring.  Ladda upp data tooAzure Blob storage finns [Using hello Azure CLI med Azure Storage](../storage/common/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Skicka Data Lake Analytics-jobb
hello Data Lake Analytics-jobb skrivs på hello U-SQL-språket. toolearn mer om U-SQL finns [Kom igång med U-SQL-språket](data-lake-analytics-u-sql-get-started.md) och [U-SQL-språket eence](http://go.microsoft.com/fwlink/?LinkId=691348).

**toocreate Data Lake Analytics-jobbskript**

Skapa en textfil med följande U-SQL-skript och spara hello text filen tooyour arbetsstation:

```
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
```

U-SQL-skriptet läser hello källa data filen med hjälp av **Extractors.Tsv()**, och skapar sedan en CSV-fil med hjälp av **Outputters.Csv()**.

Ändra inte hello två sökvägarna om du kopierar hello källfilen till en annan plats.  Data Lake Analytics skapar Utdatamappen hello om den inte finns.

Det är enklare toouse relativa sökvägar för filer lagrade i Data Lake Store standardkonton. Du kan också använda absoluta sökvägar.  Exempel:

```
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

Du måste använda absoluta sökvägar tooaccess filer i länkade Storage-konton.  hello syntaxen för filer som lagras i länkade Azure Storage-konto är:

```
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> Azure Blob-behållare med offentliga blobbar stöds inte.      
> Azure Blob-behållare med offentliga behållare stöds inte.      
>

**toosubmit jobb**

Använd följande syntax toosubmit ett jobb hello.

```
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

Exempel:

```
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

**toolist jobb och visa jobbinformation**

```
azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

**toocancel jobb**

```
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

## <a name="retrieve-job-results"></a>Hämta jobbresultat

När ett jobb har slutförts kan du använda hello följande kommandon toolist hello utgående filer och hämta hello filer:

```
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destintion>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs downlod --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "<Destination Path and File Name>"
```

Exempel:

```
az dls fs downlod --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "C:\DLA\myfile.csv"
```

## <a name="pipelines-and-recurrences"></a>Pipelines och upprepningar

**Hämta information om pipelines och upprepningar**

Använd hello `az dla job pipeline` kommandon toosee hello pipeline-information skickats tidigare jobb.

```
az dla job pipeline list --account "<Data Lake Analytics Account Name>"

az dla job pipeline show --account "<Data Lake Analytics Account Name>" --pipeline-identity "<Pipeline ID>"
```

Använd hello `az dla job recurrence` kommandon toosee hello återkommande information för tidigare skickade jobb.

```
az dla job recurrence list --account "<Data Lake Analytics Account Name>"

az dla job recurrence show --account "<Data Lake Analytics Account Name>" --recurrence-identity "<Recurrence ID>"
```

## <a name="next-steps"></a>Nästa steg

* toosee hello Data Lake Analytics CLI 2.0 Referensdokumentet finns [Datasjöanalys](https://docs.microsoft.com/cli/azure/dla).
* toosee hello Data Lake Store CLI 2.0 Referensdokumentet finns [Datasjölager](https://docs.microsoft.com/cli/azure/dls).
* toosee en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
