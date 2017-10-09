---
title: "Dataflödet i aaaOptimize VM-nätverket | Microsoft Docs"
description: "Lär dig hur toooptimize Azure virtuella nätverk genomflöde."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: steveesp
ms.openlocfilehash: a5cff2d0ab6e3553c3f90d99629521a431477de0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Optimera dataflödet i nätverket för virtuella Azure-datorer

Virtuella Azure-datorer (VM) har standardinställningar för nätverket som kan optimeras ytterligare för dataflödet i nätverket. Den här artikeln beskriver hur toooptimize nätverk dataflöde för Microsoft Azure Windows och Linux virtuella datorer, inklusive större distributioner, till exempel Ubuntu och CentOS Red Hat.

## <a name="windows-vm"></a>Windows VM

Om din Windows-VM stöds med [snabbare nätverk](virtual-network-create-vm-accelerated-networking.md), aktivera funktionen skulle vara hello bästa konfigurationen för genomströmning. För alla andra virtuella Windows-datorer, skalning på mottagarsidan (RSS) kan du nå ut högre maximal genomströmning än en virtuell dator utan RSS. RSS kan inaktiveras som standard i en virtuell Windows-dator. Slutföra hello följande steg toodetermine om RSS är aktiverat och tooenable den om den är inaktiverad.

1. Ange hello `Get-NetAdapterRss` PowerShell-kommandot toosee om RSS är aktiverat för ett nätverkskort. I hello följande exempel på utdata som returnerades från hello `Get-NetAdapterRss`, RSS har inte aktiverats.

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : False
    ```
2. Ange följande kommando tooenable RSS hello:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    hello föregående kommando saknar utdata. hello kommandot Ändra inställningarna för nätverkskortet, orsaka förlust av tillfälliga anslutning för ungefär en minut. En Reconnecting dialogruta under hello anslutningen inte bryts. Anslutningen återställs vanligtvis efter hello tredje försök.
3. Bekräfta att RSS är aktiverat i hello VM genom att ange hello `Get-NetAdapterRss` kommandot igen. Om det lyckas, returneras hello följande exempel på utdata:

    ```powershell
    Name                    :Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a>Linux VM

RSS är alltid aktiverat som standard i en Azure Linux-dator. Linux-kernel som getts ut sedan januari 2017 inkluderar nya optimering Nätverksalternativ som möjliggör en Linux VM tooachieve högre dataflödet i nätverket.

### <a name="ubuntu"></a>Ubuntu

Uppdatera toohello stöds senaste versionen från och med juni 2017, vilket är först i ordning tooget hello optimering:
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
Ange hello följande kommandon tooget hello senaste kernel när hello uppdateringen är klar:

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

Valfritt kommando:

`apt-get -y dist-upgrade`
#### <a name="ubuntu-azure-preview-kernel"></a>Ubuntu Azure Preview kernel
> [!WARNING]
> Den här Azure Linux förhandsgranskningen kernel inte kan ha hello samma nivå av tillgänglighet och tillförlitlighet som Marketplace-bilder och kernlar som är i allmänhet tillgänglighet versionen. hello-funktionen stöds inte, kan ha begränsad kapacitet och får inte vara lika tillförlitlig som hello standardkernel. Använd inte den här kernel för produktionsarbetsbelastningar.

Betydande kapaciteten kan uppnås genom att installera hello föreslagna Azure Linux-kernel. tootry kernel, lägga till den här raden too/etc/apt/sources.list

```bash
#add this toohello end of /etc/apt/sources.list (requires elevation)
deb http://archive.ubuntu.com/ubuntu/ xenial-proposed restricted main multiverse universe
```

Kör sedan följande kommandon som rot.
```bash
apt-get update
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

Uppdatera toohello stöds senaste versionen från och med juli 2017, vilket är först i ordning tooget hello optimering:
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.3",
"Version": "latest"
```
När hello uppdateringen är klar hello installera senaste Linux Integration Services (LIS).
Hej genomflödesoptimeringsjobb är i LIS, från 4.2.2-2. Ange följande kommandon tooinstall LIS hello:

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

Uppdatera toohello stöds senaste versionen från och med juli 2017, vilket är först i ordning tooget hello optimering:
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7.3"
"Version": "7.3.2017071923"
```
När hello uppdateringen är klar hello installera senaste Linux Integration Services (LIS).
Hej genomflödesoptimeringsjobb är i LIS, från 4.2. Ange följande kommandon toodownload hello och installera LIS:

```bash
mkdir lis4.2.2-2
cd lis4.2.2-2
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
tar xvzf lis-rpms-4.2.2-2.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

Mer information om Linux Integration Services Version 4.2 för Hyper-V genom att visa hello [hämtningssidan](https://www.microsoft.com/download/details.aspx?id=55106).

## <a name="next-steps"></a>Nästa steg
* Nu när hello VM optimeras, se hello resultat med [bandbredd/dataflöde testa Azure VM](virtual-network-bandwidth-testing.md) för ditt scenario.
* Lär dig mer i [Azure Virtual Network vanliga frågor (FAQ)](virtual-networks-faq.md)
