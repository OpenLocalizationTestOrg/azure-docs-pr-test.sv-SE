---
title: "aaaLearn om hello annan token och anspråkstyper som stöds av Azure AD | Microsoft Docs"
description: "En guide för att förstå och utvärdera hello anspråk i hello SAML 2.0 och token JWT (JSON Web) token som utfärdats av Azure Active Directory (AAD)"
documentationcenter: na
author: dstrockis
services: active-directory
manager: mbaldwin
editor: 
ms.assetid: 166aa18e-1746-4c5e-b382-68338af921e2
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: c512894476613e94d86a994c32a8459d6cf00fbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-token-reference"></a>Tokenreferens för Azure AD
Azure Active Directory (AD Azure) genererar flera typer av säkerhetstoken i hello bearbetningen av varje autentiseringsflödet. Det här dokumentet beskriver hello format, säkerhet egenskaperna och innehållet i varje typ av token.

## <a name="types-of-tokens"></a>Typer av token
Azure AD stöder hello [OAuth 2.0-protokollet för auktorisering](active-directory-protocols-oauth-code.md), som utnyttjar både access_tokens och refresh_tokens.  Det stöder också autentisering och logga in via [OpenID Connect](active-directory-protocols-openid-connect-code.md), som innehåller en tredje typ av token, hello id_token.  Var och en av dessa token representeras som ett ”ägartoken”.

Ett ägartoken är en förenklad säkerhetstoken som ger hello ”ägar” åtkomst tooa skyddad resurs. På detta sätt är hello ”ägar” någon part som kan utgöra hello-token. Även om autentisering med Azure AD är obligatoriskt i ordning tooreceive ägartoken, åtgärder vidtas toosecure hello token, tooprevent fångas upp av en oavsiktlig part. Eftersom ägar-token inte har en inbyggd mekanism tooprevent obehörig parter från att använda dem, transporteras de i en säker kanal, till exempel transport layer security (HTTPS). Om ett ägartoken överförs i hello rensa, kan en hello man i mitten-attack använda tooacquire hello token och få obehörig åtkomst tooa skyddad resurs. hello tillämpas samma säkerhetsprinciper när lagring eller cachelagring ägar-token för senare användning. Se alltid till att appen skickar och lagrar ägar-token på ett säkert sätt. Flera säkerheten på ägar-token finns [RFC 6750 avsnitt 5](http://tools.ietf.org/html/rfc6750).

Många av hello-token som utfärdats av Azure AD implementeras som JSON Web token eller JWTs.  En JWT är ett kompakt, URL-säkert sätt att överföra information mellan två parter.  hello informationen i JWTs kallas ”anspråk” eller intyg för information om hello ägar- och ämne för hello-token.  hello anspråk i JWTs är JSON-objekt kodade och serialiseras för överföring.  Eftersom hello JWTs som utfärdats av Azure AD är signerat, men inte är krypterade, kan du enkelt inspektera hello innehållet i en JWT för felsökning.  Det finns flera verktyg för att göra det, t.ex [jwt.calebb.net](http://jwt.calebb.net). Mer information om JWTs kan du referera toohello [JWT-specifikationen](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens
Id_tokens är en form av inloggning säkerhetstoken som appen tar emot när du utför autentisering med [OpenID Connect](active-directory-protocols-openid-connect-code.md).  De visas i form av [JWTs](#types-of-tokens), och innehåller anspråk som du kan använda för att signera hello användare i din app.  Du kan använda hello anspråk i en id_token enligt önskemål – de används ofta för att visa kontoinformation eller gör besluten om åtkomstkontroll i en app.

Id_tokens är signerat, men inte krypterade just nu.  När appen tar emot en id_token, måste [Validera hello signatur](#validating-tokens) tooprove hello-token är äkta och verifiera några anspråk i hello token tooprove dess giltighet.  hello anspråk som verifieras av en app varierar beroende på krav för scenarier, men det finns några [vanliga anspråk verifieringar](#validating-tokens) som din app måste utföra i varje scenario.

Se hello efter avsnittet information på id_tokens anspråk, samt en exempel-id_token.  Observera att hello anspråk i id_tokens inte returneras i någon särskild ordning.  Dessutom nya anspråk som kan vara introduceras i id_tokens när som helst i tid - appen bryta inte som introduceras nya anspråk.  hello innehåller följande lista hello anspråk som din app på ett tillförlitligt sätt kan tolka när detta skrivs hello.  Om nödvändigt ännu mer information hittar du i hello [OpenID Connect specifikationen](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Exempel id_token
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [!TIP]
> Försök undersöks hello anspråk i hello exempel id_token genom att klistra in det i övningen [calebb.net](http://jwt.calebb.net).
> 
> 

#### <a name="claims-in-idtokens"></a>Anspråk i id_tokens
> [!div class="mx-codeBreakAll"]
| JWT-anspråk | Namn | Beskrivning |
| --- | --- | --- |
| `appid` |Program-ID:t |Identifierar hello-program som använder hello token tooaccess en resurs. hello-programmet kan fungera som sig själv eller för en användares räkning. hello program-ID representerar vanligen programobjekt, men det kan också vara en UPN serviceobjektet i Azure AD. <br><br> **JWT exempelvärde**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud` |Målgrupp |hello avsedda mottagaren av hello-token. hello-program som tar emot hello token måste kontrollera att hello målgruppen värde är korrekt och ignorera eventuella tokens som är avsedda för en annan publik. <br><br> **Exempel SAML värdet**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **JWT exempelvärde**: <br> `"aud":"https://contoso.com"` |
| `appidacr` |Application Authentication Context Class Reference |Anger hur hello klienten autentiserades. För en offentlig klient är hello-värdet 0. Om du använder klient-ID och klienthemlighet, är hello-värdet 1. <br><br> **JWT exempelvärde**: <br> `"appidacr": "0"` |
| `acr` |Authentication Context Class Reference |Anger hur hello ämne autentiserades som skillnad från toohello klient i hello Application Authentication Context Class Reference anspråk. Värdet ”0” anger hello slutanvändarens autentisering inte uppfyllde kraven hello i ISO/IEC 29115. <br><br> **JWT exempelvärde**: <br> `"acr": "0"` |
| Autentisering snabbmeddelanden |Poster hello datum och tid då autentisering inträffade. <br><br> **Exempel SAML värdet**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` | |
| `amr` |Autentiseringsmetod |Anger hur hello ämnet för hello token autentiserades. <br><br> **Exempel SAML värdet**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **JWT exempelvärde**:`“amr”: ["pwd"]` |
| `given_name` |Förnamn |Ger hello först eller namnet på hello användare ”angivna” som angetts på hello Azure AD-användarobjektet. <br><br> **Exempel SAML värdet**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **JWT exempelvärde**: <br> `"given_name": "Frank"` |
| `groups` |Grupper |Innehåller objekt-ID som representerar hello ämne gruppmedlemskap. Dessa värden är unik (se objekt-ID) och kan användas på ett säkert sätt för att hantera åtkomst till exempel framtvinga auktorisering tooaccess en resurs. hello-grupper som ingår i hello grupper anspråk är konfigurerade på grundval per program via hello ”groupMembershipClaims”-egenskap av hello applikationsmanifestet. En null-värde kan du inte alla grupper, värdet ”säkerhetsgruppen” kommer Ta endast med Active Directory-säkerhetsgrupp medlemskap och ett värde av ”alla” kommer inkluderar både säkerhetsgrupper och distributionslistor för Office 365. <br><br> **Exempel SAML värdet**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **JWT exempelvärde**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` |Identitetsprovider |Poster hello identitetsleverantör som autentiserats hello ämnet för hello-token. Det här värdet är identiska toohello värdet för hello utfärdaren anspråk om hello användarkontot finns i en annan klient än hello utfärdare. <br><br> **Exempel SAML värdet**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **JWT exempelvärde**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` |IssuedAt |Lagrar hello tid vid vilken hello token har utfärdats. Det är ofta använda toomeasure token dokumentens. <br><br> **Exempel SAML värdet**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **JWT exempelvärde**: <br> `"iat": 1390234181` |
| `iss` |Utfärdaren |Identifierar hello säkerhetstokentjänst (STS) som skapar och returnerar hello-token. I hello-token som Azure AD returnerar är hello utfärdaren sts.windows.net. hello är GUID i hello utfärdaren anspråksvärdet hello klient-ID för hello Azure AD-katalog. hello klient-ID är oföränderlig och tillförlitlig identifierare för hello directory. <br><br> **Exempel SAML värdet**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **JWT exempelvärde**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` |Efternamn |Tillhandahåller hello efternamn, efternamn eller efternamn för användaren hello enligt definitionen i hello Azure AD-användarobjektet. <br><br> **Exempel SAML värdet**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **JWT exempelvärde**: <br> `"family_name": "Miller"` |
| `unique_name` |Namn |Ger en mänsklig läsbar värde som identifierar hello ämnet för hello-token. Det här värdet är inte säkert toobe unika inom en klient och är utformad toobe används endast för visning. <br><br> **Exempel SAML värdet**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **JWT exempelvärde**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` |Objekt-ID |Innehåller en unik identifierare för ett objekt i Azure AD. Det här värdet är oföränderlig och kan inte tilldela om eller återanvänds. Använd hello objekt-ID tooidentify ett objekt i frågor tooAzure AD. <br><br> **Exempel SAML värdet**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **JWT exempelvärde**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` |Roller |Representerar alla roller för programmet som hello ämne har beviljats direkt eller indirekt via gruppmedlemskap och kan vara används tooenforce rollbaserad åtkomstkontroll. Programroller definieras på en applikationsspecifik basis via hello `appRoles` -egenskapen för hello programmanifestet. Hej `value` -egenskapen för varje roll för programmet är hello-värde som visas i hello roller anspråk. <br><br> **Exempel SAML värdet**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **JWT exempelvärde**: <br> `“roles”: ["Admin", … ]` |
| `scp` |Omfång |Anger hello personifiering behörigheter beviljade toohello client-program. hello standardbehörigheten är `user_impersonation`. hello ägare hello skyddade resursen kan registrera ytterligare värden i Azure AD. <br><br> **JWT exempelvärde**: <br> `"scp": "user_impersonation"` |
| `sub` |Ämne |Identifierar hello principal om vilka hello token Assert information, till exempel hello användaren av ett program. Det här värdet kan inte ändras och det går inte att tilldela om eller återanvänds, så det kan vara används tooperform auktoriseringskontroller på ett säkert sätt. Eftersom hello ämne finns alltid i hello tokens hello Azure AD-problem, rekommenderar vi använder det här värdet i ett system med generella tillstånd. <br> `SubjectConfirmation`är inte ett anspråk. Beskriver hur hello ämnet för hello token har verifierats. `Bearer`Anger hello ämnet bekräftas av deras tillgång hello-token. <br><br> **Exempel SAML värdet**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **JWT exempelvärde**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"` |
| `tid` |Klient-ID:t |En ändras, icke-återanvändningsbara ID som identifierar hello directory-klient som utfärdade hello-token. Du kan använda det här värdet tooaccess klient-specifika directory-resurser i ett program med flera innehavare. Exempelvis kan du använda det här värdet tooidentify hello innehavare i ett anrop toohello Graph API. <br><br> **Exempel SAML värdet**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **JWT exempelvärde**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"` |
| `nbf`, `exp` |Livslängd för token |Definierar hello tidsintervall inom vilket en token är giltig. hello-tjänst som validerar hello token bör kontrollera att hello aktuella datumet infaller inom hello livslängd för token, annars avvisar den hello-token. hello tjänsten kan tillåta för in toofive minuter utöver hello livslängd för token intervallet tooaccount för eventuella skillnader i tiden (”tiden förskjutning”) mellan Azure AD och hello-tjänsten. <br><br> **Exempel SAML värdet**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **JWT exempelvärde**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn` |Användarens huvudnamn |Lagrar hello hello användares UPN användarnamn.<br><br> **JWT exempelvärde**: <br> `"upn": frankm@contoso.com` |
| `ver` |Version |Lagrar hello versionsnumret för hello-token. <br><br> **JWT exempelvärde**: <br> `"ver": "1.0"` |

## <a name="access-tokens"></a>Åtkomst-token
När autentiseringen lyckas returnerar Azure AD åtkomst-token som kan vara används tooaccess skyddade resurser. hello åtkomsttoken är en base 64-kodat JSON-Webbtoken (JWT) och dess innehåll kan kontrolleras genom att köra det via en avkodare.

Om din app bara *använder* access token tooget åtkomst tooAPIs, kan (och bör) behandlas åtkomsttoken som helt ogenomskinlig - de är bara strängar som din app kan passera tooresources i HTTP-förfrågningar.

När du begär en åtkomst-token returnerar Azure AD även vissa metadata om hello åtkomst-token för förbrukning av din app.  Informationen omfattar hello förfallotiden för hello åtkomst-token och hello omfattningar som är giltig.  Detta gör att din app tooperform intelligent cachelagring av åtkomsttoken utan att behöva öppna hello åtkomsttoken själva tooparse.

Om din app är en API som skyddas med Azure AD som förväntar sig åtkomst-token i HTTP-begäranden, bör sedan du utföra verifiering och kontroll av hello-token som du får. Din app ska utföra verifiering av hello åtkomst-token innan du använder den tooaccess resurser. Mer information om verifiering finns [verifierar token](#validating-tokens).  
Mer information om hur toodo detta med .NET, se [skydda ett webb-API med hjälp av ägar-token från Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

## <a name="refresh-tokens"></a>Uppdatera token

Uppdatera token är säkerhetstokens som din app kan använda tooacquire nya åtkomst till token i en OAuth 2.0-flödet.  Den kan din app tooachieve långsiktiga åtkomst tooresources för en användares räkning utan interaktion med hello användare.

Uppdaterings-tokens är flera resurs.  Det är toosay som en uppdateringstoken togs emot under en Tokenbegäran för en resurs kan lösas för åtkomst-token tooa helt annan resurs. toodo detta, ange hello `resource` parameter i hello begäran toohello riktade resurs.

Uppdateringstoken är helt ogenomskinlig tooyour app. De är långlivade, men din app ska inte skrivas tooexpect som en uppdateringstoken ska gälla för en viss tidsperiod.  Uppdaterings-tokens kan vara inaktuella när som helst för en mängd olika skäl.  Hej sätt för din app tooknow om en uppdateringstoken är giltig är tooattempt tooredeem den genom att göra en tokenbegäran tooAzure AD-token för slutpunkt.

När du lösa in en uppdateringstoken för en ny åtkomsttoken får du en ny uppdateringstoken hello token svar.  Du bör spara hello nyligen utfärdade uppdateringstoken ersätter hello något du använde i hello-begäran.  Detta garanterar att uppdaterings-tokens vara giltigt så länge som möjligt.

## <a name="validating-tokens"></a>Verifiera token

I ordning toovalidate en id_token eller en access_token bör appen Verifiera signatur och hello anspråk i båda hello-token. Appen bör också i ordning toovalidate åtkomsttoken verifiera hello utfärdare, hello målgrupp och hello token-signering. Dessa måste toobe verifieras mot hello värden i hello OpenID discovery-dokumentet. Till exempel hello klient oberoende version av hello dokumentet finns på [https://login.microsoftonline.com/common/.well-known/openid-configuration](https://login.microsoftonline.com/common/.well-known/openid-configuration). Azure AD-mellanprogram har inbyggda funktioner för att validera åtkomst-token och du kan bläddra igenom våra [exempel](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-code-samples) toofind en i hello önskat språk. Mer information om hur tooexplicitly validera en JWT-token finns hello [manuell JWT validering exempel](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation).  

Vi tillhandahåller bibliotek och kodexempel som visar hur tooeasily hantera token validering - anges hello informationen nedan bara för som toounderstand hello underliggande processen.  Det finns också flera bibliotek med öppen källkod från tredje part för JWT-verifiering: det finns minst ett alternativ för nästan alla plattform och det språket. Mer information om Azure AD-autentiseringsbibliotek och kodexempel finns [Azure AD-autentiseringsbibliotek](active-directory-authentication-libraries.md).

#### <a name="validating-hello-signature"></a>Verifiera hello signatur

En JWT innehåller tre segment som avgränsas med hello `.` tecken.  hello första segmentet kallas hello **huvud**, hello andra som hello **brödtext**, och hello tredje som hello **signatur**.  hello signatur segment kan vara används toovalidate hello äkthet hello token så att den kan vara betrott av din app.

Token som utfärdats av Azure AD är signerade med industry standard asymmetriska krypteringsalgoritmer, till exempel RSA-256. hello huvudet i hello JWT innehåller information om hello nyckeln och krypteringsmetod används toosign hello token:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

Hej `alg` anspråk anger hello-algoritmen som har använt toosign hello-token när hello `x5t` anspråk anger hello viss offentlig nyckel som har använt toosign hello-token.

Vid en viss tidpunkt, kan Azure AD logga en id_token med någon av en viss uppsättning privat-offentligt nyckelpar. Azure AD roterar hello möjliga uppsättning nycklar regelbundet, så att din app ska skrivas toohandle dessa nyckel ändras automatiskt.  En rimlig frekvens toocheck för uppdateringar toohello offentliga nycklar som används av Azure AD är 24 timmar.

Du kan skaffa hello signering nyckeldata nödvändiga toovalidate hello signatur med hjälp av hello OpenID Connect metadata dokument som finns på:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [!TIP]
> Denna URL i en webbläsare kan du prova!
> 
> 

Metadatadokumentet är en JSON-objekt som innehåller flera användbara delar av information, till exempel hello platsen för hello olika slutpunkter som krävs för att utföra autentisering med OpenID Connect.  

Den innehåller också en `jwks_uri`, vilket ger hello platsen för hello uppsättning offentliga nycklar används toosign token.  hello JSON-dokumentet finns på hello `jwks_uri` innehåller alla hello offentlig nyckelinformation används av den viss tidpunkten.  Din app kan använda hello `kid` anspråk i hello JWT huvud tooselect vilka offentliga nyckeln i det här dokumentet har använt toosign en viss token.  Den kan utföra signaturverifiering med hello rätt offentlig nyckel och hello angivna algoritm.

Utför signaturverifiering är utanför hello omfånget för det här dokumentet - det finns många bibliotek med öppen källkod för att hjälpa dig att göra det om det behövs.

#### <a name="validating-hello-claims"></a>Verifiera hello anspråk

När appen tar emot en token (en id_token när användaren loggar in, eller en åtkomst-token som en ägartoken i hello HTTP-begäran) bör det också utföra några kontroller mot hello anspråk i hello-token.  Dessa inkludera, men är inte begränsade till:

* Hej **målgruppen** anspråk - tooverify hello token är avsedda toobe angivna tooyour app.
* Hej **inte före** och **förfallotid** anspråk - tooverify hello token inte har gått ut.
* Hej **utfärdaren** anspråk - tooverify hello token utfärdades verkligen tooyour app av Azure AD.
* Hej **temporärt ID** -toomitigate en token replay-attacker.
* med mera...

En fullständig lista över anspråk verifieringar din app ska utföra för ID-token finns toohello [OpenID Connect specifikationen](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation). Information om hello förväntade värden för dessa anspråk som ingår i föregående hello [id_token avsnittet](#id-tokens) avsnitt.

## <a name="sample-tokens"></a>Exempel-token

Det här avsnittet visas exempel på SAML- och JWT-token som returnerar Azure AD. Dessa exempel kan du se hello anspråk i kontexten.

### <a name="saml-token"></a>SAML-Token

Det här är ett exempel på en typisk SAML-token.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT-Token - personifiering
Det här är ett exempel på en typisk JSON web token (JWT) används i en grant-auktoriseringskodflödet.
I tillägg tooclaims hello token innehåller ett versionsnummer i **ver** och **appidacr**, hello autentisering kontexten klassreferens, som anger hur hello klienten autentiserades. För en offentlig klient är hello-värdet 0. Om en klient-ID eller klienthemlighet användes är hello-värdet 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Relaterat innehåll
* Se hello Azure AD Graph [åtgärder](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) och hello [princip entiteten](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity), toolearn mer om hur du hanterar livslängd för token-principer via hello Azure AD Graph API.
* Mer information och exempel på att hantera principer via PowerShell-cmdletar, inklusive exempel, finns [konfigurerbara token livslängd i Azure AD](../active-directory-configurable-token-lifetimes.md). 
