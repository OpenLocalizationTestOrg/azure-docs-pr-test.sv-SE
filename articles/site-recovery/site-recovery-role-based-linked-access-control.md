---
title: "aaaUsing rollbaserad åtkomstkontroll toomanage Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur tooapply och använda rollbaserad åtkomstkontroll (RBAC) toomanage Azure Site Recovery-distributioner"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: manayar
ms.openlocfilehash: 7b721090351e561b28317ccdcf0ff283e0b146ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-role-based-access-control-toomanage-azure-site-recovery-deployments"></a>Använda rollbaserad åtkomstkontroll toomanage Azure Site Recovery-distributioner

Rollbaserad åtkomstkontroll (RBAC) i Azure ger tillgång till ingående åtkomsthantering för Azure. Med RBAC kan du särskilja ansvarsområden i din grupp och ge endast specifika behörigheter toousers som behövs tooperform specifika jobb.

Azure Site Recovery tillhandahåller 3 inbyggda roller toocontrol Site Recovery-hanteringsåtgärder. Läs mer på [inbyggda Azure RBAC-roller](../active-directory/role-based-access-built-in-roles.md)

* [Site Recovery-deltagare](../active-directory/role-based-access-built-in-roles.md#site-recovery-contributor) -rollen har alla behörigheter krävs toomanage Azure Site Recovery-åtgärder i Recovery Services-valvet. Skapa en användare med den här rollen, men kan inte eller ta bort Recovery Services-valvet eller tilldela åtkomst rättigheter tooother användare. Den här rollen är bäst för disaster recovery-administratörer som kan aktivera och hantera katastrofåterställning för program eller hela organisationer som hello fall.
* [Site Recovery-operatorn](../active-directory/role-based-access-built-in-roles.md#site-recovery-operator) -rollen har behörigheter tooexecute och manager redundans och återställning av åtgärder. En användare med den här rollen kan inte aktivera eller inaktivera replikering, skapa eller ta bort valv, registrera ny infrastruktur eller tilldela åtkomst rättigheter tooother användare. Den här rollen är bäst för en disaster recovery-operatör som kan redundans virtuella datorer eller program när du uppmanas av applikationsägare och IT-administratörer i en situation faktiska eller simulerade katastrofåterställning, till exempel en DR detaljgranska. Post lösning av hello katastrofåterställning hello DR-operatorn kan skydda igen och återställning efter fel hello virtuella datorer.
* [Site Recovery Reader](../active-directory/role-based-access-built-in-roles.md#site-recovery-reader) -rollen har behörigheter tooview alla Site Recovery-hanteringsåtgärder. Den här rollen passar bäst för en övervakning IT-chef som kan övervaka hello aktuell status för skydd och höja supportärenden om det behövs.

Om du tittar toodefine egna roller för ännu mer kontroll, se hur för[skapa anpassade roller](../active-directory/role-based-access-control-custom-roles.md) i Azure.

## <a name="permissions-required-tooenable-replication-for-new-virtual-machines"></a>Behörigheter som krävs tooEnable replikering för den nya virtuella datorer
När en ny virtuell dator är replikerade tooAzure med hjälp av Azure Site Recovery kan hello associerade användarens åtkomstnivåer är validerade tooensure som hello användaren hello krävs behörigheter toouse hello Azure-resurser som tooSite återställning.

tooenable replikering för en ny virtuell dator, måste användaren ha:
* Behörighet toocreate en virtuell dator i hello valda resursgruppen
* Behörighet toocreate en virtuell dator i hello valda virtuella nätverket
* Behörighet toowrite toohello valda lagringskontot

En användare behöver hello följande behörigheter toocomplete replikering av en ny virtuell dator.

> [!IMPORTANT]
>Kontrollera att relevanta behörigheter läggs per hello distributionsmodell (Resource Manager / klassiska) används för distribution av resursen.

| **Resurstyp** | **Distributionsmodell** | **Behörighet** |
| --- | --- | --- |
| Compute | Resource Manager | Microsoft.Compute/availabilitySets/read |
|  |  | Microsoft.Compute/virtualMachines/read |
|  |  | Microsoft.Compute/virtualMachines/write |
|  |  | Microsoft.Compute/virtualMachines/delete |
|  | Klassisk | Microsoft.ClassicCompute/domainNames/read |
|  |  | Microsoft.ClassicCompute/domainNames/write |
|  |  | Microsoft.ClassicCompute/domainNames/delete |
|  |  | Microsoft.ClassicCompute/virtualMachines/read |
|  |  | Microsoft.ClassicCompute/virtualMachines/write |
|  |  | Microsoft.ClassicCompute/virtualMachines/delete |
| Nätverk | Resource Manager | Microsoft.Network/networkInterfaces/read |
|  |  | Microsoft.Network/networkInterfaces/write |
|  |  | Microsoft.Network/networkInterfaces/delete |
|  |  | Microsoft.Network/networkInterfaces/join/action |
|  |  | Microsoft.Network/virtualNetworks/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/join/action |
|  | Klassisk | Microsoft.ClassicNetwork/virtualNetworks/read |
|  |  | Microsoft.ClassicNetwork/virtualNetworks/join/action |
| Lagring | Resource Manager | Microsoft.Storage/storageAccounts/read |
|  |  | Microsoft.Storage/storageAccounts/listkeys/action |
|  | Klassisk | Microsoft.ClassicStorage/storageAccounts/read |
|  |  | Microsoft.ClassicStorage/storageAccounts/listKeys/action |
| Resursgrupp | Resource Manager | Microsoft.Resources/deployments/* |
|  |  | Microsoft.Resources/subscriptions/resourceGroups/read |

Överväg att använda hello Virtual Machine-deltagare och 'Klassiska virtuella deltagare' [inbyggda roller](../active-directory/role-based-access-built-in-roles.md) för Resource Manager och klassisk distribution modeller respektive.

## <a name="next-steps"></a>Nästa steg
* [Rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md): komma igång med RBAC på hello Azure-portalen.
* Lär dig hur toomanage åt med:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST-API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Rollbaserad åtkomstkontroll felsökning](../active-directory/role-based-access-control-troubleshooting.md): få förslag om hur du löser vanliga problem.
