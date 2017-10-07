---
title: "Strömma Analytics utdata: alternativ för lagring, analys | Microsoft Docs"
description: "Läs mer om Stream Analytics utdata Dataalternativ inklusive Power BI för analysresultat som mål."
keywords: "Dataomvandling av, analysresultat, alternativen för datalagring"
services: stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: ba6697ac-e90f-4be3-bafd-5cfcf4bd8f1f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 99f8113f0464960e898293397fbe3de90d669857
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Strömma Analytics utdata: alternativ för lagring, analys
Överväg att hur hello resulterande data kommer att användas vid redigering av ett Stream Analytics-jobb. Hur du visar hello resultatet av hello Stream Analytics-jobbet och där du lagrar den?

Azure Stream Analytics har olika alternativ för att lagra utdata och visa analysresultat i ordning tooenable en mängd olika program mönster. Detta gör det enkelt tooview jobbutdata och ger dig flexibilitet i hello användning och lagring av hello jobbutdata för datalagring och andra ändamål. Inga utdata som konfigurerats i hello jobbet måste finnas innan hello jobbet har startats och händelser börjar flöda. Till exempel om du använder Blob storage som utdata hello jobbet kommer inte att skapa ett lagringskonto automatiskt. Den måste toobe som skapats av hello användare innan hello ASA jobb har startat.

## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Strömma Analytics stöder [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Lagringen kan toostore data för alla storlek, typ och införandet hastighet för drifts- och undersökande analyser. Stream Analytics behov toobe behörighet dessutom tooaccess hello Data Lake Store. Information om tillstånd och hur toosign för hello Data Lake Store (vid behov) diskuteras i hello [Data Lake utdata artikel](stream-analytics-data-lake-output.md).

### <a name="authorize-an-azure-data-lake-store"></a>Godkänna ett Azure Data Lake Store
När lagring av Data Lake väljs som utdata i hello Azure-portalen, kommer du att ange tooauthorize en anslutning tooan befintliga Data Lake Store.  

![Auktorisera Data Lake Store](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Fyll i hello egenskaper för hello Data Lake Store utdata som visas nedan:

![Auktorisera Data Lake Store](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

hello tabellen nedan visar hello egenskapsnamn och deras beskrivning som behövs för att skapa ett Data Lake Store-utdata.

<table>
<tbody>
<tr>
<td><B>EGENSKAPSNAMN</B></td>
<td><B>BESKRIVNING</B></td>
</tr>
<tr>
<td>Kolumnalias</td>
<td>Detta är ett eget namn som används i frågor toodirect hello frågan utdata toothis Data Lake Store.</td>
</tr>
<tr>
<td>Kontonamn</td>
<td>hello namnet på hello Data Lake-standardlagringskontot där du skickar din utdata. Du kommer att visas en listruta med datasjölagerkonton toowhich hello användaren loggat in toohello portal har åtkomst till.</td>
</tr>
<tr>
<td>Prefixet sökvägar [<I>valfria</I>]</td>
<td>Hej filen sökvägen toowrite filerna i hello angivna Data Lake Store-konto. <BR>{date} {time}<BR>Exempel 1: mapp1/logs / {date} / {time}<BR>Exempel 2: mapp1/logs / {date}</td>
</tr>
<tr>
<td>Datumformat [<I>valfria</I>]</td>
<td>Du kan välja hello datumformat där filerna ordnas om hello datumtoken används i hello prefix sökväg. Exempel: ÅÅÅÅ/MM/DD</td>
</tr>
<tr>
<td>Tidsformat [<I>valfria</I>]</td>
<td>Ange hello tidsformat där filerna ordnas om hello tid token används i hello prefix sökväg. Hello stöds endast värdet är för närvarande HH.</td>
</tr>
<tr>
<td>Händelsen serialiseringsformat</td>
<td>Serialiseringsformat för utdata. JSON-, CSV- och Avro stöds.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Om CSV- eller JSON-format, måste kodning anges. UTF-8 är hello stöds endast kodningsformat just nu.</td>
</tr>
<tr>
<td>Avgränsare</td>
<td>Gäller endast för CSV-serialisering. Stream Analytics stöder ett antal olika avgränsare för serialisering CSV. Värden som stöds är kommatecken, semikolon, blanksteg, TABB och vertikalstreck.</td>
</tr>
<tr>
<td>Format</td>
<td>Gäller endast för JSON-serialisering. Radseparering innebär att hello utdata formateras genom att varje JSON-objekt avgränsas med en ny rad. Matrisen anger hello utdata formateras som en matris av JSON-objekt.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Förnya auktorisering för Data Lake Store
Du behöver toore-autentisera ditt Data Lake Store-konto om lösenordet har ändrats sedan jobbet skapades eller senast autentiserad.

![Auktorisera Data Lake Store](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL Database
[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) kan användas som utdata för data som är relationell natur och för program som är beroende av innehållet finns i en relationsdatabas. Stream Analytics-jobb att skriva tooan befintlig tabell med en Azure SQL Database.  Observera att hello tabellschemat måste exakt matcha hello fält och deras typer som utdata från ditt jobb. En [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) kan också anges som utdata via hello SQL Database output-alternativet samt (detta är en förhandsgranskningsfunktion). hello tabellen nedan visar hello egenskapsnamn och deras beskrivning för att skapa en SQL Database-utdata.

| Egenskapsnamn | Beskrivning |
| --- | --- |
| Kolumnalias |Detta är ett eget namn som används i frågor toodirect hello fråga utdata toothis databas. |
| Databas |hello namnet på hello databasen där du skickar din utdata |
| Servernamn |hello SQL Database-servernamn |
| Användarnamn |hello användarnamn som har åtkomst toowrite toohello databas |
| Lösenord |hello lösenord tooconnect toohello databas |
| Tabell |hello tabellnamn där hello utdata ska skrivas. hello tabellnamnet är skiftlägeskänsliga och hello schemat för den här tabellen ska stämma överens exakt toohello antalet fält och deras typer som skapas av jobbutdata. |

> [!NOTE]
> För närvarande stöds hello Azure SQL Database-erbjudande för ett jobbutdata i Stream Analytics. En Azure-dator som kör SQL Server med en databas som är ansluten stöds dock inte. Detta är ämne toochange i framtida versioner.
> 
> 

## <a name="blob-storage"></a>Blob Storage
BLOB storage erbjuder en kostnadseffektiv och skalbar lösning för att lagra stora mängder Ostrukturerade data i hello molnet.  En introduktion på Azure Blob storage och användning finns i hello dokumentationen på [hur toouse Blobbar](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

hello tabellen nedan visar hello egenskapsnamn och deras beskrivning för att skapa en blob-utdata.

<table>
<tbody>
<tr>
<td>EGENSKAPSNAMN</td>
<td>BESKRIVNING</td>
</tr>
<tr>
<td>Kolumnalias</td>
<td>Detta är ett eget namn som används i frågor toodirect hello frågan utdata toothis blob storage.</td>
</tr>
<tr>
<td>Lagringskonto</td>
<td>hello namnet på hello storage-konto där du skickar din utdata.</td>
</tr>
<tr>
<td>Lagringskontonyckel</td>
<td>hello hemlig nyckel kopplad till hello storage-konto.</td>
</tr>
<tr>
<td>Lagringsbehållaren</td>
<td>Behållare innehåller en logisk gruppering för blobbar som lagras i hello Microsoft Azure Blob-tjänsten. När du överför en blob-toohello Blob-tjänsten måste du ange en behållare för blobben.</td>
</tr>
<tr>
<td>Prefixet sökvägar [valfritt]</td>
<td>hello sökväg används toowrite dina blobbar i hello angivna behållaren.<BR>Du kan välja en eller flera instanser av följande 2 variabler toospecify hello frekvens som BLOB skrivs hello toouse inom hello-sökväg:<BR>{date} {time}<BR>Exempel 1: cluster1/logs / {date} / {time}<BR>Exempel 2: cluster1/logs / {date}</td>
</tr>
<tr>
<td>[Valfritt] datumformat</td>
<td>Du kan välja hello datumformat där filerna ordnas om hello datumtoken används i hello prefix sökväg. Exempel: ÅÅÅÅ/MM/DD</td>
</tr>
<tr>
<td>[Valfritt] tidsformat</td>
<td>Ange hello tidsformat där filerna ordnas om hello tid token används i hello prefix sökväg. Hello stöds endast värdet är för närvarande HH.</td>
</tr>
<tr>
<td>Händelsen serialiseringsformat</td>
<td>Serialiseringsformat för utdata.  JSON-, CSV- och Avro stöds.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Om CSV- eller JSON-format, måste kodning anges. UTF-8 är hello stöds endast kodningsformat just nu.</td>
</tr>
<tr>
<td>Avgränsare</td>
<td>Gäller endast för CSV-serialisering. Stream Analytics stöder ett antal olika avgränsare för serialisering CSV. Värden som stöds är kommatecken, semikolon, blanksteg, TABB och vertikalstreck.</td>
</tr>
<tr>
<td>Format</td>
<td>Gäller endast för JSON-serialisering. Radseparering innebär att hello utdata formateras genom att varje JSON-objekt avgränsas med en ny rad. Matrisen anger hello utdata formateras som en matris av JSON-objekt. Denna matris stängs endast när hello jobbet slutar Stream Analytics har flytta eller i toohello nästa gång fönster. I allmänhet är det bättre toouse rad avskilda JSON, eftersom den inte kräver någon särskild hantering medan hello utdatafilen fortfarande skrivs till.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Händelsehubb
[Händelsehubbar](https://azure.microsoft.com/services/event-hubs/) är en mycket skalbar publicera och prenumerera händelseinmatare. Det kan samla in miljontals händelser per sekund.  Användningen av en Händelsehubb som utdata är när hello utdata för ett Stream Analytics-jobb kommer att hello indata för en annan direktuppspelningsjobbet.

Det finns några parametrar som är nödvändiga tooconfigure Event Hub-dataströmmar som utdata.

| Egenskapsnamn | Beskrivning |
| --- | --- |
| Kolumnalias |Detta är ett eget namn som används i frågor toodirect hello frågan utdata toothis Event Hub. |
| Service Bus Namespace |En Service Bus-namnrymd är en behållare för en uppsättning meddelandeentiteter. När du har skapat en ny Händelsehubb skapade du även en Service Bus-namnrymd |
| Händelsehubb |hello namnet på din Event Hub-utdata |
| Namnet på Händelsehubben princip |Hej princip för delad åtkomst, som kan skapas på hello Event Hub konfigurera fliken. Varje princip för delad åtkomst ska ha ett namn, behörigheter som du ställer in och åtkomstnycklar |
| Event Hub principnyckel |hello delade åtkomstnyckeln används tooauthenticate åtkomst toohello Service Bus-namnrymd |
| Partitionen nyckelkolumn [valfritt] |Kolumnen innehåller hello partitionsnyckel för Event Hub-utdata. |
| Händelsen serialiseringsformat |Serialiseringsformat för utdata.  JSON-, CSV- och Avro stöds. |
| Encoding |För CSV- och JSON är UTF-8 hello stöds endast kodningsformat just nu |
| Avgränsare |Gäller endast för CSV-serialisering. Stream Analytics stöder ett antal olika avgränsare för serialisering av data i CSV-format. Värden som stöds är kommatecken, semikolon, blanksteg, TABB och vertikalstreck. |
| Format |Gäller endast för JSON-typen. Radseparering innebär att hello utdata formateras genom att varje JSON-objekt avgränsas med en ny rad. Matrisen anger hello utdata formateras som en matris av JSON-objekt. |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) kan användas som utdata för ett Stream Analytics-jobbet tooprovide för omfattande visualisering av analysresultat. Den här funktionen kan användas för kontrollpaneler, rapportgenerering och mått som drivs reporting.

### <a name="authorize-a-power-bi-account"></a>Auktorisera en Power BI-konto
1. När Power BI väljs som utdata i hello Azure-portalen, kommer du att ange tooauthorize en befintlig Power BI-användare eller toocreate ett nytt Power BI-konto.  
   
   ![Auktorisera Power BI-användare](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  
2. Skapa ett nytt konto om du inte ännu har en och sedan klicka på Verifiera nu.  En skärm som liknar följande hello visas.  
   
   ![Azure-konto Powerbi](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  
3. I det här steget ange hello arbets- eller skolkonto för att auktorisera hello Power BI-utdata. Om du inte är redan registrerat dig för Power BI, väljer du logga nu. hello kan arbets- eller skolkonto konto som du använder för Power BI skilja sig från hello Azure-prenumeration-konto som du är inloggad på för tillfället.

### <a name="configure-hello-power-bi-output-properties"></a>Konfigurera egenskaper för hello Power BI-utdata
När du har autentiserats hello Power BI-konto kan konfigurera du hello egenskaper för din Power BI-utdata. hello tabellen nedan är hello lista med egenskapsnamn och deras beskrivning tooconfigure Power BI-utdata.

| Egenskapsnamn | Beskrivning |
| --- | --- |
| Kolumnalias |Detta är ett eget namn som används i frågor toodirect hello frågan utdata toothis PowerBI-utdata. |
| Grupp-arbetsytan |tooenable dela data med andra Power BI-användare kan du välja grupper i Power BI-konto eller Välj ”Min arbetsyta” om du inte vill toowrite tooa grupp.  Uppdatering av en befintlig grupp kräver förnya hello Power BI-autentisering. |
| DataSet-namnet |Ange ett namn för dataset som att den är önskvärd för hello Power BI utdata toouse |
| Tabellnamnet |Ange ett tabellnamn under hello dataset av hello Power BI-utdata. För närvarande kan Power BI-utdata från Stream Analytics-jobb bara ha en tabell i en datamängd |

En genomgång över hur du konfigurerar en Power BI-utdata och instrumentpanelen finns hello [Azure Stream Analytics & Power BI](stream-analytics-power-bi-dashboard.md) artikel.

> [!NOTE]
> Inte uttryckligen skapa hello dataset och tabellen i hello Power BI-instrumentpanelen. hello fylls dataset och tabell automatiskt när hello jobb har startat och hello jobbet startar pumpande utdata till Power BI. Observera att om hello jobbfråga genereras inte några resultat, hello dataset och kommer inte att skapa tabellen. Tänk också att om Power BI redan har en datauppsättning och en tabell med hello samma namn som hello en anges i det här Stream Analytics-jobbet, hello befintliga data skrivs över.
> 
> 

### <a name="schema-creation"></a>Schemat skapas
Azure Stream Analytics skapas en Power BI dataset och tabell för hello användares räkning om det inte redan finns. I annat fall uppdateras hello-tabellen med nya värden. Det finns för närvarande en hello begränsning att endast en tabell kan finnas inom en datamängd.

### <a name="data-type-conversion-from-asa-toopower-bi"></a>Datatypskonverteringen från ASA tooPower BI
Azure Stream Analytics uppdaterar hello datamodellen dynamiskt vid körning om hello utdata schemaändringar. Kolumnen namnändringar, kolumnen ändras, och hello tillägg eller borttagning av kolumner som spåras.

Den här tabellen innehåller hello konverteringar från [Stream Analytics-datatyper](https://msdn.microsoft.com/library/azure/dn835065.aspx) tooPower BIs [Entity Data Model (EDM) typer](https://powerbi.microsoft.com/documentation/powerbi-developer-walkthrough-push-data/) om en POWER BI dataset och tabellen inte finns.


Från Stream Analytics | tooPower BI
-----|-----|------------
bigint | Int64
nvarchar(max) | Sträng
Datum och tid | Datum och tid
flyttal | dubbla
Posten matris | Sträng typ, konstantvärde ”IRecord” eller ”IArray”

### <a name="schema-update"></a>Uppdatera schemat
Stream Analytics härleder modellschemat hello data baserat på hello första uppsättningen av händelser i hello utdata. Om det behövs senare är uppdaterade tooaccommodate inkommande händelser som inte ryms i hello ursprungliga schemat hello data modellschemat.

Hej `SELECT *` frågan bör undvikas tooprevent dynamiska schemauppdatering över rader. Dessutom toopotential konsekvenser för prestanda, den kan också leda indeterminacy hello tid för hello resultat. Du måste välja exakt Hello-fält som behöver toobe visas på Power BI-instrumentpanel. Dessutom ska hello datavärden vara kompatibla med hello valt datatyp.


Föregående/Current | Int64 | Sträng | Datum och tid | dubbla
-----------------|-------|--------|----------|-------
Int64 | Int64 | Sträng | Sträng | dubbla
dubbla | dubbla | Sträng | Sträng | dubbla
Sträng | Sträng | Sträng | Sträng |  | Sträng | 
Datum och tid | Sträng | Sträng |  Datum och tid | Sträng


### <a name="renew-power-bi-authorization"></a>Förnya Power BI-auktorisering
Du behöver toore-autentisera Power BI-konto om lösenordet har ändrats sedan jobbet skapades eller senast autentiserad. Om Multi-Factor Authentication (MFA) är konfigurerat i Azure Active Directory (AAD)-klient måste du också toorenew Power BI-auktorisering varannan vecka. Ett symtom på det här problemet är inga jobbutdata och en ”autentisera användaren error” i hello Åtgärdsloggar:

  ![Power BI uppdatera fel](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

tooresolve det här problemet stoppa körs jobbet och gå tooyour Power BI-utdata.  Klicka på hello ”förnya auktorisering” länk och starta om jobbet från hello stoppats senast tooavoid data går förlorade.

  ![Powerbi förnya auktorisering](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Table Storage
[Azure Table storage](../storage/common/storage-introduction.md) ger hög tillgänglighet mycket skalbar lagring, så att ett program kan anpassas automatiskt toomeet användarens behov. Table storage är Microsofts NoSQL nyckel-och attributdatabas vilken kan utnyttja för strukturerade data med mindre begränsningar på hello schema. Azure Table storage kan vara används toostore data för beständiga och effektiv hämtning.

hello tabellen nedan visar hello egenskapsnamn och deras beskrivning för att skapa en tabellutdata.

| Egenskapsnamn | Beskrivning |
| --- | --- |
| Kolumnalias |Detta är ett eget namn som används i frågor toodirect hello frågan utdata toothis table storage. |
| Lagringskonto |hello namnet på hello storage-konto där du skickar din utdata. |
| Lagringskontonyckel |Hej åtkomstnyckeln som är associerade med hello storage-konto. |
| Tabellnamnet |hello namnet på hello tabell. hello tabell skapas om den inte finns. |
| Partitionsnyckeln |hello namnet på hello utdata kolumn som innehåller hello partitionsnyckel. hello Partitionsnyckeln är en unik identifierare för hello partition inom en given tabell som utgör hello första delen av en entitets primärnyckel. Det är ett strängvärde som kan vara upp too1 KB i storlek. |
| Radnyckel |hello namnet på hello utdata kolumn som innehåller hello raden nyckel. Hej radnyckel är en unik identifierare för en entitet i en given partition. Den utgör hello andra delen av en entitets primärnyckel. Hej radnyckel är ett strängvärde som kan vara upp too1 KB i storlek. |
| Batchstorlek |hello antalet poster för en batchåtgärd. Vanligtvis hello standard räcker för de flesta jobb finns toohello [tabell batchåtgärd spec](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) för mer information om hur du ändrar den här inställningen. |

## <a name="service-bus-queues"></a>Service Bus-köer
[Service Bus-köer](https://msdn.microsoft.com/library/azure/hh367516.aspx) erbjuder en First In, First Out (FIFO) meddelandet leverans tooone eller flera konkurrerande konsumenter. Meddelanden är oftast förväntade toobe emot och bearbetas av hello mottagarna i hello ordning som de har lagts till toohello kön, och varje meddelande tas emot och bearbetas av bara en meddelandekonsument.

hello tabellen nedan visar hello egenskapsnamn och deras beskrivning för att skapa en utgående kö.

| Egenskapsnamn | Beskrivning |
| --- | --- |
| Kolumnalias |Detta är ett eget namn som används i frågor toodirect hello frågan utdata toothis Service Bus-kö. |
| Service Bus Namespace |En Service Bus-namnrymd är en behållare för en uppsättning meddelandeentiteter. |
| Könamnet |hello namnet på hello Service Bus-kö. |
| Kön Principnamn |Du kan även skapa principer för delad åtkomst hello kön konfigurera på fliken när du skapar en kö. Varje princip för delad åtkomst ska ha ett namn, behörigheter som du ställer in och åtkomstnycklar. |
| Kön principnyckel |hello delade åtkomstnyckeln används tooauthenticate åtkomst toohello Service Bus-namnrymd |
| Händelsen serialiseringsformat |Serialiseringsformat för utdata.  JSON-, CSV- och Avro stöds. |
| Encoding |För CSV- och JSON är UTF-8 hello stöds endast kodningsformat just nu |
| Avgränsare |Gäller endast för CSV-serialisering. Stream Analytics stöder ett antal olika avgränsare för serialisering av data i CSV-format. Värden som stöds är kommatecken, semikolon, blanksteg, TABB och vertikalstreck. |
| Format |Gäller endast för JSON-typen. Radseparering innebär att hello utdata formateras genom att varje JSON-objekt avgränsas med en ny rad. Matrisen anger hello utdata formateras som en matris av JSON-objekt. |

## <a name="service-bus-topics"></a>Avsnitt om Service Bus
När Service Bus-köer ger en metod för kommunikation av en tooone från avsändaren tooreceiver [Service Bus-ämnen](https://msdn.microsoft.com/library/azure/hh367516.aspx) tillhandahålla en en-till-många-kommunikation.

hello tabellen nedan visar hello egenskapsnamn och deras beskrivning för att skapa en tabellutdata.

| Egenskapsnamn | Beskrivning |
| --- | --- |
| Kolumnalias |Detta är ett eget namn som används i frågor toodirect hello frågan utdata toothis Service Bus-ämne. |
| Service Bus Namespace |En Service Bus-namnrymd är en behållare för en uppsättning meddelandeentiteter. När du har skapat en ny Händelsehubb skapade du även en Service Bus-namnrymd |
| Ämnesnamn |Ämnen messaging entiteter, liknande tooevent NAV och köer. De är utformad toocollect händelseströmmar från ett antal olika enheter och tjänster. När ett ämne skapas, ges även ett specifikt namn. hälsningsmeddelande skickas tooa avsnittet inte blir tillgängliga om en prenumeration har skapats, så se till att det finns en eller flera prenumerationer under hello avsnittet |
| Avsnittet Principnamn |När du skapar ett ämne kan skapa du också principer för delad åtkomst på hello avsnittet Konfigurera fliken. Varje princip för delad åtkomst ska ha ett namn, behörigheter som du ställer in och åtkomstnycklar |
| Avsnittet principnyckel |hello delade åtkomstnyckeln används tooauthenticate åtkomst toohello Service Bus-namnrymd |
| Händelsen serialiseringsformat |Serialiseringsformat för utdata.  JSON-, CSV- och Avro stöds. |
| Encoding |Om CSV- eller JSON-format, måste kodning anges. UTF-8 är hello stöds endast kodningsformat just nu |
| Avgränsare |Gäller endast för CSV-serialisering. Stream Analytics stöder ett antal olika avgränsare för serialisering av data i CSV-format. Värden som stöds är kommatecken, semikolon, blanksteg, TABB och vertikalstreck. |

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos-DB](https://azure.microsoft.com/services/documentdb/) är en fullständigt hanterad NoSQL-dokumentdatabastjänst som erbjuder frågan och transaktioner över schemafria data, förutsägbara och tillförlitlig prestanda och snabb utveckling.

hello under listan information hello egenskapsnamn och deras beskrivning för att skapa ett Azure DB som Cosmos-utdata.

* **Utdata Alias** – ett alias toorefer detta utdata i frågan ASA  
* **Kontonamn** – hello namn eller endpoint-URI för hello Cosmos-DB-konto.  
* **Kontot nyckeln** – hello delade åtkomstnyckeln för hello Cosmos-DB-konto.  
* **Databasen** – hello Cosmos-DB-databasnamn.  
* **Samlingsnamnsmönstret** – hello samlingens namn eller deras mönster för hello samlingar toobe används. Hej samlingsnamnsformatet kan konstrueras med valfritt {partition}-token hello där partitionerna börjar från 0. Följande är exempel giltiga indata:  
  1\) MyCollection – en samling med namnet ”MyCollection” måste finnas.  
  2\) MyCollection {partition} – dessa samlingar måste finnas – ”MyCollection0”, ”MyCollection1”, ”MyCollection2” och så vidare.  
* **Partitionera nyckeln** – det är valfritt. Det här krävs bara om du använder en {partition}-token i din samlingsnamnsmönstret. hello namnet på hello fält i utdata händelser som används toospecify hello nyckel för att partionera utdata över samlingarna. Enda samling utdata för en godtycklig utdatakolumnen kan vara används t.ex. PartitionId.  
* **Dokumentera ID** – det är valfritt. hello namnet på hello-fältet i utdatahändelserna används toospecify hello primärnyckel operations baseras på vilka insert eller update.  


## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
Du har varit introducerades tooStream Analytics, en hanterad tjänst för streaming analytics på data från hello Sakernas Internet. toolearn mer om den här tjänsten finns i:

* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
