---
title: "resurser för aaaAzure API Management-mall | Microsoft Docs"
description: "Läs mer om hello typer av resurser som är tillgängliga för användning i developer portal mallar i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 51a1b4c6-a9fd-4524-9e0e-03a9800c3e94
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 2221e3852986d485d13817b483e473dfe451d3c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-template-resources"></a>Resurser med Azure API Management-mall
Azure API Management ger hello följande typer av resurser för användning i hello developer portal mallar.  
  
-   [Strängresurser](#strings)  
  
-   [Glyf resurser](#glyphs)  
  
##  <a name="strings"></a>Strängresurser  
 API Management ger en omfattande uppsättning strängresurserna för användning i hello developer-portalen. Dessa resurser är lokaliserade till alla hello språk som stöds av API-hantering. hello standarduppsättning mallar använder dessa resurser för sidhuvuden, etiketter och alla fasta strängar som visas i hello developer-portalen. toouse en strängresurs i dina mallar anger hello resurs sträng prefixet följt av namnet på hello sträng, enligt följande exempel hello.  
  
```  
{% localized "Prefix|Name" %}  
  
```  
  
 hello följande exempel är från hello produkten mall och visar **produkter** hello överst på hello sidan.  
  
```  
<h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>  
  
```  
  
 Läs toohello följande tabeller för hello strängresurser tillgängliga för användning i mallarna developer portal. Använd hello tabellnamn som hello prefix för hello strängresurser i tabellen.  
  
-   [ApisStrings](#ApisStrings)  
  
-   [ApplicationListStrings](#ApplicationListStrings)  
  
-   [AppDetailsStrings](#AppDetailsStrings)  
  
-   [AppStrings](#AppStrings)  
  
-   [CommonResources](#CommonResources)  
  
-   [CommonStrings](#CommonStrings)  
  
-   [Dokumentation](#Documentation)  
  
-   [ErrorPageStrings](#ErrorPageStrings)  
  
-   [IssuesStrings](#IssuesStrings)  
  
-   [NotFoundStrings](#NotFoundStrings)  
  
-   [ProductDetailsStrings](#ProductDetailsStrings)  
  
-   [ProductsStrings](#ProductsStrings)  
  
-   [ProviderInfoStrings](#ProviderInfoStrings)  
  
-   [SigninResources](#SigninResources)  
  
-   [SigninStrings](#SigninStrings)  
  
-   [SignupStrings](#SignupStrings)  
  
-   [SubscriptionListStrings](#SubscriptionListStrings)  
  
-   [SubscriptionStrings](#SubscriptionStrings)  
  
-   [UpdateProfileStrings](#UpdateProfileStrings)  
  
-   [Användarprofil](#UserProfile)  
  
###  <a name="ApisStrings"></a>ApisStrings  
  
|Namn|Text|  
|----------|----------|  
|PageTitleApis|API:er|  
  
###  <a name="AppDetailsStrings"></a>AppDetailsStrings  
  
|Namn|Text|  
|----------|----------|  
|WebApplicationsDetailsTitle|Programmet preview|  
|WebApplicationsRequirementsHeader|Krav|  
|WebApplicationsScreenshotAlt|Skärmbild|  
|WebApplicationsScreenshotsHeader|Skärmdumpar|  
  
###  <a name="ApplicationListStrings"></a>ApplicationListStrings  
  
|Namn|Text|  
|----------|----------|  
|WebDevelopersAppDeleteConfirmation|Är du säker på att du vill tooremove program?|  
|WebDevelopersAppNotPublished|Inte publiceras|  
|WebDevelopersAppNotSubminted|Inte skicka|  
|WebDevelopersAppTableCategoryHeader|Kategori|  
|WebDevelopersAppTableNameHeader|Namn|  
|WebDevelopersAppTableStateHeader|Status|  
|WebDevelopersEditLink|Redigera|  
|WebDevelopersRegisterAppLink|Registrera program|  
|WebDevelopersRemoveLink|Ta bort|  
|WebDevelopersSubmitLink|Skicka|  
|WebDevelopersYourApplicationsHeader|Dina program|  
  
###  <a name="AppStrings"></a>AppStrings  
  
|Namn|Text|  
|----------|----------|  
|WebApplicationsHeader|Program|  
  
###  <a name="CommonResources"></a>CommonResources  
  
|Namn|Text|  
|----------|----------|  
|NoItemsToDisplay|Hittade inga resultat.|  
|GeneralExceptionMessage|Något är inte rätt. Det kan vara ett tillfälligt fel eller ett programfel. Försök igen.|  
|GeneralJsonExceptionMessage|Något är inte rätt. Det kan vara ett tillfälligt fel eller ett programfel. Ange, uppdatera hello sidan och försök igen.|  
|ConfirmationMessageUnsavedChanges|Det finns vissa ändringar. Är du säker toocancel och om du vill ta bort hello ändringar?|  
|AzureActiveDirectory|Azure Active Directory|  
|HttpLargeRequestMessage|HTTP-brödtext i begäran för stor.|  
  
###  <a name="CommonStrings"></a>CommonStrings  
  
|Namn|Text|  
|----------|----------|  
|ButtonLabelCancel|Avbryt|  
|ButtonLabelSave|Spara|  
|GeneralExceptionMessage|Något är inte rätt. Det kan vara ett tillfälligt fel eller ett programfel. Försök igen.|  
|NoItemsToDisplay|Det finns inga objekt toodisplay.|  
|PagerButtonLabelFirst|Första|  
|PagerButtonLabelLast|senaste|  
|PagerButtonLabelNext|Nästa|  
|PagerButtonLabelPrevious|Föregående|  
|PagerLabelPageNOfM|Sidan {0} för {1}|  
|PasswordTooShort|hello lösenordet är för kort|  
|EmailAsPassword|Använd inte din e-postadress som ditt lösenord|  
|PasswordSameAsUserName|Lösenordet får inte innehålla ditt användarnamn|  
|PasswordTwoCharacterClasses|Använd olika teckenuppsättningar klasser|  
|PasswordTooManyRepetitions|För många upprepningar|  
|PasswordSequenceFound|Lösenordet innehåller sekvenser|  
|PagerLabelPageSize|Sidstorleken|  
|CurtainLabelLoading|Läser in...|  
|TablePlaceholderNothingToDisplay|Det finns inga data för valda hello period och omfång|  
|ButtonLabelClose|Stäng|  
  
###  <a name="Documentation"></a>Dokumentation  
  
|Namn|Text|  
|----------|----------|  
|WebDocumentationInvalidHeaderErrorMessage|Ogiltigt huvud: {0}|  
|WebDocumentationInvalidRequestErrorMessage|Ogiltig begärande-URL|  
|TextboxLabelAccessToken|Åtkomsttoken *|  
|DropdownOptionPrimaryKeyFormat|Primär-{0}|  
|DropdownOptionSecondaryKeyFormat|Sekundär-{0}|  
|WebDocumentationSubscriptionKeyText|Din prenumeration nyckel|  
|WebDocumentationTemplatesAddHeaders|Lägg till nödvändiga HTTP-huvuden|  
|WebDocumentationTemplatesBasicAuthSample|Grundläggande auktorisering exempel|  
|WebDocumentationTemplatesCurlForBasicAuth|för grundläggande auktorisering:--användaren {username}: {lösenord}|  
|WebDocumentationTemplatesCurlValuesForPath|Ange värden för parametrarna (visas som {...}), din prenumeration nyckel och värden för frågeparametrar|  
|WebDocumentationTemplatesDeveloperKey|Ange nyckel för din prenumeration|  
|WebDocumentationTemplatesJavaApache|Det här exemplet använder hello Apache HTTP-klient från http-komponenter (http://hc.apache.org/httpcomponents-client-ga/)|  
|WebDocumentationTemplatesOptionalParams|Ange värden för valfria parametrar, vid behov|  
|WebDocumentationTemplatesPhpPackage|Det här exemplet använder hello HTTP_Request2 paket. (Mer information: http://pear.php.net/package/HTTP_Request2)|  
|WebDocumentationTemplatesPythonValuesForPath|Ange värden för parametrarna (visas som {...}) och begäran om det behövs|  
|WebDocumentationTemplatesRequestBody|Ange text för begäran|  
|WebDocumentationTemplatesRequiredParams|Ange värden för följande parametrar hello|  
|WebDocumentationTemplatesValuesForPath|Ange värden för parametrarna (visas som {...})|  
|OAuth2AuthorizationEndpointDescription|Hämta ett authorization grant hello autentiseringsslutpunkt eftersom det är används toointeract med hello resurs-ägare.|  
|OAuth2AuthorizationEndpointName|autentiseringsslutpunkt|  
|OAuth2TokenEndpointDescription|Hej tokenslutpunkten används av hello klienten tooobtain en åtkomst-token genom att presentera tillstånd bevilja eller uppdatera token.|  
|OAuth2TokenEndpointName|token för slutpunkt|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Description|< p\> hello klienten initierar hello flödet genom att dirigera hello resursägare användaragent toohello autentiseringsslutpunkt.  hello klienten innehåller dess klient-ID, begärda omfånget, lokala tillstånd och en omdirigering URI toowhich hello auktorisering server skickar hello användaragent tillbaka när åtkomst beviljas (eller nekas).     < /p\> < p\> hello auktorisering servern autentiserar hello resursägare (via hello användaragent) och upprättar om hello resursägaren beviljas eller nekas hello klienten åtkomst-begäran.     < /p\> < p\> under förutsättning att hello resursägaren beviljar åtkomst, hello auktorisering servern omdirigerar hello användaragent tillbaka toohello klienten med hjälp av hello omdirigerings-URI som angavs tidigare (hello begäran eller enligt klien t registrering).  hello omdirigerings-URI innehåller en Auktoriseringskoden och några lokala tillstånd som tillhandahålls av hello klienten tidigare.     < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ErrorDescription|< p\> om hello användaren nekar hello åtkomstbegäran av om hello förfrågan är ogiltig, hello klienten informeras med följande parametrar som lagts till på toohello omdirigering hello: < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Name|Begäran om godkännande|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_RequestDescription|< p\> hello-klientappen måste skicka hello användaren toohello autentiseringsslutpunkt i ordning tooinitiate hello OAuth-processen.          Vid hello autentiseringsslutpunkt autentiserar hello användare och sedan beviljar eller nekar åtkomst toohello app.     < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ResponseDescription|< p\> under förutsättning att hello resursägaren beviljar åtkomst, auktorisering servern omdirigerar hello användaragent tillbaka toohello klienten med hjälp av hello omdirigerings-URI som angavs tidigare (hello begäran eller under registreringen).  hello omdirigerings-URI innehåller en Auktoriseringskoden och några lokala tillstånd som tillhandahålls av hello klienten tidigare. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_Description|< p\> hello klienten begär en åtkomst-token från hello auktorisering server'' s tokenslutpunkten genom att inkludera hello auktoriseringskod togs emot i hello föregående steg.  När du gör hello begäran autentiserar hello klienten med hello auktorisering server.  hello klienten innehåller hello omdirigering URI som används tooobtain hello auktoriseringskod för verifiering. < /p\> < p\> hello auktorisering server autentiserar hello klient, verifierar hello auktoriseringskod och säkerställer att hello omdirigerings-URI togs emot matchar hello URI används tooredirect hello klienten i steg C.  Om det är giltigt svarar hello auktorisering servern igen med en åtkomst-token och eventuellt en uppdateringstoken. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ErrorDescription|< p\> om hello klientautentisering för begäran inte kunde eller är ogiltig, hello auktorisering servern svarar med en statuskod HTTP 400 (felaktig begäran) (om inget annat anges) och innehåller följande parametrar med hello svar hello. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_RequestDescription|< p\> hello klienten skickar en begäran toohello tokenslutpunkten genom att skicka hello följande parametrar formatet hello ”application/x-www-formuläret-urlencoded” med en teckenkodningen UTF-8 i hello HTTP-begäran huvuddelen. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ResponseDescription|< p\> hello auktorisering server utfärdar en åtkomst-token och valfritt uppdateringstoken och konstruktioner hello svar genom att lägga till hello följande parametrar toohello huvuddelen av hello HTTP-svar med en kod 200 (OK) status. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_Description|< p\> hello klienten autentiserar med hello auktorisering server och begär en åtkomst-token från hello-token för slutpunkt. < /p\> < p\> hello auktorisering servern autentiserar hello klienten och om det är giltigt utfärdar en åtkomst-token. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> om hello-begäran är ogiltig eller misslyckades klientautentisering hello auktorisering servern svarar med en statuskod HTTP 400 (felaktig begäran) (om inget annat anges) och innehåller följande parametrar med hello svar hello. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> hello klienten skickar en begäran toohello tokenslutpunkten genom att lägga till följande parametrar formatet hello ”application/x-www-formuläret-urlencoded” med en teckenkodningen UTF-8 i hello HTTP-begäran huvuddelen hello. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> om begäran hello-åtkomst-token är giltig och auktoriserade hello auktorisering utfärdar en åtkomst-token och valfritt uppdateringstoken och konstruktioner hello svar genom att lägga till följande parametrar toohello huvuddelen av hello HTTP hello svar med en kod 200 (OK) status. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_Description|< p\> hello klienten initierar hello flödet genom att dirigera hello resursägare '' s användaragent toohello autentiseringsslutpunkt.  hello klienten innehåller dess klient-ID, begärda omfånget, lokala tillstånd och en omdirigering URI toowhich hello auktorisering server skickar hello användaragent tillbaka när åtkomst beviljas (eller nekas). < /p\> < p\> hello auktorisering servern autentiserar hello resursägare (via hello användaragent) och upprättar om hello resursägaren beviljas eller nekas hello klienten '' s åtkomstbegäran. < /p\> < p\> under förutsättning att hello resursägaren beviljar åtkomst, hello auktorisering servern dirigerar hello användaragent tillbaka toohello klienten med hjälp av hello omdirigering URI som tidigare.  hello omdirigerings-URI innehåller hello åtkomst-token i hello URI-fragment. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ErrorDescription|< p\> om hello resursägare nekar hello åtkomstbegäran eller om hello begäran misslyckas av andra orsaker än en saknad eller ogiltig omdirigerings-URI, hello auktorisering server informerar hello-klienten genom att lägga till följande parametrar toohello fragme hello NT-komponenten i hello omdirigering URI med hello ”application/x-www-formuläret-urlencoded” format. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_RequestDescription|< p\> hello-klientappen måste skicka hello användaren toohello autentiseringsslutpunkt i ordning tooinitiate hello OAuth-processen.      Vid hello autentiseringsslutpunkt autentiserar hello användare och sedan beviljar eller nekar åtkomst toohello app. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ResponseDescription|< p\> om hello resursägaren beviljar hello åtkomstbegäran, hello auktorisering servern utfärdar en åtkomst-token och levererar toohello klienten genom att lägga till följande parametrar toohello fragment-komponent av hello omdirigerings-URI hello med hello ”a pplication/x-www-formuläret-urlencoded ”-format. < /p\>|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Description|Auktoriseringskodflödet är optimerad för klienter som kan hantera hello sekretess för sina autentiseringsuppgifter (t.ex. program för webbservrar implementeras med hjälp av PHP, Java, Python, Ruby, ASP.NET, etc.).|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Name|Auktorisering kod bevilja|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Description|Klientautentiseringsuppgifter är lämpligt i fall där hello klienten (programmet) begär åtkomst toohello skyddade resurser under dess kontroll. hello klienten anses som en resursägare, så ingen användarinteraktion krävs.|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Name|Klientens autentiseringsuppgifter bevilja|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Description|Implicita flödet är optimerad för klienter som inte klarar av att upprätthålla hello sekretess för sina autentiseringsuppgifter kända toooperate för en viss omdirigerings-URI. Dessa klienter implementeras normalt i en webbläsare som använder ett skriptspråk som JavaScript.|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Name|Implicit bevilja|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Description|Resurs ägare lösenord autentiseringsuppgifter flödet är lämpligt i fall där hello resursägare har en förtroenderelation med hello-klienten (programmet), till exempel hello enhetens operativsystem eller ett mycket Privilegierade program. Detta flöde är lämplig för klienter som kan hämta hello resurs ägarens autentiseringsuppgifter (användarnamn och lösenord, vanligtvis med ett interaktivt formulär).|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Name|Bevilja lösenordsinformation för resurs-ägare|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_Description|< p\> hello resursägare tillhandahåller hello med sitt användarnamn och lösenord. < /p\> < p\> hello klienten begär en åtkomst-token från hello auktorisering server'' s tokenslutpunkten genom att inkludera hello autentiseringsuppgifter togs emot från hello resurs-ägare.  När du gör hello begäran autentiserar hello klienten med hello auktorisering server. < /p\> < p\> hello auktorisering servern autentiserar hello klienten validerar hello resursautentiseringsuppgifterna ägare och utfärdar en åtkomst-token om det är giltigt. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> om hello-begäran är ogiltig eller misslyckades klientautentisering hello auktorisering servern svarar med en statuskod HTTP 400 (felaktig begäran) (om inget annat anges) och innehåller följande parametrar med hello svar hello. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> hello klienten skickar en begäran toohello tokenslutpunkten genom att lägga till följande parametrar formatet hello ”application/x-www-formuläret-urlencoded” med en teckenkodningen UTF-8 i hello HTTP-begäran huvuddelen hello. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> om begäran hello-åtkomst-token är giltig och auktoriserade hello auktorisering utfärdar en åtkomst-token och valfritt uppdateringstoken och konstruktioner hello svar genom att lägga till följande parametrar toohello huvuddelen av hello HTTP respo hello nSe med en kod 200 (OK) status. < /p\>|  
|OAuth2Step_AccessTokenRequest_Name|Begäran för åtkomst-token|  
|OAuth2Step_AuthorizationRequest_Name|Begäran om godkännande|  
|OAuth2AccessToken_AuthorizationCodeGrant_TokenResponse|KRÄVS. hello åtkomsttoken har utfärdats av hello auktorisering server.|  
|OAuth2AccessToken_ClientCredentialsGrant_TokenResponse|KRÄVS. hello åtkomsttoken har utfärdats av hello auktorisering server.|  
|OAuth2AccessToken_ImplicitGrant_AuthorizationResponse|KRÄVS. hello åtkomsttoken har utfärdats av hello auktorisering server.|  
|OAuth2AccessToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|KRÄVS. hello åtkomsttoken har utfärdats av hello auktorisering server.|  
|OAuth2ClientId_AuthorizationCodeGrant_AuthorizationRequest|KRÄVS. Klient-ID.|  
|OAuth2ClientId_AuthorizationCodeGrant_TokenRequest|KRÄVS om hello klienten inte autentiseras med hello auktorisering server.|  
|OAuth2ClientId_ImplicitGrant_AuthorizationRequest|KRÄVS. hello klient-ID.|  
|OAuth2Code_AuthorizationCodeGrant_AuthorizationResponse|KRÄVS. hello auktoriseringskod genereras av hello auktorisering server.|  
|OAuth2Code_AuthorizationCodeGrant_TokenRequest|KRÄVS. Hej Auktoriseringskoden togs emot från hello auktorisering servern.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_AuthorizationErrorResponse|VALFRITT. Läsbart ASCII-text som ger ytterligare information.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_TokenErrorResponse|VALFRITT. Läsbart ASCII-text som ger ytterligare information.|  
|OAuth2ErrorDescription_ClientCredentialsGrant_TokenErrorResponse|VALFRITT. Läsbart ASCII-text som ger ytterligare information.|  
|OAuth2ErrorDescription_ImplicitGrant_AuthorizationErrorResponse|VALFRITT. Läsbart ASCII-text som ger ytterligare information.|  
|OAuth2ErrorDescription_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|VALFRITT. Läsbart ASCII-text som ger ytterligare information.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_AuthorizationErrorResponse|VALFRITT. En URI som identifierar en läsbar sida med information om hello felet.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_TokenErrorResponse|VALFRITT. En URI som identifierar en läsbar sida med information om hello felet.|  
|OAuth2ErrorUri_ClientCredentialsGrant_TokenErrorResponse|VALFRITT. En URI som identifierar en läsbar sida med information om hello felet.|  
|OAuth2ErrorUri_ImplicitGrant_AuthorizationErrorResponse|VALFRITT. En URI som identifierar en läsbar sida med information om hello felet.|  
|OAuth2ErrorUri_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|VALFRITT. En URI som identifierar en läsbar sida med information om hello felet.|  
|OAuth2Error_AuthorizationCodeGrant_AuthorizationErrorResponse|KRÄVS. En enda ASCII-felkod från hello följande: invalid_request, unauthorized_client, om access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_AuthorizationCodeGrant_TokenErrorResponse|KRÄVS. En enda ASCII-felkod från hello följande: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ClientCredentialsGrant_TokenErrorResponse|KRÄVS. En enda ASCII-felkod från hello följande: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ImplicitGrant_AuthorizationErrorResponse|KRÄVS. En enda ASCII-felkod från hello följande: invalid_request, unauthorized_client, om access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|KRÄVS. En enda ASCII-felkod från hello följande: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2ExpiresIn_AuthorizationCodeGrant_TokenResponse|REKOMMENDERAS. hello livstid i sekunder för hello åtkomst-token.|  
|OAuth2ExpiresIn_ClientCredentialsGrant_TokenResponse|REKOMMENDERAS. hello livstid i sekunder för hello åtkomst-token.|  
|OAuth2ExpiresIn_ImplicitGrant_AuthorizationResponse|REKOMMENDERAS. hello livstid i sekunder för hello åtkomst-token.|  
|OAuth2ExpiresIn_ResourceOwnerPasswordCredentialsGrant_TokenResponse|REKOMMENDERAS. hello livstid i sekunder för hello åtkomst-token.|  
|OAuth2GrantType_AuthorizationCodeGrant_TokenRequest|KRÄVS. Värdet måste anges för ”authorization_code”.|  
|OAuth2GrantType_ClientCredentialsGrant_TokenRequest|KRÄVS. Värdet måste anges för ”client_credentials”.|  
|OAuth2GrantType_ResourceOwnerPasswordCredentialsGrant_TokenRequest|KRÄVS. Värdet måste anges för ”password”.|  
|OAuth2Password_ResourceOwnerPasswordCredentialsGrant_TokenRequest|KRÄVS. hello resurs ägarlösenord.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_AuthorizationRequest|VALFRITT. hello omdirigering av slutpunkten URI måste vara en absolut URI.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_TokenRequest|KRÄVS om hello ”redirect_uri” parametern ingick i hello auktoriseringsbegäran och deras värden måste vara identiska.|  
|OAuth2RedirectUri_ImplicitGrant_AuthorizationRequest|VALFRITT. hello omdirigering av slutpunkten URI måste vara en absolut URI.|  
|OAuth2RefreshToken_AuthorizationCodeGrant_TokenResponse|VALFRITT. Hej uppdateringstoken som kan vara används tooobtain ny åtkomsttoken.|  
|OAuth2RefreshToken_ClientCredentialsGrant_TokenResponse|VALFRITT. Hej uppdateringstoken som kan vara används tooobtain ny åtkomsttoken.|  
|OAuth2RefreshToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|VALFRITT. Hej uppdateringstoken som kan vara används tooobtain ny åtkomsttoken.|  
|OAuth2ResponseType_AuthorizationCodeGrant_AuthorizationRequest|KRÄVS. Värdet måste anges för ”kod”.|  
|OAuth2ResponseType_ImplicitGrant_AuthorizationRequest|KRÄVS. Värdet måste anges för ”token”.|  
|OAuth2Scope_AuthorizationCodeGrant_AuthorizationRequest|VALFRITT. hello omfattning hello åtkomstbegäran.|  
|OAuth2Scope_AuthorizationCodeGrant_TokenResponse|VALFRIA om identiska toohello scope har begärt hello-klienten. i annat fall krävs.|  
|OAuth2Scope_ClientCredentialsGrant_TokenRequest|VALFRITT. hello omfattning hello åtkomstbegäran.|  
|OAuth2Scope_ClientCredentialsGrant_TokenResponse|Om identiska toohello scope som begärs av klienten hello. i annat fall krävs.|  
|OAuth2Scope_ImplicitGrant_AuthorizationRequest|VALFRITT. hello omfattning hello åtkomstbegäran.|  
|OAuth2Scope_ImplicitGrant_AuthorizationResponse|VALFRIA om identiska toohello scope har begärt hello-klienten. i annat fall krävs.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenRequest|VALFRITT. hello omfattning hello åtkomstbegäran.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenResponse|Om identiska toohello scope som begärs av klienten hello. i annat fall krävs.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationErrorResponse|KRÄVS om hello ”tillstånd” parametern fanns i hello klientbegäran för auktorisering.  hello exakt värde togs emot från hello-klienten.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationRequest|REKOMMENDERAS. En täckande värde som används av hello toomaintain klientstatus mellan hello begäran och återanrop.  hello auktorisering server inkluderar det här värdet när omdirigera hello användaragent tillbaka toohello klienten.  hello-parametern ska användas för att förhindra förfalskning av begäran.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationResponse|KRÄVS om hello ”tillstånd” parametern fanns i hello klientbegäran för auktorisering.  hello exakt värde togs emot från hello-klienten.|  
|OAuth2State_ImplicitGrant_AuthorizationErrorResponse|KRÄVS om hello ”tillstånd” parametern fanns i hello klientbegäran för auktorisering.  hello exakt värde togs emot från hello-klienten.|  
|OAuth2State_ImplicitGrant_AuthorizationRequest|REKOMMENDERAS. En täckande värde som används av hello toomaintain klientstatus mellan hello begäran och återanrop.  hello auktorisering server inkluderar det här värdet när omdirigera hello användaragent tillbaka toohello klienten.  hello-parametern ska användas för att förhindra förfalskning av begäran.|  
|OAuth2State_ImplicitGrant_AuthorizationResponse|KRÄVS om hello ”tillstånd” parametern fanns i hello klientbegäran för auktorisering.  hello exakt värde togs emot från hello-klienten.|  
|OAuth2TokenType_AuthorizationCodeGrant_TokenResponse|KRÄVS. hello typ av hello-token som utfärdas.|  
|OAuth2TokenType_ClientCredentialsGrant_TokenResponse|KRÄVS. hello typ av hello-token som utfärdas.|  
|OAuth2TokenType_ImplicitGrant_AuthorizationResponse|KRÄVS. hello typ av hello-token som utfärdas.|  
|OAuth2TokenType_ResourceOwnerPasswordCredentialsGrant_TokenResponse|KRÄVS. hello typ av hello-token som utfärdas.|  
|OAuth2UserName_ResourceOwnerPasswordCredentialsGrant_TokenRequest|KRÄVS. hello resurs ägare användarnamnet.|  
|OAuth2UnsupportedTokenType|Tokentyp: {0} är inte supporetd.|  
|OAuth2InvalidState|Ogiltigt svar från servern för auktorisering|  
|OAuth2GrantType_AuthorizationCode|Auktoriseringskoden|  
|OAuth2GrantType_Implicit|Implicit|  
|OAuth2GrantType_ClientCredentials|Klientens autentiseringsuppgifter|  
|OAuth2GrantType_ResourceOwnerPassword|Resurs-ägarlösenord|  
|WebDocumentation302Code|302 hittades|  
|WebDocumentation400Code|400 (felaktig begäran)|  
|OAuth2SendingMethod_AuthHeader|Authorization-huvud|  
|OAuth2SendingMethod_QueryParam|Frågeparametern|  
|OAuth2AuthorizationServerGeneralException|Ett fel uppstod när auktorisera åtkomst via {0}|  
|OAuth2AuthorizationServerCommunicationException|Det gick inte att upprätta en HTTP-anslutning tooauthorization server eller den har stängts oväntat.|  
|WebDocumentationOAuth2GeneralErrorMessage|Oväntat fel uppstod.|  
|AuthorizationServerCommunicationException|Auktorisering server kommunikation undantag har inträffat. Kontakta administratören.|  
|TextblockSubscriptionKeyHeaderDescription|Prenumerationen nyckel som ger åtkomst toothis API. I din < en href ='/ utvecklare '\>profil < /a\>.|  
|TextblockOAuthHeaderDescription|OAuth 2.0-åtkomsttoken har hämtats från < jag\>{0} < /i\>. Bevilja typer som stöds: < jag\>{1} < /i\>.|  
|TextblockContentTypeHeaderDescription|Medietyp av hello text skickas toohello API.|  
|ErrorMessageApiNotAccessible|hello API som du försöker toocall är inte tillgänglig just nu. Kontakta hello API publisher < en href = ”/ utfärdar”\>här < /a\>.|  
|ErrorMessageApiTimedout|hello API som du försöker toocall tar längre tid än normalt tooget svar tillbaka. Kontakta hello API publisher < en href = ”/ utfärdar”\>här < /a\>.|  
|BadRequestParameterExpected|”{0}-parametern för startdiskvolymen”|  
|TooltipTextDoubleClickToSelectAll|Dubbelklicka på alla tooselect.|  
|TooltipTextHideRevealSecret|Visa/Dölj|  
|ButtonLinkOpenConsole|Prova|  
|SectionHeadingRequestBody|Begärandetexten|  
|SectionHeadingRequestParameters|Parametrar för begäran|  
|SectionHeadingRequestUrl|URL-begäran|  
|SectionHeadingResponse|Svar|  
|SectionHeadingRequestHeaders|Huvuden för begäran|  
|FormLabelSubtextOptional|Valfria|  
|SectionHeadingCodeSamples|Kodexempel|  
|TextblockOpenidConnectHeaderDescription|OpenID Connect-ID-token hämtades från < jag\>{0} < /i\>. Bevilja typer som stöds: < jag\>{1} < /i\>.|  
  
###  <a name="ErrorPageStrings"></a>ErrorPageStrings  
  
|Namn|Text|  
|----------|----------|  
|LinkLabelBack|Säkerhetskopiera|  
|LinkLabelHomePage|Startsida|  
|LinkLabelSendUsEmail|Skicka ett e-postmeddelande|  
|PageTitleError|Tyvärr uppstod ett problem betjänar hello begärd sida|  
|TextblockPotentialCauseIntermittentIssue|Detta kan vara ett tillfälligt data access-problem som redan är borta.|  
|TextblockPotentialCauseOldLink|hello länken som du har klickat på kan vara gamla och inte punkt toohello korrigera plats längre.|  
|TextblockPotentialCauseTechnicalProblem|Det kan finnas ett tekniskt problem hos oss.|  
|TextblockPotentialSolutionRefresh|Försök att uppdatera hello-sidan.|  
|TextblockPotentialSolutionStartOver|Börja om från vår {0}.|  
|TextblockPotentialSolutionTryAgain|Gå {0} och försök hello instruktionen igen.|  
|TextReportProblem|{0} som beskriver vad som gick fel och vi titta på det som vi kan.|  
|TitlePotentialCause|Möjlig orsak|  
|TitlePotentialSolution|Det är eventuellt ett tillfälligt problem, några saker tootry|  
  
###  <a name="IssuesStrings"></a>IssuesStrings  
  
|Namn|Text|  
|----------|----------|  
|WebIssuesIndexTitle|Problem|  
|WebIssuesNoActiveSubscriptions|Du har inga aktiva prenumerationer. Du behöver toosubscribe för en produkt tooreport ett problem.|  
|WebIssuesNotSignin|Du är inte inloggad. Ange {0} tooreport ett problem eller efter en kommentar.|  
|WebIssuesReportIssueButton|Rapporten problemet|  
|WebIssuesSignIn|logga in|  
|WebIssuesStatusReportedBy|Status: {0} &#124; Rapporteras av {1}|  
  
###  <a name="NotFoundStrings"></a>NotFoundStrings  
  
|Namn|Text|  
|----------|----------|  
|LinkLabelHomePage|Startsida|  
|LinkLabelSendUsEmail|Skicka ett e-postmeddelande|  
|PageTitleNotFound|Det går inte att hitta hello-sidan som du söker efter|  
|TextblockPotentialCauseMisspelledUrl|Du kan vara felstavat hello URL Om du har angett den i.|  
|TextblockPotentialCauseOldLink|hello länken som du har klickat på kan vara gamla och inte punkt toohello korrigera plats längre.|  
|TextblockPotentialSolutionRetype|Försök att skriva in hello-URL.|  
|TextblockPotentialSolutionStartOver|Börja om från vår {0}.|  
|TextReportProblem|{0} som beskriver vad som gick fel och vi titta på det som vi kan.|  
|TitlePotentialCause|Möjlig orsak|  
|TitlePotentialSolution|Möjlig lösning|  
  
###  <a name="ProductDetailsStrings"></a>ProductDetailsStrings  
  
|Namn|Text|  
|----------|----------|  
|WebProductsAgreement|Genom att prenumerera för {0} produkt, acceptera toohello `<a data-toggle='modal' href='#legal-terms'\>Terms of Use</a\>`.|  
|WebProductsLegalTermsLink|Användningsvillkor|  
|WebProductsSubscribeButton|Prenumerera|  
|WebProductsUsageLimitsHeader|Gränserna för Resursanvändning|  
|WebProductsYouAreNotSubscribed|Är du prenumererar på toothis produkten.|  
|WebProductsYouRequestedSubscription|Du har begärt prenumeration toothis produkten.|  
|ErrorYouNeedtoAgreeWithLegalTerms|Innan du kan fortsätta måste du godkänna toohello användningsvillkoren.|  
|ButtonLabelAddSubscription|Lägg till en prenumeration|  
|LinkLabelChangeSubscriptionName|Ändra|  
|ButtonLabelConfirm|Bekräfta|  
|TextblockMultipleSubscriptionsCount|Du har {0} prenumerationer toothis produkten:|  
|TextblockSingleSubscriptionsCount|Du har {0} prenumeration toothis produkten:|  
|TextblockSingleApisCount|Den här produkten innehåller {0}-API:|  
|TextblockMultipleApisCount|Den här produkten innehåller {0} API: er:|  
|TextblockHeaderSubscribe|Prenumerera på tooproduct|  
|TextblockSubscriptionDescription|En ny prenumeration skapas på följande sätt:|  
|TextblockSubscriptionLimitReached|Prenumerationer har nåtts.|  
  
###  <a name="ProductsStrings"></a>ProductsStrings  
  
|Namn|Text|  
|----------|----------|  
|PageTitleProducts|Produkter|  
  
###  <a name="ProviderInfoStrings"></a>ProviderInfoStrings  
  
|Namn|Text|  
|----------|----------|  
|TextboxExternalIdentitiesDisabled|Inloggning har inaktiverats av hello administratörer hello tillfället.|  
|TextboxExternalIdentitiesSigninInvitation|Du kan också logga in med|  
|TextboxExternalIdentitiesSigninInvitationPrimary|Logga in med:|  
  
###  <a name="SigninResources"></a>SigninResources  
  
|Namn|Text|  
|----------|----------|  
|PrincipalNotFound|Det gick inte att hitta Principal eller signaturen är ogiltig|  
|ErrorSsoAuthenticationFailed|SSO-autentiseringen misslyckades|  
|ErrorSsoAuthenticationFailedDetailed|Ogiltig token som tillhandahölls eller en signatur kan inte verifieras.|  
|ErrorSsoTokenInvalid|SSO-token är ogiltig|  
|ValidationErrorSpecificEmailAlreadyExists|E-post: {0} har redan registrerats|  
|ValidationErrorSpecificEmailInvalid|E-post: {0} är ogiltig|  
|ValidationErrorPasswordInvalid|Lösenordet är ogiltigt. Korrigera hello fel och försök igen.|  
|PropertyTooShort|{0} är för kort|  
|WebAuthenticationAddresserEmailInvalidErrorMessage|Ogiltig e-postadress.|  
|ValidationMessageNewPasswordConfirmationRequired|Bekräfta nytt lösenord|  
|ValidationErrorPasswordConfirmationRequired|Bekräfta lösenordet är tomt|  
|WebAuthenticationEmailChangeNotice|Ändra e-postbekräftelse finns på hello alldeles för {0}. Följ anvisningarna i den tooconfirm din nya e-postadress. Om hello e-post inte kommer till tooyour inkorg i hello nästa några minuter, kontrollera mappen för skräppost.|  
|WebAuthenticationEmailChangeNoticeHeader|Din e-begäran bearbetades|  
|WebAuthenticationEmailChangeNoticeTitle|E-ändring har begärts|  
|WebAuthenticationEmailHasBeenRevertedNotice|Det finns redan en du e-post. Begäran har återställts|  
|ValidationErrorEmailAlreadyExists|Det finns redan en e-post|  
|ValidationErrorEmailInvalid|Ogiltig e-postadress|  
|TextboxLabelEmail|E-post|  
|ValidationErrorEmailRequired|E-post måste anges.|  
|WebAuthenticationErrorNoticeHeader|Fel|  
|WebAuthenticationFieldLengthErrorMessage|{0} måste vara en maximal längd på {1}|  
|TextboxLabelEmailFirstName|Förnamn|  
|ValidationErrorFirstNameRequired|Förnamn måste anges.|  
|ValidationErrorFirstNameInvalid|Ogiltigt förnamn|  
|NoticeInvalidInvitationToken|Observera att bekräftelse länkar är giltiga för endast 48 timmar. Om du fortfarande inom denna period, kontrollera att länken är korrekt. Om länken har gått ut kan du upprepa sedan hello åtgärd du försöker tooconfirm.|  
|NoticeHeaderInvalidInvitationToken|Ogiltig inbjudan token|  
|NoticeTitleInvalidInvitationToken|Bekräftelse-fel|  
|WebAuthenticationLastNameInvalidErrorMessage|Ogiltigt efternamn|  
|TextboxLabelEmailLastName|Efternamn|  
|ValidationErrorLastNameRequired|Efternamn måste anges.|  
|WebAuthenticationLinkExpiredNotice|Bekräftelse länk som har skickats tooyou har gått ut. `<a href={0}?token={1}>Resend confirmation email.</a\>`|  
|NoticePasswordResetLinkInvalidOrExpired|Din länk för lösenordsåterställning är ogiltigt eller har upphört att gälla.|  
|WebAuthenticationLinkExpiredNoticeTitle|Länk som har skickats|  
|WebAuthenticationNewPasswordLabel|Nytt lösenord|  
|ValidationMessageNewPasswordRequired|Nytt lösenord krävs.|  
|TextboxLabelNotificationsSenderEmail|Meddelanden avsändarens e-post|  
|TextboxLabelOrganizationName|Organisationens namn|  
|WebAuthenticationOrganizationRequiredErrorMessage|Organisationens namn är tomt|  
|WebAuthenticationPasswordChangedNotice|Ditt lösenord har uppdaterats|  
|WebAuthenticationPasswordChangedNoticeTitle|Lösenordet uppdateras|  
|WebAuthenticationPasswordCompareErrorMessage|Lösenorden stämmer inte överens|  
|WebAuthenticationPasswordConfirmLabel|Bekräfta lösenord|  
|ValidationErrorPasswordInvalidDetailed|Lösenordet är för svag.|  
|WebAuthenticationPasswordLabel|Lösenord|  
|ValidationErrorPasswordRequired|Lösenord krävs.|  
|WebAuthenticationPasswordResetSendNotice|Ändra lösenord e-postbekräftelse finns på hello alldeles för {0}. Följ hello instruktioner inom hello e-toocontinue processen för ändring av lösenord.|  
|WebAuthenticationPasswordResetSendNoticeHeader|Din begäran om lösenordsåterställning bearbetat har|  
|WebAuthenticationPasswordResetSendNoticeTitle|Lösenordsåterställning begärdes|  
|WebAuthenticationRequestNotFoundNotice|Begäran hittades inte|  
|WebAuthenticationSenderEmailRequiredErrorMessage|Meddelanden avsändarens e-post är tom|  
|WebAuthenticationSigninPasswordLabel|Bekräfta hello ändringen genom att ange ett lösenord|  
|WebAuthenticationSignupConfirmNotice|Registrering av e-postbekräftelse är på väg för {0}. < br /\> Kontrollera Följ instruktionerna i hello e-tooactivate ditt konto. < br /\> om hello e-post inte tas emot i Inkorgen i hello nästa några minuter, kontrollera mappen för skräppost.|  
|WebAuthenticationSignupConfirmNoticeHeader|Ditt konto har skapats|  
|WebAuthenticationSignupConfirmNoticeRepeatHeader|Registrering bekräftelse e-postmeddelandet skickades igen|  
|WebAuthenticationSignupConfirmNoticeTitle|Konto som har skapats|  
|WebAuthenticationTokenRequiredErrorMessage|Token är tom|  
|WebAuthenticationUserAlreadyRegisteredNotice|Det verkar vara en användare med den här e-post är redan registrerad i hello system. Om du har glömt ditt lösenord försök toorestore den eller Kontakta supportteamet.|  
|WebAuthenticationUserAlreadyRegisteredNoticeHeader|Användaren har redan registrerats|  
|WebAuthenticationUserAlreadyRegisteredNoticeTitle|Redan registrerats|  
|ButtonLabelChangePassword|Ändra lösenord|  
|ButtonLabelChangeAccountInfo|Ändra kontoinformation|  
|ButtonLabelCloseAccount|Avsluta konto|  
|WebAuthenticationInvalidCaptchaErrorMessage|Texten som angetts matchar inte text på hello bild. Försök igen.|  
|ValidationErrorCredentialsInvalid|E-post eller lösenordet är ogiltigt. Korrigera hello fel och försök igen.|  
|WebAuthenticationRequestIsNotValid|Begäran är inte giltig|  
|WebAuthenticationUserIsNotConfirm|Bekräfta att registreringen innan du försöker toosign i.|  
|WebAuthenticationInvalidEmailFormated|E-post är ogiltig: {0}|  
|WebAuthenticationUserNotFound|Användaren hittades inte|  
|WebAuthenticationTenantNotRegistered|Ditt konto hör tooa Azure Active Directory-klient som inte är auktoriserade tooaccess den här portalen.|  
|WebAuthenticationAuthenticationFailed|Autentiseringen misslyckades.|  
|WebAuthenticationGooglePlusNotEnabled|Autentiseringen misslyckades. Om du har behörighet hello programmet sedan kontakta hello admin toomake är Kontrollera att Google-autentisering korrekt konfigurerad.|  
|ValidationErrorAllowedTenantIsRequired|Tillåtna klient krävs|  
|ValidationErrorTenantIsNotValid|hello Azure Active Directory-klient: {0} är inte giltig.|  
|WebAuthenticationActiveDirectoryTitle|Azure Active Directory|  
|WebAuthenticationLoginUsingYourProvider|Logga in med ditt {0}|  
|WebAuthenticationUserLimitNotice|Den här tjänsten har nått hello maximalt antal tillåtna användare. Kontrollera `<a href="mailto:{0}"\>contact hello administrator</a\>` tooupgrade sina service och återaktivera användarregistrering.|  
|WebAuthenticationUserLimitNoticeHeader|Användarregistrering inaktiverad|  
|WebAuthenticationUserLimitNoticeTitle|Användarregistrering inaktiverad|  
|WebAuthenticationUserRegistrationDisabledNotice|Registrering av användare har inaktiverats av Hej administratör. Logga in med externa identitetsprovider.|  
|WebAuthenticationUserRegistrationDisabledNoticeHeader|Användarregistrering inaktiverad|  
|WebAuthenticationUserRegistrationDisabledNoticeTitle|Användarregistrering inaktiverad|  
|WebAuthenticationSignupPendingConfirmationNotice|Innan vi kan slutföra hello skapandet av ditt konto behöver vi tooverify din e-postadress. Vi har skickat ett e-postmeddelande för {0}. Följ hello instruktioner i hello e-tooactivate ditt konto. Om hello e-post inte inom hello kommer till nästa några minuter, kontrollera mappen för skräppost.|  
|WebAuthenticationSignupPendingConfirmationAccountFoundNotice|Ett obekräftat konto påträffades för hello e-postadress {0}. toocomplete hello skapa ditt konto behöver vi tooverify din e-postadress. Vi har skickat ett e-postmeddelande för {0}. Följ hello instruktioner i hello e-tooactivate ditt konto. Om hello e-post inte inom hello kommer till nästa några minuter, kontrollera mappen för skräppost|  
|WebAuthenticationSignupConfirmationAlmostDone|Nästan klar|  
|WebAuthenticationSignupConfirmationEmailSent|Vi har skickat ett e-postmeddelande för {0}. Följ hello instruktioner i hello e-tooactivate ditt konto. Om hello e-post inte inom hello kommer till nästa några minuter, kontrollera mappen för skräppost.|  
|WebAuthenticationEmailSentNotificationMessage|E-postmeddelandet som skickats för {0}|  
|WebAuthenticationNoAadTenantConfigured|Ingen Azure Active Directory-klient som konfigurerats för hello-tjänsten.|  
|CheckboxLabelUserRegistrationTermsConsentRequired|Jag accepterar toohello `<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use</a\>`.|  
|TextblockUserRegistrationTermsProvided|Granska`<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use.</a\>`|  
|DialogHeadingTermsOfUse|Användningsvillkor|  
|ValidationMessageConsentNotAccepted|Innan du kan fortsätta måste du godkänna toohello användningsvillkoren.|  
  
###  <a name="SigninStrings"></a>SigninStrings  
  
|Namn|Text|  
|----------|----------|  
|WebAuthenticationForgotPassword|Glömt ditt lösenord?|  
|WebAuthenticationIfAdministrator|Om du är administratör måste du logga in `<a href="{0}"\>here</a\>`.|  
|WebAuthenticationNotAMember|Inte en medlem ännu? `<a href="/signup"\>Sign up now</a\>`|  
|WebAuthenticationRemember|Kom ihåg mig på den här datorn|  
|WebAuthenticationSigininWithPassword|Logga in med ditt användarnamn och lösenord|  
|WebAuthenticationSigninTitle|Logga in|  
|WebAuthenticationSignUpNow|Registrera dig nu|  
  
###  <a name="SignupStrings"></a>SignupStrings  
  
|Namn|Text|  
|----------|----------|  
|PageTitleSignup|Registrera dig|  
|WebAuthenticationAlreadyAMember|Är du redan medlem?|  
|WebAuthenticationCreateNewAccount|Skapa ett nytt konto för API Management|  
|WebAuthenticationSigninNow|Logga in nu|  
|ButtonLabelSignup|Registrera dig|  
  
###  <a name="SubscriptionListStrings"></a>SubscriptionListStrings  
  
|Namn|Text|  
|----------|----------|  
|SubscriptionCancelConfirmation|Är du säker på att du vill toocancel den här prenumerationen?|  
|SubscriptionRenewConfirmation|Är du säker på att du vill toorenew den här prenumerationen?|  
|WebDevelopersManageSubscriptions|Hantera prenumerationer|  
|WebDevelopersPrimaryKey|Primär nyckel|  
|WebDevelopersRegenerateLink|Återskapa|  
|WebDevelopersSecondaryKey|Sekundärnyckeln|  
|ButtonLabelShowKey|Visa|  
|ButtonLabelRenewSubscription|Förnya|  
|WebDevelopersSubscriptionReqested|Begärs på {0}|  
|WebDevelopersSubscriptionRequestedState|Begärt|  
|WebDevelopersSubscriptionTableNameHeader|Namn|  
|WebDevelopersSubscriptionTableStateHeader|Status|  
|WebDevelopersUsageStatisticsLink|Analytics-rapporter|  
|WebDevelopersYourSubscriptions|Dina prenumerationer|  
|SubscriptionPropertyLabelRequestedDate|Begärs på|  
|SubscriptionPropertyLabelStartedDate|Startats på|  
|PageTitleRenameSubscription|Byt namn på prenumerationen|  
|SubscriptionPropertyLabelName|Prenumerationsnamn|  
  
###  <a name="SubscriptionStrings"></a>SubscriptionStrings  
  
|Namn|Text|  
|----------|----------|  
|SectionHeadingCloseAccount|Söker tooclose ditt konto?|  
|PageTitleDeveloperProfile|Profil|  
|ButtonLabelHideKey|Dölj|  
|ButtonLabelRegenerateKey|Återskapa|  
|InformationMessageKeyWasRegenerated|Är du säker på att du vill tooregenerate den här nyckeln?|  
|ButtonLabelShowKey|Visa|  
  
###  <a name="UpdateProfileStrings"></a>UpdateProfileStrings  
  
|Namn|Text|  
|----------|----------|  
|ButtonLabelUpdateProfile|Uppdatera profilen|  
|PageTitleUpdateProfile|Uppdatera kontoinformation|  
  
###  <a name="UserProfile"></a>Användarprofil  
  
|Namn|Text|  
|----------|----------|  
|ButtonLabelChangeAccountInfo|Ändra kontoinformation|  
|ButtonLabelChangePassword|Ändra lösenord|  
|ButtonLabelCloseAccount|Avsluta konto|  
|TextboxLabelEmail|E-post|  
|TextboxLabelEmailFirstName|Förnamn|  
|TextboxLabelEmailLastName|Efternamn|  
|TextboxLabelNotificationsSenderEmail|Meddelanden avsändarens e-post|  
|TextboxLabelOrganizationName|Organisationens namn|  
|SubscriptionStateActive|Active|  
|SubscriptionStateCancelled|Avbröts|  
|SubscriptionStateExpired|Upphört att gälla|  
|SubscriptionStateRejected|Avvisade|  
|SubscriptionStateRequested|Begärt|  
|SubscriptionStateSuspended|avbruten|  
|DefaultSubscriptionNameTemplate|{0} (standard)|  
|SubscriptionNameTemplate|Utvecklare åtkomst #{0}|  
|TextboxLabelSubscriptionName|Prenumerationsnamn|  
|ValidationMessageSubscriptionNameRequired|Namn får inte vara tomt.|  
|ApiManagementUserLimitReached|Den här tjänsten har nått hello maximalt antal tillåtna användare. Uppgradera tooa högre prisnivå.|  
  
##  <a name="glyphs"></a>Glyf resurser  
 API Management developer portal mallar kan använda hello tecken från [Glyphicons från Bootstrap](http://getbootstrap.com/components/#glyphicons). Den här specialtecken innehåller över 250 tecken i teckensnittsformat från hello [Glyphicon](http://glyphicons.com/) Halflings ange. toouse ett tecken från den här ställa, Använd följande syntax hello.  
  
```html  
<span class="glyphicon glyphicon-user">  
```  
  
 Hello fullständig lista över specialtecken, se [Glyphicons från Bootstrap](http://getbootstrap.com/components/#glyphicons).

## <a name="next-steps"></a>Nästa steg
Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).
