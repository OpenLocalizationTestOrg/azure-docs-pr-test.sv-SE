---
title: "aaaDownload en Windows-VHD från Azure | Microsoft Docs"
description: "Hämta en Windows-VHD med hello Azure-portalen."
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
ms.openlocfilehash: d0ca8842db98f22751f01648c0ba4e5cde090043
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="download-a-windows-vhd-from-azure"></a>Hämta Windows VHD från Azure

I den här artikeln får du lära dig hur toodownload en [Windows virtuell hårddisk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) filen från Azure med hjälp av hello Azure-portalen. 

Virtuella datorer (VM) i Azure används [diskar](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) som plats-toostore ett operativsystem, program och data. Alla virtuella Azure-datorer har minst två diskar – en disk i Windows-operativsystem och en tillfällig disk. hello operativsystemdisk har skapats ursprungligen från en avbildning och både hello operativsystemdisken och hello avbildningen är virtuella hårddiskar som lagras i ett Azure storage-konto. Virtuella datorer kan också ha en eller flera datadiskar som lagras också som virtuella hårddiskar.

## <a name="stop-hello-vm"></a>Stoppa hello VM

En virtuell Hårddisk kan inte hämtas från Azure om den är ansluten tooa använder VM. Du måste toostop hello VM toodownload en virtuell Hårddisk. Om du vill toouse en virtuell Hårddisk som en [bild](tutorial-custom-images.md) toocreate andra virtuella datorer med nya diskar kan du använda [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) toogeneralize hello operativsystem finns i hello-filen och sedan stoppa hello VM. toouse hello VHD som en disk för en ny instans av en befintlig virtuell dator eller en datadisk du bara behöver toostop och frigöra hello VM.

toouse Hej VHD som en bild toocreate andra virtuella datorer, gör följande:

1.  Om du inte redan har gjort det loggar du in toohello [Azure-portalen](https://portal.azure.com/).
2.  [Ansluta toohello VM](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
3.  Öppna hello Kommandotolken som administratör på hello VM.
4.  Ändra hello katalogen för*%windir%\system32\sysprep* och köra sysprep.exe.
5.  Hello Systemförberedelseverktyget i dialogrutan Välj **ange System Out of Box Experience (OOBE)**, och se till att **Generalize** är markerad.
6.  Välj i avstängningsalternativ, **avstängning**, och klicka sedan på **OK**. 

toouse hello VHD som en disk för en ny instans av en befintlig virtuell dator eller en datadisk, utföra följande steg:

1.  Hej hubbmenyn i hello Azure-portalen, klicka på **virtuella datorer**.
2.  Välj hello VM hello-listan.
3.  Klicka på hello bladet för hello VM **stoppa**.

    ![Stoppa VM](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>Generera en SAS-URL

toodownload hello VHD-filen, behöver du toogenerate en [signatur för delad åtkomst (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL. När hello URL: en genereras tilldelas en förfallotid toohello URL.

1.  På menyn hello av hello-bladet för hello VM **diskar**.
2.  Välj hello operativsystemdisken för hello VM och klicka sedan på **exportera**.
3.  Ange hello förfallotid för hello-URL för*36000*.
4.  Klicka på **generera URL**.

    ![Generera en URL](./media/download-vhd/export-generate.png)

> [!NOTE]
> hello förfallotid ökas från hello standard tooprovide tillräckligt med tid för toodownload hello stor VHD-filen för ett Windows Server-operativsystem. Du kan förvänta dig en VHD-fil som innehåller hello Windows Server operativsystem tootake toodownload för flera timmar beroende på anslutningen. Om du hämtar en virtuell Hårddisk för en datadisk räcker hello standardtid. 
> 
> 

## <a name="download-vhd"></a>Hämta VHD

1.  Klicka på Hämta hello VHD-filen under hello-URL som har genererats.

    ![Hämta VHD](./media/download-vhd/export-download.png)

2.  Du kan behöva tooclick **spara** i hello webbläsare toostart hello hämtning. hello standardnamnet för hello VHD-filen är *abcd*.

    ![Klicka på Spara i hello webbläsare](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Nästa steg

- Lär dig hur för[ladda upp en VHD-filen tooAzure](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
- [Skapa hanterade diskar från ohanterade diskar i ett lagringskonto](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- [Hantera Azure-diskar med PowerShell](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

