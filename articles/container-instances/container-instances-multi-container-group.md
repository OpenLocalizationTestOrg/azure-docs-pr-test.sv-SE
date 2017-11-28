---
title: "aaaAzure Behållarinstanser - flera behållargruppen | Azure-dokument"
description: "Azure Behållarinstanser - grupp med flera behållare"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 976f578cd2a9bf7f05ab97f24662139bb72062ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-container-group"></a><span data-ttu-id="1da7f-103">Distribuera en behållare grupp</span><span class="sxs-lookup"><span data-stu-id="1da7f-103">Deploy a container group</span></span>

<span data-ttu-id="1da7f-104">Stöd för Azure Behållarinstanser hello distribution av flera behållare till en enda värd med en *behållargruppen*.</span><span class="sxs-lookup"><span data-stu-id="1da7f-104">Azure Container Instances support hello deployment of multiple containers onto a single host using a *container group*.</span></span> <span data-ttu-id="1da7f-105">Detta är användbart när du skapar ett program sidovagn för loggning, övervakning eller någon annan konfiguration där en tjänst behöver en andra anslutna process.</span><span class="sxs-lookup"><span data-stu-id="1da7f-105">This is useful when building an application sidecar for logging, monitoring, or any other configuration where a service needs a second attached process.</span></span> 

<span data-ttu-id="1da7f-106">Det här dokumentet går igenom kör en enkel flera behållare sidovagn konfiguration med en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="1da7f-106">This document walks through running a simple multi-container sidecar configuration using an Azure Resource Manager template.</span></span>

## <a name="configure-hello-template"></a><span data-ttu-id="1da7f-107">Konfigurera hello mall</span><span class="sxs-lookup"><span data-stu-id="1da7f-107">Configure hello template</span></span>

<span data-ttu-id="1da7f-108">Skapa en fil med namnet `azuredeploy.json` och kopiera hello följande json till den.</span><span class="sxs-lookup"><span data-stu-id="1da7f-108">Create a file named `azuredeploy.json` and copy hello following json into it.</span></span> 

<span data-ttu-id="1da7f-109">I det här exemplet definieras en behållare grupp med två behållare och en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="1da7f-109">In this sample, a container group with two containers and a public IP address is defined.</span></span> <span data-ttu-id="1da7f-110">hello första behållare för hello grupp körs ett Internetriktade Internetprogram.</span><span class="sxs-lookup"><span data-stu-id="1da7f-110">hello first container of hello group runs an internet facing application.</span></span> <span data-ttu-id="1da7f-111">hello andra behållare, hello sidovagn gör ett HTTP-begäran toohello huvudsakliga webbprogram via hello gruppen lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="1da7f-111">hello second container, hello sidecar, makes an HTTP request toohello main web application via hello group's local network.</span></span> 

<span data-ttu-id="1da7f-112">Det här exemplet sidovagn kan vara expanderade tootrigger en avisering om det tog emot en HTTP-svarskoden än 200 OK.</span><span class="sxs-lookup"><span data-stu-id="1da7f-112">This sidecar example could be expanded tootrigger an alert if it received an HTTP response code other than 200 OK.</span></span> 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "microsoft/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",    
    "container2image": "microsoft/aci-tutorial-sidecar"
  },
    "resources": [
      {
        "name": "myContainerGroup",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2017-08-01-preview",
        "location": "[resourceGroup().location]",
        "properties": {
          "containers": [
            {
              "name": "[variables('container1name')]",
              "properties": {
                "image": "[variables('container1image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                },
                "ports": [
                  {
                    "port": 80
                  }
                ]
              }
            },
            {
              "name": "[variables('container2name')]",
              "properties": {
                "image": "[variables('container2image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                }
              }
            }
          ],
          "osType": "Linux",
          "ipAddress": {
            "type": "Public",
            "ports": [
              {
                "protocol": "tcp",
                "port": "80"
              }
            ]
          }
        }
      }
    ],
    "outputs": {
      "containerIPv4Address": {
        "type": "string",
        "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', 'myContainerGroup')).ipAddress.ip]"
      }
    }
  }
```

<span data-ttu-id="1da7f-113">toouse en bild registret privata behållare lägga till ett objekt toohello json-dokument med hello följande format.</span><span class="sxs-lookup"><span data-stu-id="1da7f-113">toouse a private container image registry, add an object toohello json document with hello following format.</span></span>

```json
"imageRegistryCredentials": [
    {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
    }
]
```

## <a name="deploy-hello-template"></a><span data-ttu-id="1da7f-114">Distribuera hello mall</span><span class="sxs-lookup"><span data-stu-id="1da7f-114">Deploy hello template</span></span>

<span data-ttu-id="1da7f-115">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="1da7f-115">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="1da7f-116">Distribuera hello mallen med hello [az distribution skapa](/cli/azure/group/deployment#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="1da7f-116">Deploy hello template with hello [az group deployment create](/cli/azure/group/deployment#create) command.</span></span>

```azurecli-interactive
az group deployment create --name myContainerGroup --resource-group myResourceGroup --template-file azuredeploy.json
```

<span data-ttu-id="1da7f-117">Inom några sekunder får du ett första svar från Azure.</span><span class="sxs-lookup"><span data-stu-id="1da7f-117">Within a few seconds, you will receive an initial response from Azure.</span></span> 

## <a name="view-deployment-state"></a><span data-ttu-id="1da7f-118">Visa status för distributionen</span><span class="sxs-lookup"><span data-stu-id="1da7f-118">View deployment state</span></span>

<span data-ttu-id="1da7f-119">tooview hello tillstånd hello distribution, Använd hello `az container show` kommando.</span><span class="sxs-lookup"><span data-stu-id="1da7f-119">tooview hello state of hello deployment, use hello `az container show` command.</span></span> <span data-ttu-id="1da7f-120">Detta returnerar hello etablerats offentliga IP-adressen via vilken hello program kan nås.</span><span class="sxs-lookup"><span data-stu-id="1da7f-120">This returns hello provisioned public IP address over which hello application can be accessed.</span></span>

```azurecli-interactive
az container show --name myContainerGroup --resource-group myResourceGroup -o table
```

<span data-ttu-id="1da7f-121">Resultat:</span><span class="sxs-lookup"><span data-stu-id="1da7f-121">Output:</span></span>

```azurecli
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGrou2  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a><span data-ttu-id="1da7f-122">Visa loggfiler</span><span class="sxs-lookup"><span data-stu-id="1da7f-122">View logs</span></span>   

<span data-ttu-id="1da7f-123">Visa hello loggutdata för en behållare med hello `az container logs` kommando.</span><span class="sxs-lookup"><span data-stu-id="1da7f-123">View hello log output of a container using hello `az container logs` command.</span></span> <span data-ttu-id="1da7f-124">Hej `--container-name` argumentet anger hello behållare från vilken toopull loggar.</span><span class="sxs-lookup"><span data-stu-id="1da7f-124">hello `--container-name` argument specifies hello container from which toopull logs.</span></span> <span data-ttu-id="1da7f-125">I det här exemplet har hello första behållare angetts.</span><span class="sxs-lookup"><span data-stu-id="1da7f-125">In this example, hello first container is specified.</span></span> 

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-app --resource-group myResourceGroup
```

<span data-ttu-id="1da7f-126">Resultat:</span><span class="sxs-lookup"><span data-stu-id="1da7f-126">Output:</span></span>

```bash
istening on port 80
::1 - - [27/Jul/2017:17:35:29 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:32 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:35 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:38 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

<span data-ttu-id="1da7f-127">toosee hello loggar för hello sida bil behållare, kör hello samma kommando anger hello andra behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="1da7f-127">toosee hello logs for hello side-car container, run hello same command specifying hello second container name.</span></span>

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-sidecar --resource-group myResourceGroup
```

<span data-ttu-id="1da7f-128">Resultat:</span><span class="sxs-lookup"><span data-stu-id="1da7f-128">Output:</span></span>

```bash
Every 3.0s: curl -I http://localhost                                                                                                                       Mon Jul 17 11:27:36 2017

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 1663
Content-Type: text/html; charset=utf-8
Last-Modified: Sun, 16 Jul 2017 02:08:22 GMT
Date: Mon, 17 Jul 2017 18:27:36 GMT
```

<span data-ttu-id="1da7f-129">Som du ser att hello sidovagn regelbundet en HTTP-begäran toohello huvudsakliga webbprogram via hello gruppen lokala nätverket tooensure körs.</span><span class="sxs-lookup"><span data-stu-id="1da7f-129">As you can see, hello sidecar is periodically making an HTTP request toohello main web application via hello group's local network tooensure that it is running.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1da7f-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1da7f-130">Next steps</span></span>

<span data-ttu-id="1da7f-131">Det här dokumentet beskrivs hello steg som krävs för att distribuera flera behållare instans av Azure-behållaren.</span><span class="sxs-lookup"><span data-stu-id="1da7f-131">This document covered hello steps needed for deploying a multi-container Azure container instance.</span></span> <span data-ttu-id="1da7f-132">En utgången tooend Azure Behållarinstanser upplevelse, finns i hello Azure Behållarinstanser kursen.</span><span class="sxs-lookup"><span data-stu-id="1da7f-132">For an end tooend Azure Container Instances experience, see hello Azure Container Instances tutorial.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="1da7f-133">[Azure Behållarinstanser kursen]:./container-instances-tutorial-prepare-app.md</span><span class="sxs-lookup"><span data-stu-id="1da7f-133">[Azure Container Instances tutorial]: ./container-instances-tutorial-prepare-app.md</span></span>
