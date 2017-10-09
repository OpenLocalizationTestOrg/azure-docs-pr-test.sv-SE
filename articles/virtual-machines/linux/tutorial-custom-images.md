---
title: aaaCreate anpassade VM-avbildningar med hello Azure CLI | Microsoft Docs
description: "Självstudiekurs – skapa en anpassad VM-avbildning med hjälp av hello Azure CLI."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/21/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 217a993c0c1d48939b74108ac6c5f7a1a619416c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-hello-cli"></a>Skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av hello CLI

Anpassade avbildningar liknar marketplace-bilder, men du skapa dem själv. Anpassade avbildningar kan vara används toobootstrap konfigurationer, till exempel förinstalleras program, Programinställningar och andra OS-konfigurationer. I den här självstudiekursen skapar du en egen anpassad avbildning av en virtuell Azure-dator. Lär dig att:

> [!div class="checklist"]
> * Ta bort etableringen och generalisera virtuella datorer
> * Skapa en egen avbildning
> * Skapa en virtuell dator från en anpassad avbildning
> * Visa en lista med alla hello bilder i din prenumeration
> * Ta bort en bild


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Innan du börjar

hello stegen nedan i detalj hur tootake en befintlig virtuell dator och aktivera den till en återanvändbara anpassad avbildning som du kan använda toocreate nya VM-instanser.

toocomplete hello exempel i den här självstudiekursen, måste du ha en befintlig virtuell dator. Om det behövs, detta [skriptexempel](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) kan skapa en åt dig. När gå igenom självstudiekursen hello ersätter namn hello resursgrupp och VM där det behövs.

## <a name="create-a-custom-image"></a>Skapa en egen avbildning

toocreate en avbildning av en virtuell dator måste tooprepare hello VM genom avetablering det frigjorts och markera hello källa VM som generaliserad. En gång hello VM har förberetts, du kan skapa en avbildning.

### <a name="deprovision-hello-vm"></a>Ta bort etableringen hello VM 

Avetablering generalisering hello VM genom att ta bort information om datorn. Den här generalisering gör det möjligt toodeploy många virtuella datorer från en enda avbildning. Under avetablering, återställa hello värdnamn för*localhost.localdomain*. SSH-värdnycklar, nameserver konfigurationer, rotlösenordet och cachelagrade DHCP-lån tas också bort.

toodeprovision hello VM, använda hello Azure VM-agenten (waagent). hello Azure VM-agenten är installerad på hello VM och hanterar etablering och interagerar med hello Azure-Infrastrukturkontrollanten. Mer information finns i hello [Azure Linux-agenten användarhandboken](agent-user-guide.md).

Ansluta tooyour VM med SSH och kör hello kommandot toodeprovision hello VM. Med hello `+user` argument, hello senaste kontot för etablerad användare och alla associerade data tas också bort. Ersätt hello IP-adressen med hello offentliga IP-adressen för den virtuella datorn.

SSH toohello VM.
```bash
ssh azureuser@52.174.34.95
```
Ta bort etableringen hello VM.

```bash
sudo waagent -deprovision+user -force
```
Stäng hello SSH-session.

```bash
exit
```

### <a name="deallocate-and-mark-hello-vm-as-generalized"></a>Frigöra och markera hello VM som generaliserad

toocreate en avbildning måste hello VM toobe frigjorts. Frigöra hello VM med hjälp av [az vm frigöra](/cli//azure/vm#deallocate). 
   
```azurecli-interactive 
az vm deallocate --resource-group myResourceGroup --name myVM
```

Ange slutligen hello tillstånd för hello VM som generaliserad med [az vm generalisera](/cli//azure/vm#generalize) så hello Azure-plattformen vet hello VM har generaliserats. Du kan bara skapa en avbildning från en generaliserad virtuell dator.
   
```azurecli-interactive 
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-hello-image"></a>Skapa hello avbildning

Nu kan du skapa en avbildning av hello VM med hjälp av [az bild skapa](/cli//azure/image#create). hello följande exempel skapas en bild med namnet *myImage* från en virtuell dator med namnet *myVM*.
   
```azurecli-interactive 
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-hello-image"></a>Skapa virtuella datorer från hello-bild

Nu när du har skapat en avbildning kan du skapa en eller flera nya virtuella datorer från hello avbildning med [az vm skapa](/cli/azure/vm#create). hello följande exempel skapas en virtuell dator med namnet *myVMfromImage* från hello bild med namnet *myImage*.

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a>Avbildningshantering 

Här följer några exempel på vanliga hanteringsuppgifter för avbildningen och hur toocomplete dem med hello Azure CLI.

Visa alla avbildningar av namnet i tabellformat.

```azurecli-interactive 
az image list \
  --resource-group myResourceGroup
```

Ta bort en bild. Det här exemplet tar bort hello bild med namnet *myOldImage* från hello *myResourceGroup*.

```azurecli-interactive 
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

I kursen får skapat du en anpassad VM-avbildning. Du har lärt dig att:

> [!div class="checklist"]
> * Ta bort etableringen och generalisera virtuella datorer
> * Skapa en egen avbildning
> * Skapa en virtuell dator från en anpassad avbildning
> * Visa en lista med alla hello bilder i din prenumeration
> * Ta bort en bild

Avancera toohello nästa självstudiekurs toolearn om virtuella datorer med hög tillgänglighet.

> [!div class="nextstepaction"]
> [Skapa högtillgängliga virtuella datorer](tutorial-availability-sets.md).

