---
title: "aaaAzure AD Självbetjäning för lösenordsåterställning översikt | Microsoft Docs"
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
ms.openlocfilehash: 0efc291b1eeec0b7ae33ff5a7d9ed38e70c8be6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-self-service-password-reset-for-hello-it-professional"></a>Azure AD lösenordsåterställning via Självbetjäning för hello IT-proffs

”Tjänsten” är en buzzword utlösts runt inom många IT-avdelningar över hello world med olika betydelser. hello marknaden översvämmas med produkter som gör att du toomanage lokala grupper, lösenord eller användarprofiler från hello molnet eller lokalt.

Azure Active Directory (AD Azure) lösenordsåterställning via självbetjäning (SSPR) anger sig själv från varandra med enkel användning och distribution. Lösenord för självbetjäning av Azure AD Återställ kombinerar en uppsättning funktioner som:

* Tillåt användare-toomanage sina egna lösenord
  * Från valfri enhet
  * Var som helst
  * När som helst
* Upprätthålla överensstämmelse med principer som definierar du som administratör

Om du är klar kan du komma igång med Azure AD SSPR med hjälp av vår [Snabbstartsguide vägledning](active-directory-passwords-getting-started.md) och snabbt har användarna återställa sina lösenord.

## <a name="what-is-possible"></a>Vad du kan göra

* **Självbetjäning lösenordsändring** tillåter användare eller administratörer toochange sina lösenord utan hjälp av administratören
* **Självbetjäning kontoupplåsning** tillåter slutet användare toounlock sina egna konton utan hjälp av administratören
* **Självbetjäning för lösenordsåterställning** tillåter användare eller administratörer tooreset sina lösenord automatiskt utan hjälp av administratören. Lösenordsåterställning via självbetjäning kräver Azure AD Premium eller Basic - [Azure Active Directory-versioner](active-directory-editions.md).
* **Administratörsstartade lösenordsåterställning** kan en administratör tooreset en slutanvändare eller en annan administratör lösenord från inom hello [Azure-portalen](https://docs.microsoft.com/azure/azure-portal-overview)
* **Lösenord management aktivitetsrapporter** ger administratörer insikter om aktiviteten för återställning och registrering av lösenord pågår i deras organisation - [rapporter](active-directory-passwords-reporting.md)
* **Tillbakaskrivning av lösenord** låter dig hantera lokala lösenord från hello molnet så att alla hello som föregår scenarier kan utföras av eller på hello vägnar, federerad eller lösenord synkroniserade användare. Tillbakaskrivning av lösenord kräver [Azure AD Premium](active-directory-get-started-premium.md).

## <a name="why-choose-azure-ad-self-service-password-reset"></a>Varför ska jag välja Azure AD Självbetjäning för lösenordsåterställning

* **Minska kostnaderna** som supportavdelning och support assisterad lösenordsåterställning är vanligtvis tillbringar 20% av en IT-organisation
* **Förbättra upplevelse för slutanvändare** och **minska supportavdelningen volym** genom att ge slutet användare hello power tooresolve lösenordsproblem med sina egna på en gång utan anropar en supportavdelning eller öppna en supportbegäran.
* **Enheten mobility** som användare kan återställa sina lösenord från var de än är.

## <a name="azure-ad-self-service-password-reset-availability"></a>Tillgänglighet för återställning av lösenord för självbetjäning av Azure AD

Azure AD självbetjäning lösenordet för återställning finns i tre nivåer beroende på din prenumeration.

* **Azure AD-lediga** – endast molnbaserad administratörer kan återställa sina lösenord
* **Azure AD Basic** eller **betald prenumeration på Office 365** – endast molnbaserad användare och endast molnbaserad administratörer kan återställa sina lösenord
* **Azure AD Premium** – alla användare eller administratör, inklusive endast molnbaserad, federerad eller lösenord synkroniseras användare, kan återställa sina lösenord. Lokala lösenord kräver lösenord tillbakaskrivning toobe aktiverat.

## <a name="azure-ad-self-service-password-reset-a-sum-of-hello-parts"></a>Azure AD Självbetjäning för återställning av lösenord, summan av hello delar

Självbetjäning för återställning av lösenord i Azure AD består av hello följande komponenter:

* **Lösenord för hanteringsportalen configuration** där du kan kontrollera alternativ för hur lösenorden hanteras i din klient via hello Azure-portalen
* **Portalen för registrering för lösenordsåterställning** där användare kan registrera för återställning av lösenord
* **Portal för återställning av lösenord** där användare kan återställa sina lösenord med hjälp av hello utmaningar som definierats av Hej administratör och hello svar användare har angett
* **Portalen för ändring av lösenord** där användare kan ändra sina egna lösenord genom att ange sina gamla lösenord och ett nytt lösenord
* **Rapporter för lösenord** där administratörer kan visa och analysera lösenord aktivitet rapporter för sina klienter i hello Azure-portalen
* **Lösenord tillbakaskrivning tooon lokala med Azure AD Connect** tillåter du tooenable hantering av lokal federerad, eller lösenord synkroniseras användare från hello moln

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Azure AD prissättning, SLA, uppdateringar och -översikt

Mer information om de här ämnena finns på följande sidor hello

* [**Prisinformation för Azure AD**](https://azure.microsoft.com/pricing/details/active-directory/)
* [**Priser för Office 365**](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [**Azure servicenivåavtal**](https://azure.microsoft.com/support/legal/sla/)
* [**Serviceavtal för Microsoft Online Services**](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [**Azure-uppdateringar**](https://azure.microsoft.com/updates/)
* [**Azure-översikt**](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Nästa steg

hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering
* [**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här
* [**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? -Svar tooquestions du alltid vill ha tooask
* [**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR

