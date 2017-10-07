---
title: aaaRed Hat Update infrastruktur (RHUI) | Microsoft Docs
description: "Lär dig mer om Red Hat Update infrastruktur (RHUI) för på begäran Red Hat Enterprise Linux-instanser i Microsoft Azure"
services: virtual-machines-linux
documentationcenter: 
author: BorisB2015
manager: timlt
editor: 
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/13/2017
ms.author: borisb
ms.openlocfilehash: cc244857104b25e4e61862c518db77e915e137ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a><span data-ttu-id="f99d4-103">Red Hat Update infrastruktur (RHUI) för att på begäran Red Hat Enterprise Linux virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="f99d4-103">Red Hat Update Infrastructure (RHUI) for on-demand Red Hat Enterprise Linux VMs in Azure</span></span>
<span data-ttu-id="f99d4-104">Virtuella datorer skapas från hello på begäran Red Hat Enterprise Linux (RHEL) bilder som finns i Azure Marketplace är registrerade tooaccess hello Red Hat Update infrastruktur (RHUI) distribuerade i Azure.</span><span class="sxs-lookup"><span data-stu-id="f99d4-104">Virtual machines created from hello on-demand Red Hat Enterprise Linux (RHEL) images available in Azure Marketplace are registered tooaccess hello Red Hat Update Infrastructure (RHUI) deployed in Azure.</span></span>  <span data-ttu-id="f99d4-105">hello på begäran RHEL instanser ha åtkomst tooa regionala yum databasen och kan tooreceive inkrementella uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="f99d4-105">hello on-demand RHEL instances have access tooa regional yum repository and able tooreceive incremental updates.</span></span>

<span data-ttu-id="f99d4-106">Hej yum listan som hanteras av RHUI har konfigurerats i din instans av RHEL under etableringen.</span><span class="sxs-lookup"><span data-stu-id="f99d4-106">hello yum repository list, which is managed by RHUI, is configured in your RHEL instance during provisioning.</span></span> <span data-ttu-id="f99d4-107">Du behöver inte toodo någon ytterligare konfiguration – kör `yum update` när RHEL-instans är klar tooget hello senaste uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="f99d4-107">You don't need toodo any additional configuration - run `yum update` after your RHEL instance is ready tooget hello latest updates.</span></span>

> [!NOTE]
> <span data-ttu-id="f99d4-108">I September 2016 distribuerade vi en uppdaterad Azure RHUI och i januari 2017 startade vi stegvis avstängning av hello äldre Azure RHUI.</span><span class="sxs-lookup"><span data-stu-id="f99d4-108">In September 2016 we deployed an updated Azure RHUI and in January 2017 we started phased shutdown of hello older Azure RHUI.</span></span> <span data-ttu-id="f99d4-109">Om du har använt hello RHEL bilder (eller deras ögonblicksbilder) från September 2016 eller senare - troligen krävs ingen åtgärd.</span><span class="sxs-lookup"><span data-stu-id="f99d4-109">If you have been using hello RHEL images (or their snapshots) from September 2016 or later - likely no action is required.</span></span> <span data-ttu-id="f99d4-110">Om du har dock äldre ögonblicksbilder/virtuella datorer, måste deras konfiguration uppdateras för oavbruten tillgång toohello Azure RHUI toobe.</span><span class="sxs-lookup"><span data-stu-id="f99d4-110">If, however, you have older snapshots/VMs, their configuration needs toobe updated for uninterrupted access toohello Azure RHUI.</span></span>
> 

## <a name="rhui-azure-infrastructure-update"></a><span data-ttu-id="f99d4-111">Uppdatering av RHUI Azure-infrastrukturen</span><span class="sxs-lookup"><span data-stu-id="f99d4-111">RHUI Azure Infrastructure Update</span></span>
<span data-ttu-id="f99d4-112">Från och med September 2016 har Azure en ny uppsättning Red Hat Update infrastruktur (RHUI) servrar.</span><span class="sxs-lookup"><span data-stu-id="f99d4-112">As of September 2016, Azure has a new set of Red Hat Update Infrastructure (RHUI) servers.</span></span> <span data-ttu-id="f99d4-113">Dessa servrar distribueras med [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) så att en enda slutpunkt (rhui 1.microsoft.com) kan användas av alla VM oavsett region.</span><span class="sxs-lookup"><span data-stu-id="f99d4-113">These servers are deployed with [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) so that a single endpoint (rhui-1.microsoft.com) can be used by any VM regardless of region.</span></span> <span data-ttu-id="f99d4-114">Hej nya RHEL betala per användning (PAYG) bilder i hello Azure Marketplace (versioner datum September 2016 eller senare) punkt toohello nya Azure RHUI servrar och inte kräver någon ytterligare åtgärd.</span><span class="sxs-lookup"><span data-stu-id="f99d4-114">hello new RHEL Pay-As-You-Go (PAYG) images in hello Azure Marketplace (versions dated September 2016 or later) point toohello new Azure RHUI servers and do not require any additional action.</span></span>

### <a name="determine-if-action-is-required"></a><span data-ttu-id="f99d4-115">Avgöra om åtgärd krävs</span><span class="sxs-lookup"><span data-stu-id="f99d4-115">Determine if action is required</span></span>
<span data-ttu-id="f99d4-116">Följ anvisningarna nedan om du har problem med att ansluta tooAzure RHUI från din Azure RHEL PAYG VM</span><span class="sxs-lookup"><span data-stu-id="f99d4-116">If you are experiencing problems connecting tooAzure RHUI from your Azure RHEL PAYG VM, follow these steps</span></span>
1. <span data-ttu-id="f99d4-117">Kontrollera konfigurationen för virtuell dator för Azure RHUI slutpunkt</span><span class="sxs-lookup"><span data-stu-id="f99d4-117">Inspect VM configuration for Azure RHUI endpoint</span></span>

    <span data-ttu-id="f99d4-118">Kontrollera om `/etc/yum.repos.d/rh-cloud.repo` filen innehåller referens för`rhui-[1-3].microsoft.com` i baseurl av `[rhui-microsoft-azure-rhel*]` i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="f99d4-118">Check if `/etc/yum.repos.d/rh-cloud.repo` file contains reference too`rhui-[1-3].microsoft.com` in baseurl of `[rhui-microsoft-azure-rhel*]` section of hello file.</span></span> <span data-ttu-id="f99d4-119">Om det är – du använder hello nya Azure RHUI.</span><span class="sxs-lookup"><span data-stu-id="f99d4-119">If it is - you are using hello new Azure RHUI.</span></span>

    <span data-ttu-id="f99d4-120">Om den peka tooa plats med hello enligt `mirrorlist.*cds[1-4].cloudapp.net` -hello konfiguration måste uppdateras.</span><span class="sxs-lookup"><span data-stu-id="f99d4-120">If it pointing tooa location with hello following pattern `mirrorlist.*cds[1-4].cloudapp.net` - hello configuration update is required.</span></span>

    <span data-ttu-id="f99d4-121">Om du använder hello ny konfiguration och fortfarande inte kan ansluta tooAzure RHUI - filen som ett supportärende med Microsoft eller Red Hat.</span><span class="sxs-lookup"><span data-stu-id="f99d4-121">If you are using hello new configuration and still cannot connect tooAzure RHUI - file a support case with Microsoft or Red Hat.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f99d4-122">Åtkomst tooAzure värd RHUI är begränsad toohello VM: ar inom [Microsoft Azure Datacenter IP-intervall](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="f99d4-122">Access tooAzure-hosted RHUI is limited toohello VMs within [Microsoft Azure Datacenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>
    > 

2. <span data-ttu-id="f99d4-123">Om hello gamla Azure RHUI fortfarande är tillgänglig när du gör den här kontrollen och du vill att tooautomatically uppdateringskonfiguration hello, kör du följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f99d4-123">If hello old Azure RHUI is still available when you do this check and you would like tooautomatically update hello configuration, execute hello following command:</span></span>

    <span data-ttu-id="f99d4-124">`sudo yum update RHEL6`eller `sudo yum update RHEL7` beroende på hello RHEL-versionen.</span><span class="sxs-lookup"><span data-stu-id="f99d4-124">`sudo yum update RHEL6` or `sudo yum update RHEL7` depending on hello RHEL family version.</span></span>

3. <span data-ttu-id="f99d4-125">Om du inte längre kan ansluta toohello gamla Azure RHUI, följ hello manuella stegen som beskrivs i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="f99d4-125">If you can no longer connect toohello old Azure RHUI, follow hello manual steps described in hello next section.</span></span>

4. <span data-ttu-id="f99d4-126">Kontrollera att tooupdate hello konfiguration på hello källan/ögonblicksbilden påverkas VM har etablerats från.</span><span class="sxs-lookup"><span data-stu-id="f99d4-126">Make sure tooupdate hello configuration on hello source image/snapshot affected VM was provisioned from.</span></span>

### <a name="phased-shutdown-of-hello-old-azure-rhui"></a><span data-ttu-id="f99d4-127">Stegvis avstängning av hello gamla Azure RHUI</span><span class="sxs-lookup"><span data-stu-id="f99d4-127">Phased shutdown of hello old Azure RHUI</span></span>
<span data-ttu-id="f99d4-128">Under hello avstängning av hello gamla Azure RHUI vi begränsa åtkomst till tooit på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f99d4-128">During hello shutdown of hello old Azure RHUI we restrict access tooit as follows:</span></span>

1. <span data-ttu-id="f99d4-129">Ytterligare begränsa åtkomst (ACL) tooset IP-adresser som redan ansluter tooit.</span><span class="sxs-lookup"><span data-stu-id="f99d4-129">Further restrict access (ACL) tooset of IP addresses that are already connecting tooit.</span></span> <span data-ttu-id="f99d4-130">Möjliga sidoeffekter: Om du fortsätter med hello gamla Azure RHUI – din nya virtuella datorer kanske inte kan tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="f99d4-130">Possible side-effects: if you continue using hello old Azure RHUI - your new VMs may not be able tooconnect tooit.</span></span> <span data-ttu-id="f99d4-131">RHEL virtuella datorer med dynamiska IP-adresser som avstängning/frigöra/starta rad kan få nya IP och därför också kunde starta misslyckas tooconnect toohello gamla Azure RHUI</span><span class="sxs-lookup"><span data-stu-id="f99d4-131">RHEL VMs with dynamic IPs that go through shutdown/deallocate/start sequence may receive new IP and hence also could start failing tooconnect toohello old Azure RHUI</span></span>

2. <span data-ttu-id="f99d4-132">Stäng av spegling innehållsleverans servrar.</span><span class="sxs-lookup"><span data-stu-id="f99d4-132">Shutdown of mirror content delivery servers.</span></span> <span data-ttu-id="f99d4-133">Möjliga sidoeffekter: som vi Stäng mer CDSes kan du se längre `yum update` behandlingen tid mer tidsgränser fram till hello peka när du inte längre kan ansluta toohello gamla Azure RHUI.</span><span class="sxs-lookup"><span data-stu-id="f99d4-133">Possible side-effects: as we shut down more CDSes you may see longer `yum update` servicing time, more timeouts up until hello point when you can no longer connect toohello old Azure RHUI.</span></span>

### <a name="hello-ips-for-hello-new-rhui-content-delivery-servers-are"></a><span data-ttu-id="f99d4-134">hello IP-adresser för hello nya RHUI innehållsleverans servrar är</span><span class="sxs-lookup"><span data-stu-id="f99d4-134">hello IPs for hello new RHUI content delivery servers are</span></span>
<span data-ttu-id="f99d4-135">Om du använder network configuration toofurther begränsa åtkomst från RHEL PAYG virtuella datorer, se till att hello efter IP-adresser tillåts för `yum update` toowork beroende på hello-miljö i.</span><span class="sxs-lookup"><span data-stu-id="f99d4-135">If you are using network configuration toofurther restrict access from RHEL PAYG VMs, make sure hello following IPs are allowed for `yum update` toowork depending on hello environment you are in.</span></span> 

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193

# Azure Germany
51.5.243.77
51.4.228.145
```

### <a name="manual-update-procedure-toouse-hello-new-azure-rhui-servers"></a><span data-ttu-id="f99d4-136">Manuell uppdatering proceduren toouse hello nya Azure RHUI servrar</span><span class="sxs-lookup"><span data-stu-id="f99d4-136">Manual update procedure toouse hello new Azure RHUI servers</span></span>
<span data-ttu-id="f99d4-137">Hämta (via curl) hello offentliga nyckel signatur</span><span class="sxs-lookup"><span data-stu-id="f99d4-137">Download (via curl) hello public key signature</span></span>

```bash
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

<span data-ttu-id="f99d4-138">Verifiera hello ned nyckeln</span><span class="sxs-lookup"><span data-stu-id="f99d4-138">Verify hello downloaded key</span></span>

```bash
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

<span data-ttu-id="f99d4-139">Kontrollera hello utdata, kontrollera `keyid` och `user ID packet`:</span><span class="sxs-lookup"><span data-stu-id="f99d4-139">Check hello output, verify `keyid` and `user ID packet`:</span></span>

```bash
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

<span data-ttu-id="f99d4-140">Installera hello offentlig nyckel</span><span class="sxs-lookup"><span data-stu-id="f99d4-140">Install hello public key</span></span>

```bash
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

<span data-ttu-id="f99d4-141">Hämta, kontrollera och installera klienten RPM</span><span class="sxs-lookup"><span data-stu-id="f99d4-141">Download, Verify, and Install Client RPM</span></span>

<span data-ttu-id="f99d4-142">Hämta: För RHEL 6</span><span class="sxs-lookup"><span data-stu-id="f99d4-142">Download: For RHEL 6</span></span>

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

<span data-ttu-id="f99d4-143">För RHEL 7</span><span class="sxs-lookup"><span data-stu-id="f99d4-143">For RHEL 7</span></span>

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

<span data-ttu-id="f99d4-144">Kontrollera:</span><span class="sxs-lookup"><span data-stu-id="f99d4-144">Verify:</span></span>

```bash
rpm -Kv azureclient.rpm
```

<span data-ttu-id="f99d4-145">Kontrollera i utdata signaturen för hello paketet är OK</span><span class="sxs-lookup"><span data-stu-id="f99d4-145">Check in output that signature of hello package is OK</span></span>

```bash
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

<span data-ttu-id="f99d4-146">Installera hello RPM</span><span class="sxs-lookup"><span data-stu-id="f99d4-146">Install hello RPM</span></span>

```bash
sudo rpm -U azureclient.rpm
```

<span data-ttu-id="f99d4-147">Kontrollera att du har åtkomst till Azure RHUI formuläret hello VM när åtgärden har slutförts</span><span class="sxs-lookup"><span data-stu-id="f99d4-147">Upon completion, verify that you can access Azure RHUI form hello VM</span></span>

### <a name="all-in-one-script-for-automating-hello-preceding-task"></a><span data-ttu-id="f99d4-148">Allt i ett skript för att automatisera hello föregående aktivitet</span><span class="sxs-lookup"><span data-stu-id="f99d4-148">All-in-one script for automating hello preceding task</span></span>
<span data-ttu-id="f99d4-149">Använd följande skript som behövs tooautomate hello aktivitet för att uppdatera berörda VMs toohello nya Azure RHUI servrar hello.</span><span class="sxs-lookup"><span data-stu-id="f99d4-149">Use hello following script as needed tooautomate hello task of updating affected VMs toohello new Azure RHUI servers.</span></span>

```sh
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a><span data-ttu-id="f99d4-150">Översikt över RHUI</span><span class="sxs-lookup"><span data-stu-id="f99d4-150">RHUI overview</span></span>
<span data-ttu-id="f99d4-151">[Infrastrukturen för Red Hat](https://access.redhat.com/products/red-hat-update-infrastructure) erbjuder en mycket skalbar lösning toomanage yum databasinnehåll för Red Hat Enterprise Linux moln-instanser som hanteras av Red Hat-certifierad molntjänstleverantörer.</span><span class="sxs-lookup"><span data-stu-id="f99d4-151">[Red Hat Update Infrastructure](https://access.redhat.com/products/red-hat-update-infrastructure) offers a highly scalable solution toomanage yum repository content for Red Hat Enterprise Linux cloud instances that are hosted by Red Hat-certified cloud providers.</span></span> <span data-ttu-id="f99d4-152">Baserat på hello överordnade massa projektet RHUI gör att molntjänstleverantörer toolocally spegling Red Hat-värdbaserad databasinnehåll, skapa anpassade databaser med sitt eget innehåll och gör dessa databaser tillgängliga tooa stor grupp med användare via en Utjämning av nätverksbelastning Content delivery system.</span><span class="sxs-lookup"><span data-stu-id="f99d4-152">Based on hello upstream Pulp project, RHUI allows cloud providers toolocally mirror Red Hat-hosted repository content, create custom repositories with their own content, and make those repositories available tooa large group of end users through a load-balanced content delivery system.</span></span>

## <a name="regions-where-rhui-is-available"></a><span data-ttu-id="f99d4-153">Regioner där RHUI är tillgängligt</span><span class="sxs-lookup"><span data-stu-id="f99d4-153">Regions where RHUI is available</span></span>
<span data-ttu-id="f99d4-154">RHUI är tillgänglig i alla regioner där RHEL på begäran bilder är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="f99d4-154">RHUI is available in all regions where RHEL on-demand images are available.</span></span> <span data-ttu-id="f99d4-155">För närvarande innehåller alla offentliga områden på hello [Azure status instrumentpanelen](https://azure.microsoft.com/status/) sidan Azure som tillhör amerikanska myndigheter och Azure Tyskland regioner.</span><span class="sxs-lookup"><span data-stu-id="f99d4-155">It currently includes all public regions listed on hello [Azure status dashboard](https://azure.microsoft.com/status/) page, Azure US Government and Azure Germany regions.</span></span> <span data-ttu-id="f99d4-156">RHUI åtkomst för virtuella datorer som etablerats från RHEL på begäran avbildningar finns i deras pris.</span><span class="sxs-lookup"><span data-stu-id="f99d4-156">RHUI access for VMs provisioned from RHEL on-demand images is included in their price.</span></span> <span data-ttu-id="f99d4-157">Ytterligare regionala/nationella molnet tillgänglighet kommer att uppdateras när vi Expandera RHEL på begäran tillgänglighet i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="f99d4-157">Additional regional/national cloud availability will be updated as we expand RHEL on-demand availability in hello future.</span></span>

> [!NOTE]
> <span data-ttu-id="f99d4-158">Åtkomst tooAzure värd RHUI är begränsad toohello VM: ar inom [Microsoft Azure Datacenter IP-intervall](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="f99d4-158">Access tooAzure-hosted RHUI is limited toohello VMs within [Microsoft Azure Datacenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>
> 
> 

## <a name="get-updates-from-another-update-repository"></a><span data-ttu-id="f99d4-159">Hämta uppdateringar från en annan uppdatering databas</span><span class="sxs-lookup"><span data-stu-id="f99d4-159">Get updates from another update repository</span></span>
<span data-ttu-id="f99d4-160">Om du behöver tooget uppdateringar från en annan uppdatering databas (i stället för Azure-baserad RHUI), måste du först toounregister dina instanser från RHUI.</span><span class="sxs-lookup"><span data-stu-id="f99d4-160">If you need tooget updates from a different update repository (instead of Azure-hosted RHUI), first you need toounregister your instances from RHUI.</span></span> <span data-ttu-id="f99d4-161">Du måste registrera toore dem med hello önskade infrastrukturen (till exempel Red Hat satellit eller Red Hat kundens Portal CDN).</span><span class="sxs-lookup"><span data-stu-id="f99d4-161">Then you need toore-register them with hello desired update infrastructure (such as Red Hat Satellite or Red Hat Customer Portal CDN).</span></span> <span data-ttu-id="f99d4-162">Du behöver lämpliga Red Hat prenumerationer för dessa tjänster och registrering för [Red Hat Molnåtkomst i Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).</span><span class="sxs-lookup"><span data-stu-id="f99d4-162">You will need appropriate Red Hat subscriptions for these services and registration for [Red Hat Cloud Access in Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).</span></span>

<span data-ttu-id="f99d4-163">Så här toounregister RHUI och registrera om tooyour uppdatering infrastruktur:</span><span class="sxs-lookup"><span data-stu-id="f99d4-163">toounregister RHUI and reregister tooyour update infrastructure follow these steps:</span></span>

1. <span data-ttu-id="f99d4-164">Redigera /etc/yum.repos.d/rh-cloud.repo och ändra alla `enabled=1` för`enabled=0`.</span><span class="sxs-lookup"><span data-stu-id="f99d4-164">Edit /etc/yum.repos.d/rh-cloud.repo and change all `enabled=1` too`enabled=0`.</span></span> <span data-ttu-id="f99d4-165">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f99d4-165">For example:</span></span>
   
   ```bash
   sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo
   ```
   
2. <span data-ttu-id="f99d4-166">Redigera /etc/yum/pluginconf.d/rhnplugin.conf och ändra `enabled=0` för`enabled=1`.</span><span class="sxs-lookup"><span data-stu-id="f99d4-166">Edit /etc/yum/pluginconf.d/rhnplugin.conf and change `enabled=0` too`enabled=1`.</span></span>
3. <span data-ttu-id="f99d4-167">Registrera sedan med hello önskade infrastrukturen, till exempel Red Hat-kundportalen.</span><span class="sxs-lookup"><span data-stu-id="f99d4-167">Then register with hello desired infrastructure, such as Red Hat Customer Portal.</span></span> <span data-ttu-id="f99d4-168">Följ Red Hat solution guide på [hur tooregister och prenumerera på system-toohello Red Hat-kundportalen](https://access.redhat.com/solutions/253273).</span><span class="sxs-lookup"><span data-stu-id="f99d4-168">Follow Red Hat solution guide on [how tooregister and subscribe a system toohello Red Hat Customer Portal](https://access.redhat.com/solutions/253273).</span></span>

> [!NOTE]
> <span data-ttu-id="f99d4-169">Åtkomst toohello Azure-baserad RHUI ingår i hello RHEL betala per användning (PAYG) avbildningen pris.</span><span class="sxs-lookup"><span data-stu-id="f99d4-169">Access toohello Azure-hosted RHUI is included in hello RHEL Pay-As-You-Go (PAYG) image price.</span></span> <span data-ttu-id="f99d4-170">Avregistrerar en virtuell dator RHEL PAYG från hello Azure-baserad RHUI konverterar inte hello virtuella datorn till Bring-Your-äger-licens (BYOL) typ VM.</span><span class="sxs-lookup"><span data-stu-id="f99d4-170">Unregistering a PAYG RHEL VM from hello Azure-hosted RHUI does not convert hello virtual machine into Bring-Your-Own-License (BYOL) type VM.</span></span> <span data-ttu-id="f99d4-171">Om du registrerar hello samma virtuella dator med en annan källa för uppdateringar kan du drabbas dubbla avgifter: första gången för Azure RHEL programvara avgift och hello gång för Red Hat-abonnemang.</span><span class="sxs-lookup"><span data-stu-id="f99d4-171">If you register hello same VM with another source of updates you may be incurring double charges: first time for Azure RHEL software fee, and hello second time for Red Hat subscription(s).</span></span> 
> 
> <span data-ttu-id="f99d4-172">Om du behöver konsekvent toouse en annan infrastruktur än Azure-baserad RHUI du skapa och distribuera dina egna avbildningar (BYOL-typ) enligt beskrivningen i [skapa och ladda upp Red Hat-baserad virtuell dator för Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artikel.</span><span class="sxs-lookup"><span data-stu-id="f99d4-172">If you consistently need toouse an update infrastructure other than Azure-hosted RHUI consider creating and deploying your own (BYOL-type) images as described in [Create and Upload Red Hat-based virtual machine for Azure](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) article.</span></span>
> 

## <a name="next-steps"></a><span data-ttu-id="f99d4-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f99d4-173">Next steps</span></span>
<span data-ttu-id="f99d4-174">toocreate en Red Hat Enterprise Linux virtuell dator från Azure Marketplace betala per användning avbildningen och utnyttjar Azure-baserad RHUI gå för[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/).</span><span class="sxs-lookup"><span data-stu-id="f99d4-174">toocreate a Red Hat Enterprise Linux VM from Azure Marketplace Pay-As-You-Go image and leverage Azure-hosted RHUI go too[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/).</span></span> <span data-ttu-id="f99d4-175">Du kommer att kunna toouse `yum update` i din RHEL instans utan några ytterligare inställningar.</span><span class="sxs-lookup"><span data-stu-id="f99d4-175">You will be able toouse `yum update` in your RHEL instance without any additional setup.</span></span>

