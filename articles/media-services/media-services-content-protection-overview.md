---
title: "aaaProtect ditt innehåll med Azure Media Services | Microsoft Docs"
description: "Den här artikeln ger en översikt över innehållsskydd med Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: abab7602d71d7357a692976420ca9a988c0d096f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-content-overview"></a>Skydda innehåll-översikt
Microsoft Azure Media Services kan du toosecure media från hello tid den lämnar din dator via lagring, bearbetning och leverans. Media Services kan du toodeliver innehållet live och på begäran dynamiskt krypterad med Standard AES (Advanced Encryption) (med 128-bitars krypteringsnycklar) eller någon av hello större DRMs: Microsoft PlayReady och Google Widevine Apple FairPlay. Media Services tillhandahåller också en tjänst för att leverera AES-nycklar och DRM (PlayReady Widevine och FairPlay) licensierar tooauthorized klienter. 

hello följande bild visar hello innehållsskydd arbetsflöden som har stöd för AMS. 

![Skydda med PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd. 

Det här avsnittet beskriver [begrepp och termer](media-services-content-protection-overview.md) relevanta toounderstanding innehållsskydd med AMS. hello avsnittet innehåller också [länkar](media-services-content-protection-overview.md#common-scenarios) tootopics som visar hur tooachieve innehåll skyddsuppgifter. 

## <a name="dynamic-encryption"></a>Dynamisk kryptering
Microsoft Azure Media Services kan toodeliver ditt innehåll dynamiskt krypterad med AES Rensa nyckel eller DRM-kryptering: Microsoft PlayReady och Google Widevine Apple FairPlay.

För närvarande kan du kryptera hello följande strömningsformat: HLS MPEG DASH och Smooth Streaming. Det går inte att kryptera progressiv hämtning.

Om du vill använda för Media Services tooencrypt en tillgång, behöver tooassociate en krypteringsnyckel (CommonEncryption eller EnvelopeEncryption) med din tillgång och även konfigurera auktoriseringsprinciper för hello nyckeln.

Du måste också tooconfigure hello tillgångs leveransprincip. Om du vill toostream en krypterad tillgång för lagring ser till att toospecify du hur toodeliver den genom att konfigurera principen för tillgångsleverans.

När en dataströmmen har begärts av en spelare, använder Media Services hello angetts viktiga toodynamically kryptera ditt innehåll med hjälp av AES Rensa nyckel eller DRM-kryptering. toodecrypt hello dataströmmen hello player begär hello nyckeln från hello viktiga leverans av tjänsten. toodecide om huruvida användaren hello är godkänd tooget hello nyckel, hello tjänsten utvärderar hello auktoriseringsprinciper som du angav för hello nyckeln.


## <a name="storage-encryption"></a>Lagringskryptering
Använd lagring kryptering tooencrypt innehållet lokalt med hjälp av AES 256-bitarskryptering och sedan ladda upp den tooAzure lagring där den lagras krypterat i vila. Tillgångar som skyddas med lagringskryptering okrypterad och placeras i en krypterad fil system tidigare tooencoding och eventuellt omkrypterade tidigare toouploading tillbaka som en ny utdatatillgång automatiskt. hello primära användningsfall för lagringskryptering är om du vill toosecure dina indata mediefiler hög kvalitet med stark kryptering i vila på disk.

I ordning toodeliver en krypterad tillgång för lagring, måste du konfigurera hello tillgångsleveransprincip så Media Services vet du hur toodeliver ditt innehåll. Innan din tillgång kan strömmas angetts hello streaming server tar bort hello lagringskryptering och dataströmmar innehåll med hello principen för tillgångsleverans (till exempel AES, vanliga kryptering eller Ingen kryptering).

## <a name="common-encryption-cenc"></a>Vanliga kryptering (CENC)
Vanliga kryptering används när du krypterar ditt innehåll med PlayReady eller / och Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Med cbcs aapl kryptering
Cbcs aapl används för att kryptera innehållet med FairPlay.

## <a name="envelope-encryption"></a>Kuvert kryptering
Använd det här alternativet om du vill tooprotect ditt innehåll med AES-128 klartextnyckel. Om du vill att ett säkrare alternativ kan du välja en av hello DRMs som anges i det här avsnittet. 

## <a name="licenses-and-keys-delivery-service"></a>Licenser och nycklar leverans av tjänsten
Media Services tillhandahåller en tjänst för att leverera DRM (PlayReady, Widevine, FairPlay) licenser och AES avmarkera nycklar tooauthorized klienter. Du kan använda [hello Azure-portalen](media-services-portal-protect-content.md), REST-API eller Media Services SDK för .NET tooconfigure autentiseringen och auktoriseringen av principer för dina licenser och nycklar.

## <a name="token-restriction"></a>Tokenbegränsningar
Hej innehållsnyckelns auktoriseringsprincip kan ha en eller flera auktoriseringsbegränsningar: öppen eller tokenbegränsning. Hej tokenbegränsade principen måste åtföljas av en token som utfärdas av en säker säkerhetstokentjänst (STS). Media Services stöder token i hello Simple Web Tokens (SWT) format och JSON-Webbtoken (JWT)-format. Media Services tillhandahåller inte Secure Token tjänster. Du kan skapa en anpassad STS eller använda Microsoft Azure ACS tooissue token. hello STS måste vara konfigurerade toocreate en token som signerats med hello angetts nyckel och utfärda anspråk som du angav i hello tokenbegränsningar konfiguration. hello Media Services viktiga leveranser returnerar hello begärt (eller licensinformation) toohello klienten om hello token är giltig och hello anspråk i hello token matchar de som konfigurerats för hello (eller licensinformation).

Du måste ange hello primära Verifieringsnyckeln, utfärdaren och målgrupp parametrar när du konfigurerar hello tokenbegränsade principen. hello primära Verifieringsnyckeln innehåller hello nyckel att hello token som signerats med är utfärdaren hello säker tokentjänsten som utfärdar hello-token. hello målgruppen (kallas ibland för scope) beskrivs hello syftet med hello-token eller hello resurs hello token auktoriserar åtkomst till. hello Media Services viktiga delivery service verifierar att dessa värden i hello token matchar hello värden i hello mallen.

## <a name="streaming-urls"></a>Strömmande URL: er
Om din tillgång har krypterats med flera DRM, bör du använda en tagg för kryptering i hello strömnings-URL: (format = 'm3u8 aapl', kryptering = ”xxx”).

det gäller hello följande överväganden:

* Endast noll eller en typ av enhetskryptering kan anges.
* Krypteringstyp saknar toobe som anges i hello url om bara en kryptering har tillämpade toohello tillgången.
* Krypteringstyp är skiftlägeskänsligt.
* hello följande krypteringstyper kan anges:  
  * **cenc**: Common encryption (Playready eller Widevine)
  * **cbcs aapl**: Fairplay
  * **CBC**: AES envelope kryptering.

## <a name="common-scenarios"></a>Vanliga scenarier
hello följande avsnitt visar hur tooprotect innehållet i lagring, leverera dynamiskt krypterade strömmande media, använda AMS-nyckel/licensleveranstjänst

* [Skydda med AES](media-services-protect-with-aes128.md) 
* [Skydda med PlayReady och/eller Widevine](media-services-protect-with-drm.md)
* [Strömma ditt innehåll skyddade HLS med Apple FairPlay/PlayReady eller](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Fler scenarier
* [Hur toointegrate Azure PlayReady licens tjänst med din egen krypterare/streaming server](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
* [Med hjälp av castLabs toodeliver DRM-licenser tooAzure Media Services](media-services-castlabs-integration.md)

>[!NOTE]
>Ett scenario där du använder ett externt DRM server(technology) och ström från AMS stöds inte för närvarande.


## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Relaterade länkar
[Om PlayReady som en tjänst och AES dynamisk kryptering med Azure Media Services](http://mingfeiy.com/playready)

[Azure Media Services PlayReady licens leverans priser förklaras](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Hur toodebug för AES krypterad dataström i Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT-token authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integrera Azure Media Services OWIN MVC baserat app med Azure Active Directory och begränsa viktiga innehållsleverans baserat på JWT anspråk](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Använd Azure ACS tooissue token](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
