---
title: "aaaMicrosoft Azure Usage and RateCard API: er aktivera Cloudyn tooProvide ITFM för kunder | Microsoft Docs"
description: "Ger ett unikt perspektiv från Microsoft Azure Billing partner Cloudyn, på deras upplevelser integrera hello Azure Billing API: erna i sin produkt.  Detta är särskilt användbart för Azure och Cloudyn kunder som vill använda/försök Cloudyn för Azure-tjänster."
services: 
documentationcenter: 
author: BryanLa
manager: tonguyen
editor: 
tags: billing
ms.assetid: f1397397-7e92-4c20-9862-ab6b93afefb7
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 02/03/2017
ms.author: mobandyo;bryanla
ms.openlocfilehash: e221ac8b8feebb725a1cc669c8143ab829621a8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-tooprovide-itfm-for-customers"></a>Microsoft Azure-användning och RateCard API: er aktivera Cloudyn tooProvide ITFM för kunder
Cloudyn, en Microsoft-partner för utveckling och en inledande moln hanteringsfunktioner,-provider har valts för en privat förhandsgranskning av hello nya resursanvändningen för Microsoft Azure och RateCard APIs.  hello användning API ger åtkomst tooestimated Azure-förbrukningsdata för en prenumeration. Hej RateCard API ger fullständig prisinformation för alla Azure-tjänster, för icke - Enterprise-avtal-EA-kunder. Integrerad tillsammans, underlag API: erna fullständig information för indata i IT finansiella Management (ITFM) verktyg som de som tillhandahålls av Cloudyn.

## <a name="introduction"></a>Introduktion
Hej så kallade ”multiplikation” data från hello API för användning med data från hello RateCard API (användning [enheter] pris [$unit] = detaljerad användning och kostnader) skapar hello mest detaljerade, exakta och tillförlitliga faktureringsinformation tillgänglig för Azure idag.

![Översikt över ITFM][1]

Använda dessa API: er innehåller viktig information om kunders användning och kostnader, vilket gör att Cloudyn tooanalyze kundkonton i en enkel, programmässiga sätt och tooperform olika ITFM uppgifter för sina kunder.

## <a name="integrating-cloudyn-with-hello-ratecard-and-usage-apis"></a>Integrera Cloudyn med hello RateCard och användning av API: er
Hej RateCard API kräver flera indataparametrar--som region info, valuta och språk-- men hello viktigaste en är OfferDurableID som anger hello typ av Azure erbjudanden hello kund använder (betalning per användning, äldre 6 och 12 månaders åtagande planer, MSDN erbjudanden, MPN erbjudanden, specialerbjudanden och andra). Hej OfferDurableID hittar du i hello [Azure Usage and Billing portal](https://account.windowsazure.com/Subscriptions), under hello ”erbjuder-ID” för hello angivna prenumerationen.

Vid registreringen för [Cloudyn för Azure](https://www.cloudyn.com/microsoft-azure/) tjänster, kunder kan lägga till sina OfferDurableID-kod, vilket gör att Cloudyn toopull relevanta prisinformation via hello RateCard API.  Information om hello olika typer av erbjudanden finns en hello [Erbjudandeinformationen för Microsoft Azure](https://azure.microsoft.com/support/legal/offer-details/) sidan.

![Översikt över Cloudyn ITFM-motorn][2]

Cloudyn använder både hello användnings- och RateCard APIs i tillägg toohello Azure prestanda API, toocreate ytterligare skyddslager för visualisering analytics, aviseringar, kostnad rapporter, hantering och tillämplig rekommendationer som ger Azure kunderna en tillförlitlig Enterprise cloud ITFM verktyg.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Cloudyn ITFM användningsfall aktiveras med användnings- och RateCard API-integrering
Vanliga Cloudyn ITFM användningsfall aktiveras med användning och RateCard APIs inkluderar:

* **Kostnad Analysis** -tillåter molnet kostnader toobe uppdelade tooany interna identifierar dimension (provider, tjänst, konto, region osv.). hello Azure-användning och RateCard APIs du gör detta lättaste genom att tillhandahålla hello mest detaljerade uppdelning av data om användning och kostnader per konto, som sedan grupperade och filtreras efter Cloudyn och presenteras toohello användare i en grafisk eller tabular.

![Kostnad analys cirkeldiagram][3]

* **Kostnad allokering 360** -aktiverar ekonomi och IT-chefer toouncover hello faktiska kostnad uppdelning, drivrutiner och trender för deras distribution. Ytterligare tillåter chefer tooeasily associera distribution utgifter med affärsenheter, avdelningar, regioner och mer att tillhandahålla en oöverträffad insikter om molnet kostnader och underlätta enterprise Återdebiteringar och showbacks. hello Azure-användning och RateCard APIs fungera som indata tooCloudyn kostnaden allokering motorn, som kompletterar hello API: er genom att definiera metoder och affärslogik för tilldelning av ej taggade eller untaggable resurser.

![Kostnad allokering 360-diagram][4]

* **Kostnadseffektiv storlek** -rekommendationer underutnyttjade virtuella datorer, vilket minskar hello kundens utgifter i stora eller över etablerade datorer med rätt storlek. Den gör detta genom att undersöka virtuell processor och RAM mått (via prestanda API), timmars körning (via användning API) och kostnaden (via RateCard API). Cloudyn sedan innehåller rätt storlek för rekommendationer baserat på underutnyttjade CPU eller minne resurser (prestanda) och beräknar uppskattade besparingar genom att multiplicera hello pris delta (RateCard) mellan hello virtuella datorer av hello faktiska tiden-användning (användning) av hello underutnyttjade datorn.

![Kostnadseffektiv storlek][5]

* **Molnet portera rekommendationer** -innehåller finansiella råd om molnet portera. Den undersöker en användares aktuella kostnaderna för molnresurser som har distribuerats på större molnet leverantörer och jämför den toohello kostnaden för en motsvarande distribution på Azure. Den ger detaljerade, per-resurs, ekonomiskt-baserade portera rekommendationer tooAzure. Efter en bedömning av hello motsvarande distribution krävs i Azure (baserat på mått och användaren prestandainställningar) använder Cloudyn hello RateCard API tooevaluate hello kostnaden för motsvarande hello-distribution på Azure.
* **Prestandarapporterna** -aktiveras med Azures prestanda API rapporterna ger en uppsättning funktioner från processor-och RAM toooptimization rekommendationer. Nedan visas instansen användning rapporten exempelvis presentera instans uppdelning av Genomsnittlig CPU-användning.

![Prestandarapporter][6]

* **Kategori manager** -en kraftfull funktion i Cloudyn som låter dig hantera ordning toounorganized molnresurser. Det ger användare hello frihet toocreate sina egna unika kategorier (taggar) för effektiva mätning och rapportering som är i enlighet med organisationens rutiner. Dessutom att användare kan enkelt reglera och kategorisera inkonsekvent Taggning (d.v.s. skrivfel och andra avvikelser) och ej taggade resurser för korrekt kostnad information identifieras automatiskt.

![Kategori Manager][7]

## <a name="video"></a>Video
Här är en kort video som visar hur en Azure-kund kan använda Cloudyn för Azure och hello Azure Billing API: er, toogain insikter från sina data i Azure-förbrukningen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Cloudyn-Provides-Cloud-ITFM-Tools-Via-Microsoft-Azure-APIs/player]
> 
> 

## <a name="next-steps"></a>Nästa steg
* Starta en kostnadsfri [Cloudyn för Azure](https://www.cloudyn.com/microsoft-azure/) utvärderingsversion toosee hur du kan hämta kostnad genomskinlighet med anpassade rapporter och analyser för din Microsoft Azure cloud-distribution.
* Se [få insikter om dina Microsoft Azure-resursförbrukning](billing-usage-rate-card-overview.md) en översikt över hello Azure Resursanvändning och RateCard APIs.
* Kolla in hello [Azure Billing REST API-referens](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) mer information om både API: er som är en del av hello uppsättning API: er som tillhandahålls av hello Azure Resource Manager.
* Om du vill toodive i hello exempelkod checka ut våra Microsoft Azure Billing API kodexempel på [Azure kodexempel](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Läs mer
* toolearn mer om Microsoft Azure Enterprise-avtal (EA)-erbjudanden, besök [licensiering Azure för hello Enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/)
* Se hello [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) artikel toolearn mer om hello Azure Resource Manager.
* Mer information om hello uppsättning med verktyg nödvändiga toohelp i kunna förstå molnet tillbringar finns för Gartner artikel [marknaden Guide för IT finansiella Management (ITFM) verktyg](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
