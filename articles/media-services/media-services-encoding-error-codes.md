---
title: aaaAzure Media Services encoding felkoder | Microsoft Docs
description: "Det här avsnittet beskrivs felkoder som kan returneras om ett fel uppstod under hello kodning för körning av aktiviteten..."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: b69b6abee797c40c9b8b8f23bf2398273c170e7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encoding-error-codes"></a><span data-ttu-id="d504b-103">Kodning felkoder</span><span class="sxs-lookup"><span data-stu-id="d504b-103">Encoding error codes</span></span>

<span data-ttu-id="d504b-104">hello visas följande tabell felkoder som kan returneras om ett fel uppstod under hello kodning för körning av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="d504b-104">hello following table lists error codes that could be returned in case an error was encountered during hello encoding task execution.</span></span>  <span data-ttu-id="d504b-105">tooget felinformation i .NET-kod använder hello [felinformation](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="d504b-105">tooget error details in your .NET code, use hello [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) class.</span></span> <span data-ttu-id="d504b-106">tooget felinformation i REST-koden använder hello [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.</span><span class="sxs-lookup"><span data-stu-id="d504b-106">tooget error details in your REST code, use hello [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.</span></span>

| <span data-ttu-id="d504b-107">ErrorDetail.Code</span><span class="sxs-lookup"><span data-stu-id="d504b-107">ErrorDetail.Code</span></span> | <span data-ttu-id="d504b-108">Möjliga orsaker till felet</span><span class="sxs-lookup"><span data-stu-id="d504b-108">Possible causes for error</span></span> |
| --- | --- |
| <span data-ttu-id="d504b-109">Okänd</span><span class="sxs-lookup"><span data-stu-id="d504b-109">Unknown</span></span> |<span data-ttu-id="d504b-110">Okänt fel vid körning av hello-aktivitet</span><span class="sxs-lookup"><span data-stu-id="d504b-110">Unknown error while executing hello task</span></span> |
| <span data-ttu-id="d504b-111">ErrorDownloadingInputAssetMalformedContent</span><span class="sxs-lookup"><span data-stu-id="d504b-111">ErrorDownloadingInputAssetMalformedContent</span></span> |<span data-ttu-id="d504b-112">Kategori för fel som täcker fel i hämtar inkommande tillgång till exempel felaktiga filnamn, noll längd filer, felaktig formaterar och så vidare.</span><span class="sxs-lookup"><span data-stu-id="d504b-112">Category of errors that covers errors in downloading input asset such as bad file names, zero length files, incorrect formats and so on.</span></span> |
| <span data-ttu-id="d504b-113">ErrorDownloadingInputAssetServiceFailure</span><span class="sxs-lookup"><span data-stu-id="d504b-113">ErrorDownloadingInputAssetServiceFailure</span></span> |<span data-ttu-id="d504b-114">Kategori för fel som beskriver problem på tjänstsidan hello - exempel nätverks- eller fel vid hämtning av.</span><span class="sxs-lookup"><span data-stu-id="d504b-114">Category of errors that covers problems on hello service side - for example network or storage errors while downloading.</span></span> |
| <span data-ttu-id="d504b-115">ErrorParsingConfiguration</span><span class="sxs-lookup"><span data-stu-id="d504b-115">ErrorParsingConfiguration</span></span> |<span data-ttu-id="d504b-116">Kategori av fel där uppgift <see cref="MediaTask.PrivateData"/> (konfiguration) är inte giltig, till exempel hello-konfigurationen är inte ett giltigt system förinställningen eller innehåller ogiltig XML.</span><span class="sxs-lookup"><span data-stu-id="d504b-116">Category of errors where task <see cref="MediaTask.PrivateData"/> (configuration) is not valid, for example hello configuration is not a valid system preset or it contains invalid XML.</span></span> |
| <span data-ttu-id="d504b-117">ErrorExecutingTaskMalformedContent</span><span class="sxs-lookup"><span data-stu-id="d504b-117">ErrorExecutingTaskMalformedContent</span></span> |<span data-ttu-id="d504b-118">Fel under hello körning av hello uppgift där problem i hello indata mediefiler orsaka fel kategori.</span><span class="sxs-lookup"><span data-stu-id="d504b-118">Category of errors during hello execution of hello task where issues inside hello input media files cause failure.</span></span> |
| <span data-ttu-id="d504b-119">ErrorExecutingTaskUnsupportedFormat</span><span class="sxs-lookup"><span data-stu-id="d504b-119">ErrorExecutingTaskUnsupportedFormat</span></span> |<span data-ttu-id="d504b-120">Kategori av fel där hello media kunde inte bearbeta filer hello - medieformat inte stöds eller matchar inte hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d504b-120">Category of errors where hello media processor cannot process hello files provided - media format not supported, or does not match hello Configuration.</span></span> <span data-ttu-id="d504b-121">Till exempel försök tooproduce ljuddata utdata från en tillgång som har endast video</span><span class="sxs-lookup"><span data-stu-id="d504b-121">For example, trying tooproduce an audio-only output from an asset that has only video</span></span> |
| <span data-ttu-id="d504b-122">ErrorProcessingTask</span><span class="sxs-lookup"><span data-stu-id="d504b-122">ErrorProcessingTask</span></span> |<span data-ttu-id="d504b-123">Kategori för andra fel som hello medieprocessor uppstår under hello bearbetning av hello-aktivitet som är inte relaterat toocontent.</span><span class="sxs-lookup"><span data-stu-id="d504b-123">Category of other errors that hello media processor encounters during hello processing of hello task that are unrelated toocontent.</span></span> |
| <span data-ttu-id="d504b-124">ErrorUploadingOutputAsset</span><span class="sxs-lookup"><span data-stu-id="d504b-124">ErrorUploadingOutputAsset</span></span> |<span data-ttu-id="d504b-125">Fel vid överföring av hello utdatatillgången kategori</span><span class="sxs-lookup"><span data-stu-id="d504b-125">Category of errors when uploading hello output asset</span></span> |
| <span data-ttu-id="d504b-126">ErrorCancelingTask</span><span class="sxs-lookup"><span data-stu-id="d504b-126">ErrorCancelingTask</span></span> |<span data-ttu-id="d504b-127">Kategori fel toocover fel vid försök toocancel hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="d504b-127">Category of errors toocover failures when attempting toocancel hello Task</span></span> |
| <span data-ttu-id="d504b-128">TransientError</span><span class="sxs-lookup"><span data-stu-id="d504b-128">TransientError</span></span> |<span data-ttu-id="d504b-129">Kategori av fel toocover tillfälliga problem (t.ex.</span><span class="sxs-lookup"><span data-stu-id="d504b-129">Category of errors toocover transient issues (eg.</span></span> <span data-ttu-id="d504b-130">tillfällig nätverksproblem med Azure Storage)</span><span class="sxs-lookup"><span data-stu-id="d504b-130">temporary networking issues with Azure Storage)</span></span> |

<span data-ttu-id="d504b-131">tooget hjälp från hello **Media Services** team, öppna en [stöder biljett](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="d504b-131">tooget help from hello **Media Services** team, open a [support ticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="d504b-132">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="d504b-132">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d504b-133">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="d504b-133">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="d504b-134">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="d504b-134">Related articles</span></span>
* [<span data-ttu-id="d504b-135">Utföra avancerade kodning uppgifter genom att anpassa Media Encoder Standard förinställningar</span><span class="sxs-lookup"><span data-stu-id="d504b-135">Perform advanced encoding tasks by customizing Media Encoder Standard presets</span></span>](media-services-custom-mes-presets-with-dotnet.md)
* [<span data-ttu-id="d504b-136">Kvoter och begränsningar</span><span class="sxs-lookup"><span data-stu-id="d504b-136">Quotas and Limitations</span></span>](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
