---
title: "aaaUse Azure AD authentication tooaccess Azure Media Services-API med övriga | Microsoft Docs"
description: "Lär dig hur tooaccess Azure Media Services API med Azure Active Directory-autentisering med hjälp av REST."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: willzhan;juliako
ms.openlocfilehash: 114f7bdde3a8f5fe6189d712e05b6afdc8a8787a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-ad-authentication-tooaccess-hello-azure-media-services-api-with-rest"></a>Använd Azure AD authentication tooaccess hello Azure Media Services-API med övriga

hello Azure Media Services-teamet har publicerat Azure Active Directory (AD Azure) autentiseringsstöd för åtkomst till Azure Media Services. Det har också meddelat planer toodeprecate Azure Access Control service autentisering för åtkomst till Media Services. Eftersom varje Azure-prenumeration och alla Media Services-konto är anslutna tooan Azure AD-klient, ger stöd för Azure AD-autentisering många fördelar för säkerheten. Mer information om den här ändringen och migrering (om du använder hello Media Services .NET SDK för din app), finns följande hello bloggen anslår och artiklar:

- [Azure Media Services annonserar stöd för Azure AD och utfasningen av Access Control-autentisering](https://azure.microsoft.com/blog/azure%20media%20service%20aad%20auth%20and%20acs%20deprecation)
- [Åtkomst till Azure Media Services API med hjälp av Azure AD-autentisering](media-services-use-aad-auth-to-access-ams-api.md)
- [Använda Azure AD authentication tooaccess Azure Media Services API med hjälp av Microsoft .NET](media-services-dotnet-get-started-with-aad.md)
- [Komma igång med Azure AD authentication med hjälp av hello Azure-portalen](media-services-portal-get-started-with-aad.md)

Vissa kunder behöver toodevelop sina lösningar för Media Services under hello följande begränsningar:

*   De använder ett programmeringsspråk som inte är Microsoft .NET- eller C# eller hello körningsmiljö är inte Windows.
*   Azure AD-bibliotek, till exempel Active Directory-Autentiseringsbibliotek är inte tillgängliga för hello programmeringsspråket eller kan inte användas för sina körningsmiljön.

Vissa kunder har utvecklat program med hjälp av REST API för både autentisering för åtkomstkontroll och Azure Media Services. Du behöver ett sätt toouse endast hello REST API för Azure AD-autentisering för dessa kunder och efterföljande Azure Media Services åtkomst till. Du behöver toonot förlitar sig på någon av hello Azure AD-bibliotek eller hello Media Services .NET SDK. I den här artikeln vi beskriver en lösning och ger exempelkod för det här scenariot. Eftersom hello koden är alla REST API-anrop med inga beroende på Azure AD eller Azure Media Services-biblioteket, hello kod kan du enkelt att översatta tooany andra programmeringsspråk.

> [!IMPORTANT]
> Media Services stöder för närvarande hello Azure Access Control services autentisering-modell. Access Control-autentisering kommer dock föråldrad den 1 juni 2018. Vi rekommenderar att du migrerar toohello Azure AD-autentisering så snart som möjligt.


## <a name="design"></a>Designa

I den här artikeln använder vi hello efter autentisering och auktorisering design:

*  Authorization protocol: OAuth 2.0. OAuth 2.0 är en standard för web säkerhet som omfattar både autentisering och auktorisering. Det stöds av Google, Microsoft, Facebook och PayPal. Det har ratificerat oktober 2012. Microsoft stöder ordentligt OAuth 2.0 och OpenID Connect. Båda dessa normer stöds i flera tjänster och klientbibliotek, inklusive Azure Active Directory, öppna Webbgränssnitt för .NET (OWIN) Katana och Azure AD-bibliotek.
*  Bevilja: bevilja klientens autentiseringsuppgifter. Klientens autentiseringsuppgifter är en av hello fyra bevilja typer i OAuth 2.0. Det används ofta för Azure AD Microsoft Graph API-åtkomst.
*  Autentiseringsläge: tjänstens huvudnamn. hello andra autentiseringsläget är användar- eller interaktiv autentisering.

Summan av fyra program eller tjänster ingår i hello Azure AD-autentisering och auktorisering flödet för med Media Services. hello program och tjänster och hello flödet beskrivs i hello följande tabell:

|Programtyp |Program |Flöde|
|---|---|---|
|Client | Kunden app eller lösning | Den här appen (faktiskt, dess proxy) är registrerat i hello Azure AD-klient i vilka hello Azure-prenumeration och hello media service-kontot finns. hello beviljas tjänstens huvudnamn för hello registrerad app sedan med ägare eller deltagare roll i hello Access Control (IAM) för hello media-tjänstkontot. hello tjänstens huvudnamn representeras av hello app-ID och klienten klienthemlighet. |
|Identitetsprovider (IDP) | Azure AD som IDP | hello registrerade app tjänstens huvudnamn (klient-ID och klienthemlighet) autentiseras av Azure AD som hello IDP. Autentiseringen utförs internt och implicit. Som i klientautentiseringsuppgifter, är hello klienten autentiserad i stället för hello användare. |
|Skydda säkerhetstokentjänst (STS) / OAuth-server | Azure AD som STS | Efter autentisering av hello IDP (eller OAuth-Server i OAuth 2.0-villkor), en åtkomst-token eller en JSON-Webbtoken (JWT) är utfärdat av Azure AD som STS/OAuth-Server för åtkomst toohello mellannivå resurs: i det här fallet hello Media Services REST API-slutpunkt. |
|Resurs | Media Services REST API | Varje Media Services REST API-anrop är auktoriserad genom en åtkomst-token som utfärdas av Azure AD som STS eller hello OAuth-servern. |

Om du kör hello exempelkod och avbilda en JWT eller en åtkomst-token har hello JWT hello följande attribut:

    aud: "https://rest.media.azure.net",

    iss: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    iat: 1497146280,

    nbf: 1497146280,
    exp: 1497150180,

    aio: "Y2ZgYDjuy7SptPzO/muf+uRu1B+ZDQA=",

    appid: "02ed1e8e-af8b-477e-af3d-7e7219a99ac6",

    appidacr: "1",

    idp: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    oid: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    sub: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    tid: "72f988bf-86f1-41af-91ab-2d7cd011db47",

Här följer hello mappningar mellan hello attribut i hello JWT och hello fyra program eller tjänster i föregående tabell hello:

|Programtyp |Program |JWT-attribut |
|---|---|---|
|Client |Kunden app eller lösning |AppID: ”02ed1e8e-af8b-477e-af3d-7e7219a99ac6”. hello klient-ID för ett program som du registrerar tooAzure AD i hello nästa avsnitt. |
|Identitetsprovider (IDP) | Azure AD som IDP |IDP: ”https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/”.  hello GUID är hello-ID för Microsoft-klient (microsoft.onmicrosoft.com). Varje innehavare har sin egen, unikt ID. |
|Skydda säkerhetstokentjänst (STS) / OAuth-server |Azure AD som STS | ISS: ”https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/”. hello GUID är hello-ID för Microsoft-klient (microsoft.onmicrosoft.com). |
|Resurs | Media Services REST API |eller: ”https://rest.media.azure.net”. hello mottagaren eller målgruppen för hello åtkomst-token. |

## <a name="steps-for-setup"></a>Steg för installation

tooregister och skapa ett Azure AD-program för Azure AD-autentisering och tooobtain en åtkomst-token för att anropa hello Azure Media Services REST API-slutpunkt, fullständig hello följande steg:

1.  I hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), registrera en Azure AD-program (till exempel wzmediaservice) toohello Azure AD-klient (till exempel microsoft.onmicrosoft.com). Det spelar ingen roll om du har registrerat som webbapp eller inbyggda appen. Du kan också välja alla inloggnings-URL och reply-URL (till exempel http://wzmediaservice.com för både).
2. I hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), gå toohello **konfigurera** fliken i ditt program. Obs hello **klient-ID**. Sedan, under **nycklar**, generera en **klientnyckel** (klienthemlighet). 

    > [!NOTE] 
    > Anteckna hello klienthemlighet. Det kommer inte att visas igen.
    
3.  I hello [Azure-portalen](http://ms.portal.azure.com), gå toohello Media Services-konto. Välj hello **åtkomstkontroll** (IAM)-fönstret. Lägg till en ny medlem som har hello ägare eller hello deltagarrollen. Hello principal Sök efter hello programnamn som du har registrerat i steg 1 (i det här exemplet wzmediaservice).

## <a name="info-toocollect"></a>Info toocollect

tooprepare REST-kodning, samla in hello följande datapunkter tooinclude i hello kod:

*   Azure AD som en STS-slutpunkt: https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/token. En JWT-åtkomsttoken har begärts från den här slutpunkten. Dessutom tooserving som en IDP Azure AD fungerar också som en Säkerhetstokentjänst. Azure AD utfärdar en JWT för åtkomst till företagsresurser (en åtkomst-token). En JWT-token har olika krav.
*   Azure Media Services REST API som resurs eller en publik: https://rest.media.azure.net.
*   Klient-ID: Se steg 2 i [steg för att installationen](#steps-for-setup).
*   Klienthemligheten: se steg 2 i [steg för att installationen](#steps-for-setup).
*   Media Services konto REST API-slutpunkt i hello följande format:

    https://[media_service_account_name].restv2. [data_center].media.azure.net/API 

    Detta är hello slutpunkt mot vilken alla Media Services REST API-anrop i ditt program görs. Till exempel https://willzhanmswjapan.restv2.japanwest.media.azure.net/API.

Du kan sedan lägga parametrarna fem i filen web.config eller app.config toouse i koden.

## <a name="sample-code"></a>Exempelkod

Du kan hitta hello exempelkoden i [Azure AD-autentisering för Azure Media Services-åtkomst: båda via REST API](https://github.com/willzhan/WAMSRESTSoln).

hello exempelkod har två delar:

*   En DLL-biblioteksprojekt som har alla hello REST API-koden för Azure AD-autentisering och auktorisering. Det har också en metod för att göra REST API-anrop toohello Media Services REST API-slutpunkt, med hello åtkomsttoken.
*   En konsol test-klient som initierar Azure AD-autentisering och annan Media Services REST API-anrop.

hello exempelprojektet har tre funktioner:

*   Azure AD-autentiseringar via hello klientens autentiseringsuppgifter ger med hjälp av endast hello REST API.
*   Azure Media Services-åtkomst med hjälp av endast hello REST API.
*   Endast hello Azure Storage-åtkomst med hjälp av REST-API (som används toocreate Media Services-konto med hjälp av REST-API).


## <a name="where-is-hello-refresh-token"></a>Var finns hello uppdateringstoken?

Vissa webbläsare kan be: där är hello uppdateringstoken? Varför inte använda en uppdateringstoken här?

hello syftet med en uppdateringstoken är inte toorefresh en åtkomst-token. I stället är utformad toobypass autentisering eller användaren någon åtgärd från slutanvändaren och fortfarande få en giltig åtkomsttoken när ett tidigare token upphör att gälla. Ett bättre namn för en uppdateringstoken kan vara ungefär ”kringgå återexport-sign in användartoken”.

Om du använder hello OAuth 2.0 auktorisering bevilja flöde (användarnamn och lösenord, fungerar för en användares räkning), kan du hämta ett förnyat åtkomsttoken utan åtgärder från användaren begär en uppdateringstoken. Dock fungerar hello OAuth 2.0 klientens autentiseringsuppgifter bevilja flödet som beskrivs i den här artikeln, hello klienten för sin egen räkning. Du behöver inte användaren behöver göra något alls och hello auktorisering server behöver inte för (och inte) ger dig en uppdateringstoken. Om du felsöka hello **GetUrlEncodedJWT** metod du märker att hello svar från hello-token för slutpunkt har en åtkomst-token, men ingen uppdateringstoken.

## <a name="next-steps"></a>Nästa steg

Kom igång med [Överför filer tooyour konto](media-services-dotnet-upload-files.md).
