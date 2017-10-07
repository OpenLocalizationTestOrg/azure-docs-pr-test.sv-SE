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
# <a name="certificate-credentials-for-application-authentication"></a>Autentiseringsuppgifter för certifikat för autentisering

Azure Active Directory kan ett program toouse egna autentiseringsuppgifter för autentisering, till exempel i hello flödet för OAuth 2.0 klientens autentiseringsuppgifter Grant och hello på-flöde.
En form av autentiseringsuppgifter som kan användas är en JSON Web Token(JWT) assertion signeras med ett certifikat som programmet hello äger.

## <a name="format-of-hello-assertion"></a>Formatet på hello assertion
toocompute hello assertion vill du förmodligen toouse en hello många [JSON Web Token](https://jwt.io/) bibliotek i hello önskat språk. hello information av hello token är:

#### <a name="header"></a>Huvudet

| Parameter |  Kommentar |
| --- | --- | --- |
| `alg` | Bör vara **RS256** |
| `typ` | Bör vara **JWT** |
| `x5t` | Ska vara hello tumavtrycket för X.509-certifikat SHA-1 |

#### <a name="claims-payload"></a>Anspråk (Payload)

| Parameter |  Kommentar |
| --- | --- | --- |
| `aud` | Målgrupp: Bör vara  **https://login.microsoftonline.com/*tenant_Id*  /oauth2/token ** |
| `exp` | Förfallodatum: hello datum när hello-token upphör att gälla. hello tid representeras hello antal sekunder från den 1 januari 1970 (1970-01-01T0:0:0Z) UTC tills hello hello token giltighetstid upphör att gälla.|
| `iss` | Utgivare: bör vara hello client_id (program-Id för hello-klienttjänsten) |
| `jti` | GUID: hello JWT-ID |
| `nbf` | Inte före: hello-datum före vilken hello token inte kan användas. hello tid representeras hello antal sekunder från den 1 januari 1970 (1970-01-01T0:0:0Z) UTC tills hello tid hello token har utfärdats. |
| `sub` | Ämne: som för `iss`, bör vara hello client_id (program-Id för hello-klienttjänsten) |

#### <a name="signature"></a>Signatur
hello signaturen beräknas tillämpa hello certifikatet enligt beskrivningen i hello [JSON Web Token RFC7519 specifikation](https://tools.ietf.org/html/rfc7519)

### <a name="example-of-a-decoded-jwt-assertion"></a>Exempel på en avkodade JWT-kontrollen
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

### <a name="example-of-an-encoded-jwt-assertion"></a>Exempel på en kodad JWT-kontrollen
hello följande sträng är ett exempel på kodade kontrollen. Om du ser noggrant kan se du tre avsnitt avgränsade med punkter (.).
hello första avsnittet kodar hello-rubriken, hello andra hello nyttolasten och hello senast är hello signaturen som beräknats med hello certifikat från hello innehållet i hello först två avsnitt.
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

### <a name="register-your-certificate-with-azure-ad"></a>Registrera ditt certifikat med Azure AD
tooassociate hello certifikat autentiseringsuppgift med hello klientprogram i Azure AD, behöver du tooedit hello programmanifestet.
Med undantag för ett certifikat, behöver du toocompute:
- `$base64Thumbprint`, som är hello base64-kodning av hello certifikat-Hash
- `$base64Value`, som är hello base64-kodning av hello certifikatets rådata

Du måste också tooprovide en GUID tooidentify hello nyckel i programmanifestet hello (`$keyId`)

Öppna hello programmanifestet i hello Azure-appregistrering för hello klientprogrammet, och Ersätt hello *keyCredentials* egenskap med din nya certifikatinformation med hjälp av följande schemat hello:
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

Spara hello redigeringar toohello programmanifestet och ladda upp tooAzure AD. Hej keyCredentials egenskap har flera värden, så du kan ladda upp flera certifikat bättre hantering av nycklar.
