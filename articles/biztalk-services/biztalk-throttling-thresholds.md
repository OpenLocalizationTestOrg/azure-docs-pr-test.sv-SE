---
title: "aaaLearn om begränsning i BizTalk-tjänst | Microsoft Docs"
description: "Läs mer om begränsning tröskelvärden och resulterande runtime beteenden för BizTalk-tjänst. Begränsning är baserad på minnesanvändning och antalet meddelanden. MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: f6663cf2-cda4-4bac-855e-27d2ad5c4fa4
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 46c8806c3a1f4eeb793f721f849771e0ec155197
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-throttling"></a>BizTalk Services: Begränsning

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk-tjänst implementerar tjänsten begränsning baserat på två villkor: minne användnings- och hello antalet samtidiga meddelanden bearbetning. Det här avsnittet visar hello begränsning tröskelvärden och beskriver hello Runtime beteende när ett bandbreddsbegränsning villkor uppfylls.

## <a name="throttling-thresholds"></a>Begränsning av tröskelvärden
följande tabell visar hello hello bandbreddsbegränsning käll- och tröskelvärden:

|  | Beskrivning | Lågtröskelövervakare | Tröskelvärde för hög |
| --- | --- | --- | --- |
| Minne |% av total system minne tillgängligt/PageFileBytes. <p><p>Totalt antal tillgängliga PageFileBytes är cirka 2 gånger hello RAM hello system. |60% |70% |
| Meddelandebehandling |Antal meddelanden som bearbetar samtidigt |40 * antal kärnor |100 * antal kärnor |

När en högre tröskelvärdet har uppnåtts startar toothrottle Azure BizTalk-tjänst. Begränsning slutar när hello låg tröskelvärde har uppnåtts. Till exempel använder din tjänst 65% av systemminnet. I den här situationen begränsning inte hello-tjänsten. Tjänsten startar med 70% av systemminnet. I den här situationen hello tjänsten begränsar och fortsätter toothrottle tills hello tjänsten använder 60% (låg tröskel) minne.

Azure BizTalk-tjänster spårar hello begränsning status (normala tillstånd jämfört med begränsad tillstånd) och hello begränsning varaktighet.

## <a name="runtime-behavior"></a>Runtime-beteende
När Azure BizTalk-tjänst anger tillståndet bandbreddsbegränsning, inträffar hello följande:

* Begränsning är per rollinstans. Exempel:<br/>
  RoleInstanceA begränsning. RoleInstanceB begränsning är inte. I den här situationen behandlas meddelanden i RoleInstanceB som förväntat. Meddelanden i RoleInstanceA ignoreras och misslyckas med hello följande fel:<br/><br/>
  **Servern är upptagen. Försök igen.**<br/><br/>
* Alla pull-källor inte söka eller hämta ett meddelande. Exempel:<br/>
  En pipeline tar emot meddelanden från en extern källa för FTP. Hej rollinstansen gör hello pull hämtar i bandbreddsbegränsning läge. I den här situationen stoppar hello pipeline hämtar ytterligare meddelanden tills hälsningspaket rollinstansen slutar begränsning.
* Ett svar skickas toohello klienten så hello-klienten kan skicka hello-meddelande.
* Du måste vänta tills hello begränsning har lösts. Mer specifikt måste du vänta tills hello låg tröskelvärdet har uppnåtts.

## <a name="important-notes"></a>Obs!
* Begränsning kan inte inaktiveras.
* Begränsning av tröskelvärden kan inte ändras.
* Begränsning har implementerats hela systemet.
* hello Azure SQL Database-Server har också inbyggd begränsning.

## <a name="additional-azure-biztalk-services-topics"></a>Avsnitt om ytterligare Azure BizTalk-tjänst
* [Installera hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Självstudier: Azure BizTalk-tjänst](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [Hur jag börja använda hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Azure BizTalk-tjänst](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Se även
* [BizTalk-tjänst: Utvecklare, Basic, Standard och Premium-utgåvor diagram](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: Etablering av statusdiagram](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Säkerhetskopiering och återställning](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Utfärdarens namn och nyckel](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

