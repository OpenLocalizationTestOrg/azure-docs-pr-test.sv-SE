---
title: aaaAccount etablering meddelanden | Microsoft Docs
description: "Lär dig hur tooensure att meddelas du om problem relaterade toouser etablering som kräver din uppmärksamhet genom att aktivera etablering av meddelanden."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a637aac7-f06b-48ef-a66d-639835a8edec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: e33d1dd806fff43fc96f843a9dcddd7375d2e3c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="account-provisioning-notifications"></a>Kontoetableringsmeddelanden
Med användaretablering, kan du automatisera hello process för att hantera användare i SaaS-program från tredje part. <br>
Även om detta är en automatiserad process är interaktionen med den här processen från tid tootime krävs. <br>
Detta gäller, till exempel hello, när hello lösenord för hello-konto som du har konfigurerat tooexchange data med tredje part SaaS-program har upphört att gälla. 

Genom att aktivera konto etablering meddelanden kan du se till att du meddelas om problem relaterade toouser etablering som kräver din uppmärksamhet.

Du aktiverar eller inaktiverar kontot etablering meddelanden som en del av din konfiguration för tredje part SaaS-program för användaretablering.

![Användaretablering][1] 

tooactivate konto etablering meddelanden, Välj hello relaterade kryssruta på hello **bekräftelse** dialogrutan sidan och typen hello e-alias för hello mottagaren.

![Kontoetableringsmeddelanden][2]

Du kan ange en distributionslista som mottagaren. Det är dock viktigt toonote att hello e-postmeddelandet innehåller länkar tooreports som endast är tillgängliga för administratörer hello Azure AD.

Om du har konto etablering meddelanden aktiverad, får e-postmeddelanden om kritiska problem som är relaterade toouser etablering. Dock tooavoid en e-överlagring endast får du ett e-postmeddelande per dag för varje SaaS programmet hello e-postmeddelandet har aktiverats för.

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Automatisera användaren etablering/avetablering tooSaaS appar](active-directory-saas-app-provisioning.md)
* [Anpassa attributmappning för Användaretablering](active-directory-saas-customizing-attribute-mappings.md)
* [Skriva uttryck för attributmappning](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Omfångsfilter för Användaretablering](active-directory-saas-scoping-filters.md)
* [Använda SCIM tooenable Automatisk etablering av användare och grupper från Azure Active Directory tooapplications](active-directory-scim-provisioning.md)
* [Lista över självstudier om hur tooIntegrate SaaS-appar](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png
