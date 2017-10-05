---
title: Azure Active Directory-versioner | Microsoft Docs
description: "Den här artikeln beskriver alternativ gratis och betald utgåvor av Azure Active Directory. Azure Active Directory Basic, Azure Active Directory Premium P1 och Azure Active Directory Premium P2 är betald utgåvor."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: dcaf8939-7633-40a8-bd76-27dedbb6083a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 9d10ebf9d7bd07bd126302a6ecf442d809e00196
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-editions"></a>Azure Active Directory-versioner
Alla Microsoft Online-tjänster är beroende av Azure Active Directory (Azure AD) för inloggning och andra identitet. Om du prenumererar på någon av Microsoft Online-företagstjänster (till exempel Office 365 eller Microsoft Azure) kan hämta du Azure AD med åtkomst till alla ledigt funktioner som beskrivs nedan.  

Azure Active Directory är en omfattande molnlösning för identitets- och åtkomsthantering med hög tillgänglighet som kombinerar kärnkatalogstjänster, avancerad identitetsstyrning och programåtkomsthantering. Azure Active Directory erbjuder även en bred, standardbaserad plattform som ger utvecklare möjlighet att förse sina program med åtkomstkontroll, baserat på en centraliserad princip och regler. Med Azure Active Directory Free edition kan hantera användare och grupper, synkroniseras med lokala kataloger kan hämta enkel inloggning mellan Azure och Office 365 tusentals populära SaaS-program som Salesforce, Workday, Concur, DocuSign, Google Apps, rutan, ServiceNow, Dropbox och mycket mer. Mer information om Azure Active Directory [vad är Azure AD?](active-directory-whatis.md)

Du kan lägga till betalda funktioner med hjälp av Azure Active Directory Basic, Premium P1 och Premium P2-versioner för att förbättra din Azure Active Directory. Azure Active Directory betald versioner är byggda på en befintlig gratis directory tillhandahåller klassen funktioner utsträckning självbetjäning, förbättrad övervakning, rapportering om säkerhet, Multi-Factor Authentication (MFA) och säker åtkomst för den mobila arbetsstyrkan för företag.

Office 365-prenumerationer ingår ytterligare Azure Active Directory-funktioner som beskrivs i jämförelsetabellen.

> [!NOTE]
> Alternativen prisnivå av dessa versioner finns [Azure Active Directory-priser](https://azure.microsoft.com/pricing/details/active-directory/). Azure Active Directory Premium P1 Premium P2 och Azure Active Directory Basic stöds inte för närvarande i Kina. Kontakta oss på i Azure Active Directory-forumet för mer information.
>
>

* **Azure Active Directory Basic** -utformad för projektanställda med molnet första behov, den här versionen innehåller molnet program för central åtkomst och självbetjäningsportalen identitetshanteringslösningar. Med Basic-versionen av Azure Active Directory får du funktioner som ökar produktiviteten och minskar kostnaderna, som gruppbaserad åtkomsthantering, lösenordsåterställning med självbetjäning för molnprogram och Azure Active Directory Application Proxy (för att publicera lokala webbprogram med Azure Active Directory), som alla stöds av ett serviceavtal på företagsnivå med 99,9 procent drifttid.
* **Azure Active Directory Premium P1** – utformade för att gör det lättare för organisationer med flera krävande identitets- och hanteringsbehov, Azure Active Directory Premium edition lägger till funktioner för hantering av funktioner på företagsnivå identitet och möjliggör hybrid användare sömlöst kan komma åt lokalt och molntjänster funktioner. Den här versionen innehåller allt du behöver för informationsarbetare och identitetsadministratörer i hybridmiljöer över programåtkomst, identitets- och åtkomsthantering (IAM) med självbetjäning, identitetsskydd och säkerhet i molnet. Det stöder avancerade administration och delegering resurser som dynamiska grupper och grupphantering via självbetjäning. Den innehåller Microsoft Identity Manager (ett lokalt identitets- och management suite) samt molnet återskrivning funktioner för att aktivera lösningar som Självbetjäning för återställning av lösenord för lokala användare.
* **Azure Active Directory Premium P2** -utformad med avancerat skydd för alla användare och administratörer, erbjudandet nya innehåller alla funktioner i Azure AD Premium P1 samt vår nya identitetsskydd och Privileged Identity Management. Azure Active Directory-identitetsskydd utnyttjar miljarder signalerar att tillhandahålla risk-baserad villkorlig åtkomst till dina program och kritiska företagsdata. Vi kan också hjälpa dig att hantera och skydda Privilegierade konton med Azure Active Directory Privileged Identity Management så att du kan identifiera begränsa och övervaka administratörer och deras åtkomst till resurser och ger just-in-time-åtkomst vid behov.  

Om du vill registrera dig och börja använda Active Directory Premium idag, se [komma igång med Azure Active Directory Premium](active-directory-get-started-premium.md).

> [!NOTE]
> Ett antal Azure Active Directory-funktioner är tillgängliga via ”betala per användning” versioner:
>
> * Active Directory B2C är identitets- och hanteringslösning för dina konsumentinriktade program. Mer information finns i [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)
> * Azure Multi-Factor Authentication kan användas via per användare eller per autentiseringsproviders. Mer information finns i [vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
>
>

## <a name="comparing-generally-available-features"></a>Jämföra allmänt tillgängliga funktioner
> [!NOTE]
> En annan vy av dessa data finns i [Azure Active Directory-funktioner](https://www.microsoft.com/en/server-cloud/products/azure-active-directory/features.aspx).
>
>

**Vanliga funktioner**

* [Katalogobjekt](#directory-objects)
* [Hantering av användare/grupp (Lägg till/Uppdatera/ta bort) / användarbaserade etablering, registrering av enheten](#usergroup-management-addupdatedelete-user-based-provisioning-device-registration)
* [Enkel inloggning (SSO)](#single-sign-on-sso)
* [Självbetjäning ändring av lösenord för användarna](#self-service-password-change-for-cloud-users)
* [Ansluta (Synkroniseringsmotorn som utökar lokala kataloger till Azure Active Directory)](#connect-sync-engine-that-extends-on-premises-directories-to-azure-active-directory)
* [Säkerhet / rapporter](#securityusage-reports)

**Grundläggande funktioner**

* [Gruppbaserad åtkomsthantering / etablering](#group-based-access-managementprovisioning)
* [Självbetjäning för återställning av lösenord för användarna](#self-service-password-reset-for-cloud-users)
* [Företagets Branding (inloggning sidor/åtkomstpanelen anpassning)](#company-branding-logon-pagesaccess-panel-customization)
* [Programproxy](#application-proxy)
* [SLA 99,9%](#sla-999)

**Premium P1-funktioner**

* [Self-Service Group och app Management/egen-Service programmet tillägg/dynamiska grupper](#self-service-group)
* [Självbetjäning lösenord återställning/ändra/Lås upp med lokalt återskrivning](#self-service-password-resetchangeunlock-with-on-premises-write-back)
* [Multifaktorautentisering (molnet och lokalt (MFA-Server))](#multi-factor-authentication-cloud-and-on-premises-mfa-server)
* [MIM-CAL + MIM-servern](#mim-cal-mim-server)
* [Cloud App Discovery](#cloud-app-discovery)
* [Connect Health](#connect-health)
* [Lösenord för automatisk förnyelse för gruppkonton](#automatic-password-rollover-for-group-accounts)

**Premium P2-funktioner**

* [Identity Protection](active-directory-identityprotection.md)
* [Privileged Identity Management](active-directory-privileged-identity-management-configure.md)

**Azure Active Directory Join – endast relaterade funktioner för Windows 10**

* [Ansluta en enhet till Azure AD, skrivbordet SSO Microsoft Passport för Azure AD, administratör Bitlocker-återställning](#join-a-device-to-azure-ad-desktop-sso-microsoft-passport-for-azure-ad-administrator-bitlocker-recovery)
* [MDM automatisk registrering, Self-Service Bitlocker-återställning, ytterligare lokala administratörer att Windows 10-enheter via Azure AD Join](#mdm-auto-enrollment)

## <a name="common-features"></a>Vanliga funktioner
#### <a name="directory-objects"></a>Katalogobjekt
**Typ:** vanliga funktioner

Användning standardkvot är 150 000 objekt. Ett objekt är en post i katalogtjänsten som representeras av sitt unika namn. Ett exempel på ett objekt är en användarpost som används för autentiseringsändamål. Kontakta supporten om du behöver överskrider standardkvoten. Begränsningen på 500 000 objekt gäller inte för Office 365, Microsoft Intune eller någon annan betald onlinetjänst från Microsoft som förlitar sig på Azure Active Directory för katalogtjänster.

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| Upp till 500 000 objekt |Det finns ingen gräns för objektet |Det finns ingen gräns för objektet |Inga objekt gränsen för Office 365-användarkonton |

#### <a name="usergroup-management-addupdatedelete-user-based-provisioning-device-registration"></a>Hantering av användare/grupp (Lägg till/Uppdatera/ta bort), användarbaserade etablering, registrering av enheten
**Typ:** vanliga funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| ![Markera][12] |![Markera][12] |![Markera][12] |![Markera][12] |

**Mer information:**

* [Administrera Azure AD-katalogen](active-directory-administer.md)
* [Översikt över Azure Active Directory Device Registration](active-directory-conditional-access-device-registration-overview.md)

#### <a name="single-sign-on-sso"></a>Enkel inloggning (SSO)
**Typ:** vanliga funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| 10 appar per användare (1) |10 appar per användare (1) |Obegränsat (2) |10 appar per användare (1) |

1. Med Azure AD Kostnadsfri och Azure AD Basic får slutanvändarna enkel inloggning till upp till tio program.
2. Integrering med självbetjäning för program som har stöd för SAML, SCIM eller formulärbaserad autentisering med hjälp av mallar på menyn för programgalleriet. Mer information finns i [Konfigurera enkel inloggning för program som inte ingår i Azure Active Directory-programgalleriet](active-directory-saas-custom-apps.md).

**Mer information:**

* [Hantera program med Azure Active Directory (AD)](active-directory-enable-sso-scenario.md)

#### <a name="self-service-password-change-for-cloud-users"></a>Självbetjäning för ändring av lösenord för molnanvändare
**Typ:** vanliga funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| ![Markera][12] |![Markera][12] |![Markera][12] |![Markera][12] |

**Mer information:**

* [Uppdatera ditt eget lösenord](active-directory-passwords-update-your-own-password.md#reset-or-unlock-my-password-for-a-work-or-school-account)

#### <a name="connect--sync-engine-that-extends-on-premises-directories-to-azure-active-directory"></a>Ansluta (Synkroniseringsmotorn som utökar lokala kataloger till Azure Active Directory)
**Typ:** vanliga funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| ![Markera][12] |![Markera][12] |![Markera][12] |![Markera][12] |

**Mer information:**

* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)

#### <a name="securityusage-reports"></a>Säkerhet/användningsrapporter
**Typ:** vanliga funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| 3 grundläggande rapporter |3 grundläggande rapporter |Avancerade rapporter |3 grundläggande rapporter |

**Mer information:**

* [Visa åtkomst- och användningsrapporterna](active-directory-view-access-usage-reports.md)

## <a name="premium-and-basic-features"></a>Premium- och grundläggande funktioner
#### <a name="group-based-access-managementprovisioning"></a>Gruppbaserad åtkomsthantering/etablering
**Typ:** grundläggande funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; |![Markera][12] | ![Markera][12] | &nbsp; |

**Mer information:**

* [Hantera åtkomst till SaaS-program med hjälp av en grupp](active-directory-accessmanagement-group-saasapps.md)

#### <a name="self-service-password-reset-for-cloud-users"></a>Självbetjäning för återställning av lösenord för molnanvändare
**Typ:** grundläggande funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; |![Markera][12] |![Markera][12] | ![Markera][12] |

**Mer information:**

* [Återställning av Azure AD-lösenord för användare och administratörer](active-directory-passwords.md)

#### <a name="company-branding-logon-pagesaccess-panel-customization"></a>Företagsprofiler (Inloggningssidor/Access Panel-anpassning)
**Typ:** grundläggande funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; |![Markera][12] |![Markera][12] | ![Markera][12] |

**Mer information:**

* [Anpassa inloggnings- och åtkomstpanelsidorna till ditt företag](active-directory-add-company-branding.md)

#### <a name="application-proxy"></a>Programproxy
**Typ:** grundläggande funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; |![Markera][12] | ![Markera][12] | &nbsp; |

**Mer information:**

* [Hur du ger säker fjärråtkomst till lokala program](active-directory-application-proxy-get-started.md)

#### <a name="sla-999"></a>SLA 99,9%
**Typ:** grundläggande funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; |![Markera][12] |![Markera][12] | ![Markera][12] |

**Mer information:**

* [serviceavtal](https://azure.microsoft.com/support/legal/sla/)

## <a name="premium-features"></a>Premium-funktioner


#### <a name="self-service-group"></a>Self-Service Group och app Management/egen-Service programmet tillägg/dynamiska grupper
**Typ:** Premium-funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; | &nbsp; | ![Markera][12]| &nbsp; |

#### <a name="self-service-password-resetchangeunlock-with-on-premises-write-back"></a>Självbetjäning lösenord återställning/ändra/Lås upp med lokalt återskrivning
**Typ:** Premium-funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; | &nbsp; | ![Markera][12] | &nbsp; |

#### <a name="multi-factor-authentication-cloud-and-on-premises-mfa-server"></a>Multi-Factor Authentication (via molnet och lokalt (MFA-server))
**Typ:** Premium-funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; | &nbsp; |![Markera][12] |Begränsad till molnet endast för Office 365-appar |

**Mer information:**

* [Vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)


#### <a name="mim-cal-mim-server"></a>MIM-CAL + MIM-servern
Programvarurättigheter för Microsoft Identity Manager Server beviljas med Windows Server-licenser (alla versioner). Eftersom Microsoft Identity Manager körs på operativsystemet Windows Server, förutsatt att servern körs på en giltig licensierad kopia av Windows Server, kan sedan Microsoft Identity Manager installeras och används på servern. Ingen annan separat licens krävs för Microsoft Identity Manager Server.

**Typ:** Premium-funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; | &nbsp; |![Markera][12] | &nbsp; |

#### <a name="cloud-app-discovery"></a>Cloud App Discovery
**Typ:** Premium-funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; | &nbsp; | ![Markera][12] | &nbsp; |

**Mer information:**

* [Hitta ohanterad molnprogram med Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md)

#### <a name="azure-ad-connect-health"></a>Azure AD Connect Health
**Typ:** Premium-funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; | &nbsp; | ![Markera][12] | &nbsp; |

**Mer information:**

* [Övervaka den lokala identitetsinfrastrukturen och synkroniseringstjänster i molnet](active-directory-aadconnect-health.md)

#### <a name="automatic-password-rollover-for-group-accounts"></a>Automatisk lösenordsförnyelse för gruppkonton
**Typ:** Premium-funktioner

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; | &nbsp; | ![Markera][12] | &nbsp; |

#### <a name="identity-protection"></a>Identity Protection
**Typ:** Premium-funktioner

| Free Edition | Basic-versionen | Premium P2-versionen | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; | &nbsp; | ![Markera][12] | &nbsp; |

#### <a name="privileged-identity-management"></a>Privileged Identity Management
**Typ:** Premium-funktioner

| Free Edition | Basic-versionen | Premium P2-versionen | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; | &nbsp; | ![Markera][12] | &nbsp; |

## <a name="azure-active-directory-join--windows-10-only--related-features"></a>Azure Active Directory Join – endast relaterade funktioner för Windows 10
#### <a name="join-a-device-to-azure-ad-desktop-sso-microsoft-passport-for-azure-ad-administrator-bitlocker-recovery"></a>Anslut en enhet till Azure AD, Desktop SSO, Microsoft Passport för Azure AD, Administrator Bitlocker-återställning
**Typ:** Azure Active Directory Join – endast relaterade funktioner för Windows 10

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| ![Markera][12] |![Markera][12] |![Markera][12] |![Markera][12] |


#### <a name="mdm-auto-enrollment"></a>MDM automatisk registrering, Self-Service Bitlocker-återställning, ytterligare lokala administratörer att Windows 10-enheter via Azure AD Join
**Typ:** Azure Active Directory Join – endast relaterade funktioner för Windows 10

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; | &nbsp; | ![Markera][12] | &nbsp; |

#### <a name="enterprise-state-roaming"></a>Företagsroaming
**Typ:** Azure Active Directory Join – endast relaterade funktioner för Windows 10

**Tillgänglighet:**

| Free Edition | Basic-versionen | Premium (P1 och P2)-versioner | Office 365-appar |
|:---:|:---:|:---:|:---:|
| &nbsp; | &nbsp; | ![Markera][12] | &nbsp; |

**Mer information:**

* [Företagsroaming](active-directory-windows-enterprise-state-roaming-overview.md)

## <a name="next-steps"></a>Nästa steg
* [Komma igång med Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Lägga till företagsanpassning till inloggnings-- och åtkomstpanel sidor](active-directory-add-company-branding.md)
* [Visa åtkomst- och användningsrapporterna](active-directory-view-access-usage-reports.md)

<!--Image references-->
[12]: ./media/active-directory-editions/ic195031.png
