---
title: "aaaCreate din första Azure mikrotjänster app i Linux med C# | Microsoft Docs"
description: Skapa och distribuera ett Service Fabric-program med C#
services: service-fabric
documentationcenter: csharp
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 5a96d21d-fa4a-4dc2-abe8-a830a3482fb1
ms.service: service-fabric
ms.devlang: csharp
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/21/2017
ms.author: subramar
ms.openlocfilehash: 68d685e130be338ebcdb2f1af24b66d1e14f580a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-azure-service-fabric-application"></a><span data-ttu-id="713ef-103">Skapa ditt första Azure Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="713ef-103">Create your first Azure Service Fabric application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="713ef-104">C# – Windows</span><span class="sxs-lookup"><span data-stu-id="713ef-104">C# - Windows</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
> * [<span data-ttu-id="713ef-105">Java – Linux</span><span class="sxs-lookup"><span data-stu-id="713ef-105">Java - Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
> * [<span data-ttu-id="713ef-106">C# – Linux</span><span class="sxs-lookup"><span data-stu-id="713ef-106">C# - Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

<span data-ttu-id="713ef-107">Service Fabric innehåller SDK:er för att skapa tjänster i Linux i både .NET Core och Java.</span><span class="sxs-lookup"><span data-stu-id="713ef-107">Service Fabric provides SDKs for building services on Linux in both .NET Core and Java.</span></span> <span data-ttu-id="713ef-108">I den här självstudiekursen kommer vi titta på hur toocreate ett program för Linux och skapa en tjänst med C# (.NET Core).</span><span class="sxs-lookup"><span data-stu-id="713ef-108">In this tutorial, we look at how toocreate an application for Linux and build a service using C# (.NET Core).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="713ef-109">Krav</span><span class="sxs-lookup"><span data-stu-id="713ef-109">Prerequisites</span></span>
<span data-ttu-id="713ef-110">Du måste [konfigurera Linux-utvecklingsmiljön](service-fabric-get-started-linux.md) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="713ef-110">Before you get started, make sure that you have [set up your Linux development environment](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="713ef-111">Om du använder Mac OS X kan du [konfigurera en Linux-miljö på en virtuell dator med hjälp av Vagrant](service-fabric-get-started-mac.md).</span><span class="sxs-lookup"><span data-stu-id="713ef-111">If you are using Mac OS X, you can [set up a Linux one-box environment in a virtual machine using Vagrant](service-fabric-get-started-mac.md).</span></span>

<span data-ttu-id="713ef-112">Vill du även tooinstall hello [Service Fabric CLI](service-fabric-cli.md)</span><span class="sxs-lookup"><span data-stu-id="713ef-112">You will also want tooinstall hello [Service Fabric CLI](service-fabric-cli.md)</span></span>

### <a name="install-and-set-up-hello-generators-for-csharp"></a><span data-ttu-id="713ef-113">Installera och konfigurera hello generatorer för CSharp</span><span class="sxs-lookup"><span data-stu-id="713ef-113">Install and set up hello generators for CSharp</span></span>
<span data-ttu-id="713ef-114">Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa ett Service Fabric CSharp-program från terminalen med en Yeoman-mallgenerator.</span><span class="sxs-lookup"><span data-stu-id="713ef-114">Service Fabric provides scaffolding tools which will help you create a Service Fabric CSharp application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="713ef-115">Följ hello stegen nedan tooensure som du har hello Service Fabric yeoman mall generator för CSharp som arbetar på din dator.</span><span class="sxs-lookup"><span data-stu-id="713ef-115">Please follow hello steps below tooensure you have hello Service Fabric yeoman template generator for CSharp working on your machine.</span></span>
1. <span data-ttu-id="713ef-116">Installera nodejs och NPM på datorn</span><span class="sxs-lookup"><span data-stu-id="713ef-116">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="713ef-117">Installera [Yeoman](http://yeoman.io/)-mallgeneratorn på datorn från NPM</span><span class="sxs-lookup"><span data-stu-id="713ef-117">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="713ef-118">Installera hello Service Fabric Yeo Java-program generator från NPM</span><span class="sxs-lookup"><span data-stu-id="713ef-118">Install hello Service Fabric Yeo Java application generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfcsharp
  ```

## <a name="create-hello-application"></a><span data-ttu-id="713ef-119">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="713ef-119">Create hello application</span></span>
<span data-ttu-id="713ef-120">Ett Service Fabric-program kan innehålla en eller flera tjänster med en viss roll i att leverera hello-funktionerna.</span><span class="sxs-lookup"><span data-stu-id="713ef-120">A Service Fabric application can contain one or more services, each with a specific role in delivering hello application's functionality.</span></span> <span data-ttu-id="713ef-121">hello Service Fabric [Yeoman](http://yeoman.io/) generatorn för CSharp som du installerade i sista steget, gör det enkelt toocreate din första tjänst och tooadd mer senare.</span><span class="sxs-lookup"><span data-stu-id="713ef-121">hello Service Fabric [Yeoman](http://yeoman.io/) generator for CSharp, which you installed in last step, makes it easy toocreate your first service and tooadd more later.</span></span> <span data-ttu-id="713ef-122">Nu ska vi använda Yeoman toocreate ett program med en enskild tjänst.</span><span class="sxs-lookup"><span data-stu-id="713ef-122">Let's use Yeoman toocreate an application with a single service.</span></span>

1. <span data-ttu-id="713ef-123">Skriv hello efter kommandot toostart skapa hello scaffold-teknik i en terminal:`yo azuresfcsharp`</span><span class="sxs-lookup"><span data-stu-id="713ef-123">In a terminal, type hello following command toostart building hello scaffolding: `yo azuresfcsharp`</span></span>
2. <span data-ttu-id="713ef-124">Namnge ditt program.</span><span class="sxs-lookup"><span data-stu-id="713ef-124">Name your application.</span></span>
3. <span data-ttu-id="713ef-125">Välj hello typ av din första tjänst och ge den namnet.</span><span class="sxs-lookup"><span data-stu-id="713ef-125">Choose hello type of your first service and name it.</span></span> <span data-ttu-id="713ef-126">Vi väljer en tillförlitlig tjänst för aktören hello enligt den här kursen.</span><span class="sxs-lookup"><span data-stu-id="713ef-126">For hello purposes of this tutorial, we choose a Reliable Actor Service.</span></span>

   ![Service Fabric Yeoman-generator för C#][sf-yeoman]

> [!NOTE]
> <span data-ttu-id="713ef-128">Läs mer om alternativen för hello [Service Fabric programming översikt över säkerhetsmodell](service-fabric-choose-framework.md).</span><span class="sxs-lookup"><span data-stu-id="713ef-128">For more information about hello options, see [Service Fabric programming model overview](service-fabric-choose-framework.md).</span></span>
>
>

## <a name="build-hello-application"></a><span data-ttu-id="713ef-129">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="713ef-129">Build hello application</span></span>
<span data-ttu-id="713ef-130">hello Service Fabric Yeoman mallar innehåller ett build-skript som du kan använda toobuild hello-app från hello terminal (efter att navigera toohello programmappen).</span><span class="sxs-lookup"><span data-stu-id="713ef-130">hello Service Fabric Yeoman templates include a build script that you can use toobuild hello app from hello terminal (after navigating toohello application folder).</span></span>

  ```sh
 cd myapp
 ./build.sh
  ```

## <a name="deploy-hello-application"></a><span data-ttu-id="713ef-131">Distribuera programmet hello</span><span class="sxs-lookup"><span data-stu-id="713ef-131">Deploy hello application</span></span>

<span data-ttu-id="713ef-132">När hello programmet är skapat kan distribuera du den toohello lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="713ef-132">Once hello application is built, you can deploy it toohello local cluster.</span></span>

1. <span data-ttu-id="713ef-133">Ansluta toohello lokala Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="713ef-133">Connect toohello local Service Fabric cluster.</span></span>

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. <span data-ttu-id="713ef-134">Kör hello installationsskriptet hello mallen toocopy programmet hello paketet toohello klustret avbildningsarkivet, registrera hello programtyp och skapa en instans av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="713ef-134">Run hello install script provided in hello template toocopy hello application package toohello cluster's image store, register hello application type, and create an instance of hello application.</span></span>

    ```bash
    ./install.sh
    ```

<span data-ttu-id="713ef-135">Distribuera hello inbyggda program är hello samma som andra Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="713ef-135">Deploying hello built application is hello same as any other Service Fabric application.</span></span> <span data-ttu-id="713ef-136">Hello-dokumentationen på [hantera ett Service Fabric-program med hello Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) detaljerade anvisningar.</span><span class="sxs-lookup"><span data-stu-id="713ef-136">See hello documentation on [managing a Service Fabric application with hello Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) for detailed instructions.</span></span>

<span data-ttu-id="713ef-137">Parametrarna toothese kommandon finns i manifest hello genereras inuti hello programpaket.</span><span class="sxs-lookup"><span data-stu-id="713ef-137">Parameters toothese commands can be found in hello generated manifests inside hello application package.</span></span>

<span data-ttu-id="713ef-138">När programmet hello har distribuerats, öppna en webbläsare och gå till [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) på [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="713ef-138">Once hello application has been deployed, open a browser and navigate to [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) at [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="713ef-139">Expandera sedan hello **program** nod och Observera att det finns en post för din typ av program och en annan för hello första instansen av den typen.</span><span class="sxs-lookup"><span data-stu-id="713ef-139">Then, expand hello **Applications** node and note that there is now an entry for your application type and another for hello first instance of that type.</span></span>

## <a name="start-hello-test-client-and-perform-a-failover"></a><span data-ttu-id="713ef-140">Starta hello test-klienten och utför en växling vid fel</span><span class="sxs-lookup"><span data-stu-id="713ef-140">Start hello test client and perform a failover</span></span>
<span data-ttu-id="713ef-141">Aktörsprojekt gör ingenting på egen hand.</span><span class="sxs-lookup"><span data-stu-id="713ef-141">Actor projects do not do anything on their own.</span></span> <span data-ttu-id="713ef-142">De kräver en annan tjänst eller klient toosend dem meddelanden.</span><span class="sxs-lookup"><span data-stu-id="713ef-142">They require another service or client toosend them messages.</span></span> <span data-ttu-id="713ef-143">hello aktören mallen innehåller ett enkelt testskript som du kan använda toointeract med hello aktören-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="713ef-143">hello actor template includes a simple test script that you can use toointeract with hello actor service.</span></span>

1. <span data-ttu-id="713ef-144">Köra hello skript med hello titta på verktyget toosee hello utdata från hello aktören service.</span><span class="sxs-lookup"><span data-stu-id="713ef-144">Run hello script using hello watch utility toosee hello output of hello actor service.</span></span>

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. <span data-ttu-id="713ef-145">Leta upp nod som värd för hello primära repliken för hello aktören tjänsten i Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="713ef-145">In Service Fabric Explorer, locate node hosting hello primary replica for hello actor service.</span></span> <span data-ttu-id="713ef-146">I hello skärmbilden nedan är noden 3.</span><span class="sxs-lookup"><span data-stu-id="713ef-146">In hello screenshot below, it is node 3.</span></span>

    ![Hitta hello primära repliken i Service Fabric Explorer][sfx-primary]
3. <span data-ttu-id="713ef-148">Klicka på hello nod du hittades i hello föregående steg och sedan välja **inaktivera (omstart)** hello Åtgärder-menyn.</span><span class="sxs-lookup"><span data-stu-id="713ef-148">Click hello node you found in hello previous step, then select **Deactivate (restart)** from hello Actions menu.</span></span> <span data-ttu-id="713ef-149">Den här åtgärden startar om en nod i klustret lokala för att tvinga en växling vid fel tooa sekundär replik körs på en annan nod.</span><span class="sxs-lookup"><span data-stu-id="713ef-149">This action restarts one node in your local cluster forcing a failover tooa secondary replica running on another node.</span></span> <span data-ttu-id="713ef-150">När du utför den här åtgärden betala uppmärksamhet toohello utdata från hello testklienten och Observera hello räknaren fortsätter tooincrement trots hello växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="713ef-150">As you perform this action, pay attention toohello output from hello test client and note that hello counter continues tooincrement despite hello failover.</span></span>

## <a name="adding-more-services-tooan-existing-application"></a><span data-ttu-id="713ef-151">Lägga till fler tjänster tooan befintliga program</span><span class="sxs-lookup"><span data-stu-id="713ef-151">Adding more services tooan existing application</span></span>

<span data-ttu-id="713ef-152">tooadd en annan tooan tjänstprogrammet redan skapats med hjälp av `yo`, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="713ef-152">tooadd another service tooan application already created using `yo`, perform hello following steps:</span></span>
1. <span data-ttu-id="713ef-153">Ändra toohello rotkatalog till hello befintliga program.</span><span class="sxs-lookup"><span data-stu-id="713ef-153">Change directory toohello root of hello existing application.</span></span>  <span data-ttu-id="713ef-154">Till exempel `cd ~/YeomanSamples/MyApplication`om `MyApplication` är hello-program som skapats av Yeoman.</span><span class="sxs-lookup"><span data-stu-id="713ef-154">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is hello application created by Yeoman.</span></span>
2. <span data-ttu-id="713ef-155">Kör `yo azuresfcsharp:AddService`</span><span class="sxs-lookup"><span data-stu-id="713ef-155">Run `yo azuresfcsharp:AddService`</span></span>

## <a name="migrating-from-projectjson-toocsproj"></a><span data-ttu-id="713ef-156">Migrera från project.json too.csproj</span><span class="sxs-lookup"><span data-stu-id="713ef-156">Migrating from project.json too.csproj</span></span>
1. <span data-ttu-id="713ef-157">Kör 'dotnet migrera' i projektets rotkatalog kommer att migrera alla hello project.json toocsproj format.</span><span class="sxs-lookup"><span data-stu-id="713ef-157">Running 'dotnet migrate' in project root directory will migrate all hello project.json toocsproj format.</span></span>
2. <span data-ttu-id="713ef-158">Uppdatera hello projekt refererar till detta toocsproj filer i projektfiler.</span><span class="sxs-lookup"><span data-stu-id="713ef-158">Update hello project references accordingly toocsproj files in project files.</span></span>
3. <span data-ttu-id="713ef-159">Uppdatera hello projektet namn toocsproj-filer i build.sh.</span><span class="sxs-lookup"><span data-stu-id="713ef-159">Update hello project file names toocsproj files in build.sh.</span></span>

## <a name="next-steps"></a><span data-ttu-id="713ef-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="713ef-160">Next steps</span></span>

* [<span data-ttu-id="713ef-161">Läs mer om Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="713ef-161">Learn more about Reliable Actors</span></span>](service-fabric-reliable-actors-introduction.md)
* [<span data-ttu-id="713ef-162">Interaktion med Service Fabric-kluster med hello Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="713ef-162">Interacting with Service Fabric clusters using hello Service Fabric CLI</span></span>](service-fabric-cli.md)
* <span data-ttu-id="713ef-163">Lär dig mer om [Service Fabric-supportalternativen](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="713ef-163">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>
* [<span data-ttu-id="713ef-164">Kom igång med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="713ef-164">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
