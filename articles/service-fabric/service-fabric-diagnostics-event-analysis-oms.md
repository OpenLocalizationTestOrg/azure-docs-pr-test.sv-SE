---
title: "aaaAzure Service Fabric-Händelseanalys med OMS | Microsoft Docs"
description: "Läs mer om visualisera och analysera händelser med hjälp av OMS för övervakning och diagnostik av Azure Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/26/2017
ms.author: dekapur
ms.openlocfilehash: 526519293e70982c95e31241465b87f190096f74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-analysis-and-visualization-with-oms"></a>Händelseanalys och visualisering med OMS

Operations Management Suite (OMS) är en uppsättning tjänster som underlättar övervakning och diagnostik för program och tjänster i molnet hello. tooget en detaljerad översikt över OMS och vad den erbjuder, läsa [vad är OMS?](../operations-management-suite/operations-management-suite-overview.md)

## <a name="log-analytics-and-hello-oms-workspace"></a>Logga analyser och hello OMS-arbetsyta

Logganalys samlar in data från hanterade resurser, inklusive en Azure-lagring eller en agent och underhåller i en central databas. hello data kan sedan användas för analys, varningar och visualisering eller ytterligare exporterar. Logganalys stöder händelser, prestandadata eller andra anpassade data.

När OMS konfigureras, har du åtkomst tooa specifika *OMS-arbetsytan*, från där data kan efterfrågas eller visualiseras i instrumentpaneler.

När data tas emot av logganalys OMS har flera *hanteringslösningar* som är färdigförpackade lösningar toomonitor inkommande data, anpassade tooseveral scenarier. Dessa inkluderar en *Service Fabric Analytics* lösning och en *behållare* lösning som är hello två relevant de toodiagnostics och kontroll när du använder Service Fabric-kluster. Det finns flera andra program som är värda att utforska och OMS kan också för hello skapa anpassade lösningar som du kan läsa mer om [här](../operations-management-suite/operations-management-suite-solutions.md). Varje lösning som du väljer toouse för ett kluster som ska konfigureras i hello samma OMS-arbetsyta, tillsammans med logganalys. Arbetsytor tillåta anpassade instrumentpaneler och visualisering av data och ändringar toohello data som du vill toocollect, bearbeta och analysera.

## <a name="setting-up-an-oms-workspace-with-hello-service-fabric-solution"></a>Ställa in en OMS-arbetsyta med hello Service Fabric-lösningen

Vi rekommenderar att du inkluderar hello Service Fabric-lösningen i OMS-arbetsyta, eftersom det ger en bra instrumentpanel som visar hello olika inkommande loggkanaler från hello serverplattform och nivån och hello kan tooquery Service Fabric specifika loggar. Här är en relativt enkla Service Fabric-lösningen ser ut, med ett enda program som distribuerats på hello klustret:

![OMS SA lösning](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

Det finns två sätt tooprovision och konfigurera en OMS-arbetsyta, antingen via en Resource Manager-mall eller direkt från Azure Marketplace. Använd hello tidigare när du distribuerar ett kluster och hello senare om du redan har ett kluster som distribueras med diagnostik aktiverat.

### <a name="deploying-oms-using-a-resource-management-template"></a>Distribuera OMS med hjälp av en Resource Manager-mall

Detta sker när hello klustret skapas steget – när du distribuerar ett kluster med en Resource Manager-mall, hello mall kan du även skapa en ny OMS-arbetsyta, lägga till hello Service Fabric-lösningen tooit och konfigurera den tooread data från hello lämplig lagring tabeller.

>[!NOTE]
>För den här toowork diagnostik har aktiverats för hello Azure storage-tabeller tooexist för OMS toobe / Log Analytics tooread information i från.

[Här](https://azure.microsoft.com/resources/templates/service-fabric-oms/) är en exempelmall som du kan använda och ändra enligt krav som utför ovanstående åtgärder. Hello om att du vill använda flera alternativ, det finns några fler mallar som ger olika alternativ beroende på i hello processen kanske du för att skapa en OMS-arbetsyta - de finns på [Service Fabric och OMS mallar](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

### <a name="deploying-oms-using-through-azure-marketplace"></a>Distribution OMS med via Azure Marketplace

Om du vill tooadd en OMS-arbetsyta när du har distribuerat ett kluster kan gå tooAzure Marketplace och leta efter *”Service Fabric Analytics”*. Det ska bara finnas en resurs som visas, inom hello ”övervakning + Management”-kategorin som visas nedan:

![OMS SA analyser i Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

Klicka på **skapa** ber dig för en OMS-arbetsyta. Klicka på **Välj en arbetsyta** och sedan **skapa en ny arbetsyta**. Fylla hello krävs poster - hello här den enda kravet är att hello hello prenumerationen för hello Service Fabric-klustret och hello OMS-arbetsytan ska vara samma. När posterna har validerats distribuerar din OMS-arbetsyta om några minuter. När den är klar distribuera förblir hello skapandet av hello Service Fabric-lösningen bladet fortfarande öppen. Kontrollera som hello samma arbetsytan visar upp under *OMS-arbetsytan* och träffar **skapa** hello längst ned i tooadd hello Service Fabric-lösningen toohello arbetsytan.

## <a name="using-hello-oms-agent"></a>Med hjälp av hello OMS-Agent

Det rekommenderas toouse EventFlow BOMULLSTUSS som aggregering lösningar eftersom de tillåter en mer modulära metoden toodiagnostics och övervakning. Till exempel om du vill toochange dina utdata från EventFlow, kräver den ingen ändring tooyour faktiska instrumentation, bara en enkel ändring tooyour konfigurationsfil. Om du dock du bestämmer dig tooinvest med OMS och är beredd toocontinue med för händelseanalys (saknar toobe hello endast plattform som du använder, men i stället att det kommer att minst en av hello-plattformar), rekommenderar vi att du först ställa in hello [</C0>OMS-agent<spanclass="notranslate">](../log-analytics/log-analytics-windows-agents.md).</span> Du bör också använda hello OMS-agenten när du distribuerar behållare tooyour kluster, enligt beskrivningen nedan.

hello-processen för att göra detta är relativt enkelt eftersom du precis har tooadd hello agenten som en virtuell dator för skaluppsättning för tillägget tooyour Resource Manager-mall, att säkerställa att den installeras på var och en av noderna. Går att hitta en Resource Manager-mall som distribuerar hello OMS-arbetsytan med hello Service Fabric-lösning (se ovan) och lägger till hello agent tooyour noder för kluster som kör [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) eller [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

hello fördelarna med detta är hello följande:

* Rikare data på hello prestanda räknare och mått på klientsidan
* Enkelt tooconfigure data som samlas in från hello klustret och gör ändringar tooit utan att omdistribuera dina program eller hello kluster eftersom ändringar toohello inställningar av hello-agenten kan göras från hello OMS-arbetsytan och kommer bara återställa hello-agent automatiskt. tooconfigure hello OMS-agenten toopick in specifika prestandaräknare, gå toohello arbetsytan **Start > Inställningar > Data > Windows prestandaräknare** och välj hello data som samlas in toosee
* Data visas snabbare än med toobe lagras innan tas upp av OMS / logganalys
* Övervakning av behållare är mycket enklare eftersom den kan hämta docker-loggar (stdout, stderror) och statistik (prestandamått på behållaren och nod-nivåer)

hello här huvudsakliga övervägande är att eftersom det är en agent kommer att distribueras på ditt kluster tillsammans med alla program, så vissa minimal påverkan toohello prestanda för dina program på hello klustret.

## <a name="monitoring-containers"></a>Övervakning av behållare

När du distribuerar behållare tooa Service Fabric-kluster, rekommenderas att hello klustret har konfigurerats med hello OMS-agent och den hello behållare lösning har lagts till tooyour OMS arbetsytan tooenable övervakning och diagnostik. Här är vad hello behållare lösning ser ut i en arbetsyta:

![Grundläggande OMS-instrumentpanelen](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

hello agent samlas hello flera behållare-specifika loggar som kan efterfrågas i OMS eller används toovisualized nyckeltal. hello loggen typer som samlas in är:

* ContainerInventory: Visar information om behållarens plats, namn och bilder
* ContainerImageInventory: information om distribuerade bilder, inklusive ID eller storlekar
* ContainerLog: specifika felloggar, docker-loggar (stdout osv.) och andra transaktioner
* ContainerServiceLog: docker daemon kommandon som har körts
* Prestanda: prestandaräknare inklusive behållaren cpu, minne, nätverkstrafik, disk-i/o och anpassade mått från hello värd för datorer

Den här artikeln beskriver hello steg krävs tooset in behållaren övervakning för klustret. Mer om OMSS behållare lösningen toolearn checka ut sina [dokumentationen](../log-analytics/log-analytics-containers.md).

tooset in hello behållaren lösning på din arbetsyta, kontrollera att du har hello OMS-agent distribueras på din klusternoder genom att följa hello stegen ovan. Distribuera en behållare tooit när hello klustret är klar. Bära på att hello första gången en avbildning av behållare är distribuerade tooa klustret, det tar flera minuter toodownload hello avbildningen beroende på storleken.

I Azure Marketplace, söker du efter *behållare* och skapa en behållare resurs (under hello övervakning + Management kategori).

![Lägga till behållare lösning](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

I steget Skapa hello begär en OMS-arbetsyta. Välj hello ett som har skapats med hello distribution ovan. Det här steget lägger till en behållare lösning OMS-arbetsyta och identifieras automatiskt av hello OMS-agent distribueras med hello mallen. hello agenten börjar samla in data på hello behållare processer i hello klustret och mindre än 15 minuter eller så bör du se hello lösning lysa med data som hello bild av hello instrumentpanelen ovan.


## <a name="next-steps"></a>Nästa steg

Utforska hello följande OMS verktyg och alternativ toocustomize en arbetsyta tooyour måste:

* OMS erbjuder en Gateway (HTTP framåt Proxy) som kan använda toosend data tooOMS för lokala kluster. Läs mer om att [ansluta datorer utan tooOMS Internetåtkomst med hello OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md)
* Konfigurera OMS tooset in [automatiserade aviseringar](../log-analytics/log-analytics-alerts.md) tooaid i identifiering och diagnostik
* Hämta bekantat med hello [logga sökning och hämtning av](../log-analytics/log-analytics-log-searches.md) funktioner som erbjuds som en del av logganalys
