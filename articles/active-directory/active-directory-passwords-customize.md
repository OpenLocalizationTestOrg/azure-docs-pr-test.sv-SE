---
title: 'Anpassa: Azure AD SSPR | Microsoft Docs'
description: "Anpassning av alternativ för Azure AD self service för lösenordsåterställning"
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
ms.openlocfilehash: 8b9c120815473b25140b8717f8fdd539c97ebb04
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a>Anpassa Azure AD-funktionerna för självbetjäning av återställning av lösenord

IT-proffs vill distribuera lösenordsåterställning via självbetjäning kan anpassa upplevelsen för att matcha sina användare.

## <a name="customize-the-contact-your-administrator-link"></a>Anpassa kontakten din administratör

Även om SSPR inte aktiveras återställa användare fortfarande en ”kontaktar du administratören” länka på lösenordet portal.  Klicka på den här länken e-post dina administratörer som ber om hjälp med att ändra användarens lösenord. Den här e-postmeddelande skickas till följande mottagare i följande ordning:

1. Om den **lösenordsadministratör** roll har tilldelats, administratörer med den här rollen meddelas
2. Om inga lösenordsadministratörer tilldelas, sedan administratörer med den **Användaradministratör** roll meddelas
3. Om ingen av de föregående rollerna har tilldelats, sedan **globala administratörer** meddelas

I samtliga fall meddelas högst 100 mottagare.

Ta reda på mer om olika administratören roller och tilldela dem finns i dokumentet [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles.md)

### <a name="disable-contact-your-administrator-emails"></a>Inaktivera Kontakta din administratör e-post

Om organisationen inte vill att få information om lösenord-administratörer återställa begäranden, kan följande konfiguration aktiveras

* Aktivera Självbetjäning för återställning av lösenord för alla användare. Det här alternativet är **för återställning av lösenord > Egenskaper**.
    * Om du inte vill att användare kan återställa sina lösenord kan du definiera åtkomst till en tom grupp **rekommenderas inte det här alternativet**.
* Anpassa länken supportavdelningen om du vill ange en Webbadress eller mailto: adress som användarna kan använda för att få hjälp. Det här alternativet är **för återställning av lösenord > Anpassning > anpassad supportavdelningen e-postadress eller URL**.

## <a name="customize-adfs-sign-in-page-for-sspr"></a>Anpassa AD FS-inloggningssida för SSPR

AD FS-administratörer kan lägga till en länk till deras inloggningssida med hjälp av vägledningen finns i artikeln [Lägg till inloggningssidan beskrivning](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).

Kommandot som följer på AD FS-servern lägger du till en länk till inloggningssidan för AD FS att tillåta användare att ange lösenordet för självbetjäning återställning arbetsflöde direkt.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-the-sign-in-and-access-panel-look-and-feel"></a>Anpassa inloggnings- och åtkomst panelen Utseende

Du kan anpassa logotypen som visas tillsammans med avbildningen inloggningssidan så att de passar din företagsanpassning när användarna har åtkomst till inloggningssidan.

Dessa bilder visas i följande fall:

* När en användare skriver sitt lösenord
* Användare kommer åt anpassade url
    * Genom att skicka ”wattimmar” parametern till lösenordet för att återställa sidan som ”https://login.microsoftonline.com/?whr=contoso.com”
    * Genom att skicka ”användarnamn” parametern till lösenordet återställa sidan som ”https://login.microsoftonline.com/?username=admin@contoso.com”

### <a name="graphics-details"></a>Information om grafik

Följande inställningar kan du ändra de visuella egenskaperna på sidan logga in och finns under **Azure Active Directory**, **företagets anpassning**, **Redigera företagsinformation**

* Inloggningssidan bilden ska vara en PNG- eller JPG-fil 1420 × 1200 bildpunkter och inte större än 500KB. Vi rekommenderar att det ska vara runt 200 KB för bästa resultat.
* Bakgrundsfärg för inloggningssidan används vid tidsfördröjning anslutningar och måste vara i formatet RGB hexadecimala.
* Banderoll ska vara en PNG- eller JPG-fil 60 × 280 bildpunkter och inte större än 10 KB.
* Kvadratisk logotyp (normalt och mörkt tema) PNG- eller JPG 240 x 240 (ändringsbara) inte är större än 10 KB.

### <a name="sign-in-text-options"></a>Textalternativ-inloggning

Följande inställningar kan du lägga till text på inloggningssidan relevanta för din organisation. De här inställningarna kan hittas **Azure Active Directory**, **företagets anpassning**, **Redigera företagsinformation**

* **Användaren namnet tipset** ersätter exempeltexten av someone@example.com med något mer lämpliga för dina användare, bör lämnas standard när stöder interna och externa användare
* **Inloggningssidan text** är högst 256 tecken. Den här texten visas var som helst inloggning för användare online och i Azure AD Join-upplevelsen i Windows 10. Använd den här texten för villkor för användning, instruktioner och tips för dina användare. **Alla kan se din inloggningssidan så ger inte någon känslig information.**

### <a name="keep-me-signed-in-disabled"></a>Håll mig inloggad inaktiverat

Alternativet tillåter ”Håll mig inloggad inaktiverad” användare att förbli inloggad i när de Stäng och öppna sina webbläsarfönster. Det här alternativet påverkar inte session livslängd. Den här inställningen finns under **Azure Active Directory > företagets anpassning > Redigera företagsinformation**.

Vissa funktioner i SharePoint Online och Office 2010 är beroende av användare kan den här kryssrutan. Om du dölja det här alternativet kan användarna få ytterligare och oväntat inloggning anvisningarna.

### <a name="directory-name"></a>Katalognamnet

Du kan ändra namnattributet under **Azure Active Directory > Egenskaper** att visa ett eget organisationsnamn som visas i portalen och automatisk kommunikation. Det här alternativet är mest synliga i form av automatiserade e-postmeddelanden i formulär som följer

* Eget namn i e-post ”Microsoft uppdrag CONTOSO demo”
* Ämnesrad i e-post ”CONTOSO demo konto e-Verifieringskod”

## <a name="next-steps"></a>Nästa steg

Följande länkar ger ytterligare information om lösenordsåterställning med Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data**](active-directory-passwords-data.md) – Förstå de data som krävs och hur de används för lösenordshantering
* [**Distribution**](active-directory-passwords-best-practices.md) – Planera och distribuera SSPR till dina användare med hjälp av informationen finns här
* [**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord
* [**Tillbakaskrivning av lösenord**](active-directory-passwords-writeback.md) – Hur fungerar tillbakaskrivning av lösenord tillsammans med din lokala katalog?
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Teknisk djupdykning** ](active-directory-passwords-how-it-works.md) – Ta en titt bakom kulisserna för att förstå hur det hela fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? – Svar på allt du någonsin velat fråga
* [**Felsökning** ](active-directory-passwords-troubleshoot.md) – Lär dig att lösa vanliga problem med SSPR

