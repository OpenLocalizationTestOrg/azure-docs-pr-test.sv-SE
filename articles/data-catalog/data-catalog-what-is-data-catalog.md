---
title: Introduktion till Azure Data Catalog | Microsoft Docs
description: "Den här artikeln innehåller en översikt över Microsoft Azure Data Catalog, inklusive funktioner och vilka problem den är tänkt att lösa. Data Catalog låter alla användare registrera, identifiera, förstå och använda datakällor."
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
ms.openlocfilehash: a28a7679831201fcf3a9d1c15497ff706c2752a1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-data-catalog"></a>Vad är Azure Data Catalog?
Azure Data Catalog är en helt hanterad molntjänst vars användare kan identifiera datakällor de behöver och förstå datakällorna de identifierar. På samma gång hjälper Data Catalog organisationer att få ut mer av sina befintliga investeringar. 

Med Data Catalog kan alla användare (analytiker, IT-forskare eller utvecklare) identifiera, förstå och använda datakällor. Data Catalog innehåller en gemensam modell för metadata och kommentarer. Det är en gemensam, central plats där hela organisationens användare kan bidra med sina kunskaper och skapa en databaserad gemenskap och kultur för data.

## <a name="discovery-challenges-for-data-consumers"></a>Det är svårt för datakonsumenterna att hitta rätt
Traditionellt har man identifierat företagets datakällor genom en organisk process baserad på gruppens kunskaper. Detta medför stora utmaningar för företag som vill få ut största möjliga värde av sina informationstillgångar:

* Användare kan vara omedvetna om att en datakälla finns om de stöter på den som en del av en annan process. Det finns ingen central plats där datakällorna registreras.
* Om användarna vet platsen för en datakälla, kan de inte ansluta till data med hjälp av ett klientprogram. Dataförbrukningsupplevelser gör att användarna måste känner till anslutningssträngen eller sökvägen.
* Om användarna inte känner till datakällans plats kan de inte förstå dess avsedda användningsområde. Datakällor och dokumentation kanske befinner sig på olika platser och används via en mängd olika upplevelser.
* Om användare har frågor om en informationsresurs måste de hitta experten eller teamet som ansvarar för data och prata med dem offline. Det finns ingen direkt koppling mellan data och personer med expertkunskaper om hur de används.
* Om användare inte känner till hur man begär åtkomst till datakällan, kan de inte komma åt de data som behövs trots att de har identifierat datakällan och dess dokumentation.

## <a name="discovery-challenges-for-data-producers"></a>Det är svårt för dataproducenterna att hitta rätt
Datakonsumenterna står inför vissa svårigheter, medan användarna som ansvarar för att producera och upprätthålla informationstillgångar står inför andra svårigheter:

* Det är ofta en onödig ansträngning att kommentera datakällor med beskrivande metadata. Klientprogram ignorerar vanligtvis beskrivningar som lagrats i datakällan.
* Det är ofta en onödig ansträngning att skapa dokumentation för datakällor. Det är ett ständigt ansvar att synkronisera dokumentationen med datakällorna och användarna saknar förtroende för dokumentation som uppfattas som föråldrad.
* Det är komplicerat och tidskrävande att skapa och upprätthålla dokumentationen för en datakälla. Det är ofta ännu svårare att göra dokumentationen lättillgänglig för alla som använder datakällan.
* Det är en ständig utmaning att begränsa tillgången till datakällan och samtidigt se till att datakonsumenterna vet hur de begär tillgång.

I kombination utgör dessa svårigheter ett stort hinder för företag som vill uppmuntra och främja användning och förståelse av företagsdata.

## <a name="azure-data-catalog-can-help"></a>Azure Data Catalog kan hjälpa dig
Data Catalog är utformad för att åtgärda dessa problem och hjälpa företagen att få ut så mycket som möjligt av sina befintliga informationstillgångar. Data Catalog underlättar genom att göra det enkelt för användare som behöver data att identifiera och förstå datakällorna.

Data Catalog är en molnbaserad tjänst där du kan registrera datakällor. Dina data blir kvar där du har dem, men en kopia av deras metadata läggs till i Data Catalog tillsammans med en referens till platsen där datakällan finns. Dessa metadata indexeras också för att det ska bli enkelt att identifiera alla datakällor och för att användare som identifierar dem ska förstå dem.

När en datakälla har registrerats kan dess metadata utökas, antingen av användaren som registrerade källan eller av andra användare i företaget. Alla användare kan kommentera en datakälla genom att ange beskrivningar, taggar eller andra metadata, till exempel dokumentation och processer för att begära åtkomst till datakällan. Dessa beskrivande metadata kompletterar strukturella metadata (till exempel kolumnnamn och datatyper) som registrerats från datakällan.

Huvudsyftet med registrering av källorna är att identifiera och förstå datakällor och deras användningsområden. Företagsanvändare behöver data för business intelligence, programutveckling, datavetenskap eller andra uppgifter där rätt data krävs. De kan använda sökfunktionen i Data Catalog för att snabbt hitta data som motsvarar deras behov, förstå datakällan för att utvärdera dess lämplighet och använda data genom att öppna datakällan i önskat verktyg. 

På samma gång kan användarna bidra till katalogen genom att tagga, dokumentera och kommentera datakällor som redan har registrerats. De kan också registrera nya datakällor som kan sedan identifieras, förstås och användas av kataloganvändare.

![Funktioner i Data Catalog](./media/data-catalog-what-is-data-catalog/data-catalog-capabilities.png)

## <a name="learn-more-about-data-catalog"></a>Mer information om Data Catalog
Här finns mer information om funktionerna i Data Catalog:

* [Så här registrerar du datakällor](data-catalog-how-to-register.md)
* [Så här upptäcker du datakällor](data-catalog-how-to-discover.md)
* [Så här skapar du anteckningar om datakällor](data-catalog-how-to-annotate.md)
* [Så här dokumenterar du datakällor](data-catalog-how-to-documentation.md)
* [Så här skapar ansluter du till datakällor](data-catalog-how-to-connect.md)
* [Så här arbetar du med stordata](data-catalog-how-to-big-data.md)
* [Hantera datatillgångar](data-catalog-how-to-manage.md)
* [Så här konfigurerar du företagsordlistan](data-catalog-how-to-business-glossary.md)
* [Vanliga frågor och svar](data-catalog-frequently-asked-questions.md)

## <a name="next-steps"></a>Nästa steg
För att komma igång med Data Catalog, gå till:
* [Microsoft Azure Data Catalog](https://www.azuredatacatalog.com)
* [Kom igång med Azure Data Catalog](data-catalog-get-started.md)
