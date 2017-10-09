---
title: aaaProtecting DNS-zoner och poster | Microsoft Docs
description: Tooprotect DNS-zoner och registrera anger hur i Microsoft Azure DNS.
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 190e69eb-e820-4fc8-8e9a-baaf0b3fb74a
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/20/2016
ms.author: jonatul
ms.openlocfilehash: 7945f6240feeed3d79a11d340f9f845e083026ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprotect-dns-zones-and-records"></a><span data-ttu-id="b2def-103">Hur tooprotect DNS zoner och registrerar</span><span class="sxs-lookup"><span data-stu-id="b2def-103">How tooprotect DNS zones and records</span></span>

<span data-ttu-id="b2def-104">DNS-zoner och poster är viktiga resurser.</span><span class="sxs-lookup"><span data-stu-id="b2def-104">DNS zones and records are critical resources.</span></span> <span data-ttu-id="b2def-105">Ta bort en DNS-zon eller bara en DNS-post kan resultera i en totala avbrott.</span><span class="sxs-lookup"><span data-stu-id="b2def-105">Deleting a DNS zone or even just a single DNS record can result in a total service outage.</span></span>  <span data-ttu-id="b2def-106">Det är därför viktigt att kritiska DNS-zoner och poster skyddas mot obehörig eller oavsiktliga ändringar.</span><span class="sxs-lookup"><span data-stu-id="b2def-106">It is therefore important that critical DNS zones and records are protected against unauthorized or accidental changes.</span></span>

<span data-ttu-id="b2def-107">Den här artikeln förklarar hur Azure DNS gör du tooprotect din DNS-zoner och poster mot sådana ändringar.</span><span class="sxs-lookup"><span data-stu-id="b2def-107">This article explains how Azure DNS enables you tooprotect your DNS zones and records against such changes.</span></span>  <span data-ttu-id="b2def-108">Det använda två kraftfulla säkerhetsfunktioner som tillhandahålls av Azure Resource Manager: [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md) och [resurslås](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="b2def-108">We apply two powerful security features provided by Azure Resource Manager: [role-based access control](../active-directory/role-based-access-control-what-is.md) and [resource locks](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

## <a name="role-based-access-control"></a><span data-ttu-id="b2def-109">Rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="b2def-109">Role-based access control</span></span>

<span data-ttu-id="b2def-110">Azure rollbaserad åtkomstkontroll (RBAC) Aktivera detaljerad åtkomsthantering för Azure-användare, grupper och resurser.</span><span class="sxs-lookup"><span data-stu-id="b2def-110">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure users, groups, and resources.</span></span> <span data-ttu-id="b2def-111">Med RBAC kan bevilja du exakt hello mängden åtkomst att användarna måste tooperform sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="b2def-111">Using RBAC, you can grant precisely hello amount of access that users need tooperform their jobs.</span></span> <span data-ttu-id="b2def-112">Mer information om hur RBAC kan hjälpa dig att hantera åtkomsten finns [vad är rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="b2def-112">For more information about how RBAC helps you manage access, see [What is Role-Based Access Control](../active-directory/role-based-access-control-what-is.md).</span></span>

### <a name="hello-dns-zone-contributor-role"></a><span data-ttu-id="b2def-113">hello-DNS-zonen ' deltagarrollen</span><span class="sxs-lookup"><span data-stu-id="b2def-113">hello 'DNS Zone Contributor' role</span></span>

<span data-ttu-id="b2def-114">hello är DNS-zonen-deltagare en inbyggd roll som tillhandahålls av Azure för att hantera DNS-resurser.</span><span class="sxs-lookup"><span data-stu-id="b2def-114">hello 'DNS Zone Contributor' role is a built-in role provided by Azure for managing DNS resources.</span></span>  <span data-ttu-id="b2def-115">Tilldela DNS-zonen deltagare behörigheter tooa användare eller grupp kan den grupp toomanage DNS-resurser, men inte resurser för andra typer.</span><span class="sxs-lookup"><span data-stu-id="b2def-115">Assigning DNS Zone Contributor permissions tooa user or group enables that group toomanage DNS resources, but not resources of any other type.</span></span>

<span data-ttu-id="b2def-116">Anta exempelvis att myzones' hello resurs grupp' innehåller fem zoner för Contoso Corporation.</span><span class="sxs-lookup"><span data-stu-id="b2def-116">For example, suppose hello resource group 'myzones' contains five zones for Contoso Corporation.</span></span> <span data-ttu-id="b2def-117">Beviljande hello DNS-administratören DNS-zonen-deltagare behörigheter toothat resursgrupp, kan fullständig kontroll över dessa DNS-zoner.</span><span class="sxs-lookup"><span data-stu-id="b2def-117">Granting hello DNS administrator 'DNS Zone Contributor' permissions toothat resource group, enables full control over those DNS zones.</span></span> <span data-ttu-id="b2def-118">Den förhindrar tillståndsbeviljande onödiga, till exempel hello DNS-administratören inte kan skapa eller stoppa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b2def-118">It also avoids granting unnecessary permissions, for example hello DNS administrator cannot create or stop Virtual Machines.</span></span>

<span data-ttu-id="b2def-119">hello enklaste sättet tooassign RBAC behörigheter [via hello Azure-portalen](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="b2def-119">hello simplest way tooassign RBAC permissions is [via hello Azure portal](../active-directory/role-based-access-control-configure.md).</span></span>  <span data-ttu-id="b2def-120">Öppna hello-åtkomstkontroll (IAM)-bladet för hello resursgrupp, och sedan klicka på ”Lägg till' och välj sedan hello DNS-zonen-deltagare och välj hello krävs användare eller grupper toogrant behörigheter.</span><span class="sxs-lookup"><span data-stu-id="b2def-120">Open hello 'Access control (IAM)' blade for hello resource group, then click 'Add', then select hello 'DNS Zone Contributor' role and select hello required users or groups toogrant permissions.</span></span>

![Resursgruppsnivå RBAC via hello Azure-portalen](./media/dns-protect-zones-recordsets/rbac1.png)

<span data-ttu-id="b2def-122">Behörigheter kan också vara [beviljas med hjälp av Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="b2def-122">Permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant 'DNS Zone Contributor' permissions tooall zones in a resource group
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

<span data-ttu-id="b2def-123">hello likvärdigt kommando är också [tillgängliga via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="b2def-123">hello equivalent command is also [available via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant 'DNS Zone Contributor' permissions tooall zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a><span data-ttu-id="b2def-124">Zonen nivå RBAC</span><span class="sxs-lookup"><span data-stu-id="b2def-124">Zone level RBAC</span></span>

<span data-ttu-id="b2def-125">Azure RBAC-reglerna kan vara tillämpade tooa prenumeration, en resurs grupp eller tooan enskilda resurs.</span><span class="sxs-lookup"><span data-stu-id="b2def-125">Azure RBAC rules can be applied tooa subscription, a resource group or tooan individual resource.</span></span> <span data-ttu-id="b2def-126">Hello gäller Azure DNS, kan resursen vara en enskild DNS-zon eller även en enskild uppsättning av poster.</span><span class="sxs-lookup"><span data-stu-id="b2def-126">In hello case of Azure DNS, that resource can be an individual DNS zone, or even an individual record set.</span></span>

<span data-ttu-id="b2def-127">Anta exempelvis att myzones' hello resurs grupp' innehåller hello zonen ”contoso.com” och en underdomänen 'customers.contoso.com' där CNAME-poster skapas för varje kundkonto.</span><span class="sxs-lookup"><span data-stu-id="b2def-127">For example, suppose hello resource group 'myzones' contains hello zone 'contoso.com' and a subzone 'customers.contoso.com' in which CNAME records are created for each customer account.</span></span>  <span data-ttu-id="b2def-128">hello konto som används för toomanage dessa CNAME-poster ska tilldelas behörigheter toocreate poster i hello 'customers.contoso.com' zon, den får inte ha åtkomst toohello andra zoner.</span><span class="sxs-lookup"><span data-stu-id="b2def-128">hello account used toomanage these CNAME records should be assigned permissions toocreate records in hello 'customers.contoso.com' zone only, it should not have access toohello other zones.</span></span>

<span data-ttu-id="b2def-129">Zonen RBAC-behörighet kan beviljas via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b2def-129">Zone-level RBAC permissions can be granted via hello Azure portal.</span></span>  <span data-ttu-id="b2def-130">Öppna hello-åtkomstkontroll (IAM)-bladet för hello zon, och sedan klicka på ”Lägg till' och välj sedan hello DNS-zonen-deltagare och välj hello krävs användare eller grupper toogrant behörigheter.</span><span class="sxs-lookup"><span data-stu-id="b2def-130">Open hello 'Access control (IAM)' blade for hello zone, then click 'Add', then select hello 'DNS Zone Contributor' role and select hello required users or groups toogrant permissions.</span></span>

![DNS-zonen nivå RBAC via hello Azure-portalen](./media/dns-protect-zones-recordsets/rbac2.png)

<span data-ttu-id="b2def-132">Behörigheter kan också vara [beviljas med hjälp av Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="b2def-132">Permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant 'DNS Zone Contributor' permissions tooa specific zone
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

<span data-ttu-id="b2def-133">hello likvärdigt kommando är också [tillgängliga via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="b2def-133">hello equivalent command is also [available via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant 'DNS Zone Contributor' permissions tooa specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a><span data-ttu-id="b2def-134">Postuppsättning nivå RBAC</span><span class="sxs-lookup"><span data-stu-id="b2def-134">Record set level RBAC</span></span>

<span data-ttu-id="b2def-135">Vi kan ta ytterligare ett steg.</span><span class="sxs-lookup"><span data-stu-id="b2def-135">We can go one step further.</span></span> <span data-ttu-id="b2def-136">Överväg att hello e-administratören för Contoso Corporation, som behöver åtkomst toohello MX och TXT-poster på hello apex av hello ”contoso.com” zon.</span><span class="sxs-lookup"><span data-stu-id="b2def-136">Consider hello mail administrator for Contoso Corporation, who needs access toohello MX and TXT records at hello apex of hello 'contoso.com' zone.</span></span>  <span data-ttu-id="b2def-137">Hon inte behöver komma åt tooany andra MX och TXT-poster eller poster tooany av någon annan typ.</span><span class="sxs-lookup"><span data-stu-id="b2def-137">She doesn't need access tooany other MX or TXT records, or tooany records of any other type.</span></span>  <span data-ttu-id="b2def-138">Azure DNS kan du tooassign behörigheter på hello postuppsättning genom tooprecisely hello poster som hello e-administratör behöver åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="b2def-138">Azure DNS allows you tooassign permissions at hello record set level, tooprecisely hello records that hello mail administrator needs access to.</span></span>  <span data-ttu-id="b2def-139">hello e-administratör beviljas exakt hello Kontrollera hon behöver och är toomake andra ändringar.</span><span class="sxs-lookup"><span data-stu-id="b2def-139">hello mail administrator is granted precisely hello control she needs, and is unable toomake any other changes.</span></span>

<span data-ttu-id="b2def-140">Postuppsättningen behörighet på användarnivå RBAC kan konfigureras via hello Azure-portalen med hjälp av knappen hello användare i hello postuppsättning bladet:</span><span class="sxs-lookup"><span data-stu-id="b2def-140">Record-set level RBAC permissions can be configured via hello Azure portal, using hello 'Users' button in hello record set blade:</span></span>

![Postuppsättning nivå RBAC via hello Azure-portalen](./media/dns-protect-zones-recordsets/rbac3.png)

<span data-ttu-id="b2def-142">Postuppsättningen behörighet på användarnivå RBAC kan också vara [beviljas med hjälp av Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="b2def-142">Record-set level RBAC permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant permissions tooa specific record set
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

<span data-ttu-id="b2def-143">hello likvärdigt kommando är också [tillgängliga via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="b2def-143">hello equivalent command is also [available via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant permissions tooa specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a><span data-ttu-id="b2def-144">Anpassade roller</span><span class="sxs-lookup"><span data-stu-id="b2def-144">Custom roles</span></span>

<span data-ttu-id="b2def-145">hello inbyggda DNS-zonen-deltagare rollen ger fullständig kontroll över DNS-resursposter.</span><span class="sxs-lookup"><span data-stu-id="b2def-145">hello built-in 'DNS Zone Contributor' role enables full control over a DNS resource.</span></span> <span data-ttu-id="b2def-146">Det är också möjligt toobuild egna kund Azure roller, tooprovide ännu mer detaljerad kontroll.</span><span class="sxs-lookup"><span data-stu-id="b2def-146">It is also possible toobuild your own customer Azure roles, tooprovide even finer-grained control.</span></span>

<span data-ttu-id="b2def-147">Fundera igen hello exempel som en CNAME-post i zonen hello 'customers.contoso.com' skapas för varje kundkonto för Contoso Corporation.</span><span class="sxs-lookup"><span data-stu-id="b2def-147">Consider again hello example in which a CNAME record in hello zone 'customers.contoso.com' is created for each Contoso Corporation customer account.</span></span>  <span data-ttu-id="b2def-148">hello konto som används för toomanage dessa CNAME-resursposter beviljas behörighet toomanage CNAME poster.</span><span class="sxs-lookup"><span data-stu-id="b2def-148">hello account used toomanage these CNAMEs should be granted permission toomanage CNAME records only.</span></span>  <span data-ttu-id="b2def-149">Det är toomodify poster av andra typer (till exempel ändra MX-poster) eller utföra åtgärder på zonnivå, till exempel ta bort zonen.</span><span class="sxs-lookup"><span data-stu-id="b2def-149">It is then unable toomodify records of other types (such as changing MX records) or perform zone-level operations such as zone delete.</span></span>

<span data-ttu-id="b2def-150">hello som följande exempel visar en anpassad rolldefinition för att hantera CNAME-poster:</span><span class="sxs-lookup"><span data-stu-id="b2def-150">hello following example shows a custom role definition for managing CNAME records only:</span></span>

```json
{
    "Name": "DNS CNAME Contributor",
    "Id": "",
    "IsCustom": true,
    "Description": "Can manage DNS CNAME records only.",
    "Actions": [
        "Microsoft.Network/dnsZones/CNAME/*",
        "Microsoft.Network/dnsZones/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
    ],
    "NotActions": [
    ],
    "AssignableScopes": [
        "/subscriptions/ c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
}
```

<span data-ttu-id="b2def-151">hello åtgärder egenskapen definierar hello följande DNS-specifika behörigheter:</span><span class="sxs-lookup"><span data-stu-id="b2def-151">hello Actions property defines hello following DNS-specific permissions:</span></span>

* <span data-ttu-id="b2def-152">`Microsoft.Network/dnsZones/CNAME/*`ger fullständig kontroll över CNAME-poster</span><span class="sxs-lookup"><span data-stu-id="b2def-152">`Microsoft.Network/dnsZones/CNAME/*` grants full control over CNAME records</span></span>
* <span data-ttu-id="b2def-153">`Microsoft.Network/dnsZones/read`beviljar behörighet tooread DNS-zoner, men inte toomodify dem, aktiverar du toosee hello zonen i vilka hello CNAME håller på att skapas.</span><span class="sxs-lookup"><span data-stu-id="b2def-153">`Microsoft.Network/dnsZones/read` grants permission tooread DNS zones, but not toomodify them, enabling you toosee hello zone in which hello CNAME is being created.</span></span>

<span data-ttu-id="b2def-154">hello återstående åtgärder kopieras från hello [DNS-zonen inbyggda deltagarrollen](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).</span><span class="sxs-lookup"><span data-stu-id="b2def-154">hello remaining Actions are copied from hello [DNS Zone Contributor built-in role](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).</span></span>

> [!NOTE]
> <span data-ttu-id="b2def-155">Med hjälp av en anpassad RBAC rollen tooprevent bort post anger fortfarande så att de toobe uppdateras inte är en effektiv kontroll.</span><span class="sxs-lookup"><span data-stu-id="b2def-155">Using a custom RBAC role tooprevent deleting record sets while still allowing them toobe updated is not an effective control.</span></span> <span data-ttu-id="b2def-156">Det förhindrar postuppsättningar tas bort, men den förhindrar inte dem som ska ändras.</span><span class="sxs-lookup"><span data-stu-id="b2def-156">It prevents record sets from being deleted, but it does not prevent them from being modified.</span></span>  <span data-ttu-id="b2def-157">Tillåtna ändringar omfattar att lägga till och ta bort poster från hello postuppsättningen, inklusive ta bort alla poster tooleave en 'empty-postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="b2def-157">Permitted modifications include adding and removing records from hello record set, including removing all records tooleave an 'empty' record set.</span></span> <span data-ttu-id="b2def-158">Detta har samma effekt som tar bort hello post ange från en DNS-matchning synvinkel hello.</span><span class="sxs-lookup"><span data-stu-id="b2def-158">This has hello same effect as deleting hello record set from a DNS resolution viewpoint.</span></span>

<span data-ttu-id="b2def-159">Anpassade Rolldefinitioner kan för närvarande inte definieras via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b2def-159">Custom role definitions cannot currently be defined via hello Azure portal.</span></span> <span data-ttu-id="b2def-160">En anpassad roll som baseras på den här rolldefinition kan skapas med hjälp av Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b2def-160">A custom role based on this role definition can be created using Azure PowerShell:</span></span>

```powershell
# Create new role definition based on input file
New-AzureRmRoleDefinition -InputFile <file path>
```

<span data-ttu-id="b2def-161">Det kan även skapas via hello Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="b2def-161">It can also be created via hello Azure CLI:</span></span>

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

<span data-ttu-id="b2def-162">hello roll kan sedan tilldelas i hello samma sätt som inbyggda roller, enligt beskrivningen tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="b2def-162">hello role can then be assigned in hello same way as built-in roles, as described earlier in this article.</span></span>

<span data-ttu-id="b2def-163">Mer information om hur toocreate, hantera och tilldela anpassade roller finns [anpassade roller i Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="b2def-163">For more information on how toocreate, manage, and assign custom roles, see [Custom Roles in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span></span>

## <a name="resource-locks"></a><span data-ttu-id="b2def-164">Resurslås</span><span class="sxs-lookup"><span data-stu-id="b2def-164">Resource locks</span></span>

<span data-ttu-id="b2def-165">I tillägg tooRBAC Azure Resource Manager har stöd för en annan typ av säkerhetskontroll, nämligen hello möjlighet too'lock' resurser.</span><span class="sxs-lookup"><span data-stu-id="b2def-165">In addition tooRBAC, Azure Resource Manager supports another type of security control, namely hello ability too'lock' resources.</span></span> <span data-ttu-id="b2def-166">Om RBAC reglerna tillåter toocontrol hello åtgärder för specifika användare och grupper, resurslås är tillämpade toohello resurs och börjar gälla för alla användare och roller.</span><span class="sxs-lookup"><span data-stu-id="b2def-166">Where RBAC rules allow you toocontrol hello actions of specific users and groups, resource locks are applied toohello resource, and are effective across all users and roles.</span></span> <span data-ttu-id="b2def-167">Mer information finns i [Låsa resurser med Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="b2def-167">For more information, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

<span data-ttu-id="b2def-168">Det finns två typer av resurslås: **DoNotDelete** och **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="b2def-168">There are two types of resource lock: **DoNotDelete** and **ReadOnly**.</span></span> <span data-ttu-id="b2def-169">Dessa kan tillämpas tooa DNS-zon eller tooan enskilda postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="b2def-169">These can be applied either tooa DNS zone, or tooan individual record set.</span></span>  <span data-ttu-id="b2def-170">hello följande avsnitt beskrivs några vanliga scenarier och hur toosupport dem med hjälp av resource Lås.</span><span class="sxs-lookup"><span data-stu-id="b2def-170">hello following sections describe several common scenarios, and how toosupport them using resource locks.</span></span>

### <a name="protecting-against-all-changes"></a><span data-ttu-id="b2def-171">Skydd mot alla ändringar</span><span class="sxs-lookup"><span data-stu-id="b2def-171">Protecting against all changes</span></span>

<span data-ttu-id="b2def-172">tooprevent ändringar som gjorts, tillämpa en ReadOnly Lås toohello zon.</span><span class="sxs-lookup"><span data-stu-id="b2def-172">tooprevent any changes being made, apply a ReadOnly lock toohello zone.</span></span>  <span data-ttu-id="b2def-173">Detta förhindrar att nya postuppsättningar skapas och befintliga postuppsättningar från att ändras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="b2def-173">This prevents new record sets from being created, and existing record sets from being modified or deleted.</span></span>

<span data-ttu-id="b2def-174">Zonen nivån resurslås kan skapas via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b2def-174">Zone level resource locks can be created via hello Azure portal.</span></span>  <span data-ttu-id="b2def-175">Klicka på Lås, hello DNS-zonen bladet Lägg sedan 'till':</span><span class="sxs-lookup"><span data-stu-id="b2def-175">From hello DNS zone blade, click 'Locks', then 'Add':</span></span>

![Zonen nivån resurslås via hello Azure-portalen](./media/dns-protect-zones-recordsets/locks1.png)

<span data-ttu-id="b2def-177">På zonnivå resurs Lås kan även skapas via Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b2def-177">Zone-level resource locks can also be created via Azure PowerShell:</span></span>

```powershell
# Lock a DNS zone
New-AzureRmResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

<span data-ttu-id="b2def-178">Konfigurera Azure-resurslås stöds inte för närvarande via hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b2def-178">Configuring Azure resource locks is not currently supported via hello Azure CLI.</span></span>

### <a name="protecting-individual-records"></a><span data-ttu-id="b2def-179">Skydda enskilda poster</span><span class="sxs-lookup"><span data-stu-id="b2def-179">Protecting individual records</span></span>

<span data-ttu-id="b2def-180">tooprevent en befintlig DNS-post mot ändringar, gäller en ReadOnly Lås toohello postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="b2def-180">tooprevent an existing DNS record set against modification, apply a ReadOnly lock toohello record set.</span></span>

> [!NOTE]
> <span data-ttu-id="b2def-181">Tillämpa en DoNotDelete Lås tooa är postuppsättning inte en effektiv kontroll.</span><span class="sxs-lookup"><span data-stu-id="b2def-181">Applying a DoNotDelete lock tooa record set is not an effective control.</span></span> <span data-ttu-id="b2def-182">Det förhindrar hello-postuppsättning tas bort, men den förhindrar inte att den kan ändras.</span><span class="sxs-lookup"><span data-stu-id="b2def-182">It prevents hello record set from being deleted, but it does not prevent it from being modified.</span></span>  <span data-ttu-id="b2def-183">Tillåtna ändringar omfattar att lägga till och ta bort poster från hello postuppsättningen, inklusive ta bort alla poster tooleave en 'empty-postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="b2def-183">Permitted modifications include adding and removing records from hello record set, including removing all records tooleave an 'empty' record set.</span></span> <span data-ttu-id="b2def-184">Detta har samma effekt som tar bort hello post ange från en DNS-matchning synvinkel hello.</span><span class="sxs-lookup"><span data-stu-id="b2def-184">This has hello same effect as deleting hello record set from a DNS resolution viewpoint.</span></span>

<span data-ttu-id="b2def-185">Postuppsättning nivån resurslås för närvarande kan bara konfigureras med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b2def-185">Record set level resource locks can currently only be configured using Azure PowerShell.</span></span>  <span data-ttu-id="b2def-186">De stöds inte i hello Azure-portalen eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b2def-186">They are not supported in hello Azure portal or Azure CLI.</span></span>

```powershell
# Lock a DNS record set
New-AzureRmResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a><span data-ttu-id="b2def-187">Skydd mot zon borttagning</span><span class="sxs-lookup"><span data-stu-id="b2def-187">Protecting against zone deletion</span></span>

<span data-ttu-id="b2def-188">När en zon tas bort i Azure DNS, alla uppsättningar av poster i zonen hello också att tas bort.</span><span class="sxs-lookup"><span data-stu-id="b2def-188">When a zone is deleted in Azure DNS, all record sets in hello zone are also deleted.</span></span>  <span data-ttu-id="b2def-189">Den här åtgärden kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="b2def-189">This operation cannot be undone.</span></span>  <span data-ttu-id="b2def-190">Oavsiktlig borttagning av en kritisk zon har hello potentiella toohave betydande marknadsfördelar.</span><span class="sxs-lookup"><span data-stu-id="b2def-190">Accidentally deleting a critical zone has hello potential toohave a significant business impact.</span></span>  <span data-ttu-id="b2def-191">Därför är det mycket viktigt tooprotect mot oavsiktlig zonen borttagning.</span><span class="sxs-lookup"><span data-stu-id="b2def-191">It is therefore very important tooprotect against accidental zone deletion.</span></span>

<span data-ttu-id="b2def-192">Tillämpa en DoNotDelete Lås tooa zon förhindrar hello zon tas bort.</span><span class="sxs-lookup"><span data-stu-id="b2def-192">Applying a DoNotDelete lock tooa zone prevents hello zone from being deleted.</span></span>  <span data-ttu-id="b2def-193">Men eftersom Lås ärvs av underordnade resurser den förhindrar också att alla uppsättningar av poster i hello zon tas bort, vilket kan vara önskvärt.</span><span class="sxs-lookup"><span data-stu-id="b2def-193">However, since locks are inherited by child resources, it also prevents any record sets in hello zone from being deleted, which may be undesirable.</span></span>  <span data-ttu-id="b2def-194">Dessutom enligt beskrivningen i hello Obs ovan, är det också ineffektiv eftersom poster fortfarande kan tas bort från hello befintliga postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="b2def-194">Furthermore, as described in hello note above, it is also ineffective since records can still be removed from hello existing record sets.</span></span>

<span data-ttu-id="b2def-195">Överväg att använda en DoNotDelete Lås tooa post som i hello zonen, till exempel hello SOA-postuppsättning som ett alternativ.</span><span class="sxs-lookup"><span data-stu-id="b2def-195">As an alternative, consider applying a DoNotDelete lock tooa record set in hello zone, such as hello SOA record set.</span></span>  <span data-ttu-id="b2def-196">Eftersom hello zon inte kan tas bort utan att även ta bort hello postuppsättningar, skyddar detta mot zon borttagning, men ändå låta postuppsättningar inom hello zonen toobe ändrade fritt.</span><span class="sxs-lookup"><span data-stu-id="b2def-196">Since hello zone cannot be deleted without also deleting hello record sets, this protects against zone deletion, while still allowing record sets within hello zone toobe modified freely.</span></span> <span data-ttu-id="b2def-197">Om ett försök görs toodelete hello zonen, identifierar Azure Resource Manager detta skulle även bort hello SOA-postuppsättning och block hello anropet eftersom hello SOA är låst.</span><span class="sxs-lookup"><span data-stu-id="b2def-197">If an attempt is made toodelete hello zone, Azure Resource Manager detects this would also delete hello SOA record set, and blocks hello call because hello SOA is locked.</span></span>  <span data-ttu-id="b2def-198">Inga postuppsättningar tas bort.</span><span class="sxs-lookup"><span data-stu-id="b2def-198">No record sets are deleted.</span></span>

<span data-ttu-id="b2def-199">hello följande PowerShell-kommando skapar ett DoNotDelete Lås mot hello SOA-post på hello angivna zonen:</span><span class="sxs-lookup"><span data-stu-id="b2def-199">hello following PowerShell command creates a DoNotDelete lock against hello SOA record of hello given zone:</span></span>

```powershell
# Protect against zone delete with DoNotDelete lock on hello record set
New-AzureRmResourceLock -LockLevel DoNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

<span data-ttu-id="b2def-200">Ett annat sätt tooprevent zon för oavsiktlig borttagning är med hjälp av en anpassad roll tooensure hello operatorn och toomanage för konton som används av tjänsten inte har zonen zonerna ta bort behörigheter.</span><span class="sxs-lookup"><span data-stu-id="b2def-200">Another way tooprevent accidental zone deletion is by using a custom role tooensure hello operator and service accounts used toomanage your zones do not have zone delete permissions.</span></span> <span data-ttu-id="b2def-201">När du behöver toodelete en zon, kan du använda en tvåstegsverifiering ta bort, första beviljande zonen behörighet att ta bort (definitionsområdet hello zonen, tooprevent bort hello fel zon) och andra toodelete hello zon.</span><span class="sxs-lookup"><span data-stu-id="b2def-201">When you do need toodelete a zone, you can enforce a two-step delete, first granting zone delete permissions (at hello zone scope, tooprevent deleting hello wrong zone) and second toodelete hello zone.</span></span>

<span data-ttu-id="b2def-202">Den här andra metoden har hello fördelen att det fungerar för alla zoner som har åtkomst till dessa konton utan att behöva tooremember toocreate alla Lås.</span><span class="sxs-lookup"><span data-stu-id="b2def-202">This second approach has hello advantage that it works for all zones accessed by those accounts, without having tooremember toocreate any locks.</span></span> <span data-ttu-id="b2def-203">Den har hello Nackdelen är att alla konton med behörighet att zonen ta bort, till exempel hello prenumerationsägaren fortfarande av misstag kan ta bort en zon för kritiska.</span><span class="sxs-lookup"><span data-stu-id="b2def-203">It has hello disadvantage that any accounts with zone delete permissions, such as hello subscription owner, can still accidentally delete a critical zone.</span></span>

<span data-ttu-id="b2def-204">Det är möjligt toouse båda metoderna - resurslås och anpassade roller - på hello samma tid, som en metod för skydd på djupet tooDNS zon skydd.</span><span class="sxs-lookup"><span data-stu-id="b2def-204">It is possible toouse both approaches - resource locks and custom roles - at hello same time, as a defense-in-depth approach tooDNS zone protection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b2def-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b2def-205">Next steps</span></span>

* <span data-ttu-id="b2def-206">Läs mer om hur du arbetar med RBAC [Kom igång med åtkomsthantering i hello Azure-portalen](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="b2def-206">For more information about working with RBAC, see [Get started with access management in hello Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>
* <span data-ttu-id="b2def-207">Mer information om hur du arbetar med resurslås finns [låsa resurser med Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="b2def-207">For more information about working with resource locks, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

