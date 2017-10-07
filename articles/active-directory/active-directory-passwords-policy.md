---
title: 'Princip: Azure AD SSPR | Microsoft Docs'
description: "Alternativ för återställning av lösenord för självbetjäning av Azure AD"
services: active-directory
keywords: "Hantering av Active directory-lösenord, lösenordshantering, Azure AD self service för lösenordsåterställning"
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
ms.openlocfilehash: af7cb13794bf3a9fee91d355f788aa5c2246e57c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Lösenordsprinciper och begränsningar i Azure Active Directory

Den här artikeln beskriver hello lösenordsprinciper och krav på komplexitet som är associerade med användarkonton lagras i Azure AD-klienten.

## <a name="administrator-password-policy-differences"></a>Administratören lösenord princip skillnader

Microsoft tillämpar en stark standardprincip för lösenordsåterställning med **två portar** för alla Azure-administratörsroller (till exempel Global administratör, Supportavdelningsadministratör och Lösenordsadministratör).

Detta inaktiverar administratörer från att använda säkerhetsfrågor och tillämpar hello följande.

Två gate-principen, som kräver två typer av autentiseringsdata (e-postadress **och** telefonnummer), så tillämpas i hello följande omständigheter

* Alla Azure-administratörsroller
  * Supportavdelningen administratör
  * Tjänstsupportadministratör
  * Faktureringsadministratör
  * Support för partner Tier1
  * Support för partner Tier2
  * Tjänstadministratör för Exchange
  * Lync tjänstadministratör
  * Kontoadministratör för användaren
  * Directory-skrivare
  * Global administratör/företagsadministratör
  * Tjänstadministratör för SharePoint
  * Kompatibilitet administratör
  * Programadministratör
  * Säkerhetsadministratör
  * Administratör för privilegierade roller
  * Intune-tjänstadministratören
  * Tjänstadministratör för Application Proxy
  * CRM-tjänstadministratör
  * Tjänstadministratör för Power BI
  
* 30 dagar har i en utvärderingsversion **eller**
* Alternativa domänen finns (contoso.com) **eller**
* Azure AD Connect synkroniserar identiteter från din lokala katalog

### <a name="exceptions"></a>Undantag
En gate-principen, som kräver en dataenhet autentisering (e-postadress **eller** telefonnummer), så tillämpas i hello följande omständigheter

* Första 30 dagar från en utvärderingsversion **eller**
* Alternativa domänen finns inte (*. onmicrosoft.com) **och** Azure AD Connect inte synkroniserar identiteter


## <a name="userprincipalname-policies-that-apply-tooall-user-accounts"></a>UserPrincipalName principer som gäller tooall användarkonton

Varje användarkonto som behöver toosign i tooAzure AD måste ha ett unikt huvudnamn (UPN)-attributvärde som associeras med deras konto. hello tabellen nedan beskrivs hello principer som gäller tooboth lokala Active Directory-användarkonton synkroniseras toohello moln och endast toocloud användarkonton.

| Egenskap | UserPrincipalName krav |
| --- | --- |
| Tecken som tillåts |<ul> <li>A – Z</li> <li>a - z</li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
| Tecken som tillåts inte |<ul> <li>Alla ' @-tecken som inte avgränsa hello användarnamn från hello domän.</li> <li>Får inte innehålla en punkt '.' närmast föregående hello ”@” symbol</li></ul> |
| Längden begränsningar |<ul> <li>Den totala längden får inte överskrida 113 tecken</li><li>64 tecken innan hello ”@” symbol</li><li>48 tecken efter hello ”@” symbol</li></ul> |

## <a name="password-policies-that-apply-only-toocloud-user-accounts"></a>Lösenordsprinciper som gäller endast toocloud användarkonton

hello i den följande tabellen beskrivs principinställningarna för hello tillgängliga lösenord som kan vara tillämpade toouser konton som skapas och hanteras i Azure AD.

| Egenskap | Krav |
| --- | --- |
| Tecken som tillåts |<ul><li>A – Z</li><li>a - z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
| Tecken som tillåts inte |<ul><li>Unicode-tecken</li><li>Blanksteg</li><li> **Starka lösenord**: får inte innehålla en punkttecknet '.' närmast föregående hello ”@” symbol</li></ul> |
| Begränsningar för lösenord |<ul><li>minsta-8 tecken och högst 16 tecken</li><li>**Starka lösenord**: kräver 3 utanför 4 av hello följande:<ul><li>Gemener</li><li>Versaler</li><li>Siffror (0-9)</li><li>Symboler (Se ovanstående begränsningar för lösenord)</li></ul></li></ul> |
| Giltighetstiden för lösenord |<ul><li>Standardvärde: **90** dagar </li><li>Värdet kan konfigureras med hello Set-MsolPasswordPolicy cmdlet från hello Azure Active Directory-modulen för Windows PowerShell.</li></ul> |
| Meddelande om lösenords upphör att gälla |<ul><li>Standardvärde: **14** dagar (tills lösenordet upphör att gälla)</li><li>Värdet kan konfigureras med hello Set-MsolPasswordPolicy cmdlet.</li></ul> |
| Lösenordet upphör att gälla |<ul><li>Standardvärde: **FALSKT** dagar (anger att lösenordet upphör att gälla är aktiverad) </li><li>Värdet kan konfigureras för enskilda användarkonton med hello Set-MsolUser cmdlet. </li></ul> |
| Lösenordet **ändra** historik |Lösenordet **kan** användas igen när **ändra** ett lösenord. |
| Lösenordet **återställa** historik | Lösenordet **kan** användas igen när **återställer** glömt lösenordet. |
| Kontoutelåsning |Efter 10 misslyckade inloggningsförsök (fel lösenord), kommer användaren hello utelåst under en minut. Ytterligare inloggningsförsök felaktiga låser hello användaren för att öka varaktighet. |

## <a name="set-password-expiration-policies-in-azure-active-directory"></a>Ange lösenordet upphör att gälla principer i Azure Active Directory

En global administratör för en Microsoft-molntjänst kan använda hello Microsoft Azure Active Directory-modulen för Windows PowerShell tooset in att användarlösenord inte tooexpire. Du kan också använda Windows PowerShell cmdlets tooremove hello-upphör aldrig konfiguration eller toosee vilka användarlösenord ställs in inte tooexpire. Den här vägledningen gäller tooother providers som Microsoft Intune och Office 365 som också förlitar sig på Microsoft Azure Active Directory för identitets- och directory services.

> [!NOTE]
> Endast lösenord för användarkonton som inte är synkroniserade med katalogsynkronisering kan konfigureras toonot går ut. Läs mer om katalogsynkronisering[Anslut AD med Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>
>

## <a name="set-or-check-password-policies-using-powershell"></a>Ange eller kontrollera lösenordsprinciper med hjälp av PowerShell

tooget igång, du behöver för[ladda ned och installera hello Azure AD PowerShell-modulen](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0). När du har installerat kan följa du hello stegen nedan tooconfigure varje fält.

### <a name="how-toocheck-expiration-policy-for-a-password"></a>Hur toocheck förfalloprincipen för lösenord
1. Ansluta tooWindows PowerShell med administratörsbehörighet för ditt företag.
2. Kör något av följande kommandon hello:

   * toosee om en enskild användares lösenord har angetts toonever upphör, kör följande cmdlet med hjälp av användarens huvudnamn (UPN)-hello hello (till exempel aprilr@contoso.onmicrosoft.com) eller hello användar-ID hello användare som du vill toocheck:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`
   * toosee Hej ”lösenordet upphör aldrig att gälla” inställningen för alla användare, kör följande cmdlet hello:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

### <a name="set-a-password-tooexpire"></a>Ange ett lösenord tooexpire

1. Ansluta tooWindows PowerShell med administratörsbehörighet för ditt företag.
2. Kör något av följande kommandon hello:

   * tooset hello lösenordet för en användare så att hello lösenordet upphör att gälla, kör följande cmdlet med hjälp av användarens huvudnamn (UPN)-hello hello eller hello användar-ID för hello användare:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`
   * tooset hello lösenord för alla användare i organisationen hello Använd så att de upphör att gälla, hello följande cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

### <a name="set-a-password-toonever-expire"></a>Ange ett lösenord toonever går ut

1. Ansluta tooWindows PowerShell med administratörsbehörighet för ditt företag.
2. Kör något av följande kommandon hello:

   * tooset hello lösenordet för en användare toonever upphör, kör följande cmdlet med hjälp av användarens huvudnamn (UPN)-hello hello eller hello användar-ID för hello användare:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`
   * tooset hello lösenord för alla hello användare i en organisation toonever upphör, kör följande cmdlet hello:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Nästa steg

hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering
* [**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här
* [**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? -Svar tooquestions du alltid vill ha tooask
* [**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR
