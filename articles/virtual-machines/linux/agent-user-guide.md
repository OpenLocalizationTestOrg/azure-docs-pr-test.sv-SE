---
title: "aaaAzure Linux VM-agenten översikt | Microsoft Docs"
description: "Lär dig hur tooinstall och konfigurera den virtuella datorns interaktion med Azure-Infrastrukturkontrollanten för Linux-agenten (waagent) toomanage."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: e41de979-6d56-40b0-8916-895bf215ded6
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: szark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4e08c84d9205f4db7aae6fd1568ec1f15fba395c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-and-using-hello-azure-linux-agent"></a><span data-ttu-id="bb8ce-103">Förstå och använda hello Azure Linux-agenten</span><span class="sxs-lookup"><span data-stu-id="bb8ce-103">Understanding and using hello Azure Linux Agent</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a><span data-ttu-id="bb8ce-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="bb8ce-104">Introduction</span></span>
<span data-ttu-id="bb8ce-105">hello Microsoft Azure Linux-agenten hanterar (waagent) Linux och FreeBSD etablering och VM-interaktion med hello Azure-Infrastrukturkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-105">hello Microsoft Azure Linux Agent (waagent) manages Linux & FreeBSD provisioning, and VM interaction with hello Azure Fabric Controller.</span></span> <span data-ttu-id="bb8ce-106">Det innehåller följande funktioner för distributioner av Linux och FreeBSD IaaS hello:</span><span class="sxs-lookup"><span data-stu-id="bb8ce-106">It provides hello following functionality for Linux and FreeBSD IaaS deployments:</span></span>

> [!NOTE]
> <span data-ttu-id="bb8ce-107">Se hello Azure Linux-agenten [viktigt](https://github.com/Azure/WALinuxAgent/blob/master/README.md) för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-107">See hello Azure Linux agent [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md) for additional details.</span></span>
> 
> 

* <span data-ttu-id="bb8ce-108">**Bild etablering**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-108">**Image Provisioning**</span></span>
  
  * <span data-ttu-id="bb8ce-109">Skapa ett användarkonto</span><span class="sxs-lookup"><span data-stu-id="bb8ce-109">Creation of a user account</span></span>
  * <span data-ttu-id="bb8ce-110">Konfigurera typer av SSH-autentisering</span><span class="sxs-lookup"><span data-stu-id="bb8ce-110">Configuring SSH authentication types</span></span>
  * <span data-ttu-id="bb8ce-111">Distribution av offentliga SSH-nycklar och nyckelpar</span><span class="sxs-lookup"><span data-stu-id="bb8ce-111">Deployment of SSH public keys and key pairs</span></span>
  * <span data-ttu-id="bb8ce-112">Inställningen hello värdnamn</span><span class="sxs-lookup"><span data-stu-id="bb8ce-112">Setting hello host name</span></span>
  * <span data-ttu-id="bb8ce-113">Publishing hello namn toohello värdplattform DNS</span><span class="sxs-lookup"><span data-stu-id="bb8ce-113">Publishing hello host name toohello platform DNS</span></span>
  * <span data-ttu-id="bb8ce-114">Rapportering SSH nyckel toohello plattform</span><span class="sxs-lookup"><span data-stu-id="bb8ce-114">Reporting SSH host key fingerprint toohello platform</span></span>
  * <span data-ttu-id="bb8ce-115">Disk resurshantering</span><span class="sxs-lookup"><span data-stu-id="bb8ce-115">Resource Disk Management</span></span>
  * <span data-ttu-id="bb8ce-116">Formatering och monterar hello resurs disk</span><span class="sxs-lookup"><span data-stu-id="bb8ce-116">Formatting and mounting hello resource disk</span></span>
  * <span data-ttu-id="bb8ce-117">Konfigurera växlingsutrymme</span><span class="sxs-lookup"><span data-stu-id="bb8ce-117">Configuring swap space</span></span>
* <span data-ttu-id="bb8ce-118">**Nätverk**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-118">**Networking**</span></span>
  
  * <span data-ttu-id="bb8ce-119">Hanterar vägar tooimprove kompatibilitet med plattformar DHCP-servrar</span><span class="sxs-lookup"><span data-stu-id="bb8ce-119">Manages routes tooimprove compatibility with platform DHCP servers</span></span>
  * <span data-ttu-id="bb8ce-120">Garanterar hello stabiliteten i hello nätverksgränssnittets namn</span><span class="sxs-lookup"><span data-stu-id="bb8ce-120">Ensures hello stability of hello network interface name</span></span>
* <span data-ttu-id="bb8ce-121">**Kernel**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-121">**Kernel**</span></span>
  
  * <span data-ttu-id="bb8ce-122">Konfigurerar virtuell NUMA (inaktivera för kernel < 2.6.37)</span><span class="sxs-lookup"><span data-stu-id="bb8ce-122">Configures virtual NUMA (disable for kernel <2.6.37)</span></span>
  * <span data-ttu-id="bb8ce-123">Förbrukar Hyper-V entropi för /dev/random</span><span class="sxs-lookup"><span data-stu-id="bb8ce-123">Consumes Hyper-V entropy for /dev/random</span></span>
  * <span data-ttu-id="bb8ce-124">Konfigurerar SCSI tidsgränser för hello rotenheten (som kan vara fjärråtkomst)</span><span class="sxs-lookup"><span data-stu-id="bb8ce-124">Configures SCSI timeouts for hello root device (which could be remote)</span></span>
* <span data-ttu-id="bb8ce-125">**Diagnostik**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-125">**Diagnostics**</span></span>
  
  * <span data-ttu-id="bb8ce-126">Konsolen omdirigering toohello seriell port</span><span class="sxs-lookup"><span data-stu-id="bb8ce-126">Console redirection toohello serial port</span></span>
* <span data-ttu-id="bb8ce-127">**SCVMM-distributioner**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-127">**SCVMM Deployments**</span></span>
  
  * <span data-ttu-id="bb8ce-128">Identifierar och startar hello VMM-agenten för Linux vid körning i en miljö med System Center Virtual Machine Manager 2012 R2</span><span class="sxs-lookup"><span data-stu-id="bb8ce-128">Detects and bootstraps hello VMM agent for Linux when running in a System Center Virtual Machine Manager 2012 R2 environment</span></span>
* <span data-ttu-id="bb8ce-129">**Tillägg för virtuell dator**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-129">**VM Extension**</span></span>
  
  * <span data-ttu-id="bb8ce-130">Mata in komponenten som skapats av Microsoft och Partners i Linux VM (IaaS) tooenable program och konfiguration automation</span><span class="sxs-lookup"><span data-stu-id="bb8ce-130">Inject component authored by Microsoft and Partners into Linux VM (IaaS) tooenable software and configuration automation</span></span>
  * <span data-ttu-id="bb8ce-131">VM-tillägget referensimplementering på [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)</span><span class="sxs-lookup"><span data-stu-id="bb8ce-131">VM Extension reference implementation on [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)</span></span>

## <a name="communication"></a><span data-ttu-id="bb8ce-132">Kommunikation</span><span class="sxs-lookup"><span data-stu-id="bb8ce-132">Communication</span></span>
<span data-ttu-id="bb8ce-133">hello informationsflödet från hello plattform toohello agent görs via två kanaler:</span><span class="sxs-lookup"><span data-stu-id="bb8ce-133">hello information flow from hello platform toohello agent occurs via two channels:</span></span>

* <span data-ttu-id="bb8ce-134">En starten kopplade DVD för IaaS-distributioner.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-134">A boot-time attached DVD for IaaS deployments.</span></span> <span data-ttu-id="bb8ce-135">DVD-skivan innehåller ett OVF-kompatibel konfigurationsfil som innehåller alla Etableringsinformation än hello faktiska SSH keypairs.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-135">This DVD includes an OVF-compliant configuration file that includes all provisioning information other than hello actual SSH keypairs.</span></span>
* <span data-ttu-id="bb8ce-136">En TCP-slutpunkt som Exponerar en REST-API används tooobtain distribution och konfiguration av topologi.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-136">A TCP endpoint exposing a REST API used tooobtain deployment and topology configuration.</span></span>

## <a name="requirements"></a><span data-ttu-id="bb8ce-137">Krav</span><span class="sxs-lookup"><span data-stu-id="bb8ce-137">Requirements</span></span>
<span data-ttu-id="bb8ce-138">hello följande system har testats och är känd toowork med hello Azure Linux-agenten:</span><span class="sxs-lookup"><span data-stu-id="bb8ce-138">hello following systems have been tested and are known toowork with hello Azure Linux Agent:</span></span>

> [!NOTE]
> <span data-ttu-id="bb8ce-139">Den här listan kan skilja sig från hello officiella lista över operativsystem som stöds på hello Microsoft Azure-plattformen, som beskrivs här: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)</span><span class="sxs-lookup"><span data-stu-id="bb8ce-139">This list may differ from hello official list of supported systems on hello Microsoft Azure Platform, as described here: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)</span></span>
> 
> 

* <span data-ttu-id="bb8ce-140">CoreOS</span><span class="sxs-lookup"><span data-stu-id="bb8ce-140">CoreOS</span></span>
* <span data-ttu-id="bb8ce-141">CentOS 6.3 +</span><span class="sxs-lookup"><span data-stu-id="bb8ce-141">CentOS 6.3+</span></span>
* <span data-ttu-id="bb8ce-142">Red Hat Enterprise Linux 6.7 +</span><span class="sxs-lookup"><span data-stu-id="bb8ce-142">Red Hat Enterprise Linux 6.7+</span></span>
* <span data-ttu-id="bb8ce-143">Debian 7.0 +</span><span class="sxs-lookup"><span data-stu-id="bb8ce-143">Debian 7.0+</span></span>
* <span data-ttu-id="bb8ce-144">Ubuntu 12.04 +</span><span class="sxs-lookup"><span data-stu-id="bb8ce-144">Ubuntu 12.04+</span></span>
* <span data-ttu-id="bb8ce-145">openSUSE 12.3 +</span><span class="sxs-lookup"><span data-stu-id="bb8ce-145">openSUSE 12.3+</span></span>
* <span data-ttu-id="bb8ce-146">SLES 11 SP3 +</span><span class="sxs-lookup"><span data-stu-id="bb8ce-146">SLES 11 SP3+</span></span>
* <span data-ttu-id="bb8ce-147">Oracle Linux 6.4 +</span><span class="sxs-lookup"><span data-stu-id="bb8ce-147">Oracle Linux 6.4+</span></span>

<span data-ttu-id="bb8ce-148">Andra operativsystem som stöds:</span><span class="sxs-lookup"><span data-stu-id="bb8ce-148">Other Supported Systems:</span></span>

* <span data-ttu-id="bb8ce-149">FreeBSD 10 + (Azure Linux-agenten v2.0.10 +)</span><span class="sxs-lookup"><span data-stu-id="bb8ce-149">FreeBSD 10+ (Azure Linux Agent v2.0.10+)</span></span>

<span data-ttu-id="bb8ce-150">hello Linux-agenten beror på vissa Systempaket i ordning toofunction korrekt:</span><span class="sxs-lookup"><span data-stu-id="bb8ce-150">hello Linux agent depends on some system packages in order toofunction properly:</span></span>

* <span data-ttu-id="bb8ce-151">Python 2.6 +</span><span class="sxs-lookup"><span data-stu-id="bb8ce-151">Python 2.6+</span></span>
* <span data-ttu-id="bb8ce-152">OpenSSL 1.0 +</span><span class="sxs-lookup"><span data-stu-id="bb8ce-152">OpenSSL 1.0+</span></span>
* <span data-ttu-id="bb8ce-153">OpenSSH 5.3 +</span><span class="sxs-lookup"><span data-stu-id="bb8ce-153">OpenSSH 5.3+</span></span>
* <span data-ttu-id="bb8ce-154">Verktyg för filsystem: sfdisk datorn mkfs, åtskilda</span><span class="sxs-lookup"><span data-stu-id="bb8ce-154">Filesystem utilities: sfdisk, fdisk, mkfs, parted</span></span>
* <span data-ttu-id="bb8ce-155">Lösenord verktyg: chpasswd sudo</span><span class="sxs-lookup"><span data-stu-id="bb8ce-155">Password tools: chpasswd, sudo</span></span>
* <span data-ttu-id="bb8ce-156">Text som bearbetar verktyg: sed grep</span><span class="sxs-lookup"><span data-stu-id="bb8ce-156">Text processing tools: sed, grep</span></span>
* <span data-ttu-id="bb8ce-157">Verktyg: IP-väg</span><span class="sxs-lookup"><span data-stu-id="bb8ce-157">Network tools: ip-route</span></span>
* <span data-ttu-id="bb8ce-158">Kernel stöd för att montera UDF-filsystem.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-158">Kernel support for mounting UDF filesystems.</span></span>

## <a name="installation"></a><span data-ttu-id="bb8ce-159">Installation</span><span class="sxs-lookup"><span data-stu-id="bb8ce-159">Installation</span></span>
<span data-ttu-id="bb8ce-160">Installation med en RPM eller ett DEB-paket från din distribution paketdatabasen är hello önskad metod för installation och uppgradering hello Azure Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-160">Installation using an RPM or a DEB package from your distribution's package repository is hello preferred method of installing and upgrading hello Azure Linux Agent.</span></span> <span data-ttu-id="bb8ce-161">Alla hello [godkända distribution providers](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) integrera hello Azure Linux-agenten i sina avbildningar och databaser.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-161">All hello [endorsed distribution providers](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) integrate hello Azure Linux agent package into their images and repositories.</span></span>

<span data-ttu-id="bb8ce-162">Se dokumentationen för toohello i hello [lagringsplatsen för Azure Linux-agenten på GitHub](https://github.com/Azure/WALinuxAgent) för avancerade installationsalternativ, till exempel installera från käll- eller toocustom platser eller prefix.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-162">Refer toohello documentation in hello [Azure Linux Agent repo on GitHub](https://github.com/Azure/WALinuxAgent) for advanced installation options, such as installing from source or toocustom locations or prefixes.</span></span>

## <a name="command-line-options"></a><span data-ttu-id="bb8ce-163">Kommandoradsalternativ</span><span class="sxs-lookup"><span data-stu-id="bb8ce-163">Command Line Options</span></span>
### <a name="flags"></a><span data-ttu-id="bb8ce-164">Flaggor</span><span class="sxs-lookup"><span data-stu-id="bb8ce-164">Flags</span></span>
* <span data-ttu-id="bb8ce-165">utförlig: öka mängden information i angivna kommandot</span><span class="sxs-lookup"><span data-stu-id="bb8ce-165">verbose: Increase verbosity of specified command</span></span>
* <span data-ttu-id="bb8ce-166">tvinga: hoppa över interaktiva bekräftelse för vissa kommandon</span><span class="sxs-lookup"><span data-stu-id="bb8ce-166">force: Skip interactive confirmation for some commands</span></span>

### <a name="commands"></a><span data-ttu-id="bb8ce-167">Kommandon</span><span class="sxs-lookup"><span data-stu-id="bb8ce-167">Commands</span></span>
* <span data-ttu-id="bb8ce-168">hjälp: Visar hello stöds kommandon och flaggor.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-168">help: Lists hello supported commands and flags.</span></span>
* <span data-ttu-id="bb8ce-169">avetablering: försök tooclean hello system och gör den lämplig för etablering igen.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-169">deprovision: Attempt tooclean hello system and make it suitable for re-provisioning.</span></span> <span data-ttu-id="bb8ce-170">Den här åtgärden tas bort hello följande:</span><span class="sxs-lookup"><span data-stu-id="bb8ce-170">This operation deleted hello following:</span></span>
  
  * <span data-ttu-id="bb8ce-171">Alla SSH-värdnycklar (om Provisioning.RegenerateSshHostKeyPair är ”y” i konfigurationsfilen för hello)</span><span class="sxs-lookup"><span data-stu-id="bb8ce-171">All SSH host keys (if Provisioning.RegenerateSshHostKeyPair is 'y' in hello configuration file)</span></span>
  * <span data-ttu-id="bb8ce-172">NameServer konfigurationen i /etc/resolv.conf</span><span class="sxs-lookup"><span data-stu-id="bb8ce-172">Nameserver configuration in /etc/resolv.conf</span></span>
  * <span data-ttu-id="bb8ce-173">Rotlösenordet från/etc/Shadow (om Provisioning.DeleteRootPassword är ”y” i konfigurationsfilen för hello)</span><span class="sxs-lookup"><span data-stu-id="bb8ce-173">Root password from /etc/shadow (if Provisioning.DeleteRootPassword is 'y' in hello configuration file)</span></span>
  * <span data-ttu-id="bb8ce-174">Cachelagrade DHCP-klientlån</span><span class="sxs-lookup"><span data-stu-id="bb8ce-174">Cached DHCP client leases</span></span>
  * <span data-ttu-id="bb8ce-175">Återställer värd namn toolocalhost.localdomain</span><span class="sxs-lookup"><span data-stu-id="bb8ce-175">Resets host name toolocalhost.localdomain</span></span>

> [!WARNING]
> <span data-ttu-id="bb8ce-176">Avetablering garanterar inte hello avbildningen är avmarkerad av all känslig information och lämpar sig för omfördelning.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-176">Deprovisioning does not guarantee that hello image is cleared of all sensitive information and suitable for redistribution.</span></span>
> 
> 

* <span data-ttu-id="bb8ce-177">avetablering + användare: utför allt under - deprovision (ovan) och även tar bort hello senaste etablerade användarkonto (hämtades från /var/lib/waagent) och tillhörande data.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-177">deprovision+user: Performs everything under -deprovision (above) and also deletes hello last provisioned user account (obtained from /var/lib/waagent) and associated data.</span></span> <span data-ttu-id="bb8ce-178">Den här parametern är att etablera en avbildning som tidigare etablering i Azure så att den kan samlas in och används igen.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-178">This parameter is when de-provisioning an image that was previously provisioning on Azure so it may be captured and re-used.</span></span>
* <span data-ttu-id="bb8ce-179">version: Visar hello version av waagent</span><span class="sxs-lookup"><span data-stu-id="bb8ce-179">version: Displays hello version of waagent</span></span>
* <span data-ttu-id="bb8ce-180">serialconsole: konfigurerar GRUB toomark ttyS0 (hello första serieport) som hello Start konsol.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-180">serialconsole: Configures GRUB toomark ttyS0 (hello first serial port) as hello boot console.</span></span> <span data-ttu-id="bb8ce-181">Detta säkerställer att kernel Autostart loggar skickas toothe seriell port och tillgängliga för felsökning.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-181">This ensures that kernel bootup logs are sent toothe serial port and made available for debugging.</span></span>
* <span data-ttu-id="bb8ce-182">Daemon: kört waagent som en daemon toomanage interaktion med hello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-182">daemon: Run waagent as a daemon toomanage interaction with hello platform.</span></span> <span data-ttu-id="bb8ce-183">Det här argumentet är angivna toowaagent i hello waagent init-skriptet.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-183">This argument is specified toowaagent in hello waagent init script.</span></span>
* <span data-ttu-id="bb8ce-184">Starta: kört waagent i bakgrunden</span><span class="sxs-lookup"><span data-stu-id="bb8ce-184">start: Run waagent as a background process</span></span>

## <a name="configuration"></a><span data-ttu-id="bb8ce-185">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="bb8ce-185">Configuration</span></span>
<span data-ttu-id="bb8ce-186">En konfigurationsfil (/ etc/waagent.conf) kontroller hello åtgärder för waagent.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-186">A configuration file (/etc/waagent.conf) controls hello actions of waagent.</span></span> <span data-ttu-id="bb8ce-187">En exempelkonfigurationsfilen visas nedan:</span><span class="sxs-lookup"><span data-stu-id="bb8ce-187">A sample configuration file is shown below:</span></span>

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

<span data-ttu-id="bb8ce-188">hello olika konfigurationsalternativ beskrivs i detalj nedan.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-188">hello various configuration options are described in detail below.</span></span> <span data-ttu-id="bb8ce-189">Konfigurationsalternativ som är av tre typer. Boolesk, sträng eller heltal.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-189">Configuration options are of three types; Boolean, String or Integer.</span></span> <span data-ttu-id="bb8ce-190">hello booleskt konfigurationsalternativ kan anges som ”y” eller ”n”.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-190">hello Boolean configuration options can be specified as "y" or "n".</span></span> <span data-ttu-id="bb8ce-191">Hej särskilda nyckelordet ”None” kan användas för vissa sträng typen konfigurationsposter enligt anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-191">hello special keyword "None" may be used for some string type configuration entries as detailed below.</span></span>

<span data-ttu-id="bb8ce-192">**Provisioning.Enabled:**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-192">**Provisioning.Enabled:**</span></span>  
<span data-ttu-id="bb8ce-193">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="bb8ce-193">Type: Boolean</span></span>  
<span data-ttu-id="bb8ce-194">Standard: y</span><span class="sxs-lookup"><span data-stu-id="bb8ce-194">Default: y</span></span>

<span data-ttu-id="bb8ce-195">Detta gör att hello användaren tooenable eller inaktivera hello etablering funktioner i hello agent.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-195">This allows hello user tooenable or disable hello provisioning functionality in hello agent.</span></span> <span data-ttu-id="bb8ce-196">Giltiga värden är ”y” eller ”n”.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-196">Valid values are "y" or "n".</span></span> <span data-ttu-id="bb8ce-197">Om etablering inaktiveras SSH-värden och användaren nycklar hello bild bevaras och en konfiguration som angetts i hello Azure etablering API ignoreras.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-197">If provisioning is disabled, SSH host and user keys in hello image are preserved and any configuration specified in hello Azure provisioning API is ignored.</span></span>

> [!NOTE]
> <span data-ttu-id="bb8ce-198">Hej `Provisioning.Enabled` som standard för ”n” på Ubuntu molnet bilder som använder molnet init för etablering.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-198">hello `Provisioning.Enabled` parameter defaults too"n" on Ubuntu Cloud Images that use cloud-init for provisioning.</span></span>
> 
> 

<span data-ttu-id="bb8ce-199">**Provisioning.DeleteRootPassword:**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-199">**Provisioning.DeleteRootPassword:**</span></span>  
<span data-ttu-id="bb8ce-200">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="bb8ce-200">Type: Boolean</span></span>  
<span data-ttu-id="bb8ce-201">Standard: n</span><span class="sxs-lookup"><span data-stu-id="bb8ce-201">Default: n</span></span>

<span data-ttu-id="bb8ce-202">Om mängd hello rotlösenordet i filen hello/etc/shadow raderas under hello etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-202">If set, hello root password in hello /etc/shadow file is erased during hello provisioning process.</span></span>

<span data-ttu-id="bb8ce-203">**Provisioning.RegenerateSshHostKeyPair:**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-203">**Provisioning.RegenerateSshHostKeyPair:**</span></span>  
<span data-ttu-id="bb8ce-204">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="bb8ce-204">Type: Boolean</span></span>  
<span data-ttu-id="bb8ce-205">Standard: y</span><span class="sxs-lookup"><span data-stu-id="bb8ce-205">Default: y</span></span>

<span data-ttu-id="bb8ce-206">Om mängd alla SSH värden nyckelpar (ecdsa, dsa och rsa) tas bort under hello etableringsprocessen från/etc/ssh /.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-206">If set, all SSH host key pairs (ecdsa, dsa and rsa) are deleted during hello provisioning process from /etc/ssh/.</span></span> <span data-ttu-id="bb8ce-207">Och en enda ny nyckelpar genereras.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-207">And a single fresh key pair is generated.</span></span>

<span data-ttu-id="bb8ce-208">hello krypteringstyp för hello ny nyckelpar kan konfigureras av hello Provisioning.SshHostKeyPairType post.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-208">hello encryption type for hello fresh key pair is configurable by hello Provisioning.SshHostKeyPairType entry.</span></span> <span data-ttu-id="bb8ce-209">Observera att vissa distributioner återskapas SSH-nyckelpar för valfri krypteringstyp som saknas när hello SSH-daemon startas (till exempel vid en omstart).</span><span class="sxs-lookup"><span data-stu-id="bb8ce-209">Please note that some distributions will re-create SSH key pairs for any missing encryption types when hello SSH daemon is restarted (for example, upon a reboot).</span></span>

<span data-ttu-id="bb8ce-210">**Provisioning.SshHostKeyPairType:**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-210">**Provisioning.SshHostKeyPairType:**</span></span>  
<span data-ttu-id="bb8ce-211">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="bb8ce-211">Type: String</span></span>  
<span data-ttu-id="bb8ce-212">Standard: rsa</span><span class="sxs-lookup"><span data-stu-id="bb8ce-212">Default: rsa</span></span>

<span data-ttu-id="bb8ce-213">Detta kan ställas in tooan algoritmen krypteringstyp som stöds av hello SSH-daemon på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-213">This can be set tooan encryption algorithm type that is supported by hello SSH daemon on hello virtual machine.</span></span> <span data-ttu-id="bb8ce-214">hello stöds vanligtvis värden är ”rsa”, ”dsa” och ”ecdsa”.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-214">hello typically supported values are "rsa", "dsa" and "ecdsa".</span></span> <span data-ttu-id="bb8ce-215">Observera att ”putty.exe” i Windows inte har stöd för ”ecdsa”.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-215">Note that "putty.exe" on Windows does not support "ecdsa".</span></span> <span data-ttu-id="bb8ce-216">Så om du planerar toouse putty.exe på Windows tooconnect tooa Linux-distribution, Använd ”rsa” eller ”dsa”.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-216">So, if you intend toouse putty.exe on Windows tooconnect tooa Linux deployment, please use "rsa" or "dsa".</span></span>

<span data-ttu-id="bb8ce-217">**Provisioning.MonitorHostName:**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-217">**Provisioning.MonitorHostName:**</span></span>  
<span data-ttu-id="bb8ce-218">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="bb8ce-218">Type: Boolean</span></span>  
<span data-ttu-id="bb8ce-219">Standard: y</span><span class="sxs-lookup"><span data-stu-id="bb8ce-219">Default: y</span></span>

<span data-ttu-id="bb8ce-220">Om waagent ska övervaka hello virtuell Linux-dator för hostname ändringar (som returneras av kommandot för hello ”hostname”) och automatiskt uppdatera hello nätverkskonfigurationen i hello avbildningen tooreflect hello ändra.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-220">If set, waagent will monitor hello Linux virtual machine for hostname changes (as returned by hello "hostname" command) and automatically update hello networking configuration in hello image tooreflect hello change.</span></span> <span data-ttu-id="bb8ce-221">I ordning toopush hello namn ändra toohello DNS-servrar, nätverk kommer att startas om i hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-221">In order toopush hello name change toohello DNS servers, networking will be restarted in hello virtual machine.</span></span> <span data-ttu-id="bb8ce-222">Detta resulterar i korthet förlust av Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-222">This will result in brief loss of Internet connectivity.</span></span>

<span data-ttu-id="bb8ce-223">**Provisioning.DecodeCustomData**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-223">**Provisioning.DecodeCustomData**</span></span>  
<span data-ttu-id="bb8ce-224">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="bb8ce-224">Type: Boolean</span></span>  
<span data-ttu-id="bb8ce-225">Standard: n</span><span class="sxs-lookup"><span data-stu-id="bb8ce-225">Default: n</span></span>

<span data-ttu-id="bb8ce-226">Om har angetts waagent avkodar CustomData från Base64.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-226">If set, waagent will decode CustomData from Base64.</span></span>

<span data-ttu-id="bb8ce-227">**Provisioning.ExecuteCustomData**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-227">**Provisioning.ExecuteCustomData**</span></span>  
<span data-ttu-id="bb8ce-228">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="bb8ce-228">Type: Boolean</span></span>  
<span data-ttu-id="bb8ce-229">Standard: n</span><span class="sxs-lookup"><span data-stu-id="bb8ce-229">Default: n</span></span>

<span data-ttu-id="bb8ce-230">Om det har angetts waagent körs CustomData efter etableringen.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-230">If set, waagent will execute CustomData after provisioning.</span></span>

<span data-ttu-id="bb8ce-231">**Provisioning.PasswordCryptId**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-231">**Provisioning.PasswordCryptId**</span></span>  
<span data-ttu-id="bb8ce-232">Typ: sträng</span><span class="sxs-lookup"><span data-stu-id="bb8ce-232">Type:String</span></span>  
<span data-ttu-id="bb8ce-233">Standard: 6</span><span class="sxs-lookup"><span data-stu-id="bb8ce-233">Default:6</span></span>

<span data-ttu-id="bb8ce-234">Algoritm som används av crypt vid generering av lösenords-hash.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-234">Algorithm used by crypt when generating password hash.</span></span>  
 <span data-ttu-id="bb8ce-235">1 - MD5</span><span class="sxs-lookup"><span data-stu-id="bb8ce-235">1 - MD5</span></span>  
 <span data-ttu-id="bb8ce-236">2a - Blowfish</span><span class="sxs-lookup"><span data-stu-id="bb8ce-236">2a - Blowfish</span></span>  
 <span data-ttu-id="bb8ce-237">5 - SHA-256</span><span class="sxs-lookup"><span data-stu-id="bb8ce-237">5 - SHA-256</span></span>  
 <span data-ttu-id="bb8ce-238">6 - SHA-512</span><span class="sxs-lookup"><span data-stu-id="bb8ce-238">6 - SHA-512</span></span>  

<span data-ttu-id="bb8ce-239">**Provisioning.PasswordCryptSaltLength**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-239">**Provisioning.PasswordCryptSaltLength**</span></span>  
<span data-ttu-id="bb8ce-240">Typ: sträng</span><span class="sxs-lookup"><span data-stu-id="bb8ce-240">Type:String</span></span>  
<span data-ttu-id="bb8ce-241">Standard: 10</span><span class="sxs-lookup"><span data-stu-id="bb8ce-241">Default:10</span></span>

<span data-ttu-id="bb8ce-242">Längden på slumpmässiga salt som används vid generering av lösenords-hash.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-242">Length of random salt used when generating password hash.</span></span>

<span data-ttu-id="bb8ce-243">**ResourceDisk.Format:**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-243">**ResourceDisk.Format:**</span></span>  
<span data-ttu-id="bb8ce-244">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="bb8ce-244">Type: Boolean</span></span>  
<span data-ttu-id="bb8ce-245">Standard: y</span><span class="sxs-lookup"><span data-stu-id="bb8ce-245">Default: y</span></span>

<span data-ttu-id="bb8ce-246">Om har angetts hello resurs disketten hello plattform formateras och monteras med waagent om hello filesystem typen som begärts av hello användare i ”ResourceDisk.Filesystem” är något annat än ”ntfs”.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-246">If set, hello resource disk provided by hello platform will be formatted and mounted by waagent if hello filesystem type requested by hello user in "ResourceDisk.Filesystem" is anything other than "ntfs".</span></span> <span data-ttu-id="bb8ce-247">En partition av typen Linux (83) görs tillgänglig hello disk.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-247">A single partition of type Linux (83) will be made available on hello disk.</span></span> <span data-ttu-id="bb8ce-248">Observera att den här partitionen inte formateras om den monteras har.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-248">Note that this partition will not be formatted if it can be successfully mounted.</span></span>

<span data-ttu-id="bb8ce-249">**ResourceDisk.Filesystem:**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-249">**ResourceDisk.Filesystem:**</span></span>  
<span data-ttu-id="bb8ce-250">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="bb8ce-250">Type: String</span></span>  
<span data-ttu-id="bb8ce-251">Standard: ext4</span><span class="sxs-lookup"><span data-stu-id="bb8ce-251">Default: ext4</span></span>

<span data-ttu-id="bb8ce-252">Detta anger hello filesystem hello resurs disk.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-252">This specifies hello filesystem type for hello resource disk.</span></span> <span data-ttu-id="bb8ce-253">Värden som stöds beror på Linux-distribution.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-253">Supported values vary by Linux distribution.</span></span> <span data-ttu-id="bb8ce-254">Om hello X, sedan mkfs. X ska finnas på hello Linux avbildningen.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-254">If hello string is X, then mkfs.X should be present on hello Linux image.</span></span> <span data-ttu-id="bb8ce-255">SLES 11 bilder vanligtvis använda 'ext3'.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-255">SLES 11 images should typically use 'ext3'.</span></span> <span data-ttu-id="bb8ce-256">FreeBSD bilder ska använda 'ufs2' här.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-256">FreeBSD images should use 'ufs2' here.</span></span>

<span data-ttu-id="bb8ce-257">**ResourceDisk.MountPoint:**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-257">**ResourceDisk.MountPoint:**</span></span>  
<span data-ttu-id="bb8ce-258">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="bb8ce-258">Type: String</span></span>  
<span data-ttu-id="bb8ce-259">Standard: / mnt/resurs</span><span class="sxs-lookup"><span data-stu-id="bb8ce-259">Default: /mnt/resource</span></span> 

<span data-ttu-id="bb8ce-260">Detta anger Hej då hello resurs disken är monterad.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-260">This specifies hello path at which hello resource disk is mounted.</span></span> <span data-ttu-id="bb8ce-261">Observera att hello resurs disken är en *tillfälliga* disk och kan tömmas när hello VM är avetablerade.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-261">Note that hello resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span>

<span data-ttu-id="bb8ce-262">**ResourceDisk.MountOptions**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-262">**ResourceDisk.MountOptions**</span></span>  
<span data-ttu-id="bb8ce-263">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="bb8ce-263">Type: String</span></span>  
<span data-ttu-id="bb8ce-264">Standard: ingen</span><span class="sxs-lookup"><span data-stu-id="bb8ce-264">Default: None</span></span>

<span data-ttu-id="bb8ce-265">Anger disk montera toobe skickades toohello mount -o alternativ.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-265">Specifies disk mount options toobe passed toohello mount -o command.</span></span> <span data-ttu-id="bb8ce-266">Detta är en kommaavgränsad lista med värden, t.ex.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-266">This is a comma separated list of values, ex.</span></span> <span data-ttu-id="bb8ce-267">'nodev nosuid'.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-267">'nodev,nosuid'.</span></span> <span data-ttu-id="bb8ce-268">Se mount(8) mer information.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-268">See mount(8) for details.</span></span>

<span data-ttu-id="bb8ce-269">**ResourceDisk.EnableSwap:**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-269">**ResourceDisk.EnableSwap:**</span></span>  
<span data-ttu-id="bb8ce-270">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="bb8ce-270">Type: Boolean</span></span>  
<span data-ttu-id="bb8ce-271">Standard: n</span><span class="sxs-lookup"><span data-stu-id="bb8ce-271">Default: n</span></span>

<span data-ttu-id="bb8ce-272">Om ange en växlingsfil (/ swapfile) skapas på disken för hello resurs och läggs toohello system växlingsutrymme.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-272">If set, a swap file (/swapfile) is created on hello resource disk and added toohello system swap space.</span></span>

<span data-ttu-id="bb8ce-273">**ResourceDisk.SwapSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-273">**ResourceDisk.SwapSizeMB:**</span></span>  
<span data-ttu-id="bb8ce-274">Typ: heltal</span><span class="sxs-lookup"><span data-stu-id="bb8ce-274">Type: Integer</span></span>  
<span data-ttu-id="bb8ce-275">Standard: 0</span><span class="sxs-lookup"><span data-stu-id="bb8ce-275">Default: 0</span></span>

<span data-ttu-id="bb8ce-276">hello storleken på växlingsfilen för hello i megabyte.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-276">hello size of hello swap file in megabytes.</span></span>

<span data-ttu-id="bb8ce-277">**Logs.Verbose:**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-277">**Logs.Verbose:**</span></span>  
<span data-ttu-id="bb8ce-278">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="bb8ce-278">Type: Boolean</span></span>  
<span data-ttu-id="bb8ce-279">Standard: n</span><span class="sxs-lookup"><span data-stu-id="bb8ce-279">Default: n</span></span>

<span data-ttu-id="bb8ce-280">Om mängd loggen detaljnivå förstärks.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-280">If set, log verbosity is boosted.</span></span> <span data-ttu-id="bb8ce-281">Waagent loggar too/var/log/waagent.log och utnyttjar hello logrotate funktioner toorotate systemloggar.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-281">Waagent logs too/var/log/waagent.log and leverages hello system logrotate functionality toorotate logs.</span></span>

<span data-ttu-id="bb8ce-282">**OS. EnableRDMA**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-282">**OS.EnableRDMA**</span></span>  
<span data-ttu-id="bb8ce-283">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="bb8ce-283">Type: Boolean</span></span>  
<span data-ttu-id="bb8ce-284">Standard: n</span><span class="sxs-lookup"><span data-stu-id="bb8ce-284">Default: n</span></span>

<span data-ttu-id="bb8ce-285">Om hello agent kommer försök tooinstall och läsa in en RDMA kernel-drivrutinen som matchar hello-versionen av den inbyggda programvaran hello på hello underliggande maskinvara.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-285">If set, hello agent will attempt tooinstall and then load an RDMA kernel driver that matches hello version of hello firmware on hello underlying hardware.</span></span>

<span data-ttu-id="bb8ce-286">**OS. RootDeviceScsiTimeout:**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-286">**OS.RootDeviceScsiTimeout:**</span></span>  
<span data-ttu-id="bb8ce-287">Typ: heltal</span><span class="sxs-lookup"><span data-stu-id="bb8ce-287">Type: Integer</span></span>  
<span data-ttu-id="bb8ce-288">Standard: 300</span><span class="sxs-lookup"><span data-stu-id="bb8ce-288">Default: 300</span></span>

<span data-ttu-id="bb8ce-289">Detta konfigurerar hello SCSI-tidsgräns i sekunder för hello OS-disk- och enheter.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-289">This configures hello SCSI timeout in seconds on hello OS disk and data drives.</span></span> <span data-ttu-id="bb8ce-290">Om inte ange hello system som standard.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-290">If not set, hello system defaults are used.</span></span>

<span data-ttu-id="bb8ce-291">**OS. OpensslPath:**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-291">**OS.OpensslPath:**</span></span>  
<span data-ttu-id="bb8ce-292">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="bb8ce-292">Type: String</span></span>  
<span data-ttu-id="bb8ce-293">Standard: ingen</span><span class="sxs-lookup"><span data-stu-id="bb8ce-293">Default: None</span></span>

<span data-ttu-id="bb8ce-294">Detta kan vara används toospecify en alternativ sökväg för hello openssl binära toouse för kryptografiska åtgärder.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-294">This can be used toospecify an alternate path for hello openssl binary toouse for cryptographic operations.</span></span>

<span data-ttu-id="bb8ce-295">**HttpProxy.Host HttpProxy.Port**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-295">**HttpProxy.Host, HttpProxy.Port**</span></span>  
<span data-ttu-id="bb8ce-296">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="bb8ce-296">Type: String</span></span>  
<span data-ttu-id="bb8ce-297">Standard: ingen</span><span class="sxs-lookup"><span data-stu-id="bb8ce-297">Default: None</span></span>

<span data-ttu-id="bb8ce-298">Om har angetts hello agenten ska använda denna proxy server tooaccess hello internet.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-298">If set, hello agent will use this proxy server tooaccess hello internet.</span></span> 

## <a name="ubuntu-cloud-images"></a><span data-ttu-id="bb8ce-299">Ubuntu molnet bilder</span><span class="sxs-lookup"><span data-stu-id="bb8ce-299">Ubuntu Cloud Images</span></span>
<span data-ttu-id="bb8ce-300">Observera att använda Ubuntu molnet bilder [moln init](https://launchpad.net/ubuntu/+source/cloud-init) tooperform många konfigurationsåtgärder som annars skulle hanteras av hello Azure Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-300">Note that Ubuntu Cloud Images utilize [cloud-init](https://launchpad.net/ubuntu/+source/cloud-init) tooperform many configuration tasks that would otherwise be managed by hello Azure Linux Agent.</span></span>  <span data-ttu-id="bb8ce-301">Observera hello följande skillnader:</span><span class="sxs-lookup"><span data-stu-id="bb8ce-301">Please note hello following differences:</span></span>

* <span data-ttu-id="bb8ce-302">**Provisioning.Enabled** standardvärden för ”n” på Ubuntu molnet bilder som använder molnet init tooperform etablering uppgifter.</span><span class="sxs-lookup"><span data-stu-id="bb8ce-302">**Provisioning.Enabled** defaults too"n" on Ubuntu Cloud Images that use cloud-init tooperform provisioning tasks.</span></span>
* <span data-ttu-id="bb8ce-303">hello följande konfigurationsparametrar som påverkar inte Ubuntu molnet bilder som använder molnet init toomanage hello resurs disk och växla utrymme:</span><span class="sxs-lookup"><span data-stu-id="bb8ce-303">hello following configuration parameters have no effect on Ubuntu Cloud Images that use cloud-init toomanage hello resource disk and swap space:</span></span>
  
  * <span data-ttu-id="bb8ce-304">**ResourceDisk.Format**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-304">**ResourceDisk.Format**</span></span>
  * <span data-ttu-id="bb8ce-305">**ResourceDisk.Filesystem**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-305">**ResourceDisk.Filesystem**</span></span>
  * <span data-ttu-id="bb8ce-306">**ResourceDisk.MountPoint**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-306">**ResourceDisk.MountPoint**</span></span>
  * <span data-ttu-id="bb8ce-307">**ResourceDisk.EnableSwap**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-307">**ResourceDisk.EnableSwap**</span></span>
  * <span data-ttu-id="bb8ce-308">**ResourceDisk.SwapSizeMB**</span><span class="sxs-lookup"><span data-stu-id="bb8ce-308">**ResourceDisk.SwapSizeMB**</span></span>
* <span data-ttu-id="bb8ce-309">Se följande resurser tooconfigure hello resurs disk monteringspunkt hello och växlingsutrymme på Ubuntu molnet bilder under etablering:</span><span class="sxs-lookup"><span data-stu-id="bb8ce-309">Please see hello following resources tooconfigure hello resource disk mount point and swap space on Ubuntu Cloud Images during provisioning:</span></span>
  
  * [<span data-ttu-id="bb8ce-310">Ubuntu Wiki: Konfigurera växlingen partitioner</span><span class="sxs-lookup"><span data-stu-id="bb8ce-310">Ubuntu Wiki: Configure Swap Partitions</span></span>](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [<span data-ttu-id="bb8ce-311">Placera anpassade Data till en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="bb8ce-311">Injecting Custom Data into an Azure Virtual Machine</span></span>](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

