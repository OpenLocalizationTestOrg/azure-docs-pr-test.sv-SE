---
title: 'aaaOverview av hello Azure Relay nod API: erna | Microsoft Docs'
description: "Vidarebefordra nod API-översikt"
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b7d6e822-7c32-4cb5-a4b8-df7d009bdc85
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: sethm
ms.openlocfilehash: d231acc854be0eaa965dec0229cf63b08ff27067
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="relay-hybrid-connections-node-api-overview"></a>Översikt över Hybrid anslutningar nod API-relä

## <a name="overview"></a>Översikt

Hej [ `hyco-ws` ](https://www.npmjs.com/package/hyco-ws) nod paketet för Azure Relay Hybridanslutningar bygger på och utökar hello ['ws'](https://www.npmjs.com/package/ws) NPM-paketet. Det här paketet igen exporterar alla export av det grundläggande paketet och lägger till nya export som möjliggör integrering med hello Azure vidarebefordrande tjänsten Hybridanslutningar funktion. 

Befintliga program som `require('ws')` kan använda det här paketet med `require('hyco-ws')` i stället som kan också hybridscenarier där ett program kan lyssna efter WebSocket-anslutningar från lokalt ”innanför hello brandvägg” och via Hybridanslutningar, allt på Hej samma tid.
  
## <a name="documentation"></a>Dokumentation

hello API: er är [dokumenterade i hello huvudsakliga ws paketet](https://github.com/websockets/ws/blob/master/doc/ws.md). Den här artikeln beskriver hur det här paketet skiljer sig från den baslinjen. 

hello viktiga skillnader mellan grundläggande hello-paket och den här 'hyco-ws' är det lägger till en ny serverklass som exporteras via `require('hyco-ws').RelayedServer`, och några hjälpmetoder.

### <a name="package-helper-methods"></a>Paketet hjälpmetoder

Det finns flera metoder för hello paketet som du kan referera till enligt följande:

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

Hej hjälpmetoder inte kan användas med det här paketet, men kan även användas av en nod-server för att aktivera webb- eller klienter toocreate lyssnare eller avsändare. hello-servern använder dessa metoder genom att skicka dem till URI: er som bädda in tillfällig token. Dessa URI: er kan också användas med vanliga WebSocket-stackar som inte stöder inställningen HTTP-huvuden för hello WebSocket-handskakning. Bädda in auktorisering token i hello URI stöds för dessa bibliotek externa Användningsscenarier. 

#### <a name="createrelaylistenuri"></a>createRelayListenUri

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

Skapar en giltig Azure Relay Hybridanslutning-lyssnare URI för hello namnområdet och sökvägen. Den här URI kan sedan användas med hello relay-versionen av hello WebSocketServer klass.

- `namespaceName`(krävs) - hello domän kvalificerade namnet på hello Azure Relay namnområdet toouse.
- `path`namnet på en befintlig Azure Relay Hybrid-anslutning i detta namnområde (krävs) - hello.
- `token`(valfritt) – en Relay-åtkomst som tidigare utfärdade token som är inbäddad i hello-lyssnaren URI (se följande exempel hello).
- `id`(valfritt) – ett spårnings-ID som möjliggör slutpunkt till slutpunkt diagnostik uppföljning av begäranden.

Hej `token` värdet är valfritt och bör endast användas när det inte är möjligt toosend HTTP-huvuden tillsammans med hello WebSocket-handskakningen eftersom hello fallet med hello W3C WebSocket-stacken.                  


#### <a name="createrelaysenduri"></a>createRelaySendUri
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

Skapar en giltig Azure Relay Hybridanslutning-skicka URI för hello namnområdet och sökvägen. Den här URI kan användas med WebSocket-klienten.

- `namespaceName`(krävs) - hello domän kvalificerade namnet på hello Azure Relay namnområdet toouse.
- `path`namnet på en befintlig Azure Relay Hybrid-anslutning i detta namnområde (krävs) - hello.
- `token`(valfritt) – skicka ett tidigare utfärdade Relay åtkomst-token som är inbäddad i hello URI (se följande exempel hello).
- `id`(valfritt) – ett spårnings-ID som möjliggör slutpunkt till slutpunkt diagnostik uppföljning av begäranden.

Hej `token` värdet är valfritt och bör endast användas när det inte är möjligt toosend HTTP-huvuden tillsammans med hello WebSocket-handskakningen eftersom hello fallet med hello W3C WebSocket-stacken.                   


#### <a name="createrelaytoken"></a>createRelayToken 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Skapar en Azure Relay delade signatur åtkomst (SAS)-token för hello givet mål-URI, SAS-regeln och SAS regeln nyckel som är giltig för hello får antal sekunder eller en timme från hello aktuella snabbmeddelanden om hello utgången argument utelämnas.

- `uri`(krävs) - hello URI för vilka hello token är toobe utfärdas. hello URI är normaliserat toouse hello HTTP-schemat och läsa sträng information tas bort.
- `ruleName`(krävs) - SAS Regelnamn för antingen hello entitet som representeras av hello angivna URI: N, eller för hello namnområde åt hello värddelen URI.
- `key`(krävs) - giltig nyckel för hello SAS-regel. 
- `expirationSeconds`(valfritt) – hello antalet sekunder tills hello genererats token ska upphöra att gälla. Om inget anges är hello standardvärdet 1 timme (3600).

hello utfärdade token ger hello rättigheter som är associerade med hello angetts SAS-regel för hello angivna varaktighet.

#### <a name="appendrelaytoken"></a>appendRelayToken

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

Den här metoden är funktionellt motsvarande toohello `createRelayToken` metoden beskrivs tidigare, men returnerar hello token korrekt tillagda toohello indata-URI.

### <a name="class-wsrelayedserver"></a>Klassen ws. RelayedServer

Hej `hycows.RelayedServer` klassen är en alternativ toohello `ws.Server` klass som inte avlyssna hello lokala nätverk, men delegerar lyssnande toohello Azure vidarebefordrande tjänsten.

hello två klasser är främst kontraktet som är kompatibla, vilket innebär att ett befintligt program med hjälp av hello `ws.Server` klassen enkelt kan ändrade toouse hello vidarebefordrande version. hello viktigaste skillnaderna är i hello konstruktorn och hello tillgängliga alternativ.

#### <a name="constructor"></a>Konstruktorn  

```JavaScript 
var ws = require('hyco-ws');
var server = ws.RelayedServer;

var wss = new server(
    {
        server: ws.createRelayListenUri(ns, path),
        token: function() { return ws.createRelayToken('http://' + ns, keyrule, key); }
    });
```

Hej `RelayedServer` konstruktorn stöder en annan uppsättning argument än hello `Server`eftersom det inte är en fristående lyssnare eller kan toobe bädda in i en befintlig http-lyssnaren framework. Det finns också färre alternativ eftersom hello WebSocket-management är i stort sett delegerad toohello vidarebefordrande tjänsten.

Konstruktorargument:

- `server`(krävs) - kvalificerade hello fullständigt URI för en Hybridanslutning på vilka toolisten vanligtvis konstruerade med hello WebSocket.createRelayListenUri() hjälpmetod.
- `token`(krävs) - innehåller det här argumentet en tidigare utfärdade token sträng eller en callback-funktion som kan anropas tooobtain en token sträng. hello återanrop alternativ rekommenderas eftersom det token förnyelse.

#### <a name="events"></a>Händelser

`RelayedServer`instanser genererar tre händelser som aktiverar du toohandle inkommande begäranden, upprätta anslutningar och identifiera felvillkor. Du måste prenumerera toohello `connect` toohandle händelsemeddelanden. 

##### <a name="headers"></a>Rubriker

```JavaScript 
function(headers)
```

Hej `headers` händelse utlöses innan en inkommande anslutning är godkända att aktivera ändringen av hello huvuden toosend toohello klienten. 

##### <a name="connection"></a>anslutning

```JavaScript
function(socket)
```

Orsakat när en ny WebSocket-anslutning accepteras. hello-objektet är av typen `ws.WebSocket`, samma som med grundläggande hello-paket.


##### <a name="error"></a>fel

```JavaScript
function(error)
```

Om hello underliggande server genererar ett fel, skickas den här.  

#### <a name="helpers"></a>Hjälpprogram

toosimplify startar en vidarebefordrande server och omedelbart prenumerera tooincoming anslutningar hello paketet visar en enkel hjälpfunktion som också används i hello exempel på följande sätt:

##### <a name="createrelayedlistener"></a>createRelayedListener

```JavaScript
var WebSocket = require('hyco-ws');

var wss = WebSocket.createRelayedServer(
    {
        server: WebSocket.createRelayListenUri(ns, path),
        token: function() { return WebSocket.createRelayToken('http://' + ns, keyrule, key); }
    }, 
    function (ws) {
        console.log('connection accepted');
        ws.onmessage = function (event) {
            console.log(JSON.parse(event.data));
        };
        ws.on('close', function () {
            console.log('connection closed');
        });       
});
``` 

##### <a name="createrelayedserver"></a>createRelayedServer

```javascript
var server = createRelayedServer([options], [connectCallback] )
```

Den här metoden anropar hello konstruktorn toocreate en ny instans av hello RelayedServer och prenumererar hello angivna återanropet toohello anslutning händelse.
 
##### <a name="relayedconnect"></a>relayedConnect

Bara spegling hello `createRelayedServer` helper i funktionen `relayedConnect` skapar en klientanslutning och prenumererar toohello 'öppen' händelse på hello resulterande socket.

```JavaScript
var uri = WebSocket.createRelaySendUri(ns, path);
WebSocket.relayedConnect(
    uri,
    WebSocket.createRelayToken(uri, keyrule, key),
    function (socket) {
        ...
    }
);
```

## <a name="next-steps"></a>Nästa steg
toolearn mer om Azure Relay finns i följande länkar:
* [Vad är Azure Relay?](relay-what-is-it.md)
* [Tillgängliga Relay-API: er](relay-api-overview.md)
