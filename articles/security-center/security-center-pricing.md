---
title: "priser för aaaSecurity Center | Microsoft Docs"
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
ms.openlocfilehash: 93cc284540114b048b7a960891c0e68553b008be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-pricing"></a>Priser för Azure Security Center
Azure Security Center hjälper dig att förebygga, upptäcka och åtgärda toothreats med bättre överblick och kontroll över hello säkerheten för dina Azure-resurser. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

## <a name="pricing-tiers"></a>Prisnivåer
Security Center erbjuds i två nivåer:

* Hej **kostnadsfria nivån** aktiveras automatiskt på alla Azure-prenumerationer. hello kostnadsfria nivån ger inblick i hello säkerhetstillståndet hos dina Azure-resurser, grundläggande säkerhetsprinciper, säkerhetsrekommendationer och integrering med säkerhetsprodukter och tjänster från partner.
* Hej **standardnivån** lägger till avancerade threat detection funktioner, inklusive hot intelligence, beteendeanalys, avvikelseidentifiering, säkerhetsincidenter och hot assessment rapporter. hello standardnivån erbjuds gratis för hello första 60 dagarna.

Mer information finns i hello Security Center [sida med priser](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="try-standard-free-for-60-days"></a>Försök Standard gratis under 60 dagar
hello standardnivån erbjuds gratis för hello första 60 dagarna. Hello slutet av 60 dagar startar bör du välja toocontinue använder hello tjänst, vi automatiskt avgifter för användning.

tooget hello standardnivån:

1. Välj hello **princip** panelen på hello **Security Center** bladet.
2. Välj hello prenumeration som du vill tooupgrade tooStandard.
3. På hello **säkerhetsprincip** bladet väljer **prisnivå**.
4. På hello **Välj din prisnivå** bladet väljer **Standard**.
5. Klicka på **Välj**.


## <a name="why-upgrade-toostandard"></a>Varför uppgradera tooStandard?
hello standardnivån av Security Center innehåller alla funktioner i hello kostnadsfria nivån plus avancerade hotidentifiering. Avancerade hot identifiering hjälper till att identifiera active hot målobjekt för Azure-resurser och ger dig hello insikter behövs toorespond snabbt.

Security Center använder avancerade säkerhetsanalyser, som går mycket längre än signaturbaserade lösningar. Genombrott big data och teknik för maskininlärning är används tooevaluate händelser över hello hela molninfrastrukturresurs – upptäcka hot som skulle vara omöjligt tooidentify med manuella metoder och förutsäga hello utvecklingen av attacker.

Säkerhet analytics som medföljer hello standardnivån är:

* **Hot intelligence** -ser ut för kända obehöriga med hjälp av globala hotinformation från Microsoftprodukter och tjänster, hello Microsoft Digital Crimes Unit hello Microsoft Security Response Center och externa flöden används.
* **Beteendeanalys** -gäller kända mönster toodiscover skadligt beteende
* **Avvikelseidentifiering** -använder statistiska toobuild utgångspunkt historisk profilering. Varnar på avvikelser från etablerade baslinjer som följer tooa potentiella angrepp

I hello **säkerhetsaviseringar** bladet nedan Security Center har identifierat en säkerhet **incident**. En säkerhetsincident är en sammanställning av alla aviseringar för en resurs som överensstämmer med kill kedjan mönster. Du väljer hello säkerhetsincident visar mer information om hello incident och visar hello relaterade aviseringar. Mer information om att innehåller du väljer en avisering.

![Säkerhetsincident][2]

Hej **nätverkskommunikation** avisering nedan innehåller information om hello avisering. Innehåller en fullständig beskrivning, dess allvarlighetsgrad, det aktuella tillståndet (vilket i så fall avvisas, betydelse hello användaren tog åtgärd toodismiss den), hello angripna resurser och steg. Det finns också en lista med länkar tooMicrosoft Hotinformation rapporter. De här rapporterna kan användas för säkerhet reparation och defensiva ändamål.

![Utförlig information om säkerhetsaviseringar][3]

## <a name="enable-data-collection"></a>Aktivera datainsamling
tooenable beteendeanalys för virtuell dator, insamling av data måste vara aktiverad.

toovalidate att datainsamling har aktiverats:

1. Välj hello **princip** panelen. Hej **säkerhetsprincip** blad öppnas där dina Azure-prenumerationer.
2. Välj en prenumeration.
3. Om **datainsamling** är inaktiverat, ändra den tooon och spara hello ändringen.

> [!NOTE]
> Om du använder Azure Security Center ledigt, kan du inaktivera insamling av data från virtuella datorer i hello säkerhetsprincip. Insamling av data krävs för prenumerationer på hello standardnivån.
>
>

Se [Aktivera datainsamling i Azure Security Center](security-center-enable-data-collection.md) för mer information.

## <a name="next-steps"></a>Nästa steg
* I det här dokumentet har introducerades toopricing för Security Center. Ytterligare prisinformation finns hello Security Center [sida med priser](https://azure.microsoft.com/pricing/details/security-center/).
* toolearn mer om Security Center har avancerade funktioner för identifiering, se [identifieringsfunktionerna i Azure Security Center](security-center-detection-capabilities.md).
* toolearn mer information om hur data hanteras och garanteras i Security Center finns [datasäkerhet i Azure Security Center](security-center-data-security.md).
* Om du har frågor om hur du använder Security Center finns hello [Azure Security Center vanliga frågor och svar](security-center-faq.md).
* Om du fortfarande har frågor om hur du använder Security Center eller Azure gå hello [Azure forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureSecurityCenter&filter=alltypes&sort=lastpostdesc).

<!--Image references-->
[1]: ./media/security-center-pricing/standard.png
[2]: ./media/security-center-pricing/incident.png
[3]: ./media/security-center-pricing/network-alert.png
