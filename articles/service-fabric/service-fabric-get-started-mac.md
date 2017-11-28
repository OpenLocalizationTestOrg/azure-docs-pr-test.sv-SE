---
title: "Konfigurera en utvecklingsmiljö i Mac OS X så att den fungerar med Azure Service Fabric | Microsoft Docs"
description: "Installera runtime, SDK och verktyg och skapa ett lokalt utvecklingskluster. När du har slutfört den här installationen är du redo att börja bygga program i Mac OS X."
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
ms.openlocfilehash: 8b4fc0ab9034263418cac42ced203035e0a8fcad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a><span data-ttu-id="29d2d-104">Konfigurera din utvecklingsmiljö i Mac OS X</span><span class="sxs-lookup"><span data-stu-id="29d2d-104">Set up your development environment on Mac OS X</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="29d2d-105">Windows</span><span class="sxs-lookup"><span data-stu-id="29d2d-105">Windows</span></span>](service-fabric-get-started.md)
> * [<span data-ttu-id="29d2d-106">Linux</span><span class="sxs-lookup"><span data-stu-id="29d2d-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="29d2d-107">OSX</span><span class="sxs-lookup"><span data-stu-id="29d2d-107">OSX</span></span>](service-fabric-get-started-mac.md)
>
>  

<span data-ttu-id="29d2d-108">Du kan skapa Service Fabric-program som körs i Linux-kluster i Mac OS X. Den här artikeln visar hur du konfigurerar din utvecklingsmiljö i Mac.</span><span class="sxs-lookup"><span data-stu-id="29d2d-108">You can build Service Fabric applications to run on Linux clusters using Mac OS X. This article covers how to set up your Mac for development.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29d2d-109">Krav</span><span class="sxs-lookup"><span data-stu-id="29d2d-109">Prerequisites</span></span>
<span data-ttu-id="29d2d-110">Service Fabric kan inte köras internt i OS X. För att du ska kunna köra ett lokalt Service Fabric-kluster tillhandahåller vi en förkonfigurerad virtuell Ubuntu-dator med Vagrant och VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="29d2d-110">Service Fabric does not run natively on OS X. To run a local Service Fabric cluster, we provide a pre-configured Ubuntu virtual machine using Vagrant and VirtualBox.</span></span> <span data-ttu-id="29d2d-111">Innan du börjar behöver du:</span><span class="sxs-lookup"><span data-stu-id="29d2d-111">Before you get started, you need:</span></span>

* [<span data-ttu-id="29d2d-112">Vagrant (v1.8.4 eller senare)</span><span class="sxs-lookup"><span data-stu-id="29d2d-112">Vagrant (v1.8.4 or later)</span></span>](http://www.vagrantup.com/downloads.html)
* [<span data-ttu-id="29d2d-113">VirtualBox</span><span class="sxs-lookup"><span data-stu-id="29d2d-113">VirtualBox</span></span>](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> <span data-ttu-id="29d2d-114">Du behöver använda versioner som både stöds av Vagrant och VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="29d2d-114">You need to use mutually supported versions of Vagrant and VirtualBox.</span></span> <span data-ttu-id="29d2d-115">Vagrant kan bete sig oförutsägbart med en VirtualBox-version som inte stöds.</span><span class="sxs-lookup"><span data-stu-id="29d2d-115">Vagrant might behave erratically on an unsupported VirtualBox version.</span></span>
>

## <a name="create-the-local-vm"></a><span data-ttu-id="29d2d-116">Skapa den lokala virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="29d2d-116">Create the local VM</span></span>
<span data-ttu-id="29d2d-117">För att skapa den lokala virtuella datorn med ett 5-nods Service Fabric-kluster utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="29d2d-117">To create the local VM containing a 5-node Service Fabric cluster, perform the following steps:</span></span>

1. <span data-ttu-id="29d2d-118">Klona `Vagrantfile`-databasen</span><span class="sxs-lookup"><span data-stu-id="29d2d-118">Clone the `Vagrantfile` repo</span></span>

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    <span data-ttu-id="29d2d-119">Det här steget hämtar filen `Vagrantfile` som innehåller VM-konfigurationen tillsammans med den plats som den virtuella datorn laddas ned från.</span><span class="sxs-lookup"><span data-stu-id="29d2d-119">This steps bring downs the file `Vagrantfile` containing the VM configuration along with the location the VM is downloaded from.</span></span>

2. <span data-ttu-id="29d2d-120">Navigera till den lokala repoklonen</span><span class="sxs-lookup"><span data-stu-id="29d2d-120">Navigate to the local clone of the repo</span></span>

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. <span data-ttu-id="29d2d-121">(Valfritt) Ändra standardinställningarna för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="29d2d-121">(Optional) Modify the default VM settings</span></span>

    <span data-ttu-id="29d2d-122">Som standard konfigureras den lokala virtuella datorn på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="29d2d-122">By default, the local VM is configured as follows:</span></span>

   * <span data-ttu-id="29d2d-123">3 GB minne allokeras</span><span class="sxs-lookup"><span data-stu-id="29d2d-123">3 GB of memory allocated</span></span>
   * <span data-ttu-id="29d2d-124">Det privata värdnätverket konfigureras på IP 192.168.50.50 för att möjliggöra genomströmning av trafik från Mac-värden</span><span class="sxs-lookup"><span data-stu-id="29d2d-124">Private host network configured at IP 192.168.50.50 enabling passthrough of traffic from the Mac host</span></span>

     <span data-ttu-id="29d2d-125">Du kan ändra dessa inställningar eller lägga till en annan konfiguration för den virtuella datorn i `Vagrantfile`.</span><span class="sxs-lookup"><span data-stu-id="29d2d-125">You can change either of these settings or add other configuration to the VM in the `Vagrantfile`.</span></span> <span data-ttu-id="29d2d-126">En fullständig lista över konfigurationsalternativen finns i [Vagrant-dokumentationen](http://www.vagrantup.com/docs).</span><span class="sxs-lookup"><span data-stu-id="29d2d-126">See the [Vagrant documentation](http://www.vagrantup.com/docs) for the full list of configuration options.</span></span>
4. <span data-ttu-id="29d2d-127">Skapa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="29d2d-127">Create the VM</span></span>

    ```bash
    vagrant up
    ```

   <span data-ttu-id="29d2d-128">I det här steget laddas den förkonfigurerade VM-avbildningen ned. Avbildningen startas lokalt och ett lokalt Service Fabric-kluster konfigureras sedan på datorn.</span><span class="sxs-lookup"><span data-stu-id="29d2d-128">This step downloads the preconfigured VM image, boot it locally, and then set up a local Service Fabric cluster in it.</span></span> <span data-ttu-id="29d2d-129">Det här kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="29d2d-129">You should expect it to take a few minutes.</span></span> <span data-ttu-id="29d2d-130">Om installationen lyckas, får du upp ett meddelande som indikerar att klustret startas.</span><span class="sxs-lookup"><span data-stu-id="29d2d-130">If setup completes successfully, you see a message in the output indicating that the cluster is starting up.</span></span>

    ![Klusterinstallationen startar efter att den virtuella datorn har etablerats][cluster-setup-script]

    >[!TIP]
    > <span data-ttu-id="29d2d-132">Om nedladdningen av den virtuella datorn tar lång tid kan du hämta den med hjälp av wget eller curl eller via en webbläsare genom att gå till länken som anges i **config.vm.box_url** i filen `Vagrantfile`.</span><span class="sxs-lookup"><span data-stu-id="29d2d-132">If the VM download is taking a long time, you can download it using wget or curl or through a browser by navigating to the link specified by **config.vm.box_url** in the file `Vagrantfile`.</span></span> <span data-ttu-id="29d2d-133">När du hämtat den lokalt redigerar du `Vagrantfile` så att den pekar på den lokala sökväg som du laddade ned avbildningen till.</span><span class="sxs-lookup"><span data-stu-id="29d2d-133">After downloading it locally, edit `Vagrantfile` to point to the local path where you downloaded the image.</span></span> <span data-ttu-id="29d2d-134">Om du till exempel laddade ned avbildningen till /home/users/test/azureservicefabric.tp8.box anger du **config.vm.box_url** till den sökvägen.</span><span class="sxs-lookup"><span data-stu-id="29d2d-134">For example if you downloaded the image to /home/users/test/azureservicefabric.tp8.box, then set **config.vm.box_url** to that path.</span></span>
    >

5. <span data-ttu-id="29d2d-135">Testa att klustret är korrekt installerat genom att gå till Service Fabric Explorer på http://192.168.50.50:19080/Explorer (förutsatt att du har behållit standard-IP för det privata nätverket).</span><span class="sxs-lookup"><span data-stu-id="29d2d-135">Test that the cluster has been set up correctly by navigating to Service Fabric Explorer at http://192.168.50.50:19080/Explorer (assuming you kept the default private network IP).</span></span>

    ![Service Fabric Explorer på Mac-värddatorn][sfx-mac]


## <a name="create-application-on-mac-using-yeoman"></a><span data-ttu-id="29d2d-137">Skapa program på Mac med Yeoman</span><span class="sxs-lookup"><span data-stu-id="29d2d-137">Create application on Mac using Yeoman</span></span>
<span data-ttu-id="29d2d-138">Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa ett Service Fabric-program från terminalen med en Yeoman-mallgenerator.</span><span class="sxs-lookup"><span data-stu-id="29d2d-138">Service Fabric provides scaffolding tools which will help you create a Service Fabric application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="29d2d-139">Följ stegen nedan för att se till att du har Service Fabric Yeoman-mallgeneratorn på datorn.</span><span class="sxs-lookup"><span data-stu-id="29d2d-139">Please follow the steps below to ensure you have the Service Fabric yeoman template generator working on your machine.</span></span>

1. <span data-ttu-id="29d2d-140">Du måste ha Node.js och NPM installerade på din Mac.</span><span class="sxs-lookup"><span data-stu-id="29d2d-140">You need to have Node.js and NPM installed on you mac.</span></span> <span data-ttu-id="29d2d-141">Annars kan du installera Node.js och NPM med Homebrew enligt följande.</span><span class="sxs-lookup"><span data-stu-id="29d2d-141">If not you can install Node.js and NPM using Homebrew using the following.</span></span> <span data-ttu-id="29d2d-142">Om du vill kontrollera vilka versioner av Node.js och NPM som är installerade på din Mac kan du använda alternativet ``-v``.</span><span class="sxs-lookup"><span data-stu-id="29d2d-142">To check the versions of Node.js and NPM installed on your Mac, you can use the ``-v`` option.</span></span>

  ```bash
  brew install node
  node -v
  npm -v
  ```
2. <span data-ttu-id="29d2d-143">Installera [Yeoman](http://yeoman.io/)-mallgeneratorn på datorn från NPM</span><span class="sxs-lookup"><span data-stu-id="29d2d-143">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  npm install -g yo
  ```
3. <span data-ttu-id="29d2d-144">Installera den Yeoman-generator som du vill använda enligt instruktionerna i [dokumentationen](service-fabric-get-started-linux.md) för att komma igång.</span><span class="sxs-lookup"><span data-stu-id="29d2d-144">Install the Yeoman generator you want to use, following the steps in the getting started [documentation](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="29d2d-145">Om du vill skapa Service Fabric-program med hjälp av Yeoman följer du stegen -</span><span class="sxs-lookup"><span data-stu-id="29d2d-145">To create Service Fabric Applications using Yeoman, follow the steps -</span></span>

  ```bash
  npm install -g generator-azuresfjava       # for Service Fabric Java Applications
  npm install -g generator-azuresfguest      # for Service Fabric Guest executables
  npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
  ```
4. <span data-ttu-id="29d2d-146">Om du vill skapa ett Service Fabric Java-program på Mac måste du ha JDK 1.8 och Gradle installerade på datorn.</span><span class="sxs-lookup"><span data-stu-id="29d2d-146">To build a Service Fabric Java application on Mac, you would need - JDK 1.8 and Gradle installed on the machine.</span></span>


## <a name="install-the-service-fabric-plugin-for-eclipse-neon"></a><span data-ttu-id="29d2d-147">Installera Service Fabric-plugin-programmet för Eclipse Neon</span><span class="sxs-lookup"><span data-stu-id="29d2d-147">Install the Service Fabric plugin for Eclipse Neon</span></span>

<span data-ttu-id="29d2d-148">Service Fabric innehåller ett plugin-program för **Eclipse Neon för Java IDE** som förenklar processen med att skapa, bygga och distribuera Java-tjänster.</span><span class="sxs-lookup"><span data-stu-id="29d2d-148">Service Fabric provides a plugin for the **Eclipse Neon for Java IDE** that can simplify the process of creating, building, and deploying Java services.</span></span> <span data-ttu-id="29d2d-149">Du kan följa installationsstegen i den här allmänna [dokumentationen](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) som beskriver hur du installerar eller uppdaterar Service Fabric Eclipse-plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="29d2d-149">You can follow the installation steps mentioned in this general [documentation](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) about installing or updating Service Fabric Eclipse plugin.</span></span>

>[!TIP]
> <span data-ttu-id="29d2d-150">Som standard stöds standard-IP-adressen som anges i ``Vagrantfile`` i ``Local.json`` för det genererade programmet.</span><span class="sxs-lookup"><span data-stu-id="29d2d-150">By default we support the default IP as mentioned in the ``Vagrantfile`` in the ``Local.json`` of the generated application.</span></span> <span data-ttu-id="29d2d-151">Om du ändrar den och distribuerar Vagrant med en annan IP-adress ska du även uppdatera motsvarande IP-adress i ``Local.json`` för programmet.</span><span class="sxs-lookup"><span data-stu-id="29d2d-151">In case you change that and deploy Vagrant with a different IP, please update the corresponding IP in ``Local.json`` of your application as well.</span></span>

## <a name="next-steps"></a><span data-ttu-id="29d2d-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29d2d-152">Next steps</span></span>
<!-- Links -->
* [<span data-ttu-id="29d2d-153">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med hjälp av Yeoman</span><span class="sxs-lookup"><span data-stu-id="29d2d-153">Create and deploy your first Service Fabric Java application on Linux using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="29d2d-154">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med Service Fabric-plugin-programmet för Eclipse</span><span class="sxs-lookup"><span data-stu-id="29d2d-154">Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="29d2d-155">Skapa ett Service Fabric-kluster i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="29d2d-155">Create a Service Fabric cluster in the Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* [<span data-ttu-id="29d2d-156">Skapa ett Service Fabric-kluster med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="29d2d-156">Create a Service Fabric cluster using the Azure Resource Manager</span></span>](service-fabric-cluster-creation-via-arm.md)
* [<span data-ttu-id="29d2d-157">Förstå Service Fabric-programmodellen</span><span class="sxs-lookup"><span data-stu-id="29d2d-157">Understand the Service Fabric application model</span></span>](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
