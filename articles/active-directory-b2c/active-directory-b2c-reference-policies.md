---
title: 'Azure Active Directory B2C: Inbyggda principer | Microsoft Docs'
description: "Ett ämne på hello expanderbara principramverk av Azure Active Directory B2C och hur toocreate olika principtyper"
services: active-directory-b2c
documentationcenter: 
author: sama
manager: mbaldwin
editor: PatAltimore
ms.assetid: 0d453e72-7f70-4aa2-953d-938d2814d5a9
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: sama
ms.openlocfilehash: 24bb85eba30f888c6ea7d0401e05235e8eb6ea79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-built-in-policies"></a>Azure Active Directory B2C: Inbyggda principer


hello expanderbara principramverk av Azure Active Directory (AD Azure) B2C är hello core styrkan hos hello-tjänsten. Principer fullständigt beskriver konsumenten identitetsupplevelser som registrering, inloggning eller Redigera profil. Till exempel kan en princip för registrering du toocontrol beteenden genom att konfigurera hello följande inställningar:

* Kontotyp (sociala konton som Facebook) eller lokala konton, till exempel e-postadresser som konsumenterna kan använda toosign för programmet hello
* Attribut (till exempel förnamn, postnummer och sko storlek) toobe samlas in från hello konsumenten under registreringen
* Användning av Azure Multi-Factor Authentication
* hello utseendet på alla sidor för registrering
* Information (som visar som anspråk i en token) som hello programmet tar emot när hello princip kör är klar

Du kan skapa flera principer för olika typer i din klient och använda dem i dina program efter behov. Principer kan återanvändas i program. Den här flexibiliteten gör det möjligt för utvecklare toodefine och ändra konsumenten identitetsupplevelser med minimal eller inga ändringar tootheir kod.

Principer kan användas via en enkel developer-gränssnittet. Programmet utlöser en princip med hjälp av en standard HTTP autentiseringsbegäran (skicka en princip för parameter i hello begäran) och får en anpassad token som svar. Hello enda skillnaden mellan förfrågningar som anropar en princip för registrering och förfrågningar som anropar en princip för inloggning är till exempel hello principnamn som används i hello ”p” frågesträngparametern:

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Läs mer om hello principramverk [i det här blogginlägget om Azure AD B2C på hello Enterprise Mobility och Säkerhetsblogg](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-or-sign-in-policy"></a>Skapa en princip för registrering eller inloggning

Den här principen hanterar upplevelser för registrering och inloggning i båda konsumenten med en enda konfiguration. Konsumenterna vägleds ned hello rätt sökväg (registrering eller inloggning) beroende på hello kontext. Här beskrivs också hello innehållet i token som programmet hello får lyckats logga in eller inloggningar.  En kodexempel för hello-princip för registrering eller inloggning är [tillgänglig här](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  Är det rekommenderade att du använder den här principen via en principen för registrering och inloggning princip.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a>Skapa en princip för registrering

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a>Skapa en princip för inloggning

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a>Skapa en profil Redigera princip

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a>Skapa en princip för lösenordsåterställning

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a>Hur länkar en princip för registrering eller inloggning med en princip för lösenordsåterställning?
När du skapar en princip för registrering eller inloggning (med lokala konton) visas en **har du glömt lösenordet?** länk på hello första sidan i hello upplevelse. Klicka på den här länken återställs inte utlösaren lösenord automatiskt princip. 

I stället hello felkoden  **`AADB2C90118`**  returneras tooyour app. Appen måste toohandle felet genom att aktivera en specifik princip för lösenordsåterställning. Mer information finns i en [exempel som visar hello-metod länka principer för](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a>Bör jag använda en princip för registrering eller inloggning eller en princip för registrering och en princip för inloggning?
Vi rekommenderar att du använder en princip för registrering eller inloggning via en princip för registrering och en princip för inloggning.  

hello-princip för registrering eller inloggning har fler funktioner än hello inloggningsprincip. Dessutom kan du toouse sidan anpassningar och har bättre stöd för lokalisering. 

Hej inloggningsprincip rekommenderas om du inte behöver toolocalize dina principer, bara behöver mindre anpassningsmöjligheter för anpassning och vill lösenord återställning som är inbyggda i den.

## <a name="next-steps"></a>Nästa steg
* [Token, session och konfiguration för enkel inloggning](active-directory-b2c-token-session-sso.md)
* [Inaktivera e-Postverifiering under konsumenten registrering](active-directory-b2c-reference-disable-ev.md)

