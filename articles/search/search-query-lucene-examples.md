---
title: "aaaLucene fråga exempel för Azure Search | Microsoft Docs"
description: "Lucene frågesyntaxen för fuzzy sökning, närhet sökning, termen förstärkning, reguljära uttryck och sökning med jokertecken."
services: search
documentationcenter: 
author: LiamCa
manager: pablocas
editor: 
tags: Lucene query analyzer syntax
ms.assetid: 147f360d-a5ce-4d7b-a909-c8b65bfb748c
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/21/2017
ms.author: liamca
ms.openlocfilehash: d859cf697ef9485a49e9e0591b68e812ffa55f1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Lucene fråga syntaxexemplen för att skapa frågor i Azure Search
När konstruera frågor för Azure Search kan du använda antingen standard hello [enkel frågesyntaxen](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) eller hello alternativa [Lucene Frågeparsern i Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search). Hej Lucene Frågeparsern stöder mer komplex fråga konstruktioner fältet omfång frågor, fuzzy sökning, närhet sökning, termen förstärkning och reguljärt uttryck för sökning.

I den här artikeln kan du gå igenom exemplen visar frågeåtgärder som är tillgängliga när du använder hello fullständig syntax.

## <a name="viewing-hello-examples-in-jsfiddle"></a>Visa hello exemplen i JSFiddle

Alla hello exemplen i den här artikeln är körbara frågor som körs mot en förinstallerade sökindex i [JSFiddle](https://jsfiddle.net), Kodredigerare online för att testa skript och HTML. 

toorun dem, högerklicka på hello frågan exempel-URL: er tooopen JSFiddle i ett nytt webbläsarfönster.

> [!NOTE]
> hello följande exempel utnyttjar en sökindex som består av jobb tillgängliga baserat på en datamängd som tillhandahålls av hello [stad New York OpenData](https://nycopendata.socrata.com/) initiativ. Dessa data ska inte ses aktuella eller slutförd. hello index finns på en sandbox-tjänst som tillhandahålls av Microsoft. Du behöver inte en Azure-prenumeration eller Azure Search tootry dessa frågor.
>


## <a name="how-tooinvoke-full-lucene-parsing"></a>Hur tooinvoke fullständig Lucene parsning

Alla hello exemplen i den här artikeln anger hello **queryType = full** sökparameter, som anger den fullständiga syntaxen hello, hanteras av hello Lucene Frågeparsern. 

**Exempel 1** --Högerklicka hello följande fråga fragment tooopen den i webbläsaren sidan som läser in JSFiddle och kör hello fråga:

* [& queryType = fullständig & Sök = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

I hello nytt webbläsarfönster visas hello JavaScript käll- och HTML-utdata sida vid sida. hello skript refererar till en fullständig fråga (inte bara hello fragment enligt hello länk). hello fullständiga frågan visas i hello URL: er för varje exempel. 

Den här frågan returnerar dokument från indexet New York City jobb (nycjobs, har lästs in på en sandbox-tjänst). Planeringsaspekter anger hello frågan endast företag titlar returneras. hello fullständig underliggande frågan är följande:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Hej **searchFields** parametern begränsar hello toojust hello business rubrik sökfältet. Hej **queryType** har angetts för**fullständig**, vilket gör att Azure Search toouse hello Lucene Frågeparsern för den här frågan.

> [!NOTE]
> Bakgrundsinformation om frågebearbetning finns [hur Fullständig textsökning fungerar i Azure Search](search-lucene-query-architecture.md). Mer information om sökparametrarna finns [Sök dokument (Azure Söktjänsts-REST API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).
>

### <a name="fielded-query-operation"></a>Fielded frågeåtgärden
Du kan ändra hello exemplen i den här artikeln genom att ange en **fieldname:searchterm** konstruktionen toodefine en fielded frågeåtgärden där hello fältet är ett enstaka ord och hello sökordet är också ett ord eller en fras, Du kan också med booleska operatorer. Några exempel hello följande:

* business_title:(senior NOT junior)
* status: (”New York” och ”nya Jersey”)

Vara säker på att tooput flera strängar inom citattecken om du vill att båda strängarna toobe utvärderas som en enda enhet, som det här fallet söker efter två distinkta städer i hello Platsfältet. Kontrollera också hello operatorn versal som du ser med inte och och.

hello-fält som anges i **fieldname:searchterm** måste vara sökbara fält. Se [Create Index (Azure Söktjänsts-REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) information om hur indexattribut används i fältdefinitioner.

**Exempel 2** --Högerklicka hello följande fråga fragment den här frågan söker efter företag titlar med hello term erfarna i dem, men inte oerfarna:

* [& queryType = fullständig & Sök = business_title:senior inte oerfarna](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="fuzzy-search-example"></a>Fuzzy Sök-exempel
En fuzzy sökning matchningar har hittats i villkor som har en liknande konstruktion. Per [Lucene dokumentationen](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), fuzzy sökningar baseras på [Damerau Levenshtein avstånd](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

toodo fuzzy sökning, lägga till hello tilde ”~” symbol hello slutet av ett ord med en valfri parameter, ett värde mellan 0 och 2, som anger hello redigera avstånd. Till exempel ”blå ~” eller ”blå ~ 1” returnerar blått, blå och ibland sammanlänkande.

**Exempel 3** --Högerklicka hello följande fråga fragment. Den här frågan söker efter jobb med hello termen associera (där det är felstavat):

* [& queryType = fullständig & Sök = business_title:asosiate ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

> [!Note]
> Fuzzy frågor är inte [analyseras](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis), vilket kan vara konstigt om du räknar härrör eller lemmatisering. Lexikaliskt analys utförs endast på fullständiga villkor (en term fråga eller en frasfråga). Frågetyper med ofullständiga villkor (prefix frågan, jokertecken frågan, regex-fråga, fuzzy fråga) läggs direkt toohello frågan trädet kringgå hello analys steget. hello endast omvandling utförs på ofullständiga sökord lowercasing.
>

## <a name="proximity-search-example"></a>Närhet Sök-exempel
Närhet sökningar är används toofind termer som är nära varandra i ett dokument. Infoga tilde ”~” symbol hello slutet av en fras följt av hello antalet ord som skapar hello närhet gräns. Till exempel ”hotell flygplats” ~ 5 hittar hello villkoren hotell och flygplats inom 5 ord från varandra i ett dokument.

**Exempel 4** --Högerklicka hello frågan. Sök efter jobb med hello termen ”erfarna analytiker” där det avgränsas med mer än ett ord:

* [& queryType = fullständig & Sök = business_title: ”erfarna analytiker” ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Exempel 5** --prova igen att ta bort hello ord mellan hello termen ”erfarna analytiker”.

* [& queryType = fullständig & Sök = business_title: ”erfarna analytiker” ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting-examples"></a>Termen förstärkning exempel
Termen förstärkning refererar tooranking ett dokument som är högre om den innehåller hello ökat sikt relativa toodocuments som inte innehåller hello har löpt ut. Detta skiljer sig från bedömningen profiler i att bedömningsprofil profiler öka vissa fält i stället för på specifika villkor. hello följande exempel hjälper till att illustrera hello skillnader.

Överväg att bedömningsprofilen som förstärker matchar i ett visst fält som **genre** i hello musicstoreindex exempel. Termen förstärkning kunde använda toofurther förstärkningen vissa Sök termer senare än andra. Till exempel ”Berg ^ 2 elektronisk” förbättras dokument som innehåller hello sökorden i hello **genre** fält som är högre än andra sökbara fält i hello index. Dessutom kommer dokument som innehåller hello sökterm ”Berg” att rangordnas högre än hello andra sökterm ”elektronisk” på grund av hello termen förstärkningen värde (2).

tooboost en term, Använd hello hatt ”^”, symbol med en faktor förstärkningen (en siffra) när hello hello du söker. Hej högre hello förstärkningen faktor hello mer relevant hello kommer vara relativ tooother sökvillkoren. Som standard är hello förstärkningen faktor 1. Även om hello förstärkningen tillväxtfaktor måste vara positivt, kan det vara mindre än 1 (till exempel 0,2).

**Exempel 6** --Högerklicka hello frågan. Sök efter jobb med hello termen ”datorn analytiker” där det finns inga resultat med både ord dator och analytiker ännu analytiker jobb är hello överst i hello resultat.

* [& queryType = fullständig & Sök = business_title:computer analytiker](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Exempel 7** --försök igen, den här gången förstärkning innebär med hello termen dator över hello termen analytiker om båda orden inte finns.

* [& queryType = fullständig & Sök = business_title:computer ^ 2 analytiker](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression-example"></a>Reguljärt uttryck exempel
Ett reguljärt uttryck hittas en matchning utifrån hello innehåll mellan snedstreck ”/”, som dokumenterade i hello [klassen RegExp](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

**Exempel 8** --Högerklicka hello frågan. Sök efter jobb med hello termen chef eller sitt första år.

* `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

hello-URL för det här exemplet kommer inte att återge korrekt hello på sidan. Kopiera hello URL nedan och klistra in den i hello webbläsare URL-adressen som en lösning:`http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`

## <a name="wildcard-search-example"></a>Jokertecken Sök-exempel
Du kan använda allmänt erkända syntax för flera (\*) eller enstaka jokertecken med tecken (?). Observera hello Lucene frågan parsern stöder hello användning av dessa symboler med en enda term och inte en fras.

**Exempel 9** --Högerklicka hello frågan. Sök efter jobb som innehåller hello prefixet 'prog' som omfattar business titlar med hello villkor programming programmerare i den.

* [& queryType = fullständiga & $select = business_title & Sök = business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26queryType=full%26$select=business_title%26search=business_title:prog*)

Du kan inte använda en * eller? symbol hello första tecknet i en sökning.

## <a name="next-steps"></a>Nästa steg
Försök att ange hello Lucene Frågeparsern i din kod. hello följande länkar beskriver hur tooset söktiden frågor för både .NET och hello REST API. hello länkar syntaxen hello standard enkla så du måste tooapply vad du lärt dig från den här artikeln toospecify hello **queryType**.

* [Fråga ditt Azure Search Index med hello .NET SDK](search-query-dotnet.md)
* [Fråga ditt Azure Search Index med hello REST API](search-query-rest-api.md)

## <a name="see-also"></a>Se även

 [Hur full textsökning fungerar i Azure Search](search-lucene-query-architecture.md)