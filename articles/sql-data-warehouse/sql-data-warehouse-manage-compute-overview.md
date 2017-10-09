---
title: "aaaManage beräkningskraft i Azure SQL Data Warehouse (översikt) | Microsoft Docs"
description: "Prestanda skala ut funktioner i Azure SQL Data Warehouse. Skala ut genom att justera dwu: er eller pausa och återuppta beräkning resurser toosave kostnader."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e13a82b0-abfe-429f-ac3c-f2b6789a70c6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/22/2017
ms.author: elbutter
ms.openlocfilehash: 1ffbe8d694ac181eaeb6f585a2cee87a570ed7d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Hantera datorkraft i Azure SQL Data Warehouse (översikt)
> [!div class="op_single_selector"]
> * [Översikt](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

hello-arkitekturen för SQL Data Warehouse separerar lagring och beräkning, så att varje tooscale oberoende av varandra. Beräkning kan därför vara skalade toomeet prestandakrav oberoende av hello mängden data. En naturlig följd av den här arkitekturen är att [fakturering] [ billed] för beräkning och lagring är desamma. 

Den här översikten beskriver hur skala ut fungerar med SQL Data Warehouse och hur tooutilize hello pausa, återuppta och skala funktionerna för SQL Data Warehouse. Kontakta hello [enheter (dwu: er) för datalager] [ data warehouse units (DWUs)] sidan toolearn hur dwu: er och prestanda är relaterade. 

## <a name="how-compute-management-operations-work-in-sql-data-warehouse"></a>Beräkna hur hanteringsåtgärder fungerar i SQL Data Warehouse
hello-arkitekturen för SQL Data Warehouse består av en control-noden, datornoder och hello lagringsskikt sprids 60-distributioner. 

Under normala aktiv session i SQL Data Warehouse systemets huvudnod hanterar hello metadata och innehåller hello distribuerad frågeoptimerare. Under den här huvudnod är compute-noder och lagring-lagret. Systemet har en huvudnod fyra datornoder och hello lagringsskikt som består av 60 distributioner för en DWU 400. 

När du genomgå en skala eller pausa åtgärden hello systemet först stoppar alla inkommande förfrågningar och sedan återställer transaktioner tooensure ett konsekvent tillstånd. För skala utförs skalning endast när den här transaktionella återställning har slutförts. En skala upp åtgärd hello system etablerar hello extra önskat antal datornoder och startar sedan återansluta hello noder toohello lagring beräkningslager. Hello onödiga noder släpps och hello återstående datornoderna återansluta själva toohello lämpligt antal distributioner för en skala ned-åtgärd. För en paus åtgärden beräkna alla noder släpps och datorn kommer att göras en mängd olika metadata operations tooleave slutliga systemet i ett stabilt tillstånd.

| DWU  | \#med beräkningsnoder | \#för distributioner per nod |
| ---- | ------------------ | ---------------------------- |
| 100  | 1                  | 60                           |
| 200  | 2                  | 30                           |
| 300  | 3                  | 20                           |
| 400  | 4                  | 15                           |
| 500  | 5                  | 12                           |
| 600  | 6                  | 10                           |
| 1000 | 10                 | 6                            |
| 1200 | 12                 | 5                            |
| 1500 | 15                 | 4                            |
| 2000 | 20                 | 3                            |
| 3000 | 30                 | 2                            |
| 6000 | 60                 | 1                            |

hello tre primära funktioner för hantering av beräkning är:

1. Pausa
2. Återuppta
3. Skala

Var och en av dessa åtgärder kan ta flera minuter toocomplete. Om du är skalning/pausa/återupptar automatiskt kan kanske du vill tooimplement logik tooensure att vissa åtgärder har slutförts innan du fortsätter med en annan åtgärd. 

Kontrollera hello databastillståndet via olika slutpunkter kan du toocorrectly implementera automatisering av dessa åtgärder. hello portal ger meddelande vid slutförandet av en åtgärd och hello databaser aktuella tillstånd, men tillåter inte programmässiga kontroll av tillståndet. 

>  [!NOTE]
>
>  Beräkna hanteringsfunktioner inte finns för alla slutpunkterna.
>
>  

|              | Pausa/Fortsätt | Skala | Kontrollera databasens status |
| ------------ | ------------ | ----- | -------------------- |
| Azure Portal | Ja          | Ja   | **Nej**               |
| PowerShell   | Ja          | Ja   | Ja                  |
| REST API     | Ja          | Ja   | Ja                  |
| T-SQL        | **Nej**       | Ja   | Ja                  |



<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Skala bearbetning

Prestanda i SQL Data Warehouse mäts i [enheter (dwu: er) för datalager] [ data warehouse units (DWUs)] som är en abstracted mått på beräkningsresurser t.ex CPU, minne och i/o-bandbredd. En användare som önskar tooscale sina systemets prestanda kan göra detta på olika sätt som hello portal, T-SQL och REST API: er. 

### <a name="how-do-i-scale-compute"></a>Hur skala beräkning?
Datorkraft hanteras SQL Data Warehouse genom att ändra hello DWU-inställningar. Prestandaökningar [linjärt] [ linearly] när du lägger till flera DWU för vissa åtgärder.  Vi erbjuder DWU-erbjudanden som säkerställer att prestandan kommer att ändras märkbart när du skalar systemet upp eller ned. 

tooadjust dwu: er, du kan använda någon av dessa enskilda metoder.

* [Skala datorkraft med Azure-portalen][Scale compute power with Azure portal]
* [Skala datorkraft med PowerShell][Scale compute power with PowerShell]
* [Skala datorkraft med REST API: er][Scale compute power with REST APIs]
* [Skala datorkraft med TSQL][Scale compute power with TSQL]

### <a name="how-many-dwus-should-i-use"></a>Hur många dwu: er ska jag använda?

toounderstand vad ditt perfekta DWU-värde är, försök att skala upp och ner och köra några frågor efter att läsa in data. Eftersom det går snabbt att skala, kan du försöka olika prestandanivåer i en timme eller mindre. 

> [!Note] 
> SQL Data Warehouse är utformad tooprocess stora mängder data. toosee dess true funktioner för skalning, speciellt vid större dwu: er som du vill använda toouse stora datamängder som närmar sig eller större än 1 TB.

Rekommendationer för att hitta hello bästa DWU för din arbetsbelastning:

1. Börja genom att välja en mindre DWU prestandanivå för ett datalager under utveckling.  En bra utgångspunkt är DW400 eller DW200.
2. Övervakare för programmets prestanda, sett hello antalet dwu: er som valts jämfört med toohello prestanda du se.
3. Avgör hur mycket snabbare eller långsammare prestanda bör vara för du tooreach hello optimala prestandanivån för dina krav genom att överta linjär skala.
4. Öka eller minska antalet hello dwu: er i andel toohow mycket snabbare eller långsammare som du vill tooperform din arbetsbelastning. 
5. Fortsätt att göra justeringar tills du når nivån optimal prestanda för dina affärsbehov.

> [!NOTE]
>
> Frågeprestanda ökar bara med flera parallellisering om hello arbete delas mellan beräkningsnoder. Om du upptäcker att skalning inte är ändra din prestanda, finns våra artiklar toocheck om dina data fördelas ojämnt eller om du introducerar en stor mängd data movement för prestandajustering. 

### <a name="when-should-i-scale-dwus"></a>När bör jag skala dwu: er?
Skalning dwu: er ändrar hello följande viktiga scenarier:

1. Ändra linjärt prestanda för hello för sökningar, aggregeringar och CTAS uttryck
2. Öka hello antal läsare och skrivare vid inläsning av med PolyBase
3. Maximalt antal samtidiga frågor och samtidighet fack

Rekommendationer för när tooscale dwu: er:

1. Innan du utför en tung datainläsnings- eller omvandlingsåtgärd åtgärd skala upp dwu: er så att dina data blir tillgängliga snabbare.
2. Skala tooaccommodate större antal samtidiga frågor under belastning kontorstid. 

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pausa beräkning
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

toopause en databas med någon av metoderna enskilda.

* [Pausa beräkning med Azure-portalen][Pause compute with Azure portal]
* [Pausa beräkning med PowerShell][Pause compute with PowerShell]
* [Pausa beräkning med REST API: er][Pause compute with REST APIs]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Återuppta beräkning
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

tooresume en databas med någon av metoderna enskilda.

* [Återuppta beräkning med Azure-portalen][Resume compute with Azure portal]
* [Återuppta beräkning med PowerShell][Resume compute with PowerShell]
* [Återuppta beräkning med REST API: er][Resume compute with REST APIs]

<a name="check-compute-bk"></a>

## <a name="check-database-state"></a>Kontrollera databasens status 

tooresume en databas med någon av metoderna enskilda.

- [Kontrollera databasens status med T-SQL][Check database state with T-SQL]
- [Kontrollera databasens status med PowerShell][Check database state with PowerShell]
- [Kontrollera databasens status med REST API: er][Check database state with REST APIs]

## <a name="permissions"></a>Behörigheter

Skalning hello-databasen kräver hello behörigheterna som beskrivs i [ALTER DATABASE][ALTER DATABASE].  Pausa och återuppta kräver hello [SQL DB-deltagare] [ SQL DB Contributor] behörighet, särskilt Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nästa steg
Se följande artiklar toohelp du känner till vissa ytterligare KPI begreppen toohello:

* [Hantering av arbetsbelastning och samtidighet][Workload and concurrency management]
* [Översikt över tabellen design][Table design overview]
* [Tabell-distribution][Table distribution]
* [Tabell indexering][Table indexing]
* [Tabellpartitionering][Table partitioning]
* [Tabellstatistik][Table statistics]
* [Bästa praxis][Best practices]

<!--Image reference-->

<!--Article references-->
[data warehouse units (DWUs)]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
[billed]: https://azure.microsoft.com/en-us/pricing/details/sql-data-warehouse/
[linearly]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
[Scale compute power with Azure portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[Scale compute power with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Scale compute power with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Scale compute power with TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Pause compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Pause compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Pause compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Resume compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Resume compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Resume compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Check database state with T-SQL]: ./sql-data-warehouse-manage-compute-tsql.md#check-database-state-and-operation-progress
[Check database state with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#check-database-state
[Check database state with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#check-database-state

[Workload and concurrency management]: ./sql-data-warehouse-develop-concurrency.md
[Table design overview]: ./sql-data-warehouse-tables-overview.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexing]: ./sql-data-warehouse-tables-index.md
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Table statistics]: ./sql-data-warehouse-tables-statistics.md
[Best practices]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Contributor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
