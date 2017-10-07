---
title: "Undvika avbrott i tjänsten med Azure Stream Analytics-jobb | Microsoft Docs"
description: "Riktlinjer för att göra Stream Analytics jobb uppgraderingen flexibel."
services: stream-analytics
documentationCenter: 
authors: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 3ac65c93ecb47e93e963dd9869a7af70f73b19c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Garantera Stream Analytics-jobbet tillförlitlighet under uppdateringar av tjänsten

En del av en helt hanterad tjänst som är hello kapaciteten toointroduce nya service funktioner och förbättringar i en snabb takt. Stream Analytics kan därför ha en tjänstuppdatering distribuera varje vecka (eller oftare). Oavsett hur mycket testning finns fortfarande en risk för att en befintlig, körs jobbet avbryts på grund av toohello införandet av ett programfel. För kunder som kör kritiska strömmande bearbetning jobb måste riskerna toobe undvikas. En mekanism för kunder kan använda tooreduce risken är Azures  **[parad region](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  modell. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Hur parad Azure-regioner adressera detta?

Stream Analytics garanterar jobb i parad regioner har uppdaterats i separata grupper. Det finns därför en tillräcklig lucka mellan hello uppdateringar tooidentify potentiella bryta buggar och åtgärda dem.

_Med undantag för hello av centrala Indien_ (vars parad region, södra Indien och inte har Stream Analytics förekomst), hello distribution av en uppdatering tooStream Analytics inte skulle inträffar vid hello samma tid i en parad regioner. Distribution i flera områden **i hello samma grupp** kan uppstå **på hello samtidigt**.

hello artikel på  **[tillgänglighet och parad regioner](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  har hello allra senaste informationen som regioner har parats ihop.

Kunder är uppmanade toodeploy identiska jobb tooboth länkas regioner. Dessutom tooStream Analytics interna övervakningsfunktionerna kunder är också bäst toomonitor hello jobb som om **både** produktion jobb. Om ett avbrott identifierade toobe ett resultat av hello Stream Analytics tjänstuppdatering eskalera på rätt sätt och över alla underordnade konsumenter toohello felfri jobbutdata. Eskalering toosupport kommer att förhindra att hello parad region påverkas av hello ny distribution och underhålla hello integriteten hos hello länkas jobb.
