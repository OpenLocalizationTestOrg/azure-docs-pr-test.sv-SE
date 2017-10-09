---
title: "aaaUnderstand fakturan för Azure | Microsoft Docs"
description: "Lär dig hur tooread och förstå din användning och faktura för din Azure-prenumeration"
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/14/2017
ms.author: tonguyen
ms.openlocfilehash: a3195eb129b1576e8cb665aa6f88a1a2647edd78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-your-bill-for-microsoft-azure"></a>Förstå fakturan för Microsoft Azure
toounderstand din Azure debiterar, jämför fakturan med hello detaljerad daglig användning fil- och hello kostnad rapporter i hello Azure-portalen.

tooobtain en PDF-fil på fakturan och en kopia av din detaljerad daglig användning CSV Filhämtning, se [hämta ditt Azure billing faktura och dagligen användningsdata](billing-download-azure-invoice-daily-usage-date.md). 

Detaljerade villkor och beskrivningar av din faktura och detaljerad daglig användning fil finns [Förstå villkoren på fakturan Microsoft Azure](billing-understand-your-invoice.md) och [förstå villkor i Microsoft Azure detaljerad användning](billing-understand-your-usage.md). 

Mer information om hello kostnad rapporter finns [Azure-portalen kostnadshanteringen](https://docs.microsoft.com/en-us/azure/billing/billing-getting-started).


## <a name="charges"></a>Hur kan jag vara säker på att hello tillägg i min faktura är korrekt?
Om en avgift på fakturan som du vill ha mer information om finns det ett par olika alternativ.

### <a name="option-1-review-your-invoice-and-compare-hello-usage-and-costs-with-hello-detailed-usage-csv-file"></a>Alternativ 1: Granska din faktura och jämför hello användning och kostnader med hello detaljerad användning CSV-fil

hello visar detaljerad användning CSV-fil dina avgifter per faktureringsperiod och daglig användning. tooget din detaljerad användning CSV-fil finns [hämta ditt Azure billing faktura och dagligen användningsdata](https://docs.microsoft.com/en-us/azure/billing/billing-download-azure-invoice-daily-usage-date).

Avgifter för användning visas på hello mätaren nivå. hello: följande villkor hello samma sak i båda hello faktura och hello detaljerad användning filen. Hello fakturering cykel på hello faktura är till exempel motsvarande toohello faktureringsperiod visas i hello detaljerad användning filen.

 | Fakturan (PDF) | Detaljerad användning (CSV)|
 | --- | --- |
|Faktureringscykel | Faktureringsperiod |
 |Namn |Mätarkategori |
 |Typ |Mätaren underkategorin |
 |Resurs |Mätarnamn |
 |Region |Mätarregion |
 |Förbrukad |Förbrukat antal |
 |Ingår |Inkluderad mängd |
 |Faktureringsbar |Överbliven kvantitet |

Hej **Användningskostnader** avsnitt på fakturan har hello totala värde för varje mätaren förbrukades under din faktureringsperioden. Till exempel hello följande skärmbild visar ett användning kostnad för hello Azure Schemaläggaren.

![Avgifterna för användning av faktura](./media/billing-understand-your-bill/1.png)

Hej **instruktionen** avsnitt av din detaljerad användning CSV visar hello samma kostnad. Båda hello *förbrukad* belopp och *värdet* matchar hello faktura.

![Avgifterna för användning av CSV](./media/billing-understand-your-bill/2.png)

toosee en uppdelning av denna avgift dagligen, gå toohello **daglig användning** avsnitt i hello CSV. Filtrera efter ”Scheduler” *mätaren kategori* och du kan se vilka dagar hello mätaren användes och hur mycket har förbrukats. Hej *resurs* och *resursgruppen* informationen visas också för jämförelse. Hej *förbrukad* värden ska lägga ihop toowhat's visas på fakturan hello.

![Daglig användning avsnitt hello CSV](./media/billing-understand-your-bill/3.png)

tooget hello kostnad per dag, multiplicera hello *förbrukad* med hello *hastighet* värde från hello **instruktionen** avsnitt.

toolearn mer om hello fakturan finns [förstå fakturan Azure](billing-understand-your-invoice.md).

toolearn om varje hello kolumner i hello CSV, se [förstå din Azure detaljerad användning](billing-understand-your-invoice.md).

### <a name="option-2-review-your-invoice-and-compare-with-hello-usage-and-costs-in-hello-azure-portal"></a>Alternativ 2: Granska din faktura och jämföra med hello användning och kostnader i hello Azure-portalen

hello Azure-portalen kan också hjälpa dig att kontrollera dina debiteringar. hello Azure-portalen innehåller kostnaden management diagram för en snabb överblick över dina användnings- och hello avgifter på fakturan.

toocontinue med hello-exemplet ovan, besök hello [prenumerationssidan](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), väljer din prenumeration och välj sedan **kostnad analysis**. Därifrån kan du ange tidsintervallet för hello och visas syntax för hello Azure Schemaläggaren.

![Visa kostnad analys i Azure-portalen](./media/billing-understand-your-bill/4.png)

toosee hello sammanställning per dag kostnaden i **kostnad historik**, klicka på hello rad.

![Kostnad historikvyn i Azure-portalen](./media/billing-understand-your-bill/5.png)

Det finns fler toolearn [förhindrar oväntade kostnader med Azure fakturering och hantering av kostnaden](billing-getting-started.md#costs).

## <a name="external"></a>Nyheter om externa serviceavgifter?
Externa tjänster (även kallat Azure Marketplace order) tillhandahålls av oberoende tjänsteleverantörer och faktureras separat. hello avgifter visas inte på fakturan Azure. Det finns fler toolearn [förstå Azure externa avgifterna](billing-understand-your-azure-marketplace-charges.md).

## <a name="payment"></a>Hur gör jag en betalning

Om du har ställt in ett kreditkort eller en bankkort som betalningsmetoden debiteras hello betalning automatiskt inom 10 dagar efter hello faktureringsperioden tar slut. På ditt kreditkort instruktionen hello radobjekt säger **MSFT Azure**.

Om du [betala av fakturering](billing-how-to-pay-by-invoice.md), skicka din betalning toohello plats som anges i hello längst ned på fakturan. Mer hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="how-do-i-check-hello-status-of-a-payment-made-by-credit-card"></a>Hur kontrollerar hello status för en betalning kreditkort?

[Skapa ett supportärende](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooask hello status för din betalning. 

## <a name="tips-for-cost-management"></a>Tips för hantering av kostnad
- Uppskatta kostnader genom att använda hello [prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/calculator/) och [totalkostnad för ägarskap Kalkylatorn](https://aka.ms/azure-tco-calculator), och få hello [utförlig prisinformation för varje tjänst](https://azure.microsoft.com/en-us/pricing/).
- [Ställ in fakturering aviseringar](billing-set-up-alerts.md).
- [Granska din användning och kostnader regelbundet i hello Azure-portalen](billing-getting-started.md#costs).

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.

Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.
