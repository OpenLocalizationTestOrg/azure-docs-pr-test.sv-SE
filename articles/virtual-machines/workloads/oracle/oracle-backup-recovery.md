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
# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a><span data-ttu-id="da0c6-103">Säkerhetskopiera och återställa en Oracle-databas 12c-databas på en virtuell Azure Linux-dator</span><span class="sxs-lookup"><span data-stu-id="da0c6-103">Back up and recover an Oracle Database 12c database on an Azure Linux virtual machine</span></span>

<span data-ttu-id="da0c6-104">Du kan använda Azure CLI toocreate och hantera Azure-resurser i en kommandotolk eller använda skript.</span><span class="sxs-lookup"><span data-stu-id="da0c6-104">You can use Azure CLI toocreate and manage Azure resources at a command prompt, or use scripts.</span></span> <span data-ttu-id="da0c6-105">I den här artikeln använder vi Azure CLI skript toodeploy en Oracle-databas 12c-databas från en avbildning för Azure Marketplace-galleriet.</span><span class="sxs-lookup"><span data-stu-id="da0c6-105">In this article, we use Azure CLI scripts toodeploy an Oracle Database 12c database from an Azure Marketplace gallery image.</span></span>

<span data-ttu-id="da0c6-106">Innan du börjar bör du kontrollera att Azure CLI är installerad.</span><span class="sxs-lookup"><span data-stu-id="da0c6-106">Before you begin, make sure that Azure CLI is installed.</span></span> <span data-ttu-id="da0c6-107">Mer information finns i hello [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="da0c6-107">For more information, see hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-hello-environment"></a><span data-ttu-id="da0c6-108">Förbered hello-miljön</span><span class="sxs-lookup"><span data-stu-id="da0c6-108">Prepare hello environment</span></span>

### <a name="step-1-prerequisites"></a><span data-ttu-id="da0c6-109">Steg 1: förutsättningar</span><span class="sxs-lookup"><span data-stu-id="da0c6-109">Step 1: Prerequisites</span></span>

*   <span data-ttu-id="da0c6-110">tooperform hello-säkerhetskopiering och återställning process, måste du först skapa en Linux VM som har en installerad instans av Oracle-databas 12c.</span><span class="sxs-lookup"><span data-stu-id="da0c6-110">tooperform hello backup and recovery process, you must first create a Linux VM that has an installed instance of Oracle Database 12c.</span></span> <span data-ttu-id="da0c6-111">hello Marketplace-avbildning som du använder toocreate hello VM heter *Oracle: Oracle-databasen-Ee:12.1.0.2:latest*.</span><span class="sxs-lookup"><span data-stu-id="da0c6-111">hello Marketplace image you use toocreate hello VM is named *Oracle:Oracle-Database-Ee:12.1.0.2:latest*.</span></span>

    <span data-ttu-id="da0c6-112">toolearn hur toocreate en Oracle-databas finns hello [Oracle skapa database Snabbstart](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).</span><span class="sxs-lookup"><span data-stu-id="da0c6-112">toolearn how toocreate an Oracle database, see hello [Oracle create database quickstart](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).</span></span>


### <a name="step-2-connect-toohello-vm"></a><span data-ttu-id="da0c6-113">Steg 2: Anslut toohello VM</span><span class="sxs-lookup"><span data-stu-id="da0c6-113">Step 2: Connect toohello VM</span></span>

*   <span data-ttu-id="da0c6-114">toocreate en SSH (Secure Shell)-session med hello VM, använda hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="da0c6-114">toocreate a Secure Shell (SSH) session with hello VM, use hello following command.</span></span> <span data-ttu-id="da0c6-115">Ersätt hello IP-adress och hello värdnamn med hello `publicIpAddress` värde för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="da0c6-115">Replace hello IP address and hello host name combination with hello `publicIpAddress` value for your VM.</span></span>

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-hello-database"></a><span data-ttu-id="da0c6-116">Steg 3: Förbered hello-databas</span><span class="sxs-lookup"><span data-stu-id="da0c6-116">Step 3: Prepare hello database</span></span>

1.  <span data-ttu-id="da0c6-117">Det här steget förutsätter att du har en Oracle-instans (cdb1) som körs på en virtuell dator med namnet *myVM*.</span><span class="sxs-lookup"><span data-stu-id="da0c6-117">This step assumes that you have an Oracle instance (cdb1) that is running on a VM named *myVM*.</span></span>

    <span data-ttu-id="da0c6-118">Kör hello *oracle* superanvändare rot och sedan initiera hello lyssnare:</span><span class="sxs-lookup"><span data-stu-id="da0c6-118">Run hello *oracle* superuser root, and then initialize hello listener:</span></span>

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

2.  <span data-ttu-id="da0c6-119">(Valfritt) Kontrollera att hello databasen är i läget för archive log:</span><span class="sxs-lookup"><span data-stu-id="da0c6-119">(Optional) Make sure hello database is in archive log mode:</span></span>

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
3.  <span data-ttu-id="da0c6-120">(Valfritt) Skapa en tabell tootest hello incheckning:</span><span class="sxs-lookup"><span data-stu-id="da0c6-120">(Optional) Create a table tootest hello commit:</span></span>

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
4.  <span data-ttu-id="da0c6-121">Kontrollera eller ändra storlek och hello Säkerhetskopians plats:</span><span class="sxs-lookup"><span data-stu-id="da0c6-121">Verify or change hello backup file location and size:</span></span>

    ```bash
    $ sqlplus / as sysdba
    SQL> show parameter db_recovery
    NAME                                 TYPE        VALUE
    ------------------------------------ ----------- ------------------------------
    db_recovery_file_dest                string      /u01/app/oracle/fast_recovery_area
    db_recovery_file_dest_size           big integer 4560M
    ```
5. <span data-ttu-id="da0c6-122">Använd Oracle Recovery Manager (RMAN) tooback hello databasen:</span><span class="sxs-lookup"><span data-stu-id="da0c6-122">Use Oracle Recovery Manager (RMAN) tooback up hello database:</span></span>

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a><span data-ttu-id="da0c6-123">Steg 4: Programkonsekvent säkerhetskopiering för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="da0c6-123">Step 4: Application-consistent backup for Linux VMs</span></span>

<span data-ttu-id="da0c6-124">Programkonsekvent säkerhetskopiering är en ny funktion i Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="da0c6-124">Application-consistent backups is a new feature in Azure Backup.</span></span> <span data-ttu-id="da0c6-125">Du kan skapa och välj tooexecute skript före och efter hello VM-ögonblicksbild (före ögonblicksbild och efter ögonblicksbild).</span><span class="sxs-lookup"><span data-stu-id="da0c6-125">You can create and select scripts tooexecute before and after hello VM snapshot (pre-snapshot and post-snapshot).</span></span>

1. <span data-ttu-id="da0c6-126">Hämta hello JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="da0c6-126">Download hello JSON file.</span></span>

    <span data-ttu-id="da0c6-127">Hämta VMSnapshotScriptPluginConfig.json från https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig.</span><span class="sxs-lookup"><span data-stu-id="da0c6-127">Download VMSnapshotScriptPluginConfig.json from https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig.</span></span> <span data-ttu-id="da0c6-128">hello filinnehållet se liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="da0c6-128">hello file contents look similar toohello following:</span></span>

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

2. <span data-ttu-id="da0c6-129">Skapa hello /etc/azure mapp på hello VM:</span><span class="sxs-lookup"><span data-stu-id="da0c6-129">Create hello /etc/azure folder on hello VM:</span></span>

    ```bash
    $ sudo su -
    # mkdir /etc/azure
    # cd /etc/azure
    ```

3. <span data-ttu-id="da0c6-130">Kopiera hello JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="da0c6-130">Copy hello JSON file.</span></span>

    <span data-ttu-id="da0c6-131">Kopiera VMSnapshotScriptPluginConfig.json toohello /etc/azure mapp.</span><span class="sxs-lookup"><span data-stu-id="da0c6-131">Copy VMSnapshotScriptPluginConfig.json toohello /etc/azure folder.</span></span>

4. <span data-ttu-id="da0c6-132">Redigera hello JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="da0c6-132">Edit hello JSON file.</span></span>

    <span data-ttu-id="da0c6-133">Redigera hello VMSnapshotScriptPluginConfig.json filen tooinclude hello `PreScriptLocation` och `PostScriptlocation` parametrar.</span><span class="sxs-lookup"><span data-stu-id="da0c6-133">Edit hello VMSnapshotScriptPluginConfig.json file tooinclude hello `PreScriptLocation` and `PostScriptlocation` parameters.</span></span> <span data-ttu-id="da0c6-134">Exempel:</span><span class="sxs-lookup"><span data-stu-id="da0c6-134">For example:</span></span>

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

5. <span data-ttu-id="da0c6-135">Skapa hello inför ögonblicksbilden och efter ögonblickbild skriptfiler.</span><span class="sxs-lookup"><span data-stu-id="da0c6-135">Create hello pre-snapshot and post-snapshot script files.</span></span>

    <span data-ttu-id="da0c6-136">Här är ett exempel på skript inför ögonblicksbilden och efter ögonblicksbild för en ”kalla” säkerhetskopiering (en offlinesäkerhetskopiering, med avstängning och omstart):</span><span class="sxs-lookup"><span data-stu-id="da0c6-136">Here's an example of pre-snapshot and post-snapshot scripts for a "cold backup" (an offline backup, with shutdown and restart):</span></span>

    <span data-ttu-id="da0c6-137">För /etc/azure/pre_script.sh:</span><span class="sxs-lookup"><span data-stu-id="da0c6-137">For /etc/azure/pre_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="da0c6-138">För /etc/azure/post_script.sh:</span><span class="sxs-lookup"><span data-stu-id="da0c6-138">For /etc/azure/post_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
    ```

    <span data-ttu-id="da0c6-139">Här är ett exempel på inför ögonblicksbilden och efter ögonblickbild skript för ”varm säkerhetskopiering” (en onlinesäkerhetskopiering):</span><span class="sxs-lookup"><span data-stu-id="da0c6-139">Here's an example of pre-snapshot and post-snapshot scripts for a "hot backup" (an online backup):</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="da0c6-140">För /etc/azure/post_script.sh:</span><span class="sxs-lookup"><span data-stu-id="da0c6-140">For /etc/azure/post_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="da0c6-141">För /etc/azure/pre_script.sql, ändrar du hello innehållet i filen hello enligt dina krav:</span><span class="sxs-lookup"><span data-stu-id="da0c6-141">For /etc/azure/pre_script.sql, modify hello contents of hello file per your requirements:</span></span>

    ```bash
    alter tablespace SYSTEM begin backup;
    ...
    ...
    alter system switch logfile;
    alter system archive log stop;
    ```

    <span data-ttu-id="da0c6-142">För /etc/azure/post_script.sql, ändrar du hello innehållet i filen hello enligt dina krav:</span><span class="sxs-lookup"><span data-stu-id="da0c6-142">For /etc/azure/post_script.sql, modify hello contents of hello file per your requirements:</span></span>

    ```bash
    alter tablespace SYSTEM end backup;
    ...
    ...
    alter system archive log start;
    ```

6. <span data-ttu-id="da0c6-143">Ändra behörigheter för filer:</span><span class="sxs-lookup"><span data-stu-id="da0c6-143">Change file permissions:</span></span>

    ```bash
    # chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
    # chmod 700 /etc/azure/pre_script.sh
    # chmod 700 /etc/azure/post_script.sh
    ```

7. <span data-ttu-id="da0c6-144">Testa hello-skript.</span><span class="sxs-lookup"><span data-stu-id="da0c6-144">Test hello scripts.</span></span>

    <span data-ttu-id="da0c6-145">tootest hello skript som först logga in som rot.</span><span class="sxs-lookup"><span data-stu-id="da0c6-145">tootest hello scripts, first, sign in as root.</span></span> <span data-ttu-id="da0c6-146">Kontrollera sedan att det inte finns några fel:</span><span class="sxs-lookup"><span data-stu-id="da0c6-146">Then, ensure that there are no errors:</span></span>

    ```bash
    # /etc/azure/pre_script.sh
    # /etc/azure/post_script.sh
    ```

<span data-ttu-id="da0c6-147">Mer information finns i [programkonsekvent säkerhetskopiering för virtuella Linux-datorer](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).</span><span class="sxs-lookup"><span data-stu-id="da0c6-147">For more information, see [Application-consistent backup for Linux VMs](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).</span></span>


### <a name="step-5-use-azure-recovery-services-vaults-tooback-up-hello-vm"></a><span data-ttu-id="da0c6-148">Steg 5: Använd Azure Recovery Services-valv tooback in hello VM</span><span class="sxs-lookup"><span data-stu-id="da0c6-148">Step 5: Use Azure Recovery Services vaults tooback up hello VM</span></span>

1.  <span data-ttu-id="da0c6-149">I hello Azure-portalen, söka efter **Recovery Services-valv**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-149">In hello Azure portal, search for **Recovery Services vaults**.</span></span>

    ![Sidan för Recovery Services-valv](./media/oracle-backup-recovery/recovery_service_01.png)

2.  <span data-ttu-id="da0c6-151">På hello **Recovery Services-valv** tooadd ett nytt valv bladet klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-151">On hello **Recovery Services vaults** blade, tooadd a new vault, click **Add**.</span></span>

    ![Recovery Services-valv lägger du till sidan](./media/oracle-backup-recovery/recovery_service_02.png)

3.  <span data-ttu-id="da0c6-153">toocontinue, klickar du på **myVault**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-153">toocontinue, click **myVault**.</span></span>

    ![Sidan innehåller information om Recovery Services-valv](./media/oracle-backup-recovery/recovery_service_03.png)

4.  <span data-ttu-id="da0c6-155">På hello **myVault** bladet, klickar du på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-155">On hello **myVault** blade, click **Backup**.</span></span>

    ![Recovery Services-valv säkerhetskopiera sida](./media/oracle-backup-recovery/recovery_service_04.png)

5.  <span data-ttu-id="da0c6-157">På hello **säkerhetskopiering målet** blad, Använd hello standardvärdena för **Azure** och **virtuella**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-157">On hello **Backup Goal** blade, use hello default values of **Azure** and **Virtual machine**.</span></span> <span data-ttu-id="da0c6-158">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-158">Click **OK**.</span></span>

    ![Sidan innehåller information om Recovery Services-valv](./media/oracle-backup-recovery/recovery_service_05.png)

6.  <span data-ttu-id="da0c6-160">För **säkerhetskopiera princip**, använda **DefaultPolicy**, eller välj **Skapa ny princip**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-160">For **Backup policy**, use **DefaultPolicy**, or select **Create New policy**.</span></span> <span data-ttu-id="da0c6-161">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-161">Click **OK**.</span></span>

    ![Recovery Services-valv säkerhetskopiera princip detaljsida](./media/oracle-backup-recovery/recovery_service_06.png)

7.  <span data-ttu-id="da0c6-163">På hello **Välj virtuella datorer** bladet, Välj hello **myVM1** kryssrutan och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-163">On hello **Select virtual machines** blade, select hello **myVM1** check box, and then click **OK**.</span></span> <span data-ttu-id="da0c6-164">Klicka på hello **Aktivera säkerhetskopiering** knappen.</span><span class="sxs-lookup"><span data-stu-id="da0c6-164">Click hello **Enable backup** button.</span></span>

    ![Recovery Services valv objekt toohello säkerhetskopiering detaljsida](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > <span data-ttu-id="da0c6-166">När du klickar på **Aktivera säkerhetskopiering**, hello säkerhetskopieringen startas inte förrän hello schemalagda tiden går ut.</span><span class="sxs-lookup"><span data-stu-id="da0c6-166">After you click **Enable backup**, hello backup process doesn't start until hello scheduled time expires.</span></span> <span data-ttu-id="da0c6-167">tooset upp en omedelbar säkerhetskopia fullständig hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="da0c6-167">tooset up an immediate backup, complete hello next step.</span></span>

8.  <span data-ttu-id="da0c6-168">På hello **myVault - objekt för säkerhetskopiering** bladet under **säkerhetskopiering OBJEKTANTAL**, Välj hello säkerhetskopiering objektantal.</span><span class="sxs-lookup"><span data-stu-id="da0c6-168">On hello **myVault - Backup items** blade, under **BACKUP ITEM COUNT**, select hello backup item count.</span></span>

    ![Recovery Services valv myVault detaljsida](./media/oracle-backup-recovery/recovery_service_08.png)

9.  <span data-ttu-id="da0c6-170">På hello **säkerhetskopiering objekt (Azure virtuell dator)** bladet hello höger på sidan hello Klicka hello knappen (**...** ) knappen och klicka sedan på **Säkerhetskopiera nu**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-170">On hello **Backup Items (Azure Virtual Machine)** blade, on hello right side of hello page, click hello ellipsis (**...**) button, and then click **Backup now**.</span></span>

    ![Recovery Services-valv säkerhetskopiering nu kommando](./media/oracle-backup-recovery/recovery_service_09.png)

10. <span data-ttu-id="da0c6-172">Klicka på hello **säkerhetskopiering** knappen.</span><span class="sxs-lookup"><span data-stu-id="da0c6-172">Click hello **Backup** button.</span></span> <span data-ttu-id="da0c6-173">Vänta tills hello säkerhetskopieringsprocessen toofinish.</span><span class="sxs-lookup"><span data-stu-id="da0c6-173">Wait for hello backup process toofinish.</span></span> <span data-ttu-id="da0c6-174">Gå sedan för[steg 6: ta bort databasfilerna hello](#step-6-remove-the-database-files).</span><span class="sxs-lookup"><span data-stu-id="da0c6-174">Then, go too[Step 6: Remove hello database files](#step-6-remove-the-database-files).</span></span>

    <span data-ttu-id="da0c6-175">tooview hello status för hello säkerhetskopieringsjobb, klickar du på **jobb**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-175">tooview hello status of hello backup job, click **Jobs**.</span></span>

    ![Recovery Services-valv jobbet sida](./media/oracle-backup-recovery/recovery_service_10.png)

    <span data-ttu-id="da0c6-177">hello status för hello säkerhetskopieringsjobbet visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="da0c6-177">hello status of hello backup job appears in hello following image:</span></span>

    ![Recovery Services-valv jobbet sida med status](./media/oracle-backup-recovery/recovery_service_11.png)

11. <span data-ttu-id="da0c6-179">Åtgärda eventuella fel i hello loggfilen för en programkonsekvent säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="da0c6-179">For an application-consistent backup, address any errors in hello log file.</span></span> <span data-ttu-id="da0c6-180">hello loggfilen finns i /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.</span><span class="sxs-lookup"><span data-stu-id="da0c6-180">hello log file is located at /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.</span></span>

### <a name="step-6-remove-hello-database-files"></a><span data-ttu-id="da0c6-181">Steg 6: Ta bort hello-databasfiler</span><span class="sxs-lookup"><span data-stu-id="da0c6-181">Step 6: Remove hello database files</span></span> 
<span data-ttu-id="da0c6-182">Senare i den här artikeln lär du dig hur tootest hello återställningsprocessen.</span><span class="sxs-lookup"><span data-stu-id="da0c6-182">Later in this article, you'll learn how tootest hello recovery process.</span></span> <span data-ttu-id="da0c6-183">Innan du kan testa hello återställningsprocessen har tooremove hello-databasfiler.</span><span class="sxs-lookup"><span data-stu-id="da0c6-183">Before you can test hello recovery process, you have tooremove hello database files.</span></span>

1.  <span data-ttu-id="da0c6-184">Ta bort hello tabellutrymmet och säkerhetskopiering filer:</span><span class="sxs-lookup"><span data-stu-id="da0c6-184">Remove hello tablespace and backup files:</span></span>

    ```bash
    $ sudo su - oracle
    $ cd /u01/app/oracle/oradata/cdb1
    $ rm -f *.dbf
    $ cd /u01/app/oracle/fast_recovery_area/CDB1/backupset
    $ rm -rf *
    ```
    
2.  <span data-ttu-id="da0c6-185">(Valfritt) Stänga av hello Oracle-instansen:</span><span class="sxs-lookup"><span data-stu-id="da0c6-185">(Optional) Shut down hello Oracle instance:</span></span>

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-hello-deleted-files-from-hello-recovery-services-vaults"></a><span data-ttu-id="da0c6-186">Återställa hello bort filer från hello Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="da0c6-186">Restore hello deleted files from hello Recovery Services vaults</span></span>
<span data-ttu-id="da0c6-187">toorestore hello borttagna filer, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="da0c6-187">toorestore hello deleted files, complete hello following steps:</span></span>

1. <span data-ttu-id="da0c6-188">I hello Azure-portalen, söka efter hello *myVault* Recovery Services-valv objektet.</span><span class="sxs-lookup"><span data-stu-id="da0c6-188">In hello Azure portal, search for hello *myVault* Recovery Services vaults item.</span></span> <span data-ttu-id="da0c6-189">På hello **översikt** bladet under **Säkerhetskopiera objekt**, Välj hello antal objekt.</span><span class="sxs-lookup"><span data-stu-id="da0c6-189">On hello **Overview** blade, under **Backup items**, select hello number of items.</span></span>

    ![Recovery Services valv myVault Säkerhetskopiera objekt](./media/oracle-backup-recovery/recovery_service_12.png)

2. <span data-ttu-id="da0c6-191">Under **säkerhetskopiering OBJEKTANTAL**, Välj hello antal objekt.</span><span class="sxs-lookup"><span data-stu-id="da0c6-191">Under **BACKUP ITEM COUNT**, select hello number of items.</span></span>

    ![Recovery Services-valv antal för virtuell dator i Azure-säkerhetskopiering objekt](./media/oracle-backup-recovery/recovery_service_13.png)

3. <span data-ttu-id="da0c6-193">På hello **myvm1** bladet, klickar du på **filåterställning (förhandsgranskning)**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-193">On hello **myvm1** blade, click **File Recovery (Preview)**.</span></span>

    ![Skärmbild av hello Recovery Services-valv återställningssidan för filen](./media/oracle-backup-recovery/recovery_service_14.png)

4. <span data-ttu-id="da0c6-195">På hello **filåterställning (förhandsgranskning)** rutan klickar du på **hämta skriptet**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-195">On hello **File Recovery (Preview)** pane, click **Download Script**.</span></span> <span data-ttu-id="da0c6-196">Spara hello download (.sh) tooa mapp på hello-klientdator.</span><span class="sxs-lookup"><span data-stu-id="da0c6-196">Then, save hello download (.sh) file tooa folder on hello client computer.</span></span>

    ![Hämta filen sparar alternativ](./media/oracle-backup-recovery/recovery_service_15.png)

5. <span data-ttu-id="da0c6-198">Kopiera hello .sh filen toohello VM.</span><span class="sxs-lookup"><span data-stu-id="da0c6-198">Copy hello .sh file toohello VM.</span></span>

    <span data-ttu-id="da0c6-199">hello som följande exempel visar hur du toouse en säker kopia (scp) kommandot toomove hello filen toohello VM.</span><span class="sxs-lookup"><span data-stu-id="da0c6-199">hello following example shows how you toouse a secure copy (scp) command toomove hello file toohello VM.</span></span> <span data-ttu-id="da0c6-200">Du kan också kopiera hello innehållet toohello Urklipp och klistra in hello innehållet om du i en ny fil som har ställts in på hello VM.</span><span class="sxs-lookup"><span data-stu-id="da0c6-200">You also can copy hello contents toohello clipboard, and then paste hello contents in a new file that is set up on hello VM.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="da0c6-201">I följande exempel hello, kontrollerar du att du uppdaterar hello IP-adress och mappen värden.</span><span class="sxs-lookup"><span data-stu-id="da0c6-201">In hello following example, ensure that you update hello IP address and folder values.</span></span> <span data-ttu-id="da0c6-202">hello-värden måste mappa toohello mappen där hello filen sparas.</span><span class="sxs-lookup"><span data-stu-id="da0c6-202">hello values must map toohello folder where hello file is saved.</span></span>

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```
6. <span data-ttu-id="da0c6-203">Ändra hello-fil, så att det ägs av hello rot.</span><span class="sxs-lookup"><span data-stu-id="da0c6-203">Change hello file, so that it's owned by hello root.</span></span>

    <span data-ttu-id="da0c6-204">I följande exempel hello, ändra hello-filen så att det ägs av hello rot.</span><span class="sxs-lookup"><span data-stu-id="da0c6-204">In hello following example, change hello file so that it's owned by hello root.</span></span> <span data-ttu-id="da0c6-205">Ändra behörigheter.</span><span class="sxs-lookup"><span data-stu-id="da0c6-205">Then, change permissions.</span></span>

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```
    <span data-ttu-id="da0c6-206">hello följande exempel visar vad som ska visas när du har kört hello föregående skript.</span><span class="sxs-lookup"><span data-stu-id="da0c6-206">hello following example shows what you should see after you run hello preceding script.</span></span> <span data-ttu-id="da0c6-207">När du uppmanas toocontinue, ange **Y**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-207">When you're prompted toocontinue, enter **Y**.</span></span>

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

7. <span data-ttu-id="da0c6-208">Åtkomst toohello monterade volymer bekräftas.</span><span class="sxs-lookup"><span data-stu-id="da0c6-208">Access toohello mounted volumes is confirmed.</span></span>

    <span data-ttu-id="da0c6-209">tooexit, ange **q**, och sök sedan efter hello monterade volymer.</span><span class="sxs-lookup"><span data-stu-id="da0c6-209">tooexit, enter **q**, and then search for hello mounted volumes.</span></span> <span data-ttu-id="da0c6-210">en lista över hello läggs volymer i en kommandotolk, ange toocreate **df -k**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-210">toocreate a list of hello added volumes, at a command prompt, enter **df -k**.</span></span>

    ![hello df -k kommando](./media/oracle-backup-recovery/recovery_service_16.png)

8. <span data-ttu-id="da0c6-212">Använd hello följande skript toocopy hello saknas filer tillbaka toohello mappar:</span><span class="sxs-lookup"><span data-stu-id="da0c6-212">Use hello following script toocopy hello missing files back toohello folders:</span></span>

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
9. <span data-ttu-id="da0c6-213">I följande skript hello, använder du RMAN toorecover hello databasen:</span><span class="sxs-lookup"><span data-stu-id="da0c6-213">In hello following script, use RMAN toorecover hello database:</span></span>

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```
    
10. <span data-ttu-id="da0c6-214">Demontera hello disk.</span><span class="sxs-lookup"><span data-stu-id="da0c6-214">Unmount hello disk.</span></span>

    <span data-ttu-id="da0c6-215">I hello Azure-portalen på hello **filåterställning (förhandsgranskning)** bladet, klickar du på **demontera diskar**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-215">In hello Azure portal, on hello **File Recovery (Preview)** blade, click **Unmount Disks**.</span></span>

    ![Demontera diskar kommando](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-hello-entire-vm"></a><span data-ttu-id="da0c6-217">Återställa hello hela VM</span><span class="sxs-lookup"><span data-stu-id="da0c6-217">Restore hello entire VM</span></span>

<span data-ttu-id="da0c6-218">I stället för att återställa hello bort filer från hello Recovery Services-valv, kan du återställa hello hela datorn.</span><span class="sxs-lookup"><span data-stu-id="da0c6-218">Instead of restoring hello deleted files from hello Recovery Services vaults, you can restore hello entire VM.</span></span>

### <a name="step-1-delete-myvm"></a><span data-ttu-id="da0c6-219">Steg 1: Ta bort myVM</span><span class="sxs-lookup"><span data-stu-id="da0c6-219">Step 1: Delete myVM</span></span>

*   <span data-ttu-id="da0c6-220">I hello Azure-portalen, går toohello **myVM1** valvet och välj sedan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-220">In hello Azure portal, go toohello **myVM1** vault, and then select **Delete**.</span></span>

    ![Valvet delete-kommandot](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-hello-vm"></a><span data-ttu-id="da0c6-222">Steg 2: Återställa hello VM</span><span class="sxs-lookup"><span data-stu-id="da0c6-222">Step 2: Recover hello VM</span></span>

1.  <span data-ttu-id="da0c6-223">Gå för**Recovery Services-valv**, och välj sedan **myVault**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-223">Go too**Recovery Services vaults**, and then select **myVault**.</span></span>

    ![myVault post](./media/oracle-backup-recovery/recover_vm_02.png)

2.  <span data-ttu-id="da0c6-225">På hello **översikt** bladet under **Säkerhetskopiera objekt**, Välj hello antal objekt.</span><span class="sxs-lookup"><span data-stu-id="da0c6-225">On hello **Overview** blade, under **Backup items**, select hello number of items.</span></span>

    ![myVault Säkerhetskopiera objekt](./media/oracle-backup-recovery/recover_vm_03.png)

3.  <span data-ttu-id="da0c6-227">På hello **säkerhetskopiering objekt (Azure virtuell dator)** bladet väljer **myvm1**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-227">On hello **Backup Items (Azure Virtual Machine)** blade, select **myvm1**.</span></span>

    ![Återställningssidan för VM](./media/oracle-backup-recovery/recover_vm_04.png)

4.  <span data-ttu-id="da0c6-229">På hello **myvm1** bladet Klicka hello knappen (**...** ) knappen och klicka sedan på **återställa VM**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-229">On hello **myvm1** blade, click hello ellipsis (**...**) button,  and then click **Restore VM**.</span></span>

    ![Restore-kommandots VM](./media/oracle-backup-recovery/recover_vm_05.png)

5.  <span data-ttu-id="da0c6-231">På hello **Välj återställningspunkt** bladet, Välj hello objekt du vill toorestore och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-231">On hello **Select restore point** blade, select hello item that you want toorestore, and then click **OK**.</span></span>

    ![Välj hello återställningspunkt](./media/oracle-backup-recovery/recover_vm_06.png)

    <span data-ttu-id="da0c6-233">Om du har aktiverat programkonsekvent säkerhetskopiering, visas en blå vertikalstreck.</span><span class="sxs-lookup"><span data-stu-id="da0c6-233">If you have enabled application-consistent backup, a vertical blue bar appears.</span></span>

6.  <span data-ttu-id="da0c6-234">På hello **Återställ konfiguration** bladet välj hello virtuella datornamnet, Välj hello resursgrupp och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-234">On hello **Restore configuration** blade, select hello virtual machine name, select hello resource group, and then click **OK**.</span></span>

    ![Återställa konfigurationsvärden](./media/oracle-backup-recovery/recover_vm_07.png)

7.  <span data-ttu-id="da0c6-236">toorestore hello VM, klicka på hello **återställa** knappen.</span><span class="sxs-lookup"><span data-stu-id="da0c6-236">toorestore hello VM, click hello **Restore** button.</span></span>

8.  <span data-ttu-id="da0c6-237">tooview hello status för hello återställningsprocessen klickar du på **jobb**, och klicka sedan på **säkerhetskopieringsjobb**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-237">tooview hello status of hello restore process, click **Jobs**, and then click **Backup Jobs**.</span></span>

    ![Säkerhetskopieringsjobb status-kommandot](./media/oracle-backup-recovery/recover_vm_08.png)

    <span data-ttu-id="da0c6-239">hello visar följande bild hello återställningsprocessen hello status:</span><span class="sxs-lookup"><span data-stu-id="da0c6-239">hello following figure shows hello status of hello restore process:</span></span>

    ![Status för hello återställningsprocessen](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-hello-public-ip-address"></a><span data-ttu-id="da0c6-241">Steg 3: Ange hello offentliga IP-adress</span><span class="sxs-lookup"><span data-stu-id="da0c6-241">Step 3: Set hello public IP address</span></span>
<span data-ttu-id="da0c6-242">Efter hello VM har återställts, ställa in hello offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="da0c6-242">After hello VM is restored, set up hello public IP address.</span></span>

1.  <span data-ttu-id="da0c6-243">Skriv i sökrutan hello **offentliga IP-adressen**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-243">In hello search box, enter **public IP address**.</span></span>

    ![Lista över offentliga IP-adresser](./media/oracle-backup-recovery/create_ip_00.png)

2.  <span data-ttu-id="da0c6-245">På hello **offentliga IP-adresser** bladet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-245">On hello **Public IP addresses** blade, click **Add**.</span></span> <span data-ttu-id="da0c6-246">På hello **skapa offentlig IP-adress** bladet för **namnet**väljer hello offentliga IP-namn.</span><span class="sxs-lookup"><span data-stu-id="da0c6-246">On hello **Create public IP address** blade, for **Name**, select hello public IP name.</span></span> <span data-ttu-id="da0c6-247">För **resursgrupp**, väljer du **använd befintlig**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-247">For **Resource group**, select **Use existing**.</span></span> <span data-ttu-id="da0c6-248">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-248">Then, click **Create**.</span></span>

    ![Skapa IP-adress](./media/oracle-backup-recovery/create_ip_01.png)

3.  <span data-ttu-id="da0c6-250">tooassociate hello offentlig IP-adress med hello nätverksgränssnittet för hello VM, söka efter och välj **myVMip**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-250">tooassociate hello public IP address with hello network interface for hello VM, search for and select **myVMip**.</span></span> <span data-ttu-id="da0c6-251">Klicka på **associera**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-251">Then, click **Associate**.</span></span>

    ![Associera IP-adress](./media/oracle-backup-recovery/create_ip_02.png)

4.  <span data-ttu-id="da0c6-253">För **resurstypen**väljer **nätverksgränssnittet**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-253">For **Resource type**, select **Network interface**.</span></span> <span data-ttu-id="da0c6-254">Välj hello nätverksgränssnitt som används av hello myVM instansen och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="da0c6-254">Select hello network interface that is used by hello myVM instance, and then click **OK**.</span></span>

    ![Välj resurstyp och NIC-värden](./media/oracle-backup-recovery/create_ip_03.png)

5.  <span data-ttu-id="da0c6-256">Sök efter och öppna hello instans av myVM portar från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="da0c6-256">Search for and open hello instance of myVM that is ported from hello portal.</span></span> <span data-ttu-id="da0c6-257">hello IP-adress som är associerad med hello VM visas på hello myVM **översikt** bladet.</span><span class="sxs-lookup"><span data-stu-id="da0c6-257">hello IP address that is associated with hello VM appears on hello myVM **Overview** blade.</span></span>

    ![Värdet för IP-adress](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-toohello-vm"></a><span data-ttu-id="da0c6-259">Steg 4: Anslut toohello VM</span><span class="sxs-lookup"><span data-stu-id="da0c6-259">Step 4: Connect toohello VM</span></span>

*   <span data-ttu-id="da0c6-260">tooconnect toohello VM, använda hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="da0c6-260">tooconnect toohello VM, use hello following script:</span></span>

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-5-test-whether-hello-database-is-accessible"></a><span data-ttu-id="da0c6-261">Steg 5: Testa om hello-databasen är tillgänglig</span><span class="sxs-lookup"><span data-stu-id="da0c6-261">Step 5: Test whether hello database is accessible</span></span>
*   <span data-ttu-id="da0c6-262">tootest tillgänglighet, Använd hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="da0c6-262">tootest accessibility, use hello following script:</span></span>

    ```bash 
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> startup
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="da0c6-263">Om hello databasen **Start** kommandot genererar ett fel, toorecover hello databasen, se [steg 6: Använd RMAN toorecover hello databasen](#step-6-optional-use-rman-to-recover-the-database).</span><span class="sxs-lookup"><span data-stu-id="da0c6-263">If hello database **startup** command generates an error, toorecover hello database, see [Step 6: Use RMAN toorecover hello database](#step-6-optional-use-rman-to-recover-the-database).</span></span>

### <a name="step-6-optional-use-rman-toorecover-hello-database"></a><span data-ttu-id="da0c6-264">Steg 6: (Valfritt) använda RMAN toorecover hello databas</span><span class="sxs-lookup"><span data-stu-id="da0c6-264">Step 6: (Optional) Use RMAN toorecover hello database</span></span>
*   <span data-ttu-id="da0c6-265">toorecover hello databas, Använd hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="da0c6-265">toorecover hello database, use hello following script:</span></span>

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

<span data-ttu-id="da0c6-266">hello säkerhetskopiering och återställning av hello Oracle 12c databasen på en Azure Linux-VM är nu klar.</span><span class="sxs-lookup"><span data-stu-id="da0c6-266">hello backup and recovery of hello Oracle Database 12c database on an Azure Linux VM is now finished.</span></span>

## <a name="delete-hello-vm"></a><span data-ttu-id="da0c6-267">Ta bort hello VM</span><span class="sxs-lookup"><span data-stu-id="da0c6-267">Delete hello VM</span></span>

<span data-ttu-id="da0c6-268">När du behöver inte längre hello VM, kan du använda hello efter kommandot tooremove hello resursgrupp, hello VM och alla relaterade resurser:</span><span class="sxs-lookup"><span data-stu-id="da0c6-268">When you no longer need hello VM, you can use hello following command tooremove hello resource group, hello VM, and all related resources:</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="da0c6-269">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da0c6-269">Next steps</span></span>

[<span data-ttu-id="da0c6-270">Självstudier: Skapa högtillgängliga virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="da0c6-270">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="da0c6-271">Utforska VM distribution Azure CLI-exempel</span><span class="sxs-lookup"><span data-stu-id="da0c6-271">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)



