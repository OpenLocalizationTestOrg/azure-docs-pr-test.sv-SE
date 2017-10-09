---
title: "aaaDeploy en App för start av värdet på Kubernetes i Azure Container Service | Microsoft Docs"
description: "Den här kursen får du om hello steg toodeploy en källan startprogrammet i ett kluster med Kubernetes i Microsoft Azure."
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
ms.openlocfilehash: 2bf9df459f874a1f478f43cdd29992d86c370837
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-hello-azure-container-service"></a><span data-ttu-id="c61de-103">Distribuera ett program för start av värdet på ett kluster med Kubernetes på hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="c61de-103">Deploy a Spring Boot Application on a Kubernetes Cluster in hello Azure Container Service</span></span>

<span data-ttu-id="c61de-104">Hej  **[Vårversionen Framework]**  ett populära ramverk för öppen källkod som gör att Java-utvecklare skapa webb-, mobil- och API-program.</span><span class="sxs-lookup"><span data-stu-id="c61de-104">hello **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="c61de-105">Den här kursen använder en exempelapp som skapats med hjälp av [Vårversionen Start], en konvention inriktat tillvägagångssätt för att använda vår tooget igång snabbt.</span><span class="sxs-lookup"><span data-stu-id="c61de-105">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring tooget started quickly.</span></span>

<span data-ttu-id="c61de-106">**[Kubernetes]**  och  **[Docker]**  är öppen källkod som hjälper utvecklare att automatisera hello distribution, skalning och hanteringen av sina program som körs i behållare.</span><span class="sxs-lookup"><span data-stu-id="c61de-106">**[Kubernetes]** and **[Docker]** are open-source solutions that help developers automate hello deployment, scaling, and management of their applications  running in containers.</span></span>

<span data-ttu-id="c61de-107">Den här självstudiekursen vägleder dig om att kombinera dessa två populära, öppen källkod tekniker toodevelop och distribuera en källan Start programmet tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c61de-107">This tutorial walks you though combining these two popular, open-source technologies toodevelop and deploy a Spring Boot application tooMicrosoft Azure.</span></span> <span data-ttu-id="c61de-108">Mer specifikt du använder  *[Vårversionen Start]*  för programutveckling,  *[Kubernetes]*  för behållaren distribution och hello [Azure Container Service (ACS)] toohost ditt program.</span><span class="sxs-lookup"><span data-stu-id="c61de-108">More specifically, you use *[Spring Boot]* for application development, *[Kubernetes]* for container deployment, and hello [Azure Container Service (ACS)] toohost your application.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="c61de-109">Krav</span><span class="sxs-lookup"><span data-stu-id="c61de-109">Prerequisites</span></span>

* <span data-ttu-id="c61de-110">En Azure-prenumeration; Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner] eller registrera dig för en [kostnadsfritt Azure-konto].</span><span class="sxs-lookup"><span data-stu-id="c61de-110">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="c61de-111">Hej [Azure-kommandoradsgränssnittet (CLI)].</span><span class="sxs-lookup"><span data-stu-id="c61de-111">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="c61de-112">En uppdaterad [Java Developer Kit (JDK)].</span><span class="sxs-lookup"><span data-stu-id="c61de-112">An up-to-date [Java Developer Kit (JDK)].</span></span>
* <span data-ttu-id="c61de-113">Apache's [Maven] skapa verktyget (Version 3).</span><span class="sxs-lookup"><span data-stu-id="c61de-113">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="c61de-114">En [Git] klienten.</span><span class="sxs-lookup"><span data-stu-id="c61de-114">A [Git] client.</span></span>
* <span data-ttu-id="c61de-115">En [Docker] klienten.</span><span class="sxs-lookup"><span data-stu-id="c61de-115">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="c61de-116">På grund av toohello virtualisering kraven i den här kursen får inte kan du använda hello stegen i den här artikeln på en virtuell dator; Du måste använda en fysisk dator med aktiverat funktionerna för virtualisering.</span><span class="sxs-lookup"><span data-stu-id="c61de-116">Due toohello virtualization requirements of this tutorial, you cannot follow hello steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="create-hello-spring-boot-on-docker-getting-started-web-app"></a><span data-ttu-id="c61de-117">Skapa hello källan Start på Docker komma igång webbprogram</span><span class="sxs-lookup"><span data-stu-id="c61de-117">Create hello Spring Boot on Docker Getting Started web app</span></span>

<span data-ttu-id="c61de-118">hello följande steg beskriver hur du skapar ett webbprogram i vår start och testa dem lokalt.</span><span class="sxs-lookup"><span data-stu-id="c61de-118">hello following steps walk you through building a Spring Boot web application and testing it locally.</span></span>

1. <span data-ttu-id="c61de-119">Öppna en kommandotolk och skapa en lokal katalog toohold programmet och ändra toothat katalog. Exempel:</span><span class="sxs-lookup"><span data-stu-id="c61de-119">Open a command-prompt and create a local directory toohold your application, and change toothat directory; for example:</span></span>
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="c61de-120">-- eller--</span><span class="sxs-lookup"><span data-stu-id="c61de-120">-- or --</span></span>
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="c61de-121">Klona hello [Vårversionen Start på Docker komma igång] exempelprojektet i hello katalog.</span><span class="sxs-lookup"><span data-stu-id="c61de-121">Clone hello [Spring Boot on Docker Getting Started] sample project into hello directory.</span></span>
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. <span data-ttu-id="c61de-122">Ändra katalogen toohello slutförts projektet.</span><span class="sxs-lookup"><span data-stu-id="c61de-122">Change directory toohello completed project.</span></span>
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. <span data-ttu-id="c61de-123">Använd Maven toobuild och kör hello sample-appen.</span><span class="sxs-lookup"><span data-stu-id="c61de-123">Use Maven toobuild and run hello sample app.</span></span>
   ```
   mvn package spring-boot:run
   ```

1. <span data-ttu-id="c61de-124">Testa hello-webbprogram genom att bläddra toohttp://localhost:8080 eller med hello följande `curl` kommando:</span><span class="sxs-lookup"><span data-stu-id="c61de-124">Test hello web app by browsing toohttp://localhost:8080, or with hello following `curl` command:</span></span>
   ```
   curl http://localhost:8080
   ```

1. <span data-ttu-id="c61de-125">Du bör se hello följande meddelande visas: **Docker hälsningsmeddelande**</span><span class="sxs-lookup"><span data-stu-id="c61de-125">You should see hello following message displayed: **Hello Docker World**</span></span>

   ![Bläddra Sample-appen lokalt][SB01]

## <a name="create-an-azure-container-registry-using-hello-azure-cli"></a><span data-ttu-id="c61de-127">Skapa ett Azure Container registret med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c61de-127">Create an Azure Container Registry using hello Azure CLI</span></span>

1. <span data-ttu-id="c61de-128">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="c61de-128">Open a command prompt.</span></span>

1. <span data-ttu-id="c61de-129">Logga in tooyour Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="c61de-129">Log in tooyour Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="c61de-130">Skapa en resursgrupp för hello Azure-resurser används i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="c61de-130">Create a resource group for hello Azure resources used in this tutorial.</span></span>
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. <span data-ttu-id="c61de-131">Skapa ett register för privat Azure-behållaren i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c61de-131">Create a private Azure container registry in hello resource group.</span></span> <span data-ttu-id="c61de-132">hello självstudiekursen skickar hello sample-appen som Docker bild toothis register i senare steg.</span><span class="sxs-lookup"><span data-stu-id="c61de-132">hello tutorial pushes hello sample app as a Docker image toothis registry in later steps.</span></span> <span data-ttu-id="c61de-133">Ersätt `wingtiptoysregistry` med ett unikt namn för registret.</span><span class="sxs-lookup"><span data-stu-id="c61de-133">Replace `wingtiptoysregistry` with a unique name for your registry.</span></span>
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-toohello-container-registry"></a><span data-ttu-id="c61de-134">Push-app toohello behållare registret</span><span class="sxs-lookup"><span data-stu-id="c61de-134">Push your app toohello container registry</span></span>

1. <span data-ttu-id="c61de-135">Navigera toohello konfigurationskatalogen för Maven-installationen (standard ~/.m2/ eller C:\Users\username\.m2) och öppna hello *settings.xml* fil med en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="c61de-135">Navigate toohello configuration directory for your Maven installation (default ~/.m2/ or C:\Users\username\.m2) and open hello *settings.xml* file with a text editor.</span></span>

1. <span data-ttu-id="c61de-136">Hämta hello lösenordet för behållaren registret från hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="c61de-136">Retrieve hello password for your container registry from hello Azure CLI.</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. <span data-ttu-id="c61de-137">Lägg till din Azure-behållare register-id och lösenord tooa nya `<server>` samling i hello *settings.xml* fil.</span><span class="sxs-lookup"><span data-stu-id="c61de-137">Add your Azure Container Registry id and password tooa new `<server>` collection in hello *settings.xml* file.</span></span>
<span data-ttu-id="c61de-138">Hej `id` och `username` är hello namnet på hello registret.</span><span class="sxs-lookup"><span data-stu-id="c61de-138">hello `id` and `username` are hello name of hello registry.</span></span> <span data-ttu-id="c61de-139">Använd hello `password` värde från hello föregående kommando (utan citattecken).</span><span class="sxs-lookup"><span data-stu-id="c61de-139">Use hello `password` value from hello previous command (without quotes).</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. <span data-ttu-id="c61de-140">Navigera toohello slutförts projektkatalogen för tillämpningsprogrammet källan start (till exempel ”*C:\SpringBoot\gs-spring-boot-docker\complete*” eller ”*/users/robert/SpringBoot/gs-spring-boot-docker / fullständig*”), och öppna hello *pom.xml* fil med en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="c61de-140">Navigate toohello completed project directory for your Spring Boot application (for example, "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open hello *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="c61de-141">Uppdatera hello `<properties>` samling i hello *pom.xml* fil med hello inloggningen server värde för Azure-behållare registret.</span><span class="sxs-lookup"><span data-stu-id="c61de-141">Update hello `<properties>` collection in hello *pom.xml* file with hello login server value for your Azure Container Registry.</span></span>

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. <span data-ttu-id="c61de-142">Uppdatera hello `<plugins>` samling i hello *pom.xml* filen så som hello `<plugin>` innehåller hello server adress och registret inloggningsnamn för ditt Azure-behållare registret.</span><span class="sxs-lookup"><span data-stu-id="c61de-142">Update hello `<plugins>` collection in hello *pom.xml* file so that hello `<plugin>` contains hello login server address and registry name for your Azure Container Registry.</span></span>

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

1. <span data-ttu-id="c61de-143">Navigera toohello slutförts projektkatalogen för tillämpningsprogrammet källan start och kör hello efter kommandot toobuild hello Docker-behållaren och push hello avbildningen toohello registret:</span><span class="sxs-lookup"><span data-stu-id="c61de-143">Navigate toohello completed project directory for your Spring Boot application and run hello following command toobuild hello Docker container and push hello image toohello registry:</span></span>

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  <span data-ttu-id="c61de-144">Du får ett felmeddelande liknande tooone av hello följande när Maven skickar hello avbildningen tooAzure:</span><span class="sxs-lookup"><span data-stu-id="c61de-144">You may receive an error message that is similar tooone of hello following when Maven pushes hello image tooAzure:</span></span>
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed tooexecute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> <span data-ttu-id="c61de-145">Om du får det här felet kan logga in tooAzure från hello Docker-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="c61de-145">If you get this error, log in tooAzure from hello Docker command line.</span></span>
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> <span data-ttu-id="c61de-146">Sedan push din behållare:</span><span class="sxs-lookup"><span data-stu-id="c61de-146">Then push your container:</span></span>
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-hello-azure-cli"></a><span data-ttu-id="c61de-147">Skapa ett Kubernetes kluster på ACS med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c61de-147">Create a Kubernetes Cluster on ACS using hello Azure CLI</span></span>

1. <span data-ttu-id="c61de-148">Skapa ett Kubernetes-kluster i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="c61de-148">Create a Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="c61de-149">hello följande kommando skapar en *kubernetes* klustret i hello *VingspetsLeksaker kubernetes* resursen med *VingspetsLeksaker containerservice* som hello kluster namn och *VingspetsLeksaker kubernetes* som hello DNS-prefix:</span><span class="sxs-lookup"><span data-stu-id="c61de-149">hello following command creates a *kubernetes* cluster in hello *wingtiptoys-kubernetes* resource group, with *wingtiptoys-containerservice* as hello cluster name, and *wingtiptoys-kubernetes* as hello DNS prefix:</span></span>
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   <span data-ttu-id="c61de-150">Det här kommandot kan ta en stund toocomplete.</span><span class="sxs-lookup"><span data-stu-id="c61de-150">This command may take a while toocomplete.</span></span>

1. <span data-ttu-id="c61de-151">Installera `kubectl` med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="c61de-151">Install `kubectl` using hello Azure CLI.</span></span> <span data-ttu-id="c61de-152">Linux-användare kan ha tooprefix det här kommandot med `sudo` eftersom den distribuerar hello Kubernetes CLI för`/usr/local/bin`.</span><span class="sxs-lookup"><span data-stu-id="c61de-152">Linux users may have tooprefix this command with `sudo` since it deploys hello Kubernetes CLI too`/usr/local/bin`.</span></span>
   ```azurecli
   az acs kubernetes install-cli
   ```

1. <span data-ttu-id="c61de-153">Hämta konfigurationsinformation för hello klustret så att du kan hantera klustret från hello Kubernetes webbgränssnitt och `kubectl`.</span><span class="sxs-lookup"><span data-stu-id="c61de-153">Download hello cluster configuration information so you can manage your cluster from hello Kubernetes web interface and `kubectl`.</span></span> 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-hello-image-tooyour-kubernetes-cluster"></a><span data-ttu-id="c61de-154">Distribuera hello avbildningen tooyour Kubernetes kluster</span><span class="sxs-lookup"><span data-stu-id="c61de-154">Deploy hello image tooyour Kubernetes cluster</span></span>

<span data-ttu-id="c61de-155">Den här kursen distribuerar hello app med hjälp av `kubectl`, sedan kan du tooexplore hello distribution via hello Kubernetes webbgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="c61de-155">This tutorial deploys hello app using `kubectl`, then allow you tooexplore hello deployment through hello Kubernetes web interface.</span></span>

### <a name="deploy-with-hello-kubernetes-web-interface"></a><span data-ttu-id="c61de-156">Distribuera med hello Kubernetes webbgränssnitt</span><span class="sxs-lookup"><span data-stu-id="c61de-156">Deploy with hello Kubernetes web interface</span></span>

1. <span data-ttu-id="c61de-157">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="c61de-157">Open a command prompt.</span></span>

1. <span data-ttu-id="c61de-158">Öppna hello configuration webbplats för klustret Kubernetes i standardwebbläsaren:</span><span class="sxs-lookup"><span data-stu-id="c61de-158">Open hello configuration website for your Kubernetes cluster in your default browser:</span></span>
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. <span data-ttu-id="c61de-159">När hello Kubernetes configuration webbplats öppnas i webbläsaren, klickar du på länken hello för**distribuera en behållare app**:</span><span class="sxs-lookup"><span data-stu-id="c61de-159">When hello Kubernetes configuration website opens in your browser, click hello link too**deploy a containerized app**:</span></span>

   ![Kubernetes Configuration webbplats][KB01]

1. <span data-ttu-id="c61de-161">När hello **distribuera en behållare app** visas, ange hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="c61de-161">When hello **Deploy a containerized app** page is displayed, specify hello following options:</span></span>

   <span data-ttu-id="c61de-162">a.</span><span class="sxs-lookup"><span data-stu-id="c61de-162">a.</span></span> <span data-ttu-id="c61de-163">Välj **ange information om appar nedan**.</span><span class="sxs-lookup"><span data-stu-id="c61de-163">Select **Specify app details below**.</span></span>

   <span data-ttu-id="c61de-164">b.</span><span class="sxs-lookup"><span data-stu-id="c61de-164">b.</span></span> <span data-ttu-id="c61de-165">Ange programnamnet källan Start för hello **appnamn**, till exempel ”:*gs-källan-Start-docker*”.</span><span class="sxs-lookup"><span data-stu-id="c61de-165">Enter your Spring Boot application name for hello **App name**; for example: "*gs-spring-boot-docker*".</span></span>

   <span data-ttu-id="c61de-166">c.</span><span class="sxs-lookup"><span data-stu-id="c61de-166">c.</span></span> <span data-ttu-id="c61de-167">Ange inloggningen server och en behållare avbildningen från tidigare för hello **behållaren image**, till exempel ”:*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*”.</span><span class="sxs-lookup"><span data-stu-id="c61de-167">Enter your login server and container image from earlier for hello **Container image**; for example: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*".</span></span>

   <span data-ttu-id="c61de-168">d.</span><span class="sxs-lookup"><span data-stu-id="c61de-168">d.</span></span> <span data-ttu-id="c61de-169">Välj **externa** för hello **tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="c61de-169">Choose **External** for hello **Service**.</span></span>

   <span data-ttu-id="c61de-170">e.</span><span class="sxs-lookup"><span data-stu-id="c61de-170">e.</span></span> <span data-ttu-id="c61de-171">Ange din externa och interna portar i hello **Port** och **mål port** textrutor.</span><span class="sxs-lookup"><span data-stu-id="c61de-171">Specify your external and internal ports in hello **Port** and **Target port** text boxes.</span></span>

   ![Kubernetes Configuration webbplats][KB02]


1. <span data-ttu-id="c61de-173">Klicka på **distribuera** toodeploy hello behållare.</span><span class="sxs-lookup"><span data-stu-id="c61de-173">Click **Deploy** toodeploy hello container.</span></span>

   ![Distribuera behållare][KB05]

1. <span data-ttu-id="c61de-175">När programmet har distribuerats visas källan Start programmet visas under **Services**.</span><span class="sxs-lookup"><span data-stu-id="c61de-175">Once your application has been deployed, you will see your Spring Boot application listed under **Services**.</span></span>

   ![Kubernetes tjänster][KB06]

1. <span data-ttu-id="c61de-177">Om du klickar på länken hello **externa slutpunkter**, du kan se din källan Start-program som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="c61de-177">If you click hello link for **External endpoints**, you can see your Spring Boot application running on Azure.</span></span>

   ![Kubernetes tjänster][KB07]

   ![Bläddra Sample-appen på Azure][SB02]


### <a name="deploy-with-kubectl"></a><span data-ttu-id="c61de-180">Distribuera med kubectl</span><span class="sxs-lookup"><span data-stu-id="c61de-180">Deploy with kubectl</span></span>

1. <span data-ttu-id="c61de-181">Öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="c61de-181">Open a command prompt.</span></span>

1. <span data-ttu-id="c61de-182">Kör ditt behållare i hello Kubernetes kluster genom att använda hello `kubectl run` kommando.</span><span class="sxs-lookup"><span data-stu-id="c61de-182">Run your container in hello Kubernetes cluster by using hello `kubectl run` command.</span></span> <span data-ttu-id="c61de-183">Ge en tjänst för din app i Kubernetes och hello fullständiga namn.</span><span class="sxs-lookup"><span data-stu-id="c61de-183">Give a service name for your app in Kubernetes and hello full image name.</span></span> <span data-ttu-id="c61de-184">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c61de-184">For example:</span></span>
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   <span data-ttu-id="c61de-185">I det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="c61de-185">In this command:</span></span>

   * <span data-ttu-id="c61de-186">hello behållarnamn `gs-spring-boot-docker` anges omedelbart efter hello `run` kommando</span><span class="sxs-lookup"><span data-stu-id="c61de-186">hello container name `gs-spring-boot-docker` is specified immediately after hello `run` command</span></span>

   * <span data-ttu-id="c61de-187">Hej `--image` parametern anger hello kombineras inloggningsserver och Avbildningsnamnet som`wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span><span class="sxs-lookup"><span data-stu-id="c61de-187">hello `--image` parameter specifies hello combined login server and image name as `wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`</span></span>

1. <span data-ttu-id="c61de-188">Exponera Kubernetes klustret externt med hjälp av hello `kubectl expose` kommando.</span><span class="sxs-lookup"><span data-stu-id="c61de-188">Expose your Kubernetes cluster externally by using hello `kubectl expose` command.</span></span> <span data-ttu-id="c61de-189">Ange tjänstnamnet, hello offentliga TCP-port som används tooaccess hello appen och hello interna målporten appen lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="c61de-189">Specify your service name, hello public-facing TCP port used tooaccess hello app, and hello internal target port your app listens on.</span></span> <span data-ttu-id="c61de-190">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c61de-190">For example:</span></span>
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   <span data-ttu-id="c61de-191">I det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="c61de-191">In this command:</span></span>

   * <span data-ttu-id="c61de-192">hello behållarnamn `gs-spring-boot-docker` anges omedelbart efter hello `expose deployment` kommando</span><span class="sxs-lookup"><span data-stu-id="c61de-192">hello container name `gs-spring-boot-docker` is specified immediately after hello `expose deployment` command</span></span>

   * <span data-ttu-id="c61de-193">Hej `--type` parametern anger hello klustret använder belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="c61de-193">hello `--type` parameter specifies that hello cluster uses load balancer</span></span>

   * <span data-ttu-id="c61de-194">Hej `--port` parametern anger hello offentliga TCP-port 80.</span><span class="sxs-lookup"><span data-stu-id="c61de-194">hello `--port` parameter specifies hello public-facing TCP port of 80.</span></span> <span data-ttu-id="c61de-195">Du har åtkomst till hello app på denna port.</span><span class="sxs-lookup"><span data-stu-id="c61de-195">You access hello app on this port.</span></span>

   * <span data-ttu-id="c61de-196">Hej `--target-port` parametern anger hello interna TCP-port för 8080.</span><span class="sxs-lookup"><span data-stu-id="c61de-196">hello `--target-port` parameter specifies hello internal TCP port of 8080.</span></span> <span data-ttu-id="c61de-197">hello belastningsutjämning vidarebefordrar begäranden tooyour app på denna port.</span><span class="sxs-lookup"><span data-stu-id="c61de-197">hello load balancer forwards requests tooyour app on this port.</span></span>

1. <span data-ttu-id="c61de-198">När hello appen har distribuerats toohello klustret fråga hello extern IP-adress och öppna den i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="c61de-198">Once hello app is deployed toohello cluster, query hello external IP address and open it in your web browser:</span></span>

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Bläddra Sample-appen på Azure][SB02]


## <a name="next-steps"></a><span data-ttu-id="c61de-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c61de-200">Next steps</span></span>

<span data-ttu-id="c61de-201">Mer information om hur du använder värdet Start på Azure finns i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="c61de-201">For more information about using Spring Boot on Azure, see hello following articles:</span></span>

* [<span data-ttu-id="c61de-202">Distribuera en källan startprogrammet toohello Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c61de-202">Deploy a Spring Boot Application toohello Azure App Service</span></span>](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [<span data-ttu-id="c61de-203">Distribuera ett program med vår Start på Linux i hello Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="c61de-203">Deploy a Spring Boot application on Linux in hello Azure Container Service</span></span>](container-service-deploy-spring-boot-app-on-linux.md)

<span data-ttu-id="c61de-204">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="c61de-204">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="c61de-205">Mer information om hello källan Start på Docker exempelprojektet finns [Vårversionen Start på Docker komma igång].</span><span class="sxs-lookup"><span data-stu-id="c61de-205">For more information about hello Spring Boot on Docker sample project, see [Spring Boot on Docker Getting Started].</span></span>

<span data-ttu-id="c61de-206">hello följande länkar innehåller ytterligare information om hur du skapar källan startprogram:</span><span class="sxs-lookup"><span data-stu-id="c61de-206">hello following links provide additional information about creating Spring Boot applications:</span></span>

* <span data-ttu-id="c61de-207">Mer information om hur du skapar ett enkelt källan Start-program finns i hello källan Initializr på https://start.spring.io/.</span><span class="sxs-lookup"><span data-stu-id="c61de-207">For more information about creating a simple Spring Boot application, see hello Spring Initializr at https://start.spring.io/.</span></span>

<span data-ttu-id="c61de-208">hello innehåller följande länkar ytterligare information om hur du använder Kubernetes med Azure:</span><span class="sxs-lookup"><span data-stu-id="c61de-208">hello following links provide additional information about using Kubernetes with Azure:</span></span>

* [<span data-ttu-id="c61de-209">Kom igång med ett Kubernetes kluster i Container Service</span><span class="sxs-lookup"><span data-stu-id="c61de-209">Get started with a Kubernetes cluster in Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [<span data-ttu-id="c61de-210">Med hjälp av hello webbgränssnitt Kubernetes med Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="c61de-210">Using hello Kubernetes web UI with Azure Container Service</span></span>](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

<span data-ttu-id="c61de-211">Mer information om hur du använder Kubernetes kommandoradsgränssnittet är tillgänglig i hello **kubectl** användarhandboken på <https://kubernetes.io/docs/user-guide/kubectl/>.</span><span class="sxs-lookup"><span data-stu-id="c61de-211">More information about using Kubernetes command-line interface is available in hello **kubectl** user guide at <https://kubernetes.io/docs/user-guide/kubectl/>.</span></span>

<span data-ttu-id="c61de-212">Hej Kubernetes webbplats har flera artiklar som handlar om med bilder i privata register:</span><span class="sxs-lookup"><span data-stu-id="c61de-212">hello Kubernetes website has several articles that discuss using images in private registries:</span></span>

* <span data-ttu-id="c61de-213">[Konfigurera Service-konton för skida]</span><span class="sxs-lookup"><span data-stu-id="c61de-213">[Configuring Service Accounts for Pods]</span></span>
* <span data-ttu-id="c61de-214">[Namnområden]</span><span class="sxs-lookup"><span data-stu-id="c61de-214">[Namespaces]</span></span>
* <span data-ttu-id="c61de-215">[Dra en bild från ett privat register]</span><span class="sxs-lookup"><span data-stu-id="c61de-215">[Pulling an Image from a Private Registry]</span></span>

<span data-ttu-id="c61de-216">Ytterligare exempel för hur toouse anpassade Docker bilder med Azure finns [med hjälp av en anpassad Docker-avbildning för Azure Web App på Linux].</span><span class="sxs-lookup"><span data-stu-id="c61de-216">For additional examples for how toouse custom Docker images with Azure, see [Using a custom Docker image for Azure Web App on Linux].</span></span>

<!-- URL List -->

[Azure-kommandoradsgränssnittet (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using hello Azure portal]: /azure/container-registry/container-registry-get-started-portal
[med hjälp av en anpassad Docker-avbildning för Azure Web App på Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[kostnadsfritt Azure-konto]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[MSDN-prenumerantförmåner]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Vårversionen Start]: http://projects.spring.io/spring-boot/
[Vårversionen Start på Docker komma igång]: https://github.com/spring-guides/gs-spring-boot-docker
[Vårversionen Framework]: https://spring.io/
[Konfigurera Service-konton för skida]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[Namnområden]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[Dra en bild från ett privat register]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

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
