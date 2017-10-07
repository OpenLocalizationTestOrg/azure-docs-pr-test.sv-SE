---
title: "aaaCreate din första Azure Behållarinstanser behållare | Azure-dokument"
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
ms.openlocfilehash: b57c52714933bd3b28c44d33f9af7cd1f23fb951
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a><span data-ttu-id="e84a4-103">Skapa din första behållare i Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="e84a4-103">Create your first container in Azure Container Instances</span></span>

<span data-ttu-id="e84a4-104">Azure Behållarinstanser som gör det enkelt toocreate och hantera behållare i Azure.</span><span class="sxs-lookup"><span data-stu-id="e84a4-104">Azure Container Instances makes it easy toocreate and manage containers in Azure.</span></span> <span data-ttu-id="e84a4-105">I den här snabbstartsguide du skapa en behållare i Azure och exponera den toohello internet med en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e84a4-105">In this quick start, you will create a container in Azure and expose it toohello internet with a public IP address.</span></span> <span data-ttu-id="e84a4-106">Den här åtgärden utförs med ett enda kommando.</span><span class="sxs-lookup"><span data-stu-id="e84a4-106">This operation is completed in a single command.</span></span> <span data-ttu-id="e84a4-107">Inom några sekunder visas det här i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="e84a4-107">Within just a few seconds, you will see this in your browser:</span></span>

![App som distribuerats via Azure Container Instances visas i webbläsare][aci-app-browser]

<span data-ttu-id="e84a4-109">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="e84a4-109">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="e84a4-110">Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.12 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e84a4-110">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.12 or later.</span></span> <span data-ttu-id="e84a4-111">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="e84a4-111">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="e84a4-112">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e84a4-112">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="e84a4-113">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e84a4-113">Create a resource group</span></span>

<span data-ttu-id="e84a4-114">Azure Container Instances är Azure-resurser och måste vara placerade i en Azure-resursgrupp, en logisk samling dit Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="e84a4-114">Azure Container Instances are Azure resources and must be placed in an Azure resource group, a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="e84a4-115">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="e84a4-115">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="e84a4-116">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.</span><span class="sxs-lookup"><span data-stu-id="e84a4-116">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a><span data-ttu-id="e84a4-117">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="e84a4-117">Create a container</span></span>

<span data-ttu-id="e84a4-118">Du kan skapa en behållare genom att ange ett namn, en Docker-avbildning och en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="e84a4-118">You can create a container by providing a name, a Docker image, and an Azure resource group.</span></span> <span data-ttu-id="e84a4-119">Du kan eventuellt exponera hello behållaren toohello internet med en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e84a4-119">You can optionally expose hello container toohello internet with a public IP address.</span></span> <span data-ttu-id="e84a4-120">I så fall använder vi en behållare som är värd för en väldigt enkel webbapp skriven i [Node.js](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="e84a4-120">In this case, we'll use a container that hosts a very simple web app written in [Node.js](http://nodejs.org).</span></span>

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public 
```

<span data-ttu-id="e84a4-121">Du bör få ett svar tooyour begäran inom några sekunder.</span><span class="sxs-lookup"><span data-stu-id="e84a4-121">Within a few seconds, you should get a response tooyour request.</span></span> <span data-ttu-id="e84a4-122">Inledningsvis hello behållare kommer att ha en **skapa** tillstånd, men det bör starta inom några sekunder.</span><span class="sxs-lookup"><span data-stu-id="e84a4-122">Initially, hello container will be in a **Creating** state, but it should start within a few seconds.</span></span> <span data-ttu-id="e84a4-123">Du kan kontrollera statusen för hello med hello `show` kommando:</span><span class="sxs-lookup"><span data-stu-id="e84a4-123">You can check hello status using hello `show` command:</span></span>

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

<span data-ttu-id="e84a4-124">Längst ned hello hello utdata visas etableringsstatusen hello-behållaren och dess IP-adress:</span><span class="sxs-lookup"><span data-stu-id="e84a4-124">At hello bottom of hello output, you will see hello container's provisioning state and its IP address:</span></span>

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

<span data-ttu-id="e84a4-125">När behållaren hello flyttar toohello **lyckades** tillstånd, kan du nå den i hello webbläsare med hello IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="e84a4-125">Once hello container moves toohello **Succeeded** state, you can reach it in hello browser using hello IP address provided.</span></span> 

![App som distribuerats via Azure Container Instances visas i webbläsare][aci-app-browser]

## <a name="pull-hello-container-logs"></a><span data-ttu-id="e84a4-127">Hämta hello behållaren loggar</span><span class="sxs-lookup"><span data-stu-id="e84a4-127">Pull hello container logs</span></span>

<span data-ttu-id="e84a4-128">Du kan dra hello loggar för hello-behållaren som du skapat med hello `logs` kommando:</span><span class="sxs-lookup"><span data-stu-id="e84a4-128">You can pull hello logs for hello container you created using hello `logs` command:</span></span>

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

<span data-ttu-id="e84a4-129">Resultat:</span><span class="sxs-lookup"><span data-stu-id="e84a4-129">Output:</span></span>

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-hello-container"></a><span data-ttu-id="e84a4-130">Ta bort hello behållaren</span><span class="sxs-lookup"><span data-stu-id="e84a4-130">Delete hello container</span></span>

<span data-ttu-id="e84a4-131">När du är klar med hello behållare, du kan ta bort den med hjälp av hello `delete` kommando:</span><span class="sxs-lookup"><span data-stu-id="e84a4-131">When you are done with hello container, you can remove it using hello `delete` command:</span></span>

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="e84a4-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e84a4-132">Next steps</span></span>

<span data-ttu-id="e84a4-133">Alla hello code för hello-behållare som används i den här snabbstartsguide [på GitHub][app-github-repo], tillsammans med dess Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="e84a4-133">All of hello code for hello container used in this quick start is available [on GitHub][app-github-repo], along with its Dockerfile.</span></span> <span data-ttu-id="e84a4-134">Om du vill tootry skapar själv och distribuera den tooAzure Behållarinstanser som använder hello Azure Container registret fortsätta toohello Azure Behållarinstanser kursen.</span><span class="sxs-lookup"><span data-stu-id="e84a4-134">If you'd like tootry building it yourself and deploying it tooAzure Container Instances using hello Azure Container Registry, continue toohello Azure Container Instances tutorial.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e84a4-135">Azure Container Instances-självstudier</span><span class="sxs-lookup"><span data-stu-id="e84a4-135">Azure Container Instances tutorials</span></span>](./container-instances-tutorial-prepare-app.md)


<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png