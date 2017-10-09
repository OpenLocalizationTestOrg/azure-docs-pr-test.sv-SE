---
title: "aaaConfigure Innehållsnyckelns auktoriseringsprincip med hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur tooconfigure en auktoriseringsprincip för en innehållsnyckel."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 157fb691b7f71f4889228817e1dc64555e327d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-content-key-authorization-policy"></a>Konfigurera Innehållsnyckelns auktoriseringsprincip
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Översikt
Microsoft Azure Media Services kan du toodeliver MPEG DASH, Smooth Streaming och HTTP Live Streaming (HLS)-dataströmmar som skyddas med Standard AES (Advanced Encryption) (med 128-bitars krypteringsnycklar) eller [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS gör också att du toodeliver DASH-dataströmmar krypterat med Widevine DRM. Både PlayReady och Widevine krypteras enligt hello Common Encryption (ISO/IEC 23001 7 CENC)-specifikationen.

Media Services tillhandahåller också en **nyckel-/ Licensleveranstjänst** från vilka klienterna kan hämta AES-nycklar eller PlayReady/Widevine-licenser tooplay hello krypterat innehåll.

Det här avsnittet visar hur toouse hello Azure portal tooconfigure hello innehållsnyckelns auktoriseringsprincip. hello nyckeln kan användas senare toodynamically kryptera ditt innehåll. Observera att för närvarande kan du kryptera hello följande strömningsformat: HLS MPEG DASH och Smooth Streaming. Det går inte att kryptera progressiv hämtning.

När en spelare begär en ström som har angetts toobe dynamiskt krypterad, kryptera Media Services använder hello konfigurerats viktiga toodynamically ditt innehåll med hjälp av AES eller DRM-kryptering. toodecrypt hello dataströmmen hello player begär hello nyckeln från hello viktiga leverans av tjänsten. toodecide om huruvida användaren hello är godkänd tooget hello nyckel, hello tjänsten utvärderar hello auktoriseringsprinciper som du angav för hello nyckeln.

Om du planerar toohave nycklar för multiinnehåll eller toospecify om en **nyckel-/ Licensleveranstjänst** URL än hello Media Services viktiga delivery service använda Media Services .NET SDK eller REST API: er.

[Konfigurera Innehållsnyckelns auktoriseringsprincip med Media Services .NET SDK](media-services-dotnet-configure-content-key-auth-policy.md)

[Konfigurera Innehållsnyckelns auktoriseringsprincip med Media Services REST API](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Vissa förutsättningar gäller:
* När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering, strömmande slutpunkten har toobe i hello **kör** tillstånd. 
* Din tillgång måste innehålla en uppsättning MP4s med anpassningsbar bithastighet eller Smooth Streaming-filer. Mer information finns i [koda en tillgång](media-services-encode-asset.md).
* hello nyckeln Delivery service cachelagrar ContentKeyAuthorizationPolicy och dess relaterade objekt (alternativ och begränsningar) i 15 minuter.  Om du skapar en ContentKeyAuthorizationPolicy och ange toouse ”Token” begränsning, och sedan testa den och sedan uppdatera hello-principen för ”öppna” begränsning, det tar ungefär 15 minuter innan hello princip växlar toohello ”öppen” version av hello-principen.

## <a name="how-to-configure-hello-key-authorization-policy"></a>Så här: Konfigurera hello innehållsnyckelns auktoriseringsprincip
tooconfigure hello innehållsnyckelns auktoriseringsprincip, Välj hello **INNEHÅLLSSKYDD** sidan.

Media Services stöder flera olika sätt att auktorisera användare som begär nycklar. Hej innehållsnyckelns auktoriseringsprincip kan ha **öppna**, **token**, eller **IP** auktoriseringsbegränsningar (**IP** kan konfigureras med REST- eller .NET SDK).

### <a name="open-restriction"></a>Öppna begränsning
Hej **öppna** begränsning innebär hello system levererar hello viktiga tooanyone som begär nycklar. Den här begränsningen kan vara användbart för testning.

![OpenPolicy][open_policy]

### <a name="token-restriction"></a>Tokenbegränsningar
toochoose hello token begränsad princip, tryck på hello **TOKEN** knappen.

Hej **token** tokenbegränsade principen måste åtföljas av en token som utfärdas av en **Secure Token Service** (STS). Media Services stöder token i hello **Simple Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) format och **JSON Web Token** (JWT)-format. Mer information finns i [JWT tokenautentisering](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services tillhandahåller inte **Secure Token Services**. Du kan skapa en anpassad STS eller använda Microsoft Azure ACS tooissue token. hello STS måste vara konfigurerade toocreate en token som signerats med hello angetts nyckel och utfärda anspråk som du angav i hello tokenbegränsningar konfiguration. hello returnerar Media Services-tjänsten för leverans av nyckeln hello encryption key toohello klienten om hello token är giltig och hello anspråk i hello token som matchar de som konfigurerats för hello innehållsnyckeln. Mer information finns i [Använd Azure ACS tooissue token](http://mingfeiy.com/acs-with-key-services).

När du konfigurerar hello **TOKEN** tokenbegränsade principen måste du ange värden för **Verifieringsnyckeln**, **utfärdaren** och **målgruppen**. hello primära Verifieringsnyckeln innehåller hello nyckel att hello token som signerats med är utfärdaren hello säker tokentjänsten som utfärdar hello-token. hello målgruppen (kallas ibland för scope) beskrivs hello syftet med hello-token eller hello resurs hello token auktoriserar åtkomst till. hello Media Services viktiga delivery service verifierar att dessa värden i hello token matchar hello värden i hello mallen.

### <a name="playready"></a>PlayReady
När du skyddar ditt innehåll med **PlayReady**, med en hello saker du måste toospecify i principen för auktorisering är en XML-sträng som definierar hello PlayReady license-mall. Som standard anges hello följande princip:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1"><LicenseTemplates> <PlayReadyLicenseTemplate> <AllowTestDevices>SANT</AllowTestDevices> <ContentKey i:type="ContentEncryptionKeyFromHeader" /> <LicenseType>Nonpersistent</LicenseType> <PlayRight> <AllowPassingVideoContentToUnknownOutput>tillåtna</AllowPassingVideoContentToUnknownOutput> </PlayRight> </PlayReadyLicenseTemplate> </LicenseTemplates></PlayReadyLicenseResponseTemplate>

Du kan klicka på hello **Importera princip-xml** knappen och ange en annan XML som uppfyller toohello XML-Schema som definierats [här](media-services-playready-license-template-overview.md).

## <a name="next-step"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

