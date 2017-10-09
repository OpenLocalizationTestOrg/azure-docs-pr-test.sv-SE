---
title: "aaaAzure AD-tjänsten tooService Auth med OAuth2.0 | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse HTTP meddelanden tooimplement tjänsten tooservice autentisering med hjälp av bevilja klientautentiseringsuppgifter för hello OAuth2.0."
services: active-directory
documentationcenter: .net
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: a7f939d9-532d-4b6d-b6d3-95520207965d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: f4bfd4ea8a7de1929c7dcf7ad65a156edff74f71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Tooservice-anrop med klientens autentiseringsuppgifter (delad hemlighet eller certifikat)
hello OAuth 2.0 klientens autentiseringsuppgifter bevilja flöda tillåter en webbtjänst (*konfidentiell klienten*) toouse egna autentiseringsuppgifter i stället för att personifiera en användare tooauthenticate vid anrop av en annan webbtjänsten. I det här scenariot hello klienten är vanligtvis en webbtjänst på mellannivå, en daemon tjänst eller webbplats. Azure AD kan också hello anropar tjänsten toouse ett certifikat (i stället för en delad hemlighet) som en referens för en högre säkerhetsnivå.

## Klientens autentiseringsuppgifter bevilja flödesdiagram
hello förklarar följande diagram hur hello klientens autentiseringsuppgifter bevilja flödet fungerar i Azure Active Directory (AD Azure).

![OAuth2.0 bevilja för klientautentiseringsuppgifter](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. hello klientprogrammet autentiserar toohello Azure AD utfärdande slutpunkt och begär en åtkomst-token.
2. hello Azure AD utfärdande endpoint problem hello åtkomst-token.
3. hello åtkomsttoken är används tooauthenticate toohello skyddad resurs.
4. Data från en skyddad resurs hello returneras toohello webbprogram.

## Registrera hello tjänster i Azure AD
Registrera både hello anropar tjänsten och hello ta emot service i Azure Active Directory (AD Azure). Detaljerade instruktioner finns [integrera program med Azure Active Directory](active-directory-integrating-applications.md).

## Begär en åtkomst-Token
toorequest ett åtkomsttoken, använda en HTTP POST toohello klient-specifika Azure AD-slutpunkt.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## Begäran för tjänst-till-tjänst åtkomst-token
Det finns två fall beroende på om hello klientprogrammet väljer toobe som skyddas av en delad hemlighet eller ett certifikat.

### Först fall: token åtkomst-begäran med en delad hemlighet
När du använder en delad hemlighet, innehåller en tjänst-till-tjänst åtkomst tokenbegäran hello följande parametrar:

| Parameter |  | Beskrivning |
| --- | --- | --- |
| grant_type |Krävs |Anger hello begärt bevilja. I ett klient autentiseringsuppgifter Grant-flöde hello-värdet måste vara **client_credentials**. |
| client_id |Krävs |Anger hello Azure AD-klient-id hello anropa webbtjänsten. toofind hello anrop programmets klient-ID i hello [Azure-portalen](https://portal.azure.com), klickar du på **Active Directory**, växla directory, klicka på hello program. Hej client_id är hello *program-ID* |
| client_secret |Krävs |Ange en nyckel som registrerats för hello anropar tjänsten eller daemon webbprogram i Azure AD. toocreate en nyckel i hello Azure-portalen klickar du på **Active Directory**, växla directory, klicka på hello program, **inställningar**, klickar du på **nycklar**, och lägga till en nyckel.|
| Resursen |Krävs |Ange hello App-ID-URI för hello mottagning av webbtjänsten. toofind hello App-ID URI i hello Azure-portalen klickar du på **Active Directory**, växla directory, klicka på hello-tjänstprogrammet och klicka sedan på **inställningar** och **egenskaper** |

#### Exempel
hello följande HTTP POST-begäranden en åtkomst-token för hello https://service.contoso.com/-webbtjänsten. Hej `client_id` identifierar hello webbtjänsttillägg som begär hello åtkomst-token.

```
POST /contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

### Andra fall: token åtkomst-begäran med ett certifikat
En token-tjänster åtkomst-begäran med ett certifikat innehåller hello följande parametrar:

| Parameter |  | Beskrivning |
| --- | --- | --- |
| grant_type |Krävs |Anger hello begärt svarstyp. I ett klient autentiseringsuppgifter Grant-flöde hello-värdet måste vara **client_credentials**. |
| client_id |Krävs |Anger hello Azure AD-klient-id hello anropa webbtjänsten. toofind hello anrop programmets klient-ID i hello [Azure-portalen](https://portal.azure.com), klickar du på **Active Directory**, växla directory, klicka på hello program. Hej client_id är hello *program-ID* |
| client_assertion_type |Krävs |hello-värdet måste vara`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Krävs | Ett intyg (en JSON Web Token) att du behöver toocreate och logga in med hello certifikat du registrerad som autentiseringsuppgifterna för ditt program. Läs mer om [certifikat autentiseringsuppgifter](active-directory-certificate-credentials.md) toolearn hur tooregister ditt certifikat och hello formatering av hello-kontrollen.|
| Resursen | Krävs |Ange hello App-ID-URI för hello mottagning av webbtjänsten. toofind hello App-ID URI i hello Azure-portalen klickar du på **Active Directory**på hello katalog, klicka på hello program, och klicka sedan på **konfigurera**. |

Observera att hello parametrar är nästan hello samma som hello fallet med hello begäran av delad hemlighet förutom att hello client_secret parameter har ersatts av två parametrar: en client_assertion_type och client_assertion.

#### Exempel
hello följande HTTP POST-begäranden en åtkomst-token för hello https://service.contoso.com/ webbtjänst med ett certifikat. Hej `client_id` identifierar hello webbtjänsttillägg som begär hello åtkomst-token.

```
POST /<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

resource=https%3A%2F%contoso.onmicrosoft.com%2Ffc7664b4-cdd6-43e1-9365-c2e1c4e1b3bf&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

### Tjänst-till-tjänst åtkomst-Token svar

Ett lyckat svar innehåller ett JSON OAuth 2.0-svar med hello följande parametrar:

| Parameter | Beskrivning |
| --- | --- |
| access_token |hello begärda åtkomst-token. hello anropar webbtjänsten kan använda den här token tooauthenticate toohello mottagning av webbtjänsten. |
| token_type |Anger hello tokentypen värdet. Hej typ som har stöd för Azure AD är **ägar**. Mer information om ägar-token finns hello [OAuth 2.0 auktorisering Framework: ägar-Token användning (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| expires_in |Hur länge hello åtkomst-token är giltig (i sekunder). |
| expires_on |hello tid när hello åtkomst-token upphör att gälla. hello representeras som hello antal sekunder från 1970-01-01T0:0:0Z UTC tills hello förfallotid. Det här värdet är används toodetermine hello livstid cachelagrade token. |
| not_before |hello tiden från vilka hello åtkomsttoken blir kan användas. hello representeras som hello antal sekunder från 1970-01-01T0:0:0Z UTC förrän giltighetstiden för hello-token.|
| Resursen |hello App-ID-URI för hello mottagning av webbtjänsten. |

#### Exempel på svaret
hello visar följande exempel en lyckad svar tooa begäran om en åtkomst-token tooa webbtjänst.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## Se även
* [OAuth 2.0 i Azure AD](active-directory-protocols-oauth-code.md)
* [Exemplet i C# för hello service tooservice anrop med en delad hemlighet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) och [exemplet i C# för hello service tooservice anrop med ett certifikat](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)
