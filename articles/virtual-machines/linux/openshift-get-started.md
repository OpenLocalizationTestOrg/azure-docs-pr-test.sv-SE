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
# <a name="deploy-openshift-origin-tooazure-virtual-machines"></a><span data-ttu-id="e8357-103">Distribuera OpenShift ursprung tooAzure virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e8357-103">Deploy OpenShift Origin tooAzure Virtual Machines</span></span> 

<span data-ttu-id="e8357-104">[OpenShift ursprung](https://www.openshift.org/) är en öppen källkod behållaren plattform som bygger på [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="e8357-104">[OpenShift Origin](https://www.openshift.org/) is an open source container platform built on [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="e8357-105">Det förenklar hello processen för distribution, skalning och användning av program med flera klienter.</span><span class="sxs-lookup"><span data-stu-id="e8357-105">It simplifies hello process of deploying, scaling, and operating multi-tenant applications.</span></span> 

<span data-ttu-id="e8357-106">Den här guiden beskriver hur toodeploy OpenShift ursprung på Azure Virtual Machines med hello Azure CLI och Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="e8357-106">This guide describes how toodeploy OpenShift Origin on Azure Virtual Machines using hello Azure CLI and Azure Resource Manager Templates.</span></span> <span data-ttu-id="e8357-107">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="e8357-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e8357-108">Skapa en KeyVault toomanage SSH-nycklar för hello OpenShift kluster.</span><span class="sxs-lookup"><span data-stu-id="e8357-108">Create a KeyVault toomanage SSH keys for hello OpenShift cluster.</span></span>
> * <span data-ttu-id="e8357-109">Distribuera ett OpenShift kluster på Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="e8357-109">Deploy an OpenShift cluster on Azure VMs.</span></span> 
> * <span data-ttu-id="e8357-110">Installera och konfigurera hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello klustret.</span><span class="sxs-lookup"><span data-stu-id="e8357-110">Install and configure hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello cluster.</span></span>
> * <span data-ttu-id="e8357-111">Anpassa hello OpenShift distribution.</span><span class="sxs-lookup"><span data-stu-id="e8357-111">Customize hello OpenShift deployment.</span></span>

<span data-ttu-id="e8357-112">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="e8357-112">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="e8357-113">Denna Snabbstart kräver hello Azure CLI version 2.0.8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e8357-113">This quick start requires hello Azure CLI version 2.0.8 or later.</span></span> <span data-ttu-id="e8357-114">toofind hello version, kör `az --version`.</span><span class="sxs-lookup"><span data-stu-id="e8357-114">toofind hello version, run `az --version`.</span></span> <span data-ttu-id="e8357-115">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e8357-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="log-in-tooazure"></a><span data-ttu-id="e8357-116">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="e8357-116">Log in tooAzure</span></span> 
<span data-ttu-id="e8357-117">Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommandot och följ hello på skärmen anvisningarna eller klicka **prova** toouse moln Shell.</span><span class="sxs-lookup"><span data-stu-id="e8357-117">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions or click **Try it** toouse Cloud Shell.</span></span>

```azurecli 
az login
```
## <a name="create-a-resource-group"></a><span data-ttu-id="e8357-118">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e8357-118">Create a resource group</span></span>

<span data-ttu-id="e8357-119">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="e8357-119">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="e8357-120">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="e8357-120">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="e8357-121">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.</span><span class="sxs-lookup"><span data-stu-id="e8357-121">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-key-vault"></a><span data-ttu-id="e8357-122">Skapa en Key Vault-lösning</span><span class="sxs-lookup"><span data-stu-id="e8357-122">Create a Key Vault</span></span>
<span data-ttu-id="e8357-123">Skapa en KeyVault toostore hello SSH-nycklar för hello kluster med hello [az keyvault skapa](/cli/azure/keyvault#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="e8357-123">Create a KeyVault toostore hello SSH keys for hello cluster with hello [az keyvault create](/cli/azure/keyvault#create) command.</span></span>  

```azurecli 
az keyvault create --resource-group myResourceGroup --name myKeyVault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a><span data-ttu-id="e8357-124">Skapa en SSH-nyckel</span><span class="sxs-lookup"><span data-stu-id="e8357-124">Create an SSH key</span></span> 
<span data-ttu-id="e8357-125">En SSH-nyckel är nödvändiga toosecure åtkomst toohello OpenShift ursprung klustret.</span><span class="sxs-lookup"><span data-stu-id="e8357-125">An SSH key is needed toosecure access toohello OpenShift Origin cluster.</span></span> <span data-ttu-id="e8357-126">Skapa SSH-nyckel med hjälp av hello `ssh-keygen` kommando.</span><span class="sxs-lookup"><span data-stu-id="e8357-126">Create an SSH key-pair using hello `ssh-keygen` command.</span></span> 
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> <span data-ttu-id="e8357-127">hello SSH-nyckel som du skapar får inte ha en lösenfras.</span><span class="sxs-lookup"><span data-stu-id="e8357-127">hello SSH key pair you create must not have a passphrase.</span></span>

<span data-ttu-id="e8357-128">Mer information om SSH-nycklar i Windows, [hur toocreate SSH-nycklar i Windows](/azure/virtual-machines/linux/ssh-from-windows).</span><span class="sxs-lookup"><span data-stu-id="e8357-128">For more information on SSH keys on Windows, [How toocreate SSH keys on Windows](/azure/virtual-machines/linux/ssh-from-windows).</span></span>

## <a name="store-ssh-private-key-in-key-vault"></a><span data-ttu-id="e8357-129">Lagra privata SSH-nyckeln i Key Vault</span><span class="sxs-lookup"><span data-stu-id="e8357-129">Store SSH private key in Key Vault</span></span>
<span data-ttu-id="e8357-130">Hej OpenShift distributionen använder hello SSH-nyckel som du skapade toosecure åtkomst toohello OpenShift master.</span><span class="sxs-lookup"><span data-stu-id="e8357-130">hello OpenShift deployment uses hello SSH key you created toosecure access toohello OpenShift master.</span></span> <span data-ttu-id="e8357-131">tooenable hello distribution toosecurely hämta hello SSH-nyckeln, lagra hello nyckel i Nyckelvalvet med hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="e8357-131">tooenable hello deployment toosecurely retrieve hello SSH key, store hello key in Key Vault using hello following command.</span></span>

# <a name="enabled-for-template-deployment"></a><span data-ttu-id="e8357-132">Aktiverad för malldistribution</span><span class="sxs-lookup"><span data-stu-id="e8357-132">Enabled for template deployment</span></span>
```azurecli
az keyvault secret set --vault-name KeyVaultName --name OpenShiftKey --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a><span data-ttu-id="e8357-133">Skapa ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="e8357-133">Create a service principal</span></span> 
<span data-ttu-id="e8357-134">OpenShift kommunicerar med Azure med ett användarnamn och lösenord eller ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e8357-134">OpenShift communicates with Azure using a username and password or a service principal.</span></span> <span data-ttu-id="e8357-135">En Azure-tjänstens huvudnamn är en säkerhetsidentitet som du kan använda med appar, tjänster och verktyg för automatisering som OpenShift.</span><span class="sxs-lookup"><span data-stu-id="e8357-135">An Azure service principal is a security identity that you can use with apps, services, and automation tools like OpenShift.</span></span> <span data-ttu-id="e8357-136">Du styr och definiera hello behörigheter toowhat operations hello tjänstens huvudnamn kan utföra i Azure.</span><span class="sxs-lookup"><span data-stu-id="e8357-136">You control and define hello permissions as toowhat operations hello service principal can perform in Azure.</span></span> <span data-ttu-id="e8357-137">tooimprove säkerhet över bara att ange ett användarnamn och lösenord, det här exemplet skapar en grundläggande service principal.</span><span class="sxs-lookup"><span data-stu-id="e8357-137">tooimprove security over just providing a username and password, this example creates a basic service principal.</span></span>

<span data-ttu-id="e8357-138">Skapa en tjänstens huvudnamn med [az ad sp skapa-för-rbac](/cli/azure/ad/sp#create-for-rbac) och utdata hello autentiseringsuppgifter som OpenShift måste:</span><span class="sxs-lookup"><span data-stu-id="e8357-138">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output hello credentials that OpenShift needs:</span></span>

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {strong password} \
          --scopes $(az group show --name myResourceGroup --query id)
```
<span data-ttu-id="e8357-139">Anteckna hello appId egenskap returnerades från hello-kommandot.</span><span class="sxs-lookup"><span data-stu-id="e8357-139">Take note of hello appId property returned from hello command.</span></span>
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
 > <span data-ttu-id="e8357-140">Skapa inte ett osäkert lösenord.</span><span class="sxs-lookup"><span data-stu-id="e8357-140">Don't create an insecure password.</span></span>  <span data-ttu-id="e8357-141">Följ vägledningen med [regler för lösenord och begränsningar i Azure AD](/azure/active-directory/active-directory-passwords-policy).</span><span class="sxs-lookup"><span data-stu-id="e8357-141">Follow the [Azure AD password rules and restrictions](/azure/active-directory/active-directory-passwords-policy) guidance.</span></span>

<span data-ttu-id="e8357-142">Mer information om tjänstens huvudnamn finns [skapa en Azure tjänstens huvudnamn med Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="e8357-142">For more information on service principals, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)</span></span>

## <a name="deploy-hello-openshift-origin-template"></a><span data-ttu-id="e8357-143">Distribuera hello OpenShift ursprung mall</span><span class="sxs-lookup"><span data-stu-id="e8357-143">Deploy hello OpenShift Origin template</span></span>
<span data-ttu-id="e8357-144">Distribuera bredvid OpenShift ursprung med en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="e8357-144">Next deploy OpenShift Origin using an Azure Resource Manager template.</span></span> 

> [!NOTE] 
> <span data-ttu-id="e8357-145">hello följande kommando kräver az CLI 2.0.8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e8357-145">hello following command requires az CLI 2.0.8 or later.</span></span> <span data-ttu-id="e8357-146">Du kan verifiera hello az CLI version med hello `az --version` kommando.</span><span class="sxs-lookup"><span data-stu-id="e8357-146">You can verify hello az CLI version with hello `az --version` command.</span></span> <span data-ttu-id="e8357-147">tooupdate hello CLI-versionen finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e8357-147">tooupdate hello CLI version, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="e8357-148">Använd hello `appId` värde från hello tjänstens huvudnamn som du skapade tidigare för hello `aadClientId` parameter.</span><span class="sxs-lookup"><span data-stu-id="e8357-148">Use hello `appId` value from hello service principal you created earlier for hello `aadClientId` parameter.</span></span>

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
<span data-ttu-id="e8357-149">hello distributionen kan ta too20 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="e8357-149">hello deployment may take up too20 minutes toocomplete.</span></span> <span data-ttu-id="e8357-150">hello URL för hello OpenShift konsolen och DNS-namnet på hello OpenShift master är ut toohello terminal när hello distributionen är klar.</span><span class="sxs-lookup"><span data-stu-id="e8357-150">hello URL of hello OpenShift console and DNS name of hello OpenShift master is printed toohello terminal when hello deployment completes.</span></span>

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ocpadmin@myopenshiftmaster.cloudapp.azure.com"
}
```
## <a name="connect-toohello-openshift-cluster"></a><span data-ttu-id="e8357-151">Ansluta toohello OpenShift kluster</span><span class="sxs-lookup"><span data-stu-id="e8357-151">Connect toohello OpenShift cluster</span></span>
<span data-ttu-id="e8357-152">När hello distributionen är klar ansluta toohello OpenShift konsol med hello webbläsare med hello `OpenShift Console Uri`.</span><span class="sxs-lookup"><span data-stu-id="e8357-152">When hello deployment completes, connect toohello OpenShift console using hello browser using hello `OpenShift Console Uri`.</span></span> <span data-ttu-id="e8357-153">Du kan också ansluta toohello OpenShift master med hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="e8357-153">Alternatively, you can connect toohello OpenShift master using hello following command.</span></span>

```bash
$ ssh ocpadmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a><span data-ttu-id="e8357-154">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="e8357-154">Clean up resources</span></span>
<span data-ttu-id="e8357-155">När du inte längre behövs kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, OpenShift kluster och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="e8357-155">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, OpenShift cluster, and all related resources.</span></span>

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="e8357-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e8357-156">Next steps</span></span>

<span data-ttu-id="e8357-157">I den här självstudiekursen inlärda så här:</span><span class="sxs-lookup"><span data-stu-id="e8357-157">In this tutorial, learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="e8357-158">Skapa en KeyVault toomanage SSH-nycklar för hello OpenShift kluster.</span><span class="sxs-lookup"><span data-stu-id="e8357-158">Create a KeyVault toomanage SSH keys for hello OpenShift cluster.</span></span>
> * <span data-ttu-id="e8357-159">Distribuera ett OpenShift kluster på Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="e8357-159">Deploy an OpenShift cluster on Azure VMs.</span></span> 
> * <span data-ttu-id="e8357-160">Installera och konfigurera hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello klustret.</span><span class="sxs-lookup"><span data-stu-id="e8357-160">Install and configure hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello cluster.</span></span>

<span data-ttu-id="e8357-161">Nu distribueras OpenShift ursprung klustret.</span><span class="sxs-lookup"><span data-stu-id="e8357-161">Now that OpenShift Origin cluster is deployed.</span></span> <span data-ttu-id="e8357-162">Du kan följa OpenShift självstudier toolearn hur toodeploy din första programmet och använda hello OpenShift verktyg.</span><span class="sxs-lookup"><span data-stu-id="e8357-162">You can follow OpenShift tutorials toolearn how toodeploy your first application and use hello OpenShift tools.</span></span> <span data-ttu-id="e8357-163">Se [komma igång med OpenShift ursprung](https://docs.openshift.org/latest/getting_started/index.html) tooget igång.</span><span class="sxs-lookup"><span data-stu-id="e8357-163">See [Getting Started with OpenShift Origin](https://docs.openshift.org/latest/getting_started/index.html) tooget started.</span></span> 
