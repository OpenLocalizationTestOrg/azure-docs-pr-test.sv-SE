---
title: "aaaAzure Active Directory B2B-samarbete vanliga frågor och svar | Microsoft Docs"
description: "Få svar toofrequently frågor och svar om Azure Active Directory B2B-samarbete."
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
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 4412bbc65274ff01782db81dfcc8818a6362ea7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-faqs"></a>Azure Active Directory B2B-samarbete vanliga frågor och svar

Dessa vanliga frågor och svar om Azure Active Directory (AD Azure) business-to-business (B2B) samarbete är regelbundet uppdaterade tooinclude nya artiklar.

### <a name="is-azure-ad-b2b-collaboration-available-in-hello-azure-classic-portal"></a>Finns Azure AD B2B-samarbete på hello klassiska Azure-portalen?
Nej. Azure AD B2B-samarbetsfunktioner är endast tillgängliga i hello [Azure-portalen](https://portal.azure.com) och i hello [åtkomstpanelen](https://myapps.microsoft.com/). 

### <a name="can-we-customize-our-sign-in-page-so-it-is-more-intuitive-for-our-b2b-collaboration-guest-users"></a>Kan vi anpassa vår inloggningssida så att det är mer intuitiv för gästanvändare våra B2B-samarbete?
Absolut! Se vår [blogginlägget om den här funktionen](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/). Mer information om hur toocustomize organisationens inloggning kan du se [lägga till företagsanpassning toosign i och åtkomstpanelsidan](active-directory-add-company-branding.md).

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>Har B2B-samarbete användare åtkomst till SharePoint Online och OneDrive?
Ja. Hello möjlighet toosearch för befintliga gästanvändare i SharePoint Online med hjälp av Väljaren för hello personer är dock **av** som standard. Ange tooturn på hello alternativet toosearch för befintliga gästanvändare **ShowPeoplePickerSuggestionsForGuestUsers** för**på**. Du kan aktivera den här inställningen antingen på hello klient nivå eller på hello webbplatssamlingsnivå. Du kan ändra den här inställningen genom att använda hello Set SPOTenant och Set-SPOSite-cmdlets. Med dessa cmdlets söka medlemmar alla befintliga gästanvändare i hello directory. Ändringar i hello klientorganisationsområde påverkar inte SharePoint Online-platserna som redan har etablerats.

### <a name="is-hello-csv-upload-feature-still-supported"></a>Är hello CSV överför funktionen fortfarande stöds?
Ja. Mer information om funktionen hello CSV-fil överför finns [PowerShell-exempel](active-directory-b2b-code-samples.md).

### <a name="how-can-i-customize-my-invitation-emails"></a>Hur kan jag Anpassa min inbjudan e-post?
Du kan anpassa nästan allt om hello bjuder in processen med hjälp av hello [B2B inbjudan API: er](active-directory-b2b-api.md).

### <a name="can-an-invited-external-user-leave-hello-organization-after-being-invited"></a>En extern inbjudna användare kan göra hello organisation när du har fått en inbjudan?
hello bjuda in organisationens administratör kan ta bort en B2B-samarbete gästanvändare från sina kataloger, men hello gästanvändaren kan inte lämna hello bjuda in organisationskatalog sig själva. 

### <a name="can-guest-users-reset-their-multi-factor-authentication-method"></a>Gästanvändare kan återställa sina multifaktorautentisering metoden?
Ja. Gästanvändare kan återställa sina multifaktorautentisering metoden hello samma sätt som vanliga användare behöver.

### <a name="which-organization-is-responsible-for-multi-factor-authentication-licenses"></a>Vilken organisation ansvarar för multifaktorautentisering licenser?
hello bjuda in organisation utför multifaktorautentisering. hello bjuda in organisation måste se till att hello organisationen har tillräckligt många licenser för sina B2B-användare som använder multifaktorautentisering.

### <a name="what-if-a-partner-organization-already-has-multi-factor-authentication-set-up-can-we-trust-their-multi-factor-authentication-and-not-use-our-own-multi-factor-authentication"></a>Vad händer om en organisation redan har multifaktorautentisering konfigurera? Kan vi förtroende för sina multifaktorautentisering och inte använda egna multifaktorautentisering?
Den här funktionen är planerad för senare versioner, så som sedan du kan välja specifika partner tooexclude från (hello bjuda in organisationens) Multi-Factor authentication.

### <a name="how-can-i-use-delayed-invitations"></a>Hur kan jag använda fördröjd inbjudningar?
En organisation kanske vill tooadd B2B-samarbete användare, etablera dem tooapplications vid behov och skicka inbjudningar. Du kan använda hello B2B-samarbete inbjudan API toocustomize hello onboarding arbetsflöde.

### <a name="can-i-make-a-guest-user-a-limited-administrator"></a>Kan jag göra en gästanvändare en begränsad administratör?
Absolut. Mer information finns i [lägger till tooa gästrollen](active-directory-users-assign-role-azure-portal.md).

### <a name="does-azure-ad-b2b-collaboration-allow-b2b-users-tooaccess-hello-azure-portal"></a>Tillåter att Azure AD B2B-samarbete B2B användare tooaccess hello Azure-portalen?
Om en användare har tilldelats hello roll begränsad administratör eller global administratör, behöver inte B2B-samarbete användare någon åtkomst toohello Azure-portalen. Dock B2B-samarbete användare som är tilldelade hello roll begränsad administratör eller global administratör kan komma åt hello-portalen. Även om en gästanvändare som inte har tilldelats någon av administratörsrollerna använder hello portal kan hello användaren kanske kan tooaccess vissa delar av hello upplevelse. hello gäst användarroll har vissa behörigheter i hello directory.

### <a name="can-i-block-access-toohello-azure-portal-for-guest-users"></a>Kan jag blockera åtkomst toohello Azure-portalen för gästanvändare?
Visst! När du konfigurerar den här principen kan vara försiktig tooavoid av misstag blockerar åtkomst toomembers och administratörer.
tooblock gästanvändare åtkomst till toohello [Azure-portalen](https://portal.azure.com), använda en princip för villkorlig åtkomst i hello Windows Azure klassisk distribution modellen API:
1. Ändra hello **alla användare** gruppen så att den innehåller bara medlemmar.
  ![Ändra hello grupp skärmbild](media/active-directory-b2b-faq/modify-all-users-group.png)
2. Skapa en dynamisk grupp som innehåller gästanvändare.
  ![Skapa grupp skärmbild](media/active-directory-b2b-faq/group-with-guest-users.png)
3. Ställa in en villkorlig åtkomst princip tooblock gästanvändare från att komma åt hello-portalen, enligt följande hello video:
  
  > [!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player] 

### <a name="does-azure-ad-b2b-collaboration-support-multi-factor-authentication-and-consumer-email-accounts"></a>Stöder Azure AD B2B-samarbete multifaktorautentisering och konsumenten e-postkonton?
Ja. Multi-Factor authentication och konsumenten e-postkonton stöds både för Azure AD B2B-samarbete.

### <a name="do-you-plan-toosupport-password-reset-for-azure-ad-b2b-collaboration-users"></a>Planeras toosupport lösenordsåterställning för användare i Azure AD B2B-samarbete?
Ja. Här följer hello viktig information för lösenordsåterställning via självbetjäning (SSPR) för en B2B-användare som är inbjuden från en partnerorganisation:
 
* SSPR uppstår endast i hello identitet innehavare för hello B2B-användare.
* Om hello identitet innehavaren är ett Microsoft-konto, används hello SSPR mekanism för Microsoft-konto.
* Om hello identitet klient är en just-in-time (JIT) eller ”viral” klient, skickas ett e-postmeddelande för återställning av lösenord.
* För andra klienter följs hello SSPR standardprocessen för B2B-användare. Som member SSPR för B2B användare hello gäller hello resursen blockeras innehavare. 

### <a name="is-password-reset-available-for-guest-users-in-a-just-in-time-jit-or-viral-tenant-who-accepted-invitations-with-a-work-or-school-email-address-but-who-didnt-have-a-pre-existing-azure-ad-account"></a>Lösenordet återställs tillgängliga för gästanvändare i en just-in-time (JIT) eller ”viral” klient som har accepterat inbjudan med ett arbets eller skolans e-postadress, men som inte har en befintlig Azure AD-kontot?
Ja. Ett lösenord för återställning av e-post kan skickas som tillåter en användare tooreset sina lösenord i hello JIT innehavare.

### <a name="does-microsoft-dynamics-crm-provide-online-support-for-azure-ad-b2b-collaboration"></a>Tillhandahåller Microsoft Dynamics CRM online-support för Azure AD B2B-samarbete?
Microsoft Dynamics CRM ger för närvarande inte support online för Azure AD B2B-samarbete. Men planerar vi toosupport detta i hello framtida.

### <a name="what-is-hello-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Vad är hello livslängden för en inledande lösenordet för en nyligen skapade B2B-samarbete användare?
Azure AD har en fast uppsättning tecken och lösenordssäkerhet kontoutelåsning krav som gäller både tooall Azure AD cloud användarkonton. Användarkonton i molnet är konton som inte är federerat med en annan identitetsleverantör som 
* Microsoft-konto
* Facebook
* Active Directory Federation Services
* Ett annat moln-klient (för B2B-samarbete)

För federerade konton beroende lösenordsprincip hello-princip som tillämpas i inställningarna för hello lokalt innehavare och hello användarens Microsoft-kontot.

### <a name="an-organization-might-want-toohave-different-experiences-in-their-applications-for-tenant-users-and-guest-users-is-there-standard-guidance-for-this-is-hello-presence-of-hello-identity-provider-claim-hello-correct-model-toouse"></a>En organisation kanske vill toohave olika funktioner i sina program för klient-användare och gästanvändare. Är det standard riktlinjer för detta? Är hello förekomst av hello identitet providern anspråk hello rätt modell toouse?
 Gästanvändare kan använda alla identity-providern tooauthenticate. Mer information finns i [egenskaperna för en B2B-samarbete användare](active-directory-b2b-user-properties.md). Använd hello **UserType** egenskapen toodetermine användarupplevelsen. Hej **UserType** anspråk är för närvarande inte ingår i hello-token. Program ska använda hello Graph API tooquery hello directory för hello användar- och tooget hello UserType.

### <a name="where-can-i-find-a-b2b-collaboration-community-tooshare-solutions-and-toosubmit-ideas"></a>Var hittar jag en B2B-samarbete community tooshare lösningar och toosubmit idéer?
Vi lyssnar hela tiden tooyour feedback, tooimprove B2B-samarbete. Vi ber du tooshare användaren scenarier, bästa praxis och vad du tycker om Azure AD B2B-samarbete. Anslut hello diskussion i hello [Microsoft teknisk Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).
 
Vi erbjuder även du toosubmit din idéer och rösta på framtida funktioner på [B2B-samarbete idéer](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas).

### <a name="can-we-send-an-invitation-that-is-automatically-redeemed-so-that-hello-user-is-just-ready-toogo-or-does-hello-user-always-have-tooclick-through-toohello-redemption-url"></a>Kan vi skicka en inbjudan som automatiskt Inlöst, så som hello användare är bara ”klar toogo”? Eller hello användare alltid har tooclick via toohello inlösning URL?
Inbjudningar som skickas av en användare i hello bjuda in organisation som även är medlem i hello kontopartner-organisationen kräver inte inlösning av hello B2B-användare.

Vi rekommenderar att bjuda in användare från hello partner organisation toojoin hello bjuda in organisation. [Lägga till den här toohello gäst bjuder in användarrollen i hello resursorganisationen](active-directory-b2b-add-guest-to-role.md). Den här användaren kan erbjuda andra användare i hello partnerorganisation med hjälp av inloggning hello UI, PowerShell-skript eller API: er. Sedan B2B-samarbete användare från den organisationen är inte obligatoriska tooredeem deras inbjudan.

### <a name="how-does-b2b-collaboration-work-when-hello-invited-partner-is-using-federation-tooadd-their-own-on-premises-authentication"></a>Hur fungerar B2B-samarbete när hello inbjuden partnern använder federation tooadd sina egna lokal autentisering?
Om hello partnern har en Azure AD-klient som är federerat toohello lokal infrastruktur för autentisering, lokalt enkel inloggning (SSO) uppnås automatiskt. Om hello partner inte har en Azure AD-klient, skapas en Azure AD-kontot för nya användare. 

### <a name="i-thought-azure-ad-b2b-didnt-accept-gmailcom-and-outlookcom-email-addresses-and-that-b2c-was-used-for-those-kinds-of-accounts"></a>Jag tror Azure AD B2B accepterade gmail.com och outlook.com-e-postadresser och att B2C användes för dessa typer av konton?
Vi tar bort hello skillnader mellan B2B och företag att företagets (B2C) samarbete enligt vilken identiteter stöds. hello-identitet som används är inte en bra toochoose mellan med hjälp av B2B eller B2C. Information om hur du väljer ett samarbetsalternativ finns [jämför B2B-samarbete och B2C i Azure Active Directory](active-directory-b2b-compare-b2c.md).

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>Vilka program och tjänster stöder Azure B2B gästanvändare?
Alla Azure AD-integrerade program stöder Azure B2B gästanvändare. 

### <a name="can-we-force-multi-factor-authentication-for-b2b-guest-users-if-our-partners-dont-have-multi-factor-authentication"></a>Kan vi framtvinga multifaktorautentisering för B2B gästanvändare om våra samarbetspartners inte har multifaktorautentisering?
Ja. Mer information finns i [villkorlig åtkomst för användare för B2B-samarbete](active-directory-b2b-mfa-instructions.md).

### <a name="in-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-can-we-do-this-in-azure"></a>I SharePoint, kan du definiera en lista för ”Tillåt” eller ”neka” för externa användare. Kan vi göra detta i Azure?
Ja. Azure AD B2B-samarbete stöder listor Tillåt och neka listor. 

### <a name="what-licenses-do-we-need-toouse-azure-ad-b2b"></a>Licenser gör vi behöver toouse Azure AD B2B?
För information om vad licenser organisationen måste toouse Azure AD B2B, se [Azure Active Directory B2B-samarbete licensiering vägledning](active-directory-b2b-licensing.md).

### <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:

* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hur lägger Azure AD-administratörer till B2B-samarbete användare?](active-directory-b2b-admin-add-users.md)
* [Hur lägger informationsarbetare till B2B-samarbete användare?](active-directory-b2b-iw-add-users.md)
* [hello-element i hello e-postinbjudan för B2B-samarbete](active-directory-b2b-invitation-email.md)
* [B2B-samarbete inbjudan inlösning](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B-samarbete och licensiering](active-directory-b2b-licensing.md)
* [Felsökning av Azure AD B2B-samarbete](active-directory-b2b-troubleshooting.md)
* [Azure AD B2B-samarbete API och anpassning](active-directory-b2b-api.md)
* [Multi-Factor Authentication för användare av B2B-samarbete](active-directory-b2b-mfa-instructions.md)
* [Lägg till B2B-samarbete användare utan inbjudan](active-directory-b2b-add-user-without-invite.md)
* [Artikelindex för programhantering i Azure AD](active-directory-apps-index.md)
