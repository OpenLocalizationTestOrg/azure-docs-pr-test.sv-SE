---
title: "Dela konton med hjälp av Azure AD | Microsoft Docs"
description: "Beskriver hur Azure Active Directory kan organisationer på ett säkert sätt dela konton för konsumenten molntjänster och lokala appar."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: e2d77104-d978-46a3-bfea-03ffdf3b61e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 3b6a83d91ec5d8466669655d6c3bd7ae7b42dd2f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="sharing-accounts-with-azure-ad"></a>Dela konton med Azure AD
## <a name="overview"></a>Översikt
Ibland måste organisationer du använda ett enda användarnamn och lösenord för flera personer som normalt görs i två fall:

* Vid åtkomst till program som kräver en unik inloggningsnamn och lösenord för varje användare om lokala appar eller konsumenten molntjänster (till exempel företagets sociala medier konton).
* När du skapar miljöer med flera användare. Du kanske har ett enda, lokalt konto som har upphöjda privilegier och används för att core-installation, administration och återställning aktiviteter. Till exempel kontot Lokal ”global administratör” för Office 365 eller rot-konto i Salesforce.

Traditionellt är dessa konton gemensamma för distribution av autentiseringsuppgifter (användarnamn och lösenord) till rätt personer eller spara dem i en delad plats som flera betrodda agenter kan komma åt.

Den traditionella delningsapplikationen modellen har flera nackdelar:

* Aktivera åtkomst till nya program måste du distribuera autentiseringsuppgifterna till alla som behöver åtkomst.
* Varje delad program kan kräva en egen unik uppsättning delade autentiseringsuppgifter, vilket kräver att användarna ska komma ihåg flera uppsättningar med autentiseringsuppgifter. När användare behöver komma ihåg många autentiseringsuppgifter, ökar risken använda riskfyllda metoder. (till exempel skriver ned lösenord).
* Du kan inte se vem som har åtkomst till ett program.
* Du kan inte se vem som har *nås* ett program.
* När du vill ta bort åtkomst till ett program måste du uppdatera autentiseringsuppgifterna och distribuera dem till alla som behöver åtkomst till programmet.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory-konto och delning
Azure AD innehåller en ny metod för att använda delade konton som löser dessa nackdelar.

Azure AD-administratör konfigurerar vilka program som en användare får åtkomst med hjälp av åtkomstpanelen och välja vilken typ av enkel inloggning bäst lämpade för programmet. En av dessa typer *lösenordsbaserade enkel inloggning på*, kan Azure AD som fungerar som ett slags ”broker” under inloggning för appen.

Användare logga in en gång med organisationskontot. Det här kontot är samma de regelbundet använder för att få åtkomst till skrivbordet eller e-post. De kan identifiera och komma åt de program som de har tilldelats. Den här listan över program kan innehålla valfritt antal delade autentiseringsuppgifter med delade konton. Slutanvändaren behöver inte spara eller skriva ned olika konton som de använder.

Delade konton inte bara öka tillsyn och förbättra användbarhet, de också förbättra säkerheten. Användare med behörighet att använda autentiseringsuppgifterna som kan se inte de delade lösenordet, men i stället får behörighet att använda lösenord som en del av en framförhållning autentiseringsflödet. Dessutom ger vissa lösenord SSO program dig möjlighet att använda Azure AD för att regelbundet förnyelse (uppdatering) lösenord. Systemet använder stora, komplexa lösenord, vilket ökar kontosäkerheten. Administratören kan enkelt bevilja eller återkalla åtkomst till ett program vet vem som har åtkomst till kontot och vem som har åtkomst till den tidigare.

Azure AD stöder delade konton för Enterprise Mobility Suite (EMS), Premium eller Basic licensierade användare, för alla typer av lösenord för enkel inloggning program. Du kan dela konton för någon av de tusentals förintegrerade program i programgalleriet och kan lägga till programmet med autentisering av lösenord med [anpassade SSO appar](active-directory-enterprise-apps-manage-sso.md).

Azure AD-funktioner som aktiverar delning av konto är:

* [Lösenord för enkel inloggning](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Lösenord för enkel inloggning agent
* [Grupptilldelning](active-directory-accessmanagement-self-service-group-management.md)
* Lösenord för anpassade appar
* [Appen instrumentpanelen/användningsrapporter](active-directory-passwords-get-insights.md)
* Slutanvändarportaler åtkomst
* [App-proxy](active-directory-application-proxy-get-started.md)
* [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Dela ett konto
Om du vill använda Azure AD för att dela ett konto, måste du:

* Lägga till ett program [app galleriet](https://azure.microsoft.com/marketplace/active-directory/) eller [anpassat program](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
* Konfigurera program för lösenord enkel inloggning (SSO)
* Använd [gruppbaserad tilldelning](active-directory-accessmanagement-group-saasapps.md) och välj alternativet för att ange en delad autentiseringsuppgift
* Valfritt: i vissa program, till exempel Facebook, Twitter och LinkedIn, du kan aktivera alternativet för [Azure AD automated förlängningen för lösenord](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

Du kan också göra din delat konto säkrare med Multi-Factor Authentication (MFA) (Läs mer om [skydda program med Azure AD](../multi-factor-authentication/multi-factor-authentication-get-started.md)) och du kan delegera möjligheten att hantera vem har tillgång till det program som använder [ Azure AD-självbetjäning](active-directory-accessmanagement-self-service-group-management.md) hantering.

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Skydda appar med villkorlig åtkomst](active-directory-conditional-access-azure-portal.md)
* [Självbetjäning hantering/SSAA](active-directory-accessmanagement-self-service-group-management.md)

