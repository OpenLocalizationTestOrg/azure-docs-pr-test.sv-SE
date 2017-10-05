---
title: "Tjänstens huvudnamn för Azure Kubernetes-kluster | Microsoft- Docs"
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
ms.openlocfilehash: 37171b4e69ad7d8c41ca8e7475c33ce70379f484
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="set-up-an-azure-ad-service-principal-for-a-kubernetes-cluster-in-container-service"></a><span data-ttu-id="84d67-103">Konfigurera ett Azure AD-tjänstobjekt för ett Kubernetes-kluster i Container Service</span><span class="sxs-lookup"><span data-stu-id="84d67-103">Set up an Azure AD service principal for a Kubernetes cluster in Container Service</span></span>


<span data-ttu-id="84d67-104">I Azure Container Service kräver ett Kubernetes-kluster ett [Azure Active Directory-tjänstobjekt](../../active-directory/develop/active-directory-application-objects.md) för att kunna interagera med Azure-API:er.</span><span class="sxs-lookup"><span data-stu-id="84d67-104">In Azure Container Service, a Kubernetes cluster requires an [Azure Active Directory service principal](../../active-directory/develop/active-directory-application-objects.md) to interact with Azure APIs.</span></span> <span data-ttu-id="84d67-105">Tjänstens huvudnamn krävs för att dynamiskt hantera resurser som [användardefinierade vägar](../../virtual-network/virtual-networks-udr-overview.md) och [lager 4 för Azure Load Balancer](../../load-balancer/load-balancer-overview.md).</span><span class="sxs-lookup"><span data-stu-id="84d67-105">The service principal is needed to dynamically manage resources such as [user-defined routes](../../virtual-network/virtual-networks-udr-overview.md) and the [Layer 4 Azure Load Balancer](../../load-balancer/load-balancer-overview.md).</span></span> 


<span data-ttu-id="84d67-106">Den här artikeln beskriver hur du konfigurerar ett tjänstobjekt för ett Kubernetes-kluster på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="84d67-106">This article shows different options to set up a service principal for your Kubernetes cluster.</span></span> <span data-ttu-id="84d67-107">Om du till exempel har installerat och konfigurerat [Azure CLI 2.0](/cli/azure/install-az-cli2) kan du köra kommandot [`az acs create`](/cli/azure/acs#create) för att skapa Kubernetes-klustret och tjänstobjektet samtidigt.</span><span class="sxs-lookup"><span data-stu-id="84d67-107">For example, if you installed and set up the [Azure CLI 2.0](/cli/azure/install-az-cli2), you can run the [`az acs create`](/cli/azure/acs#create) command to create the Kubernetes cluster and the service principal at the same time.</span></span>


## <a name="requirements-for-the-service-principal"></a><span data-ttu-id="84d67-108">Krav för tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="84d67-108">Requirements for the service principal</span></span>

<span data-ttu-id="84d67-109">Du kan använda ett befintligt Azure AD-tjänstobjekt som uppfyller kraven nedan, eller skapa ett nytt.</span><span class="sxs-lookup"><span data-stu-id="84d67-109">You can use an existing Azure AD service principal that meets the following requirements, or create a new one.</span></span>

* <span data-ttu-id="84d67-110">**Omfång**: Den resursgrupp i prenumerationen som används för att distribuera Kubernetes-klustret, eller (mindre restriktivt) prenumerationen som används för att distribuera klustret.</span><span class="sxs-lookup"><span data-stu-id="84d67-110">**Scope**: the resource group in the subscription used to deploy the Kubernetes cluster, or (less restrictively) the subscription used to deploy the cluster.</span></span>

* <span data-ttu-id="84d67-111">**Roll**: **Deltagare**</span><span class="sxs-lookup"><span data-stu-id="84d67-111">**Role**: **Contributor**</span></span>

* <span data-ttu-id="84d67-112">**Klienthemlighet**: måste vara ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="84d67-112">**Client secret**: must be a password.</span></span> <span data-ttu-id="84d67-113">För närvarande kan du inte använda ett huvudnamn för tjänsten som konfigurerats för certifikatautentisering.</span><span class="sxs-lookup"><span data-stu-id="84d67-113">Currently, you can't use a service principal set up for certificate authentication.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="84d67-114">För att skapa ett tjänstobjekt måste du ha behörighet att registrera ett program med din Azure AD-klientorganisation, samt behörighet att tilldela programmet till en roll i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="84d67-114">To create a service principal, you must have permissions to register an application with your Azure AD tenant, and to assign the application to a role in your subscription.</span></span> <span data-ttu-id="84d67-115">Du kan kontrollera om du har nödvändig behörighet [på portalen](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions).</span><span class="sxs-lookup"><span data-stu-id="84d67-115">To see if you have the required permissions, [check in the Portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions).</span></span> 
>

## <a name="option-1-create-a-service-principal-in-azure-ad"></a><span data-ttu-id="84d67-116">Alternativ 1: Skapa ett tjänstobjekt i Azure AD</span><span class="sxs-lookup"><span data-stu-id="84d67-116">Option 1: Create a service principal in Azure AD</span></span>

<span data-ttu-id="84d67-117">Om du vill skapa ett Azure AD-tjänstobjekt innan du distribuerar Kubernetes-klustret kan du välja mellan flera metoder i Azure.</span><span class="sxs-lookup"><span data-stu-id="84d67-117">If you want to create an Azure AD service principal before you deploy your Kubernetes cluster, Azure provides several methods.</span></span> 

<span data-ttu-id="84d67-118">Kommandona i följande exempel visar hur du gör detta med [Azure CLI 2.0](../../azure-resource-manager/resource-group-authenticate-service-principal-cli.md).</span><span class="sxs-lookup"><span data-stu-id="84d67-118">The following example commands show you how to do this with the [Azure CLI 2.0](../../azure-resource-manager/resource-group-authenticate-service-principal-cli.md).</span></span> <span data-ttu-id="84d67-119">Du kan också skapa ett tjänstobjekt med [Azure PowerShell](../../azure-resource-manager/resource-group-authenticate-service-principal.md), [portalen](../../azure-resource-manager/resource-group-create-service-principal-portal.md) eller andra metoder.</span><span class="sxs-lookup"><span data-stu-id="84d67-119">You can alternatively create a service principal using [Azure PowerShell](../../azure-resource-manager/resource-group-authenticate-service-principal.md), the [portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md), or other methods.</span></span>

```azurecli
az login

az account set --subscription "mySubscriptionID"

az group create -n "myResourceGroupName" -l "westus"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID/resourceGroups/myResourceGroupName"
```

<span data-ttu-id="84d67-120">De utdata som returneras ser ut ungefär så här (redigerat i bilden):</span><span class="sxs-lookup"><span data-stu-id="84d67-120">Output is similar to the following (shown here redacted):</span></span>

![Skapa ett huvudnamn för tjänsten](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

<span data-ttu-id="84d67-122">De **klient-ID** (`appId`) och **klienthemligheten** (`password`) som du använder som parametrar för tjänstens huvudnamn för klusterdistribution är markerade.</span><span class="sxs-lookup"><span data-stu-id="84d67-122">Highlighted are the **client ID** (`appId`) and the **client secret** (`password`) that you use as service principal parameters for cluster deployment.</span></span>


### <a name="specify-service-principal-when-creating-the-kubernetes-cluster"></a><span data-ttu-id="84d67-123">Ange tjänstobjektet när du skapar Kubernetes-klustret</span><span class="sxs-lookup"><span data-stu-id="84d67-123">Specify service principal when creating the Kubernetes cluster</span></span>

<span data-ttu-id="84d67-124">Ange **klient-ID:t** (kallas även `appId`, dvs. program-ID) och **klienthemligheten** (`password`) för ett befintligt tjänstobjekt som parametrar när du skapar Kubernetes-klustret.</span><span class="sxs-lookup"><span data-stu-id="84d67-124">Provide the **client ID** (also called the `appId`, for Application ID) and **client secret** (`password`) of an existing service principal as parameters when you create the Kubernetes cluster.</span></span> <span data-ttu-id="84d67-125">Kontrollera att tjänstobjektet uppfyller kraven som anges i början av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="84d67-125">Make sure the service principal meets the requirements at the beginning this article.</span></span>

<span data-ttu-id="84d67-126">Du kan ange dessa parametrar när du distribuerar Kubernetes-klustret med hjälp av [Azure Command-Line Interface (CLI) 2.0](container-service-kubernetes-walkthrough.md), [Azure Portal](../dcos-swarm/container-service-deployment.md) eller andra metoder.</span><span class="sxs-lookup"><span data-stu-id="84d67-126">You can specify these parameters when deploying the Kubernetes cluster using the [Azure Command-Line Interface (CLI) 2.0](container-service-kubernetes-walkthrough.md), [Azure portal](../dcos-swarm/container-service-deployment.md), or other methods.</span></span>

>[!TIP] 
><span data-ttu-id="84d67-127">När du anger **klient-ID**, måste du använda `appId`, och inte `ObjectId`, av huvudnamnet för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="84d67-127">When specifying the **client ID**, be sure to use the `appId`, not the `ObjectId`, of the service principal.</span></span>
>

<span data-ttu-id="84d67-128">Följande exempel beskriver ett sätt att överföra parametrarna med Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="84d67-128">The following example shows one way to pass the parameters with the Azure CLI 2.0.</span></span> <span data-ttu-id="84d67-129">I det här exemplet används [mallen Kubernetes-snabbstart](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).</span><span class="sxs-lookup"><span data-stu-id="84d67-129">This example uses the [Kubernetes quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).</span></span>

1. <span data-ttu-id="84d67-130">[Hämta](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) filen med mallparametrar `azuredeploy.parameters.json` från GitHub.</span><span class="sxs-lookup"><span data-stu-id="84d67-130">[Download](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) the template parameters file `azuredeploy.parameters.json` from GitHub.</span></span>

2. <span data-ttu-id="84d67-131">För att ange tjänstens huvudnamn, anger du värden för `servicePrincipalClientId` och `servicePrincipalClientSecret` i filen.</span><span class="sxs-lookup"><span data-stu-id="84d67-131">To specify the service principal, enter values for `servicePrincipalClientId` and `servicePrincipalClientSecret` in the file.</span></span> <span data-ttu-id="84d67-132">(Du måste också ange dina egna värden för `dnsNamePrefix` och `sshRSAPublicKey`.</span><span class="sxs-lookup"><span data-stu-id="84d67-132">(You also need to provide your own values for `dnsNamePrefix` and `sshRSAPublicKey`.</span></span> <span data-ttu-id="84d67-133">Dessa är den offentliga SSH-nyckeln till klustret.) Spara filen.</span><span class="sxs-lookup"><span data-stu-id="84d67-133">The latter is the SSH public key to access the cluster.) Save the file.</span></span>

    ![Skicka parametrar för tjänstens huvudnamn](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. <span data-ttu-id="84d67-135">Kör följande kommando, med hjälp av `--parameters` för att ange sökvägen till filen azuredeploy.parameters.json.</span><span class="sxs-lookup"><span data-stu-id="84d67-135">Run the following command, using `--parameters` to set the path to the azuredeploy.parameters.json file.</span></span> <span data-ttu-id="84d67-136">Det här kommandot distribuerar i klustret i en resursgrupp du skapar med namnet `myResourceGroup` i regionen USA, västra.</span><span class="sxs-lookup"><span data-stu-id="84d67-136">This command deploys the cluster in a resource group you create called `myResourceGroup` in the West US region.</span></span>

    ```azurecli
    az login

    az account set --subscription "mySubscriptionID"

    az group create --name "myResourceGroup" --location "westus" 
    
    az group deployment create -g "myResourceGroup" --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" --parameters @azuredeploy.parameters.json
    ```


## <a name="option-2-generate-a-service-principal-when-creating-the-cluster-with-az-acs-create"></a><span data-ttu-id="84d67-137">Alternativ 2: Generera ett tjänstobjekt när du skapar klustret med `az acs create`</span><span class="sxs-lookup"><span data-stu-id="84d67-137">Option 2: Generate a service principal when creating the cluster with `az acs create`</span></span>

<span data-ttu-id="84d67-138">Om du kör kommandot [`az acs create`](/cli/azure/acs#create) för att skapa Kubernetes-klustret kan du välja att generera ett tjänstobjekt automatiskt.</span><span class="sxs-lookup"><span data-stu-id="84d67-138">If you run the [`az acs create`](/cli/azure/acs#create) command to create the Kubernetes cluster, you have the option to generate a service principal automatically.</span></span>

<span data-ttu-id="84d67-139">Som med andra alternativ för att skapa Kubernetes-kluster, kan du ange parametrar för en befintlig tjänsts huvudnamn när du kör `az acs create`.</span><span class="sxs-lookup"><span data-stu-id="84d67-139">As with other Kubernetes cluster creation options, you can specify parameters for an existing service principal when you run `az acs create`.</span></span> <span data-ttu-id="84d67-140">Om du utelämnar dessa parametrar skapar Azure CLI ett automatiskt för användning med Container Service.</span><span class="sxs-lookup"><span data-stu-id="84d67-140">However, when you omit these parameters, the Azure CLI creates one automatically for use with Container Service.</span></span> <span data-ttu-id="84d67-141">Detta sker transparent under distributionen.</span><span class="sxs-lookup"><span data-stu-id="84d67-141">This takes place transparently during the deployment.</span></span> 

<span data-ttu-id="84d67-142">Följande kommando skapar ett Kubernetes-kluster och genererar både SSH-nycklar och autentiseringsuppgifter för tjänstens huvudnamn:</span><span class="sxs-lookup"><span data-stu-id="84d67-142">The following command creates a Kubernetes cluster and generates both SSH keys and service principal credentials:</span></span>

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

> [!IMPORTANT]
> <span data-ttu-id="84d67-143">Om ditt konto saknar de Azure AD- och prenumerationsbehörigheter som krävs för att skapa ett tjänstobjekt genererar kommandot ett fel liknande `Insufficient privileges to complete the operation.`</span><span class="sxs-lookup"><span data-stu-id="84d67-143">If your account doesn't have the Azure AD and subscription permissions to create a service principal, the command generates an error similar to `Insufficient privileges to complete the operation.`</span></span>
> 

## <a name="additional-considerations"></a><span data-ttu-id="84d67-144">Annat som är bra att tänka på</span><span class="sxs-lookup"><span data-stu-id="84d67-144">Additional considerations</span></span>

* <span data-ttu-id="84d67-145">Om du inte har behörighet att skapa ett tjänstobjekt i din prenumeration kan du behöva be din Azure AD- eller prenumerationsadministratör att tilldela nödvändiga behörigheter, eller be om ett tjänstobjekt som kan användas med Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="84d67-145">If you don't have permissions to create a service principal in your subscription, you might need to ask your Azure AD or subscription administrator to assign the necessary permissions, or ask them for a service principal to use with Azure Container Service.</span></span> 

* <span data-ttu-id="84d67-146">Tjänstobjektet för Kubernetes är en del av klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="84d67-146">The service principal for Kubernetes is a part of the cluster configuration.</span></span> <span data-ttu-id="84d67-147">Men använd inte identiteten för att distribuera klustret.</span><span class="sxs-lookup"><span data-stu-id="84d67-147">However, don't use the identity to deploy the cluster.</span></span>

* <span data-ttu-id="84d67-148">Varje tjänstobjekt är associerat med ett Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="84d67-148">Every service principal is associated with an Azure AD application.</span></span> <span data-ttu-id="84d67-149">Tjänstobjektet för ett Kubernetes-kluster kan associeras med ett giltigt Azure Active Directory-programnamn (till exempel: `https://www.contoso.org/example`).</span><span class="sxs-lookup"><span data-stu-id="84d67-149">The service principal for a Kubernetes cluster can be associated with any valid Azure AD application name (for example: `https://www.contoso.org/example`).</span></span> <span data-ttu-id="84d67-150">URL:en för programmet behöver inte vara en verklig slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="84d67-150">The URL for the application doesn't have to be a real endpoint.</span></span>

* <span data-ttu-id="84d67-151">När du anger **klient-ID:t** för tjänstobjektet kan du använda värdet för `appId` (som anges i den här artikeln) eller motsvarande `name` för tjänstobjektet (till exempel `https://www.contoso.org/example`).</span><span class="sxs-lookup"><span data-stu-id="84d67-151">When specifying the service principal **Client ID**, you can use the value of the `appId` (as shown in this article) or the corresponding service principal `name` (for example,`https://www.contoso.org/example`).</span></span>

* <span data-ttu-id="84d67-152">På virtuella huvud- och agentdatorer i Kubernetes-klustret lagras autentiseringsuppgifterna för tjänstobjektet i filen /etc/kubernetes/azure.json.</span><span class="sxs-lookup"><span data-stu-id="84d67-152">On the master and agent VMs in the Kubernetes cluster, the service principal credentials are stored in the file /etc/kubernetes/azure.json.</span></span>

* <span data-ttu-id="84d67-153">Om du använder kommandot `az acs create` för att generera tjänstobjektet automatiskt, skrivs autentiseringsuppgifterna för tjänstobjektet till filen ~/.azure/acsServicePrincipal.json på den dator som används för att köra kommandot.</span><span class="sxs-lookup"><span data-stu-id="84d67-153">When you use the `az acs create` command to generate the service principal automatically, the service principal credentials are written to the file ~/.azure/acsServicePrincipal.json on the machine used to run the command.</span></span> 

* <span data-ttu-id="84d67-154">Om du använder kommandot `az acs create` för att generera tjänstobjektet automatiskt, kan tjänstobjektet även autentisera med ett [Azure-behållarregister](../../container-registry/container-registry-intro.md) som skapats i samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="84d67-154">When you use the `az acs create` command to generate the service principal automatically, the service principal can also authenticate with an [Azure container registry](../../container-registry/container-registry-intro.md) created in the same subscription.</span></span>




## <a name="next-steps"></a><span data-ttu-id="84d67-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="84d67-155">Next steps</span></span>

* <span data-ttu-id="84d67-156">[Kom igång med Kubernetes](container-service-kubernetes-walkthrough.md) i behållaren för tjänsteklustret.</span><span class="sxs-lookup"><span data-stu-id="84d67-156">[Get started with Kubernetes](container-service-kubernetes-walkthrough.md) in your container service cluster.</span></span>

* <span data-ttu-id="84d67-157">Information om hur du felsöker tjänstobjektet för Kubernetes finns i [ACS Engine-dokumentationen](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes.md#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="84d67-157">To troubleshoot the service principal for Kubernetes, see the [ACS Engine documentation](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes.md#troubleshooting).</span></span>


