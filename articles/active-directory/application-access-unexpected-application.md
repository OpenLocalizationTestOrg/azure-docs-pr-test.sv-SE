---
title: aaaUnexpected programmet i listan med mina program | Microsoft Docs
description: "Hur toosee alla program i din klient och förstå hur program visas i listan alla program under företagsprogram"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 2f974046b655bc36a05f002c56511a8a988cd01c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-application-in-my-applications-list"></a>Oväntat programmet i listan med mina program

Den här artikeln hjälper dig toounderstand hur program visas i din **alla program** listan **företagsprogram**. 

## <a name="how-toosee-all-applications-in-your-tenant"></a>Hur toosee alla program i din klient

toosee alla hello program i din klient, måste du toouse hello **Filter** styra tooshow **alla program** under hello **alla program** lista. toodo detta, följ hello stegen nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

6.  Klicka på hello Använd hello **Filter** kontroll hello överst i hello **listan med alla program**.

7.  På hello **Filter** bladet, ange hello **visa** alternativet för**alla program.**

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a>Varför visas ett visst program i programlistan alla?

När filtreras för**alla program**, hello **alla program** **listan** visas alla objekt för tjänstens huvudnamn i din klient. Tjänstens huvudnamn objekt som kan visas i listan på olika sätt:

1.  När du lägger till ett program från hello programgalleriet, inklusive:

   1. **Azure AD-galleriet program** – ett program som har varit förintegrerade för enkel inloggning med Azure AD.

   2. **Application Proxy program** – ett program som körs i din lokala miljö som du vill tooprovide säker enkel inloggning på tooexternally.

   3. **Anpassad utvecklat program** – ett program som din organisation vill toodevelop på hello Azure AD plattform, men som kanske inte finns ännu.

   4. **Icke-galleriet program** – ta med egna program! En länk som du vill eller program som visas med ett användarnamn och lösenord fält har stöd för SAML- eller OpenID Connect protokoll eller stöder SCIM som du önskar toointegrate för enkel inloggning med Azure AD.

2.  När du registrerar dig för eller loggar in på en 3<sup>rd</sup> partsprogram integrerat med Azure Active Directory. Ett exempel på detta är [Smartsheet](https://app.smartsheet.com/b/home) eller [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).

3.  När du registrerar dig för eller lägga till en licens tooa användare eller grupp tooa första partsprogram, t.ex. [Microsoft Office 365](http://products.office.com/).

4.  När du lägger till en ny programregistrering genom att skapa en anpassad utvecklade program med hjälp av hello [programmet registret](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).

5.  När du lägger till en ny programregistrering genom att skapa en anpassad utvecklade program med hjälp av hello [V2.0 Programregistreringsportalen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).

6.  När du lägger till ett program du utvecklar med Visual Studio [ASP.net autentiseringsmetoder](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) eller [Connected Services](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).

7.  När du skapar ett service principal objekt med hello [Azure AD PowerShell-modulen](/powershell/azure/install-adv2?view=azureadps-2.0).

8.  När du [medgivande tooan programmet](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) som en administratör toouse data i din klient.

9.  När en [användaren godkänner tooan programmet](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toouse data i din klient.

10. När du aktiverar vissa tjänster som lagrar data i din klient. Ett exempel på detta är återställning av lösenord, som modellerats som en service principal toostore din princip för lösenordsåterställning på ett säkert sätt.

tooget mer information om hur appar läggs tooyour directory läsa [hur och varför program läggs tooAzure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).

## <a name="i-want-tooremove-a-specific-users-or-groups-assignment-tooan-application"></a>Jag vill tooremove en specifik användare eller grupp tilldelning tooan program

tooremove en användare eller grupp tilldelning tooan program, följ hello steg i hello [ta bort en användare eller grupp från en enterprise-app i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) artikel.

## <a name="i-want-toodisable-all-access-tooan-application-for-every-user"></a>Jag vill toodisable alla åtkomst tooan program för varje användare

toodisable alla inloggningar tooan användarprogram, följ hello stegen i hello [inaktivera användarinloggningar för en enterprise-app i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) artikel.

## <a name="i-want-toodelete-an-application-entirely"></a>Jag vill toodelete ett program helt

för**ta bort ett program**, följ hello instruktionerna nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill toodelete.

7.  När programmet hello läses in klickar du på **ta bort** ikon från hello översta program **översikt** bladet.

## <a name="i-want-toodisable-all-future-user-consent-operations-tooany-application"></a>Jag vill toodisable alla framtida medgivande operations tooany-användarprogram

Inaktiverar användargodkännande för hela katalogen förhindra användare från principer tooany program. Administratörer kan fortfarande medgivande på användarens behalves. toolearn mer om programmet samtycke och varför du kanske eller kanske inte vill toodo detta, Läs [förstå användare och admin medgivande](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

för**inaktivera alla framtida användarens medgivande åtgärder i hela katalogen**, följ hello instruktionerna nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **användarinställningar**.

6.  Inaktivera alla framtida användarens medgivande åtgärder genom att ange hello **användare kan låta appar tooaccess sina data** växla för**nr** och klicka på hello **spara** knappen.

## <a name="next-steps"></a>Nästa steg
[Hantera program med Azure Active Directory](active-directory-enable-sso-scenario.md)
