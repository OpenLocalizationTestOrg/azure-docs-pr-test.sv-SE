---
title: "Översikt för återställning av lösenord för självbetjäning av Azure AD | Microsoft Docs"
description: "Vad gör Självbetjäning för återställning av lösenord i Azure AD för din organisation?"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 3903c53b78e61b380bb812a9d3ade694655b5afb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>Azure AD lösenordsåterställning via Självbetjäning för IT-proffs

”Tjänsten” är en buzzword utlösts runt inom många IT-avdelningar över hela världen med olika betydelser. Marknaden översvämmas med produkter som du kan hantera lokala grupper, lösenord eller användarprofiler från molnet eller lokalt.

Azure Active Directory (AD Azure) lösenordsåterställning via självbetjäning (SSPR) anger sig själv från varandra med enkel användning och distribution. Lösenord för självbetjäning av Azure AD Återställ kombinerar en uppsättning funktioner som:

* Tillåt att användarna kan hantera sina egna lösenord
  * Från valfri enhet
  * Var som helst
  * När som helst
* Upprätthålla överensstämmelse med principer som definierar du som administratör

Om du är klar kan du komma igång med Azure AD SSPR med hjälp av vår [Snabbstartsguide vägledning](active-directory-passwords-getting-started.md) och snabbt har användarna återställa sina lösenord.

## <a name="what-is-possible"></a>Vad du kan göra

* **Självbetjäning lösenordsändring** att slutanvändare och administratörer kan ändra sina lösenord utan hjälp av administratören
* **Självbetjäning kontoupplåsning** kan användarna att låsa upp sitt eget konto utan hjälp av administratören
* **Självbetjäning för lösenordsåterställning** kan slutanvändare och administratörer kan återställa sina lösenord automatiskt utan hjälp av administratören. Lösenordsåterställning via självbetjäning kräver Azure AD Premium eller Basic - [Azure Active Directory-versioner](active-directory-editions.md).
* **Administratörsstartade lösenordsåterställning** kan en administratör att återställa lösenord för en slutanvändare eller en annan administratör inifrån den [Azure-portalen](https://docs.microsoft.com/azure/azure-portal-overview)
* **Lösenord management aktivitetsrapporter** ger administratörer insikter om aktiviteten för återställning och registrering av lösenord pågår i deras organisation - [rapporter](active-directory-passwords-reporting.md)
* **Tillbakaskrivning av lösenord** låter dig hantera lokala lösenord från molnet så att alla scenarier som föregår kan utföras av eller på uppdrag av, federerad eller lösenord synkroniserade användare. Tillbakaskrivning av lösenord kräver [Azure AD Premium](active-directory-get-started-premium.md).

## <a name="why-choose-azure-ad-self-service-password-reset"></a>Varför ska jag välja Azure AD Självbetjäning för lösenordsåterställning

* **Minska kostnaderna** som supportavdelning och support assisterad lösenordsåterställning är vanligtvis tillbringar 20% av en IT-organisation
* **Förbättra upplevelse för slutanvändare** och **minska supportavdelningen volym** genom att ge slutanvändare behörighet att lösa lösenordsproblem med sina egna på en gång utan anropar en supportavdelning eller öppna en supportbegäran.
* **Enheten mobility** som användare kan återställa sina lösenord från var de än är.

## <a name="azure-ad-self-service-password-reset-availability"></a>Tillgänglighet för återställning av lösenord för självbetjäning av Azure AD

Azure AD självbetjäning lösenordet för återställning finns i tre nivåer beroende på din prenumeration.

* **Azure AD-lediga** – endast molnbaserad administratörer kan återställa sina lösenord
* **Azure AD Basic** eller **betald prenumeration på Office 365** – endast molnbaserad användare och endast molnbaserad administratörer kan återställa sina lösenord
* **Azure AD Premium** – alla användare eller administratör, inklusive endast molnbaserad, federerad eller lösenord synkroniseras användare, kan återställa sina lösenord. Lokala lösenord kräver tillbakaskrivning av lösenord som ska aktiveras.

## <a name="azure-ad-self-service-password-reset-a-sum-of-the-parts"></a>Azure AD Självbetjäning för återställning av lösenord, summan av delar

Självbetjäning för återställning av lösenord i Azure AD består av följande komponenter:

* **Lösenord för hanteringsportalen configuration** där du kan kontrollera alternativ för hur lösenorden hanteras i din klient via Azure portal
* **Portalen för registrering för lösenordsåterställning** där användare kan registrera för återställning av lösenord
* **Portal för återställning av lösenord** där användare kan återställa sina lösenord med hjälp av de utmaningar som definieras av administratören och svar användare har angett
* **Portalen för ändring av lösenord** där användare kan ändra sina egna lösenord genom att ange sina gamla lösenord och ett nytt lösenord
* **Rapporter för lösenord** där administratörer kan visa och analysera lösenord aktivitet rapporter för sina klienter i Azure-portalen
* **Tillbakaskrivning av lösenord till lokalt med Azure AD Connect** kan du aktivera hantering av lokal federerad, eller lösenord synkroniserade användare från molnet

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Azure AD prissättning, SLA, uppdateringar och -översikt

Mer information om de här ämnena finns på följande sidor

* [**Prisinformation för Azure AD**](https://azure.microsoft.com/pricing/details/active-directory/)
* [**Priser för Office 365**](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [**Azure servicenivåavtal**](https://azure.microsoft.com/support/legal/sla/)
* [**Serviceavtal för Microsoft Online Services**](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [**Azure-uppdateringar**](https://azure.microsoft.com/updates/)
* [**Azure-översikt**](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Nästa steg

Följande länkar ger ytterligare information om lösenordsåterställning med Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data**](active-directory-passwords-data.md) – Förstå de data som krävs och hur de används för lösenordshantering
* [**Distribution**](active-directory-passwords-best-practices.md) – Planera och distribuera SSPR till dina användare med hjälp av informationen finns här
* [**Anpassa**](active-directory-passwords-customize.md) – Anpassa utseendet för företagets SSPR-funktion.
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Teknisk djupdykning** ](active-directory-passwords-how-it-works.md) – Ta en titt bakom kulisserna för att förstå hur det hela fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? – Svar på allt du någonsin velat fråga
* [**Felsökning** ](active-directory-passwords-troubleshoot.md) – Lär dig att lösa vanliga problem med SSPR

