---
title: "aaaDownload en Linux VHD från Azure | Microsoft Docs"
description: "Hämta en Linux VHD med hello Azure CLI och hello Azure-portalen."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: davidmu
ms.openlocfilehash: 7e08e985a64a6be581b8f5eedcce60fbd314eaf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="download-a-linux-vhd-from-azure"></a>Hämta en Linux-VHD från Azure

I den här artikeln får du lära dig hur toodownload en [Linux virtuell hårddisk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) filen från Azure med hjälp av hello Azure CLI och Azure-portalen. 

Virtuella datorer (VM) i Azure används [diskar](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) som plats-toostore ett operativsystem, program och data. Alla virtuella Azure-datorer har minst två diskar – en disk i Windows-operativsystem och en tillfällig disk. hello operativsystemdisk har skapats ursprungligen från en avbildning och både hello operativsystemdisken och hello avbildningen är virtuella hårddiskar som lagras i ett Azure storage-konto. Virtuella datorer kan också ha en eller flera datadiskar som lagras också som virtuella hårddiskar.

Om du inte redan har gjort det installerar [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="stop-hello-vm"></a>Stoppa hello VM

En virtuell Hårddisk kan inte hämtas från Azure om den är ansluten tooa använder VM. Du måste toostop hello VM toodownload en virtuell Hårddisk. Om du vill toouse en virtuell Hårddisk som en [bild](tutorial-custom-images.md) toocreate andra virtuella datorer med nya diskar du behöver toodeprovision och generalisera hello operativsystem finns i hello filen och stoppa hello VM. toouse hello VHD som en disk för en ny instans av en befintlig virtuell dator eller en datadisk du bara behöver toostop och frigöra hello VM.

toouse Hej VHD som en bild toocreate andra virtuella datorer, gör följande:

1. Använda SSH, hello kontonamn och hello offentliga IP-adress hello VM tooconnect tooit och ta bort etableringen av den. hello + användaren parametern tar också bort hello senaste etablerade användarkonto. Om du gräddning kontoautentiseringsuppgifter toohello VM, lämnar ut detta + user-parameter. hello följande exempel tar bort hello senaste etablerade användarkonto:

    ```bash
    ssh azureuser@40.118.249.235
    sudo waagent -deprovision+user -force
    exit 
    ```

2. Logga in tooyour Azure-konto med [az inloggningen](https://docs.microsoft.com/cli/azure/#login).
3. Stoppa och frigöra hello VM.

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. Generalisera hello VM. 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

toouse hello VHD som en disk för en ny instans av en befintlig virtuell dator eller en datadisk, utföra följande steg:

1.  Logga in toohello [Azure-portalen](https://portal.azure.com/).
2.  Hej hubbmenyn, klicka på **virtuella datorer**.
3.  Välj hello VM hello-listan.
4.  Klicka på hello bladet för hello VM **stoppa**.

    ![Stoppa VM](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>Generera en SAS-URL

toodownload hello VHD-filen, behöver du toogenerate en [signatur för delad åtkomst (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL. När hello URL: en genereras tilldelas en förfallotid toohello URL.

1.  På menyn hello av hello-bladet för hello VM **diskar**.
2.  Välj hello operativsystemdisken för hello VM och klicka sedan på **exportera**.
3.  Klicka på **generera URL**.

    ![Generera en URL](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a>Hämta VHD

1.  Klicka på Hämta hello VHD-filen under hello-URL som har genererats.

    ![Hämta VHD](./media/download-vhd/export-download.png)

2.  Du kan behöva tooclick **spara** i hello webbläsare toostart hello hämtning. hello standardnamnet för hello VHD-filen är *abcd*.

    ![Klicka på Spara i hello webbläsare](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Nästa steg

- Lär dig hur för[överför och skapa en Linux VM från anpassade disken med hello Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- [Hantera Azure-diskarna hello Azure CLI](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

