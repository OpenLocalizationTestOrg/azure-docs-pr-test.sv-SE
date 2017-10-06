---
title: "aaaDelegate inbjudningar för Azure Active Directory B2B-samarbete | Microsoft Docs"
description: "Azure Active Directory B2B-samarbete användaregenskaper kan konfigureras"
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
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: c0122d6f60d494c6e251c41d947dc254ea887620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Delegera inbjudningar för Azure Active Directory B2B-samarbete

Med Azure Active Directory (AD Azure) business-to-business (B2B) samarbete har du inte toobe en global administratör toosend inbjudningar. Du kan i stället använda principer och delegera inbjudningar toousers vars roller att de toosend inbjudningar. Ett viktigt nytt sätt toodelegate gästen användaren inbjudningar är via hello gäst bjuder in roll.

## <a name="guest-inviter-role"></a>Gästen bjuder in roll
Vi kan tilldela hello användaren tooGuest bjuder in rollen toosend inbjudningar. Du har inte toobe tillhör hello global administratör rollen toosend inbjudningar. Som standard anropa vanliga användare hello inbjudan API en global administratör inaktiverat inbjudningar för vanliga användare. En användare kan även anropa hello API: et med hello Azure-portalen eller PowerShell.

Här är ett exempel som visar hur toouse PowerShell tooadd en användarroll toohello gäst bjuder in:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Kontrollera vem som kan bjuda in

![Kontrollera hur tooinvite](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

En klientadministratör kan ange hello efter inbjudan principer med Azure AD B2B-samarbete:

- Inaktivera inbjudningar
- Bara administratörer och användare i hello gäst bjuder in roll kan bjuda in
- Administratörer, hello gäst bjuder in roll och medlemmar kan bjuda in
- Alla användare, inklusive gäster, kan bjuda in

Klienter är som standard för #4. (Alla användare, inklusive gäster, erbjuda B2B-användare.)

## <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:

* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Egenskaper för användare av B2B-samarbete](active-directory-b2b-user-properties.md)
* [Lägga till en B2B-samarbete tooa användarroll](active-directory-b2b-add-guest-to-role.md)
* [Dynamiska grupper och B2B-samarbete](active-directory-b2b-dynamic-groups.md)
* [B2B-samarbete kod och PowerShell-exempel](active-directory-b2b-code-samples.md)
* [Konfigurera SaaS-appar för B2B-samarbete](active-directory-b2b-configure-saas-apps.md)
* [Användartoken för B2B-samarbete](active-directory-b2b-user-token.md)
* [B2B-samarbete användaranspråk mappning](active-directory-b2b-claims-mapping.md)
* [Extern delning av Office 365](active-directory-b2b-o365-external-user.md)
* [Aktuella begränsningar för B2B-samarbete](active-directory-b2b-current-limitations.md)
