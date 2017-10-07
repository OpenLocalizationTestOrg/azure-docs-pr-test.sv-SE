---
title: 'aaaOverview av hello Azure Relay .NET Standard API: erna | Microsoft Docs'
description: "Översikt över Standard .NET-API-relä"
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b1da9ac1-811b-4df7-a22c-ccd013405c40
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: sethm
ms.openlocfilehash: c90e00e809bd44eb0fbbff5eb03dfc8afa486523
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-relay-hybrid-connections-net-standard-api-overview"></a>Översikt över Azure Relay Hybrid anslutningar .NET Standard-API

Den här artikeln sammanfattas några av hello nyckeln Azure Relay Hybrid anslutningar .NET Standard [klientens API: er](/dotnet/api/microsoft.azure.relay).
  
## <a name="relay-connection-string-builder"></a>Relay-anslutning sträng Builder

Hej [RelayConnectionStringBuilder] [ RelayConnectionStringBuilder] klassen formaterar anslutningssträngar som är specifika tooRelay Hybridanslutningar. Du kan använda den tooverify hello format i anslutningssträngen eller toobuild en anslutningssträng från grunden. Se hello följande kod för ett exempel:

```csharp
var endpoint = "{Relay namespace}";
var entityPath = "{Name of hello Hybrid Connection}";
var sharedAccessKeyName = "{SAS key name}";
var sharedAccessKey = "{SAS key value}";

var connectionStringBuilder = new RelayConnectionStringBuilder()
{
    Endpoint = endpoint,
    EntityPath = entityPath,
    SharedAccessKeyName = sasKeyName,
    SharedAccessKey = sasKeyValue
};
```

Du kan också skicka en anslutning direkt string toohello `RelayConnectionStringBuilder` metod. Den här åtgärden aktiverar tooverify hello anslutningssträngen är ett ogiltigt format. Om någon av hello-parametrarna är ogiltiga hello konstruktorn genererar en `ArgumentException`.

```csharp
var myConnectionString = "{RelayConnectionString}";
// Declare hello connectionStringBuilder so that it can be used outside of hello loop if needed
RelayConnectionStringBuilder connectionStringBuilder;
try
{
    // Create hello connectionStringBuilder using hello supplied connection string
    connectionStringBuilder = new RelayConnectionStringBuilder(myConnectionString);
}
catch (ArgumentException ae)
{
    // Perform some error handling
}
```

## <a name="hybrid-connection-stream"></a>Dataströmmen för hybrid-anslutning
Hej [HybridConnectionStream] [ HCStream] klass är hello primära objektet som används för toosend och ta emot data från Azure Relay slutpunkt, oavsett om du arbetar med en [HybridConnectionClient] [ HCClient], eller en [HybridConnectionListener][HCListener].

### <a name="getting-a-hybrid-connection-stream"></a>Hämtar en dataström för Hybrid-anslutning

#### <a name="listener"></a>Lyssnare
Med hjälp av en [HybridConnectionListener][HCListener], kan du hämta en `HybridConnectionStream` objekt på följande sätt:

```csharp
// Use hello RelayConnectionStringBuilder tooget a valid connection string
var listener = new HybridConnectionListener(csb.ToString());
// Open a connection toohello Relay endpoint
await listener.OpenAsync();
// Get a `HybridConnectionStream`
var hybridConnectionStream = await listener.AcceptConnectionAsync();
```

#### <a name="client"></a>Client
Med hjälp av en [HybridConnectionClient][HCClient], kan du hämta en `HybridConnectionStream` objekt på följande sätt:

```csharp
// Use hello RelayConnectionStringBuilder tooget a valid connection string
var client = new HybridConnectionClient(csb.ToString());
// Open a connection toohello Relay endpoint and get a `HybridConnectionStream`
var hybridConnectionStream = await client.CreateConnectionAsync();
```

### <a name="receiving-data"></a>Ta emot data
Hej [HybridConnectionStream] [ HCStream] klassen kan dubbelriktad kommunikation. I de flesta fall felmeddelandet kontinuerligt från hello dataström. Om du läser text från hello stream, kan du också toouse en [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) -objektet, vilket möjliggör enklare tolkning av hello data. Du kan till exempel läsa data som text i stället för som `byte[]`.

hello läser följande kod enskilda rader med text från hello dataström tills en annullering begärs:

```csharp
// Create a CancellationToken, so that we can cancel hello while loop
var cancellationToken = new CancellationToken();
// Create a StreamReader from hello 'hybridConnectionStream`
var streamReader = new StreamReader(hybridConnectionStream);

while (!cancellationToken.IsCancellationRequested)
{
    // Read a line of input until a newline is encountered
    var line = await streamReader.ReadLineAsync();
    if (string.IsNullOrEmpty(line))
    {
        // If there's no input data, we will signal that 
        // we will no longer send data on this connection
        // and then break out of hello processing loop.
        await hybridConnectionStream.ShutdownAsync(cancellationToken);
        break;
    }
}
```

### <a name="sending-data"></a>Skicka data
När du har skapat en koppling, kan du skicka ett meddelande toohello Relay-slutpunkten. Eftersom hello anslutningsobjekt ärver [dataströmmen](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), skicka dina data som en `byte[]`. följande exempel visar hur hello toodo detta:

```csharp
var data = Encoding.UTF8.GetBytes("hello");
await clientConnection.WriteAsync(data, 0, data.Length);
```

Om du vill toosend texten direkt, utan att behöva tooencode hello sträng varje gång du dock omsluter hello `hybridConnectionStream` objekt med en [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) objekt.

```csharp
// hello StreamWriter object only needs toobe created once
var textWriter = new StreamWriter(hybridConnectionStream);
await textWriter.WriteLineAsync("hello");
```

## <a name="next-steps"></a>Nästa steg
toolearn mer om Azure Relay finns i följande länkar:

* [Microsoft.Azure.Relay referens](/dotnet/api/microsoft.azure.relay)
* [Vad är Azure Relay?](relay-what-is-it.md)
* [Tillgängliga Relay-API: er](relay-api-overview.md)

[RelayConnectionStringBuilder]: /dotnet/api/microsoft.azure.relay.relayconnectionstringbuilder
[HCStream]: /dotnet/api/microsoft.azure.relay.hybridconnectionstream
[HCClient]: /dotnet/api/microsoft.azure.relay.hybridconnectionclient
[HCListener]: /dotnet/api/microsoft.azure.relay.hybridconnectionlistener