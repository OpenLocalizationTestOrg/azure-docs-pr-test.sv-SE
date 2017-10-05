---
title: " Överför filer till ett Media Services-konto med Azure-portalen | Microsoft Docs"
description: "Den här kursen vägleder dig igenom steg som överför filer till ett Media Services-konto med Azure-portalen"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 3a1dd7470f940da839687478b636464d930d8ab7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-into-a-media-services-account-using-the-azure-portal"></a><span data-ttu-id="aef5b-103">Överför filer till ett Media Services-konto med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="aef5b-103">Upload files into a Media Services account using the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="aef5b-104">Portal</span><span class="sxs-lookup"><span data-stu-id="aef5b-104">Portal</span></span>](media-services-portal-upload-files.md)
> * [<span data-ttu-id="aef5b-105">.NET</span><span class="sxs-lookup"><span data-stu-id="aef5b-105">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="aef5b-106">REST</span><span class="sxs-lookup"><span data-stu-id="aef5b-106">REST</span></span>](media-services-rest-upload-files.md)
> 
> [!NOTE]
> <span data-ttu-id="aef5b-107">Du behöver ett Azure-konto för att slutföra den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="aef5b-107">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="aef5b-108">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="aef5b-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 


<span data-ttu-id="aef5b-109">I Media Services överför du dina digitala filer till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="aef5b-109">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="aef5b-110">Tillgång kan innehålla video, ljud, bilder, miniatyrsamlingar, textspår och filer med dold textning (samt metadata om dessa filer.) När filerna har överförts lagras innehållet på ett säkert sätt i molnet för ytterligare bearbetning och strömning.</span><span class="sxs-lookup"><span data-stu-id="aef5b-110">The Asset  can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and the metadata about these files.) Once the files are uploaded, your content is stored securely in the cloud for further processing and streaming.</span></span>


## <a name="upload-files"></a><span data-ttu-id="aef5b-111">Överföra filer</span><span class="sxs-lookup"><span data-stu-id="aef5b-111">Upload files</span></span>

>[!NOTE]
><span data-ttu-id="aef5b-112">Det finns en gräns för maximal filstorlek för bearbetning i Media Services.</span><span class="sxs-lookup"><span data-stu-id="aef5b-112">There is a limit to the maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="aef5b-113">Information om filstorleksbegränsningen finns i [det här](media-services-quotas-and-limitations.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="aef5b-113">Please see [this](media-services-quotas-and-limitations.md) topic for details about the file size limitation.</span></span>
>

1. <span data-ttu-id="aef5b-114">Välj ditt Azure Media Services-konto i [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="aef5b-114">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="aef5b-115">I bladet **Inställningar** klickar du på **Tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="aef5b-115">On the **Settings** blade, click **Assets**.</span></span>
   
    ![Överföra filer](./media/media-services-portal-vod-get-started/media-services-upload.png)
3. <span data-ttu-id="aef5b-117">Klicka på knappen **Överför**.</span><span class="sxs-lookup"><span data-stu-id="aef5b-117">Click the **Upload** button.</span></span>
   
    <span data-ttu-id="aef5b-118">Fönstret **Överför en videotillgång** visas.</span><span class="sxs-lookup"><span data-stu-id="aef5b-118">The **Upload a video asset** window appears.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="aef5b-119">Det finns inga filstorleksbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="aef5b-119">There is no file size limitation.</span></span>
   > 
   > 
4. <span data-ttu-id="aef5b-120">Bläddra till den önskade videon på datorn, markera den och tryck på OK.</span><span class="sxs-lookup"><span data-stu-id="aef5b-120">Browse to the desired video on your computer, select it, and hit OK.</span></span>  
   
    <span data-ttu-id="aef5b-121">Överföringen startar och du kan följa förloppet under filnamnet.</span><span class="sxs-lookup"><span data-stu-id="aef5b-121">The upload starts and you can see the progress under the file name.</span></span>  

<span data-ttu-id="aef5b-122">När överföringen är klar visas den nya tillgången i listan **Tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="aef5b-122">Once the upload completes, you will see the new asset listed in the **Assets** window.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="aef5b-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aef5b-123">Next steps</span></span>
<span data-ttu-id="aef5b-124">Du kan nu koda överförda tillgångar.</span><span class="sxs-lookup"><span data-stu-id="aef5b-124">You can now encode your uploaded assets.</span></span> <span data-ttu-id="aef5b-125">Mer information finns i [Koda tillgångar](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="aef5b-125">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="aef5b-126">Du kan också använda Azure Functions för att utlösa ett kodningsjobb baserat på en fil som skickas till den konfigurerade behållaren.</span><span class="sxs-lookup"><span data-stu-id="aef5b-126">You can also use Azure Functions to trigger an encoding job based on a file arriving in the configured container.</span></span> <span data-ttu-id="aef5b-127">Mer information finns i [det här exemplet](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="aef5b-127">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="aef5b-128">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="aef5b-128">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="aef5b-129">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="aef5b-129">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

