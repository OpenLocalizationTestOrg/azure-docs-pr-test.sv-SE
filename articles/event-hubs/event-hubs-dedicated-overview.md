---
title: aaaOverview Azure Event Hubs dedikerade kapacitet | Microsoft Docs
description: "Översikt över Microsoft Azure Event Hubs dedikerade kapacitet."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: sethm;babanisa
ms.openlocfilehash: 79c09975e5c0a6d4729c8f836576770abaaf322e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-event-hubs-dedicated"></a>Översikt av Händelsehubbar dedikerad

*Event Hubs dedikerade* kapacitet ger stöd för en innehavare distributioner för kunder med hello mest krävande krav. I full skala Azure Event Hubs kan ingång över 2 miljoner händelser per sekund eller in too2 GB per sekund telemetri med fullständigt varaktiga svarstid för lagring och sekund. Den här även aktiverar integrerad lösningar genom att behandla realtid och batch på hello samma system. Du kan minska hello komplexiteten i lösningen genom att ha en enda dataström som stöder både i realtid och batch-baserade pipelines med Event Hubs Arkiv ingår i erbjudandet hello.

hello jämförs följande tabell hello tillgängliga tjänstnivåer i Händelsehubbar. hello Event Hubs dedikerad erbjudande är en fast månadsavgift jämfört med toousage priser för de flesta funktioner i Standard- och Basic. hello dedikerade nivå erbjuder hello funktioner för hello standardplan, men med enterprise skala kapacitet för kunder med större arbetsbelastning. 

| Funktion | Basic | Standard | Dedikerad |
| --- |:---:|:---:|:---:|
| Ingångshändelser | Betala per miljoner händelser | Betala per miljoner händelser | Ingår |
| Genomflödesenhet (1 MB per sekund ingång, utgång 2 MB per sekund) | Betala per timme | Betala per timme | Ingår |
| Meddelandestorlek | 256 kB | 256 kB | 1 MB |
| Utgivarprinciper | Saknas | Ja | Ja |     
| Konsumentgrupper | 1 - standard | 20 | 20 |
| Återuppspelning av meddelande | Ja | Ja | Ja |
| Högsta antal Throughput Units | 20 | 20 (flexibel too100)  | 1 kapacitetsenhet ≈ 200 |
| Brokered Connections | 100 ingår | 1 000 ingår | 100 K ingår |
| Fler Brokered Connections | Saknas | Ja | Ja |
| Meddelandelagring | 1 dag ingår | 1 dag ingår | Upp too7 dagar ingår |
| Arkiv (förhandsgranskning) | Saknas   | Betala per timme | Ingår |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>Fördelarna med Event Hubs dedikerade kapacitet

När du använder Event Hubs dedikerad finns hello följande fördelar:

* En organisation som värd har inga brus från andra klienter.
* Meddelandet som ökar storleken too1 MB jämfört too256 KB för Standard- och Basic.
* Repeterbara prestanda varje gång.
* Garanterad kapacitet toomeet din burst måste.
* Scalable mellan 1 och 8 kapacitetsenheter (CU) – ger upp too2 miljoner ingångshändelser per sekund.
  * Anpassad Communi hantera hello skalan för Event Hubs dedikerade, där varje CU kan ange cirka hello motsvarigheten till 200 genomflödesenheter (TU).
* Noll Underhåll: vi hanterar belastningsutjämning, OS uppdateringar, säkerhetsuppdateringar och partitionering.
* Fast priser för varje månad.

Event Hubs dedikerat tar också bort vissa av hello genomströmningsbegränsningar för hello Standard erbjudande. Genomflödesenheter i nivåerna Basic och Standard berättiga too1000 händelser per sekund eller 1 MB per sekund ingång per TU och dubbla denna mängd utgång. hello dedikerade skala erbjudande har inga begränsningar för ingång och utgång händelse räknar. Dessa gränser regleras bara av hello bearbetning av kapacitet för hello köpt händelsehubbar.

Den här tjänsten är riktad mot hello största telemetri användare och är tillgängliga toocustomers med ett enterprise-avtal.

## <a name="how-tooonboard"></a>Hur tooonboard

hello Event Hubs dedikerad plattform erbjuds via ett enterprise-avtal med olika storlekar för anpassad Communi. Varje CU innehåller ungefär hello motsvarigheten till 200 genomflödesenheter. Du kan skala din kapacitet uppåt eller nedåt i hela hello månad toomeet dina behov genom att lägga till eller ta bort anpassad Communi. hello dedikerade plan är unikt i att du får en mer praktiska onboarding från hello Händelsehubbar produkten team tooget hello flexibel distribution som passar dig. 

## <a name="next-steps"></a>Nästa steg
Kontakta Microsoft säljare eller Microsoft-supporten tooget ytterligare information om händelsen hubbar dedikerade kapacitet. Du kan också få mer information om Händelsehubbar genom att besöka hello följande länkar:

Detaljerad information om priser finns i hello följande länkar:

- [Event Hubs dedikerad priser](https://azure.microsoft.com/pricing/details/event-hubs/). Du kan också kontakta Microsoft säljare eller Microsoft-supporten tooget ytterligare information om händelsen hubbar dedikerade kapacitet.
- Hej [Event Hubs FAQ](event-hubs-faq.md) innehåller information om priser och besvarar några vanliga frågor om Händelsehubbar. 
