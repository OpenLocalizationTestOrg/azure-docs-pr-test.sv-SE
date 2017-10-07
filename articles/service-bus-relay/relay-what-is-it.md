---
title: "aaaWhat är Azure Relay och varför ska jag använda översikt | Microsoft Docs"
description: "Översikt över Azure Relay"
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1e3e971d-2a24-4f96-a88a-ce3ea2b1a1cd
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: 4cfd77048210a435c446b908b7896737cad0edbf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-relay"></a>Vad är Azure Relay?

hello Azure vidarebefordrande tjänsten underlättar hybrid program genom att aktivera du toosecurely exponera tjänster som finns i ett företagsnätverk som kopplar nätverk toohello offentliga moln, utan att behöva tooopen en brandväggsanslutning eller kräva störande ändringar tooa företagets nätverksinfrastruktur. Relay stöder en mängd olika transportprotokoll och webbtjänststandarder.

hello vidarebefordrande tjänsten stöder traditionella enkelriktat frågor och svar och peer-to-peer-trafik. Den stöder även händelsedistribution på internet-skala tooenable publicera och prenumerationsscenarier och dubbelriktad socketkommunikation för ökad point-to-point effektivitet. 

I hello vidarebefordrande data transfer mönster, en lokal tjänst ansluter toohello vidarebefordrande tjänsten via en utgående port och skapar en dubbelriktad socket för kommunikation som är kopplad tooa viss rendezvous-adress. hello-klienten kan sedan kommunicera med hello lokala tjänsten genom att skicka trafik toohello vidarebefordrande tjänsten hello rendezvous-adressen som mål. hello vidarebefordrande tjänsten vidarebefordrar ”” data toohello lokala tjänsten via en dedikerad tooeach dubbelriktad socket-klient. hello klienten behöver inte ha en direktanslutning toohello lokal tjänst och kan inte nödvändiga tooknow där hello-tjänsten finns och hello lokala tjänsten behöver inte ha några inkommande portar öppna hello-brandväggen.

hello kapaciteten för viktiga element som tillhandahålls av Relay är dubbelriktad, obuffrade kommunikation över nätverksgränser med TCP-liknande begränsning, endpoint-identifiering, anslutningsstatus och slutpunktssäkerhet ett lager. hello relay funktioner skiljer sig från på nätverksnivå integration tekniker som VPN, i den relay kan vara begränsade tooa programslutpunkt på en enskild dator medan VPN-teknik är mycket mer påträngande som använder ändra hello nätverksmiljö .

Azure Relay erbjuder två funktioner:

1. [Hybridanslutningar](#hybrid-connections) - använder hello öppna standard webbplats sockets att aktivera scenarier med flera plattformar.
2. [WCF-reläer](#wcf-relays) -använder Windows Communication Foundation (WCF) tooenable RPC-anrop. Vidarebefordrande WCF är hello äldre relay erbjuder många kunder redan använder med deras WCF programmeringsmodeller.

Hybridanslutningar och WCF-reläer aktivera säker anslutning tooassets som finns i ett företagsnätverk. Användning av något över hello andra beror på dina specifika behov enligt beskrivningen i följande tabell hello:

|  | WCF-relä | Hybridanslutningar |
| --- |:---:|:---:|
| **WCF** |x | |
| **.NET Core** | |x |
| **.NET Framework** |x |x |
| **JavaScript/NodeJS** | |x |
| **Standardbaserat öppet protokoll** | |x |
| **Flera RPC-programmeringsmodeller** | |x |

## <a name="hybrid-connections"></a>Hybridanslutningar

Hej [Azure Relay Hybridanslutningar](relay-hybrid-connections-protocol.md) funktion är en säker, öppna protokoll utvecklingen av hello befintliga Relay-funktioner som kan implementeras på alla plattformar och språk som har en grundläggande WebSocket-funktion som explicit innehåller hello WebSocket API i vanliga webbläsare. Hybridanslutningar baseras på HTTP och WebSockets.

### <a name="service-history"></a>Tjänsthistorik

Hybridanslutningar skickas hello f.d., på samma sätt med namnet ”BizTalk-tjänst”-funktion som har skapats på hello Azure Service Bus WCF Relay. hello nya Hybridanslutningar funktionen kompletterar hello befintliga vidarebefordrande WCF-funktionen och dessa två funktioner finns sida vid sida i hello Azure vidarebefordrande tjänsten. De delar en gemensam gateway, men är i övrigt olika implementeringar.

## <a name="wcf-relays"></a>WCF-reläer

hello vidarebefordrande WCF fungerar för hello fullständig .NET Framework (NETFX) och för WCF. Du kan initiera hello anslutningen mellan din lokala tjänst och hello vidarebefordrande tjänsten med hjälp av en uppsättning ”vidarebefordra” WCF-bindningar. Hello bakgrunden mappas vidarebefordringsbindningarna hello toonew transport bindning utformade toocreate WCF-kanalkomponenter som integreras med Service Bus i molnet hello.

## <a name="architecture-processing-of-incoming-relay-requests"></a>Arkitektur: Bearbetning av inkommande vidarebefordrade begäranden
När en klient skickar en begäran toohello [Azure Relay](/azure/service-bus-relay/) service, hello Azure belastningsutjämnare skickar den tooany av hello gateway-noder. Om hello-begäran är en begäran om lyssna, skapas en ny vidarebefordran hello gateway-noden. Om hello-begäran är en specifik vidarebefordran för anslutningen begäran tooa, vidarebefordrar hello anslutning begäran toohello gateway-noden som äger hello relay hello gateway-noden. hello gateway-noden som äger hello relay skickar en rendezvous-begäran toohello lyssnande klienten och ber hello lyssnare toocreate en tillfällig kanal toohello gateway-noden som tog emot begäran om hello-anslutning.

När hello relay-anslutning har upprättats utbyter hello klienter meddelanden via hello gateway-noden som används för hello rendezvous.

![Bearbetning av inkommande WCF Relay-begäranden](./media/relay-what-is-it/ic690645.png)

## <a name="next-steps"></a>Nästa steg

* [Vanliga frågor och svar om Relay](relay-faq.md)
* [Skapa ett namnområde](relay-create-namespace-portal.md)
* [Kom igång med .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Kom igång med Node](relay-hybrid-connections-node-get-started.md)

