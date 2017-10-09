---
title: "aaaProperties för en Azure Active Directory B2B-samarbete användare | Microsoft Docs"
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
ms.date: 05/25/2017
ms.author: sasubram
ms.openlocfilehash: 78709f64430ed4c14eadf4dc257f175c24698c5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Egenskaper för en användare för Azure Active Directory B2B-samarbete

En Azure Active Directory (AD Azure) business-to-business (B2B) samarbete användare är en användare med UserType = gäst. Gästanvändaren normalt är från en organisation och har begränsad behörighet i hello bjuda in directory, som standard.

Beroende på hello bjuda in organisations behov, kan en Azure AD B2B-samarbete användare ha något av följande konto tillstånd hello:

- Tillstånd 1 ”: Multihomed” i en extern instans av Azure AD och representeras som gästanvändare i hello bjuda in organisation. I det här fallet loggar hello B2B användare in med ett konto i Azure AD som tillhör toohello inbjuden klient. Om hello resurspartnerns organisation inte använder Azure AD kan skapas fortfarande hello gästanvändaren i Azure AD. hello krav är att de lösa sina inbjudan och Azure AD verifierar sin e-postadress. Den här ordningen kallas även en just-in-time (JIT) innehavare eller ”viral” innehavare.

- Tillstånd 2 ”: Multihomed” i ett Microsoft-konto och representeras som gästanvändare i hello värden organisation. I det här fallet loggar hello gästanvändaren in med ett Microsoft-konto. hello inbjuden sociala användaridentitet (google.com eller liknande), som inte är ett Microsoft-konto skapas som ett Microsoft-konto under erbjudande åtgärd.

- Tillstånd 3: Homed i hello värden organisationens lokala Active Directory och synkroniserats med hello värden organisationens Azure AD. Under den här versionen, måste du använda PowerShell toomanually ändra hello UserType för dessa användare i hello molnet.

- Tillstånd 4: Homed i värden organisationens Azure AD med UserType = Gäst och autentiseringsuppgifter som hello värden organisation hanterar.

  ![Visa hello bjuder in initialer](media/active-directory-b2b-user-properties/redemption-diagram.png)


Nu ska vi se hur en användare med Azure AD B2B-samarbete i läge 1 som ser ut i Azure AD.

### <a name="before-invitation-redemption"></a>Innan du inbjudan inlösning

![Innan erbjudandet inlösning](media/active-directory-b2b-user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>Efter inbjudan inlösning

![När erbjudandet inlösning](media/active-directory-b2b-user-properties/after-redemption.png)

## <a name="key-properties-of-hello-azure-ad-b2b-collaboration-user"></a>Nyckelegenskaperna för hello Azure AD B2B-samarbete användare
### <a name="usertype"></a>UserType
Den här egenskapen anger hello relationen mellan hello användaren toohello värden innehavare. Den här egenskapen kan ha två värden:
- Medlem: Det här värdet anger en medarbetare hello värden organisation och en användare i hello organisation löneuppgifter. Den här användaren förväntar exempelvis toohave åtkomst endast toointernal-platser. Den här användaren kan inte betraktas som en extern deltagare.

- Gäst: Det här värdet anger en användare som inte anses vara interna toohello företaget, till exempel en extern deltagare, partner, kund eller liknande användare. Användaren skulle vara förväntade tooreceive en CEO interna PM eller ta emot fördelar för företaget, till exempel.

  > [!NOTE]
  > hello UserType har ingen relation toohow hello användaren loggar in, hello directory roll hello användar- och så vidare. Den här egenskapen anger hello användarens relation toohello värden organisation bara och tillåter hello organisation tooenforce principer som är beroende av den här egenskapen.

### <a name="source"></a>Källa
Den här egenskapen anger hur hello användaren loggar in.

- Inbjudna användare: Den här användaren har bjudits in men har ännu inte Inlöst en inbjudan.

- Externa Active Directory: Den här användaren homed i en extern organisation och autentiserar med hjälp av en Azure AD-konto som tillhör toohello andra organisationen. Den här typen av inloggning motsvarar tooState 1.

- Microsoft-konto: den här användaren homed i ett Microsoft-konto och autentiserar med hjälp av ett Microsoft-konto. Den här typen av inloggning motsvarar tooState 2.

- Windows Server Active Directory: Den här användaren har loggat in från lokala Active Directory som tillhör toothis organisation. Den här typen av inloggning motsvarar tooState 3.

- Azure Active Directory: Autentiserar användaren med en Azure AD-konto som tillhör toothis organisation. Den här typen av inloggning motsvarar tooState 4.
  > [!NOTE]
  > Käll- och UserType är oberoende egenskaper. Ett värde av datakälla innebär inte ett visst värde för UserType.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Kan Azure AD B2B-användare läggas till som medlemmar i stället för gäster?
En Azure AD B2B-användare och gästanvändare normalt synonym. Därför kan en Azure AD B2B-samarbete användare läggs till som en användare med UserType = Gäst som standard. Men i vissa fall är hello partnerorganisation medlem i en större organisation toowhich hello värden organisation också tillhör. I så fall, kanske hello värden organisation vill tootreat användare i hello partnerorganisation som medlemmar i stället för gäster. Använd hello Azure AD B2B inbjudan Manager API: erna tooadd eller be en användare från hello organisation toohello värden partnerorganisation som en medlem.

## <a name="filter-for-guest-users-in-hello-directory"></a>Filtret för gästanvändare i hello directory

![Filtrera gästanvändare](media/active-directory-b2b-user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>Konvertera UserType
För närvarande finns är det möjligt för användare tooconvert UserType från medlem tooGuest och vice versa med hjälp av PowerShell. Hello UserType-egenskapen ska toorepresent hello användarens relation toohello organisation. Därför bör hello värdet för den här egenskapen bara ändra om hello relationen mellan hello användarorganisation toohello ändras. Om hello relationen mellan hello användaren ändras bör problem, till exempel om hello användarens huvudnamn (UPN) ändras kan åtgärdas? Hello användaren fortsätta toohave åtkomst toohello samma resurser? Ska tilldelas en postlåda? Därför rekommenderar vi inte ändra hello UserType med hjälp av PowerShell som en atomisk aktivitet. Dessutom, om den här egenskapen blir inte ändras med hjälp av PowerShell, rekommenderas inte tar ett beroende på det här värdet.

## <a name="remove-guest-user-limitations"></a>Ta bort gästen användaren begränsningar
Det kan finnas fall där du vill att toogive din högre privilegier för gäst-användare. Du kan lägga till en roll tooany Gäst och även ta bort hello standardbegränsningar gästen användaren i hello directory toogive en användare hello samma privilegier som medlemmar.

Det är möjligt tooturn av hello standardbegränsningar gästen användaren så att gästanvändare i hello företagets katalog ges hello samma behörigheter som en medlemsanvändare.

![Ta bort gästen användaren begränsningar](media/active-directory-b2b-user-properties/remove-guest-limitations.png)

## <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:

* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Lägga till en B2B-samarbete tooa användarroll](active-directory-b2b-add-guest-to-role.md)
* [Delegera B2B-samarbete inbjudningar](active-directory-b2b-delegate-invitations.md)
* [B2B-samarbete användaren granskning och rapportering](active-directory-b2b-auditing-and-reporting.md)
* [Dynamiska grupper och B2B-samarbete](active-directory-b2b-dynamic-groups.md)
* [B2B-samarbete kod och PowerShell-exempel](active-directory-b2b-code-samples.md)
* [Konfigurera SaaS-appar för B2B-samarbete](active-directory-b2b-configure-saas-apps.md)
* [Användartoken för B2B-samarbete](active-directory-b2b-user-token.md)
* [B2B-samarbete användaranspråk mappning](active-directory-b2b-claims-mapping.md)
* [Extern delning av Office 365](active-directory-b2b-o365-external-user.md)
* [Aktuella begränsningar för B2B-samarbete](active-directory-b2b-current-limitations.md)
