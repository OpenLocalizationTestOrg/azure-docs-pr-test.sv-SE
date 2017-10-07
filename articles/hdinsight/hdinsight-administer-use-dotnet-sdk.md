---
title: aaaManage Hadoop-kluster i HDInsight med .NET SDK - Azure | Microsoft Docs
description: "Lär dig hur tooperform administrativa uppgifter för hello Hadoop-kluster i HDInsight med HDInsight .NET SDK."
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: fd134765-c2a0-488a-bca6-184d814d78e9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: d8bbf966b7eba3e943dfb2f764d15d8e52b9be71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a><span data-ttu-id="bd52c-103">Hantera Hadoop-kluster i HDInsight med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="bd52c-103">Manage Hadoop clusters in HDInsight by using .NET SDK</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="bd52c-104">Lär dig hur toomanage HDInsight-kluster med [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd52c-104">Learn how toomanage HDInsight clusters using [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).</span></span>

<span data-ttu-id="bd52c-105">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="bd52c-105">**Prerequisites**</span></span>

<span data-ttu-id="bd52c-106">Du måste ha hello följande innan du börjar den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="bd52c-106">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="bd52c-107">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="bd52c-107">**An Azure subscription**.</span></span> <span data-ttu-id="bd52c-108">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="bd52c-108">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="connect-tooazure-hdinsight"></a><span data-ttu-id="bd52c-109">Ansluta tooAzure HDInsight</span><span class="sxs-lookup"><span data-stu-id="bd52c-109">Connect tooAzure HDInsight</span></span>

<span data-ttu-id="bd52c-110">Du behöver hello följande Nuget-paket:</span><span class="sxs-lookup"><span data-stu-id="bd52c-110">You need hello following Nuget packages:</span></span>

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight

<span data-ttu-id="bd52c-111">hello visar följande kodexempel hur tooconnect tooAzure innan du kan administrera HDInsight-kluster under din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bd52c-111">hello following code sample shows you how tooconnect tooAzure before you can administer HDInsight clusters under your Azure subscription.</span></span>

    using System;
    using Microsoft.Azure;
    using Microsoft.Azure.Management.HDInsight;
    using Microsoft.Azure.Management.HDInsight.Models;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;

    namespace HDInsightManagement
    {
        class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
            // Replace with your AAD tenant ID if necessary
            private const string TenantId = UserTokenProvider.CommonTenantId; 
            private const string SubscriptionId = "<Your Azure Subscription ID>";
            // This is hello GUID for hello PowerShell client. Used for interactive logins in this example.
            private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

            static void Main(string[] args)
            {
                // Authenticate and get a token
                var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
                // Flag subscription for HDInsight, if it isn't already.
                EnableHDInsight(authToken);
                // Get an HDInsight management client
                _hdiManagementClient = new HDInsightManagementClient(authToken);

                // insert code here

                System.Console.WriteLine("Press ENTER toocontinue");
                System.Console.ReadLine();
            }

            /// <summary>
            /// Authenticate tooan Azure subscription and retrieve an authentication token
            /// </summary>
            static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
            {
                var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
                var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                    ClientId, 
                    new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                    PromptBehavior.Always, 
                    UserIdentifier.AnyUser);
                return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
            }
            /// <summary>
            /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
            /// </summary>
            /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
            /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
            /// <param name="authToken">An authentication token for your Azure subscription</param>
            static void EnableHDInsight(TokenCloudCredentials authToken)
            {
                // Create a client for hello Resource manager and set hello subscription ID
                var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
                resourceManagementClient.SubscriptionId = SubscriptionId;
                // Register hello HDInsight provider
                var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
            }
        }
    }

<span data-ttu-id="bd52c-112">En uppmaning visas när du kör programmet.</span><span class="sxs-lookup"><span data-stu-id="bd52c-112">You shall see a prompt when you run this program.</span></span>  <span data-ttu-id="bd52c-113">Om du inte vill toosee hello Kommandotolken, se [skapa icke-interaktiv autentisering .NET HDInsight-program](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span><span class="sxs-lookup"><span data-stu-id="bd52c-113">If you don't want toosee hello prompt, see [Create non-interactive authentication .NET HDInsight applications](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span></span>

## <a name="create-clusters"></a><span data-ttu-id="bd52c-114">Skapa kluster</span><span class="sxs-lookup"><span data-stu-id="bd52c-114">Create clusters</span></span>
<span data-ttu-id="bd52c-115">Se [skapa Linux-baserade kluster i HDInsight med hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="bd52c-115">See [Create Linux-based clusters in HDInsight using hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span>

## <a name="list-clusters"></a><span data-ttu-id="bd52c-116">Lista över kluster</span><span class="sxs-lookup"><span data-stu-id="bd52c-116">List clusters</span></span>
<span data-ttu-id="bd52c-117">hello visas följande kodavsnitt kluster och vissa egenskaper:</span><span class="sxs-lookup"><span data-stu-id="bd52c-117">hello following code snippet lists clusters and some properties:</span></span>

    var results = _hdiManagementClient.Clusters.List();
    foreach (var name in results.Clusters) {
        Console.WriteLine("Cluster Name: " + name.Name);
        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
        Console.WriteLine("\t Cluster location: " + name.Location);
        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
    }

## <a name="delete-clusters"></a><span data-ttu-id="bd52c-118">Ta bort kluster</span><span class="sxs-lookup"><span data-stu-id="bd52c-118">Delete clusters</span></span>
<span data-ttu-id="bd52c-119">Använd följande kodfragment toodelete ett kluster synkront eller asynkront hello:</span><span class="sxs-lookup"><span data-stu-id="bd52c-119">Use hello following code snippet toodelete a cluster synchronously or asynchronously:</span></span> 

    _hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
    _hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");

## <a name="scale-clusters"></a><span data-ttu-id="bd52c-120">Skala kluster</span><span class="sxs-lookup"><span data-stu-id="bd52c-120">Scale clusters</span></span>
<span data-ttu-id="bd52c-121">hello klustret skalning funktionen kan du toochange hello antalet arbetarnoder som används av ett kluster som körs i Azure HDInsight utan toore-skapa hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="bd52c-121">hello cluster scaling feature allows you toochange hello number of worker nodes used by a cluster that is running in Azure HDInsight without having toore-create hello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="bd52c-122">Endast kluster med HDInsight version 3.1.3 eller högre stöds.</span><span class="sxs-lookup"><span data-stu-id="bd52c-122">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="bd52c-123">Du kan kontrollera hello-egenskapssidan om du är osäker på hello version av klustret.</span><span class="sxs-lookup"><span data-stu-id="bd52c-123">If you are unsure of hello version of your cluster, you can check hello Properties page.</span></span>  <span data-ttu-id="bd52c-124">Se [listan och visa](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="bd52c-124">See [List and show clusters](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span></span>
> 
> 

<span data-ttu-id="bd52c-125">hello konsekvenser av att ändra hello antalet datanoder för varje typ av kluster som stöds av HDInsight:</span><span class="sxs-lookup"><span data-stu-id="bd52c-125">hello impact of changing hello number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="bd52c-126">Hadoop</span><span class="sxs-lookup"><span data-stu-id="bd52c-126">Hadoop</span></span>
  
    <span data-ttu-id="bd52c-127">Sömlöst kan du öka hello antalet arbetarnoder i ett Hadoop-kluster som körs utan att påverka alla väntande eller körs jobb.</span><span class="sxs-lookup"><span data-stu-id="bd52c-127">You can seamlessly increase hello number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="bd52c-128">Nya jobb kan också skicka medan hello-åtgärd pågår.</span><span class="sxs-lookup"><span data-stu-id="bd52c-128">New jobs can also be submitted while hello operation is in progress.</span></span> <span data-ttu-id="bd52c-129">Fel i en åtgärd för skalning hanteras korrekt så att hello klustret alltid lämnas i ett fungerande tillstånd.</span><span class="sxs-lookup"><span data-stu-id="bd52c-129">Failures in a scaling operation are gracefully handled so that hello cluster is always left in a functional state.</span></span>
  
    <span data-ttu-id="bd52c-130">När ett Hadoop-kluster skalas genom att minska hello antalet datanoder hello tjänster i hello kluster har startats om.</span><span class="sxs-lookup"><span data-stu-id="bd52c-130">When a Hadoop cluster is scaled down by reducing hello number of data nodes, some of hello services in hello cluster are restarted.</span></span> <span data-ttu-id="bd52c-131">Detta medför att alla körs och väntande jobb toofail på hello hello skalning åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="bd52c-131">This causes all running and pending jobs toofail at hello completion of hello scaling operation.</span></span> <span data-ttu-id="bd52c-132">Du kan dock skicka hello jobb när hello åtgärden är klar.</span><span class="sxs-lookup"><span data-stu-id="bd52c-132">You can, however, resubmit hello jobs once hello operation is complete.</span></span>
* <span data-ttu-id="bd52c-133">HBase</span><span class="sxs-lookup"><span data-stu-id="bd52c-133">HBase</span></span>
  
    <span data-ttu-id="bd52c-134">Sömlöst kan du lägga till eller ta bort noder tooyour HBase-kluster medan den körs.</span><span class="sxs-lookup"><span data-stu-id="bd52c-134">You can seamlessly add or remove nodes tooyour HBase cluster while it is running.</span></span> <span data-ttu-id="bd52c-135">Regional servrar balanseras automatiskt inom några minuter efter att du avslutat hello skalning igen.</span><span class="sxs-lookup"><span data-stu-id="bd52c-135">Regional Servers are automatically balanced within a few minutes of completing hello scaling operation.</span></span> <span data-ttu-id="bd52c-136">Du kan också manuellt balansera hello regionala servrar genom att logga in hello headnode för klustret och köra hello följande kommandon från en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="bd52c-136">However, you can also manually balance hello regional servers by logging into hello headnode of cluster and running hello following commands from a command prompt window:</span></span>
  
        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* <span data-ttu-id="bd52c-137">Storm</span><span class="sxs-lookup"><span data-stu-id="bd52c-137">Storm</span></span>
  
    <span data-ttu-id="bd52c-138">Sömlöst kan du lägga till eller ta bort data noder tooyour Storm-kluster medan den körs.</span><span class="sxs-lookup"><span data-stu-id="bd52c-138">You can seamlessly add or remove data nodes tooyour Storm cluster while it is running.</span></span> <span data-ttu-id="bd52c-139">Men när en slutförd hello skalning åtgärden måste toorebalance hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="bd52c-139">But after a successful completion of hello scaling operation, you will need toorebalance hello topology.</span></span>
  
    <span data-ttu-id="bd52c-140">Ombalansering kan utföras på två sätt:</span><span class="sxs-lookup"><span data-stu-id="bd52c-140">Rebalancing can be accomplished in two ways:</span></span>
  
  * <span data-ttu-id="bd52c-141">Storm webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="bd52c-141">Storm web UI</span></span>
  * <span data-ttu-id="bd52c-142">Verktyget kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="bd52c-142">Command-line interface (CLI) tool</span></span>
    
    <span data-ttu-id="bd52c-143">Se toohello [Apache Storm dokumentationen](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) för mer information.</span><span class="sxs-lookup"><span data-stu-id="bd52c-143">Please refer toohello [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>
    
    <span data-ttu-id="bd52c-144">hello Storm webbgränssnittet är tillgänglig på hello HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="bd52c-144">hello Storm web UI is available on hello HDInsight cluster:</span></span>
    
    ![HDInsight Storm skala balansera](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)
    
    <span data-ttu-id="bd52c-146">Här är ett exempel hur toouse hello CLI kommandot toorebalance hello Storm-topologi:</span><span class="sxs-lookup"><span data-stu-id="bd52c-146">Here is an example how toouse hello CLI command toorebalance hello Storm topology:</span></span>
    
        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="bd52c-147">Hej följande fragment visas hur tooresize ett kluster synkront eller asynkront:</span><span class="sxs-lookup"><span data-stu-id="bd52c-147">hello following code snippet shows how tooresize a cluster synchronously or asynchronously:</span></span>

    _hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    _hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   


## <a name="grantrevoke-access"></a><span data-ttu-id="bd52c-148">Bevilja/återkalla åtkomst</span><span class="sxs-lookup"><span data-stu-id="bd52c-148">Grant/revoke access</span></span>
<span data-ttu-id="bd52c-149">HDInsight-kluster har hello efter http-webbtjänster (alla dessa tjänster har RESTful slutpunkter):</span><span class="sxs-lookup"><span data-stu-id="bd52c-149">HDInsight clusters have hello following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="bd52c-150">ODBC</span><span class="sxs-lookup"><span data-stu-id="bd52c-150">ODBC</span></span>
* <span data-ttu-id="bd52c-151">JDBC</span><span class="sxs-lookup"><span data-stu-id="bd52c-151">JDBC</span></span>
* <span data-ttu-id="bd52c-152">Ambari</span><span class="sxs-lookup"><span data-stu-id="bd52c-152">Ambari</span></span>
* <span data-ttu-id="bd52c-153">Oozie</span><span class="sxs-lookup"><span data-stu-id="bd52c-153">Oozie</span></span>
* <span data-ttu-id="bd52c-154">Templeton</span><span class="sxs-lookup"><span data-stu-id="bd52c-154">Templeton</span></span>

<span data-ttu-id="bd52c-155">Som standard beviljas dessa tjänster för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="bd52c-155">By default, these services are granted for access.</span></span> <span data-ttu-id="bd52c-156">Du kan återkalla/bevilja hello åtkomst.</span><span class="sxs-lookup"><span data-stu-id="bd52c-156">You can revoke/grant hello access.</span></span> <span data-ttu-id="bd52c-157">toorevoke:</span><span class="sxs-lookup"><span data-stu-id="bd52c-157">toorevoke:</span></span>

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = false,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);

<span data-ttu-id="bd52c-158">toogrant:</span><span class="sxs-lookup"><span data-stu-id="bd52c-158">toogrant:</span></span>

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = enable,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);


> [!NOTE]
> <span data-ttu-id="bd52c-159">Genom att bevilja/återkalla hello åtkomst, återställs hello klustret användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="bd52c-159">By granting/revoking hello access, you will reset hello cluster user name and password.</span></span>
> 
> 

<span data-ttu-id="bd52c-160">Detta kan även göras via hello Portal.</span><span class="sxs-lookup"><span data-stu-id="bd52c-160">This can also be done via hello Portal.</span></span> <span data-ttu-id="bd52c-161">Se [administrera HDInsight med hjälp av hello Azure-portalen][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="bd52c-161">See [Administer HDInsight by using hello Azure portal][hdinsight-admin-portal].</span></span>

## <a name="update-http-user-credentials"></a><span data-ttu-id="bd52c-162">Uppdatera autentiseringsuppgifterna för HTTP</span><span class="sxs-lookup"><span data-stu-id="bd52c-162">Update HTTP user credentials</span></span>
<span data-ttu-id="bd52c-163">Det är hello samma procedur som [HTTP att bevilja/återkalla åtkomst](#grant/revoke-access). Om hello klustret har beviljats hello HTTP-åtkomst, måste du först återkalla den.</span><span class="sxs-lookup"><span data-stu-id="bd52c-163">It is hello same procedure as [Grant/revoke HTTP access](#grant/revoke-access).If hello cluster has been granted hello HTTP access, you must first revoke it.</span></span>  <span data-ttu-id="bd52c-164">Och sedan ge hello åtkomst med nya HTTP-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bd52c-164">And then grant hello access with new HTTP user credentials.</span></span>

## <a name="find-hello-default-storage-account"></a><span data-ttu-id="bd52c-165">Hitta hello standardkontot för lagring</span><span class="sxs-lookup"><span data-stu-id="bd52c-165">Find hello default storage account</span></span>
<span data-ttu-id="bd52c-166">hello följande kodfragment som visar hur tooget hello standard lagringskontonamn och hello standard lagringskontonyckel för ett kluster.</span><span class="sxs-lookup"><span data-stu-id="bd52c-166">hello following code snippet demonstrates how tooget hello default storage account name and hello default storage account key for a cluster.</span></span>

    var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
    foreach (var key in results.Configuration.Keys)
    {
        Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
    }


## <a name="submit-jobs"></a><span data-ttu-id="bd52c-167">Skicka jobb</span><span class="sxs-lookup"><span data-stu-id="bd52c-167">Submit jobs</span></span>
<span data-ttu-id="bd52c-168">**toosubmit MapReduce-jobb**</span><span class="sxs-lookup"><span data-stu-id="bd52c-168">**toosubmit MapReduce jobs**</span></span>

<span data-ttu-id="bd52c-169">Se [kör Hadoop-MapReduce-exempel i HDInsight](hdinsight-hadoop-run-samples-linux.md).</span><span class="sxs-lookup"><span data-stu-id="bd52c-169">See [Run Hadoop MapReduce samples in HDInsight](hdinsight-hadoop-run-samples-linux.md).</span></span>

<span data-ttu-id="bd52c-170">**toosubmit Hive-jobb**</span><span class="sxs-lookup"><span data-stu-id="bd52c-170">**toosubmit Hive jobs**</span></span> 

<span data-ttu-id="bd52c-171">Se [köra Hive-frågor med .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="bd52c-171">See [Run Hive queries using .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span>

<span data-ttu-id="bd52c-172">**toosubmit Pig-jobb**</span><span class="sxs-lookup"><span data-stu-id="bd52c-172">**toosubmit Pig jobs**</span></span>

<span data-ttu-id="bd52c-173">Se [köra Pig-jobb med hjälp av .NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="bd52c-173">See [Run Pig jobs using .NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md).</span></span>

<span data-ttu-id="bd52c-174">**toosubmit Sqoop jobb**</span><span class="sxs-lookup"><span data-stu-id="bd52c-174">**toosubmit Sqoop jobs**</span></span>

<span data-ttu-id="bd52c-175">Se [använda Sqoop med HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="bd52c-175">See [Use Sqoop with HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).</span></span>

<span data-ttu-id="bd52c-176">**toosubmit Oozie jobb**</span><span class="sxs-lookup"><span data-stu-id="bd52c-176">**toosubmit Oozie jobs**</span></span>

<span data-ttu-id="bd52c-177">Se [Använd Oozie med Hadoop toodefine och kör ett arbetsflöde i HDInsight](hdinsight-use-oozie-linux-mac.md).</span><span class="sxs-lookup"><span data-stu-id="bd52c-177">See [Use Oozie with Hadoop toodefine and run a workflow in HDInsight](hdinsight-use-oozie-linux-mac.md).</span></span>

## <a name="upload-data-tooazure-blob-storage"></a><span data-ttu-id="bd52c-178">Ladda upp data tooAzure Blob storage</span><span class="sxs-lookup"><span data-stu-id="bd52c-178">Upload data tooAzure Blob storage</span></span>
<span data-ttu-id="bd52c-179">Se [överför data tooHDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="bd52c-179">See [Upload data tooHDInsight][hdinsight-upload-data].</span></span>

## <a name="see-also"></a><span data-ttu-id="bd52c-180">Se även</span><span class="sxs-lookup"><span data-stu-id="bd52c-180">See Also</span></span>
* [<span data-ttu-id="bd52c-181">HDInsight .NET SDK-referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="bd52c-181">HDInsight .NET SDK reference documentation</span></span>](https://msdn.microsoft.com/library/mt271028.aspx)
* <span data-ttu-id="bd52c-182">[Administrera HDInsight med hjälp av hello Azure-portalen][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="bd52c-182">[Administer HDInsight by using hello Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="bd52c-183">[Administrera HDInsight med ett kommandoradsgränssnitt][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="bd52c-183">[Administer HDInsight using a command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="bd52c-184">[Skapa HDInsight-kluster][hdinsight-provision]</span><span class="sxs-lookup"><span data-stu-id="bd52c-184">[Create HDInsight clusters][hdinsight-provision]</span></span>
* <span data-ttu-id="bd52c-185">[Ladda upp data tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="bd52c-185">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="bd52c-186">[Kom igång med Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="bd52c-186">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-portal-linux.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md


