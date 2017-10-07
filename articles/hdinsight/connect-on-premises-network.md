---
title: "aaaConnect HDInsight tooyour lokalt nätverk - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate ett HDInsight-kluster i en Azure-nätverk och ansluter sedan tooyour lokalt nätverk. Lär dig hur tooconfigure namnmatchning mellan HDInsight och ditt lokala nätverk med hjälp av en anpassad DNS-server."
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.openlocfilehash: 8a3adf0e3df7726d8e6566d723700506baaf89a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-hdinsight-tooyour-on-premise-network"></a>Ansluta HDInsight tooyour lokalt nätverk

Lär dig hur tooconnect HDInsight tooyour lokalt nätverk med hjälp av virtuella Azure-nätverk och en VPN-gateway. Det här dokumentet innehåller planeringsinformation om:

* Använda HDInsight i ett virtuellt Azure-nätverk som ansluter tooyour lokalt nätverk.

* Konfigurera DNS-namnmatchningen mellan hello virtuella nätverket och ditt lokala nätverk.

* Konfigurera network security grupper toorestrict internet access tooHDInsight.

* Portar som tillhandahålls av HDInsight hello virtuella nätverket.

## <a name="create-hello-virtual-network-configuration"></a>Skapa hello virtuella nätverkskonfigurationen

> [!IMPORTANT]
> Om du letar efter steg-för-steg-vägledning om hur du ansluter HDInsight tooyour lokalt nätverk via ett virtuellt Azure-nätverk, se hello [ansluta HDInsight tooyour lokalt nätverk](connect-on-premises-network.md) dokumentet.

Använd hello följande dokument toolearn hur toocreate ett virtuellt Azure-nätverk som är anslutna tooyour lokala nätverk:
    
* [Med hjälp av hello Azure-portalen](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [Använda Azure PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [Använda Azure CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a>Konfigurera namnmatchning

tooallow HDInsight och resurser i hello anslutna nätverket toocommunicate efter namn, måste du utföra följande åtgärder hello:

* Skapa en anpassad DNS-server i hello Azure Virtual Network.

* Konfigurera hello virtuellt nätverk toouse hello anpassad DNS-server i stället för default hello Azure rekursiv matchning.

* Konfigurera vidarebefordran mellan hello anpassad DNS-server och lokala DNS-servern.

Den här konfigurationen kan hello följande beteende:

* Begäranden om fullständiga domännamn som har hello DNS-suffix __för hello virtuella nätverket__ vidarebefordras toohello anpassad DNS-server. hello anpassade DNS-servern vidarebefordrar sedan dessa begäranden toohello Azure rekursiv matcharen som returnerar hello IP-adress.

* Alla andra begäranden vidarebefordras toohello lokala DNS-servern. Även begäranden för offentliga internet-resurser, till exempel microsoft.com vidarebefordras toohello lokala DNS-server för namnmatchning.

I följande diagram hello, är gröna linjer begäranden för resurser som slutar i hello DNS-suffix hello virtuellt nätverk. Blå raderna är begäranden för resurser i hello lokala nätverk eller på hello offentliga internet.

![Diagram över hur DNS-förfrågningar har lösts i hello konfiguration används i det här dokumentet](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

### <a name="create-a-custom-dns-server"></a>Skapa en anpassad DNS-server

> [!IMPORTANT]
> Du måste skapa och konfigurera hello DNS-servern innan du installerar HDInsight i hello virtuellt nätverk.

toocreate en Linux VM som använder hello [binda](https://www.isc.org/downloads/bind/) DNS-programvara, Använd hello följande steg:

> [!NOTE]
> hello följande använda hello [Azure-portalen](https://portal.azure.com) toocreate en virtuell dator i Azure. Andra sätt toocreate en virtuell dator finns hello [skapa VM - Azure CLI](../virtual-machines/linux/quick-create-cli.md) och [skapa VM - Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) dokument.

1. Från hello [Azure-portalen](https://portal.azure.com)väljer  __+__ , __Compute__, och __Ubuntu Server 16.04 LTS__.

    ![Skapa en virtuell Ubuntu-dator](./media/connect-on-premises-network/create-ubuntu-vm.png)

2. Från hello __grunderna__ ange hello följande information:

    * __Namnet__: ett eget namn som identifierar den här virtuella datorn. Till exempel __DNSProxy__.
    * __Användarnamnet__: hello namnet på hello SSH-konto.
    * __Offentlig SSH-nyckel__ eller __lösenord__: hello autentiseringsmetod för hello SSH-konto. Vi rekommenderar att du använder offentliga nycklar som de är säkrare. Mer information finns i hello [skapa och använda SSH-nycklar för Linux virtuella datorer](../virtual-machines/linux/mac-create-ssh-keys.md) dokumentet.
    * __Resursgruppen__: Välj __använda befintliga__, och välj sedan hello resursgruppen som innehåller hello virtuella nätverk som skapades tidigare.
    * __Plats__: Välj hello samma plats som hello virtuellt nätverk.

    ![Grundläggande konfiguration av virtuell dator](./media/connect-on-premises-network/vm-basics.png)

    Lämna andra transaktioner på hello standardvärden och välj sedan __OK__.

3. Från hello __välja en storlek__ avsnittet väljer hello VM-storlek. Välj hello minsta och lägsta kostnaden alternativet för den här självstudiekursen. toocontinue, Använd hello __Välj__ knappen.

4. Från hello __inställningar__ ange hello följande information:

    * __Virtuellt nätverk__: Välj hello virtuella nätverk som du skapade tidigare.

    * __Undernät__: Välj hello standardundernät för hello virtuellt nätverk. Gör __inte__ Välj hello undernät som används av hello VPN-gateway.

    * __Diagnostiklagringskonto__: Välj ett befintligt lagringskonto eller skapa en ny.

    ![Inställningarna för virtuella nätverk](./media/connect-on-premises-network/virtual-network-settings.png)

    Lämna hello andra transaktioner på hello standardvärdet, och välj sedan __OK__ toocontinue.

5. Från hello __inköp__ avsnitt, Välj hello __inköp__ knappen toocreate hello virtuell dator.

6. När hello virtuella datorn har skapats, dess __översikt__ avsnitt visas. Välj hello listan hello vänster __egenskaper__. Spara hello __offentliga IP-adressen__ och __privata IP-adressen__ värden. Den används i nästa avsnitt om hello.

    ![Offentliga och privata IP-adresser](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a>Installera och konfigurera Bind (DNS-programvara)

1. Använda SSH tooconnect toohello __offentliga IP-adressen__ för hello virtuell dator. följande exempel hello ansluter tooa virtuell dator på 40.68.254.142:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    Ersätt `sshuser` med hello SSH-användarkonto du angav när du skapar hello kluster.

    > [!NOTE]
    > Det finns en mängd olika sätt tooobtain hello `ssh` verktyget. Linux-, Unix- och macOS anges som en del av hello-operativsystemet. Om du använder Windows, bör du något av följande alternativ för hello:
    >
    > * [Azure-molnet Shell](../cloud-shell/quickstart.md)
    > * [Bash på Ubuntu på Windows 10](https://msdn.microsoft.com/commandline/wsl/about)
    > * [Git (https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. tooinstall bindning, Använd följande kommandon från hello SSH-session hello:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. tooconfigure Bind tooforward namn upplösning begäranden tooyour lokala DNS-servern använder hello följande text som hello hello `/etc/bind/named.conf.options` fil:

        acl goodclients {
            10.0.0.0/16; # Replace with hello IP address range of hello virtual network
            10.1.0.0/16; # Replace with hello IP address range of hello on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with hello IP address of hello on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform tooRFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]
    > Ersätt hello värden i hello `goodclients` avsnitt med hello IP-adressintervall i hello virtuella nätverk och lokala nätverk. Det här avsnittet definierar hello-adresser som DNS-servern accepterar begäranden från.
    >
    > Ersätt hello `192.168.0.1` post i hello `forwarders` avsnitt med hello IP-adress i lokala DNS-servern. Den här posten vägar DNS-förfrågningar tooyour lokala DNS-servern för namnmatchning.

    tooedit den här filen, Använd hello följande kommando:

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    toosave hello-fil, Använd __Ctrl + X__, __Y__, och sedan __RETUR__.

4. Använd följande kommando hello från hello SSH-sessionen:

    ```bash
    hostname -f
    ```

    Det här kommandot returnerar ett värde liknande toohello följande text:

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    Hej `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` texten är hello __DNS-suffix__ för den här virtuella nätverket. Spara det här värdet eftersom det används senare.

5. tooconfigure Bind tooresolve DNS-namn för resurser inom hello virtuellt nätverk, använder hello följande text som hello hello `/etc/bind/named.conf.local` fil:

        // Replace hello following with hello DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # hello Azure recursive resolver
        };

    > [!IMPORTANT]
    > Du måste ersätta hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` med hello DNS-suffix som du hämtade tidigare.

    tooedit den här filen, Använd hello följande kommando:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    toosave hello-fil, Använd __Ctrl + X__, __Y__, och sedan __RETUR__.

6. toostart bindning, Använd hello följande kommando:

    ```bash
    sudo service bind9 restart
    ```

7. tooverify binda kan lösa hello namnen på de resurser i ditt lokala nätverk, Använd hello följande kommandon:

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > Ersätt `dns.mynetwork.net` med hello fullständigt kvalificerade domännamnet (FQDN) för en resurs i ditt lokala nätverk.
    >
    > Ersätt `10.0.0.4` med hello __interna IP-adress__ på anpassade DNS-servern i hello virtuella nätverk.

    hello svar visas liknande toohello följande text:

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-hello-virtual-network-toouse-hello-custom-dns-server"></a>Konfigurera hello virtuellt nätverk toouse hello anpassade DNS-server

tooconfigure hello virtuellt nätverk toouse hello anpassade DNS-servern i stället för hello Azure rekursiv matcharen använda hello följande steg:

1. I hello [Azure-portalen](https://portal.azure.com), Välj hello virtuella nätverk och väljer sedan __DNS-servrar__.

2. Välj __anpassade__, och ange hello __interna IP-adress__ för hello anpassade DNS-server. Välj slutligen __spara__.

    ![Ange hello anpassade DNS-server för hello nätverket](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-hello-on-premises-dns-server"></a>Konfigurera hello lokala DNS-server

I föregående avsnitt hello konfigurerat du hello anpassade DNS-server tooforward begäranden toohello lokala DNS-server. Därefter måste du konfigurera hello lokala DNS-server tooforward begäranden toohello anpassade DNS-server.

Specifika anvisningar för hur tooconfigure DNS-servern, hello finns i dokumentationen för din DNS-server-program. Leta efter hello instruktioner för hur tooconfigure en __villkorlig vidarebefordrare__.

Villkorlig vidarebefordran vidarebefordrar bara begäranden för en specifik DNS-suffix. I det här fallet måste du konfigurera en vidarebefordrare för hello DNS-suffix hello virtuellt nätverk. Begäranden för det här suffixet ska vidarebefordras toohello hello anpassade DNS-serverns IP-adress. 

hello följande är ett exempel på en villkorlig vidarebefordrare konfiguration för hello **binda** DNS-programvara:

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # hello custom DNS server's internal IP address
    };

Information om hur du använder DNS på **Windows Server 2016**, se hello [Lägg till DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) dokumentationen...

När du har konfigurerat hello lokala DNS-server, kan du använda `nslookup` från hello lokalt nätverk tooverify att du kan matcha namnen i hello virtuellt nätverk. följande exempel hello 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

Det här exemplet använder hello lokala DNS-servern vid 196.168.0.4 tooresolve hello namnet på hello anpassad DNS-server. Ersätt hello IP-adress med hello en för hello lokala DNS-server. Ersätt hello `dnsproxy` adress med hello fullständigt kvalificerade domännamnet för hello anpassad DNS-server.

## <a name="optional-control-network-traffic"></a>Valfritt: Kontrollen nätverkstrafik

Du kan använda nätverkssäkerhetsgrupper (NSG) eller användardefinierade vägar (UDR) toocontrol nätverkstrafik. NSG: er kan du toofilter inkommande och utgående trafik, och tillåta eller neka hello trafik. Udr: er kan du toocontrol hur trafiken flödar mellan resurser i hello virtuellt nätverk, hello internet och hello lokalt nätverk.

> [!WARNING]
> HDInsight kräver inkommande åtkomst från särskilda IP-adresser i hello Azure-molnet och obegränsad utgående åtkomst. När du använder NSG: er eller udr: er toocontrol trafik, måste du utföra hello följande steg:
>
> 1. Hitta hello IP-adresser för hello-plats som innehåller det virtuella nätverket. En lista över nödvändiga IP-adresser per plats, se [krävs IP-adresser](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).
>
> 2. Tillåta inkommande trafik från hello IP-adresser.
>
>    * __NSG__: Tillåt __inkommande__ trafik på port __443__ från hello __Internet__.
>    * __UDR__: Ange hello __nästa hopp__ typ av hello flödet too__Internet__.

Ett exempel på hur Azure PowerShell eller hello Azure CLI toocreate NSG: er finns i hello [utöka HDInsight med Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) dokumentet.

## <a name="create-hello-hdinsight-cluster"></a>Skapa hello HDInsight-kluster

> [!WARNING]
> Innan du installerar HDInsight i hello virtuella nätverk måste du konfigurera hello anpassad DNS-server.

Använd hello stegen i hello [skapar ett HDInsight-kluster med hjälp av hello Azure-portalen](./hdinsight-hadoop-create-linux-clusters-portal.md) dokument toocreate ett HDInsight-kluster.

> [!WARNING]
> * Du måste välja hello-plats som innehåller det virtuella nätverket när klustret skapas.
>
> * I hello __avancerade inställningar__ en del av konfigurationen, måste du välja hello virtuella nätverk och undernät som du skapade tidigare.

## <a name="connecting-toohdinsight"></a>Ansluta tooHDInsight

De flesta dokumentation på HDInsight förutsätter att du har åtkomst toohello klustret via hello internet. Till exempel att du kan ansluta toohello klustret på https://CLUSTERNAME.azurehdinsight.net. Den här adressen använder offentliga hello-gatewayen, som inte är tillgängligt om du har använt NSG: er eller udr: er toorestrict åtkomst från hello internet.

toodirectly ansluta tooHDInsight via hello virtuellt nätverk, använder hello följande steg:

1. toodiscover hello interna fullständigt kvalificerade domännamn för hello HDInsight-klusternoder med någon av följande metoder hello:

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

2. toodetermine hello port som en tjänst är tillgänglig, finns hello [portar som används av Hadoop-tjänster på HDInsight](./hdinsight-hadoop-port-settings-for-services.md) dokumentet.

    > [!IMPORTANT]
    > Vissa tjänster som finns på hello huvudnoderna är bara aktiva på en nod i taget. Om du försöker att komma åt en tjänst på en huvudnod och det misslyckas, växla toohello andra huvudnod.
    >
    > Till exempel är Ambari endast aktiv i en huvudnod i taget. Om du försöker komma åt Ambari på en huvudnod och returnerar ett 404-fel, och sedan den körs på hello andra huvudnod.

## <a name="next-steps"></a>Nästa steg

* Mer information om hur du använder HDInsight i ett virtuellt nätverk finns [utöka HDInsight med hjälp av Azure Virtual Networks](./hdinsight-extend-hadoop-virtual-network.md).

* Mer information om virtuella Azure-nätverk finns hello [översikt över Azure Virtual Network](../virtual-network/virtual-networks-overview.md).

* Mer information om nätverkssäkerhetsgrupper finns [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md).

* Mer information om användardefinierade vägar finns [användardefinierade vägar och IP-vidarebefordring](../virtual-network/virtual-networks-udr-overview.md).
