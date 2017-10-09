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
# <a name="how-toocontainerize-your-service-fabric-reliable-services-and-reliable-actors-preview"></a>Hur toocontainerize din Service Fabric tillförlitlig Services och Reliable Actors (förhandsgranskning)

Service Fabric stöder containerizing Service Fabric-mikrotjänster (Reliable Services och tillförlitlig baserat aktörstjänster). Mer information finns i [service fabric-behållare](service-fabric-containers-overview.md).


 Den här funktionen är i förhandsversionen och den här artikeln innehåller hello olika steg tooget din tjänst som körs i en behållare.  

> [!NOTE]
> Den här funktionen är i förhandsvisning och stöds inte i produktion. Den här funktionen fungerar för närvarande endast för Windows.

## <a name="steps-toocontainerize-your-service-fabric-application"></a>Steg toocontainerize tillämpningsprogrammet Service Fabric

1. Öppna Service Fabric-program i Visual Studio.

2. Lägg till klass [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) tooyour projekt. hello koden i den här klassen är en helper toocorrectly belastningen hello Service Fabric runtime-binärfiler i ditt program vid körning i en behållare.

3. För varje kodpaketet som toocontainerize, initiera hello inläsaren på hello startpunkt för programmet. Lägg till hello statisk konstruktor som visas i hello följande kodfragment tooyour post punkt programfilen.

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

4. Skapa och [paketet](service-fabric-package-apps.md#Package-App) projektet. toobuild och skapa ett paket, högerklicka på projektet i Solution Explorer hello och välj hello **paketet** kommando.

5. Du måste toocontainerize kör hello PowerShell-skript för varje kodpaketet [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1). hello syntax är följande:
  ```powershell
    $codePackagePath = 'Path toohello code package toocontainerize.'
    $dockerPackageOutputDirectoryPath = 'Output path for hello generated docker folder.'
    $applicationExeName = 'Name of hello ode package executable.'
    CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
 ```
  hello skriptet skapar en mapp med Docker-artefakter på $dockerPackageOutputDirectoryPath. Ändra hello genereras Dockerfile tooexpose alla portar, köra installationsskript etc. baserat på dina behov.

6. Därefter behöver du för[skapa](service-fabric-get-started-containers.md#Build-Containers) och [push](service-fabric-get-started-containers.md#Push-Containers) Docker behållare paketet tooyour databasen.

7. Ändra hello ApplicationManifest.xml och ServiceManifest.xml tooadd din behållaren bild, databasen information, registret autentisering och port-till-värd-mappning. För att ändra hello manifesten finns [skapar ett program för Azure Service Fabric-behållaren](service-fabric-get-started-containers.md). hello kod paketdefinitionen i hello service manifest behov toobe ersätts med motsvarande behållaren bild. Se till att toochange hello EntryPoint tooa ContainerHost typen.

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

8. Lägga till hello-port till värd-mappning för replikatorn och tjänstslutpunkten. Eftersom båda dessa portar tilldelas vid körning av Service Fabric, anges hello ContainerPort toozero toouse hello tilldelade porten för mappning.

 ```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
  </ContainerHostPolicies>
</Policies>
 ```

9. tootest programmet, du behöver toodeploy den tooa kluster som kör version 5.7 eller högre. Dessutom måste tooedit och uppdatera hello klustret inställningar tooenable preview-funktionen. Följ hello stegen i den här [artikel](service-fabric-cluster-fabric-settings.md) tooadd hello inställningen visas nästa.
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
10. Nästa [distribuera](service-fabric-deploy-remove-applications.md) hello redigeras programmet paketet toothis klustret.

Du bör nu ha en container Service Fabric-program som körs i klustret.

## <a name="next-steps"></a>Nästa steg
* Mer information om hur du kör [behållare i Service Fabric](service-fabric-get-started-containers.md).
* Lär dig mer om hello Service Fabric [programmet livscykel](service-fabric-application-lifecycle.md).
