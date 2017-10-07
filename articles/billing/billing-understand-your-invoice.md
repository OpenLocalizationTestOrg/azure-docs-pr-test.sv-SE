---
title: aaaUnderstand din Azure fakturerar | Microsoft Docs
description: "Lär dig hur tooread och förstå hello användning och faktura för din Azure-prenumeration"
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: tonguyen
ms.openlocfilehash: d7629b448fe48115ae1f7c4f462c82a3f03e3784
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-terms-on-your-microsoft-azure-invoice"></a>Förstå villkor på fakturan Microsoft Azure
hello fakturan innehåller en sammanfattning av dina debiteringar och innehåller instruktioner för betalning. Den är tillgänglig för nedladdning i hello Portable Document Format (PDF) från hello [Azure-portalen](https://portal.azure.com/) eller kan skickas via e-post. Mer information finns i [hur tooget Azure faktureringen faktura och dagliga användningsdata](billing-download-azure-invoice-daily-usage-date.md).

Några saker toonote:

-   Om du använder en kostnadsfri utvärderingsprenumeration får du information om din detaljerad användning i hello Azure-portalen, men du har inte en faktura.

-   In too24 timmars användning hello slutet av hello kan tidigare faktureringsperiod visas i din aktuella fakturan.

-   Avgifterna som visas på fakturering instruktioner för internationella kunder är endast för beräkning av. Banker kan ha olika kostnader för konvertering priser.

## <a name="detailed-terms-and-descriptions-of-your-invoice"></a>Detaljerade villkor och beskrivningar av fakturan
hello listar följande avsnitt hello viktiga termer som du ser på din faktura och beskrivningar av varje term.

### <a name="account-information"></a>Kontoinformation

hello-konto i avsnittet information i hello faktura är hello längst upp på den första sidan som visar information om din profil och en prenumeration.

![Kontot i avsnittet information i faktura](./media/billing-understand-your-invoice/1.png)

| Period | Beskrivning |
| --- | --- |
| Kundens Inköpsordernummer |Ett valfritt inköpsordernummer som tilldelats av du för spårning |
| Fakturanr |Ett unikt, Microsoft genererade fakturan tal som används i spårningssyfte |
| Faktureringscykel |Datumintervall som omfattar den här fakturan |
| Fakturadatum |Datum att hello fakturan har skapats, vanligtvis en dag efter slutet av hello fakturering cykel |
| Betalningsmetod |Typen av betalning som används på hello kontot (faktura eller kreditkort) |
| Struktur för|Faktureringsadress som anges för hello-konto |
| Prenumerationen erbjuder (”betala per användning”) |Typ av prenumerationserbjudande som har köpts (betala per användning, BizSpark Plus, Azure-Pass, etc.). Mer information finns i [Azure erbjudandetyper](https://azure.microsoft.com/support/legal/offer-details/). |
| E-postadress till kontoägaren | hello kontos e-postadress som Microsoft Azure-konto för hello är registrerad under. <br /><br />toochange hello e-postadress finns [hur toochange profilinformation i ditt Azure-konto som kontaktens e-post, adress och telefonnummer](billing-how-to-change-azure-account-profile.md). |

### <a name="understand-hello-invoice-summary"></a>Förstå hello faktura sammanfattning
Hej **fakturasammanfattning** avsnittet av hello faktura innehåller hello total transaktionsbelopp sedan den senaste faktureringsperioden och dina aktuella användningsavgifter.

![Fakturan sammanfattningen](./media/billing-understand-your-invoice/2.png)

hello prenumerationsnamn (”produktion lagring”) är hello namnet på prenumerationen för den här fakturan.

#### <a name="understand-hello-previous-charges"></a>Förstå hello tidigare avgifter
Hej tidigare, betalningar och utestående saldo avsnittet på fakturan sammanfattas transaktioner sedan den senaste faktureringsperioden.

| Period | Beskrivning |
| --- | --- |
| Föregående saldo |hello totalbelopp förfaller från din senaste faktureringsperioden |
| Betalningar |Totalt antal betalningar och krediter tillämpas tooyour senaste faktureringsperioden |
| Utestående saldo (från föregående faktureringsperiod) |Alla krediter eller återstoden i ditt konto sedan den senaste faktureringsperioden |

#### <a name="understand-hello-current-charges"></a>Förstå hello aktuella avgifter
hello aktuella avgifter delen av hello faktura visas information om dina månatliga avgifter för hello aktuella fakturering period.

| Period | Beskrivning |
| --- | --- |
| Avgifter för användning |Avgifter för användning är hello totala månatliga avgifter för en prenumeration för hello aktuella fakturering period|
| Rabatter |Tjänsten rabatter tooyour aktuella fakturering period|
| Justeringar |Diverse krediter (ledigt användning, krediter osv.) eller utestående avgifter tillämpas tooyour aktuella fakturering period.<br/><br/>Om du har hello Visual Studio Enterprise med MSDN erbjudande finns till exempel en månadskredit. Om du avbryter prenumerationen finns månatliga användningskostnader som överskrider hello månadskredit som du får med erbjudandet prenumeration. hello avgifter innebära hello början av den aktuella faktureringsperioden tills hello prenumeration annulleringsdatum. |

#### <a name="sold-tooand-payment-instructions"></a>Säljs tooand betalningsinstruktioner

hello beskrivs följande tabell hello säljs tooand betalning anvisningarna som visas på hello andra sidan på fakturan.

| Period |Beskrivning |
| --- | --- |
| Säljs för|Profil-adressen som finns på hello-konto. <br/><br/>Om du behöver toochange hello adress finns [hur toochange profilinformation i ditt Azure-konto som kontaktens e-post, adress och telefonnummer](billing-how-to-change-azure-account-profile.md).|
| Betalningsanvisningar |Instruktioner om hur toopay beroende på betalningsmetod (exempelvis med kredit kort eller av en faktura). |

#### <a name="usage-charges"></a>Användningsdebitering

hello användning avgifter avsnitt i hello faktura visar mätaren nivåinformation på dina debiteringar.

![Användning avgifter avsnitt](./media/billing-understand-your-invoice/3.png)

hello beskrivs följande tabell hello användning avgifter kolumnrubriker visas på fakturan.

| Period |Beskrivning |
| --- | --- |
| Namn |Identifierar hello översta tjänst för hello användning |
| Typ |Definierar hello Azure service-typen som kan påverka hello hastighet |
| Resurs |Identifierar hello enheten för hello mätaren används |
| Region |Identifierar hello platsen för hello datacenter för vissa tjänster som är mest baserat på plats för datacenter |
| Förbrukad |hello mängden hello mätaren används under hello faktureringsperioden |
| Ingår |hello mängden hello mätaren som ingår kostnadsfritt i din aktuella faktureringsperioden |
| Faktureringsbar |Visar hello skillnaden mellan hello används kvantitet och hello ingår kvantitet. Det är debiteras för den här mängden. För betala per användning erbjudanden utan belopp som ingår i erbjudandet hello är denna summa hello samtidigt som hello förbrukad kvantitet |
| Pris |hello hastigheten med vilken du är debiteras per fakturerbar enhet |
| Värde |Visar hello resultatet av att hello överförbrukning kvantitet kolumn efter hello hastighet kolumn. Om hello används kvantitet inte överstiga hello ingår kvantitet, är gratis i den här kolumnen. |
| Delsumma |hello summan av alla dina kostnader före skatt för denna faktureringsperiod |
| Totalsumma |hello summan av alla kostnader efter skatt för denna faktureringsperiod |

## <a name="how-do-i-make-sure-that-hello-charges-in-my-invoice-are-correct"></a>Hur kan jag vara säker på att hello tillägg i min faktura är korrekt?
Om det finns en avgift på fakturan som du vill ha mer information på, se [förstå fakturan för Microsoft Azure.](billing-understand-your-bill.md)

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.
