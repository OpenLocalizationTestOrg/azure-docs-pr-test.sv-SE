---
title: "aaaSQL datalagret kapacitetsbegränsningar | Microsoft Docs"
description: "Högsta tillåtna värden för anslutningar, databaser, tabeller och frågor för SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: e1eac122-baee-4200-a2ed-f38bfa0f67ce
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: reference
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 8619cb997f0955d649d447cb8ca15cd742cc70b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-capacity-limits"></a>SQL Data Warehouse kapacitetsbegränsningar
hello innehåller följande tabeller hello högsta värden som tillåts för olika komponenter i Azure SQL Data Warehouse.

## <a name="workload-management"></a>Arbetsbelastningshantering
| Kategori | Beskrivning | Maximalt |
|:--- |:--- |:--- |
| [Informationslagerenheter (DWU)][Data Warehouse Units (DWU)] |Max DWU för en enskild SQL Data Warehouse |6000 |
| [Informationslagerenheter (DWU)][Data Warehouse Units (DWU)] |Max DWU för en SQL-server |6000 som standard<br/><br/> Varje SQLServer (t.ex. myserver.database.windows.net) har en DTU-kvot på 45,000 som gör att too6000 DWU som standard. Kvoten är helt enkelt en säkerhetsgräns. Du kan öka din kvot genom [skapat ett supportärende] [ creating a support ticket] och välja *kvot* som hello typ av begäran.  toocalculate din DTU måste, multiplicera hello 7.5 av hello totalt DWU behövs. Du kan visa din aktuella DTU-förbrukning från hello SQL server-bladet i hello-portalen. Både pausas och icke pausas räknas in hello DTU-kvot. |
| Databasanslutning |Öppna samtidiga sessioner |1024<br/><br/>Vi stöder högst 1024 aktiva anslutningar som kan skicka begäranden tooa SQL Data Warehouse-databas på hello samtidigt. Observera att det finns begränsningar för hello antalet frågor som faktiskt kan köras samtidigt. När hello samtidighet gränsen har överskridits hello förfrågan som skickas till en intern kö där det väntar toobe bearbetas. |
| Databasanslutning |Största minnesstorlek för förberedda instruktioner |20 MB |
| [Hantering av arbetsbelastning][Workload management] |Maximalt antal samtidiga frågor |32<br/><br/> Som standard kan SQL Data Warehouse köra maximalt 32 samtidiga frågor och köer återstående frågor.<br/><br/>hello samtidighet nivå kan minska när användare har tilldelats tooa högre resursklassen eller SQL Data Warehouse är konfigurerad med låg DWU. Några frågor som DMV frågor tillåts alltid toorun. |
| [TempDB][Tempdb] |Maxstorlek på Tempdb |399 GB per DW100. Därför är storlek too3.99 TB på DWU1000 Tempdb |

## <a name="database-objects"></a>objekt i databasen
| Kategori | Beskrivning | Maximalt |
|:--- |:--- |:--- |
| Databas |Maxstorlek |240 TB komprimerat på disk<br/><br/>Här är oberoende av utrymme i tempdb- eller loggfil och därför utrymmet är dedikerade toopermanent tabeller.  Det grupperade columnstore-komprimering uppskattas till 5 X.  Den här komprimeringen kan hello databasen toogrow tooapproximately 1 PB när alla tabeller som är grupperade columnstore (tabelltyp hello standard). |
| Tabell |Maxstorlek |60 TB komprimerat på disk |
| Tabell |Tabeller per databas |2 miljarder |
| Tabell |Kolumner per tabell |1 024 kolumner |
| Tabell |Byte per kolumn |Beroende på kolumnen [datatyp][data type].  Gränsen är 8000 för datatyperna char, 4000 för nvarchar eller 2 GB för MAX-datatyper. |
| Tabell |Byte per rad, definierad storlek |8 060 byte<br/><br/>hello antalet byte per rad beräknas i hello samma sätt som den är för SQL Server med sidan komprimering. T.ex. SQL Server SQL Data Warehouse stöder raden spill lagring som gör att **kolumner med variabel längd** toobe pushas utanför raden. När variabel längd rader skickas utanför raden, lagras endast 24 byte rot i hello huvudsakliga post. Mer information finns i hello [raden spill Data mer än 8 KB] [ Row-Overflow Data Exceeding 8 KB] MSDN-artikel. |
| Tabell |Partitioner per tabell |15,000<br/><br/>För hög prestanda, rekommenderar vi att minimera hello antalet partitioner måste du medan fortfarande stöd för dina affärsbehov. Som hello antalet partitioner växer hello kostnader för Data Definition Language (DDL) och Data Manipulation Language (DML) operations växer och orsakar långsammare. |
| Tabell |Tecken per partition gränsvärdet. |4000 |
| Index |Icke-grupperade index per tabell. |999<br/><br/>Gäller endast toorowstore tabeller. |
| Index |Grupperade index per tabell. |1<br><br/>Gäller tooboth rowstore och columnstore-tabeller. |
| Index |Indexnyckelstorleken. |900 byte.<br/><br/>Gäller endast toorowstore index.<br/><br/>Index för varchar kolumner med en maximal storlek på mer än 900 byte kan skapas om hello befintliga data i hello kolumner inte överskrida 900 byte när hello index skapas. Dock senare infoga eller uppdatera åtgärder på hello kolumner som orsakar hello total storlek tooexceed 900 byte misslyckas. |
| Index |Nyckelkolumner per index. |16<br/><br/>Gäller endast toorowstore index. Grupperade columnstore-index omfattar alla kolumner. |
| Statistik |Storleken på hello kombineras kolumnvärdena. |900 byte. |
| Statistik |Kolumner per statistik objekt. |32 |
| Statistik |Statistik skapas för kolumner per tabell. |30,000 |
| Lagrade procedurer |Maximal kapslingsnivåer. |8 |
| Visa |Kolumner per vy |1,024 |

## <a name="loads"></a>Belastningar
| Kategori | Beskrivning | Maximalt |
|:--- |:--- |:--- |
| Polybase belastningar |MB per rad |1<br/><br/>Polybase belastningar är begränsad tooloading rader båda mindre än 1MB och det går inte att läsa in tooVARCHR(MAX), NVARCHAR(MAX) eller VARBINARY(MAX).<br/><br/> |

## <a name="queries"></a>Frågor
| Kategori | Beskrivning | Maximalt |
|:--- |:--- |:--- |
| Fråga |Köade frågor om användartabeller. |1000 |
| Fråga |Samtidiga frågor om systemvyer. |100 |
| Fråga |Köade frågor för systemvyer |1000 |
| Fråga |Maximal parametrar |2098 |
| Batch |Maximal storlek |65,536*4096 |
| Välj resultat |Kolumner per rad |4096<br/><br/>Du kan aldrig ha fler än 4096 kolumner per rad i hello Välj resultat. Det är inte säkert att du alltid har 4096. Om hello frågeplan kräver en tillfällig tabell, kan hello 1 024 kolumner per tabell maximala tillämpas. |
| VÄLJ |Kapslade underfrågor |32<br/><br/>Du kan aldrig ha fler än 32 kapslade underfrågor i en SELECT-instruktion. Det är inte säkert att du alltid har 32. Exempelvis kan en koppling införa en underfråga i hello frågeplanen. hello antalet underfrågor kan också begränsas av tillgängligt minne. |
| VÄLJ |Kolumner per koppling |1 024 kolumner<br/><br/>Du kan aldrig ha fler än 1024 kolumner i hello koppling. Det är inte säkert att du alltid har 1024. Om hello koppling plan kräver en tillfällig tabell med fler kolumner än hello kopplingsresultatet, gäller hello 1024 gränsen toohello tillfällig tabell. |
| VÄLJ |Antal byte per GROUP BY-kolumner. |8060<br/><br/>hello kolumner i hello GROUP BY-sats kan ha högst 8 060 byte. |
| VÄLJ |Byte per ORDER BY kolumner |8 060 byte.<br/><br/>hello kolumner i hello ORDER BY-sats kan ha högst 8 060 byte. |
| Identifierare och konstanter per instruktionen |Antalet refererade identifierare och konstanter. |65,535<br/><br/>SQL Data Warehouse begränsar hello antal identifierare och konstanter som kan finnas i ett enda uttryck i en fråga. Den här gränsen är 65 535. Överstiger det här antalet resultat i SQL Server-fel 8632. Mer information finns i [internt fel: en uttryckstjänstgräns har nåtts][Internal error: An expression services limit has been reached]. |

## <a name="metadata"></a>Metadata
| Visa system | Max antal rader |
|:--- |:--- |
| sys.dm_pdw_component_health_alerts |10 000 |
| sys.dm_pdw_dms_cores |100 |
| sys.dm_pdw_dms_workers |Totalt antal DMS arbetare för hello senaste 1000 SQL-begäranden. |
| sys.dm_pdw_errors |10 000 |
| sys.dm_pdw_exec_requests |10 000 |
| sys.dm_pdw_exec_sessions |10 000 |
| sys.dm_pdw_request_steps |Totalt antal steg för hello senaste 1000 SQL-begäranden som lagras i sys.dm_pdw_exec_requests. |
| sys.dm_pdw_os_event_logs |10 000 |
| sys.dm_pdw_sql_requests |hello senaste 1000 SQL begäranden som lagras i sys.dm_pdw_exec_requests. |

## <a name="next-steps"></a>Nästa steg
Läs mer till referens [översikt över SQL Data Warehouse-referens][SQL Data Warehouse reference overview].

<!--Image references-->

<!--Article references-->
[Data Warehouse Units (DWU)]: ./sql-data-warehouse-overview-what-is.md
[SQL Data Warehouse reference overview]: ./sql-data-warehouse-overview-reference.md
[Workload management]: ./sql-data-warehouse-develop-concurrency.md
[Tempdb]: ./sql-data-warehouse-tables-temporary.md
[data type]: ./sql-data-warehouse-tables-data-types.md
[creating a support ticket]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Row-Overflow Data Exceeding 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[Internal error: An expression services limit has been reached]: https://support.microsoft.com/kb/913050
