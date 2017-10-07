---
title: aaaRemote skrivbord tooa Linux VM | Microsoft Docs
description: "Lär dig hur tooinstall och konfigurera fjärrskrivbord tooconnect tooa Microsoft Azure Linux VM för hello klassiska distributionsmodellen"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 34348659-ddb7-41da-82d6-b5885859e7e4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: mingzhan
ms.openlocfilehash: aadd6e87883cf9cacf9d198b680669d594206e61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-remote-desktop-tooconnect-tooa-microsoft-azure-linux-vm"></a>Med hjälp av fjärrskrivbord tooconnect tooa Microsoft Azure Linux VM
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Hello uppdaterades Resource Manager-versionen av den här artikeln finns [här](../use-remote-desktop.md).

## <a name="overview"></a>Översikt
RDP (Remote Desktop Protocol) är ett protokoll för Windows. Hur kan vi för att använda RDP tooconnect tooa Linux VM (virtuell dator) från en annan dator?

Den här vägledningen ger hello svaret! Det hjälper dig tooinstall och config xrdp på din Microsoft Azure Linux VM, som kan du ansluta tooit med fjärrskrivbord från en Windows-dator. Vi använder Linux VM kör Ubuntu eller OpenSUSE hello exempel i den här vägledningen.

Hej xrdp verktyget är en öppen källa RDP-server som du kan använda tooconnect Linux-server med fjärrskrivbord från en Windows-dator. RDP har bättre prestanda än VNC (virtuella nätverk datorer). VNC återger använder JPEG-kvalitet grafik och kan vara långsam, RDP är snabb och crystal Rensa.

> [!NOTE]
> Du måste redan ha en Microsoft Azure-dator som kör Linux. toocreate och skapa en Linux VM finns hello [virtuella Azure Linux-datorn kursen](createportal.md).
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a>Skapa en slutpunkt för fjärrskrivbord
Vi använder hello standardslutpunkten 3389 för fjärrskrivbord i det här dokumentet. Ställ in 3389 slutpunkt som `Remote Desktop` tooyour Linux VM som nedan:

![Bild](./media/remote-desktop/endpoint-for-linux-server.png)

Om du inte vet hur tooset in en slutpunkt för den virtuella datorn, se [vägledningen](setup-endpoints.md).

## <a name="install-gnome-desktop"></a>Installera gör väldigt lätt Desktop
Ansluta tooyour Linux VM via `putty`, och installera `Gnome Desktop`.

Ubuntu, Använd:

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


För OpenSUSE, använder du:

    #sudo zypper install gnome-session

## <a name="install-xrdp"></a>Installera xrdp
Ubuntu, Använd:

    #sudo apt-get install xrdp

För OpenSUSE, använder du:

> [!NOTE]
> Uppdatera hello OpenSUSE version med hello-version som du använder i hello följande kommando. hello exemplet nedan används för `OpenSUSE 13.2`.
> 
> 

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>Starta xrdp och Ställ in xdrp tjänsten vid uppstart
För OpenSUSE, använder du:

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

För Ubuntu, xrdp startas och eanbled vid uppstart automatiskt efter installationen.

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a>Med hjälp av xfce om du använder en Ubuntu version senare än Ubuntu 12.04LTS
Eftersom hello aktuella versionen av xrdp inte stöder gör väldigt lätt skrivbordet för Ubuntu versioner senare än Ubuntu 12.04LTS, kommer vi att använda `xfce` skrivbordet i stället.

tooinstall `xfce`, använder du följande kommando:

    #sudo apt-get install xubuntu-desktop

Aktivera sedan `xfce` med det här kommandot:

    #echo xfce4-session >~/.xsession

Redigera konfigurationsfilen för hello `/etc/xrdp/startwm.sh`:

    #sudo vi /etc/xrdp/startwm.sh   

Lägg till rad hello `xfce4-session` före hello raden `/etc/X11/Xsession`.

toorestart Hej xrdp service, Använd den här:

    #sudo service xrdp restart


## <a name="connect-your-linux-vm-from-a-windows-machine"></a>Ansluta din Linux-VM från en Windows-dator
Starta hello fjärrskrivbordsklienten på en Windows-dator och ange Linux VM DNS-namn. Eller gå toohello instrumentpanelen på den virtuella datorn i hello Azure-portalen och klicka på `Connect` tooconnect din Linux VM. I så fall kan se du hello inloggningsfönstret:

![Bild](./media/remote-desktop/no2.png)

Logga in med hello användarnamn och lösenord för Linux-VM.

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du använder xrdp [http://www.xrdp.org/](http://www.xrdp.org/).
