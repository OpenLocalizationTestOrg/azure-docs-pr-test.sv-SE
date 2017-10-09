---
title: aaaUsing ACR med ett Azure DC/OS-kluster | Microsoft Docs
description: "Använda en Azure-behållare registret med ett DC/OS-kluster i Azure Container Service"
services: container-service
documentationcenter: 
author: julienstroheker
manager: dcaro
editor: 
tags: acs, azure-container-service, acr, azure-container-registry
keywords: "Docker, behållare, Micro-tjänster, Mesos, Azure, filresursen, cifs"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: 9a2802da788b50147d8b4259436bdcdb0c929db8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-acr-with-a-dcos-cluster-toodeploy-your-application"></a><span data-ttu-id="fca2d-104">Använda ACR med ett DC/OS-klustret toodeploy ditt program</span><span class="sxs-lookup"><span data-stu-id="fca2d-104">Use ACR with a DC/OS cluster toodeploy your application</span></span>

<span data-ttu-id="fca2d-105">I den här artikeln förklarar vi hur toouse registret för Azure-behållare med DC/OS-kluster.</span><span class="sxs-lookup"><span data-stu-id="fca2d-105">In this article, we explore how toouse Azure Container Registry with a DC/OS cluster.</span></span> <span data-ttu-id="fca2d-106">Med hjälp av ACR kan du tooprivately store och hantera behållare avbildningar.</span><span class="sxs-lookup"><span data-stu-id="fca2d-106">Using ACR allows you tooprivately store and manage container images.</span></span> <span data-ttu-id="fca2d-107">Den här kursen ingår hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="fca2d-107">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fca2d-108">Distribuera Azure-behållare registret (vid behov)</span><span class="sxs-lookup"><span data-stu-id="fca2d-108">Deploy Azure Container Registry (if needed)</span></span>
> * <span data-ttu-id="fca2d-109">Konfigurera ACR autentisering på en DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="fca2d-109">Configure ACR authentication on a DC/OS cluster</span></span>
> * <span data-ttu-id="fca2d-110">Överföra en bild toohello Azure Container registret</span><span class="sxs-lookup"><span data-stu-id="fca2d-110">Uploaded an image toohello Azure Container Registry</span></span>
> * <span data-ttu-id="fca2d-111">Kör en avbildning av behållare från hello Azure Container registret</span><span class="sxs-lookup"><span data-stu-id="fca2d-111">Run a container image from hello Azure Container Registry</span></span>

<span data-ttu-id="fca2d-112">Du behöver en ACS-DC /-OS klustret toocomplete hello stegen i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="fca2d-112">You need an ACS DC/OS cluster toocomplete hello steps in this tutorial.</span></span> <span data-ttu-id="fca2d-113">Om det behövs, [detta skriptexempel](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) kan skapa en åt dig.</span><span class="sxs-lookup"><span data-stu-id="fca2d-113">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="fca2d-114">Den här kursen kräver hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="fca2d-114">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="fca2d-115">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="fca2d-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="fca2d-116">Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fca2d-116">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-azure-container-registry"></a><span data-ttu-id="fca2d-117">Distribuera Azure-behållaren registret</span><span class="sxs-lookup"><span data-stu-id="fca2d-117">Deploy Azure Container Registry</span></span>

<span data-ttu-id="fca2d-118">Om det behövs, skapa ett Azure-behållare registret med hello [az acr skapa](/cli/azure/acr#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="fca2d-118">If needed, create an Azure Container registry with hello [az acr create](/cli/azure/acr#create) command.</span></span> 

<span data-ttu-id="fca2d-119">hello följande exempel skapas ett register med ett slumpmässigt Generera namn.</span><span class="sxs-lookup"><span data-stu-id="fca2d-119">hello following example creates a registry with a randomly generate name.</span></span> <span data-ttu-id="fca2d-120">hello registret konfigureras också med ett administratörskonto med hello `--admin-enabled` argumentet.</span><span class="sxs-lookup"><span data-stu-id="fca2d-120">hello registry is also configured with an admin account using hello `--admin-enabled` argument.</span></span>

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry$RANDOM --sku Basic --admin-enabled true
```

<span data-ttu-id="fca2d-121">När du har skapat hello registret hello Azure CLI matar ut data liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="fca2d-121">Once hello registry has been created, hello Azure CLI outputs data similar toohello following.</span></span> <span data-ttu-id="fca2d-122">Anteckna hello `name` och `loginServer`, dessa används i senare steg.</span><span class="sxs-lookup"><span data-stu-id="fca2d-122">Take note of hello `name` and `loginServer`, these are used in later steps.</span></span>

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T03:40:56.511597+00:00",
  "id": "/subscriptions/f2799821-a08a-434e-9128-454ec4348b10/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry23489",
  "location": "eastus",
  "loginServer": "mycontainerregistry23489.azurecr.io",
  "name": "myContainerRegistry23489",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "mycontainerregistr034017"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

<span data-ttu-id="fca2d-123">Hämta hello behållaren registret autentiseringsuppgifter med hjälp av hello [az acr autentiseringsuppgifter visa](/cli/azure/acr/credential) kommando.</span><span class="sxs-lookup"><span data-stu-id="fca2d-123">Get hello container registry credentials using hello [az acr credential show](/cli/azure/acr/credential) command.</span></span> <span data-ttu-id="fca2d-124">Ersätt hello `--name` med hello en anges i hello sista steget.</span><span class="sxs-lookup"><span data-stu-id="fca2d-124">Substitute hello `--name` with hello one noted in hello last step.</span></span> <span data-ttu-id="fca2d-125">Ta del av lösenord, det behövs i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="fca2d-125">Take note of one password, it is needed in a later step.</span></span>

```azurecli-interactive
az acr credential show --name myContainerRegistry23489
```

<span data-ttu-id="fca2d-126">Läs mer om Azure-behållare registret [introduktion tooprivate Docker behållare register](../../container-registry/container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="fca2d-126">For more information on Azure Container Registry, see [Introduction tooprivate Docker container registries](../../container-registry/container-registry-intro.md).</span></span> 

## <a name="manage-acr-authentication"></a><span data-ttu-id="fca2d-127">Hantera ACR-autentisering</span><span class="sxs-lookup"><span data-stu-id="fca2d-127">Manage ACR authentication</span></span>

<span data-ttu-id="fca2d-128">hello konventionellt sätt toopush och pull-avbildning från en privat registret är toofirst autentisera med hello registret.</span><span class="sxs-lookup"><span data-stu-id="fca2d-128">hello conventional way toopush and pull image from a private registry is toofirst authenticate with hello registry.</span></span> <span data-ttu-id="fca2d-129">toodo så använder du hello `docker login` på alla klienter som behöver tooaccess hello privata registret.</span><span class="sxs-lookup"><span data-stu-id="fca2d-129">toodo so, you would use hello `docker login` command on any client that needs tooaccess hello private registry.</span></span> <span data-ttu-id="fca2d-130">Eftersom ett DC/OS-kluster kan innehålla flera noder, alla måste autentiseras med hello ACR toobe är användbara tooautomate processen på varje nod.</span><span class="sxs-lookup"><span data-stu-id="fca2d-130">Because a DC/OS cluster can contain many nodes, all of which need toobe authenticated with hello ACR, it is helpful tooautomate this process across each node.</span></span> 

### <a name="create-shared-storage"></a><span data-ttu-id="fca2d-131">Skapa delad lagring</span><span class="sxs-lookup"><span data-stu-id="fca2d-131">Create shared storage</span></span>

<span data-ttu-id="fca2d-132">Den här processen använder en Azure-filresurs som har monterats på varje nod i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="fca2d-132">This process uses an Azure file share that has been mounted on each node in hello cluster.</span></span> <span data-ttu-id="fca2d-133">Om du inte redan har installerat delad lagring, se [konfigurera en filresurs i ett DC/OS-kluster](container-service-dcos-fileshare.md).</span><span class="sxs-lookup"><span data-stu-id="fca2d-133">If you have not already set up shared storage, see [Setup a file share inside a DC/OS cluster](container-service-dcos-fileshare.md).</span></span>

### <a name="configure-acr-authentication"></a><span data-ttu-id="fca2d-134">Konfigurera ACR-autentisering</span><span class="sxs-lookup"><span data-stu-id="fca2d-134">Configure ACR authentication</span></span>

<span data-ttu-id="fca2d-135">Först hämta hello FQDN för hello DC/OS master och lagrar den i en variabel.</span><span class="sxs-lookup"><span data-stu-id="fca2d-135">First, get hello FQDN of hello DC/OS master and store it in a variable.</span></span>

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

<span data-ttu-id="fca2d-136">Skapa en SSH-anslutning med hello master (eller hello första master) på ditt DC/OS-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="fca2d-136">Create an SSH connection with hello master (or hello first master) of your DC/OS-based cluster.</span></span> <span data-ttu-id="fca2d-137">Uppdatera hello användarnamn om ett standardvärde används när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="fca2d-137">Update hello user name if a non-default value was used when creating hello cluster.</span></span>

```azurecli-interactive
ssh azureuser@$FQDN
```

<span data-ttu-id="fca2d-138">Hello kör följande kommando toologin toohello Azure Container registret.</span><span class="sxs-lookup"><span data-stu-id="fca2d-138">Run hello following command toologin toohello Azure Container Registry.</span></span> <span data-ttu-id="fca2d-139">Ersätt hello `--username` med hello namnet hello behållaren registret och hello `--password` med något av hello som lösenord.</span><span class="sxs-lookup"><span data-stu-id="fca2d-139">Replace hello `--username` with hello name of hello container registry, and hello `--password` with one of hello provided passwords.</span></span> <span data-ttu-id="fca2d-140">Ersätt hello sista argumentet *mycontainerregistry.azurecr.io* i hello exempel med hello loginServer namn på hello behållaren register.</span><span class="sxs-lookup"><span data-stu-id="fca2d-140">Replace hello last argument *mycontainerregistry.azurecr.io* in hello example with hello loginServer name of hello container registry.</span></span> 

<span data-ttu-id="fca2d-141">Det här kommandot lagrar hello autentisering värden lokalt under hello `~/.docker` sökväg.</span><span class="sxs-lookup"><span data-stu-id="fca2d-141">This command stores hello authentication values locally under hello `~/.docker` path.</span></span>

```azurecli-interactive
docker -H tcp://localhost:2375 login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry.azurecr.io
```

<span data-ttu-id="fca2d-142">Skapa en komprimerad fil som innehåller hello behållaren registervärden för autentisering.</span><span class="sxs-lookup"><span data-stu-id="fca2d-142">Create a compressed file that contains hello container registry authentication values.</span></span>

```azurecli-interactive
tar czf docker.tar.gz .docker
```

<span data-ttu-id="fca2d-143">Kopiera filen toohello klusterdelade lagringen.</span><span class="sxs-lookup"><span data-stu-id="fca2d-143">Copy this file toohello cluster shared storage.</span></span> <span data-ttu-id="fca2d-144">Det här steget blir hello tillgänglig på alla noder i hello DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="fca2d-144">This step makes hello file available on all nodes of hello DC/OS cluster.</span></span>

```azurecli-interactive
cp docker.tar.gz /mnt/share/dcosshare
```

## <a name="upload-image-tooacr"></a><span data-ttu-id="fca2d-145">Överför avbildningen tooACR</span><span class="sxs-lookup"><span data-stu-id="fca2d-145">Upload image tooACR</span></span>

<span data-ttu-id="fca2d-146">Från en utvecklingsdator eller andra system med Docker installerat kan nu skapa en avbildning och överför den toohello Azure Container registret.</span><span class="sxs-lookup"><span data-stu-id="fca2d-146">Now from a development machine, or any other system with Docker installed, create an image and upload it toohello Azure Container Registry.</span></span>

<span data-ttu-id="fca2d-147">Skapa en behållare från hello Ubuntu avbildning.</span><span class="sxs-lookup"><span data-stu-id="fca2d-147">Create a container from hello Ubuntu image.</span></span>

```azurecli-interactive
docker run ubunut --name base-image
```

<span data-ttu-id="fca2d-148">Samla in hello behållare till en ny avbildning.</span><span class="sxs-lookup"><span data-stu-id="fca2d-148">Now capture hello container into a new image.</span></span> <span data-ttu-id="fca2d-149">hello avbildningsnamn måste tooinclude hello `loginServer` namn på hello behållaren registrywith ett format för `loginServer/imageName`.</span><span class="sxs-lookup"><span data-stu-id="fca2d-149">hello image name needs tooinclude hello `loginServer` name of hello container registrywith a format of `loginServer/imageName`.</span></span>

```azurecli-interactive
docker -H tcp://localhost:2375 commit base-image mycontainerregistry30678.azurecr.io/dcos-demo
````

<span data-ttu-id="fca2d-150">Logga in på hello Azure Container registret.</span><span class="sxs-lookup"><span data-stu-id="fca2d-150">Login into hello Azure Container Registry.</span></span> <span data-ttu-id="fca2d-151">Ersätt hello namn med hello loginServer namn, hello--användarnamn med hello namnet hello behållaren registret och hello--lösenord med en hello som lösenord.</span><span class="sxs-lookup"><span data-stu-id="fca2d-151">Replace hello name with hello loginServer name, hello --username with hello name of hello container registry, and hello --password with one of hello provided passwords.</span></span>

```azurecli-interactive
docker login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry2675.azurecr.io
```

<span data-ttu-id="fca2d-152">Slutligen överför hello avbildningen toohello ACR registret.</span><span class="sxs-lookup"><span data-stu-id="fca2d-152">Finally, upload hello image toohello ACR registry.</span></span> <span data-ttu-id="fca2d-153">Det här exemplet överför en bild med namnet DC/OS-demonstrationen.</span><span class="sxs-lookup"><span data-stu-id="fca2d-153">This example uploads an image named dcos-demo.</span></span>

```azurecli-interactive
docker push mycontainerregistry30678.azurecr.io/dcos-demo
```

## <a name="run-an-image-from-acr"></a><span data-ttu-id="fca2d-154">Kör en bild från ACR</span><span class="sxs-lookup"><span data-stu-id="fca2d-154">Run an image from ACR</span></span>

<span data-ttu-id="fca2d-155">toouse en bild från hello ACR registret, skapa en fil namn *acrDemo.json* och kopiera hello följande text i den.</span><span class="sxs-lookup"><span data-stu-id="fca2d-155">toouse an image from hello ACR registry, create a file names *acrDemo.json* and copy hello following text into it.</span></span> <span data-ttu-id="fca2d-156">Ersätt hello avbildningsnamn med hello behållarnamn registret loginServer och namn, till exempel `loginServer/imageName`.</span><span class="sxs-lookup"><span data-stu-id="fca2d-156">Replace hello image name with hello container registry loginServer name and image name, for example `loginServer/imageName`.</span></span> <span data-ttu-id="fca2d-157">Anteckna hello `uris` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="fca2d-157">Take note of hello `uris` property.</span></span> <span data-ttu-id="fca2d-158">Den här egenskapen innehåller hello plats hello behållaren autentisering registerfilen, som i det här fallet är hello Azure-filresursen som är monterade på varje nod i hello DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="fca2d-158">This property holds hello location of hello container registry authentication file, which in this case is hello Azure file share that is mounted on each node in hello DC/OS cluster.</span></span>

```json
{
  "id": "myapp",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "mycontainerregistry30678.azurecr.io/dcos-demo",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "uris":  [
       "file:///mnt/share/dcosshare/docker.tar.gz"
   ]
}
```

<span data-ttu-id="fca2d-159">Distribuera programmet hello med hello DC/OC CLI.</span><span class="sxs-lookup"><span data-stu-id="fca2d-159">Deploy hello application with hello DC/OC CLI.</span></span>

```azurecli-interactive
dcos marathon app add acrDemo.json
```

## <a name="next-steps"></a><span data-ttu-id="fca2d-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fca2d-160">Next steps</span></span>

<span data-ttu-id="fca2d-161">I den här kursen har du konfigurera DC/OS toouse Azure Container registret inklusive hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="fca2d-161">In this tutorial you have configure DC/OS toouse Azure Container Registry including hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fca2d-162">Distribuera Azure-behållare registret (vid behov)</span><span class="sxs-lookup"><span data-stu-id="fca2d-162">Deploy Azure Container Registry (if needed)</span></span>
> * <span data-ttu-id="fca2d-163">Konfigurera ACR autentisering på en DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="fca2d-163">Configure ACR authentication on a DC/OS cluster</span></span>
> * <span data-ttu-id="fca2d-164">Överföra en bild toohello Azure Container registret</span><span class="sxs-lookup"><span data-stu-id="fca2d-164">Uploaded an image toohello Azure Container Registry</span></span>
> * <span data-ttu-id="fca2d-165">Kör en avbildning av behållare från hello Azure Container registret</span><span class="sxs-lookup"><span data-stu-id="fca2d-165">Run a container image from hello Azure Container Registry</span></span>
