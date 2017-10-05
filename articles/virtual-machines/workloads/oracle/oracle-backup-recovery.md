---
title: "Säkerhetskopiera och återställa en Oracle-databas 12c-databas på en virtuell Azure Linux-dator | Microsoft Docs"
description: "Lär dig mer om att säkerhetskopiera och återställa en databas med Oracle-databas 12c i Azure-miljön."
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
ms.openlocfilehash: 9a2293f13b90e9a4cb11b4169fad969dd622a9a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a><span data-ttu-id="c58ef-103">Säkerhetskopiera och återställa en Oracle-databas 12c-databas på en virtuell Azure Linux-dator</span><span class="sxs-lookup"><span data-stu-id="c58ef-103">Back up and recover an Oracle Database 12c database on an Azure Linux virtual machine</span></span>

<span data-ttu-id="c58ef-104">Du kan använda Azure CLI för att skapa och hantera Azure-resurser i en kommandotolk eller använda skript.</span><span class="sxs-lookup"><span data-stu-id="c58ef-104">You can use Azure CLI to create and manage Azure resources at a command prompt, or use scripts.</span></span> <span data-ttu-id="c58ef-105">Vi använder Azure CLI-skript för att distribuera en Oracle-databas 12c-databas från en avbildning för Azure Marketplace-galleriet i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c58ef-105">In this article, we use Azure CLI scripts to deploy an Oracle Database 12c database from an Azure Marketplace gallery image.</span></span>

<span data-ttu-id="c58ef-106">Innan du börjar bör du kontrollera att Azure CLI är installerad.</span><span class="sxs-lookup"><span data-stu-id="c58ef-106">Before you begin, make sure that Azure CLI is installed.</span></span> <span data-ttu-id="c58ef-107">Mer information finns i [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c58ef-107">For more information, see the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="c58ef-108">Förbered miljön</span><span class="sxs-lookup"><span data-stu-id="c58ef-108">Prepare the environment</span></span>

### <a name="step-1-prerequisites"></a><span data-ttu-id="c58ef-109">Steg 1: förutsättningar</span><span class="sxs-lookup"><span data-stu-id="c58ef-109">Step 1: Prerequisites</span></span>

*   <span data-ttu-id="c58ef-110">Du måste skapa en Linux VM som har en installerad instans av Oracle-databas 12c om du vill utföra processen för säkerhetskopiering och återställning.</span><span class="sxs-lookup"><span data-stu-id="c58ef-110">To perform the backup and recovery process, you must first create a Linux VM that has an installed instance of Oracle Database 12c.</span></span> <span data-ttu-id="c58ef-111">Marketplace-avbildning som du använder för att skapa den virtuella datorn har namnet *Oracle: Oracle-databasen-Ee:12.1.0.2:latest*.</span><span class="sxs-lookup"><span data-stu-id="c58ef-111">The Marketplace image you use to create the VM is named *Oracle:Oracle-Database-Ee:12.1.0.2:latest*.</span></span>

    <span data-ttu-id="c58ef-112">Information om hur du skapar en Oracle-databas finns i [Oracle skapa database Snabbstart](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).</span><span class="sxs-lookup"><span data-stu-id="c58ef-112">To learn how to create an Oracle database, see the [Oracle create database quickstart](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).</span></span>


### <a name="step-2-connect-to-the-vm"></a><span data-ttu-id="c58ef-113">Steg 2: Anslut till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="c58ef-113">Step 2: Connect to the VM</span></span>

*   <span data-ttu-id="c58ef-114">Om du vill skapa en SSH (Secure Shell)-session med den virtuella datorn, använder du följande kommando.</span><span class="sxs-lookup"><span data-stu-id="c58ef-114">To create a Secure Shell (SSH) session with the VM, use the following command.</span></span> <span data-ttu-id="c58ef-115">Ersätt IP-adress och värdnamn med den `publicIpAddress` värde för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c58ef-115">Replace the IP address and the host name combination with the `publicIpAddress` value for your VM.</span></span>

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-the-database"></a><span data-ttu-id="c58ef-116">Steg 3: Förbereda databasen</span><span class="sxs-lookup"><span data-stu-id="c58ef-116">Step 3: Prepare the database</span></span>

1.  <span data-ttu-id="c58ef-117">Det här steget förutsätter att du har en Oracle-instans (cdb1) som körs på en virtuell dator med namnet *myVM*.</span><span class="sxs-lookup"><span data-stu-id="c58ef-117">This step assumes that you have an Oracle instance (cdb1) that is running on a VM named *myVM*.</span></span>

    <span data-ttu-id="c58ef-118">Kör den *oracle* superanvändare rot och sedan initiera lyssnaren:</span><span class="sxs-lookup"><span data-stu-id="c58ef-118">Run the *oracle* superuser root, and then initialize the listener:</span></span>

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    Copyright (c) 1991, 2014, Oracle.  All rights reserved.

    Starting /u01/app/oracle/product/12.1.0/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Log messages written to /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))

    Connecting to (ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
    STATUS of the LISTENER
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
    The listener supports no services
    The command completed successfully
    ```

2.  <span data-ttu-id="c58ef-119">(Valfritt) Kontrollera att databasen är i läget för archive log:</span><span class="sxs-lookup"><span data-stu-id="c58ef-119">(Optional) Make sure the database is in archive log mode:</span></span>

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
3.  <span data-ttu-id="c58ef-120">(Valfritt) Skapa en tabell om du vill testa genomförandet:</span><span class="sxs-lookup"><span data-stu-id="c58ef-120">(Optional) Create a table to test the commit:</span></span>

    ```bash
    SQL> alter session set "_ORACLE_SCRIPT"=true ;
    Session altered.
    SQL> create user scott identified by tiger;
    User created.
    SQL> grant create session to scott;
    Grant succeeded.
    SQL> grant create table to scott;
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
4.  <span data-ttu-id="c58ef-121">Kontrollera eller ändra storlek och Säkerhetskopians plats:</span><span class="sxs-lookup"><span data-stu-id="c58ef-121">Verify or change the backup file location and size:</span></span>

    ```bash
    $ sqlplus / as sysdba
    SQL> show parameter db_recovery
    NAME                                 TYPE        VALUE
    ------------------------------------ ----------- ------------------------------
    db_recovery_file_dest                string      /u01/app/oracle/fast_recovery_area
    db_recovery_file_dest_size           big integer 4560M
    ```
5. <span data-ttu-id="c58ef-122">Använd Oracle Recovery Manager (RMAN) för att säkerhetskopiera databasen:</span><span class="sxs-lookup"><span data-stu-id="c58ef-122">Use Oracle Recovery Manager (RMAN) to back up the database:</span></span>

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a><span data-ttu-id="c58ef-123">Steg 4: Programkonsekvent säkerhetskopiering för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="c58ef-123">Step 4: Application-consistent backup for Linux VMs</span></span>

<span data-ttu-id="c58ef-124">Programkonsekvent säkerhetskopiering är en ny funktion i Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="c58ef-124">Application-consistent backups is a new feature in Azure Backup.</span></span> <span data-ttu-id="c58ef-125">Du kan skapa och välj skript körs före och efter VM-ögonblicksbild (före ögonblicksbild och efter ögonblicksbild).</span><span class="sxs-lookup"><span data-stu-id="c58ef-125">You can create and select scripts to execute before and after the VM snapshot (pre-snapshot and post-snapshot).</span></span>

1. <span data-ttu-id="c58ef-126">Hämta JSON-filen.</span><span class="sxs-lookup"><span data-stu-id="c58ef-126">Download the JSON file.</span></span>

    <span data-ttu-id="c58ef-127">Hämta VMSnapshotScriptPluginConfig.json från https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig.</span><span class="sxs-lookup"><span data-stu-id="c58ef-127">Download VMSnapshotScriptPluginConfig.json from https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig.</span></span> <span data-ttu-id="c58ef-128">Filinnehållet se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="c58ef-128">The file contents look similar to the following:</span></span>

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

2. <span data-ttu-id="c58ef-129">Skapa mappen /etc/azure på den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="c58ef-129">Create the /etc/azure folder on the VM:</span></span>

    ```bash
    $ sudo su -
    # mkdir /etc/azure
    # cd /etc/azure
    ```

3. <span data-ttu-id="c58ef-130">Kopiera JSON-filen.</span><span class="sxs-lookup"><span data-stu-id="c58ef-130">Copy the JSON file.</span></span>

    <span data-ttu-id="c58ef-131">Kopiera VMSnapshotScriptPluginConfig.json till mappen /etc/azure.</span><span class="sxs-lookup"><span data-stu-id="c58ef-131">Copy VMSnapshotScriptPluginConfig.json to the /etc/azure folder.</span></span>

4. <span data-ttu-id="c58ef-132">Redigera JSON-filen.</span><span class="sxs-lookup"><span data-stu-id="c58ef-132">Edit the JSON file.</span></span>

    <span data-ttu-id="c58ef-133">Redigera filen VMSnapshotScriptPluginConfig.json att inkludera den `PreScriptLocation` och `PostScriptlocation` parametrar.</span><span class="sxs-lookup"><span data-stu-id="c58ef-133">Edit the VMSnapshotScriptPluginConfig.json file to include the `PreScriptLocation` and `PostScriptlocation` parameters.</span></span> <span data-ttu-id="c58ef-134">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c58ef-134">For example:</span></span>

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

5. <span data-ttu-id="c58ef-135">Skapa inför ögonblicksbilden och efter ögonblickbild skriptfilerna.</span><span class="sxs-lookup"><span data-stu-id="c58ef-135">Create the pre-snapshot and post-snapshot script files.</span></span>

    <span data-ttu-id="c58ef-136">Här är ett exempel på skript inför ögonblicksbilden och efter ögonblicksbild för en ”kalla” säkerhetskopiering (en offlinesäkerhetskopiering, med avstängning och omstart):</span><span class="sxs-lookup"><span data-stu-id="c58ef-136">Here's an example of pre-snapshot and post-snapshot scripts for a "cold backup" (an offline backup, with shutdown and restart):</span></span>

    <span data-ttu-id="c58ef-137">För /etc/azure/pre_script.sh:</span><span class="sxs-lookup"><span data-stu-id="c58ef-137">For /etc/azure/pre_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="c58ef-138">För /etc/azure/post_script.sh:</span><span class="sxs-lookup"><span data-stu-id="c58ef-138">For /etc/azure/post_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
    ```

    <span data-ttu-id="c58ef-139">Här är ett exempel på inför ögonblicksbilden och efter ögonblickbild skript för ”varm säkerhetskopiering” (en onlinesäkerhetskopiering):</span><span class="sxs-lookup"><span data-stu-id="c58ef-139">Here's an example of pre-snapshot and post-snapshot scripts for a "hot backup" (an online backup):</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="c58ef-140">För /etc/azure/post_script.sh:</span><span class="sxs-lookup"><span data-stu-id="c58ef-140">For /etc/azure/post_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="c58ef-141">/Etc/azure/pre_script.sql, ändra i innehållet i filen enligt dina krav:</span><span class="sxs-lookup"><span data-stu-id="c58ef-141">For /etc/azure/pre_script.sql, modify the contents of the file per your requirements:</span></span>

    ```bash
    alter tablespace SYSTEM begin backup;
    ...
    ...
    alter system switch logfile;
    alter system archive log stop;
    ```

    <span data-ttu-id="c58ef-142">/Etc/azure/post_script.sql, ändra i innehållet i filen enligt dina krav:</span><span class="sxs-lookup"><span data-stu-id="c58ef-142">For /etc/azure/post_script.sql, modify the contents of the file per your requirements:</span></span>

    ```bash
    alter tablespace SYSTEM end backup;
    ...
    ...
    alter system archive log start;
    ```

6. <span data-ttu-id="c58ef-143">Ändra behörigheter för filer:</span><span class="sxs-lookup"><span data-stu-id="c58ef-143">Change file permissions:</span></span>

    ```bash
    # chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
    # chmod 700 /etc/azure/pre_script.sh
    # chmod 700 /etc/azure/post_script.sh
    ```

7. <span data-ttu-id="c58ef-144">Testa skripten.</span><span class="sxs-lookup"><span data-stu-id="c58ef-144">Test the scripts.</span></span>

    <span data-ttu-id="c58ef-145">Om du vill testa skripten först logga in som rot.</span><span class="sxs-lookup"><span data-stu-id="c58ef-145">To test the scripts, first, sign in as root.</span></span> <span data-ttu-id="c58ef-146">Kontrollera sedan att det inte finns några fel:</span><span class="sxs-lookup"><span data-stu-id="c58ef-146">Then, ensure that there are no errors:</span></span>

    ```bash
    # /etc/azure/pre_script.sh
    # /etc/azure/post_script.sh
    ```

<span data-ttu-id="c58ef-147">Mer information finns i [programkonsekvent säkerhetskopiering för virtuella Linux-datorer](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).</span><span class="sxs-lookup"><span data-stu-id="c58ef-147">For more information, see [Application-consistent backup for Linux VMs](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).</span></span>


### <a name="step-5-use-azure-recovery-services-vaults-to-back-up-the-vm"></a><span data-ttu-id="c58ef-148">Steg 5: Använd Azure Recovery Services-valv om du vill säkerhetskopiera den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="c58ef-148">Step 5: Use Azure Recovery Services vaults to back up the VM</span></span>

1.  <span data-ttu-id="c58ef-149">I Azure-portalen, söka efter **Recovery Services-valv**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-149">In the Azure portal, search for **Recovery Services vaults**.</span></span>

    ![Sidan för Recovery Services-valv](./media/oracle-backup-recovery/recovery_service_01.png)

2.  <span data-ttu-id="c58ef-151">På den **Recovery Services-valv** klickar du på bladet för att lägga till ett nytt valv **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-151">On the **Recovery Services vaults** blade, to add a new vault, click **Add**.</span></span>

    ![Recovery Services-valv lägger du till sidan](./media/oracle-backup-recovery/recovery_service_02.png)

3.  <span data-ttu-id="c58ef-153">Om du vill fortsätta klickar du på **myVault**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-153">To continue, click **myVault**.</span></span>

    ![Sidan innehåller information om Recovery Services-valv](./media/oracle-backup-recovery/recovery_service_03.png)

4.  <span data-ttu-id="c58ef-155">På den **myVault** bladet, klickar du på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-155">On the **myVault** blade, click **Backup**.</span></span>

    ![Recovery Services-valv säkerhetskopiera sida](./media/oracle-backup-recovery/recovery_service_04.png)

5.  <span data-ttu-id="c58ef-157">På den **säkerhetskopiering målet** bladet använder standardvärden för **Azure** och **virtuella**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-157">On the **Backup Goal** blade, use the default values of **Azure** and **Virtual machine**.</span></span> <span data-ttu-id="c58ef-158">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-158">Click **OK**.</span></span>

    ![Sidan innehåller information om Recovery Services-valv](./media/oracle-backup-recovery/recovery_service_05.png)

6.  <span data-ttu-id="c58ef-160">För **säkerhetskopiera princip**, använda **DefaultPolicy**, eller välj **Skapa ny princip**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-160">For **Backup policy**, use **DefaultPolicy**, or select **Create New policy**.</span></span> <span data-ttu-id="c58ef-161">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-161">Click **OK**.</span></span>

    ![Recovery Services-valv säkerhetskopiera princip detaljsida](./media/oracle-backup-recovery/recovery_service_06.png)

7.  <span data-ttu-id="c58ef-163">På den **Välj virtuella datorer** bladet Välj den **myVM1** kryssrutan och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-163">On the **Select virtual machines** blade, select the **myVM1** check box, and then click **OK**.</span></span> <span data-ttu-id="c58ef-164">Klicka på den **Aktivera säkerhetskopiering** knappen.</span><span class="sxs-lookup"><span data-stu-id="c58ef-164">Click the **Enable backup** button.</span></span>

    ![Återställningsobjekt Services valv till säkerhetskopiering detaljsida](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > <span data-ttu-id="c58ef-166">När du klickar på **Aktivera säkerhetskopiering**, säkerhetskopieringen startas inte förrän den schemalagda tiden går ut.</span><span class="sxs-lookup"><span data-stu-id="c58ef-166">After you click **Enable backup**, the backup process doesn't start until the scheduled time expires.</span></span> <span data-ttu-id="c58ef-167">Slutför nästa steg om du vill konfigurera en omedelbar säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="c58ef-167">To set up an immediate backup, complete the next step.</span></span>

8.  <span data-ttu-id="c58ef-168">På den **myVault - objekt för säkerhetskopiering** bladet under **säkerhetskopiering OBJEKTANTAL**, Välj antalet säkerhetskopiering objekt.</span><span class="sxs-lookup"><span data-stu-id="c58ef-168">On the **myVault - Backup items** blade, under **BACKUP ITEM COUNT**, select the backup item count.</span></span>

    ![Recovery Services valv myVault detaljsida](./media/oracle-backup-recovery/recovery_service_08.png)

9.  <span data-ttu-id="c58ef-170">På den **säkerhetskopiering objekt (Azure virtuell dator)** bladet till höger på sidan, klicka på ellipsknappen (**...** ) knappen och klicka sedan på **Säkerhetskopiera nu**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-170">On the **Backup Items (Azure Virtual Machine)** blade, on the right side of the page, click the ellipsis (**...**) button, and then click **Backup now**.</span></span>

    ![Recovery Services-valv säkerhetskopiering nu kommando](./media/oracle-backup-recovery/recovery_service_09.png)

10. <span data-ttu-id="c58ef-172">Klicka på den **säkerhetskopiering** knappen.</span><span class="sxs-lookup"><span data-stu-id="c58ef-172">Click the **Backup** button.</span></span> <span data-ttu-id="c58ef-173">Vänta tills säkerhetskopieringen ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="c58ef-173">Wait for the backup process to finish.</span></span> <span data-ttu-id="c58ef-174">Gå sedan till [steg 6: ta bort databasfilerna](#step-6-remove-the-database-files).</span><span class="sxs-lookup"><span data-stu-id="c58ef-174">Then, go to [Step 6: Remove the database files](#step-6-remove-the-database-files).</span></span>

    <span data-ttu-id="c58ef-175">Om du vill visa status för jobbet, klickar du på **jobb**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-175">To view the status of the backup job, click **Jobs**.</span></span>

    ![Recovery Services-valv jobbet sida](./media/oracle-backup-recovery/recovery_service_10.png)

    <span data-ttu-id="c58ef-177">Status för jobbet visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="c58ef-177">The status of the backup job appears in the following image:</span></span>

    ![Recovery Services-valv jobbet sida med status](./media/oracle-backup-recovery/recovery_service_11.png)

11. <span data-ttu-id="c58ef-179">Åtgärda eventuella fel i loggfilen för en programkonsekvent säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="c58ef-179">For an application-consistent backup, address any errors in the log file.</span></span> <span data-ttu-id="c58ef-180">Loggfilen finns i /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.</span><span class="sxs-lookup"><span data-stu-id="c58ef-180">The log file is located at /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.</span></span>

### <a name="step-6-remove-the-database-files"></a><span data-ttu-id="c58ef-181">Steg 6: Ta bort databasfilerna</span><span class="sxs-lookup"><span data-stu-id="c58ef-181">Step 6: Remove the database files</span></span> 
<span data-ttu-id="c58ef-182">Senare i den här artikeln lär du dig hur du testar återställningsprocessen.</span><span class="sxs-lookup"><span data-stu-id="c58ef-182">Later in this article, you'll learn how to test the recovery process.</span></span> <span data-ttu-id="c58ef-183">Innan du kan testa återställningsprocessen måste du ta bort databasfilerna.</span><span class="sxs-lookup"><span data-stu-id="c58ef-183">Before you can test the recovery process, you have to remove the database files.</span></span>

1.  <span data-ttu-id="c58ef-184">Ta bort filer tabellutrymmet och säkerhetskopiering:</span><span class="sxs-lookup"><span data-stu-id="c58ef-184">Remove the tablespace and backup files:</span></span>

    ```bash
    $ sudo su - oracle
    $ cd /u01/app/oracle/oradata/cdb1
    $ rm -f *.dbf
    $ cd /u01/app/oracle/fast_recovery_area/CDB1/backupset
    $ rm -rf *
    ```
    
2.  <span data-ttu-id="c58ef-185">(Valfritt) Stänga av Oracle-instansen:</span><span class="sxs-lookup"><span data-stu-id="c58ef-185">(Optional) Shut down the Oracle instance:</span></span>

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-the-deleted-files-from-the-recovery-services-vaults"></a><span data-ttu-id="c58ef-186">Återställa borttagna filer från Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="c58ef-186">Restore the deleted files from the Recovery Services vaults</span></span>
<span data-ttu-id="c58ef-187">Om du vill återställa borttagna filer, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="c58ef-187">To restore the deleted files, complete the following steps:</span></span>

1. <span data-ttu-id="c58ef-188">I Azure-portalen, söka efter den *myVault* Recovery Services-valv objektet.</span><span class="sxs-lookup"><span data-stu-id="c58ef-188">In the Azure portal, search for the *myVault* Recovery Services vaults item.</span></span> <span data-ttu-id="c58ef-189">På den **översikt** bladet under **Säkerhetskopiera objekt**, Välj antalet objekt.</span><span class="sxs-lookup"><span data-stu-id="c58ef-189">On the **Overview** blade, under **Backup items**, select the number of items.</span></span>

    ![Recovery Services valv myVault Säkerhetskopiera objekt](./media/oracle-backup-recovery/recovery_service_12.png)

2. <span data-ttu-id="c58ef-191">Under **säkerhetskopiering OBJEKTANTAL**, Välj antalet objekt.</span><span class="sxs-lookup"><span data-stu-id="c58ef-191">Under **BACKUP ITEM COUNT**, select the number of items.</span></span>

    ![Recovery Services-valv antal för virtuell dator i Azure-säkerhetskopiering objekt](./media/oracle-backup-recovery/recovery_service_13.png)

3. <span data-ttu-id="c58ef-193">På den **myvm1** bladet, klickar du på **filåterställning (förhandsgranskning)**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-193">On the **myvm1** blade, click **File Recovery (Preview)**.</span></span>

    ![Skärmbild av Recovery Services-valv återställningssidan för filen](./media/oracle-backup-recovery/recovery_service_14.png)

4. <span data-ttu-id="c58ef-195">På den **filåterställning (förhandsgranskning)** rutan klickar du på **hämta skriptet**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-195">On the **File Recovery (Preview)** pane, click **Download Script**.</span></span> <span data-ttu-id="c58ef-196">Spara sedan filen download (.sh) till en mapp på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="c58ef-196">Then, save the download (.sh) file to a folder on the client computer.</span></span>

    ![Hämta filen sparar alternativ](./media/oracle-backup-recovery/recovery_service_15.png)

5. <span data-ttu-id="c58ef-198">Kopiera filen .sh till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c58ef-198">Copy the .sh file to the VM.</span></span>

    <span data-ttu-id="c58ef-199">I följande exempel visas hur du kan använda en säker kopia (scp) kommandot för att flytta filen till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c58ef-199">The following example shows how you to use a secure copy (scp) command to move the file to the VM.</span></span> <span data-ttu-id="c58ef-200">Du kan också kopiera innehållet i Urklipp och klistra in innehållet i en ny fil som har ställts in på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c58ef-200">You also can copy the contents to the clipboard, and then paste the contents in a new file that is set up on the VM.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c58ef-201">Se till att du uppdaterar IP-adress och mappen värdena i exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="c58ef-201">In the following example, ensure that you update the IP address and folder values.</span></span> <span data-ttu-id="c58ef-202">Värdena måste mappas till mappen där filen sparas.</span><span class="sxs-lookup"><span data-stu-id="c58ef-202">The values must map to the folder where the file is saved.</span></span>

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```
6. <span data-ttu-id="c58ef-203">Ändra filen, så att det ägs av roten.</span><span class="sxs-lookup"><span data-stu-id="c58ef-203">Change the file, so that it's owned by the root.</span></span>

    <span data-ttu-id="c58ef-204">I följande exempel visas att ändra filen så att det ägs av roten.</span><span class="sxs-lookup"><span data-stu-id="c58ef-204">In the following example, change the file so that it's owned by the root.</span></span> <span data-ttu-id="c58ef-205">Ändra behörigheter.</span><span class="sxs-lookup"><span data-stu-id="c58ef-205">Then, change permissions.</span></span>

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```
    <span data-ttu-id="c58ef-206">I följande exempel visar vad som ska visas när du har kört skriptet.</span><span class="sxs-lookup"><span data-stu-id="c58ef-206">The following example shows what you should see after you run the preceding script.</span></span> <span data-ttu-id="c58ef-207">När du uppmanas att fortsätta ange **Y**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-207">When you're prompted to continue, enter **Y**.</span></span>

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    The script requires 'open-iscsi' and 'lshw' to run.
    Do you want us to install 'open-iscsi' and 'lshw' on this machine?
    Please press 'Y' to continue with installation, 'N' to abort the operation. : Y
    Installing 'open-iscsi'....
    Installing 'lshw'....

    Connecting to recovery point using ISCSI service...

    Connection succeeded!

    Please wait while we attach volumes of the recovery point to this machine...

    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath

    1)  | /dev/sde  |  /dev/sde1  |  /root/myVM-20170517093913/Volume1

    2)  | /dev/sde  |  /dev/sde2  |  /root/myVM-20170517093913/Volume2

    ************ Open File Explorer to browse for files. ************

    After recovery, to remove the disks and close the connection to the recovery point, please click 'Unmount Disks' in step 3 of the portal.

    Please enter 'q/Q' to exit...
    ```

7. <span data-ttu-id="c58ef-208">Åtkomst till de monterade volymerna bekräftas.</span><span class="sxs-lookup"><span data-stu-id="c58ef-208">Access to the mounted volumes is confirmed.</span></span>

    <span data-ttu-id="c58ef-209">Om du vill avsluta, ange **q**, och sök sedan efter de monterade volymerna.</span><span class="sxs-lookup"><span data-stu-id="c58ef-209">To exit, enter **q**, and then search for the mounted volumes.</span></span> <span data-ttu-id="c58ef-210">Om du vill skapa en lista över volymer som lagts till i en kommandotolk, ange **df -k**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-210">To create a list of the added volumes, at a command prompt, enter **df -k**.</span></span>

    ![Kommandot df -k](./media/oracle-backup-recovery/recovery_service_16.png)

8. <span data-ttu-id="c58ef-212">Använd följande skript om du vill kopiera de saknade filerna till mapparna:</span><span class="sxs-lookup"><span data-stu-id="c58ef-212">Use the following script to copy the missing files back to the folders:</span></span>

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
9. <span data-ttu-id="c58ef-213">Använd RMAN ska kunna återställa databasen i följande skript:</span><span class="sxs-lookup"><span data-stu-id="c58ef-213">In the following script, use RMAN to recover the database:</span></span>

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```
    
10. <span data-ttu-id="c58ef-214">Demontera disken.</span><span class="sxs-lookup"><span data-stu-id="c58ef-214">Unmount the disk.</span></span>

    <span data-ttu-id="c58ef-215">I Azure-portalen på den **filåterställning (förhandsgranskning)** bladet, klickar du på **demontera diskar**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-215">In the Azure portal, on the **File Recovery (Preview)** blade, click **Unmount Disks**.</span></span>

    ![Demontera diskar kommando](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-the-entire-vm"></a><span data-ttu-id="c58ef-217">Återställa hela den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="c58ef-217">Restore the entire VM</span></span>

<span data-ttu-id="c58ef-218">Du kan återställa hela den virtuella datorn i stället för att återställa borttagna filer från Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="c58ef-218">Instead of restoring the deleted files from the Recovery Services vaults, you can restore the entire VM.</span></span>

### <a name="step-1-delete-myvm"></a><span data-ttu-id="c58ef-219">Steg 1: Ta bort myVM</span><span class="sxs-lookup"><span data-stu-id="c58ef-219">Step 1: Delete myVM</span></span>

*   <span data-ttu-id="c58ef-220">I Azure-portalen går du till den **myVM1** valvet och välj sedan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-220">In the Azure portal, go to the **myVM1** vault, and then select **Delete**.</span></span>

    ![Valvet delete-kommandot](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-the-vm"></a><span data-ttu-id="c58ef-222">Steg 2: Återställa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="c58ef-222">Step 2: Recover the VM</span></span>

1.  <span data-ttu-id="c58ef-223">Gå till **Recovery Services-valv**, och välj sedan **myVault**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-223">Go to **Recovery Services vaults**, and then select **myVault**.</span></span>

    ![myVault post](./media/oracle-backup-recovery/recover_vm_02.png)

2.  <span data-ttu-id="c58ef-225">På den **översikt** bladet under **Säkerhetskopiera objekt**, Välj antalet objekt.</span><span class="sxs-lookup"><span data-stu-id="c58ef-225">On the **Overview** blade, under **Backup items**, select the number of items.</span></span>

    ![myVault Säkerhetskopiera objekt](./media/oracle-backup-recovery/recover_vm_03.png)

3.  <span data-ttu-id="c58ef-227">På den **säkerhetskopiering objekt (Azure virtuell dator)** bladet väljer **myvm1**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-227">On the **Backup Items (Azure Virtual Machine)** blade, select **myvm1**.</span></span>

    ![Återställningssidan för VM](./media/oracle-backup-recovery/recover_vm_04.png)

4.  <span data-ttu-id="c58ef-229">På den **myvm1** bladet Klicka på ellipsknappen (**...** ) knappen och klicka sedan på **återställa VM**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-229">On the **myvm1** blade, click the ellipsis (**...**) button,  and then click **Restore VM**.</span></span>

    ![Restore-kommandots VM](./media/oracle-backup-recovery/recover_vm_05.png)

5.  <span data-ttu-id="c58ef-231">På den **Välj återställningspunkt** bladet, Välj det objekt som du vill återställa och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-231">On the **Select restore point** blade, select the item that you want to restore, and then click **OK**.</span></span>

    ![Välj återställningspunkten](./media/oracle-backup-recovery/recover_vm_06.png)

    <span data-ttu-id="c58ef-233">Om du har aktiverat programkonsekvent säkerhetskopiering, visas en blå vertikalstreck.</span><span class="sxs-lookup"><span data-stu-id="c58ef-233">If you have enabled application-consistent backup, a vertical blue bar appears.</span></span>

6.  <span data-ttu-id="c58ef-234">På den **Återställ konfiguration** bladet välj namnet på virtuella datorn, välja en resursgrupp och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-234">On the **Restore configuration** blade, select the virtual machine name, select the resource group, and then click **OK**.</span></span>

    ![Återställa konfigurationsvärden](./media/oracle-backup-recovery/recover_vm_07.png)

7.  <span data-ttu-id="c58ef-236">Om du vill återställa den virtuella datorn, klickar du på den **återställa** knappen.</span><span class="sxs-lookup"><span data-stu-id="c58ef-236">To restore the VM, click the **Restore** button.</span></span>

8.  <span data-ttu-id="c58ef-237">Om du vill visa status för återställningen klickar du på **jobb**, och klicka sedan på **säkerhetskopieringsjobb**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-237">To view the status of the restore process, click **Jobs**, and then click **Backup Jobs**.</span></span>

    ![Säkerhetskopieringsjobb status-kommandot](./media/oracle-backup-recovery/recover_vm_08.png)

    <span data-ttu-id="c58ef-239">Följande bild visar status för återställningen:</span><span class="sxs-lookup"><span data-stu-id="c58ef-239">The following figure shows the status of the restore process:</span></span>

    ![Status för återställningen](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-the-public-ip-address"></a><span data-ttu-id="c58ef-241">Steg 3: Ange den offentliga IP-adressen</span><span class="sxs-lookup"><span data-stu-id="c58ef-241">Step 3: Set the public IP address</span></span>
<span data-ttu-id="c58ef-242">När den virtuella datorn har återställts kan ställa in den offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="c58ef-242">After the VM is restored, set up the public IP address.</span></span>

1.  <span data-ttu-id="c58ef-243">I sökrutan anger **offentliga IP-adressen**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-243">In the search box, enter **public IP address**.</span></span>

    ![Lista över offentliga IP-adresser](./media/oracle-backup-recovery/create_ip_00.png)

2.  <span data-ttu-id="c58ef-245">På den **offentliga IP-adresser** bladet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-245">On the **Public IP addresses** blade, click **Add**.</span></span> <span data-ttu-id="c58ef-246">På den **skapa offentlig IP-adress** bladet för **namn**, Välj offentlig IP-namnet.</span><span class="sxs-lookup"><span data-stu-id="c58ef-246">On the **Create public IP address** blade, for **Name**, select the public IP name.</span></span> <span data-ttu-id="c58ef-247">För **resursgrupp**, väljer du **använd befintlig**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-247">For **Resource group**, select **Use existing**.</span></span> <span data-ttu-id="c58ef-248">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-248">Then, click **Create**.</span></span>

    ![Skapa IP-adress](./media/oracle-backup-recovery/create_ip_01.png)

3.  <span data-ttu-id="c58ef-250">Om du vill associera den offentliga IP-adressen till nätverksgränssnittet för den virtuella datorn, leta upp och markera **myVMip**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-250">To associate the public IP address with the network interface for the VM, search for and select **myVMip**.</span></span> <span data-ttu-id="c58ef-251">Klicka på **associera**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-251">Then, click **Associate**.</span></span>

    ![Associera IP-adress](./media/oracle-backup-recovery/create_ip_02.png)

4.  <span data-ttu-id="c58ef-253">För **resurstypen**väljer **nätverksgränssnittet**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-253">For **Resource type**, select **Network interface**.</span></span> <span data-ttu-id="c58ef-254">Välj det nätverksgränssnitt som används av myVM-instansen och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c58ef-254">Select the network interface that is used by the myVM instance, and then click **OK**.</span></span>

    ![Välj resurstyp och NIC-värden](./media/oracle-backup-recovery/create_ip_03.png)

5.  <span data-ttu-id="c58ef-256">Söka efter och öppna instansen av myVM är portar från portalen.</span><span class="sxs-lookup"><span data-stu-id="c58ef-256">Search for and open the instance of myVM that is ported from the portal.</span></span> <span data-ttu-id="c58ef-257">IP-adressen som är associerad med den virtuella datorn visas på myVM **översikt** bladet.</span><span class="sxs-lookup"><span data-stu-id="c58ef-257">The IP address that is associated with the VM appears on the myVM **Overview** blade.</span></span>

    ![Värdet för IP-adress](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-to-the-vm"></a><span data-ttu-id="c58ef-259">Steg 4: Anslut till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="c58ef-259">Step 4: Connect to the VM</span></span>

*   <span data-ttu-id="c58ef-260">Använd följande skript för att ansluta till den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="c58ef-260">To connect to the VM, use the following script:</span></span>

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-5-test-whether-the-database-is-accessible"></a><span data-ttu-id="c58ef-261">Steg 5: Testa om databasen är tillgänglig</span><span class="sxs-lookup"><span data-stu-id="c58ef-261">Step 5: Test whether the database is accessible</span></span>
*   <span data-ttu-id="c58ef-262">Testa hjälpmedel genom att använda följande skript:</span><span class="sxs-lookup"><span data-stu-id="c58ef-262">To test accessibility, use the following script:</span></span>

    ```bash 
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> startup
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="c58ef-263">Om databasen **Start** kommandot genererar ett fel, om du vill återställa databasen finns [steg 6: Använd RMAN ska kunna återställa databasen](#step-6-optional-use-rman-to-recover-the-database).</span><span class="sxs-lookup"><span data-stu-id="c58ef-263">If the database **startup** command generates an error, to recover the database, see [Step 6: Use RMAN to recover the database](#step-6-optional-use-rman-to-recover-the-database).</span></span>

### <a name="step-6-optional-use-rman-to-recover-the-database"></a><span data-ttu-id="c58ef-264">Steg 6: (Valfritt) använda RMAN ska kunna återställa databasen</span><span class="sxs-lookup"><span data-stu-id="c58ef-264">Step 6: (Optional) Use RMAN to recover the database</span></span>
*   <span data-ttu-id="c58ef-265">Om du vill återställa databasen använder du följande skript:</span><span class="sxs-lookup"><span data-stu-id="c58ef-265">To recover the database, use the following script:</span></span>

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

<span data-ttu-id="c58ef-266">Säkerhetskopiering och återställning av databasen på 12c Oracle-databas på en Azure Linux-VM är nu klar.</span><span class="sxs-lookup"><span data-stu-id="c58ef-266">The backup and recovery of the Oracle Database 12c database on an Azure Linux VM is now finished.</span></span>

## <a name="delete-the-vm"></a><span data-ttu-id="c58ef-267">Ta bort den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="c58ef-267">Delete the VM</span></span>

<span data-ttu-id="c58ef-268">Du kan använda följande kommando för att ta bort resursgruppen, den virtuella datorn och alla relaterade resurser när du inte längre behöver den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="c58ef-268">When you no longer need the VM, you can use the following command to remove the resource group, the VM, and all related resources:</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="c58ef-269">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c58ef-269">Next steps</span></span>

[<span data-ttu-id="c58ef-270">Självstudier: Skapa högtillgängliga virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c58ef-270">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="c58ef-271">Utforska VM distribution Azure CLI-exempel</span><span class="sxs-lookup"><span data-stu-id="c58ef-271">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)



