---
title: "aaaUnderstand din Azure detaljerad användning | Microsoft Docs"
description: "Lär dig hur tooread och förstå hello avsnitt i din detaljerad användning CSV för din Azure-prenumeration"
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
ms.openlocfilehash: c9284bf94bfa9f36cdd5d39e653a35def7c1aa34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-terms-on-your-microsoft-azure-detailed-usage-charges"></a>Förstå villkor för din Microsoft Azure användningsdebitering 
Hej detaljerad användning avgifter CSV-filen innehåller varje dag och mätaren nivå användningsdebiteringen för hello aktuella fakturering period. 

tooget filen detaljerad användning finns [hur tooget Azure faktureringen faktura och dagliga användningsdata](billing-download-azure-invoice-daily-usage-date.md).
Den är tillgänglig i en fil med kommaavgränsade värden (CSV)-filformat som kan öppnas i ett kalkylbladsprogram. Om du ser två versioner som är tillgängliga, hämta version 2. Det är hello mest aktuella filformat.

Avgifter för användning är hello totala **månatliga** avgifter för en prenumeration. Avgifter för användning beakta inte eventuella krediter eller rabatter.

## <a name="detailed-terms-and-descriptions-of-your-detailed-usage-file"></a>Detaljerade villkor och beskrivningar av filen detaljerad användning
hello beskrivs följande avsnitt hello viktiga termer som visas i version 2 av hello detaljerad användning filen.

### <a name="statement"></a>Instruktionen
hello längst upp i hello detaljerad användning CSV visar hello Filtjänster som du använde under hello månad faktureringsperioden. hello följande tabell visas hello termer och beskrivningar som visas i det här avsnittet.

| Period | Beskrivning |
| --- | --- |
|Faktureringsperiod |hello faktureringsperiod när hello mätare användes |
|Mätarkategori |Identifierar hello översta tjänst för hello användning |
|Mätarens underkategori |Definierar hello Azure-tjänst som kan påverka hello hastighet |
|Mätarnamn |Identifierar hello enheten för hello mätaren används |
|Mätarregion |Identifierar hello platsen för hello datacenter för vissa tjänster som är mest baserat på plats för datacenter |
|SKU |Identifierar hello unika system-ID för varje Azure-mätare |
|Enhet |Identifierar hello enhet som hello service debiteras i. Till exempel GB, timmar, 10 000 s. |
|Förbrukat antal |hello mängden hello mätaren används under hello faktureringsperioden |
|Inkluderad mängd |hello mängden hello mätaren som ingår kostnadsfritt i din aktuella faktureringsperioden |
|Överbliven kvantitet |Visar hello skillnaden mellan hello används kvantitet och hello ingår kvantitet. Det är debiteras för den här mängden. För betala per användning erbjudanden med ingen ingår kvantitet hello erbjudande är samma som hello används kvantitet hello denna summa. |
|Inom åtagande |Visar hello mätaren avgifter som dras från din beloppet som är associerade med 6 eller 12 månaders erbjudandet. Mätaren avgifter subtraheras i kronologisk ordning. |
|Valuta |hello valutan som används i din aktuella faktureringsperioden |
|Överanvändning |Visar hello mätaren avgifter som överskrider din beloppet som är associerade med erbjudandet 6 eller 12 månader |
|Åtagandepris |Visar hello åtagande som baseras på hello totala beloppet som är associerade med erbjudandet 6 eller 12 månader |
|Pris |hello hastigheten med vilken du är debiteras per fakturerbar enhet |
|Värde |Visar hello resultatet av att hello överförbrukning kvantitet kolumn efter hello hastighet kolumn. Om hello används kvantitet inte överstiga hello ingår kvantitet, är gratis i den här kolumnen. |

### <a name="daily-usage"></a>Daglig användning

hello daglig användning avsnitt i hello CSV-fil visar användningsinformation som påverkar hello fakturerbara kostnader. hello följande tabell visas hello termer och beskrivningar som visas i det här avsnittet.

| Period | Beskrivning |
| --- | --- |
|Datum för användning |hello datum när hello mätaren användes |
|Mätarkategori |Identifierar hello översta tjänst som tillhör denna användning |
|Mätar-ID |hello debiteras mätaren identifierare som har använt tooprice fakturering användning |
|Mätarens underkategori |Definierar hello Azure service-typen som kan påverka hello hastighet |
|Mätarnamn |Identifierar hello enheten för hello mätaren används |
|Mätarregion |Identifierar hello platsen för hello datacenter för vissa tjänster som är mest baserat på plats för datacenter |
|Enhet |Identifierar hello enhet som hello mätaren debiteras i. Till exempel GB, timmar, 10 000 s. |
|Förbrukat antal |hello mängden hello mätaren som har förbrukats för den dagen |
|Resursplats |Identifierar hello datacenter där hello mätaren körs |
|Förbrukad tjänst |hello Azure-plattformstjänsten som du använde |
|Resursgrupp |hello resursgrupp i vilka hello distribuerade mätaren körs i. <br/><br/>Mer information finns i [Översikt över Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
|Instans-ID | hello identifierare för hello mätaren. <br/><br/> hello identifierare innehåller hello namn som du anger för hello mätaren när den skapades. Det är antingen hello namnet på hello resurs eller hello ett fullständigt resurs-ID. Mer information finns i [Azure Resource Manager API](https://docs.microsoft.com/rest/api/resources/resources). |
|Taggar | Taggen som du tilldelar toohello mätaren. Använd taggar toogroup fakturering poster.<br/><br/>Du kan till exempel använda taggar toodistribute kostnader för av hello avdelningen som använder hello mätaren. Tjänster som stöder ljusavgivande taggar är virtuella datorer, lagring och nätverkstjänster som etablerats med hjälp av hello [Azure Resource Manager API](https://docs.microsoft.com/rest/api/resources/resources). Mer information finns i [ordna dina Azure-resurser med taggar](http://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/). |
|Ytterligare info |Tjänstspecifika metadata. Till exempel en bildtyp för en virtuell dator. |
|Serviceinfo 1 |hello projektnamn som hello tjänsten tillhör tooon din prenumeration |
|Serviceinfo 2 |Äldre fält som samlar in valfria tjänstspecifika metadata |

## <a name="how-do-i-make-sure-that-hello-charges-in-my-detailed-usage-file-are-correct"></a>Hur kan jag vara säker på att hello avgifter i filen detaljerad användning är rätt?
Om en avgift på detaljerad användning filen som du vill ha mer information på Se [förstå fakturan för Microsoft Azure.](./billing-understand-your-bill.md)

## <a name="external"></a>Nyheter om externa serviceavgifter?
Externa tjänster (även kallat marknadsplatsorder) tillhandahålls av oberoende tjänsteleverantörer och faktureras separat. hello avgifter visas inte på hello Azure faktura. Det finns fler toolearn [förstå Azure externa avgifterna](billing-understand-your-azure-marketplace-charges.md).

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?) få snabbt lösa problemet.
