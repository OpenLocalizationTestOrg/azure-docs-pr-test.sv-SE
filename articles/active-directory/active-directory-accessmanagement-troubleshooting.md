---
title: "Felsöka dynamiskt medlemskap för grupper | Microsoft Docs"
description: "Felsökningstips för dynamiskt medlemskap för grupper i Azure AD."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 89bb04b6-a379-49c2-8465-fe386641816a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro
ms.openlocfilehash: 0bb4c294cc6a4e1c9c2f1ad405c539854b6bcf5b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Felsöka dynamiska medlemskap för grupper
**Jag har konfigurerat en regel för en grupp men inget medlemskap uppdateras i gruppen**<br/>Kontrollera värdena för användarattribut på regeln: det finns användare som uppfyller regeln? Om allt ser bra ut kan ta lite tid att fylla i gruppen. Beroende på klientens storlek så kan det ta upp till 24 timmar att fylla gruppen första gången eller efter en regeländring.

**Jag har konfigurerat en regel, men nu befintliga medlemmar i regeln tas bort**<br/>Detta är förväntat. Befintliga medlemmar i gruppen tas bort när en regel är aktiverad eller har ändrats. Användare som har returnerats från tillämpningen av regeln läggs till som medlemmar i gruppen.     

**Jag ser medlemskap ändras direkt när jag lägger till eller ändra en regel, varför inte?**<br/>Dedikerad medlemskapsutvärdering görs med jämna mellanrum i en asynkron bakgrunden. Hur lång tid tar processen bestäms av antalet användare i din katalog och storleken på gruppen som skapades på grund av regeln. Normalt ser med mindre antal användare ändringarna av gruppmedlemskap mindre än några minuter. Med ett stort antal användare kan ta 30 minuter eller längre att fylla i.

### <a name="next-steps"></a>Nästa steg
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Hantera åtkomst till resurser med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Vad är Azure Active Directory?](active-directory-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
