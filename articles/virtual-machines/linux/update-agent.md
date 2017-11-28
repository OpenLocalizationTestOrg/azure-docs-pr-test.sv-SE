---
title: "aaaUpdate hello Azure Linux-agenten från GitHub | Microsoft Docs"
description: "Lär dig hur tooupdate Azure Linux-agenten för dina Linux VM i Azure toohello senaste versionen från GitHub"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: f1f19300-987d-4f29-9393-9aba866f049c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: mingzhan
ms.openlocfilehash: 4ce7c56efc1e6563e6415f7687573f9fb9e7b4c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-hello-azure-linux-agent-on-a-vm"></a><span data-ttu-id="89e01-103">Hur tooupdate hello Azure Linux-agenten på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="89e01-103">How tooupdate hello Azure Linux Agent on a VM</span></span>

<span data-ttu-id="89e01-104">tooupdate din [Azure Linux-agenten](https://github.com/Azure/WALinuxAgent) på en Linux-VM i Azure måste du ha:</span><span class="sxs-lookup"><span data-stu-id="89e01-104">tooupdate your [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) on a Linux VM in Azure, you must already have:</span></span>

- <span data-ttu-id="89e01-105">En körande Linux VM i Azure.</span><span class="sxs-lookup"><span data-stu-id="89e01-105">A running Linux VM in Azure.</span></span>
- <span data-ttu-id="89e01-106">En anslutning toothat Linux VM via SSH.</span><span class="sxs-lookup"><span data-stu-id="89e01-106">A connection toothat Linux VM using SSH.</span></span>

<span data-ttu-id="89e01-107">Du bör alltid kontrollera för ett paket i hello Linux distro databasen först.</span><span class="sxs-lookup"><span data-stu-id="89e01-107">You should always check for a package in hello Linux distro repository first.</span></span> <span data-ttu-id="89e01-108">Det är möjligt hello paketet finns kanske inte hello senaste versionen, men att aktivera automatisk uppdatering säkerställer hello Linux-Agent kommer alltid att hämta hello senaste uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="89e01-108">It is possible hello package available may not be hello latest version, however, enabling autoupdate will ensure hello Linux Agent will always get hello latest update.</span></span> <span data-ttu-id="89e01-109">Om du har problem med att installera från hello paketet chefer, bör du söka stöd från hello distro leverantör.</span><span class="sxs-lookup"><span data-stu-id="89e01-109">Should you have issues installing from hello package managers, you should seek support from hello distro vendor.</span></span>

## <a name="updating-hello-azure-linux-agent"></a><span data-ttu-id="89e01-110">Uppdatera hello Azure Linux-agenten</span><span class="sxs-lookup"><span data-stu-id="89e01-110">Updating hello Azure Linux Agent</span></span>

## <a name="ubuntu"></a><span data-ttu-id="89e01-111">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="89e01-111">Ubuntu</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="89e01-112">Kontrollera din nuvarande Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-112">Check your current package version</span></span>

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a><span data-ttu-id="89e01-113">Uppdatera paketcachen</span><span class="sxs-lookup"><span data-stu-id="89e01-113">Update package cache</span></span>

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="89e01-114">Installera hello senaste Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-114">Install hello latest package version</span></span>

```bash
sudo apt-get install walinuxagent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="89e01-115">Kontrollera att automatiska uppdateringar är aktiverat</span><span class="sxs-lookup"><span data-stu-id="89e01-115">Ensure auto update is enabled</span></span>

<span data-ttu-id="89e01-116">Kontrollera först toosee om det är aktiverat:</span><span class="sxs-lookup"><span data-stu-id="89e01-116">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="89e01-117">Hitta 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="89e01-117">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="89e01-118">Om du ser den här utdatan aktiveras:</span><span class="sxs-lookup"><span data-stu-id="89e01-118">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="89e01-119">tooenable kör:</span><span class="sxs-lookup"><span data-stu-id="89e01-119">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="89e01-120">Starta om hello waagent-tjänsten</span><span class="sxs-lookup"><span data-stu-id="89e01-120">Restart hello waagent service</span></span>

#### <a name="restart-agent-for-1404"></a><span data-ttu-id="89e01-121">Starta om agenten för 14.04</span><span class="sxs-lookup"><span data-stu-id="89e01-121">Restart agent for 14.04</span></span>

```bash
initctl restart walinuxagent
```

#### <a name="restart-agent-for-1604--1704"></a><span data-ttu-id="89e01-122">Starta om agenten för 16.04 / 17.04</span><span class="sxs-lookup"><span data-stu-id="89e01-122">Restart agent for 16.04 / 17.04</span></span>

```bash
systemctl restart walinuxagent.service
```

## <a name="debian"></a><span data-ttu-id="89e01-123">Debian</span><span class="sxs-lookup"><span data-stu-id="89e01-123">Debian</span></span>

### <a name="debian-7-wheezy"></a><span data-ttu-id="89e01-124">Debian 7 ”Wheezy”</span><span class="sxs-lookup"><span data-stu-id="89e01-124">Debian 7 “Wheezy”</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="89e01-125">Kontrollera din nuvarande Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-125">Check your current package version</span></span>

```bash
dpkg -l | grep waagent
```

#### <a name="update-package-cache"></a><span data-ttu-id="89e01-126">Uppdatera paketcachen</span><span class="sxs-lookup"><span data-stu-id="89e01-126">Update package cache</span></span>

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="89e01-127">Installera hello senaste Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-127">Install hello latest package version</span></span>

```bash
sudo apt-get install waagent
```

#### <a name="enable-agent-auto-update"></a><span data-ttu-id="89e01-128">Aktivera agenten för automatisk uppdatering</span><span class="sxs-lookup"><span data-stu-id="89e01-128">Enable agent auto update</span></span>
<span data-ttu-id="89e01-129">Den här versionen av Debian inte har en version > = 2.0.16, automatisk uppdatering är därför inte tillgänglig för den.</span><span class="sxs-lookup"><span data-stu-id="89e01-129">This version of Debian does not have a version >= 2.0.16, therefore AutoUpdate is not available for it.</span></span> <span data-ttu-id="89e01-130">hello utdata från hello ovanstående kommando visar om hello paketet är uppdaterad.</span><span class="sxs-lookup"><span data-stu-id="89e01-130">hello output from hello above command will show you if hello package is up-to-date.</span></span>

### <a name="debian-8-jessie--debian-9-stretch"></a><span data-ttu-id="89e01-131">Debian 8 ”Jessie” / Debian 9 ”Stretch”</span><span class="sxs-lookup"><span data-stu-id="89e01-131">Debian 8 “Jessie” / Debian 9 “Stretch”</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="89e01-132">Kontrollera din nuvarande Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-132">Check your current package version</span></span>

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a><span data-ttu-id="89e01-133">Uppdatera paketcachen</span><span class="sxs-lookup"><span data-stu-id="89e01-133">Update package cache</span></span>

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="89e01-134">Installera hello senaste Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-134">Install hello latest package version</span></span>

```bash
sudo apt-get install waagent
```
#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="89e01-135">Kontrollera att automatiska uppdateringar är aktiverat</span><span class="sxs-lookup"><span data-stu-id="89e01-135">Ensure auto update is enabled</span></span> 

<span data-ttu-id="89e01-136">Kontrollera först toosee om det är aktiverat:</span><span class="sxs-lookup"><span data-stu-id="89e01-136">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="89e01-137">Hitta 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="89e01-137">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="89e01-138">Om du ser den här utdatan aktiveras:</span><span class="sxs-lookup"><span data-stu-id="89e01-138">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="89e01-139">tooenable kör:</span><span class="sxs-lookup"><span data-stu-id="89e01-139">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="89e01-140">Starta om hello waagent-tjänsten</span><span class="sxs-lookup"><span data-stu-id="89e01-140">Restart hello waagent service</span></span>

```
sudo systemctl restart walinuxagent.service
```

## <a name="redhat--centos"></a><span data-ttu-id="89e01-141">Redhat / CentOS</span><span class="sxs-lookup"><span data-stu-id="89e01-141">Redhat / CentOS</span></span>

### <a name="rhelcentos-6"></a><span data-ttu-id="89e01-142">RHEL/CentOS 6</span><span class="sxs-lookup"><span data-stu-id="89e01-142">RHEL/CentOS 6</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="89e01-143">Kontrollera din nuvarande Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-143">Check your current package version</span></span>

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a><span data-ttu-id="89e01-144">Kontrollera tillgängliga uppdateringar</span><span class="sxs-lookup"><span data-stu-id="89e01-144">Check available updates</span></span>

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="89e01-145">Installera hello senaste Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-145">Install hello latest package version</span></span>

```bash
sudo yum install WALinuxAgent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="89e01-146">Kontrollera att automatiska uppdateringar är aktiverat</span><span class="sxs-lookup"><span data-stu-id="89e01-146">Ensure auto update is enabled</span></span> 

<span data-ttu-id="89e01-147">Kontrollera först toosee om det är aktiverat:</span><span class="sxs-lookup"><span data-stu-id="89e01-147">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="89e01-148">Hitta 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="89e01-148">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="89e01-149">Om du ser den här utdatan aktiveras:</span><span class="sxs-lookup"><span data-stu-id="89e01-149">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="89e01-150">tooenable kör:</span><span class="sxs-lookup"><span data-stu-id="89e01-150">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="89e01-151">Starta om hello waagent-tjänsten</span><span class="sxs-lookup"><span data-stu-id="89e01-151">Restart hello waagent service</span></span>

```
sudo service waagent restart
```

### <a name="rhelcentos-7"></a><span data-ttu-id="89e01-152">RHEL/CentOS 7</span><span class="sxs-lookup"><span data-stu-id="89e01-152">RHEL/CentOS 7</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="89e01-153">Kontrollera din nuvarande Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-153">Check your current package version</span></span>

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a><span data-ttu-id="89e01-154">Kontrollera tillgängliga uppdateringar</span><span class="sxs-lookup"><span data-stu-id="89e01-154">Check available updates</span></span>

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="89e01-155">Installera hello senaste Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-155">Install hello latest package version</span></span>

```bash
sudo yum install WALinuxAgent  
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="89e01-156">Kontrollera att automatiska uppdateringar är aktiverat</span><span class="sxs-lookup"><span data-stu-id="89e01-156">Ensure auto update is enabled</span></span> 

<span data-ttu-id="89e01-157">Kontrollera först toosee om det är aktiverat:</span><span class="sxs-lookup"><span data-stu-id="89e01-157">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="89e01-158">Hitta 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="89e01-158">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="89e01-159">Om du ser den här utdatan aktiveras:</span><span class="sxs-lookup"><span data-stu-id="89e01-159">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="89e01-160">tooenable kör:</span><span class="sxs-lookup"><span data-stu-id="89e01-160">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="89e01-161">Starta om hello waagent-tjänsten</span><span class="sxs-lookup"><span data-stu-id="89e01-161">Restart hello waagent service</span></span>

```bash
sudo systemctl restart waagent.service
```

## <a name="suse-sles"></a><span data-ttu-id="89e01-162">SUSE SLES</span><span class="sxs-lookup"><span data-stu-id="89e01-162">SUSE SLES</span></span>

### <a name="suse-sles-11-sp4"></a><span data-ttu-id="89e01-163">SUSE SLES 11 SP4</span><span class="sxs-lookup"><span data-stu-id="89e01-163">SUSE SLES 11 SP4</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="89e01-164">Kontrollera din nuvarande Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-164">Check your current package version</span></span>

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a><span data-ttu-id="89e01-165">Kontrollera tillgängliga uppdateringar</span><span class="sxs-lookup"><span data-stu-id="89e01-165">Check available updates</span></span>

<span data-ttu-id="89e01-166">hello ovan utdata visar om hello paketet är in toodate.</span><span class="sxs-lookup"><span data-stu-id="89e01-166">hello above output will show you if hello package is up toodate.</span></span>

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="89e01-167">Installera hello senaste Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-167">Install hello latest package version</span></span>

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="89e01-168">Kontrollera att automatiska uppdateringar är aktiverat</span><span class="sxs-lookup"><span data-stu-id="89e01-168">Ensure auto update is enabled</span></span> 

<span data-ttu-id="89e01-169">Kontrollera först toosee om det är aktiverat:</span><span class="sxs-lookup"><span data-stu-id="89e01-169">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="89e01-170">Hitta 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="89e01-170">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="89e01-171">Om du ser den här utdatan aktiveras:</span><span class="sxs-lookup"><span data-stu-id="89e01-171">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="89e01-172">tooenable kör:</span><span class="sxs-lookup"><span data-stu-id="89e01-172">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="89e01-173">Starta om hello waagent-tjänsten</span><span class="sxs-lookup"><span data-stu-id="89e01-173">Restart hello waagent service</span></span>

```bash
sudo /etc/init.d/waagent restart
```

### <a name="suse-sles-12-sp2"></a><span data-ttu-id="89e01-174">SUSE SLES 12 SP2</span><span class="sxs-lookup"><span data-stu-id="89e01-174">SUSE SLES 12 SP2</span></span>

#### <a name="check-your-current-package-version"></a><span data-ttu-id="89e01-175">Kontrollera din nuvarande Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-175">Check your current package version</span></span>

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a><span data-ttu-id="89e01-176">Kontrollera tillgängliga uppdateringar</span><span class="sxs-lookup"><span data-stu-id="89e01-176">Check available updates</span></span>

<span data-ttu-id="89e01-177">Hello utdata från hello ovan och visar detta om hello paketet är upp till datum.</span><span class="sxs-lookup"><span data-stu-id="89e01-177">In hello output from hello above, this will show you if hello package is upto date.</span></span>

#### <a name="install-hello-latest-package-version"></a><span data-ttu-id="89e01-178">Installera hello senaste Paketversion</span><span class="sxs-lookup"><span data-stu-id="89e01-178">Install hello latest package version</span></span>

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="89e01-179">Kontrollera att automatiska uppdateringar är aktiverat</span><span class="sxs-lookup"><span data-stu-id="89e01-179">Ensure auto update is enabled</span></span> 

<span data-ttu-id="89e01-180">Kontrollera först toosee om det är aktiverat:</span><span class="sxs-lookup"><span data-stu-id="89e01-180">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="89e01-181">Hitta 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="89e01-181">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="89e01-182">Om du ser den här utdatan aktiveras:</span><span class="sxs-lookup"><span data-stu-id="89e01-182">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="89e01-183">tooenable kör:</span><span class="sxs-lookup"><span data-stu-id="89e01-183">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a><span data-ttu-id="89e01-184">Starta om hello waagent-tjänsten</span><span class="sxs-lookup"><span data-stu-id="89e01-184">Restart hello waagent service</span></span>

```bash
sudo systemctl restart waagent.service
```

## <a name="oracle-6-and-7"></a><span data-ttu-id="89e01-185">Oracle 6 och 7</span><span class="sxs-lookup"><span data-stu-id="89e01-185">Oracle 6 and 7</span></span>

<span data-ttu-id="89e01-186">För Oracle Linux, kontrollera att hello `Addons` databasen är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="89e01-186">For Oracle Linux, make sure that hello `Addons` repository is enabled.</span></span> <span data-ttu-id="89e01-187">Välj tooedit hello fil `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) eller `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), och ändra hello rad `enabled=0` för`enabled=1` under **[ol6_addons]** eller **[ol7_addons]** i den här filen.</span><span class="sxs-lookup"><span data-stu-id="89e01-187">Choose tooedit hello file `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) or `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), and change hello line `enabled=0` too`enabled=1` under **[ol6_addons]** or **[ol7_addons]** in this file.</span></span>

<span data-ttu-id="89e01-188">Sedan tooinstall hello senaste versionen av hello Azure Linux-agenten, typ:</span><span class="sxs-lookup"><span data-stu-id="89e01-188">Then, tooinstall hello latest version of hello Azure Linux Agent, type:</span></span>

```bash
sudo yum install WALinuxAgent
```

<span data-ttu-id="89e01-189">Om du inte hittar hello tillägg databasen kan du bara lägga till dessa rader hello slutet av filen .repo enligt tooyour Oracle Linux-versionen:</span><span class="sxs-lookup"><span data-stu-id="89e01-189">If you don't find hello add-on repository you can simply add these lines at hello end of your .repo file according tooyour Oracle Linux release:</span></span>

<span data-ttu-id="89e01-190">För Oracle Linux 6 virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="89e01-190">For Oracle Linux 6 virtual machines:</span></span>

```sh
[ol6_addons]
name=Add-Ons for Oracle Linux $releasever ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
gpgcheck=1
enabled=1
```

<span data-ttu-id="89e01-191">För Oracle Linux 7 virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="89e01-191">For Oracle Linux 7 virtual machines:</span></span>

```sh
[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0
```

<span data-ttu-id="89e01-192">Sedan skriver du:</span><span class="sxs-lookup"><span data-stu-id="89e01-192">Then type:</span></span>

```bash
sudo yum update WALinuxAgent
```

<span data-ttu-id="89e01-193">Detta är vanligtvis allt du behöver, men om du av någon anledning som du behöver tooinstall från https://github.com direkt, Använd hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="89e01-193">Typically this is all you need, but if for some reason you need tooinstall it from https://github.com directly, use hello following steps.</span></span>


## <a name="update-hello-linux-agent-when-no-agent-package-exists-for-distribution"></a><span data-ttu-id="89e01-194">Uppdatera hello Linux-agenten när det finns ingen agentpaket för distribution</span><span class="sxs-lookup"><span data-stu-id="89e01-194">Update hello Linux Agent when no agent package exists for distribution</span></span>

<span data-ttu-id="89e01-195">Installera wget (det finns vissa distributioner som inte installeras som standard, till exempel Redhat, CentOS och Oracle Linux-version 6.4 och 6.5) genom att skriva `sudo yum install wget` på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="89e01-195">Install wget (there are some distros that don't install it by default, such as Redhat, CentOS, and Oracle Linux versions 6.4 and 6.5) by typing `sudo yum install wget` on hello command line.</span></span>

### <a name="1-download-hello-latest-version"></a><span data-ttu-id="89e01-196">1. Hämta hello senaste versionen</span><span class="sxs-lookup"><span data-stu-id="89e01-196">1. Download hello latest version</span></span>
<span data-ttu-id="89e01-197">Öppna [hello versionen av Azure Linux-agenten i GitHub](https://github.com/Azure/WALinuxAgent/releases) i en webbsida och ta reda på hello senaste versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="89e01-197">Open [hello release of Azure Linux Agent in GitHub](https://github.com/Azure/WALinuxAgent/releases) in a web page, and find out hello latest version number.</span></span> <span data-ttu-id="89e01-198">(Du hittar din nuvarande version genom att skriva `waagent --version`.)</span><span class="sxs-lookup"><span data-stu-id="89e01-198">(You can locate your current version by typing `waagent --version`.)</span></span>

#### <a name="for-version-22x-or-later-type"></a><span data-ttu-id="89e01-199">För version 2.2.x eller senare, skriver du:</span><span class="sxs-lookup"><span data-stu-id="89e01-199">For version 2.2.x or later, type:</span></span>
```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.x.zip
unzip v2.2.x.zip.zip
cd WALinuxAgent-2.2.x
```

<span data-ttu-id="89e01-200">hello använder följande rad version 2.2.0 som exempel:</span><span class="sxs-lookup"><span data-stu-id="89e01-200">hello following line uses version 2.2.0 as an example:</span></span>

```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.14.zip
unzip v2.2.14.zip  
cd WALinuxAgent-2.2.14
```

### <a name="2-install-hello-azure-linux-agent"></a><span data-ttu-id="89e01-201">2. Installera hello Azure Linux-agenten</span><span class="sxs-lookup"><span data-stu-id="89e01-201">2. Install hello Azure Linux Agent</span></span>

#### <a name="for-version-22x-use"></a><span data-ttu-id="89e01-202">Version för 2.2.x, Använd:</span><span class="sxs-lookup"><span data-stu-id="89e01-202">For version 2.2.x, use:</span></span>
<span data-ttu-id="89e01-203">Du kan behöva tooinstall hello paketet `setuptools` först-- [här](https://pypi.python.org/pypi/setuptools).</span><span class="sxs-lookup"><span data-stu-id="89e01-203">You may need tooinstall hello package `setuptools` first--see [here](https://pypi.python.org/pypi/setuptools).</span></span> <span data-ttu-id="89e01-204">Kör sedan:</span><span class="sxs-lookup"><span data-stu-id="89e01-204">Then run:</span></span>

```bash
sudo python setup.py install
```

#### <a name="ensure-auto-update-is-enabled"></a><span data-ttu-id="89e01-205">Kontrollera att automatiska uppdateringar är aktiverat</span><span class="sxs-lookup"><span data-stu-id="89e01-205">Ensure auto update is enabled</span></span>

<span data-ttu-id="89e01-206">Kontrollera först toosee om det är aktiverat:</span><span class="sxs-lookup"><span data-stu-id="89e01-206">First, check toosee if it is enabled:</span></span>

```bash
cat /etc/waagent.conf
```

<span data-ttu-id="89e01-207">Hitta 'AutoUpdate.Enabled'.</span><span class="sxs-lookup"><span data-stu-id="89e01-207">Find 'AutoUpdate.Enabled'.</span></span> <span data-ttu-id="89e01-208">Om du ser den här utdatan aktiveras:</span><span class="sxs-lookup"><span data-stu-id="89e01-208">If you see this output, it is enabled:</span></span>

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

<span data-ttu-id="89e01-209">tooenable kör:</span><span class="sxs-lookup"><span data-stu-id="89e01-209">tooenable run:</span></span>

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="3-restart-hello-waagent-service"></a><span data-ttu-id="89e01-210">3. Starta om hello waagent-tjänsten</span><span class="sxs-lookup"><span data-stu-id="89e01-210">3. Restart hello waagent service</span></span>
<span data-ttu-id="89e01-211">För de flesta distributioner för Linux:</span><span class="sxs-lookup"><span data-stu-id="89e01-211">For most of Linux distros:</span></span>

```bash
sudo service waagent restart
```

<span data-ttu-id="89e01-212">Ubuntu, Använd:</span><span class="sxs-lookup"><span data-stu-id="89e01-212">For Ubuntu, use:</span></span>

```bash
sudo service walinuxagent restart
```

<span data-ttu-id="89e01-213">För virtuell CoreOS, använder du:</span><span class="sxs-lookup"><span data-stu-id="89e01-213">For CoreOS, use:</span></span>

```bash
sudo systemctl restart waagent
```

### <a name="4-confirm-hello-azure-linux-agent-version"></a><span data-ttu-id="89e01-214">4. Bekräfta hello Azure Linux-agentens version</span><span class="sxs-lookup"><span data-stu-id="89e01-214">4. Confirm hello Azure Linux Agent version</span></span>
    
```bash
waagent -version
```

<span data-ttu-id="89e01-215">För virtuell CoreOS fungerar inte hello ovanstående kommando.</span><span class="sxs-lookup"><span data-stu-id="89e01-215">For CoreOS, hello above command may not work.</span></span>

<span data-ttu-id="89e01-216">Du ser att hello Azure Linux-agentens version har uppdaterats toohello ny version.</span><span class="sxs-lookup"><span data-stu-id="89e01-216">You will see that hello Azure Linux Agent version has been updated toohello new version.</span></span>

<span data-ttu-id="89e01-217">Mer information om hello Azure Linux-agenten finns [Azure Linux-agenten viktigt](https://github.com/Azure/WALinuxAgent).</span><span class="sxs-lookup"><span data-stu-id="89e01-217">For more information regarding hello Azure Linux Agent, see [Azure Linux Agent README](https://github.com/Azure/WALinuxAgent).</span></span>
