---
title: aaaAzure Security Center quick start guiden | Microsoft Docs
description: "Den här artikeln hjälper dig att komma igång snabbt med Azure Security Center genom guidar dig genom hello säkerhetsövervakning och principhantering management säkerhetskomponenter och länka toonext steg."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 23b2444ba1ba30d0a1bd1a1afbc4fd0abfd0827c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-quick-start-guide"></a>Snabbstartsguide för Azure Security Center
Den här artikeln hjälper dig att snabbt komma igång med Azure Security Center genom instruktioner hello säkerhetsövervakning och principhantering management säkerhetskomponenter av Security Center.

> [!NOTE]
> Från och med tidig juni 2017 Security Center använder hello Microsoft Monitoring Agent toocollect och lagra data. Se [Azure Security Center-plattformen migrering](security-center-platform-migration.md) toolearn mer. hello informationen i den här artikeln representerar Security Center-funktionalitet efter övergången toohello Microsoft Monitoring Agent.
>
>

## <a name="prerequisites"></a>Krav
tooget igång med Security Center, måste du ha en prenumeration tooMicrosoft Azure. Om du inte har någon prenumeration kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).

hello kostnadsfria nivån av Security Center aktiveras automatiskt med din prenumeration och ger inblick i hello säkerhetstillståndet hos dina Azure-resurser. Den tillhandahåller grundläggande säkerhetsprinciper, säkerhetsrekommendationer och integrering med säkerhetsprodukter och tjänster från Azure-partners.

Security Center öppnas från hello [Azure-portalen](https://azure.microsoft.com/features/azure-portal/). toolearn mer om hello Azure-portalen finns hello [portaldokumentationen](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="permissions"></a>Behörigheter
I Security Center, du kan bara se information om tooan Azure-resurs när du har tilldelats hello rollen ägare, deltagare eller läsare för hello prenumerationen eller resursen gruppen som en resurs tillhör. Se [behörigheter i Azure Security Center](security-center-permissions.md) toolearn mer om roller och tillåtna åtgärder i Security Center.

## <a name="data-collection"></a>Datainsamling
Security Center samlar in data från virtuella datorer (VM)-tooassess sina säkerhetstillstånd anger säkerhetsrekommendationer och varna dig toothreats. När du först öppnar Security Center aktiveras insamling av data på alla virtuella datorer i din prenumeration. Security Center tillhandahåller hello Microsoft Monitoring Agent på alla befintliga stöds virtuella Azure-datorer och nya filer som skapas. Se [Aktivera datainsamling](security-center-enable-data-collection.md) toolearn mer om hur datainsamling fungerar.

Insamling av data rekommenderas. Om du använder hello kostnadsfria nivån av Security Center kan inaktivera du insamling av data från virtuella datorer genom att stänga av insamling av data i hello säkerhetsprincipen. Insamling av data krävs för prenumerationer på hello standardnivån av Security Center. Se [Security Center priser](security-center-pricing.md) toolearn mer om hello lediga och Standard prisnivåer.

hello följande steg beskriver hur tooaccess och använda hello komponenter i Security Center. I följande steg ska vi visa dig hur tooturn av insamling av data om du väljer tooopt ut.

> [!NOTE]
> Den här artikeln introducerar hello-tjänsten med hjälp av ett exempel på distribution. Artikeln är alltså inte en steg-för-steg-guide.
>
>

## <a name="access-security-center"></a>Öppna Security Center
Följ dessa steg tooaccess Security Center i hello-portalen:

1. På hello **Microsoft Azure** väljer du **Security Center**.

   ![Azure-menyn][1]
2. Om du öppnar Security Center för hello första gången, hello **Välkommen** blad öppnas. Välj **starta Security Center** tooopen hello **Security Center** bladet och tooenable datainsamling.
   ![Välkomstskärmen][10]
3. När du startar Security Center från hello Välkommen bladet eller välj Security Center hello Microsoft Azure-menyn, hello **Security Center** blad öppnas. För enkel åtkomst toohello **Security Center** bladet i hello framtida, Välj hello **PIN-kod bladet toodashboard** alternativet (längst upp till höger).
   ![PIN-kod bladet toodashboard alternativet][2]

## <a name="use-security-center"></a>Använda Security Center
Du kan konfigurera säkerhetsprinciper för dina Azure-prenumerationer och -resursgrupper. Vi ska konfigurera en säkerhetsprincip för din prenumeration:

1. På hello **Security Center** bladet, Välj hello **princip** panelen.
2. På hello **säkerhetsprincip – definiera princip per prenumeration** bladet Välj en prenumeration.
3. På hello **säkerhetsprincip** bladet **datainsamling** är aktiverade tooautomatically samla in loggar. hello övervakning tillägget har etablerats på alla aktuella och nya virtuella datorer i hello prenumeration. (På hello kostnadsfria nivån av Security Center kan du välja utanför insamling av data genom att ange **datainsamling** för**av**. Ange **datainsamling** för**av** så att du säkerhetsaviseringar och rekommendationer förhindrar Security Center.)
4. På hello **säkerhetsprincip** bladet väljer **skyddsprincip**. Då öppnas hello **skyddsprincip** bladet.
5. På hello **skyddsprincip** bladet aktivera hello rekommendationer som du vill toosee som en del av din säkerhetsprincip. Exempel:

   * Ange **systemuppdateringar** för**på** genomsökningar alla stöd för virtuella datorer för saknade OS-uppdateringar.
   * Ange **OS säkerhetsrisker** för**på** genomsökningar stöds alla virtuella datorer tooidentify OS-konfigurationer som kan göra hello VM mer sårbara tooattack.

### <a name="view-recommendations"></a>Visa rekommendationer
1. Returnera toohello **Security Center** bladet och väljer hello **rekommendationer** panelen. Security Center analyserar regelbundet hello säkerhetstillståndet hos dina Azure-resurser. Security Center identifierar potentiella säkerhetsrisker, visas rekommendationer på hello **rekommendationer** bladet.
   ![Rekommendationer i Azure Security Center][5]
2. Välj en rekommendation om hello **rekommendationer** bladet tooview mer information och/eller tootake åtgärd tooresolve hello utfärda.

### <a name="view-hello-security-state-of-your-resources"></a>Visa hello säkerhetsstatus för dina resurser
1. Returnera toohello **Security Center** bladet. Hej **förebyggande** delen av hello instrumentpanelen innehåller indikatorer för hello säkerhetsstatus för virtuella datorer, nätverk, data och program.
2. Välj **Compute** tooview mer information. Hej **Compute** blad öppnas som visar tre flikar:

  - **Översikt över** -innehåller övervakning och rekommendationer för Virtuella datorer.
  - **Virtuella datorer** -visar en lista över alla virtuella datorer och aktuell säkerhet tillstånd.
  - **Molntjänster** -listar webb- och arbetsroller roller som övervakas via Security Center.

    ![Hej resurshälsa i Azure Security Center][6]

3. På hello **översikt** väljer du en rekommendation under **rekommendationer för VIRTUELLA datorer** tooview mer information och/eller vidta åtgärden tooconfigure nödvändiga kontroller.
4. På hello **virtuella datorer** väljer du ett VM-tooview ytterligare information.

### <a name="view-security-alerts"></a>Visa säkerhetsaviseringar
1. Returnera toohello **Security Center** bladet och väljer hello **säkerhetsaviseringar** panelen. Hej **säkerhetsaviseringar** blad öppnas och visar en lista över aviseringar. hello Security Centers analys av dina säkerhetsloggar och din nätverksaktivitet genererar aviseringarna. Aviseringar från integrerade partnerlösningar ingår.
   ![Säkerhetsaviseringar i Azure Security Center][7]

   > [!NOTE]
   > Säkerhetsaviseringar är endast tillgängliga om hello standardnivån av Security Center aktiveras. En 60 dagars utvärderingsversion av hello standardnivån är tillgänglig. Se [nästa steg](#next-steps) information om hur tooget hello Standard tjänstnivån.
   >
   >
2. Välj en avisering tooview ytterligare information. I det här exemplet väljer vi **Modified system binary discovered** (Identifierad ändring av binär systemfil). Detta öppnar blad som tillhandahåller ytterligare information om hello avisering.
   ![Information om säkerhetsaviseringar i Azure Security Center][8]

### <a name="view-hello-health-of-your-partner-solutions"></a>Visa hello hälsa på partnerlösningarna
1. Returnera toohello **Security Center** bladet. Hej **partnerlösningar** sida vid sida kan du övervaka en överblick över hello hälsostatusen på de partnerlösningar som är integrerade i azureprenumerationen.
2. Välj hello **partnerlösningar** panelen. Då öppnas ett blad och visar en lista över partnerlösningarna anslutna tooSecurity Center.
   ![Partnerlösningar][9]
3. Markera en partnerlösning. I det här exemplet väljer vi hello **QualysVa1** lösning.  Ett blad öppnas och visar hello status för Partnerlösningen hello och hello lösning associerade resurser. Välj **lösning konsolen** tooopen hello partner partnerhanteringsfunktionen för lösningen.

## <a name="next-steps"></a>Nästa steg
Den här artikeln introduceras toohello säkerhetsövervakning och principhantering management säkerhetskomponenter av Security Center. Nu när du är bekant med Security Center, försök hello följande steg:

* Vi ska konfigurera en säkerhetsprincip för din Azure-prenumeration. Det finns fler toolearn [ställa in säkerhetsprinciper i Azure Security Center](security-center-policies.md).
* Använd hello rekommendationer i Security Center toohelp skyddar du dina Azure-resurser. Det finns fler toolearn [hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md).
* Granska och hantera din aktuella säkerhetsaviseringar. Det finns fler toolearn [hantering och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md).
- [Datasäkerhet i Azure Security Center](security-center-data-security.md) – Lär dig hur data hanteras och garanteras i Security Center.
* Mer information om hello [advanced threat funktioner för identifiering](security-center-detection-capabilities.md) som medföljer hello [standardnivån](security-center-pricing.md) av Security Center. hello standardnivån erbjuds gratis för hello första 60 dagarna.
* Om du har frågor om hur du använder Security Center finns hello [Azure Security Center vanliga frågor och svar](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
