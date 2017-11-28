---
title: "aaaLearn om hello auktorisering protokoll som stöds av Azure AD v2.0 | Microsoft Docs"
description: "En guide tooprotocols som stöds av hello Azure AD v2.0-slutpunkten."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5fb4fa1b-8fc4-438e-b3b0-258d8c145f22
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: f90511b1880ff223f725c1f79df9f79bddccc7bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# v2.0 protokoll - OAuth 2.0 & OpenID Connect
hello v2.0-slutpunkten kan använda Azure AD identity-as-a-service med standardprotokollen, OpenID Connect och OAuth 2.0.  Hello-tjänsten är standardkompatibel, kan det finnas skillnader mellan två implementeringar av dessa protokoll.  hello information här kan vara användbart om du väljer toowrite din kod genom att skicka direkt & hanterar HTTP-begäranden eller Använd en 3 part Öppna källa biblioteket i stället för att använda någon av våra bibliotek med öppen källkod.
<!-- TODO: Need link toolibraries above -->

> [!NOTE]
> Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.  toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
>
>

## hello grunderna
I nästan alla OAuth & OpenID Connect flöden finns det fyra parter ingår i hello exchange:

![OAuth 2.0-roller](../../media/active-directory-v2-flows/protocols_roles.png)

* Hej **auktorisering Server** är hello v2.0-slutpunkten.  Den ansvarar för att säkerställa hello användarens identitet, bevilja och återkalla åtkomst tooresources och utfärdar token.  Den kallas även hello identitetsleverantör – den på ett säkert sätt hanterar något toodo med hello användarens information, deras åtkomst och hello förtroenderelationer mellan parterna i ett flöde.
* Hej **resursägare** är vanligtvis hello slutanvändaren.  Det är hello part som äger hello data och har hello power tooallow tredje parter tooaccess data, eller resurs.
* Hej **OAuth-klient** är din app som identifieras av ett program-Id.  Det är vanligtvis hello parten att hello slutanvändarens interagerar med och begär token från hello auktorisering server.  hello-klienten måste ha beviljats behörighet tooaccess hello resurs av hello resurs-ägare.
* Hej **resursservern** är där hello resurs eller data finns.  Den förtroenden hello auktorisering Server toosecurely autentisera och auktorisera hello OAuth-klienten och använder ägar access_tokens tooensure med åtkomst till tooa resurs kan beviljas.

## App-registrering
Alla appar som använder hello v2.0-slutpunkten måste registreras för toobe [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) innan det kan interagera med hjälp av OAuth eller OpenID Connect.  hello registreringsprocessen kommer samla in & tilldelar några värden tooyour app:

* En **program-Id** som unikt identifierar din app
* En **omdirigerings-URI** eller **Paketidentifierare** som kan vara används toodirect svar tillbaka tooyour app
* Några andra scenariespecifika-värden.

Mer information lär du dig hur för[registrera en app](active-directory-v2-app-registration.md).

## Slutpunkter
När registreringen är klar kommunicerar hello appen med Azure AD genom att skicka begäranden toohello v2.0-slutpunkten:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Hej där `{tenant}` kan vidta någon av fyra olika värden:

| Värde | Beskrivning |
| --- | --- |
| `common` |Användarna med både personliga Microsoft-konton och arbete/skolkonton från Azure Active Directory toosign till hello program. |
| `organizations` |Kan endast användare med arbete/skolkonton från Azure Active Directory toosign till hello program. |
| `consumers` |Kan endast användare med personliga Microsoft-konton (MSA) toosign till hello program. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` eller `contoso.onmicrosoft.com` |Kan endast användare med arbete/skolkonton från en viss Azure Active Directory-klient toosign till hello program.  Hello eget domännamn hello Azure AD-klient eller hello klient-guid-identifierare kan användas. |

Mer information om hur toointeract med dessa slutpunkter, Välj en viss app nedan.

## Token
hello v2.0 implementering av OAuth 2.0 och OpenID Connect utnyttja omfattande ägar-token, inklusive ägar-token som JWTs. Ett ägartoken är en förenklad säkerhetstoken som ger hello ”ägar” åtkomst tooa skyddad resurs. På detta sätt är hello ”ägar” någon part som kan utgöra hello-token. Även om en part måste de först autentisera med Azure AD tooreceive hello ägar-token om hello krävs åtgärder inte vidtas toosecure hello token i överföringen och lagringen, kan de snappas upp och används av en oavsiktlig part. Även om vissa säkerhetstoken har en inbyggd mekanism för att förhindra att obehöriga personer använder dem, ägar-token har inte den här mekanismen och måste transporteras i en säker kanal, till exempel transport layer security (HTTPS). Om ett ägartoken överförs i hello rensa, en hello man i mitten-attack kan användas av skadliga tooacquire hello-token från en och använda det för en obehörig åtkomst tooa skyddad resurs. hello tillämpas samma säkerhetsprinciper när lagring eller cachelagring ägar-token för senare användning. Se alltid till att appen skickar och lagrar ägar-token på ett säkert sätt. Flera säkerheten på ägar-token finns [RFC 6750 avsnitt 5](http://tools.ietf.org/html/rfc6750).

Mer information om olika typer av token som används i hello v2.0-slutpunkten är tillgänglig i [hello v2.0-slutpunkten tokenreferens](active-directory-v2-tokens.md).

## Protokoll
Om du är klar toosee några exempel begär, komma igång med en hello nedan självstudier.  Var och en motsvarar tooa viss autentiseringsscenariot.  Om du behöver hjälp med att fastställa vilket är hello rätt flödet för du kolla [hello typer av appar som du kan skapa med hello v2.0](active-directory-v2-flows.md).

* [Skapa mobila och interna program med OAuth 2.0](active-directory-v2-protocols-oauth-code.md)
* [Skapa Web Apps med öppna ID Connect](active-directory-v2-protocols-oidc.md)
* [Skapa den enda sidan appar med hello OAuth 2.0 Implicit flöda](active-directory-v2-protocols-implicit.md)
* [Skapa Daemon eller Server Side processer med hello flöda för OAuth 2.0 klientens autentiseringsuppgifter](active-directory-v2-protocols-oauth-client-creds.md)
* [Hämta token i Web API med OAuth 2.0 för flöda hello](active-directory-v2-protocols-oauth-on-behalf-of.md)
