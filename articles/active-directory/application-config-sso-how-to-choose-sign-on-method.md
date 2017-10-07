---
title: "aaaHow toodetermine vilka enkel inloggning på metoden toouse | Microsoft Docs"
description: "Förstå hello enkel inloggning lägen som stöds av Azure AD och hur toopick som en toochoose för hello programmet du är intresserad av."
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
ms.openlocfilehash: 64f0bc1dc8281d1ab8222fd50eaceaf710704886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetermine-what-single-sign-on-method-toouse"></a>Hur toodetermine vilka enkel inloggning på metoden toouse

Den här artikeln hjälper dig toounderstand hello enkel inloggning lägen som stöds av Azure AD och hur toopick som en toochoose för hello programmet du är intresserad av.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Enkel inloggning och provisioning lägen som stöds av specifika programtyper

hello tabellen nedan beskrivs hello olika enkel inloggning och provisioning lägen som stöds av varje hello ovan programtyper. Du kan använda den här tabellen toohelp toounderstand vilket program som du behöver tooadd toosupport ett specifikt mål.

  ![Asien/Stillahavsområdet register](./media/application-tables/table1.png)

## <a name="how-toochoose-a-single-sign-on-mode"></a>Hur toochoose ett läge för enkel inloggning

hello stöds **enkel inloggning** lägen för Azure AD-program visas nedan.

-   **Azure AD enkel inloggning inaktiverad** – Välj Azure AD enkel inloggning inaktiverad **läget för enkel inloggning** om du ännu inte klar toointegrate programmet med enkel inloggning med Azure AD, eller bara testar det.

-   **Länkad inloggning** – Välj hello [inloggning länkade](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **läget för enkel inloggning** om du har ett program som redan är kopplad till en befintlig enkel inloggning lösning, eller om du bara vill toopublish en enkel länka för användarna i deras [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) eller [startprogrammet för Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Lösenordsbaserade inloggning** – Välj hello [lösenordsbaserade inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **läget för enkel inloggning** om ditt program visas med ett HTML-fält för användarnamn och lösenord och du vill toostore som användarnamn och lösenord på ett säkert sätt toobe spelas toohello programmet senare

-   **SAML-baserade inloggning** – Välj hello [SAML-baserade inloggning](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) enkel inloggning på läge om programmet stöder hello SAML eller OpenID Connect protokoll eller om du vill toobe kan toomap användare toospecific programroller baserat regler som du definierar i din SAML anspråk *(**Obs:** det här alternativet är inte tillgängligt när hello programproxy konfigureras för ett program) *

-   **Rubrik-baserade inloggning** – Välj det här [huvud-baserade inloggning](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) enkel inloggning läge om du har ett program med PingAccess som stöder HTTP-huvudet baseras autentisering som du vill att tooperform enkel inloggning på för *(**Obs:** det här alternativet är endast tillgängligt när hello programproxy och PingAccess konfigureras för ett program) *

-   **Integrerad Windows-autentisering** – Välj hello [integrerad Windows-autentisering](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) enkel inloggning på läge när exponera ett lokalt WIA program som du vill att tooperform enkel inloggning på för*()*  *Obs:** det här alternativet är endast tillgängligt när hello programproxy konfigureras för ett program) *

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

6.  Välj hello-program som du vill tooconfigure enkel inloggning

7.  När programmet hello läses in klickar du på **enkel inloggning** från hello programmet vänstra navigeringsmenyn.

## <a name="next-steps"></a>Nästa steg
[Tillhandahålla enkel inloggning tooyour appar med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)

