---
title: "aaaB2B samarbete användaranspråk mappning i Azure Active Directory | Microsoft Docs"
description: "anspråk mappning referens för Azure Active Directory B2B-samarbete"
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
ms.openlocfilehash: 9e26085e91a6004b2f11286ae9c1df133bd47341
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>B2B-samarbete användaranspråk mappning i Azure Active Directory

Azure Active Directory (AD Azure) stöder anpassa hello anspråk som utfärdats i hello SAML-token för B2B-samarbete användare. Azure AD utfärdar en SAML-token toohello app som innehåller information (eller anspråk) om hello-användaren som unikt identifierar dem när en användare autentiseras toohello program. Som standard innehåller detta hello användarens användarnamn, e-postadress, Förnamn och efternamn. Du kan visa eller redigera hello anspråk skickas i hello SAML-token toohello programmet under fliken för hello-attribut.

Det finns två möjliga orsaker till varför du kanske behöver tooedit hello anspråk som utfärdats i hello SAML-token.

1. programmet hello har skrivits toorequire en annan uppsättning anspråk URI: er eller anspråksvärden

2. Programmet kräver hello NameIdentifier anspråk toobe något annat än hello användarens huvudnamn lagras i Azure Active Directory.

  ![Visa anspråk i SAML-token](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

Information om hur tooadd och redigera anspråk checka ut denna artikel på anspråk anpassning [anpassa anspråk som utfärdats i hello SAML-token för förintegrerade appar i Azure Active Directory](develop/active-directory-saml-claims-customization.md). Användare, mappning NameID och UPN cross-klient som inte av säkerhetsskäl för B2B-samarbete.


## <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:

* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Egenskaper för användare av B2B-samarbete](active-directory-b2b-user-properties.md)
* [Lägga till en B2B-samarbete tooa användarroll](active-directory-b2b-add-guest-to-role.md)
* [Delegera B2bB samarbete inbjudningar](active-directory-b2b-delegate-invitations.md)
* [Dynamiska grupper och B2B-samarbete](active-directory-b2b-dynamic-groups.md)
* [B2B-samarbete kod och PowerShell-exempel](active-directory-b2b-code-samples.md)
* [Konfigurera SaaS-appar för B2B-samarbete](active-directory-b2b-configure-saas-apps.md)
* [Extern delning av Office 365](active-directory-b2b-o365-external-user.md)
* [Användartoken för B2B-samarbete](active-directory-b2b-user-token.md)
* [Aktuella begränsningar för B2B-samarbete](active-directory-b2b-current-limitations.md)
