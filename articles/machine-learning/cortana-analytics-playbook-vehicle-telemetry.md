---
title: "aaaPredict vehicle hälsotillstånd och andra vanor - Azure | Microsoft Docs"
description: "Använda hello funktionerna i Cortana Intelligence toogain i realtid och förutsägbara insikter på vehicle hälsotillstånd och andra vanor."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 54cc890ff39493bc040bb809721388349665720f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Fordonstelemetrianalys, lösning, playbook
Detta **menyn** länkar toohello kapitlen i den här playbook. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Översikt
Super datorer har flyttats från hello testmiljön och nu parkerade i vår garage! Dessa senaste bilar innehåller en mängd sensorer, ger dem hello möjlighet tootrack och övervaka miljontals händelser per sekund. Vi räknar med att 2020, de flesta av dessa bilar kommer har anslutna toohello Internet. Anta att trycka på i denna mängd data tooprovide större säkerhet, tillförlitlighet och en bättre intresseväckande upplevelse! Microsoft har gjort detta drömma en verkligheten med Cortana Intelligence.

Microsofts Cortana Intelligence advanced analytics suite som gör att du tootransform dina data i intelligent åtgärd är en helt hanterad stordata. Vi vill toointroduce toohello Cortana Intelligence Vehicle telemetri Analytics Lösningsmall. Den här lösningen visar hur bil hos återförsäljarna, bil tillverkare och försäkringsbolag kan använda hello funktionerna i Cortana Intelligence toogain realtid och förutsägbara insikter om vehicle hälsa och köra vanor. 

hello lösningen har implementerats som en [lambda-arkitektur mönster](https://en.wikipedia.org/wiki/Lambda_architecture) visar hello fullt ut i hello Cortana Intelligence-plattformen för realtid och batchbearbetning. hello-lösningen: 

* ger en Vehicle telematik simulator
* utnyttjar Händelsehubbar för att föra in miljontals simulerade vehicle telemetriska händelser till Azure 
* använder Stream Analytics toogain realtidsinsikter på vehicle hälsa
* håller kvar hello data till långsiktig lagring för bättre batch analytics. 
* drar nytta av Machine Learning för identifiering av avvikelse i realtid och batch-bearbetning toogain förutsägande insikter.
* utnyttjar HDInsight tootransform data på skala och Data Factory toohandle orchestration, schemaläggning, resurshantering och övervakning av hello batch process-pipelinen 
* ger den här lösningen en omfattande instrumentpanel för data i realtid och förutsägelseanalys visualiseringar med Power BI

## <a name="architecture"></a>Arkitektur
![Arkitekturdiagram för lösningen](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*bild 1 – Vehicle telemetri Analytics lösningsarkitektur*

Den här lösningen omfattar hello följande **Cortana Intelligence-komponenterna** och visar deras slutet tooend integrering:

* **Händelsehubbar** för att föra in miljontals vehicle telemetriska händelser i Azure.
* **Strömma Analytics** för att få realtidsinsikter på vehicle hälsa och kvarstår dessa data till långsiktig lagring för bättre batch analytics.
* **Machine Learning** för identifiering av avvikelse i realtid och batchbearbetning toogain förutsägande insikter.
* **HDInsight** är balanserad tootransform data i skala
* **Data Factory** hanterar orchestration, schemaläggning, resurshantering och övervakning av hello batch process-pipelinen.
* **Power BI** ger den här lösningen en omfattande instrumentpanel för data i realtid och förutsägelseanalys visualiseringar.

Den här lösningen använder två olika **datakällor**: 

* **Simulerade vehicle signaler och diagnostik**: vehicle telematik simulator avger diagnostisk information och signalerar som motsvarar toohello tillståndet för hello vehicle och hello körning mönster vid en viss tidpunkt. 
* **Vehicle katalogen**: en referens dataset som innehåller en VIN toomodel mappning.

