---
title: "aaaCopy data från Azure Storage-Blobbar i Data Lake Store | Microsoft Docs"
description: "Använda AdlCopy verktyget toocopy data från Azure Storage BLOB tooData Datasjölager"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: dc273ef8-96ef-47a6-b831-98e8a777a5c1
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: a3d4172eaefe7395cdef2fff72691bd70f642b78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-from-azure-storage-blobs-toodata-lake-store"></a>Kopiera data från Azure Storage BLOB tooData Datasjölager
> [!div class="op_single_selector"]
> * [Använda DistCp](data-lake-store-copy-data-wasb-distcp.md)
> * [Använda AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Azure Data Lake Store ger ett kommandoradsverktyg [AdlCopy](http://aka.ms/downloadadlcopy), toocopy data från hello följande källor:

* Från Azure Storage BLOB till Data Lake Store. Du kan inte använda AdlCopy toocopy data från Data Lake Store tooAzure Storage-blobbar.
* Mellan två Azure Data Lake Store-konton.

Du kan också använda verktyget för hello AdlCopy i två olika lägen:

* **Fristående**, där hello verktyget använder Data Lake Store resurser tooperform hello aktivitet.
* **Med ett Data Lake Analytics-konto**, där hello-enheter som tilldelats tooyour Data Lake Analytics-konto är används tooperform hello kopieringen. Du kanske vill toouse det här alternativet när du söker tooperform hello kopiera aktiviteter förutsägbart.

## <a name="prerequisites"></a>Krav
Du måste ha hello följande innan du börjar den här artikeln:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Storage-Blobbar** behållare med vissa data.
* **Ett Azure Data Lake Store-konto**. Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure Data Lake Analytics-kontot (valfritt)** -finns [Kom igång med Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) anvisningar för hur toocreate ett Data Lake lagrar konto.
* **AdlCopy verktyget**. Installera verktyget för hello AdlCopy från [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-hello-adlcopy-tool"></a>Syntaxen för verktyget för hello AdlCopy
Använd följande syntax toowork med hello AdlCopy verktyget hello

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern

hello parametrar i hello syntaxen beskrivs nedan:

| Alternativ | Beskrivning |
| --- | --- |
| Källa |Anger hello plats hello källdata i hello Azure storage blob. hello källan kan vara en blob-behållare, en blobb eller en annan Data Lake Store-konto. |
| Målet |Anger hello Data Lake Store mål toocopy till. |
| SourceKey |Anger hello lagringsåtkomstnyckel för hello Azure storage blob källa. Detta krävs endast om hello källan är en blob-behållare eller en blob. |
| Konto |**Valfritt**. Använd det här alternativet om du vill toouse Azure Data Lake Analytics-konto toorun hello kopieringsjobbet. Om du använder hello /Account alternativet i hello syntax men inte anger ett Data Lake Analytics-konto, använder AdlCopy ett standard konto toorun hello jobb. Även om du använder det här alternativet måste du lägga hello källa (Azure Storage Blob) och mål (Azure Data Lake Store) som datakällor för Data Lake Analytics-kontot. |
| Enheter |Anger hello antalet Data Lake Analytics-enheter som ska användas för hello kopieringsjobbet. Det här alternativet är obligatoriskt om du använder hello **/kontot** alternativet toospecify hello Data Lake Analytics-konto. |
| Mönstret |Anger ett regex-mönster som anger vilka filer eller BLOB toocopy. AdlCopy använder skiftlägeskänsliga matchar. hello Standardmönster som används när inga mönster anges är toocopy alla objekt. Ange flera filen mönster stöds inte. |

## <a name="use-adlcopy-as-standalone-toocopy-data-from-an-azure-storage-blob"></a>Använda AdlCopy (som fristående) toocopy data från en Azure Storage-blob
1. Öppna en kommandotolk och navigera toohello directory där AdlCopy installeras normalt `%HOMEPATH%\Documents\adlcopy`.
2. Kör följande kommando toocopy hello en specifik blobb från hello källa behållaren tooa Data Lake Store:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Exempel:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[AZURE.NOTE] hello ovanstående syntax anger hello toobe kopierade tooa mapp i hello Data Lake Store-konto. AdlCopy verktyget skapar en mapp om hello angivna mappen inte finns.

    Du kan ange tooenter hello autentiseringsuppgifter för hello Azure-prenumeration som du har ditt Data Lake Store-konto. En liknande toohello följande i utdata visas:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. Du kan också kopiera alla hello BLOB från en behållare toohello Data Lake Store-konto med hello följande kommando:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Exempel:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a>Saker att tänka på gällande prestanda

Om du kopierar från ett Azure Blob Storage-konto kan du begränsas under kopia på hello blob-lagring på klientsidan. Hello prestanda kopiera jobbet kommer att försämras. toolearn mer om hello gränserna för Azure Blob Storage finns i Azure Storage gränser med [Azure-prenumeration och tjänstbegränsningarna](../azure-subscription-service-limits.md).

## <a name="use-adlcopy-as-standalone-toocopy-data-from-another-data-lake-store-account"></a>Använda AdlCopy (som fristående) toocopy data från en annan Data Lake Store-konto
Du kan också använda AdlCopy toocopy data mellan två Data Lake Store-konton.

1. Öppna en kommandotolk och navigera toohello directory där AdlCopy installeras normalt `%HOMEPATH%\Documents\adlcopy`.
2. Kör följande kommando toocopy hello en viss fil från ett Data Lake Store-konto tooanother.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Exempel:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

   > [!NOTE]
   > hello ovanstående syntax anger hello toobe kopierade tooa mapp i hello mål Data Lake Store-konto. AdlCopy verktyget skapar en mapp om hello angivna mappen inte finns.
   >
   >

    Du kan ange tooenter hello autentiseringsuppgifter för hello Azure-prenumeration som du har ditt Data Lake Store-konto. En liknande toohello följande i utdata visas:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
3. hello kopierar följande kommando alla filer i en mapp i hello Data Lake Store-konto tooa källmapp i hello målet Data Lake Store-konto.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a>Saker att tänka på gällande prestanda

När du använder AdlCopy som ett fristående verktyg hello kopia körs på delade, Azure hanterade resurser. hello prestanda du kan få i den här miljön är beroende av systembelastning och tillgängliga resurser. Det här läget är bäst för mindre överföringar på ad hoc-basis. Inga parametrar måste toobe justerade när du använder AdlCopy som ett fristående verktyg.

## <a name="use-adlcopy-with-data-lake-analytics-account-toocopy-data"></a>Använda AdlCopy (med Data Lake Analytics-konto) toocopy data
Du kan också använda Data Lake Analytics konto toorun hello AdlCopy toocopy jobbdata från Azure storage-blobbar tooData Lake Store. Normalt använder du det här alternativet när hello data toobe flyttas är i intervallet hello gigabyte och terabyte och du vill ha bättre och förutsägbar prestanda, genomströmning.

toouse ditt Data Lake Analytics-konto med AdlCopy toocopy från ett Azure Storage Blob, hello källa (Azure Storage Blob) måste läggas till som en datakälla för Data Lake Analytics-kontot. Anvisningar för att lägga till ytterligare datakällor tooyour Data Lake Analytics-konto finns [hantera Data Lake Analytics-kontot datakällor](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).

> [!NOTE]
> Om du kopierar från ett Azure Data Lake Store-konto som hello-datakälla med hjälp av ett Data Lake Analytics-konto behöver du inte tooassociate hello Data Lake Store-konto med hello Data Lake Analytics-konto. hello krav tooassociate hello källa store med hello Data Lake Analytics-konto är bara när hello källa är ett Azure Storage-konto.
>
>

Kör följande kommando toocopy från ett Azure Storage blob tooa Data Lake Store-konto med hjälp av Data Lake Analytics-konto hello:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Exempel:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

På liknande sätt, kör hello efter kommandot toocopy från ett Azure Storage blob tooa Data Lake Store-konto med hjälp av Data Lake Analytics-konto:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a>Saker att tänka på gällande prestanda

När du kopierar data i intervallet hello terabyte ger AdlCopy med din egen Azure Data Lake Analytics-kontot bättre och mer förutsägbar prestanda. hello-parameter som ska justeras är hello Azure Data Lake Analytics enheter toouse för hello kopieringsjobbet. Öka hello antalet enheter ökar hello prestanda för Kopiera projekt. Varje fil toobe kopieras kan använda maximalt en enhet. Ange fler enheter än hello antalet filer som kopieras kommer inte att öka prestanda.

## <a name="use-adlcopy-toocopy-data-using-pattern-matching"></a>Använd AdlCopy toocopy data med hjälp av mönstermatchning
I det här avsnittet lär du dig hur toouse AdlCopy toocopy data från en källa (i vårt exempel nedan använder vi Azure Storage Blob) tooa mål Data Lake Store-konto med hjälp av mönstermatchning. Du kan till exempel använda hello stegen nedan toocopy alla filer med CSV-fil från hello källa blob toohello mål.

1. Öppna en kommandotolk och navigera toohello directory där AdlCopy installeras normalt `%HOMEPATH%\Documents\adlcopy`.
2. Kör följande kommando toocopy hello alla filer med *.csv tillägg från en specifik blobb från hello källa behållaren tooa Data Lake Store:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Exempel:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a>Fakturering
* Om du använder hello AdlCopy verktyget som fristående du debiteras för kostnader för nätverksegress för att flytta hello data om hello källa Azure Storage-konto inte är i samma region som hello Data Lake Store.
* Om du använder hello AdlCopy verktyget med Data Lake Analytics-kontot, standard [Data Lake Analytics fakturering priser](https://azure.microsoft.com/pricing/details/data-lake-analytics/) gäller.

## <a name="considerations-for-using-adlcopy"></a>Överväganden för att använda AdlCopy
* AdlCopy (för version 1.0.5), har stöd för kopiering av data från källor som gemensamt har mer än tusentals filer och mappar. Om det uppstår problem kopiera en stor datauppsättning kan du dock distribuera hello filer/mappar till olika undermappar och Använd hello sökvägen toothose undermappar som hello källa i stället.

## <a name="performance-considerations-for-using-adlcopy"></a>Prestandaöverväganden för att använda AdlCopy

AdlCopy har stöd för kopiering av data som innehåller tusentals filer och mappar. Om du får problem med kopiera en stor datauppsättning kan du distribuera hello filer/mappar till mindre underordnade mappar. AdlCopy skapades för ad hoc-kopior. Om du försöker toocopy data regelbundet, bör du använda [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) som ger fullständig hantering runt hello kopieringsåtgärd.

## <a name="release-notes"></a>Viktig information
* 1.0.13 - om du kopierar data toohello samma Azure Data Lake Store-konto över flera adlcopy kommandon du inte behöver tooreenter dina autentiseringsuppgifter för varje köras längre. Adlcopy nu cachelagras informationen över flera körs.

## <a name="next-steps"></a>Nästa steg
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)
* [Använd Azure Data Lake Analytics med Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Använd Azure HDInsight med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
