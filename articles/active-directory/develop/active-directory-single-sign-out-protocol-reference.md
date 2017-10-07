---
title: aaaAzure enda logga ut SAML-protokoll | Microsoft Docs
description: "Den här artikeln beskriver hello enda Sign-Out SAML-protokoll i Azure Active Directory"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 0e4aa75d-d1ad-4bde-a94c-d8a41fb0abe6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: priyamo
ms.custom: aaddev
ms.openlocfilehash: 889c9b3397a601c16ba6971d2b15bfee305576de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Enkel utloggning SAML-protokoll
Azure Active Directory (AD Azure) stöder hello SAML 2.0 web webbläsare enskild utloggning profil. För enkel utloggning toowork korrekt hello **LogoutURL** för programmet hello uttryckligen måste registreras med Azure AD under registreringen av program. Azure AD använder hello LogoutURL tooredirect användare när de har loggat.

Det här diagrammet visar hello arbetsflöde för hello Azure AD som enda process för utloggning.

![Enkel inloggning i arbetsflödet](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## LogoutRequest
Hej cloud service skickar en `LogoutRequest` meddelande tooAzure AD tooindicate att en session har avslutats. hello följande utdrag visar ett exempel `LogoutRequest` element.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### LogoutRequest
Hej `LogoutRequest` element skickas tooAzure AD kräver hello följande attribut:

* `ID`: Det identifierar hello utloggning förfrågan. Hej värdet för `ID` får inte inledas med en siffra. hello vanliga sättet är tooappend **id** toohello strängrepresentation av en GUID.
* `Version`: Ange hello-värdet för det här elementet för**2.0**. Det här värdet är obligatoriskt.
* `IssueInstant`: Det här är en `DateTime` sträng med ett värde för samordna Universal Time (UTC) och [fram och åter format (”o”)](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD förväntas ett värde av den här typen, men använda inte den.

### Utfärdaren
Hej `Issuer` element i en `LogoutRequest` måste exakt matcha en hello **ServicePrincipalNames** i hello Molntjänsten i Azure AD. Detta är normalt inställt toohello **App-ID URI** som anges under programmet registreringen.

### NameID
Hej värdet för hello `NameID` element måste exakt matcha hello `NameID` för hello användare som loggas ut.

## LogoutResponse
Azure AD skickar en `LogoutResponse` i svaret tooa `LogoutRequest` element. hello följande utdrag visar ett exempel på en `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### LogoutResponse
Azure AD anger hello `ID`, `Version` och `IssueInstant` värden i hello `LogoutResponse` element. Anger också hello `InResponseTo` toohello elementvärde av hello `ID` attribut för hello `LogoutRequest` som förvärvas hello svar.

### Utfärdaren
Azure AD anger det här värdet för`https://login.microsoftonline.com/<TenantIdGUID>/` där <TenantIdGUID> är hello klient-ID för hello Azure AD-klient.

tooevaluate hello värdet för hello `Issuer` element, Använd hello värdet för hello **App-ID URI** angav under registreringen av program.

### Status
Azure AD använder hello `StatusCode` element i hello `Status` elementet tooindicate hello lyckad eller misslyckad utloggning. När hello utloggning försöket misslyckas hello `StatusCode` element kan också innehålla anpassade felmeddelanden.
