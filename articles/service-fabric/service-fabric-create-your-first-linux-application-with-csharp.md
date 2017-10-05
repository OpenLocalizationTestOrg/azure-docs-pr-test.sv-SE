---
title: "Skapa din första Azure-mikrotjänstapp i Linux med hjälp av C# | Microsoft Docs"
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
ms.openlocfilehash: adcafaa5522fcddc0a01eb1dc8deba04ebfc38f2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-azure-service-fabric-application"></a><span data-ttu-id="17ffc-103">Skapa ditt första Azure Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="17ffc-103">Create your first Azure Service Fabric application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="17ffc-104">C# – Windows</span><span class="sxs-lookup"><span data-stu-id="17ffc-104">C# - Windows</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
> * [<span data-ttu-id="17ffc-105">Java – Linux</span><span class="sxs-lookup"><span data-stu-id="17ffc-105">Java - Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
> * [<span data-ttu-id="17ffc-106">C# – Linux</span><span class="sxs-lookup"><span data-stu-id="17ffc-106">C# - Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

<span data-ttu-id="17ffc-107">Service Fabric innehåller SDK:er för att skapa tjänster i Linux i både .NET Core och Java.</span><span class="sxs-lookup"><span data-stu-id="17ffc-107">Service Fabric provides SDKs for building services on Linux in both .NET Core and Java.</span></span> <span data-ttu-id="17ffc-108">I den här självstudien, visar vi hur man skapar ett program för Linux och en tjänst med C# (.NET Core).</span><span class="sxs-lookup"><span data-stu-id="17ffc-108">In this tutorial, we look at how to create an application for Linux and build a service using C# (.NET Core).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17ffc-109">Krav</span><span class="sxs-lookup"><span data-stu-id="17ffc-109">Prerequisites</span></span>
<span data-ttu-id="17ffc-110">Du måste [konfigurera Linux-utvecklingsmiljön](service-fabric-get-started-linux.md) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="17ffc-110">Before you get started, make sure that you have [set up your Linux development environment](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="17ffc-111">Om du använder Mac OS X kan du [konfigurera en Linux-miljö på en virtuell dator med hjälp av Vagrant](service-fabric-get-started-mac.md).</span><span class="sxs-lookup"><span data-stu-id="17ffc-111">If you are using Mac OS X, you can [set up a Linux one-box environment in a virtual machine using Vagrant](service-fabric-get-started-mac.md).</span></span>

<span data-ttu-id="17ffc-112">Du bör även installera [Service Fabric CLI](service-fabric-cli.md)</span><span class="sxs-lookup"><span data-stu-id="17ffc-112">You will also want to install the [Service Fabric CLI](service-fabric-cli.md)</span></span>

### <a name="install-and-set-up-the-generators-for-csharp"></a><span data-ttu-id="17ffc-113">Installera och konfigurera generatorerna för CSharp</span><span class="sxs-lookup"><span data-stu-id="17ffc-113">Install and set up the generators for CSharp</span></span>
<span data-ttu-id="17ffc-114">Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa ett Service Fabric CSharp-program från terminalen med en Yeoman-mallgenerator.</span><span class="sxs-lookup"><span data-stu-id="17ffc-114">Service Fabric provides scaffolding tools which will help you create a Service Fabric CSharp application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="17ffc-115">Följ stegen nedan för att se till att du har Service Fabric Yeoman-mallgeneratorn för CSharp på datorn.</span><span class="sxs-lookup"><span data-stu-id="17ffc-115">Please follow the steps below to ensure you have the Service Fabric yeoman template generator for CSharp working on your machine.</span></span>
1. <span data-ttu-id="17ffc-116">Installera nodejs och NPM på datorn</span><span class="sxs-lookup"><span data-stu-id="17ffc-116">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="17ffc-117">Installera [Yeoman](http://yeoman.io/)-mallgeneratorn på datorn från NPM</span><span class="sxs-lookup"><span data-stu-id="17ffc-117">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="17ffc-118">Installera Service Fabric Yeo Java-programgeneratorn från NPM</span><span class="sxs-lookup"><span data-stu-id="17ffc-118">Install the Service Fabric Yeo Java application generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfcsharp
  ```

## <a name="create-the-application"></a><span data-ttu-id="17ffc-119">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="17ffc-119">Create the application</span></span>
<span data-ttu-id="17ffc-120">Ett Service Fabric-program kan innehålla en eller flera tjänster, som var och en ansvarar för att leverera programmets funktioner.</span><span class="sxs-lookup"><span data-stu-id="17ffc-120">A Service Fabric application can contain one or more services, each with a specific role in delivering the application's functionality.</span></span> <span data-ttu-id="17ffc-121">Service Fabric [Yeoman](http://yeoman.io/)-generatorn för CSharp, som du installerade i förra steget, gör det enkelt att skapa din första tjänst och lägga till fler senare.</span><span class="sxs-lookup"><span data-stu-id="17ffc-121">The Service Fabric [Yeoman](http://yeoman.io/) generator for CSharp, which you installed in last step, makes it easy to create your first service and to add more later.</span></span> <span data-ttu-id="17ffc-122">Använd Yeoman för att skapa ett program med en enskild tjänst.</span><span class="sxs-lookup"><span data-stu-id="17ffc-122">Let's use Yeoman to create an application with a single service.</span></span>

1. <span data-ttu-id="17ffc-123">Skriv följande kommando i en terminal, för att börja bygga ställningarna: `yo azuresfcsharp`</span><span class="sxs-lookup"><span data-stu-id="17ffc-123">In a terminal, type the following command to start building the scaffolding: `yo azuresfcsharp`</span></span>
2. <span data-ttu-id="17ffc-124">Namnge ditt program.</span><span class="sxs-lookup"><span data-stu-id="17ffc-124">Name your application.</span></span>
3. <span data-ttu-id="17ffc-125">Välj vilken typ din första tjänst ska ha och namnge den.</span><span class="sxs-lookup"><span data-stu-id="17ffc-125">Choose the type of your first service and name it.</span></span> <span data-ttu-id="17ffc-126">För den här självstudien väljer vi en Reliable Actor-tjänst.</span><span class="sxs-lookup"><span data-stu-id="17ffc-126">For the purposes of this tutorial, we choose a Reliable Actor Service.</span></span>

   ![Service Fabric Yeoman-generator för C#][sf-yeoman]

> [!NOTE]
> <span data-ttu-id="17ffc-128">Mer information om alternativen finns i [Översikt över Service Fabric-programmeringsmodell](service-fabric-choose-framework.md).</span><span class="sxs-lookup"><span data-stu-id="17ffc-128">For more information about the options, see [Service Fabric programming model overview](service-fabric-choose-framework.md).</span></span>
>
>

## <a name="build-the-application"></a><span data-ttu-id="17ffc-129">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="17ffc-129">Build the application</span></span>
<span data-ttu-id="17ffc-130">Service Fabric Yeoman-mallarna inkluderar ett byggskript som du kan använda för att skapa programmet från terminalen (efter att du navigerat till programmappen).</span><span class="sxs-lookup"><span data-stu-id="17ffc-130">The Service Fabric Yeoman templates include a build script that you can use to build the app from the terminal (after navigating to the application folder).</span></span>

  ```sh
 cd myapp
 ./build.sh
  ```

## <a name="deploy-the-application"></a><span data-ttu-id="17ffc-131">Distribuera programmet</span><span class="sxs-lookup"><span data-stu-id="17ffc-131">Deploy the application</span></span>

<span data-ttu-id="17ffc-132">När du har skapat programmet kan du distribuera det till det lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="17ffc-132">Once the application is built, you can deploy it to the local cluster.</span></span>

1. <span data-ttu-id="17ffc-133">Anslut till det lokala Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="17ffc-133">Connect to the local Service Fabric cluster.</span></span>

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. <span data-ttu-id="17ffc-134">Kör installationsskriptet som medföljer mallen för att kopiera programpaketet till klustrets avbildningsarkiv, registrera programtypen och skapa en instans av programmet.</span><span class="sxs-lookup"><span data-stu-id="17ffc-134">Run the install script provided in the template to copy the application package to the cluster's image store, register the application type, and create an instance of the application.</span></span>

    ```bash
    ./install.sh
    ```

<span data-ttu-id="17ffc-135">Distributionen går till på samma sätt som för andra Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="17ffc-135">Deploying the built application is the same as any other Service Fabric application.</span></span> <span data-ttu-id="17ffc-136">Detaljerade instruktioner finns i dokumentationen om att [hantera ett Service Fabric-program med Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md).</span><span class="sxs-lookup"><span data-stu-id="17ffc-136">See the documentation on [managing a Service Fabric application with the Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) for detailed instructions.</span></span>

<span data-ttu-id="17ffc-137">Du hittar parametrarna till de här kommandona i de genererade manifesten i programpaketet.</span><span class="sxs-lookup"><span data-stu-id="17ffc-137">Parameters to these commands can be found in the generated manifests inside the application package.</span></span>

<span data-ttu-id="17ffc-138">När programmet har distribuerats öppnar du en webbläsare och går till [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) på adressen [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="17ffc-138">Once the application has been deployed, open a browser and navigate to [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) at [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="17ffc-139">Expandera sedan noden **Program** och observera att det nu finns en post för din programtyp och en post för den första instansen av den typen.</span><span class="sxs-lookup"><span data-stu-id="17ffc-139">Then, expand the **Applications** node and note that there is now an entry for your application type and another for the first instance of that type.</span></span>

## <a name="start-the-test-client-and-perform-a-failover"></a><span data-ttu-id="17ffc-140">Starta testklienten och utför en redundansväxling</span><span class="sxs-lookup"><span data-stu-id="17ffc-140">Start the test client and perform a failover</span></span>
<span data-ttu-id="17ffc-141">Aktörsprojekt gör ingenting på egen hand.</span><span class="sxs-lookup"><span data-stu-id="17ffc-141">Actor projects do not do anything on their own.</span></span> <span data-ttu-id="17ffc-142">Det behövs en annan tjänst eller klient för att skicka meddelanden till dem.</span><span class="sxs-lookup"><span data-stu-id="17ffc-142">They require another service or client to send them messages.</span></span> <span data-ttu-id="17ffc-143">Aktörsmallen innehåller ett enkelt testskript som du kan använda för att interagera med aktörstjänsten.</span><span class="sxs-lookup"><span data-stu-id="17ffc-143">The actor template includes a simple test script that you can use to interact with the actor service.</span></span>

1. <span data-ttu-id="17ffc-144">Kör skriptet med övervakningsverktyget för att se resultatet av aktörstjänsten.</span><span class="sxs-lookup"><span data-stu-id="17ffc-144">Run the script using the watch utility to see the output of the actor service.</span></span>

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. <span data-ttu-id="17ffc-145">I Service Fabric Explorer letar du reda på noden där den primära repliken för aktörstjänsten finns.</span><span class="sxs-lookup"><span data-stu-id="17ffc-145">In Service Fabric Explorer, locate node hosting the primary replica for the actor service.</span></span> <span data-ttu-id="17ffc-146">På skärmbilden nedan är det nod 3.</span><span class="sxs-lookup"><span data-stu-id="17ffc-146">In the screenshot below, it is node 3.</span></span>

    ![Hitta den primära repliken i Service Fabric Explorer][sfx-primary]
3. <span data-ttu-id="17ffc-148">Klicka på noden du hittade i föregående steg och välj sedan **Deactivate (restart)** (Inaktivera (omstart)) på menyn Actions (Åtgärder).</span><span class="sxs-lookup"><span data-stu-id="17ffc-148">Click the node you found in the previous step, then select **Deactivate (restart)** from the Actions menu.</span></span> <span data-ttu-id="17ffc-149">Den här åtgärden startar om en nod i ditt lokala kluster, vilket framtvingar en redundansväxling till en sekundär replik som körs på en annan nod.</span><span class="sxs-lookup"><span data-stu-id="17ffc-149">This action restarts one node in your local cluster forcing a failover to a secondary replica running on another node.</span></span> <span data-ttu-id="17ffc-150">När du utför åtgärden, ska du vara uppmärksam på utdata från testklienten och notera att räknaren fortsätter att öka trots redundansen.</span><span class="sxs-lookup"><span data-stu-id="17ffc-150">As you perform this action, pay attention to the output from the test client and note that the counter continues to increment despite the failover.</span></span>

## <a name="adding-more-services-to-an-existing-application"></a><span data-ttu-id="17ffc-151">Lägga till fler tjänster till ett befintligt program</span><span class="sxs-lookup"><span data-stu-id="17ffc-151">Adding more services to an existing application</span></span>

<span data-ttu-id="17ffc-152">Om du vill lägga till en till tjänst till ett program som redan har skapats med hjälp av `yo` utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="17ffc-152">To add another service to an application already created using `yo`, perform the following steps:</span></span>
1. <span data-ttu-id="17ffc-153">Ändra katalogen till roten för det befintliga programmet.</span><span class="sxs-lookup"><span data-stu-id="17ffc-153">Change directory to the root of the existing application.</span></span>  <span data-ttu-id="17ffc-154">Till exempel `cd ~/YeomanSamples/MyApplication` om `MyApplication` är programmet som skapats av Yeoman.</span><span class="sxs-lookup"><span data-stu-id="17ffc-154">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is the application created by Yeoman.</span></span>
2. <span data-ttu-id="17ffc-155">Kör `yo azuresfcsharp:AddService`</span><span class="sxs-lookup"><span data-stu-id="17ffc-155">Run `yo azuresfcsharp:AddService`</span></span>

## <a name="migrating-from-projectjson-to-csproj"></a><span data-ttu-id="17ffc-156">Migrera från project.json till .csproj</span><span class="sxs-lookup"><span data-stu-id="17ffc-156">Migrating from project.json to .csproj</span></span>
1. <span data-ttu-id="17ffc-157">Om du kör ”dotnet migrate” i projektets rotkatalog migreras alla project.json-filer till csproj-format.</span><span class="sxs-lookup"><span data-stu-id="17ffc-157">Running 'dotnet migrate' in project root directory will migrate all the project.json to csproj format.</span></span>
2. <span data-ttu-id="17ffc-158">Uppdatera projektreferenserna till csproj-filer i projektfiler.</span><span class="sxs-lookup"><span data-stu-id="17ffc-158">Update the project references accordingly to csproj files in project files.</span></span>
3. <span data-ttu-id="17ffc-159">Uppdatera projektfilnamnen till csproj-filer i build.sh.</span><span class="sxs-lookup"><span data-stu-id="17ffc-159">Update the project file names to csproj files in build.sh.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17ffc-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="17ffc-160">Next steps</span></span>

* [<span data-ttu-id="17ffc-161">Läs mer om Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="17ffc-161">Learn more about Reliable Actors</span></span>](service-fabric-reliable-actors-introduction.md)
* [<span data-ttu-id="17ffc-162">Interagera med Service Fabric-kluster med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="17ffc-162">Interacting with Service Fabric clusters using the Service Fabric CLI</span></span>](service-fabric-cli.md)
* <span data-ttu-id="17ffc-163">Lär dig mer om [Service Fabric-supportalternativen](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="17ffc-163">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>
* [<span data-ttu-id="17ffc-164">Kom igång med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="17ffc-164">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
