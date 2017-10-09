---
title: "rekommendationer för aaaApply - Azure SQL Database | Microsoft Docs"
description: "Du kan använda hello Azure portal toofind rekommendationer som kan optimera prestanda för din Azure SQL Database eller toocorrect vissa problem som identifieras i din arbetsbelastning."
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
ms.openlocfilehash: 0b2234840fc3254d235db9e7d18f5bc912851c6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-and-apply-performance-recommendations"></a>Hitta och tillämpa rekommendationer

Du kan använda hello Azure portal toofind rekommendationer som kan optimera prestanda för din Azure SQL Database eller toocorrect vissa problem som identifieras i din arbetsbelastning. **Prestanda rekommendation** sidan i Azure-portalen kan du toofind hello översta rekommendationer baserat på deras eventuella inverkan. 

## <a name="viewing-recommendations"></a>Visa rekommendationer

tooview och tillämpa rekommendationer, behöver du hello rätt [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md) behörigheter i Azure. **Läsaren**, **SQL DB-deltagare** behörigheter som är nödvändiga tooview rekommendationer och **ägare**, **SQL DB-deltagare** behörigheter som krävs tooexecute alla åtgärder. Skapa eller drop index och avbryta skapandet av index.

Använd hello följa steg toofind rekommendationer på Azure-portalen:

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Gå för**fler tjänster** > **SQL-databaser**, och välj din databas.
3. Navigera för**prestanda rekommendation** tooview tillgängliga rekommendationer för hello valda databasen.

Rekommendationer är shonw hello tabell liknande toohello som visas på följande bild hello:

![Rekommendationer](./media/sql-database-advisor-portal/recommendations.png)

Rekommendationer sorteras efter deras eventuella inverkan på prestanda i hello följande kategorier:

| Påverkan | Beskrivning |
|:--- |:--- |
| Hög |Hög inverkan rekommendationer bör ge hello mest betydande inverkan på prestanda. |
| Medel |Medelhög påverkan rekommendationer bör förbättra prestanda, men inte avsevärt. |
| Låg |Låg påverkan rekommendationer bör ge bättre prestanda än utan, men kan inte vara betydande förbättringar. |


> [!NOTE]
> Azure SQL Database måste toomonitor aktiviteter minst en dag i ordning tooidentify några rekommendationer. hello Azure SQL Database kan enklare att optimera för konsekvent frågemönster än den för slumpmässiga ojämn belastning för aktiviteten. Om rekommendationerna inte är tillgängliga corrently, hello **prestanda rekommendation** sidan visar ett meddelande som förklarar varför.
> 

Du kan också visa hello status för hello tidigare åtgärder. Välj en rekommendation eller status toosee mer information.

Här är ett exempel på ”Skapa indexet” rekommendation i hello Azure-portalen.

![Skapa index](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a>Tillämpa rekommendationer
Azure SQL Database ger dig fullständig kontroll över hur rekommendationerna är aktiverad med någon av följande tre alternativ hello: 

* Använda enskilda rekommendationer en i taget.
* Aktivera hello automatisk justering tooautomatically gäller rekommendationer.
* tooimplement en rekommendation köra manuellt, hello rekommenderas T-SQL-skript mot databasen.

Markera varje rekommendation tooview information och klicka på **Visa skript** tooreview hello mer information om hur hello rekommendation skapas.

hello databasen förblir online medan hello rekommendation används – en databas med hjälp av prestanda rekommendation eller automatisk justering aldrig tar offline.

### <a name="apply-an-individual-recommendation"></a>Använda en enskild rekommendation
Du kan granska och Godkänn rekommendationer en i taget.

1. På hello **rekommendationer** bladet Välj en rekommendation.
2. På hello **information** bladet och klickar på **tillämpa** knappen.
   
    ![Tillämpa rekommendation](./media/sql-database-advisor-portal/apply.png)

Valda rekommendation tillämpas på hello-databasen.

### <a name="removing-recommendations-from-hello-list"></a>Ta bort rekommendationer hello listan
Om din lista över rekommendationer innehåller objekt som du vill tooremove hello listan kan du ta bort hello rekommendation:

1. Välj en rekommendation i hello lista över **rekommendationer** tooopen hello information.
2. Klicka på **Ignorera** på hello **information** bladet.

Om du vill kan du lägga till borttagna objekten tillbaka toohello **rekommendationer** lista:

1. På hello **rekommendationer** bladet och klickar på **Visa ignorerade**.
2. Välj en borttagna objekt från hello listan tooview information.
3. Du kan också klicka på **Ångra Ignorera** tooadd hello index tillbaka toohello huvudsakliga lista över **rekommendationer**.


### <a name="enable-automatic-tuning"></a>Aktivera automatisk inställning
Du kan ange hello Azure SQL Database tooimplement rekommendationer automatiskt. Rekommendationer blir tillgängliga tillämpas de automatiskt. Som med alla rekommendationer som hanteras av hello kommer service om hello prestanda påverkas negativt hello rekommendation att återställas.

1. På hello **rekommendationer** bladet, klickar du på **automatisera**:
   
    ![Inställningar för klassificering](./media/sql-database-advisor-portal/settings.png)
2. Välj åtgärder tooautomate:
   
    ![Rekommenderat index](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-hello-recommended-t-sql-script"></a>Köra manuellt hello rekommenderas T-SQL-skript
Markera varje rekommendation och klicka på **Visa skript**. Köra detta skript mot databasen toomanually gäller hello rekommendation.

*Index som utförs manuellt inte övervakas och valideras för prestandapåverkan av hello tjänsten* så rekommenderas du övervaka dessa index efter skapa tooverify de ger prestandavinster och justera eller ta bort dem om nödvändiga. Mer information om hur du skapar index finns [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).

### <a name="canceling-recommendations"></a>Om du avbryter rekommendationer
Rekommendationer som finns i en **väntande**, **verifiera**, eller **lyckade** status kan avbrytas. Rekommendationer med statusen **Executing** kan inte avbrytas.

1. Välj en rekommendation i hello **justera historik** område tooopen hello **rekommendationsdetaljer** bladet.
2. Klicka på **Avbryt** tooabort hello process för tillämpning av hello rekommendation.

## <a name="monitoring-operations"></a>Övervakningsåtgärder
Tillämpa en rekommendation kanske inte omedelbart sker. hello portal innehåller information om hello status för rekommendation. hello följande är möjliga tillstånd att ett index i:

| Status | Beskrivning |
|:--- |:--- |
| Väntande åtgärder |Tillämpa rekommendation kommandot har tagits emot och har schemalagts för körning. |
| Köra |hello rekommendation tillämpas. |
| Verifiera |Rekommendation har tillämpats och hello tjänst mäter hello fördelar. |
| Lyckades |Rekommendation har tillämpats och fördelar har tagits mäts. |
| Fel |Ett fel uppstod under hello process för tillämpning av hello rekommendation. Detta kan vara ett övergående problem eller eventuellt ett schema ändra toohello tabell och hello skript är inte längre giltig. |
| Återställa |hello rekommendation tillämpades, men har bedömts vara icke-performant och återställs automatiskt. |
| Har återställts |hello rekommendation återställdes. |

Klicka på ett pågående rekommendation från hello listan toosee mer information:

![Rekommenderat index](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a>Återställa en rekommendation
Om du använde hello prestanda rekommendationer tooapply hello rekommendation återställs (vilket innebär att du inte manuellt köra hello T-SQL-skript) den automatiskt den om den hittar hello prestanda påverkan toobe negativt. Om av någon anledning som du bara vill toorevert en rekommendation kan du göra hello följande:

1. Välj en har tillämpade rekommendation i hello **justera historik** område.
2. Klicka på **Återställ** på hello **rekommendationsdetaljer** bladet.

![Rekommenderat index](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a>Övervaka prestandapåverkan index-rekommendationer
När rekommendationerna har implementerats (för närvarande, index-åtgärder och parameterstyra frågor rekommendationer endast) kan du klicka på **frågan insikter** på hello rekommendation information bladet tooopen [fråga Insikter i frågeprestanda](sql-database-query-performance.md) och se hello prestandapåverkan top-frågor.

![Övervakaren prestandapåverkan](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a>Sammanfattning
Azure SQL-databasen innehåller rekommendationer för att förbättra prestanda för SQL-databasen. Du får en bra hjälp med att optimera din databas och slutligen förbättra frågeprestanda genom att tillhandahålla T-SQL-skript, samt individuella och fullständigt-automatisk.

## <a name="next-steps"></a>Nästa steg
Övervaka dina rekommendationer och fortsätta tooapply dem toorefine prestanda. Databasarbetsbelastningar är dynamiska och ändra kontinuerligt. Azure SQL Database fortsätter toomonitor och ger rekommendationer som kan förbättra dina databasprestanda. 

* Se [automatisk justering](sql-database-automatic-tuning.md) toolearn mer om hello automatisk justering i Azure SQL Database.
* Se [rekommendationer](sql-database-advisor.md) en översikt över Azure SQL Database prestanda rekommendationer.
* Se [insikter i frågeprestanda](sql-database-query-performance.md) toolearn om hur du visar hello prestandapåverkan top-frågor.

## <a name="additional-resources"></a>Ytterligare resurser
* [Query Store](https://msdn.microsoft.com/library/dn817826.aspx)
* [SKAPA INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
* [Rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md)

