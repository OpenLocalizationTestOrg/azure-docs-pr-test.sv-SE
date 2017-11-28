---
title: "aaaCI/CD med Azure Container Service-motorn och Swarm läge | Microsoft Docs"
description: "Använda Azure Container Service-motorn med Docker Swarm-läge, ett Azure Container registret och Visual Studio Team Services toodeliver kontinuerligt ett flera behållare .NET Core-program"
services: container-service
documentationcenter: " "
author: diegomrtnzg
manager: esterdnb
tags: acs, azure-container-service, acs-engine
keywords: "Docker behållare Micro-tjänster, Swarm Azure, Visual Studio Team Services, DevOps, ACS-motorn"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2017
ms.author: diegomrtnzg
ms.custom: mvc
ms.openlocfilehash: 040522c452f7ea0ce3c92f2fe57b1c141b97e380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="full-cicd-pipeline-toodeploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-visual-studio-team-services"></a><span data-ttu-id="23a30-104">Fullständig CI/CD-pipeline toodeploy ett program för flera behållare på Azure Container Service med ACS-motorn och Docker Swarm-läge med hjälp av Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="23a30-104">Full CI/CD pipeline toodeploy a multi-container application on Azure Container Service with ACS Engine and Docker Swarm Mode using Visual Studio Team Services</span></span>

<span data-ttu-id="23a30-105">*Den här artikeln är baserad på [fullständig CI/CD-pipeline toodeploy ett program för flera behållare på Azure Container Service med Docker Swarm med hjälp av Visual Studio Team Services](container-service-docker-swarm-setup-ci-cd.md) dokumentation*</span><span class="sxs-lookup"><span data-stu-id="23a30-105">*This article is based on [Full CI/CD pipeline toodeploy a multi-container application on Azure Container Service with Docker Swarm using Visual Studio Team Services](container-service-docker-swarm-setup-ci-cd.md) documentation*</span></span>

<span data-ttu-id="23a30-106">Men i dag en av hello största utmaningarna när utveckla moderna applikationer för hello molnet kan toodeliver dessa program kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="23a30-106">Nowadays, One of hello biggest challenges when developing modern applications for hello cloud is being able toodeliver these applications continuously.</span></span> <span data-ttu-id="23a30-107">I den här artikeln får du lära dig hur pipeline med tooimplement en fullständig kontinuerlig integrering och distribution (CI/CD):</span><span class="sxs-lookup"><span data-stu-id="23a30-107">In this article, you learn how tooimplement a full continuous integration and deployment (CI/CD) pipeline using:</span></span> 
* <span data-ttu-id="23a30-108">Azure Container Service-motorn med Docker Swarm-läge</span><span class="sxs-lookup"><span data-stu-id="23a30-108">Azure Container Service Engine with Docker Swarm Mode</span></span>
* <span data-ttu-id="23a30-109">Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="23a30-109">Azure Container Registry</span></span>
* <span data-ttu-id="23a30-110">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="23a30-110">Visual Studio Team Services</span></span>

<span data-ttu-id="23a30-111">Den här artikeln är baserad på ett enkelt program tillgängliga på [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), utvecklade med ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="23a30-111">This article is based on a simple application, available on [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), developed with ASP.NET Core.</span></span> <span data-ttu-id="23a30-112">hello program består av fyra olika tjänster: tre webb-API: er och en frontwebb:</span><span class="sxs-lookup"><span data-stu-id="23a30-112">hello application is composed of four different services: three web APIs and one web front end:</span></span>

![MyShop exempelprogrammet](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

<span data-ttu-id="23a30-114">hello mål är toodeliver programmet kontinuerligt i ett läge för Docker Swarm-kluster med hjälp av Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="23a30-114">hello objective is toodeliver this application continuously in a Docker Swarm Mode cluster, using Visual Studio Team Services.</span></span> <span data-ttu-id="23a30-115">hello följande figur information denna kontinuerlig leverans pipeline:</span><span class="sxs-lookup"><span data-stu-id="23a30-115">hello following figure details this continuous delivery pipeline:</span></span>

![MyShop exempelprogrammet](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

<span data-ttu-id="23a30-117">Här följer en kort beskrivning av hello steg:</span><span class="sxs-lookup"><span data-stu-id="23a30-117">Here is a brief explanation of hello steps:</span></span>

1. <span data-ttu-id="23a30-118">Koden ändringarna har bekräftats toohello källdatabasen kod (här GitHub)</span><span class="sxs-lookup"><span data-stu-id="23a30-118">Code changes are committed toohello source code repository (here, GitHub)</span></span> 
2. <span data-ttu-id="23a30-119">GitHub utlöser en version i Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="23a30-119">GitHub triggers a build in Visual Studio Team Services</span></span> 
3. <span data-ttu-id="23a30-120">Visual Studio Team Services hämtar hello senaste versionen av hello källor och skapar alla hello-avbildningar som ska utgöra hello program</span><span class="sxs-lookup"><span data-stu-id="23a30-120">Visual Studio Team Services gets hello latest version of hello sources and builds all hello images that compose hello application</span></span> 
4. <span data-ttu-id="23a30-121">Visual Studio Team Services skickar varje avbildning tooa Docker-registret som skapats med hjälp av hello Azure Container Registry-tjänsten</span><span class="sxs-lookup"><span data-stu-id="23a30-121">Visual Studio Team Services pushes each image tooa Docker registry created using hello Azure Container Registry service</span></span> 
5. <span data-ttu-id="23a30-122">Visual Studio Team Services utlöser en ny version</span><span class="sxs-lookup"><span data-stu-id="23a30-122">Visual Studio Team Services triggers a new release</span></span> 
6. <span data-ttu-id="23a30-123">hello körs vissa kommandon med hjälp av SSH på hello Azure container service master klusternod</span><span class="sxs-lookup"><span data-stu-id="23a30-123">hello release runs some commands using SSH on hello Azure container service cluster master node</span></span> 
7. <span data-ttu-id="23a30-124">Docker Swarm-läge på kluster hello hämtar hello senaste versionen av hello bilder</span><span class="sxs-lookup"><span data-stu-id="23a30-124">Docker Swarm Mode on hello cluster pulls hello latest version of hello images</span></span> 
8. <span data-ttu-id="23a30-125">hello ny version av programmet hello distribueras med Docker-stacken</span><span class="sxs-lookup"><span data-stu-id="23a30-125">hello new version of hello application is deployed using Docker Stack</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="23a30-126">Krav</span><span class="sxs-lookup"><span data-stu-id="23a30-126">Prerequisites</span></span>

<span data-ttu-id="23a30-127">Innan du påbörjar de här självstudierna måste toocomplete hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="23a30-127">Before starting this tutorial, you need toocomplete hello following tasks:</span></span>

- [<span data-ttu-id="23a30-128">Skapa ett läge för Swarm-kluster i Azure Container Service med ACS-motorn</span><span class="sxs-lookup"><span data-stu-id="23a30-128">Create a Swarm Mode cluster in Azure Container Service with ACS Engine</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [<span data-ttu-id="23a30-129">Ansluta med hello Swarm-kluster i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="23a30-129">Connect with hello Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)
- [<span data-ttu-id="23a30-130">Skapa en Azure-behållaren registernyckel</span><span class="sxs-lookup"><span data-stu-id="23a30-130">Create an Azure container registry</span></span>](../../container-registry/container-registry-get-started-portal.md)
- [<span data-ttu-id="23a30-131">Har en Visual Studio Team Services-konto och team projekt</span><span class="sxs-lookup"><span data-stu-id="23a30-131">Have a Visual Studio Team Services account and team project created</span></span>](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [<span data-ttu-id="23a30-132">Duplicera hello GitHub-lagringsplatsen tooyour GitHub-konto</span><span class="sxs-lookup"><span data-stu-id="23a30-132">Fork hello GitHub repository tooyour GitHub account</span></span>](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> <span data-ttu-id="23a30-133">Hej Docker Swarm orchestrator i Azure Container Service använder äldre fristående Swarm.</span><span class="sxs-lookup"><span data-stu-id="23a30-133">hello Docker Swarm orchestrator in Azure Container Service uses legacy standalone Swarm.</span></span> <span data-ttu-id="23a30-134">För närvarande hello integrerad [Swarm läge](https://docs.docker.com/engine/swarm/) (i Docker 1.12 och högre) är inte en stöds orchestrator i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="23a30-134">Currently, hello integrated [Swarm mode](https://docs.docker.com/engine/swarm/) (in Docker 1.12 and higher) is not a supported orchestrator in Azure Container Service.</span></span> <span data-ttu-id="23a30-135">Därför kan vi använda [ACS-motorn](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), en community-bidragit [quickstart mallen](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), eller en Docker-lösning i hello [Azure Marketplace](https://azuremarketplace.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="23a30-135">For this reason, we are using [ACS Engine](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), a community-contributed [quickstart template](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), or a Docker solution in hello [Azure Marketplace](https://azuremarketplace.microsoft.com).</span></span>
>

## <a name="step-1-configure-your-visual-studio-team-services-account"></a><span data-ttu-id="23a30-136">Steg 1: Konfigurera Visual Studio Team Services-konto</span><span class="sxs-lookup"><span data-stu-id="23a30-136">Step 1: Configure your Visual Studio Team Services account</span></span> 

<span data-ttu-id="23a30-137">I det här avsnittet kan du konfigurera Visual Studio Team Services-konto.</span><span class="sxs-lookup"><span data-stu-id="23a30-137">In this section, you configure your Visual Studio Team Services account.</span></span> <span data-ttu-id="23a30-138">tooconfigure VSTS Services-slutpunkter, i ditt projekt i Visual Studio Team Services, klicka på hello **inställningar** ikon i hello verktygsfältet och välj **Services**.</span><span class="sxs-lookup"><span data-stu-id="23a30-138">tooconfigure VSTS Services Endpoints, in your Visual Studio Team Services project, click hello **Settings** icon in hello toolbar, and select **Services**.</span></span>

![Öppna Services slutpunkt](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-visual-studio-team-services-and-azure-account"></a><span data-ttu-id="23a30-140">Ansluta till Visual Studio Team Services och Azure-konto</span><span class="sxs-lookup"><span data-stu-id="23a30-140">Connect Visual Studio Team Services and Azure account</span></span>

<span data-ttu-id="23a30-141">Upprätta en anslutning mellan projektet VSTS och ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="23a30-141">Set up a connection between your VSTS project and your Azure account.</span></span>

1. <span data-ttu-id="23a30-142">Klicka på vänster hello **nya tjänstslutpunkten** > **Azure Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="23a30-142">On hello left, click **New Service Endpoint** > **Azure Resource Manager**.</span></span>
2. <span data-ttu-id="23a30-143">tooauthorize VSTS toowork med ditt Azure-konto, Välj din **prenumeration** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="23a30-143">tooauthorize VSTS toowork with your Azure account, select your **Subscription** and click **OK**.</span></span>

    ![Visual Studio Team Services - auktorisera Azure](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-visual-studio-team-services-and-github"></a><span data-ttu-id="23a30-145">Ansluta Visual Studio Team Services och GitHub</span><span class="sxs-lookup"><span data-stu-id="23a30-145">Connect Visual Studio Team Services and GitHub</span></span>

<span data-ttu-id="23a30-146">Upprätta en anslutning mellan projektet VSTS och GitHub-konto.</span><span class="sxs-lookup"><span data-stu-id="23a30-146">Set up a connection between your VSTS project and your GitHub account.</span></span>

1. <span data-ttu-id="23a30-147">Klicka på vänster hello **nya tjänstslutpunkten** > **GitHub**.</span><span class="sxs-lookup"><span data-stu-id="23a30-147">On hello left, click **New Service Endpoint** > **GitHub**.</span></span>
2. <span data-ttu-id="23a30-148">tooauthorize VSTS toowork med ditt GitHub-konto, klickar du på **auktorisera** och följa hello i hello-fönster som öppnas.</span><span class="sxs-lookup"><span data-stu-id="23a30-148">tooauthorize VSTS toowork with your GitHub account, click **Authorize** and follow hello procedure in hello window that opens.</span></span>

    ![Visual Studio Team Services - auktorisera GitHub](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-vsts-tooyour-azure-container-service-cluster"></a><span data-ttu-id="23a30-150">Ansluta VSTS tooyour Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="23a30-150">Connect VSTS tooyour Azure Container Service cluster</span></span>

<span data-ttu-id="23a30-151">hello sista stegen innan du hämtar till hello CI/CD-pipeline är tooconfigure externa anslutningar tooyour Docker Swarm-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="23a30-151">hello last steps before getting into hello CI/CD pipeline are tooconfigure external connections tooyour Docker Swarm cluster in Azure.</span></span> 

1. <span data-ttu-id="23a30-152">Lägga till en slutpunkt av typen för Docker Swarm-kluster hello **SSH**.</span><span class="sxs-lookup"><span data-stu-id="23a30-152">For hello Docker Swarm cluster, add an endpoint of type **SSH**.</span></span> <span data-ttu-id="23a30-153">Ange hello SSH-anslutningsinformationen för Swarm-klustret (nod).</span><span class="sxs-lookup"><span data-stu-id="23a30-153">Then enter hello SSH connection information of your Swarm cluster (master node).</span></span>

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

<span data-ttu-id="23a30-155">Alla hello-konfigurationen är nu klar.</span><span class="sxs-lookup"><span data-stu-id="23a30-155">All hello configuration is done now.</span></span> <span data-ttu-id="23a30-156">Skapa hello CI/CD-pipeline som skapar och distribuerar hello programmet toohello Docker Swarm-kluster i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="23a30-156">In hello next steps, you create hello CI/CD pipeline that builds and deploys hello application toohello Docker Swarm cluster.</span></span> 

## <a name="step-2-create-hello-build-definition"></a><span data-ttu-id="23a30-157">Steg 2: Skapa hello build-definition</span><span class="sxs-lookup"><span data-stu-id="23a30-157">Step 2: Create hello build definition</span></span>

<span data-ttu-id="23a30-158">I det här steget du ställer in en build-definition för projektet VSTS och definiera hello build arbetsflöde för bilderna behållare</span><span class="sxs-lookup"><span data-stu-id="23a30-158">In this step, you set up a build definition for your VSTS project and define hello build workflow for your container images</span></span>

### <a name="initial-definition-setup"></a><span data-ttu-id="23a30-159">Inledande definition installationen</span><span class="sxs-lookup"><span data-stu-id="23a30-159">Initial definition setup</span></span>

1. <span data-ttu-id="23a30-160">toocreate en build-definition ansluta tooyour Visual Studio Team Services projektet och klicka på **Skapa & släpper**.</span><span class="sxs-lookup"><span data-stu-id="23a30-160">toocreate a build definition, connect tooyour Visual Studio Team Services project and click **Build & Release**.</span></span> <span data-ttu-id="23a30-161">I hello **skapa definitioner** klickar du på **+ ny**.</span><span class="sxs-lookup"><span data-stu-id="23a30-161">In hello **Build definitions** section, click **+ New**.</span></span> 

    ![Visual Studio Team Services - nya skapa Definition](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. <span data-ttu-id="23a30-163">Välj hello **tom process**.</span><span class="sxs-lookup"><span data-stu-id="23a30-163">Select hello **Empty process**.</span></span>

    ![Visual Studio Team Services - ny tom skapa Definition](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. <span data-ttu-id="23a30-165">Klicka på hello **variabler** fliken och skapar två nya variabler: **RegistryURL** och **AgentURL**.</span><span class="sxs-lookup"><span data-stu-id="23a30-165">Then, click hello **Variables** tab and create two new variables: **RegistryURL** and **AgentURL**.</span></span> <span data-ttu-id="23a30-166">Klistra in hello värden för registernyckeln och klustret agenter DNS.</span><span class="sxs-lookup"><span data-stu-id="23a30-166">Paste hello values of your Registry and Cluster Agents DNS.</span></span>

    ![Visual Studio Team Services - variabler Versionskonfiguration](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. <span data-ttu-id="23a30-168">På hello **skapa definitioner** sida, öppna hello **utlösare** och konfigurera hello build toouse kontinuerlig integration med hello förgrening av hello MyShop projekt som du skapade i hello krav.</span><span class="sxs-lookup"><span data-stu-id="23a30-168">On hello **Build Definitions** page, open hello **Triggers** tab and configure hello build toouse continuous integration with hello fork of hello MyShop project that you created in hello prerequisites.</span></span> <span data-ttu-id="23a30-169">Markera **Batch ändringar**.</span><span class="sxs-lookup"><span data-stu-id="23a30-169">Then, select **Batch changes**.</span></span> <span data-ttu-id="23a30-170">Kontrollera att du väljer *docker-linux* som hello **Förgrena specifikationen**.</span><span class="sxs-lookup"><span data-stu-id="23a30-170">Make sure that you select *docker-linux* as hello **Branch specification**.</span></span>

    ![Visual Studio Team Services - databasen Versionskonfiguration](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. <span data-ttu-id="23a30-172">Klicka slutligen på hello **alternativ** och konfigurera hello standard agent kön för**finns Linux Preview**.</span><span class="sxs-lookup"><span data-stu-id="23a30-172">Finally, click hello **Options** tab and configure hello Default agent queue too**Hosted Linux Preview**.</span></span>

    ![Visual Studio Team Services - agenten Värdkonfiguration](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-hello-build-workflow"></a><span data-ttu-id="23a30-174">Definiera hello build-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="23a30-174">Define hello build workflow</span></span>
<span data-ttu-id="23a30-175">Nästa steg i hello definiera hello build-arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="23a30-175">hello next steps define hello build workflow.</span></span> <span data-ttu-id="23a30-176">Du måste först tooconfigure hello källan för hello koden.</span><span class="sxs-lookup"><span data-stu-id="23a30-176">First, you need tooconfigure hello source of hello code.</span></span> <span data-ttu-id="23a30-177">toodo det, väljer **GitHub** och **databasen** och **gren** (docker-linux).</span><span class="sxs-lookup"><span data-stu-id="23a30-177">toodo it, select **GitHub** and your **repository** and **branch** (docker-linux).</span></span>

![Konfigurera Visual Studio Team Services - Kodkälla](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

<span data-ttu-id="23a30-179">Det finns fem behållaren bilder toobuild för hello *MyShop* program.</span><span class="sxs-lookup"><span data-stu-id="23a30-179">There are five container images toobuild for hello *MyShop* application.</span></span> <span data-ttu-id="23a30-180">Varje avbildning skapas med hello Dockerfile finns i hello projektet mappar:</span><span class="sxs-lookup"><span data-stu-id="23a30-180">Each image is built using hello Dockerfile located in hello project folders:</span></span>

* <span data-ttu-id="23a30-181">ProductsApi</span><span class="sxs-lookup"><span data-stu-id="23a30-181">ProductsApi</span></span>
* <span data-ttu-id="23a30-182">Proxy</span><span class="sxs-lookup"><span data-stu-id="23a30-182">Proxy</span></span>
* <span data-ttu-id="23a30-183">RatingsApi</span><span class="sxs-lookup"><span data-stu-id="23a30-183">RatingsApi</span></span>
* <span data-ttu-id="23a30-184">RecommandationsApi</span><span class="sxs-lookup"><span data-stu-id="23a30-184">RecommandationsApi</span></span>
* <span data-ttu-id="23a30-185">ShopFront</span><span class="sxs-lookup"><span data-stu-id="23a30-185">ShopFront</span></span>

<span data-ttu-id="23a30-186">Du behöver två Docker steg för varje avbildning, en toobuild hello bild och en toopush hello bild i registret för hello Azure-behållaren.</span><span class="sxs-lookup"><span data-stu-id="23a30-186">You need two Docker steps for each image, one toobuild hello image, and one toopush hello image in hello Azure container registry.</span></span> 

1. <span data-ttu-id="23a30-187">tooadd ett steg i hello build-arbetsflöde, klickar du på **+ Lägg till build steg** och välj **Docker**.</span><span class="sxs-lookup"><span data-stu-id="23a30-187">tooadd a step in hello build workflow, click **+ Add build step** and select **Docker**.</span></span>

    ![Visual Studio Team Services – Lägg till Build steg](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. <span data-ttu-id="23a30-189">För varje bild och konfigurera ett steg som använder hello `docker build` kommando.</span><span class="sxs-lookup"><span data-stu-id="23a30-189">For each image, configure one step that uses hello `docker build` command.</span></span>

    ![Visual Studio Team Services - Docker skapa](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    <span data-ttu-id="23a30-191">Hello skapa i åtgärden, Välj Azure-behållare registret, hello **skapa en avbildning** åtgärd och hello Dockerfile som definierar varje avbildning.</span><span class="sxs-lookup"><span data-stu-id="23a30-191">For hello build operation, select your Azure Container Registry, hello **Build an image** action, and hello Dockerfile that defines each image.</span></span> <span data-ttu-id="23a30-192">Ange hello **Working Directory** som hello Dockerfile rotkatalog, definiera hello **avbildningsnamn**, och välj **inkludera senaste taggen**.</span><span class="sxs-lookup"><span data-stu-id="23a30-192">Set hello **Working Directory** as hello Dockerfile root directory, define hello **Image Name**, and select **Include Latest Tag**.</span></span>
    
    <span data-ttu-id="23a30-193">hello Avbildningsnamnet har toobe i det här formatet: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```.</span><span class="sxs-lookup"><span data-stu-id="23a30-193">hello Image Name has toobe in this format: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```.</span></span> <span data-ttu-id="23a30-194">Ersätt **[NAME]** med hello avbildningens namn:</span><span class="sxs-lookup"><span data-stu-id="23a30-194">Replace **[NAME]** with hello image name:</span></span>
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. <span data-ttu-id="23a30-195">För varje bild och konfigurera ett andra steg som använder hello `docker push` kommando.</span><span class="sxs-lookup"><span data-stu-id="23a30-195">For each image, configure a second step that uses hello `docker push` command.</span></span>

    ![Visual Studio Team Services - Docker Push](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    <span data-ttu-id="23a30-197">Hello push, välja för din Azure-behållaren i registret hello **Push en bild** åtgärd, ange hello **avbildningsnamn** som är inbyggd i hello föregående steg och välj **inkludera senaste tagg** .</span><span class="sxs-lookup"><span data-stu-id="23a30-197">For hello push operation, select your Azure container registry, hello **Push an image** action, enter hello **Image Name** that is built in hello previous step and select **Include Latest Tag**.</span></span>

4. <span data-ttu-id="23a30-198">När du har konfigurerat hello build och push steg för varje hello fem bilder, lägga till tre fler steg i hello skapa arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="23a30-198">After you configure hello build and push steps for each of hello five images, add three more steps in hello build workflow.</span></span>

   ![Visual Studio Team Services – Lägg till Kommandoradsaktivitet](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

      1. <span data-ttu-id="23a30-200">En kommandoradsaktivitet som använder ett bash-skript tooreplace hello *RegistryURL* förekomsten i filen för hello docker-compose.yml med hello RegistryURL variabel.</span><span class="sxs-lookup"><span data-stu-id="23a30-200">A command-line task that uses a bash script tooreplace hello *RegistryURL* occurrence in hello docker-compose.yml file with hello RegistryURL variable.</span></span> 
    
          ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

          ![Visual Studio Team Services - uppdatering skriva filen med registret URL](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

      2. <span data-ttu-id="23a30-202">En kommandoradsaktivitet som använder ett bash-skript tooreplace hello *AgentURL* förekomsten i filen för hello docker-compose.yml med hello AgentURL variabel.</span><span class="sxs-lookup"><span data-stu-id="23a30-202">A command-line task that uses a bash script tooreplace hello *AgentURL* occurrence in hello docker-compose.yml file with hello AgentURL variable.</span></span>
  
          ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

     3. <span data-ttu-id="23a30-203">En uppgift som släpper hello uppdateras skriva filen eftersom en build-artefakt så den kan användas i hello-versionen.</span><span class="sxs-lookup"><span data-stu-id="23a30-203">A task that drops hello updated Compose file as a build artifact so it can be used in hello release.</span></span> <span data-ttu-id="23a30-204">Se hello följande skärm för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="23a30-204">See hello following screen for details.</span></span>

         ![Visual Studio Team Services - Publicera artefakt](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

         ![Visual Studio Team Services - Publicera Skriv fil](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. <span data-ttu-id="23a30-207">Klicka på **Spara & kö** tootest build-definition.</span><span class="sxs-lookup"><span data-stu-id="23a30-207">Click **Save & queue** tootest your build definition.</span></span>

   ![Visual Studio Team-Services - spara och kön](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![Visual Studio Team Services - ny kö](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. <span data-ttu-id="23a30-210">Om hello **skapa** är korrekt, du har toosee den här skärmen:</span><span class="sxs-lookup"><span data-stu-id="23a30-210">If hello **Build** is correct, you have toosee this screen:</span></span>

  ![Visual Studio Team Services - genereringen lyckades](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-hello-release-definition"></a><span data-ttu-id="23a30-212">Steg 3: Skapa hello versionen definition</span><span class="sxs-lookup"><span data-stu-id="23a30-212">Step 3: Create hello release definition</span></span>

<span data-ttu-id="23a30-213">Visual Studio Team Services kan du för[hantera versioner i miljöer](https://www.visualstudio.com/team-services/release-management/).</span><span class="sxs-lookup"><span data-stu-id="23a30-213">Visual Studio Team Services allows you too[manage releases across environments](https://www.visualstudio.com/team-services/release-management/).</span></span> <span data-ttu-id="23a30-214">Du kan aktivera kontinuerlig distribution toomake till att programmet har distribuerats på dina olika miljöer (t.ex dev, testa, Förproduktion och produktion) i enkelt.</span><span class="sxs-lookup"><span data-stu-id="23a30-214">You can enable continuous deployment toomake sure that your application is deployed on your different environments (such as dev, test, pre-production, and production) in a smooth way.</span></span> <span data-ttu-id="23a30-215">Du kan skapa en miljö som representerar läge i Azure Container Service Docker Swarm-klustret.</span><span class="sxs-lookup"><span data-stu-id="23a30-215">You can create an environment that represents your Azure Container Service Docker Swarm Mode cluster.</span></span>

![Visual Studio Team Services - versionen tooACS](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a><span data-ttu-id="23a30-217">Första utgåvan installationen</span><span class="sxs-lookup"><span data-stu-id="23a30-217">Initial release setup</span></span>

1. <span data-ttu-id="23a30-218">toocreate en definition av versionen, klickar du på **versioner** > **+ släpper**</span><span class="sxs-lookup"><span data-stu-id="23a30-218">toocreate a release definition, click **Releases** > **+ Release**</span></span>

2. <span data-ttu-id="23a30-219">tooconfigure hello artefakt källa, klicka på **artefakter** > **länka en artefakt källa**.</span><span class="sxs-lookup"><span data-stu-id="23a30-219">tooconfigure hello artifact source, Click **Artifacts** > **Link an artifact source**.</span></span> <span data-ttu-id="23a30-220">Här kan länka den här nya versionen definition toohello build-versionen som du definierade i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="23a30-220">Here, link this new release definition toohello build that you defined in hello previous step.</span></span> <span data-ttu-id="23a30-221">Efter det är hello docker-compose.yml filen tillgänglig i hello release-processen.</span><span class="sxs-lookup"><span data-stu-id="23a30-221">After that, hello docker-compose.yml file is available in hello release process.</span></span>

    ![Visual Studio Team Services - versionen artefakter](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. <span data-ttu-id="23a30-223">tooconfigure hello versionen utlösaren klickar du på **utlösare** och välj **kontinuerlig distribution**.</span><span class="sxs-lookup"><span data-stu-id="23a30-223">tooconfigure hello release trigger, click **Triggers** and select **Continuous Deployment**.</span></span> <span data-ttu-id="23a30-224">Ange hello utlösare på hello samma artefakt källa.</span><span class="sxs-lookup"><span data-stu-id="23a30-224">Set hello trigger on hello same artifact source.</span></span> <span data-ttu-id="23a30-225">Den här inställningen garanterar att en ny version startar när hello build har slutförts.</span><span class="sxs-lookup"><span data-stu-id="23a30-225">This setting ensures that a new release starts when hello build completes successfully.</span></span>

    ![Visual Studio Team Services - versionen utlösare](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. <span data-ttu-id="23a30-227">tooconfigure hello versionen variabler, klickar du på **variabler** och välj **+ variabeln** toocreate tre nya variabler med hello information av hello registerdata: **docker.username**, **docker.password**, och **docker.registry**.</span><span class="sxs-lookup"><span data-stu-id="23a30-227">tooconfigure hello release variables, click **Variables** and select **+Variable** toocreate three new variables with hello info of hello registry: **docker.username**, **docker.password**, and **docker.registry**.</span></span> <span data-ttu-id="23a30-228">Klistra in hello värden för registernyckeln och klustret agenter DNS.</span><span class="sxs-lookup"><span data-stu-id="23a30-228">Paste hello values of your Registry and Cluster Agents DNS.</span></span>

    ![Visual Studio Team Services - databasen Versionskonfiguration](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > <span data-ttu-id="23a30-230">Som visas i föregående hello-skärmen, klickar du på hello **Lås** kryssrutan i docker.password.</span><span class="sxs-lookup"><span data-stu-id="23a30-230">As shown on hello previous screen, click hello **Lock** checkbox in docker.password.</span></span> <span data-ttu-id="23a30-231">Den här inställningen är viktiga toorestrict hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="23a30-231">This setting is important toorestrict hello password.</span></span>
    >

### <a name="define-hello-release-workflow"></a><span data-ttu-id="23a30-232">Definiera hello versionen arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="23a30-232">Define hello release workflow</span></span>

<span data-ttu-id="23a30-233">hello versionen arbetsflöde består av två aktiviteter som du lägger till.</span><span class="sxs-lookup"><span data-stu-id="23a30-233">hello release workflow is composed of two tasks that you add.</span></span>

1. <span data-ttu-id="23a30-234">Konfigurera en aktivitet toosecurely kopiera hello utgöra filen tooa *distribuera* mapp på hello Docker Swarm huvudnoden, använder du tidigare konfigurerat hello SSH-anslutning.</span><span class="sxs-lookup"><span data-stu-id="23a30-234">Configure a task toosecurely copy hello compose file tooa *deploy* folder on hello Docker Swarm master node, using hello SSH connection you configured previously.</span></span> <span data-ttu-id="23a30-235">Se hello följande skärm för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="23a30-235">See hello following screen for details.</span></span>
    
    <span data-ttu-id="23a30-236">Källmapp:```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```</span><span class="sxs-lookup"><span data-stu-id="23a30-236">Source folder: ```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```</span></span>

    ![Visual Studio Team Services - versionen SCP](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. <span data-ttu-id="23a30-238">Konfigurera en andra uppgiften tooexecute ett bash kommandot toorun `docker` och `docker stack deploy` kommandon på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="23a30-238">Configure a second task tooexecute a bash command toorun `docker` and `docker stack deploy` commands on hello master node.</span></span> <span data-ttu-id="23a30-239">Se hello följande skärm för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="23a30-239">See hello following screen for details.</span></span>

    ```docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth```

    ![Visual Studio Team Services - versionen Bash](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    <span data-ttu-id="23a30-241">hello-kommandot som kördes på hello master använder hello Docker CLI och hello Docker Compose CLI toodo hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="23a30-241">hello command executed on hello master uses hello Docker CLI and hello Docker-Compose CLI toodo hello following tasks:</span></span>

    - <span data-ttu-id="23a30-242">Logga in toohello Azure-behållaren registret (den använder tre build-variabler som definieras i hello **variabler** fliken)</span><span class="sxs-lookup"><span data-stu-id="23a30-242">Log in toohello Azure container registry (it uses three build variables that are defined in hello **Variables** tab)</span></span>
    - <span data-ttu-id="23a30-243">Definiera hello **DOCKER_HOST** variabeln toowork med hello Swarm-slutpunkten (: 2375)</span><span class="sxs-lookup"><span data-stu-id="23a30-243">Define hello **DOCKER_HOST** variable toowork with hello Swarm endpoint (:2375)</span></span>
    - <span data-ttu-id="23a30-244">Navigera toohello *distribuera* mapp som har skapats av föregående aktivitet säker copy hello och som innehåller filen för hello docker-compose.yml</span><span class="sxs-lookup"><span data-stu-id="23a30-244">Navigate toohello *deploy* folder that was created by hello preceding secure copy task and that contains hello docker-compose.yml file</span></span> 
    - <span data-ttu-id="23a30-245">Köra `docker stack deploy` kommandon som hämtar hello nya bilder och skapa hello behållare.</span><span class="sxs-lookup"><span data-stu-id="23a30-245">Execute `docker stack deploy` commands that pull hello new images and create hello containers.</span></span>

    >[!IMPORTANT]
    > <span data-ttu-id="23a30-246">Som det visas på föregående hello-skärmen, lämna hello **misslyckas på STDERR** kryssrutan avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="23a30-246">As shown on hello previous screen, leave hello **Fail on STDERR** checkbox unchecked.</span></span> <span data-ttu-id="23a30-247">Den här inställningen gör att vi toocomplete hello release-processen på grund av för`docker-compose` flera diagnostiska meddelanden skrivs ut som behållare är stoppas eller tas bort på hello standardfel utdata.</span><span class="sxs-lookup"><span data-stu-id="23a30-247">This setting allows us toocomplete hello release process due too`docker-compose` prints several diagnostic messages, such as containers are stopping or being deleted, on hello standard error output.</span></span> <span data-ttu-id="23a30-248">Om du markerar kryssrutan hello rapporterar Visual Studio Team Services att fel uppstod vid hello versionen, även om allt går bra.</span><span class="sxs-lookup"><span data-stu-id="23a30-248">If you check hello checkbox, Visual Studio Team Services reports that errors occurred during hello release, even if all goes well.</span></span>
    >
3. <span data-ttu-id="23a30-249">Spara den här nya versionen definitionen.</span><span class="sxs-lookup"><span data-stu-id="23a30-249">Save this new release definition.</span></span>

## <a name="step-4-test-hello-cicd-pipeline"></a><span data-ttu-id="23a30-250">Steg 4: Testa hello CI/CD-pipeline</span><span class="sxs-lookup"><span data-stu-id="23a30-250">Step 4: Test hello CI/CD pipeline</span></span>

<span data-ttu-id="23a30-251">Nu när du är klar med hello konfiguration, det är tid tootest detta nya CI/CD-pipeline.</span><span class="sxs-lookup"><span data-stu-id="23a30-251">Now that you are done with hello configuration, it's time tootest this new CI/CD pipeline.</span></span> <span data-ttu-id="23a30-252">Hej enklaste sättet tootest är det tooupdate hello källa kod och bekräfta hello ändras till GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="23a30-252">hello easiest way tootest it is tooupdate hello source code and commit hello changes into your GitHub repository.</span></span> <span data-ttu-id="23a30-253">Några sekunder efter att du överför hello kod, ser du en ny version körs i Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="23a30-253">A few seconds after you push hello code, you will see a new build running in Visual Studio Team Services.</span></span> <span data-ttu-id="23a30-254">När slutförts, utlöses en ny version och distribuera hello ny version av programmet hello på hello Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="23a30-254">Once completed successfully, a new release is triggered and deployed hello new version of hello application on hello Azure Container Service cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="23a30-255">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="23a30-255">Next steps</span></span>

* <span data-ttu-id="23a30-256">Mer information om CI/CD-skiva med Visual Studio Team Services finns hello [VSTS skapa översikt](https://www.visualstudio.com/docs/build/overview).</span><span class="sxs-lookup"><span data-stu-id="23a30-256">For more information about CI/CD with Visual Studio Team Services, see hello [VSTS Build overview](https://www.visualstudio.com/docs/build/overview).</span></span>
* <span data-ttu-id="23a30-257">Mer information om ACS-motorn finns hello [ACS-motorn GitHub-repo-](https://github.com/Azure/acs-engine).</span><span class="sxs-lookup"><span data-stu-id="23a30-257">For more information about ACS Engine, see hello [ACS Engine GitHub repo](https://github.com/Azure/acs-engine).</span></span>
* <span data-ttu-id="23a30-258">Mer information om Docker Swarm läge finns hello [Docker Swarm översikt över](https://docs.docker.com/engine/swarm/).</span><span class="sxs-lookup"><span data-stu-id="23a30-258">For more information about Docker Swarm mode, see hello [Docker Swarm mode overview](https://docs.docker.com/engine/swarm/).</span></span>
