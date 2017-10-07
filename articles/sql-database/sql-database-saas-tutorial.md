---
title: "aaaDeploy och utforska hello flera innehavare Wingtip SaaS-program som använder Azure SQL Database | Microsoft Docs"
description: "Distribuera och utforska hello Wingtip SaaS flera innehavare-program, som visar SaaS mönster med Azure SQL Database."
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
ms.date: 07/26/2017
ms.author: sstein
ms.openlocfilehash: 7c528ee19472d3b8c7a285b2f86013945e8f4bf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-explore-a-multi-tenant-application-that-uses-azure-sql-database---wingtip-saas"></a>Distribuera och utforska ett program med flera innehavare som använder Azure SQL Database - Wingtip SaaS

I den här självstudiekursen, distribuera och utforska hello Wingtip SaaS-program. hello program använder en databas-per-klient, SaaS programmönster tooservice flera innehavare. hello program är utformad tooshowcase funktioner i Azure SQL Database som gör det lättare att aktivera SaaS-scenarier.

Fem minuter när du klickar på hello *distribuera tooAzure* knappen nedan, du har ett SaaS-program för flera innehavare, med hjälp av SQL-databas, upp och körs i hello moln. hello programmet distribueras med tre exempel klienter, var och en med sin egen databas alla distribuerade till en elastisk SQL-pool. hello appen är distribuerade tooyour Azure-prenumeration, vilket ger dig fullständig åtkomst tooexplore och arbeta med hello enskilda programkomponenter. hello programmet källa koden och hantering av skript är tillgängliga i hello WingtipSaaS GitHub-lagringsplatsen.


I den här guiden lär du dig:

> [!div class="checklist"]

> * Hur toodeploy hello Wingtip SaaS-program
> * Där tooget hello programmets källkod och av hanteringsskript
> * Om hello servrar, pooler och databaser som utgör hello app
> * Hur klienter mappas tootheir data med hello *katalog*
> * Hur tooprovision en ny klient
> * Hur toomonitor klient aktivitet i hello app

tooexplore användningsmönster för olika SaaS design och hantering, en [antal relaterade självstudier](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials) är tillgänglig som bygger på den här första distributionen. Fördjupa dig i hello tillhandahålls skript när du går igenom hello självstudier och granska hur hello olika SaaS mönster implementeras. Gå igenom hello skript i varje självstudiekursen toogain en djupare förståelse hur tooimplement hello många SQL-databas funktioner som förenklar utveckla SaaS-program.

## <a name="prerequisites"></a>Krav

den här självstudiekursen, se till att hello följande krav är slutförda toocomplete:

* Azure PowerShell ska ha installerats. Mer information finns i [Komma igång med Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="deploy-hello-wingtip-saas-application"></a>Distribuera hello Wingtip SaaS-program

Distribuera hello Wingtip SaaS app:

1. Klicka på hello **distribuera tooAzure** knappen öppnar hello Azure portal toohello Wingtip SaaS Distributionsmall. hello mallen kräver två parametervärden. ett namn för en ny resursgrupp och ett användarnamn som särskiljer den här distributionen från andra distributioner av hello Wingtip SaaS-app. Nästa steg i hello innehåller information för att ange dessa värden.

   Se till att toonote hello exakta värden som du använder eftersom du behöver tooenter dem till en konfiguration filen senare.

   <a href="http://aka.ms/deploywtpapp" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Ange parametervärden som krävs för distribution av hello:

    > [!IMPORTANT]
    > En del autentisering och brandväggar för servern har avsiktligt lämnats oskyddade i demonstrationssyfte. **Skapa en ny resursgrupp**, och Använd inte befintliga resursgrupper, servrar eller pooler. Använd inte det här programmet eller alla resurser som den skapar för produktion. Ta bort den här resursen gruppen när du är klar med hello programmet toostop relaterade fakturering.

    * **Resursgruppen** – Välj **Skapa nytt** och ger en **namn** för hello resursgrupp. Välj en **plats** hello nedrullningsbara listan.
    * **Användaren** -resurser kräver globalt unikt namn. tooensure unikhet varje gång du distribuerar programmet hello ange ett värde toodifferentiate resurser som du skapar från resurser som skapats av andra distribution av hello Wingtip program. Rekommenderar vi toouse en kort **användare** namn, till exempel din initialer plus ett tal (till exempel *bg1*), och sedan använda som hello resursgruppens namn (till exempel *wingtip bg1*). Den **användaren** parametern kan bara innehålla bokstäver, siffror och bindestreck (inga blanksteg). hello måste första och sista tecknet vara en bokstav eller en siffra (alla gemena rekommenderas).


1. **Distribuera programmet hello**.

    * Klicka på tooagree toohello villkor.
    * Klicka på **Köp**.

1. Övervaka Distributionsstatus genom att klicka på **meddelanden** (hello klockikonen höger om sökrutan hello). Distribuera hello Wingtip SaaS app tar cirka fem minuter.

   ![distributionen lyckades](media/sql-database-saas-tutorial/succeeded.png)

## <a name="download-and-unblock-hello-wingtip-saas-scripts"></a>Hämta och avblockera hello Wingtip SaaS-skript

Vid distribution av programmet hello hämta hello källa koden och hantering av skript.

> [!IMPORTANT]
> Körbara innehållet (skript, DLL-filer) kan blockeras av Windows när zip-filer laddas ned från en extern källa och extraheras. Extraherar hello skript från en zip-fil, gör när hello nedan toounblock hello ZIP-filen innan du extraherar. Detta säkerställer att hello skript tillåts toorun.

1. Bläddra för[github-repo-hello Wingtip SaaS](https://github.com/Microsoft/WingtipSaaS).
1. Klicka på **kloning eller hämta**.
1. Klicka på **hämta ZIP** och spara hello-filen.
1. Högerklicka på hello **WingtipSaaS master.zip** fil och markera **egenskaper**.
1. På hello **allmänna** väljer **avblockera**, och klicka på **tillämpa**.
1. Klicka på **OK**.
1. Extrahera hello-filer.

Skript finns i hello *... \\WingtipSaaS master\\Learning moduler* mapp.

## <a name="update-hello-configuration-file-for-this-deployment"></a>Uppdatera hello konfigurationsfilen för den här distributionen

Innan du kör alla skript, ange hello *resursgruppen* och *användare* värdena i **UserConfig.psm1**. Ange dessa variabler toohello värden som du anger under distributionen.

1. Öppna... \\Learning moduler\\*UserConfig.psm1* i hello *PowerShell ISE*
1. Uppdatera *ResourceGroupName* och *namn* med hello specifika värden för din distribution (på rader 10 och 11 endast).
1. Spara ändringarna för Hej!

Det här helt enkelt att hålla du från att ha tooupdate dessa distribution-specifika värden i alla skript.

## <a name="run-hello-application"></a>Kör programmet hello

hello app demonstreras handelsplatser, såsom konsertsalar jazz föreningar sport föreningar som värd för händelser. Skapas registrera som kunder (eller klienter) hello Wingtip plattform för ett enkelt sätt toolist händelser och sälja biljetter. Varje plats hämtar en anpassad web app toomanage och deras händelser och sälja biljetter oberoende och isoleras från andra klienter. Varje klient hämtar under hello försättsbladen, en SQL-databas som har distribuerats till en elastisk SQL-pool.

Central **händelser hubb** innehåller en lista över klient URL: er specifika tooyour distribution.

1. Öppna hello _händelser hubb_ i webbläsaren: http://events.wtp.&lt; ANVÄNDAREN&gt;. trafficmanager.net (Ersätt med användarnamnet för din distribution):

    ![evenemangshubben](media/sql-database-saas-tutorial/events-hub.png)

1. Klicka på **Fabrikam Jazzklubb** i *evenemangshubben*.

   ![Händelser](./media/sql-database-saas-tutorial/fabrikam.png)


toocontrol hello distribution av inkommande begäranden, hello app använder [ *Azure Traffic Manager*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview). hello händelser sidor som är specifika för klient kräver att klienten namn är inkluderade i hello URL: er. Alla hello klient URL: er omfattar ditt specifika *användare* värde och följande format: http://events.wtp.&lt; ANVÄNDAREN&gt;.trafficmanager.net/*fabrikamjazzclub*. hello händelser app Parsar hello klientnamn från hello URL och använder den toocreate en nyckel tooaccess en katalog med hjälp av [ *Fragmentera kartan management*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-scale-shard-map-management). hello katalog maps hello viktiga toohello klient databasplatsen. Hej **händelser hubb** använder utökade metadata i hello katalogen tooretrieve hello innehavarens namn som associeras med varje databas tooprovide hello URL-listan.

I en produktionsmiljö skulle du vanligtvis skapa en CNAME DNS-posten [ *peka företagets Internetdomän* ](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) till hello traffic manager-profil.

## <a name="start-generating-load-on-hello-tenant-databases"></a>Börja generera belastningen på hello klient databaser

Nu när hello appen har distribuerats kan vi lägger den toowork! Hej *Demo-LoadGenerator* PowerShell-skript startar en arbetsbelastning som körs mot alla klient-databaser. hello verkliga belastningen på många SaaS-appar är vanligtvis sporadiska och oförutsägbart. toosimulate den här typen av belastning, hello generator producerar en belastning fördelad över alla klienter med slumpmässig belastning på varje klient som uppstår vid slumpmässiga intervall. Därför det tar flera minuter för hello belastningen mönster tooemerge så är det bästa toolet hello generator kör på minst tre eller fyra minuter innan du övervakar hello läsa in.

1. I hello *PowerShell ISE*öppnar hello... \\Learning moduler\\verktyg\\*Demo-LoadGenerator.ps1* skript.
1. Tryck på **F5** kör hello skript och starta hello belastningen generator (lämna hello Standardparametervärden just nu).

> [!IMPORTANT]
> toorun andra skript, öppna ett nytt fönster i PowerShell ISE. hello belastningen generator körs som en serie av jobb i din lokala PowerShell-session. Hej *Demo-LoadGenerator.ps1* skript systemtillstånd aktiveras hello faktiska belastningen generator skript som körs som en aktivitet i förgrunden plus en serie belastningen generations bakgrundsjobb. Ett jobb belastningen generator anropas för varje databas som har registrerats i hello-katalogen. hello jobb körs i din lokala PowerShell-session, så att stänga hello PowerShell-sessionen avbryts alla jobb. Om du avbryter din dator, load generation pausas och återupptas när du aktiverar din dator.

När hello belastningen generator anropar belastningen generations jobb för varje klient, hello förgrunden aktiviteten finns kvar i ett jobb startar tillstånd där den startas ytterligare bakgrundsjobb för alla nya klienter som tillhandahålls senare. Du kan använda *Ctrl-C* eller tryck på hello *stoppa* knappen toostop hello förgrunden aktiviteten, men befintliga bakgrunden jobb kommer att fortsätta skapa belastningen på varje databas. Om du behöver toomonitor och styra hello bakgrundsjobb använda *Get-Job*, *Receive-Job* och *stoppa jobbet*. Medan hello förgrunden körs som du kan inte använda hello tooexecute med samma PowerShell-session andra skript. toorun andra skript, öppna ett nytt fönster i PowerShell ISE.

Om du vill toorestart hello belastningen generator sessionen, till exempel med andra parametrar du kan stoppa hello förgrunden aktivitet och sedan kör hello *Demo-LoadGenerator.ps1* skript. Köra *Demo-LoadGenerator.ps1* stoppar alla pågående jobb och omstarter hello generator som en ny uppsättning jobb med hjälp av hello aktuella parametrar av systemtillstånd aktiveras först.

Lämna hello belastningen generator körs i hello jobbet anropar tillstånd för tillfället.


## <a name="provision-a-new-tenant"></a>Etablera en ny klient

hello första distributionen skapas tre exempel hyresgäster, men nu ska vi skapa en annan klient toosee hur detta påverkar hello distribuerade program. hello etablering Wingtip SaaS-innehavare arbetsflödet beskrivs i hello [etablera och katalogen kursen](sql-database-saas-tutorial-provision-and-catalog.md). I det här steget kan skapa du snabbt en ny klient.

1. Öppna... \\Learning Modules\Provision och katalogen\\*Demo-ProvisionAndCatalog.ps1* i hello *PowerShell ISE*.
1. Tryck på **F5** att köra hello-skript (lämna hello standardvärdena för tillfället).

   > [!NOTE]
   > Många SaaS Wingtip skript använder *$PSScriptRoot* tooallow navigera mappar toocall funktioner i andra skript. Den här variabeln utvärderas bara när hello skriptet körs genom att trycka på **F5**.  Markera och kör en markering (**F8**) kan leda till fel, trycker du på **F5** när du kör skript.

hello ny klient databas skapas i en elastisk SQL-pool, initiera och registreras i hello-katalogen. Efter lyckad etableringen hello ny klient har biljett sälja *händelser* platsen visas i webbläsaren:

![Ny klient](./media/sql-database-saas-tutorial/red-maple-racing.png)

Uppdatera hello *händelser hubb* och hello ny klient visas nu i hello-listan.


## <a name="explore-hello-servers-pools-and-tenant-databases"></a>Utforska hello servrar, pooler, och klient databaser

Nu när du har startat körs en belastning mot hello samling av innehavare kan vi titta på några av hello-resurser som har distribuerats:

1. I den [Azure-portalen](http://portal.azure.com), bläddra tooyour lista över SQL-servrar och öppna den **katalog -&lt;användare&gt;**  server. hello katalogserver innehåller två databaserna. Hej **tenantcatalog**, och hello **basetenantdb** (en tom *gyllene* eller malldatabasen som kopieras toocreate nya klienter).

   ![databaser](./media/sql-database-saas-tutorial/databases.png)

1. Gå tillbaka tooyour lista över SQL-servrar och öppna hello **tenants1 -&lt;användare&gt;**  servern där hello klient databaser. Varje klient-databasen är en _elastisk Standard_ databas i en standard 50 eDTU-pool. Observera också att det finns en _Röd lönn tävling_ databasen, hello klient databasen som du tidigare har etablerats.

   ![server](./media/sql-database-saas-tutorial/server.png)

## <a name="monitor-hello-pool"></a>Övervakaren hello pool

Om hello belastningen generator har körts i flera minuter, ska tillräckligt med data vara tillgängliga toostart tittar på några av hello övervakningsfunktionerna som är inbyggda i pooler och databaser.

1. Bläddra toohello server **tenants1 -&lt;användare&gt;**, och klicka på **Pool1** att visa resursanvändningen för hello pool (hello belastningen generator kördes under en timme i följande diagram hello) :

   ![övervaka poolen](./media/sql-database-saas-tutorial/monitor-pool.png)

De här diagrammen visar hur väl lämpade elastiska pooler och SQL Database är för arbetsbelastningar från SaaS-program. Fyra databaser som varje burst-överföring tooas mycket som 40 edtu: er stöds enkelt i en 50 eDTU-pool. Om de har etablerats som fristående databaser, de skulle varje måste toobe en S2 (50 DTU) toosupport hello belastning. hello kostnaden för 4 fristående S2 databaser är nästan 3 gånger hello priset för hello pool och hello poolen har fortfarande tillräckligt med utrymme för många flera databaser. I verkliga situationer körs SQL Database-kunder för närvarande av too500 databaser i 200 eDTU pooler. Mer information finns i hello [prestandaövervakning kursen](sql-database-saas-tutorial-performance-monitoring.md).


## <a name="next-steps"></a>Nästa steg

I den här guiden har du lärt dig:

> [!div class="checklist"]

> * Hur toodeploy hello Wingtip SaaS-program
> * Om hello servrar, pooler och databaser som utgör hello app
> * Klienter som är mappade tootheir data med hello *katalog*
> * Hur tooprovision nya klienter
> * Hur tooview pool användning toomonitor klient aktivitet
> * Hur toodelete exempel resurser toostop relaterade fakturering

Prova nu hello [etablera och katalogen självstudiekursen](sql-database-saas-tutorial-provision-and-catalog.md)



## <a name="additional-resources"></a>Ytterligare resurser

* Ytterligare [självstudier som bygger på hello Wingtip SaaS-program](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* toolearn om elastiska pooler finns [ *vad är en Azure SQL-elastisk pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* toolearn om elastiska jobb finns [ *hantera utskalat moln databaser*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
* toolearn om flera innehavare SaaS-program, se [ *designmönster för SaaS-program med flera innehavare*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)
