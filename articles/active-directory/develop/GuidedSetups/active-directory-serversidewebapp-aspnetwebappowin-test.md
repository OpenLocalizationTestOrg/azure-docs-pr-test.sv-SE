---
title: "aaaAzure AD v2 ASP.NET Web Server komma igång - Test | Microsoft Docs"
description: "Implementera Microsoft logga In på en ASP.NET-lösning med ett traditionellt webbläsarbaserade program med OpenID Connect standard"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 99c7525b9146605142180962fc2a61b3c953c064
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a>Testa din kod

Tryck på `F5` toorun ditt projekt i Visual Studio. hello vid webbläsaren öppnas och leder dig direkt för*http://localhost: {port}* där du ser hello *logga in med Microsoft* knappen. Gå vidare och på den toosign i.

När du är klar tootest, Använd kontot toosign i ett arbets- eller skolkonto (Azure Active Directory) eller en personlig (live.com, outlook.com). 

![Logga in med Microsoft webbläsarfönster](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Logga in med Microsoft webbläsarfönster](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Förväntat resultat
Efter inloggning är hello användaren omdirigerade toohello startsidan på webbplatsen som är hello HTTPS-URL som anges i programmet registreringsinformation i hello portalen för registrering av Microsoft-program. Den här sidan visas nu *Hello {User}* och en länk toosign ut och en länk toosee hello användarens anspråk – vilket är en länk toohello auktorisera styrenhet skapade tidigare.

### <a name="see-users-claims"></a>Se användarens anspråk
Välj hello hyperlink toosee hello användarens anspråk. Detta leder dig toohello domänkontrollant och visa som endast är tillgängliga toousers som autentiseras.

#### <a name="expected-results"></a>Förväntat resultat
 Du bör se en tabell som innehåller hello grundläggande egenskaper för hello inloggade användare:

| Egenskap | Värde | Beskrivning|
|---|---|---|
| Namn | {Fullständig användarnamn} | hello användarens förnamn och efternamn
|Användarnamn | <span>user@domain.com</span>| hello användarnamn används tooidentify hello inloggade användare
| Ämne| {Ämne}|En sträng toouniquely identifiera hello användarinloggning mellan hello web|
| Klient-ID:t| {Guid}| En *guid* toouniquely representerar hello användarens Azure Active Directory-organisation.|

Dessutom visas en tabell, inklusive alla användaranspråk som ingår i autentiseringsbegäran. En lista över alla anspråk i en Id-Token och förklaring finns [artikel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "lista av anspråk i Token Id").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Testa åtkomst till en metod som har en *[auktorisera]* attribut (valfritt)
I det här steget ska du testa åtkomst till hello autentiserad domänkontrollant som en anonym användare:<br/>
Välj hello länka toosign ut hello användare och fullständig hello utloggning processen.<br/>
Skriv nu i din webbläsare http://localhost: {port} / autentiserade tooaccess styrenheten som skyddas med hello `[Authorize]` attribut

#### <a name="expected-results"></a>Förväntat resultat
Du bör få hello-fråga kräver tooauthenticate toosee hello vyn.

## <a name="additional-information"></a>Ytterligare information

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>Skydda hela webbplatsen
tooprotect hela webbplatsen, lägga till hello `AuthorizeAttribute` för`GlobalFilters` i `Global.asax` `Application_Start` metod:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> **Hur toorestrict användare från en enda organisation toosign i tooyour program**

> Som standard personliga konton (inklusive outlook.com, live.com och andra) samt arbets- och skolkonton konton från alla företag eller organisation som har integrerat med Azure Active Directory kan logga in tooyour program. 

> Om du vill att din ansökan tooaccept inloggningar från en enda organisation i Azure Active Directory, ersätter hello `Tenant` parameter i *web.config* från `Common` toohello innehavarens namn för hello organisation – exempelvis *contoso.onmicrosoft.com*. Efter det att ändra hello `ValidateIssuer` argument i din *OWIN-startklass* för`true`.

> Ange tooallow användare från att endast en lista över specifika organisationer `ValidateIssuer` tootrue och använda hello `ValidIssuers` parametern toospecify en lista över organisationer.

> Ett annat alternativ är tooimplement en anpassad metod toovalidate hello utfärdare IssuerValidator-parametern. Mer information om `TokenValidationParameters`, se [detta](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN-artikel") MSDN-artikel.

