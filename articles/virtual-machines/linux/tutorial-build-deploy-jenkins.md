---
title: "CI/CD: N från Jenkins för virtuella Azure-datorer med Team Services | Microsoft Docs"
description: "Ställ in kontinuerlig integration (KO) och kontinuerlig distribution (CD) av en Node.js-app med Jenkins till Azure virtuella datorer från versionshantering i Visual Studio Team Services VSTS () eller Microsoft Team Foundation Server (TFS)"
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/15/2017
ms.author: ahomer
ms.custom: mvc
ms.openlocfilehash: a40e26a8681df31fad664e4d1df4c1513311900d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-your-app-to-linux-vms-using-jenkins-and-team-services"></a><span data-ttu-id="6a1d2-103">Distribuera din app till Linux virtuella datorer med hjälp av Jenkins och Team Services</span><span class="sxs-lookup"><span data-stu-id="6a1d2-103">Deploy your app to Linux VMs using Jenkins and Team Services</span></span>

<span data-ttu-id="6a1d2-104">Kontinuerlig integration (KO) och kontinuerlig distribution (CD) är en pipeline som du kan bygga, släppa och distribuera din kod.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-104">Continuous integration (CI) and continuous deployment (CD) is a pipeline by which you can build, release, and deploy your code.</span></span> <span data-ttu-id="6a1d2-105">Team Services innehåller en fullständig, komplett funktionalitet uppsättning av verktyg för automatisering av CI/CD för distribution till Azure.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-105">Team Services provides a complete, fully featured set of CI/CD automation tools for deployment to Azure.</span></span> <span data-ttu-id="6a1d2-106">Jenkins är ett populära 3 parts CI/CD-server-baserade verktyg som också ger CI/CD-automatisering.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-106">Jenkins is a popular 3rd-party CI/CD server-based tool that also provides CI/CD automation.</span></span> <span data-ttu-id="6a1d2-107">Du kan använda båda tillsammans för att anpassa hur du levererar ditt molnapp eller tjänst.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-107">You can use both together to customize how you deliver your cloud app or service.</span></span>

<span data-ttu-id="6a1d2-108">I den här självstudiekursen kommer du använda Jenkins för att skapa en **Node.js-webbapp**, och Visual Studio Team Services för att distribuera den till en [distributionsgruppen](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) som innehåller virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-108">In this tutorial, you use Jenkins to build a **Node.js web app**, and Visual Studio Team Services to deploy it to a [deployment group](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) containing Linux virtual machines.</span></span>

<span data-ttu-id="6a1d2-109">Du kommer att:</span><span class="sxs-lookup"><span data-stu-id="6a1d2-109">You will:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6a1d2-110">Skapa din app i Jenkins</span><span class="sxs-lookup"><span data-stu-id="6a1d2-110">Build your app in Jenkins</span></span>
> * <span data-ttu-id="6a1d2-111">Konfigurera Jenkins för integrering med Team Services</span><span class="sxs-lookup"><span data-stu-id="6a1d2-111">Configure Jenkins for Team Services integration</span></span>
> * <span data-ttu-id="6a1d2-112">Skapa en distributionsgrupp för de virtuella Azure-datorerna</span><span class="sxs-lookup"><span data-stu-id="6a1d2-112">Create a deployment group for the Azure virtual machines</span></span>
> * <span data-ttu-id="6a1d2-113">Skapa en definition av versionen som konfigurerar de virtuella datorerna och distribuerar appen</span><span class="sxs-lookup"><span data-stu-id="6a1d2-113">Create a release definition that configures the VMs and deploys the app</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="6a1d2-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="6a1d2-114">Before you begin</span></span>

* <span data-ttu-id="6a1d2-115">Du behöver åtkomst till ett konto för Jenkins.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-115">You need access to a Jenkins account.</span></span> <span data-ttu-id="6a1d2-116">Om du inte har skapat en Jenkins server, se [Jenkins dokumentationen](https://jenkins.io/doc/).</span><span class="sxs-lookup"><span data-stu-id="6a1d2-116">If you have not yet created a Jenkins server, see [Jenkins Documentation](https://jenkins.io/doc/).</span></span> 

* <span data-ttu-id="6a1d2-117">Logga in på ditt Team Services-konto (`https://{youraccount}.visualstudio.com`).</span><span class="sxs-lookup"><span data-stu-id="6a1d2-117">Sign in to your Team Services account (`https://{youraccount}.visualstudio.com`).</span></span> 
  <span data-ttu-id="6a1d2-118">Du kan få en [kostnadsfritt konto Team Services](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).</span><span class="sxs-lookup"><span data-stu-id="6a1d2-118">You can get a [free Team Services account](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).</span></span>

  > [!NOTE]
  > <span data-ttu-id="6a1d2-119">Mer information finns i [Anslut till Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).</span><span class="sxs-lookup"><span data-stu-id="6a1d2-119">For more information, see [Connect to Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).</span></span>

* <span data-ttu-id="6a1d2-120">Skapa en personlig åtkomsttoken (PATRIK) i ditt Team Services-konto om du inte redan har ett.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-120">Create a personal access token (PAT) in your Team Services account if you don't already have one.</span></span> <span data-ttu-id="6a1d2-121">Jenkins kräver den här informationen för att få åtkomst till ditt Team Services-konto.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-121">Jenkins requires this information to access your Team Services account.</span></span>
  <span data-ttu-id="6a1d2-122">Läs [hur skapar jag en personlig åtkomsttoken för Team Services och TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) att lära dig hur du skapar en.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-122">Read [How do I create a personal access token for Team Services and TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) to learn how to generate one.</span></span>

## <a name="get-the-sample-app"></a><span data-ttu-id="6a1d2-123">Hämta sample-appen</span><span class="sxs-lookup"><span data-stu-id="6a1d2-123">Get the sample app</span></span>

<span data-ttu-id="6a1d2-124">Du måste distribuera en app lagras i en Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-124">You need an app to deploy stored in a Git repository.</span></span>
<span data-ttu-id="6a1d2-125">Den här kursen rekommenderar vi du använder [denna sample-appen som är tillgängliga från GitHub](https://github.com/azooinmyluggage/fabrikam-node).</span><span class="sxs-lookup"><span data-stu-id="6a1d2-125">For this tutorial, we recommend you use [this sample app available from GitHub](https://github.com/azooinmyluggage/fabrikam-node).</span></span>

1. <span data-ttu-id="6a1d2-126">Skapa en duplicering av den här appen och Anteckna platsen (URL) för användning i senare steg i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-126">Create a fork of this app and take note of the location (URL) for use in later steps of this tutorial.</span></span>

1. <span data-ttu-id="6a1d2-127">Kontrollera förgrening **offentliga** att förenkla ansluter till GitHub senare.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-127">Make the fork **public** to simplify connecting to GitHub later.</span></span>

> [!NOTE]
> <span data-ttu-id="6a1d2-128">Mer information finns i [duplicera en repo](https://help.github.com/articles/fork-a-repo/) och [gör en privat databas offentliga](https://help.github.com/articles/making-a-private-repository-public/).</span><span class="sxs-lookup"><span data-stu-id="6a1d2-128">For more information, see [Fork a repo](https://help.github.com/articles/fork-a-repo/) and [Making a private repository public](https://help.github.com/articles/making-a-private-repository-public/).</span></span>

> [!NOTE]
> <span data-ttu-id="6a1d2-129">Appen har skapats med [Yeoman](http://yeoman.io/learning/index.html); används **Express**, **bower**, och **grunt**; och har några **npm** paket som beroenden.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-129">The app was built using [Yeoman](http://yeoman.io/learning/index.html); it uses **Express**, **bower**, and **grunt**; and it has some **npm** packages as dependencies.</span></span>
> <span data-ttu-id="6a1d2-130">Exempelappen innehåller en uppsättning [Azure Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) som används för att dynamiskt skapa virtuella datorer för distribution på Azure.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-130">The sample app contains a set of [Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) that are used to dynamically create the virtual machines for deployment on Azure.</span></span> <span data-ttu-id="6a1d2-131">Dessa mallar används av uppgifter i den [Team Services släpper definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).</span><span class="sxs-lookup"><span data-stu-id="6a1d2-131">These templates are used by tasks in the [Team Services release definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).</span></span>
> <span data-ttu-id="6a1d2-132">Den huvudsakliga mallen skapar en nätverkssäkerhetsgrupp, en virtuell dator och ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-132">The main template creates a network security group, a virtual machine, and a virtual network.</span></span> <span data-ttu-id="6a1d2-133">Den tilldelas en offentlig IP-adress och öppnar inkommande port 80.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-133">It assigns a public IP address and opens inbound port 80.</span></span> <span data-ttu-id="6a1d2-134">Det ger också en tagg som används av distributionsgruppen för att välja datorer för att ta emot distributionen.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-134">It also adds a tag that is used by the deployment group to select the machines to receive the deployment.</span></span>
>
> <span data-ttu-id="6a1d2-135">Exemplet innehåller också ett skript som konfigurerar Nginx och distribuerar appen.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-135">The sample also contains a script that sets up Nginx and deploys the app.</span></span> <span data-ttu-id="6a1d2-136">Den körs på var och en av de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-136">It is executed on each of the virtual machines.</span></span> <span data-ttu-id="6a1d2-137">Mer specifikt installerar skriptet nod, Nginx och PM2; konfigurerar Nginx och PM2; Därefter startar appen nod.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-137">Specifically, the script installs Node, Nginx, and PM2; configures Nginx and PM2; then starts the Node app.</span></span>

## <a name="configure-jenkins-plugins"></a><span data-ttu-id="6a1d2-138">Konfigurera Jenkins plugin-program</span><span class="sxs-lookup"><span data-stu-id="6a1d2-138">Configure Jenkins plugins</span></span>

<span data-ttu-id="6a1d2-139">Först måste du konfigurera två Jenkins plugin-program för **NodeJS** och **integrering med Team Services**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-139">First, you must configure two Jenkins plugins for **NodeJS** and **Integration with Team Services**.</span></span>

1. <span data-ttu-id="6a1d2-140">Öppna Jenkins-konto och välj **hantera Jenkins**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-140">Open your Jenkins account and choose **Manage Jenkins**.</span></span>

1. <span data-ttu-id="6a1d2-141">I den **hantera Jenkins** väljer **hantera plugin-program**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-141">In the **Manage Jenkins** page, choose **Manage Plugins**.</span></span>

1. <span data-ttu-id="6a1d2-142">Filtrera listan för att hitta den **NodeJS** plugin-program och installera den utan omstart.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-142">Filter the list to locate the **NodeJS** plugin and install it without restart.</span></span>

   ![Att lägga till plugin-programmet för NodeJS Jenkins](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)

1. <span data-ttu-id="6a1d2-144">Filtrera listan efter den **Team Foundation Server** plugin-program och installera den.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-144">Filter the list to find the **Team Foundation Server** plugin and install it.</span></span> <span data-ttu-id="6a1d2-145">(Det här plugin-programmet fungerar för både Team Services och Team Foundation Server.) Det är inte nödvändigt att starta om Jenkins.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-145">(This plug-in works for both Team Services and Team Foundation Server.) Restarting Jenkins is not necessary.</span></span>

## <a name="configure-jenkins-build-for-nodejs"></a><span data-ttu-id="6a1d2-146">Konfigurera Jenkins build för Node.js</span><span class="sxs-lookup"><span data-stu-id="6a1d2-146">Configure Jenkins build for Node.js</span></span>

<span data-ttu-id="6a1d2-147">Skapa ett nytt build-projekt i Jenkins, och konfigurerar det enligt följande:</span><span class="sxs-lookup"><span data-stu-id="6a1d2-147">In Jenkins, create a new build project and configure it as follows:</span></span>

1. <span data-ttu-id="6a1d2-148">I den **allmänna** och anger ett namn för ditt build-projekt.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-148">In the **General** tab, enter a name for your build project.</span></span>

1. <span data-ttu-id="6a1d2-149">I den **källa kod Management** väljer **Git** och ange information om databasen och grenen med Appkod.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-149">In the **Source Code Management** tab, select **Git** and enter the details of the repository and the branch containing your app code.</span></span>

   ![Lägg till en repo i din version](media/tutorial-build-deploy-jenkins/jenkins-git.png)

   > [!NOTE]
   > <span data-ttu-id="6a1d2-151">Om databasen inte är offentlig väljer **Lägg till** och ange autentiseringsuppgifter för att ansluta till den.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-151">If your repository is not public, choose **Add** and provide credentials to connect to it.</span></span>

1. <span data-ttu-id="6a1d2-152">I den **Skapa utlösare** väljer **avsökning SCM** och ange schemat `H/03 * * * *` avsöker Git-lagringsplats för ändringar i alla tre minuter.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-152">In the **Build Triggers** tab, select **Poll SCM** and enter the schedule `H/03 * * * *` to poll the Git repository for changes every three minutes.</span></span> 

1. <span data-ttu-id="6a1d2-153">I den **kompileringsmiljö** väljer **ange nod &amp; npm bin / mappen SÖKVÄGEN** och ange `NodeJS` för Installation av JS-värdet.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-153">In the **Build Environment** tab, select **Provide Node &amp; npm bin/ folder PATH** and enter `NodeJS` for the Node JS Installation value.</span></span> <span data-ttu-id="6a1d2-154">Lämna **npmrc filen** inställd på ”Använd systemstandard”.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-154">Leave **npmrc file** set to "use system default."</span></span>

1. <span data-ttu-id="6a1d2-155">I den **skapa** ange kommandot `npm install` att se till att alla beroenden är uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-155">In the **Build** tab, enter the command `npm install` to ensure all dependencies are updated.</span></span>

## <a name="configure-jenkins-for-team-services-integration"></a><span data-ttu-id="6a1d2-156">Konfigurera Jenkins för integrering med Team Services</span><span class="sxs-lookup"><span data-stu-id="6a1d2-156">Configure Jenkins for Team Services integration</span></span>

1. <span data-ttu-id="6a1d2-157">I den **efter build åtgärder** fliken för **filer att arkivera**, ange `**/*` att inkludera alla filer.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-157">In the **Post-build Actions** tab, for **Files to archive**, enter `**/*` to include all files.</span></span>

1. <span data-ttu-id="6a1d2-158">För **utlösa version i TFS/Team Services**, ange en fullständig Webbadress för ditt konto (som `https://your-account-name.visualstudio.com`), projektnamnet, ett namn för versionen definitionen (skapas senare) och autentiseringsuppgifterna för att ansluta till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-158">For **Trigger release in TFS/Team Services**, enter the full URL of your account (such as `https://your-account-name.visualstudio.com`), the project name, a name for the release definition (created later), and the credentials to connect to your account.</span></span>
   <span data-ttu-id="6a1d2-159">Du behöver ditt användarnamn och PATRIK som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-159">You need your user name and the PAT you created earlier.</span></span> 

   ![Konfigurera Jenkins efter build-åtgärder](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)

1. <span data-ttu-id="6a1d2-161">Spara build-projektet.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-161">Save the build project.</span></span>

## <a name="create-a-jenkins-service-endpoint"></a><span data-ttu-id="6a1d2-162">Skapa en Jenkins tjänstslutpunkt</span><span class="sxs-lookup"><span data-stu-id="6a1d2-162">Create a Jenkins service endpoint</span></span>

<span data-ttu-id="6a1d2-163">En tjänstslutpunkt kan Team Services att ansluta till Jenkins.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-163">A service endpoint allows Team Services to connect to Jenkins.</span></span>

1. <span data-ttu-id="6a1d2-164">Öppna den **Services** i Team Services, öppna den **nya tjänstslutpunkten** listan, och välj **Jenkins**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-164">Open the **Services** page in Team Services, open the **New Service Endpoint** list, and choose **Jenkins**.</span></span>

   ![Lägg till en Jenkins slutpunkt](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)

1. <span data-ttu-id="6a1d2-166">Ange ett namn som du använder för att referera till den här anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-166">Enter a name you will use to refer to this connection.</span></span>

1. <span data-ttu-id="6a1d2-167">Ange Webbadressen till din Jenkins server och skalstreck den **accepterar ej betrodda certifikat för SSL** alternativet.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-167">Enter the URL of your Jenkins server, and tick the **Accept untrusted SSL certificates** option.</span></span>

1. <span data-ttu-id="6a1d2-168">Ange användarnamn och lösenord för kontot för Jenkins.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-168">Enter the user name and password for your Jenkins account.</span></span>

1. <span data-ttu-id="6a1d2-169">Välj **verifiera anslutning** att kontrollera att informationen är korrekt.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-169">Choose **Verify connection** to check that the information is correct.</span></span>

1. <span data-ttu-id="6a1d2-170">Välj **OK** att skapa tjänsteslutpunkt.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-170">Choose **OK** to create the service endpoint.</span></span>

## <a name="create-a-deployment-group"></a><span data-ttu-id="6a1d2-171">Skapa en distributionsgrupp</span><span class="sxs-lookup"><span data-stu-id="6a1d2-171">Create a deployment group</span></span>

<span data-ttu-id="6a1d2-172">Du behöver en [distributionsgruppen](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) som innehåller de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-172">You need a [deployment group](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) to  contain the virtual machines.</span></span>

1. <span data-ttu-id="6a1d2-173">Öppna den **versioner** för den **skapa &amp; versionen** hubben, öppna den **distributionsgrupper** , och välj **+ ny**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-173">Open the **Releases** tab of the **Build &amp; Release** hub, then open the **Deployment groups** tab, and choose **+ New**.</span></span>

1. <span data-ttu-id="6a1d2-174">Ange ett namn för distributionsgruppen och en valfri beskrivning.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-174">Enter a name for the deployment group, and an optional description.</span></span>
   <span data-ttu-id="6a1d2-175">Välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-175">Then choose **Create**.</span></span>

<span data-ttu-id="6a1d2-176">Aktiviteten Azure Resource distribution skapar och registrerar de virtuella datorerna när den körs med hjälp av Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-176">The Azure Resource Group Deployment task creates and registers the VMs when it runs using the Azure Resource Manager template.</span></span>
<span data-ttu-id="6a1d2-177">Du behöver inte skapa och registrera de virtuella datorerna själv.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-177">You don't need to create and register the virtual machines yourself.</span></span>

## <a name="create-a-release-definition"></a><span data-ttu-id="6a1d2-178">Skapa en definition för versionen</span><span class="sxs-lookup"><span data-stu-id="6a1d2-178">Create a release definition</span></span>

<span data-ttu-id="6a1d2-179">En definition av versionen anger processen Team Services kommer att köras om du vill distribuera appen.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-179">A release definition specifies the process Team Services will execute to deploy the app.</span></span>
<span data-ttu-id="6a1d2-180">Skapa definition för versionen i Team Services:</span><span class="sxs-lookup"><span data-stu-id="6a1d2-180">To create the release definition in Team Services:</span></span>

1. <span data-ttu-id="6a1d2-181">Öppna den **versioner** för den **skapa &amp; släpper** hubben, öppna den  **+**  listrutan i listan över definitioner av versionen och välj  **Skapa en definition för versionen**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-181">Open the **Releases** tab of the **Build &amp; Release** hub, open the **+** drop-down in the list of release definitions, and choose the **Create release definition**.</span></span> 

1. <span data-ttu-id="6a1d2-182">Välj den **tom** mall och välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-182">Select the **Empty** template and choose **Next**.</span></span>

1. <span data-ttu-id="6a1d2-183">I den **artefakter** klickar du på **länka en artefakt** och välj **Jenkins**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-183">In the **Artifacts** section, click on **Link an Artifact** and choose **Jenkins**.</span></span> <span data-ttu-id="6a1d2-184">Välj Jenkins tjänst endpoint-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-184">Select your Jenkins service endpoint connection.</span></span> <span data-ttu-id="6a1d2-185">Sedan väljer du det Jenkins källa och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-185">Then select the Jenkins source job and choose **Create**.</span></span> 

1. <span data-ttu-id="6a1d2-186">I den nya versionen definition, Välj **+ Lägg till aktiviteter** och lägga till en **Azure Resource distribution** aktivitet för standard-miljö.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-186">In the new release definition, choose **+ Add tasks** and add an **Azure Resource Group Deployment** task to the default environment.</span></span>

1. <span data-ttu-id="6a1d2-187">Välj nedrullningsbara pilen bredvid den **+ Lägg till aktiviteter** länkar och Lägg till en grupp fas i distributionen till definition.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-187">Choose the drop down arrow next to the **+ Add tasks** link and add a deployment group phase to the definition.</span></span>

   ![Lägga till en distribution grupp fas](media/tutorial-build-deploy-jenkins/deployment-group-phase-in-release-definition.png) 

1. <span data-ttu-id="6a1d2-189">Öppna i katalogen aktivitet i **Utility** och lägger till en instans av den **kommandoskript** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-189">In the Task catalog, open the **Utility** section and add an instance of the **Shell Script** task.</span></span>

1. <span data-ttu-id="6a1d2-190">Parametrarna-mallen som används i aktiviteten Azure Resource distribution anger administratörslösenordet som används för att ansluta till de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-190">The parameters template used in the Azure Resource Group Deployment task sets the admin password used to connect to the VMs.</span></span>
   <span data-ttu-id="6a1d2-191">Du kan ange lösenord med variabeln **$(adminpassword)**:</span><span class="sxs-lookup"><span data-stu-id="6a1d2-191">You provide this password with the variable **$(adminpassword)**:</span></span>
   
   - <span data-ttu-id="6a1d2-192">Öppna den **variabler** fliken och i den **variabler** ange namnet `adminpassword`.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-192">Open the **Variables** tab and, in the **Variables** section, enter the name `adminpassword`.</span></span>

   - <span data-ttu-id="6a1d2-193">Ange administratörens lösenord.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-193">Enter the administrator password.</span></span>

   - <span data-ttu-id="6a1d2-194">Välj hänglåsikonen ”” bredvid textrutan värdet för att skydda lösenordet.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-194">Choose the "padlock" icon next to the value textbox to protect the password.</span></span> 

## <a name="configure-the-azure-resource-group-deployment-task"></a><span data-ttu-id="6a1d2-195">Konfigurera aktiviteten Azure Resource distribution</span><span class="sxs-lookup"><span data-stu-id="6a1d2-195">Configure the Azure Resource Group Deployment task</span></span>

<span data-ttu-id="6a1d2-196">Den **Azure Resource distribution** aktivitet används för att skapa distributionsgruppen.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-196">The **Azure Resource Group Deployment** task is used to create the deployment group.</span></span> <span data-ttu-id="6a1d2-197">Konfigurera enligt följande:</span><span class="sxs-lookup"><span data-stu-id="6a1d2-197">Configure it as follows:</span></span>

* <span data-ttu-id="6a1d2-198">**Azure-prenumeration:** Välj en anslutning i listan under **tillgängliga Azure Service anslutningar**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-198">**Azure Subscription:** Select a connection from the list under **Available Azure Service Connections**.</span></span> 
  <span data-ttu-id="6a1d2-199">Om inga anslutningar visas väljer du **hantera**väljer **nya tjänstslutpunkten** sedan **Azure Resource Manager**, och följ anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-199">If no connections appear, choose **Manage**, select **New Service Endpoint** then **Azure Resource Manager**, and follow the prompts.</span></span>
  <span data-ttu-id="6a1d2-200">Gå tillbaka till din versionen definition, uppdatera det **AzureRM prenumeration** listan och väljer den anslutning som du skapade.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-200">Return to your release definition, refresh the **AzureRM Subscription** list, and select the connection you created.</span></span>

* <span data-ttu-id="6a1d2-201">**Resursgruppen**: Ange ett namn för resursgruppen som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-201">**Resource group**: Enter a name of the resource group you created earlier.</span></span>

* <span data-ttu-id="6a1d2-202">**Plats**: Välj en region för distributionen.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-202">**Location**: Select a region for the deployment.</span></span>

  ![Skapa en ny resursgrupp](media/tutorial-build-deploy-jenkins/provision-web-server.png)

* <span data-ttu-id="6a1d2-204">**Platsen mall**:`URL of the file`</span><span class="sxs-lookup"><span data-stu-id="6a1d2-204">**Template location**: `URL of the file`</span></span>

* <span data-ttu-id="6a1d2-205">**Mallen länken**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.json`</span><span class="sxs-lookup"><span data-stu-id="6a1d2-205">**Template link**: `{your-git-repo}/ARM-Templates/UbuntuWeb1.json`</span></span>

* <span data-ttu-id="6a1d2-206">**Mallen parametrar länken**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`</span><span class="sxs-lookup"><span data-stu-id="6a1d2-206">**Template parameters link**: `{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`</span></span>

* <span data-ttu-id="6a1d2-207">**Åsidosätt mallparametrar**: en lista över åsidosättningen värden, till exempel: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-207">**Override template parameters**: A list of the override values, for example: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.</span></span><br /><span data-ttu-id="6a1d2-208">Lägga till egna specifika värden för {platshållare}.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-208">Insert your own specific values for the {placeholders}.</span></span> 

* <span data-ttu-id="6a1d2-209">**Aktivera krav**:`Configure with Deployment Group agent`</span><span class="sxs-lookup"><span data-stu-id="6a1d2-209">**Enable prerequisites**: `Configure with Deployment Group agent`</span></span>

* <span data-ttu-id="6a1d2-210">**TFS/VSTS endpoint**: Välj **Lägg till** och i dialogrutan ”Lägg till ny Team Foundation Server-teamets Services anslutning” Välj **Token för autentisering**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-210">**TFS/VSTS endpoint**: Choose **Add** and, in the "Add new Team Foundation Server/Team Services Connection" dialog, select **Token Based Authentication**.</span></span> <span data-ttu-id="6a1d2-211">Ange ett namn för anslutningen och Webbadressen för ditt team projekt.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-211">Enter a name of the connection and the URL of your team project.</span></span> <span data-ttu-id="6a1d2-212">Generera och ange sedan en [personlig åtkomst-Token (PATRIK)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) att autentisera anslutningen till team projekt.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-212">Then generate and enter a [Personal Access Token (PAT)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) to authenticate the connection to your team project.</span></span>

  ![Skapa en personlig åtkomsttoken](media/tutorial-build-deploy-jenkins/create-a-pat.png)

* <span data-ttu-id="6a1d2-214">**Grupprojekt**: Välj det aktuella projektet.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-214">**Team project**: Select your current project.</span></span>

* <span data-ttu-id="6a1d2-215">**Distributionsgruppen**: Ange namnet på samma distribution som du använde i den **resursgruppen** parameter.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-215">**Deployment Group**: Enter the same deployment group name as you used for the **Resource group** parameter.</span></span>

<span data-ttu-id="6a1d2-216">Standardinställningarna för aktiviteten Azure Resource distribution är att skapa eller uppdatera en resurs och gör inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-216">The default settings for the Azure Resource Group Deployment task are to create or update a resource, and to do so incrementally.</span></span> <span data-ttu-id="6a1d2-217">Uppgiften skapar de virtuella datorerna första gången körs, och därefter bara uppdatera dem.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-217">The task creates the VMs the first time it runs, and subsequently just update them.</span></span>

## <a name="configure-the-shell-script-task"></a><span data-ttu-id="6a1d2-218">Konfigurera aktiviteten Shell-skript</span><span class="sxs-lookup"><span data-stu-id="6a1d2-218">Configure the Shell Script task</span></span>

<span data-ttu-id="6a1d2-219">Den **kommandoskript** aktivitet används för att ange konfigurationen för ett skript köras på varje server för att installera Node.js och starta appen.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-219">The **Shell Script** task is used to provide the configuration for a script to run on each server to install Node.js and start the app.</span></span> <span data-ttu-id="6a1d2-220">Konfigurera enligt följande:</span><span class="sxs-lookup"><span data-stu-id="6a1d2-220">Configure it as follows:</span></span>

* <span data-ttu-id="6a1d2-221">**Script sökvägen**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`</span><span class="sxs-lookup"><span data-stu-id="6a1d2-221">**Script Path**: `$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`</span></span>

* <span data-ttu-id="6a1d2-222">**Ange fungerande Directory**:`Checked`</span><span class="sxs-lookup"><span data-stu-id="6a1d2-222">**Specify Working Directory**: `Checked`</span></span>

* <span data-ttu-id="6a1d2-223">**Arbetskatalog**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`</span><span class="sxs-lookup"><span data-stu-id="6a1d2-223">**Working Directory**: `$(System.DefaultWorkingDirectory)/Fabrikam-Node`</span></span>
   
## <a name="rename-and-save-the-release-definition"></a><span data-ttu-id="6a1d2-224">Byt namn på och spara versionen definition</span><span class="sxs-lookup"><span data-stu-id="6a1d2-224">Rename and save the release definition</span></span>

1. <span data-ttu-id="6a1d2-225">Redigera namnet på definitionen versionen till det namn du angav i den **efter build åtgärder** för versionen i Jenkins.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-225">Edit the name of the release definition to the name you specified in the **Post-build Actions** tab of the build in Jenkins.</span></span> <span data-ttu-id="6a1d2-226">Jenkins kräver det här namnet för att kunna utlösa en ny version när artefakter källa har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-226">Jenkins requires this name to be able to trigger a new release when the source artifacts are updated.</span></span>

1. <span data-ttu-id="6a1d2-227">Du kan också ändra namnet på miljön genom att klicka på namnet.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-227">Optionally, change the name of the environment by clicking on the name.</span></span> 

1. <span data-ttu-id="6a1d2-228">Välj **spara**, och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-228">Choose **Save**, and choose **OK**.</span></span>

## <a name="start-a-manual-deployment"></a><span data-ttu-id="6a1d2-229">Starta en manuell distribution</span><span class="sxs-lookup"><span data-stu-id="6a1d2-229">Start a manual deployment</span></span>

1. <span data-ttu-id="6a1d2-230">Välj **+ versionen** och välj **skapa versionen**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-230">Choose **+ Release** and select **Create Release**.</span></span>

1. <span data-ttu-id="6a1d2-231">Välj build du slutfört i den markerade listrutan och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-231">Select the build you completed in the highlighted drop-down list and choose **Create**.</span></span>

1. <span data-ttu-id="6a1d2-232">Välj länken versionen i popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-232">Choose the release link in the popup message.</span></span> <span data-ttu-id="6a1d2-233">Till exempel ”: versionen **Release 1** har skapats”.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-233">For example: "Release **Release-1** has been created."</span></span>

1. <span data-ttu-id="6a1d2-234">Öppna den **loggar** fliken kan du titta på utmatningen versionen.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-234">Open the **Logs** tab to watch the release console output.</span></span>

1. <span data-ttu-id="6a1d2-235">Öppna URL-Adressen till en av de servrar som du har lagt till i gruppen för distribution i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-235">In your browser, open the URL of one of the servers you added to your deployment group.</span></span> <span data-ttu-id="6a1d2-236">Ange till exempel`http://{your-server-ip-address}`</span><span class="sxs-lookup"><span data-stu-id="6a1d2-236">For example, enter `http://{your-server-ip-address}`</span></span>

## <a name="start-a-cicd-deployment"></a><span data-ttu-id="6a1d2-237">Starta en CI/CD-distribution</span><span class="sxs-lookup"><span data-stu-id="6a1d2-237">Start a CI/CD deployment</span></span>

1. <span data-ttu-id="6a1d2-238">I definitionen versionen avmarkera den **aktiverad** kryssrutan i den **alternativ för åtkomstkontroll** i inställningarna för distribution av Azure Resource grupp-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-238">In the release definition, uncheck the **Enabled** checkbox in the **Control Options** section of the settings for the Azure Resource Group Deployment task.</span></span>
   <span data-ttu-id="6a1d2-239">För framtida distributioner i den befintliga distributionsgruppen behöver inte köra aktiviteten igen.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-239">For future deployments to the existing deployment group, you do not need to re-execute this task.</span></span>

1. <span data-ttu-id="6a1d2-240">Gå till Git-lagerkälla och ändra innehållet i den **h1** rubriken i filen [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).</span><span class="sxs-lookup"><span data-stu-id="6a1d2-240">Go to the source Git repository and modify the contents of the **h1** heading in the file [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).</span></span>

1. <span data-ttu-id="6a1d2-241">Genomför ändringarna.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-241">Commit your change.</span></span>

1. <span data-ttu-id="6a1d2-242">Efter några minuter ser du en ny utgåva som skapats i den **versioner** i Team Services eller TFS.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-242">After a few minutes, you will see a new release created in the **Releases** page of Team Services or TFS.</span></span> <span data-ttu-id="6a1d2-243">Öppna versionen om du vill se distributionen äger rum.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-243">Open the release to see the deployment taking place.</span></span> <span data-ttu-id="6a1d2-244">Grattis!</span><span class="sxs-lookup"><span data-stu-id="6a1d2-244">Congratulations!</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a1d2-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6a1d2-245">Next Steps</span></span>

<span data-ttu-id="6a1d2-246">I den här självstudiekursen automatiserad distribution av en app på Azure med hjälp av Jenkins bygg- och Team Services för versionen.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-246">In this tutorial, you automated the deployment of an app to Azure using Jenkins build and Team Services for release.</span></span> <span data-ttu-id="6a1d2-247">Du har lärt dig hur till:</span><span class="sxs-lookup"><span data-stu-id="6a1d2-247">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6a1d2-248">Skapa din app i Jenkins</span><span class="sxs-lookup"><span data-stu-id="6a1d2-248">Build your app in Jenkins</span></span>
> * <span data-ttu-id="6a1d2-249">Konfigurera Jenkins för integrering med Team Services</span><span class="sxs-lookup"><span data-stu-id="6a1d2-249">Configure Jenkins for Team Services integration</span></span>
> * <span data-ttu-id="6a1d2-250">Skapa en distributionsgrupp för de virtuella Azure-datorerna</span><span class="sxs-lookup"><span data-stu-id="6a1d2-250">Create a deployment group for the Azure virtual machines</span></span>
> * <span data-ttu-id="6a1d2-251">Skapa en definition av versionen som konfigurerar de virtuella datorerna och distribuerar appen</span><span class="sxs-lookup"><span data-stu-id="6a1d2-251">Create a release definition that configures the VMs and deploys the app</span></span>

<span data-ttu-id="6a1d2-252">Gå till nästa kurs vill veta mer om hur du distribuerar ett ljus (Linux, Apache, MySQL och PHP) stacken.</span><span class="sxs-lookup"><span data-stu-id="6a1d2-252">Advance to the next tutorial to learn more about how to deploy a LAMP (Linux, Apache, MySQL, and PHP) stack.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6a1d2-253">Distribuera LAMP-stack</span><span class="sxs-lookup"><span data-stu-id="6a1d2-253">Deploy LAMP stack</span></span>](tutorial-lamp-stack.md)