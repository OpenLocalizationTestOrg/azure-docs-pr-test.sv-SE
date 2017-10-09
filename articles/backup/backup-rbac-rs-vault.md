---
title: "Hantera säkerhetskopior med rollbaserad åtkomstkontroll i Azure | Microsoft Docs"
description: "Använda rollbaserad åtkomstkontroll toomanage åtkomst toobackup hanteringsåtgärder i Recovery Services-valvet."
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
ms.openlocfilehash: 26d034d152f9b77fc6d5b2ffd5ef2648b1797f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-role-based-access-control-toomanage-azure-backup-recovery-points"></a>Använda rollbaserad åtkomstkontroll toomanage Azure Backup återställningspunkter
Rollbaserad åtkomstkontroll (RBAC) i Azure ger tillgång till ingående åtkomsthantering för Azure. Med RBAC kan du särskilja uppgifter i din grupp och ge endast hello mängd åtkomst toousers de behöver tooperform sitt arbete.

> [!IMPORTANT]
> Roller som tillhandahålls av Azure Backup är begränsad tooactions som kan utföras i Azure-portalen eller Recovery Services-valvet PowerShell-cmdlets. Åtgärder som utförs i Azure backup Agent klientens användargränssnitt eller System center Data Protection Manager UI- eller Azure Backup Server UI ligger utanför kontroll över dessa roller.

Azure Backup är 3 inbyggda roller toocontrol säkerhetskopiering hanteringsåtgärder. Läs mer på [inbyggda Azure RBAC-roller](../active-directory/role-based-access-built-in-roles.md)

* [Säkerhetskopiera deltagare](../active-directory/role-based-access-built-in-roles.md#backup-contributor) -rollen har alla behörigheter toocreate och hantera säkerhetskopia, förutom att skapa Recovery Services-valvet och ger åtkomst till tooothers. Anta rollen som administratör för hantering av säkerhetskopiering som kan göra varje säkerhetskopiering management-åtgärd.
* [Säkerhetskopiera operatorn](../active-directory/role-based-access-built-in-roles.md#backup-operator) -rollen har behörigheter tooeverything deltagare förutom att ta bort säkerhetskopiering och hantera principer för säkerhetskopiering. Den här rollen är likvärdiga toocontributor förutom att det går inte att utföra skadliga åtgärder som att stoppa säkerhetskopiering med ta bort data eller ta bort registreringen av lokala resurser.
* [Säkerhetskopiera Reader](../active-directory/role-based-access-built-in-roles.md#backup-reader) -rollen har behörigheter tooview alla säkerhetskopiering hanteringsåtgärder. Anta att den här rollen toobe någon övervakning.

Om du tittar toodefine egna roller för ännu mer kontroll, se hur för[skapa anpassade roller](../active-directory/role-based-access-control-custom-roles.md) i Azure RBAC.



## <a name="mapping-backup-built-in-roles-toobackup-management-actions"></a>Mappning av säkerhetskopiering inbyggda roller toobackup hanteringsåtgärder
i den följande tabellen hello avbildas hello hanteringsåtgärder för säkerhetskopiering och motsvarande minsta RBAC rollen krävs tooperform åtgärden.

| Management-åtgärd | Minsta RBAC rollen krävs |
| --- | --- |
| Skapa Recovery Services-valvet | Deltagare i resursgruppen för valvet |
| Aktivera säkerhetskopiering av virtuella Azure-datorer | Ansvarig för säkerhetskopiering på valvet, Virtual machine-deltagare på virtuella datorer |
| Säkerhetskopiering på begäran för den virtuella datorn | Ansvarig för säkerhetskopiering |
| Återställa VM | Ansvarig för säkerhetskopiering, resurs grupp deltagare där VM och Vnet kommer tooget distribueras |
| Återställa enskilda filer från en säkerhetskopia av Virtuella diskar | Ansvarig för säkerhetskopiering |
| Skapa princip för säkerhetskopiering för Virtuella Azure-säkerhetskopiering | Säkerhetskopiering deltagare |
| Ändra princip för säkerhetskopiering av Virtuella Azure-säkerhetskopiering | Säkerhetskopiering deltagare |
| Ta bort princip för säkerhetskopiering av Virtuella Azure-säkerhetskopiering | Säkerhetskopiering deltagare |
| Stoppa säkerhetskopiering (och behålla data eller ta bort data) på säkerhetskopiering | Säkerhetskopiering deltagare |
| Registrera lokalt Windows Server/client/SCDPM eller Azure Backup-Server | Ansvarig för säkerhetskopiering |
| Ta bort registrerade lokalt Windows Server/client/SCDPM eller Azure Backup-Server | Säkerhetskopiering deltagare |

## <a name="next-steps"></a>Nästa steg
* [Rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md): komma igång med RBAC på hello Azure-portalen.
* Lär dig hur toomanage åt med:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST-API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Rollbaserad åtkomstkontroll felsökning](../active-directory/role-based-access-control-troubleshooting.md): få förslag om hur du löser vanliga problem.
