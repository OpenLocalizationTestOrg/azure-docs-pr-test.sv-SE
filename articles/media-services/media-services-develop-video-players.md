---
title: aaaDevelop videospelarprogram
description: "hello avsnittet tooPlayer ramverk för länkar och plugin-program som du kan använda toodevelop egna klientprogram som kan använda strömmande media från Media Services."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 55e419fc-4c39-4902-9c62-f41cfcd86c6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: juliako
ms.openlocfilehash: a66daa4f006a1f05271cc9ed6a02ea7ee460321d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-video-player-applications"></a>Utveckla videospelarprogram
## <a name="overview"></a>Översikt
Azure Media Services innehåller hello verktyg du behöver toocreate omfattande dynamiska klientprogram för uppspelare för de flesta plattformar, bland annat: iOS-enheter, Android-enheter, Windows, Windows Phone, Xbox och fristående rutorna. Det här avsnittet innehåller också länkar tooSDKs och Spelarramverk som du kan använda toodevelop egna klientprogram som kan använda strömmande media från Azure Media Services.

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd. 
 
## <a name="azure-media-player"></a>Azure Media Player
[Azure Media Player](http://aka.ms/ampinfo) en web videospelare bygger tooplay tillbaka medieinnehåll från Microsoft Azure Media Services på en mängd olika webbläsare och enheter. Azure Media Player använder branschstandarder som HTML5 Media källa tillägg (mus) och krypterade Media tillägg (EME) tooprovide en utökade anpassningsbar strömning upplevelse. När dessa normer inte är tillgängliga på en enhet eller i en webbläsare, använder Azure Media Player Flash och Silverlight som reserv teknik. Oavsett hello uppspelning teknik används för har utvecklare en enhetlig JavaScript-gränssnittet tooaccess API: er. Detta ger innehåll som hanteras av Azure Media Services toobe spelas upp över ett brett utbud av enheter och webbläsare utan någon extra arbete.

Microsoft Azure Media Services kan för innehåll toobe hanteras med DASH, Smooth Streaming och HLS-strömnings format tooplay tillbaka innehåll. Azure Media Player tar hänsyn till dessa olika format och automatiskt spelar hello bästa länken baserat på hello plattform/webbläsarfunktioner. Microsoft Azure Media Services kan även användas för dynamisk kryptering av tillgångar med PlayReady-kryptering eller AES-128-bitars kryptering för intervall. Azure Media Player kan för dekryptering av PlayReady och AES-128-bitars krypterat innehåll när korrekt konfigurerad. 

Mer information:

* [Azure Media Player](http://aka.ms/ampinfo)
* [Dokumentation för Azure Media Player](http://aka.ms/ampdocs) 
* [Azure Media Player komma igång blogg](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
* [Registrera dig toostay in toodate med hello senaste från Azure Media Player](http://aka.ms/ampsignup)
* [Lägga till nya funktioner som efterfrågas, idéer, feedback](http://aka.ms/ampuservoice) 

## <a name="other-tools-for-creating-player-applications"></a>Andra verktyg för att skapa Player-program
Du kan också använda någon av följande SDK hello:

* [Utjämna strömmande klient-SDK](http://www.iis.net/downloads/microsoft/smooth-streaming) 
* [Smooth Streaming Windows Store-App](media-services-build-smooth-streaming-apps.md)
* [Microsoft Media Platform: Player Framework](http://playerframework.codeplex.com/) 
* [HTML5 Player Framework-dokumentationen](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
* [Microsoft Smooth Streaming-plugin-programmet för OSMF](https://www.microsoft.com/download/details.aspx?id=36057) 
* [Licensiering Microsoft® Smooth Streaming klienten portera Kit](http://aka.ms/sspk) 
* [XBOX Video programutveckling](http://xbox.create.msdn.com/) 

## <a name="advertising"></a>Reklam
Azure Media Services tillhandahåller support för ad infogning via hello Windows Media-plattform: Spelarramverk. Spelarramverk med stöd för ad är tillgängliga för Windows 8, Silverlight, Windows Phone 8 och iOS-enheter. Varje player framework innehåller exempelkod som visar hur tooimplement ett player-program. Det finns tre olika typer av annonser som du kan infoga i media:

Linjär – fullständig ram annonser som pausa hello huvudsakliga video

Linjära – överlägget annonser som visas när hello huvudsakliga video spelas, vanligtvis en logotyp eller andra statisk avbildning som placerats i hello player

Medföljande – annonser som visas utanför hello player

Active Directory kan placeras när som helst hello huvudsakliga video tidslinje. Du måste ange hello player när tooplay hello ad och som ads tooplay. Detta görs med hjälp av en uppsättning XML-baserade standardfiler: Video Ad Service mall (VAST), Digital Video flera Ad spelningslista (VMAP), Media abstrakt ordningsföljd mall (MAST) och Digital Video Player Ad Interface Definition (VPAID). STORA filer ange vilka annonser toodisplay. VMAP filer ange när tooplay olika annonser och innehålla stora XML. MAST filer är ett annat sätt toosequence annonser som även kan innehålla stora XML. VPAID filer definierar ett gränssnitt mellan hello videospelare och hello ad eller ad-server. Mer information finns i [lägga till Ads](https://msdn.microsoft.com/library/dn387398.aspx).

Information om textning och Active Directory-stöd i direktsänd strömning videor finns [stöds textning och Ad infogning standarder](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Se även
[Bädda in MPEG-DASH-anpassad direktuppspelad video i ett HTML5-program med DASH.js](media-services-embed-mpeg-dash-in-html5.md)

[GitHub dash.js-databas](https://github.com/Dash-Industry-Forum/dash.js)

