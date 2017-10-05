---
title: "Azure Disk Encryption för Windows och Linux-IaaS-VM | Microsoft Docs"
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
ms.openlocfilehash: a4f20fc19ae40561d042d5cff744a030014f75c7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a><span data-ttu-id="531f3-103">Azure Disk Encryption för Windows och Linux-IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="531f3-103">Azure Disk Encryption for Windows and Linux IaaS VMs</span></span>
<span data-ttu-id="531f3-104">Microsoft Azure värnar starkt din datasekretess, data suveränitet och aktiverar du att styra dina Azure värdbaserade data via ett intervall med avancerade tekniker för att kryptera, styra och hantera krypteringsnycklar kontroll & granska åtkomsten till data.</span><span class="sxs-lookup"><span data-stu-id="531f3-104">Microsoft Azure is strongly committed to ensuring your data privacy, data sovereignty and enables you to control your Azure hosted data through a range of advanced technologies to encrypt, control and manage encryption keys, control & audit access of data.</span></span> <span data-ttu-id="531f3-105">Det ger Azure-kunder möjlighet att välja den lösning som bäst uppfyller deras behov av företag.</span><span class="sxs-lookup"><span data-stu-id="531f3-105">This provides Azure customers the flexibility to choose the solution that best meets their business needs.</span></span> <span data-ttu-id="531f3-106">I det här dokumentet, vi innehåller en introduktion till en ny tekniklösning ”Azure Disk Encryption för Windows och Linux IaaS VMS” om du vill skydda och skydda dina data för att uppfylla din organisations säkerhet och efterlevnad åtaganden.</span><span class="sxs-lookup"><span data-stu-id="531f3-106">In this paper, we will introduce you to a new technology solution “Azure Disk Encryption for Windows and Linux IaaS VM’s” to help protect and safeguard your data to meet your organizational security and compliance commitments.</span></span> <span data-ttu-id="531f3-107">Dokumentet ger detaljerad information om hur du använder Azure disk encryption-funktioner inklusive scenarierna som stöds och användaren inträffar.</span><span class="sxs-lookup"><span data-stu-id="531f3-107">The paper provides detailed guidance on how to use the Azure disk encryption features including the supported scenarios and the user experiences.</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-108">Vissa rekommendationerna kan öka data, nätverk och beräkning Resursanvändning, vilket resulterar i ytterligare kostnader för licens eller prenumeration.</span><span class="sxs-lookup"><span data-stu-id="531f3-108">Certain recommendations might increase data, network, or compute resource usage, resulting in additional license or subscription costs.</span></span>

## <a name="overview"></a><span data-ttu-id="531f3-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="531f3-109">Overview</span></span>
<span data-ttu-id="531f3-110">Azure Disk Encryption är en ny funktion som hjälper dig att kryptera din Windows- och Linux IaaS virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="531f3-110">Azure Disk Encryption is a new capability that helps you encrypt your Windows and Linux IaaS virtual machine disks.</span></span> <span data-ttu-id="531f3-111">Azure Disk Encryption utnyttjar branschstandarden [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funktion i Windows och [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funktion i Linux att tillhandahålla volymkryptering för Operativsystemet och datadiskar.</span><span class="sxs-lookup"><span data-stu-id="531f3-111">Azure Disk Encryption leverages the industry standard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) feature of Windows and the [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) feature of Linux to provide volume encryption for the OS and the data disks.</span></span> <span data-ttu-id="531f3-112">Lösningen är integrerad med [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) som hjälper dig att styra och hantera disk krypteringsnycklar och hemligheter i nyckelvalvet-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="531f3-112">The solution is integrated with [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) to help you control and manage the disk-encryption keys and secrets in your key vault subscription.</span></span> <span data-ttu-id="531f3-113">Lösningen betyder också att krypteras alla data på virtuella diskar i vila i ditt Azure storage.</span><span class="sxs-lookup"><span data-stu-id="531f3-113">The solution also ensures that all data on the virtual machine disks are encrypted at rest in your Azure storage.</span></span>

<span data-ttu-id="531f3-114">Azure disk encryption för Windows och Linux IaaS-VM är nu i **allmän tillgänglighet** i alla Azure offentliga och AzureGov regioner för Standard virtuella datorer och virtuella datorer med premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="531f3-114">Azure disk encryption for Windows and Linux IaaS VMs is now in **General Availability** in all Azure public regions and AzureGov regions for Standard  VMs and VMs with premium storage.</span></span>

### <a name="encryption-scenarios"></a><span data-ttu-id="531f3-115">Scenarier för kryptering</span><span class="sxs-lookup"><span data-stu-id="531f3-115">Encryption scenarios</span></span>
<span data-ttu-id="531f3-116">Azure Disk Encryption-lösningen stöder följande kundscenarier:</span><span class="sxs-lookup"><span data-stu-id="531f3-116">The Azure Disk Encryption solution supports the following customer scenarios:</span></span>

* <span data-ttu-id="531f3-117">Aktivera kryptering på den nya virtuella IaaS-datorer skapas från förkrypterade VHD- och krypteringsnycklar</span><span class="sxs-lookup"><span data-stu-id="531f3-117">Enable encryption on new IaaS VMs created from pre-encrypted VHD and encryption keys</span></span>
* <span data-ttu-id="531f3-118">Aktivera kryptering på den nya virtuella IaaS-datorer skapas från stöds avbildningar i Azure-galleriet</span><span class="sxs-lookup"><span data-stu-id="531f3-118">Enable encryption on new IaaS VMs created from the supported Azure Gallery images</span></span>
* <span data-ttu-id="531f3-119">Aktivera kryptering på befintliga virtuella IaaS-datorer körs i Azure</span><span class="sxs-lookup"><span data-stu-id="531f3-119">Enable encryption on existing IaaS VMs running in Azure</span></span>
* <span data-ttu-id="531f3-120">Inaktivera kryptering på Windows IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="531f3-120">Disable encryption on Windows IaaS VMs</span></span>
* <span data-ttu-id="531f3-121">Inaktivera kryptering på dataenheter för Linux virtuella IaaS-datorer</span><span class="sxs-lookup"><span data-stu-id="531f3-121">Disable encryption on data drives for Linux IaaS VMs</span></span>
* <span data-ttu-id="531f3-122">Aktivera kryptering för hanterade diskar virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="531f3-122">Enable encryption of managed disk VMs</span></span>
* <span data-ttu-id="531f3-123">Uppdatera krypteringsinställningarna för en befintlig krypterade premium-lagring VM</span><span class="sxs-lookup"><span data-stu-id="531f3-123">Update encryption settings of an existing encrypted non-premium storage VM</span></span>
* <span data-ttu-id="531f3-124">Säkerhetskopiering och återställning av krypterade virtuella datorer som krypterats med nyckelkryptering nyckel</span><span class="sxs-lookup"><span data-stu-id="531f3-124">Backup and restore of encrypted VMs, encrypted with key encryption key</span></span>

<span data-ttu-id="531f3-125">Lösningen stöder följande scenarion för virtuella IaaS-datorer när de är aktiverade i Microsoft Azure:</span><span class="sxs-lookup"><span data-stu-id="531f3-125">The solution supports the following scenarios for IaaS VMs when they are enabled in Microsoft Azure:</span></span>

* <span data-ttu-id="531f3-126">Integrering med Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="531f3-126">Integration with Azure Key Vault</span></span>
* <span data-ttu-id="531f3-127">Standardnivån VMs: [A, D, DS, G, GS, F och så vidare serien IaaS-VM](https://azure.microsoft.com/pricing/details/virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="531f3-127">Standard tier VMs: [A, D, DS, G, GS, F, and so forth series IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)</span></span>
* <span data-ttu-id="531f3-128">Aktivera kryptering på Windows- och Linux virtuella IaaS-datorer och hanterade diskar virtuella datorer från stöds avbildningar i Azure-galleriet</span><span class="sxs-lookup"><span data-stu-id="531f3-128">Enable encryption on Windows and Linux IaaS VMs and managed disk VMs from the supported Azure Gallery images</span></span>
* <span data-ttu-id="531f3-129">Inaktivera kryptering på Operativsystemet och enheter för Windows IaaS-VM och hanterade diskar virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="531f3-129">Disable encryption on OS and data drives for Windows IaaS VMs and managed disk VMs</span></span>
* <span data-ttu-id="531f3-130">Inaktivera kryptering på dataenheter för Linux virtuella IaaS-datorer och hanterade diskar virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="531f3-130">Disable encryption on data drives for Linux IaaS VMs and managed disk VMs</span></span>
* <span data-ttu-id="531f3-131">Aktivera kryptering på IaaS virtuella datorer som kör Windows klient-OS</span><span class="sxs-lookup"><span data-stu-id="531f3-131">Enable encryption on IaaS VMs running Windows Client OS</span></span>
* <span data-ttu-id="531f3-132">Aktivera kryptering på volymer med monteringssökvägar</span><span class="sxs-lookup"><span data-stu-id="531f3-132">Enable encryption on volumes with mount paths</span></span>
* <span data-ttu-id="531f3-133">Aktivera kryptering på den virtuella Linux-datorer konfigurerade med disk striping (RAID) med hjälp av mdadm</span><span class="sxs-lookup"><span data-stu-id="531f3-133">Enable encryption on Linux VMs configured with disk striping (RAID) using mdadm</span></span>
* <span data-ttu-id="531f3-134">Aktivera kryptering på den virtuella Linux-datorer med hjälp av LVM för datadiskar</span><span class="sxs-lookup"><span data-stu-id="531f3-134">Enable encryption on Linux VMs using LVM for data disks</span></span>
* <span data-ttu-id="531f3-135">Aktivera kryptering på virtuella Windows-datorer konfigurerade med lagringsutrymmen</span><span class="sxs-lookup"><span data-stu-id="531f3-135">Enable encryption on Windows VMs configured with Storage Spaces</span></span>
* <span data-ttu-id="531f3-136">Uppdatera krypteringsinställningarna för en befintlig krypterade premium-lagring VM</span><span class="sxs-lookup"><span data-stu-id="531f3-136">Update encryption settings of an existing encrypted non-premium storage VM</span></span>
* <span data-ttu-id="531f3-137">Alla offentliga Azure och AzureGov regioner som stöds</span><span class="sxs-lookup"><span data-stu-id="531f3-137">All Azure Public and AzureGov regions are supported</span></span>

<span data-ttu-id="531f3-138">Lösningen stöder inte följande scenarier, funktioner och teknik:</span><span class="sxs-lookup"><span data-stu-id="531f3-138">The solution does not support the following scenarios, features, and technology:</span></span>

* <span data-ttu-id="531f3-139">Grundnivån IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="531f3-139">Basic tier IaaS VMs</span></span>
* <span data-ttu-id="531f3-140">Om du inaktiverar kryptering på en OS-enhet för Linux virtuella IaaS-datorer</span><span class="sxs-lookup"><span data-stu-id="531f3-140">Disabling encryption on an OS drive for Linux IaaS VMs</span></span>
* <span data-ttu-id="531f3-141">Om du inaktiverar kryptering på en dataenhet om OS-enheten är krypterad för Linux virtuella Iaas-datorer</span><span class="sxs-lookup"><span data-stu-id="531f3-141">Disabling encryption on a data drive if the OS drive is encrypted for Linux Iaas VMs</span></span>
* <span data-ttu-id="531f3-142">Virtuella IaaS-datorer som skapas med hjälp av klassiska metod för skapande av virtuell dator</span><span class="sxs-lookup"><span data-stu-id="531f3-142">IaaS VMs that are created by using the classic VM creation method</span></span>
* <span data-ttu-id="531f3-143">Aktivera kryptering på Windows och Linux-IaaS-VM kunden anpassade avbildningar inte stöds.</span><span class="sxs-lookup"><span data-stu-id="531f3-143">Enable encryption on Windows and Linux IaaS VMs customer custom images is NOT supported.</span></span> <span data-ttu-id="531f3-144">Aktivera enccryption på Linux LVM OS disk stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="531f3-144">Enable enccryption on Linux LVM OS disk is not supported currently.</span></span> <span data-ttu-id="531f3-145">Det här stödet kommer snart.</span><span class="sxs-lookup"><span data-stu-id="531f3-145">This support will come soon.</span></span>
* <span data-ttu-id="531f3-146">Integrering med din lokala nyckelhanteringstjänst</span><span class="sxs-lookup"><span data-stu-id="531f3-146">Integration with your on-premises Key Management Service</span></span>
* <span data-ttu-id="531f3-147">Azure Files (delade filsystem), Network File System (NFS), dynamiska volymer och virtuella Windows-datorer som är konfigurerade med programvarubaserad RAID-system</span><span class="sxs-lookup"><span data-stu-id="531f3-147">Azure Files (shared file system), Network File System (NFS), dynamic volumes, and Windows VMs that are configured with software-based RAID systems</span></span>
* <span data-ttu-id="531f3-148">Säkerhetskopiering och återställning av krypterade virtuella datorer, krypterade utan nyckelkryptering nyckel.</span><span class="sxs-lookup"><span data-stu-id="531f3-148">Backup and restore of encrypted VMs, encrypted without key encryption key.</span></span>
* <span data-ttu-id="531f3-149">Uppdatera krypteringsinställningarna för en befintlig krypterade premium-lagring VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-149">Update encryption settings of an existing encrypted premium storage VM.</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-150">Säkerhetskopiering och återställning av krypterade virtuella datorer stöds bara för virtuella datorer som är krypterade med KEK konfiguration.</span><span class="sxs-lookup"><span data-stu-id="531f3-150">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with the KEK configuration.</span></span> <span data-ttu-id="531f3-151">Den stöds inte på virtuella datorer som är krypterade utan KEK.</span><span class="sxs-lookup"><span data-stu-id="531f3-151">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="531f3-152">KEK är en valfri parameter som aktiverar VM-kryptering.</span><span class="sxs-lookup"><span data-stu-id="531f3-152">KEK is an optional parameter that enables VM encryption.</span></span> <span data-ttu-id="531f3-153">Det här stödet kommer snart.</span><span class="sxs-lookup"><span data-stu-id="531f3-153">This support is coming soon.</span></span>
> <span data-ttu-id="531f3-154">Uppdatera krypteringsinställningarna för en befintlig krypterade premium-lagring VM inte stöds.</span><span class="sxs-lookup"><span data-stu-id="531f3-154">Update encryption settings of an existing encrypted premium storage VM are not supported.</span></span> <span data-ttu-id="531f3-155">Det här stödet kommer snart.</span><span class="sxs-lookup"><span data-stu-id="531f3-155">This support is coming soon.</span></span>

### <a name="encryption-features"></a><span data-ttu-id="531f3-156">Krypteringsfunktioner</span><span class="sxs-lookup"><span data-stu-id="531f3-156">Encryption features</span></span>
<span data-ttu-id="531f3-157">När du aktiverar och distribuera Azure Disk Encryption för Azure IaaS-VM är följande funktioner aktiverade beroende på konfigurationen som:</span><span class="sxs-lookup"><span data-stu-id="531f3-157">When you enable and deploy Azure Disk Encryption for Azure IaaS VMs, the following capabilities are enabled, depending on the configuration provided:</span></span>

* <span data-ttu-id="531f3-158">Kryptering av systemvolymen att skydda startvolymen i vila i lagringsutrymmet</span><span class="sxs-lookup"><span data-stu-id="531f3-158">Encryption of the OS volume to protect the boot volume at rest in your storage</span></span>
* <span data-ttu-id="531f3-159">Kryptering av datavolymer att skydda datavolymer i vila i lagringsutrymmet</span><span class="sxs-lookup"><span data-stu-id="531f3-159">Encryption of data volumes to protect the data volumes at rest in your storage</span></span>
* <span data-ttu-id="531f3-160">Om du inaktiverar kryptering på Operativsystemet och enheter för Windows IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="531f3-160">Disabling encryption on the OS and data drives for Windows IaaS VMs</span></span>
* <span data-ttu-id="531f3-161">Om du inaktiverar kryptering för data-enheter för Linux virtuella IaaS-datorer (endast om OS är inte krypterad enhet)</span><span class="sxs-lookup"><span data-stu-id="531f3-161">Disabling encryption on the data drives for Linux IaaS VMs (only if OS drive IS NOT encrypted)</span></span>
* <span data-ttu-id="531f3-162">Skydda krypteringsnycklar och hemligheter i nyckelvalvet-prenumeration</span><span class="sxs-lookup"><span data-stu-id="531f3-162">Safeguarding the encryption keys and secrets in your key vault subscription</span></span>
* <span data-ttu-id="531f3-163">Rapportering krypteringsstatus av krypterade IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="531f3-163">Reporting the encryption status of the encrypted IaaS VM</span></span>
* <span data-ttu-id="531f3-164">Borttagning av disk-kryptering konfigurationsinställningar från virtuell IaaS-dator</span><span class="sxs-lookup"><span data-stu-id="531f3-164">Removal of disk-encryption configuration settings from the IaaS virtual machine</span></span>
* <span data-ttu-id="531f3-165">Säkerhetskopiering och återställning av krypterade virtuella datorer med hjälp av Azure Backup-tjänsten</span><span class="sxs-lookup"><span data-stu-id="531f3-165">Backup and restore of encrypted VMs by using the Azure Backup service</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-166">Säkerhetskopiering och återställning av krypterade virtuella datorer stöds bara för virtuella datorer som är krypterade med KEK konfiguration.</span><span class="sxs-lookup"><span data-stu-id="531f3-166">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with the KEK configuration.</span></span> <span data-ttu-id="531f3-167">Den stöds inte på virtuella datorer som är krypterade utan KEK.</span><span class="sxs-lookup"><span data-stu-id="531f3-167">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="531f3-168">KEK är en valfri parameter som aktiverar VM-kryptering.</span><span class="sxs-lookup"><span data-stu-id="531f3-168">KEK is an optional parameter that enables VM encryption.</span></span>

<span data-ttu-id="531f3-169">Azure Disk Encryption för IaaS VMS för Windows och Linux-lösningen innehåller:</span><span class="sxs-lookup"><span data-stu-id="531f3-169">Azure Disk Encryption for IaaS VMS for Windows and Linux solution includes:</span></span>

* <span data-ttu-id="531f3-170">Diskkryptering tillägget för Windows.</span><span class="sxs-lookup"><span data-stu-id="531f3-170">The disk-encryption extension for Windows.</span></span>
* <span data-ttu-id="531f3-171">Tillägget för Linux-diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="531f3-171">The disk-encryption extension for Linux.</span></span>
* <span data-ttu-id="531f3-172">Disk-kryptering PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="531f3-172">The disk-encryption PowerShell cmdlets.</span></span>
* <span data-ttu-id="531f3-173">Disk-kryptering Azure-kommandoradsgränssnittet (CLI) cmdlets.</span><span class="sxs-lookup"><span data-stu-id="531f3-173">The disk-encryption Azure command-line interface (CLI) cmdlets.</span></span>
* <span data-ttu-id="531f3-174">Disk-kryptering Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="531f3-174">The disk-encryption Azure Resource Manager templates.</span></span>

<span data-ttu-id="531f3-175">Lösning för Azure Disk Encryption stöds på virtuella IaaS-datorer som kör Windows eller Linux OS.</span><span class="sxs-lookup"><span data-stu-id="531f3-175">The Azure Disk Encryption solution is supported on IaaS VMs that are running Windows or Linux OS.</span></span> <span data-ttu-id="531f3-176">Mer information om operativsystem som stöds finns i avsnittet ”förutsättningar”.</span><span class="sxs-lookup"><span data-stu-id="531f3-176">For more information about the supported operating systems, see the "Prerequisites" section.</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-177">Det finns inga extra kostnad för att kryptera Virtuella diskar med Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="531f3-177">There is no additional charge for encrypting VM disks with Azure Disk Encryption.</span></span>

### <a name="value-proposition"></a><span data-ttu-id="531f3-178">Förslagsvärde</span><span class="sxs-lookup"><span data-stu-id="531f3-178">Value proposition</span></span>
<span data-ttu-id="531f3-179">När du använder Azure Disk Encryption-hanteringslösningen kan du uppfylla följande affärsbehov:</span><span class="sxs-lookup"><span data-stu-id="531f3-179">When you apply the Azure Disk Encryption-management solution, you can satisfy the following business needs:</span></span>

* <span data-ttu-id="531f3-180">IaaS-VM är skyddade i vila, eftersom du kan använda branschstandardiserad krypteringsteknik för att adressera organisatoriska krav för säkerhet och efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="531f3-180">IaaS VMs are secured at rest, because you can use industry-standard encryption technology to address organizational security and compliance requirements.</span></span>
* <span data-ttu-id="531f3-181">IaaS-VM start under kund-kontrollerade nycklar och principer, och du kan granska deras användning i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-181">IaaS VMs boot under customer-controlled keys and policies, and you can audit their usage in your key vault.</span></span>

### <a name="encryption-workflow"></a><span data-ttu-id="531f3-182">Arbetsflöde för kryptering</span><span class="sxs-lookup"><span data-stu-id="531f3-182">Encryption workflow</span></span>
<span data-ttu-id="531f3-183">Om du vill aktivera kryptering för Windows och Linux virtuella datorer, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-183">To enable disk encryption for Windows and Linux VMs, do the following:</span></span>

1. <span data-ttu-id="531f3-184">Välj ett scenario för kryptering bland föregående kryptering scenarier.</span><span class="sxs-lookup"><span data-stu-id="531f3-184">Choose an encryption scenario from among the preceding encryption scenarios.</span></span>
2. <span data-ttu-id="531f3-185">Välja att aktivera disk kryptering via Azure Disk Encryption Resource Manager-mall, PowerShell-cmdlets eller CLI-kommando och ange konfigurationen för kryptering.</span><span class="sxs-lookup"><span data-stu-id="531f3-185">Opt in to enabling disk encryption via the Azure Disk Encryption Resource Manager template, PowerShell cmdlets, or CLI command, and specify the encryption configuration.</span></span>

   * <span data-ttu-id="531f3-186">Överför krypterade VHD för scenariot kund-krypterad VHD till ditt lagringskonto och kryptering nyckelmaterial till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-186">For the customer-encrypted VHD scenario, upload the encrypted VHD to your storage account and the encryption key material to your key vault.</span></span> <span data-ttu-id="531f3-187">Ange hur kryptering för att aktivera kryptering på en ny IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-187">Then, provide the encryption configuration to enable encryption on a new IaaS VM.</span></span>
   * <span data-ttu-id="531f3-188">Ange den kryptering för att aktivera kryptering på IaaS-VM för nya virtuella datorer som skapas från Marketplace och befintliga virtuella datorer som redan körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="531f3-188">For new VMs that are created from the Marketplace and existing VMs that are already running in Azure, provide the encryption configuration to enable encryption on the IaaS VM.</span></span>

3. <span data-ttu-id="531f3-189">Bevilja åtkomst till Azure-plattformen läsa krypteringsnyckel för material (krypteringsnycklar BitLocker för Windows-System) och lösenfrasen för Linux från nyckelvalvet att aktivera kryptering på IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-189">Grant access to the Azure platform to read the encryption-key material (BitLocker encryption keys for Windows systems and Passphrase for Linux) from your key vault to enable encryption on the IaaS VM.</span></span>

4. <span data-ttu-id="531f3-190">Ange Programidentitet Azure Active Directory (AD Azure) om du vill skriva krypteringsnyckeln materialet till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-190">Provide the Azure Active Directory (Azure AD) application identity to write the encryption key material to your key vault.</span></span> <span data-ttu-id="531f3-191">På så sätt kan kryptering på IaaS-VM för scenarier som nämns i steg 2.</span><span class="sxs-lookup"><span data-stu-id="531f3-191">Doing so enables encryption on the IaaS VM for the scenarios mentioned in step 2.</span></span>

5. <span data-ttu-id="531f3-192">Azure uppdaterar tjänstmodell VM med kryptering och nyckelvalv-konfigurationen och ställer in den kryptera virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-192">Azure updates the VM service model with encryption and the key vault configuration, and sets up your encrypted VM.</span></span>

 ![Microsoft Antimalware i Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a><span data-ttu-id="531f3-194">Arbetsflöde för dekryptering</span><span class="sxs-lookup"><span data-stu-id="531f3-194">Decryption workflow</span></span>
<span data-ttu-id="531f3-195">Om du vill inaktivera hårddiskkryptering för IaaS-VM utför du följande anvisningar:</span><span class="sxs-lookup"><span data-stu-id="531f3-195">To disable disk encryption for IaaS VMs, complete the following high-level steps:</span></span>

1. <span data-ttu-id="531f3-196">Välja att inaktivera kryptering (dekryptering) på en körs IaaS-VM i Azure via Azure Disk Encryption Resource Manager-mall eller PowerShell-cmdlets och ange konfigurationen för dekryptering.</span><span class="sxs-lookup"><span data-stu-id="531f3-196">Choose to disable encryption (decryption) on a running IaaS VM in Azure via the Azure Disk Encryption Resource Manager template or PowerShell cmdlets, and specify the decryption configuration.</span></span>

 <span data-ttu-id="531f3-197">Det här steget inaktiverar kryptering av Operativsystemet eller datavolym eller både på körs Windows IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-197">This step disables encryption of the OS or the data volume or both on the running Windows IaaS VM.</span></span> <span data-ttu-id="531f3-198">Men som nämns i föregående avsnitt stöds om du inaktiverar kryptering för OS-disk för Linux inte.</span><span class="sxs-lookup"><span data-stu-id="531f3-198">However, as mentioned in the previous section, disabling OS disk encryption for Linux is not supported.</span></span> <span data-ttu-id="531f3-199">Steget dekryptering är endast tillåten för dataenheter på Linux virtuella datorer som OS-disken inte krypteras.</span><span class="sxs-lookup"><span data-stu-id="531f3-199">The decryption step is allowed only for data drives on Linux VMs as long as the OS disk is not encrypted.</span></span>
2. <span data-ttu-id="531f3-200">Azure uppdaterar VM tjänstmodell och IaaS VM markeras dekrypterade.</span><span class="sxs-lookup"><span data-stu-id="531f3-200">Azure updates the VM service model, and the IaaS VM is marked decrypted.</span></span> <span data-ttu-id="531f3-201">Innehållet i den virtuella datorn är inte längre krypterat i vila.</span><span class="sxs-lookup"><span data-stu-id="531f3-201">The contents of the VM are no longer encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-202">Inaktivera kryptering åtgärden tar inte bort nyckelvalvet och nyckelmaterial för kryptering (krypteringsnycklar BitLocker för Windows-System) eller lösenfrasen för Linux.</span><span class="sxs-lookup"><span data-stu-id="531f3-202">The disable-encryption operation does not delete your key vault and the encryption key material (BitLocker encryption keys for Windows systems or Passphrase for Linux).</span></span>
 > <span data-ttu-id="531f3-203">Om du inaktiverar kryptering för OS-disk för Linux stöds inte.</span><span class="sxs-lookup"><span data-stu-id="531f3-203">Disabling OS disk encryption for Linux is not supported.</span></span> <span data-ttu-id="531f3-204">Steget dekryptering är endast tillåtna för dataenheter i virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="531f3-204">The decryption step is allowed only for data drives on Linux VMs.</span></span>
<span data-ttu-id="531f3-205">Inaktivera disk datakryptering för Linux stöds inte om OS-enheten är krypterad.</span><span class="sxs-lookup"><span data-stu-id="531f3-205">Disabling data disk encryption for Linux is not supported if the OS drive is encrypted.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="531f3-206">Krav</span><span class="sxs-lookup"><span data-stu-id="531f3-206">Prerequisites</span></span>
<span data-ttu-id="531f3-207">Innan du aktiverar Azure Disk Encryption på Azure IaaS-VM för de scenarier som stöds som beskrivs i avsnittet ”Översikt”, se följande krav:</span><span class="sxs-lookup"><span data-stu-id="531f3-207">Before you enable Azure Disk Encryption on Azure IaaS VMs for the supported scenarios that were discussed in the "Overview" section, see the following prerequisites:</span></span>

* <span data-ttu-id="531f3-208">Du måste ha en giltig aktiv Azure-prenumeration att skapa resurser i Azure i regionerna som stöds.</span><span class="sxs-lookup"><span data-stu-id="531f3-208">You must have a valid active Azure subscription to create resources in Azure in the supported regions.</span></span>
* <span data-ttu-id="531f3-209">Azure Disk Encryption stöds på följande versioner av Windows Server: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 och Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="531f3-209">Azure Disk Encryption is supported on the following Windows Server versions: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, and Windows Server 2016.</span></span>
* <span data-ttu-id="531f3-210">Azure Disk Encryption stöds på följande Windows-klientversioner: klienten för Windows 8 och Windows 10-klient.</span><span class="sxs-lookup"><span data-stu-id="531f3-210">Azure Disk Encryption is supported on the following Windows client versions: Windows 8 client and Windows 10 client.</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-211">Windows Server 2008 R2, måste du ha .NET Framework 4.5 installerat innan du aktiverar kryptering i Azure.</span><span class="sxs-lookup"><span data-stu-id="531f3-211">For Windows Server 2008 R2, you must have .NET Framework 4.5 installed before you enable encryption in Azure.</span></span> <span data-ttu-id="531f3-212">Du kan installera det från Windows Update genom att installera valfria Microsoft .NET Framework 4.5.2 för Windows Server 2008 R2 x64-baserade system ([KB2901983](https://support.microsoft.com/kb/2901983)).</span><span class="sxs-lookup"><span data-stu-id="531f3-212">You can install it from Windows Update by installing the optional update Microsoft .NET Framework 4.5.2 for Windows Server 2008 R2 x64-based systems ([KB2901983](https://support.microsoft.com/kb/2901983)).</span></span>

* <span data-ttu-id="531f3-213">Azure Disk Encryption stöds på följande Azure-galleriet baserat Linux server-distributioner och versioner:</span><span class="sxs-lookup"><span data-stu-id="531f3-213">Azure Disk Encryption is supported on the following Azure Gallery based Linux server distributions and versions:</span></span>

| <span data-ttu-id="531f3-214">Linux-Distribution</span><span class="sxs-lookup"><span data-stu-id="531f3-214">Linux Distribution</span></span> | <span data-ttu-id="531f3-215">Version</span><span class="sxs-lookup"><span data-stu-id="531f3-215">Version</span></span> | <span data-ttu-id="531f3-216">Volymtyp som stöds för kryptering</span><span class="sxs-lookup"><span data-stu-id="531f3-216">Volume Type Supported for Encryption</span></span>|
| --- | --- |--- |
| <span data-ttu-id="531f3-217">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="531f3-217">Ubuntu</span></span> | <span data-ttu-id="531f3-218">16.04-VARJE DAG-LTS</span><span class="sxs-lookup"><span data-stu-id="531f3-218">16.04-DAILY-LTS</span></span> | <span data-ttu-id="531f3-219">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="531f3-219">OS and Data disk</span></span> |
| <span data-ttu-id="531f3-220">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="531f3-220">Ubuntu</span></span> | <span data-ttu-id="531f3-221">14.04.5-DAILY-LTS</span><span class="sxs-lookup"><span data-stu-id="531f3-221">14.04.5-DAILY-LTS</span></span> | <span data-ttu-id="531f3-222">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="531f3-222">OS and Data disk</span></span> |
| <span data-ttu-id="531f3-223">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="531f3-223">Ubuntu</span></span> | <span data-ttu-id="531f3-224">12.10</span><span class="sxs-lookup"><span data-stu-id="531f3-224">12.10</span></span> | <span data-ttu-id="531f3-225">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-225">Data disk</span></span> |
| <span data-ttu-id="531f3-226">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="531f3-226">Ubuntu</span></span> | <span data-ttu-id="531f3-227">12.04</span><span class="sxs-lookup"><span data-stu-id="531f3-227">12.04</span></span> | <span data-ttu-id="531f3-228">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-228">Data disk</span></span> |
| <span data-ttu-id="531f3-229">RHEL</span><span class="sxs-lookup"><span data-stu-id="531f3-229">RHEL</span></span> | <span data-ttu-id="531f3-230">7.3</span><span class="sxs-lookup"><span data-stu-id="531f3-230">7.3</span></span> | <span data-ttu-id="531f3-231">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="531f3-231">OS and Data disk</span></span> |
| <span data-ttu-id="531f3-232">RHEL</span><span class="sxs-lookup"><span data-stu-id="531f3-232">RHEL</span></span> | <span data-ttu-id="531f3-233">7.2</span><span class="sxs-lookup"><span data-stu-id="531f3-233">7.2</span></span> | <span data-ttu-id="531f3-234">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="531f3-234">OS and Data disk</span></span> |
| <span data-ttu-id="531f3-235">RHEL</span><span class="sxs-lookup"><span data-stu-id="531f3-235">RHEL</span></span> | <span data-ttu-id="531f3-236">6.8</span><span class="sxs-lookup"><span data-stu-id="531f3-236">6.8</span></span> | <span data-ttu-id="531f3-237">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="531f3-237">OS and Data disk</span></span> |
| <span data-ttu-id="531f3-238">RHEL</span><span class="sxs-lookup"><span data-stu-id="531f3-238">RHEL</span></span> | <span data-ttu-id="531f3-239">6.7</span><span class="sxs-lookup"><span data-stu-id="531f3-239">6.7</span></span> | <span data-ttu-id="531f3-240">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-240">Data disk</span></span> |
| <span data-ttu-id="531f3-241">CentOS</span><span class="sxs-lookup"><span data-stu-id="531f3-241">CentOS</span></span> | <span data-ttu-id="531f3-242">7.3</span><span class="sxs-lookup"><span data-stu-id="531f3-242">7.3</span></span> | <span data-ttu-id="531f3-243">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="531f3-243">OS and Data disk</span></span> |
| <span data-ttu-id="531f3-244">CentOS</span><span class="sxs-lookup"><span data-stu-id="531f3-244">CentOS</span></span> | <span data-ttu-id="531f3-245">7.2n</span><span class="sxs-lookup"><span data-stu-id="531f3-245">7.2n</span></span> | <span data-ttu-id="531f3-246">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="531f3-246">OS and Data disk</span></span> |
| <span data-ttu-id="531f3-247">CentOS</span><span class="sxs-lookup"><span data-stu-id="531f3-247">CentOS</span></span> | <span data-ttu-id="531f3-248">6.8</span><span class="sxs-lookup"><span data-stu-id="531f3-248">6.8</span></span> | <span data-ttu-id="531f3-249">OS- och disk</span><span class="sxs-lookup"><span data-stu-id="531f3-249">OS and Data disk</span></span> |
| <span data-ttu-id="531f3-250">CentOS</span><span class="sxs-lookup"><span data-stu-id="531f3-250">CentOS</span></span> | <span data-ttu-id="531f3-251">7.1</span><span class="sxs-lookup"><span data-stu-id="531f3-251">7.1</span></span> | <span data-ttu-id="531f3-252">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-252">Data disk</span></span> |
| <span data-ttu-id="531f3-253">CentOS</span><span class="sxs-lookup"><span data-stu-id="531f3-253">CentOS</span></span> | <span data-ttu-id="531f3-254">7.0</span><span class="sxs-lookup"><span data-stu-id="531f3-254">7.0</span></span> | <span data-ttu-id="531f3-255">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-255">Data disk</span></span> |
| <span data-ttu-id="531f3-256">CentOS</span><span class="sxs-lookup"><span data-stu-id="531f3-256">CentOS</span></span> | <span data-ttu-id="531f3-257">6.7</span><span class="sxs-lookup"><span data-stu-id="531f3-257">6.7</span></span> | <span data-ttu-id="531f3-258">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-258">Data disk</span></span> |
| <span data-ttu-id="531f3-259">CentOS</span><span class="sxs-lookup"><span data-stu-id="531f3-259">CentOS</span></span> | <span data-ttu-id="531f3-260">6.6</span><span class="sxs-lookup"><span data-stu-id="531f3-260">6.6</span></span> | <span data-ttu-id="531f3-261">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-261">Data disk</span></span> |
| <span data-ttu-id="531f3-262">CentOS</span><span class="sxs-lookup"><span data-stu-id="531f3-262">CentOS</span></span> | <span data-ttu-id="531f3-263">6.5</span><span class="sxs-lookup"><span data-stu-id="531f3-263">6.5</span></span> | <span data-ttu-id="531f3-264">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-264">Data disk</span></span> |
| <span data-ttu-id="531f3-265">openSUSE</span><span class="sxs-lookup"><span data-stu-id="531f3-265">openSUSE</span></span> | <span data-ttu-id="531f3-266">13.2</span><span class="sxs-lookup"><span data-stu-id="531f3-266">13.2</span></span> | <span data-ttu-id="531f3-267">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-267">Data disk</span></span> |
| <span data-ttu-id="531f3-268">SLES</span><span class="sxs-lookup"><span data-stu-id="531f3-268">SLES</span></span> | <span data-ttu-id="531f3-269">12 SP1</span><span class="sxs-lookup"><span data-stu-id="531f3-269">12 SP1</span></span> | <span data-ttu-id="531f3-270">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-270">Data disk</span></span> |
| <span data-ttu-id="531f3-271">SLES</span><span class="sxs-lookup"><span data-stu-id="531f3-271">SLES</span></span> | <span data-ttu-id="531f3-272">12-SP1 (Premium)</span><span class="sxs-lookup"><span data-stu-id="531f3-272">12-SP1 (Premium)</span></span> | <span data-ttu-id="531f3-273">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-273">Data disk</span></span> |
| <span data-ttu-id="531f3-274">SLES</span><span class="sxs-lookup"><span data-stu-id="531f3-274">SLES</span></span> | <span data-ttu-id="531f3-275">HPC 12</span><span class="sxs-lookup"><span data-stu-id="531f3-275">HPC 12</span></span> | <span data-ttu-id="531f3-276">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-276">Data disk</span></span> |
| <span data-ttu-id="531f3-277">SLES</span><span class="sxs-lookup"><span data-stu-id="531f3-277">SLES</span></span> | <span data-ttu-id="531f3-278">11 SP4 (Premium)</span><span class="sxs-lookup"><span data-stu-id="531f3-278">11-SP4 (Premium)</span></span> | <span data-ttu-id="531f3-279">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-279">Data disk</span></span> |
| <span data-ttu-id="531f3-280">SLES</span><span class="sxs-lookup"><span data-stu-id="531f3-280">SLES</span></span> | <span data-ttu-id="531f3-281">11 SP4</span><span class="sxs-lookup"><span data-stu-id="531f3-281">11 SP4</span></span> | <span data-ttu-id="531f3-282">Datadisk</span><span class="sxs-lookup"><span data-stu-id="531f3-282">Data disk</span></span> |

* <span data-ttu-id="531f3-283">Azure Disk Encryption kräver att dina nyckelvalvet och virtuella datorer finns i samma Azure-region och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="531f3-283">Azure Disk Encryption requires that your key vault and VMs reside in the same Azure region and subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-284">Konfigurera resurserna i olika områden orsakar ett fel i Azure Disk Encryption funktionen aktiveras.</span><span class="sxs-lookup"><span data-stu-id="531f3-284">Configuring the resources in separate regions causes a failure in enabling the Azure Disk Encryption feature.</span></span>

* <span data-ttu-id="531f3-285">Om du vill installera och konfigurera nyckelvalvet för Azure Disk Encryption, finns i avsnittet **ställa upp och konfigurera nyckelvalvet för Azure Disk Encryption** i den *krav* i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="531f3-285">To set up and configure your key vault for Azure Disk Encryption, see section **Set up and configure your key vault for Azure Disk Encryption** in the *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="531f3-286">Om du vill installera och konfigurera Azure AD-program i Azure Active directory för Azure Disk Encryption, finns i avsnittet **konfigurera Azure AD-program i Azure Active Directory** i den *krav* i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="531f3-286">To set up and configure Azure AD application in Azure Active directory for Azure Disk Encryption, see section **Set up the Azure AD application in Azure Active Directory** in the *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="531f3-287">Om du vill installera och konfigurera nyckelvalv åtkomstprincip för Azure AD-program, finns i avsnittet **konfigurera nyckelvalv åtkomstprincip för Azure AD-programmet** i den *krav* i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="531f3-287">To set up and configure the key vault access policy for the Azure AD application, see section **Set up the key vault access policy for the Azure AD application** in the *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="531f3-288">För att förbereda en förkrypterade Windows VHD finns i avsnittet **förbereda en förkrypterade Windows VHD** i den *bilaga*.</span><span class="sxs-lookup"><span data-stu-id="531f3-288">To prepare a pre-encrypted Windows VHD, see section **Prepare a pre-encrypted Windows VHD** in the *Appendix*.</span></span>
* <span data-ttu-id="531f3-289">För att förbereda en förkrypterade Linux VHD finns i avsnittet **förbereda en förkrypterade Linux VHD** i den *bilaga*.</span><span class="sxs-lookup"><span data-stu-id="531f3-289">To prepare a pre-encrypted Linux VHD, see section **Prepare a pre-encrypted Linux VHD** in the *Appendix*.</span></span>
* <span data-ttu-id="531f3-290">Azure-plattformen behöver åtkomst till krypteringsnycklarna eller hemligheter i nyckelvalvet att göra dem tillgängliga för den virtuella datorn när den startar och dekrypterar systemvolymen virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="531f3-290">The Azure platform needs access to the encryption keys or secrets in your key vault to make them available to the virtual machine when it boots and decrypts the virtual machine OS volume.</span></span> <span data-ttu-id="531f3-291">Om du vill tilldela behörigheter till Azure-plattformen, ange den **EnabledForDiskEncryption** egenskap i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-291">To grant permissions to Azure platform, set the **EnabledForDiskEncryption** property in the key vault.</span></span> <span data-ttu-id="531f3-292">Mer information finns i **ställa in och konfigurera nyckelvalvet för Azure Disk Encryption** i tillägget.</span><span class="sxs-lookup"><span data-stu-id="531f3-292">For more information, see **Set up and configure your key vault for Azure Disk Encryption** in the Appendix.</span></span>
* <span data-ttu-id="531f3-293">Ditt nyckelvalv hemlighet och KEK URL: er måste vara en ny version.</span><span class="sxs-lookup"><span data-stu-id="531f3-293">Your key vault secret and KEK URLs must be versioned.</span></span> <span data-ttu-id="531f3-294">Azure tillämpar den här begränsningen för versionshantering.</span><span class="sxs-lookup"><span data-stu-id="531f3-294">Azure enforces this restriction of versioning.</span></span> <span data-ttu-id="531f3-295">Giltig hemlighet och KEK URL: er, se följande exempel:</span><span class="sxs-lookup"><span data-stu-id="531f3-295">For valid secret and KEK URLs, see the following examples:</span></span>

  * <span data-ttu-id="531f3-296">Exempel på en giltig hemliga URL: *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="531f3-296">Example of a valid secret URL:   *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>
  * <span data-ttu-id="531f3-297">Exempel på en giltig URL för KEK: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="531f3-297">Example of a valid KEK URL:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>

* <span data-ttu-id="531f3-298">Azure Disk Encryption stöder inte att ange portnummer som en del av KEK URL: er och hemligheter i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-298">Azure Disk Encryption does not support specifying port numbers as part of key vault secrets and KEK URLs.</span></span> <span data-ttu-id="531f3-299">Exempel på inte stöds och stöds nyckelvalv URL: er finns i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="531f3-299">For examples of non-supported and supported key vault URLs, see the following:</span></span>

  * <span data-ttu-id="531f3-300">Oacceptabel key vault-URL *https://contosovault.vault.azure.net:443/hemligheter/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="531f3-300">Unacceptable key vault URL  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>
  * <span data-ttu-id="531f3-301">Godkända key vault-URL: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="531f3-301">Acceptable key vault URL:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>

* <span data-ttu-id="531f3-302">Om du vill aktivera Azure Disk Encryption måste IaaS-VM-funktionen uppfylla följande slutpunkt configuration nätverkskrav:</span><span class="sxs-lookup"><span data-stu-id="531f3-302">To enable the Azure Disk Encryption feature, the IaaS VMs must meet the following network endpoint configuration requirements:</span></span>
  * <span data-ttu-id="531f3-303">För att få en token för att ansluta till nyckelvalvet, IaaS VM måste kunna ansluta till en Azure Active Directory-slutpunkt \[login.microsoftonline.com\].</span><span class="sxs-lookup"><span data-stu-id="531f3-303">To get a token to connect to your key vault, the IaaS VM must be able to connect to an Azure Active Directory endpoint, \[login.microsoftonline.com\].</span></span>
  * <span data-ttu-id="531f3-304">Om du vill skriva krypteringsnycklarna till nyckelvalvet, kunna IaaS VM ansluta till slutpunkt för nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="531f3-304">To write the encryption keys to your key vault, the IaaS VM must be able to connect to the key vault endpoint.</span></span>
  * <span data-ttu-id="531f3-305">IaaS VM måste kunna ansluta till en Azure storage-slutpunkt som är värd för databasen Azure-tillägget och ett Azure storage-konto som är värd för VHD-filer.</span><span class="sxs-lookup"><span data-stu-id="531f3-305">The IaaS VM must be able to connect to an Azure storage endpoint that hosts the Azure extension repository and an Azure storage account that hosts the VHD files.</span></span>

  > [!NOTE]
  > <span data-ttu-id="531f3-306">Om din säkerhetsprincip begränsar åtkomst från Azure virtuella datorer till Internet, kan du matcha föregående URI: N och konfigurera en specifik regel som tillåter utgående anslutningar till IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="531f3-306">If your security policy limits access from Azure VMs to the Internet, you can resolve the preceding URI and configure a specific rule to allow outbound connectivity to the IPs.</span></span>
  >
  ><span data-ttu-id="531f3-307">Att konfigurera och komma åt Azure Key Vault bakom en brandvägg (https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall)</span><span class="sxs-lookup"><span data-stu-id="531f3-307">To configure and access Azure Key Vault behind a firewall(https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall)</span></span>

* <span data-ttu-id="531f3-308">Använd den senaste versionen av Azure PowerShell SDK-version för att konfigurera Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="531f3-308">Use the latest version of Azure PowerShell SDK version to configure Azure Disk Encryption.</span></span> <span data-ttu-id="531f3-309">Hämta den senaste versionen av [versionen av Azure PowerShell](https://github.com/Azure/azure-powershell/releases)</span><span class="sxs-lookup"><span data-stu-id="531f3-309">Download the latest version of [Azure PowerShell release](https://github.com/Azure/azure-powershell/releases)</span></span>

 > [!NOTE]
  > <span data-ttu-id="531f3-310">Azure Disk Encryption stöds inte på [Azure PowerShell SDK version 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016).</span><span class="sxs-lookup"><span data-stu-id="531f3-310">Azure Disk Encryption is not supported on [Azure PowerShell SDK version 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016).</span></span> <span data-ttu-id="531f3-311">Om du får ett fel om att använda Azure PowerShell 1.1.0, se [Azure Disk Encryption fel relaterade till Azure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).</span><span class="sxs-lookup"><span data-stu-id="531f3-311">If you are receiving an error related to using Azure PowerShell 1.1.0, see [Azure Disk Encryption Error Related to Azure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).</span></span>

* <span data-ttu-id="531f3-312">Om du vill köra alla Azure CLI-kommandon och koppla den till din Azure-prenumeration, måste du först installera Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="531f3-312">To run any Azure CLI command and associate it with your Azure subscription, you must first install Azure CLI:</span></span>
  * <span data-ttu-id="531f3-313">Om du vill installera Azure CLI och koppla den till din Azure-prenumeration, se [installera och konfigurera Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="531f3-313">To install Azure CLI and associate it with your Azure subscription, see [How to install and configure Azure CLI](../cli-install-nodejs.md).</span></span>
  * <span data-ttu-id="531f3-314">Om du vill använda Azure CLI för Mac, Linux och Windows med Azure Resource Manager finns [Azure CLI-kommandona i Resource Manager-läget](../virtual-machines/azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="531f3-314">To use Azure CLI for Mac, Linux, and Windows with Azure Resource Manager, see [Azure CLI commands in Resource Manager mode](../virtual-machines/azure-cli-arm-commands.md).</span></span>

* <span data-ttu-id="531f3-315">När du krypterar en hanterade diskar är obligatorisk förutsättning för att ta en ögonblicksbild av den hantera disken eller en säkerhetskopia av disken utanför Azure Disk Encryption innan du aktiverar kryptering.</span><span class="sxs-lookup"><span data-stu-id="531f3-315">When encrypting a managed disk, it is mandatory prerequisite to take a snapshot of the managed disk or a backup of the disk outside of Azure Disk Encryption prior to enabling encryption.</span></span>  <span data-ttu-id="531f3-316">Utan en säkerhetskopia för kan ett oväntat fel under krypteringen kanske den disken och VM användas utan ett återställningsalternativ.</span><span class="sxs-lookup"><span data-stu-id="531f3-316">Without a backup in place, any unexpected failure during encryption may render the disk and VM inaccessible without a recovery option.</span></span>  <span data-ttu-id="531f3-317">Ange AzureRmVMDiskEncryptionExtension för närvarande inte säkerhetskopiera hanterade diskar och kommer fel om användas mot en hanterad disk såvida inte parametern - skipVmBackup har angetts.</span><span class="sxs-lookup"><span data-stu-id="531f3-317">Set-AzureRmVMDiskEncryptionExtension does not currently back up managed disks and will error if used against a managed disk unless the -skipVmBackup parameter has been specified.</span></span>  <span data-ttu-id="531f3-318">Den här parametern är inte säkert att använda såvida inte en säkerhetskopia har redan gjorts utanför Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="531f3-318">This parameter is unsafe to use unless a backup has already been made outside of Azure Disk Encryption.</span></span>   <span data-ttu-id="531f3-319">När parametern - skipVmBackup anges cmdlet inte att göra en säkerhetskopia av den hantera disken före kryptering.</span><span class="sxs-lookup"><span data-stu-id="531f3-319">When the -skipVmBackup parameter is specified, the cmdlet will not make a backup of the managed disk prior to encryption.</span></span>  <span data-ttu-id="531f3-320">Därför anses en obligatorisk förutsättning för att se till att en säkerhetskopia av den hantera disken VM är på plats innan du aktiverar Azure Disk Encryption om senare behövs för återställning.</span><span class="sxs-lookup"><span data-stu-id="531f3-320">For this reason, it is considered a mandatory prerequisite to make sure a backup of the managed disk VM is in place prior to enabling Azure Disk Encryption in case recovery is later needed.</span></span>  
> [!NOTE]
 > <span data-ttu-id="531f3-321">Parametern - skipVmBackup ska aldrig användas om en ögonblicksbild eller säkerhetskopiering har redan gjorts utanför Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="531f3-321">The -skipVmBackup parameter should never be used unless a snapshot or backup has already been made outside of Azure Disk Encryption.</span></span> 

* <span data-ttu-id="531f3-322">Azure Disk Encryption lösningen använder BitLocker externa nyckelskyddet för Windows IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-322">The Azure Disk Encryption solution uses the BitLocker external key protector for Windows IaaS VMs.</span></span> <span data-ttu-id="531f3-323">Domän push anslutna virtuella datorer, inte i alla grupprinciper som genomdriva TPM-skydd.</span><span class="sxs-lookup"><span data-stu-id="531f3-323">For domain joined VMs, DO NOT push any group policies that enforce TPM protectors.</span></span> <span data-ttu-id="531f3-324">Information om en grupprincip för ”Tillåt BitLocker utan en kompatibel TPM”, se [BitLocker gruppolicy referens](https://technet.microsoft.com/library/ee706521).</span><span class="sxs-lookup"><span data-stu-id="531f3-324">For information about the group policy for “Allow BitLocker without a compatible TPM,” see [BitLocker Group Policy Reference](https://technet.microsoft.com/library/ee706521).</span></span>
* <span data-ttu-id="531f3-325">BitLocker-principen på domänanslutna virtuella datorer med anpassade Grupprincip måste innehålla följande inställning: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key` Azure Disk Encryption misslyckas när anpassade grupprincipinställningarna för Bitlocker är inkompatibla.</span><span class="sxs-lookup"><span data-stu-id="531f3-325">Bitlocker policy on domain joined virtual machines with custom group policy must include the following setting: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key`  Azure Disk Encryption will fail when custom group policy settings for Bitlocker are incompatible.</span></span> <span data-ttu-id="531f3-326">På datorer som inte har rätt princip kan inställningen, tillämpa den nya principen, tvingar den nya principen för att uppdatera (gpupdate.exe/Force) och starta sedan om krävas.</span><span class="sxs-lookup"><span data-stu-id="531f3-326">On machines that did not have the correct policy setting, applying the new policy, forcing the new policy to update (gpupdate.exe /force), and then restarting may be required.</span></span>  
* <span data-ttu-id="531f3-327">För att skapa ett Azure AD-program, skapa nyckelvalvet, eller konfigurera en befintlig nyckelvalv och aktivera kryptering, finns det [PowerShell-skript för Azure Disk Encryption nödvändiga](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).</span><span class="sxs-lookup"><span data-stu-id="531f3-327">To create an Azure AD application, create a key vault, or set up an existing key vault and enable encryption, see the [Azure Disk Encryption prerequisite PowerShell script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).</span></span>
* <span data-ttu-id="531f3-328">Om du vill konfigurera krav för disk-kryptering med hjälp av Azure CLI, se [Bash skriptet](https://github.com/ejarvi/ade-cli-getting-started).</span><span class="sxs-lookup"><span data-stu-id="531f3-328">To configure disk-encryption prerequisites using the Azure CLI, see [this Bash script](https://github.com/ejarvi/ade-cli-getting-started).</span></span>
* <span data-ttu-id="531f3-329">Om du vill använda Azure Backup-tjänsten för att säkerhetskopiera och återställa krypterade VMs när kryptering är aktiverat med Azure Disk Encryption, kryptera dina virtuella datorer med hjälp av Azure Disk Encryption key konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="531f3-329">To use the Azure Backup service to back up and restore encrypted VMs, when encryption is enabled with Azure Disk Encryption, encrypt your VMs by using the Azure Disk Encryption key configuration.</span></span> <span data-ttu-id="531f3-330">Backup-tjänsten har stöd för virtuella datorer som krypteras med endast KEK-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="531f3-330">The Backup service supports VMs that are encrypted using KEK configuration only.</span></span> <span data-ttu-id="531f3-331">Se [säkerhetskopiera och återställa krypterade virtuella datorer med Azure Backup kryptering](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).</span><span class="sxs-lookup"><span data-stu-id="531f3-331">See [How to back up and restore encrypted virtual machines with Azure Backup  encryption](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).</span></span>

* <span data-ttu-id="531f3-332">När du krypterar en Linux OS-volym, Observera att en VM-omstart krävs för närvarande i slutet av processen.</span><span class="sxs-lookup"><span data-stu-id="531f3-332">When encrypting a Linux OS volume, note that a VM restart is currently required at the end of the process.</span></span> <span data-ttu-id="531f3-333">Detta kan göras via portalen, powershell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="531f3-333">This can be done via the portal, powershell, or CLI.</span></span>   <span data-ttu-id="531f3-334">Om du vill spåra förloppet för kryptering, regelbundet avsöka det statusmeddelande som returneras av Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus.</span><span class="sxs-lookup"><span data-stu-id="531f3-334">To track the progress of encryption, periodically poll the status message returned by Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus.</span></span>  <span data-ttu-id="531f3-335">När kryptering är klar, det statusmeddelande som returneras av det här kommandot visar detta.</span><span class="sxs-lookup"><span data-stu-id="531f3-335">Once encryption is complete, the the status message returned by this command will indicate this.</span></span>  <span data-ttu-id="531f3-336">Till exempel ”ProgressMessage: OS-disken har krypterats, starta om den virtuella datorn” nu kan den virtuella datorn startas om och användas.</span><span class="sxs-lookup"><span data-stu-id="531f3-336">For example, "ProgressMessage: OS disk successfully encrypted, please reboot the VM"  At this point the VM can be restarted and used.</span></span>  

* <span data-ttu-id="531f3-337">Azure Disk Encryption för Linux kräver datadiskar för att ha ett anslutet filsystem i Linux före kryptering</span><span class="sxs-lookup"><span data-stu-id="531f3-337">Azure Disk Encryption for Linux requires data disks to have a mounted file system in Linux prior to encryption</span></span>

* <span data-ttu-id="531f3-338">Rekursivt monterade diskar inte stöds av Azure Disk Encryption för Linux data.</span><span class="sxs-lookup"><span data-stu-id="531f3-338">Recursively mounted data disks are not supported by the Azure Disk Encryption for Linux.</span></span> <span data-ttu-id="531f3-339">Om exempelvis målsystemet har monterats en disk i foo-fältet och sedan en annan på /foo/bar/baz, kryptering av /foo/bar/baz lyckas, men misslyckas kryptering av/foo/stapel.</span><span class="sxs-lookup"><span data-stu-id="531f3-339">For example, if the target system has mounted a disk on /foo/bar and then another on /foo/bar/baz, the encryption of /foo/bar/baz will succeed, but encryption of /foo/bar will fail.</span></span> 

* <span data-ttu-id="531f3-340">Azure Disk Encryption stöds bara på Azure stöds galleriavbildningar som uppfyller dessa krav.</span><span class="sxs-lookup"><span data-stu-id="531f3-340">Azure Disk Encryption is only supported on Azure gallery supported images that meet the aforementioned prerequisites.</span></span> <span data-ttu-id="531f3-341">Kunden anpassade avbildningar stöds inte på grund av anpassade partitionsscheman och processen beteenden som kan finnas på dessa bilder.</span><span class="sxs-lookup"><span data-stu-id="531f3-341">Customer custom images are not supported due to custom partition schemes and process behaviors that may exist on these images.</span></span> <span data-ttu-id="531f3-342">Dessutom Bildbaserad även galleriet Virtuella datorer som ursprungligen uppfyllda förutsättningar men har ändrats efter att skapa kan vara inkompatibla.</span><span class="sxs-lookup"><span data-stu-id="531f3-342">Further, even gallery image based VM's that initially met prerequisites but have been modified after creation may be incompatible.</span></span>  <span data-ttu-id="531f3-343">För som är därför den föreslagna proceduren för att kryptera en Linux VM att starta från en avbildning av ren galleriet, kryptera den virtuella datorn och sedan lägga till anpassade programvara eller data i den virtuella datorn efter behov.</span><span class="sxs-lookup"><span data-stu-id="531f3-343">For that reason, the suggested procedure for encrypting a Linux VM is to start from a clean gallery image, encrypt the VM, and then add custom software or data to the VM as needed.</span></span>  

> [!NOTE]
> <span data-ttu-id="531f3-344">Säkerhetskopiering och återställning av krypterade virtuella datorer stöds bara för virtuella datorer som är krypterade med KEK konfiguration.</span><span class="sxs-lookup"><span data-stu-id="531f3-344">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with the KEK configuration.</span></span> <span data-ttu-id="531f3-345">Den stöds inte på virtuella datorer som är krypterade utan KEK.</span><span class="sxs-lookup"><span data-stu-id="531f3-345">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="531f3-346">KEK är en valfri parameter som möjliggör VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-346">KEK is an optional parameter that enables VM.</span></span>

#### <a name="set-up-the-azure-ad-application-in-azure-active-directory"></a><span data-ttu-id="531f3-347">Konfigurera Azure AD-program i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="531f3-347">Set up the Azure AD application in Azure Active Directory</span></span>
<span data-ttu-id="531f3-348">När du behöver kryptering har aktiverats på en aktiv virtuell dator i Azure, Azure Disk Encryption genererar och skriver krypteringsnycklarna till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-348">When you need encryption to be enabled on a running VM in Azure, Azure Disk Encryption generates and writes the encryption keys to your key vault.</span></span> <span data-ttu-id="531f3-349">Hantera krypteringsnycklar i nyckelvalvet kräver Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="531f3-349">Managing encryption keys in your key vault requires Azure AD authentication.</span></span>

<span data-ttu-id="531f3-350">Skapa ett Azure AD-program för det här ändamålet.</span><span class="sxs-lookup"><span data-stu-id="531f3-350">For this purpose, create an Azure AD application.</span></span> <span data-ttu-id="531f3-351">Du kan hitta detaljerade anvisningar för att registrera ett program i avsnittet ”hämta en identitet för programmet” i blogginlägget [Azure Key Vault - steg-för-steg](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="531f3-351">You can find detailed steps for registering an application in the “Get an Identity for the Application” section of the blog post [Azure Key Vault - Step by Step](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span></span> <span data-ttu-id="531f3-352">Det här exemplet innehåller också ett antal användbara exempel för att installera och konfigurera nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-352">This post also contains a number of helpful examples for setting up and configuring your key vault.</span></span> <span data-ttu-id="531f3-353">Du kan använda client secret-baserad autentisering eller klientautentisering certifikatbaserad Azure AD för autentisering.</span><span class="sxs-lookup"><span data-stu-id="531f3-353">For authentication purposes, you can use either client secret-based authentication or client certificate-based Azure AD authentication.</span></span>

#### <a name="client-secret-based-authentication-for-azure-ad"></a><span data-ttu-id="531f3-354">Klienten hemlighet-baserad autentisering för Azure AD</span><span class="sxs-lookup"><span data-stu-id="531f3-354">Client secret-based authentication for Azure AD</span></span>
<span data-ttu-id="531f3-355">Avsnitten som följer kan hjälpa dig att konfigurera en hemlighet-baserade klientautentisering för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="531f3-355">The sections that follow can help you configure a client secret-based authentication for Azure AD.</span></span>

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a><span data-ttu-id="531f3-356">Skapa ett Azure AD-program med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="531f3-356">Create an Azure AD application by using Azure PowerShell</span></span>
<span data-ttu-id="531f3-357">Använd följande PowerShell-cmdlet för att skapa en Azure AD-program:</span><span class="sxs-lookup"><span data-stu-id="531f3-357">Use the following PowerShell cmdlet to create an Azure AD application:</span></span>

    $aadClientSecret = "yourSecret"
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> <span data-ttu-id="531f3-358">$azureAdApplication.ApplicationId är Azure AD-ClientID och $aadClientSecret är klienthemlighet som du ska använda för att aktivera Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="531f3-358">$azureAdApplication.ApplicationId is the Azure AD ClientID and $aadClientSecret is the client secret that you should use later to enable Azure Disk Encryption.</span></span> <span data-ttu-id="531f3-359">Skydda hemligheten för Azure AD-klient på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="531f3-359">Safeguard the Azure AD client secret appropriately.</span></span>

##### <a name="setting-up-the-azure-ad-client-id-and-secret-from-the-azure-classic-portal"></a><span data-ttu-id="531f3-360">Konfigurera Azure AD-klient-ID och Hemlig från den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="531f3-360">Setting up the Azure AD client ID and secret from the Azure classic portal</span></span>
<span data-ttu-id="531f3-361">Du kan också ställa in din Azure AD-klient-ID och Hemlig med hjälp av den [klassiska Azure-portalen]( https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="531f3-361">You can also set up your Azure AD client ID and secret by using the [Azure classic portal]( https://manage.windowsazure.com).</span></span> <span data-ttu-id="531f3-362">Om du vill utföra den här uppgiften gör du följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-362">To perform this task, do the following:</span></span>

1. <span data-ttu-id="531f3-363">Klicka på den **Active Directory** fliken.</span><span class="sxs-lookup"><span data-stu-id="531f3-363">Click the **Active Directory** tab.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. <span data-ttu-id="531f3-365">Klicka på **Lägg till program**, och skriv sedan namnet på programmet.</span><span class="sxs-lookup"><span data-stu-id="531f3-365">Click **Add Application**, and then type the application name.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. <span data-ttu-id="531f3-367">Klicka på pilen och sedan konfigurera egenskaper för program.</span><span class="sxs-lookup"><span data-stu-id="531f3-367">Click the arrow button, and then configure the application properties.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. <span data-ttu-id="531f3-369">Klicka på kryssmarkeringen i det nedre vänstra hörnet ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="531f3-369">Click the check mark in the lower left corner to finish.</span></span> <span data-ttu-id="531f3-370">Konfigurationssidan för programmet, och Azure AD-klient-ID visas längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="531f3-370">The application configuration page appears, and the Azure AD client ID is displayed at the bottom of the page.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. <span data-ttu-id="531f3-372">Spara hemligheten som Azure AD-klienten genom att klicka på den **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="531f3-372">Save the Azure AD client secret by clicking the **Save** button.</span></span> <span data-ttu-id="531f3-373">Observera att Azure AD-klienthemlighet i textrutan nycklar.</span><span class="sxs-lookup"><span data-stu-id="531f3-373">Note the Azure AD client secret in the keys text box.</span></span> <span data-ttu-id="531f3-374">Skydda på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="531f3-374">Safeguard it appropriately.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig7.png)

 > [!NOTE]
 > <span data-ttu-id="531f3-376">Föregående flödet stöds inte på den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="531f3-376">The preceding flow is not supported on the Azure classic portal.</span></span>

##### <a name="use-an-existing-application"></a><span data-ttu-id="531f3-377">Använda ett befintligt program</span><span class="sxs-lookup"><span data-stu-id="531f3-377">Use an existing application</span></span>
<span data-ttu-id="531f3-378">För att köra följande kommandon, hämtar och använder den [Azure AD PowerShell-modulen](https://technet.microsoft.com/library/jj151815.aspx).</span><span class="sxs-lookup"><span data-stu-id="531f3-378">To execute the following commands, obtain and use the [Azure AD PowerShell module](https://technet.microsoft.com/library/jj151815.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-379">Följande kommandon måste köras från en ny PowerShell-fönstret.</span><span class="sxs-lookup"><span data-stu-id="531f3-379">The following commands must be executed from a new PowerShell window.</span></span> <span data-ttu-id="531f3-380">Använd inte Azure PowerShell eller Azure Resource Manager-fönstret för att köra kommandona.</span><span class="sxs-lookup"><span data-stu-id="531f3-380">Do not use Azure PowerShell or the Azure Resource Manager window to execute the commands.</span></span> <span data-ttu-id="531f3-381">Vi rekommenderar den här metoden eftersom dessa cmdlets finns i MSOnline modul eller Azure AD PowerShell.</span><span class="sxs-lookup"><span data-stu-id="531f3-381">We recommend this approach because these cmdlets are in the MSOnline module or Azure AD PowerShell.</span></span>

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a><span data-ttu-id="531f3-382">Certifikatbaserad autentisering för Azure AD</span><span class="sxs-lookup"><span data-stu-id="531f3-382">Certificate-based authentication for Azure AD</span></span>
> [!NOTE]
> <span data-ttu-id="531f3-383">Azure AD certifikatbaserad autentisering stöds för närvarande inte på Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="531f3-383">Azure AD certificate-based authentication is currently not supported on Linux VMs.</span></span>

<span data-ttu-id="531f3-384">I de följande avsnitten visar hur du konfigurerar en certifikatbaserad autentisering för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="531f3-384">The sections that follow show how to configure a certificate-based authentication for Azure AD.</span></span>

##### <a name="create-an-azure-ad-application"></a><span data-ttu-id="531f3-385">Skapa en Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="531f3-385">Create an Azure AD application</span></span>
<span data-ttu-id="531f3-386">Kör följande PowerShell-cmdlets för att skapa en Azure AD-program:</span><span class="sxs-lookup"><span data-stu-id="531f3-386">To create an Azure AD application, execute the following PowerShell cmdlets:</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-387">Ersätt följande `yourpassword` sträng med säkra lösenord och skydda lösenordet.</span><span class="sxs-lookup"><span data-stu-id="531f3-387">Replace the following `yourpassword` string with your secure password, and safeguard the password.</span></span>

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

<span data-ttu-id="531f3-388">När du har slutfört det här steget ladda upp PFX-filen till nyckelvalvet och aktivera åtkomstprincipen som krävs för att distribuera certifikatet till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="531f3-388">After you finish this step, upload a PFX file to your key vault and enable the access policy needed to deploy that certificate to a VM.</span></span>

##### <a name="use-an-existing-azure-ad-application"></a><span data-ttu-id="531f3-389">Använda ett befintligt Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="531f3-389">Use an existing Azure AD application</span></span>
<span data-ttu-id="531f3-390">Om du konfigurerar certifikatbaserad autentisering för ett befintligt program, använder du PowerShell-cmdlets som visas här.</span><span class="sxs-lookup"><span data-stu-id="531f3-390">If you are configuring certificate-based authentication for an existing application, use the PowerShell cmdlets shown here.</span></span> <span data-ttu-id="531f3-391">Glöm inte att köra dem från ett nytt PowerShell-fönster.</span><span class="sxs-lookup"><span data-stu-id="531f3-391">Be sure to execute them from a new PowerShell window.</span></span>

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

<span data-ttu-id="531f3-392">När du har slutfört det här steget ladda upp PFX-filen till nyckelvalvet och aktivera åtkomstprincipen som behövs för att distribuera certifikatet till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="531f3-392">After you finish this step, upload a PFX file to your key vault and enable the access policy that's needed to deploy the certificate to a VM.</span></span>

##### <a name="upload-a-pfx-file-to-your-key-vault"></a><span data-ttu-id="531f3-393">Ladda upp PFX-filen till nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="531f3-393">Upload a PFX file to your key vault</span></span>
<span data-ttu-id="531f3-394">En detaljerad förklaring av den här processen finns [den officiella Azure nyckeln valvet Teamblogg](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx).</span><span class="sxs-lookup"><span data-stu-id="531f3-394">For a detailed explanation of this process, see [The Official Azure Key Vault Team Blog](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx).</span></span> <span data-ttu-id="531f3-395">Följande PowerShell-cmdlets är allt du behöver för uppgiften.</span><span class="sxs-lookup"><span data-stu-id="531f3-395">However, the following PowerShell cmdlets are all you need for the task.</span></span> <span data-ttu-id="531f3-396">Glöm inte att köra dem från Azure PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="531f3-396">Be sure to execute them from Azure PowerShell console.</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-397">Ersätt följande `yourpassword` sträng med säkra lösenord och skydda lösenordet.</span><span class="sxs-lookup"><span data-stu-id="531f3-397">Replace the following `yourpassword` string with your secure password, and safeguard the password.</span></span>

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

##### <a name="deploy-a-certificate-in-your-key-vault-to-an-existing-vm"></a><span data-ttu-id="531f3-398">Distribuera ett certifikat i nyckelvalvet till en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="531f3-398">Deploy a certificate in your key vault to an existing VM</span></span>
<span data-ttu-id="531f3-399">När du är klar med att ladda upp PFX distribuera ett certifikat i nyckelvalvet till en befintlig virtuell dator med följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-399">After you finish uploading the PFX, deploy a certificate in the key vault to an existing VM with the following:</span></span>
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

#### <a name="set-up-the-key-vault-access-policy-for-the-azure-ad-application"></a><span data-ttu-id="531f3-400">Ställ in nyckelvalv åtkomstprincip för Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="531f3-400">Set up the key vault access policy for the Azure AD application</span></span>
<span data-ttu-id="531f3-401">Azure AD-program behöver behörighet att komma åt nycklar och hemligheter i valvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-401">Your Azure AD application needs rights to access the keys or secrets in the vault.</span></span> <span data-ttu-id="531f3-402">Använd den [ `Set-AzureKeyVaultAccessPolicy` ](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) för att bevilja behörighet till programmet, med klient-ID (som skapades när programmet registrerades) som den _– ServicePrincipalName_ parametervärdet.</span><span class="sxs-lookup"><span data-stu-id="531f3-402">Use the [`Set-AzureKeyVaultAccessPolicy`](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) cmdlet to grant permissions to the application, using the client ID (which was generated when the application was registered) as the _–ServicePrincipalName_ parameter value.</span></span> <span data-ttu-id="531f3-403">Mer information finns i bloggposten [Azure Key Vault - steg-för-steg](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="531f3-403">To learn more, see the blog post [Azure Key Vault - Step by Step](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span></span> <span data-ttu-id="531f3-404">Här är ett exempel på hur du utför den här uppgiften via PowerShell:</span><span class="sxs-lookup"><span data-stu-id="531f3-404">Here is an example of how to perform this task via PowerShell:</span></span>

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> <span data-ttu-id="531f3-405">Azure Disk Encryption måste du konfigurera följande principer för åtkomst till din Azure AD-klientprogram: _WrapKey_ och _ange_ behörigheter.</span><span class="sxs-lookup"><span data-stu-id="531f3-405">Azure Disk Encryption requires you to configure the following access policies to your Azure AD client application: _WrapKey_ and _Set_ permissions.</span></span>

## <a name="terminology"></a><span data-ttu-id="531f3-406">Terminologi</span><span class="sxs-lookup"><span data-stu-id="531f3-406">Terminology</span></span>
<span data-ttu-id="531f3-407">Använd tabellen nedan terminologi för att förstå några av de vanliga termer som används av den här tekniken:</span><span class="sxs-lookup"><span data-stu-id="531f3-407">To understand some of the common terms used by this technology, use the following terminology table:</span></span>

| <span data-ttu-id="531f3-408">Terminologi</span><span class="sxs-lookup"><span data-stu-id="531f3-408">Terminology</span></span> | <span data-ttu-id="531f3-409">Definition</span><span class="sxs-lookup"><span data-stu-id="531f3-409">Definition</span></span> |
| --- | --- |
| <span data-ttu-id="531f3-410">Azure AD</span><span class="sxs-lookup"><span data-stu-id="531f3-410">Azure AD</span></span> | <span data-ttu-id="531f3-411">Azure AD [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="531f3-411">Azure AD is [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).</span></span> <span data-ttu-id="531f3-412">Azure AD-kontot är en förutsättning för autentisering, lagra och hämta hemligheter från en nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-412">An Azure AD account is a prerequisite for authenticating, storing, and retrieving secrets from a key vault.</span></span> |
| <span data-ttu-id="531f3-413">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="531f3-413">Azure Key Vault</span></span> | <span data-ttu-id="531f3-414">Key Vault är en kryptografisk, key management-tjänst som baseras på FIPS Federal Information Processing Standards validerade maskinvarusäkerhetsmoduler, som bidrar till att skydda din kryptografiska nycklar och hemligheter känslig.</span><span class="sxs-lookup"><span data-stu-id="531f3-414">Key Vault is a cryptographic, key management service that's based on Federal Information Processing Standards (FIPS)-validated hardware security modules, which help safeguard your cryptographic keys and sensitive secrets.</span></span> <span data-ttu-id="531f3-415">Mer information finns i [Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="531f3-415">For more information, see [Key Vault](https://azure.microsoft.com/services/key-vault/) documentation.</span></span> |
| <span data-ttu-id="531f3-416">ARM</span><span class="sxs-lookup"><span data-stu-id="531f3-416">ARM</span></span> | <span data-ttu-id="531f3-417">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="531f3-417">Azure Resource Manager</span></span> |
| <span data-ttu-id="531f3-418">BitLocker</span><span class="sxs-lookup"><span data-stu-id="531f3-418">BitLocker</span></span> |<span data-ttu-id="531f3-419">[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) är en branschstandard identifieras Windows volym krypteringsteknik som används för att aktivera kryptering på Windows IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-419">[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) is an industry-recognized Windows volume encryption technology that's used to enable disk encryption on Windows IaaS VMs.</span></span> |
| <span data-ttu-id="531f3-420">BEK</span><span class="sxs-lookup"><span data-stu-id="531f3-420">BEK</span></span> | <span data-ttu-id="531f3-421">Krypteringsnycklarna i BitLocker används för att kryptera OS startvolymen och datavolymer.</span><span class="sxs-lookup"><span data-stu-id="531f3-421">BitLocker encryption keys are used to encrypt the OS boot volume and data volumes.</span></span> <span data-ttu-id="531f3-422">BitLocker-nycklar skyddas i ett nyckelvalv som hemligheter.</span><span class="sxs-lookup"><span data-stu-id="531f3-422">The BitLocker keys are safeguarded in a key vault as secrets.</span></span> |
| <span data-ttu-id="531f3-423">CLI</span><span class="sxs-lookup"><span data-stu-id="531f3-423">CLI</span></span> | <span data-ttu-id="531f3-424">Se [Azure-kommandoradsgränssnittet](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="531f3-424">See [Azure command-line interface](../cli-install-nodejs.md).</span></span> |
| <span data-ttu-id="531f3-425">DM-Crypt</span><span class="sxs-lookup"><span data-stu-id="531f3-425">DM-Crypt</span></span> |<span data-ttu-id="531f3-426">[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) är Linux-baserade och transparent diskkryptering undersystemet som används för att aktivera kryptering på Linux IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-426">[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) is the Linux-based, transparent disk-encryption subsystem that's used to enable disk encryption on Linux IaaS VMs.</span></span> |
| <span data-ttu-id="531f3-427">KEK</span><span class="sxs-lookup"><span data-stu-id="531f3-427">KEK</span></span> | <span data-ttu-id="531f3-428">Viktiga krypteringsnyckeln är den asymmetriska nyckeln (RSA 2048) som du kan använda för att skydda eller omsluter hemligheten.</span><span class="sxs-lookup"><span data-stu-id="531f3-428">Key encryption key is the asymmetric key (RSA 2048) that you can use to protect or wrap the secret.</span></span> <span data-ttu-id="531f3-429">Du kan ange en säkerhets-och maskinvara moduler (HSM)-skyddad nyckel eller programvaruskyddad nyckel.</span><span class="sxs-lookup"><span data-stu-id="531f3-429">You can provide a hardware security modules (HSM)-protected key or software-protected key.</span></span> <span data-ttu-id="531f3-430">Mer information finns i [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="531f3-430">For more details, see [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentation.</span></span> |
| <span data-ttu-id="531f3-431">PS-cmdlets</span><span class="sxs-lookup"><span data-stu-id="531f3-431">PS cmdlets</span></span> | <span data-ttu-id="531f3-432">Se [Azure PowerShell-cmdlets](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="531f3-432">See [Azure PowerShell cmdlets](/powershell/azure/overview).</span></span> |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a><span data-ttu-id="531f3-433">Installera och konfigurera nyckelvalvet för Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="531f3-433">Set up and configure your key vault for Azure Disk Encryption</span></span>
<span data-ttu-id="531f3-434">Azure Disk Encryption hjälper till att skydda disk-kryptering nycklar och hemligheter i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-434">Azure Disk Encryption helps safeguard the disk-encryption keys and secrets in your key vault.</span></span> <span data-ttu-id="531f3-435">Slutför stegen i följande avsnitt om du vill konfigurera nyckelvalvet för Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="531f3-435">To set up your key vault for Azure Disk Encryption, complete the steps in each of the following sections.</span></span>

#### <a name="create-a-key-vault"></a><span data-ttu-id="531f3-436">Skapa ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="531f3-436">Create a key vault</span></span>
<span data-ttu-id="531f3-437">Om du vill skapa ett nyckelvalv med någon av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="531f3-437">To create a key vault, use one of the following options:</span></span>

* [<span data-ttu-id="531f3-438">”101-nyckel-valvet-skapa” Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="531f3-438">"101-Key-Vault-Create" Resource Manager template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
* [<span data-ttu-id="531f3-439">Azure PowerShell nyckelvalv-cmdlets</span><span class="sxs-lookup"><span data-stu-id="531f3-439">Azure PowerShell key vault cmdlets</span></span>](/powershell/module/azurerm.keyvault/#key_vault)
* <span data-ttu-id="531f3-440">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="531f3-440">Azure Resource Manager</span></span>
* <span data-ttu-id="531f3-441">Så här [Secure nyckelvalvet](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)</span><span class="sxs-lookup"><span data-stu-id="531f3-441">How to [Secure your key vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-442">Om du redan har installerat en nyckelvalv för din prenumeration, vidare till nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="531f3-442">If you have already set up a key vault for your subscription, skip to the next section.</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a><span data-ttu-id="531f3-444">Ställa in en krypteringsnyckel nyckel (valfritt)</span><span class="sxs-lookup"><span data-stu-id="531f3-444">Set up a key encryption key (optional)</span></span>
<span data-ttu-id="531f3-445">Om du vill använda en KEK för ett extra säkerhetslager för krypteringsnycklar BitLocker måste du lägga till en KEK i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-445">If you want to use a KEK for an additional layer of security for the BitLocker encryption keys, add a KEK to your key vault.</span></span> <span data-ttu-id="531f3-446">Använd den [ `Add-AzureKeyVaultKey` ](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) för att skapa en nyckel för kryptering i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-446">Use the [`Add-AzureKeyVaultKey`](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet to create a key encryption key in the key vault.</span></span> <span data-ttu-id="531f3-447">Du kan också importera en KEK från din lokala nyckelhantering HSM.</span><span class="sxs-lookup"><span data-stu-id="531f3-447">You can also import a KEK from your on-premises key management HSM.</span></span> <span data-ttu-id="531f3-448">Mer information finns i [Key Vault dokumentationen](https://azure.microsoft.com/documentation/services/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="531f3-448">For more details, see [Key Vault Documentation](https://azure.microsoft.com/documentation/services/key-vault/).</span></span>

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

<span data-ttu-id="531f3-449">Du kan lägga till KEK genom att gå till Azure Resource Manager eller genom att använda nyckelvalv-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="531f3-449">You can add the KEK by going to Azure Resource Manager or by using your key vault interface.</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a><span data-ttu-id="531f3-451">Ange behörigheter för nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="531f3-451">Set key vault permissions</span></span>
<span data-ttu-id="531f3-452">Azure-plattformen behöver åtkomst till krypteringsnycklarna eller hemligheter i nyckelvalvet att göra dem tillgängliga för den virtuella datorn för start och dekryptera volymerna.</span><span class="sxs-lookup"><span data-stu-id="531f3-452">The Azure platform needs access to the encryption keys or secrets in your key vault to make them available to the VM for booting and decrypting the volumes.</span></span> <span data-ttu-id="531f3-453">Om du vill tilldela behörigheter till Azure-plattformen, ange den **EnabledForDiskEncryption** egenskap i nyckeln valvet med hjälp av PowerShell-cmdleten nyckelvalv:</span><span class="sxs-lookup"><span data-stu-id="531f3-453">To grant permissions to the Azure platform, set the **EnabledForDiskEncryption** property in the key vault by using the key vault PowerShell cmdlet:</span></span>

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

<span data-ttu-id="531f3-454">Du kan också ange den **EnabledForDiskEncryption** egenskapen genom att besöka den [resursutforskaren Azure](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="531f3-454">You can also set the **EnabledForDiskEncryption** property by visiting the [Azure Resource Explorer](https://resources.azure.com).</span></span>

<span data-ttu-id="531f3-455">Som tidigare nämnts kan du ange den **EnabledForDiskEncryption** -egenskapen i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-455">As mentioned earlier, you must set the **EnabledForDiskEncryption** property on your key vault.</span></span> <span data-ttu-id="531f3-456">Annars misslyckas distributionen.</span><span class="sxs-lookup"><span data-stu-id="531f3-456">Otherwise, the deployment will fail.</span></span>

<span data-ttu-id="531f3-457">Du kan konfigurera principer för ditt Azure AD-program från nyckelvalvet-gränssnitt som visas här:</span><span class="sxs-lookup"><span data-stu-id="531f3-457">You can set up access policies for your Azure AD application from the key vault interface, as shown here:</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

<span data-ttu-id="531f3-460">På den **avancerade åtkomstprinciper** och se till att nyckelvalvet är aktiverat för Azure Disk Encryption:</span><span class="sxs-lookup"><span data-stu-id="531f3-460">On the **Advanced access policies** tab, make sure that your key vault is enabled for Azure Disk Encryption:</span></span>

![Azure key vault](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a><span data-ttu-id="531f3-462">Distributionsscenarier för disk-kryptering och användarupplevelser</span><span class="sxs-lookup"><span data-stu-id="531f3-462">Disk-encryption deployment scenarios and user experiences</span></span>
<span data-ttu-id="531f3-463">Du kan aktivera många scenarier för disk-kryptering och stegen kan variera beroende på scenario.</span><span class="sxs-lookup"><span data-stu-id="531f3-463">You can enable many disk-encryption scenarios, and the steps may vary according to the scenario.</span></span> <span data-ttu-id="531f3-464">Följande avsnitt beskriver scenarier i detalj.</span><span class="sxs-lookup"><span data-stu-id="531f3-464">The following sections cover the scenarios in greater detail.</span></span>

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-the-marketplace"></a><span data-ttu-id="531f3-465">Aktivera kryptering på den nya virtuella IaaS-datorer som skapas från Marketplace</span><span class="sxs-lookup"><span data-stu-id="531f3-465">Enable encryption on new IaaS VMs that are created from the Marketplace</span></span>
<span data-ttu-id="531f3-466">Du kan aktivera kryptering på nya Windows IaaS-VM från Marketplace i Azure med hjälp av den [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).</span><span class="sxs-lookup"><span data-stu-id="531f3-466">You can enable disk encryption on new IaaS Windows VM from the Marketplace in Azure by using the  [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).</span></span>

1. <span data-ttu-id="531f3-467">Klicka på mallen Azure Snabbkurs **till Azure**, ange konfigurationen för kryptering på den **parametrar** bladet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="531f3-467">On the Azure quick-start template, click **Deploy to Azure**, enter the encryption configuration on the **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="531f3-468">Välj prenumerationen, resursgruppen, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** att aktivera kryptering på en ny IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-468">Select the subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** to enable encryption on a new IaaS VM.</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-469">Den här mallen skapar en ny krypterade Windows virtuell dator som använder Windows Server 2012 galleriet avbildningen.</span><span class="sxs-lookup"><span data-stu-id="531f3-469">This template creates a new encrypted Windows VM that uses the Windows Server 2012 gallery image.</span></span>

<span data-ttu-id="531f3-470">Du kan aktivera kryptering på en ny IaaS RedHat Linux 7.2 virtuell dator med en 200 GB RAID-0-matris med den här [Resource Manager-mall](https://aka.ms/fde-rhel).</span><span class="sxs-lookup"><span data-stu-id="531f3-470">You can enable disk encryption on a new IaaS RedHat Linux 7.2 VM with a 200-GB RAID-0 array by using this [Resource Manager template](https://aka.ms/fde-rhel).</span></span> <span data-ttu-id="531f3-471">När du distribuerar mallen kontrollerar du status för VM-kryptering med hjälp av den `Get-AzureRmVmDiskEncryptionStatus` cmdlet, enligt beskrivningen i [kryptera OS enhet på en körande Linux VM](#encrypting-os-drive-on-a-running-linux-vm).</span><span class="sxs-lookup"><span data-stu-id="531f3-471">After you deploy the template, verify the VM encryption status by using the `Get-AzureRmVmDiskEncryptionStatus` cmdlet, as described in [Encrypting OS drive on a running Linux VM](#encrypting-os-drive-on-a-running-linux-vm).</span></span> <span data-ttu-id="531f3-472">När datorn returnerar statusvärdet _VMRestartPending_, starta om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-472">When the machine returns a status of _VMRestartPending_, restart the VM.</span></span>

<span data-ttu-id="531f3-473">I följande tabell visas mallparametrar Resource Manager för nya virtuella datorer från Marketplace-scenario med hjälp av Azure AD-klient-ID:</span><span class="sxs-lookup"><span data-stu-id="531f3-473">The following table lists the Resource Manager template parameters for new VMs from the Marketplace scenario using Azure AD client ID:</span></span>

| <span data-ttu-id="531f3-474">Parameter</span><span class="sxs-lookup"><span data-stu-id="531f3-474">Parameter</span></span> | <span data-ttu-id="531f3-475">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="531f3-475">Description</span></span> |
| --- | --- |
| <span data-ttu-id="531f3-476">adminUserName</span><span class="sxs-lookup"><span data-stu-id="531f3-476">adminUserName</span></span> | <span data-ttu-id="531f3-477">Admin användarnamn för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-477">Admin user name for the virtual machine.</span></span> |
| <span data-ttu-id="531f3-478">adminPassword</span><span class="sxs-lookup"><span data-stu-id="531f3-478">adminPassword</span></span> | <span data-ttu-id="531f3-479">Administratörslösenord för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-479">Admin user password for the virtual machine.</span></span> |
| <span data-ttu-id="531f3-480">newStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="531f3-480">newStorageAccountName</span></span> | <span data-ttu-id="531f3-481">Namnet på lagringskontot för att lagra Operativsystemet och virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="531f3-481">Name of the storage account to store OS and data VHDs.</span></span> |
| <span data-ttu-id="531f3-482">vmSize</span><span class="sxs-lookup"><span data-stu-id="531f3-482">vmSize</span></span> | <span data-ttu-id="531f3-483">Storleken på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-483">Size of the VM.</span></span> <span data-ttu-id="531f3-484">För närvarande stöds endast Standard A, D och G serien.</span><span class="sxs-lookup"><span data-stu-id="531f3-484">Currently, only Standard A, D, and G series are supported.</span></span> |
| <span data-ttu-id="531f3-485">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="531f3-485">virtualNetworkName</span></span> | <span data-ttu-id="531f3-486">Namnet på den VNet som VM NIC ska tillhöra.</span><span class="sxs-lookup"><span data-stu-id="531f3-486">Name of the VNet that the VM NIC should belong to.</span></span> |
| <span data-ttu-id="531f3-487">subnetName</span><span class="sxs-lookup"><span data-stu-id="531f3-487">subnetName</span></span> | <span data-ttu-id="531f3-488">Namn på undernät i VNet VM NIC ska tillhöra.</span><span class="sxs-lookup"><span data-stu-id="531f3-488">Name of the subnet in the VNet that the VM NIC should belong to.</span></span> |
| <span data-ttu-id="531f3-489">AADClientID</span><span class="sxs-lookup"><span data-stu-id="531f3-489">AADClientID</span></span> | <span data-ttu-id="531f3-490">Klient-ID för Azure AD-program som har behörighet att skriva hemligheter i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-490">Client ID of the Azure AD application that has permissions to write secrets to your key vault.</span></span> |
| <span data-ttu-id="531f3-491">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="531f3-491">AADClientSecret</span></span> | <span data-ttu-id="531f3-492">Klienthemlighet för Azure AD-program som har behörighet att skriva hemligheter i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-492">Client secret of the Azure AD application that has permissions to write secrets to your key vault.</span></span> |
| <span data-ttu-id="531f3-493">keyVaultURL</span><span class="sxs-lookup"><span data-stu-id="531f3-493">keyVaultURL</span></span> | <span data-ttu-id="531f3-494">URL för nyckelvalvet som BitLocker-nyckel ska överföras till.</span><span class="sxs-lookup"><span data-stu-id="531f3-494">URL of the key vault that the BitLocker key should be uploaded to.</span></span> <span data-ttu-id="531f3-495">Du kan hämta den med hjälp av cmdleten `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`.</span><span class="sxs-lookup"><span data-stu-id="531f3-495">You can get it by using the cmdlet `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`.</span></span> |
| <span data-ttu-id="531f3-496">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="531f3-496">keyEncryptionKeyURL</span></span> | <span data-ttu-id="531f3-497">URL till den viktiga krypteringsnyckel som används för att kryptera genererade BitLocker-nyckel (valfritt).</span><span class="sxs-lookup"><span data-stu-id="531f3-497">URL of the key encryption key that's used to encrypt the generated BitLocker key (optional).</span></span> |
| <span data-ttu-id="531f3-498">keyVaultResourceGroup</span><span class="sxs-lookup"><span data-stu-id="531f3-498">keyVaultResourceGroup</span></span> | <span data-ttu-id="531f3-499">Resursgruppen för nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-499">Resource group of the key vault.</span></span> |
| <span data-ttu-id="531f3-500">vmName</span><span class="sxs-lookup"><span data-stu-id="531f3-500">vmName</span></span> | <span data-ttu-id="531f3-501">Namnet på den virtuella datorn som krypteringsåtgärden ska utföras på.</span><span class="sxs-lookup"><span data-stu-id="531f3-501">Name of the VM that the encryption operation is to be performed on.</span></span> |

> [!NOTE]
> <span data-ttu-id="531f3-502">_KeyEncryptionKeyURL_ är en valfri parameter.</span><span class="sxs-lookup"><span data-stu-id="531f3-502">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="531f3-503">Du kan ge din egen KEK ytterligare säkerhetskontroll datakrypteringsnyckeln (lösenfrasen hemlig) i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-503">You can bring your own KEK to further safeguard the data encryption key (Passphrase secret) in your key vault.</span></span>

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a><span data-ttu-id="531f3-504">Aktivera kryptering på den nya virtuella IaaS-datorer som skapas från kund-krypterad VHD- och krypteringsnycklar</span><span class="sxs-lookup"><span data-stu-id="531f3-504">Enable encryption on new IaaS VMs that are created from customer-encrypted VHD and encryption keys</span></span>
<span data-ttu-id="531f3-505">Du kan aktivera kryptering genom att använda Resource Manager-mall, PowerShell-cmdlets eller CLI-kommandona i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="531f3-505">In this scenario, you can enable encrypting by using the Resource Manager template, PowerShell cmdlets, or CLI commands.</span></span> <span data-ttu-id="531f3-506">I följande avsnitt beskrivs i detalj Resource Manager-mall och CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="531f3-506">The following sections explain in greater detail the Resource Manager template and CLI commands.</span></span>

<span data-ttu-id="531f3-507">Följ instruktionerna från någon av dessa avsnitt för att förbereda inför krypterade avbildningar som kan användas i Azure.</span><span class="sxs-lookup"><span data-stu-id="531f3-507">Follow the instructions from one of these sections for preparing pre-encrypted images that can be used in Azure.</span></span> <span data-ttu-id="531f3-508">När avbildningen har skapats kan använda du stegen i nästa avsnitt för att skapa en krypterad Azure VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-508">After the image is created, you can use the steps in the next section to create an encrypted Azure VM.</span></span>

* [<span data-ttu-id="531f3-509">Förbereda en förkrypterade Windows VHD</span><span class="sxs-lookup"><span data-stu-id="531f3-509">Prepare a pre-encrypted Windows VHD</span></span>](#preparing-a-pre-encrypted-windows-vhd)
* [<span data-ttu-id="531f3-510">Förbereda en förkrypterade Linux VHD</span><span class="sxs-lookup"><span data-stu-id="531f3-510">Prepare a pre-encrypted Linux VHD</span></span>](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-the-resource-manager-template"></a><span data-ttu-id="531f3-511">Med hjälp av Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="531f3-511">Using the Resource Manager template</span></span>
<span data-ttu-id="531f3-512">Du kan aktivera kryptering på den kryptera virtuella Hårddisken med hjälp av den [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).</span><span class="sxs-lookup"><span data-stu-id="531f3-512">You can enable disk encryption on your encrypted VHD by using the [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).</span></span>

1. <span data-ttu-id="531f3-513">Klicka på mallen Azure Snabbkurs **till Azure**, ange konfigurationen för kryptering på den **parametrar** bladet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="531f3-513">On the Azure quick-start template, click **Deploy to Azure**, enter the encryption configuration on the **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="531f3-514">Välj prenumerationen, resursgruppen, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** att aktivera kryptering på den nya IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-514">Select the subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** to enable encryption on the new IaaS VM.</span></span>

<span data-ttu-id="531f3-515">I följande tabell visas mallparametrar Resource Manager för den kryptera virtuella Hårddisken:</span><span class="sxs-lookup"><span data-stu-id="531f3-515">The following table lists the Resource Manager template parameters for your encrypted VHD:</span></span>

| <span data-ttu-id="531f3-516">Parameter</span><span class="sxs-lookup"><span data-stu-id="531f3-516">Parameter</span></span> | <span data-ttu-id="531f3-517">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="531f3-517">Description</span></span> |
| --- | --- |
| <span data-ttu-id="531f3-518">newStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="531f3-518">newStorageAccountName</span></span> | <span data-ttu-id="531f3-519">Namnet på lagringskontot för att lagra krypterade OS VHD: N.</span><span class="sxs-lookup"><span data-stu-id="531f3-519">Name of the storage account to store the encrypted OS VHD.</span></span> <span data-ttu-id="531f3-520">Det här lagringskontot bör redan har skapats i samma resursgrupp och samma plats som den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-520">This storage account should already have been created in the same resource group and same location as the VM.</span></span> |
| <span data-ttu-id="531f3-521">osVhdUri</span><span class="sxs-lookup"><span data-stu-id="531f3-521">osVhdUri</span></span> | <span data-ttu-id="531f3-522">URI för OS-VHD från lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="531f3-522">URI of the OS VHD from the storage account.</span></span> |
| <span data-ttu-id="531f3-523">osType</span><span class="sxs-lookup"><span data-stu-id="531f3-523">osType</span></span> | <span data-ttu-id="531f3-524">OS-produkttyp (Windows-/ Linux).</span><span class="sxs-lookup"><span data-stu-id="531f3-524">OS product type (Windows/Linux).</span></span> |
| <span data-ttu-id="531f3-525">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="531f3-525">virtualNetworkName</span></span> | <span data-ttu-id="531f3-526">Namnet på den VNet som VM NIC ska tillhöra.</span><span class="sxs-lookup"><span data-stu-id="531f3-526">Name of the VNet that the VM NIC should belong to.</span></span> <span data-ttu-id="531f3-527">Namnet bör redan har skapats i samma resursgrupp och samma plats som den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-527">The name should already have been created in the same resource group and same location as the VM.</span></span> |
| <span data-ttu-id="531f3-528">subnetName</span><span class="sxs-lookup"><span data-stu-id="531f3-528">subnetName</span></span> | <span data-ttu-id="531f3-529">Namn på undernät för det virtuella nätverk som VM NIC ska tillhöra.</span><span class="sxs-lookup"><span data-stu-id="531f3-529">Name of the subnet on the VNet that the VM NIC should belong to.</span></span> |
| <span data-ttu-id="531f3-530">vmSize</span><span class="sxs-lookup"><span data-stu-id="531f3-530">vmSize</span></span> | <span data-ttu-id="531f3-531">Storleken på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-531">Size of the VM.</span></span> <span data-ttu-id="531f3-532">För närvarande stöds endast Standard A, D och G serien.</span><span class="sxs-lookup"><span data-stu-id="531f3-532">Currently, only Standard A, D, and G series are supported.</span></span> |
| <span data-ttu-id="531f3-533">keyVaultResourceID</span><span class="sxs-lookup"><span data-stu-id="531f3-533">keyVaultResourceID</span></span> | <span data-ttu-id="531f3-534">Resurs-ID som identifierar nyckelvalv resurs i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="531f3-534">The ResourceID that identifies the key vault resource in Azure Resource Manager.</span></span> <span data-ttu-id="531f3-535">Du kan hämta den med hjälp av PowerShell-cmdleten `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`.</span><span class="sxs-lookup"><span data-stu-id="531f3-535">You can get it by using the PowerShell cmdlet `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`.</span></span> |
| <span data-ttu-id="531f3-536">keyVaultSecretUrl</span><span class="sxs-lookup"><span data-stu-id="531f3-536">keyVaultSecretUrl</span></span> | <span data-ttu-id="531f3-537">URL för disk-krypteringsnyckeln som har ställts in i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-537">URL of the disk-encryption key that's set up in the key vault.</span></span> |
| <span data-ttu-id="531f3-538">keyVaultKekUrl</span><span class="sxs-lookup"><span data-stu-id="531f3-538">keyVaultKekUrl</span></span> | <span data-ttu-id="531f3-539">URL för nyckelkryptering-nyckel för att kryptera nyckeln genererade diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="531f3-539">URL of the key encryption key for encrypting the generated disk-encryption key.</span></span> |
| <span data-ttu-id="531f3-540">vmName</span><span class="sxs-lookup"><span data-stu-id="531f3-540">vmName</span></span> | <span data-ttu-id="531f3-541">Namnet på IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-541">Name of the IaaS VM.</span></span> |

#### <a name="using-powershell-cmdlets"></a><span data-ttu-id="531f3-542">Med hjälp av PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="531f3-542">Using PowerShell cmdlets</span></span>
<span data-ttu-id="531f3-543">Du kan aktivera kryptering på den kryptera virtuella Hårddisken med hjälp av PowerShell-cmdleten [ `Set-AzureRmVMOSDisk` ](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span><span class="sxs-lookup"><span data-stu-id="531f3-543">You can enable disk encryption on your encrypted VHD by using the PowerShell cmdlet [`Set-AzureRmVMOSDisk`](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span></span>  

#### <a name="using-cli-commands"></a><span data-ttu-id="531f3-544">Med hjälp av CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="531f3-544">Using CLI commands</span></span>
<span data-ttu-id="531f3-545">Gör följande om du vill aktivera kryptering för det här scenariot med hjälp av CLI-kommandon:</span><span class="sxs-lookup"><span data-stu-id="531f3-545">To enable disk encryption for this scenario by using CLI commands, do the following:</span></span>

1. <span data-ttu-id="531f3-546">Ange åtkomstprinciper i nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="531f3-546">Set access policies in your key vault:</span></span>

   * <span data-ttu-id="531f3-547">Ange den **EnabledForDiskEncryption** flaggan:</span><span class="sxs-lookup"><span data-stu-id="531f3-547">Set the **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * <span data-ttu-id="531f3-548">Ange behörigheter till Azure AD-program att skriva hemligheter i nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="531f3-548">Set permissions to Azure AD application to write secrets to your key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. <span data-ttu-id="531f3-549">Om du vill aktivera kryptering på en befintlig eller körs VM, skriver du:</span><span class="sxs-lookup"><span data-stu-id="531f3-549">To enable encryption on an existing or running VM, type:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. <span data-ttu-id="531f3-550">Hämta krypteringsstatus:</span><span class="sxs-lookup"><span data-stu-id="531f3-550">Get encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. <span data-ttu-id="531f3-551">Om du vill möjliggöra kryptering på en ny virtuell dator från den kryptera virtuella Hårddisken måste använda följande parametrar med den `azure vm create` kommando:</span><span class="sxs-lookup"><span data-stu-id="531f3-551">To enable encryption on a new VM from your encrypted VHD, use the following parameters with the `azure vm create` command:</span></span>

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a><span data-ttu-id="531f3-552">Aktivera kryptering på befintlig eller körs IaaS Windows VM i Azure</span><span class="sxs-lookup"><span data-stu-id="531f3-552">Enable encryption on existing or running IaaS Windows VM in Azure</span></span>
<span data-ttu-id="531f3-553">Du kan aktivera kryptering genom att använda Resource Manager-mall, PowerShell-cmdlets eller CLI-kommandona i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="531f3-553">In this scenario, you can enable encrypting by using the Resource Manager template, PowerShell cmdlets, or CLI commands.</span></span> <span data-ttu-id="531f3-554">I följande avsnitt beskrivs i detalj hur du aktiverar det med hjälp av Resource Manager-mall och CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="531f3-554">The following sections explain in greater detail how to enable it by using the Resource Manager template and CLI commands.</span></span>

#### <a name="using-the-resource-manager-template"></a><span data-ttu-id="531f3-555">Med hjälp av Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="531f3-555">Using the Resource Manager template</span></span>
<span data-ttu-id="531f3-556">Du kan aktivera kryptering på befintliga eller IaaS Windows virtuella datorer som körs i Azure med hjälp av den [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="531f3-556">You can enable disk encryption on existing or running IaaS Windows VMs in Azure by using the [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).</span></span>

1. <span data-ttu-id="531f3-557">Klicka på mallen Azure Snabbkurs **till Azure**, ange konfigurationen för kryptering på den **parametrar** bladet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="531f3-557">On the Azure quick-start template, click **Deploy to Azure**, enter the encryption configuration on the **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="531f3-558">Välj prenumerationen, resursgruppen, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** att aktivera kryptering på den befintliga eller körs IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-558">Select the subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** to enable encryption on the existing or running IaaS VM.</span></span>

<span data-ttu-id="531f3-559">I följande tabell visas mallparametrar Resource Manager för befintlig eller virtuella datorer som använder en Azure AD-klient-ID som körs:</span><span class="sxs-lookup"><span data-stu-id="531f3-559">The following table lists the Resource Manager template parameters for existing or running VMs that use an Azure AD client ID:</span></span>

| <span data-ttu-id="531f3-560">Parameter</span><span class="sxs-lookup"><span data-stu-id="531f3-560">Parameter</span></span> | <span data-ttu-id="531f3-561">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="531f3-561">Description</span></span> |
| --- | --- |
| <span data-ttu-id="531f3-562">AADClientID</span><span class="sxs-lookup"><span data-stu-id="531f3-562">AADClientID</span></span> | <span data-ttu-id="531f3-563">Klient-ID för Azure AD-program som har behörighet att skriva hemligheter i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-563">Client ID of the Azure AD application that has permissions to write secrets to the key vault.</span></span> |
| <span data-ttu-id="531f3-564">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="531f3-564">AADClientSecret</span></span> | <span data-ttu-id="531f3-565">Klienthemlighet för Azure AD-program som har behörighet att skriva hemligheter i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-565">Client secret of the Azure AD application that has permissions to write secrets to the key vault.</span></span> |
| <span data-ttu-id="531f3-566">keyVaultName</span><span class="sxs-lookup"><span data-stu-id="531f3-566">keyVaultName</span></span> | <span data-ttu-id="531f3-567">Namnet på nyckelvalvet som BitLocker-nyckel ska överföras till.</span><span class="sxs-lookup"><span data-stu-id="531f3-567">Name of the key vault that the BitLocker key should be uploaded to.</span></span> <span data-ttu-id="531f3-568">Du kan hämta den med hjälp av cmdleten `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span><span class="sxs-lookup"><span data-stu-id="531f3-568">You can get it by using the cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span></span> |
|  <span data-ttu-id="531f3-569">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="531f3-569">keyEncryptionKeyURL</span></span> | <span data-ttu-id="531f3-570">URL till den viktiga krypteringsnyckel som används för att kryptera den genererade BitLocker-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="531f3-570">URL of the key encryption key that's used to encrypt the generated BitLocker key.</span></span> <span data-ttu-id="531f3-571">Den här parametern är valfri om du väljer **nokek** i listrutan UseExistingKek.</span><span class="sxs-lookup"><span data-stu-id="531f3-571">This parameter is optional if you select **nokek** in the UseExistingKek drop-down list.</span></span> <span data-ttu-id="531f3-572">Om du väljer **kek** i listrutan UseExistingKek måste du ange den _keyEncryptionKeyURL_ värde.</span><span class="sxs-lookup"><span data-stu-id="531f3-572">If you select **kek** in the UseExistingKek drop-down list, you must enter the _keyEncryptionKeyURL_ value.</span></span> |
| <span data-ttu-id="531f3-573">volumeType</span><span class="sxs-lookup"><span data-stu-id="531f3-573">volumeType</span></span> | <span data-ttu-id="531f3-574">Typ av krypteringsåtgärden utförs på volymen.</span><span class="sxs-lookup"><span data-stu-id="531f3-574">Type of volume that the encryption operation is performed on.</span></span> <span data-ttu-id="531f3-575">Giltiga värden är _OS_, _Data_, och _alla_.</span><span class="sxs-lookup"><span data-stu-id="531f3-575">Valid values are _OS_, _Data_, and _All_.</span></span> |
| <span data-ttu-id="531f3-576">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="531f3-576">sequenceVersion</span></span> | <span data-ttu-id="531f3-577">Sekvens version av BitLocker-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="531f3-577">Sequence version of the BitLocker operation.</span></span> <span data-ttu-id="531f3-578">Öka det här versionsnumret varje gång en diskkryptering åtgärden utförs på samma virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="531f3-578">Increment this version number every time a disk-encryption operation is performed on the same VM.</span></span> |
| <span data-ttu-id="531f3-579">vmName</span><span class="sxs-lookup"><span data-stu-id="531f3-579">vmName</span></span> | <span data-ttu-id="531f3-580">Namnet på den virtuella datorn som krypteringsåtgärden ska utföras på.</span><span class="sxs-lookup"><span data-stu-id="531f3-580">Name of the VM that the encryption operation is to be performed on.</span></span> |

> [!NOTE]
> <span data-ttu-id="531f3-581">_KeyEncryptionKeyURL_ är en valfri parameter.</span><span class="sxs-lookup"><span data-stu-id="531f3-581">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="531f3-582">Du kan ge din egen KEK ytterligare säkerhetskontroll datakrypteringsnyckeln (BitLocker-kryptering hemliga) i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-582">You can bring your own KEK to further safeguard the data encryption key (BitLocker encryption secret) in the key vault.</span></span>

#### <a name="using-powershell-cmdlets"></a><span data-ttu-id="531f3-583">Med hjälp av PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="531f3-583">Using PowerShell cmdlets</span></span>
<span data-ttu-id="531f3-584">Information om hur du aktiverar kryptering med Azure Disk Encryption med PowerShell-cmdlets finns i blogginläggen [utforska Azure Disk Encryption med Azure PowerShell - del 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) och [utforska Azure Disk Encryption med Azure PowerShell - del 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span><span class="sxs-lookup"><span data-stu-id="531f3-584">For information about enabling encryption with Azure Disk Encryption by using PowerShell cmdlets, see the blog posts [Explore Azure Disk Encryption with Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) and [Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span></span>

#### <a name="using-cli-commands"></a><span data-ttu-id="531f3-585">Med hjälp av CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="531f3-585">Using CLI commands</span></span>
<span data-ttu-id="531f3-586">Om du vill möjliggöra kryptering på befintlig eller använder IaaS Windows VM i Azure med hjälp av CLI-kommandona måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-586">To enable encryption on existing or running IaaS Windows VM in Azure using CLI commands, do the following:</span></span>

1. <span data-ttu-id="531f3-587">Att ange åtkomstprinciper i nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="531f3-587">To set access policies in the key vault:</span></span>
   * <span data-ttu-id="531f3-588">Ange den **EnabledForDiskEncryption** flaggan:</span><span class="sxs-lookup"><span data-stu-id="531f3-588">Set the **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * <span data-ttu-id="531f3-589">Ange behörigheter till Azure AD-program att skriva hemligheter i nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="531f3-589">Set permissions to Azure AD application to write secrets to your key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. <span data-ttu-id="531f3-590">Aktivera kryptering på en befintlig eller körs VM:</span><span class="sxs-lookup"><span data-stu-id="531f3-590">To enable encryption on an existing or running VM:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. <span data-ttu-id="531f3-591">Hämta krypteringsstatus:</span><span class="sxs-lookup"><span data-stu-id="531f3-591">To get encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. <span data-ttu-id="531f3-592">Om du vill möjliggöra kryptering på en ny virtuell dator från den kryptera virtuella Hårddisken måste använda följande parametrar med den `azure vm create` kommando:</span><span class="sxs-lookup"><span data-stu-id="531f3-592">To enable encryption on a new VM from your encrypted VHD, use the following parameters with the `azure vm create` command:</span></span>

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a><span data-ttu-id="531f3-593">Aktivera kryptering på en befintlig eller körs IaaS Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="531f3-593">Enable encryption on an existing or running IaaS Linux VM in Azure</span></span>
<span data-ttu-id="531f3-594">Du kan aktivera kryptering på en befintlig eller körs IaaS Linux-dator i Azure med hjälp av den [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).</span><span class="sxs-lookup"><span data-stu-id="531f3-594">You can enable disk encryption on an existing or running IaaS Linux VM in Azure by using the [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).</span></span>

1. <span data-ttu-id="531f3-595">Klicka på **till Azure** på mallen Azure Snabbkurs, ange konfigurationen för kryptering på den **parametrar** bladet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="531f3-595">Click **Deploy to Azure** on the Azure quick-start template, enter the encryption configuration on the **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="531f3-596">Välj prenumerationen, resursgruppen, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** att aktivera kryptering på den befintliga eller körs IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-596">Select the subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** to enable encryption on the existing or running IaaS VM.</span></span>

<span data-ttu-id="531f3-597">I följande tabell visas Resource Manager mallparametrar för befintlig eller virtuella datorer som använder en Azure AD-klient-ID som körs:</span><span class="sxs-lookup"><span data-stu-id="531f3-597">The following table lists Resource Manager template parameters for existing or running VMs that use an Azure AD client ID:</span></span>

| <span data-ttu-id="531f3-598">Parameter</span><span class="sxs-lookup"><span data-stu-id="531f3-598">Parameter</span></span> | <span data-ttu-id="531f3-599">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="531f3-599">Description</span></span> |
| --- | --- |
| <span data-ttu-id="531f3-600">AADClientID</span><span class="sxs-lookup"><span data-stu-id="531f3-600">AADClientID</span></span> | <span data-ttu-id="531f3-601">Klient-ID för Azure AD-program som har behörighet att skriva hemligheter i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-601">Client ID of the Azure AD application that has permissions to write secrets to the key vault.</span></span> |
| <span data-ttu-id="531f3-602">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="531f3-602">AADClientSecret</span></span> | <span data-ttu-id="531f3-603">Klienthemlighet för Azure AD-program som har behörighet att skriva hemligheter i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-603">Client secret of the Azure AD application that has permissions to write secrets to your key vault.</span></span> |
| <span data-ttu-id="531f3-604">keyVaultName</span><span class="sxs-lookup"><span data-stu-id="531f3-604">keyVaultName</span></span> | <span data-ttu-id="531f3-605">Namnet på nyckelvalvet som BitLocker-nyckel ska överföras till.</span><span class="sxs-lookup"><span data-stu-id="531f3-605">Name of the key vault that the BitLocker key should be uploaded to.</span></span> <span data-ttu-id="531f3-606">Du kan hämta den med hjälp av cmdleten `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span><span class="sxs-lookup"><span data-stu-id="531f3-606">You can get it by using the cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span></span> |
|  <span data-ttu-id="531f3-607">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="531f3-607">keyEncryptionKeyURL</span></span> | <span data-ttu-id="531f3-608">URL till den viktiga krypteringsnyckel som används för att kryptera den genererade BitLocker-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="531f3-608">URL of the key encryption key that's used to encrypt the generated BitLocker key.</span></span> <span data-ttu-id="531f3-609">Den här parametern är valfri om du väljer **nokek** i listrutan UseExistingKek.</span><span class="sxs-lookup"><span data-stu-id="531f3-609">This parameter is optional if you select **nokek** in the UseExistingKek drop-down list.</span></span> <span data-ttu-id="531f3-610">Om du väljer **kek** i listrutan UseExistingKek måste du ange den _keyEncryptionKeyURL_ värde.</span><span class="sxs-lookup"><span data-stu-id="531f3-610">If you select **kek** in the UseExistingKek drop-down list, you must enter the _keyEncryptionKeyURL_ value.</span></span> |
| <span data-ttu-id="531f3-611">volumeType</span><span class="sxs-lookup"><span data-stu-id="531f3-611">volumeType</span></span> | <span data-ttu-id="531f3-612">Typ av krypteringsåtgärden utförs på volymen.</span><span class="sxs-lookup"><span data-stu-id="531f3-612">Type of volume that the encryption operation is performed on.</span></span> <span data-ttu-id="531f3-613">Giltiga värden som stöds är _OS_ eller _alla_ (för RHEL 7.2, CentOS 7.2 och Ubuntu 16.04) och _Data_ (för alla andra distributioner).</span><span class="sxs-lookup"><span data-stu-id="531f3-613">Valid supported values are _OS_ or _All_ (for RHEL 7.2, CentOS 7.2, and Ubuntu 16.04), and _Data_ (for all other distributions).</span></span> |
| <span data-ttu-id="531f3-614">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="531f3-614">sequenceVersion</span></span> | <span data-ttu-id="531f3-615">Sekvens version av BitLocker-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="531f3-615">Sequence version of the BitLocker operation.</span></span> <span data-ttu-id="531f3-616">Öka det här versionsnumret varje gång en diskkryptering åtgärden utförs på samma virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="531f3-616">Increment this version number every time a disk-encryption operation is performed on the same VM.</span></span> |
| <span data-ttu-id="531f3-617">vmName</span><span class="sxs-lookup"><span data-stu-id="531f3-617">vmName</span></span> | <span data-ttu-id="531f3-618">Namnet på den virtuella datorn som krypteringsåtgärden ska utföras på.</span><span class="sxs-lookup"><span data-stu-id="531f3-618">Name of the VM that the encryption operation is to be performed on.</span></span> |
| <span data-ttu-id="531f3-619">Lösenfrasen</span><span class="sxs-lookup"><span data-stu-id="531f3-619">passPhrase</span></span> | <span data-ttu-id="531f3-620">Ange ett starkt lösenord som datakrypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="531f3-620">Type a strong passphrase as the data encryption key.</span></span> |

> [!NOTE]
> <span data-ttu-id="531f3-621">_KeyEncryptionKeyURL_ är en valfri parameter.</span><span class="sxs-lookup"><span data-stu-id="531f3-621">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="531f3-622">Du kan ge din egen KEK ytterligare säkerhetskontroll datakrypteringsnyckeln (lösenfrasen hemlig) i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-622">You can bring your own KEK to further safeguard the data encryption key (passphrase secret) in your key vault.</span></span>

#### <a name="cli-commands"></a><span data-ttu-id="531f3-623">CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="531f3-623">CLI commands</span></span>
<span data-ttu-id="531f3-624">Du kan aktivera kryptering på den kryptera virtuella Hårddisken genom att installera och använda den [CLI kommandot](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="531f3-624">You can enable disk encryption on your encrypted VHD by installing and using the [CLI command](../cli-install-nodejs.md).</span></span> <span data-ttu-id="531f3-625">Om du vill möjliggöra kryptering på befintlig eller IaaS Linux virtuella datorer som körs i Azure med hjälp av CLI-kommandona måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-625">To enable encryption on existing or running IaaS Linux VMs in Azure by using CLI commands, do the following:</span></span>

1. <span data-ttu-id="531f3-626">Ange åtkomstprinciper i nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="531f3-626">Set access policies in the key vault:</span></span>

 * <span data-ttu-id="531f3-627">Ange den **EnabledForDiskEncryption** flaggan:</span><span class="sxs-lookup"><span data-stu-id="531f3-627">Set the **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * <span data-ttu-id="531f3-628">Ange behörigheter till Azure AD-program att skriva hemligheter i nyckelvalvet:</span><span class="sxs-lookup"><span data-stu-id="531f3-628">Set permissions to Azure AD application to write secrets to your key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. <span data-ttu-id="531f3-629">Aktivera kryptering på en befintlig eller körs VM:</span><span class="sxs-lookup"><span data-stu-id="531f3-629">To enable encryption on an existing or running VM:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. <span data-ttu-id="531f3-630">Hämta krypteringsstatus:</span><span class="sxs-lookup"><span data-stu-id="531f3-630">Get encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. <span data-ttu-id="531f3-631">Om du vill möjliggöra kryptering på en ny virtuell dator från den kryptera virtuella Hårddisken måste använda följande parametrar med den `azure vm create` kommando:</span><span class="sxs-lookup"><span data-stu-id="531f3-631">To enable encryption on a new VM from your encrypted VHD, use the following parameters with the `azure vm create` command:</span></span>
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-the-encryption-status-of-an-encrypted-iaas-vm"></a><span data-ttu-id="531f3-632">Hämta krypteringsstatus för en krypterad IaaS VM</span><span class="sxs-lookup"><span data-stu-id="531f3-632">Get the encryption status of an encrypted IaaS VM</span></span>
<span data-ttu-id="531f3-633">Du kan hämta krypteringsstatus med Azure Resource Manager [PowerShell-cmdlets](/powershell/azure/overview), eller CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="531f3-633">You can get the encryption status by using Azure Resource Manager, [PowerShell cmdlets](/powershell/azure/overview), or CLI commands.</span></span> <span data-ttu-id="531f3-634">I följande avsnitt beskrivs hur du använder den klassiska Azure-portalen och CLI-kommandon för att hämta krypteringsstatus.</span><span class="sxs-lookup"><span data-stu-id="531f3-634">The following sections explain how to use the Azure classic portal and CLI commands to get the encryption status.</span></span>

#### <a name="get-the-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a><span data-ttu-id="531f3-635">Hämta krypteringsstatus för en krypterad Windows-dator med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="531f3-635">Get the encryption status of an encrypted Windows VM by using Azure Resource Manager</span></span>
<span data-ttu-id="531f3-636">Du kan hämta krypteringsstatus av IaaS-VM från Azure Resource Manager genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-636">You can get the encryption status of the IaaS VM from Azure Resource Manager by doing the following:</span></span>

1. <span data-ttu-id="531f3-637">Logga in på den [klassiska Azure-portalen](https://portal.azure.com/), och klicka sedan på **virtuella datorer** i den vänstra rutan att visa en sammanfattning av de virtuella datorerna i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="531f3-637">Sign in to the [Azure classic portal](https://portal.azure.com/), and then click **Virtual machines** in the left pane to see a summary view of the virtual machines in your subscription.</span></span> <span data-ttu-id="531f3-638">Du kan filtrera vyn virtuella datorer genom att välja prenumerationsnamn i den **prenumeration** listrutan.</span><span class="sxs-lookup"><span data-stu-id="531f3-638">You can filter the virtual machines view by selecting the subscription name in the **Subscription** drop-down list.</span></span>

2. <span data-ttu-id="531f3-639">Längst upp i den **virtuella datorer** klickar du på **kolumner**.</span><span class="sxs-lookup"><span data-stu-id="531f3-639">At the top of the **Virtual machines** page, click **Columns**.</span></span>

3. <span data-ttu-id="531f3-640">På den **Välj kolumnen** bladet väljer **diskkryptering**, och klicka sedan på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="531f3-640">On the **Choose column** blade, select **Disk Encryption**, and then click **Update**.</span></span> <span data-ttu-id="531f3-641">Du bör se diskkryptering kolumnen visar krypteringsstatus _aktiverad_ eller _inte aktiverat_ för varje VM som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="531f3-641">You should see the disk-encryption column showing the encryption state _Enabled_ or _Not Enabled_ for each VM, as shown in the following figure:</span></span>

 ![Microsoft Antimalware i Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-the-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-the-disk-encryption-powershell-cmdlet"></a><span data-ttu-id="531f3-643">Hämta krypteringsstatus för en krypterad (Windows-/ Linux) IaaS VM med hjälp av PowerShell-cmdlet diskkryptering</span><span class="sxs-lookup"><span data-stu-id="531f3-643">Get the encryption status of an encrypted (Windows/Linux) IaaS VM by using the disk-encryption PowerShell cmdlet</span></span>
<span data-ttu-id="531f3-644">Du kan hämta krypteringsstatus av IaaS-VM från PowerShell-cmdlet diskkryptering `Get-AzureRmVMDiskEncryptionStatus`.</span><span class="sxs-lookup"><span data-stu-id="531f3-644">You can get the encryption status of the IaaS VM from the disk-encryption PowerShell cmdlet `Get-AzureRmVMDiskEncryptionStatus`.</span></span> <span data-ttu-id="531f3-645">För att få krypteringsinställningar för den virtuella datorn, anger du följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-645">To get the encryption settings for your VM, enter the following:</span></span>

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

<span data-ttu-id="531f3-646">Du kan inspektera resultatet av _Get-AzureRmVMDiskEncryptionStatus_ nyckeln URL: er för kryptering.</span><span class="sxs-lookup"><span data-stu-id="531f3-646">You can inspect the output of _Get-AzureRmVMDiskEncryptionStatus_ for encryption key URLs.</span></span>

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

<span data-ttu-id="531f3-647">Värdena OSVolumeEncrypted och DataVolumesEncrypted inställningar är inställda på _krypterad_, vilket visar att båda volymerna krypteras med Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="531f3-647">The OSVolumeEncrypted and DataVolumesEncrypted settings values are set to _Encrypted_, which shows that both volumes are encrypted using Azure Disk Encryption.</span></span> <span data-ttu-id="531f3-648">Information om hur du aktiverar kryptering med Azure Disk Encryption med PowerShell-cmdlets finns i blogginläggen [utforska Azure Disk Encryption med Azure PowerShell - del 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) och [utforska Azure Disk Encryption med Azure PowerShell - del 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span><span class="sxs-lookup"><span data-stu-id="531f3-648">For information about enabling encryption with Azure Disk Encryption by using PowerShell cmdlets, see the blog posts [Explore Azure Disk Encryption with Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) and [Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-649">På Linux virtuella datorer, det tar tre eller fyra minuter den `Get-AzureRmVMDiskEncryptionStatus` för att rapportera krypteringsstatus.</span><span class="sxs-lookup"><span data-stu-id="531f3-649">On Linux VMs, it takes three to four minutes for the `Get-AzureRmVMDiskEncryptionStatus` cmdlet to report the encryption status.</span></span>

#### <a name="get-the-encryption-status-of-the-iaas-vm-from-the-disk-encryption-cli-command"></a><span data-ttu-id="531f3-650">Hämta krypteringsstatus av IaaS-VM från kommandot CLI-diskkryptering</span><span class="sxs-lookup"><span data-stu-id="531f3-650">Get the encryption status of the IaaS VM from the disk-encryption CLI command</span></span>
<span data-ttu-id="531f3-651">Du kan hämta krypteringsstatus av IaaS-VM med hjälp av kommandot CLI-diskkryptering `azure vm show-disk-encryption-status`.</span><span class="sxs-lookup"><span data-stu-id="531f3-651">You can get the encryption status of the IaaS VM by using the disk-encryption CLI command `azure vm show-disk-encryption-status`.</span></span> <span data-ttu-id="531f3-652">Ange sessionen Azure CLI för att få krypteringsinställningar för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="531f3-652">To get the encryption settings for your VM, enter your Azure CLI session:</span></span>

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a><span data-ttu-id="531f3-653">Inaktivera kryptering på använder Windows IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="531f3-653">Disable encryption on running Windows IaaS VM</span></span>
<span data-ttu-id="531f3-654">Du kan inaktivera kryptering på en aktiv Windows eller Linux IaaS-VM via Azure Disk Encryption Resource Manager-mall eller PowerShell-cmdlets och ange konfigurationen för dekryptering.</span><span class="sxs-lookup"><span data-stu-id="531f3-654">You can disable encryption on a running Windows or Linux IaaS VM via the Azure Disk Encryption Resource Manager template or PowerShell cmdlets and specify the decryption configuration.</span></span>

##### <a name="windows-vm"></a><span data-ttu-id="531f3-655">Windows VM</span><span class="sxs-lookup"><span data-stu-id="531f3-655">Windows VM</span></span>
<span data-ttu-id="531f3-656">Inaktivera kryptering steget inaktiverar kryptering av Operativsystemet, datavolym eller båda på körs Windows IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-656">The disable-encryption step disables encryption of the OS, the data volume, or both on the running Windows IaaS VM.</span></span> <span data-ttu-id="531f3-657">Du kan inte inaktivera systemvolymen och lämna datavolym krypteras.</span><span class="sxs-lookup"><span data-stu-id="531f3-657">You cannot disable the OS volume and leave the data volume encrypted.</span></span> <span data-ttu-id="531f3-658">När steget inaktivera kryptering utförs Azure klassiska distributionsmodellen uppdaterar VM tjänstmodell och Windows IaaS-VM är markerad dekrypterade.</span><span class="sxs-lookup"><span data-stu-id="531f3-658">When the disable-encryption step is performed, the Azure classic deployment model updates the VM service model, and the Windows IaaS VM is marked decrypted.</span></span> <span data-ttu-id="531f3-659">Innehållet i den virtuella datorn är inte längre krypterat i vila.</span><span class="sxs-lookup"><span data-stu-id="531f3-659">The contents of the VM are no longer encrypted at rest.</span></span> <span data-ttu-id="531f3-660">Dekrypteringen tar inte bort nyckelvalvet och kryptering nyckelmaterial (krypteringsnycklar BitLocker för Windows och lösenfrasen för Linux).</span><span class="sxs-lookup"><span data-stu-id="531f3-660">The decryption does not delete your key vault and the encryption key material (BitLocker encryption keys for Windows and Passphrase for Linux).</span></span>

##### <a name="linux-vm"></a><span data-ttu-id="531f3-661">Linux VM</span><span class="sxs-lookup"><span data-stu-id="531f3-661">Linux VM</span></span>
<span data-ttu-id="531f3-662">Inaktivera kryptering steget inaktiverar kryptering av datavolym på körs Linux IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-662">The disable-encryption step disables encryption of the data volume on the running Linux IaaS VM.</span></span> <span data-ttu-id="531f3-663">Det här steget fungerar bara om OS-disken inte krypteras.</span><span class="sxs-lookup"><span data-stu-id="531f3-663">This step only works if the OS disk is not encrypted.</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-664">Om du inaktiverar kryptering på OS-disken tillåts inte för virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="531f3-664">Disabling encryption on the OS disk is not allowed on Linux VMs.</span></span>

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a><span data-ttu-id="531f3-665">Inaktivera kryptering på en befintlig eller körs IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="531f3-665">Disable encryption on an existing or running IaaS VM</span></span>
<span data-ttu-id="531f3-666">Du kan inaktivera diskkryptering på Windows IaaS virtuella datorer som körs med hjälp av den [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="531f3-666">You can disable disk encryption on running Windows IaaS VMs by using the [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).</span></span>

1. <span data-ttu-id="531f3-667">Klicka på mallen Azure Snabbkurs **till Azure**, ange dekryptering konfigurationen på den **parametrar** bladet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="531f3-667">On the Azure quick-start template, click **Deploy to Azure**, enter the decryption configuration on the **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="531f3-668">Välj prenumerationen, resursgruppen, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** att aktivera kryptering på en ny IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="531f3-668">Select the subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** to enable encryption on a new IaaS VM.</span></span>

<span data-ttu-id="531f3-669">För Linux virtuella datorer kan du inaktivera kryptering med hjälp av den [inaktivera kryptering på en körs Linux VM](https://aka.ms/decrypt-linuxvm) mall.</span><span class="sxs-lookup"><span data-stu-id="531f3-669">For Linux VMs, you can disable encryption by using the [Disable encryption on a running Linux VM](https://aka.ms/decrypt-linuxvm) template.</span></span>

<span data-ttu-id="531f3-670">I följande tabell visas mallparametrar för Resource Manager för inaktivering av kryptering på en körs IaaS-VM:</span><span class="sxs-lookup"><span data-stu-id="531f3-670">The following table lists Resource Manager template parameters for disabling encryption on a running IaaS VM:</span></span>

| <span data-ttu-id="531f3-671">Parameter</span><span class="sxs-lookup"><span data-stu-id="531f3-671">Parameter</span></span> | <span data-ttu-id="531f3-672">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="531f3-672">Description</span></span> |
| --- | --- |
| <span data-ttu-id="531f3-673">vmName</span><span class="sxs-lookup"><span data-stu-id="531f3-673">vmName</span></span> | <span data-ttu-id="531f3-674">Namnet på den virtuella datorn som krypteringsåtgärden ska utföras på.</span><span class="sxs-lookup"><span data-stu-id="531f3-674">Name of the VM that the encryption operation is to be performed on.</span></span>
| <span data-ttu-id="531f3-675">volumeType</span><span class="sxs-lookup"><span data-stu-id="531f3-675">volumeType</span></span> | <span data-ttu-id="531f3-676">Typ av volymen som en dekrypteringsåtgärd utförs på.</span><span class="sxs-lookup"><span data-stu-id="531f3-676">Type of volume that a decryption operation is performed on.</span></span> <span data-ttu-id="531f3-677">Giltiga värden är _OS_, _Data_, och _alla_.</span><span class="sxs-lookup"><span data-stu-id="531f3-677">Valid values are _OS_, _Data_, and _All_.</span></span> <span data-ttu-id="531f3-678">Du kan inte inaktivera kryptering på kör Windows IaaS-VM OS/startvolymen utan inaktiverar kryptering på den _Data_ volym.</span><span class="sxs-lookup"><span data-stu-id="531f3-678">You cannot disable encryption on running Windows IaaS VM OS/boot volume without disabling encryption on the _Data_ volume.</span></span> <span data-ttu-id="531f3-679">Observera också att om du inaktiverar kryptering på OS-disken inte tillåts för virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="531f3-679">Also note that disabling encryption on the OS disk is not allowed on Linux VMs.</span></span> |
| <span data-ttu-id="531f3-680">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="531f3-680">sequenceVersion</span></span> | <span data-ttu-id="531f3-681">Sekvens version av BitLocker-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="531f3-681">Sequence version of the BitLocker operation.</span></span> <span data-ttu-id="531f3-682">Öka det här versionsnumret varje gång en disk dekrypteringsåtgärden utförs på samma virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="531f3-682">Increment this version number every time a disk decryption operation is performed on the same VM.</span></span> |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a><span data-ttu-id="531f3-683">Inaktivera kryptering på en befintlig eller körs IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="531f3-683">Disable encryption on an existing or running IaaS VM</span></span>
<span data-ttu-id="531f3-684">Om du vill inaktivera kryptering på en befintlig eller körs IaaS-VM med hjälp av PowerShell-cmdleten, se [ `Disable-AzureRmVMDiskEncryption` ](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption).</span><span class="sxs-lookup"><span data-stu-id="531f3-684">To disable encryption on an existing or running IaaS VM by using the PowerShell cmdlet, see [`Disable-AzureRmVMDiskEncryption`](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption).</span></span> <span data-ttu-id="531f3-685">Denna cmdlet har stöd för både Windows- och Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="531f3-685">This cmdlet supports both Windows and Linux VMs.</span></span> <span data-ttu-id="531f3-686">Om du vill inaktivera kryptering, installerar ett tillägg på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-686">To disable encryption, it installs an extension on the virtual machine.</span></span> <span data-ttu-id="531f3-687">Om den _namn_ parameter har angetts ett tillägg med standardnamnet _AzureDiskEncryption för virtuella Windows-datorer_ skapas.</span><span class="sxs-lookup"><span data-stu-id="531f3-687">If the _Name_ parameter is not specified, an extension with the default name _AzureDiskEncryption for Windows VMs_ is created.</span></span>

<span data-ttu-id="531f3-688">Tillägget AzureDiskEncryptionForLinux används på Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="531f3-688">On Linux VMs, the AzureDiskEncryptionForLinux extension is used.</span></span>

> [!NOTE]
> <span data-ttu-id="531f3-689">Denna cmdlet startar om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-689">This cmdlet reboots the virtual machine.</span></span>

### <a name="enable-encryption-on-pre-encrypted-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="531f3-690">Aktivera kryptering på förkrypterade IaaS-VM med Azure-hanterade disken</span><span class="sxs-lookup"><span data-stu-id="531f3-690">Enable encryption on pre-encrypted IaaS VM with Azure Managed Disk</span></span>
<span data-ttu-id="531f3-691">Använda Azure hanteras Disk ARM-mall för att skapa en krypterad VM från en förkrypterade virtuell Hårddisk med hjälp av ARM-mallen finns i</span><span class="sxs-lookup"><span data-stu-id="531f3-691">Use the Azure Managed Disk ARM template to create a encrypted VM from a pre-encrypted VHD using the ARM template located at</span></span>   
<span data-ttu-id="531f3-692">[Skapa en ny krypterade hanterade disk från en förkrypterade VHD/storage-blob] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)</span><span class="sxs-lookup"><span data-stu-id="531f3-692">[Create a new encrypted managed disk from a pre-encrypted VHD/storage blob] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)</span></span>

### <a name="enable-encryption-on-a-new-linux-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="531f3-693">Aktivera kryptering på en ny Linux IaaS virtuell dator med hanterad Azure-disken</span><span class="sxs-lookup"><span data-stu-id="531f3-693">Enable encryption on a new Linux IaaS VM with Azure Managed Disk</span></span>
<span data-ttu-id="531f3-694">Använd Azure hanteras Disk ARM-mall för att skapa en ny krypterade Linux IaaS virtuell dator med hjälp av ARM-mallen finns i</span><span class="sxs-lookup"><span data-stu-id="531f3-694">Use the Azure Managed Disk ARM template to create a new encrypted Linux IaaS VM using the ARM template located at</span></span>   
<span data-ttu-id="531f3-695">[Distributionen av RHEL 7.2 med fullständig diskkryptering] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)</span><span class="sxs-lookup"><span data-stu-id="531f3-695">[Deployment of RHEL 7.2 with full disk encryption] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)</span></span>

### <a name="enable-encryption-on-a-new-windows-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="531f3-696">Aktivera kryptering på en ny Windows IaaS-VM med Azure-hanterade disken</span><span class="sxs-lookup"><span data-stu-id="531f3-696">Enable encryption on a new Windows IaaS VM with Azure Managed Disk</span></span>
 <span data-ttu-id="531f3-697">Använd Azure hanteras Disk ARM-mall för att skapa en ny krypterade Linux IaaS virtuell dator med hjälp av ARM-mallen finns i</span><span class="sxs-lookup"><span data-stu-id="531f3-697">Use the Azure Managed Disk ARM template to create a new encrypted Linux IaaS VM using the ARM template located at</span></span>   
 <span data-ttu-id="531f3-698">[Skapa en ny krypterade Windows IaaS hanteras Disk virtuell dator från galleriet avbildning] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)</span><span class="sxs-lookup"><span data-stu-id="531f3-698">[Create a new encrypted Windows IaaS Managed Disk VM from gallery image] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)</span></span>

  > [!NOTE]
  ><span data-ttu-id="531f3-699">Det är obligatoriskt att ögonblicksbilden och/eller säkerhetskopiering hanterade diskar baserat utanför och innan du aktiverar Azure Disk Encryption VM-instans.</span><span class="sxs-lookup"><span data-stu-id="531f3-699">It is mandatory to snapshot and/or backup a managed disk based VM instance outside of and prior to enabling Azure Disk Encryption.</span></span>  <span data-ttu-id="531f3-700">En ögonblicksbild av den hantera disken kan hämtas från portalen eller Azure Backup kan användas.</span><span class="sxs-lookup"><span data-stu-id="531f3-700">A snapshot of the managed disk can be taken from the portal, or Azure Backup can be used.</span></span>  <span data-ttu-id="531f3-701">Säkerhetskopior Se till att ett återställningsalternativ är möjligt när det gäller ett oväntat fel under krypteringen.</span><span class="sxs-lookup"><span data-stu-id="531f3-701">Backups ensure that a recovery option is possible in the case of any unexpected failure during encryption.</span></span>  <span data-ttu-id="531f3-702">Cmdlet Set-AzureRmVMDiskEncryptionExtension kan användas för att kryptera hanterade diskar genom att ange parametern - skipVmBackup när en säkerhetskopia görs.</span><span class="sxs-lookup"><span data-stu-id="531f3-702">Once a backup is made, the Set-AzureRmVMDiskEncryptionExtension cmdlet can be used to encrypt managed disks by specifying the -skipVmBackup parameter.</span></span>  <span data-ttu-id="531f3-703">Det här kommandot misslyckas mot hanterade diskbaserat VM tills en säkerhetskopia som har gjorts och den här parametern har angetts.</span><span class="sxs-lookup"><span data-stu-id="531f3-703">This command will fail against managed disk based VM's until a backup has been made and this parameter has been specified.</span></span>    
 
### <a name="update-encryption-settings-of-an-existing-encrypted-non-premium-vm"></a><span data-ttu-id="531f3-704">Uppdatera krypteringsinställningarna för en befintlig krypterade icke-premium virtuell dator</span><span class="sxs-lookup"><span data-stu-id="531f3-704">Update encryption settings of an existing encrypted non-premium VM</span></span>
  <span data-ttu-id="531f3-705">Använda befintliga Azure-disken kryptering stöds gränssnitt för att köra VM [PS-cmdlets, CLI eller ARM-mallar] för att uppdatera inställningarna för kryptering som AAD klient ID/hemlig nyckel krypteringsnyckeln [KEK] BitLocker-krypteringsnyckeln för Windows-VM eller lösenfrasen för Linux Virtuella osv. Inställningen för kryptering av uppdatering stöds bara för virtuella datorer som backas upp av icke-premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="531f3-705">Use the existing Azure disk encryption supported interfaces for running VM [PS cmdlets, CLI or ARM templates] to update the encryption settings like AAD client ID/secret, Key encryption key [KEK], BitLocker encryption key for Windows VM or Passphrase for Linux VM etc. The update encryption setting is supported only for VMs backed by non-premium storage.</span></span> <span data-ttu-id="531f3-706">Det är NNOT stöd för virtuella datorer som backas upp av premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="531f3-706">It is NNOT supported for VMs backed by premium storage.</span></span>

## <a name="appendix"></a><span data-ttu-id="531f3-707">Bilaga</span><span class="sxs-lookup"><span data-stu-id="531f3-707">Appendix</span></span>
### <a name="connect-to-your-subscription"></a><span data-ttu-id="531f3-708">Ansluta till din prenumeration</span><span class="sxs-lookup"><span data-stu-id="531f3-708">Connect to your subscription</span></span>
<span data-ttu-id="531f3-709">Innan du fortsätter kan du granska den *krav* i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="531f3-709">Before you proceed, review the *Prerequisites* section in this article.</span></span> <span data-ttu-id="531f3-710">När du har kontrollerat att alla krav är uppfyllda, kan du ansluta till din prenumeration genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-710">After you ensure that all prerequisites have been met, connect to your subscription by doing the following:</span></span>

1. <span data-ttu-id="531f3-711">Starta en Azure PowerShell-session och logga in på ditt Azure-konto med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="531f3-711">Start an Azure PowerShell session, and sign in to your Azure account with the following command:</span></span>

    `Login-AzureRmAccount`

2. <span data-ttu-id="531f3-712">Skriv följande om du vill visa prenumerationer för ditt konto om du har flera prenumerationer och vill ange en ska användas:</span><span class="sxs-lookup"><span data-stu-id="531f3-712">If you have multiple subscriptions and want to specify one to use, type the following to see the subscriptions for your account:</span></span>

    `Get-AzureRmSubscription`

3. <span data-ttu-id="531f3-713">Om du vill ange den prenumeration som du vill använda, skriver du:</span><span class="sxs-lookup"><span data-stu-id="531f3-713">To specify the subscription you want to use, type:</span></span>

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. <span data-ttu-id="531f3-714">Om du vill kontrollera att prenumerationen konfigurerats är korrekt, skriver du:</span><span class="sxs-lookup"><span data-stu-id="531f3-714">To verify that the subscription configured is correct, type:</span></span>

    `Get-AzureRmSubscription`

5. <span data-ttu-id="531f3-715">För att bekräfta Azure Disk Encryption-cmdlet: ar är installerade, skriver du:</span><span class="sxs-lookup"><span data-stu-id="531f3-715">To confirm the Azure Disk Encryption cmdlets are installed, type:</span></span>

    `Get-command *diskencryption*`

6. <span data-ttu-id="531f3-716">Följande utdata bekräftar Azure Disk Encryption PowerShell-installationen:</span><span class="sxs-lookup"><span data-stu-id="531f3-716">The following output confirms the Azure Disk Encryption PowerShell installation:</span></span>

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a><span data-ttu-id="531f3-717">Förbereda en förkrypterade Windows VHD</span><span class="sxs-lookup"><span data-stu-id="531f3-717">Prepare a pre-encrypted Windows VHD</span></span>
<span data-ttu-id="531f3-718">Avsnitten som följer är nödvändiga för att förbereda en förkrypterade Windows VHD för distribution som en krypterad VHD i Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="531f3-718">The sections that follow are necessary to prepare a pre-encrypted Windows VHD for deployment as an encrypted VHD in Azure IaaS.</span></span> <span data-ttu-id="531f3-719">Använd informationen för att förbereda och starta en ny Windows VM (VHD) på Azure Site Recovery eller Azure.</span><span class="sxs-lookup"><span data-stu-id="531f3-719">Use the information to prepare and boot a fresh Windows VM (VHD) on Azure Site Recovery or Azure.</span></span>

#### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a><span data-ttu-id="531f3-720">Uppdatera grupprincipen för att tillåta utan TPM för OS-skydd</span><span class="sxs-lookup"><span data-stu-id="531f3-720">Update group policy to allow non-TPM for OS protection</span></span>
<span data-ttu-id="531f3-721">Konfigurera BitLocker grupprincipinställningen **BitLocker-diskkryptering**, som du hittar **lokal datorprincip** > **Datorkonfiguration** > **Administrationsmallar** > **Windows-komponenter**.</span><span class="sxs-lookup"><span data-stu-id="531f3-721">Configure the BitLocker Group Policy setting **BitLocker Drive Encryption**, which you'll find under **Local Computer Policy** > **Computer Configuration** > **Administrative Templates** > **Windows Components**.</span></span> <span data-ttu-id="531f3-722">Ändra inställningen till **operativsystemsenheter** > **kräver ytterligare autentisering vid start** > **Tillåt BitLocker utan en kompatibel TPM**som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="531f3-722">Change this setting to **Operating System Drives** > **Require additional authentication at startup** > **Allow BitLocker without a compatible TPM**, as shown in the following figure:</span></span>

![Microsoft Antimalware i Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a><span data-ttu-id="531f3-724">Installera komponenter för BitLocker-funktion</span><span class="sxs-lookup"><span data-stu-id="531f3-724">Install BitLocker feature components</span></span>
<span data-ttu-id="531f3-725">För Windows Server 2012 och senare, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="531f3-725">For Windows Server 2012 and later, use the following command:</span></span>

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

<span data-ttu-id="531f3-726">För Windows Server 2008 R2, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="531f3-726">For Windows Server 2008 R2, use the following command:</span></span>

    ServerManagerCmd -install BitLockers

#### <a name="prepare-the-os-volume-for-bitlocker-by-using-bdehdcfg"></a><span data-ttu-id="531f3-727">Förbereda systemvolymen för BitLocker med hjälp av`bdehdcfg`</span><span class="sxs-lookup"><span data-stu-id="531f3-727">Prepare the OS volume for BitLocker by using `bdehdcfg`</span></span>
<span data-ttu-id="531f3-728">Om du vill komprimera OS-partitionen och förbereda datorn för BitLocker, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="531f3-728">To compress the OS partition and prepare the machine for BitLocker, execute the following command:</span></span>

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-the-os-volume-by-using-bitlocker"></a><span data-ttu-id="531f3-729">Skydda systemvolymen med BitLocker</span><span class="sxs-lookup"><span data-stu-id="531f3-729">Protect the OS volume by using BitLocker</span></span>
<span data-ttu-id="531f3-730">Använd den [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) kommando för att aktivera kryptering på startvolymen med hjälp av ett externt nyckelskydd.</span><span class="sxs-lookup"><span data-stu-id="531f3-730">Use the [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx) command to enable encryption on the boot volume using an external key protector.</span></span> <span data-ttu-id="531f3-731">Också placera den externa nyckeln (.bek-fil) på extern enhet eller volym.</span><span class="sxs-lookup"><span data-stu-id="531f3-731">Also place the external key (.bek file) on the external drive or volume.</span></span> <span data-ttu-id="531f3-732">Kryptering är aktiverat på systemet/startvolymen efter nästa omstart.</span><span class="sxs-lookup"><span data-stu-id="531f3-732">Encryption is enabled on the system/boot volume after the next reboot.</span></span>

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> <span data-ttu-id="531f3-733">Förbered den virtuella datorn med en separat/Dataresurs VHD för att hämta den externa nyckeln med hjälp av BitLocker.</span><span class="sxs-lookup"><span data-stu-id="531f3-733">Prepare the VM with a separate data/resource VHD for getting the external key by using BitLocker.</span></span>

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a><span data-ttu-id="531f3-734">Kryptera en OS-enheten på en Linux-VM som körs</span><span class="sxs-lookup"><span data-stu-id="531f3-734">Encrypting an OS drive on a running Linux VM</span></span>
<span data-ttu-id="531f3-735">Kryptering av en OS-enhet på en Linux-VM som körs stöds på följande distributioner:</span><span class="sxs-lookup"><span data-stu-id="531f3-735">Encryption of an OS drive on a running Linux VM is supported on the following distributions:</span></span>

* <span data-ttu-id="531f3-736">RHEL 7.2</span><span class="sxs-lookup"><span data-stu-id="531f3-736">RHEL 7.2</span></span>
* <span data-ttu-id="531f3-737">CentOS 7.2</span><span class="sxs-lookup"><span data-stu-id="531f3-737">CentOS 7.2</span></span>
* <span data-ttu-id="531f3-738">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="531f3-738">Ubuntu 16.04</span></span>

##### <a name="prerequisites-for-os-disk-encryption"></a><span data-ttu-id="531f3-739">Krav för OS-diskkryptering</span><span class="sxs-lookup"><span data-stu-id="531f3-739">Prerequisites for OS disk encryption</span></span>

* <span data-ttu-id="531f3-740">Den virtuella datorn måste skapas från Marketplace-avbildning i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="531f3-740">The VM must be created from the Marketplace image in Azure Resource Manager.</span></span>
* <span data-ttu-id="531f3-741">Azure virtuell dator med minst 4 GB RAM-minne (rekommenderas storleken är 7 GB).</span><span class="sxs-lookup"><span data-stu-id="531f3-741">Azure VM with at least 4 GB of RAM (recommended size is 7 GB).</span></span>
* <span data-ttu-id="531f3-742">(För RHEL och CentOS) Inaktivera SELinux.</span><span class="sxs-lookup"><span data-stu-id="531f3-742">(For RHEL and CentOS) Disable SELinux.</span></span> <span data-ttu-id="531f3-743">Om du vill inaktivera SELinux finns i ”4.4.2.</span><span class="sxs-lookup"><span data-stu-id="531f3-743">To disable SELinux, see "4.4.2.</span></span> <span data-ttu-id="531f3-744">Inaktivera SELinux ”i den [SELinux användar- och Administrator's Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-744">Disabling SELinux" in the [SELinux User's and Administrator's Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) on the VM.</span></span>
* <span data-ttu-id="531f3-745">Starta om den virtuella datorn på minst en gång när du inaktiverar SELinux.</span><span class="sxs-lookup"><span data-stu-id="531f3-745">After you disable SELinux, reboot the VM at least once.</span></span>

##### <a name="steps"></a><span data-ttu-id="531f3-746">Steg</span><span class="sxs-lookup"><span data-stu-id="531f3-746">Steps</span></span>
1. <span data-ttu-id="531f3-747">Skapa en virtuell dator med någon av de distributioner som har angetts tidigare.</span><span class="sxs-lookup"><span data-stu-id="531f3-747">Create a VM by using one of the distributions specified previously.</span></span>

 <span data-ttu-id="531f3-748">OS-diskkryptering stöds för CentOS 7.2 via en särskild avbildning.</span><span class="sxs-lookup"><span data-stu-id="531f3-748">For CentOS 7.2, OS disk encryption is supported via a special image.</span></span> <span data-ttu-id="531f3-749">Ange ”7.2n” som SKU: N om du vill använda den här avbildningen när du skapar den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="531f3-749">To use this image, specify "7.2n" as the SKU when you create the VM:</span></span>
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. <span data-ttu-id="531f3-750">Konfigurera den virtuella datorn efter dina behov.</span><span class="sxs-lookup"><span data-stu-id="531f3-750">Configure the VM according to your needs.</span></span> <span data-ttu-id="531f3-751">Om du vill att kryptera alla (OS + data) enheter, dataenheter måste vara angivet och kan monteras från /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="531f3-751">If you are going to encrypt all the (OS + data) drives, the data drives need to be specified and mountable from /etc/fstab.</span></span>

 > [!NOTE]
 > <span data-ttu-id="531f3-752">Använd UUID =... att ange dataenheter i/etc/fstab istället för att ange blockera enhetens namn (till exempel/dev/sdb1).</span><span class="sxs-lookup"><span data-stu-id="531f3-752">Use UUID=... to specify data drives in /etc/fstab instead of specifying the block device name (for example, /dev/sdb1).</span></span> <span data-ttu-id="531f3-753">Ordningen på enheter ändras på den virtuella datorn under krypteringen.</span><span class="sxs-lookup"><span data-stu-id="531f3-753">During encryption, the order of drives changes on the VM.</span></span> <span data-ttu-id="531f3-754">Om den virtuella datorn är beroende av en viss ordning för blockera enheter, kommer inte att montera dem efter kryptering.</span><span class="sxs-lookup"><span data-stu-id="531f3-754">If your VM relies on a specific order of block devices, it will fail to mount them after encryption.</span></span>

3. <span data-ttu-id="531f3-755">Logga ut från SSH-sessioner.</span><span class="sxs-lookup"><span data-stu-id="531f3-755">Sign out of the SSH sessions.</span></span>

4. <span data-ttu-id="531f3-756">Ange om du vill kryptera OS volumeType som **alla** eller **OS** när du [Aktivera kryptering](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).</span><span class="sxs-lookup"><span data-stu-id="531f3-756">To encrypt the OS, specify volumeType as **All** or **OS** when you [enable encryption](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).</span></span>

 > [!NOTE]
 > <span data-ttu-id="531f3-757">Alla användare kan processer som inte körs som `systemd` tjänster ska avslutas med en `SIGKILL`.</span><span class="sxs-lookup"><span data-stu-id="531f3-757">All user-space processes that are not running as `systemd` services should be killed with a `SIGKILL`.</span></span> <span data-ttu-id="531f3-758">Starta om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-758">Reboot the VM.</span></span> <span data-ttu-id="531f3-759">När du aktiverar OS-diskkryptering på en aktiv virtuell dator planera VM driftstopp.</span><span class="sxs-lookup"><span data-stu-id="531f3-759">When you enable OS disk encryption on a running VM, plan on VM downtime.</span></span>

5. <span data-ttu-id="531f3-760">Regelbundet övervaka förloppet för kryptering med hjälp av anvisningarna i den [nästa avsnitt](#monitoring-os-encryption-progress).</span><span class="sxs-lookup"><span data-stu-id="531f3-760">Periodically monitor the progress of encryption by using the instructions in the [next section](#monitoring-os-encryption-progress).</span></span>

6. <span data-ttu-id="531f3-761">När Get-AzureRmVmDiskEncryptionStatus visar ”VMRestartPending”, startar du om den virtuella datorn genom att logga in till den eller genom att använda portalen, PowerShell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="531f3-761">After Get-AzureRmVmDiskEncryptionStatus shows "VMRestartPending," restart your VM either by signing in to it or by using the portal, PowerShell, or CLI.</span></span>
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot the VM
    ```
<span data-ttu-id="531f3-762">Innan du startar om rekommenderar vi att du sparar [starta diagnostik](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-762">Before you reboot, we recommend that you save [boot diagnostics](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) of the VM.</span></span>

#### <a name="monitoring-os-encryption-progress"></a><span data-ttu-id="531f3-763">Övervaka förloppet för OS-kryptering</span><span class="sxs-lookup"><span data-stu-id="531f3-763">Monitoring OS encryption progress</span></span>
<span data-ttu-id="531f3-764">Du kan övervaka förloppet för OS-kryptering på tre sätt:</span><span class="sxs-lookup"><span data-stu-id="531f3-764">You can monitor OS encryption progress in three ways:</span></span>

* <span data-ttu-id="531f3-765">Använd den `Get-AzureRmVmDiskEncryptionStatus` cmdlet och kontrollera fältet ProgressMessage:</span><span class="sxs-lookup"><span data-stu-id="531f3-765">Use the `Get-AzureRmVmDiskEncryptionStatus` cmdlet and inspect the ProgressMessage field:</span></span>
    ```
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 <span data-ttu-id="531f3-766">När den virtuella datorn når ”OS-disken kryptering igång”, tar ungefär 40 till 50 minuter säkerhetskopieras VM på en Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="531f3-766">After the VM reaches "OS disk encryption started," it takes about 40 to 50 minutes on a Premium-storage backed VM.</span></span>

 <span data-ttu-id="531f3-767">Eftersom [utfärda #388](https://github.com/Azure/WALinuxAgent/issues/388) i WALinuxAgent, `OsVolumeEncrypted` och `DataVolumesEncrypted` visas som `Unknown` i vissa distributioner.</span><span class="sxs-lookup"><span data-stu-id="531f3-767">Because of [issue #388](https://github.com/Azure/WALinuxAgent/issues/388) in WALinuxAgent, `OsVolumeEncrypted` and `DataVolumesEncrypted` show up as `Unknown` in some distributions.</span></span> <span data-ttu-id="531f3-768">Med WALinuxAgent version 2.1.5 och senare kan det här problemet löses automatiskt.</span><span class="sxs-lookup"><span data-stu-id="531f3-768">With WALinuxAgent version 2.1.5 and later, this issue is fixed automatically.</span></span> <span data-ttu-id="531f3-769">Om du ser `Unknown` utdata och du kan kontrollera status för disk-kryptering med hjälp av resursutforskaren i Azure.</span><span class="sxs-lookup"><span data-stu-id="531f3-769">If you see `Unknown` in the output, you can verify disk-encryption status by using the Azure Resource Explorer.</span></span>

 <span data-ttu-id="531f3-770">Gå till [resursutforskaren Azure](https://resources.azure.com/), och expandera sedan den här hierarkin på panelen markeringen vänster:</span><span class="sxs-lookup"><span data-stu-id="531f3-770">Go to [Azure Resource Explorer](https://resources.azure.com/), and then expand this hierarchy in the selection panel on left:</span></span>

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

 <span data-ttu-id="531f3-771">Rulla ned till krypteringsstatus för enheter i InstanceView.</span><span class="sxs-lookup"><span data-stu-id="531f3-771">In the InstanceView, scroll down to see the encryption status of your drives.</span></span>

 ![VM-instansvyn](./media/azure-security-disk-encryption/vm-instanceview.png)

* <span data-ttu-id="531f3-773">Titta på [starta diagnostik](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/).</span><span class="sxs-lookup"><span data-stu-id="531f3-773">Look at [boot diagnostics](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/).</span></span> <span data-ttu-id="531f3-774">Meddelanden från tillägget ADE prefixet `[AzureDiskEncryption]`.</span><span class="sxs-lookup"><span data-stu-id="531f3-774">Messages from the ADE extension should be prefixed with `[AzureDiskEncryption]`.</span></span>

* <span data-ttu-id="531f3-775">Logga in på den virtuella datorn via SSH och hämta tillägget loggen:</span><span class="sxs-lookup"><span data-stu-id="531f3-775">Sign in to the VM via SSH, and get the extension log from:</span></span>

    <span data-ttu-id="531f3-776">/var/log/Azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux</span><span class="sxs-lookup"><span data-stu-id="531f3-776">/var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux</span></span>

 <span data-ttu-id="531f3-777">Vi rekommenderar att du inte loggar in till den virtuella datorn när OS-kryptering pågår.</span><span class="sxs-lookup"><span data-stu-id="531f3-777">We recommend that you do not sign in to the VM while OS encryption is in progress.</span></span> <span data-ttu-id="531f3-778">Kopiera loggar endast när de två metoderna har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="531f3-778">Copy the logs only when the other two methods have failed.</span></span>

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a><span data-ttu-id="531f3-779">Förbereda en förkrypterade Linux VHD</span><span class="sxs-lookup"><span data-stu-id="531f3-779">Prepare a pre-encrypted Linux VHD</span></span>
##### <a name="ubuntu-16"></a><span data-ttu-id="531f3-780">Ubuntu 16</span><span class="sxs-lookup"><span data-stu-id="531f3-780">Ubuntu 16</span></span>
<span data-ttu-id="531f3-781">Konfigurera kryptering under installationen av distributionsplatsen genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-781">Configure encryption during the distribution installation by doing the following:</span></span>

1. <span data-ttu-id="531f3-782">Välj **konfigurera krypterade volymer** när du partitionera diskarna.</span><span class="sxs-lookup"><span data-stu-id="531f3-782">Select **Configure encrypted volumes** when you partition the disks.</span></span>

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. <span data-ttu-id="531f3-784">Skapa en separat startenheten som inte får vara krypterade.</span><span class="sxs-lookup"><span data-stu-id="531f3-784">Create a separate boot drive, which must not be encrypted.</span></span> <span data-ttu-id="531f3-785">Kryptera din rotenhet.</span><span class="sxs-lookup"><span data-stu-id="531f3-785">Encrypt your root drive.</span></span>

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. <span data-ttu-id="531f3-787">Ange en lösenfras.</span><span class="sxs-lookup"><span data-stu-id="531f3-787">Provide a passphrase.</span></span> <span data-ttu-id="531f3-788">Detta är det lösenord som du överför till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-788">This is the passphrase that you upload to the key vault.</span></span>

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. <span data-ttu-id="531f3-790">Slut partitionering.</span><span class="sxs-lookup"><span data-stu-id="531f3-790">Finish partitioning.</span></span>

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. <span data-ttu-id="531f3-792">När du startar den virtuella datorn och ange en lösenfras, använder du det lösenord som du angav i steg 3.</span><span class="sxs-lookup"><span data-stu-id="531f3-792">When you boot the VM and are asked for a passphrase, use the passphrase you provided in step 3.</span></span>

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. <span data-ttu-id="531f3-794">Förbereda den virtuella datorn för överföring till Azure med hjälp av [instruktionerna](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="531f3-794">Prepare the VM for uploading into Azure using [these instructions](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/).</span></span> <span data-ttu-id="531f3-795">Kör inte det sista steget (avetablering VM) ännu.</span><span class="sxs-lookup"><span data-stu-id="531f3-795">Do not run the last step (deprovisioning the VM) yet.</span></span>

<span data-ttu-id="531f3-796">Konfigurera krypteringen ska fungera med Azure genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-796">Configure encryption to work with Azure by doing the following:</span></span>

1. <span data-ttu-id="531f3-797">Skapa en fil under /usr/local/sbin/azure_crypt_key.sh, med innehållet i följande skript.</span><span class="sxs-lookup"><span data-stu-id="531f3-797">Create a file under /usr/local/sbin/azure_crypt_key.sh, with the content in the following script.</span></span> <span data-ttu-id="531f3-798">Vara uppmärksam på KeyFileName, eftersom den är filnamnet lösenfras som används av Azure.</span><span class="sxs-lookup"><span data-stu-id="531f3-798">Pay attention to the KeyFileName, because it is the passphrase file name used by Azure.</span></span>

    ```
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
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
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
```

2. <span data-ttu-id="531f3-799">Ändra crypt config i */etc/crypttab*.</span><span class="sxs-lookup"><span data-stu-id="531f3-799">Change the crypt config in */etc/crypttab*.</span></span> <span data-ttu-id="531f3-800">Det bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="531f3-800">It should look like this:</span></span>
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. <span data-ttu-id="531f3-801">Om du redigerar *azure_crypt_key.sh* i Windows och du kopierade den till Linux, kör `dos2unix /usr/local/sbin/azure_crypt_key.sh`.</span><span class="sxs-lookup"><span data-stu-id="531f3-801">If you are editing *azure_crypt_key.sh* in Windows and you copied it to Linux, run `dos2unix /usr/local/sbin/azure_crypt_key.sh`.</span></span>

4. <span data-ttu-id="531f3-802">Lägg till körrättigheter i skriptet:</span><span class="sxs-lookup"><span data-stu-id="531f3-802">Add executable permissions to the script:</span></span>
 ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
 ```
5. <span data-ttu-id="531f3-803">Redigera */etc/initramfs-tools/modules* genom att lägga till rader:</span><span class="sxs-lookup"><span data-stu-id="531f3-803">Edit */etc/initramfs-tools/modules* by appending lines:</span></span>
 ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
```
6. <span data-ttu-id="531f3-804">Kör `update-initramfs -u -k all` att uppdatera initramfs så att den `keyscript` börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="531f3-804">Run `update-initramfs -u -k all` to update the initramfs to make the `keyscript` take effect.</span></span>

7. <span data-ttu-id="531f3-805">Nu kan du ta bort etableringen den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="531f3-805">Now you can deprovision the VM.</span></span>

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. <span data-ttu-id="531f3-807">Fortsätt till nästa steg och [överför den virtuella Hårddisken](#upload-encrypted-vhd-to-an-azure-storage-account) till Azure.</span><span class="sxs-lookup"><span data-stu-id="531f3-807">Continue to the next step and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

##### <a name="opensuse-132"></a><span data-ttu-id="531f3-808">openSUSE 13.2</span><span class="sxs-lookup"><span data-stu-id="531f3-808">openSUSE 13.2</span></span>
<span data-ttu-id="531f3-809">Om du vill konfigurera kryptering under installationen av distributionsplatsen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-809">To configure encryption during the distribution installation, do the following:</span></span>
1. <span data-ttu-id="531f3-810">När du partitionera diskarna väljer **kryptera volymen grupp**, och sedan ange ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="531f3-810">When you partition the disks, select **Encrypt Volume Group**, and then enter a password.</span></span> <span data-ttu-id="531f3-811">Detta är det lösenord som du överför till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-811">This is the password that you will upload to your key vault.</span></span>

 ![Konfigurera openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. <span data-ttu-id="531f3-813">Starta den virtuella datorn med hjälp av lösenordet.</span><span class="sxs-lookup"><span data-stu-id="531f3-813">Boot the VM using your password.</span></span>

 ![Konfigurera openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. <span data-ttu-id="531f3-815">Förbereda den virtuella datorn för överföring till Azure genom att följa instruktionerna i [förbereda en virtuell dator SLES eller openSUSE för Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131).</span><span class="sxs-lookup"><span data-stu-id="531f3-815">Prepare the VM for uploading to Azure by following the instructions in [Prepare a SLES or openSUSE virtual machine for Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131).</span></span> <span data-ttu-id="531f3-816">Kör inte det sista steget (avetablering VM) ännu.</span><span class="sxs-lookup"><span data-stu-id="531f3-816">Do not run the last step (deprovisioning the VM) yet.</span></span>

<span data-ttu-id="531f3-817">Om du vill konfigurera krypteringen ska fungera med Azure, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-817">To configure encryption to work with Azure, do the following:</span></span>
1. <span data-ttu-id="531f3-818">Redigera /etc/dracut.conf och Lägg till följande rad:</span><span class="sxs-lookup"><span data-stu-id="531f3-818">Edit the /etc/dracut.conf, and add the following line:</span></span>
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. <span data-ttu-id="531f3-819">Kommentera bort dessa linjer i slutet av filen /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span><span class="sxs-lookup"><span data-stu-id="531f3-819">Comment out these lines by the end of the file /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span></span>
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

3. <span data-ttu-id="531f3-820">Lägg till följande rad i början av filen /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span><span class="sxs-lookup"><span data-stu-id="531f3-820">Append the following line at the beginning of the file /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span></span>
 ```
    DRACUT_SYSTEMD=0
 ```
<span data-ttu-id="531f3-821">Och ändra alla förekomster av:</span><span class="sxs-lookup"><span data-stu-id="531f3-821">And change all occurrences of:</span></span>
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
<span data-ttu-id="531f3-822">till:</span><span class="sxs-lookup"><span data-stu-id="531f3-822">to:</span></span>
```
    if [ 1 ]; then
```
4. <span data-ttu-id="531f3-823">Redigera /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh och lägger till dem i ”# öppna LUKS enhet”:</span><span class="sxs-lookup"><span data-stu-id="531f3-823">Edit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh and append it to “# Open LUKS device”:</span></span>

    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
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
5. <span data-ttu-id="531f3-824">Kör `/usr/sbin/dracut -f -v` att uppdatera initrd.</span><span class="sxs-lookup"><span data-stu-id="531f3-824">Run `/usr/sbin/dracut -f -v` to update the initrd.</span></span>

6. <span data-ttu-id="531f3-825">Nu kan du ta bort etableringen den virtuella datorn och [överför den virtuella Hårddisken](#upload-encrypted-vhd-to-an-azure-storage-account) till Azure.</span><span class="sxs-lookup"><span data-stu-id="531f3-825">Now you can deprovision the VM and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

##### <a name="centos-7"></a><span data-ttu-id="531f3-826">CentOS 7</span><span class="sxs-lookup"><span data-stu-id="531f3-826">CentOS 7</span></span>
<span data-ttu-id="531f3-827">Om du vill konfigurera kryptering under installationen av distributionsplatsen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-827">To configure encryption during the distribution installation, do the following:</span></span>
1. <span data-ttu-id="531f3-828">Välj **kryptera data** när du partitionera diskarna.</span><span class="sxs-lookup"><span data-stu-id="531f3-828">Select **Encrypt my data** when you partition disks.</span></span>

 ![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. <span data-ttu-id="531f3-830">Kontrollera att **kryptera** har valts för rot-partitionen.</span><span class="sxs-lookup"><span data-stu-id="531f3-830">Make sure **Encrypt** is selected for root partition.</span></span>

 ![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. <span data-ttu-id="531f3-832">Ange en lösenfras.</span><span class="sxs-lookup"><span data-stu-id="531f3-832">Provide a passphrase.</span></span> <span data-ttu-id="531f3-833">Detta är det lösenord som du överför till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-833">This is the passphrase that you will upload to your key vault.</span></span>

 ![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. <span data-ttu-id="531f3-835">När du startar den virtuella datorn och ange en lösenfras, använder du det lösenord som du angav i steg 3.</span><span class="sxs-lookup"><span data-stu-id="531f3-835">When you boot the VM and are asked for a passphrase, use the passphrase you provided in step 3.</span></span>

 ![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. <span data-ttu-id="531f3-837">Förbereda den virtuella datorn för överföring till Azure med hjälp av anvisningarna i ”CentOS 7.0 +” [förbereda en CentOS-baserad virtuell dator för Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70).</span><span class="sxs-lookup"><span data-stu-id="531f3-837">Prepare the VM for uploading into Azure by using the "CentOS 7.0+" instructions in [Prepare a CentOS-based virtual machine for Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70).</span></span> <span data-ttu-id="531f3-838">Kör inte det sista steget (avetablering VM) ännu.</span><span class="sxs-lookup"><span data-stu-id="531f3-838">Do not run the last step (deprovisioning the VM) yet.</span></span>

6. <span data-ttu-id="531f3-839">Nu kan du ta bort etableringen den virtuella datorn och [överför den virtuella Hårddisken](#upload-encrypted-vhd-to-an-azure-storage-account) till Azure.</span><span class="sxs-lookup"><span data-stu-id="531f3-839">Now you can deprovision the VM and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

<span data-ttu-id="531f3-840">Om du vill konfigurera krypteringen ska fungera med Azure, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="531f3-840">To configure encryption to work with Azure, do the following:</span></span>

1. <span data-ttu-id="531f3-841">Redigera /etc/dracut.conf och Lägg till följande rad:</span><span class="sxs-lookup"><span data-stu-id="531f3-841">Edit the /etc/dracut.conf, and add the following line:</span></span>
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. <span data-ttu-id="531f3-842">Kommentera bort dessa linjer i slutet av filen /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span><span class="sxs-lookup"><span data-stu-id="531f3-842">Comment out these lines by the end of the file /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span></span>
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

3. <span data-ttu-id="531f3-843">Lägg till följande rad i början av filen /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span><span class="sxs-lookup"><span data-stu-id="531f3-843">Append the following line at the beginning of the file /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span></span>
```
    DRACUT_SYSTEMD=0
```
<span data-ttu-id="531f3-844">Och ändra alla förekomster av:</span><span class="sxs-lookup"><span data-stu-id="531f3-844">And change all occurrences of:</span></span>
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
<span data-ttu-id="531f3-845">till</span><span class="sxs-lookup"><span data-stu-id="531f3-845">to</span></span>
```
    if [ 1 ]; then
```
4. <span data-ttu-id="531f3-846">Redigera /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh och Lägg till detta efter ”# öppna LUKS enhet”:</span><span class="sxs-lookup"><span data-stu-id="531f3-846">Edit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh and append this after the “# Open LUKS device”:</span></span>
    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
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
5. <span data-ttu-id="531f3-847">Kör den ”/ usr/sbin/dracut - f - v” att uppdatera initrd.</span><span class="sxs-lookup"><span data-stu-id="531f3-847">Run the “/usr/sbin/dracut -f -v” to update the initrd.</span></span>

![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a><span data-ttu-id="531f3-849">Överföra krypterad VHD till Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="531f3-849">Upload encrypted VHD to an Azure storage account</span></span>
<span data-ttu-id="531f3-850">När BitLocker-kryptering eller DM-Crypt kryptering har aktiverats, måste lokal krypterade VHD överföras till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="531f3-850">After BitLocker encryption or DM-Crypt encryption is enabled, the local encrypted VHD needs to be uploaded to your storage account.</span></span>

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-the-disk-encryption-secret-for-the-pre-encrypted-vm-to-your-key-vault"></a><span data-ttu-id="531f3-851">Ladda upp hemligheten som disk-kryptering för förkrypterade VM till nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="531f3-851">Upload the disk-encryption secret for the pre-encrypted VM to your key vault</span></span>
<span data-ttu-id="531f3-852">Den hemlighet som disk-kryptering som du hämtade måste tidigare laddas upp som en hemlighet i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-852">The disk-encryption secret that you obtained previously must be uploaded as a secret in your key vault.</span></span> <span data-ttu-id="531f3-853">Nyckelvalvet måste ha behörighet som aktiverats för din Azure AD-klient och kryptering.</span><span class="sxs-lookup"><span data-stu-id="531f3-853">The key vault needs to have disk encryption and permissions enabled for your Azure AD client.</span></span>

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a><span data-ttu-id="531f3-854">Disk encryption hemlighet inte krypterats med en KEK</span><span class="sxs-lookup"><span data-stu-id="531f3-854">Disk encryption secret not encrypted with a KEK</span></span>
<span data-ttu-id="531f3-855">Om du vill konfigurera hemlighet i nyckelvalvet, använda [Set AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret).</span><span class="sxs-lookup"><span data-stu-id="531f3-855">To set up the secret in your key vault, use [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret).</span></span> <span data-ttu-id="531f3-856">Om du har en Windows-dator kan filen bek kodas som en base64-sträng och sedan överförs till nyckelvalvet med hjälp av den `Set-AzureKeyVaultSecret` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="531f3-856">If you have a Windows virtual machine, the bek file is encoded as a base64 string and then uploaded to your key vault using the `Set-AzureKeyVaultSecret` cmdlet.</span></span> <span data-ttu-id="531f3-857">För Linux lösenfrasen kodas som en base64-sträng och sedan har överförts till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-857">For Linux, the passphrase is encoded as a base64 string and then uploaded to the key vault.</span></span> <span data-ttu-id="531f3-858">Kontrollera dessutom att följande taggar anges när du skapar hemligheten i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="531f3-858">In addition, make sure that the following tags are set when you create the secret in the key vault.</span></span>

    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

<span data-ttu-id="531f3-859">Använd den `$secretUrl` i nästa steg för [kopplar OS-disk utan att använda KEK](#without-using-a-kek).</span><span class="sxs-lookup"><span data-stu-id="531f3-859">Use the `$secretUrl` in the next step for [attaching the OS disk without using KEK](#without-using-a-kek).</span></span>

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a><span data-ttu-id="531f3-860">Disk encryption hemlighet krypteras med en KEK</span><span class="sxs-lookup"><span data-stu-id="531f3-860">Disk encryption secret encrypted with a KEK</span></span>
<span data-ttu-id="531f3-861">Innan du laddar upp hemligheten nyckelvalvet kryptera om du vill den med hjälp av en nyckel krypteringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="531f3-861">Before you upload the secret to the key vault, you can optionally encrypt it by using a key encryption key.</span></span> <span data-ttu-id="531f3-862">Använd radbyte [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) att först kryptera hemligheten med hjälp av nyckeln krypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="531f3-862">Use the wrap [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) to first encrypt the secret using the key encryption key.</span></span> <span data-ttu-id="531f3-863">Utdata från åtgärden radbyte är en base64-URL: en kodad sträng som du kan sedan överföra som en hemlighet med hjälp av den [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="531f3-863">The output of this wrap operation is a base64 URL encoded string, which you can then upload as a secret by using the [`Set-AzureKeyVaultSecret`](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet.</span></span>

    # This is the passphrase that was provided for encryption during the distribution installation
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

<span data-ttu-id="531f3-864">Använd `$KeyEncryptionKey` och `$secretUrl` i nästa steg för [kopplar OS-disk med hjälp av KEK](#using-a-kek).</span><span class="sxs-lookup"><span data-stu-id="531f3-864">Use `$KeyEncryptionKey` and `$secretUrl` in the next step for [attaching the OS disk using KEK](#using-a-kek).</span></span>

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a><span data-ttu-id="531f3-865">Ange en hemlig URL när du ansluter en OS-disk</span><span class="sxs-lookup"><span data-stu-id="531f3-865">Specify a secret URL when you attach an OS disk</span></span>
#### <a name="without-using-a-kek"></a><span data-ttu-id="531f3-866">Utan att använda en KEK</span><span class="sxs-lookup"><span data-stu-id="531f3-866">Without using a KEK</span></span>
<span data-ttu-id="531f3-867">När du bifogar OS-disk, måste du skicka `$secretUrl`.</span><span class="sxs-lookup"><span data-stu-id="531f3-867">While you are attaching the OS disk, you need to pass `$secretUrl`.</span></span> <span data-ttu-id="531f3-868">URL: en har genererats i avsnittet ”Disk encryption hemlighet inte krypterats med en KEK”.</span><span class="sxs-lookup"><span data-stu-id="531f3-868">The URL was generated in the "Disk-encryption secret not encrypted with a KEK" section.</span></span>

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a><span data-ttu-id="531f3-869">Med hjälp av en KEK</span><span class="sxs-lookup"><span data-stu-id="531f3-869">Using a KEK</span></span>
<span data-ttu-id="531f3-870">När du ansluter OS-disk, skicka `$KeyEncryptionKey` och `$secretUrl`.</span><span class="sxs-lookup"><span data-stu-id="531f3-870">When you attach the OS disk, pass `$KeyEncryptionKey` and `$secretUrl`.</span></span> <span data-ttu-id="531f3-871">URL: en har genererats i avsnittet ”Disk encryption hemlighet inte krypterats med en KEK”.</span><span class="sxs-lookup"><span data-stu-id="531f3-871">The URL was generated in the "Disk-encryption secret not encrypted with a KEK" section.</span></span>

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

## <a name="download-this-guide"></a><span data-ttu-id="531f3-872">Hämta denna guide</span><span class="sxs-lookup"><span data-stu-id="531f3-872">Download this guide</span></span>
<span data-ttu-id="531f3-873">Du kan hämta den här guiden från den [TechNet-galleriet](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).</span><span class="sxs-lookup"><span data-stu-id="531f3-873">You can download this guide from the [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).</span></span>

## <a name="for-more-information"></a><span data-ttu-id="531f3-874">Mer information</span><span class="sxs-lookup"><span data-stu-id="531f3-874">For more information</span></span>
[<span data-ttu-id="531f3-875">Utforska Azure Disk Encryption med Azure PowerShell - del 1</span><span class="sxs-lookup"><span data-stu-id="531f3-875">Explore Azure Disk Encryption with Azure PowerShell - Part 1</span></span>](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)  
[<span data-ttu-id="531f3-876">Utforska Azure Disk Encryption med Azure PowerShell - del 2</span><span class="sxs-lookup"><span data-stu-id="531f3-876">Explore Azure Disk Encryption with Azure PowerShell - Part 2</span></span>](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
