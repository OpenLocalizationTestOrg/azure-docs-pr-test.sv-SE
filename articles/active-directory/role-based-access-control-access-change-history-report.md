---
title: aaaAccess reporting - Azure RBAC | Microsoft Docs
description: "Generera en rapport som visar alla ändringar i åtkomst tooyour Azure-prenumerationer med rollbaserad åtkomstkontroll över hello senaste 90 dagarna."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9ad85d3d8e66ce167032638a35e4afffb46d3892
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-access-report-for-role-based-access-control"></a>Skapa en access-rapport för rollbaserad åtkomstkontroll
Varje gång någon tilldelar eller återkallar åtkomst i din prenumeration, får hello ändringar loggas i Azure-händelser. Du kan skapa åtkomst ändra historik rapporter toosee alla ändringar för hello senaste 90 dagarna.

## <a name="create-a-report-with-azure-powershell"></a>Skapa en rapport med Azure PowerShell
toocreate åtkomsten ändra historik i PowerShell, Använd hello [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) kommando.

När du anropar det här kommandot anger du vilken egenskap hello tilldelningar som du vill ha med, inklusive hello följande:

| Egenskap | Beskrivning |
| --- | --- |
| **Åtgärd** |Anger om åtkomst beviljas eller återkallas |
| **Anroparen** |hello ägaren ansvarar för tillgången hello ändra |
| **PrincipalId** | Unik identifierare för hello användare, grupp eller program som har tilldelats rollen hello hello |
| **Huvudkontot** |hello namnet på hello användare, grupp eller program |
| **PrincipalType** |Om hello tilldelning var för en användare, grupp eller ett program |
| **RoleDefinitionId** |hello GUID för hello roll som beviljas eller återkallas |
| **RoleName** |hello-roll som beviljas eller återkallas |
| **Omfång** | hello Unik identifierare för hello prenumerationen, resursgruppen eller resursen som hello tilldelning gäller för| 
| **ScopeName** |hello namnet på hello prenumerationen, resursgruppen eller resursen |
| **ScopeType** |Om hello tilldelningen har på hello prenumerationen, resursgruppen eller resursen omfång |
| **Tidsstämpel** |hello datum och tid då åtkomst har ändrats |

Detta kommando visar alla ändringar i hello prenumerationen för hello senaste sju dagarna:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell Get-AzureRMAuthorizationChangeLog – skärmbild](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Skapa en rapport med Azure CLI
toocreate en över åtkomständringshistorik i hello Azure-kommandoradsgränssnittet (CLI) använda hello `azure role assignment changelog list` kommando.

## <a name="export-tooa-spreadsheet"></a>Exportera tooa kalkylblad
toosave hello rapporten eller ändra hello-data, export hello ändras till en CSV-fil. Du kan sedan visa hello rapporten i ett kalkylblad för granskning.

![Changelog visas som kalkylblad – skärmbild](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="next-steps"></a>Nästa steg
* Arbeta med [anpassade roller i Azure RBAC](role-based-access-control-custom-roles.md)
* Lär dig hur toomanage [Azure RBAC med powershell](role-based-access-control-manage-access-powershell.md)

