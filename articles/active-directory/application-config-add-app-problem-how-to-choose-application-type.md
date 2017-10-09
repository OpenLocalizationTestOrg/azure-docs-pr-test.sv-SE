---
title: "aaaHow toochoose vilket program skriver toouse när du lägger till ett program | Microsoft Docs"
description: "Förstå hello stöds typer av program som du kan integrera med Azure AD och deras relaterade konfigurationsalternativ"
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
ms.openlocfilehash: 46e3672e7f5048b0fa54171f0fc169362c9d5ac6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toochoose-which-application-type-toouse-when-adding-an-application"></a>Hur toochoose vilket program skriver toouse när du lägger till ett program

Den här artikeln hjälpa toounderstand hello fyra typer av program som du kan integrera med Azure AD:

* Vad som stöds av var och en av dem.
* Varför du kan välja vilket program
* Hur tooconfigure huvudegenskaper för dessa program, t.ex. hur användare kan **etableras**, eller vad **enkel inloggning** teknik toouse.

## <a name="supported-application-types-in-azure-ad"></a>Typer av program som stöds i Azure AD

Azure AD stöder fyra huvudsakliga programtyper som du kan lägga till med hjälp av hello **Lägg till** funktion hittar du under **företagsprogram**. Exempel på dessa är:

-   **Azure AD-galleriet program** – ett program som har varit förintegrerade för enkel inloggning med Azure AD.

-   **Application Proxy program** – ett program som körs i din lokala miljö som du vill tooprovide säker enkel inloggning på tooexternally.

-   **Anpassad utvecklat program** – ett program som din organisation vill toodevelop på hello Azure AD plattform, men som kanske inte finns ännu.

-   **Icke-galleriet program** – ta med egna program! En länk som du vill eller program som visas med ett användarnamn och lösenord fält har stöd för SAML- eller OpenID Connect protokoll eller stöder SCIM som du önskar toointegrate för enkel inloggning med Azure AD.

## <a name="features-and-capabilities-supported-by-all-hello-above-application-types"></a>Funktioner som stöds av alla hello ovan programtyper

hello följande funktioner som stöds av någon av hello ovan 4 programtyper i Azure AD:

-   **Snabbstart** – komma igång med ett program snabbare genom att följa [steg för enkel distribution](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)

-   **Allmänna egenskaper för management** – hämta en [direkt djuplänken](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) tooan programmet [anpassa profilering hello](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) för ett program eller [inaktivera programmet hello](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) för alla användare.

-   **Hantering av användare och grupp** – [tilldela](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) eller [ta bort](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) användare och grupper tooan program och du kan också tilldela hello specifika programroller har åtkomst till dessa användare och grupper

-   **Självbetjäning programåtkomst** – aktivera din användare toorequest [självbetjäning programåtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooan programmet från sina programåtkomst paneler antingen genom att lägga till ett program direkt eller [ ansluter till en aktiverad grupp med självbetjäning](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), kan du alternativt som kräver godkännande för business längs hello sätt

-   **Logga in loggar** – Se [alla hello inloggningar tooan programmet](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), eller alla program

-   **Granskningsloggar** – Se [detaljerad granskningsloggar om ändringar tooan programmet](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), eller tooall dina program

-   **Villkorlig och risker-baserad åtkomst** – Ställ kraftfulla [villkor-baserade åtkomstregler](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) som tillämpas när en användare försöker toosign i tooa specifika program

-   **Visa behörigheter** – visa alla hello [OAuth2 behörigheter](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) ett program som har åtkomst tooin din katalog från en enda plats

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Enkel inloggning och provisioning lägen som stöds av specifika programtyper

hello tabellen nedan beskrivs hello olika enkel inloggning och provisioning lägen som stöds av varje hello ovan programtyper. Du kan använda den här tabellen toohelp toounderstand vilket program som du behöver tooadd toosupport ett specifikt mål.

  ![Tabell för App-typer](./media/application-tables/table1.png)

## <a name="how-toochoose-a-single-sign-on-mode"></a>Hur toochoose ett läge för enkel inloggning

hello stöds **enkel inloggning** lägen för Azure AD-program visas nedan.

-   **Azure AD enkel inloggning inaktiverad** – Välj Azure AD enkel inloggning inaktiverad **läget för enkel inloggning** om du ännu inte klar toointegrate programmet med enkel inloggning med Azure AD, eller bara testar det.

-   **Länkad inloggning** – Välj hello [inloggning länkade](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **läget för enkel inloggning** om du har ett program som redan är kopplad till en befintlig enkel inloggning lösning, eller om du bara vill toopublish en enkel länka för användarna i deras [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) eller [startprogrammet för Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Lösenordsbaserade inloggning** – Välj hello [lösenordsbaserade inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **läget för enkel inloggning** om ditt program visas med ett HTML-fält för användarnamn och lösenord och du vill toostore som användarnamn och lösenord på ett säkert sätt toobe spelas toohello programmet senare

-   **SAML-baserade inloggning** – Välj hello [SAML-baserade inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) enkel inloggning på läge om programmet stöder hello SAML eller OpenID Connect protokoll eller om du vill toobe kan toomap användare toospecific programroller baserat regler som du definierar i din SAML anspråk *

   >[!NOTE]
   >Det här alternativet är inte tillgängligt när hello programproxy konfigureras för ett program.
   >
   >

-   **Rubrik-baserade inloggning** – Välj det här [huvud-baserade inloggning](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) enkel inloggning läge om du har ett program med PingAccess som stöder HTTP-huvudet baseras autentisering som du vill att tooperform enkel inloggning på för

   >[!NOTE]
   >Det här alternativet är bara tillgänglig när hello programproxy och PingAccess konfigureras för ett program.
   >
   >

-   **Integrerad Windows-autentisering** – Välj hello [integrerad Windows-autentisering](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) enkel inloggning på läge när exponera ett lokalt WIA program som du vill att tooperform enkel inloggning på för

   >[!NOTE]
   >Det här alternativet är bara tillgänglig när hello programproxy konfigureras för ett program.
   >
   >

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Enkel inloggning lägen för anpassad utvecklade program

Program som du har anpassat utvecklas genom hello [anpassad utvecklat program](#_Custom-Developed_Applications) upplevelse stöder också ytterligare enkel inloggning lägen inte listas här ovan. Exempel på dessa är:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) baserat inloggning

-   [OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) baserat inloggning

-   [WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) baserat inloggning

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) baserat inloggning

Läs hello [Utvecklarhandbok för Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) toolearn mer om hur toocreate en anpassad utvecklade program som stöder dessa enkel inloggning lägen.

## <a name="how-tooset-an-applications-single-sign-on-mode"></a>Hur tooset ett program har enkel inloggning läge

tooset ett program **enkel inloggning** läge, följ hello instruktionerna nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooconfigure enkel inloggning.

7.  När programmet hello läses in klickar du på **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

## <a name="how-toochoose-a-provisioning-mode"></a>Hur toochoose ett Etableringsläge

-   **Manuell etablering** – Välj hello [manuell](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) Etableringsläge om du har befintliga konton eller vill toomanage konton för det här programmet utanför Azure AD.

-   **Automatisk etablering** – Välj hello [automatisk](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **Etableringsläge** om du vill tooenable automatisk API-baserad etablering och/eller ta bort etableringen av användarkonton toothis programmet 

   >[!NOTE]
   >Det här alternativet är bara tillgängligt för applikationer inom hello **aktuella** kategori av hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).
   >
   >

-   **SCIM-baserade Automatisk etablering** – använda [SCIM-baserade Automatisk etablering](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) om programmet stöder hello SCIM protokoll för att upptäcka ändringar toousers och grupper som släpps automatiskt efter ändringar tooany program som är integrerade med Azure AD 

   >[!NOTE]
   >Det här alternativet anges inte som en specifik Etableringsläge, men är aktiverad som standard för alla program som är integrerade med Azure AD.
   >
   >

## <a name="how-tooset-an-applications-provisioning-mode"></a>Hur tooset ett program Etableringsläge

tooset ett program **etablering** läge, följ hello instruktionerna nedan:

tooset ett program **enkel inloggning** läge, följ hello instruktionerna nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

  * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill tooconfigure etablering.

7.  När programmet hello läses in klickar du på **etablering** från hello programmet vänstra navigeringsmenyn.

## <a name="next-steps"></a>Nästa steg
[Hantera program med Azure Active Directory](active-directory-enable-sso-scenario.md)
