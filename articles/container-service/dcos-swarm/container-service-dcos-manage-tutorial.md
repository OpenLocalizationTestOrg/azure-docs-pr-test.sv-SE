---
title: "aaaAzure Container Service tutorial – hantera DC/OS | Microsoft Docs"
description: "Självstudiekurs för Azure Container Service - hantera DC/OS"
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
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b91c433bfd7e48ec405cc62be1486d9d4662839d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-service-tutorial---manage-dcos"></a><span data-ttu-id="3110d-104">Självstudiekurs för Azure Container Service - hantera DC/OS</span><span class="sxs-lookup"><span data-stu-id="3110d-104">Azure Container Service tutorial - Manage DC/OS</span></span>

<span data-ttu-id="3110d-105">DC/OS är en distribuerad plattform för moderna och körning av program.</span><span class="sxs-lookup"><span data-stu-id="3110d-105">DC/OS provides a distributed platform for running modern and containerized applications.</span></span> <span data-ttu-id="3110d-106">Med Azure Container Service är etablering av en produktion redo DC/OS-kluster snabbt och enkelt.</span><span class="sxs-lookup"><span data-stu-id="3110d-106">With Azure Container Service, provisioning of a production ready DC/OS cluster is simple and quick.</span></span> <span data-ttu-id="3110d-107">Den här snabbstartsguide information grundläggande steg behövs toodeploy ett DC/OS-klustret och köra grundläggande arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="3110d-107">This quick start details basic steps needed toodeploy a DC/OS cluster and run basic workload.</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3110d-108">Skapa ett ACS DC/OS-kluster</span><span class="sxs-lookup"><span data-stu-id="3110d-108">Create an ACS DC/OS cluster</span></span>
> * <span data-ttu-id="3110d-109">Ansluta toohello kluster</span><span class="sxs-lookup"><span data-stu-id="3110d-109">Connect toohello cluster</span></span>
> * <span data-ttu-id="3110d-110">Installera hello DC/OS CLI</span><span class="sxs-lookup"><span data-stu-id="3110d-110">Install hello DC/OS CLI</span></span>
> * <span data-ttu-id="3110d-111">Distribuera ett program toohello kluster</span><span class="sxs-lookup"><span data-stu-id="3110d-111">Deploy an application toohello cluster</span></span>
> * <span data-ttu-id="3110d-112">Skala ett program på hello klustret</span><span class="sxs-lookup"><span data-stu-id="3110d-112">Scale an application on hello cluster</span></span>
> * <span data-ttu-id="3110d-113">Skala hello DC/OS-klusternoder</span><span class="sxs-lookup"><span data-stu-id="3110d-113">Scale hello DC/OS cluster nodes</span></span>
> * <span data-ttu-id="3110d-114">Grundläggande DC/OS-hantering</span><span class="sxs-lookup"><span data-stu-id="3110d-114">Basic DC/OS management</span></span>
> * <span data-ttu-id="3110d-115">Ta bort hello DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="3110d-115">Delete hello DC/OS cluster</span></span>

<span data-ttu-id="3110d-116">Den här kursen kräver hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3110d-116">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="3110d-117">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="3110d-117">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="3110d-118">Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3110d-118">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-dcos-cluster"></a><span data-ttu-id="3110d-119">Skapa DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="3110d-119">Create DC/OS cluster</span></span>

<span data-ttu-id="3110d-120">Först skapar en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="3110d-120">First, create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="3110d-121">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="3110d-121">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="3110d-122">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westeurope* plats.</span><span class="sxs-lookup"><span data-stu-id="3110d-122">hello following example creates a resource group named *myResourceGroup* in hello *westeurope* location.</span></span>

```azurecli
az group create --name myResourceGroup --location westeurope
```

<span data-ttu-id="3110d-123">Skapa sedan ett DC/OS-kluster med hello [az acs skapa](/cli/azure/acs#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="3110d-123">Next, create a DC/OS cluster with hello [az acs create](/cli/azure/acs#create) command.</span></span>

<span data-ttu-id="3110d-124">hello följande exempel skapas ett DC/OS-kluster med namnet *myDCOSCluster* och skapar SSH-nycklar, om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="3110d-124">hello following example creates a DC/OS cluster named *myDCOSCluster* and creates SSH keys if they do not already exist.</span></span> <span data-ttu-id="3110d-125">toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.</span><span class="sxs-lookup"><span data-stu-id="3110d-125">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

<span data-ttu-id="3110d-126">Om några minuter hello-kommandot har slutförts och returnerar information om hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="3110d-126">After several minutes, hello command completes, and returns information about hello deployment.</span></span>

## <a name="connect-toodcos-cluster"></a><span data-ttu-id="3110d-127">Ansluta tooDC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="3110d-127">Connect tooDC/OS cluster</span></span>

<span data-ttu-id="3110d-128">När ett DC/OS-klustret har skapats, kan det vara åtkomst via en SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="3110d-128">Once a DC/OS cluster has been created, it can be accesses through an SSH tunnel.</span></span> <span data-ttu-id="3110d-129">Hello kör följande kommando tooreturn hello offentliga IP-adressen för hello DC/OS-hanteraren.</span><span class="sxs-lookup"><span data-stu-id="3110d-129">Run hello following command tooreturn hello public IP address of hello DC/OS master.</span></span> <span data-ttu-id="3110d-130">Den här IP-adressen är lagrad i en variabel och användas i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="3110d-130">This IP address is stored in a variable and used in hello next step.</span></span>

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

<span data-ttu-id="3110d-131">toocreate hello SSH-tunnel, kör följande kommando hello och följer hello på skärmen instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="3110d-131">toocreate hello SSH tunnel, run hello following command and follow hello on-screen instructions.</span></span> <span data-ttu-id="3110d-132">Om port 80 används misslyckas hello kommandot.</span><span class="sxs-lookup"><span data-stu-id="3110d-132">If port 80 is already in use, hello command fails.</span></span> <span data-ttu-id="3110d-133">Uppdatera hello tunneldata port tooone används som `85:localhost:80`.</span><span class="sxs-lookup"><span data-stu-id="3110d-133">Update hello tunneled port tooone not in use, such as `85:localhost:80`.</span></span> 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

## <a name="install-dcos-cli"></a><span data-ttu-id="3110d-134">Installera DC/OS CLI</span><span class="sxs-lookup"><span data-stu-id="3110d-134">Install DC/OS CLI</span></span>

<span data-ttu-id="3110d-135">Installera hello DC/OS cli med hello [az installera acs-DC/OS-cli](/azure/acs/dcos#install-cli) kommando.</span><span class="sxs-lookup"><span data-stu-id="3110d-135">Install hello DC/OS cli using hello [az acs dcos install-cli](/azure/acs/dcos#install-cli) command.</span></span> <span data-ttu-id="3110d-136">Om du använder Azure CloudShell har hello DC/OS CLI redan installerats.</span><span class="sxs-lookup"><span data-stu-id="3110d-136">If you are using Azure CloudShell, hello DC/OS CLI is already installed.</span></span> <span data-ttu-id="3110d-137">Om du kör hello Azure CLI på macOS- eller Linux behöva toorun hello kommandot med sudo.</span><span class="sxs-lookup"><span data-stu-id="3110d-137">If you are running hello Azure CLI on macOS or Linux, you might need toorun hello command with sudo.</span></span>

```azurecli
az acs dcos install-cli
```

<span data-ttu-id="3110d-138">Innan du hello CLI kan användas med hello kluster, måste det vara konfigurerade toouse hello SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="3110d-138">Before hello CLI can be used with hello cluster, it must be configured toouse hello SSH tunnel.</span></span> <span data-ttu-id="3110d-139">toodo kör så hello följande kommando, justera hello porten om det behövs.</span><span class="sxs-lookup"><span data-stu-id="3110d-139">toodo so, run hello following command, adjusting hello port if needed.</span></span>

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a><span data-ttu-id="3110d-140">Köra ett program</span><span class="sxs-lookup"><span data-stu-id="3110d-140">Run an application</span></span>

<span data-ttu-id="3110d-141">hello standard schemaläggning mekanism för en ACS DC/OS-klustret är Marathon.</span><span class="sxs-lookup"><span data-stu-id="3110d-141">hello default scheduling mechanism for an ACS DC/OS cluster is Marathon.</span></span> <span data-ttu-id="3110d-142">Marathon är används toostart ett program och hantera hello tillståndet för programmet hello på hello DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="3110d-142">Marathon is used toostart an application and manage hello state of hello application on hello DC/OS cluster.</span></span> <span data-ttu-id="3110d-143">tooschedule ett program via Marathon, skapa en fil med namnet **marathon app.json**, och kopiera hello efter innehållet i den.</span><span class="sxs-lookup"><span data-stu-id="3110d-143">tooschedule an application through Marathon, create a file named **marathon-app.json**, and copy hello following contents into it.</span></span> 

```json
{
  "id": "demo-app-private",
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
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

<span data-ttu-id="3110d-144">Kör hello efter kommandot tooschedule hello programmet toorun hello DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="3110d-144">Run hello following command tooschedule hello application toorun on hello DC/OS cluster.</span></span>

```azurecli
dcos marathon app add marathon-app.json
```

<span data-ttu-id="3110d-145">toosee hello Distributionsstatus för hello app, kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="3110d-145">toosee hello deployment status for hello app, run hello following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="3110d-146">När hello **uppgifter** kolumnvärde växlar från *0/1* för*1/1*, programdistribution har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3110d-146">When hello **TASKS** column value switches from *0/1* too*1/1*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a><span data-ttu-id="3110d-147">Skala Marathon program</span><span class="sxs-lookup"><span data-stu-id="3110d-147">Scale Marathon application</span></span>

<span data-ttu-id="3110d-148">En enda instans-programmet skapades i föregående exempel hello.</span><span class="sxs-lookup"><span data-stu-id="3110d-148">In hello previous example, a single instance application was created.</span></span> <span data-ttu-id="3110d-149">tooupdate distributionen så att tre instanser av programmet hello är tillgängliga, öppna hello **marathon app.json** filen och uppdatera hello instans egenskapen too3.</span><span class="sxs-lookup"><span data-stu-id="3110d-149">tooupdate this deployment so that three instances of hello application are available, open up hello **marathon-app.json** file, and update hello instance property too3.</span></span>

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 3,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

<span data-ttu-id="3110d-150">Uppdatera hello program med hjälp av hello `dcos marathon app update` kommando.</span><span class="sxs-lookup"><span data-stu-id="3110d-150">Update hello application using hello `dcos marathon app update` command.</span></span>

```azurecli
dcos marathon app update demo-app-private < marathon-app.json
```

<span data-ttu-id="3110d-151">toosee hello Distributionsstatus för hello app, kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="3110d-151">toosee hello deployment status for hello app, run hello following command.</span></span>

```azurecli
dcos marathon app list
```

<span data-ttu-id="3110d-152">När hello **uppgifter** kolumnvärde växlar från *1/3* för*3-1*, programdistribution har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3110d-152">When hello **TASKS** column value switches from *1/3* too*3/1*, application deployment has completed.</span></span>

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a><span data-ttu-id="3110d-153">Kör internet tillgänglig app</span><span class="sxs-lookup"><span data-stu-id="3110d-153">Run internet accessible app</span></span>

<span data-ttu-id="3110d-154">hello ACS DC/OS-klustret består av två mängder nod, en offentlig som kan nås på hello internet och en privat som inte är tillgänglig på hello internet.</span><span class="sxs-lookup"><span data-stu-id="3110d-154">hello ACS DC/OS cluster consists of two node sets, one public which is accessible on hello internet, and one private which is not accessible on hello internet.</span></span> <span data-ttu-id="3110d-155">hello standarduppsättning är hello privata noder, som användes i hello sista exemplet.</span><span class="sxs-lookup"><span data-stu-id="3110d-155">hello default set is hello private nodes, which was used in hello last example.</span></span>

<span data-ttu-id="3110d-156">toomake ett program som är tillgänglig på Hej internet, distribuera dem toohello offentliga noduppsättning har angetts.</span><span class="sxs-lookup"><span data-stu-id="3110d-156">toomake an application accessible on hello internet, deploy them toohello public node set.</span></span> <span data-ttu-id="3110d-157">toodo så ger hello `acceptedResourceRoles` objekt värdet `slave_public`.</span><span class="sxs-lookup"><span data-stu-id="3110d-157">toodo so, give hello `acceptedResourceRoles` object a value of `slave_public`.</span></span>

<span data-ttu-id="3110d-158">Skapa en fil med namnet **nginx public.json** och kopiera hello efter innehållet i den.</span><span class="sxs-lookup"><span data-stu-id="3110d-158">Create a file named **nginx-public.json** and copy hello following contents into it.</span></span>

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

<span data-ttu-id="3110d-159">Kör hello efter kommandot tooschedule hello programmet toorun hello DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="3110d-159">Run hello following command tooschedule hello application toorun on hello DC/OS cluster.</span></span>

```azurecli 
dcos marathon app add nginx-public.json
```

<span data-ttu-id="3110d-160">Hämta hello offentliga IP-adress hello DC/OS offentliga klustret agenter.</span><span class="sxs-lookup"><span data-stu-id="3110d-160">Get hello public IP address of hello DC/OS public cluster agents.</span></span>

```azurecli 
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

<span data-ttu-id="3110d-161">Bläddra toothis adress returnerar hello NGINX standardwebbplatsen.</span><span class="sxs-lookup"><span data-stu-id="3110d-161">Browsing toothis address returns hello default NGINX site.</span></span>

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a><span data-ttu-id="3110d-163">Skala DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="3110d-163">Scale DC/OS cluster</span></span>

<span data-ttu-id="3110d-164">I föregående exempel hello var ett program skalade toomultiple-instans.</span><span class="sxs-lookup"><span data-stu-id="3110d-164">In hello previous examples, an application was scaled toomultiple instance.</span></span> <span data-ttu-id="3110d-165">hello DC/OS-infrastruktur kan också vara skalade tooprovide mer eller mindre beräkningskapacitet.</span><span class="sxs-lookup"><span data-stu-id="3110d-165">hello DC/OS infrastructure can also be scaled tooprovide more or less compute capacity.</span></span> <span data-ttu-id="3110d-166">Detta görs med hello [az acs skala]() kommando.</span><span class="sxs-lookup"><span data-stu-id="3110d-166">This is done with hello [az acs scale]() command.</span></span> 

<span data-ttu-id="3110d-167">toosee hello aktuellt antal DC/OS-agenterna använda hello [az acs visa](/cli/azure/acs#show) kommando.</span><span class="sxs-lookup"><span data-stu-id="3110d-167">toosee hello current count of DC/OS agents, use hello [az acs show](/cli/azure/acs#show) command.</span></span>

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

<span data-ttu-id="3110d-168">tooincrease hello antal too5 använder hello [az acs skala](/cli/azure/acs#scale) kommando.</span><span class="sxs-lookup"><span data-stu-id="3110d-168">tooincrease hello count too5, use hello [az acs scale](/cli/azure/acs#scale) command.</span></span> 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a><span data-ttu-id="3110d-169">Ta bort DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="3110d-169">Delete DC/OS cluster</span></span>

<span data-ttu-id="3110d-170">När du inte längre behövs kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, DC/OS-kluster och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="3110d-170">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, DC/OS cluster, and all related resources.</span></span>

```azurecli 
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a><span data-ttu-id="3110d-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3110d-171">Next steps</span></span>

<span data-ttu-id="3110d-172">I den här kursen har du lärt dig om grundläggande DC/OS-hanteringsaktivitet inklusive hello följande.</span><span class="sxs-lookup"><span data-stu-id="3110d-172">In this tutorial, you have learned about basic DC/OS management task including hello following.</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="3110d-173">Skapa ett ACS DC/OS-kluster</span><span class="sxs-lookup"><span data-stu-id="3110d-173">Create an ACS DC/OS cluster</span></span>
> * <span data-ttu-id="3110d-174">Ansluta toohello kluster</span><span class="sxs-lookup"><span data-stu-id="3110d-174">Connect toohello cluster</span></span>
> * <span data-ttu-id="3110d-175">Installera hello DC/OS CLI</span><span class="sxs-lookup"><span data-stu-id="3110d-175">Install hello DC/OS CLI</span></span>
> * <span data-ttu-id="3110d-176">Distribuera ett program toohello kluster</span><span class="sxs-lookup"><span data-stu-id="3110d-176">Deploy an application toohello cluster</span></span>
> * <span data-ttu-id="3110d-177">Skala ett program på hello klustret</span><span class="sxs-lookup"><span data-stu-id="3110d-177">Scale an application on hello cluster</span></span>
> * <span data-ttu-id="3110d-178">Skala hello DC/OS-klusternoder</span><span class="sxs-lookup"><span data-stu-id="3110d-178">Scale hello DC/OS cluster nodes</span></span>
> * <span data-ttu-id="3110d-179">Ta bort hello DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="3110d-179">Delete hello DC/OS cluster</span></span>

<span data-ttu-id="3110d-180">Avancerade toohello nästa självstudiekurs toolearn om att läsa in belastningsutjämning programmet i DC/OS på Azure.</span><span class="sxs-lookup"><span data-stu-id="3110d-180">Advance toohello next tutorial toolearn about load balancing application in DC/OS on Azure.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="3110d-181">Belastningsutjämna program</span><span class="sxs-lookup"><span data-stu-id="3110d-181">Load balance applications</span></span>](container-service-load-balancing.md)