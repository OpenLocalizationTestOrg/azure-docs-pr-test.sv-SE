---
title: 'Azure-portalen: skapa och hantera en SQL Database-elastisk pool | Microsoft Docs'
description: "Lär dig hur toouse hello Azure-portalen och SQL-databas inbyggd intelligens toomanage, övervaka och rätt storlek en skalbar elastisk pool toooptimize databasens prestanda och hantera kostnaden."
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
ms.openlocfilehash: e0de952bc0c91177f64c04363630783d72435741
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-an-elastic-pool-with-hello-azure-portal"></a>Skapa och hantera en elastisk pool med hello Azure-portalen
Det här avsnittet beskrivs hur du toocreate och hantera skalbara [elastiska pooler](sql-database-elastic-pool.md) med hello Azure-portalen. Du kan också skapa och hantera en Azure elastisk pool med [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API eller [C#](sql-database-elastic-pool-manage-csharp.md). Du kan också skapa och flytta databaser till och från elastiska pooler med [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).

## <a name="create-an-elastic-pool"></a>Skapa en elastisk pool 

Det finns två sätt som du kan skapa en elastisk pool. Du kan göra det från grunden om du vet hello pool-installationen du vill eller börja med en rekommendation från hello-tjänsten. SQL Database har inbyggd intelligens som rekommenderar en elastisk pool-installationen om det är mer kostnadseffektivt baserat på hello senaste användningstelemetrin för dina databaser.

Du kan skapa flera pooler på en server, men du kan inte lägga till databaser från olika servrar i hello samma pool. 

> [!NOTE]
> Elastiska pooler är allmänt tillgängliga (GA) i alla Azure-regioner utom Västra Indien där de genomgår förhandsgranskning.  GA för elastiska pooler i den här regionen inträffar så snart som möjligt.
>

### <a name="step-1-create-an-elastic-pool"></a>Steg 1: Skapa en elastisk pool

Skapa en elastisk pool från en befintlig **server** bladet i hello portal är hello enklaste sättet toomove befintliga databaser till en elastisk pool.

> [!NOTE]
> Du kan också skapa en elastisk pool genom att söka **elastiska SQL-poolen** i hello **Marketplace** eller klicka på **+ Lägg till** i hello **SQL elastiska pooler**Bläddra bladet. Du kan kan toospecify en ny eller befintlig server via den här poolen etablering av arbetsflöde.
>
>

1. I hello [Azure-portalen](http://portal.azure.com/), klickar du på **fler tjänster**  **>**  **SQL-servrar**, och klicka sedan på hello-server som innehåller hello databaser som du vill tooadd tooan elastisk pool.
2. Klicka på **Ny pool**.

    ![Lägg till pool tooa server](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    **-ELLER-**

    Du kan se ett meddelande om att det finns rekommenderade elastiska pooler för hello-servern. Klicka på hello meddelandet toosee hello rekommenderade poolerna baserat på historisk telemetri, och på hello nivå toosee mer information och anpassa hello poolen. Se [förstå poolrekommendationer](#understand-elastic-pool-recommendations) senare i det här avsnittet för hur hello rekommendation.

    ![rekommenderad pool](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. Hej **elastisk pool** bladet visas, som är där du anger hello inställningarna för din pool. Om du klickade på **ny pool** i föregående steg hello hello prisnivån är **Standard** som standard och inga databaser har valts. Du kan skapa en tom pool eller ange en uppsättning befintliga databaser från denna server toomove till hello pool. Om du skapar en rekommenderad pool hello rekommenderas prisnivån, prestandainställningar för och listan över databaser är förinställd, men du kan ändra dem.

    ![Konfigurera elastisk pool](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. Ange ett namn för hello elastisk pool eller lämna den som standard hello.

### <a name="step-2-choose-a-pricing-tier"></a>Steg 2: Välj prisnivå.

hello avgör prisnivån för poolen hello funktioner tillgängliga toohello elastics i hello poolen och hello maximala antalet edtu: er (eDTU MAX) och lagringsutrymme (GB) tillgängliga tooeach databas. Mer information finns i tjänstnivåer.

toochange hello prisnivå för poolen hello, klickar du på **prisnivå**, klicka på hello prisnivån och klickar sedan på **Välj**.

> [!IMPORTANT]
> När du väljer hello prisnivån och genomför ändringarna genom att klicka på **OK** hello sista steget, inte kan toochange hello prisnivån för hello pool. toochange hello prisnivå för en befintlig elastisk pool, skapa en elastisk pool i hello önskade prisnivån och migrera hello databaser toothis ny pool.
>

![Välj en prisnivå](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

### <a name="step-3-configure-hello-pool"></a>Steg 3: Konfigurera hello pool

Klicka på Konfigurera pool när du har angett hello prisnivån, där du lägger till databaser, ange pool-edtu: er och lagring (pool-GB) och när du anger hello min och max edtu: er för hello elastics i hello pool.

1. Klicka på **Konfigurera pool**
2. Välj hello-databaser som du vill tooadd toohello pool. Det här steget är valfritt medan du skapar hello poolen. Databaser kan läggas till efter hello poolen har skapats.
    tooadd databaserna klickar du på **Lägg till databas**, klickar du på hello databaser du vill tooadd och klicka sedan på hello **Välj** knappen.

    ![Lägg till databaser](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    Om hello databaser du arbetar med har tillräckligt med historisk användningstelemetri, hello **beräknad eDTU och GB-användning** diagram och hello **faktisk eDTU-användning** stapeldiagram uppdatering toohelp du göra konfiguration beslut. Dessutom du hello-tjänsten kan ge en rekommendation meddelandet toohelp du rätt storlek hello poolen. Se [Dynamiska rekommendationer](#understand-elastic-pool-recommendations).

3. Använda hello kontroller i hello **konfigurera pool** sidan tooexplore inställningar och konfigurera din pool. Se [begränsningar för elastiska pooler](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) för mer information om begränsningarna för varje tjänstnivå och se [pris- och prestandaöverväganden för elastiska pooler](sql-database-elastic-pool.md) för detaljerad vägledning om rätt storlek för en elastisk pool. Mer information om poolinställningar finns [elastisk pool egenskaper](sql-database-elastic-pool.md#database-properties-for-pooled-databases).

    ![Konfigurera elastisk pool](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. Klicka på **Välj** i hello **konfigurera Pool** bladet när du har ändrat inställningarna.
5. Klicka på **OK** toocreate hello poolen.

## <a name="understand-elastic-pool-recommendations"></a>Förstå rekommendationer för elastiska pooler

hello SQL Database-tjänsten utvärderar Användningshistorik och rekommenderar en eller flera pooler när det är mer kostnadseffektivt än att använda enskilda databaser. Varje rekommendation är konfigurerad med en unik delmängd av hello server-databaser som bäst passar hello poolen.

![rekommenderad pool](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

hello pool-rekommendationerna omfattar:

- En prisnivå för hello poolen (Basic, Standard, Premium eller Premium RS)
- Lämpliga **POOL-eDTU:er** (kallas även Max eDTU:er per pool)
- Hej **eDTU MAX** och **eDTU Min** per databas
- hello listan över rekommenderade databaser för hello-pool

> [!IMPORTANT]
> hello service beaktas hello senaste 30 dagarnas telemetri vid rekommendation av pooler. För en databas toobe betraktas som en kandidat för en elastisk pool, måste det finnas minst 7 dagar. Databaser som redan finns i en elastisk pool är inte rekommenderade kandidater för elastiska pooler.
>

hello tjänsten utvärderar resursbehov och kostnadseffektivitet av glidande hello enda databaser i varje tjänstnivå till pooler inom hello samma nivå. Alla standard-databaser på servern utvärderas exempelvis för hur de skulle passa in i en standard elastisk pool. Det innebär att hello-tjänsten inte göra rekommendationer mellan olika nivåer, till exempel flytta en Standard-databas till en Premium-pool.

När du lägger till databaser toohello pool genereras dynamiskt rekommendationer baserat på hello historisk användning av hello-databaser som du har valt. De här rekommendationerna som visas i hello eDTU och GB och i en rekommendations-banderoll överst hello i hello **konfigurera pool** bladet. De här rekommendationerna är avsedda tooassist som du skapar en elastisk pool optimerad för dina specifika databaser.

![dynamiska rekommendationer](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## <a name="manage-and-monitor-an-elastic-pool"></a>Hantera och övervaka en elastisk pool

Du kan använda hello Azure portal toomonitor och hantera en elastisk pool och hello-databaser i hello pool. Du kan övervaka hello-belastningen för en elastisk pool och hello databaser i poolen från hello-portalen. Du kan också göra en uppsättning ändringar tooyour elastisk pool och skicka alla ändringar på hello samma tid. De här förändringarna innefattar att lägga till eller ta bort databaser, ändra inställningarna elastisk pool eller ändra databasinställningarna.

hello följande bild visar ett exempel elastisk pool. hello-vyn innehåller:

*  Diagram för övervakning av resursanvändningen för både hello elastisk pool och hello databaser som ingår i hello pool.
*  Hej **konfigurera** pool knappen toomake ändras toohello elastisk pool.
*  Hej **Skapa databas** skapar du en databas och lägger till den toohello aktuella elastisk pool.
*  Elastiska jobb som hjälper dig hantera stort antal databaser genom att köra Transact-SQL-skript mot alla databaser i en lista.

![Pool-vy][2]

Du kan gå tooa viss poolen toosee dess resursutnyttjande. Hello poolen är som standard konfigurerade tooshow lagring och eDTU-användning för hello senaste timmen. hello diagrammet kan vara konfigurerade tooshow olika mätvärden via olika tidsfönster.

1. Välj en elastisk pool toowork med.
2. Under **elastisk Pool övervakning** är ett diagram med etiketten **resursutnyttjande**. Klicka på hello diagram.

    ![Övervakning av elastisk pool][3]

    Hej **mått** blad öppnas, visar en detaljerad vy av hello anges mätvärden via hello angivna tidsfönstret.   

    ![Bladet Mått][9]

### <a name="toocustomize-hello-chart-display"></a>toocustomize hello diagramvyn

Du kan redigera hello diagram och hello mått bladet toodisplay andra mått som CPU-procent, data IO-procent och loggen IO-procent som används.

1. På mått hello-bladet klickar du på **redigera**.

    ![Klicka på Redigera][6]

2. I hello **redigera diagram** bladet Välj ett tidsintervall (efter timme, dag, eller föregående vecka), eller klicka på **anpassade** tooselect i hello vara ett datum senaste två veckorna. Välj hello diagramtyp (stapel- eller rad), och välj sedan hello resurser toomonitor.

   > [!Note]
   > Endast mått med samma enhet som kan visas i hello hello diagram på hello samma tid. Till exempel om du väljer ”eDTU i procent” kan du bara välja andra mått med procentandel som hello enhet.
   >

    ![Klicka på Redigera](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

3. Klicka sedan på **OK**.

## <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Hantera och övervaka databaser i en elastisk pool

Enskilda databaser kan också övervakas för potentiella problem.

1. Under **elastisk databas övervakning**, det finns ett diagram som visar mått för fem databaser. Som standard hello diagrammet visar hello övre 5 databaser i poolen hello av genomsnittlig eDTU-användning i hello senaste timmen. Klicka på hello diagram.

    ![Övervakning av elastisk pool][4]

2. Hej **databasen resursutnyttjande** bladet visas. Detta ger en detaljerad vy av hello databasanvändningen i hello pool. Använder hello rutnät i hello nedre delen av hello-bladet, kan du välja alla databaser i hello poolen toodisplay användningen i hello diagram (upp too5 databaser). Du kan också anpassa hello mått och tid fönster som visas i diagrammet hello genom att klicka på **redigera diagram**.

    ![-Databasresursbladet användning][8]

### <a name="toocustomize-hello-view"></a>toocustomize hello vy

1. I hello **databasen resursutnyttjande** bladet, klickar du på **redigera diagram**.

    ![Klicka på Redigera diagram](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. I hello **redigera** diagram bladet väljer du ett tidsintervall (efter timme eller senaste 24 timmarna) eller klicka på **anpassade** tooselect en annan dag i hello efter två veckor toodisplay.

    ![Klicka på anpassad](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Klicka på hello **databaser genom att jämföra** listrutan tooselect olika mått toouse när databaser.

    ![Redigera hello diagram](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="tooselect-databases-toomonitor"></a>tooselect databaser toomonitor

I listan för hello databasen hello **databasen resursutnyttjande** bladet hittar du viss databaser genom att titta igenom hello sidorna i hello listan eller genom att skriva hello namnet på en databas. Använd hello kryssrutan tooselect hello-databasen.

![Sök efter databaser toomonitor][7]


## <a name="add-an-alert-tooan-elastic-pool-resource"></a>Lägg till en resurs för avisering tooan elastisk pool

Du kan lägga till regler tooan elastisk pool som skickar e-post toopeople eller varning strängar tooURL slutpunkter när hello elastisk pool träffar ett tröskelvärde för användning som du skapat.

**tooadd en avisering tooany resurs:**

1. Klicka på hello **resursutnyttjande** diagram tooopen hello **mått** bladet, klickar du på **Lägg till avisering**, och fyller sedan hello informationen i hello **lägga till en avisering regeln** bladet (**resurs** konfigureras automatiskt toobe hello pool du arbetar med).
2. Ange en **namn** och **beskrivning** som identifierar hello avisering tooyou och hello mottagare.
3. Välj en **mått** som du vill tooalert hello-listan.

    hello diagrammet visar dynamiskt resursanvändningen för den mått toohelp du väljer ett tröskelvärde.

4. Välj en **villkoret** (större än mindre än osv) och en **tröskelvärdet**.
5. Välj en **Period** som hello mått regeln måste uppfyllas innan hello avisering utlösare.
6. Klicka på **OK**.

Mer information finns i [skapa SQL-databas aviseringar i Azure-portalen](sql-database-insights-alerts-portal.md).

## <a name="move-a-database-into-an-elastic-pool"></a>Flytta en databas till en elastisk pool

Du kan lägga till eller ta bort databaser från en befintlig adresspool. hello databaser kan vara i andra pooler. Men du kan bara lägga till databaser som finns på hello samma logiska server.

1. Hello-bladet för hello poolen, under **elastiska databaser** klickar du på **konfigurera pool**.

    ![Klicka på Konfigurera pool][1]

2. I hello **konfigurera pool** bladet, klickar du på **lägga till toopool**.

    ![Klicka på Lägg till toopool](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. I hello **lägga till databaser** bladet, Välj hello databasen eller databaser tooadd toohello pool. Klicka på **Välj**.

    ![Välj databaser tooadd](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    Hej **konfigurera pool** bladet nu visar hello markerade toobe som lagts till med statusen för databasen**väntande**.

    ![Väntande pool-tillägg](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. I hello **konfigurera pool bladet**, klickar du på **spara**.

    ![Klicka på Spara](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Flytta en databas från en elastisk pool

1. I hello **konfigurera pool** bladet, Välj hello databasen eller databaser tooremove.

    ![Visar en lista över databaser](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Klicka på **ta bort**.

    ![Visar en lista över databaser](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    Hej **konfigurera pool** bladet nu visar hello markerade toobe bort med statusen för databasen**väntande**.

    ![Förhandsgranska databasen tillägg och borttagning](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. I hello **konfigurera pool bladet**, klickar du på **spara**.

    ![Klicka på Spara](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-an-elastic-pool"></a>Ändra inställningar för prestanda för en elastisk pool

När du övervakar hello resursanvändningen för en elastisk pool kan du märka att några justeringar behövs. Hello poolen behöver kanske en ändring i hello prestanda eller lagring gränser. Du vill eventuellt toochange hello databasinställningarna i hello pool. Du kan ändra hello installationen av hello poolen på varje gång tooget hello bästa balansen mellan prestanda och kostnad. Se [när en elastisk pool kan användas?](sql-database-elastic-pool.md) för mer information.

toochange hello edtu: er eller lagring gränser per pool och edtu: er per databas:

1. Öppna hello **konfigurera pool** bladet.

    Under **elastisk poolinställningar**, använda antingen skjutreglaget toochange hello inställningar.

    ![Resursutnyttjande elastisk pool](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. När hello inställningen ändras, visar hello visa hello beräknade månadskostnaden för hello ändringen.

    ![Uppdatera en elastisk pool och nya månadskostnaden](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="latency-of-elastic-pool-operations"></a>Svarstiden för åtgärder elastisk pool
* Ändra hello min edtu: er per databas eller max edtu: er per databas vanligtvis har slutförts i 5 minuter eller mindre.
* Ändra hello edtu: er per pool beror på hello totala mängden diskutrymme som används av alla databaser i hello pool. Ändringarna tar i genomsnitt 90 minuter eller mindre per 100 GB. Om hello totalt utrymme som används av alla databaser i poolen hello är exempelvis 200 GB, och sedan hello förväntade svarstid för att ändra hello pool-eDTU per pool är tre timmar eller mindre.

## <a name="next-steps"></a>Nästa steg

- toounderstand är, finns i en elastisk pool [SQL Database-elastisk pool](sql-database-elastic-pool.md).
- Anvisningar om hur du använder elastiska pooler finns [pris- och prestandaöverväganden för elastiska pooler](sql-database-elastic-pool.md).
- toouse elastiska jobb toorun Transact-SQL-skript mot valfritt antal databaser i hello pool, se [elastiska jobb översikt](sql-database-elastic-jobs-overview.md).
- tooquery över valfritt antal databaser i hello pool, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).
- Transaktioner valfritt antal databaser i hello pool finns [elastisk transaktioner](sql-database-elastic-transactions-overview.md).


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
