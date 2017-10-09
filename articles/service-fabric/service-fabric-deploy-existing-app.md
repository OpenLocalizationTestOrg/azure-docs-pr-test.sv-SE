---
title: "aaaDeploy befintliga körbara tooAzure Service Fabric | Microsoft Docs"
description: "Genomgång av hur toopackage ett befintligt program som en gäst körbar fil, så det kan vara distribueras tooa Service Fabric-kluster"
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
ms.openlocfilehash: 5599802bdb6bda2407a138d77e12148ccb64f437
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-guest-executable-tooservice-fabric"></a><span data-ttu-id="435e8-103">Distribuera en gäst körbara tooService Fabric</span><span class="sxs-lookup"><span data-stu-id="435e8-103">Deploy a guest executable tooService Fabric</span></span>
<span data-ttu-id="435e8-104">Du kan köra alla typer av kod, exempelvis Node.js, Java eller C++ i Azure Service Fabric som en tjänst.</span><span class="sxs-lookup"><span data-stu-id="435e8-104">You can run any type of code, such as Node.js, Java, or C++ in Azure Service Fabric as a service.</span></span> <span data-ttu-id="435e8-105">Service Fabric refererar toothese typer av tjänster som gäst körbara filer.</span><span class="sxs-lookup"><span data-stu-id="435e8-105">Service Fabric refers toothese types of services as guest executables.</span></span>

<span data-ttu-id="435e8-106">Gästen körbara filer behandlas av Service Fabric som tillståndslösa tjänster.</span><span class="sxs-lookup"><span data-stu-id="435e8-106">Guest executables are treated by Service Fabric like stateless services.</span></span> <span data-ttu-id="435e8-107">Detta innebär att de är placerade på noder i ett kluster, baserat på tillgänglighet och andra mått.</span><span class="sxs-lookup"><span data-stu-id="435e8-107">As a result, they are placed on nodes in a cluster, based on availability and other metrics.</span></span> <span data-ttu-id="435e8-108">Den här artikeln beskriver hur toopackage och distribuera ett körbart tooa Service Fabric gästkluster med hjälp av Visual Studio eller ett kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="435e8-108">This article describes how toopackage and deploy a guest executable tooa Service Fabric cluster, by using Visual Studio or a command-line utility.</span></span>

<span data-ttu-id="435e8-109">I den här artikeln vi täcka hello steg toopackage gäst körbara och distribuerar den tooService Fabric.</span><span class="sxs-lookup"><span data-stu-id="435e8-109">In this article, we cover hello steps toopackage a guest executable and deploy it tooService Fabric.</span></span>  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a><span data-ttu-id="435e8-110">Fördelar med att köra gäst körbara i Service Fabric</span><span class="sxs-lookup"><span data-stu-id="435e8-110">Benefits of running a guest executable in Service Fabric</span></span>
<span data-ttu-id="435e8-111">Det finns flera fördelar toorunning gäst körbara i ett Service Fabric-kluster:</span><span class="sxs-lookup"><span data-stu-id="435e8-111">There are several advantages toorunning a guest executable in a Service Fabric cluster:</span></span>

* <span data-ttu-id="435e8-112">Hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="435e8-112">High availability.</span></span> <span data-ttu-id="435e8-113">Program som körs i Service Fabric görs högtillgänglig.</span><span class="sxs-lookup"><span data-stu-id="435e8-113">Applications that run in Service Fabric are made highly available.</span></span> <span data-ttu-id="435e8-114">Service Fabric säkerställer att instanser av ett program körs.</span><span class="sxs-lookup"><span data-stu-id="435e8-114">Service Fabric ensures that instances of an application are running.</span></span>
* <span data-ttu-id="435e8-115">Övervakning av hälsotillstånd.</span><span class="sxs-lookup"><span data-stu-id="435e8-115">Health monitoring.</span></span> <span data-ttu-id="435e8-116">Service Fabric-hälsoövervakning känner av om ett program körs och innehåller diagnostisk information om det uppstår ett fel.</span><span class="sxs-lookup"><span data-stu-id="435e8-116">Service Fabric health monitoring detects if an application is running, and provides diagnostic information if there is a failure.</span></span>   
* <span data-ttu-id="435e8-117">Livscykeln för hantering.</span><span class="sxs-lookup"><span data-stu-id="435e8-117">Application lifecycle management.</span></span> <span data-ttu-id="435e8-118">Förutom att tillhandahålla uppgraderingar utan avbrott, tillhandahåller Service Fabric automatisk återställning toohello tidigare version om det finns en felaktig hälsohändelse som rapporteras vid en uppgradering.</span><span class="sxs-lookup"><span data-stu-id="435e8-118">Besides providing upgrades with no downtime, Service Fabric provides automatic rollback toohello previous version if there is a bad health event reported during an upgrade.</span></span>    
* <span data-ttu-id="435e8-119">Densitet.</span><span class="sxs-lookup"><span data-stu-id="435e8-119">Density.</span></span> <span data-ttu-id="435e8-120">Du kan köra flera program i ett kluster, vilket eliminerar hello behovet av att varje program toorun på sin egen maskinvara.</span><span class="sxs-lookup"><span data-stu-id="435e8-120">You can run multiple applications in a cluster, which eliminates hello need for each application toorun on its own hardware.</span></span>
* <span data-ttu-id="435e8-121">Synlighet: Med hjälp av REST kan du anropa hello Service Fabric Naming service toofind andra tjänster i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="435e8-121">Discoverability: Using REST you can call hello Service Fabric Naming service toofind other services in hello cluster.</span></span> 

## <a name="samples"></a><span data-ttu-id="435e8-122">Exempel</span><span class="sxs-lookup"><span data-stu-id="435e8-122">Samples</span></span>
* [<span data-ttu-id="435e8-123">Exempel för förpackning och distribution av en gäst körbar fil</span><span class="sxs-lookup"><span data-stu-id="435e8-123">Sample for packaging and deploying a guest executable</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="435e8-124">Exempel på två gäst körbara filer (C# och nodejs) kommunicera via hello Naming service med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="435e8-124">Sample of two guest executables (C# and nodejs) communicating via hello Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a><span data-ttu-id="435e8-125">Översikt över programmet och service manifest-filer</span><span class="sxs-lookup"><span data-stu-id="435e8-125">Overview of application and service manifest files</span></span>
<span data-ttu-id="435e8-126">Som del av distributionen av en gäst körbar fil är användbara toounderstand hello Service Fabric paketering och distributionsmodell som beskrivs i [programmodell](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="435e8-126">As part of deploying a guest executable, it is useful toounderstand hello Service Fabric packaging and deployment model as described in [application model](service-fabric-application-model.md).</span></span> <span data-ttu-id="435e8-127">hello Service Fabric paketering modellen förlitar sig på två XML-filer: hello applikationen eller tjänsten manifest.</span><span class="sxs-lookup"><span data-stu-id="435e8-127">hello Service Fabric packaging model relies on two XML files: hello application and service manifests.</span></span> <span data-ttu-id="435e8-128">hello schemadefinition för hello ApplicationManifest.xml och ServiceManifest.xml filer har installerats med hello Service Fabric SDK i *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="435e8-128">hello schema definition for hello ApplicationManifest.xml and ServiceManifest.xml files is installed with hello Service Fabric SDK into *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

* <span data-ttu-id="435e8-129">**Applikationsmanifestet** hello programmanifestet är används toodescribe hello program.</span><span class="sxs-lookup"><span data-stu-id="435e8-129">**Application manifest** hello application manifest is used toodescribe hello application.</span></span> <span data-ttu-id="435e8-130">Den visar hello-tjänster som ska utgöra den och andra parametrar som används toodefine hur en eller flera tjänster ska distribueras, till exempel hello antal instanser.</span><span class="sxs-lookup"><span data-stu-id="435e8-130">It lists hello services that compose it, and other parameters that are used toodefine how one or more services should be deployed, such as hello number of instances.</span></span>

  <span data-ttu-id="435e8-131">Ett program är en enhet av distribution och uppgradering i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="435e8-131">In Service Fabric, an application is a unit of deployment and upgrade.</span></span> <span data-ttu-id="435e8-132">Ett program kan uppgraderas som en enhet där potentiella problem och potentiella återställningar hanteras.</span><span class="sxs-lookup"><span data-stu-id="435e8-132">An application can be upgraded as a single unit where potential failures and potential rollbacks are managed.</span></span> <span data-ttu-id="435e8-133">Service Fabric garanterar att hello uppgraderingsprocessen är antingen lyckas, eller om hello uppgraderingen misslyckas, lämnar då inte hello program i ett okänt eller instabilt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="435e8-133">Service Fabric guarantees that hello upgrade process is either successful, or, if hello upgrade fails, does not leave hello application in an unknown or unstable state.</span></span>
* <span data-ttu-id="435e8-134">**Tjänstmanifestet** hello tjänstmanifestet beskriver hello komponenter av en tjänst.</span><span class="sxs-lookup"><span data-stu-id="435e8-134">**Service manifest** hello service manifest describes hello components of a service.</span></span> <span data-ttu-id="435e8-135">Den innehåller data, till exempel hello namn och typ av tjänst, och koden och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="435e8-135">It includes data, such as hello name and type of service, and its code and configuration.</span></span> <span data-ttu-id="435e8-136">hello tjänstmanifestet även vissa ytterligare parametrar som kan vara används tooconfigure hello-tjänsten när den har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="435e8-136">hello service manifest also includes some additional parameters that can be used tooconfigure hello service once it is deployed.</span></span>

## <a name="application-package-file-structure"></a><span data-ttu-id="435e8-137">Filstruktur för programmets paket</span><span class="sxs-lookup"><span data-stu-id="435e8-137">Application package file structure</span></span>
<span data-ttu-id="435e8-138">toodeploy ett program tooService Fabric hello program bör följa en fördefinierad katalogstruktur.</span><span class="sxs-lookup"><span data-stu-id="435e8-138">toodeploy an application tooService Fabric, hello application should follow a predefined directory structure.</span></span> <span data-ttu-id="435e8-139">hello följande är ett exempel på denna struktur.</span><span class="sxs-lookup"><span data-stu-id="435e8-139">hello following is an example of that structure.</span></span>

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

<span data-ttu-id="435e8-140">Hej ApplicationPackageRoot innehåller hello ApplicationManifest.xml-fil som definierar hello program.</span><span class="sxs-lookup"><span data-stu-id="435e8-140">hello ApplicationPackageRoot contains hello ApplicationManifest.xml file that defines hello application.</span></span> <span data-ttu-id="435e8-141">En underkatalog för varje tjänst som ingår i programmet hello är används toocontain alla hello artefakter hello tjänsten kräver.</span><span class="sxs-lookup"><span data-stu-id="435e8-141">A subdirectory for each service included in hello application is used toocontain all hello artifacts that hello service requires.</span></span> <span data-ttu-id="435e8-142">Dessa underkataloger är hello ServiceManifest.xml och normalt hello följande:</span><span class="sxs-lookup"><span data-stu-id="435e8-142">These subdirectories are hello ServiceManifest.xml and, typically, hello following:</span></span>

* <span data-ttu-id="435e8-143">*Koden*.</span><span class="sxs-lookup"><span data-stu-id="435e8-143">*Code*.</span></span> <span data-ttu-id="435e8-144">Den här katalogen innehåller hello Tjänstkod.</span><span class="sxs-lookup"><span data-stu-id="435e8-144">This directory contains hello service code.</span></span>
* <span data-ttu-id="435e8-145">*Config*. Den här katalogen innehåller en Settings.xml (och andra filer vid behov) att hello-tjänsten kan komma åt vid körning tooretrieve specifika konfigurationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="435e8-145">*Config*. This directory contains a Settings.xml file (and other files if necessary) that hello service can access at runtime tooretrieve specific configuration settings.</span></span>
* <span data-ttu-id="435e8-146">*Data*.</span><span class="sxs-lookup"><span data-stu-id="435e8-146">*Data*.</span></span> <span data-ttu-id="435e8-147">Det här är en ytterligare katalog toostore ytterligare lokala data som kan behöva hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="435e8-147">This is an additional directory toostore additional local data that hello service may need.</span></span> <span data-ttu-id="435e8-148">Data ska använda toostore endast tillfälliga data.</span><span class="sxs-lookup"><span data-stu-id="435e8-148">Data should be used toostore only ephemeral data.</span></span> <span data-ttu-id="435e8-149">Service Fabric stöder inte att kopiera eller replikera ändringar toohello datakatalog om hello-tjänsten måste toobe flyttas (till exempel under växling vid fel).</span><span class="sxs-lookup"><span data-stu-id="435e8-149">Service Fabric does not copy or replicate changes toohello data directory if hello service needs toobe relocated (for example, during failover).</span></span>

> [!NOTE]
> <span data-ttu-id="435e8-150">Du har inte toocreate hello `config` och `data` kataloger om du inte behöver dem.</span><span class="sxs-lookup"><span data-stu-id="435e8-150">You don't have toocreate hello `config` and `data` directories if you don't need them.</span></span>
>
>

## <a name="package-an-existing-executable"></a><span data-ttu-id="435e8-151">Paketet en befintlig körbar fil</span><span class="sxs-lookup"><span data-stu-id="435e8-151">Package an existing executable</span></span>
<span data-ttu-id="435e8-152">När Paketera en gäst körbar fil kan du välja antingen toouse en projektmall för Visual Studio eller för[skapa hello programpaket manuellt](#manually).</span><span class="sxs-lookup"><span data-stu-id="435e8-152">When packaging a guest executable, you can choose either toouse a Visual Studio project template or too[create hello application package manually](#manually).</span></span> <span data-ttu-id="435e8-153">Med Visual Studio hello programmet paketet struktur och manifest-filer skapas av hello ny projektmall för dig.</span><span class="sxs-lookup"><span data-stu-id="435e8-153">Using Visual Studio, hello application package structure and manifest files are created by hello new project template for you.</span></span>

> [!TIP]
> <span data-ttu-id="435e8-154">hello enklaste sättet toopackage en befintlig Windows körbara till en tjänst är toouse Visual Studio och på Linux toouse Yeoman</span><span class="sxs-lookup"><span data-stu-id="435e8-154">hello easiest way toopackage an existing Windows executable into a service is toouse Visual Studio and on Linux toouse Yeoman</span></span>
>

## <a name="use-visual-studio-toopackage-and-deploy-an-existing-executable"></a><span data-ttu-id="435e8-155">Använda Visual Studio toopackage och distribuera en befintlig körbar fil</span><span class="sxs-lookup"><span data-stu-id="435e8-155">Use Visual Studio toopackage and deploy an existing executable</span></span>
<span data-ttu-id="435e8-156">Visual Studio har ett Service Fabric service mallen toohelp du distribuera ett gästkluster körbara tooa Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="435e8-156">Visual Studio provides a Service Fabric service template toohelp you deploy a guest executable tooa Service Fabric cluster.</span></span>

1. <span data-ttu-id="435e8-157">Välj **filen** > **nytt projekt**, och skapa ett Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="435e8-157">Choose **File** > **New Project**, and create a Service Fabric application.</span></span>
2. <span data-ttu-id="435e8-158">Välj **gäst körbara** som mall för hello-tjänster.</span><span class="sxs-lookup"><span data-stu-id="435e8-158">Choose **Guest Executable** as hello service template.</span></span>
3. <span data-ttu-id="435e8-159">Klicka på **Bläddra** tooselect hello mappen med den körbara filen och Fyll i hello resten av hello parametrar toocreate hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="435e8-159">Click **Browse** tooselect hello folder with your executable and fill in hello rest of hello parameters toocreate hello service.</span></span>
   * <span data-ttu-id="435e8-160">*Code paketet beteende*.</span><span class="sxs-lookup"><span data-stu-id="435e8-160">*Code Package Behavior*.</span></span> <span data-ttu-id="435e8-161">Kan vara set toocopy alla hello innehållet i din mapp toohello Visual Studio-projekt, vilket är användbart om hello körbara inte ändras.</span><span class="sxs-lookup"><span data-stu-id="435e8-161">Can be set toocopy all hello content of your folder toohello Visual Studio Project, which is useful if hello executable does not change.</span></span> <span data-ttu-id="435e8-162">Om du förväntar dig hello körbara toochange och vill hello möjlighet toopick in nya versioner dynamiskt, kan du välja toolink toohello mapp i stället.</span><span class="sxs-lookup"><span data-stu-id="435e8-162">If you expect hello executable toochange and want hello ability toopick up new builds dynamically, you can choose toolink toohello folder instead.</span></span> <span data-ttu-id="435e8-163">Du kan använda länkade mappar när du skapar hello application-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="435e8-163">You can use linked folders when creating hello application project in Visual Studio.</span></span> <span data-ttu-id="435e8-164">Detta länkar toohello källplats från inom hello-projektet att göra det möjligt för du tooupdate hello gäst körbara i dess källa mål.</span><span class="sxs-lookup"><span data-stu-id="435e8-164">This links toohello source location from within hello project, making it possible for you tooupdate hello guest executable in its source destination.</span></span> <span data-ttu-id="435e8-165">Dessa uppdateringar blir en del av hello programpaket version.</span><span class="sxs-lookup"><span data-stu-id="435e8-165">Those updates become part of hello application package on build.</span></span>
   * <span data-ttu-id="435e8-166">*Programmet* anger hello körbar fil som ska köras toostart hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="435e8-166">*Program* specifies hello executable that should be run toostart hello service.</span></span>
   * <span data-ttu-id="435e8-167">*Argument* anger hello-argument som ska skickas toohello körbara.</span><span class="sxs-lookup"><span data-stu-id="435e8-167">*Arguments* specifies hello arguments that should be passed toohello executable.</span></span> <span data-ttu-id="435e8-168">Det kan vara en lista över parametrar med argument.</span><span class="sxs-lookup"><span data-stu-id="435e8-168">It can be a list of parameters with arguments.</span></span>
   * <span data-ttu-id="435e8-169">*WorkingFolder* anger hello arbetskatalog för hello-processen som ska toobe igång.</span><span class="sxs-lookup"><span data-stu-id="435e8-169">*WorkingFolder* specifies hello working directory for hello process that is going toobe started.</span></span> <span data-ttu-id="435e8-170">Du kan ange tre värden:</span><span class="sxs-lookup"><span data-stu-id="435e8-170">You can specify three values:</span></span>
     * <span data-ttu-id="435e8-171">`CodeBase`Anger att hello arbetskatalogen försätts toobe ange toohello kod katalog i hello-programpaket (`Code` directory som visas i föregående filstruktur hello).</span><span class="sxs-lookup"><span data-stu-id="435e8-171">`CodeBase` specifies that hello working directory is going toobe set toohello code directory in hello application package (`Code` directory shown in hello preceding file structure).</span></span>
     * <span data-ttu-id="435e8-172">`CodePackage`Anger att hello arbetskatalogen försätts toobe ange toohello rot hello-programpaket (`GuestService1Pkg` visas i föregående filstruktur hello).</span><span class="sxs-lookup"><span data-stu-id="435e8-172">`CodePackage` specifies that hello working directory is going toobe set toohello root of hello application package    (`GuestService1Pkg` shown in hello preceding file structure).</span></span>
     * <span data-ttu-id="435e8-173">`Work`Anger att hello filer placeras i en underkatalog med namnet arbete.</span><span class="sxs-lookup"><span data-stu-id="435e8-173">`Work` specifies that hello files are placed in a subdirectory called work.</span></span>
4. <span data-ttu-id="435e8-174">Namnge tjänsten och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="435e8-174">Give your service a name, and click **OK**.</span></span>
5. <span data-ttu-id="435e8-175">Om din tjänst måste en slutpunkt för kommunikation, kan du nu lägga till hello-protokollet, port och typen toohello ServiceManifest.xml filen.</span><span class="sxs-lookup"><span data-stu-id="435e8-175">If your service needs an endpoint for communication, you can now add hello protocol, port, and type toohello ServiceManifest.xml file.</span></span> <span data-ttu-id="435e8-176">Till exempel: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.</span><span class="sxs-lookup"><span data-stu-id="435e8-176">For example: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.</span></span>
6. <span data-ttu-id="435e8-177">Du kan nu använda hello paketet och publicera åtgärd mot ditt lokala kluster med felsökning hello lösningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="435e8-177">You can now use hello package and publish action against your local cluster by debugging hello solution in Visual Studio.</span></span> <span data-ttu-id="435e8-178">När du är klar kan du publicera programmet hello tooa fjärrkluster eller kontrollera hello lösning toosource kontrollen.</span><span class="sxs-lookup"><span data-stu-id="435e8-178">When ready, you can publish hello application tooa remote cluster or check in hello solution toosource control.</span></span>
7. <span data-ttu-id="435e8-179">Gå toohello slutet av den här artikeln toosee hur tooview din körbara gäst-tjänst körs i Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="435e8-179">Go toohello end of this article toosee how tooview your guest executable service running in Service Fabric Explorer.</span></span>

## <a name="use-yoeman-toopackage-and-deploy-an-existing-executable-on-linux"></a><span data-ttu-id="435e8-180">Använda Yoeman toopackage och distribuera en befintlig körbar fil på Linux</span><span class="sxs-lookup"><span data-stu-id="435e8-180">Use Yoeman toopackage and deploy an existing executable on Linux</span></span>

<span data-ttu-id="435e8-181">hello proceduren för att skapa och distribuera gäst körbara på Linux är hello samma som distribuerar ett csharp eller java-program.</span><span class="sxs-lookup"><span data-stu-id="435e8-181">hello procedure for creating and deploying a guest executable on Linux is hello same as deploying a csharp or java application.</span></span>

1. <span data-ttu-id="435e8-182">I en terminal, skriver du in `yo azuresfguest`.</span><span class="sxs-lookup"><span data-stu-id="435e8-182">In a terminal, type `yo azuresfguest`.</span></span>
2. <span data-ttu-id="435e8-183">Namnge ditt program.</span><span class="sxs-lookup"><span data-stu-id="435e8-183">Name your application.</span></span>
3. <span data-ttu-id="435e8-184">Namnge din tjänst och ange hello information, inklusive sökvägen till hello körbara och hello parametrar det måste anropas med.</span><span class="sxs-lookup"><span data-stu-id="435e8-184">Name your service, and provide hello details including path of hello executable and hello parameters it must be invoked with.</span></span>

<span data-ttu-id="435e8-185">Yeoman skapar ett programpaket med lämpliga hello-program och manifestfiler tillsammans med installera och avinstallera skript.</span><span class="sxs-lookup"><span data-stu-id="435e8-185">Yeoman creates an application package with hello appropriate application and manifest files along with install and uninstall scripts.</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a><span data-ttu-id="435e8-186">Paketera och distribuera en befintlig körbar fil manuellt</span><span class="sxs-lookup"><span data-stu-id="435e8-186">Manually package and deploy an existing executable</span></span>
<span data-ttu-id="435e8-187">hello processen om paketeringen manuellt av en gäst körbar fil baserat på hello följande allmänna steg:</span><span class="sxs-lookup"><span data-stu-id="435e8-187">hello process of manually packaging a guest executable is based on hello following general steps:</span></span>

1. <span data-ttu-id="435e8-188">Skapa hello paketet katalogstruktur.</span><span class="sxs-lookup"><span data-stu-id="435e8-188">Create hello package directory structure.</span></span>
2. <span data-ttu-id="435e8-189">Lägg till hello-kod och configuration-filer.</span><span class="sxs-lookup"><span data-stu-id="435e8-189">Add hello application's code and configuration files.</span></span>
3. <span data-ttu-id="435e8-190">Redigera hello service manifest-filen.</span><span class="sxs-lookup"><span data-stu-id="435e8-190">Edit hello service manifest file.</span></span>
4. <span data-ttu-id="435e8-191">Redigera hello programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="435e8-191">Edit hello application manifest file.</span></span>

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you toocreate hello ApplicationPackage automatically. hello tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-hello-package-directory-structure"></a><span data-ttu-id="435e8-192">Skapa katalogstruktur för hello-paket</span><span class="sxs-lookup"><span data-stu-id="435e8-192">Create hello package directory structure</span></span>
<span data-ttu-id="435e8-193">Du kan börja med att skapa hello katalogstrukturen, enligt beskrivningen i föregående avsnitt, hello ”programmet paketet filstruktur”.</span><span class="sxs-lookup"><span data-stu-id="435e8-193">You can start by creating hello directory structure, as described in hello preceding section, "Application package file structure."</span></span>

### <a name="add-hello-applications-code-and-configuration-files"></a><span data-ttu-id="435e8-194">Lägg till hello-koden och konfigurationen filer</span><span class="sxs-lookup"><span data-stu-id="435e8-194">Add hello application's code and configuration files</span></span>
<span data-ttu-id="435e8-195">När du har skapat hello katalogstrukturen, du kan lägga till hello programmet koden och konfigurationen under hello koden och konfigurationsversionerna kataloger.</span><span class="sxs-lookup"><span data-stu-id="435e8-195">After you have created hello directory structure, you can add hello application's code and configuration files under hello code and config directories.</span></span> <span data-ttu-id="435e8-196">Du kan också skapa ytterligare kataloger eller underkataloger på hello kod eller konfigurationsversion kataloger.</span><span class="sxs-lookup"><span data-stu-id="435e8-196">You can also create additional directories or subdirectories under hello code or config directories.</span></span>

<span data-ttu-id="435e8-197">Service Fabric har en `xcopy` av hello i hello tillämpningsprogrammets rotkatalog, så det finns inga fördefinierade strukturen toouse än att skapa två översta kataloger, kod och inställningar.</span><span class="sxs-lookup"><span data-stu-id="435e8-197">Service Fabric does an `xcopy` of hello content of hello application root directory, so there is no predefined structure toouse other than creating two top directories, code and settings.</span></span> <span data-ttu-id="435e8-198">(Du kan välja olika namn om du vill.</span><span class="sxs-lookup"><span data-stu-id="435e8-198">(You can pick different names if you want.</span></span> <span data-ttu-id="435e8-199">Mer information finns i hello nästa avsnitt.)</span><span class="sxs-lookup"><span data-stu-id="435e8-199">More details are in hello next section.)</span></span>

> [!NOTE]
> <span data-ttu-id="435e8-200">Kontrollera att du inkluderar alla hello filer och beroenden som hello programbehov.</span><span class="sxs-lookup"><span data-stu-id="435e8-200">Make sure that you include all hello files and dependencies that hello application needs.</span></span> <span data-ttu-id="435e8-201">Service Fabric kopierar hello innehåll i hello programpaket på alla noder i hello kluster där hello programtjänster är pågående toobe distribueras.</span><span class="sxs-lookup"><span data-stu-id="435e8-201">Service Fabric copies hello content of hello application package on all nodes in hello cluster where hello application's services are going toobe deployed.</span></span> <span data-ttu-id="435e8-202">hello paketet innehåller alla hello kod att programmet hello måste toorun.</span><span class="sxs-lookup"><span data-stu-id="435e8-202">hello package should contain all hello code that hello application needs toorun.</span></span> <span data-ttu-id="435e8-203">Förutsätter inte att hello beroenden redan är installerade.</span><span class="sxs-lookup"><span data-stu-id="435e8-203">Do not assume that hello dependencies are already installed.</span></span>
>
>

### <a name="edit-hello-service-manifest-file"></a><span data-ttu-id="435e8-204">Redigera hello service manifest-filen</span><span class="sxs-lookup"><span data-stu-id="435e8-204">Edit hello service manifest file</span></span>
<span data-ttu-id="435e8-205">hello nästa steg är tooedit hello service manifestfilen tooinclude hello följande information:</span><span class="sxs-lookup"><span data-stu-id="435e8-205">hello next step is tooedit hello service manifest file tooinclude hello following information:</span></span>

* <span data-ttu-id="435e8-206">hello namnet på hello service-typen.</span><span class="sxs-lookup"><span data-stu-id="435e8-206">hello name of hello service type.</span></span> <span data-ttu-id="435e8-207">Detta är ett ID som använder Service Fabric tooidentify en tjänst.</span><span class="sxs-lookup"><span data-stu-id="435e8-207">This is an ID that Service Fabric uses tooidentify a service.</span></span>
* <span data-ttu-id="435e8-208">hello kommandot toouse toolaunch hello program (ExeHost).</span><span class="sxs-lookup"><span data-stu-id="435e8-208">hello command toouse toolaunch hello application (ExeHost).</span></span>
* <span data-ttu-id="435e8-209">Alla skript som behöver toobe kör tooset in hello program (SetupEntrypoint).</span><span class="sxs-lookup"><span data-stu-id="435e8-209">Any script that needs toobe run tooset up hello application (SetupEntrypoint).</span></span>

<span data-ttu-id="435e8-210">hello följande är ett exempel på en `ServiceManifest.xml` fil:</span><span class="sxs-lookup"><span data-stu-id="435e8-210">hello following is an example of a `ServiceManifest.xml` file:</span></span>

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

<span data-ttu-id="435e8-211">följande avsnitt hello gå igenom hello olika delar av hello-fil som du behöver tooupdate.</span><span class="sxs-lookup"><span data-stu-id="435e8-211">hello following sections go over hello different parts of hello file that you need tooupdate.</span></span>

#### <a name="update-servicetypes"></a><span data-ttu-id="435e8-212">Uppdatera ServiceTypes</span><span class="sxs-lookup"><span data-stu-id="435e8-212">Update ServiceTypes</span></span>
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* <span data-ttu-id="435e8-213">Du kan välja vilket namn som du vill använda för `ServiceTypeName`.</span><span class="sxs-lookup"><span data-stu-id="435e8-213">You can pick any name that you want for `ServiceTypeName`.</span></span> <span data-ttu-id="435e8-214">hello värdet används i hello `ApplicationManifest.xml` filen tooidentify hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="435e8-214">hello value is used in hello `ApplicationManifest.xml` file tooidentify hello service.</span></span>
* <span data-ttu-id="435e8-215">Ange `UseImplicitHost="true"`.</span><span class="sxs-lookup"><span data-stu-id="435e8-215">Specify `UseImplicitHost="true"`.</span></span> <span data-ttu-id="435e8-216">Det här attributet anger Service Fabric att hello tjänsten baserat på en fristående app, så måste alla Service Fabric toodo är toolaunch den som en process och övervaka dess hälsa.</span><span class="sxs-lookup"><span data-stu-id="435e8-216">This attribute tells Service Fabric that hello service is based on a self-contained app, so all Service Fabric needs toodo is toolaunch it as a process and monitor its health.</span></span>

#### <a name="update-codepackage"></a><span data-ttu-id="435e8-217">Uppdatera CodePackage</span><span class="sxs-lookup"><span data-stu-id="435e8-217">Update CodePackage</span></span>
<span data-ttu-id="435e8-218">Hej CodePackage element anger hello plats (och version) av hello service-kod.</span><span class="sxs-lookup"><span data-stu-id="435e8-218">hello CodePackage element specifies hello location (and version) of hello service's code.</span></span>

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

<span data-ttu-id="435e8-219">Hej `Name` element är används toospecify hello namn i hello katalog i hello programpaket som innehåller hello service-kod.</span><span class="sxs-lookup"><span data-stu-id="435e8-219">hello `Name` element is used toospecify hello name of hello directory in hello application package that contains hello service's code.</span></span> <span data-ttu-id="435e8-220">`CodePackage`har också hello `version` attribut.</span><span class="sxs-lookup"><span data-stu-id="435e8-220">`CodePackage` also has hello `version` attribute.</span></span> <span data-ttu-id="435e8-221">Detta kan vara används toospecify hello version av hello kod och kan också vara tooupgrade hello service koden används med hjälp av hello livscykel infrastruktur för hantering i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="435e8-221">This can be used toospecify hello version of hello code, and can also potentially be used tooupgrade hello service's code by using hello application lifecycle management infrastructure in Service Fabric.</span></span>

#### <a name="optional-update-setupentrypoint"></a><span data-ttu-id="435e8-222">Valfritt: Uppdatera SetupEntrypoint</span><span class="sxs-lookup"><span data-stu-id="435e8-222">Optional: Update SetupEntrypoint</span></span>
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
<span data-ttu-id="435e8-223">Hej SetupEntryPoint element är används toospecify en körbar fil eller en batch-fil som ska köras innan hello service-koden har startats.</span><span class="sxs-lookup"><span data-stu-id="435e8-223">hello SetupEntryPoint element is used toospecify any executable or batch file that should be executed before hello service's code is launched.</span></span> <span data-ttu-id="435e8-224">Det är ett valfritt steg så inte behöver toobe om det finns inga initiering krävs.</span><span class="sxs-lookup"><span data-stu-id="435e8-224">It is an optional step, so it does not need toobe included if there is no initialization required.</span></span> <span data-ttu-id="435e8-225">Hej SetupEntryPoint körs varje gång hello-tjänsten startas.</span><span class="sxs-lookup"><span data-stu-id="435e8-225">hello SetupEntryPoint is executed every time hello service is restarted.</span></span>

<span data-ttu-id="435e8-226">Det finns endast en SetupEntryPoint installationsskript måste toobe grupperade i en enda kommandofil om hello programinställningar kräver flera skript.</span><span class="sxs-lookup"><span data-stu-id="435e8-226">There is only one SetupEntryPoint, so setup scripts need toobe grouped in a single batch file if hello application's setup requires multiple scripts.</span></span> <span data-ttu-id="435e8-227">Hej SetupEntryPoint kan köra alla typer av filer: körbara filer, batch-filer och PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="435e8-227">hello SetupEntryPoint can execute any type of file: executable files, batch files, and PowerShell cmdlets.</span></span> <span data-ttu-id="435e8-228">Mer information finns i [konfigurera SetupEntryPoint](service-fabric-application-runas-security.md).</span><span class="sxs-lookup"><span data-stu-id="435e8-228">For more details, see [Configure SetupEntryPoint](service-fabric-application-runas-security.md).</span></span>

<span data-ttu-id="435e8-229">I föregående exempel hello, hello SetupEntryPoint körs en batchfil som kallas `LaunchConfig.cmd` som finns i hello `scripts` hello kod-mappen (förutsatt att hello WorkingFolder element anges tooCodeBase).</span><span class="sxs-lookup"><span data-stu-id="435e8-229">In hello preceding example, hello SetupEntryPoint runs a batch file called `LaunchConfig.cmd` that is located in hello `scripts` subdirectory of hello code directory (assuming hello WorkingFolder element is set tooCodeBase).</span></span>

#### <a name="update-entrypoint"></a><span data-ttu-id="435e8-230">Uppdatera EntryPoint</span><span class="sxs-lookup"><span data-stu-id="435e8-230">Update EntryPoint</span></span>
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

<span data-ttu-id="435e8-231">Hej `EntryPoint` -elementet i hello service manifest-filen är används toospecify hur toolaunch hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="435e8-231">hello `EntryPoint` element in hello service manifest file is used toospecify how toolaunch hello service.</span></span> <span data-ttu-id="435e8-232">hello `ExeHost` element anger hello körbara (och argument) som ska användas toolaunch hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="435e8-232">hello `ExeHost` element specifies hello executable (and arguments) that should be used toolaunch hello service.</span></span>

* <span data-ttu-id="435e8-233">`Program`Anger hello för hello körbar fil som ska starta hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="435e8-233">`Program` specifies hello name of hello executable that should start hello service.</span></span>
* <span data-ttu-id="435e8-234">`Arguments`Anger hello-argument som ska skickas toohello körbara.</span><span class="sxs-lookup"><span data-stu-id="435e8-234">`Arguments` specifies hello arguments that should be passed toohello executable.</span></span> <span data-ttu-id="435e8-235">Det kan vara en lista över parametrar med argument.</span><span class="sxs-lookup"><span data-stu-id="435e8-235">It can be a list of parameters with arguments.</span></span>
* <span data-ttu-id="435e8-236">`WorkingFolder`Anger hello arbetskatalog för hello-processen som ska toobe igång.</span><span class="sxs-lookup"><span data-stu-id="435e8-236">`WorkingFolder` specifies hello working directory for hello process that is going toobe started.</span></span> <span data-ttu-id="435e8-237">Du kan ange tre värden:</span><span class="sxs-lookup"><span data-stu-id="435e8-237">You can specify three values:</span></span>
  * <span data-ttu-id="435e8-238">`CodeBase`Anger att hello arbetskatalogen försätts toobe ange toohello kod katalog i hello-programpaket (`Code` katalogen i hello föregående filstruktur).</span><span class="sxs-lookup"><span data-stu-id="435e8-238">`CodeBase` specifies that hello working directory is going toobe set toohello code directory in hello application package (`Code` directory in hello preceding file structure).</span></span>
  * <span data-ttu-id="435e8-239">`CodePackage`Anger att hello arbetskatalogen försätts toobe ange toohello rot hello-programpaket (`GuestService1Pkg` i hello föregående filstruktur).</span><span class="sxs-lookup"><span data-stu-id="435e8-239">`CodePackage` specifies that hello working directory is going toobe set toohello root of hello application package    (`GuestService1Pkg` in hello preceding file structure).</span></span>
    * <span data-ttu-id="435e8-240">`Work`Anger att hello filer placeras i en underkatalog med namnet arbete.</span><span class="sxs-lookup"><span data-stu-id="435e8-240">`Work` specifies that hello files are placed in a subdirectory called work.</span></span>

<span data-ttu-id="435e8-241">Hej WorkingFolder är användbara tooset hello rätt arbetskatalogen så att relativa sökvägar kan användas av antingen hello program eller initiering skript.</span><span class="sxs-lookup"><span data-stu-id="435e8-241">hello WorkingFolder is useful tooset hello correct working directory so that relative paths can be used by either hello application or initialization scripts.</span></span>

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a><span data-ttu-id="435e8-242">Uppdatera slutpunkter och registrera med namngivningstjänst för kommunikation</span><span class="sxs-lookup"><span data-stu-id="435e8-242">Update Endpoints and register with Naming Service for communication</span></span>
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
<span data-ttu-id="435e8-243">I föregående exempel hello, hello `Endpoint` element anger hello-slutpunkter som hello programmet kan lyssna på.</span><span class="sxs-lookup"><span data-stu-id="435e8-243">In hello preceding example, hello `Endpoint` element specifies hello endpoints that hello application can listen on.</span></span> <span data-ttu-id="435e8-244">I det här exemplet hello Node.js-program som lyssnar på http på 3000-port.</span><span class="sxs-lookup"><span data-stu-id="435e8-244">In this example, hello Node.js application listens on http on port 3000.</span></span>

<span data-ttu-id="435e8-245">Dessutom be du Service Fabric toopublish den här slutpunkten toohello Naming Service så att andra tjänster kan identifiera hello endpoint adress toothis service.</span><span class="sxs-lookup"><span data-stu-id="435e8-245">Furthermore you can ask Service Fabric toopublish this endpoint toohello Naming Service so other services can discover hello endpoint address toothis service.</span></span> <span data-ttu-id="435e8-246">Detta gör att du toobe kan toocommunicate mellan tjänster som gäst körbara filer.</span><span class="sxs-lookup"><span data-stu-id="435e8-246">This enables you toobe able toocommunicate between services that are guest executables.</span></span>
<span data-ttu-id="435e8-247">hello publicerade slutpunktsadress har hello form `UriScheme://IPAddressOrFQDN:Port/PathSuffix`.</span><span class="sxs-lookup"><span data-stu-id="435e8-247">hello published endpoint address is of hello form `UriScheme://IPAddressOrFQDN:Port/PathSuffix`.</span></span> <span data-ttu-id="435e8-248">`UriScheme`och `PathSuffix` är valfria attribut.</span><span class="sxs-lookup"><span data-stu-id="435e8-248">`UriScheme` and `PathSuffix` are optional attributes.</span></span> <span data-ttu-id="435e8-249">`IPAddressOrFQDN`är hello IP-adress eller fullständigt domännamn för hello nod den körbara filen hämtar placerad och den beräknas åt dig.</span><span class="sxs-lookup"><span data-stu-id="435e8-249">`IPAddressOrFQDN` is hello IP address or fully qualified domain name of hello node this executable gets placed on, and it is calculated for you.</span></span>

<span data-ttu-id="435e8-250">I hello följande exempel visas en gång hello-tjänsten har distribuerats, Service Fabric Explorer visas i en liknande slutpunkt för`http://10.1.4.92:3000/myapp/` publicerats för hello tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="435e8-250">In hello following example, once hello service is deployed, in Service Fabric Explorer you see an endpoint similar too`http://10.1.4.92:3000/myapp/` published for hello service instance.</span></span> <span data-ttu-id="435e8-251">Eller om det är en lokal dator kan du se `http://localhost:3000/myapp/`.</span><span class="sxs-lookup"><span data-stu-id="435e8-251">Or if this is a local machine, you see `http://localhost:3000/myapp/`.</span></span>

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
<span data-ttu-id="435e8-252">Du kan använda dessa adresser med [omvänd proxy](service-fabric-reverseproxy.md) toocommunicate olika tjänster.</span><span class="sxs-lookup"><span data-stu-id="435e8-252">You can use these addresses with [reverse proxy](service-fabric-reverseproxy.md) toocommunicate between services.</span></span>

### <a name="edit-hello-application-manifest-file"></a><span data-ttu-id="435e8-253">Redigera hello programmanifestfilen</span><span class="sxs-lookup"><span data-stu-id="435e8-253">Edit hello application manifest file</span></span>
<span data-ttu-id="435e8-254">När du har konfigurerat hello `Servicemanifest.xml` filen, behöver du toomake vissa ändringar toohello `ApplicationManifest.xml` filen tooensure hello rätt typ av tjänst och namnet används.</span><span class="sxs-lookup"><span data-stu-id="435e8-254">Once you have configured hello `Servicemanifest.xml` file, you need toomake some changes toohello `ApplicationManifest.xml` file tooensure that hello correct service type and name are used.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a><span data-ttu-id="435e8-255">ServiceManifestImport</span><span class="sxs-lookup"><span data-stu-id="435e8-255">ServiceManifestImport</span></span>
<span data-ttu-id="435e8-256">I hello `ServiceManifestImport` element, kan du ange en eller flera tjänster som du vill tooinclude i hello app.</span><span class="sxs-lookup"><span data-stu-id="435e8-256">In hello `ServiceManifestImport` element, you can specify one or more services that you want tooinclude in hello app.</span></span> <span data-ttu-id="435e8-257">Tjänster refereras med `ServiceManifestName`, vilket anger hello namnet på hello katalog där hello `ServiceManifest.xml` filen finns.</span><span class="sxs-lookup"><span data-stu-id="435e8-257">Services are referenced with `ServiceManifestName`, which specifies hello name of hello directory where hello `ServiceManifest.xml` file is located.</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a><span data-ttu-id="435e8-258">Konfigurera loggning</span><span class="sxs-lookup"><span data-stu-id="435e8-258">Set up logging</span></span>
<span data-ttu-id="435e8-259">Gästen körbara filer är det användbart toobe kan toosee konsolen loggar toofind ut om hello program och konfiguration skript visar eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="435e8-259">For guest executables, it is useful toobe able toosee console logs toofind out if hello application and configuration scripts show any errors.</span></span>
<span data-ttu-id="435e8-260">Omdirigering av konsol kan konfigureras i hello `ServiceManifest.xml` fil med hello `ConsoleRedirection` element.</span><span class="sxs-lookup"><span data-stu-id="435e8-260">Console redirection can be configured in hello `ServiceManifest.xml` file using hello `ConsoleRedirection` element.</span></span>

> [!WARNING]
> <span data-ttu-id="435e8-261">Använd aldrig hello konsolen omdirigeringspolicyn i ett program som distribuerats i produktionsmiljön eftersom detta kan påverka hello programmet redundans.</span><span class="sxs-lookup"><span data-stu-id="435e8-261">Never use hello console redirection policy in an application that is deployed in production because this can affect hello application failover.</span></span> <span data-ttu-id="435e8-262">*Endast* använda detta för lokal utveckling och felsökning.</span><span class="sxs-lookup"><span data-stu-id="435e8-262">*Only* use this for local development and debugging purposes.</span></span>  
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

<span data-ttu-id="435e8-263">`ConsoleRedirection`kan vara används tooredirect konsolen utdata (stdout och stderr) tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="435e8-263">`ConsoleRedirection` can be used tooredirect console output (both stdout and stderr) tooa working directory.</span></span> <span data-ttu-id="435e8-264">Detta ger hello möjlighet tooverify att det inte finns några fel vid hello installation eller körning av programmet hello i hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="435e8-264">This provides hello ability tooverify that there are no errors during hello setup or execution of hello application in hello Service Fabric cluster.</span></span>

<span data-ttu-id="435e8-265">`FileRetentionCount`Anger hur många filer sparas i hello arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="435e8-265">`FileRetentionCount` determines how many files are saved in hello working directory.</span></span> <span data-ttu-id="435e8-266">Värdet 5, till exempel innebär att hello loggfiler för hello tidigare fem körningar lagras i hello arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="435e8-266">A value of 5, for example, means that hello log files for hello previous five executions are stored in hello working directory.</span></span>

<span data-ttu-id="435e8-267">`FileMaxSizeInKb`Anger hello maxstorleken för hello loggfiler.</span><span class="sxs-lookup"><span data-stu-id="435e8-267">`FileMaxSizeInKb` specifies hello maximum size of hello log files.</span></span>

<span data-ttu-id="435e8-268">Loggfilerna sparas i en av hello-tjänsten fungerar kataloger.</span><span class="sxs-lookup"><span data-stu-id="435e8-268">Log files are saved in one of hello service's working directories.</span></span> <span data-ttu-id="435e8-269">toodetermine där hello filer finns, Använd Service Fabric Explorer toodetermine vilken nod hello-tjänsten körs på och vilka arbetskatalogen används.</span><span class="sxs-lookup"><span data-stu-id="435e8-269">toodetermine where hello files are located, use Service Fabric Explorer toodetermine which node hello service is running on, and which working directory is being used.</span></span> <span data-ttu-id="435e8-270">Den här processen beskrivs senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="435e8-270">This process is covered later in this article.</span></span>

## <a name="deployment"></a><span data-ttu-id="435e8-271">Distribution</span><span class="sxs-lookup"><span data-stu-id="435e8-271">Deployment</span></span>
<span data-ttu-id="435e8-272">hello sista steget är för[distribuera programmet](service-fabric-deploy-remove-applications.md).</span><span class="sxs-lookup"><span data-stu-id="435e8-272">hello last step is too[deploy your application](service-fabric-deploy-remove-applications.md).</span></span> <span data-ttu-id="435e8-273">Hej följande PowerShell-skript visar hur toodeploy dina program toohello lokal utveckling klustret och starta en ny Service Fabric-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="435e8-273">hello following PowerShell script shows how toodeploy your application toohello local development cluster, and start a new Service Fabric service.</span></span>

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
> <span data-ttu-id="435e8-274">[Komprimera hello paketet](service-fabric-package-apps.md#compress-a-package) innan du kopierar toohello avbildningsarkivet om hello paketet är stora eller många filer.</span><span class="sxs-lookup"><span data-stu-id="435e8-274">[Compress hello package](service-fabric-package-apps.md#compress-a-package) before copying toohello image store if hello package is large or has many files.</span></span> <span data-ttu-id="435e8-275">Läs mer [här](service-fabric-deploy-remove-applications.md#upload-the-application-package).</span><span class="sxs-lookup"><span data-stu-id="435e8-275">Read more [here](service-fabric-deploy-remove-applications.md#upload-the-application-package).</span></span>
>

<span data-ttu-id="435e8-276">Ett Service Fabric-tjänsten kan distribueras i olika ”konfigurationer”.</span><span class="sxs-lookup"><span data-stu-id="435e8-276">A Service Fabric service can be deployed in various "configurations."</span></span> <span data-ttu-id="435e8-277">Till exempel den kan distribueras som en eller flera instanser eller mallen kan distribueras på ett sådant sätt att det finns en instans av hello-tjänsten på varje nod i hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="435e8-277">For example, it can be deployed as single or multiple instances, or it can be deployed in such a way that there is one instance of hello service on each node of hello Service Fabric cluster.</span></span>

<span data-ttu-id="435e8-278">Hej `InstanceCount` parametern för hello `New-ServiceFabricService` cmdlet har använt toospecify hur många instanser av hello tjänst ska startas i hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="435e8-278">hello `InstanceCount` parameter of hello `New-ServiceFabricService` cmdlet is used toospecify how many instances of hello service should be launched in hello Service Fabric cluster.</span></span> <span data-ttu-id="435e8-279">Du kan ange hello `InstanceCount` värde, beroende på hello typ av program som du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="435e8-279">You can set hello `InstanceCount` value, depending on hello type of application that you are deploying.</span></span> <span data-ttu-id="435e8-280">hello två vanligaste scenarierna är:</span><span class="sxs-lookup"><span data-stu-id="435e8-280">hello two most common scenarios are:</span></span>

* <span data-ttu-id="435e8-281">`InstanceCount = "1"`.</span><span class="sxs-lookup"><span data-stu-id="435e8-281">`InstanceCount = "1"`.</span></span> <span data-ttu-id="435e8-282">Endast en instans av hello tjänsten distribueras i det här fallet i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="435e8-282">In this case, only one instance of hello service is deployed in hello cluster.</span></span> <span data-ttu-id="435e8-283">Service Fabric scheduler avgör vilken nod hello tjänsten kommer toobe distribueras på.</span><span class="sxs-lookup"><span data-stu-id="435e8-283">Service Fabric's scheduler determines which node hello service is going toobe deployed on.</span></span>
* <span data-ttu-id="435e8-284">`InstanceCount ="-1"`.</span><span class="sxs-lookup"><span data-stu-id="435e8-284">`InstanceCount ="-1"`.</span></span> <span data-ttu-id="435e8-285">I det här fallet distribueras en instans av hello tjänsten på varje nod i hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="435e8-285">In this case, one instance of hello service is deployed on every node in hello Service Fabric cluster.</span></span> <span data-ttu-id="435e8-286">hello resultatet har ett (och endast ett) instans av hello-tjänsten för varje nod i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="435e8-286">hello result is having one (and only one) instance of hello service for each node in hello cluster.</span></span>

<span data-ttu-id="435e8-287">Detta är en användbar konfiguration för frontend-program (till exempel en REST-slutpunkt), eftersom klientprogram måste för ”ansluta” tooany hello noder i hello klustret toouse hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="435e8-287">This is a useful configuration for front-end applications (for example, a REST endpoint), because client applications need too"connect" tooany of hello nodes in hello cluster toouse hello endpoint.</span></span> <span data-ttu-id="435e8-288">Den här konfigurationen kan också användas när alla noder i hello Service Fabric-kluster är till exempel anslutna tooa belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="435e8-288">This configuration can also be used when, for example, all nodes of hello Service Fabric cluster are connected tooa load balancer.</span></span> <span data-ttu-id="435e8-289">Klienttrafik kan sedan distribueras över hello-tjänst som körs på alla noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="435e8-289">Client traffic can then be distributed across hello service that is running on all nodes in hello cluster.</span></span>

## <a name="check-your-running-application"></a><span data-ttu-id="435e8-290">Kontrollera att programmet körs</span><span class="sxs-lookup"><span data-stu-id="435e8-290">Check your running application</span></span>
<span data-ttu-id="435e8-291">I Service Fabric Explorer identifiera hello-nod där hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="435e8-291">In Service Fabric Explorer, identify hello node where hello service is running.</span></span> <span data-ttu-id="435e8-292">I det här exemplet körs den på Nod1:</span><span class="sxs-lookup"><span data-stu-id="435e8-292">In this example, it runs on Node1:</span></span>

![Noden som tjänsten körs](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

<span data-ttu-id="435e8-294">Om du navigerar toohello nod och bläddra toohello program, kan du se hello väsentliga nod information, inklusive dess plats på disken.</span><span class="sxs-lookup"><span data-stu-id="435e8-294">If you navigate toohello node and browse toohello application, you see hello essential node information, including its location on disk.</span></span>

![Plats på disken](./media/service-fabric-deploy-existing-app/locationondisk2.png)

<span data-ttu-id="435e8-296">Om du bläddrar toohello directory med hjälp av Server Explorer hittar du hello arbetskatalogen och hello service loggmappen, som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="435e8-296">If you browse toohello directory by using Server Explorer, you can find hello working directory and hello service's log folder, as shown in hello following screenshot:</span></span> 

![Plats för logg](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a><span data-ttu-id="435e8-298">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="435e8-298">Next steps</span></span>
<span data-ttu-id="435e8-299">I den här artikeln har du lärt dig hur toopackage en gäst körbar fil och distribuera den tooService Fabric.</span><span class="sxs-lookup"><span data-stu-id="435e8-299">In this article, you have learned how toopackage a guest executable and deploy it tooService Fabric.</span></span> <span data-ttu-id="435e8-300">Se följande artiklar för relaterad information och uppgifter hello.</span><span class="sxs-lookup"><span data-stu-id="435e8-300">See hello following articles for related information and tasks.</span></span>

* <span data-ttu-id="435e8-301">[Exempel för förpackning och distribution av en gäst körbar fil](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), inklusive en länk toohello förhandsversion av hello paketering verktyget</span><span class="sxs-lookup"><span data-stu-id="435e8-301">[Sample for packaging and deploying a guest executable](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), including a link toohello prerelease of hello packaging tool</span></span>
* [<span data-ttu-id="435e8-302">Exempel på två gäst körbara filer (C# och nodejs) kommunicera via hello Naming service med hjälp av REST</span><span class="sxs-lookup"><span data-stu-id="435e8-302">Sample of two guest executables (C# and nodejs) communicating via hello Naming service using REST</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
* [<span data-ttu-id="435e8-303">Distribuera flera körbara gäster</span><span class="sxs-lookup"><span data-stu-id="435e8-303">Deploy multiple guest executables</span></span>](service-fabric-deploy-multiple-apps.md)
* [<span data-ttu-id="435e8-304">Skapa ditt första Service Fabric-program med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="435e8-304">Create your first Service Fabric application using Visual Studio</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
