---
title: "Skapa och integrera kontinuerligt för ditt Azure Service Fabric Java-program för Linux med hjälp av Jenkins | Microsoft Docs"
description: "Skapa och integrera kontinuerligt för ditt Java-program för Linux med hjälp av Jenkins"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: saysa
ms.openlocfilehash: d9372407540d903acca5b1639a2d9ceb0bf3c571
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-jenkins-to-build-and-deploy-your-linux-java-application"></a><span data-ttu-id="97c5d-103">Skapa och distribuera ditt Java-program för Linux med hjälp av Jenkins</span><span class="sxs-lookup"><span data-stu-id="97c5d-103">Use Jenkins to build and deploy your Linux Java application</span></span>
<span data-ttu-id="97c5d-104">Jenkins är ett populärt verktyg för kontinuerlig integrering och distribution av appar.</span><span class="sxs-lookup"><span data-stu-id="97c5d-104">Jenkins is a popular tool for continuous integration and deployment of your apps.</span></span> <span data-ttu-id="97c5d-105">Så här skapar och distribuerar du ett Azure Service Fabric-program med Jenkins.</span><span class="sxs-lookup"><span data-stu-id="97c5d-105">Here's how to build and deploy your Azure Service Fabric application by using Jenkins.</span></span>

## <a name="general-prerequisites"></a><span data-ttu-id="97c5d-106">Allmänna krav</span><span class="sxs-lookup"><span data-stu-id="97c5d-106">General prerequisites</span></span>
- <span data-ttu-id="97c5d-107">Du måste ha Git installerat lokalt.</span><span class="sxs-lookup"><span data-stu-id="97c5d-107">Have Git installed locally.</span></span> <span data-ttu-id="97c5d-108">På [nedladdningssidan för Git](https://git-scm.com/downloads) kan du installera lämplig Git-version för ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="97c5d-108">You can install the appropriate Git version from [the Git downloads page](https://git-scm.com/downloads), based on your operating system.</span></span> <span data-ttu-id="97c5d-109">Om du är nybörjare på Git kan du läsa mer i [Git-dokumentationen](https://git-scm.com/docs).</span><span class="sxs-lookup"><span data-stu-id="97c5d-109">If you are new to Git, learn more about it from the [Git documentation](https://git-scm.com/docs).</span></span>
- <span data-ttu-id="97c5d-110">Du måste ha plugin-programmet Service Fabric Jenkins till hands.</span><span class="sxs-lookup"><span data-stu-id="97c5d-110">Have the Service Fabric Jenkins plug-in handy.</span></span> <span data-ttu-id="97c5d-111">Du kan ladda ned det från [Service Fabric-nedladdningar](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi).</span><span class="sxs-lookup"><span data-stu-id="97c5d-111">You can download it from [Service Fabric downloads](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi).</span></span>

## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a><span data-ttu-id="97c5d-112">Konfigurera Jenkins i ett Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="97c5d-112">Set up Jenkins inside a Service Fabric cluster</span></span>

<span data-ttu-id="97c5d-113">Du kan konfigurera Jenkins i eller utanför ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="97c5d-113">You can set up Jenkins either inside or outside a Service Fabric cluster.</span></span> <span data-ttu-id="97c5d-114">Följande avsnitt visar hur du konfigurerar det i ett kluster när ett Azure storage-konto för att spara tillståndet för behållaren-instans.</span><span class="sxs-lookup"><span data-stu-id="97c5d-114">The following sections show how to set it up inside a cluster while using an Azure storage account to save the state of the container instance.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="97c5d-115">Krav</span><span class="sxs-lookup"><span data-stu-id="97c5d-115">Prerequisites</span></span>
1. <span data-ttu-id="97c5d-116">Ha ett Service Fabric Linux-kluster redo.</span><span class="sxs-lookup"><span data-stu-id="97c5d-116">Have a Service Fabric Linux cluster ready.</span></span> <span data-ttu-id="97c5d-117">Docker finns redan installerat i Service Fabric-kluster som skapas via Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="97c5d-117">A Service Fabric cluster created from the Azure portal already has Docker installed.</span></span> <span data-ttu-id="97c5d-118">Om du kör klustret lokalt kan du kontrollera om Docker är installerat med hjälp av kommandot ``docker info``.</span><span class="sxs-lookup"><span data-stu-id="97c5d-118">If you are running the cluster locally, check if Docker is installed by using the command ``docker info``.</span></span> <span data-ttu-id="97c5d-119">Om Docker inte är installerat kan du installera det med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="97c5d-119">If it is not installed, install it accordingly by using the following commands:</span></span>

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```
2. <span data-ttu-id="97c5d-120">Distribuera Service Fabric-behållarprogrammet till klustret enligt stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="97c5d-120">Have the Service Fabric container application deployed on the cluster, by using the following steps:</span></span>

  ```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
cd service-fabric-java-getting-started/Services/JenkinsDocker/
```

3. <span data-ttu-id="97c5d-121">Du måste alternativet Anslut information om Azure storage-filresursen där du vill spara tillståndet för Jenkins behållare instans.</span><span class="sxs-lookup"><span data-stu-id="97c5d-121">You need the connect option details of the Azure storage file-share, where you want to persist the state of the Jenkins container instance.</span></span> <span data-ttu-id="97c5d-122">Om du använder Microsoft Azure-portalen för samma du följer du stegen – skapa ett Azure storage-konto säg ``sfjenkinsstorage1``.</span><span class="sxs-lookup"><span data-stu-id="97c5d-122">If you are using the Microsoft Azure portal for the same, please follow the steps - Create an Azure storage account, say ``sfjenkinsstorage1``.</span></span> <span data-ttu-id="97c5d-123">Skapa en **filresurs** under detta lagringskonto säger ``sfjenkins``.</span><span class="sxs-lookup"><span data-stu-id="97c5d-123">Create a **File Share** under that storage account, say ``sfjenkins``.</span></span> <span data-ttu-id="97c5d-124">Klicka på **Anslut** för filresurser och Observera värdena visas **ansluter från Linux**, säg detta skulle se ut så här -</span><span class="sxs-lookup"><span data-stu-id="97c5d-124">Click on **Connect** for the file-share and note the values it displays under **Connecting from Linux**, say this would look like as follows -</span></span>
```sh
sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
```

4. <span data-ttu-id="97c5d-125">Uppdatera platshållarvärdena i den ```setupentrypoint.sh``` skriptet med detaljer om motsvarande azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="97c5d-125">Update the placeholder values in the ```setupentrypoint.sh``` script with corresponding azure-storage details.</span></span>
```sh
vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
```
<span data-ttu-id="97c5d-126">Ersätt ``[REMOTE_FILE_SHARE_LOCATION]`` med värdet ``//sfjenkinsstorage1.file.core.windows.net/sfjenkins`` från utdata från connect i punkt 3 ovan.</span><span class="sxs-lookup"><span data-stu-id="97c5d-126">Replace ``[REMOTE_FILE_SHARE_LOCATION]`` with the value ``//sfjenkinsstorage1.file.core.windows.net/sfjenkins`` from the output of the connect in point 3 above.</span></span>
<span data-ttu-id="97c5d-127">Ersätt ``[FILE_SHARE_CONNECT_OPTIONS_STRING]`` med värdet ``vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777`` från punkt 3 ovan.</span><span class="sxs-lookup"><span data-stu-id="97c5d-127">Replace ``[FILE_SHARE_CONNECT_OPTIONS_STRING]`` with the value ``vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777`` from point 3 above.</span></span>

5. <span data-ttu-id="97c5d-128">Anslut till klustret och installera behållarprogrammet.</span><span class="sxs-lookup"><span data-stu-id="97c5d-128">Connect to the cluster and install the container application.</span></span>
```azurecli
sfctl cluster select --endpoint http://PublicIPorFQDN:19080   # cluster connect command
bash Scripts/install.sh
```
<span data-ttu-id="97c5d-129">Detta installerar en Jenkins-behållare på klustret och kan övervakas med Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="97c5d-129">This installs a Jenkins container on the cluster, and can be monitored by using the Service Fabric Explorer.</span></span>

### <a name="steps"></a><span data-ttu-id="97c5d-130">Steg</span><span class="sxs-lookup"><span data-stu-id="97c5d-130">Steps</span></span>
1. <span data-ttu-id="97c5d-131">Gå till ``http://PublicIPorFQDN:8081`` i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="97c5d-131">From your browser, go to ``http://PublicIPorFQDN:8081``.</span></span> <span data-ttu-id="97c5d-132">På sidan visas sökvägen till det ursprungliga administratörslösenordet som krävs för att logga in.</span><span class="sxs-lookup"><span data-stu-id="97c5d-132">It provides the path of the initial admin password required to sign in.</span></span> <span data-ttu-id="97c5d-133">Du kan fortsätta att använda Jenkins som administratörsanvändare.</span><span class="sxs-lookup"><span data-stu-id="97c5d-133">You can continue to use Jenkins as an admin user.</span></span> <span data-ttu-id="97c5d-134">Eller så kan du skapa en ny användare och byta till den när du har loggat in med det ursprungliga administratörskontot.</span><span class="sxs-lookup"><span data-stu-id="97c5d-134">Or you can create and change the user, after you sign in with the initial admin account.</span></span>

   > [!NOTE]
   > <span data-ttu-id="97c5d-135">Se till att port 8081 anges som programmets slutpunktsport när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="97c5d-135">Ensure that the 8081 port is specified as the application endpoint port while you are creating the cluster.</span></span>
   >

2. <span data-ttu-id="97c5d-136">Hämta behållarens instans-ID med ``docker ps -a``.</span><span class="sxs-lookup"><span data-stu-id="97c5d-136">Get the container instance ID by using ``docker ps -a``.</span></span>
3. <span data-ttu-id="97c5d-137">Logga in med SSH-inloggning (Secure Shell ) till behållaren och klistra in sökvägen som visades i Jenkins-portalen.</span><span class="sxs-lookup"><span data-stu-id="97c5d-137">Secure Shell (SSH) sign in to the container, and paste the path you were shown on the Jenkins portal.</span></span> <span data-ttu-id="97c5d-138">Om exempelvis sökvägen `PATH_TO_INITIAL_ADMIN_PASSWORD` visas i portalen kan du köra följande:</span><span class="sxs-lookup"><span data-stu-id="97c5d-138">For example, if in the portal it shows the path `PATH_TO_INITIAL_ADMIN_PASSWORD`, run the following:</span></span>

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. <span data-ttu-id="97c5d-139">Konfigurera GitHub för Jenkins genom att utföra åtgärderna som nämns i [Generating a new SSH key and adding it to the SSH agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) (Generera en ny SSH-nyckel och lägga till den i SSH-agenten).</span><span class="sxs-lookup"><span data-stu-id="97c5d-139">Set up GitHub to work with Jenkins, by using the steps mentioned in [Generating a new SSH key and adding it to the SSH agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).</span></span>
    * <span data-ttu-id="97c5d-140">Använd instruktionerna från GitHub för att skapa SSH-nyckeln och lägg till SSH-nyckeln på det GitHub-konto som är (blir) värd för databasen.</span><span class="sxs-lookup"><span data-stu-id="97c5d-140">Use the instructions provided by GitHub to generate the SSH key, and to add the SSH key to the GitHub account that is hosting your repository.</span></span>
    * <span data-ttu-id="97c5d-141">Kör de kommandon som nämns i länken ovan i Jenkins Docker-gränssnittet (och inte på värden).</span><span class="sxs-lookup"><span data-stu-id="97c5d-141">Run the commands mentioned in the preceding link in the Jenkins Docker shell (and not on your host).</span></span>
    * <span data-ttu-id="97c5d-142">Om du vill logga in till Jenkins-gränssnittet från värden ska du använda följande kommando:</span><span class="sxs-lookup"><span data-stu-id="97c5d-142">To sign in to the Jenkins shell from your host, use the following command:</span></span>

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a><span data-ttu-id="97c5d-143">Konfigurera Jenkins utanför ett Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="97c5d-143">Set up Jenkins outside a Service Fabric cluster</span></span>

<span data-ttu-id="97c5d-144">Du kan konfigurera Jenkins i eller utanför ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="97c5d-144">You can set up Jenkins either inside or outside of a Service Fabric cluster.</span></span> <span data-ttu-id="97c5d-145">I följande avsnitt visas hur du konfigurerar Jenkins utanför ett kluster.</span><span class="sxs-lookup"><span data-stu-id="97c5d-145">The following sections show how to set it up outside a cluster.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="97c5d-146">Krav</span><span class="sxs-lookup"><span data-stu-id="97c5d-146">Prerequisites</span></span>
<span data-ttu-id="97c5d-147">Du måste ha Docker installerat.</span><span class="sxs-lookup"><span data-stu-id="97c5d-147">You need to have Docker installed.</span></span> <span data-ttu-id="97c5d-148">Följande kommandon kan användas för att installera Docker från terminalen:</span><span class="sxs-lookup"><span data-stu-id="97c5d-148">The following commands can be used to install Docker from the terminal:</span></span>

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

<span data-ttu-id="97c5d-149">När du kör ``docker info`` på terminalen visar utdata nu att Docker-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="97c5d-149">Now when you run ``docker info`` in the terminal, you should see in the output that the Docker service is running.</span></span>

### <a name="steps"></a><span data-ttu-id="97c5d-150">Steg</span><span class="sxs-lookup"><span data-stu-id="97c5d-150">Steps</span></span>
  1. <span data-ttu-id="97c5d-151">Kör behållaren Service Fabric Jenkins, avbildning: ``docker pull raunakpandya/jenkins:v1``</span><span class="sxs-lookup"><span data-stu-id="97c5d-151">Pull the Service Fabric Jenkins container image: ``docker pull raunakpandya/jenkins:v1``</span></span>
  2. <span data-ttu-id="97c5d-152">Kör behållaravbildningen: ``docker run -itd -p 8080:8080 raunakpandya/jenkins:v1``</span><span class="sxs-lookup"><span data-stu-id="97c5d-152">Run the container image: ``docker run -itd -p 8080:8080 raunakpandya/jenkins:v1``</span></span>
  3. <span data-ttu-id="97c5d-153">Hämta ID:t för behållaravbildningsinstansen.</span><span class="sxs-lookup"><span data-stu-id="97c5d-153">Get the ID of the container image instance.</span></span> <span data-ttu-id="97c5d-154">Du kan visa en lista med alla Docker-behållare med hjälp av kommandot ``docker ps –a``</span><span class="sxs-lookup"><span data-stu-id="97c5d-154">You can list all the Docker containers with the command ``docker ps –a``</span></span>
  4. <span data-ttu-id="97c5d-155">Logga in på Jenkins-portalen med följande steg:</span><span class="sxs-lookup"><span data-stu-id="97c5d-155">Sign in to the Jenkins portal by using the following steps:</span></span>

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    <span data-ttu-id="97c5d-156">Om behållar-ID:t är 2d24a73b5964 ska du använda 2d24.</span><span class="sxs-lookup"><span data-stu-id="97c5d-156">If container ID is 2d24a73b5964, use 2d24.</span></span>
    * <span data-ttu-id="97c5d-157">Det här lösenordet krävs för att logga in på Jenkins-instrumentpanelen från portalen som är ``http://<HOST-IP>:8080``</span><span class="sxs-lookup"><span data-stu-id="97c5d-157">This password is required for signing in to the Jenkins dashboard from portal, which is ``http://<HOST-IP>:8080``</span></span>
    * <span data-ttu-id="97c5d-158">När du har loggat in för första gången kan du skapa ett eget användarkonto och använda det för senare behov, eller så kan du fortsätta att använda administratörskontot.</span><span class="sxs-lookup"><span data-stu-id="97c5d-158">After you sign in for the first time, you can create your own user account and use that for future purposes, or you can continue to use the administrator account.</span></span> <span data-ttu-id="97c5d-159">När du har skapat en användare måste du fortsätta med den användaren.</span><span class="sxs-lookup"><span data-stu-id="97c5d-159">After you create a user, you need to continue with that.</span></span>
  5. <span data-ttu-id="97c5d-160">Konfigurera GitHub för Jenkins genom att utföra åtgärderna som nämns i [Generating a new SSH key and adding it to the SSH agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) (Generera en ny SSH-nyckel och lägga till den i SSH-agenten).</span><span class="sxs-lookup"><span data-stu-id="97c5d-160">Set up GitHub to work with Jenkins, by using the steps mentioned in [Generating a new SSH key and adding it to the SSH agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).</span></span>
        * <span data-ttu-id="97c5d-161">Följ instruktionerna från GitHub för att skapa SSH-nyckeln och lägga till SSH-nyckeln på det GitHub-konto som är (blir) värd för databasen.</span><span class="sxs-lookup"><span data-stu-id="97c5d-161">Use the instructions provided by GitHub to generate the SSH key, and to add the SSH key to the GitHub account that is hosting the repository.</span></span>
        * <span data-ttu-id="97c5d-162">Kör de kommandon som nämns i länken ovan i Jenkins Docker-gränssnittet (och inte på värden).</span><span class="sxs-lookup"><span data-stu-id="97c5d-162">Run the commands mentioned in the preceding link in the Jenkins Docker shell (and not on your host).</span></span>
      * <span data-ttu-id="97c5d-163">Om du vill logga in till Jenkins-gränssnittet från värden ska du använda följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="97c5d-163">To sign in to the Jenkins shell from your host, use the following commands:</span></span>

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

<span data-ttu-id="97c5d-164">Kontrollera att klustret eller datorn där Jenkins-behållaravbildningen finns har en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="97c5d-164">Ensure that the cluster or machine where the Jenkins container image is hosted has a public-facing IP.</span></span> <span data-ttu-id="97c5d-165">Detta gör att Jenkins-instansen kan ta emot meddelanden från GitHub.</span><span class="sxs-lookup"><span data-stu-id="97c5d-165">This enables the Jenkins instance to receive notifications from GitHub.</span></span>

## <a name="install-the-service-fabric-jenkins-plug-in-from-the-portal"></a><span data-ttu-id="97c5d-166">Installera plugin-programmet till Service Fabric Jenkins från portalen</span><span class="sxs-lookup"><span data-stu-id="97c5d-166">Install the Service Fabric Jenkins plug-in from the portal</span></span>

1. <span data-ttu-id="97c5d-167">Gå till ``http://PublicIPorFQDN:8081``</span><span class="sxs-lookup"><span data-stu-id="97c5d-167">Go to ``http://PublicIPorFQDN:8081``</span></span>
2. <span data-ttu-id="97c5d-168">Från Jenkins-instrumentpanelen väljer du **Manage Jenkins (Hantera Jenkins)** > **Manage Plugins (Hantera plugin-program)** > **Avancerat**.</span><span class="sxs-lookup"><span data-stu-id="97c5d-168">From the Jenkins dashboard, select **Manage Jenkins** > **Manage Plugins** > **Advanced**.</span></span>
<span data-ttu-id="97c5d-169">Här kan du ladda upp ett plugin-program.</span><span class="sxs-lookup"><span data-stu-id="97c5d-169">Here, you can upload a plug-in.</span></span> <span data-ttu-id="97c5d-170">Välj alternativet **Välj fil** och välj sedan den **serviceFabric.hpi**-fil som du har hämtat under Krav.</span><span class="sxs-lookup"><span data-stu-id="97c5d-170">Select **Choose file**, and then select the **serviceFabric.hpi** file, which you downloaded under prerequisites.</span></span> <span data-ttu-id="97c5d-171">När du väljer alternativet för att **ladda upp** installerar Jenkins automatiskt plugin-programmet åt dig.</span><span class="sxs-lookup"><span data-stu-id="97c5d-171">When you select **Upload**, Jenkins automatically installs the plug-in.</span></span> <span data-ttu-id="97c5d-172">Tillåt en omstart om det begärs.</span><span class="sxs-lookup"><span data-stu-id="97c5d-172">Allow a restart if requested.</span></span>

## <a name="create-and-configure-a-jenkins-job"></a><span data-ttu-id="97c5d-173">Skapa och konfigurera ett Jenkins-jobb</span><span class="sxs-lookup"><span data-stu-id="97c5d-173">Create and configure a Jenkins job</span></span>

1. <span data-ttu-id="97c5d-174">Skapa en **ny post** från instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="97c5d-174">Create a **new item** from dashboard.</span></span>
2. <span data-ttu-id="97c5d-175">Ange ett namn (till exempel **MyJob**).</span><span class="sxs-lookup"><span data-stu-id="97c5d-175">Enter an item name (for example, **MyJob**).</span></span> <span data-ttu-id="97c5d-176">Välj **free-style project** (freestyle-projekt) och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="97c5d-176">Select **free-style project**, and click **OK**.</span></span>
3. <span data-ttu-id="97c5d-177">Gå till jobbsidan och klicka sedan på **Konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="97c5d-177">Go the job page, and click **Configure**.</span></span>

   <span data-ttu-id="97c5d-178">a.</span><span class="sxs-lookup"><span data-stu-id="97c5d-178">a.</span></span> <span data-ttu-id="97c5d-179">Ange URL:en för GitHub-projektet under **GitHub-projekt** i det allmänna avsnittet.</span><span class="sxs-lookup"><span data-stu-id="97c5d-179">In the general section, under **GitHub project**, specify your GitHub project URL.</span></span> <span data-ttu-id="97c5d-180">Den här URL:en är värd för det Service Fabric Java-program som du vill integrera med Jenkins CI/CD-flödet (t.ex. ``https://github.com/sayantancs/SFJenkins``).</span><span class="sxs-lookup"><span data-stu-id="97c5d-180">This URL hosts the Service Fabric Java application that you want to integrate with the Jenkins continuous integration, continuous deployment (CI/CD) flow (for example, ``https://github.com/sayantancs/SFJenkins``).</span></span>

   <span data-ttu-id="97c5d-181">b.</span><span class="sxs-lookup"><span data-stu-id="97c5d-181">b.</span></span> <span data-ttu-id="97c5d-182">I avsnittet **Source Code Management** (Källkodshantering) väljer du **Git**.</span><span class="sxs-lookup"><span data-stu-id="97c5d-182">Under the **Source Code Management** section, select **Git**.</span></span> <span data-ttu-id="97c5d-183">Ange URL för databasen som är värd för det Service Fabric Java-program som du vill integrera med Jenkins CI/CD-flödet (t.ex. ``https://github.com/sayantancs/SFJenkins.git``).</span><span class="sxs-lookup"><span data-stu-id="97c5d-183">Specify the repository URL that hosts the Service Fabric Java application that you want to integrate with the Jenkins CI/CD flow (for example, ``https://github.com/sayantancs/SFJenkins.git``).</span></span> <span data-ttu-id="97c5d-184">Du kan också ange här vilken gren som ska byggas (t.ex. ***/master**).</span><span class="sxs-lookup"><span data-stu-id="97c5d-184">Also, you can specify here which branch to build (for example, **/master**).</span></span>
4. <span data-ttu-id="97c5d-185">Konfigurera din *GitHub* (som är värd för databasen) så att den kan kommunicera med Jenkins.</span><span class="sxs-lookup"><span data-stu-id="97c5d-185">Configure your *GitHub* (which is hosting the repository) so that it is able to talk to Jenkins.</span></span> <span data-ttu-id="97c5d-186">Använd följande steg:</span><span class="sxs-lookup"><span data-stu-id="97c5d-186">Use the following steps:</span></span>

   <span data-ttu-id="97c5d-187">a.</span><span class="sxs-lookup"><span data-stu-id="97c5d-187">a.</span></span> <span data-ttu-id="97c5d-188">Gå till GitHub-lagringsplatssidan.</span><span class="sxs-lookup"><span data-stu-id="97c5d-188">Go to your GitHub repository page.</span></span> <span data-ttu-id="97c5d-189">Gå till **Inställningar** > **Integrations and Services** (Integreringar och tjänster).</span><span class="sxs-lookup"><span data-stu-id="97c5d-189">Go to **Settings** > **Integrations and Services**.</span></span>

   <span data-ttu-id="97c5d-190">b.</span><span class="sxs-lookup"><span data-stu-id="97c5d-190">b.</span></span> <span data-ttu-id="97c5d-191">Välj **Lägg till tjänst**, skriv **Jenkins** och välj **Jenkins GitHub-plugin-programmet**.</span><span class="sxs-lookup"><span data-stu-id="97c5d-191">Select **Add Service**, type **Jenkins**, and select the **Jenkins-GitHub plugin**.</span></span>

   <span data-ttu-id="97c5d-192">c.</span><span class="sxs-lookup"><span data-stu-id="97c5d-192">c.</span></span> <span data-ttu-id="97c5d-193">Ange din Jenkins-webhooksadress (som standard ska den vara ``http://<PublicIPorFQDN>:8081/github-webhook/``).</span><span class="sxs-lookup"><span data-stu-id="97c5d-193">Enter your Jenkins webhook URL (by default, it should be ``http://<PublicIPorFQDN>:8081/github-webhook/``).</span></span> <span data-ttu-id="97c5d-194">Klicka på **Lägg till/Uppdatera tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="97c5d-194">Click **add/update service**.</span></span>

   <span data-ttu-id="97c5d-195">d.</span><span class="sxs-lookup"><span data-stu-id="97c5d-195">d.</span></span> <span data-ttu-id="97c5d-196">En testhändelse skickas till Jenkins-instansen.</span><span class="sxs-lookup"><span data-stu-id="97c5d-196">A test event is sent to your Jenkins instance.</span></span> <span data-ttu-id="97c5d-197">Du bör se en grön bock vid webhooken i GitHub och projektet skapas.</span><span class="sxs-lookup"><span data-stu-id="97c5d-197">You should see a green check by the webhook in GitHub, and your project will build.</span></span>

   <span data-ttu-id="97c5d-198">e.</span><span class="sxs-lookup"><span data-stu-id="97c5d-198">e.</span></span> <span data-ttu-id="97c5d-199">I avsnittet om **build-utlösare** väljer du önskat alternativ.</span><span class="sxs-lookup"><span data-stu-id="97c5d-199">Under the **Build Triggers** section, select which build option you want.</span></span> <span data-ttu-id="97c5d-200">I det här exemplet vill du utlösa en build när något skickas till databasen.</span><span class="sxs-lookup"><span data-stu-id="97c5d-200">For this example, you want to trigger a build whenever some push to the repository happens.</span></span> <span data-ttu-id="97c5d-201">Därför väljer du **GitHub hook trigger for GITScm polling** (GitHub-hookutlösare för GITScm-avsökning).</span><span class="sxs-lookup"><span data-stu-id="97c5d-201">So you select **GitHub hook trigger for GITScm polling**.</span></span> <span data-ttu-id="97c5d-202">(Tidigare hette det här alternativet **Build when a change is pushed to GitHub**) (Bygg när en ändring skickas till GitHub).</span><span class="sxs-lookup"><span data-stu-id="97c5d-202">(Previously, this option was called **Build when a change is pushed to GitHub**.)</span></span>

   <span data-ttu-id="97c5d-203">f.</span><span class="sxs-lookup"><span data-stu-id="97c5d-203">f.</span></span> <span data-ttu-id="97c5d-204">Under avsnittet **Build** (Bygg) i listrutan **Add build step** (Lägg till byggsteg) väljer du alternativet **Invoke Gradle Script** (Anropa Gradle-skript).</span><span class="sxs-lookup"><span data-stu-id="97c5d-204">Under the **Build section**, from the drop-down **Add build step**, select the option **Invoke Gradle Script**.</span></span> <span data-ttu-id="97c5d-205">I widgeten som visas anger du sökvägen till **rotbuildskript** för ditt program.</span><span class="sxs-lookup"><span data-stu-id="97c5d-205">In the widget that comes, specify the path to **Root build script** for your application.</span></span> <span data-ttu-id="97c5d-206">Då hämtas build.gradle från den angivna sökvägen och fungerar på motsvarande sätt.</span><span class="sxs-lookup"><span data-stu-id="97c5d-206">It picks up build.gradle from the path specified, and works accordingly.</span></span> <span data-ttu-id="97c5d-207">Om du skapar ett projekt med namnet ``MyActor`` (med Eclipse-plugin-programmet eller Yeoman-generatorn), ska rotbuildskriptet innehålla ``${WORKSPACE}/MyActor``.</span><span class="sxs-lookup"><span data-stu-id="97c5d-207">If you create a project named ``MyActor`` (using the Eclipse plug-in or Yeoman generator), the root build script should contain ``${WORKSPACE}/MyActor``.</span></span> <span data-ttu-id="97c5d-208">Följande skärmbild visar ett exempel på hur det kan se ut:</span><span class="sxs-lookup"><span data-stu-id="97c5d-208">See the following screenshot for an example of what this looks like:</span></span>

    ![Service Fabric Jenkins Build-åtgärd][build-step]

   <span data-ttu-id="97c5d-210">g.</span><span class="sxs-lookup"><span data-stu-id="97c5d-210">g.</span></span> <span data-ttu-id="97c5d-211">I listrutan **Post-Build Actions** (Åtgärder efter skapandet) väljer du **Deploy Service Fabric Project** (Distribuera Service Fabric-projekt).</span><span class="sxs-lookup"><span data-stu-id="97c5d-211">From the **Post-Build Actions** drop-down, select **Deploy Service Fabric Project**.</span></span> <span data-ttu-id="97c5d-212">Här måste du ange klusterinformation där Jenkins-kompilerade Service Fabric-programmet skulle distribueras.</span><span class="sxs-lookup"><span data-stu-id="97c5d-212">Here you need to provide cluster details where the Jenkins compiled Service Fabric application would be deployed.</span></span> <span data-ttu-id="97c5d-213">Du kan även ange ytterligare information som används för att distribuera programmet.</span><span class="sxs-lookup"><span data-stu-id="97c5d-213">You can also provide additional application details used to deploy the application.</span></span> <span data-ttu-id="97c5d-214">Följande skärmbild visar ett exempel på hur det kan se ut:</span><span class="sxs-lookup"><span data-stu-id="97c5d-214">See the following screenshot for an example of what this looks like:</span></span>

    ![Service Fabric Jenkins Build-åtgärd][post-build-step]

   > [!NOTE]
   > <span data-ttu-id="97c5d-216">Det här klustret kan vara detsamma som det kluster som är värd för Jenkins-behållarprogrammet om du använder Service Fabric för att distribuera Jenkins-behållaravbildningen.</span><span class="sxs-lookup"><span data-stu-id="97c5d-216">The cluster here could be same as the one hosting the Jenkins container application, in case you are using Service Fabric to deploy the Jenkins container image.</span></span>
   >

## <a name="next-steps"></a><span data-ttu-id="97c5d-217">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="97c5d-217">Next steps</span></span>
<span data-ttu-id="97c5d-218">GitHub och Jenkins har nu konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="97c5d-218">GitHub and Jenkins are now configured.</span></span> <span data-ttu-id="97c5d-219">Fundera över om du vill göra ändringar i ditt ``MyActor``-projekt i databasexemplet på https://github.com/sayantancs/SFJenkins.</span><span class="sxs-lookup"><span data-stu-id="97c5d-219">Consider making some sample change in your ``MyActor`` project in the repository example, https://github.com/sayantancs/SFJenkins.</span></span> <span data-ttu-id="97c5d-220">Skicka dina ändringar till en ``master``-fjärrgren (eller valfri gren som du använder i ditt projekt).</span><span class="sxs-lookup"><span data-stu-id="97c5d-220">Push your changes to a remote ``master`` branch (or any branch that you have configured to work with).</span></span> <span data-ttu-id="97c5d-221">Detta utlöser Jenkins-jobbet (``MyJob``) som du konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="97c5d-221">This triggers the Jenkins job, ``MyJob``, that you configured.</span></span> <span data-ttu-id="97c5d-222">Jobbet hämtar ändringarna från GitHub, bygger dem och distribuerar programmet till den klusterslutpunkt som du angav i åtgärderna efter byggprocessen.</span><span class="sxs-lookup"><span data-stu-id="97c5d-222">It fetches the changes from GitHub, builds them, and deploys the application to the cluster endpoint you specified in post-build actions.</span></span>  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png
