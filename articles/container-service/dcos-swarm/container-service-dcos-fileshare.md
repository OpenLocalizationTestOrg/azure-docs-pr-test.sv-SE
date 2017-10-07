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
# <a name="create-and-mount-a-file-share-tooa-dcos-cluster"></a>Skapa och montera en filresursen tooa DC/OS-klustret
Den här självstudiekursen beskrivs hur toocreate en fil dela i Azure och montera på varje agent och huvudserver för hello DC/OS-klustret. Skapa en filresurs gör det enklare tooshare filer i klustret som konfiguration, access, loggar och mycket mer. hello följande aktiviteter utförs i den här självstudiekursen:

> [!div class="checklist"]
> * Skapa ett Azure-lagringskonto
> * Skapa en filresurs
> * Montera hello resursen i hello DC/OS-klustret

Du behöver en ACS-DC /-OS klustret toocomplete hello stegen i den här kursen. Om det behövs, [detta skriptexempel](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) kan skapa en åt dig.

Den här kursen kräver hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-file-share-on-microsoft-azure"></a>Skapa en filresurs på Microsoft Azure

Hello storage-konto och filresursen måste skapas innan du använder en Azure-filresursen med en ACS DC/OS-klustret. Kör följande skript toocreate hello lagring och filresursen hello. Uppdatera hello parametrar med thoes från din miljö.

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

## <a name="mount-hello-share-in-your-cluster"></a>Montera hello resursen i klustret

Därefter hello filresursen behov toobe monteras på varje virtuell dator i klustret. Den här åtgärden har slutförts med verktyget hello-cifs/protokollet. hello montering kan utföras manuellt på varje nod i klustret hello eller genom att köra ett skript mot varje nod i klustret hello.

I det här exemplet två skript körs, en toomount hello Azure resursen och andra toorun skriptet på varje nod i hello DC/OS-klustret.

Först krävs hello Azure lagringskontonamnet och åtkomstnyckeln. Kör följande kommandon tooget hello informationen. Notera vardera, dessa värden används i ett senare steg.

Lagringskontonamn:

```azurecli-interactive
STORAGE_ACCT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCT
```

Åtkomstnyckeln för lagringskontot:

```azurecli-interactive
az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCT --query "[0].value" -o tsv
```

Därefter hämta hello FQDN för hello DC/OS master och lagrar den i en variabel.

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

Kopiera din privata nyckel toohello huvudnod. Den här nyckeln är nödvändiga toocreate en ssh-anslutning med alla noder i klustret hello. Uppdatera hello användarnamn om ett standardvärde används när du skapar hello kluster. 

```azurecli-interactive
scp ~/.ssh/id_rsa azureuser@$FQDN:~/.ssh
```

Skapa en SSH-anslutning med hello master (eller hello första master) på ditt DC/OS-baserade kluster. Uppdatera hello användarnamn om ett standardvärde används när du skapar hello kluster.

```azurecli-interactive
ssh azureuser@$FQDN
```

Skapa en fil med namnet **cifsMount.sh**, och kopiera hello efter innehållet i den. 

Det här skriptet är används toomount hello Azure-filresursen. Uppdatera hello `STORAGE_ACCT_NAME` och `ACCESS_KEY` variabler med hello information som samlas in tidigare.

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
Skapa en andra fil som heter **getNodesRunScript.sh** och kopiera hello efter innehållet i hello-filen. 

Det här skriptet identifierar alla noder i klustret och kör sedan hello **cifsMount.sh** toomount skript hello filresurs på varje.

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

Kör hello skriptet toomount hello Azure-filresursen på alla noder i klustret hello.

```azurecli-interactive
sh ./getNodesRunScript.sh
```  

hello filresursen är nu tillgänglig på `/mnt/share/dcosshare` på varje nod i klustret hello.

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen en Azure har filresursen gjorts tillgängliga tooa DC/OS-kluster med hjälp av hello steg:

> [!div class="checklist"]
> * Skapa ett Azure-lagringskonto
> * Skapa en filresurs
> * Montera hello resursen i hello DC/OS-klustret

Avancera toohello nästa självstudiekurs toolearn om integrering av registret en Azure-behållare med DC/OS i Azure.  

> [!div class="nextstepaction"]
> [Belastningsutjämna program](container-service-dcos-acr.md)