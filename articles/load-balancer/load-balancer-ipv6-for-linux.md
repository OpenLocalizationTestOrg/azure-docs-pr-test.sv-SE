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
# <a name="configuring-dhcpv6-for-linux-vms"></a><span data-ttu-id="8e687-104">Konfigurera DHCPv6 för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="8e687-104">Configuring DHCPv6 for Linux VMs</span></span>

<span data-ttu-id="8e687-105">Vissa av hello Linux virtuella bilder i hello Azure Marketplace har inte DHCPv6 konfigureras som standard.</span><span class="sxs-lookup"><span data-stu-id="8e687-105">Some of hello Linux virtual machine images in hello Azure Marketplace do not have DHCPv6 configured by default.</span></span> <span data-ttu-id="8e687-106">toosupport IPv6, DHCPv6 måste vara konfigurerad i hello Linux OS-distribution som du använder.</span><span class="sxs-lookup"><span data-stu-id="8e687-106">toosupport IPv6, DHCPv6 must be configured in within hello Linux OS distribution that you are using.</span></span> <span data-ttu-id="8e687-107">Olika Linux-distributioner har olika sätt att konfigurera DHCPv6 eftersom de använder olika paket.</span><span class="sxs-lookup"><span data-stu-id="8e687-107">Different Linux distributions have different ways of configuring DHCPv6 because they use different packages.</span></span>

> [!NOTE]
> <span data-ttu-id="8e687-108">Senaste SUSE Linux och virtuell CoreOS-avbildningar i hello Azure Marketplace har redan konfigurerats med DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="8e687-108">Recent SUSE Linux and CoreOS images in hello Azure Marketplace have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="8e687-109">Inga ytterligare ändringar krävs när du använder dessa bilder.</span><span class="sxs-lookup"><span data-stu-id="8e687-109">No additional changes are required when using those images.</span></span>

<span data-ttu-id="8e687-110">Det här dokumentet beskriver hur tooenable DHCPv6 så att den virtuella Linux-datorn hämtar en IPv6-adress.</span><span class="sxs-lookup"><span data-stu-id="8e687-110">This document describes how tooenable DHCPv6 so that your Linux virtual machine obtains an IPv6 address.</span></span>

> [!WARNING]
> <span data-ttu-id="8e687-111">Felaktigt redigering network configuration-filer kan det leda till att du toolose network access tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="8e687-111">Improperly editing network configuration files can cause you toolose network access tooyour VM.</span></span> <span data-ttu-id="8e687-112">Vi rekommenderar att du testar din konfigurationsändringar på datorer i icke-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8e687-112">We recommended that you test your configuration changes on non-production systems.</span></span> <span data-ttu-id="8e687-113">hello instruktionerna i den här artikeln har testats på hello senaste versionerna av hello Linux bilder i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="8e687-113">hello instructions in this article have been tested on hello latest versions of hello Linux images in hello Azure Marketplace.</span></span> <span data-ttu-id="8e687-114">Hello dokumentationen för din specifika version av Linux mer detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="8e687-114">Consult hello documentation for your specific version of Linux for more detailed instructions.</span></span>

## <a name="ubuntu"></a><span data-ttu-id="8e687-115">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="8e687-115">Ubuntu</span></span>

1. <span data-ttu-id="8e687-116">Redigera filen hello `/etc/dhcp/dhclient6.conf` och Lägg till följande rad hello:</span><span class="sxs-lookup"><span data-stu-id="8e687-116">Edit hello file `/etc/dhcp/dhclient6.conf` and add hello following line:</span></span>

        timeout 10;

2. <span data-ttu-id="8e687-117">Redigera hello nätverkskonfigurationen för hello eth0 gränssnitt med hello följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="8e687-117">Edit hello network configuration for hello eth0 interface with hello following configuration:</span></span>

   * <span data-ttu-id="8e687-118">På **Ubuntu 12.04 och 14.04**, redigera hello-filen`/etc/network/interfaces.d/eth0.cfg`</span><span class="sxs-lookup"><span data-stu-id="8e687-118">On **Ubuntu 12.04 and 14.04**, edit hello file `/etc/network/interfaces.d/eth0.cfg`</span></span>
   * <span data-ttu-id="8e687-119">På **Ubuntu 16.04**, redigera hello-filen`/etc/network/interfaces.d/50-cloud-init.cfg`</span><span class="sxs-lookup"><span data-stu-id="8e687-119">On **Ubuntu 16.04**, edit hello file `/etc/network/interfaces.d/50-cloud-init.cfg`</span></span>

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. <span data-ttu-id="8e687-120">Förnya IPv6-adress:</span><span class="sxs-lookup"><span data-stu-id="8e687-120">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a><span data-ttu-id="8e687-121">Debian</span><span class="sxs-lookup"><span data-stu-id="8e687-121">Debian</span></span>

1. <span data-ttu-id="8e687-122">Redigera filen hello `/etc/dhcp/dhclient6.conf` och Lägg till följande rad hello:</span><span class="sxs-lookup"><span data-stu-id="8e687-122">Edit hello file `/etc/dhcp/dhclient6.conf` and add hello following line:</span></span>

        timeout 10;

2. <span data-ttu-id="8e687-123">Redigera filen hello `/etc/network/interfaces` och Lägg till hello följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="8e687-123">Edit hello file `/etc/network/interfaces` and add hello following configuration:</span></span>

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. <span data-ttu-id="8e687-124">Förnya IPv6-adress:</span><span class="sxs-lookup"><span data-stu-id="8e687-124">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a><span data-ttu-id="8e687-125">RHEL / CentOS / Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="8e687-125">RHEL / CentOS / Oracle Linux</span></span>

1. <span data-ttu-id="8e687-126">Redigera filen hello `/etc/sysconfig/network` och Lägg till hello följande parameter:</span><span class="sxs-lookup"><span data-stu-id="8e687-126">Edit hello file `/etc/sysconfig/network` and add hello following parameter:</span></span>

        NETWORKING_IPV6=yes

2. <span data-ttu-id="8e687-127">Redigera filen hello `/etc/sysconfig/network-scripts/ifcfg-eth0` och Lägg till hello följande två parametrar:</span><span class="sxs-lookup"><span data-stu-id="8e687-127">Edit hello file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add hello following two parameters:</span></span>

        IPV6INIT=yes
        DHCPV6C=yes

3. <span data-ttu-id="8e687-128">Förnya IPv6-adress:</span><span class="sxs-lookup"><span data-stu-id="8e687-128">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a><span data-ttu-id="8e687-129">SLES 11 & openSUSE 13</span><span class="sxs-lookup"><span data-stu-id="8e687-129">SLES 11 & openSUSE 13</span></span>

<span data-ttu-id="8e687-130">Senaste SLES och openSUSE avbildningar i Azure har redan konfigurerats med DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="8e687-130">Recent SLES and openSUSE images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="8e687-131">Inga ytterligare ändringar krävs när du använder dessa bilder.</span><span class="sxs-lookup"><span data-stu-id="8e687-131">No additional changes are required when using those images.</span></span> <span data-ttu-id="8e687-132">Om du har en virtuell dator baserat på en äldre eller anpassade SUSE avbildning, använder du hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8e687-132">If you have a VM based on an older or custom SUSE image, use hello following steps:</span></span>

1. <span data-ttu-id="8e687-133">Installera hello `dhcp-client` paketet, om det behövs:</span><span class="sxs-lookup"><span data-stu-id="8e687-133">Install hello `dhcp-client` package, if needed:</span></span>

    ```bash
    sudo zypper install dhcp-client
    ```

2. <span data-ttu-id="8e687-134">Redigera filen hello `/etc/sysconfig/network/ifcfg-eth0` och Lägg till hello följande parameter:</span><span class="sxs-lookup"><span data-stu-id="8e687-134">Edit hello file `/etc/sysconfig/network/ifcfg-eth0` and add hello following parameter:</span></span>

        DHCLIENT6_MODE='managed'

3. <span data-ttu-id="8e687-135">Förnya hello IPv6-adress:</span><span class="sxs-lookup"><span data-stu-id="8e687-135">Renew hello IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a><span data-ttu-id="8e687-136">SLES 12 och openSUSE Leap</span><span class="sxs-lookup"><span data-stu-id="8e687-136">SLES 12 and openSUSE Leap</span></span>

<span data-ttu-id="8e687-137">Senaste SLES och openSUSE avbildningar i Azure har redan konfigurerats med DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="8e687-137">Recent SLES and openSUSE images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="8e687-138">Inga ytterligare ändringar krävs när du använder dessa bilder.</span><span class="sxs-lookup"><span data-stu-id="8e687-138">No additional changes are required when using those images.</span></span> <span data-ttu-id="8e687-139">Om du har en virtuell dator baserat på en äldre eller anpassade SUSE avbildning, använder du hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8e687-139">If you have a VM based on an older or custom SUSE image, use hello following steps:</span></span>

1. <span data-ttu-id="8e687-140">Redigera filen hello `/etc/sysconfig/network/ifcfg-eth0` och ersätter den här parametern</span><span class="sxs-lookup"><span data-stu-id="8e687-140">Edit hello file `/etc/sysconfig/network/ifcfg-eth0` and replace this parameter</span></span>

        #BOOTPROTO='dhcp4'

    <span data-ttu-id="8e687-141">med hello följande värde:</span><span class="sxs-lookup"><span data-stu-id="8e687-141">with hello following value:</span></span>

        BOOTPROTO='dhcp'

2. <span data-ttu-id="8e687-142">Lägg till följande parameter för hello`/etc/sysconfig/network/ifcfg-eth0`:</span><span class="sxs-lookup"><span data-stu-id="8e687-142">Add hello following parameter too`/etc/sysconfig/network/ifcfg-eth0`:</span></span>

        DHCLIENT6_MODE='managed'

3. <span data-ttu-id="8e687-143">Förnya hello IPv6-adress:</span><span class="sxs-lookup"><span data-stu-id="8e687-143">Renew hello IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a><span data-ttu-id="8e687-144">CoreOS</span><span class="sxs-lookup"><span data-stu-id="8e687-144">CoreOS</span></span>

<span data-ttu-id="8e687-145">Senaste virtuell CoreOS-avbildningar i Azure har redan konfigurerats med DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="8e687-145">Recent CoreOS images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="8e687-146">Inga ytterligare ändringar krävs när du använder dessa bilder.</span><span class="sxs-lookup"><span data-stu-id="8e687-146">No additional changes are required when using those images.</span></span> <span data-ttu-id="8e687-147">Om du har en virtuell dator baserat på en äldre eller anpassade virtuell CoreOS-avbildning, använder du hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8e687-147">If you have a VM based on an older or custom CoreOS image, use hello following steps:</span></span>

1. <span data-ttu-id="8e687-148">Redigera hello-filen`/etc/systemd/network/10_dhcp.network`</span><span class="sxs-lookup"><span data-stu-id="8e687-148">Edit hello file `/etc/systemd/network/10_dhcp.network`</span></span>

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. <span data-ttu-id="8e687-149">Förnya hello IPv6-adress:</span><span class="sxs-lookup"><span data-stu-id="8e687-149">Renew hello IPv6 address:</span></span>

    ```bash
    sudo systemctl restart systemd-networkd
    ```
