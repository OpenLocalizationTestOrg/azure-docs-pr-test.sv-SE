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
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a>Hantera Hadoop-kluster i HDInsight med hjälp av .NET SDK
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Lär dig hur toomanage HDInsight-kluster med [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).

**Förutsättningar**

Du måste ha hello följande innan du börjar den här artikeln:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="connect-tooazure-hdinsight"></a>Ansluta tooAzure HDInsight

Du behöver hello följande Nuget-paket:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight

hello visar följande kodexempel hur tooconnect tooAzure innan du kan administrera HDInsight-kluster under din Azure-prenumeration.

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

En uppmaning visas när du kör programmet.  Om du inte vill toosee hello Kommandotolken, se [skapa icke-interaktiv autentisering .NET HDInsight-program](hdinsight-create-non-interactive-authentication-dotnet-applications.md).

## <a name="create-clusters"></a>Skapa kluster
Se [skapa Linux-baserade kluster i HDInsight med hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)

## <a name="list-clusters"></a>Lista över kluster
hello visas följande kodavsnitt kluster och vissa egenskaper:

    var results = _hdiManagementClient.Clusters.List();
    foreach (var name in results.Clusters) {
        Console.WriteLine("Cluster Name: " + name.Name);
        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
        Console.WriteLine("\t Cluster location: " + name.Location);
        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
    }

## <a name="delete-clusters"></a>Ta bort kluster
Använd följande kodfragment toodelete ett kluster synkront eller asynkront hello: 

    _hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
    _hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");

## <a name="scale-clusters"></a>Skala kluster
hello klustret skalning funktionen kan du toochange hello antalet arbetarnoder som används av ett kluster som körs i Azure HDInsight utan toore-skapa hello-kluster.

> [!NOTE]
> Endast kluster med HDInsight version 3.1.3 eller högre stöds. Du kan kontrollera hello-egenskapssidan om du är osäker på hello version av klustret.  Se [listan och visa](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
> 
> 

hello konsekvenser av att ändra hello antalet datanoder för varje typ av kluster som stöds av HDInsight:

* Hadoop
  
    Sömlöst kan du öka hello antalet arbetarnoder i ett Hadoop-kluster som körs utan att påverka alla väntande eller körs jobb. Nya jobb kan också skicka medan hello-åtgärd pågår. Fel i en åtgärd för skalning hanteras korrekt så att hello klustret alltid lämnas i ett fungerande tillstånd.
  
    När ett Hadoop-kluster skalas genom att minska hello antalet datanoder hello tjänster i hello kluster har startats om. Detta medför att alla körs och väntande jobb toofail på hello hello skalning åtgärden har slutförts. Du kan dock skicka hello jobb när hello åtgärden är klar.
* HBase
  
    Sömlöst kan du lägga till eller ta bort noder tooyour HBase-kluster medan den körs. Regional servrar balanseras automatiskt inom några minuter efter att du avslutat hello skalning igen. Du kan också manuellt balansera hello regionala servrar genom att logga in hello headnode för klustret och köra hello följande kommandon från en kommandotolk:
  
        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* Storm
  
    Sömlöst kan du lägga till eller ta bort data noder tooyour Storm-kluster medan den körs. Men när en slutförd hello skalning åtgärden måste toorebalance hello-topologi.
  
    Ombalansering kan utföras på två sätt:
  
  * Storm webbgränssnittet
  * Verktyget kommandoradsgränssnittet (CLI)
    
    Se toohello [Apache Storm dokumentationen](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) för mer information.
    
    hello Storm webbgränssnittet är tillgänglig på hello HDInsight-kluster:
    
    ![HDInsight Storm skala balansera](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)
    
    Här är ett exempel hur toouse hello CLI kommandot toorebalance hello Storm-topologi:
    
        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

Hej följande fragment visas hur tooresize ett kluster synkront eller asynkront:

    _hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    _hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   


## <a name="grantrevoke-access"></a>Bevilja/återkalla åtkomst
HDInsight-kluster har hello efter http-webbtjänster (alla dessa tjänster har RESTful slutpunkter):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Som standard beviljas dessa tjänster för åtkomst. Du kan återkalla/bevilja hello åtkomst. toorevoke:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = false,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);

toogrant:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = enable,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);


> [!NOTE]
> Genom att bevilja/återkalla hello åtkomst, återställs hello klustret användarnamn och lösenord.
> 
> 

Detta kan även göras via hello Portal. Se [administrera HDInsight med hjälp av hello Azure-portalen][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>Uppdatera autentiseringsuppgifterna för HTTP
Det är hello samma procedur som [HTTP att bevilja/återkalla åtkomst](#grant/revoke-access). Om hello klustret har beviljats hello HTTP-åtkomst, måste du först återkalla den.  Och sedan ge hello åtkomst med nya HTTP-autentiseringsuppgifter.

## <a name="find-hello-default-storage-account"></a>Hitta hello standardkontot för lagring
hello följande kodfragment som visar hur tooget hello standard lagringskontonamn och hello standard lagringskontonyckel för ett kluster.

    var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
    foreach (var key in results.Configuration.Keys)
    {
        Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
    }


## <a name="submit-jobs"></a>Skicka jobb
**toosubmit MapReduce-jobb**

Se [kör Hadoop-MapReduce-exempel i HDInsight](hdinsight-hadoop-run-samples-linux.md).

**toosubmit Hive-jobb** 

Se [köra Hive-frågor med .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).

**toosubmit Pig-jobb**

Se [köra Pig-jobb med hjälp av .NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md).

**toosubmit Sqoop jobb**

Se [använda Sqoop med HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).

**toosubmit Oozie jobb**

Se [Använd Oozie med Hadoop toodefine och kör ett arbetsflöde i HDInsight](hdinsight-use-oozie-linux-mac.md).

## <a name="upload-data-tooazure-blob-storage"></a>Ladda upp data tooAzure Blob storage
Se [överför data tooHDInsight][hdinsight-upload-data].

## <a name="see-also"></a>Se även
* [HDInsight .NET SDK-referensdokumentation](https://msdn.microsoft.com/library/mt271028.aspx)
* [Administrera HDInsight med hjälp av hello Azure-portalen][hdinsight-admin-portal]
* [Administrera HDInsight med ett kommandoradsgränssnitt][hdinsight-admin-cli]
* [Skapa HDInsight-kluster][hdinsight-provision]
* [Ladda upp data tooHDInsight][hdinsight-upload-data]
* [Kom igång med Azure HDInsight][hdinsight-get-started]

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


