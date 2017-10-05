---
title: "Översikt över Azure Relay nod-API: er | Microsoft Docs"
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
ms.openlocfilehash: 28526c05c7f364f0fcaaa362fc97857f850040ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="relay-hybrid-connections-node-api-overview"></a><span data-ttu-id="5e308-103">Översikt över Hybrid anslutningar nod API-relä</span><span class="sxs-lookup"><span data-stu-id="5e308-103">Relay Hybrid Connections Node API overview</span></span>

## <a name="overview"></a><span data-ttu-id="5e308-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="5e308-104">Overview</span></span>

<span data-ttu-id="5e308-105">Den [ `hyco-ws` ](https://www.npmjs.com/package/hyco-ws) nod paketet för Azure Relay Hybridanslutningar bygger på och utökar den ['ws'](https://www.npmjs.com/package/ws) NPM-paketet.</span><span class="sxs-lookup"><span data-stu-id="5e308-105">The [`hyco-ws`](https://www.npmjs.com/package/hyco-ws) Node package for Azure Relay Hybrid Connections is built on and extends the ['ws'](https://www.npmjs.com/package/ws) NPM package.</span></span> <span data-ttu-id="5e308-106">Det här paketet igen exporterar alla export av det grundläggande paketet och lägger till nya export som möjliggör integrering med funktionen Azure vidarebefordrande tjänsten Hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="5e308-106">This package re-exports all exports of that base package and adds new exports that enable integration with the Azure Relay service Hybrid Connections feature.</span></span> 

<span data-ttu-id="5e308-107">Befintliga program som `require('ws')` kan använda det här paketet med `require('hyco-ws')` i stället som kan också hybridscenarier där ett program kan lyssna efter WebSocket-anslutningar från ”innanför brandväggen” lokalt och via Hybridanslutningar, på samma gång.</span><span class="sxs-lookup"><span data-stu-id="5e308-107">Existing applications that `require('ws')` can use this package with `require('hyco-ws')` instead, which also enables hybrid scenarios in which an application can listen for WebSocket connections locally from "inside the firewall" and via Hybrid Connections, all at the same time.</span></span>
  
## <a name="documentation"></a><span data-ttu-id="5e308-108">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="5e308-108">Documentation</span></span>

<span data-ttu-id="5e308-109">API: erna är [dokumenterade i paketets huvudsakliga ws](https://github.com/websockets/ws/blob/master/doc/ws.md).</span><span class="sxs-lookup"><span data-stu-id="5e308-109">The APIs are [documented in the main 'ws' package](https://github.com/websockets/ws/blob/master/doc/ws.md).</span></span> <span data-ttu-id="5e308-110">Den här artikeln beskriver hur det här paketet skiljer sig från den baslinjen.</span><span class="sxs-lookup"><span data-stu-id="5e308-110">This article describes how this package differs from that baseline.</span></span> 

<span data-ttu-id="5e308-111">De viktigaste skillnaderna mellan grundläggande paketet och den här 'hyco-ws' är det lägger till en ny serverklass som exporteras via `require('hyco-ws').RelayedServer`, och några hjälpmetoder.</span><span class="sxs-lookup"><span data-stu-id="5e308-111">The key differences between the base package and this 'hyco-ws' is that it adds a new server class, exported via `require('hyco-ws').RelayedServer`, and a few helper methods.</span></span>

### <a name="package-helper-methods"></a><span data-ttu-id="5e308-112">Paketet hjälpmetoder</span><span class="sxs-lookup"><span data-stu-id="5e308-112">Package helper methods</span></span>

<span data-ttu-id="5e308-113">Det finns flera metoder för export paketet som du kan referera till enligt följande:</span><span class="sxs-lookup"><span data-stu-id="5e308-113">There are several utility methods available on the package export that you can reference as follows:</span></span>

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

<span data-ttu-id="5e308-114">Helper-metoder för användning med det här paketet, men kan också användas av en nod-server för att aktivera webb- eller klienter att skapa lyssnare eller avsändare.</span><span class="sxs-lookup"><span data-stu-id="5e308-114">The helper methods are for use with this package, but can also be used by a Node server for enabling web or device clients to create listeners or senders.</span></span> <span data-ttu-id="5e308-115">Servern använder dessa metoder genom att skicka dem till URI: er som bädda in tillfällig token.</span><span class="sxs-lookup"><span data-stu-id="5e308-115">The server uses these methods by passing them URIs that embed short-lived tokens.</span></span> <span data-ttu-id="5e308-116">Dessa URI: er kan också användas med vanliga WebSocket-stackar som inte stöder inställningen HTTP-huvuden för WebSocket-handskakning.</span><span class="sxs-lookup"><span data-stu-id="5e308-116">These URIs can also be used with common WebSocket stacks that do not support setting HTTP headers for the WebSocket handshake.</span></span> <span data-ttu-id="5e308-117">Bädda in auktorisering token i URI: N stöds i första hand för dessa bibliotek externa Användningsscenarier.</span><span class="sxs-lookup"><span data-stu-id="5e308-117">Embedding authorization tokens into the URI is supported primarily for those library-external usage scenarios.</span></span> 

#### <a name="createrelaylistenuri"></a><span data-ttu-id="5e308-118">createRelayListenUri</span><span class="sxs-lookup"><span data-stu-id="5e308-118">createRelayListenUri</span></span>

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

<span data-ttu-id="5e308-119">Skapar en giltig Azure Relay Hybridanslutning-lyssnare URI för det angivna namnområdet och sökvägen.</span><span class="sxs-lookup"><span data-stu-id="5e308-119">Creates a valid Azure Relay Hybrid Connection listener URI for the given namespace and path.</span></span> <span data-ttu-id="5e308-120">Den här URI kan sedan användas med klassen WebSocketServer relay-versionen.</span><span class="sxs-lookup"><span data-stu-id="5e308-120">This URI can then be used with the relay version of the WebSocketServer class.</span></span>

- <span data-ttu-id="5e308-121">`namespaceName`(obligatoriskt) - domän kvalificerade namnet på Azure Relay-namnområde som ska användas.</span><span class="sxs-lookup"><span data-stu-id="5e308-121">`namespaceName` (required) - the domain-qualified name of the Azure Relay namespace to use.</span></span>
- <span data-ttu-id="5e308-122">`path`(obligatoriskt) - namnet på en befintlig Azure Relay Hybrid-anslutning i detta namnområde.</span><span class="sxs-lookup"><span data-stu-id="5e308-122">`path` (required) - the name of an existing Azure Relay Hybrid Connection in that namespace.</span></span>
- <span data-ttu-id="5e308-123">`token`(valfritt) – en Relay-åtkomst som tidigare utfärdade token som är inbäddad i lyssnaren URI (se följande exempel).</span><span class="sxs-lookup"><span data-stu-id="5e308-123">`token` (optional) - a previously issued Relay access token that is embedded in the listener URI (see the following example).</span></span>
- <span data-ttu-id="5e308-124">`id`(valfritt) – ett spårnings-ID som möjliggör slutpunkt till slutpunkt diagnostik uppföljning av begäranden.</span><span class="sxs-lookup"><span data-stu-id="5e308-124">`id` (optional) - a tracking identifier that enables end-to-end diagnostics tracking of requests.</span></span>

<span data-ttu-id="5e308-125">Den `token` värdet är valfritt och bör endast användas när det inte går att skicka HTTP-huvuden tillsammans med WebSocket-handskakning som är fallet med W3C WebSocket-stacken.</span><span class="sxs-lookup"><span data-stu-id="5e308-125">The `token` value is optional and should only be used when it is not possible to send HTTP headers along with the WebSocket handshake, as is the case with the W3C WebSocket stack.</span></span>                  


#### <a name="createrelaysenduri"></a><span data-ttu-id="5e308-126">createRelaySendUri</span><span class="sxs-lookup"><span data-stu-id="5e308-126">createRelaySendUri</span></span>
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

<span data-ttu-id="5e308-127">Skapar en giltig Azure Relay Hybridanslutning-skicka URI för det angivna namnområdet och sökvägen.</span><span class="sxs-lookup"><span data-stu-id="5e308-127">Creates a valid Azure Relay Hybrid Connection send URI for the given namespace and path.</span></span> <span data-ttu-id="5e308-128">Den här URI kan användas med WebSocket-klienten.</span><span class="sxs-lookup"><span data-stu-id="5e308-128">This URI can be used with any WebSocket client.</span></span>

- <span data-ttu-id="5e308-129">`namespaceName`(obligatoriskt) - domän kvalificerade namnet på Azure Relay-namnområde som ska användas.</span><span class="sxs-lookup"><span data-stu-id="5e308-129">`namespaceName` (required) - the domain-qualified name of the Azure Relay namespace to use.</span></span>
- <span data-ttu-id="5e308-130">`path`(obligatoriskt) - namnet på en befintlig Azure Relay Hybrid-anslutning i detta namnområde.</span><span class="sxs-lookup"><span data-stu-id="5e308-130">`path` (required) - the name of an existing Azure Relay Hybrid Connection in that namespace.</span></span>
- <span data-ttu-id="5e308-131">`token`(valfritt) – en Relay-åtkomst som tidigare utfärdade token som är inbäddad i Skicka URI (se följande exempel).</span><span class="sxs-lookup"><span data-stu-id="5e308-131">`token` (optional) - a previously issued Relay access token that is embedded in the send URI (see the following example).</span></span>
- <span data-ttu-id="5e308-132">`id`(valfritt) – ett spårnings-ID som möjliggör slutpunkt till slutpunkt diagnostik uppföljning av begäranden.</span><span class="sxs-lookup"><span data-stu-id="5e308-132">`id` (optional) - a tracking identifier that enables end-to-end diagnostics tracking of requests.</span></span>

<span data-ttu-id="5e308-133">Den `token` värdet är valfritt och bör endast användas när det inte går att skicka HTTP-huvuden tillsammans med WebSocket-handskakning som är fallet med W3C WebSocket-stacken.</span><span class="sxs-lookup"><span data-stu-id="5e308-133">The `token` value is optional and should only be used when it is not possible to send HTTP headers along with the WebSocket handshake, as is the case with the W3C WebSocket stack.</span></span>                   


#### <a name="createrelaytoken"></a><span data-ttu-id="5e308-134">createRelayToken</span><span class="sxs-lookup"><span data-stu-id="5e308-134">createRelayToken</span></span> 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

<span data-ttu-id="5e308-135">Skapar ett Azure Relay delade signatur åtkomst (SAS)-token för angivna mål-URI, SAS-regeln och SAS regeln nyckel som är giltig för det angivna antalet sekunder eller en timme från aktuell om detta utelämnas används argumentet upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="5e308-135">Creates an Azure Relay Shared Access Signature (SAS) token for the given target URI, SAS rule, and SAS rule key that is valid for the given number of seconds or for an hour from the current instant if the expiry argument is omitted.</span></span>

- <span data-ttu-id="5e308-136">`uri`(obligatoriskt) - URI: N som är token utfärdas.</span><span class="sxs-lookup"><span data-stu-id="5e308-136">`uri` (required) - the URI for which the token is to be issued.</span></span> <span data-ttu-id="5e308-137">URI: N är normaliserat om du vill använda HTTP-schemat och läsa sträng information tas bort.</span><span class="sxs-lookup"><span data-stu-id="5e308-137">The URI is normalized to use the HTTP scheme, and query string information is stripped.</span></span>
- <span data-ttu-id="5e308-138">`ruleName`(krävs) - Regelnamn SAS för antingen det entitet som representeras av den angivna URI: N eller för det namnområde som representeras av värddelen URI.</span><span class="sxs-lookup"><span data-stu-id="5e308-138">`ruleName` (required) - SAS rule name for either the entity represented by the given URI, or for the namespace represented by the URI host portion.</span></span>
- <span data-ttu-id="5e308-139">`key`(krävs) - giltig nyckel för SAS-regeln.</span><span class="sxs-lookup"><span data-stu-id="5e308-139">`key` (required) - valid key for the SAS rule.</span></span> 
- <span data-ttu-id="5e308-140">`expirationSeconds`(valfritt) – antal sekunder innan den genererade token ska upphöra att gälla.</span><span class="sxs-lookup"><span data-stu-id="5e308-140">`expirationSeconds` (optional) - the number of seconds until the generated token should expire.</span></span> <span data-ttu-id="5e308-141">Om den inte anges är standardvärdet 1 timme (3600).</span><span class="sxs-lookup"><span data-stu-id="5e308-141">If not specified, the default is 1 hour (3600).</span></span>

<span data-ttu-id="5e308-142">Utfärdad token ger de rättigheter som associeras med den angivna regeln för SAS för den angivna varaktigheten.</span><span class="sxs-lookup"><span data-stu-id="5e308-142">The issued token confers the rights associated with the specified SAS rule for the given duration.</span></span>

#### <a name="appendrelaytoken"></a><span data-ttu-id="5e308-143">appendRelayToken</span><span class="sxs-lookup"><span data-stu-id="5e308-143">appendRelayToken</span></span>

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

<span data-ttu-id="5e308-144">Den här metoden fungerar som den `createRelayToken` returnerar metoden som beskrivs tidigare, men den token som ska läggas till för indata-URI.</span><span class="sxs-lookup"><span data-stu-id="5e308-144">This method is functionally equivalent to the `createRelayToken` method documented previously, but returns the token correctly appended to the input URI.</span></span>

### <a name="class-wsrelayedserver"></a><span data-ttu-id="5e308-145">Klassen ws. RelayedServer</span><span class="sxs-lookup"><span data-stu-id="5e308-145">Class ws.RelayedServer</span></span>

<span data-ttu-id="5e308-146">Den `hycows.RelayedServer` klass är ett alternativ till att den `ws.Server` klass som inte lyssnar på nätverket, men delegater som lyssnar på den Azure vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5e308-146">The `hycows.RelayedServer` class is an alternative to the `ws.Server` class that does not listen on the local network, but delegates listening to the Azure Relay service.</span></span>

<span data-ttu-id="5e308-147">Två klasser är främst kontraktet som är kompatibla, vilket innebär att en befintlig programmet med den `ws.Server` klassen kan enkelt ändras för att använda den vidarebefordrande versionen.</span><span class="sxs-lookup"><span data-stu-id="5e308-147">The two classes are mostly contract compatible, meaning that an existing application using the `ws.Server` class can easily be changed to use the relayed version.</span></span> <span data-ttu-id="5e308-148">De viktigaste skillnaderna är i konstruktorn och de tillgängliga alternativen.</span><span class="sxs-lookup"><span data-stu-id="5e308-148">The main differences are in the constructor and in the available options.</span></span>

#### <a name="constructor"></a><span data-ttu-id="5e308-149">Konstruktorn</span><span class="sxs-lookup"><span data-stu-id="5e308-149">Constructor</span></span>  

```JavaScript 
var ws = require('hyco-ws');
var server = ws.RelayedServer;

var wss = new server(
    {
        server: ws.createRelayListenUri(ns, path),
        token: function() { return ws.createRelayToken('http://' + ns, keyrule, key); }
    });
```

<span data-ttu-id="5e308-150">Den `RelayedServer` konstruktorn stöder en annan uppsättning argument än den `Server`, eftersom den inte är en fristående lyssnare eller kan vara inbäddat i ett befintligt http-lyssnaren ramverk.</span><span class="sxs-lookup"><span data-stu-id="5e308-150">The `RelayedServer` constructor supports a different set of arguments than the `Server`, because it is not a standalone listener, or able to be embedded into an existing HTTP listener framework.</span></span> <span data-ttu-id="5e308-151">Det finns också färre alternativ eftersom WebSocket-management är i stort sett delegerat till den vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5e308-151">There are also fewer options available since the WebSocket management is largely delegated to the Relay service.</span></span>

<span data-ttu-id="5e308-152">Konstruktorargument:</span><span class="sxs-lookup"><span data-stu-id="5e308-152">Constructor arguments:</span></span>

- <span data-ttu-id="5e308-153">`server`(krävs) - konstruerade fullständigt kvalificerade URI för en Hybridanslutning namn som ska övervakas, vanligtvis med hjälpmetoden WebSocket.createRelayListenUri().</span><span class="sxs-lookup"><span data-stu-id="5e308-153">`server` (required) - the fully qualified URI for a Hybrid Connection name on which to listen, usually constructed with the WebSocket.createRelayListenUri() helper method.</span></span>
- <span data-ttu-id="5e308-154">`token`(krävs) - innehåller det här argumentet en tidigare utfärdade token sträng eller en motringning funktion som kan användas för att hämta en token sträng.</span><span class="sxs-lookup"><span data-stu-id="5e308-154">`token` (required) - this argument holds either a previously issued token string or a callback function that can be called to obtain such a token string.</span></span> <span data-ttu-id="5e308-155">Alternativet är att föredra, eftersom det token förnyelse.</span><span class="sxs-lookup"><span data-stu-id="5e308-155">The callback option is preferred, as it enables token renewal.</span></span>

#### <a name="events"></a><span data-ttu-id="5e308-156">Händelser</span><span class="sxs-lookup"><span data-stu-id="5e308-156">Events</span></span>

<span data-ttu-id="5e308-157">`RelayedServer`instanser genererar tre händelser som hjälper dig att hantera inkommande begäranden, upprätta anslutningar och identifiera felvillkor.</span><span class="sxs-lookup"><span data-stu-id="5e308-157">`RelayedServer` instances emit three events that enable you to handle incoming requests, establish connections, and detect error conditions.</span></span> <span data-ttu-id="5e308-158">Måste du prenumerera på `connect` händelsen för att hantera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5e308-158">You must subscribe to the `connect` event to handle messages.</span></span> 

##### <a name="headers"></a><span data-ttu-id="5e308-159">Rubriker</span><span class="sxs-lookup"><span data-stu-id="5e308-159">headers</span></span>

```JavaScript 
function(headers)
```

<span data-ttu-id="5e308-160">Den `headers` händelse utlöses innan en inkommande anslutning är godkända att aktivera ändringen av huvuden som skickas till klienten.</span><span class="sxs-lookup"><span data-stu-id="5e308-160">The `headers` event is raised just before an incoming connection is accepted, enabling modification of the headers to send to the client.</span></span> 

##### <a name="connection"></a><span data-ttu-id="5e308-161">anslutning</span><span class="sxs-lookup"><span data-stu-id="5e308-161">connection</span></span>

```JavaScript
function(socket)
```

<span data-ttu-id="5e308-162">Orsakat när en ny WebSocket-anslutning accepteras.</span><span class="sxs-lookup"><span data-stu-id="5e308-162">Emitted when a new WebSocket connection is accepted.</span></span> <span data-ttu-id="5e308-163">Objektet är av typen `ws.WebSocket`, samma som med grundläggande paketet.</span><span class="sxs-lookup"><span data-stu-id="5e308-163">The object is of type `ws.WebSocket`, same as with the base package.</span></span>


##### <a name="error"></a><span data-ttu-id="5e308-164">fel</span><span class="sxs-lookup"><span data-stu-id="5e308-164">error</span></span>

```JavaScript
function(error)
```

<span data-ttu-id="5e308-165">Om den underliggande servern genererar ett fel, skickas den här.</span><span class="sxs-lookup"><span data-stu-id="5e308-165">If the underlying server emits an error, it is forwarded here.</span></span>  

#### <a name="helpers"></a><span data-ttu-id="5e308-166">Hjälpprogram</span><span class="sxs-lookup"><span data-stu-id="5e308-166">Helpers</span></span>

<span data-ttu-id="5e308-167">För att förenkla vidarebefordrande server startas och omedelbart prenumerera på inkommande anslutningar, visar paketet en enkel hjälpfunktion som också används i exemplen, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="5e308-167">To simplify starting a relayed server and immediately subscribing to incoming connections, the package exposes a simple helper function, which is also used in the examples, as follows:</span></span>

##### <a name="createrelayedlistener"></a><span data-ttu-id="5e308-168">createRelayedListener</span><span class="sxs-lookup"><span data-stu-id="5e308-168">createRelayedListener</span></span>

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

##### <a name="createrelayedserver"></a><span data-ttu-id="5e308-169">createRelayedServer</span><span class="sxs-lookup"><span data-stu-id="5e308-169">createRelayedServer</span></span>

```javascript
var server = createRelayedServer([options], [connectCallback] )
```

<span data-ttu-id="5e308-170">Den här metoden anropar konstruktorn för att skapa en ny instans av RelayedServer och prenumererar på angivna återanropet på händelsen 'anslutningen'.</span><span class="sxs-lookup"><span data-stu-id="5e308-170">This method calls the constructor to create a new instance of the RelayedServer and then subscribes the provided callback to the 'connection' event.</span></span>
 
##### <a name="relayedconnect"></a><span data-ttu-id="5e308-171">relayedConnect</span><span class="sxs-lookup"><span data-stu-id="5e308-171">relayedConnect</span></span>

<span data-ttu-id="5e308-172">Spegling bara den `createRelayedServer` helper i funktionen `relayedConnect` skapar en klientanslutning och prenumererar på händelsen 'öppen' på den resulterande socketen.</span><span class="sxs-lookup"><span data-stu-id="5e308-172">Simply mirroring the `createRelayedServer` helper in function, `relayedConnect` creates a client connection and subscribes to the 'open' event on the resulting socket.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="5e308-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5e308-173">Next steps</span></span>
<span data-ttu-id="5e308-174">Mer information om Azure Relay finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="5e308-174">To learn more about Azure Relay, visit these links:</span></span>
* [<span data-ttu-id="5e308-175">Vad är Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="5e308-175">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="5e308-176">Tillgängliga Relay-API: er</span><span class="sxs-lookup"><span data-stu-id="5e308-176">Available Relay APIs</span></span>](relay-api-overview.md)
