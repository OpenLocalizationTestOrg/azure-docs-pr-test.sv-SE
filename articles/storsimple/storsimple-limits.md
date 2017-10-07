---
title: "aaaStorSimple 8000-serien system gränser | Microsoft Docs"
description: "Beskriver system gränser och rekommenderade storlekar för StorSimple 8000-serien komponenter och anslutningar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c7392678-0924-46c6-9c59-1665cb9b6586
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5f21862989f23f2fa4cf02c884cc0fc85c6cc2bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-storsimple-8000-series-system-limits"></a>Vad är StorSimple 8000-serien system gränser?
## <a name="overview"></a>Översikt
StorSimple innehåller skalbara och flexibla lagring för ditt datacenter. Det finns dock vissa begränsningar som du bör tänka på när du planerar, distribuerar och fungerar din StorSimple-lösning. hello följande tabell beskriver dessa begränsningar och innehåller några rekommendationer så att du kan få hello mest av din StorSimple-lösning.

| Gräns för identifierare | Gräns | Kommentarer |
| --- | --- | --- |
| Maximalt antal lagringskontouppgifter |64 | |
| Maximalt antal volymbehållare |64 | |
| Maximalt antal volymer |255 | |
| Maximalt antal lokalt fästa volymer |32 | |
| Maximalt antal scheman per bandbreddsmall |168 |Ett schema för varje timme, varje dag i veckan hello (24 * 7). |
| Maximal storlek för en nivåindelad volym på fysiska enheter |64 TB för 8100 och 8600 |8100 och 8600 är fysiska enheter. |
| Maximal storlek för en nivåindelad volym på virtuella enheter i Azure |30 TB för 8010 <br></br> 64 TB för 8020 |8010 och 8020 är virtuella enheter i Azure som använder standardlagring respektive Premium-lagring. |
| Maximal storlek för en lokalt Fäst volym på fysiska enheter |8.5 TB för 8100 <br></br> 22,5 TB för 8600 |8100 och 8600 är fysiska enheter. |
| Maximalt antal iSCSI-anslutningar |512 | |
| Maximalt antal iSCSI klientanslutningar från initierarna |512 | |
| Högsta antal access control poster per enhet |64 | |
| Maximalt antal volymer per princip för säkerhetskopiering |20 | |
| Maximalt antal säkerhetskopior behålls per schema (i en princip för säkerhetskopiering) |64 | |
| Maximalt antal scheman per princip för säkerhetskopiering |10 | |
| Maximalt antal ögonblicksbilder av typer som kan behållas per volym |256 |Antalet inkluderar lokala ögonblicksbilder och molnbaserade ögonblicksbilder. |
| Maximalt antal ögonblicksbilder som kan finnas i valfri enhet |10 000 | |
| Maximalt antal volymer som kan bearbetas parallellt för säkerhetskopiering, återställning eller klona |16 |<ul><li>Om det finns fler än 16 volymer, tur de och ordning när bearbetningen fack blir tillgängliga.</li><li>Nya säkerhetskopior av en klonade eller en återställd nivåindelad volym kan inte ske förrän hello-åtgärden har slutförts. Men för en lokal volym är säkerhetskopior tillåtna när hello volymen är online.</li></ul> |
| Återställa och klona Återställ tid för nivåindelade volymer |< 2 minuter |<ul><li>hello volym görs tillgänglig inom 2 minuter för återställning eller klona igen, oavsett hello volymens storlek.</li><li>hello gå volymen först långsammare än normalt som de flesta hello data och metadata finns fortfarande i hello molnet. Prestanda kan öka som data flödar från hello molnet toohello StorSimple-enhet.</li><li>hello total tid toodownload metadata är beroende av hello allokerade volymens storlek. Metadata hämtas automatiskt till hello-enhet i hello bakgrunden i hello frekvens på 5 minuter per TB allokerade volymdata. Detta värde kan påverkas av Internet bandbredd toohello moln.</li><li>Hej återställning eller kopieringen är klar när alla hello metadata är på hello enhet.</li><li>Säkerhetskopieringsåtgärder kan inte utföras förrän hello återställning eller kopieringen är helt klar. |
| Återställning återställa tid för lokalt fästa volymer |< 2 minuter |<ul><li>hello volym görs tillgänglig inom 2 minuter efter hello återställningen oavsett hello volymens storlek.</li><li>hello gå volymen först långsammare än normalt som de flesta hello data och metadata finns fortfarande i hello molnet. Prestanda kan öka som data flödar från hello molnet toohello StorSimple-enhet.</li><li>hello total tid toodownload metadata är beroende av hello allokerade volymens storlek. Metadata hämtas automatiskt till hello-enhet i hello bakgrunden i hello frekvens på 5 minuter per TB allokerade volymdata. Detta värde kan påverkas av Internet bandbredd toohello moln.</li><li>Till skillnad från nivåindelade volymer för lokalt fästa volymer hämtas även hello volymdata lokalt på hello enhet. hello återställningen är klar när alla hello volymdata har trätt toohello enhet.</li><li>hello återställningsåtgärderna kan bli långa. hello total tid toocomplete hello återställning beror på hello storleken på hello etablerats lokal volym och din Internet-bandbredd och hello befintliga data på hello enhet. Säkerhetskopieringsåtgärder på hello lokalt Fäst volym är tillåtna när hello återställningen pågår. |
| Behandlingstakt för molnögonblicksbilder |15 minuter/TB |<ul><li>Minimitid toomake hello ögonblicksbild i molnet klar för överföring per TB allokerad volymdata i säkerhetskopian. </li><li> Totalt antal molnet ögonblicksbild tid beräknas genom att lägga till den här gången toohello ögonblicksbild överföringstid som påverkas av hello Internet bandbredd toocloud. |
| Maximalt antal klienter läsning och skrivning genomströmning (när hanteras från hello SSD-nivån) * |920/720 MB/s med ett enda 10 GbE-nätverksgränssnitt |Konfigurera too2x med MPIO och två nätverksgränssnitt. |
| Maximalt antal klienter läsning och skrivning genomströmning (när hanteras från hello hårddisknivå) * |120/250 MB/s | |
| Maximalt antal klienter läsning och skrivning genomströmning (när hanteras från hello molnnivån) * för uppdatering 3 och senare ** |40/60 MB/s för nivåindelade volymer<br><br>60/80 MB/s för nivåindelade volymer med arkivering alternativet under skapande av volym |Läs genomströmning beroende klienter skapa och upprätthålla tillräcklig i/o-ködjup. <br><br>Hastigheten uppnås beror på hello hastighet hello underliggande lagringskontot används. |

&#42; Maximalt dataflöde per i/o-typ har mätt med 100 procent Läs- och 100 procent skrivåtgärder scenarier. Faktiska genomflöde kan vara lägre och beror på i/o blanda och nätverk villkor.

&#42; &#42; Prestanda siffror tidigare tooUpdate 3 kan vara lägre.

## <a name="next-steps"></a>Nästa steg
Granska hello [StorSimple systemkrav](storsimple-system-requirements.md). 

