---
title: aaaCapture en avbildning av en Windows Azure-dator | Microsoft Docs
description: "Hämta en avbildning av en virtuell dator för Windows Azure som skapats med hello klassiska distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: a5986eac-4cf3-40bd-9b79-7c811806b880
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: b9bbc437012aa44295f90941c9d72e39509df28f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-hello-classic-deployment-model"></a>Hämta en avbildning av en virtuell dator för Windows Azure som skapats med hello klassiska distributionsmodellen.
> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Resource Manager modellinformation finns i [samla in en hanterad avbildning av en generaliserad virtuell dator i Azure](../capture-image-resource.md).

Den här artikeln beskrivs hur du toocapture en virtuell Azure-dator med Windows så att du kan använda den som en bild toocreate andra virtuella datorer. Den här avbildningen innehåller hello operativsystemdisken och datadiskar som är anslutna toohello virtuella datorn. Det finns inget nätverkskonfigurationer, så du behöver tooset upp nätverkskonfigurationer när du skapar hello andra virtuella datorer som använder hello bild.

Azure lagrar hello bilden under **VM-avbildningar (klassisk)**, **Compute** tjänst som visas när du visar alla hello Azure-tjänster. Detta är hello samma ställe där du har överfört bilder lagras. Mer information om avbildningar finns [om avbildningar för virtuella datorer](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).

## <a name="before-you-begin"></a>Innan du börjar
De här stegen förutsätter att du har redan skapat en virtuell Azure-dator och konfigurerat hello operativsystemet, inklusive bifoga datadiskar. Om du inte gjort det ännu, se hello följande artiklar information för att skapa och förbereder hello virtuella datorn:

* [Skapa en virtuell dator från en avbildning](createportal.md)
* [Hur tooattach data disk tooa virtuell dator](attach-disk.md)
* Kontrollera att hello serverroller stöds med Sysprep. Mer information finns i [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [!WARNING]
> Den här processen tar bort hello ursprungliga virtuella datorn när den är klar.
>
>

Tidigare toocapturing en bild av en virtuell Azure-dator bör säkerhetskopieras hello virtuella måldatorn. Virtuella Azure-datorer kan säkerhetskopieras med Azure Backup. Mer information finns i [Säkerhetskopiera virtuella Azure-datorer](../../../backup/backup-azure-vms.md). Det finns andra lösningar från certifierade partner. toofind reda på vad som är tillgänglig, Sök hello Azure Marketplace.

## <a name="capture-hello-virtual-machine"></a>Avbilda hello virtuella datorn
1. I hello [Azure-portalen](http://portal.azure.com), **Anslut** toohello virtuella datorn. Instruktioner finns i [hur toosign i tooa virtuell dator som kör Windows Server][How toosign in tooa virtual machine running Windows Server].
2. Öppna ett kommandotolksfönster som administratör.
3. Ändra hello katalogen för`%windir%\system32\sysprep`, och sedan köra sysprep.exe.
4. Hej **systemförberedelseverktyget** dialogrutan visas. Hej du följande:

   * I **Rensa systemåtgärd**väljer **ange System Out of Box Experience (OOBE)** och se till att **Generalize** är markerad. Mer information om att använda Sysprep finns [hur tooUse Sysprep: en introduktion][How tooUse Sysprep: An Introduction].
   * I **avstängningsalternativ**väljer **avstängning**.
   * Klicka på **OK**.

   ![Kör Sysprep](./media/capture-image/SysprepGeneral.png)
5. Sysprep stängs av hello virtuell dator som ändrar hello status för hello virtuell dator i hello Azure-portalen för**stoppad**.
6. I hello Azure-portalen klickar du på **virtuella datorer (klassisk)** och välj hello virtuell dator som du vill toocapture. Hej **VM-avbildningar (klassisk)** grupp visas under **Compute** när du visar **fler tjänster**.

7. Klicka på hello kommandofältet **avbilda**.

   ![Avbilda virtuell dator](./media/capture-image/CaptureVM.png)

   Hej **avbilda hello virtuella** dialogrutan visas.

8. I **avbildningsnamn**, Skriv ett namn för hello ny avbildning. I **avbildningsetiketten**, ange en etikett för hello ny avbildning.

9. Klicka på **jag har kört Sysprep på den virtuella datorn hello**. Den här kryssrutan refererar toohello åtgärder med Sysprep i steg 3-5. En avbildning _måste_ vara generaliserad med Sysprep innan du lägger till en Windows Server avbildningen tooyour uppsättning anpassade avbildningar.

10. När hello avbildningen har slutförts hello ny avbildning blir tillgängligt i hello **Marketplace**, i hello **Compute**, **VM-avbildningar (klassisk)** behållare.

    ![Avbildningen lyckades](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Nästa steg
hello bilden är klar toobe används toocreate virtuella datorer. toodo, ska du skapa en virtuell dator genom att välja hello **fler tjänster** menyalternativet längst hello hello services menyn och sedan **VM-avbildningar (klassisk)** i hello **Compute**grupp. Instruktioner finns i [skapa en virtuell dator från en avbildning](createportal.md).

[How toosign in tooa virtual machine running Windows Server]:connect-logon.md
[How tooUse Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/capture-image/SysprepGeneral.png
[hello virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of hello virtual machine]: ./media/capture-image/CaptureVM.png
[Enter hello image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use hello captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
