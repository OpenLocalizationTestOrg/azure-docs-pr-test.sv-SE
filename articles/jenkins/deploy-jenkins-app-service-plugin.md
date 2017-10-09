---
title: aaaDeploy tooAzure App Service med Jenkins Plugin | Microsoft Docs
description: "Lär dig hur toouse Azure App Service Jenkins plugin-programmet toodeploy en Java web app tooAzure i Jenkins"
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 7/24/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 080be7277555ce7d688dccdf38eef309e7a7b194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-app-service-with-jenkins-plugin"></a><span data-ttu-id="b427d-103">Distribuera tooAzure Apptjänst med Jenkins plugin-program</span><span class="sxs-lookup"><span data-stu-id="b427d-103">Deploy tooAzure App Service with Jenkins plugin</span></span> 
<span data-ttu-id="b427d-104">toodeploy tooAzure en Java web app som du kan använda Azure CLI i [Jenkins Pipeline](/azure/jenkins/execute-cli-jenkins-pipeline) eller så kan du använda hello [plugin-program för Azure App Service Jenkins](https://plugins.jenkins.io/azure-app-service).</span><span class="sxs-lookup"><span data-stu-id="b427d-104">toodeploy a Java web app tooAzure, you can use Azure CLI in [Jenkins Pipeline](/azure/jenkins/execute-cli-jenkins-pipeline) or you can make use of hello [Azure App Service Jenkins plugin](https://plugins.jenkins.io/azure-app-service).</span></span> <span data-ttu-id="b427d-105">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="b427d-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b427d-106">Konfigurera Jenkins toodeploy tooAzure Apptjänst via FTP</span><span class="sxs-lookup"><span data-stu-id="b427d-106">Configure Jenkins toodeploy tooAzure App Service through FTP</span></span> 
> * <span data-ttu-id="b427d-107">Konfigurera Jenkins toodeploy tooAzure Apptjänst på Linux via Docker</span><span class="sxs-lookup"><span data-stu-id="b427d-107">Configure Jenkins toodeploy tooAzure App Service on Linux through Docker</span></span> 

## <a name="create-and-configure-jenkins-instance"></a><span data-ttu-id="b427d-108">Skapa och konfigurera Jenkins instans</span><span class="sxs-lookup"><span data-stu-id="b427d-108">Create and Configure Jenkins instance</span></span>
<span data-ttu-id="b427d-109">Om du inte redan har en Jenkins master börja med hello [Lösningsmall](install-jenkins-solution-template.md), bland annat JDK8 och hello följande obligatoriska plugin-program:</span><span class="sxs-lookup"><span data-stu-id="b427d-109">If you do not already have a Jenkins master, start with hello [Solution Template](install-jenkins-solution-template.md), which includes JDK8 and hello following required plugins:</span></span>

* <span data-ttu-id="b427d-110">[Jenkins Git-plugin för klienten](https://plugins.jenkins.io/git-client) v.2.4.6</span><span class="sxs-lookup"><span data-stu-id="b427d-110">[Jenkins Git client plugin](https://plugins.jenkins.io/git-client) v.2.4.6</span></span> 
* <span data-ttu-id="b427d-111">[Plugin-programmet för docker Commons](https://plugins.jenkins.io/docker-commons) v.1.4.0</span><span class="sxs-lookup"><span data-stu-id="b427d-111">[Docker Commons Plugin](https://plugins.jenkins.io/docker-commons) v.1.4.0</span></span>
* <span data-ttu-id="b427d-112">[Autentiseringsuppgifter för Azure](https://plugins.jenkins.io/azure-credentials) v.1.2</span><span class="sxs-lookup"><span data-stu-id="b427d-112">[Azure Credentials](https://plugins.jenkins.io/azure-credentials) v.1.2</span></span>
* <span data-ttu-id="b427d-113">[Azure Apptjänst](https://plugins.jenkins.io/azure-app-server) v.0.1</span><span class="sxs-lookup"><span data-stu-id="b427d-113">[Azure App Service](https://plugins.jenkins.io/azure-app-server) v.0.1</span></span>

<span data-ttu-id="b427d-114">Du kan använda hello Apptjänst plugin-programmet toodeploy Web App på alla språk (till exempel C#, PHP, Java och node.js etc.) som stöds av Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b427d-114">You can use hello App Service plugin toodeploy Web App in all languages (for instance, C#, PHP, Java, and node.js etc.) supported by Azure App Service.</span></span> <span data-ttu-id="b427d-115">I den här kursen använder hello för Java exempelapp [enkel Java-Webbapp för Azure](https://github.com/azure-devops/javawebappsample).</span><span class="sxs-lookup"><span data-stu-id="b427d-115">In this tutorial, we are using hello sample Java app, [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample).</span></span> <span data-ttu-id="b427d-116">toofork hello lagringsplatsen tooyour äger GitHub-konto, klickar du på hello **Återställningsförgreningar** knapp i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="b427d-116">toofork hello repo tooyour own GitHub account, click hello **Fork** button in hello top right-hand corner.</span></span>  

<span data-ttu-id="b427d-117">JDK Java och Maven krävs för att skapa hello Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="b427d-117">Java JDK and Maven are required for building hello Java project.</span></span> <span data-ttu-id="b427d-118">Se till att installera hello komponenter i hello Jenkins master eller hello VM-agenten om du använder en löpande integreringen.</span><span class="sxs-lookup"><span data-stu-id="b427d-118">Make sure you install hello components in hello Jenkins master or hello VM agent if you use one for continuous integration.</span></span> 

<span data-ttu-id="b427d-119">tooinstall, logga in toohello Jenkins-instans med SSH och kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="b427d-119">tooinstall, log in toohello Jenkins instance using SSH and run hello following commands:</span></span>

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

<span data-ttu-id="b427d-120">För att distribuera tooApp-tjänsten på Linux, behöver du också tooinstall Docker på hello Jenkins master eller hello VM-agenten används för version.</span><span class="sxs-lookup"><span data-stu-id="b427d-120">For deploying tooApp Service on Linux, you also need tooinstall Docker on hello Jenkins master or hello VM agent used for build.</span></span> <span data-ttu-id="b427d-121">Se toothis artikel tooinstall Docker: https://docs.docker.com/engine/installation/linux/ubuntu/.</span><span class="sxs-lookup"><span data-stu-id="b427d-121">Refer toothis article tooinstall Docker: https://docs.docker.com/engine/installation/linux/ubuntu/.</span></span>

## <a name="add-azure-service-principal-toojenkins-credential"></a><span data-ttu-id="b427d-122">Lägg till Azure-tjänstens huvudnamn tooJenkins autentiseringsuppgift</span><span class="sxs-lookup"><span data-stu-id="b427d-122">Add Azure service principal tooJenkins credential</span></span>

<span data-ttu-id="b427d-123">En Azure-tjänstens huvudnamn är nödvändiga toodeploy tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b427d-123">An Azure service principal is needed toodeploy tooAzure.</span></span> 

<ol>
<li><span data-ttu-id="b427d-124">Använd [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) eller [Azure-portalen](/azure/azure-resource-manager/resource-group-create-service-principal-portal) toocreate en Azure-tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="b427d-124">Use [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) or [Azure portal](/azure/azure-resource-manager/resource-group-create-service-principal-portal) toocreate an Azure service principal</span></span></li>
<li><span data-ttu-id="b427d-125">Inom hello Jenkins instrumentpanelen, klickar du på **autentiseringsuppgifter -> System ->**.</span><span class="sxs-lookup"><span data-stu-id="b427d-125">Within hello Jenkins dashboard, click **Credentials -> System ->**.</span></span> <span data-ttu-id="b427d-126">Klicka på **globala credentials(unrestricted)**.</span><span class="sxs-lookup"><span data-stu-id="b427d-126">Click **Global credentials(unrestricted)**.</span></span></li>
<li><span data-ttu-id="b427d-127">Klicka på **lägga till autentiseringsuppgifter** tooadd ett huvudnamn för tjänsten Microsoft Azure för genom att fylla i hello prenumerations-ID, klient-ID, Klienthemligheten och OAuth 2.0-Token för slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="b427d-127">Click **Add Credentials** tooadd a Microsoft Azure service principal by filling out hello Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span> <span data-ttu-id="b427d-128">Ange ett ID **mySp**, för användning i efterföljande steg.</span><span class="sxs-lookup"><span data-stu-id="b427d-128">Provide an ID, **mySp**, for use in subsequent steps.</span></span></li>
</ol>

## <a name="azure-app-service-plugin"></a><span data-ttu-id="b427d-129">Azure Apptjänst-plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="b427d-129">Azure App Service plugin</span></span>

<span data-ttu-id="b427d-130">Azure Apptjänst-plugin-programmet v1.0 stöder kontinuerlig distribution tooAzure webbprogram via:</span><span class="sxs-lookup"><span data-stu-id="b427d-130">Azure App Service plugin v1.0 supports continuous deployment tooAzure Web App through:</span></span>

* <span data-ttu-id="b427d-131">Git och FTP</span><span class="sxs-lookup"><span data-stu-id="b427d-131">Git and FTP</span></span>
* <span data-ttu-id="b427d-132">Docker för webbprogrammet på Linux</span><span class="sxs-lookup"><span data-stu-id="b427d-132">Docker for Web App on Linux</span></span>

## <a name="configure-jenkins-toodeploy-web-app-through-ftp-using-hello-jenkins-dashboard"></a><span data-ttu-id="b427d-133">Konfigurera Jenkins toodeploy webbprogram via FTP-hjälp hello Jenkins instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="b427d-133">Configure Jenkins toodeploy Web App through FTP using hello Jenkins dashboard</span></span>

<span data-ttu-id="b427d-134">toodeploy ditt projekt tooAzure Web App kan du överföra build-artefakter (till exempel .war fil i Java) med hjälp av Git eller FTP.</span><span class="sxs-lookup"><span data-stu-id="b427d-134">toodeploy your project tooAzure Web App, you can upload your build artifacts (for example, .war file in Java) using Git or FTP.</span></span>

<span data-ttu-id="b427d-135">Innan du konfigurerar hello jobb i Jenkins behöver du en Azure App Service-plan och en Webbapp för körs hello Java-app.</span><span class="sxs-lookup"><span data-stu-id="b427d-135">Before setting up hello job in Jenkins, you need an Azure App Service plan and a Web App for running hello Java app.</span></span>


1. <span data-ttu-id="b427d-136">Skapa en Azure App Service-plan med hello **lediga** prisnivån med hello [az programtjänstplan skapa](/cli/azure/appservice/plan#create) CLI-kommando.</span><span class="sxs-lookup"><span data-stu-id="b427d-136">Create an Azure App Service plan with hello **FREE** pricing tier using hello  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="b427d-137">Hej programtjänstplan definierar hello fysiska resurser som används toohost dina appar.</span><span class="sxs-lookup"><span data-stu-id="b427d-137">hello appservice plan defines hello physical resources used toohost your apps.</span></span> <span data-ttu-id="b427d-138">Alla program som tilldelats tooan programtjänstplan dela dessa resurser, så att du toosave kostnad när värd för flera appar.</span><span class="sxs-lookup"><span data-stu-id="b427d-138">All applications assigned tooan appservice plan share these resources, allowing you toosave cost when hosting multiple apps.</span></span>
2. <span data-ttu-id="b427d-139">Skapa en webbapp.</span><span class="sxs-lookup"><span data-stu-id="b427d-139">Create a Web App.</span></span> <span data-ttu-id="b427d-140">Du kan antingen använda hello [Azure-portalen](/azure/app-service-web/web-sites-configure) eller Använd hello följande Az CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="b427d-140">You can either use hello [Azure portal](/azure/app-service-web/web-sites-configure) or use hello following Az CLI command:</span></span>
```azurecli-interactive 
az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
```

3. <span data-ttu-id="b427d-141">Kontrollera att du ställer in hello Java runtime konfiguration som din app behöver.</span><span class="sxs-lookup"><span data-stu-id="b427d-141">Make sure you set up hello Java runtime configuration that your app needs.</span></span> <span data-ttu-id="b427d-142">hello följande Azure CLI kommando konfigurerar hello web app toorun på en senaste Java 8 JDK och [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="b427d-142">hello following Azure CLI command configures hello web app toorun on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>
```azurecli-interactive
az webapp config set \
--name <myAppName> \
--resource-group <myResourceGroup> \
--java-version 1.8 \
--java-container Tomcat \
--java-container-version 8.0
```

### <a name="set-up-hello-jenkins-job"></a><span data-ttu-id="b427d-143">Ange hello Jenkins jobb</span><span class="sxs-lookup"><span data-stu-id="b427d-143">Set up hello Jenkins job</span></span>


1. <span data-ttu-id="b427d-144">Skapa en ny **freestyle** projekt i Jenkins instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="b427d-144">Create a new **freestyle** project in Jenkins Dashboard</span></span>
2. <span data-ttu-id="b427d-145">Konfigurera **källa kod Management** toouse din lokala förgrening av [enkel Java-Webbapp för Azure](https://github.com/azure-devops/javawebappsample) genom att tillhandahålla hello **databasen URL**.</span><span class="sxs-lookup"><span data-stu-id="b427d-145">Configure **Source Code Management** toouse your local fork of [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) by providing hello **Repository URL**.</span></span> <span data-ttu-id="b427d-146">Till exempel: http://github.com/&lt;yourID > / javawebappsample.</span><span class="sxs-lookup"><span data-stu-id="b427d-146">For example: http://github.com/&lt;yourID>/javawebappsample.</span></span>
3. <span data-ttu-id="b427d-147">Lägga till ett Build steg toobuild hello-projekt med Maven.</span><span class="sxs-lookup"><span data-stu-id="b427d-147">Add a Build step toobuild hello project using Maven.</span></span> <span data-ttu-id="b427d-148">Göra det genom att lägga till en **köra shell**.</span><span class="sxs-lookup"><span data-stu-id="b427d-148">Do so by adding an **Execute shell**.</span></span> <span data-ttu-id="b427d-149">I det här exemplet behöver vi en ytterligare steg toorename hello *.war fil i målet mappen tooROOT.war.</span><span class="sxs-lookup"><span data-stu-id="b427d-149">For this example, we need an additional step toorename hello *.war file in target folder tooROOT.war.</span></span>   
```bash
mvn clean package
mv target/*.war target/ROOT.war
```

4. <span data-ttu-id="b427d-150">Lägg till en efter build-åtgärd genom att välja **publicera ett Azure Web App**.</span><span class="sxs-lookup"><span data-stu-id="b427d-150">Add a post-build action by selecting **Publish an Azure Web App**.</span></span>
5. <span data-ttu-id="b427d-151">Ange ”mySp”, hello Azure tjänstens huvudnamn lagras i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="b427d-151">Supply, "mySp", hello Azure service principal stored in previous step.</span></span>
6. <span data-ttu-id="b427d-152">I **Appkonfiguration** väljer hello resurs grupp och webb-app i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b427d-152">In **App Configuration** section, choose hello resource group and web app in your subscription.</span></span> <span data-ttu-id="b427d-153">hello-plugin-programmet identifierar automatiskt om hello Web App är Windows eller Linux-baserade.</span><span class="sxs-lookup"><span data-stu-id="b427d-153">hello plugin automatically detects whether hello Web App is Windows or Linux-based.</span></span> <span data-ttu-id="b427d-154">För en Windows-baserade Webbapp, hello alternativet ”Publicera filer” visas.</span><span class="sxs-lookup"><span data-stu-id="b427d-154">For a Windows-based Web App, hello option "Publish Files" is presented.</span></span>
7. <span data-ttu-id="b427d-155">Fyll i hello filer du toodeploy (till exempel en war-paket om du använder Java.) Källa och målkatalog är valfria.</span><span class="sxs-lookup"><span data-stu-id="b427d-155">Fill in hello files you want toodeploy (for example, a war package if you're using Java.) Source Directory and Target Directory are optional.</span></span> <span data-ttu-id="b427d-156">hello-parametrar kan du toospecify käll- och mappar när överföringen av filer.</span><span class="sxs-lookup"><span data-stu-id="b427d-156">hello parameters allow you toospecify source and target folders when uploading files.</span></span> <span data-ttu-id="b427d-157">Java-webbapp på Azure körs i en Tomcat-server.</span><span class="sxs-lookup"><span data-stu-id="b427d-157">Java web app on Azure is run in a Tomcat server.</span></span> <span data-ttu-id="b427d-158">Så att du överför krig paketet till webbappens mapp.</span><span class="sxs-lookup"><span data-stu-id="b427d-158">So you upload you war package to webapps folder.</span></span> <span data-ttu-id="b427d-159">Det här exemplet anger **källkatalog** för ”mål” och ange ”webbappar” för **målkatalogen**.</span><span class="sxs-lookup"><span data-stu-id="b427d-159">For this example, set **Source Directory** too"target" and supply "webapps" for **Target Directory**.</span></span>
8. <span data-ttu-id="b427d-160">Om du vill toodeploy tooa fack än produktion, kan du också ange **fack** namn.</span><span class="sxs-lookup"><span data-stu-id="b427d-160">If you want toodeploy tooa slot other than production, you can also set **Slot** Name.</span></span>
9. <span data-ttu-id="b427d-161">Spara hello projekt och skapa den.</span><span class="sxs-lookup"><span data-stu-id="b427d-161">Save hello project and build it.</span></span> <span data-ttu-id="b427d-162">Webbappen är distribuerade tooAzure när version har slutförts.</span><span class="sxs-lookup"><span data-stu-id="b427d-162">Your web app is deployed tooAzure when build is complete.</span></span>

### <a name="deploy-web-app-through-ftp-using-jenkins-pipeline"></a><span data-ttu-id="b427d-163">Distribuera webbprogram via FTP-använda Jenkins pipeline</span><span class="sxs-lookup"><span data-stu-id="b427d-163">Deploy Web App through FTP using Jenkins pipeline</span></span>

<span data-ttu-id="b427d-164">hello-plugin-programmet är redo pipeline.</span><span class="sxs-lookup"><span data-stu-id="b427d-164">hello plugin is pipeline-ready.</span></span> <span data-ttu-id="b427d-165">Du kan se tooa provet i GitHub-repo-hello.</span><span class="sxs-lookup"><span data-stu-id="b427d-165">You can refer tooa sample in hello GitHub repo.</span></span>

1. <span data-ttu-id="b427d-166">Öppna i GitHub-webbgränssnittet, **Jenkinsfile_ftp_plugin** fil.</span><span class="sxs-lookup"><span data-stu-id="b427d-166">In GitHub web UI, open **Jenkinsfile_ftp_plugin** file.</span></span> <span data-ttu-id="b427d-167">Klicka på hello penna ikonen tooedit den här filen tooupdate hello resursgruppens namn och namnet på ditt webbprogram på rad 11 och 12 respektive.</span><span class="sxs-lookup"><span data-stu-id="b427d-167">Click hello pencil icon tooedit this file tooupdate hello resource group and name of your web app on line 11 and 12 respectively.</span></span>    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. <span data-ttu-id="b427d-168">Ändra raden 14 tooupdate uppgifts-ID i din instans av Jenkins.</span><span class="sxs-lookup"><span data-stu-id="b427d-168">Change line 14 tooupdate credential ID in your Jenkins instance.</span></span>    
```java
withCredentials([azureServicePrincipal('<mySp>')]) {
```

### <a name="create-a-jenkins-pipeline"></a><span data-ttu-id="b427d-169">Skapa en Jenkins pipeline</span><span class="sxs-lookup"><span data-stu-id="b427d-169">Create a Jenkins pipeline</span></span>

1. <span data-ttu-id="b427d-170">Öppna Jenkins i en webbläsare, klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="b427d-170">Open Jenkins in a web browser, click **New Item**.</span></span>
2. <span data-ttu-id="b427d-171">Ange ett namn för hello jobbet och välja **Pipeline**.</span><span class="sxs-lookup"><span data-stu-id="b427d-171">Provide a name for hello job and select **Pipeline**.</span></span> <span data-ttu-id="b427d-172">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b427d-172">Click **OK**.</span></span>
3. <span data-ttu-id="b427d-173">Klicka på hello **Pipeline** fliken nästa.</span><span class="sxs-lookup"><span data-stu-id="b427d-173">Click hello **Pipeline** tab next.</span></span>
4. <span data-ttu-id="b427d-174">För **Definition**väljer **Pipeline-skriptet från SCM**.</span><span class="sxs-lookup"><span data-stu-id="b427d-174">For **Definition**, select **Pipeline script from SCM**.</span></span>
5. <span data-ttu-id="b427d-175">För **SCM**väljer **Git**.</span><span class="sxs-lookup"><span data-stu-id="b427d-175">For **SCM**, select **Git**.</span></span> <span data-ttu-id="b427d-176">Ange hello GitHub-URL för din andelen vridvuxna lagringsplatsen: https:&lt;din andelen vridvuxna lagringsplatsen > .git</span><span class="sxs-lookup"><span data-stu-id="b427d-176">Enter hello GitHub URL for your forked repo: https:&lt;your forked repo>.git</span></span>
6. <span data-ttu-id="b427d-177">Uppdatera **skriptsökvägen** för ”Jenkinsfile_ftp_plugin”</span><span class="sxs-lookup"><span data-stu-id="b427d-177">Update **Script Path** too"Jenkinsfile_ftp_plugin"</span></span>
7. <span data-ttu-id="b427d-178">Klicka på **spara** och kör hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="b427d-178">Click **Save** and run hello job.</span></span>

## <a name="configure-jenkins-toodeploy-web-app-on-linux-through-docker"></a><span data-ttu-id="b427d-179">Konfigurera Jenkins toodeploy webbprogrammet på Linux via Docker</span><span class="sxs-lookup"><span data-stu-id="b427d-179">Configure Jenkins toodeploy Web App on Linux through Docker</span></span>

<span data-ttu-id="b427d-180">Förutom Git/FTP stöder webbprogram på Linux-distributionen med Docker.</span><span class="sxs-lookup"><span data-stu-id="b427d-180">Apart from Git/FTP, Web App on Linux supports deployment using Docker.</span></span> <span data-ttu-id="b427d-181">toodeploy med Docker, behöver du tooprovide en Dockerfile som ditt webbprogram med tjänsten runtime-paket till en docker-bild.</span><span class="sxs-lookup"><span data-stu-id="b427d-181">toodeploy using Docker, you need tooprovide a Dockerfile that packages your web app with service runtime into a docker image.</span></span> <span data-ttu-id="b427d-182">Sedan hello plugin bygger hello bild, skickar den tooa docker registret och distribuerar hello avbildningen tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b427d-182">Then hello plugin builds hello image, pushes it tooa docker registry and deploys hello image tooyour web app.</span></span>

<span data-ttu-id="b427d-183">Web App på Linux stöder också traditionella sätt som Git och FTP, men endast för inbyggda språk (.NET Core, Node.js, PHP och Ruby).</span><span class="sxs-lookup"><span data-stu-id="b427d-183">Web App on Linux also supports traditional ways like Git and FTP, but only for built-in languages (.NET Core, Node.js, PHP, and Ruby).</span></span> <span data-ttu-id="b427d-184">För andra språk du behöver toopackage programmets kod och tjänsten körning tillsammans i en docker-avbildningen och använda docker toodeploy.</span><span class="sxs-lookup"><span data-stu-id="b427d-184">For other languages, you need toopackage your application code and service runtime together into a docker image and use docker toodeploy.</span></span>

<span data-ttu-id="b427d-185">Innan du konfigurerar hello jobb i Jenkins måste en Azure apptjänst i Linux.</span><span class="sxs-lookup"><span data-stu-id="b427d-185">Before setting up hello job in Jenkins, you need an Azure app service on Linux.</span></span> <span data-ttu-id="b427d-186">En behållare registret är också nödvändig toostore och hantera dina privata Docker behållare avbildningar.</span><span class="sxs-lookup"><span data-stu-id="b427d-186">A container registry is also needed toostore and manage your private Docker container images.</span></span> <span data-ttu-id="b427d-187">Du kan använda DockerHub; Vi använder Azure Container registret för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="b427d-187">You can use DockerHub; we are using Azure Container Registry for this example.</span></span>

* <span data-ttu-id="b427d-188">Du kan följa stegen hello [här](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate ett webbprogram på Linux</span><span class="sxs-lookup"><span data-stu-id="b427d-188">You can follow hello steps [here](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate a Web App on Linux</span></span> 
* <span data-ttu-id="b427d-189">Azure-behållare är en hanterad [Docker registret] (https://docs.docker.com/registry/) tjänsten baserat på hello öppen källkod Docker registret 2.0.</span><span class="sxs-lookup"><span data-stu-id="b427d-189">Azure Container Registry is a managed [Docker registry] (https://docs.docker.com/registry/) service based on hello open-source Docker Registry 2.0.</span></span> <span data-ttu-id="b427d-190">Gör hello [här] (/ azure/container-registry/container-registry-get-started-azure-cli) för mer information om hur toodo så.</span><span class="sxs-lookup"><span data-stu-id="b427d-190">Follow hello steps [here] (/azure/container-registry/container-registry-get-started-azure-cli) for more guidance on how toodo so.</span></span> <span data-ttu-id="b427d-191">Du kan också använda DockerHub.</span><span class="sxs-lookup"><span data-stu-id="b427d-191">You can also use DockerHub.</span></span>

### <a name="toodeploy-using-docker"></a><span data-ttu-id="b427d-192">toodeploy med docker:</span><span class="sxs-lookup"><span data-stu-id="b427d-192">toodeploy using docker:</span></span>

1. <span data-ttu-id="b427d-193">Skapa ett nytt freestyle projekt i Jenkins instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b427d-193">Create a new freestyle project in Jenkins Dashboard.</span></span>
2. <span data-ttu-id="b427d-194">Konfigurera **källa kod Management** toouse din lokala förgrening av [enkel Java-Webbapp för Azure](https://github.com/azure-devops/javawebappsample) genom att tillhandahålla hello **databasen URL**.</span><span class="sxs-lookup"><span data-stu-id="b427d-194">Configure **Source Code Management** toouse your local fork of [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) by providing hello **Repository URL**.</span></span> <span data-ttu-id="b427d-195">Till exempel: http://github.com/&lt;yourid > / javawebappsample.</span><span class="sxs-lookup"><span data-stu-id="b427d-195">For example: http://github.com/&lt;yourid>/javawebappsample.</span></span>
<span data-ttu-id="b427d-196">Lägga till ett Build steg toobuild hello-projekt med Maven.</span><span class="sxs-lookup"><span data-stu-id="b427d-196">Add a Build step toobuild hello project using Maven.</span></span> <span data-ttu-id="b427d-197">Göra det genom att lägga till en **köra shell** och Lägg till följande rad i hello **kommandot**:</span><span class="sxs-lookup"><span data-stu-id="b427d-197">Do so by adding an **Execute shell** and add hello following line in **Command**:</span></span>    
```bash
mvn clean package
```

3. <span data-ttu-id="b427d-198">Lägg till en efter build-åtgärd genom att välja **publicera ett Azure Web App**.</span><span class="sxs-lookup"><span data-stu-id="b427d-198">Add a post-build action by selecting **Publish an Azure Web App**.</span></span>
4. <span data-ttu-id="b427d-199">Ange, **mySp**, hello Azure tjänstens huvudnamn lagras i föregående steg som Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b427d-199">Supply, **mySp**, hello Azure service principal stored in previous step as Azure Credentials.</span></span>
5. <span data-ttu-id="b427d-200">I **Appkonfiguration** väljer hello resursgrupp och en Linux-webbapp i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b427d-200">In **App Configuration** section, choose hello resource group and a Linux web app in your subscription.</span></span>
6. <span data-ttu-id="b427d-201">Välj publicera via Docker.</span><span class="sxs-lookup"><span data-stu-id="b427d-201">Choose Publish via Docker.</span></span>
7. <span data-ttu-id="b427d-202">Fyll i **Dockerfile** sökväg.</span><span class="sxs-lookup"><span data-stu-id="b427d-202">Fill in **Dockerfile** path.</span></span> <span data-ttu-id="b427d-203">Du kan behålla standard hello ”/ Dockerfile” för **Docker registret URL**, varor i hello-format för https://&lt;myRegistry >. azurecr.io om du använder Azure-behållare registret.</span><span class="sxs-lookup"><span data-stu-id="b427d-203">You can keep hello default "/Dockerfile" For **Docker registry URL**, supply in hello format of https://&lt;myRegistry>.azurecr.io if you use Azure Container Registry.</span></span> <span data-ttu-id="b427d-204">Lämna det tomt om du använder DockerHub.</span><span class="sxs-lookup"><span data-stu-id="b427d-204">Leave it blank if you use DockerHub.</span></span>
8. <span data-ttu-id="b427d-205">För **registret autentiseringsuppgifter**, Lägg till hello autentiseringsuppgift för hello Azure Container registret.</span><span class="sxs-lookup"><span data-stu-id="b427d-205">For **Registry credentials**, add hello credential for hello Azure Container Registry.</span></span> <span data-ttu-id="b427d-206">Du kan hämta hello användar-ID och lösenord genom att köra följande kommandon i Azure CLI hello.</span><span class="sxs-lookup"><span data-stu-id="b427d-206">You can get hello userid and password by running hello following commands in Azure CLI.</span></span> <span data-ttu-id="b427d-207">hello första kommando aktiverar hello-administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="b427d-207">hello first command enables hello administrator account.</span></span>    
```azurecli-interactive
az acr update -n <yourRegistry> --admin-enabled true
az acr credential show -n <yourRegistry>
```

9. <span data-ttu-id="b427d-208">Hej docker namn och en tagg i **Avancerat** fliken är valfria.</span><span class="sxs-lookup"><span data-stu-id="b427d-208">hello docker image name and tag in **Advanced** tab are optional.</span></span> <span data-ttu-id="b427d-209">Som standard avbildningsnamn erhålls från hello avbildning namn som du konfigurerade i Azure portal (i Dockerbehållare inställningen.) hello taggen genereras från $BUILD_NUMBER.</span><span class="sxs-lookup"><span data-stu-id="b427d-209">By default, image name is obtained from hello image name you configured in Azure portal (in Docker Container setting.) hello tag is generated from $BUILD_NUMBER.</span></span> <span data-ttu-id="b427d-210">Kontrollera att du anger hello avbildningsnamn i antingen Azure-portalen eller ange ett värde för **Docker bild** i **Avancerat** fliken. Det här exemplet anger ”&lt;yourRegistry >.azurecr.io/calculator” för **Docker bild** och lämna **Docker bildtagg** tomt.</span><span class="sxs-lookup"><span data-stu-id="b427d-210">Make sure you specify hello image name in either Azure portal or supply a value for **Docker Image** in **Advanced** tab. For this example, supply "&lt;yourRegistry>.azurecr.io/calculator" for **Docker image** and leave **Docker Image Tag** blank.</span></span>
10. <span data-ttu-id="b427d-211">Obs distributionen misslyckas om du använder inställningen av inbyggda Docker-bild.</span><span class="sxs-lookup"><span data-stu-id="b427d-211">Note deployment fails if you use built-in Docker image setting.</span></span> <span data-ttu-id="b427d-212">Kontrollera att du ändrar docker config toouse anpassad avbildning i Dockerbehållare inställningen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b427d-212">Make sure you change docker config toouse custom image in Docker Container setting in Azure portal.</span></span> <span data-ttu-id="b427d-213">Använd filen överför metoden toodeploy för inbyggda avbildningen.</span><span class="sxs-lookup"><span data-stu-id="b427d-213">For built-in image, use file upload approach toodeploy.</span></span>
11. <span data-ttu-id="b427d-214">Du kan välja en annan plats än produktion liknande toofile överför sätt.</span><span class="sxs-lookup"><span data-stu-id="b427d-214">Similar toofile upload approach, you can choose a different slot other than production.</span></span>
12. <span data-ttu-id="b427d-215">Spara och skapa hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="b427d-215">Save and build hello project.</span></span> <span data-ttu-id="b427d-216">Du ser behållaren avbildningen skickas tooyour registret och webbapp distribueras.</span><span class="sxs-lookup"><span data-stu-id="b427d-216">You see your container image is pushed tooyour registry and web app is deployed.</span></span>

### <a name="deploy-tooweb-app-on-linux-through-docker-using-jenkins-pipeline"></a><span data-ttu-id="b427d-217">Distribuera tooWeb App på Linux via Docker använda Jenkins pipeline</span><span class="sxs-lookup"><span data-stu-id="b427d-217">Deploy tooWeb App on Linux through Docker using Jenkins pipeline</span></span>

1. <span data-ttu-id="b427d-218">Öppna i GitHub-webbgränssnittet, **Jenkinsfile_container_plugin** fil.</span><span class="sxs-lookup"><span data-stu-id="b427d-218">In GitHub web UI, open **Jenkinsfile_container_plugin** file.</span></span> <span data-ttu-id="b427d-219">Klicka på hello penna ikonen tooedit den här filen tooupdate hello resursgruppens namn och namnet på ditt webbprogram på rad 11 och 12 respektive.</span><span class="sxs-lookup"><span data-stu-id="b427d-219">Click hello pencil icon tooedit this file tooupdate hello resource group and name of your web app on line 11 and 12 respectively.</span></span>    
```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<myAppName>'
```

2. <span data-ttu-id="b427d-220">Ändra raden 13 tooyour behållare registret servern</span><span class="sxs-lookup"><span data-stu-id="b427d-220">Change line 13 tooyour container registry server</span></span>    
```java
def registryServer = '<registryURL>'
```    

3. <span data-ttu-id="b427d-221">Ändra raden 16 tooupdate uppgifts-ID i Jenkins-instans</span><span class="sxs-lookup"><span data-stu-id="b427d-221">Change line 16 tooupdate credential ID in your Jenkins instance</span></span>    
```java
azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
```    
### <a name="create-jenkins-pipeline"></a><span data-ttu-id="b427d-222">Skapa Jenkins pipeline</span><span class="sxs-lookup"><span data-stu-id="b427d-222">Create Jenkins pipeline</span></span>    

1. <span data-ttu-id="b427d-223">Öppna Jenkins i en webbläsare, klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="b427d-223">Open Jenkins in a web browser, click **New Item**.</span></span>
2. <span data-ttu-id="b427d-224">Ange ett namn för hello jobbet och välja **Pipeline**.</span><span class="sxs-lookup"><span data-stu-id="b427d-224">Provide a name for hello job and select **Pipeline**.</span></span> <span data-ttu-id="b427d-225">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b427d-225">Click **OK**.</span></span>
3. <span data-ttu-id="b427d-226">Klicka på hello **Pipeline** fliken nästa.</span><span class="sxs-lookup"><span data-stu-id="b427d-226">Click hello **Pipeline** tab next.</span></span>
4. <span data-ttu-id="b427d-227">För **Definition**väljer **Pipeline-skriptet från SCM**.</span><span class="sxs-lookup"><span data-stu-id="b427d-227">For **Definition**, select **Pipeline script from SCM**.</span></span>
5. <span data-ttu-id="b427d-228">För **SCM**väljer **Git**.</span><span class="sxs-lookup"><span data-stu-id="b427d-228">For **SCM**, select **Git**.</span></span>
6. <span data-ttu-id="b427d-229">Ange hello GitHub-URL för din andelen vridvuxna lagringsplatsen: https:&lt;din andelen vridvuxna lagringsplatsen > .git</span><span class="sxs-lookup"><span data-stu-id="b427d-229">Enter hello GitHub URL for your forked repo: https:&lt;your forked repo>.git</span></span></li>
<span data-ttu-id="b427d-230">7, uppdatera **skriptsökvägen** för ”Jenkinsfile_container_plugin”</span><span class="sxs-lookup"><span data-stu-id="b427d-230">7, Update **Script Path** too"Jenkinsfile_container_plugin"</span></span>
8. <span data-ttu-id="b427d-231">Klicka på **spara** och kör hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="b427d-231">Click **Save** and run hello job.</span></span>

## <a name="verify-your-web-app"></a><span data-ttu-id="b427d-232">Kontrollera ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="b427d-232">Verify your web app</span></span>

1. <span data-ttu-id="b427d-233">tooverify hello WAR-filen har distribueras tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b427d-233">tooverify hello WAR file is deployed successfully tooyour web app.</span></span> <span data-ttu-id="b427d-234">Öppna en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b427d-234">Open a web browser.</span></span>
2. <span data-ttu-id="b427d-235">Gå toohttp: / /&lt;programnamn >.azurewebsites.net/api/calculator/ping visas:</span><span class="sxs-lookup"><span data-stu-id="b427d-235">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/ping You see:</span></span>    
     <span data-ttu-id="b427d-236">Välkommen till tooJava webbprogrammet!</span><span class="sxs-lookup"><span data-stu-id="b427d-236">Welcome tooJava Web App!!!</span></span> <span data-ttu-id="b427d-237">Detta har uppdaterats!</span><span class="sxs-lookup"><span data-stu-id="b427d-237">This is updated!</span></span>
   <span data-ttu-id="b427d-238">Sun Jun 17 16:39:10 UTC 2017</span><span class="sxs-lookup"><span data-stu-id="b427d-238">Sun Jun 17 16:39:10 UTC 2017</span></span>
3. <span data-ttu-id="b427d-239">Gå toohttp: / /&lt;appnamn >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (ersätta &lt;x > och &lt;y > med några siffror) tooget hello summan av x och y</span><span class="sxs-lookup"><span data-stu-id="b427d-239">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) tooget hello sum of x and y</span></span>        
    ![Kalkylatorn: Lägg till](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-app-service-on-linux"></a><span data-ttu-id="b427d-241">För apptjänst på Linux</span><span class="sxs-lookup"><span data-stu-id="b427d-241">For App service on Linux</span></span>

* <span data-ttu-id="b427d-242">tooverify i Azure CLI kör:</span><span class="sxs-lookup"><span data-stu-id="b427d-242">tooverify, in Azure CLI, run:</span></span>

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    <span data-ttu-id="b427d-243">Du får hello följande resultat:</span><span class="sxs-lookup"><span data-stu-id="b427d-243">You get hello following result:</span></span>
    
    ```
    [
    "calculator"
    ]
    ```
    
    <span data-ttu-id="b427d-244">Gå toohttp: / /&lt;programnamn >.azurewebsites.net/api/calculator/ping.</span><span class="sxs-lookup"><span data-stu-id="b427d-244">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/ping.</span></span> <span data-ttu-id="b427d-245">Hello-meddelande visas:</span><span class="sxs-lookup"><span data-stu-id="b427d-245">You see hello message:</span></span> 
    
        Welcome tooJava Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    <span data-ttu-id="b427d-246">Gå toohttp: / /&lt;appnamn >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (ersätta &lt;x > och &lt;y > med några siffror) tooget hello summan av x och y</span><span class="sxs-lookup"><span data-stu-id="b427d-246">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) tooget hello sum of x and y</span></span>
    
## <a name="next-steps"></a><span data-ttu-id="b427d-247">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b427d-247">Next steps</span></span>

<span data-ttu-id="b427d-248">I den här kursen använder du hello Azure App Service-plugin-programmet toodeploy tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b427d-248">In this tutorial, you use hello Azure App Service plugin toodeploy tooAzure.</span></span>

<span data-ttu-id="b427d-249">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="b427d-249">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b427d-250">Konfigurera Jenkins toodeploy Azure App Service via FTP</span><span class="sxs-lookup"><span data-stu-id="b427d-250">Configure Jenkins toodeploy Azure App Service through FTP</span></span> 
> * <span data-ttu-id="b427d-251">Konfigurera Jenkins toodeploy tooAzure Apptjänst på Linux via Docker</span><span class="sxs-lookup"><span data-stu-id="b427d-251">Configure Jenkins toodeploy tooAzure App Service on Linux through Docker</span></span> 
