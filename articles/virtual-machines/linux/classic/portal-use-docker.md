---
title: "aaaUsing Docker VM-tillägget för Linux | Microsoft Docs"
description: "Beskriver Docker och hello Azure Virtual Machines tillägg och hur toocreate Azure virtuella datorer som är docker-värdar med hello Azure CLI i klassiska distributionsmodellen."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 19cf64e8-f92c-43ad-a120-8976cd9102ac
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/27/2016
ms.author: rasquill
ms.openlocfilehash: 9d455b63c48b0c1b6f14862e072f899a73b46153
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-docker-vm-extension-with-hello-azure-classic-portal"></a><span data-ttu-id="237c1-103">Hej Docker VM-tillägget med hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="237c1-103">Using hello Docker VM Extension with hello Azure classic portal</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="237c1-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="237c1-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="237c1-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="237c1-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="237c1-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="237c1-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="237c1-107">[Docker](https://www.docker.com/) är en av hello populäraste virtualisering metoder som använder [Linux behållare](http://en.wikipedia.org/wiki/LXC) i stället för virtuella datorer som ett sätt att isolera data och databehandling i delade resurser.</span><span class="sxs-lookup"><span data-stu-id="237c1-107">[Docker](https://www.docker.com/) is one of hello most popular virtualization approaches that uses [Linux containers](http://en.wikipedia.org/wiki/LXC) rather than virtual machines as a way of isolating data and computing on shared resources.</span></span> <span data-ttu-id="237c1-108">Du kan använda hello Docker VM-tillägget som hanteras av [Azure Linux-agenten] toocreate en Docker virtuell dator som är värd för valfritt antal behållare för dina program på Azure.</span><span class="sxs-lookup"><span data-stu-id="237c1-108">You can use hello Docker VM extension managed by [Azure Linux Agent] toocreate a Docker VM that hosts any number of containers for your applications on Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="237c1-109">Det här avsnittet beskrivs hur toocreate en Docker-VM från hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="237c1-109">This topic describes how toocreate a Docker VM from hello Azure classic portal.</span></span> <span data-ttu-id="237c1-110">hur toocreate en Docker-VM kommandoraden hello, se toosee [hur toouse hello Docker VM-tillägget från hello Azure-kommandoradsgränssnittet (Azure CLI)].</span><span class="sxs-lookup"><span data-stu-id="237c1-110">toosee how toocreate a Docker VM at hello command line, see [How toouse hello Docker VM Extension from hello Azure Command-line Interface (Azure CLI)].</span></span> <span data-ttu-id="237c1-111">toosee en utförlig beskrivning av behållare och deras fördelar finns hello [Docker hög nivå Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span><span class="sxs-lookup"><span data-stu-id="237c1-111">toosee a high-level discussion of containers and their advantages, see hello [Docker High Level Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).</span></span>
> 
> 

## <a name="create-a-new-vm-from-hello-image-gallery"></a><span data-ttu-id="237c1-112">Skapa en ny virtuell dator från hello Image-galleriet</span><span class="sxs-lookup"><span data-stu-id="237c1-112">Create a new VM from hello Image Gallery</span></span>
<span data-ttu-id="237c1-113">hello första steget kräver en Azure-dator från en avbildning av Linux som stöder hello Docker VM-tillägget med en Ubuntu 14.04 LTS bild från hello avbildningen galleriet som en serveravbildning exempel och Ubuntu 14.04 skrivbordet som en klient.</span><span class="sxs-lookup"><span data-stu-id="237c1-113">hello first step requires an Azure VM from a Linux image that supports hello Docker VM Extension, using an Ubuntu 14.04 LTS image from hello Image Gallery as an example server image and Ubuntu 14.04 Desktop as a client.</span></span> <span data-ttu-id="237c1-114">I hello-portalen klickar du på **+ ny** i hello nedre vänstra hörnet toocreate en ny VM-instans och välj en Ubuntu 14.04 LTS-avbildning från hello tillgängliga alternativ eller hello Slutför avbildningen galleriet som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="237c1-114">In hello portal, click **+ New** in hello bottom left corner toocreate a new VM instance and select an Ubuntu 14.04 LTS image from hello selections available or from hello complete Image Gallery, as shown below.</span></span>

> [!NOTE]
> <span data-ttu-id="237c1-115">För närvarande stöder endast Ubuntu 14.04 LTS bilder nyare än juli 2014 hello Docker VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="237c1-115">Currently, only Ubuntu 14.04 LTS images more recent than July 2014 support hello Docker VM Extension.</span></span>
> 
> 

![Skapa en ny Ubuntu-avbildning](./media/portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a><span data-ttu-id="237c1-117">Skapa Docker-certifikat</span><span class="sxs-lookup"><span data-stu-id="237c1-117">Create Docker Certificates</span></span>
<span data-ttu-id="237c1-118">Se till att Docker är installerad på din klientdator efter hello VM har skapats.</span><span class="sxs-lookup"><span data-stu-id="237c1-118">After hello VM has been created, ensure that Docker is installed on your client computer.</span></span> <span data-ttu-id="237c1-119">(Mer information finns i [Docker Installationsinstruktioner](https://docs.docker.com/installation/#installation).)</span><span class="sxs-lookup"><span data-stu-id="237c1-119">(For details, see [Docker installation instructions](https://docs.docker.com/installation/#installation).)</span></span>

<span data-ttu-id="237c1-120">Skapa hello certifikat och nyckel filer för Docker kommunikation enligt för[kör Docker med https] och placera dem i hello  **`~/.docker`**  på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="237c1-120">Create hello certificate and key files for Docker communication according too[Running Docker with https] and place them in hello **`~/.docker`** directory on your client computer.</span></span>

> [!NOTE]
> <span data-ttu-id="237c1-121">Hej Docker VM-tillägget i hello portal kräver för närvarande autentiseringsuppgifter som är base64-kodad.</span><span class="sxs-lookup"><span data-stu-id="237c1-121">hello Docker VM Extension in hello portal currently requires credentials that are base64 encoded.</span></span>
> 
> 

<span data-ttu-id="237c1-122">Hello kommandoraden, Använd  **`base64`**  eller en annan favorit kodning verktyget toocreate base64-kodad avsnitt.</span><span class="sxs-lookup"><span data-stu-id="237c1-122">At hello command line, use **`base64`** or another favorite encoding tool toocreate base64-encoded topics.</span></span> <span data-ttu-id="237c1-123">Gör detta med en enkel uppsättning certifikatet och nyckel kan se ut ungefär toothis:</span><span class="sxs-lookup"><span data-stu-id="237c1-123">Doing this with a simple set of certificate and key files might look similar toothis:</span></span>

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-hello-docker-vm-extension"></a><span data-ttu-id="237c1-124">Lägg till hello Docker VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="237c1-124">Add hello Docker VM Extension</span></span>
<span data-ttu-id="237c1-125">tooadd hello Docker VM-tillägget, leta upp hello VM-instans som du skapade och rulla nedåt för**tillägg** och klicka på den toobring in VM-tillägg som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="237c1-125">tooadd hello Docker VM Extension, locate hello VM instance you created and scroll down too**Extensions** and click it toobring up VM Extensions, as shown below.</span></span>

> [!NOTE]
> <span data-ttu-id="237c1-126">Den här funktionen stöds i hello preview portal: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="237c1-126">This functionality is supported in hello preview portal only: https://portal.azure.com/</span></span>
> 
> 

![](media/portal-use-docker/ClickExtensions.png)

### <a name="add-an-extension"></a><span data-ttu-id="237c1-127">Lägga till ett tillägg</span><span class="sxs-lookup"><span data-stu-id="237c1-127">Add an Extension</span></span>
<span data-ttu-id="237c1-128">Klicka på hello **+ Lägg till** toodisplay hello möjliga VM-tillägg som du kan lägga till toothis VM.</span><span class="sxs-lookup"><span data-stu-id="237c1-128">Click hello **+ Add** toodisplay hello possible VM Extensions you can add toothis VM.</span></span>

![](media/portal-use-docker/ClickAdd.png)

### <a name="select-hello-docker-vm-extension"></a><span data-ttu-id="237c1-129">Välj hello Docker VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="237c1-129">Select hello Docker VM Extension</span></span>
<span data-ttu-id="237c1-130">Välj hello Docker VM-tillägg som visar hello Docker beskrivning och viktiga länkar, och klicka sedan på **skapa** på hello nedre toobegin hello installationsproceduren.</span><span class="sxs-lookup"><span data-stu-id="237c1-130">Choose hello Docker VM Extension, which brings up hello Docker description and important links, and then click **Create** at hello bottom toobegin hello installation procedure.</span></span>

![](media/portal-use-docker/ChooseDockerExtension.png)

![](media/portal-use-docker/CreateButtonFocus.png)

### <a name="add-your-certificate-and-key-files"></a><span data-ttu-id="237c1-131">Lägg till ditt certifikat och nyckelfiler:</span><span class="sxs-lookup"><span data-stu-id="237c1-131">Add your Certificate and Key Files:</span></span>
<span data-ttu-id="237c1-132">Ange hello base64-kodad versioner av CA-certifikat, servercertifikatet och din Server-nyckel i hello formulärfält som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="237c1-132">In hello form fields, enter hello base64-encoded versions of your CA Certificate, your Server Certificate, and your Server Key, as shown in hello following graphic.</span></span>

![](media/portal-use-docker/AddExtensionFormFilled.png)

> [!NOTE]
> <span data-ttu-id="237c1-133">Observera att (som i föregående bild hello) hello 2376 fylls i som standard.</span><span class="sxs-lookup"><span data-stu-id="237c1-133">Note that (as in hello preceding image) hello 2376 is filled in by default.</span></span> <span data-ttu-id="237c1-134">Du kan ange valfri slutpunkt här men hello nästa steg kan tooopen in hello matchar slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="237c1-134">You can enter any endpoint here, but hello next step will be tooopen up hello matching endpoint.</span></span> <span data-ttu-id="237c1-135">Om du ändrar hello standard se till att tooopen upp hello matchar slutpunkt i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="237c1-135">If you change hello default, make sure tooopen up hello matching endpoint in hello next step.</span></span>
> 
> 

## <a name="add-hello-docker-communication-endpoint"></a><span data-ttu-id="237c1-136">Lägg till hello Docker kommunikationsslutpunkt</span><span class="sxs-lookup"><span data-stu-id="237c1-136">Add hello Docker Communication Endpoint</span></span>
<span data-ttu-id="237c1-137">När du visar hello resursgrupp som du har skapat, Välj hello Nätverkssäkerhetsgruppen som är kopplade till den virtuella datorn och klicka på **inkommande säkerhetsregler** tooview hello regler som visas här.</span><span class="sxs-lookup"><span data-stu-id="237c1-137">When viewing hello resource group you've created, select hello Network Security Group associated with your VM, and click **Inbound Security Rules** tooview hello rules as shown here.</span></span>

![](media/portal-use-docker/AddingEndpoint.png)

<span data-ttu-id="237c1-138">Klicka på **+ Lägg till** tooadd en annan regel, och ange ett namn för hello slutpunkt i hello standardfallet (i det här exemplet **Docker**), och 2376 'Målportintervall'.</span><span class="sxs-lookup"><span data-stu-id="237c1-138">Click **+ Add** tooadd another rule, and in hello default case, enter a name for hello endpoint (in this example, **Docker**), and 2376 'Destination Port Range'.</span></span> <span data-ttu-id="237c1-139">Ange hello protokollet värdet visar **TCP**, och klicka på **OK** toocreate hello regeln.</span><span class="sxs-lookup"><span data-stu-id="237c1-139">Set hello protocol value showing **TCP**, and Click **OK** toocreate hello rule.</span></span>

![](media/portal-use-docker/AddEndpointFormFilledOut.png)

## <a name="test-your-docker-client-and-azure-docker-host"></a><span data-ttu-id="237c1-140">Testa din Docker-klienten och Azure Docker-värden</span><span class="sxs-lookup"><span data-stu-id="237c1-140">Test your Docker Client and Azure Docker Host</span></span>
<span data-ttu-id="237c1-141">Leta upp och kopiera hello namnet på den Virtuella datorns domän och kommandoraden hello från din klientdator, typen `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (där *dockerextension* ersätts av hello underdomänen för den virtuella datorn).</span><span class="sxs-lookup"><span data-stu-id="237c1-141">Locate and copy hello name of your VM's domain, and at hello command line of your client computer, type `docker --tls -H tcp://`*dockerextension*`.cloudapp.net:2376 info` (where *dockerextension* is replaced by hello subdomain for your VM).</span></span>

<span data-ttu-id="237c1-142">hello resultatet ska se ut ungefär toothis:</span><span class="sxs-lookup"><span data-stu-id="237c1-142">hello result should appear similar toothis:</span></span>

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

<span data-ttu-id="237c1-143">När du har slutfört hello senare steg nu har du en fullt fungerande Docker-värd som kör på en Azure-dator, toobe anslutna tooremotely från andra klienter.</span><span class="sxs-lookup"><span data-stu-id="237c1-143">Once you complete hello above steps, you now have a fully functional Docker host running on an Azure VM, configured toobe connected tooremotely from other clients.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="237c1-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="237c1-144">Next steps</span></span>
<span data-ttu-id="237c1-145">Du är klar toogo toohello [Docker användarhandboken] och använda Docker-VM.</span><span class="sxs-lookup"><span data-stu-id="237c1-145">You are ready toogo toohello [Docker User Guide] and use your Docker VM.</span></span> <span data-ttu-id="237c1-146">Om du vill skapa Docker-värdar för virtuella datorer i Azure via kommandoradsgränssnittet tooautomate finns [hur toouse hello Docker VM-tillägget från hello Azure-kommandoradsgränssnittet (Azure CLI)]</span><span class="sxs-lookup"><span data-stu-id="237c1-146">If you want tooautomate creating Docker hosts on Azure VMs through command line interface, see [How toouse hello Docker VM Extension from hello Azure Command-line Interface (Azure CLI)]</span></span>

<!--Anchors-->
[Create a new VM from hello Image Gallery]:#createvm
[Create Docker Certificates]:#dockercerts
[Add hello Docker VM Extension]:#adddockerextension
[Test Docker Client and Azure Docker Host]:#testclientandserver
[Next steps]:#next-steps

<!--Image references-->
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[6]:./media/markdown-template-for-new-articles/pretty49.png
[7]:./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[hur toouse hello Docker VM-tillägget från hello Azure-kommandoradsgränssnittet (Azure CLI)]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux-agenten]:../agent-user-guide.md
[Link 3 tooanother azure.microsoft.com documentation topic]:../storage-whatis-account.md

[kör Docker med https]:http://docs.docker.com/articles/https/
[Docker användarhandboken]:https://docs.docker.com/userguide/
