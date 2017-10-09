---
title: "aaaCreate en Azure RemoteApp-avbildning baserat på en virtuell dator i Azure | Microsoft Docs"
description: "Lär dig hur toocreate en bild för Azure RemoteApp genom att börja med en virtuell Azure-dator."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: d41583ef-6cd8-4115-8dcb-b2cd5b3d301a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 2d432bcb15be68a2946d91b5f36f41d980726338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Skapa en Azure RemoteApp-avbildning baserat på en virtuell Azure-dator
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Du kan skapa Azure RemoteApp-bilder (som håller hello-appar som du delar i samlingen) från en virtuell Azure-dator. Du kan också välja toouse en avbildning av virtuell dator som vi har lagt till toohello Azure VM avbildningen galleriet som uppfyller alla krav för avbildningen av hello Azure RemoteApp - du kan använda den VM-avbildningen som en startpunkt för egna VM, om du vill. Titta bara för hello ”Windows Server värd för fjärrskrivbordssession” bilden hello-biblioteket.

Det finns två steg toocreate en egen avbildning baserat på en Azure VM – skapa hello avbildning och överför den från hello Azure VM biblioteket tooAzure RemoteApp.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Skapa en anpassad avbildning baserat på en Azure VM
Använd de här stegen toocreate en avbildning baserat på en virtuell dator i Azure.

1. Skapa en virtuell Azure-dator. Du kan använda hello ”Windows Server värd för fjärrskrivbordssession” eller ”Windows Server Remote Desktop Session Host med Microsoft Office 365 ProPlus” hello-avbildning från hello Azure virtuella avbildningen gallery. Den här avbildningen uppfyller alla hello Azure RemoteApp mallen avbildningen.
   
    Mer information finns i [skapa en virtuell dator som kör Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Anslut toohello VM och installera och konfigurera hello appar som du vill tooshare via RemoteApp. Se till att tooperform några ytterligare Windows-konfigurationer som krävs av dina appar.
   
    Mer information finns i [hur tooLog på tooa virtuell dator kör Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Om du använder en av hello Windows Server värd för fjärrskrivbordssession bilder, finns det en inkluderade verifieringsskriptet som ser till att den virtuella datorn uppfyller hello RemoteApp pre-reqs. toorun skript, dubbelklicka på **ValidateRemoteAppImage** hello skrivbordet. Se till att alla fel som rapporteras av skriptet hello korrigeras innan du fortsätter toohello nästa steg.
4. SYSPREP generalize och hello avbildning. Se [hur tooCapture en virtuell Windows-dator tooUse som en mall](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) anvisningar.

## <a name="import-hello-image-into-hello-azure-remoteapp-image-library"></a>Importera hello bild till hello Azure RemoteApp bildbibliotek
Använd de här stegen tooimport hello ny avbildning i Azure RemoteApp:

1. I hello **Mallavbildningarna** fliken:
   
   * Om du har inga befintliga bilder, klickar du på **överföra eller importera en Mallavbildningen**.
   * Om du redan har minst en bild, klickar du på  **+**  tooadd en ny avbildning.
2. Välj **importera en bild från de virtuella datorerna** biblioteket och klicka sedan på **nästa**.
3. Välj den anpassade avbildningen hello listan och bekräfta att du följt hello stegen när du skapade avbildningen på hello nästa sida. Klicka på **Nästa**.
4. Ange ett namn för hello ny RemoteApp-avbildning och välj hello plats och sedan på hello markering toostart hello importen.

> [!NOTE]
> Du kan importera bilder från Azure-plats som stöds av Azure Virtual Machines tooany Azure-plats som stöds av Azure RemoteApp. Hello import kan ta upp too25 minuter beroende på hello platser.
> 
> 

Nu är du klar toocreate den nya samlingen antingen en [moln](remoteapp-create-cloud-deployment.md) samling eller [hybrid](remoteapp-create-hybrid-deployment.md), beroende på dina behov.

