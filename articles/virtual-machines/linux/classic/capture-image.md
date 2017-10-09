---
title: aaaCapture en bild av Linux VM | Microsoft Docs
description: "Lär dig hur toocapture en avbildning av en Linux-baserade Azure virtuell dator (VM) skapas med hello klassiska distributionsmodellen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17d7ffee-a58e-4290-9de1-64c3cf1ddc05
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
ms.openlocfilehash: 33c4059d5bb919a86bdc3492abca540750f365ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocapture-a-classic-linux-virtual-machine-as-an-image"></a>Hur toocapture en klassisk virtuell Linux-dator som en bild
> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Den här artikeln beskrivs hur du toocapture en klassisk Azure virtuell dator (VM) kör Linux som en bild toocreate andra virtuella datorer. Den här avbildningen innehåller hello OS-disken och datadiskar kopplade toohello VM. Det finns inget nätverkskonfigurationen, så du måste tooconfigure som när du skapar hello andra Virtuella från hello avbildning.

Azure lagrar hello bilden under **bilder**, tillsammans med eventuella avbildningar som du har överfört. Mer information om avbildningar finns [om avbildningar av virtuella datorer i Azure][About Virtual Machine Images in Azure].

## <a name="before-you-begin"></a>Innan du börjar
Dessa instruktioner förutsätter att du redan har skapat en virtuell Azure-dator med hjälp av hello klassiska distributionsmodellen och konfigurerade hello operativsystemet, inklusive bifoga datadiskar. Om du behöver toocreate en virtuell dator kan läsa [hur tooCreate en Linux-dator][How tooCreate a Linux Virtual Machine].

## <a name="capture-hello-virtual-machine"></a>Avbilda hello virtuella datorn
1. [Ansluta toohello VM](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) med hjälp av en SSH-klient av önskat slag.
2. Ange följande kommando hello i hello SSH fönstret. hello utdata från `waagent` kan variera beroende på hello version av det här verktyget:

    ```bash
    sudo waagent -deprovision+user
    ```

    hello föregående kommando försöker tooclean hello system och gör den lämplig för reprovisioning. Den här åtgärden utför hello följande uppgifter:

   * Tar bort SSH-nycklar för värd (om Provisioning.RegenerateSshHostKeyPair är ”y” i konfigurationsfilen för hello)
   * Rensar nameserver konfigurationen i /etc/resolv.conf
   * Tar bort hello `root` användarlösenord från/etc/shadow (om Provisioning.DeleteRootPassword ”y” i konfigurationsfilen för hello)
   * Tar bort cachelagrade lån för DHCP-klienter
   * Återställer värd namn toolocalhost.localdomain
   * Tar bort hello senaste etablerade användarkonto (hämtades från /var/lib/waagent) **och tillhörande data**.

     > [!NOTE]
     > Avetablering tar bort filer och data för ”generalisera” hello avbildningen. Endast köra kommandot på en virtuell dator som du avser toocapture som en ny mall för avbildningen. Det garanterar inte hello avbildningen rensas av all känslig information eller lämpar sig för Vidaredistribution toothird parter.

3. Typen **y** toocontinue. Du kan lägga till hello `-force` parametern tooavoid åtgärden bekräftelse.
4. Typen **avsluta** tooclose hello SSH-klienten.

   > [!NOTE]
   > hello stegen förutsätter att du redan har [installerat hello Azure CLI](../../../cli-install-nodejs.md) på klientdatorn. Alla hello följande steg kan även utföras i hello [Azure-portalen](http://portal.azure.com).

5. Öppna Azure CLI och inloggningen tooyour Azure-prenumeration från din klientdator. Mer information läser [ansluta tooan Azure-prenumeration från hello Azure CLI](../../../xplat-cli-connect.md).

   > [!NOTE]
   > Logga in toohello portalen i hello Azure-portalen.

6. Kontrollera att du befinner dig i Service Management-läge:

    ```azurecli
    azure config mode asm
    ```

7. Stäng hello virtuell dator som redan avetableras. hello följande exempel stängs av hello virtuella datorn med namnet `myVM`:

    ```azurecli
    azure vm shutdown myVM
    ```
   Om det behövs, kan du visa en lista över alla hello virtuella datorer som skapats i din prenumeration med hjälp av`azure vm list`

   > [!NOTE]
   > Om du använder hello Azure-portalen, Välj hello VM och klicka på **stoppa** stänga hello VM.

8. När hello VM stoppas avbildning hello. följande exempel insamlingar hello hello virtuella datorn med namnet `myVM` och skapar en generaliserad avbildning med namnet `myNewVM`:

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    Hej `-t` underkommandot borttagningar hello ursprungliga virtuella datorn.

    > [!NOTE]
    > I hello Azure-portalen, kan du göra en avbildning genom att välja **bild** hello hub-menyn. Du behöver toosupply hello följande information för hello bild: namn, resursgruppens namn, plats, typ av operativsystem och blob-sökvägen för lagring.

9. hello ny avbildning är nu tillgänglig hello listan över avbildningar som kan vara används tooconfigure någon ny virtuell dator. Du kan visa den med hello-kommando:

   ```azurecli
   azure vm image list
   ```

   På hello [Azure-portalen](http://portal.azure.com), visas hello bilden i hello **VM-avbildningar (klassisk)** som tillhör toohello **Compute** tjänster. Du kan komma åt **VM-avbildningar (klassisk)** genom att klicka på _fler tjänster_ längst hello hello Azure service lista och titta sedan hello **Compute** tjänster.   

   ![Avbildningen lyckades](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Nästa steg
hello bilden är klar toobe används toocreate virtuella datorer. Du kan använda hello Azure CLI kommando `azure vm create` och ange hello Avbildningsnamnet som du skapade. Mer information finns i [Using hello Azure CLI med klassiska distributionsmodellen](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

Du kan också använda hello [Azure-portalen](http://portal.azure.com) toocreate en egen virtuell dator med hjälp av hello **bild** metod och välja hello-avbildning som du skapade. Mer information finns i [hur tooCreate en anpassad VM][How tooCreate a Custom Virtual Machine].

**Se även:** [användarhandboken för Azure Linux-Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How tooCreate a Custom Virtual Machine]:create-custom.md
[How tooAttach a Data Disk tooa Virtual Machine]:attach-disk.md
[How tooCreate a Linux Virtual Machine]:create-custom.md
