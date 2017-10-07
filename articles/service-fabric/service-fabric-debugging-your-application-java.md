---
title: aaaDebug Azure Service Fabric-programmet i Eclipse | Microsoft Docs
description: "Förbättra hello pålitligheten och prestandan för dina tjänster genom att utveckla och felsökning av dem i Eclipse i ett kluster för lokal utveckling."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/10/2017
ms.author: vturecek;mikhegn
ms.openlocfilehash: ab86254a5c312db40fd631746c89aab0bbb9d1a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a><span data-ttu-id="4ef2d-103">Felsöka med Eclipse programmet Java Service Fabric</span><span class="sxs-lookup"><span data-stu-id="4ef2d-103">Debug your Java Service Fabric application using Eclipse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4ef2d-104">Visual Studio/CSharp</span><span class="sxs-lookup"><span data-stu-id="4ef2d-104">Visual Studio/CSharp</span></span>](service-fabric-debugging-your-application.md) 
> * [<span data-ttu-id="4ef2d-105">Eclipse/Java</span><span class="sxs-lookup"><span data-stu-id="4ef2d-105">Eclipse/Java</span></span>](service-fabric-debugging-your-application-java.md)
> 

1. <span data-ttu-id="4ef2d-106">Starta en lokal utveckling klustret genom att följa stegen hello i [ställa in din utvecklingsmiljö för Service Fabric](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="4ef2d-106">Start a local development cluster by following hello steps in [Setting up your Service Fabric development environment](service-fabric-get-started-linux.md).</span></span>

2. <span data-ttu-id="4ef2d-107">Uppdatera entryPoint.sh av Hej tjänst du vill toodebug, så att den startar hello java-processen med fjärråtkomst debug-parametrar.</span><span class="sxs-lookup"><span data-stu-id="4ef2d-107">Update entryPoint.sh of hello service you wish toodebug, so that it starts hello java process with remote debug parameters.</span></span> <span data-ttu-id="4ef2d-108">Den här filen finns på följande plats hello: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``.</span><span class="sxs-lookup"><span data-stu-id="4ef2d-108">This file can be found at hello following location: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``.</span></span> <span data-ttu-id="4ef2d-109">Port 8001 har angetts för felsökning i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="4ef2d-109">Port 8001 is set for debugging in this example.</span></span>

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=y -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. <span data-ttu-id="4ef2d-110">Uppdatera hello Application Manifest genom att ange hello instanser eller hello replikantalet för hello-tjänst som felsöks too1.</span><span class="sxs-lookup"><span data-stu-id="4ef2d-110">Update hello Application Manifest by setting hello instance count or hello replica count for hello service that is being debugged too1.</span></span> <span data-ttu-id="4ef2d-111">Den här inställningen förhindrar konflikter för hello-port som används för felsökning.</span><span class="sxs-lookup"><span data-stu-id="4ef2d-111">This setting avoids conflicts for hello port that is used for debugging.</span></span> <span data-ttu-id="4ef2d-112">Ange till exempel för tillståndslösa tjänster ``InstanceCount="1"`` och för tillståndskänsliga tjänster set hello mål och min replikuppsättningen storlekar too1 på följande sätt: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.</span><span class="sxs-lookup"><span data-stu-id="4ef2d-112">For example, for stateless services, set ``InstanceCount="1"`` and for stateful services set hello target and min replica set sizes too1 as follows: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.</span></span>

4. <span data-ttu-id="4ef2d-113">Distribuera programmet hello.</span><span class="sxs-lookup"><span data-stu-id="4ef2d-113">Deploy hello application.</span></span>

5. <span data-ttu-id="4ef2d-114">I hello Eclipse IDE, väljer **kör -> Felsöka konfigurationer > Remote Java-program och ange anslutningsegenskaper** och ange hello egenskaper på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4ef2d-114">In hello Eclipse IDE, select **Run -> Debug Configurations -> Remote Java Application and input connection properties** and set hello properties as follows:</span></span>

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  <span data-ttu-id="4ef2d-115">Ange brytpunkter på önskade distributionsplatser och felsöka hello program.</span><span class="sxs-lookup"><span data-stu-id="4ef2d-115">Set breakpoints at desired points and debug hello application.</span></span>

<span data-ttu-id="4ef2d-116">Du kan också ta tooenable coredumps om hello programmet kraschar.</span><span class="sxs-lookup"><span data-stu-id="4ef2d-116">If hello application is crashing, you may also want tooenable coredumps.</span></span> <span data-ttu-id="4ef2d-117">Köra ``ulimit -c`` i ett gränssnitt och om den returnerar 0 kommer coredumps inte har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="4ef2d-117">Execute ``ulimit -c`` in a shell and if it returns 0, then coredumps are not enabled.</span></span> <span data-ttu-id="4ef2d-118">tooenable obegränsade coredumps köra följande kommando hello: ``ulimit -c unlimited``.</span><span class="sxs-lookup"><span data-stu-id="4ef2d-118">tooenable unlimited coredumps, execute hello following command: ``ulimit -c unlimited``.</span></span> <span data-ttu-id="4ef2d-119">Du kan också kontrollera hello status hello kommandot ``ulimit -a``.</span><span class="sxs-lookup"><span data-stu-id="4ef2d-119">You can also verify hello status using hello command ``ulimit -a``.</span></span>  <span data-ttu-id="4ef2d-120">Om du vill tooupdate hello coredump generation sökvägen kan köra ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``.</span><span class="sxs-lookup"><span data-stu-id="4ef2d-120">If you wanted tooupdate hello coredump generation path, execute ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``.</span></span> 

### <a name="next-steps"></a><span data-ttu-id="4ef2d-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4ef2d-121">Next steps</span></span>

* <span data-ttu-id="4ef2d-122">[Samla in loggar med hjälp av Linux Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md).</span><span class="sxs-lookup"><span data-stu-id="4ef2d-122">[Collect logs using Linux Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md).</span></span>
* <span data-ttu-id="4ef2d-123">[Övervaka och diagnostisera tjänster lokalt](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).</span><span class="sxs-lookup"><span data-stu-id="4ef2d-123">[Monitor and diagnose services locally](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).</span></span>
