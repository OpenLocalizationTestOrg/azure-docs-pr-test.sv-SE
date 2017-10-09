---
title: aaaAuthentication och auktorisering i Azure App Service | Microsoft Docs
description: "Konceptuell referens- och översikt över hello autentisering / auktorisering funktion för Azure App Service"
services: app-service
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/29/2016
ms.author: mahender
ms.openlocfilehash: dc2074b16cce47b72b78ea7afeda89dbc4832166
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Autentisering och auktorisering i Azure App Service
## <a name="what-is-app-service-authentication--authorization"></a>Vad är App Service autentisering / auktorisering?
App Service autentisering / auktorisering är en funktion som gör det möjligt för dina program toosign användare så att du inte har toochange kod på hello-appserverdelen. Det ger ett enkelt sätt tooprotect programmet och arbete data per användare.

Apptjänst använder federerad identitet, där en tredjeparts identitetsprovider lagrar konton och autentiserar användare. hello programmet beroende hello providern identitetsinformation så att hello app saknar toostore sådan information. Apptjänst fem identitetsleverantörer out of box hello: Azure Active Directory, Facebook, Google, Account och Twitter. Din app kan använda valfritt antal dessa identitet providers tooprovide användarna med alternativ för hur de loggar in. tooexpand hello inbyggt stöd kan du integrera en annan identitetsleverantör eller [egna anpassade identitetslösning][custom-auth].

Om du vill tooget igång direkt, finns i hello följande kurser:

* [Lägg till iOS-app för autentisering tooyour] [ iOS] (eller [Android], [Windows], [Xamarin.iOS], [ Xamarin.Android], [Xamarin.Forms], eller [Cordova])
* [Användarautentisering för API Apps i Azure App Service][apia-user]
* [Kom igång med Azure App Service - del 2][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>Hur autentisering fungerar i App Service
I ordning tooauthenticate genom att använda en hello identitetsleverantörer, måste du först tooconfigure hello identitet providern tooknow för om ditt program. hello identitetsleverantör sedan ger-ID: N och hemligheter som du anger tooApp Service. Detta avslutar hello förtroenderelation så att Apptjänst kan validera intyg för användaren, till exempel autentiseringstoken från hello identitetsleverantören.

toosign i en användare med någon av dessa providers hello användaren måste vara omdirigerade tooan slutpunkt som loggar in användare för providern. Om kunder använder en webbläsare måste ha du Apptjänst automatiskt dirigera alla toohello slutpunkt för oautentiserade användare som loggar in användare. I annat fall måste toodirect kunderna för`{your App Service base URL}/.auth/login/<provider>`, där `<provider>` är ett av följande värden hello: aad, facebook, google, microsoft eller twitter. Mobil- och API-scenarier beskrivs i avsnitten nedan.

Användare som interagerar med ditt program via en webbläsare har en cookie så att de kan vara autentiserade när de surfar ditt program. För andra klienttyper, till exempel mobile, en JSON web token (JWT) som ska visas i hello `X-ZUMO-AUTH` sidhuvud, utfärdas toohello klienten. hello Mobile Apps klienten SDK hantera det åt dig. Du kan också en token som Azure Active Directory identitet och åtkomst-token kan direkt ingå i hello `Authorization` huvud som en [ägartoken](https://tools.ietf.org/html/rfc6750).

Apptjänst validerar alla cookie eller token att tillämpningsprogrammet utfärdar tooauthenticate användare. toorestrict som har åtkomst till ditt program finns hello [auktorisering](#authorization) senare i den här artikeln.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Mobila autentisering med en SDK-provider
När allting har konfigurerats på hello serverdel kan ändra du mobila klienter toosign in med App Service. Det finns två tillvägagångssätt här:

* Använda en SDK att en given identitetsleverantör publicerar tooestablish identitet och sedan får åtkomst tooApp Service.
* Använda en enda rad kod så att hello Mobile Apps klientdatorer SDK kan logga in användare.

> [!TIP]
> De flesta program ska använda en provider SDK tooget en mer konsekvent användarupplevelse när användare loggar in, toouse uppdatera support och tooget andra fördelar hello providern anger.
> 
> 

När du använder en provider SDK och användarna kan logga in körs tooan upplevelse som är integrerat mer med hello operativsystem hello appen på. Detta ger dig också en provider-token och viss användarinformation på hello-klienten, vilket gör det mycket enklare tooconsume graph API: er och anpassa hello användarupplevelsen. Du ser den här hänvisade tooas hello ”flödet” eller ”klienten dirigeras flödet” eftersom koden på hello-klient loggar in användare och hello klientkod har åtkomsttoken tooa providern ibland på bloggar och forum.

När en provider-token har fått måste toobe skickas tooApp Service för verifiering. När Apptjänst verifierar hello token, skapas en ny App Service-token som returneras toohello klienten App Service. hello Mobile Apps klient SDK har helper metoder toomanage detta utbyte och koppla automatiskt hello token tooall begäranden toohello programmet backend. Utvecklare kan också behålla en referens toohello providern token om de vill.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Mobila autentisering utan en SDK-provider
Om du inte vill att tooset upp en provider SDK kan du tillåta hello Mobile Apps-funktionen i Azure App Service toosign i åt dig. hello Mobile Apps klient-SDK ska öppna en web visa toohello leverantör du väljer och logga in hello användare. Ibland på bloggar och forum kommer att se den här hänvisade tooas hello ”server flödet” eller ”server-riktade flödet” eftersom hello-servern hanterar hello process som loggar in användare och hello klient-SDK får aldrig hello providern token.

Felkod toostart detta flöde är inkluderad i hello autentisering självstudier för varje plattform. Hello slutet av hello flödet, hello klient-SDK har en Apptjänst-token och hello token är automatiskt bifogade tooall begäranden toohello programmet backend.

### <a name="service-to-service-authentication"></a>Tjänst-till-tjänst-autentisering
Även om du kan ge användare åtkomst tooyour programmet, kan du också lita på ett annat program toocall egna API. Du kan till exempel ha en webbapp som en API-anrop i ett annat webbprogram. I det här scenariot använder du autentiseringsuppgifter för ett tjänstkonto i stället för användarens autentiseringsuppgifter tooget en token. Ett tjänstkonto kallas även en *tjänstens huvudnamn* i Azure Active Directory kallas och autentisering som använder sådant konto kallas även ett scenario med tjänst-till-tjänst.

> [!IMPORTANT]
> Eftersom mobila appar körs på kunden enheter, mobilprogram gör *inte* räknas som betrodda program och bör inte använda ett service principal flöde. De ska i stället använda ett flöde för användare som har beskrivs ovan.
> 
> 

Scenarier med service to service kan Apptjänst skydda ditt program med hjälp av Azure Active Directory. hello anropande programmet måste bara tooprovide en Azure Active Directory service principal autentiseringstoken som erhålls genom att ange hello klient-ID och hemliga från Azure Active Directory-klient. Ett exempel på det här scenariot som använder ASP.NET API-appar beskrivs av hello kursen [Service principal autentisering för API Apps][apia-service].

Om du vill toouse Apptjänst autentisering toohandle ett scenario med tjänst-till-tjänst kan använda du klientcertifikat eller grundläggande autentisering. Information om klientcertifikat i Azure finns [hur tooConfigure TLS ömsesidig autentisering för Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Information om grundläggande autentisering i ASP.NET finns [autentisering filter i ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Autentiseringen av tjänsten konto från en Apptjänst logic app tooan API-app är ett specialfall som beskrivs i [använda anpassat API som finns på Apptjänst med Logic apps](../logic-apps/logic-apps-custom-hosted-api.md).

## <a name="authorization"></a>Så här fungerar auktorisering i Apptjänst
Har du fullständig kontroll över hello-begäranden som har åtkomst till ditt program. App Service autentisering / auktorisering kan konfigureras med någon av följande beteenden hello:

* Tillåt endast autentiserade begäranden tooreach ditt program.
  
    Om en webbläsare får en anonym begäran, omdirigerar App-tjänsten tooa hello identitetsleverantör som du väljer så att användarna kan logga in på sidan. Om hello begäran kommer från en mobil enhet, hello returnerade svar är en HTTP *401 obehörig* svar.
  
    Med det här alternativet behöver du inte toowrite alla Autentiseringskod alls i din app. Om du behöver bättre auktorisering, är information om hello användaren tillgänglig tooyour kod.
* Tillåt alla begäranden tooreach ditt program men Validera autentiserade begäranden och skickar vidare autentiseringsinformation i hello HTTP-rubriker.
  
    Det här alternativet skiljer sig auktorisering beslut tooyour programkod. Det ger bättre flexibilitet vid anonyma begäranden, men du har toowrite kod.
* Ingen åtgärd för autentisering på hello begäranden kan alla förfrågningar tooreach ditt program.
  
    I det här fallet hello autentisering / auktoriseringsfunktionen är inaktiverad. hello uppgifterna för autentisering och auktorisering är helt tooyour programkod.

hello tidigare beteenden styrs av hello **åtgärd tootake när begäran inte har autentiserats** alternativ i hello Azure-portalen. Om du väljer ** logga in med *providernamn* **, alla begäranden har toobe autentiseras. **Tillåt begäran (ingen åtgärd)** skjuts upp hello beslut tooyour auktoriseringskod, men fortfarande ger autentiseringsinformationen. Om du vill toohave koden hantera allt, så kan du inaktivera hello autentisering / auktoriseringsfunktionen.

## <a name="working-with-user-identities-in-your-application"></a>Arbeta med användaridentiteter i ditt program
Apptjänst skickar vissa användare information tooyour program med hjälp av särskilda huvuden. Externa begäranden förhindrar att dessa huvuden och endast tillgänglig om anges av App Service-autentisering / auktorisering. Vissa exempel huvuden innehåller:

* X-MS-CLIENT--HUVUDNAMN
* X-MS-CLIENT-SÄKERHETSOBJEKT-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Kod som skrivs i valfritt språk eller ramverk får hello information som krävs i dessa huvuden. ASP.NET 4.6 appar hello **ClaimsPrincipal** ställs in automatiskt med hello lämpliga värden.

Programmet kan också hämta information om ytterligare användare via en HTTP GET på hello `/.auth/me` slutpunkten för ditt program. En giltig token som inkluderas hello-begäran returnerar en JSON-nyttolast med information om hello-providern som används kan hello den underliggande providern token och vissa andra användarinformation. hello Mobile Apps servern SDK: er anger hjälpmetoder toowork med dessa data. Mer information finns i [hur toouse hello Azure Mobile Apps Node.js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), och [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Dokumentation och ytterligare resurser
### <a name="identity-providers"></a>Identitetsleverantörer
Hej följande kurser visar hur tooconfigure App toouse olika leverantörer:

* [Hur tooconfigure din app toouse Azure Active Directory-inloggning][AAD]
* [Hur tooconfigure din app toouse Facebook-inloggning][Facebook]
* [Hur tooconfigure din app toouse Google-inloggning][Google]
* [Hur tooconfigure din app toouse Account inloggning][MSA]
* [Hur tooconfigure din app toouse Twitter-inloggning][Twitter]

Om du vill toouse ett identitetssystem än hello de som anges här, kan du också använda hello [Förhandsgranska stöd för anpassad autentisering i hello Mobile Apps .NET server SDK][custom-auth], som kan användas i web apps mobilappar och API apps.

### <a name="web-applications"></a>Webbprogram
hello följande kurser visar hur tooadd autentisering tooa webbprogrammet:

* [Kom igång med Azure App Service - del 2][web-getstarted]

### <a name="mobile-applications"></a>Mobila program
hello följande kurser visar hur hello tooadd autentisering tooyour mobila klienter med hjälp av server-riktade flöde:

* [Lägg till autentisering tooyour iOS-app][iOS]
* [Lägg till autentisering tooyour Android app][Android]
* [Lägg till autentisering tooyour Windows app][Windows]
* [Lägg till autentisering tooyour Xamarin.iOS-app][Xamarin.iOS]
* [Lägg till autentisering tooyour Xamarin.Android app][ Xamarin.Android]
* [Lägg till autentisering tooyour Xamarin.Forms-app][Xamarin.Forms]
* [Lägg till autentisering tooyour Cordova-app][Cordova]

Använd följande resurser om du vill toouse hello klienten dirigeras flödet för Azure Active Directory hello:

* [Använd hello Active Directory Authentication Library för iOS][ADAL-iOS]
* [Använd hello Active Directory Authentication Library för Android][ADAL-Android]
* [Använd hello Active Directory Authentication Library för Windows och Xamarin][ADAL-dotnet]

Använd följande resurser om du vill toouse hello klienten dirigeras flödet för Facebook hello:

* [Använd hello Facebook SDK för iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Använd följande resurser om du vill toouse hello klienten dirigeras flödet för Twitter hello:

* [Använd Twitter infrastruktur för iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Använd följande resurser om du vill toouse hello klienten dirigeras flödet för Google hello:

* [Använd hello Google inloggning SDK för iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>API-program
Hej följande kurser visar hur tooprotect API apps:

* [Användarautentisering för API Apps i Azure App Service][apia-user]
* [Tjänstens huvudnamn autentisering för API Apps i Azure App Service][apia-service]

[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[ Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
