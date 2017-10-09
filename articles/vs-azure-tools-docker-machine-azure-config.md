---
title: "aaaCreate Docker-värdar i Azure med Docker-dator | Microsoft Docs"
description: "Beskriver användningen av Docker datorn toocreate docker-värdar i Azure."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 7a3ff6e1-fa93-4a62-b524-ab182d2fea08
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: fbf67e8189bbf33f874c4a9b619a931f28ccee12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Skapa Docker-värdar i Azure med Docker-dator
Kör [Docker](https://www.docker.com/) behållare kräver en värd VM körs hello docker daemon.
Det här avsnittet beskrivs hur toouse hello [docker-datorn](https://docs.docker.com/machine/) kommandot toocreate nya Linux virtuella datorer som konfigurerats med hello Docker-daemon körs i Azure. 

**Obs!** 

* *Den här artikeln beror på docker-datorn version 0.9.0-rc2 eller större*
* *Windows-behållare kommer att stödjas via docker-dator i hello nära framtiden*

## <a name="create-vms-with-docker-machine"></a>Skapa virtuella datorer med Docker-dator
Skapa docker värden virtuella datorer i Azure med hello `docker-machine create` kommandot med hello `azure` drivrutin. 

hello Azure drivrutinen kräver ditt prenumerations-ID. Du kan använda hello [Azure CLI](cli-install-nodejs.md) eller hello [Azure Portal](https://portal.azure.com) tooretrieve din Azure-prenumeration. 

**Med hjälp av hello Azure-portalen**

* Välj **prenumerationer** från hello navigeringen till vänster sida och kopiera hello prenumerations-id.

**Med hjälp av hello Azure CLI**

* Typen ```azure account list``` och kopiera hello prenumerations-id.

Typen `docker-machine create --driver azure` toosee hello alternativ och standardvärdena.
Du kan också se hello [Docker Azure Driver-dokumentationen](https://docs.docker.com/machine/drivers/azure/) för mer information. 

hello följande exempel använder hello [standardvärdena](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), men den om du vill ange dessa värden: 

* Azure-dns för hello namnet som associeras med hello offentlig IP-adress och certifikat som genereras. Detta är hello DNS-namnet på den virtuella datorn. hello VM kan sedan på ett säkert sätt stoppa, släpper hello dynamisk IP och ange hello möjlighet tooreconnect när hello vm börjar igen med en ny IP-adress. hello prefixet måste vara unikt för regionen UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.
* Öppna port 80 på hello VM för utgående Internetåtkomst
* storleken på hello VM tooutilize snabbare premium-lagring
* Premium-lagring som används för hello virtuell disk

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Välj en docker-värd med docker-dator
När du har en post i docker-datorn för värden kan du ange standardvärden för hello när du kör docker-kommandon.

## <a name="using-powershell"></a>Använda PowerShell
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a>Med hjälp av Bash
```bash
eval $(docker-machine env MyDockerHost)
```

Du kan nu köra docker kommandon mot hello angivna värden

```
docker ps
docker info
```

## <a name="run-a-container"></a>Kör en behållare
Med en värd som har konfigurerats och kan du nu köra en enkel web server tootest om värden har konfigurerats korrekt.
Här vi använder en standard nginx-avbildning, ange att den ska lyssna på port 80 och om hello värden VM har startats om kommer hello behållare startar om samt (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

hello utdata ska se ut ungefär hello följande:

```
Unable toofind image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-hello-container"></a>Testa hello behållare
Granska körs behållare med `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

Och toosee hello kör behållare, typen `docker-machine ip <VM name>` toofind hello IP-adress tooenter i hello webbläsare:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Körs ngnix behållare](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a>Sammanfattning
Du kan enkelt etablera docker-värdar i Azure för din enskilda docker värden verifieringar med docker-datorn.
Produktion av behållare finns hello [Azure Container Service](http://aka.ms/AzureContainerService)

toodevelop .NET Core-program med Visual Studio finns [Docker Tools för Visual Studio](http://aka.ms/DockerToolsForVS)

