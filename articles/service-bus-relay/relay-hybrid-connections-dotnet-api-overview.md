---
title: "Översikt över Azure Relay .NET Standard API: er | Microsoft Docs"
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
ms.openlocfilehash: f3f4a2e721b1a75a5b92a5c17a9939c7013340d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-relay-hybrid-connections-net-standard-api-overview"></a><span data-ttu-id="c32b8-103">Översikt över Azure Relay Hybrid anslutningar .NET Standard-API</span><span class="sxs-lookup"><span data-stu-id="c32b8-103">Azure Relay Hybrid Connections .NET Standard API overview</span></span>

<span data-ttu-id="c32b8-104">Den här artikeln sammanfattas några av nyckeln Azure Relay Hybrid anslutningar .NET Standard [klientens API: er](/dotnet/api/microsoft.azure.relay).</span><span class="sxs-lookup"><span data-stu-id="c32b8-104">This article summarizes some of the key Azure Relay Hybrid Connections .NET Standard [client APIs](/dotnet/api/microsoft.azure.relay).</span></span>
  
## <a name="relay-connection-string-builder"></a><span data-ttu-id="c32b8-105">Relay-anslutning sträng Builder</span><span class="sxs-lookup"><span data-stu-id="c32b8-105">Relay Connection String Builder</span></span>

<span data-ttu-id="c32b8-106">Den [RelayConnectionStringBuilder] [ RelayConnectionStringBuilder] klassen formaterar anslutningssträngar som är specifika för Relay Hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="c32b8-106">The [RelayConnectionStringBuilder][RelayConnectionStringBuilder] class formats connection strings that are specific to Relay Hybrid Connections.</span></span> <span data-ttu-id="c32b8-107">Du kan använda den för att verifiera en Anslutningssträngens format eller att skapa en anslutningssträng från grunden.</span><span class="sxs-lookup"><span data-stu-id="c32b8-107">You can use it to verify the format of a connection string, or to build a connection string from scratch.</span></span> <span data-ttu-id="c32b8-108">Se följande kod för ett exempel:</span><span class="sxs-lookup"><span data-stu-id="c32b8-108">See the following code for an example:</span></span>

```csharp
var endpoint = "{Relay namespace}";
var entityPath = "{Name of the Hybrid Connection}";
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

<span data-ttu-id="c32b8-109">Du kan också ange en anslutningssträng direkt till den `RelayConnectionStringBuilder` metoden.</span><span class="sxs-lookup"><span data-stu-id="c32b8-109">You can also pass a connection string directly to the `RelayConnectionStringBuilder` method.</span></span> <span data-ttu-id="c32b8-110">Den här åtgärden kan du kontrollera att anslutningssträngen är i ett giltigt format.</span><span class="sxs-lookup"><span data-stu-id="c32b8-110">This operation enables you to verify that the connection string is in a valid format.</span></span> <span data-ttu-id="c32b8-111">Om någon av parametrarna är ogiltiga konstruktorn genererar en `ArgumentException`.</span><span class="sxs-lookup"><span data-stu-id="c32b8-111">If any of the parameters are invalid, the constructor generates an `ArgumentException`.</span></span>

```csharp
var myConnectionString = "{RelayConnectionString}";
// Declare the connectionStringBuilder so that it can be used outside of the loop if needed
RelayConnectionStringBuilder connectionStringBuilder;
try
{
    // Create the connectionStringBuilder using the supplied connection string
    connectionStringBuilder = new RelayConnectionStringBuilder(myConnectionString);
}
catch (ArgumentException ae)
{
    // Perform some error handling
}
```

## <a name="hybrid-connection-stream"></a><span data-ttu-id="c32b8-112">Dataströmmen för hybrid-anslutning</span><span class="sxs-lookup"><span data-stu-id="c32b8-112">Hybrid Connection Stream</span></span>
<span data-ttu-id="c32b8-113">Den [HybridConnectionStream] [ HCStream] klass är det primära objekt som används för att skicka och ta emot data från Azure Relay slutpunkt, oavsett om du arbetar med en [HybridConnectionClient] [ HCClient], eller en [HybridConnectionListener][HCListener].</span><span class="sxs-lookup"><span data-stu-id="c32b8-113">The [HybridConnectionStream][HCStream] class is the primary object used to send and receive data from an Azure Relay endpoint, whether you are working with a [HybridConnectionClient][HCClient], or a [HybridConnectionListener][HCListener].</span></span>

### <a name="getting-a-hybrid-connection-stream"></a><span data-ttu-id="c32b8-114">Hämtar en dataström för Hybrid-anslutning</span><span class="sxs-lookup"><span data-stu-id="c32b8-114">Getting a Hybrid Connection Stream</span></span>

#### <a name="listener"></a><span data-ttu-id="c32b8-115">Lyssnare</span><span class="sxs-lookup"><span data-stu-id="c32b8-115">Listener</span></span>
<span data-ttu-id="c32b8-116">Med hjälp av en [HybridConnectionListener][HCListener], kan du hämta en `HybridConnectionStream` objekt på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c32b8-116">Using a [HybridConnectionListener][HCListener], you can obtain a `HybridConnectionStream` object as follows:</span></span>

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var listener = new HybridConnectionListener(csb.ToString());
// Open a connection to the Relay endpoint
await listener.OpenAsync();
// Get a `HybridConnectionStream`
var hybridConnectionStream = await listener.AcceptConnectionAsync();
```

#### <a name="client"></a><span data-ttu-id="c32b8-117">Client</span><span class="sxs-lookup"><span data-stu-id="c32b8-117">Client</span></span>
<span data-ttu-id="c32b8-118">Med hjälp av en [HybridConnectionClient][HCClient], kan du hämta en `HybridConnectionStream` objekt på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c32b8-118">Using a [HybridConnectionClient][HCClient], you can obtain a `HybridConnectionStream` object as follows:</span></span>

```csharp
// Use the RelayConnectionStringBuilder to get a valid connection string
var client = new HybridConnectionClient(csb.ToString());
// Open a connection to the Relay endpoint and get a `HybridConnectionStream`
var hybridConnectionStream = await client.CreateConnectionAsync();
```

### <a name="receiving-data"></a><span data-ttu-id="c32b8-119">Ta emot data</span><span class="sxs-lookup"><span data-stu-id="c32b8-119">Receiving data</span></span>
<span data-ttu-id="c32b8-120">Den [HybridConnectionStream] [ HCStream] klassen kan dubbelriktad kommunikation.</span><span class="sxs-lookup"><span data-stu-id="c32b8-120">The [HybridConnectionStream][HCStream] class enables two-way communication.</span></span> <span data-ttu-id="c32b8-121">I de flesta fall felmeddelandet kontinuerligt från dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="c32b8-121">In most cases, you continuously receive from the stream.</span></span> <span data-ttu-id="c32b8-122">Om du läser text från dataströmmen kan du också vill använda en [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) -objektet, vilket möjliggör enklare tolkning av data.</span><span class="sxs-lookup"><span data-stu-id="c32b8-122">If you are reading text from the stream, you may also want to use a [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) object, which enables easier parsing of the data.</span></span> <span data-ttu-id="c32b8-123">Du kan till exempel läsa data som text i stället för som `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="c32b8-123">For example, you can read data as text, rather than as `byte[]`.</span></span>

<span data-ttu-id="c32b8-124">Följande kod läser enskilda rader med text från dataströmmen tills en annullering begärs:</span><span class="sxs-lookup"><span data-stu-id="c32b8-124">The following code reads individual lines of text from the stream until a cancellation is requested:</span></span>

```csharp
// Create a CancellationToken, so that we can cancel the while loop
var cancellationToken = new CancellationToken();
// Create a StreamReader from the 'hybridConnectionStream`
var streamReader = new StreamReader(hybridConnectionStream);

while (!cancellationToken.IsCancellationRequested)
{
    // Read a line of input until a newline is encountered
    var line = await streamReader.ReadLineAsync();
    if (string.IsNullOrEmpty(line))
    {
        // If there's no input data, we will signal that 
        // we will no longer send data on this connection
        // and then break out of the processing loop.
        await hybridConnectionStream.ShutdownAsync(cancellationToken);
        break;
    }
}
```

### <a name="sending-data"></a><span data-ttu-id="c32b8-125">Skicka data</span><span class="sxs-lookup"><span data-stu-id="c32b8-125">Sending data</span></span>
<span data-ttu-id="c32b8-126">När du har skapat en koppling kan du skicka ett meddelande till Relay-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="c32b8-126">Once you have a connection established, you can send a message to the Relay endpoint.</span></span> <span data-ttu-id="c32b8-127">Eftersom anslutningsobjektet ärver [dataströmmen](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), skicka dina data som en `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="c32b8-127">Because the connection object inherits [Stream](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), send your data as a `byte[]`.</span></span> <span data-ttu-id="c32b8-128">I följande exempel visas hur du gör detta:</span><span class="sxs-lookup"><span data-stu-id="c32b8-128">The following example shows how to do this:</span></span>

```csharp
var data = Encoding.UTF8.GetBytes("hello");
await clientConnection.WriteAsync(data, 0, data.Length);
```

<span data-ttu-id="c32b8-129">Men om du vill skicka text direkt, utan att behöva koda strängen varje gång du kan packa ihop den `hybridConnectionStream` objekt med en [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) objekt.</span><span class="sxs-lookup"><span data-stu-id="c32b8-129">However, if you want to send text directly, without needing to encode the string each time, you can wrap the `hybridConnectionStream` object with a [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) object.</span></span>

```csharp
// The StreamWriter object only needs to be created once
var textWriter = new StreamWriter(hybridConnectionStream);
await textWriter.WriteLineAsync("hello");
```

## <a name="next-steps"></a><span data-ttu-id="c32b8-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c32b8-130">Next steps</span></span>
<span data-ttu-id="c32b8-131">Mer information om Azure Relay finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="c32b8-131">To learn more about Azure Relay, visit these links:</span></span>

* [<span data-ttu-id="c32b8-132">Microsoft.Azure.Relay referens</span><span class="sxs-lookup"><span data-stu-id="c32b8-132">Microsoft.Azure.Relay reference</span></span>](/dotnet/api/microsoft.azure.relay)
* [<span data-ttu-id="c32b8-133">Vad är Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="c32b8-133">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="c32b8-134">Tillgängliga Relay-API: er</span><span class="sxs-lookup"><span data-stu-id="c32b8-134">Available Relay APIs</span></span>](relay-api-overview.md)

[RelayConnectionStringBuilder]: /dotnet/api/microsoft.azure.relay.relayconnectionstringbuilder
[HCStream]: /dotnet/api/microsoft.azure.relay.hybridconnectionstream
[HCClient]: /dotnet/api/microsoft.azure.relay.hybridconnectionclient
[HCListener]: /dotnet/api/microsoft.azure.relay.hybridconnectionlistener