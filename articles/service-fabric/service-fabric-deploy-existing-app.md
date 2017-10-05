---
title: "Distribuera en befintlig körbar fil till Azure Service Fabric | Microsoft Docs"
description: "Genomgång av hur du paketerar ett befintligt program som gäst körbara, så den kan användas för Service Fabric-kluster"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/02/2017
ms.author: mfussell;mikhegn
ms.openlocfilehash: a1db3dda674ffe43587333d88f3816549af3019c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-guest-executable-to-service-fabric"></a><span data-ttu-id="bbb54-103">Distribuera gäst körbara till Service Fabric</span><span class="sxs-lookup"><span data-stu-id="bbb54-103">Deploy a guest executable to Service Fabric</span></span>
<span data-ttu-id="bbb54-104">Du kan köra alla typer av kod, exempelvis Node.js, Java eller C++ i Azure Service Fabric som en tjänst.</span><span class="sxs-lookup"><span data-stu-id="bbb54-104">You can run any type of code, such as Node.js, Java, or C++ in Azure Service Fabric as a service.</span></span> <span data-ttu-id="bbb54-105">Service Fabric refererar till dessa typer av tjänster som gäst körbara filer.</span><span class="sxs-lookup"><span data-stu-id="bbb54-105">Service Fabric refers to these types of services as guest executables.</span></span>

<span data-ttu-id="bbb54-106">Gästen körbara filer behandlas av Service Fabric som tillståndslösa tjänster.</span><span class="sxs-lookup"><span data-stu-id="bbb54-106">Guest executables are treated by Service Fabric like stateless services.</span></span> <span data-ttu-id="bbb54-107">Detta innebär att de är placerade på noder i ett kluster, baserat på tillgänglighet och andra mått.</span><span class="sxs-lookup"><span data-stu-id="bbb54-107">As a result, they are placed on nodes in a cluster, based on availability and other metrics.</span></span> <span data-ttu-id="bbb54-108">Den här artikeln beskriver hur du paketet och distribuerar gäst körbara till Service Fabric-kluster med hjälp av Visual Studio eller ett kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="bbb54-108">This article describes how to package and deploy a guest executable to a Service Fabric cluster, by using Visual Studio or a command-line utility.</span></span>

<span data-ttu-id="bbb54-109">I den här artikeln beskriver vi stegen för att paketera gäst körbara och distribuera den till Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="bbb54-109">In this article, we cover the steps to package a guest executable and deploy it to Service Fabric.</span></span>  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a><span data-ttu-id="bbb54-110">Fördelar med att köra gäst körbara i Service Fabric</span><span class="sxs-lookup"><span data-stu-id="bbb54-110">Benefits of running a guest executable in Service Fabric</span></span>
<span data-ttu-id="bbb54-111">Det finns flera fördelar med att köra gäst körbara i ett Service Fabric-kluster:</span><span class="sxs-lookup"><span data-stu-id="bbb54-111">There are several advantages to running a guest executable in a Service Fabric cluster:</span></span>

* <span data-ttu-id="bbb54-112">Hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="bbb54-112">High availability.</span></span> <span data-ttu-id="bbb54-113">Program som körs i Service Fabric görs högtillgänglig.</span><span class="sxs-lookup"><span data-stu-id="bbb54-113">Applications that run in Service Fabric are made highly available.</span></span> <span data-ttu-id="bbb54-114">Service Fabric säkerställer att instanser av ett program körs.</span><span class="sxs-lookup"><span data-stu-id="bbb54-114">Service Fabric ensures that instances of an application are running.</span></span>
* <span data-ttu-id="bbb54-115">Övervakning av hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="bbb54-115">Health monitoring.</span></span> <span data-ttu-id="bbb54-116">Service Fabric-hälsoövervakning känner av om ett program körs och innehåller diagnostisk information om det uppstår ett fel.</span><span class="sxs-lookup"><span data-stu-id="bbb54-116">Service Fabric health monitoring detects if an application is running, and provides diagnostic information if there is a failure.</span></span>   
* <span data-ttu-id="bbb54-117">Livscykeln för hantering.</span><span class="sxs-lookup"><span data-stu-id="bbb54-117">Application lifecycle management.</span></span> <span data-ttu-id="bbb54-118">Förutom att tillhandahålla uppgraderingar utan avbrott, ger automatisk återställning till den tidigare versionen av Service Fabric om det finns en felaktig hälsohändelse som rapporteras vid en uppgradering.</span><span class="sxs-lookup"><span data-stu-id="bbb54-118">Besides providing upgrades with no downtime, Service Fabric provides automatic rollback to the previous version if there is a bad health event reported during an upgrade.</span></span>    
* <span data-ttu-id="bbb54-119">Densitet.</span><span class="sxs-lookup"><span data-stu-id="bbb54-119">Density.</span></span> <span data-ttu-id="bbb54-120">Du kan köra flera program i ett kluster, vilket eliminerar behovet av att varje program körs i sin egen maskinvara.</span><span class="sxs-lookup"><span data-stu-id="bbb54-120">You can run multiple applications in a cluster, which eliminates the need for each application to run on its own hardware.</span></span>
* <span data-ttu-id="bbb54-121">Synlighet: Med hjälp av REST kan du anropa namngivning av Service Fabric-tjänsten för att hitta andra tjänster i klustret.</span><span class="sxs-lookup"><span data-stu-id="bbb54-121">Discoverability: Using REST you can call the Service Fabric Naming service to find other services in the cluster.</span></span> 

## <a name="samples"></a><span data-ttu-id="bbb54-122">Exempel</span><span class="sxs-lookup"><span data-stu-id="bbb54-122">Samples</span></span>
* [<span data-ttu-id="bbb54-123">Exempel för förpackning och distribution av en gäst körbar fil</span><span class="sxs-lookup"><span data-stu-id="bbb54-123">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="bbb54-124">Exempel på två gäst körbara filer (C# och nodejs) kommunicerar via Naming service med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="bbb54-124">Sample of two guest executables (C# and nodejs) communicating via the Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a><span data-ttu-id="bbb54-125">Översikt över programmet och service manifest-filer</span><span class="sxs-lookup"><span data-stu-id="bbb54-125">Overview of application and service manifest files</span></span>
<span data-ttu-id="bbb54-126">Som del av distributionen av en gäst körbar fil är det bra att förstå Service Fabric paketering och distribution av modellen som beskrivs i [programmodell](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="bbb54-126">As part of deploying a guest executable, it is useful to understand the Service Fabric packaging and deployment model as described in [application model](service-fabric-application-model.md).</span></span> <span data-ttu-id="bbb54-127">Service Fabric paketering modellen förlitar sig på två XML-filer: manifesten applikationen eller tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbb54-127">The Service Fabric packaging model relies on two XML files: the application and service manifests.</span></span> <span data-ttu-id="bbb54-128">Schemadefinitionen för ApplicationManifest.xml och ServiceManifest.xml har installerats med Service Fabric-SDK i *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="bbb54-128">The schema definition for the ApplicationManifest.xml and ServiceManifest.xml files is installed with the Service Fabric SDK into *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

* <span data-ttu-id="bbb54-129">**Applikationsmanifestet** programmanifestet används för att beskriva programmet.</span><span class="sxs-lookup"><span data-stu-id="bbb54-129">**Application manifest** The application manifest is used to describe the application.</span></span> <span data-ttu-id="bbb54-130">Den Listar de tjänster som ska utgöra den och andra parametrar som används för att definiera hur en eller flera tjänster ska distribueras, till exempel antal instanser.</span><span class="sxs-lookup"><span data-stu-id="bbb54-130">It lists the services that compose it, and other parameters that are used to define how one or more services should be deployed, such as the number of instances.</span></span>

  <span data-ttu-id="bbb54-131">Ett program är en enhet av distribution och uppgradering i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="bbb54-131">In Service Fabric, an application is a unit of deployment and upgrade.</span></span> <span data-ttu-id="bbb54-132">Ett program kan uppgraderas som en enhet där potentiella problem och potentiella återställningar hanteras.</span><span class="sxs-lookup"><span data-stu-id="bbb54-132">An application can be upgraded as a single unit where potential failures and potential rollbacks are managed.</span></span> <span data-ttu-id="bbb54-133">Service Fabric garanterar att uppgraderingsprocessen är antingen lyckas, eller om uppgraderingen misslyckas, lämnar då inte programmet i ett okänt eller instabilt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="bbb54-133">Service Fabric guarantees that the upgrade process is either successful, or, if the upgrade fails, does not leave the application in an unknown or unstable state.</span></span>
* <span data-ttu-id="bbb54-134">**Tjänstmanifestet** service manifest går igenom komponenterna av en tjänst.</span><span class="sxs-lookup"><span data-stu-id="bbb54-134">**Service manifest** The service manifest describes the components of a service.</span></span> <span data-ttu-id="bbb54-135">Den innehåller data, till exempel namn och typ av tjänst, och dess kod och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="bbb54-135">It includes data, such as the name and type of service, and its code and configuration.</span></span> <span data-ttu-id="bbb54-136">Service manifest även vissa ytterligare parametrar som kan användas för att konfigurera tjänsten när den har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="bbb54-136">The service manifest also includes some additional parameters that can be used to configure the service once it is deployed.</span></span>

## <a name="application-package-file-structure"></a><span data-ttu-id="bbb54-137">Filstruktur för programmets paket</span><span class="sxs-lookup"><span data-stu-id="bbb54-137">Application package file structure</span></span>
<span data-ttu-id="bbb54-138">Om du vill distribuera ett program till Service Fabric bör programmet följa en fördefinierad katalogstruktur.</span><span class="sxs-lookup"><span data-stu-id="bbb54-138">To deploy an application to Service Fabric, the application should follow a predefined directory structure.</span></span> <span data-ttu-id="bbb54-139">Följande är ett exempel på denna struktur.</span><span class="sxs-lookup"><span data-stu-id="bbb54-139">The following is an example of that structure.</span></span>

```
|-- ApplicationPackageRoot
    |-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```

<span data-ttu-id="bbb54-140">ApplicationPackageRoot innehåller ApplicationManifest.xml-fil som definierar programmet.</span><span class="sxs-lookup"><span data-stu-id="bbb54-140">The ApplicationPackageRoot contains the ApplicationManifest.xml file that defines the application.</span></span> <span data-ttu-id="bbb54-141">En underkatalog för varje tjänst som ingår i programmet används för att innehålla alla artefakter som krävs för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbb54-141">A subdirectory for each service included in the application is used to contain all the artifacts that the service requires.</span></span> <span data-ttu-id="bbb54-142">Dessa underkataloger är ServiceManifest.xml och vanligtvis följande:</span><span class="sxs-lookup"><span data-stu-id="bbb54-142">These subdirectories are the ServiceManifest.xml and, typically, the following:</span></span>

* <span data-ttu-id="bbb54-143">*Koden*.</span><span class="sxs-lookup"><span data-stu-id="bbb54-143">*Code*.</span></span> <span data-ttu-id="bbb54-144">Den här katalogen innehåller koden.</span><span class="sxs-lookup"><span data-stu-id="bbb54-144">This directory contains the service code.</span></span>
* <span data-ttu-id="bbb54-145">*Config*.</span><span class="sxs-lookup"><span data-stu-id="bbb54-145">*Config*.</span></span> <span data-ttu-id="bbb54-146">Den här katalogen innehåller en Settings.xml (och andra filer vid behov) att tjänsten kan komma åt vid körning att hämta specifika konfigurationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="bbb54-146">This directory contains a Settings.xml file (and other files if necessary) that the service can access at runtime to retrieve specific configuration settings.</span></span>
* <span data-ttu-id="bbb54-147">*Data*.</span><span class="sxs-lookup"><span data-stu-id="bbb54-147">*Data*.</span></span> <span data-ttu-id="bbb54-148">Det här är en ytterligare katalog att lagra ytterligare lokala data som kan behövas för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbb54-148">This is an additional directory to store additional local data that the service may need.</span></span> <span data-ttu-id="bbb54-149">Data bör användas för att lagra endast tillfälliga data.</span><span class="sxs-lookup"><span data-stu-id="bbb54-149">Data should be used to store only ephemeral data.</span></span> <span data-ttu-id="bbb54-150">Service Fabric Kopiera inte eller replikera ändringar till datakatalogen om tjänsten måste flyttas (till exempel under växling vid fel).</span><span class="sxs-lookup"><span data-stu-id="bbb54-150">Service Fabric does not copy or replicate changes to the data directory if the service needs to be relocated (for example, during failover).</span></span>

> [!NOTE]
> <span data-ttu-id="bbb54-151">Du behöver skapa den `config` och `data` kataloger om du inte behöver dem.</span><span class="sxs-lookup"><span data-stu-id="bbb54-151">You don't have to create the `config` and `data` directories if you don't need them.</span></span>
>
>

## <a name="package-an-existing-executable"></a><span data-ttu-id="bbb54-152">Paketet en befintlig körbar fil</span><span class="sxs-lookup"><span data-stu-id="bbb54-152">Package an existing executable</span></span>
<span data-ttu-id="bbb54-153">När Paketera en gäst körbar fil kan du välja att använda en mall för Visual Studio-projekt eller [skapa programpaketet manuellt](#manually).</span><span class="sxs-lookup"><span data-stu-id="bbb54-153">When packaging a guest executable, you can choose either to use a Visual Studio project template or to [create the application package manually](#manually).</span></span> <span data-ttu-id="bbb54-154">Med Visual Studio skapas application paketet struktur och manifest-filer av den nya projektmallen.</span><span class="sxs-lookup"><span data-stu-id="bbb54-154">Using Visual Studio, the application package structure and manifest files are created by the new project template for you.</span></span>

> [!TIP]
> <span data-ttu-id="bbb54-155">Det enklaste sättet att paketera en befintlig Windows körbara till en tjänst är att använda Visual Studio och på Linux för att använda Yeoman</span><span class="sxs-lookup"><span data-stu-id="bbb54-155">The easiest way to package an existing Windows executable into a service is to use Visual Studio and on Linux to use Yeoman</span></span>
>

## <a name="use-visual-studio-to-package-and-deploy-an-existing-executable"></a><span data-ttu-id="bbb54-156">Använda Visual Studio för att paketera och distribuera en befintlig körbar fil</span><span class="sxs-lookup"><span data-stu-id="bbb54-156">Use Visual Studio to package and deploy an existing executable</span></span>
<span data-ttu-id="bbb54-157">Visual Studio har ett Service Fabric-tjänstmall som hjälper dig att distribuera gäst körbara till Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="bbb54-157">Visual Studio provides a Service Fabric service template to help you deploy a guest executable to a Service Fabric cluster.</span></span>

1. <span data-ttu-id="bbb54-158">Välj **filen** > **nytt projekt**, och skapa ett Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="bbb54-158">Choose **File** > **New Project**, and create a Service Fabric application.</span></span>
2. <span data-ttu-id="bbb54-159">Välj **gäst körbara** som tjänstmallen.</span><span class="sxs-lookup"><span data-stu-id="bbb54-159">Choose **Guest Executable** as the service template.</span></span>
3. <span data-ttu-id="bbb54-160">Klicka på **Bläddra** att välja mappen med den körbara filen och Fyll i resten av parametrar för att skapa tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbb54-160">Click **Browse** to select the folder with your executable and fill in the rest of the parameters to create the service.</span></span>
   * <span data-ttu-id="bbb54-161">*Code paketet beteende*.</span><span class="sxs-lookup"><span data-stu-id="bbb54-161">*Code Package Behavior*.</span></span> <span data-ttu-id="bbb54-162">Kan ställas in för att kopiera allt innehåll i mappen till Visual Studio-projektet, vilket är användbart om den körbara filen inte ändras.</span><span class="sxs-lookup"><span data-stu-id="bbb54-162">Can be set to copy all the content of your folder to the Visual Studio Project, which is useful if the executable does not change.</span></span> <span data-ttu-id="bbb54-163">Om du tror att den körbara filen för att ändra och vill kunna hämta nya versioner dynamiskt kan välja du att länka till mappen i stället.</span><span class="sxs-lookup"><span data-stu-id="bbb54-163">If you expect the executable to change and want the ability to pick up new builds dynamically, you can choose to link to the folder instead.</span></span> <span data-ttu-id="bbb54-164">Du kan använda länkade mappar när du skapar programprojekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bbb54-164">You can use linked folders when creating the application project in Visual Studio.</span></span> <span data-ttu-id="bbb54-165">Detta leder till källplatsen från i projektet, vilket gör det möjligt för dig att uppdatera gästen körbara i dess källa mål.</span><span class="sxs-lookup"><span data-stu-id="bbb54-165">This links to the source location from within the project, making it possible for you to update the guest executable in its source destination.</span></span> <span data-ttu-id="bbb54-166">Dessa uppdateringar blir en del av programpaket version.</span><span class="sxs-lookup"><span data-stu-id="bbb54-166">Those updates become part of the application package on build.</span></span>
   * <span data-ttu-id="bbb54-167">*Programmet* anger körbar fil som ska köras för att starta tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbb54-167">*Program* specifies the executable that should be run to start the service.</span></span>
   * <span data-ttu-id="bbb54-168">*Argument* anger argument som ska skickas till den körbara filen.</span><span class="sxs-lookup"><span data-stu-id="bbb54-168">*Arguments* specifies the arguments that should be passed to the executable.</span></span> <span data-ttu-id="bbb54-169">Det kan vara en lista över parametrar med argument.</span><span class="sxs-lookup"><span data-stu-id="bbb54-169">It can be a list of parameters with arguments.</span></span>
   * <span data-ttu-id="bbb54-170">*WorkingFolder* anger arbetskatalog för processen som ska startas.</span><span class="sxs-lookup"><span data-stu-id="bbb54-170">*WorkingFolder* specifies the working directory for the process that is going to be started.</span></span> <span data-ttu-id="bbb54-171">Du kan ange tre värden:</span><span class="sxs-lookup"><span data-stu-id="bbb54-171">You can specify three values:</span></span>
     * <span data-ttu-id="bbb54-172">`CodeBase`Anger att arbetskatalogen ska anges till katalogen koden i programpaketet (`Code` directory som visas i föregående filstruktur).</span><span class="sxs-lookup"><span data-stu-id="bbb54-172">`CodeBase` specifies that the working directory is going to be set to the code directory in the application package (`Code` directory shown in the preceding file structure).</span></span>
     * <span data-ttu-id="bbb54-173">`CodePackage`Anger att arbetskatalogen ska anges till roten i programpaketet (`GuestService1Pkg` visas i föregående filstruktur).</span><span class="sxs-lookup"><span data-stu-id="bbb54-173">`CodePackage` specifies that the working directory is going to be set to the root of the application package    (`GuestService1Pkg` shown in the preceding file structure).</span></span>
     * <span data-ttu-id="bbb54-174">`Work`Anger att filerna placeras i en underkatalog med namnet arbete.</span><span class="sxs-lookup"><span data-stu-id="bbb54-174">`Work` specifies that the files are placed in a subdirectory called work.</span></span>
4. <span data-ttu-id="bbb54-175">Namnge tjänsten och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bbb54-175">Give your service a name, and click **OK**.</span></span>
5. <span data-ttu-id="bbb54-176">Om din tjänst måste en slutpunkt för kommunikation, du kan nu lägga till protokollet, porten och typ ServiceManifest.xml-filen.</span><span class="sxs-lookup"><span data-stu-id="bbb54-176">If your service needs an endpoint for communication, you can now add the protocol, port, and type to the ServiceManifest.xml file.</span></span> <span data-ttu-id="bbb54-177">Till exempel: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.</span><span class="sxs-lookup"><span data-stu-id="bbb54-177">For example: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.</span></span>
6. <span data-ttu-id="bbb54-178">Du kan nu använder paketet och publicera åtgärd mot ditt lokala kluster med felsökning lösningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bbb54-178">You can now use the package and publish action against your local cluster by debugging the solution in Visual Studio.</span></span> <span data-ttu-id="bbb54-179">När du är klar kan du publicera program till ett kluster eller kontrollera i lösningen till källkontroll.</span><span class="sxs-lookup"><span data-stu-id="bbb54-179">When ready, you can publish the application to a remote cluster or check in the solution to source control.</span></span>
7. <span data-ttu-id="bbb54-180">Gå till slutet av den här artikeln för att se hur du visar din körbara gäst-tjänst körs i Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="bbb54-180">Go to the end of this article to see how to view your guest executable service running in Service Fabric Explorer.</span></span>

## <a name="use-yoeman-to-package-and-deploy-an-existing-executable-on-linux"></a><span data-ttu-id="bbb54-181">Använda Yoeman att paketera och distribuera en befintlig körbar fil på Linux</span><span class="sxs-lookup"><span data-stu-id="bbb54-181">Use Yoeman to package and deploy an existing executable on Linux</span></span>

<span data-ttu-id="bbb54-182">Proceduren för att skapa och distribuera gäst körbara på Linux är samma som distribuerar ett csharp eller java-program.</span><span class="sxs-lookup"><span data-stu-id="bbb54-182">The procedure for creating and deploying a guest executable on Linux is the same as deploying a csharp or java application.</span></span>

1. <span data-ttu-id="bbb54-183">I en terminal, skriver du in `yo azuresfguest`.</span><span class="sxs-lookup"><span data-stu-id="bbb54-183">In a terminal, type `yo azuresfguest`.</span></span>
2. <span data-ttu-id="bbb54-184">Namnge ditt program.</span><span class="sxs-lookup"><span data-stu-id="bbb54-184">Name your application.</span></span>
3. <span data-ttu-id="bbb54-185">Namnge din tjänst och ange de information, inklusive sökvägen till den körbara filen och det måste anropas med parametrar.</span><span class="sxs-lookup"><span data-stu-id="bbb54-185">Name your service, and provide the details including path of the executable and the parameters it must be invoked with.</span></span>

<span data-ttu-id="bbb54-186">Yeoman skapar ett programpaket med det aktuella programmet och manifestfiler tillsammans med installera och avinstallera skript.</span><span class="sxs-lookup"><span data-stu-id="bbb54-186">Yeoman creates an application package with the appropriate application and manifest files along with install and uninstall scripts.</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a><span data-ttu-id="bbb54-187">Paketera och distribuera en befintlig körbar fil manuellt</span><span class="sxs-lookup"><span data-stu-id="bbb54-187">Manually package and deploy an existing executable</span></span>
<span data-ttu-id="bbb54-188">Manuellt Paketera en körbar fil gäst baseras på följande allmänna steg:</span><span class="sxs-lookup"><span data-stu-id="bbb54-188">The process of manually packaging a guest executable is based on the following general steps:</span></span>

1. <span data-ttu-id="bbb54-189">Skapa katalogstrukturen paketet.</span><span class="sxs-lookup"><span data-stu-id="bbb54-189">Create the package directory structure.</span></span>
2. <span data-ttu-id="bbb54-190">Lägg till programmets kod och configuration-filer.</span><span class="sxs-lookup"><span data-stu-id="bbb54-190">Add the application's code and configuration files.</span></span>
3. <span data-ttu-id="bbb54-191">Redigera service manifest-filen.</span><span class="sxs-lookup"><span data-stu-id="bbb54-191">Edit the service manifest file.</span></span>
4. <span data-ttu-id="bbb54-192">Redigera programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="bbb54-192">Edit the application manifest file.</span></span>

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a><span data-ttu-id="bbb54-193">Skapa katalogstrukturen paketets</span><span class="sxs-lookup"><span data-stu-id="bbb54-193">Create the package directory structure</span></span>
<span data-ttu-id="bbb54-194">Du kan börja med att skapa katalogstrukturen, enligt beskrivningen i föregående avsnitt, ”programmet paketet filstruktur”.</span><span class="sxs-lookup"><span data-stu-id="bbb54-194">You can start by creating the directory structure, as described in the preceding section, "Application package file structure."</span></span>

### <a name="add-the-applications-code-and-configuration-files"></a><span data-ttu-id="bbb54-195">Lägg till programmets kod och configuration-filer</span><span class="sxs-lookup"><span data-stu-id="bbb54-195">Add the application's code and configuration files</span></span>
<span data-ttu-id="bbb54-196">När du har skapat katalogstrukturen kan du lägga till programkoden och konfigurationsfiler under koden och konfigurationsversionerna kataloger.</span><span class="sxs-lookup"><span data-stu-id="bbb54-196">After you have created the directory structure, you can add the application's code and configuration files under the code and config directories.</span></span> <span data-ttu-id="bbb54-197">Du kan också skapa ytterligare kataloger eller underkataloger på kod eller konfigurationsversion kataloger.</span><span class="sxs-lookup"><span data-stu-id="bbb54-197">You can also create additional directories or subdirectories under the code or config directories.</span></span>

<span data-ttu-id="bbb54-198">Service Fabric har en `xcopy` av innehållet i tillämpningsprogrammets rotkatalog, så det finns ingen fördefinierad struktur att använda andra än att skapa två översta kataloger, kod och inställningar.</span><span class="sxs-lookup"><span data-stu-id="bbb54-198">Service Fabric does an `xcopy` of the content of the application root directory, so there is no predefined structure to use other than creating two top directories, code and settings.</span></span> <span data-ttu-id="bbb54-199">(Du kan välja olika namn om du vill.</span><span class="sxs-lookup"><span data-stu-id="bbb54-199">(You can pick different names if you want.</span></span> <span data-ttu-id="bbb54-200">Mer information finns i nästa avsnitt.)</span><span class="sxs-lookup"><span data-stu-id="bbb54-200">More details are in the next section.)</span></span>

> [!NOTE]
> <span data-ttu-id="bbb54-201">Kontrollera att du inkluderar alla filer och beroenden som programmet behöver.</span><span class="sxs-lookup"><span data-stu-id="bbb54-201">Make sure that you include all the files and dependencies that the application needs.</span></span> <span data-ttu-id="bbb54-202">Service Fabric kopieras innehållet för programpaket på alla noder i klustret där programmets tjänster kommer att distribueras.</span><span class="sxs-lookup"><span data-stu-id="bbb54-202">Service Fabric copies the content of the application package on all nodes in the cluster where the application's services are going to be deployed.</span></span> <span data-ttu-id="bbb54-203">Paketet innehåller all kod som programmet måste köras.</span><span class="sxs-lookup"><span data-stu-id="bbb54-203">The package should contain all the code that the application needs to run.</span></span> <span data-ttu-id="bbb54-204">Förutsätter inte att beroenden som redan är installerade.</span><span class="sxs-lookup"><span data-stu-id="bbb54-204">Do not assume that the dependencies are already installed.</span></span>
>
>

### <a name="edit-the-service-manifest-file"></a><span data-ttu-id="bbb54-205">Redigera filen service manifest</span><span class="sxs-lookup"><span data-stu-id="bbb54-205">Edit the service manifest file</span></span>
<span data-ttu-id="bbb54-206">Nästa steg är att redigera filen service manifest för att inkludera följande information:</span><span class="sxs-lookup"><span data-stu-id="bbb54-206">The next step is to edit the service manifest file to include the following information:</span></span>

* <span data-ttu-id="bbb54-207">Namnet på service-typen.</span><span class="sxs-lookup"><span data-stu-id="bbb54-207">The name of the service type.</span></span> <span data-ttu-id="bbb54-208">Det här är ett ID som Service Fabric används för att identifiera en tjänst.</span><span class="sxs-lookup"><span data-stu-id="bbb54-208">This is an ID that Service Fabric uses to identify a service.</span></span>
* <span data-ttu-id="bbb54-209">Kommandot för att starta programmet (ExeHost).</span><span class="sxs-lookup"><span data-stu-id="bbb54-209">The command to use to launch the application (ExeHost).</span></span>
* <span data-ttu-id="bbb54-210">Alla skript som måste köras för att ställa in programmet (SetupEntrypoint).</span><span class="sxs-lookup"><span data-stu-id="bbb54-210">Any script that needs to be run to set up the application (SetupEntrypoint).</span></span>

<span data-ttu-id="bbb54-211">Följande är ett exempel på en `ServiceManifest.xml` fil:</span><span class="sxs-lookup"><span data-stu-id="bbb54-211">The following is an example of a `ServiceManifest.xml` file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

<span data-ttu-id="bbb54-212">I följande avsnitt gå igenom de olika delarna av filen som du behöver uppdatera.</span><span class="sxs-lookup"><span data-stu-id="bbb54-212">The following sections go over the different parts of the file that you need to update.</span></span>

#### <a name="update-servicetypes"></a><span data-ttu-id="bbb54-213">Uppdatera ServiceTypes</span><span class="sxs-lookup"><span data-stu-id="bbb54-213">Update ServiceTypes</span></span>
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* <span data-ttu-id="bbb54-214">Du kan välja vilket namn som du vill använda för `ServiceTypeName`.</span><span class="sxs-lookup"><span data-stu-id="bbb54-214">You can pick any name that you want for `ServiceTypeName`.</span></span> <span data-ttu-id="bbb54-215">Värdet används i den `ApplicationManifest.xml` fil för att identifiera tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbb54-215">The value is used in the `ApplicationManifest.xml` file to identify the service.</span></span>
* <span data-ttu-id="bbb54-216">Ange `UseImplicitHost="true"`.</span><span class="sxs-lookup"><span data-stu-id="bbb54-216">Specify `UseImplicitHost="true"`.</span></span> <span data-ttu-id="bbb54-217">Det här attributet anger Service Fabric att tjänsten är baserat på en fristående app så att alla Service Fabric behöver göra är att starta som en process och övervaka dess hälsa.</span><span class="sxs-lookup"><span data-stu-id="bbb54-217">This attribute tells Service Fabric that the service is based on a self-contained app, so all Service Fabric needs to do is to launch it as a process and monitor its health.</span></span>

#### <a name="update-codepackage"></a><span data-ttu-id="bbb54-218">Uppdatera CodePackage</span><span class="sxs-lookup"><span data-stu-id="bbb54-218">Update CodePackage</span></span>
<span data-ttu-id="bbb54-219">Elementet CodePackage anger tjänstens kod plats (och version).</span><span class="sxs-lookup"><span data-stu-id="bbb54-219">The CodePackage element specifies the location (and version) of the service's code.</span></span>

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

<span data-ttu-id="bbb54-220">Den `Name` element som används för att ange namnet på katalogen i programpaket som innehåller tjänstens kod.</span><span class="sxs-lookup"><span data-stu-id="bbb54-220">The `Name` element is used to specify the name of the directory in the application package that contains the service's code.</span></span> <span data-ttu-id="bbb54-221">`CodePackage`också har den `version` attribut.</span><span class="sxs-lookup"><span data-stu-id="bbb54-221">`CodePackage` also has the `version` attribute.</span></span> <span data-ttu-id="bbb54-222">Detta kan användas för att ange versionen av koden och också potentiellt kan användas för att uppgradera tjänstens kod med hjälp av programmet livscykel hanteringsinfrastrukturen i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="bbb54-222">This can be used to specify the version of the code, and can also potentially be used to upgrade the service's code by using the application lifecycle management infrastructure in Service Fabric.</span></span>

#### <a name="optional-update-setupentrypoint"></a><span data-ttu-id="bbb54-223">Valfritt: Uppdatera SetupEntrypoint</span><span class="sxs-lookup"><span data-stu-id="bbb54-223">Optional: Update SetupEntrypoint</span></span>
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
<span data-ttu-id="bbb54-224">Elementet SetupEntryPoint används för att ange en körbar fil eller en batch-fil som ska köras innan tjänstens koden har startats.</span><span class="sxs-lookup"><span data-stu-id="bbb54-224">The SetupEntryPoint element is used to specify any executable or batch file that should be executed before the service's code is launched.</span></span> <span data-ttu-id="bbb54-225">Det är ett valfritt steg så inte behöver inkluderas om det inte finns några initiering krävs.</span><span class="sxs-lookup"><span data-stu-id="bbb54-225">It is an optional step, so it does not need to be included if there is no initialization required.</span></span> <span data-ttu-id="bbb54-226">SetupEntryPoint körs varje gång tjänsten startas.</span><span class="sxs-lookup"><span data-stu-id="bbb54-226">The SetupEntryPoint is executed every time the service is restarted.</span></span>

<span data-ttu-id="bbb54-227">Det finns endast en SetupEntryPoint installationsskript behöver grupperas i en enda kommandofil om programmets installationen kräver flera skript.</span><span class="sxs-lookup"><span data-stu-id="bbb54-227">There is only one SetupEntryPoint, so setup scripts need to be grouped in a single batch file if the application's setup requires multiple scripts.</span></span> <span data-ttu-id="bbb54-228">SetupEntryPoint kan köra alla typer av filer: körbara filer, batch-filer och PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="bbb54-228">The SetupEntryPoint can execute any type of file: executable files, batch files, and PowerShell cmdlets.</span></span> <span data-ttu-id="bbb54-229">Mer information finns i [konfigurera SetupEntryPoint](service-fabric-application-runas-security.md).</span><span class="sxs-lookup"><span data-stu-id="bbb54-229">For more details, see [Configure SetupEntryPoint](service-fabric-application-runas-security.md).</span></span>

<span data-ttu-id="bbb54-230">I föregående exempel körs SetupEntryPoint en batchfil som kallas `LaunchConfig.cmd` som finns i den `scripts` kod-mappen (förutsatt att elementet WorkingFolder är inställd på CodeBase).</span><span class="sxs-lookup"><span data-stu-id="bbb54-230">In the preceding example, the SetupEntryPoint runs a batch file called `LaunchConfig.cmd` that is located in the `scripts` subdirectory of the code directory (assuming the WorkingFolder element is set to CodeBase).</span></span>

#### <a name="update-entrypoint"></a><span data-ttu-id="bbb54-231">Uppdatera EntryPoint</span><span class="sxs-lookup"><span data-stu-id="bbb54-231">Update EntryPoint</span></span>
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

<span data-ttu-id="bbb54-232">Den `EntryPoint` element i service manifest-filen används för att ange hur du vill starta tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbb54-232">The `EntryPoint` element in the service manifest file is used to specify how to launch the service.</span></span> <span data-ttu-id="bbb54-233">Den `ExeHost` element anger den körbara filen (och argument) som ska användas för att starta tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbb54-233">The `ExeHost` element specifies the executable (and arguments) that should be used to launch the service.</span></span>

* <span data-ttu-id="bbb54-234">`Program`Anger namnet på den körbara filen som ska starta tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbb54-234">`Program` specifies the name of the executable that should start the service.</span></span>
* <span data-ttu-id="bbb54-235">`Arguments`Anger de argument som ska skickas till den körbara filen.</span><span class="sxs-lookup"><span data-stu-id="bbb54-235">`Arguments` specifies the arguments that should be passed to the executable.</span></span> <span data-ttu-id="bbb54-236">Det kan vara en lista över parametrar med argument.</span><span class="sxs-lookup"><span data-stu-id="bbb54-236">It can be a list of parameters with arguments.</span></span>
* <span data-ttu-id="bbb54-237">`WorkingFolder`Anger arbetskatalog för processen som ska startas.</span><span class="sxs-lookup"><span data-stu-id="bbb54-237">`WorkingFolder` specifies the working directory for the process that is going to be started.</span></span> <span data-ttu-id="bbb54-238">Du kan ange tre värden:</span><span class="sxs-lookup"><span data-stu-id="bbb54-238">You can specify three values:</span></span>
  * <span data-ttu-id="bbb54-239">`CodeBase`Anger att arbetskatalogen ska anges till katalogen koden i programpaketet (`Code` katalogen i föregående filstruktur).</span><span class="sxs-lookup"><span data-stu-id="bbb54-239">`CodeBase` specifies that the working directory is going to be set to the code directory in the application package (`Code` directory in the preceding file structure).</span></span>
  * <span data-ttu-id="bbb54-240">`CodePackage`Anger att arbetskatalogen ska anges till roten i programpaketet (`GuestService1Pkg` i föregående filstruktur).</span><span class="sxs-lookup"><span data-stu-id="bbb54-240">`CodePackage` specifies that the working directory is going to be set to the root of the application package    (`GuestService1Pkg` in the preceding file structure).</span></span>
    * <span data-ttu-id="bbb54-241">`Work`Anger att filerna placeras i en underkatalog med namnet arbete.</span><span class="sxs-lookup"><span data-stu-id="bbb54-241">`Work` specifies that the files are placed in a subdirectory called work.</span></span>

<span data-ttu-id="bbb54-242">WorkingFolder är användbar för att ange rätt arbetskatalogen så att relativa sökvägar kan användas av program-eller initiering.</span><span class="sxs-lookup"><span data-stu-id="bbb54-242">The WorkingFolder is useful to set the correct working directory so that relative paths can be used by either the application or initialization scripts.</span></span>

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a><span data-ttu-id="bbb54-243">Uppdatera slutpunkter och registrera med namngivningstjänst för kommunikation</span><span class="sxs-lookup"><span data-stu-id="bbb54-243">Update Endpoints and register with Naming Service for communication</span></span>
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
<span data-ttu-id="bbb54-244">I föregående exempel är den `Endpoint` element anger de slutpunkter som programmet kan lyssna på.</span><span class="sxs-lookup"><span data-stu-id="bbb54-244">In the preceding example, the `Endpoint` element specifies the endpoints that the application can listen on.</span></span> <span data-ttu-id="bbb54-245">I det här exemplet Node.js-program som lyssnar på http på 3000-port.</span><span class="sxs-lookup"><span data-stu-id="bbb54-245">In this example, the Node.js application listens on http on port 3000.</span></span>

<span data-ttu-id="bbb54-246">Dessutom kan du be Service Fabric att publicera den här slutpunkten till Naming Service så att andra tjänster kan identifiera slutpunktsadress till den här tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbb54-246">Furthermore you can ask Service Fabric to publish this endpoint to the Naming Service so other services can discover the endpoint address to this service.</span></span> <span data-ttu-id="bbb54-247">På så sätt kan du kommunicera mellan tjänster som gäst körbara filer.</span><span class="sxs-lookup"><span data-stu-id="bbb54-247">This enables you to be able to communicate between services that are guest executables.</span></span>
<span data-ttu-id="bbb54-248">Publicerade slutpunktsadressen har formatet `UriScheme://IPAddressOrFQDN:Port/PathSuffix`.</span><span class="sxs-lookup"><span data-stu-id="bbb54-248">The published endpoint address is of the form `UriScheme://IPAddressOrFQDN:Port/PathSuffix`.</span></span> <span data-ttu-id="bbb54-249">`UriScheme`och `PathSuffix` är valfria attribut.</span><span class="sxs-lookup"><span data-stu-id="bbb54-249">`UriScheme` and `PathSuffix` are optional attributes.</span></span> <span data-ttu-id="bbb54-250">`IPAddressOrFQDN`är IP-adress eller fullständigt kvalificerade domännamnet för den körbara filen hämtar placeras på noden och beräknas åt dig.</span><span class="sxs-lookup"><span data-stu-id="bbb54-250">`IPAddressOrFQDN` is the IP address or fully qualified domain name of the node this executable gets placed on, and it is calculated for you.</span></span>

<span data-ttu-id="bbb54-251">I följande exempel, när tjänsten har distribuerats, Service Fabric Explorer visas i en slutpunkt som liknar `http://10.1.4.92:3000/myapp/` publicerats för tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="bbb54-251">In the following example, once the service is deployed, in Service Fabric Explorer you see an endpoint similar to `http://10.1.4.92:3000/myapp/` published for the service instance.</span></span> <span data-ttu-id="bbb54-252">Eller om det är en lokal dator kan du se `http://localhost:3000/myapp/`.</span><span class="sxs-lookup"><span data-stu-id="bbb54-252">Or if this is a local machine, you see `http://localhost:3000/myapp/`.</span></span>

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
<span data-ttu-id="bbb54-253">Du kan använda dessa adresser med [omvänd proxy](service-fabric-reverseproxy.md) för kommunikation mellan tjänster.</span><span class="sxs-lookup"><span data-stu-id="bbb54-253">You can use these addresses with [reverse proxy](service-fabric-reverseproxy.md) to communicate between services.</span></span>

### <a name="edit-the-application-manifest-file"></a><span data-ttu-id="bbb54-254">Redigera programmanifestfilen</span><span class="sxs-lookup"><span data-stu-id="bbb54-254">Edit the application manifest file</span></span>
<span data-ttu-id="bbb54-255">När du har konfigurerat den `Servicemanifest.xml` -fil, måste du göra vissa ändringar i den `ApplicationManifest.xml` filen så att rätt typ och namn används.</span><span class="sxs-lookup"><span data-stu-id="bbb54-255">Once you have configured the `Servicemanifest.xml` file, you need to make some changes to the `ApplicationManifest.xml` file to ensure that the correct service type and name are used.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a><span data-ttu-id="bbb54-256">ServiceManifestImport</span><span class="sxs-lookup"><span data-stu-id="bbb54-256">ServiceManifestImport</span></span>
<span data-ttu-id="bbb54-257">I den `ServiceManifestImport` element, kan du ange en eller flera tjänster som du vill ska ingå i appen.</span><span class="sxs-lookup"><span data-stu-id="bbb54-257">In the `ServiceManifestImport` element, you can specify one or more services that you want to include in the app.</span></span> <span data-ttu-id="bbb54-258">Tjänster refereras med `ServiceManifestName`, som anger namnet på katalogen där den `ServiceManifest.xml` filen finns.</span><span class="sxs-lookup"><span data-stu-id="bbb54-258">Services are referenced with `ServiceManifestName`, which specifies the name of the directory where the `ServiceManifest.xml` file is located.</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a><span data-ttu-id="bbb54-259">Konfigurera loggning</span><span class="sxs-lookup"><span data-stu-id="bbb54-259">Set up logging</span></span>
<span data-ttu-id="bbb54-260">Gästen körbara filer är det praktiskt att kunna se loggarna för konsolen ta reda på om program- och skript visar eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="bbb54-260">For guest executables, it is useful to be able to see console logs to find out if the application and configuration scripts show any errors.</span></span>
<span data-ttu-id="bbb54-261">Omdirigering av konsol kan konfigureras i den `ServiceManifest.xml` fil med hjälp av `ConsoleRedirection` element.</span><span class="sxs-lookup"><span data-stu-id="bbb54-261">Console redirection can be configured in the `ServiceManifest.xml` file using the `ConsoleRedirection` element.</span></span>

> [!WARNING]
> <span data-ttu-id="bbb54-262">Använd aldrig omdirigeringspolicyn konsolen i ett program som distribuerats i produktionsmiljön eftersom detta kan påverka program för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="bbb54-262">Never use the console redirection policy in an application that is deployed in production because this can affect the application failover.</span></span> <span data-ttu-id="bbb54-263">*Endast* använda detta för lokal utveckling och felsökning.</span><span class="sxs-lookup"><span data-stu-id="bbb54-263">*Only* use this for local development and debugging purposes.</span></span>  
>
>

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

<span data-ttu-id="bbb54-264">`ConsoleRedirection`kan användas för att dirigera konsolens utdata (stdout och stderr) till en arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="bbb54-264">`ConsoleRedirection` can be used to redirect console output (both stdout and stderr) to a working directory.</span></span> <span data-ttu-id="bbb54-265">Detta ger dig möjlighet att kontrollera att det inte finns några fel vid installation eller körning av program i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="bbb54-265">This provides the ability to verify that there are no errors during the setup or execution of the application in the Service Fabric cluster.</span></span>

<span data-ttu-id="bbb54-266">`FileRetentionCount`Anger hur många filer sparas i arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="bbb54-266">`FileRetentionCount` determines how many files are saved in the working directory.</span></span> <span data-ttu-id="bbb54-267">Värdet 5, till exempel innebär att du loggfilerna för föregående fem körningar lagras i arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="bbb54-267">A value of 5, for example, means that the log files for the previous five executions are stored in the working directory.</span></span>

<span data-ttu-id="bbb54-268">`FileMaxSizeInKb`Anger den maximala storleken för loggfiler.</span><span class="sxs-lookup"><span data-stu-id="bbb54-268">`FileMaxSizeInKb` specifies the maximum size of the log files.</span></span>

<span data-ttu-id="bbb54-269">Loggfilerna sparas i en av tjänstens fungerande kataloger.</span><span class="sxs-lookup"><span data-stu-id="bbb54-269">Log files are saved in one of the service's working directories.</span></span> <span data-ttu-id="bbb54-270">Att fastställa där filerna finns, Service Fabric Explorer för att bestämma vilken nod som tjänsten körs på och vilka arbetskatalogen används.</span><span class="sxs-lookup"><span data-stu-id="bbb54-270">To determine where the files are located, use Service Fabric Explorer to determine which node the service is running on, and which working directory is being used.</span></span> <span data-ttu-id="bbb54-271">Den här processen beskrivs senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="bbb54-271">This process is covered later in this article.</span></span>

## <a name="deployment"></a><span data-ttu-id="bbb54-272">Distribution</span><span class="sxs-lookup"><span data-stu-id="bbb54-272">Deployment</span></span>
<span data-ttu-id="bbb54-273">Det sista steget är att [distribuera programmet](service-fabric-deploy-remove-applications.md).</span><span class="sxs-lookup"><span data-stu-id="bbb54-273">The last step is to [deploy your application](service-fabric-deploy-remove-applications.md).</span></span> <span data-ttu-id="bbb54-274">Följande PowerShell-skript visar hur du distribuerar ditt program i klustret för lokal utveckling och starta en ny Service Fabric-tjänst.</span><span class="sxs-lookup"><span data-stu-id="bbb54-274">The following PowerShell script shows how to deploy your application to the local development cluster, and start a new Service Fabric service.</span></span>

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```

>[!TIP]
> <span data-ttu-id="bbb54-275">[Komprimera paketet](service-fabric-package-apps.md#compress-a-package) innan du kopierar till image store om paketet är stora eller många filer.</span><span class="sxs-lookup"><span data-stu-id="bbb54-275">[Compress the package](service-fabric-package-apps.md#compress-a-package) before copying to the image store if the package is large or has many files.</span></span> <span data-ttu-id="bbb54-276">Läs mer [här](service-fabric-deploy-remove-applications.md#upload-the-application-package).</span><span class="sxs-lookup"><span data-stu-id="bbb54-276">Read more [here](service-fabric-deploy-remove-applications.md#upload-the-application-package).</span></span>
>

<span data-ttu-id="bbb54-277">Ett Service Fabric-tjänsten kan distribueras i olika ”konfigurationer”.</span><span class="sxs-lookup"><span data-stu-id="bbb54-277">A Service Fabric service can be deployed in various "configurations."</span></span> <span data-ttu-id="bbb54-278">Till exempel den kan distribueras som en eller flera instanser eller mallen kan distribueras på ett sådant sätt att det finns en instans av tjänsten på varje nod i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="bbb54-278">For example, it can be deployed as single or multiple instances, or it can be deployed in such a way that there is one instance of the service on each node of the Service Fabric cluster.</span></span>

<span data-ttu-id="bbb54-279">Den `InstanceCount` parameter för den `New-ServiceFabricService` cmdlet som används för att ange hur många instanser av tjänsten ska startas i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="bbb54-279">The `InstanceCount` parameter of the `New-ServiceFabricService` cmdlet is used to specify how many instances of the service should be launched in the Service Fabric cluster.</span></span> <span data-ttu-id="bbb54-280">Du kan ange den `InstanceCount` värde, beroende på vilken typ av program som du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="bbb54-280">You can set the `InstanceCount` value, depending on the type of application that you are deploying.</span></span> <span data-ttu-id="bbb54-281">De två vanligaste scenarierna är:</span><span class="sxs-lookup"><span data-stu-id="bbb54-281">The two most common scenarios are:</span></span>

* <span data-ttu-id="bbb54-282">`InstanceCount = "1"`.</span><span class="sxs-lookup"><span data-stu-id="bbb54-282">`InstanceCount = "1"`.</span></span> <span data-ttu-id="bbb54-283">Endast en instans av tjänsten distribueras i det här fallet i klustret.</span><span class="sxs-lookup"><span data-stu-id="bbb54-283">In this case, only one instance of the service is deployed in the cluster.</span></span> <span data-ttu-id="bbb54-284">Service Fabric scheduler avgör vilken nod som ska distribueras på tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bbb54-284">Service Fabric's scheduler determines which node the service is going to be deployed on.</span></span>
* <span data-ttu-id="bbb54-285">`InstanceCount ="-1"`.</span><span class="sxs-lookup"><span data-stu-id="bbb54-285">`InstanceCount ="-1"`.</span></span> <span data-ttu-id="bbb54-286">I det här fallet distribueras en instans av tjänsten på varje nod i Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="bbb54-286">In this case, one instance of the service is deployed on every node in the Service Fabric cluster.</span></span> <span data-ttu-id="bbb54-287">Resultatet är att ha ett (och endast ett) instans av tjänsten för varje nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="bbb54-287">The result is having one (and only one) instance of the service for each node in the cluster.</span></span>

<span data-ttu-id="bbb54-288">Detta är en användbar konfiguration för frontend-program (till exempel en REST-slutpunkt), eftersom klientprogram behöver ”ansluta” till någon av noderna i klustret för att använda slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="bbb54-288">This is a useful configuration for front-end applications (for example, a REST endpoint), because client applications need to "connect" to any of the nodes in the cluster to use the endpoint.</span></span> <span data-ttu-id="bbb54-289">Den här konfigurationen kan också användas om till exempel alla noder i Service Fabric-klustret är anslutna till en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="bbb54-289">This configuration can also be used when, for example, all nodes of the Service Fabric cluster are connected to a load balancer.</span></span> <span data-ttu-id="bbb54-290">Klienttrafik kan sedan distribueras över den tjänst som körs på alla noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="bbb54-290">Client traffic can then be distributed across the service that is running on all nodes in the cluster.</span></span>

## <a name="check-your-running-application"></a><span data-ttu-id="bbb54-291">Kontrollera att programmet körs</span><span class="sxs-lookup"><span data-stu-id="bbb54-291">Check your running application</span></span>
<span data-ttu-id="bbb54-292">Service Fabric Explorer för att identifiera den nod där tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="bbb54-292">In Service Fabric Explorer, identify the node where the service is running.</span></span> <span data-ttu-id="bbb54-293">I det här exemplet körs den på Nod1:</span><span class="sxs-lookup"><span data-stu-id="bbb54-293">In this example, it runs on Node1:</span></span>

![Noden som tjänsten körs](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

<span data-ttu-id="bbb54-295">Om du navigerar du till noden och bläddra till programmet, kan du se grundläggande nod-information, inklusive dess plats på disken.</span><span class="sxs-lookup"><span data-stu-id="bbb54-295">If you navigate to the node and browse to the application, you see the essential node information, including its location on disk.</span></span>

![Plats på disken](./media/service-fabric-deploy-existing-app/locationondisk2.png)

<span data-ttu-id="bbb54-297">Om du bläddrar till katalogen med hjälp av Server Explorer hittar du arbetskatalogen och tjänstens loggmappen, som visas i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="bbb54-297">If you browse to the directory by using Server Explorer, you can find the working directory and the service's log folder, as shown in the following screenshot:</span></span> 

![Plats för logg](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a><span data-ttu-id="bbb54-299">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bbb54-299">Next steps</span></span>
<span data-ttu-id="bbb54-300">I den här artikeln har du lärt dig hur du paketerar ett gäst körbart program och distribuerar den till Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="bbb54-300">In this article, you have learned how to package a guest executable and deploy it to Service Fabric.</span></span> <span data-ttu-id="bbb54-301">Se följande artiklar för relaterad information och uppgifter.</span><span class="sxs-lookup"><span data-stu-id="bbb54-301">See the following articles for related information and tasks.</span></span>

* <span data-ttu-id="bbb54-302">[Exempel för förpackning och distribution av en gäst körbar fil](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), inklusive en länk till förhandsutgåvan av verktyget paketering</span><span class="sxs-lookup"><span data-stu-id="bbb54-302">[Sample for packaging and deploying a guest executable](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), including a link to the prerelease of the packaging tool</span></span>
* [<span data-ttu-id="bbb54-303">Exempel på två gäst körbara filer (C# och nodejs) kommunicerar via Naming service med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="bbb54-303">Sample of two guest executables (C# and nodejs) communicating via the Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
* [<span data-ttu-id="bbb54-304">Distribuera flera körbara gäster</span><span class="sxs-lookup"><span data-stu-id="bbb54-304">Deploy multiple guest executables</span></span>](service-fabric-deploy-multiple-apps.md)
* [<span data-ttu-id="bbb54-305">Skapa ditt första Service Fabric-program med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbb54-305">Create your first Service Fabric application using Visual Studio</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
