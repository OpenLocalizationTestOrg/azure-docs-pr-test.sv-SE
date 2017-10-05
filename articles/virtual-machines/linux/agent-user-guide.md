---
title: "Översikt över Azure virtuell Linux-dator | Microsoft Docs"
description: "Lär dig hur du installerar och konfigurerar Linux-agenten (waagent) för att hantera den virtuella datorns interaktion med Azure-Infrastrukturkontrollanten."
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
ms.openlocfilehash: 486ad6bb148583a957fb82b7954ff94f853b12cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="understanding-and-using-the-azure-linux-agent"></a><span data-ttu-id="9bbeb-103">Förstå och använda Azure Linux-agenten</span><span class="sxs-lookup"><span data-stu-id="9bbeb-103">Understanding and using the Azure Linux Agent</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a><span data-ttu-id="9bbeb-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="9bbeb-104">Introduction</span></span>
<span data-ttu-id="9bbeb-105">Microsoft Azure Linux-agenten (waagent) hanterar Linux och FreeBSD etablering och VM-interaktion med Azure-Infrastrukturkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-105">The Microsoft Azure Linux Agent (waagent) manages Linux & FreeBSD provisioning, and VM interaction with the Azure Fabric Controller.</span></span> <span data-ttu-id="9bbeb-106">Det innehåller följande funktioner för Linux och FreeBSD IaaS distributioner:</span><span class="sxs-lookup"><span data-stu-id="9bbeb-106">It provides the following functionality for Linux and FreeBSD IaaS deployments:</span></span>

> [!NOTE]
> <span data-ttu-id="9bbeb-107">Se Azure Linux-agent [viktigt](https://github.com/Azure/WALinuxAgent/blob/master/README.md) för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-107">See the Azure Linux agent [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md) for additional details.</span></span>
> 
> 

* <span data-ttu-id="9bbeb-108">**Bild etablering**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-108">**Image Provisioning**</span></span>
  
  * <span data-ttu-id="9bbeb-109">Skapa ett användarkonto</span><span class="sxs-lookup"><span data-stu-id="9bbeb-109">Creation of a user account</span></span>
  * <span data-ttu-id="9bbeb-110">Konfigurera typer av SSH-autentisering</span><span class="sxs-lookup"><span data-stu-id="9bbeb-110">Configuring SSH authentication types</span></span>
  * <span data-ttu-id="9bbeb-111">Distribution av offentliga SSH-nycklar och nyckelpar</span><span class="sxs-lookup"><span data-stu-id="9bbeb-111">Deployment of SSH public keys and key pairs</span></span>
  * <span data-ttu-id="9bbeb-112">Ange värdnamnet</span><span class="sxs-lookup"><span data-stu-id="9bbeb-112">Setting the host name</span></span>
  * <span data-ttu-id="9bbeb-113">Publicera värdnamnet för DNS-plattformen</span><span class="sxs-lookup"><span data-stu-id="9bbeb-113">Publishing the host name to the platform DNS</span></span>
  * <span data-ttu-id="9bbeb-114">Rapportering nyckel för SSH-värden för plattformen</span><span class="sxs-lookup"><span data-stu-id="9bbeb-114">Reporting SSH host key fingerprint to the platform</span></span>
  * <span data-ttu-id="9bbeb-115">Disk resurshantering</span><span class="sxs-lookup"><span data-stu-id="9bbeb-115">Resource Disk Management</span></span>
  * <span data-ttu-id="9bbeb-116">Formatering och monterar resurs-disk</span><span class="sxs-lookup"><span data-stu-id="9bbeb-116">Formatting and mounting the resource disk</span></span>
  * <span data-ttu-id="9bbeb-117">Konfigurera växlingsutrymme</span><span class="sxs-lookup"><span data-stu-id="9bbeb-117">Configuring swap space</span></span>
* <span data-ttu-id="9bbeb-118">**Nätverk**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-118">**Networking**</span></span>
  
  * <span data-ttu-id="9bbeb-119">Hanterar vägar för att förbättra kompatibiliteten med plattformen DHCP-servrar</span><span class="sxs-lookup"><span data-stu-id="9bbeb-119">Manages routes to improve compatibility with platform DHCP servers</span></span>
  * <span data-ttu-id="9bbeb-120">Garanterar stabiliteten för nätverksgränssnittets namn</span><span class="sxs-lookup"><span data-stu-id="9bbeb-120">Ensures the stability of the network interface name</span></span>
* <span data-ttu-id="9bbeb-121">**Kernel**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-121">**Kernel**</span></span>
  
  * <span data-ttu-id="9bbeb-122">Konfigurerar virtuell NUMA (inaktivera för kernel < 2.6.37)</span><span class="sxs-lookup"><span data-stu-id="9bbeb-122">Configures virtual NUMA (disable for kernel <2.6.37)</span></span>
  * <span data-ttu-id="9bbeb-123">Förbrukar Hyper-V entropi för /dev/random</span><span class="sxs-lookup"><span data-stu-id="9bbeb-123">Consumes Hyper-V entropy for /dev/random</span></span>
  * <span data-ttu-id="9bbeb-124">Konfigurerar SCSI tidsgränser för rotenheten (som kan vara fjärråtkomst)</span><span class="sxs-lookup"><span data-stu-id="9bbeb-124">Configures SCSI timeouts for the root device (which could be remote)</span></span>
* <span data-ttu-id="9bbeb-125">**Diagnostik**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-125">**Diagnostics**</span></span>
  
  * <span data-ttu-id="9bbeb-126">Omdirigering av konsol till seriell port</span><span class="sxs-lookup"><span data-stu-id="9bbeb-126">Console redirection to the serial port</span></span>
* <span data-ttu-id="9bbeb-127">**SCVMM-distributioner**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-127">**SCVMM Deployments**</span></span>
  
  * <span data-ttu-id="9bbeb-128">Identifierar och startar VMM-agenten för Linux vid körning i en miljö med System Center Virtual Machine Manager 2012 R2</span><span class="sxs-lookup"><span data-stu-id="9bbeb-128">Detects and bootstraps the VMM agent for Linux when running in a System Center Virtual Machine Manager 2012 R2 environment</span></span>
* <span data-ttu-id="9bbeb-129">**Tillägg för virtuell dator**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-129">**VM Extension**</span></span>
  
  * <span data-ttu-id="9bbeb-130">Mata in komponenten som skapats av Microsoft och Partners i Linux VM (IaaS) till att programvaran och konfiguration automation</span><span class="sxs-lookup"><span data-stu-id="9bbeb-130">Inject component authored by Microsoft and Partners into Linux VM (IaaS) to enable software and configuration automation</span></span>
  * <span data-ttu-id="9bbeb-131">VM-tillägget referensimplementering på [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)</span><span class="sxs-lookup"><span data-stu-id="9bbeb-131">VM Extension reference implementation on [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)</span></span>

## <a name="communication"></a><span data-ttu-id="9bbeb-132">Kommunikation</span><span class="sxs-lookup"><span data-stu-id="9bbeb-132">Communication</span></span>
<span data-ttu-id="9bbeb-133">Informationsflödet från plattform till agenten sker via två kanaler:</span><span class="sxs-lookup"><span data-stu-id="9bbeb-133">The information flow from the platform to the agent occurs via two channels:</span></span>

* <span data-ttu-id="9bbeb-134">En starten kopplade DVD för IaaS-distributioner.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-134">A boot-time attached DVD for IaaS deployments.</span></span> <span data-ttu-id="9bbeb-135">DVD-skivan innehåller ett OVF-kompatibel konfigurationsfil som innehåller alla Etableringsinformation än den faktiska SSH-keypairs.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-135">This DVD includes an OVF-compliant configuration file that includes all provisioning information other than the actual SSH keypairs.</span></span>
* <span data-ttu-id="9bbeb-136">En TCP-slutpunkt som Exponerar en REST-API för att hämta distribution och konfiguration av topologi.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-136">A TCP endpoint exposing a REST API used to obtain deployment and topology configuration.</span></span>

## <a name="requirements"></a><span data-ttu-id="9bbeb-137">Krav</span><span class="sxs-lookup"><span data-stu-id="9bbeb-137">Requirements</span></span>
<span data-ttu-id="9bbeb-138">Följande datorer har testats och är känt att arbeta med Azure Linux-agenten:</span><span class="sxs-lookup"><span data-stu-id="9bbeb-138">The following systems have been tested and are known to work with the Azure Linux Agent:</span></span>

> [!NOTE]
> <span data-ttu-id="9bbeb-139">Den här listan kan skilja sig från officiella listan över operativsystem som stöds på Microsoft Azure-plattformen som beskrivs här: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)</span><span class="sxs-lookup"><span data-stu-id="9bbeb-139">This list may differ from the official list of supported systems on the Microsoft Azure Platform, as described here: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)</span></span>
> 
> 

* <span data-ttu-id="9bbeb-140">CoreOS</span><span class="sxs-lookup"><span data-stu-id="9bbeb-140">CoreOS</span></span>
* <span data-ttu-id="9bbeb-141">CentOS 6.3 +</span><span class="sxs-lookup"><span data-stu-id="9bbeb-141">CentOS 6.3+</span></span>
* <span data-ttu-id="9bbeb-142">Red Hat Enterprise Linux 6.7 +</span><span class="sxs-lookup"><span data-stu-id="9bbeb-142">Red Hat Enterprise Linux 6.7+</span></span>
* <span data-ttu-id="9bbeb-143">Debian 7.0 +</span><span class="sxs-lookup"><span data-stu-id="9bbeb-143">Debian 7.0+</span></span>
* <span data-ttu-id="9bbeb-144">Ubuntu 12.04 +</span><span class="sxs-lookup"><span data-stu-id="9bbeb-144">Ubuntu 12.04+</span></span>
* <span data-ttu-id="9bbeb-145">openSUSE 12.3 +</span><span class="sxs-lookup"><span data-stu-id="9bbeb-145">openSUSE 12.3+</span></span>
* <span data-ttu-id="9bbeb-146">SLES 11 SP3 +</span><span class="sxs-lookup"><span data-stu-id="9bbeb-146">SLES 11 SP3+</span></span>
* <span data-ttu-id="9bbeb-147">Oracle Linux 6.4 +</span><span class="sxs-lookup"><span data-stu-id="9bbeb-147">Oracle Linux 6.4+</span></span>

<span data-ttu-id="9bbeb-148">Andra operativsystem som stöds:</span><span class="sxs-lookup"><span data-stu-id="9bbeb-148">Other Supported Systems:</span></span>

* <span data-ttu-id="9bbeb-149">FreeBSD 10 + (Azure Linux-agenten v2.0.10 +)</span><span class="sxs-lookup"><span data-stu-id="9bbeb-149">FreeBSD 10+ (Azure Linux Agent v2.0.10+)</span></span>

<span data-ttu-id="9bbeb-150">Linux-agenten beror på vissa Systempaket för att fungera korrekt:</span><span class="sxs-lookup"><span data-stu-id="9bbeb-150">The Linux agent depends on some system packages in order to function properly:</span></span>

* <span data-ttu-id="9bbeb-151">Python 2.6 +</span><span class="sxs-lookup"><span data-stu-id="9bbeb-151">Python 2.6+</span></span>
* <span data-ttu-id="9bbeb-152">OpenSSL 1.0 +</span><span class="sxs-lookup"><span data-stu-id="9bbeb-152">OpenSSL 1.0+</span></span>
* <span data-ttu-id="9bbeb-153">OpenSSH 5.3 +</span><span class="sxs-lookup"><span data-stu-id="9bbeb-153">OpenSSH 5.3+</span></span>
* <span data-ttu-id="9bbeb-154">Verktyg för filsystem: sfdisk datorn mkfs, åtskilda</span><span class="sxs-lookup"><span data-stu-id="9bbeb-154">Filesystem utilities: sfdisk, fdisk, mkfs, parted</span></span>
* <span data-ttu-id="9bbeb-155">Lösenord verktyg: chpasswd sudo</span><span class="sxs-lookup"><span data-stu-id="9bbeb-155">Password tools: chpasswd, sudo</span></span>
* <span data-ttu-id="9bbeb-156">Text som bearbetar verktyg: sed grep</span><span class="sxs-lookup"><span data-stu-id="9bbeb-156">Text processing tools: sed, grep</span></span>
* <span data-ttu-id="9bbeb-157">Verktyg: IP-väg</span><span class="sxs-lookup"><span data-stu-id="9bbeb-157">Network tools: ip-route</span></span>
* <span data-ttu-id="9bbeb-158">Kernel stöd för att montera UDF-filsystem.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-158">Kernel support for mounting UDF filesystems.</span></span>

## <a name="installation"></a><span data-ttu-id="9bbeb-159">Installation</span><span class="sxs-lookup"><span data-stu-id="9bbeb-159">Installation</span></span>
<span data-ttu-id="9bbeb-160">Installation med en RPM eller ett DEB-paket från din distribution paketdatabasen är den bästa metoden för installation och uppgradering av Azure Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-160">Installation using an RPM or a DEB package from your distribution's package repository is the preferred method of installing and upgrading the Azure Linux Agent.</span></span> <span data-ttu-id="9bbeb-161">Alla de [godkända distribution providers](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) integrera Azure Linux-agenten i sina avbildningar och databaser.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-161">All the [endorsed distribution providers](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) integrate the Azure Linux agent package into their images and repositories.</span></span>

<span data-ttu-id="9bbeb-162">Dokumentationen i den [lagringsplatsen för Azure Linux-agenten på GitHub](https://github.com/Azure/WALinuxAgent) för avancerade installationsalternativ, till exempel installera från källan eller anpassade platser eller prefix.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-162">Refer to the documentation in the [Azure Linux Agent repo on GitHub](https://github.com/Azure/WALinuxAgent) for advanced installation options, such as installing from source or to custom locations or prefixes.</span></span>

## <a name="command-line-options"></a><span data-ttu-id="9bbeb-163">Kommandoradsalternativ</span><span class="sxs-lookup"><span data-stu-id="9bbeb-163">Command Line Options</span></span>
### <a name="flags"></a><span data-ttu-id="9bbeb-164">Flaggor</span><span class="sxs-lookup"><span data-stu-id="9bbeb-164">Flags</span></span>
* <span data-ttu-id="9bbeb-165">utförlig: öka mängden information i angivna kommandot</span><span class="sxs-lookup"><span data-stu-id="9bbeb-165">verbose: Increase verbosity of specified command</span></span>
* <span data-ttu-id="9bbeb-166">tvinga: hoppa över interaktiva bekräftelse för vissa kommandon</span><span class="sxs-lookup"><span data-stu-id="9bbeb-166">force: Skip interactive confirmation for some commands</span></span>

### <a name="commands"></a><span data-ttu-id="9bbeb-167">Kommandon</span><span class="sxs-lookup"><span data-stu-id="9bbeb-167">Commands</span></span>
* <span data-ttu-id="9bbeb-168">hjälp: Listar de kommandon som stöds och flaggor.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-168">help: Lists the supported commands and flags.</span></span>
* <span data-ttu-id="9bbeb-169">avetablering: försök att rensa systemet och gör den lämplig för etablering igen.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-169">deprovision: Attempt to clean the system and make it suitable for re-provisioning.</span></span> <span data-ttu-id="9bbeb-170">Den här åtgärden ta bort följande:</span><span class="sxs-lookup"><span data-stu-id="9bbeb-170">This operation deleted the following:</span></span>
  
  * <span data-ttu-id="9bbeb-171">Alla SSH-värdnycklar (om Provisioning.RegenerateSshHostKeyPair är ”y” i konfigurationsfilen)</span><span class="sxs-lookup"><span data-stu-id="9bbeb-171">All SSH host keys (if Provisioning.RegenerateSshHostKeyPair is 'y' in the configuration file)</span></span>
  * <span data-ttu-id="9bbeb-172">NameServer konfigurationen i /etc/resolv.conf</span><span class="sxs-lookup"><span data-stu-id="9bbeb-172">Nameserver configuration in /etc/resolv.conf</span></span>
  * <span data-ttu-id="9bbeb-173">Rotlösenordet från/etc/Shadow (om Provisioning.DeleteRootPassword är ”y” i konfigurationsfilen)</span><span class="sxs-lookup"><span data-stu-id="9bbeb-173">Root password from /etc/shadow (if Provisioning.DeleteRootPassword is 'y' in the configuration file)</span></span>
  * <span data-ttu-id="9bbeb-174">Cachelagrade DHCP-klientlån</span><span class="sxs-lookup"><span data-stu-id="9bbeb-174">Cached DHCP client leases</span></span>
  * <span data-ttu-id="9bbeb-175">Återställer värdnamnet till localhost.localdomain</span><span class="sxs-lookup"><span data-stu-id="9bbeb-175">Resets host name to localhost.localdomain</span></span>

> [!WARNING]
> <span data-ttu-id="9bbeb-176">Avetablering garanterar inte att avbildningen är avmarkerad av all känslig information och lämpar sig för omfördelning.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-176">Deprovisioning does not guarantee that the image is cleared of all sensitive information and suitable for redistribution.</span></span>
> 
> 

* <span data-ttu-id="9bbeb-177">avetablering + användare: utför allt under - deprovision (ovan) och även tar bort det senaste kontot för etablerad användare (hämtades från /var/lib/waagent) och tillhörande data.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-177">deprovision+user: Performs everything under -deprovision (above) and also deletes the last provisioned user account (obtained from /var/lib/waagent) and associated data.</span></span> <span data-ttu-id="9bbeb-178">Den här parametern är att etablera en avbildning som tidigare etablering i Azure så att den kan samlas in och används igen.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-178">This parameter is when de-provisioning an image that was previously provisioning on Azure so it may be captured and re-used.</span></span>
* <span data-ttu-id="9bbeb-179">version: Visar vilken version av waagent</span><span class="sxs-lookup"><span data-stu-id="9bbeb-179">version: Displays the version of waagent</span></span>
* <span data-ttu-id="9bbeb-180">serialconsole: konfigurerar GRUB om du vill markera ttyS0 (första seriell port) som start-konsol.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-180">serialconsole: Configures GRUB to mark ttyS0 (the first serial port) as the boot console.</span></span> <span data-ttu-id="9bbeb-181">Detta säkerställer att kernel Autostart loggar skickas till seriell port och tillgängliga för felsökning.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-181">This ensures that kernel bootup logs are sent to the serial port and made available for debugging.</span></span>
* <span data-ttu-id="9bbeb-182">Daemon: kört waagent som en daemon för att hantera interaktion med plattformen.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-182">daemon: Run waagent as a daemon to manage interaction with the platform.</span></span> <span data-ttu-id="9bbeb-183">Det här argumentet har angetts till waagent i skriptet waagent init.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-183">This argument is specified to waagent in the waagent init script.</span></span>
* <span data-ttu-id="9bbeb-184">Starta: kört waagent i bakgrunden</span><span class="sxs-lookup"><span data-stu-id="9bbeb-184">start: Run waagent as a background process</span></span>

## <a name="configuration"></a><span data-ttu-id="9bbeb-185">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="9bbeb-185">Configuration</span></span>
<span data-ttu-id="9bbeb-186">En konfigurationsfil (/ etc/waagent.conf) styr åtgärder för waagent.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-186">A configuration file (/etc/waagent.conf) controls the actions of waagent.</span></span> <span data-ttu-id="9bbeb-187">En exempelkonfigurationsfilen visas nedan:</span><span class="sxs-lookup"><span data-stu-id="9bbeb-187">A sample configuration file is shown below:</span></span>

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

<span data-ttu-id="9bbeb-188">Olika beskrivs i detalj nedan.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-188">The various configuration options are described in detail below.</span></span> <span data-ttu-id="9bbeb-189">Konfigurationsalternativ som är av tre typer. Boolesk, sträng eller heltal.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-189">Configuration options are of three types; Boolean, String or Integer.</span></span> <span data-ttu-id="9bbeb-190">Booleskt konfigurationsalternativen kan anges som ”y” eller ”n”.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-190">The Boolean configuration options can be specified as "y" or "n".</span></span> <span data-ttu-id="9bbeb-191">Särskilda nyckelordet ”None” kan användas för vissa sträng typen konfigurationsposter enligt anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-191">The special keyword "None" may be used for some string type configuration entries as detailed below.</span></span>

<span data-ttu-id="9bbeb-192">**Provisioning.Enabled:**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-192">**Provisioning.Enabled:**</span></span>  
<span data-ttu-id="9bbeb-193">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="9bbeb-193">Type: Boolean</span></span>  
<span data-ttu-id="9bbeb-194">Standard: y</span><span class="sxs-lookup"><span data-stu-id="9bbeb-194">Default: y</span></span>

<span data-ttu-id="9bbeb-195">Detta används att aktivera eller inaktivera funktionen etablering i agenten.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-195">This allows the user to enable or disable the provisioning functionality in the agent.</span></span> <span data-ttu-id="9bbeb-196">Giltiga värden är ”y” eller ”n”.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-196">Valid values are "y" or "n".</span></span> <span data-ttu-id="9bbeb-197">Om etablering inaktiveras SSH-värden och användaren nycklar i avbildningen bevaras och eventuella konfiguration som angetts i Azure API-etablering ignoreras.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-197">If provisioning is disabled, SSH host and user keys in the image are preserved and any configuration specified in the Azure provisioning API is ignored.</span></span>

> [!NOTE]
> <span data-ttu-id="9bbeb-198">Den `Provisioning.Enabled` ”n” på Ubuntu molnet bilder som använder molnet init för etablering som standard.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-198">The `Provisioning.Enabled` parameter defaults to "n" on Ubuntu Cloud Images that use cloud-init for provisioning.</span></span>
> 
> 

<span data-ttu-id="9bbeb-199">**Provisioning.DeleteRootPassword:**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-199">**Provisioning.DeleteRootPassword:**</span></span>  
<span data-ttu-id="9bbeb-200">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="9bbeb-200">Type: Boolean</span></span>  
<span data-ttu-id="9bbeb-201">Standard: n</span><span class="sxs-lookup"><span data-stu-id="9bbeb-201">Default: n</span></span>

<span data-ttu-id="9bbeb-202">Om mängd rotlösenordet i shadow-filen/etc/tas bort under etableringen.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-202">If set, the root password in the /etc/shadow file is erased during the provisioning process.</span></span>

<span data-ttu-id="9bbeb-203">**Provisioning.RegenerateSshHostKeyPair:**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-203">**Provisioning.RegenerateSshHostKeyPair:**</span></span>  
<span data-ttu-id="9bbeb-204">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="9bbeb-204">Type: Boolean</span></span>  
<span data-ttu-id="9bbeb-205">Standard: y</span><span class="sxs-lookup"><span data-stu-id="9bbeb-205">Default: y</span></span>

<span data-ttu-id="9bbeb-206">Om mängd alla SSH värden nyckelpar (ecdsa, dsa och rsa) tas bort under etableringen från/etc/ssh /.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-206">If set, all SSH host key pairs (ecdsa, dsa and rsa) are deleted during the provisioning process from /etc/ssh/.</span></span> <span data-ttu-id="9bbeb-207">Och en enda ny nyckelpar genereras.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-207">And a single fresh key pair is generated.</span></span>

<span data-ttu-id="9bbeb-208">Krypteringstyp för ny nyckelparet kan konfigureras av Provisioning.SshHostKeyPairType posten.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-208">The encryption type for the fresh key pair is configurable by the Provisioning.SshHostKeyPairType entry.</span></span> <span data-ttu-id="9bbeb-209">Observera att vissa distributioner återskapas SSH-nyckelpar för valfri krypteringstyp som saknas när SSH-daemon startas (till exempel vid en omstart).</span><span class="sxs-lookup"><span data-stu-id="9bbeb-209">Please note that some distributions will re-create SSH key pairs for any missing encryption types when the SSH daemon is restarted (for example, upon a reboot).</span></span>

<span data-ttu-id="9bbeb-210">**Provisioning.SshHostKeyPairType:**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-210">**Provisioning.SshHostKeyPairType:**</span></span>  
<span data-ttu-id="9bbeb-211">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="9bbeb-211">Type: String</span></span>  
<span data-ttu-id="9bbeb-212">Standard: rsa</span><span class="sxs-lookup"><span data-stu-id="9bbeb-212">Default: rsa</span></span>

<span data-ttu-id="9bbeb-213">Detta kan ställas in att algoritmen krypteringstyp som stöds av SSH-daemon på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-213">This can be set to an encryption algorithm type that is supported by the SSH daemon on the virtual machine.</span></span> <span data-ttu-id="9bbeb-214">Värdena som vanligtvis stöds är ”rsa”, ”dsa” och ”ecdsa”.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-214">The typically supported values are "rsa", "dsa" and "ecdsa".</span></span> <span data-ttu-id="9bbeb-215">Observera att ”putty.exe” i Windows inte har stöd för ”ecdsa”.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-215">Note that "putty.exe" on Windows does not support "ecdsa".</span></span> <span data-ttu-id="9bbeb-216">Så om du tänker använda putty.exe i Windows för att ansluta till en Linux-distribution, Använd ”rsa” eller ”dsa”.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-216">So, if you intend to use putty.exe on Windows to connect to a Linux deployment, please use "rsa" or "dsa".</span></span>

<span data-ttu-id="9bbeb-217">**Provisioning.MonitorHostName:**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-217">**Provisioning.MonitorHostName:**</span></span>  
<span data-ttu-id="9bbeb-218">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="9bbeb-218">Type: Boolean</span></span>  
<span data-ttu-id="9bbeb-219">Standard: y</span><span class="sxs-lookup"><span data-stu-id="9bbeb-219">Default: y</span></span>

<span data-ttu-id="9bbeb-220">Om waagent ska övervaka Linux-dator för hostname ändringar (som returneras av kommandot ”värdnamnet”) och automatiskt uppdatera nätverkskonfigurationen på bilden som ska ändras.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-220">If set, waagent will monitor the Linux virtual machine for hostname changes (as returned by the "hostname" command) and automatically update the networking configuration in the image to reflect the change.</span></span> <span data-ttu-id="9bbeb-221">För att push-namnbytet till DNS-servrar, startas nätverk på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-221">In order to push the name change to the DNS servers, networking will be restarted in the virtual machine.</span></span> <span data-ttu-id="9bbeb-222">Detta resulterar i korthet förlust av Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-222">This will result in brief loss of Internet connectivity.</span></span>

<span data-ttu-id="9bbeb-223">**Provisioning.DecodeCustomData**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-223">**Provisioning.DecodeCustomData**</span></span>  
<span data-ttu-id="9bbeb-224">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="9bbeb-224">Type: Boolean</span></span>  
<span data-ttu-id="9bbeb-225">Standard: n</span><span class="sxs-lookup"><span data-stu-id="9bbeb-225">Default: n</span></span>

<span data-ttu-id="9bbeb-226">Om har angetts waagent avkodar CustomData från Base64.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-226">If set, waagent will decode CustomData from Base64.</span></span>

<span data-ttu-id="9bbeb-227">**Provisioning.ExecuteCustomData**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-227">**Provisioning.ExecuteCustomData**</span></span>  
<span data-ttu-id="9bbeb-228">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="9bbeb-228">Type: Boolean</span></span>  
<span data-ttu-id="9bbeb-229">Standard: n</span><span class="sxs-lookup"><span data-stu-id="9bbeb-229">Default: n</span></span>

<span data-ttu-id="9bbeb-230">Om det har angetts waagent körs CustomData efter etableringen.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-230">If set, waagent will execute CustomData after provisioning.</span></span>

<span data-ttu-id="9bbeb-231">**Provisioning.PasswordCryptId**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-231">**Provisioning.PasswordCryptId**</span></span>  
<span data-ttu-id="9bbeb-232">Typ: sträng</span><span class="sxs-lookup"><span data-stu-id="9bbeb-232">Type:String</span></span>  
<span data-ttu-id="9bbeb-233">Standard: 6</span><span class="sxs-lookup"><span data-stu-id="9bbeb-233">Default:6</span></span>

<span data-ttu-id="9bbeb-234">Algoritm som används av crypt vid generering av lösenords-hash.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-234">Algorithm used by crypt when generating password hash.</span></span>  
 <span data-ttu-id="9bbeb-235">1 - MD5</span><span class="sxs-lookup"><span data-stu-id="9bbeb-235">1 - MD5</span></span>  
 <span data-ttu-id="9bbeb-236">2a - Blowfish</span><span class="sxs-lookup"><span data-stu-id="9bbeb-236">2a - Blowfish</span></span>  
 <span data-ttu-id="9bbeb-237">5 - SHA-256</span><span class="sxs-lookup"><span data-stu-id="9bbeb-237">5 - SHA-256</span></span>  
 <span data-ttu-id="9bbeb-238">6 - SHA-512</span><span class="sxs-lookup"><span data-stu-id="9bbeb-238">6 - SHA-512</span></span>  

<span data-ttu-id="9bbeb-239">**Provisioning.PasswordCryptSaltLength**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-239">**Provisioning.PasswordCryptSaltLength**</span></span>  
<span data-ttu-id="9bbeb-240">Typ: sträng</span><span class="sxs-lookup"><span data-stu-id="9bbeb-240">Type:String</span></span>  
<span data-ttu-id="9bbeb-241">Standard: 10</span><span class="sxs-lookup"><span data-stu-id="9bbeb-241">Default:10</span></span>

<span data-ttu-id="9bbeb-242">Längden på slumpmässiga salt som används vid generering av lösenords-hash.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-242">Length of random salt used when generating password hash.</span></span>

<span data-ttu-id="9bbeb-243">**ResourceDisk.Format:**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-243">**ResourceDisk.Format:**</span></span>  
<span data-ttu-id="9bbeb-244">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="9bbeb-244">Type: Boolean</span></span>  
<span data-ttu-id="9bbeb-245">Standard: y</span><span class="sxs-lookup"><span data-stu-id="9bbeb-245">Default: y</span></span>

<span data-ttu-id="9bbeb-246">Om har angetts resurs disketten plattformen formateras och monteras med waagent om typ av filsystem som begärts av användaren i ”ResourceDisk.Filesystem” är något annat än ”ntfs”.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-246">If set, the resource disk provided by the platform will be formatted and mounted by waagent if the filesystem type requested by the user in "ResourceDisk.Filesystem" is anything other than "ntfs".</span></span> <span data-ttu-id="9bbeb-247">En partition av typen Linux (83) görs tillgängliga på disken.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-247">A single partition of type Linux (83) will be made available on the disk.</span></span> <span data-ttu-id="9bbeb-248">Observera att den här partitionen inte formateras om den monteras har.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-248">Note that this partition will not be formatted if it can be successfully mounted.</span></span>

<span data-ttu-id="9bbeb-249">**ResourceDisk.Filesystem:**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-249">**ResourceDisk.Filesystem:**</span></span>  
<span data-ttu-id="9bbeb-250">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="9bbeb-250">Type: String</span></span>  
<span data-ttu-id="9bbeb-251">Standard: ext4</span><span class="sxs-lookup"><span data-stu-id="9bbeb-251">Default: ext4</span></span>

<span data-ttu-id="9bbeb-252">Anger typ av filsystem för resurs-disk.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-252">This specifies the filesystem type for the resource disk.</span></span> <span data-ttu-id="9bbeb-253">Värden som stöds beror på Linux-distribution.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-253">Supported values vary by Linux distribution.</span></span> <span data-ttu-id="9bbeb-254">Om strängen är X, sedan mkfs. X ska finnas på Linux-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-254">If the string is X, then mkfs.X should be present on the Linux image.</span></span> <span data-ttu-id="9bbeb-255">SLES 11 bilder vanligtvis använda 'ext3'.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-255">SLES 11 images should typically use 'ext3'.</span></span> <span data-ttu-id="9bbeb-256">FreeBSD bilder ska använda 'ufs2' här.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-256">FreeBSD images should use 'ufs2' here.</span></span>

<span data-ttu-id="9bbeb-257">**ResourceDisk.MountPoint:**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-257">**ResourceDisk.MountPoint:**</span></span>  
<span data-ttu-id="9bbeb-258">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="9bbeb-258">Type: String</span></span>  
<span data-ttu-id="9bbeb-259">Standard: / mnt/resurs</span><span class="sxs-lookup"><span data-stu-id="9bbeb-259">Default: /mnt/resource</span></span> 

<span data-ttu-id="9bbeb-260">Detta anger den sökväg där resursen disken är monterad.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-260">This specifies the path at which the resource disk is mounted.</span></span> <span data-ttu-id="9bbeb-261">Observera att disken som resursen är en *tillfälliga* disk och kan tömmas när den virtuella datorn avetableras.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-261">Note that the resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span>

<span data-ttu-id="9bbeb-262">**ResourceDisk.MountOptions**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-262">**ResourceDisk.MountOptions**</span></span>  
<span data-ttu-id="9bbeb-263">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="9bbeb-263">Type: String</span></span>  
<span data-ttu-id="9bbeb-264">Standard: ingen</span><span class="sxs-lookup"><span data-stu-id="9bbeb-264">Default: None</span></span>

<span data-ttu-id="9bbeb-265">Anger disk monteringsalternativ som ska skickas till kommandot mount -o.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-265">Specifies disk mount options to be passed to the mount -o command.</span></span> <span data-ttu-id="9bbeb-266">Detta är en kommaavgränsad lista med värden, t.ex.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-266">This is a comma separated list of values, ex.</span></span> <span data-ttu-id="9bbeb-267">'nodev nosuid'.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-267">'nodev,nosuid'.</span></span> <span data-ttu-id="9bbeb-268">Se mount(8) mer information.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-268">See mount(8) for details.</span></span>

<span data-ttu-id="9bbeb-269">**ResourceDisk.EnableSwap:**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-269">**ResourceDisk.EnableSwap:**</span></span>  
<span data-ttu-id="9bbeb-270">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="9bbeb-270">Type: Boolean</span></span>  
<span data-ttu-id="9bbeb-271">Standard: n</span><span class="sxs-lookup"><span data-stu-id="9bbeb-271">Default: n</span></span>

<span data-ttu-id="9bbeb-272">Om ange en växlingsfil (/ swapfile) skapas på disken för resursen och läggs till växlingsutrymme system.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-272">If set, a swap file (/swapfile) is created on the resource disk and added to the system swap space.</span></span>

<span data-ttu-id="9bbeb-273">**ResourceDisk.SwapSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-273">**ResourceDisk.SwapSizeMB:**</span></span>  
<span data-ttu-id="9bbeb-274">Typ: heltal</span><span class="sxs-lookup"><span data-stu-id="9bbeb-274">Type: Integer</span></span>  
<span data-ttu-id="9bbeb-275">Standard: 0</span><span class="sxs-lookup"><span data-stu-id="9bbeb-275">Default: 0</span></span>

<span data-ttu-id="9bbeb-276">Storleken på växlingsfilen i megabyte.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-276">The size of the swap file in megabytes.</span></span>

<span data-ttu-id="9bbeb-277">**Logs.Verbose:**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-277">**Logs.Verbose:**</span></span>  
<span data-ttu-id="9bbeb-278">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="9bbeb-278">Type: Boolean</span></span>  
<span data-ttu-id="9bbeb-279">Standard: n</span><span class="sxs-lookup"><span data-stu-id="9bbeb-279">Default: n</span></span>

<span data-ttu-id="9bbeb-280">Om mängd loggen detaljnivå förstärks.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-280">If set, log verbosity is boosted.</span></span> <span data-ttu-id="9bbeb-281">Waagent loggar till /var/log/waagent.log och använder system logrotate funktioner för att rotera loggar.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-281">Waagent logs to /var/log/waagent.log and leverages the system logrotate functionality to rotate logs.</span></span>

<span data-ttu-id="9bbeb-282">**OS. EnableRDMA**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-282">**OS.EnableRDMA**</span></span>  
<span data-ttu-id="9bbeb-283">Typ: booleskt</span><span class="sxs-lookup"><span data-stu-id="9bbeb-283">Type: Boolean</span></span>  
<span data-ttu-id="9bbeb-284">Standard: n</span><span class="sxs-lookup"><span data-stu-id="9bbeb-284">Default: n</span></span>

<span data-ttu-id="9bbeb-285">Om kan agenten försöker att installera och läsa in en RDMA kernel-drivrutinen som matchar versionen av den inbyggda programvaran i den underliggande maskinvaran.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-285">If set, the agent will attempt to install and then load an RDMA kernel driver that matches the version of the firmware on the underlying hardware.</span></span>

<span data-ttu-id="9bbeb-286">**OS. RootDeviceScsiTimeout:**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-286">**OS.RootDeviceScsiTimeout:**</span></span>  
<span data-ttu-id="9bbeb-287">Typ: heltal</span><span class="sxs-lookup"><span data-stu-id="9bbeb-287">Type: Integer</span></span>  
<span data-ttu-id="9bbeb-288">Standard: 300</span><span class="sxs-lookup"><span data-stu-id="9bbeb-288">Default: 300</span></span>

<span data-ttu-id="9bbeb-289">Detta konfigurerar SCSI-tidsgräns i sekunder för OS-disk- och enheter.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-289">This configures the SCSI timeout in seconds on the OS disk and data drives.</span></span> <span data-ttu-id="9bbeb-290">Om den inte har angetts i systemet som standard.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-290">If not set, the system defaults are used.</span></span>

<span data-ttu-id="9bbeb-291">**OS. OpensslPath:**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-291">**OS.OpensslPath:**</span></span>  
<span data-ttu-id="9bbeb-292">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="9bbeb-292">Type: String</span></span>  
<span data-ttu-id="9bbeb-293">Standard: ingen</span><span class="sxs-lookup"><span data-stu-id="9bbeb-293">Default: None</span></span>

<span data-ttu-id="9bbeb-294">Detta kan användas för att ange en alternativ sökväg för openssl binära för kryptografiska åtgärder.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-294">This can be used to specify an alternate path for the openssl binary to use for cryptographic operations.</span></span>

<span data-ttu-id="9bbeb-295">**HttpProxy.Host HttpProxy.Port**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-295">**HttpProxy.Host, HttpProxy.Port**</span></span>  
<span data-ttu-id="9bbeb-296">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="9bbeb-296">Type: String</span></span>  
<span data-ttu-id="9bbeb-297">Standard: ingen</span><span class="sxs-lookup"><span data-stu-id="9bbeb-297">Default: None</span></span>

<span data-ttu-id="9bbeb-298">Om kan agenten ska använda den här proxyservern åtkomst till internet.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-298">If set, the agent will use this proxy server to access the internet.</span></span> 

## <a name="ubuntu-cloud-images"></a><span data-ttu-id="9bbeb-299">Ubuntu molnet bilder</span><span class="sxs-lookup"><span data-stu-id="9bbeb-299">Ubuntu Cloud Images</span></span>
<span data-ttu-id="9bbeb-300">Observera att använda Ubuntu molnet bilder [moln init](https://launchpad.net/ubuntu/+source/cloud-init) att utföra många konfigurationer som annars skulle hanteras av Azure Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-300">Note that Ubuntu Cloud Images utilize [cloud-init](https://launchpad.net/ubuntu/+source/cloud-init) to perform many configuration tasks that would otherwise be managed by the Azure Linux Agent.</span></span>  <span data-ttu-id="9bbeb-301">Observera följande skillnader:</span><span class="sxs-lookup"><span data-stu-id="9bbeb-301">Please note the following differences:</span></span>

* <span data-ttu-id="9bbeb-302">**Provisioning.Enabled** ”n” på Ubuntu molnet bilder som använder molnet init etablering arbetsuppgifter som standard.</span><span class="sxs-lookup"><span data-stu-id="9bbeb-302">**Provisioning.Enabled** defaults to "n" on Ubuntu Cloud Images that use cloud-init to perform provisioning tasks.</span></span>
* <span data-ttu-id="9bbeb-303">Följande konfigurationsparametrar har ingen effekt på Ubuntu molnet bilder som använder molnet init att hantera resurs disken och växlingsutrymme:</span><span class="sxs-lookup"><span data-stu-id="9bbeb-303">The following configuration parameters have no effect on Ubuntu Cloud Images that use cloud-init to manage the resource disk and swap space:</span></span>
  
  * <span data-ttu-id="9bbeb-304">**ResourceDisk.Format**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-304">**ResourceDisk.Format**</span></span>
  * <span data-ttu-id="9bbeb-305">**ResourceDisk.Filesystem**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-305">**ResourceDisk.Filesystem**</span></span>
  * <span data-ttu-id="9bbeb-306">**ResourceDisk.MountPoint**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-306">**ResourceDisk.MountPoint**</span></span>
  * <span data-ttu-id="9bbeb-307">**ResourceDisk.EnableSwap**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-307">**ResourceDisk.EnableSwap**</span></span>
  * <span data-ttu-id="9bbeb-308">**ResourceDisk.SwapSizeMB**</span><span class="sxs-lookup"><span data-stu-id="9bbeb-308">**ResourceDisk.SwapSizeMB**</span></span>
* <span data-ttu-id="9bbeb-309">Se följande resurser för att konfigurera resursen disk monteringspunkten och växlingsutrymme på Ubuntu molnet bilder under etableringen:</span><span class="sxs-lookup"><span data-stu-id="9bbeb-309">Please see the following resources to configure the resource disk mount point and swap space on Ubuntu Cloud Images during provisioning:</span></span>
  
  * [<span data-ttu-id="9bbeb-310">Ubuntu Wiki: Konfigurera växlingen partitioner</span><span class="sxs-lookup"><span data-stu-id="9bbeb-310">Ubuntu Wiki: Configure Swap Partitions</span></span>](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [<span data-ttu-id="9bbeb-311">Placera anpassade Data till en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="9bbeb-311">Injecting Custom Data into an Azure Virtual Machine</span></span>](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

