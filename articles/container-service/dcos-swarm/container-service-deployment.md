---
title: "aaaDeploy en dockerbehållare kluster i Azure | Microsoft Docs"
description: "Distribuera en lösning för Kubernetes, DC/OS eller Docker Swarm i Azure Container Service med hello Azure-portalen eller en Resource Manager-mall."
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
ms.openlocfilehash: 26e3a7d0af9d71acd8b5c85fd667fcf7d84cef66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-docker-container-hosting-solution-using-hello-azure-portal"></a><span data-ttu-id="c5731-104">Distribuera en dockerbehållare som värd-lösning med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c5731-104">Deploy a Docker container hosting solution using hello Azure portal</span></span>



<span data-ttu-id="c5731-105">Azure Container Service ger snabb distribution av populär behållarklustring med öppen källkod och orchestration-lösningar.</span><span class="sxs-lookup"><span data-stu-id="c5731-105">Azure Container Service provides rapid deployment of popular open-source container clustering and orchestration solutions.</span></span> <span data-ttu-id="c5731-106">Det här dokumentet vägleder dig genom att distribuera ett Azure Container Service-kluster med hjälp av hello Azure-portalen eller en mall för Azure Resource Manager-Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="c5731-106">This document walks you through deploying an Azure Container Service cluster by using hello Azure portal or an Azure Resource Manager quickstart template.</span></span> 

<span data-ttu-id="c5731-107">Du kan också distribuera ett Azure Container Service-kluster med hjälp av hello [Azure CLI 2.0](container-service-create-acs-cluster-cli.md) eller hello Azure Container Service API: er.</span><span class="sxs-lookup"><span data-stu-id="c5731-107">You can also deploy an Azure Container Service cluster by using hello [Azure CLI 2.0](container-service-create-acs-cluster-cli.md) or hello Azure Container Service APIs.</span></span>

<span data-ttu-id="c5731-108">Bakgrundsinformation finns i [Introduktion till Azure Container Service](../container-service-intro.md).</span><span class="sxs-lookup"><span data-stu-id="c5731-108">For background, see [Azure Container Service introduction](../container-service-intro.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="c5731-109">Krav</span><span class="sxs-lookup"><span data-stu-id="c5731-109">Prerequisites</span></span>

* <span data-ttu-id="c5731-110">**Azure-prenumeration**: Om du inte har någon kan du registrera dig för en [kostnadsfri utvärderingsversion](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span><span class="sxs-lookup"><span data-stu-id="c5731-110">**Azure subscription**: If you don't have one, sign up for a [free trial](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span></span> <span data-ttu-id="c5731-111">Överväg en prenumeration utan fast avgift eller andra alternativ för ett större kluster.</span><span class="sxs-lookup"><span data-stu-id="c5731-111">For a larger cluster, consider a pay-as-you go subscription or other purchase options.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c5731-112">Din användning av Azure-prenumeration och [resurskvoter](../../azure-subscription-service-limits.md), till exempel kärnor kvoter kan begränsa hello storleken på hello-kluster som du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="c5731-112">Your Azure subscription usage and [resource quotas](../../azure-subscription-service-limits.md), such as cores quotas, can limit hello size of hello cluster you deploy.</span></span> <span data-ttu-id="c5731-113">toorequest ökad kvot, öppna ett [online kundsupport](../../azure-supportability/how-to-create-azure-support-request.md) utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="c5731-113">toorequest a quota increase, open an [online customer support request](../../azure-supportability/how-to-create-azure-support-request.md) at no charge.</span></span>
    >

* <span data-ttu-id="c5731-114">**Offentlig SSH-RSA-nyckel**: när du distribuerar via hello-portalen eller en av hello Azure quickstart mallar, måste tooprovide hello offentlig nyckel för autentisering mot Azure Container Service virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c5731-114">**SSH RSA public key**: When deploying through hello portal or one of hello Azure quickstart templates, you need tooprovide hello public key for authentication against Azure Container Service virtual machines.</span></span> <span data-ttu-id="c5731-115">toocreate SSH (Secure Shell) RSA-nycklar, se hello [OS X- och Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) eller [Windows](../../virtual-machines/linux/ssh-from-windows.md) vägledning.</span><span class="sxs-lookup"><span data-stu-id="c5731-115">toocreate Secure Shell (SSH) RSA keys, see hello [OS X and Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) or [Windows](../../virtual-machines/linux/ssh-from-windows.md) guidance.</span></span> 

* <span data-ttu-id="c5731-116">**Service principal klient-ID och Hemlig** (Kubernetes): Mer information och vägledning toocreate ett Azure Active Directory-tjänstens huvudnamn, se [om hello tjänstens huvudnamn för ett kluster med Kubernetes](../kubernetes/container-service-kubernetes-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="c5731-116">**Service principal client ID and secret** (Kubernetes only): For more information and guidance toocreate an Azure Active Directory service principal, see [About hello service principal for a Kubernetes cluster](../kubernetes/container-service-kubernetes-service-principal.md).</span></span>



## <a name="create-a-cluster-by-using-hello-azure-portal"></a><span data-ttu-id="c5731-117">Skapa ett kluster med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c5731-117">Create a cluster by using hello Azure portal</span></span>
1. <span data-ttu-id="c5731-118">Logga in toohello Azure-portalen, Välj **ny**, och Sök hello Azure Marketplace för **Azure Container Service**.</span><span class="sxs-lookup"><span data-stu-id="c5731-118">Sign in toohello Azure portal, select **New**, and search hello Azure Marketplace for **Azure Container Service**.</span></span>

    ![Azure Container Service i Marketplace](./media/container-service-deployment/acs-portal1.png)  <br />

2. <span data-ttu-id="c5731-120">Klicka på **Azure Container Service** och klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c5731-120">Click **Azure Container Service**, and click **Create**.</span></span>

3. <span data-ttu-id="c5731-121">På hello **grunderna** bladet ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="c5731-121">On hello **Basics** blade, enter hello following information:</span></span>

    * <span data-ttu-id="c5731-122">**Orchestrator**: Välj något av hello behållaren orchestrators toodeploy på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="c5731-122">**Orchestrator**: Select one of hello container orchestrators toodeploy on hello cluster.</span></span>
        * <span data-ttu-id="c5731-123">**DC/OS**: distribuerar ett DC/OS-kluster.</span><span class="sxs-lookup"><span data-stu-id="c5731-123">**DC/OS**: Deploys a DC/OS cluster.</span></span>
        * <span data-ttu-id="c5731-124">**Swarm**: distribuerar ett Docker Swarm-kluster.</span><span class="sxs-lookup"><span data-stu-id="c5731-124">**Swarm**: Deploys a Docker Swarm cluster.</span></span>
        * <span data-ttu-id="c5731-125">**Kubernetes**: Distribuerar ett Kubernetes-kluster.</span><span class="sxs-lookup"><span data-stu-id="c5731-125">**Kubernetes**: Deploys a Kubernetes cluster.</span></span>
    * <span data-ttu-id="c5731-126">**Prenumeration**: Välj en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c5731-126">**Subscription**: Select an Azure subscription.</span></span>
    * <span data-ttu-id="c5731-127">**Resursgruppen**: Anger hello namnet på en ny resursgrupp för hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="c5731-127">**Resource group**: Enter hello name of a new resource group for hello deployment.</span></span>
    * <span data-ttu-id="c5731-128">**Plats**: Välj en Azure-region för hello Azure Container Service-distributionen.</span><span class="sxs-lookup"><span data-stu-id="c5731-128">**Location**: Select an Azure region for hello Azure Container Service deployment.</span></span> <span data-ttu-id="c5731-129">Om du vill kontrollera tillgänglighet läser du [Produkttillgänglighet per region](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="c5731-129">For availability, check [Products available by region](https://azure.microsoft.com/regions/services/).</span></span>
    
    ![Grundläggande inställningar](./media/container-service-deployment/acs-portal3.png)  <br />
    
    <span data-ttu-id="c5731-131">Klicka på **OK** när du är klar tooproceed.</span><span class="sxs-lookup"><span data-stu-id="c5731-131">Click **OK** when you're ready tooproceed.</span></span>

4. <span data-ttu-id="c5731-132">På hello **Master configuration** bladet ange hello följande inställningar för hello Linux huvudnoden eller noder i klustret för hello (vissa inställningar är särskilda tooeach orchestrator):</span><span class="sxs-lookup"><span data-stu-id="c5731-132">On hello **Master configuration** blade, enter hello following settings for hello Linux master node or nodes in hello cluster (some settings are specific tooeach orchestrator):</span></span>

    * <span data-ttu-id="c5731-133">**Master DNS-namnet**: hello prefix används toocreate ett unikt fullständigt kvalificerade domännamnet (FQDN) för hello master.</span><span class="sxs-lookup"><span data-stu-id="c5731-133">**Master DNS name**: hello prefix used toocreate a unique fully qualified domain name (FQDN) for hello master.</span></span> <span data-ttu-id="c5731-134">hello master FQDN är hello formatet *prefix*hanteringsdata*plats*. cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="c5731-134">hello master FQDN is of hello form *prefix*mgmt.*location*.cloudapp.azure.com.</span></span>
    * <span data-ttu-id="c5731-135">**Användarnamnet**: hello användarnamn för ett konto på varje hello virtuella Linux-datorer i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="c5731-135">**User name**: hello user name for an account on each of hello Linux virtual machines in hello cluster.</span></span>
    * <span data-ttu-id="c5731-136">**Offentlig SSH-RSA-nyckel**: Lägg till hello offentliga nyckel toobe används för autentisering mot hello Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c5731-136">**SSH RSA public key**: Add hello public key toobe used for authentication against hello Linux virtual machines.</span></span> <span data-ttu-id="c5731-137">Det är viktigt att den här nyckeln innehåller några radbrytningar och innehåller hello `ssh-rsa` prefix.</span><span class="sxs-lookup"><span data-stu-id="c5731-137">It is important that this key contains no line breaks, and it includes hello `ssh-rsa` prefix.</span></span> <span data-ttu-id="c5731-138">Hej `username@domain` username@Domain är valfritt.</span><span class="sxs-lookup"><span data-stu-id="c5731-138">hello `username@domain` postfix is optional.</span></span> <span data-ttu-id="c5731-139">hello nyckel ska se ut ungefär hello följande: **ssh-rsa AAAAB3Nz... <>...... UcyupgH azureuser@linuxvm** .</span><span class="sxs-lookup"><span data-stu-id="c5731-139">hello key should look something like hello following: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**.</span></span> 
    * <span data-ttu-id="c5731-140">**Tjänstens huvudnamn**: Ange ett Azure Active Directory om du har valt hello Kubernetes orchestrator **Service principal klient-ID** (kallas även hello appId) och **Service principal klienthemlighet** (lösenord).</span><span class="sxs-lookup"><span data-stu-id="c5731-140">**Service principal**: If you selected hello Kubernetes orchestrator, enter an Azure Active Directory **Service principal client ID** (also called hello appId) and **Service principal client secret** (password).</span></span> <span data-ttu-id="c5731-141">Mer information finns i [om hello tjänstens huvudnamn för ett kluster med Kubernetes](../kubernetes/container-service-kubernetes-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="c5731-141">For more information, see [About hello service principal for a Kubernetes cluster](../kubernetes/container-service-kubernetes-service-principal.md).</span></span>
    * <span data-ttu-id="c5731-142">**Master-antal**: hello antal huvudservrar i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="c5731-142">**Master count**: hello number of masters in hello cluster.</span></span>
    * <span data-ttu-id="c5731-143">**Diagnostik för Virtuella datorer**: för vissa orchestrators kan du aktivera diagnostik för Virtuella datorer på hello huvudservrar.</span><span class="sxs-lookup"><span data-stu-id="c5731-143">**VM diagnostics**: For some orchestrators, you can enable VM diagnostics on hello masters.</span></span>

    ![Konfiguration av huvudservrar](./media/container-service-deployment/acs-portal4.png)  <br />

    <span data-ttu-id="c5731-145">Klicka på **OK** när du är klar tooproceed.</span><span class="sxs-lookup"><span data-stu-id="c5731-145">Click **OK** when you're ready tooproceed.</span></span>

5. <span data-ttu-id="c5731-146">På hello **agentkonfiguration** bladet ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="c5731-146">On hello **Agent configuration** blade, enter hello following information:</span></span>

    * <span data-ttu-id="c5731-147">**Agentantal**: det här värdet är för Docker Swarm och Kubernetes hello inledande antalet agenter i agentskalningsuppsättningen hello.</span><span class="sxs-lookup"><span data-stu-id="c5731-147">**Agent count**: For Docker Swarm and Kubernetes, this value is hello initial number of agents in hello agent scale set.</span></span> <span data-ttu-id="c5731-148">DC/OS är det hello inledande antalet agenter i en privat skalningsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="c5731-148">For DC/OS, it is hello initial number of agents in a private scale set.</span></span> <span data-ttu-id="c5731-149">Dessutom skapas en offentlig skalningsuppsättning för DC/OS som innehåller ett förinställt antal agenter.</span><span class="sxs-lookup"><span data-stu-id="c5731-149">Additionally, a public scale set is created for DC/OS, which contains a predetermined number of agents.</span></span> <span data-ttu-id="c5731-150">hello antalet agenter i den här offentliga skalningsuppsättningen avgörs av hello antal huvudservrar i klustret hello: en offentlig agent för en överordnad och två offentliga agenter för tre eller fem huvudservrar.</span><span class="sxs-lookup"><span data-stu-id="c5731-150">hello number of agents in this public scale set is determined by hello number of masters in hello cluster: one public agent for one master, and two public agents for three or five masters.</span></span>
    * <span data-ttu-id="c5731-151">**Agenten virtuella datorstorleken**: hello storleken på hello agentens virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c5731-151">**Agent virtual machine size**: hello size of hello agent virtual machines.</span></span>
    * <span data-ttu-id="c5731-152">**Operativsystemet**: den här inställningen är för närvarande endast tillgängligt om du har valt hello Kubernetes orchestrator.</span><span class="sxs-lookup"><span data-stu-id="c5731-152">**Operating system**: This setting is currently available only if you selected hello Kubernetes orchestrator.</span></span> <span data-ttu-id="c5731-153">Välj en Linux-distribution eller en Windows Server operativsystem toorun på hello agenter.</span><span class="sxs-lookup"><span data-stu-id="c5731-153">Choose either a Linux distribution or a Windows Server operating system toorun on hello agents.</span></span> <span data-ttu-id="c5731-154">Den här inställningen bestämmer om ditt kluster kan köra Linux- eller Windows-behållarappar.</span><span class="sxs-lookup"><span data-stu-id="c5731-154">This setting determines whether your cluster can run Linux or Windows container apps.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="c5731-155">Stöd för Windows-behållare finns i förhandsgranskningen för Kubernetes-kluster.</span><span class="sxs-lookup"><span data-stu-id="c5731-155">Windows container support is in preview for Kubernetes clusters.</span></span> <span data-ttu-id="c5731-156">På DC-/OS- och Swarm-kluster stöds för närvarande endast Linux-agenter i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="c5731-156">On DC/OS and Swarm clusters, only Linux agents are currently supported in Azure Container Service.</span></span>

    * <span data-ttu-id="c5731-157">**Autentiseringsuppgifter för agenten**: Ange en administratör om du har valt hello Windows-operativsystemet **användarnamn** och **lösenord** för hello agent virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c5731-157">**Agent credentials**: If you selected hello Windows operating system, enter an administrator **User name** and **Password** for hello agent VMs.</span></span> 

    ![Agentkonfiguration](./media/container-service-deployment/acs-portal5.png)  <br />

    <span data-ttu-id="c5731-159">Klicka på **OK** när du är klar tooproceed.</span><span class="sxs-lookup"><span data-stu-id="c5731-159">Click **OK** when you're ready tooproceed.</span></span>

6. <span data-ttu-id="c5731-160">Klicka på **OK** när tjänsteverifieringen är klar.</span><span class="sxs-lookup"><span data-stu-id="c5731-160">After service validation finishes, click **OK**.</span></span>

    ![Validering](./media/container-service-deployment/acs-portal6.png)  <br />

7. <span data-ttu-id="c5731-162">Granska hello villkoren.</span><span class="sxs-lookup"><span data-stu-id="c5731-162">Review hello terms.</span></span> <span data-ttu-id="c5731-163">Distributionsprocess för toostart hello, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c5731-163">toostart hello deployment process, click **Create**.</span></span>

    <span data-ttu-id="c5731-164">Om du har valt toopin hello distribution toohello Azure-portalen kan du se hello distributionens status.</span><span class="sxs-lookup"><span data-stu-id="c5731-164">If you've elected toopin hello deployment toohello Azure portal, you can see hello deployment status.</span></span>

    ![Status för distribution](./media/container-service-deployment/acs-portal8.png)  <br />

<span data-ttu-id="c5731-166">hello distributionen tar flera minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="c5731-166">hello deployment takes several minutes toocomplete.</span></span> <span data-ttu-id="c5731-167">Hello Azure Container Service-kluster är redo för användning.</span><span class="sxs-lookup"><span data-stu-id="c5731-167">Then, hello Azure Container Service cluster is ready for use.</span></span>


## <a name="create-a-cluster-by-using-a-quickstart-template"></a><span data-ttu-id="c5731-168">Skapa ett kluster med en snabbstartsmall</span><span class="sxs-lookup"><span data-stu-id="c5731-168">Create a cluster by using a quickstart template</span></span>
<span data-ttu-id="c5731-169">Azure quickstart mallar är tillgängliga toodeploy ett kluster i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="c5731-169">Azure quickstart templates are available toodeploy a cluster in Azure Container Service.</span></span> <span data-ttu-id="c5731-170">hello angiven snabbstartsmallar kan vara ändrade tooinclude ytterligare eller avancerade Azure-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c5731-170">hello provided quickstart templates can be modified tooinclude additional or advanced Azure configuration.</span></span> <span data-ttu-id="c5731-171">toocreate ett Azure Container Service-kluster med hjälp av en mall för Azure quickstart du behöver en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c5731-171">toocreate an Azure Container Service cluster by using an Azure quickstart template, you need an Azure subscription.</span></span> <span data-ttu-id="c5731-172">Om du inte har någon kan du registrera dig för en [kostnadsfri utvärderingsversion](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span><span class="sxs-lookup"><span data-stu-id="c5731-172">If you don't have one, then sign up for a [free trial](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span></span> 

<span data-ttu-id="c5731-173">Följ dessa steg toodeploy ett kluster med en mall och hello Azure CLI 2.0 (se [installations- och instruktioner](/cli/azure/install-az-cli2)).</span><span class="sxs-lookup"><span data-stu-id="c5731-173">Follow these steps toodeploy a cluster using a template and hello Azure CLI 2.0 (see [installation and setup instructions](/cli/azure/install-az-cli2)).</span></span>

> [!NOTE] 
> <span data-ttu-id="c5731-174">Om du är på en Windows-dator kan använda du liknande steg toodeploy en mall med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c5731-174">If you're on a Windows system, you can use similar steps toodeploy a template using Azure PowerShell.</span></span> <span data-ttu-id="c5731-175">Se anvisningarna senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c5731-175">See steps later in this section.</span></span> <span data-ttu-id="c5731-176">Du kan också distribuera en mall med hello [portal](../../azure-resource-manager/resource-group-template-deploy-portal.md) eller andra metoder.</span><span class="sxs-lookup"><span data-stu-id="c5731-176">You can also deploy a template through hello [portal](../../azure-resource-manager/resource-group-template-deploy-portal.md) or other methods.</span></span>

1. <span data-ttu-id="c5731-177">toodeploy ett DC/OS, Docker Swarm eller Kubernetes kluster, Välj en av hello tillgängliga quickstart mallar från GitHub.</span><span class="sxs-lookup"><span data-stu-id="c5731-177">toodeploy a DC/OS, Docker Swarm, or Kubernetes cluster, select one of hello available quickstart templates from GitHub.</span></span> <span data-ttu-id="c5731-178">En ofullständig lista finns nedan.</span><span class="sxs-lookup"><span data-stu-id="c5731-178">A partial list follows.</span></span> <span data-ttu-id="c5731-179">hello DC/OS och Swarm mallar är hello samma förutom hello standardvalet av orchestrator.</span><span class="sxs-lookup"><span data-stu-id="c5731-179">hello DC/OS and Swarm templates are hello same, except for hello default orchestrator selection.</span></span>

    * [<span data-ttu-id="c5731-180">DC/OS-mall</span><span class="sxs-lookup"><span data-stu-id="c5731-180">DC/OS template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [<span data-ttu-id="c5731-181">Swarm-mall</span><span class="sxs-lookup"><span data-stu-id="c5731-181">Swarm template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [<span data-ttu-id="c5731-182">Kubernetes-mall</span><span class="sxs-lookup"><span data-stu-id="c5731-182">Kubernetes template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. <span data-ttu-id="c5731-183">Logga in tooyour Azure-konto (`az login`), och kontrollera att hello Azure CLI är anslutna tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c5731-183">Log in tooyour Azure account (`az login`), and make sure that hello Azure CLI is connected tooyour Azure subscription.</span></span> <span data-ttu-id="c5731-184">Du kan se hello standard prenumeration med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c5731-184">You can see hello default subscription by using hello following command:</span></span>

    ```azurecli
    az account show
    ```
    
    <span data-ttu-id="c5731-185">Om du har mer än en prenumeration och behöver tooset en annan standard prenumeration kör `az account set --subscription` och ange hello prenumerations-ID eller namn.</span><span class="sxs-lookup"><span data-stu-id="c5731-185">If you have more than one subscription and need tooset a different default subscription, run `az account set --subscription` and specify hello subscription ID or name.</span></span>

3. <span data-ttu-id="c5731-186">Ett bra tips är att använda en ny resursgrupp för hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="c5731-186">As a best practice, use a new resource group for hello deployment.</span></span> <span data-ttu-id="c5731-187">toocreate en resursgrupp, använda hello `az group create` kommando anger resursgruppens namn och plats:</span><span class="sxs-lookup"><span data-stu-id="c5731-187">toocreate a resource group, use hello `az group create` command specify a resource group name and location:</span></span> 

    ```azurecli
    az group create --name "RESOURCE_GROUP" --location "LOCATION"
    ```

4. <span data-ttu-id="c5731-188">Skapa en JSON-fil som innehåller hello nödvändiga parametrar.</span><span class="sxs-lookup"><span data-stu-id="c5731-188">Create a JSON file containing hello required template parameters.</span></span> <span data-ttu-id="c5731-189">Hämta hello parameterfil som heter `azuredeploy.parameters.json` som medföljer hello Azure Container Service-mallen `azuredeploy.json` i GitHub.</span><span class="sxs-lookup"><span data-stu-id="c5731-189">Download hello parameters file named `azuredeploy.parameters.json` that accompanies hello Azure Container Service template `azuredeploy.json` in GitHub.</span></span> <span data-ttu-id="c5731-190">Ange de parametervärden som krävs för klustret.</span><span class="sxs-lookup"><span data-stu-id="c5731-190">Enter required parameter values for your cluster.</span></span> 

    <span data-ttu-id="c5731-191">Till exempel toouse hello [DC/OS-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos), ange parametervärden för `dnsNamePrefix` och `sshRSAPublicKey`.</span><span class="sxs-lookup"><span data-stu-id="c5731-191">For example, toouse hello [DC/OS template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos), supply parameter values for `dnsNamePrefix` and `sshRSAPublicKey`.</span></span> <span data-ttu-id="c5731-192">Se hello beskrivningar i `azuredeploy.json` och alternativ för andra parametrar.</span><span class="sxs-lookup"><span data-stu-id="c5731-192">See hello descriptions in `azuredeploy.json` and options for other parameters.</span></span>  
 

5. <span data-ttu-id="c5731-193">Skapa ett Container Service-kluster genom att skicka hello distribution parameterfilen med hello följande kommando, där:</span><span class="sxs-lookup"><span data-stu-id="c5731-193">Create a Container Service cluster by passing hello deployment parameters file with hello following command, where:</span></span>

    * <span data-ttu-id="c5731-194">**RESOURCE_GROUP** är hello resursgrupp som du skapade i föregående steg i hello hello namn.</span><span class="sxs-lookup"><span data-stu-id="c5731-194">**RESOURCE_GROUP** is hello name of hello resource group that you created in hello previous step.</span></span>
    * <span data-ttu-id="c5731-195">**DEPLOYMENT_NAME** (valfritt) är ett namn som du ger toohello distribution.</span><span class="sxs-lookup"><span data-stu-id="c5731-195">**DEPLOYMENT_NAME** (optional) is a name you give toohello deployment.</span></span>
    * <span data-ttu-id="c5731-196">**TEMPLATE_URI** är hello platsen för distributionsfilen hello `azuredeploy.json`.</span><span class="sxs-lookup"><span data-stu-id="c5731-196">**TEMPLATE_URI** is hello location of hello deployment file `azuredeploy.json`.</span></span> <span data-ttu-id="c5731-197">Den här URI måste vara hello Raw-filen, inte en pekare toohello GitHub UI.</span><span class="sxs-lookup"><span data-stu-id="c5731-197">This URI must be hello Raw file, not a pointer toohello GitHub UI.</span></span> <span data-ttu-id="c5731-198">toofind URI: N, väljer hello `azuredeploy.json` filen i GitHub och på hello **Raw** knappen.</span><span class="sxs-lookup"><span data-stu-id="c5731-198">toofind this URI, select hello `azuredeploy.json` file in GitHub, and click hello **Raw** button.</span></span>  

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters @azuredeploy.parameters.json
    ```

    <span data-ttu-id="c5731-199">Du kan också ange parametrar som en JSON-formaterad sträng på hello kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="c5731-199">You can also provide parameters as a JSON-formatted string on hello command line.</span></span> <span data-ttu-id="c5731-200">Använd ett kommando liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="c5731-200">Use a command similar toohello following:</span></span>

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters "{ \"param1\": {\"value1\"} … }"
    ```

    > [!NOTE]
    > <span data-ttu-id="c5731-201">hello distributionen tar flera minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="c5731-201">hello deployment takes several minutes toocomplete.</span></span>
    > 

### <a name="equivalent-powershell-commands"></a><span data-ttu-id="c5731-202">Motsvarande PowerShell-kommandon</span><span class="sxs-lookup"><span data-stu-id="c5731-202">Equivalent PowerShell commands</span></span>
<span data-ttu-id="c5731-203">Du kan även distribuera en mall för ett Azure Container Service-kluster med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c5731-203">You can also deploy an Azure Container Service cluster template with PowerShell.</span></span> <span data-ttu-id="c5731-204">Det här dokumentet är baserad på hello version 1.0 [Azure PowerShell-modulen](https://azure.microsoft.com/blog/azps-1-0/).</span><span class="sxs-lookup"><span data-stu-id="c5731-204">This document is based on hello version 1.0 [Azure PowerShell module](https://azure.microsoft.com/blog/azps-1-0/).</span></span>

1. <span data-ttu-id="c5731-205">toodeploy ett DC/OS, Docker Swarm eller Kubernetes kluster, Välj en av hello tillgängliga quickstart mallar från GitHub.</span><span class="sxs-lookup"><span data-stu-id="c5731-205">toodeploy a DC/OS, Docker Swarm, or Kubernetes cluster, select one of hello available quickstart templates from GitHub.</span></span> <span data-ttu-id="c5731-206">En ofullständig lista finns nedan.</span><span class="sxs-lookup"><span data-stu-id="c5731-206">A partial list follows.</span></span> <span data-ttu-id="c5731-207">Observera att hello DC/OS och Swarm mallar är hello detsamma, med undantag för hello av hello standardvalet av orchestrator.</span><span class="sxs-lookup"><span data-stu-id="c5731-207">Note that hello DC/OS and Swarm templates are hello same, with hello exception of hello default orchestrator selection.</span></span>

    * [<span data-ttu-id="c5731-208">DC/OS-mall</span><span class="sxs-lookup"><span data-stu-id="c5731-208">DC/OS template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [<span data-ttu-id="c5731-209">Swarm-mall</span><span class="sxs-lookup"><span data-stu-id="c5731-209">Swarm template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [<span data-ttu-id="c5731-210">Kubernetes-mall</span><span class="sxs-lookup"><span data-stu-id="c5731-210">Kubernetes template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. <span data-ttu-id="c5731-211">Kontrollera att PowerShell-sessionen har loggats in tooAzure innan du skapar ett kluster i Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c5731-211">Before creating a cluster in your Azure subscription, verify that your PowerShell session has been signed in tooAzure.</span></span> <span data-ttu-id="c5731-212">Du kan göra detta med hello `Get-AzureRMSubscription` kommando:</span><span class="sxs-lookup"><span data-stu-id="c5731-212">You can do this with hello `Get-AzureRMSubscription` command:</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

3. <span data-ttu-id="c5731-213">Om du behöver toosign i tooAzure använder hello `Login-AzureRMAccount` kommando:</span><span class="sxs-lookup"><span data-stu-id="c5731-213">If you need toosign in tooAzure, use hello `Login-AzureRMAccount` command:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

4. <span data-ttu-id="c5731-214">Ett bra tips är att använda en ny resursgrupp för hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="c5731-214">As a best practice, use a new resource group for hello deployment.</span></span> <span data-ttu-id="c5731-215">toocreate en resursgrupp, använda hello `New-AzureRmResourceGroup` kommando och ange en resurs grupp namn och mål region:</span><span class="sxs-lookup"><span data-stu-id="c5731-215">toocreate a resource group, use hello `New-AzureRmResourceGroup` command, and specify a resource group name and destination region:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
    ```

5. <span data-ttu-id="c5731-216">När du har skapat en resursgrupp kan du skapa klustret med hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="c5731-216">After you create a resource group, you can create your cluster with hello following command.</span></span> <span data-ttu-id="c5731-217">hello-URI för hello önskade mallen anges med hello `-TemplateUri` parameter.</span><span class="sxs-lookup"><span data-stu-id="c5731-217">hello URI of hello desired template is specified with hello `-TemplateUri` parameter.</span></span> <span data-ttu-id="c5731-218">När du kör det här kommandot uppmanar PowerShell dig att ange parametervärden för distributionen.</span><span class="sxs-lookup"><span data-stu-id="c5731-218">When you run this command, PowerShell prompts you for deployment parameter values.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
    ```

#### <a name="provide-template-parameters"></a><span data-ttu-id="c5731-219">Ange mallparametrar</span><span class="sxs-lookup"><span data-stu-id="c5731-219">Provide template parameters</span></span>
<span data-ttu-id="c5731-220">Om du är bekant med PowerShell vet du att du kan gå igenom hello tillgängliga parametrar för en cmdlet genom att skriva ett minustecken (-) och sedan trycka på TABB-tangenten hello.</span><span class="sxs-lookup"><span data-stu-id="c5731-220">If you're familiar with PowerShell, you know that you can cycle through hello available parameters for a cmdlet by typing a minus sign (-) and then pressing hello TAB key.</span></span> <span data-ttu-id="c5731-221">Den här metoden går även att använda med parametrar som du anger i mallen.</span><span class="sxs-lookup"><span data-stu-id="c5731-221">This same functionality also works with parameters that you define in your template.</span></span> <span data-ttu-id="c5731-222">Så snart du skriver hello mallnamn hello cmdlet hämtar hello mallen, Parsar hello parametrar och lägger till hello mallen parametrar toohello kommandot dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="c5731-222">As soon as you type hello template name, hello cmdlet fetches hello template, parses hello parameters, and adds hello template parameters toohello command dynamically.</span></span> <span data-ttu-id="c5731-223">Detta gör det enkelt toospecify hello parametervärden för mallen.</span><span class="sxs-lookup"><span data-stu-id="c5731-223">This makes it easy toospecify hello template parameter values.</span></span> <span data-ttu-id="c5731-224">Och om du glömmer ett nödvändigt parametervärde efterfrågar PowerShell hello värde.</span><span class="sxs-lookup"><span data-stu-id="c5731-224">And, if you forget a required parameter value, PowerShell prompts you for hello value.</span></span>

<span data-ttu-id="c5731-225">Här är hello fullständiga kommandot inklusive parametrar.</span><span class="sxs-lookup"><span data-stu-id="c5731-225">Here is hello full command, with parameters included.</span></span> <span data-ttu-id="c5731-226">Ange egna värden för hello namnen på de hello resurser.</span><span class="sxs-lookup"><span data-stu-id="c5731-226">Provide your own values for hello names of hello resources.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a><span data-ttu-id="c5731-227">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c5731-227">Next steps</span></span>
<span data-ttu-id="c5731-228">Nu när du har ett fungerande kluster kan du visa dessa dokument för anslutnings- och hanteringsinformation:</span><span class="sxs-lookup"><span data-stu-id="c5731-228">Now that you have a functioning cluster, see these documents for connection and management details:</span></span>

* [<span data-ttu-id="c5731-229">Ansluta tooan Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="c5731-229">Connect tooan Azure Container Service cluster</span></span>](../container-service-connect.md)
* [<span data-ttu-id="c5731-230">Arbeta med Azure Container Service och DC/OS</span><span class="sxs-lookup"><span data-stu-id="c5731-230">Work with Azure Container Service and DC/OS</span></span>](container-service-mesos-marathon-rest.md)
* [<span data-ttu-id="c5731-231">Arbeta med Azure Container Service och Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="c5731-231">Work with Azure Container Service and Docker Swarm</span></span>](container-service-docker-swarm.md)
* [<span data-ttu-id="c5731-232">Arbeta med Azure Container Service och Kubernetes</span><span class="sxs-lookup"><span data-stu-id="c5731-232">Work with Azure Container Service and Kubernetes</span></span>](../kubernetes/container-service-kubernetes-walkthrough.md)
