---
title: "Snabbstartsguide för Azure Security Center | Microsoft Docs"
description: "Den här artikeln hjälper dig att komma igång snabbt med Azure Security Center och guidar dig genom komponenterna för säkerhetsövervakning och principhantering och länkar till nästa steg."
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
ms.openlocfilehash: 392c814b7d3ff6b4f0f7850a51960576775e0307
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-security-center-quick-start-guide"></a>Snabbstartsguide för Azure Security Center
Den här artikeln hjälper dig att komma igång med Azure Security Center och guidar dig genom komponenterna för säkerhetsövervakning och principhantering i Security Center.

> [!NOTE]
> Från och med tidig juni 2017 använder Security Center Microsoft Monitoring Agent för att samla in och lagra data. Mer information finns under [plattformsmigrering i Azure Security Center](security-center-platform-migration.md). Informationen i den här artikeln representerar Security Centers funktionalitet efter övergången till Microsoft Monitoring Agent.
>
>

## <a name="prerequisites"></a>Krav
Du måste ha en prenumeration på Microsoft Azure för att komma igång med Security Center. Om du inte har någon prenumeration kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).

Den kostnadsfria nivån i Security Center aktiveras automatiskt med din prenumeration och ger bättre inblick i säkerhetstillståndet hos dina Azure-resurser. Den tillhandahåller grundläggande säkerhetsprinciper, säkerhetsrekommendationer och integrering med säkerhetsprodukter och tjänster från Azure-partners.

Security Center öppnas från [Azure Portal](https://azure.microsoft.com/features/azure-portal/). Om du vill ta reda på mer om Azure Portal går du till [portaldokumentationen](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="permissions"></a>Behörigheter
I Security Center kan se du bara information relaterad till en Azure-resurs när du har tilldelats rollen ägare, deltagare eller läsare för prenumeration eller resursgrupp som en resurs tillhör. Se [behörigheter i Azure Security Center](security-center-permissions.md) för mer information om roller och tillåtna åtgärder i Security Center.

## <a name="data-collection"></a>Datainsamling
Security Center samlar in data från dina virtuella datorer (VM) för att utvärdera deras säkerhetstillstånd, ange säkerhetsrekommendationer och varna för hot. När du först öppnar Security Center aktiveras insamling av data på alla virtuella datorer i din prenumeration. Security Center tillhandahåller Microsoft Monitoring Agent på alla befintliga stöds virtuella Azure-datorer och nya filer som skapas. Se [Aktivera datainsamling](security-center-enable-data-collection.md) vill veta mer om hur datainsamling fungerar.

Insamling av data rekommenderas. Om du använder den kostnadsfria nivån av Security Center kan du inaktivera insamling av data från virtuella datorer genom att stänga av insamling av data i säkerhetsprincipen. Insamling av data krävs för prenumerationer på standardnivån av Security Center. Se [Security Center priser](security-center-pricing.md) lära dig mer om den kostnadsfria och Standard prisnivåer.

I följande steg beskrivs hur du får åtkomst till och använder komponenterna i Security Center. I dessa steg visar vi hur du inaktiverar insamling av data om du vill avanmäla dig.

> [!NOTE]
> I den här artikeln beskrivs tjänsten genom en exempeldistribution. Artikeln är alltså inte en steg-för-steg-guide.
>
>

## <a name="access-security-center"></a>Öppna Security Center
Följ anvisningarna för att öppna Security Center i portalen:

1. På menyn **Microsoft Azure** väljer du **Security Center**.

   ![Azure-menyn][1]
2. Om du öppnar Security Center för första gången visas **välkomstbladet**. Välj **starta Security Center** att öppna den **Security Center** blad och vill aktivera datainsamling.
   ![Välkomstskärmen][10]
3. När du har startat Security Center från välkomstbladet eller valt Security Center på Microsoft Azure-menyn öppnas bladet **Security Center**. Om du enkelt vill komma åt bladet **Security Center** framöver markerar du alternativet **Fäst bladet på instrumentpanelen** (längst upp till höger).
   ![Alternativet Fäst bladet på instrumentpanelen][2]

## <a name="use-security-center"></a>Använda Security Center
Du kan konfigurera säkerhetsprinciper för dina Azure-prenumerationer och -resursgrupper. Vi ska konfigurera en säkerhetsprincip för din prenumeration:

1. På bladet **Security Center** markerar du panelen **Princip**.
2. På den **säkerhetsprincip – definiera princip per prenumeration** bladet Välj en prenumeration.
3. På bladet **Säkerhetsprincip** aktiverar du **Datainsamling** så att loggar samlas in automatiskt. Övervakningstillägget etableras på alla aktuella och nya virtuella datorer i prenumerationen. (Om den kostnadsfria nivån av Security Center kan du välja utanför insamling av data genom att ange **datainsamling** till **av**. Ange **datainsamling** till **av** så att du säkerhetsaviseringar och rekommendationer förhindrar Security Center.)
4. På bladet **Säkerhetsprincip** väljer du **Skyddsprincip**. Då öppnas bladet **Skyddsprincip**.
5. På bladet **Skyddsprincip** aktiverar du de rekommendationer du vill ska visas som en del av din säkerhetsprincip. Exempel:

   * Ange **systemuppdateringar** till **på** genomsökningar alla stöd för virtuella datorer för saknade OS-uppdateringar.
   * Ange **OS säkerhetsrisker** till **på** genomsökningar alla stöd för virtuella datorer för att identifiera OS-konfigurationer som kan göra den virtuella datorn mer sårbara för angrepp.

### <a name="view-recommendations"></a>Visa rekommendationer
1. Gå tillbaka till bladet **Security Center** och välj panelen **Rekommendationer**. Security Center analyserar regelbundet säkerhetstillståndet hos dina Azure-resurser. När Security Center identifierar potentiella säkerhetsproblem visas rekommendationer på bladet **Rekommendationer**.
   ![Rekommendationer i Azure Security Center][5]
2. Markera en rekommendation på bladet **Rekommendationer** om du vill se mer information och/eller vidta åtgärder för att lösa problemet.

### <a name="view-the-security-state-of-your-resources"></a>Se säkerhetsstatus för dina resurser
1. Gå tillbaka till bladet **Security Center**. Den **förebyggande** på instrumentpanelen innehåller indikatorer gällande säkerhetsstatus för virtuella datorer, nätverk, data och program.
2. Välj **Compute** vill visa mer information. Den **Compute** blad öppnas som visar tre flikar:

  - **Översikt över** -innehåller övervakning och rekommendationer för Virtuella datorer.
  - **Virtuella datorer** -visar en lista över alla virtuella datorer och aktuell säkerhet tillstånd.
  - **Molntjänster** -listar webb- och arbetsroller roller som övervakas via Security Center.

    ![Panelen för resursernas hälsotillstånd i Azure Security Center][6]

3. På den **översikt** väljer du en rekommendation under **rekommendationer för VIRTUELLA datorer** att visa mer information och/eller vidta åtgärder för att konfigurera nödvändiga kontroller.
4. På den **virtuella datorer** väljer du en virtuell dator för att visa ytterligare information.

### <a name="view-security-alerts"></a>Visa säkerhetsaviseringar
1. Gå tillbaka till bladet **Security Center** och välj panelen **Säkerhetsaviseringar**. Bladet **Säkerhetsaviseringar** öppnas och visar en lista över aviseringar. Aviseringarna genereras av Security Centers analys av dina säkerhetsloggar och din nätverksaktivitet. Aviseringar från integrerade partnerlösningar ingår.
   ![Säkerhetsaviseringar i Azure Security Center][7]

   > [!NOTE]
   > Säkerhetsaviseringar är endast tillgängliga om standardnivån i Security Center är aktiverad. En 60 dagars utvärderingsversion av standardnivån är tillgänglig. Se [Nästa steg](#next-steps) för att få information om hur du hämtar standardnivån.
   >
   >
2. Välj en avisering om du vill visa ytterligare information. I det här exemplet väljer vi **Modified system binary discovered** (Identifierad ändring av binär systemfil). Blad som kan ger mer information om aviseringen öppnas.
   ![Information om säkerhetsaviseringar i Azure Security Center][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Visa hälsotillstånd för dina partnerlösningar
1. Gå tillbaka till bladet **Security Center**. I panelen **Partner solutions (Partnerlösningar)** kan du snabbt se hälsostatusen på de partnerlösningar som är integrerade i din Azure-prenumeration.
2. Klicka på rutan **Partner solutions (Partnerlösningar)**. Då öppnas ett blad som visar lista över alla partnerlösningar som är anslutna till Security Center.
   ![Partnerlösningar][9]
3. Markera en partnerlösning. I det här exemplet ska vi väljer den **QualysVa1** lösning.  Då öppnas ett blad som visar status för partnerlösningen och resurserna som är kopplade till lösningen. Om du klickar på **Solution console (Lösningskonsol)** öppnas partnerhanteringsfunktionen för lösningen i fråga.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du introducerats för komponenterna säkerhetsövervakning och principhantering i Security Center. Nu när du är bekant med Security Center kan du prova följande steg:

* Vi ska konfigurera en säkerhetsprincip för din Azure-prenumeration. Läs mer i [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md).
* Använd rekommendationer i Security Center som hjälper dig att skydda dina Azure-resurser. Läs mer i [Managing security recommendations in Azure Security Center](security-center-recommendations.md) (Hantera säkerhetsrekommendationer i Azure Security Center).
* Granska och hantera din aktuella säkerhetsaviseringar. Läs mer i [Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md).
- [Datasäkerhet i Azure Security Center](security-center-data-security.md) – Lär dig hur data hanteras och garanteras i Security Center.
* Lär dig mer om [funktionerna för avancerad hotidentifiering](security-center-detection-capabilities.md) som medföljde [standardnivån](security-center-pricing.md) i Security Center. Standardnivån erbjuds kostnadsfritt de första 60 dagarna.
* Om du har frågor om hur du använder Security Center går du till [Vanliga frågor och svar om Azure Security Center](security-center-faq.md).

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
