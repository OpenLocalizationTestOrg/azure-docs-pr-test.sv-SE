---
title: "Använda Azure-mallar för att skapa HDInsight och Data Lake Store | Microsoft Docs"
description: "Använd Azure Resource Manager-mallar för att skapa och använda HDInsight-kluster med Azure Data Lake Store"
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
ms.openlocfilehash: 6f43423096f0e74f41afea275e4ec9801dc2cea5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-resource-manager-template"></a><span data-ttu-id="23a88-103">Skapa ett HDInsight-kluster med Data Lake Store med Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="23a88-103">Create an HDInsight cluster with Data Lake Store using Azure Resource Manager template</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="23a88-104">Använda portalen</span><span class="sxs-lookup"><span data-stu-id="23a88-104">Using Portal</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [<span data-ttu-id="23a88-105">Med hjälp av PowerShell (för standardlagring)</span><span class="sxs-lookup"><span data-stu-id="23a88-105">Using PowerShell (for default storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [<span data-ttu-id="23a88-106">Med hjälp av PowerShell (för ytterligare lagringsutrymme)</span><span class="sxs-lookup"><span data-stu-id="23a88-106">Using PowerShell (for additional storage)</span></span>](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [<span data-ttu-id="23a88-107">Med Resource Manager</span><span class="sxs-lookup"><span data-stu-id="23a88-107">Using Resource Manager</span></span>](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

<span data-ttu-id="23a88-108">Lär dig hur du konfigurerar ett HDInsight-kluster med Azure Data Lake Store med hjälp av Azure PowerShell **som ytterligare lagringsutrymme**.</span><span class="sxs-lookup"><span data-stu-id="23a88-108">Learn how to use Azure PowerShell to configure an HDInsight cluster with Azure Data Lake Store, **as additional storage**.</span></span>

<span data-ttu-id="23a88-109">För stöds klustertyper användas Data Lake Store som standardlagring eller ytterligare storage-konto.</span><span class="sxs-lookup"><span data-stu-id="23a88-109">For supported cluster types, Data Lake Store be used as an default storage or additional storage account.</span></span> <span data-ttu-id="23a88-110">När du använder Data Lake Store som ytterligare lagringsutrymme standardkontot för lagring för kluster kommer fortfarande att Azure Storage BLOB (WASB) och kluster-relaterade-filer (till exempel loggar osv.) är fortfarande skrivs till standardlagring medan de data som du vill bearbeta kan lagras i ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="23a88-110">When Data Lake Store is used as additional storage, the default storage account for the clusters will still be Azure Storage Blobs (WASB) and the cluster-related files (such as logs, etc.) are still written to the default storage, while the data that you want to process can be stored in a Data Lake Store account.</span></span> <span data-ttu-id="23a88-111">Med Data Lake Store som ett ytterligare storage-konto inte påverkar prestanda eller ge möjlighet att läsa eller skriva till lagring från klustret.</span><span class="sxs-lookup"><span data-stu-id="23a88-111">Using Data Lake Store as an additional storage account does not impact performance or the ability to read/write to the storage from the cluster.</span></span>

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a><span data-ttu-id="23a88-112">Med hjälp av Data Lake Store för HDInsight klusterlagring</span><span class="sxs-lookup"><span data-stu-id="23a88-112">Using Data Lake Store for HDInsight cluster storage</span></span>

<span data-ttu-id="23a88-113">Här följer några viktiga överväganden när för att använda HDInsight med Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="23a88-113">Here are some important considerations for using HDInsight with Data Lake Store:</span></span>

* <span data-ttu-id="23a88-114">Alternativet för att skapa HDInsight-kluster med åtkomst till Data Lake Store som standard lagringsutrymmet är tillgängligt för HDInsight version 3.5 och 3,6.</span><span class="sxs-lookup"><span data-stu-id="23a88-114">Option to create HDInsight clusters with access to Data Lake Store as default storage is available for HDInsight version 3.5 and 3.6.</span></span>

* <span data-ttu-id="23a88-115">Alternativet för att skapa HDInsight-kluster med åtkomst till Data Lake Store som ytterligare lagringsutrymmet är tillgängligt för HDInsight version 3.2, 3.4, 3.5 och 3,6.</span><span class="sxs-lookup"><span data-stu-id="23a88-115">Option to create HDInsight clusters with access to Data Lake Store as additional storage is available for HDInsight versions 3.2, 3.4, 3.5, and 3.6.</span></span>

<span data-ttu-id="23a88-116">I den här artikeln etablera vi ett Hadoop-kluster med Data Lake Store som ytterligare lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="23a88-116">In this article, we provision a Hadoop cluster with Data Lake Store as additional storage.</span></span> <span data-ttu-id="23a88-117">Anvisningar om hur du skapar ett Hadoop-kluster med Data Lake Store som standardlagring finns [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="23a88-117">For instructions on how to create a Hadoop cluster with Data Lake Store as default storage, see [Create an HDInsight cluster with Data Lake Store using Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="23a88-118">Krav</span><span class="sxs-lookup"><span data-stu-id="23a88-118">Prerequisites</span></span>
<span data-ttu-id="23a88-119">Innan du påbörjar de här självstudierna måste du ha:</span><span class="sxs-lookup"><span data-stu-id="23a88-119">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="23a88-120">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="23a88-120">**An Azure subscription**.</span></span> <span data-ttu-id="23a88-121">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="23a88-121">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="23a88-122">**Installera Azure PowerShell 1.0 eller senare**.</span><span class="sxs-lookup"><span data-stu-id="23a88-122">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="23a88-123">Se [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="23a88-123">See [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="23a88-124">**Azure Active Directory Service Principal**.</span><span class="sxs-lookup"><span data-stu-id="23a88-124">**Azure Active Directory Service Principal**.</span></span> <span data-ttu-id="23a88-125">Stegen i den här kursen ger instruktioner för hur du skapar ett huvudnamn för tjänsten i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="23a88-125">Steps in this tutorial provide instructions on how to create a service principal in Azure AD.</span></span> <span data-ttu-id="23a88-126">Du måste dock vara en Azure AD-administratör för att kunna skapa ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="23a88-126">However, you must be an Azure AD administrator to be able to create a service principal.</span></span> <span data-ttu-id="23a88-127">Om du är administratör för Azure AD kan du hoppa över det här kravet och fortsätta med guiden.</span><span class="sxs-lookup"><span data-stu-id="23a88-127">If you are an Azure AD administrator, you can skip this prerequisite and proceed with the tutorial.</span></span>

    <span data-ttu-id="23a88-128">**Om du inte är en Azure AD-administratör**, kommer du inte kunna utföra stegen som krävs för att skapa ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="23a88-128">**If you are not an Azure AD administrator**, you will not be able to perform the steps required to create a service principal.</span></span> <span data-ttu-id="23a88-129">I sådana fall måste din Azure AD-administratör först skapa ett huvudnamn för tjänsten innan du kan skapa ett HDInsight-kluster med Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23a88-129">In such a case, your Azure AD administrator must first create a service principal before you can create an HDInsight cluster with Data Lake Store.</span></span> <span data-ttu-id="23a88-130">Dessutom tjänstens huvudnamn måste skapas med hjälp av ett certifikat, enligt beskrivningen i [skapa ett huvudnamn för tjänsten med certifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span><span class="sxs-lookup"><span data-stu-id="23a88-130">Also, the service principal must be created using a certificate, as described at [Create a service principal with certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).</span></span>

## <a name="create-an-hdinsight-cluster-with-azure-data-lake-store"></a><span data-ttu-id="23a88-131">Skapa ett HDInsight-kluster med Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="23a88-131">Create an HDInsight cluster with Azure Data Lake Store</span></span>
<span data-ttu-id="23a88-132">Resource Manager-mallen och krav för att använda mallen finns på GitHub på [distribuera ett HDInsight Linux-kluster med nya Data Lake Store](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage).</span><span class="sxs-lookup"><span data-stu-id="23a88-132">The Resource Manager template, and the prerequisites for using the template, are available on GitHub at [Deploy a HDInsight Linux cluster with new Data Lake Store](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage).</span></span> <span data-ttu-id="23a88-133">Följ anvisningarna i den här länken för att skapa ett HDInsight-kluster med Azure Data Lake Store som det extra lagringsutrymmet.</span><span class="sxs-lookup"><span data-stu-id="23a88-133">Follow the instructions provided at this link to create an HDInsight cluster with Azure Data Lake Store as the additional storage.</span></span>

<span data-ttu-id="23a88-134">Anvisningarna i länken ovan kräver PowerShell.</span><span class="sxs-lookup"><span data-stu-id="23a88-134">The instructions at the link mentioned above require PowerShell.</span></span> <span data-ttu-id="23a88-135">Innan du börjar med dessa instruktioner, kontrollera att du loggar in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="23a88-135">Before you start with those instructions, make sure you log in to your Azure account.</span></span> <span data-ttu-id="23a88-136">Öppna ett nytt Azure PowerShell-fönster på skrivbordet och ange följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="23a88-136">From your desktop, open a new Azure PowerShell window, and enter the following snippets.</span></span> <span data-ttu-id="23a88-137">När du uppmanas att logga in, se till att logga in som en av prenumerationens admininistratörer/ägare:</span><span class="sxs-lookup"><span data-stu-id="23a88-137">When prompted to log in, make sure you log in as one of the subscription admininistrators/owner:</span></span>

```
# Log in to your Azure account
Login-AzureRmAccount

# List all the subscriptions associated to your account
Get-AzureRmSubscription

# Select a subscription
Set-AzureRmContext -SubscriptionId <subscription ID>
```

## <a name="upload-sample-data-to-the-azure-data-lake-store"></a><span data-ttu-id="23a88-138">Ladda upp exempeldata till Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="23a88-138">Upload sample data to the Azure Data Lake Store</span></span>
<span data-ttu-id="23a88-139">Resource Manager-mallen skapar ett nytt Data Lake Store-konto och associerar den med HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="23a88-139">The Resource Manager template creates a new Data Lake Store account and associates it with the HDInsight cluster.</span></span> <span data-ttu-id="23a88-140">Du måste ladda upp exempeldata till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23a88-140">You must now upload some sample data to the Data Lake Store.</span></span> <span data-ttu-id="23a88-141">Du behöver den här informationen senare i guiden för att köra jobb från ett HDInsight-kluster som har åtkomst till data i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23a88-141">You'll need this data later in the tutorial to run jobs from an HDInsight cluster that access data in the Data Lake Store.</span></span> <span data-ttu-id="23a88-142">Instruktioner om hur du överför data finns [överföra en fil till din Data Lake Store](data-lake-store-get-started-portal.md#uploaddata).</span><span class="sxs-lookup"><span data-stu-id="23a88-142">For instructions on how to upload data, see [Upload a file to your Data Lake Store](data-lake-store-get-started-portal.md#uploaddata).</span></span> <span data-ttu-id="23a88-143">Om du behöver exempeldata att ladda upp, kan du hämta mappen **Ambulansdata** från [Azure Data Lake Git-lagringsplatsen](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="23a88-143">If you are looking for some sample data to upload, you can get the **Ambulance Data** folder from the [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span>

## <a name="set-relevant-acls-on-the-sample-data"></a><span data-ttu-id="23a88-144">Ange relevant ACL: er för exempeldata</span><span class="sxs-lookup"><span data-stu-id="23a88-144">Set relevant ACLs on the sample data</span></span>
<span data-ttu-id="23a88-145">Om du vill kontrollera att du laddar upp exempeldata är tillgänglig från HDInsight-kluster, måste du kontrollera att Azure AD-program som används för att upprätta mellan HDInsight-kluster och Data Lake Store har åtkomst till filen/mappen du försöker komma åt.</span><span class="sxs-lookup"><span data-stu-id="23a88-145">To make sure the sample data you upload is accessible from the HDInsight cluster, you must ensure that the Azure AD application that is used to establish identity between the HDInsight cluster and Data Lake Store has access to the file/folder you are trying to access.</span></span> <span data-ttu-id="23a88-146">Utför följande steg om du vill göra detta.</span><span class="sxs-lookup"><span data-stu-id="23a88-146">To do this, perform the following steps.</span></span>

1. <span data-ttu-id="23a88-147">Hitta namnet på Azure AD-program som är associerat med HDInsight-kluster och Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23a88-147">Find the name of the Azure AD application that is associated with HDInsight cluster and the Data Lake Store.</span></span> <span data-ttu-id="23a88-148">Klicka på ett sätt att söka efter namnet på är att öppna bladet för HDInsight-kluster som du skapat med Resource Manager-mall i **kluster-AAD-identitet** fliken och Sök efter värdet för **tjänstens huvudnamn visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="23a88-148">One way to look for the name is to open the HDInsight cluster blade that you created using the Resource Manager template, click the **Cluster AAD Identity** tab, and look for the value of **Service Principal Display Name**.</span></span>
2. <span data-ttu-id="23a88-149">Nu kan ge åtkomst till Azure AD-program på filen/mappen som du vill komma åt från HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="23a88-149">Now, provide access to this Azure AD application on the file/folder that you want to access from the HDInsight cluster.</span></span> <span data-ttu-id="23a88-150">Om du vill ange rätt åtkomstkontrollistor på filen/mappen i Data Lake Store, se [skydda data i Data Lake Store](data-lake-store-secure-data.md#filepermissions).</span><span class="sxs-lookup"><span data-stu-id="23a88-150">To set the right ACLs on the file/folder in Data Lake Store, see [Securing data in Data Lake Store](data-lake-store-secure-data.md#filepermissions).</span></span>

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a><span data-ttu-id="23a88-151">Kör testjobb i HDInsight-klustret för att använda Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="23a88-151">Run test jobs on the HDInsight cluster to use the Data Lake Store</span></span>
<span data-ttu-id="23a88-152">När du har konfigurerat ett HDInsight-kluster, kan du köra testjobb på klustret för att testa att HDInsight-klustret har åtkomst till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23a88-152">After you have configured an HDInsight cluster, you can run test jobs on the cluster to test that the HDInsight cluster can access Data Lake Store.</span></span> <span data-ttu-id="23a88-153">Om du vill göra det, kommer vi kör ett Hive-jobb som skapar en tabell med exempeldata som du tidigare har överförts till din Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23a88-153">To do so, we will run a sample Hive job that creates a table using the sample data that you uploaded earlier to your Data Lake Store.</span></span>

<span data-ttu-id="23a88-154">I det här avsnittet kommer du att SSH till ett kluster i HDInsight Linux och kör den en exempelfråga Hive.</span><span class="sxs-lookup"><span data-stu-id="23a88-154">In this section you will SSH into an HDInsight Linux cluster and run the a sample Hive query.</span></span> <span data-ttu-id="23a88-155">Om du använder en Windows-klient, bör du använda **PuTTY**, som kan hämtas från [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="23a88-155">If you are using a Windows client, we recommend using **PuTTY**, which can be downloaded from [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

<span data-ttu-id="23a88-156">Läs mer om hur du använder PuTTY [använda SSH med Linux-baserade Hadoop i HDInsight från Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="23a88-156">For more information on using PuTTY, see [Use SSH with Linux-based Hadoop on HDInsight from Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>

1. <span data-ttu-id="23a88-157">När du är ansluten, startar du Hive-CLI med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="23a88-157">Once connected, start the Hive CLI by using the following command:</span></span>

   ```
   hive
   ```
2. <span data-ttu-id="23a88-158">Med hjälp av CLI, ange följande instruktioner för att skapa en ny tabell med namnet **fordon** med exempeldata i Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="23a88-158">Using the CLI, enter the following statements to create a new table named **vehicles** by using the sample data in the Data Lake Store:</span></span>

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
   SELECT * FROM vehicles LIMIT 10;
   ```

   <span data-ttu-id="23a88-159">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="23a88-159">You should see an output similar to the following:</span></span>

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


## <a name="access-data-lake-store-using-hdfs-commands"></a><span data-ttu-id="23a88-160">Åtkomst till Data Lake Store med hjälp av HDFS-kommandon</span><span class="sxs-lookup"><span data-stu-id="23a88-160">Access Data Lake Store using HDFS commands</span></span>
<span data-ttu-id="23a88-161">När du har konfigurerat HDInsight-klustret för att använda Data Lake Store kan använda du HDFS-gränssnittskommandon tillgång till store.</span><span class="sxs-lookup"><span data-stu-id="23a88-161">Once you have configured the HDInsight cluster to use Data Lake Store, you can use the HDFS shell commands to access the store.</span></span>

<span data-ttu-id="23a88-162">I det här avsnittet kommer du att SSH i Linux ett HDInsight-kluster och kör HDFS-kommandon.</span><span class="sxs-lookup"><span data-stu-id="23a88-162">In this section you will SSH into an HDInsight Linux cluster and run the HDFS commands.</span></span> <span data-ttu-id="23a88-163">Om du använder en Windows-klient, bör du använda **PuTTY**, som kan hämtas från [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="23a88-163">If you are using a Windows client, we recommend using **PuTTY**, which can be downloaded from [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

<span data-ttu-id="23a88-164">Läs mer om hur du använder PuTTY [använda SSH med Linux-baserade Hadoop i HDInsight från Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span><span class="sxs-lookup"><span data-stu-id="23a88-164">For more information on using PuTTY, see [Use SSH with Linux-based Hadoop on HDInsight from Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).</span></span>

<span data-ttu-id="23a88-165">När du är ansluten, Använd kommandot filesystem HDFS för att visa filer i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23a88-165">Once connected, use the following HDFS filesystem command to list the files in the Data Lake Store.</span></span>

```
hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/
```

<span data-ttu-id="23a88-166">Du bör nu se filen som du tidigare har överförts till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="23a88-166">This should list the file that you uploaded earlier to the Data Lake Store.</span></span>

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder
```

<span data-ttu-id="23a88-167">Du kan också använda den `hdfs dfs -put` kommando för att överföra filer till Data Lake Store och sedan använda `hdfs dfs -ls` att kontrollera om filerna som har laddats upp.</span><span class="sxs-lookup"><span data-stu-id="23a88-167">You can also use the `hdfs dfs -put` command to upload some files to the Data Lake Store, and then use `hdfs dfs -ls` to verify whether the files were successfully uploaded.</span></span>


## <a name="next-steps"></a><span data-ttu-id="23a88-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="23a88-168">Next steps</span></span>
* [<span data-ttu-id="23a88-169">Kopiera data från Azure Storage-Blobbar till Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="23a88-169">Copy data from Azure Storage Blobs to Data Lake Store</span></span>](data-lake-store-copy-data-wasb-distcp.md)
