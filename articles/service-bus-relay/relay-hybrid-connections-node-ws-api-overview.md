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
# <a name="relay-hybrid-connections-node-api-overview"></a><span data-ttu-id="35426-103">Översikt över Hybrid anslutningar nod API-relä</span><span class="sxs-lookup"><span data-stu-id="35426-103">Relay Hybrid Connections Node API overview</span></span>

## <a name="overview"></a><span data-ttu-id="35426-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="35426-104">Overview</span></span>

<span data-ttu-id="35426-105">Hej [ `hyco-ws` ](https://www.npmjs.com/package/hyco-ws) nod paketet för Azure Relay Hybridanslutningar bygger på och utökar hello ['ws'](https://www.npmjs.com/package/ws) NPM-paketet.</span><span class="sxs-lookup"><span data-stu-id="35426-105">hello [`hyco-ws`](https://www.npmjs.com/package/hyco-ws) Node package for Azure Relay Hybrid Connections is built on and extends hello ['ws'](https://www.npmjs.com/package/ws) NPM package.</span></span> <span data-ttu-id="35426-106">Det här paketet igen exporterar alla export av det grundläggande paketet och lägger till nya export som möjliggör integrering med hello Azure vidarebefordrande tjänsten Hybridanslutningar funktion.</span><span class="sxs-lookup"><span data-stu-id="35426-106">This package re-exports all exports of that base package and adds new exports that enable integration with hello Azure Relay service Hybrid Connections feature.</span></span> 

<span data-ttu-id="35426-107">Befintliga program som `require('ws')` kan använda det här paketet med `require('hyco-ws')` i stället som kan också hybridscenarier där ett program kan lyssna efter WebSocket-anslutningar från lokalt ”innanför hello brandvägg” och via Hybridanslutningar, allt på Hej samma tid.</span><span class="sxs-lookup"><span data-stu-id="35426-107">Existing applications that `require('ws')` can use this package with `require('hyco-ws')` instead, which also enables hybrid scenarios in which an application can listen for WebSocket connections locally from "inside hello firewall" and via Hybrid Connections, all at hello same time.</span></span>
  
## <a name="documentation"></a><span data-ttu-id="35426-108">Dokumentation</span><span class="sxs-lookup"><span data-stu-id="35426-108">Documentation</span></span>

<span data-ttu-id="35426-109">hello API: er är [dokumenterade i hello huvudsakliga ws paketet](https://github.com/websockets/ws/blob/master/doc/ws.md).</span><span class="sxs-lookup"><span data-stu-id="35426-109">hello APIs are [documented in hello main 'ws' package](https://github.com/websockets/ws/blob/master/doc/ws.md).</span></span> <span data-ttu-id="35426-110">Den här artikeln beskriver hur det här paketet skiljer sig från den baslinjen.</span><span class="sxs-lookup"><span data-stu-id="35426-110">This article describes how this package differs from that baseline.</span></span> 

<span data-ttu-id="35426-111">hello viktiga skillnader mellan grundläggande hello-paket och den här 'hyco-ws' är det lägger till en ny serverklass som exporteras via `require('hyco-ws').RelayedServer`, och några hjälpmetoder.</span><span class="sxs-lookup"><span data-stu-id="35426-111">hello key differences between hello base package and this 'hyco-ws' is that it adds a new server class, exported via `require('hyco-ws').RelayedServer`, and a few helper methods.</span></span>

### <a name="package-helper-methods"></a><span data-ttu-id="35426-112">Paketet hjälpmetoder</span><span class="sxs-lookup"><span data-stu-id="35426-112">Package helper methods</span></span>

<span data-ttu-id="35426-113">Det finns flera metoder för hello paketet som du kan referera till enligt följande:</span><span class="sxs-lookup"><span data-stu-id="35426-113">There are several utility methods available on hello package export that you can reference as follows:</span></span>

```JavaScript
const WebSocket = require('hyco-ws');

var listenUri = WebSocket.createRelayListenUri('namespace.servicebus.windows.net', 'path');
listenUri = WebSocket.appendRelayToken(listenUri, 'ruleName', '...key...')
...

```

<span data-ttu-id="35426-114">Hej hjälpmetoder inte kan användas med det här paketet, men kan även användas av en nod-server för att aktivera webb- eller klienter toocreate lyssnare eller avsändare.</span><span class="sxs-lookup"><span data-stu-id="35426-114">hello helper methods are for use with this package, but can also be used by a Node server for enabling web or device clients toocreate listeners or senders.</span></span> <span data-ttu-id="35426-115">hello-servern använder dessa metoder genom att skicka dem till URI: er som bädda in tillfällig token.</span><span class="sxs-lookup"><span data-stu-id="35426-115">hello server uses these methods by passing them URIs that embed short-lived tokens.</span></span> <span data-ttu-id="35426-116">Dessa URI: er kan också användas med vanliga WebSocket-stackar som inte stöder inställningen HTTP-huvuden för hello WebSocket-handskakning.</span><span class="sxs-lookup"><span data-stu-id="35426-116">These URIs can also be used with common WebSocket stacks that do not support setting HTTP headers for hello WebSocket handshake.</span></span> <span data-ttu-id="35426-117">Bädda in auktorisering token i hello URI stöds för dessa bibliotek externa Användningsscenarier.</span><span class="sxs-lookup"><span data-stu-id="35426-117">Embedding authorization tokens into hello URI is supported primarily for those library-external usage scenarios.</span></span> 

#### <a name="createrelaylistenuri"></a><span data-ttu-id="35426-118">createRelayListenUri</span><span class="sxs-lookup"><span data-stu-id="35426-118">createRelayListenUri</span></span>

```JavaScript
var uri = createRelayListenUri([namespaceName], [path], [[token]], [[id]])
```

<span data-ttu-id="35426-119">Skapar en giltig Azure Relay Hybridanslutning-lyssnare URI för hello namnområdet och sökvägen.</span><span class="sxs-lookup"><span data-stu-id="35426-119">Creates a valid Azure Relay Hybrid Connection listener URI for hello given namespace and path.</span></span> <span data-ttu-id="35426-120">Den här URI kan sedan användas med hello relay-versionen av hello WebSocketServer klass.</span><span class="sxs-lookup"><span data-stu-id="35426-120">This URI can then be used with hello relay version of hello WebSocketServer class.</span></span>

- <span data-ttu-id="35426-121">`namespaceName`(krävs) - hello domän kvalificerade namnet på hello Azure Relay namnområdet toouse.</span><span class="sxs-lookup"><span data-stu-id="35426-121">`namespaceName` (required) - hello domain-qualified name of hello Azure Relay namespace toouse.</span></span>
- <span data-ttu-id="35426-122">`path`namnet på en befintlig Azure Relay Hybrid-anslutning i detta namnområde (krävs) - hello.</span><span class="sxs-lookup"><span data-stu-id="35426-122">`path` (required) - hello name of an existing Azure Relay Hybrid Connection in that namespace.</span></span>
- <span data-ttu-id="35426-123">`token`(valfritt) – en Relay-åtkomst som tidigare utfärdade token som är inbäddad i hello-lyssnaren URI (se följande exempel hello).</span><span class="sxs-lookup"><span data-stu-id="35426-123">`token` (optional) - a previously issued Relay access token that is embedded in hello listener URI (see hello following example).</span></span>
- <span data-ttu-id="35426-124">`id`(valfritt) – ett spårnings-ID som möjliggör slutpunkt till slutpunkt diagnostik uppföljning av begäranden.</span><span class="sxs-lookup"><span data-stu-id="35426-124">`id` (optional) - a tracking identifier that enables end-to-end diagnostics tracking of requests.</span></span>

<span data-ttu-id="35426-125">Hej `token` värdet är valfritt och bör endast användas när det inte är möjligt toosend HTTP-huvuden tillsammans med hello WebSocket-handskakningen eftersom hello fallet med hello W3C WebSocket-stacken.</span><span class="sxs-lookup"><span data-stu-id="35426-125">hello `token` value is optional and should only be used when it is not possible toosend HTTP headers along with hello WebSocket handshake, as is hello case with hello W3C WebSocket stack.</span></span>                  


#### <a name="createrelaysenduri"></a><span data-ttu-id="35426-126">createRelaySendUri</span><span class="sxs-lookup"><span data-stu-id="35426-126">createRelaySendUri</span></span>
 
```JavaScript
var uri = createRelaySendUri([namespaceName], [path], [[token]], [[id]])
```

<span data-ttu-id="35426-127">Skapar en giltig Azure Relay Hybridanslutning-skicka URI för hello namnområdet och sökvägen.</span><span class="sxs-lookup"><span data-stu-id="35426-127">Creates a valid Azure Relay Hybrid Connection send URI for hello given namespace and path.</span></span> <span data-ttu-id="35426-128">Den här URI kan användas med WebSocket-klienten.</span><span class="sxs-lookup"><span data-stu-id="35426-128">This URI can be used with any WebSocket client.</span></span>

- <span data-ttu-id="35426-129">`namespaceName`(krävs) - hello domän kvalificerade namnet på hello Azure Relay namnområdet toouse.</span><span class="sxs-lookup"><span data-stu-id="35426-129">`namespaceName` (required) - hello domain-qualified name of hello Azure Relay namespace toouse.</span></span>
- <span data-ttu-id="35426-130">`path`namnet på en befintlig Azure Relay Hybrid-anslutning i detta namnområde (krävs) - hello.</span><span class="sxs-lookup"><span data-stu-id="35426-130">`path` (required) - hello name of an existing Azure Relay Hybrid Connection in that namespace.</span></span>
- <span data-ttu-id="35426-131">`token`(valfritt) – skicka ett tidigare utfärdade Relay åtkomst-token som är inbäddad i hello URI (se följande exempel hello).</span><span class="sxs-lookup"><span data-stu-id="35426-131">`token` (optional) - a previously issued Relay access token that is embedded in hello send URI (see hello following example).</span></span>
- <span data-ttu-id="35426-132">`id`(valfritt) – ett spårnings-ID som möjliggör slutpunkt till slutpunkt diagnostik uppföljning av begäranden.</span><span class="sxs-lookup"><span data-stu-id="35426-132">`id` (optional) - a tracking identifier that enables end-to-end diagnostics tracking of requests.</span></span>

<span data-ttu-id="35426-133">Hej `token` värdet är valfritt och bör endast användas när det inte är möjligt toosend HTTP-huvuden tillsammans med hello WebSocket-handskakningen eftersom hello fallet med hello W3C WebSocket-stacken.</span><span class="sxs-lookup"><span data-stu-id="35426-133">hello `token` value is optional and should only be used when it is not possible toosend HTTP headers along with hello WebSocket handshake, as is hello case with hello W3C WebSocket stack.</span></span>                   


#### <a name="createrelaytoken"></a><span data-ttu-id="35426-134">createRelayToken</span><span class="sxs-lookup"><span data-stu-id="35426-134">createRelayToken</span></span> 

```JavaScript
var token = createRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

<span data-ttu-id="35426-135">Skapar en Azure Relay delade signatur åtkomst (SAS)-token för hello givet mål-URI, SAS-regeln och SAS regeln nyckel som är giltig för hello får antal sekunder eller en timme från hello aktuella snabbmeddelanden om hello utgången argument utelämnas.</span><span class="sxs-lookup"><span data-stu-id="35426-135">Creates an Azure Relay Shared Access Signature (SAS) token for hello given target URI, SAS rule, and SAS rule key that is valid for hello given number of seconds or for an hour from hello current instant if hello expiry argument is omitted.</span></span>

- <span data-ttu-id="35426-136">`uri`(krävs) - hello URI för vilka hello token är toobe utfärdas.</span><span class="sxs-lookup"><span data-stu-id="35426-136">`uri` (required) - hello URI for which hello token is toobe issued.</span></span> <span data-ttu-id="35426-137">hello URI är normaliserat toouse hello HTTP-schemat och läsa sträng information tas bort.</span><span class="sxs-lookup"><span data-stu-id="35426-137">hello URI is normalized toouse hello HTTP scheme, and query string information is stripped.</span></span>
- <span data-ttu-id="35426-138">`ruleName`(krävs) - SAS Regelnamn för antingen hello entitet som representeras av hello angivna URI: N, eller för hello namnområde åt hello värddelen URI.</span><span class="sxs-lookup"><span data-stu-id="35426-138">`ruleName` (required) - SAS rule name for either hello entity represented by hello given URI, or for hello namespace represented by hello URI host portion.</span></span>
- <span data-ttu-id="35426-139">`key`(krävs) - giltig nyckel för hello SAS-regel.</span><span class="sxs-lookup"><span data-stu-id="35426-139">`key` (required) - valid key for hello SAS rule.</span></span> 
- <span data-ttu-id="35426-140">`expirationSeconds`(valfritt) – hello antalet sekunder tills hello genererats token ska upphöra att gälla.</span><span class="sxs-lookup"><span data-stu-id="35426-140">`expirationSeconds` (optional) - hello number of seconds until hello generated token should expire.</span></span> <span data-ttu-id="35426-141">Om inget anges är hello standardvärdet 1 timme (3600).</span><span class="sxs-lookup"><span data-stu-id="35426-141">If not specified, hello default is 1 hour (3600).</span></span>

<span data-ttu-id="35426-142">hello utfärdade token ger hello rättigheter som är associerade med hello angetts SAS-regel för hello angivna varaktighet.</span><span class="sxs-lookup"><span data-stu-id="35426-142">hello issued token confers hello rights associated with hello specified SAS rule for hello given duration.</span></span>

#### <a name="appendrelaytoken"></a><span data-ttu-id="35426-143">appendRelayToken</span><span class="sxs-lookup"><span data-stu-id="35426-143">appendRelayToken</span></span>

```JavaScript
var uri = appendRelayToken([uri], [ruleName], [key], [[expirationSeconds]])
```

<span data-ttu-id="35426-144">Den här metoden är funktionellt motsvarande toohello `createRelayToken` metoden beskrivs tidigare, men returnerar hello token korrekt tillagda toohello indata-URI.</span><span class="sxs-lookup"><span data-stu-id="35426-144">This method is functionally equivalent toohello `createRelayToken` method documented previously, but returns hello token correctly appended toohello input URI.</span></span>

### <a name="class-wsrelayedserver"></a><span data-ttu-id="35426-145">Klassen ws. RelayedServer</span><span class="sxs-lookup"><span data-stu-id="35426-145">Class ws.RelayedServer</span></span>

<span data-ttu-id="35426-146">Hej `hycows.RelayedServer` klassen är en alternativ toohello `ws.Server` klass som inte avlyssna hello lokala nätverk, men delegerar lyssnande toohello Azure vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="35426-146">hello `hycows.RelayedServer` class is an alternative toohello `ws.Server` class that does not listen on hello local network, but delegates listening toohello Azure Relay service.</span></span>

<span data-ttu-id="35426-147">hello två klasser är främst kontraktet som är kompatibla, vilket innebär att ett befintligt program med hjälp av hello `ws.Server` klassen enkelt kan ändrade toouse hello vidarebefordrande version.</span><span class="sxs-lookup"><span data-stu-id="35426-147">hello two classes are mostly contract compatible, meaning that an existing application using hello `ws.Server` class can easily be changed toouse hello relayed version.</span></span> <span data-ttu-id="35426-148">hello viktigaste skillnaderna är i hello konstruktorn och hello tillgängliga alternativ.</span><span class="sxs-lookup"><span data-stu-id="35426-148">hello main differences are in hello constructor and in hello available options.</span></span>

#### <a name="constructor"></a><span data-ttu-id="35426-149">Konstruktorn</span><span class="sxs-lookup"><span data-stu-id="35426-149">Constructor</span></span>  

```JavaScript 
var ws = require('hyco-ws');
var server = ws.RelayedServer;

var wss = new server(
    {
        server: ws.createRelayListenUri(ns, path),
        token: function() { return ws.createRelayToken('http://' + ns, keyrule, key); }
    });
```

<span data-ttu-id="35426-150">Hej `RelayedServer` konstruktorn stöder en annan uppsättning argument än hello `Server`eftersom det inte är en fristående lyssnare eller kan toobe bädda in i en befintlig http-lyssnaren framework.</span><span class="sxs-lookup"><span data-stu-id="35426-150">hello `RelayedServer` constructor supports a different set of arguments than hello `Server`, because it is not a standalone listener, or able toobe embedded into an existing HTTP listener framework.</span></span> <span data-ttu-id="35426-151">Det finns också färre alternativ eftersom hello WebSocket-management är i stort sett delegerad toohello vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="35426-151">There are also fewer options available since hello WebSocket management is largely delegated toohello Relay service.</span></span>

<span data-ttu-id="35426-152">Konstruktorargument:</span><span class="sxs-lookup"><span data-stu-id="35426-152">Constructor arguments:</span></span>

- <span data-ttu-id="35426-153">`server`(krävs) - kvalificerade hello fullständigt URI för en Hybridanslutning på vilka toolisten vanligtvis konstruerade med hello WebSocket.createRelayListenUri() hjälpmetod.</span><span class="sxs-lookup"><span data-stu-id="35426-153">`server` (required) - hello fully qualified URI for a Hybrid Connection name on which toolisten, usually constructed with hello WebSocket.createRelayListenUri() helper method.</span></span>
- <span data-ttu-id="35426-154">`token`(krävs) - innehåller det här argumentet en tidigare utfärdade token sträng eller en callback-funktion som kan anropas tooobtain en token sträng.</span><span class="sxs-lookup"><span data-stu-id="35426-154">`token` (required) - this argument holds either a previously issued token string or a callback function that can be called tooobtain such a token string.</span></span> <span data-ttu-id="35426-155">hello återanrop alternativ rekommenderas eftersom det token förnyelse.</span><span class="sxs-lookup"><span data-stu-id="35426-155">hello callback option is preferred, as it enables token renewal.</span></span>

#### <a name="events"></a><span data-ttu-id="35426-156">Händelser</span><span class="sxs-lookup"><span data-stu-id="35426-156">Events</span></span>

<span data-ttu-id="35426-157">`RelayedServer`instanser genererar tre händelser som aktiverar du toohandle inkommande begäranden, upprätta anslutningar och identifiera felvillkor.</span><span class="sxs-lookup"><span data-stu-id="35426-157">`RelayedServer` instances emit three events that enable you toohandle incoming requests, establish connections, and detect error conditions.</span></span> <span data-ttu-id="35426-158">Du måste prenumerera toohello `connect` toohandle händelsemeddelanden.</span><span class="sxs-lookup"><span data-stu-id="35426-158">You must subscribe toohello `connect` event toohandle messages.</span></span> 

##### <a name="headers"></a><span data-ttu-id="35426-159">Rubriker</span><span class="sxs-lookup"><span data-stu-id="35426-159">headers</span></span>

```JavaScript 
function(headers)
```

<span data-ttu-id="35426-160">Hej `headers` händelse utlöses innan en inkommande anslutning är godkända att aktivera ändringen av hello huvuden toosend toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="35426-160">hello `headers` event is raised just before an incoming connection is accepted, enabling modification of hello headers toosend toohello client.</span></span> 

##### <a name="connection"></a><span data-ttu-id="35426-161">anslutning</span><span class="sxs-lookup"><span data-stu-id="35426-161">connection</span></span>

```JavaScript
function(socket)
```

<span data-ttu-id="35426-162">Orsakat när en ny WebSocket-anslutning accepteras.</span><span class="sxs-lookup"><span data-stu-id="35426-162">Emitted when a new WebSocket connection is accepted.</span></span> <span data-ttu-id="35426-163">hello-objektet är av typen `ws.WebSocket`, samma som med grundläggande hello-paket.</span><span class="sxs-lookup"><span data-stu-id="35426-163">hello object is of type `ws.WebSocket`, same as with hello base package.</span></span>


##### <a name="error"></a><span data-ttu-id="35426-164">fel</span><span class="sxs-lookup"><span data-stu-id="35426-164">error</span></span>

```JavaScript
function(error)
```

<span data-ttu-id="35426-165">Om hello underliggande server genererar ett fel, skickas den här.</span><span class="sxs-lookup"><span data-stu-id="35426-165">If hello underlying server emits an error, it is forwarded here.</span></span>  

#### <a name="helpers"></a><span data-ttu-id="35426-166">Hjälpprogram</span><span class="sxs-lookup"><span data-stu-id="35426-166">Helpers</span></span>

<span data-ttu-id="35426-167">toosimplify startar en vidarebefordrande server och omedelbart prenumerera tooincoming anslutningar hello paketet visar en enkel hjälpfunktion som också används i hello exempel på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="35426-167">toosimplify starting a relayed server and immediately subscribing tooincoming connections, hello package exposes a simple helper function, which is also used in hello examples, as follows:</span></span>

##### <a name="createrelayedlistener"></a><span data-ttu-id="35426-168">createRelayedListener</span><span class="sxs-lookup"><span data-stu-id="35426-168">createRelayedListener</span></span>

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

##### <a name="createrelayedserver"></a><span data-ttu-id="35426-169">createRelayedServer</span><span class="sxs-lookup"><span data-stu-id="35426-169">createRelayedServer</span></span>

```javascript
var server = createRelayedServer([options], [connectCallback] )
```

<span data-ttu-id="35426-170">Den här metoden anropar hello konstruktorn toocreate en ny instans av hello RelayedServer och prenumererar hello angivna återanropet toohello anslutning händelse.</span><span class="sxs-lookup"><span data-stu-id="35426-170">This method calls hello constructor toocreate a new instance of hello RelayedServer and then subscribes hello provided callback toohello 'connection' event.</span></span>
 
##### <a name="relayedconnect"></a><span data-ttu-id="35426-171">relayedConnect</span><span class="sxs-lookup"><span data-stu-id="35426-171">relayedConnect</span></span>

<span data-ttu-id="35426-172">Bara spegling hello `createRelayedServer` helper i funktionen `relayedConnect` skapar en klientanslutning och prenumererar toohello 'öppen' händelse på hello resulterande socket.</span><span class="sxs-lookup"><span data-stu-id="35426-172">Simply mirroring hello `createRelayedServer` helper in function, `relayedConnect` creates a client connection and subscribes toohello 'open' event on hello resulting socket.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="35426-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="35426-173">Next steps</span></span>
<span data-ttu-id="35426-174">toolearn mer om Azure Relay finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="35426-174">toolearn more about Azure Relay, visit these links:</span></span>
* [<span data-ttu-id="35426-175">Vad är Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="35426-175">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="35426-176">Tillgängliga Relay-API: er</span><span class="sxs-lookup"><span data-stu-id="35426-176">Available Relay APIs</span></span>](relay-api-overview.md)
