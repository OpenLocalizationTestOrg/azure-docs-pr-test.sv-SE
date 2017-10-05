---
title: "Azure Behållarinstanser - flera behållargruppen | Azure-dokument"
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
ms.openlocfilehash: 140f58582645ea32f77e901eb13364ed145bbecf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-container-group"></a><span data-ttu-id="a9ba1-103">Distribuera en behållare grupp</span><span class="sxs-lookup"><span data-stu-id="a9ba1-103">Deploy a container group</span></span>

<span data-ttu-id="a9ba1-104">Azure Behållarinstanser stöd för distribution av flera behållare till en enda värd med en *behållargruppen*.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-104">Azure Container Instances support the deployment of multiple containers onto a single host using a *container group*.</span></span> <span data-ttu-id="a9ba1-105">Detta är användbart när du skapar ett program sidovagn för loggning, övervakning eller någon annan konfiguration där en tjänst behöver en andra anslutna process.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-105">This is useful when building an application sidecar for logging, monitoring, or any other configuration where a service needs a second attached process.</span></span> 

<span data-ttu-id="a9ba1-106">Det här dokumentet går igenom kör en enkel flera behållare sidovagn konfiguration med en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-106">This document walks through running a simple multi-container sidecar configuration using an Azure Resource Manager template.</span></span>

## <a name="configure-the-template"></a><span data-ttu-id="a9ba1-107">Konfigurera mallen</span><span class="sxs-lookup"><span data-stu-id="a9ba1-107">Configure the template</span></span>

<span data-ttu-id="a9ba1-108">Skapa en fil med namnet `azuredeploy.json` och kopierar följande json till den.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-108">Create a file named `azuredeploy.json` and copy the following json into it.</span></span> 

<span data-ttu-id="a9ba1-109">I det här exemplet definieras en behållare grupp med två behållare och en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-109">In this sample, a container group with two containers and a public IP address is defined.</span></span> <span data-ttu-id="a9ba1-110">Första behållare för gruppen kör ett Internetriktade Internetprogram.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-110">The first container of the group runs an internet facing application.</span></span> <span data-ttu-id="a9ba1-111">Behållaren andra sidvagn, gör en HTTP-begäran till webbprogrammet huvudsakliga via gruppens lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-111">The second container, the sidecar, makes an HTTP request to the main web application via the group's local network.</span></span> 

<span data-ttu-id="a9ba1-112">Det här exemplet sidovagn kan expanderas för att utlösa en avisering om det tog emot en HTTP-svarskoden än 200 OK.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-112">This sidecar example could be expanded to trigger an alert if it received an HTTP response code other than 200 OK.</span></span> 

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

<span data-ttu-id="a9ba1-113">Om du vill använda ett privat behållaren image register att lägga till ett objekt i json-dokumentet med följande format.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-113">To use a private container image registry, add an object to the json document with the following format.</span></span>

```json
"imageRegistryCredentials": [
    {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
    }
]
```

## <a name="deploy-the-template"></a><span data-ttu-id="a9ba1-114">Distribuera mallen</span><span class="sxs-lookup"><span data-stu-id="a9ba1-114">Deploy the template</span></span>

<span data-ttu-id="a9ba1-115">Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="a9ba1-115">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="a9ba1-116">Distribuera mallen med den [az distribution skapa](/cli/azure/group/deployment#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-116">Deploy the template with the [az group deployment create](/cli/azure/group/deployment#create) command.</span></span>

```azurecli-interactive
az group deployment create --name myContainerGroup --resource-group myResourceGroup --template-file azuredeploy.json
```

<span data-ttu-id="a9ba1-117">Inom några sekunder får du ett första svar från Azure.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-117">Within a few seconds, you will receive an initial response from Azure.</span></span> 

## <a name="view-deployment-state"></a><span data-ttu-id="a9ba1-118">Visa status för distributionen</span><span class="sxs-lookup"><span data-stu-id="a9ba1-118">View deployment state</span></span>

<span data-ttu-id="a9ba1-119">Du kan visa statusen för distributionen av `az container show` kommando.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-119">To view the state of the deployment, use the `az container show` command.</span></span> <span data-ttu-id="a9ba1-120">Detta returnerar etablerade offentliga IP-adressen som programmet kan användas.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-120">This returns the provisioned public IP address over which the application can be accessed.</span></span>

```azurecli-interactive
az container show --name myContainerGroup --resource-group myResourceGroup -o table
```

<span data-ttu-id="a9ba1-121">Resultat:</span><span class="sxs-lookup"><span data-stu-id="a9ba1-121">Output:</span></span>

```azurecli
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGrou2  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a><span data-ttu-id="a9ba1-122">Visa loggfiler</span><span class="sxs-lookup"><span data-stu-id="a9ba1-122">View logs</span></span>   

<span data-ttu-id="a9ba1-123">Visa loggutdata från en behållare med hjälp av den `az container logs` kommando.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-123">View the log output of a container using the `az container logs` command.</span></span> <span data-ttu-id="a9ba1-124">Den `--container-name` argumentet anger behållaren som hämtar loggarna.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-124">The `--container-name` argument specifies the container from which to pull logs.</span></span> <span data-ttu-id="a9ba1-125">Den första behållaren har angetts i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-125">In this example, the first container is specified.</span></span> 

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-app --resource-group myResourceGroup
```

<span data-ttu-id="a9ba1-126">Resultat:</span><span class="sxs-lookup"><span data-stu-id="a9ba1-126">Output:</span></span>

```bash
istening on port 80
::1 - - [27/Jul/2017:17:35:29 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:32 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:35 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:38 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

<span data-ttu-id="a9ba1-127">Om du vill se loggar för behållaren sida bilen köra samma kommando anger andra behållarens namn.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-127">To see the logs for the side-car container, run the same command specifying the second container name.</span></span>

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-sidecar --resource-group myResourceGroup
```

<span data-ttu-id="a9ba1-128">Resultat:</span><span class="sxs-lookup"><span data-stu-id="a9ba1-128">Output:</span></span>

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

<span data-ttu-id="a9ba1-129">Som du kan se göra sidovagn regelbundet en HTTP-begäran till huvudsakliga webb-applikationen via gruppens lokala nätverk så att den körs.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-129">As you can see, the sidecar is periodically making an HTTP request to the main web application via the group's local network to ensure that it is running.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9ba1-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9ba1-130">Next steps</span></span>

<span data-ttu-id="a9ba1-131">Det här dokumentet beskrivs de steg som krävs för att distribuera en instans av flera behållare Azure-behållaren.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-131">This document covered the steps needed for deploying a multi-container Azure container instance.</span></span> <span data-ttu-id="a9ba1-132">En slutpunkt till slutpunkt Azure Behållarinstanser upplevelse finns i Azure Container instanser kursen.</span><span class="sxs-lookup"><span data-stu-id="a9ba1-132">For an end to end Azure Container Instances experience, see the Azure Container Instances tutorial.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="a9ba1-133">[Azure Behållarinstanser kursen]:./container-instances-tutorial-prepare-app.md</span><span class="sxs-lookup"><span data-stu-id="a9ba1-133">[Azure Container Instances tutorial]: ./container-instances-tutorial-prepare-app.md</span></span>
