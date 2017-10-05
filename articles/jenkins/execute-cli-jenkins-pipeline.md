---
title: "Köra Azure CLI med Jenkins | Microsoft Docs"
description: "Lär dig hur du använder Azure CLI för att distribuera en Java-webbapp till Azure i Jenkins Pipeline"
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
ms.openlocfilehash: 5ca8338d4bf343f08fe70081cff755fa76a126a9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-to-azure-app-service-with-jenkins-and-the-azure-cli"></a><span data-ttu-id="b9f06-103">Distribuera till Azure App Service med Jenkins och Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b9f06-103">Deploy to Azure App Service with Jenkins and the Azure CLI</span></span>
<span data-ttu-id="b9f06-104">Om du vill distribuera en Java-webbapp till Azure kan du använda Azure CLI i [Jenkins Pipeline](https://jenkins.io/doc/book/pipeline/).</span><span class="sxs-lookup"><span data-stu-id="b9f06-104">To deploy a Java web app to Azure, you can use Azure CLI in [Jenkins Pipeline](https://jenkins.io/doc/book/pipeline/).</span></span> <span data-ttu-id="b9f06-105">I den här självstudiekursen skapar du en CI/CD-pipeline på en Azure VM att:</span><span class="sxs-lookup"><span data-stu-id="b9f06-105">In this tutorial, you create a CI/CD pipeline on an Azure VM including how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b9f06-106">Skapa en virtuell dator Jenkins</span><span class="sxs-lookup"><span data-stu-id="b9f06-106">Create a Jenkins VM</span></span>
> * <span data-ttu-id="b9f06-107">Konfigurera Jenkins</span><span class="sxs-lookup"><span data-stu-id="b9f06-107">Configure Jenkins</span></span>
> * <span data-ttu-id="b9f06-108">Skapa en webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="b9f06-108">Create a web app in Azure</span></span>
> * <span data-ttu-id="b9f06-109">Förbereda en GitHub-databas</span><span class="sxs-lookup"><span data-stu-id="b9f06-109">Prepare a GitHub repository</span></span>
> * <span data-ttu-id="b9f06-110">Skapa Jenkins pipeline</span><span class="sxs-lookup"><span data-stu-id="b9f06-110">Create Jenkins pipeline</span></span>
> * <span data-ttu-id="b9f06-111">Kör pipeline och verifiera webbappen</span><span class="sxs-lookup"><span data-stu-id="b9f06-111">Run the pipeline and verify the web app</span></span>

<span data-ttu-id="b9f06-112">För den här självstudien krävs Azure CLI-version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b9f06-112">This tutorial requires the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="b9f06-113">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="b9f06-113">To find the version, run `az --version`.</span></span> <span data-ttu-id="b9f06-114">Om du behöver uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b9f06-114">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a><span data-ttu-id="b9f06-115">Skapa och konfigurera Jenkins instans</span><span class="sxs-lookup"><span data-stu-id="b9f06-115">Create and Configure Jenkins instance</span></span>
<span data-ttu-id="b9f06-116">Om du inte redan har en Jenkins master börja med den [Lösningsmall](install-jenkins-solution-template.md), som innehåller de nödvändiga [Azure-autentiseringsuppgifterna](https://plugins.jenkins.io/azure-credentials) plugin-programmet som standard.</span><span class="sxs-lookup"><span data-stu-id="b9f06-116">If you do not already have a Jenkins master, start with the [Solution Template](install-jenkins-solution-template.md), which includes the required [Azure Credentials](https://plugins.jenkins.io/azure-credentials) plugin by default.</span></span> 

<span data-ttu-id="b9f06-117">Azure-autentiseringsuppgifter plugin-programmet kan du lagra Microsoft Azure service principal autentiseringsuppgifter i Jenkins.</span><span class="sxs-lookup"><span data-stu-id="b9f06-117">The Azure Credential plugin allows you to store Microsoft Azure service principal credentials in Jenkins.</span></span> <span data-ttu-id="b9f06-118">I version 1.2, vi har lagt till stöd så att Jenkins-Pipeline kan hämta autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="b9f06-118">In version 1.2, we added the support so that Jenkins Pipeline can get the Azure credentials.</span></span> 

<span data-ttu-id="b9f06-119">Se till att du har version 1.2 eller senare:</span><span class="sxs-lookup"><span data-stu-id="b9f06-119">Ensure you have version 1.2 or later:</span></span>
* <span data-ttu-id="b9f06-120">I instrumentpanelen Jenkins klickar du på **Plugin-hanteraren -> Hantera Jenkins ->** och Sök efter **Azure autentiseringsuppgifter**.</span><span class="sxs-lookup"><span data-stu-id="b9f06-120">Within the Jenkins dashboard, click **Manage Jenkins -> Plugin Manager ->** and search for **Azure Credential**.</span></span> 
* <span data-ttu-id="b9f06-121">Uppdatera plugin-programmet om versionen är tidigare än 1.2.</span><span class="sxs-lookup"><span data-stu-id="b9f06-121">Update the plugin if the version is earlier than 1.2.</span></span>

<span data-ttu-id="b9f06-122">JDK Java och Maven också krävs i Jenkins master.</span><span class="sxs-lookup"><span data-stu-id="b9f06-122">Java JDK and Maven are also required in the Jenkins master.</span></span> <span data-ttu-id="b9f06-123">För att installera, logga in på Jenkins master med SSH och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="b9f06-123">To install, log in to Jenkins master using SSH and run the following commands:</span></span>
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-to-jenkins-credential"></a><span data-ttu-id="b9f06-124">Lägg till Azure-tjänstens huvudnamn i Jenkins autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="b9f06-124">Add Azure service principal to Jenkins credential</span></span>

<span data-ttu-id="b9f06-125">Azure autentiseringsuppgifter krävs för att köra Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b9f06-125">An Azure credential is needed to execute Azure CLI.</span></span>

* <span data-ttu-id="b9f06-126">I instrumentpanelen Jenkins klickar du på **autentiseringsuppgifter -> System ->**.</span><span class="sxs-lookup"><span data-stu-id="b9f06-126">Within the Jenkins dashboard, click **Credentials -> System ->**.</span></span> <span data-ttu-id="b9f06-127">Klicka på **globala credentials(unrestricted)**.</span><span class="sxs-lookup"><span data-stu-id="b9f06-127">Click **Global credentials(unrestricted)**.</span></span>
* <span data-ttu-id="b9f06-128">Klicka på **lägga till autentiseringsuppgifter** att lägga till en [Microsoft Azure-tjänstens huvudnamn](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) genom att fylla i prenumerations-ID, klient-ID, Klienthemligheten och OAuth 2.0-Token för slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="b9f06-128">Click **Add Credentials** to add a [Microsoft Azure service principal](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) by filling out the Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span> <span data-ttu-id="b9f06-129">Ange ett ID för användning i senare steg.</span><span class="sxs-lookup"><span data-stu-id="b9f06-129">Provide an ID for use in subsequent step.</span></span>

![Lägga till autentiseringsuppgifter](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-the-java-web-app"></a><span data-ttu-id="b9f06-131">Skapa en Azure App Service för att distribuera Java-webbapp</span><span class="sxs-lookup"><span data-stu-id="b9f06-131">Create an Azure App Service for deploying the Java web app</span></span>

<span data-ttu-id="b9f06-132">Skapa en Azure App Service-plan med den **lediga** priser nivå med hjälp av den [az programtjänstplan skapa](/cli/azure/appservice/plan#create) CLI-kommando.</span><span class="sxs-lookup"><span data-stu-id="b9f06-132">Create an Azure App Service plan with the **FREE** pricing tier using the  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="b9f06-133">Programtjänstplan definierar de fysiska resurserna som används som värd för dina appar.</span><span class="sxs-lookup"><span data-stu-id="b9f06-133">The appservice plan defines the physical resources used to host your apps.</span></span> <span data-ttu-id="b9f06-134">Alla program som har tilldelats en programtjänstplan dela dessa resurser, så att du kan spara kostnader när värd för flera appar.</span><span class="sxs-lookup"><span data-stu-id="b9f06-134">All applications assigned to an appservice plan share these resources, allowing you to save cost when hosting multiple apps.</span></span> 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

<span data-ttu-id="b9f06-135">När planen är klar, visas Azure CLI liknande utdata i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b9f06-135">When the plan is ready, the Azure CLI shows similar output to the following example:</span></span>

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

### <a name="create-an-azure-web-app"></a><span data-ttu-id="b9f06-136">Skapa en Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="b9f06-136">Create an Azure Web app</span></span>

 <span data-ttu-id="b9f06-137">Använd den [az webapp skapa](/cli/azure/appservice/web#create) CLI-kommando för att skapa en web app definition i den `myAppServicePlan` App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="b9f06-137">Use the [az webapp create](/cli/azure/appservice/web#create) CLI command to create a web app definition in the `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="b9f06-138">Web app definitionen innehåller en URL för att få åtkomst till ditt program med och konfigurerar du flera alternativ för att distribuera din kod till Azure.</span><span class="sxs-lookup"><span data-stu-id="b9f06-138">The web app definition provides a URL to access your application with and configures several options to deploy your code to Azure.</span></span> 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

<span data-ttu-id="b9f06-139">Ersätt den `<app_name>` med dina egna unika namn.</span><span class="sxs-lookup"><span data-stu-id="b9f06-139">Substitute the `<app_name>` placeholder with your own unique app name.</span></span> <span data-ttu-id="b9f06-140">Detta unika namn är en del av standarddomännamnet för webbprogram, så att namnet måste vara unikt över alla program i Azure.</span><span class="sxs-lookup"><span data-stu-id="b9f06-140">This unique name is part of the default domain name for the web app, so the name needs to be unique across all apps in Azure.</span></span> <span data-ttu-id="b9f06-141">Du kan mappa en anpassad domän namnpost till webbprogrammet innan du ansluter till dina användare.</span><span class="sxs-lookup"><span data-stu-id="b9f06-141">You can map a custom domain name entry to the web app before you expose it to your users.</span></span>

<span data-ttu-id="b9f06-142">När web app definitionen är klar, visas Azure CLI information liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b9f06-142">When the web app definition is ready, the Azure CLI shows information similar to the following example:</span></span> 

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

### <a name="configure-java"></a><span data-ttu-id="b9f06-143">Konfigurera Java</span><span class="sxs-lookup"><span data-stu-id="b9f06-143">Configure Java</span></span> 

<span data-ttu-id="b9f06-144">Ställa in Java runtime-konfiguration som din app behöver med den [uppdatering az apptjänst web-config](/cli/azure/appservice/web/config#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="b9f06-144">Set up the Java runtime configuration that your app needs with the  [az appservice web config update](/cli/azure/appservice/web/config#update) command.</span></span>

<span data-ttu-id="b9f06-145">Följande kommando konfigurerar webbprogram för att köra på en senaste Java 8 JDK och [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="b9f06-145">The following command configures the web app to run on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a><span data-ttu-id="b9f06-146">Förbereda en GitHub-databas</span><span class="sxs-lookup"><span data-stu-id="b9f06-146">Prepare a GitHub Repository</span></span>
<span data-ttu-id="b9f06-147">Öppna den [enkel Java-Webbapp för Azure](https://github.com/azure-devops/javawebappsample) lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="b9f06-147">Open the [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) repo.</span></span> <span data-ttu-id="b9f06-148">Om du vill duplicera lagringsplatsen till ditt GitHub-konto, klickar du på den **Återställningsförgreningar** -knappen i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="b9f06-148">To fork the repo to your own GitHub account, click the **Fork** button in the top right-hand corner.</span></span>

* <span data-ttu-id="b9f06-149">Öppna i GitHub-webbgränssnittet, **Jenkinsfile** fil.</span><span class="sxs-lookup"><span data-stu-id="b9f06-149">In GitHub web UI, open **Jenkinsfile** file.</span></span> <span data-ttu-id="b9f06-150">Klicka på pennikonen om du vill redigera den här filen om du vill uppdatera den resursgrupp och namnet på ditt webbprogram på rad 20 och 21 respektive.</span><span class="sxs-lookup"><span data-stu-id="b9f06-150">Click the pencil icon to edit this file to update the resource group and name of your web app on line 20 and 21 respectively.</span></span>

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* <span data-ttu-id="b9f06-151">Ändra raden 23 att uppdatera uppgifts-ID i Jenkins-instans</span><span class="sxs-lookup"><span data-stu-id="b9f06-151">Change line 23 to update credential ID in your Jenkins instance</span></span>

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a><span data-ttu-id="b9f06-152">Skapa Jenkins pipeline</span><span class="sxs-lookup"><span data-stu-id="b9f06-152">Create Jenkins pipeline</span></span>
<span data-ttu-id="b9f06-153">Öppna Jenkins i en webbläsare, klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="b9f06-153">Open Jenkins in a web browser, click **New Item**.</span></span> 

* <span data-ttu-id="b9f06-154">Ange ett namn för jobbet och välj **Pipeline**.</span><span class="sxs-lookup"><span data-stu-id="b9f06-154">Provide a name for the job and select **Pipeline**.</span></span> <span data-ttu-id="b9f06-155">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b9f06-155">Click **OK**.</span></span>
* <span data-ttu-id="b9f06-156">Klicka på den **Pipeline** fliken nästa.</span><span class="sxs-lookup"><span data-stu-id="b9f06-156">Click the **Pipeline** tab next.</span></span> 
* <span data-ttu-id="b9f06-157">För **Definition**väljer **Pipeline-skriptet från SCM**.</span><span class="sxs-lookup"><span data-stu-id="b9f06-157">For **Definition**, select **Pipeline script from SCM**.</span></span>
* <span data-ttu-id="b9f06-158">För **SCM**väljer **Git**.</span><span class="sxs-lookup"><span data-stu-id="b9f06-158">For **SCM**, select **Git**.</span></span>
* <span data-ttu-id="b9f06-159">Ange GitHub-URL för din andelen vridvuxna lagringsplatsen: https:\<din andelen vridvuxna lagringsplatsen\>.git</span><span class="sxs-lookup"><span data-stu-id="b9f06-159">Enter the GitHub URL for your forked repo: https:\<your forked repo\>.git</span></span>
* <span data-ttu-id="b9f06-160">Klicka på **spara**</span><span class="sxs-lookup"><span data-stu-id="b9f06-160">Click **Save**</span></span>

## <a name="test-your-pipeline"></a><span data-ttu-id="b9f06-161">Testa din pipeline</span><span class="sxs-lookup"><span data-stu-id="b9f06-161">Test your pipeline</span></span>
* <span data-ttu-id="b9f06-162">Gå till pipelinen som du skapade **skapa nu**</span><span class="sxs-lookup"><span data-stu-id="b9f06-162">Go to the pipeline you created, click **Build Now**</span></span>
* <span data-ttu-id="b9f06-163">En version ska lyckas i några sekunder och du kan gå att bygga och klicka på **konsolens utdata** att visa detaljer</span><span class="sxs-lookup"><span data-stu-id="b9f06-163">A build should succeed in a few seconds, and you can go to the build and click **Console Output** to see the details</span></span>

## <a name="verify-your-web-app"></a><span data-ttu-id="b9f06-164">Kontrollera ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="b9f06-164">Verify your web app</span></span>
<span data-ttu-id="b9f06-165">Om du vill verifiera WAR distribuerats filen till ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b9f06-165">To verify the WAR file is deployed successfully to your web app.</span></span> <span data-ttu-id="b9f06-166">Öppna en webbläsare:</span><span class="sxs-lookup"><span data-stu-id="b9f06-166">Open a web browser:</span></span>

* <span data-ttu-id="b9f06-167">Gå till http://&lt;programnamn >.azurewebsites.net/api/calculator/ping</span><span class="sxs-lookup"><span data-stu-id="b9f06-167">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/ping</span></span>  
<span data-ttu-id="b9f06-168">Du ser:</span><span class="sxs-lookup"><span data-stu-id="b9f06-168">You see:</span></span>

        Welcome to Java Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* <span data-ttu-id="b9f06-169">Gå till http://&lt;appnamn >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (ersätta &lt;x > och &lt;y > med några siffror) att hämta summan av x och y</span><span class="sxs-lookup"><span data-stu-id="b9f06-169">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) to get the sum of x and y</span></span>

![Kalkylatorn: Lägg till](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-to-azure-web-app-on-linux"></a><span data-ttu-id="b9f06-171">Distribuera till Azure-Webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="b9f06-171">Deploy to Azure Web App on Linux</span></span>
<span data-ttu-id="b9f06-172">Nu när du vet hur du använder Azure CLI i Jenkins-pipeline kan du ändra skriptet för att distribuera till en Azure-Webbapp på Linux.</span><span class="sxs-lookup"><span data-stu-id="b9f06-172">Now that you know how to use Azure CLI in your Jenkins pipeline, you can modify the script to deploy to an Azure Web App on Linux.</span></span>

<span data-ttu-id="b9f06-173">Web App på Linux stöder olika sätt att göra distribution, vilket är att använda Docker.</span><span class="sxs-lookup"><span data-stu-id="b9f06-173">Web App on Linux supports a different way to do the deployment, which is to use Docker.</span></span> <span data-ttu-id="b9f06-174">Om du vill distribuera, som du behöver ange en Dockerfile som ditt webbprogram med tjänsten runtime-paket till en Docker-bild.</span><span class="sxs-lookup"><span data-stu-id="b9f06-174">To deploy, you need to provide a Dockerfile that packages your web app with service runtime into a Docker image.</span></span> <span data-ttu-id="b9f06-175">Plugin-programmet skapa avbildningen, push-installera den en Docker-registret och därefter distribuera avbildningen till ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b9f06-175">The plugin will then build the image, push it to a Docker registry and deploy the image to your web app.</span></span>

* <span data-ttu-id="b9f06-176">Följ stegen [här](/azure/app-service-web/app-service-linux-how-to-create-web-app) att skapa en Azure-Webbapp som körs på Linux.</span><span class="sxs-lookup"><span data-stu-id="b9f06-176">Follow the steps [here](/azure/app-service-web/app-service-linux-how-to-create-web-app) to create an Azure Web App running on Linux.</span></span>
* <span data-ttu-id="b9f06-177">Installera Docker på Jenkins-instans genom att följa anvisningarna i det här [artikel](https://docs.docker.com/engine/installation/linux/ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="b9f06-177">Install Docker on your Jenkins instance by following the instructions in this [article](https://docs.docker.com/engine/installation/linux/ubuntu/).</span></span>
* <span data-ttu-id="b9f06-178">Skapa en behållare registret i Azure-portalen med hjälp av stegen [här](/azure/container-registry/container-registry-get-started-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b9f06-178">Create a Container Registry in the Azure portal by using the steps [here](/azure/container-registry/container-registry-get-started-azure-cli).</span></span>
* <span data-ttu-id="b9f06-179">I samma [enkel Java-Webbapp för Azure](https://github.com/azure-devops/javawebappsample) lagringsplatsen forked du kan redigera den **Jenkinsfile2** fil:</span><span class="sxs-lookup"><span data-stu-id="b9f06-179">In the same [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) repo you forked, edit the **Jenkinsfile2** file:</span></span>
    * <span data-ttu-id="b9f06-180">Rad 18 21, uppdatera till namnen på din resursgrupp, webbprogram och ACR respektive.</span><span class="sxs-lookup"><span data-stu-id="b9f06-180">Line 18-21, update to the names of your resource group, web app, and ACR respectively.</span></span> 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * <span data-ttu-id="b9f06-181">Raden 24, uppdatera \<azsrvprincipal\> till din uppgifts-ID</span><span class="sxs-lookup"><span data-stu-id="b9f06-181">Line 24, update \<azsrvprincipal\> to your credential ID</span></span>
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* <span data-ttu-id="b9f06-182">Skapa en ny Jenkins pipeline som du när du distribuerar till Azure-webbapp i Windows, men den här gången använder **Jenkinsfile2** i stället.</span><span class="sxs-lookup"><span data-stu-id="b9f06-182">Create a new Jenkins pipeline as you did when deploying to Azure web app in Windows, only this time, use **Jenkinsfile2** instead.</span></span>
* <span data-ttu-id="b9f06-183">Kör ditt nya jobb.</span><span class="sxs-lookup"><span data-stu-id="b9f06-183">Run your new job.</span></span>
* <span data-ttu-id="b9f06-184">Om du vill verifiera i Azure CLI kör du:</span><span class="sxs-lookup"><span data-stu-id="b9f06-184">To verify, in Azure CLI, run:</span></span>

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    <span data-ttu-id="b9f06-185">Du får följande resultat:</span><span class="sxs-lookup"><span data-stu-id="b9f06-185">You get the following result:</span></span>
    
    ```
    [
    "calculator"
    ]
    ```
    
    <span data-ttu-id="b9f06-186">Gå till http://&lt;programnamn >.azurewebsites.net/api/calculator/ping.</span><span class="sxs-lookup"><span data-stu-id="b9f06-186">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/ping.</span></span> <span data-ttu-id="b9f06-187">Du ser meddelandet:</span><span class="sxs-lookup"><span data-stu-id="b9f06-187">You see the message:</span></span> 
    
        Welcome to Java Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    <span data-ttu-id="b9f06-188">Gå till http://&lt;appnamn >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (ersätta &lt;x > och &lt;y > med några siffror) att hämta summan av x och y</span><span class="sxs-lookup"><span data-stu-id="b9f06-188">Go to http://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (substitute &lt;x> and &lt;y> with any numbers) to get the sum of x and y</span></span>
    
## <a name="next-steps"></a><span data-ttu-id="b9f06-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9f06-189">Next steps</span></span>
<span data-ttu-id="b9f06-190">Du har konfigurerat en Jenkins pipeline som checkar ut källkoden i GitHub-repo i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="b9f06-190">In this tutorial, you configured a Jenkins pipeline that checks out the source code in GitHub repo.</span></span> <span data-ttu-id="b9f06-191">Kör Maven för att skapa en war-fil och sedan använder Azure CLI för att distribuera till Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b9f06-191">Runs Maven to build a war file and then uses Azure CLI to deploy to Azure App Service.</span></span> <span data-ttu-id="b9f06-192">Du har lärt dig hur till:</span><span class="sxs-lookup"><span data-stu-id="b9f06-192">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b9f06-193">Skapa en virtuell dator Jenkins</span><span class="sxs-lookup"><span data-stu-id="b9f06-193">Create a Jenkins VM</span></span>
> * <span data-ttu-id="b9f06-194">Konfigurera Jenkins</span><span class="sxs-lookup"><span data-stu-id="b9f06-194">Configure Jenkins</span></span>
> * <span data-ttu-id="b9f06-195">Skapa en webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="b9f06-195">Create a web app in Azure</span></span>
> * <span data-ttu-id="b9f06-196">Förbereda en GitHub-databas</span><span class="sxs-lookup"><span data-stu-id="b9f06-196">Prepare a GitHub repository</span></span>
> * <span data-ttu-id="b9f06-197">Skapa Jenkins pipeline</span><span class="sxs-lookup"><span data-stu-id="b9f06-197">Create Jenkins pipeline</span></span>
> * <span data-ttu-id="b9f06-198">Kör pipeline och verifiera webbappen</span><span class="sxs-lookup"><span data-stu-id="b9f06-198">Run the pipeline and verify the web app</span></span>
