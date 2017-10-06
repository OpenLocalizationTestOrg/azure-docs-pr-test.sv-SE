---
title: "aaaGet igång integrera Azure AD med appar | Microsoft Docs"
description: "Den här artikeln är en Kom igång-guide för att integrera Azure Active Directory (AD) med lokala program och molnprogram."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: db6d210d-c970-49e9-bd20-36d984bcd1c3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: 5a7a851e8418083fee72ab58477a9cab75d0d4bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Integrera Azure Active Directory med program komma igång
## <a name="overview"></a>Översikt
Det här avsnittet är avsedda toogive du en översikt över integrera program med Azure Active Directory (AD). Hello avsnitten nedan innehåller en kort sammanfattning av en mer detaljerad avsnittet så att du kan identifiera vilka delar av den här Kom igång-guide är relevanta tooyou.  Följ hello länkar för en mer grundlig genomgång på varje område.

## <a name="before-you-begin-take-inventory"></a>Innan du kan inventera
Innan du går i toointegrating program med Azure AD är viktiga tooknow där du och där du vill toogo.  hello är följande frågor avsedda toohelp du tycker om Azure AD application integration-projekt.

### <a name="application-inventory"></a>Applikationsinventering
* Var finns alla dina program? Vem äger dem?
* Vilken typ av autentisering kräver dina program?
* Som behöver åtkomst toowhich program?
* Vill du toodeploy ett nytt program?
  * Du skapar den interna och distribuera den på en instans av Azure-beräkning?
  * Ska du använda ett som är tillgängliga i hello Azure Application Gallery?

### <a name="user-and-group-inventory"></a>Användar- och programvaruinventering
* Där finns dina användarkonton?
  * Lokalt Active Directory
  * Azure AD
  * I en separat program-databas som du äger
  * I ej sanktionerade program
  * Alla hello ovan
* Vilka behörigheter och rolltilldelningar enskilda användare för närvarande har? Behöver du tooreview deras åtkomst eller är du säker på att dina användare åtkomst och rollen tilldelningar är lämpliga nu?
* Grupper redan upprättas i din lokala Active Directory?
  * Hur organiseras grupper?
  * Vilka är hello gruppmedlemmar?
  * Vilka behörigheter/rolltilldelningar hello grupper för närvarande har?
* Behöver du tooclean av användare/grupp databaser innan du integrera?  (Detta är ett ganska viktig fråga. Skräpinsamling i skräp ut).

### <a name="access-management-inventory"></a>Access management inventering
* Hur hanterar du användaren åtkomst tooapplications för närvarande? Behöver som toochange?  Har du funderat andra sätt toomanage åtkomst som med [RBAC](role-based-access-control-configure.md) till exempel?
* Som behöver åtkomst toowhat?

Du kanske har inte hello svar tooall frågor direkt, men det är OK.  Den här guiden hjälper dig besvara vissa av dessa frågor och göra vissa välgrundade beslut.

## <a name="prerequisites"></a>Krav
* En Azure-prenumeration och en Azure Active Directory-katalog.  Om du inte redan har en Azure-prenumeration, kan du prova Azure kostnadsfritt i 30 dagar. [Prova!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Programmet integrering med Azure AD
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Hitta ej sanktionerad molnprogram med Cloud App Discovery
Som nämnts ovan är kan det finnas program som inte har hanterats av din organisation fram till nu.  Som en del av hello inventering är det möjligt toofind ej sanktionerad molnprogram. Se [hitta ej sanktionerade molnappar med Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Typer av autentisering
Var och en av dina program kan ha olika krav. Med Azure AD, kan signera certifikat användas med program som använder SAML 2.0, WS-Federation, eller OpenID Connect protokoll samt lösenordet enkel inloggning. Mer information om programmet autentiseringstyper för användning med Azure AD finns [hantera certifikat för federerad enkel inloggning i Azure Active Directory](active-directory-sso-certs.md) och [lösenord utifrån för enkel inloggning](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Aktivera enkel inloggning med Azure AD App-Proxy
Du kan ange åtkomst tooapplications finns i ditt privata nätverk på ett säkert sätt, från var som helst och på alla enheter med Microsoft Azure AD Application Proxy. När du har installerat en application proxy connector i din miljö, kan den enkelt konfigureras med Azure AD.

### <a name="integrating-applications-with-azure-ad"></a>Integrera program med Azure AD
hello behandlas följande artiklar hello olika sätt program integrera med Azure AD och innehåller vägledning.

* [När du fastställer vilka Active Directory-toouse](active-directory-administer.md)
* [Använda program i hello Azure-programgalleriet](active-directory-appssoaccess-whatis.md)
* [Integrera SaaS-program självstudier lista](active-directory-saas-tutorial-list.md)

## <a name="managing-access-tooapplications"></a>Hantera åtkomst tooapplications
hello beskriver följande artiklar hur du kan hantera åtkomst tooapplications när de har integrerats med Azure AD med hjälp av Azure AD-anslutningar och Azure AD.

* [Hantera åtkomst tooapps med hjälp av Azure AD](active-directory-managing-access-to-apps.md)
* [Automatisera med Azure AD-kopplingar](active-directory-saas-app-provisioning.md)
* [Tilldela användare tooan program](active-directory-applications-guiding-developers-assigning-users.md)
* [Tilldela grupper tooan program](active-directory-applications-guiding-developers-assigning-groups.md)
* [Dela konton](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Integrera anpassade program
Om du skriver ett nytt program och vill tooassist utvecklare i utnyttja hello power Azure AD, se [Guiding utvecklare](active-directory-applications-guiding-developers-for-lob-applications.md).

Om du vill tooadd ditt eget program toohello Azure Application Gallery finns [”anpassa din egen app” Azure AD Self-Service SAML-konfiguration](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Se även
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)

