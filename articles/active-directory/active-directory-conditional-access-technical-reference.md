---
title: "aaaAzure Teknisk referens för Active Directory villkorlig åtkomst | Microsoft Docs"
description: "Med villkorlig åtkomstkontroll kontrollerar hello särskilda villkor som du väljer när du autentiserar användaren hello och innan åtkomst toohello program i Azure Active Directory. När dessa villkor är uppfyllda, hello användare autentiseras och tillåtet åtkomst toohello program."
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
ms.openlocfilehash: ee201405d1d17f130607a95bf455b60cd222dd0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a>Teknisk referens för Azure Active Directory villkorlig åtkomst

## <a name="services-enabled-with-conditional-access"></a>Services aktiverat med villkorlig åtkomst

Regler för villkorlig åtkomst stöds mellan olika typer av Azure AD-program. Listan innehåller:


* Program som är registrerade med hello Azure Application Proxy
* Azure RemoteApp
* Utvecklade branschspecifika företag och program med flera klienter registrerad med Azure AD
* Dynamics CRM
* Federerade program från hello Azure AD application gallery
* Microsoft Office 365 Yammer
* Microsoft Office 365 Exchange Online
* Microsoft Office 365 SharePoint Online (inklusive OneDrive för företag)
* Microsoft Power BI 
* Lösenord SSO-program från hello Azure AD application gallery
* Visual Studio Team Services
* Microsoft Teams









## <a name="enable-access-rules"></a>Aktivera åtkomstregler
Varje regel kan aktiveras eller inaktiveras på en per program baser. När reglerna är **ON** ska vara aktiverad och framtvingas för användare med åtkomst till programmet hello. När de är **OFF** de används inte och kommer inte påverkan hello användare loggar in upplevelse.

## <a name="applying-rules-toospecific-users"></a>Tillämpa regler toospecific användare
Regler kan vara tillämpade toospecific uppsättningar med användare som är baserade på säkerhetsgruppen genom att ange **tillämpa på**. **Tillämpa på** kan anges för**alla användare** eller **grupper**. När värdet för**alla användare** hello regler gäller tooany användare med åtkomst toohello program. Hej **grupper** alternativet tillåter specifika säkerhetsbehörigheter och distribution grupper toobe markerad regler tillämpas endast för dessa grupper.

När du distribuerar en regel, är det vanligt toofirst tillämpa det en begränsad uppsättning användare som är medlemmar i en pilot-grupper. När fullständig hello regel kan användas för**alla användare**. Detta innebär att hello regeln toobe tillämpas för alla användare i hello organisation.

Välj grupper kan också undantas från principen med hjälp av hello **utom** alternativet. Alla medlemmar i dessa grupper kommer undantas även om de visas i en inkluderad grupp.

## <a name="at-work-networks"></a>”På arbetet” nätverk
Regler för villkorlig åtkomst som använder en ”på arbetet”-nätverket som förlitar sig på tillförlitliga IP-adressintervall som har konfigurerats i Azure AD eller användning av hello ”i corpnet” anspråk från AD FS. Bestämmelserna omfattar:

* Kräv Multi-Factor authentication när de inte är på arbetet
* Blockera åtkomst när de inte är på arbetet

Alternativ för att ange ”på arbetet” nätverk

1. Konfigurera betrodda IP-adressintervall i hello [multifaktorautentisering konfigurationssidan](../multi-factor-authentication/multi-factor-authentication-whats-next.md). Princip för villkorlig åtkomst använder hello konfigurerade intervallen för varje begäran och token utfärdande tooevaluate reglerna för autentisering. 
2. Konfigurera användningen av hello inuti corpnet anspråk, det här alternativet kan användas med externa kataloger, med hjälp av AD FS. toolearn mer om hello inuti corpnet anspråk, se [Tusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).


## <a name="rules-based-on-application-sensitivity"></a>Regler baserat på program känslighet
Regler är konfigurerade per program så att hello värdefulla tjänster toobe skyddas utan att påverka tooother tjänster. Regler för villkorlig åtkomst kan konfigureras på hello **konfigurera** fliken av programmet hello. 

Regler som för närvarande finns:

* **Kräv Multi-Factor authentication**
  
  * Alla användare att den här principen är tillämpade toowill vara nödvändiga tooauthenticate via multifaktorautentisering minst en gång.
* **Kräv Multi-Factor authentication när de inte är på arbetet**
  
  * Om principen gäller kommer alla användare att nödvändiga toohave utförs multifaktorautentisering minst en gång om de komma åt hello-tjänsten från en annan plats fungerar. Om de flyttar från en plats för arbete tooremote vara de nödvändiga tooperform flerfunktionsautentisering när hello-tjänsten.
* **Blockera åtkomst när de inte är på arbetet** 
  
  * När användarna flyttar från arbete tooa fjärrplats, blockeras de om hello ”blockera åtkomst när de inte är på arbetet” principen är tillämpade toothem.  De tillåts nytt åtkomst när de är på en plats för arbetet.

## <a name="related-topics"></a>Relaterade ämnen
* [Skydda åtkomsten tooOffice 365 och andra appar anslutna tooAzure Active Directory](active-directory-conditional-access.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)

