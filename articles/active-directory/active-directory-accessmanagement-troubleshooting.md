---
title: "aaaTroubleshooting dynamiskt medlemskap för grupper | Microsoft Docs"
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
ms.openlocfilehash: d792fc406288844e2c5dbe3702c2c9870d09473e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Felsöka dynamiska medlemskap för grupper
**Jag har konfigurerat en regel för en grupp, men inget medlemskap uppdateras i hello grupp**<br/>Kontrollera att hello **aktivera delegerad grupphantering** inställningen för**Ja** i hello **konfigurera** fliken. Den här inställningen visas bara om du är inloggad som en användare toowhom en Azure Active Directory Premium-licens har tilldelats. Kontrollera hello värden för användarattribut på hello regeln: det finns användare som uppfyller hello regeln?

**Jag har konfigurerat en regel, men nu hello befintliga medlemmar i hello regeln tas bort**<br/>Detta är förväntat. Befintliga medlemmarna i hello tas bort när en regel är aktiverad eller har ändrats. hello-användare som har returnerats från utvärdering av hello regeln läggs till som medlemmar toohello grupp.     

**Jag ser medlemskap ändras direkt när jag lägger till eller ändra en regel, varför inte?**<br/>Dedikerad medlemskapsutvärdering görs med jämna mellanrum i en asynkron bakgrunden. Hur länge hello processen tar bestäms av hello antalet användare i din katalog och hello storleken på hello-gruppen som skapades på grund av hello regeln. Normalt ser med mindre antal användare hello ändringar i gruppmedlemskap mindre än några minuter. Med ett stort antal användare kan ta 30 minuter eller längre toopopulate.

### <a name="next-steps"></a>Nästa steg
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Hantera åtkomst tooresources med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Vad är Azure Active Directory?](active-directory-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
