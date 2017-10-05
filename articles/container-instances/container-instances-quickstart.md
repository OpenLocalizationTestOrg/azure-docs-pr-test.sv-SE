---
title: "Skapa din första Azure Container Instances-behållare | Azure Docs"
description: "Distribuera och kom igång med Azure Container Instances"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 905f69e5e1e237a31d9bb1e326969ec83292c244
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a><span data-ttu-id="6c121-103">Skapa din första behållare i Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="6c121-103">Create your first container in Azure Container Instances</span></span>

<span data-ttu-id="6c121-104">Med Azure Container Instances är det enkelt att skapa och hantera behållare i Azure.</span><span class="sxs-lookup"><span data-stu-id="6c121-104">Azure Container Instances makes it easy to create and manage containers in Azure.</span></span> <span data-ttu-id="6c121-105">I den här snabbstartsguiden ska du skapa en behållare i Azure och göra den tillgänglig på Internet med en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="6c121-105">In this quick start, you will create a container in Azure and expose it to the internet with a public IP address.</span></span> <span data-ttu-id="6c121-106">Den här åtgärden utförs med ett enda kommando.</span><span class="sxs-lookup"><span data-stu-id="6c121-106">This operation is completed in a single command.</span></span> <span data-ttu-id="6c121-107">Inom några sekunder visas det här i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="6c121-107">Within just a few seconds, you will see this in your browser:</span></span>

![App som distribuerats via Azure Container Instances visas i webbläsare][aci-app-browser]

<span data-ttu-id="6c121-109">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="6c121-109">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="6c121-110">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0.12 eller senare.</span><span class="sxs-lookup"><span data-stu-id="6c121-110">If you choose to install and use the CLI locally, this quickstart requires that you are running the Azure CLI version 2.0.12 or later.</span></span> <span data-ttu-id="6c121-111">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="6c121-111">Run `az --version` to find the version.</span></span> <span data-ttu-id="6c121-112">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6c121-112">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="6c121-113">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="6c121-113">Create a resource group</span></span>

<span data-ttu-id="6c121-114">Azure Container Instances är Azure-resurser och måste vara placerade i en Azure-resursgrupp, en logisk samling dit Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="6c121-114">Azure Container Instances are Azure resources and must be placed in an Azure resource group, a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="6c121-115">Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="6c121-115">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="6c121-116">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* på platsen *eastus*.</span><span class="sxs-lookup"><span data-stu-id="6c121-116">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a><span data-ttu-id="6c121-117">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="6c121-117">Create a container</span></span>

<span data-ttu-id="6c121-118">Du kan skapa en behållare genom att ange ett namn, en Docker-avbildning och en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6c121-118">You can create a container by providing a name, a Docker image, and an Azure resource group.</span></span> <span data-ttu-id="6c121-119">Du kan också göra behållaren tillgänglig på Internet med en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="6c121-119">You can optionally expose the container to the internet with a public IP address.</span></span> <span data-ttu-id="6c121-120">I så fall använder vi en behållare som är värd för en väldigt enkel webbapp skriven i [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="6c121-120">In this case, we'll use a container that hosts a very simple web app written in [Node.js](http://nodejs.org).</span></span>

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public 
```

<span data-ttu-id="6c121-121">Om några sekunder bör du få ett svar på din begäran.</span><span class="sxs-lookup"><span data-stu-id="6c121-121">Within a few seconds, you should get a response to your request.</span></span> <span data-ttu-id="6c121-122">Först har behållaren statusen **Creating** (skapas) men bör starta inom några sekunder.</span><span class="sxs-lookup"><span data-stu-id="6c121-122">Initially, the container will be in a **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="6c121-123">Du kan kontrollera statusen med kommandot `show`:</span><span class="sxs-lookup"><span data-stu-id="6c121-123">You can check the status using the `show` command:</span></span>

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

<span data-ttu-id="6c121-124">Längst ned i resultatet visas behållarens etableringsstatus och IP-adress:</span><span class="sxs-lookup"><span data-stu-id="6c121-124">At the bottom of the output, you will see the container's provisioning state and its IP address:</span></span>

```json
...
"ipAddress": {
      "ip": "13.88.8.148",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

<span data-ttu-id="6c121-125">När behållaren övergår i status **Succeeded** (lyckades) kan du nå den via webbläsaren med den angivna IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="6c121-125">Once the container moves to the **Succeeded** state, you can reach it in the browser using the IP address provided.</span></span> 

![App som distribuerats via Azure Container Instances visas i webbläsare][aci-app-browser]

## <a name="pull-the-container-logs"></a><span data-ttu-id="6c121-127">Hämta behållarloggarna</span><span class="sxs-lookup"><span data-stu-id="6c121-127">Pull the container logs</span></span>

<span data-ttu-id="6c121-128">Du kan hämta loggarna för den behållare du skapat med kommandot `logs`:</span><span class="sxs-lookup"><span data-stu-id="6c121-128">You can pull the logs for the container you created using the `logs` command:</span></span>

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

<span data-ttu-id="6c121-129">Resultat:</span><span class="sxs-lookup"><span data-stu-id="6c121-129">Output:</span></span>

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-the-container"></a><span data-ttu-id="6c121-130">Ta bort behållaren</span><span class="sxs-lookup"><span data-stu-id="6c121-130">Delete the container</span></span>

<span data-ttu-id="6c121-131">När du är klar med behållaren kan du ta bort den med kommandot `delete`:</span><span class="sxs-lookup"><span data-stu-id="6c121-131">When you are done with the container, you can remove it using the `delete` command:</span></span>

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="6c121-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c121-132">Next steps</span></span>

<span data-ttu-id="6c121-133">All kod för behållaren som används i den här snabbstartsguiden finns [på GitHub][app-github-repo] tillsammans med dess Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="6c121-133">All of the code for the container used in this quick start is available [on GitHub][app-github-repo], along with its Dockerfile.</span></span> <span data-ttu-id="6c121-134">Om du vill försöka skapa den på egen hand och distribuera den till Azure Container Instances via Azure Container Registry, går du vidare till självstudierna för Azure Container Instances.</span><span class="sxs-lookup"><span data-stu-id="6c121-134">If you'd like to try building it yourself and deploying it to Azure Container Instances using the Azure Container Registry, continue to the Azure Container Instances tutorial.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6c121-135">Azure Container Instances-självstudier</span><span class="sxs-lookup"><span data-stu-id="6c121-135">Azure Container Instances tutorials</span></span>](./container-instances-tutorial-prepare-app.md)


<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png