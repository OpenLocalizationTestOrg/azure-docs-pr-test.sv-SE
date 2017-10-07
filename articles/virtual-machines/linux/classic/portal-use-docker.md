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
# <a name="using-hello-docker-vm-extension-with-hello-azure-classic-portal"></a>Hej Docker VM-tillägget med hello klassiska Azure-portalen
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

[Docker](https://www.docker.com/) är en av hello populäraste virtualisering metoder som använder [Linux behållare](http://en.wikipedia.org/wiki/LXC) i stället för virtuella datorer som ett sätt att isolera data och databehandling i delade resurser. Du kan använda hello Docker VM-tillägget som hanteras av [Azure Linux-agenten] toocreate en Docker virtuell dator som är värd för valfritt antal behållare för dina program på Azure.

> [!NOTE]
> Det här avsnittet beskrivs hur toocreate en Docker-VM från hello klassiska Azure-portalen. hur toocreate en Docker-VM kommandoraden hello, se toosee [hur toouse hello Docker VM-tillägget från hello Azure-kommandoradsgränssnittet (Azure CLI)]. toosee en utförlig beskrivning av behållare och deras fördelar finns hello [Docker hög nivå Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).
> 
> 

## <a name="create-a-new-vm-from-hello-image-gallery"></a>Skapa en ny virtuell dator från hello Image-galleriet
hello första steget kräver en Azure-dator från en avbildning av Linux som stöder hello Docker VM-tillägget med en Ubuntu 14.04 LTS bild från hello avbildningen galleriet som en serveravbildning exempel och Ubuntu 14.04 skrivbordet som en klient. I hello-portalen klickar du på **+ ny** i hello nedre vänstra hörnet toocreate en ny VM-instans och välj en Ubuntu 14.04 LTS-avbildning från hello tillgängliga alternativ eller hello Slutför avbildningen galleriet som visas nedan.

> [!NOTE]
> För närvarande stöder endast Ubuntu 14.04 LTS bilder nyare än juli 2014 hello Docker VM-tillägget.
> 
> 

![Skapa en ny Ubuntu-avbildning](./media/portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Skapa Docker-certifikat
Se till att Docker är installerad på din klientdator efter hello VM har skapats. (Mer information finns i [Docker Installationsinstruktioner](https://docs.docker.com/installation/#installation).)

Skapa hello certifikat och nyckel filer för Docker kommunikation enligt för[kör Docker med https] och placera dem i hello  **`~/.docker`**  på klientdatorn.

> [!NOTE]
> Hej Docker VM-tillägget i hello portal kräver för närvarande autentiseringsuppgifter som är base64-kodad.
> 
> 

Hello kommandoraden, Använd  **`base64`**  eller en annan favorit kodning verktyget toocreate base64-kodad avsnitt. Gör detta med en enkel uppsättning certifikatet och nyckel kan se ut ungefär toothis:

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

## <a name="add-hello-docker-vm-extension"></a>Lägg till hello Docker VM-tillägget
tooadd hello Docker VM-tillägget, leta upp hello VM-instans som du skapade och rulla nedåt för**tillägg** och klicka på den toobring in VM-tillägg som visas nedan.

> [!NOTE]
> Den här funktionen stöds i hello preview portal: https://portal.azure.com/
> 
> 

![](media/portal-use-docker/ClickExtensions.png)

### <a name="add-an-extension"></a>Lägga till ett tillägg
Klicka på hello **+ Lägg till** toodisplay hello möjliga VM-tillägg som du kan lägga till toothis VM.

![](media/portal-use-docker/ClickAdd.png)

### <a name="select-hello-docker-vm-extension"></a>Välj hello Docker VM-tillägget
Välj hello Docker VM-tillägg som visar hello Docker beskrivning och viktiga länkar, och klicka sedan på **skapa** på hello nedre toobegin hello installationsproceduren.

![](media/portal-use-docker/ChooseDockerExtension.png)

![](media/portal-use-docker/CreateButtonFocus.png)

### <a name="add-your-certificate-and-key-files"></a>Lägg till ditt certifikat och nyckelfiler:
Ange hello base64-kodad versioner av CA-certifikat, servercertifikatet och din Server-nyckel i hello formulärfält som visas i följande bild hello.

![](media/portal-use-docker/AddExtensionFormFilled.png)

> [!NOTE]
> Observera att (som i föregående bild hello) hello 2376 fylls i som standard. Du kan ange valfri slutpunkt här men hello nästa steg kan tooopen in hello matchar slutpunkt. Om du ändrar hello standard se till att tooopen upp hello matchar slutpunkt i hello nästa steg.
> 
> 

## <a name="add-hello-docker-communication-endpoint"></a>Lägg till hello Docker kommunikationsslutpunkt
När du visar hello resursgrupp som du har skapat, Välj hello Nätverkssäkerhetsgruppen som är kopplade till den virtuella datorn och klicka på **inkommande säkerhetsregler** tooview hello regler som visas här.

![](media/portal-use-docker/AddingEndpoint.png)

Klicka på **+ Lägg till** tooadd en annan regel, och ange ett namn för hello slutpunkt i hello standardfallet (i det här exemplet **Docker**), och 2376 'Målportintervall'. Ange hello protokollet värdet visar **TCP**, och klicka på **OK** toocreate hello regeln.

![](media/portal-use-docker/AddEndpointFormFilledOut.png)

## <a name="test-your-docker-client-and-azure-docker-host"></a>Testa din Docker-klienten och Azure Docker-värden
Leta upp och kopiera hello namnet på den Virtuella datorns domän och kommandoraden hello från din klientdator, typen `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (där *dockerextension* ersätts av hello underdomänen för den virtuella datorn).

hello resultatet ska se ut ungefär toothis:

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

När du har slutfört hello senare steg nu har du en fullt fungerande Docker-värd som kör på en Azure-dator, toobe anslutna tooremotely från andra klienter.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg
Du är klar toogo toohello [Docker användarhandboken] och använda Docker-VM. Om du vill skapa Docker-värdar för virtuella datorer i Azure via kommandoradsgränssnittet tooautomate finns [hur toouse hello Docker VM-tillägget från hello Azure-kommandoradsgränssnittet (Azure CLI)]

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
