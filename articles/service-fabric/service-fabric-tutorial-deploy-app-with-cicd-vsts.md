---
title: Distribuera ett Azure Service Fabric-program med kontinuerlig integration (Team Services) | Microsoft Docs
description: "Lär dig hur du ställer in kontinuerlig integrering och distribution för ett Service Fabric-program med hjälp av Visual Studio Team Services.  Distribuera ett program till ett Service Fabric-kluster i Azure."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi
ms.openlocfilehash: 631f9794994530092d05a33b06ebf8c07f331649
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a><span data-ttu-id="bce8c-104">Distribuera ett program med CI/CD: N till ett Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="bce8c-104">Deploy an application with CI/CD to a Service Fabric cluster</span></span>
<span data-ttu-id="bce8c-105">Den här kursen ingår tre av en serie och beskriver hur du ställer in kontinuerlig integrering och distribution för ett Azure Service Fabric-program med Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="bce8c-105">This tutorial is part three of a series and describes how to set up continuous integration and deployment for an Azure Service Fabric application using Visual Studio Team Services.</span></span>  <span data-ttu-id="bce8c-106">Behövs för ett befintligt Service Fabric-program, programmet skapas i [skapar ett .NET-program](service-fabric-tutorial-create-dotnet-app.md) används som exempel.</span><span class="sxs-lookup"><span data-stu-id="bce8c-106">An existing Service Fabric application is needed, the application created in [Build a .NET application](service-fabric-tutorial-create-dotnet-app.md) is used as an example.</span></span>

<span data-ttu-id="bce8c-107">I del tre av serien får du lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="bce8c-107">In part three of the series, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bce8c-108">Lägg till källkontroll i projektet</span><span class="sxs-lookup"><span data-stu-id="bce8c-108">Add source control to your project</span></span>
> * <span data-ttu-id="bce8c-109">Skapa en build-definition i Team Services</span><span class="sxs-lookup"><span data-stu-id="bce8c-109">Create a build definition in Team Services</span></span>
> * <span data-ttu-id="bce8c-110">Skapa en definition för versionen i Team Services</span><span class="sxs-lookup"><span data-stu-id="bce8c-110">Create a release definition in Team Services</span></span>
> * <span data-ttu-id="bce8c-111">Distribuera och uppgradera ett program automatiskt</span><span class="sxs-lookup"><span data-stu-id="bce8c-111">Automatically deploy and upgrade an application</span></span>

<span data-ttu-id="bce8c-112">I den här självstudiekursen serien lär du dig hur du:</span><span class="sxs-lookup"><span data-stu-id="bce8c-112">In this tutorial series you learn how to:</span></span>
> [!div class="checklist"]
> * [<span data-ttu-id="bce8c-113">Skapa ett .NET Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="bce8c-113">Build a .NET Service Fabric application</span></span>](service-fabric-tutorial-create-dotnet-app.md)
> * [<span data-ttu-id="bce8c-114">Distribuera programmet till ett kluster</span><span class="sxs-lookup"><span data-stu-id="bce8c-114">Deploy the application to a remote cluster</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * <span data-ttu-id="bce8c-115">Konfigurera CI/CD: N med hjälp av Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="bce8c-115">Configure CI/CD using Visual Studio Team Services</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bce8c-116">Krav</span><span class="sxs-lookup"><span data-stu-id="bce8c-116">Prerequisites</span></span>
<span data-ttu-id="bce8c-117">Innan du börjar den här kursen:</span><span class="sxs-lookup"><span data-stu-id="bce8c-117">Before you begin this tutorial:</span></span>
- <span data-ttu-id="bce8c-118">Om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="bce8c-118">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="bce8c-119">[Installera Visual Studio 2017](https://www.visualstudio.com/) och installera den **Azure-utveckling** och **ASP.NET och web development** arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="bce8c-119">[Install Visual Studio 2017](https://www.visualstudio.com/) and install the **Azure development** and **ASP.NET and web development** workloads.</span></span>
- [<span data-ttu-id="bce8c-120">Installera Service Fabric SDK</span><span class="sxs-lookup"><span data-stu-id="bce8c-120">Install the Service Fabric SDK</span></span>](service-fabric-get-started.md)
- <span data-ttu-id="bce8c-121">Skapa ett Service Fabric-program, till exempel genom [följa de här självstudierna](service-fabric-tutorial-create-dotnet-app.md).</span><span class="sxs-lookup"><span data-stu-id="bce8c-121">Create a Service Fabric application, for example by [following this tutorial](service-fabric-tutorial-create-dotnet-app.md).</span></span> 
- <span data-ttu-id="bce8c-122">Skapa ett Windows Service Fabric-kluster i Azure, till exempel med [följa de här självstudierna](service-fabric-tutorial-create-cluster-azure-ps.md)</span><span class="sxs-lookup"><span data-stu-id="bce8c-122">Create a Windows Service Fabric cluster on Azure, for example by [following this tutorial](service-fabric-tutorial-create-cluster-azure-ps.md)</span></span>
- <span data-ttu-id="bce8c-123">Skapa en [Team Services-konto](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).</span><span class="sxs-lookup"><span data-stu-id="bce8c-123">Create a [Team Services account](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).</span></span>

## <a name="download-the-voting-sample-application"></a><span data-ttu-id="bce8c-124">Ladda ned exempelprogrammet röst</span><span class="sxs-lookup"><span data-stu-id="bce8c-124">Download the Voting sample application</span></span>
<span data-ttu-id="bce8c-125">Om du inte att skapa exempelprogrammet röst [ingår i den här självstudiekursen serie](service-fabric-tutorial-create-dotnet-app.md), du kan ladda ned den.</span><span class="sxs-lookup"><span data-stu-id="bce8c-125">If you did not build the Voting sample application in [part one of this tutorial series](service-fabric-tutorial-create-dotnet-app.md), you can download it.</span></span> <span data-ttu-id="bce8c-126">Kör följande kommando för att klona exempel app lagringsplatsen till den lokala datorn i ett kommandofönster.</span><span class="sxs-lookup"><span data-stu-id="bce8c-126">In a command window, run the following command to clone the sample app repository to your local machine.</span></span>

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a><span data-ttu-id="bce8c-127">Förbereda en publiceringsprofil</span><span class="sxs-lookup"><span data-stu-id="bce8c-127">Prepare a publish profile</span></span>
<span data-ttu-id="bce8c-128">Nu när du har [skapat ett program](service-fabric-tutorial-create-dotnet-app.md) och har [distribuerat program till Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md), är du redo att konfigurera kontinuerlig integration.</span><span class="sxs-lookup"><span data-stu-id="bce8c-128">Now that you've [created an application](service-fabric-tutorial-create-dotnet-app.md) and have [deployed the application to Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md), you're ready to set up continuous integration.</span></span>  <span data-ttu-id="bce8c-129">Förbereda en publiceringsprofil i ditt program för användning av distributionsprocessen som körs i Team Services.</span><span class="sxs-lookup"><span data-stu-id="bce8c-129">First, prepare a publish profile within your application for use by the deployment process that executes within Team Services.</span></span>  <span data-ttu-id="bce8c-130">Profilen som ska konfigureras för att rikta det kluster som du skapat tidigare.</span><span class="sxs-lookup"><span data-stu-id="bce8c-130">The publish profile should be configured to target the cluster that you've previously created.</span></span>  <span data-ttu-id="bce8c-131">Starta Visual Studio och öppna ett befintligt projekt för Service Fabric-programmet.</span><span class="sxs-lookup"><span data-stu-id="bce8c-131">Start Visual Studio and open an existing Service Fabric application project.</span></span>  <span data-ttu-id="bce8c-132">I **Solution Explorer**, högerklicka på programmet och välj **publicera...** .</span><span class="sxs-lookup"><span data-stu-id="bce8c-132">In **Solution Explorer**, right-click the application and select **Publish...**.</span></span>

<span data-ttu-id="bce8c-133">Välj en profil för målet i projektet program att använda för kontinuerlig integration arbetsflödet, till exempel molnet.</span><span class="sxs-lookup"><span data-stu-id="bce8c-133">Choose a target profile within your application project to use for your continuous integration workflow, for example Cloud.</span></span>  <span data-ttu-id="bce8c-134">Ange klustret Anslutningens slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="bce8c-134">Specify the cluster connection endpoint.</span></span>  <span data-ttu-id="bce8c-135">Kontrollera den **uppgradera programmet** kryssrutan så att programmet uppgraderas för varje distribution i Team Services.</span><span class="sxs-lookup"><span data-stu-id="bce8c-135">Check the **Upgrade the Application** checkbox so that your application upgrades for each deployment in Team Services.</span></span>  <span data-ttu-id="bce8c-136">Klicka på den **spara** hyperlänk till spara inställningarna i profilen och klickar sedan på **Avbryt** att stänga dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bce8c-136">Click the **Save** hyperlink to save the settings to the publish profile and then click **Cancel** to close the dialog box.</span></span>  

![Push-profil][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-team-services-git-repo"></a><span data-ttu-id="bce8c-138">Dela din Visual Studio-lösning till ett nytt Team Services Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="bce8c-138">Share your Visual Studio solution to a new Team Services Git repo</span></span>
<span data-ttu-id="bce8c-139">Dela källfilerna för programmet till ett team projekt i Team Services så att du kan generera versioner.</span><span class="sxs-lookup"><span data-stu-id="bce8c-139">Share your application source files to a team project in Team Services so you can generate builds.</span></span>  

<span data-ttu-id="bce8c-140">Skapa en ny lokal Git repo för ditt projekt genom att välja **lägga till källkontroll** -> **Git** i statusfältet i det nedre högra hörnet av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bce8c-140">Create a new local Git repo for your project by selecting **Add to Source Control** -> **Git** on the status bar in the lower right-hand corner of Visual Studio.</span></span> 

<span data-ttu-id="bce8c-141">I den **Push** visa i **Team Explorer**, Välj den **publicera Git Repo** knappen **Push till Visual Studio Team Services**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-141">In the **Push** view in **Team Explorer**, select the **Publish Git Repo** button under **Push to Visual Studio Team Services**.</span></span>

![Push-Git repo][push-git-repo]

<span data-ttu-id="bce8c-143">Verifiera din e-post och välj ditt konto i den **Team Services domän** listrutan.</span><span class="sxs-lookup"><span data-stu-id="bce8c-143">Verify your email and select your account in the **Team Services Domain** drop-down.</span></span> <span data-ttu-id="bce8c-144">Ange databasens namn och välj **publicera databasen**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-144">Enter your repository name and select **Publish repository**.</span></span>

![Push-Git repo][publish-code]

<span data-ttu-id="bce8c-146">Publicerar lagringsplatsen skapas ett nytt grupprojekt i ditt konto med samma namn som den lokala lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="bce8c-146">Publishing the repo creates a new team project in your account with the same name as the local repo.</span></span> <span data-ttu-id="bce8c-147">Klicka för att skapa lagringsplatsen i en befintlig grupprojekt **Avancerat** bredvid **databasen** namn och välj ett grupprojekt.</span><span class="sxs-lookup"><span data-stu-id="bce8c-147">To create the repo in an existing team project, click **Advanced** next to **Repository** name and select a team project.</span></span> <span data-ttu-id="bce8c-148">Du kan visa koden på webben genom att välja **finns på webben**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-148">You can view your code on the web by selecting **See it on the web**.</span></span>

## <a name="configure-continuous-delivery-with-vsts"></a><span data-ttu-id="bce8c-149">Konfigurera kontinuerlig leverans med VSTS</span><span class="sxs-lookup"><span data-stu-id="bce8c-149">Configure Continuous Delivery with VSTS</span></span>
<span data-ttu-id="bce8c-150">En definition av Team Services build beskriver ett arbetsflöde som består av en uppsättning build-åtgärder som utförs i tur och ordning.</span><span class="sxs-lookup"><span data-stu-id="bce8c-150">A Team Services build definition describes a workflow that is composed of a set of build steps that are executed sequentially.</span></span> <span data-ttu-id="bce8c-151">Skapa en definition av build som som producerar ett Service Fabric-programpaket och andra artefakter att distribuera till ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="bce8c-151">Create a build definition that that produces a Service Fabric application package, and other artifacts, to deploy to a Service Fabric cluster.</span></span> <span data-ttu-id="bce8c-152">Lär dig mer om [Team Services skapa definitioner](https://www.visualstudio.com/docs/build/define/create).</span><span class="sxs-lookup"><span data-stu-id="bce8c-152">Learn more about [Team Services build definitions](https://www.visualstudio.com/docs/build/define/create).</span></span> 

<span data-ttu-id="bce8c-153">En definition av Team Services versionen beskriver ett arbetsflöde som distribuerar ett programpaket till ett kluster.</span><span class="sxs-lookup"><span data-stu-id="bce8c-153">A Team Services release definition describes a workflow that deploys an application package to a cluster.</span></span> <span data-ttu-id="bce8c-154">Köra i hela arbetsflödet som börjar med källfiler som slutar med ett program som körs i klustret när de används tillsammans build definitionen och versionen definition.</span><span class="sxs-lookup"><span data-stu-id="bce8c-154">When used together, the build definition and release definition execute the entire workflow starting with source files to ending with a running application in your cluster.</span></span> <span data-ttu-id="bce8c-155">Mer information om Team Services [viktiga definitioner](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).</span><span class="sxs-lookup"><span data-stu-id="bce8c-155">Learn more about Team Services [release definitions](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).</span></span>

### <a name="create-a-build-definition"></a><span data-ttu-id="bce8c-156">Skapa en build-definition</span><span class="sxs-lookup"><span data-stu-id="bce8c-156">Create a build definition</span></span>
<span data-ttu-id="bce8c-157">Öppna en webbläsare och gå till det nya projektet för team på: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting.</span><span class="sxs-lookup"><span data-stu-id="bce8c-157">Open a web browser and navigate to your new team project at: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting .</span></span> 

<span data-ttu-id="bce8c-158">Välj den **Skapa & släpper** sedan fliken **bygger**, sedan **+ ny definition**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-158">Select the **Build & Release** tab, then **Builds**, then **+ New definition**.</span></span>  <span data-ttu-id="bce8c-159">I **Välj en mall**, Välj den **Azure Service Fabric-programmet** mall och klicka på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-159">In **Select a template**, select the **Azure Service Fabric Application** template and click **Apply**.</span></span> 

![Välj build-mall][select-build-template] 

<span data-ttu-id="bce8c-161">Röstning programmet innehåller ett .NET Core-projekt, så Lägg till en uppgift som återställer beroenden.</span><span class="sxs-lookup"><span data-stu-id="bce8c-161">The voting application contains a .NET Core project, so add a task that restores the dependencies.</span></span> <span data-ttu-id="bce8c-162">I den **uppgifter** väljer **+ Lägg till aktivitet** i nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="bce8c-162">In the **Tasks** view, select **+ Add Task** in the bottom left.</span></span> <span data-ttu-id="bce8c-163">Sök på ”kommandoraden” för att hitta kommandoradsaktiviteten och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-163">Search on "Command Line" to find the command-line task, then click **Add**.</span></span> 

![Lägg till aktivitet][add-task] 

<span data-ttu-id="bce8c-165">I den nya aktiviteten, anger du ”kör dotnet.exe” i **visningsnamn**, ”dotnet.exe” i **verktyget**, och ”återställa” i **argument**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-165">In the new task, enter "Run dotnet.exe" in **Display name**, "dotnet.exe" in **Tool**, and "restore" in **Arguments**.</span></span> 

![Ny aktivitet][new-task] 

<span data-ttu-id="bce8c-167">I den **utlösare** klickar du på den **aktivera den här utlösaren** växla **kontinuerlig Integration**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-167">In the **Triggers** view, click the **Enable this trigger** switch under **Continuous Integration**.</span></span> 

<span data-ttu-id="bce8c-168">Välj **Spara & kö** och anger ”värd VS2017” som den **Agent kön**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-168">Select **Save & queue** and enter "Hosted VS2017" as the **Agent queue**.</span></span> <span data-ttu-id="bce8c-169">Välj **kön** manuellt starta en version.</span><span class="sxs-lookup"><span data-stu-id="bce8c-169">Select **Queue** to manually start a build.</span></span>  <span data-ttu-id="bce8c-170">Bygger också utlösare på push eller incheckning.</span><span class="sxs-lookup"><span data-stu-id="bce8c-170">Builds also triggers upon push or check-in.</span></span>

<span data-ttu-id="bce8c-171">Om du vill kontrollera förloppet build växla till den **bygger** fliken.  När du har kontrollerat att bygga körs korrekt kan du definiera en definition av versionen som distribuerar programmet till ett kluster.</span><span class="sxs-lookup"><span data-stu-id="bce8c-171">To check your build progress, switch to the **Builds** tab.  Once you verify that the build executes successfully, define a release definition that deploys your application to a cluster.</span></span> 

### <a name="create-a-release-definition"></a><span data-ttu-id="bce8c-172">Skapa en definition för versionen</span><span class="sxs-lookup"><span data-stu-id="bce8c-172">Create a release definition</span></span>  

<span data-ttu-id="bce8c-173">Välj den **Skapa & släpper** sedan fliken **versioner**, sedan **+ ny definition**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-173">Select the **Build & Release** tab, then **Releases**, then **+ New definition**.</span></span>  <span data-ttu-id="bce8c-174">I **skapa versionen definition**, Välj den **Azure Service Fabric-distribution** mall från listan och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-174">In **Create release definition**, select the **Azure Service Fabric Deployment** template from the list and click **Next**.</span></span>  <span data-ttu-id="bce8c-175">Välj den **skapa** källa, kontrollera den **kontinuerlig distribution** och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-175">Select the **Build** source, check the **Continuous deployment** box, and click **Create**.</span></span> 

<span data-ttu-id="bce8c-176">I den **miljöer** klickar du på **Lägg till** till höger om **klustret anslutning**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-176">In the **Environments** view, click **Add** to the right of **Cluster Connection**.</span></span>  <span data-ttu-id="bce8c-177">Ange ett anslutningsnamn ”mysftestcluster”, en klusterslutpunkten för ”tcp://mysftestcluster.westus.cloudapp.azure.com:19000” och Azure Active Directory eller certifikatet autentiseringsuppgifter för klustret.</span><span class="sxs-lookup"><span data-stu-id="bce8c-177">Specify a connection name of "mysftestcluster", a cluster endpoint of "tcp://mysftestcluster.westus.cloudapp.azure.com:19000", and the Azure Active Directory or certificate credentials for the cluster.</span></span> <span data-ttu-id="bce8c-178">Definiera de autentiseringsuppgifter som du vill använda för att ansluta till klustret i Azure Active Directory-autentiseringsuppgifter för den **användarnamn** och **lösenord** fält.</span><span class="sxs-lookup"><span data-stu-id="bce8c-178">For Azure Active Directory credentials, define the credentials you want to use to connect to the cluster in the **Username** and **Password** fields.</span></span> <span data-ttu-id="bce8c-179">Definiera för certifikatbaserad autentisering Base64-kodning av certifikatfilen klient i den **klientcertifikat** fältet.</span><span class="sxs-lookup"><span data-stu-id="bce8c-179">For certificate-based authentication, define the Base64 encoding of the client certificate file in the **Client Certificate** field.</span></span>  <span data-ttu-id="bce8c-180">Hjälpen popup på fältet för information om hur du hämtar det värdet.</span><span class="sxs-lookup"><span data-stu-id="bce8c-180">See the help pop-up on that field for info on how to get that value.</span></span>  <span data-ttu-id="bce8c-181">Om certifikatet är lösenordsskyddad, ange lösenordet i den **lösenord** fältet.</span><span class="sxs-lookup"><span data-stu-id="bce8c-181">If your certificate is password-protected, define the password in the **Password** field.</span></span>  <span data-ttu-id="bce8c-182">Klicka på **spara** att spara versionen-definitionen.</span><span class="sxs-lookup"><span data-stu-id="bce8c-182">Click **Save** to save the release definition.</span></span>

![Lägga till kluster-anslutning][add-cluster-connection] 

<span data-ttu-id="bce8c-184">Klicka på **körs på agent**och välj **finns VS2017** för **distribution kön**.</span><span class="sxs-lookup"><span data-stu-id="bce8c-184">Click **Run on agent**, then select **Hosted VS2017** for **Deployment queue**.</span></span> <span data-ttu-id="bce8c-185">Klicka på **spara** att spara versionen-definitionen.</span><span class="sxs-lookup"><span data-stu-id="bce8c-185">Click **Save** to save the release definition.</span></span>

![Körs på agent][run-on-agent]

<span data-ttu-id="bce8c-187">Välj **+ släpper** -> **skapa släpper** -> **skapa** att manuellt skapa en version.</span><span class="sxs-lookup"><span data-stu-id="bce8c-187">Select **+Release** -> **Create Release** -> **Create** to manually create a release.</span></span>  <span data-ttu-id="bce8c-188">Kontrollera att distributionen har slutförts och programmet körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="bce8c-188">Verify that the deployment succeeded and the application is running in the cluster.</span></span>  <span data-ttu-id="bce8c-189">Öppna en webbläsare och gå till [http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span><span class="sxs-lookup"><span data-stu-id="bce8c-189">Open a web browser and navigate to [http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span></span>  <span data-ttu-id="bce8c-190">Observera programversionen i det här exemplet är det ”1.0.0.20170616.3”.</span><span class="sxs-lookup"><span data-stu-id="bce8c-190">Note the application version, in this example it is "1.0.0.20170616.3".</span></span> 

## <a name="commit-and-push-changes-trigger-a-release"></a><span data-ttu-id="bce8c-191">Bekräfta och skicka ändringar kan utlösa en Versionspost</span><span class="sxs-lookup"><span data-stu-id="bce8c-191">Commit and push changes, trigger a release</span></span>
<span data-ttu-id="bce8c-192">Kontrollera att kontinuerlig integration pipeline fungerar genom att kontrollera kod ändrar till Team Services.</span><span class="sxs-lookup"><span data-stu-id="bce8c-192">To verify that the continuous integration pipeline is functioning by checking in some code changes to Team Services.</span></span>    

<span data-ttu-id="bce8c-193">När du skriver koden spåras automatiskt dina ändringar av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bce8c-193">As you write your code, your changes are automatically tracked by Visual Studio.</span></span> <span data-ttu-id="bce8c-194">Genomför ändringar till din lokala Git-lagringsplats genom att välja ikonen-(väntande ändringar</span><span class="sxs-lookup"><span data-stu-id="bce8c-194">Commit changes to your local Git repository by selecting the pending changes icon (</span></span>![Väntande åtgärder][pending]<span data-ttu-id="bce8c-196">) från statusfältet i nederkant högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="bce8c-196">) from the status bar in the bottom right.</span></span>

<span data-ttu-id="bce8c-197">På den **ändringar** i teamet Explorer, lägga till ett meddelande som beskriver uppdateringen och sedan spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="bce8c-197">On the **Changes** view in Team Explorer, add a message describing your update and commit your changes.</span></span>

![Genomför alla][changes]

<span data-ttu-id="bce8c-199">Välj ikonen i opublicerade ändringar statusfältet (![opublicerade ändringar][unpublished-changes]) eller synkronisera vyn i teamet Explorer.</span><span class="sxs-lookup"><span data-stu-id="bce8c-199">Select the unpublished changes status bar icon (![Unpublished changes][unpublished-changes]) or the Sync view in Team Explorer.</span></span> <span data-ttu-id="bce8c-200">Välj **Push** att uppdatera din kod i Team Services/TFS.</span><span class="sxs-lookup"><span data-stu-id="bce8c-200">Select **Push** to update your code in Team Services/TFS.</span></span>

![Skicka ändringar][push]

<span data-ttu-id="bce8c-202">Skicka ändringar till Team Services automatiskt utlöser en version.</span><span class="sxs-lookup"><span data-stu-id="bce8c-202">Pushing the changes to Team Services automatically triggers a build.</span></span>  <span data-ttu-id="bce8c-203">När build-definition har slutförts, skapas automatiskt en version och börja uppgradera programmet på klustret.</span><span class="sxs-lookup"><span data-stu-id="bce8c-203">When the build definition successfully completes, a release is automatically created and starts upgrading the application on the cluster.</span></span>

<span data-ttu-id="bce8c-204">Om du vill kontrollera förloppet build växla till den **bygger** fliken i **Team Explorer** i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bce8c-204">To check your build progress, switch to the **Builds** tab in **Team Explorer** in Visual Studio.</span></span>  <span data-ttu-id="bce8c-205">När du har kontrollerat att bygga körs korrekt kan du definiera en definition av versionen som distribuerar programmet till ett kluster.</span><span class="sxs-lookup"><span data-stu-id="bce8c-205">Once you verify that the build executes successfully, define a release definition that deploys your application to a cluster.</span></span>

<span data-ttu-id="bce8c-206">Kontrollera att distributionen har slutförts och programmet körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="bce8c-206">Verify that the deployment succeeded and the application is running in the cluster.</span></span>  <span data-ttu-id="bce8c-207">Öppna en webbläsare och gå till [http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span><span class="sxs-lookup"><span data-stu-id="bce8c-207">Open a web browser and navigate to [http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span></span>  <span data-ttu-id="bce8c-208">Observera programversionen i det här exemplet är det ”1.0.0.20170815.3”.</span><span class="sxs-lookup"><span data-stu-id="bce8c-208">Note the application version, in this example it is "1.0.0.20170815.3".</span></span>

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a><span data-ttu-id="bce8c-210">Uppdatera programmet</span><span class="sxs-lookup"><span data-stu-id="bce8c-210">Update the application</span></span>
<span data-ttu-id="bce8c-211">Göra kodändringar i programmet.</span><span class="sxs-lookup"><span data-stu-id="bce8c-211">Make code changes in the application.</span></span>  <span data-ttu-id="bce8c-212">Spara och genomföra ändringarna, följa de här stegen.</span><span class="sxs-lookup"><span data-stu-id="bce8c-212">Save and commit the changes, following the previous steps.</span></span>

<span data-ttu-id="bce8c-213">När du börjar uppgraderingen av programmet kan du titta på Uppgraderingsförlopp i Service Fabric Explorer:</span><span class="sxs-lookup"><span data-stu-id="bce8c-213">Once the upgrade of the application begins, you can watch the upgrade progress in Service Fabric Explorer:</span></span>

![Service Fabric Explorer][sfx2]

<span data-ttu-id="bce8c-215">Program-uppgraderingen kan ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="bce8c-215">The application upgrade may take several minutes.</span></span> <span data-ttu-id="bce8c-216">När uppgraderingen är slutförd, kör programmet nästa version.</span><span class="sxs-lookup"><span data-stu-id="bce8c-216">When the upgrade is complete, the application will be running the next version.</span></span>  <span data-ttu-id="bce8c-217">I det här exemplet ”1.0.0.20170815.4”.</span><span class="sxs-lookup"><span data-stu-id="bce8c-217">In this example "1.0.0.20170815.4".</span></span>

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a><span data-ttu-id="bce8c-219">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bce8c-219">Next steps</span></span>
<span data-ttu-id="bce8c-220">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="bce8c-220">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bce8c-221">Lägg till källkontroll i projektet</span><span class="sxs-lookup"><span data-stu-id="bce8c-221">Add source control to your project</span></span>
> * <span data-ttu-id="bce8c-222">Skapa en build-definition</span><span class="sxs-lookup"><span data-stu-id="bce8c-222">Create a build definition</span></span>
> * <span data-ttu-id="bce8c-223">Skapa en definition för versionen</span><span class="sxs-lookup"><span data-stu-id="bce8c-223">Create a release definition</span></span>
> * <span data-ttu-id="bce8c-224">Distribuera och uppgradera ett program automatiskt</span><span class="sxs-lookup"><span data-stu-id="bce8c-224">Automatically deploy and upgrade an application</span></span>

<span data-ttu-id="bce8c-225">Nu när du har distribuerat ett program och konfigurerat kontinuerlig integration, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="bce8c-225">Now that you have deployed an application and configured continuous integration, try the following:</span></span>
- [<span data-ttu-id="bce8c-226">Uppgradera en app</span><span class="sxs-lookup"><span data-stu-id="bce8c-226">Upgrade an app</span></span>](service-fabric-application-upgrade.md)
- [<span data-ttu-id="bce8c-227">Testa en app</span><span class="sxs-lookup"><span data-stu-id="bce8c-227">Test an app</span></span>](service-fabric-testability-overview.md) 
- [<span data-ttu-id="bce8c-228">Övervaka och diagnostisera</span><span class="sxs-lookup"><span data-stu-id="bce8c-228">Monitor and diagnose</span></span>](service-fabric-diagnostics-overview.md)


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[add-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddTask.png
[new-task]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewTask.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png
[run-on-agent]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/RunOnAgent.png