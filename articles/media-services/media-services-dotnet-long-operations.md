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
# <a name="delivering-live-streaming-with-azure-media-services"></a>Leverera direktsänd strömning med Azure Media Services

## <a name="overview"></a>Översikt

Microsoft Azure Media Services erbjuder API: er som skickar begäranden tooMedia Services toostart åtgärder (till exempel: skapa, starta, stoppa eller ta bort en kanal). Dessa åtgärder är långvariga.

hello Media Services .NET SDK innehåller API: er som skickar begäran om hello och vänta tills hello åtgärden toocomplete (internt hello API: er avsöker för åtgärden pågår vid vissa intervall). Till exempel när du anropar kanal. Start(), hello-metoden returnerar när hello kanal har startats. Du kan också använda hello asynkrona versionen: väntan på kanalen. StartAsync() (information om uppgiftsbaserade asynkront mönster finns [trycker du på](https://msdn.microsoft.com/library/hh873175\(v=vs.110\).aspx)). API: er som skickar en begäran om åtgärden och avsökas hello status tills hello-åtgärden har slutförts kallas ”avsökning metoder”. Dessa metoder (särskilt hello Async-version) rekommenderas för rich-klientprogram och/eller tillståndskänsliga tjänster.

Det finns scenarier där ett program inte kan vänta på en lång http-begäran och vill toopoll för hello åtgärden pågår manuellt. Ett typiskt exempel är en webbläsare som interagerar med en tillståndslös webbtjänst: när hello webbläsare begär toocreate en kanal, webbtjänst hello initierar långvarig åtgärd och returnerar hello åtgärden ID toohello webbläsare. hello webbläsare sedan be hello web service tooget hello Åtgärdsstatus baserat på hello-ID. hello Media Services .NET SDK innehåller API: er som är användbara för det här scenariot. Dessa API: er kallas ”icke-avsökning metoder”.
hello ”icke-avsökning metoder” har hello följer mönstret: skicka*OperationName*åtgärd (till exempel SendCreateOperation). Skicka*OperationName*åtgärden metoder returnerar hello **IOperation** objektet; hello returnerade objekt innehåller information som kan vara används tootrack hello igen. hello skicka*OperationName*OperationAsync metoder returnerar **aktivitet<IOperation>**.

För närvarande hello följande metoder för klasser stöd för icke-avsökning: **kanal**, **StreamingEndpoint**, och **programmet**.

toopoll för hello Åtgärdsstatus, Använd hello **GetOperation** metod på hello **OperationBaseCollection** klass. Använd hello följande intervall toocheck hello Åtgärdsstatus: för **kanal** och **StreamingEndpoint** åtgärder, använda 30 sekunder; för **programmet** åtgärder, använder 10 sekunder.

## <a name="create-and-configure-a-visual-studio-project"></a>Skapa och konfigurera ett Visual Studio-projekt

Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).

## <a name="example"></a>Exempel

hello följande exempel definierar en klass med namnet **ChannelOperations**. Definitionen av den här klassen kan vara en startpunkt för din web service klassdefinitionen. För enkelhetens skull använder hello exemplen hello icke asynkrona versioner av metoder.

hello exemplet visas hur hello klienten kan använda den här klassen.

### <a name="channeloperations-class-definition"></a>ChannelOperations klassdefinitionen

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

### <a name="hello-client-code"></a>hello klientkod
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



## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

