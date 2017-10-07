---
title: "aaaHow tooUse rollbaserad åtkomstkontroll i Azure API Management | Microsoft Docs"
description: "Lär dig hur toouse hello inbyggda roller och skapa anpassade roller i Azure API Management"
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: apimpm
ms.openlocfilehash: c51da2ff6886ebcaf796022e3a759c67f36670a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-role-based-access-control-in-azure-api-management"></a>Hur tooUse rollbaserad åtkomstkontroll i Azure API Management
Azure API Management är beroende av rollbaserad åtkomstkontroll (RBAC) tooenable detaljerad åtkomsthantering för API Management-tjänster och enheter (t.ex. API: er, principer). Den här artikeln ger en översikt över hello inbyggda och anpassade roller i API-hantering. Om du vill ha mer information om åtkomsthantering av i hello Azure-portalen finns [Kom igång med åtkomsthantering i hello Azure-portalen](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)

## <a name="built-in-roles"></a>Inbyggda roller
API-hantering för närvarande tillhandahåller 3 inbyggda roller och lägger till flera roller som 2 hello nära framtid. Rollerna kan tilldelas på olika omfång, inklusive prenumerationen, resursgruppen och enskilda API Management-instans. Exempelvis om hello ”Azure API Management-tjänsten Reader” roll har tilldelats tooan användaren vid hello resursgruppsnivå, har sedan hello användaren läsbehörighet tooall API Management instanser i hello resursgruppen. 

hello innehåller följande tabell en kort beskrivning av hello inbyggda roller. Du kan tilldela dessa roller med hjälp av hello Azure-portalen eller andra verktyg, inklusive Azure [PowerShell](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-powershell), Azure [kommandoradsgränssnittet](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-azure-cli), och [REST API](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-rest). Mer information om hur tooassign inbyggda roller finns [använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/).

| Roll          | Läsbehörighet<sup>[1]</sup> | Skrivåtkomst<sup>[2]</sup> | Skapa en tjänst, borttagning, skalning, VPN och konfiguration för domänen | Komma åt toolegacy Publsiher Portal | Beskrivning
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| Azure API Management-tjänsten deltagare | ✓ | ✓ | ✓ | ✓ | Superanvändare. Har fullständig CRUD åtkomst tooAPI tjänster och enheter (t.ex. API: er, principer). Har åtkomst toohello äldre publisher portal. |
| Azure API Management-tjänsten läsare | ✓ | | || Har skrivskyddad åtkomst tooAPI tjänster och enheter. |
| Azure API Management-tjänsten Operator | ✓ | | ✓ | | Kan hantera API Management services, men inte enheter.|
| Azure API Management Service Editor<sup>*</sup> | ✓ | ✓ | |  | Kan hantera API Management entiteter men inte tjänster.|
| Azure API Management Content Manager<sup>*</sup> | ✓ | | | ✓ | Kan hantera developer-portalen. Skrivskyddad åtkomst tooservices eller enheter.|

<sup>[1] läsbehörighet tooAPI tjänster och enheter (t.ex. API: er, principer)</sup>

<sup>[2] skrivbehörighet tooAPI tjänster och entiteter förutom följande opeartions: 1) instans skapas, tas bort eller skalning 2) VPN-konfiguration 3) anpassad domän inställning</sup>

<sup>\*hello Service Editor rollen blir tillgänglig efter vi att migrera alla admin UI från hello befintliga publisher portal toohello Azure-portalen. hello rollen Innehållshanterare blir tillgänglig när hello publisher portal omstrukturerade tooonly innehålla funktioner relaterade toomanaging hello developer-portalen.</sup>  


## <a name="custom-roles"></a>Anpassade roller
Om ingen av hello inbyggda roller uppfyller dina specifika behov anpassade roller kan skapas tooprovide mer detaljerad åtkomsthantering för API Management-enheter. Du kan till exempel skapa en anpassad roll som har skrivskyddad åtkomst tooan API Management-tjänsten men bara har skrivbehörighet tooone specifika API: et. Mer information om anpassade roller finns i toolearn [anpassade roller i Azure RBAC](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles). 

När du skapar en anpassad roll är enklare toostart med något av hello inbyggda roller. Redigera hello attribut tooadd hello åtgärder, NotActions eller AssignableScopes och sedan spara hello ändringar som en ny roll. hello följande exempel börjar med hello ”Azure API Management-tjänsten Reader” roll och skapar en anpassad roll som kallas ”Kalkylatorn API Editor”. hello anpassad roll kan tilldelas endast tooa specifika API: et därför kommer bara har åtkomst toothat API. 

```
$role = Get-AzureRmRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access tooContoso APIM instance and write access toohello Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzureRmRoleDefinition -Role $role
New-AzureRmRoleAssignment -ObjectId <object ID of hello user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

## <a name="watch-a-video-overview"></a>Titta på en videoöversikt

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player]
> 
> 

## <a name="next-steps"></a>Nästa steg

* Mer information om rollbaserad åtkomstkontroll i Azure
  * [Kom igång med åtkomsthantering i hello Azure-portalen](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Anpassade roller i Azure RBAC](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles)
