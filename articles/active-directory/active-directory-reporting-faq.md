---
title: "aaaAzure Active Directory Reporting vanliga frågor och svar | Microsoft Docs"
description: "Azure Active Directory reporting vanliga frågor och svar."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: be65a05574ea3b5b190cd02a96b211c571ba70bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-faq"></a>Azure Active Directory reporting vanliga frågor och svar

Den här artikeln innehåller svar toofrequently frågor (FAQ) och svar om Azure Active Directory reporting.  
Mer information finns i [Azure Active Directory reporting](active-directory-reporting-azure-portal.md). 

**F: Vad är hello datalagring för aktivitetsloggar (granskning och inloggningar) hello Azure-portalen?** 

**S:** vi erbjuder 7 dagars data för kunden ledigt och genom att växla tooan Azure AD Premium 1 eller 2 för Premium-licens, du kan komma åt data för in too30 dagar. Mer information om kvarhållning finns [rapportkvarhållningsregler i Azure Active Directory](active-directory-reporting-retention.md)

--- 

**F: hur lång tid tar det tills hello aktivitetsdata visas när jag har slutfört min uppgiften?**

**S:** granskningsloggarna för aktivitet har en fördröjning mellan 15 minuter tooan timme. Inloggningsaktivitet loggar har en fördröjning mellan 15 minuter för de flesta poster och uppåt too2 timmar innan några poster.

---

**F: måste toobe en global administratör toosee hello aktivitet loggar hello Azure-portalen eller tooget data via hello API?**

**S:** Nej. Du kan antingen vara en **säkerhet Reader**, **säkerhet Admin** eller en **Global administratör** toosee rapporterar data i Azure-portalen eller genom att öppna den via hello API.

---

**F: kan jag få logginformation för Office 365 aktiviteten via hello Azure-portalen?**

**S:** även om aktiviteten för Office 365 och Azure AD aktivitet loggar resursen mycket hello directory resurser om du vill att en fullständig överblick över Hej aktivitetsloggar för Office 365, bör du gå toohello logginformation för Office 365 Admin Center tooget Office 365-aktivitet.

---


**F: som API: er ska jag använda tooget information om Office 365 aktivitetsloggar?**

**S:** Använd hello Office 365 Management API: er tooaccess hello [Office 365 aktivitet loggar via ett API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).

---

**F: hur många poster som jag kan hämta från Azure-portalen?**

**S:** kan du hämta too120K poster från hello Azure-portalen. hello-poster sorteras efter *senaste* och som standard får hello senaste 120 K poster. 

---

**F: hur många poster kan fråga via hello aktiviteter API?**

**S:** du kan fråga efter too1 miljoner poster (om du inte använder hello operatorn top, som sorterar hello post efter de senaste). Om du använder hello ”top” operator, kan du fråga efter too500K poster. Du kan hitta exempelfrågor på hur toouse hello API här [här](active-directory-reporting-api-getting-started.md).

---

**F: hur får jag en premium-licens?**

**S:** finns [komma igång med Azure Active Directory Premium](active-directory-get-started-premium.md) för en fråga toothis.

---

**F: hur snart bör se aktiviteter data när en premium-licens?**

**S:** om du redan har data som aktiviteter som en kostnadsfri licens, så du kan se hello samma data. Om du inte har några data, tar en eller två dagar.

---

**F: kan jag se senaste månaden data när en Azure AD premium-licens?**

**S:** om du har nyligen har bytt tooa Premium version (inklusive en utvärderingsversion), kan du se data in too7 dagar från början. När data ackumuleras visas in too30 dagar.

---

**F: det finns en risk händelse i Identity Protection men jag ser inte motsvarande inloggning i hello alla inloggningar. Förväntas detta?**

**S:** Ja, identitetsskydd utvärderar risk för alla flöden för autentisering om om vara interaktiva eller icke-interaktivt. Alla inloggningar endast rapporten innehåller dock endast hello interaktiva inloggningar.

---

**F: hur kan jag hämta hello ”användare som flaggats för risk” rapporten i Azure-portalen?**

**S:** hello alternativet toodownload *användare som har flaggats för risk* rapporten läggs snart.

---

**F: hur vet jag varför en inloggning eller en användare som har flaggats riskfyllda i hello Azure-portalen?**

**S:** Premium edition kunder kan läsa mer om hello underliggande riskhändelser genom att klicka på hello användare i ”användare som flaggats för risk” eller genom att klicka på hello ”riskfyllda inloggningar”. Ledigt och grundläggande edition kunder få toosee hello vilka användare och inloggningar utan hello underliggande händelseinformation för risk.

---

**F: hur beräknas IP-adresser i hello inloggningar och riskfyllda inloggningar rapporten?**

**S:** IP-adresser utfärdas så att det finns ingen slutgiltiga anslutning mellan en IP-adress och där hello datorn med adressen finns fysiskt. Detta är komplicerade av faktorer, till exempel mobila providers och VPN-anslutningar utfärda IP-adresser från central pooler ofta mycket långt där hello klientenheten verkligen används. Hello ovan är, konvertera IP-adress tooa fysisk plats en bästa prestanda baserat på spårningar, registerdata, omvänd sökningar och annan information. 

---
