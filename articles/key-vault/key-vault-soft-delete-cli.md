---
ms.assetid: 
title: "aaaAzure nyckel valvet – hur toouse mjuka bort med CLI"
description: "Använda case exempel på mjuk borttagning med CLI-kodstyckena"
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.author: bruceper
ms.openlocfilehash: 672f5210ab119c244ca712f0bb80b653b50ea79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-key-vault-soft-delete-with-cli"></a>Hur toouse Key Vault mjuk borttagning med CLI

Funktionen för mjuk borttagning av Azure Key Vault kan återställning av borttagna valv och valvet objekt. Mer specifikt mjuk borttagning adresser hello följande scenarier:

- Stöd för återställningsbara borttagningen av ett nyckelvalv
- Stöd för återställningsbara borttagning av objekt i nyckelvalvet. nycklar hemligheter, och certifikat

## <a name="prerequisites"></a>Krav

- Azure CLI 2.0 - om du inte har den här installationen för din miljö finns [hantera Nyckelvalv med hjälp av CLI 2.0](key-vault-manage-with-cli2.md).

Key Vault specifika referensinformation för CLI, se [Azure CLI 2.0 Key Vault referens](https://docs.microsoft.com/cli/azure/keyvault).

## <a name="required-permissions"></a>Behörigheter som krävs

Key Vault hanteras separat via rollbaserade behörigheter för åtkomstkontroll (RBAC) enligt följande:

| Åtgärd | Beskrivning | Användarbehörighet |
|:--|:--|:--|
|Visa lista|Visar bort nyckelvalv.|Microsoft.KeyVault/deletedVaults/read|
|Återställ|Återställer borttagna nyckelvalvet.|Microsoft.KeyVault/vaults/write|
|Rensa|Tar permanent bort borttagna nyckelvalvet och allt dess innehåll.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

Mer information om behörighet och åtkomstkontroll finns [Secure nyckelvalvet](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Aktivera mjuk borttagning

toobe kan toorecover borttagna nyckelvalv eller objekt som lagras i en nyckel valvet, måste du först aktivera mjuk borttagning för att nyckelvalvet.

### <a name="existing-key-vault"></a>Befintliga nyckelvalvet

Aktivera mjuk borttagning på följande sätt för en befintlig nyckelvalv med namnet ContosoVault. 

>[!NOTE]
>För närvarande måste toouse Azure Resource Manager resurs manipulering toodirectly skrivåtgärder hello *enableSoftDelete* egenskapen toohello Key Vault-resurs.

```azurecli
az resource update --id $(az keyvault show --name ContosoVault -o tsv | awk '{print $1}') --set properties.enableSoftDelete=true
```

### <a name="new-key-vault"></a>Nytt nyckelvalv

Aktivera mjuk borttagning för ett nytt nyckelvalv görs vid skapandet genom att lägga till hello soft-ta bort flaggan tooyour skapa kommando.

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --enable-soft-delete true --location westus
```

### <a name="verify-soft-delete-enablement"></a>Kontrollera mjuk borttagning aktivering

tooverify som ett nyckelvalv har aktiverat mjuk borttagning, kör hello *visa* kommandot och leta efter hello ”ej permanent ta bort aktiverat' attribut och dess inställning, true eller false.

```azurecli
az keyvault show --name ContosoVault
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>Om du tar bort ett nyckelvalv som skyddas av mjuk borttagning

hello kommandot toodelete (eller ta bort) en nyckelvalv förblir hello detsamma, men dess funktionsändringar beroende på om du har aktiverat soft-ta bort eller inte.

```azurecli
az keyvault delete --name ContosoVault
```

> [!IMPORTANT]
>Om du kör hello föregående kommando för ett nyckelvalv som inte har aktiverat soft-delete tar permanent bort den här nyckelvalvet och allt dess innehåll utan alternativ för återställning.

### <a name="how-soft-delete-protects-your-key-vaults"></a>Hur mjuk borttagning skyddar ditt nyckelvalv

Aktiverad med mjuk borttagning:

- När en nyckelvalv tas bort, den tas bort från dess resursgrupp och placeras i ett reserverat namnområde som endast är associerad med hello plats där den skapades. 
- Objekt i en borttagen för valvet, t.ex. nycklar, hemligheter och certifikat, är tillgänglig och förblir det medan sina som innehåller nyckelvalv hello bort tillstånd. 
- hello DNS-namn för nyckelvalvet i ett borttaget tillstånd är kvar så det inte går att skapa ett nytt nyckelvalv med samma namn.  

Du kan visa Borttaget tillstånd nyckelvalv, som är associerade med din prenumeration med hjälp av hello följande kommando:

```azurecli
az keyvault list-deleted
```

Hej *resurs-ID* i hello utdata refererar toohello ursprungliga resurs-ID för det här valvet. Eftersom den här nyckelvalv är nu i ett borttaget tillstånd, det finns någon resurs med resurs-ID. Hej *Id* fältet kan vara används tooidentify hello resurs när du återställer eller rensa. Hej *schemalagda Rensa datum* anger när hello valvet tas bort permanent (rensas) om ingen åtgärd utförs för borttagna valvet. hello standard kvarhållning period, använt toocalculate hello *schemalagda Rensa datum*, är 90 dagar.

## <a name="recovering-a-key-vault"></a>Återställa en nyckelvalv

toorecover ett nyckelvalv måste toospecify hello nyckelvalv namn, resursgruppens namn och plats. Obs hello plats och hello resursgruppen av hello bort nyckelvalv som du behöver dem för en nyckelvalv återställningsprocessen.

```azurecli
az keyvault recover --location westus --name ContosoVault
```

När en nyckelvalv återställs är hello resultatet en ny resurs med hello nyckelvalv ursprungliga resurs-ID. Om hello resursgruppen där hello nyckelvalv fanns har tagits bort, måste en ny resursgrupp med samma namn skapas innan hello nyckelvalv kan återställas.

## <a name="key-vault-objects-and-soft-delete"></a>Key Vault-objekt och Mjuk borttagning

För en nyckel ContosoFirstKey, i en källnyckelvalvets med namnet 'ContosoVault' med mjuk borttagning aktiverad här hur du skulle ta bort nyckeln.

```azurecli
az keyvault key delete --name ContosoFirstKey --vault-name ContosoVault
```

Med nyckelvalvet aktiverad för mjuk borttagning, visas borttagna nyckeln fortfarande som om det har tagits bort utom, när du inte uttryckligen lista eller hämta borttagna nycklar. De flesta åtgärder på en nyckel i hello bort tillstånd misslyckas förutom visar en lista över borttagna nyckeln, återställs eller rensa den. 

Till exempel använda toorequest toolist bort nycklar i nyckelvalvet, hello följande kommando:

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="transition-state"></a>Övergångstillstånd 

Det kan ta några sekunder för hello övergången toocomplete när du tar bort en nyckel i ett nyckelvalv med aktiverad mjuk borttagning. Under den här övergångstillstånd kan visas hello nyckeln inte är i hello active eller hello bort tillstånd. Det här kommandot visar en lista över alla borttagna nycklar i nyckelvalvet med namnet 'ContosoVault'.

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="using-soft-delete-with-key-vault-objects"></a>Hur mjuk borttagning med nyckelvalv objekt

Precis som nyckelvalv, en borttagen nyckel hemlighet eller certifikat finns kvar i Borttaget tillstånd för in too90 dagar såvida du inte återställa den eller rensa den. 

#### <a name="keys"></a>Nycklar

toorecover borttagna nyckeln:

```azurecli
az keyvault key recover --name ContosoFirstKey --vault-name ContosoVault
```

toopermanently ta bort en nyckel:

```azurecli
az keyvault key purge --name ContosoFirstKey --vault-name ContosoVault
```

>[!NOTE]
>Rensa en nyckel tar permanent bort, vilket innebär att det inte går att återställa.

Hej **återställa** och **Rensa** åtgärder har sina egna behörigheter i en åtkomstprincip för nyckelvalv. För en användare eller tjänst huvudnamn toobe kan tooexecute en **återställa** eller **Rensa** åtgärd som de måste ha hello respektive behörighet för objektet (nyckel eller hemlighet) i hello nyckelvalv åtkomstprincip. Som standard hello **Rensa** behörighet läggs inte tooa nyckelvalv åtkomstprincip hello 'all' genväg kan du använda toogrant alla behörigheter tooa användare. Du måste uttryckligen bevilja **Rensa** behörighet. Till exempel hello efter kommandot ger user@contoso.com behörighet tooperform flera åtgärder med nycklar i *ContosoVault* inklusive **Rensa**.

#### <a name="set-a-key-vault-access-policy"></a>Skapa en princip för nyckelvalvet åtkomst

```azurecli
az keyvault set-policy --name ContosoVault --key-permissions get create delete list update import backup restore recover purge
```

>[!NOTE] 
> Om du har en befintlig nyckelvalv som precis har haft soft-ta bort aktiverat kan du kanske inte har **återställa** och **Rensa** behörigheter.

#### <a name="secrets"></a>Hemligheter

Som nycklar drivs hemligheter i en nyckelvalvet på med sina egna kommandon. Följande, är hello-kommandon för att ta bort, lista, återställa och rensa hemligheter.

- Ta bort en hemlighet som heter SQLPassword: 
```azurecli
az keyvault secret delete --vault-name ContosoVault -name SQLPassword
```

- Lista alla borttagna hemligheter i en nyckelvalvet: 
```azurecli
az keyvault secret list-deleted --vault-name ContosoVault
```

- Återställa en hemlighet i hello bort tillstånd: 
```azurecli
az keyvault secret recover --name SQLPassword --vault-name ContosoVault
```

- Rensa en hemlighet i Borttaget tillstånd: 
```azurecli
az keyvault secret purge --name SQLPAssword --vault-name ContosoVault
```

>[!NOTE]
>Rensa en hemlighet tar permanent bort, vilket innebär att det inte går att återställa.

## <a name="purging-and-key-vaults"></a>Ovanstående och nyckel valv

### <a name="key-vault-objects"></a>Nyckelvalv objekt

Rensa en nyckel tar hemlighet eller certifikat permanent bort, vilket innebär att det inte går att återställa. Hej nyckelvalv med hello borttagna objekt förblir men intakta som kommer alla andra objekt i hello nyckelvalvet. 

### <a name="key-vaults-as-containers"></a>Nyckel-valv som behållare
När en nyckelvalv rensas tas allt innehåll, inklusive nycklar och hemligheter certifikat bort permanent. toopurge nyckelvalvet, använda hello `az keyvault purge` kommando. Du kan hitta hello plats din prenumeration borttagna nyckelvalv hello kommandot `az keyvault list-deleted`.

```azurecli
az keyvault purge --location westus --name ContosoVault
```

>[!NOTE]
>Rensa en key vault tar permanent bort, vilket innebär att det inte går att återställa.

### <a name="purge-permissions-required"></a>Rensa behörigheter som krävs
- toopurge borttagna nyckelvalvet, så att hello valvet och allt dess innehåll tas bort permanent, hello användare behöver RBAC behörighet tooperform en *Microsoft.KeyVault/locations/deletedVaults/purge/action* igen. 
- toolist hello bort nyckel hello valvet en användare behöver RBAC behörighet tooperform *Microsoft.KeyVault/deletedVaults/read* behörighet. 
- Som standard har bara en prenumeration administratör dessa behörigheter. 

### <a name="scheduled-purge"></a>Schemalagda Rensa

Visar en lista över borttagna nyckelvalv-objekt visar när de är schedled toobe rensas av Key Vault. Hej *schemalagda Rensa datum* fältet visas när ett nyckelvalv objekt tas bort permanent, om ingen åtgärd utförs. Som standard är hello kvarhållningsperiod för ett objekt för borttagna nyckelvalv 90 dagar.

>[!NOTE]
>Ett objekt för borttagna valvet utlöses av dess *schemalagda Rensa datum* fältet, bort permanent. Den kan inte återställas.

## <a name="other-resources"></a>Andra resurser

- En översikt över funktionen för mjuk borttagning av Key Vault finns [översikt över Azure Key Vault-soft-ta bort](key-vault-ovw-soft-delete.md).
- En allmän översikt över användning av Azure Key Vault finns [Kom igång med Azure Key Vault](key-vault-get-started.md).

