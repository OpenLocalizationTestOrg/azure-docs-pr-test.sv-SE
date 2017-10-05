---
title: 'Azure-portalen: skapa och hantera en SQL Database-elastisk pool | Microsoft Docs'
description: "Lär dig hur du använder Azure portal och SQL-databas inbyggd intelligens att hantera, övervaka och en skalbar elastisk pool för att optimera databasprestanda och hantera kostnad få rätt storlek."
keywords: 
services: sql-database
documentationcenter: 
author: ninarn
manager: jhubbard
editor: cgronlun
ms.assetid: 3dc9b7a3-4b10-423a-8e44-9174aca5cf3d
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.date: 06/06/2016
ms.author: ninarn
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 4ffd1db31f42967dc7f07aa979898dddbb333641
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-an-elastic-pool-with-the-azure-portal"></a>Skapa och hantera en elastisk pool med Azure-portalen
Det här avsnittet beskrivs hur du skapar och hanterar skalbara [elastiska pooler](sql-database-elastic-pool.md) med Azure-portalen. Du kan också skapa och hantera en Azure elastisk pool med [PowerShell](sql-database-elastic-pool-manage-powershell.md), REST-API eller [C#](sql-database-elastic-pool-manage-csharp.md). Du kan också skapa och flytta databaser till och från elastiska pooler med [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).

## <a name="create-an-elastic-pool"></a>Skapa en elastisk pool 

Det finns två sätt som du kan skapa en elastisk pool. Du kan göra det från grunden om du vet vilken typ av pool du vill ha, eller så kan du börja med en rekommendation från tjänsten. SQL Database har inbyggd intelligens som rekommenderar en elastisk pool-installationen om det är mer kostnadseffektivt baserat på den senaste användningstelemetrin för dina databaser.

Du kan skapa flera pooler på en server, men du kan inte lägga till databaser från olika servrar i samma pool. 

> [!NOTE]
> Elastiska pooler är allmänt tillgängliga (GA) i alla Azure-regioner utom Västra Indien där de genomgår förhandsgranskning.  GA för elastiska pooler i den här regionen inträffar så snart som möjligt.
>

### <a name="step-1-create-an-elastic-pool"></a>Steg 1: Skapa en elastisk pool

Skapa en elastisk pool från en befintlig **server** blad i portalen är det enklaste sättet att flytta befintliga databaser till en elastisk pool.

> [!NOTE]
> Du kan också skapa en elastisk pool genom att söka **elastiska SQL-poolen** i den **Marketplace** eller klicka på **+ Lägg till** i den **SQL elastiska pooler** Bläddra i bladet. Du kan ange en ny eller befintlig server via den här poolen etablering av arbetsflöde.
>
>

1. I den [Azure-portalen](http://portal.azure.com/), klickar du på **fler tjänster**  **>**  **SQL-servrar**, och klicka sedan på den server som innehåller den databaser som du vill lägga till en elastisk pool.
2. Klicka på **Ny pool**.

    ![Lägg till poolen till en server](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    **-ELLER-**

    Du kan se ett meddelande om att det finns rekommenderade elastiska pooler för servern. Klicka på meddelandet för att se de rekommenderade poolerna baserat på historisk telemetri över databasanvändningen och klicka på nivån för att visa mer information och anpassa poolen. Se [Förstå pool-rekommendationer](#understand-elastic-pool-recommendations) lite senare i ämnet för hur rekommendationen görs.

    ![rekommenderad pool](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. Den **elastisk pool** bladet visas, som är där du anger inställningarna för din pool. Om du klickade på **ny pool** i föregående steg, prisnivån är **Standard** som standard och inga databaser har valts. Du kan skapa en tom pool eller ange en uppsättning befintliga databaser från den servern, som du vill flytta till poolen. Om du skapar en rekommenderad pool rekommenderade prisnivå, inställningar och listan över databaser är förinställd, men du kan ändra dem.

    ![Konfigurera elastisk pool](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. Ange ett namn för den elastiska poolen, eller lämna den som standard.

### <a name="step-2-choose-a-pricing-tier"></a>Steg 2: Välj prisnivå.

Prisnivån för poolen avgör vilka funktioner som är tillgängliga för elastics i poolen och maximala antalet edtu: er (eDTU MAX) och lagringsutrymme (GB) är tillgängliga för varje databas. Mer information finns i tjänstnivåer.

Om du vill ändra prisnivå för poolen klickar du på **Prisnivå**, klickar på önskad prisnivå och sedan på **Välj**.

> [!IMPORTANT]
> När du valt prisnivå och bekräftat ditt val genom att klicka på **OK** i det sista steget, kommer du inte kunna ändra prisnivå för poolen igen. Om du vill ändra prisnivå för en befintlig elastisk pool, skapa en elastisk pool i den önskade prisnivån och migrera databaserna till den nya poolen.
>

![Välj en prisnivå](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

### <a name="step-3-configure-the-pool"></a>Steg 3: Konfigurera poolen

När du har ställt in prisnivå, klickar du på Konfigurera pool där du lägger till databaser, ange pool-edtu: er och lagring (pool-GB) och när du anger den min och max edtu: er för elastics i poolen.

1. Klicka på **Konfigurera pool**
2. Välj de databaser som du vill lägga till i poolen. Det här steget är valfritt medan du skapar poolen. Databaser kan läggas till efter att poolen har skapats.
    För att lägga till databaser, klickar du på **Lägg till databas**, klickar på de databaser som du vill lägga till och klickar sedan på knappen **Välj**.

    ![Lägg till databaser](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    Om de databaser du arbetar med har tillräckligt med historisk användningstelemetri, kommer stapeldiagrammen **Beräknad eDTU- och GB-användning** och **Faktisk eDTU-användning** att uppdateras för att hjälpa dig att fatta konfigurationsbeslut. Tjänsten kan också ge dig ett rekommendationsmeddelande för att hjälpa dig få rätt storlek på poolen. Se [Dynamiska rekommendationer](#understand-elastic-pool-recommendations).

3. Använd kontrollerna på sidan **Konfigurera pool** för att kolla igenom inställningarna och konfigurera din pool. Se [begränsningar för elastiska pooler](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) för mer information om begränsningarna för varje tjänstnivå och se [pris- och prestandaöverväganden för elastiska pooler](sql-database-elastic-pool.md) för detaljerad vägledning om rätt storlek för en elastisk pool. Mer information om poolinställningar finns [elastisk pool egenskaper](sql-database-elastic-pool.md#database-properties-for-pooled-databases).

    ![Konfigurera elastisk pool](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. Klicka på **Välj** på bladet **Konfigurera pool**, efter att du har ändrat inställningarna.
5. Klicka på **OK** för att skapa poolen.

## <a name="understand-elastic-pool-recommendations"></a>Förstå rekommendationer för elastiska pooler

SQL Database-tjänsten utvärderar användningshistorik och rekommenderar en eller flera pooler när det är mer kostnadseffektivt än att använda enskilda databaser. Varje rekommendation är konfigurerad med en unik delmängd av serverns databaser som bäst passar i poolen.

![rekommenderad pool](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

Pool-rekommendationerna omfattar:

- En prisnivå för poolen (Basic, Standard, Premium eller Premium RS)
- Lämpliga **POOL-eDTU:er** (kallas även Max eDTU:er per pool)
- **eDTU MAX** och **eDTU Min** per databas
- Listan över rekommenderade databaser för poolen

> [!IMPORTANT]
> Tjänsten tar hänsyn till de senaste 30 dagarnas telemetri vid rekommendation av pooler. Det måste finnas minst 7 dagar för en databas ska anses som en kandidat för en elastisk pool. Databaser som redan finns i en elastisk pool är inte rekommenderade kandidater för elastiska pooler.
>

Tjänsten utvärderar resursbehov och kostnadseffektivitet vid flytt av de enskilda databaserna i varje tjänstnivå till pooler inom samma nivå. Alla standard-databaser på servern utvärderas exempelvis för hur de skulle passa in i en standard elastisk pool. Det innebär att tjänsten inte göra rekommendationer mellan olika nivåer, som att flytta en standard-databas till en premium-pool.

När du lägger till databaserna i poolen, genereras dynamiskt rekommendationer baserat på historisk användning av databaserna som du har valt. De här rekommendationerna som visas i eDTU och GB Användningsdiagram och i en rekommendations-banderoll överst i den **konfigurera pool** bladet. De här rekommendationerna är avsedda att hjälpa dig att skapa en elastisk pool som är optimerade för dina specifika databaser.

![dynamiska rekommendationer](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## <a name="manage-and-monitor-an-elastic-pool"></a>Hantera och övervaka en elastisk pool

Du kan använda Azure-portalen för att övervaka och hantera en elastisk pool och databaserna i poolen. Du kan övervaka användningen av en elastisk pool och databaserna i poolen från portalen. Du kan också göra en uppsättning ändringar i den elastiska poolen och skicka alla ändringar på samma gång. De här förändringarna innefattar att lägga till eller ta bort databaser, ändra inställningarna elastisk pool eller ändra databasinställningarna.

Följande bild visar ett exempel elastisk pool. Vyn innehåller:

*  Diagram för övervakning av resursanvändningen för både den elastiska poolen och databaser som ingår i poolen.
*  Den **konfigurera** pool för att göra ändringar i den elastiska poolen.
*  Den **Skapa databas** knapp som skapar en databas och lägger till den aktuella elastisk pool.
*  Elastiska jobb som hjälper dig hantera stort antal databaser genom att köra Transact-SQL-skript mot alla databaser i en lista.

![Pool-vy][2]

Du kan gå till en viss pool för att se dess resursutnyttjande. Som standard konfigureras poolen om du vill visa lagrings- och eDTU-användning för den senaste timmen. Diagrammet kan konfigureras för att visa olika mätvärden via olika tidsfönster.

1. Välj en elastisk pool att arbeta med.
2. Under **elastisk Pool övervakning** är ett diagram med etiketten **resursutnyttjande**. Klicka på diagrammet.

    ![Övervakning av elastisk pool][3]

    Den **mått** blad öppnas och visar en detaljerad vy av de angivna mätvärdena via den angivna tidsperioden.   

    ![Bladet Mått][9]

### <a name="to-customize-the-chart-display"></a>Du kan anpassa diagram

Du kan redigera diagrammet och mått bladet för att visa andra mått som CPU-procent, data IO-procent och loggen IO-procent som används.

1. Klicka på bladet mått **redigera**.

    ![Klicka på Redigera][6]

2. I den **redigera diagram** bladet Välj ett tidsintervall (efter timme, dag, eller föregående vecka), eller klicka på **anpassade** att välja alla datumintervall under de senaste två veckorna. Välj diagramtyp (stapel- eller rad) och välj sedan resurserna som ska övervaka.

   > [!Note]
   > Endast mått med samma enheten kan visas i diagrammet på samma gång. Till exempel om du väljer ”eDTU i procent” kan du bara välja andra mått med procentandel som enheten.
   >

    ![Klicka på Redigera](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

3. Klicka sedan på **OK**.

## <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Hantera och övervaka databaser i en elastisk pool

Enskilda databaser kan också övervakas för potentiella problem.

1. Under **elastisk databas övervakning**, det finns ett diagram som visar mått för fem databaser. Som standard i diagrammet visas de översta 5 databaserna i poolen efter genomsnittlig eDTU-användning under den senaste timmen. Klicka på diagrammet.

    ![Övervakning av elastisk pool][4]

2. Den **databasen resursutnyttjande** bladet visas. Detta ger en detaljerad vy av databasanvändningen i poolen. Med rutnätet i den nedre delen av bladet kan välja du alla databaser i poolen för att visa dess användning i diagrammet (upp till 5 databaser). Du kan också anpassa fönstret mått och tid som visas i diagrammet genom att klicka på **redigera diagram**.

    ![-Databasresursbladet användning][8]

### <a name="to-customize-the-view"></a>Anpassa vyn

1. I den **databasen resursutnyttjande** bladet, klickar du på **redigera diagram**.

    ![Klicka på Redigera diagram](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. I den **redigera** diagram bladet väljer du ett tidsintervall (efter timme eller senaste 24 timmarna) eller klicka på **anpassade** att välja en annan dag under de senaste 2 veckorna ska visas.

    ![Klicka på anpassad](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Klicka på den **databaser genom att jämföra** listrutan och välj ett annat mått som ska användas vid jämförelse av databaser.

    ![Redigera schemat](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Att välja databaser för att övervaka

I databaslistan i den **databasen resursutnyttjande** bladet hittar du viss databaser genom att titta igenom sidorna i listan eller genom att skriva namnet på en databas. Använd kryssrutan för att välja databasen.

![Sök efter databaser för att övervaka][7]


## <a name="add-an-alert-to-an-elastic-pool-resource"></a>Lägga till en avisering till en elastisk pool-resurs

Du kan lägga till regler för en elastisk pool som skickar e-post till personer eller avisering strängar till URL: en slutpunkter när den elastiska poolen träffar ett tröskelvärde för användning som du skapat.

**Lägga till en avisering till en resurs:**

1. Klicka på den **resursutnyttjande** diagrammet för att öppna den **mått** bladet klickar du på **Lägg till avisering**, och fyller sedan informationen i den **lägga till en varningsregel** bladet (**resurs** har angetts automatiskt upp till att du arbetar med poolen).
2. Ange en **namn** och **beskrivning** som identifierar aviseringen du och mottagare.
3. Välj en **mått** som du vill Varna från listan.

    Diagrammet visar dynamiskt resursanvändningen för den måtten för att välja ett tröskelvärde.

4. Välj en **villkoret** (större än mindre än osv) och en **tröskelvärdet**.
5. Välj en **Period** som mått regeln måste uppfyllas innan aviseringen utlösare.
6. Klicka på **OK**.

Mer information finns i [skapa SQL-databas aviseringar i Azure-portalen](sql-database-insights-alerts-portal.md).

## <a name="move-a-database-into-an-elastic-pool"></a>Flytta en databas till en elastisk pool

Du kan lägga till eller ta bort databaser från en befintlig adresspool. Databaserna kan vara i andra pooler. Du kan bara lägga till databaser som finns på samma logiska server.

1. I bladet för i poolen, under **elastiska databaser** klickar du på **konfigurera pool**.

    ![Klicka på Konfigurera pool][1]

2. I den **konfigurera pool** bladet, klickar du på **lägger till poolen**.

    ![Klicka på Lägg till pool](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. I den **lägga till databaser** bladet Välj databasen eller databaser att lägga till i poolen. Klicka på **Välj**.

    ![Välj databaser att lägga till](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    Den **konfigurera pool** bladet visas nu i databasen som du har valt för att läggas till, med statusen **väntande**.

    ![Väntande pool-tillägg](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. I den **konfigurera pool bladet**, klickar du på **spara**.

    ![Klicka på Spara](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Flytta en databas från en elastisk pool

1. I den **konfigurera pool** bladet Välj databasen eller databaser att ta bort.

    ![Visar en lista över databaser](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Klicka på **ta bort**.

    ![Visar en lista över databaser](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    Den **konfigurera pool** bladet visas nu i databasen som du har valt att tas bort med statusen **väntande**.

    ![Förhandsgranska databasen tillägg och borttagning](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. I den **konfigurera pool bladet**, klickar du på **spara**.

    ![Klicka på Spara](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-an-elastic-pool"></a>Ändra inställningar för prestanda för en elastisk pool

När du övervakar resursanvändningen för en elastisk pool kan du märka att några justeringar behövs. Poolen måste kanske en ändring i gränser prestanda eller lagring. Kanske vill du ändra databasinställningarna i poolen. Du kan ändra inställningarna för poolen när som helst för att hämta den bästa balansen mellan prestanda och kostnader. Se [när en elastisk pool kan användas?](sql-database-elastic-pool.md) för mer information.

Begränsar per pool och edtu: er per databas om du vill ändra edtu: er eller lagring:

1. Öppna den **konfigurera pool** bladet.

    Under **elastisk poolinställningar**, ändra poolinställningarna för med hjälp av antingen skjutreglaget.

    ![Resursutnyttjande elastisk pool](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. När inställningen ändras, visas den månatliga uppskattade kostnaden för ändringen.

    ![Uppdatera en elastisk pool och nya månadskostnaden](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="latency-of-elastic-pool-operations"></a>Svarstiden för åtgärder elastisk pool
* Ändra min edtu: er per databas eller max edtu: er per databas vanligtvis har slutförts i 5 minuter eller mindre.
* Ändra edtu: er per pool beror på den totala mängden diskutrymme som används av alla databaser i poolen. Ändringarna tar i genomsnitt 90 minuter eller mindre per 100 GB. Om det totala utrymmet som används av alla databaser i poolen är exempelvis 200 GB och den förväntade svarstiden för att ändra pool-eDTU per pool är 3 timmar eller mindre.

## <a name="next-steps"></a>Nästa steg

- För att förstå vad en elastisk pool är, se [SQL Database-elastisk pool](sql-database-elastic-pool.md).
- Anvisningar om hur du använder elastiska pooler finns [pris- och prestandaöverväganden för elastiska pooler](sql-database-elastic-pool.md).
- Om du vill använda elastiska jobb för att köra Transact-SQL-skript mot valfritt antal databaser i poolen, se [elastiska jobb översikt](sql-database-elastic-jobs-overview.md).
- Om du vill fråga på valfritt antal databaser i poolen, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).
- Transaktioner valfritt antal databaser i poolen, finns i [elastisk transaktioner](sql-database-elastic-transactions-overview.md).


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
