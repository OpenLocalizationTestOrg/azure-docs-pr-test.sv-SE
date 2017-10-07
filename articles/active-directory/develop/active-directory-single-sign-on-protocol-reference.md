---
title: "aaaAzure enkel inloggning på SAML-protokoll | Microsoft Docs"
description: "Den här artikeln beskriver hello enkel inloggning på SAML-protokoll i Azure Active Directory"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: ad8437f5-b887-41ff-bd77-779ddafc33fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: priyamo
ms.custom: aaddev
ms.openlocfilehash: 435cfe0e7be3f2defd34e8b6f6b0f08596ee1f48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Enkel inloggning SAML-protokoll
Den här artikeln beskriver hello SAML 2.0-autentiseringsbegäranden och -svar som har stöd för Azure Active Directory (Azure AD) för enkel inloggning.

hello protokollet diagrammet nedan beskrivs hello enkel inloggning serie. hello tjänst i molnet (hello service provider) använder en omdirigering för HTTP-bindning toopass en `AuthnRequest` (autentiseringsbegäran) elementet tooAzure AD (hello identitetsleverantör). Azure AD sedan använder en HTTP post-bindning toopost en `Response` elementet toohello Molntjänsten.

![Arbetsflöde för enkel inloggning](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## AuthnRequest
toorequest en användarautentisering molntjänster skicka en `AuthnRequest` elementet tooAzure AD. Ett exempel på SAML 2.0 `AuthnRequest` kan se ut så här:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Parameter |  | Beskrivning |
| --- | --- | --- |
| ID |Krävs |Azure AD använder det här attributet toopopulate hello `InResponseTo` attribut för hello returnerade svar. ID: T kan inte börja med ett tal, så att en gemensam strategi är tooprepend liknande ”id” toohello strängrepresentation av en GUID. Till exempel `id6c1c178c166d486687be4aaf5e482730` är ett giltigt ID. |
| Version |Krävs |Detta bör vara **2.0**. |
| IssueInstant |Krävs |Detta är ett DateTime-sträng med ett värde för UTC och [fram och åter format (”o”)](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD förväntar sig ett DateTime-värde av samma typ, men inte utvärdera och använda hello värde. |
| AssertionConsumerServiceUrl |Valfria |Om det måste matcha hello `RedirectUri` för hello Molntjänsten i Azure AD. |
| ForceAuthn |Valfria | Detta är ett booleskt värde. Om värdet är SANT innebär hello användaren kommer att framtvingad toore-autentisera, även om de har en ogiltig session med Azure AD. |
| IsPassive |Valfria |Detta är ett booleskt värde som anger om Azure AD ska autentisera hello användaren tyst, utan användarinteraktion, med hjälp av hello sessions-cookie om sådan finns. Om detta är sant, försöker Azure AD tooauthenticate hello användare som använder hello sessions-cookie. |

Alla andra `AuthnRequest` attribut, t.ex. ditt medgivande, mål, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex och ProviderName är **ignoreras**.

Azure AD också ignorerar hello `Conditions` element i `AuthnRequest`.

### Utfärdaren
Hej `Issuer` element i en `AuthnRequest` måste exakt matcha en hello **ServicePrincipalNames** i hello Molntjänsten i Azure AD. Detta är normalt inställt toohello **App-ID URI** som anges under programmet registreringen.

Ett exempel SAML utdrag som innehåller hello `Issuer` element ser ut så här:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### NameIDPolicy
Det här elementet begär ett visst namn-ID-format i hello svar och är valfritt i `AuthnRequest` element skickas tooAzure AD.

Ett exempel på en `NameIdPolicy` element ser ut så här:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Om `NameIDPolicy` har angetts kan du inkludera den valfria `Format` attribut. Hej `Format` kan ha endast ett av följande värden hello; andra värdet resulterar i ett fel.

* `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory utfärdar hello NameID anspråk som en pairwise identifierare.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory utfärdar hello NameID anspråk i formatet för e-postadress.
* `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Det här värdet tillåter Azure Active Directory tooselect hello anspråk format. Azure Active Directory utfärdar hello NameID som en pairwise identifierare.
* `urn:oasis:names:tc:SAML:2.0:nameid-format:transient`: Azure Active Directory utfärdar hello NameID anspråk som en slumpmässigt genererad värde som är unikt toohello aktuella SSO-åtgärden. Detta innebär att hello-värdet är tillfällig och går inte att använda tooidentify hello autentisera användaren.

Azure AD ignorerar hello `AllowCreate` attribut.

### RequestAuthnContext
Hej `RequestedAuthnContext` element anger hello önskad autentiseringsmetoder. Det är valfritt i `AuthnRequest` element skickas tooAzure AD. Azure AD stöder endast en `AuthnContextClassRef` värde: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### Ange omfång
Hej `Scoping` element som innehåller en lista över identitetsleverantörer är valfritt i `AuthnRequest` element skickas tooAzure AD.

Om inte innehåller hello `ProxyCount` attributet `IDPListOption` eller `RequesterID` element, eftersom de inte stöds.

### Signatur
Inkludera inte en `Signature` element i `AuthnRequest` element, som Azure AD inte stöder signerats autentiseringsbegäranden.

### Ämne
Azure AD ignorerar hello `Subject` element av `AuthnRequest` element.

## Svar
När en begärda inloggning är klar skickar en molntjänst för svar toohello Azure AD. Exempel svar tooa inloggning fått ser ut så här:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### Svar
Hej `Response` elementet innehåller hello resultatet av hello auktoriseringsbegäran. Azure AD anger hello `ID`, `Version` och `IssueInstant` värden i hello `Response` element. Den anger också hello följande attribut:

* `Destination`: När inloggningen är klar detta anges toohello `RedirectUri` hello service provider (Molntjänsten).
* `InResponseTo`: Det här värdet toohello `ID` attribut för hello `AuthnRequest` element som initierade hello svar.

### Utfärdaren
Azure AD anger hello `Issuer` element för`https://login.microsoftonline.com/<TenantIDGUID>/` där <TenantIDGUID> är hello klient-ID för hello Azure AD-klient.

Ett exempelsvar med utfärdaren element kan se ut så här:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### Status
Hej `Status` förmedlas elementet hello lyckad eller Misslyckad inloggning. Den omfattar hello `StatusCode` element som innehåller en kod eller en uppsättning kapslade koder som representerar hello status för hello-begäran. Den omfattar också hello `StatusMessage` element som innehåller anpassade felmeddelanden som genereras under hello inloggning.

<!-- TODO: Add a authentication protocol error reference -->

hello följande är en SAML-svar tooan misslyckat inloggning försök.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: hello SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### kontrollen
I tillägg toohello `ID`, `IssueInstant` och `Version`, Azure AD anger hello följande element i hello `Assertion` element av hello svar.

#### Utfärdaren
Detta är inställt för`https://sts.windows.net/<TenantIDGUID>/`där <TenantIDGUID> är hello klient-ID för hello Azure AD-klient.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### Signatur
Azure AD loggar hello assertion i svaret tooa lyckad inloggning. Hej `Signature` elementet innehåller en digital signatur som hello Molntjänsten kan använda tooauthenticate hello källa tooverify hello integriteten hos hello-kontrollen.

den digitala signaturen, Azure AD använder toogenerate hello signeringsnyckeln i hello `IDPSSODescriptor` element i Metadatadokumentet dess.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### Ämne
Anger hello säkerhetsobjekt som är hello instruktioner i hello assertion hello ämne. Den innehåller en `NameID` element som motsvarar hello autentiserade användaren. Hej `NameID` värdet är en riktad identifierare som är riktade endast toohello-leverantör som är hello målgruppen för hello-token. Det är beständiga – den kan återkallas, men aldrig omtilldelas. Det är också täckande, eftersom den inte avslöjar något om hello användare och kan inte användas som en identifierare för attributet frågor.

Hej `Method` attribut för hello `SubjectConfirmation` elementet alltid inställt för`urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### Villkor
Det här elementet anger villkor som definierar hello godkänd användning av SAML intyg.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

Hej `NotBefore` och `NotOnOrAfter` attribut anger hello intervall under vilka hello assertion är giltig.

* Hej värdet för hello `NotBefore` attribut är lika tooor något (mindre än en sekund) senare än hello värdet `IssueInstant` attribut för hello `Assertion` element. Azure AD tar inte hänsyn till eventuella tidsskillnaden mellan sig själv och hello molntjänst (service provider) och lägger inte till helst toothis buffert.
* Hej värdet för hello `NotOnOrAfter` attributet är 70 minuter senare än hello värdet för hello `NotBefore` attribut.

#### Målgrupp
Innehåller en URI som identifierar en målgrupp. Azure AD anger hello-värdet för det här elementet toohello värdet av `Issuer` element av hello `AuthnRequest` som initierade hello-inloggning. tooevaluate hello `Audience` värde, använda hello värdet hello `App ID URI` som angavs under registreringen av program.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Som hello `Issuer` värde, hello `Audience` värdet måste exakt matcha en hello tjänsthuvudnamn som representerar hello Molntjänsten i Azure AD. Men om hello värdet av hello `Issuer` elementet är inte ett URI-värde hello `Audience` värdet hello svar är hello `Issuer` värdet med prefixet `spn:`.

#### AttributeStatement
Innehåller anspråk om hello ämne eller användare. hello följande utdrag innehåller ett exempel på `AttributeStatement` element. hello tre punkter anger att hello-element kan innehålla flera attribut och attributvärden.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```        

* **Namnge anspråk** : hello värdet för hello `Name` attribut (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) är hello huvudnamn för hello autentiserade användare som `testuser@managedtenant.com`.
* **ObjectIdentifier anspråk** : hello värdet för hello `ObjectIdentifier` attribut (`http://schemas.microsoft.com/identity/claims/objectidentifier`) är hello `ObjectId` av hello katalogobjekt som representerar hello autentiserade användare i Azure AD. `ObjectId`är en oföränderlig globalt unik och återanvända säker identifierare för hello autentiserade användare.

#### AuthnStatement
Det här elementet Assert hello assertion ämnet autentiserades på ett visst sätt vid en viss tidpunkt.

* Hej `AuthnInstant` attributet anger hello tiden i vilka hello-användare som autentiseras med Azure AD.
* Hej `AuthnContext` elementet anger hello autentiseringskontext tooauthenticate hello användare.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```