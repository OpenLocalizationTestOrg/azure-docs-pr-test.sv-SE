---
title: "aaaAzure behållaren Service Quickstart - distribuera DC/OS-kluster | Microsoft Docs"
description: Azure Container Service Quickstart - distribuera DC/OS-klustret
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b961f15bd73deeafda5a3fc25ab53c839195219b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-dcos-cluster"></a><span data-ttu-id="bcd3e-104">Distribuera ett DC/OS-kluster</span><span class="sxs-lookup"><span data-stu-id="bcd3e-104">Deploy a DC/OS cluster</span></span>

<span data-ttu-id="bcd3e-105">DC/OS är en distribuerad plattform för moderna och körning av program.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-105">DC/OS provides a distributed platform for running modern and containerized applications.</span></span> <span data-ttu-id="bcd3e-106">Med Azure Container Service är etablering av en produktion redo DC/OS-kluster snabbt och enkelt.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-106">With Azure Container Service, provisioning of a production ready DC/OS cluster is simple and quick.</span></span> <span data-ttu-id="bcd3e-107">Den här snabbstartsguide information hello grundläggande steg behövs toodeploy ett DC/OS-klustret och köra grundläggande arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-107">This quick start details hello basic steps needed toodeploy a DC/OS cluster and run basic workload.</span></span>

<span data-ttu-id="bcd3e-108">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="bcd3e-109">Den här kursen kräver hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-109">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="bcd3e-110">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-110">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="bcd3e-111">Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="bcd3e-111">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="log-in-tooazure"></a><span data-ttu-id="bcd3e-112">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="bcd3e-112">Log in tooAzure</span></span> 

<span data-ttu-id="bcd3e-113">Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-113">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

```azurecli
az login
```

## <a name="create-a-resource-group"></a><span data-ttu-id="bcd3e-114">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="bcd3e-114">Create a resource group</span></span>

<span data-ttu-id="bcd3e-115">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-115">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="bcd3e-116">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-116">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="bcd3e-117">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-117">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-dcos-cluster"></a><span data-ttu-id="bcd3e-118">Skapa DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="bcd3e-118">Create DC/OS cluster</span></span>

<span data-ttu-id="bcd3e-119">Skapa ett DC/OS-kluster med hello [az acs skapa](/cli/azure/acs#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-119">Create a DC/OS cluster with hello [az acs create](/cli/azure/acs#create) command.</span></span>

<span data-ttu-id="bcd3e-120">hello följande exempel skapas ett DC/OS-kluster med namnet *myDCOSCluster* och skapar SSH-nycklar, om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-120">hello following example creates a DC/OS cluster named *myDCOSCluster* and creates SSH keys if they do not already exist.</span></span> <span data-ttu-id="bcd3e-121">toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-121">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

<span data-ttu-id="bcd3e-122">Om några minuter hello-kommandot har slutförts och returnerar information om hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-122">After several minutes, hello command completes, and returns information about hello deployment.</span></span>

## <a name="connect-toodcos-cluster"></a><span data-ttu-id="bcd3e-123">Ansluta tooDC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="bcd3e-123">Connect tooDC/OS cluster</span></span>

<span data-ttu-id="bcd3e-124">När ett DC/OS-klustret har skapats, kan det vara åtkomst via en SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-124">Once a DC/OS cluster has been created, it can be accesses through an SSH tunnel.</span></span> <span data-ttu-id="bcd3e-125">Hello kör följande kommando tooreturn hello offentliga IP-adressen för hello DC/OS-hanteraren.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-125">Run hello following command tooreturn hello public IP address of hello DC/OS master.</span></span> <span data-ttu-id="bcd3e-126">Den här IP-adressen är lagrad i en variabel och användas i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-126">This IP address is stored in a variable and used in hello next step.</span></span>

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

<span data-ttu-id="bcd3e-127">toocreate hello SSH-tunnel, kör följande kommando hello och följer hello på skärmen instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-127">toocreate hello SSH tunnel, run hello following command and follow hello on-screen instructions.</span></span> <span data-ttu-id="bcd3e-128">Om port 80 används misslyckas hello kommandot.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-128">If port 80 is already in use, hello command fails.</span></span> <span data-ttu-id="bcd3e-129">Uppdatera hello tunneldata port tooone används som `85:localhost:80`.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-129">Update hello tunneled port tooone not in use, such as `85:localhost:80`.</span></span> 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

<span data-ttu-id="bcd3e-130">hello SSH-tunnel kan testas genom att bläddra för`http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-130">hello SSH tunnel can be tested by browsing too`http://localhost`.</span></span> <span data-ttu-id="bcd3e-131">Om en port andra att 80 har använts, justera hello plats toomatch.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-131">If a port other that 80 has been used, adjust hello location toomatch.</span></span> 

<span data-ttu-id="bcd3e-132">Om hello SSH-tunnel har skapats, returneras hello DC/OS-portalen.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-132">If hello SSH tunnel was successfully created, hello DC/OS portal is returned.</span></span>

![DC/OS-GRÄNSSNITTET](./media/container-service-dcos-quickstart/dcos-ui.png)

## <a name="install-dcos-cli"></a><span data-ttu-id="bcd3e-134">Installera DC/OS CLI</span><span class="sxs-lookup"><span data-stu-id="bcd3e-134">Install DC/OS CLI</span></span>

<span data-ttu-id="bcd3e-135">hello DC/OS-kommandoradsgränssnittet är används toomanage ett DC/OS-kluster från hello kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-135">hello DC/OS command line interface is used toomanage a DC/OS cluster from hello command-line.</span></span> <span data-ttu-id="bcd3e-136">Installera hello DC/OS cli med hello [az installera acs-DC/OS-cli](/azure/acs/dcos#install-cli) kommando.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-136">Install hello DC/OS cli using hello [az acs dcos install-cli](/azure/acs/dcos#install-cli) command.</span></span> <span data-ttu-id="bcd3e-137">Om du använder Azure CloudShell har hello DC/OS CLI redan installerats.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-137">If you are using Azure CloudShell, hello DC/OS CLI is already installed.</span></span> 

<span data-ttu-id="bcd3e-138">Om du kör hello Azure CLI på macOS- eller Linux behöva toorun hello kommandot med sudo.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-138">If you are running hello Azure CLI on macOS or Linux, you might need toorun hello command with sudo.</span></span>

```azurecli
az acs dcos install-cli
```

<span data-ttu-id="bcd3e-139">Innan du hello CLI kan användas med hello kluster, måste det vara konfigurerade toouse hello SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-139">Before hello CLI can be used with hello cluster, it must be configured toouse hello SSH tunnel.</span></span> <span data-ttu-id="bcd3e-140">toodo kör så hello följande kommando, justera hello porten om det behövs.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-140">toodo so, run hello following command, adjusting hello port if needed.</span></span>

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a><span data-ttu-id="bcd3e-141">Köra ett program</span><span class="sxs-lookup"><span data-stu-id="bcd3e-141">Run an application</span></span>

<span data-ttu-id="bcd3e-142">hello standard schemaläggning mekanism för en ACS DC/OS-klustret är Marathon.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-142">hello default scheduling mechanism for an ACS DC/OS cluster is Marathon.</span></span> <span data-ttu-id="bcd3e-143">Marathon är används toostart ett program och hantera hello tillståndet för programmet hello på hello DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-143">Marathon is used toostart an application and manage hello state of hello application on hello DC/OS cluster.</span></span> <span data-ttu-id="bcd3e-144">tooschedule ett program via Marathon, skapa en fil med namnet *marathon app.json*, och kopiera hello efter innehållet i den.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-144">tooschedule an application through Marathon, create a file named *marathon-app.json*, and copy hello following contents into it.</span></span> 

```json
{
  "id": "demo-app",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  },
  "acceptedResourceRoles": [
    "slave_public"
  ]
}
```

<span data-ttu-id="bcd3e-145">Kör hello efter kommandot tooschedule hello programmet toorun hello DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-145">Run hello following command tooschedule hello application toorun on hello DC/OS cluster.</span></span>

```azurecli
dcos marathon app add marathon-app.json
```

<span data-ttu-id="bcd3e-146">toosee hello Distributionsstatus för hello app, kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-146">toosee hello deployment status for hello app, run hello following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="bcd3e-147">När hello **väntar på** kolumnvärde växlar från *SANT* för*FALSKT*, programdistribution har slutförts.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-147">When hello **WAITING** column value switches from *True* too*False*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/1    ---       ---      False      DOCKER   None
```

<span data-ttu-id="bcd3e-148">Hämta hello offentliga IP-adress hello DC/OS-klustret agenterna.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-148">Get hello public IP address of hello DC/OS cluster agents.</span></span>

```azurecli
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

<span data-ttu-id="bcd3e-149">Bläddra toothis adress returnerar hello NGINX standardwebbplatsen.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-149">Browsing toothis address returns hello default NGINX site.</span></span>

![NGINX](./media/container-service-dcos-quickstart/nginx.png)

## <a name="delete-dcos-cluster"></a><span data-ttu-id="bcd3e-151">Ta bort DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="bcd3e-151">Delete DC/OS cluster</span></span>

<span data-ttu-id="bcd3e-152">När du inte längre behövs kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, DC/OS-kluster och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-152">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, DC/OS cluster, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a><span data-ttu-id="bcd3e-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bcd3e-153">Next steps</span></span>

<span data-ttu-id="bcd3e-154">I den här snabbstartsguide du har distribuerat ett DC/OS-kluster och har kört en enkel dockerbehållare på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-154">In this quick start, you’ve deployed a DC/OS cluster and have run a simple Docker container on hello cluster.</span></span> <span data-ttu-id="bcd3e-155">toolearn mer om Azure Container Service fortsätta toohello ACS självstudier.</span><span class="sxs-lookup"><span data-stu-id="bcd3e-155">toolearn more about Azure Container Service, continue toohello ACS tutorials.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bcd3e-156">Hantera en ACS DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="bcd3e-156">Manage an ACS DC/OS Cluster</span></span>](container-service-dcos-manage-tutorial.md)