---
title: aaaAzure Service Fabric Docker Compose Preview | Microsoft Docs
description: "Azure Service Fabric accepterar Docker Compose format toomake den enklare tooorchestrate exsiting behållare med hjälp av Service Fabric. Detta stöd är för närvarande under förhandsgranskning."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 824044fd698f0ed94c4212722bc82187905315dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-volume-plugins-and-logging-drivers-for-your-container"></a><span data-ttu-id="d70a0-104">Ange plugin-program för volymen och loggning drivrutiner för din behållare</span><span class="sxs-lookup"><span data-stu-id="d70a0-104">Specifying volume plugins and logging drivers for your container</span></span>

<span data-ttu-id="d70a0-105">Service Fabric har stöd för att ange [volym plugin-program för Docker](https://docs.docker.com/engine/extend/plugins_volume/) och [Docker loggning drivrutiner](https://docs.docker.com/engine/admin/logging/overview/) för behållartjänsten.</span><span class="sxs-lookup"><span data-stu-id="d70a0-105">Service Fabric supports specifying [Docker volume plugins](https://docs.docker.com/engine/extend/plugins_volume/) and [Docker logging drivers](https://docs.docker.com/engine/admin/logging/overview/) for your container service.</span></span> <span data-ttu-id="d70a0-106">hello-plugin-program har angetts i applikationsmanifestet hello enligt följande manifestet hello:</span><span class="sxs-lookup"><span data-stu-id="d70a0-106">hello plugins are specified in hello application manifest as shown in hello following manifest:</span></span>


```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv"> 
        <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
        <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
        <LogConfig Driver="etwlogs" >
          <DriverOption Name="test" Value="vale"/>
        </LogConfig>
        <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
        <Volume Source="d:\myfolder" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
        <Volume Source="myexternalvolume" Destination="c:\testmountlocation3" Driver="sf" IsReadOnly="true"></Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

<span data-ttu-id="d70a0-107">I föregående exempel hello, hello `Source` taggen för hello `Volume` refererar toohello källmapp.</span><span class="sxs-lookup"><span data-stu-id="d70a0-107">In hello preceding example, hello `Source` tag for hello `Volume` refers toohello source folder.</span></span> <span data-ttu-id="d70a0-108">hello källmapp kan vara en mapp i hello virtuell dator som är värd för hello behållare eller en fjärransluten beständiga arkivet.</span><span class="sxs-lookup"><span data-stu-id="d70a0-108">hello source folder could be a folder in hello VM that hosts hello containers or a persistent remote store.</span></span> <span data-ttu-id="d70a0-109">Hej `Destination` taggen är hello-plats som hello `Source` körs mappade toowithin hello behållare.</span><span class="sxs-lookup"><span data-stu-id="d70a0-109">hello `Destination` tag is hello location that hello `Source` is mapped toowithin hello running container.</span></span> 

<span data-ttu-id="d70a0-110">När du anger ett volym plugin-program, skapar Service Fabric automatiskt hello volymen med hello parametrar har angetts.</span><span class="sxs-lookup"><span data-stu-id="d70a0-110">When specifying a volume plugin, Service Fabric automatically creates hello volume using hello parameters specified.</span></span> <span data-ttu-id="d70a0-111">Hej `Source` taggen är hello namnet på hello volym och hello `Driver` anger hello volym drivrutinen plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="d70a0-111">hello `Source` tag is hello name of hello volume, and hello `Driver` tag specifies hello volume driver plugin.</span></span> <span data-ttu-id="d70a0-112">Alternativen kan specificeras med hello `DriverOption` tagg som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="d70a0-112">Options can be specified using hello `DriverOption` tag as shown in hello following snippet:</span></span>

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azurefile" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

<span data-ttu-id="d70a0-113">Om en drivrutin för Docker-loggen har angetts är det nödvändigt toodeploy agenter (eller behållare) toohandle hello loggar i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="d70a0-113">If a Docker log driver is specified, it is necessary toodeploy agents (or containers) toohandle hello logs in hello cluster.</span></span>  <span data-ttu-id="d70a0-114">Hej `DriverOption` taggen kan vara används toospecify drivrutinen Loggalternativ samt.</span><span class="sxs-lookup"><span data-stu-id="d70a0-114">hello `DriverOption` tag can be used toospecify log driver options as well.</span></span>

<span data-ttu-id="d70a0-115">Se följande artiklar toodeploy behållare tooa Service Fabric-kluster toohello:</span><span class="sxs-lookup"><span data-stu-id="d70a0-115">Refer toohello following articles toodeploy containers tooa Service Fabric cluster:</span></span>


[<span data-ttu-id="d70a0-116">Distribuera en behållare för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="d70a0-116">Deploy a container on Service Fabric</span></span>](service-fabric-deploy-container.md)

