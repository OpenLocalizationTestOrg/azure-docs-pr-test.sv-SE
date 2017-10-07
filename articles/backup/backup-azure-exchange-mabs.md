---
title: "aaaBack in en säkerhetskopiering med Azure Backup Server med Exchange server tooAzure | Microsoft Docs"
description: "Lär dig hur tooback upp en Exchange server-tooAzure säkerhetskopiera med hjälp av Azure Backup Server"
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
ms.assetid: e46557e8-2eaf-4ee0-99ea-00fbb8687dca
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: db874161151fc57c5b79c41531e18d577f567f66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-an-exchange-server-tooazure-backup-with-azure-backup-server"></a>Säkerhetskopiera en Exchange server-tooAzure säkerhetskopiering med Azure Backup Server
Den här artikeln beskriver hur tooconfigure Microsoft Azure Backup Server (MABS) tooback upp en Microsoft Exchange server-tooAzure.  

## <a name="prerequisites"></a>Krav
Innan du fortsätter, kontrollera att Azure Backup Server [installerad och förberedda](backup-azure-microsoft-azure-backup.md).

## <a name="mabs-protection-agent"></a>MABS skyddsagenten
Hej tooinstall MABS skyddsagenten på hello Exchange-servern, Följ dessa steg:

1. Kontrollera att hello brandväggar är korrekt konfigurerade. Se [konfigurera brandväggsundantag för agenten hello](https://technet.microsoft.com/library/Hh758204.aspx).
2. Installera hello-agenten på hello Exchange-servern genom att klicka på **Management > agenter > installera** i MABS-administratörskonsolen. Se [installera hello MABS skyddsagenten](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) detaljerade anvisningar.

## <a name="create-a-protection-group-for-hello-exchange-server"></a>Skapa en skyddsgrupp för hello Exchange-server
1. Hello MABS administratörskonsolen, klicka på **skydd**, och klicka sedan på **ny** på hello verktyget menyfliksområdet tooopen hello **Skapa ny Skyddsgrupp** guiden.
2. På hello **Välkommen** skärmen hello guiden Klicka **nästa**.
3. På hello **Välj typ av skyddsgrupp** , väljer **servrar** och på **nästa**.
4. Välj hello Exchange server-databas som du vill använda tooprotect och klicka på **nästa**.

   > [!NOTE]
   > Om du skyddar Exchange 2013 Kontrollera hello [krav för Exchange 2013](https://technet.microsoft.com/library/dn751029.aspx).
   >
   >

    I följande exempel hello, har hello Exchange 2010-databas valts.

    ![Välj gruppmedlemmar](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. Välj dataskyddsmetod hello.

    Namnet hello skyddsgrupp och välj sedan båda hello följande alternativ:

   * Jag vill ha kortvarigt skydd med Disk.
   * Jag vill använda onlineskydd.
6. Klicka på **Nästa**.
7. Välj hello **kör Eseutil toocheck dataintegritet** om du vill toocheck hello integriteten hos hello Exchange Server-databaser.

    När du har valt det här alternativet säkerhetskopiering konsekvenskontroll kommer att köras på MABS tooavoid hello i/o-trafik som genereras genom att köra hello **eseutil** på hello Exchange server.

   > [!NOTE]
   > toouse detta alternativ, måste du kopiera hello Ese.dll och Eseutil.exe filer toohello C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin katalog på hello MAB-servern. Annars utlöses hello följande fel:  
   > ![Eseutil-fel](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. Klicka på **Nästa**.
9. Välj hello databasen för **kopiering av säkerhetskopia**, och klicka sedan på **nästa**.

   > [!NOTE]
   > Om du inte väljer ”fullständiga säkerhetskopieringen” för minst en DAG kopia av en databas trunkeras inte loggar.
   >
   >
10. Konfigurera hello mål för **kortsiktig säkerhetskopiering**, och klicka sedan på **nästa**.
11. Granska hello tillgängligt diskutrymme och klicka sedan på **nästa**.
12. Välj hello tid vid vilken hello MAB Server skapar hello inledande replikering, och klicka sedan på **nästa**.
13. Välj alternativ för konsekvenskontroll hello och klicka sedan på **nästa**.
14. Välj hello-databas som du vill tooback in tooAzure och klicka sedan på **nästa**. Exempel:

    ![Ange onlineskyddsdata](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. Definiera hello schema för **Azure Backup**, och klicka sedan på **nästa**. Exempel:

    ![Ange onlinesäkerhetskopieringsschema](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Observera onlineåterställningspunkter baseras på snabb och fullständig återställningspunkter. Därför måste du schemalägga hello onlineåterställningspunkt stund hello som har angetts för hello express fullständig återställningspunkt.
    >
    >
16. Konfigurera hello bevarandeprincipen för **Azure Backup**, och klicka sedan på **nästa**.
17. Välj ett alternativ för onlinereplikeringsmetoden och klicka på **nästa**.

    Det kan ta lång tid för hello första säkerhetskopiering toobe skapade hello nätverket om du har en stor databas. tooavoid problemet bör du skapa en offlinesäkerhetskopiering.  

    ![Ange bevarandeprincip](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Bekräfta inställningarna för hello och klicka sedan på **Skapa grupp**.
19. Klicka på **Stäng**.

## <a name="recover-hello-exchange-database"></a>Återställa hello Exchange-databas
1. toorecover Exchange-databaser, klicka på **Recovery** i hello MABS-administratörskonsolen.
2. Leta upp hello Exchange-databas som du vill toorecover.
3. Välj en onlineåterställningspunkt hello *återställningstid* listrutan.
4. Klicka på **återställa** toostart hello **Återställningsguiden**.

Onlineåterställningspunkter finns det fem typer av återställning:

* **Återställa toooriginal Exchange-serverplatsen:** hello data kommer att återställda toohello ursprungliga Exchange-servern.
* **Återställa tooanother databas på en Exchange Server:** hello data kommer att återställda tooanother databas på en annan Exchange server.
* **Återställa tooa Återställningsdatabas:** hello data kommer att återställda tooan Exchange Recovery databasen (RDB).
* **Kopiera tooa nätverksmapp:** hello data kommer att återställda tooa nätverksmapp.
* **Kopiera tootape:** om du har ett bandbibliotek eller en fristående bandenhet som är anslutna och konfigurerat på MABS hello återställningspunkt kommer att kopieras tooa ledigt band.

    ![Välj online replikering](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Nästa steg
* [Azure Backup vanliga frågor och svar](backup-azure-backup-faq.md)
