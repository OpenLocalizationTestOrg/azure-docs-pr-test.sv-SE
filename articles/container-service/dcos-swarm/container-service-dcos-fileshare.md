---
title: "Filresurs för Azure DC/OS-kluster | Microsoft Docs"
description: "Skapa och montera en filresurs på en DC/OS-klustret i Azure Container Service"
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
ms.openlocfilehash: 549b52bfb0a0268f754da26c6a374b267861f6d0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-and-mount-a-file-share-to-a-dcos-cluster"></a><span data-ttu-id="53ee4-104">Skapa och montera en filresurs på en DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="53ee4-104">Create and mount a file share to a DC/OS cluster</span></span>
<span data-ttu-id="53ee4-105">Den här självstudiekursen beskrivs hur du skapar en filresurs i Azure och montera på varje agent och huvudserver för DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="53ee4-105">This tutorial details how to create a file share in Azure and mount it on each agent and master of the DC/OS cluster.</span></span> <span data-ttu-id="53ee4-106">Skapa en filresurs gör det enklare att dela filer på ditt kluster som konfiguration, access, loggar och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="53ee4-106">Setting up a file share makes it easier to share files across your cluster such as configuration, access, logs, and more.</span></span> <span data-ttu-id="53ee4-107">Följande åtgärder har utförts i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="53ee4-107">The following tasks are completed in this tutorial:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53ee4-108">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="53ee4-108">Create an Azure storage account</span></span>
> * <span data-ttu-id="53ee4-109">Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="53ee4-109">Create a file share</span></span>
> * <span data-ttu-id="53ee4-110">Montera filresursen i DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="53ee4-110">Mount the share in the DC/OS cluster</span></span>

<span data-ttu-id="53ee4-111">Du behöver en ACS DC/OS-klustret för att slutföra stegen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="53ee4-111">You need an ACS DC/OS cluster to complete the steps in this tutorial.</span></span> <span data-ttu-id="53ee4-112">Om det behövs, [detta skriptexempel](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) kan skapa en åt dig.</span><span class="sxs-lookup"><span data-stu-id="53ee4-112">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="53ee4-113">För den här självstudien krävs Azure CLI-version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="53ee4-113">This tutorial requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="53ee4-114">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="53ee4-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="53ee4-115">Om du behöver uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="53ee4-115">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-file-share-on-microsoft-azure"></a><span data-ttu-id="53ee4-116">Skapa en filresurs på Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="53ee4-116">Create a file share on Microsoft Azure</span></span>

<span data-ttu-id="53ee4-117">Storage-konto och filresursen måste skapas innan du använder en Azure-filresursen med en ACS DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="53ee4-117">Before using an Azure file share with an ACS DC/OS cluster, the storage account and file share must be created.</span></span> <span data-ttu-id="53ee4-118">Kör följande skript för att skapa lagring och filresursen.</span><span class="sxs-lookup"><span data-stu-id="53ee4-118">Run the following script to create the storage and file share.</span></span> <span data-ttu-id="53ee4-119">Uppdatera parametrar med thoes från din miljö.</span><span class="sxs-lookup"><span data-stu-id="53ee4-119">Update the parameters with thoes from your environment.</span></span>

```azurecli-interactive
# Change these four parameters
DCOS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
DCOS_PERS_RESOURCE_GROUP=myResourceGroup
DCOS_PERS_LOCATION=eastus
DCOS_PERS_SHARE_NAME=dcosshare

# Create the storage account with the parameters
az storage account create -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -l $DCOS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -o tsv`

# Create the share
az storage share create -n $DCOS_PERS_SHARE_NAME
```

## <a name="mount-the-share-in-your-cluster"></a><span data-ttu-id="53ee4-120">Montera filresursen i klustret</span><span class="sxs-lookup"><span data-stu-id="53ee4-120">Mount the share in your cluster</span></span>

<span data-ttu-id="53ee4-121">Därefter måste filresursen vara monterad på varje virtuell dator i klustret.</span><span class="sxs-lookup"><span data-stu-id="53ee4-121">Next, the file share needs to be mounted on every virtual machine inside your cluster.</span></span> <span data-ttu-id="53ee4-122">Den här åtgärden har slutförts med verktyget cifs/protokollet.</span><span class="sxs-lookup"><span data-stu-id="53ee4-122">This task is completed using the cifs tool/protocol.</span></span> <span data-ttu-id="53ee4-123">Monteringen kan utföras manuellt på varje nod i klustret eller genom att köra ett skript mot varje nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="53ee4-123">The mount operation can be completed manually on each node of the cluster, or by running a script against each node in the cluster.</span></span>

<span data-ttu-id="53ee4-124">I det här exemplet två skript körs, en för att montera Azure-filresursen och en andra köra skriptet på varje nod i DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="53ee4-124">In this example, two scripts are run, one to mount the Azure file share, and a second to run this script on each node of the DC/OS cluster.</span></span>

<span data-ttu-id="53ee4-125">Först krävs Azure lagringskontonamnet och åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="53ee4-125">First, the Azure storage account name, and access key are needed.</span></span> <span data-ttu-id="53ee4-126">Kör följande kommandon för att hämta informationen.</span><span class="sxs-lookup"><span data-stu-id="53ee4-126">Run the following commands to get this information.</span></span> <span data-ttu-id="53ee4-127">Notera vardera, dessa värden används i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="53ee4-127">Take note of each, these values are used in a later step.</span></span>

<span data-ttu-id="53ee4-128">Lagringskontonamn:</span><span class="sxs-lookup"><span data-stu-id="53ee4-128">Storage account name:</span></span>

```azurecli-interactive
STORAGE_ACCT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCT
```

<span data-ttu-id="53ee4-129">Åtkomstnyckeln för lagringskontot:</span><span class="sxs-lookup"><span data-stu-id="53ee4-129">Storage account access key:</span></span>

```azurecli-interactive
az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCT --query "[0].value" -o tsv
```

<span data-ttu-id="53ee4-130">Sedan hämta det fullständiga Domännamnet för DC/OS-hanteraren och lagrar den i en variabel.</span><span class="sxs-lookup"><span data-stu-id="53ee4-130">Next, get the FQDN of the DC/OS master and store it in a variable.</span></span>

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

<span data-ttu-id="53ee4-131">Kopiera din privata nyckel till huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="53ee4-131">Copy your private key to the master node.</span></span> <span data-ttu-id="53ee4-132">Den här nyckeln behövs för att skapa en ssh-anslutning med alla noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="53ee4-132">This key is needed to create an ssh connection with all nodes in the cluster.</span></span> <span data-ttu-id="53ee4-133">Uppdatera användarnamnet om ett standardvärde används när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="53ee4-133">Update the user name if a non-default value was used when creating the cluster.</span></span> 

```azurecli-interactive
scp ~/.ssh/id_rsa azureuser@$FQDN:~/.ssh
```

<span data-ttu-id="53ee4-134">Skapa en SSH-anslutning med huvudservern (eller den första huvudservern) på ditt DC/OS-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="53ee4-134">Create an SSH connection with the master (or the first master) of your DC/OS-based cluster.</span></span> <span data-ttu-id="53ee4-135">Uppdatera användarnamnet om ett standardvärde används när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="53ee4-135">Update the user name if a non-default value was used when creating the cluster.</span></span>

```azurecli-interactive
ssh azureuser@$FQDN
```

<span data-ttu-id="53ee4-136">Skapa en fil med namnet **cifsMount.sh**, och kopiera följande innehåll till den.</span><span class="sxs-lookup"><span data-stu-id="53ee4-136">Create a file named **cifsMount.sh**, and copy the following contents into it.</span></span> 

<span data-ttu-id="53ee4-137">Det här skriptet för att montera Azure-filresursen.</span><span class="sxs-lookup"><span data-stu-id="53ee4-137">This script is used to mount the Azure file share.</span></span> <span data-ttu-id="53ee4-138">Uppdatering av `STORAGE_ACCT_NAME` och `ACCESS_KEY` variabler med informationen som samlas in tidigare.</span><span class="sxs-lookup"><span data-stu-id="53ee4-138">Update the `STORAGE_ACCT_NAME` and `ACCESS_KEY` variables with the information collected earlier.</span></span>

```azurecli-interactive
#!/bin/bash

# Azure storage account name and access key
STORAGE_ACCT_NAME=mystorageaccount
ACCESS_KEY=mystorageaccountKey

# Install the cifs utils, should be already installed
sudo apt-get update && sudo apt-get -y install cifs-utils

# Create the local folder that will contain our share
if [ ! -d "/mnt/share/dcosshare" ]; then sudo mkdir -p "/mnt/share/dcosshare" ; fi

# Mount the share under the previous local folder created
sudo mount -t cifs //$STORAGE_ACCT_NAME.file.core.windows.net/dcosshare /mnt/share/dcosshare -o vers=3.0,username=$STORAGE_ACCT_NAME,password=$ACCESS_KEY,dir_mode=0777,file_mode=0777
```
<span data-ttu-id="53ee4-139">Skapa en andra fil som heter **getNodesRunScript.sh** och kopiera följande innehåll i filen.</span><span class="sxs-lookup"><span data-stu-id="53ee4-139">Create a second file named **getNodesRunScript.sh** and copy the following contents into the file.</span></span> 

<span data-ttu-id="53ee4-140">Det här skriptet identifierar alla noder i klustret och kör sedan den **cifsMount.sh** skript för att montera filresursen på varje.</span><span class="sxs-lookup"><span data-stu-id="53ee4-140">This script discovers all cluster nodes, and then runs the **cifsMount.sh** script to mount the file share on each.</span></span>

```azurecli-interactive
#!/bin/bash

# Install jq used for the next command
sudo apt-get install jq -y

# Get the IP address of each node using the mesos API and store it inside a file called nodes
curl http://leader.mesos:1050/system/health/v1/nodes | jq '.nodes[].host_ip' | sed 's/\"//g' | sed '/172/d' > nodes

# From the previous file created, run our script to mount our share on each node
cat nodes | while read line
do
  ssh `whoami`@$line -o StrictHostKeyChecking=no < ./cifsMount.sh
  done
```

<span data-ttu-id="53ee4-141">Kör skriptet för att ansluta till Azure-filresursen på alla noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="53ee4-141">Run the script to mount the Azure file share on all nodes of the cluster.</span></span>

```azurecli-interactive
sh ./getNodesRunScript.sh
```  

<span data-ttu-id="53ee4-142">Filresursen är nu tillgänglig på `/mnt/share/dcosshare` på varje nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="53ee4-142">The file share is now accessible at `/mnt/share/dcosshare` on each node of the cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="53ee4-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53ee4-143">Next steps</span></span>

<span data-ttu-id="53ee4-144">Filresursen har gjorts tillgängliga för ett DC/OS-kluster med hjälp av stegen i den här självstudiekursen en Azure:</span><span class="sxs-lookup"><span data-stu-id="53ee4-144">In this tutorial an Azure file share was made available to a DC/OS cluster using the steps:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53ee4-145">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="53ee4-145">Create an Azure storage account</span></span>
> * <span data-ttu-id="53ee4-146">Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="53ee4-146">Create a file share</span></span>
> * <span data-ttu-id="53ee4-147">Montera filresursen i DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="53ee4-147">Mount the share in the DC/OS cluster</span></span>

<span data-ttu-id="53ee4-148">Gå vidare till nästa kurs att lära dig om att integrera en registret för Azure-behållare med DC/OS i Azure.</span><span class="sxs-lookup"><span data-stu-id="53ee4-148">Advance to the next tutorial to learn about integrating an Azure Container Registry with DC/OS in Azure.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="53ee4-149">Belastningsutjämna program</span><span class="sxs-lookup"><span data-stu-id="53ee4-149">Load balance applications</span></span>](container-service-dcos-acr.md)