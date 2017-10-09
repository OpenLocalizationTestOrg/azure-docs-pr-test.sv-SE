---
title: "aaaManage Azure Data Lake Analytics med hjälp av Azure-kommandoradsgränssnittet | Microsoft Docs"
description: "Lär dig hur toomanage Data Lake Analytics-konton, datakällor, jobb och användare som använder Azure CLI"
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: 0af1f89080739b39f6980989b7694734cc135715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Hantera Azure Data Lake Analytics med hjälp av Azure-kommandoradsgränssnittet (CLI)
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Lär dig hur toomanage Azure Data Lake Analytics-konton, datakällor, användare och jobb med hjälp av hello Azure CLI. toosee management information med andra verktyg klickar du på hello flikväljaren ovan.


**Förutsättningar**

Innan du påbörjar den här självstudien måste du ha hello följande:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure CLI**. Se [Installera och konfigurera Azure CLI](../cli-install-nodejs.md).
  * Hämta och installera hello **förhandsversionen** [Azure CLI-verktygen](https://github.com/MicrosoftBigData/AzureDataLake/releases) ordning toocomplete i den här demon.
* **Autentisering**med hjälp av hello följande kommando:
  
        azure login
    Mer information om autentisering med ett arbets- eller skolkonto konto finns [ansluta tooan Azure-prenumeration från hello Azure CLI](../xplat-cli-connect.md).
* **Växla toohello Azure Resource Manager läge**med hjälp av hello följande kommando:
  
        azure config mode arm

**toolist hello Data Lake Store och Data Lake Analytics-kommandon:**

    azure datalake store
    azure datalake analytics

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-accounts"></a>Hantera konton
Du måste ha ett Data Lake Analytics-konto innan du kör alla Data Lake Analytics-jobb. Till skillnad från Azure HDInsight betalar inte du för Analytics-konto när ett jobb inte körs.  Du betalar bara för hello tid när den körs ett jobb.  Mer information finns i [översikt över Azure Data Lake Analytics](data-lake-analytics-overview.md).  

### <a name="create-accounts"></a>Skapa konton
      azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"


### <a name="update-accounts"></a>Uppdatera konton
hello uppdaterar följande kommando hello egenskaper för ett befintligt Data Lake Analytics-konto

    azure datalake analytics account set "<Data Lake Analytics Account Name>"


### <a name="list-accounts"></a>Lista över konton
Lista över Data Lake Analytics-konton 

    azure datalake analytics account list

Lista över Data Lake Analytics-konton inom en viss resursgrupp

    azure datalake analytics account list -g "<Azure Resource Group Name>"

Hämta information om ett visst Data Lake Analytics-konto

    azure datalake analytics account show -g "<Azure Resource Group Name>" -n "<Data Lake Analytics Account Name>"

### <a name="delete-data-lake-analytics-accounts"></a>Ta bort Data Lake Analytics-konton
      azure datalake analytics account delete "<Data Lake Analytics Account Name>"


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Hantera datakällor för kontot
Data Lake Analytics stöder för närvarande hello följande datakällor:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage](../storage/common/storage-introduction.md)

När du skapar ett Analytics-konto, anger du ett Azure Data Lake Storage-konto toobe hello standardkontot för lagring. hello ADL standardkontot för lagring används toostore jobbet metadata och jobbet granskningsloggar. När du har skapat ett Analytics-konto kan du lägga till ytterligare Data Lake-lagringskontona och/eller Azure Storage-konto. 

### <a name="find-hello-default-adl-storage-account"></a>Hitta hello ADL standardkontot för lagring
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

hello värde visas under egenskaper: datalakeStoreAccount:name.

### <a name="add-additional-azure-blob-storage-accounts"></a>Lägg till ytterligare Azure Blob storage-konton
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -b "<Azure Blob Storage Account Short Name>" -k "<Azure Storage Account Key>"

> [!NOTE]
> Endast Blob storage kortnamn stöds.  Använd inte FQDN, till exempel ”myblob.blob.core.windows.net”.
> 
> 

### <a name="add-additional-data-lake-store-accounts"></a>Lägga till ytterligare Data Lake Store-konton
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -l "<Data Lake Store Account Name>" [-d]

[-d] är en valfri växel tooindicate om hello Data Lake som läggs till är hello Data Lake-standardkontot. 

### <a name="update-existing-data-source"></a>Uppdatera befintlig datakälla
tooset standard toobe-hello en befintlig Data Lake Store-konto:

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -l "<Azure Data Lake Store Account Name>" -d

tooupdate en befintlig nyckel för Blob storage-konto:

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -b "<Blob Storage Account Name>" -k "<New Blob Storage Account Key>"

### <a name="list-data-sources"></a>Lista över datakällor:
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

![Datakälla för data Lake Analytics-lista](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a>Ta bort datakällor:
toodelete ett Data Lake Store-konto:

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Azure Data Lake Store Account Name>"

toodelete Blob storage-kontot:

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Blob Storage Account Name>"

## <a name="manage-jobs"></a>Hantera jobb
Du måste ha ett Data Lake Analytics-konto innan du kan skapa ett jobb.  Mer information finns i [hantera Data Lake Analytics-konton](#manage-accounts).

### <a name="list-jobs"></a>Lista över jobb
      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"

![Datakälla för data Lake Analytics-lista](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a>Hämta jobbinformation
      azure datalake analytics job show -n "<Data Lake Analytics Account Name>" -j "<Job ID>"

### <a name="submit-jobs"></a>Skicka jobb
> [!NOTE]
> hello standardprioritet för ett jobb är 1000 och hello standard grad av parallellitet för ett jobb är 1.
> 
> 

    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"

### <a name="cancel-jobs"></a>Avbryta jobb
Använd hello listan kommandot toofind hello jobb-id och använder sedan Avbryt toocancel hello jobbet.

      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"
      azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job ID>"

## <a name="manage-catalog"></a>Hantera katalog
hello U-SQL-katalogen är används toostructure data och kod så att de kan delas av U-SQL-skript. hello katalog kan hello högsta prestanda möjligt med data i Azure Data Lake. Mer information finns i [Använd U-SQL-katalogen](data-lake-analytics-use-u-sql-catalog.md).

### <a name="list-catalog-items"></a>Lista katalogartiklar
    #List databases
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t database

    #List tables
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t table

hello typer är databasen, schemat, sammansättningen, extern datakälla, tabell, Tabellvärdesfunktionen eller tabellstatistik.

<!-- ################################ -->
<!-- ################################ -->
## <a name="use-arm-groups"></a>Använda grupper för ARM
Program består vanligtvis av flera komponenter, till exempel en webbapp, databas, databasserver, lagring och tredjepartstjänster. Azure Resource Manager (ARM) kan du toowork med hello resurser i ditt program som en grupp, som anges tooas Azure-resursgrupp. Du kan distribuera, uppdatera, övervaka eller ta bort alla hello resurser i ditt program i en enda, samordnad åtgärd. Du använder en mall för distributionen. Mallen kan användas i olika miljöer, till exempel för testning, mellanlagring och produktion. Du kan tydliggöra fakturering för din organisation genom att visa hello upplyfta kostnader för hello hela gruppen. Mer information finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). 

Data Lake Analytics-tjänsten kan omfatta hello följande komponenter:

* Azure Data Lake Analytics-konto
* Nödvändiga standardkontot för lagring av Azure Data Lake
* Ytterligare Azure Data Lake Storage-konton
* Ytterligare Azure Storage-konton

Du kan skapa alla dessa komponenter under en ARM grupp toomake dem lättare toomanage.

![Azure Data Lake Analytics-konto och lagring](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Ett Data Lake Analytics-konto och hello beroende storage-konton måste placeras i hello samma Azure-datacenter.
hello ARM-grupp kan dock finnas i olika datacenter.  

## <a name="see-also"></a>Se även
* [Översikt över Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Kom igång med Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-get-started-portal.md)
* [Hantera Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-manage-use-portal.md)
* [Övervaka och felsök Azure Data Lake Analytics-jobb med hjälp av Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

