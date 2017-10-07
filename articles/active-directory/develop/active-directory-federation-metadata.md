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
# <a name="federation-metadata"></a><span data-ttu-id="d8688-103">Federationsmetadata</span><span class="sxs-lookup"><span data-stu-id="d8688-103">Federation metadata</span></span>
<span data-ttu-id="d8688-104">Azure Active Directory (AD Azure) publicerar ett dokument för federation metadata för tjänster som är konfigurerade tooaccept hello säkerhetstoken som Azure AD utfärdar.</span><span class="sxs-lookup"><span data-stu-id="d8688-104">Azure Active Directory (Azure AD) publishes a federation metadata document for services that is configured tooaccept hello security tokens that Azure AD issues.</span></span> <span data-ttu-id="d8688-105">hello federation metadata dokumentformat beskrivs i hello [Web Services Federation Language (WS-Federation) Version 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), som utökar [Metadata för hello OASIS Security Assertion Markup Language (SAML) v2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).</span><span class="sxs-lookup"><span data-stu-id="d8688-105">hello federation metadata document format is described in hello [Web Services Federation Language (WS-Federation) Version 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), which extends [Metadata for hello OASIS Security Assertion Markup Language (SAML) v2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).</span></span>

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a><span data-ttu-id="d8688-106">Klient-specifika och klient oberoende Metadataslutpunkter</span><span class="sxs-lookup"><span data-stu-id="d8688-106">Tenant-specific and Tenant-independent metadata endpoints</span></span>
<span data-ttu-id="d8688-107">Azure AD publicerar klient-specifika och klient oberoende slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="d8688-107">Azure AD publishes tenant-specific and tenant-independent endpoints.</span></span>

<span data-ttu-id="d8688-108">Klient-specifika slutpunkter är utformade för en viss klient.</span><span class="sxs-lookup"><span data-stu-id="d8688-108">Tenant-specific endpoints are designed for a particular tenant.</span></span> <span data-ttu-id="d8688-109">hello klient-specifika federationsmetadata innehåller information om hello-klienten, inklusive klient-specifika utfärdare och slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="d8688-109">hello tenant-specific federation metadata includes information about hello tenant, including tenant-specific issuer and endpoint information.</span></span> <span data-ttu-id="d8688-110">Program som begränsar åtkomst tooa enda innehavare använda klient-specifika slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="d8688-110">Applications that restrict access tooa single tenant use tenant-specific endpoints.</span></span>

<span data-ttu-id="d8688-111">Klient-oberoende slutpunkter ger information som är vanliga tooall Azure AD-klienter.</span><span class="sxs-lookup"><span data-stu-id="d8688-111">Tenant-independent endpoints provide information that is common tooall Azure AD tenants.</span></span> <span data-ttu-id="d8688-112">Den här informationen gäller tootenants som värd *login.microsoftonline.com* och delas av klienter.</span><span class="sxs-lookup"><span data-stu-id="d8688-112">This information applies tootenants hosted at *login.microsoftonline.com* and is shared across tenants.</span></span> <span data-ttu-id="d8688-113">Klient-oberoende slutpunkter rekommenderas för program med flera klienter, eftersom de inte är associerade med viss innehavare.</span><span class="sxs-lookup"><span data-stu-id="d8688-113">Tenant-independent endpoints are recommended for multi-tenant applications, since they are not associated with any particular tenant.</span></span>

## <a name="federation-metadata-endpoints"></a><span data-ttu-id="d8688-114">Federation Metadataslutpunkter</span><span class="sxs-lookup"><span data-stu-id="d8688-114">Federation metadata endpoints</span></span>
<span data-ttu-id="d8688-115">Azure AD publicerar federationsmetadata på `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.</span><span class="sxs-lookup"><span data-stu-id="d8688-115">Azure AD publishes federation metadata at `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.</span></span>

<span data-ttu-id="d8688-116">För **klient-specifika slutpunkter**, hello `TenantDomainName` kan vara något av följande typer hello:</span><span class="sxs-lookup"><span data-stu-id="d8688-116">For **tenant-specific endpoints**, hello `TenantDomainName` can be one of hello following types:</span></span>

* <span data-ttu-id="d8688-117">Ett registrerat domännamn för en Azure AD-klient, t.ex: `contoso.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="d8688-117">A registered domain name of an Azure AD tenant, such as: `contoso.onmicrosoft.com`.</span></span>
* <span data-ttu-id="d8688-118">hello ändras klient-ID för hello domän, som `72f988bf-86f1-41af-91ab-2d7cd011db45`.</span><span class="sxs-lookup"><span data-stu-id="d8688-118">hello immutable tenant ID of hello domain, such as `72f988bf-86f1-41af-91ab-2d7cd011db45`.</span></span>

<span data-ttu-id="d8688-119">För **klient oberoende slutpunkter**, hello `TenantDomainName` är `common`.</span><span class="sxs-lookup"><span data-stu-id="d8688-119">For **tenant-independent endpoints**, hello `TenantDomainName` is `common`.</span></span> <span data-ttu-id="d8688-120">Det här dokumentet innehåller endast hello Federationsmetadata-element som är vanliga tooall Azure AD-klienter som finns på login.microsoftonline.com.</span><span class="sxs-lookup"><span data-stu-id="d8688-120">This document lists only hello Federation Metadata elements that are common tooall Azure AD tenants that are hosted at login.microsoftonline.com.</span></span>

<span data-ttu-id="d8688-121">En klient-specifika slutpunkt kan till exempel vara `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`.</span><span class="sxs-lookup"><span data-stu-id="d8688-121">For example, a tenant-specific endpoint might be `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`.</span></span> <span data-ttu-id="d8688-122">hello oberoende av klient-slutpunkten är [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml).</span><span class="sxs-lookup"><span data-stu-id="d8688-122">hello tenant-independent endpoint is [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml).</span></span> <span data-ttu-id="d8688-123">Du kan visa hello federation Metadatadokumentet genom att ange den här URL i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="d8688-123">You can view hello federation metadata document by typing this URL in a browser.</span></span>

## <a name="contents-of-federation-metadata"></a><span data-ttu-id="d8688-124">Innehållet i federation Metadata</span><span class="sxs-lookup"><span data-stu-id="d8688-124">Contents of federation Metadata</span></span>
<span data-ttu-id="d8688-125">hello innehåller följande avsnitt information som krävs av tjänster som använder hello-token som utfärdats av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d8688-125">hello following section provides information needed by services that consume hello tokens issued by Azure AD.</span></span>

### <a name="entity-id"></a><span data-ttu-id="d8688-126">Enhets-ID</span><span class="sxs-lookup"><span data-stu-id="d8688-126">Entity ID</span></span>
<span data-ttu-id="d8688-127">Hej `EntityDescriptor` elementet innehåller ett `EntityID` attribut.</span><span class="sxs-lookup"><span data-stu-id="d8688-127">hello `EntityDescriptor` element contains an `EntityID` attribute.</span></span> <span data-ttu-id="d8688-128">Hej värdet för hello `EntityID` attributet representerar hello utfärdaren, det vill säga token hello säkerhet tjänsten (STS) som utfärdade hello-token.</span><span class="sxs-lookup"><span data-stu-id="d8688-128">hello value of hello `EntityID` attribute represents hello issuer, that is, hello security token service (STS) that issued hello token.</span></span> <span data-ttu-id="d8688-129">Det är viktigt toovalidate hello utfärdaren när du får en token.</span><span class="sxs-lookup"><span data-stu-id="d8688-129">It is important toovalidate hello issuer when you receive a token.</span></span>

<span data-ttu-id="d8688-130">hello följande metadata visas ett exempel på klient-specifika `EntityDescriptor` element med ett `EntityID` element.</span><span class="sxs-lookup"><span data-stu-id="d8688-130">hello following metadata shows a sample tenant-specific `EntityDescriptor` element with an `EntityID` element.</span></span>

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
<span data-ttu-id="d8688-131">Du kan ersätta hello klient-ID i hello klient oberoende slutpunkt med din klient-ID toocreate klient-specifika `EntityID` värde.</span><span class="sxs-lookup"><span data-stu-id="d8688-131">You can replace hello tenant ID in hello tenant-independent endpoint with your tenant ID toocreate a tenant-specific `EntityID` value.</span></span> <span data-ttu-id="d8688-132">hello resultatvärdet kommer hello samma som hello token utfärdare.</span><span class="sxs-lookup"><span data-stu-id="d8688-132">hello resulting value will be hello same as hello token issuer.</span></span> <span data-ttu-id="d8688-133">hello kan ett program med flera innehavare toovalidate hello utfärdaren för en viss klient.</span><span class="sxs-lookup"><span data-stu-id="d8688-133">hello strategy allows a multi-tenant application toovalidate hello issuer for a given tenant.</span></span>

<span data-ttu-id="d8688-134">hello följande metadata visas ett exempel på klient-oberoende `EntityID` element.</span><span class="sxs-lookup"><span data-stu-id="d8688-134">hello following metadata shows a sample tenant-independent `EntityID` element.</span></span> <span data-ttu-id="d8688-135">Observera att hello `{tenant}` är en literal, inte en platshållare.</span><span class="sxs-lookup"><span data-stu-id="d8688-135">Please note, that hello `{tenant}` is a literal, not a placeholder.</span></span>

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a><span data-ttu-id="d8688-136">Certifikat för tokensignering</span><span class="sxs-lookup"><span data-stu-id="d8688-136">Token signing certificates</span></span>
<span data-ttu-id="d8688-137">När en tjänst tar emot en token som utfärdas av en Azure AD-klient, måste hello signaturen för hello token verifieras med en signeringsnyckel som publiceras i hello federation Metadatadokumentet.</span><span class="sxs-lookup"><span data-stu-id="d8688-137">When a service receives a token that is issued by a Azure AD tenant, hello signature of hello token must be validated with a signing key that is published in hello federation metadata document.</span></span> <span data-ttu-id="d8688-138">Hej federationsmetadata innehåller hello offentliga delen av hello-certifikat som hello-klienter använder för tokensignering.</span><span class="sxs-lookup"><span data-stu-id="d8688-138">hello federation metadata includes hello public portion of hello certificates that hello tenants use for token signing.</span></span> <span data-ttu-id="d8688-139">hello certifikat byte visas i hello `KeyDescriptor` element.</span><span class="sxs-lookup"><span data-stu-id="d8688-139">hello certificate raw bytes appear in hello `KeyDescriptor` element.</span></span> <span data-ttu-id="d8688-140">hello certifikatet för tokensignering är giltigt för signering endast när hello värdet för hello `use` attributet `signing`.</span><span class="sxs-lookup"><span data-stu-id="d8688-140">hello token signing certificate is valid for signing only when hello value of hello `use` attribute is `signing`.</span></span>

<span data-ttu-id="d8688-141">Ett federation metadata dokument som publicerats av Azure AD kan ha flera nycklar för signering, till exempel när Azure AD förbereder tooupdate hello signeringscertifikat.</span><span class="sxs-lookup"><span data-stu-id="d8688-141">A federation metadata document published by Azure AD can have multiple signing keys, such as when Azure AD is preparing tooupdate hello signing certificate.</span></span> <span data-ttu-id="d8688-142">När ett dokument för federation metadata innehåller fler än ett certifikat är en tjänst som verifieras hello token ska ha stöd för alla certifikat i hello dokumentet.</span><span class="sxs-lookup"><span data-stu-id="d8688-142">When a federation metadata document includes more than one certificate, a service that is validating hello tokens should support all certificates in hello document.</span></span>

<span data-ttu-id="d8688-143">hello följande metadata visas ett exempel på `KeyDescriptor` element med ett signeringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="d8688-143">hello following metadata shows a sample `KeyDescriptor` element with a signing key.</span></span>

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

<span data-ttu-id="d8688-144">Hej `KeyDescriptor` elementet visas på två platser i hello federation Metadatadokumentet; i hello WS-Federation-specifik och hello SAML-specifika avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d8688-144">hello `KeyDescriptor` element appears in two places in hello federation metadata document; in hello WS-Federation-specific section and hello SAML-specific section.</span></span> <span data-ttu-id="d8688-145">hello-certifikat som publicerats för båda avsnitten kommer hello samma.</span><span class="sxs-lookup"><span data-stu-id="d8688-145">hello certificates published in both sections will be hello same.</span></span>

<span data-ttu-id="d8688-146">Hello WS-Federation-specifik under en läsare för WS-Federation metadata läser hello certifikat från en `RoleDescriptor` element med hello `SecurityTokenServiceType` typen.</span><span class="sxs-lookup"><span data-stu-id="d8688-146">In hello WS-Federation-specific section, a WS-Federation metadata reader would read hello certificates from a `RoleDescriptor` element with hello `SecurityTokenServiceType` type.</span></span>

<span data-ttu-id="d8688-147">hello följande metadata visas ett exempel på `RoleDescriptor` element.</span><span class="sxs-lookup"><span data-stu-id="d8688-147">hello following metadata shows a sample `RoleDescriptor` element.</span></span>

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

<span data-ttu-id="d8688-148">Hello SAML-specifika under en läsare för WS-Federation metadata läser hello certifikat från en `IDPSSODescriptor` element.</span><span class="sxs-lookup"><span data-stu-id="d8688-148">In hello SAML-specific section, a WS-Federation metadata reader would read hello certificates from a `IDPSSODescriptor` element.</span></span>

<span data-ttu-id="d8688-149">hello följande metadata visas ett exempel på `IDPSSODescriptor` element.</span><span class="sxs-lookup"><span data-stu-id="d8688-149">hello following metadata shows a sample `IDPSSODescriptor` element.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
<span data-ttu-id="d8688-150">Det finns inga skillnader i hello-format för klient-specifika och klient oberoende certifikat.</span><span class="sxs-lookup"><span data-stu-id="d8688-150">There are no differences in hello format of tenant-specific and tenant-independent certificates.</span></span>

### <a name="ws-federation-endpoint-url"></a><span data-ttu-id="d8688-151">WS-Federation slutpunkts-URL</span><span class="sxs-lookup"><span data-stu-id="d8688-151">WS-Federation endpoint URL</span></span>
<span data-ttu-id="d8688-152">Hej federationsmetadata innehåller hello URL som är Azure AD används för enkel inloggning och enkel utloggning i WS-Federation-protokollet.</span><span class="sxs-lookup"><span data-stu-id="d8688-152">hello federation metadata includes hello URL that is Azure AD uses for single sign-in and single sign-out in WS-Federation protocol.</span></span> <span data-ttu-id="d8688-153">Den här slutpunkten som visas i hello `PassiveRequestorEndpoint` element.</span><span class="sxs-lookup"><span data-stu-id="d8688-153">This endpoint appears in hello `PassiveRequestorEndpoint` element.</span></span>

<span data-ttu-id="d8688-154">hello följande metadata visas ett exempel på `PassiveRequestorEndpoint` element för en slutpunkt för innehavare.</span><span class="sxs-lookup"><span data-stu-id="d8688-154">hello following metadata shows a sample `PassiveRequestorEndpoint` element for a tenant-specific endpoint.</span></span>

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
<span data-ttu-id="d8688-155">För hello klient oberoende slutpunkten hello WS-Federation-URL som visas i hello WS-Federation-slutpunkt som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="d8688-155">For hello tenant-independent endpoint, hello WS-Federation URL appears in hello WS-Federation endpoint, as shown in hello following sample.</span></span>

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a><span data-ttu-id="d8688-156">Slutpunkts-URL för SAML-protokoll</span><span class="sxs-lookup"><span data-stu-id="d8688-156">SAML protocol endpoint URL</span></span>
<span data-ttu-id="d8688-157">Hej federationsmetadata innehåller hello URL där Azure AD för enkel inloggning och enskild utloggning i SAML 2.0-protokollet.</span><span class="sxs-lookup"><span data-stu-id="d8688-157">hello federation metadata includes hello URL that Azure AD uses for single sign-in and single sign-out in SAML 2.0 protocol.</span></span> <span data-ttu-id="d8688-158">Dessa slutpunkter som visas i hello `IDPSSODescriptor` element.</span><span class="sxs-lookup"><span data-stu-id="d8688-158">These endpoints appear in hello `IDPSSODescriptor` element.</span></span>

<span data-ttu-id="d8688-159">Hej inloggning och utloggning URL: er som visas i hello `SingleSignOnService` och `SingleLogoutService` element.</span><span class="sxs-lookup"><span data-stu-id="d8688-159">hello sign-in and sign-out URLs appear in hello `SingleSignOnService` and `SingleLogoutService` elements.</span></span>

<span data-ttu-id="d8688-160">hello följande metadata visas ett exempel på `PassiveResistorEndpoint` för en slutpunkt för innehavare.</span><span class="sxs-lookup"><span data-stu-id="d8688-160">hello following metadata shows a sample `PassiveResistorEndpoint` for a tenant-specific endpoint.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

<span data-ttu-id="d8688-161">På liknande sätt publiceras hello slutpunkter för hello vanliga SAML 2.0-protokollet slutpunkter i hello klient oberoende federationsmetadata, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="d8688-161">Similarly hello endpoints for hello common SAML 2.0 protocol endpoints are published in hello tenant-independent federation metadata, as shown in hello following sample.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
