---
title: aaaAzure AD service tooservice auth med OAuth2.0 On-Behalf-Of utkast till en specifikation | Microsoft Docs
description: "Den här artikeln beskriver hur toouse HTTP meddelanden tooimplement tooservice tjänstautentisering med hjälp av hello OAuth2.0 på-flöde."
services: active-directory
documentationcenter: .net
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 55b7fcfe6c0223bddedd8d8fa2defcb5769b43c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Tjänsten tooservice anrop med hjälp av delegerade användaridentitet i hello på-flöde
hello flödet för OAuth 2.0 On-Behalf-Of fungerar hello användningsfall där ett program anropar ett service/webb-API, som i sin tur måste toocall en annan tjänst/webb-API. hello idé är toopropagate hello delegerad användarens identitet och behörigheter via hello begäranden har gjorts. För hello mellannivå toomake autentiserade begäranden toohello underordnade tjänsten måste den toosecure en åtkomst-token från Azure Active Directory (Azure AD) för hello användares räkning.

## On-Behalf-Of flödesdiagram
Anta att hello-användaren har autentiserats på ett program med hello [OAuth 2.0 flöde beviljat med auktoriseringskod](active-directory-protocols-oauth-code.md). Hello-programmet har nu en åtkomst-token (token A) med hello användarens anspråk och medgivande tooaccess hello mellannivå webb-API (API-A). Nu måste API A toomake en begäran från en autentiserad toohello underordnat webb-API (API-B).

hello steg som följer utgöra hello på-flöde och förklaras med hello hello följande diagram.

![OAuth2.0 på-flöde](media/active-directory-protocols-oauth-on-behalf-of/active-directory-protocols-oauth-on-behalf-of-flow.png)


1. hello klientprogram gör en begäran tooAPI A med hello token A.
2. API-A autentiserar toohello Azure AD utfärdande slutpunkt och begär en token tooaccess API B.
3. hello Azure AD utfärdande endpoint validerar API A autentiseringsuppgifter med ett token och problem hello åtkomsttoken för API-B (token B).
4. hello token B har angetts i hello authorization-huvud för hello begäran tooAPI B.
5. Data från en skyddad resurs hello returneras av API B.

## Registrera hello och en tjänst i Azure AD
Registrera både hello klientprogrammet och hello mellannivå service i Azure AD.
### Registrera hello mellannivå tjänsten
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hello översta, klicka på fältet för ditt konto och på hello **Directory** Välj plats tooregister programmet hello Active Directory-klient.
3. Klicka på **fler tjänster** i hello vänstra nav och välj **Azure Active Directory**.
4. Klicka på **App registreringar** och välj **nya appregistrering**.
5. Ange ett eget namn för programmet hello och välj hello programtyp. Baserat på programtyp hello set hello inloggnings-URL eller omdirigera URL toohello bas-URL. Klicka på **skapa** toocreate hello program.
6. Välj programmet när fortfarande i hello Azure-portalen och klicka på **inställningar**. Välj på menyn Inställningar hello **nycklar** och lägga till en nyckel - markerar du en nyckel varaktighet för antingen 1 eller 2 år. När du sparar den här sidan, om hello nyckelvärdet visas, kopiera och spara hello värdet på en säker plats – du behöver den här nyckeln senare tooconfigure hello programinställningar i din implementering - värdet för nyckeln inte visas igen eller strängfält av någon andra sätt, så du posten det så snart den är synlig från hello Azure-portalen.

### Registrera hello klientprogrammet
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hello översta, klicka på fältet för ditt konto och på hello **Directory** Välj plats tooregister programmet hello Active Directory-klient.
3. Klicka på **fler tjänster** i hello vänstra nav och välj **Azure Active Directory**.
4. Klicka på **App registreringar** och välj **nya appregistrering**.
5. Ange ett eget namn för programmet hello och välj hello programtyp. Baserat på programtyp hello set hello inloggnings-URL eller omdirigera URL toohello bas-URL. Klicka på **skapa** toocreate hello program.
6. Konfigurera behörigheter för ditt program - hello Inställningar-menyn, Välj hello **nödvändiga behörigheter** klickar du på **Lägg till**, sedan **väljer en API**, och typen hello namnet på hello mellannivå tjänst i hello textruta. Klicka på **Välj behörigheter** och välj ' åtkomst *tjänstnamnet*'.

### Konfigurera kända klientprogram
I det här scenariot har hello mellannivå tjänsten ingen användare interaktion tooobtain hello användarens medgivande tooaccess hello underordnade API. Därför hello alternativet toogrant åtkomst toohello underordnade API måste visas direkt som en del av hello medgivande steg under autentiseringen.
tooachieve, följ hello här nedan tooexplicitly bind hello klienten appens registrering i Azure AD med hello registreringen av hello mellannivå-tjänsten, som sammanfogar hello medgivande som krävs av både hello-klienten och mellannivå till en enda dialog.
1. Navigera toohello mellannivå tjänstregistrering och klicka på **Manifest** tooopen hello manifestet editor.
2. Leta upp hello i hello manifestet `knownClientApplications` matrisen egenskapen och lägger till hello klient-ID för hello klientprogram som ett element.
3. Klicka på hello spara knappen sparas hello manifest.

## Tooservice åtkomst-token tjänstbegäran
toorequest ett åtkomsttoken, se en HTTP POST toohello klient-specifika Azure AD-slutpunkt med hello följande parametrar.

```
https://login.microsoftonline.com/<tenant>/oauth2/token
```
Det finns två fall beroende på om hello klientprogrammet väljer toobe som skyddas av en delad hemlighet eller ett certifikat.

### Först fall: token åtkomst-begäran med en delad hemlighet
När du använder en delad hemlighet, innehåller en tjänst-till-tjänst åtkomst tokenbegäran hello följande parametrar:

| Parameter |  | Beskrivning |
| --- | --- | --- |
| grant_type |Krävs | hello typ av begäran om hello-token. För en begäran som använder en JWT hello-värdet måste vara **urn: ietf:params:oauth:grant-typ: jwt-ägar**. |
| kontrollen |Krävs | hello-värdet för hello-token som används i hello-begäran. |
| client_id |Krävs | hello tilldelade App-ID toohello anropa tjänsten under registrering med Azure AD. toofind hello App-ID i hello Azure-hanteringsportalen klickar du på **Active Directory**på hello katalog och klicka sedan på hello programnamn. |
| client_secret |Krävs | hello nyckeln registrerats för hello anropar tjänsten i Azure AD. Det här värdet ska har konstaterats vid hello tiden för registrering. |
| Resursen |Krävs | hello App-ID-URI för hello tar emot service (skyddad resurs). toofind hello App-ID URI i hello Azure-hanteringsportalen klickar du på **Active Directory**, klicka på hello directory, hello programnamn, **alla inställningar** och klicka sedan på **egenskaper** . |
| requested_token_use |Krävs | Anger hur hello begäran ska bearbetas. I hello på-flöde hello-värdet måste vara **on_behalf_of**. |
| Omfång |Krävs | Ett utrymme avgränsade lista över scope för hello tokenbegäran. Hej omfånget för OpenID Connect **openid** måste anges.|

#### Exempel
hello följande HTTP POST-begäran som en åtkomsttoken för hello https://graph.windows.net webb-API. Hej `client_id` identifierar hello-tjänst som begär hello åtkomst-token.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_secret=0Y1W%2BY3yYb3d9N8vSjvm8WrGzVZaAaHbHHcGbcgG%2BoI%3D
&resource=https%3A%2F%2Fgraph.windows.net
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

### Andra fall: token åtkomst-begäran med ett certifikat
En token-tjänster åtkomst-begäran med ett certifikat innehåller hello följande parametrar:

| Parameter |  | Beskrivning |
| --- | --- | --- |
| grant_type |Krävs | hello typ av begäran om hello-token. För en begäran som använder en JWT hello-värdet måste vara **urn: ietf:params:oauth:grant-typ: jwt-ägar**. |
| kontrollen |Krävs | hello-värdet för hello-token som används i hello-begäran. |
| client_id |Krävs | hello tilldelade App-ID toohello anropa tjänsten under registrering med Azure AD. toofind hello App-ID i hello Azure-hanteringsportalen klickar du på **Active Directory**på hello katalog och klicka sedan på hello programnamn. |
| client_assertion_type |Krävs |hello-värdet måste vara`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Krävs | Ett intyg (en JSON Web Token) att du behöver toocreate och logga in med hello certifikat du registrerad som autentiseringsuppgifterna för ditt program.  Läs mer om [certifikat autentiseringsuppgifter](active-directory-certificate-credentials.md) toolearn hur tooregister ditt certifikat och hello formatering av hello-kontrollen.|
| Resursen |Krävs | hello App-ID-URI för hello tar emot service (skyddad resurs). toofind hello App-ID URI i hello Azure-hanteringsportalen klickar du på **Active Directory**, klicka på hello directory, hello programnamn, **alla inställningar** och klicka sedan på **egenskaper** . |
| requested_token_use |Krävs | Anger hur hello begäran ska bearbetas. I hello på-flöde hello-värdet måste vara **on_behalf_of**. |
| Omfång |Krävs | Ett utrymme avgränsade lista över scope för hello tokenbegäran. Hej omfånget för OpenID Connect **openid** måste anges.|

Observera att hello parametrar är nästan hello samma som hello fallet med hello begäran av delad hemlighet förutom att hello client_secret parameter har ersatts av två parametrar: en client_assertion_type och client_assertion.

#### Exempel
hello följande HTTP POST-begäranden en åtkomsttoken för hello https://graph.windows.net webb-API med ett certifikat. Hej `client_id` identifierar hello-tjänst som begär hello åtkomst-token.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&resource=https%3A%2F%2Fgraph.windows.net
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

## Tjänsten tooservice åtkomst-token svar
Ett lyckat svar är ett JSON OAuth 2.0-svar med hello följande parametrar.

| Parameter | Beskrivning |
| --- | --- |
| token_type |Anger hello tokentypen värdet. Hej typ som har stöd för Azure AD är **ägar**. Mer information om ägar-token finns hello [OAuth 2.0 auktorisering Framework: ägar-Token användning (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| Omfång |hello omfattningen av åtkomst som beviljas i hello-token. |
| expires_in |hello lång tid hello åtkomst-token är giltig (i sekunder). |
| expires_on |hello tid när hello åtkomst-token upphör att gälla. hello representeras som hello antal sekunder från 1970-01-01T0:0:0Z UTC tills hello förfallotid. Det här värdet är används toodetermine hello livstid cachelagrade token. |
| Resursen |hello App-ID-URI för hello tar emot service (skyddad resurs). |
| access_token |hello begärda åtkomst-token. hello anropar tjänsten kan använda den här token tooauthenticate toohello mottagande tjänsten. |
| id_token |hello begärda ID: token. hello anropar tjänsten kan använda tooverify hello användarens identitet och starta en session med hello användare. |
| refresh_token |Hej uppdateringstoken för hello begärt åtkomst-token. hello anropar tjänsten kan använda den här token toorequest en annan åtkomsttoken när hello aktuella åtkomst-token upphör att gälla. |

### Lyckade svar-exempel
hello visar följande exempel en lyckad svar tooa begäran om en åtkomsttoken för hello https://graph.windows.net webb-API.

```
{
    "token_type":"Bearer",
    "scope":"User.Read",
    "expires_in":"43482",
    "ext_expires_in":"302683",
    "expires_on":"1493466951",
    "not_before":"1493423168",
    "resource":"https://graph.windows.net",
    "access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ",
    "refresh_token":"AQABAAAAAABnfiG-mA6NTae7CdWW7QfdjKGu9-t1scy_TDEmLi4eLQMjJGt_nAoVu6A4oSu1KsRiz8XyQIPKQxSGfbf2FoSK-hm2K8TYzbJuswYusQpJaHUQnSqEvdaCeFuqXHBv84wjFhuanzF9dQZB_Ng5za9xKlUENrNtlq9XuLNVKzxEyeUM7JyxzdY7JiEphWImwgOYf6II316d0Z6-H3oYsFezf4Xsjz-MOBYEov0P64UaB5nJMvDyApV-NWpgklLASfNoSPGb67Bc02aFRZrm4kLk-xTl6eKE6hSo0XU2z2t70stFJDxvNQobnvNHrAmBaHWPAcC3FGwFnBOojpZB2tzG1gLEbmdROVDp8kHEYAwnRK947Py12fJNKExUdN0njmXrKxNZ_fEM33LHW1Tf4kMX_GvNmbWHtBnIyG0w5emb-b54ef5AwV5_tGUeivTCCysgucEc-S7G8Cz0xNJ_BOiM_4bAv9iFmrm9STkltpz0-Tftg8WKmaJiC0xXj6uTf4ZkX79mJJIuuM7XP4ARIcLpkktyg2Iym9jcZqymRkGH2Rm9sxBwC4eeZXM7M5a7TJ-5CqOdfuE3sBPq40RdEWMFLcrAzFvP0VDR8NKHIrPR1AcUruat9DETmTNJukdlJN3O41nWdZOVoJM-uKN3uz2wQ2Ld1z0Mb9_6YfMox9KTJNzRzcL52r4V_y3kB6ekaOZ9wQ3HxGBQ4zFt-2U0mSszIAA",
    "id_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC8yNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IvIiwiaWF0IjoxNDkzNDIzMTY4LCJuYmYiOjE0OTM0MjMxNjgsImV4cCI6MTQ5MzQ2Njk1MSwiYW1yIjpbInB3ZCJdLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXRpIjoieEN3ZnpoYS1QMFdKUU9MeENHZ0tBQSIsInZlciI6IjEuMCJ9."
}
```

### Fel svar-exempel
Ett felsvar returneras av Azure AD-token för slutpunkt vid tooacquire en åtkomst-token för hello underordnade API, om hello underordnade API: et har en princip för villkorlig åtkomst, till exempel multifaktorautentisering som angetts för den. hello mellannivå tjänsten ska ansluta det här felet toohello klientprogrammet så att klientprogrammet hello kan hello användaren interaktion toosatisfy hello villkorliga åtkomstprincipen.

```
{
    "error":"interaction_required",
    "error_description":"AADSTS50079: Due tooa configuration change made by your administrator, or because you moved tooa new location, you must enroll in multi-factor authentication tooaccess 'bf8d80f9-9098-4972-b203-500f535113b1'.\r\nTrace ID: b72a68c3-0926-4b8e-bc35-3150069c2800\r\nCorrelation ID: 73d656cf-54b1-4eb2-b429-26d8165a52d7\r\nTimestamp: 2017-05-01 22:43:20Z",
    "error_codes":[50079],
    "timestamp":"2017-05-01 22:43:20Z",
    "trace_id":"b72a68c3-0926-4b8e-bc35-3150069c2800",
    "correlation_id":"73d656cf-54b1-4eb2-b429-26d8165a52d7",
    "claims":"{\"access_token\":{\"polids\":{\"essential\":true,\"values\":[\"9ab03e19-ed42-4168-b6b7-7001fb3e933a\"]}}}"
}
```

## Använd hello åtkomst-token tooaccess hello skyddad resurs
Hello mellannivå service kan nu använda hello token anskaffats ovan toomake autentiserade begäranden toohello nedströms webb-API, genom att ange hello token i hello `Authorization` huvud.

### Exempel
```
GET /me?api-version=2013-11-08 HTTP/1.1
Host: graph.windows.net
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ
```

## Nästa steg
Läs mer om hello OAuth 2.0-protokollet och ett annat sätt tooperform service tooservice autentisering med autentiseringsuppgifter för klienten.
* [Tjänsten tooservice autentisering med OAuth 2.0 klientens autentiseringsuppgifter grant i Azure AD](active-directory-protocols-oauth-service-to-service.md)
* [OAuth 2.0 i Azure AD](active-directory-protocols-oauth-code.md)
