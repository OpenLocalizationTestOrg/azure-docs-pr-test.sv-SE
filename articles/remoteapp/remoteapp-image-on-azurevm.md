---
title: "Skapa en Azure RemoteApp-avbildning baserat på en virtuell dator i Azure | Microsoft Docs"
description: "Lär dig mer om att skapa en avbildning för Azure RemoteApp genom att börja med en virtuell Azure-dator."
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
ms.openlocfilehash: ee64b86835af8e6237cddcd8acc779fc6dac8214
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Skapa en Azure RemoteApp-avbildning baserat på en virtuell Azure-dator
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Du kan skapa Azure RemoteApp-bilder (som rymmer de appar som du delar i samlingen) från en virtuell Azure-dator. Du kan också välja att använda en avbildning av virtuell dator som vi har lagt till i avbildningen Virtuella Azure-galleriet som uppfyller alla krav för Azure RemoteApp-avbildning – du kan använda den VM-avbildningen som en startpunkt för egna VM, om du vill. Sök bara efter ”Windows Server värd för fjärrskrivbordssession” avbildning i biblioteket.

Det finns två steg för att skapa en egen avbildning baserat på en Azure VM – skapa avbildningen och överföra den Virtuella Azure-biblioteket till Azure RemoteApp.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Skapa en anpassad avbildning baserat på en Azure VM
Följ dessa steg för att skapa en avbildning baserat på en virtuell dator i Azure.

1. Skapa en virtuell Azure-dator. Du kan använda den ”Windows Server värd för fjärrskrivbordssession” eller ”Windows Server Remote Desktop Session Host med Microsoft Office 365 ProPlus” bilden från galleriet virtuella Azure-datorn för avbildning. Den här avbildningen uppfyller alla Azure RemoteApp mallen avbildningen.
   
    Mer information finns i [skapa en virtuell dator som kör Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Ansluta till den virtuella datorn och installera och konfigurera de appar som du vill dela via RemoteApp. Se till att utföra några ytterligare Windows-konfigurationer som krävs av dina appar.
   
    Mer information finns i [så att logga in på en virtuell dator kör Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Om du använder en av avbildningarna Windows Server värd för fjärrskrivbordssession, finns det en inkluderade verifieringsskriptet som ser till att den virtuella datorn uppfyller RemoteApp pre-reqs. Kör skript genom att dubbelklicka på **ValidateRemoteAppImage** på skrivbordet. Se till att alla fel som rapporteras av skriptet korrigeras innan du fortsätter till nästa steg.
4. SYSPREP generalize och spara avbildningen. Se [så här skapar du en virtuell dator i Windows till mall](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) anvisningar.

## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a>Importera den till Azure RemoteApp-bildbibliotek
Följ dessa steg för att importera den nya avbildningen till Azure RemoteApp:

1. I den **Mallavbildningarna** fliken:
   
   * Om du har inga befintliga bilder, klickar du på **överföra eller importera en Mallavbildningen**.
   * Om du redan har minst en bild, klickar du på  **+**  att lägga till en ny avbildning.
2. Välj **importera en bild från de virtuella datorerna** biblioteket och klicka sedan på **nästa**.
3. Välj den anpassade avbildningen i listan och bekräfta att du har följt stegen när du skapade avbildningen på nästa sida. Klicka på **Nästa**.
4. Ange ett namn för den nya avbildningen för RemoteApp- och välj platsen, och klicka sedan på bockmarkeringen för att starta importen.

> [!NOTE]
> Du kan importera bilder från Azure-plats som stöds av Azure virtuella datorer till Azure-plats som stöds av Azure RemoteApp. Importen kan ta upp till 25 minuter beroende på platserna.
> 
> 

Nu är du redo att skapa den nya samlingen antingen en [moln](remoteapp-create-cloud-deployment.md) samling eller [hybrid](remoteapp-create-hybrid-deployment.md), beroende på dina behov.

