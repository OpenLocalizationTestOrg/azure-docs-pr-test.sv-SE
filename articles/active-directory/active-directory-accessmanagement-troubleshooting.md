---
title: "Felsöka dynamiskt medlemskap för grupper | Microsoft Docs"
description: "Felsökningstips för dynamiskt medlemskap för grupper i Azure AD."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 89bb04b6-a379-49c2-8465-fe386641816a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: 32947e8cc69c9a48d9a285bf0a37ab3398571f86
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Felsöka dynamiska medlemskap för grupper
**Jag har konfigurerat en regel för en grupp men inget medlemskap uppdateras i gruppen**<br/>Kontrollera att den **aktivera delegerad grupphantering** inställningen **Ja** i den **konfigurera** fliken. Den här inställningen visas bara om du är inloggad som en användare som tilldelats en Azure Active Directory Premium-licens. Kontrollera värdena för användarattribut på regeln: det finns användare som uppfyller regeln?

**Jag har konfigurerat en regel, men nu befintliga medlemmar i regeln tas bort**<br/>Detta är förväntat. Befintliga medlemmar i gruppen tas bort när en regel är aktiverad eller har ändrats. Användare som har returnerats från tillämpningen av regeln läggs till som medlemmar i gruppen.     

**Jag ser medlemskap ändras direkt när jag lägger till eller ändra en regel, varför inte?**<br/>Dedikerad medlemskapsutvärdering görs med jämna mellanrum i en asynkron bakgrunden. Hur lång tid tar processen bestäms av antalet användare i din katalog och storleken på gruppen som skapades på grund av regeln. Normalt ser med mindre antal användare ändringarna av gruppmedlemskap mindre än några minuter. Med ett stort antal användare kan ta 30 minuter eller längre att fylla i.

### <a name="next-steps"></a>Nästa steg
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Hantera åtkomst till resurser med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Vad är Azure Active Directory?](active-directory-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
