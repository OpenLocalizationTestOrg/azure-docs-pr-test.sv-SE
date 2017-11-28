---
title: "aaaFile resurs för Azure DC/OS-kluster | Microsoft Docs"
description: Skapa och montera en filresursen tooa DC/OS-klustret i Azure Container Service
services: container-service
documentationcenter: 
author: julienstroheker
manager: dcaro
editor: 
tags: acs, azure-container-service
keywords: "Docker, behållare, Micro-tjänster, Mesos, Azure, filresursen, cifs"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: d18090d414a3e00202ccde442ac9b865d74f1e34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-mount-a-file-share-tooa-dcos-cluster"></a><span data-ttu-id="e003d-104">Skapa och montera en filresursen tooa DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="e003d-104">Create and mount a file share tooa DC/OS cluster</span></span>
<span data-ttu-id="e003d-105">Den här självstudiekursen beskrivs hur toocreate en fil dela i Azure och montera på varje agent och huvudserver för hello DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="e003d-105">This tutorial details how toocreate a file share in Azure and mount it on each agent and master of hello DC/OS cluster.</span></span> <span data-ttu-id="e003d-106">Skapa en filresurs gör det enklare tooshare filer i klustret som konfiguration, access, loggar och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="e003d-106">Setting up a file share makes it easier tooshare files across your cluster such as configuration, access, logs, and more.</span></span> <span data-ttu-id="e003d-107">hello följande aktiviteter utförs i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="e003d-107">hello following tasks are completed in this tutorial:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e003d-108">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="e003d-108">Create an Azure storage account</span></span>
> * <span data-ttu-id="e003d-109">Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="e003d-109">Create a file share</span></span>
> * <span data-ttu-id="e003d-110">Montera hello resursen i hello DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="e003d-110">Mount hello share in hello DC/OS cluster</span></span>

<span data-ttu-id="e003d-111">Du behöver en ACS-DC /-OS klustret toocomplete hello stegen i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e003d-111">You need an ACS DC/OS cluster toocomplete hello steps in this tutorial.</span></span> <span data-ttu-id="e003d-112">Om det behövs, [detta skriptexempel](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) kan skapa en åt dig.</span><span class="sxs-lookup"><span data-stu-id="e003d-112">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="e003d-113">Den här kursen kräver hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e003d-113">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="e003d-114">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="e003d-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="e003d-115">Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e003d-115">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-file-share-on-microsoft-azure"></a><span data-ttu-id="e003d-116">Skapa en filresurs på Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="e003d-116">Create a file share on Microsoft Azure</span></span>

<span data-ttu-id="e003d-117">Hello storage-konto och filresursen måste skapas innan du använder en Azure-filresursen med en ACS DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="e003d-117">Before using an Azure file share with an ACS DC/OS cluster, hello storage account and file share must be created.</span></span> <span data-ttu-id="e003d-118">Kör följande skript toocreate hello lagring och filresursen hello.</span><span class="sxs-lookup"><span data-stu-id="e003d-118">Run hello following script toocreate hello storage and file share.</span></span> <span data-ttu-id="e003d-119">Uppdatera hello parametrar med thoes från din miljö.</span><span class="sxs-lookup"><span data-stu-id="e003d-119">Update hello parameters with thoes from your environment.</span></span>

```azurecli-interactive
# Change these four parameters
DCOS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
DCOS_PERS_RESOURCE_GROUP=myResourceGroup
DCOS_PERS_LOCATION=eastus
DCOS_PERS_SHARE_NAME=dcosshare

# Create hello storage account with hello parameters
az storage account create -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -l $DCOS_PERS_LOCATION --sku Standard_LRS

# Export hello connection string as an environment variable, this is used when creating hello Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -o tsv`

# Create hello share
az storage share create -n $DCOS_PERS_SHARE_NAME
```

## <a name="mount-hello-share-in-your-cluster"></a><span data-ttu-id="e003d-120">Montera hello resursen i klustret</span><span class="sxs-lookup"><span data-stu-id="e003d-120">Mount hello share in your cluster</span></span>

<span data-ttu-id="e003d-121">Därefter hello filresursen behov toobe monteras på varje virtuell dator i klustret.</span><span class="sxs-lookup"><span data-stu-id="e003d-121">Next, hello file share needs toobe mounted on every virtual machine inside your cluster.</span></span> <span data-ttu-id="e003d-122">Den här åtgärden har slutförts med verktyget hello-cifs/protokollet.</span><span class="sxs-lookup"><span data-stu-id="e003d-122">This task is completed using hello cifs tool/protocol.</span></span> <span data-ttu-id="e003d-123">hello montering kan utföras manuellt på varje nod i klustret hello eller genom att köra ett skript mot varje nod i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="e003d-123">hello mount operation can be completed manually on each node of hello cluster, or by running a script against each node in hello cluster.</span></span>

<span data-ttu-id="e003d-124">I det här exemplet två skript körs, en toomount hello Azure resursen och andra toorun skriptet på varje nod i hello DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="e003d-124">In this example, two scripts are run, one toomount hello Azure file share, and a second toorun this script on each node of hello DC/OS cluster.</span></span>

<span data-ttu-id="e003d-125">Först krävs hello Azure lagringskontonamnet och åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="e003d-125">First, hello Azure storage account name, and access key are needed.</span></span> <span data-ttu-id="e003d-126">Kör följande kommandon tooget hello informationen.</span><span class="sxs-lookup"><span data-stu-id="e003d-126">Run hello following commands tooget this information.</span></span> <span data-ttu-id="e003d-127">Notera vardera, dessa värden används i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="e003d-127">Take note of each, these values are used in a later step.</span></span>

<span data-ttu-id="e003d-128">Lagringskontonamn:</span><span class="sxs-lookup"><span data-stu-id="e003d-128">Storage account name:</span></span>

```azurecli-interactive
STORAGE_ACCT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCT
```

<span data-ttu-id="e003d-129">Åtkomstnyckeln för lagringskontot:</span><span class="sxs-lookup"><span data-stu-id="e003d-129">Storage account access key:</span></span>

```azurecli-interactive
az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCT --query "[0].value" -o tsv
```

<span data-ttu-id="e003d-130">Därefter hämta hello FQDN för hello DC/OS master och lagrar den i en variabel.</span><span class="sxs-lookup"><span data-stu-id="e003d-130">Next, get hello FQDN of hello DC/OS master and store it in a variable.</span></span>

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

<span data-ttu-id="e003d-131">Kopiera din privata nyckel toohello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="e003d-131">Copy your private key toohello master node.</span></span> <span data-ttu-id="e003d-132">Den här nyckeln är nödvändiga toocreate en ssh-anslutning med alla noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="e003d-132">This key is needed toocreate an ssh connection with all nodes in hello cluster.</span></span> <span data-ttu-id="e003d-133">Uppdatera hello användarnamn om ett standardvärde används när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="e003d-133">Update hello user name if a non-default value was used when creating hello cluster.</span></span> 

```azurecli-interactive
scp ~/.ssh/id_rsa azureuser@$FQDN:~/.ssh
```

<span data-ttu-id="e003d-134">Skapa en SSH-anslutning med hello master (eller hello första master) på ditt DC/OS-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="e003d-134">Create an SSH connection with hello master (or hello first master) of your DC/OS-based cluster.</span></span> <span data-ttu-id="e003d-135">Uppdatera hello användarnamn om ett standardvärde används när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="e003d-135">Update hello user name if a non-default value was used when creating hello cluster.</span></span>

```azurecli-interactive
ssh azureuser@$FQDN
```

<span data-ttu-id="e003d-136">Skapa en fil med namnet **cifsMount.sh**, och kopiera hello efter innehållet i den.</span><span class="sxs-lookup"><span data-stu-id="e003d-136">Create a file named **cifsMount.sh**, and copy hello following contents into it.</span></span> 

<span data-ttu-id="e003d-137">Det här skriptet är används toomount hello Azure-filresursen.</span><span class="sxs-lookup"><span data-stu-id="e003d-137">This script is used toomount hello Azure file share.</span></span> <span data-ttu-id="e003d-138">Uppdatera hello `STORAGE_ACCT_NAME` och `ACCESS_KEY` variabler med hello information som samlas in tidigare.</span><span class="sxs-lookup"><span data-stu-id="e003d-138">Update hello `STORAGE_ACCT_NAME` and `ACCESS_KEY` variables with hello information collected earlier.</span></span>

```azurecli-interactive
#!/bin/bash

# Azure storage account name and access key
STORAGE_ACCT_NAME=mystorageaccount
ACCESS_KEY=mystorageaccountKey

# Install hello cifs utils, should be already installed
sudo apt-get update && sudo apt-get -y install cifs-utils

# Create hello local folder that will contain our share
if [ ! -d "/mnt/share/dcosshare" ]; then sudo mkdir -p "/mnt/share/dcosshare" ; fi

# Mount hello share under hello previous local folder created
sudo mount -t cifs //$STORAGE_ACCT_NAME.file.core.windows.net/dcosshare /mnt/share/dcosshare -o vers=3.0,username=$STORAGE_ACCT_NAME,password=$ACCESS_KEY,dir_mode=0777,file_mode=0777
```
<span data-ttu-id="e003d-139">Skapa en andra fil som heter **getNodesRunScript.sh** och kopiera hello efter innehållet i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="e003d-139">Create a second file named **getNodesRunScript.sh** and copy hello following contents into hello file.</span></span> 

<span data-ttu-id="e003d-140">Det här skriptet identifierar alla noder i klustret och kör sedan hello **cifsMount.sh** toomount skript hello filresurs på varje.</span><span class="sxs-lookup"><span data-stu-id="e003d-140">This script discovers all cluster nodes, and then runs hello **cifsMount.sh** script toomount hello file share on each.</span></span>

```azurecli-interactive
#!/bin/bash

# Install jq used for hello next command
sudo apt-get install jq -y

# Get hello IP address of each node using hello mesos API and store it inside a file called nodes
curl http://leader.mesos:1050/system/health/v1/nodes | jq '.nodes[].host_ip' | sed 's/\"//g' | sed '/172/d' > nodes

# From hello previous file created, run our script toomount our share on each node
cat nodes | while read line
do
  ssh `whoami`@$line -o StrictHostKeyChecking=no < ./cifsMount.sh
  done
```

<span data-ttu-id="e003d-141">Kör hello skriptet toomount hello Azure-filresursen på alla noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="e003d-141">Run hello script toomount hello Azure file share on all nodes of hello cluster.</span></span>

```azurecli-interactive
sh ./getNodesRunScript.sh
```  

<span data-ttu-id="e003d-142">hello filresursen är nu tillgänglig på `/mnt/share/dcosshare` på varje nod i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="e003d-142">hello file share is now accessible at `/mnt/share/dcosshare` on each node of hello cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e003d-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e003d-143">Next steps</span></span>

<span data-ttu-id="e003d-144">I den här självstudiekursen en Azure har filresursen gjorts tillgängliga tooa DC/OS-kluster med hjälp av hello steg:</span><span class="sxs-lookup"><span data-stu-id="e003d-144">In this tutorial an Azure file share was made available tooa DC/OS cluster using hello steps:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e003d-145">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="e003d-145">Create an Azure storage account</span></span>
> * <span data-ttu-id="e003d-146">Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="e003d-146">Create a file share</span></span>
> * <span data-ttu-id="e003d-147">Montera hello resursen i hello DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="e003d-147">Mount hello share in hello DC/OS cluster</span></span>

<span data-ttu-id="e003d-148">Avancera toohello nästa självstudiekurs toolearn om integrering av registret en Azure-behållare med DC/OS i Azure.</span><span class="sxs-lookup"><span data-stu-id="e003d-148">Advance toohello next tutorial toolearn about integrating an Azure Container Registry with DC/OS in Azure.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="e003d-149">Belastningsutjämna program</span><span class="sxs-lookup"><span data-stu-id="e003d-149">Load balance applications</span></span>](container-service-dcos-acr.md)