---
title: "aaaThe Azure AD Privileged Identity Management-säkerhetsguiden"
description: "hello visas första gången du använder hello Azure Active Directory Privileged Identity Management-tillägg, med en säkerhetsguiden. Den här artikeln beskriver hello steg för att använda hello guiden."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: a53a3719-8cc7-4fc7-8164-aafca192871b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 0b3019134d3c7cfac33b3acfcf430b4d4f67b119
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-security-wizard-in-azure-ad-privileged-identity-management"></a>Med hjälp av guiden för hello säkerhet i Azure AD Privileged Identity Management 
Om du är hello första person toorun Azure Privileged Identity Management (PIM) för din organisation, visas en guide. hello-guiden hjälper dig att förstå hello säkerhetsriskerna med Privilegierade identiteter och hur toouse PIM tooreduce riskerna. Du behöver inte toomake alla ändringar tooexisting rolltilldelningar hello i guiden om du föredrar toodo senare.

## <a name="what-tooexpect"></a>Vilka tooexpect
Innan din organisation kan börja använda PIM, alla rolltilldelningar är permanenta: hello användare är alltid i dessa roller även om de inte för närvarande behöver sina privilegier.  hello första steget hello guiden visar en lista över privilegierad roller och hur många användare som för närvarande rollerna. Du kan gå i tooa viss roll toolearn mer information om användare om en eller flera av dem är okänd.

hello andra steget hello guiden ger dig en möjlighet toochange administratör rolltilldelningar.  

> [!WARNING]
> Det är viktigt att du har minst en global administratör och mer än en administratör av Privilegierade roller med ett organisationskonto (inte ett Microsoft-konto). Om det finns bara en administratör av Privilegierade roller, tas inte hello organisation kan toomanage PIM om att kontot har tagits bort.
> Kom också rolltilldelningar permanent om en användare har ett Microsoft-konto (ett konto som de använder toosign i tooMicrosoft tjänster som Skype och Outlook.com). Om du planerar toorequire MFA för aktivering för rollen, kommer den användaren utelåst.
> 
> 

När du har gjort ändringar hello guiden kommer inte längre att visas. hello visas nästa gång du eller en annan administratör av Privilegierade roller använder PIM, hello PIM-instrumentpanelen.  

* Om du skulle t.ex. tooadd eller ta bort användare från roller eller ändra tilldelningar från permanent tooeligible, kan du läsa mer i [hur tooadd eller ta bort en användarroll](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
* Om du vill att toogive fler användare komma åt toomanage PIM, Läs mer på [hur toogive åt toomanage i PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

