---
title: "Hantera säkerhetskopior med rollbaserad åtkomstkontroll i Azure | Microsoft Docs"
description: "Använda rollbaserad åtkomstkontroll för att hantera åtkomst till säkerhetskopiering hanteringsåtgärder i Recovery Services-valvet."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 3bd46b97-4b29-47a5-b5ac-ac174dd36760
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/22/2017
ms.author: trinadhk;markgal
ms.openlocfilehash: b6e4c6761e1bd5c17c9c3428491113042d3b1d31
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="use-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Använda rollbaserad åtkomstkontroll för att hantera Azure Backup återställningspunkter
Rollbaserad åtkomstkontroll (RBAC) i Azure ger tillgång till ingående åtkomsthantering för Azure. Med RBAC kan du särskilja uppgifter i din grupp och ge bara mängden åtkomst till användare som de behöver för att utföra sitt arbete.

> [!IMPORTANT]
> Roller som tillhandahålls av Azure Backup är begränsade till åtgärder som kan utföras i Azure-portalen eller Recovery Services-valvet PowerShell-cmdlets. Åtgärder som utförs i Azure backup Agent klientens användargränssnitt eller System center Data Protection Manager UI- eller Azure Backup Server UI ligger utanför kontroll över dessa roller.

Azure Backup är 3 inbyggda roller för att styra säkerhetskopiering hanteringsåtgärder. Läs mer på [inbyggda Azure RBAC-roller](../active-directory/role-based-access-built-in-roles.md)

* [Säkerhetskopiera deltagare](../active-directory/role-based-access-built-in-roles.md#backup-contributor) -rollen har alla behörigheter att skapa och hantera säkerhetskopia, förutom att skapa Recovery Services-valvet och ge åtkomst till andra. Anta rollen som administratör för hantering av säkerhetskopiering som kan göra varje säkerhetskopiering management-åtgärd.
* [Säkerhetskopiera operatorn](../active-directory/role-based-access-built-in-roles.md#backup-operator) -rollen har behörighet att allt deltagare förutom att ta bort säkerhetskopiering och hantera principer för säkerhetskopiering. Den här rollen motsvarar deltagare, förutom det går inte att utföra skadliga åtgärder som stoppa säkerhetskopiering med ta bort data eller ta bort registreringen av lokala resurser.
* [Säkerhetskopiera Reader](../active-directory/role-based-access-built-in-roles.md#backup-reader) -rollen har behörighet att visa alla åtgärder för hantering av säkerhetskopiering. Anta rollen för att vara en person som övervakning.

Om du behöver för att definiera egna roller för ännu mer kontroll, se hur du [skapa anpassade roller](../active-directory/role-based-access-control-custom-roles.md) i Azure RBAC.



## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Mappa säkerhetskopiering inbyggda roller till hanteringsåtgärder för säkerhetskopiering
I följande tabell innehåller hanteringsåtgärder för säkerhetskopiering och motsvarande minsta RBAC roll som krävs för att utföra åtgärden.

| Management-åtgärd | Minsta RBAC rollen krävs |
| --- | --- |
| Skapa Recovery Services-valvet | Deltagare i resursgruppen för valvet |
| Aktivera säkerhetskopiering av virtuella Azure-datorer | Ansvarig för säkerhetskopiering på valvet, Virtual machine-deltagare på virtuella datorer |
| Säkerhetskopiering på begäran för den virtuella datorn | Ansvarig för säkerhetskopiering |
| Återställa VM | Ansvarig för säkerhetskopiering, resurs grupp deltagare där VM och Vnet kommer att distribueras |
| Återställa enskilda filer från en säkerhetskopia av Virtuella diskar | Ansvarig för säkerhetskopiering, Virtual machine-deltagare på virtuella datorer |
| Skapa princip för säkerhetskopiering för Virtuella Azure-säkerhetskopiering | Säkerhetskopiering deltagare |
| Ändra princip för säkerhetskopiering av Virtuella Azure-säkerhetskopiering | Säkerhetskopiering deltagare |
| Ta bort princip för säkerhetskopiering av Virtuella Azure-säkerhetskopiering | Säkerhetskopiering deltagare |
| Stoppa säkerhetskopiering (och behålla data eller ta bort data) på säkerhetskopiering | Säkerhetskopiering deltagare |
| Registrera lokalt Windows Server/client/SCDPM eller Azure Backup-Server | Ansvarig för säkerhetskopiering |
| Ta bort registrerade lokalt Windows Server/client/SCDPM eller Azure Backup-Server | Säkerhetskopiering deltagare |

## <a name="next-steps"></a>Nästa steg
* [Rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md): komma igång med RBAC på Azure-portalen.
* Lär dig mer om att hantera åtkomst med:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Rollbaserad åtkomstkontroll felsökning](../active-directory/role-based-access-control-troubleshooting.md): få förslag om hur du löser vanliga problem.
