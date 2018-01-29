---
title: "Anpassa HDInsight-kluster med skriptåtgärder - Azure | Microsoft Docs"
description: "Lär dig hur du anpassar HDInsight-kluster med skriptåtgärder."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3a63e216-4163-40c1-aa04-6b42fd0162ad
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: ec95b6d66c71b4278dd1e16807fcc75f5e8b1c36
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Anpassa Windows-baserade HDInsight-kluster med skriptåtgärder
**Script åtgärd** kan användas för att anropa [anpassade skript](hdinsight-hadoop-script-actions.md) när klustret skapas för att installera ytterligare programvara på ett kluster.

Informationen i den här artikeln är specifik för Windows-baserade HDInsight-kluster. Linux-baserade kluster, se [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).

> [!IMPORTANT]
> Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

HDInsight-kluster kan anpassas i en mängd andra sätt, till exempel inklusive ytterligare Azure Storage-konton, ändringar i Hadoop konfigurationsfiler (core-site.xml, hive-site.xml osv.), eller lägga till delade bibliotek (t.ex. Hive, Oozie) till vanliga platser i klustret. Dessa anpassningar kan göras via Azure PowerShell, Azure HDInsight .NET SDK eller Azure-portalen. Mer information finns i [skapa Hadoop-kluster i HDInsight][hdinsight-provision-cluster].

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Skriptåtgärder i klusterskapandeprocessen
Skriptåtgärder används endast när ett kluster som håller på att skapas. Följande diagram illustrerar när skriptet körs under skapandeprocessen:

![HDInsight-kluster anpassning och faserna när klustret skapas][img-hdi-cluster-states]

När skriptet körs klustret försätts den **ClusterCustomization** steget. I det här skedet skriptet körs under kontot system admin parallellt på noderna i klustret, och ger fullständig admin-privilegier på noderna.

> [!NOTE]
> Eftersom du har administratörsrättigheter på klusternoder under den **ClusterCustomization** skede, du kan använda skriptet för att utföra åtgärder som att stoppa och starta tjänster, inklusive Hadoop-relaterade tjänster. Därför som en del av skript, måste du kontrollera att tjänsterna Ambari och andra Hadoop-relaterade tjänster är igång innan körningen för skriptet. De här tjänsterna krävs har kännedom om hälsa och tillstånd för klustret medan det skapas. Om du ändrar någon konfiguration på det kluster som påverkar dessa tjänster måste du använda hjälpfunktioner som tillhandahålls. Läs mer om hjälpfunktioner [utveckla skriptåtgärd skript för HDInsight][hdinsight-write-script].
>
>

Utdata och i felloggarna för skriptet lagras i standardkontot för lagring som du angav för klustret. Loggfilerna lagras i en tabell med namnet **u < \cluster-name-fragment >< \time-stamp > setuplog**. Det här är sammanställda loggar från skriptet köras på alla noder (huvudnod och arbetarnoder) i klustret.

Varje kluster kan acceptera flera skriptåtgärder som anropas i den ordning som de har angetts. Ett skript kan vara kördes på huvudnoden, arbetsnoderna eller båda.

HDInsight tillhandahåller flera skript för att installera följande komponenter i HDInsight-kluster:

| Namn | Skript |
| --- | --- |
| **Installera Spark** |https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Se [installera och använda Spark i HDInsight-kluster][hdinsight-install-spark]. |
| **Installera R** |https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Se [installera och använda R i HDInsight-kluster][hdinsight-install-r]. |
| **Installera Solr** |https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Se [installerar och använder Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md). |
| - **Installera Giraph** |https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Se [installerar och använder Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md). |
| **Läsa in Hive-bibliotek** |https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. Se [lägga till Hive-bibliotek i HDInsight-kluster](hdinsight-hadoop-add-hive-libraries.md) |

## <a name="call-scripts-using-the-azure-portal"></a>Anropa skript med hjälp av Azure portal
**Från Azure-portalen**

1. Skapa ett kluster, enligt beskrivningen i [skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
2. Under valfri konfiguration för den **skriptåtgärder** bladet, klickar du på **lägga till skriptåtgärd** att ge information om skriptåtgärd, enligt nedan:

    ![Använd skriptåtgärder för att anpassa ett kluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Använd skriptåtgärder att anpassa ett kluster")

    <table border='1'>
        <tr><th>Egenskap</th><th>Värde</th></tr>
        <tr><td>Namn</td>
            <td>Ange ett namn för skriptåtgärden.</td></tr>
        <tr><td>Skript-URI</td>
            <td>Ange URI till det skript som anropas för att anpassa klustret. S</td></tr>
        <tr><td>HEAD/Worker</td>
            <td>Ange noderna (**Head** eller **Worker**) som anpassning skriptet körs.</b>.
        <tr><td>Parametrar</td>
            <td>Ange parametrar, om det krävs av skriptet.</td></tr>
    </table>

    Tryck på RETUR för att lägga till fler än en skriptåtgärd för att installera flera komponenter i klustret.
3. Klicka på **Välj** spara skriptet åtgärd konfigurationen och fortsätta med skapa ett kluster.

## <a name="call-scripts-using-azure-powershell"></a>Anropa skript med hjälp av Azure PowerShell
Den här följande PowerShell-skript visar hur du installerar Spark på Windows-baserat HDInsight-kluster.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.

    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"

    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    #############################################################
    # Connect to Azure
    #############################################################

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #############################################################
    # Prepare the dependent components
    #############################################################

    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext

    #############################################################
    # Create cluster with ApacheSpark
    #############################################################

    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey


    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `

    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Om du vill installera andra program, måste du ersätta skriptfilen i skriptet:

När du uppmanas, anger du autentiseringsuppgifterna för klustret. Det kan ta flera minuter innan klustret har skapats.

## <a name="call-scripts-using-net-sdk"></a>Anropa skript med hjälp av .NET SDK
I följande exempel visar hur du installerar Spark på Windows-baserat HDInsight-kluster. Om du vill installera annan programvara som behöver du ersätta skriptfilen i koden.

**Så här skapar du ett HDInsight-kluster med Spark**

1. Skapa ett C#-konsolprogram i Visual Studio.
2. Kör följande kommando från Nuget Package Manager-konsolen.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight
3. Använd följande using-instruktioner i filen Program.cs:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
4. Placera koden i klassen med följande:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId;
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,
                DefaultStorageInfo = new AzureStorageInfo(ExistingStorageName, ExistingStorageKey, ExistingContainer),
                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
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
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
5. Tryck på **F5** för att köra programmet.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Stöd för öppen källkod programvara som används i HDInsight-kluster
Tjänsten Microsoft Azure HDInsight är en flexibel plattform som låter dig skapa stordataprogram i molnet med hjälp av ett ekosystem med öppen källkod tekniker formaterat runt Hadoop. Microsoft Azure tillhandahåller en allmän supportnivå för öppen källkod tekniker som beskrivs i den **stöd för Scope** avsnitt i den <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure Support FAQ webbplats</a>. HDInsight-tjänst ger ytterligare en säkerhetsnivå för stöd för några av komponenterna som beskrivs nedan.

Det finns två typer av komponenter som öppen källkod som är tillgängliga i HDInsight-tjänst:

* **Inbyggda komponenter** -komponenterna är förinstallerat på HDInsight-kluster och tillhandahåller huvudfunktionerna i klustret. Till exempel YARN ResourceManager Hive-frågespråket (HiveQL) och Mahout biblioteket som hör till den här kategorin. En fullständig lista över komponenter i serverkluster finns i [vad är nytt i Hadoop-klusterversioner som tillhandahålls av HDInsight?](hdinsight-component-versioning.md) </a>.
* **Anpassade komponenter** -kan du som användare i klustret, installera eller använda i din arbetsbelastning någon komponent som är tillgängliga i communityn eller skapades av du.

Inbyggda komponenter stöds fullt ut och Microsoft-supporten hjälper att isolera och lösa problem relaterade till komponenterna.

> [!WARNING]
> Komponenter som ingår i HDInsight-kluster stöds fullt ut och Microsoft-supporten hjälper att isolera och lösa problem relaterade till komponenterna.
>
> Anpassade komponenter få kommersiellt rimliga stöd för att hjälpa dig att felsöka problemet ytterligare. Detta kan resultera i att lösa problemet eller där du uppmanas att engagera tillgängliga kanaler för öppen källkod där djup expertis för att teknik finns. Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).
>
>

HDInsight-tjänst finns flera sätt att använda anpassade komponenter. Oavsett hur en komponent används eller installeras i klustret, gäller samma nivå av support. Nedan visas en lista över de vanligaste sätten att anpassade komponenter kan användas på HDInsight-kluster:

1. Jobbet - Hadoop och andra typer av jobb som kör eller använda anpassade komponenter kan skickas till klustret.
2. Anpassning av kluster - när klustret skapas som du kan ange ytterligare inställningar och anpassade komponenter som ska installeras på klusternoderna.
3. Exempel - för populära anpassade komponenter, Microsoft och andra kan ge exempel på hur du kan använda de här komponenterna i HDInsight-kluster. De här exemplen tillhandahålls utan stöd.

## <a name="develop-script-action-scripts"></a>Utveckla skriptåtgärd skript
Se [utveckla skriptåtgärd skript för HDInsight][hdinsight-write-script].

## <a name="see-also"></a>Se även
* [Skapa Hadoop-kluster i HDInsight] [ hdinsight-provision-cluster] innehåller instruktioner om hur du skapar ett HDInsight-kluster med hjälp av andra anpassade alternativ.
* [Utveckla skriptåtgärd skript för HDInsight][hdinsight-write-script]
* [Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]
* [Installera och använda R i HDInsight-kluster][hdinsight-install-r]
* [Installera och använda Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md).
* [Installera och använda Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Steg när klustret skapas"
