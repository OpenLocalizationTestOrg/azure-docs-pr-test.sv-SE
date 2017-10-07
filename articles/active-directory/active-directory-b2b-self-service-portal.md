---
title: "registreringsportalen för aaaSelf-tjänsten för Azure Active Directory B2B-samarbete | Microsoft Docs"
description: "Azure Active Directory B2B-samarbete stöder företagsomfattande relationer genom att aktivera business partners tooselectively åtkomst till företagets program"
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
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: c78920ecf812f7efc06a8b54b1fff834c32904f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Självbetjäningsportalen för Azure AD B2B-samarbete registrering

Kunder kan göra mycket med hello inbyggda funktioner som är tillgängliga via vår IT-administratören [Azure-portalen](https://portal.azure.com) och vår [programmet åtkomstpanelen](https://myapps.microsoft.com) för slutanvändare. Men det finns också företag måste toocustomize hello onboarding arbetsflöde för B2B användare toofit organisationens behov. De kan göra det med [vårt API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

I diskussioner med våra kunder finns en gemensam måste öka upp över alla andra. hello bjuda in organisation kanske inte vet i förväg som hello enskilda externa samarbetspartners är som behöver åtkomst till tootheir resurser. De vill ha ett sätt för användare från partnerföretag för registrera sig själva med en uppsättning principer som hello bjuda in organisation kontroller. Det här scenariot är möjligt via våra API: er, så vi har publicerat ett projekt på Github som har just: [Github exempelprojektet](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

Vår Github-projektet visar hur organisationer kan använda våra API: er och funktioner för en principbaserad, självbetjäning registrering för deras betrodda partners med regler som bestämmer hello-appar som de kan komma åt. Partner användare får åtkomst till tooresources när de behöver dem, på ett säkert sätt, utan hello bjuda in organisation toomanually publicera dem. Du kan enkelt distribuera hello projekt till en Azure-prenumeration önskat.

## <a name="as-is-code"></a>Som-kod

Kom ihåg att den här koden görs tillgänglig som ett exempel toodemonstrate hello Azure Active Directory B2B-inbjudan API. Det bör anpassas efter dev-team eller en partner och bör granskas innan de kan distribueras i ett produktionsscenario.

## <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:
* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?](active-directory-b2b-admin-add-users.md)
* [Hur lägger informationsarbetare till B2B-samarbete användare?](active-directory-b2b-iw-add-users.md)
* [hello-element i hello e-postinbjudan för B2B-samarbete](active-directory-b2b-invitation-email.md)
* [B2B-samarbete inbjudan inlösning](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B-samarbete och licensiering](active-directory-b2b-licensing.md)
* [Felsökning av Azure Active Directory B2B-samarbete](active-directory-b2b-troubleshooting.md)
* [Vanliga och frågor svar om Azure Active Directory B2B-samarbete](active-directory-b2b-faq.md)
* [Multi-Factor Authentication för användare av B2B-samarbete](active-directory-b2b-mfa-instructions.md)
* [Lägg till B2B-samarbete användare utan inbjudan](active-directory-b2b-add-user-without-invite.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)