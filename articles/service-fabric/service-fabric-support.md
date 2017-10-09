---
title: aaaLearn om Azure Service Fabric supportalternativ | Microsoft Docs
description: "Azure Service Fabric-kluster-versioner som stöds och länkar toofile supportärenden"
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
ms.openlocfilehash: 42394e2cd9dad2040d37d3a2ff3600ee040d8720
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-support-options"></a>Azure Service Fabric-supportalternativ

toodeliver hello stöd för ditt Service Fabric-kluster att du kör programmet arbetet läses in på, vi har lagt upp olika alternativ för dig. Beroende på hello supportnivå behövs och hello allvarlighetsgraden hello problemet du får toopick hello rätt alternativ. 


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
Vi har skapat en GitHub-lagringsplatsen för att rapportera problem med Service Fabric.  Vi övervakar också aktivt hello följande forum.

### <a name="github-repo"></a>GitHub-repo 
Rapportera problem med Azure Service Fabric [problem för Service Fabric git repo](https://github.com/Azure/service-fabric-issues). Den här lagringsplatsen är avsedd för rapportering och uppföljning av problem med Azure Service Fabric och för att göra små funktionsförfrågningar. **Gör inte använder den här tooreport live-platsen skickar**.

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow och MSDN-forum

Hej [Service Fabric-tagg på StackOverflow] [ stackoverflow] och hello [Service Fabric-forum på MSDN] [ msdn-forum] är bästa används för att ställa frågor om hur hello plattform fungerar och hur du kan utföra vissa uppgifter med den.

### <a name="azure-feedback-forum"></a>Azure Feedback-forum

Hej [Azure Feedback Forum för Service Fabric] [ uservoice-forum] är hello bästa platsen för att skicka stora funktionen idéer som du har för hello produkten som vi går igenom hello populäraste förfrågningar är en del av vår medelhög toolong-term Planera. Vi rekommenderar att du toorally stöd för dina förslag inom hello community.


<a id="releasesuport"></a>
## <a name="supported-service-fabric-versions"></a>Service Fabric-versioner som stöds.

Kontrollera att klustret är alltid en Service Fabric-version som stöds. Och när vi meddela hello-versionen av en ny version av Service Fabric, har hello tidigare version markerats för slutet av stödet efter minst 60 dagar från det datumet. hello nya versioner tillkännages [på hello Service Fabric-teamets blogg](https://blogs.msdn.microsoft.com/azureservicefabric/).

Se toohello efter information om hur dokument tookeep ditt kluster som kör en Service Fabric-version som stöds.

- [Uppgradera Service Fabric-versionen på ett Azure-kluster](service-fabric-cluster-upgrade.md)
- [Uppgradera Service Fabric-versionen på en fristående windows server-kluster](service-fabric-cluster-upgrade-windows-server.md)
 
Här följer hello lista över hello Service Fabric-versioner som stöds och deras stöd slutdatum.

| **Service Fabric runtime-kluster** | **Kompatibel SDK / NuGet-paketet versioner** | **Slutet av Support datum** |
| --- | --- | --- |
| Alla kluster-versioner tidigare too5.3.121 |Mindre än eller lika med tooversion 2.3 |20 januari 2017 |
| 5.3.* |Mindre än eller lika med tooversion 2.3 |24 februari 2017 |
| 5.4.* |Mindre än eller lika med tooversion 2.4 |Kan 10,2017     |
| 5.5.* |Mindre än eller lika med tooversion 2,5 |Augusti 10,2017    |
| 5.6.* |Mindre än eller lika med tooversion 2.6 |Oktober 13,2017    |
| 5.7.* |Mindre än eller lika med tooversion 2.7 |Aktuell version och därför inget slutdatum

<a id="previewversion"></a>
## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Service Fabric förhandsversioner - stöds inte för produktion.
Från tid tootime släpp vi-versioner som har viktiga funktioner som vi vill feedback, som släpps som förhandsgranskningar. Dessa förhandsversioner bör endast användas för testning. Klustret produktion bör alltid köra en stöds, stabil, Service Fabric-version. En förhandsversion börjar alltid med ett högre och lägre versionsnummer 255. Till exempel är ett Service Fabric version 255.255.5703.949 visas den utgivna versionen endast toobe används i testkluster och är en förhandsversion. Dessa förhandsvisningarna också tillkännages i hello [Service Fabric-teamets blogg](https://blogs.msdn.microsoft.com/azureservicefabric) och kommer att ha information på hello-funktionerna.

Det finns inget stöd för betald alternativ för dessa förhandsvisningarna. Använd någon av hello alternativen [rapporten Azure Service Fabric utfärdar](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues) tooask frågor eller ge feedback.

## <a name="next-steps"></a>Nästa steg

- [Uppgradera service fabric-versionen på ett Azure-kluster](service-fabric-cluster-upgrade.md)
- [Uppgradera Service Fabric-versionen på en fristående windows server-kluster](service-fabric-cluster-upgrade-windows-server.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: http://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: http://aka.ms/servicefabricdocs
[sample-repos]: http://aka.ms/servicefabricsamples
