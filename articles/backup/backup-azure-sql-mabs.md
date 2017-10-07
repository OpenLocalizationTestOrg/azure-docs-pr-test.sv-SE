---
title: "aaaAzure säkerhetskopiering för SQL Server-arbetsbelastningar med Azure Backup Server | Microsoft Docs"
description: En introduktion toobacking in SQL Server-databaser med Azure Backup Server
services: backup
documentationcenter: 
author: pvrk
manager: Shivamg
editor: 
ms.assetid: c8b1f7ec-26b1-4ef0-a3f2-91aec959daea
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 3a94338e8aca3f9d8611a72bcd223397ffb96f3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-sql-server-tooazure-with-azure-backup-server"></a>Säkerhetskopiera SQL Server-tooAzure med Azure Backup Server
Den här artikeln leder dig genom hello konfigurationssteg för säkerhetskopiering av SQL Server-databaser med Microsoft Azure Backup Server (MABS).

hello hantering av säkerhetskopiering tooAzure för SQL Server-databasen och återställa från Azure omfattar tre steg:

1. Skapa en princip för säkerhetskopiering tooprotect SQL Server-databaser tooAzure.
2. Skapa säkerhetskopior på begäran tooAzure.
3. Återställa hello databas från Azure.

## <a name="before-you-start"></a>Innan du börjar
Innan du börjar bör du kontrollera att du har [installerad och förberedda hello Azure Backup Server](backup-azure-microsoft-azure-backup.md).

## <a name="create-a-backup-policy-tooprotect-sql-server-databases-tooazure"></a>Skapa en princip för säkerhetskopiering tooprotect SQL Server-databaser tooAzure
1. Klicka på hello Azure Backup Server UI, hello **skydd** arbetsytan.
2. Klicka på verktygsfliken hello **ny** toocreate en ny skyddsgrupp.

    ![Skapa en Skyddsgrupp](./media/backup-azure-backup-sql/protection-group.png)
3. MABS visar hello startskärmen med hello vägledning om hur du skapar en **Skyddsgrupp**. Klicka på **Nästa**.
4. Välj **servrar**.

    ![Välj typ av Skyddsgrupp - 'Servrar'](./media/backup-azure-backup-sql/pg-servers.png)
5. Expandera hello SQL Server-dator där hello databaser toobe säkerhetskopierades från finns. MABS visas olika datakällor som kan säkerhetskopieras från servern. Expandera hello **alla SQL-resurser** och välj hello-databaser (i det här fallet vi valt ReportServer$ MSDPM2012 och ReportServer$ MSDPM2012TempDB) toobe säkerhetskopieras. Klicka på **Nästa**.

    ![Välj SQL-databas](./media/backup-azure-backup-sql/pg-databases.png)
6. Ange ett namn för hello skyddsgrupp och välj hello **jag vill Onlineskydd** kryssrutan.

    ![Dataskyddsmetod - kortvarigt disk & Online Azure](./media/backup-azure-backup-sql/pg-name.png)
7. I hello **ange kortvariga mål** skärmen, inkludera hello nödvändiga indata toocreate säkerhetskopiering punkter toodisk.

    Här visas som **Kvarhållningsintervall** har angetts för*5 dagar*, **synkroniseringsfrekvensen** tooonce anges varje *15 minuter* som är hello frekvensen säkerhetskopia görs. **Snabb och fullständig säkerhetskopiering** har angetts för*8:00 P.M*.

    ![Kortsiktiga mål](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > Vid 20:00:00 (enligt toohello skärmen indata) en säkerhetskopiering skapas varje dag av överföra hello data som har ändrats från hello föregående dag 8:00 PM säkerhetskopieringspunkt. Den här processen kallas **snabb fullständig säkerhetskopiering**. Medan hello transaktionsloggar synkroniseras express var 15: e minut, om det finns en behovet toorecover hello databas vid 21:00:00 – hello skapas genom att spela upp hello loggar från hello senast fullständig säkerhetskopiering punkt (20: 00 i detta fall).
   >
   >

8. Klicka på **Nästa**

    MABS visar hello övergripande lagringsutrymme och hello potentiella utrymme diskanvändning.

    ![Diskallokering](./media/backup-azure-backup-sql/pg-storage.png)

    Som standard skapar MABS en volym per datakälla (SQL Server-databas) som används för hello första säkerhetskopiering. Med den här metoden begränsar hello LDM Logical Disk Manager () MABS skydd too300 datakällor (SQL Server-databaser). toowork runt denna begränsning, Välj hello **samplacera data i DPM-lagringspoolen**, alternativet. Om du använder det här alternativet använder MABS en enda volym för flera datakällor, vilket gör att MABS tooprotect in too2000 SQL-databaser.

    Om **Utöka automatiskt volymerna hello** alternativet har valts, MABS kan kontot för hello ökade säkerhetskopieringsvolymen när hello produktionsdata växer. Om **Utöka automatiskt volymerna hello** alternativet inte är markerat, MABS begränsar hello lagring för säkerhetskopiering används toohello datakällor i skyddsgruppen hello.
9. Administratörer ges hello valet att överföra den här första säkerhetskopiering manuellt (av nätverk) tooavoid bandbredd överbelastning eller hello nätverk. De kan också konfigurera hello tid vid vilken hello inledande överföring kan inträffa. Klicka på **Nästa**.

    ![Metoden för inledande replikering](./media/backup-azure-backup-sql/pg-manual.png)

    hello första säkerhetskopiering kräver överföring av hello hela datakällan (SQL Server-databas) från tooMABS för produktion-server (SQL Server-dator). Dessa data kan vara stora och hello-data som överförs över nätverket hello kan överstiga bandbredd. Därför kan administratörer välja tootransfer hello första säkerhetskopian: **manuellt** (med flyttbara media) tooavoid bandbredd överbelastning eller **automatiskt över hello nätverk** (vid en angiven tid).

    När hello första säkerhetskopieringen är klar, är hello resten av hello säkerhetskopior säkerhetskopior på hello första säkerhetskopiering. Inkrementella säkerhetskopieringar tenderar toobe liten och överföras lätt över hello nätverk.
10. Välj om du vill att hello konsekvenskontroll Kontrollera toorun och på **nästa**.

    ![Konsekvenskontroll](./media/backup-azure-backup-sql/pg-consistent.png)

    MABS kan utföra en konsekvenskontroll Kontrollera toocheck hello integriteten hos hello säkerhetskopieringspunkt. Hello kontrollsumma för hello säkerhetskopian på produktionsservern hello (SQL Server-dator i det här scenariot) och hello säkerhetskopierade data för filen på MABS beräknas. En konflikt hello gäller förutsätts att hello säkerhetskopierade filen på MABS är skadad. MABS hejdar hello säkerhetskopierade data genom att skicka hello block motsvarande toohello felaktig matchning av kontrollsumma. Hello konsekvenskontroll är en igen prestandakrävande, har administratörer hello möjlighet att schemalägga hello konsekvenskontroll eller körs automatiskt.
11. toospecify online-skydd av hello datakällor, Välj hello databaser toobe skyddade tooAzure och klicka på **nästa**.

    ![Välj datakällor](./media/backup-azure-backup-sql/pg-sqldatabases.png)
12. Administratörer kan välja scheman för säkerhetskopiering och bevarandeprinciper som passar organisationens principer.

    ![Schemat och lagring](./media/backup-azure-backup-sql/pg-schedule.png)

    I det här exemplet tas säkerhetskopieringar en gång om dagen med 12:00 och 20: 00 (längst ned i hello-skärmen)

    > [!NOTE]
    > Det är en bra idé toohave några kortsiktig återställningspunkter på disk för snabb återställning. Dessa återställningspunkter används för ”operativa återställning”. Azure fungerar som en bra extern plats med högre SLA och garanteras tillgänglighet.
    >
    >

    **Bästa praxis**: Kontrollera att Azure-säkerhetskopieringar är schemalagda efter hello lokal disk-säkerhetskopiering med DPM. Detta gör att hello senaste disk säkerhetskopiering toobe kopieras tooAzure.

13. Välj hello bevarandeschemat princip. hello information om hur hello bevarandeprincip fungerar tillhandahålls på [Använd Azure Backup tooreplace band infrastruktur artikeln](backup-azure-backup-cloud-as-tape.md).

    ![Bevarandeprincip](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    I det här exemplet:

    * Säkerhetskopieringar tas en gång om dagen med 12:00 och 20: 00 (längst ned i hello-skärmen) och bevaras i 180 dagar.
    * hello säkerhetskopiering på lördag kl. 12:00 sparas i 104 veckor
    * hello säkerhetskopiering på sista lördag kl. 12:00 sparas i 60 månader
    * hello säkerhetskopiering på sista lördag mars klockan 12:00. sparas för 10 år
14. Klicka på **nästa** och välj hello lämpligt alternativ för att överföra hello första säkerhetskopiering tooAzure. Du kan välja **automatiskt över hello nätverk** eller **Offline säkerhetskopiering**.

    * **Automatiskt över hello nätverk** överföringar hello säkerhetskopieringsdata tooAzure enligt hello schemat för säkerhetskopiering.
    * Hur **Offline säkerhetskopiering** fungerar förklaras på [Offline säkerhetskopiering arbetsflöde i Azure Backup](backup-azure-backup-import-export.md).

    Välj hello relevanta överföring mekanism toosend hello första säkerhetskopiering tooAzure och klicka på **nästa**.
15. När du granska information om hello principen i hello **sammanfattning** klickar du på på hello **Skapa grupp** knappen toocomplete hello arbetsflöde. Du kan klicka på hello **Stäng** knappen och övervaka hello jobbförloppet i arbetsytan övervakning.

    ![Skapandet av Skyddsgruppen pågår](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>Säkerhetskopiering på begäran av en SQL Server-databas
Medan hello föregående steg skapat en princip för säkerhetskopiering, skapas en ”återställningspunkt” endast när hello första säkerhetskopia. I stället för väntar hello scheduler tookick i, peka hello stegen nedan utlösaren hello genereringen av en återställningspunkt manuellt.

1. Vänta tills hello skyddsgruppens status visas **OK** för hello databasen innan hello återställningspunkt skapas.

    ![Skyddsgruppmedlemmar](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Högerklicka på hello databasen och välj **Skapa återställningspunkt**.

    ![Skapa Onlineåterställningspunkt](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Välj **Onlineskydd** i hello nedrullningsbara menyn och klicka på **OK**. Detta startar hello skapar en återställningspunkt i Azure.

    ![Skapa återställningspunkt](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. Du kan visa hello jobbförloppet i hello **övervakning** arbetsyta där du hittar en pågående jobb som hello en beskrivs i nästa hello-bild.

    ![Övervakningskonsolen](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Återställa en SQL Server-databas från Azure
hello är följande steg nödvändiga toorecover en skyddad entitet (SQL Server-databas) från Azure.

1. Öppna hanteringskonsolen hello DPM-servern. Navigera för**Recovery** arbetsyta där du kan se hello servrar som säkerhetskopieras av DPM. Bläddra hello obligatorisk databas (i det här fallet ReportServer$ MSDPM2012). Välj en **återställning från** som slutar med **Online**.

    ![Välj återställningspunkt](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Högerklicka på hello databasens namn och klicka på **återställa**.

    ![Återställa från Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. DPM visar hello information hello återställningspunkt. Klicka på **Nästa**. toooverwrite hello databasen, väljer hello återställningstyp **Återställ toooriginal instans av SQL Server**. Klicka på **Nästa**.

    ![Återställa tooOriginal plats](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    I det här exemplet kan DPM återställning av hello databasen tooanother SQL Server-instans eller tooa fristående nätverksmapp.
4. I hello **Ange återställningsalternativ** skärmen, som du kan välja hello återställningsalternativ som bandbreddsbegränsningen toothrottle hello bandbredden för återställning. Klicka på **Nästa**.
5. I hello **sammanfattning** ser du alla hello konfigurationer som hittills. Klicka på **återställa**.

    Hej återställningsstatusen visar hello-databas som återställs. Du kan klicka på **Stäng** tooclose hello guiden och visa hello utvecklingen hello **övervakning** arbetsytan.

    ![Starta återställningsprocessen](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    När hello återställningen är klar är hello återställs databasen program som är konsekventa.

### <a name="next-steps"></a>Nästa steg:
• [Azure Backup vanliga frågor och svar](backup-azure-backup-faq.md)
