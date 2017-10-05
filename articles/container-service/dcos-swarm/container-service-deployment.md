---
title: "Distribuera ett Docker-behållarkluster i Azure | Microsoft Docs"
description: "Distribuera en Kubernetes-, DC/OS- eller Docker Swarm lösning i Azure Container Service med hjälp av Azure Portal eller en Resource Manager-mall."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Mesos, Azure, dcos, swarm, kubernetes, azure container service, acs
ms.service: container-service
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: rogardle
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 0ef256537bf095e2a5d582bd345a9c8dcede2095
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-docker-container-hosting-solution-using-the-azure-portal"></a><span data-ttu-id="ca867-104">Distribuera en värdlösning med en Docker-behållare via Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ca867-104">Deploy a Docker container hosting solution using the Azure portal</span></span>



<span data-ttu-id="ca867-105">Azure Container Service ger snabb distribution av populär behållarklustring med öppen källkod och orchestration-lösningar.</span><span class="sxs-lookup"><span data-stu-id="ca867-105">Azure Container Service provides rapid deployment of popular open-source container clustering and orchestration solutions.</span></span> <span data-ttu-id="ca867-106">Det här dokumentet innehåller anvisningar för att distribuera ett Azure Container Service-kluster via Azure Portal eller en snabbstartsmall för Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ca867-106">This document walks you through deploying an Azure Container Service cluster by using the Azure portal or an Azure Resource Manager quickstart template.</span></span> 

<span data-ttu-id="ca867-107">Du kan även distribuera ett Azure Container Service-kluster med hjälp av [Azure CLI 2.0](container-service-create-acs-cluster-cli.md) eller Azure Container Service-API:erna.</span><span class="sxs-lookup"><span data-stu-id="ca867-107">You can also deploy an Azure Container Service cluster by using the [Azure CLI 2.0](container-service-create-acs-cluster-cli.md) or the Azure Container Service APIs.</span></span>

<span data-ttu-id="ca867-108">Bakgrundsinformation finns i [Introduktion till Azure Container Service](../container-service-intro.md).</span><span class="sxs-lookup"><span data-stu-id="ca867-108">For background, see [Azure Container Service introduction](../container-service-intro.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="ca867-109">Krav</span><span class="sxs-lookup"><span data-stu-id="ca867-109">Prerequisites</span></span>

* <span data-ttu-id="ca867-110">**Azure-prenumeration**: Om du inte har någon kan du registrera dig för en [kostnadsfri utvärderingsversion](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span><span class="sxs-lookup"><span data-stu-id="ca867-110">**Azure subscription**: If you don't have one, sign up for a [free trial](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span></span> <span data-ttu-id="ca867-111">Överväg en prenumeration utan fast avgift eller andra alternativ för ett större kluster.</span><span class="sxs-lookup"><span data-stu-id="ca867-111">For a larger cluster, consider a pay-as-you go subscription or other purchase options.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ca867-112">Din användning av din Azure-prenumeration och dina [resurskvoter](../../azure-subscription-service-limits.md), till exempel kärnkvoter, kan begränsa storleken på det kluster som du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="ca867-112">Your Azure subscription usage and [resource quotas](../../azure-subscription-service-limits.md), such as cores quotas, can limit the size of the cluster you deploy.</span></span> <span data-ttu-id="ca867-113">Om du vill begära en ökning av kvoten kan du öppna ett [kundsupportärende online](../../azure-supportability/how-to-create-azure-support-request.md) utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="ca867-113">To request a quota increase, open an [online customer support request](../../azure-supportability/how-to-create-azure-support-request.md) at no charge.</span></span>
    >

* <span data-ttu-id="ca867-114">**Offentlig SSH RSA-nyckel**: När du distribuerar via portalen eller en av Azures snabbstartsmallar måste du ange den offentliga nyckeln för autentisering mot virtuella datorer i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="ca867-114">**SSH RSA public key**: When deploying through the portal or one of the Azure quickstart templates, you need to provide the public key for authentication against Azure Container Service virtual machines.</span></span> <span data-ttu-id="ca867-115">Information om hur du skapar SSH (Secure Shell) RSA-nycklar finns i hjälpartiklarna för [OS X och Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) eller [Windows](../../virtual-machines/linux/ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="ca867-115">To create Secure Shell (SSH) RSA keys, see the [OS X and Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) or [Windows](../../virtual-machines/linux/ssh-from-windows.md) guidance.</span></span> 

* <span data-ttu-id="ca867-116">**Klient-ID och hemlighet för tjänstens huvudnamn** (endast Kubernetes): Mer information och vägledning om hur du skapar ett Azure Active Directory-huvudnamn finns i [Om tjänstens huvudnamn för ett Kubernetes-kluster](../kubernetes/container-service-kubernetes-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="ca867-116">**Service principal client ID and secret** (Kubernetes only): For more information and guidance to create an Azure Active Directory service principal, see [About the service principal for a Kubernetes cluster](../kubernetes/container-service-kubernetes-service-principal.md).</span></span>



## <a name="create-a-cluster-by-using-the-azure-portal"></a><span data-ttu-id="ca867-117">Skapa ett kluster med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ca867-117">Create a cluster by using the Azure portal</span></span>
1. <span data-ttu-id="ca867-118">Logga in på Azure Portal, välj **Ny** och sök på Azure Marketplace efter **Azure Container Service**.</span><span class="sxs-lookup"><span data-stu-id="ca867-118">Sign in to the Azure portal, select **New**, and search the Azure Marketplace for **Azure Container Service**.</span></span>

    ![Azure Container Service i Marketplace](./media/container-service-deployment/acs-portal1.png)  <br />

2. <span data-ttu-id="ca867-120">Klicka på **Azure Container Service** och klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ca867-120">Click **Azure Container Service**, and click **Create**.</span></span>

3. <span data-ttu-id="ca867-121">Ange följande information i bladet **Grundläggande inställningar**:</span><span class="sxs-lookup"><span data-stu-id="ca867-121">On the **Basics** blade, enter the following information:</span></span>

    * <span data-ttu-id="ca867-122">**Orchestrator**: Välj något av behållardirigeringsverktygen för att distribuera klustret.</span><span class="sxs-lookup"><span data-stu-id="ca867-122">**Orchestrator**: Select one of the container orchestrators to deploy on the cluster.</span></span>
        * <span data-ttu-id="ca867-123">**DC/OS**: distribuerar ett DC/OS-kluster.</span><span class="sxs-lookup"><span data-stu-id="ca867-123">**DC/OS**: Deploys a DC/OS cluster.</span></span>
        * <span data-ttu-id="ca867-124">**Swarm**: distribuerar ett Docker Swarm-kluster.</span><span class="sxs-lookup"><span data-stu-id="ca867-124">**Swarm**: Deploys a Docker Swarm cluster.</span></span>
        * <span data-ttu-id="ca867-125">**Kubernetes**: Distribuerar ett Kubernetes-kluster.</span><span class="sxs-lookup"><span data-stu-id="ca867-125">**Kubernetes**: Deploys a Kubernetes cluster.</span></span>
    * <span data-ttu-id="ca867-126">**Prenumeration**: Välj en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ca867-126">**Subscription**: Select an Azure subscription.</span></span>
    * <span data-ttu-id="ca867-127">**Resursgrupp**: Ange namnet på en ny resursgrupp för distributionen.</span><span class="sxs-lookup"><span data-stu-id="ca867-127">**Resource group**: Enter the name of a new resource group for the deployment.</span></span>
    * <span data-ttu-id="ca867-128">**Plats**: Välj en Azure-region för Azure Container Service-distributionen.</span><span class="sxs-lookup"><span data-stu-id="ca867-128">**Location**: Select an Azure region for the Azure Container Service deployment.</span></span> <span data-ttu-id="ca867-129">Om du vill kontrollera tillgänglighet läser du [Produkttillgänglighet per region](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="ca867-129">For availability, check [Products available by region](https://azure.microsoft.com/regions/services/).</span></span>
    
    ![Grundläggande inställningar](./media/container-service-deployment/acs-portal3.png)  <br />
    
    <span data-ttu-id="ca867-131">Klicka på **OK** när du är redo att gå vidare.</span><span class="sxs-lookup"><span data-stu-id="ca867-131">Click **OK** when you're ready to proceed.</span></span>

4. <span data-ttu-id="ca867-132">På bladet **Konfiguration av huvudservrar** anger du följande inställningar för den överordnade Linux-noden eller -noderna i klustret (vissa inställningar är specifika för varje orchestrator):</span><span class="sxs-lookup"><span data-stu-id="ca867-132">On the **Master configuration** blade, enter the following settings for the Linux master node or nodes in the cluster (some settings are specific to each orchestrator):</span></span>

    * <span data-ttu-id="ca867-133">**Huvud-DNS-namn**: Prefixet som används för att skapa ett unikt fullständigt kvalificerat domännamn (FQDN) för huvudservern.</span><span class="sxs-lookup"><span data-stu-id="ca867-133">**Master DNS name**: The prefix used to create a unique fully qualified domain name (FQDN) for the master.</span></span> <span data-ttu-id="ca867-134">Det fullständiga domännamnet är i formatet *prefix*mgmt.*location*.cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="ca867-134">The master FQDN is of the form *prefix*mgmt.*location*.cloudapp.azure.com.</span></span>
    * <span data-ttu-id="ca867-135">**Användarnamn**: Användarkontot för ett konto på varje virtuell Linux-dator i klustret.</span><span class="sxs-lookup"><span data-stu-id="ca867-135">**User name**: The user name for an account on each of the Linux virtual machines in the cluster.</span></span>
    * <span data-ttu-id="ca867-136">**Offentlig SSH RSA-nyckel**: Lägg till den offentliga nyckel som ska användas för autentisering mot virtuella datorer i Linux.</span><span class="sxs-lookup"><span data-stu-id="ca867-136">**SSH RSA public key**: Add the public key to be used for authentication against the Linux virtual machines.</span></span> <span data-ttu-id="ca867-137">Det är viktigt att den här nyckeln inte innehåller några radbrytningar och att den innehåller prefixet `ssh-rsa`.</span><span class="sxs-lookup"><span data-stu-id="ca867-137">It is important that this key contains no line breaks, and it includes the `ssh-rsa` prefix.</span></span> <span data-ttu-id="ca867-138">Postfixen `username@domain` är valfri.</span><span class="sxs-lookup"><span data-stu-id="ca867-138">The `username@domain` postfix is optional.</span></span> <span data-ttu-id="ca867-139">Nyckeln bör vara lik följande: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**.</span><span class="sxs-lookup"><span data-stu-id="ca867-139">The key should look something like the following: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**.</span></span> 
    * <span data-ttu-id="ca867-140">**Tjänstens huvudnamn**: Om du valde Kubernetes-orchestrator anger du ett Azure Active Directory-**klient-ID för tjänstens huvudnamn** (kallas även appId) och en **klienthemlighet för tjänstens huvudnamn** (lösenord).</span><span class="sxs-lookup"><span data-stu-id="ca867-140">**Service principal**: If you selected the Kubernetes orchestrator, enter an Azure Active Directory **Service principal client ID** (also called the appId) and **Service principal client secret** (password).</span></span> <span data-ttu-id="ca867-141">Mer information finns i [Om tjänstens huvudnamn för ett Kubernetes-kluster](../kubernetes/container-service-kubernetes-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="ca867-141">For more information, see [About the service principal for a Kubernetes cluster](../kubernetes/container-service-kubernetes-service-principal.md).</span></span>
    * <span data-ttu-id="ca867-142">**Antal huvudservrar**: antal huvudservrar i klustret.</span><span class="sxs-lookup"><span data-stu-id="ca867-142">**Master count**: The number of masters in the cluster.</span></span>
    * <span data-ttu-id="ca867-143">**VM-diagnostik**: För vissa orchestrators kan du aktivera VM-diagnostik för huvudservrarna.</span><span class="sxs-lookup"><span data-stu-id="ca867-143">**VM diagnostics**: For some orchestrators, you can enable VM diagnostics on the masters.</span></span>

    ![Konfiguration av huvudservrar](./media/container-service-deployment/acs-portal4.png)  <br />

    <span data-ttu-id="ca867-145">Klicka på **OK** när du är redo att gå vidare.</span><span class="sxs-lookup"><span data-stu-id="ca867-145">Click **OK** when you're ready to proceed.</span></span>

5. <span data-ttu-id="ca867-146">Ange följande information i bladet **Agentkonfiguration**:</span><span class="sxs-lookup"><span data-stu-id="ca867-146">On the **Agent configuration** blade, enter the following information:</span></span>

    * <span data-ttu-id="ca867-147">**Antal agenter**: För Docker Swarm och Kubernetes är det här värdet det inledande antalet agenter i agentskalningsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="ca867-147">**Agent count**: For Docker Swarm and Kubernetes, this value is the initial number of agents in the agent scale set.</span></span> <span data-ttu-id="ca867-148">När det gäller DC/OS utgör det här det inledande antalet agenter i en privat skalningsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="ca867-148">For DC/OS, it is the initial number of agents in a private scale set.</span></span> <span data-ttu-id="ca867-149">Dessutom skapas en offentlig skalningsuppsättning för DC/OS som innehåller ett förinställt antal agenter.</span><span class="sxs-lookup"><span data-stu-id="ca867-149">Additionally, a public scale set is created for DC/OS, which contains a predetermined number of agents.</span></span> <span data-ttu-id="ca867-150">Antalet agenter i den här offentliga skalningsuppsättningen bestäms av antalet huvudservrar i klustret. En offentlig agent för en huvudserver och två offentliga agenter för tre eller fem huvudservrar.</span><span class="sxs-lookup"><span data-stu-id="ca867-150">The number of agents in this public scale set is determined by the number of masters in the cluster: one public agent for one master, and two public agents for three or five masters.</span></span>
    * <span data-ttu-id="ca867-151">**Storlek på agentens virtuella dator**: storleken på agentens virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ca867-151">**Agent virtual machine size**: The size of the agent virtual machines.</span></span>
    * <span data-ttu-id="ca867-152">**Operativsystem**: Den här inställningen är för närvarande endast tillgänglig om du har valt Kubernetes-orchestratorn.</span><span class="sxs-lookup"><span data-stu-id="ca867-152">**Operating system**: This setting is currently available only if you selected the Kubernetes orchestrator.</span></span> <span data-ttu-id="ca867-153">Välj antingen en Linux-distribution eller ett Windows Server-operativsystem som ska köras på agenterna.</span><span class="sxs-lookup"><span data-stu-id="ca867-153">Choose either a Linux distribution or a Windows Server operating system to run on the agents.</span></span> <span data-ttu-id="ca867-154">Den här inställningen bestämmer om ditt kluster kan köra Linux- eller Windows-behållarappar.</span><span class="sxs-lookup"><span data-stu-id="ca867-154">This setting determines whether your cluster can run Linux or Windows container apps.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="ca867-155">Stöd för Windows-behållare finns i förhandsgranskningen för Kubernetes-kluster.</span><span class="sxs-lookup"><span data-stu-id="ca867-155">Windows container support is in preview for Kubernetes clusters.</span></span> <span data-ttu-id="ca867-156">På DC-/OS- och Swarm-kluster stöds för närvarande endast Linux-agenter i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="ca867-156">On DC/OS and Swarm clusters, only Linux agents are currently supported in Azure Container Service.</span></span>

    * <span data-ttu-id="ca867-157">**Agentautentiseringsuppgifter**: Om du valde Windows-operativsystemet anger du ett **administratörsanvändarnamn** och **lösenord** för VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="ca867-157">**Agent credentials**: If you selected the Windows operating system, enter an administrator **User name** and **Password** for the agent VMs.</span></span> 

    ![Agentkonfiguration](./media/container-service-deployment/acs-portal5.png)  <br />

    <span data-ttu-id="ca867-159">Klicka på **OK** när du är redo att gå vidare.</span><span class="sxs-lookup"><span data-stu-id="ca867-159">Click **OK** when you're ready to proceed.</span></span>

6. <span data-ttu-id="ca867-160">Klicka på **OK** när tjänsteverifieringen är klar.</span><span class="sxs-lookup"><span data-stu-id="ca867-160">After service validation finishes, click **OK**.</span></span>

    ![Validering](./media/container-service-deployment/acs-portal6.png)  <br />

7. <span data-ttu-id="ca867-162">Granska villkoren.</span><span class="sxs-lookup"><span data-stu-id="ca867-162">Review the terms.</span></span> <span data-ttu-id="ca867-163">Klicka på **Skapa** för att starta distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="ca867-163">To start the deployment process, click **Create**.</span></span>

    <span data-ttu-id="ca867-164">Om du har valt att fästa distributionen på Azure Portal, visas distributionsstatusen.</span><span class="sxs-lookup"><span data-stu-id="ca867-164">If you've elected to pin the deployment to the Azure portal, you can see the deployment status.</span></span>

    ![Status för distribution](./media/container-service-deployment/acs-portal8.png)  <br />

<span data-ttu-id="ca867-166">Distributionen tar normalt flera minuter för att slutföras.</span><span class="sxs-lookup"><span data-stu-id="ca867-166">The deployment takes several minutes to complete.</span></span> <span data-ttu-id="ca867-167">Sedan kan Azure Container Service-klustret användas.</span><span class="sxs-lookup"><span data-stu-id="ca867-167">Then, the Azure Container Service cluster is ready for use.</span></span>


## <a name="create-a-cluster-by-using-a-quickstart-template"></a><span data-ttu-id="ca867-168">Skapa ett kluster med en snabbstartsmall</span><span class="sxs-lookup"><span data-stu-id="ca867-168">Create a cluster by using a quickstart template</span></span>
<span data-ttu-id="ca867-169">Azure-snabbstartsmallar är tillgängliga för att distribuera ett kluster i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="ca867-169">Azure quickstart templates are available to deploy a cluster in Azure Container Service.</span></span> <span data-ttu-id="ca867-170">De tillhandahållna snabbstartsmallarna kan ändras så att de inkluderar ytterligare eller avancerad Azure-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ca867-170">The provided quickstart templates can be modified to include additional or advanced Azure configuration.</span></span> <span data-ttu-id="ca867-171">Du måste ha en Azure-prenumeration för att kunna skapa ett Azure Container Service-kluster med en Azure-snabbstartsmall.</span><span class="sxs-lookup"><span data-stu-id="ca867-171">To create an Azure Container Service cluster by using an Azure quickstart template, you need an Azure subscription.</span></span> <span data-ttu-id="ca867-172">Om du inte har någon kan du registrera dig för en [kostnadsfri utvärderingsversion](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span><span class="sxs-lookup"><span data-stu-id="ca867-172">If you don't have one, then sign up for a [free trial](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span></span> 

<span data-ttu-id="ca867-173">Följ stegen nedan för att distribuera ett kluster med hjälp av en mall och Azure CLI 2.0 (se [installations- och konfigurationsanvisningarna](/cli/azure/install-az-cli2)).</span><span class="sxs-lookup"><span data-stu-id="ca867-173">Follow these steps to deploy a cluster using a template and the Azure CLI 2.0 (see [installation and setup instructions](/cli/azure/install-az-cli2)).</span></span>

> [!NOTE] 
> <span data-ttu-id="ca867-174">Du kan använda liknande steg för att distribuera en mall med Azure PowerShell om du är på ett Windows-system.</span><span class="sxs-lookup"><span data-stu-id="ca867-174">If you're on a Windows system, you can use similar steps to deploy a template using Azure PowerShell.</span></span> <span data-ttu-id="ca867-175">Se anvisningarna senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ca867-175">See steps later in this section.</span></span> <span data-ttu-id="ca867-176">Du kan även distribuera en mall genom [Portalen](../../azure-resource-manager/resource-group-template-deploy-portal.md) eller genom andra metoder.</span><span class="sxs-lookup"><span data-stu-id="ca867-176">You can also deploy a template through the [portal](../../azure-resource-manager/resource-group-template-deploy-portal.md) or other methods.</span></span>

1. <span data-ttu-id="ca867-177">Om du vill distribuera ett DC/OS-, Docker Swarm- eller Kubernetes-kluster väljer du någon av de tillgängliga snabbstartsmallarna från GitHub.</span><span class="sxs-lookup"><span data-stu-id="ca867-177">To deploy a DC/OS, Docker Swarm, or Kubernetes cluster, select one of the available quickstart templates from GitHub.</span></span> <span data-ttu-id="ca867-178">En ofullständig lista finns nedan.</span><span class="sxs-lookup"><span data-stu-id="ca867-178">A partial list follows.</span></span> <span data-ttu-id="ca867-179">DC/OS- och Swarm-mallarna är likadana, med undantag för standardvalet av orchestrator.</span><span class="sxs-lookup"><span data-stu-id="ca867-179">The DC/OS and Swarm templates are the same, except for the default orchestrator selection.</span></span>

    * [<span data-ttu-id="ca867-180">DC/OS-mall</span><span class="sxs-lookup"><span data-stu-id="ca867-180">DC/OS template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [<span data-ttu-id="ca867-181">Swarm-mall</span><span class="sxs-lookup"><span data-stu-id="ca867-181">Swarm template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [<span data-ttu-id="ca867-182">Kubernetes-mall</span><span class="sxs-lookup"><span data-stu-id="ca867-182">Kubernetes template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. <span data-ttu-id="ca867-183">Logga in på ditt Azure-konto (`az login`) och kontrollera att Azure CLI är anslutet till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ca867-183">Log in to your Azure account (`az login`), and make sure that the Azure CLI is connected to your Azure subscription.</span></span> <span data-ttu-id="ca867-184">Du kan se standard-prenumerationen med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ca867-184">You can see the default subscription by using the following command:</span></span>

    ```azurecli
    az account show
    ```
    
    <span data-ttu-id="ca867-185">Om du har fler än en prenumeration och måste ange en annan standard-prenumeration kör du `az account set --subscription` och anger prenumerationens ID eller namn.</span><span class="sxs-lookup"><span data-stu-id="ca867-185">If you have more than one subscription and need to set a different default subscription, run `az account set --subscription` and specify the subscription ID or name.</span></span>

3. <span data-ttu-id="ca867-186">Du bör använda en ny resursgrupp för distributionen.</span><span class="sxs-lookup"><span data-stu-id="ca867-186">As a best practice, use a new resource group for the deployment.</span></span> <span data-ttu-id="ca867-187">Skapa en resursgrupp genom att använda kommandot `az group create` och ange ett resursgruppnamn och en plats:</span><span class="sxs-lookup"><span data-stu-id="ca867-187">To create a resource group, use the `az group create` command specify a resource group name and location:</span></span> 

    ```azurecli
    az group create --name "RESOURCE_GROUP" --location "LOCATION"
    ```

4. <span data-ttu-id="ca867-188">Skapa en JSON-fil som innehåller nödvändiga mallparametrarna.</span><span class="sxs-lookup"><span data-stu-id="ca867-188">Create a JSON file containing the required template parameters.</span></span> <span data-ttu-id="ca867-189">Hämta parameterfilen som heter `azuredeploy.parameters.json` som kommer med Azure Container Service-mallarna `azuredeploy.json` i GitHub.</span><span class="sxs-lookup"><span data-stu-id="ca867-189">Download the parameters file named `azuredeploy.parameters.json` that accompanies the Azure Container Service template `azuredeploy.json` in GitHub.</span></span> <span data-ttu-id="ca867-190">Ange de parametervärden som krävs för klustret.</span><span class="sxs-lookup"><span data-stu-id="ca867-190">Enter required parameter values for your cluster.</span></span> 

    <span data-ttu-id="ca867-191">För att till exempel använda [DC/OS-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos) anger du parametervärden för `dnsNamePrefix` och `sshRSAPublicKey`.</span><span class="sxs-lookup"><span data-stu-id="ca867-191">For example, to use the [DC/OS template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos), supply parameter values for `dnsNamePrefix` and `sshRSAPublicKey`.</span></span> <span data-ttu-id="ca867-192">Se beskrivningarna i `azuredeploy.json` och alternativ för andra parametrar.</span><span class="sxs-lookup"><span data-stu-id="ca867-192">See the descriptions in `azuredeploy.json` and options for other parameters.</span></span>  
 

5. <span data-ttu-id="ca867-193">Skapa ett Container Service-kluster genom att skicka filen med parametrarna för distribution med följande kommando, där:</span><span class="sxs-lookup"><span data-stu-id="ca867-193">Create a Container Service cluster by passing the deployment parameters file with the following command, where:</span></span>

    * <span data-ttu-id="ca867-194">**RESOURCE_GROUP** är namnet på resursgruppen som du skapade i det föregående steget.</span><span class="sxs-lookup"><span data-stu-id="ca867-194">**RESOURCE_GROUP** is the name of the resource group that you created in the previous step.</span></span>
    * <span data-ttu-id="ca867-195">**DEPLOYMENT_NAME** (valfritt) är namnet du ger distributionen.</span><span class="sxs-lookup"><span data-stu-id="ca867-195">**DEPLOYMENT_NAME** (optional) is a name you give to the deployment.</span></span>
    * <span data-ttu-id="ca867-196">**TEMPLATE_URI** är platsen för distributionsfilen `azuredeploy.json`.</span><span class="sxs-lookup"><span data-stu-id="ca867-196">**TEMPLATE_URI** is the location of the deployment file `azuredeploy.json`.</span></span> <span data-ttu-id="ca867-197">Den här URI:n måste vara rådatafilen, inte en pekare till GitHub-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="ca867-197">This URI must be the Raw file, not a pointer to the GitHub UI.</span></span> <span data-ttu-id="ca867-198">Du kan hitta den här URI:en genom att markera filen `azuredeploy.json` i GitHub och klicka på **Raw**-knappen.</span><span class="sxs-lookup"><span data-stu-id="ca867-198">To find this URI, select the `azuredeploy.json` file in GitHub, and click the **Raw** button.</span></span>  

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters @azuredeploy.parameters.json
    ```

    <span data-ttu-id="ca867-199">Du kan även ange parametrar som en JSON-formaterad sträng i kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="ca867-199">You can also provide parameters as a JSON-formatted string on the command line.</span></span> <span data-ttu-id="ca867-200">Använd ett kommando som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="ca867-200">Use a command similar to the following:</span></span>

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters "{ \"param1\": {\"value1\"} … }"
    ```

    > [!NOTE]
    > <span data-ttu-id="ca867-201">Distributionen tar normalt flera minuter för att slutföras.</span><span class="sxs-lookup"><span data-stu-id="ca867-201">The deployment takes several minutes to complete.</span></span>
    > 

### <a name="equivalent-powershell-commands"></a><span data-ttu-id="ca867-202">Motsvarande PowerShell-kommandon</span><span class="sxs-lookup"><span data-stu-id="ca867-202">Equivalent PowerShell commands</span></span>
<span data-ttu-id="ca867-203">Du kan även distribuera en mall för ett Azure Container Service-kluster med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ca867-203">You can also deploy an Azure Container Service cluster template with PowerShell.</span></span> <span data-ttu-id="ca867-204">Det här dokumentet utgår från version 1.0 av [Azure PowerShell-modulen](https://azure.microsoft.com/blog/azps-1-0/).</span><span class="sxs-lookup"><span data-stu-id="ca867-204">This document is based on the version 1.0 [Azure PowerShell module](https://azure.microsoft.com/blog/azps-1-0/).</span></span>

1. <span data-ttu-id="ca867-205">Om du vill distribuera ett DC/OS-, Docker Swarm- eller Kubernetes-kluster väljer du någon av de tillgängliga snabbstartsmallarna från GitHub.</span><span class="sxs-lookup"><span data-stu-id="ca867-205">To deploy a DC/OS, Docker Swarm, or Kubernetes cluster, select one of the available quickstart templates from GitHub.</span></span> <span data-ttu-id="ca867-206">En ofullständig lista finns nedan.</span><span class="sxs-lookup"><span data-stu-id="ca867-206">A partial list follows.</span></span> <span data-ttu-id="ca867-207">Observera att DC/OS- och Swarm-mallarna är likadana, med undantag för standardvalet av orchestrator.</span><span class="sxs-lookup"><span data-stu-id="ca867-207">Note that the DC/OS and Swarm templates are the same, with the exception of the default orchestrator selection.</span></span>

    * [<span data-ttu-id="ca867-208">DC/OS-mall</span><span class="sxs-lookup"><span data-stu-id="ca867-208">DC/OS template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [<span data-ttu-id="ca867-209">Swarm-mall</span><span class="sxs-lookup"><span data-stu-id="ca867-209">Swarm template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [<span data-ttu-id="ca867-210">Kubernetes-mall</span><span class="sxs-lookup"><span data-stu-id="ca867-210">Kubernetes template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. <span data-ttu-id="ca867-211">Innan du skapar ett kluster i din Azure-prenumeration måste du kontrollera att PowerShell-sessionen har loggats in på Azure.</span><span class="sxs-lookup"><span data-stu-id="ca867-211">Before creating a cluster in your Azure subscription, verify that your PowerShell session has been signed in to Azure.</span></span> <span data-ttu-id="ca867-212">Det kan du göra med kommandot `Get-AzureRMSubscription`:</span><span class="sxs-lookup"><span data-stu-id="ca867-212">You can do this with the `Get-AzureRMSubscription` command:</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

3. <span data-ttu-id="ca867-213">Om du behöver logga in på Azure kan du använda kommandot `Login-AzureRMAccount`:</span><span class="sxs-lookup"><span data-stu-id="ca867-213">If you need to sign in to Azure, use the `Login-AzureRMAccount` command:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

4. <span data-ttu-id="ca867-214">Du bör använda en ny resursgrupp för distributionen.</span><span class="sxs-lookup"><span data-stu-id="ca867-214">As a best practice, use a new resource group for the deployment.</span></span> <span data-ttu-id="ca867-215">Skapa en resursgrupp genom att använda kommandot `New-AzureRmResourceGroup` och ange ett resursgruppnamn och en målregion:</span><span class="sxs-lookup"><span data-stu-id="ca867-215">To create a resource group, use the `New-AzureRmResourceGroup` command, and specify a resource group name and destination region:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
    ```

5. <span data-ttu-id="ca867-216">När du har skapat en resursgrupp kan du skapa klustret med nedanstående kommando.</span><span class="sxs-lookup"><span data-stu-id="ca867-216">After you create a resource group, you can create your cluster with the following command.</span></span> <span data-ttu-id="ca867-217">URI:n för den önskade mallen anges för parametern `-TemplateUri`.</span><span class="sxs-lookup"><span data-stu-id="ca867-217">The URI of the desired template is specified with the `-TemplateUri` parameter.</span></span> <span data-ttu-id="ca867-218">När du kör det här kommandot uppmanar PowerShell dig att ange parametervärden för distributionen.</span><span class="sxs-lookup"><span data-stu-id="ca867-218">When you run this command, PowerShell prompts you for deployment parameter values.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
    ```

#### <a name="provide-template-parameters"></a><span data-ttu-id="ca867-219">Ange mallparametrar</span><span class="sxs-lookup"><span data-stu-id="ca867-219">Provide template parameters</span></span>
<span data-ttu-id="ca867-220">Om du är bekant med PowerShell vet du att du kan gå igenom tillgängliga parametrar för en cmdlet genom att skriva ett minustecken (-) och sedan trycka på TABB-tangenten.</span><span class="sxs-lookup"><span data-stu-id="ca867-220">If you're familiar with PowerShell, you know that you can cycle through the available parameters for a cmdlet by typing a minus sign (-) and then pressing the TAB key.</span></span> <span data-ttu-id="ca867-221">Den här metoden går även att använda med parametrar som du anger i mallen.</span><span class="sxs-lookup"><span data-stu-id="ca867-221">This same functionality also works with parameters that you define in your template.</span></span> <span data-ttu-id="ca867-222">När du anger mallnamnet hämtar cmdleten mallen, parsar parametrarna och lägger dynamiskt till mallparametrarna i kommandot.</span><span class="sxs-lookup"><span data-stu-id="ca867-222">As soon as you type the template name, the cmdlet fetches the template, parses the parameters, and adds the template parameters to the command dynamically.</span></span> <span data-ttu-id="ca867-223">På så sätt är det enkelt att ange parametervärden för mallen.</span><span class="sxs-lookup"><span data-stu-id="ca867-223">This makes it easy to specify the template parameter values.</span></span> <span data-ttu-id="ca867-224">Och om du glömmer ett nödvändigt parametervärde efterfrågar PowerShell värdet.</span><span class="sxs-lookup"><span data-stu-id="ca867-224">And, if you forget a required parameter value, PowerShell prompts you for the value.</span></span>

<span data-ttu-id="ca867-225">Här visas det fullständiga kommandot inklusive parametrar.</span><span class="sxs-lookup"><span data-stu-id="ca867-225">Here is the full command, with parameters included.</span></span> <span data-ttu-id="ca867-226">Ange egna värden för resursnamnen.</span><span class="sxs-lookup"><span data-stu-id="ca867-226">Provide your own values for the names of the resources.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a><span data-ttu-id="ca867-227">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ca867-227">Next steps</span></span>
<span data-ttu-id="ca867-228">Nu när du har ett fungerande kluster kan du visa dessa dokument för anslutnings- och hanteringsinformation:</span><span class="sxs-lookup"><span data-stu-id="ca867-228">Now that you have a functioning cluster, see these documents for connection and management details:</span></span>

* [<span data-ttu-id="ca867-229">Ansluta till ett Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="ca867-229">Connect to an Azure Container Service cluster</span></span>](../container-service-connect.md)
* [<span data-ttu-id="ca867-230">Arbeta med Azure Container Service och DC/OS</span><span class="sxs-lookup"><span data-stu-id="ca867-230">Work with Azure Container Service and DC/OS</span></span>](container-service-mesos-marathon-rest.md)
* [<span data-ttu-id="ca867-231">Arbeta med Azure Container Service och Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="ca867-231">Work with Azure Container Service and Docker Swarm</span></span>](container-service-docker-swarm.md)
* [<span data-ttu-id="ca867-232">Arbeta med Azure Container Service och Kubernetes</span><span class="sxs-lookup"><span data-stu-id="ca867-232">Work with Azure Container Service and Kubernetes</span></span>](../kubernetes/container-service-kubernetes-walkthrough.md)
