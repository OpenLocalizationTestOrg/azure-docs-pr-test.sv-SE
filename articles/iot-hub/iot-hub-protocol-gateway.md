---
title: aaaAzure IoT-protokollet gateway | Microsoft Docs
description: "Hur protokollet toouse ett Azure IoT gateway tooextend IoT-hubb-funktioner och protokollet stöd tooenable enheter tooconnect tooyour händelsehubben med hjälp av protokoll som inte stöds av IoT-hubb internt."
services: iot-hub
documentationcenter: 
author: kdotchkoff
manager: timlt
editor: 
ms.assetid: 555e59ae-3136-4533-8ba8-f3a3b6acf648
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: kdotchko
ms.openlocfilehash: 9cfed30149672d8f7e021a9899192105bbcdd400
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Stöd för fler protokoll för IoT-hubb
Azure IoT-hubb har inbyggt stöd för kommunikation över hello MQTT AMQP och HTTP-protokoll. I vissa fall kan enheter eller gateways för fältet kanske inte kan toouse något av dessa standardprotokoll och kräver protokollet anpassning. I sådana fall måste använda du en anpassad gateway. En anpassad gateway kan aktivera protokollet anpassning för IoT-hubb-slutpunkter med bryggning hello trafik tooand från IoT-hubb. Du kan använda hello [Azure IoT-protokollet gateway](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) som en anpassad gateway tooenable protokollet anpassning för IoT-hubb.

## Azure IoT protocol-gateway
hello Azure IoT-protokollet gateway är ett ramverk för protokollet anpassning som är avsedd för hög skalning, enheten dubbelriktad kommunikation med IoT-hubben. hello protocol-gateway är en direkt komponent som accepterar enhetsanslutningar över ett visst protokoll. Det få hello trafik tooIoT Hub via AMQP 1.0. hello Azure IoT-protokollet gateway är tillgänglig som en öppen källkod projektet tooprovide flexibilitet för att lägga till stöd för olika protokoll och protokoll version.

Du kan distribuera hello protocol-gateway i Azure på ett mycket skalbart sätt med hjälp av Azure Service Fabric, arbetsroller i Azure Cloud Services och Windows-datorer. Dessutom kan hello protocol-gateway distribueras i lokala miljöer, t.ex fältet gateways.

hello Azure IoT-protokollet gateway innehåller en MQTT protocol-kortet som gör att du toocustomize hello MQTT protokollbeteendena om det behövs. Eftersom IoT-hubben har inbyggt stöd för hello MQTT v3.1.1 protokoll, bör endast du använda hello MQTT protocol-kortet om protokollet anpassningar eller särskilda krav för ytterligare funktioner krävs.

Hej MQTT nätverkskort visas också hello programmeringsmodell för att skapa protokollet nätverkskort för andra protokoll. Hello Azure IoT-protokollet gateway programmeringsmodell kan dessutom tooplug i anpassade komponenter för särskild bearbetning som anpassad autentisering, meddelande omvandlingar, komprimering/dekomprimering eller kryptering och dekryptering av trafik mellan hello enheter och IoT-hubb.

För flexibilitet finns hello protocol-gateway och MQTT implementering i ett projekt med öppen källkod. Detta ger dig toocustomize hello implementering efter behov.

## Nästa steg
Mer om hello Azure IoT-protokollet gateway toolearn och hur toouse och distribuera den som en del av IoT-lösningen, se:

* [Azure IoT-protokollet gateway-databasen på GitHub](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Azure IoT-protokollet gateway utvecklarhandboken](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

toolearn mer information om hur du planerar din IoT-hubb-distribution, se:

* [Jämför med Händelsehubbar][lnk-compare]
* [Skalning, hög tillgänglighet och Katastrofåterställning][lnk-scaling]
* [Utvecklarhandbok för IoT-hubb][lnk-devguide]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
