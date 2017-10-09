---
title: "aaaHow toocontainerize din Azure Service Fabric-mikrotjänster (förhandsgranskning)"
description: "Azure Service Fabric har lagt till nya funktioner toocontainerize Service Fabric-mikrotjänster. Den här funktionen är för närvarande en förhandsversion."
services: service-fabric
documentationcenter: .net
author: anmolah
manager: anmolah
editor: anmolah
ms.assetid: 0b41efb3-4063-4600-89f5-b077ea81fa3a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/04/2017
ms.author: anmola
ms.openlocfilehash: 6edaff73c0828707c7fa736669ba8084663d31ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocontainerize-your-service-fabric-reliable-services-and-reliable-actors-preview"></a><span data-ttu-id="54fae-104">Hur toocontainerize din Service Fabric tillförlitlig Services och Reliable Actors (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="54fae-104">How toocontainerize your Service Fabric Reliable Services and Reliable Actors (Preview)</span></span>

<span data-ttu-id="54fae-105">Service Fabric stöder containerizing Service Fabric-mikrotjänster (Reliable Services och tillförlitlig baserat aktörstjänster).</span><span class="sxs-lookup"><span data-stu-id="54fae-105">Service Fabric supports containerizing Service Fabric microservices (Reliable Services, and Reliable Actor based services).</span></span> <span data-ttu-id="54fae-106">Mer information finns i [service fabric-behållare](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="54fae-106">For more information, see [service fabric containers](service-fabric-containers-overview.md).</span></span>


 <span data-ttu-id="54fae-107">Den här funktionen är i förhandsversionen och den här artikeln innehåller hello olika steg tooget din tjänst som körs i en behållare.</span><span class="sxs-lookup"><span data-stu-id="54fae-107">This feature is in preview and this article provides hello various steps tooget your service running inside a container.</span></span>  

> [!NOTE]
> <span data-ttu-id="54fae-108">Den här funktionen är i förhandsvisning och stöds inte i produktion.</span><span class="sxs-lookup"><span data-stu-id="54fae-108">This feature is in preview and is not supported in production.</span></span> <span data-ttu-id="54fae-109">Den här funktionen fungerar för närvarande endast för Windows.</span><span class="sxs-lookup"><span data-stu-id="54fae-109">Currently this feature only works for Windows.</span></span>

## <a name="steps-toocontainerize-your-service-fabric-application"></a><span data-ttu-id="54fae-110">Steg toocontainerize tillämpningsprogrammet Service Fabric</span><span class="sxs-lookup"><span data-stu-id="54fae-110">Steps toocontainerize your Service Fabric Application</span></span>

1. <span data-ttu-id="54fae-111">Öppna Service Fabric-program i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54fae-111">Open your Service Fabric application in Visual Studio.</span></span>

2. <span data-ttu-id="54fae-112">Lägg till klass [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="54fae-112">Add class [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) tooyour project.</span></span> <span data-ttu-id="54fae-113">hello koden i den här klassen är en helper toocorrectly belastningen hello Service Fabric runtime-binärfiler i ditt program vid körning i en behållare.</span><span class="sxs-lookup"><span data-stu-id="54fae-113">hello code in this class is a helper toocorrectly load hello Service Fabric runtime binaries inside your application when running inside a container.</span></span>

3. <span data-ttu-id="54fae-114">För varje kodpaketet som toocontainerize, initiera hello inläsaren på hello startpunkt för programmet.</span><span class="sxs-lookup"><span data-stu-id="54fae-114">For each code package you would like toocontainerize, initialize hello loader at hello program entry point.</span></span> <span data-ttu-id="54fae-115">Lägg till hello statisk konstruktor som visas i hello följande kodfragment tooyour post punkt programfilen.</span><span class="sxs-lookup"><span data-stu-id="54fae-115">Add hello static constructor shown in hello following code snippet tooyour program entry point file.</span></span>

  ```csharp
  namespace MyApplication
  {
      internal static class Program
      {
          static Program()
          {
              SFBinaryLoader.Initialize();
          }

          /// <summary>
          /// This is hello entry point of hello service host process.
          /// </summary>
          private static void Main()
          {
  ```

4. <span data-ttu-id="54fae-116">Skapa och [paketet](service-fabric-package-apps.md#Package-App) projektet.</span><span class="sxs-lookup"><span data-stu-id="54fae-116">Build and [package](service-fabric-package-apps.md#Package-App) your project.</span></span> <span data-ttu-id="54fae-117">toobuild och skapa ett paket, högerklicka på projektet i Solution Explorer hello och välj hello **paketet** kommando.</span><span class="sxs-lookup"><span data-stu-id="54fae-117">toobuild and create a package, right-click hello application project in Solution Explorer and choose hello **Package** command.</span></span>

5. <span data-ttu-id="54fae-118">Du måste toocontainerize kör hello PowerShell-skript för varje kodpaketet [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1).</span><span class="sxs-lookup"><span data-stu-id="54fae-118">For every code package you need toocontainerize, run hello PowerShell script [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1).</span></span> <span data-ttu-id="54fae-119">hello syntax är följande:</span><span class="sxs-lookup"><span data-stu-id="54fae-119">hello usage is as follows:</span></span>
  ```powershell
    $codePackagePath = 'Path toohello code package toocontainerize.'
    $dockerPackageOutputDirectoryPath = 'Output path for hello generated docker folder.'
    $applicationExeName = 'Name of hello ode package executable.'
    CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
 ```
  <span data-ttu-id="54fae-120">hello skriptet skapar en mapp med Docker-artefakter på $dockerPackageOutputDirectoryPath.</span><span class="sxs-lookup"><span data-stu-id="54fae-120">hello script creates a folder with Docker artifacts at $dockerPackageOutputDirectoryPath.</span></span> <span data-ttu-id="54fae-121">Ändra hello genereras Dockerfile tooexpose alla portar, köra installationsskript etc. baserat på dina behov.</span><span class="sxs-lookup"><span data-stu-id="54fae-121">Modify hello generated Dockerfile tooexpose any ports, run setup scripts etc. based on your needs.</span></span>

6. <span data-ttu-id="54fae-122">Därefter behöver du för[skapa](service-fabric-get-started-containers.md#Build-Containers) och [push](service-fabric-get-started-containers.md#Push-Containers) Docker behållare paketet tooyour databasen.</span><span class="sxs-lookup"><span data-stu-id="54fae-122">Next you need too[build](service-fabric-get-started-containers.md#Build-Containers) and [push](service-fabric-get-started-containers.md#Push-Containers) your Docker container package tooyour repository.</span></span>

7. <span data-ttu-id="54fae-123">Ändra hello ApplicationManifest.xml och ServiceManifest.xml tooadd din behållaren bild, databasen information, registret autentisering och port-till-värd-mappning.</span><span class="sxs-lookup"><span data-stu-id="54fae-123">Modify hello ApplicationManifest.xml and ServiceManifest.xml tooadd your container image, repository information, registry authentication, and port-to-host mapping.</span></span> <span data-ttu-id="54fae-124">För att ändra hello manifesten finns [skapar ett program för Azure Service Fabric-behållaren](service-fabric-get-started-containers.md).</span><span class="sxs-lookup"><span data-stu-id="54fae-124">For modifying hello manifests, see [Create an Azure Service Fabric container application](service-fabric-get-started-containers.md).</span></span> <span data-ttu-id="54fae-125">hello kod paketdefinitionen i hello service manifest behov toobe ersätts med motsvarande behållaren bild.</span><span class="sxs-lookup"><span data-stu-id="54fae-125">hello code package definition in hello service manifest needs toobe replaced with corresponding container image.</span></span> <span data-ttu-id="54fae-126">Se till att toochange hello EntryPoint tooa ContainerHost typen.</span><span class="sxs-lookup"><span data-stu-id="54fae-126">Make sure toochange hello EntryPoint tooa ContainerHost type.</span></span>

  ```xml
<!-- Code package is your service executable. -->
<CodePackage Name="Code" Version="1.0.0">
  <EntryPoint>
    <!-- Follow this link for more information about deploying Windows containers tooService Fabric: https://aka.ms/sfguestcontainers -->
    <ContainerHost>
      <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
    </ContainerHost>
  </EntryPoint>
  <!-- Pass environment variables tooyour container: -->    
</CodePackage>
  ```

8. <span data-ttu-id="54fae-127">Lägga till hello-port till värd-mappning för replikatorn och tjänstslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="54fae-127">Add hello port-to-host mapping for your replicator and service endpoint.</span></span> <span data-ttu-id="54fae-128">Eftersom båda dessa portar tilldelas vid körning av Service Fabric, anges hello ContainerPort toozero toouse hello tilldelade porten för mappning.</span><span class="sxs-lookup"><span data-stu-id="54fae-128">Since both these ports are assigned at runtime by Service Fabric, hello ContainerPort is set toozero toouse hello assigned port for mapping.</span></span>

 ```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
  </ContainerHostPolicies>
</Policies>
 ```

9. <span data-ttu-id="54fae-129">tootest programmet, du behöver toodeploy den tooa kluster som kör version 5.7 eller högre.</span><span class="sxs-lookup"><span data-stu-id="54fae-129">tootest this application, you need toodeploy it tooa cluster that is running version 5.7 or higher.</span></span> <span data-ttu-id="54fae-130">Dessutom måste tooedit och uppdatera hello klustret inställningar tooenable preview-funktionen.</span><span class="sxs-lookup"><span data-stu-id="54fae-130">In addition, you need tooedit and update hello cluster settings tooenable this preview feature.</span></span> <span data-ttu-id="54fae-131">Följ hello stegen i den här [artikel](service-fabric-cluster-fabric-settings.md) tooadd hello inställningen visas nästa.</span><span class="sxs-lookup"><span data-stu-id="54fae-131">Follow hello steps in this [article](service-fabric-cluster-fabric-settings.md) tooadd hello setting shown next.</span></span>
```
      {
        "name": "Hosting",
        "parameters": [
          {
            "name": "FabricContainerAppsEnabled",
            "value": "true"
          }
        ]
      }
```
10. <span data-ttu-id="54fae-132">Nästa [distribuera](service-fabric-deploy-remove-applications.md) hello redigeras programmet paketet toothis klustret.</span><span class="sxs-lookup"><span data-stu-id="54fae-132">Next [deploy](service-fabric-deploy-remove-applications.md) hello edited application package toothis cluster.</span></span>

<span data-ttu-id="54fae-133">Du bör nu ha en container Service Fabric-program som körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="54fae-133">You should now have a containerized Service Fabric application running your cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54fae-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="54fae-134">Next steps</span></span>
* <span data-ttu-id="54fae-135">Mer information om hur du kör [behållare i Service Fabric](service-fabric-get-started-containers.md).</span><span class="sxs-lookup"><span data-stu-id="54fae-135">Learn more about running [containers on Service Fabric](service-fabric-get-started-containers.md).</span></span>
* <span data-ttu-id="54fae-136">Lär dig mer om hello Service Fabric [programmet livscykel](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="54fae-136">Learn about hello Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
