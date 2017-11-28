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
# <a name="create-an-access-report-for-role-based-access-control"></a><span data-ttu-id="4da93-103">Skapa en access-rapport för rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="4da93-103">Create an access report for Role-Based Access Control</span></span>
<span data-ttu-id="4da93-104">Varje gång någon tilldelar eller återkallar åtkomst i din prenumeration, får hello ändringar loggas i Azure-händelser.</span><span class="sxs-lookup"><span data-stu-id="4da93-104">Any time someone grants or revokes access within your subscriptions, hello changes get logged in Azure events.</span></span> <span data-ttu-id="4da93-105">Du kan skapa åtkomst ändra historik rapporter toosee alla ändringar för hello senaste 90 dagarna.</span><span class="sxs-lookup"><span data-stu-id="4da93-105">You can create access change history reports toosee all changes for hello past 90 days.</span></span>

## <a name="create-a-report-with-azure-powershell"></a><span data-ttu-id="4da93-106">Skapa en rapport med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4da93-106">Create a report with Azure PowerShell</span></span>
<span data-ttu-id="4da93-107">toocreate åtkomsten ändra historik i PowerShell, Använd hello [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) kommando.</span><span class="sxs-lookup"><span data-stu-id="4da93-107">toocreate an access change history report in PowerShell, use hello [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) command.</span></span>

<span data-ttu-id="4da93-108">När du anropar det här kommandot anger du vilken egenskap hello tilldelningar som du vill ha med, inklusive hello följande:</span><span class="sxs-lookup"><span data-stu-id="4da93-108">When you call this command, you can specify which property of hello assignments you want listed, including hello following:</span></span>

| <span data-ttu-id="4da93-109">Egenskap</span><span class="sxs-lookup"><span data-stu-id="4da93-109">Property</span></span> | <span data-ttu-id="4da93-110">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4da93-110">Description</span></span> |
| --- | --- |
| <span data-ttu-id="4da93-111">**Åtgärd**</span><span class="sxs-lookup"><span data-stu-id="4da93-111">**Action**</span></span> |<span data-ttu-id="4da93-112">Anger om åtkomst beviljas eller återkallas</span><span class="sxs-lookup"><span data-stu-id="4da93-112">Whether access was granted or revoked</span></span> |
| <span data-ttu-id="4da93-113">**Anroparen**</span><span class="sxs-lookup"><span data-stu-id="4da93-113">**Caller**</span></span> |<span data-ttu-id="4da93-114">hello ägaren ansvarar för tillgången hello ändra</span><span class="sxs-lookup"><span data-stu-id="4da93-114">hello owner responsible for hello access change</span></span> |
| <span data-ttu-id="4da93-115">**PrincipalId**</span><span class="sxs-lookup"><span data-stu-id="4da93-115">**PrincipalId**</span></span> | <span data-ttu-id="4da93-116">Unik identifierare för hello användare, grupp eller program som har tilldelats rollen hello hello</span><span class="sxs-lookup"><span data-stu-id="4da93-116">hello unique identifier of hello user, group, or application that was assigned hello role</span></span> |
| <span data-ttu-id="4da93-117">**Huvudkontot**</span><span class="sxs-lookup"><span data-stu-id="4da93-117">**PrincipalName**</span></span> |<span data-ttu-id="4da93-118">hello namnet på hello användare, grupp eller program</span><span class="sxs-lookup"><span data-stu-id="4da93-118">hello name of hello user, group, or application</span></span> |
| <span data-ttu-id="4da93-119">**PrincipalType**</span><span class="sxs-lookup"><span data-stu-id="4da93-119">**PrincipalType**</span></span> |<span data-ttu-id="4da93-120">Om hello tilldelning var för en användare, grupp eller ett program</span><span class="sxs-lookup"><span data-stu-id="4da93-120">Whether hello assignment was for a user, group, or application</span></span> |
| <span data-ttu-id="4da93-121">**RoleDefinitionId**</span><span class="sxs-lookup"><span data-stu-id="4da93-121">**RoleDefinitionId**</span></span> |<span data-ttu-id="4da93-122">hello GUID för hello roll som beviljas eller återkallas</span><span class="sxs-lookup"><span data-stu-id="4da93-122">hello GUID of hello role that was granted or revoked</span></span> |
| <span data-ttu-id="4da93-123">**RoleName**</span><span class="sxs-lookup"><span data-stu-id="4da93-123">**RoleName**</span></span> |<span data-ttu-id="4da93-124">hello-roll som beviljas eller återkallas</span><span class="sxs-lookup"><span data-stu-id="4da93-124">hello role that was granted or revoked</span></span> |
| <span data-ttu-id="4da93-125">**Omfång**</span><span class="sxs-lookup"><span data-stu-id="4da93-125">**Scope**</span></span> | <span data-ttu-id="4da93-126">hello Unik identifierare för hello prenumerationen, resursgruppen eller resursen som hello tilldelning gäller för</span><span class="sxs-lookup"><span data-stu-id="4da93-126">hello unique identifier of hello subscription, resource group, or resource that hello assignment applies too</span></span>| 
| <span data-ttu-id="4da93-127">**ScopeName**</span><span class="sxs-lookup"><span data-stu-id="4da93-127">**ScopeName**</span></span> |<span data-ttu-id="4da93-128">hello namnet på hello prenumerationen, resursgruppen eller resursen</span><span class="sxs-lookup"><span data-stu-id="4da93-128">hello name of hello subscription, resource group, or resource</span></span> |
| <span data-ttu-id="4da93-129">**ScopeType**</span><span class="sxs-lookup"><span data-stu-id="4da93-129">**ScopeType**</span></span> |<span data-ttu-id="4da93-130">Om hello tilldelningen har på hello prenumerationen, resursgruppen eller resursen omfång</span><span class="sxs-lookup"><span data-stu-id="4da93-130">Whether hello assignment was at hello subscription, resource group, or resource scope</span></span> |
| <span data-ttu-id="4da93-131">**Tidsstämpel**</span><span class="sxs-lookup"><span data-stu-id="4da93-131">**Timestamp**</span></span> |<span data-ttu-id="4da93-132">hello datum och tid då åtkomst har ändrats</span><span class="sxs-lookup"><span data-stu-id="4da93-132">hello date and time that access was changed</span></span> |

<span data-ttu-id="4da93-133">Detta kommando visar alla ändringar i hello prenumerationen för hello senaste sju dagarna:</span><span class="sxs-lookup"><span data-stu-id="4da93-133">This example command lists all access changes in hello subscription for hello past seven days:</span></span>

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell Get-AzureRMAuthorizationChangeLog – skärmbild](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a><span data-ttu-id="4da93-135">Skapa en rapport med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4da93-135">Create a report with Azure CLI</span></span>
<span data-ttu-id="4da93-136">toocreate en över åtkomständringshistorik i hello Azure-kommandoradsgränssnittet (CLI) använda hello `azure role assignment changelog list` kommando.</span><span class="sxs-lookup"><span data-stu-id="4da93-136">toocreate an access change history report in hello Azure command-line interface (CLI), use hello `azure role assignment changelog list` command.</span></span>

## <a name="export-tooa-spreadsheet"></a><span data-ttu-id="4da93-137">Exportera tooa kalkylblad</span><span class="sxs-lookup"><span data-stu-id="4da93-137">Export tooa spreadsheet</span></span>
<span data-ttu-id="4da93-138">toosave hello rapporten eller ändra hello-data, export hello ändras till en CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="4da93-138">toosave hello report, or manipulate hello data, export hello access changes into a .csv file.</span></span> <span data-ttu-id="4da93-139">Du kan sedan visa hello rapporten i ett kalkylblad för granskning.</span><span class="sxs-lookup"><span data-stu-id="4da93-139">You can then view hello report in a spreadsheet for review.</span></span>

![Changelog visas som kalkylblad – skärmbild](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="next-steps"></a><span data-ttu-id="4da93-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4da93-141">Next steps</span></span>
* <span data-ttu-id="4da93-142">Arbeta med [anpassade roller i Azure RBAC](role-based-access-control-custom-roles.md)</span><span class="sxs-lookup"><span data-stu-id="4da93-142">Work with [Custom roles in Azure RBAC](role-based-access-control-custom-roles.md)</span></span>
* <span data-ttu-id="4da93-143">Lär dig hur toomanage [Azure RBAC med powershell](role-based-access-control-manage-access-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="4da93-143">Learn how toomanage [Azure RBAC with powershell](role-based-access-control-manage-access-powershell.md)</span></span>

