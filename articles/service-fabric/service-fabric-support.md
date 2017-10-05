---
title: "Lär dig mer om Azure Service Fabric-supportalternativ | Microsoft Docs"
description: "Azure Service Fabric-kluster-versioner som stöds och länkar till filen supportärenden"
services: service-fabric
documentationcenter: .net
author: pkc
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: pkc
ms.openlocfilehash: 78e68cff3a757cbbcd8dc6f53120e6a4af54591a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-service-fabric-support-options"></a>Azure Service Fabric-supportalternativ

Vi har ställt in olika alternativ för att ge stöd för ditt Service Fabric-kluster som du kör ditt program arbetsbelastningar på. Beroende på nivån av stöd som behövs och problemets allvarlighetsgrad, får du välja rätt alternativ. 


<a id="getlivesitesupportonazure"></a>

## <a name="report-production-or-live-site-issues-or-request-paid-support-for-azure"></a>Rapportera produktion eller problem med live-plats eller begär betald support för Azure

Skapa ett ärende för professionell support för rapportering live-plats problem på ditt Service Fabric-kluster som distribueras på Azure, [på Azure-portalen](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) eller [Microsoft support-portalen](http://support.microsoft.com/oas/default.aspx?prid=16146).

Läs mer om:
 
- [Professional Support från Microsoft för Azure](https://azure.microsoft.com/en-us/support/plans/?b=16.44).
- [Microsoft premier support](https://support.microsoft.com/en-us/premier).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-or-live-site-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Rapportera produktion eller problem med live-plats eller begär betald support för fristående Service Fabric-kluster

Rapportera problem med live-plats på Service Fabric-klustret distribueras lokalt eller i andra moln, skapa ett ärende för professionell support på [Microsoft support-portalen](http://support.microsoft.com/oas/default.aspx?prid=16146).

Läs mer om:

- [Professional Support från Microsoft för lokalt](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
- [Microsoft premier support](https://support.microsoft.com/en-us/premier).


<a id="getsupportonissues"></a>
## <a name="report-azure-service-fabric-issues"></a>Rapporten Azure Service Fabric-problem 
Vi har skapat en GitHub-lagringsplatsen för att rapportera problem med Service Fabric.  Vi övervakar också aktivt följande forum.

### <a name="github-repo"></a>GitHub-repo 
Rapportera problem med Azure Service Fabric [problem för Service Fabric git repo](https://github.com/Azure/service-fabric-issues). Den här lagringsplatsen är avsedd för rapportering och uppföljning av problem med Azure Service Fabric och för att göra små funktionsförfrågningar. **Använd detta inte att rapporten live-plats problem**.

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow och MSDN-forum

Den [Service Fabric-tagg på StackOverflow] [ stackoverflow] och [Service Fabric-forum på MSDN] [ msdn-forum] är bästa används för att ställa frågor om hur plattform fungerar och hur du kan utföra vissa uppgifter med den.

### <a name="azure-feedback-forum"></a>Azure Feedback-forum

Den [Azure Feedback Forum för Service Fabric] [ uservoice-forum] är den bästa platsen för att skicka stora funktionen idéer som du har för produkten som vi går igenom de mest populära begärandena är en del av vår medelstora och långsiktig planering. Vi rekommenderar att du rally stöd för dina förslag inom gruppen.


<a id="releasesuport"></a>
## <a name="supported-service-fabric-versions"></a>Service Fabric-versioner som stöds.

Kontrollera att klustret är alltid en Service Fabric-version som stöds. Och när vi meddela lanseringen av en ny version av Service Fabric, markeras den tidigare versionen för slutet av stödet efter 60 dagar från det datum då minst. De nya versionerna meddelas [på Service Fabric-teamets blogg](https://blogs.msdn.microsoft.com/azureservicefabric/).

Referera till följande dokument på information om hur du kan skydda ditt kluster som kör en Service Fabric-version som stöds.

- [Uppgradera Service Fabric-versionen på ett Azure-kluster](service-fabric-cluster-upgrade.md)
- [Uppgradera Service Fabric-versionen på en fristående windows server-kluster](service-fabric-cluster-upgrade-windows-server.md)
 
Här är listan över Service Fabric-versioner som stöds och deras stöd slutdatum.

| **Service Fabric runtime-kluster** | **Kompatibel SDK / NuGet-paketet versioner** | **Slutet av Support datum** |
| --- | --- | --- |
| Kluster-versioner före 5.3.121 |Mindre än eller lika med version 2.3 |20 januari 2017 |
| 5.3.* |Mindre än eller lika med version 2.3 |24 februari 2017 |
| 5.4.* |Mindre än eller lika med version 2.4 |Kan 10,2017     |
| 5.5.* |Mindre än eller lika med version 2.5 |Augusti 10,2017    |
| 5.6.* |Mindre än eller lika med version 2.6 |Oktober 13,2017    |
| 5.7.* |Mindre än eller lika med version 2.7 |Aktuell version och därför inget slutdatum

<a id="previewversion"></a>
## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Service Fabric förhandsversioner - stöds inte för produktion.
Då, släpp vi-versioner som har viktiga funktioner som vi vill feedback, som släpps som förhandsgranskningar. Dessa förhandsversioner bör endast användas för testning. Klustret produktion bör alltid köra en stöds, stabil, Service Fabric-version. En förhandsversion börjar alltid med ett högre och lägre versionsnummer 255. Till exempel om du ser ett Service Fabric version 255.255.5703.949 som versionen ska bara användas i testkluster och är en förhandsversion. Dessa förhandsvisningarna också tillkännages i den [Service Fabric-teamets blogg](https://blogs.msdn.microsoft.com/azureservicefabric) och kommer att ha information på funktioner som ingår.

Det finns inget stöd för betald alternativ för dessa förhandsvisningarna. Använd någon av de alternativ som beskrivs [rapporten Azure Service Fabric utfärdar](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues) att ställa frågor eller lämna feedback.

## <a name="next-steps"></a>Nästa steg

- [Uppgradera service fabric-versionen på ett Azure-kluster](service-fabric-cluster-upgrade.md)
- [Uppgradera Service Fabric-versionen på en fristående windows server-kluster](service-fabric-cluster-upgrade-windows-server.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: http://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: http://aka.ms/servicefabricdocs
[sample-repos]: http://aka.ms/servicefabricsamples
