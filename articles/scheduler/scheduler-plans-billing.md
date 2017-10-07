---
title: "aaaPlans och fakturering i Azure Schemaläggaren"
description: "Planer och fakturering i Azure Schemaläggaren"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 13a2be8c-dc14-46cc-ab7d-5075bfd4d724
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 051b8eeb1ea19678b3cef4db3237ebf04c8b0e13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="plans-and-billing-in-azure-scheduler"></a>Planer och fakturering i Azure Schemaläggaren
## <a name="job-collection-plans"></a>Samlingen Jobbplaner
Jobbsamlingar är hello fakturerbar enhet i Azure Schemaläggaren. Jobbsamlingar innehåller ett antal jobb och kommer i tre planer – lediga, Standard och Premium – som beskrivs nedan.

| **Jobbet Samlingsplanen** | **Max antal jobb per Jobbsamlingen** | **Max upprepning** | **Max Jobbsamlingar per prenumeration** | **Begränsningar** |
|:--- |:--- |:--- |:--- |:--- |
| **Ledigt** |5 jobb per jobbsamlingen |En gång i timmen. Det går inte att köra jobb oftare än en gång i timmen |En prenumeration tillåts upp too1 fri jobbsamling |Det går inte att använda [HTTP utgående auktorisering objekt](scheduler-outbound-authentication.md) |
| **Standard** |50 jobb per jobbsamlingen |En gång per minut. Det går inte att köra jobb oftare än en gång i minuten |En prenumeration tillåts upp too100 standard jobbsamlingar |Åtkomst toofull funktionsuppsättningen Schemaläggaren |
| **P10 Premium** |50 jobb per jobbsamlingen |En gång per minut. Det går inte att köra jobb oftare än en gång i minuten |En prenumeration tillåts upp too10 000 P10 Premium jobbsamlingar. <a href="mailto:wapteams@microsoft.com">Kontakta oss</a> mer information. |Åtkomst toofull funktionsuppsättningen Schemaläggaren |
| **P20 Premium** |1000 jobb per jobbsamlingen |En gång per minut. Det går inte att köra jobb oftare än en gång i minuten |En prenumeration tillåts upp too10 000 P20 Premium jobbsamlingar. <a href="mailto:wapteams@microsoft.com">Kontakta oss</a> mer information. |Åtkomst toofull funktionsuppsättningen Schemaläggaren |

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Uppgraderingar och Downgrades av jobbet Samlingsplaner
Du kan uppgradera eller nedgradera ett jobb samlingsplanen när som helst bland hello lediga, Standard och Premium-planer. När nedgradera tooa fri jobbsamling misslyckas hello nedgradering av något av följande orsaker hello:

* En fri jobbsamling redan finns i hello prenumeration
* Ett jobb i jobbsamlingen hello har en upprepning som högre än det tillåtna jobben kostnadsfria jobbsamlingar. hello största antal upprepningar som tillåts i en fri jobbsamling är en gång i timmen
* Det finns mer än 5 jobb i jobbsamlingen hello
* Ett jobb i jobbsamlingen hello har en åtgärd för HTTP eller HTTPS som använder en [HTTP utgående auktorisering objekt](scheduler-outbound-authentication.md)

## <a name="billing-and-azure-plans"></a>Fakturerings- och Azure-planer
Prenumerationer debiteras inte gratis jobbsamlingar. Om du har fler än 100 standard jobbsamlingar (10 fakturering standardenheter), så är det en bättre systembearbetningstid toohave jobbet alla samlingar i hello premiumplan.

Om du har en standard jobbsamlingen och en premium jobbsamlingen är du fakturerade en standard fakturering enhet *och* en premium fakturering enhet. hello Scheduler-tjänsten växlar baserat på hello antal aktiva jobbsamlingar som är inställda tooeither standard eller premium; Detta beskrivs närmare hello följande två avsnitt.

## <a name="standard-billable-units"></a>Standardenheter för fakturerbar tid
En standard fakturerbar enhet kan innehålla upp too10 standard jobbsamlingar. Eftersom en standard jobbsamling kan ha too50 jobb per jobbsamlingen, kan en standard fakturering enhet en prenumeration toohave too500 jobb – in tooalmost 22 miljoner jobbet körningar per månad.

Om du har mellan 1 och 10 standard jobbsamlingar faktureras du för 1 standard fakturering enhet. Om du har mellan 11 och 20 standard jobbsamlingar faktureras du för 2 fakturering standardenheter. Om du har mellan 21 och 30 standard jobbsamlingar du faktureras för 3 fakturering standardenheter och så vidare.

## <a name="p10-premium-billable-units"></a>P10 Fakturerbar, Premiumenheter
En fakturerbar enhet P10 premium kan innehålla upp too10 000 P10 premium jobbsamlingar. Eftersom en P10 premium jobbsamling kan ha too50 jobb per jobbsamlingen, kan en premium fakturering enhet en prenumeration toohave in too500 000 jobb – in tooalmost 22 miljarder jobbet körningar per månad.

Om du har mellan 1 och 10 000 premium jobbsamlingar faktureras du för 1 P10 premium fakturering enhet. Om du har mellan 10,001 och 20 000 premium jobbsamlingar du faktureras för 2 P10 fakturering, premiumenheter och så vidare.

Därför hello P10 premium jobbet samlingar har samma funktion som hello standard jobbsamlingar men ett pris avbrott om ditt program kräver mycket jobbsamlingar.

## <a name="p20-premium-billable-units"></a>P20 Fakturerbar, Premiumenheter
En fakturerbar enhet P20 premium kan innehålla upp too5 000 P20 premium jobbsamlingar. Eftersom en P20 premium jobbsamling kan ha upp too1 000 jobb per jobbsamlingen, kan en premium fakturering enhet en prenumeration toohave in too5 000, 000 jobb – in tooalmost 220 miljarder jobbet körningar per månad.

P20 premium jobbsamlingar ger hello samma funktioner som P10 premium jobbsamlingar men även stöder ett större antal jobb per jobbsamlingen och en större totala antalet jobb övergripande än P10 premium vilket gör att du toohave mer skalbarhet.

## <a name="billing-and-active-status"></a>Fakturerings- och aktiv Status
Jobbsamlingar är alltid aktivt om din hela prenumerationen har gått till vissa temporära inaktiverat tillstånd på grund av problem med toobilling. hello endast sätt tooensure att en jobbsamling inte faktureras är tooeither ange toohello *lediga* plan eller toodelete hello jobbsamlingen.

Men du kan inaktivera alla jobb i en jobbsamling i en enda åtgärd, ändras inte hello fakturering status för hello jobbsamlingen – hello jobbsamlingen kommer *fortfarande* debiteras. På samma sätt tom jobbsamlingar betraktas som aktiv och kommer att debiteras.

## <a name="pricing"></a>Prissättning
Information om priser, se [Scheduler priser](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Se även
 [Vad är Scheduler?](scheduler-intro.md)

 [Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler](scheduler-concepts-terms.md)

 [Komma igång med Schemaläggaren i hello Azure-portalen](scheduler-get-started-portal.md)

 [Referens för REST-API:et för Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Referens för PowerShell-cmdlets för Azure Scheduler](scheduler-powershell-reference.md)

 [Hög tillgänglighet och tillförlitlighet i Azure Scheduler](scheduler-high-availability-reliability.md)

 [Gränser, standardinställningar och felkoder i Azure Scheduler](scheduler-limits-defaults-errors.md)

 [Utgående autentisering i Azure Scheduler](scheduler-outbound-authentication.md)

