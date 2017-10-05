---
title: "Konfigurera en utvecklingsmiljö i Linux | Microsoft Docs"
description: "Installera runtime och SDK, och skapa ett lokalt utvecklingskluster i Linux. När du har slutfört den här installationen är du redo att börja bygga program."
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
ms.openlocfilehash: 58c6bbbb16d7008e6b573cf8dbc8cf62da9789f5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="prepare-your-development-environment-on-linux"></a><span data-ttu-id="7177b-104">Förbereda utvecklingsmiljön i Linux</span><span class="sxs-lookup"><span data-stu-id="7177b-104">Prepare your development environment on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7177b-105">Windows</span><span class="sxs-lookup"><span data-stu-id="7177b-105">Windows</span></span>](service-fabric-get-started.md)
> * [<span data-ttu-id="7177b-106">Linux</span><span class="sxs-lookup"><span data-stu-id="7177b-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="7177b-107">OSX</span><span class="sxs-lookup"><span data-stu-id="7177b-107">OSX</span></span>](service-fabric-get-started-mac.md)
>
>  

<span data-ttu-id="7177b-108">För att kunna skapa och köra [Azure Service Fabric-program](service-fabric-application-model.md) på en Linux-utvecklingsdator måste du installera runtime och SDK.</span><span class="sxs-lookup"><span data-stu-id="7177b-108">To deploy and run [Azure Service Fabric applications](service-fabric-application-model.md) on your Linux development machine, install the runtime and common SDK.</span></span> <span data-ttu-id="7177b-109">Du kan även installera SDK:er för Java och .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7177b-109">You can also install optional SDKs for Java and .NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7177b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7177b-110">Prerequisites</span></span>

<span data-ttu-id="7177b-111">Följande operativsystemversioner stöds för utveckling:</span><span class="sxs-lookup"><span data-stu-id="7177b-111">The following operating system versions are supported for development:</span></span>

* <span data-ttu-id="7177b-112">Ubuntu 16.04 (`Xenial Xerus`)</span><span class="sxs-lookup"><span data-stu-id="7177b-112">Ubuntu 16.04 (`Xenial Xerus`)</span></span>

## <a name="update-your-apt-sources"></a><span data-ttu-id="7177b-113">Uppdatera dina APT-källor</span><span class="sxs-lookup"><span data-stu-id="7177b-113">Update your APT sources</span></span>
<span data-ttu-id="7177b-114">Om du vill installera SDK och det tillhörande runtime-paketet via kommandoradsverktyget apt-get så måste du först uppdatera dina APT (Advanced Packaging Tool)-källor.</span><span class="sxs-lookup"><span data-stu-id="7177b-114">To install the SDK and the associated runtime package via the apt-get command-line tool, you must first update your Advanced Packaging Tool (APT) sources.</span></span>

1. <span data-ttu-id="7177b-115">Öppna en terminal.</span><span class="sxs-lookup"><span data-stu-id="7177b-115">Open a terminal.</span></span>
2. <span data-ttu-id="7177b-116">Lägga till Service Fabric-repon i listan med källor.</span><span class="sxs-lookup"><span data-stu-id="7177b-116">Add the Service Fabric repo to your sources list.</span></span>

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ xenial main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. <span data-ttu-id="7177b-117">Lägg till `dotnet`-repon i listan med källor.</span><span class="sxs-lookup"><span data-stu-id="7177b-117">Add the `dotnet` repo to your sources list.</span></span>

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. <span data-ttu-id="7177b-118">Lägg till den nya Gnu Privacy Guard-nyckeln (GnuPG eller GPG) i APT-nyckelringen.</span><span class="sxs-lookup"><span data-stu-id="7177b-118">Add the new Gnu Privacy Guard (GnuPG, or GPG) key to your APT keyring.</span></span>

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. <span data-ttu-id="7177b-119">Lägg till den officiella Docker GPG-nyckeln i din APT-nyckelring.</span><span class="sxs-lookup"><span data-stu-id="7177b-119">Add the official Docker GPG key to your APT keyring.</span></span>

    ```bash
    sudo apt-get install curl
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. <span data-ttu-id="7177b-120">Ställ in Docker-databasen.</span><span class="sxs-lookup"><span data-stu-id="7177b-120">Set up the Docker repository.</span></span>

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. <span data-ttu-id="7177b-121">Uppdatera paketlistor baserat på nyligen tillagda lagringsplatser.</span><span class="sxs-lookup"><span data-stu-id="7177b-121">Refresh your package lists based on the newly added repositories.</span></span>

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk-for-local-cluster-setup"></a><span data-ttu-id="7177b-122">Installera och konfigurera SDK för lokal klusterkonfiguration</span><span class="sxs-lookup"><span data-stu-id="7177b-122">Install and set up the SDK for local cluster setup</span></span>

<span data-ttu-id="7177b-123">När du har uppdaterat källorna kan du installera SDK.</span><span class="sxs-lookup"><span data-stu-id="7177b-123">After you have updated your sources, you can install the SDK.</span></span> <span data-ttu-id="7177b-124">Installera Service Fabric SDK-paketet, bekräfta installationen och acceptera licensavtalet (EULA).</span><span class="sxs-lookup"><span data-stu-id="7177b-124">Install the Service Fabric SDK package, confirm the installation, and agree to the license agreement.</span></span>

```bash
sudo apt-get install servicefabricsdkcommon
```

>   [!TIP]
>   <span data-ttu-id="7177b-125">Följande kommandon automatiserar godkännandet av licensen för Service Fabric-paket:</span><span class="sxs-lookup"><span data-stu-id="7177b-125">The following commands automate accepting the license for Service Fabric packages:</span></span>
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-v1 select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | sudo debconf-set-selections
>   ```

## <a name="set-up-a-local-cluster"></a><span data-ttu-id="7177b-126">Konfigurera ett lokalt kluster</span><span class="sxs-lookup"><span data-stu-id="7177b-126">Set up a local cluster</span></span>
  <span data-ttu-id="7177b-127">Om installationen har slutförts kan du starta ett lokalt kluster.</span><span class="sxs-lookup"><span data-stu-id="7177b-127">If the installation is successful, you should be able to start a local cluster.</span></span>

  1. <span data-ttu-id="7177b-128">Kör klusterinstallationsskriptet.</span><span class="sxs-lookup"><span data-stu-id="7177b-128">Run the cluster setup script.</span></span>

      ```bash
      sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
      ```

  2. <span data-ttu-id="7177b-129">Öppna en webbläsare och gå till [Service Fabric Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="7177b-129">Open a web browser and go to [Service Fabric Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="7177b-130">Om klustret har startats visas instrumentpanelen Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="7177b-130">If the cluster has started, you should see the Service Fabric Explorer dashboard.</span></span>

      ![Service Fabric Explorer på Linux][sfx-linux]

  <span data-ttu-id="7177b-132">Nu kan du distribuera fördefinierade Service Fabric-programpaket eller nya paket baserat på gästbehållare eller körbara gästprogram.</span><span class="sxs-lookup"><span data-stu-id="7177b-132">At this point, you can deploy pre-built Service Fabric application packages or new ones based on guest containers or guest executables.</span></span> <span data-ttu-id="7177b-133">Om du vill skapa nya tjänster med SDK:er för Java eller .NET Core, följer du installationsanvisningarna i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7177b-133">To build new services by using the Java or .NET Core SDKs, follow the optional setup steps that are provided in subsequent sections.</span></span>


  > [!NOTE]
  > <span data-ttu-id="7177b-134">Fristående kluster stöds inte i Linux.</span><span class="sxs-lookup"><span data-stu-id="7177b-134">Standalone clusters aren't supported in Linux.</span></span> <span data-ttu-id="7177b-135">Förhandsversionen stöder endast one-box-kluster och kluster på flera Azure Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="7177b-135">The preview supports only one-box and Azure Linux multi-machine clusters.</span></span>
  >

## <a name="set-up-the-service-fabric-cli"></a><span data-ttu-id="7177b-136">Konfigurera Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="7177b-136">Set up the Service Fabric CLI</span></span>

<span data-ttu-id="7177b-137">[Service Fabric CLI](service-fabric-cli.md) innehåller kommandon för att interagera med Service Fabric-entiteter, t.ex. kluster och program.</span><span class="sxs-lookup"><span data-stu-id="7177b-137">The [Service Fabric CLI](service-fabric-cli.md) has commands for interacting with Service Fabric entities, including clusters and applications.</span></span> <span data-ttu-id="7177b-138">Den är baserad på python, så se till att python och pip är installerade innan du fortsätter med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7177b-138">It is based on python, so be sure to have python and pip installed before you proceed with the following command:</span></span>

```bash
pip install sfctl
```

## <a name="install-and-set-up-the-generators-for-containers-and-guest-executables"></a><span data-ttu-id="7177b-139">Installera och konfigurera generatorer för behållare och körbara gästprogram</span><span class="sxs-lookup"><span data-stu-id="7177b-139">Install and set up the generators for containers and guest-executables</span></span>
<span data-ttu-id="7177b-140">Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa ett Service Fabric-program från terminalen med en Yeoman-mallgenerator.</span><span class="sxs-lookup"><span data-stu-id="7177b-140">Service Fabric provides scaffolding tools which will help you create a Service Fabric applications from terminal using Yeoman template generator.</span></span> <span data-ttu-id="7177b-141">Följ stegen nedan för att se till att du har Service Fabric Yeoman-mallgeneratorn på datorn.</span><span class="sxs-lookup"><span data-stu-id="7177b-141">Please follow the steps below to ensure you have the Service Fabric yeoman template generator for working on your machine.</span></span>

1. <span data-ttu-id="7177b-142">Installera nodejs och NPM på datorn</span><span class="sxs-lookup"><span data-stu-id="7177b-142">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="7177b-143">Installera [Yeoman](http://yeoman.io/)-mallgeneratorn på datorn från NPM</span><span class="sxs-lookup"><span data-stu-id="7177b-143">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="7177b-144">Installera Yeo-behållargeneratorn för Service Fabric och generatorn för körbara gästprogram från NPM</span><span class="sxs-lookup"><span data-stu-id="7177b-144">Install the Service Fabric Yeo container generator and guest execuatble generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfcontainer  # for Service Fabric container application
  sudo npm install -g generator-azuresfguest      # for Service Fabric guest executable application
  ```

<span data-ttu-id="7177b-145">När du har installerat generatorerna ovan kan du skapa appar med körbara gästprogram eller behållartjänster genom att köra `yo azuresfguest` eller `yo azuresfcontainer`.</span><span class="sxs-lookup"><span data-stu-id="7177b-145">After you have installed the above generators, you should be able to create apps with guest executable or container services by running `yo azuresfguest` or `yo azuresfcontainer` respectively.</span></span>

## <a name="install-the-necessary-java-artifacts-optional-if-you-want-to-use-the-java-programming-models"></a><span data-ttu-id="7177b-146">Installera nödvändiga Java-artefakter (valfritt om du vill använda Java-programmeringsmodeller)</span><span class="sxs-lookup"><span data-stu-id="7177b-146">Install the necessary Java artifacts (optional, if you want to use the Java programming models)</span></span>

<span data-ttu-id="7177b-147">Om du vill skapa Service Fabric-tjänster med hjälp av Java ska du kontrollera att du har JDK 1.8 installerat tillsammans med Gradle, som används för att köra build-uppgifter.</span><span class="sxs-lookup"><span data-stu-id="7177b-147">To build Service Fabric services using Java, ensure you have JDK 1.8 installed along with Gradle which is used for running build tasks.</span></span> <span data-ttu-id="7177b-148">Följande kodfragment installerar Open JDK 1.8 tillsammans med Gradle.</span><span class="sxs-lookup"><span data-stu-id="7177b-148">The following snippet installs Open JDK 1.8 along with Gradle.</span></span> <span data-ttu-id="7177b-149">Java-biblioteken för Service Fabric hämtas från Maven.</span><span class="sxs-lookup"><span data-stu-id="7177b-149">The Service Fabric Java libraries are pulled from Maven.</span></span>

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

## <a name="install-the-eclipse-neon-plug-in-optional"></a><span data-ttu-id="7177b-150">Installera Eclipse Neon-plugin-programmet (valfritt)</span><span class="sxs-lookup"><span data-stu-id="7177b-150">Install the Eclipse Neon plug-in (optional)</span></span>

<span data-ttu-id="7177b-151">Du kan installera Eclipse-plugin-programmet för Service Fabric i **Eclipse IDE för Java-utvecklare**.</span><span class="sxs-lookup"><span data-stu-id="7177b-151">You can install the Eclipse plug-in for Service Fabric from within the **Eclipse IDE for Java Developers**.</span></span> <span data-ttu-id="7177b-152">Du kan använda Eclipse för att skapa körbara Service Fabric-gästprogram och behållarprogram utöver Service Fabric Java-program.</span><span class="sxs-lookup"><span data-stu-id="7177b-152">You can use Eclipse to create Service Fabric guest executable applications and container applications in addition to Service Fabric Java applications.</span></span>

1. <span data-ttu-id="7177b-153">Kontrollera att du har den senaste Eclipse Neon-versionen och den senaste Buildship-versionen (1.0.17 eller senare) installerat.</span><span class="sxs-lookup"><span data-stu-id="7177b-153">In Eclipse, ensure that you have latest Eclipse Neon and the latest Buildship version (1.0.17 or later) installed.</span></span> <span data-ttu-id="7177b-154">Du kan kontrollera vilka versioner de installerade komponenterna har genom att välja **Hjälp** > **Installationsinformation**.</span><span class="sxs-lookup"><span data-stu-id="7177b-154">You can check the versions of installed components by selecting **Help** > **Installation Details**.</span></span> <span data-ttu-id="7177b-155">Om du vill uppdatera Buildship kan du läsa [Eclipse Buildship: Eclipse-plugin-program för Gradle][buildship-update].</span><span class="sxs-lookup"><span data-stu-id="7177b-155">You can update Buildship by using the instructions at [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update].</span></span>

2. <span data-ttu-id="7177b-156">Om du vill installera Service Fabric-plugin-programmet väljer du **Hjälp** > **Installera ny programvara**.</span><span class="sxs-lookup"><span data-stu-id="7177b-156">To install the Service Fabric plug-in, select **Help** > **Install New Software**.</span></span>

3. <span data-ttu-id="7177b-157">Ange **http://dl.microsoft.com/eclipse** i textrutan **Arbeta med**.</span><span class="sxs-lookup"><span data-stu-id="7177b-157">In the **Work with** box, type **http://dl.microsoft.com/eclipse**.</span></span>

4. <span data-ttu-id="7177b-158">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7177b-158">Click **Add**.</span></span>

    ![Sidan Tillgänglig programvara][sf-eclipse-plugin]

5. <span data-ttu-id="7177b-160">Välj **Service Fabric**-plugin-programmet och klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="7177b-160">Select the **ServiceFabric** plug-in, and then click **Next**.</span></span>

6. <span data-ttu-id="7177b-161">Slutför installationsstegen och acceptera licensavtalet för användare.</span><span class="sxs-lookup"><span data-stu-id="7177b-161">Complete the installation steps, and then accept the end-user license agreement.</span></span>

<span data-ttu-id="7177b-162">Om du redan har Service Fabric Eclipse-plugin-programmet installerat kontrollerar du att du har den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="7177b-162">If you already have the Service Fabric Eclipse plug-in installed, make sure that you have the latest version.</span></span> <span data-ttu-id="7177b-163">Du kan kontrollera detta genom att välja **Hjälp** > **Installationsinformation** och sedan söka efter Service Fabric i listan över installerade plugin-program. Välj **Uppdatera** om det finns en nyare version.</span><span class="sxs-lookup"><span data-stu-id="7177b-163">You can check by selecting **Help** > **Installation Details** and then searching for Service Fabric in the list of installed plug-ins. If a newer version is available, select **Update**.</span></span>

<span data-ttu-id="7177b-164">Mer information finns i [Service Fabric-plugin-program för utveckling av Java-program i Eclipse](service-fabric-get-started-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="7177b-164">For more information, see [Service Fabric plug-in for Eclipse Java application development](service-fabric-get-started-eclipse.md).</span></span>


## <a name="install-the-net-core-sdk-optional-if-you-want-to-use-the-net-core-programming-models"></a><span data-ttu-id="7177b-165">Installera .NET Core SDK (valfritt, om du vill använda .NET Core-programmeringsmodeller)</span><span class="sxs-lookup"><span data-stu-id="7177b-165">Install the .NET Core SDK (optional, if you want to use the .NET Core programming models)</span></span>
<span data-ttu-id="7177b-166">.NET Core SDK innehåller de bibliotek och mallar som krävs för att skapa Service Fabric-tjänster med .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7177b-166">The .NET Core SDK provides the libraries and templates that are required to build Service Fabric services with .NET Core.</span></span> <span data-ttu-id="7177b-167">Installera .NET Core SDK-paketet genom att köra följande -</span><span class="sxs-lookup"><span data-stu-id="7177b-167">Install the .NET Core SDK package by running the following -</span></span>

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

## <a name="update-the-sdk-and-runtime"></a><span data-ttu-id="7177b-168">Uppdatera SDK och Runtime</span><span class="sxs-lookup"><span data-stu-id="7177b-168">Update the SDK and runtime</span></span>

<span data-ttu-id="7177b-169">Om du vill uppdatera till den senaste versionen av SDK och Runtime kör du följande kommandon (avmarkera de SDK:er från listan som du inte vill ha):</span><span class="sxs-lookup"><span data-stu-id="7177b-169">To update to the latest version of the SDK and runtime, run the following commands (deselect the SDKs that you don't want):</span></span>

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp
```
<span data-ttu-id="7177b-170">Om du vill uppdatera Java SDK-binärfilerna från Maven måste du uppdatera versionsinformationen för motsvarande binärfil i ``build.gradle``-filen så att den pekar på den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="7177b-170">To update the Java SDK binaries from Maven, you need to update the version details of the corresponding binary in the ``build.gradle`` file to point to the latest version.</span></span> <span data-ttu-id="7177b-171">Om du vill veta exakt var du behöver uppdatera versionen kan du titta i någon av ``build.gradle``-filerna i komma igång-exemplen för Service Fabric [här](https://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="7177b-171">To know exactly where you need to update the version, you can refer to any ``build.gradle`` file in Service Fabric getting-started samples [here](https://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>

> [!NOTE]
> <span data-ttu-id="7177b-172">När du uppdaterar paketen ovan kan ditt lokala miljökluster stoppas.</span><span class="sxs-lookup"><span data-stu-id="7177b-172">Updating the packages might cause your local development cluster to stop running.</span></span> <span data-ttu-id="7177b-173">Starta om ditt lokala kluster efter en uppgradering genom att följa instruktionerna på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="7177b-173">Restart your local cluster after an upgrade by following the instructions on this page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7177b-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7177b-174">Next steps</span></span>

* [<span data-ttu-id="7177b-175">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med hjälp av Yeoman</span><span class="sxs-lookup"><span data-stu-id="7177b-175">Create and deploy your first Service Fabric Java application on Linux by using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="7177b-176">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med Service Fabric-plugin-programmet för Eclipse</span><span class="sxs-lookup"><span data-stu-id="7177b-176">Create and deploy your first Service Fabric Java application on Linux by using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="7177b-177">Skapa ditt första CSharp-program i Linux</span><span class="sxs-lookup"><span data-stu-id="7177b-177">Create your first CSharp application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
* [<span data-ttu-id="7177b-178">Förbereda utvecklingsmiljön i OSX</span><span class="sxs-lookup"><span data-stu-id="7177b-178">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="7177b-179">Använd Service Fabric CLI för att hantera dina program</span><span class="sxs-lookup"><span data-stu-id="7177b-179">Use the Service Fabric CLI to manage your applications</span></span>](service-fabric-application-lifecycle-sfctl.md)
* [<span data-ttu-id="7177b-180">Skillnader mellan Service Fabric i Windows och Linux</span><span class="sxs-lookup"><span data-stu-id="7177b-180">Service Fabric Windows/Linux differences</span></span>](service-fabric-linux-windows-differences.md)
* [<span data-ttu-id="7177b-181">Kom igång med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="7177b-181">Get started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
