---
title: "aaaAzure Disk Encryption för Windows och Linux virtuella IaaS-datorer | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över Microsoft Azure Disk Encryption för Windows och Linux virtuella IaaS-datorer."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: d3fac8bb-4829-405e-8701-fa7229fb1725
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: kakhan
ms.openlocfilehash: b685abdcc908e66d2352ec5ac2d9996aa75af1b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a><span data-ttu-id="5eec6-103">Azure Disk Encryption för Windows och Linux-IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-103">Azure Disk Encryption for Windows and Linux IaaS VMs</span></span>
<span data-ttu-id="5eec6-104">Microsoft Azure är starkt allokerat tooensuring datasekretessen data suveränitet och gör att du toocontrol din Azure värdbaserade data via en mängd avancerad teknik tooencrypt styra och hantera krypteringsnycklar kontroll & granska åtkomsten till data.</span><span class="sxs-lookup"><span data-stu-id="5eec6-104">Microsoft Azure is strongly committed tooensuring your data privacy, data sovereignty and enables you toocontrol your Azure hosted data through a range of advanced technologies tooencrypt, control and manage encryption keys, control & audit access of data.</span></span> <span data-ttu-id="5eec6-105">Det ger Azure-kunder hello flexibilitet toochoose hello lösning som bäst motsvarar deras affärsbehov.</span><span class="sxs-lookup"><span data-stu-id="5eec6-105">This provides Azure customers hello flexibility toochoose hello solution that best meets their business needs.</span></span> <span data-ttu-id="5eec6-106">I det här dokumentet får du lära dig mer tooa nya tekniklösning ”Azure Disk Encryption för Windows och Linux IaaS VM” toohelp skydda och skydda dina data toomeet din organisations säkerhet och efterlevnad åtaganden.</span><span class="sxs-lookup"><span data-stu-id="5eec6-106">In this paper, we will introduce you tooa new technology solution “Azure Disk Encryption for Windows and Linux IaaS VM’s” toohelp protect and safeguard your data toomeet your organizational security and compliance commitments.</span></span> <span data-ttu-id="5eec6-107">hello dokumentet ger detaljerad vägledning om hur toouse hello Azure disk encryption funktioner inklusive hello stöds scenarier och hello användarupplevelser.</span><span class="sxs-lookup"><span data-stu-id="5eec6-107">hello paper provides detailed guidance on how toouse hello Azure disk encryption features including hello supported scenarios and hello user experiences.</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-108">Vissa rekommendationerna kan öka data, nätverk och beräkning Resursanvändning, vilket resulterar i ytterligare kostnader för licens eller prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5eec6-108">Certain recommendations might increase data, network, or compute resource usage, resulting in additional license or subscription costs.</span></span>

## <a name="overview"></a><span data-ttu-id="5eec6-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="5eec6-109">Overview</span></span>
<span data-ttu-id="5eec6-110">Azure Disk Encryption är en ny funktion som hjälper dig att kryptera din Windows- och Linux IaaS virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="5eec6-110">Azure Disk Encryption is a new capability that helps you encrypt your Windows and Linux IaaS virtual machine disks.</span></span> <span data-ttu-id="5eec6-111">Azure Disk Encryption utnyttjar hello branschstandarden [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funktion i Windows och hello [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funktion i Linux tooprovide volymkryptering för hello OS och hello datadiskar.</span><span class="sxs-lookup"><span data-stu-id="5eec6-111">Azure Disk Encryption leverages hello industry standard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) feature of Windows and hello [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) feature of Linux tooprovide volume encryption for hello OS and hello data disks.</span></span> <span data-ttu-id="5eec6-112">hello-lösning är integrerad med [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp du styra och hantera hello-diskkryptering nycklar och hemligheter i nyckelvalvet-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5eec6-112">hello solution is integrated with [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp you control and manage hello disk-encryption keys and secrets in your key vault subscription.</span></span> <span data-ttu-id="5eec6-113">hello lösning betyder också att krypteras alla data på hello virtuella diskar i vila i ditt Azure storage.</span><span class="sxs-lookup"><span data-stu-id="5eec6-113">hello solution also ensures that all data on hello virtual machine disks are encrypted at rest in your Azure storage.</span></span>

<span data-ttu-id="5eec6-114">Azure disk encryption för Windows och Linux IaaS-VM är nu i **allmän tillgänglighet** i alla Azure offentliga och AzureGov regioner för Standard virtuella datorer och virtuella datorer med premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="5eec6-114">Azure disk encryption for Windows and Linux IaaS VMs is now in **General Availability** in all Azure public regions and AzureGov regions for Standard  VMs and VMs with premium storage.</span></span>

### <a name="encryption-scenarios"></a><span data-ttu-id="5eec6-115">Scenarier för kryptering</span><span class="sxs-lookup"><span data-stu-id="5eec6-115">Encryption scenarios</span></span>
<span data-ttu-id="5eec6-116">hello Azure Disk Encryption lösning stöder hello efter kundscenarier:</span><span class="sxs-lookup"><span data-stu-id="5eec6-116">hello Azure Disk Encryption solution supports hello following customer scenarios:</span></span>

* <span data-ttu-id="5eec6-117">Aktivera kryptering på den nya virtuella IaaS-datorer skapas från förkrypterade VHD- och krypteringsnycklar</span><span class="sxs-lookup"><span data-stu-id="5eec6-117">Enable encryption on new IaaS VMs created from pre-encrypted VHD and encryption keys</span></span>
* <span data-ttu-id="5eec6-118">Aktivera kryptering på den nya virtuella IaaS-datorer skapas från galleriavbildningar för hello stöds Azure</span><span class="sxs-lookup"><span data-stu-id="5eec6-118">Enable encryption on new IaaS VMs created from hello supported Azure Gallery images</span></span>
* <span data-ttu-id="5eec6-119">Aktivera kryptering på befintliga virtuella IaaS-datorer körs i Azure</span><span class="sxs-lookup"><span data-stu-id="5eec6-119">Enable encryption on existing IaaS VMs running in Azure</span></span>
* <span data-ttu-id="5eec6-120">Inaktivera kryptering på Windows IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-120">Disable encryption on Windows IaaS VMs</span></span>
* <span data-ttu-id="5eec6-121">Inaktivera kryptering på dataenheter för Linux virtuella IaaS-datorer</span><span class="sxs-lookup"><span data-stu-id="5eec6-121">Disable encryption on data drives for Linux IaaS VMs</span></span>
* <span data-ttu-id="5eec6-122">Aktivera kryptering för hanterade diskar virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="5eec6-122">Enable encryption of managed disk VMs</span></span>
* <span data-ttu-id="5eec6-123">Uppdatera krypteringsinställningarna för en befintlig krypterade premium-lagring VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-123">Update encryption settings of an existing encrypted non-premium storage VM</span></span>
* <span data-ttu-id="5eec6-124">Säkerhetskopiering och återställning av krypterade virtuella datorer som krypterats med nyckelkryptering nyckel</span><span class="sxs-lookup"><span data-stu-id="5eec6-124">Backup and restore of encrypted VMs, encrypted with key encryption key</span></span>

<span data-ttu-id="5eec6-125">hello-lösningen stöder hello följande scenarier för virtuella IaaS-datorer när de är aktiverade i Microsoft Azure:</span><span class="sxs-lookup"><span data-stu-id="5eec6-125">hello solution supports hello following scenarios for IaaS VMs when they are enabled in Microsoft Azure:</span></span>

* <span data-ttu-id="5eec6-126">Integrering med Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5eec6-126">Integration with Azure Key Vault</span></span>
* <span data-ttu-id="5eec6-127">Standardnivån VMs: [A, D, DS, G, GS, F och så vidare serien IaaS-VM](https://azure.microsoft.com/pricing/details/virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="5eec6-127">Standard tier VMs: [A, D, DS, G, GS, F, and so forth series IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)</span></span>
* <span data-ttu-id="5eec6-128">Aktivera kryptering på Windows och Linux virtuella IaaS-datorer och virtuella datorer för hanterade diskar från hello stöds avbildningar i Azure-galleriet</span><span class="sxs-lookup"><span data-stu-id="5eec6-128">Enable encryption on Windows and Linux IaaS VMs and managed disk VMs from hello supported Azure Gallery images</span></span>
* <span data-ttu-id="5eec6-129">Inaktivera kryptering på Operativsystemet och enheter för Windows IaaS-VM och hanterade diskar virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="5eec6-129">Disable encryption on OS and data drives for Windows IaaS VMs and managed disk VMs</span></span>
* <span data-ttu-id="5eec6-130">Inaktivera kryptering på dataenheter för Linux virtuella IaaS-datorer och hanterade diskar virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="5eec6-130">Disable encryption on data drives for Linux IaaS VMs and managed disk VMs</span></span>
* <span data-ttu-id="5eec6-131">Aktivera kryptering på IaaS virtuella datorer som kör Windows klient-OS</span><span class="sxs-lookup"><span data-stu-id="5eec6-131">Enable encryption on IaaS VMs running Windows Client OS</span></span>
* <span data-ttu-id="5eec6-132">Aktivera kryptering på volymer med monteringssökvägar</span><span class="sxs-lookup"><span data-stu-id="5eec6-132">Enable encryption on volumes with mount paths</span></span>
* <span data-ttu-id="5eec6-133">Aktivera kryptering på den virtuella Linux-datorer konfigurerade med disk striping (RAID) med hjälp av mdadm</span><span class="sxs-lookup"><span data-stu-id="5eec6-133">Enable encryption on Linux VMs configured with disk striping (RAID) using mdadm</span></span>
* <span data-ttu-id="5eec6-134">Aktivera kryptering på den virtuella Linux-datorer med hjälp av LVM för datadiskar</span><span class="sxs-lookup"><span data-stu-id="5eec6-134">Enable encryption on Linux VMs using LVM for data disks</span></span>
* <span data-ttu-id="5eec6-135">Aktivera kryptering på virtuella Windows-datorer konfigurerade med lagringsutrymmen</span><span class="sxs-lookup"><span data-stu-id="5eec6-135">Enable encryption on Windows VMs configured with Storage Spaces</span></span>
* <span data-ttu-id="5eec6-136">Uppdatera krypteringsinställningarna för en befintlig krypterade premium-lagring VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-136">Update encryption settings of an existing encrypted non-premium storage VM</span></span>
* <span data-ttu-id="5eec6-137">Alla offentliga Azure och AzureGov regioner som stöds</span><span class="sxs-lookup"><span data-stu-id="5eec6-137">All Azure Public and AzureGov regions are supported</span></span>

<span data-ttu-id="5eec6-138">hello-lösningen stöder inte hello följande scenarier, funktioner och teknik:</span><span class="sxs-lookup"><span data-stu-id="5eec6-138">hello solution does not support hello following scenarios, features, and technology:</span></span>

* <span data-ttu-id="5eec6-139">Grundnivån IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-139">Basic tier IaaS VMs</span></span>
* <span data-ttu-id="5eec6-140">Om du inaktiverar kryptering på en OS-enhet för Linux virtuella IaaS-datorer</span><span class="sxs-lookup"><span data-stu-id="5eec6-140">Disabling encryption on an OS drive for Linux IaaS VMs</span></span>
* <span data-ttu-id="5eec6-141">Om du inaktiverar kryptering på en dataenhet om hello OS-enheten är krypterad för Linux virtuella Iaas-datorer</span><span class="sxs-lookup"><span data-stu-id="5eec6-141">Disabling encryption on a data drive if hello OS drive is encrypted for Linux Iaas VMs</span></span>
* <span data-ttu-id="5eec6-142">Virtuella IaaS-datorer som skapas med hjälp av hello klassiska Virtuella metod för skapande av</span><span class="sxs-lookup"><span data-stu-id="5eec6-142">IaaS VMs that are created by using hello classic VM creation method</span></span>
* <span data-ttu-id="5eec6-143">Aktivera kryptering på Windows och Linux-IaaS-VM kunden anpassade avbildningar inte stöds.</span><span class="sxs-lookup"><span data-stu-id="5eec6-143">Enable encryption on Windows and Linux IaaS VMs customer custom images is NOT supported.</span></span> <span data-ttu-id="5eec6-144">Aktivera enccryption på Linux LVM OS disk stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="5eec6-144">Enable enccryption on Linux LVM OS disk is not supported currently.</span></span> <span data-ttu-id="5eec6-145">Det här stödet kommer snart.</span><span class="sxs-lookup"><span data-stu-id="5eec6-145">This support will come soon.</span></span>
* <span data-ttu-id="5eec6-146">Integrering med din lokala nyckelhanteringstjänst</span><span class="sxs-lookup"><span data-stu-id="5eec6-146">Integration with your on-premises Key Management Service</span></span>
* <span data-ttu-id="5eec6-147">Azure Files (delade filsystem), Network File System (NFS), dynamiska volymer och virtuella Windows-datorer som är konfigurerade med programvarubaserad RAID-system</span><span class="sxs-lookup"><span data-stu-id="5eec6-147">Azure Files (shared file system), Network File System (NFS), dynamic volumes, and Windows VMs that are configured with software-based RAID systems</span></span>
* <span data-ttu-id="5eec6-148">Säkerhetskopiering och återställning av krypterade virtuella datorer, krypterade utan nyckelkryptering nyckel.</span><span class="sxs-lookup"><span data-stu-id="5eec6-148">Backup and restore of encrypted VMs, encrypted without key encryption key.</span></span>
* <span data-ttu-id="5eec6-149">Uppdatera krypteringsinställningarna för en befintlig krypterade premium-lagring VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-149">Update encryption settings of an existing encrypted premium storage VM.</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-150">Säkerhetskopiering och återställning av krypterade virtuella datorer stöds bara för virtuella datorer som är krypterade med hello KEK konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5eec6-150">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with hello KEK configuration.</span></span> <span data-ttu-id="5eec6-151">Den stöds inte på virtuella datorer som är krypterade utan KEK.</span><span class="sxs-lookup"><span data-stu-id="5eec6-151">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="5eec6-152">KEK är en valfri parameter som aktiverar VM-kryptering.</span><span class="sxs-lookup"><span data-stu-id="5eec6-152">KEK is an optional parameter that enables VM encryption.</span></span> <span data-ttu-id="5eec6-153">Det här stödet kommer snart.</span><span class="sxs-lookup"><span data-stu-id="5eec6-153">This support is coming soon.</span></span>
> <span data-ttu-id="5eec6-154">Uppdatera krypteringsinställningarna för en befintlig krypterade premium-lagring VM inte stöds.</span><span class="sxs-lookup"><span data-stu-id="5eec6-154">Update encryption settings of an existing encrypted premium storage VM are not supported.</span></span> <span data-ttu-id="5eec6-155">Det här stödet kommer snart.</span><span class="sxs-lookup"><span data-stu-id="5eec6-155">This support is coming soon.</span></span>

### <a name="encryption-features"></a><span data-ttu-id="5eec6-156">Krypteringsfunktioner</span><span class="sxs-lookup"><span data-stu-id="5eec6-156">Encryption features</span></span>
<span data-ttu-id="5eec6-157">När du aktiverar och distribuera Azure Disk Encryption för Azure IaaS-VM är hello följande funktioner aktiverade, beroende på konfigurationen hello:</span><span class="sxs-lookup"><span data-stu-id="5eec6-157">When you enable and deploy Azure Disk Encryption for Azure IaaS VMs, hello following capabilities are enabled, depending on hello configuration provided:</span></span>

* <span data-ttu-id="5eec6-158">Kryptering av startvolymen för hello OS volym tooprotect hello i vila i lagringsutrymmet</span><span class="sxs-lookup"><span data-stu-id="5eec6-158">Encryption of hello OS volume tooprotect hello boot volume at rest in your storage</span></span>
* <span data-ttu-id="5eec6-159">Kryptering av data volymer tooprotect hello datavolymer i vila i lagringsutrymmet</span><span class="sxs-lookup"><span data-stu-id="5eec6-159">Encryption of data volumes tooprotect hello data volumes at rest in your storage</span></span>
* <span data-ttu-id="5eec6-160">Om du inaktiverar kryptering på hello OS och data enheter för Windows IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-160">Disabling encryption on hello OS and data drives for Windows IaaS VMs</span></span>
* <span data-ttu-id="5eec6-161">Om du inaktiverar kryptering på hello data enheter för Linux virtuella IaaS-datorer (endast om OS är inte krypterad enhet)</span><span class="sxs-lookup"><span data-stu-id="5eec6-161">Disabling encryption on hello data drives for Linux IaaS VMs (only if OS drive IS NOT encrypted)</span></span>
* <span data-ttu-id="5eec6-162">Skydda hello krypteringsnycklar och hemligheter i nyckelvalvet-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5eec6-162">Safeguarding hello encryption keys and secrets in your key vault subscription</span></span>
* <span data-ttu-id="5eec6-163">Rapportering hello krypteringsstatus för hello krypterade IaaS VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-163">Reporting hello encryption status of hello encrypted IaaS VM</span></span>
* <span data-ttu-id="5eec6-164">Borttagning av disk-kryptering konfigurationsinställningar från hello virtuell IaaS-dator</span><span class="sxs-lookup"><span data-stu-id="5eec6-164">Removal of disk-encryption configuration settings from hello IaaS virtual machine</span></span>
* <span data-ttu-id="5eec6-165">Säkerhetskopiering och återställning av krypterade virtuella datorer med hjälp av hello Azure Backup-tjänsten</span><span class="sxs-lookup"><span data-stu-id="5eec6-165">Backup and restore of encrypted VMs by using hello Azure Backup service</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-166">Säkerhetskopiering och återställning av krypterade virtuella datorer stöds bara för virtuella datorer som är krypterade med hello KEK konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5eec6-166">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with hello KEK configuration.</span></span> <span data-ttu-id="5eec6-167">Den stöds inte på virtuella datorer som är krypterade utan KEK.</span><span class="sxs-lookup"><span data-stu-id="5eec6-167">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="5eec6-168">KEK är en valfri parameter som aktiverar VM-kryptering.</span><span class="sxs-lookup"><span data-stu-id="5eec6-168">KEK is an optional parameter that enables VM encryption.</span></span>

<span data-ttu-id="5eec6-169">Azure Disk Encryption för IaaS VMS för Windows och Linux-lösningen innehåller:</span><span class="sxs-lookup"><span data-stu-id="5eec6-169">Azure Disk Encryption for IaaS VMS for Windows and Linux solution includes:</span></span>

* <span data-ttu-id="5eec6-170">hello diskkryptering tillägget för Windows.</span><span class="sxs-lookup"><span data-stu-id="5eec6-170">hello disk-encryption extension for Windows.</span></span>
* <span data-ttu-id="5eec6-171">hello diskkryptering tillägget för Linux.</span><span class="sxs-lookup"><span data-stu-id="5eec6-171">hello disk-encryption extension for Linux.</span></span>
* <span data-ttu-id="5eec6-172">hello-diskkryptering PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="5eec6-172">hello disk-encryption PowerShell cmdlets.</span></span>
* <span data-ttu-id="5eec6-173">Hej diskkryptering Azure-kommandoradsgränssnittet (CLI) cmdlets.</span><span class="sxs-lookup"><span data-stu-id="5eec6-173">hello disk-encryption Azure command-line interface (CLI) cmdlets.</span></span>
* <span data-ttu-id="5eec6-174">hello-diskkryptering Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="5eec6-174">hello disk-encryption Azure Resource Manager templates.</span></span>

<span data-ttu-id="5eec6-175">hello Azure Disk Encryption lösning stöds på virtuella IaaS-datorer som kör Windows eller Linux OS.</span><span class="sxs-lookup"><span data-stu-id="5eec6-175">hello Azure Disk Encryption solution is supported on IaaS VMs that are running Windows or Linux OS.</span></span> <span data-ttu-id="5eec6-176">Mer information om operativsystem som stöds hello finns hello ”krav” avsnittet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-176">For more information about hello supported operating systems, see hello "Prerequisites" section.</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-177">Det finns inga extra kostnad för att kryptera Virtuella diskar med Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="5eec6-177">There is no additional charge for encrypting VM disks with Azure Disk Encryption.</span></span>

### <a name="value-proposition"></a><span data-ttu-id="5eec6-178">Förslagsvärde</span><span class="sxs-lookup"><span data-stu-id="5eec6-178">Value proposition</span></span>
<span data-ttu-id="5eec6-179">När du använder hello Azure Disk Encryption-management-lösning kan du uppfylla hello följande affärsbehov:</span><span class="sxs-lookup"><span data-stu-id="5eec6-179">When you apply hello Azure Disk Encryption-management solution, you can satisfy hello following business needs:</span></span>

* <span data-ttu-id="5eec6-180">IaaS-VM är skyddade i vila, eftersom du kan använda branschstandardiserad teknik tooaddress organisations säkerhet och efterlevnad kraven för datakryptering.</span><span class="sxs-lookup"><span data-stu-id="5eec6-180">IaaS VMs are secured at rest, because you can use industry-standard encryption technology tooaddress organizational security and compliance requirements.</span></span>
* <span data-ttu-id="5eec6-181">IaaS-VM start under kund-kontrollerade nycklar och principer, och du kan granska deras användning i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-181">IaaS VMs boot under customer-controlled keys and policies, and you can audit their usage in your key vault.</span></span>

### <a name="encryption-workflow"></a><span data-ttu-id="5eec6-182">Arbetsflöde för kryptering</span><span class="sxs-lookup"><span data-stu-id="5eec6-182">Encryption workflow</span></span>
<span data-ttu-id="5eec6-183">kryptering av tooenable för Windows och Linux virtuella datorer, hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-183">tooenable disk encryption for Windows and Linux VMs, do hello following:</span></span>

1. <span data-ttu-id="5eec6-184">Välj ett scenario för kryptering bland hello före kryptering scenarier.</span><span class="sxs-lookup"><span data-stu-id="5eec6-184">Choose an encryption scenario from among hello preceding encryption scenarios.</span></span>
2. <span data-ttu-id="5eec6-185">Välja tooenabling diskkryptering via hello Azure Disk Encryption Resource Manager-mall, PowerShell-cmdlets eller CLI-kommando och ange hello kryptering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5eec6-185">Opt in tooenabling disk encryption via hello Azure Disk Encryption Resource Manager template, PowerShell cmdlets, or CLI command, and specify hello encryption configuration.</span></span>

   * <span data-ttu-id="5eec6-186">Överför hello krypterade VHD tooyour storage-konto och hello encryption key väsentlig tooyour nyckelvalv hello kund-krypterad VHD scenariot.</span><span class="sxs-lookup"><span data-stu-id="5eec6-186">For hello customer-encrypted VHD scenario, upload hello encrypted VHD tooyour storage account and hello encryption key material tooyour key vault.</span></span> <span data-ttu-id="5eec6-187">Ange hello kryptering configuration tooenable på en ny IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-187">Then, provide hello encryption configuration tooenable encryption on a new IaaS VM.</span></span>
   * <span data-ttu-id="5eec6-188">Ange hello kryptering configuration tooenable på hello IaaS VM för nya virtuella datorer som skapas från hello Marketplace och befintliga virtuella datorer som redan körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="5eec6-188">For new VMs that are created from hello Marketplace and existing VMs that are already running in Azure, provide hello encryption configuration tooenable encryption on hello IaaS VM.</span></span>

3. <span data-ttu-id="5eec6-189">Bevilja åtkomst toohello Azure-plattformen tooread hello-krypteringsnyckeln material (krypteringsnycklar BitLocker för Windows-System) och lösenfrasen för Linux från ditt nyckelvalv tooenable kryptering på hello IaaS VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-189">Grant access toohello Azure platform tooread hello encryption-key material (BitLocker encryption keys for Windows systems and Passphrase for Linux) from your key vault tooenable encryption on hello IaaS VM.</span></span>

4. <span data-ttu-id="5eec6-190">Ange hello Azure Active Directory (Azure AD) application identitet toowrite hello kryptering viktiga väsentlig tooyour nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="5eec6-190">Provide hello Azure Active Directory (Azure AD) application identity toowrite hello encryption key material tooyour key vault.</span></span> <span data-ttu-id="5eec6-191">På så sätt kan kryptering på hello IaaS VM hello-scenarier som nämns i steg 2.</span><span class="sxs-lookup"><span data-stu-id="5eec6-191">Doing so enables encryption on hello IaaS VM for hello scenarios mentioned in step 2.</span></span>

5. <span data-ttu-id="5eec6-192">Azure uppdaterar hello VM modell med kryptering och hello nyckelvalv konfigurationen och ställer in den kryptera virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5eec6-192">Azure updates hello VM service model with encryption and hello key vault configuration, and sets up your encrypted VM.</span></span>

 ![Microsoft Antimalware i Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a><span data-ttu-id="5eec6-194">Arbetsflöde för dekryptering</span><span class="sxs-lookup"><span data-stu-id="5eec6-194">Decryption workflow</span></span>
<span data-ttu-id="5eec6-195">kryptering av toodisable för IaaS-VM, fullständig hello följande anvisningar:</span><span class="sxs-lookup"><span data-stu-id="5eec6-195">toodisable disk encryption for IaaS VMs, complete hello following high-level steps:</span></span>

1. <span data-ttu-id="5eec6-196">Välj toodisable kryptering (dekryptering) på en körs IaaS-VM i Azure via hello Azure Disk Encryption Resource Manager-mall eller PowerShell-cmdlets och ange hello dekryptering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5eec6-196">Choose toodisable encryption (decryption) on a running IaaS VM in Azure via hello Azure Disk Encryption Resource Manager template or PowerShell cmdlets, and specify hello decryption configuration.</span></span>

 <span data-ttu-id="5eec6-197">Det här steget inaktiverar kryptering av hello OS eller hello datavolym eller båda på hello som använder Windows IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-197">This step disables encryption of hello OS or hello data volume or both on hello running Windows IaaS VM.</span></span> <span data-ttu-id="5eec6-198">Men som nämns i föregående avsnitt i hello stöds om du inaktiverar kryptering för OS-disk för Linux inte.</span><span class="sxs-lookup"><span data-stu-id="5eec6-198">However, as mentioned in hello previous section, disabling OS disk encryption for Linux is not supported.</span></span> <span data-ttu-id="5eec6-199">hello dekryptering steg tillåts endast för dataenheter i virtuella Linux-datorer så länge hello OS-disken inte krypteras.</span><span class="sxs-lookup"><span data-stu-id="5eec6-199">hello decryption step is allowed only for data drives on Linux VMs as long as hello OS disk is not encrypted.</span></span>
2. <span data-ttu-id="5eec6-200">Azure-uppdateringar hello VM modell och hello IaaS VM markeras dekrypterade.</span><span class="sxs-lookup"><span data-stu-id="5eec6-200">Azure updates hello VM service model, and hello IaaS VM is marked decrypted.</span></span> <span data-ttu-id="5eec6-201">hello innehållet i hello VM krypteras inte längre i vila.</span><span class="sxs-lookup"><span data-stu-id="5eec6-201">hello contents of hello VM are no longer encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-202">hello inaktivera kryptering åtgärden tar inte bort din key vault och hello kryptering nyckelmaterial (krypteringsnycklar BitLocker för Windows-System) eller lösenfrasen för Linux.</span><span class="sxs-lookup"><span data-stu-id="5eec6-202">hello disable-encryption operation does not delete your key vault and hello encryption key material (BitLocker encryption keys for Windows systems or Passphrase for Linux).</span></span>
 > <span data-ttu-id="5eec6-203">Om du inaktiverar kryptering för OS-disk för Linux stöds inte.</span><span class="sxs-lookup"><span data-stu-id="5eec6-203">Disabling OS disk encryption for Linux is not supported.</span></span> <span data-ttu-id="5eec6-204">hello dekryptering steg tillåts endast för dataenheter i virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="5eec6-204">hello decryption step is allowed only for data drives on Linux VMs.</span></span>
<span data-ttu-id="5eec6-205">Inaktivera disk datakryptering för Linux stöds inte om hello OS-enheten är krypterad.</span><span class="sxs-lookup"><span data-stu-id="5eec6-205">Disabling data disk encryption for Linux is not supported if hello OS drive is encrypted.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5eec6-206">Krav</span><span class="sxs-lookup"><span data-stu-id="5eec6-206">Prerequisites</span></span>
<span data-ttu-id="5eec6-207">Innan du aktiverar Azure Disk Encryption på Azure IaaS-VM för hello stöds scenarier som beskrivs i hello ”Overview” avsnittet finns hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="5eec6-207">Before you enable Azure Disk Encryption on Azure IaaS VMs for hello supported scenarios that were discussed in hello "Overview" section, see hello following prerequisites:</span></span>

* <span data-ttu-id="5eec6-208">Du måste ha ett giltigt aktiv Azure-prenumeration toocreate resurser i Azure i hello stöds regioner.</span><span class="sxs-lookup"><span data-stu-id="5eec6-208">You must have a valid active Azure subscription toocreate resources in Azure in hello supported regions.</span></span>
* <span data-ttu-id="5eec6-209">Azure Disk Encryption stöds på hello följande versioner av Windows Server: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 och Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="5eec6-209">Azure Disk Encryption is supported on hello following Windows Server versions: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, and Windows Server 2016.</span></span>
* <span data-ttu-id="5eec6-210">Azure Disk Encryption stöds på följande Windows-klientversioner hello: klienten för Windows 8 och Windows 10-klient.</span><span class="sxs-lookup"><span data-stu-id="5eec6-210">Azure Disk Encryption is supported on hello following Windows client versions: Windows 8 client and Windows 10 client.</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-211">Windows Server 2008 R2, måste du ha .NET Framework 4.5 installerat innan du aktiverar kryptering i Azure.</span><span class="sxs-lookup"><span data-stu-id="5eec6-211">For Windows Server 2008 R2, you must have .NET Framework 4.5 installed before you enable encryption in Azure.</span></span> <span data-ttu-id="5eec6-212">Du kan installera det från Windows Update genom att installera hello valfri uppdatering Microsoft .NET Framework 4.5.2 för Windows Server 2008 R2 x64-baserade system ([KB2901983](https://support.microsoft.com/kb/2901983)).</span><span class="sxs-lookup"><span data-stu-id="5eec6-212">You can install it from Windows Update by installing hello optional update Microsoft .NET Framework 4.5.2 for Windows Server 2008 R2 x64-based systems ([KB2901983](https://support.microsoft.com/kb/2901983)).</span></span>

* <span data-ttu-id="5eec6-213">Det finns stöd för Azure Disk Encryption hello följande Azure-galleriet baserad på Linux-server-distributioner och versioner:</span><span class="sxs-lookup"><span data-stu-id="5eec6-213">Azure Disk Encryption is supported on hello following Azure Gallery based Linux server distributions and versions:</span></span>

| <span data-ttu-id="5eec6-214">Linux-Distribution</span><span class="sxs-lookup"><span data-stu-id="5eec6-214">Linux Distribution</span></span> | <span data-ttu-id="5eec6-215">Version</span><span class="sxs-lookup"><span data-stu-id="5eec6-215">Version</span></span> | <span data-ttu-id="5eec6-216">Volymtyp som stöds för kryptering</span><span class="sxs-lookup"><span data-stu-id="5eec6-216">Volume Type Supported for Encryption</span></span>|
| --- | --- |--- |
| <span data-ttu-id="5eec6-217">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="5eec6-217">Ubuntu</span></span> | <span data-ttu-id="5eec6-218">16.04-VARJE DAG-LTS</span><span class="sxs-lookup"><span data-stu-id="5eec6-218">16.04-DAILY-LTS</span></span> | <span data-ttu-id="5eec6-219">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="5eec6-219">OS and Data disk</span></span> |
| <span data-ttu-id="5eec6-220">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="5eec6-220">Ubuntu</span></span> | <span data-ttu-id="5eec6-221">14.04.5-DAILY-LTS</span><span class="sxs-lookup"><span data-stu-id="5eec6-221">14.04.5-DAILY-LTS</span></span> | <span data-ttu-id="5eec6-222">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="5eec6-222">OS and Data disk</span></span> |
| <span data-ttu-id="5eec6-223">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="5eec6-223">Ubuntu</span></span> | <span data-ttu-id="5eec6-224">12.10</span><span class="sxs-lookup"><span data-stu-id="5eec6-224">12.10</span></span> | <span data-ttu-id="5eec6-225">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-225">Data disk</span></span> |
| <span data-ttu-id="5eec6-226">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="5eec6-226">Ubuntu</span></span> | <span data-ttu-id="5eec6-227">12.04</span><span class="sxs-lookup"><span data-stu-id="5eec6-227">12.04</span></span> | <span data-ttu-id="5eec6-228">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-228">Data disk</span></span> |
| <span data-ttu-id="5eec6-229">RHEL</span><span class="sxs-lookup"><span data-stu-id="5eec6-229">RHEL</span></span> | <span data-ttu-id="5eec6-230">7.3</span><span class="sxs-lookup"><span data-stu-id="5eec6-230">7.3</span></span> | <span data-ttu-id="5eec6-231">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="5eec6-231">OS and Data disk</span></span> |
| <span data-ttu-id="5eec6-232">RHEL</span><span class="sxs-lookup"><span data-stu-id="5eec6-232">RHEL</span></span> | <span data-ttu-id="5eec6-233">7.2</span><span class="sxs-lookup"><span data-stu-id="5eec6-233">7.2</span></span> | <span data-ttu-id="5eec6-234">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="5eec6-234">OS and Data disk</span></span> |
| <span data-ttu-id="5eec6-235">RHEL</span><span class="sxs-lookup"><span data-stu-id="5eec6-235">RHEL</span></span> | <span data-ttu-id="5eec6-236">6.8</span><span class="sxs-lookup"><span data-stu-id="5eec6-236">6.8</span></span> | <span data-ttu-id="5eec6-237">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="5eec6-237">OS and Data disk</span></span> |
| <span data-ttu-id="5eec6-238">RHEL</span><span class="sxs-lookup"><span data-stu-id="5eec6-238">RHEL</span></span> | <span data-ttu-id="5eec6-239">6.7</span><span class="sxs-lookup"><span data-stu-id="5eec6-239">6.7</span></span> | <span data-ttu-id="5eec6-240">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-240">Data disk</span></span> |
| <span data-ttu-id="5eec6-241">CentOS</span><span class="sxs-lookup"><span data-stu-id="5eec6-241">CentOS</span></span> | <span data-ttu-id="5eec6-242">7.3</span><span class="sxs-lookup"><span data-stu-id="5eec6-242">7.3</span></span> | <span data-ttu-id="5eec6-243">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="5eec6-243">OS and Data disk</span></span> |
| <span data-ttu-id="5eec6-244">CentOS</span><span class="sxs-lookup"><span data-stu-id="5eec6-244">CentOS</span></span> | <span data-ttu-id="5eec6-245">7.2n</span><span class="sxs-lookup"><span data-stu-id="5eec6-245">7.2n</span></span> | <span data-ttu-id="5eec6-246">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="5eec6-246">OS and Data disk</span></span> |
| <span data-ttu-id="5eec6-247">CentOS</span><span class="sxs-lookup"><span data-stu-id="5eec6-247">CentOS</span></span> | <span data-ttu-id="5eec6-248">6.8</span><span class="sxs-lookup"><span data-stu-id="5eec6-248">6.8</span></span> | <span data-ttu-id="5eec6-249">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="5eec6-249">OS and Data disk</span></span> |
| <span data-ttu-id="5eec6-250">CentOS</span><span class="sxs-lookup"><span data-stu-id="5eec6-250">CentOS</span></span> | <span data-ttu-id="5eec6-251">7.1</span><span class="sxs-lookup"><span data-stu-id="5eec6-251">7.1</span></span> | <span data-ttu-id="5eec6-252">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-252">Data disk</span></span> |
| <span data-ttu-id="5eec6-253">CentOS</span><span class="sxs-lookup"><span data-stu-id="5eec6-253">CentOS</span></span> | <span data-ttu-id="5eec6-254">7.0</span><span class="sxs-lookup"><span data-stu-id="5eec6-254">7.0</span></span> | <span data-ttu-id="5eec6-255">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-255">Data disk</span></span> |
| <span data-ttu-id="5eec6-256">CentOS</span><span class="sxs-lookup"><span data-stu-id="5eec6-256">CentOS</span></span> | <span data-ttu-id="5eec6-257">6.7</span><span class="sxs-lookup"><span data-stu-id="5eec6-257">6.7</span></span> | <span data-ttu-id="5eec6-258">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-258">Data disk</span></span> |
| <span data-ttu-id="5eec6-259">CentOS</span><span class="sxs-lookup"><span data-stu-id="5eec6-259">CentOS</span></span> | <span data-ttu-id="5eec6-260">6.6</span><span class="sxs-lookup"><span data-stu-id="5eec6-260">6.6</span></span> | <span data-ttu-id="5eec6-261">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-261">Data disk</span></span> |
| <span data-ttu-id="5eec6-262">CentOS</span><span class="sxs-lookup"><span data-stu-id="5eec6-262">CentOS</span></span> | <span data-ttu-id="5eec6-263">6.5</span><span class="sxs-lookup"><span data-stu-id="5eec6-263">6.5</span></span> | <span data-ttu-id="5eec6-264">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-264">Data disk</span></span> |
| <span data-ttu-id="5eec6-265">openSUSE</span><span class="sxs-lookup"><span data-stu-id="5eec6-265">openSUSE</span></span> | <span data-ttu-id="5eec6-266">13.2</span><span class="sxs-lookup"><span data-stu-id="5eec6-266">13.2</span></span> | <span data-ttu-id="5eec6-267">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-267">Data disk</span></span> |
| <span data-ttu-id="5eec6-268">SLES</span><span class="sxs-lookup"><span data-stu-id="5eec6-268">SLES</span></span> | <span data-ttu-id="5eec6-269">12 SP1</span><span class="sxs-lookup"><span data-stu-id="5eec6-269">12 SP1</span></span> | <span data-ttu-id="5eec6-270">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-270">Data disk</span></span> |
| <span data-ttu-id="5eec6-271">SLES</span><span class="sxs-lookup"><span data-stu-id="5eec6-271">SLES</span></span> | <span data-ttu-id="5eec6-272">12-SP1 (Premium)</span><span class="sxs-lookup"><span data-stu-id="5eec6-272">12-SP1 (Premium)</span></span> | <span data-ttu-id="5eec6-273">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-273">Data disk</span></span> |
| <span data-ttu-id="5eec6-274">SLES</span><span class="sxs-lookup"><span data-stu-id="5eec6-274">SLES</span></span> | <span data-ttu-id="5eec6-275">HPC 12</span><span class="sxs-lookup"><span data-stu-id="5eec6-275">HPC 12</span></span> | <span data-ttu-id="5eec6-276">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-276">Data disk</span></span> |
| <span data-ttu-id="5eec6-277">SLES</span><span class="sxs-lookup"><span data-stu-id="5eec6-277">SLES</span></span> | <span data-ttu-id="5eec6-278">11 SP4 (Premium)</span><span class="sxs-lookup"><span data-stu-id="5eec6-278">11-SP4 (Premium)</span></span> | <span data-ttu-id="5eec6-279">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-279">Data disk</span></span> |
| <span data-ttu-id="5eec6-280">SLES</span><span class="sxs-lookup"><span data-stu-id="5eec6-280">SLES</span></span> | <span data-ttu-id="5eec6-281">11 SP4</span><span class="sxs-lookup"><span data-stu-id="5eec6-281">11 SP4</span></span> | <span data-ttu-id="5eec6-282">Datadisk</span><span class="sxs-lookup"><span data-stu-id="5eec6-282">Data disk</span></span> |

* <span data-ttu-id="5eec6-283">Azure Disk Encryption kräver att dina nyckelvalvet och virtuella datorer finns i hello samma Azure-region och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5eec6-283">Azure Disk Encryption requires that your key vault and VMs reside in hello same Azure region and subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-284">Konfigurera hello resurser i olika områden orsakar ett fel i hello Azure Disk Encryption funktionen aktiveras.</span><span class="sxs-lookup"><span data-stu-id="5eec6-284">Configuring hello resources in separate regions causes a failure in enabling hello Azure Disk Encryption feature.</span></span>

* <span data-ttu-id="5eec6-285">tooset upp och konfigurera nyckelvalvet för Azure Disk Encryption, se avsnittet **ställa upp och konfigurera nyckelvalvet för Azure Disk Encryption** i hello *krav* i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5eec6-285">tooset up and configure your key vault for Azure Disk Encryption, see section **Set up and configure your key vault for Azure Disk Encryption** in hello *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="5eec6-286">tooset upp och konfigurera Azure AD-program i Azure Active directory för Azure Disk Encryption, se avsnittet **konfigurera hello Azure AD-program i Azure Active Directory** i hello *krav* avsnitt i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5eec6-286">tooset up and configure Azure AD application in Azure Active directory for Azure Disk Encryption, see section **Set up hello Azure AD application in Azure Active Directory** in hello *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="5eec6-287">tooset upp och konfigurerar hello nyckelvalv åtkomstprincip för hello Azure AD-program, finns i avsnittet **konfigurera hello nyckelvalv åtkomstprincip för hello Azure AD application** i hello *krav* avsnitt i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5eec6-287">tooset up and configure hello key vault access policy for hello Azure AD application, see section **Set up hello key vault access policy for hello Azure AD application** in hello *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="5eec6-288">tooprepare en förkrypterade Windows VHD finns i avsnittet **förbereda en förkrypterade Windows VHD** i hello *bilaga*.</span><span class="sxs-lookup"><span data-stu-id="5eec6-288">tooprepare a pre-encrypted Windows VHD, see section **Prepare a pre-encrypted Windows VHD** in hello *Appendix*.</span></span>
* <span data-ttu-id="5eec6-289">tooprepare en förkrypterade Linux VHD finns i avsnittet **förbereda en förkrypterade Linux VHD** i hello *bilaga*.</span><span class="sxs-lookup"><span data-stu-id="5eec6-289">tooprepare a pre-encrypted Linux VHD, see section **Prepare a pre-encrypted Linux VHD** in hello *Appendix*.</span></span>
* <span data-ttu-id="5eec6-290">hello Azure-plattformen måste åtkomst toohello krypteringsnycklar och hemligheter i nyckelvalvet-toomake dem tillgängliga toohello virtuella datorn när den startar och dekrypterar hello virtuella OS volym.</span><span class="sxs-lookup"><span data-stu-id="5eec6-290">hello Azure platform needs access toohello encryption keys or secrets in your key vault toomake them available toohello virtual machine when it boots and decrypts hello virtual machine OS volume.</span></span> <span data-ttu-id="5eec6-291">toogrant behörigheter tooAzure plattform, ange hello **EnabledForDiskEncryption** egenskap i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-291">toogrant permissions tooAzure platform, set hello **EnabledForDiskEncryption** property in hello key vault.</span></span> <span data-ttu-id="5eec6-292">Mer information finns i **ställa upp och konfigurera nyckelvalvet för Azure Disk Encryption** i hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="5eec6-292">For more information, see **Set up and configure your key vault for Azure Disk Encryption** in hello Appendix.</span></span>
* <span data-ttu-id="5eec6-293">Ditt nyckelvalv hemlighet och KEK URL: er måste vara en ny version.</span><span class="sxs-lookup"><span data-stu-id="5eec6-293">Your key vault secret and KEK URLs must be versioned.</span></span> <span data-ttu-id="5eec6-294">Azure tillämpar den här begränsningen för versionshantering.</span><span class="sxs-lookup"><span data-stu-id="5eec6-294">Azure enforces this restriction of versioning.</span></span> <span data-ttu-id="5eec6-295">KEK URL: er och giltig hemlighet finns hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="5eec6-295">For valid secret and KEK URLs, see hello following examples:</span></span>

  * <span data-ttu-id="5eec6-296">Exempel på en giltig hemliga URL: *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="5eec6-296">Example of a valid secret URL:   *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>
  * <span data-ttu-id="5eec6-297">Exempel på en giltig URL för KEK: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="5eec6-297">Example of a valid KEK URL:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>

* <span data-ttu-id="5eec6-298">Azure Disk Encryption stöder inte att ange portnummer som en del av KEK URL: er och hemligheter i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-298">Azure Disk Encryption does not support specifying port numbers as part of key vault secrets and KEK URLs.</span></span> <span data-ttu-id="5eec6-299">Exempel på inte stöds och stöds nyckelvalv URL: er finns i hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-299">For examples of non-supported and supported key vault URLs, see hello following:</span></span>

  * <span data-ttu-id="5eec6-300">Oacceptabel key vault-URL *https://contosovault.vault.azure.net:443/hemligheter/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="5eec6-300">Unacceptable key vault URL  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>
  * <span data-ttu-id="5eec6-301">Godkända key vault-URL: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="5eec6-301">Acceptable key vault URL:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>

* <span data-ttu-id="5eec6-302">tooenable hello Azure Disk Encryption funktionen hello virtuella IaaS-datorer måste uppfylla hello följande nätverkskrav endpoint konfiguration:</span><span class="sxs-lookup"><span data-stu-id="5eec6-302">tooenable hello Azure Disk Encryption feature, hello IaaS VMs must meet hello following network endpoint configuration requirements:</span></span>
  * <span data-ttu-id="5eec6-303">tooget en token tooconnect tooyour nyckelvalvet hello IaaS VM måste vara kan tooconnect tooan Azure Active Directory-slutpunkt, \[login.microsoftonline.com\].</span><span class="sxs-lookup"><span data-stu-id="5eec6-303">tooget a token tooconnect tooyour key vault, hello IaaS VM must be able tooconnect tooan Azure Active Directory endpoint, \[login.microsoftonline.com\].</span></span>
  * <span data-ttu-id="5eec6-304">toowrite hello kryptering nycklar tooyour nyckelvalvet hello IaaS VM måste vara kan tooconnect toohello nyckelvalv slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="5eec6-304">toowrite hello encryption keys tooyour key vault, hello IaaS VM must be able tooconnect toohello key vault endpoint.</span></span>
  * <span data-ttu-id="5eec6-305">Hej IaaS VM måste vara kan tooconnect tooan Azure storage endpoint att värdar hello Azure-tillägget databasen och ett Azure storage-konto att värdar hello VHD-filer.</span><span class="sxs-lookup"><span data-stu-id="5eec6-305">hello IaaS VM must be able tooconnect tooan Azure storage endpoint that hosts hello Azure extension repository and an Azure storage account that hosts hello VHD files.</span></span>

  > [!NOTE]
  > <span data-ttu-id="5eec6-306">Om din säkerhetsprincip begränsar åtkomst från virtuella datorer i Azure toohello Internet, kan du lösa hello föregående URI och konfigurera en specifik regel tooallow utgående anslutning toohello IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="5eec6-306">If your security policy limits access from Azure VMs toohello Internet, you can resolve hello preceding URI and configure a specific rule tooallow outbound connectivity toohello IPs.</span></span>
  >
  ><span data-ttu-id="5eec6-307">tooconfigure och åtkomst till Azure Key Vault bakom en brandvägg (https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall)</span><span class="sxs-lookup"><span data-stu-id="5eec6-307">tooconfigure and access Azure Key Vault behind a firewall(https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall)</span></span>

* <span data-ttu-id="5eec6-308">Använd hello senaste versionen av Azure PowerShell SDK version tooconfigure Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="5eec6-308">Use hello latest version of Azure PowerShell SDK version tooconfigure Azure Disk Encryption.</span></span> <span data-ttu-id="5eec6-309">Hämta hello senaste versionen av [versionen av Azure PowerShell](https://github.com/Azure/azure-powershell/releases)</span><span class="sxs-lookup"><span data-stu-id="5eec6-309">Download hello latest version of [Azure PowerShell release](https://github.com/Azure/azure-powershell/releases)</span></span>

 > [!NOTE]
  > <span data-ttu-id="5eec6-310">Azure Disk Encryption stöds inte på [Azure PowerShell SDK version 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016).</span><span class="sxs-lookup"><span data-stu-id="5eec6-310">Azure Disk Encryption is not supported on [Azure PowerShell SDK version 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016).</span></span> <span data-ttu-id="5eec6-311">Om du får ett fel relaterade toousing Azure PowerShell 1.1.0, se [Azure Disk Encryption fel relaterade tooAzure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).</span><span class="sxs-lookup"><span data-stu-id="5eec6-311">If you are receiving an error related toousing Azure PowerShell 1.1.0, see [Azure Disk Encryption Error Related tooAzure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).</span></span>

* <span data-ttu-id="5eec6-312">toorun alla Azure CLI-kommandon och koppla den till din Azure-prenumeration, måste du först installera Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="5eec6-312">toorun any Azure CLI command and associate it with your Azure subscription, you must first install Azure CLI:</span></span>
  * <span data-ttu-id="5eec6-313">tooinstall Azure CLI och koppla den till din Azure-prenumeration, se [hur tooinstall och konfigurera Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="5eec6-313">tooinstall Azure CLI and associate it with your Azure subscription, see [How tooinstall and configure Azure CLI](../cli-install-nodejs.md).</span></span>
  * <span data-ttu-id="5eec6-314">toouse Azure CLI för Mac, Linux och Windows med Azure Resource Manager finns [Azure CLI-kommandona i Resource Manager-läget](../virtual-machines/azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="5eec6-314">toouse Azure CLI for Mac, Linux, and Windows with Azure Resource Manager, see [Azure CLI commands in Resource Manager mode](../virtual-machines/azure-cli-arm-commands.md).</span></span>

* <span data-ttu-id="5eec6-315">När du krypterar en hanterade diskar är obligatoriska nödvändiga tootake en ögonblicksbild av hello hanteras disk eller en säkerhetskopia av hello disk utanför Azure Disk Encryption tidigare tooenabling kryptering.</span><span class="sxs-lookup"><span data-stu-id="5eec6-315">When encrypting a managed disk, it is mandatory prerequisite tootake a snapshot of hello managed disk or a backup of hello disk outside of Azure Disk Encryption prior tooenabling encryption.</span></span>  <span data-ttu-id="5eec6-316">Utan en säkerhetskopia för kan ett oväntat fel under krypteringen återge hello disk- och Virtuella tillgänglig utan ett återställningsalternativ.</span><span class="sxs-lookup"><span data-stu-id="5eec6-316">Without a backup in place, any unexpected failure during encryption may render hello disk and VM inaccessible without a recovery option.</span></span>  <span data-ttu-id="5eec6-317">Ange AzureRmVMDiskEncryptionExtension för närvarande inte säkerhetskopiera hanterade diskar och kommer fel om användas mot en hanterad disk såvida inte hello - skipVmBackup parametern har angetts.</span><span class="sxs-lookup"><span data-stu-id="5eec6-317">Set-AzureRmVMDiskEncryptionExtension does not currently back up managed disks and will error if used against a managed disk unless hello -skipVmBackup parameter has been specified.</span></span>  <span data-ttu-id="5eec6-318">Den här parametern är osäkra toouse om inte en säkerhetskopia har redan gjorts utanför Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="5eec6-318">This parameter is unsafe toouse unless a backup has already been made outside of Azure Disk Encryption.</span></span>   <span data-ttu-id="5eec6-319">När hello skipVmBackup - parametern anges, hello cmdlet inte att göra en säkerhetskopia av hello hanterade disken tidigare tooencryption.</span><span class="sxs-lookup"><span data-stu-id="5eec6-319">When hello -skipVmBackup parameter is specified, hello cmdlet will not make a backup of hello managed disk prior tooencryption.</span></span>  <span data-ttu-id="5eec6-320">Därför anses en obligatorisk nödvändiga toomake till en säkerhetskopia av hello hanterade diskar virtuella datorn är i drift tidigare tooenabling Azure Disk Encryption återställning är senare behövs.</span><span class="sxs-lookup"><span data-stu-id="5eec6-320">For this reason, it is considered a mandatory prerequisite toomake sure a backup of hello managed disk VM is in place prior tooenabling Azure Disk Encryption in case recovery is later needed.</span></span>  
> [!NOTE]
 > <span data-ttu-id="5eec6-321">hello - skipVmBackup parametern ska aldrig användas om en ögonblicksbild eller säkerhetskopiering har redan gjorts utanför Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="5eec6-321">hello -skipVmBackup parameter should never be used unless a snapshot or backup has already been made outside of Azure Disk Encryption.</span></span> 

* <span data-ttu-id="5eec6-322">hello Azure Disk Encryption lösningen använder hello externa nyckelskydd för BitLocker för Windows IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-322">hello Azure Disk Encryption solution uses hello BitLocker external key protector for Windows IaaS VMs.</span></span> <span data-ttu-id="5eec6-323">Domän push anslutna virtuella datorer, inte i alla grupprinciper som genomdriva TPM-skydd.</span><span class="sxs-lookup"><span data-stu-id="5eec6-323">For domain joined VMs, DO NOT push any group policies that enforce TPM protectors.</span></span> <span data-ttu-id="5eec6-324">Information om hello Grupprincip för ”Tillåt BitLocker utan en kompatibel TPM” finns [BitLocker gruppolicy referens](https://technet.microsoft.com/library/ee706521).</span><span class="sxs-lookup"><span data-stu-id="5eec6-324">For information about hello group policy for “Allow BitLocker without a compatible TPM,” see [BitLocker Group Policy Reference](https://technet.microsoft.com/library/ee706521).</span></span>
* <span data-ttu-id="5eec6-325">BitLocker-principen på domänanslutna virtuella datorer med anpassade Grupprincip måste innehålla hello följande inställning: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key` Azure Disk Encryption misslyckas när anpassade grupprincipinställningarna för Bitlocker är inkompatibla.</span><span class="sxs-lookup"><span data-stu-id="5eec6-325">Bitlocker policy on domain joined virtual machines with custom group policy must include hello following setting: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key`  Azure Disk Encryption will fail when custom group policy settings for Bitlocker are incompatible.</span></span> <span data-ttu-id="5eec6-326">På datorer som inte hade hello kan rätt principinställning hello nya principer, tillämpas att tvinga hello ny princip tooupdate (gpupdate.exe/Force) och starta sedan om krävas.</span><span class="sxs-lookup"><span data-stu-id="5eec6-326">On machines that did not have hello correct policy setting, applying hello new policy, forcing hello new policy tooupdate (gpupdate.exe /force), and then restarting may be required.</span></span>  
* <span data-ttu-id="5eec6-327">toocreate ett Azure AD-program, skapa nyckelvalvet, eller konfigurera en befintlig nyckelvalv och aktivera kryptering, se hello [PowerShell-skript för Azure Disk Encryption nödvändiga](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).</span><span class="sxs-lookup"><span data-stu-id="5eec6-327">toocreate an Azure AD application, create a key vault, or set up an existing key vault and enable encryption, see hello [Azure Disk Encryption prerequisite PowerShell script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).</span></span>
* <span data-ttu-id="5eec6-328">tooconfigure-diskkryptering krav med hello Azure CLI, se [Bash skriptet](https://github.com/ejarvi/ade-cli-getting-started).</span><span class="sxs-lookup"><span data-stu-id="5eec6-328">tooconfigure disk-encryption prerequisites using hello Azure CLI, see [this Bash script](https://github.com/ejarvi/ade-cli-getting-started).</span></span>
* <span data-ttu-id="5eec6-329">kryptera dina virtuella datorer med hjälp av hello Azure Disk Encryption key configuration toouse hello Azure Backup service tooback upp och återställa krypterade virtuella datorer, vid kryptering har aktiverats med Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="5eec6-329">toouse hello Azure Backup service tooback up and restore encrypted VMs, when encryption is enabled with Azure Disk Encryption, encrypt your VMs by using hello Azure Disk Encryption key configuration.</span></span> <span data-ttu-id="5eec6-330">hello Backup-tjänsten har stöd för virtuella datorer som krypteras med endast KEK-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5eec6-330">hello Backup service supports VMs that are encrypted using KEK configuration only.</span></span> <span data-ttu-id="5eec6-331">Se [hur tooback upp och återställning av krypterade virtuella datorer med Azure Backup kryptering](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).</span><span class="sxs-lookup"><span data-stu-id="5eec6-331">See [How tooback up and restore encrypted virtual machines with Azure Backup  encryption](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).</span></span>

* <span data-ttu-id="5eec6-332">När du krypterar en Linux OS-volym, Observera att en VM-omstart krävs för närvarande hello slutet av hello-processen.</span><span class="sxs-lookup"><span data-stu-id="5eec6-332">When encrypting a Linux OS volume, note that a VM restart is currently required at hello end of hello process.</span></span> <span data-ttu-id="5eec6-333">Detta kan göras via hello portal, powershell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="5eec6-333">This can be done via hello portal, powershell, or CLI.</span></span>   <span data-ttu-id="5eec6-334">tootrack hello förloppet för kryptering, regelbundet avsöka hello statusmeddelande som returneras av Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus.</span><span class="sxs-lookup"><span data-stu-id="5eec6-334">tootrack hello progress of encryption, periodically poll hello status message returned by Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus.</span></span>  <span data-ttu-id="5eec6-335">När kryptering är klar anger hello hello statusmeddelande som returneras av det här kommandot detta.</span><span class="sxs-lookup"><span data-stu-id="5eec6-335">Once encryption is complete, hello hello status message returned by this command will indicate this.</span></span>  <span data-ttu-id="5eec6-336">Till exempel ”ProgressMessage: OS-disken har krypterats, starta om hello VM” på den här punkten hello VM kan startas om och användas.</span><span class="sxs-lookup"><span data-stu-id="5eec6-336">For example, "ProgressMessage: OS disk successfully encrypted, please reboot hello VM"  At this point hello VM can be restarted and used.</span></span>  

* <span data-ttu-id="5eec6-337">Azure Disk Encryption för Linux kräver data diskar toohave ett anslutet filsystem i Linux tidigare tooencryption</span><span class="sxs-lookup"><span data-stu-id="5eec6-337">Azure Disk Encryption for Linux requires data disks toohave a mounted file system in Linux prior tooencryption</span></span>

* <span data-ttu-id="5eec6-338">Rekursivt monterade diskar inte stöds av hello Azure Disk Encryption för Linux.</span><span class="sxs-lookup"><span data-stu-id="5eec6-338">Recursively mounted data disks are not supported by hello Azure Disk Encryption for Linux.</span></span> <span data-ttu-id="5eec6-339">Till exempel om hello målsystemet har monterats en disk på /foo/bar och sedan en annan på /foo/bar/baz hello kryptering av /foo/bar/baz lyckas, men misslyckas kryptering av/foo/stapel.</span><span class="sxs-lookup"><span data-stu-id="5eec6-339">For example, if hello target system has mounted a disk on /foo/bar and then another on /foo/bar/baz, hello encryption of /foo/bar/baz will succeed, but encryption of /foo/bar will fail.</span></span> 

* <span data-ttu-id="5eec6-340">Azure Disk Encryption stöds bara på Azure stöds galleriavbildningar som uppfyller dessa krav för hello.</span><span class="sxs-lookup"><span data-stu-id="5eec6-340">Azure Disk Encryption is only supported on Azure gallery supported images that meet hello aforementioned prerequisites.</span></span> <span data-ttu-id="5eec6-341">Kunden anpassade avbildningar stöds inte på grund av toocustom partition system och processen beteenden som kan finnas på dessa bilder.</span><span class="sxs-lookup"><span data-stu-id="5eec6-341">Customer custom images are not supported due toocustom partition schemes and process behaviors that may exist on these images.</span></span> <span data-ttu-id="5eec6-342">Dessutom Bildbaserad även galleriet Virtuella datorer som ursprungligen uppfyllda förutsättningar men har ändrats efter att skapa kan vara inkompatibla.</span><span class="sxs-lookup"><span data-stu-id="5eec6-342">Further, even gallery image based VM's that initially met prerequisites but have been modified after creation may be incompatible.</span></span>  <span data-ttu-id="5eec6-343">För att därför hello förslag proceduren för att kryptera en Linux VM toostart från en avbildning av ren galleriet, kryptera hello VM och sedan lägga till anpassad programvara eller data toohello VM efter behov.</span><span class="sxs-lookup"><span data-stu-id="5eec6-343">For that reason, hello suggested procedure for encrypting a Linux VM is toostart from a clean gallery image, encrypt hello VM, and then add custom software or data toohello VM as needed.</span></span>  

> [!NOTE]
> <span data-ttu-id="5eec6-344">Säkerhetskopiering och återställning av krypterade virtuella datorer stöds bara för virtuella datorer som är krypterade med hello KEK konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5eec6-344">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with hello KEK configuration.</span></span> <span data-ttu-id="5eec6-345">Den stöds inte på virtuella datorer som är krypterade utan KEK.</span><span class="sxs-lookup"><span data-stu-id="5eec6-345">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="5eec6-346">KEK är en valfri parameter som möjliggör VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-346">KEK is an optional parameter that enables VM.</span></span>

#### <a name="set-up-hello-azure-ad-application-in-azure-active-directory"></a><span data-ttu-id="5eec6-347">Ställ in hello Azure AD-program i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5eec6-347">Set up hello Azure AD application in Azure Active Directory</span></span>
<span data-ttu-id="5eec6-348">När du behöver kryptering toobe aktiverad på en aktiv virtuell dator i Azure, Azure Disk Encryption genererar och skriver hello kryptering nycklar tooyour nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-348">When you need encryption toobe enabled on a running VM in Azure, Azure Disk Encryption generates and writes hello encryption keys tooyour key vault.</span></span> <span data-ttu-id="5eec6-349">Hantera krypteringsnycklar i nyckelvalvet kräver Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="5eec6-349">Managing encryption keys in your key vault requires Azure AD authentication.</span></span>

<span data-ttu-id="5eec6-350">Skapa ett Azure AD-program för det här ändamålet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-350">For this purpose, create an Azure AD application.</span></span> <span data-ttu-id="5eec6-351">Du kan hitta detaljerade anvisningar för att registrera ett program under hello ”hämta en identitet för hello programmet” hello blogginlägget [Azure Key Vault - steg-för-steg](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="5eec6-351">You can find detailed steps for registering an application in hello “Get an Identity for hello Application” section of hello blog post [Azure Key Vault - Step by Step](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span></span> <span data-ttu-id="5eec6-352">Det här exemplet innehåller också ett antal användbara exempel för att installera och konfigurera nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-352">This post also contains a number of helpful examples for setting up and configuring your key vault.</span></span> <span data-ttu-id="5eec6-353">Du kan använda client secret-baserad autentisering eller klientautentisering certifikatbaserad Azure AD för autentisering.</span><span class="sxs-lookup"><span data-stu-id="5eec6-353">For authentication purposes, you can use either client secret-based authentication or client certificate-based Azure AD authentication.</span></span>

#### <a name="client-secret-based-authentication-for-azure-ad"></a><span data-ttu-id="5eec6-354">Klienten hemlighet-baserad autentisering för Azure AD</span><span class="sxs-lookup"><span data-stu-id="5eec6-354">Client secret-based authentication for Azure AD</span></span>
<span data-ttu-id="5eec6-355">hello avsnitten som följer kan hjälpa dig att konfigurera en hemlighet-baserade klientautentisering för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5eec6-355">hello sections that follow can help you configure a client secret-based authentication for Azure AD.</span></span>

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a><span data-ttu-id="5eec6-356">Skapa ett Azure AD-program med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5eec6-356">Create an Azure AD application by using Azure PowerShell</span></span>
<span data-ttu-id="5eec6-357">Använd följande PowerShell-cmdlet toocreate en Azure AD-programmet hello:</span><span class="sxs-lookup"><span data-stu-id="5eec6-357">Use hello following PowerShell cmdlet toocreate an Azure AD application:</span></span>

    $aadClientSecret = "yourSecret"
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> <span data-ttu-id="5eec6-358">$azureAdApplication.ApplicationId är hello Azure AD ClientID och $aadClientSecret är att du ska använda senare tooenable Azure Disk Encryption hello klienthemlighet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-358">$azureAdApplication.ApplicationId is hello Azure AD ClientID and $aadClientSecret is hello client secret that you should use later tooenable Azure Disk Encryption.</span></span> <span data-ttu-id="5eec6-359">Skydda hello Azure AD-klienthemlighet på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="5eec6-359">Safeguard hello Azure AD client secret appropriately.</span></span>

##### <a name="setting-up-hello-azure-ad-client-id-and-secret-from-hello-azure-classic-portal"></a><span data-ttu-id="5eec6-360">Ställa in hello Azure AD-klient-ID och Hemlig från hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5eec6-360">Setting up hello Azure AD client ID and secret from hello Azure classic portal</span></span>
<span data-ttu-id="5eec6-361">Du kan också ställa in din Azure AD-klient-ID och Hemlig med hjälp av hello [klassiska Azure-portalen]( https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="5eec6-361">You can also set up your Azure AD client ID and secret by using hello [Azure classic portal]( https://manage.windowsazure.com).</span></span> <span data-ttu-id="5eec6-362">tooperform detta uppgift, hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-362">tooperform this task, do hello following:</span></span>

1. <span data-ttu-id="5eec6-363">Klicka på hello **Active Directory** fliken.</span><span class="sxs-lookup"><span data-stu-id="5eec6-363">Click hello **Active Directory** tab.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. <span data-ttu-id="5eec6-365">Klicka på **Lägg till program**, och sedan hello programmet typnamnet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-365">Click **Add Application**, and then type hello application name.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. <span data-ttu-id="5eec6-367">Klicka på hello pilknappen och sedan konfigurera hello programegenskaper.</span><span class="sxs-lookup"><span data-stu-id="5eec6-367">Click hello arrow button, and then configure hello application properties.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. <span data-ttu-id="5eec6-369">Klicka på hello hello nedre vänstra hörnet toofinish är markerat.</span><span class="sxs-lookup"><span data-stu-id="5eec6-369">Click hello check mark in hello lower left corner toofinish.</span></span> <span data-ttu-id="5eec6-370">hello konfigurationssidan för programmet, och hello Azure AD-klient-ID visas på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="5eec6-370">hello application configuration page appears, and hello Azure AD client ID is displayed at hello bottom of hello page.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. <span data-ttu-id="5eec6-372">Spara hello Azure AD-klienthemlighet genom att klicka på hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5eec6-372">Save hello Azure AD client secret by clicking hello **Save** button.</span></span> <span data-ttu-id="5eec6-373">Observera hello Azure AD klienthemlighet i textrutan för hello nycklar.</span><span class="sxs-lookup"><span data-stu-id="5eec6-373">Note hello Azure AD client secret in hello keys text box.</span></span> <span data-ttu-id="5eec6-374">Skydda på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="5eec6-374">Safeguard it appropriately.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig7.png)

 > [!NOTE]
 > <span data-ttu-id="5eec6-376">hello föregående flödet stöds inte på hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5eec6-376">hello preceding flow is not supported on hello Azure classic portal.</span></span>

##### <a name="use-an-existing-application"></a><span data-ttu-id="5eec6-377">Använda ett befintligt program</span><span class="sxs-lookup"><span data-stu-id="5eec6-377">Use an existing application</span></span>
<span data-ttu-id="5eec6-378">tooexecute hello följande kommandon, hämta och använda hello [Azure AD PowerShell-modulen](https://technet.microsoft.com/library/jj151815.aspx).</span><span class="sxs-lookup"><span data-stu-id="5eec6-378">tooexecute hello following commands, obtain and use hello [Azure AD PowerShell module](https://technet.microsoft.com/library/jj151815.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-379">hello måste följande kommandon köras från en ny PowerShell-fönstret.</span><span class="sxs-lookup"><span data-stu-id="5eec6-379">hello following commands must be executed from a new PowerShell window.</span></span> <span data-ttu-id="5eec6-380">Använd inte Azure PowerShell eller hello Azure Resource Manager tooexecute hello kommandon.</span><span class="sxs-lookup"><span data-stu-id="5eec6-380">Do not use Azure PowerShell or hello Azure Resource Manager window tooexecute hello commands.</span></span> <span data-ttu-id="5eec6-381">Vi rekommenderar den här metoden eftersom dessa cmdlets finns i hello MSOnline modul eller Azure AD PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5eec6-381">We recommend this approach because these cmdlets are in hello MSOnline module or Azure AD PowerShell.</span></span>

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a><span data-ttu-id="5eec6-382">Certifikatbaserad autentisering för Azure AD</span><span class="sxs-lookup"><span data-stu-id="5eec6-382">Certificate-based authentication for Azure AD</span></span>
> [!NOTE]
> <span data-ttu-id="5eec6-383">Azure AD certifikatbaserad autentisering stöds för närvarande inte på Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5eec6-383">Azure AD certificate-based authentication is currently not supported on Linux VMs.</span></span>

<span data-ttu-id="5eec6-384">Hej avsnitten som följer visa hur tooconfigure certifikatbaserad autentisering för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5eec6-384">hello sections that follow show how tooconfigure a certificate-based authentication for Azure AD.</span></span>

##### <a name="create-an-azure-ad-application"></a><span data-ttu-id="5eec6-385">Skapa en Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="5eec6-385">Create an Azure AD application</span></span>
<span data-ttu-id="5eec6-386">toocreate ett Azure AD-program, köra hello följande PowerShell-cmdlets:</span><span class="sxs-lookup"><span data-stu-id="5eec6-386">toocreate an Azure AD application, execute hello following PowerShell cmdlets:</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-387">Ersätt hello följande `yourpassword` sträng med säkra lösenord och skydda hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="5eec6-387">Replace hello following `yourpassword` string with your secure password, and safeguard hello password.</span></span>

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

<span data-ttu-id="5eec6-388">När du har slutfört det här steget överför nyckelvalv en PFX-filen tooyour och aktivera hello åtkomst principinformation som behövs toodeploy som certifikatet tooa VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-388">After you finish this step, upload a PFX file tooyour key vault and enable hello access policy needed toodeploy that certificate tooa VM.</span></span>

##### <a name="use-an-existing-azure-ad-application"></a><span data-ttu-id="5eec6-389">Använda ett befintligt Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="5eec6-389">Use an existing Azure AD application</span></span>
<span data-ttu-id="5eec6-390">Om du konfigurerar certifikatbaserad autentisering för ett befintligt program använda hello PowerShell-cmdlets som visas här.</span><span class="sxs-lookup"><span data-stu-id="5eec6-390">If you are configuring certificate-based authentication for an existing application, use hello PowerShell cmdlets shown here.</span></span> <span data-ttu-id="5eec6-391">Vara säker på att tooexecute dem från ett nytt PowerShell-fönster.</span><span class="sxs-lookup"><span data-stu-id="5eec6-391">Be sure tooexecute them from a new PowerShell window.</span></span>

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

<span data-ttu-id="5eec6-392">När du har slutfört det här steget överför nyckelvalv en PFX-filen tooyour och aktivera hello åtkomstprincip som behövs för toodeploy hello certifikat tooa VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-392">After you finish this step, upload a PFX file tooyour key vault and enable hello access policy that's needed toodeploy hello certificate tooa VM.</span></span>

##### <a name="upload-a-pfx-file-tooyour-key-vault"></a><span data-ttu-id="5eec6-393">Överför nyckelvalv en PFX-filen tooyour</span><span class="sxs-lookup"><span data-stu-id="5eec6-393">Upload a PFX file tooyour key vault</span></span>
<span data-ttu-id="5eec6-394">En detaljerad förklaring av den här processen finns [hello officiella Azure Key Vault-teamets blogg](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx).</span><span class="sxs-lookup"><span data-stu-id="5eec6-394">For a detailed explanation of this process, see [hello Official Azure Key Vault Team Blog](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx).</span></span> <span data-ttu-id="5eec6-395">Hello följande PowerShell-cmdlets är allt du behöver för hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-395">However, hello following PowerShell cmdlets are all you need for hello task.</span></span> <span data-ttu-id="5eec6-396">Vara säker på att tooexecute dem från Azure PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="5eec6-396">Be sure tooexecute them from Azure PowerShell console.</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-397">Ersätt hello följande `yourpassword` sträng med säkra lösenord och skydda hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="5eec6-397">Replace hello following `yourpassword` string with your secure password, and safeguard hello password.</span></span>

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### <a name="deploy-a-certificate-in-your-key-vault-tooan-existing-vm"></a><span data-ttu-id="5eec6-398">Distribuera ett certifikat i ditt nyckelvalv tooan befintliga VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-398">Deploy a certificate in your key vault tooan existing VM</span></span>
<span data-ttu-id="5eec6-399">När du är klar med att ladda upp hello PFX distribuera ett certifikat i hello nyckelvalv tooan befintlig virtuell dator med hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-399">After you finish uploading hello PFX, deploy a certificate in hello key vault tooan existing VM with hello following:</span></span>
 ```
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName
 ```

#### <a name="set-up-hello-key-vault-access-policy-for-hello-azure-ad-application"></a><span data-ttu-id="5eec6-400">Ställ in hello nyckelvalv åtkomstprincip för hello Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="5eec6-400">Set up hello key vault access policy for hello Azure AD application</span></span>
<span data-ttu-id="5eec6-401">Azure AD-program måste rättigheter tooaccess hello nycklar och hemligheter i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-401">Your Azure AD application needs rights tooaccess hello keys or secrets in hello vault.</span></span> <span data-ttu-id="5eec6-402">Använd hello [ `Set-AzureKeyVaultAccessPolicy` ](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) cmdlet toogrant behörigheter toohello, används hello klient-ID (som skapades när programmet hello registrerades) som programmet hello _– ServicePrincipalName_ parametervärdet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-402">Use hello [`Set-AzureKeyVaultAccessPolicy`](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) cmdlet toogrant permissions toohello application, using hello client ID (which was generated when hello application was registered) as hello _–ServicePrincipalName_ parameter value.</span></span> <span data-ttu-id="5eec6-403">toolearn finns fler hello bloggposten [Azure Key Vault - steg-för-steg](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="5eec6-403">toolearn more, see hello blog post [Azure Key Vault - Step by Step](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span></span> <span data-ttu-id="5eec6-404">Här är ett exempel på hur tooperform detta uppgift PowerShell:</span><span class="sxs-lookup"><span data-stu-id="5eec6-404">Here is an example of how tooperform this task via PowerShell:</span></span>

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> <span data-ttu-id="5eec6-405">Azure Disk Encryption kräver tooconfigure hello följande åtkomst principer tooyour Azure AD-klientprogram: _WrapKey_ och _ange_ behörigheter.</span><span class="sxs-lookup"><span data-stu-id="5eec6-405">Azure Disk Encryption requires you tooconfigure hello following access policies tooyour Azure AD client application: _WrapKey_ and _Set_ permissions.</span></span>

## <a name="terminology"></a><span data-ttu-id="5eec6-406">Terminologi</span><span class="sxs-lookup"><span data-stu-id="5eec6-406">Terminology</span></span>
<span data-ttu-id="5eec6-407">toounderstand vissa hello vanliga termer används av den här tekniken, Använd hello följande terminologi tabell:</span><span class="sxs-lookup"><span data-stu-id="5eec6-407">toounderstand some of hello common terms used by this technology, use hello following terminology table:</span></span>

| <span data-ttu-id="5eec6-408">Terminologi</span><span class="sxs-lookup"><span data-stu-id="5eec6-408">Terminology</span></span> | <span data-ttu-id="5eec6-409">Definition</span><span class="sxs-lookup"><span data-stu-id="5eec6-409">Definition</span></span> |
| --- | --- |
| <span data-ttu-id="5eec6-410">Azure AD</span><span class="sxs-lookup"><span data-stu-id="5eec6-410">Azure AD</span></span> | <span data-ttu-id="5eec6-411">Azure AD [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="5eec6-411">Azure AD is [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).</span></span> <span data-ttu-id="5eec6-412">Azure AD-kontot är en förutsättning för autentisering, lagra och hämta hemligheter från en nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-412">An Azure AD account is a prerequisite for authenticating, storing, and retrieving secrets from a key vault.</span></span> |
| <span data-ttu-id="5eec6-413">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5eec6-413">Azure Key Vault</span></span> | <span data-ttu-id="5eec6-414">Key Vault är en kryptografisk, key management-tjänst som baseras på FIPS Federal Information Processing Standards validerade maskinvarusäkerhetsmoduler, som bidrar till att skydda din kryptografiska nycklar och hemligheter känslig.</span><span class="sxs-lookup"><span data-stu-id="5eec6-414">Key Vault is a cryptographic, key management service that's based on Federal Information Processing Standards (FIPS)-validated hardware security modules, which help safeguard your cryptographic keys and sensitive secrets.</span></span> <span data-ttu-id="5eec6-415">Mer information finns i [Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="5eec6-415">For more information, see [Key Vault](https://azure.microsoft.com/services/key-vault/) documentation.</span></span> |
| <span data-ttu-id="5eec6-416">ARM</span><span class="sxs-lookup"><span data-stu-id="5eec6-416">ARM</span></span> | <span data-ttu-id="5eec6-417">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5eec6-417">Azure Resource Manager</span></span> |
| <span data-ttu-id="5eec6-418">BitLocker</span><span class="sxs-lookup"><span data-stu-id="5eec6-418">BitLocker</span></span> |<span data-ttu-id="5eec6-419">[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) är en branschstandard identifieras Windows volym krypteringsteknik som har använt tooenable diskkryptering på Windows IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-419">[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) is an industry-recognized Windows volume encryption technology that's used tooenable disk encryption on Windows IaaS VMs.</span></span> |
| <span data-ttu-id="5eec6-420">BEK</span><span class="sxs-lookup"><span data-stu-id="5eec6-420">BEK</span></span> | <span data-ttu-id="5eec6-421">Krypteringsnycklarna i BitLocker är används tooencrypt hello OS startvolymen och datavolymer.</span><span class="sxs-lookup"><span data-stu-id="5eec6-421">BitLocker encryption keys are used tooencrypt hello OS boot volume and data volumes.</span></span> <span data-ttu-id="5eec6-422">hello BitLocker nycklar skyddas i ett nyckelvalv som hemligheter.</span><span class="sxs-lookup"><span data-stu-id="5eec6-422">hello BitLocker keys are safeguarded in a key vault as secrets.</span></span> |
| <span data-ttu-id="5eec6-423">CLI</span><span class="sxs-lookup"><span data-stu-id="5eec6-423">CLI</span></span> | <span data-ttu-id="5eec6-424">Se [Azure-kommandoradsgränssnittet](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="5eec6-424">See [Azure command-line interface](../cli-install-nodejs.md).</span></span> |
| <span data-ttu-id="5eec6-425">DM-Crypt</span><span class="sxs-lookup"><span data-stu-id="5eec6-425">DM-Crypt</span></span> |<span data-ttu-id="5eec6-426">[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) är hello Linux-baserade och transparent diskkryptering delsystem som har använt tooenable diskkryptering på Linux IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-426">[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) is hello Linux-based, transparent disk-encryption subsystem that's used tooenable disk encryption on Linux IaaS VMs.</span></span> |
| <span data-ttu-id="5eec6-427">KEK</span><span class="sxs-lookup"><span data-stu-id="5eec6-427">KEK</span></span> | <span data-ttu-id="5eec6-428">Viktiga krypteringsnyckeln är hello asymmetrisk nyckel (RSA 2048) att du kan använda tooprotect eller omsluter hello hemlighet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-428">Key encryption key is hello asymmetric key (RSA 2048) that you can use tooprotect or wrap hello secret.</span></span> <span data-ttu-id="5eec6-429">Du kan ange en säkerhets-och maskinvara moduler (HSM)-skyddad nyckel eller programvaruskyddad nyckel.</span><span class="sxs-lookup"><span data-stu-id="5eec6-429">You can provide a hardware security modules (HSM)-protected key or software-protected key.</span></span> <span data-ttu-id="5eec6-430">Mer information finns i [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="5eec6-430">For more details, see [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentation.</span></span> |
| <span data-ttu-id="5eec6-431">PS-cmdlets</span><span class="sxs-lookup"><span data-stu-id="5eec6-431">PS cmdlets</span></span> | <span data-ttu-id="5eec6-432">Se [Azure PowerShell-cmdlets](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5eec6-432">See [Azure PowerShell cmdlets](/powershell/azure/overview).</span></span> |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a><span data-ttu-id="5eec6-433">Installera och konfigurera nyckelvalvet för Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="5eec6-433">Set up and configure your key vault for Azure Disk Encryption</span></span>
<span data-ttu-id="5eec6-434">Azure Disk Encryption hjälper till att skydda hello-diskkryptering nycklar och hemligheter i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-434">Azure Disk Encryption helps safeguard hello disk-encryption keys and secrets in your key vault.</span></span> <span data-ttu-id="5eec6-435">tooset in nyckelvalvet för Azure Disk Encryption fullständig hello steg i varje hello följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5eec6-435">tooset up your key vault for Azure Disk Encryption, complete hello steps in each of hello following sections.</span></span>

#### <a name="create-a-key-vault"></a><span data-ttu-id="5eec6-436">Skapa ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="5eec6-436">Create a key vault</span></span>
<span data-ttu-id="5eec6-437">toocreate nyckelvalvet, med någon av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="5eec6-437">toocreate a key vault, use one of hello following options:</span></span>

* [<span data-ttu-id="5eec6-438">”101-nyckel-valvet-skapa” Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="5eec6-438">"101-Key-Vault-Create" Resource Manager template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
* [<span data-ttu-id="5eec6-439">Azure PowerShell nyckelvalv-cmdlets</span><span class="sxs-lookup"><span data-stu-id="5eec6-439">Azure PowerShell key vault cmdlets</span></span>](/powershell/module/azurerm.keyvault/#key_vault)
* <span data-ttu-id="5eec6-440">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5eec6-440">Azure Resource Manager</span></span>
* <span data-ttu-id="5eec6-441">Hur för[Secure nyckelvalvet](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)</span><span class="sxs-lookup"><span data-stu-id="5eec6-441">How too[Secure your key vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-442">Om du redan har installerat en nyckelvalv för din prenumeration, hoppar du över toohello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5eec6-442">If you have already set up a key vault for your subscription, skip toohello next section.</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a><span data-ttu-id="5eec6-444">Ställa in en krypteringsnyckel nyckel (valfritt)</span><span class="sxs-lookup"><span data-stu-id="5eec6-444">Set up a key encryption key (optional)</span></span>
<span data-ttu-id="5eec6-445">Lägg till en KEK tooyour nyckelvalv om du vill toouse en KEK för ett extra säkerhetslager för hello BitLocker krypteringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="5eec6-445">If you want toouse a KEK for an additional layer of security for hello BitLocker encryption keys, add a KEK tooyour key vault.</span></span> <span data-ttu-id="5eec6-446">Använd hello [ `Add-AzureKeyVaultKey` ](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet toocreate en krypteringsnyckel nyckel i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-446">Use hello [`Add-AzureKeyVaultKey`](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet toocreate a key encryption key in hello key vault.</span></span> <span data-ttu-id="5eec6-447">Du kan också importera en KEK från din lokala nyckelhantering HSM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-447">You can also import a KEK from your on-premises key management HSM.</span></span> <span data-ttu-id="5eec6-448">Mer information finns i [Key Vault dokumentationen](https://azure.microsoft.com/documentation/services/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="5eec6-448">For more details, see [Key Vault Documentation](https://azure.microsoft.com/documentation/services/key-vault/).</span></span>

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

<span data-ttu-id="5eec6-449">Du kan lägga till hello KEK genom att gå tooAzure Resource Manager eller genom att använda nyckelvalv-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-449">You can add hello KEK by going tooAzure Resource Manager or by using your key vault interface.</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a><span data-ttu-id="5eec6-451">Ange behörigheter för nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="5eec6-451">Set key vault permissions</span></span>
<span data-ttu-id="5eec6-452">hello Azure-plattformen måste åtkomst toohello krypteringsnycklar och hemligheter i nyckelvalvet-toomake dem tillgängliga toohello VM för att starta och dekryptera hello volymer.</span><span class="sxs-lookup"><span data-stu-id="5eec6-452">hello Azure platform needs access toohello encryption keys or secrets in your key vault toomake them available toohello VM for booting and decrypting hello volumes.</span></span> <span data-ttu-id="5eec6-453">toogrant behörigheter toohello Azure-plattformen, ange hello **EnabledForDiskEncryption** egenskap i hello nyckeln valvet med hello nyckelvalv PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="5eec6-453">toogrant permissions toohello Azure platform, set hello **EnabledForDiskEncryption** property in hello key vault by using hello key vault PowerShell cmdlet:</span></span>

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

<span data-ttu-id="5eec6-454">Du kan också ange hello **EnabledForDiskEncryption** egenskapen genom att besöka hello [resursutforskaren Azure](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5eec6-454">You can also set hello **EnabledForDiskEncryption** property by visiting hello [Azure Resource Explorer](https://resources.azure.com).</span></span>

<span data-ttu-id="5eec6-455">Som tidigare nämnts kan du ange hello **EnabledForDiskEncryption** -egenskapen i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-455">As mentioned earlier, you must set hello **EnabledForDiskEncryption** property on your key vault.</span></span> <span data-ttu-id="5eec6-456">Annars misslyckas hello distributionen.</span><span class="sxs-lookup"><span data-stu-id="5eec6-456">Otherwise, hello deployment will fail.</span></span>

<span data-ttu-id="5eec6-457">Du kan konfigurera principer för ditt Azure AD-program från hello nyckelvalv gränssnitt som visas här:</span><span class="sxs-lookup"><span data-stu-id="5eec6-457">You can set up access policies for your Azure AD application from hello key vault interface, as shown here:</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

<span data-ttu-id="5eec6-460">På hello **avancerade åtkomstprinciper** och se till att nyckelvalvet är aktiverat för Azure Disk Encryption:</span><span class="sxs-lookup"><span data-stu-id="5eec6-460">On hello **Advanced access policies** tab, make sure that your key vault is enabled for Azure Disk Encryption:</span></span>

![Azure key vault](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a><span data-ttu-id="5eec6-462">Distributionsscenarier för disk-kryptering och användarupplevelser</span><span class="sxs-lookup"><span data-stu-id="5eec6-462">Disk-encryption deployment scenarios and user experiences</span></span>
<span data-ttu-id="5eec6-463">Du kan aktivera många scenarier för disk-kryptering och hello steg kan variera bl.a toohello scenario.</span><span class="sxs-lookup"><span data-stu-id="5eec6-463">You can enable many disk-encryption scenarios, and hello steps may vary according toohello scenario.</span></span> <span data-ttu-id="5eec6-464">hello beskriver följande avsnitt hello scenarier i detalj.</span><span class="sxs-lookup"><span data-stu-id="5eec6-464">hello following sections cover hello scenarios in greater detail.</span></span>

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-hello-marketplace"></a><span data-ttu-id="5eec6-465">Aktivera kryptering på den nya virtuella IaaS-datorer som skapas från hello Marketplace</span><span class="sxs-lookup"><span data-stu-id="5eec6-465">Enable encryption on new IaaS VMs that are created from hello Marketplace</span></span>
<span data-ttu-id="5eec6-466">Du kan aktivera kryptering på nya Windows IaaS-VM från hello Marketplace i Azure med hjälp av hello [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).</span><span class="sxs-lookup"><span data-stu-id="5eec6-466">You can enable disk encryption on new IaaS Windows VM from hello Marketplace in Azure by using hello  [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).</span></span>

1. <span data-ttu-id="5eec6-467">Klicka på hello Azure Snabbkurs mallen **distribuera tooAzure**, ange hello kryptering konfiguration på hello **parametrar** bladet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5eec6-467">On hello Azure quick-start template, click **Deploy tooAzure**, enter hello encryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="5eec6-468">Välj hello prenumeration, resursgrupp, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** tooenable kryptering på en ny IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-468">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on a new IaaS VM.</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-469">Den här mallen skapar en ny krypterade Windows virtuell dator som använder bild för hello Windows Server 2012-galleriet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-469">This template creates a new encrypted Windows VM that uses hello Windows Server 2012 gallery image.</span></span>

<span data-ttu-id="5eec6-470">Du kan aktivera kryptering på en ny IaaS RedHat Linux 7.2 virtuell dator med en 200 GB RAID-0-matris med den här [Resource Manager-mall](https://aka.ms/fde-rhel).</span><span class="sxs-lookup"><span data-stu-id="5eec6-470">You can enable disk encryption on a new IaaS RedHat Linux 7.2 VM with a 200-GB RAID-0 array by using this [Resource Manager template](https://aka.ms/fde-rhel).</span></span> <span data-ttu-id="5eec6-471">När du har distribuerat hello mallen verifiera hello VM krypteringsstatus med hello `Get-AzureRmVmDiskEncryptionStatus` cmdlet, enligt beskrivningen i [kryptera OS enhet på en körande Linux VM](#encrypting-os-drive-on-a-running-linux-vm).</span><span class="sxs-lookup"><span data-stu-id="5eec6-471">After you deploy hello template, verify hello VM encryption status by using hello `Get-AzureRmVmDiskEncryptionStatus` cmdlet, as described in [Encrypting OS drive on a running Linux VM](#encrypting-os-drive-on-a-running-linux-vm).</span></span> <span data-ttu-id="5eec6-472">När hello datorn returnerar statusvärdet _VMRestartPending_, starta om hello VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-472">When hello machine returns a status of _VMRestartPending_, restart hello VM.</span></span>

<span data-ttu-id="5eec6-473">hello följande tabell listar hello Resource Manager-mallens parametrar för nya virtuella datorer från hello Marketplace-scenario med Azure AD-klient-ID:</span><span class="sxs-lookup"><span data-stu-id="5eec6-473">hello following table lists hello Resource Manager template parameters for new VMs from hello Marketplace scenario using Azure AD client ID:</span></span>

| <span data-ttu-id="5eec6-474">Parameter</span><span class="sxs-lookup"><span data-stu-id="5eec6-474">Parameter</span></span> | <span data-ttu-id="5eec6-475">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5eec6-475">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5eec6-476">adminUserName</span><span class="sxs-lookup"><span data-stu-id="5eec6-476">adminUserName</span></span> | <span data-ttu-id="5eec6-477">Admin användarnamn för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5eec6-477">Admin user name for hello virtual machine.</span></span> |
| <span data-ttu-id="5eec6-478">adminPassword</span><span class="sxs-lookup"><span data-stu-id="5eec6-478">adminPassword</span></span> | <span data-ttu-id="5eec6-479">Administratörslösenord för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5eec6-479">Admin user password for hello virtual machine.</span></span> |
| <span data-ttu-id="5eec6-480">newStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="5eec6-480">newStorageAccountName</span></span> | <span data-ttu-id="5eec6-481">Namnet på hello storage-konto toostore OS och data virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="5eec6-481">Name of hello storage account toostore OS and data VHDs.</span></span> |
| <span data-ttu-id="5eec6-482">vmSize</span><span class="sxs-lookup"><span data-stu-id="5eec6-482">vmSize</span></span> | <span data-ttu-id="5eec6-483">Storleken på hello VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-483">Size of hello VM.</span></span> <span data-ttu-id="5eec6-484">För närvarande stöds endast Standard A, D och G serien.</span><span class="sxs-lookup"><span data-stu-id="5eec6-484">Currently, only Standard A, D, and G series are supported.</span></span> |
| <span data-ttu-id="5eec6-485">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="5eec6-485">virtualNetworkName</span></span> | <span data-ttu-id="5eec6-486">Namnet på hello VNet som hello VM NIC ska tillhöra.</span><span class="sxs-lookup"><span data-stu-id="5eec6-486">Name of hello VNet that hello VM NIC should belong to.</span></span> |
| <span data-ttu-id="5eec6-487">subnetName</span><span class="sxs-lookup"><span data-stu-id="5eec6-487">subnetName</span></span> | <span data-ttu-id="5eec6-488">Namnet på hello undernät i hello VNet som hello VM NIC ska tillhöra.</span><span class="sxs-lookup"><span data-stu-id="5eec6-488">Name of hello subnet in hello VNet that hello VM NIC should belong to.</span></span> |
| <span data-ttu-id="5eec6-489">AADClientID</span><span class="sxs-lookup"><span data-stu-id="5eec6-489">AADClientID</span></span> | <span data-ttu-id="5eec6-490">Klient-ID för hello Azure AD-program som har behörigheter toowrite hemligheter tooyour nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-490">Client ID of hello Azure AD application that has permissions toowrite secrets tooyour key vault.</span></span> |
| <span data-ttu-id="5eec6-491">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="5eec6-491">AADClientSecret</span></span> | <span data-ttu-id="5eec6-492">Klienthemlighet för hello Azure AD-program som har behörigheter toowrite hemligheter tooyour nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-492">Client secret of hello Azure AD application that has permissions toowrite secrets tooyour key vault.</span></span> |
| <span data-ttu-id="5eec6-493">keyVaultURL</span><span class="sxs-lookup"><span data-stu-id="5eec6-493">keyVaultURL</span></span> | <span data-ttu-id="5eec6-494">URL för hello nyckeln valvet som hello BitLocker nyckel ska överföras till.</span><span class="sxs-lookup"><span data-stu-id="5eec6-494">URL of hello key vault that hello BitLocker key should be uploaded to.</span></span> <span data-ttu-id="5eec6-495">Du kan hämta den med hjälp av cmdlet hello `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`.</span><span class="sxs-lookup"><span data-stu-id="5eec6-495">You can get it by using hello cmdlet `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`.</span></span> |
| <span data-ttu-id="5eec6-496">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="5eec6-496">keyEncryptionKeyURL</span></span> | <span data-ttu-id="5eec6-497">URL för hello nyckelkryptering nyckel som används tooencrypt hello genereras BitLocker-nyckel (valfritt).</span><span class="sxs-lookup"><span data-stu-id="5eec6-497">URL of hello key encryption key that's used tooencrypt hello generated BitLocker key (optional).</span></span> |
| <span data-ttu-id="5eec6-498">keyVaultResourceGroup</span><span class="sxs-lookup"><span data-stu-id="5eec6-498">keyVaultResourceGroup</span></span> | <span data-ttu-id="5eec6-499">Resursgruppen för hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-499">Resource group of hello key vault.</span></span> |
| <span data-ttu-id="5eec6-500">vmName</span><span class="sxs-lookup"><span data-stu-id="5eec6-500">vmName</span></span> | <span data-ttu-id="5eec6-501">Namnet på hello VM som hello krypteringsåtgärden är toobe utförs på.</span><span class="sxs-lookup"><span data-stu-id="5eec6-501">Name of hello VM that hello encryption operation is toobe performed on.</span></span> |

> [!NOTE]
> <span data-ttu-id="5eec6-502">_KeyEncryptionKeyURL_ är en valfri parameter.</span><span class="sxs-lookup"><span data-stu-id="5eec6-502">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="5eec6-503">Du kan sätta egna KEK toofurther skydda hello datakrypteringsnyckeln (lösenfrasen hemlig) i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-503">You can bring your own KEK toofurther safeguard hello data encryption key (Passphrase secret) in your key vault.</span></span>

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a><span data-ttu-id="5eec6-504">Aktivera kryptering på den nya virtuella IaaS-datorer som skapas från kund-krypterad VHD- och krypteringsnycklar</span><span class="sxs-lookup"><span data-stu-id="5eec6-504">Enable encryption on new IaaS VMs that are created from customer-encrypted VHD and encryption keys</span></span>
<span data-ttu-id="5eec6-505">Du kan aktivera kryptering genom att använda hello Resource Manager-mall, PowerShell-cmdlets eller CLI-kommandona i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="5eec6-505">In this scenario, you can enable encrypting by using hello Resource Manager template, PowerShell cmdlets, or CLI commands.</span></span> <span data-ttu-id="5eec6-506">hello följande avsnitt beskrivs i större detalj hello Resource Manager-mall och CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="5eec6-506">hello following sections explain in greater detail hello Resource Manager template and CLI commands.</span></span>

<span data-ttu-id="5eec6-507">Följ instruktionerna för hello från någon av dessa avsnitt för att förbereda inför krypterade avbildningar som kan användas i Azure.</span><span class="sxs-lookup"><span data-stu-id="5eec6-507">Follow hello instructions from one of these sections for preparing pre-encrypted images that can be used in Azure.</span></span> <span data-ttu-id="5eec6-508">När hello avbildningen har skapats kan kan du använda hello steg i hello nästa avsnitt toocreate en krypterad Azure VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-508">After hello image is created, you can use hello steps in hello next section toocreate an encrypted Azure VM.</span></span>

* [<span data-ttu-id="5eec6-509">Förbereda en förkrypterade Windows VHD</span><span class="sxs-lookup"><span data-stu-id="5eec6-509">Prepare a pre-encrypted Windows VHD</span></span>](#preparing-a-pre-encrypted-windows-vhd)
* [<span data-ttu-id="5eec6-510">Förbereda en förkrypterade Linux VHD</span><span class="sxs-lookup"><span data-stu-id="5eec6-510">Prepare a pre-encrypted Linux VHD</span></span>](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-hello-resource-manager-template"></a><span data-ttu-id="5eec6-511">Med hjälp av hello Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="5eec6-511">Using hello Resource Manager template</span></span>
<span data-ttu-id="5eec6-512">Du kan aktivera kryptering på den kryptera virtuella Hårddisken med hjälp av hello [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).</span><span class="sxs-lookup"><span data-stu-id="5eec6-512">You can enable disk encryption on your encrypted VHD by using hello [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).</span></span>

1. <span data-ttu-id="5eec6-513">Klicka på hello Azure Snabbkurs mallen **distribuera tooAzure**, ange hello kryptering konfiguration på hello **parametrar** bladet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5eec6-513">On hello Azure quick-start template, click **Deploy tooAzure**, enter hello encryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="5eec6-514">Välj hello prenumeration, resursgrupp, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** tooenable kryptering på hello nya IaaS VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-514">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on hello new IaaS VM.</span></span>

<span data-ttu-id="5eec6-515">hello följande tabell listar hello Resource Manager-mallens parametrar för den kryptera virtuella Hårddisken:</span><span class="sxs-lookup"><span data-stu-id="5eec6-515">hello following table lists hello Resource Manager template parameters for your encrypted VHD:</span></span>

| <span data-ttu-id="5eec6-516">Parameter</span><span class="sxs-lookup"><span data-stu-id="5eec6-516">Parameter</span></span> | <span data-ttu-id="5eec6-517">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5eec6-517">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5eec6-518">newStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="5eec6-518">newStorageAccountName</span></span> | <span data-ttu-id="5eec6-519">Namnet på hello storage-konto toostore hello krypterad OS-VHD.</span><span class="sxs-lookup"><span data-stu-id="5eec6-519">Name of hello storage account toostore hello encrypted OS VHD.</span></span> <span data-ttu-id="5eec6-520">Det här lagringskontot bör redan har skapats i hello samma resursgrupp och samma plats som hello VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-520">This storage account should already have been created in hello same resource group and same location as hello VM.</span></span> |
| <span data-ttu-id="5eec6-521">osVhdUri</span><span class="sxs-lookup"><span data-stu-id="5eec6-521">osVhdUri</span></span> | <span data-ttu-id="5eec6-522">URI för hello OS VHD från hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="5eec6-522">URI of hello OS VHD from hello storage account.</span></span> |
| <span data-ttu-id="5eec6-523">osType</span><span class="sxs-lookup"><span data-stu-id="5eec6-523">osType</span></span> | <span data-ttu-id="5eec6-524">OS-produkttyp (Windows-/ Linux).</span><span class="sxs-lookup"><span data-stu-id="5eec6-524">OS product type (Windows/Linux).</span></span> |
| <span data-ttu-id="5eec6-525">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="5eec6-525">virtualNetworkName</span></span> | <span data-ttu-id="5eec6-526">Namnet på hello VNet som hello VM NIC ska tillhöra.</span><span class="sxs-lookup"><span data-stu-id="5eec6-526">Name of hello VNet that hello VM NIC should belong to.</span></span> <span data-ttu-id="5eec6-527">hello namn bör redan har skapats i hello samma resursgrupp och samma plats som hello VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-527">hello name should already have been created in hello same resource group and same location as hello VM.</span></span> |
| <span data-ttu-id="5eec6-528">subnetName</span><span class="sxs-lookup"><span data-stu-id="5eec6-528">subnetName</span></span> | <span data-ttu-id="5eec6-529">Namnet på hello undernät på hello VNet som hello VM NIC ska tillhöra.</span><span class="sxs-lookup"><span data-stu-id="5eec6-529">Name of hello subnet on hello VNet that hello VM NIC should belong to.</span></span> |
| <span data-ttu-id="5eec6-530">vmSize</span><span class="sxs-lookup"><span data-stu-id="5eec6-530">vmSize</span></span> | <span data-ttu-id="5eec6-531">Storleken på hello VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-531">Size of hello VM.</span></span> <span data-ttu-id="5eec6-532">För närvarande stöds endast Standard A, D och G serien.</span><span class="sxs-lookup"><span data-stu-id="5eec6-532">Currently, only Standard A, D, and G series are supported.</span></span> |
| <span data-ttu-id="5eec6-533">keyVaultResourceID</span><span class="sxs-lookup"><span data-stu-id="5eec6-533">keyVaultResourceID</span></span> | <span data-ttu-id="5eec6-534">hello ResourceID som identifierar hello nyckelvalv resurs i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5eec6-534">hello ResourceID that identifies hello key vault resource in Azure Resource Manager.</span></span> <span data-ttu-id="5eec6-535">Du kan hämta den med hjälp av PowerShell-cmdlet för hello `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`.</span><span class="sxs-lookup"><span data-stu-id="5eec6-535">You can get it by using hello PowerShell cmdlet `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`.</span></span> |
| <span data-ttu-id="5eec6-536">keyVaultSecretUrl</span><span class="sxs-lookup"><span data-stu-id="5eec6-536">keyVaultSecretUrl</span></span> | <span data-ttu-id="5eec6-537">URL till hello disk-krypteringsnyckeln som har ställts in i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-537">URL of hello disk-encryption key that's set up in hello key vault.</span></span> |
| <span data-ttu-id="5eec6-538">keyVaultKekUrl</span><span class="sxs-lookup"><span data-stu-id="5eec6-538">keyVaultKekUrl</span></span> | <span data-ttu-id="5eec6-539">URL till hello nyckelkryptering nyckel för att kryptera hello genereras disk-krypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="5eec6-539">URL of hello key encryption key for encrypting hello generated disk-encryption key.</span></span> |
| <span data-ttu-id="5eec6-540">vmName</span><span class="sxs-lookup"><span data-stu-id="5eec6-540">vmName</span></span> | <span data-ttu-id="5eec6-541">Namnet på hello IaaS VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-541">Name of hello IaaS VM.</span></span> |

#### <a name="using-powershell-cmdlets"></a><span data-ttu-id="5eec6-542">Med hjälp av PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="5eec6-542">Using PowerShell cmdlets</span></span>
<span data-ttu-id="5eec6-543">Du kan aktivera kryptering på den kryptera virtuella Hårddisken med hjälp av PowerShell-cmdlet för hello [ `Set-AzureRmVMOSDisk` ](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span><span class="sxs-lookup"><span data-stu-id="5eec6-543">You can enable disk encryption on your encrypted VHD by using hello PowerShell cmdlet [`Set-AzureRmVMOSDisk`](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span></span>  

#### <a name="using-cli-commands"></a><span data-ttu-id="5eec6-544">Med hjälp av CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="5eec6-544">Using CLI commands</span></span>
<span data-ttu-id="5eec6-545">kryptering av tooenable för det här scenariot med hjälp av CLI-kommandona hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-545">tooenable disk encryption for this scenario by using CLI commands, do hello following:</span></span>

1. <span data-ttu-id="5eec6-546">Ange åtkomstprinciper i nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="5eec6-546">Set access policies in your key vault:</span></span>

   * <span data-ttu-id="5eec6-547">Ange hello **EnabledForDiskEncryption** flaggan:</span><span class="sxs-lookup"><span data-stu-id="5eec6-547">Set hello **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * <span data-ttu-id="5eec6-548">Ange behörigheter tooAzure AD programmet toowrite hemligheter tooyour nyckelvalv:</span><span class="sxs-lookup"><span data-stu-id="5eec6-548">Set permissions tooAzure AD application toowrite secrets tooyour key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. <span data-ttu-id="5eec6-549">tooenable kryptering på en befintlig eller körs VM typ:</span><span class="sxs-lookup"><span data-stu-id="5eec6-549">tooenable encryption on an existing or running VM, type:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. <span data-ttu-id="5eec6-550">Hämta krypteringsstatus:</span><span class="sxs-lookup"><span data-stu-id="5eec6-550">Get encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. <span data-ttu-id="5eec6-551">tooenable kryptering på en ny virtuell dator från den kryptera virtuella Hårddisken, Använd hello följande parametrar med hello `azure vm create` kommando:</span><span class="sxs-lookup"><span data-stu-id="5eec6-551">tooenable encryption on a new VM from your encrypted VHD, use hello following parameters with hello `azure vm create` command:</span></span>

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a><span data-ttu-id="5eec6-552">Aktivera kryptering på befintlig eller körs IaaS Windows VM i Azure</span><span class="sxs-lookup"><span data-stu-id="5eec6-552">Enable encryption on existing or running IaaS Windows VM in Azure</span></span>
<span data-ttu-id="5eec6-553">Du kan aktivera kryptering genom att använda hello Resource Manager-mall, PowerShell-cmdlets eller CLI-kommandona i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="5eec6-553">In this scenario, you can enable encrypting by using hello Resource Manager template, PowerShell cmdlets, or CLI commands.</span></span> <span data-ttu-id="5eec6-554">hello följande avsnitt beskrivs i detalj hur tooenable den med hjälp av hello Resource Manager-mall och CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="5eec6-554">hello following sections explain in greater detail how tooenable it by using hello Resource Manager template and CLI commands.</span></span>

#### <a name="using-hello-resource-manager-template"></a><span data-ttu-id="5eec6-555">Med hjälp av hello Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="5eec6-555">Using hello Resource Manager template</span></span>
<span data-ttu-id="5eec6-556">Du kan aktivera kryptering på befintliga eller IaaS Windows virtuella datorer som körs i Azure med hjälp av hello [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="5eec6-556">You can enable disk encryption on existing or running IaaS Windows VMs in Azure by using hello [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).</span></span>

1. <span data-ttu-id="5eec6-557">Klicka på hello Azure Snabbkurs mallen **distribuera tooAzure**, ange hello kryptering konfiguration på hello **parametrar** bladet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5eec6-557">On hello Azure quick-start template, click **Deploy tooAzure**, enter hello encryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="5eec6-558">Välj hello prenumeration, resursgrupp, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** tooenable kryptering på hello befintlig eller använder IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-558">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on hello existing or running IaaS VM.</span></span>

<span data-ttu-id="5eec6-559">hello visas följande tabell hello Resource Manager mallparametrar för befintlig eller virtuella datorer som använder en Azure AD-klient-ID som körs:</span><span class="sxs-lookup"><span data-stu-id="5eec6-559">hello following table lists hello Resource Manager template parameters for existing or running VMs that use an Azure AD client ID:</span></span>

| <span data-ttu-id="5eec6-560">Parameter</span><span class="sxs-lookup"><span data-stu-id="5eec6-560">Parameter</span></span> | <span data-ttu-id="5eec6-561">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5eec6-561">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5eec6-562">AADClientID</span><span class="sxs-lookup"><span data-stu-id="5eec6-562">AADClientID</span></span> | <span data-ttu-id="5eec6-563">Klient-ID för hello Azure AD-program som har behörigheter toowrite hemligheter toohello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-563">Client ID of hello Azure AD application that has permissions toowrite secrets toohello key vault.</span></span> |
| <span data-ttu-id="5eec6-564">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="5eec6-564">AADClientSecret</span></span> | <span data-ttu-id="5eec6-565">Klienthemlighet för hello Azure AD-program som har behörigheter toowrite hemligheter toohello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-565">Client secret of hello Azure AD application that has permissions toowrite secrets toohello key vault.</span></span> |
| <span data-ttu-id="5eec6-566">keyVaultName</span><span class="sxs-lookup"><span data-stu-id="5eec6-566">keyVaultName</span></span> | <span data-ttu-id="5eec6-567">Namnet på nyckeln hello valvet som hello BitLocker nyckel ska överföras till.</span><span class="sxs-lookup"><span data-stu-id="5eec6-567">Name of hello key vault that hello BitLocker key should be uploaded to.</span></span> <span data-ttu-id="5eec6-568">Du kan hämta den med hjälp av cmdlet hello `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span><span class="sxs-lookup"><span data-stu-id="5eec6-568">You can get it by using hello cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span></span> |
|  <span data-ttu-id="5eec6-569">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="5eec6-569">keyEncryptionKeyURL</span></span> | <span data-ttu-id="5eec6-570">URL för hello nyckelkryptering nyckel som används tooencrypt hello genereras BitLocker-nyckel.</span><span class="sxs-lookup"><span data-stu-id="5eec6-570">URL of hello key encryption key that's used tooencrypt hello generated BitLocker key.</span></span> <span data-ttu-id="5eec6-571">Den här parametern är valfri om du väljer **nokek** i hello UseExistingKek nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="5eec6-571">This parameter is optional if you select **nokek** in hello UseExistingKek drop-down list.</span></span> <span data-ttu-id="5eec6-572">Om du väljer **kek** i hello UseExistingKek nedrullningsbara listan, måste du ange hello _keyEncryptionKeyURL_ värde.</span><span class="sxs-lookup"><span data-stu-id="5eec6-572">If you select **kek** in hello UseExistingKek drop-down list, you must enter hello _keyEncryptionKeyURL_ value.</span></span> |
| <span data-ttu-id="5eec6-573">volumeType</span><span class="sxs-lookup"><span data-stu-id="5eec6-573">volumeType</span></span> | <span data-ttu-id="5eec6-574">Typ av volymen som hello krypteringsåtgärden utförs på.</span><span class="sxs-lookup"><span data-stu-id="5eec6-574">Type of volume that hello encryption operation is performed on.</span></span> <span data-ttu-id="5eec6-575">Giltiga värden är _OS_, _Data_, och _alla_.</span><span class="sxs-lookup"><span data-stu-id="5eec6-575">Valid values are _OS_, _Data_, and _All_.</span></span> |
| <span data-ttu-id="5eec6-576">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="5eec6-576">sequenceVersion</span></span> | <span data-ttu-id="5eec6-577">Sekvens version av hello BitLocker igen.</span><span class="sxs-lookup"><span data-stu-id="5eec6-577">Sequence version of hello BitLocker operation.</span></span> <span data-ttu-id="5eec6-578">Öka det här versionsnumret varje gång en diskkryptering åtgärden utförs på hello samma virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="5eec6-578">Increment this version number every time a disk-encryption operation is performed on hello same VM.</span></span> |
| <span data-ttu-id="5eec6-579">vmName</span><span class="sxs-lookup"><span data-stu-id="5eec6-579">vmName</span></span> | <span data-ttu-id="5eec6-580">Namnet på hello VM som hello krypteringsåtgärden är toobe utförs på.</span><span class="sxs-lookup"><span data-stu-id="5eec6-580">Name of hello VM that hello encryption operation is toobe performed on.</span></span> |

> [!NOTE]
> <span data-ttu-id="5eec6-581">_KeyEncryptionKeyURL_ är en valfri parameter.</span><span class="sxs-lookup"><span data-stu-id="5eec6-581">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="5eec6-582">Du kan hämta din egen KEK toofurther skydda hello datakrypteringsnyckeln (BitLocker-kryptering hemliga) i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-582">You can bring your own KEK toofurther safeguard hello data encryption key (BitLocker encryption secret) in hello key vault.</span></span>

#### <a name="using-powershell-cmdlets"></a><span data-ttu-id="5eec6-583">Med hjälp av PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="5eec6-583">Using PowerShell cmdlets</span></span>
<span data-ttu-id="5eec6-584">Information om hur du aktiverar kryptering med Azure Disk Encryption med PowerShell-cmdlets finns hello blogginlägg [utforska Azure Disk Encryption med Azure PowerShell - del 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) och [utforska Azure Disk Encryption med Azure PowerShell - del 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span><span class="sxs-lookup"><span data-stu-id="5eec6-584">For information about enabling encryption with Azure Disk Encryption by using PowerShell cmdlets, see hello blog posts [Explore Azure Disk Encryption with Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) and [Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span></span>

#### <a name="using-cli-commands"></a><span data-ttu-id="5eec6-585">Med hjälp av CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="5eec6-585">Using CLI commands</span></span>
<span data-ttu-id="5eec6-586">tooenable kryptering på befintlig eller använder IaaS Windows VM i Azure med hjälp av CLI-kommandona hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-586">tooenable encryption on existing or running IaaS Windows VM in Azure using CLI commands, do hello following:</span></span>

1. <span data-ttu-id="5eec6-587">tooset åtkomstprinciper i hello nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="5eec6-587">tooset access policies in hello key vault:</span></span>
   * <span data-ttu-id="5eec6-588">Ange hello **EnabledForDiskEncryption** flaggan:</span><span class="sxs-lookup"><span data-stu-id="5eec6-588">Set hello **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * <span data-ttu-id="5eec6-589">Ange behörigheter tooAzure AD programmet toowrite hemligheter tooyour nyckelvalv:</span><span class="sxs-lookup"><span data-stu-id="5eec6-589">Set permissions tooAzure AD application toowrite secrets tooyour key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. <span data-ttu-id="5eec6-590">tooenable kryptering på en befintlig eller körs VM:</span><span class="sxs-lookup"><span data-stu-id="5eec6-590">tooenable encryption on an existing or running VM:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. <span data-ttu-id="5eec6-591">tooget krypteringsstatus:</span><span class="sxs-lookup"><span data-stu-id="5eec6-591">tooget encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. <span data-ttu-id="5eec6-592">tooenable kryptering på en ny virtuell dator från den kryptera virtuella Hårddisken, Använd hello följande parametrar med hello `azure vm create` kommando:</span><span class="sxs-lookup"><span data-stu-id="5eec6-592">tooenable encryption on a new VM from your encrypted VHD, use hello following parameters with hello `azure vm create` command:</span></span>

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a><span data-ttu-id="5eec6-593">Aktivera kryptering på en befintlig eller körs IaaS Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="5eec6-593">Enable encryption on an existing or running IaaS Linux VM in Azure</span></span>
<span data-ttu-id="5eec6-594">Du kan aktivera kryptering på en befintlig eller körs IaaS Linux-dator i Azure med hjälp av hello [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).</span><span class="sxs-lookup"><span data-stu-id="5eec6-594">You can enable disk encryption on an existing or running IaaS Linux VM in Azure by using hello [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).</span></span>

1. <span data-ttu-id="5eec6-595">Klicka på **distribuera tooAzure** ange hello kryptering konfiguration på hello på hello Azure Snabbkurs mallen **parametrar** bladet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5eec6-595">Click **Deploy tooAzure** on hello Azure quick-start template, enter hello encryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="5eec6-596">Välj hello prenumeration, resursgrupp, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** tooenable kryptering på hello befintlig eller använder IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-596">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on hello existing or running IaaS VM.</span></span>

<span data-ttu-id="5eec6-597">hello i den följande tabellen listas Resource Manager mallparametrar för befintlig eller virtuella datorer som använder en Azure AD-klient-ID som körs:</span><span class="sxs-lookup"><span data-stu-id="5eec6-597">hello following table lists Resource Manager template parameters for existing or running VMs that use an Azure AD client ID:</span></span>

| <span data-ttu-id="5eec6-598">Parameter</span><span class="sxs-lookup"><span data-stu-id="5eec6-598">Parameter</span></span> | <span data-ttu-id="5eec6-599">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5eec6-599">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5eec6-600">AADClientID</span><span class="sxs-lookup"><span data-stu-id="5eec6-600">AADClientID</span></span> | <span data-ttu-id="5eec6-601">Klient-ID för hello Azure AD-program som har behörigheter toowrite hemligheter toohello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-601">Client ID of hello Azure AD application that has permissions toowrite secrets toohello key vault.</span></span> |
| <span data-ttu-id="5eec6-602">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="5eec6-602">AADClientSecret</span></span> | <span data-ttu-id="5eec6-603">Klienthemlighet för hello Azure AD-program som har behörigheter toowrite hemligheter tooyour nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-603">Client secret of hello Azure AD application that has permissions toowrite secrets tooyour key vault.</span></span> |
| <span data-ttu-id="5eec6-604">keyVaultName</span><span class="sxs-lookup"><span data-stu-id="5eec6-604">keyVaultName</span></span> | <span data-ttu-id="5eec6-605">Namnet på nyckeln hello valvet som hello BitLocker nyckel ska överföras till.</span><span class="sxs-lookup"><span data-stu-id="5eec6-605">Name of hello key vault that hello BitLocker key should be uploaded to.</span></span> <span data-ttu-id="5eec6-606">Du kan hämta den med hjälp av cmdlet hello `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span><span class="sxs-lookup"><span data-stu-id="5eec6-606">You can get it by using hello cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span></span> |
|  <span data-ttu-id="5eec6-607">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="5eec6-607">keyEncryptionKeyURL</span></span> | <span data-ttu-id="5eec6-608">URL för hello nyckelkryptering nyckel som används tooencrypt hello genereras BitLocker-nyckel.</span><span class="sxs-lookup"><span data-stu-id="5eec6-608">URL of hello key encryption key that's used tooencrypt hello generated BitLocker key.</span></span> <span data-ttu-id="5eec6-609">Den här parametern är valfri om du väljer **nokek** i hello UseExistingKek nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="5eec6-609">This parameter is optional if you select **nokek** in hello UseExistingKek drop-down list.</span></span> <span data-ttu-id="5eec6-610">Om du väljer **kek** i hello UseExistingKek nedrullningsbara listan, måste du ange hello _keyEncryptionKeyURL_ värde.</span><span class="sxs-lookup"><span data-stu-id="5eec6-610">If you select **kek** in hello UseExistingKek drop-down list, you must enter hello _keyEncryptionKeyURL_ value.</span></span> |
| <span data-ttu-id="5eec6-611">volumeType</span><span class="sxs-lookup"><span data-stu-id="5eec6-611">volumeType</span></span> | <span data-ttu-id="5eec6-612">Typ av volymen som hello krypteringsåtgärden utförs på.</span><span class="sxs-lookup"><span data-stu-id="5eec6-612">Type of volume that hello encryption operation is performed on.</span></span> <span data-ttu-id="5eec6-613">Giltiga värden som stöds är _OS_ eller _alla_ (för RHEL 7.2, CentOS 7.2 och Ubuntu 16.04) och _Data_ (för alla andra distributioner).</span><span class="sxs-lookup"><span data-stu-id="5eec6-613">Valid supported values are _OS_ or _All_ (for RHEL 7.2, CentOS 7.2, and Ubuntu 16.04), and _Data_ (for all other distributions).</span></span> |
| <span data-ttu-id="5eec6-614">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="5eec6-614">sequenceVersion</span></span> | <span data-ttu-id="5eec6-615">Sekvens version av hello BitLocker igen.</span><span class="sxs-lookup"><span data-stu-id="5eec6-615">Sequence version of hello BitLocker operation.</span></span> <span data-ttu-id="5eec6-616">Öka det här versionsnumret varje gång en diskkryptering åtgärden utförs på hello samma virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="5eec6-616">Increment this version number every time a disk-encryption operation is performed on hello same VM.</span></span> |
| <span data-ttu-id="5eec6-617">vmName</span><span class="sxs-lookup"><span data-stu-id="5eec6-617">vmName</span></span> | <span data-ttu-id="5eec6-618">Namnet på hello VM som hello krypteringsåtgärden är toobe utförs på.</span><span class="sxs-lookup"><span data-stu-id="5eec6-618">Name of hello VM that hello encryption operation is toobe performed on.</span></span> |
| <span data-ttu-id="5eec6-619">Lösenfrasen</span><span class="sxs-lookup"><span data-stu-id="5eec6-619">passPhrase</span></span> | <span data-ttu-id="5eec6-620">Ange ett starkt lösenord som hello datakrypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="5eec6-620">Type a strong passphrase as hello data encryption key.</span></span> |

> [!NOTE]
> <span data-ttu-id="5eec6-621">_KeyEncryptionKeyURL_ är en valfri parameter.</span><span class="sxs-lookup"><span data-stu-id="5eec6-621">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="5eec6-622">Du kan sätta egna KEK toofurther skydda hello datakrypteringsnyckeln (lösenfrasen hemlig) i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-622">You can bring your own KEK toofurther safeguard hello data encryption key (passphrase secret) in your key vault.</span></span>

#### <a name="cli-commands"></a><span data-ttu-id="5eec6-623">CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="5eec6-623">CLI commands</span></span>
<span data-ttu-id="5eec6-624">Du kan aktivera kryptering på den kryptera virtuella Hårddisken genom att installera och använda hello [CLI kommandot](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="5eec6-624">You can enable disk encryption on your encrypted VHD by installing and using hello [CLI command](../cli-install-nodejs.md).</span></span> <span data-ttu-id="5eec6-625">tooenable kryptering på befintlig eller IaaS Linux virtuella datorer som körs i Azure med hjälp av CLI-kommandona hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-625">tooenable encryption on existing or running IaaS Linux VMs in Azure by using CLI commands, do hello following:</span></span>

1. <span data-ttu-id="5eec6-626">Ange åtkomstprinciper i hello nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="5eec6-626">Set access policies in hello key vault:</span></span>

 * <span data-ttu-id="5eec6-627">Ange hello **EnabledForDiskEncryption** flaggan:</span><span class="sxs-lookup"><span data-stu-id="5eec6-627">Set hello **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * <span data-ttu-id="5eec6-628">Ange behörigheter tooAzure AD programmet toowrite hemligheter tooyour nyckelvalv:</span><span class="sxs-lookup"><span data-stu-id="5eec6-628">Set permissions tooAzure AD application toowrite secrets tooyour key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. <span data-ttu-id="5eec6-629">tooenable kryptering på en befintlig eller körs VM:</span><span class="sxs-lookup"><span data-stu-id="5eec6-629">tooenable encryption on an existing or running VM:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. <span data-ttu-id="5eec6-630">Hämta krypteringsstatus:</span><span class="sxs-lookup"><span data-stu-id="5eec6-630">Get encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. <span data-ttu-id="5eec6-631">tooenable kryptering på en ny virtuell dator från den kryptera virtuella Hårddisken, Använd hello följande parametrar med hello `azure vm create` kommando:</span><span class="sxs-lookup"><span data-stu-id="5eec6-631">tooenable encryption on a new VM from your encrypted VHD, use hello following parameters with hello `azure vm create` command:</span></span>
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-hello-encryption-status-of-an-encrypted-iaas-vm"></a><span data-ttu-id="5eec6-632">Hämta hello krypteringsstatus för en krypterad IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-632">Get hello encryption status of an encrypted IaaS VM</span></span>
<span data-ttu-id="5eec6-633">Du kan hämta hello krypteringsstatus med Azure Resource Manager [PowerShell-cmdlets](/powershell/azure/overview), eller CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="5eec6-633">You can get hello encryption status by using Azure Resource Manager, [PowerShell cmdlets](/powershell/azure/overview), or CLI commands.</span></span> <span data-ttu-id="5eec6-634">hello följande avsnitt förklarar hur toouse hello klassiska Azure-portalen och CLI-kommandon tooget hello krypteringsstatus.</span><span class="sxs-lookup"><span data-stu-id="5eec6-634">hello following sections explain how toouse hello Azure classic portal and CLI commands tooget hello encryption status.</span></span>

#### <a name="get-hello-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a><span data-ttu-id="5eec6-635">Hämta hello krypteringsstatus för en krypterad Windows-dator med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5eec6-635">Get hello encryption status of an encrypted Windows VM by using Azure Resource Manager</span></span>
<span data-ttu-id="5eec6-636">Du kan få hello krypteringsstatus för hello IaaS VM från Azure Resource Manager hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-636">You can get hello encryption status of hello IaaS VM from Azure Resource Manager by doing hello following:</span></span>

1. <span data-ttu-id="5eec6-637">Logga in toohello [klassiska Azure-portalen](https://portal.azure.com/), och klicka sedan på **virtuella datorer** i hello vänster toosee en sammanfattning av hello virtuella datorer i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5eec6-637">Sign in toohello [Azure classic portal](https://portal.azure.com/), and then click **Virtual machines** in hello left pane toosee a summary view of hello virtual machines in your subscription.</span></span> <span data-ttu-id="5eec6-638">Du kan filtrera hello virtuella datorer vyn genom att välja hello prenumerationsnamn i hello **prenumeration** listrutan.</span><span class="sxs-lookup"><span data-stu-id="5eec6-638">You can filter hello virtual machines view by selecting hello subscription name in hello **Subscription** drop-down list.</span></span>

2. <span data-ttu-id="5eec6-639">Hello överst i hello **virtuella datorer** klickar du på **kolumner**.</span><span class="sxs-lookup"><span data-stu-id="5eec6-639">At hello top of hello **Virtual machines** page, click **Columns**.</span></span>

3. <span data-ttu-id="5eec6-640">På hello **Välj kolumnen** bladet väljer **diskkryptering**, och klicka sedan på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="5eec6-640">On hello **Choose column** blade, select **Disk Encryption**, and then click **Update**.</span></span> <span data-ttu-id="5eec6-641">Du bör se hello diskkryptering kolumn som visar hello krypteringsstatus _aktiverad_ eller _inte aktiverat_ för varje VM som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="5eec6-641">You should see hello disk-encryption column showing hello encryption state _Enabled_ or _Not Enabled_ for each VM, as shown in hello following figure:</span></span>

 ![Microsoft Antimalware i Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-hello-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-hello-disk-encryption-powershell-cmdlet"></a><span data-ttu-id="5eec6-643">Hämta hello krypteringsstatus för en krypterad (Windows-/ Linux) IaaS VM med hjälp av PowerShell-cmdlet för hello-diskkryptering</span><span class="sxs-lookup"><span data-stu-id="5eec6-643">Get hello encryption status of an encrypted (Windows/Linux) IaaS VM by using hello disk-encryption PowerShell cmdlet</span></span>
<span data-ttu-id="5eec6-644">Du kan hämta hello krypteringsstatus för hello IaaS VM från hello-diskkryptering PowerShell-cmdleten `Get-AzureRmVMDiskEncryptionStatus`.</span><span class="sxs-lookup"><span data-stu-id="5eec6-644">You can get hello encryption status of hello IaaS VM from hello disk-encryption PowerShell cmdlet `Get-AzureRmVMDiskEncryptionStatus`.</span></span> <span data-ttu-id="5eec6-645">tooget hello krypteringsinställningar för den virtuella datorn anger hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-645">tooget hello encryption settings for your VM, enter hello following:</span></span>

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

<span data-ttu-id="5eec6-646">Du kan inspektera hello utdata från _Get-AzureRmVMDiskEncryptionStatus_ nyckeln URL: er för kryptering.</span><span class="sxs-lookup"><span data-stu-id="5eec6-646">You can inspect hello output of _Get-AzureRmVMDiskEncryptionStatus_ for encryption key URLs.</span></span>

    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey

    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

<span data-ttu-id="5eec6-647">Hej OSVolumeEncrypted och DataVolumesEncrypted värden anges too_Encrypted_ som visar att båda volymerna krypteras med Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="5eec6-647">hello OSVolumeEncrypted and DataVolumesEncrypted settings values are set too_Encrypted_, which shows that both volumes are encrypted using Azure Disk Encryption.</span></span> <span data-ttu-id="5eec6-648">Information om hur du aktiverar kryptering med Azure Disk Encryption med PowerShell-cmdlets finns hello blogginlägg [utforska Azure Disk Encryption med Azure PowerShell - del 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) och [utforska Azure Disk Encryption med Azure PowerShell - del 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span><span class="sxs-lookup"><span data-stu-id="5eec6-648">For information about enabling encryption with Azure Disk Encryption by using PowerShell cmdlets, see hello blog posts [Explore Azure Disk Encryption with Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) and [Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-649">På Linux virtuella datorer, det tar tre toofour minuter hello `Get-AzureRmVMDiskEncryptionStatus` cmdlet tooreport hello krypteringsstatus.</span><span class="sxs-lookup"><span data-stu-id="5eec6-649">On Linux VMs, it takes three toofour minutes for hello `Get-AzureRmVMDiskEncryptionStatus` cmdlet tooreport hello encryption status.</span></span>

#### <a name="get-hello-encryption-status-of-hello-iaas-vm-from-hello-disk-encryption-cli-command"></a><span data-ttu-id="5eec6-650">Hämta hello krypteringsstatus för hello IaaS VM från hello-diskkryptering CLI kommando</span><span class="sxs-lookup"><span data-stu-id="5eec6-650">Get hello encryption status of hello IaaS VM from hello disk-encryption CLI command</span></span>
<span data-ttu-id="5eec6-651">Du kan hämta hello krypteringsstatus för hello IaaS VM med kommandot hello-diskkryptering CLI `azure vm show-disk-encryption-status`.</span><span class="sxs-lookup"><span data-stu-id="5eec6-651">You can get hello encryption status of hello IaaS VM by using hello disk-encryption CLI command `azure vm show-disk-encryption-status`.</span></span> <span data-ttu-id="5eec6-652">tooget hello krypteringsinställningar för den virtuella datorn, ange din Azure CLI-session:</span><span class="sxs-lookup"><span data-stu-id="5eec6-652">tooget hello encryption settings for your VM, enter your Azure CLI session:</span></span>

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a><span data-ttu-id="5eec6-653">Inaktivera kryptering på använder Windows IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-653">Disable encryption on running Windows IaaS VM</span></span>
<span data-ttu-id="5eec6-654">Du kan inaktivera kryptering på en aktiv Windows eller Linux IaaS-VM via hello Azure Disk Encryption Resource Manager-mall eller PowerShell-cmdlets och ange hello dekryptering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5eec6-654">You can disable encryption on a running Windows or Linux IaaS VM via hello Azure Disk Encryption Resource Manager template or PowerShell cmdlets and specify hello decryption configuration.</span></span>

##### <a name="windows-vm"></a><span data-ttu-id="5eec6-655">Windows VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-655">Windows VM</span></span>
<span data-ttu-id="5eec6-656">hello inaktivera kryptering steget inaktiverar kryptering av hello OS, hello datavolym eller båda på hello som använder Windows IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-656">hello disable-encryption step disables encryption of hello OS, hello data volume, or both on hello running Windows IaaS VM.</span></span> <span data-ttu-id="5eec6-657">Du kan inte inaktivera hello systemvolymen och lämna hello datavolym krypteras.</span><span class="sxs-lookup"><span data-stu-id="5eec6-657">You cannot disable hello OS volume and leave hello data volume encrypted.</span></span> <span data-ttu-id="5eec6-658">När hello inaktivera kryptering steg utförs hello Azure klassiska distributionsmodellen uppdaterar hello VM modell och hello Windows IaaS-VM är markerad dekrypterade.</span><span class="sxs-lookup"><span data-stu-id="5eec6-658">When hello disable-encryption step is performed, hello Azure classic deployment model updates hello VM service model, and hello Windows IaaS VM is marked decrypted.</span></span> <span data-ttu-id="5eec6-659">hello innehållet i hello VM krypteras inte längre i vila.</span><span class="sxs-lookup"><span data-stu-id="5eec6-659">hello contents of hello VM are no longer encrypted at rest.</span></span> <span data-ttu-id="5eec6-660">hello dekryptering tar inte bort din key vault och hello kryptering nyckelmaterial (krypteringsnycklar BitLocker för Windows och lösenfrasen för Linux).</span><span class="sxs-lookup"><span data-stu-id="5eec6-660">hello decryption does not delete your key vault and hello encryption key material (BitLocker encryption keys for Windows and Passphrase for Linux).</span></span>

##### <a name="linux-vm"></a><span data-ttu-id="5eec6-661">Linux VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-661">Linux VM</span></span>
<span data-ttu-id="5eec6-662">hello inaktivera kryptering steget inaktiverar kryptering av hello datavolym på hello som kör Linux IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-662">hello disable-encryption step disables encryption of hello data volume on hello running Linux IaaS VM.</span></span> <span data-ttu-id="5eec6-663">Det här steget fungerar bara om hello OS-disken inte krypteras.</span><span class="sxs-lookup"><span data-stu-id="5eec6-663">This step only works if hello OS disk is not encrypted.</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-664">Om du inaktiverar kryptering på hello OS-disken tillåts inte för virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="5eec6-664">Disabling encryption on hello OS disk is not allowed on Linux VMs.</span></span>

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a><span data-ttu-id="5eec6-665">Inaktivera kryptering på en befintlig eller körs IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-665">Disable encryption on an existing or running IaaS VM</span></span>
<span data-ttu-id="5eec6-666">Du kan inaktivera diskkryptering på Windows IaaS virtuella datorer som körs med hjälp av hello [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="5eec6-666">You can disable disk encryption on running Windows IaaS VMs by using hello [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).</span></span>

1. <span data-ttu-id="5eec6-667">Klicka på hello Azure Snabbkurs mallen **distribuera tooAzure**, ange hello dekryptering konfiguration på hello **parametrar** bladet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5eec6-667">On hello Azure quick-start template, click **Deploy tooAzure**, enter hello decryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="5eec6-668">Välj hello prenumeration, resursgrupp, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** tooenable kryptering på en ny IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-668">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on a new IaaS VM.</span></span>

<span data-ttu-id="5eec6-669">För Linux virtuella datorer kan du inaktivera kryptering med hjälp av hello [inaktivera kryptering på en körs Linux VM](https://aka.ms/decrypt-linuxvm) mall.</span><span class="sxs-lookup"><span data-stu-id="5eec6-669">For Linux VMs, you can disable encryption by using hello [Disable encryption on a running Linux VM](https://aka.ms/decrypt-linuxvm) template.</span></span>

<span data-ttu-id="5eec6-670">hello visas följande tabell mallparametrar för Resource Manager för inaktivering av kryptering på en körs IaaS-VM:</span><span class="sxs-lookup"><span data-stu-id="5eec6-670">hello following table lists Resource Manager template parameters for disabling encryption on a running IaaS VM:</span></span>

| <span data-ttu-id="5eec6-671">Parameter</span><span class="sxs-lookup"><span data-stu-id="5eec6-671">Parameter</span></span> | <span data-ttu-id="5eec6-672">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5eec6-672">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5eec6-673">vmName</span><span class="sxs-lookup"><span data-stu-id="5eec6-673">vmName</span></span> | <span data-ttu-id="5eec6-674">Namnet på hello VM som hello krypteringsåtgärden är toobe utförs på.</span><span class="sxs-lookup"><span data-stu-id="5eec6-674">Name of hello VM that hello encryption operation is toobe performed on.</span></span>
| <span data-ttu-id="5eec6-675">volumeType</span><span class="sxs-lookup"><span data-stu-id="5eec6-675">volumeType</span></span> | <span data-ttu-id="5eec6-676">Typ av volymen som en dekrypteringsåtgärd utförs på.</span><span class="sxs-lookup"><span data-stu-id="5eec6-676">Type of volume that a decryption operation is performed on.</span></span> <span data-ttu-id="5eec6-677">Giltiga värden är _OS_, _Data_, och _alla_.</span><span class="sxs-lookup"><span data-stu-id="5eec6-677">Valid values are _OS_, _Data_, and _All_.</span></span> <span data-ttu-id="5eec6-678">Du kan inte inaktivera kryptering på kör Windows IaaS-VM OS/startvolymen utan att inaktivera kryptering på hello _Data_ volym.</span><span class="sxs-lookup"><span data-stu-id="5eec6-678">You cannot disable encryption on running Windows IaaS VM OS/boot volume without disabling encryption on hello _Data_ volume.</span></span> <span data-ttu-id="5eec6-679">Observera också att om du inaktiverar kryptering på hello OS-disken inte tillåts för virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="5eec6-679">Also note that disabling encryption on hello OS disk is not allowed on Linux VMs.</span></span> |
| <span data-ttu-id="5eec6-680">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="5eec6-680">sequenceVersion</span></span> | <span data-ttu-id="5eec6-681">Sekvens version av hello BitLocker igen.</span><span class="sxs-lookup"><span data-stu-id="5eec6-681">Sequence version of hello BitLocker operation.</span></span> <span data-ttu-id="5eec6-682">Öka det här versionsnumret varje gång en disk dekrypteringsåtgärden utförs på hello samma virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="5eec6-682">Increment this version number every time a disk decryption operation is performed on hello same VM.</span></span> |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a><span data-ttu-id="5eec6-683">Inaktivera kryptering på en befintlig eller körs IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="5eec6-683">Disable encryption on an existing or running IaaS VM</span></span>
<span data-ttu-id="5eec6-684">toodisable kryptering på en befintlig eller körs IaaS-VM med hjälp av PowerShell-cmdleten hello finns [ `Disable-AzureRmVMDiskEncryption` ](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption).</span><span class="sxs-lookup"><span data-stu-id="5eec6-684">toodisable encryption on an existing or running IaaS VM by using hello PowerShell cmdlet, see [`Disable-AzureRmVMDiskEncryption`](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption).</span></span> <span data-ttu-id="5eec6-685">Denna cmdlet har stöd för både Windows- och Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5eec6-685">This cmdlet supports both Windows and Linux VMs.</span></span> <span data-ttu-id="5eec6-686">toodisable kryptering, ett tillägg installeras på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5eec6-686">toodisable encryption, it installs an extension on hello virtual machine.</span></span> <span data-ttu-id="5eec6-687">Om hello _namn_ parameter har angetts något tillägg med hello standardnamnet _AzureDiskEncryption för virtuella Windows-datorer_ skapas.</span><span class="sxs-lookup"><span data-stu-id="5eec6-687">If hello _Name_ parameter is not specified, an extension with hello default name _AzureDiskEncryption for Windows VMs_ is created.</span></span>

<span data-ttu-id="5eec6-688">Hej AzureDiskEncryptionForLinux tillägget används på Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5eec6-688">On Linux VMs, hello AzureDiskEncryptionForLinux extension is used.</span></span>

> [!NOTE]
> <span data-ttu-id="5eec6-689">Denna cmdlet startas hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5eec6-689">This cmdlet reboots hello virtual machine.</span></span>

### <a name="enable-encryption-on-pre-encrypted-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="5eec6-690">Aktivera kryptering på förkrypterade IaaS-VM med Azure-hanterade disken</span><span class="sxs-lookup"><span data-stu-id="5eec6-690">Enable encryption on pre-encrypted IaaS VM with Azure Managed Disk</span></span>
<span data-ttu-id="5eec6-691">Använd hello Azure hanteras Disk ARM-mall toocreate en krypterad virtuell dator från en förkrypterade virtuell Hårddisk med hello ARM-mall finns i</span><span class="sxs-lookup"><span data-stu-id="5eec6-691">Use hello Azure Managed Disk ARM template toocreate a encrypted VM from a pre-encrypted VHD using hello ARM template located at</span></span>   
<span data-ttu-id="5eec6-692">[Skapa en ny krypterade hanterade disk från en förkrypterade VHD/storage-blob] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)</span><span class="sxs-lookup"><span data-stu-id="5eec6-692">[Create a new encrypted managed disk from a pre-encrypted VHD/storage blob] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)</span></span>

### <a name="enable-encryption-on-a-new-linux-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="5eec6-693">Aktivera kryptering på en ny Linux IaaS virtuell dator med hanterad Azure-disken</span><span class="sxs-lookup"><span data-stu-id="5eec6-693">Enable encryption on a new Linux IaaS VM with Azure Managed Disk</span></span>
<span data-ttu-id="5eec6-694">Använd hello Azure hanteras Disk ARM-mall toocreate en ny krypterade Linux IaaS-VM med hello ARM-mall finns i</span><span class="sxs-lookup"><span data-stu-id="5eec6-694">Use hello Azure Managed Disk ARM template toocreate a new encrypted Linux IaaS VM using hello ARM template located at</span></span>   
<span data-ttu-id="5eec6-695">[Distributionen av RHEL 7.2 med fullständig diskkryptering] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)</span><span class="sxs-lookup"><span data-stu-id="5eec6-695">[Deployment of RHEL 7.2 with full disk encryption] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)</span></span>

### <a name="enable-encryption-on-a-new-windows-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="5eec6-696">Aktivera kryptering på en ny Windows IaaS-VM med Azure-hanterade disken</span><span class="sxs-lookup"><span data-stu-id="5eec6-696">Enable encryption on a new Windows IaaS VM with Azure Managed Disk</span></span>
 <span data-ttu-id="5eec6-697">Använd hello Azure hanteras Disk ARM-mall toocreate en ny krypterade Linux IaaS-VM med hello ARM-mall finns i</span><span class="sxs-lookup"><span data-stu-id="5eec6-697">Use hello Azure Managed Disk ARM template toocreate a new encrypted Linux IaaS VM using hello ARM template located at</span></span>   
 <span data-ttu-id="5eec6-698">[Skapa en ny krypterade Windows IaaS hanteras Disk virtuell dator från galleriet avbildning] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)</span><span class="sxs-lookup"><span data-stu-id="5eec6-698">[Create a new encrypted Windows IaaS Managed Disk VM from gallery image] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)</span></span>

  > [!NOTE]
  ><span data-ttu-id="5eec6-699">Det är obligatoriskt toosnapshot och/eller säkerhetskopiering hanterade diskar baserade VM-instans utanför och tidigare tooenabling Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="5eec6-699">It is mandatory toosnapshot and/or backup a managed disk based VM instance outside of and prior tooenabling Azure Disk Encryption.</span></span>  <span data-ttu-id="5eec6-700">En ögonblicksbild av hello hanterade diskar kan hämtas från hello-portalen eller Azure Backup kan användas.</span><span class="sxs-lookup"><span data-stu-id="5eec6-700">A snapshot of hello managed disk can be taken from hello portal, or Azure Backup can be used.</span></span>  <span data-ttu-id="5eec6-701">Säkerhetskopieringar kan du kontrollera att ett återställningsalternativ är möjligt i hello fall av ett oväntat fel under krypteringen.</span><span class="sxs-lookup"><span data-stu-id="5eec6-701">Backups ensure that a recovery option is possible in hello case of any unexpected failure during encryption.</span></span>  <span data-ttu-id="5eec6-702">När en säkerhetskopia görs vara hello Set-AzureRmVMDiskEncryptionExtension cmdlet används tooencrypt hanterade diskar genom att ange hello skipVmBackup - parametern.</span><span class="sxs-lookup"><span data-stu-id="5eec6-702">Once a backup is made, hello Set-AzureRmVMDiskEncryptionExtension cmdlet can be used tooencrypt managed disks by specifying hello -skipVmBackup parameter.</span></span>  <span data-ttu-id="5eec6-703">Det här kommandot misslyckas mot hanterade diskbaserat VM tills en säkerhetskopia som har gjorts och den här parametern har angetts.</span><span class="sxs-lookup"><span data-stu-id="5eec6-703">This command will fail against managed disk based VM's until a backup has been made and this parameter has been specified.</span></span>    
 
### <a name="update-encryption-settings-of-an-existing-encrypted-non-premium-vm"></a><span data-ttu-id="5eec6-704">Uppdatera krypteringsinställningarna för en befintlig krypterade icke-premium virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5eec6-704">Update encryption settings of an existing encrypted non-premium VM</span></span>
  <span data-ttu-id="5eec6-705">Använd hello befintliga Azure-disken kryptering stöds gränssnitt för att köra VM [PS-cmdlets, CLI eller ARM-mallar] tooupdate hello krypteringsinställningar som AAD klient ID/hemlig nyckel krypteringsnyckeln [KEK] BitLocker-krypteringsnyckeln för Windows-VM eller lösenfrasen för Linux VM etc. hello update krypteringsinställning stöds bara för virtuella datorer som backas upp av icke-premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="5eec6-705">Use hello existing Azure disk encryption supported interfaces for running VM [PS cmdlets, CLI or ARM templates] tooupdate hello encryption settings like AAD client ID/secret, Key encryption key [KEK], BitLocker encryption key for Windows VM or Passphrase for Linux VM etc. hello update encryption setting is supported only for VMs backed by non-premium storage.</span></span> <span data-ttu-id="5eec6-706">Det är NNOT stöd för virtuella datorer som backas upp av premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="5eec6-706">It is NNOT supported for VMs backed by premium storage.</span></span>

## <a name="appendix"></a><span data-ttu-id="5eec6-707">Bilaga</span><span class="sxs-lookup"><span data-stu-id="5eec6-707">Appendix</span></span>
### <a name="connect-tooyour-subscription"></a><span data-ttu-id="5eec6-708">Ansluta tooyour prenumeration</span><span class="sxs-lookup"><span data-stu-id="5eec6-708">Connect tooyour subscription</span></span>
<span data-ttu-id="5eec6-709">Innan du fortsätter kan du granska hello *krav* i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5eec6-709">Before you proceed, review hello *Prerequisites* section in this article.</span></span> <span data-ttu-id="5eec6-710">När du har kontrollerat att alla krav har uppfyllts, ansluter du tooyour prenumeration hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-710">After you ensure that all prerequisites have been met, connect tooyour subscription by doing hello following:</span></span>

1. <span data-ttu-id="5eec6-711">Starta en Azure PowerShell-session och logga in tooyour Azure-konto med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5eec6-711">Start an Azure PowerShell session, and sign in tooyour Azure account with hello following command:</span></span>

    `Login-AzureRmAccount`

2. <span data-ttu-id="5eec6-712">Om du har flera prenumerationer och vill toospecify en toouse, skriver du hello följande toosee hello prenumerationer för ditt konto:</span><span class="sxs-lookup"><span data-stu-id="5eec6-712">If you have multiple subscriptions and want toospecify one toouse, type hello following toosee hello subscriptions for your account:</span></span>

    `Get-AzureRmSubscription`

3. <span data-ttu-id="5eec6-713">toospecify hello prenumeration som du vill toouse, typ:</span><span class="sxs-lookup"><span data-stu-id="5eec6-713">toospecify hello subscription you want toouse, type:</span></span>

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. <span data-ttu-id="5eec6-714">tooverify att hello prenumeration konfigurerats är korrekt, typ:</span><span class="sxs-lookup"><span data-stu-id="5eec6-714">tooverify that hello subscription configured is correct, type:</span></span>

    `Get-AzureRmSubscription`

5. <span data-ttu-id="5eec6-715">tooconfirm hello Azure Disk Encryption är installerade, typ:</span><span class="sxs-lookup"><span data-stu-id="5eec6-715">tooconfirm hello Azure Disk Encryption cmdlets are installed, type:</span></span>

    `Get-command *diskencryption*`

6. <span data-ttu-id="5eec6-716">följande utdata hello bekräftar hello Azure Disk Encryption PowerShell-installationen:</span><span class="sxs-lookup"><span data-stu-id="5eec6-716">hello following output confirms hello Azure Disk Encryption PowerShell installation:</span></span>

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a><span data-ttu-id="5eec6-717">Förbereda en förkrypterade Windows VHD</span><span class="sxs-lookup"><span data-stu-id="5eec6-717">Prepare a pre-encrypted Windows VHD</span></span>
<span data-ttu-id="5eec6-718">hello avsnitten som följer är nödvändiga tooprepare en förkrypterade Windows VHD för distribution som en krypterad VHD i Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="5eec6-718">hello sections that follow are necessary tooprepare a pre-encrypted Windows VHD for deployment as an encrypted VHD in Azure IaaS.</span></span> <span data-ttu-id="5eec6-719">Använd hello information tooprepare och starta en ny Windows VM (VHD) på Azure Site Recovery eller Azure.</span><span class="sxs-lookup"><span data-stu-id="5eec6-719">Use hello information tooprepare and boot a fresh Windows VM (VHD) on Azure Site Recovery or Azure.</span></span>

#### <a name="update-group-policy-tooallow-non-tpm-for-os-protection"></a><span data-ttu-id="5eec6-720">Uppdatera grupp princip tooallow utan TPM för OS-skydd</span><span class="sxs-lookup"><span data-stu-id="5eec6-720">Update group policy tooallow non-TPM for OS protection</span></span>
<span data-ttu-id="5eec6-721">Konfigurera hello BitLocker grupprincipinställningen **BitLocker-diskkryptering**, som du hittar **lokal datorprincip** > **Datorkonfiguration**  >  **Administrationsmallar** > **Windows-komponenter**.</span><span class="sxs-lookup"><span data-stu-id="5eec6-721">Configure hello BitLocker Group Policy setting **BitLocker Drive Encryption**, which you'll find under **Local Computer Policy** > **Computer Configuration** > **Administrative Templates** > **Windows Components**.</span></span> <span data-ttu-id="5eec6-722">Ändra inställningen för**operativsystemsenheter** > **kräver ytterligare autentisering vid start** > **Tillåt BitLocker utan en kompatibel TPM**, enligt följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="5eec6-722">Change this setting too**Operating System Drives** > **Require additional authentication at startup** > **Allow BitLocker without a compatible TPM**, as shown in hello following figure:</span></span>

![Microsoft Antimalware i Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a><span data-ttu-id="5eec6-724">Installera komponenter för BitLocker-funktion</span><span class="sxs-lookup"><span data-stu-id="5eec6-724">Install BitLocker feature components</span></span>
<span data-ttu-id="5eec6-725">För Windows Server 2012 och senare, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5eec6-725">For Windows Server 2012 and later, use hello following command:</span></span>

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

<span data-ttu-id="5eec6-726">För Windows Server 2008 R2, använder du hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5eec6-726">For Windows Server 2008 R2, use hello following command:</span></span>

    ServerManagerCmd -install BitLockers

#### <a name="prepare-hello-os-volume-for-bitlocker-by-using-bdehdcfg"></a><span data-ttu-id="5eec6-727">Förbereda hello systemvolymen för BitLocker med hjälp av`bdehdcfg`</span><span class="sxs-lookup"><span data-stu-id="5eec6-727">Prepare hello OS volume for BitLocker by using `bdehdcfg`</span></span>
<span data-ttu-id="5eec6-728">toocompress hello OS-partitionen och Förbered hello datorn för BitLocker, köra hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5eec6-728">toocompress hello OS partition and prepare hello machine for BitLocker, execute hello following command:</span></span>

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-hello-os-volume-by-using-bitlocker"></a><span data-ttu-id="5eec6-729">Skydda hello systemvolymen med BitLocker</span><span class="sxs-lookup"><span data-stu-id="5eec6-729">Protect hello OS volume by using BitLocker</span></span>
<span data-ttu-id="5eec6-730">Använd hello [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) kommandot tooenable kryptering på hello startvolymen med hjälp av ett externt nyckelskydd.</span><span class="sxs-lookup"><span data-stu-id="5eec6-730">Use hello [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx) command tooenable encryption on hello boot volume using an external key protector.</span></span> <span data-ttu-id="5eec6-731">Också placera hello extern nyckel (.bek-fil) på hello extern enhet eller volym.</span><span class="sxs-lookup"><span data-stu-id="5eec6-731">Also place hello external key (.bek file) on hello external drive or volume.</span></span> <span data-ttu-id="5eec6-732">Kryptering är aktiverat på hello system/startvolymen efter hello nästa omstart.</span><span class="sxs-lookup"><span data-stu-id="5eec6-732">Encryption is enabled on hello system/boot volume after hello next reboot.</span></span>

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> <span data-ttu-id="5eec6-733">Förbered hello VM med en separat/Dataresurs VHD för att hämta hello externa nyckeln med hjälp av BitLocker.</span><span class="sxs-lookup"><span data-stu-id="5eec6-733">Prepare hello VM with a separate data/resource VHD for getting hello external key by using BitLocker.</span></span>

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a><span data-ttu-id="5eec6-734">Kryptera en OS-enheten på en Linux-VM som körs</span><span class="sxs-lookup"><span data-stu-id="5eec6-734">Encrypting an OS drive on a running Linux VM</span></span>
<span data-ttu-id="5eec6-735">Kryptering av en OS-enhet på en Linux-VM som körs stöds på hello följande distributioner:</span><span class="sxs-lookup"><span data-stu-id="5eec6-735">Encryption of an OS drive on a running Linux VM is supported on hello following distributions:</span></span>

* <span data-ttu-id="5eec6-736">RHEL 7.2</span><span class="sxs-lookup"><span data-stu-id="5eec6-736">RHEL 7.2</span></span>
* <span data-ttu-id="5eec6-737">CentOS 7.2</span><span class="sxs-lookup"><span data-stu-id="5eec6-737">CentOS 7.2</span></span>
* <span data-ttu-id="5eec6-738">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="5eec6-738">Ubuntu 16.04</span></span>

##### <a name="prerequisites-for-os-disk-encryption"></a><span data-ttu-id="5eec6-739">Krav för OS-diskkryptering</span><span class="sxs-lookup"><span data-stu-id="5eec6-739">Prerequisites for OS disk encryption</span></span>

* <span data-ttu-id="5eec6-740">hello VM måste skapas från hello Marketplace-avbildning i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5eec6-740">hello VM must be created from hello Marketplace image in Azure Resource Manager.</span></span>
* <span data-ttu-id="5eec6-741">Azure virtuell dator med minst 4 GB RAM-minne (rekommenderas storleken är 7 GB).</span><span class="sxs-lookup"><span data-stu-id="5eec6-741">Azure VM with at least 4 GB of RAM (recommended size is 7 GB).</span></span>
* <span data-ttu-id="5eec6-742">(För RHEL och CentOS) Inaktivera SELinux.</span><span class="sxs-lookup"><span data-stu-id="5eec6-742">(For RHEL and CentOS) Disable SELinux.</span></span> <span data-ttu-id="5eec6-743">toodisable SELinux, se ”4.4.2.</span><span class="sxs-lookup"><span data-stu-id="5eec6-743">toodisable SELinux, see "4.4.2.</span></span> <span data-ttu-id="5eec6-744">Inaktivera SELinux ”i hello [SELinux användar- och Administrator's Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) på hello VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-744">Disabling SELinux" in hello [SELinux User's and Administrator's Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) on hello VM.</span></span>
* <span data-ttu-id="5eec6-745">När du inaktiverar SELinux startas hello VM minst en gång.</span><span class="sxs-lookup"><span data-stu-id="5eec6-745">After you disable SELinux, reboot hello VM at least once.</span></span>

##### <a name="steps"></a><span data-ttu-id="5eec6-746">Steg</span><span class="sxs-lookup"><span data-stu-id="5eec6-746">Steps</span></span>
1. <span data-ttu-id="5eec6-747">Skapa en virtuell dator med någon av hello distributioner har angett tidigare.</span><span class="sxs-lookup"><span data-stu-id="5eec6-747">Create a VM by using one of hello distributions specified previously.</span></span>

 <span data-ttu-id="5eec6-748">OS-diskkryptering stöds för CentOS 7.2 via en särskild avbildning.</span><span class="sxs-lookup"><span data-stu-id="5eec6-748">For CentOS 7.2, OS disk encryption is supported via a special image.</span></span> <span data-ttu-id="5eec6-749">toouse det bild, ange ”7.2n” som hello SKU när du skapar hello VM:</span><span class="sxs-lookup"><span data-stu-id="5eec6-749">toouse this image, specify "7.2n" as hello SKU when you create hello VM:</span></span>
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. <span data-ttu-id="5eec6-750">Konfigurera hello VM bl.a tooyour behov.</span><span class="sxs-lookup"><span data-stu-id="5eec6-750">Configure hello VM according tooyour needs.</span></span> <span data-ttu-id="5eec6-751">Om du ska tooencrypt alla hello (OS + data)-enheter behöver hello dataenheter toobe angivna och monteras från /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="5eec6-751">If you are going tooencrypt all hello (OS + data) drives, hello data drives need toobe specified and mountable from /etc/fstab.</span></span>

 > [!NOTE]
 > <span data-ttu-id="5eec6-752">Använd UUID =... toospecify dataenheter i/etc/fstab istället för att ange hello blockera enhetens namn (till exempel /dev/sdb1).</span><span class="sxs-lookup"><span data-stu-id="5eec6-752">Use UUID=... toospecify data drives in /etc/fstab instead of specifying hello block device name (for example, /dev/sdb1).</span></span> <span data-ttu-id="5eec6-753">Hej ordningen på enheter ändringar på hello VM under krypteringen.</span><span class="sxs-lookup"><span data-stu-id="5eec6-753">During encryption, hello order of drives changes on hello VM.</span></span> <span data-ttu-id="5eec6-754">Om den virtuella datorn är beroende av en viss ordning för blockera enheter, misslyckas toomount dem efter kryptering.</span><span class="sxs-lookup"><span data-stu-id="5eec6-754">If your VM relies on a specific order of block devices, it will fail toomount them after encryption.</span></span>

3. <span data-ttu-id="5eec6-755">Logga ut från hello SSH-sessioner.</span><span class="sxs-lookup"><span data-stu-id="5eec6-755">Sign out of hello SSH sessions.</span></span>

4. <span data-ttu-id="5eec6-756">tooencrypt hello OS, ange volumeType som **alla** eller **OS** när du [Aktivera kryptering](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).</span><span class="sxs-lookup"><span data-stu-id="5eec6-756">tooencrypt hello OS, specify volumeType as **All** or **OS** when you [enable encryption](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).</span></span>

 > [!NOTE]
 > <span data-ttu-id="5eec6-757">Alla användare kan processer som inte körs som `systemd` tjänster ska avslutas med en `SIGKILL`.</span><span class="sxs-lookup"><span data-stu-id="5eec6-757">All user-space processes that are not running as `systemd` services should be killed with a `SIGKILL`.</span></span> <span data-ttu-id="5eec6-758">Starta om hello VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-758">Reboot hello VM.</span></span> <span data-ttu-id="5eec6-759">När du aktiverar OS-diskkryptering på en aktiv virtuell dator planera VM driftstopp.</span><span class="sxs-lookup"><span data-stu-id="5eec6-759">When you enable OS disk encryption on a running VM, plan on VM downtime.</span></span>

5. <span data-ttu-id="5eec6-760">Regelbundet övervaka hello kryptering genom hello instruktionerna i hello [nästa avsnitt](#monitoring-os-encryption-progress).</span><span class="sxs-lookup"><span data-stu-id="5eec6-760">Periodically monitor hello progress of encryption by using hello instructions in hello [next section](#monitoring-os-encryption-progress).</span></span>

6. <span data-ttu-id="5eec6-761">När Get-AzureRmVmDiskEncryptionStatus visar ”VMRestartPending”, startar du om den virtuella datorn genom att logga in tooit eller genom att använda hello-portalen, PowerShell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="5eec6-761">After Get-AzureRmVmDiskEncryptionStatus shows "VMRestartPending," restart your VM either by signing in tooit or by using hello portal, PowerShell, or CLI.</span></span>
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot hello VM
    ```
<span data-ttu-id="5eec6-762">Innan du startar om rekommenderar vi att du sparar [starta diagnostik](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) av hello VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-762">Before you reboot, we recommend that you save [boot diagnostics](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) of hello VM.</span></span>

#### <a name="monitoring-os-encryption-progress"></a><span data-ttu-id="5eec6-763">Övervaka förloppet för OS-kryptering</span><span class="sxs-lookup"><span data-stu-id="5eec6-763">Monitoring OS encryption progress</span></span>
<span data-ttu-id="5eec6-764">Du kan övervaka förloppet för OS-kryptering på tre sätt:</span><span class="sxs-lookup"><span data-stu-id="5eec6-764">You can monitor OS encryption progress in three ways:</span></span>

* <span data-ttu-id="5eec6-765">Använd hello `Get-AzureRmVmDiskEncryptionStatus` cmdlet och inspektera hello ProgressMessage fält:</span><span class="sxs-lookup"><span data-stu-id="5eec6-765">Use hello `Get-AzureRmVmDiskEncryptionStatus` cmdlet and inspect hello ProgressMessage field:</span></span>
    ```
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 <span data-ttu-id="5eec6-766">När hello VM når ”OS-disken kryptering igång”, tar ungefär 40 too50 minuter på en Premium-lagring säkerhetskopieras VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-766">After hello VM reaches "OS disk encryption started," it takes about 40 too50 minutes on a Premium-storage backed VM.</span></span>

 <span data-ttu-id="5eec6-767">Eftersom [utfärda #388](https://github.com/Azure/WALinuxAgent/issues/388) i WALinuxAgent, `OsVolumeEncrypted` och `DataVolumesEncrypted` visas som `Unknown` i vissa distributioner.</span><span class="sxs-lookup"><span data-stu-id="5eec6-767">Because of [issue #388](https://github.com/Azure/WALinuxAgent/issues/388) in WALinuxAgent, `OsVolumeEncrypted` and `DataVolumesEncrypted` show up as `Unknown` in some distributions.</span></span> <span data-ttu-id="5eec6-768">Med WALinuxAgent version 2.1.5 och senare kan det här problemet löses automatiskt.</span><span class="sxs-lookup"><span data-stu-id="5eec6-768">With WALinuxAgent version 2.1.5 and later, this issue is fixed automatically.</span></span> <span data-ttu-id="5eec6-769">Om du ser `Unknown` hello utdata och du kan kontrollera disk krypteringsstatus med hjälp av hello Azure Resursläsaren.</span><span class="sxs-lookup"><span data-stu-id="5eec6-769">If you see `Unknown` in hello output, you can verify disk-encryption status by using hello Azure Resource Explorer.</span></span>

 <span data-ttu-id="5eec6-770">Gå för[resursutforskaren Azure](https://resources.azure.com/), och expandera sedan den här hierarkin i hello markeringen panelen vänster:</span><span class="sxs-lookup"><span data-stu-id="5eec6-770">Go too[Azure Resource Explorer](https://resources.azure.com/), and then expand this hierarchy in hello selection panel on left:</span></span>

 ~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

 <span data-ttu-id="5eec6-771">Rulla ned toosee hello krypteringsstatus enheter i hello InstanceView.</span><span class="sxs-lookup"><span data-stu-id="5eec6-771">In hello InstanceView, scroll down toosee hello encryption status of your drives.</span></span>

 ![VM-instansvyn](./media/azure-security-disk-encryption/vm-instanceview.png)

* <span data-ttu-id="5eec6-773">Titta på [starta diagnostik](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/).</span><span class="sxs-lookup"><span data-stu-id="5eec6-773">Look at [boot diagnostics](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/).</span></span> <span data-ttu-id="5eec6-774">Meddelanden från hello ADE tillägget prefixet `[AzureDiskEncryption]`.</span><span class="sxs-lookup"><span data-stu-id="5eec6-774">Messages from hello ADE extension should be prefixed with `[AzureDiskEncryption]`.</span></span>

* <span data-ttu-id="5eec6-775">Logga in toohello VM via SSH och hämta hello tillägget loggen:</span><span class="sxs-lookup"><span data-stu-id="5eec6-775">Sign in toohello VM via SSH, and get hello extension log from:</span></span>

    <span data-ttu-id="5eec6-776">/var/log/Azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux</span><span class="sxs-lookup"><span data-stu-id="5eec6-776">/var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux</span></span>

 <span data-ttu-id="5eec6-777">Vi rekommenderar att du inte loggar in toohello VM när OS-kryptering pågår.</span><span class="sxs-lookup"><span data-stu-id="5eec6-777">We recommend that you do not sign in toohello VM while OS encryption is in progress.</span></span> <span data-ttu-id="5eec6-778">Kopiera hello loggar endast när hello andra två metoder har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="5eec6-778">Copy hello logs only when hello other two methods have failed.</span></span>

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a><span data-ttu-id="5eec6-779">Förbereda en förkrypterade Linux VHD</span><span class="sxs-lookup"><span data-stu-id="5eec6-779">Prepare a pre-encrypted Linux VHD</span></span>
##### <a name="ubuntu-16"></a><span data-ttu-id="5eec6-780">Ubuntu 16</span><span class="sxs-lookup"><span data-stu-id="5eec6-780">Ubuntu 16</span></span>
<span data-ttu-id="5eec6-781">Konfigurera kryptering under installationen av hello distribution hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-781">Configure encryption during hello distribution installation by doing hello following:</span></span>

1. <span data-ttu-id="5eec6-782">Välj **konfigurera krypterade volymer** när du partitionera hello diskar.</span><span class="sxs-lookup"><span data-stu-id="5eec6-782">Select **Configure encrypted volumes** when you partition hello disks.</span></span>

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. <span data-ttu-id="5eec6-784">Skapa en separat startenheten som inte får vara krypterade.</span><span class="sxs-lookup"><span data-stu-id="5eec6-784">Create a separate boot drive, which must not be encrypted.</span></span> <span data-ttu-id="5eec6-785">Kryptera din rotenhet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-785">Encrypt your root drive.</span></span>

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. <span data-ttu-id="5eec6-787">Ange en lösenfras.</span><span class="sxs-lookup"><span data-stu-id="5eec6-787">Provide a passphrase.</span></span> <span data-ttu-id="5eec6-788">Detta är hello lösenfras som du överför toohello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-788">This is hello passphrase that you upload toohello key vault.</span></span>

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. <span data-ttu-id="5eec6-790">Slut partitionering.</span><span class="sxs-lookup"><span data-stu-id="5eec6-790">Finish partitioning.</span></span>

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. <span data-ttu-id="5eec6-792">När du startar hello VM och ange en lösenfras använda hello lösenfras som du angav i steg 3.</span><span class="sxs-lookup"><span data-stu-id="5eec6-792">When you boot hello VM and are asked for a passphrase, use hello passphrase you provided in step 3.</span></span>

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. <span data-ttu-id="5eec6-794">Förbereda hello VM för överföring till Azure med hjälp av [instruktionerna](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="5eec6-794">Prepare hello VM for uploading into Azure using [these instructions](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/).</span></span> <span data-ttu-id="5eec6-795">Kör inte hello sista steget (avställningsskript hello VM) ännu.</span><span class="sxs-lookup"><span data-stu-id="5eec6-795">Do not run hello last step (deprovisioning hello VM) yet.</span></span>

<span data-ttu-id="5eec6-796">Konfigurera kryptering toowork med Azure hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-796">Configure encryption toowork with Azure by doing hello following:</span></span>

1. <span data-ttu-id="5eec6-797">Skapa en fil under /usr/local/sbin/azure_crypt_key.sh, med hello innehåll i hello följande skript.</span><span class="sxs-lookup"><span data-stu-id="5eec6-797">Create a file under /usr/local/sbin/azure_crypt_key.sh, with hello content in hello following script.</span></span> <span data-ttu-id="5eec6-798">Betala uppmärksamhet toohello KeyFileName, eftersom det är hello lösenfras filnamn som används av Azure.</span><span class="sxs-lookup"><span data-stu-id="5eec6-798">Pay attention toohello KeyFileName, because it is hello passphrase file name used by Azure.</span></span>

    ```
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do

        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED toofind suitable passphrase file ..." >&2
        echo -n "Try tooenter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
```

2. <span data-ttu-id="5eec6-799">Ändra hello crypt config i */etc/crypttab*.</span><span class="sxs-lookup"><span data-stu-id="5eec6-799">Change hello crypt config in */etc/crypttab*.</span></span> <span data-ttu-id="5eec6-800">Det bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="5eec6-800">It should look like this:</span></span>
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. <span data-ttu-id="5eec6-801">Om du redigerar *azure_crypt_key.sh* i Windows och du har kopierat tooLinux, kör `dos2unix /usr/local/sbin/azure_crypt_key.sh`.</span><span class="sxs-lookup"><span data-stu-id="5eec6-801">If you are editing *azure_crypt_key.sh* in Windows and you copied it tooLinux, run `dos2unix /usr/local/sbin/azure_crypt_key.sh`.</span></span>

4. <span data-ttu-id="5eec6-802">Lägg till körrättigheter toohello skript:</span><span class="sxs-lookup"><span data-stu-id="5eec6-802">Add executable permissions toohello script:</span></span>
 ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
 ```
5. <span data-ttu-id="5eec6-803">Redigera */etc/initramfs-tools/modules* genom att lägga till rader:</span><span class="sxs-lookup"><span data-stu-id="5eec6-803">Edit */etc/initramfs-tools/modules* by appending lines:</span></span>
 ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
```
6. <span data-ttu-id="5eec6-804">Kör `update-initramfs -u -k all` tooupdate hello initramfs toomake hello `keyscript` börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="5eec6-804">Run `update-initramfs -u -k all` tooupdate hello initramfs toomake hello `keyscript` take effect.</span></span>

7. <span data-ttu-id="5eec6-805">Nu kan du ta bort etableringen hello VM.</span><span class="sxs-lookup"><span data-stu-id="5eec6-805">Now you can deprovision hello VM.</span></span>

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. <span data-ttu-id="5eec6-807">Fortsätta toohello nästa steg och [överför den virtuella Hårddisken](#upload-encrypted-vhd-to-an-azure-storage-account) till Azure.</span><span class="sxs-lookup"><span data-stu-id="5eec6-807">Continue toohello next step and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

##### <a name="opensuse-132"></a><span data-ttu-id="5eec6-808">openSUSE 13.2</span><span class="sxs-lookup"><span data-stu-id="5eec6-808">openSUSE 13.2</span></span>
<span data-ttu-id="5eec6-809">tooconfigure kryptering under installationen av hello distribution hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-809">tooconfigure encryption during hello distribution installation, do hello following:</span></span>
1. <span data-ttu-id="5eec6-810">När du partitionera hello diskar väljer **kryptera volymen grupp**, och sedan ange ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="5eec6-810">When you partition hello disks, select **Encrypt Volume Group**, and then enter a password.</span></span> <span data-ttu-id="5eec6-811">Detta är hello lösenord som du kommer att överföra tooyour nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-811">This is hello password that you will upload tooyour key vault.</span></span>

 ![Konfigurera openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. <span data-ttu-id="5eec6-813">Starta hello VM som använder ditt lösenord.</span><span class="sxs-lookup"><span data-stu-id="5eec6-813">Boot hello VM using your password.</span></span>

 ![Konfigurera openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. <span data-ttu-id="5eec6-815">Förbered hello VM för uppladdning av tooAzure genom att följa instruktionerna hello i [förbereda en virtuell dator SLES eller openSUSE för Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131).</span><span class="sxs-lookup"><span data-stu-id="5eec6-815">Prepare hello VM for uploading tooAzure by following hello instructions in [Prepare a SLES or openSUSE virtual machine for Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131).</span></span> <span data-ttu-id="5eec6-816">Kör inte hello sista steget (avställningsskript hello VM) ännu.</span><span class="sxs-lookup"><span data-stu-id="5eec6-816">Do not run hello last step (deprovisioning hello VM) yet.</span></span>

<span data-ttu-id="5eec6-817">tooconfigure kryptering toowork med Azure, hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-817">tooconfigure encryption toowork with Azure, do hello following:</span></span>
1. <span data-ttu-id="5eec6-818">Redigera hello /etc/dracut.conf och Lägg till följande rad hello:</span><span class="sxs-lookup"><span data-stu-id="5eec6-818">Edit hello /etc/dracut.conf, and add hello following line:</span></span>
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. <span data-ttu-id="5eec6-819">Kommentera ut dessa rader hello slutet av hello filen /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span><span class="sxs-lookup"><span data-stu-id="5eec6-819">Comment out these lines by hello end of hello file /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span></span>
 ```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
 ```

3. <span data-ttu-id="5eec6-820">Lägg till följande rad hello början av hello filen /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh hello:</span><span class="sxs-lookup"><span data-stu-id="5eec6-820">Append hello following line at hello beginning of hello file /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span></span>
 ```
    DRACUT_SYSTEMD=0
 ```
<span data-ttu-id="5eec6-821">Och ändra alla förekomster av:</span><span class="sxs-lookup"><span data-stu-id="5eec6-821">And change all occurrences of:</span></span>
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
<span data-ttu-id="5eec6-822">till:</span><span class="sxs-lookup"><span data-stu-id="5eec6-822">to:</span></span>
```
    if [ 1 ]; then
```
4. <span data-ttu-id="5eec6-823">Redigera /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh och Lägg till för ”# öppna LUKS enhet”:</span><span class="sxs-lookup"><span data-stu-id="5eec6-823">Edit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh and append it too“# Open LUKS device”:</span></span>

    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```
5. <span data-ttu-id="5eec6-824">Kör `/usr/sbin/dracut -f -v` tooupdate hello initrd.</span><span class="sxs-lookup"><span data-stu-id="5eec6-824">Run `/usr/sbin/dracut -f -v` tooupdate hello initrd.</span></span>

6. <span data-ttu-id="5eec6-825">Nu du kan ta bort etableringen hello VM och [överför den virtuella Hårddisken](#upload-encrypted-vhd-to-an-azure-storage-account) till Azure.</span><span class="sxs-lookup"><span data-stu-id="5eec6-825">Now you can deprovision hello VM and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

##### <a name="centos-7"></a><span data-ttu-id="5eec6-826">CentOS 7</span><span class="sxs-lookup"><span data-stu-id="5eec6-826">CentOS 7</span></span>
<span data-ttu-id="5eec6-827">tooconfigure kryptering under installationen av hello distribution hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-827">tooconfigure encryption during hello distribution installation, do hello following:</span></span>
1. <span data-ttu-id="5eec6-828">Välj **kryptera data** när du partitionera diskarna.</span><span class="sxs-lookup"><span data-stu-id="5eec6-828">Select **Encrypt my data** when you partition disks.</span></span>

 ![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. <span data-ttu-id="5eec6-830">Kontrollera att **kryptera** har valts för rot-partitionen.</span><span class="sxs-lookup"><span data-stu-id="5eec6-830">Make sure **Encrypt** is selected for root partition.</span></span>

 ![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. <span data-ttu-id="5eec6-832">Ange en lösenfras.</span><span class="sxs-lookup"><span data-stu-id="5eec6-832">Provide a passphrase.</span></span> <span data-ttu-id="5eec6-833">Detta är hello lösenfras som du kommer att överföra tooyour nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-833">This is hello passphrase that you will upload tooyour key vault.</span></span>

 ![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. <span data-ttu-id="5eec6-835">När du startar hello VM och ange en lösenfras använda hello lösenfras som du angav i steg 3.</span><span class="sxs-lookup"><span data-stu-id="5eec6-835">When you boot hello VM and are asked for a passphrase, use hello passphrase you provided in step 3.</span></span>

 ![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. <span data-ttu-id="5eec6-837">Förbered hello VM för överföring till Azure med hjälp av hello ”CentOS 7.0 +” instruktionerna i [förbereda en CentOS-baserad virtuell dator för Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70).</span><span class="sxs-lookup"><span data-stu-id="5eec6-837">Prepare hello VM for uploading into Azure by using hello "CentOS 7.0+" instructions in [Prepare a CentOS-based virtual machine for Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70).</span></span> <span data-ttu-id="5eec6-838">Kör inte hello sista steget (avställningsskript hello VM) ännu.</span><span class="sxs-lookup"><span data-stu-id="5eec6-838">Do not run hello last step (deprovisioning hello VM) yet.</span></span>

6. <span data-ttu-id="5eec6-839">Nu du kan ta bort etableringen hello VM och [överför den virtuella Hårddisken](#upload-encrypted-vhd-to-an-azure-storage-account) till Azure.</span><span class="sxs-lookup"><span data-stu-id="5eec6-839">Now you can deprovision hello VM and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

<span data-ttu-id="5eec6-840">tooconfigure kryptering toowork med Azure, hello följande:</span><span class="sxs-lookup"><span data-stu-id="5eec6-840">tooconfigure encryption toowork with Azure, do hello following:</span></span>

1. <span data-ttu-id="5eec6-841">Redigera hello /etc/dracut.conf och Lägg till följande rad hello:</span><span class="sxs-lookup"><span data-stu-id="5eec6-841">Edit hello /etc/dracut.conf, and add hello following line:</span></span>
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. <span data-ttu-id="5eec6-842">Kommentera ut dessa rader hello slutet av hello filen /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span><span class="sxs-lookup"><span data-stu-id="5eec6-842">Comment out these lines by hello end of hello file /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span></span>
```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
```

3. <span data-ttu-id="5eec6-843">Lägg till följande rad hello början av hello filen /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh hello:</span><span class="sxs-lookup"><span data-stu-id="5eec6-843">Append hello following line at hello beginning of hello file /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span></span>
```
    DRACUT_SYSTEMD=0
```
<span data-ttu-id="5eec6-844">Och ändra alla förekomster av:</span><span class="sxs-lookup"><span data-stu-id="5eec6-844">And change all occurrences of:</span></span>
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
<span data-ttu-id="5eec6-845">till</span><span class="sxs-lookup"><span data-stu-id="5eec6-845">to</span></span>
```
    if [ 1 ]; then
```
4. <span data-ttu-id="5eec6-846">Redigera /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh och Lägg till detta efter hello ”# öppna LUKS enhet”:</span><span class="sxs-lookup"><span data-stu-id="5eec6-846">Edit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh and append this after hello “# Open LUKS device”:</span></span>
    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```    
5. <span data-ttu-id="5eec6-847">Kör hello ”/ usr/sbin/dracut - f - v” tooupdate hello initrd.</span><span class="sxs-lookup"><span data-stu-id="5eec6-847">Run hello “/usr/sbin/dracut -f -v” tooupdate hello initrd.</span></span>

![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-tooan-azure-storage-account"></a><span data-ttu-id="5eec6-849">Överför krypterade VHD tooan Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="5eec6-849">Upload encrypted VHD tooan Azure storage account</span></span>
<span data-ttu-id="5eec6-850">När BitLocker-kryptering eller DM-Crypt kryptering har aktiverats måste krypteras hello lokala VHD måste toobe upp tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="5eec6-850">After BitLocker encryption or DM-Crypt encryption is enabled, hello local encrypted VHD needs toobe uploaded tooyour storage account.</span></span>

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-hello-disk-encryption-secret-for-hello-pre-encrypted-vm-tooyour-key-vault"></a><span data-ttu-id="5eec6-851">Överför hello-diskkryptering hemligheten för hello förkrypterade VM tooyour nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="5eec6-851">Upload hello disk-encryption secret for hello pre-encrypted VM tooyour key vault</span></span>
<span data-ttu-id="5eec6-852">hello-diskkryptering hemlighet som du hämtade måste tidigare laddas upp som en hemlighet i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-852">hello disk-encryption secret that you obtained previously must be uploaded as a secret in your key vault.</span></span> <span data-ttu-id="5eec6-853">Hej nyckelvalv måste toohave diskkryptering och behörigheter som har aktiverats för din Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="5eec6-853">hello key vault needs toohave disk encryption and permissions enabled for your Azure AD client.</span></span>

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a><span data-ttu-id="5eec6-854">Disk encryption hemlighet inte krypterats med en KEK</span><span class="sxs-lookup"><span data-stu-id="5eec6-854">Disk encryption secret not encrypted with a KEK</span></span>
<span data-ttu-id="5eec6-855">tooset in hello hemlighet i nyckelvalvet, Använd [Set AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret).</span><span class="sxs-lookup"><span data-stu-id="5eec6-855">tooset up hello secret in your key vault, use [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret).</span></span> <span data-ttu-id="5eec6-856">Om du har en virtuell dator för Windows hello bek filen kodas som en base64-sträng och överföra tooyour nyckelvalv med hello `Set-AzureKeyVaultSecret` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-856">If you have a Windows virtual machine, hello bek file is encoded as a base64 string and then uploaded tooyour key vault using hello `Set-AzureKeyVaultSecret` cmdlet.</span></span> <span data-ttu-id="5eec6-857">För Linux hello lösenfras kodas som en base64-sträng och överföra toohello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-857">For Linux, hello passphrase is encoded as a base64 string and then uploaded toohello key vault.</span></span> <span data-ttu-id="5eec6-858">Dessutom se till att hello följande taggar anges när du skapar hello hemlighet i nyckelvalvet hello.</span><span class="sxs-lookup"><span data-stu-id="5eec6-858">In addition, make sure that hello following tags are set when you create hello secret in hello key vault.</span></span>

    # This is hello passphrase that was provided for encryption during hello distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

<span data-ttu-id="5eec6-859">Använd hello `$secretUrl` i hello nästa steg för [kopplar hello OS-disk utan att använda KEK](#without-using-a-kek).</span><span class="sxs-lookup"><span data-stu-id="5eec6-859">Use hello `$secretUrl` in hello next step for [attaching hello OS disk without using KEK](#without-using-a-kek).</span></span>

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a><span data-ttu-id="5eec6-860">Disk encryption hemlighet krypteras med en KEK</span><span class="sxs-lookup"><span data-stu-id="5eec6-860">Disk encryption secret encrypted with a KEK</span></span>
<span data-ttu-id="5eec6-861">Innan du laddar upp hello hemliga toohello nyckelvalv kryptera om du vill den med hjälp av en nyckel krypteringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="5eec6-861">Before you upload hello secret toohello key vault, you can optionally encrypt it by using a key encryption key.</span></span> <span data-ttu-id="5eec6-862">Använd hello radbyte [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) toofirst kryptera hello hemligheten med hello viktiga krypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="5eec6-862">Use hello wrap [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) toofirst encrypt hello secret using hello key encryption key.</span></span> <span data-ttu-id="5eec6-863">hello utdata från åtgärden radbyte är en base64-URL-kodade sträng som du kan sedan överföra som en hemlighet hello [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-863">hello output of this wrap operation is a base64 URL encoded string, which you can then upload as a secret by using hello [`Set-AzureKeyVaultSecret`](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet.</span></span>

    # This is hello passphrase that was provided for encryption during hello distribution installation
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id

<span data-ttu-id="5eec6-864">Använd `$KeyEncryptionKey` och `$secretUrl` i hello nästa steg för [kopplar hello OS-disk med hjälp av KEK](#using-a-kek).</span><span class="sxs-lookup"><span data-stu-id="5eec6-864">Use `$KeyEncryptionKey` and `$secretUrl` in hello next step for [attaching hello OS disk using KEK](#using-a-kek).</span></span>

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a><span data-ttu-id="5eec6-865">Ange en hemlig URL när du ansluter en OS-disk</span><span class="sxs-lookup"><span data-stu-id="5eec6-865">Specify a secret URL when you attach an OS disk</span></span>
#### <a name="without-using-a-kek"></a><span data-ttu-id="5eec6-866">Utan att använda en KEK</span><span class="sxs-lookup"><span data-stu-id="5eec6-866">Without using a KEK</span></span>
<span data-ttu-id="5eec6-867">När du bifogar hello OS-disk, behöver du toopass `$secretUrl`.</span><span class="sxs-lookup"><span data-stu-id="5eec6-867">While you are attaching hello OS disk, you need toopass `$secretUrl`.</span></span> <span data-ttu-id="5eec6-868">hello URL har genererats i hello ”Disk encryption hemlighet inte krypterats med en KEK” avsnittet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-868">hello URL was generated in hello "Disk-encryption secret not encrypted with a KEK" section.</span></span>

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a><span data-ttu-id="5eec6-869">Med hjälp av en KEK</span><span class="sxs-lookup"><span data-stu-id="5eec6-869">Using a KEK</span></span>
<span data-ttu-id="5eec6-870">När du ansluter hello OS-disk, skicka `$KeyEncryptionKey` och `$secretUrl`.</span><span class="sxs-lookup"><span data-stu-id="5eec6-870">When you attach hello OS disk, pass `$KeyEncryptionKey` and `$secretUrl`.</span></span> <span data-ttu-id="5eec6-871">hello URL har genererats i hello ”Disk encryption hemlighet inte krypterats med en KEK” avsnittet.</span><span class="sxs-lookup"><span data-stu-id="5eec6-871">hello URL was generated in hello "Disk-encryption secret not encrypted with a KEK" section.</span></span>

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id

## <a name="download-this-guide"></a><span data-ttu-id="5eec6-872">Hämta denna guide</span><span class="sxs-lookup"><span data-stu-id="5eec6-872">Download this guide</span></span>
<span data-ttu-id="5eec6-873">Du kan hämta den här guiden från hello [TechNet-galleriet](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).</span><span class="sxs-lookup"><span data-stu-id="5eec6-873">You can download this guide from hello [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).</span></span>

## <a name="for-more-information"></a><span data-ttu-id="5eec6-874">Mer information</span><span class="sxs-lookup"><span data-stu-id="5eec6-874">For more information</span></span>
[<span data-ttu-id="5eec6-875">Utforska Azure Disk Encryption med Azure PowerShell - del 1</span><span class="sxs-lookup"><span data-stu-id="5eec6-875">Explore Azure Disk Encryption with Azure PowerShell - Part 1</span></span>](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)  
[<span data-ttu-id="5eec6-876">Utforska Azure Disk Encryption med Azure PowerShell - del 2</span><span class="sxs-lookup"><span data-stu-id="5eec6-876">Explore Azure Disk Encryption with Azure PowerShell - Part 2</span></span>](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
