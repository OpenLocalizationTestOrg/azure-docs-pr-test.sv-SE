---
title: "Översikt över aaaMedia Services operations REST API | Microsoft Docs"
description: "Media Services REST API-översikt"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a5f1c5e7-ec52-4e26-9a44-d9ea699f68d9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: b54f4d9123486d6cae832c9817688b0f3da5b401
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="media-services-operations-rest-api-overview"></a>Media Services operations REST API-översikt
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Hej **Media Services Operations REST** API används för att skapa jobb, tillgångar, principer för åtkomst och andra åtgärder på objekt i ett Media Service-konto. Mer information finns i [Media Services Operations REST API-referensen](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

Microsoft Azure Media Services är en tjänst som accepterar OData-baserade HTTP-begäranden och kan svara i utförlig JSON eller atom + pub. Eftersom Media Services överensstämmer tooAzure designriktlinjer, finns det en uppsättning nödvändiga HTTP-huvuden som varje klient måste använda när du ansluter tooMedia tjänster, samt en uppsättning valfria huvuden som kan användas. hello följande avsnitt beskrivs hello sidhuvuden och HTTP-verb som du kan använda när du skapar förfrågningar och ta emot svar från Media Services.

Det här avsnittet ger en översikt över hur toouse REST v2 med Media Services.

## <a name="considerations"></a>Överväganden

hello följande överväganden gäller när du använder RESTEN.

* När du frågar entiteter finns en gräns på 1000 entiteter som returneras i taget eftersom offentlig REST-v2 begränsar frågans resultat too1000 resultat. Du behöver toouse **hoppa över** och **ta** (.NET) / **upp** (REST) enligt beskrivningen i [exemplet .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) och [REST-API exempel](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
* När med JSON och ange toouse hello **__metadata** nyckelord i hello-begäran (till exempel tooreferences ett länkat) måste du ange hello **acceptera** huvud för[utförlig JSON-format ](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) (se följande exempel hello). OData inte förstår hello **__metadata** egenskap i hello begäran om du inte anger den tooverbose.  
  
        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
  
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 

## <a name="standard-http-request-headers-supported-by-media-services"></a>Vanliga HTTP-begärans sidhuvud stöds av Media Services
För varje anrop som du gör i Media Services är en uppsättning krävs huvuden måste du inkludera i din förfrågan och en uppsättning valfria huvuden du kan också tooinclude. följande tabell visar hello hello krävs rubriker:

| Huvudet | Typ | Värde |
| --- | --- | --- |
| Auktorisering |Ägar |Ägar är hello accepteras endast mekanism för auktorisering. hello-värdet måste också innehålla hello åtkomsttoken som tillhandahålls av ACS. |
| x-ms-version |Decimal |2.11 |
| DataServiceVersion |Decimal |3.0 |
| MaxDataServiceVersion |Decimal |3.0 |

> [!NOTE]
> Eftersom Media Services använder OData tooexpose ska dess underliggande tillgångens metadata-databasen via REST API: er, hello DataServiceVersion och MaxDataServiceVersion rubriker ingå i en begäran. men om de inte kan förutsätter sedan för närvarande Media Services hello DataServiceVersion värde används 3.0.
> 
> 

hello följande är en uppsättning valfria huvuden:

| Huvudet | Typ | Värde |
| --- | --- | --- |
| Date |RFC 1123 datum |Tidsstämpeln för begäran om hello |
| Acceptera |Innehållstyp |hello begärda content-type för hello svar, till exempel hello följande:<p> -program/json; odata = verbose<p> -application/atom + xml<p> Svaren kan ha en annan innehållstyp, till exempel en blob-fetch där ett lyckat svar innehåller hello blob-dataström som hello nyttolast. |
| Acceptera-kodning |Gzip, deflate |GZIP och DEFLATE kodning, i tillämpliga fall. Obs: För stora resurser Media Services kan ignorera det här huvudet och returnera data komprimeras. |
| Acceptera språk |”SV”, ”a” och så vidare. |Anger hello önskad hello svar. |
| Acceptera teckenuppsättning |Teckenuppsättning typen like ”UTF-8” |Standardvärdet är UTF-8. |
| X-HTTP-Method |HTTP-metoden |Gör att klienter eller brandväggar som inte stöder HTTP-metoder som till exempel PLACERA eller ta bort toouse metoderna tunneldata via en GET-anrop. |
| Innehållstyp |Innehållstyp |Innehållstypen för hello begärandetexten i PUT- eller POST-begäranden. |
| Client-request-id |Sträng |En anropare definierat värde som identifierar hello som anges i begäran. Om det här värdet inkluderas i hello svarsmeddelande som en sätt toomap hello-begäran. <p><p>**Viktigt**<p>Värden ska vara begränsad till 2096b (2k). |

## <a name="standard-http-response-headers-supported-by-media-services"></a>HTTP-svarshuvuden stöds av Media Services
hello följande är en uppsättning rubriker som kan returneras tooyou beroende på hello resursen som du begärt och hello åtgärd som du tänkt tooperform.

| Huvudet | Typ | Värde |
| --- | --- | --- |
| id för begäran |Sträng |En unik identifierare för hello aktuella åtgärd, tjänst skapas. |
| Client-request-id |Sträng |En identifierare som anges av hello anroparen i hello ursprungliga begäran, om sådan finns. |
| Date |RFC 1123 datum |hello datum hello begäran bearbetades. |
| Innehållstyp |Det varierar |hello innehållstypen för hello svarstexten. |
| Innehållskodning |Det varierar |Gzip och deflate på lämpligt sätt. |

## <a name="standard-http-verbs-supported-by-media-services"></a>Vanliga HTTP-verb som stöds av Media Services
hello följande är en fullständig lista över HTTP-verb som kan användas när gör HTTP-begäranden:

| Verb | Beskrivning |
| --- | --- |
| HÄMTA |Returnerar hello aktuella värdet för ett objekt. |
| POST |Skapar ett objekt baserat på informationen hello eller skickar ett kommando. |
| PLACERA |Ersätter ett objekt eller skapar ett namngivet objekt (i förekommande fall). |
| TA BORT |Tar bort ett objekt. |
| SAMMANFOGA |Uppdaterar ett befintligt objekt med namngivna egenskapsändringar. |
| HUVUDET |Returnerar metadata för ett objekt för en GET-svar. |

## <a name="discover-media-services-model"></a>Identifiera Media Services-modell
Mer synliga toomake Media Services entiteter hello $metadata åtgärden kan användas. Det gör att du tooretrieve alla giltiga entitetstyper, Entitetsegenskaper, kopplingar, funktioner, åtgärder och så vidare. hello följande exempel visas hur tooconstruct hello URI: https://media.windows.net/API/$ metadata.

Du bör lägga till ”? api-version=2.x” toohello slutet av hello URI om du vill tooview hello metadata i en webbläsare eller inkluderar inte hello x-ms-version-huvud i begäran.

## <a name="connect-toomedia-services"></a>Ansluta tooMedia tjänster

Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI. Du måste göra följande anrop toohello ny URI.

## <a name="next-steps"></a>Nästa steg

tooaccess AMS APIs med övriga, se [använda Azure AD authentication tooaccess hello Azure Media Services-API med övriga](media-services-rest-connect-with-aad.md).

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

