---
title: aaaOverview Azure IoT kant | Microsoft Docs
description: "Beskriver hello arkitektur nyckelbegrepp i Azure IoT kant som gateways, moduler och mäklare."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dobett
ms.openlocfilehash: 32debc0d4f40cfd7f2cce7cf8c76b12ec18ee2dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-edge-architectural-concepts"></a>Azure IoT kant arkitektur begrepp

Innan du granska alla exempelkod eller skapa egna fältet gateway med IoT kant, bör du förstå hello viktiga begrepp som ligger till grund för hello arkitekturen för IoT kant.

## <a name="iot-edge-modules"></a>IoT-Edge-moduler

Du skapar en gateway med Azure IoT kant genom att skapa och montera *IoT kant moduler*. Använda moduler *meddelanden* tooexchange data med varandra. En modul tar emot ett meddelande, utför en åtgärd, omvandlar eventuellt det till ett nytt meddelande och publicerar den för andra moduler tooprocess. Vissa moduler kan bara generera nya meddelanden och bearbetar aldrig inkommande meddelanden. En kedja av moduler skapar en pipeline för databearbetning med varje modul som utför en omvandling på hello data en gång i denna pipeline.

![En kedja med moduler i en gateway som skapats med Azure IoT Egde][1]

IoT-Edge innehåller hello följande komponenter:

* Färdig moduler som utför vanliga gateway-funktioner.
* hello gränssnitt en utvecklare kan använda toowrite anpassade moduler.
* Hej infrastruktur nödvändiga toodeploy och köra en uppsättning moduler.

hello SDK tillhandahåller ett Abstraktionslager som gör att du toobuild gateways toorun på olika operativsystem och plattformar.

![Azure IoT Edge-abstraktionslager][2]

## <a name="messages"></a>Meddelanden

Även om du tänker moduler som skickar meddelanden tooeach andra är ett bekvämt sätt tooconceptualize hur en gateway-funktioner, den inte korrekt speglar vad som händer. IoT-Edge moduler använder en broker toocommunicate med varandra. Moduler publicera meddelanden toohello broker (med meddelandemönster, till exempel bus eller publicera/prenumerera) och därefter låta hello broker väg hello meddelandet toohello moduler anslutna tooit.

En modul använder hello **Broker_Publish** toopublish förhandlare meddelandet toohello att fungera. hello broker levererar meddelanden tooa modulen genom att anropa en callback-funktion. Ett meddelande består av en uppsättning nyckel-/värdeegenskaper och innehåll som skickas som ett block med minne.

![hello roll hello Broker i Azure IoT kant][3]

## <a name="message-routing-and-filtering"></a>Meddelandedirigering och filtrering

Det finns två sätt toodirect meddelanden toohello rätt IoT kant moduler:

* Du kan skicka en uppsättning länkar kan skickas toohello broker så hello broker vet hello källa och mottagare för varje modul.
* En modul kan filtrera efter hello egenskaper för hello-meddelande.

En modul bör endast agera på ett meddelande om hello-meddelande är avsedd för den. Länkar och meddelandet filtrering effektivt skapar du en message-pipeline.

## <a name="next-steps"></a>Nästa steg

toosee dessa begrepp som används i ett prov du kan köra finns [utforska Azure IoT kant arkitektur][lnk-hello-world].

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md