---
title: "Ladda upp filer till ett Azure Media Services-konto från Azure StorSimple | Microsoft Docs"
description: "I den här artikeln finns en kort översikt över Azure StorSimple Data Manager. Artikeln innehåller också länkar till självstudier om hur du kan extrahera data från StorSimple och ladda upp dem som tillgångar till ett Azure Media Services-konto."
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
ms.openlocfilehash: 636d55c15aa383208ffb39d5224123831af962c9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-into-an-azure-media-services-account-from-azure-storsimple"></a><span data-ttu-id="705c6-104">Ladda upp filer till ett Azure Media Services-konto från Azure StorSimple</span><span class="sxs-lookup"><span data-stu-id="705c6-104">Upload files into an Azure Media Services account from Azure StorSimple</span></span>

<span data-ttu-id="705c6-105">I den här artikeln finns en kort översikt över Azure StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="705c6-105">This article gives a brief overview of Azure StorSimple Data Manager.</span></span> <span data-ttu-id="705c6-106">Artikeln innehåller också länkar till självstudier om hur du kan extrahera data från StorSimple och ladda upp dem som tillgångar till ett Azure Media Services-konto (AMS).</span><span class="sxs-lookup"><span data-stu-id="705c6-106">The article also links to tutorials that show you how to extract data from StorSimple and upload this data as assets to an Azure Media Services (AMS) account.</span></span>

> 
> [!NOTE]
> <span data-ttu-id="705c6-107">Azure StorSimple Data Manager finns för närvarande som privat förhandsutgåva.</span><span class="sxs-lookup"><span data-stu-id="705c6-107">Azure StorSimple Data Manager is currently in private preview.</span></span> 
> 

## <a name="overview"></a><span data-ttu-id="705c6-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="705c6-108">Overview</span></span>

<span data-ttu-id="705c6-109">I Media Services överför du dina digitala filer till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="705c6-109">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="705c6-110">Tillgång kan innehålla video, ljud, bilder, miniatyrsamlingar, textspår och filer med dold textning (samt metadata om dessa filer.) När filerna har överförts lagras innehållet på ett säkert sätt i molnet för ytterligare bearbetning och strömning.</span><span class="sxs-lookup"><span data-stu-id="705c6-110">The Asset  can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and the metadata about these files.) Once the files are uploaded, your content is stored securely in the cloud for further processing and streaming.</span></span>

<span data-ttu-id="705c6-111">[Azure StorSimple](https://docs.microsoft.com/azure/storsimple/) använder lagringsutrymmet i molnet som ett tillägg till den lokala lösningen och fördelar automatiskt data mellan det lokala lagringsutrymmet och lagringsutrymmet i molnet.</span><span class="sxs-lookup"><span data-stu-id="705c6-111">[Azure StorSimple](https://docs.microsoft.com/azure/storsimple/) uses cloud storage as an extension of the on-premises solution and automatically tiers data across the on-premises storage and cloud storage.</span></span> <span data-ttu-id="705c6-112">StorSimple-enheten deduplicerar och komprimerar data innan du skickar dem till molnet, vilket gör det mycket effektivt för att skicka stora filer till molnet.</span><span class="sxs-lookup"><span data-stu-id="705c6-112">The StorSimple device dedupes and compresses your data before sending it to the cloud making it very efficient for sending large files to the cloud.</span></span> <span data-ttu-id="705c6-113">Tjänsten [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md) innehåller API: er som gör det möjligt att extrahera data från StorSimple och presentera dem som AMS-tillgångar.</span><span class="sxs-lookup"><span data-stu-id="705c6-113">The [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md) service provides APIs that enable you to extract data from StorSimple and present it as AMS assets.</span></span>

## <a name="get-started"></a><span data-ttu-id="705c6-114">Kom igång</span><span class="sxs-lookup"><span data-stu-id="705c6-114">Get started</span></span>

1. <span data-ttu-id="705c6-115">[Skapa ett Media Services-konto](media-services-portal-create-account.md) som du vill överföra tillgångarna till.</span><span class="sxs-lookup"><span data-stu-id="705c6-115">[Create a Media Services account](media-services-portal-create-account.md) into which you want to transfer the assets.</span></span>
2. <span data-ttu-id="705c6-116">Registrera dig för förhandsutgåvan av Data Manager på det sätt som beskrivs i artikeln [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="705c6-116">Sign up for Data Manager preview, as described in the [StorSimple Data Manager](../storsimple/storsimple-data-manager-overview.md) article.</span></span>
3. <span data-ttu-id="705c6-117">Skapa ett StorSimple Data Manager-konto.</span><span class="sxs-lookup"><span data-stu-id="705c6-117">Create a StorSimple Data Manager account.</span></span>
4. <span data-ttu-id="705c6-118">Skapa ett omvandlingsjobb som när det körs hämtar data från en StorSimple-enhet och överför dem till ett AMS-konto som tillgångar.</span><span class="sxs-lookup"><span data-stu-id="705c6-118">Create a data transformation job that when runs, extracts data from a StorSimple device and transfers it into an AMS account as assets.</span></span> 

    <span data-ttu-id="705c6-119">När jobbet börjar köras skapas en lagringskö.</span><span class="sxs-lookup"><span data-stu-id="705c6-119">When the job starts running, a storage queue is created.</span></span> <span data-ttu-id="705c6-120">Den här kön fylls med meddelanden om transformerade blobbar i takt med att de är klara.</span><span class="sxs-lookup"><span data-stu-id="705c6-120">This queue is populated with messages about transformed blobs as they are ready.</span></span> <span data-ttu-id="705c6-121">Namnet på den här kön är detsamma som namnet på jobbdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="705c6-121">The name of this queue is the same as the name of the job definition.</span></span> <span data-ttu-id="705c6-122">Du kan använda den här kön för att avgöra när tillgången är klar och anropa önskad Media Services-åtgärd som ska köras på den.</span><span class="sxs-lookup"><span data-stu-id="705c6-122">You can use this queue to determine when as asset is ready and call your desired Media Services operation to run on it.</span></span> <span data-ttu-id="705c6-123">Du kan till exempel använda den här kön för att utlösa en Azure-funktion som innehåller den nödvändiga Media Services-koden.</span><span class="sxs-lookup"><span data-stu-id="705c6-123">For example, you can use this queue to trigger an Azure Function that has the necessary Media Services code in it.</span></span>

## <a name="see-also"></a><span data-ttu-id="705c6-124">Se även</span><span class="sxs-lookup"><span data-stu-id="705c6-124">See also</span></span>

[<span data-ttu-id="705c6-125">Använd .Net SDK för att utlösa jobb i Data Manager</span><span class="sxs-lookup"><span data-stu-id="705c6-125">Use the .Net SDK to trigger jobs in the Data Manager</span></span>](../storsimple/storsimple-data-manager-dotnet-jobs.md)

## <a name="media-services-learning-paths"></a><span data-ttu-id="705c6-126">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="705c6-126">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="705c6-127">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="705c6-127">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="705c6-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="705c6-128">Next steps</span></span>

<span data-ttu-id="705c6-129">Du kan nu koda överförda tillgångar.</span><span class="sxs-lookup"><span data-stu-id="705c6-129">You can now encode your uploaded assets.</span></span> <span data-ttu-id="705c6-130">Mer information finns i [Koda tillgångar](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="705c6-130">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>
