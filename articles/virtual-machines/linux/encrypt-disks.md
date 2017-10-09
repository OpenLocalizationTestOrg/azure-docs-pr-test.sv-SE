---
title: "aaaEncrypt diskar på en Linux-VM i Azure | Microsoft Docs"
description: "Hur tooencrypt virtuella diskar på en Linux-VM för ökad säkerhet med hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: d6197742bc8562630e8395588c072093fc01d614
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencrypt-virtual-disks-on-a-linux-vm"></a>Hur tooencrypt virtuella diskar på en Linux-VM
För förbättrad virtuell dator (VM) säkerhet och efterlevnad krypteras virtuella diskar i Azure. Diskar krypteras med kryptografiska nycklar som skyddas i ett Azure Key Vault. Du styr dessa kryptografiska nycklar och granska deras användning. Den här artikeln beskrivs hur tooencrypt virtuella diskar på en Linux VM som använder hello Azure CLI 2.0. Du kan också utföra dessa steg med hello [Azure CLI 1.0](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-commands"></a>Snabbkommandon
Om du behöver tooquickly utföra hello uppgiften, hello följande avsnitt information hello grundläggande kommandon tooencrypt virtuella diskar på den virtuella datorn. Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](#overview-of-disk-encryption).

Du behöver hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login). Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar *myResourceGroup*, *MinNyckel*, och *myVM*.

Först aktiverar hello Azure Key Vault-providern i din Azure-prenumeration med [az providern registrera](/cli/azure/provider#register) och skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapas ett Resursgruppsnamn *myResourceGroup* i hello *eastus* plats:

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

Skapa ett Azure Key Vault med [az keyvault skapa](/cli/azure/keyvault#create) och aktivera hello Key Vault för användning med diskkryptering. Ange ett unikt namn för Key Vault för *keyvault_name* på följande sätt:

```azurecli
keyvault_name=mykeyvaultikf
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

Skapa en kryptografisk nyckel i ditt Nyckelvalv med [az keyvault nyckel skapa](/cli/azure/keyvault/key#create). hello följande exempel skapas en nyckel som heter *MinNyckel*:

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```

Skapa ett huvudnamn för tjänsten med hjälp av Azure Active Directory med [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac). hello service principal handtag hello autentisering och utbyte av kryptografiska nycklar från Nyckelvalvet. följande exempel hello läser i hello värden för tjänstens huvudnamn hello-Id och lösenord för användning i senare kommandon:

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

hello lösenord är bara utdata när du skapar hello tjänstens huvudnamn. Om du vill visa och registrera hello lösenord (`echo $sp_password`). Du kan visa din service principal med [az sp annonslista](/cli/azure/ad/sp#list) och visa ytterligare information om en specifik tjänstens huvudnamn med [az ad sp visa](/cli/azure/ad/sp#show).

Ange behörigheter för ditt Nyckelvalv med [az keyvault set-policy](/cli/azure/keyvault#set-policy). I följande exempel hello, har hello service ägar-ID angetts från hello föregående kommando:

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
    --key-permissions wrapKey \
    --secret-permissions set
```

Skapa en virtuell dator med [az vm skapa](/cli/azure/vm#create) och bifoga en disk på 5 Gb data. Endast vissa marketplace-bilder stöd för kryptering. hello följande exempel skapas en virtuell dator med namnet `myVM` med hjälp av en **CentOS 7.2n** avbildningen:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH tooyour VM med hello `publicIpAddress` visas i hello utdata från hello föregående kommando. Skapa en partition och filsystemet och sedan montera hello datadisk. Mer information finns i [ansluta tooa Linux VM toomount hello ny disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Stäng SSH-sessionen.

Kryptera din virtuella dator med [az vm-kryptering aktiverar](/cli/azure/vm/encryption#enable). hello följande exempel används hello `$sp_id` och `$sp_password` variabler från föregående hello `ad sp create-for-rbac` kommando:

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

Det tar en stund innan hello disk encryption processen toocomplete. Övervaka hello status hello processen med [az vm kryptering visa](/cli/azure/vm/encryption#show):

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

Hej status visas **EncryptionInProgress**. Vänta tills hello status för hello OS disk rapporter **VMRestartPending**, starta om den virtuella datorn med [az vm omstart](/cli/azure/vm#restart):

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

hello disk krypteringsprocessen är slutförd under startprocessen hello, så Vänta några minuter innan kontrollerar hello statusen för kryptering igen med **az vm kryptering visa**:

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

hello status bör nu rapportera både hello OS-disk- och datadisk som **krypterade**.

## <a name="overview-of-disk-encryption"></a>Översikt över diskkryptering
Virtuella diskar på virtuella Linux-datorer krypteras på resten med hjälp av [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Det är gratis för kryptering av virtuella diskar i Azure. Kryptografiska nycklar lagras i Azure Key Vault med skydd av programvara eller du kan importera eller generera dina nycklar i Maskinvarusäkerhetsmoduler (HSM) certifierade tooFIPS 140-2 level 2-standarder. Du behålla kontrollen över dessa kryptografiska nycklar och granska deras användning. Dessa kryptografiska nycklar används tooencrypt och dekryptera virtuella diskar anslutna tooyour VM. Ett Azure Active Directory-tjänstens huvudnamn ger en säker mekanism för att utfärda de kryptografiska nycklarna som virtuella datorer är igång på och stänga av.

hello-processen för att kryptera en virtuell dator är som följer:

1. Skapa en kryptografisk nyckel i en Azure Key Vault.
2. Konfigurera hello kryptografiska nyckel toobe kan användas för att kryptera diskar.
3. tooread hello krypteringsnyckeln från hello Azure Key Vault skapa en Azure Active Directory service principal med hello lämpliga behörigheter.
4. Utfärda hello kommandot tooencrypt din virtuella diskar, att ange hello Azure Active Directory tjänstens huvudnamn och lämpliga kryptografiska nyckel toobe används.
5. hello Azure Active Directory service principal begäranden hello krävs krypteringsnyckeln från Azure Key Vault.
6. hello virtuella diskar krypteras med hello tillhandahålls kryptografisk nyckel.

## <a name="encryption-process"></a>Krypteringsprocessen
Kryptering är beroende av hello följande ytterligare komponenter:

* **Azure Key Vault** -används toosafeguard kryptografiska nycklar och hemligheter som används för hello disk kryptering/dekryptering process.
  * Om det finns ett kan du använda en befintlig Azure Key Vault. Du har inte toodedicate en Key Vault tooencrypting diskar.
  * Du kan skapa ett dedikerat Nyckelvalv tooseparate administrativa gränser och viktiga synlighet.
* **Azure Active Directory** - referenser hello säker utbyte av kryptografiska nycklar som krävs och autentisering för begärt åtgärder.
  * Vanligtvis kan du använda en befintlig instans av Azure Active Directory för ditt program.
  * hello tjänstens huvudnamn tillhandahåller en mekanism för säker toorequest och utfärdas hello lämpliga kryptografiska nycklar. Du utvecklar inte ett verkligt program som integreras med Azure Active Directory.

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

## <a name="create-azure-key-vault-and-keys"></a>Skapa Azure Key Vault och nycklar
Du behöver hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login). Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar *myResourceGroup*, *MinNyckel*, och *myVM*.

hello första steget är toocreate ett Azure Key Vault toostore kryptografiska nycklar. Azure Key Vault kan lagra nycklar, hemligheter, eller lösenord som gör att du toosecurely implementeras i dina program och tjänster. För virtuell diskkryptering, använda Key Vault toostore en krypteringsnyckel som används tooencrypt eller dekryptera din virtuella diskar.

Aktivera hello Azure Key Vault-providern i din Azure-prenumeration med [az providern registrera](/cli/azure/provider#register) och skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapas ett Resursgruppsnamn *myResourceGroup* i hello `eastus` plats:

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

hello Azure Key Vault som innehåller hello krypteringsnycklar och associerade beräkning resurser, till exempel lagring och hello Virtuella datorn måste finnas i hello samma region. Skapa ett Azure Key Vault med [az keyvault skapa](/cli/azure/keyvault#create) och aktivera hello Key Vault för användning med diskkryptering. Ange ett unikt namn för Key Vault för *keyvault_name* på följande sätt:

```azurecli
keyvault_name=myUniqueKeyVaultName
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

Du kan lagra kryptografiska nycklar med hjälp av programvara eller maskinvara säkerhet modellen (HSM)-skydd. Använda en HSM kräver en premium Key Vault. Det finns ett extra kostnad toocreating bidrag Key Vault snarare än standard Key Vault som lagrar programvara-skyddade nycklar. toocreate lägga till en premium Key Vault i föregående steg hello `--sku Premium` toohello kommando. hello följande exempel används programvaruskyddad nycklar eftersom vi har skapat ett Nyckelvalv som standard.

För båda modellerna skydd måste hello Azure-plattformen toobe beviljas åtkomst toorequest hello Kryptografiska nycklar när hello VM startas toodecrypt hello virtuella diskar. Skapa en kryptografisk nyckel i ditt Nyckelvalv med [az keyvault nyckel skapa](/cli/azure/keyvault/key#create). hello följande exempel skapas en nyckel som heter *MinNyckel*:

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-hello-azure-active-directory-service-principal"></a>Skapa hello Azure Active Directory-tjänstens huvudnamn
När virtuella diskar krypteras och dekrypteras, anger du ett konto toohandle hello autentisering och byta ut kryptografiska nycklar från Nyckelvalvet. Det här kontot, tjänstens huvudnamn för ett Azure Active Directory kan hello Azure-plattformen toorequest hello lämpliga krypteringsnycklar uppdrag hello VM. En standardinstans för Azure Active Directory finns i din prenumeration, även om många organisationer har särskilda Azure Active Directory-kataloger.

Skapa ett huvudnamn för tjänsten med hjälp av Azure Active Directory med [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac). följande exempel hello läser i hello värden för tjänstens huvudnamn hello-Id och lösenord för användning i senare kommandon:

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

hello lösenord visas bara när du skapar hello tjänstens huvudnamn. Om du vill visa och registrera hello lösenord (`echo $sp_password`). Du kan visa din service principal med [az sp annonslista](/cli/azure/ad/sp#list) och visa ytterligare information om en specifik tjänstens huvudnamn med [az ad sp visa](/cli/azure/ad/sp#show).

toosuccessfully kryptera eller dekryptera virtuella diskar, behörigheter på hello kryptografiska nyckel som lagras i Nyckelvalvet måste vara set toopermit hello Azure Active Directory service principal tooread hello nycklar. Ange behörigheter för ditt Nyckelvalv med [az keyvault set-policy](/cli/azure/keyvault#set-policy). I följande exempel hello, har hello service ägar-ID angetts från hello föregående kommando:

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-virtual-machine"></a>Skapa en virtuell dator
tooactually kryptera vissa virtuella diskar, kan du skapa en virtuell dator och lägger till en datadisk. Skapa en VM-tooencrypt med [az vm skapa](/cli/azure/vm#create) och bifoga en disk på 5 Gb data. Endast vissa marketplace-bilder stöd för kryptering. hello följande exempel skapas en virtuell dator med namnet *myVM* med hjälp av en **CentOS 7.2n** avbildningen:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH tooyour VM med hello `publicIpAddress` visas i hello utdata från hello föregående kommando. Skapa en partition och filsystemet och sedan montera hello datadisk. Mer information finns i [ansluta tooa Linux VM toomount hello ny disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Stäng SSH-sessionen.


## <a name="encrypt-virtual-machine"></a>Kryptera en virtuell dator
tooencrypt hello virtuella diskar kan du sammanföra alla tidigare hello-komponenter:

1. Ange hello Azure Active Directory-tjänstens huvudnamn och lösenord.
2. Ange hello Key Vault toostore hello metadata för krypterade diskarna.
3. Ange hello krypteringsnycklar toouse för hello faktiska kryptering och dekryptering.
4. Ange om du vill tooencrypt hello OS-disk, hello datadiskar eller alla.

Kryptera din virtuella dator med [az vm-kryptering aktiverar](/cli/azure/vm/encryption#enable). hello följande exempel används hello `$sp_id` och `$sp_password` variabler från föregående hello `ad sp create-for-rbac` kommando:

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

Det tar en stund innan hello disk encryption processen toocomplete. Övervaka hello status hello processen med [az vm kryptering visa](/cli/azure/vm/encryption#show):

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

hello utdata är liknande toohello följande trunkerat exempel:

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress",
]
```

Vänta tills hello status för hello OS disk rapporter **VMRestartPending**, starta om den virtuella datorn med [az vm omstart](/cli/azure/vm#restart):

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

hello disk krypteringsprocessen är slutförd under startprocessen hello, så Vänta några minuter innan kontrollerar hello statusen för kryptering igen med **az vm kryptering visa**:

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

hello status bör nu rapportera både hello OS-disk- och datadisk som **krypterade**.


## <a name="add-additional-data-disks"></a>Lägg till ytterligare datadiskar
När du har krypterade datadiskarna kan du också kryptera dem senare lägga till ytterligare virtuella diskar tooyour VM. Exempelvis kan du lägga till en andra virtuell disk tooyour VM enligt följande:

```azurecli
az vm disk attach-new --resource-group myResourceGroup --vm-name myVM --size-in-gb 5
```

Kör hello kommandot tooencrypt hello virtuella diskar på följande sätt:

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```


## <a name="next-steps"></a>Nästa steg
* Läs mer om hur du hanterar Azure Key Vault, inklusive ta bort krypteringsnycklarna och valv, [hantera Key Vault med hjälp av CLI](../../key-vault/key-vault-manage-with-cli2.md).
* Läs mer om diskkryptering, till exempel förbereda en krypterad anpassade VM tooupload tooAzure, [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
