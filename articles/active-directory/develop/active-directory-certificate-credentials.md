---
title: aaaCertificate autentiseringsuppgifter i Azure AD | Microsoft Docs
description: "Den här artikeln beskrivs hello registrering och användning av autentiseringsuppgifter för certifikat för autentisering"
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
ms.openlocfilehash: 3508803112ac06268d553db86ab74812aa53e455
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="certificate-credentials-for-application-authentication"></a><span data-ttu-id="fb27e-103">Autentiseringsuppgifter för certifikat för autentisering</span><span class="sxs-lookup"><span data-stu-id="fb27e-103">Certificate credentials for application authentication</span></span>

<span data-ttu-id="fb27e-104">Azure Active Directory kan ett program toouse egna autentiseringsuppgifter för autentisering, till exempel i hello flödet för OAuth 2.0 klientens autentiseringsuppgifter Grant och hello på-flöde.</span><span class="sxs-lookup"><span data-stu-id="fb27e-104">Azure Active Directory allows an application toouse its own credentials for authentication, for example, in hello OAuth 2.0 Client Credentials Grant flow and hello On-Behalf-Of flow.</span></span>
<span data-ttu-id="fb27e-105">En form av autentiseringsuppgifter som kan användas är en JSON Web Token(JWT) assertion signeras med ett certifikat som programmet hello äger.</span><span class="sxs-lookup"><span data-stu-id="fb27e-105">One form of credential that can be used is a JSON Web Token(JWT) assertion signed with a certificate that hello application owns.</span></span>

## <a name="format-of-hello-assertion"></a><span data-ttu-id="fb27e-106">Formatet på hello assertion</span><span class="sxs-lookup"><span data-stu-id="fb27e-106">Format of hello assertion</span></span>
<span data-ttu-id="fb27e-107">toocompute hello assertion vill du förmodligen toouse en hello många [JSON Web Token](https://jwt.io/) bibliotek i hello önskat språk.</span><span class="sxs-lookup"><span data-stu-id="fb27e-107">toocompute hello assertion, you probably want toouse one of hello many [JSON Web Token](https://jwt.io/) libraries in hello language of your choice.</span></span> <span data-ttu-id="fb27e-108">hello information av hello token är:</span><span class="sxs-lookup"><span data-stu-id="fb27e-108">hello information carried by hello token is:</span></span>

#### <a name="header"></a><span data-ttu-id="fb27e-109">Huvudet</span><span class="sxs-lookup"><span data-stu-id="fb27e-109">Header</span></span>

| <span data-ttu-id="fb27e-110">Parameter</span><span class="sxs-lookup"><span data-stu-id="fb27e-110">Parameter</span></span> |  <span data-ttu-id="fb27e-111">Kommentar</span><span class="sxs-lookup"><span data-stu-id="fb27e-111">Remark</span></span> |
| --- | --- | --- |
| `alg` | <span data-ttu-id="fb27e-112">Bör vara **RS256**</span><span class="sxs-lookup"><span data-stu-id="fb27e-112">Should be **RS256**</span></span> |
| `typ` | <span data-ttu-id="fb27e-113">Bör vara **JWT**</span><span class="sxs-lookup"><span data-stu-id="fb27e-113">Should be **JWT**</span></span> |
| `x5t` | <span data-ttu-id="fb27e-114">Ska vara hello tumavtrycket för X.509-certifikat SHA-1</span><span class="sxs-lookup"><span data-stu-id="fb27e-114">Should be hello X.509 Certificate SHA-1 thumbprint</span></span> |

#### <a name="claims-payload"></a><span data-ttu-id="fb27e-115">Anspråk (Payload)</span><span class="sxs-lookup"><span data-stu-id="fb27e-115">Claims (Payload)</span></span>

| <span data-ttu-id="fb27e-116">Parameter</span><span class="sxs-lookup"><span data-stu-id="fb27e-116">Parameter</span></span> |  <span data-ttu-id="fb27e-117">Kommentar</span><span class="sxs-lookup"><span data-stu-id="fb27e-117">Remark</span></span> |
| --- | --- | --- |
| `aud` | <span data-ttu-id="fb27e-118">Målgrupp: Bör vara  **https://login.microsoftonline.com/*tenant_Id*  /oauth2/token **</span><span class="sxs-lookup"><span data-stu-id="fb27e-118">Audience: Should be **https://login.microsoftonline.com/*tenant_Id*/oauth2/token**</span></span> |
| `exp` | <span data-ttu-id="fb27e-119">Förfallodatum: hello datum när hello-token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="fb27e-119">Expiration date: hello date when hello token expires.</span></span> <span data-ttu-id="fb27e-120">hello tid representeras hello antal sekunder från den 1 januari 1970 (1970-01-01T0:0:0Z) UTC tills hello hello token giltighetstid upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="fb27e-120">hello time is represented as hello number of seconds from January 1, 1970 (1970-01-01T0:0:0Z) UTC until hello time hello token validity expires.</span></span>|
| `iss` | <span data-ttu-id="fb27e-121">Utgivare: bör vara hello client_id (program-Id för hello-klienttjänsten)</span><span class="sxs-lookup"><span data-stu-id="fb27e-121">Issuer: should be hello client_id (Application Id of hello client service)</span></span> |
| `jti` | <span data-ttu-id="fb27e-122">GUID: hello JWT-ID</span><span class="sxs-lookup"><span data-stu-id="fb27e-122">GUID: hello JWT ID</span></span> |
| `nbf` | <span data-ttu-id="fb27e-123">Inte före: hello-datum före vilken hello token inte kan användas.</span><span class="sxs-lookup"><span data-stu-id="fb27e-123">Not Before: hello date before which hello token cannot be used.</span></span> <span data-ttu-id="fb27e-124">hello tid representeras hello antal sekunder från den 1 januari 1970 (1970-01-01T0:0:0Z) UTC tills hello tid hello token har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="fb27e-124">hello time is represented as hello number of seconds from January 1, 1970 (1970-01-01T0:0:0Z) UTC until hello time hello token was issued.</span></span> |
| `sub` | <span data-ttu-id="fb27e-125">Ämne: som för `iss`, bör vara hello client_id (program-Id för hello-klienttjänsten)</span><span class="sxs-lookup"><span data-stu-id="fb27e-125">Subject: As for `iss`, should be hello client_id (Application Id of hello client service)</span></span> |

#### <a name="signature"></a><span data-ttu-id="fb27e-126">Signatur</span><span class="sxs-lookup"><span data-stu-id="fb27e-126">Signature</span></span>
<span data-ttu-id="fb27e-127">hello signaturen beräknas tillämpa hello certifikatet enligt beskrivningen i hello [JSON Web Token RFC7519 specifikation](https://tools.ietf.org/html/rfc7519)</span><span class="sxs-lookup"><span data-stu-id="fb27e-127">hello signature is computed applying hello certificate as described in hello [JSON Web Token RFC7519 specification](https://tools.ietf.org/html/rfc7519)</span></span>

### <a name="example-of-a-decoded-jwt-assertion"></a><span data-ttu-id="fb27e-128">Exempel på en avkodade JWT-kontrollen</span><span class="sxs-lookup"><span data-stu-id="fb27e-128">Example of a decoded JWT assertion</span></span>
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

### <a name="example-of-an-encoded-jwt-assertion"></a><span data-ttu-id="fb27e-129">Exempel på en kodad JWT-kontrollen</span><span class="sxs-lookup"><span data-stu-id="fb27e-129">Example of an encoded JWT assertion</span></span>
<span data-ttu-id="fb27e-130">hello följande sträng är ett exempel på kodade kontrollen.</span><span class="sxs-lookup"><span data-stu-id="fb27e-130">hello following string is an example of encoded assertion.</span></span> <span data-ttu-id="fb27e-131">Om du ser noggrant kan se du tre avsnitt avgränsade med punkter (.).</span><span class="sxs-lookup"><span data-stu-id="fb27e-131">If you look carefully, you notice three sections separated by dots (.).</span></span>
<span data-ttu-id="fb27e-132">hello första avsnittet kodar hello-rubriken, hello andra hello nyttolasten och hello senast är hello signaturen som beräknats med hello certifikat från hello innehållet i hello först två avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fb27e-132">hello first section encodes hello header, hello second hello payload, and hello last is hello signature computed with hello certificates from hello content of hello first two sections.</span></span>
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

### <a name="register-your-certificate-with-azure-ad"></a><span data-ttu-id="fb27e-133">Registrera ditt certifikat med Azure AD</span><span class="sxs-lookup"><span data-stu-id="fb27e-133">Register your certificate with Azure AD</span></span>
<span data-ttu-id="fb27e-134">tooassociate hello certifikat autentiseringsuppgift med hello klientprogram i Azure AD, behöver du tooedit hello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="fb27e-134">tooassociate hello certificate credential with hello client application in Azure AD, you need tooedit hello application manifest.</span></span>
<span data-ttu-id="fb27e-135">Med undantag för ett certifikat, behöver du toocompute:</span><span class="sxs-lookup"><span data-stu-id="fb27e-135">Having hold of a certificate, you need toocompute:</span></span>
- <span data-ttu-id="fb27e-136">`$base64Thumbprint`, som är hello base64-kodning av hello certifikat-Hash</span><span class="sxs-lookup"><span data-stu-id="fb27e-136">`$base64Thumbprint`, which is hello base64 encoding of hello certificate Hash</span></span>
- <span data-ttu-id="fb27e-137">`$base64Value`, som är hello base64-kodning av hello certifikatets rådata</span><span class="sxs-lookup"><span data-stu-id="fb27e-137">`$base64Value`, which is hello base64 encoding of hello certificate raw data</span></span>

<span data-ttu-id="fb27e-138">Du måste också tooprovide en GUID tooidentify hello nyckel i programmanifestet hello (`$keyId`)</span><span class="sxs-lookup"><span data-stu-id="fb27e-138">you also need tooprovide a GUID tooidentify hello key in hello application manifest (`$keyId`)</span></span>

<span data-ttu-id="fb27e-139">Öppna hello programmanifestet i hello Azure-appregistrering för hello klientprogrammet, och Ersätt hello *keyCredentials* egenskap med din nya certifikatinformation med hjälp av följande schemat hello:</span><span class="sxs-lookup"><span data-stu-id="fb27e-139">In hello Azure app registration for hello client application, open hello application manifest, and replace hello *keyCredentials* property with your new certificate information using hello following schema:</span></span>
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

<span data-ttu-id="fb27e-140">Spara hello redigeringar toohello programmanifestet och ladda upp tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="fb27e-140">Save hello edits toohello application manifest, and upload tooAzure AD.</span></span> <span data-ttu-id="fb27e-141">Hej keyCredentials egenskap har flera värden, så du kan ladda upp flera certifikat bättre hantering av nycklar.</span><span class="sxs-lookup"><span data-stu-id="fb27e-141">hello keyCredentials property is multi-valued, so you may upload multiple certificates for richer key management.</span></span>
