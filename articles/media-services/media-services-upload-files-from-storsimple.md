---
title: "aaaUpload filer till ett Azure Media Services-konto från Azure StorSimple | Microsoft Docs"
description: "I den här artikeln finns en kort översikt över Azure StorSimple Data Manager. hello artikeln innehåller också länkar tootutorials som visar hur tooextract data från StorSimple och ladda upp det som tillgångar tooan Azure Media Services-konto."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 1dd09328-262b-43ef-8099-73241b49a925
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/27/2017
ms.author: juliako
ms.openlocfilehash: 7e9712aa480106bbd5fcc63eaecf0418b24a8bef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-an-azure-media-services-account-from-azure-storsimple"></a><span data-ttu-id="074cb-104">Ladda upp filer till ett Azure Media Services-konto från Azure StorSimple</span><span class="sxs-lookup"><span data-stu-id="074cb-104">Upload files into an Azure Media Services account from Azure StorSimple</span></span>

<span data-ttu-id="074cb-105">I den här artikeln finns en kort översikt över Azure StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="074cb-105">This article gives a brief overview of Azure StorSimple Data Manager.</span></span> <span data-ttu-id="074cb-106">hello artikeln innehåller också länkar tootutorials som visar hur tooextract data från StorSimple och ladda upp data som tillgångar tooan konto i Azure Media Services (AMS).</span><span class="sxs-lookup"><span data-stu-id="074cb-106">hello article also links tootutorials that show you how tooextract data from StorSimple and upload this data as assets tooan Azure Media Services (AMS) account.</span></span>

> 
> [!NOTE]
> <span data-ttu-id="074cb-107">Azure StorSimple Data Manager finns för närvarande som privat förhandsutgåva.</span><span class="sxs-lookup"><span data-stu-id="074cb-107">Azure StorSimple Data Manager is currently in private preview.</span></span> 
> 

## <a name="overview"></a><span data-ttu-id="074cb-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="074cb-108">Overview</span></span>

<span data-ttu-id="074cb-109">I Media Services överför du dina digitala filer till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="074cb-109">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="074cb-110">hello tillgång kan innehålla video, ljud, bilder, miniatyrsamlingar, text spår och textning filer (och hello metadata om dessa filer.) När hello filerna har överförts lagras innehållet på ett säkert sätt i hello molnet för ytterligare bearbetning och strömning.</span><span class="sxs-lookup"><span data-stu-id="074cb-110">hello Asset  can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.) Once hello files are uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span>

<span data-ttu-id="074cb-111">[Azure StorSimple](https://docs.microsoft.com/azure/storsimple/) använder molnlagring som ett tillägg av hello lokalt lösning och automatiskt nivåer data över hello lokal lagring och lagringsutrymmet i molnet.</span><span class="sxs-lookup"><span data-stu-id="074cb-111">[Azure StorSimple](https://docs.microsoft.com/azure/storsimple/) uses cloud storage as an extension of hello on-premises solution and automatically tiers data across hello on-premises storage and cloud storage.</span></span> <span data-ttu-id="074cb-112">Hej StorSimple-enhet dedupes och komprimerar data innan du skickar den toohello moln, vilket gör det mycket effektivt för att skicka stora filer toohello moln.</span><span class="sxs-lookup"><span data-stu-id="074cb-112">hello StorSimple device dedupes and compresses your data before sending it toohello cloud making it very efficient for sending large files toohello cloud.</span></span> <span data-ttu-id="074cb-113">Hej [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md) service ger API: er som aktiverar du tooextract data från StorSimple och visa AMS tillgångar.</span><span class="sxs-lookup"><span data-stu-id="074cb-113">hello [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md) service provides APIs that enable you tooextract data from StorSimple and present it as AMS assets.</span></span>

## <a name="get-started"></a><span data-ttu-id="074cb-114">Kom igång</span><span class="sxs-lookup"><span data-stu-id="074cb-114">Get started</span></span>

1. <span data-ttu-id="074cb-115">[Skapa ett Media Services-konto](media-services-portal-create-account.md) som du vill tootransfer hello tillgångar.</span><span class="sxs-lookup"><span data-stu-id="074cb-115">[Create a Media Services account](media-services-portal-create-account.md) into which you want tootransfer hello assets.</span></span>
2. <span data-ttu-id="074cb-116">Registrera dig för förhandsversionen av Data Manager, enligt beskrivningen i hello [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="074cb-116">Sign up for Data Manager preview, as described in hello [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md) article.</span></span>
3. <span data-ttu-id="074cb-117">Skapa ett StorSimple Data Manager-konto.</span><span class="sxs-lookup"><span data-stu-id="074cb-117">Create a StorSimple Data Manager account.</span></span>
4. <span data-ttu-id="074cb-118">Skapa ett omvandlingsjobb som när det körs hämtar data från en StorSimple-enhet och överför dem till ett AMS-konto som tillgångar.</span><span class="sxs-lookup"><span data-stu-id="074cb-118">Create a data transformation job that when runs, extracts data from a StorSimple device and transfers it into an AMS account as assets.</span></span> 

    <span data-ttu-id="074cb-119">När hello jobb börjar köras, skapas en storage-kö.</span><span class="sxs-lookup"><span data-stu-id="074cb-119">When hello job starts running, a storage queue is created.</span></span> <span data-ttu-id="074cb-120">Den här kön fylls med meddelanden om transformerade blobbar i takt med att de är klara.</span><span class="sxs-lookup"><span data-stu-id="074cb-120">This queue is populated with messages about transformed blobs as they are ready.</span></span> <span data-ttu-id="074cb-121">hello namnet på den här kön är hello samma som hello namnet på hello jobbdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="074cb-121">hello name of this queue is hello same as hello name of hello job definition.</span></span> <span data-ttu-id="074cb-122">Du kan använda den här kön toodetermine när tillgången är klar och anropa din önskade toorun för Media Services-åtgärden på den.</span><span class="sxs-lookup"><span data-stu-id="074cb-122">You can use this queue toodetermine when as asset is ready and call your desired Media Services operation toorun on it.</span></span> <span data-ttu-id="074cb-123">Du kan till exempel använda den här kön tootrigger en Azure-funktion som innehåller hello nödvändig Media Services-kod.</span><span class="sxs-lookup"><span data-stu-id="074cb-123">For example, you can use this queue tootrigger an Azure Function that has hello necessary Media Services code in it.</span></span>

## <a name="see-also"></a><span data-ttu-id="074cb-124">Se även</span><span class="sxs-lookup"><span data-stu-id="074cb-124">See also</span></span>

[<span data-ttu-id="074cb-125">Använd hello .net SDK tootrigger jobb i hello Data Manager</span><span class="sxs-lookup"><span data-stu-id="074cb-125">Use hello .Net SDK tootrigger jobs in hello Data Manager</span></span>](../storsimple/storsimple-data-manager-dotnet-jobs.md)

## <a name="media-services-learning-paths"></a><span data-ttu-id="074cb-126">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="074cb-126">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="074cb-127">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="074cb-127">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="074cb-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="074cb-128">Next steps</span></span>

<span data-ttu-id="074cb-129">Du kan nu koda överförda tillgångar.</span><span class="sxs-lookup"><span data-stu-id="074cb-129">You can now encode your uploaded assets.</span></span> <span data-ttu-id="074cb-130">Mer information finns i [Koda tillgångar](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="074cb-130">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>
