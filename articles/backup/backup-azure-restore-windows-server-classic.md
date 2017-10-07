---
title: "aaaRestore data tooa Windows Server eller Windows-klienten från att använda Azure hello klassiska distributionsmodellen | Microsoft Docs"
description: "Lär dig hur toorestore från en Windows Server eller Windows-klient."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 85585dfc-c764-4e8c-8f0e-40b969640ac2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 4d1458d5233c4f55004ecfa95cbf7b3b18a03dde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-files-tooa-windows-server-or-windows-client-machine-using-hello-classic-deployment-model"></a>Återställ filer tooa Windows server eller Windows-klientdatorn med hjälp av hello klassiska distributionsmodellen
> [!div class="op_single_selector"]
> * [Klassisk portal](backup-azure-restore-windows-server-classic.md)
> * [Azure Portal](backup-azure-restore-windows-server.md)
>
>

Den här artikeln förklarar hur toorecover data från en säkerhetskopia valvet och återställer den tooa server eller dator. Med början i mars 2017 kan skapa du inte längre säkerhetskopieringsvalv i hello klassiska portalen.

> [!IMPORTANT]
> Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv. Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.<br/> **15 oktober 2017**, kommer du inte längre att kunna toouse PowerShell toocreate säkerhetskopieringsvalv. <br/> **Från den 1 november 2017**:
>- Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.
>- Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen. Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.
>

toorestore data använder du guiden för hello återställa Data i hello Microsoft Azure Recovery Services MARS-agenten. När du återställer data är det möjligt att:

* Återställ data toohello samma datorn från vilken hello säkerhetskopieringar som utförts.
* Återställa data tooan alternativa dator.

Microsoft släppt en Preview update toohello MARS-agenten i januari 2017. Tillsammans med felkorrigeringar, kan du använda snabbmeddelanden återställa, vilket gör att du toomount en skrivbar återställning punkt ögonblicksbild som en återställningspunkt-volym. Du kan sedan utforska hello recovery volym och kopiera filer tooa lokal dator vilket selektivt återställningen.

> [!NOTE]
> Hej [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) krävs om du vill toouse omedelbar återställning toorestore av data. Hello säkerhetskopierade data måste också skyddas i valv i språk som anges i hello support-artikeln. Kontakta hello [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) hello senaste lista över språk som har stöd för omedelbar återställning. Omedelbar återställning **inte** är tillgänglig i alla språk.
>

Omedelbar återställning är tillgänglig för användning i Recovery Services-valv i hello Azure-portalen och säkerhetskopieringsvalv i hello klassiska portalen. Om du vill toouse Instant återställa hämta hello MARS uppdateringen och följ hello procedurer som nämner omedelbar återställning.


## <a name="use-instant-restore-toorecover-data-toohello-same-machine"></a>Använda snabbmeddelanden återställa toorecover data toohello samma dator

Om du av misstag tar bort en fil- och vill toorestore den toohello samma dator (från vilka hello säkerhetskopiering tas), hello följande steg hjälper dig att återställa hello data.

1. Öppna hello **Microsoft Azure Backup** fästa i. Om du inte vet där hello snapin-modulen installerades, söka hello dator eller server för **Microsoft Azure Backup**.

    Hej skrivbordsapp ska visas i sökresultaten hello.

2. Klicka på **återställa Data** toostart hello guiden.

    ![Återställa Data](./media/backup-azure-restore-windows-server/recover.png)

3. På hello **komma igång** rutan, toorestore hello data toohello samma server eller dator, Välj **servern (`<server name>`)** och på **nästa**.

    ![Välj den här servern alternativet toorestore hello data toohello samma dator](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. På hello **Välj återställningsläge** fönstret Välj **enskilda filer och mappar** och klicka sedan på **nästa**.

    ![Bläddra bland filer](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. På hello **Välj volym och datum** rutan, Välj hello volymen som innehåller hello filer och/eller mappar som du vill toorestore.

    Välj en återställningspunkt hello kalendern. Du kan återställa från valfri återställningspunkt i tid. Datum i **fetstil** ange hello tillgängligheten för minst en återställningspunkt. När du har valt ett datum, om det finns flera återställningspunkter, Välj hello specifik återställningspunkt hello **tid** nedrullningsbara menyn.

    ![Volym och datum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. När du har valt hello recovery punkt toorestore, klickar du på **montera**.

    Azure-säkerhetskopiering monterar hello lokal återställningspunkt och använder den som en återställningspunkt-volym.

7. På hello **Bläddra och återställa filer** rutan klickar du på **Bläddra** tooopen Windows Explorer och hitta hello filer och mappar som du vill.

    ![Återställningsalternativ](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. I Utforskaren, kopiera hello filer och mappar du vill toorestore och klistra in dem tooany lokala toohello servern eller datorn. Du kan öppna eller strömma hello filer direkt från hello recovery volym och verifiera hello rätt versioner återställs.

    ![Kopiera och klistra in filer och mappar från monterad volym toolocal plats](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. När du är klar återställer hello filer och/eller mappar på hello **Bläddra och återställningsfiler** rutan klickar du på **koppla från**. Klicka på **Ja** tooconfirm som du vill toounmount hello volym.

    ![Demontera hello volym och bekräfta](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Om du inte klicka på Koppla från, förblir hello Recovery volym monterade under sex timmar från hello tid när den är monterad. Inga säkerhetskopieringsåtgärder kommer att köras när hello volym är monterad. Alla Säkerhetskopieringsåtgärden schemalagda toorun under hello tiden när hello volym är monterad, körs när hello recovery volymen är frånkopplade.
    >


## <a name="recover-data-toohello-same-machine"></a>Återställa data toohello samma dator
Om du av misstag tar bort en fil- och vill toorestore den toohello samma dator (från vilka hello säkerhetskopiering tas), hello följande steg hjälper dig att återställa hello data.

1. Öppna hello **Microsoft Azure Backup** fästa i.
2. Klicka på **återställa Data** tooinitiate hello arbetsflöde.

    ![Återställa Data](./media/backup-azure-restore-windows-server-classic/recover.png)
3. Välj hello  **servern (*yourmachinename*) ** alternativet toorestore hello säkerhetskopierade filen på hello samma dator.

    ![Samma dator](./media/backup-azure-restore-windows-server-classic/samemachine.png)
4. Välj för**Bläddra efter filer** eller **söka efter filer**.

    Lämna hello standardalternativet om du planerar toorestore en eller flera filer vars sökväg är känd. Om du inte är säker hello mappstrukturen men vill toosearch för en fil, Välj hello **söka efter filer** alternativet. Vi kommer att fortsätta med hello standardalternativet för hello syftet med det här avsnittet.

    ![Bläddra bland filer](./media/backup-azure-restore-windows-server-classic/browseandsearch.png)
5. Välj hello volym som du vill toorestore hello-filen.

    Du kan återställa från valfri punkt i tiden. Datum som visas i **fetstil** i hello kalenderkontroll indikera hello tillgängligheten för en återställningspunkt. När ett datum har valts baserat på schemat för säkerhetskopiering (och hello genomförandet av en säkerhetskopiering), kan du välja en punkt i tiden från hello **tid** listrutan.

    ![Volym och datum](./media/backup-azure-restore-windows-server-classic/volanddate.png)
6. Välj hello objekt toorecover. Du kan välja flera mappar/filer som du vill toorestore.

    ![Välj filer](./media/backup-azure-restore-windows-server-classic/selectfiles.png)
7. Ange hello recovery parametrar.

    ![Återställningsalternativ](./media/backup-azure-restore-windows-server-classic/recoveroptions.png)

   * Har du möjlighet att återställa toohello ursprungliga plats (filen/mappen skulle skrivas i vilka hello) eller tooanother plats i hello samma dator.
   * Om hello filen/mappen du vill toorestore finns i hello målplats, kan du skapa kopior (två versioner av hello samma fil), skriva över hello filer i hello målplatsen eller hoppa över hello återställning av hello-filer som finns i hello mål.
   * Vi rekommenderar starkt att du lämnar hello standardalternativet återställa hello ACL: er på hello-filer som är som återställs.
8. När dessa indata är tillgängliga, klickar du på **nästa**. hello recovery arbetsflöde, vilket återställer hello filer toothis datorn startas.

## <a name="recover-tooan-alternate-machine"></a>Återställa tooan alternativa dator
Du kan fortfarande återställa data från Azure Backup tooa annan dator om hela servern går förlorad. hello följande steg visar hello arbetsflöde.  

hello-terminologi som används i de här stegen innefattar:

* *Källdatorn* – hello ursprungliga datorn från vilken hello säkerhetskopian skapades och som inte är tillgänglig för tillfället.
* *Måldatorn* – hello datorn toowhich hello data återställs.
* *Exempel valvet* – hello Backup-valvet toowhich hello *källdatorn* och *måldatorn* registreras. <br/>

> [!NOTE]
> Säkerhetskopieringar som utförts från en dator kan inte återställas på en dator som kör en tidigare version av operativsystemet för hello. Till exempel om säkerhetskopiering utförs från en Windows 7-dator, kan den återställas på en Windows 8 eller senare datorn. Dock har hello vice versa inte värdet true.
>
>

1. Öppna hello **Microsoft Azure Backup** fästa i på hello *måldatorn*.
2. Se till att hello *måldatorn* och hello *källdatorn* är registrerade toohello samma säkerhetskopieringsvalvet.
3. Klicka på **återställa Data** tooinitiate hello arbetsflöde.

    ![Återställa Data](./media/backup-azure-restore-windows-server-classic/recover.png)
4. Välj **en annan server**

    ![En annan Server](./media/backup-azure-restore-windows-server-classic/anotherserver.png)
5. Ange hello valvautentiseringsfilen som motsvarar toohello *exempel valvet*. Ladda ned en ny valvautentiseringsfil från hello om hello valvautentiseringsfilen är ogiltig (eller har upphört att gälla) *exempel valvet* i hello klassiska Azure-portalen. När hello valvautentiseringsfilen tillhandahålls visas hello säkerhetskopieringsvalvet mot hello valvautentiseringsfilen.
6. Välj hello *källdatorn* hello listan med visade datorer.

    ![Lista över datorer](./media/backup-azure-restore-windows-server-classic/machinelist.png)
7. Välj antingen hello **söka efter filer** eller **Bläddra efter filer** alternativet. För hello syftet med det här avsnittet, kommer vi att använda hello **söka efter filer** alternativet.

    ![Söka](./media/backup-azure-restore-windows-server-classic/search.png)
8. Välj hello volym och datum hello nästa skärm. Sök hello mappen/filen namn du vill ha toorestore.

    ![Sökobjekt](./media/backup-azure-restore-windows-server-classic/searchitems.png)
9. Välj hello plats där hello filer måste toobe återställs.

    ![Återställa en plats](./media/backup-azure-restore-windows-server-classic/restorelocation.png)
10. Ange hello krypteringslösenfrasen som du angav under *källdatorns Målegenskaper* registrering för*exempel valvet*.

    ![Kryptering](./media/backup-azure-restore-windows-server-classic/encryption.png)
11. När hello indata anges, klickar du på **återställa**som utlösare hello återställning av hello säkerhetskopierade filer toohello mål som angavs.

## <a name="use-instant-restore-toorestore-data-tooan-alternate-machine"></a>Använda snabbmeddelanden återställa toorestore data tooan alternativa dator
Du kan fortfarande återställa data från Azure Backup tooa annan dator om hela servern går förlorad. hello följande steg visar hello arbetsflöde.

hello-terminologi som används i de här stegen innefattar:

* *Källdatorn* – hello ursprungliga datorn från vilken hello säkerhetskopian skapades och som inte är tillgänglig för tillfället.
* *Måldatorn* – hello datorn toowhich hello data återställs.
* *Exempel valvet* – hello Recovery Services-valvet toowhich hello *källdatorn* och *måldatorn* registreras. <br/>

> [!NOTE]
> Säkerhetskopieringar kan inte vara återställda tooa måldatorn som kör en tidigare version av operativsystemet för hello. Till exempel återställts en säkerhetskopia som gjorts från en Windows 7-dator kan vara på en Windows 8 eller senare, dator. En säkerhetskopia som gjorts från en Windows 8-dator kan inte vara återställda tooa Windows 7-dator.
>
>

1. Öppna hello **Microsoft Azure Backup** fästa i på hello *måldatorn*.

2. Se till att hello *måldatorn* och hello *källdatorn* är registrerade toohello samma Recovery Services-valvet.

3. Klicka på **återställa Data** tooopen hello **guiden återställa Data**.

    ![Återställa Data](./media/backup-azure-restore-windows-server/recover.png)

4. På hello **komma igång** väljer **en annan server**

    ![En annan Server](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Ange hello valvautentiseringsfilen som motsvarar toohello *exempel valvet*, och klicka på **nästa**.

    Om hello valvautentiseringsfilen är ogiltig (eller har upphört att gälla), ladda ned en ny valvautentiseringsfil från hello *exempel valvet* i hello Azure-portalen. När du har angett en giltig valvautentiseringen visas hello hello motsvarande Backup-valvet.

6. På hello **Välj Backup Server** rutan, Välj hello *källdatorn* hello listan över datorer som visas och ange hello lösenfras. Klicka sedan på **Nästa**.

    ![Lista över datorer](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. På hello **Välj återställningsläge** väljer **enskilda filer och mappar** och på **nästa**.

    ![Söka](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. På hello **Välj volym och datum** rutan, Välj hello volymen som innehåller hello filer och/eller mappar som du vill toorestore.

    Välj en återställningspunkt hello kalendern. Du kan återställa från valfri återställningspunkt i tid. Datum i **fetstil** ange hello tillgängligheten för minst en återställningspunkt. När du har valt ett datum, om det finns flera återställningspunkter, Välj hello specifik återställningspunkt hello **tid** nedrullningsbara menyn.

    ![Sökobjekt](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Klicka på **montera** toolocally hello recovery monteringspunkt som en återställning volym på din *måldatorn*.

10. På hello **Bläddra och återställa filer** rutan klickar du på **Bläddra** tooopen Windows Explorer och hitta hello filer och mappar som du vill.

    ![Kryptering](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. I Utforskaren, kopiera hello filer och mappar från hello recovery volym och klistra in tooyour *måldatorn* plats. Du kan öppna eller strömma hello filer direkt från hello recovery volym och verifiera hello rätt versioner återställs.

    ![Kryptering](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. När du är klar återställer hello filer och/eller mappar på hello **Bläddra och återställningsfiler** rutan klickar du på **koppla från**. Klicka på **Ja** tooconfirm som du vill toounmount hello volym.

    ![Kryptering](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Om du inte klicka på Koppla från, förblir hello Recovery volym monterade under sex timmar från hello tid när den är monterad. Inga säkerhetskopieringsåtgärder kommer att köras när hello volym är monterad. Alla Säkerhetskopieringsåtgärden schemalagda toorun under hello tiden när hello volym är monterad, körs när hello recovery volymen är frånkopplade.
    >


## <a name="next-steps"></a>Nästa steg
* [Azure Backup vanliga frågor och svar](backup-azure-backup-faq.md)
* Besök hello [Azure Backup-forumet](http://go.microsoft.com/fwlink/p/?LinkId=290933).

## <a name="learn-more"></a>Läs mer
* [Översikt över Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=222425)
* [Säkerhetskopiera Azure virtuella datorer](backup-azure-vms-introduction.md)
* [Säkerhetskopiera Microsoft-arbetsbelastningar](backup-azure-dpm-introduction.md)
