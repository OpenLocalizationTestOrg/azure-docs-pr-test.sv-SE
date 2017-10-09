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
# <a name="how-tooprotect-dns-zones-and-records"></a>Hur tooprotect DNS zoner och registrerar

DNS-zoner och poster är viktiga resurser. Ta bort en DNS-zon eller bara en DNS-post kan resultera i en totala avbrott.  Det är därför viktigt att kritiska DNS-zoner och poster skyddas mot obehörig eller oavsiktliga ändringar.

Den här artikeln förklarar hur Azure DNS gör du tooprotect din DNS-zoner och poster mot sådana ändringar.  Det använda två kraftfulla säkerhetsfunktioner som tillhandahålls av Azure Resource Manager: [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md) och [resurslås](../azure-resource-manager/resource-group-lock-resources.md).

## <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll

Azure rollbaserad åtkomstkontroll (RBAC) Aktivera detaljerad åtkomsthantering för Azure-användare, grupper och resurser. Med RBAC kan bevilja du exakt hello mängden åtkomst att användarna måste tooperform sitt arbete. Mer information om hur RBAC kan hjälpa dig att hantera åtkomsten finns [vad är rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md).

### <a name="hello-dns-zone-contributor-role"></a>hello-DNS-zonen ' deltagarrollen

hello är DNS-zonen-deltagare en inbyggd roll som tillhandahålls av Azure för att hantera DNS-resurser.  Tilldela DNS-zonen deltagare behörigheter tooa användare eller grupp kan den grupp toomanage DNS-resurser, men inte resurser för andra typer.

Anta exempelvis att myzones' hello resurs grupp' innehåller fem zoner för Contoso Corporation. Beviljande hello DNS-administratören DNS-zonen-deltagare behörigheter toothat resursgrupp, kan fullständig kontroll över dessa DNS-zoner. Den förhindrar tillståndsbeviljande onödiga, till exempel hello DNS-administratören inte kan skapa eller stoppa virtuella datorer.

hello enklaste sättet tooassign RBAC behörigheter [via hello Azure-portalen](../active-directory/role-based-access-control-configure.md).  Öppna hello-åtkomstkontroll (IAM)-bladet för hello resursgrupp, och sedan klicka på ”Lägg till' och välj sedan hello DNS-zonen-deltagare och välj hello krävs användare eller grupper toogrant behörigheter.

![Resursgruppsnivå RBAC via hello Azure-portalen](./media/dns-protect-zones-recordsets/rbac1.png)

Behörigheter kan också vara [beviljas med hjälp av Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant 'DNS Zone Contributor' permissions tooall zones in a resource group
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

hello likvärdigt kommando är också [tillgängliga via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions tooall zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a>Zonen nivå RBAC

Azure RBAC-reglerna kan vara tillämpade tooa prenumeration, en resurs grupp eller tooan enskilda resurs. Hello gäller Azure DNS, kan resursen vara en enskild DNS-zon eller även en enskild uppsättning av poster.

Anta exempelvis att myzones' hello resurs grupp' innehåller hello zonen ”contoso.com” och en underdomänen 'customers.contoso.com' där CNAME-poster skapas för varje kundkonto.  hello konto som används för toomanage dessa CNAME-poster ska tilldelas behörigheter toocreate poster i hello 'customers.contoso.com' zon, den får inte ha åtkomst toohello andra zoner.

Zonen RBAC-behörighet kan beviljas via hello Azure-portalen.  Öppna hello-åtkomstkontroll (IAM)-bladet för hello zon, och sedan klicka på ”Lägg till' och välj sedan hello DNS-zonen-deltagare och välj hello krävs användare eller grupper toogrant behörigheter.

![DNS-zonen nivå RBAC via hello Azure-portalen](./media/dns-protect-zones-recordsets/rbac2.png)

Behörigheter kan också vara [beviljas med hjälp av Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant 'DNS Zone Contributor' permissions tooa specific zone
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

hello likvärdigt kommando är också [tillgängliga via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions tooa specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a>Postuppsättning nivå RBAC

Vi kan ta ytterligare ett steg. Överväg att hello e-administratören för Contoso Corporation, som behöver åtkomst toohello MX och TXT-poster på hello apex av hello ”contoso.com” zon.  Hon inte behöver komma åt tooany andra MX och TXT-poster eller poster tooany av någon annan typ.  Azure DNS kan du tooassign behörigheter på hello postuppsättning genom tooprecisely hello poster som hello e-administratör behöver åtkomst till.  hello e-administratör beviljas exakt hello Kontrollera hon behöver och är toomake andra ändringar.

Postuppsättningen behörighet på användarnivå RBAC kan konfigureras via hello Azure-portalen med hjälp av knappen hello användare i hello postuppsättning bladet:

![Postuppsättning nivå RBAC via hello Azure-portalen](./media/dns-protect-zones-recordsets/rbac3.png)

Postuppsättningen behörighet på användarnivå RBAC kan också vara [beviljas med hjälp av Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant permissions tooa specific record set
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

hello likvärdigt kommando är också [tillgängliga via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant permissions tooa specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>Anpassade roller

hello inbyggda DNS-zonen-deltagare rollen ger fullständig kontroll över DNS-resursposter. Det är också möjligt toobuild egna kund Azure roller, tooprovide ännu mer detaljerad kontroll.

Fundera igen hello exempel som en CNAME-post i zonen hello 'customers.contoso.com' skapas för varje kundkonto för Contoso Corporation.  hello konto som används för toomanage dessa CNAME-resursposter beviljas behörighet toomanage CNAME poster.  Det är toomodify poster av andra typer (till exempel ändra MX-poster) eller utföra åtgärder på zonnivå, till exempel ta bort zonen.

hello som följande exempel visar en anpassad rolldefinition för att hantera CNAME-poster:

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

hello åtgärder egenskapen definierar hello följande DNS-specifika behörigheter:

* `Microsoft.Network/dnsZones/CNAME/*`ger fullständig kontroll över CNAME-poster
* `Microsoft.Network/dnsZones/read`beviljar behörighet tooread DNS-zoner, men inte toomodify dem, aktiverar du toosee hello zonen i vilka hello CNAME håller på att skapas.

hello återstående åtgärder kopieras från hello [DNS-zonen inbyggda deltagarrollen](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).

> [!NOTE]
> Med hjälp av en anpassad RBAC rollen tooprevent bort post anger fortfarande så att de toobe uppdateras inte är en effektiv kontroll. Det förhindrar postuppsättningar tas bort, men den förhindrar inte dem som ska ändras.  Tillåtna ändringar omfattar att lägga till och ta bort poster från hello postuppsättningen, inklusive ta bort alla poster tooleave en 'empty-postuppsättning. Detta har samma effekt som tar bort hello post ange från en DNS-matchning synvinkel hello.

Anpassade Rolldefinitioner kan för närvarande inte definieras via hello Azure-portalen. En anpassad roll som baseras på den här rolldefinition kan skapas med hjälp av Azure PowerShell:

```powershell
# Create new role definition based on input file
New-AzureRmRoleDefinition -InputFile <file path>
```

Det kan även skapas via hello Azure CLI:

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

hello roll kan sedan tilldelas i hello samma sätt som inbyggda roller, enligt beskrivningen tidigare i den här artikeln.

Mer information om hur toocreate, hantera och tilldela anpassade roller finns [anpassade roller i Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).

## <a name="resource-locks"></a>Resurslås

I tillägg tooRBAC Azure Resource Manager har stöd för en annan typ av säkerhetskontroll, nämligen hello möjlighet too'lock' resurser. Om RBAC reglerna tillåter toocontrol hello åtgärder för specifika användare och grupper, resurslås är tillämpade toohello resurs och börjar gälla för alla användare och roller. Mer information finns i [Låsa resurser med Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).

Det finns två typer av resurslås: **DoNotDelete** och **ReadOnly**. Dessa kan tillämpas tooa DNS-zon eller tooan enskilda postuppsättning.  hello följande avsnitt beskrivs några vanliga scenarier och hur toosupport dem med hjälp av resource Lås.

### <a name="protecting-against-all-changes"></a>Skydd mot alla ändringar

tooprevent ändringar som gjorts, tillämpa en ReadOnly Lås toohello zon.  Detta förhindrar att nya postuppsättningar skapas och befintliga postuppsättningar från att ändras eller tas bort.

Zonen nivån resurslås kan skapas via hello Azure-portalen.  Klicka på Lås, hello DNS-zonen bladet Lägg sedan 'till':

![Zonen nivån resurslås via hello Azure-portalen](./media/dns-protect-zones-recordsets/locks1.png)

På zonnivå resurs Lås kan även skapas via Azure PowerShell:

```powershell
# Lock a DNS zone
New-AzureRmResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

Konfigurera Azure-resurslås stöds inte för närvarande via hello Azure CLI.

### <a name="protecting-individual-records"></a>Skydda enskilda poster

tooprevent en befintlig DNS-post mot ändringar, gäller en ReadOnly Lås toohello postuppsättning.

> [!NOTE]
> Tillämpa en DoNotDelete Lås tooa är postuppsättning inte en effektiv kontroll. Det förhindrar hello-postuppsättning tas bort, men den förhindrar inte att den kan ändras.  Tillåtna ändringar omfattar att lägga till och ta bort poster från hello postuppsättningen, inklusive ta bort alla poster tooleave en 'empty-postuppsättning. Detta har samma effekt som tar bort hello post ange från en DNS-matchning synvinkel hello.

Postuppsättning nivån resurslås för närvarande kan bara konfigureras med Azure PowerShell.  De stöds inte i hello Azure-portalen eller Azure CLI.

```powershell
# Lock a DNS record set
New-AzureRmResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a>Skydd mot zon borttagning

När en zon tas bort i Azure DNS, alla uppsättningar av poster i zonen hello också att tas bort.  Den här åtgärden kan inte ångras.  Oavsiktlig borttagning av en kritisk zon har hello potentiella toohave betydande marknadsfördelar.  Därför är det mycket viktigt tooprotect mot oavsiktlig zonen borttagning.

Tillämpa en DoNotDelete Lås tooa zon förhindrar hello zon tas bort.  Men eftersom Lås ärvs av underordnade resurser den förhindrar också att alla uppsättningar av poster i hello zon tas bort, vilket kan vara önskvärt.  Dessutom enligt beskrivningen i hello Obs ovan, är det också ineffektiv eftersom poster fortfarande kan tas bort från hello befintliga postuppsättningar.

Överväg att använda en DoNotDelete Lås tooa post som i hello zonen, till exempel hello SOA-postuppsättning som ett alternativ.  Eftersom hello zon inte kan tas bort utan att även ta bort hello postuppsättningar, skyddar detta mot zon borttagning, men ändå låta postuppsättningar inom hello zonen toobe ändrade fritt. Om ett försök görs toodelete hello zonen, identifierar Azure Resource Manager detta skulle även bort hello SOA-postuppsättning och block hello anropet eftersom hello SOA är låst.  Inga postuppsättningar tas bort.

hello följande PowerShell-kommando skapar ett DoNotDelete Lås mot hello SOA-post på hello angivna zonen:

```powershell
# Protect against zone delete with DoNotDelete lock on hello record set
New-AzureRmResourceLock -LockLevel DoNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

Ett annat sätt tooprevent zon för oavsiktlig borttagning är med hjälp av en anpassad roll tooensure hello operatorn och toomanage för konton som används av tjänsten inte har zonen zonerna ta bort behörigheter. När du behöver toodelete en zon, kan du använda en tvåstegsverifiering ta bort, första beviljande zonen behörighet att ta bort (definitionsområdet hello zonen, tooprevent bort hello fel zon) och andra toodelete hello zon.

Den här andra metoden har hello fördelen att det fungerar för alla zoner som har åtkomst till dessa konton utan att behöva tooremember toocreate alla Lås. Den har hello Nackdelen är att alla konton med behörighet att zonen ta bort, till exempel hello prenumerationsägaren fortfarande av misstag kan ta bort en zon för kritiska.

Det är möjligt toouse båda metoderna - resurslås och anpassade roller - på hello samma tid, som en metod för skydd på djupet tooDNS zon skydd.

## <a name="next-steps"></a>Nästa steg

* Läs mer om hur du arbetar med RBAC [Kom igång med åtkomsthantering i hello Azure-portalen](../active-directory/role-based-access-control-what-is.md).
* Mer information om hur du arbetar med resurslås finns [låsa resurser med Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).

