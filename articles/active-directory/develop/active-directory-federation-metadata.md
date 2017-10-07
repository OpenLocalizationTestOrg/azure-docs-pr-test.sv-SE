---
title: aaaAzure AD Federationsmetadata | Microsoft Docs
description: "Den här artikeln beskriver hello federation metadata dokument som Azure Active Directory publicerar för tjänster som accepterar token för Azure Active Directory."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: c2d5f80b-aa74-452c-955b-d8eb3ed62652
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 23535bcd5eeb3e9b2e17d89a9b0420fc98bd3895
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="federation-metadata"></a>Federationsmetadata
Azure Active Directory (AD Azure) publicerar ett dokument för federation metadata för tjänster som är konfigurerade tooaccept hello säkerhetstoken som Azure AD utfärdar. hello federation metadata dokumentformat beskrivs i hello [Web Services Federation Language (WS-Federation) Version 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), som utökar [Metadata för hello OASIS Security Assertion Markup Language (SAML) v2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Klient-specifika och klient oberoende Metadataslutpunkter
Azure AD publicerar klient-specifika och klient oberoende slutpunkter.

Klient-specifika slutpunkter är utformade för en viss klient. hello klient-specifika federationsmetadata innehåller information om hello-klienten, inklusive klient-specifika utfärdare och slutpunkten. Program som begränsar åtkomst tooa enda innehavare använda klient-specifika slutpunkter.

Klient-oberoende slutpunkter ger information som är vanliga tooall Azure AD-klienter. Den här informationen gäller tootenants som värd *login.microsoftonline.com* och delas av klienter. Klient-oberoende slutpunkter rekommenderas för program med flera klienter, eftersom de inte är associerade med viss innehavare.

## <a name="federation-metadata-endpoints"></a>Federation Metadataslutpunkter
Azure AD publicerar federationsmetadata på `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

För **klient-specifika slutpunkter**, hello `TenantDomainName` kan vara något av följande typer hello:

* Ett registrerat domännamn för en Azure AD-klient, t.ex: `contoso.onmicrosoft.com`.
* hello ändras klient-ID för hello domän, som `72f988bf-86f1-41af-91ab-2d7cd011db45`.

För **klient oberoende slutpunkter**, hello `TenantDomainName` är `common`. Det här dokumentet innehåller endast hello Federationsmetadata-element som är vanliga tooall Azure AD-klienter som finns på login.microsoftonline.com.

En klient-specifika slutpunkt kan till exempel vara `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. hello oberoende av klient-slutpunkten är [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Du kan visa hello federation Metadatadokumentet genom att ange den här URL i en webbläsare.

## <a name="contents-of-federation-metadata"></a>Innehållet i federation Metadata
hello innehåller följande avsnitt information som krävs av tjänster som använder hello-token som utfärdats av Azure AD.

### <a name="entity-id"></a>Enhets-ID
Hej `EntityDescriptor` elementet innehåller ett `EntityID` attribut. Hej värdet för hello `EntityID` attributet representerar hello utfärdaren, det vill säga token hello säkerhet tjänsten (STS) som utfärdade hello-token. Det är viktigt toovalidate hello utfärdaren när du får en token.

hello följande metadata visas ett exempel på klient-specifika `EntityDescriptor` element med ett `EntityID` element.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
Du kan ersätta hello klient-ID i hello klient oberoende slutpunkt med din klient-ID toocreate klient-specifika `EntityID` värde. hello resultatvärdet kommer hello samma som hello token utfärdare. hello kan ett program med flera innehavare toovalidate hello utfärdaren för en viss klient.

hello följande metadata visas ett exempel på klient-oberoende `EntityID` element. Observera att hello `{tenant}` är en literal, inte en platshållare.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Certifikat för tokensignering
När en tjänst tar emot en token som utfärdas av en Azure AD-klient, måste hello signaturen för hello token verifieras med en signeringsnyckel som publiceras i hello federation Metadatadokumentet. Hej federationsmetadata innehåller hello offentliga delen av hello-certifikat som hello-klienter använder för tokensignering. hello certifikat byte visas i hello `KeyDescriptor` element. hello certifikatet för tokensignering är giltigt för signering endast när hello värdet för hello `use` attributet `signing`.

Ett federation metadata dokument som publicerats av Azure AD kan ha flera nycklar för signering, till exempel när Azure AD förbereder tooupdate hello signeringscertifikat. När ett dokument för federation metadata innehåller fler än ett certifikat är en tjänst som verifieras hello token ska ha stöd för alla certifikat i hello dokumentet.

hello följande metadata visas ett exempel på `KeyDescriptor` element med ett signeringsnyckeln.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

Hej `KeyDescriptor` elementet visas på två platser i hello federation Metadatadokumentet; i hello WS-Federation-specifik och hello SAML-specifika avsnitt. hello-certifikat som publicerats för båda avsnitten kommer hello samma.

Hello WS-Federation-specifik under en läsare för WS-Federation metadata läser hello certifikat från en `RoleDescriptor` element med hello `SecurityTokenServiceType` typen.

hello följande metadata visas ett exempel på `RoleDescriptor` element.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

Hello SAML-specifika under en läsare för WS-Federation metadata läser hello certifikat från en `IDPSSODescriptor` element.

hello följande metadata visas ett exempel på `IDPSSODescriptor` element.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Det finns inga skillnader i hello-format för klient-specifika och klient oberoende certifikat.

### <a name="ws-federation-endpoint-url"></a>WS-Federation slutpunkts-URL
Hej federationsmetadata innehåller hello URL som är Azure AD används för enkel inloggning och enkel utloggning i WS-Federation-protokollet. Den här slutpunkten som visas i hello `PassiveRequestorEndpoint` element.

hello följande metadata visas ett exempel på `PassiveRequestorEndpoint` element för en slutpunkt för innehavare.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
För hello klient oberoende slutpunkten hello WS-Federation-URL som visas i hello WS-Federation-slutpunkt som visas i följande exempel hello.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>Slutpunkts-URL för SAML-protokoll
Hej federationsmetadata innehåller hello URL där Azure AD för enkel inloggning och enskild utloggning i SAML 2.0-protokollet. Dessa slutpunkter som visas i hello `IDPSSODescriptor` element.

Hej inloggning och utloggning URL: er som visas i hello `SingleSignOnService` och `SingleLogoutService` element.

hello följande metadata visas ett exempel på `PassiveResistorEndpoint` för en slutpunkt för innehavare.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

På liknande sätt publiceras hello slutpunkter för hello vanliga SAML 2.0-protokollet slutpunkter i hello klient oberoende federationsmetadata, som visas i följande exempel hello.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
