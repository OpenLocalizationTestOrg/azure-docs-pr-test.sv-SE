---
title: "Smooth Streaming plugin-programmet för öppen källkod Media Framework"
description: "Lär dig hur du använder Azure Media Services Smooth Streaming plugin-programmet för Adobe öppen källa Media ram."
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
ms.openlocfilehash: 9c764f176ae75085320882de3fb26d8e7d52daaf
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a><span data-ttu-id="3e42a-103">Hur du använder Microsoft-Smooth Streaming-plugin-programmet för Adobe öppen källkod Media Framework</span><span class="sxs-lookup"><span data-stu-id="3e42a-103">How to Use the Microsoft Smooth Streaming Plugin for the Adobe Open Source Media Framework</span></span>
## <a name="overview"></a><span data-ttu-id="3e42a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="3e42a-104">Overview</span></span>
<span data-ttu-id="3e42a-105">Microsoft Smooth Streaming-plugin för öppen källa Media Framework 2.0 (SS för OSMF) utökar funktionerna för OSMF standard och lägger till Microsoft Smooth Streaming innehåll uppspelning nya och befintliga OSMF spelare.</span><span class="sxs-lookup"><span data-stu-id="3e42a-105">The Microsoft Smooth Streaming plugin for Open Source Media Framework 2.0 (SS for OSMF) extends the default capabilities of OSMF and adds Microsoft Smooth Streaming content playback to new and existing OSMF players.</span></span> <span data-ttu-id="3e42a-106">Plugin-programmet lägger också till Smooth Streaming uppspelning funktioner till Strobe Media uppspelning (SMP).</span><span class="sxs-lookup"><span data-stu-id="3e42a-106">The plugin also adds Smooth Streaming playback capabilities to Strobe Media Playback (SMP).</span></span>

<span data-ttu-id="3e42a-107">SS för OSMF innehåller två versioner av plugin-program:</span><span class="sxs-lookup"><span data-stu-id="3e42a-107">SS for OSMF includes two versions of plugin:</span></span>

* <span data-ttu-id="3e42a-108">Statisk Smooth Streaming-plugin för OSMF (.swc)</span><span class="sxs-lookup"><span data-stu-id="3e42a-108">Static Smooth Streaming plugin for OSMF (.swc)</span></span>
* <span data-ttu-id="3e42a-109">Dynamisk Smooth Streaming-plugin för OSMF (.swf)</span><span class="sxs-lookup"><span data-stu-id="3e42a-109">Dynamic Smooth Streaming plugin for OSMF (.swf)</span></span>

<span data-ttu-id="3e42a-110">Det här dokumentet utgår från att läsaren allmänna kunskaper om OSMF och OSMF plugin-program. Mer information om OSMF finns i dokumentationen om den [officiella OSMF plats](http://osmf.org/).</span><span class="sxs-lookup"><span data-stu-id="3e42a-110">This document assumes that the reader has a general working knowledge of OSMF and OSMF plug-ins. For more information about OSMF, please see the documentation on the [official OSMF site](http://osmf.org/).</span></span>

### <a name="smooth-streaming-plugin-for-osmf-20"></a><span data-ttu-id="3e42a-111">Smooth Streaming-plugin för OSMF 2.0</span><span class="sxs-lookup"><span data-stu-id="3e42a-111">Smooth Streaming plugin for OSMF 2.0</span></span>
<span data-ttu-id="3e42a-112">Plugin-programmet stöder inläsning och uppspelning av innehåll Smooth Streaming på begäran för följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="3e42a-112">The plugin supports loading and playback of on-demand Smooth Streaming content with the following features:</span></span>

* <span data-ttu-id="3e42a-113">Spela upp Smooth Streaming på begäran (Play pausa Seek, stopp)</span><span class="sxs-lookup"><span data-stu-id="3e42a-113">On-demand Smooth Streaming playback (Play, Pause, Seek, Stop)</span></span>
* <span data-ttu-id="3e42a-114">Live Smooth Streaming uppspelning (Play)</span><span class="sxs-lookup"><span data-stu-id="3e42a-114">Live Smooth Streaming playback (Play)</span></span>
* <span data-ttu-id="3e42a-115">Live DVR funktioner (paus, Seek, DVR uppspelningen gå-to-Live)</span><span class="sxs-lookup"><span data-stu-id="3e42a-115">Live DVR functions (Pause, Seek, DVR Playback, Go-to-Live)</span></span>
* <span data-ttu-id="3e42a-116">Stöd för video-codec - H.264</span><span class="sxs-lookup"><span data-stu-id="3e42a-116">Support for video codecs - H.264</span></span>
* <span data-ttu-id="3e42a-117">Stöd för ljud codec - AAC</span><span class="sxs-lookup"><span data-stu-id="3e42a-117">Support for Audio codecs - AAC</span></span>
* <span data-ttu-id="3e42a-118">Växla till OSMF inbyggda API: er för flera ljud språk</span><span class="sxs-lookup"><span data-stu-id="3e42a-118">Multiple audio language switching with OSMF built-in APIs</span></span>
* <span data-ttu-id="3e42a-119">Max uppspelning kvalitet markeringen med OSMF inbyggda API: er</span><span class="sxs-lookup"><span data-stu-id="3e42a-119">Max playback quality selection with OSMF built-in APIs</span></span>
* <span data-ttu-id="3e42a-120">Sidovagn textning med OSMF bildtexter plugin-program</span><span class="sxs-lookup"><span data-stu-id="3e42a-120">Sidecar closed captions with OSMF captions plugin</span></span>
* <span data-ttu-id="3e42a-121">Adobe&reg; Flash&reg; Player 11,4 eller högre.</span><span class="sxs-lookup"><span data-stu-id="3e42a-121">Adobe&reg; Flash&reg; Player 11.4 or higher.</span></span>
* <span data-ttu-id="3e42a-122">Den här versionen stöder endast OSMF 2.0.</span><span class="sxs-lookup"><span data-stu-id="3e42a-122">This version only supports OSMF 2.0.</span></span>

## <a name="supported-features-and-known-issues"></a><span data-ttu-id="3e42a-123">Funktioner som stöds och kända problem</span><span class="sxs-lookup"><span data-stu-id="3e42a-123">Supported features and known issues</span></span>
<span data-ttu-id="3e42a-124">En fullständig lista över funktioner som stöds och funktioner som inte stöds, kända problem finns i [dokumentet](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).</span><span class="sxs-lookup"><span data-stu-id="3e42a-124">For a full list of supported features, unsupported features and known issues, refer to [this document](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).</span></span>

## <a name="loading-the-plugin"></a><span data-ttu-id="3e42a-125">Läser in plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="3e42a-125">Loading the Plugin</span></span>
<span data-ttu-id="3e42a-126">OSMF plugin-program kan läsas in statiskt (vid kompileringen) eller dynamiskt (vid körning).</span><span class="sxs-lookup"><span data-stu-id="3e42a-126">OSMF plugins can be loaded statically (at compile time) or dynamically (at run-time).</span></span> <span data-ttu-id="3e42a-127">Plugin-programmet för Smooth Streaming OSMF hämtas innehåller både dynamiska och statiska versioner.</span><span class="sxs-lookup"><span data-stu-id="3e42a-127">The Smooth Streaming plugin for OSMF download includes both dynamic and static versions.</span></span>

* <span data-ttu-id="3e42a-128">Statisk inläsning: Om du vill läsa in statiskt, ett statiskt bibliotek (SWC)-fil måste anges.</span><span class="sxs-lookup"><span data-stu-id="3e42a-128">Static loading: To load statically, a static library (SWC) file is required.</span></span> <span data-ttu-id="3e42a-129">Statisk plugin-program läggs till som en referens till projekt och sammankoppling i den slutgiltiga utdatafil vid kompileringen.</span><span class="sxs-lookup"><span data-stu-id="3e42a-129">Static plugins are added as a reference to the projects and merge inside the final output file at the compile time.</span></span>
* <span data-ttu-id="3e42a-130">Dynamisk inläsning: Om du vill läsa in dynamiskt, krävs en förkompilerade (SWF)-fil.</span><span class="sxs-lookup"><span data-stu-id="3e42a-130">Dynamic loading: To load dynamically, a precompiled (SWF) file is required.</span></span> <span data-ttu-id="3e42a-131">Plugin-program för dynamiska läses in i körningsmiljön och inte ingår i projektet utdata.</span><span class="sxs-lookup"><span data-stu-id="3e42a-131">Dynamic plugins are loaded in the runtime and not included in the project output.</span></span> <span data-ttu-id="3e42a-132">(Kompilerade utdata) Dynamisk plugin-program kan läsas med hjälp av protokollen HTTP och fil.</span><span class="sxs-lookup"><span data-stu-id="3e42a-132">(Compiled output) Dynamic plugins can be loaded using HTTP and FILE protocols.</span></span>

<span data-ttu-id="3e42a-133">Mer information om statiska och dynamiska inläsning finns i officiellt [OSMF plugin-sida](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).</span><span class="sxs-lookup"><span data-stu-id="3e42a-133">For more information on static and dynamic loading, see the official [OSMF plugin page](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).</span></span>

### <a name="ss-for-osmf-static-loading"></a><span data-ttu-id="3e42a-134">SS för OSMF statiska inläsning</span><span class="sxs-lookup"><span data-stu-id="3e42a-134">SS for OSMF Static Loading</span></span>
<span data-ttu-id="3e42a-135">Kodfragmentet nedan visar hur du läsa in SS plugin-programmet för OSMF statiskt och spela upp en grundläggande video OSMF MediaFactory-klassen.</span><span class="sxs-lookup"><span data-stu-id="3e42a-135">The code snippet below shows how to load the SS plugin for OSMF statically and play a basic video using OSMF MediaFactory class.</span></span> <span data-ttu-id="3e42a-136">Innan inklusive SS OSMF kod, se till att projektreferensen innehåller ”MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc” statiska plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="3e42a-136">Before including the SS for OSMF code, please ensure that the project reference includes the "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" static plugin.</span></span>

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

            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load the plugin class 
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
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")

        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started to load.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 

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
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;

            // Add the media element
            _mediaPlayerSprite.media = element;
        }     

    }
}
```


### <a name="ss-for-osmf-dynamic-loading"></a><span data-ttu-id="3e42a-137">SS för OSMF dynamisk inläsning</span><span class="sxs-lookup"><span data-stu-id="3e42a-137">SS for OSMF Dynamic Loading</span></span>
<span data-ttu-id="3e42a-138">Kodfragmentet nedan visar hur du läsa in SS plugin-programmet för OSMF dynamiskt och spela upp en grundläggande video med hjälp av klassen OSMF MediaFactory.</span><span class="sxs-lookup"><span data-stu-id="3e42a-138">The code snippet below shows how to load the SS plugin for OSMF dynamically and play a basic video using the OSMF MediaFactory class.</span></span> <span data-ttu-id="3e42a-139">Kopiera ”MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf” dynamisk plugin-programmet till projektmappen om du vill läsa in filen protokollet innan inklusive SS OSMF kod, eller kopiera under en webbserver för HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e42a-139">Before including the SS for OSMF code, copy the "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" dynamic plugin to the project folder if you want to load using FILE protocol, or copy under a web server for HTTP load.</span></span> <span data-ttu-id="3e42a-140">Det finns inget behov av att inkludera ”MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc” i projektreferenserna.</span><span class="sxs-lookup"><span data-stu-id="3e42a-140">There is no need to include "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" in the project references.</span></span>

<span data-ttu-id="3e42a-141">paketet {</span><span class="sxs-lookup"><span data-stu-id="3e42a-141">package {</span></span>

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;


    //Sets the size of the SWF

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

            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started to load.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 

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
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     

    }
<span data-ttu-id="3e42a-142">}</span><span class="sxs-lookup"><span data-stu-id="3e42a-142">}</span></span>

## <a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a><span data-ttu-id="3e42a-143">Stroboskop uppspelning med SS ODMF dynamiska plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="3e42a-143">Strobe Media  Playback with the SS ODMF Dynamic Plugin</span></span>
<span data-ttu-id="3e42a-144">Smooth Streaming för OSMF dynamiska plugin-programmet är kompatibelt med [Strobe Media uppspelning (SMP)](http://osmf.org/strobe_mediaplayback.html).</span><span class="sxs-lookup"><span data-stu-id="3e42a-144">The Smooth Streaming for OSMF dynamic plugin is compatible with [Strobe Media Playback (SMP)](http://osmf.org/strobe_mediaplayback.html).</span></span> <span data-ttu-id="3e42a-145">Du kan använda SS för OSMF plugin-programmet för att lägga till Smooth Streaming innehåll uppspelning SMP.</span><span class="sxs-lookup"><span data-stu-id="3e42a-145">You can use the SS for OSMF plugin to add Smooth Streaming content playback to SMP.</span></span> <span data-ttu-id="3e42a-146">Gör du genom att kopiera ”MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf” under en webbserver för HTTP med följande steg:</span><span class="sxs-lookup"><span data-stu-id="3e42a-146">To do this, copy "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" under a web server for HTTP load using the following steps:</span></span>

1. <span data-ttu-id="3e42a-147">Bläddra i [Strobe uppspelning installationssidan](http://osmf.org/dev/2.0gm/setup.html).</span><span class="sxs-lookup"><span data-stu-id="3e42a-147">Browse the [Strobe Media Playback setup page](http://osmf.org/dev/2.0gm/setup.html).</span></span> 
2. <span data-ttu-id="3e42a-148">Ange src till en datakälla som Smooth Streaming (t.ex. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest)</span><span class="sxs-lookup"><span data-stu-id="3e42a-148">Set the src to a Smooth Streaming source, (e.g. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest)</span></span> 
3. <span data-ttu-id="3e42a-149">Gör önskade ändringar och klicka på Förhandsgranska och uppdatera.</span><span class="sxs-lookup"><span data-stu-id="3e42a-149">Make the desired configuration changes and click Preview and Update.</span></span>
   
   <span data-ttu-id="3e42a-150">**Obs** webbservern innehåll måste en giltig crossdomain.xml.</span><span class="sxs-lookup"><span data-stu-id="3e42a-150">**Note** Your content web server needs a valid crossdomain.xml.</span></span> 
4. <span data-ttu-id="3e42a-151">Kopiera och klistra in koden till en enkel HTML-sida med hjälp av valfri textredigerare, som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="3e42a-151">Copy and paste the code to a simple HTML page using your favorite text editor, such as in the following example:</span></span>

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



1. <span data-ttu-id="3e42a-152">Lägga till plugin-programmet för Smooth Streaming OSMF i den inbäddade koden och spara.</span><span class="sxs-lookup"><span data-stu-id="3e42a-152">Add Smooth Streaming OSMF plugin to the embed code and save.</span></span>
   
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
2. <span data-ttu-id="3e42a-153">Spara HTML-sidan och publicera till en webbserver.</span><span class="sxs-lookup"><span data-stu-id="3e42a-153">Save your HTML page and publish to a web server.</span></span> <span data-ttu-id="3e42a-154">Bläddra till den publicerade webbsidan med hjälp av din favorit Flash&reg; Player-aktiverad webbläsare (Internet Explorer, Chrome, Firefox, osv).</span><span class="sxs-lookup"><span data-stu-id="3e42a-154">Browse to the published web page using your favorite Flash&reg; Player enabled Internet browser (Internet Explorer, Chrome, Firefox, so on).</span></span>
3. <span data-ttu-id="3e42a-155">Få Smooth Streaming innehåll i Adobe&reg; Flash&reg; Player.</span><span class="sxs-lookup"><span data-stu-id="3e42a-155">Enjoy Smooth Streaming content inside Adobe&reg; Flash&reg; Player.</span></span>

<span data-ttu-id="3e42a-156">Mer information om allmänna OSMF utvecklingen finns i officiellt [OSMF development sidan](http://osmf.org/resources.html).</span><span class="sxs-lookup"><span data-stu-id="3e42a-156">For more information on general OSMF development, please see the official [OSMF development page](http://osmf.org/resources.html).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="3e42a-157">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="3e42a-157">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3e42a-158">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="3e42a-158">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="3e42a-159">Se även</span><span class="sxs-lookup"><span data-stu-id="3e42a-159">See Also</span></span>
[<span data-ttu-id="3e42a-160">Microsoft anpassningsbar strömning plugin-programmet för OSMF uppdatering</span><span class="sxs-lookup"><span data-stu-id="3e42a-160">Microsoft Adaptive Streaming Plugin for OSMF Update</span></span>](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 

