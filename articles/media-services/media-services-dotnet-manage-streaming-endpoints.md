---
title: "aaaManage strömningsslutpunkter med .NET SDK. | Microsoft Docs"
description: "Det här avsnittet visar hur toomanage strömningsslutpunkter med hello Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: 0da34a97-f36c-48d0-8ea2-ec12584a2215
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 30c092a8ebf4e2b2902392f4cf98f46d812ccdbc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-streaming-endpoints-with-net-sdk"></a><span data-ttu-id="4476c-104">Hantera strömningsslutpunkter med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4476c-104">Manage streaming endpoints with .NET SDK</span></span>

>[!NOTE]
><span data-ttu-id="4476c-105">Se till att tooreview hello [översikt](media-services-streaming-endpoints-overview.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4476c-105">Make sure tooreview hello [overview](media-services-streaming-endpoints-overview.md) topic.</span></span> <span data-ttu-id="4476c-106">Granska även [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span><span class="sxs-lookup"><span data-stu-id="4476c-106">Also, review [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span></span>

<span data-ttu-id="4476c-107">hello kod i det här avsnittet visar hur toodo hello följande aktiviteter med hjälp av hello Azure Media Services .NET SDK:</span><span class="sxs-lookup"><span data-stu-id="4476c-107">hello code in this topic shows how toodo hello following tasks using hello Azure Media Services .NET SDK:</span></span>

- <span data-ttu-id="4476c-108">Granska hello standard strömmande slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="4476c-108">Examine hello default streaming endpoint.</span></span>
- <span data-ttu-id="4476c-109">Skapa/lägga till nya strömmande slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="4476c-109">Create/add new streaming endpoint.</span></span>

    <span data-ttu-id="4476c-110">Du kanske vill toohave flera strömningsslutpunkter om du planerar toohave olika CDN-nät eller en CDN och direktåtkomst.</span><span class="sxs-lookup"><span data-stu-id="4476c-110">You might want toohave multiple streaming endpoints if you plan toohave different CDNs or a CDN and direct access.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4476c-111">Du debiteras endast när din Strömningsslutpunkt är i körningstillstånd.</span><span class="sxs-lookup"><span data-stu-id="4476c-111">You are only billed when your Streaming Endpoint is in running state.</span></span>
    
- <span data-ttu-id="4476c-112">Uppdatera hello strömmande slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="4476c-112">Update hello streaming endpoint.</span></span>
    
    <span data-ttu-id="4476c-113">Se till att toocall hello Update() funktion.</span><span class="sxs-lookup"><span data-stu-id="4476c-113">Make sure toocall hello Update() function.</span></span>

- <span data-ttu-id="4476c-114">Ta bort hello strömmande slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="4476c-114">Delete hello streaming endpoint.</span></span>

    >[!NOTE]
    ><span data-ttu-id="4476c-115">hello standard strömmande slutpunkten kan inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="4476c-115">hello default streaming endpoint cannot be deleted.</span></span>

<span data-ttu-id="4476c-116">Information om hur tooscale hello strömmande slutpunkten finns [detta](media-services-portal-scale-streaming-endpoints.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4476c-116">For information about how tooscale hello streaming endpoint, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="4476c-117">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="4476c-117">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="4476c-118">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="4476c-118">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="add-code-that-manages-streaming-endpoints"></a><span data-ttu-id="4476c-119">Lägg till kod som hanterar strömningsslutpunkter</span><span class="sxs-lookup"><span data-stu-id="4476c-119">Add code that manages streaming endpoints</span></span>
    
<span data-ttu-id="4476c-120">Ersätt hello koden i hello Program.cs med följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="4476c-120">Replace hello code in hello Program.cs with hello following code:</span></span>

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.Live;

    namespace AMSStreamingEndpoint
    {
        class Program
        {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            var defaultStreamingEndpoint = _context.StreamingEndpoints.Where(s => s.Name.Contains("default")).FirstOrDefault();
            ExamineStreamingEndpoint(defaultStreamingEndpoint);

            IStreamingEndpoint newStreamingEndpoint = AddStreamingEndpoint();
            UpdateStreamingEndpoint(newStreamingEndpoint);
            DeleteStreamingEndpoint(newStreamingEndpoint);
        }

        static public void ExamineStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
        {
            Console.WriteLine(streamingEndpoint.Name);
            Console.WriteLine(streamingEndpoint.StreamingEndpointVersion);
            Console.WriteLine(streamingEndpoint.FreeTrialEndTime);
            Console.WriteLine(streamingEndpoint.ScaleUnits);
            Console.WriteLine(streamingEndpoint.CdnProvider);
            Console.WriteLine(streamingEndpoint.CdnProfile);
            Console.WriteLine(streamingEndpoint.CdnEnabled);
        }

        static public IStreamingEndpoint AddStreamingEndpoint()
        {
            var name = "StreamingEndpoint" + DateTime.UtcNow.ToString("hhmmss");
            var option = new StreamingEndpointCreationOptions(name, 1)
            {
            StreamingEndpointVersion = new Version("2.0"),
            CdnEnabled = true,
            CdnProfile = "CdnProfile",
            CdnProvider = CdnProviderType.PremiumVerizon
            };

            var streamingEndpoint = _context.StreamingEndpoints.Create(option);

            return streamingEndpoint;
        }

        static public void UpdateStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
        {
            if (streamingEndpoint.StreamingEndpointVersion == "1.0")
            streamingEndpoint.StreamingEndpointVersion = "2.0";

            streamingEndpoint.CdnEnabled = false;
            streamingEndpoint.Update();
        }

        static public void DeleteStreamingEndpoint(IStreamingEndpoint streamingEndpoint)
        {
            streamingEndpoint.Delete();
        }
        }
    }


## <a name="next-steps"></a><span data-ttu-id="4476c-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4476c-121">Next steps</span></span>
<span data-ttu-id="4476c-122">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="4476c-122">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4476c-123">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="4476c-123">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

