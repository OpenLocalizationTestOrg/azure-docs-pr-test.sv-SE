---
title: Certifikat autentiseringsuppgifter i Azure AD | Microsoft Docs
description: "Den här artikeln beskrivs registrering och användning av autentiseringsuppgifter för certifikat för autentisering"
services: active-directory
documentationcenter: .net
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 88f0c64a-25f7-4974-aca2-2acadc9acbd8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 08bb5140bb35bbd120aaa506afeab8ad247f81e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="certificate-credentials-for-application-authentication"></a><span data-ttu-id="4ee9e-103">Autentiseringsuppgifter för certifikat för autentisering</span><span class="sxs-lookup"><span data-stu-id="4ee9e-103">Certificate credentials for application authentication</span></span>

<span data-ttu-id="4ee9e-104">Azure Active Directory kan ett program att använda sina egna autentiseringsuppgifter för autentisering, till exempel i flödet för OAuth 2.0 klientens autentiseringsuppgifter Grant och On-Behalf-Of-flöde.</span><span class="sxs-lookup"><span data-stu-id="4ee9e-104">Azure Active Directory allows an application to use its own credentials for authentication, for example, in the OAuth 2.0 Client Credentials Grant flow and the On-Behalf-Of flow.</span></span>
<span data-ttu-id="4ee9e-105">En form av autentiseringsuppgifter som kan användas är en JSON Web Token(JWT) assertion signeras med ett certifikat som programmet äger.</span><span class="sxs-lookup"><span data-stu-id="4ee9e-105">One form of credential that can be used is a JSON Web Token(JWT) assertion signed with a certificate that the application owns.</span></span>

## <a name="format-of-the-assertion"></a><span data-ttu-id="4ee9e-106">Formatet på kontrollen</span><span class="sxs-lookup"><span data-stu-id="4ee9e-106">Format of the assertion</span></span>
<span data-ttu-id="4ee9e-107">Om du vill beräkna kontrollen, vill du förmodligen använda en av många [JSON Web Token](https://jwt.io/) bibliotek på önskat språk.</span><span class="sxs-lookup"><span data-stu-id="4ee9e-107">To compute the assertion, you probably want to use one of the many [JSON Web Token](https://jwt.io/) libraries in the language of your choice.</span></span> <span data-ttu-id="4ee9e-108">Informationen som token är:</span><span class="sxs-lookup"><span data-stu-id="4ee9e-108">The information carried by the token is:</span></span>

#### <a name="header"></a><span data-ttu-id="4ee9e-109">Huvudet</span><span class="sxs-lookup"><span data-stu-id="4ee9e-109">Header</span></span>

| <span data-ttu-id="4ee9e-110">Parameter</span><span class="sxs-lookup"><span data-stu-id="4ee9e-110">Parameter</span></span> |  <span data-ttu-id="4ee9e-111">Kommentar</span><span class="sxs-lookup"><span data-stu-id="4ee9e-111">Remark</span></span> |
| --- | --- | --- |
| `alg` | <span data-ttu-id="4ee9e-112">Bör vara **RS256**</span><span class="sxs-lookup"><span data-stu-id="4ee9e-112">Should be **RS256**</span></span> |
| `typ` | <span data-ttu-id="4ee9e-113">Bör vara **JWT**</span><span class="sxs-lookup"><span data-stu-id="4ee9e-113">Should be **JWT**</span></span> |
| `x5t` | <span data-ttu-id="4ee9e-114">Ska vara tumavtrycket för X.509-certifikat SHA-1</span><span class="sxs-lookup"><span data-stu-id="4ee9e-114">Should be the X.509 Certificate SHA-1 thumbprint</span></span> |

#### <a name="claims-payload"></a><span data-ttu-id="4ee9e-115">Anspråk (Payload)</span><span class="sxs-lookup"><span data-stu-id="4ee9e-115">Claims (Payload)</span></span>

| <span data-ttu-id="4ee9e-116">Parameter</span><span class="sxs-lookup"><span data-stu-id="4ee9e-116">Parameter</span></span> |  <span data-ttu-id="4ee9e-117">Kommentar</span><span class="sxs-lookup"><span data-stu-id="4ee9e-117">Remark</span></span> |
| --- | --- | --- |
| `aud` | <span data-ttu-id="4ee9e-118">Målgrupp: Bör vara  **https://login.microsoftonline.com/*tenant_Id*  /oauth2/token **</span><span class="sxs-lookup"><span data-stu-id="4ee9e-118">Audience: Should be **https://login.microsoftonline.com/*tenant_Id*/oauth2/token**</span></span> |
| `exp` | <span data-ttu-id="4ee9e-119">Förfallodatum: det datum då token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="4ee9e-119">Expiration date: the date when the token expires.</span></span> <span data-ttu-id="4ee9e-120">Tiden representeras som antalet sekunder från den 1 januari 1970 (1970-01-01T0:0:0Z) UTC tills giltigheten token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="4ee9e-120">The time is represented as the number of seconds from January 1, 1970 (1970-01-01T0:0:0Z) UTC until the time the token validity expires.</span></span>|
| `iss` | <span data-ttu-id="4ee9e-121">Utgivare: bör vara client_id (program-Id för klient-tjänst)</span><span class="sxs-lookup"><span data-stu-id="4ee9e-121">Issuer: should be the client_id (Application Id of the client service)</span></span> |
| `jti` | <span data-ttu-id="4ee9e-122">GUID: JWT ID</span><span class="sxs-lookup"><span data-stu-id="4ee9e-122">GUID: the JWT ID</span></span> |
| `nbf` | <span data-ttu-id="4ee9e-123">Inte före: datum före vilken token inte kan användas.</span><span class="sxs-lookup"><span data-stu-id="4ee9e-123">Not Before: the date before which the token cannot be used.</span></span> <span data-ttu-id="4ee9e-124">Tiden representeras som antalet sekunder från den 1 januari 1970 (1970-01-01T0:0:0Z) UTC tills token har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="4ee9e-124">The time is represented as the number of seconds from January 1, 1970 (1970-01-01T0:0:0Z) UTC until the time the token was issued.</span></span> |
| `sub` | <span data-ttu-id="4ee9e-125">Ämne: som för `iss`, bör vara client_id (program-Id för klient-tjänst)</span><span class="sxs-lookup"><span data-stu-id="4ee9e-125">Subject: As for `iss`, should be the client_id (Application Id of the client service)</span></span> |

#### <a name="signature"></a><span data-ttu-id="4ee9e-126">Signatur</span><span class="sxs-lookup"><span data-stu-id="4ee9e-126">Signature</span></span>
<span data-ttu-id="4ee9e-127">Signaturen beräknas tillämpa certifikatet som beskrivs i den [JSON Web Token RFC7519 specifikation](https://tools.ietf.org/html/rfc7519)</span><span class="sxs-lookup"><span data-stu-id="4ee9e-127">The signature is computed applying the certificate as described in the [JSON Web Token RFC7519 specification](https://tools.ietf.org/html/rfc7519)</span></span>

### <a name="example-of-a-decoded-jwt-assertion"></a><span data-ttu-id="4ee9e-128">Exempel på en avkodade JWT-kontrollen</span><span class="sxs-lookup"><span data-stu-id="4ee9e-128">Example of a decoded JWT assertion</span></span>
```
{
  "alg": "RS256",
  "typ": "JWT",
  "x5t": "gx8tGysyjcRqKjFPnd7RFwvwZI0"
}
.
{
  "aud": "https: //login.microsoftonline.com/contoso.onmicrosoft.com/oauth2/token",
  "exp": 1484593341,
  "iss": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05",
  "jti": "22b3bb26-e046-42df-9c96-65dbd72c1c81",
  "nbf": 1484592741,  
  "sub": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05"
}
.
"Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"

```

### <a name="example-of-an-encoded-jwt-assertion"></a><span data-ttu-id="4ee9e-129">Exempel på en kodad JWT-kontrollen</span><span class="sxs-lookup"><span data-stu-id="4ee9e-129">Example of an encoded JWT assertion</span></span>
<span data-ttu-id="4ee9e-130">Följande sträng är ett exempel på kodade kontrollen.</span><span class="sxs-lookup"><span data-stu-id="4ee9e-130">The following string is an example of encoded assertion.</span></span> <span data-ttu-id="4ee9e-131">Om du ser noggrant kan se du tre avsnitt avgränsade med punkter (.).</span><span class="sxs-lookup"><span data-stu-id="4ee9e-131">If you look carefully, you notice three sections separated by dots (.).</span></span>
<span data-ttu-id="4ee9e-132">Det första avsnittet kodar rubriken, den andra nyttolasten och sist är signaturen som beräknats med certifikat från innehållet i de två första avsnitten.</span><span class="sxs-lookup"><span data-stu-id="4ee9e-132">The first section encodes the header, the second the payload, and the last is the signature computed with the certificates from the content of the first two sections.</span></span>
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

### <a name="register-your-certificate-with-azure-ad"></a><span data-ttu-id="4ee9e-133">Registrera ditt certifikat med Azure AD</span><span class="sxs-lookup"><span data-stu-id="4ee9e-133">Register your certificate with Azure AD</span></span>
<span data-ttu-id="4ee9e-134">Om du vill associera certifikat-autentiseringsuppgifter med klientprogrammet i Azure AD, måste du redigera programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="4ee9e-134">To associate the certificate credential with the client application in Azure AD, you need to edit the application manifest.</span></span>
<span data-ttu-id="4ee9e-135">Med undantag för ett certifikat, måste du beräkna:</span><span class="sxs-lookup"><span data-stu-id="4ee9e-135">Having hold of a certificate, you need to compute:</span></span>
- <span data-ttu-id="4ee9e-136">`$base64Thumbprint`, vilket är base64-kodning av certifikat-Hash</span><span class="sxs-lookup"><span data-stu-id="4ee9e-136">`$base64Thumbprint`, which is the base64 encoding of the certificate Hash</span></span>
- <span data-ttu-id="4ee9e-137">`$base64Value`, vilket är base64-kodning av certifikatets rådata</span><span class="sxs-lookup"><span data-stu-id="4ee9e-137">`$base64Value`, which is the base64 encoding of the certificate raw data</span></span>

<span data-ttu-id="4ee9e-138">Du måste också ange ett GUID för att identifiera nyckeln i programmanifestet (`$keyId`)</span><span class="sxs-lookup"><span data-stu-id="4ee9e-138">you also need to provide a GUID to identify the key in the application manifest (`$keyId`)</span></span>

<span data-ttu-id="4ee9e-139">Öppna programmanifestet i Azure-app-registrering för klientprogrammet och Ersätt den *keyCredentials* egenskap med information för din nya certifikat med hjälp av följande schema:</span><span class="sxs-lookup"><span data-stu-id="4ee9e-139">In the Azure app registration for the client application, open the application manifest, and replace the *keyCredentials* property with your new certificate information using the following schema:</span></span>
```
"keyCredentials": [
    {
        "customKeyIdentifier": "$base64Thumbprint",
        "keyId": "$keyid",
        "type": "AsymmetricX509Cert",
        "usage": "Verify",
        "value":  "$base64Value"
    }
]
```

<span data-ttu-id="4ee9e-140">Spara ändringarna i programmanifestet och ladda upp till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4ee9e-140">Save the edits to the application manifest, and upload to Azure AD.</span></span> <span data-ttu-id="4ee9e-141">Egenskapen keyCredentials är ett flervärdesattribut, så du kan ladda upp flera certifikat bättre hantering av nycklar.</span><span class="sxs-lookup"><span data-stu-id="4ee9e-141">The keyCredentials property is multi-valued, so you may upload multiple certificates for richer key management.</span></span>
