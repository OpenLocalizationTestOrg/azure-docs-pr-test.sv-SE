---
title: "aaaAll ämnen för tjänsten SQL Data Warehouse | Microsoft Docs"
description: "Tabell med alla ämnen för hello Azure-tjänsten med namnet SQL Data Warehouse som finns på http://azure.microsoft.com/documentation/articles/, rubrik och beskrivning."
services: sql-data-warehouse
documentationcenter: 
author: barbkess
manager: jhubbard
editor: 
ms.assetid: a26a6dec-9c08-4415-8f58-4ee1dd41f718
ms.service: sql-data-warehouse
ms.workload: sql-data-warehouse
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: reference
ms.date: 03/30/2017
ms.author: barbkess
ms.openlocfilehash: 6f71d35b76b50764a5904525445675dafaa56b85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Alla ämnen för Azure SQL Data Warehouse-tjänsten
Det här avsnittet listar alla avsnittet som särskilt gäller direkt toohello **SQL Data Warehouse** tjänsten Azure. Du hittar den här webbsidan för nyckelord med hjälp av **Ctrl + F**, toofind hello intresseområden aktuella.

## <a name="new"></a>Ny
| &nbsp; | Rubrik | Beskrivning |
| ---:|:--- |:--- |
| 1 |[SQL Data Warehouse-säkerhetskopieringar](sql-data-warehouse-backups.md) |Läs mer om SQL Data Warehouse inbyggd databassäkerhetskopieringar som gör att du toorestore en återställningspunkt för Azure SQL Data Warehouse tooa eller en annan geografisk region. |

## <a name="updated-articles-sql-data-warehouse"></a>Uppdaterade artiklar, SQL Data Warehouse
Det här avsnittet innehåller artiklar som nyligen har uppdaterats, där hello uppdatera är stort eller betydande. För varje uppdaterade artikel till en grov fragment av hello markdown texten visas. hello artiklar har uppdaterats i hello datumintervall för **2016-08-22** för**2016-10-05**.

| &nbsp; | Artikel | Uppdaterad text, fragment | När |
| ---:|:--- |:--- |:--- |
| 2 |[Läs in data från Azure blob storage till SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) |/ tootrack byte och välj r.command, s.request_id, r.status, count (distinkta input_name) som nbr_files, sum (s.bytes_processed) / 1024/1024 som gb_processed från sys.dm_pdw_exec_requests r inre koppling sys.dm_pdw_dms_external_work s på r.request_id-filer = s.request_id var r. etikett = ' CTAS: Läs in rederiets. DimProduct ' eller r. etikett = ' CTAS: Läs in rederiets. FactOnlineSales' GROUP BY r.command, s.request_id, r.status ORDER BY nbr_files desc, gb_processed desc; |2016-09-07 |
| 3 |[SQL Data Warehouse-återställning](sql-data-warehouse-restore-database-overview.md) |** Kan jag återställa ett pausat data warehouse? ** toorestore ett datalager som är pausad, behöver du toofirst ta den online igen. När hello-datalagret är tillbaka online har sju dagar efter återställningspunkter toochoose från. ** Återställa tooa geo-redundant region ** om du använder hello geo-redundant lagring, kan du återställa hello data warehouse tooyour parad datacenter i en annan geografisk region. hello-datalagret har återställts från hello senaste daglig säkerhetskopiering. ** Återställa tidslinjen ** kan du återställa en databas tooany återställningspunkt inom hello senaste sju dagarna. Ögonblicksbilder Starta var fjärde timme tooeight och är tillgängliga i sju dagar. När en ögonblicksbild är äldre än sju dagar, den upphör att gälla och dess återställningspunkten är inte längre tillgänglig. ** Återställa kostnader ** hello lagring kostnad för hello återställts datalagret faktureras med hello Azure Premium Storage hastighet. Om du pausar ett återställda data warehouse, debiteras du för lagring med hello Azure Premium Storage hastighet. hello utnyttja pausa är du inte är kostnad |2016-09-29 |

## <a name="get-started"></a>Kom igång
| &nbsp; | Rubrik | Beskrivning |
| ---:|:--- |:--- |
| 4 |[Autentisering tooAzure SQL Data Warehouse](sql-data-warehouse-authentication.md) |Azure Active Directory (AAD) och SQL Server-autentisering tooAzure SQL Data Warehouse. |
| 5 |[Metodtips för Azure SQL Data Warehouse](sql-data-warehouse-best-practices.md) |Rekommendationer och metodtips som du bör känna till när du utvecklar lösningar för Azure SQL Data Warehouse. De hjälper dig att lyckas! |
| 6 |[Drivrutiner för Azure SQL Data Warehouse](sql-data-warehouse-connection-strings.md) |Anslutningssträngar och drivrutiner för SQL Data Warehouse |
| 7 |[Ansluta tooAzure SQL Data Warehouse](sql-data-warehouse-connect-overview.md) |Hur toofind hello servernamn och anslutningssträng för din tooAzure SQL Data Warehouse |
| 8 |[Analysera data med Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) |Använd Azure Machine Learning toobuild förutsägande machine learning-modell baserad på data som lagras i Azure SQL Data Warehouse. |
| 9 |[Frågan Azure SQL Data Warehouse (sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) |Förfrågningar till Azure SQL Data Warehouse med hello sqlcmd Command-line Utility. |
| 10 |[Skapa en SQL Data Warehouse-databas med hjälp av Transact-SQL (TSQL)](sql-data-warehouse-get-started-create-database-tsql.md) |Lär dig hur toocreate en Azure SQL Data Warehouse med TSQL |
| 11 |[Hur toocreate en biljett för SQL Data Warehouse](sql-data-warehouse-get-started-create-support-ticket.md) |Hur biljett toocreate stöd i Azure SQL Data Warehouse. |
| 12 |[Läs in Data med Azure Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md) |Läs tooload data med Azure Data Factory |
| 13 |[Läs in data med PolyBase i SQL Data Warehouse](sql-data-warehouse-get-started-load-with-polybase.md) |Lär dig vad PolyBase är och hur toouse det för informationslagerscenarier. |
| 14 |[Skapa en Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md) |Lär dig hur hello toocreate en Azure SQL Data Warehouse i Azure-portalen |
| 15 |[Skapa SQL Data Warehouse med hjälp av PowerShell](sql-data-warehouse-get-started-provision-powershell.md) |Skapa ett SQL Data Warehouse med hjälp av PowerShell |
| 16 |[Visualisera data med Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) |Visualisera SQL Data Warehouse-data med Power BI |
| 17 |[Fråga Azure SQL Data Warehouse (Visual Studio)](sql-data-warehouse-query-visual-studio.md) |Fråga SQL Data Warehouse med Visual Studio. |

## <a name="develop"></a>Utveckla
| &nbsp; | Rubrik | Beskrivning |
| ---:|:--- |:--- |
| 18 |[Optimera transaktioner för SQL Data Warehouse](sql-data-warehouse-develop-best-practices-transactions.md) |Bästa praxis riktlinjer för att skriva effektiva transaktionsuppdateringar i Azure SQL Data Warehouse |
| 19 |[Hantering av samtidighet och arbetsbelastning i SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md) |Förstå samtidighet och arbetsbelastningen hantering i Azure SQL Data Warehouse för utveckling av lösningar. |
| 20 |[Skapa en tabell som Select (CTAS) i SQL Data Warehouse](sql-data-warehouse-develop-ctas.md) |Tips för att koda med hello Skapa tabell som select-uttryck (CTAS) i Azure SQL Data Warehouse för utveckling av lösningar. |
| 21 |[Dynamisk SQL i SQL Data Warehouse](sql-data-warehouse-develop-dynamic-sql.md) |Tips för att använda dynamisk SQL i Azure SQL Data Warehouse för utveckling av lösningar. |
| 22 |[Gruppera efter alternativ i SQL Data Warehouse](sql-data-warehouse-develop-group-by-options.md) |Tips för gruppen av alternativen i Azure SQL Data Warehouse för utveckling av lösningar. |
| 23 |[Använda etiketter tooinstrument frågor i SQL Data Warehouse](sql-data-warehouse-develop-label.md) |Tips för att använda etiketter tooinstrument frågor i Azure SQL Data Warehouse för utveckling av lösningar. |
| 24 |[Slingor i SQL Data Warehouse](sql-data-warehouse-develop-loops.md) |Tips för Transact-SQL-slingor och ersätta markörer i Azure SQL Data Warehouse för utveckling av lösningar. |
| 25 |[Lagrade procedurer i SQL Data Warehouse](sql-data-warehouse-develop-stored-procedures.md) |Tips för att använda lagrade procedurer i Azure SQL Data Warehouse för utveckling av lösningar. |
| 26 |[Transaktioner i SQL Data Warehouse](sql-data-warehouse-develop-transactions.md) |Tips för transaktioner i Azure SQL Data Warehouse för utveckling av lösningar. |
| 27 |[Användardefinierade scheman i SQL Data Warehouse](sql-data-warehouse-develop-user-defined-schemas.md) |Tips för att använda Transact-SQL-scheman i Azure SQL Data Warehouse för utveckling av lösningar. |
| 28 |[Tilldela variabler i SQL Data Warehouse](sql-data-warehouse-develop-variable-assignment.md) |Tips för att tilldela Transact-SQL-variabler i Azure SQL Data Warehouse för utveckling av lösningar. |
| 29 |[Vyer i SQL Data Warehouse](sql-data-warehouse-develop-views.md) |Tips för att använda Transact-SQL-vyer i Azure SQL Data Warehouse för utveckling av lösningar. |
| 30 |[Designbeslut och kodning tekniker för SQL Data Warehouse](sql-data-warehouse-overview-develop.md) |Begrepp för utveckling, designbeslut, rekommendationer och kodning tekniker för SQL Data Warehouse. |

## <a name="manage"></a>Hantera
| &nbsp; | Rubrik | Beskrivning |
| ---:|:--- |:--- |
| 31 |[Hantera datorkraft i Azure SQL Data Warehouse (översikt)](sql-data-warehouse-manage-compute-overview.md) |Prestanda skala ut funktioner i Azure SQL Data Warehouse. Skala ut genom att justera dwu: er eller pausa och återuppta beräkning resurser toosave kostnader. |
| 32 |[Hantera datorkraft i Azure SQL Data Warehouse (Azure portal)](sql-data-warehouse-manage-compute-portal.md) |Azure portal uppgifter toomanage beräkningskraft. Skala beräkningsresurser genom att justera dwu: er. Eller, pausa och återuppta beräkning resurser toosave kostnader. |
| 33 |[Hantera datorkraft i Azure SQL Data Warehouse (PowerShell)](sql-data-warehouse-manage-compute-powershell.md) |PowerShell uppgifter toomanage beräkningskraft. Skala beräkningsresurser genom att justera dwu: er. Eller, pausa och återuppta beräkning resurser toosave kostnader. |
| 34 |[Hantera datorkraft i Azure SQL Data Warehouse (REST)](sql-data-warehouse-manage-compute-rest-api.md) |PowerShell uppgifter toomanage beräkningskraft. Skala beräkningsresurser genom att justera dwu: er. Eller, pausa och återuppta beräkning resurser toosave kostnader. |
| 35 |[Hantera datorkraft i Azure SQL Data Warehouse (T-SQL)](sql-data-warehouse-manage-compute-tsql.md) |Transact-SQL (T-SQL) uppgifter tooscale ut prestanda genom att justera dwu: er. Spara kostnader genom att skala tillbaka under tider med låg belastning. |
| 36 |[Monitor your workload using DMVs](sql-data-warehouse-manage-monitor.md) (Övervaka arbetsbelastningen med datahanteringsvyer) |Lär dig hur toomonitor din arbetsbelastning med av DMV: er. |
| 37 |[Hantera databaser i Azure SQL Data Warehouse](sql-data-warehouse-overview-manage.md) |Översikt över hantering av SQL Data Warehouse-databaser. Innehåller hanteringsverktyg, dwu: er och skalbar prestanda, felsökning frågeprestanda, etablera bra säkerhetsprinciper och återställa en databas från skadade data eller från ett regionalt strömavbrott. |
| 38 |[Övervaka användarfrågor i Azure SQL Data Warehouse](sql-data-warehouse-overview-manage-user-queries.md) |Översikt över hello överväganden bästa praxis och uppgifter för övervakning av användarfrågor i Azure SQL Data Warehouse |
| 39 |[SQL Data Warehouse-återställning](sql-data-warehouse-restore-database-overview.md) |Översikt över hello databasen återställningsalternativ för att återställa en databas i Azure SQL Data Warehouse. |
| 40 |[Återställa en Azure SQL Data Warehouse (Portal)](sql-data-warehouse-restore-database-portal.md) |Azure portal uppgifter för att återställa en Azure SQL Data Warehouse. |
| 41 |[Återställa en Azure SQL Data Warehouse (PowerShell)](sql-data-warehouse-restore-database-powershell.md) |PowerShell-uppgifter för att återställa en Azure SQL Data Warehouse. |
| 42 |[Återställa en Azure SQL Data Warehouse (REST-API)](sql-data-warehouse-restore-database-rest-api.md) |REST API-uppgifterna för att återställa en Azure SQL Data Warehouse. |

## <a name="tables-and-indexes"></a>Tabeller och index
| &nbsp; | Rubrik | Beskrivning |
| ---:|:--- |:--- |
| 43 |[Datatyper för tabellerna i SQL Data Warehouse](sql-data-warehouse-tables-data-types.md) |Komma igång med datatyper för Azure SQL Data Warehouse-tabeller. |
| 44 |[Distribuera tabeller i SQL Data Warehouse](sql-data-warehouse-tables-distribute.md) |Komma igång med att distribuera tabeller i Azure SQL Data Warehouse. |
| 45 |[Indexering tabeller i SQL Data Warehouse](sql-data-warehouse-tables-index.md) |Komma igång med tabellen indexering i Azure SQL Data Warehouse. |
| 46 |[Översikt över tabeller i SQL Data Warehouse](sql-data-warehouse-tables-overview.md) |Komma igång med Azure SQL Data Warehouse-tabeller. |
| 47 |[Partitionering tabeller i SQL Data Warehouse](sql-data-warehouse-tables-partition.md) |Komma igång med Tabellpartitionering i Azure SQL Data Warehouse. |
| 48 |[Hantera statistik på tabellerna i SQL Data Warehouse](sql-data-warehouse-tables-statistics.md) |Komma igång med statistik på tabellerna i Azure SQL Data Warehouse. |
| 49 |[Temporära tabeller i SQL Data Warehouse](sql-data-warehouse-tables-temporary.md) |Komma igång med temporära tabeller i Azure SQL Data Warehouse. |

## <a name="integrate"></a>Integrera
| &nbsp; | Rubrik | Beskrivning |
| ---:|:--- |:--- |
| 50 |[Använda Azure Data Factory med SQL Data Warehouse](sql-data-warehouse-integrate-azure-data-factory.md) |Tips för att använda Azure Data Factory (ADM) med Azure SQL Data Warehouse för utveckling av lösningar. |
| 51 |[Använda Azure Machine Learning med SQL Data Warehouse](sql-data-warehouse-integrate-azure-machine-learning.md) |Självstudier för att använda Azure Machine Learning med Azure SQL Data Warehouse för utveckling av lösningar. |
| 52 |[Använda Azure Stream Analytics med SQL Data Warehouse](sql-data-warehouse-integrate-azure-stream-analytics.md) |Tips för att använda Azure Stream Analytics med Azure SQL Data Warehouse för utveckling av lösningar. |
| 53 |[Använda Powerbi med SQL Data Warehouse](sql-data-warehouse-integrate-power-bi.md) |Tips för att använda Power BI med Azure SQL Data Warehouse för utveckling av lösningar. |
| 54 |[Dra nytta av andra tjänster med SQL Data Warehouse](sql-data-warehouse-overview-integrate.md) |Verktyg och partners med lösningar som kan integreras med SQL Data Warehouse. |

## <a name="load"></a>Läsa in
| &nbsp; | Rubrik | Beskrivning |
| ---:|:--- |:--- |
| 55 |[Läs in data från Azure blobblagring till Azure SQL Data Warehouse (Azure Data Factory)](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) |Läs tooload data med Azure Data Factory |
| 56 |[Läs in data från Azure blob storage till SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) |Lär dig hur toouse PolyBase tooload data från Azure blob storage till SQL Data Warehouse. Läsa in några tabeller från offentliga data till hello Contoso Retail-datalagret schemat. |
| 57 |[Läs in data från SQL Server till Azure SQL Data Warehouse (AZCopy)](sql-data-warehouse-load-from-sql-server-with-azcopy.md) |Använder bcp tooexport data från SQL Server tooflat filer, AZCopy tooimport data tooAzure blobblagring och PolyBase tooingest hello data till Azure SQL Data Warehouse. |
| 58 |[Läs in data från SQL Server till Azure SQL Data Warehouse (flat-filer)](sql-data-warehouse-load-from-sql-server-with-bcp.md) |För små datastorlekar, använder du bcp tooexport data från SQL Server tooflat filer och importera hello data direkt i Azure SQL Data Warehouse. |
| 59 |[Läs in data från SQL Server till Azure SQL Data Warehouse (SSIS)](sql-data-warehouse-load-from-sql-server-with-integration-services.md) |Visar hur toocreate SQL Server Integration Services (SSIS) toomove paketdata från en mängd olika data datakällor tooSQL Data Warehouse. |
| 60 |[Läs in data med PolyBase i SQL Data Warehouse](sql-data-warehouse-load-from-sql-server-with-polybase.md) |Använder bcp tooexport data från SQL Server tooflat filer, AZCopy tooimport data tooAzure blobblagring och PolyBase tooingest hello data till Azure SQL Data Warehouse. |
| 61 |[Guide för att använda PolyBase i SQL Data Warehouse](sql-data-warehouse-load-polybase-guide.md) |Riktlinjer och rekommendationer för att använda PolyBase i SQL Data Warehouse-scenarier. |
| 62 |[läs in exempeldata i SQL Data Warehouse](sql-data-warehouse-load-sample-databases.md) |läs in exempeldata i SQL Data Warehouse |
| 63 |[Läs in data med BCP](sql-data-warehouse-load-with-bcp.md) |Lär dig vad bcp är och hur toouse det för informationslagerscenarier. |
| 64 |[Läs in data till Azure SQL Data Warehouse](sql-data-warehouse-overview-load.md) |Lär dig hello vanliga scenarier för datainläsning till SQL Data Warehouse. Dessa inkluderar med PolyBase, Azure blob storage, flat-filer och disk leverans. Du kan också använda verktyg från tredje part. |

## <a name="migrate"></a>Migrera
| &nbsp; | Rubrik | Beskrivning |
| ---:|:--- |:--- |
| 65 |[Migrera dina SQL-kod tooSQL Data Warehouse](sql-data-warehouse-migrate-code.md) |Tips för att migrera dina SQL-kod tooAzure SQL Data Warehouse för utveckling av lösningar. |
| 66 |[Migrera dina Data](sql-data-warehouse-migrate-data.md) |Tips för att migrera dina data tooAzure SQL Data Warehouse för utveckling av lösningar. |
| 67 |[Verktyg för migrering av data Warehouse (förhandsgranskning)](sql-data-warehouse-migrate-migration-utility.md) |Migrera tooSQL Data Warehouse. |
| 68 |[Migrera din schemat tooSQL Data Warehouse](sql-data-warehouse-migrate-schema.md) |Tips för att migrera din schemat tooAzure SQL Data Warehouse för utveckling av lösningar. |
| 69 |[Migrera din lösning tooSQL Data Warehouse](sql-data-warehouse-overview-migrate.md) |Riktlinjer för migrering för att skapa din lösning tooAzure SQL Data Warehouse-plattform. |

## <a name="partners"></a>Partner
| &nbsp; | Rubrik | Beskrivning |
| ---:|:--- |:--- |
| 70 |[SQL Data Warehouse business intelligence-partner](sql-data-warehouse-partner-business-intelligence.md) |Listor med tredje parts intelligence affärspartner med lösningar som har stöd för SQL Data Warehouse. |
| 71 |[SQL Data Warehouse data integration partners](sql-data-warehouse-partner-data-integration.md) |Listor med tredje parts samarbetar med lösningar för katalogintegrering som stöder Azure SQL Data Warehouse. |
| 72 |[SQL Data Warehouse data management partners](sql-data-warehouse-partner-data-management.md) |Listor med tredje parts data management samarbetar med lösningar som stöder SQL Data Warehouse. |

## <a name="reference"></a>Referens
| &nbsp; | Rubrik | Beskrivning |
| ---:|:--- |:--- |
| 73 |[Referensinformation för SQL Data Warehouse](sql-data-warehouse-overview-reference.md) |Innehåll referenslänkar för SQL Data Warehouse. |
| 74 |[PowerShell-cmdletar och REST-API: er för SQL Data Warehouse](sql-data-warehouse-reference-powershell-cmdlets.md) |Hitta hello översta PowerShell-cmdlets för Azure SQL Data Warehouse hur toopause och återuppta en databas. |
| 75 |[Språkelement](sql-data-warehouse-reference-tsql-language-elements.md) |Lista över länkar tooreference innehåll för hello Transact-SQL-språkelement används för SQL Data Warehouse. |
| 76 |[Transact-SQL-avsnitt](sql-data-warehouse-reference-tsql-statements.md) |Länkar tooreference innehåll för hello Transact-SQL-information som används av SQL Data Warehouse. |
| 77 |[Systemvyer](sql-data-warehouse-reference-tsql-system-views.md) |Länkar toosystem vyer innehåll för SQL Data Warehouse. |

## <a name="security"></a>Säkerhet
| &nbsp; | Rubrik | Beskrivning |
| ---:|:--- |:--- |
| 78 |[SQL Data Warehouse - klientversioner stöd för granskning och dynamisk Datamaskning](sql-data-warehouse-auditing-downlevel-clients.md) |Lär dig mer om SQL Data Warehouse äldre klienter stöd för granskning av data |
| 79 |[Granskning i Azure SQL Data Warehouse](sql-data-warehouse-auditing-overview.md) |Kom igång med granskning i Azure SQL Data Warehouse |
| 80 |[Kom igång med Transparent Data kryptering (TDE) i SQL Data Warehouse](sql-data-warehouse-encryption-tde.md) |Transparent datakryptering (TDE) i SQL Data Warehouse |
| 81 |[Kom igång med Transparent Data kryptering (TDE)](sql-data-warehouse-encryption-tde-tsql.md) |Transparent datakryptering (TDE) i SQL Data Warehouse (T-SQL) |
| 82 |[Skydda en databas i SQL Data Warehouse](sql-data-warehouse-overview-manage-security.md) |Tips för att skydda en databas i Azure SQL Data Warehouse för utveckling av lösningar. |

## <a name="miscellaneous"></a>Övrigt
| &nbsp; | Rubrik | Beskrivning |
| ---:|:--- |:--- |
| 83 |[Installera Visual Studio och SSDT för SQL Data Warehouse](sql-data-warehouse-install-visual-studio.md) |Installera Visual Studio och SQL Server Development Tools (SSDT) för Azure SQL Data Warehouse |
| 84 |[Migrering tooPremium lagringsinformation](sql-data-warehouse-migrate-to-premium-storage.md) |Anvisningar för att migrera en befintlig SQL Data Warehouse toopremium lagring |
| 85 |[Kom igång med hotidentifiering](sql-data-warehouse-security-threat-detection.md) |Hur tooget igång med Hotidentifiering |
| 86 |[SQL Data Warehouse kapacitetsbegränsningar](sql-data-warehouse-service-capacity-limits.md) |Högsta tillåtna värden för anslutningar, databaser, tabeller och frågor för SQL Data Warehouse. |
| 87 |[Felsöka Azure SQL Data Warehouse](sql-data-warehouse-troubleshoot.md) |Felsöka Azure SQL Data Warehouse. |

