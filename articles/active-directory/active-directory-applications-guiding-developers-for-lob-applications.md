---
title: "aaaDevelop appar för Azure AD | Microsoft Docs"
description: "Den här artikeln innehåller riktlinjer för integrera Azure-program med Active Directory skrivna för hello IT-proffs."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: dd69f2bc-37c5-457c-857d-27acb84267fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2924be752af0be2843b1d9b74d9ec446d3fe1ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a>Utveckla av branschspecifika appar för Azure Active Directory
Den här guiden ger en översikt över utvecklar line-of-business (LoB)-program för Azure Active Directory (AD) .hello avsedd målgruppen är globala administratörer för Active Directory/Office 365.

## <a name="overview"></a>Översikt
Skapa program som är integrerade med Azure AD ger användare i din organisation enkel inloggning med Office 365. Med hello program i Azure AD-ger dig kontroll över hello autentiseringsprincip för hello program. Mer om villkorsstyrd åtkomst och hur tooprotect appar med multifaktorautentisering (MFA) Se toolearn [konfigurera åtkomstregler](active-directory-conditional-access-azuread-connected-apps.md).

Registrera ditt program toouse Azure Active Directory. Registrera hello program innebär att utvecklarna kan använda Azure AD-användare tooauthenticate och begär åtkomst toouser resurser, till exempel e-post, kalender och dokument.

Alla medlemmar i din katalog (inte gäster) kan registrera ett program, även kallat *skapar ett programobjekt*.

Registrera ett program kan alla användare toodo hello följande:

* Hämta en identitet för de program som kan identifieras av Azure AD
* Skaffa en eller flera hemligheter/nycklar som hello program kan du använda tooauthenticate själva tooAD
* Varumärken hello programmet i hello Azure-portalen med ett anpassat namn, logotyp och så vidare.
* Använda Azure AD auktorisering funktioner tootheir appen, inklusive:

  * Rollbaserad åtkomstkontroll (RBAC)
  * Azure Active Directory som server för oAuth-auktorisering (skydda en API som exponeras av programmet hello)
* Deklarera behörighet krävs för hello programmet toofunction som förväntat, inklusive:

      - Appbehörigheter (endast globala administratörer). Till exempel: medlemskap i en annan Azure AD-program eller roll medlemskap relativa tooan Azure resurs, resursgrupp, eller en prenumeration
      - Delegerad behörighet (alla användare). Till exempel: Azure AD-inloggning och Läs profil

> [!NOTE]
> Som standard kan alla medlemmar registrera ett program. hur toorestrict behörigheter för att registrera program toospecific medlemmar, se toolearn [hur program läggs tooAzure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).
>
>

Här är vad du, hello global administratör, måste toodo toohelp utvecklare förbereda sina program för produktion:

* Konfigurera åtkomstregler (åtkomst princip/MFA)
* Konfigurera hello app toorequire Användartilldelning och tilldela användare
* Ignorera hello medgivande standardgränssnittet

## <a name="configure-access-rules"></a>Konfigurera åtkomstregler
Konfigurera programspecifika regler tooyour SaaS-appar. Du kan till exempel kräva MFA eller bara tillåta åtkomst toousers på betrodda nätverk. hello information om detta finns i hello dokument [konfigurera åtkomstregler](active-directory-conditional-access-azuread-connected-apps.md).

## <a name="configure-hello-app-toorequire-user-assignment-and-assign-users"></a>Konfigurera hello app toorequire Användartilldelning och tilldela användare
Som standard kan användare komma åt program utan att ha tilldelats. Om programmet hello exponerar roller eller om du vill hello programmet tooappear på åtkomstpanelen för en användare, bör du kräva Användartilldelning.

[Kräv användartilldelning](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Om du är en Azure AD Premium eller Enterprise Mobility Suite (EMS) prenumerant, rekommenderar vi starkt med hjälp av grupper. Tilldela grupper toohello program kan du toodelegate pågående access management toohello ägare hello gruppen. Du kan skapa hello grupp eller be hello ansvarig part i din organisation toocreate hello-gruppen med din grupp management anläggning.

[Tilldela användare tooan program](active-directory-applications-guiding-developers-assigning-users.md)  
[Tilldela grupper tooan program](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Ignorera tillstånd
Som standard genomgår varje användare en medgivande upplevelse toosign i. hello kan medgivande, och ber användarna toogrant behörigheter tooan programmet vara förvirrande för användare som inte är bekant med att fatta detta beslut.

För program som du litar på, kan du förenkla hello användarupplevelse av principer toohello program uppdrag av din organisation.

Mer information om tillstånd och hello samtycke upplevelse i Azure, se [integrera program med Azure Active Directory](active-directory-integrating-applications.md).

## <a name="related-articles"></a>Relaterade artiklar
* [Aktivera säker fjärråtkomst tooon lokala program med Azure AD Application Proxy](active-directory-application-proxy-get-started.md)
* [Förhandsgranska Azure villkorlig åtkomst för SaaS-appar](active-directory-conditional-access-azuread-connected-apps.md)
* [Hantera åtkomst tooapps med Azure AD](active-directory-managing-access-to-apps.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
