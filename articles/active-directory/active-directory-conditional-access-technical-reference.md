---
title: "Teknisk referens för Azure Active Directory villkorlig åtkomst | Microsoft Docs"
description: "Med villkorlig åtkomstkontroll kontrollerar de särskilda villkor som du väljer när du autentiserar användaren och innan du tillåter åtkomst till programmet i Azure Active Directory. När dessa villkor är uppfyllda, autentiserade användaren och få tillgång till programmet."
services: active-directory.
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: ca16a5399f94fd1ab267e0798cade3fd83f75b13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a>Teknisk referens för Azure Active Directory villkorlig åtkomst

## <a name="services-enabled-with-conditional-access"></a>Services aktiverat med villkorlig åtkomst

Regler för villkorlig åtkomst stöds mellan olika typer av Azure AD-program. Listan innehåller:


* Program som är registrerade med Azure Application Proxy
* Azure RemoteApp
* Utvecklade branschspecifika företag och program med flera klienter registrerad med Azure AD
* Dynamics CRM
* Federerade program från Azure AD application gallery
* Microsoft Office 365 Yammer
* Microsoft Office 365 Exchange Online
* Microsoft Office 365 SharePoint Online (inklusive OneDrive för företag)
* Microsoft Power BI 
* Lösenord SSO-program från Azure AD application gallery
* Visual Studio Team Services
* Microsoft Teams









## <a name="enable-access-rules"></a>Aktivera åtkomstregler
Varje regel kan aktiveras eller inaktiveras på en per program baser. När reglerna är **ON** ska vara aktiverad och framtvingas för användare med åtkomst till programmet. När de är **OFF** de används inte och kommer inte att påverka användare inloggningen.

## <a name="applying-rules-to-specific-users"></a>Tillämpa regler för specifika användare
Regler kan tillämpas på vissa uppsättningar användare baserat på säkerhetsgruppen genom att ange **tillämpa på**. **Tillämpa på** kan anges till **alla användare** eller **grupper**. Om värdet är **alla användare** regler gäller för alla användare med åtkomst till programmet. Den **grupper** alternativet tillåter specifika säkerhets- och distributionsgrupper väljas, regler tillämpas endast för dessa grupper.

När du distribuerar en regel, är det vanligt att först använda den en begränsad uppsättning användare som är medlemmar i en pilot-grupper. När du är klar regeln kan tillämpas på **alla användare**. Detta innebär att regeln ska gälla för alla användare i organisationen.

Välj grupper kan också undantas från principen med den **utom** alternativet. Alla medlemmar i dessa grupper kommer undantas även om de visas i en inkluderad grupp.

## <a name="at-work-networks"></a>”På arbetet” nätverk
Regler för villkorlig åtkomst som använder en ”på arbetet”-nätverket som förlitar sig på tillförlitliga IP-adressintervall som har konfigurerats i Azure AD eller använda anspråkets ”i corpnet” från AD FS. Bestämmelserna omfattar:

* Kräv Multi-Factor authentication när de inte är på arbetet
* Blockera åtkomst när de inte är på arbetet

Alternativ för att ange ”på arbetet” nätverk

1. Konfigurera betrodda IP-adressintervall i den [multifaktorautentisering konfigurationssidan](../multi-factor-authentication/multi-factor-authentication-whats-next.md). Princip för villkorlig åtkomst använder de konfigurerade intervallen för varje autentiseringsbegäran och utfärdande för att utvärdera regler. 
2. Konfigurera användning av inre corpnet begära det här alternativet kan användas med externa kataloger, med hjälp av AD FS. Mer information om inre corpnet anspråk, se [Tusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).


## <a name="rules-based-on-application-sensitivity"></a>Regler baserat på program känslighet
Regler är konfigurerade per program så att tjänsterna för högt värde som ska skyddas utan att påverka åtkomst till andra tjänster. Regler för villkorlig åtkomst kan konfigureras på den **konfigurera** för programmet. 

Regler som för närvarande finns:

* **Kräv Multi-Factor authentication**
  
  * Alla användare som principen tillämpas på måste utföras för att autentisera via multifaktorautentisering minst en gång.
* **Kräv Multi-Factor authentication när de inte är på arbetet**
  
  * Om principen gäller kommer alla användare att behöva har utfört multifaktorautentisering minst en gång om de har åtkomst till tjänsten från en annan plats. Om de flyttar från ett verk till fjärrplatsen behöver de utföra multifaktorautentisering vid åtkomst till tjänsten.
* **Blockera åtkomst när de inte är på arbetet** 
  
  * När användarna flyttar från arbetet till en fjärrplats, blockeras de om ”blockera åtkomst när de inte är på arbetet”-principen gäller för dem.  De tillåts nytt åtkomst när de är på en plats för arbetet.

## <a name="related-topics"></a>Relaterade ämnen
* [Skydda åtkomst till Office 365 och andra appar ansluten till Azure Active Directory](active-directory-conditional-access.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)

