---
title: "aaaCreate en virtuell dator (klassisk) med flera nätverkskort med hjälp av PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate och konfigurera virtuella datorer med flera nätverkskort med hjälp av PowerShell."
services: virtual-network, virtual-machines
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a1a3952c-2dcc-4977-bd7a-52d623c1fb07
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 8ef35bd4cfd7e6a527080f1cfc541275ca86f5e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics"></a>Skapa en virtuell dator (klassisk) med flera nätverkskort
Du kan skapa virtuella datorer (VM) i Azure och bifoga flera nätverk gränssnitt (NIC) tooeach för dina virtuella datorer. Flera nätverkskort är ett krav för många virtuella nätverksenheter, till exempel leverans av program och optimering av WAN-lösningar. Flera nätverkskort kan också tillhandahålla isolering för trafik mellan nätverkskort.

![Flera nätverkskort för den virtuella datorn](./media/virtual-networks-multiple-nics/IC757773.png)

hello bild visas en virtuell dator med tre nätverkskort, var och en ansluten tooa olika undernät.

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager.

* Internet-riktade VIP (klassiska distributioner) stöds bara på hello ”default” NIC. Det finns endast en VIP toohello IP hello standard NIC.
* Instans nivå offentlig IP (LPIP)-adresser (klassiska distributioner) stöds inte för flera virtuella datorer NIC för tillfället.
* Hej ordning hello nätverkskort från inuti hello VM slumpmässigt och kan också ändra över Azure-infrastrukturuppdateringar. Hej IP-adresser och hello motsvarande ethernet MAC adresser förblir hello samma. Anta till exempel **Eth1** har 10.1.0.100 och MAC-adress 00-0D-3A-B0-39-0D; när en uppdatering för Azure-infrastrukturen och omstart, den kan ändras för**Eth2**, men hello IP- och MAC länkning kommer förblir hello samma. Om en omstart kund-initierad hello NIC ordning förblir hello samma.
* hello-adress för varje nätverkskort på varje virtuell dator måste finnas i ett undernät, flera nätverkskort på en enda VM kan varje tilldelas adresser i hello samma undernät.
* hello VM-storlek anger hello antal nätverkskort som du kan skapa för en virtuell dator. Referens hello [Windows Server](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) och [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) VM-storlekar artiklar toodetermine hur många nätverkskort som har stöd för varje VM-storlek. 

## <a name="network-security-groups-nsgs"></a>Nätverkssäkerhetsgrupper (NSG: er)
I en Resource Manager-distribution, kan ett nätverkskort på en virtuell dator associeras med en Nätverkssäkerhetsgrupp (NSG), inklusive alla nätverkskort på en virtuell dator som har flera nätverkskort som är aktiverad. Om ett nätverkskort tilldelas en adress i ett undernät där hello undernätet är kopplat till en NSG, sedan gäller hello regler i hello undernät NSG även toothat NIC. Du kan även associera ett nätverkskort med en NSG i tillägg tooassociating undernät med NSG: er.

Om ett undernät som är associerad med en NSG och ett nätverkskort i undernätet är individuellt associerad med en NSG, hello associerade NSG-regler tillämpas i **flöda ordning** enligt toohello trafikriktning hello som skickas till eller från hello NIC:

* **Inkommande trafik** vars mål är hello NIC i fråga först flödar genom hello undernät, utlöser hello undernät NSG-regler innan du skickar till hello NIC och sedan utlösa hello NIC NSG-regler.
* **Utgående trafik** vars källan är hello NIC i fråga flödar först ut från hello NIC, utlöser hello NIC NSG-regler innan passera hello undernät och utlösa hello undernät NSG-regler.

Lär dig mer om [Nätverkssäkerhetsgrupper](virtual-networks-nsg.md) och hur de används baserat på kopplingarna toosubnets, virtuella datorer och nätverkskort...

## <a name="how-tooconfigure-a-multi-nic-vm-in-a-classic-deployment"></a>Hur tooConfigure multi NIC VM i en klassisk distribution
hello anvisningarna nedan hjälper dig att skapa flera NIC VM som innehåller 3 nätverkskort: standard NIC och två ytterligare nätverkskort. hello konfigurationssteg skapar en virtuell dator som kommer att konfigureras enligt toohello service configuration file fragment nedan:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over hello remainder section …
    </VirtualNetworkSite>


Du måste följande krav innan du försöker toorun hello PowerShell-kommandon i hello exempel hello.

* En Azure-prenumeration.
* Konfigurerade virtuella nätverk. Se [översikt över virtuella nätverk](virtual-networks-overview.md) mer information om Vnet.
* hello senaste versionen av Azure PowerShell hämtas och installeras. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

toocreate en virtuell dator med flera nätverkskort, fullständig hello följande genom att ange varje kommando i en enda PowerShell-session:

1. Välj en VM-avbildning från galleriet för Virtuella Azure-avbildning. Observera att bilder ändras ofta och är tillgängliga efter region. hello avbildning som anges i hello exemplet nedan får ändra eller kanske inte i din region, så Tänk toospecify hello bilden som du behöver.

    ```powershell
    $image = Get-AzureVMImage `
    -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"
    ```

2. Skapa en VM-konfiguration.

    ```powershell
    $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
    -Image $image.ImageName –AvailabilitySetName "MyAVSet"
    ```

3. Skapa hello standard administratörsinloggning.

    ```powershell
    Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
    -Password "<YourAdminPassword>"
    ```

4. Lägg till ytterligare nätverkskort toohello VM-konfiguration.

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
    -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
    Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
    -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm
    ```

5. Ange hello undernät och IP-adress för hello standard nätverkskort.

    ```powershell
    Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
    Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm
    ```

6. Skapa hello VM i ditt virtuella nätverk.

    ```powershell
    New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm
    ```

    > [!NOTE]
    > Hej VNet som du anger här måste redan finnas (som anges i hello krav). hello exemplet nedan anger ett virtuellt nätverk med namnet **MultiNIC VNet**.
    >

## <a name="limitations"></a>Begränsningar
hello följande begränsningar gäller när du använder flera nätverkskort:

* Virtuella datorer med flera nätverkskort måste skapas i virtuella Azure-nätverk (Vnet). Icke-VNet virtuella datorer kan inte konfigureras med flera nätverkskort.
* Alla virtuella datorer i en tillgänglighetsgrupp ange måste toouse flera nätverkskort eller ett enda nätverkskort. Det får inte vara en blandning av flera NIC virtuella datorer och en enda NIC virtuella datorer i en tillgänglighetsuppsättning. Samma regler gäller för virtuella datorer i en molntjänst. För flera nätverkskort virtuella datorer kan inte de nödvändiga toohave hello samma antal nätverkskort, så länge som de har minst två.
* En virtuell dator med en enda nätverkskort kan inte konfigureras med flera nätverkskort (och vice versa) när den har distribuerats, utan att ta bort och återskapa den.

## <a name="secondary-nics-access-tooother-subnets"></a>Sekundär nätverkskort åtkomst tooother undernät
Blir som standard sekundära nätverkskort inte konfigureras med en standard-gateway på grund av toowhich hello trafikflödet på hello sekundära nätverkskort begränsad toobe inom hello, samma undernät. Om hello användare vill tooenable sekundära nätverkskort tootalk utanför sin egen undernät, måste de tooadd en post i hello routning tabell tooconfigure hello-gateway som beskrivs nedan.

> [!NOTE]
> Virtuella datorer som skapades innan juli 2015 kan ha en standard-gateway som konfigurerats för alla nätverkskort. hello standardgateway för sekundära nätverkskort tas inte bort förrän dessa virtuella datorer startas om. I operativsystem som använder hello svaga värden routning modell, till exempel Linux kan Internetanslutning dela om hello ingående och utgående trafik använder olika nätverkskort.
> 

### <a name="configure-windows-vms"></a>Konfigurera virtuella Windows-datorer
Anta att du har en virtuell Windows-dator med två nätverkskort på följande sätt:

* Primär NIC IP-adress: 192.168.1.4
* Sekundär NIC IP-adress: 192.168.2.5

hello IPv4-routningstabellen för den här virtuella datorn skulle se ut så här:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Observera att hello standardvägen (0.0.0.0) är bara tillgängliga toohello primära nätverkskortet. Du kommer inte att kunna tooaccess resurser utanför hello undernät för hello sekundära NIC, enligt nedan:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

tooadd standard dirigera på hello sekundära NIC, följ hello stegen nedan:

1. Från Kommandotolken kör hello-kommandot nedan tooidentify hello Indextalet för hello sekundära NIC:
   
        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================
2. Observera hello andra post hello tabell med ett index över 27 (i det här exemplet).
3. Hello-Kommandotolken kör hello **väg lägga till** som visas nedan. I det här exemplet anger du 192.168.2.1 som hello standardgateway för hello sekundära NIC:
   
        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27
4. anslutning till tootest, gå tillbaka toohello Kommandotolken och försök tooping ett annat undernät från hello sekundära NIC som visas int FT exemplet nedan:
   
        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5
   
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
5. Du kan också kontrollera din väg tabell toocheck hello nyligen tillagda flödet, enligt nedan:
   
        C:\Users\Administrator>route print
   
        ...
   
        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Konfigurera virtuella Linux-datorer
För Linux virtuella datorer, eftersom hello standardbeteendet använder svaga värden routning, rekommenderar vi att hello sekundära nätverkskort begränsade tootraffic flöden endast inom hello samma undernät. Men om vissa scenarier kräver anslutningar utanför hello undernät, användare bör aktivera principbaserad routning tooensure som hello ingång och utgång trafik använder hello samma nätverkskort.

## <a name="next-steps"></a>Nästa steg
* Distribuera [MultiNIC virtuella datorer i ett scenario med 2-nivåprogram i en Resource Manager distribution](virtual-network-deploy-multinic-arm-template.md).
* Distribuera [MultiNIC virtuella datorer i ett scenario 2-nivåprogram klassisk distribution](virtual-network-deploy-multinic-classic-ps.md).

