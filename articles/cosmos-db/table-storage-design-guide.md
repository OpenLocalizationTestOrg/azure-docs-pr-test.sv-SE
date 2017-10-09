---
title: "aaaAzure designguiden för lagring tabellen | Microsoft Docs"
description: Design skalbar och Performant tabeller i Azure-tabellagring
services: cosmos-db
documentationcenter: na
author: mimig1
manager: tadb
editor: tysonn
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 02/28/2017
ms.author: mimig
ms.openlocfilehash: 059f05a1d20e4d9537034b7ca133c5334bbefa04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-table-design-guide-designing-scalable-and-performant-tables"></a>Designguide för Azure Storage-tabellen: Utforma skalbar och Performant tabeller
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

toodesign skalbar och performant tabeller måste du tänka på ett antal faktorer, till exempel prestanda, skalbarhet och kostnader. Om du har tidigare designat scheman för relationsdatabaser, dessa överväganden blir bekant tooyou, men det finns vissa likheter mellan hello Azure Table storage modell och relationella modeller, det finns också flera viktiga skillnader. Dessa skillnader leda vanligtvis toovery olika konstruktionerna som kan se bekant med relationsdatabaser toosomeone krånglig eller fel, men som gör bra uppfattning om du designar för en NoSQL nyckel/värde-butiken, till exempel hello Azure Table-tjänsten. Många av din design skillnader visar hello faktum att hello tabelltjänsten är utformad toosupport molnskala program som kan innehålla miljarder enheter (rader i en relationsdatabas terminologi) av data eller för datauppsättningar som måste ha stöd för hög transaktionsvolymer: därför du behöver toothink annorlunda om hur du lagrar data och förstå hur hello tabelltjänsten fungerar. En väl utformad NoSQL-databas kan aktivera din lösning tooscale mycket mer (och till en lägre kostnad) än en lösning som använder en relationsdatabas. Den här guiden hjälper dig med dessa frågor.  

## <a name="about-hello-azure-table-service"></a>Om hello Azure Table-tjänsten
Det här avsnittet beskrivs några av hello viktiga funktioner i hello tabelltjänsten som är särskilt relevanta toodesigning för prestanda och skalbarhet. Om du är ny tooAzure lagring och hello tabelltjänsten först läsa [introduktion tooMicrosoft Azure Storage](../storage/common/storage-introduction.md) och [Kom igång med Azure Table Storage med hjälp av .NET](table-storage-how-to-use-dotnet.md) innan du läser hello resten av det här artikel. Även om hello av den här guiden fokuserar på hello tabelltjänsten, innehåller en beskrivning av hello Azure Queue och Blob-tjänster och hur du kan använda dem tillsammans med hello tabelltjänsten i en lösning.  

Vad är tabelltjänsten hello? Som du kan förvänta sig från hello namn använder hello tabelltjänsten ett tabellformat toostore data. Varje rad i tabellen hello representerar en entitet i hello standard terminologi och hello kolumner store hello olika egenskaper för enheten. Varje entitet har två nycklar toouniquely identifiera den och en tidsstämpelkolumn som hello tabelltjänsten använder tootrack när hello entiteten senast uppdaterades (detta sker automatiskt och du kan inte skriva över hello tidsstämpel manuellt med ett godtyckligt värde). Hej tabelltjänsten använder den här last-modified tidsstämpel (LMT) toomanage Optimistisk samtidighet.  

> [!NOTE]
> hello tabell REST API tjänståtgärder också returnera en **ETag** värde som den härleds från hello last-modified tidsstämpel (LMT). I det här dokumentet använder vi hello termer ETag och LMT synonymt eftersom de refererar toohello samma underliggande data.  
> 
> 

hello följande exempel visar en enkel tabell design toostore medarbetare och avdelning entiteter. Många av hello exemplen senare i den här handboken är baserade på den här enkla designen.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>tidsstämpel</th>
<th></th>
</tr>
<tr>
<td>Marknadsföring</td>
<td>00001</td>
<td>2014-08-22T00:50:32Z</td>
<td>
<table>
<tr>
<th>Förnamn</th>
<th>Efternamn</th>
<th>Ålder</th>
<th>E-post</th>
</tr>
<tr>
<td>Ta</td>
<td>Hall</td>
<td>34</td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marknadsföring</td>
<td>00002</td>
<td>2014-08-22T00:50:34Z</td>
<td>
<table>
<tr>
<th>Förnamn</th>
<th>Efternamn</th>
<th>Ålder</th>
<th>E-post</th>
</tr>
<tr>
<td>Jun</td>
<td>CaO</td>
<td>47</td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marknadsföring</td>
<td>Avdelning</td>
<td>2014-08-22T00:50:30Z</td>
<td>
<table>
<tr>
<th>DepartmentName</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Marknadsföring</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Försäljning</td>
<td>00010</td>
<td>2014-08-22T00:50:44Z</td>
<td>
<table>
<tr>
<th>Förnamn</th>
<th>Efternamn</th>
<th>Ålder</th>
<th>E-post</th>
</tr>
<tr>
<td>Ken</td>
<td>Kwok</td>
<td>23</td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


Hittills har detta ser ut ungefär tooa tabellen i en relationsdatabas med hello viktiga skillnader som hello obligatoriska kolumnerna och hello möjlighet toostore flera entitet typer i hello samma tabell. Dessutom kan varje hello användardefinierade egenskaper som **Förnamn** eller **ålder** har en datatyp som heltal eller en sträng, precis som en kolumn i en relationsdatabas. Men till skillnad från i en relationsdatabas hello schemat mindre uppbyggnad hello tabellen service innebär att en egenskap måste ha hello samma datatyp för varje entitet. toostore komplexa datatyper i en enskild egenskap, måste du använda ett serialiserat format, till exempel JSON eller XML. Mer information om datatyper för hello tabellen service som stöds, stöds datumintervall, namngivningsregler och storlek restriktioner finns [hello förstå tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Eftersom du kommer att se ditt val av **PartitionKey** och **RowKey** är grundläggande toogood tabelldesign. Varje entitet som lagras i en tabell måste ha en unik kombination av **PartitionKey** och **RowKey**. Precis som med nycklar i en relationsdatabas tabell hello **PartitionKey** och **RowKey** värden är indexerad toocreate ett grupperat index som möjliggör snabb look-ups; dock hello tabelltjänsten skapar inte någon sekundärindex så att de hello endast två indexerade egenskaper (del av hello mönster som beskrivs senare visar hur du kan undvika den här tydligt begränsningen).  

En tabell består av en eller flera partitioner och som du kommer att se många hello utforma beslut blir runt att välja en lämplig **PartitionKey** och **RowKey** toooptimize din lösning. En lösning kan bestå av bara en enda tabell som innehåller alla entiteter indelade i partitioner, men vanligtvis en lösning har flera tabeller. Tabeller hjälper dig toologically ordna dina enheter, hjälper dig att hantera toohello data med hjälp av åtkomstkontrollistor och du kan släppa en hel tabell med hjälp av en enda lagring-åtgärd.  

### <a name="table-partitions"></a>Tabellpartitioner
hello kontonamn, tabellnamnet och **PartitionKey** identifierar tillsammans hello partition inom hello lagringstjänsten där hello tabelltjänsten lagrar hello entitet. Samt som en del av hello adresseringsschemat för entiteter partitioner för att definiera ett omfång för transaktioner (se [entitet gruppera transaktioner](#entity-group-transactions) nedan), och formuläret hello grunden för hur hello tabelltjänsten skalas. Läs mer på partitioner [Azure Storage skalbarhets- och prestandamål](../storage/common/storage-scalability-targets.md).  

I hello tabelltjänsten, en enskild nod services en eller flera slutföra partitioner och hello service skalor av dynamiskt nätverksbelastning partitioner mellan noder. Om en nod under belastning hello tabelltjänsten kan *dela* hello antal partitioner som underhålls av noden på olika noder; när trafik subsides hello-tjänsten kan *merge* hello partition intervall från tyst noder tillbaka till en enda nod.  

Mer information om hello interna detaljer om hello tabelltjänst och hur hello tjänsten hanterar partitioner, finns i synnerhet hello papper [Microsoft Azure Storage: A hög Available Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

### <a name="entity-group-transactions"></a>Entiteten gruppera transaktioner
Entiteten gruppera transaktioner (EGTs) finns i hello tabelltjänsten hello inbyggda mekanism för att utföra atomiska uppdateringar över flera enheter. EGTs är också enligt tooas *batch transaktioner* i viss dokumentation. EGTs fungerar bara med entiteter som lagras i hello samma partition (resursen hello samma partitionsnyckel i en given tabell), vilja ha atomiska transaktionella beteende över flera enheter måste du tooensure som dessa enheter finns i hello samma partition. Det här är ofta en orsak till att ha flera enhetstyper i hello samma tabell (och partition) och inte använder flera tabeller för olika enhetstyper. En enda EGT kan tillämpas på högst 100 entiteter.  Om du skickar flera samtidiga EGTs för bearbetning av det är viktigt tooensure fungerar inte dessa EGTs på enheter som är gemensamma mellan EGTs som annars bearbetning kan vara fördröjd.

EGTs också introducera en potentiell kompromiss för tooevaluate i din design: använda fler partitioner ökar hello skalbarhet eftersom Azure har flera möjligheter för belastningsutjämning begäranden mellan noder, men detta kan begränsa hello möjligheten för dina program tooperform atomiska transaktioner och underhålla stark konsekvens för dina data. Det finns också skalbarhetsmål för specifika på hello nivå för en partition som kan begränsa hello genomströmning av transaktioner som du kan förvänta dig för en enskild nod: Mer information om hello skalbarhetsmål för Azure storage-konton och hello tabell tjänsten, se [Azure Storage skalbarhets- och prestandamål](../storage/common/storage-scalability-targets.md). Senare avsnitt i den här handboken beskrivs olika design strategier som kan hjälpa dig att hantera avvägningarna, till exempel den här och diskutera hur du bäst toochoose din partitionsnyckel baserat på hello särskilda krav på ditt klientprogram.  

### <a name="capacity-considerations"></a>Överväganden för kapacitetsplanering
hello innehåller följande tabell några av hello nyckelvärden toobe medveten om när du designar en lösning för tabell:  

| Total kapacitet för ett Azure storage-konto | 500 TB |
| --- | --- |
| Antalet tabeller i ett Azure storage-konto |Begränsas bara av hello kapacitet hello storage-konto |
| Antalet partitioner i en tabell |Begränsas bara av hello kapacitet hello storage-konto |
| Antal entiteter i en partition |Begränsas bara av hello kapacitet hello storage-konto |
| Storleken på en enskild entitet |Upp too1 MB med högst 255 egenskaper (inklusive hello **PartitionKey**, **RowKey**, och **tidsstämpel**) |
| Storleken på hello **PartitionKey** |En sträng in storlek too1 KB |
| Storleken på hello **RowKey** |En sträng in storlek too1 KB |
| Storleken på en entitet grupp-transaktion |En transaktion får innehålla högst 100 entiteter och hello nyttolasten måste vara mindre än 4 MB i storlek. En entitet kan bara uppdatera en gång i en EGT. |

Mer information finns i [hello förstå tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx).  

### <a name="cost-considerations"></a>Kostnad överväganden
Table storage är relativt billig, men du bör innehålla kostnaderna för båda kapacitet användnings- och hello antal transaktioner som en del av din utvärderingsversion av någon lösning som använder hello tabelltjänsten. I många scenarier för att lagra data för Avnormaliserade eller dubbletter i ordning tooimprove hello är prestanda och skalbarhet för din lösning dock en giltig metod tootake. Mer information om priser finns [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="guidelines-for-table-design"></a>Riktlinjer för tabelldesign
Listorna sammanfattas några av hello viktiga riktlinjer som du bör tänka på när du utformar dina tabeller och den här guiden att behandla dem i detalj senare i. Dessa riktlinjer är bland annat hello riktlinjer som du vanligtvis utför för relationsdatabas design.  

Designa din tabell service lösning toobe *läsa* effektivt:

* ***Design för frågor i Läs-aktiverat program.*** När du utformar dina tabeller tänka hello-frågor (särskilt hello svarstid känsliga de) som du ska köra innan du tycker om hur du ska uppdatera-enheterna. Detta ger vanligtvis en effektiv och performant lösning.  
* ***Ange både PartitionKey och RowKey i dina frågor.*** *Peka frågor* som dessa är hello effektivaste tabellen service frågor.  
* ***Överväg att lagra dubbletter av enheter.*** Table storage är billiga så Överväg lagra hello samma entitet flera gånger (med olika nycklar) tooenable effektivare frågor.  
* ***Överväg att denormalizing dina data.*** Table storage är billiga så Överväg denormalizing dina data. Lagra exempelvis sammanfattning enheter så att frågor för att samla in data behöver bara tooaccess en enda entitet.  
* ***Använd sammansatt nyckelvärden.*** hello endast nycklar som du har är **PartitionKey** och **RowKey**. Till exempel använda sammansatta nyckelvärden tooenable alternativ nycklad åtkomst sökvägar tooentities.  
* ***Använd fråga projektion.*** Du kan minska hello mängden data som du överför hello nätverket med hjälp av frågor som väljer bara hello fält som du behöver.  

Designa din tabell service lösning toobe *skriva* effektivt:  

* ***Skapa inte varm partitioner.*** Välj tangenter som gör att du toospread dina begäranden mellan flera partitioner vid någon tidpunkt.  
* ***Undvika toppar i trafiken.*** Utjämna hello trafik under en rimlig tid och undvika toppar i trafiken.
* ***Inte nödvändigtvis att skapa en separat tabell för varje typ av enhet.*** När du behöver atomiska transaktioner över typer av enheter du kan lagra dessa flera typer av enheter i samma partition i hello hello samma tabell.
* ***Överväg att hello maximalt dataflöde du måste uppnå.*** Du måste vara medveten om hello skalbarhetsmål för hello tabelltjänsten och se till att designen inte ska orsaka du tooexceed dem.  

När du läser handboken visas exempel placera alla dessa principer i praktiken.  

## <a name="design-for-querying"></a>Design för frågor
Tabell tjänstelösningar får läsas intensiva, Skriv intensiva eller en blandning av hello två. Det här avsnittet fokuserar på hello saker toobear i åtanke när du utformar din tabell service toosupport läsåtgärder effektivt. Vanligtvis är en design som stöder läsåtgärder effektivt också effektivt för skrivåtgärder. Men det finns ytterligare överväganden toobear i åtanke när designa toosupport skrivåtgärder, beskrivs i nästa avsnitt hello [Design för dataändring](#design-for-data-modification).

En bra utgångspunkt för att utforma din tabell service lösning tooenable tooread data effektivt är tooask ”vilka frågor kommer Mina program behöver tooexecute tooretrieve hello data måste från hello tabelltjänsten”?  

> [!NOTE]
> Hello tabelltjänsten, är det viktigt tooget hello design rätt direkt eftersom det är svåra och dyra toochange senare. Till exempel indexerar möjliga tooaddress prestandaproblem genom att lägga till i en relationsdatabas är det ofta tooan befintlig databas: Detta är inte ett alternativ med hello tabelltjänsten.  
> 
> 

Det här avsnittet fokuserar på hello viktiga problem som du måste ta när du designar tabeller för frågor. hello behandlar i det här avsnittet:

* [Hur du väljer PartitionKey och RowKey påverkar prestanda för frågor](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [Att välja en lämplig PartitionKey](#choosing-an-appropriate-partitionkey)
* [Optimera frågor för hello tabelltjänst](#optimizing-queries-for-the-table-service)
* [Sortera data i hello tabelltjänst](#sorting-data-in-the-table-service)

### <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>Hur du väljer PartitionKey och RowKey påverkar prestanda för frågor
hello följande exempel förutsätter hello tabelltjänsten lagrar medarbetare entiteter med följande struktur hello (de flesta av hello exempel utelämna hello **tidsstämpel** egenskapen för tydlighetens skull):  

| *Kolumnnamn* | *Datatyp* |
| --- | --- |
| **PartitionKey** (avdelningsnamn) |Sträng |
| **RowKey** (anställnings-Id) |Sträng |
| **Förnamn** |Sträng |
| **Efternamn** |Sträng |
| **Ålder** |Integer |
| **E-postadress** |Sträng |

Hej tidigare avsnittet [översikt över Azure Table](#overview) beskrivs några av hello viktiga funktioner i hello Azure Table-tjänsten som har en direkt inverkan på utformning för frågan. Dessa resultera i hello följande allmänna riktlinjer för att utforma tabellen service frågor. Observera att hello filtersyntaxen används i hello exemplen nedan är från hello tabelltjänsten REST API för mer information finns i [fråga entiteter](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

* En ***punkt frågan*** är hello effektivaste sökning toouse och rekommenderas toobe som används för omfattande sökningar eller sökningar som kräver lägsta svarstid. Sådan fråga kan använda hello index toolocate en enskild entitet mycket effektivt genom att ange både hello **PartitionKey** och **RowKey** värden. Till exempel: $filter = (PartitionKey eq 'Sales') och (RowKey eq '2')  
* Andra bäst är en ***intervallet frågan*** som använder hello **PartitionKey** och filter på en mängd **RowKey** värden tooreturn mer än en entitet. Hej **PartitionKey** värdet identifierar en specifik partition och hello **RowKey** identifierar en delmängd av hello entiteter i partitionen. Till exempel: $filter = PartitionKey eq 'Försäljning och RowKey ge' och RowKey lt t '  
* Tredje bästa är en ***Partition skanna*** som använder hello **PartitionKey** och filter på en annan icke-nyckelegenskapen och som kan returnera flera enheter. Hej **PartitionKey** värdet identifierar en specifik partition och hello egenskapen värden väljer för en delmängd av hello entiteter i partitionen. Till exempel: $filter = PartitionKey eq 'Försäljning' och efternamn eq ”Smith”  
* En ***tabell skanna*** inkluderar inte hello **PartitionKey** och är mycket ineffektiv eftersom den söker igenom alla hello partitioner som utgör din tabell i sin tur för alla matchande entiteter. Utför en tabellgenomsökning oavsett om huruvida filtret använder hello **RowKey**. Till exempel: $filter = efternamn eq 'Karlsson'  
* Frågor som returnerar flera entiteter tillbaka dem i **PartitionKey** och **RowKey** ordning. tooavoid sortera hello enheterna hello-klienten väljer en **RowKey** som definierar hello vanligaste sorteringsordning.  

Observera att använda en ”**eller**” toospecify ett filter baserat på **RowKey** värden resulterar i en partition genomsökning och inte behandlas som en fråga för intervallet. Därför bör du undvika frågor som använder filter exempelvis: $filter = PartitionKey eq 'Sales' och (RowKey eq '121' eller RowKey eq '322')  

Exempel på klientsidan kod som använder hello Storage-klientbibliotek tooexecute effektiva frågor finns:  

* [Köra en punkt-fråga med hello Storage-klientbibliotek](#executing-a-point-query-using-the-storage-client-library)
* [Hämtning av flera enheter med hjälp av LINQ](#retrieving-multiple-entities-using-linq)
* [Projektion av serversidan](#server-side-projection)  

Exempel på klientsidan kod som kan hantera flera entitet typer lagras i hello samma tabell, se:  

* [Arbeta med heterogena entitetstyper](#working-with-heterogeneous-entity-types)  

### <a name="choosing-an-appropriate-partitionkey"></a>Att välja en lämplig PartitionKey
Ditt val av **PartitionKey** ska utjämna hello måste tooenables hello användning av EGTs (tooensure konsekvenskontroll) mot hello krav toodistribute entiteterna mot flera partitioner (tooensure en skalbar lösning).  

Vid en extreme kan du lagra alla entiteter i en enda partition, men detta kan begränsa hello skalbarhet för din lösning och hello tabelltjänsten skulle förhindra att kan tooload-begäranden. Vid hello andra extreme kan du lagra en entitet per partition, vilket är mycket skalbart och som kan hello tabell tjänstbegäranden tooload saldo, men som skulle kunna hindra att du använder enheten grupptransaktioner.  

Perfekt **PartitionKey** är en som du kan använda toouse effektiva frågor och som har tillräcklig partitioner tooensure din lösning är skalbart. Du hittar normalt att-enheterna kommer att ha en lämplig egenskap som distribuerar entiteter i tillräcklig partitioner.

> [!NOTE]
> I ett system som lagrar information om användare eller anställda kanske användar-ID för en bra PartitionKey. Du kan ha flera enheter som använder en viss användar-ID som hello partitionsnyckel. Varje entitet som lagrar data om en användare är grupperade i en enda partition och så dessa enheter är tillgängliga via entitet grupptransaktioner, samtidigt som de skalbara.
> 
> 

Det finns fler överväganden i valfri **PartitionKey** som relaterar toohow ska du infoga, uppdatera och ta bort entiteter: hello i avsnittet [Design för dataändring](#design-for-data-modification) nedan.  

### <a name="optimizing-queries-for-hello-table-service"></a>Optimera frågor för hello tabelltjänst
Hej tabelltjänsten indexerar automatiskt dina enheter med hjälp av hello **PartitionKey** och **RowKey** värden i ett enda grupperat index därför hello orsak att punkt frågor är hello effektivaste toouse . Det finns dock några index än det som på hello grupperade indexet för hello **PartitionKey** och **RowKey**.

Många-Designer måste uppfylla kraven tooenable sökning efter enheter baserat på flera villkor. Hitta medarbetare enheter baserat på e-post, till exempel anställnings-id eller efternamn. hello följande mönster i hello avsnittet [tabell designmönster](#table-design-patterns) dessa typer av krav och beskriver fungerar runt hello faktum att hello tabelltjänsten inte ger sekundärindex på olika sätt:  

* [Intra-partition sekundärt index mönster](#intra-partition-secondary-index-pattern) -lagra flera kopior av varje enhet med hjälp av olika **RowKey** värden (i hello samma partition) tooenable snabba och effektiva sökningar och alternativa sorteringen sorterar med hjälp av olika **RowKey** värden.  
* [Mellan sekundära Partitionsindex mönster](#inter-partition-secondary-index-pattern) – lagra flera kopior av varje entitet med olika RowKey värden i olika partitioner eller i separata tabeller tooenable snabb och effektiv sökningar och alternativa sorteringen sorterar med hjälp av olika **RowKey** värden.  
* [Index entiteter mönster](#index-entities-pattern) -Underhåll entiteter tooenable effektiva sökningar som returnerar en lista över enheter.  

### <a name="sorting-data-in-hello-table-service"></a>Sortera data i hello tabelltjänst
Hej tabelltjänsten returnerar entiteter sorterade i stigande ordning utifrån **PartitionKey** och sedan efter **RowKey**. Nycklarna är strängvärden och tooensure numeriska värden sorteras korrekt, bör du konvertera dem tooa fast längd och Fyll ut dem med nollor. Till exempel, om hello medarbetare ID-värde du använda hello **RowKey** är ett heltal, bör du konvertera anställnings-id **123** för**00000123**.  

Många program har krav toouse data sorteras i olika ordning: till exempel sortera anställda efter namn eller ansluta till datum. hello följande mönster i hello avsnittet [tabell designmönster](#table-design-patterns) adressen hur tooalternate sorteringsordningar för dina enheter:  

* [Intra-partition sekundärt index mönster](#intra-partition-secondary-index-pattern) -lagra flera kopior av varje entitet med olika värden för RowKey (i hello samma partition) tooenable snabba och effektiva sökningar och alternativa sorteringen sorterar med hjälp av olika RowKey värden.  
* [Mellan sekundära Partitionsindex mönster](#inter-partition-secondary-index-pattern) – lagra flera kopior av varje entitet med olika RowKey värden i separata partitioner i separata tabeller tooenable snabb och effektiv sökningar och alternativa sorteringen sorterar med hjälp av olika RowKey värden.
* [Loggen pilslut mönster](#log-tail-pattern) -hämta hello  *n*  entiteter senast lades tooa partition med hjälp av en **RowKey** värde som sorterar i omvänd datum och tid ordning.  

## <a name="design-for-data-modification"></a>Design för dataändring
Det här avsnittet fokuserar på hello designöverväganden för att optimera infogningar, uppdateringar, och tar bort. I vissa fall måste tooevaluate hello säkerhetsaspekten Designer optimerar för frågor mot Designer optimerar för dataändring precis som i Designer för relationsdatabaser (även om hello tekniker för att hantera hello design avvägningarna är olika i en relationsdatabas). Hej avsnittet [tabell designmönster](#table-design-patterns) beskriver vissa detaljerad designmönster för hello tabelltjänsten och beskrivs några dessa avvägningarna. Du hittar många Designer som optimerats för att fråga entiteter också fungerar bra för att ändra entiteter i praktiken.  

### <a name="optimizing-hello-performance-of-insert-update-and-delete-operations"></a>Optimera hello prestanda för insert-, uppdatera och ta bort
tooupdate eller ta bort en enhet, måste du vara kan tooidentify den med hjälp av hello **PartitionKey** och **RowKey** värden. I detta avseende valet av **PartitionKey** och **RowKey** för ändra entiteter bör följa liknande villkor tooyour val toosupport peka frågor eftersom du vill tooidentify entiteter som effektiv som möjligt. Du inte vill toouse en ineffektiv partition eller tabell genomsökning toolocate en entitet i ordning toodiscover hello **PartitionKey** och **RowKey** värden som du behöver tooupdate eller ta bort den.  

Hej följande mönster i hello avsnittet [tabell designmönster](#table-design-patterns) adress optimera prestanda för hello eller din insert, update och delete-åtgärder:  

* [Hög volym ta bort mönster](#high-volume-delete-pattern) -aktivera hello borttagningen av ett stort antal enheter genom att lagra alla hello entiteter för samtidiga borttagning i sina egna separata tabeller; du ta bort hello enheter genom att ta bort hello tabell.  
* [Serien datamönster](#data-series-pattern) -Store fullständig dataserien i en enda entitet toominimize hello antal begäranden som du gör.  
* [Wide entiteter mönster](#wide-entities-pattern) -använder flera logiska enheter som fysiska entiteter toostore med mer än 252 egenskaper.  
* [Mönster för stora entiteter](#large-entities-pattern) -Använd blob storage toostore stora värden.  

### <a name="ensuring-consistency-in-your-stored-entities"></a>Säkerställa konsekvens i lagrade entiteter
Hej andra viktiga faktorer som påverkar ditt val av nycklar för optimera dataändringar är hur tooensure konsekvens med hjälp av atomiska transaktioner. Du kan bara använda en EGT toooperate på entiteter som lagras i hello samma partition.  

Hej följande mönster i hello avsnittet [tabell designmönster](#table-design-patterns) adress hantera konsekvens:  

* [Intra-partition sekundärt index mönster](#intra-partition-secondary-index-pattern) -lagra flera kopior av varje enhet med hjälp av olika **RowKey** värden (i hello samma partition) tooenable snabba och effektiva sökningar och alternativa sorteringen sorterar med hjälp av olika **RowKey** värden.  
* [Mellan sekundära Partitionsindex mönster](#inter-partition-secondary-index-pattern) – lagra flera kopior av varje entitet med olika RowKey värden i olika partitioner eller i separata tabeller tooenable snabb och effektiv sökningar och alternativa sorteringen sorterar med hjälp av olika **RowKey** värden.  
* [Överensstämmelse transaktioner mönster](#eventually-consistent-transactions-pattern) -aktivera överensstämmelse beteende mellan partitionsgränser eller lagring system gränser med hjälp av Azure köer.
* [Index entiteter mönster](#index-entities-pattern) -Underhåll entiteter tooenable effektiva sökningar som returnerar en lista över enheter.  
* [Denormalization mönster](#denormalization-pattern) -kombinera relaterade data tillsammans i en enda entitet tooenable tooretrieve alla hello data som du behöver med en enda fråga.  
* [Serien datamönster](#data-series-pattern) -Store fullständig dataserien i en enda entitet toominimize hello antal begäranden som du gör.  

Information om entiteten grupptransaktioner i avsnittet hello [entitet gruppera transaktioner](#entity-group-transactions).  

### <a name="ensuring-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Säkerställa en design för effektiv ändringar underlättar effektiv frågor
I många fall bör alltid en design för effektiva frågor ger effektiv ändringar, men du utvärdera om så är fallet hello för din situation. Del av hello mönster i hello avsnittet [tabell designmönster](#table-design-patterns) explicit utvärdera avvägningarna mellan fråga och ändra entiteter och du bör alltid hänsyn hello kontonummer för varje typ av åtgärd.  

Hej följande mönster i hello avsnittet [tabell designmönster](#table-design-patterns) adressen avvägningarna mellan utformning för effektiva frågor och designar för effektiv dataändring:  

* [Sammansatt nyckel mönster](#compound-key-pattern) -Använd sammansatta **RowKey** värden tooenable en klient toolookup relaterade data med en enda fråga.  
* [Loggen pilslut mönster](#log-tail-pattern) -hämta hello  *n*  entiteter senast lades tooa partition med hjälp av en **RowKey** värde som sorterar i omvänd datum och tid ordning.  

## <a name="encrypting-table-data"></a>Kryptera tabelldata
Hej .NET Azure Storage-klientbibliotek har stöd för kryptering av strängen Entitetsegenskaper för insert och ersätt-åtgärder. hello krypterade strängar lagras på hello-tjänsten som binära egenskaper och de konverteras tillbaka toostrings efter dekrypteringen.    

För tabeller, dessutom toohello krypteringsprincipen användare måste ange hello egenskaper toobe krypteras. Detta kan göras genom att antingen att ange attributet [EncryptProperty] (för POCO entiteter som är härledda från TableEntity) eller en kryptering matcharen begäran alternativ. En kryptering Konfliktlösaren är en delegat som tar en partitionsnyckel, radnyckel och egenskapsnamn och returnerar ett booleskt värde som anger om egenskapen ska krypteras. Under krypteringen använder hello klientbiblioteket denna information toodecide om en egenskap som ska krypteras vid skrivning till toohello överföring. Det ger också hello ombud hello möjlighet till logik runt hur egenskaperna är krypterade. (Till exempel om X, sedan kryptera egenskapen A; annars kryptera egenskaper A och b) Observera att det är inte nödvändigt tooprovide denna information vid läsning eller fråga entiteter.

Observera att koppling inte stöds för närvarande. Eftersom en delmängd av egenskaper kan har krypterats tidigare med hjälp av en annan nyckel leder bara sammanslagning hello nya egenskaper och uppdaterar hello metadata till dataförlust. Sammanslagning av antingen kräver att göra extra service anropar tooread hello befintlig entitet från hello-tjänsten eller med en ny nyckel för varje egenskap som lämpar sig inte av prestandaskäl.     

Information om hur du krypterar tabelldata finns [kryptering på klientsidan och Azure Key Vault för Microsoft Azure Storage](../storage/common/storage-client-side-encryption.md).  

## <a name="modelling-relationships"></a>Modellering relationer
Skapa domänmodeller är ett viktigt steg i hello design av komplexa system. Normalt använder du hello modellering processen tooidentify entiteter och hello relationer mellan dem som ett sätt toounderstand hello business domän och informera hello utformning av systemet. Det här avsnittet fokuserar på hur du kan översätta vissa hello vanliga relationstyper hittades i domänen modeller toodesigns för hello tabelltjänsten. hello är mappning från en logisk datamodell tooa fysiska baserat NoSQL-datamodell väldigt annorlunda än den som används när du skapar en relationsdatabas. Relationsdatabaser design förutsätter vanligtvis en process för normalisering som har optimerats för att minimera redundans – och en deklarativ frågar funktion som sammanfattningar hello hur implementeringen av hur hello databasen fungerar.  

### <a name="one-to-many-relationships"></a>En-till-många-relationer
En-till-många-relationer mellan företag domänobjekt inträffa ofta: till exempel en avdelning har många anställda. Det finns flera sätt tooimplement en-till-många-relationer i hello tabelltjänsten varje med för- och nackdelar som kan vara relevanta toohello visst scenario.  

Överväg att hello exempel på flera nationella stora företag med tusentals avdelningar och medarbetare enheter där varje avdelning har många anställda och varje medarbetare som tillhör en viss avdelning. En metod som är toostore separat avdelning och medarbetare entiteter som dessa:  

![][1]

Det här exemplet illustrerar en implicit en-till-många-relation mellan hello typer baserat på hello **PartitionKey** värde. Varje avdelning kan ha många anställda.  

Det här exemplet visar också en avdelning entitet och dess relaterade medarbetare entiteter i hello samma partition. Du kan välja toouse olika partitioner, tabeller och även storage-konton för hello olika enhetstyper.  

En annan metod är toodenormalize dina data och store endast anställda entiteter med Avnormaliserade information som visas i följande exempel hello. I det här scenariot kan då Avnormaliserade kanske inte hello bäst om du har ett krav toobe kan toochange hello information för en avdelningschef eftersom toodo detta behöver du tooupdate alla medarbetare i hello-avdelningen.  

![][2]

Mer information finns i hello [Denormalization mönster](#denormalization-pattern) senare i den här guiden.  

hello följande tabell sammanfattas hello- och nackdelar med varje hello-metoder som beskrivs ovan för att lagra medarbetare och avdelningen enheter som har en en-till-många-relation. Det bör också övervägas hur ofta du förväntar dig tooperform olika åtgärder: Det kan vara godtagbar toohave en design som innehåller en kostsam åtgärd om åtgärden endast inträffar sällan.  

<table>
<tr>
<th>Metoden</th>
<th>Tekniker</th>
<th>Nackdelar</th>
</tr>
<tr>
<td>Separata entitetstyper, samma partition i samma tabell</td>
<td>
<ul>
<li>Du kan uppdatera en avdelning entitet med en enda åtgärd.</li>
<li>Du kan använda en EGT toomaintain konsekvenskontroll om du har ett krav toomodify en avdelning enhet när du uppdatering/insert/ta bort en medarbetare entitet. Till exempel om du har ett antal avdelningens anställda för varje avdelning.</li>
</ul>
</td>
<td>
<ul>
<li>Du kan behöva tooretrieve både anställda och en avdelning entitet för vissa aktiviteter för klienten.</li>
<li>Storage-åtgärder sker i hello samma partition. Vid höga transaktionsvolymer kan detta resultera i en hotspot.</li>
<li>Du kan inte flytta en medarbetare tooa nya avdelning med hjälp av en EGT.</li>
</ul>
</td>
</tr>
<tr>
<td>Separata entitetstyper, olika partitioner eller tabeller storage-konton</td>
<td>
<ul>
<li>Du kan uppdatera en avdelning entitet eller medarbetare enhet med en enda åtgärd.</li>
<li>Detta kan hjälpa med höga transaktionsvolymer spridning hello belastningen över flera partitioner.</li>
</ul>
</td>
<td>
<ul>
<li>Du kan behöva tooretrieve både anställda och en avdelning entitet för vissa aktiviteter för klienten.</li>
<li>Du kan inte använda EGTs toomaintain konsekvenskontroll när du uppdatering/insert/ta bort en medarbetare och uppdatera en avdelning. Till exempel uppdaterar ett antal anställda i en avdelning entitet.</li>
<li>Du kan inte flytta en medarbetare tooa nya avdelning med hjälp av en EGT.</li>
</ul>
</td>
</tr>
<tr>
<td>Denormalize till samma enhetstyp</td>
<td>
<ul>
<li>Du kan hämta alla hello information du behöver med en enskild begäran.</li>
</ul>
</td>
<td>
<ul>
<li>Det kan vara dyra toomaintain konsekvenskontroll om du behöver tooupdate avdelning information (det här kräver du tooupdate alla hello anställda på en avdelning).</li>
</ul>
</td>
</tr>
</table>

Hur kan du välja mellan alternativen och vilka hello-tekniker och nackdelar som är störst, beror på dina specifika Programscenarier. Till exempel hur ofta du ändrar avdelning entiteter; behöver alla medarbetare frågor hello avdelningsnivå tilläggsinformation; hur nära är du toohello skalbarhetsgränser på partitioner eller storage-konto?  

### <a name="one-to-one-relationships"></a>1: 1-relationer
Domänmodeller kan innehålla 1: 1-relationer mellan entiteter. Om du behöver tooimplement en-till-en relation i hello tabelltjänsten, måste du också välja hur toolink hello två relaterade entiteter när du behöver tooretrieve båda. Den här länken kan vara implicit, baserat på en konvention i hello nyckelvärden eller explicit genom att lagra en länk i hello form av **PartitionKey** och **RowKey** värdena i varje entitet tooits relaterade entiteten. Mer information om huruvida du bör lagra hello relaterade entiteter i Hej samma partition, hello i avsnittet [en-till-många-relationer](#one-to-many-relationships).  

Observera att det finns också implementering som kan leda du tooimplement 1: 1-relationer i hello tabelltjänsten:  

* Hantering av stora entiteter (Mer information finns i [stora entiteter mönster](#large-entities-pattern)).  
* Implementera åtkomstkontroller (Mer information finns i [Kontrollera åtkomst med signaturer för delad åtkomst](#controlling-access-with-shared-access-signatures)).  

### <a name="join-in-hello-client"></a>Delta i hello-klienten
Även om det finns olika sätt toomodel relationer i hello tabelltjänsten, ska du inte glömma att hello två huvudsakliga skäl till att använda hello tabelltjänsten finns skalbarhet och prestanda. Om du hittar du modellering många relationer som angripa hello prestanda och skalbarhet i lösningen, bör du fråga dig själv om det är nödvändigt toobuild alla hello data relationer i tabelldesign. Du kan kan toosimplify hello design och förbättra hello skalbarhet och prestanda för din lösning om du låta ditt klientprogram som utför alla nödvändiga kopplingar.  

Till exempel om du har liten tabeller som innehåller data som inte ändras ofta kan sedan du hämta dessa data en gång och cachelagras på klienten hello. Detta kan undvika upprepade görs tooretrieve hello samma data. I hello exempel vi har tittat på i den här handboken är hello avdelningar i en liten organisation sannolikt toobe små och ändrar sällan att göra det en bra kandidat för data som klientprogram kan ladda ned en gång och cache som slå upp data.  

### <a name="inheritance-relationships"></a>Arvsrelationer
Om klientprogrammet använder en uppsättning klasser som utgör en del av ett affärsobjekt arv relationen toorepresent, kan du enkelt sparas dessa enheter i hello tabelltjänsten. Du kan till exempel ha följande uppsättning klasser som definieras i ditt klientprogram hello där **Person** är en abstrakt klass.

![][3]

Du kan spara instanser av hello två konkreta klasser i hello tabelltjänsten med hjälp av en enskild Person tabell med entiteter i den ser ut så här:  

![][4]

Mer information om hur du arbetar med flera typer av enheter i samma tabell i klientkod hello avsnittet hello [arbeta med heterogena entitetstyper](#working-with-heterogeneous-entity-types) senare i den här guiden. Detta ger exempel på hur toorecognize hello entitetstypen i klientkod.  

## <a name="table-design-patterns"></a>Designmönster för tabellen
Du har sett vissa detaljerad information om hur toooptimize tabellen-design för både hämtades entitetsdata med hjälp av frågor och infoga, uppdatera och ta bort entitetsdata i föregående avsnitt. Det här avsnittet beskrivs vissa mönster som är lämpliga för användning med tjänstelösningar för tabellen. Dessutom visas hur du praktiskt taget kan lösa vissa problem med hello och avvägningarna aktiveras tidigare i den här guiden. hello visas följande diagram hello relationer mellan hello olika mönster:  

![][5]

hello mönster kartan ovan visar relationer mellan mönster (blå) och ett mönster (orange) som finns dokumenterade i handboken. Det är naturligtvis många mönster som är värda att ta hänsyn till. Till exempel en av hello viktiga scenarier för Tabelltjänsten är toouse hello [Materialiserade vyn mönster](https://msdn.microsoft.com/library/azure/dn589782.aspx) från hello [kommandot frågan ansvar ansvarsfördelning (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) mönster.  

### <a name="intra-partition-secondary-index-pattern"></a>Intra-partition sekundärt index mönster
Lagra flera kopior av varje enhet med hjälp av olika **RowKey** värden (i hello samma partition) tooenable snabba och effektiva sökningar och alternativa sorteringen sorterar med hjälp av olika **RowKey** värden. Uppdateringar mellan kopior kan vara konsekvent med EGT'S.  

#### <a name="context-and-problem"></a>Kontexten och problem
Hej tabelltjänsten indexerar automatiskt enheter med hjälp av hello **PartitionKey** och **RowKey** värden. Detta gör att en klient programmet tooretrieve en entitet som effektivt använder dessa värden. Till exempel använder hello tabellstrukturen som visas nedan, ett klientprogram kan använda en punkt frågan tooretrieve en enskild medarbetare entitet med hjälp av hello avdelningsnamn och hello anställnings-id (hello **PartitionKey** och  **RowKey** värden). En klient kan också hämta entiteter sorterade efter anställnings-id inom varje avdelning.

![][6]

Om du även vill toobe kan toofind en medarbetare entitet baserat på hello-värdet för en annan egenskap, t.ex e-postadress, måste du använda en mindre effektiv partition genomsökning toofind en matchning. Det beror på att hello tabelltjänsten inte ger sekundärindex. Dessutom finns inga alternativ toorequest en lista över anställda sorterade i en annan ordning än **RowKey** ordning.  

#### <a name="solution"></a>Lösning
toowork runt hello bristande sekundärindex kan du lagra flera kopior av varje entitet med varje kopia med ett annat **RowKey** värde. Om du sparar en entitet med hello-strukturer som visas nedan, kan du effektivt hämta medarbetare enheter baserat på e-postadress eller medarbetare id. Hej prefixvärden för hello **RowKey**, ”empid_” och ”email_” kan du tooquery för en medarbetare eller ett intervall med anställda med hjälp av en mängd e-postadresser eller medarbetare-ID: n.  

![][7]

hello följande två filtervillkor (en slå upp av anställnings-id och en sökning efter av e-postadress) ange både punkt frågor:  

* $filter = (PartitionKey eq 'Sales') och (RowKey eq 'empid_000223')  
* $filter = (PartitionKey eq 'Sales') och (RowKey eq 'email_jonesj@contoso.com')  

Om du frågar efter ett intervall med anställdas enheter du kan ange ett intervall i medarbetare id ordning eller ett intervall som är sorterad i e-postadress ordning genom att fråga om entiteter med hello rätt prefix i hello **RowKey**.  

* toofind alla hello anställda på försäljningsavdelningen hello med medarbetare id i hello intervallet 000100 too000199 använder: $filter = (PartitionKey eq 'Sales') och (RowKey ge 'empid_000100') och (RowKey le 'empid_000199')  
* toofind alla hello anställda på försäljningsavdelningen hello med en e-postadress som börjar med hello bokstaven ”a” Använd: $filter = (PartitionKey eq 'Sales') och (RowKey ge 'email_a') och (RowKey lt 'email_b')  
  
  Observera att hello filtersyntaxen används i hello exemplen ovan är från hello tabelltjänsten REST API för mer information finns i [fråga entiteter](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Problem och överväganden
Överväg att hello följande punkter när du bestämmer hur tooimplement detta mönster:  

* Table storage är relativt billig toouse så hello kostnad arbetet med att lagra dubblettdata inte får vara ett större problem. Du bör dock alltid utvärdera hello kostnaden av din design utifrån din förväntade lagringsbehov och bara lägga till dubbla entiteter toosupport hello frågor client-program körs.  
* Eftersom hello sekundärt index entiteter lagras i hello samma partition som hello ursprungliga enheter, bör du kontrollera att du inte överskrider hello skalbarhetsmål för en enskild partition.  
* Du kan behålla-dubbla enheterna överensstämmer med varandra med hjälp av EGTs tooupdate hello två kopior av hello enheten automatiskt. Detta innebär att du bör lagra alla kopior av en entitet i hello samma partition. Mer information finns i avsnittet hello [med hjälp av entiteten gruppera transaktioner](#entity-group-transactions).  
* hello-värde som används för hello **RowKey** måste vara unikt för varje entitet. Överväg att använda sammansatta nyckelvärden.  
* Utfyllnad numeriska värden i hello **RowKey** (exempelvis hello anställnings-id 000223), aktiverar korrigera sortera och filtrera baserat på övre och nedre gränser.  
* Du behöver inte nödvändigtvis tooduplicate alla hello egenskaperna för entiteten. Till exempel om hello frågor som hello uppslagsentiteter med hello e-post-adressen i hello **RowKey** behöver aldrig hello medarbetarens ålder, dessa enheter kan ha hello följande struktur:

![][8]

* Det är vanligtvis bättre toostore duplicerade data och se till att du kan hämta alla hello data som du behöver med en enda fråga, än toouse en fråga toolocate en entitet och en annan toolookup hello obligatoriska uppgifter.  

#### <a name="when-toouse-this-pattern"></a>När toouse det här mönstret
Använd det här mönstret när klientprogrammet måste tooretrieve enheter med hjälp av en mängd olika nycklar när klienten måste tooretrieve entiteter i olika sorteringsordningar, och där du kan identifiera varje entitet med olika unika värden. Dock bör du se till att du inte överskrider hello partition skalbarhetsgränser när du utför entitet sökningar med olika hello **RowKey** värden.  

#### <a name="related-patterns-and-guidance"></a>Vägledning och relaterade mönster
hello kan följande mönster och guider även vara relevanta när du implementerar det här mönstret:  

* [Mellan sekundära Partitionsindex mönster](#inter-partition-secondary-index-pattern)
* [Sammansatt nyckel mönster](#compound-key-pattern)
* [Entiteten gruppera transaktioner](#entity-group-transactions)
* [Arbeta med heterogena entitetstyper](#working-with-heterogeneous-entity-types)

### <a name="inter-partition-secondary-index-pattern"></a>Mellan sekundära Partitionsindex mönster
Lagra flera kopior av varje enhet med hjälp av olika **RowKey** värden i olika partitioner eller i separata tabeller tooenable snabba och effektiva sökningar och alternativa sorteringsordningen genom att använda olika **RowKey**värden.  

#### <a name="context-and-problem"></a>Kontexten och problem
Hej tabelltjänsten indexerar automatiskt enheter med hjälp av hello **PartitionKey** och **RowKey** värden. Detta gör att en klient programmet tooretrieve en entitet som effektivt använder dessa värden. Till exempel använder hello tabellstrukturen som visas nedan, ett klientprogram kan använda en punkt frågan tooretrieve en enskild medarbetare entitet med hjälp av hello avdelningsnamn och hello anställnings-id (hello **PartitionKey** och  **RowKey** värden). En klient kan också hämta entiteter sorterade efter anställnings-id inom varje avdelning.  

![][9]

Om du även vill toobe kan toofind en medarbetare entitet baserat på hello-värdet för en annan egenskap, t.ex e-postadress, måste du använda en mindre effektiv partition genomsökning toofind en matchning. Det beror på att hello tabelltjänsten inte ger sekundärindex. Dessutom finns inga alternativ toorequest en lista över anställda sorterade i en annan ordning än **RowKey** ordning.  

Du förutse en mycket stor volym med transaktioner mot dessa enheter och vill toominimize hello risken för hello tabelltjänsten begränsning av klienten.  

#### <a name="solution"></a>Lösning
toowork runt hello bristande sekundärindex kan du lagra flera kopior av varje entitet med varje kopia med hjälp av olika **PartitionKey** och **RowKey** värden. Om du sparar en entitet med hello-strukturer som visas nedan, kan du effektivt hämta medarbetare enheter baserat på e-postadress eller medarbetare id. Hej prefixvärden för hello **PartitionKey**, ”empid_” och ”email_” kan du tooidentify som index som du vill använda toouse för en fråga.  

![][10]

hello följande två filtervillkor (en slå upp av anställnings-id och en sökning efter av e-postadress) ange både punkt frågor:  

* $filter = (PartitionKey eq ' empid_Sales') och (RowKey eq '000223')
* $filter = (PartitionKey eq ' email_Sales') och (RowKey eq 'jonesj@contoso.com')  

Om du frågar efter ett intervall med anställdas enheter du kan ange ett intervall i medarbetare id ordning eller ett intervall som är sorterad i e-postadress ordning genom att fråga om entiteter med hello rätt prefix i hello **RowKey**.  

* toofind alla hello anställda på hello försäljningsavdelningen med medarbetare id i hello intervallet **000100** för**000199** sorteras medarbetare id ordning används: $filter = (PartitionKey eq ' empid_Sales') och (RowKey ge ' 000100') och (RowKey le '000199')  
* toofind alla hello medarbetare i hello försäljningsavdelningen med en e-postadress som börjar med ”a” i e-postadress ordning används: $filter = (PartitionKey eq ' email_Sales') och (RowKey ge ”a”) och (RowKey lt ”b”)  

Observera att hello filtersyntaxen används i hello exemplen ovan är från hello tabelltjänsten REST API för mer information finns i [fråga entiteter](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Problem och överväganden
Överväg att hello följande punkter när du bestämmer hur tooimplement detta mönster:  

* Du kan behålla din dubbla entiteter överensstämmelse med varandra med hjälp av hello [överensstämmelse transaktioner mönster](#eventually-consistent-transactions-pattern) toomaintain hello primära och sekundära index entiteter.  
* Table storage är relativt billig toouse så hello kostnad arbetet med att lagra dubblettdata inte får vara ett större problem. Du bör dock alltid utvärdera hello kostnaden av din design utifrån din förväntade lagringsbehov och bara lägga till dubbla entiteter toosupport hello frågor client-program körs.  
* hello-värde som används för hello **RowKey** måste vara unikt för varje entitet. Överväg att använda sammansatta nyckelvärden.  
* Utfyllnad numeriska värden i hello **RowKey** (exempelvis hello anställnings-id 000223), aktiverar korrigera sortera och filtrera baserat på övre och nedre gränser.  
* Du behöver inte nödvändigtvis tooduplicate alla hello egenskaperna för entiteten. Till exempel om hello frågor som hello uppslagsentiteter med hello e-post-adressen i hello **RowKey** behöver aldrig hello medarbetarens ålder, dessa enheter kan ha hello följande struktur:
  
  ![][11]
* Det är vanligtvis bättre toostore duplicerade data och se till att du kan hämta alla hello-data som du behöver med en enskild fråga än toouse en fråga toolocate en entitet med hello sekundärt index och en annan toolookup hello nödvändiga data i hello primärindex.  

#### <a name="when-toouse-this-pattern"></a>När toouse det här mönstret
Använd det här mönstret när klientprogrammet måste tooretrieve enheter med hjälp av en mängd olika nycklar när klienten måste tooretrieve entiteter i olika sorteringsordningar, och där du kan identifiera varje entitet med olika unika värden. Använd det här mönstret när du vill tooavoid överstiger hello partition skalbarhetsgränser när du utför entitet sökningar med olika hello **RowKey** värden.  

#### <a name="related-patterns-and-guidance"></a>Vägledning och relaterade mönster
hello kan följande mönster och guider även vara relevanta när du implementerar det här mönstret:  

* [Mönster för överensstämmelse transaktioner](#eventually-consistent-transactions-pattern)  
* [Intra-partition sekundärt index mönster](#intra-partition-secondary-index-pattern)  
* [Sammansatt nyckel mönster](#compound-key-pattern)  
* [Entiteten gruppera transaktioner](#entity-group-transactions)  
* [Arbeta med heterogena entitetstyper](#working-with-heterogeneous-entity-types)  

### <a name="eventually-consistent-transactions-pattern"></a>Mönster för överensstämmelse transaktioner
Aktivera överensstämmelse beteende mellan partitionsgränser eller lagring system gränser med hjälp av Azure köer.  

#### <a name="context-and-problem"></a>Kontexten och problem
EGTs aktivera atomiska transaktioner över flera enheter som delar hello samma partitionsnyckel. Du kan bestämma toostore entiteter som har konsekvenskontroll krav i separata partitioner eller i ett separat lagringssystem för prestanda och skalbarhet: du kan inte använda EGTs toomaintain konsekvens i ett sådant scenario. Du kan till exempel ha en krav toomaintain slutliga konsekvensen mellan:  

* Entiteter som lagras i två olika partitioner i hello samma tabell, i olika tabeller i i olika lagringskonton.  
* En entitet som lagras i hello tabelltjänsten och en blob som lagras i hello Blob-tjänsten.  
* En entitet som lagras i tabelltjänsten hello och en fil i ett filsystem.  
* En entitet lagra i hello tabelltjänsten har indexerat med hello Azure Search-tjänsten.  

#### <a name="solution"></a>Lösning
Du kan implementera en lösning som ger slutliga konsekvensen mellan två eller flera partitioner lagringssystem med hjälp av Azure köer.
tooillustrate detta hanterar, förutsätter att du har ett krav toobe kan tooarchive gamla medarbetare-enheter. Den gamla medarbetare enheter frågas sällan och bör undantas från alla aktiviteter som handlar om aktuella anställda. tooimplement detta krav som du lagrar aktiva medarbetare i hello **aktuella** tabell och tidigare anställda i hello **Arkiv** tabell. Arkivering av medarbetaren kräver toodelete hello entitet från hello **aktuella** tabell och lägga till hello entiteten toohello **Arkiv** , men du kan inte använda en EGT tooperform dessa två åtgärder. tooavoid hello risk att ett fel orsakar en entitet tooappear i båda eller inget tabeller, hello arkivåtgärden måste vara överensstämmelse. hello beskriver följande sekvensdiagram hello steg i den här åtgärden. Mer information ges för undantag sökvägar i hello texten nedan.  

![][12]

En klient initierar hello Arkiv igen genom att placera ett meddelande på en Azure-kö, i det här exemplet tooarchive anställd #456. En arbetsroll avsöker hello kön för nya meddelanden. När den hittar en läser hello-meddelande och lämnar en dold kopia på hello kön. Hej arbetsrollen bredvid hämtar en kopia av hello entitet från hello **aktuella** tabell, infogar en kopia i hello **Arkiv** tabell och borttagningar hello ursprungliga från hello **aktuella**tabell. Slutligen, om det inte fanns några fel från hello föregående steg, hello worker-rollen tar bort dolda hello-meddelande från hello kön.  

I det här exemplet steg 4 infogas hello medarbetare hello **Arkiv** tabell. Det gick att lägga till hello medarbetare tooa blob i hello Blob-tjänsten eller en fil i ett filsystem.  

#### <a name="recovering-from-failures"></a>Återställa från fel
Det är viktigt som hello åtgärder i steg **4** och **5** måste vara *idempotent* om hello arbetsrollen behöver toorestart hello Arkiv igen. Om du använder hello tabelltjänsten för steget **4** bör du använda en ”infoga eller ersätta” åtgärd; för steg **5** bör du använda en ”ta bort om finns” åtgärden i hello klientbiblioteket som du använder. Om du använder en annan lagringssystemet, måste du använda en lämplig idempotent-åtgärd.  

Om hello arbetsrollen slutförs aldrig steg **6**, sedan efter en tidsgräns hello-meddelande dyker upp i hello kön är klara för hello worker-rollen tootry tooreprocess den. hello worker-rollen kan kontrollera hur många gånger meddelandet i kön hello har lästs och eventuellt Flagga det är ett ”skadligt” meddelande för undersökningen genom att skicka den tooa separata kön. Mer information om läsning Kömeddelanden och kontrollerar hello har status Created antal, se [få meddelanden](https://msdn.microsoft.com/library/azure/dd179474.aspx).  

Fel från hello tabell och kön services är tillfälligt fel och ditt klientprogram ska innehålla lämplig försök logik toohandle dem.  

#### <a name="issues-and-considerations"></a>Problem och överväganden
Överväg att hello följande punkter när du bestämmer hur tooimplement detta mönster:  

* Den här lösningen ger inte för transaktionsisoleringen. En klient kan till exempel läsa hello **aktuella** och **Arkiv** tabeller när hello arbetsrollen mellan stegen **4** och **5**, och se en Inkonsekvent visning av hello data. Observera att hello data blir konsekvent förr eller senare.  
* Du måste vara säker på att steg 4 och 5 är idempotent i ordning tooensure slutliga konsekvensen.  
* Du kan skala hello lösning med flera köer och worker rollinstanser.  

#### <a name="when-toouse-this-pattern"></a>När toouse det här mönstret
Använd det här mönstret när du vill tooguarantee slutliga konsekvensen mellan enheter som finns i olika partitioner eller tabeller. Du kan utöka det här mönstret tooensure slutliga konsekvensen för åtgärder över hello tabelltjänsten och hello Blob-tjänsten och andra Azure Storage-datakällor, till exempel databasen eller hello-filsystem.  

#### <a name="related-patterns-and-guidance"></a>Vägledning och relaterade mönster
hello kan följande mönster och guider även vara relevanta när du implementerar det här mönstret:  

* [Entiteten gruppera transaktioner](#entity-group-transactions)  
* [Merge eller ersätta](#merge-or-replace)  

> [!NOTE]
> Om transaktionsisoleringen är viktiga tooyour lösning, bör du designar dina tabeller tooenable du toouse EGTs.  
> 
> 

### <a name="index-entities-pattern"></a>Index entiteter mönster
Underhålla entiteter tooenable effektiva sökningar som returnerar en lista över enheter.  

#### <a name="context-and-problem"></a>Kontexten och problem
Hej tabelltjänsten indexerar automatiskt enheter med hjälp av hello **PartitionKey** och **RowKey** värden. Detta gör att en klient programmet tooretrieve en entitet som effektivt med en punkt-fråga. Till exempel använder hello tabellstrukturen som visas nedan, ett klientprogram enkelt kan hämta en enskild medarbetare entitet med hjälp av hello avdelningsnamn och hello anställnings-id (hello **PartitionKey** och **RowKey** ).  

![][13]

Om du vill använda toobe kan tooretrieve en lista över anställdas enheter baserat på hello värdet för en annan icke-unikt egenskap, till exempel efternamn, måste du använda en mindre effektiv partition skanna toofind matchar snarare än att använda ett index toolook dem upp direkt. Det beror på att hello tabelltjänsten inte ger sekundärindex.  

#### <a name="solution"></a>Lösning
tooenable sökning efter efternamn med hello entitet struktur som visas ovan, måste du upprätthålla en lista över medarbetare-ID: n. Om du vill tooretrieve hello medarbetare entiteter med ett visst efternamn, till exempel Karlsson, måste du först lokalisera hello lista över medarbetare-ID för anställda med Karlsson som efternamn och hämta anställdas enheter. Det finns tre huvudsakliga alternativ för att lagra hello listor över medarbetare-ID: n:  

* Använda blob storage.  
* Skapa index entiteter i hello samma partition som hello medarbetare enheter.  
* Skapa index entiteter i en separat partition eller tabellen.  

<u>Alternativ #1: Använda blob storage</u>  

För hello första alternativet som, du skapar en blob för varje unik efternamn och i varje blobstore en lista över hello **PartitionKey** (avdelning) och **RowKey** (anställnings-id) värden för medarbetare som har det senaste Namn. När du lägger till eller ta bort en medarbetare bör du kontrollera att hello innehållet i hello relevanta blob är överensstämmelse med hello medarbetare entiteter.  

<u>Alternativ #2:</u> skapa index entiteter i hello samma partition  

För hello andra alternativet, använder du index entiteter som lagrar hello följande data:  

![][14]

Hej **EmployeeIDs** egenskapen innehåller en lista över medarbetare-ID för anställda med hello efternamn som lagras i hello **RowKey**.  

hello beskriver följande steg hello process som du bör följa när du lägger till en ny medarbetare om du använder hello andra alternativ. I det här exemplet vi lägger till en anställd med Id 000152 och efternamn Karlsson på försäljningsavdelningen hello:  

1. Hämta hello index entitet med en **PartitionKey** värdet ”Försäljning” och hello **RowKey** värdet ”Jansson”. Spara hello ETag för den här entiteten toouse i steg 2.  
2. Skapa entitet grupp transaktion (det vill säga en batchåtgärd) som infogar hello ny medarbetare entitet (**PartitionKey** värdet ”Försäljning” och **RowKey** värdet ”000152”), och uppdateringar hello index entitet ( **PartitionKey** värdet ”Försäljning” och **RowKey** värdet ”Jansson”) genom att lägga till hello nya id toohello lista över anställda i hello EmployeeIDs fältet. Mer information om entiteten grupptransaktioner finns [entitet gruppera transaktioner](#entity-group-transactions).  
3. Om hello entitet grupp transaktionen misslyckas på grund av ett fel för Optimistisk samtidighet (någon annan har bara ändrade hello index entiteten), måste toostart över från steg 1 igen.  

Du kan använda en liknande metoden toodeleting en medarbetare om du använder hello andra alternativ. Ändra en anställds efternamn är lite mer komplext eftersom du behöver tooexecute en entitet grupp transaktion som uppdaterar tre enheter: hello medarbetare entitetstyper, hello index entiteten för hello gamla efternamn och hello index entiteten för hello nya efternamn. Innan du gör ändringar i ordning tooretrieve hello ETag-värden som du kan sedan använda tooperform hello uppdateringar med hjälp av Optimistisk samtidighet måste du hämta varje entitet.  

hello beskriver följande steg hello process som du bör följa när du behöver toolook upp alla hello anställda med ett visst efternamn på en avdelning om du använder hello andra alternativ. Det här exemplet letar vi upp alla hello anställda med efternamn Karlsson på försäljningsavdelningen hello:  

1. Hämta hello index entitet med en **PartitionKey** värdet ”Försäljning” och hello **RowKey** värdet ”Jansson”.  
2. Parsa hello lista över anställnings-ID i hello EmployeeIDs fältet.  
3. Om du behöver ytterligare information om var och en av dessa anställda (till exempel sina e-postadresser) att hämta varje hello medarbetare enheter med hjälp av **PartitionKey** värdet ”Försäljning” och **RowKey** värden från hello lista över anställda som du hämtade i steg 2.  

<u>Alternativet #3:</u> skapa index entiteter i en separat partition eller tabell  

Använd index entiteter som lagrar hello följande data för hello tredje alternativet:  

![][15]

Hej **EmployeeIDs** egenskapen innehåller en lista över medarbetare-ID för anställda med hello efternamn som lagras i hello **RowKey**.  

Du kan inte använda EGTs toomaintain konsekvenskontroll eftersom hello index entiteter i en separat partition från hello medarbetare entiteter med hello tredje alternativet. Du bör kontrollera att hello index entiteter är överensstämmelse med hello medarbetare entiteter.  

#### <a name="issues-and-considerations"></a>Problem och överväganden
Överväg att hello följande punkter när du bestämmer hur tooimplement detta mönster:  

* Denna lösning kräver minst två frågor tooretrieve matchar entiteter: tooquery hello index entiteter tooobtain hello listan över **RowKey** värden och frågar tooretrieve varje entitet i hello-listan.  
* Med hänsyn till att en enskild entitet har en maximal storlek på 1 MB, alternativ #2 och alternativet #3 i hello lösning förutsätter hello listan över medarbetare-ID: n för alla angivna efternamn aldrig är större än 1 MB. Om hello medarbetare-ID: n är sannolikt toobe som är större än 1 MB i storlek är alternativet #1 och lagra hello index data i blob storage.  
* Om du använder alternativet #2 måste (med EGTs toohandle att lägga till och ta bort anställda och ändra en anställds efternamn) du utvärdera om hello mängder transaktioner kommer hanterar hello skalbarhetsgränser i en given partition. Om så är fallet hello bör du överväga en överensstämmelse lösning (#1 eller #3) som använder köer toohandle hello update begär och aktiverar du toostore index entiteter i en separat partition från hello medarbetare entiteter.  
* Alternativet #2 i den här lösningen förutsätter att du vill att toolook efter efternamn inom en avdelning: till exempel önskade tooretrieve en lista över anställda med efternamn Karlsson i hello försäljningsavdelningen. Om du vill toobe kan toolook upp alla hello anställda med efternamn Karlsson över hello hela organisationen kan använda alternativet #1 eller alternativet #3.
* Du kan implementera en lösning med kön som levererar slutliga konsekvensen (se hello [överensstämmelse transaktioner mönster](#eventually-consistent-transactions-pattern) för mer information).  

#### <a name="when-toouse-this-pattern"></a>När toouse det här mönstret
Använd det här mönstret när du vill toolookup en mängd av entiteter med samma vanliga egenskapsvärde, till exempel alla anställda med hello efternamn Karlsson.  

#### <a name="related-patterns-and-guidance"></a>Vägledning och relaterade mönster
hello kan följande mönster och guider även vara relevanta när du implementerar det här mönstret:  

* [Sammansatt nyckel mönster](#compound-key-pattern)  
* [Mönster för överensstämmelse transaktioner](#eventually-consistent-transactions-pattern)  
* [Entiteten gruppera transaktioner](#entity-group-transactions)  
* [Arbeta med heterogena entitetstyper](#working-with-heterogeneous-entity-types)  

### <a name="denormalization-pattern"></a>Denormalization mönster
Kombinera relaterade data tillsammans i en enda entitet tooenable tooretrieve alla hello data som du behöver med en enda fråga.  

#### <a name="context-and-problem"></a>Kontexten och problem
I en relationsdatabas normalisera du vanligtvis tooremove dataduplicering som resulterar i frågor som hämtar data från flera tabeller. Om du normalisera dina data i Azure-tabeller, måste du se flera kommunikationsturer från hello klienten toohello server tooretrieve relaterade data. Till exempel med hello tabellstrukturen nedan om du behöver två avrunda resor tooretrieve hello information för en avdelning: en toofetch hello avdelning entitet som innehåller hello Resurshanterar-id och sedan en annan begäran toofetch hello Managers information i en medarbetare entitet.  

![][16]

#### <a name="solution"></a>Lösning
I stället för att lagra hello data i två separata entiteterna, denormalize hello data och behålla en kopia av information om hello manager i hello avdelning entitet. Exempel:  

![][17]

Du kan nu hämta alla hello information du behöver om en avdelning med en punkt-fråga med avdelning entiteter som lagras med dessa egenskaper finns.  

#### <a name="issues-and-considerations"></a>Problem och överväganden
Överväg att hello följande punkter när du bestämmer hur tooimplement detta mönster:  

* Det finns en kostnad som associeras med att lagra vissa data två gånger. hello prestandafördelar (följd färre begäranden toohello storage-tjänst) vanligtvis uppväger hello marginell ökade kostnader för lagring (och kostnaden delvis förskjutning genom en minskning av hello antal transaktioner som du behöver toofetch hello information på en avdelning).  
* Du måste upprätthålla hello enhetliga hello två entiteter som lagrar information om chefer. Du kan hantera hello konsekvenskontroll problemet med hjälp av EGTs tooupdate flera entiteter i en atomisk transaktion: i det här fallet hello avdelning entiteten och hello medarbetare entiteten för hello avdelning manager lagras i hello samma partition.  

#### <a name="when-toouse-this-pattern"></a>När toouse det här mönstret
Använd det här mönstret när du ofta behöver toolook in relaterad information. Det här mönstret minskar hello antalet frågor som klienten måste se tooretrieve hello data som krävs.  

#### <a name="related-patterns-and-guidance"></a>Vägledning och relaterade mönster
hello kan följande mönster och guider även vara relevanta när du implementerar det här mönstret:  

* [Sammansatt nyckel mönster](#compound-key-pattern)  
* [Entiteten gruppera transaktioner](#entity-group-transactions)  
* [Arbeta med heterogena entitetstyper](#working-with-heterogeneous-entity-types)

### <a name="compound-key-pattern"></a>Sammansatt nyckel mönster
Använd sammansatta **RowKey** värden tooenable en klient toolookup relaterade data med en enda fråga.  

#### <a name="context-and-problem"></a>Kontexten och problem
I en relationsdatabas, är det ganska naturlig toouse kopplingar i frågor tooreturn relaterade delar av data toohello klienten i en enskild fråga. Du kan till exempel använda hello medarbetare id toolook visas en lista över relaterade entiteter som innehåller prestanda och granska data för denna medarbetare.  

Anta att du lagrar medarbetare entiteter i hello tabelltjänsten med hello följande struktur:  

![][18]

Du måste också toostore historiska data om tooreviews och prestanda för varje år hello medarbetare har arbetat för din organisation och du behöver toobe kan tooaccess informationen per år. Ett alternativ är toocreate en annan tabell som lagrar entiteter med hello följande struktur:  

![][19]

Observera att med den här hanterar du besluta tooduplicate information (till exempel förnamn och efternamn) i hello ny entitet tooenable du tooretrieve dina data med en enskild begäran. Du kan dock behålla stark konsekvens eftersom du inte kan använda en EGT tooupdate hello två entiteter automatiskt.  

#### <a name="solution"></a>Lösning
Lagra nya entitetstypen i den ursprungliga tabellen med hjälp av entiteter med hello följande struktur:  

![][20]

Observera hur hello **RowKey** nu en sammansatt nyckel består av hello anställnings-id och hello år hello granska data som du kan använda tooretrieve hello medarbetarens prestanda och granska data med en begäran för en enda entitet.  

hello följande exempel beskrivs hur du kan hämta alla hello granska data för en viss medarbetare (till exempel medarbetare 000123 i hello försäljningsavdelningen):  

$filter = (PartitionKey eq 'Sales') och (RowKey ge 'empid_000123') och (RowKey lt 'empid_000124') & $select = RowKey, Arbetsledarens, peer-klassificering, kommentarer  

#### <a name="issues-and-considerations"></a>Problem och överväganden
Överväg att hello följande punkter när du bestämmer hur tooimplement detta mönster:  

* Du bör använda ett lämpligt avgränsningstecknet som gör det enkelt tooparse hello **RowKey** värde: till exempel **000123_2012**.  
* Du också lagrar entiteten i samma partition som andra enheter som innehåller relaterade data för hello hello samma medarbetare, vilket innebär att du kan använda EGTs toomaintain stark konsekvens.
* Du bör överväga hur ofta du ska fråga hello data toodetermine om det här mönstret är lämpligt.  Om du ansluter till hello granska data mer sällan och hello huvudsakliga medarbetardata ofta bör du hålla dem som separata entiteterna.  

#### <a name="when-toouse-this-pattern"></a>När toouse det här mönstret
Använd det här mönstret när du behöver toostore en eller flera relaterade entiteter som du frågar ofta.  

#### <a name="related-patterns-and-guidance"></a>Vägledning och relaterade mönster
hello kan följande mönster och guider även vara relevanta när du implementerar det här mönstret:  

* [Entiteten gruppera transaktioner](#entity-group-transactions)  
* [Arbeta med heterogena entitetstyper](#working-with-heterogeneous-entity-types)  
* [Mönster för överensstämmelse transaktioner](#eventually-consistent-transactions-pattern)  

### <a name="log-tail-pattern"></a>Loggen pilslut mönster
Hämta hello  *n*  entiteter senast lades tooa partition med hjälp av en **RowKey** värde som sorterar i omvänd datum och tid ordning.  

#### <a name="context-and-problem"></a>Kontexten och problem
Ett vanligt krav är att kunna tooretrieve hello nyaste entiteter, till exempel hello tio senaste utgifter ansökningar som görs av en medarbetare. Tabell frågar stöd för en **$top** fråga åtgärden tooreturn hello först  *n*  entiteter från en uppsättning: det finns inga motsvarande frågan åtgärden tooreturn hello sista n entiteter i en mängd.  

#### <a name="solution"></a>Lösning
Store hello enheter med hjälp av en **RowKey** att naturligt sorterar i omvänd tidsvärdet ordning med hjälp av så att den senaste hello-posten är alltid hello förstnämnda i hello tabell.  

Till exempel toobe kan tooretrieve hello tio senaste utgifter ansökningar som görs av en medarbetare kan du använda en omvänd tick-värde som härletts från hello aktuellt datum och tid. hello följande kodexempel för C# visar enkelriktade toocreate ett lämpligt ”inverterad tick” värde för en **RowKey** som sorterar från hello senaste toohello äldsta:  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

Du kan få tillbaka toohello datum tid-värden med hjälp av hello följande kod:  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

hello tabellfråga ser ut så här:  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

#### <a name="issues-and-considerations"></a>Problem och överväganden
Överväg att hello följande punkter när du bestämmer hur tooimplement detta mönster:  

* Du måste fylla hello omvänd skalstreck värdet med inledande nollor tooensure hello strängvärde sorterar som förväntat.  
* Du måste vara medveten om hello skalbarhetsmål på hello nivå för en partition. Var noga med att inte skapa hotspot-partitioner.  

#### <a name="when-toouse-this-pattern"></a>När toouse det här mönstret
Använd det här mönstret när du behöver tooaccess entiteter i tidsvärdet i omvänd ordning eller när du behöver tooaccess hello senast har lagts till entiteter.  

#### <a name="related-patterns-and-guidance"></a>Vägledning och relaterade mönster
hello kan följande mönster och guider även vara relevanta när du implementerar det här mönstret:  

* [Lägga / lägga till ett mönster](#prepend-append-anti-pattern)  
* [Hämta entiteter](#retrieving-entities)  

### <a name="high-volume-delete-pattern"></a>Ta bort mönster för hög volym
Aktivera hello borttagning av ett stort antal enheter genom att lagra alla hello entiteter för samtidiga borttagning i sina egna separata tabell. du tar bort hello enheter genom att ta bort hello tabell.  

#### <a name="context-and-problem"></a>Kontexten och problem
Många program ta bort gamla data som inte längre behöver toobe tillgängliga tooa klientprogrammet eller programmet hello har arkiverats tooanother lagringsmedium. Dessa data identifieras vanligtvis med ett datum: du till exempel har en krav toodelete poster för alla inloggningsbegäranden som är mer än 60 dagar.  

Ett möjligt design är toouse hello datum och tid för begäran om inloggning hello i hello **RowKey**:  

![][21]

Det här sättet undviker partition surfpunkter eftersom programmet hello kan infoga och ta bort inloggningen entiteter för varje användare i en separat partition. Men den här metoden kan vara kostsamma ta lång tid om du har ett stort antal entiteter eftersom du måste först tooperform en tabell söka i ordning tooidentify alla hello entiteter toodelete och du måste ta bort varje gamla entitet. Observera att du kan minska hello antalet sändningar toohello server krävs toodelete hello gamla enheter av batchbearbetning flera delete-begäranden till EGTs.  

#### <a name="solution"></a>Lösning
Använd en separat tabell för varje dag på inloggningsförsök. Du kan använda hello entitet design ovan tooavoid surfpunkter när du infogar entiteter och ta bort gamla enheter är nu bara en fråga om du tar bort en tabell varje dag (en enda Lagringsåtgärden) i stället för att hitta och ta bort hundratals och tusentals enheter som enskilda inloggningen varje dag.  

#### <a name="issues-and-considerations"></a>Problem och överväganden
Överväg att hello följande punkter när du bestämmer hur tooimplement detta mönster:  

* Stöder din design andra sätt som programmet ska använda hello data, till exempel söka efter specifika enheter som länkar till andra data eller genererar samlar in information?  
* Din design undvika aktiva punkter vid infogning av nya enheter?  
* Förvänta dig en fördröjning om du vill tooreuse hello samma tabellnamnet när den har tagits bort. Det är bättre tooalways Använd unikt tabellnamn.  
* Räkna med vissa begränsningar när du börjar använda en ny tabell medan hello tabelltjänsten lär sig hello åtkomstmönster och distribuerar hello partitioner mellan noder. Du bör överväga hur ofta du behöver toocreate nya tabeller.  

#### <a name="when-toouse-this-pattern"></a>När toouse det här mönstret
Använd det här mönstret när du har en stor volym med enheter som du måste ta bort hello samtidigt.  

#### <a name="related-patterns-and-guidance"></a>Vägledning och relaterade mönster
hello kan följande mönster och guider även vara relevanta när du implementerar det här mönstret:  

* [Entiteten gruppera transaktioner](#entity-group-transactions)
* [Ändra entiteter](#modifying-entities)  

### <a name="data-series-pattern"></a>Serien datamönster
Store fullständig dataserien i en enda entitet toominimize hello antal begäranden som du gör.  

#### <a name="context-and-problem"></a>Kontexten och problem
Ett vanligt scenario är för ett program toostore en serie av data som vanligtvis måste tooretrieve på samma gång. Programmet kan till exempel registrera hur många Snabbmeddelanden meddelanden varje anställd skickar varje timme och sedan använda denna information tooplot hur många meddelanden varje användare hello föregående 24 timmar. En design kanske toostore 24 entiteter för varje medarbetare:  

![][22]

Med den här designen kan du lätt hitta och uppdatera hello entiteten tooupdate för varje medarbetare när programmet hello måste tooupdate hello-meddelande count-värdet. Dock tooretrieve Hej information tooplot ett diagram över hello aktivitet för hello föregående 24 timmar, måste du hämta 24 entiteter.  

#### <a name="solution"></a>Lösning
Använd följande design med antal för en separat egenskapen toostore hello-meddelande för varje timme hello:  

![][23]

Med den här designen kan använda du merge åtgärden tooupdate hello meddelandet antal för en medarbetare för en given timme. Du kan nu hämta alla hello information som du behöver tooplot hello diagram med en begäran för en enda entitet.  

#### <a name="issues-and-considerations"></a>Problem och överväganden
Överväg att hello följande punkter när du bestämmer hur tooimplement detta mönster:  

* Om fullständig dataserierna inte får plats i en enda entitet (en entitet kan ha too252 egenskaper), kan du använda ett alternativt datalager, till exempel en blob.  
* Om du har flera klienter samtidigt uppdaterar en entitet måste toouse hello **ETag** tooimplement Optimistisk samtidighet. Om du har många klienter kan uppstå det hög konkurrens.  

#### <a name="when-toouse-this-pattern"></a>När toouse det här mönstret
Använd det här mönstret när du behöver tooupdate och hämta en serie som är associerade med en enskild entitet.  

#### <a name="related-patterns-and-guidance"></a>Vägledning och relaterade mönster
hello kan följande mönster och guider även vara relevanta när du implementerar det här mönstret:  

* [Mönster för stora entiteter](#large-entities-pattern)  
* [Merge eller ersätta](#merge-or-replace)  
* [Överensstämmelse transaktioner mönster](#eventually-consistent-transactions-pattern) (om du lagrar hello dataserien i en blob)  

### <a name="wide-entities-pattern"></a>Wide entiteter mönster
Använda flera logiska enheter som fysiska entiteter toostore med mer än 252 egenskaper.  

#### <a name="context-and-problem"></a>Kontexten och problem
En enskild entitet kan ha högst 252 egenskaper (exklusive hello obligatoriska Systemegenskaper) och kan inte lagra mer än 1 MB data totalt. I en relationsdatabas, skulle du normalt får avrunda några gränser för hello storleken på en rad att lägga till en ny tabell och framtvinga en 1-till-1-relation mellan dem.  

#### <a name="solution"></a>Lösning
Du kan lagra flera entiteter toorepresent ett enda stort företag objekt med mer än 252 egenskaper med hello tabelltjänsten. Om du vill toostore antalet hello antalet IM-meddelanden som skickas av medarbetaren för hello senaste 365 dagarna, kan du använda följande design som använder två entiteter med olika scheman hello:  

![][24]

Du kan använda en EGT om du behöver toomake en ändring som behöver uppdateras både entiteter tookeep dem synkroniserade med varandra. Annars kan du använda en enda merge tooupdate hello meddelandet antal för en viss dag. tooretrieve alla hello data för en enskild medarbetare som du måste hämta båda enheter som du kan göra med två effektivt begäranden som använder både ett **PartitionKey** och en **RowKey** värde.  

#### <a name="issues-and-considerations"></a>Problem och överväganden
Överväg att hello följande punkter när du bestämmer hur tooimplement detta mönster:  

* Hämtar en fullständig logisk entitet omfattar minst två lagringstransaktioner: en tooretrieve fysisk entitet.  

#### <a name="when-toouse-this-pattern"></a>När toouse det här mönstret
Använd det här mönstret när måste toostore enheter vars storlek eller ett antal egenskaper överskrider hello gränserna för en enskild entitet i hello tabelltjänst.  

#### <a name="related-patterns-and-guidance"></a>Vägledning och relaterade mönster
hello kan följande mönster och guider även vara relevanta när du implementerar det här mönstret:  

* [Entiteten gruppera transaktioner](#entity-group-transactions)
* [Merge eller ersätta](#merge-or-replace)

### <a name="large-entities-pattern"></a>Mönster för stora entiteter
Använda blob storage toostore stora värden.  

#### <a name="context-and-problem"></a>Kontexten och problem
En enskild entitet kan inte lagra mer än 1 MB data totalt. Om en eller flera av dina egenskaper lagrar värden som gör hello totala storleken på din enhet tooexceed detta värde kan lagra du inte hello hela entiteten i hello tabelltjänsten.  

#### <a name="solution"></a>Lösning
Om entiteten överskrider 1 MB i storlek eftersom en eller flera egenskaper innehåller stora mängder data kan du lagra data i hello Blob-tjänsten och spara hello hello blob-adress i en egenskap i hello entitet. Du kan exempelvis lagra hello foto av en medarbetare i blob storage och lagrar en länk toohello foto i hello **foto** egenskap för dina medarbetare entitet:  

![][25]

#### <a name="issues-and-considerations"></a>Problem och överväganden
Överväg att hello följande punkter när du bestämmer hur tooimplement detta mönster:  

* toomaintain slutliga konsekvensen mellan hello entiteten i hello tabelltjänsten och hello data i hello Blob-tjänsten använder hello [överensstämmelse transaktioner mönster](#eventually-consistent-transactions-pattern) toomaintain-enheterna.
* Hämtar en fullständig entitet omfattar minst två lagringstransaktioner: en tooretrieve hello entitet och en tooretrieve hello blob-data.  

#### <a name="when-toouse-this-pattern"></a>När toouse det här mönstret
Använd det här mönstret när du behöver toostore enheter vars storlek överskrider hello gränserna för en enskild entitet i hello tabelltjänsten.  

#### <a name="related-patterns-and-guidance"></a>Vägledning och relaterade mönster
hello kan följande mönster och guider även vara relevanta när du implementerar det här mönstret:  

* [Mönster för överensstämmelse transaktioner](#eventually-consistent-transactions-pattern)  
* [Wide entiteter mönster](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>

### <a name="prependappend-anti-pattern"></a>Lägga/lägga till ett mönster
Öka skalbarheten när du har en stor volym med infogningar genom att sprida hello infogningar över flera partitioner.  

#### <a name="context-and-problem"></a>Kontexten och problem
Tillagt eller lägger till entiteter tooyour lagras entiteter vanligtvis resulterar i hello programmet att först lägga till nya entiteter toohello eller sista partition av en sekvens av partitioner. Alla hello infogningar vid något tillfälle finns i det här fallet äger rum i hello samma partition, skapar en hotspot som förhindrar att hello tabelltjänsten belastningsutjämning infogar över flera noder och vilket kan orsaka att ditt program toohit hello skalbarhet mål för partitionen. Till exempel om du har ett program som loggar nätverks- och åtkomst av anställda, en entitet struktur som på bilden nedan kan resultera i att hello aktuell timme partition blir en hotspot om hello mängder transaktioner når hello skalbarhet mål för en enskild partition:  

![][26]

#### <a name="solution"></a>Lösning
hello förhindrar följande alternativa entitet struktur en hotspot på en viss partition som hello program loggar händelser:  

![][27]

Observera med det här exemplet hur både hello **PartitionKey** och **RowKey** är sammansatta nycklar. Hej **PartitionKey** använder både hello avdelning och medarbetare id toodistribute hello loggning på flera partitioner.  

#### <a name="issues-and-considerations"></a>Problem och överväganden
Överväg att hello följande punkter när du bestämmer hur tooimplement detta mönster:  

* Innehåller hello alternativa nyckelstruktur som undviker att skapa varm partitioner på infogningar effektivt stöd hello frågor klientprogrammet gör?  
* Innebär förväntade volymen av transaktioner du förmodligen tooreach hello skalbarhetsmål för en enskild partition och begränsas av hello lagringstjänsten?  

#### <a name="when-toouse-this-pattern"></a>När toouse det här mönstret
Undvik hello lägga/lägga till ett mönster när volymen av transaktioner är sannolikt tooresult i begränsning av hello lagringstjänsten när du använder en varm partition.  

#### <a name="related-patterns-and-guidance"></a>Vägledning och relaterade mönster
hello kan följande mönster och guider även vara relevanta när du implementerar det här mönstret:  

* [Sammansatt nyckel mönster](#compound-key-pattern)  
* [Loggen pilslut mönster](#log-tail-pattern)  
* [Ändra entiteter](#modifying-entities)  

### <a name="log-data-anti-pattern"></a>Loggen anti datamönster
Normalt bör du använda hello Blob-tjänsten i stället för hello toostore tjänstlogginformation på tabellen.  

#### <a name="context-and-problem"></a>Kontexten och problem
Ett vanligt användningsfall för loggdata är tooretrieve ett urval av loggposter för ett visst datum/tid-intervall: till exempel önskade toofind alla hello fel och kritiska meddelanden som tillämpningsprogrammet loggade mellan 15:04 och 15:06 på ett visst datum. Du inte vill toouse hello datum och tid för hello logga meddelandet toodetermine hello partition du sparar logg entiteter till: att resulterar i en varm partition eftersom samtidigt, delar alla hello loggen entiteter hello samma **PartitionKey** värdet (i avsnittet hello [Prepend/lägga till ett mönster](#prepend-append-anti-pattern)). Hello efter entitet schemat för ett loggmeddelande resulterar i en varm partition eftersom programmet hello skriver alla loggmeddelanden toohello partition för hello aktuella datum och tid:  

![][28]

I det här exemplet hello **RowKey** omfattar hello datum och tid för hello logga meddelandet tooensure loggmeddelanden lagras i tidsvärdet ordning och omfattar en meddelande-id om flera loggmeddelanden dela hello samma datum och tid.  

En annan metod är toouse en **PartitionKey** som säkerställer att programmet hello skriver meddelanden över ett antal partitioner. Om hello källan för hello loggmeddelande erbjuder ett sätt toodistribute meddelanden mellan många partitioner, kan du använda hello efter entitet schemat:  

![][29]

Hello problem med det här schemat är dock att tooretrieve alla hello loggmeddelanden för ett visst tidsintervall måste du söka varje partition i hello tabell.

#### <a name="solution"></a>Lösning
hello föregående avsnitt markerade hello problemet med försök toouse hello loggposter för tabellen service toostore och förslag på två otillräckliga, designerna. En lösning som ledde tooa varm partition med hello risk för prestandaproblem skriva loggmeddelanden; hello resulterade andra lösning i dåliga prestanda på grund av hello krav tooscan varje partition i tabellen hello tooretrieve loggmeddelanden för ett visst tidsintervall. BLOB storage erbjuder en bättre lösning för den här typen av scenario och detta är hur Azure Storage Analytics lagrar hello loggdata som samlas in.  

Det här avsnittet beskrivs hur Storage Analytics lagrar loggdata i blob storage som en illustration av den här metoden toostoring data som du vanligtvis fråga med intervallet.  

Storage Analytics lagrar meddelanden i en avgränsad format i flera blobbar. hello avgränsade format gör det enkelt för en klient programdata tooparse hello i hello loggmeddelande.  

Storage Analytics använder en namngivningskonvention för blob som gör att du toolocate hello blob (eller BLOB) som innehåller hello loggmeddelanden som du söker. Till exempel innehåller en blob med namnet ”queue/2014/07/31/1800/000001.log” loggmeddelanden som handlar om toohello Kötjänsten för hello timma med början vid 18:00 31 juli 2014. Hej ”000001” anger att detta är hello första loggfilen för den här perioden. Storage Analytics registrerar också hello tidsstämplar av hello först och logga senaste meddelanden som lagras i hello-filen som en del av hello blob-metadata. hello API för blob storage kan du hitta blobbar i en behållare baserat på ett namnprefix: toolocate alla hello blob som innehåller kön logga data för hello timma med början vid 18:00, kan du använda hello prefixet ”kö/2014/07/31/1800”.  

Storage Analytics buffrar loggmeddelanden internt och uppdaterar sedan regelbundet hello lämpliga blob eller skapar en ny hello senaste batchen med loggposter. Detta minskar antalet hello av skrivningar toohello blob-tjänsten måste utföras.  

Om du implementerar en liknande lösning i ditt eget program måste du tänka på hur toomanage hello säkerhetsaspekten tillförlitlighet (skriver varje logg post tooblob lagring när den uppstår) och kostnad och skalbarhet (buffring uppdateringar i ditt program och skrivning av dem tooblob lagring i batchar).  

#### <a name="issues-and-considerations"></a>Problem och överväganden
Överväg följande punkter när du bestämmer hur toostore logga data hello:  

* Om du skapar en tabelldesign som undviker potentiella hot partitioner kan hända att du inte kommer åt din loggdata effektivt.  
* tooprocess logga data, en klient måste ofta tooload många poster.  
* Även om loggdata är ofta strukturerad, kan blob-lagring vara en bättre lösning.  

### <a name="implementation-considerations"></a>Implementering
Det här avsnittet beskrivs några av hello överväganden toobear i åtanke när du implementerar hello mönster som beskrivs i föregående avsnitt i hello. De flesta av det här avsnittet använder exempel skrivna i C# som använder hello Storage-klientbibliotek (version 4.3.0 när hello skrivning).  

### <a name="retrieving-entities"></a>Hämta entiteter
Enligt beskrivningen i avsnittet hello [Design för frågor](#design-for-querying), hello effektivaste frågan är en punkt-fråga. Men i vissa fall kanske du måste tooretrieve flera entiteter. Det här avsnittet beskrivs några vanliga metoder tooretrieving enheter med hjälp av hello Storage-klientbiblioteket.  

#### <a name="executing-a-point-query-using-hello-storage-client-library"></a>Köra en punkt-fråga med hello Storage-klientbibliotek
hello enklaste sättet tooexecute en punkt-fråga är toouse hello **hämta** tabell åtgärden enligt följande kodavsnitt i C# hello som hämtar en entitet med en **PartitionKey** ”försäljning” och en  **RowKey** värdets ”212”:  

```csharp
TableOperation retrieveOperation = TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
var retrieveResult = employeeTable.Execute(retrieveOperation);
if (retrieveResult.Result != null)
{
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
}  
```

Observera hur det här exemplet förväntar hello entitet hämtar toobe av typen **EmployeeEntity**.  

#### <a name="retrieving-multiple-entities-using-linq"></a>Hämtning av flera enheter med hjälp av LINQ
Du kan hämta flera entiteter med hjälp av LINQ med Storage-klientbiblioteket och ange en fråga med en **där** satsen. tooavoid en tabellgenomsökning, bör du alltid skriva hello **PartitionKey** värdet i hello där-satsen, och om möjligt hello **RowKey** värdet tooavoid tabell och partition genomsökningar. Hej tabelltjänsten stöder en begränsad uppsättning jämförelse operatorer (större än, större än eller lika med, mindre än, mindre än eller lika med, lika och inte lika) toouse i hello där satsen. hello följande kodfragment för C# hittar alla hello anställda vars efternamn börjar med ”B” (förutsatt att hello **RowKey** lagrar hello efternamn) i hello försäljningsavdelningen (förutsatt att hello **PartitionKey** lagrar hello avdelningsnamn):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = employeeTable.CreateQuery<EmployeeEntity>();
var query = (from employee in employeeQuery
            where employee.PartitionKey == "Sales" &&
            employee.RowKey.CompareTo("B") >= 0 &&
            employee.RowKey.CompareTo("C") < 0
            select employee).AsTableQuery();
var employees = query.Execute();  
```

Observera hur hello-frågan anger både en **RowKey** och en **PartitionKey** tooensure bättre prestanda.  

hello följande kodexempel visar hur motsvarande fungerar med flytande hello-API (Mer information om flytande API: er i allmänhet finns [bästa praxis för att designa en flytande API](http://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(
    TableQuery.CombineFilters(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition(
    "PartitionKey", QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.GenerateFilterCondition(
    "RowKey", QueryComparisons.GreaterThanOrEqual, "B")
),
TableOperators.And,
TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "C")
    )
);
var employees = employeeTable.ExecuteQuery(employeeQuery);  
```

> [!NOTE]
> hello exempel omsluter flera **CombineFilters** metoder tooinclude hello tre filtervillkor.  
> 
> 

#### <a name="retrieving-large-numbers-of-entities-from-a-query"></a>Hämtar stort antal enheter från en fråga
En optimal frågan returnerar en enskild entitet baserat på en **PartitionKey** värde och ett **RowKey** värde. I vissa fall kan du dock ha en krav tooreturn många entiteter från hello samma partitions eller ens från flera partitioner.  

Alltid fullständigt bör du testa hello prestanda i dessa scenarier.  

En fråga mot hello tabelltjänsten kan returnera högst 1 000 enheter på en gång och kan utföra för högst fem sekunder. Om hello resultatet innehåller fler än 1 000 enheter, om hello frågan slutfördes inte inom fem sekunder, eller om frågan hello korsar hello partition gräns, hello tabelltjänsten returnerar en fortsättning token tooenable hello klientens programmet toorequest hälsning Nästa uppsättning enheter. Mer information om hur fortsättning tokens arbete finns [Timeout för fråga och sidbrytning](http://msdn.microsoft.com/library/azure/dd135718.aspx).  

Om du använder hello Storage-klientbibliotek hantera den automatiskt fortsättning token för dig som returnerar entiteter från hello tabelltjänsten. hello hanterar följande C# kodexempel använder hello Storage-klientbibliotek automatiskt fortsättning token om hello tabelltjänsten returnerar dem i ett svar:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

var employees = employeeTable.ExecuteQuery(employeeQuery);
foreach (var emp in employees)
{
        ...
}  
```

Följande C#-kod hello hanterar fortsättning token explicit:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

TableContinuationToken continuationToken = null;

do
{
        var employees = employeeTable.ExecuteQuerySegmented(
        employeeQuery, continuationToken);
    foreach (var emp in employees)
    {
    ...
    }
    continuationToken = employees.ContinuationToken;
} while (continuationToken != null);  
```

Du kan styra när programmet hämtar hello nästa segment av data med hjälp av fortsättning token explicit. Till exempel kan ditt klientprogram kan användare toopage via hello entiteter som lagras i en tabell, en användare bestämma inte toopage via alla hello entiteter som hämtas av hello frågan så att programmet bara använda en fortsättning token tooretrieve hello nästa segment när hello användaren hade slutförts bläddring genom alla hello entiteter i hello aktuella segmentet. Den här metoden har flera fördelar:  

* Den låter dig toolimit hello mängden data tooretrieve från hello tabelltjänsten och att du flyttar över hello nätverk.  
* Det gör du tooperform asynkrona i/o i .NET.  
* Den låter dig tooserialize hello fortsättning token toopersistent lagring så att du kan fortsätta i ett program kraschar hello-händelse.  

> [!NOTE]
> En fortsättningstoken returnerar vanligtvis ett segment som innehåller 1 000 enheter, även om det kan vara färre. Detta gäller även hello om du begränsar hello antalet poster som en fråga som returnerar med hjälp av **ta** tooreturn hello första n entiteter som matchar sökvillkoren sökning: hello tabelltjänsten kan returnera ett segment som innehåller färre än n enheter längs med en fortsättning token tooenable hello du tooretrieve återstående entiteter.  
> 
> 

hello visar följande C#-kod hur toomodify hello antal entiteter som returneras i ett segment:  

```csharp
employeeQuery.TakeCount = 50;  
```

#### <a name="server-side-projection"></a>Projektion av serversidan
En enda entitet kan ha upp too255 egenskaper och in too1 MB i storlek. När du frågar hello tabell och hämta entiteter kan du kanske inte behöver alla hello egenskaper och kan undvika att överföra data i onödan (toohelp minska latensen och kostnaden). Du kan använda serversidan tootransfer bara hello projektionsegenskaper du behöver. hello följande exempel är hämtar bara hello **e-post** egenskapen (tillsammans med **PartitionKey**, **RowKey**, **tidsstämpel**, och  **ETag**) från hello entiteter som valts av hello frågan.  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
List<string> columns = new List<string>() { "Email" };
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter).Select(columns);

var entities = employeeTable.ExecuteQuery(employeeQuery);
foreach (var e in entities)
{
        Console.WriteLine("RowKey: {0}, EmployeeEmail: {1}", e.RowKey, e.Email);
}  
```

Observera hur hello **RowKey** värdet är tillgängligt även om det inte ingick i hello lista över egenskaper tooretrieve.  

### <a name="modifying-entities"></a>Ändra entiteter
hello Storage-klientbibliotek kan toomodify entiteter som lagras i tabelltjänsten hello genom att lägga till, ta bort och uppdatera entiteter. Du kan använda EGTs toobatch flera insert-, update- och delete-åtgärder tillsammans tooreduce hello antalet sändningar krävs och förbättra hello prestandan för din lösning.  

Observera att undantag när hello Storage-klientbibliotek utför en EGT vanligtvis innehåller hello index för hello entitet som orsakade hello batch toofail. Detta är användbart när du felsöker kod som använder EGTs.  

Du bör också övervägas hur din design påverkar hur ditt klientprogram hanterar samtidighet och update-åtgärder.  

#### <a name="managing-concurrency"></a>Hantera samtidighet
Som standard hello tabelltjänsten implementerar Optimistisk samtidighet kontrollerar på hello nivå för enskilda entiteter för **infoga**, **sammanfoga**, och **ta bort** åtgärder, även om det är möjligt för en klient tooforce hello tabell service toobypass kontrollerna. Mer information om hur hello tabelltjänsten hanterar samtidighet finns [hantera samtidighet i Microsoft Azure Storage](../storage/common/storage-concurrency.md).  

#### <a name="merge-or-replace"></a>Merge eller ersätta
Hej **ersätta** metod för hello **TableOperation** klassen ersätter alltid hello fullständig entitet i hello tabelltjänsten. Om du inte använder en egenskap i hello begäran när egenskapen finns i hello lagras entitet hello begäran tar du bort att egenskapen från hello lagras entitet. Om du inte vill tooremove en egenskap uttryckligen från en lagrad entitet, måste du inkludera varje egenskap i hello-begäran.  

Du kan använda hello **sammanfoga** metod för hello **TableOperation** klassen tooreduce hello mängden data som du skickar toohello tabelltjänsten när du vill tooupdate en entitet. Hej **sammanfoga** metoden ersätter alla egenskaper i hello lagras entiteten med egenskapsvärden från hello-enhet som finns i hello begäran, men lämnar kvar alla egenskaper i hello lagras entitet som inte ingår i hello-begäran. Detta är användbart om du har stora entiteter och behöver bara tooupdate ett litet antal egenskaper i en begäran.  

> [!NOTE]
> Hej **ersätta** och **sammanfoga** metoder misslyckas om hello entiteten inte finns. Alternativt kan du använda hello **InsertOrReplace** och **InsertOrMerge** metoder som skapar en ny entitet om den inte finns.  
> 
> 

### <a name="working-with-heterogeneous-entity-types"></a>Arbeta med heterogena entitetstyper
Hej tabelltjänsten är en *schemat mindre* tabell butik som innebär att en enskild tabell kan lagra entiteter av flera typer som ger stor flexibilitet i din design. hello illustrerar följande exempel en tabell som lagrar både anställda och avdelningen enheter:  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>tidsstämpel</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Förnamn</th>
<th>Efternamn</th>
<th>Ålder</th>
<th>E-post</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Förnamn</th>
<th>Efternamn</th>
<th>Ålder</th>
<th>E-post</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>DepartmentName</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Förnamn</th>
<th>Efternamn</th>
<th>Ålder</th>
<th>E-post</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Observera att varje entitet måste dock ha **PartitionKey**, **RowKey**, och **tidsstämpel** värden, men kan ha en uppsättning egenskaper. Det finns inget tooindicate hello typ av en enhet om du inte väljer toostore informationen någonstans. Det finns två alternativ för att identifiera hello enhetstyp:  

* Lägga hello entitet typen toohello **RowKey** (eller eventuellt hello **PartitionKey**). Till exempel **EMPLOYEE_000123** eller **DEPARTMENT_SALES** som **RowKey** värden.  
* Använd en separat egenskapen toorecord hello entitetstyp enligt hello tabellen nedan.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>tidsstämpel</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Förnamn</th>
<th>Efternamn</th>
<th>Ålder</th>
<th>E-post</th>
</tr>
<tr>
<td>Medarbetare</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Förnamn</th>
<th>Efternamn</th>
<th>Ålder</th>
<th>E-post</th>
</tr>
<tr>
<td>Medarbetare</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>DepartmentName</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Avdelning</td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Förnamn</th>
<th>Efternamn</th>
<th>Ålder</th>
<th>E-post</th>
</tr>
<tr>
<td>Medarbetare</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

hello första alternativet, tillagt hello entitet typen toohello **RowKey**, är användbart om det finns en risk att två entiteter med olika typer kanske hello samma nyckelvärde. Dessutom grupperas entiteter av samma typ tillsammans i hello partition hello.  

hello tekniker som beskrivs i det här avsnittet är särskilt relevanta toohello diskussion [arvsrelationer](#inheritance-relationships) i den här guiden i hello avsnittet [modellering relationer](#modelling-relationships).  

> [!NOTE]
> Du bör överväga att använda ett versionsnummer i hello entitet värdet tooenable klienten program tooevolve POCO objekt av typen och arbeta med olika versioner.  
> 
> 

hello resten av det här avsnittet beskrivs några av hello funktioner i hello Storage-klientbiblioteket som gör det lättare att arbeta med flera typer av enheter i hello samma tabell.  

#### <a name="retrieving-heterogeneous-entity-types"></a>Hämta heterogena entitetstyper
Om du använder hello Storage-klientbiblioteket finns det tre alternativ för att arbeta med flera typer av enheter.  

Om du vet hello hello-enhet som lagras med ett visst **RowKey** och **PartitionKey** värden, sedan kan du ange hello entitetstypen när du hämtar hello entitet som visas i hello föregående två exempel som hämta entiteter av typen **EmployeeEntity**: [körs en punkt-fråga med hello Storage-klientbibliotek](#executing-a-point-query-using-the-storage-client-library) och [hämtning av flera enheter med hjälp av LINQ](#retrieving-multiple-entities-using-linq).  

hello andra alternativ är toouse hello **DynamicTableEntity** typ (en egenskapsuppsättning) i stället för en konkret POCO entitetstyp (det här alternativet kan också förbättra prestanda eftersom det finns inget behov av tooserialize och deserialisera hello entiteten för. NET typer). hello följande C#-kod potentiellt hämtar flera entiteter med olika typer från hello tabell, men returnerar alla entiteter som **DynamicTableEntity** instanser. Därefter använder hello **EntityType** toodetermine hello egenskapstypen varje entitet:  

```csharp
string filter = TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("PartitionKey",
    QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("RowKey",
                    QueryComparisons.GreaterThanOrEqual, "B"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey",
        QueryComparisons.LessThan, "F")
    )
);
TableQuery<DynamicTableEntity> entityQuery =
    new TableQuery<DynamicTableEntity>().Where(filter);
var employees = employeeTable.ExecuteQuery(entityQuery);

IEnumerable<DynamicTableEntity> entities = employeeTable.ExecuteQuery(entityQuery);
foreach (var e in entities)
{
EntityProperty entityTypeProperty;
if (e.Properties.TryGetValue("EntityType", out entityTypeProperty))
{
    if (entityTypeProperty.StringValue == "Employee")
    {
        // Use entityTypeProperty, RowKey, PartitionKey, Etag, and Timestamp
        }
    }
}  
```

Observera att tooretrieve andra egenskaper måste du använda hello **TryGetValue** metod på hello **egenskaper** -egenskapen för hello **DynamicTableEntity** klass.  

Ett tredje alternativ är toocombine med hello **DynamicTableEntity** typ och en **EntityResolver** instans. Detta gör att du tooresolve toomultiple POCO typer i hello samma fråga. I det här exemplet hello **EntityResolver** ombud använder hello **EntityType** egenskapen toodistinguish mellan hello två typer av entiteten som hello frågan returnerar. Hej **lösa** metoden använder hello **matcharen** delegera tooresolve **DynamicTableEntity** instanser för**TableEntity** instanser.  

```csharp
EntityResolver<TableEntity> resolver = (pk, rk, ts, props, etag) =>
{

        TableEntity resolvedEntity = null;
        if (props["EntityType"].StringValue == "Department")
        {
        resolvedEntity = new DepartmentEntity();
        }
        else if (props["EntityType"].StringValue == "Employee")
        {
        resolvedEntity = new EmployeeEntity();
        }
        else throw new ArgumentException("Unrecognized entity", "props");

        resolvedEntity.PartitionKey = pk;
        resolvedEntity.RowKey = rk;
        resolvedEntity.Timestamp = ts;
        resolvedEntity.ETag = etag;
        resolvedEntity.ReadEntity(props, null);
        return resolvedEntity;
};

string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<DynamicTableEntity> entityQuery =
        new TableQuery<DynamicTableEntity>().Where(filter);

var entities = employeeTable.ExecuteQuery(entityQuery, resolver);
foreach (var e in entities)
{
        if (e is DepartmentEntity)
        {
    ...
        }
        if (e is EmployeeEntity)
        {
    ...
        }
}  
```

#### <a name="modifying-heterogeneous-entity-types"></a>Ändra heterogena entitetstyper
Du behöver inte tooknow hello typ av en entitet toodelete den, och du alltid vet hello-typ för en entitet vid infogande av den. Du kan dock använda **DynamicTableEntity** skriver tooupdate en enhet utan att känna till dess typ och utan att använda en POCO-entitetsklass. hello följande kodexempel hämtas en enda entitet och kontrollerar hello **EmployeeCount** egenskapen finns innan den uppdateras.  

```csharp
TableResult result =
        employeeTable.Execute(TableOperation.Retrieve(partitionKey, rowKey));
DynamicTableEntity department = (DynamicTableEntity)result.Result;

EntityProperty countProperty;

if (!department.Properties.TryGetValue("EmployeeCount", out countProperty))
{
        throw new
        InvalidOperationException("Invalid entity, EmployeeCount property not found.");
}
countProperty.Int32Value += 1;
employeeTable.Execute(TableOperation.Merge(department));  
```

### <a name="controlling-access-with-shared-access-signatures"></a>Kontrollera åtkomst med signaturer för delad åtkomst
Du kan använda delade signatur åtkomst (SAS) token tooenable klienten program toomodify (och fråga) tabellentiteter direkt utan hello måste tooauthenticate direkt med hello tabelltjänsten. Det finns vanligtvis tre huvudsakliga fördelar toousing SAS i ditt program:  

* Du behöver inte toodistribute lagringen konto viktiga tooan osäker plattform (till exempel en mobil enhet) i ordning tooallow som enheten tooaccess och ändra entiteter i hello tabelltjänsten.  
* Avlastning vissa hello arbete webbplatsen och arbetsroller utför när du ska hantera dina enheter tooclient enheter, till exempel slutanvändarnas datorer och mobila enheter.  
* Du kan tilldela en begränsad och tid begränsad uppsättning behörigheter tooa klient (till exempel att tillåta läsåtkomst toospecific resurser).  

Läs mer om hur du använder SAS-token med hello tabelltjänsten [med delad åtkomst signaturer (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).  

Du måste dock fortfarande generera hello SAS-token som ger en klient toohello programentiteter i hello tabelltjänsten: du bör göra detta i en miljö som har säker åtkomst tooyour lagringskontonycklar. Normalt använder du en webb- eller arbetarroll rollen toogenerate hello SAS-token och ge dem toohello klientprogram som behöver åtkomst till tooyour entiteter. Eftersom det finns fortfarande en kostnader som genererar och leverera SAS-token tooclients, bör du överväga hur bästa tooreduce denna kostnader, särskilt i stora mängder.  

Det är möjligt toogenerate en SAS-token som beviljar åtkomst till tooa delmängd av hello entiteter i en tabell. Som standard kan du skapa en SAS-token för en hel tabell, men det är också möjligt toospecify som hello SAS-token bevilja åtkomst tooeither en uppsättning **PartitionKey** värden eller en uppsättning **PartitionKey** och  **RowKey** värden. Du kan välja toogenerate SAS-token för enskilda användare av systemet så att varje användare SAS-token kan dem bara åtkomst tootheir egna entiteter i hello tabelltjänst.  

### <a name="asynchronous-and-parallel-operations"></a>Asynkron och parallella åtgärder
Om dina begäranden sprids över flera partitioner, kan du förbättra genomflöde och klienten svarstider med hjälp av asynkron eller parallella frågor.
Du kan till exempel ha två eller flera worker rollinstanser åtkomst till dina tabeller parallellt. Du kan har enskilda arbetsroller ansvarar för viss uppsättningar av partitioner eller helt enkelt ha flera instanser av worker-rollen, varje kan tooaccess alla hello partitioner i en tabell.  

Inom en klientinstans, kan du förbättra genomflöde genom att köra lagringsåtgärder asynkront. hello Storage-klientbibliotek gör det enkelt toowrite asynkrona frågor och ändringar. Du kan till exempel starta med hello synkron metod som hämtar alla hello entiteter i en partition som visas i hello följande C#-kod:  

```csharp
private static void ManyEntitiesQuery(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

        TableContinuationToken continuationToken = null;

        do
        {
        var employees = employeeTable.ExecuteQuerySegmented(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
    {
        ...
    }
        continuationToken = employees.ContinuationToken;
        } while (continuationToken != null);
}  
```

Du kan enkelt ändra den här koden så hello frågan körs asynkront på följande sätt:  

```csharp
private static async Task ManyEntitiesQueryAsync(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);
        TableContinuationToken continuationToken = null;

        do
        {
        var employees = await employeeTable.ExecuteQuerySegmentedAsync(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
        {
            ...
        }
        continuationToken = employees.ContinuationToken;
            } while (continuationToken != null);
}  
```

I det här asynkron exemplet ser du hello följande ändringar från hello synkron version:  

* hello Metodsignaturen innehåller nu hello **asynkrona** modifierare och returnerar ett **aktivitet** instans.  
* I stället för att anropa hello **ExecuteSegmented** metoden tooretrieve resultat, hello metoden nu anrop hello **ExecuteSegmentedAsync** metod och använder hello **await** modifierare tooretrieve resulterar asynkront.  

hello klientprogrammet kan anropa metoden flera gånger (med olika värden för hello **avdelning** parametern), och varje fråga som körs på en separat tråd.  

Observera att det finns inga asynkrona versionen av hello **kör** metod i hello **TableQuery** klassen eftersom hello **IEnumerable** gränssnittet stöder inte asynkron uppräkning.  

Du kan infoga, uppdatera och ta bort entiteter asynkront. hello följande C#-exempel visar en enkel, synkron metod tooinsert eller ersätta en medarbetare entitet:  

```csharp
private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = employeeTable
        .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Du kan enkelt ändra den här koden så att hello uppdatering körs asynkront på följande sätt:  

```csharp
private static async Task SimpleEmployeeUpsertAsync(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = await employeeTable
        .ExecuteAsync(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

I det här asynkron exemplet ser du hello följande ändringar från hello synkron version:  

* hello Metodsignaturen innehåller nu hello **asynkrona** modifierare och returnerar ett **aktivitet** instans.  
* I stället för att anropa hello **kör** metoden tooupdate hello entiteten, hello metoden nu anrop hello **ExecuteAsync** metod och använder hello **await** modifieraren tooretrieve resulterar asynkront.  

hello klientprogrammet kan anropa flera asynkrona metoder som det här och varje metodanropet körs på en separat tråd.  

### <a name="credits"></a>Krediter
Vi vill gärna toothank hello följande medlemmar i hello Azure-teamet för deras bidrag: Dominic Betts, Jason Hogg, Jean Ghanem, Jai Haridas, Jeff Irwin, Vamshidhar Kommineni, Vinay Shah och Serdar Ozler samt Tom Hollander från Microsoft DX. 

Vi vill också toothank hello följande Microsoft MVP för deras feedback om värdefulla vid granskning tillfällen: Igor Papirov och Edward Bakker.

[1]: ./media/storage-table-design-guide/storage-table-design-IMAGE01.png
[2]: ./media/storage-table-design-guide/storage-table-design-IMAGE02.png
[3]: ./media/storage-table-design-guide/storage-table-design-IMAGE03.png
[4]: ./media/storage-table-design-guide/storage-table-design-IMAGE04.png
[5]: ./media/storage-table-design-guide/storage-table-design-IMAGE05.png
[6]: ./media/storage-table-design-guide/storage-table-design-IMAGE06.png
[7]: ./media/storage-table-design-guide/storage-table-design-IMAGE07.png
[8]: ./media/storage-table-design-guide/storage-table-design-IMAGE08.png
[9]: ./media/storage-table-design-guide/storage-table-design-IMAGE09.png
[10]: ./media/storage-table-design-guide/storage-table-design-IMAGE10.png
[11]: ./media/storage-table-design-guide/storage-table-design-IMAGE11.png
[12]: ./media/storage-table-design-guide/storage-table-design-IMAGE12.png
[13]: ./media/storage-table-design-guide/storage-table-design-IMAGE13.png
[14]: ./media/storage-table-design-guide/storage-table-design-IMAGE14.png
[15]: ./media/storage-table-design-guide/storage-table-design-IMAGE15.png
[16]: ./media/storage-table-design-guide/storage-table-design-IMAGE16.png
[17]: ./media/storage-table-design-guide/storage-table-design-IMAGE17.png
[18]: ./media/storage-table-design-guide/storage-table-design-IMAGE18.png
[19]: ./media/storage-table-design-guide/storage-table-design-IMAGE19.png
[20]: ./media/storage-table-design-guide/storage-table-design-IMAGE20.png
[21]: ./media/storage-table-design-guide/storage-table-design-IMAGE21.png
[22]: ./media/storage-table-design-guide/storage-table-design-IMAGE22.png
[23]: ./media/storage-table-design-guide/storage-table-design-IMAGE23.png
[24]: ./media/storage-table-design-guide/storage-table-design-IMAGE24.png
[25]: ./media/storage-table-design-guide/storage-table-design-IMAGE25.png
[26]: ./media/storage-table-design-guide/storage-table-design-IMAGE26.png
[27]: ./media/storage-table-design-guide/storage-table-design-IMAGE27.png
[28]: ./media/storage-table-design-guide/storage-table-design-IMAGE28.png
[29]: ./media/storage-table-design-guide/storage-table-design-IMAGE29.png

