---
title: "aaaSet in hello företagsordlista för styrda taggning i Azure Data Catalog | Microsoft Docs"
description: "Hur tooarticle färgmarkera hello företagsordlista i Azure Data Catalog för att definiera och använder en gemensam business ordförråd tootag registrerade datatillgångar."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: b3d63dbe-1ae7-499f-bc46-42124e950cd6
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: c9adf663bd08ac3c0c7b5d3551e6af409fe69ebc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-business-glossary-for-governed-tagging"></a>Ställ in hello företagsordlista för styrs taggning
## <a name="introduction"></a>Introduktion
Azure Data Catalog gör datakälla identifiering så att du lätt kan identifiera och förstå hello datakällor som du behöver tooperform analyser och fatta beslut. Dessa funktioner gör hello största effekten när du kan hitta och förstå hello mycket stort antal typer av datakällor.

En funktion som främjar bättre kunskap om tillgångar data i Data Catalog är markering. Du kan associera nyckelord med en tillgång eller en kolumn, vilket i sin tur gör det enklare toodiscover hello tillgång via söka eller bläddra genom att använda taggar. Taggning hjälper dig också mer enkelt förstå hello kontext och syftet med hello tillgången.

Taggning kan dock ibland orsaka problem egna. Det är några exempel på problem som kan leda till märkning:

* hello användning av förkortningar på vissa tillgångar och utökade text på andra. Den här inkonsekvens hämmar hello identifiering av tillgångar, även om hello avsikten var tootag hello tillgångar med hello samma tagg.
* Potentiella variationer i betydelse, beroende på kontext. Till exempel en tagg kallas *intäkter* på en kund datauppsättning kan innebära intäkter av kunden, men hello samma tagg för ett kvartal försäljning datauppsättningen kan innebära kvartalsvisa intäkter för hello företag.  

toohelp åtgärda dessa och andra liknande utmaningar, Data Catalog innehåller en företagsordlista.

Med hjälp av hello företagsordlista för Data Catalog kan en organisation dokumentera viktiga termer och deras definitioner toocreate en gemensam ordlista för företag. Den här styrning kan konsekvens i dataanvändning över hello organisationen. När en term som har definierats i hello företagsordlista, kan den tilldelas tooa datatillgång i hello-katalogen. Den här metoden *styrs taggning*, är hello samma metod som Taggning.

## <a name="glossary-availability-and-privileges"></a>Ordlista tillgänglighet och privilegier
Hej företagsordlista är endast tillgänglig i hello Standard Edition av Azure Data Catalog. hello ledigt Edition av Data Catalog innehåller inte en ordlista och ger inte funktioner för styrda Taggning.

Du kan komma åt hello företagsordlista via hello **ordlista** alternativet i navigeringsmenyn hello Data Catalog portal.  

![Åtkomst till hello företagsordlista](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)

Data Catalog administratörer och medlemmar i hello ordlista-administratörer kan skapa, redigera och ta bort ordlista i hello företagsordlista. Alla Data Catalog-användare kan visa hello Termdefinitioner och tagg tillgångar med termer från ordlistan.

![Lägga till en ny Ordlisteterm](./media/data-catalog-how-to-business-glossary/02-new-term.png)

## <a name="creating-glossary-terms"></a>Skapa ordlista
Data Catalog administratörer och ordlista administratörer kan skapa ordlista genom att klicka på hello **ny Term** knappen. Varje termen innehåller hello följande fält:

* Ett företag definitionen för hello term
* En beskrivning som samlar in hello avsedd användning eller business regler för hello tillgång eller kolumn
* En lista över intressenter som känner hello mest om hello term
* hello överordnade term som definierar hello hierarki i vilka hello termen ordnas

## <a name="glossary-term-hierarchies"></a>Ordlista termen hierarkier
En organisation kan beskriva dess ordförråd för företag som en hierarki med villkoren med hjälp av hello företagsordlista för Data Catalog och den kan skapa en klassificering av termer som representerar bättre dess klassificering för företaget.

Ett villkor som måste vara unika på en viss nivå i hierarkin. Dubblettnamn är inte tillåtna. Det finns ingen gräns toohello antalet nivåer i en hierarki, men en hierarki är ofta lättare att förstå när det finns tre nivåer eller färre.

hello användning av hierarkier i hello företagsordlista är valfritt. Lämnar hello överordnade termen fältet tomt för ordlista skapar en platt (icke-hierarkisk) lista över villkor i hello ordlista.  

## <a name="tagging-assets-with-glossary-terms"></a>Taggning tillgångar med ordlista
När ordlista har definierats hello Catalog är hello upplevelse för taggning tillgångar optimerad toosearch hello ordlista när användaren anger en tagg. hello Data Catalog-portalen visar en lista över matchande ordlista villkoren toochoose från. Om hello användaren väljer en Ordlisteterm hello listan, läggs hello termen toohello tillgång som en etikett (kallas även en ordlista tagg). hello användare kan också välja toocreate en ny tagg genom att skriva en term som inte är i hello ordlista (kallas även en användartagg för).

![Datatillgång märkta med taggen för en användare och två ordlista taggar](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [!NOTE]
> Användaren taggar är hello endast typ av taggen stöds i hello ledigt Edition av Data Catalog.
>
>

### <a name="hover-behavior-on-tags"></a>Hovra beteende på taggar
Hello två typer av taggar finns i hello Data Catalog-portalen, visuellt distinkta och finns olika hovra beteenden. Du kan se hello taggen text och hello eller de användare som har lagt till hello taggen när du hovrar över en användartagg för. När du hovrar över en ordlista tagg kan se du också hello definitionen av hello Ordlisteterm och en länk tooopen hello business ordlista tooview hello fullständig definition hello har löpt ut.

### <a name="search-filters-for-tags"></a>Sökfilter för taggar
Ordlista taggar och användaren taggar är båda sökbara och du kan använda dem som filter i en sökning.

## <a name="summary"></a>Sammanfattning
Med hjälp av hello företagsordlista i Azure Data Catalog och hello styrs taggning det aktiverar du identifiera, hantera och identifiera datatillgångar i ett konsekvent sätt. Hej företagsordlista kan befordra inlärning av hello business ordförråd av organisationsmedlemmar. hello ordlista stöder också samla in beskrivande metadata, vilket förenklar tillgången identifiering och förstå.

## <a name="next-steps"></a>Nästa steg
* [REST API-dokumentation för affärsåtgärder ordlista](https://msdn.microsoft.com/library/mt708855.aspx)
