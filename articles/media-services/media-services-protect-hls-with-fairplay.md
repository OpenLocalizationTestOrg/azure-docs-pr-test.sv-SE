---
title: "Skydda innehåll med Microsoft PlayReady- eller Apple FairPlay - Azure HLS | Microsoft Docs"
description: "Det här avsnittet ger en översikt och visar hur du använder Azure Media Services för att kryptera dynamiskt innehåll med Apple FairPlay HTTP Live Streaming (HLS). Den visar även hur du använder Media Services licensleveranstjänst för att leverera FairPlay-licenser till klienter."
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
ms.openlocfilehash: 895d6307b1cef74e195cc2ffd8dbef4196e97b1f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="protect-your-hls-content-with-apple-fairplay-or-microsoft-playready"></a><span data-ttu-id="60b6a-104">Skydda ditt innehåll med Apple FairPlay eller Microsoft PlayReady HLS</span><span class="sxs-lookup"><span data-stu-id="60b6a-104">Protect your HLS content with Apple FairPlay or Microsoft PlayReady</span></span>
<span data-ttu-id="60b6a-105">Azure Media Services kan du kryptera dynamiskt innehåll HTTP Live Streaming (HLS) med hjälp av följande format:</span><span class="sxs-lookup"><span data-stu-id="60b6a-105">Azure Media Services enables you to dynamically encrypt your HTTP Live Streaming (HLS) content by using the following formats:</span></span>  

* <span data-ttu-id="60b6a-106">**AES-128 kuvert klartextnyckeln**</span><span class="sxs-lookup"><span data-stu-id="60b6a-106">**AES-128 envelope clear key**</span></span>

    <span data-ttu-id="60b6a-107">Det hela segmentet krypteras med hjälp av den **AES-128 CBC** läge.</span><span class="sxs-lookup"><span data-stu-id="60b6a-107">The entire chunk is encrypted by using the **AES-128 CBC** mode.</span></span> <span data-ttu-id="60b6a-108">Dekryptering av dataströmmen stöds internt av iOS och OS X spelare.</span><span class="sxs-lookup"><span data-stu-id="60b6a-108">The decryption of the stream is supported by iOS and OS X player natively.</span></span> <span data-ttu-id="60b6a-109">Mer information finns i [dynamisk med hjälp av AES-128-kryptering och viktiga leverans tjänsten](media-services-protect-with-aes128.md).</span><span class="sxs-lookup"><span data-stu-id="60b6a-109">For more information, see [Using AES-128 dynamic encryption and key delivery service](media-services-protect-with-aes128.md).</span></span>
* <span data-ttu-id="60b6a-110">**Apple FairPlay**</span><span class="sxs-lookup"><span data-stu-id="60b6a-110">**Apple FairPlay**</span></span>

    <span data-ttu-id="60b6a-111">Enskilda video och ljud prover krypteras med hjälp av den **AES-128 CBC** läge.</span><span class="sxs-lookup"><span data-stu-id="60b6a-111">The individual video and audio samples are encrypted by using the **AES-128 CBC** mode.</span></span> <span data-ttu-id="60b6a-112">**FairPlay strömning** (FPS) är integrerade i enhetens operativsystem, med inbyggt stöd på iOS- och Apple TV.</span><span class="sxs-lookup"><span data-stu-id="60b6a-112">**FairPlay Streaming** (FPS) is integrated into the device operating systems, with native support on iOS and Apple TV.</span></span> <span data-ttu-id="60b6a-113">Safari för OS X kan FPS genom att använda stöd för krypterade Media tillägg (EME)-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="60b6a-113">Safari on OS X enables FPS by using the Encrypted Media Extensions (EME) interface support.</span></span>
* <span data-ttu-id="60b6a-114">**Microsoft PlayReady**</span><span class="sxs-lookup"><span data-stu-id="60b6a-114">**Microsoft PlayReady**</span></span>

<span data-ttu-id="60b6a-115">Följande bild visar den **HLS + FairPlay eller PlayReady dynamisk kryptering** arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="60b6a-115">The following image shows the **HLS + FairPlay or PlayReady dynamic encryption** workflow.</span></span>

![Diagram över arbetsflödet för dynamisk kryptering](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

<span data-ttu-id="60b6a-117">Det här avsnittet visar hur du använder Media Services för att kryptera din HLS innehåll med Apple FairPlay dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="60b6a-117">This topic demonstrates how to use Media Services to dynamically encrypt your HLS content with Apple FairPlay.</span></span> <span data-ttu-id="60b6a-118">Den visar även hur du använder Media Services licensleveranstjänst för att leverera FairPlay-licenser till klienter.</span><span class="sxs-lookup"><span data-stu-id="60b6a-118">It also shows how to use the Media Services license delivery service to deliver FairPlay licenses to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="60b6a-119">Om du även vill kryptera dina HLS innehåll med PlayReady måste du skapa en gemensam innehållsnyckel och associera den med din tillgång.</span><span class="sxs-lookup"><span data-stu-id="60b6a-119">If you also want to encrypt your HLS content with PlayReady, you need to create a common content key and associate it with your asset.</span></span> <span data-ttu-id="60b6a-120">Du måste också konfigurera innehållsnyckelns auktoriseringsprincip, enligt beskrivningen i [med PlayReady-kryptering för dynamiska vanliga](media-services-protect-with-drm.md).</span><span class="sxs-lookup"><span data-stu-id="60b6a-120">You also need to configure the content key’s authorization policy, as described in [Using PlayReady dynamic common encryption](media-services-protect-with-drm.md).</span></span>
>
>

## <a name="requirements-and-considerations"></a><span data-ttu-id="60b6a-121">Krav och överväganden</span><span class="sxs-lookup"><span data-stu-id="60b6a-121">Requirements and considerations</span></span>

<span data-ttu-id="60b6a-122">Följande krävs när du använder Media Services att leverera HLS som krypterats med FairPlay och för att leverera FairPlay-licenser:</span><span class="sxs-lookup"><span data-stu-id="60b6a-122">The following are required when using Media Services to deliver HLS encrypted with FairPlay, and to deliver FairPlay licenses:</span></span>

  * <span data-ttu-id="60b6a-123">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="60b6a-123">An Azure account.</span></span> <span data-ttu-id="60b6a-124">Mer information finns i avsnittet om [den kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="60b6a-124">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
  * <span data-ttu-id="60b6a-125">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="60b6a-125">A Media Services account.</span></span> <span data-ttu-id="60b6a-126">Om du vill skapa en finns [skapa ett Azure Media Services-konto med hjälp av Azure portal](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="60b6a-126">To create one, see [Create an Azure Media Services account using the Azure portal](media-services-portal-create-account.md).</span></span>
  * <span data-ttu-id="60b6a-127">Logga med [Apple Development Program](https://developer.apple.com/).</span><span class="sxs-lookup"><span data-stu-id="60b6a-127">Sign up with [Apple Development Program](https://developer.apple.com/).</span></span>
  * <span data-ttu-id="60b6a-128">Apple kräver innehållets ägare att hämta den [distributionspaketet](https://developer.apple.com/contact/fps/).</span><span class="sxs-lookup"><span data-stu-id="60b6a-128">Apple requires the content owner to obtain the [deployment package](https://developer.apple.com/contact/fps/).</span></span> <span data-ttu-id="60b6a-129">Tillstånd redan implementerats nyckeln Security Module (KSM) med Media Services och att du begär slutliga FPS paketet.</span><span class="sxs-lookup"><span data-stu-id="60b6a-129">State that you already implemented Key Security Module (KSM) with Media Services, and that you are requesting the final FPS package.</span></span> <span data-ttu-id="60b6a-130">Det finns instruktioner i sista FPS paketet för att generera certifiering och hämta program hemlig nyckel (be).</span><span class="sxs-lookup"><span data-stu-id="60b6a-130">There are instructions in the final FPS package to generate certification and obtain the Application Secret Key (ASK).</span></span> <span data-ttu-id="60b6a-131">Du kan använda be för att konfigurera FairPlay.</span><span class="sxs-lookup"><span data-stu-id="60b6a-131">You use ASK to configure FairPlay.</span></span>
  * <span data-ttu-id="60b6a-132">Azure Media Services .NET SDK-versionen **3.6.0** eller senare.</span><span class="sxs-lookup"><span data-stu-id="60b6a-132">Azure Media Services .NET SDK version **3.6.0** or later.</span></span>

<span data-ttu-id="60b6a-133">Följande måste anges på Media Services viktiga leverans sida:</span><span class="sxs-lookup"><span data-stu-id="60b6a-133">The following things must be set on Media Services key delivery side:</span></span>

  * <span data-ttu-id="60b6a-134">**Appen Cert (nät)**: Detta är en .pfx-fil som innehåller den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="60b6a-134">**App Cert (AC)**: This is a .pfx file that contains the private key.</span></span> <span data-ttu-id="60b6a-135">Du skapar den här filen och kryptera den med ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="60b6a-135">You create this file and encrypt it with a password.</span></span>

       <span data-ttu-id="60b6a-136">När du konfigurerar en leveransprincip för viktiga, måste du ange lösenordet och PFX-filen i Base64-format.</span><span class="sxs-lookup"><span data-stu-id="60b6a-136">When you configure a key delivery policy, you must provide that password and the .pfx file in Base64 format.</span></span>

      <span data-ttu-id="60b6a-137">Följande steg beskriver hur du skapar en PFX-fil för FairPlay:</span><span class="sxs-lookup"><span data-stu-id="60b6a-137">The following steps describe how to generate a .pfx certificate file for FairPlay:</span></span>

    1. <span data-ttu-id="60b6a-138">Installera OpenSSL från https://slproweb.com/products/Win32OpenSSL.html.</span><span class="sxs-lookup"><span data-stu-id="60b6a-138">Install OpenSSL from https://slproweb.com/products/Win32OpenSSL.html.</span></span>

        <span data-ttu-id="60b6a-139">Gå till den mapp där FairPlay-certifikat och andra filer som levereras av Apple.</span><span class="sxs-lookup"><span data-stu-id="60b6a-139">Go to the folder where the FairPlay certificate and other files delivered by Apple are.</span></span>
    2. <span data-ttu-id="60b6a-140">Kör följande kommando från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="60b6a-140">Run the following command from the command line.</span></span> <span data-ttu-id="60b6a-141">Detta konverterar CER-filen till en PEM-filen.</span><span class="sxs-lookup"><span data-stu-id="60b6a-141">This converts the .cer file to a .pem file.</span></span>

        <span data-ttu-id="60b6a-142">”C:\OpenSSL-Win32\bin\openssl.exe” x509-informera der-i fairplay.cer-ut fairplay out.pem</span><span class="sxs-lookup"><span data-stu-id="60b6a-142">"C:\OpenSSL-Win32\bin\openssl.exe" x509 -inform der -in fairplay.cer -out fairplay-out.pem</span></span>
    3. <span data-ttu-id="60b6a-143">Kör följande kommando från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="60b6a-143">Run the following command from the command line.</span></span> <span data-ttu-id="60b6a-144">Detta konverterar PEM-filen till en .pfx-fil med den privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="60b6a-144">This converts the .pem file to a .pfx file with the private key.</span></span> <span data-ttu-id="60b6a-145">Lösenordet för PFX-filen blir sedan ombedd av OpenSSL.</span><span class="sxs-lookup"><span data-stu-id="60b6a-145">The password for the .pfx file is then asked by OpenSSL.</span></span>

        <span data-ttu-id="60b6a-146">”C:\OpenSSL-Win32\bin\openssl.exe” pkcs12-exportera - ut fairplay out.pfx-inkey privatekey.pem-i fairplay out.pem - passin file:privatekey-pem-pass.txt</span><span class="sxs-lookup"><span data-stu-id="60b6a-146">"C:\OpenSSL-Win32\bin\openssl.exe" pkcs12 -export -out fairplay-out.pfx -inkey privatekey.pem -in fairplay-out.pem -passin file:privatekey-pem-pass.txt</span></span>
  * <span data-ttu-id="60b6a-147">**Appen Cert lösenord**: lösenordet för att skapa en .pfx-fil.</span><span class="sxs-lookup"><span data-stu-id="60b6a-147">**App Cert password**: The password for creating the .pfx file.</span></span>
  * <span data-ttu-id="60b6a-148">**ID för appen Cert lösenord**: du måste överföra lösenordet, som liknar hur de överför andra Media Services-nycklar.</span><span class="sxs-lookup"><span data-stu-id="60b6a-148">**App Cert password ID**: You must upload the password, similar to how they upload other Media Services keys.</span></span> <span data-ttu-id="60b6a-149">Använd den **ContentKeyType.FairPlayPfxPassword** enum-värde för att hämta ID för Media Services</span><span class="sxs-lookup"><span data-stu-id="60b6a-149">Use the **ContentKeyType.FairPlayPfxPassword** enum value to get the Media Services ID.</span></span> <span data-ttu-id="60b6a-150">Det här är vad de behöver för att använda inuti alternativet viktiga leverans.</span><span class="sxs-lookup"><span data-stu-id="60b6a-150">This is what they need to use inside the key delivery policy option.</span></span>
  * <span data-ttu-id="60b6a-151">**IV**: Detta är ett slumpmässigt värde av 16 byte.</span><span class="sxs-lookup"><span data-stu-id="60b6a-151">**iv**: This is a random value of 16 bytes.</span></span> <span data-ttu-id="60b6a-152">Den måste matcha iv i principen för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="60b6a-152">It must match the iv in the asset delivery policy.</span></span> <span data-ttu-id="60b6a-153">Du genererar iv och placera den på båda platserna: tillgångsleveransprincip och alternativet viktiga leverans.</span><span class="sxs-lookup"><span data-stu-id="60b6a-153">You generate the iv, and put it in both places: the asset delivery policy and the key delivery policy option.</span></span>
  * <span data-ttu-id="60b6a-154">**Be**: den här nyckeln tas emot när du skapar ett certifikat med hjälp av Apple Developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="60b6a-154">**ASK**: This key is received when you generate the certification by using the Apple Developer portal.</span></span> <span data-ttu-id="60b6a-155">Varje Utvecklingsteamet får en unik be.</span><span class="sxs-lookup"><span data-stu-id="60b6a-155">Each development team will receive a unique ASK.</span></span> <span data-ttu-id="60b6a-156">Spara en kopia av be och lagra den på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="60b6a-156">Save a copy of the ASK, and store it in a safe place.</span></span> <span data-ttu-id="60b6a-157">Du behöver konfigurera be som FairPlayAsk till Media Services senare.</span><span class="sxs-lookup"><span data-stu-id="60b6a-157">You will need to configure ASK as FairPlayAsk to Media Services later.</span></span>
  * <span data-ttu-id="60b6a-158">**Be ID**: Detta ID hämtas när du överför be till Media Services.</span><span class="sxs-lookup"><span data-stu-id="60b6a-158">**ASK ID**: This ID is obtained when you upload ASK into Media Services.</span></span> <span data-ttu-id="60b6a-159">Du måste överföra be med hjälp av den **ContentKeyType.FairPlayAsk** enum-värde.</span><span class="sxs-lookup"><span data-stu-id="60b6a-159">You must upload ASK by using the **ContentKeyType.FairPlayAsk** enum value.</span></span> <span data-ttu-id="60b6a-160">Media Services ID returneras som ett resultat, och det här är vad ska användas när du ställer in alternativet viktiga leverans.</span><span class="sxs-lookup"><span data-stu-id="60b6a-160">As the result, the Media Services ID is returned, and this is what should be used when setting the key delivery policy option.</span></span>

<span data-ttu-id="60b6a-161">Följande måste anges av klientsidan FPS:</span><span class="sxs-lookup"><span data-stu-id="60b6a-161">The following things must be set by the FPS client side:</span></span>

  * <span data-ttu-id="60b6a-162">**Appen Cert (nät)**: Detta är en.cer/.der-fil som innehåller den offentliga nyckeln som operativsystemet använder för att kryptera vissa nyttolast.</span><span class="sxs-lookup"><span data-stu-id="60b6a-162">**App Cert (AC)**: This is a .cer/.der file that contains the public key, which the operating system uses to encrypt some payload.</span></span> <span data-ttu-id="60b6a-163">Media Services behöver veta om den eftersom det krävs av Windows Media player.</span><span class="sxs-lookup"><span data-stu-id="60b6a-163">Media Services needs to know about it because it is required by the player.</span></span> <span data-ttu-id="60b6a-164">Tjänsten key dekrypterar den med hjälp av motsvarande privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="60b6a-164">The key delivery service decrypts it using the corresponding private key.</span></span>

<span data-ttu-id="60b6a-165">Skaffa en verklig be först om du vill spela upp en krypterad dataström FairPlay och generera ett verkliga certifikat.</span><span class="sxs-lookup"><span data-stu-id="60b6a-165">To play back a FairPlay encrypted stream, get a real ASK first, and then generate a real certificate.</span></span> <span data-ttu-id="60b6a-166">Den här processen skapar alla tre delar:</span><span class="sxs-lookup"><span data-stu-id="60b6a-166">That process creates all three parts:</span></span>

  * <span data-ttu-id="60b6a-167">.der fil</span><span class="sxs-lookup"><span data-stu-id="60b6a-167">.der file</span></span>
  * <span data-ttu-id="60b6a-168">.pfx-fil</span><span class="sxs-lookup"><span data-stu-id="60b6a-168">.pfx file</span></span>
  * <span data-ttu-id="60b6a-169">lösenordet för PFX</span><span class="sxs-lookup"><span data-stu-id="60b6a-169">password for the .pfx</span></span>

<span data-ttu-id="60b6a-170">Följande klienter stöder HLS med **AES-128 CBC** kryptering: Safari på OS X, Apple TV iOS.</span><span class="sxs-lookup"><span data-stu-id="60b6a-170">The following clients support HLS with **AES-128 CBC** encryption: Safari on OS X, Apple TV, iOS.</span></span>

## <a name="configure-fairplay-dynamic-encryption-and-license-delivery-services"></a><span data-ttu-id="60b6a-171">Konfigurera FairPlay dynamisk kryptering och licensen för leverans av tjänster</span><span class="sxs-lookup"><span data-stu-id="60b6a-171">Configure FairPlay dynamic encryption and license delivery services</span></span>
<span data-ttu-id="60b6a-172">Följande är allmänna steg för att skydda dina tillgångar med FairPlay med hjälp av Media Services licensleveranstjänst och även med hjälp av dynamisk kryptering.</span><span class="sxs-lookup"><span data-stu-id="60b6a-172">The following are general steps for protecting your assets with FairPlay by using the Media Services license delivery service, and also by using dynamic encryption.</span></span>

1. <span data-ttu-id="60b6a-173">Skapa en tillgång och överföra filer till tillgången.</span><span class="sxs-lookup"><span data-stu-id="60b6a-173">Create an asset, and upload files into the asset.</span></span>
2. <span data-ttu-id="60b6a-174">Koda den tillgång som innehåller filen för MP4-uppsättningen med anpassad bithastighet.</span><span class="sxs-lookup"><span data-stu-id="60b6a-174">Encode the asset that contains the file to the adaptive bitrate MP4 set.</span></span>
3. <span data-ttu-id="60b6a-175">Skapa en innehållsnyckel och associera den med den kodade tillgången.</span><span class="sxs-lookup"><span data-stu-id="60b6a-175">Create a content key, and associate it with the encoded asset.</span></span>  
4. <span data-ttu-id="60b6a-176">Konfigurera innehållsnyckelns auktoriseringsprincip.</span><span class="sxs-lookup"><span data-stu-id="60b6a-176">Configure the content key’s authorization policy.</span></span> <span data-ttu-id="60b6a-177">Ange följande:</span><span class="sxs-lookup"><span data-stu-id="60b6a-177">Specify the following:</span></span>

   * <span data-ttu-id="60b6a-178">Leveransmetod (i det här fallet FairPlay).</span><span class="sxs-lookup"><span data-stu-id="60b6a-178">The delivery method (in this case, FairPlay).</span></span>
   * <span data-ttu-id="60b6a-179">FairPlay alternativkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="60b6a-179">FairPlay policy options configuration.</span></span> <span data-ttu-id="60b6a-180">Mer information om hur du konfigurerar FairPlay finns i **ConfigureFairPlayPolicyOptions()** metod i exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="60b6a-180">For details on how to configure FairPlay, see the **ConfigureFairPlayPolicyOptions()** method in the sample below.</span></span>

     > [!NOTE]
     > <span data-ttu-id="60b6a-181">Vanligtvis vill du konfigurera alternativ för FairPlay princip bara en gång, eftersom du bara har en uppsättning av en certifikatutfärdare och en fråga.</span><span class="sxs-lookup"><span data-stu-id="60b6a-181">Usually, you would want to configure FairPlay policy options only once, because you will only have one set of a certification and an ASK.</span></span>
     >
     >
   * <span data-ttu-id="60b6a-182">Begränsningar (öppen eller token).</span><span class="sxs-lookup"><span data-stu-id="60b6a-182">Restrictions (open or token).</span></span>
   * <span data-ttu-id="60b6a-183">Information som är specifik för nyckelleveranstypen och som definierar hur nyckeln levereras till klienten.</span><span class="sxs-lookup"><span data-stu-id="60b6a-183">Information specific to the key delivery type that defines how the key is delivered to the client.</span></span>
5. <span data-ttu-id="60b6a-184">Konfigurera i principen för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="60b6a-184">Configure the asset delivery policy.</span></span> <span data-ttu-id="60b6a-185">Konfigurationen för leveransprincipen omfattar:</span><span class="sxs-lookup"><span data-stu-id="60b6a-185">The delivery policy configuration includes:</span></span>

   * <span data-ttu-id="60b6a-186">Leveransprotokoll (HLS).</span><span class="sxs-lookup"><span data-stu-id="60b6a-186">The delivery protocol (HLS).</span></span>
   * <span data-ttu-id="60b6a-187">Typen av dynamisk kryptering (vanliga CBC kryptering).</span><span class="sxs-lookup"><span data-stu-id="60b6a-187">The type of dynamic encryption (common CBC encryption).</span></span>
   * <span data-ttu-id="60b6a-188">URL för anskaffning av licens.</span><span class="sxs-lookup"><span data-stu-id="60b6a-188">The license acquisition URL.</span></span>

     > [!NOTE]
     > <span data-ttu-id="60b6a-189">Du måste konfigurera separata leveransprinciperna om du vill leverera en ström som har krypterats med FairPlay och en annan Digital Rights Management (DRM) system:</span><span class="sxs-lookup"><span data-stu-id="60b6a-189">If you want to deliver a stream that is encrypted with FairPlay and another Digital Rights Management (DRM) system, you have to configure separate delivery policies:</span></span>
     >
     > * <span data-ttu-id="60b6a-190">En IAssetDeliveryPolicy att konfigurera dynamisk anpassningsbar strömning via HTTP-(bindestreck) med vanliga kryptering (CENC) (PlayReady och Widevine) och Smooth med PlayReady</span><span class="sxs-lookup"><span data-stu-id="60b6a-190">One IAssetDeliveryPolicy to configure Dynamic Adaptive Streaming over HTTP (DASH) with Common Encryption (CENC) (PlayReady + Widevine), and Smooth with PlayReady</span></span>
     > * <span data-ttu-id="60b6a-191">En annan IAssetDeliveryPolicy att konfigurera FairPlay för HLS</span><span class="sxs-lookup"><span data-stu-id="60b6a-191">Another IAssetDeliveryPolicy to configure FairPlay for HLS</span></span>
     >
     >
6. <span data-ttu-id="60b6a-192">Skapa en OnDemand-positionerare för att få en strömnings-URL.</span><span class="sxs-lookup"><span data-stu-id="60b6a-192">Create an OnDemand locator to get a streaming URL.</span></span>

## <a name="use-fairplay-key-delivery-by-player-apps"></a><span data-ttu-id="60b6a-193">Använd FairPlay viktiga leverans av player appar</span><span class="sxs-lookup"><span data-stu-id="60b6a-193">Use FairPlay key delivery by player apps</span></span>
<span data-ttu-id="60b6a-194">Du kan utveckla player appar med hjälp av iOS SDK.</span><span class="sxs-lookup"><span data-stu-id="60b6a-194">You can develop player apps by using the iOS SDK.</span></span> <span data-ttu-id="60b6a-195">Om du vill kunna spela upp FairPlay innehåll som du behöver implementera licens exchange-protokollet.</span><span class="sxs-lookup"><span data-stu-id="60b6a-195">To be able to play FairPlay content, you have to implement the license exchange protocol.</span></span> <span data-ttu-id="60b6a-196">Det här protokollet har inte angetts av Apple.</span><span class="sxs-lookup"><span data-stu-id="60b6a-196">This protocol is not specified by Apple.</span></span> <span data-ttu-id="60b6a-197">Det är varje app så att skicka begäranden viktiga leverans.</span><span class="sxs-lookup"><span data-stu-id="60b6a-197">It is up to each app how to send key delivery requests.</span></span> <span data-ttu-id="60b6a-198">Media Services FairPlay viktiga tjänsten förväntar SPC komma som meddelandet www-form-url kodad post i följande format:</span><span class="sxs-lookup"><span data-stu-id="60b6a-198">The Media Services FairPlay key delivery service expects the SPC to come as a www-form-url encoded post message, in the following form:</span></span>

    spc=<Base64 encoded SPC>

> [!NOTE]
> <span data-ttu-id="60b6a-199">Azure Media Player stöder inte FairPlay uppspelning direkt.</span><span class="sxs-lookup"><span data-stu-id="60b6a-199">Azure Media Player doesn’t support FairPlay playback out of the box.</span></span> <span data-ttu-id="60b6a-200">Hämta exempel player från Apple developer-konto för att få FairPlay uppspelning i MAC OS X.</span><span class="sxs-lookup"><span data-stu-id="60b6a-200">To get FairPlay playback on MAC OS X, obtain the sample player from the Apple developer account.</span></span>
>
>

## <a name="streaming-urls"></a><span data-ttu-id="60b6a-201">Strömmande URL: er</span><span class="sxs-lookup"><span data-stu-id="60b6a-201">Streaming URLs</span></span>
<span data-ttu-id="60b6a-202">Om din tillgång har krypterats med flera DRM, bör du använda en kryptering-tagg i strömnings-URL: (format = 'm3u8 aapl', kryptering = ”xxx”).</span><span class="sxs-lookup"><span data-stu-id="60b6a-202">If your asset was encrypted with more than one DRM, you should use an encryption tag in the streaming URL: (format='m3u8-aapl', encryption='xxx').</span></span>

<span data-ttu-id="60b6a-203">Följande gäller:</span><span class="sxs-lookup"><span data-stu-id="60b6a-203">The following considerations apply:</span></span>

* <span data-ttu-id="60b6a-204">Endast noll eller en typ av enhetskryptering kan anges.</span><span class="sxs-lookup"><span data-stu-id="60b6a-204">Only zero or one encryption type can be specified.</span></span>
* <span data-ttu-id="60b6a-205">Krypteringstypen behöver inte anges i URL: en om bara en kryptering har tillämpats på tillgången.</span><span class="sxs-lookup"><span data-stu-id="60b6a-205">The encryption type doesn't have to be specified in the URL if only one encryption was applied to the asset.</span></span>
* <span data-ttu-id="60b6a-206">Krypteringstypen är skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="60b6a-206">The encryption type is case insensitive.</span></span>
* <span data-ttu-id="60b6a-207">Följande krypteringstyper av kan anges:</span><span class="sxs-lookup"><span data-stu-id="60b6a-207">The following encryption types can be specified:</span></span>  
  * <span data-ttu-id="60b6a-208">**cenc**: Common encryption (PlayReady eller Widevine)</span><span class="sxs-lookup"><span data-stu-id="60b6a-208">**cenc**:  Common encryption (PlayReady or Widevine)</span></span>
  * <span data-ttu-id="60b6a-209">**cbcs aapl**: FairPlay</span><span class="sxs-lookup"><span data-stu-id="60b6a-209">**cbcs-aapl**: FairPlay</span></span>
  * <span data-ttu-id="60b6a-210">**CBC**: AES envelope kryptering</span><span class="sxs-lookup"><span data-stu-id="60b6a-210">**cbc**: AES envelope encryption</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="60b6a-211">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="60b6a-211">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="60b6a-212">Konfigurera utvecklingsmiljön och fyll i filen app.config med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="60b6a-212">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="60b6a-213">Lägg till följande element i **appSettings** som definieras i filen app.config:</span><span class="sxs-lookup"><span data-stu-id="60b6a-213">Add the following elements to **appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a><span data-ttu-id="60b6a-214">Exempel</span><span class="sxs-lookup"><span data-stu-id="60b6a-214">Example</span></span>

<span data-ttu-id="60b6a-215">I följande exempel visar möjligheten att använda Media Services kan leverera ditt innehåll som krypterats med FairPlay.</span><span class="sxs-lookup"><span data-stu-id="60b6a-215">The following sample demonstrates the ability to use Media Services to deliver your content encrypted with FairPlay.</span></span> <span data-ttu-id="60b6a-216">Den här funktionen introducerades i Azure Media Services SDK för .NET version 3.6.0.</span><span class="sxs-lookup"><span data-stu-id="60b6a-216">This functionality was introduced in the Azure Media Services SDK for .NET version 3.6.0.</span></span> 

<span data-ttu-id="60b6a-217">Skriv över koden i Program.cs-filen med koden som visas i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="60b6a-217">Overwrite the code in your Program.cs file with the code shown in this section.</span></span>

>[!NOTE]
><span data-ttu-id="60b6a-218">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="60b6a-218">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="60b6a-219">Du bör använda samma princip-ID om du alltid använder samma dagar/åtkomstbehörigheter, till exempel principer för positionerare som är avsedda att vara på plats under en längre tid (icke-överföringsprinciper).</span><span class="sxs-lookup"><span data-stu-id="60b6a-219">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="60b6a-220">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="60b6a-220">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="60b6a-221">Se till att uppdatera variablerna så att de pekar på mappar där dina indatafiler finns.</span><span class="sxs-lookup"><span data-stu-id="60b6a-221">Make sure to update variables to point to folders where your input files are located.</span></span>

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
        // Read values from the App.config file.
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
            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
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

            // Generate a test token based on the the data in the given TokenRestrictionTemplate.
            // Note, you need to pass the key id Guid because we specified
            // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                        DateTime.UtcNow.AddDays(365));
            Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
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

            // Associate the key with the asset.
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

            // Associate the content key authorization policy with the content key.
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

            // Associate the content key authorization policy with the content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        private static string ConfigureFairPlayPolicyOptions()
        {
            // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK.
            // However, for production you must use a real ASK from Apple bound to a real prod certificate.
            byte[] askBytes = Guid.NewGuid().ToByteArray();
            var askId = Guid.NewGuid();
            // Key delivery retrieves askKey by askId and uses this key to generate the response.
            IContentKey askKey = _context.ContentKeys.Create(
                        askId,
                        askBytes,
                        "askKey",
                        ContentKeyType.FairPlayASk);

            //Customer password for creating the .pfx file.
            string pfxPassword = "<customer password for creating the .pfx file>";
            // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
            var pfxPasswordId = Guid.NewGuid();
            byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
            IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                        pfxPasswordId,
                        pfxPasswordBytes,
                        "pfxPasswordKey",
                        ContentKeyType.FairPlayPfxPassword);

            // iv - 16 bytes random value, must match the iv in the asset delivery policy.
            byte[] iv = Guid.NewGuid().ToByteArray();

            //Specify the .pfx file created by the customer.
            var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);

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

            // Get the FairPlay license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);

            // The reason the below code replaces "https://" with "skd://" is because
            // in the IOS player sample code which you obtained in Apple developer account,
            // the player only recognizes a Key URL that starts with skd://.
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

            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }


        /// <summary>
        /// Gets the streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference to the streaming manifest file from the  
            // collection of files in the asset.

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator to the streaming content on an origin.
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL to the manifest file.
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


## <a name="next-steps-media-services-learning-paths"></a><span data-ttu-id="60b6a-222">Nästa steg: Utbildningsvägar för Media Services</span><span class="sxs-lookup"><span data-stu-id="60b6a-222">Next steps: Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="60b6a-223">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="60b6a-223">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
