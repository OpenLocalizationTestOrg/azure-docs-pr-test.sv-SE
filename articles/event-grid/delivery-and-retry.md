---
title: "aaaAzure händelse rutnätet leverans och försök igen"
description: "Beskriver hur Azure händelse rutnätet ger händelser och hur den hanterar felande meddelanden."
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/11/2017
ms.author: darosa
ms.openlocfilehash: 874b3bf8892fbf803ef40f29d0ec10eb50150916
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-message-delivery-and-retry"></a>Händelsen rutnätet meddelandeleverans och försök igen 

Den här artikeln beskrivs hur Azure händelse rutnätet hanterar händelser när leverans inte bekräftas.

Händelsen rutnätet innehåller varaktiga leverans. Den ger varje meddelande minst en gång för varje prenumeration. Händelser skickas toohello registrerad webhook för varje prenumeration omedelbart. Om en webhook inte bekräfta mottagandet av en händelse inom 60 sekunder från hello första leveransen försök, händelse rutnätet återförsök leverans av hello-händelse.

## <a name="message-delivery-status"></a>Status för leverans av meddelande

Händelsen rutnätet använder HTTP-svar koder tooacknowledge mottagandet av händelser. 

### <a name="success-codes"></a>Lyckade koder

hello indikera följande HTTP-svarskoder att en händelse har levererats har tooyour webhooken. Händelsen rutnätet anser leverans slutförd.

- 200 OK
- 202 godkända

### <a name="failure-codes"></a>Felkoder

hello efter http-svarskoder indikera att en händelse leverans försök misslyckades. Händelsen rutnätet försöker igen toosend hello händelse. 

- 400 Felaktig förfrågan
- 401 obehörig
- 404 Hittades inte
- 408 begäran-timeout
- 414 URI för lång
- 500 Internt serverfel
- 503 inte tillgänglig
- 504 gateway-Timeout

Andra svarskod eller brist på ett svar anger du ett fel. Händelsen rutnätet återförsök leverans. 

## <a name="retry-intervals"></a>Återförsöksintervall

Händelsen rutnätet använder en exponentiell backoff i principen för leverans av händelsen. Om din webhook svarar inte eller returnerar en felkod, försöker händelse rutnätet leverans på hello enligt schema:

1. 10 sekunder
2. 30 sekunder
3. 1 minut
4. 5 minuter
5. 10 minuter
6. 30 minuter
7. 1 timme

Händelsen rutnätet lägger till en liten slumpmässig tooall återförsöksintervall.

## <a name="retry-duration"></a>Försök varaktighet

Hello förhandsversionen Azure händelse rutnätet upphör att gälla alla händelser som inte levereras inom två timmar. Innan du allmän tillgänglighet kommer nu att ökad too24 timmar. 

## <a name="next-steps"></a>Nästa steg

* En introduktion tooEvent rutnätet finns [om händelsen rutnätet](overview.md).
* tooquickly komma igång med händelsen rutnätet, se [skapa och flöde anpassade händelser med Azure händelse rutnätet](custom-event-quickstart.md).
