---
title: "Hur du använder rollbaserad åtkomstkontroll i Azure API Management | Microsoft Docs"
description: "Lär dig hur du använder inbyggda roller och skapa anpassade roller i Azure API Management"
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
ms.openlocfilehash: fa757a591d788f52d759bc24accedd3c55149ae7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-role-based-access-control-in-azure-api-management"></a>Hur du använder rollbaserad åtkomstkontroll i Azure API Management
Azure API Management förlitar sig på rollbaserad åtkomstkontroll (RBAC) att aktivera detaljerad åtkomsthantering för API Management-tjänster och enheter (t.ex. API: er, principer). Den här artikeln ger en översikt över inbyggda och anpassade roller i API-hantering. Om du vill ha mer information om åtkomsthantering av i Azure portal finns [Kom igång med åtkomsthantering i Azure-portalen](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)

## <a name="built-in-roles"></a>Inbyggda roller
API Management för närvarande tillhandahåller 3 inbyggda roller och lägger till flera roller som 2 inom en snar framtid. Rollerna kan tilldelas på olika omfång, inklusive prenumerationen, resursgruppen och enskilda API Management-instans. Till exempel om rollen ”Azure API Management-tjänsten läsare” tilldelas till en användare på resursgruppsnivå, att kommer användaren ha läsbehörighet till alla API Management-instanser i resursgruppen. 

Följande tabell innehåller en kort beskrivning av de inbyggda rollerna. Du kan tilldela dessa roller med hjälp av Azure-portalen eller andra verktyg, inklusive Azure [PowerShell](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-powershell), Azure [kommandoradsgränssnittet](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-azure-cli), och [REST API](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-rest). Mer information om hur du tilldelar inbyggda roller finns [använda rolltilldelningar för att hantera åtkomst till resurserna i Azure-prenumeration](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/).

| Roll          | Läsbehörighet<sup>[1]</sup> | Skrivåtkomst<sup>[2]</sup> | Skapa en tjänst, borttagning, skalning, VPN och konfiguration för domänen | Åtkomst till äldre Publsiher Portal | Beskrivning
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| Azure API Management-tjänsten deltagare | ✓ | ✓ | ✓ | ✓ | Superanvändare. Har fullständig CRUD-åtkomst till API Management-tjänster och enheter (t.ex. API: er, principer). Har åtkomst till den äldra publisher-portalen. |
| Azure API Management-tjänsten läsare | ✓ | | || Har skrivskyddad åtkomst till API Management-tjänster och enheter. |
| Azure API Management-tjänsten Operator | ✓ | | ✓ | | Kan hantera API Management services, men inte enheter.|
| Azure API Management Service Editor<sup>*</sup> | ✓ | ✓ | |  | Kan hantera API Management entiteter men inte tjänster.|
| Azure API Management Content Manager<sup>*</sup> | ✓ | | | ✓ | Kan hantera developer-portalen. Skrivskyddad åtkomst till tjänster och enheter.|

<sup>[1] läsbehörighet till API Management-tjänster och enheter (t.ex. API: er, principer)</sup>

<sup>[2] skrivåtkomst till API Management-tjänster och entiteter förutom följande opeartions: 1) instans skapas, tas bort eller skalning 2) VPN-konfiguration 3) anpassad domän inställning</sup>

<sup>\*Rollen Service Editor blir tillgänglig efter vi att migrera alla admin UI från befintliga publisher portal till Azure-portalen. Rollen Innehållshanterare blir tillgänglig efter utgivare portal omstrukturerade så att den bara innehåller funktioner som rör hantering developer-portalen.</sup>  


## <a name="custom-roles"></a>Anpassade roller
Om ingen av de inbyggda rollerna uppfyller dina specifika behov kan du skapa anpassade roller för att ge mer detaljerad åtkomsthantering för API Management-enheter. Du kan till exempel skapa en anpassad roll som har skrivskyddad åtkomst till en API Management-tjänsten, men endast har skrivbehörighet till en specifik API. Läs mer om anpassade roller i [anpassade roller i Azure RBAC](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles). 

När du skapar en anpassad roll, är det lättare att börja med en av de inbyggda rollerna. Redigera attribut för att lägga till åtgärder, NotActions eller AssignableScopes och sedan spara ändringarna som en ny roll. I följande exempel börjar med rollen ”Azure API Management-tjänsten läsare” och skapar en anpassad roll som kallas ”Kalkylatorn API Editor”. Den anpassade rollen kan endast tilldelas en specifik API därför kommer har bara åtkomst till den API. 

```
$role = Get-AzureRmRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access to Contoso APIM instance and write access to the Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzureRmRoleDefinition -Role $role
New-AzureRmRoleAssignment -ObjectId <object ID of the user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

## <a name="watch-a-video-overview"></a>Titta på en videoöversikt

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player]
> 
> 

## <a name="next-steps"></a>Nästa steg

* Mer information om rollbaserad åtkomstkontroll i Azure
  * [Kom igång med åtkomsthantering i Azure-portalen](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Använda rolltilldelningar för att hantera åtkomsten till dina Azure-prenumerationsresurser](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Anpassade roller i Azure RBAC](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles)
