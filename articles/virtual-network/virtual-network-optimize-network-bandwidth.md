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
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a><span data-ttu-id="a3a65-103">Optimera dataflödet i nätverket för virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="a3a65-103">Optimize network throughput for Azure virtual machines</span></span>

<span data-ttu-id="a3a65-104">Virtuella Azure-datorer (VM) har standardinställningar för nätverket som kan optimeras ytterligare för dataflödet i nätverket.</span><span class="sxs-lookup"><span data-stu-id="a3a65-104">Azure virtual machines (VM) have default network settings that can be further optimized for network throughput.</span></span> <span data-ttu-id="a3a65-105">Den här artikeln beskriver hur toooptimize nätverk dataflöde för Microsoft Azure Windows och Linux virtuella datorer, inklusive större distributioner, till exempel Ubuntu och CentOS Red Hat.</span><span class="sxs-lookup"><span data-stu-id="a3a65-105">This article describes how toooptimize network throughput for Microsoft Azure Windows and Linux VMs, including major distributions such as Ubuntu, CentOS and Red Hat.</span></span>

## <a name="windows-vm"></a><span data-ttu-id="a3a65-106">Windows VM</span><span class="sxs-lookup"><span data-stu-id="a3a65-106">Windows VM</span></span>

<span data-ttu-id="a3a65-107">Om din Windows-VM stöds med [snabbare nätverk](virtual-network-create-vm-accelerated-networking.md), aktivera funktionen skulle vara hello bästa konfigurationen för genomströmning.</span><span class="sxs-lookup"><span data-stu-id="a3a65-107">If your Windows VM is supported with [Accelerated Networking](virtual-network-create-vm-accelerated-networking.md), enabling that feature would be hello optimal configuration for throughput.</span></span> <span data-ttu-id="a3a65-108">För alla andra virtuella Windows-datorer, skalning på mottagarsidan (RSS) kan du nå ut högre maximal genomströmning än en virtuell dator utan RSS.</span><span class="sxs-lookup"><span data-stu-id="a3a65-108">For all other Windows VMs, using Receive Side Scaling (RSS) can reach higher maximal throughput than a VM without RSS.</span></span> <span data-ttu-id="a3a65-109">RSS kan inaktiveras som standard i en virtuell Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="a3a65-109">RSS may be disabled by default in a Windows VM.</span></span> <span data-ttu-id="a3a65-110">Slutföra hello följande steg toodetermine om RSS är aktiverat och tooenable den om den är inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="a3a65-110">Complete hello following steps toodetermine whether RSS is enabled and tooenable it if it's disabled.</span></span>

1. <span data-ttu-id="a3a65-111">Ange hello `Get-NetAdapterRss` PowerShell-kommandot toosee om RSS är aktiverat för ett nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="a3a65-111">Enter hello `Get-NetAdapterRss` PowerShell command toosee if RSS is enabled for a network adapter.</span></span> <span data-ttu-id="a3a65-112">I hello följande exempel på utdata som returnerades från hello `Get-NetAdapterRss`, RSS har inte aktiverats.</span><span class="sxs-lookup"><span data-stu-id="a3a65-112">In hello following example output returned from hello `Get-NetAdapterRss`, RSS is not enabled.</span></span>

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : False
    ```
2. <span data-ttu-id="a3a65-113">Ange följande kommando tooenable RSS hello:</span><span class="sxs-lookup"><span data-stu-id="a3a65-113">Enter hello following command tooenable RSS:</span></span>

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    <span data-ttu-id="a3a65-114">hello föregående kommando saknar utdata.</span><span class="sxs-lookup"><span data-stu-id="a3a65-114">hello previous command does not have an output.</span></span> <span data-ttu-id="a3a65-115">hello kommandot Ändra inställningarna för nätverkskortet, orsaka förlust av tillfälliga anslutning för ungefär en minut.</span><span class="sxs-lookup"><span data-stu-id="a3a65-115">hello command changed NIC settings, causing temporary connectivity loss for about one minute.</span></span> <span data-ttu-id="a3a65-116">En Reconnecting dialogruta under hello anslutningen inte bryts.</span><span class="sxs-lookup"><span data-stu-id="a3a65-116">A Reconnecting dialog box appears during hello connectivity loss.</span></span> <span data-ttu-id="a3a65-117">Anslutningen återställs vanligtvis efter hello tredje försök.</span><span class="sxs-lookup"><span data-stu-id="a3a65-117">Connectivity is typically restored after hello third attempt.</span></span>
3. <span data-ttu-id="a3a65-118">Bekräfta att RSS är aktiverat i hello VM genom att ange hello `Get-NetAdapterRss` kommandot igen.</span><span class="sxs-lookup"><span data-stu-id="a3a65-118">Confirm that RSS is enabled in hello VM by entering hello `Get-NetAdapterRss` command again.</span></span> <span data-ttu-id="a3a65-119">Om det lyckas, returneras hello följande exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="a3a65-119">If successful, hello following example output is returned:</span></span>

    ```powershell
    Name                    :Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a><span data-ttu-id="a3a65-120">Linux VM</span><span class="sxs-lookup"><span data-stu-id="a3a65-120">Linux VM</span></span>

<span data-ttu-id="a3a65-121">RSS är alltid aktiverat som standard i en Azure Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="a3a65-121">RSS is always enabled by default in an Azure Linux VM.</span></span> <span data-ttu-id="a3a65-122">Linux-kernel som getts ut sedan januari 2017 inkluderar nya optimering Nätverksalternativ som möjliggör en Linux VM tooachieve högre dataflödet i nätverket.</span><span class="sxs-lookup"><span data-stu-id="a3a65-122">Linux kernels released since January, 2017 include new network optimization options that enable a Linux VM tooachieve higher network throughput.</span></span>

### <a name="ubuntu"></a><span data-ttu-id="a3a65-123">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="a3a65-123">Ubuntu</span></span>

<span data-ttu-id="a3a65-124">Uppdatera toohello stöds senaste versionen från och med juni 2017, vilket är först i ordning tooget hello optimering:</span><span class="sxs-lookup"><span data-stu-id="a3a65-124">In order tooget hello optimization, first update toohello latest supported version, as of June 2017, which is:</span></span>
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
<span data-ttu-id="a3a65-125">Ange hello följande kommandon tooget hello senaste kernel när hello uppdateringen är klar:</span><span class="sxs-lookup"><span data-stu-id="a3a65-125">After hello update is complete, enter hello following commands tooget hello latest kernel:</span></span>

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

<span data-ttu-id="a3a65-126">Valfritt kommando:</span><span class="sxs-lookup"><span data-stu-id="a3a65-126">Optional command:</span></span>

`apt-get -y dist-upgrade`
#### <a name="ubuntu-azure-preview-kernel"></a><span data-ttu-id="a3a65-127">Ubuntu Azure Preview kernel</span><span class="sxs-lookup"><span data-stu-id="a3a65-127">Ubuntu Azure Preview kernel</span></span>
> [!WARNING]
> <span data-ttu-id="a3a65-128">Den här Azure Linux förhandsgranskningen kernel inte kan ha hello samma nivå av tillgänglighet och tillförlitlighet som Marketplace-bilder och kernlar som är i allmänhet tillgänglighet versionen.</span><span class="sxs-lookup"><span data-stu-id="a3a65-128">This Azure Linux Preview kernel may not have hello same level of availability and reliability as Marketplace images and kernels that are in general availability release.</span></span> <span data-ttu-id="a3a65-129">hello-funktionen stöds inte, kan ha begränsad kapacitet och får inte vara lika tillförlitlig som hello standardkernel.</span><span class="sxs-lookup"><span data-stu-id="a3a65-129">hello feature is not supported, may have constrained capabilities, and may not be as reliable as hello default kernel.</span></span> <span data-ttu-id="a3a65-130">Använd inte den här kernel för produktionsarbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="a3a65-130">Do not use this kernel for production workloads.</span></span>

<span data-ttu-id="a3a65-131">Betydande kapaciteten kan uppnås genom att installera hello föreslagna Azure Linux-kernel.</span><span class="sxs-lookup"><span data-stu-id="a3a65-131">Significant throughput performance can be achieved by installing hello proposed Azure Linux kernel.</span></span> <span data-ttu-id="a3a65-132">tootry kernel, lägga till den här raden too/etc/apt/sources.list</span><span class="sxs-lookup"><span data-stu-id="a3a65-132">tootry this kernel, add this line too/etc/apt/sources.list</span></span>

```bash
#add this toohello end of /etc/apt/sources.list (requires elevation)
deb http://archive.ubuntu.com/ubuntu/ xenial-proposed restricted main multiverse universe
```

<span data-ttu-id="a3a65-133">Kör sedan följande kommandon som rot.</span><span class="sxs-lookup"><span data-stu-id="a3a65-133">Then run these commands as root.</span></span>
```bash
apt-get update
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a><span data-ttu-id="a3a65-134">CentOS</span><span class="sxs-lookup"><span data-stu-id="a3a65-134">CentOS</span></span>

<span data-ttu-id="a3a65-135">Uppdatera toohello stöds senaste versionen från och med juli 2017, vilket är först i ordning tooget hello optimering:</span><span class="sxs-lookup"><span data-stu-id="a3a65-135">In order tooget hello optimization, first update toohello latest supported version, as of July 2017, which is:</span></span>
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.3",
"Version": "latest"
```
<span data-ttu-id="a3a65-136">När hello uppdateringen är klar hello installera senaste Linux Integration Services (LIS).</span><span class="sxs-lookup"><span data-stu-id="a3a65-136">After hello update is complete, install hello latest Linux Integration Services (LIS).</span></span>
<span data-ttu-id="a3a65-137">Hej genomflödesoptimeringsjobb är i LIS, från 4.2.2-2.</span><span class="sxs-lookup"><span data-stu-id="a3a65-137">hello throughput optimization is in LIS, starting from 4.2.2-2.</span></span> <span data-ttu-id="a3a65-138">Ange följande kommandon tooinstall LIS hello:</span><span class="sxs-lookup"><span data-stu-id="a3a65-138">Enter hello following commands tooinstall LIS:</span></span>

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a><span data-ttu-id="a3a65-139">Red Hat</span><span class="sxs-lookup"><span data-stu-id="a3a65-139">Red Hat</span></span>

<span data-ttu-id="a3a65-140">Uppdatera toohello stöds senaste versionen från och med juli 2017, vilket är först i ordning tooget hello optimering:</span><span class="sxs-lookup"><span data-stu-id="a3a65-140">In order tooget hello optimization, first update toohello latest supported version, as of July 2017, which is:</span></span>
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7.3"
"Version": "7.3.2017071923"
```
<span data-ttu-id="a3a65-141">När hello uppdateringen är klar hello installera senaste Linux Integration Services (LIS).</span><span class="sxs-lookup"><span data-stu-id="a3a65-141">After hello update is complete, install hello latest Linux Integration Services (LIS).</span></span>
<span data-ttu-id="a3a65-142">Hej genomflödesoptimeringsjobb är i LIS, från 4.2.</span><span class="sxs-lookup"><span data-stu-id="a3a65-142">hello throughput optimization is in LIS, starting from 4.2.</span></span> <span data-ttu-id="a3a65-143">Ange följande kommandon toodownload hello och installera LIS:</span><span class="sxs-lookup"><span data-stu-id="a3a65-143">Enter hello following commands toodownload and install LIS:</span></span>

```bash
mkdir lis4.2.2-2
cd lis4.2.2-2
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
tar xvzf lis-rpms-4.2.2-2.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

<span data-ttu-id="a3a65-144">Mer information om Linux Integration Services Version 4.2 för Hyper-V genom att visa hello [hämtningssidan](https://www.microsoft.com/download/details.aspx?id=55106).</span><span class="sxs-lookup"><span data-stu-id="a3a65-144">Learn more about Linux Integration Services Version 4.2 for Hyper-V by viewing hello [download page](https://www.microsoft.com/download/details.aspx?id=55106).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3a65-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a3a65-145">Next steps</span></span>
* <span data-ttu-id="a3a65-146">Nu när hello VM optimeras, se hello resultat med [bandbredd/dataflöde testa Azure VM](virtual-network-bandwidth-testing.md) för ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="a3a65-146">Now that hello VM is optimized, see hello result with [Bandwidth/Throughput testing Azure VM](virtual-network-bandwidth-testing.md) for your scenario.</span></span>
* <span data-ttu-id="a3a65-147">Lär dig mer i [Azure Virtual Network vanliga frågor (FAQ)](virtual-networks-faq.md)</span><span class="sxs-lookup"><span data-stu-id="a3a65-147">Learn more with [Azure Virtual Network frequently asked questions (FAQ)](virtual-networks-faq.md)</span></span>
