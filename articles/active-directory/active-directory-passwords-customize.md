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
ms.openlocfilehash: 4762fffef040f9b409355f9ee0e8cc593e3eea0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a>Anpassa Azure AD-funktionerna för självbetjäning av återställning av lösenord

IT-proffs söka toodeploy lösenordsåterställning via självbetjäning kan användarna anpassa hello upplevelse toomatch.

## <a name="customize-hello-contact-your-administrator-link"></a>Anpassa hello Kontakta din administratör

Även om SSPR inte är aktiverade användare återställa fortfarande en ”kontaktar du administratören” länk på hello lösenord portal.  Klicka på den här länken e-post dina administratörer som ber om hjälp med att ändra hello användarens lösenord. Den här e-postmeddelande skickas toohello följande mottagare i hello följande ordning:

1. Om hello **lösenordsadministratör** roll har tilldelats, administratörer med den här rollen meddelas
2. Om inga lösenordsadministratörer tilldelas, sedan administratörer med hello **Användaradministratör** roll meddelas
3. Om inget av föregående hello-roller har tilldelats, sedan **globala administratörer** meddelas

I samtliga fall meddelas högst 100 mottagare.

toofind mer information om hello olika administratörsroller och hur tooassign dem finns hello dokumentet [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles.md)

### <a name="disable-contact-your-administrator-emails"></a>Inaktivera Kontakta din administratör e-post

Om organisationen inte vill att få information om lösenord-administratörer återställa begäranden, kan hello följande konfiguration aktiveras

* Aktivera Självbetjäning för återställning av lösenord för alla användare. Det här alternativet är **för återställning av lösenord > Egenskaper**.
    * Om du inte vill att användarna tooreset sina egna lösenord, du kan definiera åtkomst tooan tom grupp **rekommenderas inte det här alternativet**.
* Anpassa hello supportavdelningen länken tooprovide en Webbadress eller mailto: adress som användare kan använda tooget hjälp. Det här alternativet är **för återställning av lösenord > Anpassning > anpassad supportavdelningen e-postadress eller URL**.

## <a name="customize-adfs-sign-in-page-for-sspr"></a>Anpassa AD FS-inloggningssida för SSPR

AD FS-administratörer kan lägga till en länk tootheir inloggningssidan med hello vägledning finns i artikel hello [Lägg till inloggningssidan beskrivning](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).

Kommandot hello som följer på AD FS-servern lägger till en länk toohello ADFS inloggningssidan så att användare tooenter hello självbetjäning lösenord Återställ arbetsflöde direkt.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-hello-sign-in-and-access-panel-look-and-feel"></a>Anpassa hello inloggning och åtkomst panelen Utseende

Du kan anpassa hello logotyp som visas tillsammans med hello-inloggningssida avbildningen toofit din företagsanpassning när användarna har åtkomst till hello inloggningssidan.

Dessa bilder visas i hello följande omständigheter:

* När en användare skriver sitt lösenord
* Användare kommer åt anpassade url
    * Genom att skicka hello lösenordsåterställning ”wattimmar” parametern toohello sida som ”https://login.microsoftonline.com/?whr=contoso.com”
    * Genom att skicka hello ”användarnamn” parametern toohello lösenordsåterställning sida, som ”https://login.microsoftonline.com/?username=admin@contoso.com”

### <a name="graphics-details"></a>Information om grafik

hello följande inställningar kan du toochange hello visuella egenskaper hello inloggningssidan och kan hittas **Azure Active Directory**, **företagets anpassning**, **redigera företag anpassning**

* Inloggningssidan bilden ska vara en PNG- eller JPG-fil 1420 × 1200 bildpunkter och inte större än 500KB. Vi rekommenderar att den toobe runt 200 KB för bästa resultat.
* Bakgrundsfärg för inloggningssidan används vid tidsfördröjning anslutningar och måste vara i hello RGB-hexadecimalt format.
* Banderoll ska vara en PNG- eller JPG-fil 60 × 280 bildpunkter och inte större än 10 KB.
* Kvadratisk logotyp (normalt och mörkt tema) PNG- eller JPG 240 x 240 (ändringsbara) inte är större än 10 KB.

### <a name="sign-in-text-options"></a>Textalternativ-inloggning

hello följande inställningar kan du tooadd text toohello inloggningssidan relevanta tooyour organisation. De här inställningarna kan hittas **Azure Active Directory**, **företagets anpassning**, **Redigera företagsinformation**

* **Användaren namnet tipset** ersätter hello exempeltexten av someone@example.com med något mer lämpliga för dina användare, bör toobe vänstra standard när stöder interna och externa användare
* **Inloggningssidan text** är högst 256 tecken. Den här texten visas någonstans inloggning för användare online och i hello Azure AD Join-upplevelsen i Windows 10. Använd den här texten för villkor för användning, instruktioner och tips för dina användare. **Alla kan se din inloggningssidan så ger inte någon känslig information.**

### <a name="keep-me-signed-in-disabled"></a>Håll mig inloggad inaktiverat

hello alternativet ”Jag vill förbli inloggad inaktiverat” kan användare tooremain inloggad när de Stäng och öppna sina webbläsarfönster. Det här alternativet påverkar inte session livslängd. Den här inställningen finns under **Azure Active Directory > företagets anpassning > Redigera företagsinformation**.

Vissa funktioner i SharePoint Online och Office 2010 är beroende av användare kan toocheck den här rutan. Om du dölja det här alternativet kan användarna få ytterligare och oväntat inloggning anvisningarna.

### <a name="directory-name"></a>Katalognamnet

Du kan ändra hello namnattributet under **Azure Active Directory > Egenskaper** tooshow ett eget organisationsnamn ses i hello portal och automatisk kommunikation. Det här alternativet är mest synliga i hello form av automatiserade e-postmeddelanden i hello formulär som följer

* Eget namn i e-post ”Microsoft uppdrag CONTOSO demo”
* Ämnesrad i e-post ”CONTOSO demo konto e-Verifieringskod”

## <a name="next-steps"></a>Nästa steg

hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering
* [**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här
* [**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord
* [**Tillbakaskrivning av lösenord**](active-directory-passwords-writeback.md) – Hur fungerar tillbakaskrivning av lösenord tillsammans med din lokala katalog?
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? -Svar tooquestions du alltid vill ha tooask
* [**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR

