---
title: "aaaService huvudnamn för Azure Kubernetes klustret | Microsoft Docs"
description: "Skapa och hantera ett tjänstobjekt för Azure Active Directory för ett Kubernetes-kluster i Azure Container Service"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 7a01624c5ac3fa717dbcbd570e05ceb4d917c53a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-an-azure-ad-service-principal-for-a-kubernetes-cluster-in-container-service"></a><span data-ttu-id="e197a-103">Konfigurera ett Azure AD-tjänstobjekt för ett Kubernetes-kluster i Container Service</span><span class="sxs-lookup"><span data-stu-id="e197a-103">Set up an Azure AD service principal for a Kubernetes cluster in Container Service</span></span>


<span data-ttu-id="e197a-104">I Azure Container Service en Kubernetes klustret kräver en [Azure Active Directory-tjänstens huvudnamn](../../active-directory/develop/active-directory-application-objects.md) toointeract med Azure API: er.</span><span class="sxs-lookup"><span data-stu-id="e197a-104">In Azure Container Service, a Kubernetes cluster requires an [Azure Active Directory service principal](../../active-directory/develop/active-directory-application-objects.md) toointeract with Azure APIs.</span></span> <span data-ttu-id="e197a-105">hello tjänstens huvudnamn behövs toodynamically hantera resurser som [användardefinierade vägar](../../virtual-network/virtual-networks-udr-overview.md) och hello [lager 4 Azure belastningsutjämnare](../../load-balancer/load-balancer-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e197a-105">hello service principal is needed toodynamically manage resources such as [user-defined routes](../../virtual-network/virtual-networks-udr-overview.md) and hello [Layer 4 Azure Load Balancer](../../load-balancer/load-balancer-overview.md).</span></span> 


<span data-ttu-id="e197a-106">Den här artikeln visar olika alternativ tooset upp en tjänst huvudnamn för Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="e197a-106">This article shows different options tooset up a service principal for your Kubernetes cluster.</span></span> <span data-ttu-id="e197a-107">Om du har installerat och konfigurerat hello exempelvis [Azure CLI 2.0](/cli/azure/install-az-cli2), kan du köra hello [ `az acs create` ](/cli/azure/acs#create) kommandot toocreate hello Kubernetes klustret och hello tjänstens huvudnamn på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="e197a-107">For example, if you installed and set up hello [Azure CLI 2.0](/cli/azure/install-az-cli2), you can run hello [`az acs create`](/cli/azure/acs#create) command toocreate hello Kubernetes cluster and hello service principal at hello same time.</span></span>


## <a name="requirements-for-hello-service-principal"></a><span data-ttu-id="e197a-108">Krav för hello tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="e197a-108">Requirements for hello service principal</span></span>

<span data-ttu-id="e197a-109">Du kan använda en befintlig Azure AD-tjänstobjekt som uppfyller hello följande krav eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="e197a-109">You can use an existing Azure AD service principal that meets hello following requirements, or create a new one.</span></span>

* <span data-ttu-id="e197a-110">**Scope**: hello resursgrupp i hello prenumerationen används toodeploy hello Kubernetes klustret eller (mindre restriktivt) hello prenumerationen används toodeploy hello klustret.</span><span class="sxs-lookup"><span data-stu-id="e197a-110">**Scope**: hello resource group in hello subscription used toodeploy hello Kubernetes cluster, or (less restrictively) hello subscription used toodeploy hello cluster.</span></span>

* <span data-ttu-id="e197a-111">**Roll**: **Deltagare**</span><span class="sxs-lookup"><span data-stu-id="e197a-111">**Role**: **Contributor**</span></span>

* <span data-ttu-id="e197a-112">**Klienthemlighet**: måste vara ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="e197a-112">**Client secret**: must be a password.</span></span> <span data-ttu-id="e197a-113">För närvarande kan du inte använda ett huvudnamn för tjänsten som konfigurerats för certifikatautentisering.</span><span class="sxs-lookup"><span data-stu-id="e197a-113">Currently, you can't use a service principal set up for certificate authentication.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="e197a-114">toocreate en tjänstens huvudnamn, måste du ha behörigheter tooregister ett program med Azure AD-klient och tooassign hello tooa programrollen i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e197a-114">toocreate a service principal, you must have permissions tooregister an application with your Azure AD tenant, and tooassign hello application tooa role in your subscription.</span></span> <span data-ttu-id="e197a-115">toosee om du har hello krävs behörighet [checka in hello Portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions).</span><span class="sxs-lookup"><span data-stu-id="e197a-115">toosee if you have hello required permissions, [check in hello Portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions).</span></span> 
>

## <a name="option-1-create-a-service-principal-in-azure-ad"></a><span data-ttu-id="e197a-116">Alternativ 1: Skapa ett tjänstobjekt i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e197a-116">Option 1: Create a service principal in Azure AD</span></span>

<span data-ttu-id="e197a-117">Om du vill toocreate ett huvudnamn för Azure AD-tjänsten innan du distribuerar Kubernetes klustret tillhandahåller Azure flera olika metoder.</span><span class="sxs-lookup"><span data-stu-id="e197a-117">If you want toocreate an Azure AD service principal before you deploy your Kubernetes cluster, Azure provides several methods.</span></span> 

<span data-ttu-id="e197a-118">hello följande exempelkommandon visar hur toodo detta med hello [Azure CLI 2.0](../../azure-resource-manager/resource-group-authenticate-service-principal-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e197a-118">hello following example commands show you how toodo this with hello [Azure CLI 2.0](../../azure-resource-manager/resource-group-authenticate-service-principal-cli.md).</span></span> <span data-ttu-id="e197a-119">Du kan också skapa ett service principal med [Azure PowerShell](../../azure-resource-manager/resource-group-authenticate-service-principal.md), hello [portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md), eller andra metoder.</span><span class="sxs-lookup"><span data-stu-id="e197a-119">You can alternatively create a service principal using [Azure PowerShell](../../azure-resource-manager/resource-group-authenticate-service-principal.md), hello [portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md), or other methods.</span></span>

```azurecli
az login

az account set --subscription "mySubscriptionID"

az group create -n "myResourceGroupName" -l "westus"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID/resourceGroups/myResourceGroupName"
```

<span data-ttu-id="e197a-120">Utdata är liknande toohello följande (visas här omarbetade):</span><span class="sxs-lookup"><span data-stu-id="e197a-120">Output is similar toohello following (shown here redacted):</span></span>

![Skapa ett huvudnamn för tjänsten](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

<span data-ttu-id="e197a-122">Markerat är hello **klient-ID** (`appId`) och hello **klienthemlighet** (`password`) som du använder som service principal parametrar för klusterdistribution.</span><span class="sxs-lookup"><span data-stu-id="e197a-122">Highlighted are hello **client ID** (`appId`) and hello **client secret** (`password`) that you use as service principal parameters for cluster deployment.</span></span>


### <a name="specify-service-principal-when-creating-hello-kubernetes-cluster"></a><span data-ttu-id="e197a-123">Ange tjänstens huvudnamn när du skapar hello Kubernetes kluster</span><span class="sxs-lookup"><span data-stu-id="e197a-123">Specify service principal when creating hello Kubernetes cluster</span></span>

<span data-ttu-id="e197a-124">Ange hello **klient-ID** (kallas även hello `appId`, för program-ID) och **klienthemlighet** (`password`) en befintlig tjänstens huvudnamn som parametrar när du skapar hello Kubernetes kluster.</span><span class="sxs-lookup"><span data-stu-id="e197a-124">Provide hello **client ID** (also called hello `appId`, for Application ID) and **client secret** (`password`) of an existing service principal as parameters when you create hello Kubernetes cluster.</span></span> <span data-ttu-id="e197a-125">Kontrollera att hello tjänstens huvudnamn uppfyller hello på hello från den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e197a-125">Make sure hello service principal meets hello requirements at hello beginning this article.</span></span>

<span data-ttu-id="e197a-126">Du kan ange dessa parametrar när du distribuerar hello Kubernetes kluster med hjälp av hello [Azure kommandoradsgränssnittet (CLI) 2.0](container-service-kubernetes-walkthrough.md), [Azure-portalen](../dcos-swarm/container-service-deployment.md), eller andra metoder.</span><span class="sxs-lookup"><span data-stu-id="e197a-126">You can specify these parameters when deploying hello Kubernetes cluster using hello [Azure Command-Line Interface (CLI) 2.0](container-service-kubernetes-walkthrough.md), [Azure portal](../dcos-swarm/container-service-deployment.md), or other methods.</span></span>

>[!TIP] 
><span data-ttu-id="e197a-127">När du anger hello **klient-ID**, vara säker på att toouse hello `appId`, inte hello `ObjectId`, av hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="e197a-127">When specifying hello **client ID**, be sure toouse hello `appId`, not hello `ObjectId`, of hello service principal.</span></span>
>

<span data-ttu-id="e197a-128">hello följande exempel visas ett sätt toopass hello parametrar med hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="e197a-128">hello following example shows one way toopass hello parameters with hello Azure CLI 2.0.</span></span> <span data-ttu-id="e197a-129">Det här exemplet används hello [Kubernetes quickstart mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).</span><span class="sxs-lookup"><span data-stu-id="e197a-129">This example uses hello [Kubernetes quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).</span></span>

1. <span data-ttu-id="e197a-130">[Hämta](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) hello mallfilen parametrar `azuredeploy.parameters.json` från GitHub.</span><span class="sxs-lookup"><span data-stu-id="e197a-130">[Download](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) hello template parameters file `azuredeploy.parameters.json` from GitHub.</span></span>

2. <span data-ttu-id="e197a-131">toospecify hello tjänstens huvudnamn, ange värden för `servicePrincipalClientId` och `servicePrincipalClientSecret` i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="e197a-131">toospecify hello service principal, enter values for `servicePrincipalClientId` and `servicePrincipalClientSecret` in hello file.</span></span> <span data-ttu-id="e197a-132">(Du måste också tooprovide egna värden för `dnsNamePrefix` och `sshRSAPublicKey`.</span><span class="sxs-lookup"><span data-stu-id="e197a-132">(You also need tooprovide your own values for `dnsNamePrefix` and `sshRSAPublicKey`.</span></span> <span data-ttu-id="e197a-133">hello senare är hello SSH offentlig nyckel tooaccess hello kluster.) Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="e197a-133">hello latter is hello SSH public key tooaccess hello cluster.) Save hello file.</span></span>

    ![Skicka parametrar för tjänstens huvudnamn](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. <span data-ttu-id="e197a-135">Kör hello följande kommando, med `--parameters` tooset hello sökväg toohello azuredeploy.parameters.json fil.</span><span class="sxs-lookup"><span data-stu-id="e197a-135">Run hello following command, using `--parameters` tooset hello path toohello azuredeploy.parameters.json file.</span></span> <span data-ttu-id="e197a-136">Det här kommandot distribuerar hello-kluster i en resursgrupp som du skapar kallas `myResourceGroup` i hello västra USA region.</span><span class="sxs-lookup"><span data-stu-id="e197a-136">This command deploys hello cluster in a resource group you create called `myResourceGroup` in hello West US region.</span></span>

    ```azurecli
    az login

    az account set --subscription "mySubscriptionID"

    az group create --name "myResourceGroup" --location "westus" 
    
    az group deployment create -g "myResourceGroup" --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" --parameters @azuredeploy.parameters.json
    ```


## <a name="option-2-generate-a-service-principal-when-creating-hello-cluster-with-az-acs-create"></a><span data-ttu-id="e197a-137">Alternativ 2: Skapa ett huvudnamn för tjänsten när du skapar hello kluster med`az acs create`</span><span class="sxs-lookup"><span data-stu-id="e197a-137">Option 2: Generate a service principal when creating hello cluster with `az acs create`</span></span>

<span data-ttu-id="e197a-138">Om du kör hello [ `az acs create` ](/cli/azure/acs#create) kommandot toocreate hello Kubernetes klustret måste du hello alternativet toogenerate ett huvudnamn för tjänsten automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e197a-138">If you run hello [`az acs create`](/cli/azure/acs#create) command toocreate hello Kubernetes cluster, you have hello option toogenerate a service principal automatically.</span></span>

<span data-ttu-id="e197a-139">Som med andra alternativ för att skapa Kubernetes-kluster, kan du ange parametrar för en befintlig tjänsts huvudnamn när du kör `az acs create`.</span><span class="sxs-lookup"><span data-stu-id="e197a-139">As with other Kubernetes cluster creation options, you can specify parameters for an existing service principal when you run `az acs create`.</span></span> <span data-ttu-id="e197a-140">Men om du utelämnar parametrarna skapar hello Azure CLI en automatiskt för användning med Container Service.</span><span class="sxs-lookup"><span data-stu-id="e197a-140">However, when you omit these parameters, hello Azure CLI creates one automatically for use with Container Service.</span></span> <span data-ttu-id="e197a-141">Detta sker transparent under hello distributionen.</span><span class="sxs-lookup"><span data-stu-id="e197a-141">This takes place transparently during hello deployment.</span></span> 

<span data-ttu-id="e197a-142">hello följande kommando skapar ett Kubernetes kluster och genererar både SSH-nycklar och tjänstens huvudnamn autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="e197a-142">hello following command creates a Kubernetes cluster and generates both SSH keys and service principal credentials:</span></span>

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

> [!IMPORTANT]
> <span data-ttu-id="e197a-143">Om ditt konto inte har hello Azure AD och prenumeration behörigheter toocreate ett huvudnamn för tjänsten, genererar hello kommandot ett felmeddelande för`Insufficient privileges toocomplete hello operation.`</span><span class="sxs-lookup"><span data-stu-id="e197a-143">If your account doesn't have hello Azure AD and subscription permissions toocreate a service principal, hello command generates an error similar too`Insufficient privileges toocomplete hello operation.`</span></span>
> 

## <a name="additional-considerations"></a><span data-ttu-id="e197a-144">Annat som är bra att tänka på</span><span class="sxs-lookup"><span data-stu-id="e197a-144">Additional considerations</span></span>

* <span data-ttu-id="e197a-145">Om du inte har behörigheter toocreate ett huvudnamn för tjänsten i din prenumeration kan du behöva tooask din Azure AD eller prenumeration administratören tooassign hello behörighet och be dem att ge en service principal toouse med Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="e197a-145">If you don't have permissions toocreate a service principal in your subscription, you might need tooask your Azure AD or subscription administrator tooassign hello necessary permissions, or ask them for a service principal toouse with Azure Container Service.</span></span> 

* <span data-ttu-id="e197a-146">hello tjänstens huvudnamn för Kubernetes är en del av hello klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="e197a-146">hello service principal for Kubernetes is a part of hello cluster configuration.</span></span> <span data-ttu-id="e197a-147">Dock inte använda hello identitet toodeploy hello kluster.</span><span class="sxs-lookup"><span data-stu-id="e197a-147">However, don't use hello identity toodeploy hello cluster.</span></span>

* <span data-ttu-id="e197a-148">Varje tjänstobjekt är associerat med ett Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="e197a-148">Every service principal is associated with an Azure AD application.</span></span> <span data-ttu-id="e197a-149">hello tjänstens huvudnamn för ett kluster med Kubernetes kan vara kopplad till någon giltig Azure AD-programnamn (till exempel: `https://www.contoso.org/example`).</span><span class="sxs-lookup"><span data-stu-id="e197a-149">hello service principal for a Kubernetes cluster can be associated with any valid Azure AD application name (for example: `https://www.contoso.org/example`).</span></span> <span data-ttu-id="e197a-150">hello-URL för programmet hello saknar toobe en verklig slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e197a-150">hello URL for hello application doesn't have toobe a real endpoint.</span></span>

* <span data-ttu-id="e197a-151">Ange när hello tjänstens huvudnamn **klient-ID**, kan du använda hello värde av hello `appId` (som visas i den här artikeln) eller hello motsvarande tjänstens huvudnamn `name` (till exempel`https://www.contoso.org/example`).</span><span class="sxs-lookup"><span data-stu-id="e197a-151">When specifying hello service principal **Client ID**, you can use hello value of hello `appId` (as shown in this article) or hello corresponding service principal `name` (for example,`https://www.contoso.org/example`).</span></span>

* <span data-ttu-id="e197a-152">På hello master och agenten virtuella datorer i hello Kubernetes kluster lagras hello service principal autentiseringsuppgifterna i hello filen /etc/kubernetes/azure.json.</span><span class="sxs-lookup"><span data-stu-id="e197a-152">On hello master and agent VMs in hello Kubernetes cluster, hello service principal credentials are stored in hello file /etc/kubernetes/azure.json.</span></span>

* <span data-ttu-id="e197a-153">När du använder hello `az acs create` kommandot toogenerate hello tjänstens huvudnamn automatiskt, huvudnamn hello Tjänstereferenser skrivs toohello filen ~/.azure/acsServicePrincipal.json på hello datorn används toorun hello kommando.</span><span class="sxs-lookup"><span data-stu-id="e197a-153">When you use hello `az acs create` command toogenerate hello service principal automatically, hello service principal credentials are written toohello file ~/.azure/acsServicePrincipal.json on hello machine used toorun hello command.</span></span> 

* <span data-ttu-id="e197a-154">När du använder hello `az acs create` kommandot toogenerate hello tjänstens huvudnamn automatiskt, hello tjänstens huvudnamn kan också autentisera med ett [Azure-behållaren registret](../../container-registry/container-registry-intro.md) skapas i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e197a-154">When you use hello `az acs create` command toogenerate hello service principal automatically, hello service principal can also authenticate with an [Azure container registry](../../container-registry/container-registry-intro.md) created in hello same subscription.</span></span>




## <a name="next-steps"></a><span data-ttu-id="e197a-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e197a-155">Next steps</span></span>

* <span data-ttu-id="e197a-156">[Kom igång med Kubernetes](container-service-kubernetes-walkthrough.md) i behållaren för tjänsteklustret.</span><span class="sxs-lookup"><span data-stu-id="e197a-156">[Get started with Kubernetes](container-service-kubernetes-walkthrough.md) in your container service cluster.</span></span>

* <span data-ttu-id="e197a-157">tootroubleshoot hello-tjänstens huvudnamn för Kubernetes, se hello [ACS-motorn dokumentationen](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes.md#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="e197a-157">tootroubleshoot hello service principal for Kubernetes, see hello [ACS Engine documentation](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes.md#troubleshooting).</span></span>


