---
title: "aaaUnderstand hello OAuth 2.0-auktoriseringskodflödet i Azure AD | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse HTTP meddelanden tooauthorize åt tooweb program och webb-API: er i din klient med hjälp av Azure Active Directory och OAuth 2.0."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: de3412cb-5fde-4eca-903a-4e9c74db68f2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 4a6fe67d786a5fcb87d1059c2e94ba0c88d26cd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Auktorisera åtkomst tooweb program med hjälp av OAuth 2.0 och Azure Active Directory
Azure Active Directory (AD Azure) använder OAuth 2.0 tooenable du tooauthorize åtkomst tooweb program och webb-API: er i Azure AD-klienten. Den här handboken är språkoberoende och beskriver hur toosend och ta emot HTTP-meddelanden utan att använda någon av våra bibliotek med öppen källkod.

hello OAuth 2.0-auktoriseringskodflödet beskrivs i [avsnittet 4.1 av hello OAuth 2.0-specifikationen](https://tools.ietf.org/html/rfc6749#section-4.1). Det är används tooperform autentisering och auktorisering i de flesta programtyper, inklusive webbappar och internt installerade appar.

[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)]

## Flödet för OAuth 2.0-auktorisering
På en hög nivå hello hela auktorisering flödet för ett program ser ut så här:

![Flödet för OAuth-Auth-kod](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)

## Begära ett auktoriseringskod
hello-auktoriseringskodflödet börjar med hello klienten dirigera hello användaren toohello `/authorize` slutpunkt. I den här förfrågan anger hello-klienten hello behörigheter måste tooacquire från hello användaren. Du kan hämta hello OAuth 2.0-slutpunkter från ditt program sida i klassiska Azure-portalen i hello **visa slutpunkter** knapp i hello nedre lådan.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parameter |  | Beskrivning |
| --- | --- | --- |
| Klient |Krävs |Hej `{tenant}` värdet hello sökvägen till hello begäran kan vara används toocontrol som kan logga in på programmet hello.  hello tillåtna värden är klient-ID: n, till exempel `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` eller `contoso.onmicrosoft.com` eller `common` för klient-oberoende token |
| client_id |Krävs |hello program-Id tilldelade tooyour app när du har registrerat med Azure AD. Du hittar du i hello Azure-portalen. Klicka på **Active Directory**, klicka på hello directory välja hello program och klicka på **konfigurera** |
| response_type |Krävs |Måste innehålla `code` för hello-auktoriseringskodflödet. |
| redirect_uri |Rekommenderas |Hej redirect_uri för din app, där autentisering svar kan skickas och tas emot av din app.  Den måste matcha en hello redirect_uris som du har registrerat i hello portal, förutom det måste vara url-kodade.  För intern & mobila appar, bör du använda hello standardvärdet `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode |Rekommenderas |Anger hello-metod som ska använda toosend hello resulterande token tillbaka tooyour app.  Kan vara `query` eller `form_post`. |
| state |Rekommenderas |Ett värde som ingår i hello-begäran som returneras också hello token svar. Ett slumpmässigt genererat unikt värde används vanligtvis för [förhindra attacker med förfalskning av begäran](http://tools.ietf.org/html/rfc6749#section-10.12).  hello tillstånd är också används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade, exempelvis hello sida eller vy som de var på. |
| Resursen |Valfria |hello App-ID-URI för hello webb-API (skyddad resurs). toofind hello App-ID-URI för hello webb-API: T i hello Azure-portalen klickar du på **Active Directory**, klicka på hello directory hello program och klicka sedan på **konfigurera**. |
| kommandotolk |Valfria |Ange hello typ av användarinteraktion som krävs.<p> Giltiga värden är: <p> *inloggningen*: hello användaren ska ange tooreauthenticate. <p> *medgivande*: tillstånd har beviljats men måste toobe uppdateras. hello användaren ska ange tooconsent. <p> *admin_consent*: en administratör ska ange tooconsent för alla användare i organisationen |
| login_hint |Valfria |Kan vara toopre Fyll hello användarnamn/e-adressfältet i hello-inloggningssida för hello användaren, om du känner till sitt lösenord i förväg.  Ofta appar använder den här parametern under omautentisering som redan har extraherats hello användarnamn från en tidigare inloggning med hello `preferred_username` anspråk. |
| domain_hint |Valfria |Ger en ledtråd om hello klient eller domän som hello användare ska använda toosign i. hello-värdet för hello domain_hint är en registrerad domän för hello-klient. Om hello innehavaren är federerad tooan lokal katalog, omdirigerar AAD toohello angivna innehavaren federationsserver. |

> [!NOTE]
> Om hello användaren ingår i en organisation, administratör hello organisation medgivande eller neka på hello användares vägnar eller tillåta hello användaren tooconsent. hello ges användaren hello alternativet tooconsent endast när hello administratören tillåter det.
>
>

Nu uppmanas användaren hello tooenter sina autentiseringsuppgifter och medgivande toohello behörigheter anges i hello `scope` Frågeparametern. När hello användare autentiserar och ger ditt medgivande, Azure AD skickar en svar tooyour app på hello `redirect_uri` adressen i din begäran.

### Lyckat svar
Ett lyckat svar kan se ut så här:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parameter | Beskrivning |
| --- | --- |
| admin_consent |hello-värdet är SANT om en administratör godkänt tooa medgivandetext för begäran. |
| Koden |hello auktoriseringskod som programmet hello begärt. hello program kan använda hello auktorisering koden toorequest en åtkomst-token för hello målresursen. |
| session_state |Ett unikt värde som identifierar hello aktuella användarsessionen. Det här värdet är ett GUID, men ska behandlas som ett täckande värde som skickades utan undersökning. |
| state |Om en parametern state ingår i hello begäran ska hello samma värde visas i hello svar. Det är bra för hello programmet tooverify hello tillstånd värden i hello förfrågan och svar är identiska innan du använder hello svar. Detta hjälper toodetect [webbplatser begära förfalskning (CSRF) attacker](https://tools.ietf.org/html/rfc6749#section-10.12) mot hello-klienten. |

### Felsvar
Felsvar kan också skickas toohello `redirect_uri` så att programmet hello kan hantera dem på lämpligt sätt.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felvärde kod som definierats i avsnittet 5.2 av hello [OAuth 2.0 auktorisering Framework](http://tools.ietf.org/html/rfc6749). hello nästa tabell beskrivs hello felkoder som returnerar Azure AD. |
| error_description |En mer detaljerad beskrivning av hello-fel. Det här meddelandet är inte avsedd toobe slutanvändarens eget. |
| state |hello tillståndsvärde är ett slumpmässigt genererat inte återanvändas värde som skickades i begäran hello och returneras i attacker med förfalskning (CSRF) hello svar tooprevent begäran. |

#### Felkoder för auktorisering endpoint fel
hello följande tabell beskrivs hello olika felkoder som kan returneras i hello `error` parametern för hello felsvar.

| Felkod | Beskrivning | Klientåtgärd |
| --- | --- | --- |
| invalid_request |Protokollfel, till exempel en obligatorisk parameter saknas. |Åtgärda och skicka hello begäran igen. Detta är ett utvecklingsfel och vanligtvis fångas upp under inledande testningen. |
| unauthorized_client |hello klientprogrammet är inte tillåtna toorequest en Auktoriseringskoden. |Detta inträffar vanligtvis när hello klientprogrammet inte har registrerats i Azure AD eller läggs inte toohello användarens Azure AD-klient. hello program kan ange hello användare med instruktioner för att installera programmet hello och lägger till den tooAzure AD. |
| Om ACCESS_DENIED |Resursägare nekad medgivande |hello klientprogrammet kan meddela hello användare som det går inte att fortsätta om hello användaren godkänner. |
| unsupported_response_type |hello auktorisering servern stöder inte hello svarstyp i hello-begäran. |Åtgärda och skicka hello begäran igen. Detta är ett utvecklingsfel och vanligtvis fångas upp under inledande testningen. |
| server_error |hello-servern påträffade ett oväntat fel. |Försök igen med hello-begäran. Dessa fel kan bero på tillfälliga förhållanden. hello klientprogrammet kan förklara toohello användaren att svaret är försenad på grund av tooa tillfälligt fel. |
| temporarily_unavailable |hello-server är tillfälligt överbelastad toohandle hello begäran. |Försök igen med hello-begäran. hello klientprogrammet kan förklara toohello användaren att svaret är försenad på grund av tooa tillfälligt tillstånd. |
| invalid_resource |hello målresursen är ogiltig eftersom den inte finns, Azure AD kan inte hitta den eller det är inte korrekt konfigurerad. |Detta anger hello resursen om den finns inte har konfigurerats i hello-klient. hello program kan ange hello användare med instruktioner för att installera programmet hello och lägger till den tooAzure AD. |

## Använd hello auktorisering koden toorequest en åtkomst-token
Nu när du har skaffat ett auktoriseringskod och har beviljats behörighet av hello användare kan du lösa in hello-koden för en åtkomst-token toohello önskad resurs genom att skicka en POST-begäran toohello `/token` slutpunkt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parameter |  | Beskrivning |
| --- | --- | --- |
| Klient |Krävs |Hej `{tenant}` värdet hello sökvägen till hello begäran kan vara används toocontrol som kan logga in på programmet hello.  hello tillåtna värden är klient-ID: n, till exempel `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` eller `contoso.onmicrosoft.com` eller `common` för klient-oberoende token |
| client_id |Krävs |hello program-Id tilldelade tooyour app när du har registrerat med Azure AD. Du hittar du i hello klassiska Azure-portalen. Klicka på **Active Directory**, klicka på hello directory välja hello program och klicka på **konfigurera** |
| grant_type |Krävs |Måste vara `authorization_code` för hello-auktoriseringskodflödet. |
| Koden |Krävs |Hej `authorization_code` som du har införskaffade i föregående avsnitt i hello |
| redirect_uri |Krävs |Hej samma `redirect_uri` värde som har använt tooacquire hello `authorization_code`. |
| client_secret |krävs för webbprogram |Hej programhemlighet som du skapade i portalen för registrering av hello-app för din app.  Den bör inte användas i en intern app eftersom client_secrets inte kan lagras på ett tillförlitligt sätt på enheter.  Det krävs för webbappar och webb-API: er som har hello möjlighet toostore hello `client_secret` på ett säkert sätt på hello på serversidan. |
| Resursen |krävs om anges i tillståndet begäran, annars valfritt |hello App-ID-URI för hello webb-API (skyddad resurs). |

toofind hello App-ID URI i hello Azure-hanteringsportalen klickar du på **Active Directory**på hello katalog, klicka på hello program, och klicka sedan på **konfigurera**.

### Lyckat svar
Azure AD returnerar en åtkomst-token på ett lyckat svar. toominimize nätverk anrop från hello klientprogrammet och deras associerade latens hello klientprogram ska cachelagra åtkomsttoken för livslängd för hello token som anges i hello OAuth 2.0-svar. livslängd för toodetermine hello token, använda antingen hello `expires_in` eller `expires_on` parametervärden.

Om ett webb-API-resurs returnerar ett `invalid_token` felkoden detta kan tyda på att hello resurs har fastställt att hello-token har upphört att gälla. Om hello klient- och klockan tider är olika (kallas en ”time skeva”), överväga hello resurs hello token toobe upphörde att gälla innan hello token rensas från hello klientens cacheminne. Om detta inträffar avmarkera hello-token från hello cache, även om det är fortfarande inom livslängden beräknade.

Ett lyckat svar kan se ut så här:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
  "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ."
}

```

| Parameter | Beskrivning |
| --- | --- |
| access_token |hello begärda åtkomst-token. hello app kan använda den här token tooauthenticate toohello skyddad resurs, till exempel ett webb-API. |
| token_type |Anger hello tokentypen värdet. hello Skriv endast att Azure AD stöder är ägar. Mer information om ägar-token finns [OAuth2.0 auktorisering Framework: ägar-Token användning (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt) |
| expires_in |Hur länge hello åtkomst-token är giltig (i sekunder). |
| expires_on |hello tid när hello åtkomst-token upphör att gälla. hello representeras som hello antal sekunder från 1970-01-01T0:0:0Z UTC tills hello förfallotid. Det här värdet är används toodetermine hello livstid cachelagrade token. |
| Resursen |hello App-ID-URI för hello webb-API (skyddad resurs). |
| Omfång |Personifiering behörigheter beviljas toohello klientprogrammet. hello standardbehörigheten är `user_impersonation`. hello ägare hello skyddade resursen kan registrera ytterligare värden i Azure AD. |
| refresh_token |En token för uppdatering av OAuth 2.0. hello app kan använda den här token tooacquire ytterligare åtkomsttoken när hello aktuella åtkomst-token upphör att gälla.  Uppdatera token är långlivade och kan vara används tooretain åtkomst tooresources för längre tid. |
| id_token |En osignerad JSON-Webbtoken (JWT). hello app kan base64Url avkoda hello segmenten i den här token toorequest information om hello-användare som är inloggad. hello app kan cachelagra hello värden och visa dem, men det bör inte förlita dig på dem för auktorisering eller säkerhetsgränser. |

### JWT-Token anspråk
Hej JWT-token i hello värdet av hello `id_token` parameter kan avkodas till hello följande krav:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

Mer information om JSON web token finns hello [JWT IETF utkast till en specifikation](http://go.microsoft.com/fwlink/?LinkId=392344). Mer information om hello typer av token och anspråk [stöds Token och anspråkstyper](active-directory-token-and-claims.md)

Hej `id_token` parametern innehåller hello följande anspråkstyper:

| Anspråkstyp | Beskrivning |
| --- | --- |
| eller |Målgruppen för hello-token. När hello token utfärdas tooa klientprogram, hello målgruppen är hello `client_id` av hello-klienten. |
| EXP |Förfallotid. hello tid när hello-token upphör att gälla. För hello token toobe giltig, hello aktuellt datum och tid måste vara mindre än eller lika med toohello `exp` värde. hello tid representeras hello antal sekunder från den 1 januari 1970 (1970-01-01T0:0:0Z) UTC tills hello tid hello token har utfärdats. |
| family_name |Användarens senaste eller efternamn. hello-programmet kan visa det här värdet. |
| given_name |Användarens förnamn. hello-programmet kan visa det här värdet. |
| IAT |Utfärdad för närvarande. hello tid när hello JWT har utfärdats. hello tid representeras hello antal sekunder från den 1 januari 1970 (1970-01-01T0:0:0Z) UTC tills hello tid hello token har utfärdats. |
| ISS |Identifierar hello token utfärdare |
| NBF |Inte före tiden. hello tid när hello token träder i kraft. För hello token toobe giltig, måste hello aktuellt datum och tid vara större än eller lika toohello Nbf värde. hello tid representeras hello antal sekunder från den 1 januari 1970 (1970-01-01T0:0:0Z) UTC tills hello tid hello token har utfärdats. |
| OID |Objekt-ID (ID) hello användarobjektet i Azure AD. |
| Sub |Token ämne identifierare. Detta är en permanent och oåterkallelig identifierare för beskriver hello användare som hello token. Använd det här värdet i cachelagring logik. |
| tid |ID-Numret för hello Azure AD-klienten som utfärdade token hello-klient. |
| unique_name |En unik identifierare för som kan vara visas toohello användare. Detta är vanligtvis ett UPN (user Principal name). |
| UPN |Användarens huvudnamn för hello användare. |
| ver |Version. hello version av hello JWT-token, vanligtvis 1.0. |

### Felsvar
hello utfärdande endpoint fel är http-felkoder eftersom hello klientanrop hello utfärdande endpoint direkt. Dessutom toohello HTTP-statuskod hello Azure AD utfärdande endpoint returnerar också ett JSON-dokument med objekt som beskriver hello-fel.

Ett felsvar som exempel kan se ut så här:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: hello provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som kan använda tooclassify typer av fel som inträffar och kan vara används tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa utvecklare identifiera hello grundorsaken till ett autentiseringsfel. |
| error_codes |En lista över STS-specifika felkoder som kan hjälpa dig i diagnostik. |
| tidsstämpel |hello tid då hello-fel inträffade. |
| trace_id |En unik identifierare för hello-begäran som kan hjälpa dig i diagnostik. |
| correlation_id |En unik identifierare för hello-begäran som kan hjälpa dig i diagnostik för komponenter. |

#### Statuskoder för HTTP
hello visar följande tabell hello HTTP-statuskoder som hello utfärdande endpoint returnerar. I vissa fall hello felkoden är tillräckligt toodescribe hello svar, men om det finns fel, du måste tooparse hello tillhörande JSON dokumentera och undersöka felkoden.

| HTTP-kod | Beskrivning |
| --- | --- |
| 400 |Standard-http-kod. Används i de flesta fall och är vanligtvis på grund av tooa felaktigt utformad begäran. Åtgärda och skicka hello begäran igen. |
| 401 |Autentiseringen misslyckades. Till exempel hello-begäran hello client_secret parameter saknas. |
| 403 |Det gick inte att auktorisera. Till exempel har hello användaren inte behörighet tooaccess hello resurs. |
| 500 |Ett internt fel har uppstått vid hello-tjänsten. Försök igen med hello-begäran. |

#### Felkoder för token för slutpunkt-fel
| Felkod | Beskrivning | Klientåtgärd |
| --- | --- | --- |
| invalid_request |Protokollfel, till exempel en obligatorisk parameter saknas. |Åtgärda och skicka hello begäran igen |
| invalid_grant |hello auktoriseringskod är ogiltigt eller har upphört att gälla. |Försök med en ny begäran toohello `/authorize` slutpunkt |
| unauthorized_client |hello autentiserade klienten har inte behörighet toouse detta tillstånd bevilja. |Detta inträffar vanligtvis när hello klientprogrammet inte har registrerats i Azure AD eller läggs inte toohello användarens Azure AD-klient. hello program kan ange hello användare med instruktioner för att installera programmet hello och lägger till den tooAzure AD. |
| invalid_client |Klientautentisering misslyckades. |hello klientens autentiseringsuppgifter är inte giltiga. toofix, hello programadministratör uppdaterar hello autentiseringsuppgifter. |
| unsupported_grant_type |hello auktorisering servern stöder inte hello authorization grant typen. |Ändra hello bevilja i hello-begäran. Den här typen av fel bör sker enbart under utveckling och identifieras under inledande testningen. |
| invalid_resource |hello målresursen är ogiltig eftersom den inte finns, Azure AD kan inte hitta den eller det är inte korrekt konfigurerad. |Detta anger hello resursen om den finns inte har konfigurerats i hello-klient. hello program kan ange hello användare med instruktioner för att installera programmet hello och lägger till den tooAzure AD. |
| interaction_required |hello begäran kräver interaktion från användaren. Till exempel krävs ett steg i ytterligare autentisering. | I stället för en icke-interaktiv begäran, försök igen med en interaktiv auktoriseringsbegäran om hello samma resurs. |
| temporarily_unavailable |hello-server är tillfälligt överbelastad toohandle hello begäran. |Försök igen med hello-begäran. hello klientprogrammet kan förklara toohello användaren att svaret är försenad på grund av tooa tillfälligt tillstånd. |

## Använda hello åtkomst-token tooaccess hello resursen
Nu när du har skaffat har en `access_token`, du kan använda hello token i begäranden tooWeb API: er, genom att inkludera i hello `Authorization` huvud. Hej [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) specifikationen förklarar hur toouse ägar-token i HTTP-begäranden tooaccess skyddade resurser.

### Exempel på begäran
```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### Felsvar
Skyddade resurser som implementerar RFC 6750 problemet HTTP-statuskoder. Om hello-begäran innehåller inte autentiseringsuppgifter eller saknar hello token, hello svar inkluderar en `WWW-Authenticate` huvud. När en begäran inte svarar hello resursservern med hello HTTP-statuskoden och en felkod.

hello följande är ett exempel på ett misslyckade svar när hello klientbegäran inte innehåller hello ägar-token:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.microsoftonline.com/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="hello access token is missing.",
```

#### Felparametrar
| Parameter | Beskrivning |
| --- | --- |
| authorization_uri |hello hello auktorisering server-URI (fysiska slutpunkten). Det här värdet används också som en sökning tooget mer information om hello server från en slutpunkt för identifiering. <p><p> hello-klienten måste verifiera att hello auktorisering servern är betrodd. När hello resursen skyddas av Azure AD är tillräckligt tooverify som hello URL som börjar med https://login.microsoftonline.com eller ett annat värdnamn som har stöd för Azure AD. En klient-specifik resurs ska alltid returnera ett klient-specifika tillstånd URI. |
| fel |Ett felvärde kod som definierats i avsnittet 5.2 av hello [OAuth 2.0 auktorisering Framework](http://tools.ietf.org/html/rfc6749). |
| error_description |En mer detaljerad beskrivning av hello-fel. Det här meddelandet är inte avsedd toobe slutanvändarens eget. |
| resursid |Returnerar hello Unik identifierare för hello resurs. hello klientprogram kan använda den här identifieraren som hello värdet för hello `resource` parametern vid begäran om en token för hello resurs. <p><p> Det är viktigt för Hej klienten programmet tooverify det här värdet, annars skadliga service kanske kan tooinduce en **höjning av privilegier** attack <p><p> hello rekommenderas strategi för att förhindra en attack är tooverify hello `resource_id` matchar hello basen för hello web API-URL som ska användas. Till exempel om https://service.contoso.com/data används hello `resource_id` kan vara htttps://service.contoso.com/. hello-klientprogrammet måste avvisa en `resource_id` som inte börjar med hello bas-URL Om det inte finns en tillförlitlig alternativa sätt tooverify hello-ID: t. |

#### Felkoder för ägar-schema
hello RFC 6750 specifikationen definierar hello följande fel för resurser som använder hello WWW-autentisera sidhuvud och ägar schema hello svar.

| HTTP-statuskod | Felkod | Beskrivning | Klientåtgärd |
| --- | --- | --- | --- |
| 400 |invalid_request |hello-begäran är inte giltig. Till exempel den saknar en parameter eller med hjälp av hello samma parameter två gånger. |Åtgärda hello felet och försök hello-begäran. Den här typen av fel bör sker enbart under utveckling och kan identifieras i inledande tester. |
| 401 |invalid_token |hello åtkomst-token saknas, är ogiltigt eller har återkallats. hello hello error_description parameterns värde ger ytterligare detaljer. |Begär en ny token från hello auktorisering server. Om hello ny token inte har ett oväntat fel uppstod. Skicka en fel meddelande toohello användare och försök igen efter slumpmässig fördröjning. |
| 403 |insufficient_scope |hello åtkomst-token innehåller inte hello personifiering behörigheter krävs tooaccess hello resurs. |Skicka en ny begäran toohello auktorisering autentiseringsslutpunkt. Om hello svaret innehåller hello omfattningsparametern, använder du hello scope-värde i hello begäran toohello resursen. |
| 403 |insufficient_access |hello ämnet för hello token har inte hello behörigheter som är nödvändiga tooaccess hello resurs. |Fråga hello användaren toouse ett annat konto eller toorequest behörigheter toohello angivna resursen. |

## Uppdatera hello åtkomst-token
Åtkomsttoken är tillfällig och måste uppdateras efter förfallodatum toocontinue åtkomst till resurser. Du kan uppdatera hello `access_token` genom att skicka in en annan `POST` begära toohello `/token` slutpunkt, men nu att tillhandahålla hello `refresh_token` i stället för hello `code`.

Uppdatera token inte har angivna livslängd. Hello livslängd för uppdaterings-tokens normalt relativt lång. Men i vissa fall uppdaterings-tokens upphör, har återkallats eller saknar behörighet för hello önskad åtgärd. Programmet måste tooexpect och hantera fel som returneras av hello utfärdande slutpunkt på rätt sätt.

När du får ett svar med en uppdatering fel, ta bort hello aktuella uppdateringstoken och begär en ny auktoriseringskod eller åtkomst-token. I synnerhet när med hjälp av en uppdatering token i hello Authorization kod Grant flöde, om du får ett svar med hello `interaction_required` eller `invalid_grant` felkoder Ignorera hello uppdateringstoken och begär en ny auktoriseringskod.

Ett exempel begäran toohello **klient-specifika** slutpunkt (du kan också använda hello **vanliga** endpoint) tooget som en ny åtkomsttoken med hjälp av en uppdateringstoken som ser ut så här:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

### Lyckat svar
Ett lyckat token svar ser ut:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```
| Parameter | Beskrivning |
| --- | --- |
| token_type |hello tokentyp. hello stöds endast värdet är **ägar**. |
| expires_in |hello återstående hello token livstid i sekunder. Ett vanligt värde är 3 600 (en timme). |
| expires_on |hello datum och tid då hello token upphör att gälla. hello representeras som hello antal sekunder från 1970-01-01T0:0:0Z UTC tills hello förfallotid. |
| Resursen |Identifierar hello skyddad resurs som hello åtkomst-token kan använda tooaccess. |
| Omfång |Personifiering behörigheter beviljas toohello native client-program. hello standardbehörigheten är **user_impersonation**. hello målresurs hello ägare kan registrera alternativ i Azure AD. |
| access_token |hello ny åtkomsttoken som begärdes. |
| refresh_token |En ny OAuth 2.0-refresh_token som kan använda toorequest ny åtkomsttoken när hello något i det här svaret upphör att gälla. |

### Felsvar
Ett felsvar som exempel kan se ut så här:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: hello application named https://foo.microsoft.com/mail.read was not found in hello tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if hello application has not been installed by hello administrator of hello tenant or consented tooby any user in hello tenant.  You might have sent your authentication request toohello wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som kan använda tooclassify typer av fel som inträffar och kan vara används tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa utvecklare identifiera hello grundorsaken till ett autentiseringsfel. |
| error_codes |En lista över STS-specifika felkoder som kan hjälpa dig i diagnostik. |
| tidsstämpel |hello tid då hello-fel inträffade. |
| trace_id |En unik identifierare för hello-begäran som kan hjälpa dig i diagnostik. |
| correlation_id |En unik identifierare för hello-begäran som kan hjälpa dig i diagnostik för komponenter. |

En beskrivning av hello felkoder och hello rekommenderad klientåtgärd, se [felkoder för token för slutpunkt fel](#error-codes-for-token-endpoint-errors).
