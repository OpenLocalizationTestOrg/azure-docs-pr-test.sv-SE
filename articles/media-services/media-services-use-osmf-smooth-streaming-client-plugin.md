---
title: "aaaSmooth Streaming plugin-program för hello öppen källa Media Framework"
description: "Lär dig hur toouse hello Azure Media Services Smooth Streaming-plugin för hello Adobe öppen källa Media Framework."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 6068151f-b6b0-4507-9346-f03416d3d572
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: 3cf8e4679279344cf79c3f0e5b28f63adf88179d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-microsoft-smooth-streaming-plugin-for-hello-adobe-open-source-media-framework"></a><span data-ttu-id="076ec-103">Hur tooUse hello Microsoft Smooth Streaming-Plugin för hello Adobe öppen källa Media Framework</span><span class="sxs-lookup"><span data-stu-id="076ec-103">How tooUse hello Microsoft Smooth Streaming Plugin for hello Adobe Open Source Media Framework</span></span>
## <a name="overview"></a><span data-ttu-id="076ec-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="076ec-104">Overview</span></span>
<span data-ttu-id="076ec-105">Hej Microsoft Smooth Streaming-plugin för öppen källa Media Framework 2.0 (SS för OSMF) utökar hello standard funktionerna i OSMF och lägger till Microsoft Smooth Streaming innehåll uppspelning toonew och befintliga OSMF spelare.</span><span class="sxs-lookup"><span data-stu-id="076ec-105">hello Microsoft Smooth Streaming plugin for Open Source Media Framework 2.0 (SS for OSMF) extends hello default capabilities of OSMF and adds Microsoft Smooth Streaming content playback toonew and existing OSMF players.</span></span> <span data-ttu-id="076ec-106">hello-plugin-programmet lägger också till Smooth Streaming uppspelning funktioner tooStrobe Media uppspelning (SMP).</span><span class="sxs-lookup"><span data-stu-id="076ec-106">hello plugin also adds Smooth Streaming playback capabilities tooStrobe Media Playback (SMP).</span></span>

<span data-ttu-id="076ec-107">SS för OSMF innehåller två versioner av plugin-program:</span><span class="sxs-lookup"><span data-stu-id="076ec-107">SS for OSMF includes two versions of plugin:</span></span>

* <span data-ttu-id="076ec-108">Statisk Smooth Streaming-plugin för OSMF (.swc)</span><span class="sxs-lookup"><span data-stu-id="076ec-108">Static Smooth Streaming plugin for OSMF (.swc)</span></span>
* <span data-ttu-id="076ec-109">Dynamisk Smooth Streaming-plugin för OSMF (.swf)</span><span class="sxs-lookup"><span data-stu-id="076ec-109">Dynamic Smooth Streaming plugin for OSMF (.swf)</span></span>

<span data-ttu-id="076ec-110">Det här dokumentet förutsätts att hello läsaren har allmänna kunskaper om OSMF och OSMF plugin-program. Mer information om OSMF finns hello dokumentationen om hello [officiella OSMF plats](http://osmf.org/).</span><span class="sxs-lookup"><span data-stu-id="076ec-110">This document assumes that hello reader has a general working knowledge of OSMF and OSMF plug-ins. For more information about OSMF, please see hello documentation on hello [official OSMF site](http://osmf.org/).</span></span>

### <a name="smooth-streaming-plugin-for-osmf-20"></a><span data-ttu-id="076ec-111">Smooth Streaming-plugin för OSMF 2.0</span><span class="sxs-lookup"><span data-stu-id="076ec-111">Smooth Streaming plugin for OSMF 2.0</span></span>
<span data-ttu-id="076ec-112">hello-plugin-programmet stöder inläsning och uppspelning av innehåll Smooth Streaming på begäran med hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="076ec-112">hello plugin supports loading and playback of on-demand Smooth Streaming content with hello following features:</span></span>

* <span data-ttu-id="076ec-113">Spela upp Smooth Streaming på begäran (Play pausa Seek, stopp)</span><span class="sxs-lookup"><span data-stu-id="076ec-113">On-demand Smooth Streaming playback (Play, Pause, Seek, Stop)</span></span>
* <span data-ttu-id="076ec-114">Live Smooth Streaming uppspelning (Play)</span><span class="sxs-lookup"><span data-stu-id="076ec-114">Live Smooth Streaming playback (Play)</span></span>
* <span data-ttu-id="076ec-115">Live DVR funktioner (paus, Seek, DVR uppspelningen gå-to-Live)</span><span class="sxs-lookup"><span data-stu-id="076ec-115">Live DVR functions (Pause, Seek, DVR Playback, Go-to-Live)</span></span>
* <span data-ttu-id="076ec-116">Stöd för video-codec - H.264</span><span class="sxs-lookup"><span data-stu-id="076ec-116">Support for video codecs - H.264</span></span>
* <span data-ttu-id="076ec-117">Stöd för ljud codec - AAC</span><span class="sxs-lookup"><span data-stu-id="076ec-117">Support for Audio codecs - AAC</span></span>
* <span data-ttu-id="076ec-118">Växla till OSMF inbyggda API: er för flera ljud språk</span><span class="sxs-lookup"><span data-stu-id="076ec-118">Multiple audio language switching with OSMF built-in APIs</span></span>
* <span data-ttu-id="076ec-119">Max uppspelning kvalitet markeringen med OSMF inbyggda API: er</span><span class="sxs-lookup"><span data-stu-id="076ec-119">Max playback quality selection with OSMF built-in APIs</span></span>
* <span data-ttu-id="076ec-120">Sidovagn textning med OSMF bildtexter plugin-program</span><span class="sxs-lookup"><span data-stu-id="076ec-120">Sidecar closed captions with OSMF captions plugin</span></span>
* <span data-ttu-id="076ec-121">Adobe&reg; Flash&reg; Player 11,4 eller högre.</span><span class="sxs-lookup"><span data-stu-id="076ec-121">Adobe&reg; Flash&reg; Player 11.4 or higher.</span></span>
* <span data-ttu-id="076ec-122">Den här versionen stöder endast OSMF 2.0.</span><span class="sxs-lookup"><span data-stu-id="076ec-122">This version only supports OSMF 2.0.</span></span>

## <a name="supported-features-and-known-issues"></a><span data-ttu-id="076ec-123">Funktioner som stöds och kända problem</span><span class="sxs-lookup"><span data-stu-id="076ec-123">Supported features and known issues</span></span>
<span data-ttu-id="076ec-124">En fullständig lista över funktioner som stöds och funktioner som inte stöds, kända problem finns för[dokumentet](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).</span><span class="sxs-lookup"><span data-stu-id="076ec-124">For a full list of supported features, unsupported features and known issues, refer too[this document](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).</span></span>

## <a name="loading-hello-plugin"></a><span data-ttu-id="076ec-125">Läser in hello plugin-program</span><span class="sxs-lookup"><span data-stu-id="076ec-125">Loading hello Plugin</span></span>
<span data-ttu-id="076ec-126">OSMF plugin-program kan läsas in statiskt (vid kompileringen) eller dynamiskt (vid körning).</span><span class="sxs-lookup"><span data-stu-id="076ec-126">OSMF plugins can be loaded statically (at compile time) or dynamically (at run-time).</span></span> <span data-ttu-id="076ec-127">hello Smooth Streaming-plugin-programmet för att ladda ned OSMF innehåller både dynamiska och statiska versioner.</span><span class="sxs-lookup"><span data-stu-id="076ec-127">hello Smooth Streaming plugin for OSMF download includes both dynamic and static versions.</span></span>

* <span data-ttu-id="076ec-128">Statisk inläsning: tooload statiskt, ett statiskt bibliotek (SWC)-fil måste anges.</span><span class="sxs-lookup"><span data-stu-id="076ec-128">Static loading: tooload statically, a static library (SWC) file is required.</span></span> <span data-ttu-id="076ec-129">Statisk plugin-program läggs till som en referens toohello projekt och sammankoppling i hello slutversionen filen vid hello kompileringen.</span><span class="sxs-lookup"><span data-stu-id="076ec-129">Static plugins are added as a reference toohello projects and merge inside hello final output file at hello compile time.</span></span>
* <span data-ttu-id="076ec-130">Dynamisk inläsning: tooload dynamiskt, en förkompilerade (SWF)-fil måste anges.</span><span class="sxs-lookup"><span data-stu-id="076ec-130">Dynamic loading: tooload dynamically, a precompiled (SWF) file is required.</span></span> <span data-ttu-id="076ec-131">Dynamisk plugin-program laddas i hello runtime och inte ingår i hello projektet utdata.</span><span class="sxs-lookup"><span data-stu-id="076ec-131">Dynamic plugins are loaded in hello runtime and not included in hello project output.</span></span> <span data-ttu-id="076ec-132">(Kompilerade utdata) Dynamisk plugin-program kan läsas med hjälp av protokollen HTTP och fil.</span><span class="sxs-lookup"><span data-stu-id="076ec-132">(Compiled output) Dynamic plugins can be loaded using HTTP and FILE protocols.</span></span>

<span data-ttu-id="076ec-133">Mer information om statiska och dynamiska inläsning finns hello officiella [OSMF plugin-sida](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).</span><span class="sxs-lookup"><span data-stu-id="076ec-133">For more information on static and dynamic loading, see hello official [OSMF plugin page](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).</span></span>

### <a name="ss-for-osmf-static-loading"></a><span data-ttu-id="076ec-134">SS för OSMF statiska inläsning</span><span class="sxs-lookup"><span data-stu-id="076ec-134">SS for OSMF Static Loading</span></span>
<span data-ttu-id="076ec-135">hello kodfragmentet nedan visar hur tooload hello SS plugin-programmet för OSMF statiskt och videouppspelning grundläggande OSMF MediaFactory-klassen.</span><span class="sxs-lookup"><span data-stu-id="076ec-135">hello code snippet below shows how tooload hello SS plugin for OSMF statically and play a basic video using OSMF MediaFactory class.</span></span> <span data-ttu-id="076ec-136">Innan inklusive hello SS OSMF kod, se till att hello projektreferens innehåller hello ”MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc” statiska plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="076ec-136">Before including hello SS for OSMF code, please ensure that hello project reference includes hello "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" static plugin.</span></span>

```
package 
{

    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;



    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;


        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }

        private function initMediaPlayer():void
        {

            // Create hello container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds hello container toohello stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add hello listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load hello plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;

            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // hello plugin is loaded successfully.
            // Your web server needs toohost a valid crossdomain.xml file tooallow plugin toodownload Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")

        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // hello plugin is failed tooload ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started tooload.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code toodeal with Player Ready when it is hit hello first load after a source is loaded. 

                    break;

                case MediaPlayerState.BUFFERING :

                    break;

                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }

        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }

        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it toohello page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;

            // Add hello media element
            _mediaPlayerSprite.media = element;
        }     

    }
}
```


### <a name="ss-for-osmf-dynamic-loading"></a><span data-ttu-id="076ec-137">SS för OSMF dynamisk inläsning</span><span class="sxs-lookup"><span data-stu-id="076ec-137">SS for OSMF Dynamic Loading</span></span>
<span data-ttu-id="076ec-138">hello kodfragmentet nedan visar hur tooload hello SS plugin-programmet för OSMF dynamiskt och videouppspelning grundläggande hello OSMF MediaFactory-klassen.</span><span class="sxs-lookup"><span data-stu-id="076ec-138">hello code snippet below shows how tooload hello SS plugin for OSMF dynamically and play a basic video using hello OSMF MediaFactory class.</span></span> <span data-ttu-id="076ec-139">Kopiera hello ”MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf” dynamisk plugin toohello projektmappen innan inklusive hello SS OSMF kod om du vill tooload med fil-protokollet eller kopiera under en webbserver för HTTP.</span><span class="sxs-lookup"><span data-stu-id="076ec-139">Before including hello SS for OSMF code, copy hello "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" dynamic plugin toohello project folder if you want tooload using FILE protocol, or copy under a web server for HTTP load.</span></span> <span data-ttu-id="076ec-140">Det finns inga behov tooinclude ”MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc” i hello projektreferenser.</span><span class="sxs-lookup"><span data-stu-id="076ec-140">There is no need tooinclude "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" in hello project references.</span></span>

<span data-ttu-id="076ec-141">paketet {</span><span class="sxs-lookup"><span data-stu-id="076ec-141">package {</span></span>

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;


    //Sets hello size of hello SWF

    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;


        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }

        private function initMediaPlayer():void
        {

            // Create hello container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds hello container toohello stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add hello listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load hello plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs toohost a valid crossdomain.xml file tooallow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // hello plugin is loaded successfully.

            // Your web server needs toohost a valid crossdomain.xml file tooallow plugin toodownload Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // hello plugin is failed tooload ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started tooload.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code toodeal with Player Ready when it is hit hello first load after a source is loaded. 

                    break;

                case MediaPlayerState.BUFFERING :

                    break;

                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }

        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }

        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it toohello page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add hello media element
            _mediaPlayerSprite.media = element;
        }     

    }
<span data-ttu-id="076ec-142">}</span><span class="sxs-lookup"><span data-stu-id="076ec-142">}</span></span>

## <a name="strobe-media--playback-with-hello-ss-odmf-dynamic-plugin"></a><span data-ttu-id="076ec-143">Stroboskop uppspelning med hello SS ODMF dynamiska plugin-program</span><span class="sxs-lookup"><span data-stu-id="076ec-143">Strobe Media  Playback with hello SS ODMF Dynamic Plugin</span></span>
<span data-ttu-id="076ec-144">hello Smooth Streaming för OSMF dynamiska plugin-programmet är kompatibelt med [Strobe Media uppspelning (SMP)](http://osmf.org/strobe_mediaplayback.html).</span><span class="sxs-lookup"><span data-stu-id="076ec-144">hello Smooth Streaming for OSMF dynamic plugin is compatible with [Strobe Media Playback (SMP)](http://osmf.org/strobe_mediaplayback.html).</span></span> <span data-ttu-id="076ec-145">Du kan använda hello SS för OSMF plugin-programmet tooSMP tooadd Smooth Streaming uppspelning av innehåll.</span><span class="sxs-lookup"><span data-stu-id="076ec-145">You can use hello SS for OSMF plugin tooadd Smooth Streaming content playback tooSMP.</span></span> <span data-ttu-id="076ec-146">toodo, kopian ”MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf” under en webbserver för HTTP med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="076ec-146">toodo this, copy "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" under a web server for HTTP load using hello following steps:</span></span>

1. <span data-ttu-id="076ec-147">Bläddra hello [Strobe uppspelning installationssidan](http://osmf.org/dev/2.0gm/setup.html).</span><span class="sxs-lookup"><span data-stu-id="076ec-147">Browse hello [Strobe Media Playback setup page](http://osmf.org/dev/2.0gm/setup.html).</span></span> 
2. <span data-ttu-id="076ec-148">Ange hello src tooa Smooth Streaming källa, (t.ex. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest)</span><span class="sxs-lookup"><span data-stu-id="076ec-148">Set hello src tooa Smooth Streaming source, (e.g. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest)</span></span> 
3. <span data-ttu-id="076ec-149">Gör ändringar i hello desired konfigurationen och klicka på Förhandsgranska och uppdatera.</span><span class="sxs-lookup"><span data-stu-id="076ec-149">Make hello desired configuration changes and click Preview and Update.</span></span>
   
   <span data-ttu-id="076ec-150">**Obs** webbservern innehåll måste en giltig crossdomain.xml.</span><span class="sxs-lookup"><span data-stu-id="076ec-150">**Note** Your content web server needs a valid crossdomain.xml.</span></span> 
4. <span data-ttu-id="076ec-151">Kopiera och klistra in hello kod tooa enkel HTML-sida med valfri textredigerare, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="076ec-151">Copy and paste hello code tooa simple HTML page using your favorite text editor, such as in hello following example:</span></span>

        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



1. <span data-ttu-id="076ec-152">Lägg till Smooth Streaming OSMF plugin-programmet toohello bädda in kod och spara.</span><span class="sxs-lookup"><span data-stu-id="076ec-152">Add Smooth Streaming OSMF plugin toohello embed code and save.</span></span>
   
        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>
2. <span data-ttu-id="076ec-153">Spara HTML-sidan och publicera tooa webbservern.</span><span class="sxs-lookup"><span data-stu-id="076ec-153">Save your HTML page and publish tooa web server.</span></span> <span data-ttu-id="076ec-154">Bläddra toohello publicerade webbsida med hjälp av din favorit Flash&reg; Player-aktiverad webbläsare (Internet Explorer, Chrome, Firefox, osv).</span><span class="sxs-lookup"><span data-stu-id="076ec-154">Browse toohello published web page using your favorite Flash&reg; Player enabled Internet browser (Internet Explorer, Chrome, Firefox, so on).</span></span>
3. <span data-ttu-id="076ec-155">Få Smooth Streaming innehåll i Adobe&reg; Flash&reg; Player.</span><span class="sxs-lookup"><span data-stu-id="076ec-155">Enjoy Smooth Streaming content inside Adobe&reg; Flash&reg; Player.</span></span>

<span data-ttu-id="076ec-156">Mer information om allmänna OSMF utvecklingen finns hello officiella [OSMF development sidan](http://osmf.org/resources.html).</span><span class="sxs-lookup"><span data-stu-id="076ec-156">For more information on general OSMF development, please see hello official [OSMF development page](http://osmf.org/resources.html).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="076ec-157">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="076ec-157">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="076ec-158">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="076ec-158">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="076ec-159">Se även</span><span class="sxs-lookup"><span data-stu-id="076ec-159">See Also</span></span>
[<span data-ttu-id="076ec-160">Microsoft anpassningsbar strömning plugin-programmet för OSMF uppdatering</span><span class="sxs-lookup"><span data-stu-id="076ec-160">Microsoft Adaptive Streaming Plugin for OSMF Update</span></span>](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 

