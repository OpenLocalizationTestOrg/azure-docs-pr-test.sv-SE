---
title: "aaaUsing hello Docker VM-tillägget för Linux på Azure"
description: "Beskriver Docker-hello Azure Virtual Machines tillägg och visar hur tooprogrammatically skapa virtuella datorer i Azure som är docker-värdar från hello kommandoraden med hjälp av hello Azure CLI."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: eaff75e3-d929-4931-a4a1-8c377a8e7302
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/29/2016
ms.author: rasquill
ms.openlocfilehash: 1e192ad7c273aa9c997ea7bfa53b7de0b41a43c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-docker-vm-extension-from-hello-azure-command-line-interface-azure-cli"></a>Med hjälp av hello Docker VM-tillägget från hello Azure-kommandoradsgränssnittet (Azure CLI)
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Information om hur du använder hello Docker VM-tillägget med hello Resource Manager-modellen finns [här](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Det här avsnittet beskrivs hur toocreate en virtuell dator med hello Docker VM-tillägget från hello service management (asm) läge i Azure CLI på valfri plattform. [Docker](https://www.docker.com/) är en av hello populäraste virtualisering metoder som använder [Linux behållare](http://en.wikipedia.org/wiki/LXC) i stället för virtuella datorer som ett sätt att isolera data och databehandling i delade resurser. Du kan använda hello Docker VM-tillägget och hello [Azure Linux-agenten](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate en Docker virtuell dator som är värd för valfritt antal behållare för dina program på Azure. toosee en utförlig beskrivning av behållare och deras fördelar finns hello [Docker hög nivå Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="how-toouse-hello-docker-vm-extension-with-azure"></a>Hur toouse hello Docker VM-tillägget med Azure
toouse hello Docker VM-tillägget med Azure, måste du installera en version av hello [Azure-kommandoradsgränssnittet](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) högre än 0.8.6 (som den aktuella versionen av den här skrivning hello är 0.10.0). Du kan installera hello Azure CLI för Mac, Linux och Windows.

hello fullständig toouse Docker i Azure är enkel:

* Installera hello Azure CLI och dess beroenden på hello-dator som du vill toocontrol Azure (på Windows, detta är en Linux-distribution som körs som en virtuell dator)
* Använda hello Azure CLI Docker kommandon toocreate en VM Docker-värd i Azure
* Använd hello lokala Docker kommandon toomanage Docker-behållare i Docker-dator i Azure.

### <a name="install-hello-azure-command-line-interface-azure-cli"></a>Installera hello Azure-kommandoradsgränssnittet (Azure CLI)
tooinstall och konfigurera hello Azure CLI, se [hur tooinstall hello Azure-kommandoradsgränssnittet](../../../cli-install-nodejs.md). tooconfirm hello installationen, Skriv `azure` hello Kommandotolken och efter en kort stund bör du se hello Azure CLI ASCII-bilder som visar hello basic kommandon tillgängliga tooyou. Om installationen av hello fungerade bör du kunna tootype `azure help vm` och se en av hello visas kommandon är ”docker”.

> [!NOTE]
> Docker har verktyg för Windows, [Docker datorn](https://docs.docker.com/installation/windows/), som du kan också använda tooautomate hello skapandet av en docker-klient som du kan använda toowork med virtuella Azure-datorer som docker-värdar.
> 
> 

### <a name="connect-hello-azure-cli-tootooyour-azure-account"></a>Ansluta hello Azure CLI tootooyour Azure-konto
Innan du kan använda hello Azure CLI måste du koppla dina Azure-autentiseringsuppgifter med hello Azure CLI på din plattform. Hej avsnittet [hur tooconnect tooyour Azure-prenumeration](../../../xplat-cli-connect.md) förklarar hur tooeither hämta och importera din **.publishsettings** filen eller associera Azure CLI med ett organisations-id.

> [!NOTE]
> Det finns vissa skillnader i beteende när du använder en eller hello andra metoder för autentisering, så behöver vara säker på att tooread hello dokumentet ovan toounderstand hello olika funktioner.
> 
> 

### <a name="install-docker-and-use-hello-docker-vm-extension-for-azure"></a>Installera Docker och använda hello Docker VM-tillägget för Azure
Följ hello [Docker Installationsinstruktioner](https://docs.docker.com/installation/#installation) tooinstall Docker lokalt på datorn.

toouse Docker med en virtuell dator i Azure hello Linux bild som används för hello VM måste ha hello [Virtuella Azure Linux-agenten](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) installerad. Det finns för närvarande bara två typer av avbildningar som innehåller detta:

* En Ubuntu-avbildning från hello Azure-galleriet avbildningen eller
* En anpassad avbildning i Linux som du har skapat med hello Azure Linux VM-agenten installeras och konfigureras. Se [Virtuella Azure Linux-agenten](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för mer information om hur toobuild en anpassad Linux VM med hello Azure VM-agenten.

### <a name="using-hello-azure-image-gallery"></a>Med hjälp av avbildningen hello Azure-galleriet
Använd hello följande Azure CLI kommando toolocate hello senaste Ubuntu bilden i hello VM-galleriet toouse genom att skriva från en Bash eller en terminalsession

`azure vm image list | grep Ubuntu-14_04`

och välj en av hello Avbildningsnamnet som `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, och Använd hello följande kommando toocreate en ny virtuell dator med hjälp av avbildningen.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

Där:

* *&lt;namn på VM-cloudservice&gt;*  är hello namnet på hello virtuell dator som ska vara värddator för hello Docker-behållaren i Azure
* *&lt;användarnamnet&gt;*  är hello användarnamnet för hello rot standardanvändare hello VM
* *&lt;lösenordet&gt;*  är hello lösenordet för hello *användarnamn* kontot som uppfyller hello standarder förenklar för Azure

> [!NOTE]
> För närvarande ett lösenord måste vara minst 8 tecken, innehåller en gemen och en versal, en siffra och ett specialtecken, till exempel en hello följande tecken: `!@#$%^&+=`. Nej, hello punkt hello slutet av hello föregående mening är inte ett specialtecken.
> 
> 

Om hello kommandot lyckades, bör du se något liknande hello följande, beroende på hello exakt argument och alternativ som du använde:

![](media/cli-use-docker/dockercreateresults.png)

> [!NOTE]
> Skapar en virtuell dator kan ta några minuter, men när den har etablerats (hello tillstånd värdet är `ReadyRole`) hello Docker daemon (hello Docker service) startar och du kan ansluta toohello Docker behållare värden.
> 
> 

tootest hello Docker VM som du har skapat i Azure, typ

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

där  *&lt;vm-namn-du-används&gt;*  är hello namnet på hello virtuell dator som du använde i samtalet för`azure vm docker create`. Du bör se något liknande toohello följande, vilket betyder att den virtuella datorn Docker-värden är igång och körs i Azure och väntar på att dina kommandon. 

Nu kan du testa tooconnect med tooobtain för docker klientinformation (i vissa inställningar för Docker-klient, till exempel som på Mac kanske toouse `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Bara toobe säker på att det är alla fungerande, du kan undersöka hello VM för hello Docker-tillägg:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Docker VM autentiseringstyp
Dessutom toocreating hello Docker VM hello `azure vm docker create` kommando skapar också automatiskt hello nödvändiga certifikat tooallow Docker klienten datorn tooconnect toohello Azure-behållaren värden med hjälp av HTTPS och hello certifikat som lagras på båda hello-klienten och värden datorer efter behov. Vid efterföljande försök återanvändas och delas med nya värden för hello hello befintliga certifikat.

Som standard certifikat placeras i `~/.docker`, och Docker kommer att vara konfigurerade toorun på port **2376**. Om du vill att toouse en annan port eller katalogen, så du kan använda en av följande hello `azure vm docker create` kommandoraden alternativ tooconfigure din Docker behållare värden VM toouse en annan port eller annat certifikat för anslutande klienter:

```
-dp, --docker-port [port]              Port toouse for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

Hej Docker-daemon på hello värden är konfigurerade toolisten för och autentisera anslutningar på hello angivna porten med hello certifikat som genereras av hello klienten `azure vm docker create` kommando. hello klientdatorn måste ha dessa certifikat toogain åtkomst toohello Docker-värd.

> [!NOTE]
> En nätverksansluten värd som kör utan dessa certifikat kommer att sårbara tooanyone som kan tooconnect toohello datorn. Se till att du förstår hello risker tooyour datorer och program innan du ändrar hello standardkonfigurationen.
> 
> 

## <a name="next-steps"></a>Nästa steg
* Du är klar toogo toohello [Docker användarhandboken] och använda Docker-VM. toocreate en Docker-aktiverade VM i nya hello-portalen finns [hur toouse hello Docker VM-tillägget med hello Portal].
* hello Azure Docker VM-tillägg också stöder Docker Compose som använder en deklarativ YAML filen tootake en utvecklare modelleras program över alla miljöer och skapa en konsekvent distribution. Se [Kom igång med Docker Compose toodefine och köra ett program för flera behållare på en virtuell dator i Azure].  

<!--Anchors-->
[Subheading 1]:#subheading-1
[Subheading 2]:#subheading-2
[Subheading 3]:#subheading-3
[Next steps]:#next-steps

[How toouse hello Docker VM Extension with Azure]:#How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]:#Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]:#Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 tooanother azure.microsoft.com documentation topic]:../../virtual-machines-windows-hero-tutorial.md
[Link 2 tooanother azure.microsoft.com documentation topic]:../../../app-service-web/web-sites-custom-domain-name.md
[Link 3 tooanother azure.microsoft.com documentation topic]:../storage-whatis-account.md
[hur toouse hello Docker VM-tillägget med hello Portal]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Docker användarhandboken]:https://docs.docker.com/userguide/

[Kom igång med Docker Compose toodefine och köra ett program för flera behållare på en virtuell dator i Azure]:../docker-compose-quickstart.md
