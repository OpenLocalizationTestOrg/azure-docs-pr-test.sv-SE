---
title: "aaaInstall och konfigurera Ansible för användning med virtuella Azure-datorer | Microsoft Docs"
description: "Lär dig hur tooinstall och konfigurera Ansible för att hantera Azure-resurser på Ubuntu och CentOS SLES"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2017
ms.author: iainfou
ms.openlocfilehash: b33d1893909b6134a5474617c9af2d6e4f627c05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-ansible-toomanage-virtual-machines-in-azure"></a>Installera och konfigurera Ansible toomanage virtuella datorer i Azure
Den här artikeln beskrivs hur hello tooinstall Ansible och Azure SDK för Python-moduler som krävs för några av de vanligaste Linux-distributioner. Du kan installera Ansible på andra distributioner genom att justera hello installerade paket toofit en viss plattform. toocreate Azure resurser på ett säkert sätt du också lära dig hur toocreate och definiera autentiseringsuppgifter för Ansible toouse. 

Flera installationsalternativ och anvisningar för ytterligare plattformar finns hello [Ansible installationsguide](https://docs.ansible.com/ansible/intro_installation.html).


## <a name="install-ansible"></a>Installera Ansible
Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroupAnsible* i hello *eastus* plats:

```azurecli
az group create --name myResourceGroupAnsible --location eastus
```

Nu skapa en virtuell dator och installera Ansible för en hello följande distributioner:

- [Ubuntu 16.04 LTS](#ubuntu1604-lts)
- [CentOS 7.3](#centos-73)
- [SLES 12,2 SP2](#sles-122-sp2)

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS
Skapa en virtuell dator med [az vm create](/cli/azure/vm#create). hello följande exempel skapas en virtuell dator med namnet *myVMAnsible*:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH tooyour VM med hello `publicIpAddress` anges i hello utdata från hello VM Skapa-åtgärd:

```bash
ssh azureuser@<publicIpAddress>
```

På den virtuella datorn krävs hello paket installeras för hello Azure Python SDK-moduler och Ansible på följande sätt:

```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Azure SDKs via pip
pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via apt
sudo apt-get install -y software-properties-common
sudo apt-add-repository -y ppa:ansible/ansible
sudo apt-get update && sudo apt-get install -y ansible
```

Gå nu vidare för[skapa Azure-autentiseringsuppgifterna](#create-azure-credentials).


### <a name="centos-73"></a>CentOS 7.3
Skapa en virtuell dator med [az vm create](/cli/azure/vm#create). hello följande exempel skapas en virtuell dator med namnet *myVMAnsible*:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH tooyour VM med hello `publicIpAddress` anges i hello utdata från hello VM Skapa-åtgärd:

```bash
ssh azureuser@<publicIpAddress>
```

På den virtuella datorn krävs hello paket installeras för hello Azure Python SDK-moduler och Ansible på följande sätt:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Azure SDKs via pip
sudo pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via yum
sudo yum install -y ansible
```

Gå nu vidare för[skapa Azure-autentiseringsuppgifterna](#create-azure-credentials).


### <a name="sles-122-sp2"></a>SLES 12,2 SP2
Skapa en virtuell dator med [az vm create](/cli/azure/vm#create). hello följande exempel skapas en virtuell dator med namnet *myVMAnsible*:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image SLES \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH tooyour VM med hello `publicIpAddress` anges i hello utdata från hello VM Skapa-åtgärd:

```bash
ssh azureuser@<publicIpAddress>
```

På den virtuella datorn krävs hello paket installeras för hello Azure Python SDK-moduler och Ansible på följande sätt:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 python-devel \
    libopenssl-devel python-pip python-setuptools python-azure-sdk

## Install Ansible via zypper
sudo zypper addrepo http://download.opensuse.org/repositories/systemsmanagement/SLE_12_SP2/systemsmanagement.repo
sudo zypper refresh && sudo zypper install ansible
```

Gå nu vidare för[skapa Azure-autentiseringsuppgifterna](#create-azure-credentials).


## <a name="create-azure-credentials"></a>Skapa autentiseringsuppgifter för Azure
Ansible kommunicerar med Azure med ett användarnamn och lösenord eller ett huvudnamn för tjänsten. En Azure-tjänstens huvudnamn är en säkerhetsidentitet som du kan använda med appar, tjänster och verktyg för automatisering som Ansible. Du styr och definiera hello behörigheter toowhat operations hello tjänstens huvudnamn kan utföra i Azure. tooimprove säkerhet över bara att ange ett användarnamn och lösenord, det här exemplet skapar en grundläggande service principal.

Skapa en tjänstens huvudnamn med [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac) och utdata hello autentiseringsuppgifter som Ansible måste:

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

Ett exempel på hello utdata från hello föregående kommandon är följande:

```json
[
  "eec5624a-90f8-4386-8a87-02730b5410d5",
  "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "72f988bf-86f1-41af-91ab-2d7cd011db47"
]
```

tooauthenticate tooAzure du måste också tooobtain din Azure-prenumeration med ID med [az konto visa](/cli/azure/account#show):

```azurecli
az account show --query [id] --output tsv
```

Du kan använda hello utdata från dessa två kommandon i hello nästa steg.


## <a name="create-ansible-credentials-file"></a>Skapa Ansible autentiseringsuppgifter fil
tooprovide autentiseringsuppgifter tooAnsible kan du definiera miljövariabler eller skapa en fil med lokala autentiseringsuppgifter. Mer information om hur toodefine Ansible autentiseringsuppgifter finns [att tillhandahålla autentiseringsuppgifter tooAzure moduler](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules). 

En utvecklingsmiljö kan skapa en *autentiseringsuppgifter* filen för Ansible på din Virtuella värddator, enligt följande:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

Hej *autentiseringsuppgifter* själva filen kombinerar hello prenumerations-ID med hello utdata för att skapa ett huvudnamn för tjänsten. Utdata från hello tidigare [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac) kommandot är hello samma ordning som behövs för *client_id*, *hemlighet*, och *klient* . Hej följande exempel *autentiseringsuppgifter* filen visar värdena matchar hello tidigare utdata. Ange egna värden enligt följande:

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
secret=b8326643-f7e9-48fb-b0d5-952b68ab3def
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```


## <a name="use-ansible-environment-variables"></a>Använda Ansible miljövariabler
Om du ska toouse verktyg som Ansible torn eller Jenkins, kan du definiera miljövariabler på följande sätt. Dessa variabler kombinera hello prenumerations-ID med hello utdata från att skapa en tjänst huvudnamn. Utdata från hello tidigare [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac) kommandot är hello samma ordning som behövs för *AZURE_CLIENT_ID*, *AZURE_SECRET*, och *AZURE_ KLIENT*. 

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
export AZURE_SECRET=8326643-f7e9-48fb-b0d5-952b68ab3def
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a>Nästa steg
Nu har du Ansible och hello Azure Python SDK moduler som har installerats och autentiseringsuppgifter som har definierats för Ansible toouse som krävs. Lär dig hur för[skapa en virtuell dator med Ansible](ansible-create-vm.md). Du kan också lära dig hur för[skapa en fullständig Azure-VM och stödja resurser med Ansible](ansible-create-complete-vm.md).
