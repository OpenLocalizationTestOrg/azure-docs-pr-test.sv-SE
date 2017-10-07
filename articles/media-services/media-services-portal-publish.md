---
title: "aaa ”publicera innehåll med hello Azure-portalen | Microsoft Docs ”"
description: "Den här självstudiekursen vägleder dig genom stegen för hello publicera ditt innehåll med hello Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 92c364eb-5a5f-4f4e-8816-b162c031bb40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: a7a3867a6939b4b9da883176c6cc20c99d6c54e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-content-with-hello-azure-portal"></a>Publicera innehåll med hello Azure-portalen
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-publish.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [REST](media-services-rest-deliver-streaming-content.md)
> 
> 

## <a name="overview"></a>Översikt
> [!NOTE]
> toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

tooprovide din användare en URL som kan använda toostream eller hämta ditt innehåll du först måste för ”publicera” din tillgång genom att skapa en positionerare. Positionerare ger åtkomst toofiles i hello tillgången. Media Services stöder två typer av lokaliserare: 

* Streaming (OnDemandOrigin)-positionerare som används för anpassad strömning (till exempel toostream MPEG DASH, HLS eller Smooth Streaming). toocreate en strömningslokaliserare måste din tillgång innehålla en .ism-fil. 
* Progressiva SAS-lokaliserare, som används för leverans av video via progressiv hämtning.

En strömmande URL har följande format hello och du kan använda den tooplay Smooth Streaming-tillgångar.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Lägg till toobuild en HLS-strömnings-URL (format = m3u8 aapl) toohello URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

toobuild en MPEG DASH-strömnings-URL, Lägg till (format = mpd-tid-csf) toohello URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

En SAS-URL har följande format hello.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Mer information finns i [leverera Innehållsöversikt](media-services-deliver-content-overview.md).

> [!NOTE]
> Om du använde hello portal toocreate lokaliserare före mars 2015, skapades lokaliserare med ett utgångsdatum två år.  
> 
> 

tooupdate ett förfallodatum för en lokaliserare, Använd [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) eller [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API: er. Observera att när du uppdaterar en SAS-lokaliserare hello utgångsdatum ändras hello-URL.

### <a name="toouse-hello-portal-toopublish-an-asset"></a>toouse hello portal toopublish en tillgång
toouse hello portal toopublish en tillgång, hello följande:

1. I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.
2. Välj **Inställningar** > **Tillgångar**.
3. Välj hello tillgång som du vill toopublish.
4. Klicka på hello **publicera** knappen.
5. Välj typen av hello-positionerare.
6. Tryck på **Lägg till**.
   
    ![Publicera](./media/media-services-portal-vod-get-started/media-services-publish1.png)

hello URL läggs toohello lista över **publicerade URL: er**.

## <a name="play-content-from-hello-portal"></a>Spela upp innehåll från hello-portalen
hello Azure-portalen har en innehållsspelare som du kan använda tootest videon.

Hello önskad video och klicka sedan på hello **spela upp** knappen.

![Publicera](./media/media-services-portal-vod-get-started/media-services-play.png)

Vissa förutsättningar gäller:

* Kontrollera att hello videon har publicerats.
* Detta **Media player** spelar upp från hello standard strömmande slutpunkten. Om du vill tooplay från ett standardvärde strömmande slutpunkten, klicka på toocopy hello URL och Använd en annan spelare. Till exempel [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
* Hej strömningsslutpunkt från vilken du strömning måste köras.  

## <a name="next-steps"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

