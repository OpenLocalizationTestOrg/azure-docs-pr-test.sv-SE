---
title: "aaaExtend HDInsight med ett virtuellt nätverk - Azure | Microsoft Docs"
description: "Lär dig hur toouse Azure Virtual Network tooconnect HDInsight tooother cloud resurser eller resurser i ditt datacenter"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 37b9b600-d7f8-4cb1-a04a-0b3a827c6dcc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: larryfr
ms.openlocfilehash: ba80be4d9f280c6c62fa8acc996ef5f921acdbbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a>Utöka Azure HDInsight med hjälp av ett virtuellt Azure-nätverk

Lär dig hur toouse HDInsight med en [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Med ett Azure Virtual Network kan hello följande scenarier:

* Ansluter tooHDInsight direkt från ett lokalt nätverk.

* Ansluta HDInsight toodata lagras i ett virtuellt Azure-nätverk.

* Direkt åtkomst till Hadoop-tjänster som inte är tillgängliga offentligt över hello internet. Till exempel Kafka API: er eller hello HBase Java API.

> [!WARNING]
> hello informationen i det här dokumentet kräver en förståelse av TCP/IP-nätverk. Om du inte är bekant med TCP/IP-nätverk bör du samarbeta med någon som innan du gör ändringar tooproduction nätverk.

## <a name="planning"></a>Planering

hello följande är hello frågor som du måste svara på när du planerar tooinstall HDInsight i ett virtuellt nätverk:

* Behöver du tooinstall HDInsight till ett befintligt virtuellt nätverk? Eller skapar du ett nytt nätverk?

    Om du använder ett befintligt virtuellt nätverk kan behöva du toomodify hello nätverkskonfigurationen innan du kan installera HDInsight. Mer information finns i hello [lägga till HDInsight tooan befintligt virtuellt nätverk](#existingvnet) avsnitt.

* Vill du tooconnect hello virtuellt nätverk som innehåller HDInsight tooanother virtuellt nätverk eller lokala nätverk?

    tooeasily arbete med resurser över nätverk, kan du behöver toocreate en anpassad DNS och konfigurera vidarebefordran av DNS. Mer information finns i hello [ansluta flera nätverk](#multinet) avsnitt.

* Vill du toorestrict/omdirigering tooHDInsight för inkommande eller utgående trafik?

    HDInsight har obegränsad kommunikation med specifika IP-adresser i hello Azure-datacenter. Det finns också flera portar som får genom brandväggar för klientkommunikation. Mer information finns i hello [styra nätverkstrafiken](#networktraffic) avsnitt.

## <a id="existingvnet"></a>Lägg till HDInsight tooan befintligt virtuellt nätverk

Använd hello steg i det här avsnittet toodiscover hur tooadd en ny HDInsight tooan befintliga Azure Virtual Network.

> [!NOTE]
> Du kan inte lägga till ett befintligt HDInsight-kluster till ett virtuellt nätverk.

1. Du använder ett klassiskt eller Resource Manager-modellen för hello virtuella nätverket?

    HDInsight 3,4 och större kräver ett virtuellt nätverk för hanteraren för filserverresurser. Tidigare versioner av HDInsight krävs ett klassiskt virtuellt nätverk.

    Om det befintliga nätverket är ett klassiskt virtuellt nätverk, måste du skapa ett virtuellt nätverk för Resource Manager och anslut sedan hello två. [Ansluta klassiska Vnet toonew Vnet](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

    När ansluten, kan HDInsight installeras i nätverk med hello Resource Manager interagera med resurser i hello klassiska nätverk.

2. Använder du Tvingad tunneling? Tvingad tunneling är undernätsinställning som tvingar utgående Internet-trafik tooa enheten för granskning och loggning. HDInsight stöder inte Tvingad tunneling. Ta bort Tvingad tunneling innan du installerar HDInsight i ett undernät eller skapa ett nytt undernät för HDInsight.

3. Du använder nätverkssäkerhetsgrupper, användardefinierade vägar och virtuella nätverksenheter toorestrict trafik till eller från hello virtuella nätverket?

    Som en hanterad tjänst kräver HDInsight obegränsad åtkomst tooseveral IP-adresser i hello Azure-datacenter. tooallow kommunikation med IP-adresserna, uppdatera alla befintliga nätverkssäkerhetsgrupper eller användardefinierade vägar.

    HDInsight är värd för flera tjänster som använder olika portar. Inte blockera trafik toothese portar. En lista över portar tooallow genom virtuell installation brandväggar, finns hello [säkerhet](#security) avsnitt.

    toofind befintliga säkerhetskonfiguration Använd hello följande Azure PowerShell eller Azure CLI-kommandon:

    * Nätverkssäkerhetsgrupper

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
        get-azurermnetworksecuritygroup -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
        az network nsg list --resource-group $RESOURCEGROUP
        ```

        Mer information finns i hello [felsöka nätverkssäkerhetsgrupper](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) dokumentet.

        > [!IMPORTANT]
        > Regler för nätverkssäkerhetsgrupper tillämpas i ordning baserat på prioritet för regeln. Inga andra tillämpas efter den trafiken hello första regeln som matchar hello trafik mönster tillämpas. Regler från mest Tillåtande tooleast Tillåtande. Mer information finns i hello [filtrera nätverkstrafik med nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md) dokumentet.

    * Användardefinierade vägar

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
        get-azurermroutetable -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
        az network route-table list --resource-group $RESOURCEGROUP
        ```

        Mer information finns i hello [felsöka vägar](../virtual-network/virtual-network-routes-troubleshoot-portal.md) dokumentet.

4. Skapa ett HDInsight-kluster och välj hello Azure Virtual Network under konfigurationen. Använd hello steg i följande dokument toounderstand hello klusterskapandeprocessen hello:

    * [Skapa HDInsight med hjälp av hello Azure-portalen](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [Create HDInsight using Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) (Skapa HDInsight med hjälp av Azure PowerShell)
    * [Skapa HDInsight med hjälp av Azure CLI 1.0](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [Skapa HDInsight med hjälp av en Azure Resource Manager-mall](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

  > [!IMPORTANT]
  > Lägga till HDInsight är tooa virtuellt nätverk en valfri konfigurationssteg. Vara säker på att tooselect hello virtuellt nätverk när du konfigurerar hello klustret.

## <a id="multinet"></a>Ansluta flera nätverk

hello största utmaningen med en konfiguration med flera är namnmatchning mellan hello nätverk.

Azure tillhandahåller namnmatchning för Azure-tjänster som är installerade i ett virtuellt nätverk. Den här inbyggda namnmatchning kan HDInsight tooconnect toohello följande resurser genom att använda ett fullständigt kvalificerat domännamn (FQDN):

* Alla resurser som är tillgängligt på hello internet. Till exempel microsoft.com, google.com.

* Alla resurser som är i hello samma Azure-nätverk med hjälp av hello __interna DNS-namnet__ hello resurs. När du använder hello standard namnmatchning är hello följande exempel interna DNS-namn tilldelade tooHDInsight arbetsnoderna:

    * wn0 hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net
    * wn2 hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net

    Båda dessa noder kan kommunicera direkt med varandra och andra noder i HDInsight med hjälp av interna DNS-namn.

hello standard namnmatchning har __inte__ Tillåt HDInsight tooresolve hello namnen på de resurser i nätverk som är kopplade toohello virtuella nätverk. Till exempel är det vanliga toojoin ditt lokala nätverk toohello virtuellt nätverk. HDInsight kan inte med endast hello standard namnmatchning kan komma åt resurser i hello lokalt nätverk efter namn. hello motsatt gäller även, resurser i det lokala nätverket inte kan komma åt resurser i hello virtuella nätverket efter namn.

> [!WARNING]
> Du måste skapa hello anpassad DNS-server och konfigurera hello virtuellt nätverk toouse den innan du skapar hello HDInsight-kluster.

tooenable namnmatchning mellan hello virtuella nätverk och nätverksresurser på anslutna nätverk, måste du utföra följande åtgärder hello:

1. Skapa en anpassad DNS-server i hello Azure Virtual Network där du planerar att tooinstall HDInsight.

2. Konfigurera hello virtuellt nätverk toouse hello anpassad DNS-server.

3. Hitta hello Azure tilldelade DNS-suffixet för det virtuella nätverket. Det här värdet är liknande för`0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`. Information om hur du hittar hello DNS-suffix finns hello [exempel: anpassad DNS](#example-dns) avsnitt.

4. Konfigurera vidarebefordran mellan hello DNS-servrar. hello konfigurationen beror på hello typ av nätverk med fjärråtkomst.

    * Om hello fjärrnätverket är ett lokalt nätverk, konfigurera DNS på följande sätt:
        
        * __Anpassad DNS__ (i hello virtuellt nätverk):

            * Vidarebefordra begäranden för hello DNS-suffix hello virtuellt nätverk toohello Azure rekursiv matcharen (168.63.129.16). Azure hanterar begäranden för resurser i hello virtuellt nätverk

            * Vidarebefordra alla andra begäranden toohello lokala DNS-servern. hello lokalt DNS hanterar alla andra namnmatchning, även begäranden om Internetresurser, till exempel Microsoft.com.

        * __Lokal DNS__: vidarebefordrar begäranden för hello virtuellt nätverk DNS-suffix toohello anpassade DNS-server. hello anpassade DNS-servern vidarebefordrar sedan toohello Azure rekursiv matchning.

        Den här konfigurationen vägar begäranden om fullständiga domännamn som innehåller hello DNS-suffix hello-virtuellt nätverk toohello anpassad DNS-server. Alla andra begäranden (även för offentliga internet-adresser) hanteras av hello lokala DNS-servern.

    * Om hello fjärrnätverket är en annan Azure-nätverk kan du konfigurera DNS enligt följande:

        * __Anpassad DNS__ (i varje virtuellt nätverk):

            * Begäranden för hello DNS-suffix hello virtuella nätverk vidarebefordras toohello anpassade DNS-servrar. hello DNS i varje virtuellt nätverk är ansvarig för att lösa resurser inom sitt nätverk.

            * Vidarebefordra alla andra begäranden toohello Azure rekursiv matchning. hello rekursiv lösare är ansvarig för lösning av lokala och internet-resurser.

        hello DNS-server för varje nätverk som vidarebefordrar begäranden toohello andra, baserat på DNS-suffix. Andra begäranden matchas hello Azure rekursiv resolvern.

    Ett exempel på varje konfiguration finns hello [exempel: anpassad DNS](#example-dns) avsnitt.

Mer information finns i hello [namnmatchning för virtuella datorer och Rollinstanser](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) dokumentet.

## <a name="directly-connect-toohadoop-services"></a>Ansluta direkt tooHadoop tjänster

De flesta dokumentation på HDInsight förutsätter att du har åtkomst toohello klustret via hello internet. Till exempel att du kan ansluta toohello klustret på https://CLUSTERNAME.azurehdinsight.net. Den här adressen använder offentliga hello-gatewayen, som inte är tillgängligt om du har använt NSG: er eller udr: er toorestrict åtkomst från hello internet.

tooconnect tooAmbari och andra webbsidor via hello virtuella nätverk, använder du hello följande steg:

1. toodiscover hello interna fullständigt kvalificerade domännamn (FQDN) för hello HDInsight-klusternoder med någon av följande metoder hello:

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

    Hitta hello FQDN för hello huvudnoderna i hello listan över noder returneras, och använder hello FQDN tooconnect tooAmbari och andra webbtjänster. Till exempel använda `http://<headnode-fqdn>:8080` tooaccess Ambari.

    > [!IMPORTANT]
    > Vissa tjänster som finns på hello huvudnoderna är bara aktiva på en nod i taget. Om du försöker komma åt en tjänst på en huvudnod och returnerar ett 404-fel, växla toohello andra huvudnod.

2. toodetermine hello nod och port som en tjänst är tillgänglig, finns hello [portar som används av Hadoop-tjänster på HDInsight](./hdinsight-hadoop-port-settings-for-services.md) dokumentet.

## <a id="networktraffic"></a>Kontrollera nätverkstrafik

Nätverkstrafik i en virtuell Azure-nätverk kan kontrolleras med hello följande metoder:

* **Nätverkssäkerhetsgrupper** (NSG) kan du toofilter inkommande och utgående trafik toohello nätverk. Mer information finns i hello [filtrera nätverkstrafik med nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md) dokumentet.

    > [!WARNING]
    > HDInsight stöder inte begränsa utgående trafik.

* **Användardefinierade vägar** (UDR) definierar hur trafiken flödar mellan resurser i hello nätverk. Mer information finns i hello [användardefinierade vägar och IP-vidarebefordring](../virtual-network/virtual-networks-udr-overview.md) dokumentet.

* **Nätverks-virtuella installationer** replikera hello funktionerna på enheter, till exempel brandväggar och routrar. Mer information finns i hello [nätverksinstallationer](https://azure.microsoft.com/solutions/network-appliances) dokumentet.

Som en hanterad tjänst kräver HDInsight obegränsad åtkomst tooAzure hälso- och tjänster i hello Azure-molnet. När du använder NSG: er och udr: er, måste du se till att HDInsight dessa tjänster kan fortfarande kommunicera med HDInsight.

HDInsight visar tjänster på flera portar. När du använder en virtuell installation brandvägg måste du tillåta trafik på hello portar som används för dessa tjänster. Mer information finns i avsnittet hello [portar som krävs].

### <a id="hdinsight-ip"></a>HDInsight med nätverkssäkerhetsgrupper och användardefinierade vägar

Om du tänker använda **nätverkssäkerhetsgrupper** eller **användardefinierade vägar** toocontrol nätverkstrafik, utför följande åtgärder innan du installerar HDInsight hello:

1. Identifiera hello Azure-region som du planerar toouse för HDInsight.

2. Identifiera hello IP-adresser som krävs av HDInsight. Mer information finns i hello [IP-adresser som krävs av HDInsight](#hdinsight-ip) avsnitt.

3. Skapa eller ändra hello nätverkssäkerhetsgrupper eller användardefinierade vägar för hello-undernätet som du planerar tooinstall HDInsight till.

    * __Nätverkssäkerhetsgrupper__: Tillåt __inkommande__ trafik på port __443__ från hello IP-adresser.
    * __Användardefinierade vägar__: skapa en väg tooeach IP-adress och ange hello __nästa hopptyp__ too__Internet__.

Mer information om nätverkssäkerhetsgrupper eller användardefinierade vägar finns hello följande dokumentation:

* [Nätverkssäkerhetsgrupp](../virtual-network/virtual-networks-nsg.md)

* [Användardefinierade vägar](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling"></a>Tvingad tunneltrafik

Tvingad tunneling är en användardefinierad konfiguration där all trafik från ett undernät är framtvingad tooa specifika nätverk eller plats, till exempel ditt lokala nätverk. HDInsight har __inte__ stöd Tvingad tunneltrafik.

## <a id="hdinsight-ip"></a>Den begärda IP-adresser

> [!IMPORTANT]
> hello Azure hälsa och hanteringstjänster måste vara kan toocommunicate med HDInsight. Om du använder nätverkssäkerhetsgrupper eller användardefinierade vägar tillåta trafik från hello IP-adresser för dessa tjänster tooreach HDInsight.
>
> Om du inte använder nätverkssäkerhetsgrupper eller användardefinierade vägar toocontrol trafik, kan du ignorera det här avsnittet.

Om du använder nätverkssäkerhetsgrupper eller användardefinierade vägar, måste du tillåta trafik från hello Azure hälsa och management services tooreach HDInsight. Använd hello följande steg toofind hello IP-adresser som får:

1. Du måste alltid tillåta trafik från hello efter IP-adresser:

    | IP-adress | Tillåtna port | Riktning |
    | ---- | ----- | ----- |
    | 168.61.49.99 | 443 | Inkommande |
    | 23.99.5.239 | 443 | Inkommande |
    | 168.61.48.131 | 443 | Inkommande |
    | 138.91.141.162 | 443 | Inkommande |

2. Om ditt HDInsight-kluster finns i något av följande regioner hello, måste du tillåta trafik från hello IP-adresser som anges för hello region:

    > [!IMPORTANT]
    > Om inte hello Azure-region som du använder visas endast Använd hello fyra IP-adresser från steg 1.

    | Land/region | Region | Tillåtna IP-adresser | Tillåtna port | Riktning |
    | ---- | ---- | ---- | ---- | ----- |
    | Asien | Östasien | 23.102.235.122</br>52.175.38.134 | 443 | Inkommande |
    | &nbsp; | Sydostasien | 13.76.245.160</br>13.76.136.249 | 443 | Inkommande |
    | Australien | Östra Australien | 104.210.84.115</br>13.75.152.195 | 443 | Inkommande |
    | &nbsp; | Sydöstra Australien | 13.77.2.56</br>13.77.2.94 | 443 | Inkommande |
    | Brasilien | Södra Brasilien | 191.235.84.104</br>191.235.87.113 | 443 | Inkommande |
    | Kanada | Östra Kanada | 52.229.127.96</br>52.229.123.172 | 443 | Inkommande |
    | &nbsp; | Centrala Kanada | 52.228.37.66</br>52.228.45.222 | 443 | Inkommande |
    | Kina | Norra Kina | 42.159.96.170</br>139.217.2.219 | 443 | Inkommande |
    | &nbsp; | Östra Kina | 42.159.198.178</br>42.159.234.157 | 443 | Inkommande |
    | Europa | Norra Europa | 52.164.210.96</br>13.74.153.132 | 443 | Inkommande |
    | &nbsp; | Västra Europa| 52.166.243.90</br>52.174.36.244 | 443 | Inkommande |
    | Tyskland | Centrala Tyskland | 51.4.146.68</br>51.4.146.80 | 443 | Inkommande |
    | &nbsp; | Nordöstra Tyskland | 51.5.150.132</br>51.5.144.101 | 443 | Inkommande |
    | Indien | Indien, centrala | 52.172.153.209</br>52.172.152.49 | 443 | Inkommande |
    | Japan | Östra Japan | 13.78.125.90</br>13.78.89.60 | 443 | Inkommande |
    | &nbsp; | Västra Japan | 40.74.125.69</br>138.91.29.150 | 443 | Inkommande |
    | Korea | Centrala Korea | 52.231.39.142</br>52.231.36.209 | 433 | Inkommande |
    | &nbsp; | Sydkorea | 52.231.203.16</br>52.231.205.214 | 443 | Inkommande
    | Storbritannien | Storbritannien, västra | 51.141.13.110</br>51.141.7.20 | 443 | Inkommande |
    | &nbsp; | Storbritannien, södra | 51.140.47.39</br>51.140.52.16 | 443 | Inkommande |
    | USA | Centrala USA | 13.67.223.215</br>40.86.83.253 | 443 | Inkommande |
    | &nbsp; | Norra centrala USA | 157.56.8.38</br>157.55.213.99 | 443 | Inkommande |
    | &nbsp; | Västra centrala USA | 52.161.23.15</br>52.161.10.167 | 443 | Inkommande |
    | &nbsp; | Västra USA 2 | 52.175.211.210</br>52.175.222.222 | 443 | Inkommande |

    Information om hello IP-adresserna för toouse för Azure Government, finns hello [Azure Government Intelligence + analys](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) dokumentet.

3. Om du använder en anpassad DNS-server med det virtuella nätverket måste du också tillåta åtkomst från __168.63.129.16__. Den här adressen är Azures rekursiv matchning. Mer information finns i hello [namnmatchning för virtuella datorer och rollen instanser](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) dokumentet.

Mer information finns i hello [styra nätverkstrafiken](#networktraffic) avsnitt.

## <a id="hdinsight-ports"></a>Portar som krävs

Om du tänker använda ett nätverk **virtuell installation brandväggen** toosecure hello virtuellt nätverk, måste du tillåta utgående trafik på hello följande portar:

* 53
* 443
* 1433
* 11000-11999
* 14000-14999

En lista över portar för vissa tjänster finns hello [portar som används av Hadoop-tjänster på HDInsight](hdinsight-hadoop-port-settings-for-services.md) dokumentet.

Mer information om brandväggsregler för virtuella installationer finns hello [virtuella utrustningsscenario](../virtual-network/virtual-network-scenario-udr-gw-nva.md) dokumentet.

## <a id="hdinsight-nsg"></a>Exempel: nätverkssäkerhetsgrupper med HDInsight

hello exemplen i det här avsnittet visar hur toocreate nätverkssäkerhetsgruppen regler som tillåter HDInsight toocommunicate med hello Azure hanteringstjänster. Innan du använder hello exempel justera hello IP-adresser toomatch hello de för hello Azure-region som du använder. Du hittar den här informationen i hello [HDInsight med nätverkssäkerhetsgrupper och användardefinierade vägar](#hdinsight-ip) avsnitt.

### <a name="azure-resource-management-template"></a>Azure Resource Manager-mall

hello skapas följande resurshantering ett virtuellt nätverk som begränsar inkommande trafik, men tillåter trafik från hello IP-adresser som krävs av HDInsight. Den här mallen skapar också ett HDInsight-kluster i hello virtuella nätverk.

* [Distribuera en skyddad virtuell Azure-nätverk och ett HDInsight Hadoop-kluster](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

> [!IMPORTANT]
> Ändra hello IP-adresser som används i det här exemplet toomatch hello Azure-region som du använder. Du hittar den här informationen i hello [HDInsight med nätverkssäkerhetsgrupper och användardefinierade vägar](#hdinsight-ip) avsnitt.

### <a name="azure-powershell"></a>Azure PowerShell

Använd följande PowerShell-skriptet toocreate ett virtuellt nätverk begränsar inkommande trafik som tillåter trafik från hello IP-adresser för hello Nordeuropa region hello.

> [!IMPORTANT]
> Ändra hello IP-adresser som används i det här exemplet toomatch hello Azure-region som du använder. Du hittar den här informationen i hello [HDInsight med nätverkssäkerhetsgrupper och användardefinierade vägar](#hdinsight-ip) avsnitt.

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with hello resource group hello virtual network is in"
$subnetName = "Replace with hello name of hello subnet that you plan toouse for HDInsight"
# Get hello Virtual Network object
$vnet = Get-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get hello region hello Virtual network is in.
$location = $vnet.Location
# Get hello subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for hello HDInsight health and management services.
$nsg = New-AzureRmNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -name "hdirule1" `
        -Description "HDI health and management address 52.164.210.96" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "52.164.210.96" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 300 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 13.74.153.132" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "13.74.153.132" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 301 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.49.99" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.49.99" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 302 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 23.99.5.239" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "23.99.5.239" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 303 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.48.131" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.48.131" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 304 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 138.91.141.162" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "138.91.141.162" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 305 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "blockeverything" `
        -Description "Block everything else" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "*" `
        -SourceAddressPrefix "Internet" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Deny `
        -Priority 500 `
        -Direction Inbound
# Set hello changes toohello security group
Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply hello NSG toohello subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

> [!IMPORTANT]
> Det här exemplet visar hur tooadd regler tooallow inkommande trafik på hello som krävs för IP-adresser. Det innehåller inte en regel toorestrict inkommande åtkomst från andra källor.
>
> hello som följande exempel visar hur tooenable SSH åt från hello Internet:
>
> ```powershell
> Add-AzureRmNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-cli"></a>Azure CLI

Använd hello följande steg toocreate ett virtuellt nätverk som begränsar inkommande trafik, men tillåter trafik från hello IP-adresser som krävs av HDInsight.

1. Använd hello efter kommandot toocreate en ny säkerhetsgrupp för nätverk med namnet `hdisecure`. Ersätt **RESOURCEGROUPNAME** med hello resursgruppen som innehåller hello Azure Virtual Network. Ersätt **plats** med hello plats (region) hello gruppen har skapats i.

    ```azurecli
    az network nsg create -g RESOURCEGROUPNAME -n hdisecure -l LOCATION
    ```

    När hello grupp har skapats, får du information på hello ny grupp.

2. Använd hello följande tooadd regler toohello ny säkerhetsgrupp för nätverk som tillåter inkommande kommunikation på port 443 från hello Azure HDInsight-tjänst för hälsotillstånd och hantering. Ersätt **RESOURCEGROUPNAME** med hello namnet hello resursgruppen som innehåller hello Azure Virtual Network.

    > [!IMPORTANT]
    > Ändra hello IP-adresser som används i det här exemplet toomatch hello Azure-region som du använder. Du hittar den här informationen i hello [HDInsight med nätverkssäkerhetsgrupper och användardefinierade vägar](#hdinsight-ip) avsnitt.

    ```azurecli
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n block --protocol "*" --source-port-range "*" --destination-port-range "*" --source-address-prefix "Internet" --destination-address-prefix "VirtualNetwork" --access "Deny" --priority 500 --direction "Inbound"
    ```

3. tooretrieve hello Unik identifierare för den här nätverkssäkerhetsgruppen använder hello följande kommando:

    ```azurecli
    az network nsg show -g RESOURCEGROUPNAME -n hdisecure --query 'id'
    ```

    Det här kommandot returnerar ett värde liknande toohello följande text:

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    Använd dubbla citattecken runt id i hello-kommandot om du inte får hello förväntat resultat.

4. Använd hello efter kommandot tooapply hello säkerhet grupp tooa undernät. Ersätt hello __GUID__ och __RESOURCEGROUPNAME__ värden med hello som returnerades från hello föregående steg. Ersätt __VNETNAME__ och __SUBNETNAME__ med hello virtuella nätverksnamnet och undernät som du vill toocreate.

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUPNAME --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    När det här kommandot har slutförts kan du installera HDInsight i hello virtuellt nätverk.

> [!IMPORTANT]
> De här stegen kan du bara öppna toohello HDInsight hälsa och hantering av tjänsten för dataåtkomst på hello Azure-molnet. Alla andra åtkomst toohello HDInsight-kluster från utanför hello virtuellt nätverk har blockerats. tooenable åtkomst från utanför hello virtuellt nätverk, måste du lägga till ytterligare Nätverkssäkerhetsgruppen regler.
>
> hello som följande exempel visar hur tooenable SSH åt från hello Internet:
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <a id="example-dns"></a>Exempel: DNS-konfiguration

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a>Namnmatchning mellan ett virtuellt nätverk och ett anslutna lokalt nätverk

Det här exemplet gör hello följande förutsättningar:

* Du har ett virtuellt Azure-nätverk som är anslutna tooan lokalt nätverk via en VPN-gateway.

* hello anpassad DNS-server i hello virtuellt nätverk kör Linux eller Unix som hello-operativsystem.

* [Binda](https://www.isc.org/downloads/bind/) är installerad på hello anpassad DNS-server.

På hello anpassade DNS-server i hello virtuellt nätverk:

1. Använda Azure PowerShell eller Azure CLI toofind hello DNS-suffix hello virtuellt nätverk:

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Använd hello följande text som hello hello på hello anpassade DNS-server för virtuellt nätverk för hello `/etc/bind/named.conf.local` fil:

    ```
    // Forward requests for hello virtual network suffix tooAzure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    Ersätt hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` värdet med hello DNS-suffixet för det virtuella nätverket.

    Den här konfigurationen dirigerar alla DNS-förfrågningar för hello DNS-suffix hello virtuellt nätverk toohello Azure rekursiv matchning.

2. Använd hello följande text som hello hello på hello anpassade DNS-server för virtuellt nätverk för hello `/etc/bind/named.conf.options` fil:

    ```
    // Clients tooaccept requests from
    // TODO: Add hello IP range of hello joined network toothis list
    acl goodclients {
        10.0.0.0/16; # IP address range of hello virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent toohello following
            forwarders {
                192.168.0.1; # Replace with hello IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform tooRFC1035
            listen-on { any; };
    };
    ```
    
    * Ersätt hello `10.0.0.0/16` värdet med hello IP-adressintervall för det virtuella nätverket. Den här posten kan name resolution begäranden adresser i det här intervallet.

    * Lägg till hello IP-adressintervall hello lokalt nätverk toohello `acl goodclients { ... }` avsnitt.  posten kan namnmatchning från resurser i hello lokalt nätverk.
    
    * Ersätt värdet för hello `192.168.0.1` med hello IP-adressen för lokala DNS-servern. Den här posten dirigerar alla andra DNS-förfrågningar toohello lokala DNS-servern.

3. toouse hello konfiguration, starta om bindning. Till exempel `sudo service bind9 restart`.

4. Lägga till en villkorlig vidarebefordrare toohello lokala DNS-server. Konfigurera hello villkorlig vidarebefordrare toosend begäranden för hello DNS-suffix från steg 1 toohello anpassade DNS-server.

    > [!NOTE]
    > Hello i dokumentationen för DNS-programvaran för att närmare information om hur tooadd en villkorlig vidarebefordrare.

När du har slutfört de här stegen kan du ansluta tooresources i antingen nätverk med fullständigt kvalificerade domännamn (FQDN). Du kan nu installera HDInsight i hello virtuellt nätverk.

### <a name="name-resolution-between-two-connected-virtual-networks"></a>Namnmatchning mellan två anslutna virtuella nätverk

Det här exemplet gör hello följande förutsättningar:

* Du har två virtuella Azure-nätverk som är anslutna med hjälp av en VPN-gateway eller peering.

* hello anpassade DNS-servern i båda nätverken kör Linux eller Unix som hello-operativsystem.

* [Binda](https://www.isc.org/downloads/bind/) är installerad på hello anpassade DNS-servrar.

1. Använda Azure PowerShell eller Azure CLI toofind hello DNS-suffix för båda virtuella nätverken:

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Använd hello följande text som hello hello `/etc/bind/named.config.local` fil på hello anpassad DNS-server. Gör den här ändringen hello anpassade DNS-servern i båda virtuella nätverken.

    ```
    // Forward requests for hello virtual network suffix tooAzure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # hello IP address of hello DNS server in hello other virtual network
    };
    ```

    Ersätt hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` värdet med hello DNS-suffixet hello __andra__ virtuellt nätverk. Den här posten skickar begäranden för hello DNS-suffix hello fjärrnätverket toohello anpassade DNS i nätverket.

3. På hello anpassade DNS-servrar i båda virtuella nätverken använder du följande text som hello hello hello `/etc/bind/named.conf.options` fil:

    ```
    // Clients tooaccept requests from
    acl goodclients {
        10.1.0.0/16; # hello IP address range of one virtual network
        10.0.0.0/16; # hello IP address range of hello other virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            forwarders {
            168.63.129.16;   # Azure recursive resolver         
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform tooRFC1035
            listen-on { any; };
    };
    ```
    
    * Ersätt hello `10.0.0.0/16` och `10.1.0.0/16` värden med hello IP-adressintervall för ditt virtuella nätverk. Den här posten kan resurser i varje nätverk toomake begäranden hello DNS-servrar.

    Alla begäranden som inte är för hello DNS-suffix hello virtuella nätverk (till exempel microsoft.com) hanteras av hello Azure rekursiv matchning.

4. toouse hello konfiguration, starta om bindning. Till exempel `sudo service bind9 restart` på båda DNS-servrar.

När du har slutfört de här stegen kan du ansluta tooresources i hello virtuellt nätverk med fullständigt kvalificerade domännamn (FQDN). Du kan nu installera HDInsight i hello virtuellt nätverk.

## <a name="next-steps"></a>Nästa steg

* Slutpunkt till slutpunkt-exempel på hur du konfigurerar HDInsight tooconnect tooan lokalt nätverk finns i [ansluta HDInsight tooan lokalt nätverk](./connect-on-premises-network.md).

* Mer information om virtuella Azure-nätverk finns hello [översikt över Azure Virtual Network](../virtual-network/virtual-networks-overview.md).

* Mer information om nätverkssäkerhetsgrupper finns [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md).

* Mer information om användardefinierade vägar finns [användardefinierade vägar och IP-vidarebefordring](../virtual-network/virtual-networks-udr-overview.md).