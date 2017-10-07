---
title: aaaUse Data Lake Store med Hadoop i Azure HDInsight | Microsoft Docs
description: "Lär dig hur tooquery data från Azure Data Lake Store och toostore resultatet av dina analyser."
keywords: blob storage,hdfs,structured data,unstructured data, data lake store
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: jgao
ms.openlocfilehash: 89633218a37a2fe05043e05d61199dcc0252d7f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-data-lake-store-with-azure-hdinsight-clusters"></a>Använda Data Lake Store med Azure HDInsight-kluster

tooanalyze data i HDInsight-kluster, du kan lagra data hello antingen i [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md), eller båda. Båda lagringsalternativ aktivera toosafely ta bort HDInsight-kluster som används för beräkning utan att förlora användardata.

I den här artikeln får du lära dig hur Data Lake Store fungerar med HDInsight-kluster. toolearn hur Azure Storage fungerar med HDInsight-kluster finns i [använda Azure Storage med Azure HDInsight-kluster](hdinsight-hadoop-use-blob-storage.md). Mer information om hur du skapar ett HDInsight-kluster finns i [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) (Skapa Hadoop-kluster i HDInsight).

> [!NOTE]
> Data Lake Store alltid är tillgängligt via en säker kanal så det finns inget `adls`-filsystemsschemanamn. Du använder alltid `adl`.
> 
> 

## <a name="availabilities-for-hdinsight-clusters"></a>Tillgänglighet för HDInsight-kluster

Hadoop stöder begreppet standardfilsystem hello. Hej standardfilsystemet kräver ett standardschema och utfärdare. Det kan också vara används tooresolve relativa sökvägar. Under skapandeprocessen hello HDInsight-kluster, kan du ange en blob-behållare i Azure Storage som hello standardfilsystem eller med HDInsight 3.5 och nyare versioner, kan du välja Azure Storage eller Azure Data Lake Store som hello standard filer system med en få undantag. 

HDInsight-kluster kan använda Data Lake Store på två sätt:

* Som standard hello
* Som ytterligare lagringsutrymme med Azure Storage Blob som standardlagringsutrymme.

Från och med nu endast en del av hello HDInsight klusterstöd typer-versioner med Data Lake Store som standardlagring och ytterligare storage-konton:

| Typ av HDInsight-kluster | Data Lake Store som standardlagring | Data Lake Store som ytterligare lagring| Anteckningar |
|------------------------|------------------------------------|---------------------------------------|------|
| HDInsight version 3.6 | Ja | Ja | |
| HDInsight version 3.5 | Ja | Ja | Med undantag för hello av HBase|
| HDInsight version 3.4 | Nej | Ja | |
| HDInsight version 3.3 | Nej | Nej | |
| HDInsight version 3.2 | Nej | Ja | |
| HDInsight Premium (nivå)| Nej | Nej | |
| Storm | | |Du kan använda Data Lake Store toowrite data från en Storm-topologi. Du kan också använda Data Lake Store för referensdata som sedan kan läsas av en Storm-topologi.|

Med Data Lake Store som ett ytterligare storage-konto inte påverka prestanda eller hello möjlighet tooread eller skriva tooAzure lagring från hello kluster.


## <a name="use-data-lake-store-as-default-storage"></a>Använda Data Lake Store som standardlagring

När HDInsight har distribuerats med Data Lake Store som standardlagring, lagras hello kluster-relaterade filer i Data Lake Store i hello följande plats:

    adl://mydatalakestore/<cluster_root_path>/

där `<cluster_root_path>` är hello namnet på en mapp som du skapar i Data Lake Store. Genom att ange en rotsökväg för varje kluster kan du använda hello samma Data Lake Store-konto för flera kluster. Så du kan ha en konfiguration enligt följande:

* Cluster1 kan använda hello sökväg`adl://mydatalakestore/cluster1storage`
* Cluster2 kan använda hello sökväg`adl://mydatalakestore/cluster2storage`

Meddelande både hello kluster Använd hello samma Data Lake Store-konto **mydatalakestore**. Varje kluster har åtkomst tooits äger rot filsystem i Data Lake Store. hello Azure portal distributionsupplevelse särskilt efterfrågar toouse ett mappnamn som **/clusters/\<klusternamn >** för hello rotsökvägen.

toobe kan toouse ett Data Lake Store som standardlagring, måste du bevilja hello service principal åtkomst toohello följande sökvägar:

- hello roten för Data Lake Store-konto.  Till exempel: adl://mydatalakestore/.
- hello mapp för alla mappar för klustret.  Till exempel: adl://mydatalakestore/clusters.
- hello mapp för hello klustret.  Till exempel: adl://mydatalakestore/clusters/cluster1storage.

Mer information om att skapa tjänstens huvudnamn och bevilja åtkomst finns i [Konfigurera åtkomst till Data Lake Store](#configure-data-lake-store-access).


## <a name="use-data-lake-store-as-additional-storage"></a>Använda Data Lake Store som ytterligare lagring

Du kan använda Data Lake Store som ytterligare lagringsutrymme för hello-kluster. I sådana fall kan hello standard klusterlagringen vara en Azure Storage Blob eller ett Data Lake Store-konto. Om du använder HDInsight jobb mot hello data som lagras i Data Lake Store som ytterligare lagringsutrymme måste du använda hello fullständig sökväg toohello filer. Exempel:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Observera att det finns inga **cluster_root_path** i nu hello-URL. Det beror på att Data Lake Store inte är en standardlagring i det här fallet så behöver du toodo hello sökvägen toohello filer.

toobe kan toouse ett Data Lake Store som ytterligare lagringsutrymme, behöver du bara toogrant hello service principal åtkomst toohello sökvägar där filerna lagras.  Exempel:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Mer information om att skapa tjänstens huvudnamn och bevilja åtkomst finns i [Konfigurera åtkomst till Data Lake Store](#configure-data-lake-store-access).


## <a name="use-more-than-one-data-lake-store-accounts"></a>Använda fler än ett Data Lake Store-konto

Lägger till ett Data Lake Store-konto som ytterligare och lägga till fler än en Data Lake Store erhålla konton genom att ge behörighet för hello HDInsight-kluster på data i en eller flera datasjölagerkonton. Läs mer i [Konfigurera åtkomst till Data Lake Store](#configure-data-lake-store-access).

## <a name="configure-data-lake-store-access"></a>Konfigurera åtkomst till Data Lake Store

Du måste ha en Azure Active directory (AD Azure) tjänstens huvudnamn tooconfigure Data Lake store-åtkomst från ditt HDInsight-kluster. Det är bara Azure AD-administratörer som kan vara tjänstens huvudnamn. hello tjänstens huvudnamn måste skapas med ett certifikat. Mer information finns i [Konfigurera åtkomst till Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#configure-data-lake-store-access) och [Create service principal with self-signed-certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate) (Skapa tjänstens huvudnamn med självsignerat certifikat).

> [!NOTE]
> Om du ska toouse Azure Data Lake Store som ytterligare lagringsutrymme för HDInsight-kluster, rekommenderar vi att du gör detta när du skapar klustret hello som beskrivs i den här artikeln. Lägga till Azure Data Lake Store som ytterligare lagringsutrymme tooan är befintligt HDInsight-kluster en komplicerad process och felbenägna tooerrors.
>

## <a name="access-files-from-hello-cluster"></a>Komma åt filer från hello kluster

Det finns flera sätt som du kan komma åt hello filer i Data Lake Store från ett HDInsight-kluster.

* **Hello fullständigt kvalificerade namnet**. Med den här metoden ger du hello fullständig sökväg toohello filen som du vill tooaccess.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **Formatet för hello kortare sökväg**. Med den här metoden ersätts hello sökväg in toohello kluster roten adl: / / /. Så i hello-exemplet ovan kan du ersätta `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` med `adl:///`.

        adl:///<file path>

* **Med hjälp av hello relativa sökvägen**. Med den här metoden du bara ange hello relativ sökväg toohello filen som du vill tooaccess. Om exempelvis hello fullständig sökväg toohello filen är:

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    Du kan komma åt hello samma sample.log-fil med hjälp av den här relativ sökväg istället.

        /example/data/sample.log

## <a name="create-hdinsight-clusters-with-access-toodata-lake-store"></a>Skapa HDInsight-kluster med att komma åt tooData Datasjölager

Använd hello efter länkar till detaljerade instruktioner om hur toocreate HDInsight-kluster med åt tooData Lake Store.

* [Använda portalen](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
* [Använda PowerShell (med Data Lake Store som standardlagringsutrymme)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Använda PowerShell (med Data Lake Store som ytterligare lagringsutrymme)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Använda Azure-mallar](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)


## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur toouse HDFS-kompatibla Azure Data Lake Store med HDInsight. Detta gör att du toobuild skalbara, långsiktiga, arkivering lösningar för datainsamling och använda HDInsight toounlock hello information i hello lagrade strukturerade och Ostrukturerade data.

Mer information finns i:

* [Kom igång med Azure HDInsight][hdinsight-get-started]
* [Kom igång med Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md)
* [Ladda upp data tooHDInsight][hdinsight-upload-data]
* [Använda Hive med HDInsight][hdinsight-use-hive]
* [Använda Pig med HDInsight][hdinsight-use-pig]
* [Använda Azure Storage signaturer för delad åtkomst toorestrict åtkomst toodata med HDInsight][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]:../storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  
