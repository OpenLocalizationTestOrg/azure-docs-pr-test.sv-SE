---
title: aaaDeploy OpenShift ursprung tooAzure | Microsoft Docs
description: "Lär dig toodeploy OpenShift ursprung tooAzure virtuella datorer."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jbinder
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: jbinder
ms.openlocfilehash: a67450c46da41134a5f6c669a9e54e14773ac5b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-openshift-origin-tooazure-virtual-machines"></a>Distribuera OpenShift ursprung tooAzure virtuella datorer 

[OpenShift ursprung](https://www.openshift.org/) är en öppen källkod behållaren plattform som bygger på [Kubernetes](https://kubernetes.io/). Det förenklar hello processen för distribution, skalning och användning av program med flera klienter. 

Den här guiden beskriver hur toodeploy OpenShift ursprung på Azure Virtual Machines med hello Azure CLI och Azure Resource Manager-mallar. I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Skapa en KeyVault toomanage SSH-nycklar för hello OpenShift kluster.
> * Distribuera ett OpenShift kluster på Azure Virtual Machines. 
> * Installera och konfigurera hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello klustret.
> * Anpassa hello OpenShift distribution.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

Denna Snabbstart kräver hello Azure CLI version 2.0.8 eller senare. toofind hello version, kör `az --version`. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="log-in-tooazure"></a>Logga in tooAzure 
Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommandot och följ hello på skärmen anvisningarna eller klicka **prova** toouse moln Shell.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. 

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.

```azurecli 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-key-vault"></a>Skapa en Key Vault-lösning
Skapa en KeyVault toostore hello SSH-nycklar för hello kluster med hello [az keyvault skapa](/cli/azure/keyvault#create) kommando.  

```azurecli 
az keyvault create --resource-group myResourceGroup --name myKeyVault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>Skapa en SSH-nyckel 
En SSH-nyckel är nödvändiga toosecure åtkomst toohello OpenShift ursprung klustret. Skapa SSH-nyckel med hjälp av hello `ssh-keygen` kommando. 
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> hello SSH-nyckel som du skapar får inte ha en lösenfras.

Mer information om SSH-nycklar i Windows, [hur toocreate SSH-nycklar i Windows](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-ssh-private-key-in-key-vault"></a>Lagra privata SSH-nyckeln i Key Vault
Hej OpenShift distributionen använder hello SSH-nyckel som du skapade toosecure åtkomst toohello OpenShift master. tooenable hello distribution toosecurely hämta hello SSH-nyckeln, lagra hello nyckel i Nyckelvalvet med hello följande kommando.

# <a name="enabled-for-template-deployment"></a>Aktiverad för malldistribution
```azurecli
az keyvault secret set --vault-name KeyVaultName --name OpenShiftKey --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a>Skapa ett huvudnamn för tjänsten 
OpenShift kommunicerar med Azure med ett användarnamn och lösenord eller ett huvudnamn för tjänsten. En Azure-tjänstens huvudnamn är en säkerhetsidentitet som du kan använda med appar, tjänster och verktyg för automatisering som OpenShift. Du styr och definiera hello behörigheter toowhat operations hello tjänstens huvudnamn kan utföra i Azure. tooimprove säkerhet över bara att ange ett användarnamn och lösenord, det här exemplet skapar en grundläggande service principal.

Skapa en tjänstens huvudnamn med [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac) och utdata hello autentiseringsuppgifter som OpenShift måste:

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {strong password} \
          --scopes $(az group show --name myResourceGroup --query id)
```
Anteckna hello appId egenskap returnerades från hello-kommandot.
```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {strong password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Skapa inte ett osäkert lösenord.  Följ vägledningen med [regler för lösenord och begränsningar i Azure AD](/azure/active-directory/active-directory-passwords-policy).

Mer information om tjänstens huvudnamn finns [skapa en Azure tjänstens huvudnamn med Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

## <a name="deploy-hello-openshift-origin-template"></a>Distribuera hello OpenShift ursprung mall
Distribuera bredvid OpenShift ursprung med en Azure Resource Manager-mall. 

> [!NOTE] 
> hello följande kommando kräver az CLI 2.0.8 eller senare. Du kan verifiera hello az CLI version med hello `az --version` kommando. tooupdate hello CLI-versionen finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).

Använd hello `appId` värde från hello tjänstens huvudnamn som du skapade tidigare för hello `aadClientId` parameter.

```azurecli 
az group deployment create --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --params \ 
        openshiftMasterPublicIpDnsLabel=myopenshiftmaster \
        infraLbPublicIpDnsLabel=myopenshiftlb \
        openshiftPassword=Pass@word!
        sshPublicKey=~/.ssh/openshift_rsa.pub \
        keyVaultResourceGroup=myResourceGroup \
        keyVaultName=myKeyVault \
        keyVaultSecret=OpenShiftKey \
        aadClientId={appId} \
        aadClientSecret={strong password} 
```
hello distributionen kan ta too20 minuter toocomplete. hello URL för hello OpenShift konsolen och DNS-namnet på hello OpenShift master är ut toohello terminal när hello distributionen är klar.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ocpadmin@myopenshiftmaster.cloudapp.azure.com"
}
```
## <a name="connect-toohello-openshift-cluster"></a>Ansluta toohello OpenShift kluster
När hello distributionen är klar ansluta toohello OpenShift konsol med hello webbläsare med hello `OpenShift Console Uri`. Du kan också ansluta toohello OpenShift master med hello följande kommando.

```bash
$ ssh ocpadmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Rensa resurser
När du inte längre behövs kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, OpenShift kluster och alla relaterade resurser.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen inlärda så här:
> [!div class="checklist"]
> * Skapa en KeyVault toomanage SSH-nycklar för hello OpenShift kluster.
> * Distribuera ett OpenShift kluster på Azure Virtual Machines. 
> * Installera och konfigurera hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello klustret.

Nu distribueras OpenShift ursprung klustret. Du kan följa OpenShift självstudier toolearn hur toodeploy din första programmet och använda hello OpenShift verktyg. Se [komma igång med OpenShift ursprung](https://docs.openshift.org/latest/getting_started/index.html) tooget igång. 
