---
title: aaaAzure infrastruktur naming riktlinjer - Linux | Microsoft Docs
description: "Läs mer om hello viktiga design och implementeringslösning riktlinjer för namngivning i Azure infrastrukturtjänster."
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
ms.openlocfilehash: 333146e7b2071e43527a5d7dc2ec02ebfb316eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-infrastructure-naming-guidelines-for-linux-vms"></a><span data-ttu-id="3806e-103">Azure-infrastrukturen namngivningen riktlinjer för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="3806e-103">Azure infrastructure naming guidelines for Linux VMs</span></span> 

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

<span data-ttu-id="3806e-104">Den här artikeln fokuserar på att förstå hur tooapproach namnkonventionerna för alla dina olika Azure-resurser toobuild en logisk och lätt att identifiera en uppsättning resurser i din miljö.</span><span class="sxs-lookup"><span data-stu-id="3806e-104">This article focuses on understanding how tooapproach naming conventions for all your various Azure resources toobuild a logical and easily identifiable set of resources across your environment.</span></span>

## <a name="implementation-guidelines-for-naming-conventions"></a><span data-ttu-id="3806e-105">Implementeringsriktlinjer för namngivningskonventioner</span><span class="sxs-lookup"><span data-stu-id="3806e-105">Implementation guidelines for naming conventions</span></span>
<span data-ttu-id="3806e-106">Beslut:</span><span class="sxs-lookup"><span data-stu-id="3806e-106">Decisions:</span></span>

* <span data-ttu-id="3806e-107">Vilka är dina namnkonventionerna för Azure-resurser?</span><span class="sxs-lookup"><span data-stu-id="3806e-107">What are your naming conventions for Azure resources?</span></span>

<span data-ttu-id="3806e-108">Aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="3806e-108">Tasks:</span></span>

* <span data-ttu-id="3806e-109">Definiera hello affix toouse över dina resurser toomaintain konsekvenskontroll.</span><span class="sxs-lookup"><span data-stu-id="3806e-109">Define hello affixes toouse across your resources toomaintain consistency.</span></span>
* <span data-ttu-id="3806e-110">Definiera namn tilldelat hello krav för dem toobe globalt unik storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3806e-110">Define storage account names given hello requirement for them toobe globally unique.</span></span>
* <span data-ttu-id="3806e-111">Dokumentet hello naming convention toobe används och distribuera tooall parter ingår tooensure konsekvens mellan distributioner.</span><span class="sxs-lookup"><span data-stu-id="3806e-111">Document hello naming convention toobe used and distribute tooall parties involved tooensure consistency across deployments.</span></span>

## <a name="naming-conventions"></a><span data-ttu-id="3806e-112">Namngivningskonventioner</span><span class="sxs-lookup"><span data-stu-id="3806e-112">Naming conventions</span></span>
<span data-ttu-id="3806e-113">Du bör ha en god namngivningskonvention på plats innan du skapar något i Azure.</span><span class="sxs-lookup"><span data-stu-id="3806e-113">You should have a good naming convention in place before creating anything in Azure.</span></span> <span data-ttu-id="3806e-114">En namngivningskonvention garanterar att alla hello resurser har en förutsägbar namn, vilket gör det lägre hello-administrativa belastningen hantering av dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="3806e-114">A naming convention ensures that all hello resources have a predictable name, which helps lower hello administrative burden associated with managing those resources.</span></span>

<span data-ttu-id="3806e-115">Du kan välja toofollow en specifik uppsättning namnkonventioner som definierats för hela organisationen eller för en viss Azure-prenumeration eller ett konto.</span><span class="sxs-lookup"><span data-stu-id="3806e-115">You might choose toofollow a specific set of naming conventions defined for your entire organization or for a specific Azure subscription or account.</span></span> <span data-ttu-id="3806e-116">Men det är enkelt för enskilda användare i organisationer tooestablish implicit regler när du arbetar med Azure-resurser, behöver du toobe kan tooscale för grupper som arbetar i Azure.</span><span class="sxs-lookup"><span data-stu-id="3806e-116">Although it is easy for individuals within organizations tooestablish implicit rules when working with Azure resources, you need toobe able tooscale for teams working together in Azure.</span></span>

<span data-ttu-id="3806e-117">Komma överens om en uppsättning namnkonventioner direkt.</span><span class="sxs-lookup"><span data-stu-id="3806e-117">Agree on a set of naming conventions up front.</span></span> <span data-ttu-id="3806e-118">Det finns några överväganden om namngivningskonventioner som täcker som anger regler.</span><span class="sxs-lookup"><span data-stu-id="3806e-118">There are some considerations regarding naming conventions that cut across that sets of rules.</span></span>

## <a name="affixes"></a><span data-ttu-id="3806e-119">Utför</span><span class="sxs-lookup"><span data-stu-id="3806e-119">Affixes</span></span>
<span data-ttu-id="3806e-120">När du visar toodefine en namngivningskonvention är ett beslut om hello affix är på:</span><span class="sxs-lookup"><span data-stu-id="3806e-120">As you look toodefine a naming convention, one decision is whether hello affix is at:</span></span>

* <span data-ttu-id="3806e-121">hello namn börjar på hello (prefix)</span><span class="sxs-lookup"><span data-stu-id="3806e-121">hello beginning of hello name (prefix)</span></span>
* <span data-ttu-id="3806e-122">hello slutet av hello namn (suffix)</span><span class="sxs-lookup"><span data-stu-id="3806e-122">hello end of hello name (suffix)</span></span>

<span data-ttu-id="3806e-123">Här är till exempel två möjliga namn för en resursgrupp med hello `rg` fästa:</span><span class="sxs-lookup"><span data-stu-id="3806e-123">For instance, here are two possible names for a Resource Group using hello `rg` affix:</span></span>

* <span data-ttu-id="3806e-124">Rg WebApp (prefix)</span><span class="sxs-lookup"><span data-stu-id="3806e-124">Rg-WebApp (prefix)</span></span>
* <span data-ttu-id="3806e-125">WebApp-Rg (suffix)</span><span class="sxs-lookup"><span data-stu-id="3806e-125">WebApp-Rg (suffix)</span></span>

<span data-ttu-id="3806e-126">Affix kan hänvisa toodifferent aspekter som beskriver hello resurser.</span><span class="sxs-lookup"><span data-stu-id="3806e-126">Affixes can refer toodifferent aspects that describe hello particular resources.</span></span> <span data-ttu-id="3806e-127">hello visas nedan några exempel som vanligtvis används.</span><span class="sxs-lookup"><span data-stu-id="3806e-127">hello following table shows some examples typically used.</span></span>

| <span data-ttu-id="3806e-128">Aspekt</span><span class="sxs-lookup"><span data-stu-id="3806e-128">Aspect</span></span> | <span data-ttu-id="3806e-129">Exempel</span><span class="sxs-lookup"><span data-stu-id="3806e-129">Examples</span></span> | <span data-ttu-id="3806e-130">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="3806e-130">Notes</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3806e-131">Miljö</span><span class="sxs-lookup"><span data-stu-id="3806e-131">Environment</span></span> |<span data-ttu-id="3806e-132">Dev-, stg, prod</span><span class="sxs-lookup"><span data-stu-id="3806e-132">dev, stg, prod</span></span> |<span data-ttu-id="3806e-133">Beroende på hello ändamål och namnet på varje miljö.</span><span class="sxs-lookup"><span data-stu-id="3806e-133">Depending on hello purpose and name of each environment.</span></span> |
| <span data-ttu-id="3806e-134">Plats</span><span class="sxs-lookup"><span data-stu-id="3806e-134">Location</span></span> |<span data-ttu-id="3806e-135">usw (USA, västra), Använd (östra USA 2)</span><span class="sxs-lookup"><span data-stu-id="3806e-135">usw (West US), use (East US 2)</span></span> |<span data-ttu-id="3806e-136">Beroende på hello region för hello datacenter eller hello region hello organisation.</span><span class="sxs-lookup"><span data-stu-id="3806e-136">Depending on hello region of hello datacenter or hello region of hello organization.</span></span> |
| <span data-ttu-id="3806e-137">Azure-komponent, tjänst eller produkt</span><span class="sxs-lookup"><span data-stu-id="3806e-137">Azure component, service, or product</span></span> |<span data-ttu-id="3806e-138">Rg för resursgrupp, virtuella nätverk för virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="3806e-138">Rg for resource group, VNet for virtual network</span></span> |<span data-ttu-id="3806e-139">Beroende på hello produkt för vilka hello resursen stöder.</span><span class="sxs-lookup"><span data-stu-id="3806e-139">Depending on hello product for which hello resource provides support.</span></span> |
| <span data-ttu-id="3806e-140">Roll</span><span class="sxs-lookup"><span data-stu-id="3806e-140">Role</span></span> |<span data-ttu-id="3806e-141">DB-app, web</span><span class="sxs-lookup"><span data-stu-id="3806e-141">db, app, web</span></span> |<span data-ttu-id="3806e-142">Beroende på hello roll för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3806e-142">Depending on hello role of hello virtual machine.</span></span> |
| <span data-ttu-id="3806e-143">Instans</span><span class="sxs-lookup"><span data-stu-id="3806e-143">Instance</span></span> |<span data-ttu-id="3806e-144">01, 02, 03, osv.</span><span class="sxs-lookup"><span data-stu-id="3806e-144">01, 02, 03, etc.</span></span> |<span data-ttu-id="3806e-145">Resurser som har mer än en instans.</span><span class="sxs-lookup"><span data-stu-id="3806e-145">For resources that have more than one instance.</span></span> <span data-ttu-id="3806e-146">Till exempel belastningsutjämnade servrar i en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="3806e-146">For example, load balanced web servers in a cloud service.</span></span> |

<span data-ttu-id="3806e-147">När din namngivningskonventioner, se till att de klart som utför toouse för varje typ av resurs och i vilket läge (prefix vs suffix).</span><span class="sxs-lookup"><span data-stu-id="3806e-147">When establishing your naming conventions, make sure that they clearly state which affixes toouse for each type of resource, and in which position (prefix vs suffix).</span></span>

## <a name="dates"></a><span data-ttu-id="3806e-148">datum</span><span class="sxs-lookup"><span data-stu-id="3806e-148">Dates</span></span>
<span data-ttu-id="3806e-149">Det är ofta viktiga toodetermine hello datum av hello namn skapas av en resurs.</span><span class="sxs-lookup"><span data-stu-id="3806e-149">It is often important toodetermine hello date of creation from hello name of a resource.</span></span> <span data-ttu-id="3806e-150">Vi rekommenderar hello ÅÅÅÅMMDD datumformat.</span><span class="sxs-lookup"><span data-stu-id="3806e-150">We recommend hello YYYYMMDD date format.</span></span> <span data-ttu-id="3806e-151">Det här formatet blir inte bara hello fullständig datumet registreras, men också att två resurser vars namn skiljer sig bara på hello datum sorteras alfabetiskt och kronologiskt.</span><span class="sxs-lookup"><span data-stu-id="3806e-151">This format ensures that not only is hello full date is recorded, but also that two resources whose names differ only on hello date are sorted alphabetically and chronologically.</span></span>

## <a name="naming-resources"></a><span data-ttu-id="3806e-152">Namngivning av resurser</span><span class="sxs-lookup"><span data-stu-id="3806e-152">Naming resources</span></span>
<span data-ttu-id="3806e-153">Definiera varje typ av resurs i hello namngivningskonvention, som bör ha regler som definierar hur tooassign namn tooeach resurs som har skapats.</span><span class="sxs-lookup"><span data-stu-id="3806e-153">Define each type of resource in hello naming convention, which should have rules that define how tooassign names tooeach resource that is created.</span></span> <span data-ttu-id="3806e-154">Dessa regler tillämpas tooall typer av resurser, till exempel:</span><span class="sxs-lookup"><span data-stu-id="3806e-154">These rules should apply tooall types of resources, for example:</span></span>

* <span data-ttu-id="3806e-155">Prenumerationer</span><span class="sxs-lookup"><span data-stu-id="3806e-155">Subscriptions</span></span>
* <span data-ttu-id="3806e-156">Konton</span><span class="sxs-lookup"><span data-stu-id="3806e-156">Accounts</span></span>
* <span data-ttu-id="3806e-157">Lagringskonton</span><span class="sxs-lookup"><span data-stu-id="3806e-157">Storage accounts</span></span>
* <span data-ttu-id="3806e-158">Virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="3806e-158">Virtual networks</span></span>
* <span data-ttu-id="3806e-159">Undernät</span><span class="sxs-lookup"><span data-stu-id="3806e-159">Subnets</span></span>
* <span data-ttu-id="3806e-160">Tillgänglighetsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="3806e-160">Availability sets</span></span>
* <span data-ttu-id="3806e-161">Resursgrupper</span><span class="sxs-lookup"><span data-stu-id="3806e-161">Resource groups</span></span>
* <span data-ttu-id="3806e-162">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3806e-162">Virtual machines</span></span>
* <span data-ttu-id="3806e-163">Slutpunkter</span><span class="sxs-lookup"><span data-stu-id="3806e-163">Endpoints</span></span>
* <span data-ttu-id="3806e-164">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="3806e-164">Network security groups</span></span>
* <span data-ttu-id="3806e-165">Roller</span><span class="sxs-lookup"><span data-stu-id="3806e-165">Roles</span></span>

<span data-ttu-id="3806e-166">tooensure som hello namn innehåller tillräckligt med information toodetermine toowhich resurs som det refererar bör du använda deskriptiva namn.</span><span class="sxs-lookup"><span data-stu-id="3806e-166">tooensure that hello name provides enough information toodetermine toowhich resource it refers, you should use descriptive names.</span></span>

## <a name="computer-names"></a><span data-ttu-id="3806e-167">Datornamn</span><span class="sxs-lookup"><span data-stu-id="3806e-167">Computer names</span></span>
<span data-ttu-id="3806e-168">När du skapar en virtuell dator (VM), kräver Azure en VM namnet på upp too64 tecken som används för hello resursnamnet.</span><span class="sxs-lookup"><span data-stu-id="3806e-168">When you create a virtual machine (VM), Azure requires a VM name of up too64 characters that is used for hello resource name.</span></span> <span data-ttu-id="3806e-169">Azure använder hello samma namn för hello operativsystem i hello VM.</span><span class="sxs-lookup"><span data-stu-id="3806e-169">Azure uses hello same name for hello operating system installed in hello VM.</span></span> <span data-ttu-id="3806e-170">Men kan dessa namn inte alltid vara hello samma.</span><span class="sxs-lookup"><span data-stu-id="3806e-170">However, these names might not always be hello same.</span></span>

<span data-ttu-id="3806e-171">Om en virtuell dator skapas från en avbildning VHD-fil som innehåller ett operativsystem redan hello namn på virtuell dator i Azure kan skilja sig från hello datornamn för Virtuella datorer med operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="3806e-171">If a VM is created from a .vhd image file that already contains an operating system, hello VM name in Azure can differ from hello VM's operating system computer name.</span></span> <span data-ttu-id="3806e-172">Den här situationen kan lägga till en viss svårt tooVM management som därför inte rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="3806e-172">This situation can add a degree of difficulty tooVM management, which we therefore do not recommend.</span></span> <span data-ttu-id="3806e-173">Tilldela hello Azure VM resurs hello samma namn som hello datornamn som du tilldelar toohello operativsystemet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3806e-173">Assign hello Azure VM resource hello same name as hello computer name that you assign toohello operating system of that VM.</span></span>

<span data-ttu-id="3806e-174">Vi rekommenderar att hello Azure VM-namn är hello samma som hello underliggande operativsystemet datornamn.</span><span class="sxs-lookup"><span data-stu-id="3806e-174">We recommend that hello Azure VM name is hello same as hello underlying operating system computer name.</span></span>

## <a name="storage-account-names"></a><span data-ttu-id="3806e-175">Lagringskontonamn</span><span class="sxs-lookup"><span data-stu-id="3806e-175">Storage account names</span></span>
<span data-ttu-id="3806e-176">Det här avsnittet gäller inte för[Azure hanterade diskar](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)eftersom du inte skapar ett separat lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3806e-176">This section does not apply too[Azure Managed Disks](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), as you do not create a separate storage account.</span></span> <span data-ttu-id="3806e-177">Storage-konton har särskilda regler för deras namn för ohanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="3806e-177">For unmanaged disks, storage accounts have special rules governing their names.</span></span> <span data-ttu-id="3806e-178">Du kan endast använda gemena bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="3806e-178">You can only use lowercase letters and numbers.</span></span> <span data-ttu-id="3806e-179">Mer information finns i [skapa ett lagringskonto](../../storage/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="3806e-179">For more information, see [Create a storage account](../../storage/storage-create-storage-account.md#create-a-storage-account).</span></span> <span data-ttu-id="3806e-180">Hej lagringskontonamn, med core.windows.net, bör dessutom vara ett giltigt GUID, unikt DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="3806e-180">Additionally, hello storage account name, with core.windows.net, should be a globally valid, unique DNS name.</span></span> <span data-ttu-id="3806e-181">Till exempel om hello lagringskonto anropas mittlagringskonto, hello följande resulterande DNS-namn måste vara unika:</span><span class="sxs-lookup"><span data-stu-id="3806e-181">For instance, if hello storage account is called mystorageaccount, hello following resulting DNS names should be unique:</span></span>

* <span data-ttu-id="3806e-182">mystorageaccount.BLOB.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="3806e-182">mystorageaccount.blob.core.windows.net</span></span>
* <span data-ttu-id="3806e-183">mystorageaccount.Table.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="3806e-183">mystorageaccount.table.core.windows.net</span></span>
* <span data-ttu-id="3806e-184">mystorageaccount.Queue.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="3806e-184">mystorageaccount.queue.core.windows.net</span></span>

## <a name="next-steps"></a><span data-ttu-id="3806e-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3806e-185">Next steps</span></span>
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

