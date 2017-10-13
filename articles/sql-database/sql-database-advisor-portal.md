---
title: "Tillämpa rekommendationer - Azure SQL Database | Microsoft Docs"
description: "Du kan använda Azure-portalen för att hitta rekommendationer som kan optimera prestanda för din Azure SQL Database eller åtgärda vissa problem som identifieras i din arbetsbelastning."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: cda8a646-0584-4368-b28a-85cdd9b54fcd
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 018afaa8b08bd001e55693390e80c8e2c4f33a30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="find-and-apply-performance-recommendations"></a>Hitta och tillämpa rekommendationer

Du kan använda Azure-portalen för att hitta rekommendationer som kan optimera prestanda för din Azure SQL Database eller åtgärda vissa problem som identifieras i din arbetsbelastning. **Prestanda rekommendation** sidan i Azure-portalen kan du hitta översta rekommendationer baserat på deras eventuella inverkan. 

## <a name="viewing-recommendations"></a>Visa rekommendationer

Om du vill visa och använda rekommendationer, behöver du rätt [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md) behörigheter i Azure. **Läsaren**, **SQL DB-deltagare** behörigheter som krävs för att visa rekommendationer och **ägare**, **SQL DB-deltagare** behörigheter som krävs för att utföra några åtgärder; Skapa eller drop index och avbryta skapandet av index.

Använd följande steg för att hitta rekommendationer på Azure-portalen:

1. Logga in på [Azure Portal](https://portal.azure.com/).
2. Gå till **fler tjänster** > **SQL-databaser**, och välj din databas.
3. Gå till **prestanda rekommendation** att visa tillgängliga rekommendationer för den valda databasen.

Rekommendationer är shonw i tabellen som liknar det som visas på följande bild:

![Rekommendationer](./media/sql-database-advisor-portal/recommendations.png)

Rekommendationer sorteras efter deras eventuella inverkan på prestanda i följande kategorier:

| Påverkan | Beskrivning |
|:--- |:--- |
| Hög |Hög inverkan rekommendationer bör ge viktigaste prestandapåverkan. |
| Medel |Medelhög påverkan rekommendationer bör förbättra prestanda, men inte avsevärt. |
| Låg |Låg påverkan rekommendationer bör ge bättre prestanda än utan, men kan inte vara betydande förbättringar. |


> [!NOTE]
> Azure SQL Database måste att övervaka aktiviteter minst en dag för att identifiera några rekommendationer. Azure SQL-databasen kan enklare att optimera för konsekvent frågemönster än den för slumpmässiga ojämn belastning för aktiviteten. Om rekommendationerna inte är tillgängliga, corrently den **prestanda rekommendation** sidan visar ett meddelande som förklarar varför.
> 

Du kan också visa status för tidigare åtgärder. Välj en rekommendation eller status för att se mer information.

Här är ett exempel på ”Skapa indexet” rekommendation i Azure-portalen.

![Skapa index](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a>Tillämpa rekommendationer
Azure SQL Database ger dig fullständig kontroll över hur rekommendationerna är aktiverad med någon av följande tre alternativ: 

* Använda enskilda rekommendationer en i taget.
* Aktivera automatisk inställning för att automatiskt använda rekommendationer.
* För att implementera en rekommendation manuellt, kör du rekommenderade T-SQL-skript mot databasen.

Välj en rekommendation att visa information och klickar sedan på **Visa skript** att granska exakt information om hur rekommendationen skapas.

Databasen är online samtidigt rekommendationen tillämpas--med hjälp av prestanda rekommendation eller automatisk justering aldrig tar en databas offline.

### <a name="apply-an-individual-recommendation"></a>Använda en enskild rekommendation
Du kan granska och Godkänn rekommendationer en i taget.

1. På den **rekommendationer** bladet Välj en rekommendation.
2. På den **information** bladet och klickar på **tillämpa** knappen.
   
    ![Tillämpa rekommendation](./media/sql-database-advisor-portal/apply.png)

Valda rekommendation tillämpas på databasen.

### <a name="removing-recommendations-from-the-list"></a>Tar bort rekommendationer från listan
Om din lista över rekommendationer innehåller objekt som du vill ta bort från listan kan du ta bort rekommendationen:

1. Välj en rekommendation i listan över **rekommendationer** att öppna information.
2. Klicka på **Ignorera** på den **information** bladet.

Om du vill kan du lägga till borttagna objekten tillbaka till den **rekommendationer** lista:

1. På den **rekommendationer** bladet och klickar på **Visa ignorerade**.
2. Välj en borttagna objekt från listan för att visa information om den.
3. Du kan också klicka på **Ångra Ignorera** att lägga till indexet huvudsakliga listan över **rekommendationer**.


### <a name="enable-automatic-tuning"></a>Aktivera automatisk inställning
Du kan ställa in Azure SQL-databasen för att implementera rekommendationer automatiskt. Rekommendationer blir tillgängliga tillämpas de automatiskt. Som med alla rekommendationer som hanteras av tjänsten om prestandapåverkan är negativt kommer rekommendationen att återställas.

1. På den **rekommendationer** bladet, klickar du på **automatisera**:
   
    ![Inställningar för klassificering](./media/sql-database-advisor-portal/settings.png)
2. Välj åtgärder för att automatisera:
   
    ![Rekommenderat index](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-the-recommended-t-sql-script"></a>Manuellt köra rekommenderade T-SQL-skript
Markera varje rekommendation och klicka på **Visa skript**. Köra detta skript mot databasen för att använda rekommendationen manuellt.

*Index som utförs manuellt inte övervakas och valideras för prestandapåverkan av tjänsten* så vi rekommenderar att du övervaka dessa index när de har skapats för att verifiera de ger prestandavinster och justera eller ta bort dem om det behövs. Mer information om hur du skapar index finns [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).

### <a name="canceling-recommendations"></a>Om du avbryter rekommendationer
Rekommendationer som finns i en **väntande**, **verifiera**, eller **lyckade** status kan avbrytas. Rekommendationer med statusen **Executing** kan inte avbrytas.

1. Välj en rekommendation i den **justera historik** område för att öppna den **rekommendationsdetaljer** bladet.
2. Klicka på **Avbryt** att avbryta en process för tillämpning rekommendationen.

## <a name="monitoring-operations"></a>Övervakningsåtgärder
Tillämpa en rekommendation kanske inte omedelbart sker. Portalen innehåller information om status för rekommendation. Följande är möjliga tillstånd att ett index i:

| Status | Beskrivning |
|:--- |:--- |
| Väntande åtgärder |Tillämpa rekommendation kommandot har tagits emot och har schemalagts för körning. |
| Köra |Rekommendationen som används. |
| Verifiera |Rekommendation har tillämpats och tjänsten mäter fördelarna. |
| Lyckades |Rekommendation har tillämpats och fördelar har tagits mäts. |
| Fel |Det uppstod ett fel i samband med tillämpandet av rekommendationen. Detta kan vara ett övergående problem eller eventuellt ett schema ändra i tabellen och skriptet är inte längre giltig. |
| Återställa |Rekommendationen tillämpades, men har bedömts vara icke-performant och återställs automatiskt. |
| Har återställts |Rekommendationen återställdes. |

Klicka på ett pågående rekommendation från listan över att se mer information:

![Rekommenderat index](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a>Återställa en rekommendation
Om du använde prestanda rekommendationer för att tillämpa rekommendationerna återställs (vilket innebär att du inte manuellt köra T-SQL-skript) den automatiskt den om den hittar prestandapåverkan vara negativ. Om du av någon anledning som du vill återställa en rekommendation kan du göra följande:

1. Välj har tillämpade rekommendation i den **justera historik** område.
2. Klicka på **Återställ** på den **rekommendationsdetaljer** bladet.

![Rekommenderat index](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a>Övervaka prestandapåverkan index-rekommendationer
När rekommendationerna har implementerats (för närvarande, index-åtgärder och parameterstyra frågor rekommendationer endast) kan du klicka på **frågan insikter** på informationsbladet rekommendation att öppna [fråga Insikter i frågeprestanda](sql-database-query-performance.md) och se påverkan på prestandan för din de vanligaste frågorna.

![Övervakaren prestandapåverkan](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a>Sammanfattning
Azure SQL-databasen innehåller rekommendationer för att förbättra prestanda för SQL-databasen. Du får en bra hjälp med att optimera din databas och slutligen förbättra frågeprestanda genom att tillhandahålla T-SQL-skript, samt individuella och fullständigt-automatisk.

## <a name="next-steps"></a>Nästa steg
Övervaka dina rekommendationer och fortsätta att använda dem för att förbättra prestanda. Databasarbetsbelastningar är dynamiska och ändra kontinuerligt. Azure SQL-databasen fortsätter att övervaka och ger rekommendationer som kan förbättra dina databasprestanda. 

* Se [automatisk justering](sql-database-automatic-tuning.md) att lära dig mer om automatisk inställning i Azure SQL Database.
* Se [rekommendationer](sql-database-advisor.md) en översikt över Azure SQL Database prestanda rekommendationer.
* Se [insikter i frågeprestanda](sql-database-query-performance.md) mer information om hur du visar påverkan på prestandan för din de vanligaste frågorna.

## <a name="additional-resources"></a>Ytterligare resurser
* [Query Store](https://msdn.microsoft.com/library/dn817826.aspx)
* [SKAPA INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
* [Rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md)

