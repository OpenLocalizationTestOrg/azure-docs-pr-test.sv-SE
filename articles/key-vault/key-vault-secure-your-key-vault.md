---
title: aaaSecure din nyckel valvet | Microsoft Docs
description: "Hantera åtkomstbehörigheter för nyckelvalv för att hantera valv och nycklar och hemligheter. Autentisering och auktorisering modell för nyckelvalvet och hur toosecure din nyckel valvet"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
ms.openlocfilehash: 84f5fc18142a1ad89babbd11f4f65eca105afc32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-your-key-vault"></a>Säkra ditt nyckelvalv
Azure Key Vault är en molntjänst som skyddar krypteringsnycklar och hemligheter (som certifikat, anslutningssträngar, lösenord) för dina molnprogram. Eftersom dessa data är skiftlägeskänsliga och verksamhetskritiska, du vill toosecure åtkomst tooyour nyckelvalv så att endast behöriga program och användare kan komma åt nyckelvalvet. Den här artikeln innehåller en översikt över nyckelvalv modell, förklarar autentisering och auktorisering och beskriver hur toosecure åtkomst tookey valvet för dina molnprogram med ett exempel.

## <a name="overview"></a>Översikt
Åtkomst tooa nyckelvalv styrs via två separata gränssnitt: management plan och dataplan. För båda planen krävs korrekt autentisering och auktorisering innan en anropare (en användare eller ett program) får åtkomst tookey valvet. Autentisering upprättar hello identitet hello anroparen medan auktorisering avgör vilka åtgärder hello anroparen tillåts tooperform.

Både hanteringsplanet och dataplanet använder sig av Azure Active Directory för autentisering. För auktorisering använder sig hanteringsplanet av rollbaserad åtkomstkontroll (RBAC) medan dataplanet använder sig av nyckelvalvets åtkomstprincip.

Här är en kort översikt över hello ämnen som:

[Autentisering med hjälp av Azure Active Directory](#authentication-using-azure-active-directory) – det här avsnittet beskrivs hur en anropare autentiserar med Azure Active Directory tooaccess nyckelvalv via management plan och dataplan. 

[Hanteringsplanet och dataplanet](#management-plane-and-data-plane) – Hanteringsplanet och dataplanet är två åtkomstplan som används för åtkomst till ditt nyckelvalv. Varje åtkomstplan har stöd för specifika åtgärder. Det här avsnittet beskrivs hello åtkomst slutpunkter, åtgärder som stöds och åtkomst till kontrollmetod som används av varje plan. 

[Åtkomstkontroll för hantering av plan](#management-plane-access-control) – i det här avsnittet ska vi titta på att tillåta åtkomst toomanagement plan åtgärder med hjälp av rollbaserad åtkomstkontroll.

[Åtkomstkontroll för data plan](#data-plane-access-control) – det här avsnittet beskrivs hur toocontrol för toouse nyckelvalv åtkomst principdata plan åtkomst.

[Exempel](#example) -detta exempel beskrivs hur toosetup åtkomstkontroll för ditt nyckelvalv tooallow tre olika teamen (säkerhetsteam, utvecklare/operatorer och granskare) tooperform specifika uppgifter toodevelop, hantera och övervaka ett program i Azure .

## <a name="authentication-using-azure-active-directory"></a>Autentisering med Azure Active Directory
När du skapar en nyckelvalvet i en Azure-prenumeration associeras den automatiskt med hello prenumeration Azure Active Directory-klient. Anropare (användare och program) måste vara registrerade i den här innehavaren tooaccess denna nyckelvalvet. Ett program eller en användare måste autentisera med Azure Active Directory tooaccess nyckelvalvet. Detta gäller tooboth management plan och dataplan åtkomst. I bägge fallen, kan ett program få åtkomst till nyckelvalvet på två sätt:

* **användare+appåtkomst** – vanligtvis är det här för program som har åtkomst till nyckelvalvet åt en inloggad användare. Azure PowerShell, Azure Portal är exempel på den här typen av åtkomst. Det finns två sätt toogrant åtkomst toousers: enkelriktade toogrant åtkomst toousers så att de kan komma åt nyckelvalvet från alla program och hello annat sätt är toogrant ett användaren åtkomst tookey valv endast när de använder ett visst program (refererad tooas sammansatt identitet). 
* **endast App-åtkomst** – för program att köra daemon services bakgrundsjobb etc. hello Programidentitet beviljas åtkomst till toohello nyckeln valvet.

I båda typer av program hello programmet autentiserar med Azure Active Directory med hjälp av hello [stöds autentiseringsmetoder](../active-directory/active-directory-authentication-scenarios.md) och skaffar en token. Autentiseringsmetod som används beror på programmet hello. Sedan hello program använder den här token och skickar REST API-begäran tookey valvet. Vid hantering av plan åtkomst hello dirigeras begäranden via Azure Resource Manager-slutpunkten. Vid åtkomst till dataplan pratar hello program direkt tooa nyckelvalv slutpunkt. Visa mer information om hello [hela autentiseringsflödet](../active-directory/active-directory-protocols-oauth-code.md). 

hello resursnamnet som hello begär en token är olika beroende på om hello programmet försöker använda management plan eller dataplan. Därför är hello resursnamnet antingen plan eller data plan hanteringsslutpunkten beskrivs i hello tabell i ett senare avsnitt, beroende på hello Azure-miljön.

Med en mekanism för autentisering tooboth har hanterings- och plan sin egen fördelar:

* Organisationer kan centralt styra åtkomst tooall nyckelvalv i organisationen
* Om en användare lämnar förlora de omedelbart åtkomsten tooall nyckelvalv i hello organisation
* Organisationer kan anpassa autentisering via hello alternativ i Azure Active Directory (till exempel aktiverar multifaktorautentisering för extra säkerhet)

## <a name="management-plane-and-data-plane"></a>Hanteringsplanet och dataplanet
Azure Key Vault är en Azure-tjänst som finns tillgänglig via distributionsmodellen Azure Resouce Manager. När du skapar ett nyckelvalv, får du en virtuell behållare i vilken du kan skapa andra objekt som nycklar, hemligheter och certifikat. Sedan får du åtkomst till nyckelvalvet med plan och data plan tooperform specifika hanteringsåtgärder. Plan för hanteringsgränssnitt är används toomanage din nyckel valvet sig själv, till exempel skapa, ta bort, uppdaterar nyckelvalv attribut och ange åtkomstprinciper för dataplan. Data plan gränssnittet har använt tooadd, ta bort, ändra och använda hello nycklar och hemligheter certifikat som lagras i nyckelvalvet.

hello plan och data plan hanteringsgränssnitt kan nås via olika slutpunkter (se tabell). hello andra kolumnen i tabellen hello beskriver hello DNS-namn för de här slutpunkterna i olika miljöer i Azure. hello tredje kolumnen beskriver hello åtgärder du kan utföra från varje åtkomst plan. Varje åtkomstplan har sin egen åtkomstkontroll: för hanteringsplanet är åtkomstkontrollen inställd med Azure Resource Manager rollbaserad åtkomstkontroll (RBAC), medan dataplanets åtkomstkontroll är inställd via nyckelvalvets åtkomstprinciper.

| Åtkomstplan | Slutpunkter för åtkomst | Åtgärder | Åtkomstkontrollmekanismer |
| --- | --- | --- | --- |
| Hanteringsplanet |**Globalt:**<br> management.azure.com:443<br><br> **Azure i Kina:**<br> management.chinacloudapi.cn:443<br><br> **Azure för amerikanska myndigheter:**<br> management.usgovcloudapi.net:443<br><br> **Azure i Tyskland:**<br> management.microsoftazure.de:443 |Skapa/läsa/Uppdatera/Ta bort nyckelvalv <br> Ställ in åtkomstprinciper för nyckelvalvet<br>Ställ in taggar för nyckelvalvet |Azure Resource Manager rollbaserad åtkomstkontroll (RBAC) |
| Dataplanet |**Globalt:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Azure i Kina:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure för amerikanska myndigheter:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure i Tyskland:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |För nycklar: Dekryptera, kryptera, UnwrapKey, WrapKey, verifiera, signera, hämta, lista, uppdatera, skapa, importera, ta bort, säkerhetskopiera, återställa<br><br> För hemligheter: hämta, lista, ställ in, ta bort |Åtkomstprincip för nyckelvalvet |

hello management plan och data plan åtkomstkontroller fungerar oberoende av varandra. Om du vill toogrant en toouse snabbtangenter i en nyckelvalvet, behöver du bara toogrant plan åtkomstbehörighet för data med hjälp av principer för åtkomst av nyckelvalvet och ingen plan åtkomst krävs för det här programmet. Och omvänt, om du vill att en användare toobe kan tooread valvet egenskaper och taggar, men har inte någon åtkomst tookeys, hemligheter eller certifikat, kan du bevilja den här användaren, ”läsa” åtkomst med hjälp av RBAC och ingen åtkomst toodata plan måste anges.

## <a name="management-plane-access-control"></a>Åtkomstkontroll för hanteringsplanet
hello management plan består av åtgärder som påverkar hello nyckelvalv sig själv. Du kan till exempel skapa eller ta bort ett nyckelvalv. Du kan hämta en lista över valv i en prenumeration. Du kan hämta nyckelvalv egenskaper (till exempel SKU taggar) och ange nyckelvalv åtkomstprinciper som styr hello användare och program som kan komma åt nycklar och hemligheter i nyckelvalvet hello. Åtkomstkontroll för hanteringsplanet använder sig av RBAC. Visa hello fullständig lista över nyckelvalv åtgärder som kan utföras via management plan i hello tabellen i föregående avsnitt. 

### <a name="role-based-access-control-rbac"></a>Rollbaserad åtkomstkontroll (RBAC)
Varje Azure-prenumerationen har en Azure Active Directory. Användare, grupper och program från den här katalogen kan beviljas åtkomst toomanage resurser i hello Azure-prenumeration som använder hello Azure Resource Manager-distributionsmodellen. Den här typen av åtkomstkontroll är refererad tooas rollbaserad åtkomstkontroll (RBAC). toomanage detta åtkomst, kan du använda hello [Azure-portalen](https://portal.azure.com/), hello [Azure CLI-verktygen](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs), eller hello [Azure Resource Manager REST API: er](https://msdn.microsoft.com/library/azure/dn906885.aspx).

Med hello Azure Resource Manager-modellen skapa nyckelvalvet i en resurs grupp och kontroll åtkomst toohello management plan för den här nyckelvalv med hjälp av Azure Active Directory. Du kan till exempel ge användare eller en grupp kan toomanage nyckelvalv i en viss resursgrupp.

Du kan bevilja åtkomst toousers, grupper och program på ett specifikt omfång genom att tilldela relevanta RBAC-roller. Till exempel toogrant åt tooa användare toomanage nyckelvalv du ska tilldela en fördefinierad roll 'key vault deltagare' toothis användare på ett specifikt omfång. hello scope är i det här fallet en prenumeration, resursgrupp eller bara en viss nyckelvalvet. En roll som tilldelats på prenumerationsnivå gäller tooall resursgrupper och resurser i den prenumerationen. En roll som tilldelats på resursgruppsnivå gäller tooall resurser i resursgruppen. En roll som tilldelats för en viss resurs gäller bara toothat resurs. Det finns flera fördefinierade roller (se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md)), och om hello fördefinierade roller inte passar dina behov kan du definiera egna roller.

> [!IMPORTANT]
> Observera att om en användare har deltagare behörigheter (RBAC) tooa nyckelvalv management plan, hon kan bevilja sig själv åtkomst toodata plan genom att ange nyckelvalv åtkomstprincip som styr åtkomsten toodata plan. Därför rekommenderas tootightly kontrollera vem som har ”bidragsgivare” åtkomst tooyour nyckelvalv tooensure endast behöriga personer kan komma åt och hantera nyckelvalv, nycklar, hemligheter och certifikat.
> 
> 

## <a name="data-plane-access-control"></a>Åtkomstkontroll för dataplanet
Hej nyckelvalv dataplan består av åtgärder som påverkar hello objekt i nyckelvalvet, till exempel nycklar och hemligheter certifikat.  Det omfattar nyckelåtgärder som att skapa, importera, uppdatera, lista, säkerhetskopiera och återställa nycklar, kryptografiska åtgärder som att signera, verifiera, kryptera, avkryptera, wrappa och unwrappa samt ställa in taggar och andra attribut för nycklar. För hemligheter inkluderar det att hämta, ställa in, lista och ta bort.

Åtkomst till dataplanet ges genom att ställa in åtkomstprinciper för ett nyckelvalv. En användare, grupp eller ett program måste ha bidragsgivarbehörighet (RBAC) för hantering av plan för ett nyckelvalv toobe kan tooset åtkomstprinciper för att nyckelvalvet. En användare, grupp eller ett program kan beviljas åtkomst tooperform specifika åtgärder för nycklar och hemligheter i en nyckelvalvet. Stöd för nyckelvalvet in too16 åtkomst Principposter för ett nyckelvalv. Skapa en säkerhetsgrupp i Azure Active Directory och Lägg till användare toothat grupp toogrant data plan åtkomst tooseveral användare tooa nyckelvalvet.

### <a name="key-vault-access-policies"></a>Åtkomstprinciper för nyckelvalvet
Nyckelvalv åtkomstprinciper bevilja behörigheter tookeys, hemligheter och certifikat separat. Exempelvis kan du ge en användare åtkomst tooonly nycklar, men ingen behörighet till hemligheter. Behörigheter tooaccess nycklar och hemligheter eller certifikat är på nivå för hello-valvet. Med andra ord stöder inte åtkomstprinciper för nyckelvalvet behörigheter på objektsnivå. Du kan använda [Azure-portalen](https://portal.azure.com/), hello [Azure CLI-verktygen](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs), eller hello [nyckelvalv Management REST API: er](https://msdn.microsoft.com/library/azure/mt620024.aspx) tooset åtkomstprinciper för nyckelvalv.

> [!IMPORTANT]
> Observera att nyckelvalvet åtkomstprinciper tillämpas på hello valvet nivå. När en användare beviljas behörighet toocreate och ta bort nycklar utför dessa åtgärder på alla nycklar i det nyckelvalvet.
> 
> 

## <a name="example"></a>Exempel
Anta att du utvecklar ett program som använder ett certifikat för SSL, Azure Storage för att lagra data och använder sig av en RSA 2048-bitars nyckel för signeringsåtgärder. Anta att det här programmet körs i en VM (eller en VM-skaluppsättning). Du kan använda ett nyckelvalv toostore alla hello programmet hemligheter och använda nyckelvalv toostore hello bootstrap certifikat som används av hello programmet tooauthenticate med Azure Active Directory.

Så är här en sammanfattning av alla hello nycklar och hemligheter toobe att lagras i en nyckelvalvet.

* **SSL-certifikat** – används för SSL
* **Nyckeln för säkerhetslagring** -används tooget tooStorage konto
* **RSA 2048-bitars nyckel** – används för signeringsåtgärder
* **Bootstrap certifikat** -användas tooauthenticate tooAzure Active Directory, tooget åtkomst tookey toofetch hello lagring valvnyckeln och Använd hello RSA-nyckel för signering.

Nu ska vi uppfyller hello personer som hantera, distribuera och granskning av det här programmet. Vi använder oss av tre roller i det här exemplet.

* **Säkerhetsteam** – detta är vanligtvis IT-personal från hello 'office av hello rederiets (chefen säkerhetsansvarig), eller motsvarande, ansvarar för hello rätt förvaras av hemligheter som SSL-certifikat, RSA-nycklar som används för att signera anslutningssträngar för databaser lagringskontonycklar.
* **Utvecklare/operatörer** -dessa är hello-avdelningen som utvecklar programmet och distribuerar den i Azure. Vanligtvis är inte en del av hello säkerhetsteam och därför de inte borde ha åtkomst tooany känsliga data, till exempel SSL-certifikat, RSA-nycklar, men hello-program som de distribuerar ska ha åtkomst toothose.
* **Granskare** -detta är vanligtvis en annan uppsättning personer isoleras från hello utvecklare och allmänna IT-personal. Sitt ansvar att säkerställa att kraven data security standards är tooreview användning och underhåll av certifikat, nycklar, osv. 

Det finns en mer roll som är utanför hello omfånget för det här programmet men relevanta här toobe anges, och som skulle vara hello prenumeration (eller resursgrupp) administratör. Prenumerationen administratören konfigurerar inledande åtkomstbehörigheter för hello säkerhetsteam. Vi förutsätter här hello prenumeration administratören har beviljat åtkomst toohello security team tooa resursgruppen där hello alla resurser som krävs för det här programmet bor.

Nu ska vi se vad varje roll utför hello kontexten för det här programmet.

* **Säkerhetsteamet**
  * Skapa nyckelvalv
  * Slå på nyckelvalvs-loggning
  * Lägger till nycklar/hemligheter
  * Skapar säkerhetskopior av nycklar för haveriberedskap
  * Ange nyckelvalv toogrant behörigheter toousers och program tooperform specifika åtgärder
  * Byter periodvis nycklar/hemligheter
* **Utvecklare/operatörer**
  * Hämta referenser toobootstrap och SSL-certifikat (tumavtryck) lagringsnyckel (hemliga URI) och logga nyckel (nyckel URI) från säkerhetsteam
  * Utvecklar och distribuerar programmet som har programmässig åtkomst till nycklar och hemligheter
* **Granskare**
  * Granska användning loggar tooconfirm nyckel/hemlighet användning och kompatibilitet med data security standards

Nu ska vi se vilka åtkomst behörigheter tookey valvet behövs av varje roll (och hello program) tooperform tilldelade aktiviteter. 

| Användarroll | Behörigheter på hanteringsplanet | Behörigheter på dataplanet |
| --- | --- | --- |
| Säkerhetsteamet |nyckelvalvsbidragare |Nycklar: säkerhetskopiering, skapa, ta bort, hämta, importera, lista, återställa <br> Hemligheter: alla |
| Utvecklare/operatör |nyckelvalv distribuera behörighet så att hello VMs de distribuerar kan hämta hemligheter från hello nyckelvalv |Ingen |
| Granskare |Ingen |Nycklar: lista<br>Hemligheter: lista |
| Program |Ingen |Nycklar: signera<br>Hemligheter: hämta |

> [!NOTE]
> Granskare måste listan för nycklar och hemligheter så att de kan inspektera attribut för nycklar och hemligheter som inte har orsakat i hello loggar, till exempel taggar, aktivering och förfallodatum.
> 
> 

Förutom behörighet tookey valvet, alla tre roller också behöver komma åt tooother resurser. Till exempel toobe kan toodeploy virtuella datorer (eller Web Apps osv.) Utvecklare/operatorer måste också 'Deltagare' åtkomst toothose resurstyper. Granskare måste läsbehörighet toohello storage-konto där hello nyckelvalv loggar lagras.

Eftersom hello fokus i den här artikeln är att säkra åtkomsten tooyour nyckelvalvet, vi bara illustrera hello relevanta delar hör toothis avsnittet och hoppa över information om distribution av certifikat, använda nycklar och hemligheter programmässigt etc. De uppgifterna täcks av andra avsnitt. Distribution av certifikat som lagras i nyckelvalvet tooVMs ingår i en [blogginlägget](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/), och det finns [exempelkoden](https://www.microsoft.com/download/details.aspx?id=45343) tillgängliga som illustrerar hur toouse bootstrap certifikat tooauthenticate tooAzure AD tooget åtkomst tookey valvet.

De flesta av hello behörighet kan beviljas med hjälp av Azure portal, men toogrant detaljerade behörigheter som du kan behöva toouse Azure PowerShell (eller Azure CLI) tooachieve hello önskat resultat. 

Anta att hello följande kodavsnitt PowerShell:

* hello Azure Active Directory-administratören har skapat säkerhetsgrupper som representerar hello tre roller, nämligen Contoso säkerhetsteam, Contoso App Devops Contoso App Auditors. Hej administratör har även lagt till användare toohello grupper som de tillhör.
* **ContosoAppRG** är hello resursgrupp där alla hello resurserna finns. **contosologstorage** är där hello loggar lagras. 
* Nyckelvalv **ContosoKeyVault** och storage-kontot som används för nyckelvalvet loggar **contosologstorage** hello måste ha samma Azure-plats

Första hello prenumeration administratören tilldelar 'nyckeln valvet deltagare' och ' administratör för användaråtkomst ' roller toohello säkerhetsteam. Detta gör hello team toomanage åtkomst tooother säkerhetsresurser och hantera nyckelvalv i hello resursgruppen ContosoAppRG.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

hello följande skript visar hur hello säkerhetsteam kan skapa en nyckelvalvet, konfigurera loggning och ange åtkomstbehörigheter för andra roller och hello program. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $devopsrole

# Assign this newly defined role tooDev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

hello anpassad roll definierats är endast tilldelas toohello prenumeration där hello ContosoAppRG resursgruppen har skapats. Om hello samma anpassade roller ska användas för andra projekt i andra prenumerationer, kan dess scope ha flera prenumerationer som lagts till.

hello anpassade rolltilldelning hello utvecklare/operatorer för hello ”distribuera/åtgärd” behörighet är begränsade toohello resursgrupp. Det här sättet endast hello virtuella datorer skapas i hello resursgrupp 'ContosoAppRG' får hello hemligheter (SSL-certifikat och bootstrap cert). Virtuella datorer som en medlem dev/ops skapar i andra resursgruppen kommer inte att kunna tooget dessa hemligheter även om de känner hello hemlighet URI: er.

Det här exemplet visar ett enkelt scenario. Verkligheten scenarier kan vara mer komplicerat och du kan behöva tooadjust behörigheter tooyour nyckelvalv baserat på dina behov. I vårt exempel antar vi till exempel att säkerhetsteam ger hello nyckel och hemliga referenser (URI: er och tumavtryck) som utvecklare/operatörer team måste tooreference i sina program. Därför behöver toogrant utvecklare/operatörer någon plan dataåtkomst. Tänk också på att det här exemplet fokuserar på att skydda ditt nyckelvalv. Liknande bör övervägas toosecure [din virtuella dator](https://azure.microsoft.com/services/virtual-machines/security/), [lagringskonton](../storage/common/storage-security-guide.md) och andra Azure-resurser för.

> [!NOTE]
> Obs: Det här exemplet visar hur åtkomst till nyckelvalvet låses i produktionen. hello utvecklare ska ha sin egen prenumeration eller resursgrupp där de har fullständig behörighet toomanage sina valv, virtuella datorer och storage-konto där de utvecklar programmet hello.
> 
> 

## <a name="resources"></a>Resurser
* [Azure Active Directory rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md)
  
  Den här artikeln förklarar hello Azure Active Directory-rollbaserad åtkomstkontroll och hur det fungerar.
* [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md)
  
  Den här artikeln information om alla hello inbyggda roller som är tillgängliga i RBAC.
* [Förstå Resource Manager-distribution och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md)
  
  Den här artikeln beskriver hello Resource Manager-distribution och klassisk distributionsmodeller och förklarar hello fördelarna med att använda hello Resource Manager och resursgrupper
* [Hantera rollbaserad åtkomstkontroll med Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  
  Den här artikeln förklarar hur toomanage rollbaserad åtkomstkontroll med Azure PowerShell
* [Hantera rollbaserad åtkomstkontroll med hello REST API](../active-directory/role-based-access-control-manage-access-rest.md)
  
  Den här artikeln visar hur toouse hello REST API toomanage RBAC.
* [Rollbaserad åtkomstkontroll i Microsoft Azure från Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  Det här är en video på Channel 9 från hello 2015 MS Ignite konferens tooa för länken. I den här sessionen de talar om åtkomst till hanterings- och rapporteringsfunktioner i Azure och utforska bästa metoderna för att skydda åtkomsten tooAzure prenumerationer med hjälp av Azure Active Directory.
* [Auktorisera åtkomst tooweb program med hjälp av OAuth 2.0 och Azure Active Directory](../active-directory/active-directory-protocols-oauth-code.md)
  
  Den här artikeln beskriver det fullständiga OAuth 2.0-flödet för att autentisera med Azure Active Directory.
* [REST API:er för nyckelvalvshantering](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Det här dokumentet är hello referens för hello REST API: er toomanage din nyckel valvet programmässigt, inklusive principen för nyckelvalvet åtkomst.
* [REST API:er för nyckelvalvet](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  Länken tookey valvet REST API-referensdokumentation.
* [Nyckelåtkomstkontroll](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  Länka tooSecret access control-referensdokumentation.
* [Åtkomstkontroll till hemlighet](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  Länka tooKey access control-referensdokumentation.
* [Ange](https://msdn.microsoft.com/library/mt603625.aspx) och [ta bort](https://msdn.microsoft.com/library/mt619427.aspx) åtkomstprincip för nyckelvalvet med PowerShell
  
  Länkar tooreference dokumentation för PowerShell-cmdlets toomanage nyckelvalv åtkomstprincip.

## <a name="next-steps"></a>Nästa steg
För en komma igång-självstudie för administratörer, kan du se [Kom igång med Azure Key Vault](key-vault-get-started.md).

Mer information om av användningsloggning för Key Vault finns i avsnittet om [Azure Key Vault-loggning](key-vault-logging.md).

Mer information om hur du använder nycklar och hemligheter med Azure Key Vault finns i [Om nycklar och hemligheter](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Om du har frågor om nyckelvalv gå hello [Azure key vault forum](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)

