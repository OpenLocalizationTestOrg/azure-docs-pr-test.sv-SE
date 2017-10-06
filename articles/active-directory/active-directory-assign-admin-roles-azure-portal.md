---
title: "aaaAssigning administratörsroller i Azure Active Directory | Microsoft Docs"
description: "En administratörsroll kan skapa eller redigera användare, Tilldela administratörsroller tooothers, återställa användarlösenord, hantera användarlicenser eller hantera domäner. En användare som är tilldelad en administratörsroll har hello samma behörigheter över alla cloud services toowhich din organisation prenumererar på."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7fc27e8e-b55f-4194-9b8f-2e95705fb731
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.reviewer: Vince.Smith
ms.custom: it-pro;
ms.openlocfilehash: 41cddbf311767d9995c99ee386e6d276745dad18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Tilldela administratörsroller i Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-assign-admin-roles-azure-portal.md)
> * [Klassisk Azure-portal](active-directory-assign-admin-roles.md)
>
>

Med Azure Active Directory (Azure AD), kan du ange separata administratörer tooserve olika funktioner. Dessa administratörer har åtkomst toovarious funktioner i hello Azure-portalen eller klassiska Azure-portalen och, beroende på deras roll kommer att kunna toocreate eller redigera användare, Tilldela administratörsroller tooothers, återställa användarlösenord, hantera användarlicenser, och Hantera domäner, bland annat. En användare som är tilldelad en administratörsroll har hello samma behörigheter för alla hello molntjänster som din organisation prenumererar på, oavsett om du tilldelar hello roll i hello Office 365-portalen eller i hello klassiska Azure-portalen eller med hjälp av hello Azure AD-modulen för Windows PowerShell.

hello följande administratörsroller är tillgängliga:

* **Faktureringsadministratör**: gör inköp, hanterar prenumerationer, hanterar supportärenden och övervakar tjänstens hälsa.

* **Kompatibilitet administratören**: användare med den här rollen ha administrativ behörighet i i hello Office 365-säkerhet & Efterlevnadscentret och administrationscentret för Exchange. Mer information i ”[om administratörsroller i Office 365](https://support.office.com/en-us/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)”.

* **CRM-tjänstadministratör**: användare med den här rollen har globala behörigheter i Microsoft CRM Online, när hello-tjänsten finns, samt hello möjlighet toomanage supportärenden och övervakar tjänstens hälsa. Mer information på [om Office 365-administratörsroller](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Enhetsadministratörer**: användare med den här rollen blir för lokala datoradministratörer på alla Windows 10-enheter som är kopplade tooAzure Active Directory. De har inte hello möjlighet toomanage enheter objekt i Azure Active Directory.

* **Directory läsare**: Detta är en äldre roll som är tilldelade toobe tooapplications som inte stöder hello [medgivande Framework](active-directory-integrating-applications.md). Det ska inte tilldelas tooany användare.

* **Synkronisering Katalogkonton**: Använd inte. Den här rollen är tilldelas automatiskt toohello Azure AD Connect-tjänsten, och inte avsedd eller stöd för annan användning.

* **Directory skrivare**: Detta är en äldre roll som är tilldelade toobe tooapplications som inte stöder hello [medgivande Framework](active-directory-integrating-applications.md). Det ska inte tilldelas tooany användare.

* **Exchange-tjänstadministratör**: användarna med den här rollen har globala behörigheter i Microsoft Exchange Online när hello-tjänsten finns. Mer information på [om Office 365-administratörsroller](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Global administratör / företagsadministratör**: användare med den här rollen har åtkomst tooall administrativa funktioner i Azure Active Directory, samt att federate tooAzure Active Directory som Exchange Online, SharePoint Online-tjänster och Skype för företag – Online. hello person som registrerar sig för hello Azure Active Directory-klient blir en global administratör. Endast globala administratörer kan tilldela andra administratörsroller. Det kan finnas mer än en global administratör i företaget. Globala administratörer kan återställa hello lösenord för alla användare och alla andra administratörer.

  > [!NOTE]
  > Den här rollen identifieras som ”företagets administratör” i Microsoft Graph API, Azure AD Graph API och Azure AD PowerShell. Det är ”Global administratör” i hello [Azure-portalen](https://portal.azure.com).
  >
  >

* **Gästen bjuder in**: användare i den här rollen kan hantera Azure Active Directory B2B gästen användaren inbjudningar när hello ”medlemmar kan bjuda in” användare är tooNo. Mer information om B2B-samarbete när [om hello Azure AD B2B-samarbete preview](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Det innehåller inte några andra behörigheter.

* **Intune-tjänstadministratören**: användarna med den här rollen har globala behörigheter i Microsoft Intune Online när hello-tjänsten finns. Dessutom kan den här rollen innehåller hello möjlighet toomanage användare och enheter i ordning tooassociate princip, samt skapa och hantera grupper.

* **Postlåda administratören**: den här rollen används endast som en del av Exchange Online e-postsupport för RIM Blackberry-enheter. Om din organisation inte använder Exchange Online-epost på RIM Blackberry-enheter, Använd inte den här rollen.

* **Samarbeta stöd för nivå 1**: Använd inte. Den här rollen är inaktuell och kommer att tas bort från Azure AD i hello framtida. Den här rollen är avsedd för användning av ett litet antal Microsoft-partner återförsäljning och är inte avsedd för allmänt bruk.

* **Samarbeta stöd för nivå 2**: Använd inte. Den här rollen är inaktuell och kommer att tas bort från Azure AD i hello framtida. Den här rollen är avsedd för användning av ett litet antal Microsoft-partner återförsäljning och är inte avsedd för allmänt bruk.

* **Lösenord för administratör / supportavdelning administratören**: användare med den här rollen kan återställa lösenord, hanterar tjänstbegäranden och övervakar tjänstens hälsa. Lösenordsadministratörer kan bara återställa lösenord för användare och andra lösenordsadministratörer.

  > [!NOTE]
  > Den här rollen identifieras som ”supportavdelning administratör” i Microsoft Graph API, Azure AD Graph API och Azure AD PowerShell. Det är ”Lösenordsadministratör” Hej [Azure-portalen](https://portal.azure.com/).
  >
  >
  
* **Power BI tjänstadministratör**: användare med den här rollen har globala behörigheter i Microsoft Power BI, när hello-tjänsten finns, samt hello möjlighet toomanage supportärenden och övervakar tjänstens hälsa. Mer information på [om Office 365-administratörsroller](https://support.office.com/en-us/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-001&ad=US).

* **Privilegierade rollen Administratör**: användare med den här rollen kan hantera rolltilldelningar i Azure Active Directory, såväl i Azure AD Privileged Identity Management. Den här rollen kan dessutom hantering av alla aspekter av Privileged Identity Management.

* **Säkerhetsadministratör**: användare med den här rollen har alla hello läsbehörighet hello säkerhetsrollen reader plus hello möjlighet toomanage konfiguration för säkerhetsrelaterade services: Azure Active Directory Identity Protection Privileged Identity Management och Office 365-säkerhet och efterlevnad Center. Mer information om Office 365 behörigheter finns på [behörigheter i hello Office 365-säkerhet & Efterlevnadscentret](https://support.office.com/en-us/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Säkerhet Reader**: användare med den här rollen har globala skrivskyddad åtkomst, inklusive alla uppgifter i Azure Active Directory, identitetsskydd, Privileged Identity Management samt hello möjlighet tooread Azure Active Directory-inloggning rapporter och granskningsloggar. hello roll ger också läsbehörighet i Office 365-säkerhet och efterlevnad Center. Mer information om Office 365 behörigheter finns på [behörigheter i hello Office 365-säkerhet & Efterlevnadscentret](https://support.office.com/en-us/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Stöd för tjänstadministratör**: med den här rollen kan användarna öppna supportärenden med Microsoft för Azure och Office 365-tjänster och vyer hello instrumentpanelen och meddelandet servicecenter hello Azure-portalen och administrationsportalen för Office 365. Mer information på [om Office 365-administratörsroller](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **SharePoint-tjänstadministratör**: användare med den här rollen har globala behörigheter i Microsoft SharePoint Online, när hello-tjänsten finns, samt hello möjlighet toomanage supportärenden och övervakar tjänstens hälsa. Mer information på [om Office 365-administratörsroller](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Skype för företag / Lync tjänstadministratör**: användare med den här rollen har globala behörigheter i Microsoft Skype för företag, när hello-tjänsten finns, samt hantera Skype-specifika användarattribut i Azure Active Directory. Dessutom kan den här rollen ger hello möjlighet toomanage supportärenden och övervaka tjänsten hälsa. Mer information på [om Office 365-administratörsroller](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  > [!NOTE]
  > Den här rollen identifieras som ”Lync tjänstadministratör” i Microsoft Graph API, Azure AD Graph API och Azure AD PowerShell. Det är ”Skype för företag tjänstadministratör” Hej [Azure-portalen](https://portal.azure.com/).
  >
  >

* **Användaren kontoadministratör**: användare med den här rollen kan skapa och hantera alla aspekter av användare och grupper. Dessutom den här rollen omfattar hello möjlighet toomanage supportärenden och övervakar service hälsa. Vissa begränsningar gäller. Till exempel den här rollen kan inte ta bort en global administratör och när du tillåter ändring av lösenord för icke-administratörer, tillåter inte att ändra lösenord för globala administratörer eller andra Privilegierade administratörer.

## <a name="administrator-permissions"></a>Administratörsbehörighet

### <a name="billing-administrator"></a>Faktureringsadministratör

| Kan göra | Det går inte att göra |
| --- | --- |
|<p>Visa information om företagets och användare</p><p>Hantera supportärenden för Office</p><p>Utföra fakturerings- och köp åtgärder för Office-produkter</p> |<p>Återställa användarlösenord</p><p>Skapa och hantera användarvyer</p><p>Skapa, redigera, ta bort användare och grupper och hantera användarlicenser</p><p>Hantera domäner</p><p>Hantera information om företaget</p><p>Delegera administrativa roller tooothers</p><p>Använda katalogsynkronisering</p><p>Visa granskningsloggar</p>|

### <a name="global-administrator"></a>Global administratör
| Kan göra | Det går inte att göra |
| --- | --- |
| <p>Visa information om företagets och användare</p><p>Hantera supportärenden för Office</p><p>Utföra fakturerings- och köp åtgärder för Office-produkter</p><p>Återställa användarlösenord</p>
<p>Återställa andra administratörslösenord</p> <p>Skapa och hantera användarvyer</p><p>Skapa, redigera, ta bort användare och grupper och hantera användarlicenser</p><p>Hantera domäner</p><p>Hantera information om företaget</p><p>Delegera administrativa roller tooothers</p><p>Använda katalogsynkronisering</p><p>Aktivera eller inaktivera multifaktorautentisering</p><p>Visa granskningsloggar</p> |Saknas |

### <a name="password-administrator"></a>Lösenordsadministratör
| Kan göra | Det går inte att göra |
| --- | --- |
| <p>Visa information om företagets och användare</p><p>Hantera supportärenden för Office</p><p>Återställa användarlösenord</p> <p>Återställa andra administratörslösenord</p>|<p>Utföra fakturerings- och köp åtgärder för Office-produkter</p><p>Skapa och hantera användarvyer</p><p>Skapa, redigera, ta bort användare och grupper och hantera användarlicenser</p><p>Hantera domäner</p><p>Hantera information om företaget</p><p>Delegera administrativa roller tooothers</p><p>Använda katalogsynkronisering</p><p>Visa rapporter</p>|

### <a name="service-administrator"></a>Tjänstadministratör
| Kan göra | Det går inte att göra |
| --- | --- |
| <p>Visa information om företagets och användare</p><p>Hantera supportärenden för Office</p> |<p>Återställa användarlösenord</p><p>Utföra fakturerings- och köp åtgärder för Office-produkter</p><p>Skapa och hantera användarvyer</p><p>Skapa, redigera, ta bort användare och grupper och hantera användarlicenser</p><p>Hantera domäner</p><p>Hantera information om företaget</p><p>Delegera administrativa roller tooothers</p><p>Använda katalogsynkronisering</p><p>Visa granskningsloggar</p> |

### <a name="user-administrator"></a>Användare med rollen
| Kan göra | Det går inte att göra |
| --- | --- |
| <p>Visa information om företagets och användare</p><p>Hantera supportärenden för Office</p><p>Återställa användarlösenord med begränsningar.</p><p>Återställa andra administratörslösenord</p><p>Återställa lösenord för andra användare</p><p>Skapa och hantera användarvyer</p><p>Skapa, redigera, ta bort användare och grupper och hantera användarlicenser med begränsningar. Han eller hon kan inte ta bort en global administratör eller skapa andra administratörer.</p> |<p>Utföra fakturerings- och köp åtgärder för Office-produkter</p><p>Hantera domäner</p><p>Hantera information om företaget</p><p>Delegera administrativa roller tooothers</p><p>Använda katalogsynkronisering</p><p>Aktivera eller inaktivera multifaktorautentisering</p><p>Visa granskningsloggar</p> |

### <a name="security-reader"></a>Säkerhet läsare
| i | Kan göra |
| --- | --- |
| Identity Protection Center |Läsa alla säkerhetsrapporter och inställningsinformation om för säkerhet<ul><li>Skräppost<li>Kryptering<li>Skydd mot dataförlust<li>Skadlig kod<li>Avancerat skydd<li>Mot nätfiske<li>Mailflow regler |
| Privileged Identity Management |<p>Har skrivskyddad åtkomst tooall information som angetts i Azure AD PIM: principer och rapporter för Azure AD-rolltilldelningar säkerhet granskar och i hello läsa framtida åtkomst toopolicy data och rapporter för scenarier utöver Azure AD-rolltilldelning.<p>**Det går inte att** registrera dig för Azure AD PIM eller göra några ändringar tooit. PIMS portalen eller via PowerShell kan någon i den här rollen aktivera ytterligare roller (till exempel Global administratör eller Privilegierade rollen Administratör), om hello användare är en kandidat för dem. |
| <p>Tjänstens hälsa för Övervakare för Office 365</p><p>Office 365-säkerhet och efterlevnad Center</p> |<ul><li>Läsa och hantera aviseringar<li>Läs säkerhetsprinciper<li>Läsa hotinformation, Cloud App Discovery och karantän i Sök- och undersök<li>Läsa alla rapporter |

### <a name="security-administrator"></a>Säkerhetsadministratör
| i | Kan göra |
| --- | --- |
| Identity Protection Center |<ul><li>Alla behörigheter för rollen för hello säkerhet läsare.<li>Dessutom hello möjlighet tooperform alla IPC åtgärder förutom återställa lösenord. |
| Privileged Identity Management |<ul><li>Alla behörigheter för rollen för hello säkerhet läsare.<li>**Det går inte att** Hantera medlemskap i Azure AD-roller eller inställningar. |
| <p>Tjänstens hälsa för Övervakare för Office 365</p><p>Office 365-säkerhet och efterlevnad Center |<ul><li>Alla behörigheter för rollen för hello säkerhet läsare.<li>Kan konfigurera alla inställningar i hello Advanced Threat Protection-funktionen (skydd mot skadlig kod och virus, skadliga URL-config, URL-spårning, etc.). |

## <a name="details-about-hello-global-administrator-role"></a>Information om hello rollen som global administratör
hello global administratör har åtkomst tooall administrativa funktioner. Som standard tilldelas hello person som registrerar sig för en Azure-prenumeration hello rollen som global administratör för hello-katalogen. Endast globala administratörer kan tilldela andra administratörsroller.

### <a name="tooadd-a-colleague-as-a-global-administrator"></a>tooadd en kollega som global administratör

1. Logga in toohello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med ett konto som är en global administratör för hello klientkatalogen.

   ![Öppna administrationscentret för azure AD](./media/active-directory-assign-admin-roles-azure-portal/active-directory-admin-center.png)

2. Välj **användare och grupper &gt; alla användare**

3. Sök efter hello användare toodesignate som global administratör och öppna hello bladet för den användaren.

4. På bladet användare hello väljer **Directory rollen**.
 
5. Välj hello på hello directory rollen bladet **Global administratör** roll, och spara.

## <a name="assign-or-remove-administrator-roles"></a>Tilldela eller ta bort administratörsroller
hur tooassign administrativa roller tooa användare i Azure Active Directory, se toolearn [och tilldela en användare tooadministrator roller i Azure Active Directory preview](active-directory-users-assign-role-azure-portal.md).

## <a name="deprecated-roles"></a>Föråldrad roller

hello följande roller ska inte användas. De är föråldrad och kommer att tas bort från Azure AD i hello framtida.

* Administratör för ad hoc-licens
* E-post verifierade användaren Creator
* Anslut till en enhet
* Enhetshanteraren
* Användare av enheter
* Anslut enheten till arbetsplatsen

## <a name="next-steps"></a>Nästa steg

* toolearn mer om hur toochange administratörer för en Azure-prenumeration, se [hur tooadd eller ändra Azure-administratörsroller](../billing-add-change-azure-subscription-administrator.md)
* toolearn mer information om hur resursåtkomsten hanteras i Microsoft Azure finns [förstå resursåtkomst i Azure](active-directory-understanding-resource-access.md)
* Mer information om hur Azure Active Directory relaterar tooyour Azure-prenumeration finns [hur Azure-prenumerationer är associerade med Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Hantera användare](active-directory-create-users.md)
* [Hantera lösenord](active-directory-manage-passwords.md)
* [Hantera grupper](active-directory-manage-groups.md)
