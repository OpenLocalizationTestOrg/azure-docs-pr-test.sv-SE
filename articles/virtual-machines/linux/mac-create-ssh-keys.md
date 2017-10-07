---
title: "aaaCreate och använder en SSH-nyckelpar för Linux virtuella datorer i Azure | Microsoft Docs"
description: "Hur toocreate och använder ett SSH offentliga och privata nyckelpar för Linux virtuella datorer i Azure tooimprove hello säkerheten för hello autentiseringsprocessen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2017
ms.author: iainfou
ms.openlocfilehash: 7fb94841d34d5bc006f3134adf91102ddce5f174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a>Hur toocreate och använder en SSH-offentliga och privata nyckel koppla för Linux virtuella datorer i Azure
Du kan använda ett nyckelpar för secure shell (SSH), för att skapa virtuella datorer (VM) i Azure som använder SSH-nycklar för autentisering, vilket eliminerar hello behovet av att lösenord toolog i. Den här artikeln visar hur tooquickly generera och använda en SSH-protokollet version 2 RSA offentliga och privata nyckelfilen nyckelpar för Linux virtuella datorer. Mer detaljerad steg och ytterligare exempel finns [detaljerade steg toocreate SSH-nyckelpar och certifikat](create-ssh-keys-detailed.md).

## <a name="create-an-ssh-key-pair"></a>Skapa ett SSH-nyckelpar
Använd hello `ssh-keygen` kommandot toocreate SSH offentliga och privata nyckelfiler som är som standard som skapats i hello `~/.ssh` directory, men du kan ange en annan plats och ytterligare lösenfras (en lösenord tooaccess hello fil för privat nyckel) när Du uppmanas till detta. Kör följande kommando från ett Bash-gränssnitt hello besvara hello frågar med din egen information.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="use-hello-ssh-key-pair"></a>Använd hello SSH-nyckel
hello offentliga nyckeln som placeras på din Linux VM i Azure är som standard i `~/.ssh/id_rsa.pub`, om du har ändrat hello plats när du har skapat. Om du använder hello [Azure CLI 2.0](/cli/azure) toocreate din VM, ange hello platsen för den här offentliga nyckeln när du använder hello [az vm skapa](/cli/azure/vm#create) med hello `--ssh-key-path` alternativet. Om du kopierar och klistrar in hello innehållet i hello offentlig nyckelfil toouse i hello Azure-portalen eller Resource Manager-mall, kontrollera att du inte kopiera några ytterligare blanksteg. Till exempel om du använder OS X, kan du skicka hello-fil för offentlig nyckel (som standard **~/.ssh/id_rsa.pub**) för**pbcopy** toocopy hello innehållet (det finns andra Linux-program som hello samma sak, till exempel `xclip`).

Om du inte är bekant med offentliga nycklar för SSH kan du se din offentliga nyckel genom att köra `cat` enligt följande, och ersätta `~/.ssh/id_rsa.pub` med din egen plats för offentlig nyckelfil:

```bash
cat ~/.ssh/id_rsa.pub
```

Med hello offentlig nyckel på Azure VM, SSH tooyour VM med hello IP-adress eller DNS-namnet på den virtuella datorn (Kom ihåg tooreplace `azureuser` och `myvm.westus.cloudapp.azure.com` nedan med hello administratörsanvändarnamnet och fullständigt kvalificerat domännamn för hello- eller IP-adress):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Om du angav en lösenfras när du skapade din nyckelpar, ange hello lösenfras när frågan under hello inloggningen. (hello server läggs tooyour `~/.ssh/known_hosts` och du inte uppge tooconnect igen förrän hello offentliga nyckel på Virtuella Azure-ändringar eller hello servernamn tas bort från `~/.ssh/known_hosts`.)

## <a name="next-steps"></a>Nästa steg

Virtuella datorer som skapats med hjälp av SSH-nycklar är som standard konfigurerade med lösenord inaktiveras, toomake nyckelsökningsattacker gissa försöker frågeprestanda avsevärt dyrare och därför svårt. I det här avsnittet beskrivs hur du skapar en enkel SSH-nyckel för snabb användning. Om du behöver mer hjälp med att skapa SSH-nyckel eller kräver ytterligare certifikat, se [detaljerade steg toocreate SSH-nyckelpar och certifikat](create-ssh-keys-detailed.md).

Du kan skapa virtuella datorer som använder en SSH-nyckel med hjälp av hello Azure-portalen, CLI och mallar:

* [Skapa en säker Linux VM som använder hello Azure-portalen](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Skapa en säker Linux VM som använder hello Azure CLI 2.0)](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Skapa en säker virtuell Linux-dator med hjälp av en Azure-mall](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
