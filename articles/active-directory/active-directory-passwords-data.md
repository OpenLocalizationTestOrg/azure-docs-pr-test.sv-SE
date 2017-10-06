---
title: "Krav för Azure AD SSPR data | Microsoft Docs"
description: "Återställa datakrav för Azure AD-lösenordet för självbetjäning och hur toosatisfy dem."
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: b68a1d7914dcd0bb4509d0e94914dc4309f4463a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Distribuera utan slutanvändarregistrering för återställning av lösenord

Distribution av självbetjäningsportalen lösenord återställa (SSPR) kräver autentisering data toobe finns. Vissa organisationer har användarna ange sina autentiseringsdata själva, men många organisationer föredrar toosynchronize med befintliga data i Active Directory. Om du har korrekt formaterade data i din lokala katalog och konfigurerar [Azure AD Connect med standardinställningar](./connect/active-directory-aadconnect-get-started-express.md), data blir tillgängliga tooAzure AD och SSPR utan användaråtgärder krävs.

Alla telefonnummer måste vara i formatet hello + CountryCode PhoneNumber exempel: + 1 4255551234 toowork korrekt.

> [!NOTE]
> Återställning av lösenord stöder inte telefonanknytningar. Tillägg tas bort innan hello görs även i hello + 1 4255551234 X 12345-format.

## <a name="fields-populated"></a>Fält

Om du använder standardinställningarna för hello i Azure AD Connect hello följande görs mappningar.

| Lokala AD | Azure AD | Azure AD Authentication kontaktinformation |
| --- | --- | --- |
| telephoneNumber | Arbetstelefon | Telefon |
| mobila | Mobiltelefon | Telefon |


## <a name="security-questions-and-answers"></a>Säkerhetsfrågor och svar

Säkerhetsfrågor och svar lagras på ett säkert sätt i Azure AD-klienten och är endast tillgänglig toousers via hello [SSPR-registreringsportalen](https://aka.ms/ssprsetup). Administratörer kan inte se eller ändra hello innehållet i en annan användare frågor och svar.

### <a name="what-happens-when-a-user-registers"></a>Vad händer när en användare som registrerar

När en användare som registrerar anger hello registreringssidan hello följande fält:

* Telefon för autentisering
* E-post för autentisering
* Säkerhetsfrågor och svar

Om du har angett ett värde för **mobiltelefon** eller **alternativa e-postadress**, omedelbart fjärranvändning av dessa värden tooreset sina lösenord, även om de inte har registrerats för hello-tjänsten. Dessutom kan användare se dessa värden när du registrerar dig för hello första gången och ändra dem om de vill. När de har registrerat värdena kommer att sparas i hello **telefon för autentisering** och **autentisering e-post** respektive fält.

## <a name="set-and-read-authentication-data-using-powershell"></a>Ange och läsa autentiseringsdata med hjälp av PowerShell

hello följande fält kan anges med hjälp av PowerShell

* Alternativ e-postadress
* Mobiltelefon
* Arbetstelefon - kan bara anges om inte synkroniseras med en lokal katalog

### <a name="using-powershell-v1"></a>Med hjälp av PowerShell V1

tooget igång, du behöver för[ladda ned och installera hello Azure AD PowerShell-modulen](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). När du har installerat kan följa du hello steg som följer tooconfigure varje fält.

#### <a name="set-authentication-data-with-powershell-v1"></a>Ange autentiseringsdata med PowerShell V1

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-authentication-data-with-powershellpowershell-v1"></a>Läs autentiseringsdata i PowerShellPowerShell V1

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="authentication-phone-and-authentication-email-can-only-be-read-using-powershell-v1-using-hello-commands-that-follow"></a>Telefon för autentisering och autentisering e-post kan endast läsas med hjälp av Powershell V1 med hello kommandon som följer

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="using-powershell-v2"></a>Med hjälp av PowerShell V2

tooget igång, du behöver för[ladda ned och installera hello V2 för Azure AD PowerShell-modulen](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md). När du har installerat kan följa du hello steg som följer tooconfigure varje fält.

tooinstall snabbt från senare versioner av PowerShell som har stöd för installation-modulen kör kommandona (hello första raden bara kontrolleras toosee om det redan är installerat):

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-authentication-data-with-powershell-v2"></a>Ange autentiseringsdata med PowerShell V2

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

### <a name="read-authentication-data-with-powershell-v2"></a>Läs autentiseringsdata med PowerShell V2

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Nästa steg

hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här
* [**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.
* [**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? -Svar tooquestions du alltid vill ha tooask
* [**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR
