---
title: Azure-infrastrukturen naming riktlinjer - Linux | Microsoft Docs
description: "Läs mer om viktiga design och implementeringslösning riktlinjer för namngivning av i Azure infrastrukturtjänster."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 18ee4fb1-8297-49a1-8d3f-097880be67c7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1b086f0972c02d569a7219820a3d596960b6014b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-infrastructure-naming-guidelines-for-linux-vms"></a><span data-ttu-id="cb1d0-103">Azure-infrastrukturen namngivningen riktlinjer för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="cb1d0-103">Azure infrastructure naming guidelines for Linux VMs</span></span> 

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

<span data-ttu-id="cb1d0-104">Den här artikeln fokuserar på att förstå hur du hanterar namnkonventionerna för alla olika Azure-resurser att skapa en logisk och lätt att identifiera uppsättning resurser i din miljö.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-104">This article focuses on understanding how to approach naming conventions for all your various Azure resources to build a logical and easily identifiable set of resources across your environment.</span></span>

## <a name="implementation-guidelines-for-naming-conventions"></a><span data-ttu-id="cb1d0-105">Implementeringsriktlinjer för namngivningskonventioner</span><span class="sxs-lookup"><span data-stu-id="cb1d0-105">Implementation guidelines for naming conventions</span></span>
<span data-ttu-id="cb1d0-106">Beslut:</span><span class="sxs-lookup"><span data-stu-id="cb1d0-106">Decisions:</span></span>

* <span data-ttu-id="cb1d0-107">Vilka är dina namnkonventionerna för Azure-resurser?</span><span class="sxs-lookup"><span data-stu-id="cb1d0-107">What are your naming conventions for Azure resources?</span></span>

<span data-ttu-id="cb1d0-108">Aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="cb1d0-108">Tasks:</span></span>

* <span data-ttu-id="cb1d0-109">Definiera affix att använda i dina resurser för att upprätthålla enhetliga.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-109">Define the affixes to use across your resources to maintain consistency.</span></span>
* <span data-ttu-id="cb1d0-110">Definiera lagringskontonamn angivna krav att vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-110">Define storage account names given the requirement for them to be globally unique.</span></span>
* <span data-ttu-id="cb1d0-111">Dokumentera namngivningskonvention ska användas och distribueras till alla de parter som ingår för att garantera konsekvens över distributioner.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-111">Document the naming convention to be used and distribute to all parties involved to ensure consistency across deployments.</span></span>

## <a name="naming-conventions"></a><span data-ttu-id="cb1d0-112">Namngivningskonventioner</span><span class="sxs-lookup"><span data-stu-id="cb1d0-112">Naming conventions</span></span>
<span data-ttu-id="cb1d0-113">Du bör ha en god namngivningskonvention på plats innan du skapar något i Azure.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-113">You should have a good naming convention in place before creating anything in Azure.</span></span> <span data-ttu-id="cb1d0-114">En namngivningskonvention garanterar att alla resurser som har en förutsägbar namn, som hjälper dig att minska det administrativa arbetet hantering av dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-114">A naming convention ensures that all the resources have a predictable name, which helps lower the administrative burden associated with managing those resources.</span></span>

<span data-ttu-id="cb1d0-115">Du kan välja att följa en specifik uppsättning namnkonventioner som definierats för hela organisationen eller för en viss Azure-prenumeration eller ett konto.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-115">You might choose to follow a specific set of naming conventions defined for your entire organization or for a specific Azure subscription or account.</span></span> <span data-ttu-id="cb1d0-116">Även om det är enkelt för enskilda användare i organisationer att fastställa implicit regler när du arbetar med Azure-resurser som du behöver kunna skala för grupper som arbetar i Azure.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-116">Although it is easy for individuals within organizations to establish implicit rules when working with Azure resources, you need to be able to scale for teams working together in Azure.</span></span>

<span data-ttu-id="cb1d0-117">Komma överens om en uppsättning namnkonventioner direkt.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-117">Agree on a set of naming conventions up front.</span></span> <span data-ttu-id="cb1d0-118">Det finns några överväganden om namngivningskonventioner som täcker som anger regler.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-118">There are some considerations regarding naming conventions that cut across that sets of rules.</span></span>

## <a name="affixes"></a><span data-ttu-id="cb1d0-119">Utför</span><span class="sxs-lookup"><span data-stu-id="cb1d0-119">Affixes</span></span>
<span data-ttu-id="cb1d0-120">När du visar om du vill definiera en namngivningskonvention är ett beslut om Affixet är på:</span><span class="sxs-lookup"><span data-stu-id="cb1d0-120">As you look to define a naming convention, one decision is whether the affix is at:</span></span>

* <span data-ttu-id="cb1d0-121">I början av namn (prefix)</span><span class="sxs-lookup"><span data-stu-id="cb1d0-121">The beginning of the name (prefix)</span></span>
* <span data-ttu-id="cb1d0-122">I slutet av namn (suffix)</span><span class="sxs-lookup"><span data-stu-id="cb1d0-122">The end of the name (suffix)</span></span>

<span data-ttu-id="cb1d0-123">Här är till exempel två möjliga namn för en resursgrupp med hjälp av den `rg` fästa:</span><span class="sxs-lookup"><span data-stu-id="cb1d0-123">For instance, here are two possible names for a Resource Group using the `rg` affix:</span></span>

* <span data-ttu-id="cb1d0-124">Rg WebApp (prefix)</span><span class="sxs-lookup"><span data-stu-id="cb1d0-124">Rg-WebApp (prefix)</span></span>
* <span data-ttu-id="cb1d0-125">WebApp-Rg (suffix)</span><span class="sxs-lookup"><span data-stu-id="cb1d0-125">WebApp-Rg (suffix)</span></span>

<span data-ttu-id="cb1d0-126">Affix kan referera till olika aspekter som beskriver resurserna.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-126">Affixes can refer to different aspects that describe the particular resources.</span></span> <span data-ttu-id="cb1d0-127">I följande tabell visas några exempel som vanligtvis används.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-127">The following table shows some examples typically used.</span></span>

| <span data-ttu-id="cb1d0-128">Aspekt</span><span class="sxs-lookup"><span data-stu-id="cb1d0-128">Aspect</span></span> | <span data-ttu-id="cb1d0-129">Exempel</span><span class="sxs-lookup"><span data-stu-id="cb1d0-129">Examples</span></span> | <span data-ttu-id="cb1d0-130">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="cb1d0-130">Notes</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="cb1d0-131">Miljö</span><span class="sxs-lookup"><span data-stu-id="cb1d0-131">Environment</span></span> |<span data-ttu-id="cb1d0-132">Dev-, stg, prod</span><span class="sxs-lookup"><span data-stu-id="cb1d0-132">dev, stg, prod</span></span> |<span data-ttu-id="cb1d0-133">Beroende på syftet med och namnet på varje miljö.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-133">Depending on the purpose and name of each environment.</span></span> |
| <span data-ttu-id="cb1d0-134">Plats</span><span class="sxs-lookup"><span data-stu-id="cb1d0-134">Location</span></span> |<span data-ttu-id="cb1d0-135">usw (USA, västra), Använd (östra USA 2)</span><span class="sxs-lookup"><span data-stu-id="cb1d0-135">usw (West US), use (East US 2)</span></span> |<span data-ttu-id="cb1d0-136">Beroende på region i datacenter eller region för organisationen.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-136">Depending on the region of the datacenter or the region of the organization.</span></span> |
| <span data-ttu-id="cb1d0-137">Azure-komponent, tjänst eller produkt</span><span class="sxs-lookup"><span data-stu-id="cb1d0-137">Azure component, service, or product</span></span> |<span data-ttu-id="cb1d0-138">Rg för resursgrupp, virtuella nätverk för virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="cb1d0-138">Rg for resource group, VNet for virtual network</span></span> |<span data-ttu-id="cb1d0-139">Beroende på den produkt som resursen ger stöd.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-139">Depending on the product for which the resource provides support.</span></span> |
| <span data-ttu-id="cb1d0-140">Roll</span><span class="sxs-lookup"><span data-stu-id="cb1d0-140">Role</span></span> |<span data-ttu-id="cb1d0-141">DB-app, web</span><span class="sxs-lookup"><span data-stu-id="cb1d0-141">db, app, web</span></span> |<span data-ttu-id="cb1d0-142">Beroende på vilken roll för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-142">Depending on the role of the virtual machine.</span></span> |
| <span data-ttu-id="cb1d0-143">Instans</span><span class="sxs-lookup"><span data-stu-id="cb1d0-143">Instance</span></span> |<span data-ttu-id="cb1d0-144">01, 02, 03, osv.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-144">01, 02, 03, etc.</span></span> |<span data-ttu-id="cb1d0-145">Resurser som har mer än en instans.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-145">For resources that have more than one instance.</span></span> <span data-ttu-id="cb1d0-146">Till exempel belastningsutjämnade servrar i en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-146">For example, load balanced web servers in a cloud service.</span></span> |

<span data-ttu-id="cb1d0-147">När din namngivningskonventioner, kontrollera att de klart vilka affix ska användas för varje typ av resursen och vilken plats (prefix vs suffix).</span><span class="sxs-lookup"><span data-stu-id="cb1d0-147">When establishing your naming conventions, make sure that they clearly state which affixes to use for each type of resource, and in which position (prefix vs suffix).</span></span>

## <a name="dates"></a><span data-ttu-id="cb1d0-148">datum</span><span class="sxs-lookup"><span data-stu-id="cb1d0-148">Dates</span></span>
<span data-ttu-id="cb1d0-149">Ofta är det viktigt att fastställa skapandedatum från namnet på en resurs.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-149">It is often important to determine the date of creation from the name of a resource.</span></span> <span data-ttu-id="cb1d0-150">Vi rekommenderar ÅÅÅÅMMDD datumformat.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-150">We recommend the YYYYMMDD date format.</span></span> <span data-ttu-id="cb1d0-151">Detta format garanterar som är inte bara hela datumet registreras, men också att två resurser vars namn skiljer sig bara sorteras alfabetiskt och kronologiskt.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-151">This format ensures that not only is the full date is recorded, but also that two resources whose names differ only on the date are sorted alphabetically and chronologically.</span></span>

## <a name="naming-resources"></a><span data-ttu-id="cb1d0-152">Namngivning av resurser</span><span class="sxs-lookup"><span data-stu-id="cb1d0-152">Naming resources</span></span>
<span data-ttu-id="cb1d0-153">Definiera varje typ av resurs i namngivningskonvention som ska ha regler som definierar hur du tilldelar namn till varje resurs som har skapats.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-153">Define each type of resource in the naming convention, which should have rules that define how to assign names to each resource that is created.</span></span> <span data-ttu-id="cb1d0-154">Dessa regler tillämpas för alla typer av resurser, till exempel:</span><span class="sxs-lookup"><span data-stu-id="cb1d0-154">These rules should apply to all types of resources, for example:</span></span>

* <span data-ttu-id="cb1d0-155">Prenumerationer</span><span class="sxs-lookup"><span data-stu-id="cb1d0-155">Subscriptions</span></span>
* <span data-ttu-id="cb1d0-156">Konton</span><span class="sxs-lookup"><span data-stu-id="cb1d0-156">Accounts</span></span>
* <span data-ttu-id="cb1d0-157">Lagringskonton</span><span class="sxs-lookup"><span data-stu-id="cb1d0-157">Storage accounts</span></span>
* <span data-ttu-id="cb1d0-158">Virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="cb1d0-158">Virtual networks</span></span>
* <span data-ttu-id="cb1d0-159">Undernät</span><span class="sxs-lookup"><span data-stu-id="cb1d0-159">Subnets</span></span>
* <span data-ttu-id="cb1d0-160">Tillgänglighetsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="cb1d0-160">Availability sets</span></span>
* <span data-ttu-id="cb1d0-161">Resursgrupper</span><span class="sxs-lookup"><span data-stu-id="cb1d0-161">Resource groups</span></span>
* <span data-ttu-id="cb1d0-162">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="cb1d0-162">Virtual machines</span></span>
* <span data-ttu-id="cb1d0-163">Slutpunkter</span><span class="sxs-lookup"><span data-stu-id="cb1d0-163">Endpoints</span></span>
* <span data-ttu-id="cb1d0-164">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="cb1d0-164">Network security groups</span></span>
* <span data-ttu-id="cb1d0-165">Roller</span><span class="sxs-lookup"><span data-stu-id="cb1d0-165">Roles</span></span>

<span data-ttu-id="cb1d0-166">Ger tillräckligt med information för att avgöra vilken resurs som det refererar bör du använda beskrivande namn för att se till att namnet.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-166">To ensure that the name provides enough information to determine to which resource it refers, you should use descriptive names.</span></span>

## <a name="computer-names"></a><span data-ttu-id="cb1d0-167">Datornamn</span><span class="sxs-lookup"><span data-stu-id="cb1d0-167">Computer names</span></span>
<span data-ttu-id="cb1d0-168">När du skapar en virtuell dator (VM), kräver Azure ett VM-namn med upp till 64 tecken som ska användas för resursnamnet.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-168">When you create a virtual machine (VM), Azure requires a VM name of up to 64 characters that is used for the resource name.</span></span> <span data-ttu-id="cb1d0-169">Azure använder samma namn för operativsystemet installerat på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-169">Azure uses the same name for the operating system installed in the VM.</span></span> <span data-ttu-id="cb1d0-170">Men kan dessa namn inte alltid densamma.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-170">However, these names might not always be the same.</span></span>

<span data-ttu-id="cb1d0-171">Om en virtuell dator skapas från en avbildning VHD-fil som innehåller redan ett operativsystem på det Virtuella datornamnet i Azure kan skilja sig från datornamnet för den Virtuella datorns operativsystem.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-171">If a VM is created from a .vhd image file that already contains an operating system, the VM name in Azure can differ from the VM's operating system computer name.</span></span> <span data-ttu-id="cb1d0-172">Den här situationen kan lägga till en viss svårt att VM-hantering som därför inte rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-172">This situation can add a degree of difficulty to VM management, which we therefore do not recommend.</span></span> <span data-ttu-id="cb1d0-173">Tilldela den Virtuella Azure-resursen med samma namn som datornamnet som du tilldelar till operativsystemet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-173">Assign the Azure VM resource the same name as the computer name that you assign to the operating system of that VM.</span></span>

<span data-ttu-id="cb1d0-174">Vi rekommenderar att Azure VM-namn är samma som det underliggande operativsystemet datornamnet.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-174">We recommend that the Azure VM name is the same as the underlying operating system computer name.</span></span>

## <a name="storage-account-names"></a><span data-ttu-id="cb1d0-175">Lagringskontonamn</span><span class="sxs-lookup"><span data-stu-id="cb1d0-175">Storage account names</span></span>
<span data-ttu-id="cb1d0-176">Det här avsnittet gäller inte för [Azure hanterade diskar](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)eftersom du inte skapar ett separat lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-176">This section does not apply to [Azure Managed Disks](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), as you do not create a separate storage account.</span></span> <span data-ttu-id="cb1d0-177">Storage-konton har särskilda regler för deras namn för ohanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-177">For unmanaged disks, storage accounts have special rules governing their names.</span></span> <span data-ttu-id="cb1d0-178">Du kan endast använda gemena bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-178">You can only use lowercase letters and numbers.</span></span> <span data-ttu-id="cb1d0-179">Mer information finns i [skapa ett lagringskonto](../../storage/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="cb1d0-179">For more information, see [Create a storage account](../../storage/storage-create-storage-account.md#create-a-storage-account).</span></span> <span data-ttu-id="cb1d0-180">Lagringskontonamnet med core.windows.net, bör dessutom vara ett giltigt GUID, unikt DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="cb1d0-180">Additionally, the storage account name, with core.windows.net, should be a globally valid, unique DNS name.</span></span> <span data-ttu-id="cb1d0-181">Exempelvis om lagringskontot anropas mittlagringskonto, måste följande resulterande DNS-namn vara unika:</span><span class="sxs-lookup"><span data-stu-id="cb1d0-181">For instance, if the storage account is called mystorageaccount, the following resulting DNS names should be unique:</span></span>

* <span data-ttu-id="cb1d0-182">mystorageaccount.BLOB.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="cb1d0-182">mystorageaccount.blob.core.windows.net</span></span>
* <span data-ttu-id="cb1d0-183">mystorageaccount.Table.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="cb1d0-183">mystorageaccount.table.core.windows.net</span></span>
* <span data-ttu-id="cb1d0-184">mystorageaccount.Queue.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="cb1d0-184">mystorageaccount.queue.core.windows.net</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb1d0-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cb1d0-185">Next steps</span></span>
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

