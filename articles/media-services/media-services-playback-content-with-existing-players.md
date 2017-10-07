---
title: "aaaUse befintliga spelare tooplayback ditt innehåll - Azure | Microsoft Docs"
description: "Det här avsnittet innehåller befintliga spelare som du kan använda tooplayback ditt innehåll."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: juliako
ms.openlocfilehash: 54817345a19a9d3b18f1e7b352c3342043a569b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="playing-your-content-with-existing-players"></a>Spela upp ditt innehåll med befintliga spelare
Azure Media Services stöder många populära strömmande format, till exempel Smooth Streaming HTTP Live Streaming och MPEG-Dash. Det här avsnittet hänvisar tooexisting spelare som du kan använda tootest din dataströmmar.

### <a name="hello-azure-portal-media-services-content-player"></a>hello Azure-portalen Media Services player-innehåll
Hej **Azure** portalen har en innehållsspelare som du kan använda tootest videon.

Klicka på hello önskad video (se till att det var [publicerade](media-services-portal-publish.md)) och klicka på hello **spela upp** knappen längst ned hello hello-portalen.

Vissa förutsättningar gäller:

* Hej **MEDIA SERVICES-INNEHÅLLSSPELAREN** spelar upp från hello standard strömmande slutpunkten. Använd en annan spelare om du vill tooplay från en strömningsslutpunkt som inte är standard. Till exempel [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a>Azure Media Player
Använd [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tooplayback ditt innehåll (rensa eller skyddade) i något av följande format hello:

* Smooth Streaming
* MPEG DASH
* HLS
* Progressiv MP4

### <a name="flash-player"></a>Flash Player
#### <a name="aes-encrypted-with-token"></a>AES-kryptering med Token
[http://aestoken.azurewebsites.NET](http://aestoken.azurewebsites.net)

### <a name="silverlight-players"></a>Silverlight spelare
#### <a name="monitoring"></a>Övervakning
[http://SMF.cloudapp.NET/HealthMonitor](http://smf.cloudapp.net/healthmonitor)

#### <a name="playready-with-token"></a>PlayReady med Token
[http://sltoken.azurewebsites.NET](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>STRECK spelare
[http://dashplayer.azurewebsites.NET](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

### <a name="other"></a>Annat
tootest HLS webbadresser som du kan också använda:

* **Safari** på en iOS-enhet eller
* **3ivx HLS Player** i Windows.

## <a name="developing-video-players"></a>Utveckla videospelare
Information om hur toodevelop egna spelare, se [utveckla videospelare](media-services-develop-video-players.md)

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
