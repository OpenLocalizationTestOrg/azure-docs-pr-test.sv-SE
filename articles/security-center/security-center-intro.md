---
title: aaaIntroduction tooAzure Security Center | Microsoft Docs
description: "Här får du lära dig om Azure Security Center, de viktigaste funktionerna och hur Security Center fungerar."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 287dbaaa7e2004c522f103595bc316261daf05b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-security-center"></a>Introduktion tooAzure Security Center
Här får du lära dig om Azure Security Center, de viktigaste funktionerna och hur Security Center fungerar.

> [!NOTE]
> Från och med tidig juni 2017 Security Center använder hello Microsoft Monitoring Agent toocollect och lagra data. Se [Azure Security Center-plattformen migrering](security-center-platform-migration.md) toolearn mer. hello informationen i den här artikeln representerar Security Center-funktionalitet efter övergången toohello Microsoft Monitoring Agent.
>
>

## <a name="what-is-azure-security-center"></a>Vad är Azure Security Center?
 Security Center hjälper dig att förebygga, upptäcka och åtgärda toothreats med bättre överblick och kontroll över hello säkerheten för dina Azure-resurser. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

## <a name="key-capabilities"></a>De viktigaste funktionerna
 Security Center har lätt att använda och effektivt hot förhindra, upptäcka och svar-funktioner som är inbyggda i tooAzure. Här är de viktigaste funktionerna:

| Fas | Funktion |
| --- | --- |
| Förebygga |Övervakare hello säkerhetstillståndet hos dina Azure-resurser |
| Förebygga | Definierar principer för dina Azure-prenumerationer utifrån företagets säkerhetskrav, hello typer av program som du använder och hello känslighet för dina data |
| Förebygga | Utifrån principerna ges säkerhet rekommendationer tooguide tjänstens ägare via hello process för att implementera som behövs. |
| Förebygga | Tjänster och redskap från Microsoft och partnerföretag kan snabbt distribueras. |
| Upptäcka |Automatiskt samlas in och analyseras säkerhetsdata från Azure-resurser, hello nätverket och partnerlösningarna, till exempel program mot skadlig kod och brandväggar |
| Upptäcka | Använder globala hot intelligence från Microsoft-produkter och tjänster, hello Microsoft Digital Crimes Unit (DCU), hello Microsoft Security Response Center (MSRC) och externa flöden används. |
| Upptäcka | Avancerad analysteknik, till exempel machine learning och beteendeanalys används. |
| Åtgärda |Säkerhetsrelaterade incidenter/aviseringar prioritetsklassas och rapporteras. |
| Åtgärda | Ger insikter om hello källan hello angrepp och vilka resurser |
| Åtgärda | Förslag om hur toostop hello pågående angrepp och förebygga framtida angrepp. |

## <a name="introductory-walkthrough"></a>Introduktionsgenomgång

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution. Det här dokumentet är inte en stegvis guide.
>
>

 Security Center öppnas från hello [Azure-portalen](https://azure.microsoft.com/features/azure-portal/). [Logga in toohello portal](https://portal.azure.com). Under hello portal huvudmenyn rulla toohello **Security Center** alternativ eller välj hello **Security Center** panelen som du tidigare har fäst toohello portalens instrumentpanel.

![Säkerhetsrutan i Azure Portal][1]

I Security Center kan du ställa in säkerhetsprinciper, övervaka säkerhetskonfigurationer och se säkerhetsaviseringar.

### <a name="security-policies"></a>Säkerhetsprinciper
Du kan definiera principer för Azure-prenumerationer enligt tooyour företagets säkerhetskrav. Du kan också anpassa dem toohello typer av program som du använder eller toohello känslighet hello data i varje prenumeration. De resurser som används för utveckling eller tester behöver kanske en annan typ av säkerhetsinställningar än de som används för program i produktion. På samma sätt behövs det kanske en högre säkerhetsnivå för sådant som det finns lagar om, till exempel personuppgifter.

> [!NOTE]
> toomodify säkerhetspolicyn, bör du vara en säkerhetsadministratör eller hello Prenumerationens ägare eller deltagare. toolearn mer information om roller och tillåtna åtgärder i Security Center finns [behörigheter i Azure Security Center](security-center-permissions.md).
>
>

På hello **Security Center** bladet, Välj hello **princip** panelen för en lista över dina prenumerationer och resursgrupper.   

![Security Center-bladet][2]

På hello **säkerhetsprincip** bladet Välj en tooview hello princip prenumerationsinformation.

**Datainsamling** samlas data för en säkerhetsprincip. Om du aktiverar den här funktionen händer följande:

* Genomsöks alla kompatibla virtuella datorer (VM) för säkerhetsövervakning och rekommendationer.
* Säkerhetshändelser samlas in för analys och hotidentifiering.

> [!NOTE]
> Datainsamling konfigureras på hello prenumerationsnivån.
>
>

Välj **skyddsprincip** tooopen hello **skyddsprincip** bladet. **Visa rekommendationer för** kan du välja hello säkerhetsåtgärder som du vill toomonitor och hello rekommendationer som du vill toosee baserat på hello säkerhetsbehov hello resurser inom hello prenumeration.

### <a name="security-recommendations"></a>Säkerhetsrekommendationer
 Security Center analyserar hello säkerhetstillståndet hos dina Azure-resurser tooidentify potentiella säkerhetsproblem. En lista över rekommendationer hjälper dig att hello konfigureringen av nödvändiga kontroller. Exempel:

* Etablera program mot skadlig kod toohelp identifiera och ta bort skadlig programvara
* Konfigurera network security grupper och regler toocontrol trafik tooVMs
* Etablering av web application brandväggar toohelp skydd mot angrepp riktade webbprogram
* genomföra alla systemuppdateringar som fattas
* OS-konfigurationer som inte matchar hello-adressering rekommenderade baslinjer

Klicka på hello **rekommendationer** panelen för en lista över rekommendationer. Klicka på varje rekommendation tooview ytterligare information eller tootake åtgärd tooresolve hello problemet.

![Säkerhetsrekommendationer i Azure Security Center][5]

### <a name="security-state-of-azure-resources"></a>Säkerhetstillståndet hos Azure-resurser
Hej **förebyggande** avsnittet hello instrumentpanelen visar hello övergripande säkerhetstillståndet hello miljön uppdelat efter resurstyp, inklusive virtuella datorer, webbprogram och andra resurser.   

Välj en resurstyp under **förebyggande** tooview mer information, inklusive en lista över alla potentiella säkerhetsproblem har identifierats. (**Compute** väljs i hello exemplet nedan.)

![Rutan med resurshälsa][6]

### <a name="security-alerts"></a>Säkerhetsaviseringar
 Security Center automatiskt samlar in, analyseras och integreras loggdata från Azure-resurser, hello nätverket och partnerlösningarna, till exempel program mot skadlig kod och brandväggar. Om hot upptäcks skapas en säkerhetsavisering. Exempel på hot:

* Angripna virtuella datorer som kommunicerar med kända skadliga IP-adresser
* avancerad skadlig kod upptäckt genom felrapporteringen i Windows
* Brute force-attacker mot virtuella datorer
* säkerhetsaviseringar från integrerade brandväggar och program mot skadlig kod

Klicka på hello **säkerhetsaviseringar** visas en lista med prioritetsklassade aviseringar.

![Säkerhetsaviseringar][7]

Om du väljer en avisering visas mer information om hello och förslag på hur tooremediate den.

![Utförlig information om säkerhetsaviseringar][8]

### <a name="partner-solutions"></a>Partnerlösningar
Hej **partnerlösningar** sida vid sida kan du snabbt en överblick över hello säkerhetstillstånd på partnerlösningarna som är integrerad med din Azure-prenumeration. Security Center visar aviseringar från hello lösningar.

Välj hello **partnerlösningar** panelen. Då öppnas ett blad med en lista över alla anslutna partnerlösningar.

![Partnerlösningar][9]

## <a name="get-started"></a>Kom igång
tooget igång med Security Center, behöver du en prenumeration tooMicrosoft Azure. Security Center aktiveras genom Azure-prenumerationen. Om du inte har någon prenumeration kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).

 Security Center öppnas från hello [Azure-portalen](https://azure.microsoft.com/features/azure-portal/). Se hello [portaldokumentationen](https://azure.microsoft.com/documentation/services/azure-portal/) toolearn mer.

[Komma igång med Azure Security Center](security-center-get-started.md) snabbt hjälper dig att hello-säkerhetsövervakning och principhantering komponenter i Security Center.

## <a name="next-steps"></a>Nästa steg
I det här dokumentet har introducerades tooSecurity Center, de viktigaste funktionerna och hur tooget startas. toolearn se fler hello följande resurser:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
- [Datasäkerhet i Azure Security Center](security-center-data-security.md) – Lär dig hur data hanteras och garanteras i Security Center.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – hello Azure security nyheter och information.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
