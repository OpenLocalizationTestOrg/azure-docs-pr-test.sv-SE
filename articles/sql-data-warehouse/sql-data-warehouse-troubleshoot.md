---
title: aaaTroubleshooting Azure SQL Data Warehouse | Microsoft Docs
description: "Felsöka Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 51f1e444-9ef7-4e30-9a88-598946c45196
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/30/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 3c53a39f77057fe0bcc053a0b896555abf397515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-sql-data-warehouse"></a>Felsöka Azure SQL Data Warehouse
Det här avsnittet visar några av hello Vanliga felsökningsfrågor vi får höra från våra kunder.

## <a name="connecting"></a>Ansluta
| Problem | Lösning |
|:--- |:--- |
| Inloggningen misslyckades för användaren 'NT AUTHORITY\\ANONYMOUS LOGON ”. (Microsoft SQL Server, fel: 18456) |Det här felet uppstår när en AAD-användare försöker tooconnect toohello master-databasen, men har inte en användare i master.  toocorrect problemet antingen ange hello SQL Data Warehouse du vill tooconnect tooat anslutningstiden eller lägga till användaren hello toohello huvuddatabasen.  Se [Säkerhetsöversikt] [ Security overview] mer information. |
| hello server principal ”MittAnvändarnamn” inte är kan tooaccess hello databasen ”master” under hello aktuella säkerhetskontexten. Det går inte att öppna användarens standarddatabas. Inloggningen misslyckades. Inloggningen misslyckades för användaren 'MittAnvändarnamn'. (Microsoft SQL Server, fel: 916) |Det här felet uppstår när en AAD-användare försöker tooconnect toohello master-databasen, men har inte en användare i master.  toocorrect problemet antingen ange hello SQL Data Warehouse du vill tooconnect tooat anslutningstiden eller lägga till användaren hello toohello huvuddatabasen.  Se [Säkerhetsöversikt] [ Security overview] mer information. |
| CTAIP fel |Det här felet kan inträffa när en inloggning som har skapats på hello SQL server-huvuddatabasen, men inte i hello SQL Data Warehouse-databas.  Om du får det här felet kan ta en titt på hello [Säkerhetsöversikt] [ Security overview] artikel.  Den här artikeln förklarar hur toocreate skapar en inloggning och användaren på Huvudmålet och sedan hur toocreate en användare i hello SQL Data Warehouse-databas. |
| Blockeras av brandvägg |Azure SQL-databaser är skyddade av servern och databasen nivån brandväggar tooensure bara kända IP-adresser som har åtkomst till tooa databas. hello brandväggar är säkra som standard, vilket innebär att du uttryckligen aktivera och IP-adressen eller adressintervallet innan du kan ansluta.  tooconfigure brandväggen för åtkomst, gör hello i [konfigurera brandväggen serveråtkomst för klientens IP-Adressen] [ Configure server firewall access for your client IP] i hello [etablering instruktioner] [Provisioning instructions]. |
| Det går inte att ansluta med verktyget eller drivrutinen |SQL Data Warehouse rekommenderar att du använder [SSMS][SSMS], [SSDT för Visual Studio][SSDT for Visual Studio], eller [sqlcmd] [ sqlcmd] tooquery dina data. Mer information om drivrutiner och anslutande tooSQL datalagret finns [drivrutiner för Azure SQL Data Warehouse] [ Drivers for Azure SQL Data Warehouse] och [ansluta tooAzure SQL Data Warehouse] [ Connect tooAzure SQL Data Warehouse] artiklar. |

## <a name="tools"></a>Verktyg
| Problem | Lösning |
|:--- |:--- |
| Visual Studio object explorer saknas i AAD-användare |Detta är ett känt problem.  Som en tillfällig lösning kan visa hello användare i [sys.database_principals][sys.database_principals].  Se [autentisering tooAzure SQL Data Warehouse] [ Authentication tooAzure SQL Data Warehouse] toolearn mer om hur du använder Azure Active Directory med SQL Data Warehouse. |
|Manuell scripting, guiden hello skript eller ansluter via SSMS är långsam, låsta eller berörda fel| Kontrollera att användarna har skapats i hello master-databasen. I skriptalternativen, också se till att hello motorn edition har angetts som ”Microsoft Azure SQL Data Warehouse Edition” och motor är ”Microsoft Azure SQL Database”.|

## <a name="performance"></a>Prestanda
| Problem | Lösning |
|:--- |:--- |
| Felsökning av frågan prestanda |Om du försöker tootroubleshoot en viss fråga startar med [lära sig hur toomonitor dina frågor][Learning how toomonitor your queries]. |
| Dåliga prestanda och planer är ofta ett resultat av saknas statistik |hello beror vanligaste sämre prestanda på bristen på statistik på tabellerna.  Se [underhålla tabellstatistik] [ Statistics] mer information om hur toocreate statistik och varför de är viktiga tooyour prestanda. |
| Låg samtidighet / frågor i kö |Förstå [hantering av arbetsbelastning] [ Workload management] är viktigt i ordning toounderstand hur toobalance minnesallokering med samtidighet. |
| Hur tooimplement bästa praxis |hello bästa frågeprestanda för plats toostart toolearn sätt tooimprove är [SQL Data Warehouse metodtips] [ SQL Data Warehouse best practices] artikel. |
| Hur tooimprove prestanda med skalning |Ibland hello lösningens tooimproving prestanda är toosimply lägga till flera compute power tooyour frågor från [skala ditt SQL Data Warehouse][Scaling your SQL Data Warehouse]. |
| Dåliga prestanda på grund av dålig index kvalitet |Några tillfällen frågor kan långsammare eftersom [dålig columnstore-index kvalitet][Poor columnstore index quality].  Finns den här artikeln för mer information och hur för[återskapa index tooimprove segment kvalitet][Rebuild indexes tooimprove segment quality]. |

## <a name="system-management"></a>Systemhantering
| Problem | Lösning |
|:--- |:--- |
| Felmeddelande 40847: Kunde inte utföra hello-åtgärden eftersom servern skulle överskrida hello tillåtna Database Transaction Unit-kvot för 45000. |Sänk antingen hello [DWU] [ DWU] av hello databas du försöker toocreate eller [begära en ökad kvot][request a quota increase]. |
| Undersöka användningen |Se [tabell storlekar] [ Table sizes] toounderstand hello användningen av systemet. |
| Att hantera tabeller |Se hello [tabell översikt] [ Overview] artikel för att få hjälp med att hantera dina tabeller.  Den här artikeln innehåller också länkar till mer detaljerad information som [tabell datatyper][Data types], [distribuerar en tabell][Distribute], [Indexering av en tabell][Index], [partitionering en tabell][Partition], [underhålla tabellstatistik] [ Statistics] och [temporära tabeller][Temporary]. |
|Transparent data kryptering (TDE) förloppsindikatorn uppdateras inte i hello Azure-portalen|Du kan visa hello tillståndet för TDE via [powershell](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption).|

## <a name="polybase"></a>Polybase
| Problem | Lösning |
|:--- |:--- |
| Läs in misslyckas på grund av stora rader |Stor rad stöd är för närvarande inte tillgängligt för Polybase.  Det innebär att om tabellen innehåller VARCHAR(MAX), NVARCHAR(MAX) eller VARBINARY(MAX), kan externa tabeller ska använda tooload dina data.  Belastningen för stora rader är för närvarande stöds bara via Azure Data Factory (med BCP), Azure Stream Analytics, SSIS, BCP eller hello SqlBulkCopy körs .NET-klass. PolyBase-stöd för stora rader läggs till i en framtida version. |
| BCP belastningen på tabellen med MAX-datatypen kan inte |Det finns ett känt problem som kräver att VARCHAR(MAX), NVARCHAR(MAX) eller VARBINARY(MAX) placeras i hello slutet av hello tabellen i vissa situationer.  Försök att flytta din MAX kolumner toohello slutet av hello tabell. |

## <a name="differences-from-sql-database"></a>Skillnader från SQL-databas
| Problem | Lösning |
|:--- |:--- |
| Funktioner som inte stöds SQL-databas |Se [stöds inte i tabellen funktioner][Unsupported table features]. |
| Datatyper stöds inte SQL-databas |Se [datatyper][Unsupported data types]. |
| Ta bort och uppdatera begränsningar |Se [uppdatering lösningar][UPDATE workarounds], [ta bort lösningar] [ DELETE workarounds] och [med CTAS toowork runt stöds inte UPPDATERINGEN och Ta bort syntax][Using CTAS toowork around unsupported UPDATE and DELETE syntax]. |
| MERGE-instruktionen stöds inte |Se [MERGE lösningar][MERGE workarounds]. |
| Begränsningar för lagrad procedur |Se [lagrade proceduren begränsningar] [ Stored procedure limitations] toounderstand vissa hello begränsningar av lagrade procedurer. |
| UDF: er stöder inte SELECT-uttryck |Det här är en aktuell begränsning i vår UDF: er.  Se [CREATE FUNCTION] [ CREATE FUNCTION] för hello syntax vi stöder. |

## <a name="next-steps"></a>Nästa steg
Om du har toofind en lösning tooyour problemet ovan, här är några andra resurser som du kan försöka.

* [Bloggar]
* [Funktionsbegäranden]
* [Videoklipp]
* [CAT-teambloggar]
* [Skapa ett supportärende]
* [MSDN-forum]
* [Stack Overflow-forum]
* [Twitter]

<!--Image references-->

<!--Article references-->
[Security overview]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT for Visual Studio]: ./sql-data-warehouse-install-visual-studio.md
[Drivers for Azure SQL Data Warehouse]: ./sql-data-warehouse-connection-strings.md
[Connect tooAzure SQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Skapa ett supportärende]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Scaling your SQL Data Warehouse]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md
[request a quota increase]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Learning how toomonitor your queries]: ./sql-data-warehouse-manage-monitor.md
[Provisioning instructions]: ./sql-data-warehouse-get-started-provision.md
[Configure server firewall access for your client IP]: ./sql-data-warehouse-get-started-provision.md#create-a-server-level-firewall-rule-in-the-azure-portal
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[Table sizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Poor columnstore index quality]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuild indexes tooimprove segment quality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Workload management]: ./sql-data-warehouse-develop-concurrency.md
[Using CTAS toowork around unsupported UPDATE and DELETE syntax]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[UPDATE workarounds]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[DELETE workarounds]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[MERGE workarounds]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Stored procedure limitations]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Authentication tooAzure SQL Data Warehouse]: ./sql-data-warehouse-authentication.md
[Working around hello PolyBase UTF-8 requirement]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[CREATE FUNCTION]: https://msdn.microsoft.com/library/mt203952.aspx
[sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Bloggar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[CAT-teambloggar]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Funktionsbegäranden]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videoklipp]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
