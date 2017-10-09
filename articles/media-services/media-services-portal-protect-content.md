---
title: "aaaConfiguring innehållsskydd principer med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Den här artikeln visar hur toouse hello Azure portal tooconfigure innehållsskydd principer. hello artikel också visar hur tooenable dynamisk kryptering för dina tillgångar."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: 3e7ce6ddaa0e738b5a1e26dafe9eef2df221f039
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-content-protection-policies-using-hello-azure-portal"></a><span data-ttu-id="16ca1-104">Konfigurera principer för innehållsskydd med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="16ca1-104">Configuring content protection policies using hello Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="16ca1-105">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="16ca1-105">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="16ca1-106">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="16ca1-106">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="16ca1-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="16ca1-107">Overview</span></span>
<span data-ttu-id="16ca1-108">Microsoft Azure Media Services (AMS) kan du toosecure media från hello tid den lämnar din dator via lagring, bearbetning och leverans.</span><span class="sxs-lookup"><span data-stu-id="16ca1-108">Microsoft Azure Media Services (AMS) enables you toosecure your media from hello time it leaves your computer through storage, processing, and delivery.</span></span> <span data-ttu-id="16ca1-109">Media Services kan du toodeliver innehållet krypteras dynamiskt med Standard AES (Advanced Encryption) (med 128-bitars krypteringsnycklar), vanliga kryptering (CENC) med PlayReady och/eller Widevine DRM och Apple FairPlay.</span><span class="sxs-lookup"><span data-stu-id="16ca1-109">Media Services allows you toodeliver your content encrypted dynamically with Advanced Encryption Standard (AES) (using 128-bit encryption keys), common encryption (CENC) using PlayReady and/or Widevine DRM, and Apple FairPlay.</span></span> 

<span data-ttu-id="16ca1-110">AMS är en tjänst för att leverera DRM-licenser och AES avmarkera nycklar tooauthorized klienter.</span><span class="sxs-lookup"><span data-stu-id="16ca1-110">AMS provides a service for delivering DRM licenses and AES clear keys tooauthorized clients.</span></span> <span data-ttu-id="16ca1-111">hello Azure-portalen kan du toocreate en **nyckel/licens auktoriseringsprincip** för alla typer av krypteringar.</span><span class="sxs-lookup"><span data-stu-id="16ca1-111">hello Azure portal enables you toocreate one **key/license authorization policy** for all types of encryptions.</span></span>

<span data-ttu-id="16ca1-112">Den här artikeln visar hur tooconfigure innehåll protection-principer med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="16ca1-112">This article demonstrates how tooconfigure content protection policies with hello Azure portal.</span></span> <span data-ttu-id="16ca1-113">hello artikel också visar hur tooapply dynamisk kryptering tooyour tillgångar.</span><span class="sxs-lookup"><span data-stu-id="16ca1-113">hello article also shows how tooapply dynamic encryption tooyour assets.</span></span>


> [!NOTE]
> <span data-ttu-id="16ca1-114">Om du har använt hello Azure klassiska portal toocreate skyddsprinciper hello principer kanske inte visas i hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="16ca1-114">If you used hello Azure classic portal toocreate protection policies, hello policies may not appear in hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="16ca1-115">Men alla hello gamla principerna fortfarande finns.</span><span class="sxs-lookup"><span data-stu-id="16ca1-115">However, all hello old polices still exist.</span></span> <span data-ttu-id="16ca1-116">Du kan kontrollera dem med hjälp av hello Azure Media Services .NET SDK eller hello [Azure Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) verktyget (toosee hello principer, högerklicka på hello tillgångar -> Visa information (F4) -> Klicka på fliken -> nycklar för Multiinnehåll Klicka på hello nyckel).</span><span class="sxs-lookup"><span data-stu-id="16ca1-116">You can examine them using hello Azure Media Services .NET SDK or hello [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) tool (toosee hello policies, right-click on hello asset -> Display information (F4)->click on Content keys tab-> click on hello key).</span></span> 
> 
> <span data-ttu-id="16ca1-117">Om du vill tooencrypt din tillgång med nya principer, konfigurera dem med hello Azure-portalen, klickar du på Spara och återanvända dynamisk kryptering.</span><span class="sxs-lookup"><span data-stu-id="16ca1-117">If you want tooencrypt your asset using new policies, configure them with hello Azure portal, click save, and reapply dynamic encryption.</span></span> 
> 
> 

## <a name="start-configuring-content-protection"></a><span data-ttu-id="16ca1-118">Börja konfigurera innehållsskydd</span><span class="sxs-lookup"><span data-stu-id="16ca1-118">Start configuring content protection</span></span>
<span data-ttu-id="16ca1-119">toouse hello portal toostart konfigurera innehållsskydd, globala tooyour AMS-kontot, hello följande:</span><span class="sxs-lookup"><span data-stu-id="16ca1-119">toouse hello portal toostart configuring content protection, global tooyour AMS account, do hello following:</span></span>

1. <span data-ttu-id="16ca1-120">I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="16ca1-120">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="16ca1-121">Välj **inställningar** > **innehåll skydd**.</span><span class="sxs-lookup"><span data-stu-id="16ca1-121">Select **Settings** > **Content protection**.</span></span>

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a><span data-ttu-id="16ca1-123">Auktoriseringsprincip för nyckel-licens</span><span class="sxs-lookup"><span data-stu-id="16ca1-123">Key/license authorization policy</span></span>
<span data-ttu-id="16ca1-124">AMS stöder flera olika sätt att autentisera användare som begär eller licensinformation.</span><span class="sxs-lookup"><span data-stu-id="16ca1-124">AMS supports multiple ways of authenticating users who make key or license requests.</span></span> <span data-ttu-id="16ca1-125">Hej innehållsnyckelns auktoriseringsprincip måste konfigureras av dig och uppfyllas av klienten för hello nyckel/licens toobe delived toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="16ca1-125">hello content key authorization policy must be configured by you and met by your client in order for hello key/license toobe delived toohello client.</span></span> <span data-ttu-id="16ca1-126">Hej innehållsnyckelns auktoriseringsprincip kan ha en eller flera auktoriseringsbegränsningar: **öppna** eller **token** begränsning.</span><span class="sxs-lookup"><span data-stu-id="16ca1-126">hello content key authorization policy could have one or more authorization restrictions: **open** or **token** restriction.</span></span>

<span data-ttu-id="16ca1-127">hello Azure-portalen kan du toocreate en **nyckel/licens auktoriseringsprincip** för alla typer av krypteringar.</span><span class="sxs-lookup"><span data-stu-id="16ca1-127">hello Azure portal enables you toocreate one **key/license authorization policy** for all types of encryptions.</span></span>

### <a name="open"></a><span data-ttu-id="16ca1-128">Öppet</span><span class="sxs-lookup"><span data-stu-id="16ca1-128">Open</span></span>
<span data-ttu-id="16ca1-129">Öppna begränsning innebär att hello system levererar hello viktiga tooanyone som begär nycklar.</span><span class="sxs-lookup"><span data-stu-id="16ca1-129">Open restriction means that hello system will deliver hello key tooanyone who makes a key request.</span></span> <span data-ttu-id="16ca1-130">Den här begränsningen kan vara användbart för testning.</span><span class="sxs-lookup"><span data-stu-id="16ca1-130">This restriction might be useful for test purposes.</span></span> 

### <a name="token"></a><span data-ttu-id="16ca1-131">Token</span><span class="sxs-lookup"><span data-stu-id="16ca1-131">Token</span></span>
<span data-ttu-id="16ca1-132">Hej tokenbegränsade principen måste åtföljas av en token som utfärdas av en säker säkerhetstokentjänst (STS).</span><span class="sxs-lookup"><span data-stu-id="16ca1-132">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="16ca1-133">Media Services stöder token i hello Simple Web Tokens (SWT) format och JSON-Webbtoken (JWT)-format.</span><span class="sxs-lookup"><span data-stu-id="16ca1-133">Media Services supports tokens in hello Simple Web Tokens (SWT) format and JSON Web Token (JWT) format.</span></span> <span data-ttu-id="16ca1-134">Media Services tillhandahåller inte Secure Token tjänster.</span><span class="sxs-lookup"><span data-stu-id="16ca1-134">Media Services does not provide Secure Token Services.</span></span> <span data-ttu-id="16ca1-135">Du kan skapa en anpassad STS eller använda Microsoft Azure ACS tooissue token.</span><span class="sxs-lookup"><span data-stu-id="16ca1-135">You can create a custom STS or leverage Microsoft Azure ACS tooissue tokens.</span></span> <span data-ttu-id="16ca1-136">hello STS måste vara konfigurerade toocreate en token som signerats med hello angetts nyckel och utfärda anspråk som du angav i hello tokenbegränsningar konfiguration.</span><span class="sxs-lookup"><span data-stu-id="16ca1-136">hello STS must be configured toocreate a token signed with hello specified key and issue claims that you specified in hello token restriction configuration.</span></span> <span data-ttu-id="16ca1-137">hello Media Services viktiga leveranser returnerar hello begärt (eller licensinformation) toohello klienten om hello token är giltig och hello anspråk i hello token matchar de som konfigurerats för hello (eller licensinformation).</span><span class="sxs-lookup"><span data-stu-id="16ca1-137">hello Media Services key delivery service will return hello requested key (or license) toohello client if hello token is valid and hello claims in hello token match those configured for hello key (or license).</span></span>

<span data-ttu-id="16ca1-138">Du måste ange hello primära Verifieringsnyckeln, utfärdaren och målgrupp parametrar när du konfigurerar hello tokenbegränsade principen.</span><span class="sxs-lookup"><span data-stu-id="16ca1-138">When configuring hello token restricted policy, you must specify hello primary verification key, issuer, and audience parameters.</span></span> <span data-ttu-id="16ca1-139">hello primära Verifieringsnyckeln innehåller hello nyckel att hello token som signerats med är utfärdaren hello säker tokentjänsten som utfärdar hello-token.</span><span class="sxs-lookup"><span data-stu-id="16ca1-139">hello primary verification key contains hello key that hello token was signed with, issuer is hello secure token service that issues hello token.</span></span> <span data-ttu-id="16ca1-140">hello målgruppen (kallas ibland för scope) beskrivs hello syftet med hello-token eller hello resurs hello token auktoriserar åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="16ca1-140">hello audience (sometimes called scope) describes hello intent of hello token or hello resource hello token authorizes access to.</span></span> <span data-ttu-id="16ca1-141">hello Media Services viktiga delivery service verifierar att dessa värden i hello token matchar hello värden i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="16ca1-141">hello Media Services key delivery service validates that these values in hello token match hello values in hello template.</span></span>

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a><span data-ttu-id="16ca1-143">PlayReady-rättighetsprincipmall</span><span class="sxs-lookup"><span data-stu-id="16ca1-143">PlayReady rights template</span></span>
<span data-ttu-id="16ca1-144">Detaljerad information om hello PlayReady rättighetsprincipmall finns [Media Services PlayReady licens mall översikt](media-services-playready-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="16ca1-144">For detailed information about hello PlayReady rights template, see [Media Services PlayReady License Template Overview](media-services-playready-license-template-overview.md).</span></span>

### <a name="non-persistent"></a><span data-ttu-id="16ca1-145">Icke beständiga</span><span class="sxs-lookup"><span data-stu-id="16ca1-145">Non persistent</span></span>
<span data-ttu-id="16ca1-146">Om du konfigurerar licens som icke-beständig lagras den bara i minnet medan hello player använder hello-licens.</span><span class="sxs-lookup"><span data-stu-id="16ca1-146">If you configure license as non-persistent, it is only held in memory while hello player is using hello license.</span></span>  

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a><span data-ttu-id="16ca1-148">Beständiga</span><span class="sxs-lookup"><span data-stu-id="16ca1-148">Persistent</span></span>
<span data-ttu-id="16ca1-149">Om du konfigurerar hello licens som beständiga sparas den i permanent lagringsutrymme på hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="16ca1-149">If you configure hello license  as persistent, it is saved in persistent storage on hello client.</span></span>

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a><span data-ttu-id="16ca1-151">Widevine rättighetsprincipmall</span><span class="sxs-lookup"><span data-stu-id="16ca1-151">Widevine rights template</span></span>
<span data-ttu-id="16ca1-152">Detaljerad information om hello Widevine rättighetsprincipmall finns [Widevine-licens mall översikt](media-services-widevine-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="16ca1-152">For detailed information about hello Widevine rights template, see [Widevine License Template Overview](media-services-widevine-license-template-overview.md).</span></span>

### <a name="basic"></a><span data-ttu-id="16ca1-153">Basic</span><span class="sxs-lookup"><span data-stu-id="16ca1-153">Basic</span></span>
<span data-ttu-id="16ca1-154">När du väljer **grundläggande**, hello mallen kommer att skapas med alla standardvärden värden.</span><span class="sxs-lookup"><span data-stu-id="16ca1-154">When you select **Basic**, hello template will be created with all defaults values.</span></span>

### <a name="advanced"></a><span data-ttu-id="16ca1-155">Advanced</span><span class="sxs-lookup"><span data-stu-id="16ca1-155">Advanced</span></span>
<span data-ttu-id="16ca1-156">Mer detaljerad information om avancerade alternativ för Widevine konfigurationer finns [detta](media-services-widevine-license-template-overview.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="16ca1-156">For detailed explanation about advance option of Widevine configurations, see [this](media-services-widevine-license-template-overview.md) topic.</span></span>

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a><span data-ttu-id="16ca1-158">FairPlay-konfiguration</span><span class="sxs-lookup"><span data-stu-id="16ca1-158">FairPlay configuration</span></span>
<span data-ttu-id="16ca1-159">tooenable FairPlay kryptering, behöver du tooprovide hello App certifikat och programmet hemlig nyckel (be) via hello FairPlay konfigurationsalternativet.</span><span class="sxs-lookup"><span data-stu-id="16ca1-159">tooenable FairPlay encryption, you need tooprovide hello App Certificate and Application Secret Key (ASK) through hello FairPlay Configuration option.</span></span> <span data-ttu-id="16ca1-160">Detaljerad information om FairPlay konfiguration och krav finns [detta](media-services-protect-hls-with-fairplay.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="16ca1-160">For detailed information about FairPlay configuration and requirements, see [this](media-services-protect-hls-with-fairplay.md) article.</span></span>

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-tooyour-asset"></a><span data-ttu-id="16ca1-162">Använda dynamisk kryptering tooyour tillgångsinformation</span><span class="sxs-lookup"><span data-stu-id="16ca1-162">Apply dynamic encryption tooyour asset</span></span>
<span data-ttu-id="16ca1-163">tootake nytta av dynamisk kryptering du behöver tooencode källfilen till en uppsättning MP4-filer med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="16ca1-163">tootake advantage of dynamic encryption, you need tooencode your source file into a set of adaptive-bitrate MP4 files.</span></span>

### <a name="select-an-asset-that-you-want-tooencrypt"></a><span data-ttu-id="16ca1-164">Välj en tillgång som du vill tooencrypt</span><span class="sxs-lookup"><span data-stu-id="16ca1-164">Select an asset that you want tooencrypt</span></span>
<span data-ttu-id="16ca1-165">toosee dina tillgångar väljer **inställningar** > **tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="16ca1-165">toosee all your assets, select **Settings** > **Assets**.</span></span>

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a><span data-ttu-id="16ca1-167">Kryptera med AES eller DRM</span><span class="sxs-lookup"><span data-stu-id="16ca1-167">Encrypt with AES or DRM</span></span>
<span data-ttu-id="16ca1-168">När du trycker på **kryptera** på en tillgång, visas två alternativ: **AES** eller **DRM**.</span><span class="sxs-lookup"><span data-stu-id="16ca1-168">Once you press **Encrypt** on an asset, you are presented wtih two choices: **AES** or **DRM**.</span></span> 

#### <a name="aes"></a><span data-ttu-id="16ca1-169">AES</span><span class="sxs-lookup"><span data-stu-id="16ca1-169">AES</span></span>
<span data-ttu-id="16ca1-170">AES Rensa nyckelkryptering aktiveras på alla strömningsprotokoll: Smooth Streaming, HLS och MPEG-DASH.</span><span class="sxs-lookup"><span data-stu-id="16ca1-170">AES clear key encryption will be enabled on all streaming protocols: Smooth Streaming, HLS, and MPEG-DASH.</span></span>

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a><span data-ttu-id="16ca1-172">DRM</span><span class="sxs-lookup"><span data-stu-id="16ca1-172">DRM</span></span>
<span data-ttu-id="16ca1-173">När du väljer hello DRM-fliken visas med olika alternativ för principer för innehållsskydd (som måste du ha konfigurerat nu) + en uppsättning strömningsprotokoll.</span><span class="sxs-lookup"><span data-stu-id="16ca1-173">When you select hello DRM tab, you are presented with different choices of content protection policies (which you must have configured by now) + a set of streaming protocols.</span></span>

* <span data-ttu-id="16ca1-174">**PlayReady och Widevine med MPEG-DASH** -krypterar dynamiskt med PlayReady och Widevine DRMs MPEG-DASH strömmen.</span><span class="sxs-lookup"><span data-stu-id="16ca1-174">**PlayReady and Widevine with MPEG-DASH** - will dynamically encrypt your MPEG-DASH stream with PlayReady and Widevine DRMs.</span></span>
* <span data-ttu-id="16ca1-175">**PlayReady och Widevine med MPEG-DASH + FairPlay med HLS** -dynamiskt krypteras du MPEG-DASH-dataström med PlayReady och Widevine DRMs.</span><span class="sxs-lookup"><span data-stu-id="16ca1-175">**PlayReady and Widevine with MPEG-DASH + FairPlay with HLS** - will dynamically encrypt you MPEG-DASH stream with PlayReady and Widevine DRMs.</span></span> <span data-ttu-id="16ca1-176">Kommer också att kryptera din HLS dataströmmar med FairPlay.</span><span class="sxs-lookup"><span data-stu-id="16ca1-176">Will also encrypt your HLS streams with FairPlay.</span></span>
* <span data-ttu-id="16ca1-177">**PlayReady endast med Smooth Streaming, HLS och MPEG-DASH** -dynamiskt krypteras Smooth Streaming, HLS, MPEG-DASH-dataströmmar med PlayReady DRM.</span><span class="sxs-lookup"><span data-stu-id="16ca1-177">**PlayReady only with Smooth Streaming, HLS and MPEG-DASH** - will dynamically encrypt Smooth Streaming, HLS, MPEG-DASH streams with PlayReady DRM.</span></span>
* <span data-ttu-id="16ca1-178">**Widevine endast med MPEG-DASH** -dynamiskt kryptera du MPEG-DASH med Widevine DRM.</span><span class="sxs-lookup"><span data-stu-id="16ca1-178">**Widevine only with MPEG-DASH** - will dynamically encrypt you MPEG-DASH with Widevine DRM.</span></span>
* <span data-ttu-id="16ca1-179">**FairPlay endast med HLS** -krypterar dynamiskt HLS strömmen med FairPlay.</span><span class="sxs-lookup"><span data-stu-id="16ca1-179">**FairPlay only with HLS** - will dynamically encrypt your HLS stream with FairPlay.</span></span>

<span data-ttu-id="16ca1-180">tooenable FairPlay kryptering, behöver du tooprovide hello App certifikat och programmet hemlig nyckel (be) via hello FairPlay konfigurationsalternativet för hello innehållsskydd inställningsbladet.</span><span class="sxs-lookup"><span data-stu-id="16ca1-180">tooenable FairPlay encryption, you need tooprovide hello App Certificate and Application Secret Key (ASK) through hello FairPlay Configuration option of hello Content Protection settings blade.</span></span>

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection009.png)

<span data-ttu-id="16ca1-182">När du gör hello krypteringsalternativet trycker **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="16ca1-182">Once you make hello encryption selection, press **Apply**.</span></span>

>[!NOTE] 
><span data-ttu-id="16ca1-183">Om du planerar tooplay en AES krypterade HLS i Safari, se [bloggen](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span><span class="sxs-lookup"><span data-stu-id="16ca1-183">If you are planning tooplay an AES encrypted HLS in Safari, see [this blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="16ca1-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16ca1-184">Next steps</span></span>
<span data-ttu-id="16ca1-185">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="16ca1-185">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="16ca1-186">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="16ca1-186">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

