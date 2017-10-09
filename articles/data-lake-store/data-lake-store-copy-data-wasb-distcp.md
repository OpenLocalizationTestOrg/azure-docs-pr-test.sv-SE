---
title: "aaaCopy data tooand från WASB till Data Lake Store med hjälp av Distcp | Microsoft Docs"
description: "Använd Distcp verktyget toocopy data tooand från Azure Storage BLOB tooData Datasjölager"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ae2e9506-69dd-4b95-8759-4dadca37ea70
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: ec23410bbab0f82449a475412bc3b097c4a8fccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-distcp-toocopy-data-between-azure-storage-blobs-and-data-lake-store"></a>Använd Distcp toocopy data mellan Azure Storage-Blobbar och Data Lake Store
> [!div class="op_single_selector"]
> * [Använda DistCp](data-lake-store-copy-data-wasb-distcp.md)
> * [Använda AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)
>
>

När du har skapat ett HDInsight-kluster som har åtkomst till tooa Data Lake Store-konto kan du använda Hadoop-ekosystemet verktyg som Distcp toocopy data **tooand från** ett HDInsight-kluster storage (WASB) till ett Data Lake Store-konto. Den här artikeln innehåller instruktioner om hur tooachieve detta.

## <a name="prerequisites"></a>Krav
Du måste ha hello följande innan du börjar den här artikeln:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Ett Azure Data Lake Store-konto**. Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure HDInsight-kluster** med åtkomst tooa Data Lake Store-konto. Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Kontrollera att du kan aktivera Fjärrskrivbord för hello kluster.

## <a name="do-you-learn-fast-with-videos"></a>Lär du dig snabbt med videor?
[Det här videoklippet](https://mix.office.com/watch/1liuojvdx6sie) om hur toocopy data mellan Azure Storage-Blobbar och Data Lake Store med hjälp av DistCp.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Använd Distcp från ett kluster i HDInsight Linux

Ett HDInsight-kluster innehåller hello Distcp verktyg som kan vara används toocopy data från olika källor till ett HDInsight-kluster. Om du har konfigurerat hello HDInsight klustret toouse Data Lake Store som ett ytterligare lagringsutrymme, används hello Distcp verktyg kan vara out box toocopy data tooand från ett Data Lake Store-konto. I det här avsnittet titta vi på hur toouse hello Distcp-verktyget.

1. Använda SSH tooconnect toohello kluster från ditt skrivbord. Se [Anslut tooa Linux-baserade HDInsight-kluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Kör hello kommandon från hello SSH-Kommandotolken.

2. Kontrollera om du kan komma åt hello Azure Storage BLOB (WASB). Kör följande kommando hello:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Detta bör ge en lista över innehållet i hello storage-blob.
3. På liknande sätt kan kontrollera om du kan komma åt hello Data Lake Store-konto från hello kluster. Kör följande kommando hello:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    Detta bör ge en lista över filer/mappar i hello Data Lake Store-konto.
4. Använd Distcp toocopy data från WASB tooa Data Lake Store-konto.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Detta kopierar hello innehållet i hello **/exempel/data/gutenberg/** mapp i WASB för**/myfolder** i hello Data Lake Store-konto.
5. På liknande sätt, Använd Distcp toocopy data från Data Lake Store-konto tooWASB.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Detta kopierar hello innehållet i **/myfolder** i hello Data Lake Store-konto för**/exempel/data/gutenberg/** mapp i WASB.

## <a name="performance-considerations-while-using-distcp"></a>Prestandaöverväganden när du använder DistCp

Eftersom DistCp är lägsta Granularitet är en enskild fil är inställningen hello maximalt antal samtidiga kopior hello viktigaste parametern toooptimize den mot Data Lake Store. Detta styrs genom att ange hello antalet mappers ('M ') parametern på kommandoraden för hello. Den här parametern anger hello maximalt antal mappers som kommer att använda toocopy data. Standardvärdet är 20.

**Exempel**

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder -m 100

### <a name="how-do-i-determine-hello-number-of-mappers-toouse"></a>Hur vet hello antalet mappers toouse?

Här är några riktlinjer som du kan använda.

* **Steg 1: Välj den totala YARN minnet** -hello första steget är toodetermine hello YARN minne tillgängligt toohello klustret där du kör hello DistCp jobb. Den här informationen är tillgänglig i hello Ambari portal som associerats med hello kluster. Navigera tooYARN och visa hello konfigurationerna fliken toosee hello YARN minne. tooget hello totala YARN-minne, multiplicera hello YARN minne per nod med hello antalet noder som finns i klustret.

* **Steg 2: Beräkna hello antalet mappers** -hello värdet för **m** är lika toohello kvoten av den totala YARN minnet dividerat med hello YARN behållarens storlek. Hej YARN behållare Storleksinformation är tillgänglig i hello Ambari samt portal. Navigera tooYARN och visa hello konfigurationerna fliken hello YARN behållarens storlek visas i det här fönstret. Hej ekvationen tooarrive vid hello antalet mappers (**m**) är

        m = (number of nodes * YARN memory for each node) / YARN container size

**Exempel**

Anta att du har en 4 D14v2s-noder i klustret hello och du försöker tootransfer 10TB data från 10 olika mappar. Hello mappar innehålla olika mängder data och hello filstorlekar inom varje mapp är olika.

* Totala minnet i YARN - från hello Ambari portal du bestämma att hello YARN minne är 96GB för en D14-nod. Så är den totala YARN minnet för kluster med 4 noder: 

        YARN memory = 4 * 96GB = 384GB

* Antal mappers - från hello Ambari portal du bestämma att hello YARN behållarens storlek är 3072 för D14 klusternoder. Det är antal mappers:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Om andra program använder minne, kan sedan du välja tooonly används en del av ditt kluster YARN minne för DistCp.

### <a name="copying-large-datasets"></a>Kopiera stora datauppsättningar

När hello storleken på hello dataset toobe flyttas är mycket stora (t.ex. > 1TB) eller om du har många olika mappar bör du använda flera DistCp jobb. Det är förmodligen inga prestandafördelar, men den att sprida ut hello jobb så att om något jobb misslyckas, behöver du bara toorestart som jobbets i stället för hela hello-jobbet.

### <a name="limitations"></a>Begränsningar

* DistCp försöker toocreate mappers med liknande storlek toooptimize prestanda. Öka hello antal mappers kanske inte alltid att öka prestanda.

* DistCp är begränsad tooonly en mappning per fil. Du bör därför inte ha flera mappers än vad du har filer. Eftersom DistCp kan endast tilldelas en mapper tooa fil, begränsar detta hello samtidighet som kan använda toocopy stora filer.

* Om du har ett litet antal stora filer, så att du ska dela upp dem i 256MB filen segment toogive du flera potentiella samtidighet. 
 
* Om du kopierar från ett Azure Blob Storage-konto, att kopiera-projekt begränsas på hello blob storage-sida. Hello prestanda kopiera jobbet kommer att försämras. toolearn mer om hello gränserna för Azure Blob Storage finns i Azure Storage gränser med [Azure-prenumeration och tjänstbegränsningarna](../azure-subscription-service-limits.md).

## <a name="see-also"></a>Se även
* [Kopiera data från Azure Storage BLOB tooData Datasjölager](data-lake-store-copy-data-azure-storage-blob.md)
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)
* [Använd Azure Data Lake Analytics med Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Använd Azure HDInsight med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
