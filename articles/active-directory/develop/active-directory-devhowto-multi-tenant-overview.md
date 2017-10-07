---
title: "aaaHow toobuild en app som kan logga in alla Azure AD-användare | Microsoft Docs"
description: "Stegvisa kan anvisningar för att skapa ett program som logga in en användare från en Azure Active Directory-klient, även kallat ett program med flera innehavare."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 35af95cb-ced3-46ad-b01d-5d2f6fd064a3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 123ea8125fa3c308ce0f124cc58e85ec28d476d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosign-in-any-azure-active-directory-ad-user-using-hello-multi-tenant-application-pattern"></a>Hur toosign i alla Azure Active Directory (AD) användaren med hello programmönster för flera innehavare
Om du tillhandahåller en programvara som en tjänst programmet toomany organisationer bör konfigurera du ditt program tooaccept inloggningar från Azure AD-klient.  Detta kallas att göra ditt program med flera innehavare i Azure AD.  Användare i Azure AD-klient ska kunna toosign i tooyour program efter principer toouse sitt konto med ditt program.  

Om du har ett befintligt program som har ett eget konto eller andra typer av logga in från andra molntjänstleverantörer av har stöd för är att lägga till Azure AD-inloggning från en klient enkelt. Bara registrera din app, Lägg till kod logga in via OAuth2, OpenID Connect eller SAML och placera en ”logga In med Microsoft”-knappen i ditt program. Klicka på följande knapp toolearn mer om anpassning av programmet hello.

[! [Logga in knappen] [AAD-inloggning]][AAD-App-Branding]

Den här artikeln förutsätter att du redan är bekant med att skapa en enskild klient program för Azure AD.  Om du inte head säkerhetskopiera toohello [developer guide webbsida] [ AAD-Dev-Guide] och prova en av våra snabbstarter!

Det finns fyra enkla steg tooconvert ditt program i en app för flera innehavare av Azure AD:

1. Uppdatera ditt program registrering toobe flera innehavare
2. Uppdatera din kod toosend begäranden toohello/Common slutpunkt 
3. Uppdatera din kod toohandle flera utfärdaren värden
4. Förstå användare och admin medgivande och göra ändringar av lämplig kod

Nu ska vi titta på varje steg i detalj. Du kan även gå direkt för[den här listan med flera innehavare exempel][AAD-Samples-MT].

## <a name="update-registration-toobe-multi-tenant"></a>Uppdatera registreringen toobe flera innehavare
Som standard är web app/API registreringar i Azure AD enda innehavare.  Du kan göra registreringen flera innehavare genom att söka efter hello ”med flera innehavare” växla hello sidan med egenskaper för programmet registreringen i hello [Azure-portalen] [ AZURE-portal] och ange värdet för ”Ja”.

Observera även, innan ett program kan göras med flera innehavare, Azure AD kräver hello App-ID-URI för hello programmet toobe globalt unika. hello App-ID URI är en av hello sätt som ett program har identifierats i protokollmeddelanden.  Det är tillräcklig för hello App-ID URI toobe unikt inom den klienten för en enskild klient-program.  För ett program med flera innehavare, måste det vara globalt unikt så att Azure AD kan hitta hello program över alla klienter.  Globala unika tillämpas genom att kräva hello App-ID URI toohave ett värdnamn som matchar en verifierad domän till hello Azure AD-klient.  Till exempel om hello namn för din klient har contoso.onmicrosoft.com sedan en giltig URI för App-ID är `https://contoso.onmicrosoft.com/myapp`.  Om din klient har en verifierad domän i `contoso.com`, och sedan en giltig URI för App-ID kan även vara `https://contoso.com/myapp`.  Ange ett program som flera innehavare misslyckas om hello App-ID URI inte följer detta mönster.

-Klientregistreringar finns flera innehavare som standard.  Du behöver inte tootake någon åtgärd toomake en native client programmet registrering flera innehavare.

## <a name="update-your-code-toosend-requests-toocommon"></a>Uppdatera din kod toosend begäranden för/common
I ett program som en organisation skickas inloggningsförfrågningar toohello klient inloggning slutpunkt. Till exempel för contoso.onmicrosoft.com hello slutpunkten skulle vara:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Begäranden skickade tooa tenant-slutpunkten kan logga in användare (eller gäster) i den klient tooapplications i den klienten.  Med ett program med flera innehavare, hello program inte vet vilken klient hello användaren tillhör, så du inte kan skicka direkt begär tooa tenant-slutpunkten.  Begäranden skickas i stället tooan slutpunkt som multiplexes över alla Azure AD-klienter:

    https://login.microsoftonline.com/common

När Azure AD tar emot en begäran på hello/vanliga endpoint den hello användaren loggar in och som en följd identifierar vilken klient hello användare kommer från.  Hej/vanliga slutpunkt som fungerar med alla hello autentiseringsprotokoll som stöds av Azure AD: OpenID Connect, OAuth 2.0, SAML 2.0 och WS-Federation.

hello inloggning svar toohello programmet sedan innehåller en token som representerar hello användare.  hello utfärdaren värdet i hello token visar ett program vilken klient hello användaren tillhör.  När ett svar returneras från hello/vanliga slutpunkt, hello utfärdaren värdet i hello token motsvarar toohello användarens klient.  Det är viktigt toonote hello/vanliga slutpunkt är inte en klient och är inte en utfärdare, är det bara en multiplexor.  När du använder/Common behov hello logiken i dina program toovalidate token toobe uppdateras tootake detta hänsyn. 

Som nämnts tidigare och flera innehavare program bör också ge en konsekvent inloggningsupplevelse för användare, hello följande Azure AD-program företagsanpassning riktlinjer. Klicka på följande knapp toolearn mer om anpassning av programmet hello.

[! [Logga in knappen] [AAD-inloggning]][AAD-App-Branding]

Låt oss ta en titt på hello användning av hello/Common slutpunkt och implementeringen koden i detalj.

## <a name="update-your-code-toohandle-multiple-issuer-values"></a>Uppdatera din kod toohandle flera utfärdaren värden
Webbprogram och webb-API: er får och validera token från Azure AD.  

> [!NOTE]
> Intern klientprogram begära och ta emot token från Azure AD, de gör så toosend dem tooAPIs, där de verifieras.  Interna program validera inte token och hantera dem som täckande.
> 
> 

Nu ska vi titta på hur ett program validerar token tas emot från Azure AD.  En enskild klient-program tar normalt en slutpunktsvärde som:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

och använda den tooconstruct som en URL för tjänstmetadata (i det här fallet OpenID Connect):

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

toodownload två viktiga uppgifter som används toovalidate token: hello-klient signering nycklar och utfärdaren värde.  Varje Azure AD-klient har unika utfärdaren värdet hello formuläret:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

hello GUID-värde är där hello rename-säkert version av hello klient-ID för hello-klienten.  Om du klickar på hello föregående länk metadata för `contoso.onmicrosoft.com`, du kan se den här utfärdaren värde i hello dokumentet.

När en enskild klient program validerar token, kontrollerar hello signaturen för hello token mot hello Signeringsnycklar från hello metadata dokument. Detta gör det toomake att hello utfärdaren värdet hello token matchar hello något som hittades i hello Metadatadokumentet.

Eftersom hello/vanliga slutpunkten överensstämmer inte tooa klient och är inte en utfärdare när du undersöker hello utfärdaren värdet i hello metadata för/vanliga har en mallbaserat URL i stället för ett faktiskt värde:

    https://sts.windows.net/{tenantid}/

Därför kan ett program med flera innehavare kan inte verifiera token genom att bara matchar hello utfärdaren värdet i hello metadata med hello `issuer` värdet i hello-token.  Ett program med flera innehavare behöver logik toodecide utfärdaren värden som är giltiga och är inte, baserat på hello klient-ID delen av hello utfärdaren värde.  

Till exempel om ett program med flera innehavare kan bara logga in från specifika klienter som har registrerat dig för tjänsten, sedan det måste kontrollera hello utfärdaren värde eller hello `tid` anspråksvärde i hello token toomake att den klienten som är i sin lista över prenumeranter.  Om ett program med flera innehavare endast behandlar enskilda användare och inte göra någon åtkomst beslut baserat på klienter, kan den Ignorera hello utfärdaren värde helt och hållet.

I hello flera innehavare prover i hello [relaterade innehåll](#related-content) avsnittet hello slutet av den här artikeln utfärdaren validering är inaktiverat tooenable alla Azure AD-klient toosign i.

Nu ska vi titta på hello användarupplevelsen för användare som loggar in program toomulti-klienter.

## <a name="understanding-user-and-admin-consent"></a>Förstå användare och admin-medgivande
Hello program måste vara representerad i hello användarens klient för en användare toosign i tooan program i Azure AD.  Detta gör hello organisation toodo exempelvis tillämpa unika principer när användare från deras klient loggar in toohello program.  För en enskild klient programmet är registreringen enkel; Det har hello som händer när du registrerar hello program i hello [Azure-portalen][AZURE-portal].

För ett program med flera innehavare bor hello registreringen för programmet hello i hello Azure AD-klient används av hello-utvecklare.  När en användare från en annan klient loggar in toohello program för hello första gången, frågar Azure AD dem tooconsent toohello behörigheter som begärs av programmet hello.  Om de accepterar och sedan en representation av programmet hello kallas en *tjänstens huvudnamn* skapas i hello användarens klient och inloggningen kan fortsätta. Hello-katalog som innehåller hello användarens medgivande toohello program skapas också en delegering. Se [program och tjänstens huvudnamn objekt] [ AAD-App-SP-Objects] information om hello programmet program och ServicePrincipal objekt och hur de hör ihop tooeach andra.

![Medgivande toosingle skikt app][Consent-Single-Tier] 

Den här medgivande upplevelse påverkas av hello behörigheter som begärs av programmet hello.  Azure AD stöder två typer av endast app- och delegera behörigheter:

* En delegerad behörighet ger en programmet hello möjlighet tooact som en inloggad användare för en delmängd av hello saker hello användare kan göra.  Exempelvis kan du bevilja en programmet hello delegerad behörighet tooread hello inloggad användares kalender.
* En app bara behörigheten direkt toohello hello programmets identitet.  Exempelvis kan du bevilja en programmet hello endast app-tooread hello behörighetslistan för användare i en klient oavsett vem som har loggat in toohello program.

Vissa behörigheter kan vara tilllåten tooby en vanlig användare, medan andra kräver en Innehavaradministratör medgivande. 

### <a name="admin-consent"></a>Admin-medgivande
Behörigheter endast appen kräver alltid en Innehavaradministratör medgivande.  Om ditt program begär en endast app-behörighet och en användare försöker toosign i toohello program, visas ett felmeddelande om hello användaren inte kan tooconsent.

Vissa delegerade behörigheter kräver också en Innehavaradministratör medgivande.  Till exempel kräver hello möjlighet toowrite tillbaka tooAzure AD som hello inloggad användare en Innehavaradministratör medgivande.  Som endast app-behörigheter, om en vanlig användare försöker toosign i tooan program som kräver en delegerad behörighet som kräver att administratören medgivande får programmet ett fel.  Huruvida en behörighet kräver admin samtycke bestäms av hello utvecklare som publicerats hello resurs och kan vara finns i dokumentationen för hello för hello resurs.  Länkar tootopics som beskriver hello tillgängliga behörigheter för hello Azure AD Graph API och Microsoft Graph API är i hello [relaterade innehåll](#related-content) i den här artikeln.

Om programmet använder behörigheter som kräver godkännande av administratören, måste toohave en gest, till exempel en knapp eller en länk där Hej administratör kan starta hello åtgärden.  hello-begäran som programmet skickar den här åtgärden är ett vanligt OAuth2/OpenID Connect auktoriseringsbegäran, men som också innehåller hello `prompt=admin_consent` frågesträngparametern.  När Hej administratör har godkänt och hello tjänstens huvudnamn skapas i hello kundens klienten behöver inte efterföljande inloggning förfrågningar hello `prompt=admin_consent` parameter. Eftersom Hej administratör har beslutat hello begärt behörigheter accepteras uppmanas inga andra användare i hello-klient för godkännande från den tidpunkten och framåt.

Hej `prompt=admin_consent` parameter kan även användas av program som begär behörighet som inte kräver godkännande av administratören. Detta görs när hello programobjekt kräver en upplevelse där hello innehavaradministration ”registrerar sig” en tid och ingen användare tillfrågas om samtycke från den tidpunkten på.

Om ett program kräver godkännande av administratören och en administratör loggar i men hello `prompt=admin_consent` parametern skickas inte, Hej administratör kommer har medgivande toohello programmet **endast för sitt användarkonto**.  Vanliga användare fortfarande inte kan toosign i och medgivande toohello program.  Detta är användbart om du vill att toogive hello klient administratör hello möjlighet tooexplore programmet innan andra användare åtkomst.

En Innehavaradministratör kan inaktivera hello möjligheten för vanliga användare tooconsent tooapplications.  Om den här funktionen är inaktiverad krävs alltid admin medgivande för hello programmet toobe i hello-klient.  Om du vill tootest ditt program med vanlig användare samtycke inaktiverad, hittar du hello konfigurationsväxel i hello Azure AD-klient konfigurationsavsnittet för hello [Azure-portalen][AZURE-portal].

> [!NOTE]
> Vissa program vill en upplevelse där vanliga användare kan tooconsent från början, och senare hello-programmet kan omfatta hello administratören och begär behörighet som kräver godkännande av administratören.  Det finns inget sätt toodo detta med en enda programregistrering i Azure AD idag.  hello kommande Azure AD v2 endpoint kan program toorequest behörigheter vid körning, i stället för närvarande registreringen, vilket gör att det här scenariot.  Mer information finns i hello [Utvecklarhandbok för Azure AD App Model v2][AAD-V2-Dev-Guide].
> 
> 

### <a name="consent-and-multi-tier-applications"></a>Medgivande och flera nivåer program
Programmet kan ha flera nivåer, var och en representeras av dess egna registrering i Azure AD.  Till exempel ett enhetligt program som anropar ett webb-API eller ett webbprogram som anropar ett webb-API.  I båda dessa fall hello klienten (inbyggda appen eller webbprogram) begär behörighet toocall hello resurs (webb-API).  För hello klienten toobe har samtyckt till en kund-klient, måste alla resurser toowhich begär behörighet redan finnas i hello kundens klient.  Om detta inte är uppfyllt, returneras Azure AD ett fel som hello resursen måste läggas till först.

**Flera nivåer i en enskild klient**

Detta kan vara ett problem om tillämpningsprogrammet logiska består av två eller flera program registreringar, till exempel en separat klient och resurs.  Hur du får hello resurs i hello kunden klient första?  Azure AD omfattar det här fallet genom att aktivera klienten och resursen toobe godkänt i ett enda steg. hello användaren ser hello totalsumman av hello behörigheter som begärs av både hello-klienten och resursen på hello medgivande sida.  tooenable det här beteendet hello resursens appregistrering måste innehålla hello klienten App-ID som en `knownClientApplications` i sitt applikationsmanifest.  Exempel:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Den här egenskapen kan uppdateras via hello resurs [programmets manifest][AAD-App-Manifest]. Detta visas i en-klienten på flera nivåer anropa webb-API-exemplet i hello [relaterade innehåll](#related-content) avsnittet hello slutet av den här artikeln. hello ger följande diagram en översikt av medgivande för en flernivåapp som registrerats i en enskild klient:

![Medgivande toomulti skikt kända-klientappen][Consent-Multi-Tier-Known-Client] 

**Flera nivåer i flera innehavare**

Liknande fall händer om hello olika nivåer av ett program som har registrerats i olika klienter.  Anta till exempel att hello fall för att skapa en native client-program som anropar hello Office 365 Exchange Online-API.  toodevelop hello inbyggt, och senare för hello programspecifika toorun i en kund-klient, hello Exchange Online tjänstens huvudnamn måste finnas.  I det här fallet måste hello utvecklare och kunden köpa Exchange Online för hello service principal toobe skapas i sina klienter.  

Hello gäller en API som skapats av en organisation än Microsoft, behöver hello utvecklaren av hello API tooprovide ett sätt för sina kunder tooconsent hello program till sina kunder innehavare. hello rekommenderas design är för hello 3 part developer toobuild hello API så att den kan också fungera som en web klienten tooimplement registrering:

1. Följ hello tidigare avsnitt tooensure hello API implementerar hello flera innehavare registrering/kod programkrav
2. Dessutom tooexposing hello API: er scope och roller, kontrollera hello registrering innehåller hello ”logga in och Läs användarprofil” Azure AD-behörigheten (som standard)
3. Implementera en logga-i/sign-upp sida i hello webbklienten följande hello [admin medgivande](#admin-consent) vägledning nämnts tidigare 
4. När hello användaren godkänner toohello program, hello service principal och samtycke delegering kopplingarna i klientorganisationen och hello det ursprungliga programmet kan hämta token för hello API

hello följande diagram ger en översikt av medgivande för en flernivåapp som registrerats i olika klienter:

![Medgivande toomulti skikt med flera app][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Återkalla medgivande
Användare och administratörer kan återkalla medgivande tooyour program när som helst:

* Användare återkalla åtkomst tooindividual program genom att ta bort dem från sina [åtkomst panelen program] [ AAD-Access-Panel] lista.
* Administratörer återkalla åtkomst tooapplications genom att ta bort dem från Azure AD med hjälp av hello Azure AD management avsnitt i hello [Azure-portalen][AZURE-portal].

Om en administratör godkänner tooan program för alla användare i en klient, kan användare kan inte återkalla åtkomst individuellt.  Endast Hej administratör kan återkalla åtkomst och endast för hela programmet hello.

### <a name="consent-and-protocol-support"></a>Medgivande och protokollstöd
Medgivande stöds i Azure AD via OpenID Connect hello OAuth WS-Federation och SAML-protokoll.  hello SAML och WS-Federation-protokollen stöder inte hello `prompt=admin_consent` parameter, så admin medgivande är endast möjligt via OAuth och OpenID Connect.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Program med flera klienter och cachelagring åtkomst-token
Program med flera klienter kan också få åtkomst-token toocall API: er som skyddas av Azure AD.  Ett vanligt fel när du använder hello Active Directory Authentication Library (ADAL) med ett program med flera innehavare, är tooinitially begäran en token för en användare som använder/Common, ett svar och sedan begära en efterföljande token för samma användare också använder/Common.  Eftersom hello svaret från Azure AD inte kommer från en klient/gemensamma ADAL cachelagrar hello token som från hello-klient. hello efterföljande anrop för/common tooget är en åtkomst-token för hello användaren missar hello cacheposten och hello användaren tillfrågas toosign i igen.  tooavoid saknas hello-cache, se till att följande anrop för en redan inloggad användare görs toohello tenant-slutpunkten.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur toobuild ett program som kan logga in en användare från en Azure Active Directory-klient. Du kan också uppdatera ditt program tooaccess API: er som exponeras av Microsoft-resurser, t.ex. Office 365 för att aktivera enkel inloggning mellan din app och Azure Active Directory. Så kan du erbjuda en anpassad upplevelse i ditt program, till exempel visar kontextuella information toohello användare som deras profilbild eller deras nästa möte i kalendern. toolearn mer om hur du gör API anropar tooAzure Active Directory och Office 365-tjänster som Exchange, SharePoint, OneDrive, OneNote, Planner, Excel och mer finns: [Microsoft Graph API][MSFT-Graph-overview].


## <a name="related-content"></a>Relaterat innehåll
* [Exempel på program för flera innehavare][AAD-Samples-MT]
* [Riktlinjer för varumärkesanpassning för program][AAD-App-Branding]
* [Azure AD-Guide för utvecklare][AAD-Dev-Guide]
* [Program och tjänstens huvudnamn objekt][AAD-App-SP-Objects]
* [Integrera program med Azure Active Directory][AAD-Integrating-Apps]
* [Översikt över hello medgivande Framework][AAD-Consent-Overview]
* [Behörighetsomfattningen för Microsoft Graph API][MSFT-Graph-permision-scopes]
* [Azure AD Graph API Behörighetsomfattningen][AAD-Graph-Perm-Scopes]

Använd hello följande kommentarer avsnittet tooprovide feedback och hjälp oss att förfina och utforma innehållet.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-portal]: https://portal.azure.com
[MSFT-Graph-overview]: https://graph.microsoft.io/en-us/docs/overview/overview
[MSFT-Graph-permision-scopes]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ../active-directory-appmodel-v2-overview.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














