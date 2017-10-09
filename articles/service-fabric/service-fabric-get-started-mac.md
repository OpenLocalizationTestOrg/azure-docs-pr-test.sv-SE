---
title: "aaaSet in din utvecklingsmiljö för Mac OS X toowork med Azure Service Fabric | Microsoft Docs"
description: "Installera hello runtime, verktyg och SDK och skapa ett kluster för lokal utveckling. När du har slutfört den här installationen kommer du att redo toobuild program på Mac OS X."
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
ms.date: 08/21/2017
ms.author: saysa
ms.openlocfilehash: 0b8a6c1fc1871fa76f3e21cefbc7f66f79072797
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a><span data-ttu-id="02340-104">Konfigurera din utvecklingsmiljö i Mac OS X</span><span class="sxs-lookup"><span data-stu-id="02340-104">Set up your development environment on Mac OS X</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="02340-105">Windows</span><span class="sxs-lookup"><span data-stu-id="02340-105">Windows</span></span>](service-fabric-get-started.md)
> * [<span data-ttu-id="02340-106">Linux</span><span class="sxs-lookup"><span data-stu-id="02340-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="02340-107">OSX</span><span class="sxs-lookup"><span data-stu-id="02340-107">OSX</span></span>](service-fabric-get-started-mac.md)
>
>  

<span data-ttu-id="02340-108">Du kan skapa toorun för Service Fabric-program på Linux-kluster med Mac OS X. Den här artikeln beskriver hur tooset upp din Mac för utveckling.</span><span class="sxs-lookup"><span data-stu-id="02340-108">You can build Service Fabric applications toorun on Linux clusters using Mac OS X. This article covers how tooset up your Mac for development.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="02340-109">Krav</span><span class="sxs-lookup"><span data-stu-id="02340-109">Prerequisites</span></span>
<span data-ttu-id="02340-110">Service Fabric kan inte köras internt i OS X. toorun lokala Service Fabric-kluster, vi erbjuder en förkonfigurerad Ubuntu virtuell dator med Vagrant och VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="02340-110">Service Fabric does not run natively on OS X. toorun a local Service Fabric cluster, we provide a pre-configured Ubuntu virtual machine using Vagrant and VirtualBox.</span></span> <span data-ttu-id="02340-111">Innan du börjar behöver du:</span><span class="sxs-lookup"><span data-stu-id="02340-111">Before you get started, you need:</span></span>

* [<span data-ttu-id="02340-112">Vagrant (v1.8.4 eller senare)</span><span class="sxs-lookup"><span data-stu-id="02340-112">Vagrant (v1.8.4 or later)</span></span>](http://www.vagrantup.com/downloads.html)
* [<span data-ttu-id="02340-113">VirtualBox</span><span class="sxs-lookup"><span data-stu-id="02340-113">VirtualBox</span></span>](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> <span data-ttu-id="02340-114">Du måste toouse ömsesidigt stöds versioner av Vagrant och VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="02340-114">You need toouse mutually supported versions of Vagrant and VirtualBox.</span></span> <span data-ttu-id="02340-115">Vagrant kan bete sig oförutsägbart med en VirtualBox-version som inte stöds.</span><span class="sxs-lookup"><span data-stu-id="02340-115">Vagrant might behave erratically on an unsupported VirtualBox version.</span></span>
>

## <a name="create-hello-local-vm"></a><span data-ttu-id="02340-116">Skapa hello lokala VM</span><span class="sxs-lookup"><span data-stu-id="02340-116">Create hello local VM</span></span>
<span data-ttu-id="02340-117">toocreate Hej lokala VM som innehåller ett Service Fabric-kluster med 5 noder, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="02340-117">toocreate hello local VM containing a 5-node Service Fabric cluster, perform hello following steps:</span></span>

1. <span data-ttu-id="02340-118">Klona hello `Vagrantfile` lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="02340-118">Clone hello `Vagrantfile` repo</span></span>

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    <span data-ttu-id="02340-119">Ta med det här steget används hello filen `Vagrantfile` som innehåller hello VM configuration tillsammans med hello plats hello VM hämtas från.</span><span class="sxs-lookup"><span data-stu-id="02340-119">This steps bring downs hello file `Vagrantfile` containing hello VM configuration along with hello location hello VM is downloaded from.</span></span>

2. <span data-ttu-id="02340-120">Navigera toohello lokala kloning av hello lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="02340-120">Navigate toohello local clone of hello repo</span></span>

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. <span data-ttu-id="02340-121">(Valfritt) Ändra hello standardinställningarna för VM</span><span class="sxs-lookup"><span data-stu-id="02340-121">(Optional) Modify hello default VM settings</span></span>

    <span data-ttu-id="02340-122">Som standard konfigureras hello lokala VM på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="02340-122">By default, hello local VM is configured as follows:</span></span>

   * <span data-ttu-id="02340-123">3 GB minne allokeras</span><span class="sxs-lookup"><span data-stu-id="02340-123">3 GB of memory allocated</span></span>
   * <span data-ttu-id="02340-124">Privata värden datornätverk IP-192.168.50.50 aktiverar genomströmning av trafik från hello Mac-värden</span><span class="sxs-lookup"><span data-stu-id="02340-124">Private host network configured at IP 192.168.50.50 enabling passthrough of traffic from hello Mac host</span></span>

     <span data-ttu-id="02340-125">Du kan ändra dessa inställningar eller lägga till andra configuration toohello VM i hello `Vagrantfile`.</span><span class="sxs-lookup"><span data-stu-id="02340-125">You can change either of these settings or add other configuration toohello VM in hello `Vagrantfile`.</span></span> <span data-ttu-id="02340-126">Se hello [Vagrant dokumentationen](http://www.vagrantup.com/docs) hello fullständig lista över konfigurationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="02340-126">See hello [Vagrant documentation](http://www.vagrantup.com/docs) for hello full list of configuration options.</span></span>
4. <span data-ttu-id="02340-127">Skapa hello VM</span><span class="sxs-lookup"><span data-stu-id="02340-127">Create hello VM</span></span>

    ```bash
    vagrant up
    ```

   <span data-ttu-id="02340-128">Det här steget hämtar hello förkonfigurerade VM-avbildning, starta den lokalt, och sedan skapa en lokal Service Fabric-klustret i den.</span><span class="sxs-lookup"><span data-stu-id="02340-128">This step downloads hello preconfigured VM image, boot it locally, and then set up a local Service Fabric cluster in it.</span></span> <span data-ttu-id="02340-129">Du bör den tootake några minuter.</span><span class="sxs-lookup"><span data-stu-id="02340-129">You should expect it tootake a few minutes.</span></span> <span data-ttu-id="02340-130">Om installationen är klar visas ett meddelande i hello utdata som visar hello klustret startas.</span><span class="sxs-lookup"><span data-stu-id="02340-130">If setup completes successfully, you see a message in hello output indicating that hello cluster is starting up.</span></span>

    ![Klusterinstallationen startar efter att den virtuella datorn har etablerats][cluster-setup-script]

    >[!TIP]
    > <span data-ttu-id="02340-132">Om hello VM download tar lång tid kan du hämta den med hjälp av wget eller curl eller via en webbläsare genom att gå toohello länk som anges av **config.vm.box_url** i hello filen `Vagrantfile`.</span><span class="sxs-lookup"><span data-stu-id="02340-132">If hello VM download is taking a long time, you can download it using wget or curl or through a browser by navigating toohello link specified by **config.vm.box_url** in hello file `Vagrantfile`.</span></span> <span data-ttu-id="02340-133">När du har hämtat lokalt, redigera `Vagrantfile` toopoint toohello lokal sökväg där du hämtade hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="02340-133">After downloading it locally, edit `Vagrantfile` toopoint toohello local path where you downloaded hello image.</span></span> <span data-ttu-id="02340-134">För exempel om du har hämtat hello avbildningen too/home/users/test/azureservicefabric.tp8.box sedan ange **config.vm.box_url** toothat sökväg.</span><span class="sxs-lookup"><span data-stu-id="02340-134">For example if you downloaded hello image too/home/users/test/azureservicefabric.tp8.box, then set **config.vm.box_url** toothat path.</span></span>
    >

5. <span data-ttu-id="02340-135">Testa att hello klustret har ställts in korrekt genom att gå tooService Fabric-Utforskaren på http://192.168.50.50:19080/Explorer (förutsatt att du behöll hello standard privata nätverks-IP).</span><span class="sxs-lookup"><span data-stu-id="02340-135">Test that hello cluster has been set up correctly by navigating tooService Fabric Explorer at http://192.168.50.50:19080/Explorer (assuming you kept hello default private network IP).</span></span>

    ![Service Fabric Explorer visas från hello värden Mac][sfx-mac]


## <a name="create-application-on-mac-using-yeoman"></a><span data-ttu-id="02340-137">Skapa program på Mac med Yeoman</span><span class="sxs-lookup"><span data-stu-id="02340-137">Create application on Mac using Yeoman</span></span>
<span data-ttu-id="02340-138">Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa ett Service Fabric-program från terminalen med en Yeoman-mallgenerator.</span><span class="sxs-lookup"><span data-stu-id="02340-138">Service Fabric provides scaffolding tools which will help you create a Service Fabric application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="02340-139">Följ hello stegen nedan tooensure som du har hello Service Fabric yeoman mall generator fungerar på din dator.</span><span class="sxs-lookup"><span data-stu-id="02340-139">Please follow hello steps below tooensure you have hello Service Fabric yeoman template generator working on your machine.</span></span>

1. <span data-ttu-id="02340-140">Du behöver toohave Node.js och NPM installerad på du mac.</span><span class="sxs-lookup"><span data-stu-id="02340-140">You need toohave Node.js and NPM installed on you mac.</span></span> <span data-ttu-id="02340-141">Annars kan du installera Node.js och NPM med Homebrew med hello följande.</span><span class="sxs-lookup"><span data-stu-id="02340-141">If not you can install Node.js and NPM using Homebrew using hello following.</span></span> <span data-ttu-id="02340-142">toocheck hello versioner av Node.js och NPM installeras på en Mac, kan du använda hello ``-v`` alternativet.</span><span class="sxs-lookup"><span data-stu-id="02340-142">toocheck hello versions of Node.js and NPM installed on your Mac, you can use hello ``-v`` option.</span></span>

  ```bash
  brew install node
  node -v
  npm -v
  ```
2. <span data-ttu-id="02340-143">Installera [Yeoman](http://yeoman.io/)-mallgeneratorn på datorn från NPM</span><span class="sxs-lookup"><span data-stu-id="02340-143">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  npm install -g yo
  ```
3. <span data-ttu-id="02340-144">Installera hello Yeoman generator önskade toouse, följande hello i hello komma igång [dokumentationen](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="02340-144">Install hello Yeoman generator you want toouse, following hello steps in hello getting started [documentation](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="02340-145">toocreate Service Fabric-program med hjälp av Yeoman, gör hello-</span><span class="sxs-lookup"><span data-stu-id="02340-145">toocreate Service Fabric Applications using Yeoman, follow hello steps -</span></span>

  ```bash
  npm install -g generator-azuresfjava       # for Service Fabric Java Applications
  npm install -g generator-azuresfguest      # for Service Fabric Guest executables
  npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
  ```
4. <span data-ttu-id="02340-146">toobuild ett Service Fabric Java-program på Mac måste - JDK 1.8 och Gradle installeras på hello-dator.</span><span class="sxs-lookup"><span data-stu-id="02340-146">toobuild a Service Fabric Java application on Mac, you would need - JDK 1.8 and Gradle installed on hello machine.</span></span>


## <a name="install-hello-service-fabric-plugin-for-eclipse-neon"></a><span data-ttu-id="02340-147">Installera Service Fabric-plugin-programmet hello för Eclipse Neon</span><span class="sxs-lookup"><span data-stu-id="02340-147">Install hello Service Fabric plugin for Eclipse Neon</span></span>

<span data-ttu-id="02340-148">Service Fabric innehåller ett plugin-program för hello **Eclipse Neon för Java IDE** som kan förenkla hello processen att skapa, skapa och distribuera Java-tjänster.</span><span class="sxs-lookup"><span data-stu-id="02340-148">Service Fabric provides a plugin for hello **Eclipse Neon for Java IDE** that can simplify hello process of creating, building, and deploying Java services.</span></span> <span data-ttu-id="02340-149">Du kan följa hello installationssteg som nämns i detta allmänna [dokumentationen](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) om att installera eller uppdatera Service Fabric Eclipse-plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="02340-149">You can follow hello installation steps mentioned in this general [documentation](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) about installing or updating Service Fabric Eclipse plugin.</span></span>

>[!TIP]
> <span data-ttu-id="02340-150">Som standard stöder vi hello standard IP som anges i hello ``Vagrantfile`` i hello ``Local.json`` av programmet hello genereras.</span><span class="sxs-lookup"><span data-stu-id="02340-150">By default we support hello default IP as mentioned in hello ``Vagrantfile`` in hello ``Local.json`` of hello generated application.</span></span> <span data-ttu-id="02340-151">Om du ändrar som och distribuera Vagrant med en annan IP-adress, uppdatera hello motsvarande IP i ``Local.json`` för din App samt.</span><span class="sxs-lookup"><span data-stu-id="02340-151">In case you change that and deploy Vagrant with a different IP, please update hello corresponding IP in ``Local.json`` of your application as well.</span></span>

## <a name="next-steps"></a><span data-ttu-id="02340-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="02340-152">Next steps</span></span>
<!-- Links -->
* [<span data-ttu-id="02340-153">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med hjälp av Yeoman</span><span class="sxs-lookup"><span data-stu-id="02340-153">Create and deploy your first Service Fabric Java application on Linux using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="02340-154">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med Service Fabric-plugin-programmet för Eclipse</span><span class="sxs-lookup"><span data-stu-id="02340-154">Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="02340-155">Skapa ett Service Fabric-kluster i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="02340-155">Create a Service Fabric cluster in hello Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* [<span data-ttu-id="02340-156">Skapa ett Service Fabric-kluster med hello Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="02340-156">Create a Service Fabric cluster using hello Azure Resource Manager</span></span>](service-fabric-cluster-creation-via-arm.md)
* [<span data-ttu-id="02340-157">Förstå hello programmodell för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="02340-157">Understand hello Service Fabric application model</span></span>](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
