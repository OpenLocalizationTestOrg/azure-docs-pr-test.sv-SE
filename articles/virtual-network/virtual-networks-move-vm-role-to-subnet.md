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
# <a name="move-a-vm-classic-or-cloud-services-role-instance-tooa-different-subnet-using-powershell"></a>Flytta en virtuell dator (klassisk) eller molntjänster rollen instans tooa olika undernät med hjälp av PowerShell
Du kan använda PowerShell toomove din virtuella dator (klassisk) från ett undernät tooanother i hello samma virtuella nätverk (VNet). Du kan flytta rollinstanser genom att redigera hello CSCFG-fil i stället för med hjälp av PowerShell.

> [!NOTE]
> Den här artikeln förklarar hur toomove virtuella datorer distribueras via hello klassiska distributionsmodellen endast.
> 
> 

Varför flytta virtuella datorer tooanother undernät? Undernät migrering är användbar när hello äldre undernät är för liten och kan inte expanderas på grund av tooexisting virtuella datorer som körs i det undernätet. I så fall kan du skapa en ny, större undernät och migrera hello VMs toohello Nytt undernät och när migreringen är klar kan du ta bort hello gamla tomma undernät.

## <a name="how-toomove-a-vm-tooanother-subnet"></a>Hur toomove ett tooanother datorundernät
toomove kör hello Set-AzureSubnet PowerShell-cmdleten med hello exemplet nedan som en mall för en virtuell dator. I hello exemplet nedan vi flyttar TestVM från dess nuvarande undernät tooSubnet-2. Vara säker på att tooedit hello exempel tooreflect din miljö. Observera att när du kör hello uppdatering AzureVM cmdlet som en del av en procedur, startas den virtuella datorn som en del av hello uppdateringsprocessen.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

Om du har angett en statisk interna privata IP-adress för den virtuella datorn har du tooclear inställningen innan du kan flytta hello tooa nya datorundernät. I så fall Använd hello följande:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="toomove-a-role-instance-tooanother-subnet"></a>toomove ett rollen instans tooanother undernät
toomove en rollinstans redigera hello CSCFG-filen. I hello exemplet nedan vi flyttar ”Role0” i det virtuella nätverket *VNETName* från dess nuvarande undernät för*undernät 2*. Eftersom hello instansen redan har distribuerats, ska du bara ändra hello undernätsnamn = undernät 2. Vara säker på att tooedit hello exempel tooreflect din miljö.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
