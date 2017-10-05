---
title: "Azure Service Fabric-plugin-program för Eclipse | Microsoft Docs"
description: "Kom igång med Service Fabric-plugin-programmet för Eclipse."
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
ms.openlocfilehash: 98c1b99972b9ad7a396d72b98e727286f6822e42
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="service-fabric-plug-in-for-eclipse-java-application-development"></a><span data-ttu-id="de650-103">Service Fabric-plugin-program för utveckling av Java-program i Eclipse</span><span class="sxs-lookup"><span data-stu-id="de650-103">Service Fabric plug-in for Eclipse Java application development</span></span>
<span data-ttu-id="de650-104">Eclipse är en av de mest använda IDE:erna (Integrated Development Environment) för Java-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="de650-104">Eclipse is one of the most widely used integrated development environments (IDEs) for Java developers.</span></span> <span data-ttu-id="de650-105">I den här artikeln beskrivs hur du kan konfigurera din Eclipse-utvecklingsmiljö för att arbeta med Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="de650-105">In this article, we describe how to set up your Eclipse development environment to work with Azure Service Fabric.</span></span> <span data-ttu-id="de650-106">Läs om hur du installerar Service Fabric-plugin-programmet, skapar ett Service Fabric-program och distribuerar Service Fabric-programmet till ett lokalt eller fjärranslutet Service Fabric-kluster i Eclipse Neon.</span><span class="sxs-lookup"><span data-stu-id="de650-106">Learn how to install the Service Fabric plug-in, create a Service Fabric application, and deploy your Service Fabric application to a local or remote Service Fabric cluster in Eclipse Neon.</span></span>

## <a name="install-or-update-the-service-fabric-plug-in-in-eclipse-neon"></a><span data-ttu-id="de650-107">Installera eller uppdatera Service Fabric-plugin-programmet i Eclipse Neon</span><span class="sxs-lookup"><span data-stu-id="de650-107">Install or update the Service Fabric plug-in in Eclipse Neon</span></span>
<span data-ttu-id="de650-108">Du kan installera ett Service Fabric-plugin-program i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="de650-108">You can install a Service Fabric plug-in in Eclipse.</span></span> <span data-ttu-id="de650-109">Plugin-programmet gör det enklare att skapa och distribuera Java-tjänster.</span><span class="sxs-lookup"><span data-stu-id="de650-109">The plug-in can help simplify the process of building and deploying Java services.</span></span>

1.  <span data-ttu-id="de650-110">Kontrollera att du har installerat den senaste versionen av Eclipse Neon och den senaste versionen av Buildship (version 1.0.17 eller senare):</span><span class="sxs-lookup"><span data-stu-id="de650-110">Ensure that you have the latest version of Eclipse Neon and the latest version of Buildship (1.0.17 or a later version) installed:</span></span>
    -   <span data-ttu-id="de650-111">Du kan kontrollera vilka versioner de installerade komponenterna har genom att välja **Help** > **Installation Details** (Hjälp > Installationsinformation) i Eclipse Neon.</span><span class="sxs-lookup"><span data-stu-id="de650-111">To check the versions of installed components, in Eclipse Neon, go to **Help** > **Installation Details**.</span></span>
    -   <span data-ttu-id="de650-112">Om du vill uppdatera Buildship kan du läsa [Eclipse Buildship: Eclipse-plugin-program för Gradle][buildship-update] (på engelska).</span><span class="sxs-lookup"><span data-stu-id="de650-112">To update Buildship, see [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update].</span></span>
    -   <span data-ttu-id="de650-113">Om du vill söka efter och installera uppdateringar för Eclipse Neon kan du gå till **Help** > **Check for Updates** (Hjälp > Sök efter uppdateringar).</span><span class="sxs-lookup"><span data-stu-id="de650-113">To check for and install updates for Eclipse Neon, go to **Help** > **Check for Updates**.</span></span>

2.  <span data-ttu-id="de650-114">Om du vill installera Service Fabric-plugin-programmet går du till **Help** > **Install New Software** (Hjälp > Installera ny programvara) i Eclipse Neon.</span><span class="sxs-lookup"><span data-stu-id="de650-114">To install the Service Fabric plug-in, in Eclipse Neon, go to **Help** > **Install New Software**.</span></span>
  1.    <span data-ttu-id="de650-115">Ange **http://dl.microsoft.com/eclipse** i textrutan **Arbeta med**.</span><span class="sxs-lookup"><span data-stu-id="de650-115">In the **Work with** box, enter **http://dl.microsoft.com/eclipse**.</span></span>
  2.    <span data-ttu-id="de650-116">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="de650-116">Click **Add**.</span></span>

         ![Service Fabric-plugin-program för Eclipse Neon][sf-eclipse-plugin-install]
  3.    <span data-ttu-id="de650-118">Välj Service Fabric-plugin-programmet och klicka sedan på **Next** (Nästa).</span><span class="sxs-lookup"><span data-stu-id="de650-118">Select the Service Fabric plug-in, and then click **Next**.</span></span>
  4.    <span data-ttu-id="de650-119">Slutför installationsstegen och acceptera licensvillkoren för programvara från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="de650-119">Complete the installation steps, and then accept the Microsoft Software License Terms.</span></span>

<span data-ttu-id="de650-120">Om du redan har Service Fabric-plugin-programmet installerat kontrollerar du att du har den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="de650-120">If you already have the Service Fabric plug-in installed, make sure that you have the latest version.</span></span> <span data-ttu-id="de650-121">Om du vill söka efter uppdateringar går du till **Help** > **Installation Details** (Hjälp > Installationsinformation).</span><span class="sxs-lookup"><span data-stu-id="de650-121">To check for available updates, go to **Help** > **Installation Details**.</span></span> <span data-ttu-id="de650-122">Välj Service Fabric i listan över installerade plugin-program och klicka sedan på **Update** (Uppdatera).</span><span class="sxs-lookup"><span data-stu-id="de650-122">In the list of installed plug-ins, select Service Fabric, and then click **Update**.</span></span> <span data-ttu-id="de650-123">Tillgängliga uppdateringar installeras.</span><span class="sxs-lookup"><span data-stu-id="de650-123">Available updates will be installed.</span></span>

> [!NOTE]
> <span data-ttu-id="de650-124">Om installationen eller uppdateringen av Service Fabric-plugin-programmet är långsam kan det bero på en Eclipse-inställning.</span><span class="sxs-lookup"><span data-stu-id="de650-124">If installing or updating the Service Fabric plug-in is slow, it might be due to an Eclipse setting.</span></span> <span data-ttu-id="de650-125">Eclipse samlar in metadata om alla ändringar på uppdateringsplatser som är registrerade med din Eclipse-instans.</span><span class="sxs-lookup"><span data-stu-id="de650-125">Eclipse collects metadata on all changes to update sites that are registered with your Eclipse instance.</span></span> <span data-ttu-id="de650-126">Om du vill påskynda sökningen efter och installationen av uppdateringen av Service Fabric-plugin-programmet kan du gå till **Available Software Sites** (Platser med tillgänglig programvara).</span><span class="sxs-lookup"><span data-stu-id="de650-126">To speed up the process of checking for and installing a Service Fabric plug-in update, go to **Available Software Sites**.</span></span> <span data-ttu-id="de650-127">Avmarkera kryssrutorna för alla platser utom den som pekar på platsen för Service Fabric-plugin-programmet (http://dl.microsoft.com/eclipse/azure/servicefabric).</span><span class="sxs-lookup"><span data-stu-id="de650-127">Clear the check boxes for all sites except for the one that points to the Service Fabric plug-in location (http://dl.microsoft.com/eclipse/azure/servicefabric).</span></span>

## <a name="create-a-service-fabric-application-in-eclipse"></a><span data-ttu-id="de650-128">Skapa ett Service Fabric-program i Eclipse</span><span class="sxs-lookup"><span data-stu-id="de650-128">Create a Service Fabric application in Eclipse</span></span>

1.  <span data-ttu-id="de650-129">Gå till **File** > **New** > **Other** (Arkiv > Nytt > Annat) i Eclipse Neon.</span><span class="sxs-lookup"><span data-stu-id="de650-129">In Eclipse Neon, go to **File** > **New** > **Other**.</span></span> <span data-ttu-id="de650-130">Välj **Service Fabric Project** (Service Fabric Project) och klicka sedan på **Next** (Nästa).</span><span class="sxs-lookup"><span data-stu-id="de650-130">Select  **Service Fabric Project**, and then click **Next**.</span></span>

    ![Service Fabric, ny projektsida 1][create-application/p1]

2.  <span data-ttu-id="de650-132">Ange ett namn för ditt projekt och klicka sedan på **Next** (Nästa).</span><span class="sxs-lookup"><span data-stu-id="de650-132">Enter a name for your project, and then click **Next**.</span></span>

    ![Service Fabric, ny projektsida 2][create-application/p2]

3.  <span data-ttu-id="de650-134">Välj **Service Template** (Tjänstmall) i listan med mallar.</span><span class="sxs-lookup"><span data-stu-id="de650-134">In the list of templates, select **Service Template**.</span></span> <span data-ttu-id="de650-135">Välj typ av tjänstmall (Actor (Aktör), Stateless (Tillståndslös), Container (Behållare) eller Guest Binary (Gäst, binär)) och klicka sedan på **Next** (Nästa).</span><span class="sxs-lookup"><span data-stu-id="de650-135">Select your service template type (Actor, Stateless, Container, or Guest Binary), and then click **Next**.</span></span>

    ![Service Fabric, ny projektsida 3][create-application/p3]

4.  <span data-ttu-id="de650-137">Ange namnet på tjänsten och information om tjänsten och klicka sedan på **Finish** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="de650-137">Enter the service name and service details, and then click **Finish**.</span></span>

    ![Service Fabric, ny projektsida 4][create-application/p4]

5. <span data-ttu-id="de650-139">När du skapar ditt första Service Fabric-projekt klickar du på **Yes** (Ja) i dialogrutan **Open Associated Perspective** (Öppna associerat perspektiv).</span><span class="sxs-lookup"><span data-stu-id="de650-139">When you create your first Service Fabric project, in the **Open Associated Perspective** dialog box, click **Yes**.</span></span>

    ![Service Fabric, ny projektsida 5][create-application/p5]

6.  <span data-ttu-id="de650-141">Det nya projektet ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="de650-141">Your new project looks like this:</span></span>

    ![Service Fabric, ny projektsida 6][create-application/p6]

## <a name="build-and-deploy-a-service-fabric-application-in-eclipse"></a><span data-ttu-id="de650-143">Skapa och distribuera ett Service Fabric-program i Eclipse</span><span class="sxs-lookup"><span data-stu-id="de650-143">Build and deploy a Service Fabric application in Eclipse</span></span>

1.  <span data-ttu-id="de650-144">Högerklicka på det nya Service Fabric-programmet och välj sedan **Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="de650-144">Right-click your new Service Fabric application, and then select **Service Fabric**.</span></span>

    ![Service Fabric-snabbmeny][publish/RightClick]

2. <span data-ttu-id="de650-146">Välj önskat alternativ på undermenyn:</span><span class="sxs-lookup"><span data-stu-id="de650-146">In the submenu, select the option you want:</span></span>
    -   <span data-ttu-id="de650-147">Klicka på **Build Application** (Bygg program) om du vill skapa programmet utan rensning.</span><span class="sxs-lookup"><span data-stu-id="de650-147">To build the application without cleaning, click **Build Application**.</span></span>
    -   <span data-ttu-id="de650-148">Klicka på **Rebuild Application** (Bygg om program) om du vill skapa en rensad version av programmet.</span><span class="sxs-lookup"><span data-stu-id="de650-148">To do a clean build of the application, click **Rebuild Application**.</span></span>
    -   <span data-ttu-id="de650-149">Klicka på **Clean Application** (Rensa program) om du vill rensa bort byggda artefakter i programmet.</span><span class="sxs-lookup"><span data-stu-id="de650-149">To clean the application of built artifacts, click **Clean Application**.</span></span>

3.  <span data-ttu-id="de650-150">Du kan också välja att distribuera, ta bort eller publicera programmet på den här menyn:</span><span class="sxs-lookup"><span data-stu-id="de650-150">From this menu, you also can deploy, undeploy, and publish your application:</span></span>
    -   <span data-ttu-id="de650-151">Klicka på **Deploy Application** (Distribuera program) om du vill distribuera till ditt lokala kluster.</span><span class="sxs-lookup"><span data-stu-id="de650-151">To deploy to your local cluster, click **Deploy Application**.</span></span>
    -   <span data-ttu-id="de650-152">Välj en publiceringsprofil i dialogrutan **Publish Application** (Publicera program):</span><span class="sxs-lookup"><span data-stu-id="de650-152">In the **Publish Application** dialog box, select a publish profile:</span></span>
        -  <span data-ttu-id="de650-153">**Local.json**</span><span class="sxs-lookup"><span data-stu-id="de650-153">**Local.json**</span></span>
        -  <span data-ttu-id="de650-154">**Cloud.json**</span><span class="sxs-lookup"><span data-stu-id="de650-154">**Cloud.json**</span></span>

     <span data-ttu-id="de650-155">De här JSON-filerna används för att lagra information (som anslutningsslutpunkter och säkerhetsinformation) som krävs för att ansluta till ett lokalt kluster eller ett molnkluster (Azure).</span><span class="sxs-lookup"><span data-stu-id="de650-155">These JavaScript Object Notation (JSON) files store information (such as connection endpoints and security information) that is required to connect to your local or cloud (Azure) cluster.</span></span>

  ![Publicera-menyn för Service Fabric][publish/Publish]

<span data-ttu-id="de650-157">Du kan också distribuera Service Fabric-programmet med Run Configurations (Kör konfigurationer) i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="de650-157">An alternate way to deploy your Service Fabric application is by using Eclipse run configurations.</span></span>

  1.    <span data-ttu-id="de650-158">Välj **Run** > **Run Configurations** (Kör > Kör konfigurationer).</span><span class="sxs-lookup"><span data-stu-id="de650-158">Go to **Run** > **Run Configurations**.</span></span>
  2.    <span data-ttu-id="de650-159">Välj **ServiceFabricDeployer** under **Gradle Project** (Gradle-projekt).</span><span class="sxs-lookup"><span data-stu-id="de650-159">Under **Gradle Project**, select the **ServiceFabricDeployer** run configuration.</span></span>
  3.    <span data-ttu-id="de650-160">Välj **local** (lokalt) eller **cloud** (moln) i den högra rutan på fliken **Arguments** (Argument) för **publishProfile**.</span><span class="sxs-lookup"><span data-stu-id="de650-160">In the right pane, on the **Arguments** tab, for **publishProfile**, select **local** or **cloud**.</span></span>  <span data-ttu-id="de650-161">Standardvärdet är **local** (lokalt).</span><span class="sxs-lookup"><span data-stu-id="de650-161">The default is **local**.</span></span> <span data-ttu-id="de650-162">Om du distribuerar till ett fjärr- eller molnkluster väljer du **cloud** (moln).</span><span class="sxs-lookup"><span data-stu-id="de650-162">To deploy to a remote or cloud cluster, select **cloud**.</span></span>
  4.    <span data-ttu-id="de650-163">För att säkerställa att rätt information anges i publiceringsprofilerna redigerar du **Local.json** eller **Cloud.json** efter behov.</span><span class="sxs-lookup"><span data-stu-id="de650-163">To ensure that the proper information is populated in the publish profiles, edit **Local.json** or **Cloud.json** as needed.</span></span> <span data-ttu-id="de650-164">Du kan lägga till eller uppdatera information om slutpunkt och säkerhetsreferenser.</span><span class="sxs-lookup"><span data-stu-id="de650-164">You can add or update endpoint details and security credentials.</span></span>
  5.    <span data-ttu-id="de650-165">Kontrollera att **Working Directory** (Arbetskatalog) pekar på det program som du vill distribuera.</span><span class="sxs-lookup"><span data-stu-id="de650-165">Ensure that **Working Directory** points to the application you want to deploy.</span></span> <span data-ttu-id="de650-166">Om du vill ändra program klickar du på knappen **Workspace** (Arbetsyta) och väljer önskat program.</span><span class="sxs-lookup"><span data-stu-id="de650-166">To change the application, click the **Workspace** button, and then select the application you want.</span></span>
  6.    <span data-ttu-id="de650-167">Klicka på **Apply** (Verkställ) och sedan på **Run** (Kör).</span><span class="sxs-lookup"><span data-stu-id="de650-167">Click **Apply**, and then click **Run**.</span></span>

<span data-ttu-id="de650-168">Ditt program skapas och distribueras efter en liten stund.</span><span class="sxs-lookup"><span data-stu-id="de650-168">Your application builds and deploys within a few moments.</span></span> <span data-ttu-id="de650-169">Du kan övervaka distributionsstatus i Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="de650-169">You can monitor the deployment status in Service Fabric Explorer.</span></span>  

## <a name="add-a-service-fabric-service-to-your-service-fabric-application"></a><span data-ttu-id="de650-170">Lägga till en Service Fabric-tjänst till ditt Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="de650-170">Add a Service Fabric service to your Service Fabric application</span></span>

<span data-ttu-id="de650-171">Du kan lägga till en Service Fabric-tjänst till ett befintligt Service Fabric-program på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="de650-171">To add a Service Fabric service to an existing Service Fabric application, do the following steps:</span></span>

1.  <span data-ttu-id="de650-172">Högerklicka på det projekt som du vill lägga till en tjänst för och klicka sedan på **Service Fabric**.</span><span class="sxs-lookup"><span data-stu-id="de650-172">Right-click the project you want to add a service to, and then click **Service Fabric**.</span></span>

    ![Service Fabric, lägg till tjänst, sida 1][add-service/p1]

2.  <span data-ttu-id="de650-174">Klicka på **Add Service Fabric Service** (Lägg till Service Fabric-tjänst) och slutför stegen för att lägga till en tjänst i projektet.</span><span class="sxs-lookup"><span data-stu-id="de650-174">Click **Add Service Fabric Service**, and complete the set of steps to add a service to the project.</span></span>
3.  <span data-ttu-id="de650-175">Välj den tjänstmall som du vill lägga till för projektet och klicka sedan på **Next** (Nästa).</span><span class="sxs-lookup"><span data-stu-id="de650-175">Select the service template you want to add to your project, and then click **Next**.</span></span>

    ![Service Fabric, lägg till tjänst, sida 2][add-service/p2]

4.  <span data-ttu-id="de650-177">Ange namnet på tjänsten (och andra uppgifter om det behövs) och klicka på knappen **Add Service** (Lägg till tjänst).</span><span class="sxs-lookup"><span data-stu-id="de650-177">Enter the service name (and other details, as needed), and then click the **Add Service** button.</span></span>  

    ![Service Fabric, lägg till tjänst, sida 3][add-service/p3]

5.  <span data-ttu-id="de650-179">När tjänsten har lagts till ser projektstrukturen ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="de650-179">After the service is added, your overall project structure looks similar to the following project:</span></span>

    ![Service Fabric, lägg till tjänst, sida 4][add-service/p4]

## <a name="edit-manifest-versions-of-your-service-fabric-java-application"></a><span data-ttu-id="de650-181">Redigera manifestversioner för Service Fabric Java-programmet</span><span class="sxs-lookup"><span data-stu-id="de650-181">Edit Manifest versions of your Service Fabric Java application</span></span>

<span data-ttu-id="de650-182">Om du vill redigera manifestversioner högerklickar du på projektet, går till **Service Fabric** och väljer **Edit Manifest Versions** (Redigera manifestversioner) från listrutan.</span><span class="sxs-lookup"><span data-stu-id="de650-182">To edit manifest versions, right click on the project, go to **Service Fabric** and select **Edit Manifest Versions...** from the menu dropdown.</span></span> <span data-ttu-id="de650-183">I guiden kan du uppdatera manifestversioner för programmanifestet, tjänstmanifestet och versioner för paketen **Kod**, **Konfig** och **Data**.</span><span class="sxs-lookup"><span data-stu-id="de650-183">In the wizard, you can update the manifest versions for application manifest, service manifest and the versions for **Code**, **Config** and **Data** packages.</span></span>

<span data-ttu-id="de650-184">Om du markerar alternativet **Automatically update application and service versions** (Uppdatera versioner av program och tjänster automatiskt) och sedan uppdaterar en version så uppdateras manifestversionerna automatiskt.</span><span class="sxs-lookup"><span data-stu-id="de650-184">If you check the option **Automatically update application and service versions** and then update a version, then the manifest versions will be automatically updated.</span></span> <span data-ttu-id="de650-185">Säg att du först markerar kryssrutan och sedan uppdaterar versionen **Kod**-versionen från 0.0.0 till 0.0.1 och klickar på **Slutför**. Då uppdateras tjänstemanifestversionen och programmanifestversionen automatiskt till 0.0.1.</span><span class="sxs-lookup"><span data-stu-id="de650-185">To give an example, you first select the check-box, then update the version of **Code** version from 0.0.0 to 0.0.1 and click on **Finish**, then service manifest version and application manifest version will be automatically updated to 0.0.1.</span></span>

## <a name="upgrade-your-service-fabric-java-application"></a><span data-ttu-id="de650-186">Uppgradera ditt Service Fabric Java-program</span><span class="sxs-lookup"><span data-stu-id="de650-186">Upgrade your Service Fabric Java application</span></span>

<span data-ttu-id="de650-187">Anta att du har ett projekt som heter **App1** som du har skapat med Service Fabric-plugin-programmet i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="de650-187">For an upgrade scenario, say you created the **App1** project by using the Service Fabric plug-in in Eclipse.</span></span> <span data-ttu-id="de650-188">För att distribuera projektet skapade du ett program med namnet **fabric:/App1Application** med hjälp av plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="de650-188">You deployed it by using the plug-in to create an application named **fabric:/App1Application**.</span></span> <span data-ttu-id="de650-189">Programtypen är **App1AppicationType** och programversionen är 1.0.</span><span class="sxs-lookup"><span data-stu-id="de650-189">The application type is **App1AppicationType**, and the application version is 1.0.</span></span> <span data-ttu-id="de650-190">Nu vill du uppgradera programmet utan att det påverkar tillgängligheten.</span><span class="sxs-lookup"><span data-stu-id="de650-190">Now, you want to upgrade your application without interrupting availability.</span></span>

<span data-ttu-id="de650-191">Gör ändringar i programmet och bygg sedan den ändrade tjänsten på nytt.</span><span class="sxs-lookup"><span data-stu-id="de650-191">First, make any changes to your application, and then rebuild the modified service.</span></span> <span data-ttu-id="de650-192">Uppdatera manifestfilen (ServiceManifest.xml) för den ändrade tjänsten med de uppdaterade versionerna för tjänsten (samt kod, konfig eller data, om det behövs).</span><span class="sxs-lookup"><span data-stu-id="de650-192">Update the modified service’s manifest file (ServiceManifest.xml) with the updated versions for the service (and Code, Config, or Data, as relevant).</span></span> <span data-ttu-id="de650-193">Ändra också programmets manifest (ApplicationManifest.xml) med det uppdaterade versionsnumret för programmet och den ändrade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="de650-193">Also, modify the application’s manifest (ApplicationManifest.xml) with the updated version number for the application and the modified service.</span></span>  

<span data-ttu-id="de650-194">Om du vill uppgradera programmet med Eclipse Neon kan du skapa en duplicerad körningskonfigurationsprofil.</span><span class="sxs-lookup"><span data-stu-id="de650-194">To upgrade your application by using Eclipse Neon, you can create a duplicate run configuration profile.</span></span> <span data-ttu-id="de650-195">Sedan kan du använda den för att uppgradera ditt program efter behov.</span><span class="sxs-lookup"><span data-stu-id="de650-195">Then, use it to upgrade your application as needed.</span></span>

1.  <span data-ttu-id="de650-196">Välj **Run** > **Run Configurations** (Kör > Kör konfigurationer).</span><span class="sxs-lookup"><span data-stu-id="de650-196">Go to **Run** > **Run Configurations**.</span></span> <span data-ttu-id="de650-197">Klicka på den lilla pilen till vänster om **Gradle Project** (Gradle-projekt) i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="de650-197">In the left pane, click the small arrow to the left of **Gradle Project**.</span></span>
2.  <span data-ttu-id="de650-198">Högerklicka på **ServiceFabricDeployer** och välj sedan **Duplicate** (Duplicera).</span><span class="sxs-lookup"><span data-stu-id="de650-198">Right-click **ServiceFabricDeployer**, and then select **Duplicate**.</span></span> <span data-ttu-id="de650-199">Ange ett nytt namn för den här konfigurationen, till exempel **ServiceFabricUpgrader**.</span><span class="sxs-lookup"><span data-stu-id="de650-199">Enter a new name for this configuration, for example, **ServiceFabricUpgrader**.</span></span>
3.  <span data-ttu-id="de650-200">På den högra panelen på fliken **Argument** ändrar du **-Pconfig='deploy'** till **-Pconfig='upgrade'** och klickar på **Apply** (Verkställ).</span><span class="sxs-lookup"><span data-stu-id="de650-200">In the right panel, on the **Arguments** tab, change **-Pconfig='deploy'** to **-Pconfig='upgrade'**, and then click **Apply**.</span></span>

<span data-ttu-id="de650-201">Därmed skapas och sparas en körningskonfigurationsprofil som du när som helst kan använda till att uppgradera programmet.</span><span class="sxs-lookup"><span data-stu-id="de650-201">This process creates and saves a run configuration profile you can use at any time to upgrade your application.</span></span> <span data-ttu-id="de650-202">Då får du också den senast uppdaterade programtypversionen från programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="de650-202">It also gets the latest updated application type version from the application manifest file.</span></span>

<span data-ttu-id="de650-203">Det tar ett par minuter att uppgradera programmet.</span><span class="sxs-lookup"><span data-stu-id="de650-203">The application upgrade takes a few minutes.</span></span> <span data-ttu-id="de650-204">Du kan övervaka programuppgraderingen i Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="de650-204">You can monitor the application upgrade in Service Fabric Explorer.</span></span>

## <a name="migrating-old-service-fabric-java-applications-to-be-used-with-maven"></a><span data-ttu-id="de650-205">Migrera gamla Service Fabric Java-program som ska användas med Maven</span><span class="sxs-lookup"><span data-stu-id="de650-205">Migrating old Service Fabric Java applications to be used with Maven</span></span>
<span data-ttu-id="de650-206">Vi har nyligen flyttat Service Fabric Java-bibliotek från Service Fabric Java-SDK till Maven-centrallager.</span><span class="sxs-lookup"><span data-stu-id="de650-206">We have recently moved Service Fabric Java libraries from Service Fabric Java SDK to Maven repository.</span></span> <span data-ttu-id="de650-207">De nya program som du genererar med hjälp av Eclipse genererar senast uppdaterade projekt (som fungerar med Maven), men du kan uppdatera dina befintliga Service Fabric Java tillståndslösa eller aktörsprogram, som tidigare använde Service Fabric Java SDK, för att använda Service Fabric Java-beroenden från Maven.</span><span class="sxs-lookup"><span data-stu-id="de650-207">While the new applications you generate using Eclipse, will generate latest updated projects (which will be able to work with Maven), you can update your existing Service Fabric stateless or actor Java applications, which were using the Service Fabric Java SDK earlier, to use the Service Fabric Java dependencies from Maven.</span></span> <span data-ttu-id="de650-208">Följ anvisningarna [här](service-fabric-migrate-old-javaapp-to-use-maven.md) för att se till att det äldre programmet fungerar med Maven.</span><span class="sxs-lookup"><span data-stu-id="de650-208">Please follow the steps mentioned [here](service-fabric-migrate-old-javaapp-to-use-maven.md) to ensure your older application works with Maven.</span></span>

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
