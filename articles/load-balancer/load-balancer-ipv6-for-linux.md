---
title: "aaaConfiguring DHCPv6 för virtuella Linux-datorer | Microsoft Docs"
description: "Hur tooconfigure DHCPv6 för Linux virtuella datorer."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
keywords: "IPv6, azure belastningsutjämnare, dual stack, offentlig IP-adress, inbyggd ipv6, mobil, iot"
ms.assetid: b32719b6-00e8-4cd0-ba7f-e60e8146084b
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: kumud
ms.openlocfilehash: abd5a98c3496b189946f59bab1d9c20dcd0aa2c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-dhcpv6-for-linux-vms"></a>Konfigurera DHCPv6 för virtuella Linux-datorer

Vissa av hello Linux virtuella bilder i hello Azure Marketplace har inte DHCPv6 konfigureras som standard. toosupport IPv6, DHCPv6 måste vara konfigurerad i hello Linux OS-distribution som du använder. Olika Linux-distributioner har olika sätt att konfigurera DHCPv6 eftersom de använder olika paket.

> [!NOTE]
> Senaste SUSE Linux och virtuell CoreOS-avbildningar i hello Azure Marketplace har redan konfigurerats med DHCPv6. Inga ytterligare ändringar krävs när du använder dessa bilder.

Det här dokumentet beskriver hur tooenable DHCPv6 så att den virtuella Linux-datorn hämtar en IPv6-adress.

> [!WARNING]
> Felaktigt redigering network configuration-filer kan det leda till att du toolose network access tooyour VM. Vi rekommenderar att du testar din konfigurationsändringar på datorer i icke-produktionsmiljö. hello instruktionerna i den här artikeln har testats på hello senaste versionerna av hello Linux bilder i hello Azure Marketplace. Hello dokumentationen för din specifika version av Linux mer detaljerad information.

## <a name="ubuntu"></a>Ubuntu

1. Redigera filen hello `/etc/dhcp/dhclient6.conf` och Lägg till följande rad hello:

        timeout 10;

2. Redigera hello nätverkskonfigurationen för hello eth0 gränssnitt med hello följande konfiguration:

   * På **Ubuntu 12.04 och 14.04**, redigera hello-filen`/etc/network/interfaces.d/eth0.cfg`
   * På **Ubuntu 16.04**, redigera hello-filen`/etc/network/interfaces.d/50-cloud-init.cfg`

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Förnya IPv6-adress:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Redigera filen hello `/etc/dhcp/dhclient6.conf` och Lägg till följande rad hello:

        timeout 10;

2. Redigera filen hello `/etc/network/interfaces` och Lägg till hello följande konfiguration:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Förnya IPv6-adress:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Redigera filen hello `/etc/sysconfig/network` och Lägg till hello följande parameter:

        NETWORKING_IPV6=yes

2. Redigera filen hello `/etc/sysconfig/network-scripts/ifcfg-eth0` och Lägg till hello följande två parametrar:

        IPV6INIT=yes
        DHCPV6C=yes

3. Förnya IPv6-adress:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Senaste SLES och openSUSE avbildningar i Azure har redan konfigurerats med DHCPv6. Inga ytterligare ändringar krävs när du använder dessa bilder. Om du har en virtuell dator baserat på en äldre eller anpassade SUSE avbildning, använder du hello följande steg:

1. Installera hello `dhcp-client` paketet, om det behövs:

    ```bash
    sudo zypper install dhcp-client
    ```

2. Redigera filen hello `/etc/sysconfig/network/ifcfg-eth0` och Lägg till hello följande parameter:

        DHCLIENT6_MODE='managed'

3. Förnya hello IPv6-adress:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 och openSUSE Leap

Senaste SLES och openSUSE avbildningar i Azure har redan konfigurerats med DHCPv6. Inga ytterligare ändringar krävs när du använder dessa bilder. Om du har en virtuell dator baserat på en äldre eller anpassade SUSE avbildning, använder du hello följande steg:

1. Redigera filen hello `/etc/sysconfig/network/ifcfg-eth0` och ersätter den här parametern

        #BOOTPROTO='dhcp4'

    med hello följande värde:

        BOOTPROTO='dhcp'

2. Lägg till följande parameter för hello`/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Förnya hello IPv6-adress:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Senaste virtuell CoreOS-avbildningar i Azure har redan konfigurerats med DHCPv6. Inga ytterligare ändringar krävs när du använder dessa bilder. Om du har en virtuell dator baserat på en äldre eller anpassade virtuell CoreOS-avbildning, använder du hello följande steg:

1. Redigera hello-filen`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Förnya hello IPv6-adress:

    ```bash
    sudo systemctl restart systemd-networkd
    ```
