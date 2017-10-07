---
title: "aaaCreate HBase-kluster i ett virtuellt nätverk - Azure | Microsoft Docs"
description: "Komma igång med HBase i Azure HDInsight. Lär dig hur toocreate HDInsight HBase-kluster i Azure Virtual Network."
keywords: 
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 8de8e446-f818-4e61-8fad-e9d38421e80d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 097338a5a650bb607a9f6f9ddb59bb88d098b56f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hbase-clusters-on-hdinsight-in-azure-virtual-network"></a>Skapa HBase-kluster i HDInsight i Azure-nätverk
Lär dig hur toocreate Azure HDInsight HBase-kluster i en [Azure Virtual Network][1].

Med virtuell nätverksintegration HBase-kluster kan vara distribuerade toohello samma virtuella nätverk som dina program så att program kan kommunicera med HBase direkt. hello fördelar:

* Direktanslutning hello web application toohello noder i hello HBase-kluster, vilket möjliggör kommunikation via HBase Java fjärrproceduranrop anropa API: er (RPC).
* Bättre prestanda genom att inte låta trafiken gå över flera gateways och belastningsutjämnare.
* hello möjlighet tooprocess känslig information på ett säkrare sätt utan att exponera en offentlig slutpunkt.

### <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du ha hello följande objekt:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **En arbetsstation med Azure PowerShell**. Se [installera och använda Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/).

## <a name="create-hbase-cluster-into-virtual-network"></a>Skapa HBase-kluster i virtuellt nätverk
I det här avsnittet skapar du en Linux-baserade HBase-kluster med hello beroende Azure Storage-konto i ett virtuellt Azure-nätverk med hjälp av en [Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md). För andra metoder för att skapa kluster och förstå hello inställningar, se [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md). För mer information om hur du använder en mall toocreate Hadoop-kluster i HDInsight, se [skapa Hadoop-kluster i HDInsight med hjälp av Azure Resource Manager-mallar](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [!NOTE]
> Vissa egenskaper är hårdkodat till hello mall. Exempel:
>
> * **Plats**: östra USA 2
> * **Klusterversion**: 3.5
> * **Klustret worker nodsantalet**: 2
> * **Storage-konto som standard**: en unik sträng
> * **Virtuella nätverksnamnet**: &lt;klusternamn >-vnet
> * **Virtuella nätverkets adressutrymme**: 10.0.0.0/16
> * **Undernätnamnet**: Undernät1
> * **Adressintervall för gatewayundernät**: 10.0.0.0/24
>
> &lt;Klusternamn > ersätts med hello klusternamnet som du anger när du använder hello mall.
>
>

1. Klicka på hello följande bild tooopen hello mallen i hello Azure-portalen. hello-mallen finns i [Azure QuickStart mallar](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/).

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-provision-vnet/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. Från hello **anpassad distribution** bladet ange hello följande egenskaper:

   * **Prenumerationen**: Välj ett Azure-prenumeration används toocreate hello HDInsight-kluster, hello beroende lagringskontot och hello virtuella Azure-nätverket.
   * **Resursgruppen**: Välj **Skapa nytt**, och ange namn på en ny resursgrupp.
   * **Plats**: Välj en plats för hello resursgrupp.
   * **Klusternamn**: Ange ett namn för hello Hadoop-kluster toobe skapas.
   * **Klustrets inloggningsnamn och lösenord**: hello standard inloggningsnamnet är **admin**.
   * **SSH-användarnamn och lösenord**: hello Standardanvändarnamnet är **sshuser**.  Du kan byta namn.
   * **Jag accepterar toohello villkor hello ovan**: (Välj)
3. Klicka på **Köp**. Det tar cirka 20 minuter toocreate ett kluster. När hello klustret har skapats måste du klicka på hello klusterbladet i hello portal tooopen den.

När du har slutfört hello kursen kanske du vill toodelete hello klustret. Med HDInsight lagras dina data i Azure Storage så att du på ett säkert sätt kan ta bort ett kluster när det inte används. Du debiteras också för ett HDInsight-kluster, även när det inte används. Eftersom hello avgifterna för hello klustret är flera gånger större än hello avgifterna för lagring, är det ekonomiskt meningsfullt toodelete kluster när de inte används. Hello instruktioner för att ta bort ett kluster finns i [hantera Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen](hdinsight-administer-use-management-portal.md#delete-clusters).

toobegin arbetar med ditt nya kluster med HBase, du kan använda hello procedurer som påträffas i [komma igång med HBase med Hadoop i HDInsight](hdinsight-hbase-tutorial-get-started.md).

## <a name="connect-toohello-hbase-cluster-using-hbase-java-rpc-apis"></a>Ansluta toohello HBase-kluster med HBase Java RPC-API: er
1. Skapa en infrastruktur som en tjänst (IaaS) virtuell dator i hello samma virtuella Azure-nätverket och hello samma undernät. Anvisningar om hur du skapar en ny virtuell IaaS-dator finns [skapa en virtuell dator kör Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md). När följande hello i det här dokumentet, måste du använda följande värden för hello nätverkskonfigurationen hello:

   * **Virtuellt nätverk**: &lt;klusternamn >-vnet
   * **Undernät**: Undernät1

   > [!IMPORTANT]
   > Ersätt &lt;klusternamn > med hello namn du använde när du skapar hello HDInsight-kluster i föregående steg.
   >
   >

   Med dessa värden hello virtuella datorn är placerad i hello samma virtuella nätverk och undernät som hello HDInsight-kluster. Den här konfigurationen låter dem toodirectly kommunicera med varandra. Det finns ett sätt toocreate ett HDInsight-kluster med en tom kantnod. hello kantnod kan vara används toomanage hello.  Mer information finns i [använda tom edge-noder i HDInsight](hdinsight-apps-use-edge-node.md).

2. När du använder en Java-program tooconnect tooHBase via fjärranslutning, måste du använda hello fullständigt kvalificerade domännamnet (FQDN). toodetermine, måste du hämta hello anslutningsspecifika DNS-suffix hello HBase-kluster. toodo att du kan använda någon av följande metoder hello:

   * Använd en Web webbläsare toomake ett Ambari-anrop:

     Bläddra toohttps: / /&lt;klusternamn >.azurehdinsight.net/api/v1/clusters/&lt;klusternamn > / värd? minimal_response = true. Den blir en JSON-fil med hello DNS-suffix.
   * Använda hello Ambari webbplats:

     1. Bläddra för https://&lt;klusternamn >. azurehdinsight.net.
     2. Klicka på **värdar** hello översta menyn.
   * Använd Curl toomake REST-anrop:

    ```bash
        curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest
    ```

     Hitta hello ”värddatornamn” post i hello JavaScript Object Notation (JSON) data returnerades. Den innehåller hello FQDN för hello noder i hello kluster. Exempel:

         ...
         "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
         ...

     hello-delen av hello domän namn som börjar med hello klusternamnet är hello DNS-suffix. Till exempel mycluster.b1.cloudapp.net.
   * Använda Azure PowerShell

     Använd hello följande Azure PowerShell-skriptet tooregister hello **Get-ClusterDetail** funktion, vilket kan vara används tooreturn hello DNS-suffix:

    ```powershell
        function Get-ClusterDetail(
            [String]
            [Parameter( Position=0, Mandatory=$true )]
            $ClusterDnsName,
            [String]
            [Parameter( Position=1, Mandatory=$true )]
            $Username,
            [String]
            [Parameter( Position=2, Mandatory=$true )]
            $Password,
            [String]
            [Parameter( Position=3, Mandatory=$true )]
            $PropertyName
            )
        {
        <#
            .SYNOPSIS
            Displays information toofacilitate an HDInsight cluster-to-cluster scenario within hello same virtual network.
            .Description
            This command shows hello following 4 properties of an HDInsight cluster:
            1. ZookeeperQuorum (supports only HBase type cluster)
                Shows hello value of HBase property "hbase.zookeeper.quorum".
            2. ZookeeperClientPort (supports only HBase type cluster)
                Shows hello value of HBase property "hbase.zookeeper.property.clientPort".
            3. HBaseRestServers (supports only HBase type cluster)
                Shows a list of host FQDNs that run hello HBase REST server.
            4. FQDNSuffix (supports all cluster types)
                Shows hello FQDN suffix of hosts in hello cluster.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
            This command shows hello value of HBase property "hbase.zookeeper.quorum".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
            This command shows hello value of HBase property "hbase.zookeeper.property.clientPort".
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
            This command shows a list of host FQDNs that run hello HBase REST server.
            .EXAMPLE
            Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
            This command shows hello FQDN suffix of hosts in hello cluster.
        #>

            $DnsSuffix = ".azurehdinsight.net"

            $ClusterFQDN = $ClusterDnsName + $DnsSuffix
            $webclient = new-object System.Net.WebClient
            $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

            if($PropertyName -eq "ZookeeperQuorum")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
            }
            if($PropertyName -eq "ZookeeperClientPort")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
            }
            if($PropertyName -eq "HBaseRestServers")
            {
                $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                $Response1 = $webclient.DownloadString($Url1)
                $JsonObject1 = $Response1 | ConvertFrom-Json
                $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                $Response2 = $webclient.DownloadString($Url2)
                $JsonObject2 = $Response2 | ConvertFrom-Json
                foreach ($host_component in $JsonObject2.host_components)
                {
                    $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                    Write-host $ConnectionString
                }
            }
            if($PropertyName -eq "FQDNSuffix")
            {
                $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                $Response = $webclient.DownloadString($Url)
                $JsonObject = $Response | ConvertFrom-Json
                $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                $pos = $FQDN.IndexOf(".")
                $Suffix = $FQDN.Substring($pos + 1)
                Write-host $Suffix
            }
        }
    ```

     Använd hello följande kommando efter körs hello Azure PowerShell-skriptet tooreturn hello DNS-suffix genom att använda hello **Get-ClusterDetail** funktion. Ange klusternamnet i HDInsight HBase, admin namn och lösenord för administratör när du använder det här kommandot.

    ```powershell
        Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>
    ```

     Det här kommandot returnerar hello DNS-suffix. Till exempel **yourclustername.b4.internal.cloudapp.net**.


<!--
3.    Change hello primary DNS suffix configuration of hello virtual machine. This enables hello virtual machine tooautomatically resolve hello host name of hello HBase cluster without explicit specification of hello suffix. For example, hello *workernode0* host name will be correctly resolved tooworkernode0 of hello HBase cluster.

    toomake hello configuration change:

    1. RDP into hello virtual machine.
    2. Open **Local Group Policy Editor**. hello executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** toohello value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot hello virtual machine.
-->

tooverify som hello virtuella datorn kan kommunicera med hello HBase-kluster, kommandot hello `ping headnode0.<dns suffix>` från hello virtuell dator. Till exempel ping headnode0.mycluster.b1.cloudapp.net.

toouse informationen i ett Java-program som du kan följa hello stegen i [använder Maven toobuild Java-program som använder HBase med HDInsight (Hadoop)](hdinsight-hbase-build-java-maven.md) toocreate ett program. toohave hello program ansluta tooa HBase-fjärrservern, ändra hello **hbase-site.xml** Zookeeper i filen i det här exemplet toouse hello FQDN. Exempel:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [!NOTE]
> Mer information om namnmatchning i virtuella Azure-nätverk, inklusive hur toouse egna DNS-servern finns [Name Resolution (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
>
>

## <a name="next-steps"></a>Nästa steg
I kursen får du lära sig hur toocreate ett HBase-kluster. Det finns fler toolearn:

* [Komma igång med HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Använd tom edge-noder i HDInsight](hdinsight-apps-use-edge-node.md)
* [Konfigurera HBase-replikering i HDInsight](hdinsight-hbase-replication.md)
* [Skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [Komma igång med HBase med Hadoop i HDInsight](hdinsight-hbase-tutorial-get-started.md)
* [Analysera Twitter-åsikter med HBase i HDInsight](hdinsight-hbase-analyze-twitter-sentiment.md)
* [Översikt över virtuella nätverk][vnet-overview]

[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: /powershell/azureps-cmdlets-docs


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Etablera information för hello ny HBase-kluster"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Använd skriptåtgärder toocustomize ett HBase-kluster"

[azure-preview-portal]: https://portal.azure.com
