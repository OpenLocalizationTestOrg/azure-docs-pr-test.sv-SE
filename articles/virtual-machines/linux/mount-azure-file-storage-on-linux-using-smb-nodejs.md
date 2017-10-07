---
title: "aaaMount Azure File storage i virtuella Linux-datorer med hjälp av SMB med Azure CLI 1.0 | Microsoft Docs"
description: "Hur toomount Azure File storage i virtuella Linux-datorer med hjälp av SMB"
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
ms.date: 12/07/2016
ms.author: v-livech
ms.openlocfilehash: 14a4224228cadb0ae2f05e8e5c8022ee84f138a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-by-using-smb-with-azure-cli-10"></a>Montera Azure File storage i virtuella Linux-datorer med hjälp av SMB med Azure CLI 1.0

Den här artikeln visar hur toomount Azure File storage på en Linux-VM med hjälp av hello Server Message Block (SMB)-protokollet. File storage erbjuder filresurser i hello molnet via hello SMB-standardprotokollet. hello kraven är:

* En [Azure-konto](https://azure.microsoft.com/pricing/free-trial/)
* [Secure Shell (SSH) offentliga och privata nyckelfilerna](mac-create-ssh-keys.md)

## <a name="cli-versions-toouse"></a>CLI versioner toouse
Du kan göra hello med något av följande kommandoradsgränssnittet (CLI) versioner hello:

- [Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)-vår nästa generations CLI för hello resursdistributionsmodell för hantering


## <a name="quick-commands"></a>Snabbkommandon
tooaccomplish hello aktivitet snabbt åtgärderna hello i det här avsnittet. Mer detaljerad information och kontext som börjar vid hello [”detaljerad genomgång”](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) avsnitt.

### <a name="prerequisites"></a>Krav
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
Lägg till följande rad för hello`/etc/fstab`:

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Detaljerad genomgång

File storage erbjuder filresurser i hello moln som använder hello SMB-standardprotokollet. Du kan också montera en filresurs från alla operativsystem som stöder SMB 3.0 hello senaste versionen för File storage. När du använder en SMB-monteringspunkter på Linux hämta enkelt säkerhetskopieringar tooa robust, permanenta arkivering lagringsplats som stöds av ett serviceavtal.

Flytta filer från en VM tooan SMB montering som finns på File storage är en bra sätt toodebug loggar. Det beror på att hello samma SMB-resurs kan monteras lokalt tooyour Mac, Linux eller Windows-arbetsstation. SMB är hello bästa lösningen för direktuppspelning av Linux eller programloggar i realtid, eftersom hello SMB-protokollet inte är inbyggda toohandle sådana uppgifter tunga loggning. En dedikerad, enhetlig loggning layer verktyg som till exempel Fluentd är ett bättre alternativ än SMB för att samla in Linux- och programmet loggning utdata.

Den här detaljerade genomgången skapar vi hello krav behövs toofirst skapa hello fillagringsresursen och montera den via SMB på en Linux-VM.

1. Skapa ett Azure storage-konto med hjälp av hello följande kod:

    ```azurecli
    azure storage account create myStorageAccount \
    --sku-name lrs \
    --kind storage \
    -l westus \
    -g myResourceGroup
    ```

2. Visa hello lagringskontonycklar.

    När du skapar ett lagringskonto skapas hello nycklar i par så att de kan roteras utan några avbrott i tjänsten. När du växlar toohello andra nyckeln i hello par kan skapa du ett nytt nyckelpar. Ny lagringskontonycklar skapas alltid parvis, se till att du alltid har minst en oanvända lagring viktiga redo tooswitch till. tooshow hello lagringskontonycklar, Använd hello följande kod:

    ```azurecli
    azure storage account keys list myStorageAccount \
    --resource-group myResourceGroup
    ```
3. Skapa hello File storage-resurs.

    Hej fillagringsresursen innehåller hello SMB-resurs. hello kvot uttrycks alltid i gigabyte (GB). toocreate hello File storage-resurs, använder hello följande kod:

    ```azurecli
    azure storage share create mystorageshare \
    --quota 10 \
    --account-name myStorageAccount \
    --account-key nPOgPR<--snip-->4Q==
    ```

4. Skapa hello monteringspunkt katalog.

    Du måste skapa en lokal katalog i hello Linux filen system toomount hello SMB-resurs till. Något skrivs eller läsa från hello lokal montering directory vidarebefordras toohello SMB-resurs som är värd för lagring av filer. toocreate hello directory, Använd hello följande kod:

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

5. Montera hello SMB-resurs med hjälp av hello följande kod:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
    ```

6. Spara hello SMB montera via omstarter.

    När du startar om hello Linux VM är hello monterade SMB-resurs frånkopplade vid avstängningen. tooremount hello SMB-resursen på Start, måste du lägga till en rad toohello Linux /etc/fstab. Linux använder hello fstab filen toolist hello filsystem måste toomount under hello startprocessen. Att lägga till hello SMB-resurs säkerställer att hello File storage-resurs är en permanent monterade filsystem för hello Linux VM. Lägger till hello File storage SMB-resursen tooa är ny virtuell dator möjligt när du använder molntjänster initiering.

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a>Nästa steg

- [Med hjälp av molnet init toocustomize en Linux VM under skapande av](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Lägg till en disk tooa Linux VM](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Kryptera diskar på en Linux-VM med hjälp av hello Azure CLI](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
