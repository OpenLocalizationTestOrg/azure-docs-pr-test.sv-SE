---
title: "aaaPolling långvariga åtgärder | Microsoft Docs"
description: "Det här avsnittet visar hur toopoll långvariga åtgärder."
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
ms.openlocfilehash: f8315a5ddbe484d794c3e2164e47dd9e70521671
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="delivering-live-streaming-with-azure-media-services"></a><span data-ttu-id="db4fb-103">Leverera direktsänd strömning med Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="db4fb-103">Delivering Live Streaming with Azure Media Services</span></span>

## <a name="overview"></a><span data-ttu-id="db4fb-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="db4fb-104">Overview</span></span>

<span data-ttu-id="db4fb-105">Microsoft Azure Media Services erbjuder API: er som skickar begäranden tooMedia Services toostart åtgärder (till exempel: skapa, starta, stoppa eller ta bort en kanal).</span><span class="sxs-lookup"><span data-stu-id="db4fb-105">Microsoft Azure Media Services offers APIs that send requests tooMedia Services toostart operations (for example: create, start, stop, or delete a channel).</span></span> <span data-ttu-id="db4fb-106">Dessa åtgärder är långvariga.</span><span class="sxs-lookup"><span data-stu-id="db4fb-106">These operations are long-running.</span></span>

<span data-ttu-id="db4fb-107">hello Media Services .NET SDK innehåller API: er som skickar begäran om hello och vänta tills hello åtgärden toocomplete (internt hello API: er avsöker för åtgärden pågår vid vissa intervall).</span><span class="sxs-lookup"><span data-stu-id="db4fb-107">hello Media Services .NET SDK provides APIs that send hello request and wait for hello operation toocomplete (internally, hello APIs are polling for operation progress at some intervals).</span></span> <span data-ttu-id="db4fb-108">Till exempel när du anropar kanal. Start(), hello-metoden returnerar när hello kanal har startats.</span><span class="sxs-lookup"><span data-stu-id="db4fb-108">For example, when you call channel.Start(), hello method returns after hello channel is started.</span></span> <span data-ttu-id="db4fb-109">Du kan också använda hello asynkrona versionen: väntan på kanalen. StartAsync() (information om uppgiftsbaserade asynkront mönster finns [trycker du på](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)).</span><span class="sxs-lookup"><span data-stu-id="db4fb-109">You can also use hello asynchronous version: await channel.StartAsync() (for information about Task-based Asynchronous Pattern, see [TAP](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)).</span></span> <span data-ttu-id="db4fb-110">API: er som skickar en begäran om åtgärden och avsökas hello status tills hello-åtgärden har slutförts kallas ”avsökning metoder”.</span><span class="sxs-lookup"><span data-stu-id="db4fb-110">APIs that send an operation request and then poll for hello status until hello operation is complete are called “polling methods”.</span></span> <span data-ttu-id="db4fb-111">Dessa metoder (särskilt hello Async-version) rekommenderas för rich-klientprogram och/eller tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="db4fb-111">These methods (especially hello Async version) are recommended for rich client applications and/or stateful services.</span></span>

<span data-ttu-id="db4fb-112">Det finns scenarier där ett program inte kan vänta på en lång http-begäran och vill toopoll för hello åtgärden pågår manuellt.</span><span class="sxs-lookup"><span data-stu-id="db4fb-112">There are scenarios where an application cannot wait for a long running http request and wants toopoll for hello operation progress manually.</span></span> <span data-ttu-id="db4fb-113">Ett typiskt exempel är en webbläsare som interagerar med en tillståndslös webbtjänst: när hello webbläsare begär toocreate en kanal, webbtjänst hello initierar långvarig åtgärd och returnerar hello åtgärden ID toohello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="db4fb-113">A typical example would be a browser interacting with a stateless web service: when hello browser requests toocreate a channel, hello web service initiates a long running operation and returns hello operation ID toohello browser.</span></span> <span data-ttu-id="db4fb-114">hello webbläsare sedan be hello web service tooget hello Åtgärdsstatus baserat på hello-ID.</span><span class="sxs-lookup"><span data-stu-id="db4fb-114">hello browser could then ask hello web service tooget hello operation status based on hello ID.</span></span> <span data-ttu-id="db4fb-115">hello Media Services .NET SDK innehåller API: er som är användbara för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="db4fb-115">hello Media Services .NET SDK provides APIs that are useful for this scenario.</span></span> <span data-ttu-id="db4fb-116">Dessa API: er kallas ”icke-avsökning metoder”.</span><span class="sxs-lookup"><span data-stu-id="db4fb-116">These APIs are called “non-polling methods”.</span></span>
<span data-ttu-id="db4fb-117">hello ”icke-avsökning metoder” har hello följer mönstret: skicka*OperationName*åtgärd (till exempel SendCreateOperation).</span><span class="sxs-lookup"><span data-stu-id="db4fb-117">hello “non-polling methods” have hello following naming pattern: Send*OperationName*Operation (for example, SendCreateOperation).</span></span> <span data-ttu-id="db4fb-118">Skicka*OperationName*åtgärden metoder returnerar hello **IOperation** objektet; hello returnerade objekt innehåller information som kan vara används tootrack hello igen.</span><span class="sxs-lookup"><span data-stu-id="db4fb-118">Send*OperationName*Operation methods return hello **IOperation** object; hello returned object contains information that can be used tootrack hello operation.</span></span> <span data-ttu-id="db4fb-119">hello skicka*OperationName*OperationAsync metoder returnerar **aktivitet<IOperation>**.</span><span class="sxs-lookup"><span data-stu-id="db4fb-119">hello Send*OperationName*OperationAsync methods return **Task<IOperation>**.</span></span>

<span data-ttu-id="db4fb-120">För närvarande hello följande metoder för klasser stöd för icke-avsökning: **kanal**, **StreamingEndpoint**, och **programmet**.</span><span class="sxs-lookup"><span data-stu-id="db4fb-120">Currently, hello following classes support non-polling methods:  **Channel**, **StreamingEndpoint**, and **Program**.</span></span>

<span data-ttu-id="db4fb-121">toopoll för hello Åtgärdsstatus, Använd hello **GetOperation** metod på hello **OperationBaseCollection** klass.</span><span class="sxs-lookup"><span data-stu-id="db4fb-121">toopoll for hello operation status, use hello **GetOperation** method on hello **OperationBaseCollection** class.</span></span> <span data-ttu-id="db4fb-122">Använd hello följande intervall toocheck hello Åtgärdsstatus: för **kanal** och **StreamingEndpoint** åtgärder, använda 30 sekunder; för **programmet** åtgärder, använder 10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="db4fb-122">Use hello following intervals toocheck hello operation status: for **Channel** and **StreamingEndpoint** operations, use 30 seconds; for **Program** operations, use 10 seconds.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="db4fb-123">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="db4fb-123">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="db4fb-124">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="db4fb-124">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span>

## <a name="example"></a><span data-ttu-id="db4fb-125">Exempel</span><span class="sxs-lookup"><span data-stu-id="db4fb-125">Example</span></span>

<span data-ttu-id="db4fb-126">hello följande exempel definierar en klass med namnet **ChannelOperations**.</span><span class="sxs-lookup"><span data-stu-id="db4fb-126">hello following example defines a class called **ChannelOperations**.</span></span> <span data-ttu-id="db4fb-127">Definitionen av den här klassen kan vara en startpunkt för din web service klassdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="db4fb-127">This class definition could be a starting point for your web service class definition.</span></span> <span data-ttu-id="db4fb-128">För enkelhetens skull använder hello exemplen hello icke asynkrona versioner av metoder.</span><span class="sxs-lookup"><span data-stu-id="db4fb-128">For simplicity, hello following examples use hello non-async versions of methods.</span></span>

<span data-ttu-id="db4fb-129">hello exemplet visas hur hello klienten kan använda den här klassen.</span><span class="sxs-lookup"><span data-stu-id="db4fb-129">hello example also shows how hello client might use this class.</span></span>

### <a name="channeloperations-class-definition"></a><span data-ttu-id="db4fb-130">ChannelOperations klassdefinitionen</span><span class="sxs-lookup"><span data-stu-id="db4fb-130">ChannelOperations class definition</span></span>

    using Microsoft.WindowsAzure.MediaServices.Client;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Net;

    /// <summary> 
    /// hello ChannelOperations class only implements 
    /// hello Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from hello App.config file.
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
        /// Initiates hello creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name toobe given toohello new channel</param>  
        /// <returns>  
        /// Operation Id for hello long running operation being executed by Media Services. 
        /// Use this operation Id toopoll for hello channel creation status. 
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
        /// Checks if hello operation has been completed. 
        /// If hello operation succeeded, hello created channel Id is returned in hello out parameter.
        /// </summary> 
        /// <param name="operationId">hello operation Id.</param> 
        /// <param name="channel">
        /// If hello operation succeeded, 
        /// hello created channel Id is returned in hello out parameter.</param>
        /// <returns>Returns false if hello operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;

            channelId = null;

            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle hello failure. 
                    // For example, throw an exception. 
                    // Use hello following information in hello exception: operationId, operation.ErrorMessage.
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

### <a name="hello-client-code"></a><span data-ttu-id="db4fb-131">hello klientkod</span><span class="sxs-lookup"><span data-stu-id="db4fb-131">hello client code</span></span>
    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");

    string channelId = null;
    bool isCompleted = false;

    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }

    // If we got here, we should have hello newly created channel id.
    Console.WriteLine(channelId);



## <a name="media-services-learning-paths"></a><span data-ttu-id="db4fb-132">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="db4fb-132">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="db4fb-133">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="db4fb-133">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

