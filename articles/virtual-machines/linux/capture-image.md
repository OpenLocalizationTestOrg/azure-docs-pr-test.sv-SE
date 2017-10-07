---
title: "aaaCapture en avbildning av en Linux VM i Azure med hjälp av CLI 2.0 | Microsoft Docs"
description: "Hämta en avbildning av en Azure VM toouse för masslagring distributioner med hjälp av hello Azure CLI 2.0."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 9558332a86186b282775097428df462709373012
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-image-of-a-virtual-machine-or-vhd"></a>Hur toocreate en avbildning av en virtuell dator eller virtuell Hårddisk

<!-- generalize, image - extended version of hello tutorial-->

toocreate flera kopior av en virtuell dator (VM) toouse i Azure, en avbildning av hello VM eller hello OS-VHD. toocreate en avbildning måste du ta bort personlig information som gör det säkrare toodeploy flera gånger. Frigöra i hello följande du ta bort etableringen av en befintlig virtuell dator, och skapa en avbildning. Du kan använda den här avbildningen toocreate virtuella datorer i valfri resursgrupp i din prenumeration.

Om du vill toocreate en kopia av din befintliga Linux-VM för säkerhetskopiering eller felsökning eller ladda upp en särskild Linux VHD från en lokal virtuell dator, se [överför och skapa en Linux VM från anpassade diskavbildning](upload-vhd.md).  

Du kan också använda **förpackaren** toocreate anpassade konfigurationen. Mer information om hur du använder förpackaren finns [hur toouse förpackaren toocreate Linux virtuella bilder i Azure](build-image-with-packer.md).


## <a name="before-you-begin"></a>Innan du börjar
Se till att du uppfyller hello följande krav:

* Du behöver en Azure VM som skapats i hello Resource Manager-distributionsmodellen med hjälp av hanterade diskar. Om du inte har skapat en Linux VM, kan du använda hello [portal](quick-create-portal.md), hello [Azure CLI](quick-create-cli.md), eller [Resource Manager-mallar](create-ssh-secured-vm-from-template.md). Konfigurera hello VM efter behov. Till exempel [lägga till datadiskar](add-disk.md), tillämpa uppdateringar och installera program. 

* Du måste också toohave hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och loggas i tooan Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).

## <a name="quick-commands"></a>Snabbkommandon

En förenklad version av det här avsnittet för att testa, utvärderar eller Lär dig mer om virtuella datorer i Azure finns [skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av hello CLI](tutorial-custom-images.md).


## <a name="step-1-deprovision-hello-vm"></a>Steg 1: Ta bort etableringen hello VM
Du ta bort etableringen hello VM som använder hello Azure VM-agenten, toodelete datorn specifika filer och data. Använd hello `waagent` med hello *-deprovision + användaren* parameter på käll-Linux VM. Mer information finns i hello [Azure Linux-agenten användarhandboken](../windows/agent-user-guide.md).

1. Ansluta tooyour Linux VM som använder en SSH-klient.
2. Ange följande kommando hello i hello SSH fönstret:
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > Endast köra kommandot på en virtuell dator som du avser toocapture som en bild. Det garanterar inte hello avbildningen rensas av all känslig information eller lämpar sig för omfördelning. Hej *+ användaren* parametern tar också bort hello senaste etablerade användarkonto. Om du vill tookeep kontoautentiseringsuppgifter hello VM kan bara använda *-deprovision* tooleave hello användarkonto på plats.
 
3. Typen **y** toocontinue. Du kan lägga till hello **-tvinga** parametern tooavoid åtgärden bekräftelse.
4. När hello-kommandot har slutförts genom att skriva **avsluta**. Det här steget stänger hello SSH-klienten.

## <a name="step-2-create-vm-image"></a>Steg 2: Skapa VM-avbildning
Använd hello Azure CLI 2.0 toomark hello VM som generaliserad och hello avbildning. Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar *myResourceGroup*, *myVnet*, och *myVM*.

1. Frigöra hello virtuell dator som du avetableras med [az vm frigöra](/cli//azure/vm#deallocate). hello följande exempel tar bort hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup*:
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. Markera hello VM som generaliserad med [az vm generalisera](/cli//azure/vm#generalize). Hej följande exempel märken hello hello virtuell dator med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup* som generaliserad:
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. Nu skapa en avbildning av hello Virtuella datorresursen med [az bild skapa](/cli//azure/image#create). hello följande exempel skapas en bild med namnet *myImage* i hello resursgrupp med namnet *myResourceGroup* med hello VM resurs med namnet *myVM*:
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > hello avbildningen skapas i hello samma resursgrupp som ditt Virtuella källdatorn. Du kan skapa virtuella datorer i valfri resursgrupp i din prenumeration från den här avbildningen. Ur hantering av du toocreate en specifik resursgrupp för VM-resurser och bilder.

## <a name="step-3-create-a-vm-from-hello-captured-image"></a>Steg 3: Skapa en virtuell dator från hello avbildas avbildning
Skapa en virtuell dator med hjälp av hello-avbildning som du skapat med [az vm skapa](/cli/azure/vm#create). hello följande exempel skapas en virtuell dator med namnet *myVMDeployed* från hello bild med namnet *myImage*:

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-hello-vm-in-another-resource-group"></a>Skapa hello VM i en annan resursgrupp 

Du kan skapa virtuella datorer från en avbildning i valfri resursgrupp i din prenumeration. toocreate en virtuell dator i en annan resursgrupp än hello bilden ange hello fullständiga resurs-ID tooyour bild. Använd [az bildlista](/cli/azure/image#list) tooview en lista över avbildningar. hello utdata är liknande toohello följande exempel:

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

hello följande exempel används [az vm skapa](/cli/azure/vm#create) toocreate en virtuell dator i en annan resursgrupp än hello Källavbildningen genom att ange hello Bildresursen:

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-hello-deployment"></a>Steg 4: Verifiera hello-distribution

Nu SSH toohello virtuella datorn du har skapat tooverify hello distribution och börjar använda hello ny virtuell dator. tooconnect via SSH, hitta hello IP-adress eller FQDN för den virtuella datorn med [az vm visa](/cli/azure/vm#show):

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a>Nästa steg
Du kan skapa flera virtuella datorer från din datakälla VM-avbildning. Om du behöver toomake ändringar tooyour avbildningen: 

- Skapa en virtuell dator från en avbildning.
- Se några uppdateringar eller ändringar i konfigurationen.
- Följ hello steg igen toodeprovision, frigöra, generalisera och skapa en avbildning.
- Använd den här nya avbildningen för framtida distributioner. Om du vill ta bort hello ursprungliga avbildning.

Mer information om hur du hanterar dina virtuella datorer med hello CLI finns [Azure CLI 2.0](/cli/azure/overview).
