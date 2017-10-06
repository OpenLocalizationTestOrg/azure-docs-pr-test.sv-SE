---
title: "aaaAzure Active Directory B2B-samarbete licensiering vägledning | Microsoft Docs"
description: "Azure Active Directory B2B collaboration inte kräver betald Azure AD-licenser, men du kan också hämta betalda funktioner för B2B gästanvändare"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: sasubram
ms.custom: it-pro
ms.openlocfilehash: 8e15d66209b090bff210674ecdacc88cd642dcc0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Vägledning om Azure Active Directory B2B-samarbete och licenser

Du kan använda Azure AD B2B-samarbete funktioner tooinvite gästanvändare i din Azure AD-klient tooallow dem tooaccess Azure AD-tjänster och andra resurser i din organisation. Om du vill tooprovide åtkomstfunktioner toopaid Azure AD B2B-samarbete gästanvändare måste licensieras med lämpliga licenser för Azure AD. 

Närmare bestämt:
* Azure AD ledigt funktioner är tillgängliga för gästanvändare utan ytterligare licenser.
* Om du vill tooprovide åtkomst toopaid Azure AD-funktioner användare tooB2B, måste du ha tillräckligt med licenser toosupport dessa B2B-gästanvändare.
* En bjuda in klient med en Azure AD betald licens har B2B collaboration Använd rättigheter tooan ytterligare fem B2B gäst användare uppmanas toohello klient.
* hello kunden som äger hello bjuda in klient måste vara hello en toodetermine hur många B2B-samarbete användare behöver betald funktionerna i Azure AD. Beroende på hello betald Azure AD funktioner som du vill använda för din gästanvändare måste du ha tillräckligt med Azure AD betald licenser toocover B2B-samarbete användare i hello samma 5:1-förhållande.

Gästanvändare B2B-samarbete läggs till som en användare från ett partnerföretag, inte en medarbetare i din organisation eller en anställd i ett annat företag i din konglomerat. En B2B-gästanvändare kan logga in med autentiseringsuppgifter för externa eller ägs av organisationen som beskrivs i den här artikeln. 

Med andra ord anges B2B-licensiering inte av hur hello användare autentiseras utan i stället av hello relationen mellan hello användaren tooyour organisation. Om användarna inte partners, hanteras de på olika sätt i licensvillkoren. De anses inte vara toobe en B2B-samarbete användare för licensiering även om deras UserType har markerats som ”gäst”. De ska vara licensierade normalt en licens per användare. Dessa användare inkluderar:
* Dina anställda
* Personal som loggar in med hjälp av externa identiteter
* En medarbetare i ett annat företag i din konglomerat


## <a name="licensing-examples"></a>Exempel-licensiering
- En kund vill tooinvite 100 B2B-samarbete användare tooits Azure AD-klient. hello kunden tilldelar åtkomsthantering och etablering för alla användare, men 50 användare kräver också MFA och villkorlig åtkomst. Kunden måste köpa 10 Azure AD Basic licenser och 10 Azure AD Premium P1 licenser toocover användarna B2B korrekt. Om kunden hello planerar toouse identitetsskydd funktioner med B2B-användare kan de måste ha Azure AD Premium P2 licenser toocover hello uppmanas användarna i hello samma 5:1-förhållande.
- En kund har 10 anställda som licensieras alla med Azure AD Premium P1. De vill nu tooinvite 60 B2B användare som kräver Multi-Factor authentication (MFA). Under hello 5:1 licensiering regeln har hello kunden minst 12 Azure AD Premium P1 licenser toocover alla 60 B2B-samarbete användare. Eftersom de redan har 10 Premium P1-licenser för sina 10 anställda har de rättigheter tooinvite 50 B2B användare med Premium P1-funktioner som MFA. I det här exemplet sedan måste de köpa 2 ytterligare licenser i Premium P1 toocover hello återstående 10 B2B-samarbete användare.

> [!NOTE]
> Det går inte ännu tooassign licenser direkt toohello B2B användare tooenable användarrättigheter dessa B2B-samarbete.

hello kunden som äger hello bjuda in klient måste vara hello en toodetermine hur många B2B-samarbete användare behöver betald funktionerna i Azure AD. Du måste ha tillräckligt med Azure AD betald licenser toocover B2B-samarbete användare i hello 5:1-förhållande beroende på vilket betald Azure AD-funktioner som du vill använda för din gästanvändare. 

## <a name="additional-licensing-details"></a>Ytterligare information om licensiering
- Det behövs ingen tooactually tilldela licenser tooB2B användarkonton. Baserat på hello 5:1-förhållande, licensiering automatiskt beräknas och rapporteras.
- Om ingen betald Azure AD-licens finns i varje inbjudna hämtar hello användarrättigheter som hello Azure AD-lediga-hello klient edition erbjudanden.
- Om en B2B collaboration användare redan har en betald Azure AD licens från organisationen, upp de inte en hello B2B-samarbete licenser av hello bjuda in innehavaren.

## <a name="advanced-discussion-what-are-hello-licensing-considerations-when-we-add-users-from-a-conglomerate-organization-as-members-using-your-apis"></a>Avancerade diskussion: Vad är hello licensiering överväganden när vi lägger till användare från en konglomerat organisation som ”medlem” med din API: er?
Gästanvändare B2B är ett som är inbjuden från en partner organisation toowork med hello värden organisation. Normalt annan inte uppfyller kraven som B2B även den använder B2B-funktioner. Nu ska vi titta på två fall särskilt:

1. Om en värd inbjuder en medarbetare genom att använda en konsument
  * Det här scenariot är inte kompatibel med vår licensiering principer och rekommenderas inte.

2. Om en organisation värden lägger till en användare från en annan konglomerat organisation
  1. I det här fallet hello användaren uppmanas med B2B-API: er, men det här fallet är inte traditionellt B2B. Vi rekommenderar vi ska ha dessa organisationer inbjudan hello andra användare i organisationer som medlemmar (vårt API tillåter som). I det här fallet har licenser toobe tilldelade toothese medlemmar för dem tooaccess resurser i hello bjuda in organisation.

  2. Vissa företag vill kanske tooadd hello andra org användare toobe läggas till som ”gäst” som en princip. Det finns två fall här:
      * hello konglomerat företag redan använder Azure AD och hello inbjudna användare licensieras i hello andra organisationen: i detta fall kan vi inte förväntar dig inbjudna användare tooneed toofollow hello 1:5 formeln som anges tidigare i det här dokumentet. 

      * hello konglomerat organisation inte använder Azure AD eller inte har rätt licenser: I det här fallet följer hello 1:5 formeln som anges tidigare i det här dokumentet.

## <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:

* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?](active-directory-b2b-admin-add-users.md)
* [Hur lägger informationsarbetare till B2B-samarbete användare?](active-directory-b2b-iw-add-users.md)
* [hello-element i hello e-postinbjudan för B2B-samarbete](active-directory-b2b-invitation-email.md)
* [B2B-samarbete inbjudan inlösning](active-directory-b2b-redemption-experience.md)
* [Felsökning av Azure Active Directory B2B-samarbete](active-directory-b2b-troubleshooting.md)
* [Vanliga och frågor svar om Azure Active Directory B2B-samarbete](active-directory-b2b-faq.md)
* [Azure Active Directory B2B-samarbete API och anpassning](active-directory-b2b-api.md)
* [Multi-Factor Authentication för användare av B2B-samarbete](active-directory-b2b-mfa-instructions.md)
* [Lägg till B2B-samarbete användare utan inbjudan](active-directory-b2b-add-user-without-invite.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
