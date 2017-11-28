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
# <a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a><span data-ttu-id="029c7-103">Bädda in en MPEG-DASH anpassningsbar direktuppspelad Video i ett HTML5-program med DASH.js</span><span class="sxs-lookup"><span data-stu-id="029c7-103">Embedding a MPEG-DASH Adaptive Streaming Video in an HTML5 Application with DASH.js</span></span>
## <a name="overview"></a><span data-ttu-id="029c7-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="029c7-104">Overview</span></span>
<span data-ttu-id="029c7-105">MPEG-DASH är en ISO-standarden för hello anpassningsbar strömning av video innehållet, vilket medför betydande fördelar för dem som vill toodeliver hög kvalitet, anpassningsbar direktuppspelad video utdata.</span><span class="sxs-lookup"><span data-stu-id="029c7-105">MPEG-DASH is an ISO standard for hello adaptive streaming of video content, which offers significant benefits for those who wish toodeliver high-quality, adaptive video streaming output.</span></span> <span data-ttu-id="029c7-106">Med MPEG-DASH hello video-ström kommer automatiskt att släppa tooa lägre definition när hello nätverket blir överbelastad.</span><span class="sxs-lookup"><span data-stu-id="029c7-106">With MPEG-DASH, hello video stream will automatically drop tooa lower definition when hello network becomes congested.</span></span> <span data-ttu-id="029c7-107">Detta minskar hello sannolikheten för hello viewer ser en ”pausas” video medan hello hämtas hello bredvid några sekunder tooplay (aka buffert).</span><span class="sxs-lookup"><span data-stu-id="029c7-107">This reduces hello likelihood of hello viewer seeing a "paused" video while hello player downloads hello next few seconds tooplay (aka buffering).</span></span> <span data-ttu-id="029c7-108">Eftersom överbelastning minskar hello videospelare i sin tur att returnera tooa högre kvalitet-dataström.</span><span class="sxs-lookup"><span data-stu-id="029c7-108">As network congestion reduces, hello video player will in turn return tooa higher quality stream.</span></span> <span data-ttu-id="029c7-109">Den här möjligheten tooadapt hello bandbredd som krävs också resulterar i en snabbare starttid för video.</span><span class="sxs-lookup"><span data-stu-id="029c7-109">This ability tooadapt hello bandwidth required also results in a faster start time for video.</span></span> <span data-ttu-id="029c7-110">Innebär att hello några sekunder kan spelas upp i ett fast hämta lägre kvalitet segment och steg uppåt tooa högre kvalitet när tillräckligt innehåll som har buffrats.</span><span class="sxs-lookup"><span data-stu-id="029c7-110">That means that hello first few seconds can be played in a fast-to-download lower quality segment and then step up tooa higher quality once sufficient content has been buffered.</span></span>

<span data-ttu-id="029c7-111">Dash.js är en öppen källkod MPEG-DASH videospelare skriven i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="029c7-111">Dash.js is an open source MPEG-DASH video player written in JavaScript.</span></span> <span data-ttu-id="029c7-112">Syftet är tooprovide en robust, plattformsoberoende spelare som fritt kan återanvändas i program som kräver uppspelning av video.</span><span class="sxs-lookup"><span data-stu-id="029c7-112">Its goal is tooprovide a robust, cross-platform player that can be freely reused in applications that require video playback.</span></span> <span data-ttu-id="029c7-113">Det ger MPEG-DASH uppspelning i valfri webbläsare som stöder hello W3C Media källa tillägg (mus), dag att Chrome, Microsoft Edge och IE11 (sina avsiktshantering toosupport mus har angett i andra webbläsare).</span><span class="sxs-lookup"><span data-stu-id="029c7-113">It provides MPEG-DASH playback in any browser that supports hello W3C Media Source Extensions (MSE), today that is Chrome, Microsoft Edge and IE11 (other browsers have indicated their intent toosupport MSE).</span></span> <span data-ttu-id="029c7-114">Mer information om DASH.js finns js dash.js hello GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="029c7-114">For more information about DASH.js, js see hello GitHub dash.js repository.</span></span>

## <a name="creating-a-browser-based-streaming-video-player"></a><span data-ttu-id="029c7-115">Skapa en webbläsarbaserad strömmande videospelare</span><span class="sxs-lookup"><span data-stu-id="029c7-115">Creating a browser-based streaming video player</span></span>
<span data-ttu-id="029c7-116">toocreate en enkel webbsida som visar en videospelare med hello förväntades styr dessa en play, pausa, tillbakaspolning etc., behöver du:</span><span class="sxs-lookup"><span data-stu-id="029c7-116">toocreate a simple web page that displays a video player with hello expected controls such a play, pause, rewind etc., you will need to:</span></span>

1. <span data-ttu-id="029c7-117">Skapa en HTML-sida</span><span class="sxs-lookup"><span data-stu-id="029c7-117">Create an HTML page</span></span>
2. <span data-ttu-id="029c7-118">Lägga till hello video tagg</span><span class="sxs-lookup"><span data-stu-id="029c7-118">Add hello video tag</span></span>
3. <span data-ttu-id="029c7-119">Lägg till hello dash.js player</span><span class="sxs-lookup"><span data-stu-id="029c7-119">Add hello dash.js player</span></span>
4. <span data-ttu-id="029c7-120">Initiera hello player</span><span class="sxs-lookup"><span data-stu-id="029c7-120">Initialize hello player</span></span>
5. <span data-ttu-id="029c7-121">Lägga till vissa CSS-format</span><span class="sxs-lookup"><span data-stu-id="029c7-121">Add some CSS style</span></span>
6. <span data-ttu-id="029c7-122">Visa hello resultat i en webbläsare som implementerar mus</span><span class="sxs-lookup"><span data-stu-id="029c7-122">View hello results in a browser that implements MSE</span></span>

<span data-ttu-id="029c7-123">Initiera hello player kan utföras i bara ett fåtal rader med JavaScript-kod.</span><span class="sxs-lookup"><span data-stu-id="029c7-123">Initializing hello player can be completed in just a handful of lines of JavaScript code.</span></span> <span data-ttu-id="029c7-124">Med dash.js verkligen är det den enkla tooembed MPEG-DASH videon i webbläsarbaserade program.</span><span class="sxs-lookup"><span data-stu-id="029c7-124">Using dash.js, it really is that simple tooembed MPEG-DASH video in your browser based applications.</span></span>

## <a name="creating-hello-html-page"></a><span data-ttu-id="029c7-125">Skapa hello HTML-sida</span><span class="sxs-lookup"><span data-stu-id="029c7-125">Creating hello HTML Page</span></span>
<span data-ttu-id="029c7-126">hello första steget är toocreate standard HTML-sida som innehåller hello **video** element, spara filen som basicPlayer.html som hello som följande exempel visar:</span><span class="sxs-lookup"><span data-stu-id="029c7-126">hello first step is toocreate a standard HTML page containing hello **video** element, save this file as basicPlayer.html, as hello following example illustrates:</span></span>

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

## <a name="adding-hello-dashjs-player"></a><span data-ttu-id="029c7-127">Att lägga till hello DASH.js Player</span><span class="sxs-lookup"><span data-stu-id="029c7-127">Adding hello DASH.js Player</span></span>
<span data-ttu-id="029c7-128">tooadd hello dash.js referens implementering toohello program behöver toograb hello dash.all.js filen från hello 1.0-versionen av dash.js projekt.</span><span class="sxs-lookup"><span data-stu-id="029c7-128">tooadd hello dash.js reference implementation toohello application, you’ll need toograb hello dash.all.js file from hello 1.0 release of dash.js project.</span></span> <span data-ttu-id="029c7-129">Detta ska sparas i hello JavaScript-mappen för ditt program.</span><span class="sxs-lookup"><span data-stu-id="029c7-129">This should be saved in hello JavaScript folder of your application.</span></span> <span data-ttu-id="029c7-130">Den här filen är en bekvämlighet som sammanställer alla hello nödvändiga dash.js kod i en enda fil.</span><span class="sxs-lookup"><span data-stu-id="029c7-130">This file is a convenience file that pulls together all hello necessary dash.js code into a single file.</span></span> <span data-ttu-id="029c7-131">Om du har en titt runt hello dash.js databasen du kommer att hitta hello enskilda filer, testa koden och mycket mer, men om alla toodo är Använd dash.js sedan hello dash.all.js filen är vad du behöver.</span><span class="sxs-lookup"><span data-stu-id="029c7-131">If you have a look around hello dash.js repository, you will find hello individual files, test code and much more, but if all you want toodo is use dash.js, then hello dash.all.js file is what you need.</span></span>

<span data-ttu-id="029c7-132">tooadd hello dash.js player tooyour program, Lägg till ett skript toohello head taggavsnitt av basicPlayer.html:</span><span class="sxs-lookup"><span data-stu-id="029c7-132">tooadd hello dash.js player tooyour applications, add a script tag toohello head section of basicPlayer.html:</span></span>

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


<span data-ttu-id="029c7-133">Skapa sedan en funktion tooinitialize hello spelare när hello sidan läses in.</span><span class="sxs-lookup"><span data-stu-id="029c7-133">Next, create a function tooinitialize hello player when hello page loads.</span></span> <span data-ttu-id="029c7-134">Lägg till följande skript efter hello raden som du kan läsa in dash.all.js hello:</span><span class="sxs-lookup"><span data-stu-id="029c7-134">Add hello following script after hello line in which you load dash.all.js:</span></span>

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

<span data-ttu-id="029c7-135">Den här funktionen skapar först ett DashContext.</span><span class="sxs-lookup"><span data-stu-id="029c7-135">This function first creates a DashContext.</span></span> <span data-ttu-id="029c7-136">Detta är att använda tooconfigure hello program för en specifik körningsmiljö.</span><span class="sxs-lookup"><span data-stu-id="029c7-136">This is used tooconfigure hello application for a specific runtime environment.</span></span> <span data-ttu-id="029c7-137">Den definierar hello klasser som hello beroende injection framework ska användas för att konstruera hello program från en teknisk synvinkel.</span><span class="sxs-lookup"><span data-stu-id="029c7-137">From a technical point of view, it defines hello classes that hello dependency injection framework should use when constructing hello application.</span></span> <span data-ttu-id="029c7-138">I de flesta fall ska du använda Dash.di.DashContext.</span><span class="sxs-lookup"><span data-stu-id="029c7-138">In most cases, you will use Dash.di.DashContext.</span></span>

<span data-ttu-id="029c7-139">Därefter instansiera hello primära av hello dash.js framework, Media Player.</span><span class="sxs-lookup"><span data-stu-id="029c7-139">Next, instantiate hello primary class of hello dash.js framework, MediaPlayer.</span></span> <span data-ttu-id="029c7-140">Den här klassen innehåller hello core metoder som behövs spela upp och pausa, hanterar hello relation med hello video element och hanterar också hello tolkning av hello Media Presentation beskrivning (MPD)-fil som beskriver hello video toobe spelas upp.</span><span class="sxs-lookup"><span data-stu-id="029c7-140">This class contains hello core methods needed such as play and pause, manages hello relationship with hello video element and also manages hello interpretation of hello Media Presentation Description (MPD) file which describes hello video toobe played.</span></span>

<span data-ttu-id="029c7-141">Hej startup() funktionen av hello MediaPlayer-klassen anropas tooensure som hello player är klar tooplay video.</span><span class="sxs-lookup"><span data-stu-id="029c7-141">hello startup() function of hello MediaPlayer class is called tooensure that hello player is ready tooplay video.</span></span> <span data-ttu-id="029c7-142">Den här funktionen innebär bland annat att alla nödvändiga hello-klasser (som definieras av hello kontexten) har lästs in.</span><span class="sxs-lookup"><span data-stu-id="029c7-142">Amongst other things this function ensures that all hello necessary classes (as defined by hello context) have been loaded.</span></span> <span data-ttu-id="029c7-143">Du kan koppla hello video elementet tooit hello attachView() funktionen när hello player är klar.</span><span class="sxs-lookup"><span data-stu-id="029c7-143">Once hello player is ready, you can attach hello video element tooit using hello attachView() function.</span></span> <span data-ttu-id="029c7-144">Detta aktiverar hello MediaPlayer tooinject hello video-ström till hello element och också styra uppspelning vid behov.</span><span class="sxs-lookup"><span data-stu-id="029c7-144">This enables hello MediaPlayer tooinject hello video stream into hello element and also control playback as necessary.</span></span>

<span data-ttu-id="029c7-145">Skicka hello URL för hello MPD filen toohello Media Player så att den vet om hello video förväntas tooplay.hello setupVideo() funktionen skapade behöver toobe utförs när hello sidan har lästs in helt.</span><span class="sxs-lookup"><span data-stu-id="029c7-145">Pass hello URL of hello MPD file toohello MediaPlayer so that it knows about hello video it is expected tooplay.hello setupVideo() function just created will need toobe executed once hello page has fully loaded.</span></span> <span data-ttu-id="029c7-146">Du kan göra detta med hjälp av hello VidStart av hello body-elementet.</span><span class="sxs-lookup"><span data-stu-id="029c7-146">Do this by using hello onload event of hello body element.</span></span> <span data-ttu-id="029c7-147">Ändra din <body> elementet så att:</span><span class="sxs-lookup"><span data-stu-id="029c7-147">Change your <body> element to:</span></span>

    <body onload="setupVideo()">

<span data-ttu-id="029c7-148">Slutligen, Ställ hello storleken på hello video element med hjälp av CSS.</span><span class="sxs-lookup"><span data-stu-id="029c7-148">Finally, set hello size of hello video element using CSS.</span></span> <span data-ttu-id="029c7-149">Detta är särskilt viktigt i en anpassningsbar strömning miljö, eftersom hello video spelas hello storlek kan ändras när uppspelning toochanging nätverksförhållanden som anpassas efter dina behov.</span><span class="sxs-lookup"><span data-stu-id="029c7-149">In an adaptive streaming environment, this is especially important because hello size of hello video being played may change as playback adapts toochanging network conditions.</span></span> <span data-ttu-id="029c7-150">I den här enkel demo bara tvinga hello video elementet toobe 80% av hello tillgängliga webbläsarfönstret genom att lägga till hello efter CSS toohello head avsnittet av sidan hello:</span><span class="sxs-lookup"><span data-stu-id="029c7-150">In this simple demo simply force hello video element toobe 80% of hello available browser window by adding hello following CSS toohello head section of hello page:</span></span>

    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

## <a name="playing-a-video"></a><span data-ttu-id="029c7-151">Spela upp en Video</span><span class="sxs-lookup"><span data-stu-id="029c7-151">Playing a Video</span></span>
<span data-ttu-id="029c7-152">tooplay en video webbläsaren vid hello basicPlayback.html filen och klicka på play på hello videospelare visas.</span><span class="sxs-lookup"><span data-stu-id="029c7-152">tooplay a video, point your browser at hello basicPlayback.html file and click play on hello video player displayed.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="029c7-153">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="029c7-153">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="029c7-154">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="029c7-154">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="029c7-155">Se även</span><span class="sxs-lookup"><span data-stu-id="029c7-155">See Also</span></span>
[<span data-ttu-id="029c7-156">Utveckla videospelarprogram</span><span class="sxs-lookup"><span data-stu-id="029c7-156">Develop video player applications</span></span>](media-services-develop-video-players.md)

[<span data-ttu-id="029c7-157">GitHub dash.js-databas</span><span class="sxs-lookup"><span data-stu-id="029c7-157">GitHub dash.js repository</span></span>](https://github.com/Dash-Industry-Forum/dash.js) 

