---
title: "Kom igång med Azure Data Lake Analytics med hjälp av Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur du skapar ett Data Lake Analytics-konto med hjälp av Azure-kommandoradsgränssnittet 2.0, hur du skapar ett Data Lake Analytics-jobb med hjälp av U-SQL och hur du skickar jobbet. "
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
ms.openlocfilehash: fe2b84aac718ff5eddd4d73b5dc2120362952c1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli-20"></a>Kom igång med Azure Data Lake Analytics med hjälp av Azure CLI 2.0
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

I de här självstudierna utvecklar du ett jobb som läser en fil med tabbavgränsade värden (TVS) och konverterar den till en fil med kommaavgränsade värden (CSV). Använd rullgardinsmenyn överst i det här avsnittet om du vill gå igenom samma självstudier med andra verktyg.

## <a name="prerequisites"></a>Krav
Innan du börjar den här självstudiekursen behöver du följande:

* **en Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure CLI 2.0**. Se [Installera och konfigurera Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="log-in-to-azure"></a>Logga in på Azure

Logga in till din Azure-prenumeration:

```
azurecli
az login
```

Du behöver öppna en webbadress och ange en autentiseringskod.  Därefter kan du följa anvisningarna för att ange dina uppgifter.

När du har loggat in visas dina prenumerationer.

Använda en viss prenumeration:

```
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a>Skapa ett Data Lake Analytics-konto
Du måste ha ett Data Lake Analytics-konto innan du kan köra några jobb. Om du vill skapa ett Data Lake Analytics-konto, måste du ange följande objekt:

* **Azure-resursgrupp**. Ett Data Lake Analytics-konto måste skapas i en Azure-resursgrupp. Med [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kan du arbeta med resurserna i ditt program som en grupp. Du kan distribuera, uppdatera eller ta bort alla resurser i programmet i en enda, samordnad åtgärd.  

Listar de befintliga resursgrupperna under din prenumeration:

```
az group list
```

Om du vill skapa en ny resursgrupp:

```
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* **Data Lake Analytics-kontonamn**. Varje Data Lake Analytics-konto har ett namn.
* **Plats**. Använd ett av de Azure-datacenter som stöder Data Lake Analytics.
* **Data Lake Store-standardkonto**: Varje Data Lake Analytics-konto har ett Data Lake Store-standardkonto.

Om du vill skapa en lista över befintliga Data Lake Store-konton:

```
az dls account list
```

Skapa ett nytt Data Lake Store-konto:

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

Om du vill skapa ett Data Lake Analytics-konto, måste du ange följande syntax:

```
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

När du har skapat ett konto kan du använda följande kommandon för att visa en lista över konton och visa kontoinformation:

```
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-to-data-lake-store"></a>Ladda upp data till Data Lake Store
I den här självstudien bearbetar du vissa sökloggar.  Sökloggen kan lagras i Data Lake Store eller Azure Blob-lagring.

Azure Portal innehåller ett användargränssnitt för att kopiera vissa exempeldatafiler till Data Lake Store-standardkontot, bland annat en sökloggfil. Se [Förbereda källdata](data-lake-analytics-get-started-portal.md) för att ladda upp data till Data Lake Store-standardkontot.

Om du vill ladda upp filer med hjälp av CLI 2.0, använder du följande kommando:

```
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

Data Lake Analytics kan också använda Azure Blob-lagring.  Information om att ladda upp data till Azure Blob-lagring finns i [Använda Azure CLI med Azure Storage](../storage/common/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Skicka Data Lake Analytics-jobb
Data Lake Analytics-jobb skrivs på U-SQL-språket. Läs mer om U-SQL i [Kom igång med U-SQL-språket](data-lake-analytics-u-sql-get-started.md) och [Referens för U-SQL-språket](http://go.microsoft.com/fwlink/?LinkId=691348).

**Skapa ett Data Lake Analytics-jobbskript**

Skapa en textfil med följande U-SQL-skript och spara filen på din arbetsstation:

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

U-SQL-skriptet läser källdatafilen med hjälp av **Extractors.Tsv()** och skapar sedan en CSV-fil med hjälp av **Outputters.Csv()**.

Ändra inte de två sökvägarna om du inte har kopierat filen till en annan plats.  Data Lake Analytics skapar utdatamappen om den inte finns.

Det är enklare att använda relativa sökvägar för filer lagrade i Data Lake Store-standardkonton. Du kan också använda absoluta sökvägar.  Exempel:

```
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

Du måste använda absoluta sökvägar för att få åtkomst till filer i länkade Storage-konton.  Syntaxen för filer som lagras i ett länkat Azure Storage-konto är:

```
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> Azure Blob-behållare med offentliga blobbar stöds inte.      
> Azure Blob-behållare med offentliga behållare stöds inte.      
>

**Skicka jobb**

Använd följande syntax för att skicka ett jobb.

```
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

Exempel:

```
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

**Visa en lista över jobb och visa jobbinformation**

```
azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

**Avbryta jobb**

```
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

## <a name="retrieve-job-results"></a>Hämta jobbresultat

När jobbet har slutförts kan du använda följande kommandon för att visa och ladda ned filer med utdata:

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

Använd `az dla job pipeline`-kommandon för att visa information om pipeline för jobb som skickats tidigare.

```
az dla job pipeline list --account "<Data Lake Analytics Account Name>"

az dla job pipeline show --account "<Data Lake Analytics Account Name>" --pipeline-identity "<Pipeline ID>"
```

Använd `az dla job recurrence`-kommandon för att visa information om upprepningar för jobb som skickats tidigare.

```
az dla job recurrence list --account "<Data Lake Analytics Account Name>"

az dla job recurrence show --account "<Data Lake Analytics Account Name>" --recurrence-identity "<Recurrence ID>"
```

## <a name="next-steps"></a>Nästa steg

* För att visa Data Lake Analytics CLI 2.0-referensdokumentet, se [Data Lake Analytics](https://docs.microsoft.com/cli/azure/dla).
* För att visa Data Lake Store CLI 2.0-referensdokumentet, se [Data Lake Store](https://docs.microsoft.com/cli/azure/dls).
* Om du vill se en mer komplex fråga, se [Analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
