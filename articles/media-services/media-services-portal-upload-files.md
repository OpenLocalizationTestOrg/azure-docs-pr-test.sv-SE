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
# <a name="upload-files-into-a-media-services-account-using-hello-azure-portal"></a>Överföra filer till ett Media Services-konto med hello Azure-portalen
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> 
> [!NOTE]
> toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 


I Media Services överför du dina digitala filer till en tillgång. hello tillgång kan innehålla video, ljud, bilder, miniatyrsamlingar, text spår och textning filer (och hello metadata om dessa filer.) När hello filerna har överförts lagras innehållet på ett säkert sätt i hello molnet för ytterligare bearbetning och strömning.


## <a name="upload-files"></a>Överföra filer

>[!NOTE]
>Det finns en gräns toohello maximal filstorlek som stöds för bearbetning i Media Services. Se [detta](media-services-quotas-and-limitations.md) avsnittet för information om hello filstorleksbegränsningar.
>

1. I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.
2. På hello **inställningar** bladet, klickar du på **tillgångar**.
   
    ![Överföra filer](./media/media-services-portal-vod-get-started/media-services-upload.png)
3. Klicka på hello **överför** knappen.
   
    Hej **överför en videotillgång** visas.
   
   > [!NOTE]
   > Det finns inga filstorleksbegränsningar.
   > 
   > 
4. Bläddra toohello önskade videon på datorn, markerar du den och tryck på OK.  
   
    hello överföringen startar och du kan se hello förloppet under filnamnet hello.  

När hello överföringen har slutförts visas hello nya tillgången i hello **tillgångar** fönster. 

## <a name="next-steps"></a>Nästa steg
Du kan nu koda överförda tillgångar. Mer information finns i [Koda tillgångar](media-services-portal-encode.md).

Du kan också använda Azure Functions tootrigger ett kodningsjobb baserat på en fil som inkommer på hello konfigurerats behållare. Mer information finns i [det här exemplet](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

