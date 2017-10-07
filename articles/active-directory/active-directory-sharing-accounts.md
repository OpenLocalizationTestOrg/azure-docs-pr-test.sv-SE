---
title: "aaaSharing konton med hjälp av Azure AD | Microsoft Docs"
description: "Beskriver hur Azure Active Directory kan organisationer toosecurely dela konton för konsumenten molntjänster och lokala appar."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e2d77104-d978-46a3-bfea-03ffdf3b61e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 9f98bfa97a6c9ba1566d3f921c1b676d5f3c2a88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sharing-accounts-with-azure-ad"></a>Dela konton med Azure AD
## <a name="overview"></a>Översikt
Organisationer behöver ibland toouse ett enda användarnamn och lösenord för flera personer. Det händer vanligtvis i två fall:

* Vid åtkomst till program som kräver ett unikt inloggningsnamn och lösenord för varje användare om lokala appar eller konsumenten molntjänster (t.ex. företagets sociala medier konton).
* När du skapar miljöer med flera användare. Du kan ha ett lokalt konto med förhöjd behörighet och kan vara används toodo core-installation, administration och återställning aktiviteter (t.ex. hello lokala ”” konto som global administratör för Office 365 eller hello rot-konto i Salesforce).

Dessa konton skulle traditionellt delas genom att distribuera hello autentiseringsuppgifter (användarnamn/lösenord) toohello rätt personer eller spara dem i en delad plats där flera betrodda agenter kan komma åt dem.

hello traditionella delning modellen har flera nackdelar:

* Aktivera åtkomst toonew program måste du toodistribute autentiseringsuppgifter tooeveryone som behöver åtkomst.
* Varje delad program kan kräva en egen unik uppsättning delade autentiseringsuppgifter, att kräva att användare tooremember flera uppsättningar med autentiseringsuppgifter. När användarna har tooremember många autentiseringsuppgifter, ökar hello risk att de ska använda toorisky metoder. (t.ex. skriver ned lösenord).
* Du kan inte se vem som har åtkomst till tooan program.
* Du kan inte se vem som har *nås* ett program.
* När du behöver tooremove åtkomst tooan program du har tooupdate hello autentiseringsuppgifter och distribuerar dem tooeveryone som behöver åtkomst till toothat program.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory-konto och delning
Azure AD innehåller en ny metod toousing delade konton som löser dessa nackdelar.

hello Azure AD-administratör konfigurerar vilka program som en användare får åtkomst med hjälp av hello åtkomstpanelen och välja hello typ av enkel inloggning passar bäst för programmet. En av dessa typer *lösenordsbaserade enkel inloggning på*, kan Azure AD som fungerar som ett slags ”broker” under hello inloggning för appen.

Användare logga in en gång med organisationskontot. Detta är hello samma konto som de regelbundet använder tooaccess skrivbordet eller e-post. De kan identifiera och komma åt de program som de har tilldelats. Den här listan över program kan innehålla valfritt antal delade autentiseringsuppgifter med delade konton. hello slutanvändare inte behöver tooremember eller Skriv ned hello olika konton som de kan använda.

Delade konton inte bara öka tillsyn och förbättra användbarhet, de också förbättra säkerheten. Användare med behörighet toouse hello autentiseringsuppgifter ser inte hello delat lösenord, men i stället får behörighet toouse hello lösenord som en del av en framförhållning autentiseringsflödet. Dessutom med vissa lösenord SSO-program, har du hello alternativet toohave Azure AD med jämna mellanrum förnyelse (uppdatering) hello lösenord med hjälp av stora, komplexa lösenord, öka hello kontosäkerhet. Hej administratör kan enkelt bevilja eller återkalla åtkomst tooan program och också veta vem som har toohello åtkomstkonto och vem som har åtkomst till den i hello tidigare.

Azure AD stöder delade konton för alla Enterprise Mobility Suite (EMS), Premium eller Basic licensierade användare över alla typer av lösenord som är enkel inloggning program. Du kan dela konton för någon av de tusentals förintegrerade program i hello programgalleriet och kan lägga till programmet med autentisering av lösenord med [anpassade SSO appar](active-directory-sso-integrate-saas-apps.md).

Azure AD-funktioner som aktiverar delning av konto är:

* [Lösenord för enkel inloggning](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Lösenord för enkel inloggning agent
* [Grupptilldelning](active-directory-accessmanagement-self-service-group-management.md)
* Lösenord för anpassade appar
* [Appen instrumentpanelen/användningsrapporter](active-directory-passwords-get-insights.md)
* Slutanvändaren åtkomst portaler
* [App-proxy](active-directory-application-proxy-get-started.md)
* [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Dela ett konto
toouse Azure AD tooshare ett konto måste du:

* Lägga till ett program [app galleriet](https://azure.microsoft.com/marketplace/active-directory/) eller [anpassat program](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
* Konfigurera hello program för lösenord enkel inloggning (SSO)
* Använd [grupp baserad tilldelning](active-directory-accessmanagement-group-saasapps.md) och välj hello alternativet tooenter delade autentiseringsuppgifter
* Valfritt: i vissa program, till exempel Facebook, Twitter och LinkedIn, du kan aktivera hello alternativet för [Azure AD automated förlängningen för lösenord](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

Du kan också göra din delat konto säkrare med Multi-Factor Authentication (MFA) (Läs mer om [skydda program med Azure AD](../multi-factor-authentication/multi-factor-authentication-get-started.md)) och du kan delegera hello möjlighet toomanage som har toohello program med hjälp av [Azure AD-självbetjäning](active-directory-accessmanagement-self-service-group-management.md) hantering.

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Skydda appar med villkorlig åtkomst](active-directory-conditional-access.md)
* [Självbetjäning hantering/SSAA](active-directory-accessmanagement-self-service-group-management.md)

