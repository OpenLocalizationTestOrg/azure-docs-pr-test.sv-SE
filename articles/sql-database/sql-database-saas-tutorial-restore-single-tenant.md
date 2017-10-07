---
title: "aaaRestore en Azure SQL-databas i en app för flera innehavare | Microsoft Docs"
description: "Lär dig hur toorestore en enda innehavare SQL database när data har tagits bort av misstag"
keywords: sql database tutorial
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: billgib;sstein
ms.openlocfilehash: 8507ecec2424c135f1859b88ebf2bb4e17538a58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-wingtip-saas-tenants-sql-database"></a>Återställa en Wingtip SaaS hyresgäster SQL-databas

hello Wingtip SaaS-appar skapas med en databas per klient-modell där varje innehavare har sin egen databas. En av hello fördelarna med den här modellen är att det är enkelt toorestore data för en enskild klient i isolering utan att påverka andra klienter.

Lär dig två mönster för återställning av data i den här självstudiekursen:

> [!div class="checklist"]

> * Återställa en databas till en parallell databasen (sida-vid-sida)
> * Återställa en databas på plats


|||
|:--|:--|
| **Återställningspunkt klient tooa tidigare tidpunkt, till en parallell databas** | Det här mönstret kan användas av hello klient för granskning, granskning, kompatibilitet, etc. hello ursprungliga databasen är online och oförändrade. |
| **Återställa klient på plats** | Det här mönstret är brukar användas toorecover en klient tooa tidigare punkt i tid, när en klient att av misstag tar bort data. hello originaldatabasen tas offline och ersätts med hello återställda databasen. |
|||

den här självstudiekursen, se till att hello följande krav är slutförda toocomplete:

* hello Wingtip SaaS appen har distribuerats. toodeploy på mindre än fem minuter finns [distribuera och utforska hello Wingtip SaaS-program](sql-database-saas-tutorial.md)
* Azure PowerShell ska ha installerats. Mer information finns i [Komma igång med Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-toohello-saas-tenant-restore-pattern"></a>Introduktion toohello SaaS klient återställning mönster

Återställa mönster för hello klient finns det två enkla mönster för att återställa en enskild klientorganisation data. Eftersom klienten databaser är isolerade från varandra, har återställa en klient ingen inverkan på data för alla andra innehavare.

Data har återställts till en ny databas i hello första mönstret. hello klient sedan får åtkomst toothis databasen tillsammans med deras produktionsdata. Det här mönstret gör en klient admin tooreview hello återställt data och använda den potentiellt tooselectively skriva över den aktuella värden. Är det upp toohello SaaS app designer toodetermine hur avancerade hello dataåterställning alternativ ska vara. Som bara kan tooreview data i hello tillstånd den hade vid en viss tidpunkt kan vara allt som krävs i vissa scenarier. Om hello databasen använder [georeplikering](sql-database-geo-replication-overview.md), rekommenderar vi att kopiera hello krävs data från hello återställts kopierar till hello originaldatabasen. Om du ersätter hello originaldatabasen med hello återställs databasen behöver tooreconfigure och synkronisera om geo-replikering.

I andra hello-mönster, vilket förutsätter att hello-klient har haft en förlust eller data skadas, återställs hello klient produktionsdatabasen tooa tidigare tidpunkt. Hej för återställning i plats-mönster tas hello klient offline under en kort tid när hello databasen återställs och online igen. hello originaldatabasen tas bort, men kan fortfarande återställa från om du behöver toogo tillbaka tooan även tidigare tidpunkt. En variant av det här mönstret kunde hello ändra databasens namn i stället för att ta bort den, även om namnbyte hello-databasen innehåller inga ytterligare fördelar vad gäller datasäkerheten.

## <a name="get-hello-wingtip-application-scripts"></a>Hämta hello Wingtip programskript

hello Wingtip SaaS-skript och programmets källkod är tillgängliga i hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-lagringsplatsen. [Steg toodownload hello Wingtip SaaS skript](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Simulera en klient oavsiktlig borttagning av data

toodemonstrate dessa återställningsscenarier måste för*av misstag* ta bort vissa data i någon av hello klient databaser. Du kan ta bort en post, hämta hello nästa steg ställer in hello demo toonot blockeras av referensintegritet överträdelser! Det ger också vissa biljett köp data som du kan använda senare i hello *Wingtip SaaS Analytics-självstudier*.

Kör hello biljett generator skript och skapa ytterligare data. hello biljett generator köpa avsiktligt inte biljetter för varje innehavare senaste händelse.

1. Öppna... \\Learning moduler\\verktyg\\*Demo-TicketGenerator.ps1* i hello *PowerShell ISE*
1. Tryck på **F5** toorun hello skript och generera kunder och biljett försäljningsinformation.


### <a name="open-hello-events-app-tooreview-hello-current-events"></a>Öppna hello händelser app tooreview hello aktuella händelser

1. Öppna hello *händelser hubb* (http://events.wtp.&lt; användaren&gt;. trafficmanager.net) och klicka på **Contoso samklang Hall**:

   ![evenemangshubben](media/sql-database-saas-tutorial-restore-single-tenant/events-hub.png)

1. Rulla hello lista över händelser och anteckna hello sista händelsen i hello lista:

   ![Senaste händelse](media/sql-database-saas-tutorial-restore-single-tenant/last-event.png)


### <a name="run-hello-demo-scenario-tooaccidentally-delete-hello-last-event"></a>Kör hello demo scenariot tooaccidentally delete hello senaste händelse

1. Öppna... \\Learning moduler\\affärskontinuitet och Haveriberedskap\\RestoreTenant\\*Demo-RestoreTenant.ps1* i hello *PowerShell ISE*, och ange hello följande värde:
   * **$DemoScenario** = **1**, Ställ in för**1** -ta bort händelser utan biljett försäljning.
1. Tryck på **F5** toorun hello skript och ta bort hello senaste händelse. Du bör se en bekräftelse meddelande liknande toohello följande:

   ```Console
   Deleting unsold events from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

1. hello Contoso händelser sidan öppnas. Bläddra nedåt och verifiera hello händelsen är borta. Om hello händelsen är fortfarande i hello-listan, klicka på Uppdatera och kontrollera att det är borta.

   ![Senaste händelse](media/sql-database-saas-tutorial-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-hello-production-database"></a>Återställa en databas klient parallellt med hello produktionsdatabasen

Den här övningen återställer hello Contoso samklang Hall databasen tooa punkt i tiden innan hello händelsen har tagits bort. När hello händelsen har tagits bort i hello föregående steg, vill toorecover och se hello bort data. Du behöver inte toorestore produktionsdatabasen med hello bort posten, men du måste toorecover hello gamla tooaccess hello gamla data från databasen för andra affärsskäl.

 Hej *återställning TenantInParallel.ps1* skriptet skapar en parallell klient-databasen och en parallell katalogpost både med namnet *ContosoConcertHall\_gamla*. Det här mönstret av återställning passar bäst för att återställa från en mindre dataförlust eller för efterlevnad och granskning återställningsscenarier. Det är också hello rekommendationer när du använder [georeplikering](sql-database-geo-replication-overview.md).

1. Fullständig hello [simulera en användare av misstag tar bort data](#simulate-a-tenant-accidentally-deleting-data) avsnitt.
1. Öppna... \\Learning moduler\\affärskontinuitet och Haveriberedskap\\RestoreTenant\\_Demo-RestoreTenant.ps1_ i hello *PowerShell ISE*.
1. Ange **$DemoScenario** = **2**, ställa in detta också**2** för*återställning klient parallellt*.
1. Tryck på **F5** toorun hello skript.

hello skript återställer hello klient database (tooa parallella databas) tooa punkt i tiden innan du tog bort hello händelse i hello föregående avsnitt. Den skapar en andra databas, tar bort alla befintliga katalogen metadata som finns i den här databasen, och lägger till hello databasen toohello katalogen under hello *ContosoConcertHall\_gamla* post.

Hej demoskript öppnar hello händelser sida i webbläsaren. Observera från hello URL: ```http://events.wtp.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` att detta visar data från hello återställs databasen där *_old* läggs toohello namn.

Rulla hello händelserna som anges i hello webbläsare tooconfirm som hello händelse bort hello föregående avsnitt har återställts.

Observera den medför hello återställts klientorganisationen som en ytterligare klient med egna händelser app toobrowse biljetter är inte troligt toobe hur skulle du anger en klient åtkomst toorestored data, men fungerar tooeasily illustrera hello återställning mönster.

I verkligheten, skulle du förmodligen bara behålla den här återställda databasen för en definierad period. Du kan ta bort hello återställts klient post när du är klar genom att anropa hello *ta bort RestoredTenant.ps1* skript.

1. Ange **$DemoScenario** för**4** tooselect hello *ta bort återställts klient* scenario.
1. **Köra** **med** **F5**
1. Hej *ContosoConcertHall\_gamla* post tas bort nu hello-katalogen. Gå vidare och Stäng hello händelser sidan för den här klienten i webbläsaren.


## <a name="restore-a-tenant-in-place-replacing-hello-existing-tenant-database"></a>Återställa en klient på plats, ersätter hello befintlig klient-databas

Den här övningen återställer hello Contoso samklang Hall klient tooa punkt i tiden innan hello händelsen har tagits bort. Hej *återställning TenantInPlace* skript återställer hello aktuella innehavaren tooa nya databasen pekar tooa tidigare punkt i tiden och borttagningar hello originaldatabasen. Det här mönstret av återställning passar bäst för återställning från allvarliga datafel eftersom det kan finnas dataförluster som hello klient skulle ha tooaccommodate.

1. Öppna **Demo-RestoreTenant.ps1** filen i PowerShell ISE
1. Ange **$DemoScenario** för**5** tooselect hello *återställa klient i plats scenariot*.
1. Köra med **F5**.

hello skript återställer hello klient databasen tooa punkt fem minuter innan hello händelsen har tagits bort. Den gör detta genom att först tar hello Contoso samklang Hall klient offline så att det finns ingen ytterligare uppdateringar toohello data. Sedan en parallell databas genom att återställa från hello återställningspunkt som namnges med en tidsstämpel tooensure hello databasnamnet inte står i konflikt med hello befintlig klient-databasnamn. Sedan hello gamla klient databasen tas bort och hello återställda databasen har bytt namn till toohello ursprungliga databasens namn. Slutligen tas Contoso samklang Hall online tooallow hello app åtkomst toohello återställa databasen.

Du återställt har hello databasen tooa punkt i tiden innan hello händelsen har tagits bort. Hej händelser sidan öppnas, så kontrollera hello senaste händelse har återställts.


## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]

> * Återställa en databas till en parallell databasen (sida-vid-sida)
> * Återställa en databas på plats

[Hantera databasschemat för klient](sql-database-saas-tutorial-schema-management.md)

## <a name="additional-resources"></a>Ytterligare resurser

* Ytterligare [självstudier som bygger på hello Wingtip SaaS-program](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Översikt över verksamhetskontinuitet med Azure SQL Database](sql-database-business-continuity.md)
* [Lär dig mer om säkerhetskopiering av SQL-databaser](sql-database-automated-backups.md)
