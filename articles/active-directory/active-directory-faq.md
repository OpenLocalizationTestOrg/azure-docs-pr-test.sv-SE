---
title: "aaaAzure Active Directory vanliga frågor och svar | Microsoft Docs"
description: "Azure Active Directory vanliga frågor och svar svar på frågor om hur tooaccess Azure och Azure Active Directory, lösenordshantering och program åt."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: b8207760-9714-4871-93d5-f9893de31c8f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/16/2017
ms.author: markvi
ms.openlocfilehash: 63c30c4aeda4551bf02c6b968f98cded5a3b2c16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-faq"></a>Vanliga frågor och svar om Azure Active Directory
Azure Active Directory (Azure AD) är en omfattande IDaaS-lösning (Identity as a Service) som omfattar alla aspekter relaterade till identiteter, åtkomsthantering och säkerhet.

Mer information finns i [Vad är Azure Active Directory?](active-directory-whatis.md).


## <a name="access-azure-and-azure-active-directory"></a>Kom åt Azure och Azure Active Directory
**F: Varför visas ”inga prenumerationer hittades” när jag försöker tooaccess Azure AD i hello klassiska Azure-portalen?**

**S:** tooaccess Hej klassiska Azure-portalen, alla användare måste ha behörigheter med en Azure-prenumeration. Om du har en betald Office 365 eller Azure AD-prenumeration går för[http://aka.ms/accessAAD](http://aka.ms/accessAAD) för engångsaktiveringen. I annat fall behöver du ett kostnadsfritt tooactivate [Azure-konto](https://azure.microsoft.com/pricing/free-trial/) eller en betald prenumeration.

Mer information finns i:

* [Hur Azure-prenumerationer är associerade med Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Hantera hello katalogen för din Office 365-prenumeration i Azure](active-directory-manage-o365-subscription.md)

- - -
**F: Vad är hello förhållandet mellan Azure AD och Office 365 och Azure?**

**S:** Azure AD innehåller gemensamma identitets- och åtkomstfunktioner tooall webbtjänster. Om du använder Office 365, Microsoft Azure, Intune, eller andra, du är redan använder Azure AD toohelp aktivera inloggning och åtkomsthantering för alla dessa tjänster.

Alla användare som har ställts in toouse webbtjänster definieras som användarkonton i en eller flera Azure AD-instanser. Du kan ställa in dessa konton för kostnadsfria Azure AD-funktioner, till exempel programåtkomst i molnet.

Azure AD-betaltjänsterna som Enterprise Mobility + Security kompletterar andra webbtjänster som Office 365 och Microsoft Azure med heltäckande hanterings- och säkerhetslösningar i företagsklass.
- - -
**F: Varför kan logga in toohello Azure-portalen men inte hello klassiska Azure-portalen?**

**S:** hello Azure-portalen kräver inte en giltig prenumeration och hello klassiska portalen kräver en giltig prenumeration.  Om du inte har en prenumeration kan du logga in toohello klassiska portalen.
- - -
**F: Vad är hello skillnader mellan administratör för prenumeration och Directory-administratör?**

**S:** som standard du tilldelas hello prenumeration administratörsroll när du registrerar dig för Azure. En prenumerationsadministratör för kan använda ett Microsoft-konto eller ett arbets eller skolkonto från hello-katalog som hello Azure-prenumeration är associerad med.  Den här rollen är auktoriserade toomanage tjänster i hello Azure-portalen.

Om andra behöver toosign i och komma åt tjänster med hjälp av Hej samma prenumeration, du kan lägga till dem som medadministratörer. Den här rollen har hello samma behörighet som hello tjänstadministratör, men kan inte ändra hello associationen mellan prenumerationer tooAzure kataloger.  Ytterligare information om prenumerationsadministratörer finns [hur tooadd eller ändra Azure-administratörsroller](../billing-add-change-azure-subscription-administrator.md) och [hur Azure-prenumerationer är associerade med Azure Active Directory](active-directory-how-subscriptions-associated-directory.md).


Azure AD har en annan uppsättning administrativa roller toomanage hello katalog- och identitetsrelaterade funktioner.  Dessa administratörer ska ha åtkomst toovarious funktioner i hello Azure-portalen eller hello klassiska Azure-portalen. Hej administratör roll avgör vad de kan göra, som att skapa eller redigera användare, Tilldela administratörsroller tooothers, återställa användarlösenord, hantera användarlicenser eller hantera domäner.  Mer information om Azure AD-katalogadministratörer och deras roller finns i [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles.md).

Dessutom kompletterar Azure AD-betaltjänster som Enterprise Mobility + Security andra webbtjänster, som Office 365 och Microsoft Azure, med heltäckande hanterings- och säkerhetslösningar i företagsklass.

- - -
**F: Finns det någon rapport som visar när mina Azure AD-användarlicenser upphör att gälla?**

**S:** Nej.  Det här är inte tillgängligt för närvarande.

- - -

## <a name="get-started-with-hybrid-azure-ad"></a>Kom igång med en Azure AD-hybridlösning


**F: Hur lämnar jag en klient när jag har lagts till som medarbetare?**

**S:** när du har lagts till tooanother organisationens klient som en samarbetspartner, kan du använda hello ”klient switcher” i hello övre högra tooswitch mellan klienter behålls.  Det finns inget sätt tooleave hello bjuda in organisation för närvarande och Microsoft arbetar på att tillhandahålla den här funktionen.  Tills den här funktionen finns tillgänglig, kan du be hello bjuda in organisation tooremove du från klienten.
- - -
**F: hur kan jag ansluta min lokala katalog tooAzure AD?**

**S:** kan du ansluta din lokala katalog tooAzure AD med hjälp av Azure AD Connect.

Mer information finns i [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

- - -
**F: Hur konfigurerar jag enkel inloggning (SSO) mellan min lokala katalog och mina molnprogram?**

**S:** behöver du bara tooset in enkel inloggning (SSO) mellan din lokala katalog och Azure AD. Så länge som du kommer åt dina molnprogram via Azure AD enheter hello service automatiskt användarna toocorrectly autentiseras med deras lokala autentiseringsuppgifter.

Du kan enkelt implementera enkel inloggning (SSO) från det lokala systemet med federationslösningar som Active Directory Federation Services (AD FS) eller genom att konfigurera hash-synkronisering av lösenord. Du kan enkelt distribuera båda alternativen med hjälp av hello Azure AD Connect-konfigurationsguiden.

Mer information finns i [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

- - -
**F: Tillhandahåller Azure AD en självbetjäningsportal för användare i organisationen?**

**S:** Ja, Azure AD innehåller hello [Azure AD-åtkomstpanelen](http://myapps.microsoft.com) för självbetjäning och programåtkomst. Om du är en Office 365-kund kan hitta du många av hello samma funktioner i hello Office 365-portalen.

Mer information finns i [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

- - -
**F: Kan Azure AD hjälpa mig att hantera min lokala infrastruktur?**

**S:** Ja. hello Azure AD Premium edition ger Azure AD Connect Health. Azure AD Connect Health hjälper dig att övervaka och få insyn i din lokala identitet infrastruktur och hello synkroniseringstjänsterna.  

Mer information finns i [övervaka din lokala identitet infrastruktur och synkroniseringstjänster i molnet hello](active-directory-aadconnect-health.md).  

- - -
## <a name="password-management"></a>Lösenordshantering
**F: Kan jag använda tillbakaskrivning av lösenord i Azure AD utan lösenordssynkronisering? (I det här fallet är det möjligt toouse Azure AD lösenordsåterställning via självbetjäning (SSPR) med lösenord återskrivning och inte lagra lösenord i molnet hello?)**

**S:** behöver du inte toosynchronize din Active Directory lösenord tooAzure AD tooenable tillbakaskrivning. Förlitar sig på hello lokalt directory tooauthenticate hello användaren Azure AD enkel inloggning (SSO) i en federerad miljö. Det här scenariot kräver inte hello lokala lösenord toobe spåras i Azure AD.

- - -
**F: hur lång tid tar det för ett lösenord toobe skrivs tillbaka tooActive Directory lokalt?**

**S:** Tillbakaskrivningen av lösenord fungerar i realtid.

Mer information finns i [Komma igång med lösenordshantering](active-directory-passwords-getting-started.md).

- - -
**F: Kan jag använda tillbakaskrivning av lösenord med lösenord som hanteras av en administratör?**

**S:** Ja, om du har tillbakaskrivning av lösenord aktiverat hello lösenordsåtgärder som utförs av en administratör skrivs tillbaka tooyour lokala miljö.  

Fler svar toopassword-relaterade frågor finns i [vanliga frågor och svar om lösenordshantering](active-directory-passwords-faq.md).
- - -
**F: Vad gör jag om jag inte kommer ihåg mina befintliga Office 365-/ Azure AD-lösenord uppstod toochange mitt lösenord?**

**S:** För den här typen av situation finns det ett par alternativ.  Använd självbetjäning för återställning av lösenord (SSPR) om det är tillgängligt.  Huruvida SSPR fungerar eller ej beror på hur det är konfigurerat.  Mer information finns i [hur hello lösenordsåterställning portal arbete](active-directory-passwords-best-practices.md).

För Office 365-användare kan administratören återställa hello lösenord med hjälp av hello steg som beskrivs i [återställa användarlösenord](https://support.office.com/en-us/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US).

Administratörer kan återställa lösenord med hjälp av en av följande hello för Azure AD-konton:

- [Återställ konton i hello Azure-portalen](active-directory-users-reset-password-azure-portal.md)
- [Återställ konton i hello klassiska portalen](active-directory-create-users-reset-password.md)
- [Använda PowerShell](/powershell/module/msonline/set-msoluserpassword?view=azureadps-1.0)


- - -
## <a name="security"></a>Säkerhet
**Fråga: Låses konton efter ett visst antal misslyckade försök eller finns det en mer avancerad strategi?**</br>
Vi använder en mer avancerad strategi toolock konton.  Detta baseras på hello IP hello begäran och hello lösenorden. hello varaktighet hello kontoutelåsning ökar baserat på hello sannolikheten att det är en attack.  

**F: vissa (common) lösenord hämta nekas med hello meddelanden 'lösenordet har använt toomany gånger', refererar toopasswords som används i hello aktuell active directory?**</br>
Detta refererar toopasswords globalt vanliga som varianter av ”Password” och ”123456”.

**Fråga: Blockeras inloggningsbegäranden från misstänkta källor (botnät, Tor-slutpunkt) på en B2C-klient eller kräver detta att klienten har en Basic- eller Premium-utgåva?**</br>
Vi har en gateway som filtrerar begäranden och som ger ett visst skydd mot botnät. Den används för alla B2C-klienter.

## <a name="application-access"></a>Programåtkomst
**F: Var kan jag hitta en lista över program som redan är integrerade i Azure AD och deras funktioner?**

**S:** Azure AD har mer än 2 600 redan integrerade program från Microsoft, programtjänstleverantörer och partner. Alla redan integrerade program stöder enkel inloggning (SSO). Enkel inloggning kan du använda din organisations autentiseringsuppgifter tooaccess dina appar. Vissa av hello program stöder även Automatisk etablering och avetablering.

En fullständig lista över hello redan integrerade program finns i hello [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).

- - -
**F: Vad händer om hello program som jag behöver är inte i hello Azure AD-marketplace?**

**S:** Med Azure AD Premium kan du lägga till och konfigurera de program du vill. Beroende på programmets funktioner och dina preferenser kan du konfigurera enkel inloggning (SSO) och automatisk etablering.  

Mer information finns i:

* [Konfigurera enkel inloggning tooapplications som inte ingår i hello Azure Active Directory-programgalleriet](active-directory-saas-custom-apps.md)
* [Använda SCIM tooenable Automatisk etablering av användare och grupper från Azure Active Directory tooapplications](active-directory-scim-provisioning.md)

- - -
**F: hur användare loggar in tooapplications med hjälp av Azure AD?**

**S:** Azure AD innehåller flera olika sätt för användarna tooview och komma åt sina program, exempelvis:

* hello Azure AD-åtkomstpanelen
* startprogram för hello Office 365
* Direkt inloggning toofederated appar
* Djup länkar toofederated, lösenordsbaserade eller befintliga appar

Mer information finns i [distribuera Azure AD-integrerade program toousers](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

- - -
**F: Vad är hello olika sätt Azure AD aktiverar autentisering och enkel inloggning tooapplications?**

**S:** Azure AD har stöd för många standardiserade protokoll för autentisering och auktorisering, till exempel SAML 2.0, OpenID Connect, OAuth 2.0 och WS-Federation. Azure AD stöder också lösenordsvalv och automatisk inloggning för appar som endast har stöd för formulärbaserad autentisering.  

Mer information finns i:

* [Autentiseringsscenarier för Azure AD](active-directory-authentication-scenarios.md)
* [Active Directory-autentiseringsprotokoll](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [Hur fungerar enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

- - -
**F: Kan jag lägga till program som jag kör lokalt?**

**S:** Azure AD Application Proxy ger dig enkel och säker åtkomst tooon lokala webbprogram som du väljer. Du kan komma åt dessa program i hello samma hur du kommer åt din programvara som en tjänst (SaaS)-appar i Azure AD. Det finns inget behov av en VPN- eller toochange nätverksinfrastrukturen.  

Mer information finns i [hur säkra tooprovide fjärråtkomst tooon lokala program](active-directory-application-proxy-get-started.md).

- - -
**F: Hur kräver jag Multi-Factor Authentication för användare som kommer åt ett visst program?**

**S:** Med villkorlig åtkomst i Azure AD kan du tilldela en unik åtkomstprincip för varje program. Du kan kräva multifaktorautentisering alltid i en princip eller när användarna inte är anslutna toohello lokala nätverket.  

Mer information finns i [säkerhetsåtkomst tooOffice 365 och andra appar anslutna tooAzure Active Directory](active-directory-conditional-access.md).

- - -
**F: Vad är automatisk användaretablering för SaaS-appar?**

**S:** använda Azure AD tooautomate hello skapande, underhållet och borttagningen av användaridentiteter i många populära cloud SaaS-appar.

Mer information finns i [automatisera användaretablering och avetablering tooSaaS program med Azure Active Directory](active-directory-saas-app-provisioning.md).

- - -
**F: Kan jag skapa en säker LDAP-anslutning med Azure AD?**

**S:** Nej. Azure AD stöder inte hello LDAP-protokollet.
