---
title: aaaCI/CD med Azure Container Service och Swarm | Microsoft Docs
description: "Använda Azure Container Service med Docker Swarm, ett Azure Container registret och Visual Studio Team Services toodeliver kontinuerligt ett flera behållare .NET Core-program"
services: container-service
documentationcenter: " "
author: jcorioland
manager: pierlag
tags: acs, azure-container-service
keywords: "Docker behållare Micro-tjänster, Swarm, Azure, Visual Studio Team Services DevOps"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: 35348800aa620469fb62ab3e5a02b33834359446
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="full-cicd-pipeline-toodeploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a><span data-ttu-id="c3fa2-104">Fullständig CI/CD-pipeline toodeploy ett program för flera behållare på Azure Container Service med Docker Swarm med hjälp av Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="c3fa2-104">Full CI/CD pipeline toodeploy a multi-container application on Azure Container Service with Docker Swarm using Visual Studio Team Services</span></span>

<span data-ttu-id="c3fa2-105">En av hello största utmaningarna när utveckla moderna applikationer för hello molnet kan toodeliver dessa program kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-105">One of hello biggest challenges when developing modern applications for hello cloud is being able toodeliver these applications continuously.</span></span> <span data-ttu-id="c3fa2-106">I den här artikeln lär du dig hur du skapar tooimplement en fullständig kontinuerlig integrering och distribution (CI/CD) pipeline med hjälp av Azure Container Service med Docker Swarm Azure Container registret och Visual Studio Team Services och versionshantering.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-106">In this article, you learn how tooimplement a full continuous integration and deployment (CI/CD) pipeline using Azure Container Service with Docker Swarm, Azure Container Registry, and Visual Studio Team Services build and release management.</span></span>

<span data-ttu-id="c3fa2-107">Den här artikeln är baserad på ett enkelt program tillgängliga på [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), utvecklade med ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-107">This article is based on a simple application, available on [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), developed with ASP.NET Core.</span></span> <span data-ttu-id="c3fa2-108">hello program består av fyra olika tjänster: tre webb-API: er och en frontwebb:</span><span class="sxs-lookup"><span data-stu-id="c3fa2-108">hello application is composed of four different services: three web APIs and one web front end:</span></span>

![MyShop exempelprogrammet](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

<span data-ttu-id="c3fa2-110">hello mål är toodeliver programmet kontinuerligt i ett Docker Swarm-kluster med hjälp av Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-110">hello objective is toodeliver this application continuously in a Docker Swarm cluster, using Visual Studio Team Services.</span></span> <span data-ttu-id="c3fa2-111">hello följande figur information denna kontinuerlig leverans pipeline:</span><span class="sxs-lookup"><span data-stu-id="c3fa2-111">hello following figure details this continuous delivery pipeline:</span></span>

![MyShop exempelprogrammet](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

<span data-ttu-id="c3fa2-113">Här följer en kort beskrivning av hello steg:</span><span class="sxs-lookup"><span data-stu-id="c3fa2-113">Here is a brief explanation of hello steps:</span></span>

1. <span data-ttu-id="c3fa2-114">Koden ändringarna har bekräftats toohello källdatabasen kod (här GitHub)</span><span class="sxs-lookup"><span data-stu-id="c3fa2-114">Code changes are committed toohello source code repository (here, GitHub)</span></span> 
2. <span data-ttu-id="c3fa2-115">GitHub utlöser en version i Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="c3fa2-115">GitHub triggers a build in Visual Studio Team Services</span></span> 
3. <span data-ttu-id="c3fa2-116">Visual Studio Team Services hämtar hello senaste versionen av hello källor och skapar alla hello-avbildningar som ska utgöra hello program</span><span class="sxs-lookup"><span data-stu-id="c3fa2-116">Visual Studio Team Services gets hello latest version of hello sources and builds all hello images that compose hello application</span></span> 
4. <span data-ttu-id="c3fa2-117">Visual Studio Team Services skickar varje avbildning tooa Docker-registret som skapats med hjälp av hello Azure Container Registry-tjänsten</span><span class="sxs-lookup"><span data-stu-id="c3fa2-117">Visual Studio Team Services pushes each image tooa Docker registry created using hello Azure Container Registry service</span></span> 
5. <span data-ttu-id="c3fa2-118">Visual Studio Team Services utlöser en ny version</span><span class="sxs-lookup"><span data-stu-id="c3fa2-118">Visual Studio Team Services triggers a new release</span></span> 
6. <span data-ttu-id="c3fa2-119">hello körs vissa kommandon med hjälp av SSH på hello Azure container service master klusternod</span><span class="sxs-lookup"><span data-stu-id="c3fa2-119">hello release runs some commands using SSH on hello Azure container service cluster master node</span></span> 
7. <span data-ttu-id="c3fa2-120">Docker Swarm på hello klustret hämtar hello senaste versionen av hello bilder</span><span class="sxs-lookup"><span data-stu-id="c3fa2-120">Docker Swarm on hello cluster pulls hello latest version of hello images</span></span> 
8. <span data-ttu-id="c3fa2-121">hello ny version av programmet hello distribueras med Docker Compose</span><span class="sxs-lookup"><span data-stu-id="c3fa2-121">hello new version of hello application is deployed using Docker Compose</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c3fa2-122">Krav</span><span class="sxs-lookup"><span data-stu-id="c3fa2-122">Prerequisites</span></span>

<span data-ttu-id="c3fa2-123">Innan du påbörjar de här självstudierna måste toocomplete hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="c3fa2-123">Before starting this tutorial, you need toocomplete hello following tasks:</span></span>

- [<span data-ttu-id="c3fa2-124">Skapa ett Swarm-kluster i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="c3fa2-124">Create a Swarm cluster in Azure Container Service</span></span>](container-service-deployment.md)
- [<span data-ttu-id="c3fa2-125">Ansluta med hello Swarm-kluster i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="c3fa2-125">Connect with hello Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)
- [<span data-ttu-id="c3fa2-126">Skapa en Azure-behållaren registernyckel</span><span class="sxs-lookup"><span data-stu-id="c3fa2-126">Create an Azure container registry</span></span>](../../container-registry/container-registry-get-started-portal.md)
- [<span data-ttu-id="c3fa2-127">Har en Visual Studio Team Services-konto och team projekt</span><span class="sxs-lookup"><span data-stu-id="c3fa2-127">Have a Visual Studio Team Services account and team project created</span></span>](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [<span data-ttu-id="c3fa2-128">Duplicera hello GitHub-lagringsplatsen tooyour GitHub-konto</span><span class="sxs-lookup"><span data-stu-id="c3fa2-128">Fork hello GitHub repository tooyour GitHub account</span></span>](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

<span data-ttu-id="c3fa2-129">Du måste också en Ubuntu (14.04 eller 16.04)-dator med Docker installerad.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-129">You also need an Ubuntu (14.04 or 16.04) machine with Docker installed.</span></span> <span data-ttu-id="c3fa2-130">Den här datorn används av Visual Studio Team Services under hello bygga och släpper processer.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-130">This machine is used by Visual Studio Team Services during hello build and release processes.</span></span> <span data-ttu-id="c3fa2-131">Enkelriktade toocreate den här datorn är toouse hello avbildning tillgänglig i hello [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/).</span><span class="sxs-lookup"><span data-stu-id="c3fa2-131">One way toocreate this machine is toouse hello image available in hello [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/).</span></span> 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a><span data-ttu-id="c3fa2-132">Steg 1: Konfigurera Visual Studio Team Services-konto</span><span class="sxs-lookup"><span data-stu-id="c3fa2-132">Step 1: Configure your Visual Studio Team Services account</span></span> 

<span data-ttu-id="c3fa2-133">I det här avsnittet kan du konfigurera Visual Studio Team Services-konto.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-133">In this section, you configure your Visual Studio Team Services account.</span></span>

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a><span data-ttu-id="c3fa2-134">Konfigurera en Visual Studio Team Services Linux build-agent</span><span class="sxs-lookup"><span data-stu-id="c3fa2-134">Configure a Visual Studio Team Services Linux build agent</span></span>

<span data-ttu-id="c3fa2-135">toocreate Docker-bilder och push dessa avbildningar till en Azure-behållaren registret från en Visual Studio Team Services skapa måste tooregister en Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-135">toocreate Docker images and push these images into an Azure container registry from a Visual Studio Team Services build, you need tooregister a Linux agent.</span></span> <span data-ttu-id="c3fa2-136">Du har dessa installationsalternativ:</span><span class="sxs-lookup"><span data-stu-id="c3fa2-136">You have these installation options:</span></span>

* [<span data-ttu-id="c3fa2-137">Distribuera en agent på Linux</span><span class="sxs-lookup"><span data-stu-id="c3fa2-137">Deploy an agent on Linux</span></span>](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [<span data-ttu-id="c3fa2-138">Använda Docker toorun hello VSTS agent</span><span class="sxs-lookup"><span data-stu-id="c3fa2-138">Use Docker toorun hello VSTS agent</span></span>](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-hello-docker-integration-vsts-extension"></a><span data-ttu-id="c3fa2-139">Installera hello Docker integrering VSTS tillägg</span><span class="sxs-lookup"><span data-stu-id="c3fa2-139">Install hello Docker Integration VSTS extension</span></span>

<span data-ttu-id="c3fa2-140">Microsoft tillhandahåller en VSTS tillägget toowork med Docker i version och utgåva processer.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-140">Microsoft provides a VSTS extension toowork with Docker in build and release processes.</span></span> <span data-ttu-id="c3fa2-141">Det här tillägget är tillgängligt i hello [VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker).</span><span class="sxs-lookup"><span data-stu-id="c3fa2-141">This extension is available in hello [VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker).</span></span> <span data-ttu-id="c3fa2-142">Klicka på **installera** tooadd tillägget tooyour VSTS kontot:</span><span class="sxs-lookup"><span data-stu-id="c3fa2-142">Click **Install** tooadd this extension tooyour VSTS account:</span></span>

![Installera hello Docker-integrering](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

<span data-ttu-id="c3fa2-144">Du uppmanas tooconnect tooyour VSTS konto med hjälp av dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-144">You are asked tooconnect tooyour VSTS account using your credentials.</span></span> 

### <a name="connect-visual-studio-team-services-and-github"></a><span data-ttu-id="c3fa2-145">Ansluta Visual Studio Team Services och GitHub</span><span class="sxs-lookup"><span data-stu-id="c3fa2-145">Connect Visual Studio Team Services and GitHub</span></span>

<span data-ttu-id="c3fa2-146">Upprätta en anslutning mellan projektet VSTS och GitHub-konto.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-146">Set up a connection between your VSTS project and your GitHub account.</span></span>

1. <span data-ttu-id="c3fa2-147">I ditt projekt i Visual Studio Team Services, klickar du på hello **inställningar** ikon i hello verktygsfältet och välj **Services**.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-147">In your Visual Studio Team Services project, click hello **Settings** icon in hello toolbar, and select **Services**.</span></span>

    ![Visual Studio Team Services - extern anslutning](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

2. <span data-ttu-id="c3fa2-149">Klicka på vänster hello **nya tjänstslutpunkten** > **GitHub**.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-149">On hello left, click **New Service Endpoint** > **GitHub**.</span></span>

    ![Visual Studio Team Services - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

3. <span data-ttu-id="c3fa2-151">tooauthorize VSTS toowork med ditt GitHub-konto, klickar du på **auktorisera** och följa hello i hello-fönster som öppnas.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-151">tooauthorize VSTS toowork with your GitHub account, click **Authorize** and follow hello procedure in hello window that opens.</span></span>

    ![Visual Studio Team Services - auktorisera GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-tooyour-azure-container-registry-and-azure-container-service-cluster"></a><span data-ttu-id="c3fa2-153">Ansluta VSTS tooyour Azure-behållaren registret och Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="c3fa2-153">Connect VSTS tooyour Azure container registry and Azure Container Service cluster</span></span>

<span data-ttu-id="c3fa2-154">hello sista stegen innan du hämtar till hello CI/CD-pipeline är tooconfigure externa anslutningar tooyour behållare registret och din Docker Swarm-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-154">hello last steps before getting into hello CI/CD pipeline are tooconfigure external connections tooyour container registry and your Docker Swarm cluster in Azure.</span></span> 

1. <span data-ttu-id="c3fa2-155">I hello **Services** inställningarna för ditt projekt i Visual Studio Team Services, lägga till en tjänstslutpunkt av typen **Docker registret**.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-155">In hello **Services** settings of your Visual Studio Team Services project, add a service endpoint of type **Docker Registry**.</span></span> 

2. <span data-ttu-id="c3fa2-156">Ange hello URL och hello autentiseringsuppgifter registret Azure-behållaren i hello popup-fönstret som öppnas.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-156">In hello popup that opens, enter hello URL and hello credentials of your Azure container registry.</span></span>

    ![Visual Studio Team Services - Docker-registret](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

3. <span data-ttu-id="c3fa2-158">Lägga till en slutpunkt av typen för Docker Swarm-kluster hello **SSH**.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-158">For hello Docker Swarm cluster, add an endpoint of type **SSH**.</span></span> <span data-ttu-id="c3fa2-159">Ange hello SSH-anslutningsinformationen för Swarm-klustret.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-159">Then enter hello SSH connection information of your Swarm cluster.</span></span>

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

<span data-ttu-id="c3fa2-161">Alla hello-konfigurationen är nu klar.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-161">All hello configuration is done now.</span></span> <span data-ttu-id="c3fa2-162">Skapa hello CI/CD-pipeline som skapar och distribuerar hello programmet toohello Docker Swarm-kluster i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-162">In hello next steps, you create hello CI/CD pipeline that builds and deploys hello application toohello Docker Swarm cluster.</span></span> 

## <a name="step-2-create-hello-build-definition"></a><span data-ttu-id="c3fa2-163">Steg 2: Skapa hello build-definition</span><span class="sxs-lookup"><span data-stu-id="c3fa2-163">Step 2: Create hello build definition</span></span>

<span data-ttu-id="c3fa2-164">I det här steget du ställer in en build definitionfor VSTS-projektet och definiera hello build arbetsflöde för bilderna behållare</span><span class="sxs-lookup"><span data-stu-id="c3fa2-164">In this step, you set up a build definitionfor your VSTS project and define hello build workflow for your container images</span></span>

### <a name="initial-definition-setup"></a><span data-ttu-id="c3fa2-165">Inledande definition installationen</span><span class="sxs-lookup"><span data-stu-id="c3fa2-165">Initial definition setup</span></span>

1. <span data-ttu-id="c3fa2-166">toocreate en build-definition ansluta tooyour Visual Studio Team Services projektet och klicka på **Skapa & släpper**.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-166">toocreate a build definition, connect tooyour Visual Studio Team Services project and click **Build & Release**.</span></span> 

2. <span data-ttu-id="c3fa2-167">I hello **skapa definitioner** klickar du på **+ ny**.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-167">In hello **Build definitions** section, click **+ New**.</span></span> <span data-ttu-id="c3fa2-168">Välj hello **tom** mall.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-168">Select hello **Empty** template.</span></span>

    ![Visual Studio Team Services - nya skapa Definition](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

3. <span data-ttu-id="c3fa2-170">Konfigurera hello ny version med en källa för GitHub-lagringsplatsen, kontrollera **kontinuerlig integration**, och välj hello agent kö där du har registrerat din Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-170">Configure hello new build with a GitHub repository source, check **Continuous integration**, and select hello agent queue where you registered your Linux agent.</span></span> <span data-ttu-id="c3fa2-171">Klicka på **skapa** toocreate hello skapa definition.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-171">Click **Create** toocreate hello build definition.</span></span>

    ![Visual Studio Team Services - skapa Build-Definition](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

4. <span data-ttu-id="c3fa2-173">På hello **skapa definitioner** sidan, först öppna hello **databasen** och konfigurera hello build toouse hello förgrening av hello MyShop projekt som du skapade i hello krav.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-173">On hello **Build Definitions** page, first open hello **Repository** tab and configure hello build toouse hello fork of hello MyShop project that you created in hello prerequisites.</span></span> <span data-ttu-id="c3fa2-174">Kontrollera att du väljer *acs-dokumenten* som hello **standardförgrening**.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-174">Make sure that you select *acs-docs* as hello **Default branch**.</span></span>

    ![Visual Studio Team Services - databasen Versionskonfiguration](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

5. <span data-ttu-id="c3fa2-176">På hello **utlösare** konfigurerar hello build toobe Utlöses efter varje genomförande.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-176">On hello **Triggers** tab, configure hello build toobe triggered after each commit.</span></span> <span data-ttu-id="c3fa2-177">Välj **kontinuerlig integration** och **Batch ändringar**.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-177">Select **Continuous integration** and **Batch changes**.</span></span>

    ![Visual Studio Team Services - konfiguration av Build-utlösare](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-hello-build-workflow"></a><span data-ttu-id="c3fa2-179">Definiera hello build-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="c3fa2-179">Define hello build workflow</span></span>
<span data-ttu-id="c3fa2-180">Nästa steg i hello definiera hello build-arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-180">hello next steps define hello build workflow.</span></span> <span data-ttu-id="c3fa2-181">Det finns fem behållaren bilder toobuild för hello *MyShop* program.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-181">There are five container images toobuild for hello *MyShop* application.</span></span> <span data-ttu-id="c3fa2-182">Varje avbildning skapas med hello Dockerfile finns i hello projektet mappar:</span><span class="sxs-lookup"><span data-stu-id="c3fa2-182">Each image is built using hello Dockerfile located in hello project folders:</span></span>

* <span data-ttu-id="c3fa2-183">ProductsApi</span><span class="sxs-lookup"><span data-stu-id="c3fa2-183">ProductsApi</span></span>
* <span data-ttu-id="c3fa2-184">Proxy</span><span class="sxs-lookup"><span data-stu-id="c3fa2-184">Proxy</span></span>
* <span data-ttu-id="c3fa2-185">RatingsApi</span><span class="sxs-lookup"><span data-stu-id="c3fa2-185">RatingsApi</span></span>
* <span data-ttu-id="c3fa2-186">RecommandationsApi</span><span class="sxs-lookup"><span data-stu-id="c3fa2-186">RecommandationsApi</span></span>
* <span data-ttu-id="c3fa2-187">ShopFront</span><span class="sxs-lookup"><span data-stu-id="c3fa2-187">ShopFront</span></span>

<span data-ttu-id="c3fa2-188">Du behöver tooadd två Docker steg för varje avbildning, en toobuild hello bild och en toopush hello bild i registret för hello Azure-behållaren.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-188">You need tooadd two Docker steps for each image, one toobuild hello image, and one toopush hello image in hello Azure container registry.</span></span> 

1. <span data-ttu-id="c3fa2-189">tooadd ett steg i hello build-arbetsflöde, klickar du på **+ Lägg till build steg** och välj **Docker**.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-189">tooadd a step in hello build workflow, click **+ Add build step** and select **Docker**.</span></span>

    ![Visual Studio Team Services – Lägg till Build steg](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

2. <span data-ttu-id="c3fa2-191">För varje bild och konfigurera ett steg som använder hello `docker build` kommando.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-191">For each image, configure one step that uses hello `docker build` command.</span></span>

    ![Visual Studio Team Services - Docker skapa](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    <span data-ttu-id="c3fa2-193">Skapa åtgärden för hello, Välj Azure-behållaren registret, hello **skapa en avbildning** åtgärd och hello Dockerfile som definierar varje avbildning.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-193">For hello build operation, select your Azure container registry, hello **Build an image** action, and hello Dockerfile that defines each image.</span></span> <span data-ttu-id="c3fa2-194">Ange hello **Build-kontexten** hello Dockerfile rotkatalogen och definiera hello **avbildningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-194">Set hello **Build context** as hello Dockerfile root directory, and define hello **Image Name**.</span></span> 
    
    <span data-ttu-id="c3fa2-195">Börja hello avbildningsnamn med hello URI för Azure-behållaren registret på hello föregående skärm visas.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-195">As shown on hello preceding screen, start hello image name with hello URI of your Azure container registry.</span></span> <span data-ttu-id="c3fa2-196">(Du kan också använda en build variabeln tooparameterize hello taggen för hello-avbildning, till exempel hello build-ID i det här exemplet.)</span><span class="sxs-lookup"><span data-stu-id="c3fa2-196">(You can also use a build variable tooparameterize hello tag of hello image, such as hello build identifier in this example.)</span></span>

3. <span data-ttu-id="c3fa2-197">För varje bild och konfigurera ett andra steg som använder hello `docker push` kommando.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-197">For each image, configure a second step that uses hello `docker push` command.</span></span>

    ![Visual Studio Team Services - Docker Push](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    <span data-ttu-id="c3fa2-199">Hello push, välja för din Azure-behållaren i registret hello **Push en bild** åtgärd, och ange hello **avbildningsnamn** som är inbyggd i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-199">For hello push operation, select your Azure container registry, hello **Push an image** action, and enter hello **Image Name** that is built in hello previous step.</span></span>

4. <span data-ttu-id="c3fa2-200">När du har konfigurerat hello build och push steg för varje hello fem bilder, lägga till två fler steg i hello skapa arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-200">After you configure hello build and push steps for each of hello five images, add two more steps in hello build workflow.</span></span>

    <span data-ttu-id="c3fa2-201">a.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-201">a.</span></span> <span data-ttu-id="c3fa2-202">En kommandoradsaktivitet som använder ett bash-skript tooreplace hello *BuildNumber* förekomsten i hello docker-compose.yml fil med hello aktuella skapa Id. Se hello följande skärm för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-202">A command-line task that uses a bash script tooreplace hello *BuildNumber* occurrence in hello docker-compose.yml file with hello current build Id. See hello following screen for details.</span></span>

    ![Visual Studio Team Services - uppdatering Skriv fil](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    <span data-ttu-id="c3fa2-204">b.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-204">b.</span></span> <span data-ttu-id="c3fa2-205">En uppgift som släpper hello uppdateras skriva filen eftersom en build-artefakt så den kan användas i hello-versionen.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-205">A task that drops hello updated Compose file as a build artifact so it can be used in hello release.</span></span> <span data-ttu-id="c3fa2-206">Se hello följande skärm för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-206">See hello following screen for details.</span></span>

    ![Visual Studio Team Services - Publicera Skriv fil](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

5. <span data-ttu-id="c3fa2-208">Klicka på **spara** och namnet build-definition.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-208">Click **Save** and name your build definition.</span></span>

## <a name="step-3-create-hello-release-definition"></a><span data-ttu-id="c3fa2-209">Steg 3: Skapa hello versionen definition</span><span class="sxs-lookup"><span data-stu-id="c3fa2-209">Step 3: Create hello release definition</span></span>

<span data-ttu-id="c3fa2-210">Visual Studio Team Services kan du för[hantera versioner i miljöer](https://www.visualstudio.com/team-services/release-management/).</span><span class="sxs-lookup"><span data-stu-id="c3fa2-210">Visual Studio Team Services allows you too[manage releases across environments](https://www.visualstudio.com/team-services/release-management/).</span></span> <span data-ttu-id="c3fa2-211">Du kan aktivera kontinuerlig distribution toomake till att programmet har distribuerats på dina olika miljöer (t.ex dev, testa, Förproduktion och produktion) i enkelt.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-211">You can enable continuous deployment toomake sure that your application is deployed on your different environments (such as dev, test, pre-production, and production) in a smooth way.</span></span> <span data-ttu-id="c3fa2-212">Du kan skapa en ny miljö som representerar dina Azure Container Service Docker Swarm-kluster.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-212">You can create a new environment that represents your Azure Container Service Docker Swarm cluster.</span></span>

![Visual Studio Team Services - versionen tooACS](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a><span data-ttu-id="c3fa2-214">Första utgåvan installationen</span><span class="sxs-lookup"><span data-stu-id="c3fa2-214">Initial release setup</span></span>

1. <span data-ttu-id="c3fa2-215">toocreate en definition av versionen, klickar du på **versioner** > **+ släpper**</span><span class="sxs-lookup"><span data-stu-id="c3fa2-215">toocreate a release definition, click **Releases** > **+ Release**</span></span>

2. <span data-ttu-id="c3fa2-216">tooconfigure hello artefakt källa, klicka på **artefakter** > **länka en artefakt källa**.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-216">tooconfigure hello artifact source, Click **Artifacts** > **Link an artifact source**.</span></span> <span data-ttu-id="c3fa2-217">Här kan länka den här nya versionen definition toohello build-versionen som du definierade i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-217">Here, link this new release definition toohello build that you defined in hello previous step.</span></span> <span data-ttu-id="c3fa2-218">Hello docker-compose.yml filen är tillgänglig i hello versionen processen genom att göra.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-218">By doing this, hello docker-compose.yml file is available in hello release process.</span></span>

    ![Visual Studio Team Services - versionen artefakter](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

3. <span data-ttu-id="c3fa2-220">tooconfigure hello versionen utlösaren klickar du på **utlösare** och välj **kontinuerlig distribution**.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-220">tooconfigure hello release trigger, click **Triggers** and select **Continuous Deployment**.</span></span> <span data-ttu-id="c3fa2-221">Ange hello utlösare på hello samma artefakt källa.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-221">Set hello trigger on hello same artifact source.</span></span> <span data-ttu-id="c3fa2-222">Den här inställningen garanterar att en ny version startar så fort hello build har slutförts.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-222">This setting ensures that a new release starts as soon as hello build completes successfully.</span></span>

    ![Visual Studio Team Services - versionen utlösare](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-hello-release-workflow"></a><span data-ttu-id="c3fa2-224">Definiera hello versionen arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="c3fa2-224">Define hello release workflow</span></span>

<span data-ttu-id="c3fa2-225">hello versionen arbetsflöde består av två aktiviteter som du lägger till.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-225">hello release workflow is composed of two tasks that you add.</span></span>

1. <span data-ttu-id="c3fa2-226">Konfigurera en aktivitet toosecurely kopiera hello utgöra filen tooa *distribuera* mapp på hello Docker Swarm huvudnoden, använder du tidigare konfigurerat hello SSH-anslutning.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-226">Configure a task toosecurely copy hello compose file tooa *deploy* folder on hello Docker Swarm master node, using hello SSH connection you configured previously.</span></span> <span data-ttu-id="c3fa2-227">Se hello följande skärm för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-227">See hello following screen for details.</span></span>

    ![Visual Studio Team Services - versionen SCP](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

2. <span data-ttu-id="c3fa2-229">Konfigurera en andra uppgiften tooexecute ett bash kommandot toorun `docker` och `docker-compose` kommandon på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-229">Configure a second task tooexecute a bash command toorun `docker` and `docker-compose` commands on hello master node.</span></span> <span data-ttu-id="c3fa2-230">Se hello följande skärm för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-230">See hello following screen for details.</span></span>

    ![Visual Studio Team Services - versionen Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    <span data-ttu-id="c3fa2-232">hello-kommandot utförs på hello master Använd hello Docker CLI och hello Docker Compose CLI toodo hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="c3fa2-232">hello command executed on hello master use hello Docker CLI and hello Docker-Compose CLI toodo hello following tasks:</span></span>

    - <span data-ttu-id="c3fa2-233">Inloggningen toohello Azure-behållaren registret (den använder tre build variab'les som definieras i hello **variabler** fliken)</span><span class="sxs-lookup"><span data-stu-id="c3fa2-233">Login toohello Azure container registry (it uses three build variab\`les that are defined in hello **Variables** tab)</span></span>
    - <span data-ttu-id="c3fa2-234">Definiera hello **DOCKER_HOST** variabeln toowork med hello Swarm-slutpunkten (: 2375)</span><span class="sxs-lookup"><span data-stu-id="c3fa2-234">Define hello **DOCKER_HOST** variable toowork with hello Swarm endpoint (:2375)</span></span>
    - <span data-ttu-id="c3fa2-235">Navigera toohello *distribuera* mapp som har skapats av föregående aktivitet säker copy hello och som innehåller filen för hello docker-compose.yml</span><span class="sxs-lookup"><span data-stu-id="c3fa2-235">Navigate toohello *deploy* folder that was created by hello preceding secure copy task and that contains hello docker-compose.yml file</span></span> 
    - <span data-ttu-id="c3fa2-236">Köra `docker-compose` kommandon som hämtar hello nya bilder, stoppa hello tjänster, ta bort hello services och skapa hello behållare.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-236">Execute `docker-compose` commands that pull hello new images, stop hello services, remove hello services, and create hello containers.</span></span>

    >[!IMPORTANT]
    > <span data-ttu-id="c3fa2-237">Som det visas på hello föregående skärm, lämna hello **misslyckas på STDERR** kryssrutan avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-237">As shown on hello preceding screen, leave hello **Fail on STDERR** checkbox unchecked.</span></span> <span data-ttu-id="c3fa2-238">Detta är ett viktigt, eftersom `docker-compose` flera diagnostiska meddelanden skrivs ut som behållare är stoppas eller tas bort på hello standardfel utdata.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-238">This is an important setting, because `docker-compose` prints several diagnostic messages, such as containers are stopping or being deleted, on hello standard error output.</span></span> <span data-ttu-id="c3fa2-239">Om du markerar kryssrutan hello rapporterar Visual Studio Team Services att fel uppstod vid hello versionen, även om allt går bra.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-239">If you check hello checkbox, Visual Studio Team Services reports that errors occurred during hello release, even if all goes well.</span></span>
    >
3. <span data-ttu-id="c3fa2-240">Spara den här nya versionen definitionen.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-240">Save this new release definition.</span></span>


>[!NOTE]
><span data-ttu-id="c3fa2-241">Den här distributionen innehåller vissa driftstopp eftersom vi stoppa hello gamla tjänster och kör hello ny.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-241">This deployment includes some downtime because we are stopping hello old services and running hello new one.</span></span> <span data-ttu-id="c3fa2-242">Det är möjligt tooavoid detta genom att göra en blå-grön-distribution.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-242">It is possible tooavoid this by doing a blue-green deployment.</span></span>
>

## <a name="step-4-test-hello-cicd-pipeline"></a><span data-ttu-id="c3fa2-243">Steg 4.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-243">Step 4.</span></span> <span data-ttu-id="c3fa2-244">Testa hello CI/CD-pipeline</span><span class="sxs-lookup"><span data-stu-id="c3fa2-244">Test hello CI/CD pipeline</span></span>

<span data-ttu-id="c3fa2-245">Nu när du är klar med hello konfiguration, det är tid tootest detta nya CI/CD-pipeline.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-245">Now that you are done with hello configuration, it's time tootest this new CI/CD pipeline.</span></span> <span data-ttu-id="c3fa2-246">Hej enklaste sättet tootest är det tooupdate hello källa kod och bekräfta hello ändras till GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-246">hello easiest way tootest it is tooupdate hello source code and commit hello changes into your GitHub repository.</span></span> <span data-ttu-id="c3fa2-247">Några sekunder efter att du överför hello kod, ser du en ny version körs i Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-247">A few seconds after you push hello code, you will see a new build running in Visual Studio Team Services.</span></span> <span data-ttu-id="c3fa2-248">När har slutförts startas en ny version och distribuerar hello ny version av programmet hello på hello Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="c3fa2-248">Once completed successfully, a new release will be triggered and will deploy hello new version of hello application on hello Azure Container Service cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c3fa2-249">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3fa2-249">Next Steps</span></span>

* <span data-ttu-id="c3fa2-250">Mer information om CI/CD-skiva med Visual Studio Team Services finns hello [VSTS skapa översikt](https://www.visualstudio.com/docs/build/overview).</span><span class="sxs-lookup"><span data-stu-id="c3fa2-250">For more information about CI/CD with Visual Studio Team Services, see hello [VSTS Build overview](https://www.visualstudio.com/docs/build/overview).</span></span>
