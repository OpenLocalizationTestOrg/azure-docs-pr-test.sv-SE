---
title: "Priser för Security Center | Microsoft Docs"
description: "Den här artikeln innehåller information om priser för Azure Security Center."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 367b8f38cb9fcf3dc36db83641cb1696710608ef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-security-center-pricing"></a>Priser för Azure Security Center
Med hjälp av Azure Security Center kan du förebygga, upptäcka och åtgärda hot med bättre överblick och kontroll över säkerheten för dina resurser i Azure. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

## <a name="pricing-tiers"></a>Prisnivåer
Security Center erbjuds i två nivåer:

* Den **kostnadsfria nivån** aktiveras automatiskt på alla Azure-prenumerationer. Den kostnadsfria nivån ger insyn i säkerhetsläget för Azure-resurser, grundläggande säkerhetsprinciper, säkerhetsrekommendationer och integrering med säkerhetsprodukter och tjänster från partner.
* Den **standardnivån** lägger till avancerade threat detection funktioner, inklusive hot intelligence, beteendeanalys, avvikelseidentifiering, säkerhetsincidenter och hot assessment rapporter. Standardnivån erbjuds kostnadsfritt de första 60 dagarna.

Mer information finns i Security Center [sida med priser](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="try-standard-free-for-60-days"></a>Försök Standard gratis under 60 dagar
Standardnivån erbjuds kostnadsfritt de första 60 dagarna. I slutet av 60 dagar startar om du väljer att fortsätta använda tjänsten, vi automatiskt avgifter för användning.

Hämta standardnivån:

1. Välj ikonen **Princip** i bladet **Security Center**.
2. Välj den prenumeration som du vill uppgradera till Standard.
3. På den **säkerhetsprincip** bladet väljer **prisnivå**.
4. På den **Välj din prisnivå** bladet väljer **Standard**.
5. Klicka på **Välj**.


## <a name="why-upgrade-to-standard"></a>Varför uppgradera till Standard?
Standardnivån av Security Center innehåller alla funktioner i den kostnadsfria nivån plus avancerade hotidentifiering. Avancerade hotidentifiering identifierar active hot målobjekt för Azure-resurser och ger dig de insikter som behövs för att svara snabbt.

Security Center använder avancerade säkerhetsanalyser, som går mycket längre än signaturbaserade lösningar. Genombrott big data och maskininlärning tekniker används för att utvärdera händelser över hela molninfrastrukturresurs – upptäcka hot som skulle vara omöjligt att identifiera med manuella metoder och förutsäga utvecklingen av attacker.

Säkerhet analytics som medföljer standardnivån är:

* **Hot intelligence** -söker efter kända obehöriga med hjälp av globala hotinformation från Microsoftprodukter och tjänster, Microsoft Digital Crimes Unit, Microsoft Security Response Center och externa flöden används.
* **Beteendeanalys** -gäller kända mönster för att identifiera skadligt beteende
* **Avvikelseidentifiering** -använder statistiska profilering för att bygga en historisk baslinje. Varnar på avvikelser från etablerade baslinjer som överensstämmer med potentiella angrepp

I den **säkerhetsaviseringar** bladet nedan Security Center har identifierat en säkerhet **incident**. En säkerhetsincident är en sammanställning av alla aviseringar för en resurs som överensstämmer med kill kedjan mönster. Om du väljer säkerhetsincident visar mer information om incidenten och visar en lista över relaterade aviseringar. Mer information om att innehåller du väljer en avisering.

![Säkerhetsincident][2]

Den **nätverkskommunikation** avisering nedan innehåller information om aviseringen. Innehåller en fullständig beskrivning, dess allvarlighetsgrad, det aktuella tillståndet (som i det här fallet stängs, vilket innebär att användaren har gjort åtgärd för att stänga den), angripna resursen och steg. Det finns också en lista med länkar till Microsoft Hotinformation rapporter. De här rapporterna kan användas för säkerhet reparation och defensiva ändamål.

![Utförlig information om säkerhetsaviseringar][3]

## <a name="enable-data-collection"></a>Aktivera datainsamling
Om du vill aktivera beteendeanalys för virtuell dator, måste insamling av data vara aktiverad.

Kontrollera att datainsamling har aktiverats:

1. Välj den **princip** panelen. Den **säkerhetsprincip** blad öppnas där dina Azure-prenumerationer.
2. Välj en prenumeration.
3. Om **datainsamling** är inaktiverat, ändra den till på och spara ändringen.

> [!NOTE]
> Om du använder Azure Security Center ledigt, kan du inaktivera insamling av data från virtuella datorer i säkerhetsprincipen. Insamling av data krävs för prenumerationer på standardnivån.
>
>

Se [Aktivera datainsamling i Azure Security Center](security-center-enable-data-collection.md) för mer information.

## <a name="next-steps"></a>Nästa steg
* I det här dokumentet berättade priser för Security Center. Ytterligare prisinformation finns i Security Center [sida med priser](https://azure.microsoft.com/pricing/details/security-center/).
* Läs mer om funktionerna i Security Center avancerad identifiering i [identifieringsfunktionerna i Azure Security Center](security-center-detection-capabilities.md).
* Mer information om hur data hanteras och garanteras i Security Center finns [datasäkerhet i Azure Security Center](security-center-data-security.md).
* Om du har frågor om hur du använder Security Center går du till [Vanliga frågor och svar om Azure Security Center](security-center-faq.md).
* Om du fortfarande har frågor om att använda Security Center eller Azure finns i [Azure forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureSecurityCenter&filter=alltypes&sort=lastpostdesc).

<!--Image references-->
[1]: ./media/security-center-pricing/standard.png
[2]: ./media/security-center-pricing/incident.png
[3]: ./media/security-center-pricing/network-alert.png
