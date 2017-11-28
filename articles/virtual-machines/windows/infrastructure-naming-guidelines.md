---
title: aaaAzure infrastruktur naming riktlinjer - Windows | Microsoft Docs
description: "Läs mer om hello viktiga design och implementeringslösning riktlinjer för namngivning i Azure infrastrukturtjänster."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 660765fa-4d42-49cb-a9c6-8c596d26d221
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9b4a16ce99cf1cac5804c77675e24590ac2e2b33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-infrastructure-naming-guidelines-for-windows-vms"></a><span data-ttu-id="ca41d-103">Azure-infrastrukturen namngivningen riktlinjer för virtuella Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="ca41d-103">Azure infrastructure naming guidelines for Windows VMs</span></span>

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

<span data-ttu-id="ca41d-104">Den här artikeln fokuserar på att förstå hur tooapproach namnkonventionerna för alla dina olika Azure-resurser toobuild en logisk och lätt att identifiera en uppsättning resurser i din miljö.</span><span class="sxs-lookup"><span data-stu-id="ca41d-104">This article focuses on understanding how tooapproach naming conventions for all your various Azure resources toobuild a logical and easily identifiable set of resources across your environment.</span></span>

## <a name="implementation-guidelines-for-naming-conventions"></a><span data-ttu-id="ca41d-105">Implementeringsriktlinjer för namngivningskonventioner</span><span class="sxs-lookup"><span data-stu-id="ca41d-105">Implementation guidelines for naming conventions</span></span>
<span data-ttu-id="ca41d-106">Beslut:</span><span class="sxs-lookup"><span data-stu-id="ca41d-106">Decisions:</span></span>

* <span data-ttu-id="ca41d-107">Vilka är dina namnkonventionerna för Azure-resurser?</span><span class="sxs-lookup"><span data-stu-id="ca41d-107">What are your naming conventions for Azure resources?</span></span>

<span data-ttu-id="ca41d-108">Aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="ca41d-108">Tasks:</span></span>

* <span data-ttu-id="ca41d-109">Definiera hello affix toouse över dina resurser toomaintain konsekvenskontroll.</span><span class="sxs-lookup"><span data-stu-id="ca41d-109">Define hello affixes toouse across your resources toomaintain consistency.</span></span>
* <span data-ttu-id="ca41d-110">Definiera namn tilldelat hello krav för dem toobe globalt unik storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ca41d-110">Define storage account names given hello requirement for them toobe globally unique.</span></span>
* <span data-ttu-id="ca41d-111">Dokumentet hello naming convention toobe används och distribuera tooall parter ingår tooensure konsekvens mellan distributioner.</span><span class="sxs-lookup"><span data-stu-id="ca41d-111">Document hello naming convention toobe used and distribute tooall parties involved tooensure consistency across deployments.</span></span>

## <a name="naming-conventions"></a><span data-ttu-id="ca41d-112">Namngivningskonventioner</span><span class="sxs-lookup"><span data-stu-id="ca41d-112">Naming conventions</span></span>
<span data-ttu-id="ca41d-113">Du bör ha en god namngivningskonvention på plats innan du skapar något i Azure.</span><span class="sxs-lookup"><span data-stu-id="ca41d-113">You should have a good naming convention in place before creating anything in Azure.</span></span> <span data-ttu-id="ca41d-114">En namngivningskonvention garanterar att alla hello resurser har en förutsägbar namn, vilket gör det lägre hello-administrativa belastningen hantering av dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="ca41d-114">A naming convention ensures that all hello resources have a predictable name, which helps lower hello administrative burden associated with managing those resources.</span></span>

<span data-ttu-id="ca41d-115">Du kan välja toofollow en specifik uppsättning namnkonventioner som definierats för hela organisationen eller för en viss Azure-prenumeration eller ett konto.</span><span class="sxs-lookup"><span data-stu-id="ca41d-115">You might choose toofollow a specific set of naming conventions defined for your entire organization or for a specific Azure subscription or account.</span></span> <span data-ttu-id="ca41d-116">Men det är enkelt för enskilda användare i organisationer tooestablish implicit regler när du arbetar med Azure-resurser när en grupp måste toowork på ett projekt på Azure, att modellen inte bra med skalning.</span><span class="sxs-lookup"><span data-stu-id="ca41d-116">Although it is easy for individuals within organizations tooestablish implicit rules when working with Azure resources, when a team needs toowork on a project on Azure, that model does not scale well.</span></span>

<span data-ttu-id="ca41d-117">Komma överens om en uppsättning namnkonventioner direkt.</span><span class="sxs-lookup"><span data-stu-id="ca41d-117">Agree on a set of naming conventions up front.</span></span> <span data-ttu-id="ca41d-118">Det finns några överväganden om namngivningskonventioner klipp ut över den här uppsättningen regler.</span><span class="sxs-lookup"><span data-stu-id="ca41d-118">There are some considerations regarding naming conventions that cut across this set of rules.</span></span>

## <a name="affixes"></a><span data-ttu-id="ca41d-119">Utför</span><span class="sxs-lookup"><span data-stu-id="ca41d-119">Affixes</span></span>
<span data-ttu-id="ca41d-120">När du visar toodefine en namngivningskonvention, kommer ett beslut om hello affix finns på:</span><span class="sxs-lookup"><span data-stu-id="ca41d-120">As you look toodefine a naming convention, one decision comes whether hello affix is at:</span></span>

* <span data-ttu-id="ca41d-121">hello namn börjar på hello (prefix)</span><span class="sxs-lookup"><span data-stu-id="ca41d-121">hello beginning of hello name (prefix)</span></span>
* <span data-ttu-id="ca41d-122">hello slutet av hello namn (suffix)</span><span class="sxs-lookup"><span data-stu-id="ca41d-122">hello end of hello name (suffix)</span></span>

<span data-ttu-id="ca41d-123">Här är till exempel två möjliga namn för en resursgrupp med hello `rg` fästa:</span><span class="sxs-lookup"><span data-stu-id="ca41d-123">For instance, here are two possible names for a Resource Group using hello `rg` affix:</span></span>

* <span data-ttu-id="ca41d-124">Rg WebApp (prefix)</span><span class="sxs-lookup"><span data-stu-id="ca41d-124">Rg-WebApp (prefix)</span></span>
* <span data-ttu-id="ca41d-125">WebApp-Rg (suffix)</span><span class="sxs-lookup"><span data-stu-id="ca41d-125">WebApp-Rg (suffix)</span></span>

<span data-ttu-id="ca41d-126">Affix kan hänvisa toodifferent aspekter som beskriver hello resurser.</span><span class="sxs-lookup"><span data-stu-id="ca41d-126">Affixes can refer toodifferent aspects that describe hello particular resources.</span></span> <span data-ttu-id="ca41d-127">hello visas nedan några exempel som vanligtvis används.</span><span class="sxs-lookup"><span data-stu-id="ca41d-127">hello following table shows some examples typically used.</span></span>

| <span data-ttu-id="ca41d-128">Aspekt</span><span class="sxs-lookup"><span data-stu-id="ca41d-128">Aspect</span></span> | <span data-ttu-id="ca41d-129">Exempel</span><span class="sxs-lookup"><span data-stu-id="ca41d-129">Examples</span></span> | <span data-ttu-id="ca41d-130">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ca41d-130">Notes</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="ca41d-131">Miljö</span><span class="sxs-lookup"><span data-stu-id="ca41d-131">Environment</span></span> |<span data-ttu-id="ca41d-132">Dev-, stg, prod</span><span class="sxs-lookup"><span data-stu-id="ca41d-132">dev, stg, prod</span></span> |<span data-ttu-id="ca41d-133">Beroende på hello ändamål och namnet på varje miljö.</span><span class="sxs-lookup"><span data-stu-id="ca41d-133">Depending on hello purpose and name of each environment.</span></span> |
| <span data-ttu-id="ca41d-134">Plats</span><span class="sxs-lookup"><span data-stu-id="ca41d-134">Location</span></span> |<span data-ttu-id="ca41d-135">usw (USA, västra), Använd (östra USA 2)</span><span class="sxs-lookup"><span data-stu-id="ca41d-135">usw (West US), use (East US 2)</span></span> |<span data-ttu-id="ca41d-136">Beroende på hello region för hello datacenter eller hello region hello organisation.</span><span class="sxs-lookup"><span data-stu-id="ca41d-136">Depending on hello region of hello datacenter or hello region of hello organization.</span></span> |
| <span data-ttu-id="ca41d-137">Azure-komponent, tjänst eller produkt</span><span class="sxs-lookup"><span data-stu-id="ca41d-137">Azure component, service, or product</span></span> |<span data-ttu-id="ca41d-138">Rg för resursgrupp, virtuella nätverk för virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="ca41d-138">Rg for resource group, VNet for virtual network</span></span> |<span data-ttu-id="ca41d-139">Beroende på hello produkt för vilka hello resursen stöder.</span><span class="sxs-lookup"><span data-stu-id="ca41d-139">Depending on hello product for which hello resource provides support.</span></span> |
| <span data-ttu-id="ca41d-140">Roll</span><span class="sxs-lookup"><span data-stu-id="ca41d-140">Role</span></span> |<span data-ttu-id="ca41d-141">SQL, ora, sp, iis</span><span class="sxs-lookup"><span data-stu-id="ca41d-141">sql, ora, sp, iis</span></span> |<span data-ttu-id="ca41d-142">Beroende på hello roll för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ca41d-142">Depending on hello role of hello virtual machine.</span></span> |
| <span data-ttu-id="ca41d-143">Instans</span><span class="sxs-lookup"><span data-stu-id="ca41d-143">Instance</span></span> |<span data-ttu-id="ca41d-144">01, 02, 03, osv.</span><span class="sxs-lookup"><span data-stu-id="ca41d-144">01, 02, 03, etc.</span></span> |<span data-ttu-id="ca41d-145">Resurser som har mer än en instans.</span><span class="sxs-lookup"><span data-stu-id="ca41d-145">For resources that have more than one instance.</span></span> <span data-ttu-id="ca41d-146">Till exempel belastningsutjämnade servrar i en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="ca41d-146">For example, load balanced web servers in a cloud service.</span></span> |

<span data-ttu-id="ca41d-147">När din namngivningskonventioner, se till att de klart som utför toouse för varje typ av resurs och i vilket läge (prefix vs suffix).</span><span class="sxs-lookup"><span data-stu-id="ca41d-147">When establishing your naming conventions, make sure that they clearly state which affixes toouse for each type of resource, and in which position (prefix vs suffix).</span></span>

## <a name="dates"></a><span data-ttu-id="ca41d-148">datum</span><span class="sxs-lookup"><span data-stu-id="ca41d-148">Dates</span></span>
<span data-ttu-id="ca41d-149">Det är ofta viktiga toodetermine hello datum av hello namn skapas av en resurs.</span><span class="sxs-lookup"><span data-stu-id="ca41d-149">It is often important toodetermine hello date of creation from hello name of a resource.</span></span> <span data-ttu-id="ca41d-150">Vi rekommenderar hello ÅÅÅÅMMDD datumformat.</span><span class="sxs-lookup"><span data-stu-id="ca41d-150">We recommend hello YYYYMMDD date format.</span></span> <span data-ttu-id="ca41d-151">Detta format garanterar inte bara hello fullständiga datumet registreras, men också att två resurser vars namn skiljer sig bara på hello datum sorteras alfabetiskt och kronologiskt på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="ca41d-151">This format ensures that not only hello full date is recorded, but also that two resources whose names differ only on hello date is sorted alphabetically and chronologically at hello same time.</span></span>

## <a name="naming-resources"></a><span data-ttu-id="ca41d-152">Namngivning av resurser</span><span class="sxs-lookup"><span data-stu-id="ca41d-152">Naming resources</span></span>
<span data-ttu-id="ca41d-153">Definiera varje typ av resurs i hello namngivningskonvention, som bör ha regler som definierar hur tooassign namn tooeach resurs som har skapats.</span><span class="sxs-lookup"><span data-stu-id="ca41d-153">Define each type of resource in hello naming convention, which should have rules that define how tooassign names tooeach resource that is created.</span></span> <span data-ttu-id="ca41d-154">Dessa regler tillämpas tooall typer av resurser, till exempel:</span><span class="sxs-lookup"><span data-stu-id="ca41d-154">These rules should apply tooall types of resources, for example:</span></span>

* <span data-ttu-id="ca41d-155">Prenumerationer</span><span class="sxs-lookup"><span data-stu-id="ca41d-155">Subscriptions</span></span>
* <span data-ttu-id="ca41d-156">Konton</span><span class="sxs-lookup"><span data-stu-id="ca41d-156">Accounts</span></span>
* <span data-ttu-id="ca41d-157">Lagringskonton</span><span class="sxs-lookup"><span data-stu-id="ca41d-157">Storage accounts</span></span>
* <span data-ttu-id="ca41d-158">Virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="ca41d-158">Virtual networks</span></span>
* <span data-ttu-id="ca41d-159">Undernät</span><span class="sxs-lookup"><span data-stu-id="ca41d-159">Subnets</span></span>
* <span data-ttu-id="ca41d-160">Tillgänglighetsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="ca41d-160">Availability sets</span></span>
* <span data-ttu-id="ca41d-161">Resursgrupper</span><span class="sxs-lookup"><span data-stu-id="ca41d-161">Resource groups</span></span>
* <span data-ttu-id="ca41d-162">Virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="ca41d-162">Virtual machines</span></span>
* <span data-ttu-id="ca41d-163">Slutpunkter</span><span class="sxs-lookup"><span data-stu-id="ca41d-163">Endpoints</span></span>
* <span data-ttu-id="ca41d-164">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="ca41d-164">Network security groups</span></span>
* <span data-ttu-id="ca41d-165">Roller</span><span class="sxs-lookup"><span data-stu-id="ca41d-165">Roles</span></span>

<span data-ttu-id="ca41d-166">tooensure som hello namn innehåller tillräckligt med information toodetermine toowhich resurs som det refererar bör du använda deskriptiva namn.</span><span class="sxs-lookup"><span data-stu-id="ca41d-166">tooensure that hello name provides enough information toodetermine toowhich resource it refers, you should use descriptive names.</span></span>

## <a name="computer-names"></a><span data-ttu-id="ca41d-167">Datornamn</span><span class="sxs-lookup"><span data-stu-id="ca41d-167">Computer names</span></span>
<span data-ttu-id="ca41d-168">När du skapar en virtuell dator (VM), kräver Microsoft Azure VM namnet in too15 tecken som används för hello resursnamnet.</span><span class="sxs-lookup"><span data-stu-id="ca41d-168">When you create a virtual machine (VM), Microsoft Azure requires a VM name of up too15 characters which is used for hello resource name.</span></span> <span data-ttu-id="ca41d-169">Azure använder hello samma namn för hello operativsystem i hello VM.</span><span class="sxs-lookup"><span data-stu-id="ca41d-169">Azure uses hello same name for hello operating system installed in hello VM.</span></span> <span data-ttu-id="ca41d-170">Men kan dessa namn inte alltid vara hello samma.</span><span class="sxs-lookup"><span data-stu-id="ca41d-170">However, these names might not always be hello same.</span></span>

<span data-ttu-id="ca41d-171">Om en virtuell dator skapas från en avbildning VHD-fil som innehåller ett operativsystem redan hello namn på virtuell dator i Azure kan skilja sig från hello datornamn för Virtuella datorer med operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="ca41d-171">In case a VM is created from a .vhd image file that already contains an operating system, hello VM name in Azure can differ from hello VM's operating system computer name.</span></span> <span data-ttu-id="ca41d-172">Den här situationen kan lägga till en viss svårt tooVM management som därför inte rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="ca41d-172">This situation can add a degree of difficulty tooVM management, which we therefore do not recommend.</span></span> <span data-ttu-id="ca41d-173">Tilldela hello Azure VM resurs hello samma namn som hello datornamn som du tilldelar toohello operativsystemet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ca41d-173">Assign hello Azure VM resource hello same name as hello computer name that you assign toohello operating system of that VM.</span></span>

<span data-ttu-id="ca41d-174">Vi rekommenderar att hello Azure VM-namn är hello samma som hello underliggande operativsystemet datornamn.</span><span class="sxs-lookup"><span data-stu-id="ca41d-174">We recommend that hello Azure VM name is hello same as hello underlying operating system computer name.</span></span>

## <a name="storage-account-names"></a><span data-ttu-id="ca41d-175">Lagringskontonamn</span><span class="sxs-lookup"><span data-stu-id="ca41d-175">Storage account names</span></span>
<span data-ttu-id="ca41d-176">Det här avsnittet gäller inte för[Azure hanterade diskar](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)eftersom du inte skapar ett separat lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ca41d-176">This section does not apply too[Azure Managed Disks](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), as you do not create a separate storage account.</span></span> <span data-ttu-id="ca41d-177">Storage-konton har särskilda regler för deras namn för ohanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="ca41d-177">For unmanaged disks, storage accounts have special rules governing their names.</span></span> <span data-ttu-id="ca41d-178">Du kan endast använda gemena bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="ca41d-178">You can only use lowercase letters and numbers.</span></span> <span data-ttu-id="ca41d-179">Mer information finns i [skapa ett lagringskonto](../../storage/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="ca41d-179">For more information, see [Create a storage account](../../storage/storage-create-storage-account.md#create-a-storage-account).</span></span> <span data-ttu-id="ca41d-180">Hej lagringskontonamn, tillsammans med core.windows.net, bör dessutom vara ett giltigt GUID, unikt DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="ca41d-180">Additionally, hello storage account name, along with core.windows.net, should be a globally valid, unique DNS name.</span></span> <span data-ttu-id="ca41d-181">Till exempel om hello lagringskonto anropas mittlagringskonto, hello följande resulterande DNS-namn måste vara unika:</span><span class="sxs-lookup"><span data-stu-id="ca41d-181">For instance, if hello storage account is called mystorageaccount, hello following resulting DNS names should be unique:</span></span>

* <span data-ttu-id="ca41d-182">mystorageaccount.BLOB.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="ca41d-182">mystorageaccount.blob.core.windows.net</span></span>
* <span data-ttu-id="ca41d-183">mystorageaccount.Table.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="ca41d-183">mystorageaccount.table.core.windows.net</span></span>
* <span data-ttu-id="ca41d-184">mystorageaccount.Queue.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="ca41d-184">mystorageaccount.queue.core.windows.net</span></span>

## <a name="next-steps"></a><span data-ttu-id="ca41d-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ca41d-185">Next steps</span></span>
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

