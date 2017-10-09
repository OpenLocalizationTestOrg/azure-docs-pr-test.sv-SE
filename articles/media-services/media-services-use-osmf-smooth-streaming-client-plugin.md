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
# <a name="how-toouse-hello-microsoft-smooth-streaming-plugin-for-hello-adobe-open-source-media-framework"></a>Hur tooUse hello Microsoft Smooth Streaming-Plugin för hello Adobe öppen källa Media Framework
## <a name="overview"></a>Översikt
Hej Microsoft Smooth Streaming-plugin för öppen källa Media Framework 2.0 (SS för OSMF) utökar hello standard funktionerna i OSMF och lägger till Microsoft Smooth Streaming innehåll uppspelning toonew och befintliga OSMF spelare. hello-plugin-programmet lägger också till Smooth Streaming uppspelning funktioner tooStrobe Media uppspelning (SMP).

SS för OSMF innehåller två versioner av plugin-program:

* Statisk Smooth Streaming-plugin för OSMF (.swc)
* Dynamisk Smooth Streaming-plugin för OSMF (.swf)

Det här dokumentet förutsätts att hello läsaren har allmänna kunskaper om OSMF och OSMF plugin-program. Mer information om OSMF finns hello dokumentationen om hello [officiella OSMF plats](http://osmf.org/).

### <a name="smooth-streaming-plugin-for-osmf-20"></a>Smooth Streaming-plugin för OSMF 2.0
hello-plugin-programmet stöder inläsning och uppspelning av innehåll Smooth Streaming på begäran med hello följande funktioner:

* Spela upp Smooth Streaming på begäran (Play pausa Seek, stopp)
* Live Smooth Streaming uppspelning (Play)
* Live DVR funktioner (paus, Seek, DVR uppspelningen gå-to-Live)
* Stöd för video-codec - H.264
* Stöd för ljud codec - AAC
* Växla till OSMF inbyggda API: er för flera ljud språk
* Max uppspelning kvalitet markeringen med OSMF inbyggda API: er
* Sidovagn textning med OSMF bildtexter plugin-program
* Adobe&reg; Flash&reg; Player 11,4 eller högre.
* Den här versionen stöder endast OSMF 2.0.

## <a name="supported-features-and-known-issues"></a>Funktioner som stöds och kända problem
En fullständig lista över funktioner som stöds och funktioner som inte stöds, kända problem finns för[dokumentet](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).

## <a name="loading-hello-plugin"></a>Läser in hello plugin-program
OSMF plugin-program kan läsas in statiskt (vid kompileringen) eller dynamiskt (vid körning). hello Smooth Streaming-plugin-programmet för att ladda ned OSMF innehåller både dynamiska och statiska versioner.

* Statisk inläsning: tooload statiskt, ett statiskt bibliotek (SWC)-fil måste anges. Statisk plugin-program läggs till som en referens toohello projekt och sammankoppling i hello slutversionen filen vid hello kompileringen.
* Dynamisk inläsning: tooload dynamiskt, en förkompilerade (SWF)-fil måste anges. Dynamisk plugin-program laddas i hello runtime och inte ingår i hello projektet utdata. (Kompilerade utdata) Dynamisk plugin-program kan läsas med hjälp av protokollen HTTP och fil.

Mer information om statiska och dynamiska inläsning finns hello officiella [OSMF plugin-sida](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

### <a name="ss-for-osmf-static-loading"></a>SS för OSMF statiska inläsning
hello kodfragmentet nedan visar hur tooload hello SS plugin-programmet för OSMF statiskt och videouppspelning grundläggande OSMF MediaFactory-klassen. Innan inklusive hello SS OSMF kod, se till att hello projektreferens innehåller hello ”MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc” statiska plugin-programmet.

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


### <a name="ss-for-osmf-dynamic-loading"></a>SS för OSMF dynamisk inläsning
hello kodfragmentet nedan visar hur tooload hello SS plugin-programmet för OSMF dynamiskt och videouppspelning grundläggande hello OSMF MediaFactory-klassen. Kopiera hello ”MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf” dynamisk plugin toohello projektmappen innan inklusive hello SS OSMF kod om du vill tooload med fil-protokollet eller kopiera under en webbserver för HTTP. Det finns inga behov tooinclude ”MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc” i hello projektreferenser.

paketet {

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
}

## <a name="strobe-media--playback-with-hello-ss-odmf-dynamic-plugin"></a>Stroboskop uppspelning med hello SS ODMF dynamiska plugin-program
hello Smooth Streaming för OSMF dynamiska plugin-programmet är kompatibelt med [Strobe Media uppspelning (SMP)](http://osmf.org/strobe_mediaplayback.html). Du kan använda hello SS för OSMF plugin-programmet tooSMP tooadd Smooth Streaming uppspelning av innehåll. toodo, kopian ”MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf” under en webbserver för HTTP med hello följande steg:

1. Bläddra hello [Strobe uppspelning installationssidan](http://osmf.org/dev/2.0gm/setup.html). 
2. Ange hello src tooa Smooth Streaming källa, (t.ex. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3. Gör ändringar i hello desired konfigurationen och klicka på Förhandsgranska och uppdatera.
   
   **Obs** webbservern innehåll måste en giltig crossdomain.xml. 
4. Kopiera och klistra in hello kod tooa enkel HTML-sida med valfri textredigerare, som i följande exempel hello:

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



1. Lägg till Smooth Streaming OSMF plugin-programmet toohello bädda in kod och spara.
   
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
2. Spara HTML-sidan och publicera tooa webbservern. Bläddra toohello publicerade webbsida med hjälp av din favorit Flash&reg; Player-aktiverad webbläsare (Internet Explorer, Chrome, Firefox, osv).
3. Få Smooth Streaming innehåll i Adobe&reg; Flash&reg; Player.

Mer information om allmänna OSMF utvecklingen finns hello officiella [OSMF development sidan](http://osmf.org/resources.html).

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Se även
[Microsoft anpassningsbar strömning plugin-programmet för OSMF uppdatering](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 

