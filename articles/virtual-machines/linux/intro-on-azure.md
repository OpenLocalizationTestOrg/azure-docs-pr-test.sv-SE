---
title: Introduktion till Linux i Azure | Microsoft Docs
description: "Lär dig mer om hur du använder virtuella Linux-datorer i Azure."
services: virtual-machines-linux
documentationcenter: python
author: szarkos
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: b13bf305-87bf-4df3-815e-e8f6337aa6ea
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: szark
ms.openlocfilehash: 7bd0c5549a2e1f592681760d5ef464b9570ca4ab
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-linux-on-azure"></a>Introduktion till Linux på Azure
Det här avsnittet innehåller en översikt över vissa aspekter av med hjälp av virtuella Linux-datorer i Azure-molnet. Distribuera en virtuell Linux-dator är enkelt att använda en bild från galleriet.

## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Autentisering: Användarnamn, lösenord och SSH-nycklar
När du skapar en Linux-dator med hjälp av Azure portal, du uppmanas att ange antingen ett användarnamn och lösenord eller en offentlig SSH-nyckel. Valet av ett användarnamn för att distribuera en virtuell Linux-dator på Azure regleras följande begränsningar: namnen på Systemkonton (UID < 100) finns redan i den virtuella datorn inte är tillåtna, ”rot” till exempel.

* Se [skapa en virtuell dator som kör Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Se [hur du använder SSH med Linux på Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="obtaining-superuser-privileges-using-sudo"></a>Hämta superanvändare använder`sudo`
Det användarkonto som anges under distributionen av virtuella datorer instans i Azure är ett konto med privilegier. Det här kontot konfigureras med Azure Linux-agenten för att kunna upphöja behörighet till rot (superanvändare konto) med hjälp av den `sudo` verktyget. När du loggade in med det här användarkontot, kan du köra kommandon som rot med hjälp av Kommandosyntax:

    # sudo <COMMAND>

Du kan eventuellt hämta ett rot-gränssnittet med **sudo -s**.

* Se [med rotprivilegier på Linux-datorer i Azure](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="firewall-configuration"></a>Brandväggskonfiguration
Azure tillhandahåller ett inkommande paketfilter som begränsar anslutning till portar som anges i Azure-portalen. Som standard är endast tillåtna port SSH. Du kan öppna konfigurerar åtkomst till ytterligare portar på den virtuella Linux-datorn genom att konfigurera slutpunkter i Azure-portalen:

* Se: [konfigurera slutpunkter till en virtuell dator](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

Linux-avbildningar i Azure-galleriet inte aktiverar den *iptables* brandväggen som standard. Om du vill kan brandväggen konfigureras för att tillhandahålla ytterligare filtrering.

## <a name="hostname-changes"></a>Hostname ändringar
När du distribuerar en instans av en Linux-avbildning från början, måste du ange ett värdnamn för den virtuella datorn. När den virtuella datorn körs, har värdnamnet publicerats till DNS-servrar plattform så att flera virtuella datorer som är anslutna till varandra kan utföra IP-adress sökningar med värdnamn.

Använd kommandot eventuellt värdnamn ändringar är när en virtuell dator har distribuerats

    # sudo hostname <newname>

Azure Linux-agenten innehåller funktioner för att automatiskt identifiera den här namnändringen och konfigureras på rätt sätt den virtuella datorn för att spara den här ändringen och publicera den här ändringen till plattformen DNS-servrar.

* [Användarhandboken för Azure Linux-Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="cloud-init"></a>Molnet initiering
**Ubuntu** och **virtuell CoreOS** bilder använda molnet initiering på Azure, vilket ger ytterligare funktioner för att starta en virtuell dator.

* [Hur att mata in anpassade Data](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Anpassade Data och initiering molnet i Microsoft Azure](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
* [Skapa Azure växlingen partitioner som använder molnet initiering](https://wiki.ubuntu.com/AzureSwapPartitions)
* [Använda virtuell CoreOS på Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="virtual-machine-image-capture"></a>Avbildningen för virtuell dator
Azure tillhandahåller möjligheten att samla in tillståndet för en befintlig virtuell dator till en avbildning som senare kan användas för att distribuera ytterligare virtuella instanser. Azure Linux-agenten kan vara används för att återställa några av de anpassningar som utfördes under etableringen. Du kan följa stegen nedan för att avbilda en virtuell dator som en bild:

1. Kör **waagent-deprovision** Ångra allokering anpassning. Eller **waagent-deprovision + användaren** om du vill ta bort användarkontot som anges under etablering och alla associerade data.
2. Stänga ned/stänga av den virtuella datorn.
3. Klicka på **avbilda** i Azure-portalen eller Använd PowerShell eller CLI verktyg för att avbilda den virtuella datorn som en bild.
   
   * Se: [så här skapar du en virtuell Linux-dator för användning som en mall](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="attaching-disks"></a>Bifoga diskar
Varje virtuell dator har en tillfällig lokala *resurs disk* ansluten. Eftersom data på en resurs kan vara beständig mellan omstarter, det används ofta av program och processer som körs på den virtuella datorn för tillfälligt och **tillfälliga** lagring av data. Den används också för att lagra sidan eller växla filer för operativsystemet.

På Linux, resurs disken är vanligtvis hanteras av Azure Linux-agenten och automatiskt monteras **/mnt/resurs** (eller **/mnt** på Ubuntu avbildningar).

> [!NOTE]
> Observera att disken som resursen är en **tillfälliga** disk, och som kan tas bort och formateras om när den virtuella datorn har startats om.
> 
> 

På Linux datadisken kan kallas kerneln som `/dev/sdc`, och användarna måste partitionera, formatera och montera den här resursen. Detta beskrivs steg för steg i självstudiekursen: [hur du kopplar en datadisk till en virtuell dator](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

* **Se även:** [konfigurera programvarubaserad RAID på Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [konfigurera LVM på en virtuell Linux-dator i Azure](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

