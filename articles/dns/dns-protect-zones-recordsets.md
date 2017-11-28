---
title: Skydda DNS-zoner och poster | Microsoft Docs
description: "Hur du skyddar DNS-zoner och postuppsättningar i Microsoft Azure DNS."
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
ms.openlocfilehash: 0b7040d6273b3a6b85cd55850d596807226b87fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-protect-dns-zones-and-records"></a><span data-ttu-id="9fe4f-103">Så här skyddar du DNS-zoner och poster</span><span class="sxs-lookup"><span data-stu-id="9fe4f-103">How to protect DNS zones and records</span></span>

<span data-ttu-id="9fe4f-104">DNS-zoner och poster är viktiga resurser.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-104">DNS zones and records are critical resources.</span></span> <span data-ttu-id="9fe4f-105">Ta bort en DNS-zon eller bara en DNS-post kan resultera i en totala avbrott.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-105">Deleting a DNS zone or even just a single DNS record can result in a total service outage.</span></span>  <span data-ttu-id="9fe4f-106">Det är därför viktigt att kritiska DNS-zoner och poster skyddas mot obehörig eller oavsiktliga ändringar.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-106">It is therefore important that critical DNS zones and records are protected against unauthorized or accidental changes.</span></span>

<span data-ttu-id="9fe4f-107">Den här artikeln förklarar hur Azure DNS kan du skydda DNS-zoner och poster mot sådana ändringar.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-107">This article explains how Azure DNS enables you to protect your DNS zones and records against such changes.</span></span>  <span data-ttu-id="9fe4f-108">Det använda två kraftfulla säkerhetsfunktioner som tillhandahålls av Azure Resource Manager: [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md) och [resurslås](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="9fe4f-108">We apply two powerful security features provided by Azure Resource Manager: [role-based access control](../active-directory/role-based-access-control-what-is.md) and [resource locks](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

## <a name="role-based-access-control"></a><span data-ttu-id="9fe4f-109">Rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="9fe4f-109">Role-based access control</span></span>

<span data-ttu-id="9fe4f-110">Azure rollbaserad åtkomstkontroll (RBAC) Aktivera detaljerad åtkomsthantering för Azure-användare, grupper och resurser.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-110">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure users, groups, and resources.</span></span> <span data-ttu-id="9fe4f-111">Med RBAC kan bevilja du exakt åtkomstnivå som användare måste utföra sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-111">Using RBAC, you can grant precisely the amount of access that users need to perform their jobs.</span></span> <span data-ttu-id="9fe4f-112">Mer information om hur RBAC kan hjälpa dig att hantera åtkomsten finns [vad är rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="9fe4f-112">For more information about how RBAC helps you manage access, see [What is Role-Based Access Control](../active-directory/role-based-access-control-what-is.md).</span></span>

### <a name="the-dns-zone-contributor-role"></a><span data-ttu-id="9fe4f-113">Rollen DNS-zonen-deltagare</span><span class="sxs-lookup"><span data-stu-id="9fe4f-113">The 'DNS Zone Contributor' role</span></span>

<span data-ttu-id="9fe4f-114">DNS-zonen-deltagare roll är en inbyggd roll som tillhandahålls av Azure för att hantera DNS-resurser.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-114">The 'DNS Zone Contributor' role is a built-in role provided by Azure for managing DNS resources.</span></span>  <span data-ttu-id="9fe4f-115">Tilldela DNS-zonen deltagarbehörighet till en användare eller grupp kan den gruppen att hantera DNS-resurser, men inte resurser för andra typer.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-115">Assigning DNS Zone Contributor permissions to a user or group enables that group to manage DNS resources, but not resources of any other type.</span></span>

<span data-ttu-id="9fe4f-116">Anta exempelvis att resurs grupp 'myzones' innehåller fem zoner för Contoso Corporation.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-116">For example, suppose the resource group 'myzones' contains five zones for Contoso Corporation.</span></span> <span data-ttu-id="9fe4f-117">Tilldela DNS-administratören DNS-zonen-deltagare behörigheter till resursgruppen kan fullständig kontroll över dessa DNS-zoner.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-117">Granting the DNS administrator 'DNS Zone Contributor' permissions to that resource group, enables full control over those DNS zones.</span></span> <span data-ttu-id="9fe4f-118">Den förhindrar tillståndsbeviljande onödiga, till exempel DNS-administratören inte kan skapa eller stoppa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-118">It also avoids granting unnecessary permissions, for example the DNS administrator cannot create or stop Virtual Machines.</span></span>

<span data-ttu-id="9fe4f-119">Det enklaste sättet att tilldela behörigheter för RBAC [via Azure portal](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="9fe4f-119">The simplest way to assign RBAC permissions is [via the Azure portal](../active-directory/role-based-access-control-configure.md).</span></span>  <span data-ttu-id="9fe4f-120">Öppna 'Åtkomstkontroll (IAM)'-bladet för resursgruppen på ”Lägg till' och sedan Markera rollen DNS-zonen-deltagare och välj nödvändiga användare eller grupper för att bevilja behörighet.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-120">Open the 'Access control (IAM)' blade for the resource group, then click 'Add', then select the 'DNS Zone Contributor' role and select the required users or groups to grant permissions.</span></span>

![Resursgruppsnivå RBAC via Azure portal](./media/dns-protect-zones-recordsets/rbac1.png)

<span data-ttu-id="9fe4f-122">Behörigheter kan också vara [beviljas med hjälp av Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="9fe4f-122">Permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

<span data-ttu-id="9fe4f-123">Kommandot motsvarande är också [tillgängliga via Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="9fe4f-123">The equivalent command is also [available via the Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a><span data-ttu-id="9fe4f-124">Zonen nivå RBAC</span><span class="sxs-lookup"><span data-stu-id="9fe4f-124">Zone level RBAC</span></span>

<span data-ttu-id="9fe4f-125">Azure RBAC-regler kan användas till en prenumeration, resursgrupp eller till en enskild resurs.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-125">Azure RBAC rules can be applied to a subscription, a resource group or to an individual resource.</span></span> <span data-ttu-id="9fe4f-126">Vid Azure DNS kan resursen vara en enskild DNS-zon eller även en enskild uppsättning av poster.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-126">In the case of Azure DNS, that resource can be an individual DNS zone, or even an individual record set.</span></span>

<span data-ttu-id="9fe4f-127">Anta exempelvis att resurs grupp 'myzones' innehåller zonen contoso.com och en underdomänen 'customers.contoso.com' där CNAME-poster skapas för varje kundkonto.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-127">For example, suppose the resource group 'myzones' contains the zone 'contoso.com' and a subzone 'customers.contoso.com' in which CNAME records are created for each customer account.</span></span>  <span data-ttu-id="9fe4f-128">Det konto som används för att hantera dessa CNAME-poster ska tilldelas behörigheter att skapa poster i zonen 'customers.contoso.com', den inte ska ha åtkomst till andra zoner.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-128">The account used to manage these CNAME records should be assigned permissions to create records in the 'customers.contoso.com' zone only, it should not have access to the other zones.</span></span>

<span data-ttu-id="9fe4f-129">Zonen RBAC-behörighet kan beviljas via Azure portal.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-129">Zone-level RBAC permissions can be granted via the Azure portal.</span></span>  <span data-ttu-id="9fe4f-130">Öppna bladet 'Åtkomstkontroll (IAM)' för zonen, på ”Lägg till” och sedan välja rollen DNS-zonen-deltagare och välj nödvändiga användare eller grupper för att bevilja behörighet.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-130">Open the 'Access control (IAM)' blade for the zone, then click 'Add', then select the 'DNS Zone Contributor' role and select the required users or groups to grant permissions.</span></span>

![DNS-zonen nivå RBAC via Azure portal](./media/dns-protect-zones-recordsets/rbac2.png)

<span data-ttu-id="9fe4f-132">Behörigheter kan också vara [beviljas med hjälp av Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="9fe4f-132">Permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant 'DNS Zone Contributor' permissions to a specific zone
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

<span data-ttu-id="9fe4f-133">Kommandot motsvarande är också [tillgängliga via Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="9fe4f-133">The equivalent command is also [available via the Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant 'DNS Zone Contributor' permissions to a specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a><span data-ttu-id="9fe4f-134">Postuppsättning nivå RBAC</span><span class="sxs-lookup"><span data-stu-id="9fe4f-134">Record set level RBAC</span></span>

<span data-ttu-id="9fe4f-135">Vi kan ta ytterligare ett steg.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-135">We can go one step further.</span></span> <span data-ttu-id="9fe4f-136">Överväg att e-administratören för Contoso Corporation, som behöver åtkomst till MX och TXT-poster på apex i zonen ”contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-136">Consider the mail administrator for Contoso Corporation, who needs access to the MX and TXT records at the apex of the 'contoso.com' zone.</span></span>  <span data-ttu-id="9fe4f-137">Hon behöver inte åtkomst till alla andra MX och TXT-poster, eller alla poster för alla andra typer.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-137">She doesn't need access to any other MX or TXT records, or to any records of any other type.</span></span>  <span data-ttu-id="9fe4f-138">Azure DNS kan du tilldela behörigheter på nivån postuppsättning exakt för poster som e-administratör behöver åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-138">Azure DNS allows you to assign permissions at the record set level, to precisely the records that the mail administrator needs access to.</span></span>  <span data-ttu-id="9fe4f-139">E-administratör beviljas exakt kontrollera hon måste och kan inte göra några andra ändringar.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-139">The mail administrator is granted precisely the control she needs, and is unable to make any other changes.</span></span>

<span data-ttu-id="9fe4f-140">Postuppsättningen behörighet på användarnivå RBAC kan konfigureras via Azure-portalen med hjälp av knappen ”användare” i bladet postuppsättning:</span><span class="sxs-lookup"><span data-stu-id="9fe4f-140">Record-set level RBAC permissions can be configured via the Azure portal, using the 'Users' button in the record set blade:</span></span>

![Postuppsättning nivå RBAC via Azure portal](./media/dns-protect-zones-recordsets/rbac3.png)

<span data-ttu-id="9fe4f-142">Postuppsättningen behörighet på användarnivå RBAC kan också vara [beviljas med hjälp av Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="9fe4f-142">Record-set level RBAC permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant permissions to a specific record set
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

<span data-ttu-id="9fe4f-143">Kommandot motsvarande är också [tillgängliga via Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="9fe4f-143">The equivalent command is also [available via the Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant permissions to a specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a><span data-ttu-id="9fe4f-144">Anpassade roller</span><span class="sxs-lookup"><span data-stu-id="9fe4f-144">Custom roles</span></span>

<span data-ttu-id="9fe4f-145">Inbyggda DNS-zonen-deltagare-rollen ger fullständig kontroll över DNS-resursposter.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-145">The built-in 'DNS Zone Contributor' role enables full control over a DNS resource.</span></span> <span data-ttu-id="9fe4f-146">Du kan också skapa egna kunden Azure roller, som ger även mer detaljerad kontroll.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-146">It is also possible to build your own customer Azure roles, to provide even finer-grained control.</span></span>

<span data-ttu-id="9fe4f-147">Studera igen exemplet som en CNAME-post i zonen 'customers.contoso.com' skapas för varje kundkonto för Contoso Corporation.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-147">Consider again the example in which a CNAME record in the zone 'customers.contoso.com' is created for each Contoso Corporation customer account.</span></span>  <span data-ttu-id="9fe4f-148">Det konto som används för att hantera dessa skapa CNAME-poster ska beviljas behörighet att hantera CNAME-poster.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-148">The account used to manage these CNAMEs should be granted permission to manage CNAME records only.</span></span>  <span data-ttu-id="9fe4f-149">Det är sedan det går inte att ändra poster för andra typer (till exempel ändra MX-poster) eller utföra åtgärder på zonnivå, till exempel ta bort zonen.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-149">It is then unable to modify records of other types (such as changing MX records) or perform zone-level operations such as zone delete.</span></span>

<span data-ttu-id="9fe4f-150">I följande exempel visas en anpassad rolldefinition för att hantera CNAME-poster:</span><span class="sxs-lookup"><span data-stu-id="9fe4f-150">The following example shows a custom role definition for managing CNAME records only:</span></span>

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

<span data-ttu-id="9fe4f-151">Egenskapen åtgärder definierar följande DNS-specifika behörigheter:</span><span class="sxs-lookup"><span data-stu-id="9fe4f-151">The Actions property defines the following DNS-specific permissions:</span></span>

* <span data-ttu-id="9fe4f-152">`Microsoft.Network/dnsZones/CNAME/*`ger fullständig kontroll över CNAME-poster</span><span class="sxs-lookup"><span data-stu-id="9fe4f-152">`Microsoft.Network/dnsZones/CNAME/*` grants full control over CNAME records</span></span>
* <span data-ttu-id="9fe4f-153">`Microsoft.Network/dnsZones/read`ger behörighet att läsa DNS-zoner, men inte ändra dem, så att du ser zonen där CNAME håller på att skapas.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-153">`Microsoft.Network/dnsZones/read` grants permission to read DNS zones, but not to modify them, enabling you to see the zone in which the CNAME is being created.</span></span>

<span data-ttu-id="9fe4f-154">De återstående åtgärderna kopieras från den [DNS-zonen inbyggda deltagarrollen](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).</span><span class="sxs-lookup"><span data-stu-id="9fe4f-154">The remaining Actions are copied from the [DNS Zone Contributor built-in role](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).</span></span>

> [!NOTE]
> <span data-ttu-id="9fe4f-155">Använda en anpassad RBAC-roll för att förhindra att ta bort postuppsättningar fortfarande så att de kan uppdateras inte är en effektiv kontroll.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-155">Using a custom RBAC role to prevent deleting record sets while still allowing them to be updated is not an effective control.</span></span> <span data-ttu-id="9fe4f-156">Det förhindrar postuppsättningar tas bort, men den förhindrar inte dem som ska ändras.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-156">It prevents record sets from being deleted, but it does not prevent them from being modified.</span></span>  <span data-ttu-id="9fe4f-157">Tillåtna ändringar lägga till och ta bort poster från uppsättningen av poster, inklusive ta bort alla poster om du vill lämna en 'empty-postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-157">Permitted modifications include adding and removing records from the record set, including removing all records to leave an 'empty' record set.</span></span> <span data-ttu-id="9fe4f-158">Detta har samma effekt som tar bort posten från en DNS-matchning synvinkel.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-158">This has the same effect as deleting the record set from a DNS resolution viewpoint.</span></span>

<span data-ttu-id="9fe4f-159">Anpassade Rolldefinitioner kan för närvarande inte definieras via Azure portal.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-159">Custom role definitions cannot currently be defined via the Azure portal.</span></span> <span data-ttu-id="9fe4f-160">En anpassad roll som baseras på den här rolldefinition kan skapas med hjälp av Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="9fe4f-160">A custom role based on this role definition can be created using Azure PowerShell:</span></span>

```powershell
# Create new role definition based on input file
New-AzureRmRoleDefinition -InputFile <file path>
```

<span data-ttu-id="9fe4f-161">Det kan även skapas via Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="9fe4f-161">It can also be created via the Azure CLI:</span></span>

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

<span data-ttu-id="9fe4f-162">Rollen kan sedan tilldelas på samma sätt som inbyggda roller, enligt beskrivningen tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-162">The role can then be assigned in the same way as built-in roles, as described earlier in this article.</span></span>

<span data-ttu-id="9fe4f-163">Mer information om hur du skapa, hantera och tilldela anpassade roller finns [anpassade roller i Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="9fe4f-163">For more information on how to create, manage, and assign custom roles, see [Custom Roles in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span></span>

## <a name="resource-locks"></a><span data-ttu-id="9fe4f-164">Resurslås</span><span class="sxs-lookup"><span data-stu-id="9fe4f-164">Resource locks</span></span>

<span data-ttu-id="9fe4f-165">Förutom RBAC stöder en annan typ av säkerhetskontroll, nämligen möjligheten att ”låsa” resurser i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-165">In addition to RBAC, Azure Resource Manager supports another type of security control, namely the ability to 'lock' resources.</span></span> <span data-ttu-id="9fe4f-166">Där RBAC regler kan du styra åtgärder för specifika användare och grupper, resurslås som tillämpas på resursen och börjar gälla för alla användare och roller.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-166">Where RBAC rules allow you to control the actions of specific users and groups, resource locks are applied to the resource, and are effective across all users and roles.</span></span> <span data-ttu-id="9fe4f-167">Mer information finns i [Låsa resurser med Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="9fe4f-167">For more information, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

<span data-ttu-id="9fe4f-168">Det finns två typer av resurslås: **DoNotDelete** och **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-168">There are two types of resource lock: **DoNotDelete** and **ReadOnly**.</span></span> <span data-ttu-id="9fe4f-169">Dessa kan användas till en DNS-zon eller till en enskild uppsättning av poster.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-169">These can be applied either to a DNS zone, or to an individual record set.</span></span>  <span data-ttu-id="9fe4f-170">I följande avsnitt beskrivs några vanliga scenarier och hur du stöder dem med hjälp av resource Lås.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-170">The following sections describe several common scenarios, and how to support them using resource locks.</span></span>

### <a name="protecting-against-all-changes"></a><span data-ttu-id="9fe4f-171">Skydd mot alla ändringar</span><span class="sxs-lookup"><span data-stu-id="9fe4f-171">Protecting against all changes</span></span>

<span data-ttu-id="9fe4f-172">För att förhindra att alla ändringar som gäller en ReadOnly-Lås för zonen.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-172">To prevent any changes being made, apply a ReadOnly lock to the zone.</span></span>  <span data-ttu-id="9fe4f-173">Detta förhindrar att nya postuppsättningar skapas och befintliga postuppsättningar från att ändras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-173">This prevents new record sets from being created, and existing record sets from being modified or deleted.</span></span>

<span data-ttu-id="9fe4f-174">Zonen nivån resurslås kan skapas via Azure portal.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-174">Zone level resource locks can be created via the Azure portal.</span></span>  <span data-ttu-id="9fe4f-175">I bladet DNS-zonen klickar du på Lås, Lägg sedan 'till':</span><span class="sxs-lookup"><span data-stu-id="9fe4f-175">From the DNS zone blade, click 'Locks', then 'Add':</span></span>

![Zonen nivån resurslås via Azure portal](./media/dns-protect-zones-recordsets/locks1.png)

<span data-ttu-id="9fe4f-177">På zonnivå resurs Lås kan även skapas via Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="9fe4f-177">Zone-level resource locks can also be created via Azure PowerShell:</span></span>

```powershell
# Lock a DNS zone
New-AzureRmResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

<span data-ttu-id="9fe4f-178">Konfigurera Azure-resurslås stöds inte för närvarande via Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-178">Configuring Azure resource locks is not currently supported via the Azure CLI.</span></span>

### <a name="protecting-individual-records"></a><span data-ttu-id="9fe4f-179">Skydda enskilda poster</span><span class="sxs-lookup"><span data-stu-id="9fe4f-179">Protecting individual records</span></span>

<span data-ttu-id="9fe4f-180">För att förhindra att en befintlig DNS-post mot ändring gäller en ReadOnly-Lås för uppsättningen av poster.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-180">To prevent an existing DNS record set against modification, apply a ReadOnly lock to the record set.</span></span>

> [!NOTE]
> <span data-ttu-id="9fe4f-181">Tillämpa DoNotDelete lås på en postuppsättning är inte en effektiv kontroll.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-181">Applying a DoNotDelete lock to a record set is not an effective control.</span></span> <span data-ttu-id="9fe4f-182">Det förhindrar att posten tas bort, men den förhindrar inte att den kan ändras.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-182">It prevents the record set from being deleted, but it does not prevent it from being modified.</span></span>  <span data-ttu-id="9fe4f-183">Tillåtna ändringar lägga till och ta bort poster från uppsättningen av poster, inklusive ta bort alla poster om du vill lämna en 'empty-postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-183">Permitted modifications include adding and removing records from the record set, including removing all records to leave an 'empty' record set.</span></span> <span data-ttu-id="9fe4f-184">Detta har samma effekt som tar bort posten från en DNS-matchning synvinkel.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-184">This has the same effect as deleting the record set from a DNS resolution viewpoint.</span></span>

<span data-ttu-id="9fe4f-185">Postuppsättning nivån resurslås för närvarande kan bara konfigureras med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-185">Record set level resource locks can currently only be configured using Azure PowerShell.</span></span>  <span data-ttu-id="9fe4f-186">De stöds inte i Azure-portalen eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-186">They are not supported in the Azure portal or Azure CLI.</span></span>

```powershell
# Lock a DNS record set
New-AzureRmResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a><span data-ttu-id="9fe4f-187">Skydd mot zon borttagning</span><span class="sxs-lookup"><span data-stu-id="9fe4f-187">Protecting against zone deletion</span></span>

<span data-ttu-id="9fe4f-188">När en zon tas bort i Azure DNS, raderas även alla uppsättningar av poster i zonen.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-188">When a zone is deleted in Azure DNS, all record sets in the zone are also deleted.</span></span>  <span data-ttu-id="9fe4f-189">Den här åtgärden kan inte ångras.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-189">This operation cannot be undone.</span></span>  <span data-ttu-id="9fe4f-190">Oavsiktlig borttagning av en kritisk zon har möjlighet att ha en betydande marknadsfördelar.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-190">Accidentally deleting a critical zone has the potential to have a significant business impact.</span></span>  <span data-ttu-id="9fe4f-191">Därför är det mycket viktigt att skydda mot oavsiktlig zonen borttagning.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-191">It is therefore very important to protect against accidental zone deletion.</span></span>

<span data-ttu-id="9fe4f-192">Tillämpa DoNotDelete lås på en zon förhindrar att zonen tas bort.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-192">Applying a DoNotDelete lock to a zone prevents the zone from being deleted.</span></span>  <span data-ttu-id="9fe4f-193">Men eftersom Lås ärvs av underordnade resurser den förhindrar också att alla uppsättningar av poster i zonen tas bort, vilket kan vara önskvärt.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-193">However, since locks are inherited by child resources, it also prevents any record sets in the zone from being deleted, which may be undesirable.</span></span>  <span data-ttu-id="9fe4f-194">Dessutom enligt beskrivningen i noteringen ovan, är det också ineffektiv eftersom poster fortfarande kan tas bort från de befintliga postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-194">Furthermore, as described in the note above, it is also ineffective since records can still be removed from the existing record sets.</span></span>

<span data-ttu-id="9fe4f-195">Överväg att använda ett DoNotDelete Lås till en post i zonen, till exempel uppsättningen SOA-poster som ett alternativ.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-195">As an alternative, consider applying a DoNotDelete lock to a record set in the zone, such as the SOA record set.</span></span>  <span data-ttu-id="9fe4f-196">Eftersom zonen inte kan tas bort utan att även ta bort postuppsättningar, skyddar detta mot zon borttagning, men ändå låta postuppsättningar i zonen ska ändras fritt.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-196">Since the zone cannot be deleted without also deleting the record sets, this protects against zone deletion, while still allowing record sets within the zone to be modified freely.</span></span> <span data-ttu-id="9fe4f-197">Om ett försök görs att ta bort zonen identifierar Azure Resource Manager detta skulle även att ta bort uppsättningen av SOA-poster och blockerar anropet eftersom SOA är låst.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-197">If an attempt is made to delete the zone, Azure Resource Manager detects this would also delete the SOA record set, and blocks the call because the SOA is locked.</span></span>  <span data-ttu-id="9fe4f-198">Inga postuppsättningar tas bort.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-198">No record sets are deleted.</span></span>

<span data-ttu-id="9fe4f-199">Följande PowerShell-kommando skapar ett DoNotDelete Lås mot SOA-post på den angivna zonen:</span><span class="sxs-lookup"><span data-stu-id="9fe4f-199">The following PowerShell command creates a DoNotDelete lock against the SOA record of the given zone:</span></span>

```powershell
# Protect against zone delete with DoNotDelete lock on the record set
New-AzureRmResourceLock -LockLevel DoNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

<span data-ttu-id="9fe4f-200">Ett annat sätt att förhindra oavsiktlig zonen borttagning är med hjälp av en anpassad roll så operatorn och tjänstkonton som används för att hantera zoner har inte behörighet zonen.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-200">Another way to prevent accidental zone deletion is by using a custom role to ensure the operator and service accounts used to manage your zones do not have zone delete permissions.</span></span> <span data-ttu-id="9fe4f-201">Du kan tillämpa en tvåstegsverifiering delete, första beviljande zonen behörighet att ta bort (definitionsområdet zonen, så att du tar bort fel zon) och andra för att ta bort zonen när du behöver ta bort en zon.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-201">When you do need to delete a zone, you can enforce a two-step delete, first granting zone delete permissions (at the zone scope, to prevent deleting the wrong zone) and second to delete the zone.</span></span>

<span data-ttu-id="9fe4f-202">Den här andra tillvägagångssättet har fördelen att det fungerar för alla zoner som har åtkomst till dessa konton utan att behöva komma ihåg att skapa någon Lås.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-202">This second approach has the advantage that it works for all zones accessed by those accounts, without having to remember to create any locks.</span></span> <span data-ttu-id="9fe4f-203">Den har nackdelen att alla konton med behörighet att zonen ta bort, till exempel prenumerationsägaren, fortfarande av misstag kan ta bort en zon för kritiska.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-203">It has the disadvantage that any accounts with zone delete permissions, such as the subscription owner, can still accidentally delete a critical zone.</span></span>

<span data-ttu-id="9fe4f-204">Det är möjligt att använda båda metoderna - resurslås och anpassade roller - samtidigt som en metod för skydd på djupet för DNS-zonen skydd.</span><span class="sxs-lookup"><span data-stu-id="9fe4f-204">It is possible to use both approaches - resource locks and custom roles - at the same time, as a defense-in-depth approach to DNS zone protection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9fe4f-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9fe4f-205">Next steps</span></span>

* <span data-ttu-id="9fe4f-206">Läs mer om hur du arbetar med RBAC [Kom igång med åtkomsthantering i Azure portal](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="9fe4f-206">For more information about working with RBAC, see [Get started with access management in the Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>
* <span data-ttu-id="9fe4f-207">Mer information om hur du arbetar med resurslås finns [låsa resurser med Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="9fe4f-207">For more information about working with resource locks, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

