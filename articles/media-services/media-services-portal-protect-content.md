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
# <a name="configuring-content-protection-policies-using-hello-azure-portal"></a>Konfigurera principer för innehållsskydd med hello Azure-portalen
> [!NOTE]
> toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).
> 
> 

## <a name="overview"></a>Översikt
Microsoft Azure Media Services (AMS) kan du toosecure media från hello tid den lämnar din dator via lagring, bearbetning och leverans. Media Services kan du toodeliver innehållet krypteras dynamiskt med Standard AES (Advanced Encryption) (med 128-bitars krypteringsnycklar), vanliga kryptering (CENC) med PlayReady och/eller Widevine DRM och Apple FairPlay. 

AMS är en tjänst för att leverera DRM-licenser och AES avmarkera nycklar tooauthorized klienter. hello Azure-portalen kan du toocreate en **nyckel/licens auktoriseringsprincip** för alla typer av krypteringar.

Den här artikeln visar hur tooconfigure innehåll protection-principer med hello Azure-portalen. hello artikel också visar hur tooapply dynamisk kryptering tooyour tillgångar.


> [!NOTE]
> Om du har använt hello Azure klassiska portal toocreate skyddsprinciper hello principer kanske inte visas i hello [Azure-portalen](https://portal.azure.com/). Men alla hello gamla principerna fortfarande finns. Du kan kontrollera dem med hjälp av hello Azure Media Services .NET SDK eller hello [Azure Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) verktyget (toosee hello principer, högerklicka på hello tillgångar -> Visa information (F4) -> Klicka på fliken -> nycklar för Multiinnehåll Klicka på hello nyckel). 
> 
> Om du vill tooencrypt din tillgång med nya principer, konfigurera dem med hello Azure-portalen, klickar du på Spara och återanvända dynamisk kryptering. 
> 
> 

## <a name="start-configuring-content-protection"></a>Börja konfigurera innehållsskydd
toouse hello portal toostart konfigurera innehållsskydd, globala tooyour AMS-kontot, hello följande:

1. I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.
2. Välj **inställningar** > **innehåll skydd**.

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Auktoriseringsprincip för nyckel-licens
AMS stöder flera olika sätt att autentisera användare som begär eller licensinformation. Hej innehållsnyckelns auktoriseringsprincip måste konfigureras av dig och uppfyllas av klienten för hello nyckel/licens toobe delived toohello klienten. Hej innehållsnyckelns auktoriseringsprincip kan ha en eller flera auktoriseringsbegränsningar: **öppna** eller **token** begränsning.

hello Azure-portalen kan du toocreate en **nyckel/licens auktoriseringsprincip** för alla typer av krypteringar.

### <a name="open"></a>Öppet
Öppna begränsning innebär att hello system levererar hello viktiga tooanyone som begär nycklar. Den här begränsningen kan vara användbart för testning. 

### <a name="token"></a>Token
Hej tokenbegränsade principen måste åtföljas av en token som utfärdas av en säker säkerhetstokentjänst (STS). Media Services stöder token i hello Simple Web Tokens (SWT) format och JSON-Webbtoken (JWT)-format. Media Services tillhandahåller inte Secure Token tjänster. Du kan skapa en anpassad STS eller använda Microsoft Azure ACS tooissue token. hello STS måste vara konfigurerade toocreate en token som signerats med hello angetts nyckel och utfärda anspråk som du angav i hello tokenbegränsningar konfiguration. hello Media Services viktiga leveranser returnerar hello begärt (eller licensinformation) toohello klienten om hello token är giltig och hello anspråk i hello token matchar de som konfigurerats för hello (eller licensinformation).

Du måste ange hello primära Verifieringsnyckeln, utfärdaren och målgrupp parametrar när du konfigurerar hello tokenbegränsade principen. hello primära Verifieringsnyckeln innehåller hello nyckel att hello token som signerats med är utfärdaren hello säker tokentjänsten som utfärdar hello-token. hello målgruppen (kallas ibland för scope) beskrivs hello syftet med hello-token eller hello resurs hello token auktoriserar åtkomst till. hello Media Services viktiga delivery service verifierar att dessa värden i hello token matchar hello värden i hello mallen.

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>PlayReady-rättighetsprincipmall
Detaljerad information om hello PlayReady rättighetsprincipmall finns [Media Services PlayReady licens mall översikt](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Icke beständiga
Om du konfigurerar licens som icke-beständig lagras den bara i minnet medan hello player använder hello-licens.  

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Beständiga
Om du konfigurerar hello licens som beständiga sparas den i permanent lagringsutrymme på hello-klienten.

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Widevine rättighetsprincipmall
Detaljerad information om hello Widevine rättighetsprincipmall finns [Widevine-licens mall översikt](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Basic
När du väljer **grundläggande**, hello mallen kommer att skapas med alla standardvärden värden.

### <a name="advanced"></a>Advanced
Mer detaljerad information om avancerade alternativ för Widevine konfigurationer finns [detta](media-services-widevine-license-template-overview.md) avsnittet.

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay-konfiguration
tooenable FairPlay kryptering, behöver du tooprovide hello App certifikat och programmet hemlig nyckel (be) via hello FairPlay konfigurationsalternativet. Detaljerad information om FairPlay konfiguration och krav finns [detta](media-services-protect-hls-with-fairplay.md) artikel.

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-tooyour-asset"></a>Använda dynamisk kryptering tooyour tillgångsinformation
tootake nytta av dynamisk kryptering du behöver tooencode källfilen till en uppsättning MP4-filer med anpassningsbar bithastighet.

### <a name="select-an-asset-that-you-want-tooencrypt"></a>Välj en tillgång som du vill tooencrypt
toosee dina tillgångar väljer **inställningar** > **tillgångar**.

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Kryptera med AES eller DRM
När du trycker på **kryptera** på en tillgång, visas två alternativ: **AES** eller **DRM**. 

#### <a name="aes"></a>AES
AES Rensa nyckelkryptering aktiveras på alla strömningsprotokoll: Smooth Streaming, HLS och MPEG-DASH.

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
När du väljer hello DRM-fliken visas med olika alternativ för principer för innehållsskydd (som måste du ha konfigurerat nu) + en uppsättning strömningsprotokoll.

* **PlayReady och Widevine med MPEG-DASH** -krypterar dynamiskt med PlayReady och Widevine DRMs MPEG-DASH strömmen.
* **PlayReady och Widevine med MPEG-DASH + FairPlay med HLS** -dynamiskt krypteras du MPEG-DASH-dataström med PlayReady och Widevine DRMs. Kommer också att kryptera din HLS dataströmmar med FairPlay.
* **PlayReady endast med Smooth Streaming, HLS och MPEG-DASH** -dynamiskt krypteras Smooth Streaming, HLS, MPEG-DASH-dataströmmar med PlayReady DRM.
* **Widevine endast med MPEG-DASH** -dynamiskt kryptera du MPEG-DASH med Widevine DRM.
* **FairPlay endast med HLS** -krypterar dynamiskt HLS strömmen med FairPlay.

tooenable FairPlay kryptering, behöver du tooprovide hello App certifikat och programmet hemlig nyckel (be) via hello FairPlay konfigurationsalternativet för hello innehållsskydd inställningsbladet.

![Skydda innehåll](./media/media-services-portal-content-protection/media-services-content-protection009.png)

När du gör hello krypteringsalternativet trycker **tillämpa**.

>[!NOTE] 
>Om du planerar tooplay en AES krypterade HLS i Safari, se [bloggen](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

