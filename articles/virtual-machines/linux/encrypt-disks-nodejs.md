---
title: "aaaEncrypt diskar på en Linux-VM med hello Azure CLI 1.0 | Microsoft Docs"
description: "Hur tooencrypt diskar på en Linux VM som använder hello Azure CLI 1.0 och hello Resource Manager-distributionsmodellen"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/06/2017
ms.author: iainfou
ms.openlocfilehash: 68a0394d366c3c6941e2c6db0d4263123f951946
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-disks-on-a-linux-vm-using-hello-azure-cli-10"></a>Kryptera diskar på en Linux VM som använder hello Azure CLI 1.0
För förbättrad virtuell dator (VM) säkerhet och efterlevnad, kan virtuella diskar i Azure krypteras i vila. Diskar krypteras med kryptografiska nycklar som skyddas i ett Azure Key Vault. Du styr dessa kryptografiska nycklar och granska deras användning. Den här artikeln beskrivs hur tooencrypt virtuella diskar på en Linux VM som använder hello Azure CLI 1.0 och hello Resource Manager-modellen.

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering

## <a name="quick-commands"></a>Snabbkommandon
Om du behöver tooquickly utföra hello uppgiften, hello följande avsnitt information hello grundläggande kommandon tooencrypt virtuella diskar på den virtuella datorn. Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](#overview-of-disk-encryption).

Du behöver hello [senaste Azure CLI 1.0](../../xplat-cli-install.md) installerad och logga in med hello Resource Manager-läge på följande sätt:

```azurecli
azure config mode arm
```

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar `myResourceGroup`, `myKeyVault`, och `myVM`.

Först aktivera hello Azure Key Vault-providern i din Azure-prenumeration och skapa en resursgrupp. hello följande exempel skapas ett Resursgruppsnamn `myResourceGroup` i hello `WestUS` plats:

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Skapa ett Azure Key Vault. hello följande exempel skapas ett Nyckelvalv med namnet `myKeyVault`:

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Skapa en kryptografisk nyckel i ditt Nyckelvalv och aktivera kryptering. hello följande exempel skapas en nyckel som heter `myKey`:

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Skapa en slutpunkt som använder Azure Active Directory för att hantera hello autentisering och byta ut kryptografiska nycklar från Nyckelvalvet. Hej `--home-page` och `--identifier-uris` behöver inte toobe faktiska dirigerbara adresser. Hello högsta säkerhetsnivå, ska klienthemlighet användas i stället för lösenord. hello Azure CLI generera inte för närvarande klienthemlighet. Klienthemlighet kan bara skapas i hello Azure-portalen. hello följande exempel skapas en Azure Active Directory-slutpunkt med namnet `myAADApp` och använder ett lösenord på `myPassword`. Ange ditt eget lösenord på följande sätt:

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Obs hello `applicationId` visas i hello utdata från hello föregående kommando. Program-ID används i hello följande steg:

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Lägg till en befintlig virtuell dator i data disk tooan. hello följande exempel lägger till en data disk tooa virtuella datorn med namnet `myVM`:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Granska hello detaljer för din Key Vault och hello nyckel som du skapade. Du behöver hello Key Vault-ID, URI och nyckel URL: en i hello sista steget. hello följande exempel granskar hello information för ett Nyckelvalv med namnet `myKeyVault` och nyckel som heter `myKey`:

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Kryptera din diskar på följande sätt att ange egna parameternamn i hela:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

hello Azure CLI anger inte utförlig fel under hello krypteringsprocessen. Ytterligare felsökningsinformation `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Föregående kommando har många variabler som hello och kan ge mycket uppgift som toowhy hello misslyckas, blir fullständiga exemplet som följer:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Slutligen granska hello krypteringsstatus igen tooconfirm virtuella diskarna har nu krypterats. hello följande exempel kontrollerar hello status för en virtuell dator med namnet `myVM` i hello `myResourceGroup` resursgrupp:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Översikt över diskkryptering
Virtuella diskar på virtuella Linux-datorer krypteras på resten med hjälp av [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Det är gratis för kryptering av virtuella diskar i Azure. Kryptografiska nycklar lagras i Azure Key Vault med skydd av programvara eller du kan importera eller generera dina nycklar i Maskinvarusäkerhetsmoduler (HSM) certifierade tooFIPS 140-2 level 2-standarder. Du behålla kontrollen över dessa kryptografiska nycklar och granska deras användning. Dessa kryptografiska nycklar används tooencrypt och dekryptera virtuella diskar anslutna tooyour VM. En slutpunkt för Azure Active Directory ger en säker mekanism för att utfärda de kryptografiska nycklarna som virtuella datorer är igång på och stänga av.

hello-processen för att kryptera en virtuell dator är som följer:

1. Skapa en kryptografisk nyckel i en Azure Key Vault.
2. Konfigurera hello kryptografiska nyckel toobe kan användas för att kryptera diskar.
3. tooread hello krypteringsnyckeln från hello Azure Key Vault skapa en slutpunkt som använder Azure Active Directory med hello lämpliga behörigheter.
4. Utfärda hello kommandot tooencrypt din virtuella diskar, att ange hello Azure Active Directory-slutpunkten och lämpliga kryptografiska nyckel toobe används.
5. hello Azure Active Directory endpoint begär hello nödvändig kryptografiska nyckel från Azure Key Vault.
6. hello virtuella diskar krypteras med hello tillhandahålls kryptografisk nyckel.

## <a name="supporting-services-and-encryption-process"></a>Stöd för tjänster och kryptering
Kryptering är beroende av hello följande ytterligare komponenter:

* **Azure Key Vault** -används toosafeguard kryptografiska nycklar och hemligheter som används för hello disk kryptering/dekryptering process.
  * Om det finns ett kan du använda en befintlig Azure Key Vault. Du har inte toodedicate en Key Vault tooencrypting diskar.
  * Du kan skapa ett dedikerat Nyckelvalv tooseparate administrativa gränser och viktiga synlighet.
* **Azure Active Directory** - referenser hello säker utbyte av kryptografiska nycklar som krävs och autentisering för begärt åtgärder.
  * Vanligtvis kan du använda en befintlig instans av Azure Active Directory för ditt program.
  * hello program är en slutpunkt för hello Nyckelvalvet och virtuella services toorequest och hämta utfärdade hello lämpliga kryptografiska nycklar. Du utvecklar inte ett verkligt program som integreras med Azure Active Directory.

## <a name="requirements-and-limitations"></a>Krav och begränsningar
Krav för diskkryptering och scenarier som stöds:

* hello följande Linux server SKU - Ubuntu, CentOS, SUSE och SUSE Linux Enterprise Server (SLES) och Red Hat Enterprise Linux.
* Alla resurser (till exempel Nyckelvalv lagringskonto och VM) måste vara i hello samma Azure-region och prenumeration.
* Standard A, D, DS, G och GS-serien virtuella datorer.

Diskkryptering stöds inte för närvarande i hello följande scenarier:

* Grundnivån virtuella datorer.
* Virtuella datorer skapas med hello klassiska distributionsmodellen.
* Om du inaktiverar kryptering för OS-disk på virtuella Linux-datorer.
* Uppdaterar hello Kryptografiska nycklar på en redan krypterade Linux-VM.

## <a name="create-hello-azure-key-vault-and-keys"></a>Skapa hello Azure Key Vault och nycklar
toocomplete hello resten av den här guiden, måste hello [senaste Azure CLI 1.0](../../xplat-cli-install.md) installerad och logga in med hello Resource Manager-läge på följande sätt:

```azurecli
azure config mode arm
```

Ersätt alla exempel parametrar med egna namn, plats och nyckelvärden i hela hello kommandoexempel. hello följande exempel används en konvention för `myResourceGroup`, `myKeyVault`, `myAADApp`osv.

hello första steget är toocreate ett Azure Key Vault toostore kryptografiska nycklar. Azure Key Vault kan lagra nycklar, hemligheter, eller lösenord som gör att du toosecurely implementeras i dina program och tjänster. För virtuell diskkryptering, använda Key Vault toostore en krypteringsnyckel som används tooencrypt eller dekryptera din virtuella diskar.

Aktivera hello Azure Key Vault-providern i din Azure-prenumeration och sedan skapa en resursgrupp. hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `WestUS` plats:

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

hello Azure Key Vault som innehåller hello krypteringsnycklar och associerade beräkning resurser, till exempel lagring och hello Virtuella datorn måste finnas i hello samma region. hello följande exempel skapas en Azure Key Vault som heter `myKeyVault`:

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Du kan lagra kryptografiska nycklar med hjälp av programvara eller maskinvara säkerhet modellen (HSM)-skydd. Använda en HSM kräver en premium Key Vault. Det finns ett extra kostnad toocreating bidrag Key Vault snarare än standard Key Vault som lagrar programvara-skyddade nycklar. toocreate lägga till en premium Key Vault i föregående steg hello `--sku Premium` toohello kommando. hello följande exempel används programvaruskyddad nycklar eftersom vi har skapat ett Nyckelvalv som standard.

För båda modellerna skydd måste hello Azure-plattformen toobe beviljas åtkomst toorequest hello Kryptografiska nycklar när hello VM startas toodecrypt hello virtuella diskar. Skapa en krypteringsnyckel i ditt Nyckelvalv och sedan aktivera den för användning med virtuell diskkryptering. hello följande exempel skapas en nyckel som heter `myKey` och gör det möjligt för diskkryptering:

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-hello-azure-active-directory-application"></a>Skapa hello Azure Active Directory-program
När virtuella diskar krypteras och dekrypteras, använder du en slutpunkt toohandle hello autentisering och byta ut kryptografiska nycklar från Nyckelvalvet. Den här slutpunkten måste ett Azure Active Directory-program kan hello Azure-plattformen toorequest hello lämpliga krypteringsnycklar uppdrag hello VM. En standardinstans för Azure Active Directory finns i din prenumeration, även om många organisationer har särskilda Azure Active Directory-kataloger.

Eftersom du inte skapar en fullständig Azure Active Directory-programmet hello `--home-page` och `--identifier-uris` parametrar i följande exempel hello behöver inte toobe faktiska dirigerbara adresser. hello anger följande exempel också en lösenordsbaserad hemlighet i stället för att nycklarna från i hello Azure-portalen. Som den här tiden kan kan generera nycklar inte utföras från hello Azure CLI.

Skapa Azure Active Directory-program. hello följande exempel skapar ett program med namnet `myAADApp` och använder ett lösenord på `myPassword`. Ange ditt eget lösenord på följande sätt:

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Anteckna hello `applicationId` som returneras i hello utdata från hello föregående kommando. Program-ID används i vissa hello återstående steg. Skapa sedan en tjänstens huvudnamn (SPN) så att programmet hello är tillgänglig i din miljö. toosuccessfully kryptera eller dekryptera virtuella diskar, behörigheter på hello kryptografiska nyckel som lagras i Nyckelvalvet måste vara set toopermit hello Azure Active Directory application tooread hello nycklar.

Skapa hello SPN och ange hello lämpliga behörigheter på följande sätt:

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Lägg till en virtuell disk och granska krypteringsstatus
tooactually kryptera vissa virtuella diskar, kan du lägga till en disk tooan befintliga VM. Lägg till 5Gb data disk tooan befintlig virtuell dator på följande sätt:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

hello virtuella diskar krypteras inte. Granska hello aktuella krypteringsstatus för den virtuella datorn på följande sätt:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Kryptera virtuella diskar
toonow kryptera hello virtuella diskar, du samordnar alla hello tidigare komponenter:

1. Ange hello Azure Active Directory-program och lösenord.
2. Ange hello Key Vault toostore hello metadata för krypterade diskarna.
3. Ange hello krypteringsnycklar toouse för hello faktiska kryptering och dekryptering.
4. Ange om du vill tooencrypt hello OS-disk, hello datadiskar eller alla.

Kan granska hello detaljer för din Azure Key Vault och hello nyckel som du skapade när du behöver hello Key Vault-ID, URI, och ange sedan URL hello sista steget:

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Kryptera din virtuella diskar med hjälp av hello utdata från hello `azure keyvault show` och `azure keyvault key show` kommandon på följande sätt:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Som hello föregående kommando har många variabler, hello följande exempel är hello fullständiga kommandot referens:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

hello Azure CLI anger inte utförlig fel under hello krypteringsprocessen. Ytterligare felsökningsinformation `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` på hello VM som du krypterar.

Slutligen kan granska hello krypteringsstatus igen tooconfirm virtuella diskarna har nu krypterats:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Lägg till ytterligare datadiskar
När du har krypterade datadiskarna kan du också kryptera dem senare lägga till ytterligare virtuella diskar tooyour VM. När du kör hello `azure vm enable-disk-encryption` kommandot, öka hello sekvens version med hello `--sequence-version` parameter. Den här aktivitetssekvensen versionsparametern kan du tooperform upprepade åtgärder på hello samma virtuella dator.

Exempelvis kan du lägga till en andra virtuell disk tooyour VM enligt följande:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Kör hello kommandot tooencrypt hello virtuella diskar, nu lägga till hello `--sequence-version` parameter och ökar hello värde från våra första kör på följande sätt:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Nästa steg
* Läs mer om hur du hanterar Azure Key Vault, inklusive ta bort krypteringsnycklarna och valv, [hantera Key Vault med hjälp av CLI](../../key-vault/key-vault-manage-with-cli2.md).
* Läs mer om diskkryptering, till exempel förbereda en krypterad anpassade VM tooupload tooAzure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
