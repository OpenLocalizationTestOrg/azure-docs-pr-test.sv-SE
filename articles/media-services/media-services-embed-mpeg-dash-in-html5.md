---
title: aaaEmbedding en MPEG-DASH anpassningsbar direktuppspelad Video i ett HTML5-program med DASH.js | Microsoft Docs
description: "Det här avsnittet visar hur tooembed en MPEG-DASH anpassningsbar direktuppspelad Video i ett HTML5-program med DASH.js."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 5aa0e7b6-f5c3-4cc1-aa33-ed16ea4780c2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: a73713d20f95262654532b94576ae9669d829354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>Bädda in en MPEG-DASH anpassningsbar direktuppspelad Video i ett HTML5-program med DASH.js
## <a name="overview"></a>Översikt
MPEG-DASH är en ISO-standarden för hello anpassningsbar strömning av video innehållet, vilket medför betydande fördelar för dem som vill toodeliver hög kvalitet, anpassningsbar direktuppspelad video utdata. Med MPEG-DASH hello video-ström kommer automatiskt att släppa tooa lägre definition när hello nätverket blir överbelastad. Detta minskar hello sannolikheten för hello viewer ser en ”pausas” video medan hello hämtas hello bredvid några sekunder tooplay (aka buffert). Eftersom överbelastning minskar hello videospelare i sin tur att returnera tooa högre kvalitet-dataström. Den här möjligheten tooadapt hello bandbredd som krävs också resulterar i en snabbare starttid för video. Innebär att hello några sekunder kan spelas upp i ett fast hämta lägre kvalitet segment och steg uppåt tooa högre kvalitet när tillräckligt innehåll som har buffrats.

Dash.js är en öppen källkod MPEG-DASH videospelare skriven i JavaScript. Syftet är tooprovide en robust, plattformsoberoende spelare som fritt kan återanvändas i program som kräver uppspelning av video. Det ger MPEG-DASH uppspelning i valfri webbläsare som stöder hello W3C Media källa tillägg (mus), dag att Chrome, Microsoft Edge och IE11 (sina avsiktshantering toosupport mus har angett i andra webbläsare). Mer information om DASH.js finns js dash.js hello GitHub-lagringsplatsen.

## <a name="creating-a-browser-based-streaming-video-player"></a>Skapa en webbläsarbaserad strömmande videospelare
toocreate en enkel webbsida som visar en videospelare med hello förväntades styr dessa en play, pausa, tillbakaspolning etc., behöver du:

1. Skapa en HTML-sida
2. Lägga till hello video tagg
3. Lägg till hello dash.js player
4. Initiera hello player
5. Lägga till vissa CSS-format
6. Visa hello resultat i en webbläsare som implementerar mus

Initiera hello player kan utföras i bara ett fåtal rader med JavaScript-kod. Med dash.js verkligen är det den enkla tooembed MPEG-DASH videon i webbläsarbaserade program.

## <a name="creating-hello-html-page"></a>Skapa hello HTML-sida
hello första steget är toocreate standard HTML-sida som innehåller hello **video** element, spara filen som basicPlayer.html som hello som följande exempel visar:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

## <a name="adding-hello-dashjs-player"></a>Att lägga till hello DASH.js Player
tooadd hello dash.js referens implementering toohello program behöver toograb hello dash.all.js filen från hello 1.0-versionen av dash.js projekt. Detta ska sparas i hello JavaScript-mappen för ditt program. Den här filen är en bekvämlighet som sammanställer alla hello nödvändiga dash.js kod i en enda fil. Om du har en titt runt hello dash.js databasen du kommer att hitta hello enskilda filer, testa koden och mycket mer, men om alla toodo är Använd dash.js sedan hello dash.all.js filen är vad du behöver.

tooadd hello dash.js player tooyour program, Lägg till ett skript toohello head taggavsnitt av basicPlayer.html:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Skapa sedan en funktion tooinitialize hello spelare när hello sidan läses in. Lägg till följande skript efter hello raden som du kan läsa in dash.all.js hello:

    <script>
    // setup hello video element and attach it toohello Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

Den här funktionen skapar först ett DashContext. Detta är att använda tooconfigure hello program för en specifik körningsmiljö. Den definierar hello klasser som hello beroende injection framework ska användas för att konstruera hello program från en teknisk synvinkel. I de flesta fall ska du använda Dash.di.DashContext.

Därefter instansiera hello primära av hello dash.js framework, Media Player. Den här klassen innehåller hello core metoder som behövs spela upp och pausa, hanterar hello relation med hello video element och hanterar också hello tolkning av hello Media Presentation beskrivning (MPD)-fil som beskriver hello video toobe spelas upp.

Hej startup() funktionen av hello MediaPlayer-klassen anropas tooensure som hello player är klar tooplay video. Den här funktionen innebär bland annat att alla nödvändiga hello-klasser (som definieras av hello kontexten) har lästs in. Du kan koppla hello video elementet tooit hello attachView() funktionen när hello player är klar. Detta aktiverar hello MediaPlayer tooinject hello video-ström till hello element och också styra uppspelning vid behov.

Skicka hello URL för hello MPD filen toohello Media Player så att den vet om hello video förväntas tooplay.hello setupVideo() funktionen skapade behöver toobe utförs när hello sidan har lästs in helt. Du kan göra detta med hjälp av hello VidStart av hello body-elementet. Ändra din <body> elementet så att:

    <body onload="setupVideo()">

Slutligen, Ställ hello storleken på hello video element med hjälp av CSS. Detta är särskilt viktigt i en anpassningsbar strömning miljö, eftersom hello video spelas hello storlek kan ändras när uppspelning toochanging nätverksförhållanden som anpassas efter dina behov. I den här enkel demo bara tvinga hello video elementet toobe 80% av hello tillgängliga webbläsarfönstret genom att lägga till hello efter CSS toohello head avsnittet av sidan hello:

    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

## <a name="playing-a-video"></a>Spela upp en Video
tooplay en video webbläsaren vid hello basicPlayback.html filen och klicka på play på hello videospelare visas.

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Se även
[Utveckla videospelarprogram](media-services-develop-video-players.md)

[GitHub dash.js-databas](https://github.com/Dash-Industry-Forum/dash.js) 

