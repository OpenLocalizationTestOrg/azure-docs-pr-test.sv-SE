---
title: "aaa ”Överför filer till ett Media Services-konto med hello Azure-portalen | Microsoft Docs ”"
description: "Den här självstudiekursen vägleder dig genom stegen hello laddar upp filer till ett Media Services-konto med hjälp av hello Azure-portalen"
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
ms.openlocfilehash: 4ce1e133c72854532735ba7c72a43c92a75bc240
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-hello-azure-portal"></a><span data-ttu-id="1a33c-103">Överföra filer till ett Media Services-konto med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1a33c-103">Upload files into a Media Services account using hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1a33c-104">Portal</span><span class="sxs-lookup"><span data-stu-id="1a33c-104">Portal</span></span>](media-services-portal-upload-files.md)
> * [<span data-ttu-id="1a33c-105">.NET</span><span class="sxs-lookup"><span data-stu-id="1a33c-105">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="1a33c-106">REST</span><span class="sxs-lookup"><span data-stu-id="1a33c-106">REST</span></span>](media-services-rest-upload-files.md)
> 
> [!NOTE]
> <span data-ttu-id="1a33c-107">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="1a33c-107">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="1a33c-108">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1a33c-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 


<span data-ttu-id="1a33c-109">I Media Services överför du dina digitala filer till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="1a33c-109">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="1a33c-110">hello tillgång kan innehålla video, ljud, bilder, miniatyrsamlingar, text spår och textning filer (och hello metadata om dessa filer.) När hello filerna har överförts lagras innehållet på ett säkert sätt i hello molnet för ytterligare bearbetning och strömning.</span><span class="sxs-lookup"><span data-stu-id="1a33c-110">hello Asset  can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.) Once hello files are uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span>


## <a name="upload-files"></a><span data-ttu-id="1a33c-111">Överföra filer</span><span class="sxs-lookup"><span data-stu-id="1a33c-111">Upload files</span></span>

>[!NOTE]
><span data-ttu-id="1a33c-112">Det finns en gräns toohello maximal filstorlek som stöds för bearbetning i Media Services.</span><span class="sxs-lookup"><span data-stu-id="1a33c-112">There is a limit toohello maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="1a33c-113">Se [detta](media-services-quotas-and-limitations.md) avsnittet för information om hello filstorleksbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="1a33c-113">Please see [this](media-services-quotas-and-limitations.md) topic for details about hello file size limitation.</span></span>
>

1. <span data-ttu-id="1a33c-114">I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="1a33c-114">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="1a33c-115">På hello **inställningar** bladet, klickar du på **tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="1a33c-115">On hello **Settings** blade, click **Assets**.</span></span>
   
    ![Överföra filer](./media/media-services-portal-vod-get-started/media-services-upload.png)
3. <span data-ttu-id="1a33c-117">Klicka på hello **överför** knappen.</span><span class="sxs-lookup"><span data-stu-id="1a33c-117">Click hello **Upload** button.</span></span>
   
    <span data-ttu-id="1a33c-118">Hej **överför en videotillgång** visas.</span><span class="sxs-lookup"><span data-stu-id="1a33c-118">hello **Upload a video asset** window appears.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1a33c-119">Det finns inga filstorleksbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="1a33c-119">There is no file size limitation.</span></span>
   > 
   > 
4. <span data-ttu-id="1a33c-120">Bläddra toohello önskade videon på datorn, markerar du den och tryck på OK.</span><span class="sxs-lookup"><span data-stu-id="1a33c-120">Browse toohello desired video on your computer, select it, and hit OK.</span></span>  
   
    <span data-ttu-id="1a33c-121">hello överföringen startar och du kan se hello förloppet under filnamnet hello.</span><span class="sxs-lookup"><span data-stu-id="1a33c-121">hello upload starts and you can see hello progress under hello file name.</span></span>  

<span data-ttu-id="1a33c-122">När hello överföringen har slutförts visas hello nya tillgången i hello **tillgångar** fönster.</span><span class="sxs-lookup"><span data-stu-id="1a33c-122">Once hello upload completes, you will see hello new asset listed in hello **Assets** window.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="1a33c-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a33c-123">Next steps</span></span>
<span data-ttu-id="1a33c-124">Du kan nu koda överförda tillgångar.</span><span class="sxs-lookup"><span data-stu-id="1a33c-124">You can now encode your uploaded assets.</span></span> <span data-ttu-id="1a33c-125">Mer information finns i [Koda tillgångar](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="1a33c-125">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="1a33c-126">Du kan också använda Azure Functions tootrigger ett kodningsjobb baserat på en fil som inkommer på hello konfigurerats behållare.</span><span class="sxs-lookup"><span data-stu-id="1a33c-126">You can also use Azure Functions tootrigger an encoding job based on a file arriving in hello configured container.</span></span> <span data-ttu-id="1a33c-127">Mer information finns i [det här exemplet](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="1a33c-127">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="1a33c-128">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="1a33c-128">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="1a33c-129">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="1a33c-129">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

