---
title: "aaaProtect HLS innehåll med Microsoft PlayReady- eller Apple FairPlay - Azure | Microsoft Docs"
description: "Det här avsnittet ger en översikt och visar hur toouse Azure Media Services toodynamically kryptera din HTTP Live Streaming (HLS) innehåll med Apple FairPlay. Den visar även hur toouse hello Media Services licens leverans av tjänsten toodeliver FairPlay-licenser tooclients."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 91ca451e3e7bf0da1d74dac4c99180f08f39e4ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protect-your-hls-content-with-apple-fairplay-or-microsoft-playready"></a><span data-ttu-id="d1310-104">Skydda ditt innehåll med Apple FairPlay eller Microsoft PlayReady HLS</span><span class="sxs-lookup"><span data-stu-id="d1310-104">Protect your HLS content with Apple FairPlay or Microsoft PlayReady</span></span>
<span data-ttu-id="d1310-105">Azure Media Services aktiverar du toodynamically kryptera innehållet HTTP Live Streaming (HLS) med hjälp av hello följande format:</span><span class="sxs-lookup"><span data-stu-id="d1310-105">Azure Media Services enables you toodynamically encrypt your HTTP Live Streaming (HLS) content by using hello following formats:</span></span>  

* <span data-ttu-id="d1310-106">**AES-128 kuvert klartextnyckeln**</span><span class="sxs-lookup"><span data-stu-id="d1310-106">**AES-128 envelope clear key**</span></span>

    <span data-ttu-id="d1310-107">hello hela segmentet krypteras med hjälp av hello **AES-128 CBC** läge.</span><span class="sxs-lookup"><span data-stu-id="d1310-107">hello entire chunk is encrypted by using hello **AES-128 CBC** mode.</span></span> <span data-ttu-id="d1310-108">hello dekryptering av hello dataströmmen stöds internt av iOS och OS X spelare.</span><span class="sxs-lookup"><span data-stu-id="d1310-108">hello decryption of hello stream is supported by iOS and OS X player natively.</span></span> <span data-ttu-id="d1310-109">Mer information finns i [dynamisk med hjälp av AES-128-kryptering och viktiga leverans tjänsten](media-services-protect-with-aes128.md).</span><span class="sxs-lookup"><span data-stu-id="d1310-109">For more information, see [Using AES-128 dynamic encryption and key delivery service](media-services-protect-with-aes128.md).</span></span>
* <span data-ttu-id="d1310-110">**Apple FairPlay**</span><span class="sxs-lookup"><span data-stu-id="d1310-110">**Apple FairPlay**</span></span>

    <span data-ttu-id="d1310-111">Hej enskilda video och ljud exempel krypteras med hjälp av hello **AES-128 CBC** läge.</span><span class="sxs-lookup"><span data-stu-id="d1310-111">hello individual video and audio samples are encrypted by using hello **AES-128 CBC** mode.</span></span> <span data-ttu-id="d1310-112">**FairPlay strömning** (FPS) är integrerat i hello enhetens operativsystem med inbyggt stöd på iOS- och Apple TV.</span><span class="sxs-lookup"><span data-stu-id="d1310-112">**FairPlay Streaming** (FPS) is integrated into hello device operating systems, with native support on iOS and Apple TV.</span></span> <span data-ttu-id="d1310-113">Safari för OS X kan FPS med hello krypterade Media tillägg (EME) Gränssnittssupport.</span><span class="sxs-lookup"><span data-stu-id="d1310-113">Safari on OS X enables FPS by using hello Encrypted Media Extensions (EME) interface support.</span></span>
* <span data-ttu-id="d1310-114">**Microsoft PlayReady**</span><span class="sxs-lookup"><span data-stu-id="d1310-114">**Microsoft PlayReady**</span></span>

<span data-ttu-id="d1310-115">hello följande bild visar hello **HLS + FairPlay eller PlayReady dynamisk kryptering** arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="d1310-115">hello following image shows hello **HLS + FairPlay or PlayReady dynamic encryption** workflow.</span></span>

![Diagram över arbetsflödet för dynamisk kryptering](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

<span data-ttu-id="d1310-117">Det här avsnittet visar hur toouse Media Services toodynamically kryptera din HLS innehåll med Apple FairPlay.</span><span class="sxs-lookup"><span data-stu-id="d1310-117">This topic demonstrates how toouse Media Services toodynamically encrypt your HLS content with Apple FairPlay.</span></span> <span data-ttu-id="d1310-118">Den visar även hur toouse hello Media Services licens leverans av tjänsten toodeliver FairPlay-licenser tooclients.</span><span class="sxs-lookup"><span data-stu-id="d1310-118">It also shows how toouse hello Media Services license delivery service toodeliver FairPlay licenses tooclients.</span></span>

> [!NOTE]
> <span data-ttu-id="d1310-119">Om du även vill tooencrypt din HLS innehåll med PlayReady, du behöver toocreate en gemensam innehållsnyckel och associera den med din tillgång.</span><span class="sxs-lookup"><span data-stu-id="d1310-119">If you also want tooencrypt your HLS content with PlayReady, you need toocreate a common content key and associate it with your asset.</span></span> <span data-ttu-id="d1310-120">Du måste också tooconfigure hello innehållsnyckelns auktoriseringsprincip, enligt beskrivningen i [med PlayReady-kryptering för dynamiska vanliga](media-services-protect-with-drm.md).</span><span class="sxs-lookup"><span data-stu-id="d1310-120">You also need tooconfigure hello content key’s authorization policy, as described in [Using PlayReady dynamic common encryption](media-services-protect-with-drm.md).</span></span>
>
>

## <a name="requirements-and-considerations"></a><span data-ttu-id="d1310-121">Krav och överväganden</span><span class="sxs-lookup"><span data-stu-id="d1310-121">Requirements and considerations</span></span>

<span data-ttu-id="d1310-122">hello följande krävs när du använder Media Services toodeliver HLS som krypterats med FairPlay och toodeliver FairPlay-licenser:</span><span class="sxs-lookup"><span data-stu-id="d1310-122">hello following are required when using Media Services toodeliver HLS encrypted with FairPlay, and toodeliver FairPlay licenses:</span></span>

  * <span data-ttu-id="d1310-123">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="d1310-123">An Azure account.</span></span> <span data-ttu-id="d1310-124">Mer information finns i avsnittet om [den kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="d1310-124">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
  * <span data-ttu-id="d1310-125">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="d1310-125">A Media Services account.</span></span> <span data-ttu-id="d1310-126">toocreate, se [skapa ett Azure Media Services-konto med hello Azure-portalen](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="d1310-126">toocreate one, see [Create an Azure Media Services account using hello Azure portal](media-services-portal-create-account.md).</span></span>
  * <span data-ttu-id="d1310-127">Logga med [Apple Development Program](https://developer.apple.com/).</span><span class="sxs-lookup"><span data-stu-id="d1310-127">Sign up with [Apple Development Program](https://developer.apple.com/).</span></span>
  * <span data-ttu-id="d1310-128">Apple kräver hello innehållets ägare tooobtain hello [distributionspaketet](https://developer.apple.com/contact/fps/).</span><span class="sxs-lookup"><span data-stu-id="d1310-128">Apple requires hello content owner tooobtain hello [deployment package](https://developer.apple.com/contact/fps/).</span></span> <span data-ttu-id="d1310-129">Tillstånd redan implementerats nyckeln Security Module (KSM) med Media Services och att du begär hello slutliga FPS paketet.</span><span class="sxs-lookup"><span data-stu-id="d1310-129">State that you already implemented Key Security Module (KSM) with Media Services, and that you are requesting hello final FPS package.</span></span> <span data-ttu-id="d1310-130">Det finns instruktioner i hello slutliga FPS paketet toogenerate certifiering och få hello programmet hemlig nyckel (be).</span><span class="sxs-lookup"><span data-stu-id="d1310-130">There are instructions in hello final FPS package toogenerate certification and obtain hello Application Secret Key (ASK).</span></span> <span data-ttu-id="d1310-131">Du kan använda be tooconfigure FairPlay.</span><span class="sxs-lookup"><span data-stu-id="d1310-131">You use ASK tooconfigure FairPlay.</span></span>
  * <span data-ttu-id="d1310-132">Azure Media Services .NET SDK-versionen **3.6.0** eller senare.</span><span class="sxs-lookup"><span data-stu-id="d1310-132">Azure Media Services .NET SDK version **3.6.0** or later.</span></span>

<span data-ttu-id="d1310-133">följande saker hello måste anges på Media Services viktiga leverans sida:</span><span class="sxs-lookup"><span data-stu-id="d1310-133">hello following things must be set on Media Services key delivery side:</span></span>

  * <span data-ttu-id="d1310-134">**Appen Cert (nät)**: Detta är en .pfx-fil som innehåller hello privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="d1310-134">**App Cert (AC)**: This is a .pfx file that contains hello private key.</span></span> <span data-ttu-id="d1310-135">Du skapar den här filen och kryptera den med ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="d1310-135">You create this file and encrypt it with a password.</span></span>

       <span data-ttu-id="d1310-136">När du konfigurerar en nyckel leveransprincip måste du ange att lösenordet och hello .pfx-fil i Base64-format.</span><span class="sxs-lookup"><span data-stu-id="d1310-136">When you configure a key delivery policy, you must provide that password and hello .pfx file in Base64 format.</span></span>

      <span data-ttu-id="d1310-137">hello följande steg beskriver hur toogenerate certifikat PFX-filen för FairPlay:</span><span class="sxs-lookup"><span data-stu-id="d1310-137">hello following steps describe how toogenerate a .pfx certificate file for FairPlay:</span></span>

    1. <span data-ttu-id="d1310-138">Installera OpenSSL från https://slproweb.com/products/Win32OpenSSL.html.</span><span class="sxs-lookup"><span data-stu-id="d1310-138">Install OpenSSL from https://slproweb.com/products/Win32OpenSSL.html.</span></span>

        <span data-ttu-id="d1310-139">Gå toohello mapp där hello FairPlay certifikat och andra filer som levereras av Apple.</span><span class="sxs-lookup"><span data-stu-id="d1310-139">Go toohello folder where hello FairPlay certificate and other files delivered by Apple are.</span></span>
    2. <span data-ttu-id="d1310-140">Kör följande kommando från kommandoraden hello hello.</span><span class="sxs-lookup"><span data-stu-id="d1310-140">Run hello following command from hello command line.</span></span> <span data-ttu-id="d1310-141">Detta omvandlar hello .cer-fil tooa PEM-filen.</span><span class="sxs-lookup"><span data-stu-id="d1310-141">This converts hello .cer file tooa .pem file.</span></span>

        <span data-ttu-id="d1310-142">”C:\OpenSSL-Win32\bin\openssl.exe” x509-informera der-i fairplay.cer-ut fairplay out.pem</span><span class="sxs-lookup"><span data-stu-id="d1310-142">"C:\OpenSSL-Win32\bin\openssl.exe" x509 -inform der -in fairplay.cer -out fairplay-out.pem</span></span>
    3. <span data-ttu-id="d1310-143">Kör följande kommando från kommandoraden hello hello.</span><span class="sxs-lookup"><span data-stu-id="d1310-143">Run hello following command from hello command line.</span></span> <span data-ttu-id="d1310-144">Detta omvandlar hello PEM-filen tooa .pfx-filen med hello privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="d1310-144">This converts hello .pem file tooa .pfx file with hello private key.</span></span> <span data-ttu-id="d1310-145">hello lösenord för hello PFX-filen blir sedan ombedd av OpenSSL.</span><span class="sxs-lookup"><span data-stu-id="d1310-145">hello password for hello .pfx file is then asked by OpenSSL.</span></span>

        <span data-ttu-id="d1310-146">”C:\OpenSSL-Win32\bin\openssl.exe” pkcs12-exportera - ut fairplay out.pfx-inkey privatekey.pem-i fairplay out.pem - passin file:privatekey-pem-pass.txt</span><span class="sxs-lookup"><span data-stu-id="d1310-146">"C:\OpenSSL-Win32\bin\openssl.exe" pkcs12 -export -out fairplay-out.pfx -inkey privatekey.pem -in fairplay-out.pem -passin file:privatekey-pem-pass.txt</span></span>
  * <span data-ttu-id="d1310-147">**Appen Cert lösenord**: hello lösenord för att skapa hello .pfx-fil.</span><span class="sxs-lookup"><span data-stu-id="d1310-147">**App Cert password**: hello password for creating hello .pfx file.</span></span>
  * <span data-ttu-id="d1310-148">**ID för appen Cert lösenord**: du måste överföra hello lösenord, liknande toohow de överför andra Media Services-nycklar.</span><span class="sxs-lookup"><span data-stu-id="d1310-148">**App Cert password ID**: You must upload hello password, similar toohow they upload other Media Services keys.</span></span> <span data-ttu-id="d1310-149">Använd hello **ContentKeyType.FairPlayPfxPassword** enum-värde tooget hello Media Services-ID.</span><span class="sxs-lookup"><span data-stu-id="d1310-149">Use hello **ContentKeyType.FairPlayPfxPassword** enum value tooget hello Media Services ID.</span></span> <span data-ttu-id="d1310-150">Det här är de behöver toouse inuti hello viktiga princip leveransalternativ.</span><span class="sxs-lookup"><span data-stu-id="d1310-150">This is what they need toouse inside hello key delivery policy option.</span></span>
  * <span data-ttu-id="d1310-151">**IV**: Detta är ett slumpmässigt värde av 16 byte.</span><span class="sxs-lookup"><span data-stu-id="d1310-151">**iv**: This is a random value of 16 bytes.</span></span> <span data-ttu-id="d1310-152">Den måste matcha hello iv i hello tillgångsleveransprincip.</span><span class="sxs-lookup"><span data-stu-id="d1310-152">It must match hello iv in hello asset delivery policy.</span></span> <span data-ttu-id="d1310-153">Du genererar hello iv och placera den på båda platserna: hello tillgångsleveransprincip och hello viktiga princip leveransalternativ.</span><span class="sxs-lookup"><span data-stu-id="d1310-153">You generate hello iv, and put it in both places: hello asset delivery policy and hello key delivery policy option.</span></span>
  * <span data-ttu-id="d1310-154">**Be**: den här nyckeln tas emot när du skapar hello certifikatutfärdare med hjälp av hello Apple Developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="d1310-154">**ASK**: This key is received when you generate hello certification by using hello Apple Developer portal.</span></span> <span data-ttu-id="d1310-155">Varje Utvecklingsteamet får en unik be.</span><span class="sxs-lookup"><span data-stu-id="d1310-155">Each development team will receive a unique ASK.</span></span> <span data-ttu-id="d1310-156">Spara en kopia av hello be och lagra den på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="d1310-156">Save a copy of hello ASK, and store it in a safe place.</span></span> <span data-ttu-id="d1310-157">Du behöver tooconfigure be som FairPlayAsk tooMedia tjänster senare.</span><span class="sxs-lookup"><span data-stu-id="d1310-157">You will need tooconfigure ASK as FairPlayAsk tooMedia Services later.</span></span>
  * <span data-ttu-id="d1310-158">**Be ID**: Detta ID hämtas när du överför be till Media Services.</span><span class="sxs-lookup"><span data-stu-id="d1310-158">**ASK ID**: This ID is obtained when you upload ASK into Media Services.</span></span> <span data-ttu-id="d1310-159">Du måste överföra be med hjälp av hello **ContentKeyType.FairPlayAsk** enum-värde.</span><span class="sxs-lookup"><span data-stu-id="d1310-159">You must upload ASK by using hello **ContentKeyType.FairPlayAsk** enum value.</span></span> <span data-ttu-id="d1310-160">Hello Media Services ID returneras som hello resultat, och det här är vad ska användas när hello viktiga princip leveransalternativ.</span><span class="sxs-lookup"><span data-stu-id="d1310-160">As hello result, hello Media Services ID is returned, and this is what should be used when setting hello key delivery policy option.</span></span>

<span data-ttu-id="d1310-161">hello måste följande anges av hello FPS på klientsidan:</span><span class="sxs-lookup"><span data-stu-id="d1310-161">hello following things must be set by hello FPS client side:</span></span>

  * <span data-ttu-id="d1310-162">**Appen Cert (nät)**: Detta är en.cer/.der-fil som innehåller hello offentliga nyckel, vilket hello operativsystem använder tooencrypt vissa nyttolast.</span><span class="sxs-lookup"><span data-stu-id="d1310-162">**App Cert (AC)**: This is a .cer/.der file that contains hello public key, which hello operating system uses tooencrypt some payload.</span></span> <span data-ttu-id="d1310-163">Media Services måste tooknow om den eftersom det krävs av hello player.</span><span class="sxs-lookup"><span data-stu-id="d1310-163">Media Services needs tooknow about it because it is required by hello player.</span></span> <span data-ttu-id="d1310-164">hello viktiga leverans tjänsten dekrypterar den med hjälp av hello motsvarande privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="d1310-164">hello key delivery service decrypts it using hello corresponding private key.</span></span>

<span data-ttu-id="d1310-165">tooplay tillbaka en krypterad dataström FairPlay, skaffa en verklig be först och sedan generera en verklig certifikat.</span><span class="sxs-lookup"><span data-stu-id="d1310-165">tooplay back a FairPlay encrypted stream, get a real ASK first, and then generate a real certificate.</span></span> <span data-ttu-id="d1310-166">Den här processen skapar alla tre delar:</span><span class="sxs-lookup"><span data-stu-id="d1310-166">That process creates all three parts:</span></span>

  * <span data-ttu-id="d1310-167">.der fil</span><span class="sxs-lookup"><span data-stu-id="d1310-167">.der file</span></span>
  * <span data-ttu-id="d1310-168">.pfx-fil</span><span class="sxs-lookup"><span data-stu-id="d1310-168">.pfx file</span></span>
  * <span data-ttu-id="d1310-169">lösenordet för hello .pfx</span><span class="sxs-lookup"><span data-stu-id="d1310-169">password for hello .pfx</span></span>

<span data-ttu-id="d1310-170">hello följande klienter har stöd för HLS med **AES-128 CBC** kryptering: Safari på OS X, Apple TV iOS.</span><span class="sxs-lookup"><span data-stu-id="d1310-170">hello following clients support HLS with **AES-128 CBC** encryption: Safari on OS X, Apple TV, iOS.</span></span>

## <a name="configure-fairplay-dynamic-encryption-and-license-delivery-services"></a><span data-ttu-id="d1310-171">Konfigurera FairPlay dynamisk kryptering och licensen för leverans av tjänster</span><span class="sxs-lookup"><span data-stu-id="d1310-171">Configure FairPlay dynamic encryption and license delivery services</span></span>
<span data-ttu-id="d1310-172">hello följande är allmänna steg för att skydda dina tillgångar med FairPlay med hjälp av hello Media Services licensleveranstjänst och även med hjälp av dynamisk kryptering.</span><span class="sxs-lookup"><span data-stu-id="d1310-172">hello following are general steps for protecting your assets with FairPlay by using hello Media Services license delivery service, and also by using dynamic encryption.</span></span>

1. <span data-ttu-id="d1310-173">Skapa en tillgång och överföra filer till hello tillgång.</span><span class="sxs-lookup"><span data-stu-id="d1310-173">Create an asset, and upload files into hello asset.</span></span>
2. <span data-ttu-id="d1310-174">Koda hello tillgång som innehåller hello filen toohello anpassningsbar bithastighet MP4-uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="d1310-174">Encode hello asset that contains hello file toohello adaptive bitrate MP4 set.</span></span>
3. <span data-ttu-id="d1310-175">Skapa en innehållsnyckel och associera den med hello kodade tillgången.</span><span class="sxs-lookup"><span data-stu-id="d1310-175">Create a content key, and associate it with hello encoded asset.</span></span>  
4. <span data-ttu-id="d1310-176">Konfigurera hello innehållsnyckelns auktoriseringsprincip.</span><span class="sxs-lookup"><span data-stu-id="d1310-176">Configure hello content key’s authorization policy.</span></span> <span data-ttu-id="d1310-177">Ange hello följande:</span><span class="sxs-lookup"><span data-stu-id="d1310-177">Specify hello following:</span></span>

   * <span data-ttu-id="d1310-178">hello leveransmetod (i det här fallet FairPlay).</span><span class="sxs-lookup"><span data-stu-id="d1310-178">hello delivery method (in this case, FairPlay).</span></span>
   * <span data-ttu-id="d1310-179">FairPlay alternativkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="d1310-179">FairPlay policy options configuration.</span></span> <span data-ttu-id="d1310-180">Mer information om hur tooconfigure FairPlay, se hello **ConfigureFairPlayPolicyOptions()** metod i hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="d1310-180">For details on how tooconfigure FairPlay, see hello **ConfigureFairPlayPolicyOptions()** method in hello sample below.</span></span>

     > [!NOTE]
     > <span data-ttu-id="d1310-181">Vanligtvis skulle vill tooconfigure FairPlay princip alternativ bara en gång, eftersom du bara har en uppsättning av en certifikatutfärdare och en fråga.</span><span class="sxs-lookup"><span data-stu-id="d1310-181">Usually, you would want tooconfigure FairPlay policy options only once, because you will only have one set of a certification and an ASK.</span></span>
     >
     >
   * <span data-ttu-id="d1310-182">Begränsningar (öppen eller token).</span><span class="sxs-lookup"><span data-stu-id="d1310-182">Restrictions (open or token).</span></span>
   * <span data-ttu-id="d1310-183">Information specifik toohello nyckelleveranstypen och som definierar hur hello nyckeln levereras toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="d1310-183">Information specific toohello key delivery type that defines how hello key is delivered toohello client.</span></span>
5. <span data-ttu-id="d1310-184">Konfigurera principen för tillgångsleverans hello.</span><span class="sxs-lookup"><span data-stu-id="d1310-184">Configure hello asset delivery policy.</span></span> <span data-ttu-id="d1310-185">hello konfigurationen för leveransprincipen omfattar:</span><span class="sxs-lookup"><span data-stu-id="d1310-185">hello delivery policy configuration includes:</span></span>

   * <span data-ttu-id="d1310-186">Hej leveransprotokoll (HLS).</span><span class="sxs-lookup"><span data-stu-id="d1310-186">hello delivery protocol (HLS).</span></span>
   * <span data-ttu-id="d1310-187">hello typen av dynamisk kryptering (vanliga CBC kryptering).</span><span class="sxs-lookup"><span data-stu-id="d1310-187">hello type of dynamic encryption (common CBC encryption).</span></span>
   * <span data-ttu-id="d1310-188">URL för anskaffning av hello licens.</span><span class="sxs-lookup"><span data-stu-id="d1310-188">hello license acquisition URL.</span></span>

     > [!NOTE]
     > <span data-ttu-id="d1310-189">Om du vill toodeliver en ström som har krypterats med FairPlay och en annan Digital Rights Management (DRM)-system kan ha tooconfigure separat leveransprinciperna:</span><span class="sxs-lookup"><span data-stu-id="d1310-189">If you want toodeliver a stream that is encrypted with FairPlay and another Digital Rights Management (DRM) system, you have tooconfigure separate delivery policies:</span></span>
     >
     > * <span data-ttu-id="d1310-190">En IAssetDeliveryPolicy tooconfigure dynamiska anpassningsbar strömning via HTTP-(bindestreck) med vanliga kryptering (CENC) (PlayReady och Widevine) och Smooth med PlayReady</span><span class="sxs-lookup"><span data-stu-id="d1310-190">One IAssetDeliveryPolicy tooconfigure Dynamic Adaptive Streaming over HTTP (DASH) with Common Encryption (CENC) (PlayReady + Widevine), and Smooth with PlayReady</span></span>
     > * <span data-ttu-id="d1310-191">En annan IAssetDeliveryPolicy tooconfigure FairPlay för HLS</span><span class="sxs-lookup"><span data-stu-id="d1310-191">Another IAssetDeliveryPolicy tooconfigure FairPlay for HLS</span></span>
     >
     >
6. <span data-ttu-id="d1310-192">Skapa en OnDemand-positionerare tooget en strömnings-URL.</span><span class="sxs-lookup"><span data-stu-id="d1310-192">Create an OnDemand locator tooget a streaming URL.</span></span>

## <a name="use-fairplay-key-delivery-by-player-apps"></a><span data-ttu-id="d1310-193">Använd FairPlay viktiga leverans av player appar</span><span class="sxs-lookup"><span data-stu-id="d1310-193">Use FairPlay key delivery by player apps</span></span>
<span data-ttu-id="d1310-194">Du kan utveckla player appar med hjälp av hello iOS SDK.</span><span class="sxs-lookup"><span data-stu-id="d1310-194">You can develop player apps by using hello iOS SDK.</span></span> <span data-ttu-id="d1310-195">toobe kan tooplay FairPlay innehåll, har du tooimplement hello licens exchange-protokollet.</span><span class="sxs-lookup"><span data-stu-id="d1310-195">toobe able tooplay FairPlay content, you have tooimplement hello license exchange protocol.</span></span> <span data-ttu-id="d1310-196">Det här protokollet har inte angetts av Apple.</span><span class="sxs-lookup"><span data-stu-id="d1310-196">This protocol is not specified by Apple.</span></span> <span data-ttu-id="d1310-197">Är det upp tooeach app hur toosend viktiga leverans begäranden.</span><span class="sxs-lookup"><span data-stu-id="d1310-197">It is up tooeach app how toosend key delivery requests.</span></span> <span data-ttu-id="d1310-198">hello Media Services FairPlay viktiga delivery service förväntas hello SPC toocome som meddelandet www-form-url kodad post i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="d1310-198">hello Media Services FairPlay key delivery service expects hello SPC toocome as a www-form-url encoded post message, in hello following form:</span></span>

    spc=<Base64 encoded SPC>

> [!NOTE]
> <span data-ttu-id="d1310-199">Azure Media Player stöder inte FairPlay uppspelning out of box hello.</span><span class="sxs-lookup"><span data-stu-id="d1310-199">Azure Media Player doesn’t support FairPlay playback out of hello box.</span></span> <span data-ttu-id="d1310-200">tooget FairPlay uppspelning på MAC OS X, hämta hello exempel player från hello Apple developer-konto.</span><span class="sxs-lookup"><span data-stu-id="d1310-200">tooget FairPlay playback on MAC OS X, obtain hello sample player from hello Apple developer account.</span></span>
>
>

## <a name="streaming-urls"></a><span data-ttu-id="d1310-201">Strömmande URL: er</span><span class="sxs-lookup"><span data-stu-id="d1310-201">Streaming URLs</span></span>
<span data-ttu-id="d1310-202">Om din tillgång har krypterats med flera DRM, bör du använda en tagg för kryptering i hello strömnings-URL: (format = 'm3u8 aapl', kryptering = ”xxx”).</span><span class="sxs-lookup"><span data-stu-id="d1310-202">If your asset was encrypted with more than one DRM, you should use an encryption tag in hello streaming URL: (format='m3u8-aapl', encryption='xxx').</span></span>

<span data-ttu-id="d1310-203">det gäller hello följande överväganden:</span><span class="sxs-lookup"><span data-stu-id="d1310-203">hello following considerations apply:</span></span>

* <span data-ttu-id="d1310-204">Endast noll eller en typ av enhetskryptering kan anges.</span><span class="sxs-lookup"><span data-stu-id="d1310-204">Only zero or one encryption type can be specified.</span></span>
* <span data-ttu-id="d1310-205">hello krypteringstyp saknar toobe som anges i hello URL Om bara en kryptering har tillämpade toohello tillgången.</span><span class="sxs-lookup"><span data-stu-id="d1310-205">hello encryption type doesn't have toobe specified in hello URL if only one encryption was applied toohello asset.</span></span>
* <span data-ttu-id="d1310-206">hello krypteringstypen är skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="d1310-206">hello encryption type is case insensitive.</span></span>
* <span data-ttu-id="d1310-207">hello följande krypteringstyper kan anges:</span><span class="sxs-lookup"><span data-stu-id="d1310-207">hello following encryption types can be specified:</span></span>  
  * <span data-ttu-id="d1310-208">**cenc**: Common encryption (PlayReady eller Widevine)</span><span class="sxs-lookup"><span data-stu-id="d1310-208">**cenc**:  Common encryption (PlayReady or Widevine)</span></span>
  * <span data-ttu-id="d1310-209">**cbcs aapl**: FairPlay</span><span class="sxs-lookup"><span data-stu-id="d1310-209">**cbcs-aapl**: FairPlay</span></span>
  * <span data-ttu-id="d1310-210">**CBC**: AES envelope kryptering</span><span class="sxs-lookup"><span data-stu-id="d1310-210">**cbc**: AES envelope encryption</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="d1310-211">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="d1310-211">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="d1310-212">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="d1310-212">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="d1310-213">Lägg till följande element för hello**appSettings** definieras i filen app.config:</span><span class="sxs-lookup"><span data-stu-id="d1310-213">Add hello following elements too**appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a><span data-ttu-id="d1310-214">Exempel</span><span class="sxs-lookup"><span data-stu-id="d1310-214">Example</span></span>

<span data-ttu-id="d1310-215">hello som följande exempel visar hello möjlighet toouse Media Services toodeliver ditt innehåll som krypterats med FairPlay.</span><span class="sxs-lookup"><span data-stu-id="d1310-215">hello following sample demonstrates hello ability toouse Media Services toodeliver your content encrypted with FairPlay.</span></span> <span data-ttu-id="d1310-216">Den här funktionen introducerades i hello Azure Media Services SDK för .NET version 3.6.0.</span><span class="sxs-lookup"><span data-stu-id="d1310-216">This functionality was introduced in hello Azure Media Services SDK for .NET version 3.6.0.</span></span> 

<span data-ttu-id="d1310-217">Skriv över hello koden i Program.cs-filen med hello koden som visas i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="d1310-217">Overwrite hello code in your Program.cs file with hello code shown in this section.</span></span>

>[!NOTE]
><span data-ttu-id="d1310-218">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="d1310-218">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="d1310-219">Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer).</span><span class="sxs-lookup"><span data-stu-id="d1310-219">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="d1310-220">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d1310-220">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="d1310-221">Se till att tooupdate variabler toopoint toofolders där dina indatafiler finns.</span><span class="sxs-lookup"><span data-stu-id="d1310-221">Make sure tooupdate variables toopoint toofolders where your input files are located.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
    using Newtonsoft.Json;
    using System.Security.Cryptography.X509Certificates;

    namespace DynamicEncryptionWithFairPlay
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

            IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
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

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);

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

            IJob job = _context.Jobs.Create(String.Format("Encoding {0}", inputAsset.Name));

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

        static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
        {
            // Create HLS SAMPLE AES encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryptionCbcs);

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


            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();

            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
            ContentKeyDeliveryType.FairPlay,
            restrictions,
            FairPlayConfiguration);


            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

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

            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();


            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                   ContentKeyDeliveryType.FairPlay,
                   restrictions,
                   FairPlayConfiguration);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate hello content key authorization policy with hello content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        private static string ConfigureFairPlayPolicyOptions()
        {
            // For testing you can provide all zeroes for ASK bytes together with hello cert from Apple FPS SDK.
            // However, for production you must use a real ASK from Apple bound tooa real prod certificate.
            byte[] askBytes = Guid.NewGuid().ToByteArray();
            var askId = Guid.NewGuid();
            // Key delivery retrieves askKey by askId and uses this key toogenerate hello response.
            IContentKey askKey = _context.ContentKeys.Create(
                        askId,
                        askBytes,
                        "askKey",
                        ContentKeyType.FairPlayASk);

            //Customer password for creating hello .pfx file.
            string pfxPassword = "<customer password for creating hello .pfx file>";
            // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key toogenerate hello response.
            var pfxPasswordId = Guid.NewGuid();
            byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
            IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                        pfxPasswordId,
                        pfxPasswordBytes,
                        "pfxPasswordKey",
                        ContentKeyType.FairPlayPfxPassword);

            // iv - 16 bytes random value, must match hello iv in hello asset delivery policy.
            byte[] iv = Guid.NewGuid().ToByteArray();

            //Specify hello .pfx file created by hello customer.
            var appCert = new X509Certificate2("path toohello .pfx file created by hello customer", pfxPassword, X509KeyStorageFlags.Exportable);

            string FairPlayConfiguration =
            Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                appCert,
                pfxPassword,
                pfxPasswordId,
                askId,
                iv);

            return FairPlayConfiguration;
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

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();

            var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);

            FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);

            // Get hello FairPlay license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);

            // hello reason hello below code replaces "https://" with "skd://" is because
            // in hello IOS player sample code which you obtained in Apple developer account,
            // hello player only recognizes a Key URL that starts with skd://.
            // However, if you are using a customized player,
            // you can choose whatever protocol you want.
            // For example, "https".

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                    {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
            };

            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
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


## <a name="next-steps-media-services-learning-paths"></a><span data-ttu-id="d1310-222">Nästa steg: Utbildningsvägar för Media Services</span><span class="sxs-lookup"><span data-stu-id="d1310-222">Next steps: Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d1310-223">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="d1310-223">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
