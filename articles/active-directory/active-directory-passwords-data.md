---
title: "Krav för Azure AD SSPR data | Microsoft Docs"
description: "Uppgifter som krävs för Azure AD-lösenordet för självbetjäning återställning och hur du uppfylla dem."
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
ms.openlocfilehash: 2d1afd2d1265b371e0d311ed70fffbc55874b0a7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Distribuera utan slutanvändarregistrering för återställning av lösenord

Distribution av självbetjäningsportalen lösenord återställa (SSPR) kräver autentiseringsdata finnas. Vissa organisationer har användarna ange sina autentiseringsdata själva, men många organisationer föredrar att synkronisera med befintliga data i Active Directory. Om du har korrekt formaterade data i din lokala katalog och konfigurerar [Azure AD Connect med standardinställningar](./connect/active-directory-aadconnect-get-started-express.md), att data görs tillgänglig för Azure AD och SSPR utan användaråtgärder krävs.

Alla telefonnummer måste vara i formatet + CountryCode PhoneNumber exempel: + 1 4255551234 ska fungera korrekt.

> [!NOTE]
> Återställning av lösenord stöder inte telefonanknytningar. Även i formatet + 1 4255551234 X 12345 tas tillägg bort innan anropet görs.

## <a name="fields-populated"></a>Fält

Om du använder standardinställningarna i Azure AD Connect görs följande avbildningar.

| Lokala AD | Azure AD | Azure AD Authentication kontaktinformation |
| --- | --- | --- |
| telephoneNumber | Arbetstelefon | Telefon |
| mobila | Mobiltelefon | Telefon |


## <a name="security-questions-and-answers"></a>Säkerhetsfrågor och svar

Säkerhetsfrågor och svar lagras på ett säkert sätt i Azure AD-klienten och är bara tillgänglig för användare via den [SSPR-registreringsportalen](https://aka.ms/ssprsetup). Administratörer kan inte se eller ändra innehållet i en annan användare frågor och svar.

### <a name="what-happens-when-a-user-registers"></a>Vad händer när en användare som registrerar

När en användare som registrerar anger registreringssidan följande fält:

* Telefon för autentisering
* E-post för autentisering
* Säkerhetsfrågor och svar

Om du har angett ett värde för **mobiltelefon** eller **alternativa e-postadress**, omedelbart fjärranvändning av dessa värden kan återställa sina lösenord, även om de inte har registrerats för tjänsten. Dessutom kan användare se dessa värden när du registrerar dig för första gången och ändra dem om de vill. När de har registrerat värdena kommer att sparas i den **telefon för autentisering** och **autentisering e-post** respektive fält.

## <a name="set-and-read-authentication-data-using-powershell"></a>Ange och läsa autentiseringsdata med hjälp av PowerShell

Följande fält kan anges med hjälp av PowerShell

* Alternativ e-postadress
* Mobiltelefon
* Arbetstelefon - kan bara anges om inte synkroniseras med en lokal katalog

### <a name="using-powershell-v1"></a>Med hjälp av PowerShell V1

Om du vill komma igång behöver du [ladda ned och installera Azure AD PowerShell-modulen](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). När du har installerat kan följa du de steg som följer för att konfigurera varje fält.

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

#### <a name="authentication-phone-and-authentication-email-can-only-be-read-using-powershell-v1-using-the-commands-that-follow"></a>Telefon för autentisering och autentisering e-post kan endast läsas med hjälp av Powershell V1 med de kommandon som följer

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="using-powershell-v2"></a>Med hjälp av PowerShell V2

Om du vill komma igång behöver du [ladda ned och installera Azure AD-V2 PowerShell-modulen](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md). När du har installerat kan följa du de steg som följer för att konfigurera varje fält.

Om du vill installera snabbt från senare versioner av PowerShell som stöder installera modulen, kör du kommandona (den första raden bara kontrollerar om den är redan installerad):

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

Följande länkar ger ytterligare information om lösenordsåterställning med Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Distribution**](active-directory-passwords-best-practices.md) – Planera och distribuera SSPR till dina användare med hjälp av informationen finns här
* [**Anpassa**](active-directory-passwords-customize.md) – Anpassa utseendet för företagets SSPR-funktion.
* [**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Teknisk djupdykning** ](active-directory-passwords-how-it-works.md) – Ta en titt bakom kulisserna för att förstå hur det hela fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? – Svar på allt du någonsin velat fråga
* [**Felsökning** ](active-directory-passwords-troubleshoot.md) – Lär dig att lösa vanliga problem med SSPR
