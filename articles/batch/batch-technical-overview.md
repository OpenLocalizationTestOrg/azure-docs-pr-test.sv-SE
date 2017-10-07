---
title: "aaaAzure Batch körs storskaliga parallella databehandling lösningar i hello molntjänster | Microsoft Docs"
description: "Lär dig mer om hur du använder hello Azure Batch-tjänsten för storskaliga parallellt och HPC-arbetsbelastning"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 93e37d44-7585-495e-8491-312ed584ab79
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: acc52e46330c465f81951441d9067371098cf63a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-intrinsically-parallel-workloads-with-batch"></a>Köra parallella arbetsbelastningar med Batch

Azure Batch är en plattform för att köra storskaliga parallella och högpresterande datorbearbetning (HPC) program effektivt i hello molnet. Azure Batch schemalägger beräkningsintensiva arbete toorun på en hanterad samling av virtuella datorer och kan automatiskt skala beräkna resurser toomeet hello behoven hos dina jobb.

Med Azure Batch, kan du enkelt definiera Azure compute resurser tooexecute dina program parallellt och i större skala. Det finns inget behov av toomanually skapa, konfigurera och hantera ett HPC-kluster, enskilda virtuella datorer, virtuella nätverk eller ett komplexa jobb och uppgift schemaläggning infrastruktur. Azure Batch automatiserar eller förenklar dessa uppgifter åt dig.

## <a name="use-cases-for-batch"></a>Användningsfall för Batch
Batch är en hanterad Azure-tjänst som används för *batchbearbetning* eller *batchbehandling* – dvs. körning av ett stort antal liknande uppgifter för ett visst önskat resultat. Batchbehandling används framför allt av organisationer som regelbundet bearbetar, omvandlar och analyserar stora mängder data.

Batch fungerar bra med parallella program och arbetsbelastningar. Parallella arbetsbelastningar är sådana som lätt delas in i flera aktiviteter som utför arbete samtidigt på flera datorer.

![Parallella aktiviteter][1]<br/>

Några exempel på arbetsbelastningar som vanligtvis bearbetas med den här tekniken är:

* Finansiell riskmodellering.
* Analys av klimat- och hydrologidata
* Rendering, analys och bearbetning av bilder
* Mediekodning och transkodning
* Genetisk sekvensanalys
* Analyser av teknisk spänning
* Programvarutestning

Batch kan också utföra parallella beräkningar med ett minska steg hello slutet och köra mer avancerade HPC-arbetsbelastning som [Message Passing Interface (MPI)](batch-mpi.md) program.

En jämförelse mellan Batch och andra HPC-lösningsalternativ i Azure finns i [Batch- och HPC-lösningar](batch-hpc-solutions.md).

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="scenario-scale-out-a-parallel-workload"></a>Scenario: Skala ut en parallell arbetsbelastning
En vanlig lösning som använder hello Batch-API: er toointeract med hello Batch-tjänsten innebär att skala ut filsystemen parallella arbete med till exempel hello återgivningen av avbildningar för 3D-bakgrunden--på en pool med compute-noder. Den här poolen för compute-noder kan vara servergruppen ”Visa” som innehåller flera hundratals eller tusentals kärnor tooyour återgivning jobb, t.ex.

hello följande diagram visar ett vanligt Batch-arbetsflöde med ett klientprogram eller värdbaserade tjänsten med hjälp av Batch-toorun en parallell arbetsbelastning.

![Arbetsflöde för Batch-lösning][2]

Det här vanliga scenariot bearbetar programmet eller tjänsten en beräkningar arbetsbelastning i Azure Batch genom att utföra hello följande steg:

1. Överför hello **inkommande filer** och hello **programmet** som bearbetar dessa filer tooyour Azure Storage-konto. hello indatafiler kan vara alla data som programmet kommer att bearbetas, till exempel finansiella modellering data eller videofiler toobe kodas. hello-filer kan vara vilket program som används för bearbetning av hello data, till exempel ett program för 3D-återgivning eller transcoder media.
2. Skapa en Batch **pool** av beräkningsnoder i Batch-kontot--dessa noder finns hello virtuella datorer som utför aktiviteterna. Du kan ange egenskaper för till exempel hello [nodstorlek](../cloud-services/cloud-services-sizes-specs.md), deras operativsystem och hello plats i Azure Storage för programmet hello tooinstall när hello noder ansluter hello pool (hello program som du laddade upp i steg #1). Du kan också konfigurera hello poolen för[skala automatiskt](batch-automatic-scaling.md) i svaret toohello arbetsbelastning som genererar dina uppgifter. Automatisk skalning dynamiskt justerar hello antalet compute-noder i hello pool.
3. Skapa en Batch **jobbet** toorun hello arbetsbelastningen på hello-pool med compute-noder. När du skapar ett jobb kan du associera det med en Batch-pool.
4. Lägg till **uppgifter** toohello jobb. När du lägger till aktiviteter tooa jobbet hello Batch-tjänsten automatiskt hello aktiviteter schemaläggs för körning på hello datornoder i hello pool. Varje aktivitet använder hello-program som du har överfört tooprocess hello indatafiler.
   
   * 4a. Innan en aktivitet körs, kan den hämta hello data (hello inkommande filer) som är det tooprocess toohello compute-nod som den är tilldelad till. Om programmet hello inte redan har installerats på hello nod (se steg #2) du kan hämta här i stället. När hello hämtningar har slutförts kan köra hello uppgifter på sina tilldelade noder.
5. Du kan fråga Batch toomonitor hello fortskrider hello jobb och dess uppgifter som hello aktiviteter körs. Klientprogrammet eller tjänsten kommunicerar med hello Batch-tjänsten via HTTPS. Eftersom du kan övervaka tusentals aktiviteter som körs på tusentals compute-noder kan vara säker på att för[fråga hello Batch-tjänsten effektivt](batch-efficient-list-queries.md).
6. När hello uppgifter Slutför överför de sina resultatet data tooAzure lagring. Du kan också hämta filer direkt från hello filsystemet på en beräkningsnod.
7. När övervakningen upptäcker att hello uppgifter i jobbet har slutförts, kan klientprogrammet eller tjänsten hämta hello utdata för vidare bearbetning eller utvärdering.

Att tänka på detta är ett sätt toouse Batch och det här scenariot beskriver endast några av dess funktioner som är tillgängliga. Du kan till exempel köra [flera uppgifter parallellt](batch-parallel-node-tasks.md) på varje beräkningsnod, och du kan använda [jobbet förberedelse och slutförande av uppgifter](batch-job-prep-release.md) tooprepare hello noder för jobb och sedan rensa efteråt.

## <a name="next-steps"></a>Nästa steg
Nu när du har en översikt över hello Batch-tjänsten är det tid toodig djupare toolearn hur du kan använda den tooprocess beräkningsintensiva parallella arbetsbelastningar.

* Läs hello [Batch funktionsöversikt för utvecklare](batch-api-basics.md), viktig information för alla förbereder toouse Batch. hello artikeln innehåller mer detaljerad information om Batch-tjänsten resurser som pooler, noder, jobb och uppgifter och hello många API-funktioner som du kan använda när du skapar Batch-program.
* Lär dig mer om hello [Batch-API: er och verktyg](batch-apis-tools.md) tillgängliga för att skapa Batch-lösningar.
* [Kom igång med hello Azure Batch-biblioteket för .NET](batch-dotnet-get-started.md) toolearn hur toouse C# och hello Batch .NET-biblioteket tooexecute en enkel arbetsbelastning med hjälp av en gemensam Batch-arbetsflöde. Den här artikeln ska vara någon av dina första stopp då du lär dig hur toouse hello Batch-tjänsten. Det finns också en [Python-versionen](batch-python-tutorial.md) hello kursen.
* Hämta hello [kodexempel på GitHub] [ github_samples] toosee hur både C# och Python kan samverka med Batch tooschedule och processen exempel arbetsbelastningar.
* Kolla in hello [Batch Utbildningsväg] [ learning_path] tooget en uppfattning om hello resurser tillgängliga tooyou som du lär dig toowork med Batch.


[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
