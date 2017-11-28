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
# <a name="connect-tookafka-on-hdinsight-preview-through-an-azure-virtual-network"></a><span data-ttu-id="8b6ac-104">Ansluta tooKafka på HDInsight (förhandsgranskning) via ett virtuellt Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="8b6ac-104">Connect tooKafka on HDInsight (preview) through an Azure Virtual Network</span></span>

<span data-ttu-id="8b6ac-105">Lär dig hur toodirectly ansluter tooKafka på HDInsight med virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-105">Learn how toodirectly connect tooKafka on HDInsight using Azure Virtual Networks.</span></span> <span data-ttu-id="8b6ac-106">Det här dokumentet innehåller information om hur du ansluter tooKafka med hello följande konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-106">This document provides information on connecting tooKafka using hello following configurations:</span></span>

* <span data-ttu-id="8b6ac-107">Från resurser i ett lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-107">From resources in an on-premises network.</span></span> <span data-ttu-id="8b6ac-108">Den här anslutningen har upprättats med hjälp av en VPN-enhet (programvara eller maskinvara) på nätverket.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-108">This connection is established by using a VPN device (software or hardware) on your local network.</span></span>
* <span data-ttu-id="8b6ac-109">Med hjälp av en VPN-programvaruklient från en utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-109">From a development environment using a VPN software client.</span></span>

## <a name="architecture-and-planning"></a><span data-ttu-id="8b6ac-110">Arkitektur och planering</span><span class="sxs-lookup"><span data-stu-id="8b6ac-110">Architecture and planning</span></span>

<span data-ttu-id="8b6ac-111">HDInsight tillåter inte direktanslutning tooKafka över hello offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-111">HDInsight does not allow direct connection tooKafka over hello public internet.</span></span> <span data-ttu-id="8b6ac-112">Använd i stället Kafka klienter (producenter och konsumenter) måste en av följande metoder för att ansluta hello:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-112">Instead, Kafka clients (producers and consumers) must use one of hello following connection methods:</span></span>

* <span data-ttu-id="8b6ac-113">Kör hello-klienten i hello samma virtuella nätverk som Kafka på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-113">Run hello client in hello same virtual network as Kafka on HDInsight.</span></span> <span data-ttu-id="8b6ac-114">Den här konfigurationen används i hello [börja med Apache Kafka (förhandsversion) på HDInsight](hdinsight-apache-kafka-get-started.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-114">This configuration is used in hello [Start with Apache Kafka (preview) on HDInsight](hdinsight-apache-kafka-get-started.md) document.</span></span> <span data-ttu-id="8b6ac-115">hello klienten körs direkt på hello HDInsight klusternoder eller hello på en annan virtuell dator i samma nätverk.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-115">hello client runs directly on hello HDInsight cluster nodes or on another virtual machine in hello same network.</span></span>

* <span data-ttu-id="8b6ac-116">Ansluta ett privat nätverk, till exempel ditt lokala nätverk, toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-116">Connect a private network, such as your on-premises network, toohello virtual network.</span></span> <span data-ttu-id="8b6ac-117">Den här konfigurationen kan klienter i ditt lokala nätverk toodirectly arbete med Kafka.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-117">This configuration allows clients in your on-premises network toodirectly work with Kafka.</span></span> <span data-ttu-id="8b6ac-118">tooenable den här konfigurationen kan utföra hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-118">tooenable this configuration, perform hello following tasks:</span></span>

    1. <span data-ttu-id="8b6ac-119">Skapa ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-119">Create a virtual network.</span></span>
    2. <span data-ttu-id="8b6ac-120">Skapa en VPN-gateway som använder en plats-till-plats-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-120">Create a VPN gateway that uses a site-to-site configuration.</span></span> <span data-ttu-id="8b6ac-121">hello-konfiguration som används i det här dokumentet ansluter tooa VPN-gateway-enhet i ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-121">hello configuration used in this document connects tooa VPN gateway device in your on-premises network.</span></span>
    3. <span data-ttu-id="8b6ac-122">Skapa en DNS-server i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-122">Create a DNS server in hello virtual network.</span></span>
    4. <span data-ttu-id="8b6ac-123">Konfigurera vidarebefordran mellan hello DNS-server i varje nätverk.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-123">Configure forwarding between hello DNS server in each network.</span></span>
    5. <span data-ttu-id="8b6ac-124">Installera Kafka på HDInsight i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-124">Install Kafka on HDInsight into hello virtual network.</span></span>

    <span data-ttu-id="8b6ac-125">Mer information finns i hello [ansluta tooKafka från ett lokalt nätverk](#on-premises) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-125">For more information, see hello [Connect tooKafka from an on-premises network](#on-premises) section.</span></span> 

* <span data-ttu-id="8b6ac-126">Ansluta enskilda datorer toohello virtuellt nätverk med en VPN-gateway och VPN-klienten.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-126">Connect individual machines toohello virtual network using a VPN gateway and VPN client.</span></span> <span data-ttu-id="8b6ac-127">tooenable den här konfigurationen kan utföra hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-127">tooenable this configuration, perform hello following tasks:</span></span>

    1. <span data-ttu-id="8b6ac-128">Skapa ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-128">Create a virtual network.</span></span>
    2. <span data-ttu-id="8b6ac-129">Skapa en VPN-gateway som använder en punkt-till-plats-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-129">Create a VPN gateway that uses a point-to-site configuration.</span></span> <span data-ttu-id="8b6ac-130">Den här konfigurationen tillhandahåller en VPN-klient som kan installeras på Windows-klienter.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-130">This configuration provides a VPN client that can be installed on Windows clients.</span></span>
    3. <span data-ttu-id="8b6ac-131">Installera Kafka på HDInsight i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-131">Install Kafka on HDInsight into hello virtual network.</span></span>
    4. <span data-ttu-id="8b6ac-132">Konfigurera Kafka för IP-annonser.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-132">Configure Kafka for IP advertising.</span></span> <span data-ttu-id="8b6ac-133">Den här konfigurationen kan hello klienten tooconnect med IP-adresser i stället för domännamn.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-133">This configuration allows hello client tooconnect using IP addressing instead of domain names.</span></span>
    5. <span data-ttu-id="8b6ac-134">Hämta och använda hello VPN-klienten på hello utvecklingssystemet.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-134">Download and use hello VPN client on hello development system.</span></span>

    <span data-ttu-id="8b6ac-135">Mer information finns i hello [ansluta tooKafka med en VPN-klient](#vpnclient) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-135">For more information, see hello [Connect tooKafka with a VPN client](#vpnclient) section.</span></span>

    > [!WARNING]
    > <span data-ttu-id="8b6ac-136">Den här konfigurationen rekommenderas endast för utveckling på grund av hello följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-136">This configuration is only recommended for development purposes because of hello following limitations:</span></span>
    >
    > * <span data-ttu-id="8b6ac-137">Varje klient måste ansluta med en VPN-programvaruklient.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-137">Each client must connect using a VPN software client.</span></span> <span data-ttu-id="8b6ac-138">Azure tillhandahåller endast en Windows-baserad klient.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-138">Azure only provides a Windows-based client.</span></span>
    > * <span data-ttu-id="8b6ac-139">hello-klienten klarar inte name resolution begäranden toohello virtuellt nätverk, så du måste använda IP-adressering toocommunicate med Kafka.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-139">hello client does not pass name resolution requests toohello virtual network, so you must use IP addressing toocommunicate with Kafka.</span></span> <span data-ttu-id="8b6ac-140">IP-kommunikation kräver ytterligare konfiguration på hello Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-140">IP communication requires additional configuration on hello Kafka cluster.</span></span>

<span data-ttu-id="8b6ac-141">Mer information om hur du använder HDInsight i ett virtuellt nätverk finns [utöka HDInsight med hjälp av Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="8b6ac-141">For more information on using HDInsight in a virtual network, see [Extend HDInsight by using Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).</span></span>

## <span data-ttu-id="8b6ac-142"><a id="on-premises"></a>Ansluta tooKafka från ett lokalt nätverk</span><span class="sxs-lookup"><span data-stu-id="8b6ac-142"><a id="on-premises"></a> Connect tooKafka from an on-premises network</span></span>

<span data-ttu-id="8b6ac-143">toocreate ett Kafka kluster som kommunicerar med ditt lokala nätverk, gör hello i hello [ansluta HDInsight tooyour lokalt nätverk](./connect-on-premises-network.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-143">toocreate a Kafka cluster that communicates with your on-premises network, follow hello steps in hello [Connect HDInsight tooyour on-premises network](./connect-on-premises-network.md) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8b6ac-144">När du skapar hello HDInsight-kluster väljer hello __Kafka__ kluster typen.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-144">When creating hello HDInsight cluster, select hello __Kafka__ cluster type.</span></span>

<span data-ttu-id="8b6ac-145">Dessa steg skapar hello följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-145">These steps create hello following configuration:</span></span>

* <span data-ttu-id="8b6ac-146">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="8b6ac-146">Azure Virtual Network</span></span>
* <span data-ttu-id="8b6ac-147">Plats-till-plats VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="8b6ac-147">Site-to-site VPN gateway</span></span>
* <span data-ttu-id="8b6ac-148">Azure Storage-konto (används av HDInsight)</span><span class="sxs-lookup"><span data-stu-id="8b6ac-148">Azure Storage account (used by HDInsight)</span></span>
* <span data-ttu-id="8b6ac-149">Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="8b6ac-149">Kafka on HDInsight</span></span>

<span data-ttu-id="8b6ac-150">tooverify att en Kafka-klient kan ansluta toohello kluster från lokal användning hello stegen i hello [exempel: Python klienten](#python-client) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-150">tooverify that a Kafka client can connect toohello cluster from on-premises, use hello steps in hello [Example: Python client](#python-client) section.</span></span>

## <span data-ttu-id="8b6ac-151"><a id="vpnclient"></a>Anslut tooKafka med en VPN-klient</span><span class="sxs-lookup"><span data-stu-id="8b6ac-151"><a id="vpnclient"></a> Connect tooKafka with a VPN client</span></span>

<span data-ttu-id="8b6ac-152">Använd hello steg i det här avsnittet toocreate hello följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-152">Use hello steps in this section toocreate hello following configuration:</span></span>

* <span data-ttu-id="8b6ac-153">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="8b6ac-153">Azure Virtual Network</span></span>
* <span data-ttu-id="8b6ac-154">Punkt-till-plats VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="8b6ac-154">Point-to-site VPN gateway</span></span>
* <span data-ttu-id="8b6ac-155">Azure Storage-konto (används av HDInsight)</span><span class="sxs-lookup"><span data-stu-id="8b6ac-155">Azure Storage Account (used by HDInsight)</span></span>
* <span data-ttu-id="8b6ac-156">Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="8b6ac-156">Kafka on HDInsight</span></span>

1. <span data-ttu-id="8b6ac-157">Åtgärderna i hello hello [arbeta med självsignerade certifikat för plats-till-platsanslutningar](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-157">Follow hello steps in hello [Working with self-signed certificates for Point-to-site connections](../vpn-gateway/vpn-gateway-certificates-point-to-site.md) document.</span></span> <span data-ttu-id="8b6ac-158">Det här dokumentet skapar hello-certifikat som krävs för hello gateway.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-158">This document creates hello certificates needed for hello gateway.</span></span>

2. <span data-ttu-id="8b6ac-159">Öppna ett PowerShell-Kommandotolken och använda hello följande kod toolog i tooyour Azure-prenumeration:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-159">Open a PowerShell prompt and use hello following code toolog in tooyour Azure subscription:</span></span>

    ```powershell
    Add-AzureRmAccount
    # If you have multiple subscriptions, uncomment tooset hello subscription
    #Select-AzureRmSubscription -SubscriptionName "name of your subscription"
    ```

3. <span data-ttu-id="8b6ac-160">Använd hello följande kod toocreate variabler som innehåller information om konfiguration:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-160">Use hello following code toocreate variables that contain configuration information:</span></span>

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

4. <span data-ttu-id="8b6ac-161">Använd hello följande kod toocreate hello Azure-resursgrupp och virtuella nätverk:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-161">Use hello following code toocreate hello Azure resource group and virtual network:</span></span>

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
    > <span data-ttu-id="8b6ac-162">Det kan ta flera minuter för den här processen toocomplete.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-162">It can take several minutes for this process toocomplete.</span></span>

5. <span data-ttu-id="8b6ac-163">Använd följande kod toocreate hello Azure Storage-konto och blob behållaren hello:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-163">Use hello following code toocreate hello Azure Storage Account and blob container:</span></span>

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

6. <span data-ttu-id="8b6ac-164">Använd hello följande kod toocreate hello HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-164">Use hello following code toocreate hello HDInsight cluster:</span></span>

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
  > <span data-ttu-id="8b6ac-165">Den här processen tar cirka 20 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-165">This process takes around 20 minutes toocomplete.</span></span>

8. <span data-ttu-id="8b6ac-166">Använd följande cmdlet tooretrieve hello URL: en för hello Windows VPN-klienten för virtuella nätverk för hello hello:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-166">Use hello following cmdlet tooretrieve hello URL for hello Windows VPN client for hello virtual network:</span></span>

    ```powershell
    Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName `
        -VirtualNetworkGatewayName $vpnName `
        -ProcessorArchitecture Amd64
    ```

    <span data-ttu-id="8b6ac-167">toodownload hello Windows VPN-klienten, Använd hello returnerade URI i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-167">toodownload hello Windows VPN client, use hello returned URI in your web browser.</span></span>

### <a name="configure-kafka-for-ip-advertising"></a><span data-ttu-id="8b6ac-168">Konfigurera Kafka för IP-annonsering</span><span class="sxs-lookup"><span data-stu-id="8b6ac-168">Configure Kafka for IP advertising</span></span>

<span data-ttu-id="8b6ac-169">Som standard returnerar Zookeeper hello Kafka mäklare tooclients hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-169">By default, Zookeeper returns hello domain name of hello Kafka brokers tooclients.</span></span> <span data-ttu-id="8b6ac-170">Den här konfigurationen fungerar inte med hello VPN-programvaruklienten som den inte kan använda namnmatchning för entiteter i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-170">This configuration does not work with hello VPN software client, as it cannot use name resolution for entities in hello virtual network.</span></span> <span data-ttu-id="8b6ac-171">För den här konfigurationen använder hello följande steg tooconfigure Kafka tooadvertise IP-adresser i stället för domännamn:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-171">For this configuration, use hello following steps tooconfigure Kafka tooadvertise IP addresses instead of domain names:</span></span>

1. <span data-ttu-id="8b6ac-172">Använd en webbläsare och gå toohttps://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-172">Using a web browser, go toohttps://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="8b6ac-173">Ersätt __KLUSTERNAMN__ med hello namnet hello Kafka på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-173">Replace __CLUSTERNAME__ with hello name of hello Kafka on HDInsight cluster.</span></span>

    <span data-ttu-id="8b6ac-174">När du uppmanas, Använd hello HTTPS-användarnamn och lösenord för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-174">When prompted, use hello HTTPS user name and password for hello cluster.</span></span> <span data-ttu-id="8b6ac-175">Hej Ambari-Webbgränssnittet för hello kluster visas.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-175">hello Ambari Web UI for hello cluster is displayed.</span></span>

2. <span data-ttu-id="8b6ac-176">tooview information om Kafka, Välj __Kafka__ hello listan hello vänster.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-176">tooview information on Kafka, select __Kafka__ from hello list on hello left.</span></span>

    ![Tjänstlista med Kafka markerat](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. <span data-ttu-id="8b6ac-178">tooview Kafka konfiguration, markera __konfigurationerna__ från hello överst i mitten.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-178">tooview Kafka configuration, select __Configs__ from hello top middle.</span></span>

    ![Kafka konfigurationerna för länkar](./media/hdinsight-apache-kafka-connect-vpn-gateway/select-kafka-config.png)

4. <span data-ttu-id="8b6ac-180">toofind hello __kafka env__ konfiguration, ange `kafka-env` i hello __Filter__ på hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-180">toofind hello __kafka-env__ configuration, enter `kafka-env` in hello __Filter__ field on hello upper right.</span></span>

    ![Kafka konfiguration för kafka env](./media/hdinsight-apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. <span data-ttu-id="8b6ac-182">tooconfigure Kafka tooadvertise IP-adresser, lägga till hello följande text toohello längst ned på hello __kafka env mall__ fält:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-182">tooconfigure Kafka tooadvertise IP addresses, add hello following text toohello bottom of hello __kafka-env-template__ field:</span></span>

    ```
    # Configure Kafka tooadvertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. <span data-ttu-id="8b6ac-183">tooconfigure hello gränssnitt som Kafka lyssnar på, ange `listeners` i hello __Filter__ på hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-183">tooconfigure hello interface that Kafka listens on, enter `listeners` in hello __Filter__ field on hello upper right.</span></span>

7. <span data-ttu-id="8b6ac-184">tooconfigure Kafka toolisten på alla nätverksgränssnitt, ändra hello värdet i hello __lyssnare__ fältet för`PLAINTEXT://0.0.0.0:9092`.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-184">tooconfigure Kafka toolisten on all network interfaces, change hello value in hello __listeners__ field too`PLAINTEXT://0.0.0.0:9092`.</span></span>

8. <span data-ttu-id="8b6ac-185">toosave hello konfigurationsändringar, använda hello __spara__ knappen.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-185">toosave hello configuration changes, use hello __Save__ button.</span></span> <span data-ttu-id="8b6ac-186">Ange ett textmeddelande som beskriver hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-186">Enter a text message describing hello changes.</span></span> <span data-ttu-id="8b6ac-187">Välj __OK__ när hello ändringarna har sparats.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-187">Select __OK__ once hello changes have been saved.</span></span>

    ![Spara konfiguration](./media/hdinsight-apache-kafka-connect-vpn-gateway/save-button.png)

9. <span data-ttu-id="8b6ac-189">tooprevent fel när du startar om Kafka, använda hello __tjänståtgärder__ och välj __aktivera på underhållsläge__.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-189">tooprevent errors when restarting Kafka, use hello __Service Actions__ button and select __Turn On Maintenance Mode__.</span></span> <span data-ttu-id="8b6ac-190">Välj OK toocomplete den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-190">Select OK toocomplete this operation.</span></span>

    ![Tjänståtgärder med aktivera Underhåll markerat](./media/hdinsight-apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. <span data-ttu-id="8b6ac-192">toorestart Kafka, använda hello __starta om__ och välj __starta om alla berörda__.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-192">toorestart Kafka, use hello __Restart__ button and select __Restart All Affected__.</span></span> <span data-ttu-id="8b6ac-193">Bekräfta hello omstart och sedan använda hello __OK__ knappen när hello-åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-193">Confirm hello restart, and then use hello __OK__ button after hello operation has completed.</span></span>

    ![Starta om knappen med omstart alla berörda markerad](./media/hdinsight-apache-kafka-connect-vpn-gateway/restart-button.png)

11. <span data-ttu-id="8b6ac-195">Använd hello i underhållsläge toodisable __tjänståtgärder__ och välj __aktivera inaktivera underhållsläge__.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-195">toodisable maintenance mode, use hello __Service Actions__ button and select __Turn Off Maintenance Mode__.</span></span> <span data-ttu-id="8b6ac-196">Välj **OK** toocomplete den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-196">Select **OK** toocomplete this operation.</span></span>

### <a name="connect-toohello-vpn-gateway"></a><span data-ttu-id="8b6ac-197">Ansluta toohello VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="8b6ac-197">Connect toohello VPN gateway</span></span>

<span data-ttu-id="8b6ac-198">tooconnect toohello VPN-gateway från en __Windows-klient__, använda hello __ansluta tooAzure__ avsnitt i hello [konfigurerar en punkt-till-plats-anslutning](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-198">tooconnect toohello VPN gateway from a __Windows client__, use hello __Connect tooAzure__ section of hello [Configure a Point-to-Site connection](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#clientcertificate) document.</span></span>

## <span data-ttu-id="8b6ac-199"><a id="python-client"></a>Exempel: Python-klient</span><span class="sxs-lookup"><span data-stu-id="8b6ac-199"><a id="python-client"></a> Example: Python client</span></span>

<span data-ttu-id="8b6ac-200">toovalidate anslutning tooKafka använder hello följande steg toocreate och köra en Python producenten och konsumenten:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-200">toovalidate connectivity tooKafka, use hello following steps toocreate and run a Python producer and consumer:</span></span>

1. <span data-ttu-id="8b6ac-201">Använd hello följande metoder tooretrieve hello fullständigt kvalificerade domännamnet (FQDN) och IP-adresser för hello noder i hello Kafka klustret:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-201">Use one of hello following methods tooretrieve hello fully qualified domain name (FQDN) and IP addresses of hello nodes in hello Kafka cluster:</span></span>

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

    <span data-ttu-id="8b6ac-202">Det här skriptet förutsätter att `$resourceGroupName` är hello namnet på hello Azure-resursgrupp som innehåller hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-202">This script assumes that `$resourceGroupName` is hello name of hello Azure resource group that contains hello virtual network.</span></span>

    <span data-ttu-id="8b6ac-203">Spara hello returnerade information för användning i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-203">Save hello returned information for use in hello next steps.</span></span>

2. <span data-ttu-id="8b6ac-204">Använd hello följande tooinstall hello [kafka python](http://kafka-python.readthedocs.io/) klient:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-204">Use hello following tooinstall hello [kafka-python](http://kafka-python.readthedocs.io/) client:</span></span>

        pip install kafka-python

3. <span data-ttu-id="8b6ac-205">toosend data tooKafka, Använd hello följande Python-kod:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-205">toosend data tooKafka, use hello following Python code:</span></span>

  ```python
  from kafka import KafkaProducer
  # Replace hello `ip_address` entries with hello IP address of your worker nodes
  # NOTE: you don't need hello full list of worker nodes, just one or two.
  producer = KafkaProducer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'])
  for _ in range(50):
      producer.send('testtopic', b'test message')
  ```

    <span data-ttu-id="8b6ac-206">Ersätt hello `'kafka_broker'` poster med hello-adresser som har returnerats från steg 1 i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-206">Replace hello `'kafka_broker'` entries with hello addresses returned from step 1 in this section:</span></span>

    * <span data-ttu-id="8b6ac-207">Om du använder en __programvara VPN-klienten__, Ersätt hello `kafka_broker` poster med hello IP-adressen för din arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-207">If you are using a __Software VPN client__, replace hello `kafka_broker` entries with hello IP address of your worker nodes.</span></span>

    * <span data-ttu-id="8b6ac-208">Om du har __aktiverat namnmatchning via en anpassad DNS-server__, Ersätt hello `kafka_broker` poster med hello FQDN för hello arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-208">If you have __enabled name resolution through a custom DNS server__, replace hello `kafka_broker` entries with hello FQDN of hello worker nodes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8b6ac-209">Den här koden skickar hello sträng `test message` toohello avsnittet `testtopic`.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-209">This code sends hello string `test message` toohello topic `testtopic`.</span></span> <span data-ttu-id="8b6ac-210">hello standardkonfigurationen av Kafka på HDInsight är toocreate hello avsnittet om det inte finns.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-210">hello default configuration of Kafka on HDInsight is toocreate hello topic if it does not exist.</span></span>

4. <span data-ttu-id="8b6ac-211">tooretrieve hälsningsmeddelande från Kafka, Använd hello följande Python-kod:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-211">tooretrieve hello messages from Kafka, use hello following Python code:</span></span>

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

    <span data-ttu-id="8b6ac-212">Ersätt hello `'kafka_broker'` poster med hello-adresser som har returnerats från steg 1 i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-212">Replace hello `'kafka_broker'` entries with hello addresses returned from step 1 in this section:</span></span>

    * <span data-ttu-id="8b6ac-213">Om du använder en __programvara VPN-klienten__, Ersätt hello `kafka_broker` poster med hello IP-adressen för din arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-213">If you are using a __Software VPN client__, replace hello `kafka_broker` entries with hello IP address of your worker nodes.</span></span>

    * <span data-ttu-id="8b6ac-214">Om du har __aktiverat namnmatchning via en anpassad DNS-server__, Ersätt hello `kafka_broker` poster med hello FQDN för hello arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-214">If you have __enabled name resolution through a custom DNS server__, replace hello `kafka_broker` entries with hello FQDN of hello worker nodes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8b6ac-215">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8b6ac-215">Next steps</span></span>

<span data-ttu-id="8b6ac-216">Mer information om hur du använder HDInsight med ett virtuellt nätverk finns hello [utöka Azure HDInsight med hjälp av ett Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="8b6ac-216">For more information on using HDInsight with a virtual network, see hello [Extend Azure HDInsight using an Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span>

<span data-ttu-id="8b6ac-217">Mer information om hur du skapar ett Azure Virtual Network med punkt-till-plats VPN-gateway finns i hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-217">For more information on creating an Azure Virtual Network with Point-to-Site VPN gateway, see hello following documents:</span></span>

* [<span data-ttu-id="8b6ac-218">Konfigurera en punkt-till-plats-anslutning med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8b6ac-218">Configure a Point-to-Site connection using hello Azure portal</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [<span data-ttu-id="8b6ac-219">Konfigurera en punkt-till-plats-anslutning med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8b6ac-219">Configure a Point-to-Site connection using Azure PowerShell</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

<span data-ttu-id="8b6ac-220">Mer information om hur du arbetar med Kafka på HDInsight finns i hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="8b6ac-220">For more information on working with Kafka on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="8b6ac-221">Kom igång med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="8b6ac-221">Get started with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="8b6ac-222">Använd spegling med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="8b6ac-222">Use mirroring with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
