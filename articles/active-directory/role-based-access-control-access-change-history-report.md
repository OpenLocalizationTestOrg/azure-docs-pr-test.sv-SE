---
title: "Åtkomst reporting - Azure RBAC | Microsoft Docs"
description: "Generera en rapport som visar alla ändringar i åtkomst till dina Azure-prenumerationer med rollbaserad åtkomstkontroll under de senaste 90 dagarna."
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
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
ms.openlocfilehash: c430e1206e6e97f2c7fb7d2a6ff0dd6e65ee8bbf
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="create-an-access-report-for-role-based-access-control"></a>Skapa en access-rapport för rollbaserad åtkomstkontroll
Varje gång någon tilldelar eller återkallar åtkomst i din prenumeration, får ändringarna loggas i Azure-händelser. Du kan skapa åtkomst ändra historik rapporter för att visa alla ändringar för de senaste 90 dagarna.

## <a name="create-a-report-with-azure-powershell"></a>Skapa en rapport med Azure PowerShell
Om du vill skapa en profil över åtkomständringshistorik i PowerShell använder den [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) kommando.

När du anropar det här kommandot, anger du vilken egenskap tilldelningar som du vill ha med, inklusive följande:

| Egenskap | Beskrivning |
| --- | --- |
| **Åtgärd** |Anger om åtkomst beviljas eller återkallas |
| **Anroparen** |Ägaren ansvarar för att ändringen åtkomst |
| **PrincipalId** | Den unika identifieraren för användaren, gruppen eller program som har tilldelats rollen |
| **Huvudkontot** |Namnet på användaren, gruppen eller program |
| **PrincipalType** |Om tilldelningen har för en användare, grupp eller ett program |
| **RoleDefinitionId** |GUID för den roll som beviljades eller återkallats |
| **RoleName** |Rollen som beviljas eller återkallas |
| **Omfång** | Den unika identifieraren för prenumerationen, resursgruppen eller resursen som tilldelningen gäller för | 
| **ScopeName** |Namnet på prenumerationen, resursgruppen eller resursen |
| **ScopeType** |Om tilldelningen har på prenumerationen, resursgruppen eller resursen omfång |
| **Tidsstämpel** |Datum och tid då åtkomst har ändrats |

Detta kommando visar alla ändringar i prenumerationen för de senaste sju dagarna:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell Get-AzureRMAuthorizationChangeLog – skärmbild](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Skapa en rapport med Azure CLI
Så här skapar du en över åtkomständringshistorik i Azure-kommandoradsgränssnittet (CLI) i `azure role assignment changelog list` kommando.

## <a name="export-to-a-spreadsheet"></a>Exportera till ett kalkylblad
Om du vill spara rapporten eller manipulera data, exportera ändringarna åtkomst till en CSV-fil. Du kan visa rapporten i ett kalkylblad för granskning.

![Changelog visas som kalkylblad – skärmbild](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="next-steps"></a>Nästa steg
* Arbeta med [anpassade roller i Azure RBAC](role-based-access-control-custom-roles.md)
* Lär dig att hantera [Azure RBAC med powershell](role-based-access-control-manage-access-powershell.md)

