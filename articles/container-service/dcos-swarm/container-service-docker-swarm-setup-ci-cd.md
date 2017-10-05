---
title: CI/CD med Azure Container Service och Swarm | Microsoft Docs
description: "Använda Azure Container Service med Docker Swarm, ett Azure Container registret och Visual Studio Team Services för att leverera kontinuerligt ett program för flera behållare .NET Core"
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
ms.openlocfilehash: 99c27c37218a35d2a3416d6edd5e0a871cd5c011
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a><span data-ttu-id="815af-104">Fullständig CI/CD-pipelinen för att distribuera ett program för flera behållare på Azure Container Service med Docker Swarm med hjälp av Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="815af-104">Full CI/CD pipeline to deploy a multi-container application on Azure Container Service with Docker Swarm using Visual Studio Team Services</span></span>

<span data-ttu-id="815af-105">En av de största utmaningarna när utveckla moderna applikationer för molnet att kunna leverera dessa program kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="815af-105">One of the biggest challenges when developing modern applications for the cloud is being able to deliver these applications continuously.</span></span> <span data-ttu-id="815af-106">Lär dig hur du implementerar en fullständig kontinuerlig integrering och distribution (CI/CD) pipeline med Docker Swarm Azure Container registret och Visual Studio Team Services build Azure Container Service och versionshantering i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="815af-106">In this article, you learn how to implement a full continuous integration and deployment (CI/CD) pipeline using Azure Container Service with Docker Swarm, Azure Container Registry, and Visual Studio Team Services build and release management.</span></span>

<span data-ttu-id="815af-107">Den här artikeln är baserad på ett enkelt program tillgängliga på [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), utvecklade med ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="815af-107">This article is based on a simple application, available on [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), developed with ASP.NET Core.</span></span> <span data-ttu-id="815af-108">Programmet består av fyra olika tjänster: tre webb-API: er och en frontwebb:</span><span class="sxs-lookup"><span data-stu-id="815af-108">The application is composed of four different services: three web APIs and one web front end:</span></span>

![MyShop exempelprogrammet](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

<span data-ttu-id="815af-110">Målet är att ge alltid det här programmet i ett Docker Swarm-kluster med hjälp av Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="815af-110">The objective is to deliver this application continuously in a Docker Swarm cluster, using Visual Studio Team Services.</span></span> <span data-ttu-id="815af-111">Följande bild beskrivs denna kontinuerlig leverans pipeline:</span><span class="sxs-lookup"><span data-stu-id="815af-111">The following figure details this continuous delivery pipeline:</span></span>

![MyShop exempelprogrammet](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

<span data-ttu-id="815af-113">Här följer en kort beskrivning av steg:</span><span class="sxs-lookup"><span data-stu-id="815af-113">Here is a brief explanation of the steps:</span></span>

1. <span data-ttu-id="815af-114">Koden ändringarna har bekräftats kod källdatabasen (här GitHub)</span><span class="sxs-lookup"><span data-stu-id="815af-114">Code changes are committed to the source code repository (here, GitHub)</span></span> 
2. <span data-ttu-id="815af-115">GitHub utlöser en version i Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="815af-115">GitHub triggers a build in Visual Studio Team Services</span></span> 
3. <span data-ttu-id="815af-116">Visual Studio Team Services får den senaste versionen av källorna och skapar alla avbildningar som ska utgöra programmet</span><span class="sxs-lookup"><span data-stu-id="815af-116">Visual Studio Team Services gets the latest version of the sources and builds all the images that compose the application</span></span> 
4. <span data-ttu-id="815af-117">Visual Studio Team Services skickar varje avbildning till en Docker-registret som skapats med Azure Container Registry-tjänsten</span><span class="sxs-lookup"><span data-stu-id="815af-117">Visual Studio Team Services pushes each image to a Docker registry created using the Azure Container Registry service</span></span> 
5. <span data-ttu-id="815af-118">Visual Studio Team Services utlöser en ny version</span><span class="sxs-lookup"><span data-stu-id="815af-118">Visual Studio Team Services triggers a new release</span></span> 
6. <span data-ttu-id="815af-119">Versionen kör vissa kommandon med hjälp av SSH på Azure container service master klusternod</span><span class="sxs-lookup"><span data-stu-id="815af-119">The release runs some commands using SSH on the Azure container service cluster master node</span></span> 
7. <span data-ttu-id="815af-120">Docker Swarm på klustret hämtar den senaste versionen av avbildningar</span><span class="sxs-lookup"><span data-stu-id="815af-120">Docker Swarm on the cluster pulls the latest version of the images</span></span> 
8. <span data-ttu-id="815af-121">Den nya versionen av programmet distribueras med Docker Compose</span><span class="sxs-lookup"><span data-stu-id="815af-121">The new version of the application is deployed using Docker Compose</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="815af-122">Krav</span><span class="sxs-lookup"><span data-stu-id="815af-122">Prerequisites</span></span>

<span data-ttu-id="815af-123">Innan du påbörjar de här självstudierna måste du utföra följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="815af-123">Before starting this tutorial, you need to complete the following tasks:</span></span>

- [<span data-ttu-id="815af-124">Skapa ett Swarm-kluster i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="815af-124">Create a Swarm cluster in Azure Container Service</span></span>](container-service-deployment.md)
- [<span data-ttu-id="815af-125">Anslut till Swarm-klustret i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="815af-125">Connect with the Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)
- [<span data-ttu-id="815af-126">Skapa en Azure-behållaren registernyckel</span><span class="sxs-lookup"><span data-stu-id="815af-126">Create an Azure container registry</span></span>](../../container-registry/container-registry-get-started-portal.md)
- [<span data-ttu-id="815af-127">Har en Visual Studio Team Services-konto och team projekt</span><span class="sxs-lookup"><span data-stu-id="815af-127">Have a Visual Studio Team Services account and team project created</span></span>](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [<span data-ttu-id="815af-128">Duplicera GitHub-lagringsplatsen till GitHub-konto</span><span class="sxs-lookup"><span data-stu-id="815af-128">Fork the GitHub repository to your GitHub account</span></span>](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

<span data-ttu-id="815af-129">Du måste också en Ubuntu (14.04 eller 16.04)-dator med Docker installerad.</span><span class="sxs-lookup"><span data-stu-id="815af-129">You also need an Ubuntu (14.04 or 16.04) machine with Docker installed.</span></span> <span data-ttu-id="815af-130">Den här datorn används av Visual Studio Team Services under version och utgåva.</span><span class="sxs-lookup"><span data-stu-id="815af-130">This machine is used by Visual Studio Team Services during the build and release processes.</span></span> <span data-ttu-id="815af-131">Ett sätt att skapa den här datorn är att använda avbildningen som är tillgängliga i den [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/).</span><span class="sxs-lookup"><span data-stu-id="815af-131">One way to create this machine is to use the image available in the [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/).</span></span> 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a><span data-ttu-id="815af-132">Steg 1: Konfigurera Visual Studio Team Services-konto</span><span class="sxs-lookup"><span data-stu-id="815af-132">Step 1: Configure your Visual Studio Team Services account</span></span> 

<span data-ttu-id="815af-133">I det här avsnittet kan du konfigurera Visual Studio Team Services-konto.</span><span class="sxs-lookup"><span data-stu-id="815af-133">In this section, you configure your Visual Studio Team Services account.</span></span>

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a><span data-ttu-id="815af-134">Konfigurera en Visual Studio Team Services Linux build-agent</span><span class="sxs-lookup"><span data-stu-id="815af-134">Configure a Visual Studio Team Services Linux build agent</span></span>

<span data-ttu-id="815af-135">För att skapa avbildningar av Docker och skicka dessa avbildningar till en Azure-behållaren registret från en version av Visual Studio Team Services, som du behöver registrera en Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="815af-135">To create Docker images and push these images into an Azure container registry from a Visual Studio Team Services build, you need to register a Linux agent.</span></span> <span data-ttu-id="815af-136">Du har dessa installationsalternativ:</span><span class="sxs-lookup"><span data-stu-id="815af-136">You have these installation options:</span></span>

* [<span data-ttu-id="815af-137">Distribuera en agent på Linux</span><span class="sxs-lookup"><span data-stu-id="815af-137">Deploy an agent on Linux</span></span>](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [<span data-ttu-id="815af-138">Använda Docker för att köra VSTS-agent</span><span class="sxs-lookup"><span data-stu-id="815af-138">Use Docker to run the VSTS agent</span></span>](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-the-docker-integration-vsts-extension"></a><span data-ttu-id="815af-139">Installera tillägget Docker integrering VSTS</span><span class="sxs-lookup"><span data-stu-id="815af-139">Install the Docker Integration VSTS extension</span></span>

<span data-ttu-id="815af-140">Microsoft tillhandahåller ett VSTS tillägg för att arbeta med Docker i version och utgåva processer.</span><span class="sxs-lookup"><span data-stu-id="815af-140">Microsoft provides a VSTS extension to work with Docker in build and release processes.</span></span> <span data-ttu-id="815af-141">Det här tillägget är tillgängligt i den [VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker).</span><span class="sxs-lookup"><span data-stu-id="815af-141">This extension is available in the [VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker).</span></span> <span data-ttu-id="815af-142">Klicka på **installera** att lägga till det här tillägget i VSTS kontot:</span><span class="sxs-lookup"><span data-stu-id="815af-142">Click **Install** to add this extension to your VSTS account:</span></span>

![Installera Docker-integrering](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

<span data-ttu-id="815af-144">Du uppmanas att ansluta till ditt VSTS-konto med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="815af-144">You are asked to connect to your VSTS account using your credentials.</span></span> 

### <a name="connect-visual-studio-team-services-and-github"></a><span data-ttu-id="815af-145">Ansluta Visual Studio Team Services och GitHub</span><span class="sxs-lookup"><span data-stu-id="815af-145">Connect Visual Studio Team Services and GitHub</span></span>

<span data-ttu-id="815af-146">Upprätta en anslutning mellan projektet VSTS och GitHub-konto.</span><span class="sxs-lookup"><span data-stu-id="815af-146">Set up a connection between your VSTS project and your GitHub account.</span></span>

1. <span data-ttu-id="815af-147">I ditt projekt i Visual Studio Team Services, klickar du på den **inställningar** ikonen i verktygsfältet och välj **Services**.</span><span class="sxs-lookup"><span data-stu-id="815af-147">In your Visual Studio Team Services project, click the **Settings** icon in the toolbar, and select **Services**.</span></span>

    ![Visual Studio Team Services - extern anslutning](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

2. <span data-ttu-id="815af-149">Klicka på vänster, **nya tjänstslutpunkten** > **GitHub**.</span><span class="sxs-lookup"><span data-stu-id="815af-149">On the left, click **New Service Endpoint** > **GitHub**.</span></span>

    ![Visual Studio Team Services - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

3. <span data-ttu-id="815af-151">För att godkänna VSTS att arbeta med GitHub-konto, klickar du på **auktorisera** och följ proceduren i fönstret som öppnas.</span><span class="sxs-lookup"><span data-stu-id="815af-151">To authorize VSTS to work with your GitHub account, click **Authorize** and follow the procedure in the window that opens.</span></span>

    ![Visual Studio Team Services - auktorisera GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-to-your-azure-container-registry-and-azure-container-service-cluster"></a><span data-ttu-id="815af-153">Ansluta VSTS till ditt Azure-behållaren registret och Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="815af-153">Connect VSTS to your Azure container registry and Azure Container Service cluster</span></span>

<span data-ttu-id="815af-154">De sista stegen innan du hämtar till CI/CD-pipeline är att konfigurera externa anslutningar till behållaren registret och Docker Swarm-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="815af-154">The last steps before getting into the CI/CD pipeline are to configure external connections to your container registry and your Docker Swarm cluster in Azure.</span></span> 

1. <span data-ttu-id="815af-155">I den **Services** inställningarna för ditt projekt i Visual Studio Team Services, lägga till en tjänstslutpunkt av typen **Docker registret**.</span><span class="sxs-lookup"><span data-stu-id="815af-155">In the **Services** settings of your Visual Studio Team Services project, add a service endpoint of type **Docker Registry**.</span></span> 

2. <span data-ttu-id="815af-156">I popup-fönstret som öppnas, anger du URL och autentiseringsuppgifter för Azure-behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="815af-156">In the popup that opens, enter the URL and the credentials of your Azure container registry.</span></span>

    ![Visual Studio Team Services - Docker-registret](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

3. <span data-ttu-id="815af-158">Lägga till en slutpunkt av typen för Docker Swarm-kluster **SSH**.</span><span class="sxs-lookup"><span data-stu-id="815af-158">For the Docker Swarm cluster, add an endpoint of type **SSH**.</span></span> <span data-ttu-id="815af-159">Ange SSH-anslutningsinformationen för Swarm-klustret.</span><span class="sxs-lookup"><span data-stu-id="815af-159">Then enter the SSH connection information of your Swarm cluster.</span></span>

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

<span data-ttu-id="815af-161">All konfiguration görs nu.</span><span class="sxs-lookup"><span data-stu-id="815af-161">All the configuration is done now.</span></span> <span data-ttu-id="815af-162">I nästa steg ska skapa du CI/CD-pipeline som skapar och distribuerar programmet till Docker Swarm-kluster.</span><span class="sxs-lookup"><span data-stu-id="815af-162">In the next steps, you create the CI/CD pipeline that builds and deploys the application to the Docker Swarm cluster.</span></span> 

## <a name="step-2-create-the-build-definition"></a><span data-ttu-id="815af-163">Steg 2: Skapa build-definition</span><span class="sxs-lookup"><span data-stu-id="815af-163">Step 2: Create the build definition</span></span>

<span data-ttu-id="815af-164">I det här steget du ställer in en build definitionfor VSTS-projektet och definiera build-arbetsflödet för bilderna behållare</span><span class="sxs-lookup"><span data-stu-id="815af-164">In this step, you set up a build definitionfor your VSTS project and define the build workflow for your container images</span></span>

### <a name="initial-definition-setup"></a><span data-ttu-id="815af-165">Inledande definition installationen</span><span class="sxs-lookup"><span data-stu-id="815af-165">Initial definition setup</span></span>

1. <span data-ttu-id="815af-166">Om du vill skapa en build-definition, ansluta till ditt projekt i Visual Studio Team Services och klicka på **Skapa & släpper**.</span><span class="sxs-lookup"><span data-stu-id="815af-166">To create a build definition, connect to your Visual Studio Team Services project and click **Build & Release**.</span></span> 

2. <span data-ttu-id="815af-167">I den **skapa definitioner** klickar du på **+ ny**.</span><span class="sxs-lookup"><span data-stu-id="815af-167">In the **Build definitions** section, click **+ New**.</span></span> <span data-ttu-id="815af-168">Välj den **tom** mall.</span><span class="sxs-lookup"><span data-stu-id="815af-168">Select the **Empty** template.</span></span>

    ![Visual Studio Team Services - nya skapa Definition](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

3. <span data-ttu-id="815af-170">Konfigurera den nya versionen med en källa för GitHub-lagringsplatsen, kontrollera **kontinuerlig integration**, och välj agent kön där du har registrerat din Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="815af-170">Configure the new build with a GitHub repository source, check **Continuous integration**, and select the agent queue where you registered your Linux agent.</span></span> <span data-ttu-id="815af-171">Klicka på **skapa** att skapa build-definition.</span><span class="sxs-lookup"><span data-stu-id="815af-171">Click **Create** to create the build definition.</span></span>

    ![Visual Studio Team Services - skapa Build-Definition](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

4. <span data-ttu-id="815af-173">På den **skapa definitioner** sidan, öppnar du först den **databasen** och konfigurera versionen om du vill använda förgrening av MyShop projektet som du skapade i förutsättningarna.</span><span class="sxs-lookup"><span data-stu-id="815af-173">On the **Build Definitions** page, first open the **Repository** tab and configure the build to use the fork of the MyShop project that you created in the prerequisites.</span></span> <span data-ttu-id="815af-174">Kontrollera att du väljer *acs-dokumenten* som den **standardförgrening**.</span><span class="sxs-lookup"><span data-stu-id="815af-174">Make sure that you select *acs-docs* as the **Default branch**.</span></span>

    ![Visual Studio Team Services - databasen Versionskonfiguration](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

5. <span data-ttu-id="815af-176">På den **utlösare** konfigurerar build som utlöses efter varje genomförande.</span><span class="sxs-lookup"><span data-stu-id="815af-176">On the **Triggers** tab, configure the build to be triggered after each commit.</span></span> <span data-ttu-id="815af-177">Välj **kontinuerlig integration** och **Batch ändringar**.</span><span class="sxs-lookup"><span data-stu-id="815af-177">Select **Continuous integration** and **Batch changes**.</span></span>

    ![Visual Studio Team Services - konfiguration av Build-utlösare](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-the-build-workflow"></a><span data-ttu-id="815af-179">Definiera build-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="815af-179">Define the build workflow</span></span>
<span data-ttu-id="815af-180">Nästa steg definiera build-arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="815af-180">The next steps define the build workflow.</span></span> <span data-ttu-id="815af-181">Det finns fem behållaren avbildningar för att skapa för den *MyShop* program.</span><span class="sxs-lookup"><span data-stu-id="815af-181">There are five container images to build for the *MyShop* application.</span></span> <span data-ttu-id="815af-182">Varje avbildning skapas med Dockerfile i projektet mappar:</span><span class="sxs-lookup"><span data-stu-id="815af-182">Each image is built using the Dockerfile located in the project folders:</span></span>

* <span data-ttu-id="815af-183">ProductsApi</span><span class="sxs-lookup"><span data-stu-id="815af-183">ProductsApi</span></span>
* <span data-ttu-id="815af-184">Proxy</span><span class="sxs-lookup"><span data-stu-id="815af-184">Proxy</span></span>
* <span data-ttu-id="815af-185">RatingsApi</span><span class="sxs-lookup"><span data-stu-id="815af-185">RatingsApi</span></span>
* <span data-ttu-id="815af-186">RecommandationsApi</span><span class="sxs-lookup"><span data-stu-id="815af-186">RecommandationsApi</span></span>
* <span data-ttu-id="815af-187">ShopFront</span><span class="sxs-lookup"><span data-stu-id="815af-187">ShopFront</span></span>

<span data-ttu-id="815af-188">Du måste lägga till två Docker steg för varje bild, en för att skapa avbildningen och en push-avbildningen i registret för Azure-behållaren.</span><span class="sxs-lookup"><span data-stu-id="815af-188">You need to add two Docker steps for each image, one to build the image, and one to push the image in the Azure container registry.</span></span> 

1. <span data-ttu-id="815af-189">Om du vill lägga till ett steg i build-arbetsflöde, klickar du på **+ Lägg till build steg** och välj **Docker**.</span><span class="sxs-lookup"><span data-stu-id="815af-189">To add a step in the build workflow, click **+ Add build step** and select **Docker**.</span></span>

    ![Visual Studio Team Services – Lägg till Build steg](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

2. <span data-ttu-id="815af-191">För varje bild och konfigurera ett steg som använder den `docker build` kommando.</span><span class="sxs-lookup"><span data-stu-id="815af-191">For each image, configure one step that uses the `docker build` command.</span></span>

    ![Visual Studio Team Services - Docker skapa](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    <span data-ttu-id="815af-193">Välj din Azure-behållaren i registret för build-åtgärden i **skapa en avbildning** åtgärd och Dockerfile som definierar varje avbildning.</span><span class="sxs-lookup"><span data-stu-id="815af-193">For the build operation, select your Azure container registry, the **Build an image** action, and the Dockerfile that defines each image.</span></span> <span data-ttu-id="815af-194">Ange den **Build-kontexten** som Dockerfile rotkatalogen och definiera de **avbildningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="815af-194">Set the **Build context** as the Dockerfile root directory, and define the **Image Name**.</span></span> 
    
    <span data-ttu-id="815af-195">Starta Avbildningsnamnet med URI för Azure-behållaren registret som visas i den föregående skärmbilden.</span><span class="sxs-lookup"><span data-stu-id="815af-195">As shown on the preceding screen, start the image name with the URI of your Azure container registry.</span></span> <span data-ttu-id="815af-196">(Du kan också använda en build-variabel till parameterstyra taggen bild, till exempel build-ID i det här exemplet.)</span><span class="sxs-lookup"><span data-stu-id="815af-196">(You can also use a build variable to parameterize the tag of the image, such as the build identifier in this example.)</span></span>

3. <span data-ttu-id="815af-197">Konfigurera ett andra steg som används för varje avbildning i `docker push` kommando.</span><span class="sxs-lookup"><span data-stu-id="815af-197">For each image, configure a second step that uses the `docker push` command.</span></span>

    ![Visual Studio Team Services - Docker Push](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    <span data-ttu-id="815af-199">Välj din Azure-behållaren i registret för push-åtgärden i **Push en bild** åtgärd, och ange den **avbildningsnamn** som är inbyggd i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="815af-199">For the push operation, select your Azure container registry, the **Push an image** action, and enter the **Image Name** that is built in the previous step.</span></span>

4. <span data-ttu-id="815af-200">När du har konfigurerat bygg- och push-stegen för var och en av fem avbildningar kan du lägga till två fler steg i build-arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="815af-200">After you configure the build and push steps for each of the five images, add two more steps in the build workflow.</span></span>

    <span data-ttu-id="815af-201">a.</span><span class="sxs-lookup"><span data-stu-id="815af-201">a.</span></span> <span data-ttu-id="815af-202">En kommandoradsaktivitet som använder ett bash-skript för att ersätta den *BuildNumber* förekomsten i filen docker-compose.yml med aktuellt skapa Id. Se följande skärm för mer information.</span><span class="sxs-lookup"><span data-stu-id="815af-202">A command-line task that uses a bash script to replace the *BuildNumber* occurrence in the docker-compose.yml file with the current build Id. See the following screen for details.</span></span>

    ![Visual Studio Team Services - uppdatering Skriv fil](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    <span data-ttu-id="815af-204">b.</span><span class="sxs-lookup"><span data-stu-id="815af-204">b.</span></span> <span data-ttu-id="815af-205">En uppgift som släpper den uppdaterade Skriv-filen som ett build-artefakt så den kan användas i versionen.</span><span class="sxs-lookup"><span data-stu-id="815af-205">A task that drops the updated Compose file as a build artifact so it can be used in the release.</span></span> <span data-ttu-id="815af-206">Se följande skärm för mer information.</span><span class="sxs-lookup"><span data-stu-id="815af-206">See the following screen for details.</span></span>

    ![Visual Studio Team Services - Publicera Skriv fil](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

5. <span data-ttu-id="815af-208">Klicka på **spara** och namnet build-definition.</span><span class="sxs-lookup"><span data-stu-id="815af-208">Click **Save** and name your build definition.</span></span>

## <a name="step-3-create-the-release-definition"></a><span data-ttu-id="815af-209">Steg 3: Skapa en definition för versionen</span><span class="sxs-lookup"><span data-stu-id="815af-209">Step 3: Create the release definition</span></span>

<span data-ttu-id="815af-210">Visual Studio Team Services kan du [hantera versioner i miljöer](https://www.visualstudio.com/team-services/release-management/).</span><span class="sxs-lookup"><span data-stu-id="815af-210">Visual Studio Team Services allows you to [manage releases across environments](https://www.visualstudio.com/team-services/release-management/).</span></span> <span data-ttu-id="815af-211">Du kan aktivera kontinuerlig distribution att se till att programmet har distribuerats på dina olika miljöer (t.ex dev, testa, Förproduktion och produktion) i enkelt.</span><span class="sxs-lookup"><span data-stu-id="815af-211">You can enable continuous deployment to make sure that your application is deployed on your different environments (such as dev, test, pre-production, and production) in a smooth way.</span></span> <span data-ttu-id="815af-212">Du kan skapa en ny miljö som representerar dina Azure Container Service Docker Swarm-kluster.</span><span class="sxs-lookup"><span data-stu-id="815af-212">You can create a new environment that represents your Azure Container Service Docker Swarm cluster.</span></span>

![Visual Studio Team Services - versionen till ACS](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a><span data-ttu-id="815af-214">Första utgåvan installationen</span><span class="sxs-lookup"><span data-stu-id="815af-214">Initial release setup</span></span>

1. <span data-ttu-id="815af-215">Klicka för att skapa en definition av versionen **versioner** > **+ släpper**</span><span class="sxs-lookup"><span data-stu-id="815af-215">To create a release definition, click **Releases** > **+ Release**</span></span>

2. <span data-ttu-id="815af-216">Om du vill konfigurera artefakt källan, klickar du på **artefakter** > **länka en artefakt källa**.</span><span class="sxs-lookup"><span data-stu-id="815af-216">To configure the artifact source, Click **Artifacts** > **Link an artifact source**.</span></span> <span data-ttu-id="815af-217">Här kan länka den här nya versionen definitionen till build-versionen som du definierade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="815af-217">Here, link this new release definition to the build that you defined in the previous step.</span></span> <span data-ttu-id="815af-218">På så sätt finns filen docker-compose.yml i release-processen.</span><span class="sxs-lookup"><span data-stu-id="815af-218">By doing this, the docker-compose.yml file is available in the release process.</span></span>

    ![Visual Studio Team Services - versionen artefakter](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

3. <span data-ttu-id="815af-220">För att konfigurera utlösaren versionen, klickar du på **utlösare** och välj **kontinuerlig distribution**.</span><span class="sxs-lookup"><span data-stu-id="815af-220">To configure the release trigger, click **Triggers** and select **Continuous Deployment**.</span></span> <span data-ttu-id="815af-221">Ange utlösaren på samma artefakt källa.</span><span class="sxs-lookup"><span data-stu-id="815af-221">Set the trigger on the same artifact source.</span></span> <span data-ttu-id="815af-222">Den här inställningen garanterar att en ny version startar så fort versionen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="815af-222">This setting ensures that a new release starts as soon as the build completes successfully.</span></span>

    ![Visual Studio Team Services - versionen utlösare](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-the-release-workflow"></a><span data-ttu-id="815af-224">Definiera versionen arbetsflödet</span><span class="sxs-lookup"><span data-stu-id="815af-224">Define the release workflow</span></span>

<span data-ttu-id="815af-225">Arbetsflödet versionen består av två aktiviteter som du lägger till.</span><span class="sxs-lookup"><span data-stu-id="815af-225">The release workflow is composed of two tasks that you add.</span></span>

1. <span data-ttu-id="815af-226">Konfigurera en aktivitet för att på ett säkert sätt kopiera skriva filen till en *distribuera* mapp på Docker Swarm huvudnoden, med hjälp av SSH-anslutningen som du tidigare konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="815af-226">Configure a task to securely copy the compose file to a *deploy* folder on the Docker Swarm master node, using the SSH connection you configured previously.</span></span> <span data-ttu-id="815af-227">Se följande skärm för mer information.</span><span class="sxs-lookup"><span data-stu-id="815af-227">See the following screen for details.</span></span>

    ![Visual Studio Team Services - versionen SCP](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

2. <span data-ttu-id="815af-229">Konfigurera en annan uppgift om du vill köra ett bash-kommando för att köra `docker` och `docker-compose` kommandon på huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="815af-229">Configure a second task to execute a bash command to run `docker` and `docker-compose` commands on the master node.</span></span> <span data-ttu-id="815af-230">Se följande skärm för mer information.</span><span class="sxs-lookup"><span data-stu-id="815af-230">See the following screen for details.</span></span>

    ![Visual Studio Team Services - versionen Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    <span data-ttu-id="815af-232">Kommandot som kördes på master använda Docker CLI och Docker Compose CLI för att göra följande:</span><span class="sxs-lookup"><span data-stu-id="815af-232">The command executed on the master use the Docker CLI and the Docker-Compose CLI to do the following tasks:</span></span>

    - <span data-ttu-id="815af-233">Logga in på Azure-behållaren registret (den använder tre build variab'les som definieras i den **variabler** fliken)</span><span class="sxs-lookup"><span data-stu-id="815af-233">Login to the Azure container registry (it uses three build variab\`les that are defined in the **Variables** tab)</span></span>
    - <span data-ttu-id="815af-234">Definiera den **DOCKER_HOST** variabeln för att arbeta med Swarm-slutpunkten (: 2375)</span><span class="sxs-lookup"><span data-stu-id="815af-234">Define the **DOCKER_HOST** variable to work with the Swarm endpoint (:2375)</span></span>
    - <span data-ttu-id="815af-235">Navigera till den *distribuera* mapp som har skapats för den föregående aktiviteten säker kopia och som innehåller filen docker-compose.yml</span><span class="sxs-lookup"><span data-stu-id="815af-235">Navigate to the *deploy* folder that was created by the preceding secure copy task and that contains the docker-compose.yml file</span></span> 
    - <span data-ttu-id="815af-236">Köra `docker-compose` kommandon som hämtar nya bilder stoppa tjänsterna, ta bort tjänsterna och skapa behållarna.</span><span class="sxs-lookup"><span data-stu-id="815af-236">Execute `docker-compose` commands that pull the new images, stop the services, remove the services, and create the containers.</span></span>

    >[!IMPORTANT]
    > <span data-ttu-id="815af-237">Som det visas i den föregående skärmbilden, lämnar den **misslyckas på STDERR** kryssrutan avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="815af-237">As shown on the preceding screen, leave the **Fail on STDERR** checkbox unchecked.</span></span> <span data-ttu-id="815af-238">Detta är ett viktigt, eftersom `docker-compose` flera diagnostiska meddelanden skrivs ut som behållare är stoppas eller tas bort på standardfel utdata.</span><span class="sxs-lookup"><span data-stu-id="815af-238">This is an important setting, because `docker-compose` prints several diagnostic messages, such as containers are stopping or being deleted, on the standard error output.</span></span> <span data-ttu-id="815af-239">Om du markerar kryssrutan rapporterar Visual Studio Team Services att fel uppstod vid version, även om allt går bra.</span><span class="sxs-lookup"><span data-stu-id="815af-239">If you check the checkbox, Visual Studio Team Services reports that errors occurred during the release, even if all goes well.</span></span>
    >
3. <span data-ttu-id="815af-240">Spara den här nya versionen definitionen.</span><span class="sxs-lookup"><span data-stu-id="815af-240">Save this new release definition.</span></span>


>[!NOTE]
><span data-ttu-id="815af-241">Den här distributionen innehåller vissa driftstopp eftersom vi stoppar gamla tjänsterna och köra en ny.</span><span class="sxs-lookup"><span data-stu-id="815af-241">This deployment includes some downtime because we are stopping the old services and running the new one.</span></span> <span data-ttu-id="815af-242">Det går att undvika detta genom att göra en blå-grön-distribution.</span><span class="sxs-lookup"><span data-stu-id="815af-242">It is possible to avoid this by doing a blue-green deployment.</span></span>
>

## <a name="step-4-test-the-cicd-pipeline"></a><span data-ttu-id="815af-243">Steg 4.</span><span class="sxs-lookup"><span data-stu-id="815af-243">Step 4.</span></span> <span data-ttu-id="815af-244">Testa CI/CD-pipelinen</span><span class="sxs-lookup"><span data-stu-id="815af-244">Test the CI/CD pipeline</span></span>

<span data-ttu-id="815af-245">Nu när du är klar med konfigurationen, är det dags att testa den här nya CI/CD-pipeline.</span><span class="sxs-lookup"><span data-stu-id="815af-245">Now that you are done with the configuration, it's time to test this new CI/CD pipeline.</span></span> <span data-ttu-id="815af-246">Det enklaste sättet att testa den är att uppdatera källkod och spara ändringarna i GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="815af-246">The easiest way to test it is to update the source code and commit the changes into your GitHub repository.</span></span> <span data-ttu-id="815af-247">Några sekunder efter att du push-kod, ser du en ny version körs i Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="815af-247">A few seconds after you push the code, you will see a new build running in Visual Studio Team Services.</span></span> <span data-ttu-id="815af-248">När har slutförts ska utlösas när en ny version och distribuerar den nya versionen av programmet på Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="815af-248">Once completed successfully, a new release will be triggered and will deploy the new version of the application on the Azure Container Service cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="815af-249">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="815af-249">Next Steps</span></span>

* <span data-ttu-id="815af-250">Mer information om CI/CD-skiva med Visual Studio Team Services finns i [VSTS skapa översikt](https://www.visualstudio.com/docs/build/overview).</span><span class="sxs-lookup"><span data-stu-id="815af-250">For more information about CI/CD with Visual Studio Team Services, see the [VSTS Build overview](https://www.visualstudio.com/docs/build/overview).</span></span>
