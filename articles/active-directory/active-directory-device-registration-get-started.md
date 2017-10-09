---
title: "aaaHow tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory | Microsoft Docs"
description: "Konfigurera din domänanslutna Windows-enheter tooregister automatiskt med Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 1d736eba734418231f12e23a8fc1a93405f129c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-device-registration"></a>Komma igång med enhetsregistrering i Azure Active Directory

Azure Active Directory device registration är hello grunden för scenarier med enhetsbaserad villkorlig åtkomst. När en enhet registreras, tillhandahåller Azure Active Directory-enhetsregistrering hello-enhet med en identitet som används tooauthenticate hello enhet när hello användaren loggar in. hello autentiserade enheten och hello attribut för hello enhet, kan du sedan vara används tooenforce villkorliga åtkomstprinciper för program som finns i hello molnet och lokalt.

När den kombineras med en enhetsattributen lösning för mobila enheter, till exempel Microsoft Intune, uppdateras hello attributen i Azure Active Directory med ytterligare information om hello enhet. Detta ger dig toocreate regler för villkorlig åtkomst som säkerställer att åtkomsten från enheter toomeet dina krav för säkerhet och efterlevnad. Mer information om registrerar enheter i Microsoft Intune finns i registrera enheter för hantering i Intune.
Scenarier som aktiveras med Azure Active Directory Device Registration Azure Active Directory Device Registration inkluderar stöd för iOS, Android och Windows enheter. hello enskilda scenarierna som använder Azure AD Device Registration kan ha mer specifika krav och Plattformsstöd. 

Dessa scenarier är följande:

- **Villkorlig åtkomst för Office 365-program med Microsoft Intune:** IT-administratörer kan etablera villkorlig åtkomst enheten principer toosecure företagsresurser, medan på hello samma tid så att informationsarbetare på kompatibla enheter tooaccess hello-tjänster. Mer information finns i Enhetsprinciper för villkorlig åtkomst för Office 365-tjänster.

- **Villkorlig åtkomst tooapplications som finns lokalt:** du kan använda registrerade enheter med åtkomstprinciper för program som är konfigurerade toouse AD FS med Windows Server 2012 R2. Mer information om hur du konfigurerar lokal villkorlig åtkomst finns i [Konfigurera lokal villkorlig åtkomst med hjälp av Azure Active Directory Device Registration](active-directory-device-registration-on-premises-setup.md).

## <a name="setting-up-azure-active-directory-device-registration"></a>Konfigurera Azure Active Directory Device Registration

toosetup enhetsregistrering, har du flera alternativ:

- Enheter kan registreras när kopplade tooAzure AD-domän. Mer information om det här avsnittet kan du läsa mer om Azure AD Join och hello-inställningar som krävs för användare toojoin Azure AD-domän.

- Enheter kan registreras när användarna lägger till arbetet eller skolan konton tooWindows på en personlig enhet eller när mobila enheter ansluter tooa arbetsresurser kräver registrering. tooensure, måste du aktivera Azure Active Directory Device Registration hello Azure-portalen. 

- Enheter kan registreras med hjälp av automatisk enhetsregistrering för traditionella domänanslutna datorer. tooensure, måste du första installationen Azure AD Connect innan du fortsätter med automatisk enhetsregistrering.

Senaste instruktioner finns i [hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).  
Du kan också granska och aktivera eller inaktivera registrerade enheter med hjälp av hello-Administratörsportalen i Azure Active Directory.

## <a name="enable-hello-azure-active-directory-device-registration-service"></a>Aktivera hello enhetsregistreringstjänsten för Azure Active Directory

**tooenable hello enhetsregistreringstjänsten för Azure Active Directory:**

1.  Logga in toohello Microsoft Azure-portalen som administratör.

2.  Hello vänster, Välj **Active Directory**.

3.  På fliken för hello katalog väljer du din katalog.

4.  Klicka på **Konfigurera**.

5.  Rulla för**enheter**.

6.  Markera alla användare kan registrera SINA enheter med AZURE AD.

7.  Välj hello maximalt antal enheter som du vill tooauthorize per användare.

hello registrering med Microsoft Intune eller Mobile Device Management för Office 365 kräver registrering av enheten. Om du har konfigurerat någon av dessa tjänster **alla** är markerad och **NONE** är inaktiverad. Kontrollera att de är korrekt konfigurerade och har rätt hello-licensiering.

Som standard aktiveras inte tvåfaktorsautentisering för hello-tjänsten. Tvåfaktorsautentisering rekommenderas dock när du registrerar en enhet.

- Innan du kräver tvåfaktorsautentisering för den här tjänsten, måste du konfigurera en provider för tvåfaktorsautentisering i Azure Active Directory och konfigurera dina användarkonton för multi-Factor Authentication går att lägga till Multi-Factor Authentication tooAzure Active Directory

- Om du använder AD FS med Windows Server 2012 R2 måste du konfigurera en modul för tvåfaktorsautentisering i AD FS, finns i använda Multifaktorautentisering med Active Directory Federation Services.

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Visa och hantera enhetsobjekt i Azure Active Directory

Du kan visa, blockera och avblockera enheter från hello Azure-administratörsportalen. En enhet som är blockerad har inte längre åtkomst tooapplications som är konfigurerade tooallow endast registrerade enheter.

**tooview och hantera enhetsobjekt i Azure Active Directory:**
 
1.  Logga in toohello Microsoft Azure-portalen som administratör.

2.  Hello vänster, Välj **Active Directory**.

3.  Välj din katalog.

4.  Välj **användare**. 

5.  Klicka på hello användare som du vill toosee hello enheter.

6.  Välj **enheter**.

7.  Välj **registrerade enheter**.

Nu kan du granska, blockera eller avblockera hello användarens registrerade enheter.
Windows 10-enheter som är lokalt ansluten till domänen och registreras automatiskt visas inte under fliken för hello-användare. Använd hello Get-MsolDevice PowerShell-kommandot toofind alla företagets enheter. 


## <a name="next-steps"></a>Nästa steg

toosetup automatisk registrering av enheten, se [hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).


