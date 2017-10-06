---
title: "aaaAdd B2B-samarbete användare tooAzure Active Directory utan inbjudan | Microsoft Docs"
description: "Du kan låta en gästanvändare lägga till andra gäst användare tooyour Azure AD utan löser en inbjudan i Azure Active Directory B2B-samarbete."
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
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: 459d99b9f856a35973d1b2cbfabdc23fe40c8f44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a>Lägg till B2B-samarbete gästanvändare utan inbjudan

Du kan tillåta en användare, till exempel en partner representativt, tooadd användare från hello tooyour partnerorganisation utan att behöva inbjudningar toobe lösts in. Alla måste du göra är att bevilja användaren uppräkningen i hello-katalog som du använder för hello partner organisationen. 

Bevilja dessa behörighet när:

1. En användare i hello värden organisation (till exempel WoodGrove) inbjuder en användare från partnerorganisationen hello (till exempel Sam@litware.com) som gäst.
2. Hej administratör i hello värden organisation konfigurerar principer som gör att Sam tooidentify och lägga till andra användare från partnerorganisationen hello (Litware).
3. Nu Sam kan lägga till andra användare från Litware toohello WoodGrove katalog, grupper eller program utan att behöva inbjudningar toobe lösts in. Om Sam har hello lämpliga uppräkningen i Litware, sker det automatiskt.

### <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:

* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?](active-directory-b2b-admin-add-users.md)
* [Hur lägger informationsarbetare till B2B-samarbete användare?](active-directory-b2b-iw-add-users.md)
* [hello-element i hello e-postinbjudan för B2B-samarbete](active-directory-b2b-invitation-email.md)
* [B2B-samarbete inbjudan inlösning](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B-samarbete och licensiering](active-directory-b2b-licensing.md)
* [Felsökning av Azure Active Directory B2B-samarbete](active-directory-b2b-troubleshooting.md)
* [Vanliga och frågor svar om Azure Active Directory B2B-samarbete](active-directory-b2b-faq.md)
* [Azure Active Directory B2B-samarbete API och anpassning](active-directory-b2b-api.md)
* [Multi-Factor Authentication för användare av B2B-samarbete](active-directory-b2b-mfa-instructions.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)