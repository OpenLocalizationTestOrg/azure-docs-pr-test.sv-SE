---
title: "Konfigurera DHCPv6 för virtuella Linux-datorer | Microsoft Docs"
description: "Hur du konfigurerar DHCPv6 för virtuella Linux-datorer."
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
ms.openlocfilehash: 5c591e7f1838c86ca74caea9dd3a5e8f874fd8a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-dhcpv6-for-linux-vms"></a><span data-ttu-id="e3ee0-104">Konfigurera DHCPv6 för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="e3ee0-104">Configuring DHCPv6 for Linux VMs</span></span>

<span data-ttu-id="e3ee0-105">Vissa av Linux virtuella avbildningar i Azure Marketplace har inte DHCPv6 konfigureras som standard.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-105">Some of the Linux virtual machine images in the Azure Marketplace do not have DHCPv6 configured by default.</span></span> <span data-ttu-id="e3ee0-106">För att stödja IPv6 konfigureras DHCPv6 i inom den Linux OS-distribution som du använder.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-106">To support IPv6, DHCPv6 must be configured in within the Linux OS distribution that you are using.</span></span> <span data-ttu-id="e3ee0-107">Olika Linux-distributioner har olika sätt att konfigurera DHCPv6 eftersom de använder olika paket.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-107">Different Linux distributions have different ways of configuring DHCPv6 because they use different packages.</span></span>

> [!NOTE]
> <span data-ttu-id="e3ee0-108">Senaste SUSE Linux och virtuell CoreOS-avbildningar i Azure Marketplace har redan konfigurerats med DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-108">Recent SUSE Linux and CoreOS images in the Azure Marketplace have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="e3ee0-109">Inga ytterligare ändringar krävs när du använder dessa bilder.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-109">No additional changes are required when using those images.</span></span>

<span data-ttu-id="e3ee0-110">Det här dokumentet beskriver hur du aktiverar DHCPv6 så att den virtuella Linux-datorn hämtar en IPv6-adress.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-110">This document describes how to enable DHCPv6 so that your Linux virtual machine obtains an IPv6 address.</span></span>

> [!WARNING]
> <span data-ttu-id="e3ee0-111">Felaktigt redigera konfigurationsfiler för nätverket kan medföra att du förlorar åtkomst till nätverket till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-111">Improperly editing network configuration files can cause you to lose network access to your VM.</span></span> <span data-ttu-id="e3ee0-112">Vi rekommenderar att du testar din konfigurationsändringar på datorer i icke-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-112">We recommended that you test your configuration changes on non-production systems.</span></span> <span data-ttu-id="e3ee0-113">Anvisningarna i den här artikeln har testats på de senaste versionerna av Linux-avbildningar i Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-113">The instructions in this article have been tested on the latest versions of the Linux images in the Azure Marketplace.</span></span> <span data-ttu-id="e3ee0-114">I dokumentationen för din specifika version av Linux mer detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-114">Consult the documentation for your specific version of Linux for more detailed instructions.</span></span>

## <a name="ubuntu"></a><span data-ttu-id="e3ee0-115">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="e3ee0-115">Ubuntu</span></span>

1. <span data-ttu-id="e3ee0-116">Redigera filen `/etc/dhcp/dhclient6.conf` och Lägg till följande rad:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-116">Edit the file `/etc/dhcp/dhclient6.conf` and add the following line:</span></span>

        timeout 10;

2. <span data-ttu-id="e3ee0-117">Redigera nätverkskonfigurationen för eth0-gränssnittet med följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-117">Edit the network configuration for the eth0 interface with the following configuration:</span></span>

   * <span data-ttu-id="e3ee0-118">På **Ubuntu 12.04 och 14.04**, redigerar du filen`/etc/network/interfaces.d/eth0.cfg`</span><span class="sxs-lookup"><span data-stu-id="e3ee0-118">On **Ubuntu 12.04 and 14.04**, edit the file `/etc/network/interfaces.d/eth0.cfg`</span></span>
   * <span data-ttu-id="e3ee0-119">På **Ubuntu 16.04**, redigerar du filen`/etc/network/interfaces.d/50-cloud-init.cfg`</span><span class="sxs-lookup"><span data-stu-id="e3ee0-119">On **Ubuntu 16.04**, edit the file `/etc/network/interfaces.d/50-cloud-init.cfg`</span></span>

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. <span data-ttu-id="e3ee0-120">Förnya IPv6-adress:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-120">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a><span data-ttu-id="e3ee0-121">Debian</span><span class="sxs-lookup"><span data-stu-id="e3ee0-121">Debian</span></span>

1. <span data-ttu-id="e3ee0-122">Redigera filen `/etc/dhcp/dhclient6.conf` och Lägg till följande rad:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-122">Edit the file `/etc/dhcp/dhclient6.conf` and add the following line:</span></span>

        timeout 10;

2. <span data-ttu-id="e3ee0-123">Redigera filen `/etc/network/interfaces` och Lägg till följande konfiguration:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-123">Edit the file `/etc/network/interfaces` and add the following configuration:</span></span>

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. <span data-ttu-id="e3ee0-124">Förnya IPv6-adress:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-124">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a><span data-ttu-id="e3ee0-125">RHEL / CentOS / Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="e3ee0-125">RHEL / CentOS / Oracle Linux</span></span>

1. <span data-ttu-id="e3ee0-126">Redigera filen `/etc/sysconfig/network` och Lägg till följande parameter:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-126">Edit the file `/etc/sysconfig/network` and add the following parameter:</span></span>

        NETWORKING_IPV6=yes

2. <span data-ttu-id="e3ee0-127">Redigera filen `/etc/sysconfig/network-scripts/ifcfg-eth0` och Lägg till följande två parametrar:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-127">Edit the file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add the following two parameters:</span></span>

        IPV6INIT=yes
        DHCPV6C=yes

3. <span data-ttu-id="e3ee0-128">Förnya IPv6-adress:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-128">Renew IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a><span data-ttu-id="e3ee0-129">SLES 11 & openSUSE 13</span><span class="sxs-lookup"><span data-stu-id="e3ee0-129">SLES 11 & openSUSE 13</span></span>

<span data-ttu-id="e3ee0-130">Senaste SLES och openSUSE avbildningar i Azure har redan konfigurerats med DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-130">Recent SLES and openSUSE images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="e3ee0-131">Inga ytterligare ändringar krävs när du använder dessa bilder.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-131">No additional changes are required when using those images.</span></span> <span data-ttu-id="e3ee0-132">Om du har en virtuell dator baserat på en äldre eller anpassade SUSE avbildning, använder du följande steg:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-132">If you have a VM based on an older or custom SUSE image, use the following steps:</span></span>

1. <span data-ttu-id="e3ee0-133">Installera den `dhcp-client` paketet, om det behövs:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-133">Install the `dhcp-client` package, if needed:</span></span>

    ```bash
    sudo zypper install dhcp-client
    ```

2. <span data-ttu-id="e3ee0-134">Redigera filen `/etc/sysconfig/network/ifcfg-eth0` och Lägg till följande parameter:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-134">Edit the file `/etc/sysconfig/network/ifcfg-eth0` and add the following parameter:</span></span>

        DHCLIENT6_MODE='managed'

3. <span data-ttu-id="e3ee0-135">Förnya IPv6-adress:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-135">Renew the IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a><span data-ttu-id="e3ee0-136">SLES 12 och openSUSE Leap</span><span class="sxs-lookup"><span data-stu-id="e3ee0-136">SLES 12 and openSUSE Leap</span></span>

<span data-ttu-id="e3ee0-137">Senaste SLES och openSUSE avbildningar i Azure har redan konfigurerats med DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-137">Recent SLES and openSUSE images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="e3ee0-138">Inga ytterligare ändringar krävs när du använder dessa bilder.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-138">No additional changes are required when using those images.</span></span> <span data-ttu-id="e3ee0-139">Om du har en virtuell dator baserat på en äldre eller anpassade SUSE avbildning, använder du följande steg:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-139">If you have a VM based on an older or custom SUSE image, use the following steps:</span></span>

1. <span data-ttu-id="e3ee0-140">Redigera filen `/etc/sysconfig/network/ifcfg-eth0` och ersätter den här parametern</span><span class="sxs-lookup"><span data-stu-id="e3ee0-140">Edit the file `/etc/sysconfig/network/ifcfg-eth0` and replace this parameter</span></span>

        #BOOTPROTO='dhcp4'

    <span data-ttu-id="e3ee0-141">med följande värde:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-141">with the following value:</span></span>

        BOOTPROTO='dhcp'

2. <span data-ttu-id="e3ee0-142">Lägg till följande parameter till `/etc/sysconfig/network/ifcfg-eth0`:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-142">Add the following parameter to `/etc/sysconfig/network/ifcfg-eth0`:</span></span>

        DHCLIENT6_MODE='managed'

3. <span data-ttu-id="e3ee0-143">Förnya IPv6-adress:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-143">Renew the IPv6 address:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a><span data-ttu-id="e3ee0-144">CoreOS</span><span class="sxs-lookup"><span data-stu-id="e3ee0-144">CoreOS</span></span>

<span data-ttu-id="e3ee0-145">Senaste virtuell CoreOS-avbildningar i Azure har redan konfigurerats med DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-145">Recent CoreOS images in Azure have been pre-configured with DHCPv6.</span></span> <span data-ttu-id="e3ee0-146">Inga ytterligare ändringar krävs när du använder dessa bilder.</span><span class="sxs-lookup"><span data-stu-id="e3ee0-146">No additional changes are required when using those images.</span></span> <span data-ttu-id="e3ee0-147">Om du har en virtuell dator baserat på en äldre eller anpassade virtuell CoreOS-avbildning, använder du följande steg:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-147">If you have a VM based on an older or custom CoreOS image, use the following steps:</span></span>

1. <span data-ttu-id="e3ee0-148">Redigera filen`/etc/systemd/network/10_dhcp.network`</span><span class="sxs-lookup"><span data-stu-id="e3ee0-148">Edit the file `/etc/systemd/network/10_dhcp.network`</span></span>

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. <span data-ttu-id="e3ee0-149">Förnya IPv6-adress:</span><span class="sxs-lookup"><span data-stu-id="e3ee0-149">Renew the IPv6 address:</span></span>

    ```bash
    sudo systemctl restart systemd-networkd
    ```
