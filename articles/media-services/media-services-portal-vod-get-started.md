---
title: "aaaGet igång med att leverera VoD med hello Azure-portalen | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello stegen för att implementera en grundläggande Video-on-Demand (VoD) innehållsleverans tjänst med Azure Media Services (AMS)-program med hjälp av hello Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 6c98fcfa-39e6-43a5-83a5-d4954788f8a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 5c1c1b1f74ec1f1301120fe8e5a5ae183fe0338f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-hello-azure-portal"></a>Komma igång med att leverera innehåll på begäran med hjälp av hello Azure-portalen
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Den här självstudiekursen vägleder dig genom hello stegen för att implementera en grundläggande Video-on-Demand (VoD) innehållsleverans tjänst med Azure Media Services (AMS)-program med hjälp av hello Azure-portalen.

## <a name="prerequisites"></a>Krav
hello följande är obligatoriska toocomplete hello kursen:

* Ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/). 
* Ett Media Services-konto. toocreate Media Services-konto finns [hur tooCreate Media Services-konto](media-services-portal-create-account.md).

Den här självstudiekursen innehåller hello följande uppgifter:

1. Starta slutpunkt för direktuppspelning.
2. Överföra en videofil.
3. Koda hello källfilen till en uppsättning MP4-filer med anpassningsbar bithastighet.
4. Publicera hello tillgången och get strömning och progressiv nedladdning URL: er.  
5. Spela upp ditt innehåll.

## <a name="start-streaming-endpoints"></a>Starta slutpunkter för direktuppspelning 

När du arbetar med Azure Media Services är en av hello vanligaste scenarierna att leverera video via strömning med anpassningsbar bithastighet. Media Services tillhandahåller en dynamisk paketering som gör att du toodeliver dina MP4-kodade innehåll i strömningsformat som stöds av Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, utan att behöva toostore tillsammans med anpassad bithastighet versioner av var och en av dessa strömningsformat.

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd. 

toostart Hej strömmande slutpunkten, hello följande:

1. Logga in på hello [Azure-portalen](https://portal.azure.com/).
2. Streaming slutpunkter på hello inställningar i fönstret. 
3. Klicka på hello standard strömmande slutpunkten. 

    hello visas standard information om den STRÖMNINGSSLUTPUNKT.

4. Klicka på hello Start-ikonen.
5. Klicka på hello spara knappen toosave ändringarna.

## <a name="upload-files"></a>Överföra filer
toostream videor med Azure Media Services behöver du tooupload hello källvideorna, koda dem till flera olika bithastigheter och publicera hello resultat. hello första steget beskrivs i det här avsnittet. 

1. I hello **inställningen** -fönstret klickar du på **tillgångar**.
   
    ![Överföra filer](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. Klicka på hello **överför** knappen.
   
    Hej **överför en videotillgång** visas.
   
   > [!NOTE]
   > Det finns inga filstorleksbegränsningar.
   > 
   > 
3. Bläddra toohello önskade videon på datorn, markerar du den och tryck på OK.  
   
    hello överföringen startar och du kan se hello förloppet under filnamnet hello.  

När hello överföringen har slutförts visas hello nya tillgången i hello **tillgångar** fönster. 

## <a name="encode-assets"></a>Koda tillgångar

När du arbetar med Azure Media Services är ett av hello vanligaste scenarierna att leverera med anpassningsbar bithastighet strömmande tooyour klienter. Media Services stöder följande strömningstekniker med anpassningsbar bithastighet hello: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH. tooprepare videor för anpassningsbar bithastighet strömning måste tooencode källfilerna video i flera bithastigheter filer. Du bör använda hello **Media Encoder Standard** kodare tooencode videor.  

Media Services tillhandahåller också dynamisk paketering som gör att du toodeliver din multibithastighet MP4s i hello följande strömningsformat: MPEG DASH, HLS, Smooth Streaming, utan att behöva toorepackage till dessa strömningsformat. Med dynamisk paketering behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services bygger och fungerar hello lämplig respons baserat på begäranden från en klient.

tootake nytta av dynamisk paketering behöver du behöver tooencode källfilen till en uppsättning med flera bithastigheter MP4-filer (hello kodningsstegen senare i det här avsnittet).

### <a name="toouse-hello-portal-tooencode"></a>toouse hello portal tooencode
Det här avsnittet beskrivs hello åtgärder du kan vidta tooencode ditt innehåll med Media Encoder Standard.

1. I hello **inställningar** väljer **tillgångar**.  
2. I hello **tillgångar** fönster, Välj hello tillgång som du vill att tooencode.
3. Tryck på hello **koda** knappen.
4. I hello **koda en tillgång** fönster, Välj hello ”Media Encoder Standard” processor och en förinställning. Information om förinställningar finns i [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) (autogenerera en bithastighetsstege) och [Task Presets for MES](media-services-mes-presets-overview.md) (Uppgiftsförinställningar för MES). Om du planerar toocontrol vilka kodning förinställda används, Kom ihåg: det är viktigt tooselect hello förinställning som är mest lämpliga för din indatavideo. Till exempel om du vet att din indatavideo har en upplösning på 1 920 x 1 080 bildpunkter, du kan använda hello ”H264 Multibithastighet 1080p” förinställda. Om du har en video med låg upplösning (640 x 360) bör du inte använda förinställningen ”H264 multibithastighet 1080p”.
   
   Har en möjlighet att redigera hello av hello utdatatillgången och hello namn för hello jobbet för enklare hantering.
   
   ![Koda tillgångar](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Tryck på **Skapa**.

### <a name="monitor-encoding-job-progress"></a>Övervaka förloppet för kodningsjobb
toomonitor hello fortskrider hello kodning jobb, klicka på **inställningar** (överst hello på hello sidan) och välj sedan **jobb**.

![Jobb](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Publicera innehåll
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

> [!NOTE]
> Om du använde hello portal toocreate lokaliserare före mars 2015, skapades lokaliserare med ett utgångsdatum för två år.  
> 
> 

tooupdate ett förfallodatum för en lokaliserare, Använd [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) eller [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API: er. När du uppdaterar en SAS-lokaliserare hello utgångsdatum ändras hello URL: en.

### <a name="toouse-hello-portal-toopublish-an-asset"></a>toouse hello portal toopublish en tillgång
toouse hello portal toopublish en tillgång, hello följande:

1. Välj **Inställningar** > **Tillgångar**.
2. Välj hello tillgång som du vill toopublish.
3. Klicka på hello **publicera** knappen.
4. Välj typen av hello-positionerare.
5. Tryck på **Lägg till**.
   
    ![Publicera](./media/media-services-portal-vod-get-started/media-services-publish1.png)

hello URL läggs toohello lista över **publicerade URL: er**.

## <a name="play-content-from-hello-portal"></a>Spela upp innehåll från hello-portalen
hello Azure-portalen har en innehållsspelare som du kan använda tootest videon.

Hello önskad video och klicka sedan på hello **spela upp** knappen.

![Publicera](./media/media-services-portal-vod-get-started/media-services-play.png)

Vissa förutsättningar gäller:

* toobegin direktuppspelning, starta körs hello **standard** strömmande slutpunkten.
* Kontrollera att hello videon har publicerats.
* Detta **Media player** spelar upp från hello standard strömmande slutpunkten. Om du vill tooplay från ett standardvärde strömmande slutpunkten, klicka på toocopy hello URL och Använd en annan spelare. Till exempel [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

## <a name="next-steps"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

