---
title: aaaRestore data i Azure tooa Windows Server eller Windows-dator | Microsoft Docs
description: "Lär dig hur toorestore data lagras i Azure tooa Windows Server eller Windows-dator."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 742f4b9e-c0ab-4eeb-8e22-ee29b83c22c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/16/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 11335495e448986a74f1733406f87e04331641d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-files-tooa-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a>Återställ filer tooa Windows server eller Windows-klientdatorn med hjälp av Resource Manager-distributionsmodellen
> [!div class="op_single_selector"]
> * [Azure Portal](backup-azure-restore-windows-server.md)
> * [Klassisk portal](backup-azure-restore-windows-server-classic.md)
>
>

Den här artikeln förklarar hur toorestore data från ett säkerhetskopieringsvalv. toorestore data använder du guiden för hello återställa Data i hello Microsoft Azure Recovery Services MARS-agenten. När du återställer data är det möjligt att:

* Återställ data toohello samma datorn från vilken hello säkerhetskopieringar som utförts.
* Återställa data tooan alternativa dator.

Microsoft släppt en Preview update toohello MARS-agenten i januari 2017. Tillsammans med felkorrigeringar, kan du använda snabbmeddelanden återställa, vilket gör att du toomount en skrivbar återställning punkt ögonblicksbild som en återställningspunkt-volym. Du kan sedan utforska hello recovery volym och kopiera filer tooa lokal dator vilket selektivt återställningen.

> [!NOTE]
> Hej [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) krävs om du vill toouse omedelbar återställning toorestore av data. Hello säkerhetskopierade data måste också skyddas i valv i språk som anges i hello support-artikeln. Kontakta hello [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) hello senaste lista över språk som har stöd för omedelbar återställning. Omedelbar återställning **inte** är tillgänglig i alla språk.
>

Omedelbar återställning är tillgänglig för användning i Recovery Services-valv i hello Azure-portalen och säkerhetskopieringsvalv i hello klassiska portalen. Om du vill toouse Instant återställa hämta hello MARS uppdateringen och följ hello procedurer som nämner omedelbar återställning.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

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
    > Om du inte klicka på Koppla från, förblir hello Recovery volym monterade i 6 timmar från hello tid när den är monterad. Hello monteringstid är dock utökade upp till 24 timmar vid en pågående filkopiering maximalt. Inga säkerhetskopieringsåtgärder kommer att köras när hello volym är monterad. Alla Säkerhetskopieringsåtgärden schemalagda toorun under hello tiden när hello volym är monterad, körs när hello recovery volymen är frånkopplade.
    >


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
    > Om du inte klicka på Koppla från, förblir hello Recovery volym monterade i 6 timmar från hello tid när den är monterad. Hello monteringstid är dock utökade upp till 24 timmar vid en pågående filkopiering maximalt. Inga säkerhetskopieringsåtgärder kommer att köras när hello volym är monterad. Alla Säkerhetskopieringsåtgärden schemalagda toorun under hello tiden när hello volym är monterad, körs när hello recovery volymen är frånkopplade.
    >

## <a name="troubleshooting"></a>Felsökning
Om Azure Backup har inte monterar hello recovery volymen även efter flera minuter om du klickar på **montera** eller misslyckas toomount hello recovery volym med ett eller flera fel gör hello nedan toobegin återställer normalt.

1.  Avbryt hello pågående montera processen om det körs under flera minuter.

2.  Se till att du är på hello senaste versionen av hello Azure Backup agent. toofind hello version information för Azure Backup-agenten, klicka på **om Microsoft Azure Recovery Services-agenten** på hello **åtgärder** fönstret Microsoft Azure Backup konsolen och se till att hello  **Version** antalet är lika tooor som är högre än hello-version som nämns i [i den här artikeln](https://go.microsoft.com/fwlink/?linkid=229525). Du kan hämta hello senaste versionen från [här](https://go.microsoft.com/fwLink/?LinkID=288905)

3.  Gå för**Enhetshanteraren** -> **lagringsstyrenheter** och se till att du kan hitta **Microsoft iSCSI Initiator**. Om du kan hitta kan gå direkt toostep 7 nedan. 

4.  Om du inte hittar Microsoft iSCSI Initiator service som anges i steg 3, kontrollera toosee om du kan hitta en post under **Enhetshanteraren** -> **lagringsstyrenheter** kallas  **Okänd enhet** med maskinvaru-ID **ROOT\ISCSIPRT**.

5.  Högerklicka på **okänd enhet** och välj **Uppdatera drivrutin**.

6.  Uppdatera hello drivrutin genom att välja hello alternativ för **Sök automatiskt efter uppdaterade drivrutiner**. Slutförande av hello uppdatering bör ändra **okänd enhet** för**Microsoft iSCSI Initiator** enligt nedan. 

    ![Kryptering](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Gå för**Aktivitetshanteraren** -> **tjänster (lokala)** -> **Microsoft iSCSI Initiator Service**. 

    ![Kryptering](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  Starta om hello Microsoft iSCSI Initiator service genom att högerklicka på hello-tjänsten, klicka på **stoppa** och ytterligare Högerklicka igen och klicka på **starta**.

9.  Försök återställa med hjälp av omedelbar återställning. 

Starta om din serverklienten om hello återställningen fortfarande misslyckas. Om en omstart är inte önskvärt eller hello recovery fortfarande misslyckas efter hello servern startas om, försök att återställa från en annan dator och kontakta Azure stöder genom att gå för[Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) och skicka en supportförfrågan.

## <a name="next-steps"></a>Nästa steg
* Nu när du har återställt dina filer och mappar, kan du [hantera dina säkerhetskopieringar](backup-azure-manage-windows-server.md).
