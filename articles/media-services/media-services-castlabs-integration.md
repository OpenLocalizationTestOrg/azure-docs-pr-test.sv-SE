---
title: aaaUsing castLabs toodeliver Widevine-licenser tooAzure Media Services | Microsoft Docs
description: "Den här artikeln beskriver hur du kan använda Azure Media Services (AMS) toodeliver en dataström som krypteras dynamiskt med AMS med både PlayReady och Widevine DRMs. hello PlayReady-licens kommer från licensserver för Media Services PlayReady och Widevine-licens levereras med castLabs licensservern."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 2a9a408a-a995-49e1-8d8f-ac5b51e17d40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: Mingfeiy;willzhan;Juliako
ms.openlocfilehash: 80d2778fb283a96361e7e511990a36c2f551a310
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-castlabs-toodeliver-widevine-licenses-tooazure-media-services"></a>Med hjälp av castLabs toodeliver Widevine-licenser tooAzure Media Services
> [!div class="op_single_selector"]
> * [Axinom](media-services-axinom-integration.md)
> * [castLabs](media-services-castlabs-integration.md)
> 
> 

## <a name="overview"></a>Översikt
Den här artikeln beskriver hur du kan använda Azure Media Services (AMS) toodeliver en dataström som krypteras dynamiskt med AMS med både PlayReady och Widevine DRMs. hello PlayReady-licens kommer från licensserver för Media Services PlayReady och Widevine-licens levereras av **castLabs** licensservern.

tooplayback strömning innehåll som skyddas av CENC (PlayReady eller Widevine), kan du använda [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Se [AMP dokumentet](http://amp.azure.net/libs/amp/latest/docs/) mer information.

hello följande diagram visar en övergripande arkitektur för Azure Media Services och castLabs integrering.

![Integration](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

## <a name="typical-system-set-up"></a>Vanliga systemet
* Medieinnehåll lagras i AMS.
* Nyckel-ID: N innehåll nycklar lagras i både castLabs och AMS.
* castLabs och AMS har inbyggda-tokenautentisering. hello följande avsnitt beskrivs autentiseringstoken. 
* När en klient begär toostream hello video, hello innehåll krypteras dynamiskt med **Common Encryption** (CENC) och dynamiskt paketerats av AMS tooSmooth Streaming och bindestreck. Vi kan också leverera PlayReady M2TS elementära dataströmmen kryptering för HLS strömningsprotokoll.
* PlayReady-licens har hämtats från AMS licensservern och Widevine-licens har hämtats från castLabs licensservern. 
* Media Player avgör vilka licens toofetch baserat på hello klienten plattformskapacitet automatiskt. 

## <a name="authentication-token-generation-for-getting-a-license"></a>Autentisering-token generering för att hämta en licens
Både castLabs och AMS stöder JWT (JSON Web Token) token formatet tooauthorize en licens. 

### <a name="jwt-token-in-ams"></a>JWT-token i AMS
hello i den följande tabellen beskrivs JWT-token i AMS. 

| Utfärdaren | Utfärdaren sträng från hello valt Secure säkerhetstokentjänst (STS) |
| --- | --- |
| Målgrupp |Använda målgruppen sträng från hello STS |
| Anspråk |En uppsättning anspråk |
| Inte före |Starta giltighet hello-token |
| Upphör att gälla |End giltighet hello-token |
| SigningCredentials |hello-nyckel som delas med PlayReady licensservern castLabs licensservern och STS, det kan vara antingen symmetriskt eller asymmetriskt nyckel. |

### <a name="jwt-token-in-castlabs"></a>JWT-token i castLabs
hello i den följande tabellen beskrivs JWT-token i castLabs. 

| Namn | Beskrivning |
| --- | --- |
| optData |En JSON-sträng som innehåller information om dig. |
| CRT |En JSON-sträng som innehåller information om hello tillgång, dess licens info och uppspelning rättigheter. |
| IAT |hello aktuella datetime i epok. |
| jti |En unik identifierare om denna token (varje token kan bara användas en gång i hello castLabs system). |

## <a name="sample-solution-set-up"></a>Ställ in exempellösning
Hej [exempel lösning](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) består av två projekt:

* En konsolapp som kan använda tooset DRM begränsningar på en redan infogade tillgång för både PlayReady och Widevine.
* Ett webbprogram som lämnar ut tokens som kan ses som en mycket förenklad version av en Säkerhetstokentjänst.

toouse hello konsolprogram:

1. Ändra hello app.config toosetup AMS autentiseringsuppgifter, castLabs autentiseringsuppgifter, STS-konfiguration och delad nyckel.
2. Ladda upp en tillgång till AMS.
3. Get hello UUID från hello överförs tillgången och ändra raden i filen Program.cs för hello 32:
   
      var objIAsset = _context. Assets.Where (x = > x.Id == ”nb:cid:UUID:dac53a5d-1 500-80bd-b864-f1e4b62594cf”). FirstOrDefault();
4. Använd en AssetId för namngivning av hello tillgången i hello castLabs system (rad 44 i hello Program.cs-filen).
   
   Du måste ange AssetId för **castLabs**; måste toobe en unik alfanumerisk sträng.
5. Kör programmet hello.

toouse hello Web Application (STS):

1. Ändra hello web.config toosetup castlabs tillhör affärsenhet ID, hello STS konfiguration och hello delad nyckel.
2. Distribuera tooAzure webbplatser.
3. Navigera toohello webbplats.

## <a name="playing-back-a-video"></a>Spela upp en video
tooplayback en video som krypterats med vanliga kryptering (PlayReady eller Widevine), kan du använda hello [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). När du kör hello konsolprogram eko hello innehåll nyckel-ID och hello Manifest URL.

1. Öppna en ny flik och starta din STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2. Gå för[Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3. Klistra in i hello strömnings-URL.
4. Klicka på hello **avancerade alternativ** kryssrutan.
5. I hello **skydd** listrutan, Välj PlayReady och/eller Widevine.
6. Klistra in hello-token som du har fått från din STS i hello Token textruta. 
   
   Hej castLab licensservern inte behöver hello ”ägar =” prefix framför hello-token. Så ta bort som innan du skickar hello-token.
7. Uppdatera hello player.
8. hello video ska spelas.

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

