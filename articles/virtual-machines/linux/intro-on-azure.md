---
title: aaaIntroduction tooLinux i Azure | Microsoft Docs
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
ms.openlocfilehash: 3a931447ee23ce7000174ca314c3e10abc6b8e74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toolinux-on-azure"></a>Introduktion tooLinux på Azure
Det här avsnittet innehåller en översikt över vissa aspekter av med hjälp av virtuella Linux-datorer i hello Azure-molnet. Distribuera en virtuell Linux-dator är enkelt att med hjälp av en avbildning från hello-galleriet.

## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Autentisering: Användarnamn, lösenord och SSH-nycklar
När du skapar en Linux-dator med hjälp av hello Azure-portalen, du uppmanas tooprovide antingen ett användarnamn och lösenord eller en offentlig SSH-nyckel. hello valet av ett användarnamn för att distribuera en virtuell Linux-dator i Azure är ämne toohello följande villkor: namnen på Systemkonton (UID < 100) finns redan i hello virtuell dator är inte tillåtna, ”rot” till exempel.

* Se [skapa en virtuell dator som kör Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Se [hur tooUse SSH med Linux på Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="obtaining-superuser-privileges-using-sudo"></a>Hämta superanvändare använder`sudo`
hello-användarkonto som anges under distributionen av virtuella datorer instans i Azure är ett konto med privilegier. Det här kontot konfigureras genom att använda hello hello Azure Linux-agenten toobe kan tooelevate privilegier tooroot (superanvändare konto) `sudo` verktyget. När du loggade in med det här användarkontot, kommer du att kan toorun kommandon som rot med hello Kommandosyntax:

    # sudo <COMMAND>

Du kan eventuellt hämta ett rot-gränssnittet med **sudo -s**.

* Se [med rotprivilegier på Linux-datorer i Azure](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="firewall-configuration"></a>Brandväggskonfiguration
Azure tillhandahåller ett inkommande paketfilter som begränsar anslutningen tooports anges i hello Azure-portalen. Som standard hello endast tillåtna porten är SSH. Du kan öppna in åtkomst tooadditional portar på den virtuella Linux-datorn genom att konfigurera slutpunkter i hello Azure-portalen:

* Se: [hur tooSet in slutpunkter tooa virtuell dator](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

hello Linux bilder i hello Azure-galleriet inte aktiverar hello *iptables* brandväggen som standard. Om du vill hello brandväggen kan konfigureras tooprovide ytterligare filtrering.

## <a name="hostname-changes"></a>Hostname ändringar
När du distribuerar en instans av en Linux-avbildning från början, är obligatoriska tooprovide ett värdnamn för hello virtuella datorn. När hello virtuella datorn körs, är det här värdnamnet publicerade toohello plattform DNS-servrar så att flera virtuella datorer anslutna tooeach andra kan utföra IP-adress sökningar med värdnamn.

Om värdnamnet ändringar önskas när en virtuell dator har distribuerats, Använd hello kommandot

    # sudo hostname <newname>

hello Azure Linux-agenten innehåller funktioner tooautomatically identifiera den här namnändringen korrekt hello virtuella toopersist ändringen och konfigurera publicera den här ändringen toohello plattform DNS-servrar.

* [Användarhandboken för Azure Linux-Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="cloud-init"></a>Molnet initiering
**Ubuntu** och **virtuell CoreOS** bilder använda molnet initiering på Azure, vilket ger ytterligare funktioner för att starta en virtuell dator.

* [Hur tooInject anpassade Data](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Anpassade Data och initiering molnet i Microsoft Azure](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
* [Skapa Azure växlingen partitioner som använder molnet initiering](https://wiki.ubuntu.com/AzureSwapPartitions)
* [Hur tooUse virtuell CoreOS på Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="virtual-machine-image-capture"></a>Avbildningen för virtuell dator
Azure tillhandahåller hello möjlighet toocapture hello tillståndet för en befintlig virtuell dator till en bild som kan senare använda toodeploy ytterligare virtuella instanser. hello Azure Linux-agenten kanske används toorollback några av hello anpassning som utfördes under hello etableringsprocessen. Du kan följa hello stegen nedan toocapture en virtuell dator som en bild:

1. Kör **waagent-deprovision** tooundo etablering anpassning. Eller **waagent-deprovision + användaren** toooptionally ta bort hello-användarkonto som anges under etablering och alla associerade data.
2. Stänga ned/hello virtuella datorn stängas.
3. Klicka på **avbilda** i hello Azure-portalen eller Använd hello PowerShell eller CLI verktygen toocapture hello virtuell dator som en bild.
   
   * Se: [hur tooCapture en virtuell Linux-dator tooUse som en mall](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="attaching-disks"></a>Bifoga diskar
Varje virtuell dator har en tillfällig lokala *resurs disk* ansluten. Eftersom data på en resurs kan vara beständig mellan omstarter, det används ofta av program och processer som körs i hello virtuell dator för tillfällig och **tillfälliga** lagring av data. Det är också används toostore hello sidan eller växla filer för hello-operativsystem.

På Linux, hello resurs disk hanteras vanligtvis av hello Azure Linux-agenten och monterade automatiskt för**/mnt/resurs** (eller **/mnt** på Ubuntu avbildningar).

> [!NOTE]
> Observera att hello resurs disken är en **tillfälliga** disk, och som kan tas bort och formateras om när hello VM startas.
> 
> 

På Linux hello datadisk kan namnges med hello-kernel `/dev/sdc`, och användarna måste toopartition, formatera och montera den här resursen. Detta beskrivs steg för steg i självstudiekursen hello: [hur tooAttach en datadisk tooa virtuella](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

* **Se även:** [konfigurera programvarubaserad RAID på Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [konfigurera LVM på en virtuell Linux-dator i Azure](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

