---
title: "Distribuera en App för start av värdet på Kubernetes i Azure Container Service | Microsoft Docs"
description: "Den här kursen får du om stegen för att distribuera en källan startprogrammet i en Kubernetes kluster på Microsoft Azure."
services: container-service
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: 7f726436b2d459b8c16abb02e07de099abfd8974
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-container-service"></a><span data-ttu-id="82375-103">Distribuera ett Spring Boot-program på ett Kubernetes-kluster i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="82375-103">Deploy a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service</span></span>

<span data-ttu-id="82375-104">Den  **[Vårversionen Framework]**  ett populära ramverk för öppen källkod som gör att Java-utvecklare skapa webb-, mobil- och API-program.</span><span class="sxs-lookup"><span data-stu-id="82375-104">The **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="82375-105">Den här kursen använder en exempelapp som skapats med hjälp av [Vårversionen Start], en konvention inriktat tillvägagångssätt för att komma igång snabbt med källan.</span><span class="sxs-lookup"><span data-stu-id="82375-105">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring to get started quickly.</span></span>

<span data-ttu-id="82375-106">**[Kubernetes]**  och  **[Docker]**  är öppen källkod som hjälper utvecklare att automatisera distribution, skalning och hantering av de program som körs i behållare.</span><span class="sxs-lookup"><span data-stu-id="82375-106">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate the deployment, scaling, and management of their applications  running in containers.</span></span>

<span data-ttu-id="82375-107">Den här vägledningen visar dig om att kombinera dessa två populära, öppen källkod tekniker för att utveckla och distribuera en startprogrammet för källan till Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="82375-107">This tutorial walks you though combining these two popular, open-source technologies to develop and deploy a Spring Boot application to Microsoft Azure.</span></span> <span data-ttu-id="82375-108">Mer specifikt du använder  *[Vårversionen Start]*  för programutveckling,  *[Kubernetes]*  för distribution av behållaren och [Azure Container Service (ACS)] som värd för ditt program.</span><span class="sxs-lookup"><span data-stu-id="82375-108">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and the [Azure Container Service (ACS)] to host your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="82375-109">Krav</span><span class="sxs-lookup"><span data-stu-id="82375-109">Prerequisites</span></span>

* <span data-ttu-id="82375-110">En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].</span><span class="sxs-lookup"><span data-stu-id="82375-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="82375-111">Den [Azure-kommandoradsgränssnittet (CLI)].</span><span class="sxs-lookup"><span data-stu-id="82375-111">The [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="82375-112">En uppdaterad [Java Developer Kit (JDK)].</span><span class="sxs-lookup"><span data-stu-id="82375-112">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="82375-113">Apache's [Maven] skapa verktyget (Version 3).</span><span class="sxs-lookup"><span data-stu-id="82375-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="82375-114">En [Git] klienten.</span><span class="sxs-lookup"><span data-stu-id="82375-114">A [Git] client.</span></span>
* <span data-ttu-id="82375-115">En [Docker] klienten.</span><span class="sxs-lookup"><span data-stu-id="82375-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="82375-116">På grund av virtualisering kraven i den här självstudiekursen, kan du följa stegen i den här artikeln på en virtuell dator; Du måste använda en fysisk dator med aktiverat funktionerna för virtualisering.</span><span class="sxs-lookup"><span data-stu-id="82375-116">Due to the virtualization requirements of this tutorial, you cannot follow the steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="82375-117">Skapa källan Start på Docker komma igång webbprogram</span><span class="sxs-lookup"><span data-stu-id="82375-117">Create the Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="82375-118">Följande steg beskriver hur du skapar ett webbprogram i vår start och testa dem lokalt.</span><span class="sxs-lookup"><span data-stu-id="82375-118">The following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="82375-119">Öppna en kommandotolk och skapa en lokal katalog för att hålla ditt program och ändra till katalogen; Exempel:</span><span class="sxs-lookup"><span data-stu-id="82375-119">Open a command-prompt and create a local directory to hold your application, and change to that directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="82375-120">-- eller--</span><span class="sxs-lookup"><span data-stu-id="82375-120">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="82375-121">Klona den [Vårversionen Start på Docker komma igång] exempelprojektet till katalogen.</span><span class="sxs-lookup"><span data-stu-id="82375-121">Clone the [Spring Boot on Docker Getting Started] sample project into the directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="82375-122">Ändra katalogen till projektet slutfördes.</span><span class="sxs-lookup"><span data-stu-id="82375-122">Change directory to the completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="82375-123">Använd Maven för att skapa och köra sample-appen.</span><span class="sxs-lookup"><span data-stu-id="82375-123">Use Maven to build and run the sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="82375-124">Testa webbappen genom att bläddra till http://localhost: 8080 eller med följande `curl` kommando:</span><span class="sxs-lookup"><span data-stu-id="82375-124">Test the web app by browsing to http://localhost:8080, or with the following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="82375-125">Du bör se följande meddelande visas: **Hello Docker World**</span><span class="sxs-lookup"><span data-stu-id="82375-125">You should see the following message displayed: **Hello Docker World**</span></span>

   ![Bläddra Sample-appen lokalt][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a><span data-ttu-id="82375-127">Skapa ett Azure Container registret med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="82375-127">Create an Azure Container Registry using the Azure CLI</span></span>

1. <span data-ttu-id="82375-128">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="82375-128">Open a command prompt.</span></span>

1. <span data-ttu-id="82375-129">Logga in på ditt Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="82375-129">Log in to your Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="82375-130">Skapa en resursgrupp för Azure-resurser används i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="82375-130">Create a resource group for the Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="82375-131">Skapa ett register för privat Azure-behållaren i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="82375-131">Create a private Azure container registry in the resource group.</span></span> <span data-ttu-id="82375-132">Självstudiekursen skickar sample-appen som en Docker-avbildning till den här registernyckeln i senare steg.</span><span class="sxs-lookup"><span data-stu-id="82375-132">The tutorial pushes the sample app as a Docker image to this registry in later steps.</span></span> <span data-ttu-id="82375-133">Ersätt `wingtiptoysregistry` med ett unikt namn för registret.</span><span class="sxs-lookup"><span data-stu-id="82375-133">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a><span data-ttu-id="82375-134">Push-appen till behållaren registret</span><span class="sxs-lookup"><span data-stu-id="82375-134">Push your app to the container registry</span></span>

1. <span data-ttu-id="82375-135">Navigera till konfigurationskatalogen för Maven-installationen (standard ~/.m2/ eller C:\Users\username\.m2) och öppna den *settings.xml* fil med en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="82375-135">Navigate to the configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open the *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="82375-136">Hämta lösenordet för behållaren registret från Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="82375-136">Retrieve the password for your container registry from the Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="82375-137">Lägg till ditt Azure-behållare registret id och lösenord till en ny `<server>` samling i den *settings.xml* fil.</span><span class="sxs-lookup"><span data-stu-id="82375-137">Add your Azure Container Registry id and password to a new `<server>` collection in the *settings.xml* file.</span></span>
<span data-ttu-id="82375-138">Den `id` och `username` är namnet på registret.</span><span class="sxs-lookup"><span data-stu-id="82375-138">The `id` and `username` are the name of the registry.</span></span> <span data-ttu-id="82375-139">Använd den `password` värde från det föregående kommandot (utan citattecken).</span><span class="sxs-lookup"><span data-stu-id="82375-139">Use the `password` value from the previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="82375-140">Navigera till slutförda projektkatalogen för tillämpningsprogrammet källan start (till exempel ”*C:\SpringBoot\gs-spring-boot-docker\complete*” eller ”*/users/robert/SpringBoot/gs-spring-boot-docker/complete*”), och öppna den *pom.xml* fil med en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="82375-140">Navigate to the completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open the *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="82375-141">Uppdatering av `<properties>` samling i den *pom.xml* fil med inloggningen servervärdet för registernyckeln din Azure-behållare.</span><span class="sxs-lookup"><span data-stu-id="82375-141">Update the `<properties>` collection in the *pom.xml* file with the login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="82375-142">Uppdatera den `<plugins>` samling i den *pom.xml* filen så att den `<plugin>` innehåller inloggningen adress och registret servernamnet för din Azure-behållare registernyckeln.</span><span class="sxs-lookup"><span data-stu-id="82375-142">Update the `<plugins>` collection in the *pom.xml* file so that the `<plugin>` contains the login server address and registry name for your Azure Container Registry.</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. <span data-ttu-id="82375-143">Navigera till slutförda projektkatalogen för tillämpningsprogrammet källan start och kör följande kommando för att skapa Docker-behållaren och push-avbildningen i registret:</span><span class="sxs-lookup"><span data-stu-id="82375-143">Navigate to the completed project directory for your Spring Boot application and run the following command to build the Docker container and push the image to the registry:</span></span>

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  <span data-ttu-id="82375-144">Du får ett felmeddelande som liknar något av följande när Maven skickar avbildningen till Azure:</span><span class="sxs-lookup"><span data-stu-id="82375-144">You may receive an error message that is similar to one of the following when Maven pushes the image to Azure:</span></span>
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="82375-145">Om du får det här felet kan logga in Azure från Docker-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="82375-145">If you get this error, log in to Azure from the Docker command line.</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="82375-146">Sedan push din behållare:</span><span class="sxs-lookup"><span data-stu-id="82375-146">Then push your container:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-the-azure-cli"></a><span data-ttu-id="82375-147">Skapa ett Kubernetes kluster på ACS med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="82375-147">Create a Kubernetes Cluster on ACS using the Azure CLI</span></span>

1. <span data-ttu-id="82375-148">Skapa ett Kubernetes-kluster i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="82375-148">Create a Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="82375-149">Följande kommando skapar en *kubernetes* klustret i den *VingspetsLeksaker kubernetes* resursen med *VingspetsLeksaker containerservice* som klusternamnet och *VingspetsLeksaker kubernetes* som DNS-prefix:</span><span class="sxs-lookup"><span data-stu-id="82375-149">The following command creates a *kubernetes* cluster in the *wingtiptoys-kubernetes* resource group, with *wingtiptoys-containerservice* as the cluster name, and *wingtiptoys-kubernetes* as the DNS prefix:</span></span>
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   <span data-ttu-id="82375-150">Det här kommandot kan ta en stund att slutföra.</span><span class="sxs-lookup"><span data-stu-id="82375-150">This command may take a while to complete.</span></span>

1. <span data-ttu-id="82375-151">Installera `kubectl` med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="82375-151">Install `kubectl` using the Azure CLI.</span></span> <span data-ttu-id="82375-152">Linux-användare kan ha som prefix i det här kommandot med `sudo` eftersom den distribuerar Kubernetes CLI till `/usr/local/bin`.</span><span class="sxs-lookup"><span data-stu-id="82375-152">Linux users may have to prefix this command with `sudo` since it deploys the Kubernetes CLI to `/usr/local/bin`.</span></span>
   ```azurecli
   az acs kubernetes install-cli
   ```

1. <span data-ttu-id="82375-153">Hämta konfigurationsinformation för klustret så att du kan hantera klustret från webbgränssnittet Kubernetes och `kubectl`.</span><span class="sxs-lookup"><span data-stu-id="82375-153">Download the cluster configuration information so you can manage your cluster from the Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a><span data-ttu-id="82375-154">Distribuera avbildningen till Kubernetes-kluster</span><span class="sxs-lookup"><span data-stu-id="82375-154">Deploy the image to your Kubernetes cluster</span></span>

<span data-ttu-id="82375-155">Den här kursen distribuerar appen med `kubectl`, kan du utforska distributionens tillstånd via webbgränssnittet Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="82375-155">This tutorial deploys the app using `kubectl`, then allow you to explore the deployment through the Kubernetes web interface.</span></span>

### <a name="deploy-with-the-kubernetes-web-interface"></a><span data-ttu-id="82375-156">Distribuera med webbgränssnitt Kubernetes</span><span class="sxs-lookup"><span data-stu-id="82375-156">Deploy with the Kubernetes web interface</span></span>

1. <span data-ttu-id="82375-157">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="82375-157">Open a command prompt.</span></span>

1. <span data-ttu-id="82375-158">Öppna webbplatsen konfiguration för klustret Kubernetes i standardwebbläsaren:</span><span class="sxs-lookup"><span data-stu-id="82375-158">Open the configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. <span data-ttu-id="82375-159">När webbplatsen Kubernetes configuration öppnas i webbläsaren, klicka på länken till **distribuera en behållare app**:</span><span class="sxs-lookup"><span data-stu-id="82375-159">When the Kubernetes configuration website opens in your browser, click the link to **deploy a containerized app**:</span></span>

   ![Kubernetes Configuration webbplats][KB01]

1. <span data-ttu-id="82375-161">När den **distribuera en behållare app** visas, anger du följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="82375-161">When the **Deploy a containerized app** page is displayed, specify the following options:</span></span>

   <span data-ttu-id="82375-162">a.</span><span class="sxs-lookup"><span data-stu-id="82375-162">a.</span></span> <span data-ttu-id="82375-163">Välj **ange information om appar nedan**.</span><span class="sxs-lookup"><span data-stu-id="82375-163">Select **Specify app details below**.</span></span>

   <span data-ttu-id="82375-164">b.</span><span class="sxs-lookup"><span data-stu-id="82375-164">b.</span></span> <span data-ttu-id="82375-165">Ange programnamnet källan Start för den **appnamn**, till exempel ”:*gs-källan-Start-docker*”.</span><span class="sxs-lookup"><span data-stu-id="82375-165">Enter your Spring Boot application name for the **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="82375-166">c.</span><span class="sxs-lookup"><span data-stu-id="82375-166">c.</span></span> <span data-ttu-id="82375-167">Ange din inloggning server och en behållare avbildningen från tidigare för den **behållaren image**, till exempel ”:*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*”.</span><span class="sxs-lookup"><span data-stu-id="82375-167">Enter your login server and container image from earlier for the **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="82375-168">d.</span><span class="sxs-lookup"><span data-stu-id="82375-168">d.</span></span> <span data-ttu-id="82375-169">Välj **externa** för den **tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="82375-169">Choose **External** for the **Service**.</span></span>

   <span data-ttu-id="82375-170">e.</span><span class="sxs-lookup"><span data-stu-id="82375-170">e.</span></span> <span data-ttu-id="82375-171">Ange din externa och interna portar i den **Port** och **mål port** textrutor.</span><span class="sxs-lookup"><span data-stu-id="82375-171">Specify your external and internal ports in the **Port** and **Target port** text boxes.</span></span>

   ![Kubernetes Configuration webbplats][KB02]


1. <span data-ttu-id="82375-173">Klicka på **distribuera** att distribuera behållaren.</span><span class="sxs-lookup"><span data-stu-id="82375-173">Click **Deploy** to deploy the container.</span></span>

   ![Distribuera behållare][KB05]

1. <span data-ttu-id="82375-175">När programmet har distribuerats visas källan Start programmet visas under **Services**.</span><span class="sxs-lookup"><span data-stu-id="82375-175">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Kubernetes tjänster][KB06]

1. <span data-ttu-id="82375-177">Om du klickar på länken för **externa slutpunkter**, du kan se din källan Start-program som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="82375-177">If you click the link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Kubernetes tjänster][KB07]

   ![Bläddra Sample-appen på Azure][SB02]


### <a name="deploy-with-kubectl"></a><span data-ttu-id="82375-180">Distribuera med kubectl</span><span class="sxs-lookup"><span data-stu-id="82375-180">Deploy with kubectl</span></span>

1. <span data-ttu-id="82375-181">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="82375-181">Open a command prompt.</span></span>

1. <span data-ttu-id="82375-182">Kör ditt behållare i Kubernetes klustret genom att använda den `kubectl run` kommando.</span><span class="sxs-lookup"><span data-stu-id="82375-182">Run your container in the Kubernetes cluster by using the `kubectl run` command.</span></span> <span data-ttu-id="82375-183">Ange ett namn för din app i Kubernetes och fullständig avbildningens namn.</span><span class="sxs-lookup"><span data-stu-id="82375-183">Give a service name for your app in Kubernetes and the full image name.</span></span> <span data-ttu-id="82375-184">Exempel:</span><span class="sxs-lookup"><span data-stu-id="82375-184">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="82375-185">I det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="82375-185">In this command:</span></span>

   * <span data-ttu-id="82375-186">Behållarens namn `gs-spring-boot-docker` anges omedelbart efter den `run` kommando</span><span class="sxs-lookup"><span data-stu-id="82375-186">The container name `gs-spring-boot-docker` is specified immediately after the `run` command</span></span>

   * <span data-ttu-id="82375-187">Den `--image` parametern anger kombinerade server och image inloggningsnamnet som`wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span><span class="sxs-lookup"><span data-stu-id="82375-187">The `--image` parameter specifies the combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="82375-188">Exponera Kubernetes klustret externt med hjälp av den `kubectl expose` kommando.</span><span class="sxs-lookup"><span data-stu-id="82375-188">Expose your Kubernetes cluster externally by using the `kubectl expose` command.</span></span> <span data-ttu-id="82375-189">Ange tjänstens namn och offentliga TCP-port som används för att komma åt appen interna målporten appen lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="82375-189">Specify your service name, the public-facing TCP port used to access the app, and the internal target port your app listens on.</span></span> <span data-ttu-id="82375-190">Exempel:</span><span class="sxs-lookup"><span data-stu-id="82375-190">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="82375-191">I det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="82375-191">In this command:</span></span>

   * <span data-ttu-id="82375-192">Behållarens namn `gs-spring-boot-docker` anges omedelbart efter den `expose deployment` kommando</span><span class="sxs-lookup"><span data-stu-id="82375-192">The container name `gs-spring-boot-docker` is specified immediately after the `expose deployment` command</span></span>

   * <span data-ttu-id="82375-193">Den `--type` parametern anger att klustret använder belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="82375-193">The `--type` parameter specifies that the cluster uses load balancer</span></span>

   * <span data-ttu-id="82375-194">Den `--port` parametern anger offentliga TCP-port 80.</span><span class="sxs-lookup"><span data-stu-id="82375-194">The `--port` parameter specifies the public-facing TCP port of 80.</span></span> <span data-ttu-id="82375-195">Du har åtkomst till appen på den här porten.</span><span class="sxs-lookup"><span data-stu-id="82375-195">You access the app on this port.</span></span>

   * <span data-ttu-id="82375-196">Den `--target-port` parametern anger internt TCP port 8080.</span><span class="sxs-lookup"><span data-stu-id="82375-196">The `--target-port` parameter specifies the internal TCP port of 8080.</span></span> <span data-ttu-id="82375-197">Belastningsutjämnaren vidarebefordrar begäranden till din app på denna port.</span><span class="sxs-lookup"><span data-stu-id="82375-197">The load balancer forwards requests to your app on this port.</span></span>

1. <span data-ttu-id="82375-198">När appen har distribuerats till klustret, fråga extern IP-adress och öppna den i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="82375-198">Once the app is deployed to the cluster, query the external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Bläddra Sample-appen på Azure][SB02]


## <a name="next-steps"></a><span data-ttu-id="82375-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="82375-200">Next steps</span></span>

<span data-ttu-id="82375-201">Mer information om hur du använder värdet Start på Azure finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="82375-201">For more information about using Spring Boot on Azure, see the following articles:</span></span>

* [<span data-ttu-id="82375-202">Distribuera ett program för start av källan till Azure App Service</span><span class="sxs-lookup"><span data-stu-id="82375-202">Deploy a Spring Boot Application to the Azure App Service</span></span>](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [<span data-ttu-id="82375-203">Distribuera en källan startprogrammet på Linux i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="82375-203">Deploy a Spring Boot application on Linux in the Azure Container Service</span></span>](container-service-deploy-spring-boot-app-on-linux.md)

<span data-ttu-id="82375-204">Mer information om hur du använder Azure med Java finns på [Azure Java Developer Center] och i [Java Tools för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="82375-204">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="82375-205">Mer information om källan Start på Docker exempelprojektet finns [Vårversionen Start på Docker komma igång].</span><span class="sxs-lookup"><span data-stu-id="82375-205">For more information about the Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="82375-206">Följande länkar innehåller ytterligare information om hur du skapar källan startprogram:</span><span class="sxs-lookup"><span data-stu-id="82375-206">The following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="82375-207">Mer information om hur du skapar ett enkelt källan Start-program finns i vår Initializr på https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="82375-207">For more information about creating a simple Spring Boot application, see the Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="82375-208">Följande länkar innehåller ytterligare information om hur du använder Kubernetes med Azure:</span><span class="sxs-lookup"><span data-stu-id="82375-208">The following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="82375-209">Kom igång med ett Kubernetes kluster i Container Service</span><span class="sxs-lookup"><span data-stu-id="82375-209">Get started with a Kubernetes cluster in Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [<span data-ttu-id="82375-210">Med Azure Container Service Kubernetes webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="82375-210">Using the Kubernetes web UI with Azure Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

<span data-ttu-id="82375-211">Mer information om hur du använder Kubernetes kommandoradsgränssnittet finns i den **kubectl** användarhandboken på <https://kubernetes.io/docs/user-guide/kubectl/>.</span><span class="sxs-lookup"><span data-stu-id="82375-211">More information about using Kubernetes command-line interface is available in the **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="82375-212">Webbplatsen Kubernetes har flera artiklar som handlar om med bilder i privata register:</span><span class="sxs-lookup"><span data-stu-id="82375-212">The Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="82375-213">[Konfigurera Service-konton för skida]</span><span class="sxs-lookup"><span data-stu-id="82375-213">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="82375-214">[Namnområden]</span><span class="sxs-lookup"><span data-stu-id="82375-214">[Namespaces]</span></span>
* <span data-ttu-id="82375-215">[Dra en bild från ett privat register]</span><span class="sxs-lookup"><span data-stu-id="82375-215">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="82375-216">Ytterligare exempel för hur du använder anpassade Docker-avbildningar med Azure finns [med hjälp av en anpassad Docker-avbildning för Azure Web App på Linux].</span><span class="sxs-lookup"><span data-stu-id="82375-216">For additional examples for how to use custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

<span data-ttu-id="82375-217">[Azure-kommandoradsgränssnittet (CLI)]: /cli/azure/overview</span><span class="sxs-lookup"><span data-stu-id="82375-217">[Azure Command-Line Interface (CLI)]: /cli/azure/overview</span></span>
<span data-ttu-id="82375-218">[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/</span><span class="sxs-lookup"><span data-stu-id="82375-218">[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/</span></span>
<span data-ttu-id="82375-219">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="82375-219">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
<span data-ttu-id="82375-220">[med hjälp av en anpassad Docker-avbildning för Azure Web App på Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image</span><span class="sxs-lookup"><span data-stu-id="82375-220">[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image</span></span>
<span data-ttu-id="82375-221">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="82375-221">[Docker]: https://www.docker.com/</span></span>
<span data-ttu-id="82375-222">[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="82375-222">[free Azure account]: https://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="82375-223">[Git]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="82375-223">[Git]: https://github.com/</span></span>
<span data-ttu-id="82375-224">[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span><span class="sxs-lookup"><span data-stu-id="82375-224">[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/</span></span>
<span data-ttu-id="82375-225">[Java Tools för Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="82375-225">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>
<span data-ttu-id="82375-226">[Kubernetes]: https://kubernetes.io/</span><span class="sxs-lookup"><span data-stu-id="82375-226">[Kubernetes]: https://kubernetes.io/</span></span>
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
<span data-ttu-id="82375-227">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="82375-227">[Maven]: http://maven.apache.org/</span></span>
<span data-ttu-id="82375-228">[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span><span class="sxs-lookup"><span data-stu-id="82375-228">[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/</span></span>
<span data-ttu-id="82375-229">[Vårversionen Start]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="82375-229">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="82375-230">[Vårversionen Start på Docker komma igång]: https://github.com/spring-guides/gs-spring-boot-docker</span><span class="sxs-lookup"><span data-stu-id="82375-230">[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker</span></span>
<span data-ttu-id="82375-231">[Vårversionen Framework]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="82375-231">[Spring Framework]: https://spring.io/</span></span>
<span data-ttu-id="82375-232">[Konfigurera Service-konton för skida]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/</span><span class="sxs-lookup"><span data-stu-id="82375-232">[Configuring Service Accounts for Pods]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/</span></span>
<span data-ttu-id="82375-233">[Namnområden]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/</span><span class="sxs-lookup"><span data-stu-id="82375-233">[Namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/</span></span>
<span data-ttu-id="82375-234">[Dra en bild från ett privat register]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/</span><span class="sxs-lookup"><span data-stu-id="82375-234">[Pulling an Image from a Private Registry]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/</span></span>

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR04.png

[KB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB01.png
[KB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB02.png
[KB03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB03.png
[KB04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB04.png
[KB05]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB05.png
[KB06]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB06.png
[KB07]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB07.png
