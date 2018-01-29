---
title: "Lägga till Active Directory på klientsidan | Microsoft Docs"
description: "Det här avsnittet beskrivs hur du infogar annonser på klientsidan."
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
ms.openlocfilehash: 52ba731f88c630830560e3cf8406ba2e9613c8a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="inserting-ads-on-the-client-side"></a>Lägga till Active Directory på klientsidan
Det här avsnittet innehåller information om hur du infogar olika typer av annonser på klientsidan.

Information om stängd textning och ad-stöd i direktsänd strömning videor finns [stöds textning och Ad infogning standarder](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

> [!NOTE]
> Azure Media Player stöder för närvarande inte annonser.
> 
> 

## <a id="insert_ads_into_media"></a>Lägga till Ads i Media
Azure Media Services tillhandahåller support för ad infogning via Windows Media-plattformen: Spelarramverk. Spelarramverk med stöd för ad är tillgängliga för Windows 8, Silverlight, Windows Phone 8 och iOS-enheter. Varje player framework innehåller exempelkod som visar hur du implementerar en player-programmet. Det finns tre olika typer av annonser som du kan infoga i media: listan.

* **Linjär** – fullständig ram annonser som pausa huvudsakliga videon.
* **Linjära** – överlägget annonser som visas när den huvudsakliga videon spelas, vanligtvis en logotyp eller andra statisk avbildning visas i Windows Media player.
* **Medföljande** – annonser som visas utanför Windows Media player.

Annonser kan placeras på valfri plats i tidslinjen för den huvudsakliga video. Du måste se Windows Media player när spela upp ad och vilka annonser som ska spelas upp. Detta görs med hjälp av en uppsättning XML-baserade standardfiler: Video Ad Service mall (VAST), Digital Video flera Ad spelningslista (VMAP), Media abstrakt ordningsföljd mall (MAST) och Digital Video Player Ad Interface Definition (VPAID). STORA filer ange vilka annonser ska visas. VMAP filer ange när att spela upp olika annonser och innehålla stora XML. MAST filer är ett annat sätt att sekvens annonser som även kan innehålla stora XML. VPAID filer definierar ett gränssnitt mellan videospelaren och ad eller ad-server.

Varje player framework fungerar annorlunda och varje omfattas i sin egen avsnittet. Det här avsnittet beskriver de grundläggande mekanismer som används för att infoga annonser. Videospelarprogram begära annonser från en ad-server. Ad-server kan svara på ett antal olika sätt:

* Returnera en omfattande fil
* Returnera en VMAP-fil (med inbäddade VAST)
* Returnera en MAST-fil (med inbäddade VAST)
* Returnera en omfattande fil med VPAID annonser

### <a name="using-a-video-ad-service-template-vast-file"></a>Med hjälp av en Video Ad-tjänsten (VAST) mallfil
En omfattande fil anger vilka ad eller ads ska visas. Följande XML-filen är ett exempel på en omfattande fil för en linjär ad:

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

Linjär ad beskrivs av den <**linjär**> element. Den varaktighet för ad klickar du på Spåra händelser via klickar du på spårning och ett antal **MediaFile** element. Spåra händelser som har angetts i den <**TrackingEvents**> element och tillåta en ad-server att spåra olika händelser som inträffar när du visar ad. I det här fallet start-och mittpunkten, klar och expandera händelser som spåras. Starta händelsen inträffar när annonsen visas. Mittpunkten händelsen inträffar när minst 50% av den ad tidslinjen har visats. Complete-händelsen inträffar när en annons har körts i slutet. Expandera händelsen inträffar när användaren expanderar videospelaren till hela skärmen. Clickthroughs anges med en <**Klickningsförflyttningsrapport**> element i en <**VideoClicks**> element och anger en URI för en resurs ska visas när användaren klickar på ad. ClickTracking har angetts i en <**ClickTracking**> element, även i den <**VideoClicks**> element och anger en spårning resurs så begär när användaren klickar på ad. Den <**MediaFile**> element ange information om en viss kodning av en annons. Om det finns fler än en <**MediaFile**> element i videospelare kan välja den bästa kodningen för plattformen. 

Linjär annonser kan visas i en angiven ordning. Det gör du genom att lägga till ytterligare <Ad> element till VAST filen och ange vilken ordning med attributet sekvens. I följande exempel visar detta:

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

Linjära annonser som har angetts i en <Creative> -element. Följande exempel visar en <Creative> element som beskriver en linjära ad.

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


Den <**NonLinearAds**>-element kan innehålla en eller flera <**NonLinear**>-element, som kan beskriva linjära ad. Den <**NonLinear**>-element anger resursen för den linjära ad. Resursen kan vara en <**StaticResouce**>, <**IFrameResource**>, eller en <**HTMLResouce**>. <**StaticResource**> beskriver en HTML-resurs och definierar ett creativeType-attribut som anger hur resursen visas:

Bild/GIF-, image/jpeg, bild/png – resursen visas i ett HTML-<**img**> taggen.

Program/x-javascript-resursen visas i ett HTML-<**skriptet**> taggen.

Program/x-shockwave-flash-resursen visas i Flash player.

**IFrameResource** beskriver en HTML-resurs som kan visas i en IFrame. **HTMLResource** beskriver en typ av HTML-kod som kan infogas i en webbsida. **TrackingEvents** ange spårning av händelser och URI: N att begära när händelsen inträffar. I det här exemplet spåras händelserna acceptInvitation och komprimera. Mer information om den **NonLinearAds** elementet och dess underordnade finns IAB.NET/VAST. Observera att den **TrackingEvents** element finns i den **NonLinearAds** element snarare än den **NonLinear** element.

Medföljande ads definieras inom ett <CompanionAds> element. Den <CompanionAds> -element kan innehålla en eller flera <Companion> element. Varje <Companion> element beskriver en tillhörande ad och kan innehålla en <StaticResource>, <IFrameResource>, eller <HTMLResource> som anges på samma sätt som i en icke-linjära ad. En omfattande fil kan innehålla flera tillhörande annonser och player-programmet kan välja den lämpligaste ad ska visas. Läs mer om VAST [stora 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

### <a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Med hjälp av en Digital Video flera Ad spelningslista (VMAP)-fil
En VMAP-fil kan du ange när ad avbrott inträffar, hur länge varje break är, hur många annonser kan visas i en paus och vilka typer av annonser kanske visas under en paus. Följande i en exempel VMAP-fil som definierar en enda ad-break:

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

En VMAP-fil som börjar med en <VMAP> element som innehåller en eller flera <AdBreak> element som definierar ett ad-break. Varje ad-break anger en break-typ, break-ID och tidsförskjutningen. Attributet breakType anger vilken typ av ad kan spelas upp under pausen: linjär, linjära, eller visa. Visa annonser karta till stora tillhörande annonser. Fler än en ad-typ kan anges i en kommaavgränsad (inga blanksteg) lista. BreakID är ett valfritt ID för ad. TimeOffset anger när ad ska visas. Det kan anges i något av följande sätt:

1. Tid-formatet: mm: ss eller hh:mm:ss.mmm där .mmm är millisekunder. Värdet för det här attributet anger tiden från början av video tidslinjen till början av ad-break.
2. – N % format där n är procentandelen av video tidslinjen att spela upp innan spelar ad
3. Börja/sluta – anger att en annons ska visas före eller efter att videon har visats
4. Placera – anger ad radbrytningar vid tidpunkten för ad-radbrytningar är okänd, till exempel direktsänd strömning. Ordningen för varje ad-break anges i formatet #n där n är ett heltal som är 1 eller högre. 1 innebär det att ad som ska spelas vid första tillfälle 2 innebär det att ad som ska spelas andra som möjligt och så vidare.

I den <**AdBreak**> element det kan vara en <**AdSource**> element. Den <**AdSource**> elementet innehåller följande attribut:

1. -ID – anger en identifierare för ad-källa
2. allowMultipleAds – ett booleskt värde som anger om flera annonser kan visas under ad-break
3. followRedirects – ett booleskt värde som anger om videospelaren bör respektera omdirigerar inom ett ad-svar

Den <**AdSource**>-elementet har spelaren ett infogat ad svar eller en referens till ett ad-svar. Det kan innehålla något av följande element:

* <VASTAdData>Anger om ad svar är inbäddad i filen VMAP
* <AdTagURI>en URI som refererar till ett ad-svar från en annan dator
* <CustomAdData>-en godtycklig sträng som respresents ett icke-stora svar

I det här exemplet anges en infogad ad-svar med en <VASTAdData> element som innehåller en omfattande ad-svar. Mer information om andra element finns [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

Den <**AdBreak**>-elementet kan också innehålla ett <**TrackingEvents**> element. Den <**TrackingEvents**>-elementet kan du spåra början eller slutet av en ad-break eller om ett fel uppstod under ad-break. Den <**TrackingEvents**> elementet innehåller ett eller flera <**spårning**>-element, som anger en spårning händelse och spårning URI. Spårning av möjliga händelser är:

1. breakStart – spårar i början av en ad-break
2. breakEnd – spåra slutförandet av en ad-break
3. fel – spårar ett fel som uppstod under ad-break

I följande exempel visas en VMAP-fil som anger spårning av händelser

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

Mer information om den <**TrackingEvents**>-elementet och dess underordnade finns http://iab.org/VMAP.pdf

### <a name="using-a-media-abstract-sequencing-template-mast-file"></a>Med hjälp av en Media abstrakt ordningsföljd mallfilen (MAST)
En MAST-fil kan du ange utlösare som definierar när en annons visas. Följande är ett exempel MAST-fil som innehåller utlösare för en sammanslagning pre-annons, en halva sammanslagning ad och en efter sammanslagning ad.

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
            <!--This 'resets' the trigger for the next clip-->
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



En MAST fil börjar med en **MAST** element som innehåller en **utlösare** element. Den <triggers> elementet innehåller ett eller flera **utlösaren** element som definierar när en annons ska spelas upp. 

Den **utlösaren** elementet innehåller ett **startConditions** element som anger när en annons ska börja spela upp. Den **startConditions** elementet innehåller ett eller flera <condition> element. När varje <condition> utvärderas till SANT en utlösare initieras eller återkallas beroende på om den <condition> ingår i en **startConditions** eller **endConditions** element respektive. När flera <condition> element finns, de behandlas som en implicit eller, eventuella villkor som utvärderar TRUE kommer utlösaren som startar. <condition>element kan kapslas. När underordnade <condition> element är förinställda, de behandlas som en implicit och alla villkor måste utvärderas till sant att initiera utlösaren. Den <condition> elementet innehåller följande attribut som definierar villkoret: 

1. **typen** – anger vilken typ av villkor, händelse eller egenskap
2. **namnet** – namnet på egenskapen eller händelsen som ska användas under utvärderingen
3. **värdet** – det värde som en egenskap som ska utvärderas mot
4. **operatorn** – används under utvärderingen: EQ (lika), NEQ (inte lika med), GTR (större), GEQ (större eller lika med), LT (mindre än), LEQ (mindre än eller lika med), MOD (modulo)

**endConditions** också innehålla <condition> element. När villkoret utvärderas till true utlösaren återställs. Den <trigger> elementet innehåller också en <sources> element som innehåller en eller flera <source> element. Den <source> element definiera URI: N för ad-svar och typen av ad-svar. I det här exemplet får en URI för stora svar. 

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
VPAID är en API för att aktivera körbara ad enheter att kommunicera med en videospelare. Detta gör att interaktiva ad upplevelser. Användaren kan interagera med ad och ad kan svara på åtgärder som vidtas av visningsprogrammet. En annons kan till exempel visa knappar som används att visa mer information eller en längre version av ad. Videospelaren måste stödja VPAID-API och körbara ad måste implementera API: et. När en spelare begär en annons från en server med ad servern svarar med ett omfattande svar som innehåller en VPAID ad.

En körbara ad skapas i koden som måste köras i en körningsmiljö som Adobe Flash™ eller JavaScript som kan utföras i en webbläsare. När en ad-servern returnerar ett omfattande svar som innehåller en VPAID ad, värdet för apiFramework attribut i den <MediaFile> elementet måste vara ”VPAID”. Det här attributet anger att den befintliga ad är en VPAID körbara ad. Typ av attribut måste anges till MIME-typ för den körbara filen, till exempel ”application/x-shockwave-flash” eller ”application/x-javascript”. I följande XML-fragment visas den <MediaFile> element från en omfattande svar som innehåller en VPAID körbara ad. 

    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>


En körbara ad kan initieras med hjälp av den <AdParameters> element i den <Linear> eller <NonLinear> element i ett omfattande svar. Mer information om den <AdParameters> element, se [stora 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). Mer information om VPAID-API finns [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

## <a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Implementera en Windows- eller Windows Phone 8 Player med stöd för Ad
Microsoft Media-plattform: Player Framework för Windows 8 och Windows Phone 8 innehåller en samling exempelprogram som visar hur du implementerar en videospelare-program med hjälp av ramverket. Du kan hämta Player Framework och prover från [Player Framework för Windows 8 och Windows Phone 8](https://playerframework.codeplex.com).

När du öppnar Microsoft.PlayerFramework.Xaml.Samples lösningen visas ett antal mappar i projektet. Mappen reklam innehåller exempelkod som är relevanta för att skapa en videospelare med stöd för ad. I marknadsföring är mappen ett antal XAML/cs filer som visar hur du infogar annonser på olika sätt. I följande lista beskrivs var:

* AdPodPage.xaml visar hur du visar en ad-baljor.
* AdSchedulingPage.xaml visar hur du schemalägger annonser.
* FreeWheelPage.xaml visar hur du använder FreeWheel plugin-programmet för att schemalägga annonser.
* MastPage.xaml visar hur du schemalägger annonser med en MAST-fil.
* ProgrammaticAdPage.xaml visar hur du schemalägger programmässigt annonser i en video.
* ScheduleClipPage.xaml visar hur du schemalägger en annons utan en omfattande fil.
* VastLinearCompanionPage.xaml visar hur du infogar en linjär och tillhörande ad.
* VastNonLinearPage.xaml visar hur du infogar en icke-linjär ad.
* VmapPage.xaml visar hur du anger annonser med en VMAP-fil.

Var och en av de här exemplen använder Media Player-klass som definieras av player-ramverket. De flesta prover använda plugin-program som lägger till stöd för olika format för ad-svar. ProgrammaticAdPage provet samverkar programmässigt med en MediaPlayer-instans.

### <a name="adpodpage-sample"></a>AdPodPage-exempel
Det här exemplet använder AdSchedulerPlugin för att definiera när du ska visa en annons. I det här exemplet kommer en annons halva sammanslagning att spelas upp efter 5 sekunder. Ad-baljor (en grupp i Active Directory för att visa i ordning) har angetts i en omfattande fil som returneras från en ad-server. URI: N till stora filen har angetts i den <RemoteAdSource> element.

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

Mer information om AdSchedulerPlugin finns [annonserar i Player Framework på Windows 8 och Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

### <a name="adschedulingpage"></a>AdSchedulingPage
Det här exemplet använder också AdSchedulerPlugin. Den schemaläggs tre annonser, en före sammanslagning ad, en halva sammanslagning ad och en efter sammanslagning ad. URI: N till VAST för varje ad har angetts i en <RemoteAdSource> element.

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
Det här exemplet använder FreeWheelPlugin som anger ett källattribut som anger en URI som pekar på en SmartXML-fil som anger ad innehåll samt ad schemainformation.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="mastpage"></a>MastPage
Det här exemplet använder MastSchedulerPlugin som gör det möjligt att använda en MAST-fil. Källattributet anger platsen för filen MAST.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="programmaticadpage"></a>ProgrammaticAdPage
Det här exemplet samverkar programmässigt med den Media Player. Filen ProgrammaticAdPage.xaml instansierar MediaPlayer:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

Filen ProgrammaticAdPage.xaml.cs skapar en AdHandlerPlugin lägger till en TimelineMarker att ange när en annons ska visas och lägger sedan till en hanterare för händelsen MarkerReached som läser in en RemoteAdSource att ange en URI för en omfattande fil och sedan spelar ad.

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
Det här exemplet använder AdSchedulerPlugin för att schemalägga en halva sammanslagning ad genom att ange en .wmv-fil som innehåller ad.

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
Det här exemplet illustrerar hur du använder AdSchedulerPlugin för att schemalägga en linjär ad halva sammanslagning med en tillhörande ad. Den <RemoteAdSource> elementet anger platsen för filen stora.

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
Det här exemplet använder AdSchedulerPlugin att schemalägga en linjär och en icke-linjär ad. Om filens plats har angetts med den <RemoteAdSource> element.

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
Detta exempel använder VmapSchedulerPlugin för att schemalägga Active Directory med hjälp av en VMAP-fil. URI: N till filen VMAP har angetts i källattributet för den <VmapSchedulerPlugin> element.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

## <a name="implementing-an-ios-video-player-with-ad-support"></a>Implementera en iOS videospelare med stöd för Ad
Microsoft Media-plattform: Player ramverket för iOS innehåller en samling exempelprogram som visar hur du implementerar en videospelare-program med hjälp av ramverket. Du kan hämta Player Framework och prover från [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework). Github-sidan har en länk till en Wiki som innehåller ytterligare information om player framework och en introduktion till player-exempel: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).

### <a name="scheduling-ads-with-vmap"></a>Schemalägga annonser med VMAP
I följande exempel visas hur du schemalägger Active Directory med hjälp av en VMAP-fil.

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest

    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

### <a name="scheduling-ads-with-vast"></a>Schemalägga annonser med VAST
I följande exempel visas hur du schemalägger en sen bindning stora ad.

    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
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

   I följande exempel visas hur du schemalägger en tidig bindning stora ad.
Exempel: 4-schema som en tidig bindning stora ad //Download VAST filen om (! [ framework.adResolver downloadManifest: & manifestet withURL: [NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[self logFrameworkError];} else {adLinearTime.startTime = 7; adLinearTime.duration = 0;

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

I följande exempel visas hur du infogar en ad med hjälp av grov klipp ut redigering (m)

    //Example:1 How to use RCE.
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

I följande exempel visas hur du schemalägger en ad-baljor.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;

    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
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

I följande exempel visas hur du schemalägger en icke-Fäst halva sammanslagning ad. En icke-Fäst ad spelas endast när oavsett eventuella sökning visningsprogrammet utför.

    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
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

I följande exempel visas hur du schemalägger en Fäst halva sammanslagning ad. Fäst ad visas varje gång den angivna punkten på video tidslinjen har uppnåtts.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
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


I följande exempel visas hur du schemalägger en efter sammanslagning ad.

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

I följande exempel visas hur du schemalägger en före sammanslagning ad.

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

I följande exempel visas hur du schemalägger en halva sammanslagning överlägget ad.

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

