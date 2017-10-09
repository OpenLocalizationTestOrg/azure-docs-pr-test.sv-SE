---
title: "aaaUsing AES-128 dynamisk kryptering och nyckeln leverans av tjänsten | Microsoft Docs"
description: "Microsoft Azure Media Services kan du toodeliver ditt innehåll som krypterats med AES 128-bitars krypteringsnycklar. Media Services tillhandahåller också hello nyckeln Delivery service som ger dig kryptering nycklar tooauthorized användare. Det här avsnittet visar hur toodynamically kryptera med AES-128 och använda hello viktiga delivery service."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: cb1b413ec2ba79f7437464099cf72236ab93f312
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a><span data-ttu-id="a80b6-105">Med hjälp av AES-128 dynamisk kryptering och viktiga leveranser</span><span class="sxs-lookup"><span data-stu-id="a80b6-105">Using AES-128 dynamic encryption and key delivery service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a80b6-106">.NET</span><span class="sxs-lookup"><span data-stu-id="a80b6-106">.NET</span></span>](media-services-protect-with-aes128.md)
> * [<span data-ttu-id="a80b6-107">Java</span><span class="sxs-lookup"><span data-stu-id="a80b6-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="a80b6-108">PHP</span><span class="sxs-lookup"><span data-stu-id="a80b6-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="a80b6-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="a80b6-109">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="a80b6-110">Se [detta](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video för en översikt över hur tooprotect Media innehåll med AES-kryptering.</span><span class="sxs-lookup"><span data-stu-id="a80b6-110">See [this](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video for an overview of how tooprotect your Media Content with AES encryption.</span></span>
> 
> 

<span data-ttu-id="a80b6-111">Microsoft Azure Media Services kan du toodeliver Http-Live-Streaming (HLS) och Smooth strömmar som krypterats med Standard AES (Advanced Encryption) (med 128-bitars krypteringsnycklar).</span><span class="sxs-lookup"><span data-stu-id="a80b6-111">Microsoft Azure Media Services enables you toodeliver Http-Live-Streaming (HLS) and Smooth Streams encrypted with Advanced Encryption Standard (AES) (using 128-bit encryption keys).</span></span> <span data-ttu-id="a80b6-112">Media Services tillhandahåller också hello nyckeln Delivery service som ger dig kryptering nycklar tooauthorized användare.</span><span class="sxs-lookup"><span data-stu-id="a80b6-112">Media Services also provides hello Key Delivery service that delivers encryption keys tooauthorized users.</span></span> <span data-ttu-id="a80b6-113">Om du vill använda för Media Services tooencrypt en tillgång, behöver tooassociate en krypteringsnyckel med hello tillgången och även konfigurera auktoriseringsprinciper för hello nyckeln.</span><span class="sxs-lookup"><span data-stu-id="a80b6-113">If you want for Media Services tooencrypt an asset, you need tooassociate an encryption key with hello asset and also configure authorization policies for hello key.</span></span> <span data-ttu-id="a80b6-114">När en dataströmmen har begärts av en spelare, använder Media Services hello angetts viktiga toodynamically kryptera ditt innehåll med hjälp av AES-kryptering.</span><span class="sxs-lookup"><span data-stu-id="a80b6-114">When a stream is requested by a player, Media Services uses hello specified key toodynamically encrypt your content using AES encryption.</span></span> <span data-ttu-id="a80b6-115">toodecrypt hello dataströmmen hello player begär hello nyckeln från hello viktiga leverans av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a80b6-115">toodecrypt hello stream, hello player will request hello key from hello key delivery service.</span></span> <span data-ttu-id="a80b6-116">toodecide om huruvida användaren hello är godkänd tooget hello nyckel, hello tjänsten utvärderar hello auktoriseringsprinciper som du angav för hello nyckeln.</span><span class="sxs-lookup"><span data-stu-id="a80b6-116">toodecide whether or not hello user is authorized tooget hello key, hello service evaluates hello authorization policies that you specified for hello key.</span></span>

<span data-ttu-id="a80b6-117">Media Services stöder flera olika sätt att auktorisera användare som begär nycklar.</span><span class="sxs-lookup"><span data-stu-id="a80b6-117">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="a80b6-118">Hej innehållsnyckelns auktoriseringsprincip kan ha en eller flera auktoriseringsbegränsningar: öppen eller tokenbegränsning.</span><span class="sxs-lookup"><span data-stu-id="a80b6-118">hello content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span> <span data-ttu-id="a80b6-119">Hej tokenbegränsade principen måste åtföljas av en token som utfärdas av en säker säkerhetstokentjänst (STS).</span><span class="sxs-lookup"><span data-stu-id="a80b6-119">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="a80b6-120">Media Services stöder token i hello [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) format och [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT)-format.</span><span class="sxs-lookup"><span data-stu-id="a80b6-120">Media Services supports tokens in hello [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) format and [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) format.</span></span> <span data-ttu-id="a80b6-121">Mer information finns i [konfigurera hello innehållsnyckelns auktoriseringsprincip](media-services-protect-with-aes128.md#configure_key_auth_policy).</span><span class="sxs-lookup"><span data-stu-id="a80b6-121">For more information, see [Configure hello content key’s authorization policy](media-services-protect-with-aes128.md#configure_key_auth_policy).</span></span>

<span data-ttu-id="a80b6-122">tootake nytta av dynamisk kryptering behöver du toohave en tillgång som innehåller en uppsättning med flera bithastigheter MP4-filer eller Smooth Streaming källfiler i multibithastighet.</span><span class="sxs-lookup"><span data-stu-id="a80b6-122">tootake advantage of dynamic encryption, you need toohave an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="a80b6-123">Du måste också tooconfigure hello leveransprincipen för hello tillgången (beskrivs senare i det här avsnittet).</span><span class="sxs-lookup"><span data-stu-id="a80b6-123">You also need tooconfigure hello delivery policy for hello asset (described later in this topic).</span></span> <span data-ttu-id="a80b6-124">Sedan, baserat på hello-format som specificerats i hello strömnings-URL, servern för strömning på begäran av hello säkerställer hello dataströmmen levereras i hello-protokollet som du har valt.</span><span class="sxs-lookup"><span data-stu-id="a80b6-124">Then, based on hello format specified in hello streaming URL, hello On-Demand Streaming server will ensure that hello stream is delivered in hello protocol you have chosen.</span></span> <span data-ttu-id="a80b6-125">Därför behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services-tjänsten skapar och ger hello lämplig respons baserat på begäranden från en klient.</span><span class="sxs-lookup"><span data-stu-id="a80b6-125">As a result, you only need toostore and pay for hello files in single storage format and Media Services service will build and serve hello appropriate response based on requests from a client.</span></span>

<span data-ttu-id="a80b6-126">Det här avsnittet är användbara toodevelopers som arbetar på appar som levererar media som är skyddade.</span><span class="sxs-lookup"><span data-stu-id="a80b6-126">This topic would be useful toodevelopers that work on applications that deliver protected media.</span></span> <span data-ttu-id="a80b6-127">hello avsnittet visar hur tooconfigure hello viktiga leverans av tjänsten med auktoriseringsprinciper så att endast auktoriserade klienter kan få hello krypteringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="a80b6-127">hello topic shows you how tooconfigure hello key delivery service with authorization policies so that only authorized clients could receive hello encryption keys.</span></span> <span data-ttu-id="a80b6-128">Den visar även hur toouse dynamisk kryptering.</span><span class="sxs-lookup"><span data-stu-id="a80b6-128">It also shows how toouse dynamic encryption.</span></span>


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a><span data-ttu-id="a80b6-129">AES-128 dynamisk kryptering och viktiga Delivery Service arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="a80b6-129">AES-128 Dynamic Encryption and Key Delivery Service Workflow</span></span>

<span data-ttu-id="a80b6-130">hello följande är allmänna steg att tooperform för att kryptera dina tillgångar med AES hello Media Services viktiga delivery service, samt även använder dynamisk kryptering.</span><span class="sxs-lookup"><span data-stu-id="a80b6-130">hello following are general steps that you would need tooperform when encrypting your assets with AES, using hello Media Services key delivery service, and also using dynamic encryption.</span></span>

1. <span data-ttu-id="a80b6-131">[Skapa en tillgång och överföra filer till tillgången hello](media-services-protect-with-aes128.md#create_asset).</span><span class="sxs-lookup"><span data-stu-id="a80b6-131">[Create an asset and upload files into hello asset](media-services-protect-with-aes128.md#create_asset).</span></span>
2. <span data-ttu-id="a80b6-132">[Koda hello tillgång som innehåller hello filen toohello anpassningsbar bithastighet MP4-uppsättningen](media-services-protect-with-aes128.md#encode_asset).</span><span class="sxs-lookup"><span data-stu-id="a80b6-132">[Encode hello asset containing hello file toohello adaptive bitrate MP4 set](media-services-protect-with-aes128.md#encode_asset).</span></span>
3. <span data-ttu-id="a80b6-133">[Skapa en innehållsnyckel och associera den med hello kodade tillgången](media-services-protect-with-aes128.md#create_contentkey).</span><span class="sxs-lookup"><span data-stu-id="a80b6-133">[Create a content key and associate it with hello encoded asset](media-services-protect-with-aes128.md#create_contentkey).</span></span> <span data-ttu-id="a80b6-134">I Media Services innehåller innehållsnyckeln hello hello tillgångens krypteringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="a80b6-134">In Media Services, hello content key contains hello asset’s encryption key.</span></span>
4. <span data-ttu-id="a80b6-135">[Konfigurera hello innehållsnyckelns auktoriseringsprincip](media-services-protect-with-aes128.md#configure_key_auth_policy).</span><span class="sxs-lookup"><span data-stu-id="a80b6-135">[Configure hello content key’s authorization policy](media-services-protect-with-aes128.md#configure_key_auth_policy).</span></span> <span data-ttu-id="a80b6-136">Hej innehållsnyckelns auktoriseringsprincip måste konfigureras av dig och uppfyllas av hello klienten för hello innehåll viktiga toobe levererat toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="a80b6-136">hello content key authorization policy must be configured by you and met by hello client in order for hello content key toobe delivered toohello client.</span></span>
5. <span data-ttu-id="a80b6-137">[Konfigurera hello leveransprincipen för en tillgång](media-services-protect-with-aes128.md#configure_asset_delivery_policy).</span><span class="sxs-lookup"><span data-stu-id="a80b6-137">[Configure hello delivery policy for an asset](media-services-protect-with-aes128.md#configure_asset_delivery_policy).</span></span> <span data-ttu-id="a80b6-138">hello konfigurationen för leveransprincipen omfattar: URL för anskaffning och initiering Vector (IV) (AES 128 kräver hello samma IV toobe angavs när kryptera och dekryptera), leveransprotokoll (till exempel MPEG DASH, HLS, Smooth Streaming eller alla), hello typ av dynamisk kryptering (till exempel kuvert eller ingen dynamisk kryptering).</span><span class="sxs-lookup"><span data-stu-id="a80b6-138">hello delivery policy configuration includes: key acquisition URL and Initialization Vector (IV) (AES 128 requires hello same IV toobe supplied when encrypting and decrypting), delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all), hello type of dynamic encryption (for example, envelope or no dynamic encryption).</span></span>

    <span data-ttu-id="a80b6-139">Du kan använda olika principer tooeach protokoll på hello samma tillgång.</span><span class="sxs-lookup"><span data-stu-id="a80b6-139">You could apply different policy tooeach protocol on hello same asset.</span></span> <span data-ttu-id="a80b6-140">Du kan till exempel tillämpa PlayReady-kryptering tooSmooth/DASH och AES Envelope tooHLS.</span><span class="sxs-lookup"><span data-stu-id="a80b6-140">For example, you could apply PlayReady encryption tooSmooth/DASH and AES Envelope tooHLS.</span></span> <span data-ttu-id="a80b6-141">Alla protokoll som inte har definierats i en leveransprincip (exempelvis du lägga till en enda princip som endast anger HLS som hello protokoll) kommer att blockeras från strömning.</span><span class="sxs-lookup"><span data-stu-id="a80b6-141">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as hello protocol) will be blocked from streaming.</span></span> <span data-ttu-id="a80b6-142">hello undantag toothis är om du har någon tillgångsleveransprincip alls definierats.</span><span class="sxs-lookup"><span data-stu-id="a80b6-142">hello exception toothis is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="a80b6-143">Sedan tillåts alla protokoll i hello Rensa.</span><span class="sxs-lookup"><span data-stu-id="a80b6-143">Then, all protocols will be allowed in hello clear.</span></span>

6. <span data-ttu-id="a80b6-144">[Skapa en OnDemand-positionerare](media-services-protect-with-aes128.md#create_locator) ordning tooget en strömnings-URL.</span><span class="sxs-lookup"><span data-stu-id="a80b6-144">[Create an OnDemand locator](media-services-protect-with-aes128.md#create_locator) in order tooget a streaming URL.</span></span>

<span data-ttu-id="a80b6-145">hello avsnittet visar också [hur ett klientprogram kan begära en nyckel från hello viktiga leverans tjänsten](media-services-protect-with-aes128.md#client_request).</span><span class="sxs-lookup"><span data-stu-id="a80b6-145">hello topic also shows [how a client application can request a key from hello key delivery service](media-services-protect-with-aes128.md#client_request).</span></span>

<span data-ttu-id="a80b6-146">Du hittar ett komplett .NET [exempel](media-services-protect-with-aes128.md#example) hello slutet av hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a80b6-146">You will find a complete .NET [example](media-services-protect-with-aes128.md#example) at hello end of hello topic.</span></span>

<span data-ttu-id="a80b6-147">hello följande bild visar hello arbetsflödet som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="a80b6-147">hello following image demonstrates hello workflow described above.</span></span> <span data-ttu-id="a80b6-148">Här används hello token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="a80b6-148">Here hello token is used for authentication.</span></span>

![Skydda med AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

<span data-ttu-id="a80b6-150">hello resten av det här avsnittet innehåller detaljerade förklaringar, kodexempel och länkar tootopics som visar hur tooachieve hello uppgifter som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="a80b6-150">hello rest of this topic provides detailed explanations, code examples, and links tootopics that show you how tooachieve hello tasks described above.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="a80b6-151">Aktuella begränsningar</span><span class="sxs-lookup"><span data-stu-id="a80b6-151">Current limitations</span></span>
<span data-ttu-id="a80b6-152">Om du lägger till eller uppdaterar din tillgångs leveransprincip måste du ta bort en befintlig lokaliserare (om sådan finns) och skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="a80b6-152">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>

## <span data-ttu-id="a80b6-153"><a id="create_asset"></a>Skapa en tillgång och överföra filer till hello tillgång</span><span class="sxs-lookup"><span data-stu-id="a80b6-153"><a id="create_asset"></a>Create an asset and upload files into hello asset</span></span>
<span data-ttu-id="a80b6-154">I ordning toomanage, koda och strömma videor, måste du först överföra innehållet till Microsoft Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="a80b6-154">In order toomanage, encode, and stream your videos, you must first upload your content into Microsoft Azure Media Services.</span></span> <span data-ttu-id="a80b6-155">När du har överfört är ditt innehåll lagras på ett säkert sätt i hello molnet för vidare bearbetning och strömning.</span><span class="sxs-lookup"><span data-stu-id="a80b6-155">Once uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span> 

<span data-ttu-id="a80b6-156">Utförlig information finns i [Överföra filer till ett Media Services-konto](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="a80b6-156">For detailed information, see [Upload Files into a Media Services account](media-services-dotnet-upload-files.md).</span></span>

## <span data-ttu-id="a80b6-157"><a id="encode_asset"></a>Koda hello tillgång som innehåller hello filen toohello anpassningsbar bithastighet MP4-uppsättningen</span><span class="sxs-lookup"><span data-stu-id="a80b6-157"><a id="encode_asset"></a>Encode hello asset containing hello file toohello adaptive bitrate MP4 set</span></span>
<span data-ttu-id="a80b6-158">Med dynamisk kryptering är allt du behöver toocreate en tillgång som innehåller en uppsättning med flera bithastigheter MP4-filer eller Smooth Streaming källfiler i multibithastighet.</span><span class="sxs-lookup"><span data-stu-id="a80b6-158">With dynamic encryption all you need is toocreate an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="a80b6-159">Sedan, baserat på hello format som anges i manifestet hello eller Fragmentera begäran, hello strömning på begäran kommer servern att säkerställa att du har fått hello dataström i hello-protokollet som du har valt.</span><span class="sxs-lookup"><span data-stu-id="a80b6-159">Then, based on hello specified format in hello manifest or fragment request, hello On-Demand Streaming server will ensure that you receive hello stream in hello protocol you have chosen.</span></span> <span data-ttu-id="a80b6-160">Därför behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services-tjänsten skapar och ger hello lämplig respons baserat på begäranden från en klient.</span><span class="sxs-lookup"><span data-stu-id="a80b6-160">As a result, you only need toostore and pay for hello files in single storage format and Media Services service will build and serve hello appropriate response based on requests from a client.</span></span> <span data-ttu-id="a80b6-161">Mer information finns i hello [översikt över dynamisk paketering](media-services-dynamic-packaging-overview.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a80b6-161">For more information, see hello [Dynamic Packaging Overview](media-services-dynamic-packaging-overview.md) topic.</span></span>

>[!NOTE]
><span data-ttu-id="a80b6-162">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="a80b6-162">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="a80b6-163">toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="a80b6-163">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 
>
><span data-ttu-id="a80b6-164">Dessutom toobe kan toouse dynamisk paketering och dynamisk kryptering din tillgång måste innehålla en uppsättning MP4s med anpassningsbar bithastighet eller Smooth Streaming-filer.</span><span class="sxs-lookup"><span data-stu-id="a80b6-164">Also, toobe able toouse dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>

<span data-ttu-id="a80b6-165">Mer information om hur tooencode, se [hur tooencode en tillgång med Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span><span class="sxs-lookup"><span data-stu-id="a80b6-165">For instructions on how tooencode, see [How tooencode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>

## <span data-ttu-id="a80b6-166"><a id="create_contentkey"></a>Skapa en innehållsnyckel och associera den med hello kodade tillgången</span><span class="sxs-lookup"><span data-stu-id="a80b6-166"><a id="create_contentkey"></a>Create a content key and associate it with hello encoded asset</span></span>
<span data-ttu-id="a80b6-167">I Media Services innehåller innehållsnyckeln för hello hello-nyckel som du vill tooencrypt en tillgång med.</span><span class="sxs-lookup"><span data-stu-id="a80b6-167">In Media Services, hello content key contains hello key that you want tooencrypt an asset with.</span></span>

<span data-ttu-id="a80b6-168">Utförlig information finns i [Skapa en innehållsnyckel](media-services-dotnet-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="a80b6-168">For detailed information, see [Create content key](media-services-dotnet-create-contentkey.md).</span></span>

## <span data-ttu-id="a80b6-169"><a id="configure_key_auth_policy"></a>Konfigurera hello innehållsnyckelns auktoriseringsprincip</span><span class="sxs-lookup"><span data-stu-id="a80b6-169"><a id="configure_key_auth_policy"></a>Configure hello content key’s authorization policy</span></span>
<span data-ttu-id="a80b6-170">Media Services stöder flera olika sätt att auktorisera användare som begär nycklar.</span><span class="sxs-lookup"><span data-stu-id="a80b6-170">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="a80b6-171">Hej innehållsnyckelns auktoriseringsprincip måste konfigureras av dig och uppfyllas av hello klienten (spelaren) för hello viktiga toobe levereras toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="a80b6-171">hello content key authorization policy must be configured by you and met by hello client (player) in order for hello key toobe delivered toohello client.</span></span> <span data-ttu-id="a80b6-172">Hej innehållsnyckelns auktoriseringsprincip kan ha en eller flera auktoriseringsbegränsningar: öppen och token begränsning eller en IP-begränsning.</span><span class="sxs-lookup"><span data-stu-id="a80b6-172">hello content key authorization policy could have one or more authorization restrictions: open, token restriction, or IP restriction.</span></span>

<span data-ttu-id="a80b6-173">Mer information finns i [Konfigurera  innehållsnyckelns auktoriseringsprincip](media-services-dotnet-configure-content-key-auth-policy.md).</span><span class="sxs-lookup"><span data-stu-id="a80b6-173">For detailed information, see [Configure Content Key Authorization Policy](media-services-dotnet-configure-content-key-auth-policy.md).</span></span>

## <span data-ttu-id="a80b6-174"><a id="configure_asset_delivery_policy"></a>Konfigurera tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="a80b6-174"><a id="configure_asset_delivery_policy"></a>Configure asset delivery policy</span></span>
<span data-ttu-id="a80b6-175">Konfigurera hello leveransprincipen för din tillgång.</span><span class="sxs-lookup"><span data-stu-id="a80b6-175">Configure hello delivery policy for your asset.</span></span> <span data-ttu-id="a80b6-176">Vissa saker som hello tillgången konfigurationen för leveransprincipen omfattar:</span><span class="sxs-lookup"><span data-stu-id="a80b6-176">Some things that hello asset delivery policy configuration includes:</span></span>

* <span data-ttu-id="a80b6-177">URL för anskaffning av hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="a80b6-177">hello Key acquisition URL.</span></span> 
* <span data-ttu-id="a80b6-178">hello initieringen Vector (IV) toouse för hello kuvert kryptering.</span><span class="sxs-lookup"><span data-stu-id="a80b6-178">hello Initialization Vector (IV) toouse for hello envelope encryption.</span></span> <span data-ttu-id="a80b6-179">AES-128 kräver hello samma IV toobe angavs när kryptera och dekryptera.</span><span class="sxs-lookup"><span data-stu-id="a80b6-179">AES 128 requires hello same IV toobe supplied when encrypting and decrypting.</span></span> 
* <span data-ttu-id="a80b6-180">hello protokollet för tillgångsleverans (till exempel MPEG DASH, HLS, Smooth Streaming eller alla).</span><span class="sxs-lookup"><span data-stu-id="a80b6-180">hello asset delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all).</span></span>
* <span data-ttu-id="a80b6-181">hello typen av dynamisk kryptering (till exempel AES envelope) eller ingen dynamisk kryptering.</span><span class="sxs-lookup"><span data-stu-id="a80b6-181">hello type of dynamic encryption (for example, AES envelope) or no dynamic encryption.</span></span> 

<span data-ttu-id="a80b6-182">Detaljerad information finns i [Konfigurera tillgångsleveransprincip ](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="a80b6-182">For detailed information, see [Configure asset delivery policy ](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <span data-ttu-id="a80b6-183"><a id="create_locator"></a>Skapa en OnDemand-strömning positionerare ordning tooget en strömnings-URL</span><span class="sxs-lookup"><span data-stu-id="a80b6-183"><a id="create_locator"></a>Create an OnDemand streaming locator in order tooget a streaming URL</span></span>
<span data-ttu-id="a80b6-184">Du behöver tooprovide användaren hello strömmande URL för Smooth, DASH eller HLS.</span><span class="sxs-lookup"><span data-stu-id="a80b6-184">You will need tooprovide your user with hello streaming URL for Smooth, DASH or HLS.</span></span>

> [!NOTE]
> <span data-ttu-id="a80b6-185">Om du lägger till eller uppdaterar din tillgångs leveransprincip måste du ta bort en befintlig lokaliserare (om sådan finns) och skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="a80b6-185">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
> 
> 

<span data-ttu-id="a80b6-186">Anvisningar för hur toopublish en tillgång och skapa en strömnings-URL, se [skapa en strömnings-URL](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="a80b6-186">For instructions on how toopublish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="get-a-test-token"></a><span data-ttu-id="a80b6-187">Hämta en testtoken</span><span class="sxs-lookup"><span data-stu-id="a80b6-187">Get a test token</span></span>
<span data-ttu-id="a80b6-188">Hämta en testtoken baserat på hello tokenbegränsningar som användes för hello innehållsnyckelns auktoriseringsprincip.</span><span class="sxs-lookup"><span data-stu-id="a80b6-188">Get a test token based on hello token restriction that was used for hello key authorization policy.</span></span>

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on hello data in hello given TokenRestrictionTemplate.
    //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
    //so you have tooadd it in front of hello token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);

<span data-ttu-id="a80b6-189">Du kan använda hello [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest strömmen.</span><span class="sxs-lookup"><span data-stu-id="a80b6-189">You can use hello [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest your stream.</span></span>

## <span data-ttu-id="a80b6-190"><a id="client_request"></a>Hur kan klienten begär en nyckel från hello viktiga leverans tjänsten?</span><span class="sxs-lookup"><span data-stu-id="a80b6-190"><a id="client_request"></a>How can your client request a key from hello key delivery service?</span></span>
<span data-ttu-id="a80b6-191">I föregående steg hello konstrueras hello-URL som pekar tooa manifestfilen.</span><span class="sxs-lookup"><span data-stu-id="a80b6-191">In hello previous step, you constructed hello URL that points tooa manifest file.</span></span> <span data-ttu-id="a80b6-192">Klienten måste tooextract hello nödvändig information från hello strömning manifestfiler i ordning toomake en begäran toohello viktiga leveranstjänst.</span><span class="sxs-lookup"><span data-stu-id="a80b6-192">Your client needs tooextract hello necessary information from hello streaming manifest files in order toomake a request toohello key delivery service.</span></span>

### <a name="manifest-files"></a><span data-ttu-id="a80b6-193">Manifest-filer</span><span class="sxs-lookup"><span data-stu-id="a80b6-193">Manifest files</span></span>
<span data-ttu-id="a80b6-194">hello-klienten måste tooextract hello URL (som också innehåller innehållsnyckeln Id (kid)) värdet från hello manifestfilen.</span><span class="sxs-lookup"><span data-stu-id="a80b6-194">hello client needs tooextract hello URL (that also contains content key Id (kid)) value from hello manifest file.</span></span> <span data-ttu-id="a80b6-195">hello klienten försök sedan tooget hello krypteringsnyckeln från hello viktiga leverans av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a80b6-195">hello client will then try tooget hello encryption key from hello key delivery service.</span></span> <span data-ttu-id="a80b6-196">hello klienten måste också tooextract hello IV värdet och Använd den dekryptera hello stream.hello följande fragment visar hello <Protection> element hello Smooth Streaming manifestet.</span><span class="sxs-lookup"><span data-stu-id="a80b6-196">hello client also needs tooextract hello IV value and use it do decrypt hello stream.hello following snippet shows hello <Protection> element of hello Smooth Streaming manifest.</span></span>

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

<span data-ttu-id="a80b6-197">Hello gäller HLS, är hello rot manifestet uppdelad i segment filer.</span><span class="sxs-lookup"><span data-stu-id="a80b6-197">In hello case of HLS, hello root manifest is broken into segment files.</span></span> 

<span data-ttu-id="a80b6-198">Hello rot manifestet är till exempel: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) den innehåller en lista över segment filnamn.</span><span class="sxs-lookup"><span data-stu-id="a80b6-198">For example, hello root manifest is: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) and it contains a list of segment file names.</span></span>

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

<span data-ttu-id="a80b6-199">Om du öppnar en hello segment fil i redigerare (till exempel http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should innehåller #EXT X-nyckel som anger att hello-filen är krypterad.</span><span class="sxs-lookup"><span data-stu-id="a80b6-199">If you open one of hello segment files in text editor (for example, http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should contain #EXT-X-KEY which indicates that hello file is encrypted.</span></span>

    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST

>[!NOTE] 
><span data-ttu-id="a80b6-200">Om du planerar tooplay en AES krypterade HLS i Safari, se [bloggen](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span><span class="sxs-lookup"><span data-stu-id="a80b6-200">If you are planning tooplay an AES encrypted HLS in Safari, see [this blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span></span>

### <a name="request-hello-key-from-hello-key-delivery-service"></a><span data-ttu-id="a80b6-201">Begära hello nyckeln från hello viktiga delivery service</span><span class="sxs-lookup"><span data-stu-id="a80b6-201">Request hello key from hello key delivery service</span></span>

<span data-ttu-id="a80b6-202">hello följande kod visar hur toosend begäran-toohello Media Services nyckeln leverans av tjänsten med en nyckel leverans Uri (som har extraherats från hello manifestet) och en token (det här avsnittet inte dig om hur tooget Simple Web Tokens från en Secure Token Service).</span><span class="sxs-lookup"><span data-stu-id="a80b6-202">hello following code shows how toosend a request toohello Media Services key delivery service using a key delivery Uri (that was extracted from hello manifest) and a token (this topic does not talk about how tooget Simple Web Tokens from a Secure Token Service).</span></span>

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);

        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;

        var response = request.GetResponse();

        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }

        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();

        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }

## <a name="protect-your-content-with-aes-128-using-net"></a><span data-ttu-id="a80b6-203">Skydda ditt innehåll med AES-128 med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="a80b6-203">Protect your content with AES-128 using .NET</span></span>

### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="a80b6-204">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="a80b6-204">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="a80b6-205">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="a80b6-205">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="a80b6-206">Lägg till följande element för hello**appSettings** definieras i filen app.config:</span><span class="sxs-lookup"><span data-stu-id="a80b6-206">Add hello following elements too**appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <span data-ttu-id="a80b6-207"><a id="example"></a>Exempel</span><span class="sxs-lookup"><span data-stu-id="a80b6-207"><a id="example"></a>Example</span></span>

<span data-ttu-id="a80b6-208">Skriv över hello koden i Program.cs-filen med hello koden som visas i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a80b6-208">Overwrite hello code in your Program.cs file with hello code shown in this section.</span></span>
 
>[!NOTE]
><span data-ttu-id="a80b6-209">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="a80b6-209">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="a80b6-210">Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer).</span><span class="sxs-lookup"><span data-stu-id="a80b6-210">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="a80b6-211">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a80b6-211">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="a80b6-212">Se till att tooupdate variabler toopoint toofolders där dina indatafiler finns.</span><span class="sxs-lookup"><span data-stu-id="a80b6-212">Make sure tooupdate variables toopoint toofolders where your input files are located.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Security.Cryptography;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace AESDynamicEncryptionAndKeyDeliverySvc
    {
        class Program
        {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // A Uri describing hello issuer of hello token.  
        // Must match hello value in hello token for hello token toobe considered valid.
        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        // hello Audience or Scope of hello token.  
        // Must match hello value in hello token for hello token toobe considered valid.
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

            IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
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

            // Generate a test token based on hello data in hello given TokenRestrictionTemplate.
            // Note, you need toopass hello key id Guid because we specified 
            // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

            //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
            //so you have tooadd it in front of hello token string. 
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
            Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use hello bit.ly/aesplayer Flash player tootest hello URL 
            // (with open authorization policy). 
            // Paste hello URL and click hello Update button tooplay hello video. 
            //
            string URL = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
            Console.WriteLine();
            Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
            Console.WriteLine();

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
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);


            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass tooit hello name of hello 
            // processor toouse for hello specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with hello encoding details, using a string preset.
            // In this case "Adaptive Streaming" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify hello input asset toobe encoded.
            task.InputAssets.Add(asset);
            // Add an output asset toocontain hello results of hello job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means hello output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }

        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }

        static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
        {
            // Create envelope encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                keyId,
                contentKey,
                "ContentKey",
                ContentKeyType.EnvelopeEncryption);

            // Associate hello key with hello asset.
            asset.ContentKeys.Add(key);

            return key;
        }

        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {
            // Create ContentKeyAuthorizationPolicy with Open restrictions 
            // and create authorization policy             
            IContentKeyAuthorizationPolicy policy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Open Authorization Policy").Result;

            List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

            ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
            Name = "HLS Open Authorization Policy",
            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
            Requirements = null // no requirements needed for HLS
        };

            restrictions.Add(restriction);

            IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy",
            ContentKeyDeliveryType.BaselineHttp,
            restrictions,
            "");

            policy.Options.Add(policyOption);

            // Add ContentKeyAutorizationPolicy tooContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            IContentKeyAuthorizationPolicy policy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("HLS token restricted authorization policy").Result;

            List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

            ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "Token Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString
            };

            restrictions.Add(restriction);

            //You could have multiple options 
            IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "Token option for HLS",
            ContentKeyDeliveryType.BaselineHttp,
            restrictions,
            null  // no key delivery data is needed for HLS
            );

            policy.Options.Add(policyOption);

            // Add ContentKeyAutorizationPolicy tooContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);

            return tokenTemplateString;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);

            string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));

            // When configuring delivery policy, you can choose tooassociate it
            // with a key acquisition URL that has a KID appended or
            // or a key acquisition URL that does not have a KID appended  
            // in which case a content key can be reused. 

            // EnvelopeKeyAcquisitionUrl:  contains a key ID in hello key URL.
            // EnvelopeBaseKeyAcquisitionUrl:  hello URL does not contains a key ID

            // hello following policy configuration specifies: 
            // key url that will have KID=<Guid> appended toohello envelope and
            // hello Initialization Vector (IV) toouse for hello envelope encryption.

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
            };

            IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
                AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                assetDeliveryPolicyConfiguration);

            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
        }

        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference toohello streaming manifest file from hello  
            // collection of files in hello asset. 

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                EndsWith(".ism")).
                FirstOrDefault();

            // Create a 30-day readonly access policy. 
            // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
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

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
        }

        static private byte[] GetRandomBuffer(int size)
        {
            byte[] randomBytes = new byte[size];
            using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
            {
            rng.GetBytes(randomBytes);
            }

            return randomBytes;
        }
        }
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="a80b6-213">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="a80b6-213">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="a80b6-214">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="a80b6-214">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

