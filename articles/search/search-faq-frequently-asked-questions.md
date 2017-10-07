---
title: "frågor (FAQ) och svar om Azure Search aaaFrequently | Microsoft Docs"
description: "Få svar toocommon frågor om Microsoft Azure Search-tjänsten"
services: search
author: HeidiSteen
manager: jhubbard
ms.service: search
ms.technology: search
ms.topic: article
ms.date: 08/03/2017
ms.author: heidist
ms.openlocfilehash: 2c573750600d80585b746bfce20d6c0f8df5a262
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-search---frequently-asked-questions-faq"></a>Azure Search - vanliga frågor (FAQ)
 
 Hitta svar toocommonly frågor och svar om begrepp och koden och scenarier relaterade tooAzure sökning.

## <a name="platform"></a>Plattform

### <a name="how-is-azure-search-different-from-full-text-search-in-my-dbms"></a>Hur skiljer sig Azure Search från fulltextsökning i min DBMS?

Azure Search har stöd för flera datakällor [språkliga analys för många språk](https://docs.microsoft.com/rest/api/searchservice/language-support), [anpassade analys för intressanta och ovanliga indata](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), söka rank kontroller via [bedömningen profiler](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), och användarupplevelse funktioner, till exempel typeahead, träffar syntaxmarkering och fasetterad navigering. Även andra funktioner, till exempel synonymer och omfattande frågesyntaxen, men de generellt skilja inte funktioner.

### <a name="what-is-hello-difference-between-azure-search-and-elasticsearch"></a>Vad är hello skillnaden mellan Azure Search och Elasticsearch?

När du vill jämföra Sök tekniker efterfrågar kunder ofta närmare information på hur Azure Search Jämför med Elasticsearch. Kunder som väljer Azure Search över Elasticsearch för sökningen programprojekt vanligtvis inte eftersom vi har gjort en nyckel uppgift enklare eller de behöver hello inbyggda integreringen med andra Microsoft-tekniker:

+ Azure Search är en fullständigt hanterad molntjänst med 99,9% servicenivåavtal (SLA) när etableras med tillräckligt redundans (2 repliker för läsbehörighet, 3 repliker för skrivskyddad).
+ Microsofts [naturligt språk processorer](https://docs.microsoft.com/rest/api/searchservice/language-support) erbjuder senaste inguistic analys.  
+ [Azure Search indexerare](search-indexer-overview.md) kan crawlas en mängd olika Azure-datakällor för inledande och inkrementella indexering.
+ Om du behöver snabba svar toofluctuations i fråga eller indexering volymer, kan du använda [skjutreglage](search-manage.md#scale-up-or-down) i hello Azure portal eller kör en [PowerShell-skript](search-manage-powershell.md), kringgå Fragmentera management direkt.  
+ [Poäng och finjustera funktioner](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) ange hello: för att påverka Sök rangordning utöver vilka hello sökmotor enbart kan ge. 

### <a name="can-i-pause-azure-search-service-and-stop-billing"></a>Kan jag pausa Azure Search-tjänsten och stoppa fakturering?

Du kan pausa hello-tjänsten. Beräkningar och lagring resurser för exklusiv användning när hello-tjänsten har skapats. Det är inte möjligt toorelease och frigöra de resurser på begäran. 

## <a name="indexing-operations"></a>Indexeringsåtgärder

### <a name="backup-and-restore-or-download-and-move-indexes-or-index-snapshots"></a>Säkerhetskopiera och återställa (eller ladda ned och flytta) index eller index ögonblicksbilder?

Även om du kan [få en Indexdefinition](https://docs.microsoft.com/rest/api/searchservice/get-index) när som helst, det finns ingen index extrahering, ögonblicksbild eller säkerhetskopiering återställning funktionen för att ladda ned en *fylls* index som körs i hello molnet tooa lokalt system, eller Flytta den tooanother Azure Search-tjänsten. 

Index byggs och fylls i från kod som du skriver och bara köras på Azure Search i hello molnet. Normalt kunder som vill toomove ett index tooanother tjänsten gör du genom att redigera deras kod toouse en ny slutpunkt och kör sedan indexering. Om du vill hello möjlighet tootake en ögonblicksbild eller säkerhetskopiera ett index omvandla en röst [User Voice](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index).

### <a name="can-i-index-from-sql-database-replicas-applies-tooazure-sql-database-indexershttpsdocsmicrosoftcomazuresearchsearch-howto-connecting-azure-sql-database-to-azure-search-using-indexers"></a>Kan jag indexera från SQL-databasrepliker (gäller för[Azure SQL Database indexerare](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers))

 Det finns inga begränsningar hello användningen av primära eller sekundära repliker som datakälla när ett index från grunden. Uppdatera ett index med inkrementella uppdateringar (baserat på ändrade poster) kräver dock hello primära repliken. Det här kravet kommer från SQL-databas, vilket garanterar ändringsspårning på primära repliker. Om du försöker använda sekundära repliker för en arbetsbelastning för uppdatering av index, är det inte säkert som du får alla hello data.

## <a name="search-operations"></a>Sökningar

### <a name="can-i-search-across-multiple-indexes"></a>Kan jag söka över flera index?

Nej, den här åtgärden stöds inte. Sökningen är alltid begränsade tooa enda index.

### <a name="can-i-restrict-search-corpus-access-by-user-identity"></a>Kan jag begränsa sökningen Kristi åtkomsten genom användarens identitet?

Azure Search har inte en rollbaserad säkerhetsmodell för åtkomst till data per användare. Autentisering är antingen fullständiga rättigheter eller skrivskyddad på hello servicenivå. Vissa kunder har implementerat lösningar, baserade på [ `$filter` sökparameter](https://docs.microsoft.com/rest/api/searchservice/search-documents), men det är en lösning i bästa. Om du vill att den här funktionen kan du omvandla en röst [User Voice](https://feedback.azure.com/forums/263029-azure-search/category/86074-security).

### <a name="why-are-there-zero-matches-on-terms-i-know-toobe-valid"></a>Varför är noll matchar villkor som jag vet toobe giltig?

hello vanligaste fallet inte veta att varje typ av fråga stöder olika Sök beteenden och nivåer av språkliga analys. Fulltextsökning, vilket är hello övervägande arbetsbelastning, innehåller en analysfasen för språket som bryter mot villkoren ned tooroot formulär. Den här delen av frågan parsning kastar ett bredare net över möjliga matchningar eftersom hello principfilerna termen matchar ett större antal varianter.

Om du anropar [andra frågetyper](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) (jokertecken sökning, fuzzy sökning, närhet sökning, förslag, bland annat), det finns inga språkliga analys. Villkor läggs toohello frågan trädet som-är. 

### <a name="why-is-hello-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>Varför är hello sökrangordningen en konstant eller lika poäng 1.0 för varje träffar?

Som standard sökresultat ger baserat på hello [statistiska egenskaper för matchar villkoren](search-lucene-query-architecture.md#stage-4-scoring), och sorterade hög toolow i hello resultatmängden. Dock vissa fråga typer (med jokertecken, prefix, regex) alltid bidra med en konstant poäng toohello övergripande dokumentera poäng. Det här beteendet är avsiktligt. Azure Search begränsar dock en konstant poängsätta tooallow matchningar via frågan expansion toobe ingår i hello resultat utan att påverka hello rangordning. 

Anta exempelvis att indata av ”rundtur *” i en sökning med jokertecken ger träffar på ”visningar”, ”tourettes” och ”tourmaline”. Eftersom hello de här resultaten returneras, går det inte härleda tooreasonably vilket är mer värdefull än andra. Därför ignorera vi termen frekvenser när poängberäkningen resultat i frågor för typer med jokertecken, prefix och regex. Sökresultat baserat på en partiell inmatning ges en konstant poängsätta tooavoid rikta mot potentiellt oväntat matchar.

## <a name="design-patterns"></a>Designmönster

### <a name="what-is-hello-best-approach-for-implementing-localized-search"></a>Vad är hello bästa metoden för att implementera lokaliserade sökningen?

De flesta kunderna välja dedikerade fält över en samling när det gäller toosupporting olika språk (språk) i hello samma index. Språkspecifika fält gör det möjligt tooassign en lämplig analyzer. Till exempel tilldela hello Microsoft franska Analyzer tooa fält som innehåller franska strängar. Det förenklar också filtrering. Om du vet att en fråga initieras på en sida fr-fr, kan du begränsa sökfältet resultat toothis. Skapa en [bedömningen profil](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) toogive hello fältet mer relativa viktade.

## <a name="next-steps"></a>Nästa steg

Är en fråga om en funktion eller funktionalitet som saknas? Begära hello-funktionen på hello [User Voice-webbplats](https://feedback.azure.com/forums/263029-azure-search).

## <a name="see-also"></a>Se även

 [StackOverflow: Azure Search](https://stackoverflow.com/questions/tagged/azure-search)   
 [Hur full textsökning fungerar i Azure Search](search-lucene-query-architecture.md)  
 [Vad är Azure Search?](search-what-is-azure-search.md)

 