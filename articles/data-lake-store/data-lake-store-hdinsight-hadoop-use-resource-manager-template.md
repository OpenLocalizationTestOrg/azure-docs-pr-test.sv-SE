---
title: aaaUse Azure-mallar toocreate HDInsight och Data Lake Store | Microsoft Docs
description: "Använda Azure Resource Manager mallar toocreate och använda HDInsight-kluster med Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8ef8152f-2121-461e-956c-51c55144919d
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: nitinme
ms.openlocfilehash: eb88a626f2837dcc29295f3f73a91757059c3bb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-resource-manager-template"></a><span data-ttu-id="ab704-103">Skapa ett HDInsight-kluster med Data Lake Store med Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="ab704-103">Create an HDInsight cluster with Data Lake Store using Azure Resource Manager template</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ab704-104">Använda portalen</span><span class="sxs-lookup"><span data-stu-id="ab704-104">Using Portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="ab704-105">Med hjälp av PowerShell (för standardlagring)</span><span class="sxs-lookup"><span data-stu-id="ab704-105">Using PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="ab704-106">Med hjälp av PowerShell (för ytterligare lagringsutrymme)</span><span class="sxs-lookup"><span data-stu-id="ab704-106">Using PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="ab704-107">Med Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ab704-107">Using Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

<span data-ttu-id="ab704-108">Lär dig hur toouse Azure PowerShell tooconfigure ett HDInsight-kluster med Azure Data Lake Store **som ytterligare lagringsutrymme**.</span><span class="sxs-lookup"><span data-stu-id="ab704-108">Learn how toouse Azure PowerShell tooconfigure an HDInsight cluster with Azure Data Lake Store, **as additional storage**.</span></span>

<span data-ttu-id="ab704-109">För stöds klustertyper användas Data Lake Store som standardlagring eller ytterligare storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ab704-109">For supported cluster types, Data Lake Store be used as an default storage or additional storage account.</span></span> <span data-ttu-id="ab704-110">När du använder Data Lake Store som ytterligare lagringsutrymme hello standardkontot för lagring för hello kluster kommer fortfarande att Azure Storage BLOB (WASB) och hello kluster-relaterade filer (till exempel loggar osv.) skrivs fortfarande toohello standardlagring medan hello data som du vill tooprocess kan lagras i ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ab704-110">When Data Lake Store is used as additional storage, hello default storage account for hello clusters will still be Azure Storage Blobs (WASB) and hello cluster-related files (such as logs, etc.) are still written toohello default storage, while hello data that you want tooprocess can be stored in a Data Lake Store account.</span></span> <span data-ttu-id="ab704-111">Med Data Lake Store som ett ytterligare storage-konto inte påverkar prestanda eller hello möjlighet tooread/skrivning toohello lagring från hello kluster.</span><span class="sxs-lookup"><span data-stu-id="ab704-111">Using Data Lake Store as an additional storage account does not impact performance or hello ability tooread/write toohello storage from hello cluster.</span></span>

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a><span data-ttu-id="ab704-112">Med hjälp av Data Lake Store för HDInsight klusterlagring</span><span class="sxs-lookup"><span data-stu-id="ab704-112">Using Data Lake Store for HDInsight cluster storage</span></span>

<span data-ttu-id="ab704-113">Här följer några viktiga överväganden när för att använda HDInsight med Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="ab704-113">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="ab704-114">Alternativet toocreate HDInsight-kluster med åtkomst tooData Datasjölager som standardlagring är tillgänglig för HDInsight version 3.5 och 3,6.</span><span class="sxs-lookup"><span data-stu-id="ab704-114">Option toocreate HDInsight clusters with access tooData Lake Store as default storage is available for HDInsight version 3.5 and 3.6.</span></span>

* <span data-ttu-id="ab704-115">Alternativet toocreate HDInsight-kluster med åtkomst tooData Datasjölager som ytterligare lagring är tillgänglig för HDInsight version 3.2, 3.4, 3.5 och 3,6.</span><span class="sxs-lookup"><span data-stu-id="ab704-115">Option toocreate HDInsight clusters with access tooData Lake Store as additional storage is available for HDInsight versions 3.2, 3.4, 3.5, and 3.6.</span></span>

<span data-ttu-id="ab704-116">I den här artikeln etablera vi ett Hadoop-kluster med Data Lake Store som ytterligare lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="ab704-116">In this article, we provision a Hadoop cluster with Data Lake Store as additional storage.</span></span> <span data-ttu-id="ab704-117">Anvisningar för hur toocreate ett Hadoop-kluster med Data Lake Store som standardlagring finns [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ab704-117">For instructions on how toocreate a Hadoop cluster with Data Lake Store as default storage, see [Create an HDInsight cluster with Data Lake Store using Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab704-118">Krav</span><span class="sxs-lookup"><span data-stu-id="ab704-118">Prerequisites</span></span>
<span data-ttu-id="ab704-119">Innan du påbörjar den här självstudien måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="ab704-119">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="ab704-120">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="ab704-120">**An Azure subscription**.</span></span> <span data-ttu-id="ab704-121">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ab704-121">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="ab704-122">**Installera Azure PowerShell 1.0 eller senare**.</span><span class="sxs-lookup"><span data-stu-id="ab704-122">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="ab704-123">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ab704-123">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="ab704-124">**Azure Active Directory Service Principal**.</span><span class="sxs-lookup"><span data-stu-id="ab704-124">**Azure Active Directory Service Principal**.</span></span> <span data-ttu-id="ab704-125">Stegen i den här kursen ger instruktioner för hur toocreate ett huvudnamn för tjänsten i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ab704-125">Steps in this tutorial provide instructions on how toocreate a service principal in Azure AD.</span></span> <span data-ttu-id="ab704-126">Du måste dock vara en kan toocreate för Azure AD-administratör toobe på ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ab704-126">However, you must be an Azure AD administrator toobe able toocreate a service principal.</span></span> <span data-ttu-id="ab704-127">Om du är administratör för Azure AD kan du hoppa över det här kravet och fortsätt med hello kursen.</span><span class="sxs-lookup"><span data-stu-id="ab704-127">If you are an Azure AD administrator, you can skip this prerequisite and proceed with hello tutorial.</span></span>

    <span data-ttu-id="ab704-128">**Om du inte är en Azure AD-administratör**, kommer du inte att kunna tooperform hello steg krävs toocreate ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ab704-128">**If you are not an Azure AD administrator**, you will not be able tooperform hello steps required toocreate a service principal.</span></span> <span data-ttu-id="ab704-129">I sådana fall måste din Azure AD-administratör först skapa ett huvudnamn för tjänsten innan du kan skapa ett HDInsight-kluster med Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ab704-129">In such a case, your Azure AD administrator must first create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="ab704-130">Dessutom hello tjänstens huvudnamn måste skapas med hjälp av ett certifikat, enligt beskrivningen i [skapa ett huvudnamn för tjänsten med certifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="ab704-130">Also, hello service principal must be created using a certificate, as described at [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>

## <a name="create-an-hdinsight-cluster-with-azure-data-lake-store"></a><span data-ttu-id="ab704-131">Skapa ett HDInsight-kluster med Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ab704-131">Create an HDInsight cluster with Azure Data Lake Store</span></span>
<span data-ttu-id="ab704-132">hello Resource Manager-mall och hello förutsättningarna för att använda hello-mallen finns på GitHub på [distribuera ett HDInsight Linux-kluster med nya Data Lake Store](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage).</span><span class="sxs-lookup"><span data-stu-id="ab704-132">hello Resource Manager template, and hello prerequisites for using hello template, are available on GitHub at [Deploy a HDInsight Linux cluster with new Data Lake Store](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage).</span></span> <span data-ttu-id="ab704-133">Följ hello instruktionerna på den här länken toocreate ett HDInsight-kluster med Azure Data Lake Store som hello ytterligare lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="ab704-133">Follow hello instructions provided at this link toocreate an HDInsight cluster with Azure Data Lake Store as hello additional storage.</span></span>

<span data-ttu-id="ab704-134">hello instruktionerna på hello länken ovan kräver PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ab704-134">hello instructions at hello link mentioned above require PowerShell.</span></span> <span data-ttu-id="ab704-135">Innan du börjar med dessa instruktioner måste du kontrollera att du loggar in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ab704-135">Before you start with those instructions, make sure you log in tooyour Azure account.</span></span> <span data-ttu-id="ab704-136">Öppna ett nytt Azure PowerShell-fönster på skrivbordet och ange hello följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="ab704-136">From your desktop, open a new Azure PowerShell window, and enter hello following snippets.</span></span> <span data-ttu-id="ab704-137">När du tillfrågas toolog i, se till att logga in som en hello prenumerationens admininistratörer/ägare:</span><span class="sxs-lookup"><span data-stu-id="ab704-137">When prompted toolog in, make sure you log in as one of hello subscription admininistrators/owner:</span></span>

```
# Log in tooyour Azure account
Login-AzureRmAccount

# List all hello subscriptions associated tooyour account
Get-AzureRmSubscription

# Select a subscription
Set-AzureRmContext -SubscriptionId <subscription ID>
```

## <a name="upload-sample-data-toohello-azure-data-lake-store"></a><span data-ttu-id="ab704-138">Överför exempel data toohello Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ab704-138">Upload sample data toohello Azure Data Lake Store</span></span>
<span data-ttu-id="ab704-139">hello Resource Manager-mall skapas ett nytt Data Lake Store-konto och associerar den med hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ab704-139">hello Resource Manager template creates a new Data Lake Store account and associates it with hello HDInsight cluster.</span></span> <span data-ttu-id="ab704-140">Nu måste du överföra några exempel på en data-toohello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ab704-140">You must now upload some sample data toohello Data Lake Store.</span></span> <span data-ttu-id="ab704-141">Du behöver dessa data senare i hello självstudiekursen toorun jobb från ett HDInsight-kluster som har åtkomst till data i hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ab704-141">You'll need this data later in hello tutorial toorun jobs from an HDInsight cluster that access data in hello Data Lake Store.</span></span> <span data-ttu-id="ab704-142">Anvisningar för hur tooupload data finns i [överför en fil tooyour Datasjölager](data-lake-store-get-started-portal.md#uploaddata).</span><span class="sxs-lookup"><span data-stu-id="ab704-142">For instructions on how tooupload data, see [Upload a file tooyour Data Lake Store](data-lake-store-get-started-portal.md#uploaddata).</span></span> <span data-ttu-id="ab704-143">Om du vill söka efter vissa exempel data tooupload, kan du få hello **Ambulansdata** mapp från hello [Azure Data Lake Git-lagringsplatsen](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="ab704-143">If you are looking for some sample data tooupload, you can get hello **Ambulance Data** folder from hello [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span>

## <a name="set-relevant-acls-on-hello-sample-data"></a><span data-ttu-id="ab704-144">Ange relevant ACL: er på hello exempeldata</span><span class="sxs-lookup"><span data-stu-id="ab704-144">Set relevant ACLs on hello sample data</span></span>
<span data-ttu-id="ab704-145">toomake hello exempeldata som du överför är åtkomlig från hello HDInsight-kluster, måste du se till att hello Azure AD-program som används tooestablish identitet mellan hello HDInsight-kluster och Data Lake Store har du åtkomst toohello fil/mapp försöker tooaccess.</span><span class="sxs-lookup"><span data-stu-id="ab704-145">toomake sure hello sample data you upload is accessible from hello HDInsight cluster, you must ensure that hello Azure AD application that is used tooestablish identity between hello HDInsight cluster and Data Lake Store has access toohello file/folder you are trying tooaccess.</span></span> <span data-ttu-id="ab704-146">toodo, utför följande steg hello.</span><span class="sxs-lookup"><span data-stu-id="ab704-146">toodo this, perform hello following steps.</span></span>

1. <span data-ttu-id="ab704-147">Hitta hello namn hello Azure AD-program som är associerat med HDInsight-kluster och hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ab704-147">Find hello name of hello Azure AD application that is associated with HDInsight cluster and hello Data Lake Store.</span></span> <span data-ttu-id="ab704-148">Enkelriktade toolook för hello namn är tooopen hello HDInsight-klusterbladet som du skapade med hello Resource Manager-mall, klickar du på hello **kluster-AAD-identitet** fliken och Sök efter hello värdet för **tjänstens huvudnamn Visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="ab704-148">One way toolook for hello name is tooopen hello HDInsight cluster blade that you created using hello Resource Manager template, click hello **Cluster AAD Identity** tab, and look for hello value of **Service Principal Display Name**.</span></span>
2. <span data-ttu-id="ab704-149">Nu kan ange åtkomst toothis Azure AD-program på hello filen/mappen som du vill tooaccess från hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ab704-149">Now, provide access toothis Azure AD application on hello file/folder that you want tooaccess from hello HDInsight cluster.</span></span> <span data-ttu-id="ab704-150">tooset hello höger ACL: er på hello fil/mapp i Data Lake Store finns [skydda data i Data Lake Store](data-lake-store-secure-data.md#filepermissions).</span><span class="sxs-lookup"><span data-stu-id="ab704-150">tooset hello right ACLs on hello file/folder in Data Lake Store, see [Securing data in Data Lake Store](data-lake-store-secure-data.md#filepermissions).</span></span>

## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-hello-data-lake-store"></a><span data-ttu-id="ab704-151">Kör testjobb på hello HDInsight-kluster toouse hello Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ab704-151">Run test jobs on hello HDInsight cluster toouse hello Data Lake Store</span></span>
<span data-ttu-id="ab704-152">När du har konfigurerat ett HDInsight-kluster, kan du köra testjobb på hello klustret tootest som hello HDInsight klustret kan komma åt Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ab704-152">After you have configured an HDInsight cluster, you can run test jobs on hello cluster tootest that hello HDInsight cluster can access Data Lake Store.</span></span> <span data-ttu-id="ab704-153">toodo så vi ska köra en Hive-jobb som skapar en tabell med hello exempeldata som du överfört tidigare tooyour Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ab704-153">toodo so, we will run a sample Hive job that creates a table using hello sample data that you uploaded earlier tooyour Data Lake Store.</span></span>

<span data-ttu-id="ab704-154">I det här avsnittet kommer du att SSH i ett kluster i HDInsight Linux och köra hello en exempelfråga Hive.</span><span class="sxs-lookup"><span data-stu-id="ab704-154">In this section you will SSH into an HDInsight Linux cluster and run hello a sample Hive query.</span></span> <span data-ttu-id="ab704-155">Om du använder en Windows-klient, bör du använda **PuTTY**, som kan hämtas från [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="ab704-155">If you are using a Windows client, we recommend using **PuTTY**, which can be downloaded from [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

<span data-ttu-id="ab704-156">Läs mer om hur du använder PuTTY [använda SSH med Linux-baserade Hadoop i HDInsight från Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="ab704-156">For more information on using PuTTY, see [Use SSH with Linux-based Hadoop on HDInsight from Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>

1. <span data-ttu-id="ab704-157">När du är ansluten, starta hello Hive CLI med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ab704-157">Once connected, start hello Hive CLI by using hello following command:</span></span>

   ```
   hive
   ```
2. <span data-ttu-id="ab704-158">Med hjälp av hello CLI, ange hello följande instruktioner toocreate en ny tabell med namnet **fordon** med hello exempeldata i hello Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="ab704-158">Using hello CLI, enter hello following statements toocreate a new table named **vehicles** by using hello sample data in hello Data Lake Store:</span></span>

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
   SELECT * FROM vehicles LIMIT 10;
   ```

   <span data-ttu-id="ab704-159">Du bör se en utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ab704-159">You should see an output similar toohello following:</span></span>

   ```
   1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
   1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
   1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
   1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
   1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
   1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
   1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
   1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
   1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
   1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1
   ```


## <a name="access-data-lake-store-using-hdfs-commands"></a><span data-ttu-id="ab704-160">Åtkomst till Data Lake Store med hjälp av HDFS-kommandon</span><span class="sxs-lookup"><span data-stu-id="ab704-160">Access Data Lake Store using HDFS commands</span></span>
<span data-ttu-id="ab704-161">När du har konfigurerat hello HDInsight klustret toouse Data Lake Store kan använda du hello HDFS shell-kommandon tooaccess hello store.</span><span class="sxs-lookup"><span data-stu-id="ab704-161">Once you have configured hello HDInsight cluster toouse Data Lake Store, you can use hello HDFS shell commands tooaccess hello store.</span></span>

<span data-ttu-id="ab704-162">I det här avsnittet kommer du att SSH i ett kluster i HDInsight Linux och köra hello HDFS-kommandon.</span><span class="sxs-lookup"><span data-stu-id="ab704-162">In this section you will SSH into an HDInsight Linux cluster and run hello HDFS commands.</span></span> <span data-ttu-id="ab704-163">Om du använder en Windows-klient, bör du använda **PuTTY**, som kan hämtas från [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="ab704-163">If you are using a Windows client, we recommend using **PuTTY**, which can be downloaded from [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

<span data-ttu-id="ab704-164">Läs mer om hur du använder PuTTY [använda SSH med Linux-baserade Hadoop i HDInsight från Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="ab704-164">For more information on using PuTTY, see [Use SSH with Linux-based Hadoop on HDInsight from Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>

<span data-ttu-id="ab704-165">När du är ansluten, Använd hello följande HDFS filesystem kommandot toolist hello filer i hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ab704-165">Once connected, use hello following HDFS filesystem command toolist hello files in hello Data Lake Store.</span></span>

```
hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/
```

<span data-ttu-id="ab704-166">Du bör nu se hello-fil som du överfört tidigare toohello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ab704-166">This should list hello file that you uploaded earlier toohello Data Lake Store.</span></span>

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder
```

<span data-ttu-id="ab704-167">Du kan också använda hello `hdfs dfs -put` kommandot tooupload vissa filer toohello Data Lake Store och sedan använda `hdfs dfs -ls` tooverify hello filer har laddats.</span><span class="sxs-lookup"><span data-stu-id="ab704-167">You can also use hello `hdfs dfs -put` command tooupload some files toohello Data Lake Store, and then use `hdfs dfs -ls` tooverify whether hello files were successfully uploaded.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ab704-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ab704-168">Next steps</span></span>
* [<span data-ttu-id="ab704-169">Kopiera data från Azure Storage BLOB tooData Datasjölager</span><span class="sxs-lookup"><span data-stu-id="ab704-169">Copy data from Azure Storage Blobs tooData Lake Store</span></span>](data-lake-store-copy-data-wasb-distcp.md)
