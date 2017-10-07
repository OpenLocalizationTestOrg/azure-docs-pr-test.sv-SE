---
title: "aaaAzure Service Fabric-plugin-program för Eclipse | Microsoft Docs"
description: "Kom igång med hello Service Fabric-plugin-programmet för Eclipse."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/21/2016
ms.author: saysa
ms.openlocfilehash: 4ba5a28a6282387249a2bd4e62314e891ff04162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-plug-in-for-eclipse-java-application-development"></a><span data-ttu-id="84c13-103">Service Fabric-plugin-program för utveckling av Java-program i Eclipse</span><span class="sxs-lookup"><span data-stu-id="84c13-103">Service Fabric plug-in for Eclipse Java application development</span></span>
<span data-ttu-id="84c13-104">Eclipse är en av hello används mest integrerad utvecklingsmiljöer (IDEs) för Java-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="84c13-104">Eclipse is one of hello most widely used integrated development environments (IDEs) for Java developers.</span></span> <span data-ttu-id="84c13-105">I den här artikeln beskriver vi hur tooset in Eclipse development environment toowork med Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="84c13-105">In this article, we describe how tooset up your Eclipse development environment toowork with Azure Service Fabric.</span></span> <span data-ttu-id="84c13-106">Lär dig hur tooinstall hello Service Fabric-plugin-program, skapa ett Service Fabric-program och distribuera din Service Fabric application tooa lokal eller fjärransluten Service Fabric-kluster i Eclipse Neon.</span><span class="sxs-lookup"><span data-stu-id="84c13-106">Learn how tooinstall hello Service Fabric plug-in, create a Service Fabric application, and deploy your Service Fabric application tooa local or remote Service Fabric cluster in Eclipse Neon.</span></span>

## <a name="install-or-update-hello-service-fabric-plug-in-in-eclipse-neon"></a><span data-ttu-id="84c13-107">Installera eller uppdatera hello Service Fabric-plugin-programmet i Eclipse Neon</span><span class="sxs-lookup"><span data-stu-id="84c13-107">Install or update hello Service Fabric plug-in in Eclipse Neon</span></span>
<span data-ttu-id="84c13-108">Du kan installera ett Service Fabric-plugin-program i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="84c13-108">You can install a Service Fabric plug-in in Eclipse.</span></span> <span data-ttu-id="84c13-109">hello plugin-programmet kan underlätta hello processen att skapa och distribuera Java-tjänster.</span><span class="sxs-lookup"><span data-stu-id="84c13-109">hello plug-in can help simplify hello process of building and deploying Java services.</span></span>

1.  <span data-ttu-id="84c13-110">Se till att du har hello senaste versionen av Eclipse Neon och hello senaste versionen av Buildship (1.0.17 eller senare) installerat:</span><span class="sxs-lookup"><span data-stu-id="84c13-110">Ensure that you have hello latest version of Eclipse Neon and hello latest version of Buildship (1.0.17 or a later version) installed:</span></span>
    -   <span data-ttu-id="84c13-111">toocheck hello versioner av installerade komponenter i Eclipse Neon blir för**hjälp** > **installationsinformationen**.</span><span class="sxs-lookup"><span data-stu-id="84c13-111">toocheck hello versions of installed components, in Eclipse Neon, go too**Help** > **Installation Details**.</span></span>
    -   <span data-ttu-id="84c13-112">tooupdate Buildship, se [Eclipse Buildship: Eclipse plugin-program för Gradle][buildship-update].</span><span class="sxs-lookup"><span data-stu-id="84c13-112">tooupdate Buildship, see [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update].</span></span>
    -   <span data-ttu-id="84c13-113">toocheck för och installera uppdateringar för Eclipse Neon gå för**hjälp** > **söka efter uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="84c13-113">toocheck for and install updates for Eclipse Neon, go too**Help** > **Check for Updates**.</span></span>

2.  <span data-ttu-id="84c13-114">tooinstall hello Service Fabric-plugin-programmet i Eclipse Neon gå för**hjälp** > **installera ny programvara**.</span><span class="sxs-lookup"><span data-stu-id="84c13-114">tooinstall hello Service Fabric plug-in, in Eclipse Neon, go too**Help** > **Install New Software**.</span></span>
  1.    <span data-ttu-id="84c13-115">I hello **arbeta med** ange **http://dl.microsoft.com/eclipse**.</span><span class="sxs-lookup"><span data-stu-id="84c13-115">In hello **Work with** box, enter **http://dl.microsoft.com/eclipse**.</span></span>
  2.    <span data-ttu-id="84c13-116">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="84c13-116">Click **Add**.</span></span>

         ![Service Fabric-plugin-program för Eclipse Neon][sf-eclipse-plugin-install]
  3.    <span data-ttu-id="84c13-118">Välj hello Service Fabric-plugin-program och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="84c13-118">Select hello Service Fabric plug-in, and then click **Next**.</span></span>
  4.    <span data-ttu-id="84c13-119">Slutför hello installationssteg och acceptera licensvillkoren för hello.</span><span class="sxs-lookup"><span data-stu-id="84c13-119">Complete hello installation steps, and then accept hello Microsoft Software License Terms.</span></span>

<span data-ttu-id="84c13-120">Kontrollera att du har hello senaste versionen om du redan har hello Service Fabric plugin-program installerat.</span><span class="sxs-lookup"><span data-stu-id="84c13-120">If you already have hello Service Fabric plug-in installed, make sure that you have hello latest version.</span></span> <span data-ttu-id="84c13-121">Gå toocheck efter tillgängliga uppdateringar för**hjälp** > **installationsinformationen**.</span><span class="sxs-lookup"><span data-stu-id="84c13-121">toocheck for available updates, go too**Help** > **Installation Details**.</span></span> <span data-ttu-id="84c13-122">Välj Service Fabric i hello lista över installerade plugin-program, och klicka sedan på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="84c13-122">In hello list of installed plug-ins, select Service Fabric, and then click **Update**.</span></span> <span data-ttu-id="84c13-123">Tillgängliga uppdateringar installeras.</span><span class="sxs-lookup"><span data-stu-id="84c13-123">Available updates will be installed.</span></span>

> [!NOTE]
> <span data-ttu-id="84c13-124">Installera eller uppdatera hello Service Fabric-plugin-programmet är långsam, kanske på grund av tooan Eclipse inställningen.</span><span class="sxs-lookup"><span data-stu-id="84c13-124">If installing or updating hello Service Fabric plug-in is slow, it might be due tooan Eclipse setting.</span></span> <span data-ttu-id="84c13-125">Eclipse samlar in metadata i alla ändringar tooupdate platser som har registrerats med Eclipse-instans.</span><span class="sxs-lookup"><span data-stu-id="84c13-125">Eclipse collects metadata on all changes tooupdate sites that are registered with your Eclipse instance.</span></span> <span data-ttu-id="84c13-126">Gå toospeed hello registreringen av söker efter och installera en uppdatering med Service Fabric plugin-programmet för**tillgänglig programvara platser**.</span><span class="sxs-lookup"><span data-stu-id="84c13-126">toospeed up hello process of checking for and installing a Service Fabric plug-in update, go too**Available Software Sites**.</span></span> <span data-ttu-id="84c13-127">Avmarkera kryssrutorna för hello för alla platser förutom hello som pekar toohello Service Fabric-plugin-plats (http://dl.microsoft.com/eclipse/azure/servicefabric).</span><span class="sxs-lookup"><span data-stu-id="84c13-127">Clear hello check boxes for all sites except for hello one that points toohello Service Fabric plug-in location (http://dl.microsoft.com/eclipse/azure/servicefabric).</span></span>

## <a name="create-a-service-fabric-application-in-eclipse"></a><span data-ttu-id="84c13-128">Skapa ett Service Fabric-program i Eclipse</span><span class="sxs-lookup"><span data-stu-id="84c13-128">Create a Service Fabric application in Eclipse</span></span>

1.  <span data-ttu-id="84c13-129">I Eclipse Neon går för**filen** > **ny** > **andra**.</span><span class="sxs-lookup"><span data-stu-id="84c13-129">In Eclipse Neon, go too**File** > **New** > **Other**.</span></span> <span data-ttu-id="84c13-130">Välj **Service Fabric Project** (Service Fabric Project) och klicka sedan på **Next** (Nästa).</span><span class="sxs-lookup"><span data-stu-id="84c13-130">Select  **Service Fabric Project**, and then click **Next**.</span></span>

    ![Service Fabric, ny projektsida 1][create-application/p1]

2.  <span data-ttu-id="84c13-132">Ange ett namn för ditt projekt och klicka sedan på **Next** (Nästa).</span><span class="sxs-lookup"><span data-stu-id="84c13-132">Enter a name for your project, and then click **Next**.</span></span>

    ![Service Fabric, ny projektsida 2][create-application/p2]

3.  <span data-ttu-id="84c13-134">Välj i hello lista över mallar **tjänstmall**.</span><span class="sxs-lookup"><span data-stu-id="84c13-134">In hello list of templates, select **Service Template**.</span></span> <span data-ttu-id="84c13-135">Välj typ av tjänstmall (Actor (Aktör), Stateless (Tillståndslös), Container (Behållare) eller Guest Binary (Gäst, binär)) och klicka sedan på **Next** (Nästa).</span><span class="sxs-lookup"><span data-stu-id="84c13-135">Select your service template type (Actor, Stateless, Container, or Guest Binary), and then click **Next**.</span></span>

    ![Service Fabric, ny projektsida 3][create-application/p3]

4.  <span data-ttu-id="84c13-137">Ange tjänstnamnet hello tjänsten information och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="84c13-137">Enter hello service name and service details, and then click **Finish**.</span></span>

    ![Service Fabric, ny projektsida 4][create-application/p4]

5. <span data-ttu-id="84c13-139">När du skapar din första Service Fabric-projekt i hello **öppna associerade perspektiv** dialogrutan klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="84c13-139">When you create your first Service Fabric project, in hello **Open Associated Perspective** dialog box, click **Yes**.</span></span>

    ![Service Fabric, ny projektsida 5][create-application/p5]

6.  <span data-ttu-id="84c13-141">Det nya projektet ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="84c13-141">Your new project looks like this:</span></span>

    ![Service Fabric, ny projektsida 6][create-application/p6]

## <a name="build-and-deploy-a-service-fabric-application-in-eclipse"></a><span data-ttu-id="84c13-143">Skapa och distribuera ett Service Fabric-program i Eclipse</span><span class="sxs-lookup"><span data-stu-id="84c13-143">Build and deploy a Service Fabric application in Eclipse</span></span>

1.  <span data-ttu-id="84c13-144">Högerklicka på det nya Service Fabric-programmet och välj sedan **Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="84c13-144">Right-click your new Service Fabric application, and then select **Service Fabric**.</span></span>

    ![Service Fabric-snabbmeny][publish/RightClick]

2. <span data-ttu-id="84c13-146">Välj hello-alternativ som du vill använda i hello undermenyn:</span><span class="sxs-lookup"><span data-stu-id="84c13-146">In hello submenu, select hello option you want:</span></span>
    -   <span data-ttu-id="84c13-147">toobuild hello program utan rensas, klicka på **bygga program**.</span><span class="sxs-lookup"><span data-stu-id="84c13-147">toobuild hello application without cleaning, click **Build Application**.</span></span>
    -   <span data-ttu-id="84c13-148">toodo en ren version av programmet hello, klickar du på **återskapa programmet**.</span><span class="sxs-lookup"><span data-stu-id="84c13-148">toodo a clean build of hello application, click **Rebuild Application**.</span></span>
    -   <span data-ttu-id="84c13-149">tooclean hello tillämpningen av inbyggda artefakter, klickar du på **ren programmet**.</span><span class="sxs-lookup"><span data-stu-id="84c13-149">tooclean hello application of built artifacts, click **Clean Application**.</span></span>

3.  <span data-ttu-id="84c13-150">Du kan också välja att distribuera, ta bort eller publicera programmet på den här menyn:</span><span class="sxs-lookup"><span data-stu-id="84c13-150">From this menu, you also can deploy, undeploy, and publish your application:</span></span>
    -   <span data-ttu-id="84c13-151">toodeploy tooyour lokala klustret och klicka på **distribuera programmet**.</span><span class="sxs-lookup"><span data-stu-id="84c13-151">toodeploy tooyour local cluster, click **Deploy Application**.</span></span>
    -   <span data-ttu-id="84c13-152">I hello **publicera programmet** dialogrutan väljer du en profil:</span><span class="sxs-lookup"><span data-stu-id="84c13-152">In hello **Publish Application** dialog box, select a publish profile:</span></span>
        -  <span data-ttu-id="84c13-153">**Local.json**</span><span class="sxs-lookup"><span data-stu-id="84c13-153">**Local.json**</span></span>
        -  <span data-ttu-id="84c13-154">**Cloud.json**</span><span class="sxs-lookup"><span data-stu-id="84c13-154">**Cloud.json**</span></span>

     <span data-ttu-id="84c13-155">Filerna JavaScript Object Notation (JSON) lagrar information (till exempel anslutningens slutpunkter och säkerhetsinformation) som är nödvändiga tooconnect tooyour lokala eller ett kluster i molnet (Azure).</span><span class="sxs-lookup"><span data-stu-id="84c13-155">These JavaScript Object Notation (JSON) files store information (such as connection endpoints and security information) that is required tooconnect tooyour local or cloud (Azure) cluster.</span></span>

  ![Publicera-menyn för Service Fabric][publish/Publish]

<span data-ttu-id="84c13-157">Ett annat sätt toodeploy Service Fabric-program genom att använda Eclipse körs konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="84c13-157">An alternate way toodeploy your Service Fabric application is by using Eclipse run configurations.</span></span>

  1.    <span data-ttu-id="84c13-158">Gå för**kör** > **kör konfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="84c13-158">Go too**Run** > **Run Configurations**.</span></span>
  2.    <span data-ttu-id="84c13-159">Under **Gradle projekt**väljer hello **ServiceFabricDeployer** kör konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="84c13-159">Under **Gradle Project**, select hello **ServiceFabricDeployer** run configuration.</span></span>
  3.    <span data-ttu-id="84c13-160">I hello högra fönstret på hello **argument** fliken för **publishProfile**väljer **lokala** eller **moln**.</span><span class="sxs-lookup"><span data-stu-id="84c13-160">In hello right pane, on hello **Arguments** tab, for **publishProfile**, select **local** or **cloud**.</span></span>  <span data-ttu-id="84c13-161">hello standardvärdet är **lokala**.</span><span class="sxs-lookup"><span data-stu-id="84c13-161">hello default is **local**.</span></span> <span data-ttu-id="84c13-162">toodeploy tooa remote eller molnet kluster, Välj **moln**.</span><span class="sxs-lookup"><span data-stu-id="84c13-162">toodeploy tooa remote or cloud cluster, select **cloud**.</span></span>
  4.    <span data-ttu-id="84c13-163">tooensure att hello rätt information fylls i hello publicera profiler, redigera **Local.json** eller **Cloud.json** efter behov.</span><span class="sxs-lookup"><span data-stu-id="84c13-163">tooensure that hello proper information is populated in hello publish profiles, edit **Local.json** or **Cloud.json** as needed.</span></span> <span data-ttu-id="84c13-164">Du kan lägga till eller uppdatera information om slutpunkt och säkerhetsreferenser.</span><span class="sxs-lookup"><span data-stu-id="84c13-164">You can add or update endpoint details and security credentials.</span></span>
  5.    <span data-ttu-id="84c13-165">Se till att **Working Directory** pekar toohello program som du vill toodeploy.</span><span class="sxs-lookup"><span data-stu-id="84c13-165">Ensure that **Working Directory** points toohello application you want toodeploy.</span></span> <span data-ttu-id="84c13-166">toochange Hej programmet, klickar du på hello **arbetsytan** och välj hello-program som du vill.</span><span class="sxs-lookup"><span data-stu-id="84c13-166">toochange hello application, click hello **Workspace** button, and then select hello application you want.</span></span>
  6.    <span data-ttu-id="84c13-167">Klicka på **Apply** (Verkställ) och sedan på **Run** (Kör).</span><span class="sxs-lookup"><span data-stu-id="84c13-167">Click **Apply**, and then click **Run**.</span></span>

<span data-ttu-id="84c13-168">Ditt program skapas och distribueras efter en liten stund.</span><span class="sxs-lookup"><span data-stu-id="84c13-168">Your application builds and deploys within a few moments.</span></span> <span data-ttu-id="84c13-169">Du kan övervaka hello Distributionsstatus i Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="84c13-169">You can monitor hello deployment status in Service Fabric Explorer.</span></span>  

## <a name="add-a-service-fabric-service-tooyour-service-fabric-application"></a><span data-ttu-id="84c13-170">Lägg till ett Service Fabric-tjänsten tooyour Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="84c13-170">Add a Service Fabric service tooyour Service Fabric application</span></span>

<span data-ttu-id="84c13-171">tooadd ett Service Fabric-tooan befintliga Service Fabric tjänstprogram, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="84c13-171">tooadd a Service Fabric service tooan existing Service Fabric application, do hello following steps:</span></span>

1.  <span data-ttu-id="84c13-172">Högerklicka på hello projekt du tooadd en tjänst, och klickar sedan på **Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="84c13-172">Right-click hello project you want tooadd a service to, and then click **Service Fabric**.</span></span>

    ![Service Fabric, lägg till tjänst, sida 1][add-service/p1]

2.  <span data-ttu-id="84c13-174">Klicka på **lägga till Service Fabric Service**, och fullständig hello uppsättning steg tooadd en toohello service-projekt.</span><span class="sxs-lookup"><span data-stu-id="84c13-174">Click **Add Service Fabric Service**, and complete hello set of steps tooadd a service toohello project.</span></span>
3.  <span data-ttu-id="84c13-175">Välj hello tjänstmall du vill tooadd tooyour projektet och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="84c13-175">Select hello service template you want tooadd tooyour project, and then click **Next**.</span></span>

    ![Service Fabric, lägg till tjänst, sida 2][add-service/p2]

4.  <span data-ttu-id="84c13-177">Ange hello tjänstnamn (och annan information som behövs) och klicka sedan på hello **lägga till tjänsten** knappen.</span><span class="sxs-lookup"><span data-stu-id="84c13-177">Enter hello service name (and other details, as needed), and then click hello **Add Service** button.</span></span>  

    ![Service Fabric, lägg till tjänst, sida 3][add-service/p3]

5.  <span data-ttu-id="84c13-179">När hello-tjänsten har lagts ut din övergripande projektstruktur liknande toohello följande projekt:</span><span class="sxs-lookup"><span data-stu-id="84c13-179">After hello service is added, your overall project structure looks similar toohello following project:</span></span>

    ![Service Fabric, lägg till tjänst, sida 4][add-service/p4]

## <a name="edit-manifest-versions-of-your-service-fabric-java-application"></a><span data-ttu-id="84c13-181">Redigera manifestversioner för Service Fabric Java-programmet</span><span class="sxs-lookup"><span data-stu-id="84c13-181">Edit Manifest versions of your Service Fabric Java application</span></span>

<span data-ttu-id="84c13-182">tooedit manifestet versioner, högerklicka på projektet hello finns för**Service Fabric** och välj **redigera Manifest versioner...**  från hello menyn för verben.</span><span class="sxs-lookup"><span data-stu-id="84c13-182">tooedit manifest versions, right click on hello project, go too**Service Fabric** and select **Edit Manifest Versions...** from hello menu dropdown.</span></span> <span data-ttu-id="84c13-183">Hello i guiden kan du uppdatera hello manifest för programmet manifestet, tjänstmanifestet och hello versioner för **kod**, **Config** och **Data** paket.</span><span class="sxs-lookup"><span data-stu-id="84c13-183">In hello wizard, you can update hello manifest versions for application manifest, service manifest and hello versions for **Code**, **Config** and **Data** packages.</span></span>

<span data-ttu-id="84c13-184">Om du markerar alternativet hello **automatiskt uppdatera programmet och service versioner** och uppdatera sedan en version, sedan hello manifestet versioner uppdateras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="84c13-184">If you check hello option **Automatically update application and service versions** and then update a version, then hello manifest versions will be automatically updated.</span></span> <span data-ttu-id="84c13-185">toogive ett exempel du först välja hello kryssrutan och sedan uppdatera hello version av **kod** version från 0.0.0 too0.0.1 och klicka på **Slutför**, service manifest version och programmanifestet version kommer att uppdateras automatiskt too0.0.1.</span><span class="sxs-lookup"><span data-stu-id="84c13-185">toogive an example, you first select hello check-box, then update hello version of **Code** version from 0.0.0 too0.0.1 and click on **Finish**, then service manifest version and application manifest version will be automatically updated too0.0.1.</span></span>

## <a name="upgrade-your-service-fabric-java-application"></a><span data-ttu-id="84c13-186">Uppgradera ditt Service Fabric Java-program</span><span class="sxs-lookup"><span data-stu-id="84c13-186">Upgrade your Service Fabric Java application</span></span>

<span data-ttu-id="84c13-187">För en uppgraderingsscenariot säger du skapade hello **App1** projekt genom att använda hello Service Fabric-plugin-programmet i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="84c13-187">For an upgrade scenario, say you created hello **App1** project by using hello Service Fabric plug-in in Eclipse.</span></span> <span data-ttu-id="84c13-188">Du har distribuerat den med hjälp av plugin-programmet hello-toocreate ett program med namnet **fabric: / App1Application**.</span><span class="sxs-lookup"><span data-stu-id="84c13-188">You deployed it by using hello plug-in toocreate an application named **fabric:/App1Application**.</span></span> <span data-ttu-id="84c13-189">hello programtypen är **App1AppicationType**, och hello programmet version 1.0.</span><span class="sxs-lookup"><span data-stu-id="84c13-189">hello application type is **App1AppicationType**, and hello application version is 1.0.</span></span> <span data-ttu-id="84c13-190">Nu ska tooupgrade programmet utan att påverka tillgängligheten.</span><span class="sxs-lookup"><span data-stu-id="84c13-190">Now, you want tooupgrade your application without interrupting availability.</span></span>

<span data-ttu-id="84c13-191">Först gör ändringar tooyour program och sedan återskapa hello ändras service.</span><span class="sxs-lookup"><span data-stu-id="84c13-191">First, make any changes tooyour application, and then rebuild hello modified service.</span></span> <span data-ttu-id="84c13-192">Uppdatera hello ändrade tjänstens manifestfilen (ServiceManifest.xml) med hello uppdaterade versioner för hello-tjänsten (och koden, Config eller Data som är aktuellt).</span><span class="sxs-lookup"><span data-stu-id="84c13-192">Update hello modified service’s manifest file (ServiceManifest.xml) with hello updated versions for hello service (and Code, Config, or Data, as relevant).</span></span> <span data-ttu-id="84c13-193">Dessutom ändra hello programmets manifest (ApplicationManifest.xml) med hello uppdatera versionsnumret för programmet hello och hello ändrade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="84c13-193">Also, modify hello application’s manifest (ApplicationManifest.xml) with hello updated version number for hello application and hello modified service.</span></span>  

<span data-ttu-id="84c13-194">tooupgrade ditt program genom att använda Eclipse Neon som du kan skapa en duplicerad kör Konfigurationsprofil.</span><span class="sxs-lookup"><span data-stu-id="84c13-194">tooupgrade your application by using Eclipse Neon, you can create a duplicate run configuration profile.</span></span> <span data-ttu-id="84c13-195">Använd sedan tooupgrade programmet efter behov.</span><span class="sxs-lookup"><span data-stu-id="84c13-195">Then, use it tooupgrade your application as needed.</span></span>

1.  <span data-ttu-id="84c13-196">Gå för**kör** > **kör konfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="84c13-196">Go too**Run** > **Run Configurations**.</span></span> <span data-ttu-id="84c13-197">Klicka på hello liten pil toohello till vänster i hello vänster, **Gradle projekt**.</span><span class="sxs-lookup"><span data-stu-id="84c13-197">In hello left pane, click hello small arrow toohello left of **Gradle Project**.</span></span>
2.  <span data-ttu-id="84c13-198">Högerklicka på **ServiceFabricDeployer** och välj sedan **Duplicate** (Duplicera).</span><span class="sxs-lookup"><span data-stu-id="84c13-198">Right-click **ServiceFabricDeployer**, and then select **Duplicate**.</span></span> <span data-ttu-id="84c13-199">Ange ett nytt namn för den här konfigurationen, till exempel **ServiceFabricUpgrader**.</span><span class="sxs-lookup"><span data-stu-id="84c13-199">Enter a new name for this configuration, for example, **ServiceFabricUpgrader**.</span></span>
3.  <span data-ttu-id="84c13-200">I hello högra panelen på hello **argument** fliken, ändrar **- Pconfig = 'distribuera'** för**- Pconfig = 'uppgradera'**, och klicka sedan på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="84c13-200">In hello right panel, on hello **Arguments** tab, change **-Pconfig='deploy'** too**-Pconfig='upgrade'**, and then click **Apply**.</span></span>

<span data-ttu-id="84c13-201">Den här processen skapar och sparar en Konfigurationsprofil som kör du någon gång tooupgrade kan använda programmet.</span><span class="sxs-lookup"><span data-stu-id="84c13-201">This process creates and saves a run configuration profile you can use at any time tooupgrade your application.</span></span> <span data-ttu-id="84c13-202">Hello senaste uppdaterade programmets Typversion får också från hello programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="84c13-202">It also gets hello latest updated application type version from hello application manifest file.</span></span>

<span data-ttu-id="84c13-203">uppgradering av programmet hello tar några minuter.</span><span class="sxs-lookup"><span data-stu-id="84c13-203">hello application upgrade takes a few minutes.</span></span> <span data-ttu-id="84c13-204">Du kan övervaka hello programmet uppgradering i Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="84c13-204">You can monitor hello application upgrade in Service Fabric Explorer.</span></span>

## <a name="migrating-old-service-fabric-java-applications-toobe-used-with-maven"></a><span data-ttu-id="84c13-205">Migrera gamla Service Fabric Java-program toobe används med Maven</span><span class="sxs-lookup"><span data-stu-id="84c13-205">Migrating old Service Fabric Java applications toobe used with Maven</span></span>
<span data-ttu-id="84c13-206">Vi har nyligen flyttat Service Fabric Java-bibliotek från Service Fabric Java SDK tooMaven databasen.</span><span class="sxs-lookup"><span data-stu-id="84c13-206">We have recently moved Service Fabric Java libraries from Service Fabric Java SDK tooMaven repository.</span></span> <span data-ttu-id="84c13-207">Medan hello nya program som du skapar med Eclipse genererar senaste uppdaterade projekt (som kommer att kunna toowork med Maven), kan du uppdatera din befintliga Service Fabric tillståndslös eller aktören Java-program som använder hello Service Fabric Java SDK tidigare, toouse hello Service Fabric Java beroenden från Maven.</span><span class="sxs-lookup"><span data-stu-id="84c13-207">While hello new applications you generate using Eclipse, will generate latest updated projects (which will be able toowork with Maven), you can update your existing Service Fabric stateless or actor Java applications, which were using hello Service Fabric Java SDK earlier, toouse hello Service Fabric Java dependencies from Maven.</span></span> <span data-ttu-id="84c13-208">Följ anvisningarna för hello [här](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure äldre programmet fungerar med Maven.</span><span class="sxs-lookup"><span data-stu-id="84c13-208">Please follow hello steps mentioned [here](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure your older application works with Maven.</span></span>

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-eclipse/service-fabric-eclipse-plugin.png

[create-application/p1]:./media/service-fabric-get-started-eclipse/create-application/p1.png
[create-application/p2]:./media/service-fabric-get-started-eclipse/create-application/p2.png
[create-application/p3]:./media/service-fabric-get-started-eclipse/create-application/p3.png
[create-application/p4]:./media/service-fabric-get-started-eclipse/create-application/p4.png
[create-application/p5]:./media/service-fabric-get-started-eclipse/create-application/p5.png
[create-application/p6]:./media/service-fabric-get-started-eclipse/create-application/p6.png

[publish/Publish]: ./media/service-fabric-get-started-eclipse/publish/Publish.png
[publish/RightClick]: ./media/service-fabric-get-started-eclipse/publish/RightClick.png

[add-service/p1]: ./media/service-fabric-get-started-eclipse/add-service/p1.png
[add-service/p2]: ./media/service-fabric-get-started-eclipse/add-service/p2.png
[add-service/p3]: ./media/service-fabric-get-started-eclipse/add-service/p3.png
[add-service/p4]: ./media/service-fabric-get-started-eclipse/add-service/p4.png

<!-- Links -->
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
