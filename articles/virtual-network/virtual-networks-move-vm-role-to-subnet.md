---
title: "aaaMove en virtuell dator (klassisk) eller molntjänster rollen instans tooa olika undernät - Azure PowerShell | Microsoft Docs"
description: "Lär dig hur toomove virtuella datorer (klassisk) och molntjänster rollen instanser tooa olika undernät med hjälp av PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: de4135c7-dc5b-4ffa-84cc-1b8364b7b427
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c8d2de56f42a91be4a665414ea9641ffd46588fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-a-vm-classic-or-cloud-services-role-instance-tooa-different-subnet-using-powershell"></a><span data-ttu-id="b309f-103">Flytta en virtuell dator (klassisk) eller molntjänster rollen instans tooa olika undernät med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="b309f-103">Move a VM (Classic) or Cloud Services role instance tooa different subnet using PowerShell</span></span>
<span data-ttu-id="b309f-104">Du kan använda PowerShell toomove din virtuella dator (klassisk) från ett undernät tooanother i hello samma virtuella nätverk (VNet).</span><span class="sxs-lookup"><span data-stu-id="b309f-104">You can use PowerShell toomove your VMs (Classic) from one subnet tooanother in hello same virtual network (VNet).</span></span> <span data-ttu-id="b309f-105">Du kan flytta rollinstanser genom att redigera hello CSCFG-fil i stället för med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b309f-105">Role instances can be moved by editing hello CSCFG file, rather than using PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="b309f-106">Den här artikeln förklarar hur toomove virtuella datorer distribueras via hello klassiska distributionsmodellen endast.</span><span class="sxs-lookup"><span data-stu-id="b309f-106">This article explains how toomove VMs deployed through hello classic deployment model only.</span></span>
> 
> 

<span data-ttu-id="b309f-107">Varför flytta virtuella datorer tooanother undernät?</span><span class="sxs-lookup"><span data-stu-id="b309f-107">Why move VMs tooanother subnet?</span></span> <span data-ttu-id="b309f-108">Undernät migrering är användbar när hello äldre undernät är för liten och kan inte expanderas på grund av tooexisting virtuella datorer som körs i det undernätet.</span><span class="sxs-lookup"><span data-stu-id="b309f-108">Subnet migration is useful when hello older subnet is too small and cannot be expanded due tooexisting running VMs in that subnet.</span></span> <span data-ttu-id="b309f-109">I så fall kan du skapa en ny, större undernät och migrera hello VMs toohello Nytt undernät och när migreringen är klar kan du ta bort hello gamla tomma undernät.</span><span class="sxs-lookup"><span data-stu-id="b309f-109">In that case, you can create a new, larger subnet and migrate hello VMs toohello new subnet, then after migration is complete, you can delete hello old empty subnet.</span></span>

## <a name="how-toomove-a-vm-tooanother-subnet"></a><span data-ttu-id="b309f-110">Hur toomove ett tooanother datorundernät</span><span class="sxs-lookup"><span data-stu-id="b309f-110">How toomove a VM tooanother subnet</span></span>
<span data-ttu-id="b309f-111">toomove kör hello Set-AzureSubnet PowerShell-cmdleten med hello exemplet nedan som en mall för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b309f-111">toomove a VM, run hello Set-AzureSubnet PowerShell cmdlet, using hello example below as a template.</span></span> <span data-ttu-id="b309f-112">I hello exemplet nedan vi flyttar TestVM från dess nuvarande undernät tooSubnet-2.</span><span class="sxs-lookup"><span data-stu-id="b309f-112">In hello example below, we are moving TestVM from its present subnet, tooSubnet-2.</span></span> <span data-ttu-id="b309f-113">Vara säker på att tooedit hello exempel tooreflect din miljö.</span><span class="sxs-lookup"><span data-stu-id="b309f-113">Be sure tooedit hello example tooreflect your environment.</span></span> <span data-ttu-id="b309f-114">Observera att när du kör hello uppdatering AzureVM cmdlet som en del av en procedur, startas den virtuella datorn som en del av hello uppdateringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="b309f-114">Note that whenever you run hello Update-AzureVM cmdlet as part of a procedure, it will restart your VM as part of hello update process.</span></span>

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

<span data-ttu-id="b309f-115">Om du har angett en statisk interna privata IP-adress för den virtuella datorn har du tooclear inställningen innan du kan flytta hello tooa nya datorundernät.</span><span class="sxs-lookup"><span data-stu-id="b309f-115">If you specified a static internal private IP for your VM, you'll have tooclear that setting before you can move hello VM tooa new subnet.</span></span> <span data-ttu-id="b309f-116">I så fall Använd hello följande:</span><span class="sxs-lookup"><span data-stu-id="b309f-116">In that case, use hello following:</span></span>

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="toomove-a-role-instance-tooanother-subnet"></a><span data-ttu-id="b309f-117">toomove ett rollen instans tooanother undernät</span><span class="sxs-lookup"><span data-stu-id="b309f-117">toomove a role instance tooanother subnet</span></span>
<span data-ttu-id="b309f-118">toomove en rollinstans redigera hello CSCFG-filen.</span><span class="sxs-lookup"><span data-stu-id="b309f-118">toomove a role instance, edit hello CSCFG file.</span></span> <span data-ttu-id="b309f-119">I hello exemplet nedan vi flyttar ”Role0” i det virtuella nätverket *VNETName* från dess nuvarande undernät för*undernät 2*.</span><span class="sxs-lookup"><span data-stu-id="b309f-119">In hello example below, we are moving "Role0" in virtual network *VNETName* from its present subnet too*Subnet-2*.</span></span> <span data-ttu-id="b309f-120">Eftersom hello instansen redan har distribuerats, ska du bara ändra hello undernätsnamn = undernät 2.</span><span class="sxs-lookup"><span data-stu-id="b309f-120">Because hello role instance was already deployed, you'll just change hello Subnet name = Subnet-2.</span></span> <span data-ttu-id="b309f-121">Vara säker på att tooedit hello exempel tooreflect din miljö.</span><span class="sxs-lookup"><span data-stu-id="b309f-121">Be sure tooedit hello example tooreflect your environment.</span></span>

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
