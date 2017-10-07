---
title: "aaaConnect tooKafka med virtuella nätverk - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toodirectly ansluter tooKafka på HDInsight via ett Azure Virtual Network. Lär dig hur tooconnect tooKafka från utveckling klienter med en VPN-gateway eller från klienter i din lokala nätverk med hjälp av en VPN-gateway-enhet."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.custom: hdinsightactive
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/01/2017
ms.author: larryfr
ms.openlocfilehash: 03542fe14b9a1d010dffa22a8f8d96b098a1576e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tookafka-on-hdinsight-preview-through-an-azure-virtual-network"></a>Ansluta tooKafka på HDInsight (förhandsgranskning) via ett virtuellt Azure-nätverk

Lär dig hur toodirectly ansluter tooKafka på HDInsight med virtuella Azure-nätverk. Det här dokumentet innehåller information om hur du ansluter tooKafka med hello följande konfigurationer:

* Från resurser i ett lokalt nätverk. Den här anslutningen har upprättats med hjälp av en VPN-enhet (programvara eller maskinvara) på nätverket.
* Med hjälp av en VPN-programvaruklient från en utvecklingsmiljö.

## <a name="architecture-and-planning"></a>Arkitektur och planering

HDInsight tillåter inte direktanslutning tooKafka över hello offentliga internet. Använd i stället Kafka klienter (producenter och konsumenter) måste en av följande metoder för att ansluta hello:

* Kör hello-klienten i hello samma virtuella nätverk som Kafka på HDInsight. Den här konfigurationen används i hello [börja med Apache Kafka (förhandsversion) på HDInsight](hdinsight-apache-kafka-get-started.md) dokumentet. hello klienten körs direkt på hello HDInsight klusternoder eller hello på en annan virtuell dator i samma nätverk.

* Ansluta ett privat nätverk, till exempel ditt lokala nätverk, toohello virtuellt nätverk. Den här konfigurationen kan klienter i ditt lokala nätverk toodirectly arbete med Kafka. tooenable den här konfigurationen kan utföra hello följande uppgifter:

    1. Skapa ett virtuellt nätverk.
    2. Skapa en VPN-gateway som använder en plats-till-plats-konfiguration. hello-konfiguration som används i det här dokumentet ansluter tooa VPN-gateway-enhet i ditt lokala nätverk.
    3. Skapa en DNS-server i hello virtuellt nätverk.
    4. Konfigurera vidarebefordran mellan hello DNS-server i varje nätverk.
    5. Installera Kafka på HDInsight i hello virtuellt nätverk.

    Mer information finns i hello [ansluta tooKafka från ett lokalt nätverk](#on-premises) avsnitt. 

* Ansluta enskilda datorer toohello virtuellt nätverk med en VPN-gateway och VPN-klienten. tooenable den här konfigurationen kan utföra hello följande uppgifter:

    1. Skapa ett virtuellt nätverk.
    2. Skapa en VPN-gateway som använder en punkt-till-plats-konfiguration. Den här konfigurationen tillhandahåller en VPN-klient som kan installeras på Windows-klienter.
    3. Installera Kafka på HDInsight i hello virtuellt nätverk.
    4. Konfigurera Kafka för IP-annonser. Den här konfigurationen kan hello klienten tooconnect med IP-adresser i stället för domännamn.
    5. Hämta och använda hello VPN-klienten på hello utvecklingssystemet.

    Mer information finns i hello [ansluta tooKafka med en VPN-klient](#vpnclient) avsnitt.

    > [!WARNING]
    > Den här konfigurationen rekommenderas endast för utveckling på grund av hello följande begränsningar:
    >
    > * Varje klient måste ansluta med en VPN-programvaruklient. Azure tillhandahåller endast en Windows-baserad klient.
    > * hello-klienten klarar inte name resolution begäranden toohello virtuellt nätverk, så du måste använda IP-adressering toocommunicate med Kafka. IP-kommunikation kräver ytterligare konfiguration på hello Kafka klustret.

Mer information om hur du använder HDInsight i ett virtuellt nätverk finns [utöka HDInsight med hjälp av Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).

## <a id="on-premises"></a>Ansluta tooKafka från ett lokalt nätverk

toocreate ett Kafka kluster som kommunicerar med ditt lokala nätverk, gör hello i hello [ansluta HDInsight tooyour lokalt nätverk](./connect-on-premises-network.md) dokumentet.

> [!IMPORTANT]
> När du skapar hello HDInsight-kluster väljer hello __Kafka__ kluster typen.

Dessa steg skapar hello följande konfiguration:

* Azure Virtual Network
* Plats-till-plats VPN-gateway
* Azure Storage-konto (används av HDInsight)
* Kafka på HDInsight

tooverify att en Kafka-klient kan ansluta toohello kluster från lokal användning hello stegen i hello [exempel: Python klienten](#python-client) avsnitt.

## <a id="vpnclient"></a>Anslut tooKafka med en VPN-klient

Använd hello steg i det här avsnittet toocreate hello följande konfiguration:

* Azure Virtual Network
* Punkt-till-plats VPN-gateway
* Azure Storage-konto (används av HDInsight)
* Kafka på HDInsight

1. Åtgärderna i hello hello [arbeta med självsignerade certifikat för plats-till-platsanslutningar](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) dokumentet. Det här dokumentet skapar hello-certifikat som krävs för hello gateway.

2. Öppna ett PowerShell-Kommandotolken och använda hello följande kod toolog i tooyour Azure-prenumeration:

    ```powershell
    Add-AzureRmAccount
    # If you have multiple subscriptions, uncomment tooset hello subscription
    #Select-AzureRmSubscription -SubscriptionName "name of your subscription"
    ```

3. Använd hello följande kod toocreate variabler som innehåller information om konfiguration:

    ```powershell
    # Prompt for generic information
    $resourceGroupName = Read-Host "What is hello resource group name?"
    $baseName = Read-Host "What is hello base name? It is used toocreate names for resources, such as 'net-basename' and 'kafka-basename':"
    $location = Read-Host "What Azure Region do you want toocreate hello resources in?"
    $rootCert = Read-Host "What is hello file path toohello root certificate? It is used toosecure hello VPN gateway."

    # Prompt for HDInsight credentials
    $adminCreds = Get-Credential -Message "Enter hello HTTPS user name and password for hello HDInsight cluster" -UserName "admin"
    $sshCreds = Get-Credential -Message "Enter hello SSH user name and password for hello HDInsight cluster" -UserName "sshuser"

    # Names for Azure resources
    $networkName = "net-$baseName"
    $clusterName = "kafka-$baseName"
    $storageName = "store$baseName" # Can't use dashes in storage names
    $defaultContainerName = $clusterName
    $defaultSubnetName = "default"
    $gatewaySubnetName = "GatewaySubnet"
    $gatewayPublicIpName = "GatewayIp"
    $gatewayIpConfigName = "GatewayConfig"
    $vpnRootCertName = "rootcert"
    $vpnName = "VPNGateway"

    # Network settings
    $networkAddressPrefix = "10.0.0.0/16"
    $defaultSubnetPrefix = "10.0.0.0/24"
    $gatewaySubnetPrefix = "10.0.1.0/24"
    $vpnClientAddressPool = "172.16.201.0/24"

    # HDInsight settings
    $HdiWorkerNodes = 4
    $hdiVersion = "3.5"
    $hdiType = "Kafka"
    ```

4. Använd hello följande kod toocreate hello Azure-resursgrupp och virtuella nätverk:

    ```powershell
    # Create hello resource group that contains everything
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create hello subnet configuration
    $defaultSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -AddressPrefix $defaultSubnetPrefix
    $gatewaySubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -AddressPrefix $gatewaySubnetPrefix

    # Create hello subnet
    New-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AddressPrefix $networkAddressPrefix `
        -Subnet $defaultSubnetConfig, $gatewaySubnetConfig

    # Get hello network & subnet that were created
    $network = Get-AzureRmVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName
    $gatewaySubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -VirtualNetwork $network
    $defaultSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -VirtualNetwork $network

    # Set a dynamic public IP address for hello gateway subnet
    $gatewayPublicIp = New-AzureRmPublicIpAddress -Name $gatewayPublicIpName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AllocationMethod Dynamic
    $gatewayIpConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $gatewayIpConfigName `
        -Subnet $gatewaySubnet `
        -PublicIpAddress $gatewayPublicIp

    # Get hello certificate info
    # Get hello full path in case a relative path was passed
    $rootCertFile = Get-ChildItem $rootCert
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($rootCertFile)
    $certBase64 = [System.Convert]::ToBase64String($cert.RawData)
    $p2sRootCert = New-AzureRmVpnClientRootCertificate -Name $vpnRootCertName `
        -PublicCertData $certBase64

    # Create hello VPN gateway
    New-AzureRmVirtualNetworkGateway -Name $vpnName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -IpConfigurations $gatewayIpConfig `
        -GatewayType Vpn `
        -VpnType RouteBased `
        -EnableBgp $false `
        -GatewaySku Standard `
        -VpnClientAddressPool $vpnClientAddressPool `
        -VpnClientRootCertificates $p2sRootCert
    ```

    > [!WARNING]
    > Det kan ta flera minuter för den här processen toocomplete.

5. Använd följande kod toocreate hello Azure Storage-konto och blob behållaren hello:

    ```powershell
    # Create hello storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $storageName `
        -Type Standard_GRS `
        -Location $location

    # Get hello storage account keys and create a context
    $defaultStorageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName `
        -Name $storageName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageName `
        -StorageAccountKey $defaultStorageKey

    # Create hello default storage container
    New-AzureStorageContainer -Name $defaultContainerName `
        -Context $storageContext
    ```

6. Använd hello följande kod toocreate hello HDInsight-kluster:

    ```powershell
    # Create hello HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $hdiWorkerNodes `
        -ClusterType $hdiType `
        -OSType Linux `
        -Version $hdiVersion `
        -HttpCredential $adminCreds `
        -SshCredential $sshCreds `
        -DefaultStorageAccountName "$storageName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageKey `
        -DefaultStorageContainer $defaultContainerName `
        -VirtualNetworkId $network.Id `
        -SubnetName $defaultSubnet.Id
    ```

  > [!WARNING]
  > Den här processen tar cirka 20 minuter toocomplete.

8. Använd följande cmdlet tooretrieve hello URL: en för hello Windows VPN-klienten för virtuella nätverk för hello hello:

    ```powershell
    Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName `
        -VirtualNetworkGatewayName $vpnName `
        -ProcessorArchitecture Amd64
    ```

    toodownload hello Windows VPN-klienten, Använd hello returnerade URI i webbläsaren.

### <a name="configure-kafka-for-ip-advertising"></a>Konfigurera Kafka för IP-annonsering

Som standard returnerar Zookeeper hello Kafka mäklare tooclients hello domännamn. Den här konfigurationen fungerar inte med hello VPN-programvaruklienten som den inte kan använda namnmatchning för entiteter i hello virtuellt nätverk. För den här konfigurationen använder hello följande steg tooconfigure Kafka tooadvertise IP-adresser i stället för domännamn:

1. Använd en webbläsare och gå toohttps://CLUSTERNAME.azurehdinsight.net. Ersätt __KLUSTERNAMN__ med hello namnet hello Kafka på HDInsight-kluster.

    När du uppmanas, Använd hello HTTPS-användarnamn och lösenord för hello klustret. Hej Ambari-Webbgränssnittet för hello kluster visas.

2. tooview information om Kafka, Välj __Kafka__ hello listan hello vänster.

    ![Tjänstlista med Kafka markerat](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. tooview Kafka konfiguration, markera __konfigurationerna__ från hello överst i mitten.

    ![Kafka konfigurationerna för länkar](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-config.png)

4. toofind hello __kafka env__ konfiguration, ange `kafka-env` i hello __Filter__ på hello övre högra hörnet.

    ![Kafka konfiguration för kafka env](./media/hdinsight-apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. tooconfigure Kafka tooadvertise IP-adresser, lägga till hello följande text toohello längst ned på hello __kafka env mall__ fält:

    ```
    # Configure Kafka tooadvertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. tooconfigure hello gränssnitt som Kafka lyssnar på, ange `listeners` i hello __Filter__ på hello övre högra hörnet.

7. tooconfigure Kafka toolisten på alla nätverksgränssnitt, ändra hello värdet i hello __lyssnare__ fältet för`PLAINTEXT://0.0.0.0:9092`.

8. toosave hello konfigurationsändringar, använda hello __spara__ knappen. Ange ett textmeddelande som beskriver hello ändringar. Välj __OK__ när hello ändringarna har sparats.

    ![Spara konfiguration](./media/hdinsight-apache-kafka-connect-vpn-gateway/save-button.png)

9. tooprevent fel när du startar om Kafka, använda hello __tjänståtgärder__ och välj __aktivera på underhållsläge__. Välj OK toocomplete den här åtgärden.

    ![Tjänståtgärder med aktivera Underhåll markerat](./media/hdinsight-apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. toorestart Kafka, använda hello __starta om__ och välj __starta om alla berörda__. Bekräfta hello omstart och sedan använda hello __OK__ knappen när hello-åtgärden har slutförts.

    ![Starta om knappen med omstart alla berörda markerad](./media/hdinsight-apache-kafka-connect-vpn-gateway/restart-button.png)

11. Använd hello i underhållsläge toodisable __tjänståtgärder__ och välj __aktivera inaktivera underhållsläge__. Välj **OK** toocomplete den här åtgärden.

### <a name="connect-toohello-vpn-gateway"></a>Ansluta toohello VPN-gateway

tooconnect toohello VPN-gateway från en __Windows-klient__, använda hello __ansluta tooAzure__ avsnitt i hello [konfigurerar en punkt-till-plats-anslutning](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) dokumentet.

## <a id="python-client"></a>Exempel: Python-klient

toovalidate anslutning tooKafka använder hello följande steg toocreate och köra en Python producenten och konsumenten:

1. Använd hello följande metoder tooretrieve hello fullständigt kvalificerade domännamnet (FQDN) och IP-adresser för hello noder i hello Kafka klustret:

    ```powershell
    $resourceGroupName = "hello resource group that contains hello virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    Det här skriptet förutsätter att `$resourceGroupName` är hello namnet på hello Azure-resursgrupp som innehåller hello virtuellt nätverk.

    Spara hello returnerade information för användning i hello nästa steg.

2. Använd hello följande tooinstall hello [kafka python](http://kafka-python.readthedocs.io/) klient:

        pip install kafka-python

3. toosend data tooKafka, Använd hello följande Python-kod:

  ```python
  from kafka import KafkaProducer
  # Replace hello `ip_address` entries with hello IP address of your worker nodes
  # NOTE: you don't need hello full list of worker nodes, just one or two.
  producer = KafkaProducer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'])
  for _ in range(50):
      producer.send('testtopic', b'test message')
  ```

    Ersätt hello `'kafka_broker'` poster med hello-adresser som har returnerats från steg 1 i det här avsnittet:

    * Om du använder en __programvara VPN-klienten__, Ersätt hello `kafka_broker` poster med hello IP-adressen för din arbetsnoderna.

    * Om du har __aktiverat namnmatchning via en anpassad DNS-server__, Ersätt hello `kafka_broker` poster med hello FQDN för hello arbetsnoderna.

    > [!NOTE]
    > Den här koden skickar hello sträng `test message` toohello avsnittet `testtopic`. hello standardkonfigurationen av Kafka på HDInsight är toocreate hello avsnittet om det inte finns.

4. tooretrieve hälsningsmeddelande från Kafka, Använd hello följande Python-kod:

   ```python
   from kafka import KafkaConsumer
   # Replace hello `ip_address` entries with hello IP address of your worker nodes
   # Again, you only need one or two, not hello full list.
   # Note: auto_offset_reset='earliest' resets hello starting offset toohello beginning
   #       of hello topic
   consumer = KafkaConsumer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'],auto_offset_reset='earliest')
   consumer.subscribe(['testtopic'])
   for msg in consumer:
     print (msg)
   ```

    Ersätt hello `'kafka_broker'` poster med hello-adresser som har returnerats från steg 1 i det här avsnittet:

    * Om du använder en __programvara VPN-klienten__, Ersätt hello `kafka_broker` poster med hello IP-adressen för din arbetsnoderna.

    * Om du har __aktiverat namnmatchning via en anpassad DNS-server__, Ersätt hello `kafka_broker` poster med hello FQDN för hello arbetsnoderna.

## <a name="next-steps"></a>Nästa steg

Mer information om hur du använder HDInsight med ett virtuellt nätverk finns hello [utöka Azure HDInsight med hjälp av ett Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentet.

Mer information om hur du skapar ett Azure Virtual Network med punkt-till-plats VPN-gateway finns i hello följande dokument:

* [Konfigurera en punkt-till-plats-anslutning med hello Azure-portalen](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [Konfigurera en punkt-till-plats-anslutning med hjälp av Azure PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

Mer information om hur du arbetar med Kafka på HDInsight finns i hello följande dokument:

* [Kom igång med Kafka på HDInsight](hdinsight-apache-kafka-get-started.md)
* [Använd spegling med Kafka på HDInsight](hdinsight-apache-kafka-mirroring.md)
