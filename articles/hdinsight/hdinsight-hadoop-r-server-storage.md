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
# <a name="azure-storage-solutions-for-r-server-on-hdinsight"></a><span data-ttu-id="c7d9d-103">Azure-lagringslösningar för R Server på HDInsight</span><span class="sxs-lookup"><span data-stu-id="c7d9d-103">Azure Storage solutions for R Server on HDInsight</span></span>

<span data-ttu-id="c7d9d-104">Microsoft R Server på HDInsight har olika lagring lösningar toopersist data, kod eller objekt som innehåller resultatet från analys.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-104">Microsoft R Server on HDInsight has a variety of storage solutions toopersist data, code, or objects that contain results from analysis.</span></span> <span data-ttu-id="c7d9d-105">Dessa omfattar hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="c7d9d-105">These include hello following options:</span></span>

- [<span data-ttu-id="c7d9d-106">Azure Blob</span><span class="sxs-lookup"><span data-stu-id="c7d9d-106">Azure Blob</span></span>](https://azure.microsoft.com/services/storage/blobs/)
- [<span data-ttu-id="c7d9d-107">Lagring av Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="c7d9d-107">Azure Data Lake Storage</span></span>](https://azure.microsoft.com/services/data-lake-store/)
- [<span data-ttu-id="c7d9d-108">Azure File storage</span><span class="sxs-lookup"><span data-stu-id="c7d9d-108">Azure File storage</span></span>](https://azure.microsoft.com/services/storage/files/)

<span data-ttu-id="c7d9d-109">Du har också hello möjlighet att komma åt flera Azure storage-konton eller behållare med HDI-klustret.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-109">You also have hello option of accessing multiple Azure storage accounts or containers with your HDI cluster.</span></span> <span data-ttu-id="c7d9d-110">Azure File storage är en lämplig data lagringsalternativ för användning på hello kantnod som du kan använda toomount en Azure Storage-filresurs som till exempel hello Linux-system.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-110">Azure File storage is a convenient data storage option for use on hello edge node that enables you toomount an Azure Storage file share to, for example, hello Linux file system.</span></span> <span data-ttu-id="c7d9d-111">Men Azure-filresurser kan monteras och används av alla system som har ett operativsystem som stöds, till exempel Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-111">But Azure File shares can be mounted and used by any system that has a supported OS such as Windows or Linux.</span></span> 

<span data-ttu-id="c7d9d-112">När du skapar ett Hadoop-kluster i HDInsight kan du ange antingen ett **Azure storage** konto eller ett **Data Lake store**.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-112">When you create an Hadoop cluster in HDInsight, you specify either an **Azure storage** account or a **Data Lake store**.</span></span> <span data-ttu-id="c7d9d-113">En specifik storage-behållare från detta konto innehåller hello filsystem för hello-kluster som du skapar (till exempel hello Hadoop Distributed File System).</span><span class="sxs-lookup"><span data-stu-id="c7d9d-113">A specific storage container from that account holds hello file system for hello cluster that you create (for example, hello Hadoop Distributed File System).</span></span> <span data-ttu-id="c7d9d-114">Mer information och vägledning finns:</span><span class="sxs-lookup"><span data-stu-id="c7d9d-114">For more information and guidance, see:</span></span>

- [<span data-ttu-id="c7d9d-115">Använda Azure storage med HDInsight</span><span class="sxs-lookup"><span data-stu-id="c7d9d-115">Use Azure storage with HDInsight</span></span>](hdinsight-hadoop-use-blob-storage.md)
- <span data-ttu-id="c7d9d-116">[Använd Data Lake Store med Azure HDInsight-kluster](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="c7d9d-116">[Use Data Lake Store with Azure HDInsight clusters](hdinsight-hadoop-use-data-lake-store.md).</span></span> 

<span data-ttu-id="c7d9d-117">Mer information om hello Azure-lagringslösningar finns [introduktion tooMicrosoft Azure Storage](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c7d9d-117">For more information on hello Azure storage solutions, see [Introduction tooMicrosoft Azure Storage](../storage/common/storage-introduction.md).</span></span> 

<span data-ttu-id="c7d9d-118">Anvisningar om hur du väljer hello lämpligaste lagring alternativet toouse för ditt scenario finns [ska när toouse Azure BLOB-objekt, Azure-filer eller Azure Datadiskar](../storage/common/storage-decide-blobs-files-disks.md)</span><span class="sxs-lookup"><span data-stu-id="c7d9d-118">For guidance on selecting hello most appropriate storage option toouse for your scenario, see [Deciding when toouse Azure Blobs, Azure Files, or Azure Data Disks](../storage/common/storage-decide-blobs-files-disks.md)</span></span> 


## <a name="use-azure-blob-storage-accounts-with-r-server"></a><span data-ttu-id="c7d9d-119">Använda Azure Blob storage-konton med R Server</span><span class="sxs-lookup"><span data-stu-id="c7d9d-119">Use Azure Blob storage accounts with R Server</span></span>

<span data-ttu-id="c7d9d-120">Om det behövs kan du komma åt flera Azure storage-konton eller behållare med HDI-klustret.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-120">If necessary, you can access multiple Azure storage accounts or containers with your HDI cluster.</span></span> <span data-ttu-id="c7d9d-121">toodo, du behöver toospecify hello ytterligare lagringskonton i hello Användargränssnittet när du skapar hello klustret och sedan följa dessa steg toouse dem med R Server.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-121">toodo so, you need toospecify hello additional storage accounts in hello UI when you create hello cluster, and then follow these steps toouse them with R Server.</span></span>

> [!WARNING]
> <span data-ttu-id="c7d9d-122">För prestanda, hello HDInsight-kluster skapas i hello samma datacenter som hello primära storage-konto som du anger.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-122">For performance purposes, hello HDInsight cluster is created in hello same data center as hello primary storage account that you specify.</span></span> <span data-ttu-id="c7d9d-123">Med hjälp av ett lagringskonto i en annan plats än hello HDInsight-kluster stöds inte.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-123">Using a storage account in a different location than hello HDInsight cluster is not supported.</span></span>

1. <span data-ttu-id="c7d9d-124">Skapa ett HDInsight-kluster med namnet på ett lagringskonto för **storage1** och en standardbehållare kallas **container1**.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-124">Create an HDInsight cluster with a storage account name of **storage1** and a default container called **container1**.</span></span>
2. <span data-ttu-id="c7d9d-125">Ange ett konto för ytterligare lagringsutrymme som heter **storage2**.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-125">Specify an additional storage account called **storage2**.</span></span>  
3. <span data-ttu-id="c7d9d-126">Kopiera hello mycsv.csv toohello /share katalog och utföra analyser på filen.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-126">Copy hello mycsv.csv file toohello /share directory, and perform analysis on that file.</span></span>  

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal myscsv.scv /share  

4. <span data-ttu-id="c7d9d-127">R-koden ange hello namn nod för**standard** och ange din katalog- och tooprocess.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-127">In R code, set hello name node too**default,** and set your directory and file tooprocess.</span></span>  

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

<span data-ttu-id="c7d9d-128">Alla hello katalog- och lagringskontot för referenser punkt toohello wasb://container1@storage1.blob.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-128">All hello directory and file references point toohello storage account wasb://container1@storage1.blob.core.windows.net.</span></span> <span data-ttu-id="c7d9d-129">Detta är hello **standard lagringskonto** som associeras med hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-129">This is hello **default storage account** that's associated with hello HDInsight cluster.</span></span>

<span data-ttu-id="c7d9d-130">Nu kan anta att du vill tooprocess en fil som heter mySpecial.csv som finns i hello /private för **container2** i **storage2**.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-130">Now, suppose you want tooprocess a file called mySpecial.csv that's located in hello  /private directory of **container2** in **storage2**.</span></span>

<span data-ttu-id="c7d9d-131">Punkt i koden R hello namn nod referens toohello **storage2** storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-131">In your R code, point hello name node reference toohello **storage2** storage account.</span></span>


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

<span data-ttu-id="c7d9d-132">Alla hello katalog- och referenser nu punkt toohello lagringskonto wasb://container2@storage2.blob.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-132">All of hello directory and file references now point toohello storage account wasb://container2@storage2.blob.core.windows.net.</span></span> <span data-ttu-id="c7d9d-133">Detta är hello **namn nod** som du har angett.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-133">This is hello **Name Node** that you’ve specified.</span></span>

<span data-ttu-id="c7d9d-134">Du har tooconfigure hello/User/RevoShare/<SSH username> på **storage2** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c7d9d-134">You have tooconfigure hello /user/RevoShare/<SSH username> directory on **storage2** as follows:</span></span>


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>



## <a name="use-an-azure-data-lake-store-with-r-server"></a><span data-ttu-id="c7d9d-135">Använda en Azure Data Lake store med R Server</span><span class="sxs-lookup"><span data-stu-id="c7d9d-135">Use an Azure Data Lake store with R Server</span></span>

<span data-ttu-id="c7d9d-136">toouse Data Lake lagrar med ditt HDInsight-konto måste du toogive din klustret åtkomst tooeach Azure Data Lake store som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-136">toouse Data Lake stores with your HDInsight account, you need toogive your cluster access tooeach Azure Data Lake store that you want toouse.</span></span> <span data-ttu-id="c7d9d-137">Anvisningar om hur toouse hello Azure portal toocreate ett HDInsight-kluster med ett Azure Data Lake Store-konto som hello standardlagring eller som en ytterligare store, finns [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c7d9d-137">For instructions on how toouse hello Azure portal toocreate a HDInsight cluster with an Azure Data Lake Store account as hello default storage or as an additional store, see [Create an HDInsight cluster with Data Lake Store using Azure portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

<span data-ttu-id="c7d9d-138">Du sedan använda hello store i din R-skriptet mycket som du gjorde en sekundär Azure storage-konto som beskrivs i föregående procedur för hello.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-138">You then use hello store in your R script much like you did a secondary Azure storage account as described in hello previous procedure.</span></span>

### <a name="add-cluster-access-tooyour-azure-data-lake-stores"></a><span data-ttu-id="c7d9d-139">Lägg till klustret åtkomst tooyour Azure Data Lake lagrar</span><span class="sxs-lookup"><span data-stu-id="c7d9d-139">Add cluster access tooyour Azure Data Lake stores</span></span>
<span data-ttu-id="c7d9d-140">Du har åtkomst till ett Data Lake store med hjälp av ett huvudnamn för tjänsten Azure Active Directory (AD Azure) som är kopplad till ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-140">You access a Data Lake store by using an Azure Active Directory (Azure AD) Service Principal that's associated with your HDInsight cluster.</span></span>

<span data-ttu-id="c7d9d-141">tooadd en Azure AD tjänstens huvudnamn:</span><span class="sxs-lookup"><span data-stu-id="c7d9d-141">tooadd an Azure AD Service Principal:</span></span>

1. <span data-ttu-id="c7d9d-142">När du skapar ditt HDInsight-kluster, Välj **kluster-AAD-identitet** från hello **datakällan** fliken.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-142">When you create your HDInsight cluster, select **Cluster AAD Identity** from hello **Data Source** tab.</span></span>

2. <span data-ttu-id="c7d9d-143">I hello **kluster-AAD-identitet** dialogrutan under **Välj AD tjänstens huvudnamn**väljer **Skapa nytt**.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-143">In hello **Cluster AAD Identity** dialog box, under **Select AD Service Principal**, select **Create new**.</span></span>

<span data-ttu-id="c7d9d-144">När du namnge hello tjänstens huvudnamn och skapa ett lösenord för det, klickar du på **hanterar ADLS-åtkomst** tooassociate hello tjänstens huvudnamn med Data Lake lagras.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-144">After you give hello Service Principal a name and create a password for it, click **Manage ADLS Access** tooassociate hello Service Principal with your Data Lake stores.</span></span>

<span data-ttu-id="c7d9d-145">Det är också möjligt tooadd klustret åtkomst tooone eller mer Data Lake lagrar följande klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-145">It’s also possible tooadd cluster access tooone or more Data Lake stores following cluster creation.</span></span> <span data-ttu-id="c7d9d-146">Öppna hello Azure portal post för ett Data Lake store och gå för**Data Explorer > åtkomst > Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-146">Open hello Azure portal entry for a Data Lake store and go too**Data Explorer > Access > Add**.</span></span> 

### <a name="how-tooaccess-hello-data-lake-store-from-r-server"></a><span data-ttu-id="c7d9d-147">Hur tooaccess hello Data Lake store från R Server</span><span class="sxs-lookup"><span data-stu-id="c7d9d-147">How tooaccess hello Data Lake store from R Server</span></span>

<span data-ttu-id="c7d9d-148">När du har gett åtkomst till tooa Data Lake store, kan du använda hello arkivet i R Server på HDInsight hello sätt som en sekundär Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-148">Once you’ve given access tooa Data Lake store, you can use hello store in R Server on HDInsight hello way you would a secondary Azure storage account.</span></span> <span data-ttu-id="c7d9d-149">Hej endast skillnaden är att hello prefix **wasb: / /** ändras för**adl: / /** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c7d9d-149">hello only difference is that hello prefix **wasb://** changes too**adl://** as follows:</span></span>


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


<span data-ttu-id="c7d9d-150">hello följande kommandon användas tooconfigure hello Data Lake storage-konto med hello RevoShare directory och Lägg till hello CSV-exempelfil från hello föregående exempel:</span><span class="sxs-lookup"><span data-stu-id="c7d9d-150">hello following commands are used tooconfigure hello Data Lake storage account with hello RevoShare directory and add hello sample .csv file from hello previous example:</span></span>


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server"></a><span data-ttu-id="c7d9d-151">Använda Azure File storage med R Server</span><span class="sxs-lookup"><span data-stu-id="c7d9d-151">Use Azure File storage with R Server</span></span>

<span data-ttu-id="c7d9d-152">Det finns också ett praktiskt data lagringsalternativ för användning på hello kantnod kallas [Azure filer] ((https://azure.microsoft.com/services/storage/files/).</span><span class="sxs-lookup"><span data-stu-id="c7d9d-152">There is also a convenient data storage option for use on hello edge node called [Azure Files]((https://azure.microsoft.com/services/storage/files/).</span></span> <span data-ttu-id="c7d9d-153">Den låter dig toomount ett Azure Storage filen resursen toohello Linux-filsystem.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-153">It enables you toomount an Azure Storage file share toohello Linux file system.</span></span> <span data-ttu-id="c7d9d-154">Det här alternativet kan vara praktiskt för att lagra filer, R-skript och resultatobjekt som kan behövas senare, särskilt när det gör toouse mening hello filsystem på hello kantnod i stället för HDFS.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-154">This option can be handy for storing data files, R scripts, and result objects that might be needed later, especially when it makes sense toouse hello native file system on hello edge node rather than HDFS.</span></span> 

<span data-ttu-id="c7d9d-155">En större fördelarna med Azure-filer är hello filen resurser kan monteras och används av alla system som har ett operativsystem som stöds, till exempel Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-155">A major benefit of Azure Files is that hello file shares can be mounted and used by any system that has a supported OS such as Windows or Linux.</span></span> <span data-ttu-id="c7d9d-156">Den kan till exempel användas av en annan HDInsight-kluster som du eller någon annan i din grupp har, av en Azure VM eller även genom ett lokalt system.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-156">For example, it can be used by another HDInsight cluster that you or someone on your team has, by an Azure VM, or even by an on-premises system.</span></span> <span data-ttu-id="c7d9d-157">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="c7d9d-157">For more information, see:</span></span>

- [<span data-ttu-id="c7d9d-158">Hur toouse Azure File storage med Linux</span><span class="sxs-lookup"><span data-stu-id="c7d9d-158">How toouse Azure File storage with Linux</span></span>](../storage/files/storage-how-to-use-files-linux.md)
- [<span data-ttu-id="c7d9d-159">Hur toouse Azure File storage i Windows</span><span class="sxs-lookup"><span data-stu-id="c7d9d-159">How toouse Azure File storage on Windows</span></span>](../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a><span data-ttu-id="c7d9d-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c7d9d-160">Next steps</span></span>

<span data-ttu-id="c7d9d-161">Nu när du förstår hello Azure lagringsalternativ, länkar Använd hello följande toodiscover sätt för att få datavetenskap uppgifter göras med R Server på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c7d9d-161">Now that you understand hello Azure storage options, use hello following links toodiscover ways of getting data science tasks done with R Server on HDInsight.</span></span>

* [<span data-ttu-id="c7d9d-162">Översikt över R Server på HDInsight</span><span class="sxs-lookup"><span data-stu-id="c7d9d-162">Overview of R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-overview.md)
* [<span data-ttu-id="c7d9d-163">Komma igång med R server på Hadoop</span><span class="sxs-lookup"><span data-stu-id="c7d9d-163">Get started with R server on Hadoop</span></span>](hdinsight-hadoop-r-server-get-started.md)
* [<span data-ttu-id="c7d9d-164">Lägg till RStudio Server tooHDInsight (om inte läggs till när klustret skapas)</span><span class="sxs-lookup"><span data-stu-id="c7d9d-164">Add RStudio Server tooHDInsight (if not added during cluster creation)</span></span>](hdinsight-hadoop-r-server-install-r-studio.md)
* [<span data-ttu-id="c7d9d-165">Alternativ för beräkningskontexter för R Server på HDInsight</span><span class="sxs-lookup"><span data-stu-id="c7d9d-165">Compute context options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)

