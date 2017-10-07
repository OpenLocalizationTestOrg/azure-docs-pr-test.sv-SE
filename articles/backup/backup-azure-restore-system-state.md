---
title: "Azure-säkerhetskopiering: Återställa systemtillståndet tooa Windows Server | Microsoft Docs"
description: "Steg för steg förklaring för återställning av systemtillståndet för Windows Server från en säkerhetskopia i Azure."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/18/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: a45506507f53e2744350d3b6b2e52f1db415de4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-system-state-toowindows-server"></a>Återställa Systemtillstånd tooWindows Server

Den här artikeln förklarar hur toorestore Windows Server systemtillståndet från en Azure Recovery Services-valvet. toorestore systemtillstånd, måste du ha en säkerhetskopia av systemtillståndet (skapas med hello instruktionerna i [säkerhetskopiera systemtillståndet](backup-azure-system-state.md#back-up-windows-server-system-state-preview)), och kontrollera att du har installerat hello [senaste versionen av hello Microsoft Azure Recovery Services (MARS)-agenten](http://aka.ms/azurebackup_agent). Återställa Systemtillstånd för Windows Server-data från ett Azure Recovery Services-valv är en tvåstegsprocess:

1. Återställa Systemtillstånd som filer från Azure Backup. När du återställer systemtillståndet som filer från Azure Backup kan du antingen:
  * Återställa Systemtillstånd toohello samma server där hello säkerhetskopieringar som utförts, eller
  * Återställa Systemtillstånd tooan alternativ filserver.

2. Tillämpa hello återställa systemtillståndet filer tooa Windows Server.


## <a name="recover-system-state-files-toohello-same-server"></a>Återställer du systemtillståndet filer toohello samma server
hello följande steg förklarar hur tooroll tillbaka din Windows Server configuration tooa tidigare tillstånd. Kan vara bra att återställa servern configuration tillbaka tooa kända stabilt tillstånd. hello följande steg för återställning hello serverns systemtillstånd från en Recovery Services-valvet. 

1. Öppna hello **Microsoft Azure Backup** snapin-modulen. Om du inte vet där hello snapin-modulen installerades, söka hello dator eller server för **Microsoft Azure Backup**.

    Hej skrivbordsapp ska visas i sökresultaten hello.

2. Klicka på **återställa Data** toostart hello guiden.

    ![Återställa Data](./media/backup-azure-restore-windows-server/recover.png)

3. På hello **komma igång** rutan, toorestore hello data toohello samma server eller dator, Välj **servern (`<server name>`)** och på **nästa**.

    ![Välj den här servern alternativet toorestore hello data toohello samma dator](./media/backup-azure-restore-system-state/samemachine.png)

4. På hello **Välj återställningsläge** fönstret Välj **systemtillstånd** och klicka sedan på **nästa**.

    ![Bläddra bland filer](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. Hello kalendern i **Välj volym och datum** pekar du väljer en återställning. 

    Du kan återställa från valfri återställningspunkt i tid. Datum i **fetstil** ange hello tillgängligheten för minst en återställningspunkt. När du har valt ett datum, om det finns flera återställningspunkter, Välj hello specifik återställningspunkt hello **tid** nedrullningsbara menyn.

    ![Volym och datum](./media/backup-azure-restore-system-state/select-date.png)

6. När du har valt hello recovery punkt toorestore, klickar du på **nästa**.

    Azure-säkerhetskopiering monterar hello lokal återställningspunkt och använder den som en återställningspunkt-volym.

7. Ange hello mål för hello återställda filer för systemtillstånd på hello nästa ruta, och klicka på **Bläddra** tooopen Windows Explorer och hitta hello filer och mappar som du vill. Hej alternativet **skapa kopior så att du har båda versionerna**, skapar kopior av enskilda filer i en befintlig systemtillstånd filarkivet istället för att skapa hello kopia av hello hela systemtillstånd Arkiv.

    ![Återställningsalternativ](./media/backup-azure-restore-system-state/recover-as-files.png)

8. Kontrollera hello information om återställning på hello **bekräftelse** och klickar på **återställa**.

   ![Klicka på Återställ tooacknowledge hello Återställ åtgärd](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. Kopiera hello *WindowsImageBackup* katalogen i hello Recovery tooa icke-kritiska målvolymen hello-Server. Vanligtvis är hello systemvolymen Windows hello kritisk volym.

10. När hello återställningen är klar gör hello hello under [tillämpa återställa systemtillståndet filer toohello Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), toocomplete hello systemtillstånd återställningsprocessen.

## <a name="recover-system-state-files-tooan-alternate-server"></a>Återställer du systemtillståndet filer tooan alternativ server

Om din Windows-Server är skadad eller inte tillgänglig och du vill toorestore den tooa stabilt läge genom att återställa Hej systemtillstånd för Windows Server, kan du återställa hello skadat serverns systemtillstånd från en annan server. Använd hello följa steg toohello återställa systemtillståndet på en separat server.  

hello-terminologi som används i de här stegen innefattar:

- *Källdatorn* – hello ursprungliga datorn från vilken hello säkerhetskopian skapades och som inte är tillgänglig för tillfället.
- *Måldatorn* – hello datorn toowhich hello data återställs.
- *Exempel valvet* – hello Recovery Services-valvet toowhich hello *källdatorn* och *måldatorn* registreras. <br/>

> [!NOTE]
> Säkerhetskopieringar som utförts från en dator får inte vara återställda tooa dator som kör en tidigare version av operativsystemet för hello. Till exempel återställa säkerhetskopior som hämtas från en Windows Server 2016-datorn inte kan vara tooWindows Server 2012 R2. Hello inversen är dock möjligt. Du kan använda säkerhetskopior från Windows Server 2012 R2 toorestore Windows Server 2016.
>

1. Öppna hello **Microsoft Azure Backup** snapin-modulen på hello *måldatorn*.
2. Se till att hello *måldatorn* och hello *källdatorn* är registrerade toohello samma Recovery Services-valvet.
3. Klicka på **återställa Data** tooinitiate hello arbetsflöde.

    ![Återställa Data](./media/backup-azure-restore-windows-server-classic/recover.png)

4. Välj **en annan server**

    ![En annan Server](./media/backup-azure-restore-system-state/anotherserver.png)

5. Ange hello valvautentiseringsfilen som motsvarar toohello *exempel valvet*. Om hello valvautentiseringsfilen är ogiltig (eller har upphört att gälla), ladda ned en ny valvautentiseringsfil från hello *exempel valvet* i hello Azure-portalen. Hello Recovery Services-valvet som är associerade med hello valvautentiseringsfilen visas när hello valvautentiseringsfilen har angetts.

6. I fönstret Välj Backup Server hello Välj hello *källdatorn* hello listan med visade datorer.

    ![Lista över datorer](./media/backup-azure-restore-windows-server-classic/machinelist.png)

7. I fönstret Välj återställningsläge hello väljer **systemtillstånd** och på **nästa**. 

    ![Söka](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. På hello kalendern i hello **Välj volym och datum** pekar du väljer en återställning. Du kan återställa från valfri återställningspunkt i tid. Datum i **fetstil** ange hello tillgängligheten för minst en återställningspunkt. När du har valt ett datum, om det finns flera återställningspunkter, Välj hello specifik återställningspunkt hello **tid** nedrullningsbara menyn. 

    ![Sökobjekt](./media/backup-azure-restore-system-state/select-date.png)

9. När du har valt hello recovery punkt toorestore, klickar du på **nästa**.

10. På hello **Välj återställningsläge för systemet tillstånd** rutan Ange hello mål där du vill att systemtillståndet filer toobe återställas och klicka sedan på **nästa**.

    ![Kryptering](./media/backup-azure-restore-system-state/recover-as-files.png)

    Hej alternativet **skapa kopior så att du har båda versionerna**, skapar kopior av enskilda filer i en befintlig systemtillstånd filarkivet istället för att skapa hello kopia av hello hela systemtillstånd Arkiv.

11. Kontrollera informationen om hello av återställningspunkter i hello bekräftelse rutan och klicka på **återställa**. 

    ![Klicka på hello Återställ knappen tooconfirm hello återställningsprocessen](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. Kopiera hello *WindowsImageBackup* directory tooa icke-kritisk volym hello Server (till exempel D:\). Vanligtvis är hello systemvolymen Windows hello kritisk volym.

13. toocomplete hello återställningen Använd hello följande avsnitt för[gäller hello återställa systemtillståndet filer på en Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).




## <a name="apply-restored-system-state-on-a-windows-server"></a>Tillämpa återställda systemtillståndet på en Windows Server

När du har återställt systemtillstånd som filer med Azure Recovery Services-agenten, återställa Använd hello Windows Server Backup-verktyget tooapply hello systemtillståndet tooWindows Server. hello verktyg för Windows Server Backup finns redan på hello-servern. hello följande steg förklarar hur tooapply hello återställa Systemtillstånd.

1. Använd hello följande kommandon tooreboot servern i *reparationsläge för katalogtjänster*. I en upphöjd kommandotolk:

    ```
    PS C:\> Bcdedit /set safeboot dsrepair
    PS C:\> Shutdown /r /t 0
    ```

2. Efter omstart hello öppna hello Windows Server Backup snapin-modulen. Om du inte vet där hello snapin-modulen installerades, söka hello dator eller server för **Windows Server Backup**.

    Hej skrivbordsapp visas i sökresultaten hello.

3. Hello snapin-modulen, Välj **lokal säkerhetskopia**.

    ![Välj lokal säkerhetskopia toorestore därifrån](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

4. På hello lokal säkerhetskopia i konsolen hello **åtgärdsfönstret**, klickar du på **återställa** tooopen hello Återställningsguiden.

5. Välj alternativet för hello **en säkerhetskopia lagrad på en annan plats**, och klicka på **nästa**.

   ![Välj toorecover tooa annan server](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

6. När du anger hello platstyp, Välj **Remote delade mappen** om din säkerhetskopia av systemtillståndet återställda tooanother server. Om systemets tillstånd återställts lokalt, välj sedan **lokala enheter**. 

    ![Välj om toorecovery från lokal server eller en annan](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

7. Ange hello sökvägen toohello *WindowsImageBackup* directory, eller välj hello lokala enheten som innehåller den här katalogen (till exempel D:\WindowsImageBackup) återställas i samband med hello systemtillstånd filer återställning med hjälp av Azure Recovery Agent och klicka på Services **nästa**.

    ![sökvägen toohello delad fil](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

8. Välj hello systemtillstånd version som du vill toorestore och på **nästa**.

9. Välj i rutan Välj typ av återställning hello **systemtillstånd** och på **nästa**.

10. Hello platsen för hello återställning av systemtillstånd, Välj **ursprungsplatsen**, och klicka på **nästa**.

11. Granska hello-händelsebekräftelse-information, verifiera hello omstart inställningarna och klicka på **återställa** tooapplly hello återställa systemtillståndet filer.

    ![Starta hello återställa systemtillståndet filer](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

## <a name="special-considerations-for-system-state-recovery-on-active-directory-server"></a>Speciella överväganden för återställning av systemtillstånd på Active Directory-server

Säkerhetskopian av systemtillstånd innehåller Active Directory-data. Använd hello följande steg toorestore Active Directory Domain Service (AD DS) från dess aktuella tillstånd tooa tidigare tillstånd.

1. Starta om hello-domänkontrollanten i Directory återställningsläge för katalogtjänster (DSRM).
2. Gör hello [här](https://technet.microsoft.com/en-us/library/cc794755(v=ws.10).aspx) toouse Windows Server Backup-cmdletar toorecover AD DS.


## <a name="troubleshoot-failed-system-state-restore"></a>Felsökning av misslyckade återställning av systemtillståndet

Om hello föregående process för tillämpning av systemtillståndet inte slutförs korrekt, använder du hello Windows Recovery Environment (Win RE) toorecover Windows Server. hello följande steg förklarar hur toorecover med Win RE. Använd det här alternativet endast om Windows Server inte startar normalt efter en återställning av systemtillstånd. följande process hello raderar-system data, var försiktig. 

1. Starta Windows-Server i hello Windows Recovery Environment (Win RE).

2. Välj Felsök hello tre alternativ.

    ![Öppna menyn](./media/backup-azure-restore-system-state/winre-1.png)

3. Från hello **avancerade alternativ** väljer **kommandotolk** och ange hello server administratörsanvändarnamn och lösenord.

   ![Öppna menyn](./media/backup-azure-restore-system-state/winre-2.png)

4. Ange hello server administratörsanvändarnamn och lösenord.

    ![Öppna menyn](./media/backup-azure-restore-system-state/winre-3.png)

5. När du öppnar hello kommandotolk i administratörsläge, kör du följande kommando tooget hello systemtillstånd säkerhetskopior.

    ```
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```
    ![Hämta systemtillstånd säkerhetskopior](./media/backup-azure-restore-system-state/winre-4.png)

6. Kör följande kommando tooget hello alla volymer som är tillgängliga i hello säkerhetskopiering.

    ```
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![Hämta systemtillstånd säkerhetskopior](./media/backup-azure-restore-system-state/winre-5.png)

7. hello återställer följande kommando alla volymer som ingår i hello säkerhetskopia av systemtillståndet. Observera att det här steget återställer endast kritiska volymer för hello som ingår i hello systemtillstånd. Alla andra data raderas.

    ```
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```
     ![Hämta systemtillstånd säkerhetskopior](./media/backup-azure-restore-system-state/winre-6.png)



## <a name="next-steps"></a>Nästa steg
* Nu när du har återställt dina filer och mappar, kan du [hantera dina säkerhetskopieringar](backup-azure-manage-windows-server.md).
