---
title: "Översikt över aaaAzure Active Directory device registration | Microsoft Docs"
description: "Azure Active Directory device registration är hello grunden för scenarier med enhetsbaserad villkorlig åtkomst. När en enhet registreras tilldelar hello Azure Active Directory device registration bestämmelser enhet med en identitet som används tooauthenticate hello enhet när hello användaren loggar in."
services: active-directory
keywords: enhetsregistrering, aktivera enhetsregistrering, enhetsregistrering och MDM
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 1e92c1a2-01b8-4225-950b-373cd601b035
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: b9846e34fe873d271ebe71cbf8e70c6b4768885b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-device-registration"></a>Komma igång med enhetsregistrering i Azure Active Directory
Azure Active Directory device registration är hello grunden för scenarier med enhetsbaserad villkorlig åtkomst. När en enhet registreras, tillhandahåller Azure Active Directory-enhetsregistrering hello-enhet med en identitet som används tooauthenticate hello enhet när hello användaren loggar in. hello autentiserade enheten och hello attribut för hello enhet, kan du sedan vara används tooenforce villkorliga åtkomstprinciper för program som finns i hello molnet och lokalt.

När den kombineras med en enhetsattributen lösning för mobila enheter, till exempel Microsoft Intune, uppdateras hello attributen i Azure Active Directory med ytterligare information om hello enhet. Detta ger dig toocreate regler för villkorlig åtkomst som säkerställer att åtkomsten från enheter toomeet dina krav för säkerhet och efterlevnad. Mer information om hur du registrerar enheter i Microsoft Intune finns i [Registrera enheter för hantering i Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).

## <a name="scenarios-enabled-by-azure-active-directory-device-registration"></a>Scenarier som använder enhetsregistrering i Azure Active Directory
Enhetsregistrering i Azure Active Directory har stöd för iOS-, Android- och Windows-enheter. hello enskilda scenarierna som använder Azure AD device registration kan ha mer specifika krav och Plattformsstöd. Dessa scenarier är följande:

* **Villkorlig åtkomst tooapplications som finns lokalt**: du kan använda registrerade enheter med åtkomstprinciper för program som är konfigurerade toouse AD FS med Windows Server 2012 R2. Mer information om hur du konfigurerar lokal villkorlig åtkomst finns i [Konfigurera lokal villkorlig åtkomst med hjälp av Azure Active Directory Device Registration](active-directory-device-registration-on-premises-setup.md).
* **Villkorlig åtkomst för Office 365-program med Microsoft Intune** : IT-administratörer kan etablera villkorlig åtkomst enheten principer toosecure företagsresurser, medan på hello samma tid så att informationsarbetare på kompatibla enheter tooaccess hello-tjänster. Mer information finns i [Enhetsprinciper för villkorlig åtkomst för Office 365-tjänster](active-directory-conditional-access-device-policies.md).

## <a name="setting-up-azure-active-directory-device-registration"></a>Konfigurera enhetsregistrering i Azure Active Directory
Du behöver tooenable Azure AD device registration hello Azure-portalen så att mobila enheter kan identifiera hello-tjänsten genom att söka efter välkända DNS-poster. Du måste konfigurera företagets DNS så att Windows 10, Windows 8.1, Windows 7, Android och iOS-enheter kan identifiera och använda hello-tjänsten.
Du kan visa och aktivera/inaktivera registrerade enheter med hjälp av hello-Administratörsportalen i Azure Active Directory.

> [!NOTE]
> Senaste anvisningar för hur tooset in automatisk enhetsregistrering Se [hur toosetup automatisk registrering av Windows-domän-anslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
> 
> 

### <a name="enable-azure-active-directory-device-registration-service"></a>Aktivera enhetsregistreringstjänsten i Azure Active Directory
1. Logga in toohello Microsoft Azure-portalen som administratör.
2. Hello vänster, Välj **Active Directory**.
3. På hello **Directory** , Välj din katalog.
4. Välj hello **konfigurera** fliken.
5. Rulla toohello avsnittet **enheter**.
6. Välj **Alla** för **Användare kan ansluta enheter till Azure AD**.
7. Välj hello maximalt antal enheter som du vill tooauthorize per användare.

> [!NOTE]
> Registrering med Microsoft Intune eller Mobile Device Management för Office 365 kräver Workplace Join. Om du har konfigurerat någon av dessa tjänster, alla är markerat och hello inga är inaktiverat.
> 
> 

Som standard aktiveras inte tvåfaktorsautentisering för hello-tjänsten. Tvåfaktorsautentisering rekommenderas dock när du registrerar en enhet.

* Innan du kräver tvåfaktorsautentisering för den här tjänsten, du måste konfigurera en provider för tvåfaktorsautentisering i Azure Active Directory och konfigurera dina användarkonton för multi-Factor Authentication, se [att lägga till Multi-Factor Autentisering tooAzure Active Directory](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
* Om du använder AD FS med Windows Server 2012 R2 måste du konfigurera en modul för tvåfaktorsautentisering i AD FS. Mer information finns i [Använda Multi-Factor Authentication med Active Directory Federation Services](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="configure-azure-active-directory-device-registration-discovery"></a>Konfigurera identifiering av enhetsregistrering i Azure Active Directory
Windows 7 och Windows 8.1-enheter identifieras hello registreringstjänsten för enheten genom att kombinera hello användarens kontonamn med ett välkänt registrering server enhetsnamn.

Du måste skapa en DNS CNAME-post som pekar toohello A-posten som associeras med din Azure Active Directory device registration service. hello CNAME-post måste använda hello välkända prefixet enterpriseregistration följt av hello UPN-suffix som används av hello användarkonton i din organisation. Om din organisation använder flera UPN-suffix måste flera CNAME-poster skapas i DNS.

Till exempel om du använder två UPN-suffix i din organisation med namnet @contoso.com och @region.contoso.com, skapar du hello följande DNS-poster.

| Post | Typ | Adress |
| --- | --- | --- |
| enterpriseregistration.contoso.com |CNAME |enterpriseregistration.windows.net |
| enterpriseregistration.region.contoso.com |CNAME |enterpriseregistration.windows.net |

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Visa och hantera enhetsobjekt i Azure Active Directory
1. Du kan visa, blockera och avblockera enheter från hello Azure-administratörsportalen. En enhet som är blockerad har inte längre åtkomst tooapplications som är konfigurerade tooallow endast registrerade enheter.
2. Logga in toohello Microsoft Azure Portal som administratör.
3. Hello vänster, Välj **Active Directory**.
4. Välj din katalog.
5. Välj hello **användare** fliken. Välj en användare tooview sina enheter
6. Välj hello **enheter** fliken.
7. Välj **registrerade enheter** från hello nedrullningsbara meny.
8. Här kan du visa, blockera eller avblockera hello användarregistrerade enheter.

## <a name="additional-topics"></a>Ytterligare information
Du kan registrera Windows 7- och Windows 8.1-domänanslutna enheter med enhetsregistrering i Azure AD. hello följande avsnitt innehåller mer information om hello förutsättningar och hello steg krävs tooconfigure enhetsregistrering för Windows 7 och Windows 8.1-enheter.

* [Automatisk enhetsregistrering med Azure Active Directory för domänanslutna Windows-enheter](active-directory-conditional-access-automatic-device-registration.md)
* [Automatisk enhetsregistrering med Azure Active Directory för domänanslutna Windows 10-enheter](active-directory-azureadjoin-devices-group-policy.md)

