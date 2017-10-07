---
title: "aaaMigrate tooAzure Resource Manager-verktyg för HDInsight | Microsoft Docs"
description: "Hur toomigrate tooAzure Resource Manager development tools för HDInsight-kluster"
services: hdinsight
editor: cgronlun
manager: jhubbard
author: nitinme
documentationcenter: 
ms.assetid: 05efedb5-6456-4552-87ff-156d77fbe2e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: c087ae63d2544e5badae6be9c258f783aa92e2ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-tooazure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>Migrera tooAzure Resource Manager-baserade utvecklingsverktyg för HDInsight-kluster

HDInsight sluta Azure Service Manager ASM-baserat verktyg för HDInsight. Om du har använt Azure PowerShell, Azure CLI eller hello HDInsight .NET SDK toowork med HDInsight-kluster, är du rekommenderar toouse hello Azure Resource Manager ARM-baserade versioner av PowerShell, CLI och .NET SDK framöver. Den här artikeln innehåller tips om hur toomigrate toohello ny ARM-baserade metod. Tillämpliga fall lärt den här artikeln har även dig hello skillnaderna mellan hello ASM och ARM metoder för HDInsight.

> [!IMPORTANT]
> hello-stöd för ASM baserat PowerShell, CLI och .NET SDK ska upphöra på **1 januari 2017**.
> 
> 

## <a name="migrating-azure-cli-tooazure-resource-manager"></a>Migrera Azure CLI tooAzure Resource Manager
hello Azure CLI standardvärden nu tooAzure Resource Manager (ARM) läge, såvida du inte uppgraderar från en tidigare installation. i detta fall kan du behöva toouse hello `azure config mode arm` kommandot tooswitch tooARM läge.

grundläggande hello-kommandon som hello Azure CLI toowork medföljer HDInsight med hjälp av Azure Service Management (ASM) är samma hello när du använder ARM; men vissa parametrar och växlar kan ha nya namn och det finns många nya parametrar när du med hjälp av ARM. Exempelvis kan du kan nu använda `azure hdinsight cluster create` toospecify hello Azure-nätverk som ska skapas i ett kluster eller Hive och Oozie metastore information.

Grundläggande kommandon för att arbeta med HDInsight via Azure Resource Manager är:

* `azure hdinsight cluster create`-skapar ett nytt HDInsight-kluster
* `azure hdinsight cluster delete`-tar bort ett befintligt HDInsight-kluster
* `azure hdinsight cluster show`– Visa information om ett befintligt kluster
* `azure hdinsight cluster list`– Visar en lista över HDInsight-kluster för din Azure-prenumeration

Använd hello `-h` växla tooinspect hello parametrar och växlar som är tillgängliga för varje kommando.

### <a name="new-commands"></a>Nya kommandon
Nya kommandon som är tillgängliga med Azure Resource Manager är:

* `azure hdinsight cluster resize`-dynamiskt ändringar hello antalet arbetarnoder i klustret hello
* `azure hdinsight cluster enable-http-access`-aktiverar HTTPs åtkomst toohello kluster (på som standard)
* `azure hdinsight cluster disable-http-access`-inaktiverar HTTPs åtkomst toohello kluster
* `azure hdinsight script-action`-innehåller kommandon för att skapa/hantera skriptåtgärder på ett kluster
* `azure hdinsight config`-innehåller kommandon för att skapa en konfigurationsfil som kan användas med hello `hdinsight cluster create` kommandot tooprovide konfigurationsinformation.

### <a name="deprecated-commands"></a>Föråldrade kommandon
Om du använder hello `azure hdinsight job` kommandon toosubmit jobb tooyour HDInsight-kluster dessa inte är tillgängliga via hello ARM-kommandon. Om du behöver tooprogrammatically skicka jobb tooHDInsight från skript, bör du använda hello REST API: er som tillhandahålls av HDInsight. Mer information om att skicka jobb med hjälp av REST API: er finns i hello följande dokument.

* [Kör jobb för MapReduce med Hadoop i HDInsight med cURL](hdinsight-hadoop-use-mapreduce-curl.md)
* [Köra Hive-frågor med Hadoop i HDInsight med cURL](hdinsight-hadoop-use-hive-curl.md)
* [Köra Pig med Hadoop på HDInsight med cURL](hdinsight-hadoop-use-pig-curl.md)

Information om andra sätt toorun MapReduce Hive, och svin interaktivt, se [använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md), [använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md), och [använda Pig med Hadoop på HDInsight](hdinsight-use-pig.md).

### <a name="examples"></a>Exempel
**Skapar ett kluster**

* Gamla kommando (ASM)-`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Nytt kommando (ARM)-`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

**Tar bort ett kluster**

* Gamla kommando (ASM)-`azure hdinsight cluster delete myhdicluster`
* Nytt kommando (ARM)-`azure hdinsight cluster delete mycluster -g myresourcegroup`

**Lista över kluster**

* Gamla kommando (ASM)-`azure hdinsight cluster list`
* Nytt kommando (ARM)-`azure hdinsight cluster list`

> [!NOTE]
> För hello listan kommando anger hello resursen med `-g` returnerar hello-kluster i hello angivna resursgruppen.
> 
> 

**Visa information om klustret**

* Gamla kommando (ASM)-`azure hdinsight cluster show myhdicluster`
* Nytt kommando (ARM)-`azure hdinsight cluster show myhdicluster -g myresourcegroup`

## <a name="migrating-azure-powershell-tooazure-resource-manager"></a>Migrera Azure PowerShell tooAzure Resource Manager
hello allmän information om Azure PowerShell i hello Azure Resource Manager (ARM) läge finns på [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).

hello Azure PowerShell ARM-cmdlets kan vara installerade sida-vid-sida med hello ASM-cmdlets. hello-cmdlets från hello två lägen kan särskiljas genom deras namn.  hello ARM-läge har *AzureRmHDInsight* i hello cmdlet-namnen för att jämföra*AzureHDInsight* i hello ASM-läge.  Till exempel *ny AzureRmHDInsightCluster* vs. *Nya AzureHDInsightCluster*. Parametrar och växlar som kan ha nyheter namn och det finns många nya parametrar när du med hjälp av ARM.  Till exempel flera cmdlets kräver en ny växel kallas *- ResourceGroupName*. 

Innan du kan använda hello HDInsight cmdlets, måste du ansluta tooyour Azure-konto och skapa en ny resursgrupp:

* Login-AzureRmAccount eller [Välj AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx). Se [autentiserar ett huvudnamn för tjänsten med Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Ny AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a>Har bytt namn till cmdlet: ar
toolist hello HDInsight ASM-cmdlets i Windows PowerShell-konsol:

    help *azurermhdinsight*

hello följande tabell visas hello ASM-cmdlets och deras namn i hello ARM-läge:

| ASM-cmdlets | ARM-cmdlets |
| --- | --- |
| Add-AzureHDInsightConfigValues |[Lägg till AzureRmHDInsightConfigValues](https://msdn.microsoft.com/library/mt603530.aspx) |
| Add-AzureHDInsightMetastore |[Lägg till AzureRmHDInsightMetastore](https://msdn.microsoft.com/library/mt603670.aspx) |
| Add-AzureHDInsightScriptAction |[Lägg till AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) |
| Add-AzureHDInsightStorage |[Lägg till AzureRmHDInsightStorage](https://msdn.microsoft.com/library/mt619445.aspx) |
| Get-AzureHDInsightCluster |[Get-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619371.aspx) |
| Get-AzureHDInsightJob |[Get-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603590.aspx) |
| Get-AzureHDInsightJobOutput |[Get-AzureRmHDInsightJobOutput](https://msdn.microsoft.com/library/mt603793.aspx) |
| Get-AzureHDInsightProperties |[Get-AzureRmHDInsightProperties](https://msdn.microsoft.com/library/mt603546.aspx) |
| Grant-AzureHDInsightHttpServicesAccess |[Bevilja AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619407.aspx) |
| Grant-AzureHdinsightRdpAccess |[Bevilja AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603717.aspx) |
| Invoke-AzureHDInsightHiveJob |[Anropa AzureRmHDInsightHiveJob](https://msdn.microsoft.com/library/mt603593.aspx) |
| New-AzureHDInsightCluster |[Ny AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) |
| New-AzureHDInsightClusterConfig |[Ny AzureRmHDInsightClusterConfig](https://msdn.microsoft.com/library/mt603700.aspx) |
| New-AzureHDInsightHiveJobDefinition |[Ny AzureRmHDInsightHiveJobDefinition](https://msdn.microsoft.com/library/mt619448.aspx) |
| New-AzureHDInsightMapReduceJobDefinition |[Ny AzureRmHDInsightMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| New-AzureHDInsightPigJobDefinition |[Ny AzureRmHDInsightPigJobDefinition](https://msdn.microsoft.com/library/mt603671.aspx) |
| New-AzureHDInsightSqoopJobDefinition |[Ny AzureRmHDInsightSqoopJobDefinition](https://msdn.microsoft.com/library/mt608551.aspx) |
| New-AzureHDInsightStreamingMapReduceJobDefinition |[Ny AzureRmHDInsightStreamingMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| Remove-AzureHDInsightCluster |[Ta bort AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619431.aspx) |
| Revoke-AzureHDInsightHttpServicesAccess |[Återkalla AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619375.aspx) |
| Revoke-AzureHdinsightRdpAccess |[Återkalla AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603523.aspx) |
| Set-AzureHDInsightClusterSize |[Ange AzureRmHDInsightClusterSize](https://msdn.microsoft.com/library/mt603513.aspx) |
| Set-AzureHDInsightDefaultStorage |[Ange AzureRmHDInsightDefaultStorage](https://msdn.microsoft.com/library/mt603486.aspx) |
| Start-AzureHDInsightJob |[Start-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603798.aspx) |
| Stop-AzureHDInsightJob |[Stoppa AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt619424.aspx) |
| Use-AzureHDInsightCluster |[Använd AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619442.aspx) |
| Wait-AzureHDInsightJob |[Vänta AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603834.aspx) |

### <a name="new-cmdlets"></a>Nya cmdletar
hello följande är nya hello-cmdlets som endast är tillgängliga i hello ARM-läge. 

**Skriptåtgärder relaterade cmdlets:**

* **Get-AzureRmHDInsightPersistedScriptAction**: hämtar hello beständiga skriptåtgärder för ett kluster och visar dem i kronologisk ordning eller hämtar information för en angiven beständiga skriptåtgärder. 
* **Get-AzureRmHDInsightScriptActionHistory**: hämtar hello skriptåtgärdshistoriken för ett kluster och visas i omvänd kronologisk ordning eller hämtar information om en tidigare skriptåtgärder. 
* **Ta bort AzureRmHDInsightPersistedScriptAction**: tar bort en beständiga skriptåtgärder från ett HDInsight-kluster.
* **Ange AzureRmHDInsightPersistedScriptAction**: Anger en tidigare körda skript åtgärd toobe beständiga skriptåtgärder.
* **Skicka AzureRmHDInsightScriptAction**: skickar ett nytt skript åtgärd tooan Azure HDInsight-kluster. 

Av ytterligare användningsinformation finns i [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).

**Clsuter identitet relaterade cmdlets:**

* **Lägg till AzureRmHDInsightClusterIdentity**: lägger till ett konfigurationsobjekt för klustret identitet tooa klustret så att hello HDInsight-kluster kan komma åt Azure Data Lake butiker. Se [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

### <a name="examples"></a>Exempel
**Skapa kluster**

Gamla kommando (ASM): 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

Nytt kommando (ARM):

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials


**Ta bort klustret**

Gamla kommando (ASM):

    Remove-AzureHDInsightCluster -name $clusterName 

Nytt kommando (ARM):

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

**Lista över kluster**

Gamla kommando (ASM):

    Get-AzureHDInsightCluster

Nytt kommando (ARM):

    Get-AzureRmHDInsightCluster 

**Visa kluster**

Gamla kommando (ASM):

    Get-AzureHDInsightCluster -Name $clusterName

Nytt kommando (ARM):

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a>Andra exempel
* [Skapa HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Skicka Hive-jobb](hdinsight-hadoop-use-hive-powershell.md)
* [Skicka Pig-jobb](hdinsight-hadoop-use-pig-powershell.md)
* [Skicka Sqoop jobb](hdinsight-hadoop-use-sqoop-powershell.md)

## <a name="migrating-toohello-arm-based-hdinsight-net-sdk"></a>Migrera toohello ARM-baserade HDInsight .NET SDK
hello Azure Service Management-baserade [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) är nu föråldrad. Du är rekommenderar toouse hello Azure Resource Manager-baserade [(ARM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx). hello används följande ASM-baserat HDInsight-paket inte.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

Det här avsnittet innehåller pekare toomore information om hur tooperform vissa aktiviteter med hjälp av hello ARM-baserade SDK.

| Så här... med hello ARM-baserade HDInsight SDK | Länkar |
| --- | --- |
| Skapa HDInsight-kluster med .NET SDK |Se [skapa HDInsight-kluster med hjälp av .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |
| Anpassa ett kluster med skriptåtgärder med .NET SDK |Se [anpassa HDInsight Linux-kluster med skriptåtgärder](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action) |
| Autentisera program interaktivt genom att använda Azure Active Directory med .NET SDK |Se [köra Hive-frågor med .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md). hello kodstycket i den här artikeln använder hello interaktiv autentisering metod. |
| Autentisera program interaktivt genom att använda Azure Active Directory med .NET SDK |Se [skapa icke-interaktiva program för HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| Skicka ett Hive-jobb med hjälp av .NET SDK |Se [skicka Hive-jobb](hdinsight-hadoop-use-hive-dotnet-sdk.md) |
| Skicka Pig-jobbet med .NET SDK |Se [skicka Pig-jobb](hdinsight-hadoop-use-pig-dotnet-sdk.md) |
| Skicka ett Sqoop-jobb med hjälp av .NET SDK |Se [skicka Sqoop jobb](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |
| Visa en lista med HDInsight-kluster med .NET SDK |Se [listan HDInsight-kluster](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| Skala HDInsight-kluster med .NET SDK |Se [skala HDInsight-kluster](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| Bevilja/återkalla åtkomst tooHDInsight kluster med hjälp av .NET SDK |Se [bevilja/återkalla åtkomst tooHDInsight kluster](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| Uppdatera http-autentiseringsuppgifterna för HDInsight-kluster med .NET SDK |Se [uppdatering HTTP användarautentiseringsuppgifter för HDInsight-kluster](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| Hitta hello standardkontot för lagring för HDInsight-kluster med .NET SDK |Se [hitta hello standardkontot för lagring för HDInsight-kluster](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| Ta bort HDInsight-kluster med .NET SDK |Se [ta bort HDInsight-kluster](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Exempel
Nedan följer några exempel på hur en åtgärd är utförs med hjälp av hello ASM-baserade SDK och hello motsvarande kodstycke för hello ARM-baserade SDK.

**Skapar ett kluster CRUD-klient**

* Gamla kommando (ASM)
  
        //Certificate auth
        //This logs hello application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* Nytt kommando (ARM) (Service principal auktorisering)
  
        //Service principal auth
        //This will log hello application in as itself, rather than on behalf of a specific user.
        //For details, including how tooset up hello application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* Nytt kommando (ARM) (auktoriseringen av användare)
  
        //User auth
        //This will log hello application in on behalf of hello user.
        //hello end-user will see a login popup.
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);

**Skapar ett kluster**

* Gamla kommando (ASM)
  
        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);
* Nytt kommando (ARM)
  
        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Linux,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);

**Aktivera HTTP-åtkomst**

* Gamla kommando (ASM)
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* Nytt kommando (ARM)
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

**Tar bort ett kluster**

* Gamla kommando (ASM)
  
        client.DeleteCluster(dnsName);
* Nytt kommando (ARM)
  
        client.Clusters.Delete(resourceGroup, dnsname);

