---
ms.assetid: 
title: "Azure Key Vault - hur du använder mjuk borttagning med PowerShell"
description: "Använda case exempel på mjuk borttagning med PowerShell-kodstyckena"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/21/2017
ms.author: bruceper
ms.openlocfilehash: 8cf0674f7eb139e50da4a3c22a8d8376a86b0dcc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a>Hur du använder Key Vault mjuk borttagning med PowerShell

Funktionen för mjuk borttagning av Azure Key Vault kan återställning av borttagna valv och valvet objekt. Mer specifikt mjuk borttagning adresser i följande scenarier:

- Stöd för återställningsbara borttagningen av ett nyckelvalv
- Stöd för återställningsbara borttagning av objekt i nyckelvalvet. nycklar hemligheter, och certifikat

## <a name="prerequisites"></a>Krav

- Azure PowerShell 4.0.0 eller senare - om du inte redan har gjort det här installationsprogrammet, installera Azure PowerShell och koppla den till din Azure-prenumeration, se [hur du installerar och konfigurerar du Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview). 

>[!NOTE]
> Det finns en inaktuell version av våra Key Vault PowerShell-utdata formatering fil som **kan** läsas in i din miljö i stället för rätt version. Vi förutse en uppdaterad version av PowerShell som innehåller nödvändiga korrigeringen för formatering av utdata och kommer att uppdatera det här avsnittet som för närvarande. Den aktuella lösningen bör du stöta på problemet formatering är:
> - Använd följande fråga om du upptäcker att det inte uppstår mjuk borttagning enabled-egenskap som beskrivs i det här avsnittet: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.


Key Vault specifika refernece information för PowerShell finns i [Azure Key Vault PowerShell-referens](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

## <a name="required-permissions"></a>Behörigheter som krävs

Key Vault hanteras separat via rollbaserade behörigheter för åtkomstkontroll (RBAC) enligt följande:

| Åtgärd | Beskrivning | Användarbehörighet |
|:--|:--|:--|
|Visa lista|Visar bort nyckelvalv.|Microsoft.KeyVault/deletedVaults/read|
|Återställ|Återställer borttagna nyckelvalvet.|Microsoft.KeyVault/vaults/write|
|Rensa|Tar permanent bort borttagna nyckelvalvet och allt dess innehåll.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

Mer information om behörighet och åtkomstkontroll finns [Secure nyckelvalvet](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Aktivera mjuk borttagning

Om du vill kunna återställa borttagna nyckelvalv eller objekt som lagras i ett nyckelvalv måste du först aktivera mjuk borttagning för att nyckelvalvet.

### <a name="existing-key-vault"></a>Befintliga nyckelvalvet

Aktivera mjuk borttagning på följande sätt för en befintlig nyckelvalv med namnet ContosoVault. 

>[!NOTE]
>För närvarande måste du använda Azure Resource Manager resurs manipulering direkt skriva den *enableSoftDelete* egenskap till Key Vault-resurs.

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a>Nytt nyckelvalv

Aktivera mjuk borttagning för ett nytt nyckelvalv görs vid skapandet genom att lägga till soft-ta bort flaggan till din skapa-kommando.

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a>Kontrollera mjuk borttagning aktivering

Om du vill kontrollera att ett nyckelvalv har aktiverat soft-delete, kör den *hämta* kommandot och leta efter den 'ej permanent ta bort Enabled?' attribut och dess inställning, true eller false.

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>Om du tar bort ett nyckelvalv som skyddas av mjuk borttagning

Kommandot för att ta bort (eller ta bort) en nyckelvalv förblir detsamma, men dess beteende ändras beroende på om du har aktiverat soft-ta bort eller inte.

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
>Om du kör det föregående kommandot för ett nyckelvalv som inte har aktiverat soft-delete tar permanent bort den här nyckelvalvet och allt dess innehåll utan alternativ för återställning.

### <a name="how-soft-delete-protects-your-key-vaults"></a>Hur mjuk borttagning skyddar ditt nyckelvalv

Aktiverad med mjuk borttagning:

- När en nyckelvalv tas bort, den tas bort från dess resursgrupp och placeras i ett reserverat namnområde som endast som är kopplade till den plats där den skapades. 
- Objekt i en borttagen för valvet, t.ex. nycklar, hemligheter och certifikat, är tillgänglig och förblir det medan sina som innehåller nyckelvalv är i tillståndet deleted. 
- DNS-namn för nyckelvalvet i ett borttaget tillstånd är kvar så det inte går att skapa ett nytt nyckelvalv med samma namn.  

Du kan visa nyckelvalv i Borttaget tillstånd, som är associerade med din prenumeration, med följande kommando:

```powershell
PS C:\> Get-AzureRmKeyVault -InRemovedStateVault 

Name           : ContosoVault
Location             : westus
Id                   : /subscriptions/xxx/providers/Microsoft.KeyVault/locations/westus/deletedVaults/ContosoVault
Resource ID          : /subscriptions/xxx/resourceGroups/ContosoVault/providers/Microsoft.KeyVault/vaults/ContosoVault
Deletion Date        : 5/9/2017 12:14:14 AM
Scheduled Purge Date : 8/7/2017 12:14:14 AM
Tags                 :
```

Den *resurs-ID* i utdata som refererar till den ursprungliga resurs-ID för det här valvet. Eftersom den här nyckelvalv är nu i ett borttaget tillstånd, det finns någon resurs med resurs-ID. Den *Id* fält kan användas för att identifiera resursen när du återställer eller rensa. Den *schemalagda Rensa datum* anger när valvet tas bort permanent (rensas) om ingen åtgärd utförs för borttagna valvet. Loggperioden som används för att beräkna den *schemalagda Rensa datum*, är 90 dagar.

## <a name="recovering-a-key-vault"></a>Återställa en nyckelvalv

Om du vill återställa ett nyckelvalv som du behöver ange nyckelvalv namn, resursgruppens namn och plats. Notera platsen och resursgruppen för borttagna nyckelvalvet som du behöver dem för en nyckelvalv återställningsprocessen.

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

När en nyckelvalv återställs är resultatet en ny resurs med det nyckelvalv ursprungliga resurs-ID. Om resursgruppen där nyckelvalvet fanns har tagits bort, måste en ny resursgrupp med samma namn skapas innan nyckelvalvet kan återställas.

## <a name="key-vault-objects-and-soft-delete"></a>Key Vault-objekt och Mjuk borttagning

För en nyckel ContosoFirstKey, i en källnyckelvalvets med namnet 'ContosoVault' med mjuk borttagning aktiverad här hur du skulle ta bort nyckeln.

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

Med nyckelvalvet aktiverad för mjuk borttagning, visas borttagna nyckeln fortfarande som om det har tagits bort utom, när du inte uttryckligen lista eller hämta borttagna nycklar. De flesta åtgärder på en nyckel i tillståndet deleted misslyckas förutom visar en lista över borttagna nyckeln, återställs eller rensa den. 

Till exempel om du vill begära att listan bort nycklar i en nyckelvalvet, använder du följande kommando:

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a>Övergångstillstånd 

När du tar bort en nyckel i ett nyckelvalv med aktiverat soft bort kan det ta några sekunder för övergången. Under den här övergångstillstånd kan visas att nyckeln inte är i aktivt läge eller Borttaget tillstånd. Det här kommandot visar en lista över alla borttagna nycklar i nyckelvalvet med namnet 'ContosoVault'.

```powershell
  Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
  Vault Name           : ContosoVault
  Name                 : ContosoFirstKey
  Id                   : https://ContosoVault.vault.azure.net:443/keys/ContosoFirstKey
  Deleted Date         : 2/14/2017 8:20:52 PM
  Scheduled Purge Date : 5/15/2017 8:20:52 PM
  Enabled              : True
  Expires              :
  Not Before           :
  Created              : 2/14/2017 8:16:07 PM
  Updated              : 2/14/2017 8:16:07 PM
  Tags                 :
```

### <a name="using-soft-delete-with-key-vault-objects"></a>Hur mjuk borttagning med nyckelvalv objekt

Precis som nyckelvalv, en borttagen nyckel hemlighet eller certifikat finns kvar i Borttaget tillstånd för upp till 90 dagar såvida du inte återställa den eller rensa den. 

#### <a name="keys"></a>Nycklar

För att återställa en borttagen nyckel:

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

Du ta bort en nyckel:

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
>Rensa en nyckel tar permanent bort, vilket innebär att det inte går att återställa.

Den **återställa** och **Rensa** åtgärder har sina egna behörigheter i en åtkomstprincip för nyckelvalv. För en användare eller tjänstens huvudnamn för att kunna köra en **återställa** eller **Rensa** åtgärd som de måste ha behörigheten respektive för objektet (nyckel eller hemlighet) i åtkomstprincipen nyckelvalvet. Som standard den **Rensa** behörighet läggs inte till ett nyckelvalv åtkomstprincip genvägen till 'all' används för att ge alla behörigheter till en användare. Du måste uttryckligen bevilja **Rensa** behörighet. Till exempel följande kommando ger user@contoso.com behörighet att utföra flera åtgärder med nycklar i *ContosoVault* inklusive **Rensa**.

#### <a name="set-a-key-vault-access-policy"></a>Skapa en princip för nyckelvalvet åtkomst

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> Om du har en befintlig nyckelvalv som precis har haft soft-ta bort aktiverat kan du kanske inte har **återställa** och **Rensa** behörigheter.

#### <a name="secrets"></a>Hemligheter

Som nycklar drivs hemligheter i en nyckelvalvet på med sina egna kommandon. Följande, är kommandon för att ta bort, lista, återställa och rensa hemligheter.

- Ta bort en hemlighet som heter SQLPassword: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- Lista alla borttagna hemligheter i en nyckelvalvet: 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- Återställa en hemlighet i Borttaget tillstånd: 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- Rensa en hemlighet i Borttaget tillstånd: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
>Rensa en hemlighet tar permanent bort, vilket innebär att det inte går att återställa.

## <a name="purging-and-key-vaults"></a>Ovanstående och nyckel valv

### <a name="key-vault-objects"></a>Nyckelvalv objekt

Rensa en nyckel tar hemlighet eller certifikat permanent bort, vilket innebär att det inte går att återställa. Nyckelvalvet med borttagna objekt förblir dock intakta som kommer alla andra objekt i nyckelvalvet. 

### <a name="key-vaults-as-containers"></a>Nyckel-valv som behållare
När en nyckelvalv rensas tas allt innehåll, inklusive nycklar och hemligheter certifikat bort permanent. Om du vill rensa en key vault använder den `Remove-AzureRmKeyVault` kommandot med alternativet `-InRemovedState` och genom att ange platsen för borttagna nyckelvalvet med den `-Location location` argumentet. Du kan hitta platsen för en borttagen valvet med hjälp av kommandot `Get-AzureRmKeyVault -InRemovedState`.

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
>Rensa en key vault tar permanent bort, vilket innebär att det inte går att återställa.

### <a name="purge-permissions-required"></a>Rensa behörigheter som krävs
- Om du vill rensa borttagna nyckelvalvet, så att valvet och allt dess innehåll tas bort permanent, måste användaren RBAC-behörighet för att utföra en *Microsoft.KeyVault/locations/deletedVaults/purge/action* igen. 
- Om du vill visa den borttagna nyckeln valvet en användare behöver RBAC-behörighet för att utföra *Microsoft.KeyVault/deletedVaults/read* behörighet. 
- Som standard har bara en prenumeration administratör dessa behörigheter. 

### <a name="scheduled-purge"></a>Schemalagda Rensa

Visar en lista över borttagna nyckelvalv-objekt visar när de är schedled tas bort av Key Vault. Den *schemalagda Rensa datum* fältet visas när ett nyckelvalv objekt tas bort permanent, om ingen åtgärd utförs. Som standard är loggperioden för ett objekt för borttagna nyckelvalv 90 dagar.

>[!NOTE]
>Ett objekt för borttagna valvet utlöses av dess *schemalagda Rensa datum* fältet, bort permanent. Den kan inte återställas.

## <a name="other-resources"></a>Andra resurser

- En översikt över funktionen för mjuk borttagning av Key Vault finns [översikt över Azure Key Vault-soft-ta bort](key-vault-ovw-soft-delete.md).
- En allmän översikt över användning av Azure Key Vault finns [Kom igång med Azure Key Vault](key-vault-get-started.md).

