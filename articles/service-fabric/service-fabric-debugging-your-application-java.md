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
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a>Felsöka med Eclipse programmet Java Service Fabric
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
> 

1. Starta en lokal utveckling klustret genom att följa stegen hello i [ställa in din utvecklingsmiljö för Service Fabric](service-fabric-get-started-linux.md).

2. Uppdatera entryPoint.sh av Hej tjänst du vill toodebug, så att den startar hello java-processen med fjärråtkomst debug-parametrar. Den här filen finns på följande plats hello: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``. Port 8001 har angetts för felsökning i det här exemplet.

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=y -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. Uppdatera hello Application Manifest genom att ange hello instanser eller hello replikantalet för hello-tjänst som felsöks too1. Den här inställningen förhindrar konflikter för hello-port som används för felsökning. Ange till exempel för tillståndslösa tjänster ``InstanceCount="1"`` och för tillståndskänsliga tjänster set hello mål och min replikuppsättningen storlekar too1 på följande sätt: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.

4. Distribuera programmet hello.

5. I hello Eclipse IDE, väljer **kör -> Felsöka konfigurationer > Remote Java-program och ange anslutningsegenskaper** och ange hello egenskaper på följande sätt:

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  Ange brytpunkter på önskade distributionsplatser och felsöka hello program.

Du kan också ta tooenable coredumps om hello programmet kraschar. Köra ``ulimit -c`` i ett gränssnitt och om den returnerar 0 kommer coredumps inte har aktiverats. tooenable obegränsade coredumps köra följande kommando hello: ``ulimit -c unlimited``. Du kan också kontrollera hello status hello kommandot ``ulimit -a``.  Om du vill tooupdate hello coredump generation sökvägen kan köra ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``. 

### <a name="next-steps"></a>Nästa steg

* [Samla in loggar med hjälp av Linux Azure Diagnostics](service-fabric-diagnostics-how-to-setup-lad.md).
* [Övervaka och diagnostisera tjänster lokalt](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).
