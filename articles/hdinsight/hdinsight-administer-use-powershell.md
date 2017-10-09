---
title: aaaManage Hadoop-kluster i HDInsight med PowerShell - Azure | Microsoft Docs
description: "Lär dig hur tooperform administrativa uppgifter för hello Hadoop-kluster i HDInsight med hjälp av Azure PowerShell."
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: bfdfa754-18e5-4ef9-b0d6-2dbdcebc0283
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 3df082d752fa8c703db82a54b82b740290af6729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Hantera Hadoop-kluster i HDInsight med hjälp av Azure PowerShell
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell är en kraftfull skriptmiljö som du kan använda toocontrol och automatisera hello distribution och hantering av dina arbetsbelastningar i Azure. I den här artikeln får du lära dig hur toomanage Hadoop-kluster i Azure HDInsight med hjälp av den lokala Azure PowerShell-konsolen via hello använder Windows PowerShell. Hello lista över hello HDInsight PowerShell-cmdlets, se [cmdlet-referens för HDInsight][hdinsight-powershell-reference].

**Förutsättningar**

Du måste ha hello följande innan du börjar den här artikeln:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="install-azure-powershell"></a>Installera Azure PowerShell
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Om du har installerat Azure PowerShell version 0,9 x, måste du avinstallera den innan du installerar en senare version.

toocheck hello version av hello installerad PowerShell:

    Get-Module *azure*

toouninstall hello äldre version, köra program och funktioner på Kontrollpanelen för hello.

## <a name="create-clusters"></a>Skapa kluster
Se [skapa Linux-baserade kluster i HDInsight med hjälp av Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Lista över kluster
Använd följande kommando toolist hello alla kluster i hello aktuell prenumeration:

    Get-AzureRmHDInsightCluster

## <a name="show-cluster"></a>Visa kluster
Använd följande kommando tooshow information i ett visst kluster i hello aktuell prenumeration hello:

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

## <a name="delete-clusters"></a>Ta bort kluster
Använd följande kommando toodelete ett kluster hello:

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

Du kan också ta bort ett kluster genom att ta bort hello resursgruppen som innehåller hello klustret. Observera detta tar bort alla hello resurser i hello grupp, inklusive hello standardkontot för lagring.

    Remove-AzureRmResourceGroup -Name <Resource Group Name>

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

    Sömlöst kan du lägga till eller ta bort noder tooyour HBase-kluster medan den körs. Regional servrar balanseras automatiskt inom några minuter efter att du avslutat hello skalning igen. Du kan också manuellt balansera hello regionala servrar genom att logga in toohello headnode för klustret och köra hello följande kommandon från en kommandotolk:

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

    ![HDInsight storm skala balansera](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    Här är ett exempel hur toouse hello CLI kommandot toorebalance hello Storm-topologi:

        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

toochange hello Hadoop klusterstorleken med hjälp av Azure PowerShell, kör följande kommando från en klientdator hello:

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>


## <a name="grantrevoke-access"></a>Bevilja/återkalla åtkomst
HDInsight-kluster har hello efter http-webbtjänster (alla dessa tjänster har RESTful slutpunkter):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Som standard beviljas dessa tjänster för åtkomst. Du kan återkalla/bevilja hello åtkomst. toorevoke:

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

toogrant:

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter hello Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter hello HTTP username and password:" -UserName "admin"

    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

> [!NOTE]
> Genom att bevilja/återkalla hello åtkomst, återställs hello klustret användarnamn och lösenord.
>
>

Detta kan även göras via hello Portal. Se [administrera HDInsight med hjälp av hello Azure-portalen][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>Uppdatera autentiseringsuppgifterna för HTTP
Det är hello samma procedur som [HTTP att bevilja/återkalla åtkomst](#grant/revoke-access). Om hello klustret har beviljats hello HTTP-åtkomst, måste du först återkalla den.  Och sedan ge hello åtkomst med nya HTTP-autentiseringsuppgifter.

## <a name="find-hello-default-storage-account"></a>Hitta hello standardkontot för lagring
hello följande Powershell-skript visar hur tooget hello standard lagringskontonamn och hello standard lagringskontonyckel för ett kluster.

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

## <a name="find-hello-resource-group"></a>Hitta hello resursgruppen
Varje HDInsight-klustret tillhör tooan Azure-resursgrupp i hello Resource Manager-läge.  toofind hello resursgrupp:

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


## <a name="submit-jobs"></a>Skicka jobb
**toosubmit MapReduce-jobb**

Se [kör Hadoop-MapReduce-exempel i Windows-baserade HDInsight](hdinsight-run-samples.md).

**toosubmit Hive-jobb**

Se [köra Hive-frågor med hjälp av PowerShell](hdinsight-hadoop-use-hive-powershell.md).

**toosubmit Pig-jobb**

Se [köra Pig-jobb med hjälp av PowerShell](hdinsight-hadoop-use-pig-powershell.md).

**toosubmit Sqoop jobb**

Se [använda Sqoop med HDInsight](hdinsight-use-sqoop.md).

**toosubmit Oozie jobb**

Se [Använd Oozie med Hadoop toodefine och kör ett arbetsflöde i HDInsight](hdinsight-use-oozie.md).

## <a name="upload-data-tooazure-blob-storage"></a>Ladda upp data tooAzure Blob storage
Se [överför data tooHDInsight][hdinsight-upload-data].

## <a name="see-also"></a>Se även
* [Cmdlet-referensdokumentationen för HDInsight][hdinsight-powershell-reference]
* [Administrera HDInsight med hjälp av hello Azure-portalen][hdinsight-admin-portal]
* [Administrera HDInsight med ett kommandoradsgränssnitt][hdinsight-admin-cli]
* [Skapa HDInsight-kluster][hdinsight-provision]
* [Ladda upp data tooHDInsight][hdinsight-upload-data]
* [Skicka Hadoop-jobb via programmering][hdinsight-submit-jobs]
* [Kom igång med Azure HDInsight][hdinsight-get-started]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
