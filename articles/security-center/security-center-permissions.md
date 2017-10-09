---
title: aaaPermissions i Azure Security Center | Microsoft Docs
description: "Den här artikeln beskrivs hur Azure Security Center använder rollbaserad åtkomstkontroll tooassign behörigheter toousers och identifierar hello tillåtna åtgärder för varje roll."
services: security-center
cloud: na
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
ms.assetid: 
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: terrylan
ms.openlocfilehash: 03e16132dc3d951ef8ad9e86b9970b9e4d15c76b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="permissions-in-azure-security-center"></a>Behörigheter i Azure Security Center

Azure Security Center använder [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md), vilket ger [inbyggda roller](../active-directory/role-based-access-built-in-roles.md) som kan tilldelas toousers, grupper och tjänster i Azure.

Security Center utvärderar hello konfigurationen av dina resurser tooidentify säkerhetsproblem och säkerhetsproblem. I Security Center, du kan bara se information relaterade tooa resurs när du har tilldelats hello rollen ägare, deltagare eller läsare för hello prenumerationen eller resursen gruppen som en resurs tillhör.

Det finns två specifika roller i Security Center i tillägg toothese roller:

* **Säkerhet Reader**: en användare som tillhör toothis roll har visa rättigheter tooSecurity Center. hello användare kan visa rekommendationer, aviseringar, en säkerhetsprincip och säkerhetsstatus, men det går inte att göra ändringar.
* **Säkerhetsadministratör**: en användare som tillhör toothis roll har samma rättigheter som hello säkerhet Reader hello och de kan också uppdatera hello säkerhetsprincip och stänga aviseringar och rekommendationer.

> [!NOTE]
> hello säkerhetsroller, säkerhet, läsare och säkerhetsadministratör, komma åt endast i Security Center. hello säkerhetsroller har inte åtkomst tooother service områden i Azure, till exempel lagring, webb & Mobile eller Sakernas Internet.
>
>

## <a name="roles-and-allowed-actions"></a>Roller och tillåtna åtgärder

hello följande tabell visar roller och tillåtna åtgärder i Security Center. Ett X anger att hello-åtgärd tillåts för rollen.

| Roll | Redigera säkerhetsprincip | Tillämpa säkerhetsrekommendationer på en resurs | Avvisa aviseringar och rekommendationer | Visa aviseringar och rekommendationer |
|:--- |:---:|:---:|:---:|:---:|
| Prenumerationsägaren | X | X | X | X |
| Deltagare i prenumeration | X | X | X | X |
| Ägaren av resursgruppen. | -- | X | -- | X |
| Deltagare i resursgrupp | -- | X | -- | X |
| Läsare | -- | -- | -- | X |
| Säkerhetsadministratör | X | -- | X | X |
| Säkerhet läsare | -- | -- | -- | X |

> [!NOTE]
> Vi rekommenderar att du ger hello minst Tillåtande rollen som behövs för användare toocomplete sina uppgifter. Till exempel tilldela hello Reader rollen toousers som bara behöver tooview information om hello säkerhetshälsa för en resurs, men inte vidta några åtgärder, till exempel tillämpa rekommendationer eller ändra principer.
>
>

## <a name="next-steps"></a>Nästa steg
Den här artikeln förklaras hur Security Center använder RBAC tooassign behörigheter toousers och identifieras hello tillåtna åtgärder för varje roll. Nu när du är bekant med hello rolltilldelningar behövs toomonitor hello säkerhetsläget för din prenumeration, redigera IPSec-principer och tillämpa rekommendationer, Läs mer om hur du:

- [Ange säkerhetsprinciper i Security Center](security-center-policies.md)
- [Hantera säkerhetsrekommendationer i Security Center](security-center-recommendations.md)
- [Övervaka hello säkerhetshälsa i Azure-resurser](security-center-monitoring.md)
- [Hanterar och åtgärdar toosecurity aviseringar i Security Center](security-center-managing-and-responding-alerts.md)
- [Övervaka partnerlösningar för säkerhet](security-center-partner-solutions.md)
