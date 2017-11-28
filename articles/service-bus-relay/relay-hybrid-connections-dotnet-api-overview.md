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
# <a name="azure-relay-hybrid-connections-net-standard-api-overview"></a><span data-ttu-id="5cc83-103">Översikt över Azure Relay Hybrid anslutningar .NET Standard-API</span><span class="sxs-lookup"><span data-stu-id="5cc83-103">Azure Relay Hybrid Connections .NET Standard API overview</span></span>

<span data-ttu-id="5cc83-104">Den här artikeln sammanfattas några av hello nyckeln Azure Relay Hybrid anslutningar .NET Standard [klientens API: er](/dotnet/api/microsoft.azure.relay).</span><span class="sxs-lookup"><span data-stu-id="5cc83-104">This article summarizes some of hello key Azure Relay Hybrid Connections .NET Standard [client APIs](/dotnet/api/microsoft.azure.relay).</span></span>
  
## <a name="relay-connection-string-builder"></a><span data-ttu-id="5cc83-105">Relay-anslutning sträng Builder</span><span class="sxs-lookup"><span data-stu-id="5cc83-105">Relay Connection String Builder</span></span>

<span data-ttu-id="5cc83-106">Hej [RelayConnectionStringBuilder] [ RelayConnectionStringBuilder] klassen formaterar anslutningssträngar som är specifika tooRelay Hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="5cc83-106">hello [RelayConnectionStringBuilder][RelayConnectionStringBuilder] class formats connection strings that are specific tooRelay Hybrid Connections.</span></span> <span data-ttu-id="5cc83-107">Du kan använda den tooverify hello format i anslutningssträngen eller toobuild en anslutningssträng från grunden.</span><span class="sxs-lookup"><span data-stu-id="5cc83-107">You can use it tooverify hello format of a connection string, or toobuild a connection string from scratch.</span></span> <span data-ttu-id="5cc83-108">Se hello följande kod för ett exempel:</span><span class="sxs-lookup"><span data-stu-id="5cc83-108">See hello following code for an example:</span></span>

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

<span data-ttu-id="5cc83-109">Du kan också skicka en anslutning direkt string toohello `RelayConnectionStringBuilder` metod.</span><span class="sxs-lookup"><span data-stu-id="5cc83-109">You can also pass a connection string directly toohello `RelayConnectionStringBuilder` method.</span></span> <span data-ttu-id="5cc83-110">Den här åtgärden aktiverar tooverify hello anslutningssträngen är ett ogiltigt format.</span><span class="sxs-lookup"><span data-stu-id="5cc83-110">This operation enables you tooverify that hello connection string is in a valid format.</span></span> <span data-ttu-id="5cc83-111">Om någon av hello-parametrarna är ogiltiga hello konstruktorn genererar en `ArgumentException`.</span><span class="sxs-lookup"><span data-stu-id="5cc83-111">If any of hello parameters are invalid, hello constructor generates an `ArgumentException`.</span></span>

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

## <a name="hybrid-connection-stream"></a><span data-ttu-id="5cc83-112">Dataströmmen för hybrid-anslutning</span><span class="sxs-lookup"><span data-stu-id="5cc83-112">Hybrid Connection Stream</span></span>
<span data-ttu-id="5cc83-113">Hej [HybridConnectionStream] [ HCStream] klass är hello primära objektet som används för toosend och ta emot data från Azure Relay slutpunkt, oavsett om du arbetar med en [HybridConnectionClient] [ HCClient], eller en [HybridConnectionListener][HCListener].</span><span class="sxs-lookup"><span data-stu-id="5cc83-113">hello [HybridConnectionStream][HCStream] class is hello primary object used toosend and receive data from an Azure Relay endpoint, whether you are working with a [HybridConnectionClient][HCClient], or a [HybridConnectionListener][HCListener].</span></span>

### <a name="getting-a-hybrid-connection-stream"></a><span data-ttu-id="5cc83-114">Hämtar en dataström för Hybrid-anslutning</span><span class="sxs-lookup"><span data-stu-id="5cc83-114">Getting a Hybrid Connection Stream</span></span>

#### <a name="listener"></a><span data-ttu-id="5cc83-115">Lyssnare</span><span class="sxs-lookup"><span data-stu-id="5cc83-115">Listener</span></span>
<span data-ttu-id="5cc83-116">Med hjälp av en [HybridConnectionListener][HCListener], kan du hämta en `HybridConnectionStream` objekt på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5cc83-116">Using a [HybridConnectionListener][HCListener], you can obtain a `HybridConnectionStream` object as follows:</span></span>

```csharp
// Use hello RelayConnectionStringBuilder tooget a valid connection string
var listener = new HybridConnectionListener(csb.ToString());
// Open a connection toohello Relay endpoint
await listener.OpenAsync();
// Get a `HybridConnectionStream`
var hybridConnectionStream = await listener.AcceptConnectionAsync();
```

#### <a name="client"></a><span data-ttu-id="5cc83-117">Client</span><span class="sxs-lookup"><span data-stu-id="5cc83-117">Client</span></span>
<span data-ttu-id="5cc83-118">Med hjälp av en [HybridConnectionClient][HCClient], kan du hämta en `HybridConnectionStream` objekt på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5cc83-118">Using a [HybridConnectionClient][HCClient], you can obtain a `HybridConnectionStream` object as follows:</span></span>

```csharp
// Use hello RelayConnectionStringBuilder tooget a valid connection string
var client = new HybridConnectionClient(csb.ToString());
// Open a connection toohello Relay endpoint and get a `HybridConnectionStream`
var hybridConnectionStream = await client.CreateConnectionAsync();
```

### <a name="receiving-data"></a><span data-ttu-id="5cc83-119">Ta emot data</span><span class="sxs-lookup"><span data-stu-id="5cc83-119">Receiving data</span></span>
<span data-ttu-id="5cc83-120">Hej [HybridConnectionStream] [ HCStream] klassen kan dubbelriktad kommunikation.</span><span class="sxs-lookup"><span data-stu-id="5cc83-120">hello [HybridConnectionStream][HCStream] class enables two-way communication.</span></span> <span data-ttu-id="5cc83-121">I de flesta fall felmeddelandet kontinuerligt från hello dataström.</span><span class="sxs-lookup"><span data-stu-id="5cc83-121">In most cases, you continuously receive from hello stream.</span></span> <span data-ttu-id="5cc83-122">Om du läser text från hello stream, kan du också toouse en [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) -objektet, vilket möjliggör enklare tolkning av hello data.</span><span class="sxs-lookup"><span data-stu-id="5cc83-122">If you are reading text from hello stream, you may also want toouse a [StreamReader](https://msdn.microsoft.com/library/system.io.streamreader(v=vs.110).aspx) object, which enables easier parsing of hello data.</span></span> <span data-ttu-id="5cc83-123">Du kan till exempel läsa data som text i stället för som `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="5cc83-123">For example, you can read data as text, rather than as `byte[]`.</span></span>

<span data-ttu-id="5cc83-124">hello läser följande kod enskilda rader med text från hello dataström tills en annullering begärs:</span><span class="sxs-lookup"><span data-stu-id="5cc83-124">hello following code reads individual lines of text from hello stream until a cancellation is requested:</span></span>

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

### <a name="sending-data"></a><span data-ttu-id="5cc83-125">Skicka data</span><span class="sxs-lookup"><span data-stu-id="5cc83-125">Sending data</span></span>
<span data-ttu-id="5cc83-126">När du har skapat en koppling, kan du skicka ett meddelande toohello Relay-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="5cc83-126">Once you have a connection established, you can send a message toohello Relay endpoint.</span></span> <span data-ttu-id="5cc83-127">Eftersom hello anslutningsobjekt ärver [dataströmmen](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), skicka dina data som en `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="5cc83-127">Because hello connection object inherits [Stream](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx), send your data as a `byte[]`.</span></span> <span data-ttu-id="5cc83-128">följande exempel visar hur hello toodo detta:</span><span class="sxs-lookup"><span data-stu-id="5cc83-128">hello following example shows how toodo this:</span></span>

```csharp
var data = Encoding.UTF8.GetBytes("hello");
await clientConnection.WriteAsync(data, 0, data.Length);
```

<span data-ttu-id="5cc83-129">Om du vill toosend texten direkt, utan att behöva tooencode hello sträng varje gång du dock omsluter hello `hybridConnectionStream` objekt med en [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) objekt.</span><span class="sxs-lookup"><span data-stu-id="5cc83-129">However, if you want toosend text directly, without needing tooencode hello string each time, you can wrap hello `hybridConnectionStream` object with a [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) object.</span></span>

```csharp
// hello StreamWriter object only needs toobe created once
var textWriter = new StreamWriter(hybridConnectionStream);
await textWriter.WriteLineAsync("hello");
```

## <a name="next-steps"></a><span data-ttu-id="5cc83-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5cc83-130">Next steps</span></span>
<span data-ttu-id="5cc83-131">toolearn mer om Azure Relay finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="5cc83-131">toolearn more about Azure Relay, visit these links:</span></span>

* [<span data-ttu-id="5cc83-132">Microsoft.Azure.Relay referens</span><span class="sxs-lookup"><span data-stu-id="5cc83-132">Microsoft.Azure.Relay reference</span></span>](/dotnet/api/microsoft.azure.relay)
* [<span data-ttu-id="5cc83-133">Vad är Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="5cc83-133">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="5cc83-134">Tillgängliga Relay-API: er</span><span class="sxs-lookup"><span data-stu-id="5cc83-134">Available Relay APIs</span></span>](relay-api-overview.md)

[RelayConnectionStringBuilder]: /dotnet/api/microsoft.azure.relay.relayconnectionstringbuilder
[HCStream]: /dotnet/api/microsoft.azure.relay.hybridconnectionstream
[HCClient]: /dotnet/api/microsoft.azure.relay.hybridconnectionclient
[HCListener]: /dotnet/api/microsoft.azure.relay.hybridconnectionlistener