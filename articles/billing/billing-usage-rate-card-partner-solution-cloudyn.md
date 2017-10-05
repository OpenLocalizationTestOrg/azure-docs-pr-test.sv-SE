---
title: "Microsoft Azure-användning och RateCard API: er aktivera Cloudyn för att ge ITFM för kunder | Microsoft Docs"
description: "Ger ett unikt perspektiv från Microsoft Azure Billing partner Cloudyn, på deras upplevelser integrera Azure Billing API: erna i sin produkt.  Detta är särskilt användbart för Azure och Cloudyn kunder som vill använda/försök Cloudyn för Azure-tjänster."
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
ms.openlocfilehash: fac0ee2e9cbc87c8b3d04675551bba61f7a532b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>Microsoft Azure-användning och RateCard API: er aktivera Cloudyn att ge ITFM för kunder
Cloudyn, en Microsoft-partner för utveckling och en inledande moln hanteringsfunktioner,-provider har valts för en privat förhandsgranskning av nya resursanvändningen för Microsoft Azure och RateCard APIs.  Användning-API ger åtkomst till uppskattade Azure förbrukningen för en prenumeration. RateCard API ger fullständig prisinformation för alla Azure-tjänster, för icke - Enterprise-avtal-EA-kunder. Integrerad tillsammans, underlag API: erna fullständig information för indata i IT finansiella Management (ITFM) verktyg som de som tillhandahålls av Cloudyn.

## <a name="introduction"></a>Introduktion
Så kallade ”multiplikation” data från API: et för användning med data från RateCard API (användning [enheter] pris [$unit] = detaljerad användning och kostnader) skapar den mest detaljerade, korrekt och tillförlitlig faktureringsinformation tillgänglig för Azure idag.

![Översikt över ITFM][1]

Använda dessa API: er innehåller viktig information om kunders användning och kostnader, vilket gör att Cloudyn att analysera kundkonton på ett enkelt och programmässiga sätt och för att utföra olika uppgifter för ITFM för sina kunder.

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>Integrera Cloudyn med RateCard och användning av API: er
RateCard API kräver flera indataparametrar--som region info, valuta och språk -, men den viktigaste är OfferDurableID som anger vilken typ av Azure erbjuder kunden använder (betala per användning, äldre 6 och 12 månaders åtagande planer MSDN erbjuder, MPN erbjudanden, specialerbjudanden och andra). OfferDurableID finns i den [Azure Usage and Billing portal](https://account.windowsazure.com/Subscriptions), under ”erbjuder ID” för den angivna prenumerationen.

Vid registreringen för [Cloudyn för Azure](https://www.cloudyn.com/microsoft-azure/) tjänster, kunder kan lägga till sina OfferDurableID-kod, vilket gör att Cloudyn och hämtar relevanta prisinformation via RateCard-API.  Information om de olika typerna av erbjudanden finns en den [Erbjudandeinformationen för Microsoft Azure](https://azure.microsoft.com/support/legal/offer-details/) sidan.

![Översikt över Cloudyn ITFM-motorn][2]

Cloudyn använder användnings- och RateCard APIs utöver Azure prestanda API för att skapa ytterligare skyddslager för visualisering, analytics, varningar, rapportering, kostnad hantering och tillämplig rekommendationer ger Azure kunderna en tillförlitlig Enterprise cloud ITFM verktyg.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Cloudyn ITFM användningsfall aktiveras med användnings- och RateCard API-integrering
Vanliga Cloudyn ITFM användningsfall aktiveras med användning och RateCard APIs inkluderar:

* **Kostnad Analysis** -tillåter molnet kostnader fördelas någon inbyggd identifierande dimension (provider, tjänst, konto, region osv.). Azure-användning och RateCard APIs gör detta lättaste genom att tillhandahålla den mest detaljerade uppdelningen av användning och kostnaden data per konto, vilket sedan grupperade och filtreras efter Cloudyn och visas för användaren i en grafisk eller tabell.

![Kostnad analys cirkeldiagram][3]

* **Kostnad allokering 360** -aktiverar ekonomi och IT-chefer att upptäcka verklig kostnadsuppdelning, drivrutiner och trender för deras distribution. Det gör att ytterligare hanterare för att associera distribution utgifter med affärsenheter, avdelningar, regioner och mer att tillhandahålla en oöverträffad insikter om molnet kostnader och underlätta enterprise Återdebiteringar och showbacks. Azure-användning och RateCard APIs fungera som indata till Cloudyns kostnaden allokering motorn som kompletterar API: erna genom att definiera metoder och affärslogik för tilldelning av ej taggade eller untaggable resurser.

![Kostnad allokering 360-diagram][4]

* **Kostnadseffektiv storlek** -rekommendationer underutnyttjade virtuella datorer, vilket minskar kundens utgifter i stora eller över etablerade datorer med rätt storlek. Den gör detta genom att undersöka virtuell processor och RAM mått (via prestanda API), timmars körning (via användning API) och kostnaden (via RateCard API). Cloudyn sedan innehåller rätt storlek för rekommendationer baserat på underutnyttjade CPU eller minne resurser (prestanda) och beräknar uppskattade besparingar genom att multiplicera pris delta (RateCard) mellan de virtuella datorerna av den faktiska tid-användningen (användning) av den underutnyttjade datorn.

![Kostnadseffektiv storlek][5]

* **Molnet portera rekommendationer** -innehåller finansiella råd om molnet portera. Den undersöker en användares aktuella kostnaderna för molnresurser som har distribuerats på större molnet leverantörer och jämför den med kostnaden för en motsvarande distribution på Azure. Den ger detaljerade, per-resurs, ekonomiskt-baserade portera rekommendationer till Azure. Efter en bedömning av motsvarande distributionen krävs i Azure (baserat på mått och användaren prestandainställningar) använder Cloudyn RateCard API för att utvärdera kostnaden för motsvarande distributionen på Azure.
* **Prestandarapporterna** -aktiveras med Azures prestanda API rapporterna innehåller en uppsättning funktioner från processor-och RAM optimering rekommendationer. Nedan visas instansen användning rapporten exempelvis presentera instans uppdelning av Genomsnittlig CPU-användning.

![Prestandarapporter][6]

* **Kategori manager** -en kraftfull funktion i Cloudyn som ger ordning för relativt oorganiserad molnresurser. Den ger användarna friheten att skapa sina egna unika kategorier (taggar) för effektiva mätning och rapportering som är i enlighet med organisationens rutiner. Dessutom att användare kan enkelt reglera och kategorisera inkonsekvent Taggning (d.v.s. skrivfel och andra avvikelser) och ej taggade resurser för korrekt kostnad information identifieras automatiskt.

![Kategori Manager][7]

## <a name="video"></a>Video
Här är en kort video som visar hur en Azure-kund kan använda Cloudyn för Azure och Azure Billing API: erna, och få insikter från sina data i Azure-förbrukningen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Cloudyn-Provides-Cloud-ITFM-Tools-Via-Microsoft-Azure-APIs/player]
> 
> 

## <a name="next-steps"></a>Nästa steg
* Starta en kostnadsfri [Cloudyn för Azure](https://www.cloudyn.com/microsoft-azure/) utvärderingsversionen för att se hur du kan hämta kostnad genomskinlighet med anpassade rapporter och analyser för din Microsoft Azure cloud-distribution.
* Se [få insikter om dina Microsoft Azure-resursförbrukning](billing-usage-rate-card-overview.md) för en översikt över Azure Resursanvändning och RateCard APIs.
* Kolla in den [Azure Billing REST API-referens](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) mer information om både API: er som är en del av en uppsättning API: er som tillhandahålls av Azure Resource Manager.
* Om du vill sätta igång direkt i exempelkoden checka ut våra Microsoft Azure Billing API kodexempel på [Azure kodexempel](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Läs mer
* Om du vill veta mer om Microsoft Azure Enterprise-avtal (EA)-erbjudanden kan du besöka [licensiering Azure för företaget](https://azure.microsoft.com/pricing/enterprise-agreement/)
* Finns det [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) artikeln om du vill veta mer om Azure Resource Manager.
* Mer information om uppsättning med verktyg som är nödvändiga för att bidra till att få en förståelse av molnet tillbringar, se Gartner artikeln [marknaden Guide för IT finansiella Management (ITFM) verktyg](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
