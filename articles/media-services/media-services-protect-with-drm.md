---
title: aaaUsing PlayReady och/eller Widevine dynamic common kryptering | Microsoft Docs
description: "Microsoft Azure Media Services kan du toodeliver MPEG DASH, Smooth Streaming och Http Live Streaming (HLS) dataströmmar som skyddas med Microsoft PlayReady DRM. Du kan också toodelivery DASH krypterat med Widevine DRM. Det här avsnittet visar hur toodynamically kryptera med PlayReady och Widevine DRM."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 548d1a12-e2cb-45fe-9307-4ec0320567a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 0475e6ec80dcf39eb4e5c4ad4d17f821502951bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-playready-andor-widevine-dynamic-common-encryption"></a><span data-ttu-id="4212e-105">Använda PlayReady och/eller Widevine Dynamic Common Encryption</span><span class="sxs-lookup"><span data-stu-id="4212e-105">Using PlayReady and/or Widevine dynamic common encryption</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4212e-106">NET</span><span class="sxs-lookup"><span data-stu-id="4212e-106">.NET</span></span>](media-services-protect-with-drm.md)
> * [<span data-ttu-id="4212e-107">Java</span><span class="sxs-lookup"><span data-stu-id="4212e-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="4212e-108">PHP</span><span class="sxs-lookup"><span data-stu-id="4212e-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
>
>

<span data-ttu-id="4212e-109">Microsoft Azure Media Services kan du toodeliver MPEG DASH, Smooth Streaming och HTTP Live Streaming (HLS)-dataströmmar som skyddas med [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span><span class="sxs-lookup"><span data-stu-id="4212e-109">Microsoft Azure Media Services enables you toodeliver MPEG-DASH, Smooth Streaming, and HTTP-Live-Streaming (HLS) streams protected with [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span></span> <span data-ttu-id="4212e-110">Du kan också toodeliver krypterade DASH-dataströmmar med Widevine DRM-licenser.</span><span class="sxs-lookup"><span data-stu-id="4212e-110">It also enables you toodeliver encrypted DASH streams with Widevine DRM licenses.</span></span> <span data-ttu-id="4212e-111">Både PlayReady och Widevine krypteras enligt hello Common Encryption (ISO/IEC 23001 7 CENC)-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="4212e-111">Both PlayReady and Widevine are encrypted per hello Common Encryption (ISO/IEC 23001-7 CENC) specification.</span></span> <span data-ttu-id="4212e-112">Du kan använda [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (startar med hello version 3.5.1) eller REST API tooconfigure din AssetDeliveryConfiguration toouse Widevine.</span><span class="sxs-lookup"><span data-stu-id="4212e-112">You can use [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (starting with hello version 3.5.1) or REST API tooconfigure your AssetDeliveryConfiguration toouse Widevine.</span></span>

<span data-ttu-id="4212e-113">Media Services är en tjänst för att leverera PlayReady- och Widevine DRM-licenser.</span><span class="sxs-lookup"><span data-stu-id="4212e-113">Media Services provides a service for delivering PlayReady and Widevine DRM licenses.</span></span> <span data-ttu-id="4212e-114">Media Services tillhandahåller också API: er som gör att du kan konfigurera hello rättigheter och begränsningar som du vill använda för hello PlayReady eller Widevine DRM runtime tooenforce när en användare spelar upp skyddat innehåll.</span><span class="sxs-lookup"><span data-stu-id="4212e-114">Media Services also provides APIs that let you configure hello rights and restrictions that you want for hello PlayReady or Widevine DRM runtime tooenforce when a user plays back protected content.</span></span> <span data-ttu-id="4212e-115">När en användare begär ett DRM-skyddat innehåll, spelarappen hello en licens från hello AMS-Licenstjänsten.</span><span class="sxs-lookup"><span data-stu-id="4212e-115">When a user requests a DRM protected content, hello player application will request a license from hello AMS license service.</span></span> <span data-ttu-id="4212e-116">hello AMS-Licenstjänsten utfärdar en licens toohello spelare om den är auktoriserad.</span><span class="sxs-lookup"><span data-stu-id="4212e-116">hello AMS license service will issue a license toohello player if it is authorized.</span></span> <span data-ttu-id="4212e-117">En PlayReady eller Widevine-licens innehåller hello dekrypteringsnyckel som kan användas av hello klienten player toodecrypt och dataströmmen hello-innehåll.</span><span class="sxs-lookup"><span data-stu-id="4212e-117">A PlayReady or Widevine license contains hello decryption key that can be used by hello client player toodecrypt and stream hello content.</span></span>

<span data-ttu-id="4212e-118">Du kan också använda följande AMS-partner toohelp du leverera Widevine-licenser hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="4212e-118">You can also use hello following AMS partners toohelp you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span> <span data-ttu-id="4212e-119">Mer information finns i Integrering med [Axinom](media-services-axinom-integration.md) och [castLabs](media-services-castlabs-integration.md).</span><span class="sxs-lookup"><span data-stu-id="4212e-119">For more information, see: integration with [Axinom](media-services-axinom-integration.md) and [castLabs](media-services-castlabs-integration.md).</span></span>

<span data-ttu-id="4212e-120">Media Services stöder flera olika sätt att auktorisera användare som begär nycklar.</span><span class="sxs-lookup"><span data-stu-id="4212e-120">Media Services supports multiple ways of authorizing users who make key requests.</span></span> <span data-ttu-id="4212e-121">Hej innehållsnyckelns auktoriseringsprincip kan ha en eller flera auktoriseringsbegränsningar: öppen eller tokenbegränsning.</span><span class="sxs-lookup"><span data-stu-id="4212e-121">hello content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span> <span data-ttu-id="4212e-122">Hej tokenbegränsade principen måste åtföljas av en token som utfärdas av en säker säkerhetstokentjänst (STS).</span><span class="sxs-lookup"><span data-stu-id="4212e-122">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="4212e-123">Media Services stöder token i hello [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) format och [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT)-format.</span><span class="sxs-lookup"><span data-stu-id="4212e-123">Media Services supports tokens in hello [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) format and [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) format.</span></span> <span data-ttu-id="4212e-124">Mer information finns i Konfigurera hello innehållsnyckelns auktoriseringsprincip.</span><span class="sxs-lookup"><span data-stu-id="4212e-124">For more information, see Configure hello content key’s authorization policy.</span></span>

<span data-ttu-id="4212e-125">tootake nytta av dynamisk kryptering behöver du toohave en tillgång som innehåller en uppsättning med flera bithastigheter MP4-filer eller Smooth Streaming källfiler i multibithastighet.</span><span class="sxs-lookup"><span data-stu-id="4212e-125">tootake advantage of dynamic encryption, you need toohave an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="4212e-126">Du måste också tooconfigure hello leveransprinciperna för hello tillgången (beskrivs senare i det här avsnittet).</span><span class="sxs-lookup"><span data-stu-id="4212e-126">You also need tooconfigure hello delivery policies for hello asset (described later in this topic).</span></span> <span data-ttu-id="4212e-127">Sedan, baserat på hello-format som specificerats i hello strömnings-URL, servern för strömning på begäran av hello säkerställer hello dataströmmen levereras i hello-protokollet som du har valt.</span><span class="sxs-lookup"><span data-stu-id="4212e-127">Then, based on hello format specified in hello streaming URL, hello On-Demand Streaming server will ensure that hello stream is delivered in hello protocol you have chosen.</span></span> <span data-ttu-id="4212e-128">Därför behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services skapar och ger hello lämplig HTTP-respons baserat på varje begäran från en klient.</span><span class="sxs-lookup"><span data-stu-id="4212e-128">As a result, you only need toostore and pay for hello files in a single storage format and Media Services will build and serve hello appropriate HTTP response based on each request from a client.</span></span>

<span data-ttu-id="4212e-129">Det här avsnittet är användbara toodevelopers som arbetar på appar som levererar media som skyddas av flera DRM: er, som PlayReady och Widevine.</span><span class="sxs-lookup"><span data-stu-id="4212e-129">This topic would be useful toodevelopers that work on applications that deliver media protected with multiple DRMs, such as PlayReady and Widevine.</span></span> <span data-ttu-id="4212e-130">hello avsnittet visar hur tooconfigure hello Playreadys licensleveranstjänst med auktoriseringsprinciper så att endast auktoriserade klienter kan få PlayReady eller Widevine-licenser.</span><span class="sxs-lookup"><span data-stu-id="4212e-130">hello topic shows you how tooconfigure hello PlayReady license delivery service with authorization policies so that only authorized clients could receive PlayReady or Widevine licenses.</span></span> <span data-ttu-id="4212e-131">Den visar även hur toouse dynamiska kryptering med PlayReady eller Widevine DRM över DASH.</span><span class="sxs-lookup"><span data-stu-id="4212e-131">It also shows how toouse dynamic encryption encryption with PlayReady or Widevine DRM over DASH.</span></span>

>[!NOTE]
><span data-ttu-id="4212e-132">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="4212e-132">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="4212e-133">toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="4212e-133">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 

## <a name="download-sample"></a><span data-ttu-id="4212e-134">Hämta exempel</span><span class="sxs-lookup"><span data-stu-id="4212e-134">Download sample</span></span>
<span data-ttu-id="4212e-135">Du kan hämta hello-exempel som beskrivs i den här artikeln från [här](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).</span><span class="sxs-lookup"><span data-stu-id="4212e-135">You can download hello sample described in this article from [here](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).</span></span>

## <a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a><span data-ttu-id="4212e-136">Konfigurera Dynamic Common Encryption och DRM-licensleveranstjänster </span><span class="sxs-lookup"><span data-stu-id="4212e-136">Configuring Dynamic Common Encryption and DRM License Delivery Services</span></span>

<span data-ttu-id="4212e-137">hello följande är allmänna steg som behöver du tooperform när du skyddar dina tillgångar med PlayReady med hello Media Services licensleveranstjänst och även använder dynamisk kryptering.</span><span class="sxs-lookup"><span data-stu-id="4212e-137">hello following are general steps that you would need tooperform when protecting your assets with PlayReady, using hello Media Services license delivery service, and also using dynamic encryption.</span></span>

1. <span data-ttu-id="4212e-138">Skapa en tillgång och överföra filer till hello tillgång.</span><span class="sxs-lookup"><span data-stu-id="4212e-138">Create an asset and upload files into hello asset.</span></span>
2. <span data-ttu-id="4212e-139">Koda hello tillgång som innehåller hello filen toohello anpassningsbar bithastighet MP4-uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="4212e-139">Encode hello asset containing hello file toohello adaptive bitrate MP4 set.</span></span>
3. <span data-ttu-id="4212e-140">Skapa en innehållsnyckel och associera den med hello kodade tillgången.</span><span class="sxs-lookup"><span data-stu-id="4212e-140">Create a content key and associate it with hello encoded asset.</span></span> <span data-ttu-id="4212e-141">I Media Services innehåller innehållsnyckeln hello hello tillgångens krypteringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="4212e-141">In Media Services, hello content key contains hello asset’s encryption key.</span></span>
4. <span data-ttu-id="4212e-142">Konfigurera hello innehållsnyckelns auktoriseringsprincip.</span><span class="sxs-lookup"><span data-stu-id="4212e-142">Configure hello content key’s authorization policy.</span></span> <span data-ttu-id="4212e-143">Hej innehållsnyckelns auktoriseringsprincip måste konfigureras av dig och uppfyllas av hello klienten för hello innehåll viktiga toobe levererat toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="4212e-143">hello content key authorization policy must be configured by you and met by hello client in order for hello content key toobe delivered toohello client.</span></span>

    <span data-ttu-id="4212e-144">När du skapar hello innehållsnyckelns auktoriseringsprincip måste toospecify hello följande: leverans metod (PlayReady eller Widevine), begränsningar (öppen eller token) och information specifik toohello nyckelleveranstypen och som definierar hur hello nyckeln levereras toohello klienten ([PlayReady](media-services-playready-license-template-overview.md) eller [Widevine](media-services-widevine-license-template-overview.md) licensmall).</span><span class="sxs-lookup"><span data-stu-id="4212e-144">When creating hello content key authorization policy, you need toospecify hello following: delivery method (PlayReady or Widevine), restrictions (open or token), and information specific toohello key delivery type that defines how hello key is delivered toohello client ([PlayReady](media-services-playready-license-template-overview.md) or [Widevine](media-services-widevine-license-template-overview.md) license template).</span></span>

5. <span data-ttu-id="4212e-145">Konfigurera hello leveransprincipen för en tillgång.</span><span class="sxs-lookup"><span data-stu-id="4212e-145">Configure hello delivery policy for an asset.</span></span> <span data-ttu-id="4212e-146">hello konfigurationen för leveransprincipen omfattar: leveransprotokoll (till exempel MPEG DASH, HLS, Smooth Streaming eller alla), hello typen av dynamisk kryptering (till exempel Common Encryption) och PlayReady- eller URL för anskaffning av Widevine-licens.</span><span class="sxs-lookup"><span data-stu-id="4212e-146">hello delivery policy configuration includes: delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all), hello type of dynamic encryption (for example, Common Encryption), PlayReady or Widevine license acquisition URL.</span></span>

    <span data-ttu-id="4212e-147">Du kan använda olika principer tooeach protokoll på hello samma tillgång.</span><span class="sxs-lookup"><span data-stu-id="4212e-147">You could apply different policy tooeach protocol on hello same asset.</span></span> <span data-ttu-id="4212e-148">Du kan till exempel tillämpa PlayReady-kryptering tooSmooth/DASH och AES Envelope tooHLS.</span><span class="sxs-lookup"><span data-stu-id="4212e-148">For example, you could apply PlayReady encryption tooSmooth/DASH and AES Envelope tooHLS.</span></span> <span data-ttu-id="4212e-149">Alla protokoll som inte har definierats i en leveransprincip (exempelvis du lägga till en enda princip som endast anger HLS som hello protokoll) kommer att blockeras från strömning.</span><span class="sxs-lookup"><span data-stu-id="4212e-149">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as hello protocol) will be blocked from streaming.</span></span> <span data-ttu-id="4212e-150">hello undantag toothis är om du har någon tillgångsleveransprincip alls definierats.</span><span class="sxs-lookup"><span data-stu-id="4212e-150">hello exception toothis is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="4212e-151">Sedan tillåts alla protokoll i hello Rensa.</span><span class="sxs-lookup"><span data-stu-id="4212e-151">Then, all protocols will be allowed in hello clear.</span></span>

6. <span data-ttu-id="4212e-152">Skapa en OnDemand-positionerare i ordning tooget en strömnings-URL.</span><span class="sxs-lookup"><span data-stu-id="4212e-152">Create an OnDemand locator in order tooget a streaming URL.</span></span>

<span data-ttu-id="4212e-153">Du hittar ett komplett .NET-exempel hello slutet av hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4212e-153">You will find a complete .NET example at hello end of hello topic.</span></span>

<span data-ttu-id="4212e-154">hello följande bild visar hello arbetsflödet som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="4212e-154">hello following image demonstrates hello workflow described above.</span></span> <span data-ttu-id="4212e-155">Här används hello token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="4212e-155">Here hello token is used for authentication.</span></span>

![Skydda med PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

<span data-ttu-id="4212e-157">hello resten av det här avsnittet innehåller detaljerade förklaringar, kodexempel och länkar tootopics som visar hur tooachieve hello uppgifter som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="4212e-157">hello rest of this topic provides detailed explanations, code examples, and links tootopics that show you how tooachieve hello tasks described above.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="4212e-158">Aktuella begränsningar</span><span class="sxs-lookup"><span data-stu-id="4212e-158">Current limitations</span></span>
<span data-ttu-id="4212e-159">Om du lägger till eller uppdaterar en tillgångsleveransprincip måste du ta bort hello associerade lokaliserare (om sådan finns) och skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="4212e-159">If you add or update an asset delivery policy, you must delete hello associated locator (if any) and create a new locator.</span></span>

<span data-ttu-id="4212e-160">Begränsning vid kryptering med Widevine med Azure Media Services: för närvarande stöds inte nycklar för multiinnehåll.</span><span class="sxs-lookup"><span data-stu-id="4212e-160">Limitation when encrypting with Widevine with Azure Media Services: currently, multiple content keys are not supported.</span></span>

## <a name="create-an-asset-and-upload-files-into-hello-asset"></a><span data-ttu-id="4212e-161">Skapa en tillgång och överföra filer till hello tillgång</span><span class="sxs-lookup"><span data-stu-id="4212e-161">Create an asset and upload files into hello asset</span></span>
<span data-ttu-id="4212e-162">I ordning toomanage, koda och strömma videor, måste du först överföra innehållet till Microsoft Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="4212e-162">In order toomanage, encode, and stream your videos, you must first upload your content into Microsoft Azure Media Services.</span></span> <span data-ttu-id="4212e-163">När du har överfört är ditt innehåll lagras på ett säkert sätt i hello molnet för vidare bearbetning och strömning.</span><span class="sxs-lookup"><span data-stu-id="4212e-163">Once uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span>

<span data-ttu-id="4212e-164">Utförlig information finns i [Överföra filer till ett Media Services-konto](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="4212e-164">For detailed information, see [Upload Files into a Media Services account](media-services-dotnet-upload-files.md).</span></span>

## <a name="encode-hello-asset-containing-hello-file-toohello-adaptive-bitrate-mp4-set"></a><span data-ttu-id="4212e-165">Koda hello tillgång som innehåller hello filen toohello anpassningsbar bithastighet MP4-uppsättningen</span><span class="sxs-lookup"><span data-stu-id="4212e-165">Encode hello asset containing hello file toohello adaptive bitrate MP4 set</span></span>
<span data-ttu-id="4212e-166">Med dynamisk kryptering är allt du behöver toocreate en tillgång som innehåller en uppsättning med flera bithastigheter MP4-filer eller Smooth Streaming källfiler i multibithastighet.</span><span class="sxs-lookup"><span data-stu-id="4212e-166">With dynamic encryption all you need is toocreate an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="4212e-167">Sedan, baserat på hello format som anges i manifestet hello och Fragmentera begäran, hello strömning på begäran kommer servern att säkerställa att du har fått hello dataström i hello-protokollet som du har valt.</span><span class="sxs-lookup"><span data-stu-id="4212e-167">Then, based on hello specified format in hello manifest and fragment request, hello On-Demand Streaming server will ensure that you receive hello stream in hello protocol you have chosen.</span></span> <span data-ttu-id="4212e-168">Därför behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services-tjänsten skapar och ger hello lämplig respons baserat på begäranden från en klient.</span><span class="sxs-lookup"><span data-stu-id="4212e-168">As a result, you only need toostore and pay for hello files in single storage format and Media Services service will build and serve hello appropriate response based on requests from a client.</span></span> <span data-ttu-id="4212e-169">Mer information finns i hello [översikt över dynamisk paketering](media-services-dynamic-packaging-overview.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4212e-169">For more information, see hello [Dynamic Packaging Overview](media-services-dynamic-packaging-overview.md) topic.</span></span>

<span data-ttu-id="4212e-170">Mer information om hur tooencode, se [hur tooencode en tillgång med Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span><span class="sxs-lookup"><span data-stu-id="4212e-170">For instructions on how tooencode, see [How tooencode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>

## <span data-ttu-id="4212e-171"><a id="create_contentkey"></a>Skapa en innehållsnyckel och associera den med hello kodade tillgången</span><span class="sxs-lookup"><span data-stu-id="4212e-171"><a id="create_contentkey"></a>Create a content key and associate it with hello encoded asset</span></span>
<span data-ttu-id="4212e-172">I Media Services innehåller innehållsnyckeln för hello hello-nyckel som du vill tooencrypt en tillgång med.</span><span class="sxs-lookup"><span data-stu-id="4212e-172">In Media Services, hello content key contains hello key that you want tooencrypt an asset with.</span></span>

<span data-ttu-id="4212e-173">Utförlig information finns i [Skapa en innehållsnyckel](media-services-dotnet-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="4212e-173">For detailed information, see [Create content key](media-services-dotnet-create-contentkey.md).</span></span>

## <span data-ttu-id="4212e-174"><a id="configure_key_auth_policy"></a>Konfigurera hello innehållsnyckelns auktoriseringsprincip</span><span class="sxs-lookup"><span data-stu-id="4212e-174"><a id="configure_key_auth_policy"></a>Configure hello content key’s authorization policy</span></span>
<span data-ttu-id="4212e-175">Media Services stöder flera olika sätt att auktorisera användare som begär nycklar.</span><span class="sxs-lookup"><span data-stu-id="4212e-175">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="4212e-176">Hej innehållsnyckelns auktoriseringsprincip måste konfigureras av dig och uppfyllas av hello klienten (spelaren) för hello viktiga toobe levereras toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="4212e-176">hello content key authorization policy must be configured by you and met by hello client (player) in order for hello key toobe delivered toohello client.</span></span> <span data-ttu-id="4212e-177">Hej innehållsnyckelns auktoriseringsprincip kan ha en eller flera auktoriseringsbegränsningar: öppen eller tokenbegränsning.</span><span class="sxs-lookup"><span data-stu-id="4212e-177">hello content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span>

<span data-ttu-id="4212e-178">Mer information finns i [Konfigurera  innehållsnyckelns auktoriseringsprincip](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).</span><span class="sxs-lookup"><span data-stu-id="4212e-178">For detailed information, see [Configure Content Key Authorization Policy](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).</span></span>

## <span data-ttu-id="4212e-179"><a id="configure_asset_delivery_policy"></a>Konfigurera tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="4212e-179"><a id="configure_asset_delivery_policy"></a>Configure asset delivery policy</span></span>
<span data-ttu-id="4212e-180">Konfigurera hello leveransprincipen för din tillgång.</span><span class="sxs-lookup"><span data-stu-id="4212e-180">Configure hello delivery policy for your asset.</span></span> <span data-ttu-id="4212e-181">Vissa saker som hello tillgången konfigurationen för leveransprincipen omfattar:</span><span class="sxs-lookup"><span data-stu-id="4212e-181">Some things that hello asset delivery policy configuration includes:</span></span>

* <span data-ttu-id="4212e-182">hello DRM-licens förvärv URL.</span><span class="sxs-lookup"><span data-stu-id="4212e-182">hello DRM license acquisition URL.</span></span>
* <span data-ttu-id="4212e-183">hello protokollet för tillgångsleverans (till exempel MPEG DASH, HLS, Smooth Streaming eller alla).</span><span class="sxs-lookup"><span data-stu-id="4212e-183">hello asset delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all).</span></span>
* <span data-ttu-id="4212e-184">hello typen av dynamisk kryptering (i det här fallet Common Encryption).</span><span class="sxs-lookup"><span data-stu-id="4212e-184">hello type of dynamic encryption (in this case, Common Encryption).</span></span>

<span data-ttu-id="4212e-185">Detaljerad information finns i [Konfigurera tillgångsleveransprincip ](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="4212e-185">For detailed information, see [Configure asset delivery policy ](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <span data-ttu-id="4212e-186"><a id="create_locator"></a>Skapa en OnDemand-strömning positionerare ordning tooget en strömnings-URL</span><span class="sxs-lookup"><span data-stu-id="4212e-186"><a id="create_locator"></a>Create an OnDemand streaming locator in order tooget a streaming URL</span></span>
<span data-ttu-id="4212e-187">Du behöver tooprovide användaren hello strömmande URL för Smooth, DASH eller HLS.</span><span class="sxs-lookup"><span data-stu-id="4212e-187">You will need tooprovide your user with hello streaming URL for Smooth, DASH or HLS.</span></span>

> [!NOTE]
> <span data-ttu-id="4212e-188">Om du lägger till eller uppdaterar din tillgångs leveransprincip måste du ta bort en befintlig lokaliserare (om sådan finns) och skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="4212e-188">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
>
>

<span data-ttu-id="4212e-189">Anvisningar för hur toopublish en tillgång och skapa en strömnings-URL, se [skapa en strömnings-URL](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="4212e-189">For instructions on how toopublish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="get-a-test-token"></a><span data-ttu-id="4212e-190">Hämta en testtoken</span><span class="sxs-lookup"><span data-stu-id="4212e-190">Get a test token</span></span>
<span data-ttu-id="4212e-191">Hämta en testtoken baserat på hello tokenbegränsningar som användes för hello innehållsnyckelns auktoriseringsprincip.</span><span class="sxs-lookup"><span data-stu-id="4212e-191">Get a test token based on hello token restriction that was used for hello key authorization policy.</span></span>

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on hello data in hello given TokenRestrictionTemplate.
    //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
    //so you have tooadd it in front of hello token string.
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);


<span data-ttu-id="4212e-192">Du kan använda hello [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest strömmen.</span><span class="sxs-lookup"><span data-stu-id="4212e-192">You can use hello [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest your stream.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="4212e-193">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="4212e-193">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="4212e-194">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="4212e-194">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="4212e-195">Lägg till följande element för hello**appSettings** definieras i filen app.config:</span><span class="sxs-lookup"><span data-stu-id="4212e-195">Add hello following elements too**appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a><span data-ttu-id="4212e-196">Exempel</span><span class="sxs-lookup"><span data-stu-id="4212e-196">Example</span></span>

<span data-ttu-id="4212e-197">hello följande exempel visas de funktioner som infördes i Azure Media Services SDK för .net-Version 3.5.2 (särskilt hello möjlighet toodefine en Widevine licens mall och begära en Widevine-licens från Azure Media Services).</span><span class="sxs-lookup"><span data-stu-id="4212e-197">hello following sample demonstrates functionality that was introduced in Azure Media Services SDK for .Net -Version 3.5.2 (specifically, hello ability toodefine a Widevine license template and request a Widevine license from Azure Media Services).</span></span>

<span data-ttu-id="4212e-198">Skriv över hello koden i Program.cs-filen med hello koden som visas i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4212e-198">Overwrite hello code in your Program.cs file with hello code shown in this section.</span></span>

>[!NOTE]
><span data-ttu-id="4212e-199">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="4212e-199">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="4212e-200">Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer).</span><span class="sxs-lookup"><span data-stu-id="4212e-200">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="4212e-201">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4212e-201">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="4212e-202">Se till att tooupdate variabler toopoint toofolders där dina indatafiler finns.</span><span class="sxs-lookup"><span data-stu-id="4212e-202">Make sure tooupdate variables toopoint toofolders where your input files are located.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
    using Newtonsoft.Json;

    namespace DynamicEncryptionWithDRM
    {
        class Program
        {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        private static readonly Uri _sampleAudience =
            new Uri(ConfigurationManager.AppSettings["Audience"]);

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            bool tokenRestriction = false;
            string tokenTemplateString = null;

            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
            Console.WriteLine("Uploaded asset: {0}", asset.Id);

            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);

            IContentKey key = CreateCommonTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
            Console.WriteLine();

            if (tokenRestriction)
            tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
            else
            AddOpenAuthorizationPolicy(key);

            Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
            Console.WriteLine();

            CreateAssetDeliveryPolicy(encodedAsset, key);
            Console.WriteLine("Created asset delivery policy. \n");
            Console.WriteLine();

            if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
            {
            // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
            // back into a TokenRestrictionTemplate class instance.
            TokenRestrictionTemplate tokenTemplate =
                TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

            // Generate a test token based on hello hello data in hello given TokenRestrictionTemplate.
            // Note, you need toopass hello key id Guid because we specified
            // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                        DateTime.UtcNow.AddDays(365));
            Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use hello http://amsplayer.azurewebsites.net/azuremediaplayer.html player tootest streams.
            // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);

            Console.ReadLine();
        }

        static public IAsset UploadFileAndCreateAsset(string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
            Console.WriteLine("File does not exist.");
            return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
        {
            var encodingPreset = "Adaptive Streaming";

            IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} too{1}",
                        inputAsset.Name,
                        encodingPreset));

            var mediaProcessors =
            _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();

            var latestMediaProcessor =
            mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();

            ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
            encodeTask.InputAssets.Add(inputAsset);
            encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }


        static public IContentKey CreateCommonTypeContentKey(IAsset asset)
        {

            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryption);

            // Associate hello key with hello asset.
            asset.ContentKeys.Add(key);

            return key;
        }

        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {

            // Create ContentKeyAuthorizationPolicy with Open restrictions
            // and create authorization policy          

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Open",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                    Requirements = null
                }
                };

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with no restrictions").
                Result;


            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
            // Associate hello content key authorization policy with hello content key.
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString,
                }
                };

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);

            // Associate hello content key authorization policy with hello content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        static private string GenerateTokenRequirements()
        {
            TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

            template.PrimaryVerificationKey = new SymmetricVerificationKey();
            template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
            template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

            return TokenRestrictionTemplateSerializer.Serialize(template);
        }

        static private string ConfigurePlayReadyLicenseTemplate()
        {
            // hello following code configures PlayReady License Template using .NET classes
            // and returns hello XML string.

            //hello PlayReadyLicenseResponseTemplate class represents hello template for hello response sent back toohello end user.
            //It contains a field for a custom data string between hello license server and hello application
            //(may be useful for custom app logic) as well as a list of one or more license templates.
            PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

            // hello PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
            // toobe returned toohello end users.
            //It contains hello data on hello content key in hello license and any rights or restrictions toobe
            //enforced by hello PlayReady DRM runtime when using hello content key.
            PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
            //Configure whether hello license is persistent (saved in persistent storage on hello client)
            //or non-persistent (only held in memory while hello player is using hello license).  
            licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

            // AllowTestDevices controls whether test devices can use hello license or not.  
            // If true, hello MinimumSecurityLevel property of hello license
            // is set too150.  If false (hello default), hello MinimumSecurityLevel property of hello license is set too2000.
            licenseTemplate.AllowTestDevices = true;

            // You can also configure hello Play Right in hello PlayReady license by using hello PlayReadyPlayRight class.
            // It grants hello user hello ability tooplayback hello content subject toohello zero or more restrictions
            // configured in hello license and on hello PlayRight itself (for playback specific policy).
            // Much of hello policy on hello PlayRight has toodo with output restrictions
            // which control hello types of outputs that hello content can be played over and
            // any restrictions that must be put in place when using a given output.
            // For example, if hello DigitalVideoOnlyContentRestriction is enabled,
            //then hello DRM runtime will only allow hello video toobe displayed over digital outputs
            //(analog video outputs won’t be allowed toopass hello content).

            //IMPORTANT: These types of restrictions can be very powerful but can also affect hello consumer experience.
            // If hello output protections are configured too restrictive,
            // hello content might be unplayable on some clients. For more information, see hello PlayReady Compliance Rules document.

            // For example:
            //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

            responseTemplate.LicenseTemplates.Add(licenseTemplate);

            return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
        }

        private static string ConfigureWidevineLicenseTemplate()
        {
            var template = new WidevineMessage
            {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                    new ContentKeySpecs
                    {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                    }
                },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
            };

            string configuration = JsonConvert.SerializeObject(template);
            return configuration;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            // Get hello PlayReady license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

            // GetKeyDeliveryUrl for Widevine attaches hello KID toohello URL.
            // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
            // hello WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption
            // tooappend /? KID =< keyId > toohello end of hello url when creating hello manifest.
            // As a result Widevine license acquisition URL will have KID appended twice,
            // so we need tooremove hello KID that in hello URL when we call GetKeyDeliveryUrl.

            Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
            UriBuilder uriBuilder = new UriBuilder(widevineUrl);
            uriBuilder.Query = String.Empty;
            widevineUrl = uriBuilder.Uri;

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                    {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}

            };

            // In this case we only specify Dash streaming protocol in hello delivery policy,
            // All other protocols will be blocked from streaming.
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }

        /// <summary>
        /// Gets hello streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference toohello streaming manifest file from hello  
            // collection of files in hello asset.

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator toohello streaming content on an origin.
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL toohello manifest file.
            return originLocator.Path + assetFile.Name;
        }

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
        }

        static private byte[] GetRandomBuffer(int length)
        {
            var returnValue = new byte[length];

            using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
            {
            rng.GetBytes(returnValue);
            }

            return returnValue;
        }
        }
    }


## <a name="next-step"></a><span data-ttu-id="4212e-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4212e-203">Next step</span></span>
<span data-ttu-id="4212e-204">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="4212e-204">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4212e-205">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="4212e-205">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="4212e-206">Se även</span><span class="sxs-lookup"><span data-stu-id="4212e-206">See also</span></span>
[<span data-ttu-id="4212e-207">CENC med Multi-DRM och Access Control</span><span class="sxs-lookup"><span data-stu-id="4212e-207">CENC with Multi-DRM and Access Control</span></span>](media-services-cenc-with-multidrm-access-control.md)

[<span data-ttu-id="4212e-208">Konfigurera Widevine-paketering med AMS</span><span class="sxs-lookup"><span data-stu-id="4212e-208">Configure Widevine packaging with AMS</span></span>](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[<span data-ttu-id="4212e-209">Vi ber att få presentera Google Widevines licensleveranstjänster i Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="4212e-209">Announcing Google Widevine license delivery services in Azure Media Services</span></span>](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
