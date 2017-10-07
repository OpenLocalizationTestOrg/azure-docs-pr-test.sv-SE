---
title: aaaHybrid utformning av DRM subsystem(s) med Azure Media Services | Microsoft Docs
description: "Det här avsnittet beskrivs hybridutformning av DRM subsystem(s) med Azure Media Services."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: willzhan;juliako
ms.openlocfilehash: 4206248420ccd4dbfc9a87a86f4763534c6254a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-design-of-drm-subsystems"></a>Hybridutformning av DRM subsystem(s)

Det här avsnittet beskrivs hybridutformning av DRM subsystem(s) med Azure Media Services.

## <a name="overview"></a>Översikt

Azure Media Services tillhandahåller support för hello följande tre DRM-systemet:

* PlayReady
* Widevine (modulära)
* FairPlay

hello DRM stödet ingår DRM-kryptering (dynamisk kryptering) och licensleverans med Azure Media Player stöder alla 3 DRMs som en webbläsare spelare SDK.

En detaljerad behandling av DRM/CENC undersystemet design och implementering finns i dokumentet hello [CENC med Multi-DRM och Access Control](media-services-cenc-with-multidrm-access-control.md).

Även om vi har fullt stöd för tre DRM-system, måste ibland kunder toouse olika delar av sin egen infrastruktur/delsystem i tillägg tooAzure Media Services toobuild hybrid DRM-undersystemet.

Nedan är några vanliga frågor och svar av kunder:

* ”Kan jag använda min egen DRM licensservrar”? (I det här fallet kunder har investerat i DRM-licens serverkluster med inbäddade affärslogik).
* ”Kan jag använda bara din leverans för DRM-licens i Azure Media Services utan värd för innehållet i AMS”?

## <a name="modularity-of-hello-ams-drm-platform"></a>Modularitet av hello DRM AMS-plattformen

Som en del av en omfattande video molnplattform har Azure Media Services DRM en design med flexibiliteten och modulariteten i åtanke. Du kan använda Azure Media Services med någon av hello efter olika kombinationer som beskrivs i hello nedan (följer en förklaring av hello notation som används i hello tabell). 

|**Värd för innehåll & ursprung**|**Innehåll kryptering**|**DRM-licensleverans**|
|---|---|---|
|AMS|AMS|AMS|
|AMS|AMS|Från tredje part|
|AMS|Från tredje part|AMS|
|AMS|Från tredje part|Från tredje part|
|Från tredje part|Från tredje part|AMS|

### <a name="content-hosting--origin"></a>Värd för innehåll & ursprung

* AMS: videotillgång finns i AMS och strömning är via AMS strömningsslutpunkter (men inte nödvändigtvis dynamisk paketering).
* Från tredje part: video är värdbaserad och levereras på en tredje parts strömmande plattform utanför AMS.

### <a name="content-encryption"></a>Innehåll kryptering

* AMS: kryptering av innehåll är utförs dynamiskt/på begäran av AMS dynamisk kryptering.
* Från tredje part: innehåll kryptering utförs utanför AMS med före bearbetning arbetsflödet.

### <a name="drm-license-delivery"></a>DRM-licensleverans

* AMS: DRM-licens levereras av AMS-Licenstjänsten leverans.
* Från tredje part: DRM-licens levereras av en tredje parts DRM licensserver utanför AMS.

## <a name="configure-based-on-your-hybrid-scenario"></a>Konfigurera baserat på din hybridscenario

### <a name="content-key"></a>Innehållsnyckel

Du kan styra följande attribut för både AMS dynamisk kryptering och AMS-Licenstjänsten leverans hello genom konfigurationen av en innehållsnyckel:

* hello innehållsnyckeln används för dynamisk DRM-kryptering.
* DRM-licens innehåll toobe levereras av licensleveranstjänster: rättigheter, innehållsnyckeln och begränsningar.
* Typ av **innehåll begränsning för auktorisering av innehållsnyckel princip**: Öppna IP- eller tokenbegränsningar.
* Om **token** typ av **begränsning för auktorisering av innehållsnyckel principen används**, hello **innehåll begränsning för auktorisering av innehållsnyckel princip** måste vara uppfyllda innan en licens utfärdas.

### <a name="asset-delivery-policy"></a>Principen för tillgångsleverans

Du kan styra hello följande attribut som används av AMS dynamiska Paketeraren och dynamisk kryptering av ett AMS strömmande slutpunkten genom konfigurationen av en tillgångsleveransprincip:

* Direktuppspelning DRM kryptering kombinationen och protokoll, till exempel STRECK under CENC (PlayReady och Widevine), jämna strömning under PlayReady HLS under Widevine eller PlayReady.
* hello standard/inbäddade licens leverans URL: er för varje hello berörda DRMs.
* Licens om förvärv URL: er (LA_URLs) i DASH MPD eller HLS spelningslista innehåller frågesträngen nyckel (KID) för Widevine och FairPlay, respektive.

## <a name="scenarios-and-samples"></a>Scenarier och exempel

Baserat på hello förklaringar hello föregående avsnitt, hello följande fem hybridscenarion använda respektive **innehållsnyckeln**-**tillgångsleveransprincip** configuration kombinationer (hello exempel som nämns i hello sista kolumnen så hello tabell):

|**Värd för innehåll & ursprung**|**DRM-kryptering**|**DRM-licensleverans**|**Konfigurera innehållsnyckeln**|**Konfigurera tillgångsleveransprincip**|**Exempel**|
|---|---|---|---|---|---|
|AMS|AMS|AMS|Ja|Ja|Exempel 1|
|AMS|AMS|Från tredje part|Ja|Ja|Exempel 2|
|AMS|Från tredje part|AMS|Ja|Nej|Exempel 3|
|AMS|Från tredje part|Utanför|Nej|Nej|Exempel 4|
|Från tredje part|Från tredje part|AMS|Ja|Nej|    

I hello prover PlayReady skydd fungerar för både DASH och smooth streaming. hello video adresserna nedan är smooth streaming URL: er. tooget Hej motsvarande DASH URL: er, bara lägga till ”(format = mpd-tid-csf)”. Du kan använda hello [azure media testa player](http://aka.ms/amtest) tootest i en webbläsare. Det gör att du tooconfigure som strömning protokollet toouse under vilka teknisk. Stöder PlayReady via EME IE11 och MS-Edge på Windows 10. Mer information finns i [information om hello testa verktyget](https://blogs.msdn.microsoft.com/playready4/2016/02/28/azure-media-test-tool/).

### <a name="sample-1"></a>Exempel 1

* Datakälla (bas)-URL: https://willzhanmswest.streaming.mediaservices.windows.net/1efbd6bb-1e66-4e53-88c3-f7e5657a9bbd/RussianWaltz.ism/manifest 
* PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/ 
* Widevine LA_URL (bindestreck): https://willzhanmswest.keydelivery.mediaservices.windows.net/Widevine/?kid=78de73ae-6d0f-470a-8f13-5c91f7c4 
* FairPlay LA_URL (HLS): https://willzhanmswest.keydelivery.mediaservices.windows.net/FairPlay/?kid=ba7e8fb0-ee22-4291-9654-6222ac611bd8 

### <a name="sample-2"></a>Exempel 2

* Datakälla (bas)-URL: http://willzhanmswest.streaming.mediaservices.windows.net/1a670626-4515-49ee-9e7f-cd50853e41d8/Microsoft_HoloLens_TransformYourWorld_816p23.ism/Manifest 
* PlayReady LA_URL (DASH & smooth): http://willzhan12.cloudapp.net/PlayReady/RightsManager.asmx 

### <a name="sample-3"></a>Exempel 3

* Käll-URL: https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500.ism/manifest 
* PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/ 

### <a name="sample-4"></a>Exempel 4

* Käll-URL: https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500.ism/manifest 
* PlayReady LA_URL (DASH & smooth): https://willzhan12.cloudapp.net/playready/rightsmanager.asmx 

## <a name="summary"></a>Sammanfattning

Sammanfattningsvis Azure Media Services DRM-komponenterna är flexibla, du kan använda dem i ett hybridscenario genom att konfigurera innehållsnyckeln och tillgångsleveransprincip, enligt beskrivningen i det här avsnittet.

## <a name="next-steps"></a>Nästa steg
Visa Media Services utbildningsvägar.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

