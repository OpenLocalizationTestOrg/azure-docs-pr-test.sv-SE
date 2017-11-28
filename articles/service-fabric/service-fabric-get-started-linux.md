---
title: "aaaSet in din utvecklingsmiljö i Linux | Microsoft Docs"
description: "Installera hello runtime och SDK och skapa en lokal utveckling kluster på Linux. När du har slutfört den här installationen kommer du att redo toobuild program."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/23/2017
ms.author: subramar
ms.openlocfilehash: 9d82c2015f9e2c6fb55f2052c7cdb1e906c5deeb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-development-environment-on-linux"></a><span data-ttu-id="2f32d-104">Förbereda utvecklingsmiljön i Linux</span><span class="sxs-lookup"><span data-stu-id="2f32d-104">Prepare your development environment on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2f32d-105">Windows</span><span class="sxs-lookup"><span data-stu-id="2f32d-105">Windows</span></span>](service-fabric-get-started.md)
> * [<span data-ttu-id="2f32d-106">Linux</span><span class="sxs-lookup"><span data-stu-id="2f32d-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="2f32d-107">OSX</span><span class="sxs-lookup"><span data-stu-id="2f32d-107">OSX</span></span>](service-fabric-get-started-mac.md)
>
>  

<span data-ttu-id="2f32d-108">toodeploy och kör [Azure Service Fabric program](service-fabric-application-model.md) på utvecklingsdatorn Linux installera hello körning och gemensamma SDK.</span><span class="sxs-lookup"><span data-stu-id="2f32d-108">toodeploy and run [Azure Service Fabric applications](service-fabric-application-model.md) on your Linux development machine, install hello runtime and common SDK.</span></span> <span data-ttu-id="2f32d-109">Du kan även installera SDK:er för Java och .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2f32d-109">You can also install optional SDKs for Java and .NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f32d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2f32d-110">Prerequisites</span></span>

<span data-ttu-id="2f32d-111">följande versioner av operativsystemet hello stöds för utveckling:</span><span class="sxs-lookup"><span data-stu-id="2f32d-111">hello following operating system versions are supported for development:</span></span>

* <span data-ttu-id="2f32d-112">Ubuntu 16.04 (`Xenial Xerus`)</span><span class="sxs-lookup"><span data-stu-id="2f32d-112">Ubuntu 16.04 (`Xenial Xerus`)</span></span>

## <a name="update-your-apt-sources"></a><span data-ttu-id="2f32d-113">Uppdatera dina APT-källor</span><span class="sxs-lookup"><span data-stu-id="2f32d-113">Update your APT sources</span></span>
<span data-ttu-id="2f32d-114">tooinstall hello SDK och hello associerade runtime-paketet via hello lgh get-kommandoradsverktyg, måste du först uppdatera avancerade paketering verktyget (LGH)-datakällor.</span><span class="sxs-lookup"><span data-stu-id="2f32d-114">tooinstall hello SDK and hello associated runtime package via hello apt-get command-line tool, you must first update your Advanced Packaging Tool (APT) sources.</span></span>

1. <span data-ttu-id="2f32d-115">Öppna en terminal.</span><span class="sxs-lookup"><span data-stu-id="2f32d-115">Open a terminal.</span></span>
2. <span data-ttu-id="2f32d-116">Lägg till hello Service Fabric lagringsplatsen tooyour källor lista.</span><span class="sxs-lookup"><span data-stu-id="2f32d-116">Add hello Service Fabric repo tooyour sources list.</span></span>

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ xenial main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. <span data-ttu-id="2f32d-117">Lägg till hello `dotnet` lagringsplatsen tooyour källor lista.</span><span class="sxs-lookup"><span data-stu-id="2f32d-117">Add hello `dotnet` repo tooyour sources list.</span></span>

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. <span data-ttu-id="2f32d-118">Lägg till hello nya Gnu sekretess Guard (GnuPG eller GPG) nyckeln tooyour LGH nyckelring.</span><span class="sxs-lookup"><span data-stu-id="2f32d-118">Add hello new Gnu Privacy Guard (GnuPG, or GPG) key tooyour APT keyring.</span></span>

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. <span data-ttu-id="2f32d-119">Lägg till hello officiella Docker GPG viktiga tooyour LGH nyckelring.</span><span class="sxs-lookup"><span data-stu-id="2f32d-119">Add hello official Docker GPG key tooyour APT keyring.</span></span>

    ```bash
    sudo apt-get install curl
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. <span data-ttu-id="2f32d-120">Ställ in hello Docker-databasen.</span><span class="sxs-lookup"><span data-stu-id="2f32d-120">Set up hello Docker repository.</span></span>

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. <span data-ttu-id="2f32d-121">Uppdatera paketet innehåller baserat på hello nyligen lagt till databaser.</span><span class="sxs-lookup"><span data-stu-id="2f32d-121">Refresh your package lists based on hello newly added repositories.</span></span>

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-hello-sdk-for-local-cluster-setup"></a><span data-ttu-id="2f32d-122">Installera och konfigurera hello SDK för konfiguration av lokal</span><span class="sxs-lookup"><span data-stu-id="2f32d-122">Install and set up hello SDK for local cluster setup</span></span>

<span data-ttu-id="2f32d-123">När du har uppdaterat dina datakällor kan du installera hello SDK.</span><span class="sxs-lookup"><span data-stu-id="2f32d-123">After you have updated your sources, you can install hello SDK.</span></span> <span data-ttu-id="2f32d-124">Installera hello Service Fabric SDK-paketet, bekräfta hello installationen och accepterar toohello licensavtalet (EULA).</span><span class="sxs-lookup"><span data-stu-id="2f32d-124">Install hello Service Fabric SDK package, confirm hello installation, and agree toohello license agreement.</span></span>

```bash
sudo apt-get install servicefabricsdkcommon
```

>   [!TIP]
>   <span data-ttu-id="2f32d-125">hello automatisera följande kommandon accepterar hello-licens för Service Fabric-paket:</span><span class="sxs-lookup"><span data-stu-id="2f32d-125">hello following commands automate accepting hello license for Service Fabric packages:</span></span>
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-v1 select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | sudo debconf-set-selections
>   ```

## <a name="set-up-a-local-cluster"></a><span data-ttu-id="2f32d-126">Konfigurera ett lokalt kluster</span><span class="sxs-lookup"><span data-stu-id="2f32d-126">Set up a local cluster</span></span>
  <span data-ttu-id="2f32d-127">Om hello-installationen har slutförts ska kunna toostart lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="2f32d-127">If hello installation is successful, you should be able toostart a local cluster.</span></span>

  1. <span data-ttu-id="2f32d-128">Kör hello installationsskriptet för klustret.</span><span class="sxs-lookup"><span data-stu-id="2f32d-128">Run hello cluster setup script.</span></span>

      ```bash
      sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
      ```

  2. <span data-ttu-id="2f32d-129">Öppna en webbläsare och gå för[Service Fabric Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="2f32d-129">Open a web browser and go too[Service Fabric Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="2f32d-130">Du bör se hello Service Fabric Explorer instrumentpanelen om hello klustret har startats.</span><span class="sxs-lookup"><span data-stu-id="2f32d-130">If hello cluster has started, you should see hello Service Fabric Explorer dashboard.</span></span>

      ![Service Fabric Explorer på Linux][sfx-linux]

  <span data-ttu-id="2f32d-132">Nu kan du distribuera fördefinierade Service Fabric-programpaket eller nya paket baserat på gästbehållare eller körbara gästprogram.</span><span class="sxs-lookup"><span data-stu-id="2f32d-132">At this point, you can deploy pre-built Service Fabric application packages or new ones based on guest containers or guest executables.</span></span> <span data-ttu-id="2f32d-133">toobuild nya tjänster med hjälp av hello Java eller .NET Core SDK följer hello valfria konfigurationssteg som finns i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2f32d-133">toobuild new services by using hello Java or .NET Core SDKs, follow hello optional setup steps that are provided in subsequent sections.</span></span>


  > [!NOTE]
  > <span data-ttu-id="2f32d-134">Fristående kluster stöds inte i Linux.</span><span class="sxs-lookup"><span data-stu-id="2f32d-134">Standalone clusters aren't supported in Linux.</span></span> <span data-ttu-id="2f32d-135">hello preview stöder endast en ruta och flera datorer Azure Linux-kluster.</span><span class="sxs-lookup"><span data-stu-id="2f32d-135">hello preview supports only one-box and Azure Linux multi-machine clusters.</span></span>
  >

## <a name="set-up-hello-service-fabric-cli"></a><span data-ttu-id="2f32d-136">Ställ in hello Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="2f32d-136">Set up hello Service Fabric CLI</span></span>

<span data-ttu-id="2f32d-137">Hej [Service Fabric CLI](service-fabric-cli.md) har kommandon för att interagera med Service Fabric-enheter, inklusive kluster och program.</span><span class="sxs-lookup"><span data-stu-id="2f32d-137">hello [Service Fabric CLI](service-fabric-cli.md) has commands for interacting with Service Fabric entities, including clusters and applications.</span></span> <span data-ttu-id="2f32d-138">Den är baserad på python, så att toohave python och pip-installeras innan du fortsätter med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2f32d-138">It is based on python, so be sure toohave python and pip installed before you proceed with hello following command:</span></span>

```bash
pip install sfctl
```

## <a name="install-and-set-up-hello-generators-for-containers-and-guest-executables"></a><span data-ttu-id="2f32d-139">Installera och konfigurera hello generatorer för behållare och Gäst-körbara filer</span><span class="sxs-lookup"><span data-stu-id="2f32d-139">Install and set up hello generators for containers and guest-executables</span></span>
<span data-ttu-id="2f32d-140">Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa ett Service Fabric-program från terminalen med en Yeoman-mallgenerator.</span><span class="sxs-lookup"><span data-stu-id="2f32d-140">Service Fabric provides scaffolding tools which will help you create a Service Fabric applications from terminal using Yeoman template generator.</span></span> <span data-ttu-id="2f32d-141">Följ hello stegen nedan tooensure som du har hello Service Fabric yeoman mall generatorn för att arbeta på din dator.</span><span class="sxs-lookup"><span data-stu-id="2f32d-141">Please follow hello steps below tooensure you have hello Service Fabric yeoman template generator for working on your machine.</span></span>

1. <span data-ttu-id="2f32d-142">Installera nodejs och NPM på datorn</span><span class="sxs-lookup"><span data-stu-id="2f32d-142">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="2f32d-143">Installera [Yeoman](http://yeoman.io/)-mallgeneratorn på datorn från NPM</span><span class="sxs-lookup"><span data-stu-id="2f32d-143">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="2f32d-144">Installera hello Service Fabric Yeo behållare generator och Gäst execuatble generator från NPM</span><span class="sxs-lookup"><span data-stu-id="2f32d-144">Install hello Service Fabric Yeo container generator and guest execuatble generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfcontainer  # for Service Fabric container application
  sudo npm install -g generator-azuresfguest      # for Service Fabric guest executable application
  ```

<span data-ttu-id="2f32d-145">När du har installerat hello ovan generatorer, bör du kunna toocreate appar med gästtjänster för körbara filer eller behållare genom att köra `yo azuresfguest` eller `yo azuresfcontainer` respektive.</span><span class="sxs-lookup"><span data-stu-id="2f32d-145">After you have installed hello above generators, you should be able toocreate apps with guest executable or container services by running `yo azuresfguest` or `yo azuresfcontainer` respectively.</span></span>

## <a name="install-hello-necessary-java-artifacts-optional-if-you-want-toouse-hello-java-programming-models"></a><span data-ttu-id="2f32d-146">Installera hello nödvändiga Java artefakter (valfritt, om du vill toouse hello Java programmeringsmodeller)</span><span class="sxs-lookup"><span data-stu-id="2f32d-146">Install hello necessary Java artifacts (optional, if you want toouse hello Java programming models)</span></span>

<span data-ttu-id="2f32d-147">toobuild Service Fabric-tjänster med hjälp av Java, kontrollera att du har JDK 1.8 installeras tillsammans med Gradle som används för att köra build-uppgifter.</span><span class="sxs-lookup"><span data-stu-id="2f32d-147">toobuild Service Fabric services using Java, ensure you have JDK 1.8 installed along with Gradle which is used for running build tasks.</span></span> <span data-ttu-id="2f32d-148">följande fragment hello installerar öppna JDK 1.8 tillsammans med Gradle.</span><span class="sxs-lookup"><span data-stu-id="2f32d-148">hello following snippet installs Open JDK 1.8 along with Gradle.</span></span> <span data-ttu-id="2f32d-149">hello Service Fabric Java bibliotek hämtas från Maven.</span><span class="sxs-lookup"><span data-stu-id="2f32d-149">hello Service Fabric Java libraries are pulled from Maven.</span></span>

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

## <a name="install-hello-eclipse-neon-plug-in-optional"></a><span data-ttu-id="2f32d-150">Installera hello Eclipse Neon plugin-programmet (valfritt)</span><span class="sxs-lookup"><span data-stu-id="2f32d-150">Install hello Eclipse Neon plug-in (optional)</span></span>

<span data-ttu-id="2f32d-151">Du kan installera hello plugin-programmet Eclipse för Service Fabric från inom hello **Eclipse IDE för Java-utvecklare**.</span><span class="sxs-lookup"><span data-stu-id="2f32d-151">You can install hello Eclipse plug-in for Service Fabric from within hello **Eclipse IDE for Java Developers**.</span></span> <span data-ttu-id="2f32d-152">Du kan använda Eclipse toocreate Service Fabric gäst körbara program och behållarprogram i tillägg tooService Fabric Java-program.</span><span class="sxs-lookup"><span data-stu-id="2f32d-152">You can use Eclipse toocreate Service Fabric guest executable applications and container applications in addition tooService Fabric Java applications.</span></span>

1. <span data-ttu-id="2f32d-153">Se till att du har den senaste Eclipse Neon och hello senaste Buildship versionen i Eclipse (1.0.17 eller senare) installerat.</span><span class="sxs-lookup"><span data-stu-id="2f32d-153">In Eclipse, ensure that you have latest Eclipse Neon and hello latest Buildship version (1.0.17 or later) installed.</span></span> <span data-ttu-id="2f32d-154">Du kan kontrollera hello versioner av installerade komponenter genom att välja **hjälp** > **installationsinformationen**.</span><span class="sxs-lookup"><span data-stu-id="2f32d-154">You can check hello versions of installed components by selecting **Help** > **Installation Details**.</span></span> <span data-ttu-id="2f32d-155">Du kan uppdatera Buildship med hjälp av hello instruktionerna på [Eclipse Buildship: Eclipse plugin-program för Gradle][buildship-update].</span><span class="sxs-lookup"><span data-stu-id="2f32d-155">You can update Buildship by using hello instructions at [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update].</span></span>

2. <span data-ttu-id="2f32d-156">tooinstall hello Service Fabric-plugin-program, Välj **hjälp** > **installera ny programvara**.</span><span class="sxs-lookup"><span data-stu-id="2f32d-156">tooinstall hello Service Fabric plug-in, select **Help** > **Install New Software**.</span></span>

3. <span data-ttu-id="2f32d-157">I hello **arbeta med** skriver **http://dl.microsoft.com/eclipse**.</span><span class="sxs-lookup"><span data-stu-id="2f32d-157">In hello **Work with** box, type **http://dl.microsoft.com/eclipse**.</span></span>

4. <span data-ttu-id="2f32d-158">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2f32d-158">Click **Add**.</span></span>

    ![hello tillgänglig programvara sida][sf-eclipse-plugin]

5. <span data-ttu-id="2f32d-160">Välj hello **ServiceFabric** plugin-program och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2f32d-160">Select hello **ServiceFabric** plug-in, and then click **Next**.</span></span>

6. <span data-ttu-id="2f32d-161">Slutföra hello installationssteg och acceptera hello licensavtalet.</span><span class="sxs-lookup"><span data-stu-id="2f32d-161">Complete hello installation steps, and then accept hello end-user license agreement.</span></span>

<span data-ttu-id="2f32d-162">Kontrollera att du har hello senaste versionen om du redan har hello Service Fabric Eclipse plugin-program installerat.</span><span class="sxs-lookup"><span data-stu-id="2f32d-162">If you already have hello Service Fabric Eclipse plug-in installed, make sure that you have hello latest version.</span></span> <span data-ttu-id="2f32d-163">Du kan kontrollera genom att välja **hjälp** > **installationsinformationen** och sedan söka efter Service Fabric hello listan över installerade plugin-program. Välj **Uppdatera** om det finns en nyare version.</span><span class="sxs-lookup"><span data-stu-id="2f32d-163">You can check by selecting **Help** > **Installation Details** and then searching for Service Fabric in hello list of installed plug-ins. If a newer version is available, select **Update**.</span></span>

<span data-ttu-id="2f32d-164">Mer information finns i [Service Fabric-plugin-program för utveckling av Java-program i Eclipse](service-fabric-get-started-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="2f32d-164">For more information, see [Service Fabric plug-in for Eclipse Java application development](service-fabric-get-started-eclipse.md).</span></span>


## <a name="install-hello-net-core-sdk-optional-if-you-want-toouse-hello-net-core-programming-models"></a><span data-ttu-id="2f32d-165">Installera hello .NET Core SDK (valfritt, om du vill toouse hello .NET Core programmeringsmodeller)</span><span class="sxs-lookup"><span data-stu-id="2f32d-165">Install hello .NET Core SDK (optional, if you want toouse hello .NET Core programming models)</span></span>
<span data-ttu-id="2f32d-166">hello .NET Core SDK innehåller hello bibliotek och mallar som är nödvändiga toobuild Service Fabric-tjänster med .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2f32d-166">hello .NET Core SDK provides hello libraries and templates that are required toobuild Service Fabric services with .NET Core.</span></span> <span data-ttu-id="2f32d-167">Installera hello .NET Core SDK-paketet genom att köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="2f32d-167">Install hello .NET Core SDK package by running hello following -</span></span>

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

## <a name="update-hello-sdk-and-runtime"></a><span data-ttu-id="2f32d-168">Uppdatera hello SDK och körning</span><span class="sxs-lookup"><span data-stu-id="2f32d-168">Update hello SDK and runtime</span></span>

<span data-ttu-id="2f32d-169">tooupdate toohello senaste versionen av hello SDK och körning, kör följande kommandon hello (Avmarkera hello SDK: er som du inte vill):</span><span class="sxs-lookup"><span data-stu-id="2f32d-169">tooupdate toohello latest version of hello SDK and runtime, run hello following commands (deselect hello SDKs that you don't want):</span></span>

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp
```
<span data-ttu-id="2f32d-170">tooupdate hello Java SDK binärfiler från Maven du behöver tooupdate hello version information hello motsvarande binära i hello ``build.gradle`` filen toopoint toohello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="2f32d-170">tooupdate hello Java SDK binaries from Maven, you need tooupdate hello version details of hello corresponding binary in hello ``build.gradle`` file toopoint toohello latest version.</span></span> <span data-ttu-id="2f32d-171">tooknow exakt där du behöver tooupdate hello version, kan du läsa tooany ``build.gradle`` filen i Service Fabric Kom igång-exempel [här](https://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="2f32d-171">tooknow exactly where you need tooupdate hello version, you can refer tooany ``build.gradle`` file in Service Fabric getting-started samples [here](https://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>

> [!NOTE]
> <span data-ttu-id="2f32d-172">Uppdatera hello-paket kan det leda till att din toostop för lokal utveckling-kluster som körs.</span><span class="sxs-lookup"><span data-stu-id="2f32d-172">Updating hello packages might cause your local development cluster toostop running.</span></span> <span data-ttu-id="2f32d-173">Starta om dina lokala kluster efter en uppgradering genom att följa hello anvisningar på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="2f32d-173">Restart your local cluster after an upgrade by following hello instructions on this page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f32d-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2f32d-174">Next steps</span></span>

* [<span data-ttu-id="2f32d-175">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med hjälp av Yeoman</span><span class="sxs-lookup"><span data-stu-id="2f32d-175">Create and deploy your first Service Fabric Java application on Linux by using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="2f32d-176">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med Service Fabric-plugin-programmet för Eclipse</span><span class="sxs-lookup"><span data-stu-id="2f32d-176">Create and deploy your first Service Fabric Java application on Linux by using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="2f32d-177">Skapa ditt första CSharp-program i Linux</span><span class="sxs-lookup"><span data-stu-id="2f32d-177">Create your first CSharp application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
* [<span data-ttu-id="2f32d-178">Förbereda utvecklingsmiljön i OSX</span><span class="sxs-lookup"><span data-stu-id="2f32d-178">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="2f32d-179">Använd hello Service Fabric CLI toomanage dina program</span><span class="sxs-lookup"><span data-stu-id="2f32d-179">Use hello Service Fabric CLI toomanage your applications</span></span>](service-fabric-application-lifecycle-sfctl.md)
* [<span data-ttu-id="2f32d-180">Skillnader mellan Service Fabric i Windows och Linux</span><span class="sxs-lookup"><span data-stu-id="2f32d-180">Service Fabric Windows/Linux differences</span></span>](service-fabric-linux-windows-differences.md)
* [<span data-ttu-id="2f32d-181">Kom igång med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="2f32d-181">Get started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
