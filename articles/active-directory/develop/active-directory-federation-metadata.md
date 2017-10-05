---
title: Azure AD-Federationsmetadata | Microsoft Docs
description: "Den här artikeln beskriver federation Metadatadokumentet som Azure Active Directory publicerar för tjänster som accepterar token för Azure Active Directory."
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
ms.openlocfilehash: ecafb02a6ac13d1c3cd1fe77ef710cd8525e32b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="federation-metadata"></a><span data-ttu-id="e0a17-103">Federationsmetadata</span><span class="sxs-lookup"><span data-stu-id="e0a17-103">Federation metadata</span></span>
<span data-ttu-id="e0a17-104">Azure Active Directory (AD Azure) publicerar en federation metadata dokument för tjänster som är konfigurerad för att acceptera de säkerhetstoken som Azure AD utfärdar.</span><span class="sxs-lookup"><span data-stu-id="e0a17-104">Azure Active Directory (Azure AD) publishes a federation metadata document for services that is configured to accept the security tokens that Azure AD issues.</span></span> <span data-ttu-id="e0a17-105">Federation metadata dokumentets format är beskrivs i den [Web Services Federation Language (WS-Federation) Version 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), som utökar [Metadata för OASIS Security Assertion Markup Language (SAML) v2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).</span><span class="sxs-lookup"><span data-stu-id="e0a17-105">The federation metadata document format is described in the [Web Services Federation Language (WS-Federation) Version 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), which extends [Metadata for the OASIS Security Assertion Markup Language (SAML) v2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).</span></span>

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a><span data-ttu-id="e0a17-106">Klient-specifika och klient oberoende Metadataslutpunkter</span><span class="sxs-lookup"><span data-stu-id="e0a17-106">Tenant-specific and Tenant-independent metadata endpoints</span></span>
<span data-ttu-id="e0a17-107">Azure AD publicerar klient-specifika och klient oberoende slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="e0a17-107">Azure AD publishes tenant-specific and tenant-independent endpoints.</span></span>

<span data-ttu-id="e0a17-108">Klient-specifika slutpunkter är utformade för en viss klient.</span><span class="sxs-lookup"><span data-stu-id="e0a17-108">Tenant-specific endpoints are designed for a particular tenant.</span></span> <span data-ttu-id="e0a17-109">Klient-specifika federationsmetadata innehåller information om klienten, inklusive klient-specifik information för utfärdare och slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e0a17-109">The tenant-specific federation metadata includes information about the tenant, including tenant-specific issuer and endpoint information.</span></span> <span data-ttu-id="e0a17-110">Program som begränsar åtkomsten till en enskild klient använda klient-specifika slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="e0a17-110">Applications that restrict access to a single tenant use tenant-specific endpoints.</span></span>

<span data-ttu-id="e0a17-111">Klient-oberoende slutpunkter ger information som är gemensamma för alla Azure AD-klienter.</span><span class="sxs-lookup"><span data-stu-id="e0a17-111">Tenant-independent endpoints provide information that is common to all Azure AD tenants.</span></span> <span data-ttu-id="e0a17-112">Den här informationen gäller för klienter som värd *login.microsoftonline.com* och delas av klienter.</span><span class="sxs-lookup"><span data-stu-id="e0a17-112">This information applies to tenants hosted at *login.microsoftonline.com* and is shared across tenants.</span></span> <span data-ttu-id="e0a17-113">Klient-oberoende slutpunkter rekommenderas för program med flera klienter, eftersom de inte är associerade med viss innehavare.</span><span class="sxs-lookup"><span data-stu-id="e0a17-113">Tenant-independent endpoints are recommended for multi-tenant applications, since they are not associated with any particular tenant.</span></span>

## <a name="federation-metadata-endpoints"></a><span data-ttu-id="e0a17-114">Federation Metadataslutpunkter</span><span class="sxs-lookup"><span data-stu-id="e0a17-114">Federation metadata endpoints</span></span>
<span data-ttu-id="e0a17-115">Azure AD publicerar federationsmetadata på `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.</span><span class="sxs-lookup"><span data-stu-id="e0a17-115">Azure AD publishes federation metadata at `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.</span></span>

<span data-ttu-id="e0a17-116">För **klient-specifika slutpunkter**, `TenantDomainName` kan vara något av följande typer:</span><span class="sxs-lookup"><span data-stu-id="e0a17-116">For **tenant-specific endpoints**, the `TenantDomainName` can be one of the following types:</span></span>

* <span data-ttu-id="e0a17-117">Ett registrerat domännamn för en Azure AD-klient, t.ex: `contoso.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="e0a17-117">A registered domain name of an Azure AD tenant, such as: `contoso.onmicrosoft.com`.</span></span>
* <span data-ttu-id="e0a17-118">Den oåterkalleliga klient-ID på domänen som `72f988bf-86f1-41af-91ab-2d7cd011db45`.</span><span class="sxs-lookup"><span data-stu-id="e0a17-118">The immutable tenant ID of the domain, such as `72f988bf-86f1-41af-91ab-2d7cd011db45`.</span></span>

<span data-ttu-id="e0a17-119">För **klient oberoende slutpunkter**, `TenantDomainName` är `common`.</span><span class="sxs-lookup"><span data-stu-id="e0a17-119">For **tenant-independent endpoints**, the `TenantDomainName` is `common`.</span></span> <span data-ttu-id="e0a17-120">Det här dokumentet innehåller endast Federationsmetadata element som är gemensamma för alla Azure AD-klienter som finns på login.microsoftonline.com.</span><span class="sxs-lookup"><span data-stu-id="e0a17-120">This document lists only the Federation Metadata elements that are common to all Azure AD tenants that are hosted at login.microsoftonline.com.</span></span>

<span data-ttu-id="e0a17-121">En klient-specifika slutpunkt kan till exempel vara `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`.</span><span class="sxs-lookup"><span data-stu-id="e0a17-121">For example, a tenant-specific endpoint might be `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`.</span></span> <span data-ttu-id="e0a17-122">Oberoende av klient-slutpunkten är [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml).</span><span class="sxs-lookup"><span data-stu-id="e0a17-122">The tenant-independent endpoint is [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml).</span></span> <span data-ttu-id="e0a17-123">Du kan visa dokumentet federation metadata genom att ange den här URL i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="e0a17-123">You can view the federation metadata document by typing this URL in a browser.</span></span>

## <a name="contents-of-federation-metadata"></a><span data-ttu-id="e0a17-124">Innehållet i federation Metadata</span><span class="sxs-lookup"><span data-stu-id="e0a17-124">Contents of federation Metadata</span></span>
<span data-ttu-id="e0a17-125">Följande avsnitt innehåller information som krävs av tjänster som använder de token som utfärdats av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e0a17-125">The following section provides information needed by services that consume the tokens issued by Azure AD.</span></span>

### <a name="entity-id"></a><span data-ttu-id="e0a17-126">Enhets-ID</span><span class="sxs-lookup"><span data-stu-id="e0a17-126">Entity ID</span></span>
<span data-ttu-id="e0a17-127">Den `EntityDescriptor` elementet innehåller ett `EntityID` attribut.</span><span class="sxs-lookup"><span data-stu-id="e0a17-127">The `EntityDescriptor` element contains an `EntityID` attribute.</span></span> <span data-ttu-id="e0a17-128">Värdet för den `EntityID` attributet representerar utfärdaren, det vill säga säkerhetstokentjänsten (STS) som utfärdade token.</span><span class="sxs-lookup"><span data-stu-id="e0a17-128">The value of the `EntityID` attribute represents the issuer, that is, the security token service (STS) that issued the token.</span></span> <span data-ttu-id="e0a17-129">Det är viktigt att verifiera utfärdaren när du får en token.</span><span class="sxs-lookup"><span data-stu-id="e0a17-129">It is important to validate the issuer when you receive a token.</span></span>

<span data-ttu-id="e0a17-130">Följande metadata visas ett exempel på klient-specifika `EntityDescriptor` element med ett `EntityID` element.</span><span class="sxs-lookup"><span data-stu-id="e0a17-130">The following metadata shows a sample tenant-specific `EntityDescriptor` element with an `EntityID` element.</span></span>

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
<span data-ttu-id="e0a17-131">Du kan ersätta klient-ID i slutpunkten oberoende av klienten med klient-ID för att skapa en klient-specifika `EntityID` värde.</span><span class="sxs-lookup"><span data-stu-id="e0a17-131">You can replace the tenant ID in the tenant-independent endpoint with your tenant ID to create a tenant-specific `EntityID` value.</span></span> <span data-ttu-id="e0a17-132">Det resulterande värdet ska vara samma som token utfärdaren.</span><span class="sxs-lookup"><span data-stu-id="e0a17-132">The resulting value will be the same as the token issuer.</span></span> <span data-ttu-id="e0a17-133">Strategin kan ett program för flera innehavare att verifiera utgivaren för en viss klient.</span><span class="sxs-lookup"><span data-stu-id="e0a17-133">The strategy allows a multi-tenant application to validate the issuer for a given tenant.</span></span>

<span data-ttu-id="e0a17-134">Följande metadata visas ett exempel på klient-oberoende `EntityID` element.</span><span class="sxs-lookup"><span data-stu-id="e0a17-134">The following metadata shows a sample tenant-independent `EntityID` element.</span></span> <span data-ttu-id="e0a17-135">Observera, att den `{tenant}` är en literal, inte en platshållare.</span><span class="sxs-lookup"><span data-stu-id="e0a17-135">Please note, that the `{tenant}` is a literal, not a placeholder.</span></span>

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a><span data-ttu-id="e0a17-136">Certifikat för tokensignering</span><span class="sxs-lookup"><span data-stu-id="e0a17-136">Token signing certificates</span></span>
<span data-ttu-id="e0a17-137">När en tjänst tar emot en token som utfärdas av en Azure AD-klient, verifieras signaturen för token med en signeringsnyckel som publiceras i federation Metadatadokumentet.</span><span class="sxs-lookup"><span data-stu-id="e0a17-137">When a service receives a token that is issued by a Azure AD tenant, the signature of the token must be validated with a signing key that is published in the federation metadata document.</span></span> <span data-ttu-id="e0a17-138">Federationsmetadata innehåller den offentliga delen av de certifikat som klienterna använder för tokensignering.</span><span class="sxs-lookup"><span data-stu-id="e0a17-138">The federation metadata includes the public portion of the certificates that the tenants use for token signing.</span></span> <span data-ttu-id="e0a17-139">Certifikatet rå byte visas i den `KeyDescriptor` element.</span><span class="sxs-lookup"><span data-stu-id="e0a17-139">The certificate raw bytes appear in the `KeyDescriptor` element.</span></span> <span data-ttu-id="e0a17-140">Certifikatet för tokensignering är giltig för signering endast när värdet för den `use` attributet `signing`.</span><span class="sxs-lookup"><span data-stu-id="e0a17-140">The token signing certificate is valid for signing only when the value of the `use` attribute is `signing`.</span></span>

<span data-ttu-id="e0a17-141">Ett federation metadata dokument som publicerats av Azure AD kan ha flera nycklar för signering, till exempel när Azure AD förbereds att uppdatera signeringscertifikatet.</span><span class="sxs-lookup"><span data-stu-id="e0a17-141">A federation metadata document published by Azure AD can have multiple signing keys, such as when Azure AD is preparing to update the signing certificate.</span></span> <span data-ttu-id="e0a17-142">När ett dokument för federation metadata innehåller fler än ett certifikat är en tjänst som validerar token som ska ha stöd för alla certifikat i dokumentet.</span><span class="sxs-lookup"><span data-stu-id="e0a17-142">When a federation metadata document includes more than one certificate, a service that is validating the tokens should support all certificates in the document.</span></span>

<span data-ttu-id="e0a17-143">Följande metadata visas ett exempel på `KeyDescriptor` element med ett signeringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="e0a17-143">The following metadata shows a sample `KeyDescriptor` element with a signing key.</span></span>

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

<span data-ttu-id="e0a17-144">Den `KeyDescriptor` elementet visas på två platser i dokumentet federation metadata; i WS-Federation-specifik avsnitten och SAML-specifika.</span><span class="sxs-lookup"><span data-stu-id="e0a17-144">The `KeyDescriptor` element appears in two places in the federation metadata document; in the WS-Federation-specific section and the SAML-specific section.</span></span> <span data-ttu-id="e0a17-145">De certifikat som publicerats i båda avsnitten ska vara samma.</span><span class="sxs-lookup"><span data-stu-id="e0a17-145">The certificates published in both sections will be the same.</span></span>

<span data-ttu-id="e0a17-146">I avsnittet WS-Federation-specifik en läsare för WS-Federation metadata läser certifikat från en `RoleDescriptor` element med den `SecurityTokenServiceType` typen.</span><span class="sxs-lookup"><span data-stu-id="e0a17-146">In the WS-Federation-specific section, a WS-Federation metadata reader would read the certificates from a `RoleDescriptor` element with the `SecurityTokenServiceType` type.</span></span>

<span data-ttu-id="e0a17-147">Följande metadata visas ett exempel på `RoleDescriptor` element.</span><span class="sxs-lookup"><span data-stu-id="e0a17-147">The following metadata shows a sample `RoleDescriptor` element.</span></span>

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

<span data-ttu-id="e0a17-148">I avsnittet SAML-specifika en läsare för WS-Federation metadata läser certifikat från en `IDPSSODescriptor` element.</span><span class="sxs-lookup"><span data-stu-id="e0a17-148">In the SAML-specific section, a WS-Federation metadata reader would read the certificates from a `IDPSSODescriptor` element.</span></span>

<span data-ttu-id="e0a17-149">Följande metadata visas ett exempel på `IDPSSODescriptor` element.</span><span class="sxs-lookup"><span data-stu-id="e0a17-149">The following metadata shows a sample `IDPSSODescriptor` element.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
<span data-ttu-id="e0a17-150">Det finns inga skillnader i format för klient-specifika och klient oberoende certifikat.</span><span class="sxs-lookup"><span data-stu-id="e0a17-150">There are no differences in the format of tenant-specific and tenant-independent certificates.</span></span>

### <a name="ws-federation-endpoint-url"></a><span data-ttu-id="e0a17-151">WS-Federation slutpunkts-URL</span><span class="sxs-lookup"><span data-stu-id="e0a17-151">WS-Federation endpoint URL</span></span>
<span data-ttu-id="e0a17-152">Federationsmetadata innehåller den URL som använder Azure AD för enkel inloggning och enskild utloggning i WS-Federation-protokollet.</span><span class="sxs-lookup"><span data-stu-id="e0a17-152">The federation metadata includes the URL that is Azure AD uses for single sign-in and single sign-out in WS-Federation protocol.</span></span> <span data-ttu-id="e0a17-153">Den här slutpunkten som visas i den `PassiveRequestorEndpoint` element.</span><span class="sxs-lookup"><span data-stu-id="e0a17-153">This endpoint appears in the `PassiveRequestorEndpoint` element.</span></span>

<span data-ttu-id="e0a17-154">Följande metadata visas ett exempel på `PassiveRequestorEndpoint` element för en slutpunkt för innehavare.</span><span class="sxs-lookup"><span data-stu-id="e0a17-154">The following metadata shows a sample `PassiveRequestorEndpoint` element for a tenant-specific endpoint.</span></span>

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
<span data-ttu-id="e0a17-155">För oberoende av klient-slutpunkten WS-Federation-URL som visas i WS-Federation-slutpunkt som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="e0a17-155">For the tenant-independent endpoint, the WS-Federation URL appears in the WS-Federation endpoint, as shown in the following sample.</span></span>

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a><span data-ttu-id="e0a17-156">Slutpunkts-URL för SAML-protokoll</span><span class="sxs-lookup"><span data-stu-id="e0a17-156">SAML protocol endpoint URL</span></span>
<span data-ttu-id="e0a17-157">Federationsmetadata innehåller URL: en där Azure AD för enkel inloggning och enskild utloggning i SAML 2.0-protokollet.</span><span class="sxs-lookup"><span data-stu-id="e0a17-157">The federation metadata includes the URL that Azure AD uses for single sign-in and single sign-out in SAML 2.0 protocol.</span></span> <span data-ttu-id="e0a17-158">Dessa slutpunkter som visas i den `IDPSSODescriptor` element.</span><span class="sxs-lookup"><span data-stu-id="e0a17-158">These endpoints appear in the `IDPSSODescriptor` element.</span></span>

<span data-ttu-id="e0a17-159">URL: er med inloggning och utloggning visas i den `SingleSignOnService` och `SingleLogoutService` element.</span><span class="sxs-lookup"><span data-stu-id="e0a17-159">The sign-in and sign-out URLs appear in the `SingleSignOnService` and `SingleLogoutService` elements.</span></span>

<span data-ttu-id="e0a17-160">Följande metadata visas ett exempel på `PassiveResistorEndpoint` för en slutpunkt för innehavare.</span><span class="sxs-lookup"><span data-stu-id="e0a17-160">The following metadata shows a sample `PassiveResistorEndpoint` for a tenant-specific endpoint.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

<span data-ttu-id="e0a17-161">Slutpunkterna för vanliga SAML 2.0-protokollet slutpunkter publiceras på samma sätt i klient-oberoende federationsmetadata, som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="e0a17-161">Similarly the endpoints for the common SAML 2.0 protocol endpoints are published in the tenant-independent federation metadata, as shown in the following sample.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
