---
title: "aaaMount Azure File storage i virtuella Linux-datorer med hjälp av SMB | Microsoft Docs"
description: "Hur toomount Azure File storage i virtuella Linux-datorer med hjälp av SMB med hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/13/2017
ms.author: v-livech
ms.openlocfilehash: 7b34c81e602748b78c924f44a54b876744c28f56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>Montera Azure File storage i virtuella Linux-datorer med hjälp av SMB

Den här artikeln visar hur tooutilize hello Azure File storage-tjänst på en Linux VM som använder SMB montera med hello Azure CLI 2.0. Azure File storage erbjuder filresurser i hello molnet med hello SMB-standardprotokollet. Du kan också utföra dessa steg med hello [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). hello kraven är:

- [ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/)
- [offentliga och privata SSH-nyckelfiler](mac-create-ssh-keys.md)

## <a name="quick-commands"></a>Snabbkommandon

* En resursgrupp
* Azure-nätverk
* En nätverkssäkerhetsgrupp med en SSH inkommande
* Ett undernät
* Ett Azure storage-konto
* Azure lagringskontonycklar
* Ett Azure File storage-resurs
* En virtuell Linux-dator

Ersätt alla exempel med dina egna inställningar.

### <a name="create-a-directory-for-hello-local-mount"></a>Skapa en katalog för hello lokal montering

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-hello-file-storage-smb-share-toohello-mount-point"></a>Montera hello filen lagring SMB-resursen toohello monteringspunkt

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-hello-mount-after-a-reboot"></a>Spara hello monteringspunkter efter en omstart
toodo Lägg därför till följande rad toohello hello `/etc/fstab`:

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Detaljerad genomgång

File storage erbjuder filresurser i hello moln som använder hello SMB-standardprotokollet. Du kan också montera en filresurs från alla operativsystem som stöder SMB 3.0 hello senaste versionen för File storage. När du använder en SMB-monteringspunkter på Linux hämta enkelt säkerhetskopieringar tooa robust, permanenta arkivering lagringsplats som stöds av ett serviceavtal.

Flytta filer från en VM tooan SMB montering som finns på File storage är en bra sätt toodebug loggar. Det beror på att hello samma SMB-resurs kan monteras lokalt tooyour Mac, Linux eller Windows-arbetsstation. SMB är hello bästa lösningen för direktuppspelning av Linux eller programloggar i realtid, eftersom hello SMB-protokollet inte är inbyggda toohandle sådana uppgifter tunga loggning. En dedikerad, enhetlig loggning layer verktyg som till exempel Fluentd är ett bättre alternativ än SMB för att samla in Linux- och programmet loggning utdata.

Den här detaljerade genomgången skapar vi hello krav behövs toofirst skapa hello fillagringsresursen och montera den via SMB på en Linux-VM.

1. Skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create) toohold hello filresurs.

    en resursgrupp med namnet toocreate `myResourceGroup` i hello ”USA, västra” plats, använder du hello följande exempel:

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

2. Skapa ett Azure storage-konto med [az storage-konto skapar](/cli/azure/storage/account#create) toostore hello faktiska filer.

    toocreate ett lagringskonto med namnet mittlagringskonto med hjälp av hello Standard_LRS lagring SKU, Använd hello följande exempel:

    ```azurecli
    az storage account create --resource-group myResourceGroup \
        --name mystorageaccount \
        --location westus \
        --sku Standard_LRS
    ```

3. Visa hello lagringskontonycklar.

    När du skapar ett lagringskonto skapas hello nycklar i par så att de kan roteras utan några avbrott i tjänsten. När du växlar toohello andra nyckeln i hello par kan skapa du ett nytt nyckelpar. Ny lagringskontonycklar skapas alltid parvis, se till att du alltid har minst en oanvända storage-konto viktiga redo tooswitch till.

    Visa hello lagringskontonycklar med hello [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list). Hej lagringskontonycklar för hello med namnet `mystorageaccount` visas i följande exempel hello:

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount
    ```

    tooextract en enskild nyckel använder hello `--query` flaggan. hello följande exempel extraherar hello första nyckeln (`[0]`):

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount \
        --query '[0].{Key:value}' --output tsv
    ```

4. Skapa hello File storage-resurs.

    Hej fillagringsresursen innehåller hello SMB-resurs med [az lagringsresurs skapa](/cli/azure/storage/share#create). hello kvot uttrycks alltid i gigabyte (GB). Skicka in en av hello nycklar från föregående hello `az storage account keys list` kommando. Skapa en resurs med namnet mystorageshare med en 10 GB kvot genom att använda hello följande exempel:

    ```azurecli
    az storage share create --name mystorageshare \
        --quota 10 \
        --account-name mystorageaccount \
        --account-key nPOgPR<--snip-->4Q==
    ```

5. Skapa en monteringspunkt katalog.

    Skapa en lokal katalog i hello Linux filen system toomount hello SMB-resurs till. Något skrivs eller läsa från hello lokal montering directory vidarebefordras toohello SMB-resurs som är värd för lagring av filer. toocreate en lokal katalog på/mnt/mymountdirectory, Använd hello följande exempel:

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

6. Montera hello SMB-resursen toohello lokal katalog.

    Ange en egen lagring Kontoanvändarnamn och lagringskontonyckel hello montera autentiseringsuppgifter på följande sätt:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=mystorageaccount,password=mystorageaccountkey,dir_mode=0777,file_mode=0777
    ```

7. Spara hello SMB montera via omstarter.

    När du startar om hello Linux VM är hello monterade SMB-resurs frånkopplade vid avstängningen. tooremount hello SMB-resurs på Start, lägga till en rad toohello Linux /etc/fstab. Linux använder hello fstab filen toolist hello filsystem måste toomount under hello startprocessen. Att lägga till hello SMB-resurs säkerställer att hello File storage-resurs är en permanent monterade filsystem för hello Linux VM. Lägger till hello File storage SMB-resursen tooa är ny virtuell dator möjligt när du använder molntjänster initiering.

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a>Nästa steg

- [Med hjälp av molnet init toocustomize en Linux VM under skapande av](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Lägg till en disk tooa Linux VM](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Kryptera diskar på en Linux-VM med hjälp av hello Azure CLI](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
