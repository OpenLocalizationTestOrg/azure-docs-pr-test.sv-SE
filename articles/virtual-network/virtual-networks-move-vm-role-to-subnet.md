---
title: "Flytta en virtuell dator (klassisk) eller molntjänster roll till ett annat undernät - Azure PowerShell | Microsoft Docs"
description: "Lär dig mer om att flytta virtuella datorer (klassisk) och molntjänster rollinstanser till ett annat undernät med hjälp av PowerShell."
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
ms.openlocfilehash: b094f8338394ef2e84cad3070936d715411326a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="move-a-vm-classic-or-cloud-services-role-instance-to-a-different-subnet-using-powershell"></a><span data-ttu-id="f0381-103">Flytta en virtuell dator (klassisk) eller molntjänster roll till ett annat undernät med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="f0381-103">Move a VM (Classic) or Cloud Services role instance to a different subnet using PowerShell</span></span>
<span data-ttu-id="f0381-104">Du kan använda PowerShell för att flytta din virtuella dator (klassisk) från ett undernät till en annan i samma virtuella nätverk (VNet).</span><span class="sxs-lookup"><span data-stu-id="f0381-104">You can use PowerShell to move your VMs (Classic) from one subnet to another in the same virtual network (VNet).</span></span> <span data-ttu-id="f0381-105">Du kan flytta rollinstanser genom att redigera filen CSCFG i stället för med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f0381-105">Role instances can be moved by editing the CSCFG file, rather than using PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="f0381-106">Den här artikeln förklarar hur du flyttar virtuella datorer distribueras via den klassiska distributionsmodellen endast.</span><span class="sxs-lookup"><span data-stu-id="f0381-106">This article explains how to move VMs deployed through the classic deployment model only.</span></span>
> 
> 

<span data-ttu-id="f0381-107">Varför flytta virtuella datorer till ett annat undernät?</span><span class="sxs-lookup"><span data-stu-id="f0381-107">Why move VMs to another subnet?</span></span> <span data-ttu-id="f0381-108">Undernät migrering är användbart när det äldre undernätet är för litet och kan inte expanderas på grund av befintliga virtuella datorer som körs i det undernätet.</span><span class="sxs-lookup"><span data-stu-id="f0381-108">Subnet migration is useful when the older subnet is too small and cannot be expanded due to existing running VMs in that subnet.</span></span> <span data-ttu-id="f0381-109">I så fall kan du skapa en ny, större undernät och migrera de virtuella datorerna till det nya undernätet och när migreringen är klar kan du ta bort gamla tomma undernät.</span><span class="sxs-lookup"><span data-stu-id="f0381-109">In that case, you can create a new, larger subnet and migrate the VMs to the new subnet, then after migration is complete, you can delete the old empty subnet.</span></span>

## <a name="how-to-move-a-vm-to-another-subnet"></a><span data-ttu-id="f0381-110">Hur du flyttar en virtuell dator till ett annat undernät</span><span class="sxs-lookup"><span data-stu-id="f0381-110">How to move a VM to another subnet</span></span>
<span data-ttu-id="f0381-111">Om du vill flytta en virtuell dator, kör Set-AzureSubnet PowerShell-cmdleten med exemplet nedan som mall.</span><span class="sxs-lookup"><span data-stu-id="f0381-111">To move a VM, run the Set-AzureSubnet PowerShell cmdlet, using the example below as a template.</span></span> <span data-ttu-id="f0381-112">I exemplet nedan flyttar vi TestVM från dess nuvarande undernät till undernät 2.</span><span class="sxs-lookup"><span data-stu-id="f0381-112">In the example below, we are moving TestVM from its present subnet, to Subnet-2.</span></span> <span data-ttu-id="f0381-113">Se till att redigera exempel för att avspegla din miljö.</span><span class="sxs-lookup"><span data-stu-id="f0381-113">Be sure to edit the example to reflect your environment.</span></span> <span data-ttu-id="f0381-114">Observera att när du kör cmdlet Update AzureVM som en del av en procedur, startas den virtuella datorn som en del av uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="f0381-114">Note that whenever you run the Update-AzureVM cmdlet as part of a procedure, it will restart your VM as part of the update process.</span></span>

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

<span data-ttu-id="f0381-115">Om du har angett en statisk interna privata IP-adress för den virtuella datorn har du avmarkera inställningen innan du kan flytta den virtuella datorn till ett nytt undernät.</span><span class="sxs-lookup"><span data-stu-id="f0381-115">If you specified a static internal private IP for your VM, you'll have to clear that setting before you can move the VM to a new subnet.</span></span> <span data-ttu-id="f0381-116">I så fall använder du följande:</span><span class="sxs-lookup"><span data-stu-id="f0381-116">In that case, use the following:</span></span>

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a><span data-ttu-id="f0381-117">Flytta en rollinstans till ett annat undernät</span><span class="sxs-lookup"><span data-stu-id="f0381-117">To move a role instance to another subnet</span></span>
<span data-ttu-id="f0381-118">Redigera CSCFG-filen om du vill flytta en rollinstans.</span><span class="sxs-lookup"><span data-stu-id="f0381-118">To move a role instance, edit the CSCFG file.</span></span> <span data-ttu-id="f0381-119">I exemplet nedan vi flyttar ”Role0” i det virtuella nätverket *VNETName* från dess nuvarande undernätet *undernät 2*.</span><span class="sxs-lookup"><span data-stu-id="f0381-119">In the example below, we are moving "Role0" in virtual network *VNETName* from its present subnet to *Subnet-2*.</span></span> <span data-ttu-id="f0381-120">Eftersom instansen redan har distribuerats, ska du bara ändra undernätnamnet = undernät 2.</span><span class="sxs-lookup"><span data-stu-id="f0381-120">Because the role instance was already deployed, you'll just change the Subnet name = Subnet-2.</span></span> <span data-ttu-id="f0381-121">Se till att redigera exempel för att avspegla din miljö.</span><span class="sxs-lookup"><span data-stu-id="f0381-121">Be sure to edit the example to reflect your environment.</span></span>

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
