---
title: "Avsökning långvariga åtgärder | Microsoft Docs"
description: "Det här avsnittet visar hur du avsöker långvariga åtgärder."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 9a68c4b1-6159-42fe-9439-a3661a90ae03
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 7123a2d44d3b7c332afe30fb0fcea88ca29e313a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="delivering-live-streaming-with-azure-media-services"></a><span data-ttu-id="35b21-103">Leverera direktsänd strömning med Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="35b21-103">Delivering Live Streaming with Azure Media Services</span></span>

## <a name="overview"></a><span data-ttu-id="35b21-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="35b21-104">Overview</span></span>

<span data-ttu-id="35b21-105">Microsoft Azure Media Services erbjuder API: er som skickar begäranden till Media Services för att starta operations (till exempel: skapa, starta, stoppa eller ta bort en kanal).</span><span class="sxs-lookup"><span data-stu-id="35b21-105">Microsoft Azure Media Services offers APIs that send requests to Media Services to start operations (for example: create, start, stop, or delete a channel).</span></span> <span data-ttu-id="35b21-106">Dessa åtgärder är långvariga.</span><span class="sxs-lookup"><span data-stu-id="35b21-106">These operations are long-running.</span></span>

<span data-ttu-id="35b21-107">Media Services .NET SDK innehåller API: er som skickar begäran och vänta tills åtgärden har slutförts (internt, för API: er avsöker för åtgärden pågår vid vissa intervall).</span><span class="sxs-lookup"><span data-stu-id="35b21-107">The Media Services .NET SDK provides APIs that send the request and wait for the operation to complete (internally, the APIs are polling for operation progress at some intervals).</span></span> <span data-ttu-id="35b21-108">Till exempel när du anropar kanal. Start(), returnerar-metoden när kanalen har startats.</span><span class="sxs-lookup"><span data-stu-id="35b21-108">For example, when you call channel.Start(), the method returns after the channel is started.</span></span> <span data-ttu-id="35b21-109">Du kan också använda den asynkrona versionen: väntan på kanalen. StartAsync() (information om uppgiftsbaserade asynkront mönster finns [trycker du på](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)).</span><span class="sxs-lookup"><span data-stu-id="35b21-109">You can also use the asynchronous version: await channel.StartAsync() (for information about Task-based Asynchronous Pattern, see [TAP](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)).</span></span> <span data-ttu-id="35b21-110">API: er som skickar en begäran om åtgärden och söka efter status tills åtgärden har slutförts kallas ”avsökning metoder”.</span><span class="sxs-lookup"><span data-stu-id="35b21-110">APIs that send an operation request and then poll for the status until the operation is complete are called “polling methods”.</span></span> <span data-ttu-id="35b21-111">Dessa metoder (särskilt den asynkron versionen) rekommenderas för rich-klientprogram och/eller tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="35b21-111">These methods (especially the Async version) are recommended for rich client applications and/or stateful services.</span></span>

<span data-ttu-id="35b21-112">Det finns scenarier där ett program inte kan vänta på en lång http-begäran och vill söka efter åtgärden förloppet manuellt.</span><span class="sxs-lookup"><span data-stu-id="35b21-112">There are scenarios where an application cannot wait for a long running http request and wants to poll for the operation progress manually.</span></span> <span data-ttu-id="35b21-113">Ett typiskt exempel är en webbläsare som interagerar med en tillståndslös webbtjänst: när webbläsaren skickar en begäran för att skapa en kanal, webbtjänsten initierar en långvarig åtgärd och returnerar åtgärds-ID till webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="35b21-113">A typical example would be a browser interacting with a stateless web service: when the browser requests to create a channel, the web service initiates a long running operation and returns the operation ID to the browser.</span></span> <span data-ttu-id="35b21-114">Webbläsaren kan sedan ber du webbtjänsten för att hämta Åtgärdsstatus för baserat på ID.</span><span class="sxs-lookup"><span data-stu-id="35b21-114">The browser could then ask the web service to get the operation status based on the ID.</span></span> <span data-ttu-id="35b21-115">Media Services .NET SDK innehåller API: er som är användbara för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="35b21-115">The Media Services .NET SDK provides APIs that are useful for this scenario.</span></span> <span data-ttu-id="35b21-116">Dessa API: er kallas ”icke-avsökning metoder”.</span><span class="sxs-lookup"><span data-stu-id="35b21-116">These APIs are called “non-polling methods”.</span></span>
<span data-ttu-id="35b21-117">”Icke-avsökning metoder” har följande namngivningsmönstret: skicka*OperationName*åtgärd (till exempel SendCreateOperation).</span><span class="sxs-lookup"><span data-stu-id="35b21-117">The “non-polling methods” have the following naming pattern: Send*OperationName*Operation (for example, SendCreateOperation).</span></span> <span data-ttu-id="35b21-118">Skicka*OperationName*åtgärden metoder returnerar den **IOperation** objekt; den returnerade objekt innehåller information som kan användas för att spåra igen.</span><span class="sxs-lookup"><span data-stu-id="35b21-118">Send*OperationName*Operation methods return the **IOperation** object; the returned object contains information that can be used to track the operation.</span></span> <span data-ttu-id="35b21-119">Skicka*OperationName*OperationAsync metoder returnerar **aktivitet<IOperation>**.</span><span class="sxs-lookup"><span data-stu-id="35b21-119">The Send*OperationName*OperationAsync methods return **Task<IOperation>**.</span></span>

<span data-ttu-id="35b21-120">För närvarande följande klasser stöder inte avsökning metoder: **kanal**, **StreamingEndpoint**, och **programmet**.</span><span class="sxs-lookup"><span data-stu-id="35b21-120">Currently, the following classes support non-polling methods:  **Channel**, **StreamingEndpoint**, and **Program**.</span></span>

<span data-ttu-id="35b21-121">Om du vill söka efter Åtgärdsstatus använder den **GetOperation** -metoden i den **OperationBaseCollection** klass.</span><span class="sxs-lookup"><span data-stu-id="35b21-121">To poll for the operation status, use the **GetOperation** method on the **OperationBaseCollection** class.</span></span> <span data-ttu-id="35b21-122">Använd följande intervall för att kontrollera Åtgärdsstatus: för **kanal** och **StreamingEndpoint** åtgärder, använda 30 sekunder; för **programmet** åtgärder, använder 10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="35b21-122">Use the following intervals to check the operation status: for **Channel** and **StreamingEndpoint** operations, use 30 seconds; for **Program** operations, use 10 seconds.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="35b21-123">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="35b21-123">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="35b21-124">Konfigurera utvecklingsmiljön och fyll i filen app.config med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="35b21-124">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span>

## <a name="example"></a><span data-ttu-id="35b21-125">Exempel</span><span class="sxs-lookup"><span data-stu-id="35b21-125">Example</span></span>

<span data-ttu-id="35b21-126">I följande exempel definierar en klass med namnet **ChannelOperations**.</span><span class="sxs-lookup"><span data-stu-id="35b21-126">The following example defines a class called **ChannelOperations**.</span></span> <span data-ttu-id="35b21-127">Definitionen av den här klassen kan vara en startpunkt för din web service klassdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="35b21-127">This class definition could be a starting point for your web service class definition.</span></span> <span data-ttu-id="35b21-128">I följande exempel används ej asynkrona versioner av metoder för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="35b21-128">For simplicity, the following examples use the non-async versions of methods.</span></span>

<span data-ttu-id="35b21-129">Exemplet visar även hur klienten kan använda den här klassen.</span><span class="sxs-lookup"><span data-stu-id="35b21-129">The example also shows how the client might use this class.</span></span>

### <a name="channeloperations-class-definition"></a><span data-ttu-id="35b21-130">ChannelOperations klassdefinitionen</span><span class="sxs-lookup"><span data-stu-id="35b21-130">ChannelOperations class definition</span></span>

    using Microsoft.WindowsAzure.MediaServices.Client;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Net;

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        public ChannelOperations()
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
        }

        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });

            return operation.Id;
        }

        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;

            channelId = null;

            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }

        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                }
            };
        }

        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                }
            };
        }

        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

### <a name="the-client-code"></a><span data-ttu-id="35b21-131">Klientkoden</span><span class="sxs-lookup"><span data-stu-id="35b21-131">The client code</span></span>
    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");

    string channelId = null;
    bool isCompleted = false;

    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }

    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);



## <a name="media-services-learning-paths"></a><span data-ttu-id="35b21-132">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="35b21-132">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="35b21-133">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="35b21-133">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

