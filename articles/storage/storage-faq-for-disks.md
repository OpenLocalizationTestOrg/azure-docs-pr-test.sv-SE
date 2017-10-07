---
title: "Vanliga frågor (FAQ) om Azure IaaS-VM-diskarna | Microsoft Docs"
description: "Vanliga frågor och svar om Azure IaaS-VM och premiumdiskar (hanterade och ohanterade)"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: 31d0aa67b6ca58b75b432ae94f93ebcf6d730380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a><span data-ttu-id="787ee-103">Vanliga frågor och svar om Azure IaaS-VM och hanterade och ohanterade premiumdiskar</span><span class="sxs-lookup"><span data-stu-id="787ee-103">Frequently asked questions about Azure IaaS VM disks and managed and unmanaged premium disks</span></span>

<span data-ttu-id="787ee-104">Den här artikeln besvarar några vanliga frågor om Azure hanterade diskar och Azure Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="787ee-104">This article answers some frequently asked questions about Azure Managed Disks and Azure Premium Storage.</span></span>

## <a name="managed-disks"></a><span data-ttu-id="787ee-105">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="787ee-105">Managed Disks</span></span>

<span data-ttu-id="787ee-106">**Vad är Azure hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-106">**What is Azure Managed Disks?**</span></span>

<span data-ttu-id="787ee-107">Hanterade diskar är en funktion som förenklar Diskhantering för virtuella Azure IaaS-datorer genom att hantera lagringshantering konto för dig.</span><span class="sxs-lookup"><span data-stu-id="787ee-107">Managed Disks is a feature that simplifies disk management for Azure IaaS VMs by handling storage account management for you.</span></span> <span data-ttu-id="787ee-108">Mer information finns i hello [översikt för hanterade diskar](storage-managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="787ee-108">For more information, see hello [Managed Disks overview](storage-managed-disks-overview.md).</span></span>

<span data-ttu-id="787ee-109">**Om jag skapa en standard hanterade diskar från en befintlig virtuell Hårddisk som är 80 GB, hur mycket som kostar mig?**</span><span class="sxs-lookup"><span data-stu-id="787ee-109">**If I create a standard managed disk from an existing VHD that's 80 GB, how much will that cost me?**</span></span>

<span data-ttu-id="787ee-110">En standard hanterade diskar som skapats från en virtuell Hårddisk på 80 GB behandlas som hello nästa tillgängliga standard diskutrymme, vilket är en S10 disk.</span><span class="sxs-lookup"><span data-stu-id="787ee-110">A standard managed disk created from an 80-GB VHD is treated as hello next available standard disk size, which is an S10 disk.</span></span> <span data-ttu-id="787ee-111">Du debiteras är bl.a toohello S10 disk priser.</span><span class="sxs-lookup"><span data-stu-id="787ee-111">You're charged according toohello S10 disk pricing.</span></span> <span data-ttu-id="787ee-112">Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="787ee-112">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="787ee-113">**Finns det några transaktionskostnader för hanterade standarddiskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-113">**Are there any transaction costs for standard managed disks?**</span></span>

<span data-ttu-id="787ee-114">Ja.</span><span class="sxs-lookup"><span data-stu-id="787ee-114">Yes.</span></span> <span data-ttu-id="787ee-115">Du är debiteras för varje transaktion.</span><span class="sxs-lookup"><span data-stu-id="787ee-115">You're charged for each transaction.</span></span> <span data-ttu-id="787ee-116">Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="787ee-116">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="787ee-117">**För en standard hanterade disk jag debiteras för hello verkliga storleken hos hello data på hello eller hello diskens hello etablerad kapacitet?**</span><span class="sxs-lookup"><span data-stu-id="787ee-117">**For a standard managed disk, will I be charged for hello actual size of hello data on hello disk or for hello provisioned capacity of hello disk?**</span></span>

<span data-ttu-id="787ee-118">Du är debiteras baserat på hello diskens hello etablerad kapacitet.</span><span class="sxs-lookup"><span data-stu-id="787ee-118">You're charged based on hello provisioned capacity of hello disk.</span></span> <span data-ttu-id="787ee-119">Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="787ee-119">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="787ee-120">**Hur är prissättningen av hanterade premiumdiskar skiljer sig från ohanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-120">**How is pricing of premium managed disks different from unmanaged disks?**</span></span>

<span data-ttu-id="787ee-121">hello priser för premiumdiskar hanteras är hello samma som ohanterad premiumdiskar.</span><span class="sxs-lookup"><span data-stu-id="787ee-121">hello pricing of premium managed disks is hello same as unmanaged premium disks.</span></span>

<span data-ttu-id="787ee-122">**Kan jag ändra hello lagringskontotypen (Standard eller Premium) av min hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-122">**Can I change hello storage account type (Standard or Premium) of my managed disks?**</span></span>

<span data-ttu-id="787ee-123">Ja.</span><span class="sxs-lookup"><span data-stu-id="787ee-123">Yes.</span></span> <span data-ttu-id="787ee-124">Du kan ändra hello lagringskontotypen hanterade diskar med hjälp av hello Azure-portalen, PowerShell eller hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="787ee-124">You can change hello storage account type of your managed disks by using hello Azure portal, PowerShell, or hello Azure CLI.</span></span>

<span data-ttu-id="787ee-125">**Finns det ett sätt att jag kan kopiera eller exportera ett hanterade diskar tooa privata storage-konto?**</span><span class="sxs-lookup"><span data-stu-id="787ee-125">**Is there a way that I can copy or export a managed disk tooa private storage account?**</span></span>

<span data-ttu-id="787ee-126">Ja.</span><span class="sxs-lookup"><span data-stu-id="787ee-126">Yes.</span></span> <span data-ttu-id="787ee-127">Du kan exportera dina hanterade diskar med hjälp av hello Azure-portalen, PowerShell eller hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="787ee-127">You can export your managed disks by using hello Azure portal, PowerShell, or hello Azure CLI.</span></span>

<span data-ttu-id="787ee-128">**Kan jag använda en VHD-fil i ett Azure storage-konto toocreate hanterade diskar med en annan prenumeration?**</span><span class="sxs-lookup"><span data-stu-id="787ee-128">**Can I use a VHD file in an Azure storage account toocreate a managed disk with a different subscription?**</span></span>

<span data-ttu-id="787ee-129">Nej.</span><span class="sxs-lookup"><span data-stu-id="787ee-129">No.</span></span>

<span data-ttu-id="787ee-130">**Kan jag använda en VHD-fil i ett Azure storage-konto toocreate hanterade diskar i en annan region?**</span><span class="sxs-lookup"><span data-stu-id="787ee-130">**Can I use a VHD file in an Azure storage account toocreate a managed disk in a different region?**</span></span>

<span data-ttu-id="787ee-131">Nej.</span><span class="sxs-lookup"><span data-stu-id="787ee-131">No.</span></span>

<span data-ttu-id="787ee-132">**Finns det några begränsningar för skalan för kunder som använder hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-132">**Are there any scale limitations for customers that use managed disks?**</span></span>

<span data-ttu-id="787ee-133">Hanterade diskar eliminerar hello gränser som är kopplade till storage-konton.</span><span class="sxs-lookup"><span data-stu-id="787ee-133">Managed Disks eliminates hello limits associated with storage accounts.</span></span> <span data-ttu-id="787ee-134">Hello antalet hanterade diskar per prenumeration är dock begränsad too2, 000 som standard.</span><span class="sxs-lookup"><span data-stu-id="787ee-134">However, hello number of managed disks per subscription is limited too2,000 by default.</span></span> <span data-ttu-id="787ee-135">Du kan anropa stöd tooincrease numret.</span><span class="sxs-lookup"><span data-stu-id="787ee-135">You can call support tooincrease this number.</span></span>

<span data-ttu-id="787ee-136">**Kan jag göra en inkrementell ögonblicksbild av hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-136">**Can I take an incremental snapshot of a managed disk?**</span></span>

<span data-ttu-id="787ee-137">Nej.</span><span class="sxs-lookup"><span data-stu-id="787ee-137">No.</span></span> <span data-ttu-id="787ee-138">hello gör aktuella ögonblicksbild en fullständig kopia av hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="787ee-138">hello current snapshot capability makes a full copy of a managed disk.</span></span> <span data-ttu-id="787ee-139">Men planerar vi toosupport inkrementell ögonblicksbilder i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="787ee-139">However, we are planning toosupport incremental snapshots in hello future.</span></span>

<span data-ttu-id="787ee-140">**Virtuella datorer i en tillgänglighetsuppsättning kan bestå av en kombination av hanterade och ohanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-140">**Can VMs in an availability set consist of a combination of managed and unmanaged disks?**</span></span>

<span data-ttu-id="787ee-141">Nej.</span><span class="sxs-lookup"><span data-stu-id="787ee-141">No.</span></span> <span data-ttu-id="787ee-142">hello virtuella datorer i en tillgänglighetsuppsättning måste använda alla hanterade diskar eller ohanterad diskarna.</span><span class="sxs-lookup"><span data-stu-id="787ee-142">hello VMs in an availability set must use either all managed disks or all unmanaged disks.</span></span> <span data-ttu-id="787ee-143">När du skapar en tillgänglighetsuppsättning, kan du välja vilken typ av diskar du vill använda toouse.</span><span class="sxs-lookup"><span data-stu-id="787ee-143">When you create an availability set, you can choose which type of disks you want toouse.</span></span>

<span data-ttu-id="787ee-144">**Är standardalternativet för hanterade diskar hello hello Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="787ee-144">**Is Managed Disks hello default option in hello Azure portal?**</span></span>

<span data-ttu-id="787ee-145">För närvarande inte, men det blir hello standard i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="787ee-145">Not currently, but it will become hello default in hello future.</span></span>

<span data-ttu-id="787ee-146">**Kan jag skapa en tom disk som hanterade?**</span><span class="sxs-lookup"><span data-stu-id="787ee-146">**Can I create an empty managed disk?**</span></span>

<span data-ttu-id="787ee-147">Ja.</span><span class="sxs-lookup"><span data-stu-id="787ee-147">Yes.</span></span> <span data-ttu-id="787ee-148">Du kan skapa en tom disk.</span><span class="sxs-lookup"><span data-stu-id="787ee-148">You can create an empty disk.</span></span> <span data-ttu-id="787ee-149">Hanterade diskar kan skapas oberoende av en virtuell dator, till exempel utan kopplar den tooa VM.</span><span class="sxs-lookup"><span data-stu-id="787ee-149">A managed disk can be created independently of a VM, for example, without attaching it tooa VM.</span></span>

<span data-ttu-id="787ee-150">**Vad är hello stöds feldomänsantalet för en tillgänglighetsuppsättning som använder hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-150">**What is hello supported fault domain count for an availability set that uses Managed Disks?**</span></span>

<span data-ttu-id="787ee-151">Beroende på hello region där hello tillgänglighetsuppsättningen som använder hanterade diskar finns, är hello stöds feldomänsantalet 2 eller 3.</span><span class="sxs-lookup"><span data-stu-id="787ee-151">Depending on hello region where hello availability set that uses Managed Disks is located, hello supported fault domain count is 2 or 3.</span></span>

<span data-ttu-id="787ee-152">**Hur är hello standard lagringskontot för diagnostik konfigurera?**</span><span class="sxs-lookup"><span data-stu-id="787ee-152">**How is hello standard storage account for diagnostics set up?**</span></span>

<span data-ttu-id="787ee-153">Du kan konfigurera ett konto för privat lagring för diagnostik för Virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="787ee-153">You set up a private storage account for VM diagnostics.</span></span> <span data-ttu-id="787ee-154">I framtida hello, planerar vi tooswitch diagnostik tooManaged-diskar.</span><span class="sxs-lookup"><span data-stu-id="787ee-154">In hello future, we plan tooswitch diagnostics tooManaged Disks as well.</span></span>

<span data-ttu-id="787ee-155">**Vilken typ av rollbaserad åtkomstkontroll support är tillgänglig för hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-155">**What kind of Role-Based Access Control support is available for Managed Disks?**</span></span>

<span data-ttu-id="787ee-156">Hanterade diskar stöder tre viktiga standardroller:</span><span class="sxs-lookup"><span data-stu-id="787ee-156">Managed Disks supports three key default roles:</span></span>

* <span data-ttu-id="787ee-157">Ägare: Kan hantera allt, inklusive åtkomst</span><span class="sxs-lookup"><span data-stu-id="787ee-157">Owner: Can manage everything, including access</span></span>
* <span data-ttu-id="787ee-158">Deltagare: Kan hantera allt utom åtkomst</span><span class="sxs-lookup"><span data-stu-id="787ee-158">Contributor: Can manage everything except access</span></span>
* <span data-ttu-id="787ee-159">Läsare: Kan visa allt, men det går inte att göra ändringar</span><span class="sxs-lookup"><span data-stu-id="787ee-159">Reader: Can view everything, but can't make changes</span></span>

<span data-ttu-id="787ee-160">**Finns det ett sätt att jag kan kopiera eller exportera ett hanterade diskar tooa privata storage-konto?**</span><span class="sxs-lookup"><span data-stu-id="787ee-160">**Is there a way that I can copy or export a managed disk tooa private storage account?**</span></span>

<span data-ttu-id="787ee-161">Du kan hämta en skrivskyddad delad åtkomstsignatur URI för hello hanterade disken och använda den toocopy hello innehållet tooa privat lagring konto eller lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="787ee-161">You can get a read-only shared access signature URI for hello managed disk and use it toocopy hello contents tooa private storage account or on-premises storage.</span></span>

<span data-ttu-id="787ee-162">**Kan jag skapa en kopia av min hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-162">**Can I create a copy of my managed disk?**</span></span>

<span data-ttu-id="787ee-163">Kunder kan ta en ögonblicksbild av deras hanterade diskar och sedan använda hello ögonblicksbild toocreate en annan hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="787ee-163">Customers can take a snapshot of their managed disks and then use hello snapshot toocreate another managed disk.</span></span>

<span data-ttu-id="787ee-164">**Ohanterad diskar stöds fortfarande?**</span><span class="sxs-lookup"><span data-stu-id="787ee-164">**Are unmanaged disks still supported?**</span></span>

<span data-ttu-id="787ee-165">Ja.</span><span class="sxs-lookup"><span data-stu-id="787ee-165">Yes.</span></span> <span data-ttu-id="787ee-166">Vi stöder ohanterade och hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="787ee-166">We support unmanaged and managed disks.</span></span> <span data-ttu-id="787ee-167">Vi rekommenderar att du använder hanterade diskar för nya arbetsbelastningar och migrera dina aktuella arbetsbelastningar toomanaged diskar.</span><span class="sxs-lookup"><span data-stu-id="787ee-167">We recommend that you use managed disks for new workloads and migrate your current workloads toomanaged disks.</span></span>


<span data-ttu-id="787ee-168">**Om jag skapa en 128 GB disk och sedan öka hello storlek too130 GB jag debiteras för hello nästa diskstorlek (512 GB)?**</span><span class="sxs-lookup"><span data-stu-id="787ee-168">**If I create a 128-GB disk and then increase hello size too130 GB, will I be charged for hello next disk size (512 GB)?**</span></span>

<span data-ttu-id="787ee-169">Ja.</span><span class="sxs-lookup"><span data-stu-id="787ee-169">Yes.</span></span>

<span data-ttu-id="787ee-170">**Kan jag skapa lokalt redundant lagring, geo-redundant lagring och zonredundant lagring hanteras diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-170">**Can I create locally redundant storage, geo-redundant storage, and zone-redundant storage managed disks?**</span></span>

<span data-ttu-id="787ee-171">Azure-hanterade diskar stöder för närvarande endast lokalt redundant lagring hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="787ee-171">Azure Managed Disks currently supports only locally redundant storage managed disks.</span></span>

<span data-ttu-id="787ee-172">**Kan jag krympa eller downsize min hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-172">**Can I shrink or downsize my managed disks?**</span></span>

<span data-ttu-id="787ee-173">Nej.</span><span class="sxs-lookup"><span data-stu-id="787ee-173">No.</span></span> <span data-ttu-id="787ee-174">Den här funktionen stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="787ee-174">This feature is not supported currently.</span></span> 

<span data-ttu-id="787ee-175">**Kan jag ändra hello namnegenskapen för datorn när en särskild (inte skapats med hjälp av hello systemförberedelseverktyget eller generaliserad) systemdisken är används tooprovision en virtuell dator?**</span><span class="sxs-lookup"><span data-stu-id="787ee-175">**Can I change hello computer name property when a specialized (not created by using hello System Preparation tool or generalized) operating system disk is used tooprovision a VM?**</span></span>

<span data-ttu-id="787ee-176">Nej.</span><span class="sxs-lookup"><span data-stu-id="787ee-176">No.</span></span> <span data-ttu-id="787ee-177">Du kan inte uppdatera hello namnegenskapen för datorn.</span><span class="sxs-lookup"><span data-stu-id="787ee-177">You can't update hello computer name property.</span></span> <span data-ttu-id="787ee-178">hello ärver ny virtuell dator den från hello överordnade VM, som har använt toocreate hello operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="787ee-178">hello new VM inherits it from hello parent VM, which was used toocreate hello operating system disk.</span></span> 

<span data-ttu-id="787ee-179">**Var hittar jag exempel Azure Resource Manager-mallar toocreate virtuella datorer med hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-179">**Where can I find sample Azure Resource Manager templates toocreate VMs with managed disks?**</span></span>
* [<span data-ttu-id="787ee-180">Lista över mallar med hjälp av hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="787ee-180">List of templates using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="787ee-181">https://github.com/chagarw/MDPP</span><span class="sxs-lookup"><span data-stu-id="787ee-181">https://github.com/chagarw/MDPP</span></span>

## <a name="managed-disks-and-storage-service-encryption"></a><span data-ttu-id="787ee-182">Hanterade diskar och Storage Service-kryptering</span><span class="sxs-lookup"><span data-stu-id="787ee-182">Managed Disks and Storage Service Encryption</span></span> 

<span data-ttu-id="787ee-183">**Är Azure Storage Service-kryptering aktiverat som standard när jag skapar hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-183">**Is Azure Storage Service Encryption enabled by default when I create a managed disk?**</span></span>

<span data-ttu-id="787ee-184">Ja.</span><span class="sxs-lookup"><span data-stu-id="787ee-184">Yes.</span></span>

<span data-ttu-id="787ee-185">**Som hanterar hello krypteringsnycklar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-185">**Who manages hello encryption keys?**</span></span>

<span data-ttu-id="787ee-186">Microsoft hanterar hello krypteringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="787ee-186">Microsoft manages hello encryption keys.</span></span>

<span data-ttu-id="787ee-187">**Kan jag inaktivera Storage Service-kryptering för min hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-187">**Can I disable Storage Service Encryption for my managed disks?**</span></span>

<span data-ttu-id="787ee-188">Nej.</span><span class="sxs-lookup"><span data-stu-id="787ee-188">No.</span></span>

<span data-ttu-id="787ee-189">**Är Lagringstjänstens kryptering endast tillgänglig i vissa områden?**</span><span class="sxs-lookup"><span data-stu-id="787ee-189">**Is Storage Service Encryption only available in specific regions?**</span></span>

<span data-ttu-id="787ee-190">Nej.</span><span class="sxs-lookup"><span data-stu-id="787ee-190">No.</span></span> <span data-ttu-id="787ee-191">Den är tillgänglig i alla hello regioner där hanterade diskar är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="787ee-191">It's available in all hello regions where Managed Disks is available.</span></span> <span data-ttu-id="787ee-192">Hanterade diskar är tillgänglig i alla offentliga regioner och Tyskland.</span><span class="sxs-lookup"><span data-stu-id="787ee-192">Managed Disks is available in all public regions and Germany.</span></span>

<span data-ttu-id="787ee-193">**Hur kan jag ta reda om hanterade disken krypteras?**</span><span class="sxs-lookup"><span data-stu-id="787ee-193">**How can I find out if my managed disk is encrypted?**</span></span>

<span data-ttu-id="787ee-194">Du kan ta reda hello tid då en hanterad disk skapades från hello Azure-portalen, hello Azure CLI och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="787ee-194">You can find out hello time when a managed disk was created from hello Azure portal, hello Azure CLI, and PowerShell.</span></span> <span data-ttu-id="787ee-195">Om hello är senare än den 9 juni 2017 krypteras disken.</span><span class="sxs-lookup"><span data-stu-id="787ee-195">If hello time is after June 9, 2017, then your disk is encrypted.</span></span> 

<span data-ttu-id="787ee-196">**Hur kan jag kryptera Mina befintliga diskar som skapades före 10 juni 2017?**</span><span class="sxs-lookup"><span data-stu-id="787ee-196">**How can I encrypt my existing disks that were created before June 10, 2017?**</span></span>

<span data-ttu-id="787ee-197">Från och med 10 juni 2017 krypteras automatiskt nya data skrivs tooexisting hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="787ee-197">As of June 10, 2017, new data written tooexisting managed disks is automatically encrypted.</span></span> <span data-ttu-id="787ee-198">Vi också planerar tooencrypt befintliga data och hello kryptering sker asynkront i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="787ee-198">We are also planning tooencrypt existing data, and hello encryption will happen asynchronously in hello background.</span></span> <span data-ttu-id="787ee-199">Om du måste nu krypterar befintliga data måste du skapa en kopia av disken.</span><span class="sxs-lookup"><span data-stu-id="787ee-199">If you must encrypt existing data now, create a copy of your disk.</span></span> <span data-ttu-id="787ee-200">Nya diskar krypteras.</span><span class="sxs-lookup"><span data-stu-id="787ee-200">New disks will be encrypted.</span></span>

* [<span data-ttu-id="787ee-201">Kopiera hanterade diskar med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="787ee-201">Copy managed disks by using hello Azure CLI</span></span>](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)
* [<span data-ttu-id="787ee-202">Kopiera hanterade diskar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="787ee-202">Copy managed disks by using PowerShell</span></span>](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="787ee-203">**Är hanterad ögonblicksbilder och bilder som krypteras?**</span><span class="sxs-lookup"><span data-stu-id="787ee-203">**Are managed snapshots and images encrypted?**</span></span>

<span data-ttu-id="787ee-204">Ja.</span><span class="sxs-lookup"><span data-stu-id="787ee-204">Yes.</span></span> <span data-ttu-id="787ee-205">Alla hanterade ögonblicksbilder och bilder som skapats efter den 9 juni 2017 krypteras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="787ee-205">All managed snapshots and images created after June 9, 2017, are automatically encrypted.</span></span> 

<span data-ttu-id="787ee-206">**Kan jag konvertera virtuella datorer med ohanterad diskar som finns på lagringskonton som är eller fanns tidigare krypterade toomanaged diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-206">**Can I convert VMs with unmanaged disks that are located on storage accounts that are or were previously encrypted toomanaged disks?**</span></span>

<span data-ttu-id="787ee-207">Ja</span><span class="sxs-lookup"><span data-stu-id="787ee-207">Yes</span></span>

<span data-ttu-id="787ee-208">**En exporterad VHD från en hanterad disk eller en ögonblicksbild även krypteras?**</span><span class="sxs-lookup"><span data-stu-id="787ee-208">**Will an exported VHD from a managed disk or a snapshot also be encrypted?**</span></span>

<span data-ttu-id="787ee-209">Nej.</span><span class="sxs-lookup"><span data-stu-id="787ee-209">No.</span></span> <span data-ttu-id="787ee-210">Men om du exportera en VHD-tooan krypterade lagringskonto från en krypterad hanterade diskar eller ögonblicksbild och är krypterad.</span><span class="sxs-lookup"><span data-stu-id="787ee-210">But if you export a VHD tooan encrypted storage account from an encrypted managed disk or snapshot, then it's encrypted.</span></span> 

## <a name="premium-disks-managed-and-unmanaged"></a><span data-ttu-id="787ee-211">Premiumdiskar: hanterade och ohanterade</span><span class="sxs-lookup"><span data-stu-id="787ee-211">Premium disks: Managed and unmanaged</span></span>

<span data-ttu-id="787ee-212">**Om en virtuell dator använder en serie med storlek som har stöd för Premium-lagring, till exempel en DSv2 kan jag koppla premium- och datadiskar som standard?**</span><span class="sxs-lookup"><span data-stu-id="787ee-212">**If a VM uses a size series that supports Premium Storage, such as a DSv2, can I attach both premium and standard data disks?**</span></span> 

<span data-ttu-id="787ee-213">Ja.</span><span class="sxs-lookup"><span data-stu-id="787ee-213">Yes.</span></span>

<span data-ttu-id="787ee-214">**Kan jag koppla premium- och standard diskar tooa storlek dataserie som inte stöder Premium-lagring, till exempel D, Dv2, G eller F serien?**</span><span class="sxs-lookup"><span data-stu-id="787ee-214">**Can I attach both premium and standard data disks tooa size series that doesn't support Premium Storage, such as D, Dv2, G, or F series?**</span></span>

<span data-ttu-id="787ee-215">Nej.</span><span class="sxs-lookup"><span data-stu-id="787ee-215">No.</span></span> <span data-ttu-id="787ee-216">Du kan koppla bara standarddata diskar tooVMs som inte använder en serie med storlek som har stöd för Premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="787ee-216">You can attach only standard data disks tooVMs that don't use a size series that supports Premium Storage.</span></span>

<span data-ttu-id="787ee-217">**Om jag skapa en disk för premium-data från en befintlig virtuell Hårddisk som är 80 GB, hur mycket som kostar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-217">**If I create a premium data disk from an existing VHD that was 80 GB, how much will that cost?**</span></span>

<span data-ttu-id="787ee-218">En disk för premium-data som skapas från en virtuell Hårddisk på 80 GB behandlas som hello nästa tillgängliga premium-diskstorleken, vilket är en P10 disk.</span><span class="sxs-lookup"><span data-stu-id="787ee-218">A premium data disk created from an 80-GB VHD is treated as hello next-available premium disk size, which is a P10 disk.</span></span> <span data-ttu-id="787ee-219">Du debiteras är bl.a toohello P10 disk priser.</span><span class="sxs-lookup"><span data-stu-id="787ee-219">You're charged according toohello P10 disk pricing.</span></span>

<span data-ttu-id="787ee-220">**Transaktionen kostnader toouse Premium-lagring finns?**</span><span class="sxs-lookup"><span data-stu-id="787ee-220">**Are there transaction costs toouse Premium Storage?**</span></span>

<span data-ttu-id="787ee-221">Det finns en fast kostnad för varje diskstorlek som hämtas etablerade med specifika gränser på IOPS och genomflöde.</span><span class="sxs-lookup"><span data-stu-id="787ee-221">There is a fixed cost for each disk size, which comes provisioned with specific limits on IOPS and throughput.</span></span> <span data-ttu-id="787ee-222">hello är övriga kostnader utgående bandbredd och ögonblicksbild kapacitet, om tillämpligt.</span><span class="sxs-lookup"><span data-stu-id="787ee-222">hello other costs are outbound bandwidth and snapshot capacity, if applicable.</span></span> <span data-ttu-id="787ee-223">Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="787ee-223">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="787ee-224">**Vad är hello gränser för IOPS och dataflöde som jag kan från hello diskcache?**</span><span class="sxs-lookup"><span data-stu-id="787ee-224">**What are hello limits for IOPS and throughput that I can get from hello disk cache?**</span></span>

<span data-ttu-id="787ee-225">Hej kombinerade gränser för cache och lokala SSD för DS-serien är 4 000 IOPS per kärna och 33 MB per sekund per kärna.</span><span class="sxs-lookup"><span data-stu-id="787ee-225">hello combined limits for cache and local SSD for a DS series are 4,000 IOPS per core and 33 MB per second per core.</span></span> <span data-ttu-id="787ee-226">hello GS-serien ger 5 000 IOPS per kärna och 50 MB per sekund per kärna.</span><span class="sxs-lookup"><span data-stu-id="787ee-226">hello GS series offers 5,000 IOPS per core and 50 MB per second per core.</span></span>

<span data-ttu-id="787ee-227">**Är hello lokala SSD stöds för en virtuell för hanterade diskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-227">**Is hello local SSD supported for a Managed Disks VM?**</span></span>

<span data-ttu-id="787ee-228">hello är lokala SSD tillfälligt lagringsutrymme som ingår i en hanterad diskar i virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="787ee-228">hello local SSD is temporary storage that is included with a Managed Disks VM.</span></span> <span data-ttu-id="787ee-229">Det finns inget extra kostnad för den här tillfällig lagring.</span><span class="sxs-lookup"><span data-stu-id="787ee-229">There is no extra cost for this temporary storage.</span></span> <span data-ttu-id="787ee-230">Vi rekommenderar att du inte använder den här lokala SSD toostore dina programdata eftersom det inte finns kvar i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="787ee-230">We recommend that you do not use this local SSD toostore your application data because it isn't persisted in Azure Blob storage.</span></span>

<span data-ttu-id="787ee-231">**Finns det några konsekvenser för hello använda trim på premiumdiskar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-231">**Are there any repercussions for hello use of TRIM on premium disks?**</span></span>

<span data-ttu-id="787ee-232">Det finns ingen Nackdelen med toohello användning av TRIMNING på Azure-diskar på antingen premium eller standarddiskar.</span><span class="sxs-lookup"><span data-stu-id="787ee-232">There is no downside toohello use of TRIM on Azure disks on either premium or standard disks.</span></span>

## <a name="new-disk-sizes-managed-and-unmanaged"></a><span data-ttu-id="787ee-233">Nya diskstorlekar: hanterade och ohanterade</span><span class="sxs-lookup"><span data-stu-id="787ee-233">New disk sizes: Managed and unmanaged</span></span>

<span data-ttu-id="787ee-234">**Vad är hello största diskstorleken kan användas för operativsystemet och datadiskarna?**</span><span class="sxs-lookup"><span data-stu-id="787ee-234">**What is hello largest disk size supported for operating system and data disks?**</span></span>

<span data-ttu-id="787ee-235">hello partitionstypen som Azure har stöd för en operativsystemdisk är hello master boot record (MBR).</span><span class="sxs-lookup"><span data-stu-id="787ee-235">hello partition type that Azure supports for an operating system disk is hello master boot record (MBR).</span></span> <span data-ttu-id="787ee-236">hello MBR-formatet stöder en diskstorlek in too2 TB.</span><span class="sxs-lookup"><span data-stu-id="787ee-236">hello MBR format supports a disk size up too2 TB.</span></span> <span data-ttu-id="787ee-237">hello största storlek som Azure har stöd för en operativsystemdisk är 2 TB.</span><span class="sxs-lookup"><span data-stu-id="787ee-237">hello largest size that Azure supports for an operating system disk is 2 TB.</span></span> <span data-ttu-id="787ee-238">Azure stöder upp too4 TB för datadiskar.</span><span class="sxs-lookup"><span data-stu-id="787ee-238">Azure supports up too4 TB for data disks.</span></span> 

<span data-ttu-id="787ee-239">**Vad är hello största blob sidstorlek som stöds?**</span><span class="sxs-lookup"><span data-stu-id="787ee-239">**What is hello largest page blob size that's supported?**</span></span>

<span data-ttu-id="787ee-240">hello största blob sidstorlek som har stöd för Azure är 8 TB (8191 GB).</span><span class="sxs-lookup"><span data-stu-id="787ee-240">hello largest page blob size that Azure supports is 8 TB (8,191 GB).</span></span> <span data-ttu-id="787ee-241">Vi stöder inte sidblobbar som är större än 4 TB (4,095 GB) kopplade tooa VM som data eller operativsystemet diskar.</span><span class="sxs-lookup"><span data-stu-id="787ee-241">We don't support page blobs larger than 4 TB (4,095 GB) attached tooa VM as data or operating system disks.</span></span>

<span data-ttu-id="787ee-242">**Behöver jag toouse en ny version av Azure-verktyg toocreate, bifoga, ändra storlek och ladda upp diskar som är större än 1 TB?**</span><span class="sxs-lookup"><span data-stu-id="787ee-242">**Do I need toouse a new version of Azure tools toocreate, attach, resize, and upload disks larger than 1 TB?**</span></span>

<span data-ttu-id="787ee-243">Du behöver inte tooupgrade din befintliga Azure-verktyg toocreate, koppla eller ändra storlek på diskar som är större än 1 TB.</span><span class="sxs-lookup"><span data-stu-id="787ee-243">You don't need tooupgrade your existing Azure tools toocreate, attach, or resize disks larger than 1 TB.</span></span> <span data-ttu-id="787ee-244">tooupload din VHD-filen från lokala direkt tooAzure som en sidblob eller ohanterad disk måste du toouse hello senaste verktyget anger:</span><span class="sxs-lookup"><span data-stu-id="787ee-244">tooupload your VHD file from on-premises directly tooAzure as a page blob or unmanaged disk, you need toouse hello latest tool sets:</span></span>

|<span data-ttu-id="787ee-245">Azure-verktyg</span><span class="sxs-lookup"><span data-stu-id="787ee-245">Azure tools</span></span>      | <span data-ttu-id="787ee-246">Versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="787ee-246">Supported versions</span></span>                                |
|-----------------|---------------------------------------------------|
|<span data-ttu-id="787ee-247">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="787ee-247">Azure PowerShell</span></span> | <span data-ttu-id="787ee-248">Versionsnumret 4.1.0: juni 2017 släpper eller senare</span><span class="sxs-lookup"><span data-stu-id="787ee-248">Version number 4.1.0: June 2017 release or later</span></span>|
|<span data-ttu-id="787ee-249">Azure CLI v1</span><span class="sxs-lookup"><span data-stu-id="787ee-249">Azure CLI v1</span></span>     | <span data-ttu-id="787ee-250">Versionsnumret 0.10.13: släpp kan 2017 eller senare</span><span class="sxs-lookup"><span data-stu-id="787ee-250">Version number 0.10.13: May 2017 release or later</span></span>|
|<span data-ttu-id="787ee-251">AzCopy</span><span class="sxs-lookup"><span data-stu-id="787ee-251">AzCopy</span></span>           | <span data-ttu-id="787ee-252">Versionsnumret 6.1.0: juni 2017 släpper eller senare</span><span class="sxs-lookup"><span data-stu-id="787ee-252">Version number 6.1.0: June 2017 release or later</span></span>|

<span data-ttu-id="787ee-253">hello-stöd för Azure CLI v2 och Azure Lagringsutforskaren kommer snart.</span><span class="sxs-lookup"><span data-stu-id="787ee-253">hello support for Azure CLI v2 and Azure Storage Explorer is coming soon.</span></span> 

<span data-ttu-id="787ee-254">**Stöds P4 och P6 diskstorlekar för ohanterade diskar eller sidblobbar?**</span><span class="sxs-lookup"><span data-stu-id="787ee-254">**Are P4 and P6 disk sizes supported for unmanaged disks or page blobs?**</span></span>

<span data-ttu-id="787ee-255">Nej.</span><span class="sxs-lookup"><span data-stu-id="787ee-255">No.</span></span> <span data-ttu-id="787ee-256">P4 (32 GB) och P6 diskstorlekar (64 GB) stöds endast för hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="787ee-256">P4 (32 GB) and P6 (64 GB) disk sizes are supported only for managed disks.</span></span> <span data-ttu-id="787ee-257">Stöd för ohanterade diskar och sidblobbar kommer snart.</span><span class="sxs-lookup"><span data-stu-id="787ee-257">Support for unmanaged disks and page blobs is coming soon.</span></span>

<span data-ttu-id="787ee-258">**Om min befintliga premium hanteras disk mindre än 64 GB skapades innan hello liten disk har aktiverats (runt den 15 juni 2017), hur den faktureras?**</span><span class="sxs-lookup"><span data-stu-id="787ee-258">**If my existing premium managed disk less than 64 GB was created before hello small disk was enabled (around June 15, 2017), how is it billed?**</span></span>

<span data-ttu-id="787ee-259">Befintliga små premiumdiskar som är mindre än 64 GB fortsätta toobe debiteras bl.a toohello P10 prisnivå.</span><span class="sxs-lookup"><span data-stu-id="787ee-259">Existing small premium disks less than 64 GB continue toobe billed according toohello P10 pricing tier.</span></span> 

<span data-ttu-id="787ee-260">**Hur kan jag byta hello disk nivå i små premiumdiskar som är mindre än 64 GB från P10 tooP4 eller P6?**</span><span class="sxs-lookup"><span data-stu-id="787ee-260">**How can I switch hello disk tier of small premium disks less than 64 GB from P10 tooP4 or P6?**</span></span>

<span data-ttu-id="787ee-261">Du kan ta en ögonblicksbild av små diskar och sedan skapa disken tooautomatically växel hello priser nivå tooP4 eller P6 baserat på hello etablerats storlek.</span><span class="sxs-lookup"><span data-stu-id="787ee-261">You can take a snapshot of your small disks and then create a disk tooautomatically switch hello pricing tier tooP4 or P6 based on hello provisioned size.</span></span> 


## <a name="what-if-my-question-isnt-answered-here"></a><span data-ttu-id="787ee-262">Vad gör jag om min fråga inte besvaras här?</span><span class="sxs-lookup"><span data-stu-id="787ee-262">What if my question isn't answered here?</span></span>

<span data-ttu-id="787ee-263">Om din fråga inte finns med här kan för oss berätta och vi hjälper dig att hitta ett svar.</span><span class="sxs-lookup"><span data-stu-id="787ee-263">If your question isn't listed here, let us know and we'll help you find an answer.</span></span> <span data-ttu-id="787ee-264">Du kan ställa en fråga hello slutet av den här artikeln i hello kommentarer.</span><span class="sxs-lookup"><span data-stu-id="787ee-264">You can post a question at hello end of this article in hello comments.</span></span> <span data-ttu-id="787ee-265">tooengage med hello Azure Storage-teamet och andra gruppmedlemmar om den här artikeln använder hello MSDN [Azure Storage-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span><span class="sxs-lookup"><span data-stu-id="787ee-265">tooengage with hello Azure Storage team and other community members about this article, use hello MSDN [Azure Storage forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span></span>

<span data-ttu-id="787ee-266">toorequest funktioner, skicka din begäran och idéer toohello [Azure Storage Feedbackforum](https://feedback.azure.com/forums/217298-storage).</span><span class="sxs-lookup"><span data-stu-id="787ee-266">toorequest features, submit your requests and ideas toohello [Azure Storage feedback forum](https://feedback.azure.com/forums/217298-storage).</span></span>
