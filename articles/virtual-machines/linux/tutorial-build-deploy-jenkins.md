---
title: "aaaCI/CD från Jenkins tooAzure virtuella datorer med Team Services | Microsoft Docs"
description: "Ställ in kontinuerlig integration (KO) och kontinuerlig distribution (CD) av en Node.js-app med Jenkins tooAzure virtuella datorer från versionshantering i Visual Studio Team Services VSTS () eller Microsoft Team Foundation Server (TFS)"
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
ms.openlocfilehash: 400ae34cbdf45da65351811c0ff6ff5d61ef862c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-toolinux-vms-using-jenkins-and-team-services"></a><span data-ttu-id="ae594-103">Distribuera din app tooLinux virtuella datorer med hjälp av Jenkins och Team Services</span><span class="sxs-lookup"><span data-stu-id="ae594-103">Deploy your app tooLinux VMs using Jenkins and Team Services</span></span>

<span data-ttu-id="ae594-104">Kontinuerlig integration (KO) och kontinuerlig distribution (CD) är en pipeline som du kan bygga, släppa och distribuera din kod.</span><span class="sxs-lookup"><span data-stu-id="ae594-104">Continuous integration (CI) and continuous deployment (CD) is a pipeline by which you can build, release, and deploy your code.</span></span> <span data-ttu-id="ae594-105">Team Services innehåller en fullständig, komplett funktionalitet uppsättning av verktyg för automatisering av CI/CD för distribution tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ae594-105">Team Services provides a complete, fully featured set of CI/CD automation tools for deployment tooAzure.</span></span> <span data-ttu-id="ae594-106">Jenkins är ett populära 3 parts CI/CD-server-baserade verktyg som också ger CI/CD-automatisering.</span><span class="sxs-lookup"><span data-stu-id="ae594-106">Jenkins is a popular 3rd-party CI/CD server-based tool that also provides CI/CD automation.</span></span> <span data-ttu-id="ae594-107">Du kan använda båda tillsammans toocustomize hur du levererar ditt molnapp eller tjänst.</span><span class="sxs-lookup"><span data-stu-id="ae594-107">You can use both together toocustomize how you deliver your cloud app or service.</span></span>

<span data-ttu-id="ae594-108">I den här kursen använder du Jenkins toobuild en **Node.js-webbapp**, och Visual Studio Team Services toodeploy den tooa [distributionsgruppen](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) som innehåller virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="ae594-108">In this tutorial, you use Jenkins toobuild a **Node.js web app**, and Visual Studio Team Services toodeploy it tooa [deployment group](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) containing Linux virtual machines.</span></span>

<span data-ttu-id="ae594-109">Du kommer att:</span><span class="sxs-lookup"><span data-stu-id="ae594-109">You will:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ae594-110">Skapa din app i Jenkins</span><span class="sxs-lookup"><span data-stu-id="ae594-110">Build your app in Jenkins</span></span>
> * <span data-ttu-id="ae594-111">Konfigurera Jenkins för integrering med Team Services</span><span class="sxs-lookup"><span data-stu-id="ae594-111">Configure Jenkins for Team Services integration</span></span>
> * <span data-ttu-id="ae594-112">Skapa en distributionsgrupp för hello Azure virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="ae594-112">Create a deployment group for hello Azure virtual machines</span></span>
> * <span data-ttu-id="ae594-113">Skapa en definition av versionen som konfigurerar hello virtuella datorer och distribuerar hello app</span><span class="sxs-lookup"><span data-stu-id="ae594-113">Create a release definition that configures hello VMs and deploys hello app</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ae594-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="ae594-114">Before you begin</span></span>

* <span data-ttu-id="ae594-115">Du behöver åtkomst tooa Jenkins konto.</span><span class="sxs-lookup"><span data-stu-id="ae594-115">You need access tooa Jenkins account.</span></span> <span data-ttu-id="ae594-116">Om du inte har skapat en Jenkins server, se [Jenkins dokumentationen](https://jenkins.io/doc/).</span><span class="sxs-lookup"><span data-stu-id="ae594-116">If you have not yet created a Jenkins server, see [Jenkins Documentation](https://jenkins.io/doc/).</span></span> 

* <span data-ttu-id="ae594-117">Logga in tooyour Team Services-konto (`https://{youraccount}.visualstudio.com`).</span><span class="sxs-lookup"><span data-stu-id="ae594-117">Sign in tooyour Team Services account (`https://{youraccount}.visualstudio.com`).</span></span> 
  <span data-ttu-id="ae594-118">Du kan få en [kostnadsfritt konto Team Services](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).</span><span class="sxs-lookup"><span data-stu-id="ae594-118">You can get a [free Team Services account](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).</span></span>

  > [!NOTE]
  > <span data-ttu-id="ae594-119">Mer information finns i [ansluta tooTeam Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).</span><span class="sxs-lookup"><span data-stu-id="ae594-119">For more information, see [Connect tooTeam Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).</span></span>

* <span data-ttu-id="ae594-120">Skapa en personlig åtkomsttoken (PATRIK) i ditt Team Services-konto om du inte redan har ett.</span><span class="sxs-lookup"><span data-stu-id="ae594-120">Create a personal access token (PAT) in your Team Services account if you don't already have one.</span></span> <span data-ttu-id="ae594-121">Jenkins kräver den här informationen tooaccess ditt Team Services-konto.</span><span class="sxs-lookup"><span data-stu-id="ae594-121">Jenkins requires this information tooaccess your Team Services account.</span></span>
  <span data-ttu-id="ae594-122">Läs [hur skapar jag en personlig åtkomsttoken för Team Services och TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) toolearn hur toogenerate en.</span><span class="sxs-lookup"><span data-stu-id="ae594-122">Read [How do I create a personal access token for Team Services and TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) toolearn how toogenerate one.</span></span>

## <a name="get-hello-sample-app"></a><span data-ttu-id="ae594-123">Hämta hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="ae594-123">Get hello sample app</span></span>

<span data-ttu-id="ae594-124">Du behöver en app toodeploy lagras i en Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="ae594-124">You need an app toodeploy stored in a Git repository.</span></span>
<span data-ttu-id="ae594-125">Den här kursen rekommenderar vi du använder [denna sample-appen som är tillgängliga från GitHub](https://github.com/azooinmyluggage/fabrikam-node).</span><span class="sxs-lookup"><span data-stu-id="ae594-125">For this tutorial, we recommend you use [this sample app available from GitHub](https://github.com/azooinmyluggage/fabrikam-node).</span></span>

1. <span data-ttu-id="ae594-126">Skapa en duplicering av den här appen och anteckna hello plats (URL) för användning i senare steg i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="ae594-126">Create a fork of this app and take note of hello location (URL) for use in later steps of this tutorial.</span></span>

1. <span data-ttu-id="ae594-127">Se hello förgrening **offentliga** toosimplify ansluta tooGitHub senare.</span><span class="sxs-lookup"><span data-stu-id="ae594-127">Make hello fork **public** toosimplify connecting tooGitHub later.</span></span>

> [!NOTE]
> <span data-ttu-id="ae594-128">Mer information finns i [duplicera en repo](https://help.github.com/articles/fork-a-repo/) och [gör en privat databas offentliga](https://help.github.com/articles/making-a-private-repository-public/).</span><span class="sxs-lookup"><span data-stu-id="ae594-128">For more information, see [Fork a repo](https://help.github.com/articles/fork-a-repo/) and [Making a private repository public](https://help.github.com/articles/making-a-private-repository-public/).</span></span>

> [!NOTE]
> <span data-ttu-id="ae594-129">hello app har skapats med [Yeoman](http://yeoman.io/learning/index.html); används **Express**, **bower**, och **grunt**; och har några **npm**paket som beroenden.</span><span class="sxs-lookup"><span data-stu-id="ae594-129">hello app was built using [Yeoman](http://yeoman.io/learning/index.html); it uses **Express**, **bower**, and **grunt**; and it has some **npm** packages as dependencies.</span></span>
> <span data-ttu-id="ae594-130">Hej exempelapp innehåller en uppsättning [Azure Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) som har använt toodynamically skapar hello virtuella datorer för distribution på Azure.</span><span class="sxs-lookup"><span data-stu-id="ae594-130">hello sample app contains a set of [Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) that are used toodynamically create hello virtual machines for deployment on Azure.</span></span> <span data-ttu-id="ae594-131">Dessa mallar används av uppgifter i hello [Team Services släpper definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).</span><span class="sxs-lookup"><span data-stu-id="ae594-131">These templates are used by tasks in hello [Team Services release definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).</span></span>
> <span data-ttu-id="ae594-132">hello Huvudmall skapar en nätverkssäkerhetsgrupp, en virtuell dator och ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ae594-132">hello main template creates a network security group, a virtual machine, and a virtual network.</span></span> <span data-ttu-id="ae594-133">Den tilldelas en offentlig IP-adress och öppnar inkommande port 80.</span><span class="sxs-lookup"><span data-stu-id="ae594-133">It assigns a public IP address and opens inbound port 80.</span></span> <span data-ttu-id="ae594-134">Det ger också en tagg som används av hello distribution för väljer hello datorer tooreceive hello distribution.</span><span class="sxs-lookup"><span data-stu-id="ae594-134">It also adds a tag that is used by hello deployment group too select hello machines tooreceive hello deployment.</span></span>
>
> <span data-ttu-id="ae594-135">hello exemplet innehåller också ett skript som konfigurerar Nginx och distribuerar hello app.</span><span class="sxs-lookup"><span data-stu-id="ae594-135">hello sample also contains a script that sets up Nginx and deploys hello app.</span></span> <span data-ttu-id="ae594-136">Den körs på varje hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ae594-136">It is executed on each of hello virtual machines.</span></span> <span data-ttu-id="ae594-137">Mer specifikt installerar hello skriptet nod och Nginx PM2; konfigurerar Nginx och PM2; Därefter startar hello nod app.</span><span class="sxs-lookup"><span data-stu-id="ae594-137">Specifically, hello script installs Node, Nginx, and PM2; configures Nginx and PM2; then starts hello Node app.</span></span>

## <a name="configure-jenkins-plugins"></a><span data-ttu-id="ae594-138">Konfigurera Jenkins plugin-program</span><span class="sxs-lookup"><span data-stu-id="ae594-138">Configure Jenkins plugins</span></span>

<span data-ttu-id="ae594-139">Först måste du konfigurera två Jenkins plugin-program för **NodeJS** och **integrering med Team Services**.</span><span class="sxs-lookup"><span data-stu-id="ae594-139">First, you must configure two Jenkins plugins for **NodeJS** and **Integration with Team Services**.</span></span>

1. <span data-ttu-id="ae594-140">Öppna Jenkins-konto och välj **hantera Jenkins**.</span><span class="sxs-lookup"><span data-stu-id="ae594-140">Open your Jenkins account and choose **Manage Jenkins**.</span></span>

1. <span data-ttu-id="ae594-141">I hello **hantera Jenkins** väljer **hantera plugin-program**.</span><span class="sxs-lookup"><span data-stu-id="ae594-141">In hello **Manage Jenkins** page, choose **Manage Plugins**.</span></span>

1. <span data-ttu-id="ae594-142">Filter hello listan toolocate hello **NodeJS** plugin-program och installera den utan omstart.</span><span class="sxs-lookup"><span data-stu-id="ae594-142">Filter hello list toolocate hello **NodeJS** plugin and install it without restart.</span></span>

   ![Lägga till hello NodeJS plugin-programmet tooJenkins](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)

1. <span data-ttu-id="ae594-144">Filter hello listan toofind hello **Team Foundation Server** plugin-program och installera den.</span><span class="sxs-lookup"><span data-stu-id="ae594-144">Filter hello list toofind hello **Team Foundation Server** plugin and install it.</span></span> <span data-ttu-id="ae594-145">(Det här plugin-programmet fungerar för både Team Services och Team Foundation Server.) Det är inte nödvändigt att starta om Jenkins.</span><span class="sxs-lookup"><span data-stu-id="ae594-145">(This plug-in works for both Team Services and Team Foundation Server.) Restarting Jenkins is not necessary.</span></span>

## <a name="configure-jenkins-build-for-nodejs"></a><span data-ttu-id="ae594-146">Konfigurera Jenkins build för Node.js</span><span class="sxs-lookup"><span data-stu-id="ae594-146">Configure Jenkins build for Node.js</span></span>

<span data-ttu-id="ae594-147">Skapa ett nytt build-projekt i Jenkins, och konfigurerar det enligt följande:</span><span class="sxs-lookup"><span data-stu-id="ae594-147">In Jenkins, create a new build project and configure it as follows:</span></span>

1. <span data-ttu-id="ae594-148">I hello **allmänna** och anger ett namn för ditt build-projekt.</span><span class="sxs-lookup"><span data-stu-id="ae594-148">In hello **General** tab, enter a name for your build project.</span></span>

1. <span data-ttu-id="ae594-149">I hello **källa kod Management** väljer **Git** och ange hello information om hello databasen och hello gren med Appkod.</span><span class="sxs-lookup"><span data-stu-id="ae594-149">In hello **Source Code Management** tab, select **Git** and enter hello details of hello repository and hello branch containing your app code.</span></span>

   ![Lägg till en lagringsplatsen tooyour version](media/tutorial-build-deploy-jenkins/jenkins-git.png)

   > [!NOTE]
   > <span data-ttu-id="ae594-151">Om databasen inte är offentlig väljer **Lägg till** och ange autentiseringsuppgifter tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="ae594-151">If your repository is not public, choose **Add** and provide credentials tooconnect tooit.</span></span>

1. <span data-ttu-id="ae594-152">I hello **Skapa utlösare** väljer **avsökning SCM** och ange hello schema `H/03 * * * *` toopoll hello Git-lagringsplats för ändringar i alla tre minuter.</span><span class="sxs-lookup"><span data-stu-id="ae594-152">In hello **Build Triggers** tab, select **Poll SCM** and enter hello schedule `H/03 * * * *` toopoll hello Git repository for changes every three minutes.</span></span> 

1. <span data-ttu-id="ae594-153">I hello **kompileringsmiljö** väljer **ange nod &amp; npm bin / mappen SÖKVÄGEN** och ange `NodeJS` för hello Node JS-Installation-värde.</span><span class="sxs-lookup"><span data-stu-id="ae594-153">In hello **Build Environment** tab, select **Provide Node &amp; npm bin/ folder PATH** and enter `NodeJS` for hello Node JS Installation value.</span></span> <span data-ttu-id="ae594-154">Lämna **npmrc filen** inställd på ”Använd systemstandard”.</span><span class="sxs-lookup"><span data-stu-id="ae594-154">Leave **npmrc file** set to "use system default."</span></span>

1. <span data-ttu-id="ae594-155">I hello **skapa** ange hello kommandot `npm install` tooensure alla beroenden är uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="ae594-155">In hello **Build** tab, enter hello command `npm install` tooensure all dependencies are updated.</span></span>

## <a name="configure-jenkins-for-team-services-integration"></a><span data-ttu-id="ae594-156">Konfigurera Jenkins för integrering med Team Services</span><span class="sxs-lookup"><span data-stu-id="ae594-156">Configure Jenkins for Team Services integration</span></span>

1. <span data-ttu-id="ae594-157">I hello **efter build åtgärder** fliken för **filer tooarchive**, ange `**/*` tooinclude alla filer.</span><span class="sxs-lookup"><span data-stu-id="ae594-157">In hello **Post-build Actions** tab, for **Files tooarchive**, enter `**/*` tooinclude all files.</span></span>

1. <span data-ttu-id="ae594-158">För **utlösa version i TFS/Team Services**, ange hello fullständiga URL: en för ditt konto (som `https://your-account-name.visualstudio.com`), hello projektnamnet, ett namn för hello versionen definition (skapas senare), och hello autentiseringsuppgifter tooconnect tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="ae594-158">For **Trigger release in TFS/Team Services**, enter hello full URL of your account (such as `https://your-account-name.visualstudio.com`), hello project name, a name for hello release definition (created later), and hello credentials tooconnect tooyour account.</span></span>
   <span data-ttu-id="ae594-159">Du behöver ditt användarnamn och hello PATRIK som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ae594-159">You need your user name and hello PAT you created earlier.</span></span> 

   ![Konfigurera Jenkins efter build-åtgärder](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)

1. <span data-ttu-id="ae594-161">Spara hello build-projekt.</span><span class="sxs-lookup"><span data-stu-id="ae594-161">Save hello build project.</span></span>

## <a name="create-a-jenkins-service-endpoint"></a><span data-ttu-id="ae594-162">Skapa en Jenkins tjänstslutpunkt</span><span class="sxs-lookup"><span data-stu-id="ae594-162">Create a Jenkins service endpoint</span></span>

<span data-ttu-id="ae594-163">En tjänstslutpunkt kan Team Services tooconnect tooJenkins.</span><span class="sxs-lookup"><span data-stu-id="ae594-163">A service endpoint allows Team Services tooconnect tooJenkins.</span></span>

1. <span data-ttu-id="ae594-164">Öppna hello **Services** i Team Services, öppna hello **nya tjänstslutpunkten** listan, och välj **Jenkins**.</span><span class="sxs-lookup"><span data-stu-id="ae594-164">Open hello **Services** page in Team Services, open hello **New Service Endpoint** list, and choose **Jenkins**.</span></span>

   ![Lägg till en Jenkins slutpunkt](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)

1. <span data-ttu-id="ae594-166">Ange ett namn som du ska använda toorefer toothis anslutningen.</span><span class="sxs-lookup"><span data-stu-id="ae594-166">Enter a name you will use toorefer toothis connection.</span></span>

1. <span data-ttu-id="ae594-167">Ange hello URL-Adressen till din Jenkins och skalstreck hello **accepterar ej betrodda certifikat för SSL** alternativet.</span><span class="sxs-lookup"><span data-stu-id="ae594-167">Enter hello URL of your Jenkins server, and tick hello **Accept untrusted SSL certificates** option.</span></span>

1. <span data-ttu-id="ae594-168">Ange hello användarnamn och lösenord för kontot för Jenkins.</span><span class="sxs-lookup"><span data-stu-id="ae594-168">Enter hello user name and password for your Jenkins account.</span></span>

1. <span data-ttu-id="ae594-169">Välj **verifiera anslutning** toocheck som hello information är korrekt.</span><span class="sxs-lookup"><span data-stu-id="ae594-169">Choose **Verify connection** toocheck that hello information is correct.</span></span>

1. <span data-ttu-id="ae594-170">Välj **OK** toocreate hello tjänstslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="ae594-170">Choose **OK** toocreate hello service endpoint.</span></span>

## <a name="create-a-deployment-group"></a><span data-ttu-id="ae594-171">Skapa en distributionsgrupp</span><span class="sxs-lookup"><span data-stu-id="ae594-171">Create a deployment group</span></span>

<span data-ttu-id="ae594-172">Du behöver en [distributionsgruppen](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) för innehålla hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ae594-172">You need a [deployment group](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) too contain hello virtual machines.</span></span>

1. <span data-ttu-id="ae594-173">Öppna hello **versioner** för hello **skapa &amp; versionen** hubb och sedan öppna hello **distributionsgrupper** , och välj **+ ny**.</span><span class="sxs-lookup"><span data-stu-id="ae594-173">Open hello **Releases** tab of hello **Build &amp; Release** hub, then open hello **Deployment groups** tab, and choose **+ New**.</span></span>

1. <span data-ttu-id="ae594-174">Ange ett namn för hello distributionsgruppen och en valfri beskrivning.</span><span class="sxs-lookup"><span data-stu-id="ae594-174">Enter a name for hello deployment group, and an optional description.</span></span>
   <span data-ttu-id="ae594-175">Välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="ae594-175">Then choose **Create**.</span></span>

<span data-ttu-id="ae594-176">hello Azure Resource distribution aktivitet skapas och registrerar hello VMs när den körs med hello Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="ae594-176">hello Azure Resource Group Deployment task creates and registers hello VMs when it runs using hello Azure Resource Manager template.</span></span>
<span data-ttu-id="ae594-177">Du inte behöver toocreate och registrera hello virtuella datorer själv.</span><span class="sxs-lookup"><span data-stu-id="ae594-177">You don't need toocreate and register hello virtual machines yourself.</span></span>

## <a name="create-a-release-definition"></a><span data-ttu-id="ae594-178">Skapa en definition för versionen</span><span class="sxs-lookup"><span data-stu-id="ae594-178">Create a release definition</span></span>

<span data-ttu-id="ae594-179">En definition av versionen anger hello processen Team Services körs toodeploy hello app.</span><span class="sxs-lookup"><span data-stu-id="ae594-179">A release definition specifies hello process Team Services will execute toodeploy hello app.</span></span>
<span data-ttu-id="ae594-180">toocreate hello versionen definition i Team Services:</span><span class="sxs-lookup"><span data-stu-id="ae594-180">toocreate hello release definition in Team Services:</span></span>

1. <span data-ttu-id="ae594-181">Öppna hello **versioner** för hello **skapa &amp; släpper** NAV, öppna hello  **+**  listrutan i hello listan över definitioner av versionen och välj Hej **skapa versionen definition**.</span><span class="sxs-lookup"><span data-stu-id="ae594-181">Open hello **Releases** tab of hello **Build &amp; Release** hub, open hello **+** drop-down in hello list of release definitions, and choose hello **Create release definition**.</span></span> 

1. <span data-ttu-id="ae594-182">Välj hello **tom** mall och välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ae594-182">Select hello **Empty** template and choose **Next**.</span></span>

1. <span data-ttu-id="ae594-183">I hello **artefakter** klickar du på **länka en artefakt** och välj **Jenkins**.</span><span class="sxs-lookup"><span data-stu-id="ae594-183">In hello **Artifacts** section, click on **Link an Artifact** and choose **Jenkins**.</span></span> <span data-ttu-id="ae594-184">Välj Jenkins tjänst endpoint-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="ae594-184">Select your Jenkins service endpoint connection.</span></span> <span data-ttu-id="ae594-185">Sedan hello Jenkins källa jobb och **skapa**.</span><span class="sxs-lookup"><span data-stu-id="ae594-185">Then select hello Jenkins source job and choose **Create**.</span></span> 

1. <span data-ttu-id="ae594-186">I nya hello släpper definition, Välj **+ Lägg till aktiviteter** och lägga till en **Azure Resource distribution** aktivitet toohello standardmiljö.</span><span class="sxs-lookup"><span data-stu-id="ae594-186">In hello new release definition, choose **+ Add tasks** and add an **Azure Resource Group Deployment** task toohello default environment.</span></span>

1. <span data-ttu-id="ae594-187">Välj hello nedrullningsbara pilen Nästa toohello **+ Lägg till aktiviteter** länkar och Lägg till en grupp fas toohello distributionsdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="ae594-187">Choose hello drop down arrow next toohello **+ Add tasks** link and add a deployment group phase toohello definition.</span></span>

   ![Lägga till en distribution grupp fas](media/tutorial-build-deploy-jenkins/deployment-group-phase-in-release-definition.png) 

1. <span data-ttu-id="ae594-189">Öppna hello i hello uppgiften katalogen **Utility** och lägger till en instans av hello **kommandoskript** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="ae594-189">In hello Task catalog, open hello **Utility** section and add an instance of hello **Shell Script** task.</span></span>

1. <span data-ttu-id="ae594-190">hello parametrar mall som används i hello Azure Resource distribution uppgiften anger hello admin lösenord som används för tooconnect toohello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ae594-190">hello parameters template used in hello Azure Resource Group Deployment task sets hello admin password used tooconnect toohello VMs.</span></span>
   <span data-ttu-id="ae594-191">Du ge det här lösenordet hello variabeln **$(adminpassword)**:</span><span class="sxs-lookup"><span data-stu-id="ae594-191">You provide this password with hello variable **$(adminpassword)**:</span></span>
   
   - <span data-ttu-id="ae594-192">Öppna hello **variabler** fliken och i hello **variabler** ange hello namnet `adminpassword`.</span><span class="sxs-lookup"><span data-stu-id="ae594-192">Open hello **Variables** tab and, in hello **Variables** section, enter hello name `adminpassword`.</span></span>

   - <span data-ttu-id="ae594-193">Ange hello administratörslösenord.</span><span class="sxs-lookup"><span data-stu-id="ae594-193">Enter hello administrator password.</span></span>

   - <span data-ttu-id="ae594-194">Välj hello ”hänglås” ikonen nästa toohello värdet textruta tooprotect hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="ae594-194">Choose hello "padlock" icon next toohello value textbox tooprotect hello password.</span></span> 

## <a name="configure-hello-azure-resource-group-deployment-task"></a><span data-ttu-id="ae594-195">Konfigurera hello Azure Resource distribution aktivitet</span><span class="sxs-lookup"><span data-stu-id="ae594-195">Configure hello Azure Resource Group Deployment task</span></span>

<span data-ttu-id="ae594-196">Hej **Azure Resource distribution** uppgift är används toocreate hello distributionsgruppen.</span><span class="sxs-lookup"><span data-stu-id="ae594-196">hello **Azure Resource Group Deployment** task is used toocreate hello deployment group.</span></span> <span data-ttu-id="ae594-197">Konfigurera enligt följande:</span><span class="sxs-lookup"><span data-stu-id="ae594-197">Configure it as follows:</span></span>

* <span data-ttu-id="ae594-198">**Azure-prenumeration:** Välj en anslutning hello listan under **tillgängliga Azure Service anslutningar**.</span><span class="sxs-lookup"><span data-stu-id="ae594-198">**Azure Subscription:** Select a connection from hello list under **Available Azure Service Connections**.</span></span> 
  <span data-ttu-id="ae594-199">Om inga anslutningar visas väljer du **hantera**väljer **nya tjänstslutpunkten** sedan **Azure Resource Manager**, och följ anvisningarna för hello.</span><span class="sxs-lookup"><span data-stu-id="ae594-199">If no connections appear, choose **Manage**, select **New Service Endpoint** then **Azure Resource Manager**, and follow hello prompts.</span></span>
  <span data-ttu-id="ae594-200">Returnera tooyour versionen definition, uppdatera hello **AzureRM prenumeration** listan och väljer hello-anslutning som du skapade.</span><span class="sxs-lookup"><span data-stu-id="ae594-200">Return tooyour release definition, refresh hello **AzureRM Subscription** list, and select hello connection you created.</span></span>

* <span data-ttu-id="ae594-201">**Resursgruppen**: Ange ett namn på hello resursgrupp som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ae594-201">**Resource group**: Enter a name of hello resource group you created earlier.</span></span>

* <span data-ttu-id="ae594-202">**Plats**: Välj en region för hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="ae594-202">**Location**: Select a region for hello deployment.</span></span>

  ![Skapa en ny resursgrupp](media/tutorial-build-deploy-jenkins/provision-web-server.png)

* <span data-ttu-id="ae594-204">**Platsen mall**:`URL of hello file`</span><span class="sxs-lookup"><span data-stu-id="ae594-204">**Template location**: `URL of hello file`</span></span>

* <span data-ttu-id="ae594-205">**Mallen länken**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.json`</span><span class="sxs-lookup"><span data-stu-id="ae594-205">**Template link**: `{your-git-repo}/ARM-Templates/UbuntuWeb1.json`</span></span>

* <span data-ttu-id="ae594-206">**Mallen parametrar länken**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`</span><span class="sxs-lookup"><span data-stu-id="ae594-206">**Template parameters link**: `{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`</span></span>

* <span data-ttu-id="ae594-207">**Åsidosätt mallparametrar**: en lista över hello åsidosätta värden, till exempel: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.</span><span class="sxs-lookup"><span data-stu-id="ae594-207">**Override template parameters**: A list of hello override values, for example: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.</span></span><br /><span data-ttu-id="ae594-208">Infoga egna specifika värden för hello {platshållare}.</span><span class="sxs-lookup"><span data-stu-id="ae594-208">Insert your own specific values for hello {placeholders}.</span></span> 

* <span data-ttu-id="ae594-209">**Aktivera krav**:`Configure with Deployment Group agent`</span><span class="sxs-lookup"><span data-stu-id="ae594-209">**Enable prerequisites**: `Configure with Deployment Group agent`</span></span>

* <span data-ttu-id="ae594-210">**TFS/VSTS endpoint**: Välj **Lägg till** i hello ”Lägg till ny Team Foundation Server-teamets Services anslutning” dialogrutan Välj **Token för autentisering**.</span><span class="sxs-lookup"><span data-stu-id="ae594-210">**TFS/VSTS endpoint**: Choose **Add** and, in hello "Add new Team Foundation Server/Team Services Connection" dialog, select **Token Based Authentication**.</span></span> <span data-ttu-id="ae594-211">Ange ett namn för hello anslutningen och hello URL för ditt team projekt.</span><span class="sxs-lookup"><span data-stu-id="ae594-211">Enter a name of hello connection and hello URL of your team project.</span></span> <span data-ttu-id="ae594-212">Generera och ange sedan en [personlig åtkomst-Token (PATRIK)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) tooauthenticate hello anslutning tooyour team projekt.</span><span class="sxs-lookup"><span data-stu-id="ae594-212">Then generate and enter a [Personal Access Token (PAT)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) tooauthenticate hello connection tooyour team project.</span></span>

  ![Skapa en personlig åtkomsttoken](media/tutorial-build-deploy-jenkins/create-a-pat.png)

* <span data-ttu-id="ae594-214">**Grupprojekt**: Välj det aktuella projektet.</span><span class="sxs-lookup"><span data-stu-id="ae594-214">**Team project**: Select your current project.</span></span>

* <span data-ttu-id="ae594-215">**Distributionsgruppen**: Ange hello samma gruppnamn för distribution som du använde för hello **resursgruppen** parameter.</span><span class="sxs-lookup"><span data-stu-id="ae594-215">**Deployment Group**: Enter hello same deployment group name as you used for hello **Resource group** parameter.</span></span>

<span data-ttu-id="ae594-216">hello standardinställningarna för hello Azure Resource distribution aktivitet är toocreate eller uppdatera en resurs och toodo så inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="ae594-216">hello default settings for hello Azure Resource Group Deployment task are toocreate or update a resource, and toodo so incrementally.</span></span> <span data-ttu-id="ae594-217">hello uppgift skapar hello VMs hello första gången körs, och därefter bara uppdatera dem.</span><span class="sxs-lookup"><span data-stu-id="ae594-217">hello task creates hello VMs hello first time it runs, and subsequently just update them.</span></span>

## <a name="configure-hello-shell-script-task"></a><span data-ttu-id="ae594-218">Konfigurera hello kommandoskript aktivitet</span><span class="sxs-lookup"><span data-stu-id="ae594-218">Configure hello Shell Script task</span></span>

<span data-ttu-id="ae594-219">Hej **kommandoskript** uppgift är används tooprovide hello konfiguration för ett skript toorun på varje server tooinstall Node.js och starta hello app.</span><span class="sxs-lookup"><span data-stu-id="ae594-219">hello **Shell Script** task is used tooprovide hello configuration for a script toorun on each server tooinstall Node.js and start hello app.</span></span> <span data-ttu-id="ae594-220">Konfigurera enligt följande:</span><span class="sxs-lookup"><span data-stu-id="ae594-220">Configure it as follows:</span></span>

* <span data-ttu-id="ae594-221">**Script sökvägen**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`</span><span class="sxs-lookup"><span data-stu-id="ae594-221">**Script Path**: `$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`</span></span>

* <span data-ttu-id="ae594-222">**Ange fungerande Directory**:`Checked`</span><span class="sxs-lookup"><span data-stu-id="ae594-222">**Specify Working Directory**: `Checked`</span></span>

* <span data-ttu-id="ae594-223">**Arbetskatalog**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`</span><span class="sxs-lookup"><span data-stu-id="ae594-223">**Working Directory**: `$(System.DefaultWorkingDirectory)/Fabrikam-Node`</span></span>
   
## <a name="rename-and-save-hello-release-definition"></a><span data-ttu-id="ae594-224">Byt namn på och spara hello versionen definition</span><span class="sxs-lookup"><span data-stu-id="ae594-224">Rename and save hello release definition</span></span>

1. <span data-ttu-id="ae594-225">Redigera hello namn hello versionen toohello namnet på definitionen av du angav i den **efter build åtgärder** fliken hello build i Jenkins.</span><span class="sxs-lookup"><span data-stu-id="ae594-225">Edit hello name of hello release definition toohello name you specified in the **Post-build Actions** tab of hello build in Jenkins.</span></span> <span data-ttu-id="ae594-226">Jenkins kräver det här namnet toobe kan tootrigger en ny version när hello källa artefakter uppdateras.</span><span class="sxs-lookup"><span data-stu-id="ae594-226">Jenkins requires this name toobe able tootrigger a new release when hello source artifacts are updated.</span></span>

1. <span data-ttu-id="ae594-227">Du kan också ändra hello namnet på hello miljö genom att klicka på hello namn.</span><span class="sxs-lookup"><span data-stu-id="ae594-227">Optionally, change hello name of hello environment by clicking on hello name.</span></span> 

1. <span data-ttu-id="ae594-228">Välj **spara**, och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="ae594-228">Choose **Save**, and choose **OK**.</span></span>

## <a name="start-a-manual-deployment"></a><span data-ttu-id="ae594-229">Starta en manuell distribution</span><span class="sxs-lookup"><span data-stu-id="ae594-229">Start a manual deployment</span></span>

1. <span data-ttu-id="ae594-230">Välj **+ versionen** och välj **skapa versionen**.</span><span class="sxs-lookup"><span data-stu-id="ae594-230">Choose **+ Release** and select **Create Release**.</span></span>

1. <span data-ttu-id="ae594-231">Välj hello build du slutfört i hello markerade nedrullningsbara listrutan och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="ae594-231">Select hello build you completed in hello highlighted drop-down list and choose **Create**.</span></span>

1. <span data-ttu-id="ae594-232">Välj hello versionen länk i hello popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="ae594-232">Choose hello release link in hello popup message.</span></span> <span data-ttu-id="ae594-233">Till exempel ”: versionen **Release 1** har skapats”.</span><span class="sxs-lookup"><span data-stu-id="ae594-233">For example: "Release **Release-1** has been created."</span></span>

1. <span data-ttu-id="ae594-234">Öppna hello **loggar** fliken toowatch hello versionen konsolens utdata.</span><span class="sxs-lookup"><span data-stu-id="ae594-234">Open hello **Logs** tab toowatch hello release console output.</span></span>

1. <span data-ttu-id="ae594-235">Öppna hello URL i webbläsaren och en av hello servrar du har lagt till tooyour distributionsgruppen.</span><span class="sxs-lookup"><span data-stu-id="ae594-235">In your browser, open hello URL of one of hello servers you added tooyour deployment group.</span></span> <span data-ttu-id="ae594-236">Ange till exempel`http://{your-server-ip-address}`</span><span class="sxs-lookup"><span data-stu-id="ae594-236">For example, enter `http://{your-server-ip-address}`</span></span>

## <a name="start-a-cicd-deployment"></a><span data-ttu-id="ae594-237">Starta en CI/CD-distribution</span><span class="sxs-lookup"><span data-stu-id="ae594-237">Start a CI/CD deployment</span></span>

1. <span data-ttu-id="ae594-238">I hello versionen definition, avmarkerar hello **aktiverad** kryssrutan i hello **alternativ för åtkomstkontroll** hello inställningarna för hello Azure Resource distribution aktivitet.</span><span class="sxs-lookup"><span data-stu-id="ae594-238">In hello release definition, uncheck hello **Enabled** checkbox in hello **Control Options** section of hello settings for hello Azure Resource Group Deployment task.</span></span>
   <span data-ttu-id="ae594-239">För framtida distributioner toohello befintliga distributionsgruppen, behöver inte toore-köra den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="ae594-239">For future deployments toohello existing deployment group, you do not need toore-execute this task.</span></span>

1. <span data-ttu-id="ae594-240">Gå toohello Git-lagerkälla och ändra hello innehållet i hello **h1** rubriken i hello filen [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).</span><span class="sxs-lookup"><span data-stu-id="ae594-240">Go toohello source Git repository and modify hello contents of hello **h1** heading in hello file [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).</span></span>

1. <span data-ttu-id="ae594-241">Genomför ändringarna.</span><span class="sxs-lookup"><span data-stu-id="ae594-241">Commit your change.</span></span>

1. <span data-ttu-id="ae594-242">Efter några minuter ser du en ny utgåva som skapats i hello **versioner** i Team Services eller TFS.</span><span class="sxs-lookup"><span data-stu-id="ae594-242">After a few minutes, you will see a new release created in hello **Releases** page of Team Services or TFS.</span></span> <span data-ttu-id="ae594-243">Öppna hello versionen toosee hello distribution äger rum.</span><span class="sxs-lookup"><span data-stu-id="ae594-243">Open hello release toosee hello deployment taking place.</span></span> <span data-ttu-id="ae594-244">Grattis!</span><span class="sxs-lookup"><span data-stu-id="ae594-244">Congratulations!</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae594-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ae594-245">Next Steps</span></span>

<span data-ttu-id="ae594-246">I den här självstudiekursen automatiserad hello distribution av en app tooAzure med Jenkins bygg- och Team Services för versionen.</span><span class="sxs-lookup"><span data-stu-id="ae594-246">In this tutorial, you automated hello deployment of an app tooAzure using Jenkins build and Team Services for release.</span></span> <span data-ttu-id="ae594-247">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="ae594-247">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ae594-248">Skapa din app i Jenkins</span><span class="sxs-lookup"><span data-stu-id="ae594-248">Build your app in Jenkins</span></span>
> * <span data-ttu-id="ae594-249">Konfigurera Jenkins för integrering med Team Services</span><span class="sxs-lookup"><span data-stu-id="ae594-249">Configure Jenkins for Team Services integration</span></span>
> * <span data-ttu-id="ae594-250">Skapa en distributionsgrupp för hello Azure virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="ae594-250">Create a deployment group for hello Azure virtual machines</span></span>
> * <span data-ttu-id="ae594-251">Skapa en definition av versionen som konfigurerar hello virtuella datorer och distribuerar hello app</span><span class="sxs-lookup"><span data-stu-id="ae594-251">Create a release definition that configures hello VMs and deploys hello app</span></span>

<span data-ttu-id="ae594-252">Avancera toohello nästa självstudiekurs toolearn mer om hur toodeploy ett ljus (Linux, Apache, MySQL och PHP) stacken.</span><span class="sxs-lookup"><span data-stu-id="ae594-252">Advance toohello next tutorial toolearn more about how toodeploy a LAMP (Linux, Apache, MySQL, and PHP) stack.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ae594-253">Distribuera LAMP-stack</span><span class="sxs-lookup"><span data-stu-id="ae594-253">Deploy LAMP stack</span></span>](tutorial-lamp-stack.md)