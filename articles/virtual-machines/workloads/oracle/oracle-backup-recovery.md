---
title: "aaaBack upp och Återställ en Oracle-databas 12c-databasen på en virtuell Azure Linux-dator | Microsoft Docs"
description: "Lär dig hur tooback upp och Återställ en Oracle-databas 12c-databasen i Azure-miljön."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 5/17/2017
ms.author: rclaus
ms.openlocfilehash: 68846f4efce5eabdb71cd71772e003838154e93b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a>Säkerhetskopiera och återställa en Oracle-databas 12c-databas på en virtuell Azure Linux-dator

Du kan använda Azure CLI toocreate och hantera Azure-resurser i en kommandotolk eller använda skript. I den här artikeln använder vi Azure CLI skript toodeploy en Oracle-databas 12c-databas från en avbildning för Azure Marketplace-galleriet.

Innan du börjar bör du kontrollera att Azure CLI är installerad. Mer information finns i hello [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-hello-environment"></a>Förbered hello-miljön

### <a name="step-1-prerequisites"></a>Steg 1: förutsättningar

*   tooperform hello-säkerhetskopiering och återställning process, måste du först skapa en Linux VM som har en installerad instans av Oracle-databas 12c. hello Marketplace-avbildning som du använder toocreate hello VM heter *Oracle: Oracle-databasen-Ee:12.1.0.2:latest*.

    toolearn hur toocreate en Oracle-databas finns hello [Oracle skapa database Snabbstart](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).


### <a name="step-2-connect-toohello-vm"></a>Steg 2: Anslut toohello VM

*   toocreate en SSH (Secure Shell)-session med hello VM, använda hello följande kommando. Ersätt hello IP-adress och hello värdnamn med hello `publicIpAddress` värde för den virtuella datorn.

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-hello-database"></a>Steg 3: Förbered hello-databas

1.  Det här steget förutsätter att du har en Oracle-instans (cdb1) som körs på en virtuell dator med namnet *myVM*.

    Kör hello *oracle* superanvändare rot och sedan initiera hello lyssnare:

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    Copyright (c) 1991, 2014, Oracle.  All rights reserved.

    Starting /u01/app/oracle/product/12.1.0/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Log messages written too/u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))

    Connecting too(ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
    STATUS of hello LISTENER
    ------------------------
    Alias                     LISTENER
    Version                   TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Start Date                23-MAR-2017 15:32:08
    Uptime                    0 days 0 hr. 0 min. 0 sec
    Trace Level               off
    Security                  ON: Local OS Authentication
    SNMP                      OFF
    Listener Log File         /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening Endpoints Summary...
    (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))
    hello listener supports no services
    hello command completed successfully
    ```

2.  (Valfritt) Kontrollera att hello databasen är i läget för archive log:

    ```bash
    $ sqlplus / as sysdba
    SQL> SELECT log_mode FROM v$database;

    LOG_MODE
    ------------
    NOARCHIVELOG

    SQL> SHUTDOWN IMMEDIATE;
    SQL> STARTUP MOUNT;
    SQL> ALTER DATABASE ARCHIVELOG;
    SQL> ALTER DATABASE OPEN;
    SQL> ALTER SYSTEM SWITCH LOGFILE;
    ```
3.  (Valfritt) Skapa en tabell tootest hello incheckning:

    ```bash
    SQL> alter session set "_ORACLE_SCRIPT"=true ;
    Session altered.
    SQL> create user scott identified by tiger;
    User created.
    SQL> grant create session tooscott;
    Grant succeeded.
    SQL> grant create table tooscott;
    Grant succeeded.
    SQL> alter user scott quota 100M on users;
    User altered.
    SQL> connect scott/tiger
    SQL> create table scott_table(col1 number, col2 varchar2(50));
    Table created.
    SQL> insert into scott_Table VALUES(1,'Line 1');
    1 row created.
    SQL> commit;
    Commit complete.
    ```
4.  Kontrollera eller ändra storlek och hello Säkerhetskopians plats:

    ```bash
    $ sqlplus / as sysdba
    SQL> show parameter db_recovery
    NAME                                 TYPE        VALUE
    ------------------------------------ ----------- ------------------------------
    db_recovery_file_dest                string      /u01/app/oracle/fast_recovery_area
    db_recovery_file_dest_size           big integer 4560M
    ```
5. Använd Oracle Recovery Manager (RMAN) tooback hello databasen:

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a>Steg 4: Programkonsekvent säkerhetskopiering för virtuella Linux-datorer

Programkonsekvent säkerhetskopiering är en ny funktion i Azure Backup. Du kan skapa och välj tooexecute skript före och efter hello VM-ögonblicksbild (före ögonblicksbild och efter ögonblicksbild).

1. Hämta hello JSON-fil.

    Hämta VMSnapshotScriptPluginConfig.json från https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig. hello filinnehållet se liknande toohello följande:

    ```azurecli
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "",
        "postScriptLocation" : "",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

2. Skapa hello /etc/azure mapp på hello VM:

    ```bash
    $ sudo su -
    # mkdir /etc/azure
    # cd /etc/azure
    ```

3. Kopiera hello JSON-fil.

    Kopiera VMSnapshotScriptPluginConfig.json toohello /etc/azure mapp.

4. Redigera hello JSON-fil.

    Redigera hello VMSnapshotScriptPluginConfig.json filen tooinclude hello `PreScriptLocation` och `PostScriptlocation` parametrar. Exempel:

    ```azurecli
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "/etc/azure/pre_script.sh",
        "postScriptLocation" : "/etc/azure/post_script.sh",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

5. Skapa hello inför ögonblicksbilden och efter ögonblickbild skriptfiler.

    Här är ett exempel på skript inför ögonblicksbilden och efter ögonblicksbild för en ”kalla” säkerhetskopiering (en offlinesäkerhetskopiering, med avstängning och omstart):

    För /etc/azure/pre_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
    ```

    För /etc/azure/post_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
    ```

    Här är ett exempel på inför ögonblicksbilden och efter ögonblickbild skript för ”varm säkerhetskopiering” (en onlinesäkerhetskopiering):

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    För /etc/azure/post_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    För /etc/azure/pre_script.sql, ändrar du hello innehållet i filen hello enligt dina krav:

    ```bash
    alter tablespace SYSTEM begin backup;
    ...
    ...
    alter system switch logfile;
    alter system archive log stop;
    ```

    För /etc/azure/post_script.sql, ändrar du hello innehållet i filen hello enligt dina krav:

    ```bash
    alter tablespace SYSTEM end backup;
    ...
    ...
    alter system archive log start;
    ```

6. Ändra behörigheter för filer:

    ```bash
    # chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
    # chmod 700 /etc/azure/pre_script.sh
    # chmod 700 /etc/azure/post_script.sh
    ```

7. Testa hello-skript.

    tootest hello skript som först logga in som rot. Kontrollera sedan att det inte finns några fel:

    ```bash
    # /etc/azure/pre_script.sh
    # /etc/azure/post_script.sh
    ```

Mer information finns i [programkonsekvent säkerhetskopiering för virtuella Linux-datorer](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).


### <a name="step-5-use-azure-recovery-services-vaults-tooback-up-hello-vm"></a>Steg 5: Använd Azure Recovery Services-valv tooback in hello VM

1.  I hello Azure-portalen, söka efter **Recovery Services-valv**.

    ![Sidan för Recovery Services-valv](./media/oracle-backup-recovery/recovery_service_01.png)

2.  På hello **Recovery Services-valv** tooadd ett nytt valv bladet klickar du på **Lägg till**.

    ![Recovery Services-valv lägger du till sidan](./media/oracle-backup-recovery/recovery_service_02.png)

3.  toocontinue, klickar du på **myVault**.

    ![Sidan innehåller information om Recovery Services-valv](./media/oracle-backup-recovery/recovery_service_03.png)

4.  På hello **myVault** bladet, klickar du på **säkerhetskopiering**.

    ![Recovery Services-valv säkerhetskopiera sida](./media/oracle-backup-recovery/recovery_service_04.png)

5.  På hello **säkerhetskopiering målet** blad, Använd hello standardvärdena för **Azure** och **virtuella**. Klicka på **OK**.

    ![Sidan innehåller information om Recovery Services-valv](./media/oracle-backup-recovery/recovery_service_05.png)

6.  För **säkerhetskopiera princip**, använda **DefaultPolicy**, eller välj **Skapa ny princip**. Klicka på **OK**.

    ![Recovery Services-valv säkerhetskopiera princip detaljsida](./media/oracle-backup-recovery/recovery_service_06.png)

7.  På hello **Välj virtuella datorer** bladet, Välj hello **myVM1** kryssrutan och klicka sedan på **OK**. Klicka på hello **Aktivera säkerhetskopiering** knappen.

    ![Recovery Services valv objekt toohello säkerhetskopiering detaljsida](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > När du klickar på **Aktivera säkerhetskopiering**, hello säkerhetskopieringen startas inte förrän hello schemalagda tiden går ut. tooset upp en omedelbar säkerhetskopia fullständig hello nästa steg.

8.  På hello **myVault - objekt för säkerhetskopiering** bladet under **säkerhetskopiering OBJEKTANTAL**, Välj hello säkerhetskopiering objektantal.

    ![Recovery Services valv myVault detaljsida](./media/oracle-backup-recovery/recovery_service_08.png)

9.  På hello **säkerhetskopiering objekt (Azure virtuell dator)** bladet hello höger på sidan hello Klicka hello knappen (**...** ) knappen och klicka sedan på **Säkerhetskopiera nu**.

    ![Recovery Services-valv säkerhetskopiering nu kommando](./media/oracle-backup-recovery/recovery_service_09.png)

10. Klicka på hello **säkerhetskopiering** knappen. Vänta tills hello säkerhetskopieringsprocessen toofinish. Gå sedan för[steg 6: ta bort databasfilerna hello](#step-6-remove-the-database-files).

    tooview hello status för hello säkerhetskopieringsjobb, klickar du på **jobb**.

    ![Recovery Services-valv jobbet sida](./media/oracle-backup-recovery/recovery_service_10.png)

    hello status för hello säkerhetskopieringsjobbet visas i följande bild hello:

    ![Recovery Services-valv jobbet sida med status](./media/oracle-backup-recovery/recovery_service_11.png)

11. Åtgärda eventuella fel i hello loggfilen för en programkonsekvent säkerhetskopiering. hello loggfilen finns i /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.

### <a name="step-6-remove-hello-database-files"></a>Steg 6: Ta bort hello-databasfiler 
Senare i den här artikeln lär du dig hur tootest hello återställningsprocessen. Innan du kan testa hello återställningsprocessen har tooremove hello-databasfiler.

1.  Ta bort hello tabellutrymmet och säkerhetskopiering filer:

    ```bash
    $ sudo su - oracle
    $ cd /u01/app/oracle/oradata/cdb1
    $ rm -f *.dbf
    $ cd /u01/app/oracle/fast_recovery_area/CDB1/backupset
    $ rm -rf *
    ```
    
2.  (Valfritt) Stänga av hello Oracle-instansen:

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-hello-deleted-files-from-hello-recovery-services-vaults"></a>Återställa hello bort filer från hello Recovery Services-valv
toorestore hello borttagna filer, fullständig hello följande steg:

1. I hello Azure-portalen, söka efter hello *myVault* Recovery Services-valv objektet. På hello **översikt** bladet under **Säkerhetskopiera objekt**, Välj hello antal objekt.

    ![Recovery Services valv myVault Säkerhetskopiera objekt](./media/oracle-backup-recovery/recovery_service_12.png)

2. Under **säkerhetskopiering OBJEKTANTAL**, Välj hello antal objekt.

    ![Recovery Services-valv antal för virtuell dator i Azure-säkerhetskopiering objekt](./media/oracle-backup-recovery/recovery_service_13.png)

3. På hello **myvm1** bladet, klickar du på **filåterställning (förhandsgranskning)**.

    ![Skärmbild av hello Recovery Services-valv återställningssidan för filen](./media/oracle-backup-recovery/recovery_service_14.png)

4. På hello **filåterställning (förhandsgranskning)** rutan klickar du på **hämta skriptet**. Spara hello download (.sh) tooa mapp på hello-klientdator.

    ![Hämta filen sparar alternativ](./media/oracle-backup-recovery/recovery_service_15.png)

5. Kopiera hello .sh filen toohello VM.

    hello som följande exempel visar hur du toouse en säker kopia (scp) kommandot toomove hello filen toohello VM. Du kan också kopiera hello innehållet toohello Urklipp och klistra in hello innehållet om du i en ny fil som har ställts in på hello VM.

    > [!IMPORTANT]
    > I följande exempel hello, kontrollerar du att du uppdaterar hello IP-adress och mappen värden. hello-värden måste mappa toohello mappen där hello filen sparas.

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```
6. Ändra hello-fil, så att det ägs av hello rot.

    I följande exempel hello, ändra hello-filen så att det ägs av hello rot. Ändra behörigheter.

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```
    hello följande exempel visar vad som ska visas när du har kört hello föregående skript. När du uppmanas toocontinue, ange **Y**.

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    hello script requires 'open-iscsi' and 'lshw' toorun.
    Do you want us tooinstall 'open-iscsi' and 'lshw' on this machine?
    Please press 'Y' toocontinue with installation, 'N' tooabort hello operation. : Y
    Installing 'open-iscsi'....
    Installing 'lshw'....

    Connecting toorecovery point using ISCSI service...

    Connection succeeded!

    Please wait while we attach volumes of hello recovery point toothis machine...

    ************ Volumes of hello recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath

    1)  | /dev/sde  |  /dev/sde1  |  /root/myVM-20170517093913/Volume1

    2)  | /dev/sde  |  /dev/sde2  |  /root/myVM-20170517093913/Volume2

    ************ Open File Explorer toobrowse for files. ************

    After recovery, tooremove hello disks and close hello connection toohello recovery point, please click 'Unmount Disks' in step 3 of hello portal.

    Please enter 'q/Q' tooexit...
    ```

7. Åtkomst toohello monterade volymer bekräftas.

    tooexit, ange **q**, och sök sedan efter hello monterade volymer. en lista över hello läggs volymer i en kommandotolk, ange toocreate **df -k**.

    ![hello df -k kommando](./media/oracle-backup-recovery/recovery_service_16.png)

8. Använd hello följande skript toocopy hello saknas filer tillbaka toohello mappar:

    ```bash
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cp *.bkp /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cd /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # chown oracle:oinstall *.bkp
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/oradata/cdb1
    # cp *.dbf /u01/app/oracle/oradata/cdb1
    # cd /u01/app/oracle/oradata/cdb1
    # chown oracle:oinstall *.dbf
    ```
9. I följande skript hello, använder du RMAN toorecover hello databasen:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```
    
10. Demontera hello disk.

    I hello Azure-portalen på hello **filåterställning (förhandsgranskning)** bladet, klickar du på **demontera diskar**.

    ![Demontera diskar kommando](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-hello-entire-vm"></a>Återställa hello hela VM

I stället för att återställa hello bort filer från hello Recovery Services-valv, kan du återställa hello hela datorn.

### <a name="step-1-delete-myvm"></a>Steg 1: Ta bort myVM

*   I hello Azure-portalen, går toohello **myVM1** valvet och välj sedan **ta bort**.

    ![Valvet delete-kommandot](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-hello-vm"></a>Steg 2: Återställa hello VM

1.  Gå för**Recovery Services-valv**, och välj sedan **myVault**.

    ![myVault post](./media/oracle-backup-recovery/recover_vm_02.png)

2.  På hello **översikt** bladet under **Säkerhetskopiera objekt**, Välj hello antal objekt.

    ![myVault Säkerhetskopiera objekt](./media/oracle-backup-recovery/recover_vm_03.png)

3.  På hello **säkerhetskopiering objekt (Azure virtuell dator)** bladet väljer **myvm1**.

    ![Återställningssidan för VM](./media/oracle-backup-recovery/recover_vm_04.png)

4.  På hello **myvm1** bladet Klicka hello knappen (**...** ) knappen och klicka sedan på **återställa VM**.

    ![Restore-kommandots VM](./media/oracle-backup-recovery/recover_vm_05.png)

5.  På hello **Välj återställningspunkt** bladet, Välj hello objekt du vill toorestore och klicka sedan på **OK**.

    ![Välj hello återställningspunkt](./media/oracle-backup-recovery/recover_vm_06.png)

    Om du har aktiverat programkonsekvent säkerhetskopiering, visas en blå vertikalstreck.

6.  På hello **Återställ konfiguration** bladet välj hello virtuella datornamnet, Välj hello resursgrupp och klicka sedan på **OK**.

    ![Återställa konfigurationsvärden](./media/oracle-backup-recovery/recover_vm_07.png)

7.  toorestore hello VM, klicka på hello **återställa** knappen.

8.  tooview hello status för hello återställningsprocessen klickar du på **jobb**, och klicka sedan på **säkerhetskopieringsjobb**.

    ![Säkerhetskopieringsjobb status-kommandot](./media/oracle-backup-recovery/recover_vm_08.png)

    hello visar följande bild hello återställningsprocessen hello status:

    ![Status för hello återställningsprocessen](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-hello-public-ip-address"></a>Steg 3: Ange hello offentliga IP-adress
Efter hello VM har återställts, ställa in hello offentlig IP-adress.

1.  Skriv i sökrutan hello **offentliga IP-adressen**.

    ![Lista över offentliga IP-adresser](./media/oracle-backup-recovery/create_ip_00.png)

2.  På hello **offentliga IP-adresser** bladet, klickar du på **Lägg till**. På hello **skapa offentlig IP-adress** bladet för **namnet**väljer hello offentliga IP-namn. För **resursgrupp**, väljer du **använd befintlig**. Klicka på **Skapa**.

    ![Skapa IP-adress](./media/oracle-backup-recovery/create_ip_01.png)

3.  tooassociate hello offentlig IP-adress med hello nätverksgränssnittet för hello VM, söka efter och välj **myVMip**. Klicka på **associera**.

    ![Associera IP-adress](./media/oracle-backup-recovery/create_ip_02.png)

4.  För **resurstypen**väljer **nätverksgränssnittet**. Välj hello nätverksgränssnitt som används av hello myVM instansen och klicka sedan på **OK**.

    ![Välj resurstyp och NIC-värden](./media/oracle-backup-recovery/create_ip_03.png)

5.  Sök efter och öppna hello instans av myVM portar från hello-portalen. hello IP-adress som är associerad med hello VM visas på hello myVM **översikt** bladet.

    ![Värdet för IP-adress](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-toohello-vm"></a>Steg 4: Anslut toohello VM

*   tooconnect toohello VM, använda hello följande skript:

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-5-test-whether-hello-database-is-accessible"></a>Steg 5: Testa om hello-databasen är tillgänglig
*   tootest tillgänglighet, Använd hello följande skript:

    ```bash 
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> startup
    ```

    > [!IMPORTANT]
    > Om hello databasen **Start** kommandot genererar ett fel, toorecover hello databasen, se [steg 6: Använd RMAN toorecover hello databasen](#step-6-optional-use-rman-to-recover-the-database).

### <a name="step-6-optional-use-rman-toorecover-hello-database"></a>Steg 6: (Valfritt) använda RMAN toorecover hello databas
*   toorecover hello databas, Använd hello följande skript:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

hello säkerhetskopiering och återställning av hello Oracle 12c databasen på en Azure Linux-VM är nu klar.

## <a name="delete-hello-vm"></a>Ta bort hello VM

När du behöver inte längre hello VM, kan du använda hello efter kommandot tooremove hello resursgrupp, hello VM och alla relaterade resurser:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

[Självstudier: Skapa högtillgängliga virtuella datorer](../../linux/create-cli-complete.md)

[Utforska VM distribution Azure CLI-exempel](../../linux/cli-samples.md)



