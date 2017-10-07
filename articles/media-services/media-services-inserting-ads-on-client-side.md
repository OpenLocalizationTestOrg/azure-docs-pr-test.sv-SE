---
title: "aaaInserting annonser på klientsidan för hello | Microsoft Docs"
description: "Det här avsnittet visar hur hello tooinsert annonser på klientsidan."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 65c9c747-128e-497e-afe0-3f92d2bf7972
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: e6eab4aa92918ad734db8ac3a4e7818d02ed7fe4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="inserting-ads-on-hello-client-side"></a>Lägga till Active Directory på hello på klientsidan
Det här avsnittet innehåller information om hur tooinsert olika typer av annonser på hello på klientsidan.

Information om stängd textning och ad-stöd i direktsänd strömning videor finns [stöds textning och Ad infogning standarder](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

> [!NOTE]
> Azure Media Player stöder för närvarande inte annonser.
> 
> 

## <a id="insert_ads_into_media"></a>Lägga till Ads i Media
Azure Media Services tillhandahåller support för ad infogning via hello Windows Media-plattform: Spelarramverk. Spelarramverk med stöd för ad är tillgängliga för Windows 8, Silverlight, Windows Phone 8 och iOS-enheter. Varje player framework innehåller exempelkod som visar hur tooimplement ett player-program. Det finns tre olika typer av annonser som du kan infoga i media: listan.

* **Linjär** – fullständig ram annonser som pausa hello huvudsakliga video.
* **Linjära** – överlägget annonser som visas när hello huvudsakliga video spelas, vanligtvis en logotyp eller andra statisk avbildning innanför hello player.
* **Medföljande** – annonser som visas utanför hello player.

Active Directory kan placeras när som helst hello huvudsakliga video tidslinje. Du måste ange hello player när tooplay hello ad och som ads tooplay. Detta görs med hjälp av en uppsättning XML-baserade standardfiler: Video Ad Service mall (VAST), Digital Video flera Ad spelningslista (VMAP), Media abstrakt ordningsföljd mall (MAST) och Digital Video Player Ad Interface Definition (VPAID). STORA filer ange vilka annonser toodisplay. VMAP filer ange när tooplay olika annonser och innehålla stora XML. MAST filer är ett annat sätt toosequence annonser som även kan innehålla stora XML. VPAID filer definierar ett gränssnitt mellan hello videospelare och hello ad eller ad-server.

Varje player framework fungerar annorlunda och varje omfattas i sin egen avsnittet. Det här avsnittet beskriver hello grundläggande funktioner som används för tooinsert annonser. Videospelarprogram begära annonser från en ad-server. hello ad-server kan svara på ett antal olika sätt:

* Returnera en omfattande fil
* Returnera en VMAP-fil (med inbäddade VAST)
* Returnera en MAST-fil (med inbäddade VAST)
* Returnera en omfattande fil med VPAID annonser

### <a name="using-a-video-ad-service-template-vast-file"></a>Med hjälp av en Video Ad-tjänsten (VAST) mallfil
En omfattande fil anger vilka ad eller AD toodisplay. hello är följande XML ett exempel på en omfattande fil för en linjär ad:

    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>

hello linjär ad beskrivs av Hej <**linjär**> element. Den anger hello varaktigheten för hello ad klickar du på Spåra händelser via klickar du på spårning och ett antal **MediaFile** element. Spåra händelser som har angetts i Hej <**TrackingEvents**> element och tillåta en ad server tootrack olika händelser som inträffar när du visar hello ad. I det här fallet hello start, mittpunkten, klar och expandera händelser som spåras. hello start händelsen inträffar när hello ad visas. hello mittpunkten händelsen inträffar när minst 50% av hello ad tidslinjen har visats. hello fullständig händelsen inträffar när hello ad har körts toohello slut. hello Expandera händelsen inträffar när hello användaren expanderar hello videospelare toofull skärmen. Clickthroughs anges med en <**Klickningsförflyttningsrapport**> element i en <**VideoClicks**> element och anger en URI tooa resurs toodisplay när hello användare klickar på hello ad. ClickTracking har angetts i en <**ClickTracking**> element, även i Hej <**VideoClicks**> element och anger en spårning resurs för hello player toorequest när hello användare klickar på på hello ad.hello <**MediaFile**> element ange information om en viss kodning av en annons. Om det finns fler än en <**MediaFile**> elementet hello videospelare kan välja hello bästa kodning för hello plattform. 

Linjär annonser kan visas i en angiven ordning. toodo, lägga till ytterligare <Ad> element toohello VAST filen och ange hello ordning med hjälp av hello sekvensattribut. hello som följande exempel visar detta:

    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>

Linjära annonser som har angetts i en <Creative> -element. följande exempel visar hello en <Creative> element som beskriver en linjära ad.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>


Hej <**NonLinearAds**>-element kan innehålla en eller flera <**NonLinear**>-element, som kan beskriva linjära ad. Hej <**NonLinear**>-element anger hello resurs för hello linjära ad. hello resursen kan vara en <**StaticResouce**>, <**IFrameResource**>, eller en <**HTMLResouce**>. <**StaticResource**> beskriver en HTML-resurs och definierar ett creativeType-attribut som anger hur hello resurs visas:

Bild/GIF-, image/jpeg, bild/png – hello resurs visas i en HTML <**img**> taggen.

Program/x-javascript – hello resurs visas i en HTML <**skriptet**> taggen.

Program/x-shockwave-flash-hello resurs visas i Flash player.

**IFrameResource** beskriver en HTML-resurs som kan visas i en IFrame. **HTMLResource** beskriver en typ av HTML-kod som kan infogas i en webbsida. **TrackingEvents** ange spårning av händelser och hello URI toorequest när hello händelse inträffar. I det här exemplet hello spåras acceptInvitation och komprimera händelser. Mer information om hello **NonLinearAds** elementet och dess underordnade finns IAB.NET/VAST. Observera att hello **TrackingEvents** element finns i hello **NonLinearAds** element i stället för hello **NonLinear** element.

Medföljande ads definieras inom ett <CompanionAds> element. Hej <CompanionAds> -element kan innehålla en eller flera <Companion> element. Varje <Companion> element beskriver en tillhörande ad och kan innehålla en <StaticResource>, <IFrameResource>, eller <HTMLResource> som har angetts i hello samma sätt som i en icke-linjära ad. En omfattande fil kan innehålla flera tillhörande annonser och hello player program kan välja hello lämpligaste ad toodisplay. Läs mer om VAST [stora 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

### <a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Med hjälp av en Digital Video flera Ad spelningslista (VMAP)-fil
En VMAP-fil kan du toospecify när ad avbrott inträffar, hur länge varje break är, hur många annonser kan visas i en paus och vilka typer av annonser kanske visas under en paus. följande i en exempel VMAP-fil som definierar en enda ad-break hello:

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

En VMAP-fil som börjar med en <VMAP> element som innehåller en eller flera <AdBreak> element som definierar ett ad-break. Varje ad-break anger en break-typ, break-ID och tidsförskjutningen. Hej breakType attribut anger hello ad kan spelas upp under hello break: linjär, linjära, eller visa. Visa annonser mappa tooVAST tillhörande annonser. Fler än en ad-typ kan anges i en kommaavgränsad (inga blanksteg) lista. Hej breakID är ett valfritt ID för hello ad. Hej timeOffset anger när hello ad ska visas. Det kan anges i något av följande sätt hello:

1. Tid-formatet: mm: ss eller hh:mm:ss.mmm där .mmm är millisekunder. hello-värdet för det här attributet anger hello tiden från hello början av hello video tidslinjen toohello början av hello ad break.
2. Procent – n % format, där n är hello procentandelen hello video tidslinjen tooplay innan spela upp hello ad
3. Börja/sluta – anger att en annons ska visas före eller efter hello video har visats
4. Placera – anger hello ordning ad radbrytningar när hello tidtagningen av hello ad radbrytningar är okänd, till exempel direktsänd strömning. hello ordningen för varje ad-break har angetts i hello #n format, där n är ett heltal som är 1 eller högre. 1 innebär det att hello ad som ska spelas hello första som möjligt, 2 innebär det att hello ad som ska spelas andra hello som möjligt och så vidare.

Inom Hej <**AdBreak**> element det kan vara en <**AdSource**> element. Hej <**AdSource**> elementet innehåller hello följande attribut:

1. -ID – anger en identifierare för hello ad-källa
2. allowMultipleAds – ett booleskt värde som anger om flera annonser kan visas under hello ad break
3. followRedirects – ett booleskt värde som anger om hello videospelare bör respektera omdirigerar inom ett ad-svar

Hej <**AdSource**>-elementet har hello player ett infogat ad svar eller en referens tooan ad-svar. Det kan innehålla något av följande element hello:

* <VASTAdData>Anger om ad svar är inbäddad i hello VMAP fil
* <AdTagURI>en URI som refererar till ett ad-svar från en annan dator
* <CustomAdData>-en godtycklig sträng som respresents ett icke-stora svar

I det här exemplet anges en infogad ad-svar med en <VASTAdData> element som innehåller en omfattande ad-svar. Läs mer om hello andra element i [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

Hej <**AdBreak**>-elementet kan också innehålla ett <**TrackingEvents**> element. Hej <**TrackingEvents**>-elementet kan du tootrack hello början eller slutet av en ad-break eller om ett fel uppstod under hello ad break. Hej <**TrackingEvents**> elementet innehåller ett eller flera <**spårning**>-element, som anger en spårning händelse och spårning URI. hello spårning av möjliga händelser är:

1. breakStart – spårar hello början av en ad-break
2. breakEnd – spåra hello slutförande av en ad-break
3. fel – spårar ett fel som uppstod under hello ad break

hello som följande exempel visar en VMAP-fil som anger spårning av händelser

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

Mer information om Hej <**TrackingEvents**>-elementet och dess underordnade finns http://iab.org/VMAP.pdf

### <a name="using-a-media-abstract-sequencing-template-mast-file"></a>Med hjälp av en Media abstrakt ordningsföljd mallfilen (MAST)
En MAST-fil kan du toospecify utlösare som definierar när en annons visas. hello följande är ett exempel MAST-fil som innehåller utlösare för en sammanslagning pre-annons, en halva sammanslagning ad och en efter sammanslagning ad.

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' hello trigger for hello next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>



En MAST fil börjar med en **MAST** element som innehåller en **utlösare** element. Hej <triggers> elementet innehåller ett eller flera **utlösaren** element som definierar när en annons ska spelas upp. 

Hej **utlösaren** elementet innehåller ett **startConditions** element som anger när en annons ska starta tooplay. Hej **startConditions** elementet innehåller ett eller flera <condition> element. När varje <condition> utvärderar tootrue en utlösare initieras eller återkallas beroende på om hello <condition> ingår i en **startConditions** eller **endConditions** element respektive. När flera <condition> element finns, de behandlas som en implicit eller, eventuella villkor som utvärderar tootrue kommer hello utlösaren tooinitiate. <condition>element kan kapslas. När underordnade <condition> element är förinställda, de behandlas som en implicit och alla villkor måste utvärderas tootrue för hello utlösaren tooinitiate. Hej <condition> elementet innehåller hello följande attribut som definierar hello villkor: 

1. **typen** – anger hello typ av villkor, händelse eller egenskap
2. **namnet** – hello namn på hello egenskapen eller händelsen toobe som används under utvärdering
3. **värdet** – hello-värde som en egenskap som ska utvärderas mot
4. **operatorn** – hello åtgärden toouse under utvärderingen: EQ (lika), NEQ (inte lika med), GTR (större), GEQ (större eller lika med), LT (mindre än), LEQ (mindre än eller lika med), MOD (modulo)

**endConditions** också innehålla <condition> element. Om ett villkor utvärderas tootrue hello utlösare är reset.hello <trigger> elementet innehåller också en <sources> element som innehåller en eller flera <source> element. Hej <source> element definiera hello URI toohello ad svar och hello typ av ad-svar. I det här exemplet tilldelas en URI tooa stora svar. 

    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>


### <a name="using-video-player-ad-interface-definition-vpaid"></a>Med hjälp av Video Player-Ad gränssnittsdefinition (VPAID)
VPAID är en API för att aktivera körbara ad enheter toocommunicate med en videospelare. Detta gör att interaktiva ad upplevelser. hello användare kan interagera med hello ad och hello ad kan svara på tooactions genom hello viewer. En annons kan till exempel visa knappar som hello användaren tooview mer information eller en längre version av hello ad. hello videospelare måste stödja hello VPAID API och hello körbara ad måste implementera hello API. När en spelare begär en annons från ad server hello-servern svarar med ett omfattande svar som innehåller en VPAID ad.

En körbara ad skapas i koden som måste köras i en körningsmiljö som Adobe Flash™ eller JavaScript som kan utföras i en webbläsare. När en ad-servern returnerar ett omfattande svar som innehåller en VPAID ad, hello värdet för hello apiFramework attribut i hello <MediaFile> elementet måste vara ”VPAID”. Det här attributet anger att hello finns ad är en VPAID körbara ad. hello type-attributet måste anges toohello MIME-typ för hello körbara, till exempel ”application/x-shockwave-flash” eller ”application/x-javascript”. hello följande XML-fragment visar hello <MediaFile> element från en omfattande svar som innehåller en VPAID körbara ad. 

    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI tooexecutable ad -->
       </MediaFile>
    </MediaFiles>


En körbara ad kan initieras med hjälp av hello <AdParameters> element i hello <Linear> eller <NonLinear> element i ett omfattande svar. Mer information om hello <AdParameters> element, se [stora 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). Mer information om hello VPAID API finns [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

## <a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Implementera en Windows- eller Windows Phone 8 Player med stöd för Ad
hello Microsoft Media Platform: Player Framework för Windows 8 och Windows Phone 8 innehåller en samling exempelprogram som visar hur tooimplement som använder en videospelare hello framework. Du kan hämta hello Player Framework och hello prover från [Player Framework för Windows 8 och Windows Phone 8](https://playerframework.codeplex.com).

När du öppnar hello Microsoft.PlayerFramework.Xaml.Samples lösning visas ett antal mappar i hello-projekt. hello reklam mappen innehåller hello exempel kod relevanta toocreating videospelare med stöd för ad. I hello reklam mappen är ett antal XAML/cs filer varje som visar hur tooinsert annonser på olika sätt. hello följande lista beskrivs varje:

* AdPodPage.xaml visar hur toodisplay ad baljor.
* AdSchedulingPage.xaml visar hur tooschedule annonser.
* FreeWheelPage.xaml visar hur toouse hello FreeWheel plugin-programmet tooschedule annonser.
* MastPage.xaml visar hur tooschedule annonser med en MAST-fil.
* ProgrammaticAdPage.xaml visar hur tooprogrammatically schemalägga annonser i en video.
* ScheduleClipPage.xaml visar hur tooschedule en annons utan en omfattande fil.
* VastLinearCompanionPage.xaml visar hur en linjär tooinsert och tillhörande ad.
* VastNonLinearPage.xaml visar hur tooinsert en icke-linjär ad.
* VmapPage.xaml visar hur toospecify annonser med en VMAP-fil.

Var och en av de här exemplen använder hello MediaPlayer-klass som definieras av hello player framework. De flesta prover använda plugin-program som lägger till stöd för olika format för ad-svar. Hej ProgrammaticAdPage exempel samverkar programmässigt med en MediaPlayer-instans.

### <a name="adpodpage-sample"></a>AdPodPage-exempel
Det här exemplet använder hello AdSchedulerPlugin toodefine när toodisplay en annons. I det här exemplet är en annons halva sammanslagning schemalagda toobe spelas upp efter 5 sekunder. hello ad baljor (en grupp av annonser toodisplay i ordning) har angetts i en omfattande fil som returneras från en ad-server. hello URI toohello stora fil har angetts i hello <RemoteAdSource> element.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">

        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>

                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>

                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

Läs mer om hello AdSchedulerPlugin [annonserar i hello Player Framework på Windows 8 och Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

### <a name="adschedulingpage"></a>AdSchedulingPage
Det här exemplet använder också hello AdSchedulerPlugin. Den schemaläggs tre annonser, en före sammanslagning ad, en halva sammanslagning ad och en efter sammanslagning ad. Hej URI toohello VAST för varje ad har angetts i en <RemoteAdSource> element.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>

                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


### <a name="freewheelpage"></a>FreeWheelPage
Det här exemplet använder hello FreeWheelPlugin som anger ett källattribut som anger en URI som punkter tooa SmartXML-fil som anger ad innehåll samt ad schemainformation.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="mastpage"></a>MastPage
Det här exemplet använder hello MastSchedulerPlugin som gör att du toouse en MAST-fil. hello källattribut anger hello platsen för hello MAST-filen.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="programmaticadpage"></a>ProgrammaticAdPage
Det här exemplet samverkar programmässigt med hello Media Player. Hej ProgrammaticAdPage.xaml filen instansierar hello MediaPlayer:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

Hej ProgrammaticAdPage.xaml.cs fil skapas en AdHandlerPlugin, lägger till en TimelineMarker toospecify när en annons ska visas och lägger sedan till en hanterare för hello MarkerReached händelse som läser in en RemoteAdSource att ange en URI tooa stora fil och sedan spelar hello ad.

    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;

            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }

            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

### <a name="scheduleclippage"></a>ScheduleClipPage
Det här exemplet använder hello AdSchedulerPlugin tooschedule en halva sammanslagning ad genom att ange en .wmv-fil som innehåller hello ad.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vastlinearcompanionpage"></a>VastLinearCompanionPage
Det här exemplet illustrerar hur hello toouse AdSchedulerPlugin tooschedule en linjär ad halva sammanslagning med en tillhörande ad. Hej <RemoteAdSource> element anger hello platsen för stora hello-filen.

    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage
Det här exemplet använder hello AdSchedulerPlugin tooschedule en linjär och en icke-linjär ad. hello stora filsökväg har angetts med hello <RemoteAdSource> element.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vmappage"></a>VMAPPage
Detta exempel använder hello VmapSchedulerPlugin tooschedule annonser med hjälp av en VMAP-fil. hello URI toohello VMAP fil har angetts i hello källattribut hello <VmapSchedulerPlugin> element.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

## <a name="implementing-an-ios-video-player-with-ad-support"></a>Implementera en iOS videospelare med stöd för Ad
hello Microsoft Media Platform: Player ramverket för iOS innehåller en samling exempelprogram som visar hur tooimplement som använder en videospelare hello framework. Du kan hämta hello Player Framework och hello prover från [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework). Hej github-sidan har en länk tooa Wiki som innehåller ytterligare information om hello player framework och en introduktion toohello player-exempel: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).

### <a name="scheduling-ads-with-vmap"></a>Schemalägga annonser med VMAP
följande exempel visar hur hello tooschedule annonser med hjälp av en VMAP-fil.

    // How tooschedule an Ad using VMAP.
    //First download hello VMAP manifest

    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using hello downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

### <a name="scheduling-ads-with-vast"></a>Schemalägga annonser med VAST
hello följande exempel visar hur tooschedule en sen bindning stora ad.

    //Example:3 How tooschedule a late binding VAST ad.
    // set hello start time for hello ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify hello URI of hello VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL tooVAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

   hello följande exempel visar hur tooschedule en tidig bindning stora ad.
Exempel: 4-schema som en tidig bindning stora ad //Download hello VAST filen om (! [ framework.adResolver downloadManifest: & manifestet withURL: [NSURL URLWithString: @ ”http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml”]]) {[self logFrameworkError];} else {adLinearTime.startTime = 7. adLinearTime.duration = 0;

        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

hello följande exempel visar hur tooinsert ad med grov klipp ut redigering (m)

    //Example:1 How toouse RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

hello visas följande exempel hur tooschedule ad baljor.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;

    // Specify URL toocontent
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI tooad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

följande exempel visar hur hello tooschedule en icke-Fäst halva sammanslagning ad. En icke-Fäst ad spelas endast när oavsett eventuella försöka få hello viewer utför.

    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL toocontent
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

följande exempel visar hur hello tooschedule en Fäst halva sammanslagning ad. Fäst ad visas varje gång hello anges på en hello video tidslinjen har uppnåtts.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI tooad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


hello följande exempel visar hur tooschedule en efter sammanslagning ad.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

hello följande exempel visar hur tooschedule en före sammanslagning ad.

    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

hello som följande exempel visar hur tooschedule halva sammanslagning överlägg ad.

    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Se även
[Utveckla videospelarprogram](media-services-develop-video-players.md)

