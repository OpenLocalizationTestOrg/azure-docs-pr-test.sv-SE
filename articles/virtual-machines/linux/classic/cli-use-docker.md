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
# <a name="using-hello-docker-vm-extension-from-hello-azure-command-line-interface-azure-cli"></a><span data-ttu-id="dd905-103">Med hjälp av hello Docker VM-tillägget från hello Azure-kommandoradsgränssnittet (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="dd905-103">Using hello Docker VM Extension from hello Azure Command-line Interface (Azure CLI)</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="dd905-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="dd905-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="dd905-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="dd905-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="dd905-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="dd905-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="dd905-107">Information om hur du använder hello Docker VM-tillägget med hello Resource Manager-modellen finns [här](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dd905-107">For information about using hello Docker VM extension with hello Resource Manager model, see [here](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="dd905-108">Det här avsnittet beskrivs hur toocreate en virtuell dator med hello Docker VM-tillägget från hello service management (asm) läge i Azure CLI på valfri plattform.</span><span class="sxs-lookup"><span data-stu-id="dd905-108">This topic describes how toocreate a VM with hello Docker VM Extension from hello service management (asm) mode in Azure CLI on any platform.</span></span> <span data-ttu-id="dd905-109">[Docker](https://www.docker.com/) är en av hello populäraste virtualisering metoder som använder [Linux behållare](http://en.wikipedia.org/wiki/LXC) i stället för virtuella datorer som ett sätt att isolera data och databehandling i delade resurser.</span><span class="sxs-lookup"><span data-stu-id="dd905-109">[Docker](https://www.docker.com/) is one of hello most popular virtualization approaches that uses [Linux containers](http://en.wikipedia.org/wiki/LXC) rather than virtual machines as a way of isolating data and computing on shared resources.</span></span> <span data-ttu-id="dd905-110">Du kan använda hello Docker VM-tillägget och hello [Azure Linux-agenten](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate en Docker virtuell dator som är värd för valfritt antal behållare för dina program på Azure.</span><span class="sxs-lookup"><span data-stu-id="dd905-110">You can use hello Docker VM extension and hello [Azure Linux Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate a Docker VM that hosts any number of containers for your applications on Azure.</span></span> <span data-ttu-id="dd905-111">toosee en utförlig beskrivning av behållare och deras fördelar finns hello [Docker hög nivå Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span><span class="sxs-lookup"><span data-stu-id="dd905-111">toosee a high-level discussion of containers and their advantages, see hello [Docker High Level Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span></span>

## <a name="how-toouse-hello-docker-vm-extension-with-azure"></a><span data-ttu-id="dd905-112">Hur toouse hello Docker VM-tillägget med Azure</span><span class="sxs-lookup"><span data-stu-id="dd905-112">How toouse hello Docker VM Extension with Azure</span></span>
<span data-ttu-id="dd905-113">toouse hello Docker VM-tillägget med Azure, måste du installera en version av hello [Azure-kommandoradsgränssnittet](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) högre än 0.8.6 (som den aktuella versionen av den här skrivning hello är 0.10.0).</span><span class="sxs-lookup"><span data-stu-id="dd905-113">toouse hello Docker VM extension with Azure, you must install a version of hello [Azure Command-Line Interface](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) higher than 0.8.6 (as of this writing hello current version is 0.10.0).</span></span> <span data-ttu-id="dd905-114">Du kan installera hello Azure CLI för Mac, Linux och Windows.</span><span class="sxs-lookup"><span data-stu-id="dd905-114">You can install hello Azure CLI on Mac, Linux, and Windows.</span></span>

<span data-ttu-id="dd905-115">hello fullständig toouse Docker i Azure är enkel:</span><span class="sxs-lookup"><span data-stu-id="dd905-115">hello complete process toouse Docker on Azure is simple:</span></span>

* <span data-ttu-id="dd905-116">Installera hello Azure CLI och dess beroenden på hello-dator som du vill toocontrol Azure (på Windows, detta är en Linux-distribution som körs som en virtuell dator)</span><span class="sxs-lookup"><span data-stu-id="dd905-116">Install hello Azure CLI and its dependencies on hello computer from which you want toocontrol Azure (on Windows, this will be a Linux distribution running as a virtual machine)</span></span>
* <span data-ttu-id="dd905-117">Använda hello Azure CLI Docker kommandon toocreate en VM Docker-värd i Azure</span><span class="sxs-lookup"><span data-stu-id="dd905-117">Use hello Azure CLI Docker commands toocreate a VM Docker host in Azure</span></span>
* <span data-ttu-id="dd905-118">Använd hello lokala Docker kommandon toomanage Docker-behållare i Docker-dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="dd905-118">Use hello local Docker commands toomanage your Docker containers in your Docker VM in Azure.</span></span>

### <a name="install-hello-azure-command-line-interface-azure-cli"></a><span data-ttu-id="dd905-119">Installera hello Azure-kommandoradsgränssnittet (Azure CLI)</span><span class="sxs-lookup"><span data-stu-id="dd905-119">Install hello Azure Command-Line Interface (Azure CLI)</span></span>
<span data-ttu-id="dd905-120">tooinstall och konfigurera hello Azure CLI, se [hur tooinstall hello Azure-kommandoradsgränssnittet](../../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="dd905-120">tooinstall and configure hello Azure CLI, see [How tooinstall hello Azure Command-Line Interface](../../../cli-install-nodejs.md).</span></span> <span data-ttu-id="dd905-121">tooconfirm hello installationen, Skriv `azure` hello Kommandotolken och efter en kort stund bör du se hello Azure CLI ASCII-bilder som visar hello basic kommandon tillgängliga tooyou.</span><span class="sxs-lookup"><span data-stu-id="dd905-121">tooconfirm hello installation, type `azure` at hello command prompt and after a short moment you should see hello Azure CLI ASCII art, which lists hello basic commands available tooyou.</span></span> <span data-ttu-id="dd905-122">Om installationen av hello fungerade bör du kunna tootype `azure help vm` och se en av hello visas kommandon är ”docker”.</span><span class="sxs-lookup"><span data-stu-id="dd905-122">If hello installation worked correctly, you should be able tootype `azure help vm` and see that one of hello listed commands is "docker".</span></span>

> [!NOTE]
> <span data-ttu-id="dd905-123">Docker har verktyg för Windows, [Docker datorn](https://docs.docker.com/installation/windows/), som du kan också använda tooautomate hello skapandet av en docker-klient som du kan använda toowork med virtuella Azure-datorer som docker-värdar.</span><span class="sxs-lookup"><span data-stu-id="dd905-123">Docker has tools for Windows, [Docker Machine](https://docs.docker.com/installation/windows/), which you can also use tooautomate hello creation of a docker client that you can use toowork with Azure VMs as docker hosts.</span></span>
> 
> 

### <a name="connect-hello-azure-cli-tootooyour-azure-account"></a><span data-ttu-id="dd905-124">Ansluta hello Azure CLI tootooyour Azure-konto</span><span class="sxs-lookup"><span data-stu-id="dd905-124">Connect hello Azure CLI tootooyour Azure Account</span></span>
<span data-ttu-id="dd905-125">Innan du kan använda hello Azure CLI måste du koppla dina Azure-autentiseringsuppgifter med hello Azure CLI på din plattform.</span><span class="sxs-lookup"><span data-stu-id="dd905-125">Before you can use hello Azure CLI you must associate your Azure account credentials with hello Azure CLI on your platform.</span></span> <span data-ttu-id="dd905-126">Hej avsnittet [hur tooconnect tooyour Azure-prenumeration](../../../xplat-cli-connect.md) förklarar hur tooeither hämta och importera din **.publishsettings** filen eller associera Azure CLI med ett organisations-id.</span><span class="sxs-lookup"><span data-stu-id="dd905-126">hello section [How tooconnect tooyour Azure subscription](../../../xplat-cli-connect.md) explains how tooeither download and import your **.publishsettings** file or associate your Azure CLI with an organizational id.</span></span>

> [!NOTE]
> <span data-ttu-id="dd905-127">Det finns vissa skillnader i beteende när du använder en eller hello andra metoder för autentisering, så behöver vara säker på att tooread hello dokumentet ovan toounderstand hello olika funktioner.</span><span class="sxs-lookup"><span data-stu-id="dd905-127">There are some differences in behavior when using one or hello other methods of authentication, so do be sure tooread hello document above toounderstand hello different functionality.</span></span>
> 
> 

### <a name="install-docker-and-use-hello-docker-vm-extension-for-azure"></a><span data-ttu-id="dd905-128">Installera Docker och använda hello Docker VM-tillägget för Azure</span><span class="sxs-lookup"><span data-stu-id="dd905-128">Install Docker and use hello Docker VM Extension for Azure</span></span>
<span data-ttu-id="dd905-129">Följ hello [Docker Installationsinstruktioner](https://docs.docker.com/installation/#installation) tooinstall Docker lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="dd905-129">Follow hello [Docker installation instructions](https://docs.docker.com/installation/#installation) tooinstall Docker locally on your computer.</span></span>

<span data-ttu-id="dd905-130">toouse Docker med en virtuell dator i Azure hello Linux bild som används för hello VM måste ha hello [Virtuella Azure Linux-agenten](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) installerad.</span><span class="sxs-lookup"><span data-stu-id="dd905-130">toouse Docker with an Azure Virtual Machine, hello Linux image used for hello VM must have hello [Azure Linux VM Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) installed.</span></span> <span data-ttu-id="dd905-131">Det finns för närvarande bara två typer av avbildningar som innehåller detta:</span><span class="sxs-lookup"><span data-stu-id="dd905-131">Currently, there are only two types of images that provide this:</span></span>

* <span data-ttu-id="dd905-132">En Ubuntu-avbildning från hello Azure-galleriet avbildningen eller</span><span class="sxs-lookup"><span data-stu-id="dd905-132">An Ubuntu image from hello Azure Image Gallery or</span></span>
* <span data-ttu-id="dd905-133">En anpassad avbildning i Linux som du har skapat med hello Azure Linux VM-agenten installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="dd905-133">A custom Linux image that you have created with hello Azure Linux VM Agent installed and configured.</span></span> <span data-ttu-id="dd905-134">Se [Virtuella Azure Linux-agenten](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för mer information om hur toobuild en anpassad Linux VM med hello Azure VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="dd905-134">See [Azure Linux VM Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information about how toobuild a custom Linux VM with hello Azure VM Agent.</span></span>

### <a name="using-hello-azure-image-gallery"></a><span data-ttu-id="dd905-135">Med hjälp av avbildningen hello Azure-galleriet</span><span class="sxs-lookup"><span data-stu-id="dd905-135">Using hello Azure Image Gallery</span></span>
<span data-ttu-id="dd905-136">Använd hello följande Azure CLI kommando toolocate hello senaste Ubuntu bilden i hello VM-galleriet toouse genom att skriva från en Bash eller en terminalsession</span><span class="sxs-lookup"><span data-stu-id="dd905-136">From a Bash or Terminal session, use hello following Azure CLI command toolocate hello most recent Ubuntu image in hello VM gallery toouse by typing</span></span>

`azure vm image list | grep Ubuntu-14_04`

<span data-ttu-id="dd905-137">och välj en av hello Avbildningsnamnet som `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, och Använd hello följande kommando toocreate en ny virtuell dator med hjälp av avbildningen.</span><span class="sxs-lookup"><span data-stu-id="dd905-137">and select one of hello image names, such as `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, and use hello following command toocreate a new VM using that image.</span></span>

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

<span data-ttu-id="dd905-138">Där:</span><span class="sxs-lookup"><span data-stu-id="dd905-138">where:</span></span>

* <span data-ttu-id="dd905-139">*&lt;namn på VM-cloudservice&gt;*  är hello namnet på hello virtuell dator som ska vara värddator för hello Docker-behållaren i Azure</span><span class="sxs-lookup"><span data-stu-id="dd905-139">*&lt;vm-cloudservice name&gt;* is hello name of hello VM that will become hello Docker container host computer in Azure</span></span>
* <span data-ttu-id="dd905-140">*&lt;användarnamnet&gt;*  är hello användarnamnet för hello rot standardanvändare hello VM</span><span class="sxs-lookup"><span data-stu-id="dd905-140">*&lt;username&gt;* is hello username of hello default root user of hello VM</span></span>
* <span data-ttu-id="dd905-141">*&lt;lösenordet&gt;*  är hello lösenordet för hello *användarnamn* kontot som uppfyller hello standarder förenklar för Azure</span><span class="sxs-lookup"><span data-stu-id="dd905-141">*&lt;password&gt;* is hello password of hello *username* account that meets hello standards of complexity for Azure</span></span>

> [!NOTE]
> <span data-ttu-id="dd905-142">För närvarande ett lösenord måste vara minst 8 tecken, innehåller en gemen och en versal, en siffra och ett specialtecken, till exempel en hello följande tecken: `!@#$%^&+=`.</span><span class="sxs-lookup"><span data-stu-id="dd905-142">Currently, a password must be at least 8 characters, contain one lower case and one upper case character, a number, and a special character such as one of hello following characters: `!@#$%^&+=`.</span></span> <span data-ttu-id="dd905-143">Nej, hello punkt hello slutet av hello föregående mening är inte ett specialtecken.</span><span class="sxs-lookup"><span data-stu-id="dd905-143">No, hello period at hello end of hello preceding sentence is NOT a special character.</span></span>
> 
> 

<span data-ttu-id="dd905-144">Om hello kommandot lyckades, bör du se något liknande hello följande, beroende på hello exakt argument och alternativ som du använde:</span><span class="sxs-lookup"><span data-stu-id="dd905-144">If hello command was successful, you should see something like hello following, depending on hello precise arguments and options you used:</span></span>

![](media/cli-use-docker/dockercreateresults.png)

> [!NOTE]
> <span data-ttu-id="dd905-145">Skapar en virtuell dator kan ta några minuter, men när den har etablerats (hello tillstånd värdet är `ReadyRole`) hello Docker daemon (hello Docker service) startar och du kan ansluta toohello Docker behållare värden.</span><span class="sxs-lookup"><span data-stu-id="dd905-145">Creating a virtual machine can take a few minutes, but after it has been provisioned (hello state value is `ReadyRole`) hello Docker daemon (hello Docker service) starts and you can connect toohello Docker container host.</span></span>
> 
> 

<span data-ttu-id="dd905-146">tootest hello Docker VM som du har skapat i Azure, typ</span><span class="sxs-lookup"><span data-stu-id="dd905-146">tootest hello Docker VM you have created in Azure, type</span></span>

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

<span data-ttu-id="dd905-147">där  *&lt;vm-namn-du-används&gt;*  är hello namnet på hello virtuell dator som du använde i samtalet för`azure vm docker create`.</span><span class="sxs-lookup"><span data-stu-id="dd905-147">where *&lt;vm-name-you-used&gt;* is hello name of hello virtual machine that you used in your call too`azure vm docker create`.</span></span> <span data-ttu-id="dd905-148">Du bör se något liknande toohello följande, vilket betyder att den virtuella datorn Docker-värden är igång och körs i Azure och väntar på att dina kommandon.</span><span class="sxs-lookup"><span data-stu-id="dd905-148">You should see something similar toohello following, which indicates that your Docker Host VM is up and running in Azure and waiting for your commands.</span></span> 

<span data-ttu-id="dd905-149">Nu kan du testa tooconnect med tooobtain för docker klientinformation (i vissa inställningar för Docker-klient, till exempel som på Mac kanske toouse `sudo`):</span><span class="sxs-lookup"><span data-stu-id="dd905-149">Now you can try tooconnect using your docker client tooobtain information (in some Docker client setups, such as that on Mac, you may have toouse `sudo`):</span></span>

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

<span data-ttu-id="dd905-150">Bara toobe säker på att det är alla fungerande, du kan undersöka hello VM för hello Docker-tillägg:</span><span class="sxs-lookup"><span data-stu-id="dd905-150">Just toobe certain that it's all working, you can examine hello VM for hello Docker extension:</span></span>

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a><span data-ttu-id="dd905-151">Docker VM autentiseringstyp</span><span class="sxs-lookup"><span data-stu-id="dd905-151">Docker Host VM Authentication</span></span>
<span data-ttu-id="dd905-152">Dessutom toocreating hello Docker VM hello `azure vm docker create` kommando skapar också automatiskt hello nödvändiga certifikat tooallow Docker klienten datorn tooconnect toohello Azure-behållaren värden med hjälp av HTTPS och hello certifikat som lagras på båda hello-klienten och värden datorer efter behov.</span><span class="sxs-lookup"><span data-stu-id="dd905-152">In addition toocreating hello Docker VM, hello `azure vm docker create` command also automatically creates hello necessary certificates tooallow your Docker client computer tooconnect toohello Azure container host using HTTPS, and hello certificates are stored on both hello client and host machines, as appropriate.</span></span> <span data-ttu-id="dd905-153">Vid efterföljande försök återanvändas och delas med nya värden för hello hello befintliga certifikat.</span><span class="sxs-lookup"><span data-stu-id="dd905-153">On subsequent attempts, hello existing certificates are reused and shared with hello new host.</span></span>

<span data-ttu-id="dd905-154">Som standard certifikat placeras i `~/.docker`, och Docker kommer att vara konfigurerade toorun på port **2376**.</span><span class="sxs-lookup"><span data-stu-id="dd905-154">By default, certificates are placed in `~/.docker`, and Docker will be configured toorun on port **2376**.</span></span> <span data-ttu-id="dd905-155">Om du vill att toouse en annan port eller katalogen, så du kan använda en av följande hello `azure vm docker create` kommandoraden alternativ tooconfigure din Docker behållare värden VM toouse en annan port eller annat certifikat för anslutande klienter:</span><span class="sxs-lookup"><span data-stu-id="dd905-155">If you would like toouse a different port or directory, then you may use one of hello following `azure vm docker create` command line options tooconfigure your Docker container host VM toouse a different port or different certificates for connecting clients:</span></span>

```
-dp, --docker-port [port]              Port toouse for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

<span data-ttu-id="dd905-156">Hej Docker-daemon på hello värden är konfigurerade toolisten för och autentisera anslutningar på hello angivna porten med hello certifikat som genereras av hello klienten `azure vm docker create` kommando.</span><span class="sxs-lookup"><span data-stu-id="dd905-156">hello Docker daemon on hello host is configured toolisten for and authenticate client connections on hello specified port using hello certificates generated by hello `azure vm docker create` command.</span></span> <span data-ttu-id="dd905-157">hello klientdatorn måste ha dessa certifikat toogain åtkomst toohello Docker-värd.</span><span class="sxs-lookup"><span data-stu-id="dd905-157">hello client machine must have these certificates toogain access toohello Docker host.</span></span>

> [!NOTE]
> <span data-ttu-id="dd905-158">En nätverksansluten värd som kör utan dessa certifikat kommer att sårbara tooanyone som kan tooconnect toohello datorn.</span><span class="sxs-lookup"><span data-stu-id="dd905-158">A networked host running without these certificates will be vulnerable tooanyone that can tooconnect toohello machine.</span></span> <span data-ttu-id="dd905-159">Se till att du förstår hello risker tooyour datorer och program innan du ändrar hello standardkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="dd905-159">Before you modify hello default configuration, ensure that you understand hello risks tooyour computers and applications.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="dd905-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dd905-160">Next steps</span></span>
* <span data-ttu-id="dd905-161">Du är klar toogo toohello [Docker användarhandboken] och använda Docker-VM.</span><span class="sxs-lookup"><span data-stu-id="dd905-161">You are ready toogo toohello [Docker User Guide] and use your Docker VM.</span></span> <span data-ttu-id="dd905-162">toocreate en Docker-aktiverade VM i nya hello-portalen finns [hur toouse hello Docker VM-tillägget med hello Portal].</span><span class="sxs-lookup"><span data-stu-id="dd905-162">toocreate a Docker-enabled VM in hello new portal, see [How toouse hello Docker VM Extension with hello Portal].</span></span>
* <span data-ttu-id="dd905-163">hello Azure Docker VM-tillägg också stöder Docker Compose som använder en deklarativ YAML filen tootake en utvecklare modelleras program över alla miljöer och skapa en konsekvent distribution.</span><span class="sxs-lookup"><span data-stu-id="dd905-163">hello Azure Docker VM extension also supports Docker Compose, which uses a declarative YAML file tootake a developer-modeled application across any environment and generate a consistent deployment.</span></span> <span data-ttu-id="dd905-164">Se [Kom igång med Docker Compose toodefine och köra ett program för flera behållare på en virtuell dator i Azure].</span><span class="sxs-lookup"><span data-stu-id="dd905-164">See [Get Started with Docker and Compose toodefine and run a multi-container application on an Azure virtual machine].</span></span>  

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
