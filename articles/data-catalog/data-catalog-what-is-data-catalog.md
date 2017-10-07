---
title: aaaIntroduction tooAzure Data Catalog | Microsoft Docs
description: "Den här artikeln innehåller en översikt över Microsoft Azure Data Catalog, inklusive dess funktioner och hello problem den adresser. Data Catalog gör att alla användare tooregister, identifiera, förstå och använda datakällor."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: cc733907-17ec-4153-9f0c-5b3754b2db19
ms.service: data-catalog
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 82144c440b5692d3608af08208f36ee8e6dfdc93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-data-catalog"></a>Vad är Azure Data Catalog?
Azure Data Catalog är en helt hanterad molntjänst vars användare kan identifiera hello datakällor de behöver och förstå hello datakällorna de identifierar. Vid hello samma tid, Data Catalog hjälper organisationer get mer värde av sina befintliga investeringar. 

Med Data Catalog kan alla användare (analytiker, IT-forskare eller utvecklare) identifiera, förstå och använda datakällor. Data Catalog innehåller en gemensam modell för metadata och kommentarer. Det är en gemensam, central plats för alla organisationens användare toocontribute sina kunskaper och skapa en databaserad gemenskap och kultur för data.

## <a name="discovery-challenges-for-data-consumers"></a>Det är svårt för datakonsumenterna att hitta rätt
Traditionellt har man identifierat företagets datakällor genom en organisk process baserad på gruppens kunskaper. Den här metoden medför stora utmaningar för företag som vill tooget hello så mycket som möjligt av sina informationstillgångar:

* Användare kan vara omedvetna om att en datakälla finns om de stöter på den som en del av en annan process. Det finns ingen central plats där datakällorna registreras.
* Om användarna vet hello platsen för en datakälla, kan inte de ansluta toohello data med hjälp av ett klientprogram. Data-förbrukning upplevelser kräver användare tooknow hello anslutningssträngen eller sökvägen.
* Om användarna vet hello platsen för en datakälla dokumentation, förstår inte de hello avsedd använder hello data. Datakällor och dokumentation kanske befinner sig på olika platser och används via en mängd olika upplevelser.
* Om användare har frågor om en informationstillgång måste de hitta hello experten eller teamet som ansvarar för hello data och Använd dem offline. Det finns ingen direkt koppling mellan data och personer med expertkunskaper om hur de används.
* Om inte användare att förstå hello processen för att begära åtkomst toohello datakällan, hjälper identifiera hello datakälla och dess dokumentation ändå inte dem åtkomst till hello data.

## <a name="discovery-challenges-for-data-producers"></a>Det är svårt för dataproducenterna att hitta rätt
Även om data konsumenter ansikte hello tidigare nämnts utmaningar användare som ansvarar för att producera och upprätthålla informationstillgångar står inför andra svårigheter sina egna:

* Det är ofta en onödig ansträngning att kommentera datakällor med beskrivande metadata. Klientprogram vanligen ignorerar beskrivningar som lagrats i hello-datakälla.
* Det är ofta en onödig ansträngning att skapa dokumentation för datakällor. Det är ett ständigt ansvar att synkronisera dokumentationen med datakällorna och användarna saknar förtroende för dokumentation som uppfattas som föråldrad.
* Det är komplicerat och tidskrävande att skapa och upprätthålla dokumentationen för en datakälla. Den dokumentation tillgänglig tooeveryone som använder hello-datakällan kan vara blir ännu större.
* Begränsa åtkomst toodata källor och se till att datakonsumenterna vet hur toorequest åtkomst är en ständig utmaning.

När sådana utmaningar kombineras utgör de ett stort hinder för företag som vill tooencourage och främja hello användning och förståelse av företagsdata.

## <a name="azure-data-catalog-can-help"></a>Azure Data Catalog kan hjälpa dig
Data Catalog är utformad tooaddress dessa problem och toohelp företag get hello mest värdet från sina befintliga informationstillgångar. Data Catalog är datakällor enkelt identifiera och förstå hello användare som hanterar hello data.

Data Catalog är en molnbaserad tjänst där du kan registrera datakällor. hello data finns kvar i den befintliga platsen, men en kopia av dess metadata läggs tooData katalog, tillsammans med en referensplats toohello datakälla. Hej metadata är också indexerade toomake varje datakälla som enkelt kan identifieras via Sök och förstå toohello användare som identifierar den.

När en datakälla har registrerats kan utökas dess metadata, av hello användare som registrerat den eller av andra användare i hello enterprise. Alla användare kan kommentera en datakälla genom att ange beskrivningar, taggar eller andra metadata, till exempel dokumentation och processer för att begära åtkomst till datakällan. Dessa beskrivande metadata kompletterar hello strukturella metadata (till exempel namn och data kolumntyper) som registrerats från hello-datakälla.

Identifiera och förstå datakällor och deras användning är hello Huvudsyftet med registrering hello källor. Företagsanvändare behöver data för business intelligence, programutveckling, datavetenskap eller andra uppgifter där hello rätt data krävs. De kan använda Data Catalog hello identifiering upplevelse tooquickly hitta data som motsvarar deras behov och förstå hello data tooevaluate hello dess lämplighet och använda hello data genom att öppna hello-datakällan i önskat verktyg. 

AT hello samma tid användare kan bidra toohello katalog genom att tagga, dokumentera och kommentera datakällor som redan har registrerats. De kan också registrera nya datakällor som kan sedan identifieras, förstås, och används av hello kataloganvändare.

![Funktioner i Data Catalog](./media/data-catalog-what-is-data-catalog/data-catalog-capabilities.png)

## <a name="learn-more-about-data-catalog"></a>Mer information om Data Catalog
toolearn mer om hello funktionerna i Data Catalog, se:

* [Hur tooregister datakällor](data-catalog-how-to-register.md)
* [Hur toodiscover datakällor](data-catalog-how-to-discover.md)
* [Hur tooannotate datakällor](data-catalog-how-to-annotate.md)
* [Hur toodocument datakällor](data-catalog-how-to-documentation.md)
* [Hur tooconnect toodata källor](data-catalog-how-to-connect.md)
* [Hur toowork med stordata](data-catalog-how-to-big-data.md)
* [Hur toomanage datatillgångar](data-catalog-how-to-manage.md)
* [Hur tooset in hello Företagsordlista](data-catalog-how-to-business-glossary.md)
* [Vanliga frågor och svar](data-catalog-frequently-asked-questions.md)

## <a name="next-steps"></a>Nästa steg
tooget igång med Data Catalog, gå till:
* [Microsoft Azure Data Catalog](https://www.azuredatacatalog.com)
* [Kom igång med Azure Data Catalog](data-catalog-get-started.md)
