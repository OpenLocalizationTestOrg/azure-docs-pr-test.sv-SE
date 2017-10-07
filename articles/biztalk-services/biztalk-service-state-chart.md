---
title: "aaaTasks tillåts i olika tillstånd eller status i BizTalk-tjänst | Microsoft Docs"
description: "Hej åtgärder/operationer som tillåts i olika MABS status: stoppa, starta, starta om, pausa, återuppta, ta bort, skala, uppdatera konfigurationen och säkerhetskopiera upp"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: aea738f3-ec76-4099-a41b-e17fea9e252f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2016
ms.author: mandia
ms.openlocfilehash: 643307ba6fa9b05c82b867912feab249c42b65dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-you-can-and-cant-do-using-hello-biztalk-service-state"></a>Vad du kan och inte kan göra med hello BizTalk Service tillstånd

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Beroende på hello aktuell status för hello BizTalk-tjänst finns som du kan eller inte kan utföra på hello BizTalk-tjänst.

Exempelvis kan du etablera en ny BizTalk-tjänst i hello klassiska Azure-portalen. När den är klar hello BizTalk-tjänst finns i `active` tillstånd. Hello aktiv, stoppa, pausa och ta bort hello BizTalk-tjänst. Om du stoppar hello BizTalk-tjänst och stoppa misslyckas och hello BizTalk-tjänst går sedan tooa `StopFailed` tillstånd. I hello `StopFailed` tillstånd, kan du starta om hello BizTalk-tjänst. Om du försöker en åtgärd som inte tillåts, t.ex. återuppta, inträffar hello följande fel:

`Operation not allowed`

## <a name="view-hello-possible-states"></a>Visa hello möjliga tillstånd

hello följande tabeller hello Liståtgärder eller åtgärder som kan göras när hello BizTalk Service är i ett visst tillstånd. En ✔ innebär hello åtgärden tillåts i det aktuella tillståndet. En tom post innebär hello-åtgärden inte kan utföras under det aktuella tillståndet.

| Tillstånd för tjänsten | Start | Stoppa | Starta om | Pausa | Återuppta | Ta bort | Skala | Uppdatering <br/> Konfiguration | Säkerhetskopiering |
| --- | --- | --- | --- | --- | --- | --- |--- | --- | --- |
| Active |  | ✔ | ✔ | ✔ |  | ✔ |✔ |✔ |✔ |
| Disabled |  |  |  |  |  | ✔ | |  |  | 
| avbruten |  |  |  |  | ✔ | ✔ | |  | ✔ |
| Stoppad | ✔ |  | ✔ |  |  | ✔ | |  | ✔ |
| Uppdatering av tjänsten misslyckades |  |  |  |  |  | ✔ | |  |  | 
| DisableFailed |  |  |  |  |  | ✔ | |  |  | 
| EnableFailed |  |  |  |  |  | ✔ | |  |  | 
| StartFailed <br/> StopFailed <br/> RestartFailed | ✔ | ✔ | ✔ |  |  | ✔ | | ✔ | |
| SuspendedFailed <br/> ResumeFailed|  |  |  | ✔ | ✔ | ✔ | |  |  | 
| CreatedFailed <br/> RestoreFailed |  |  |  |  |  | ✔ | |  |  | 
| ConfigUpdateFailed  |  |  | ✔ |  |  | ✔ | |✔ | |
| ScaleFailed |  |  |  |  |  | ✔ |✔ | |  |  | 



## <a name="see-also"></a>Se även
* [Skapa en BizTalk Service med hjälp av hello klassiska Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [Vad du kan göra på hello instrumentpanelen, övervaka och skala flikar i BizTalk-tjänst](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [Vad du får med hello utvecklare, Basic, Standard och Premium Edition i BizTalk-tjänst](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [Hur tooback upp och återställa en BizTalk Service](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [Begränsning förklaras i BizTalk-tjänst](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
* [Hämta hello Service Bus och åtkomstkontroll Utfärdarens namn och utfärdaren nyckelvärden för BizTalk Service](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
* [Hur jag börja använda hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

