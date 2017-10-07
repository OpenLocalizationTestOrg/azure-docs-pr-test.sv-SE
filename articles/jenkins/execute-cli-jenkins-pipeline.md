---
title: aaaExecute hello Azure CLI med Jenkins | Microsoft Docs
description: "Lär dig hur toouse Azure CLI toodeploy en Java web app tooAzure i Jenkins Pipeline"
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: jenkins
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 4bd1e12e6de1f010453ff51c835f84e7361962f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-app-service-with-jenkins-and-hello-azure-cli"></a><span data-ttu-id="104e9-103">Distribuera tooAzure App Service med Jenkins och hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="104e9-103">Deploy tooAzure App Service with Jenkins and hello Azure CLI</span></span>
<span data-ttu-id="104e9-104">toodeploy tooAzure en Java web app som du kan använda Azure CLI i [Jenkins Pipeline](https://jenkins.io/doc/book/pipeline/).</span><span class="sxs-lookup"><span data-stu-id="104e9-104">toodeploy a Java web app tooAzure, you can use Azure CLI in [Jenkins Pipeline](https://jenkins.io/doc/book/pipeline/).</span></span> <span data-ttu-id="104e9-105">I den här självstudiekursen skapar du en CI/CD-pipeline på en Azure VM att:</span><span class="sxs-lookup"><span data-stu-id="104e9-105">In this tutorial, you create a CI/CD pipeline on an Azure VM including how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="104e9-106">Skapa en virtuell dator Jenkins</span><span class="sxs-lookup"><span data-stu-id="104e9-106">Create a Jenkins VM</span></span>
> * <span data-ttu-id="104e9-107">Konfigurera Jenkins</span><span class="sxs-lookup"><span data-stu-id="104e9-107">Configure Jenkins</span></span>
> * <span data-ttu-id="104e9-108">Skapa en webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="104e9-108">Create a web app in Azure</span></span>
> * <span data-ttu-id="104e9-109">Förbereda en GitHub-databas</span><span class="sxs-lookup"><span data-stu-id="104e9-109">Prepare a GitHub repository</span></span>
> * <span data-ttu-id="104e9-110">Skapa Jenkins pipeline</span><span class="sxs-lookup"><span data-stu-id="104e9-110">Create Jenkins pipeline</span></span>
> * <span data-ttu-id="104e9-111">Kör hello pipeline och verifiera hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="104e9-111">Run hello pipeline and verify hello web app</span></span>

<span data-ttu-id="104e9-112">Den här kursen kräver hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="104e9-112">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="104e9-113">toofind hello version, kör `az --version`.</span><span class="sxs-lookup"><span data-stu-id="104e9-113">toofind hello version, run `az --version`.</span></span> <span data-ttu-id="104e9-114">Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="104e9-114">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a><span data-ttu-id="104e9-115">Skapa och konfigurera Jenkins instans</span><span class="sxs-lookup"><span data-stu-id="104e9-115">Create and Configure Jenkins instance</span></span>
<span data-ttu-id="104e9-116">Om du inte redan har en Jenkins master börja med hello [Lösningsmall](install-jenkins-solution-template.md), som innehåller hello krävs [Azure-autentiseringsuppgifterna](https://plugins.jenkins.io/azure-credentials) plugin-programmet som standard.</span><span class="sxs-lookup"><span data-stu-id="104e9-116">If you do not already have a Jenkins master, start with hello [Solution Template](install-jenkins-solution-template.md), which includes hello required [Azure Credentials](https://plugins.jenkins.io/azure-credentials) plugin by default.</span></span> 

<span data-ttu-id="104e9-117">Autentiseringsuppgifter för Azure-plugin-programmet hello tillåter toostore Microsoft Azure service principal autentiseringsuppgifter i Jenkins.</span><span class="sxs-lookup"><span data-stu-id="104e9-117">hello Azure Credential plugin allows you toostore Microsoft Azure service principal credentials in Jenkins.</span></span> <span data-ttu-id="104e9-118">I version 1.2, vi har lagt till stöd för hello så att Jenkins-Pipeline kan få hello autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="104e9-118">In version 1.2, we added hello support so that Jenkins Pipeline can get hello Azure credentials.</span></span> 

<span data-ttu-id="104e9-119">Se till att du har version 1.2 eller senare:</span><span class="sxs-lookup"><span data-stu-id="104e9-119">Ensure you have version 1.2 or later:</span></span>
* <span data-ttu-id="104e9-120">Inom hello Jenkins instrumentpanelen, klickar du på **Plugin-hanteraren -> Hantera Jenkins ->** och Sök efter **Azure autentiseringsuppgifter**.</span><span class="sxs-lookup"><span data-stu-id="104e9-120">Within hello Jenkins dashboard, click **Manage Jenkins -> Plugin Manager ->** and search for **Azure Credential**.</span></span> 
* <span data-ttu-id="104e9-121">Uppdatera hello plugin om hello-versionen är tidigare än 1.2.</span><span class="sxs-lookup"><span data-stu-id="104e9-121">Update hello plugin if hello version is earlier than 1.2.</span></span>

<span data-ttu-id="104e9-122">JDK Java och Maven också krävs i hello Jenkins master.</span><span class="sxs-lookup"><span data-stu-id="104e9-122">Java JDK and Maven are also required in hello Jenkins master.</span></span> <span data-ttu-id="104e9-123">tooinstall, logga in tooJenkins master med SSH och kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="104e9-123">tooinstall, log in tooJenkins master using SSH and run hello following commands:</span></span>
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-toojenkins-credential"></a><span data-ttu-id="104e9-124">Lägg till Azure-tjänstens huvudnamn tooJenkins autentiseringsuppgift</span><span class="sxs-lookup"><span data-stu-id="104e9-124">Add Azure service principal tooJenkins credential</span></span>

<span data-ttu-id="104e9-125">En Azure-autentiseringsuppgift är nödvändiga tooexecute Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="104e9-125">An Azure credential is needed tooexecute Azure CLI.</span></span>

* <span data-ttu-id="104e9-126">Inom hello Jenkins instrumentpanelen, klickar du på **autentiseringsuppgifter -> System ->**.</span><span class="sxs-lookup"><span data-stu-id="104e9-126">Within hello Jenkins dashboard, click **Credentials -> System ->**.</span></span> <span data-ttu-id="104e9-127">Klicka på **globala credentials(unrestricted)**.</span><span class="sxs-lookup"><span data-stu-id="104e9-127">Click **Global credentials(unrestricted)**.</span></span>
* <span data-ttu-id="104e9-128">Klicka på **lägga till autentiseringsuppgifter** tooadd en [Microsoft Azure-tjänstens huvudnamn](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) genom att fylla i hello prenumerations-ID, klient-ID, Klienthemligheten och OAuth 2.0-Token för slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="104e9-128">Click **Add Credentials** tooadd a [Microsoft Azure service principal](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) by filling out hello Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span> <span data-ttu-id="104e9-129">Ange ett ID för användning i senare steg.</span><span class="sxs-lookup"><span data-stu-id="104e9-129">Provide an ID for use in subsequent step.</span></span>

![Lägga till autentiseringsuppgifter](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-hello-java-web-app"></a><span data-ttu-id="104e9-131">Skapa en Azure App Service för att distribuera hello Java-webbapp</span><span class="sxs-lookup"><span data-stu-id="104e9-131">Create an Azure App Service for deploying hello Java web app</span></span>

<span data-ttu-id="104e9-132">Skapa en Azure App Service-plan med hello **lediga** prisnivån med hello [az programtjänstplan skapa](/cli/azure/appservice/plan#create) CLI-kommando.</span><span class="sxs-lookup"><span data-stu-id="104e9-132">Create an Azure App Service plan with hello **FREE** pricing tier using hello  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="104e9-133">Hej programtjänstplan definierar hello fysiska resurser som används toohost dina appar.</span><span class="sxs-lookup"><span data-stu-id="104e9-133">hello appservice plan defines hello physical resources used toohost your apps.</span></span> <span data-ttu-id="104e9-134">Alla program som tilldelats tooan programtjänstplan dela dessa resurser, så att du toosave kostnad när värd för flera appar.</span><span class="sxs-lookup"><span data-stu-id="104e9-134">All applications assigned tooan appservice plan share these resources, allowing you toosave cost when hosting multiple apps.</span></span> 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

<span data-ttu-id="104e9-135">När hello plan är klar, utdata hello Azure CLI visar liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="104e9-135">When hello plan is ready, hello Azure CLI shows similar output toohello following example:</span></span>

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a><span data-ttu-id="104e9-136">Skapa en Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="104e9-136">Create an Azure Web app</span></span>

 <span data-ttu-id="104e9-137">Använd hello [az webapp skapa](/cli/azure/appservice/web#create) CLI kommandot toocreate en web app definition i hello `myAppServicePlan` App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="104e9-137">Use hello [az webapp create](/cli/azure/appservice/web#create) CLI command toocreate a web app definition in hello `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="104e9-138">hello web app definition innehåller en URL-tooaccess ditt program med och konfigurerar flera alternativ toodeploy tooAzure din kod.</span><span class="sxs-lookup"><span data-stu-id="104e9-138">hello web app definition provides a URL tooaccess your application with and configures several options toodeploy your code tooAzure.</span></span> 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

<span data-ttu-id="104e9-139">Ersätt hello `<app_name>` med dina egna unika namn.</span><span class="sxs-lookup"><span data-stu-id="104e9-139">Substitute hello `<app_name>` placeholder with your own unique app name.</span></span> <span data-ttu-id="104e9-140">Detta unika namn är en del av hello standarddomännamnet för hello webbprogrammet så hello namn måste toobe unikt över alla program i Azure.</span><span class="sxs-lookup"><span data-stu-id="104e9-140">This unique name is part of hello default domain name for hello web app, so hello name needs toobe unique across all apps in Azure.</span></span> <span data-ttu-id="104e9-141">Du kan mappa ett webbprogram för domänen namnet post toohello innan exponera tooyour användare.</span><span class="sxs-lookup"><span data-stu-id="104e9-141">You can map a custom domain name entry toohello web app before you expose it tooyour users.</span></span>

<span data-ttu-id="104e9-142">När hello web app definition är klar visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="104e9-142">When hello web app definition is ready, hello Azure CLI shows information similar toohello following example:</span></span> 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a><span data-ttu-id="104e9-143">Konfigurera Java</span><span class="sxs-lookup"><span data-stu-id="104e9-143">Configure Java</span></span> 

<span data-ttu-id="104e9-144">Konfigurera hello Java runtime konfiguration som din app behöver med hello [uppdatering az apptjänst web-config](/cli/azure/appservice/web/config#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="104e9-144">Set up hello Java runtime configuration that your app needs with hello  [az appservice web config update](/cli/azure/appservice/web/config#update) command.</span></span>

<span data-ttu-id="104e9-145">hello följande kommando konfigurerar hello web app toorun på en senaste Java 8 JDK och [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="104e9-145">hello following command configures hello web app toorun on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a><span data-ttu-id="104e9-146">Förbereda en GitHub-databas</span><span class="sxs-lookup"><span data-stu-id="104e9-146">Prepare a GitHub Repository</span></span>
<span data-ttu-id="104e9-147">Öppna hello [enkel Java-Webbapp för Azure](https://github.com/azure-devops/javawebappsample) lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="104e9-147">Open hello [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) repo.</span></span> <span data-ttu-id="104e9-148">toofork hello lagringsplatsen tooyour äger GitHub-konto, klickar du på hello **Återställningsförgreningar** knapp i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="104e9-148">toofork hello repo tooyour own GitHub account, click hello **Fork** button in hello top right-hand corner.</span></span>

* <span data-ttu-id="104e9-149">Öppna i GitHub-webbgränssnittet, **Jenkinsfile** fil.</span><span class="sxs-lookup"><span data-stu-id="104e9-149">In GitHub web UI, open **Jenkinsfile** file.</span></span> <span data-ttu-id="104e9-150">Klicka på hello penna ikonen tooedit den här filen tooupdate hello resursgruppens namn och namnet på ditt webbprogram på rad 20 och 21 respektive.</span><span class="sxs-lookup"><span data-stu-id="104e9-150">Click hello pencil icon tooedit this file tooupdate hello resource group and name of your web app on line 20 and 21 respectively.</span></span>

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* <span data-ttu-id="104e9-151">Ändra raden 23 tooupdate uppgifts-ID i Jenkins-instans</span><span class="sxs-lookup"><span data-stu-id="104e9-151">Change line 23 tooupdate credential ID in your Jenkins instance</span></span>

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a><span data-ttu-id="104e9-152">Skapa Jenkins pipeline</span><span class="sxs-lookup"><span data-stu-id="104e9-152">Create Jenkins pipeline</span></span>
<span data-ttu-id="104e9-153">Öppna Jenkins i en webbläsare, klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="104e9-153">Open Jenkins in a web browser, click **New Item**.</span></span> 

* <span data-ttu-id="104e9-154">Ange ett namn för hello jobbet och välja **Pipeline**.</span><span class="sxs-lookup"><span data-stu-id="104e9-154">Provide a name for hello job and select **Pipeline**.</span></span> <span data-ttu-id="104e9-155">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="104e9-155">Click **OK**.</span></span>
* <span data-ttu-id="104e9-156">Klicka på hello **Pipeline** fliken nästa.</span><span class="sxs-lookup"><span data-stu-id="104e9-156">Click hello **Pipeline** tab next.</span></span> 
* <span data-ttu-id="104e9-157">För **Definition**väljer **Pipeline-skriptet från SCM**.</span><span class="sxs-lookup"><span data-stu-id="104e9-157">For **Definition**, select **Pipeline script from SCM**.</span></span>
* <span data-ttu-id="104e9-158">För **SCM**väljer **Git**.</span><span class="sxs-lookup"><span data-stu-id="104e9-158">For **SCM**, select **Git**.</span></span>
* <span data-ttu-id="104e9-159">Ange hello GitHub-URL för din andelen vridvuxna lagringsplatsen: https:\<din andelen vridvuxna lagringsplatsen\>.git</span><span class="sxs-lookup"><span data-stu-id="104e9-159">Enter hello GitHub URL for your forked repo: https:\<your forked repo\>.git</span></span>
* <span data-ttu-id="104e9-160">Klicka på **spara**</span><span class="sxs-lookup"><span data-stu-id="104e9-160">Click **Save**</span></span>

## <a name="test-your-pipeline"></a><span data-ttu-id="104e9-161">Testa din pipeline</span><span class="sxs-lookup"><span data-stu-id="104e9-161">Test your pipeline</span></span>
* <span data-ttu-id="104e9-162">Gå toohello pipeline som du skapade **skapa nu**</span><span class="sxs-lookup"><span data-stu-id="104e9-162">Go toohello pipeline you created, click **Build Now**</span></span>
* <span data-ttu-id="104e9-163">En version ska lyckas i några sekunder och du kan gå toohello build och klicka på **konsolens utdata** toosee hello information</span><span class="sxs-lookup"><span data-stu-id="104e9-163">A build should succeed in a few seconds, and you can go toohello build and click **Console Output** toosee hello details</span></span>

## <a name="verify-your-web-app"></a><span data-ttu-id="104e9-164">Kontrollera ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="104e9-164">Verify your web app</span></span>
<span data-ttu-id="104e9-165">tooverify hello WAR-filen har distribueras tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="104e9-165">tooverify hello WAR file is deployed successfully tooyour web app.</span></span> <span data-ttu-id="104e9-166">Öppna en webbläsare:</span><span class="sxs-lookup"><span data-stu-id="104e9-166">Open a web browser:</span></span>

* <span data-ttu-id="104e9-167">Gå toohttp: / /&lt;programnamn >.azurewebsites.net/api/calculator/ping</span><span class="sxs-lookup"><span data-stu-id="104e9-167">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/ping</span></span>  
<span data-ttu-id="104e9-168">Du ser:</span><span class="sxs-lookup"><span data-stu-id="104e9-168">You see:</span></span>

        Welcome tooJava Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* <span data-ttu-id="104e9-169">Gå toohttp: / /&lt;appnamn >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (ersätta &lt;x > och &lt;y > med några siffror) tooget hello summan av x och y</span><span class="sxs-lookup"><span data-stu-id="104e9-169">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) tooget hello sum of x and y</span></span>

![Kalkylatorn: Lägg till](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-tooazure-web-app-on-linux"></a><span data-ttu-id="104e9-171">Distribuera tooAzure webbprogram på Linux</span><span class="sxs-lookup"><span data-stu-id="104e9-171">Deploy tooAzure Web App on Linux</span></span>
<span data-ttu-id="104e9-172">Nu när du vet hur toouse Azure CLI i din Jenkins pipeline, kan du ändra hello skriptet toodeploy tooan Azure Web App på Linux.</span><span class="sxs-lookup"><span data-stu-id="104e9-172">Now that you know how toouse Azure CLI in your Jenkins pipeline, you can modify hello script toodeploy tooan Azure Web App on Linux.</span></span>

<span data-ttu-id="104e9-173">Web App på Linux stöder en annorlunda sätt toodo hello distribution, vilket är toouse Docker.</span><span class="sxs-lookup"><span data-stu-id="104e9-173">Web App on Linux supports a different way toodo hello deployment, which is toouse Docker.</span></span> <span data-ttu-id="104e9-174">toodeploy, behöver du tooprovide en Dockerfile som ditt webbprogram med tjänsten runtime-paket till en Docker-bild.</span><span class="sxs-lookup"><span data-stu-id="104e9-174">toodeploy, you need tooprovide a Dockerfile that packages your web app with service runtime into a Docker image.</span></span> <span data-ttu-id="104e9-175">hello-plugin-programmet skapar hello-avbildning, push-installera den tooa Docker registret och därefter distribuera hello avbildningen tooyour webbapp.</span><span class="sxs-lookup"><span data-stu-id="104e9-175">hello plugin will then build hello image, push it tooa Docker registry and deploy hello image tooyour web app.</span></span>

* <span data-ttu-id="104e9-176">Gör hello [här](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate en Azure-Webbapp som körs på Linux.</span><span class="sxs-lookup"><span data-stu-id="104e9-176">Follow hello steps [here](/azure/app-service-web/app-service-linux-how-to-create-web-app) toocreate an Azure Web App running on Linux.</span></span>
* <span data-ttu-id="104e9-177">Installera Docker på Jenkins-instans genom att följa hello instruktionerna i det här [artikel](https://docs.docker.com/engine/installation/linux/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="104e9-177">Install Docker on your Jenkins instance by following hello instructions in this [article](https://docs.docker.com/engine/installation/linux/ubuntu/).</span></span>
* <span data-ttu-id="104e9-178">Skapa en behållare registret hello Azure-portalen med hjälp av hello steg [här](/azure/container-registry/container-registry-get-started-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="104e9-178">Create a Container Registry in hello Azure portal by using hello steps [here](/azure/container-registry/container-registry-get-started-azure-cli).</span></span>
* <span data-ttu-id="104e9-179">Hej i samma [enkel Java-Webbapp för Azure](https://github.com/azure-devops/javawebappsample) lagringsplatsen du forked redigera hello **Jenkinsfile2** fil:</span><span class="sxs-lookup"><span data-stu-id="104e9-179">In hello same [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) repo you forked, edit hello **Jenkinsfile2** file:</span></span>
    * <span data-ttu-id="104e9-180">Rad 18 21, uppdatera toohello namn på din resursgrupp, webbprogram och ACR respektive.</span><span class="sxs-lookup"><span data-stu-id="104e9-180">Line 18-21, update toohello names of your resource group, web app, and ACR respectively.</span></span> 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * <span data-ttu-id="104e9-181">Raden 24, uppdatera \<azsrvprincipal\> tooyour uppgifts-ID</span><span class="sxs-lookup"><span data-stu-id="104e9-181">Line 24, update \<azsrvprincipal\> tooyour credential ID</span></span>
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* <span data-ttu-id="104e9-182">Skapa en ny Jenkins pipeline som du gjorde när du distribuerar tooAzure webbapp i Windows, men den här gången Använd **Jenkinsfile2** i stället.</span><span class="sxs-lookup"><span data-stu-id="104e9-182">Create a new Jenkins pipeline as you did when deploying tooAzure web app in Windows, only this time, use **Jenkinsfile2** instead.</span></span>
* <span data-ttu-id="104e9-183">Kör ditt nya jobb.</span><span class="sxs-lookup"><span data-stu-id="104e9-183">Run your new job.</span></span>
* <span data-ttu-id="104e9-184">tooverify i Azure CLI kör:</span><span class="sxs-lookup"><span data-stu-id="104e9-184">tooverify, in Azure CLI, run:</span></span>

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    <span data-ttu-id="104e9-185">Du får hello följande resultat:</span><span class="sxs-lookup"><span data-stu-id="104e9-185">You get hello following result:</span></span>
    
    ```
    [
    "calculator"
    ]
    ```
    
    <span data-ttu-id="104e9-186">Gå toohttp: / /&lt;programnamn >.azurewebsites.net/api/calculator/ping.</span><span class="sxs-lookup"><span data-stu-id="104e9-186">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/ping.</span></span> <span data-ttu-id="104e9-187">Hello-meddelande visas:</span><span class="sxs-lookup"><span data-stu-id="104e9-187">You see hello message:</span></span> 
    
        Welcome tooJava Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    <span data-ttu-id="104e9-188">Gå toohttp: / /&lt;appnamn >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (ersätta &lt;x > och &lt;y > med några siffror) tooget hello summan av x och y</span><span class="sxs-lookup"><span data-stu-id="104e9-188">Go toohttp://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) tooget hello sum of x and y</span></span>
    
## <a name="next-steps"></a><span data-ttu-id="104e9-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="104e9-189">Next steps</span></span>
<span data-ttu-id="104e9-190">Du har konfigurerat en Jenkins pipeline som checkar ut hello källkoden i GitHub-repo i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="104e9-190">In this tutorial, you configured a Jenkins pipeline that checks out hello source code in GitHub repo.</span></span> <span data-ttu-id="104e9-191">Kör Maven toobuild en war-fil och sedan använder Azure CLI toodeploy tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="104e9-191">Runs Maven toobuild a war file and then uses Azure CLI toodeploy tooAzure App Service.</span></span> <span data-ttu-id="104e9-192">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="104e9-192">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="104e9-193">Skapa en virtuell dator Jenkins</span><span class="sxs-lookup"><span data-stu-id="104e9-193">Create a Jenkins VM</span></span>
> * <span data-ttu-id="104e9-194">Konfigurera Jenkins</span><span class="sxs-lookup"><span data-stu-id="104e9-194">Configure Jenkins</span></span>
> * <span data-ttu-id="104e9-195">Skapa en webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="104e9-195">Create a web app in Azure</span></span>
> * <span data-ttu-id="104e9-196">Förbereda en GitHub-databas</span><span class="sxs-lookup"><span data-stu-id="104e9-196">Prepare a GitHub repository</span></span>
> * <span data-ttu-id="104e9-197">Skapa Jenkins pipeline</span><span class="sxs-lookup"><span data-stu-id="104e9-197">Create Jenkins pipeline</span></span>
> * <span data-ttu-id="104e9-198">Kör hello pipeline och verifiera hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="104e9-198">Run hello pipeline and verify hello web app</span></span>
