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
# <a name="inserting-ads-on-hello-client-side"></a><span data-ttu-id="a0b68-103">Lägga till Active Directory på hello på klientsidan</span><span class="sxs-lookup"><span data-stu-id="a0b68-103">Inserting ads on hello client side</span></span>
<span data-ttu-id="a0b68-104">Det här avsnittet innehåller information om hur tooinsert olika typer av annonser på hello på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="a0b68-104">This topic contains information on how tooinsert various types of ads on hello client side.</span></span>

<span data-ttu-id="a0b68-105">Information om stängd textning och ad-stöd i direktsänd strömning videor finns [stöds textning och Ad infogning standarder](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).</span><span class="sxs-lookup"><span data-stu-id="a0b68-105">For information about closed captioning and ad support in Live streaming videos, see [Supported Closed Captioning and Ad Insertion Standards](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).</span></span>

> [!NOTE]
> <span data-ttu-id="a0b68-106">Azure Media Player stöder för närvarande inte annonser.</span><span class="sxs-lookup"><span data-stu-id="a0b68-106">Azure Media Player does not currently support Ads.</span></span>
> 
> 

## <span data-ttu-id="a0b68-107"><a id="insert_ads_into_media"></a>Lägga till Ads i Media</span><span class="sxs-lookup"><span data-stu-id="a0b68-107"><a id="insert_ads_into_media"></a>Inserting Ads into your Media</span></span>
<span data-ttu-id="a0b68-108">Azure Media Services tillhandahåller support för ad infogning via hello Windows Media-plattform: Spelarramverk.</span><span class="sxs-lookup"><span data-stu-id="a0b68-108">Azure Media Services provides support for ad insertion through hello Windows Media Platform: Player Frameworks.</span></span> <span data-ttu-id="a0b68-109">Spelarramverk med stöd för ad är tillgängliga för Windows 8, Silverlight, Windows Phone 8 och iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="a0b68-109">Player frameworks with ad support are available for Windows 8, Silverlight, Windows Phone 8, and iOS devices.</span></span> <span data-ttu-id="a0b68-110">Varje player framework innehåller exempelkod som visar hur tooimplement ett player-program. Det finns tre olika typer av annonser som du kan infoga i media: listan.</span><span class="sxs-lookup"><span data-stu-id="a0b68-110">Each player framework contains sample code that shows you how tooimplement a player application.There are three different kinds of ads you can insert into your media:list.</span></span>

* <span data-ttu-id="a0b68-111">**Linjär** – fullständig ram annonser som pausa hello huvudsakliga video.</span><span class="sxs-lookup"><span data-stu-id="a0b68-111">**Linear** – full frame ads that pause hello main video.</span></span>
* <span data-ttu-id="a0b68-112">**Linjära** – överlägget annonser som visas när hello huvudsakliga video spelas, vanligtvis en logotyp eller andra statisk avbildning innanför hello player.</span><span class="sxs-lookup"><span data-stu-id="a0b68-112">**Nonlinear** – overlay ads that are displayed as hello main video is playing, usually a logo or other static image placed within hello player.</span></span>
* <span data-ttu-id="a0b68-113">**Medföljande** – annonser som visas utanför hello player.</span><span class="sxs-lookup"><span data-stu-id="a0b68-113">**Companion** – ads that are displayed outside of hello player.</span></span>

<span data-ttu-id="a0b68-114">Active Directory kan placeras när som helst hello huvudsakliga video tidslinje.</span><span class="sxs-lookup"><span data-stu-id="a0b68-114">Ads can be placed at any point in hello main video’s time line.</span></span> <span data-ttu-id="a0b68-115">Du måste ange hello player när tooplay hello ad och som ads tooplay.</span><span class="sxs-lookup"><span data-stu-id="a0b68-115">You must tell hello player when tooplay hello ad and which ads tooplay.</span></span> <span data-ttu-id="a0b68-116">Detta görs med hjälp av en uppsättning XML-baserade standardfiler: Video Ad Service mall (VAST), Digital Video flera Ad spelningslista (VMAP), Media abstrakt ordningsföljd mall (MAST) och Digital Video Player Ad Interface Definition (VPAID).</span><span class="sxs-lookup"><span data-stu-id="a0b68-116">This is done using a set of standard XML-based files: Video Ad Service Template (VAST), Digital Video Multiple Ad Playlist (VMAP), Media Abstract Sequencing Template (MAST), and Digital Video Player Ad Interface Definition (VPAID).</span></span> <span data-ttu-id="a0b68-117">STORA filer ange vilka annonser toodisplay.</span><span class="sxs-lookup"><span data-stu-id="a0b68-117">VAST files specify what ads toodisplay.</span></span> <span data-ttu-id="a0b68-118">VMAP filer ange när tooplay olika annonser och innehålla stora XML.</span><span class="sxs-lookup"><span data-stu-id="a0b68-118">VMAP files specify when tooplay various ads and contain VAST XML.</span></span> <span data-ttu-id="a0b68-119">MAST filer är ett annat sätt toosequence annonser som även kan innehålla stora XML.</span><span class="sxs-lookup"><span data-stu-id="a0b68-119">MAST files are another way toosequence ads which also can contain VAST XML.</span></span> <span data-ttu-id="a0b68-120">VPAID filer definierar ett gränssnitt mellan hello videospelare och hello ad eller ad-server.</span><span class="sxs-lookup"><span data-stu-id="a0b68-120">VPAID files define an interface between hello video player and hello ad or ad server.</span></span>

<span data-ttu-id="a0b68-121">Varje player framework fungerar annorlunda och varje omfattas i sin egen avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a0b68-121">Each player framework works differently and each will be covered in its own topic.</span></span> <span data-ttu-id="a0b68-122">Det här avsnittet beskriver hello grundläggande funktioner som används för tooinsert annonser. Videospelarprogram begära annonser från en ad-server.</span><span class="sxs-lookup"><span data-stu-id="a0b68-122">This topic will describe hello basic mechanisms used tooinsert ads.Video player applications request ads from an ad server.</span></span> <span data-ttu-id="a0b68-123">hello ad-server kan svara på ett antal olika sätt:</span><span class="sxs-lookup"><span data-stu-id="a0b68-123">hello ad server can respond in a number of ways:</span></span>

* <span data-ttu-id="a0b68-124">Returnera en omfattande fil</span><span class="sxs-lookup"><span data-stu-id="a0b68-124">Return a VAST file</span></span>
* <span data-ttu-id="a0b68-125">Returnera en VMAP-fil (med inbäddade VAST)</span><span class="sxs-lookup"><span data-stu-id="a0b68-125">Return a VMAP file (with embedded VAST)</span></span>
* <span data-ttu-id="a0b68-126">Returnera en MAST-fil (med inbäddade VAST)</span><span class="sxs-lookup"><span data-stu-id="a0b68-126">Return a MAST file (with embedded VAST)</span></span>
* <span data-ttu-id="a0b68-127">Returnera en omfattande fil med VPAID annonser</span><span class="sxs-lookup"><span data-stu-id="a0b68-127">Return a VAST file with VPAID ads</span></span>

### <a name="using-a-video-ad-service-template-vast-file"></a><span data-ttu-id="a0b68-128">Med hjälp av en Video Ad-tjänsten (VAST) mallfil</span><span class="sxs-lookup"><span data-stu-id="a0b68-128">Using a Video Ad Service Template (VAST) File</span></span>
<span data-ttu-id="a0b68-129">En omfattande fil anger vilka ad eller AD toodisplay.</span><span class="sxs-lookup"><span data-stu-id="a0b68-129">A VAST file specifies what ad or ads toodisplay.</span></span> <span data-ttu-id="a0b68-130">hello är följande XML ett exempel på en omfattande fil för en linjär ad:</span><span class="sxs-lookup"><span data-stu-id="a0b68-130">hello following XML is an example of a VAST file for a linear ad:</span></span>

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

<span data-ttu-id="a0b68-131">hello linjär ad beskrivs av Hej <**linjär**> element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-131">hello linear ad is described by hello <**Linear**> element.</span></span> <span data-ttu-id="a0b68-132">Den anger hello varaktigheten för hello ad klickar du på Spåra händelser via klickar du på spårning och ett antal **MediaFile** element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-132">It specifies hello duration of hello ad, tracking events, click through, click tracking, and a number of **MediaFile** elements.</span></span> <span data-ttu-id="a0b68-133">Spåra händelser som har angetts i Hej <**TrackingEvents**> element och tillåta en ad server tootrack olika händelser som inträffar när du visar hello ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-133">Tracking events are specified within hello <**TrackingEvents**> element and allow an ad server tootrack various events that occur while viewing hello ad.</span></span> <span data-ttu-id="a0b68-134">I det här fallet hello start, mittpunkten, klar och expandera händelser som spåras.</span><span class="sxs-lookup"><span data-stu-id="a0b68-134">In this case hello start, midpoint, complete, and expand events are tracked.</span></span> <span data-ttu-id="a0b68-135">hello start händelsen inträffar när hello ad visas.</span><span class="sxs-lookup"><span data-stu-id="a0b68-135">hello start event occurs when hello ad is displayed.</span></span> <span data-ttu-id="a0b68-136">hello mittpunkten händelsen inträffar när minst 50% av hello ad tidslinjen har visats.</span><span class="sxs-lookup"><span data-stu-id="a0b68-136">hello midpoint event occurs when at least 50% of hello ad’s timeline has been viewed.</span></span> <span data-ttu-id="a0b68-137">hello fullständig händelsen inträffar när hello ad har körts toohello slut.</span><span class="sxs-lookup"><span data-stu-id="a0b68-137">hello complete event occurs when hello ad has run toohello end.</span></span> <span data-ttu-id="a0b68-138">hello Expandera händelsen inträffar när hello användaren expanderar hello videospelare toofull skärmen.</span><span class="sxs-lookup"><span data-stu-id="a0b68-138">hello Expand event occurs when hello user expands hello video player toofull screen.</span></span> <span data-ttu-id="a0b68-139">Clickthroughs anges med en <**Klickningsförflyttningsrapport**> element i en <**VideoClicks**> element och anger en URI tooa resurs toodisplay när hello användare klickar på hello ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-139">Clickthroughs are specified with a <**ClickThrough**> element within a <**VideoClicks**> element and specifies a URI tooa resource toodisplay when hello user clicks on hello ad.</span></span> <span data-ttu-id="a0b68-140">ClickTracking har angetts i en <**ClickTracking**> element, även i Hej <**VideoClicks**> element och anger en spårning resurs för hello player toorequest när hello användare klickar på på hello ad.hello <**MediaFile**> element ange information om en viss kodning av en annons.</span><span class="sxs-lookup"><span data-stu-id="a0b68-140">ClickTracking is specified in a <**ClickTracking**> element, also within hello <**VideoClicks**> element and specifies a tracking resource for hello player toorequest when hello user clicks on hello ad.hello <**MediaFile**> elements specify information about a specific encoding of an ad.</span></span> <span data-ttu-id="a0b68-141">Om det finns fler än en <**MediaFile**> elementet hello videospelare kan välja hello bästa kodning för hello plattform.</span><span class="sxs-lookup"><span data-stu-id="a0b68-141">When there is more than one <**MediaFile**> element, hello video player can choose hello best encoding for hello platform.</span></span> 

<span data-ttu-id="a0b68-142">Linjär annonser kan visas i en angiven ordning.</span><span class="sxs-lookup"><span data-stu-id="a0b68-142">Linear ads can be displayed in a specified order.</span></span> <span data-ttu-id="a0b68-143">toodo, lägga till ytterligare <Ad> element toohello VAST filen och ange hello ordning med hjälp av hello sekvensattribut.</span><span class="sxs-lookup"><span data-stu-id="a0b68-143">toodo this, add additional <Ad> elements toohello VAST file and specify hello order using hello sequence attribute.</span></span> <span data-ttu-id="a0b68-144">hello som följande exempel visar detta:</span><span class="sxs-lookup"><span data-stu-id="a0b68-144">hello following example illustrates this:</span></span>

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

<span data-ttu-id="a0b68-145">Linjära annonser som har angetts i en <Creative> -element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-145">Nonlinear ads are specified in a <Creative> element as well.</span></span> <span data-ttu-id="a0b68-146">följande exempel visar hello en <Creative> element som beskriver en linjära ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-146">hello following example shows a <Creative> element that describes a nonlinear ad.</span></span>

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


<span data-ttu-id="a0b68-147">Hej <**NonLinearAds**>-element kan innehålla en eller flera <**NonLinear**>-element, som kan beskriva linjära ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-147">hello <**NonLinearAds**> element can contain one or more <**NonLinear**> elements, each of which can describe a nonlinear ad.</span></span> <span data-ttu-id="a0b68-148">Hej <**NonLinear**>-element anger hello resurs för hello linjära ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-148">hello <**NonLinear**> element specifies hello resource for hello nonlinear ad.</span></span> <span data-ttu-id="a0b68-149">hello resursen kan vara en <**StaticResouce**>, <**IFrameResource**>, eller en <**HTMLResouce**>.</span><span class="sxs-lookup"><span data-stu-id="a0b68-149">hello resource can be a <**StaticResouce**>, an <**IFrameResource**>, or an <**HTMLResouce**>.</span></span><span data-ttu-id="a0b68-150"> <**StaticResource**> beskriver en HTML-resurs och definierar ett creativeType-attribut som anger hur hello resurs visas:</span><span class="sxs-lookup"><span data-stu-id="a0b68-150"> <**StaticResource**> describes a non-HTML resource and defines a creativeType attribute that specifies how hello resource is displayed:</span></span>

<span data-ttu-id="a0b68-151">Bild/GIF-, image/jpeg, bild/png – hello resurs visas i en HTML <**img**> taggen.</span><span class="sxs-lookup"><span data-stu-id="a0b68-151">Image/gif, image/jpeg, image/png – hello resource is displayed in an HTML <**img**> tag.</span></span>

<span data-ttu-id="a0b68-152">Program/x-javascript – hello resurs visas i en HTML <**skriptet**> taggen.</span><span class="sxs-lookup"><span data-stu-id="a0b68-152">Application/x-javascript – hello resource is displayed in an HTML <**script**> tag.</span></span>

<span data-ttu-id="a0b68-153">Program/x-shockwave-flash-hello resurs visas i Flash player.</span><span class="sxs-lookup"><span data-stu-id="a0b68-153">Application/x-shockwave-flash – hello resource is displayed in a Flash player.</span></span>

<span data-ttu-id="a0b68-154">**IFrameResource** beskriver en HTML-resurs som kan visas i en IFrame.</span><span class="sxs-lookup"><span data-stu-id="a0b68-154">**IFrameResource** describes an HTML resource that can be displayed in an IFrame.</span></span> <span data-ttu-id="a0b68-155">**HTMLResource** beskriver en typ av HTML-kod som kan infogas i en webbsida.</span><span class="sxs-lookup"><span data-stu-id="a0b68-155">**HTMLResource** describes a piece of HTML code that can be inserted into a web page.</span></span> <span data-ttu-id="a0b68-156">**TrackingEvents** ange spårning av händelser och hello URI toorequest när hello händelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="a0b68-156">**TrackingEvents** specify tracking events and hello URI toorequest when hello event occurs.</span></span> <span data-ttu-id="a0b68-157">I det här exemplet hello spåras acceptInvitation och komprimera händelser.</span><span class="sxs-lookup"><span data-stu-id="a0b68-157">In this sample hello acceptInvitation and collapse events are tracked.</span></span> <span data-ttu-id="a0b68-158">Mer information om hello **NonLinearAds** elementet och dess underordnade finns IAB.NET/VAST.</span><span class="sxs-lookup"><span data-stu-id="a0b68-158">For more information on hello **NonLinearAds** element and its children, see IAB.NET/VAST.</span></span> <span data-ttu-id="a0b68-159">Observera att hello **TrackingEvents** element finns i hello **NonLinearAds** element i stället för hello **NonLinear** element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-159">Note that hello **TrackingEvents** element is located within hello **NonLinearAds** element rather than hello **NonLinear** element.</span></span>

<span data-ttu-id="a0b68-160">Medföljande ads definieras inom ett <CompanionAds> element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-160">Companion ads are defined within a <CompanionAds> element.</span></span> <span data-ttu-id="a0b68-161">Hej <CompanionAds> -element kan innehålla en eller flera <Companion> element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-161">hello <CompanionAds> element can contain one or more <Companion> elements.</span></span> <span data-ttu-id="a0b68-162">Varje <Companion> element beskriver en tillhörande ad och kan innehålla en <StaticResource>, <IFrameResource>, eller <HTMLResource> som har angetts i hello samma sätt som i en icke-linjära ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-162">Each <Companion> element describes a companion ad and can contain a <StaticResource>, <IFrameResource>, or <HTMLResource> which are specified in hello same way as in a nonlinear ad.</span></span> <span data-ttu-id="a0b68-163">En omfattande fil kan innehålla flera tillhörande annonser och hello player program kan välja hello lämpligaste ad toodisplay.</span><span class="sxs-lookup"><span data-stu-id="a0b68-163">A VAST file can contain multiple companion ads and hello player application can choose hello most appropriate ad toodisplay.</span></span> <span data-ttu-id="a0b68-164">Läs mer om VAST [stora 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span><span class="sxs-lookup"><span data-stu-id="a0b68-164">For more information about VAST, see [VAST 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span></span>

### <a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a><span data-ttu-id="a0b68-165">Med hjälp av en Digital Video flera Ad spelningslista (VMAP)-fil</span><span class="sxs-lookup"><span data-stu-id="a0b68-165">Using a Digital Video Multiple Ad Playlist (VMAP) File</span></span>
<span data-ttu-id="a0b68-166">En VMAP-fil kan du toospecify när ad avbrott inträffar, hur länge varje break är, hur många annonser kan visas i en paus och vilka typer av annonser kanske visas under en paus.</span><span class="sxs-lookup"><span data-stu-id="a0b68-166">A VMAP file allows you toospecify when ad breaks occur, how long each break is, how many ads can be displayed within a break, and what types of ads may be displayed during a break.</span></span> <span data-ttu-id="a0b68-167">följande i en exempel VMAP-fil som definierar en enda ad-break hello:</span><span class="sxs-lookup"><span data-stu-id="a0b68-167">hello following in an example VMAP file that defines a single ad break:</span></span>

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

<span data-ttu-id="a0b68-168">En VMAP-fil som börjar med en <VMAP> element som innehåller en eller flera <AdBreak> element som definierar ett ad-break.</span><span class="sxs-lookup"><span data-stu-id="a0b68-168">A VMAP file begins with a <VMAP> element that contains one or more <AdBreak> elements, each defining an ad break.</span></span> <span data-ttu-id="a0b68-169">Varje ad-break anger en break-typ, break-ID och tidsförskjutningen.</span><span class="sxs-lookup"><span data-stu-id="a0b68-169">Each ad break specifies a break type, break ID, and time offset.</span></span> <span data-ttu-id="a0b68-170">Hej breakType attribut anger hello ad kan spelas upp under hello break: linjär, linjära, eller visa.</span><span class="sxs-lookup"><span data-stu-id="a0b68-170">hello breakType attribute specifies hello type of ad that can be played during hello break: linear, nonlinear, or display.</span></span> <span data-ttu-id="a0b68-171">Visa annonser mappa tooVAST tillhörande annonser.</span><span class="sxs-lookup"><span data-stu-id="a0b68-171">Display ads map tooVAST companion ads.</span></span> <span data-ttu-id="a0b68-172">Fler än en ad-typ kan anges i en kommaavgränsad (inga blanksteg) lista.</span><span class="sxs-lookup"><span data-stu-id="a0b68-172">More than one ad type can be specified in a comma (no spaces) separated list.</span></span> <span data-ttu-id="a0b68-173">Hej breakID är ett valfritt ID för hello ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-173">hello breakID is an optional identifier for hello ad.</span></span> <span data-ttu-id="a0b68-174">Hej timeOffset anger när hello ad ska visas.</span><span class="sxs-lookup"><span data-stu-id="a0b68-174">hello timeOffset specifies when hello ad should be displayed.</span></span> <span data-ttu-id="a0b68-175">Det kan anges i något av följande sätt hello:</span><span class="sxs-lookup"><span data-stu-id="a0b68-175">It can be specified in one of hello following ways:</span></span>

1. <span data-ttu-id="a0b68-176">Tid-formatet: mm: ss eller hh:mm:ss.mmm där .mmm är millisekunder.</span><span class="sxs-lookup"><span data-stu-id="a0b68-176">Time – in hh:mm:ss or hh:mm:ss.mmm format where .mmm is milliseconds.</span></span> <span data-ttu-id="a0b68-177">hello-värdet för det här attributet anger hello tiden från hello början av hello video tidslinjen toohello början av hello ad break.</span><span class="sxs-lookup"><span data-stu-id="a0b68-177">hello value of this attribute specifies hello time from hello beginning of hello video timeline toohello beginning of hello ad break.</span></span>
2. <span data-ttu-id="a0b68-178">Procent – n % format, där n är hello procentandelen hello video tidslinjen tooplay innan spela upp hello ad</span><span class="sxs-lookup"><span data-stu-id="a0b68-178">Percentage – in n% format where n is hello percentage of hello video timeline tooplay before playing hello ad</span></span>
3. <span data-ttu-id="a0b68-179">Börja/sluta – anger att en annons ska visas före eller efter hello video har visats</span><span class="sxs-lookup"><span data-stu-id="a0b68-179">Start/End – specifies that an ad should be displayed before or after hello video has been displayed</span></span>
4. <span data-ttu-id="a0b68-180">Placera – anger hello ordning ad radbrytningar när hello tidtagningen av hello ad radbrytningar är okänd, till exempel direktsänd strömning.</span><span class="sxs-lookup"><span data-stu-id="a0b68-180">Position – specifies hello order of ad breaks when hello timing of hello ad breaks is unknown, such as in live streaming.</span></span> <span data-ttu-id="a0b68-181">hello ordningen för varje ad-break har angetts i hello #n format, där n är ett heltal som är 1 eller högre.</span><span class="sxs-lookup"><span data-stu-id="a0b68-181">hello order of each ad break is specified in hello #n format where n is an integer 1 or greater.</span></span> <span data-ttu-id="a0b68-182">1 innebär det att hello ad som ska spelas hello första som möjligt, 2 innebär det att hello ad som ska spelas andra hello som möjligt och så vidare.</span><span class="sxs-lookup"><span data-stu-id="a0b68-182">1 signifies hello ad should be played at hello first opportunity, 2 signifies hello ad should be played at hello second opportunity and so on.</span></span>

<span data-ttu-id="a0b68-183">Inom Hej <**AdBreak**> element det kan vara en <**AdSource**> element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-183">Within hello <**AdBreak**> element there can be one <**AdSource**> element.</span></span> <span data-ttu-id="a0b68-184">Hej <**AdSource**> elementet innehåller hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="a0b68-184">hello <**AdSource**> element contains hello following attributes:</span></span>

1. <span data-ttu-id="a0b68-185">-ID – anger en identifierare för hello ad-källa</span><span class="sxs-lookup"><span data-stu-id="a0b68-185">Id – specifies an identifier for hello ad source</span></span>
2. <span data-ttu-id="a0b68-186">allowMultipleAds – ett booleskt värde som anger om flera annonser kan visas under hello ad break</span><span class="sxs-lookup"><span data-stu-id="a0b68-186">allowMultipleAds – a Boolean value that specifies whether multiple ads can be displayed during hello ad break</span></span>
3. <span data-ttu-id="a0b68-187">followRedirects – ett booleskt värde som anger om hello videospelare bör respektera omdirigerar inom ett ad-svar</span><span class="sxs-lookup"><span data-stu-id="a0b68-187">followRedirects – an optional Boolean value that specifies if hello video player should honor redirects within an ad response</span></span>

<span data-ttu-id="a0b68-188">Hej <**AdSource**>-elementet har hello player ett infogat ad svar eller en referens tooan ad-svar.</span><span class="sxs-lookup"><span data-stu-id="a0b68-188">hello <**AdSource**> element provides hello player an inline ad response or a reference tooan ad response.</span></span> <span data-ttu-id="a0b68-189">Det kan innehålla något av följande element hello:</span><span class="sxs-lookup"><span data-stu-id="a0b68-189">It can contain one of hello following elements:</span></span>

* <span data-ttu-id="a0b68-190"><VASTAdData>Anger om ad svar är inbäddad i hello VMAP fil</span><span class="sxs-lookup"><span data-stu-id="a0b68-190"><VASTAdData> indicates a VAST ad response is embedded within hello VMAP file</span></span>
* <span data-ttu-id="a0b68-191"><AdTagURI>en URI som refererar till ett ad-svar från en annan dator</span><span class="sxs-lookup"><span data-stu-id="a0b68-191"><AdTagURI> a URI that references an ad response from another system</span></span>
* <span data-ttu-id="a0b68-192"><CustomAdData>-en godtycklig sträng som respresents ett icke-stora svar</span><span class="sxs-lookup"><span data-stu-id="a0b68-192"><CustomAdData> -an arbitrary string that respresents a non-VAST response</span></span>

<span data-ttu-id="a0b68-193">I det här exemplet anges en infogad ad-svar med en <VASTAdData> element som innehåller en omfattande ad-svar.</span><span class="sxs-lookup"><span data-stu-id="a0b68-193">In this example an in-line ad response is specified with a <VASTAdData> element that contains a VAST ad response.</span></span> <span data-ttu-id="a0b68-194">Läs mer om hello andra element i [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).</span><span class="sxs-lookup"><span data-stu-id="a0b68-194">For more information about hello other elements, see [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).</span></span>

<span data-ttu-id="a0b68-195">Hej <**AdBreak**>-elementet kan också innehålla ett <**TrackingEvents**> element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-195">hello <**AdBreak**> element can also contain one <**TrackingEvents**> element.</span></span> <span data-ttu-id="a0b68-196">Hej <**TrackingEvents**>-elementet kan du tootrack hello början eller slutet av en ad-break eller om ett fel uppstod under hello ad break.</span><span class="sxs-lookup"><span data-stu-id="a0b68-196">hello <**TrackingEvents**> element allows you tootrack hello start or end of an ad break or whether an error occurred during hello ad break.</span></span> <span data-ttu-id="a0b68-197">Hej <**TrackingEvents**> elementet innehåller ett eller flera <**spårning**>-element, som anger en spårning händelse och spårning URI.</span><span class="sxs-lookup"><span data-stu-id="a0b68-197">hello <**TrackingEvents**> element contains one or more <**Tracking**> elements, each of which specifies a tracking event and a tracking URI.</span></span> <span data-ttu-id="a0b68-198">hello spårning av möjliga händelser är:</span><span class="sxs-lookup"><span data-stu-id="a0b68-198">hello possible tracking events are:</span></span>

1. <span data-ttu-id="a0b68-199">breakStart – spårar hello början av en ad-break</span><span class="sxs-lookup"><span data-stu-id="a0b68-199">breakStart – tracks hello beginning of an ad break</span></span>
2. <span data-ttu-id="a0b68-200">breakEnd – spåra hello slutförande av en ad-break</span><span class="sxs-lookup"><span data-stu-id="a0b68-200">breakEnd – track hello completion of an ad break</span></span>
3. <span data-ttu-id="a0b68-201">fel – spårar ett fel som uppstod under hello ad break</span><span class="sxs-lookup"><span data-stu-id="a0b68-201">error – tracks an error that occurred during hello ad break</span></span>

<span data-ttu-id="a0b68-202">hello som följande exempel visar en VMAP-fil som anger spårning av händelser</span><span class="sxs-lookup"><span data-stu-id="a0b68-202">hello following example shows a VMAP file that specifies tracking events</span></span>

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

<span data-ttu-id="a0b68-203">Mer information om Hej <**TrackingEvents**>-elementet och dess underordnade finns http://iab.org/VMAP.pdf</span><span class="sxs-lookup"><span data-stu-id="a0b68-203">For more information on hello <**TrackingEvents**> element and its children, see http://iab.org/VMAP.pdf</span></span>

### <a name="using-a-media-abstract-sequencing-template-mast-file"></a><span data-ttu-id="a0b68-204">Med hjälp av en Media abstrakt ordningsföljd mallfilen (MAST)</span><span class="sxs-lookup"><span data-stu-id="a0b68-204">Using a Media Abstract Sequencing Template (MAST) File</span></span>
<span data-ttu-id="a0b68-205">En MAST-fil kan du toospecify utlösare som definierar när en annons visas.</span><span class="sxs-lookup"><span data-stu-id="a0b68-205">A MAST file allows you toospecify triggers that define when an ad is displayed.</span></span> <span data-ttu-id="a0b68-206">hello följande är ett exempel MAST-fil som innehåller utlösare för en sammanslagning pre-annons, en halva sammanslagning ad och en efter sammanslagning ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-206">hello following is an example MAST file that contains triggers for a pre roll ad, a mid-roll ad, and a post-roll ad.</span></span>

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



<span data-ttu-id="a0b68-207">En MAST fil börjar med en **MAST** element som innehåller en **utlösare** element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-207">A MAST file begins with a **MAST** element that contains one **triggers** element.</span></span> <span data-ttu-id="a0b68-208">Hej <triggers> elementet innehåller ett eller flera **utlösaren** element som definierar när en annons ska spelas upp.</span><span class="sxs-lookup"><span data-stu-id="a0b68-208">hello <triggers> element contains one or more **trigger** elements that define when an ad should be played.</span></span> 

<span data-ttu-id="a0b68-209">Hej **utlösaren** elementet innehåller ett **startConditions** element som anger när en annons ska starta tooplay.</span><span class="sxs-lookup"><span data-stu-id="a0b68-209">hello **trigger** element contains a **startConditions** element which specify when an ad should begin tooplay.</span></span> <span data-ttu-id="a0b68-210">Hej **startConditions** elementet innehåller ett eller flera <condition> element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-210">hello **startConditions** element contains one or more <condition> elements.</span></span> <span data-ttu-id="a0b68-211">När varje <condition> utvärderar tootrue en utlösare initieras eller återkallas beroende på om hello <condition> ingår i en **startConditions** eller **endConditions** element respektive.</span><span class="sxs-lookup"><span data-stu-id="a0b68-211">When each <condition> evaluates tootrue a trigger is initiated or revoked depending upon whether hello <condition> is contained within a **startConditions** or **endConditions** element respectively.</span></span> <span data-ttu-id="a0b68-212">När flera <condition> element finns, de behandlas som en implicit eller, eventuella villkor som utvärderar tootrue kommer hello utlösaren tooinitiate.</span><span class="sxs-lookup"><span data-stu-id="a0b68-212">When multiple <condition> elements are present, they are treated as an implicit OR, any condition evaluating tootrue will cause hello trigger tooinitiate.</span></span> <span data-ttu-id="a0b68-213"><condition>element kan kapslas.</span><span class="sxs-lookup"><span data-stu-id="a0b68-213"><condition> elements can be nested.</span></span> <span data-ttu-id="a0b68-214">När underordnade <condition> element är förinställda, de behandlas som en implicit och alla villkor måste utvärderas tootrue för hello utlösaren tooinitiate.</span><span class="sxs-lookup"><span data-stu-id="a0b68-214">When child <condition> elements are preset, they are treated as an implicit AND, all conditions must evaluate tootrue for hello trigger tooinitiate.</span></span> <span data-ttu-id="a0b68-215">Hej <condition> elementet innehåller hello följande attribut som definierar hello villkor:</span><span class="sxs-lookup"><span data-stu-id="a0b68-215">hello <condition> element contains hello following attributes that define hello condition:</span></span> 

1. <span data-ttu-id="a0b68-216">**typen** – anger hello typ av villkor, händelse eller egenskap</span><span class="sxs-lookup"><span data-stu-id="a0b68-216">**type** – specifies hello type of condition, event or property</span></span>
2. <span data-ttu-id="a0b68-217">**namnet** – hello namn på hello egenskapen eller händelsen toobe som används under utvärdering</span><span class="sxs-lookup"><span data-stu-id="a0b68-217">**name** – hello name of hello property or event toobe used during evaluation</span></span>
3. <span data-ttu-id="a0b68-218">**värdet** – hello-värde som en egenskap som ska utvärderas mot</span><span class="sxs-lookup"><span data-stu-id="a0b68-218">**value** – hello value that a property will be evaluated against</span></span>
4. <span data-ttu-id="a0b68-219">**operatorn** – hello åtgärden toouse under utvärderingen: EQ (lika), NEQ (inte lika med), GTR (större), GEQ (större eller lika med), LT (mindre än), LEQ (mindre än eller lika med), MOD (modulo)</span><span class="sxs-lookup"><span data-stu-id="a0b68-219">**operator** – hello operation toouse during evaluation: EQ (equal), NEQ (not equal), GTR (greater), GEQ (greater or equal), LT (Less than), LEQ (less than or equal), MOD (modulo)</span></span>

<span data-ttu-id="a0b68-220">**endConditions** också innehålla <condition> element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-220">**endConditions** also contain <condition> elements.</span></span> <span data-ttu-id="a0b68-221">Om ett villkor utvärderas tootrue hello utlösare är reset.hello <trigger> elementet innehåller också en <sources> element som innehåller en eller flera <source> element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-221">When a condition evaluates tootrue hello trigger is reset.hello <trigger> element also contains a <sources> element that contains one or more <source> elements.</span></span> <span data-ttu-id="a0b68-222">Hej <source> element definiera hello URI toohello ad svar och hello typ av ad-svar.</span><span class="sxs-lookup"><span data-stu-id="a0b68-222">hello <source> elements define hello URI toohello ad response and hello type of ad response.</span></span> <span data-ttu-id="a0b68-223">I det här exemplet tilldelas en URI tooa stora svar.</span><span class="sxs-lookup"><span data-stu-id="a0b68-223">In this example a URI is given tooa VAST response.</span></span> 

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


### <a name="using-video-player-ad-interface-definition-vpaid"></a><span data-ttu-id="a0b68-224">Med hjälp av Video Player-Ad gränssnittsdefinition (VPAID)</span><span class="sxs-lookup"><span data-stu-id="a0b68-224">Using Video Player-Ad Interface Definition (VPAID)</span></span>
<span data-ttu-id="a0b68-225">VPAID är en API för att aktivera körbara ad enheter toocommunicate med en videospelare.</span><span class="sxs-lookup"><span data-stu-id="a0b68-225">VPAID is an API for enabling executable ad units toocommunicate with a video player.</span></span> <span data-ttu-id="a0b68-226">Detta gör att interaktiva ad upplevelser.</span><span class="sxs-lookup"><span data-stu-id="a0b68-226">This allows highly interactive ad experiences.</span></span> <span data-ttu-id="a0b68-227">hello användare kan interagera med hello ad och hello ad kan svara på tooactions genom hello viewer.</span><span class="sxs-lookup"><span data-stu-id="a0b68-227">hello user can interact with hello ad and hello ad can respond tooactions taken by hello viewer.</span></span> <span data-ttu-id="a0b68-228">En annons kan till exempel visa knappar som hello användaren tooview mer information eller en längre version av hello ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-228">For example an ad may display buttons that allow hello user tooview more information or a longer version of hello ad.</span></span> <span data-ttu-id="a0b68-229">hello videospelare måste stödja hello VPAID API och hello körbara ad måste implementera hello API.</span><span class="sxs-lookup"><span data-stu-id="a0b68-229">hello video player must support hello VPAID API and hello executable ad must implement hello API.</span></span> <span data-ttu-id="a0b68-230">När en spelare begär en annons från ad server hello-servern svarar med ett omfattande svar som innehåller en VPAID ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-230">When a player requests an ad from an ad server hello server may respond with a VAST response that contains a VPAID ad.</span></span>

<span data-ttu-id="a0b68-231">En körbara ad skapas i koden som måste köras i en körningsmiljö som Adobe Flash™ eller JavaScript som kan utföras i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a0b68-231">An executable ad is created in code that must be executed in a runtime environment such as Adobe Flash™ or JavaScript that can be executed in a web browser.</span></span> <span data-ttu-id="a0b68-232">När en ad-servern returnerar ett omfattande svar som innehåller en VPAID ad, hello värdet för hello apiFramework attribut i hello <MediaFile> elementet måste vara ”VPAID”.</span><span class="sxs-lookup"><span data-stu-id="a0b68-232">When an ad server returns a VAST response containing a VPAID ad, hello value of hello apiFramework attribute in hello <MediaFile> element must be “VPAID”.</span></span> <span data-ttu-id="a0b68-233">Det här attributet anger att hello finns ad är en VPAID körbara ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-233">This attribute specifies that hello contained ad is a VPAID executable ad.</span></span> <span data-ttu-id="a0b68-234">hello type-attributet måste anges toohello MIME-typ för hello körbara, till exempel ”application/x-shockwave-flash” eller ”application/x-javascript”.</span><span class="sxs-lookup"><span data-stu-id="a0b68-234">hello type attribute must be set toohello MIME type of hello executable, such as “application/x-shockwave-flash” or “application/x-javascript”.</span></span> <span data-ttu-id="a0b68-235">hello följande XML-fragment visar hello <MediaFile> element från en omfattande svar som innehåller en VPAID körbara ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-235">hello following XML snippet shows hello <MediaFile> element from a VAST response containing a VPAID executable ad.</span></span> 

    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI tooexecutable ad -->
       </MediaFile>
    </MediaFiles>


<span data-ttu-id="a0b68-236">En körbara ad kan initieras med hjälp av hello <AdParameters> element i hello <Linear> eller <NonLinear> element i ett omfattande svar.</span><span class="sxs-lookup"><span data-stu-id="a0b68-236">An executable ad can be initialized using hello <AdParameters> element within hello <Linear> or <NonLinear> elements in a VAST response.</span></span> <span data-ttu-id="a0b68-237">Mer information om hello <AdParameters> element, se [stora 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span><span class="sxs-lookup"><span data-stu-id="a0b68-237">For more information on hello <AdParameters> element, see [VAST 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span></span> <span data-ttu-id="a0b68-238">Mer information om hello VPAID API finns [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).</span><span class="sxs-lookup"><span data-stu-id="a0b68-238">For more information about hello VPAID API, see [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).</span></span>

## <a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a><span data-ttu-id="a0b68-239">Implementera en Windows- eller Windows Phone 8 Player med stöd för Ad</span><span class="sxs-lookup"><span data-stu-id="a0b68-239">Implementing a Windows or Windows Phone 8 Player with Ad Support</span></span>
<span data-ttu-id="a0b68-240">hello Microsoft Media Platform: Player Framework för Windows 8 och Windows Phone 8 innehåller en samling exempelprogram som visar hur tooimplement som använder en videospelare hello framework.</span><span class="sxs-lookup"><span data-stu-id="a0b68-240">hello Microsoft Media Platform: Player Framework for Windows 8 and Windows Phone 8 contains a collection of sample applications that show you how tooimplement a video player application using hello framework.</span></span> <span data-ttu-id="a0b68-241">Du kan hämta hello Player Framework och hello prover från [Player Framework för Windows 8 och Windows Phone 8](https://playerframework.codeplex.com).</span><span class="sxs-lookup"><span data-stu-id="a0b68-241">You can download hello Player Framework and hello samples from [Player Framework for Windows 8 and Windows Phone 8](https://playerframework.codeplex.com).</span></span>

<span data-ttu-id="a0b68-242">När du öppnar hello Microsoft.PlayerFramework.Xaml.Samples lösning visas ett antal mappar i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="a0b68-242">When you open hello Microsoft.PlayerFramework.Xaml.Samples solution you will see a number of folders within hello project.</span></span> <span data-ttu-id="a0b68-243">hello reklam mappen innehåller hello exempel kod relevanta toocreating videospelare med stöd för ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-243">hello Advertising folder contains hello sample code relevant toocreating a video player with ad support.</span></span> <span data-ttu-id="a0b68-244">I hello reklam mappen är ett antal XAML/cs filer varje som visar hur tooinsert annonser på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="a0b68-244">Inside hello Advertising folder is a number of XAML/cs files each of which show how tooinsert ads in a different way.</span></span> <span data-ttu-id="a0b68-245">hello följande lista beskrivs varje:</span><span class="sxs-lookup"><span data-stu-id="a0b68-245">hello following list describes each:</span></span>

* <span data-ttu-id="a0b68-246">AdPodPage.xaml visar hur toodisplay ad baljor.</span><span class="sxs-lookup"><span data-stu-id="a0b68-246">AdPodPage.xaml Shows how toodisplay an ad pod.</span></span>
* <span data-ttu-id="a0b68-247">AdSchedulingPage.xaml visar hur tooschedule annonser.</span><span class="sxs-lookup"><span data-stu-id="a0b68-247">AdSchedulingPage.xaml Shows how tooschedule ads.</span></span>
* <span data-ttu-id="a0b68-248">FreeWheelPage.xaml visar hur toouse hello FreeWheel plugin-programmet tooschedule annonser.</span><span class="sxs-lookup"><span data-stu-id="a0b68-248">FreeWheelPage.xaml Shows how toouse hello FreeWheel plugin tooschedule ads.</span></span>
* <span data-ttu-id="a0b68-249">MastPage.xaml visar hur tooschedule annonser med en MAST-fil.</span><span class="sxs-lookup"><span data-stu-id="a0b68-249">MastPage.xaml Shows how tooschedule ads with a MAST file.</span></span>
* <span data-ttu-id="a0b68-250">ProgrammaticAdPage.xaml visar hur tooprogrammatically schemalägga annonser i en video.</span><span class="sxs-lookup"><span data-stu-id="a0b68-250">ProgrammaticAdPage.xaml Shows how tooprogrammatically schedule ads into a video.</span></span>
* <span data-ttu-id="a0b68-251">ScheduleClipPage.xaml visar hur tooschedule en annons utan en omfattande fil.</span><span class="sxs-lookup"><span data-stu-id="a0b68-251">ScheduleClipPage.xaml Shows how tooschedule an ad without a VAST file.</span></span>
* <span data-ttu-id="a0b68-252">VastLinearCompanionPage.xaml visar hur en linjär tooinsert och tillhörande ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-252">VastLinearCompanionPage.xaml Shows how tooinsert a linear and companion ad.</span></span>
* <span data-ttu-id="a0b68-253">VastNonLinearPage.xaml visar hur tooinsert en icke-linjär ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-253">VastNonLinearPage.xaml Shows how tooinsert a non-linear ad.</span></span>
* <span data-ttu-id="a0b68-254">VmapPage.xaml visar hur toospecify annonser med en VMAP-fil.</span><span class="sxs-lookup"><span data-stu-id="a0b68-254">VmapPage.xaml Shows how toospecify ads with a VMAP file.</span></span>

<span data-ttu-id="a0b68-255">Var och en av de här exemplen använder hello MediaPlayer-klass som definieras av hello player framework.</span><span class="sxs-lookup"><span data-stu-id="a0b68-255">Each of these samples uses hello MediaPlayer class defined by hello player framework.</span></span> <span data-ttu-id="a0b68-256">De flesta prover använda plugin-program som lägger till stöd för olika format för ad-svar.</span><span class="sxs-lookup"><span data-stu-id="a0b68-256">Most samples use plugins that add support for various ad response formats.</span></span> <span data-ttu-id="a0b68-257">Hej ProgrammaticAdPage exempel samverkar programmässigt med en MediaPlayer-instans.</span><span class="sxs-lookup"><span data-stu-id="a0b68-257">hello ProgrammaticAdPage sample programmatically interacts with a MediaPlayer instance.</span></span>

### <a name="adpodpage-sample"></a><span data-ttu-id="a0b68-258">AdPodPage-exempel</span><span class="sxs-lookup"><span data-stu-id="a0b68-258">AdPodPage Sample</span></span>
<span data-ttu-id="a0b68-259">Det här exemplet använder hello AdSchedulerPlugin toodefine när toodisplay en annons.</span><span class="sxs-lookup"><span data-stu-id="a0b68-259">This sample uses hello AdSchedulerPlugin toodefine when toodisplay an ad.</span></span> <span data-ttu-id="a0b68-260">I det här exemplet är en annons halva sammanslagning schemalagda toobe spelas upp efter 5 sekunder.</span><span class="sxs-lookup"><span data-stu-id="a0b68-260">In this example a mid-roll advertisement is scheduled toobe played after 5 seconds.</span></span> <span data-ttu-id="a0b68-261">hello ad baljor (en grupp av annonser toodisplay i ordning) har angetts i en omfattande fil som returneras från en ad-server.</span><span class="sxs-lookup"><span data-stu-id="a0b68-261">hello ad pod (a group of ads toodisplay in order) is specified in a VAST file returned from an ad server.</span></span> <span data-ttu-id="a0b68-262">hello URI toohello stora fil har angetts i hello <RemoteAdSource> element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-262">hello URI toohello VAST file is specified in hello <RemoteAdSource> element.</span></span>

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

<span data-ttu-id="a0b68-263">Läs mer om hello AdSchedulerPlugin [annonserar i hello Player Framework på Windows 8 och Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)</span><span class="sxs-lookup"><span data-stu-id="a0b68-263">For more information about hello AdSchedulerPlugin, see [Advertising in hello Player Framework on Windows 8 and Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)</span></span>

### <a name="adschedulingpage"></a><span data-ttu-id="a0b68-264">AdSchedulingPage</span><span class="sxs-lookup"><span data-stu-id="a0b68-264">AdSchedulingPage</span></span>
<span data-ttu-id="a0b68-265">Det här exemplet använder också hello AdSchedulerPlugin.</span><span class="sxs-lookup"><span data-stu-id="a0b68-265">This sample also uses hello AdSchedulerPlugin.</span></span> <span data-ttu-id="a0b68-266">Den schemaläggs tre annonser, en före sammanslagning ad, en halva sammanslagning ad och en efter sammanslagning ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-266">It schedules three ads, a pre-roll ad, a mid-roll ad, and a post-roll ad.</span></span> <span data-ttu-id="a0b68-267">Hej URI toohello VAST för varje ad har angetts i en <RemoteAdSource> element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-267">hello URI toohello VAST for each ad is specified in a <RemoteAdSource> element.</span></span>

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


### <a name="freewheelpage"></a><span data-ttu-id="a0b68-268">FreeWheelPage</span><span class="sxs-lookup"><span data-stu-id="a0b68-268">FreeWheelPage</span></span>
<span data-ttu-id="a0b68-269">Det här exemplet använder hello FreeWheelPlugin som anger ett källattribut som anger en URI som punkter tooa SmartXML-fil som anger ad innehåll samt ad schemainformation.</span><span class="sxs-lookup"><span data-stu-id="a0b68-269">This sample uses hello FreeWheelPlugin which specifies a Source attribute that specifies a URI that points tooa SmartXML file that specifies ad content as well as ad scheduling information.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="mastpage"></a><span data-ttu-id="a0b68-270">MastPage</span><span class="sxs-lookup"><span data-stu-id="a0b68-270">MastPage</span></span>
<span data-ttu-id="a0b68-271">Det här exemplet använder hello MastSchedulerPlugin som gör att du toouse en MAST-fil.</span><span class="sxs-lookup"><span data-stu-id="a0b68-271">This sample uses hello MastSchedulerPlugin that allows you toouse a MAST file.</span></span> <span data-ttu-id="a0b68-272">hello källattribut anger hello platsen för hello MAST-filen.</span><span class="sxs-lookup"><span data-stu-id="a0b68-272">hello Source attribute specifies hello location of hello MAST file.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="programmaticadpage"></a><span data-ttu-id="a0b68-273">ProgrammaticAdPage</span><span class="sxs-lookup"><span data-stu-id="a0b68-273">ProgrammaticAdPage</span></span>
<span data-ttu-id="a0b68-274">Det här exemplet samverkar programmässigt med hello Media Player.</span><span class="sxs-lookup"><span data-stu-id="a0b68-274">This sample programmatically interacts with hello MediaPlayer.</span></span> <span data-ttu-id="a0b68-275">Hej ProgrammaticAdPage.xaml filen instansierar hello MediaPlayer:</span><span class="sxs-lookup"><span data-stu-id="a0b68-275">hello ProgrammaticAdPage.xaml file instantiates hello MediaPlayer:</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

<span data-ttu-id="a0b68-276">Hej ProgrammaticAdPage.xaml.cs fil skapas en AdHandlerPlugin, lägger till en TimelineMarker toospecify när en annons ska visas och lägger sedan till en hanterare för hello MarkerReached händelse som läser in en RemoteAdSource att ange en URI tooa stora fil och sedan spelar hello ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-276">hello ProgrammaticAdPage.xaml.cs file creates an AdHandlerPlugin, adds a TimelineMarker toospecify when an ad should be displayed, and then adds a handler for hello MarkerReached event which loads a RemoteAdSource specifying a URI tooa VAST file, and then plays hello ad.</span></span>

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

### <a name="scheduleclippage"></a><span data-ttu-id="a0b68-277">ScheduleClipPage</span><span class="sxs-lookup"><span data-stu-id="a0b68-277">ScheduleClipPage</span></span>
<span data-ttu-id="a0b68-278">Det här exemplet använder hello AdSchedulerPlugin tooschedule en halva sammanslagning ad genom att ange en .wmv-fil som innehåller hello ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-278">This sample uses hello AdSchedulerPlugin tooschedule a mid-roll ad by specifying a .wmv file that contains hello ad.</span></span>

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

### <a name="vastlinearcompanionpage"></a><span data-ttu-id="a0b68-279">VastLinearCompanionPage</span><span class="sxs-lookup"><span data-stu-id="a0b68-279">VastLinearCompanionPage</span></span>
<span data-ttu-id="a0b68-280">Det här exemplet illustrerar hur hello toouse AdSchedulerPlugin tooschedule en linjär ad halva sammanslagning med en tillhörande ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-280">This sample illustrates how toouse hello AdSchedulerPlugin tooschedule a mid-roll linear ad with an companion ad.</span></span> <span data-ttu-id="a0b68-281">Hej <RemoteAdSource> element anger hello platsen för stora hello-filen.</span><span class="sxs-lookup"><span data-stu-id="a0b68-281">hello <RemoteAdSource> element specifies hello location of hello VAST file.</span></span>

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

### <a name="vastlinearnonlinearpage"></a><span data-ttu-id="a0b68-282">VastLinearNonLinearPage</span><span class="sxs-lookup"><span data-stu-id="a0b68-282">VastLinearNonLinearPage</span></span>
<span data-ttu-id="a0b68-283">Det här exemplet använder hello AdSchedulerPlugin tooschedule en linjär och en icke-linjär ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-283">This sample uses hello AdSchedulerPlugin tooschedule a linear and a non-linear ad.</span></span> <span data-ttu-id="a0b68-284">hello stora filsökväg har angetts med hello <RemoteAdSource> element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-284">hello VAST file location is specified with hello <RemoteAdSource> element.</span></span>

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

### <a name="vmappage"></a><span data-ttu-id="a0b68-285">VMAPPage</span><span class="sxs-lookup"><span data-stu-id="a0b68-285">VMAPPage</span></span>
<span data-ttu-id="a0b68-286">Detta exempel använder hello VmapSchedulerPlugin tooschedule annonser med hjälp av en VMAP-fil.</span><span class="sxs-lookup"><span data-stu-id="a0b68-286">This samples uses hello VmapSchedulerPlugin tooschedule ads using a VMAP file.</span></span> <span data-ttu-id="a0b68-287">hello URI toohello VMAP fil har angetts i hello källattribut hello <VmapSchedulerPlugin> element.</span><span class="sxs-lookup"><span data-stu-id="a0b68-287">hello URI toohello VMAP file is specified in hello Source attribute of hello <VmapSchedulerPlugin> element.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

## <a name="implementing-an-ios-video-player-with-ad-support"></a><span data-ttu-id="a0b68-288">Implementera en iOS videospelare med stöd för Ad</span><span class="sxs-lookup"><span data-stu-id="a0b68-288">Implementing an iOS Video Player with Ad Support</span></span>
<span data-ttu-id="a0b68-289">hello Microsoft Media Platform: Player ramverket för iOS innehåller en samling exempelprogram som visar hur tooimplement som använder en videospelare hello framework.</span><span class="sxs-lookup"><span data-stu-id="a0b68-289">hello Microsoft Media Platform: Player Framework for iOS contains a collection of sample applications that show you how tooimplement a video player application using hello framework.</span></span> <span data-ttu-id="a0b68-290">Du kan hämta hello Player Framework och hello prover från [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework).</span><span class="sxs-lookup"><span data-stu-id="a0b68-290">You can download hello Player Framework and hello samples from [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework).</span></span> <span data-ttu-id="a0b68-291">Hej github-sidan har en länk tooa Wiki som innehåller ytterligare information om hello player framework och en introduktion toohello player-exempel: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).</span><span class="sxs-lookup"><span data-stu-id="a0b68-291">hello github page has a link tooa Wiki that contains additional information on hello player framework and an introduction toohello player sample: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).</span></span>

### <a name="scheduling-ads-with-vmap"></a><span data-ttu-id="a0b68-292">Schemalägga annonser med VMAP</span><span class="sxs-lookup"><span data-stu-id="a0b68-292">Scheduling Ads with VMAP</span></span>
<span data-ttu-id="a0b68-293">följande exempel visar hur hello tooschedule annonser med hjälp av en VMAP-fil.</span><span class="sxs-lookup"><span data-stu-id="a0b68-293">hello following example shows how tooschedule ads using a VMAP file.</span></span>

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

### <a name="scheduling-ads-with-vast"></a><span data-ttu-id="a0b68-294">Schemalägga annonser med VAST</span><span class="sxs-lookup"><span data-stu-id="a0b68-294">Scheduling Ads with VAST</span></span>
<span data-ttu-id="a0b68-295">hello följande exempel visar hur tooschedule en sen bindning stora ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-295">hello following sample shows how tooschedule a late binding VAST ad.</span></span>

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

   <span data-ttu-id="a0b68-296">hello följande exempel visar hur tooschedule en tidig bindning stora ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-296">hello following sample shows how tooschedule an early binding VAST ad.</span></span>
<span data-ttu-id="a0b68-297">Exempel: 4-schema som en tidig bindning stora ad //Download hello VAST filen om (! [ framework.adResolver downloadManifest: & manifestet withURL: [NSURL URLWithString: @ ”http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml”]]) {[self logFrameworkError];} else {adLinearTime.startTime = 7. adLinearTime.duration = 0;</span><span class="sxs-lookup"><span data-stu-id="a0b68-297">//Example:4 Schedule an early binding VAST ad //Download hello VAST file if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) { [self logFrameworkError]; } else { adLinearTime.startTime = 7; adLinearTime.duration = 0;</span></span>

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

<span data-ttu-id="a0b68-298">hello följande exempel visar hur tooinsert ad med grov klipp ut redigering (m)</span><span class="sxs-lookup"><span data-stu-id="a0b68-298">hello following sample shows how tooinsert an ad using Rough Cut Editing (RCE)</span></span>

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

<span data-ttu-id="a0b68-299">hello visas följande exempel hur tooschedule ad baljor.</span><span class="sxs-lookup"><span data-stu-id="a0b68-299">hello following example shows how tooschedule an ad pod.</span></span>

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

<span data-ttu-id="a0b68-300">följande exempel visar hur hello tooschedule en icke-Fäst halva sammanslagning ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-300">hello following example shows how tooschedule a non-sticky mid-roll ad.</span></span> <span data-ttu-id="a0b68-301">En icke-Fäst ad spelas endast när oavsett eventuella försöka få hello viewer utför.</span><span class="sxs-lookup"><span data-stu-id="a0b68-301">A non-sticky ad is only played once regardless of any seeking hello viewer performs.</span></span>

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

<span data-ttu-id="a0b68-302">följande exempel visar hur hello tooschedule en Fäst halva sammanslagning ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-302">hello following example shows how tooschedule a sticky mid-roll ad.</span></span> <span data-ttu-id="a0b68-303">Fäst ad visas varje gång hello anges på en hello video tidslinjen har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="a0b68-303">A sticky ad will be displayed each time hello specified point on hello video timeline is reached.</span></span>

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


<span data-ttu-id="a0b68-304">hello följande exempel visar hur tooschedule en efter sammanslagning ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-304">hello following sample shows how tooschedule a post-roll ad.</span></span>

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

<span data-ttu-id="a0b68-305">hello följande exempel visar hur tooschedule en före sammanslagning ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-305">hello following sample shows how tooschedule a pre-roll ad.</span></span>

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

<span data-ttu-id="a0b68-306">hello som följande exempel visar hur tooschedule halva sammanslagning överlägg ad.</span><span class="sxs-lookup"><span data-stu-id="a0b68-306">hello following sample shows how tooschedule a mid-roll overlay ad.</span></span>

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



## <a name="media-services-learning-paths"></a><span data-ttu-id="a0b68-307">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="a0b68-307">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="a0b68-308">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="a0b68-308">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="a0b68-309">Se även</span><span class="sxs-lookup"><span data-stu-id="a0b68-309">See Also</span></span>
[<span data-ttu-id="a0b68-310">Utveckla videospelarprogram</span><span class="sxs-lookup"><span data-stu-id="a0b68-310">Develop video player applications</span></span>](media-services-develop-video-players.md)

