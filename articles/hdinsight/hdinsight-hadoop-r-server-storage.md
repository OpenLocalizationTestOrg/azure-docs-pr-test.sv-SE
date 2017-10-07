---
title: "aaaAzure lagringslösningar för R Server på HDInsight - Azure | Microsoft Docs"
description: "Lär dig mer om hello olika lagringsplatser alternativ tillgängliga toousers med R Server på HDInsight"
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 1cf30096-d3ca-45ea-b526-aa3954402f66
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: ff5e80fee14d5e74cbc22e873e6bc1439a3b6037
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-solutions-for-r-server-on-hdinsight"></a>Azure-lagringslösningar för R Server på HDInsight

Microsoft R Server på HDInsight har olika lagring lösningar toopersist data, kod eller objekt som innehåller resultatet från analys. Dessa omfattar hello följande alternativ:

- [Azure Blob](https://azure.microsoft.com/services/storage/blobs/)
- [Lagring av Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/)
- [Azure File storage](https://azure.microsoft.com/services/storage/files/)

Du har också hello möjlighet att komma åt flera Azure storage-konton eller behållare med HDI-klustret. Azure File storage är en lämplig data lagringsalternativ för användning på hello kantnod som du kan använda toomount en Azure Storage-filresurs som till exempel hello Linux-system. Men Azure-filresurser kan monteras och används av alla system som har ett operativsystem som stöds, till exempel Windows eller Linux. 

När du skapar ett Hadoop-kluster i HDInsight kan du ange antingen ett **Azure storage** konto eller ett **Data Lake store**. En specifik storage-behållare från detta konto innehåller hello filsystem för hello-kluster som du skapar (till exempel hello Hadoop Distributed File System). Mer information och vägledning finns:

- [Använda Azure storage med HDInsight](hdinsight-hadoop-use-blob-storage.md)
- [Använd Data Lake Store med Azure HDInsight-kluster](hdinsight-hadoop-use-data-lake-store.md). 

Mer information om hello Azure-lagringslösningar finns [introduktion tooMicrosoft Azure Storage](../storage/common/storage-introduction.md). 

Anvisningar om hur du väljer hello lämpligaste lagring alternativet toouse för ditt scenario finns [ska när toouse Azure BLOB-objekt, Azure-filer eller Azure Datadiskar](../storage/common/storage-decide-blobs-files-disks.md) 


## <a name="use-azure-blob-storage-accounts-with-r-server"></a>Använda Azure Blob storage-konton med R Server

Om det behövs kan du komma åt flera Azure storage-konton eller behållare med HDI-klustret. toodo, du behöver toospecify hello ytterligare lagringskonton i hello Användargränssnittet när du skapar hello klustret och sedan följa dessa steg toouse dem med R Server.

> [!WARNING]
> För prestanda, hello HDInsight-kluster skapas i hello samma datacenter som hello primära storage-konto som du anger. Med hjälp av ett lagringskonto i en annan plats än hello HDInsight-kluster stöds inte.

1. Skapa ett HDInsight-kluster med namnet på ett lagringskonto för **storage1** och en standardbehållare kallas **container1**.
2. Ange ett konto för ytterligare lagringsutrymme som heter **storage2**.  
3. Kopiera hello mycsv.csv toohello /share katalog och utföra analyser på filen.  

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal myscsv.scv /share  

4. R-koden ange hello namn nod för**standard** och ange din katalog- och tooprocess.  

        myNameNode <- "default"
        myPort <- 0

        #Location of hello data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define hello Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify hello input file tooanalyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mycsv.csv")

Alla hello katalog- och lagringskontot för referenser punkt toohello wasb://container1@storage1.blob.core.windows.net. Detta är hello **standard lagringskonto** som associeras med hello HDInsight-kluster.

Nu kan anta att du vill tooprocess en fil som heter mySpecial.csv som finns i hello /private för **container2** i **storage2**.

Punkt i koden R hello namn nod referens toohello **storage2** storage-konto.


    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of hello data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify hello input file tooanalyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mySpecial.csv")

Alla hello katalog- och referenser nu punkt toohello lagringskonto wasb://container2@storage2.blob.core.windows.net. Detta är hello **namn nod** som du har angett.

Du har tooconfigure hello/User/RevoShare/<SSH username> på **storage2** på följande sätt:


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>



## <a name="use-an-azure-data-lake-store-with-r-server"></a>Använda en Azure Data Lake store med R Server

toouse Data Lake lagrar med ditt HDInsight-konto måste du toogive din klustret åtkomst tooeach Azure Data Lake store som du vill toouse. Anvisningar om hur toouse hello Azure portal toocreate ett HDInsight-kluster med ett Azure Data Lake Store-konto som hello standardlagring eller som en ytterligare store, finns [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Du sedan använda hello store i din R-skriptet mycket som du gjorde en sekundär Azure storage-konto som beskrivs i föregående procedur för hello.

### <a name="add-cluster-access-tooyour-azure-data-lake-stores"></a>Lägg till klustret åtkomst tooyour Azure Data Lake lagrar
Du har åtkomst till ett Data Lake store med hjälp av ett huvudnamn för tjänsten Azure Active Directory (AD Azure) som är kopplad till ditt HDInsight-kluster.

tooadd en Azure AD tjänstens huvudnamn:

1. När du skapar ditt HDInsight-kluster, Välj **kluster-AAD-identitet** från hello **datakällan** fliken.

2. I hello **kluster-AAD-identitet** dialogrutan under **Välj AD tjänstens huvudnamn**väljer **Skapa nytt**.

När du namnge hello tjänstens huvudnamn och skapa ett lösenord för det, klickar du på **hanterar ADLS-åtkomst** tooassociate hello tjänstens huvudnamn med Data Lake lagras.

Det är också möjligt tooadd klustret åtkomst tooone eller mer Data Lake lagrar följande klustret skapas. Öppna hello Azure portal post för ett Data Lake store och gå för**Data Explorer > åtkomst > Lägg till**. 

### <a name="how-tooaccess-hello-data-lake-store-from-r-server"></a>Hur tooaccess hello Data Lake store från R Server

När du har gett åtkomst till tooa Data Lake store, kan du använda hello arkivet i R Server på HDInsight hello sätt som en sekundär Azure storage-konto. Hej endast skillnaden är att hello prefix **wasb: / /** ändras för**adl: / /** på följande sätt:


    # Point toohello ADL store (e.g. ADLtest)
    myNameNode <- "adl://rkadl1.azuredatalakestore.net"
    myPort <- 0

    # Location of hello data (assumes a /share directory on hello ADL account)
    bigDataDirRoot <- "/share"  

    # Define Spark compute context
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    # Set compute context
    rxSetComputeContext(mySparkCluster)

    # Define HDFS file system
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    # Specify hello input file in HDFS tooanalyze
    inputFile <-file.path(bigDataDirRoot,"AirlineDemoSmall.csv")

    # Create factors for days of hello week
    colInfo <- list(DayOfWeek = list(type = "factor",
               levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
                          "Friday", "Saturday", "Sunday")))

    # Define hello data source
    airDS <- RxTextData(file = inputFile, missingValueString = "M",
                    colInfo  = colInfo, fileSystem = hdfsFS)

    # Run a linear regression
    model <- rxLinMod(ArrDelay~CRSDepTime+DayOfWeek, data = airDS)


hello följande kommandon användas tooconfigure hello Data Lake storage-konto med hello RevoShare directory och Lägg till hello CSV-exempelfil från hello föregående exempel:


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server"></a>Använda Azure File storage med R Server

Det finns också ett praktiskt data lagringsalternativ för användning på hello kantnod kallas [Azure filer] ((https://azure.microsoft.com/services/storage/files/). Den låter dig toomount ett Azure Storage filen resursen toohello Linux-filsystem. Det här alternativet kan vara praktiskt för att lagra filer, R-skript och resultatobjekt som kan behövas senare, särskilt när det gör toouse mening hello filsystem på hello kantnod i stället för HDFS. 

En större fördelarna med Azure-filer är hello filen resurser kan monteras och används av alla system som har ett operativsystem som stöds, till exempel Windows eller Linux. Den kan till exempel användas av en annan HDInsight-kluster som du eller någon annan i din grupp har, av en Azure VM eller även genom ett lokalt system. Mer information finns i:

- [Hur toouse Azure File storage med Linux](../storage/files/storage-how-to-use-files-linux.md)
- [Hur toouse Azure File storage i Windows](../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a>Nästa steg

Nu när du förstår hello Azure lagringsalternativ, länkar Använd hello följande toodiscover sätt för att få datavetenskap uppgifter göras med R Server på HDInsight.

* [Översikt över R Server på HDInsight](hdinsight-hadoop-r-server-overview.md)
* [Komma igång med R server på Hadoop](hdinsight-hadoop-r-server-get-started.md)
* [Lägg till RStudio Server tooHDInsight (om inte läggs till när klustret skapas)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Alternativ för beräkningskontexter för R Server på HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)

