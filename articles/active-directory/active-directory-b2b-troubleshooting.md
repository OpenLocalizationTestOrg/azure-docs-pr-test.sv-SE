---
title: aaaTroubleshooting Azure Active Directory B2B-samarbete | Microsoft Docs
description: "Kompensation för vanliga problem med Azure Active Directory B2B-samarbete"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/25/2017
ms.author: sasubram
ms.openlocfilehash: 6fcfd7e543cd7bb833225f8aa56e332e7a989faf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>Felsökning av Azure Active Directory B2B-samarbete

Här följer några lösningar på vanliga problem med Azure Active Directory (AD Azure) B2B-samarbete.


## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-hello-people-picker"></a>Jag har lagt till en extern användare men se inte dem i den globala adressboken eller hello personer objektväljare

I fall där externa användare inte fylls i listan hello kan hello-objekt ta några minuter tooreplicate.

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a>Gästanvändare B2B syns inte i SharePoint Online/OneDrive personer väljare 
 
hello möjlighet toosearch för befintliga gästanvändare i hello SharePoint Online (SPO) personer väljare är av äldre standardbeteendet för toomatch.

Du kan aktivera den här funktionen med hjälp av hello inställningen 'ShowPeoplePickerSuggestionsForGuestUsers' på hello klient och webbplatssamling nivå. Du kan ange hello-funktionen med hjälp av hello Set SPOTenant och Set-SPOSite cmdlet: ar, vilket tillåter medlemmar toosearch alla befintliga gästanvändare i hello directory. Ändringar i hello klientorganisationsområde påverkar inte redan etablerade SPO platser.

## <a name="invitations-have-been-disabled-for-directory"></a>Inbjudningar har inaktiverats för katalogen

Kontrollera att ditt användarkonto är auktoriserade tooinvite externa användare under Inställningar om du får ett meddelande om att du inte har behörigheter tooinvite användare:

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

Om du nyligen har ändrat inställningarna eller tilldelad hello gäst bjuder in rollen tooa användare det kan finnas en fördröjning i 60 15 minut innan hello ändringarna börjar gälla.

## <a name="hello-user-that-i-invited-is-receiving-an-error-during-redemption"></a>hello-användaren som jag inbjuden tar emot ett fel under inlösning

Vanliga fel är:

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>När en användare Admin har otillåten EmailVerified användare skapas i klientorganisationen

När användarna vars organisation använder Azure Active Directory, men där hello specifikt användarkonto finns inte (till exempel hello användaren finns inte i Azure AD contoso.com). Hej administratör contoso.com kan har en princip för att förhindra att användare skapas. hello användaren måste kontrollera med deras admin toodetermine om externa användare tillåts. hello admin för extern användare behöva tooallow e-post verifieras användare i sin domän (se den här [artikel](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) på e-post verifieras användare).

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>Externa användare finns inte redan i en federerad domän

Om du använder federation-autentisering och hello användaren inte redan finns i Azure Active Directory, kan du inte bjudas in hello användare.

i detta fall är hello tooresolve extern användare administratören måste synkronisera hello användarens konto tooAzure Active Directory.

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a>Hur fungerar\#', som inte är vanligtvis ett giltigt tecken synkronisering med Azure AD?

”\#” är ett reserverat tecken i UPN-namnen för Azure AD B2B-samarbete eller externa användare eftersom hello inbjuden konto user@contoso.com blir user_contoso.com#EXT@fabrikam.onmicrosoft.com. Därför \# i UPN-namn som kommer från lokala tillåts inte toosign i toohello Azure-portalen. 

## <a name="i-receive-an-error-when-adding-external-users-tooa-synchronized-group"></a>Ett felmeddelande när du lägger till externa användare tooa synkroniserar grupp

Externa användare kan läggas till endast för ”tilldelade” eller ”säkerhetsgrupper” och inte toogroups som hanterat lokalt.

## <a name="my-external-user-did-not-receive-an-email-tooredeem"></a>Min extern användare tog inte emot ett e-tooredeem

Kontrollera hello bjudits in med sitt Internetleverantören eller tillåts skräppost filter tooensure som hello följande adress:Invites@microsoft.com

## <a name="i-notice-that-hello-custom-message-does-not-get-included-with-invitation-messages-at-times"></a>Jag har sett att anpassade hello-meddelande inte får ingå i inbjudan meddelanden vid tillfällen

toocomply med sekretesslagar våra API: er inte inkluderar anpassade meddelanden i hello e-postinbjudan när:

- hello bjuder in har inte en e-postadress i hello bjuda in klient
- När en apptjänst-principal skickar hello inbjudan

Om det här scenariot är viktiga tooyou kan du ignorera e-postinbjudan våra API och skicka den via hello e-mekanismen för ditt val. Kontakta din organisations juridiskt biträde toomake till e-postmeddelandet du skickar det här sättet även uppfyller sekretesslagar.

## <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:

* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?](active-directory-b2b-admin-add-users.md)
* [Hur lägger informationsarbetare till B2B-samarbete användare?](active-directory-b2b-iw-add-users.md)
* [hello-element i hello e-postinbjudan för B2B-samarbete](active-directory-b2b-invitation-email.md)
* [B2B-samarbete inbjudan inlösning](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B-samarbete och licensiering](active-directory-b2b-licensing.md)
* [Vanliga och frågor svar om Azure Active Directory B2B-samarbete](active-directory-b2b-faq.md)
* [Azure Active Directory B2B-samarbete API och anpassning](active-directory-b2b-api.md)
* [Multi-Factor Authentication för användare av B2B-samarbete](active-directory-b2b-mfa-instructions.md)
* [Lägg till B2B-samarbete användare utan inbjudan](active-directory-b2b-add-user-without-invite.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
