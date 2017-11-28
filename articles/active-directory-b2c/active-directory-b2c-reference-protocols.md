---
title: 'Azure Active Directory B2C: Autentiseringsprotokoll | Microsoft Docs'
description: "Hur hello toobuild appar direkt med hjälp av protokoll som stöds av Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5e407d0a-73a2-4d74-ac81-3aa6c31ddcee
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.openlocfilehash: 8fa4cbebe711841d410b3ae43b78f893c06d9b63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Azure AD B2C: Autentiseringsprotokoll
Azure Active Directory B2C (Azure AD B2C) tillhandahåller som en tjänst för dina appar genom att stödja två standardprotokollen: OpenID Connect och OAuth 2.0. hello-tjänsten är standardkompatibel, men två implementeringar av dessa protokoll kan ha vissa skillnader. 

hello informationen i den här guiden är användbart om du kan skriva koden genom att skicka direkt och HTTP-begäranden, i stället för med hjälp av ett bibliotek med öppen källkod. Vi rekommenderar att du läser den här sidan innan du fördjupa dig i hello information om varje specifik protokoll. Men om du redan är bekant med Azure AD B2C kan du gå direkt för[hello protokollet referens guider](#protocols).

<!-- TODO: Need link toolibraries above -->

## hello-grunderna
Alla appar som använder Azure AD B2C måste registreras i din B2C-katalog i hello toobe [Azure-portalen](https://portal.azure.com). hello registreringsprocessen samlar in och tilldelar några värden tooyour app:

* Ett **program-ID** som identifierar din app unikt.
* En **omdirigerings-URI** eller **paketet identifierare** som kan vara används toodirect svar tillbaka tooyour app.
* Några andra scenariespecifika-värden. Mer information lär du dig [hur tooregister programmet](active-directory-b2c-app-registration.md).

När du har registrerat appen kommunicerar med Azure Active Directory (AD Azure) genom att skicka begäranden toohello slutpunkt:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

I nästan alla OAuth och OpenID Connect flöden ingår fyra parterna i hello exchange:

![OAuth 2.0-roller](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

* Hej **auktorisering server** är hello Azure AD-slutpunkt. På ett säkert sätt hanterar något annat relaterat toouser information och åtkomst. Den hanterar också hello förtroenderelationer mellan hello parterna i ett flöde. Den ansvarar för att verifiera hello användarens identitet, bevilja och återkalla åtkomst tooresources och utfärdar token. Det är även kallat hello identitetsleverantör.

* Hej **resursägare** är vanligtvis hello slutanvändaren. Det är hello part som äger hello data och har hello power tooallow tredje part tooaccess data eller resurs.

* Hej **OAuth-klient** är din app. Den identifieras av dess program-ID. Det är vanligtvis hello part som slutanvändarna interagera med. Begär den också token från hello auktorisering server. hello resurs-ägare måste ge hello klienten behörigheten tooaccess hello resurs.

* Hej **resursservern** är där hello resurs eller data finns. Hello auktorisering litar server toosecurely autentisera och auktorisera hello OAuth-klient. Dessutom används ägar åtkomst-token tooensure med åtkomst till tooa resurs kan beviljas.

## Principer
Azure AD B2C principer är tvekan, hello de viktigaste funktionerna för hello-tjänsten. Azure AD B2C utökar hello OAuth 2.0 och OpenID Connect standardprotokoll genom att introducera principer. Dessa kan Azure AD B2C tooperform mycket mer än enkel autentisering och auktorisering. 

Principer fullständigt beskriver konsumenten identitetsupplevelser, inklusive registrering, inloggning, och redigera-profilen. Principer kan definieras i en administrativa Gränssnittet. De kan köras med hjälp av en särskild frågeparameter i HTTP-begäranden om autentisering. 

Principer är inte standardfunktioner i OAuth 2.0 och OpenID Connect, så du bör vidta hello tid toounderstand dem. Mer information finns i hello [referenshandbok för Azure AD B2C princip](active-directory-b2c-reference-policies.md).

## Token
hello Azure AD B2C-implementering av OAuth 2.0 och OpenID Connect gör ägar-token, inklusive ägar-token som visas i form av JSON web token (JWTs) används. Ett ägartoken är en förenklad säkerhetstoken som ger hello ”ägar” åtkomst tooa skyddad resurs.

hello ägar är någon part som kan utgöra hello-token. Azure AD måste de först autentisera en part innan det kan ta emot ett ägartoken. Men om det behövs hello åtgärder inte vidtas toosecure hello token i överföringen och lagringen kan snappas upp och används av en oavsiktlig part.

Vissa säkerhetstoken har inbyggda mekanismer som förhindrar att obehöriga personer använder dem, men ägar-token har inte den här mekanismen. De måste transporteras i en säker kanal, till exempel en transport layer security (HTTPS). 

Om ett ägartoken överförs utanför en säker kanal, kan en obehörig part använder en man-in-the-middle-attack tooacquire hello token och använda den toogain obehörig åtkomst tooa skyddade resursen. hello samma säkerhetsprinciper gäller när ägar-token lagras eller cachelagras för senare användning. Se alltid till att appen skickar och lagrar ägar-token på ett säkert sätt.

Ytterligare ägar-token säkerheten finns [RFC 6750 avsnitt 5](http://tools.ietf.org/html/rfc6750).

Mer information om hello olika typer av token som används i Azure AD B2C finns i [hello Azure AD tokenreferens](active-directory-b2c-reference-tokens.md).

## Protokoll
När du är klar tooreview några exempel begär, du kan börja med en av följande kurser hello. Varje motsvarar tooa viss autentiseringsscenariot. Om du behöver hjälp med att fastställa vilka flödet är rätt för dig, kolla [hello typer av appar som du kan skapa med hjälp av Azure AD B2C](active-directory-b2c-apps.md).

* [Skapa mobila och interna program med hjälp av OAuth 2.0](active-directory-b2c-reference-oauth-code.md)
* [Skapa webbappar med OpenID Connect](active-directory-b2c-reference-oidc.md)
* [Skapa en sida appar med hjälp av implicita flödet för hello OAuth 2.0](active-directory-b2c-reference-spa.md)

