---
title: aaaDeploy ett Azure Service Fabric-program med kontinuerlig integration (Team Services) | Microsoft Docs
description: "Lär dig hur tooset in kontinuerlig integrering och distribution för ett Service Fabric-program med hjälp av Visual Studio Team Services.  Distribuera ett program tooa Service Fabric-kluster i Azure."
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
ms.openlocfilehash: ba9a632b247b0f467e7b66fbe77b4ad54fb3d9ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-with-cicd-tooa-service-fabric-cluster"></a><span data-ttu-id="2d5dd-104">Distribuera ett program med CI/CD tooa Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="2d5dd-104">Deploy an application with CI/CD tooa Service Fabric cluster</span></span>
<span data-ttu-id="2d5dd-105">Den här kursen ingår tre av en serie och beskriver hur tooset in kontinuerlig integrering och distribution för ett Azure Service Fabric-program med Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-105">This tutorial is part three of a series and describes how tooset up continuous integration and deployment for an Azure Service Fabric application using Visual Studio Team Services.</span></span>  <span data-ttu-id="2d5dd-106">Ett befintligt Service Fabric-program behövs, hello programmet skapas i [skapar ett .NET-program](service-fabric-tutorial-create-dotnet-app.md) används som exempel.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-106">An existing Service Fabric application is needed, hello application created in [Build a .NET application](service-fabric-tutorial-create-dotnet-app.md) is used as an example.</span></span>

<span data-ttu-id="2d5dd-107">Del tre av hello serie du lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="2d5dd-107">In part three of hello series, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2d5dd-108">Lägg till kontrollen tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="2d5dd-108">Add source control tooyour project</span></span>
> * <span data-ttu-id="2d5dd-109">Skapa en build-definition i Team Services</span><span class="sxs-lookup"><span data-stu-id="2d5dd-109">Create a build definition in Team Services</span></span>
> * <span data-ttu-id="2d5dd-110">Skapa en definition för versionen i Team Services</span><span class="sxs-lookup"><span data-stu-id="2d5dd-110">Create a release definition in Team Services</span></span>
> * <span data-ttu-id="2d5dd-111">Distribuera och uppgradera ett program automatiskt</span><span class="sxs-lookup"><span data-stu-id="2d5dd-111">Automatically deploy and upgrade an application</span></span>

<span data-ttu-id="2d5dd-112">I den här självstudiekursen serien lär du dig hur du:</span><span class="sxs-lookup"><span data-stu-id="2d5dd-112">In this tutorial series you learn how to:</span></span>
> [!div class="checklist"]
> * [<span data-ttu-id="2d5dd-113">Skapa ett .NET Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="2d5dd-113">Build a .NET Service Fabric application</span></span>](service-fabric-tutorial-create-dotnet-app.md)
> * [<span data-ttu-id="2d5dd-114">Distribuera programmet hello tooa fjärrkluster</span><span class="sxs-lookup"><span data-stu-id="2d5dd-114">Deploy hello application tooa remote cluster</span></span>](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * <span data-ttu-id="2d5dd-115">Konfigurera CI/CD: N med hjälp av Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="2d5dd-115">Configure CI/CD using Visual Studio Team Services</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d5dd-116">Krav</span><span class="sxs-lookup"><span data-stu-id="2d5dd-116">Prerequisites</span></span>
<span data-ttu-id="2d5dd-117">Innan du börjar den här kursen:</span><span class="sxs-lookup"><span data-stu-id="2d5dd-117">Before you begin this tutorial:</span></span>
- <span data-ttu-id="2d5dd-118">Om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="2d5dd-118">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="2d5dd-119">[Installera Visual Studio 2017](https://www.visualstudio.com/) och installera hello **Azure-utveckling** och **ASP.NET och web development** arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-119">[Install Visual Studio 2017](https://www.visualstudio.com/) and install hello **Azure development** and **ASP.NET and web development** workloads.</span></span>
- [<span data-ttu-id="2d5dd-120">Installera hello Service Fabric-SDK</span><span class="sxs-lookup"><span data-stu-id="2d5dd-120">Install hello Service Fabric SDK</span></span>](service-fabric-get-started.md)
- <span data-ttu-id="2d5dd-121">Skapa ett Service Fabric-program, till exempel genom [följa de här självstudierna](service-fabric-tutorial-create-dotnet-app.md).</span><span class="sxs-lookup"><span data-stu-id="2d5dd-121">Create a Service Fabric application, for example by [following this tutorial](service-fabric-tutorial-create-dotnet-app.md).</span></span> 
- <span data-ttu-id="2d5dd-122">Skapa ett Windows Service Fabric-kluster i Azure, till exempel med [följa de här självstudierna](service-fabric-tutorial-create-cluster-azure-ps.md)</span><span class="sxs-lookup"><span data-stu-id="2d5dd-122">Create a Windows Service Fabric cluster on Azure, for example by [following this tutorial](service-fabric-tutorial-create-cluster-azure-ps.md)</span></span>
- <span data-ttu-id="2d5dd-123">Skapa en [Team Services-konto](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).</span><span class="sxs-lookup"><span data-stu-id="2d5dd-123">Create a [Team Services account](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).</span></span>

## <a name="download-hello-voting-sample-application"></a><span data-ttu-id="2d5dd-124">Hämta hello röst exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="2d5dd-124">Download hello Voting sample application</span></span>
<span data-ttu-id="2d5dd-125">Om du inte att skapa hello röst exempelprogrammet [ingår i den här självstudiekursen serie](service-fabric-tutorial-create-dotnet-app.md), du kan ladda ned den.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-125">If you did not build hello Voting sample application in [part one of this tutorial series](service-fabric-tutorial-create-dotnet-app.md), you can download it.</span></span> <span data-ttu-id="2d5dd-126">Kör hello efter kommandot tooclone hello exempel app databasen tooyour lokala datorn i ett kommandofönster.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-126">In a command window, run hello following command tooclone hello sample app repository tooyour local machine.</span></span>

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a><span data-ttu-id="2d5dd-127">Förbereda en publiceringsprofil</span><span class="sxs-lookup"><span data-stu-id="2d5dd-127">Prepare a publish profile</span></span>
<span data-ttu-id="2d5dd-128">Nu när du har [skapat ett program](service-fabric-tutorial-create-dotnet-app.md) och har [distribueras hello programmet tooAzure](service-fabric-tutorial-deploy-app-to-party-cluster.md), du är klar tooset in kontinuerlig integration.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-128">Now that you've [created an application](service-fabric-tutorial-create-dotnet-app.md) and have [deployed hello application tooAzure](service-fabric-tutorial-deploy-app-to-party-cluster.md), you're ready tooset up continuous integration.</span></span>  <span data-ttu-id="2d5dd-129">Förbereda en publiceringsprofil i ditt program för användning av hello Distributionsprocess som körs i Team Services.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-129">First, prepare a publish profile within your application for use by hello deployment process that executes within Team Services.</span></span>  <span data-ttu-id="2d5dd-130">hello publiceringsprofil bör vara konfigurerade tootarget hello som du skapat tidigare.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-130">hello publish profile should be configured tootarget hello cluster that you've previously created.</span></span>  <span data-ttu-id="2d5dd-131">Starta Visual Studio och öppna ett befintligt projekt för Service Fabric-programmet.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-131">Start Visual Studio and open an existing Service Fabric application project.</span></span>  <span data-ttu-id="2d5dd-132">I **Solution Explorer**, högerklicka på programmet hello och välj **publicera...** .</span><span class="sxs-lookup"><span data-stu-id="2d5dd-132">In **Solution Explorer**, right-click hello application and select **Publish...**.</span></span>

<span data-ttu-id="2d5dd-133">Välj en profil för målet i ditt program projektet toouse för kontinuerlig integration arbetsflödet, till exempel molnet.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-133">Choose a target profile within your application project toouse for your continuous integration workflow, for example Cloud.</span></span>  <span data-ttu-id="2d5dd-134">Ange hello klustret Anslutningens slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-134">Specify hello cluster connection endpoint.</span></span>  <span data-ttu-id="2d5dd-135">Kontrollera hello **uppgradera hello programmet** kryssrutan så att programmet uppgraderas för varje distribution i Team Services.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-135">Check hello **Upgrade hello Application** checkbox so that your application upgrades for each deployment in Team Services.</span></span>  <span data-ttu-id="2d5dd-136">Klicka på hello **spara** hyperlink toosave hello inställningar toohello publiceringsprofil och klicka sedan på **Avbryt** tooclose hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-136">Click hello **Save** hyperlink toosave hello settings toohello publish profile and then click **Cancel** tooclose hello dialog box.</span></span>  

![Push-profil][publish-app-profile]

## <a name="share-your-visual-studio-solution-tooa-new-team-services-git-repo"></a><span data-ttu-id="2d5dd-138">Dela din Visual Studio-lösning tooa nytt Team Services Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="2d5dd-138">Share your Visual Studio solution tooa new Team Services Git repo</span></span>
<span data-ttu-id="2d5dd-139">Dela källfilerna programmet tooa grupprojekt i Team Services så att du kan generera versioner.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-139">Share your application source files tooa team project in Team Services so you can generate builds.</span></span>  

<span data-ttu-id="2d5dd-140">Skapa en ny lokal Git repo för ditt projekt genom att välja **lägga till tooSource kontrollen** -> **Git** hello statusfältet i hello nedre högra hörnet i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-140">Create a new local Git repo for your project by selecting **Add tooSource Control** -> **Git** on hello status bar in hello lower right-hand corner of Visual Studio.</span></span> 

<span data-ttu-id="2d5dd-141">I hello **Push** visa i **Team Explorer**väljer hello **publicera Git Repo** knappen **Push tooVisual Studio Team Services**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-141">In hello **Push** view in **Team Explorer**, select hello **Publish Git Repo** button under **Push tooVisual Studio Team Services**.</span></span>

![Push-Git repo][push-git-repo]

<span data-ttu-id="2d5dd-143">Verifiera din e-post och välj ditt konto i hello **Team Services domän** listrutan.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-143">Verify your email and select your account in hello **Team Services Domain** drop-down.</span></span> <span data-ttu-id="2d5dd-144">Ange databasens namn och välj **publicera databasen**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-144">Enter your repository name and select **Publish repository**.</span></span>

![Push-Git repo][publish-code]

<span data-ttu-id="2d5dd-146">Publicerar hello lagringsplatsen skapas ett nytt grupprojekt i ditt konto med samma namn som hello lokala lagringsplatsen hello.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-146">Publishing hello repo creates a new team project in your account with hello same name as hello local repo.</span></span> <span data-ttu-id="2d5dd-147">toocreate hello lagringsplatsen i en befintlig grupprojekt klickar du på **Avancerat** nästa för**databasen** namn och välj ett grupprojekt.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-147">toocreate hello repo in an existing team project, click **Advanced** next too**Repository** name and select a team project.</span></span> <span data-ttu-id="2d5dd-148">Du kan visa koden på hello webbplatsen genom att välja **finns på hello web**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-148">You can view your code on hello web by selecting **See it on hello web**.</span></span>

## <a name="configure-continuous-delivery-with-vsts"></a><span data-ttu-id="2d5dd-149">Konfigurera kontinuerlig leverans med VSTS</span><span class="sxs-lookup"><span data-stu-id="2d5dd-149">Configure Continuous Delivery with VSTS</span></span>
<span data-ttu-id="2d5dd-150">En definition av Team Services build beskriver ett arbetsflöde som består av en uppsättning build-åtgärder som utförs i tur och ordning.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-150">A Team Services build definition describes a workflow that is composed of a set of build steps that are executed sequentially.</span></span> <span data-ttu-id="2d5dd-151">Skapa en definition av build som som producerar ett Service Fabric-programpaket och andra artefakter, toodeploy tooa Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-151">Create a build definition that that produces a Service Fabric application package, and other artifacts, toodeploy tooa Service Fabric cluster.</span></span> <span data-ttu-id="2d5dd-152">Lär dig mer om [Team Services skapa definitioner](https://www.visualstudio.com/docs/build/define/create).</span><span class="sxs-lookup"><span data-stu-id="2d5dd-152">Learn more about [Team Services build definitions](https://www.visualstudio.com/docs/build/define/create).</span></span> 

<span data-ttu-id="2d5dd-153">En definition av Team Services versionen beskriver ett arbetsflöde som distribuerar ett program paketet tooa kluster.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-153">A Team Services release definition describes a workflow that deploys an application package tooa cluster.</span></span> <span data-ttu-id="2d5dd-154">När de används tillsammans hello skapa definition och versionen definition köra hello hela arbetsflödet från och med källan filer tooending med ett program som körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-154">When used together, hello build definition and release definition execute hello entire workflow starting with source files tooending with a running application in your cluster.</span></span> <span data-ttu-id="2d5dd-155">Mer information om Team Services [viktiga definitioner](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).</span><span class="sxs-lookup"><span data-stu-id="2d5dd-155">Learn more about Team Services [release definitions](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).</span></span>

### <a name="create-a-build-definition"></a><span data-ttu-id="2d5dd-156">Skapa en build-definition</span><span class="sxs-lookup"><span data-stu-id="2d5dd-156">Create a build definition</span></span>
<span data-ttu-id="2d5dd-157">Öppna en webbläsare och gå tooyour nytt grupprojekt på: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-157">Open a web browser and navigate tooyour new team project at: https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting .</span></span> 

<span data-ttu-id="2d5dd-158">Välj hello **Skapa & släpper** sedan fliken **bygger**, sedan **+ ny definition**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-158">Select hello **Build & Release** tab, then **Builds**, then **+ New definition**.</span></span>  <span data-ttu-id="2d5dd-159">I **Välj en mall**väljer hello **Azure Service Fabric-programmet** mall och klicka på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-159">In **Select a template**, select hello **Azure Service Fabric Application** template and click **Apply**.</span></span> 

![Välj build-mall][select-build-template] 

<span data-ttu-id="2d5dd-161">hello röstning program innehåller ett .NET Core-projekt, så lägger du till en uppgift som återställer hello beroenden.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-161">hello voting application contains a .NET Core project, so add a task that restores hello dependencies.</span></span> <span data-ttu-id="2d5dd-162">I hello **uppgifter** väljer **+ Lägg till aktivitet** i hello längst ned till vänster.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-162">In hello **Tasks** view, select **+ Add Task** in hello bottom left.</span></span> <span data-ttu-id="2d5dd-163">Sök på toofind ”kommandoraden” Hej kommandoradsaktivitet och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-163">Search on "Command Line" toofind hello command-line task, then click **Add**.</span></span> 

![Lägg till aktivitet][add-task] 

<span data-ttu-id="2d5dd-165">I hello ny aktivitet, anger du ”kör dotnet.exe” i **visningsnamn**, ”dotnet.exe” i **verktyget**, och ”återställa” i **argument**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-165">In hello new task, enter "Run dotnet.exe" in **Display name**, "dotnet.exe" in **Tool**, and "restore" in **Arguments**.</span></span> 

![Ny aktivitet][new-task] 

<span data-ttu-id="2d5dd-167">I hello **utlösare** klickar du på hello **aktivera den här utlösaren** växla **kontinuerlig Integration**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-167">In hello **Triggers** view, click hello **Enable this trigger** switch under **Continuous Integration**.</span></span> 

<span data-ttu-id="2d5dd-168">Välj **Spara & kö** och anger ”värd VS2017” Hej **Agent kön**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-168">Select **Save & queue** and enter "Hosted VS2017" as hello **Agent queue**.</span></span> <span data-ttu-id="2d5dd-169">Välj **kön** toomanually starta en version.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-169">Select **Queue** toomanually start a build.</span></span>  <span data-ttu-id="2d5dd-170">Bygger också utlösare på push eller incheckning.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-170">Builds also triggers upon push or check-in.</span></span>

<span data-ttu-id="2d5dd-171">toocheck ändringarna build växeln toohello **bygger** fliken.  När du har kontrollerat att hello build körs korrekt kan du definiera en definition av versionen som distribuerar programmet tooa klustret.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-171">toocheck your build progress, switch toohello **Builds** tab.  Once you verify that hello build executes successfully, define a release definition that deploys your application tooa cluster.</span></span> 

### <a name="create-a-release-definition"></a><span data-ttu-id="2d5dd-172">Skapa en definition för versionen</span><span class="sxs-lookup"><span data-stu-id="2d5dd-172">Create a release definition</span></span>  

<span data-ttu-id="2d5dd-173">Välj hello **Skapa & släpper** sedan fliken **versioner**, sedan **+ ny definition**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-173">Select hello **Build & Release** tab, then **Releases**, then **+ New definition**.</span></span>  <span data-ttu-id="2d5dd-174">I **skapa versionen definition**väljer hello **Azure Service Fabric-distribution** mall från hello listan och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-174">In **Create release definition**, select hello **Azure Service Fabric Deployment** template from hello list and click **Next**.</span></span>  <span data-ttu-id="2d5dd-175">Välj hello **skapa** källa, kontrollera hello **kontinuerlig distribution** och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-175">Select hello **Build** source, check hello **Continuous deployment** box, and click **Create**.</span></span> 

<span data-ttu-id="2d5dd-176">I hello **miljöer** klickar du på **Lägg till** toohello höger i **klustret anslutning**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-176">In hello **Environments** view, click **Add** toohello right of **Cluster Connection**.</span></span>  <span data-ttu-id="2d5dd-177">Ange anslutningsnamn ”mysftestcluster”, en klusterslutpunkten ”tcp://mysftestcluster.westus.cloudapp.azure.com:19000” hello Azure Active Directory och certifikatet autentiseringsuppgifter för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-177">Specify a connection name of "mysftestcluster", a cluster endpoint of "tcp://mysftestcluster.westus.cloudapp.azure.com:19000", and hello Azure Active Directory or certificate credentials for hello cluster.</span></span> <span data-ttu-id="2d5dd-178">Azure Active Directory-autentiseringsuppgifter för att definiera hello autentiseringsuppgifter som du vill toouse tooconnect toohello klustret i hello **användarnamn** och **lösenord** fält.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-178">For Azure Active Directory credentials, define hello credentials you want toouse tooconnect toohello cluster in hello **Username** and **Password** fields.</span></span> <span data-ttu-id="2d5dd-179">Definiera för certifikatbaserad autentisering hello Base64-kodning av hello klienten certifikatfilen i hello **klientcertifikat** fältet.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-179">For certificate-based authentication, define hello Base64 encoding of hello client certificate file in hello **Client Certificate** field.</span></span>  <span data-ttu-id="2d5dd-180">Se hello hjälp popup-fönster på fältet för information om hur tooget värde.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-180">See hello help pop-up on that field for info on how tooget that value.</span></span>  <span data-ttu-id="2d5dd-181">Om certifikatet är lösenordsskyddad, definiera hello lösenord i hello **lösenord** fältet.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-181">If your certificate is password-protected, define hello password in hello **Password** field.</span></span>  <span data-ttu-id="2d5dd-182">Klicka på **spara** toosave hello versionen definition.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-182">Click **Save** toosave hello release definition.</span></span>

![Lägga till kluster-anslutning][add-cluster-connection] 

<span data-ttu-id="2d5dd-184">Klicka på **körs på agent**och välj **finns VS2017** för **distribution kön**.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-184">Click **Run on agent**, then select **Hosted VS2017** for **Deployment queue**.</span></span> <span data-ttu-id="2d5dd-185">Klicka på **spara** toosave hello versionen definition.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-185">Click **Save** toosave hello release definition.</span></span>

![Körs på agent][run-on-agent]

<span data-ttu-id="2d5dd-187">Välj **+ släpper** -> **skapa släpper** -> **skapa** toomanually skapa en version.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-187">Select **+Release** -> **Create Release** -> **Create** toomanually create a release.</span></span>  <span data-ttu-id="2d5dd-188">Kontrollera att hello distributionen lyckades och hello programmet körs i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-188">Verify that hello deployment succeeded and hello application is running in hello cluster.</span></span>  <span data-ttu-id="2d5dd-189">Öppna en webbläsare och gå för[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span><span class="sxs-lookup"><span data-stu-id="2d5dd-189">Open a web browser and navigate too[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span></span>  <span data-ttu-id="2d5dd-190">Observera hello programversion i det här exemplet är det ”1.0.0.20170616.3”.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-190">Note hello application version, in this example it is "1.0.0.20170616.3".</span></span> 

## <a name="commit-and-push-changes-trigger-a-release"></a><span data-ttu-id="2d5dd-191">Bekräfta och skicka ändringar kan utlösa en Versionspost</span><span class="sxs-lookup"><span data-stu-id="2d5dd-191">Commit and push changes, trigger a release</span></span>
<span data-ttu-id="2d5dd-192">tooverify som hello kontinuerlig integration pipeline fungerar genom att söka i vissa kodändringar tooTeam tjänster.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-192">tooverify that hello continuous integration pipeline is functioning by checking in some code changes tooTeam Services.</span></span>    

<span data-ttu-id="2d5dd-193">När du skriver koden spåras automatiskt dina ändringar av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-193">As you write your code, your changes are automatically tracked by Visual Studio.</span></span> <span data-ttu-id="2d5dd-194">Genomför ändringar tooyour lokal Git-lagringsplats genom att välja hello väntande ändringar ikonen (</span><span class="sxs-lookup"><span data-stu-id="2d5dd-194">Commit changes tooyour local Git repository by selecting hello pending changes icon (</span></span>![Väntande åtgärder][pending]<span data-ttu-id="2d5dd-196">) från hello statusfältet i hello nedre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-196">) from hello status bar in hello bottom right.</span></span>

<span data-ttu-id="2d5dd-197">På hello **ändringar** i teamet Explorer, lägga till ett meddelande som beskriver uppdateringen och sedan spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-197">On hello **Changes** view in Team Explorer, add a message describing your update and commit your changes.</span></span>

![Genomför alla][changes]

<span data-ttu-id="2d5dd-199">Välj hello opublicerade ändringar statusikon-fältet (![opublicerade ändringar][unpublished-changes]) eller hello Sync vyn i teamet Explorer.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-199">Select hello unpublished changes status bar icon (![Unpublished changes][unpublished-changes]) or hello Sync view in Team Explorer.</span></span> <span data-ttu-id="2d5dd-200">Välj **Push** tooupdate koden i Team Services/TFS.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-200">Select **Push** tooupdate your code in Team Services/TFS.</span></span>

![Skicka ändringar][push]

<span data-ttu-id="2d5dd-202">Push-överföring hello ändringar tooTeam Services automatiskt utlösare en version.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-202">Pushing hello changes tooTeam Services automatically triggers a build.</span></span>  <span data-ttu-id="2d5dd-203">När hello build definition har slutförts skapas automatiskt en Versionspost och börja uppgradera hello programmet på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-203">When hello build definition successfully completes, a release is automatically created and starts upgrading hello application on hello cluster.</span></span>

<span data-ttu-id="2d5dd-204">toocheck ändringarna build växeln toohello **bygger** fliken i **Team Explorer** i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-204">toocheck your build progress, switch toohello **Builds** tab in **Team Explorer** in Visual Studio.</span></span>  <span data-ttu-id="2d5dd-205">När du har kontrollerat att hello build körs korrekt kan du definiera en definition av versionen som distribuerar programmet tooa klustret.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-205">Once you verify that hello build executes successfully, define a release definition that deploys your application tooa cluster.</span></span>

<span data-ttu-id="2d5dd-206">Kontrollera att hello distributionen lyckades och hello programmet körs i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-206">Verify that hello deployment succeeded and hello application is running in hello cluster.</span></span>  <span data-ttu-id="2d5dd-207">Öppna en webbläsare och gå för[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span><span class="sxs-lookup"><span data-stu-id="2d5dd-207">Open a web browser and navigate too[http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.westus.cloudapp.azure.com:19080/Explorer/).</span></span>  <span data-ttu-id="2d5dd-208">Observera hello programversion i det här exemplet är det ”1.0.0.20170815.3”.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-208">Note hello application version, in this example it is "1.0.0.20170815.3".</span></span>

![Service Fabric Explorer][sfx1]

## <a name="update-hello-application"></a><span data-ttu-id="2d5dd-210">Uppdatera hello program</span><span class="sxs-lookup"><span data-stu-id="2d5dd-210">Update hello application</span></span>
<span data-ttu-id="2d5dd-211">Göra kodändringar i hello program.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-211">Make code changes in hello application.</span></span>  <span data-ttu-id="2d5dd-212">Spara och genomför hello ändringar, följande hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-212">Save and commit hello changes, following hello previous steps.</span></span>

<span data-ttu-id="2d5dd-213">När hello uppgradera hello program börjar, kan du titta på hello Uppgraderingsförlopp i Service Fabric Explorer:</span><span class="sxs-lookup"><span data-stu-id="2d5dd-213">Once hello upgrade of hello application begins, you can watch hello upgrade progress in Service Fabric Explorer:</span></span>

![Service Fabric Explorer][sfx2]

<span data-ttu-id="2d5dd-215">uppgradering av programmet hello kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-215">hello application upgrade may take several minutes.</span></span> <span data-ttu-id="2d5dd-216">När hello uppgraderingen är klar kör hello programmet hello nästa version.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-216">When hello upgrade is complete, hello application will be running hello next version.</span></span>  <span data-ttu-id="2d5dd-217">I det här exemplet ”1.0.0.20170815.4”.</span><span class="sxs-lookup"><span data-stu-id="2d5dd-217">In this example "1.0.0.20170815.4".</span></span>

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a><span data-ttu-id="2d5dd-219">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2d5dd-219">Next steps</span></span>
<span data-ttu-id="2d5dd-220">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="2d5dd-220">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2d5dd-221">Lägg till kontrollen tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="2d5dd-221">Add source control tooyour project</span></span>
> * <span data-ttu-id="2d5dd-222">Skapa en build-definition</span><span class="sxs-lookup"><span data-stu-id="2d5dd-222">Create a build definition</span></span>
> * <span data-ttu-id="2d5dd-223">Skapa en definition för versionen</span><span class="sxs-lookup"><span data-stu-id="2d5dd-223">Create a release definition</span></span>
> * <span data-ttu-id="2d5dd-224">Distribuera och uppgradera ett program automatiskt</span><span class="sxs-lookup"><span data-stu-id="2d5dd-224">Automatically deploy and upgrade an application</span></span>

<span data-ttu-id="2d5dd-225">Nu när du har distribuerat ett program och konfigurerat kontinuerlig integration, försök hello följande:</span><span class="sxs-lookup"><span data-stu-id="2d5dd-225">Now that you have deployed an application and configured continuous integration, try hello following:</span></span>
- [<span data-ttu-id="2d5dd-226">Uppgradera en app</span><span class="sxs-lookup"><span data-stu-id="2d5dd-226">Upgrade an app</span></span>](service-fabric-application-upgrade.md)
- [<span data-ttu-id="2d5dd-227">Testa en app</span><span class="sxs-lookup"><span data-stu-id="2d5dd-227">Test an app</span></span>](service-fabric-testability-overview.md) 
- [<span data-ttu-id="2d5dd-228">Övervaka och diagnostisera</span><span class="sxs-lookup"><span data-stu-id="2d5dd-228">Monitor and diagnose</span></span>](service-fabric-diagnostics-overview.md)


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