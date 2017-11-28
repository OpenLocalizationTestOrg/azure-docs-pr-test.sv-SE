---
title: aaaCreate en Oracle-databas i en Azure VM | Microsoft Docs
description: "Snabbt en Oracle-databas 12c-databas och körs i Azure-miljön."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/17/2017
ms.author: rclaus
ms.openlocfilehash: 83205154c3275d5f57b46c8acfb0cb4e5c68a412
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-oracle-database-in-an-azure-vm"></a><span data-ttu-id="20c09-103">Skapa en Oracle-databas i en virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="20c09-103">Create an Oracle Database in an Azure VM</span></span>

<span data-ttu-id="20c09-104">Den här guiden information med hjälp av hello Azure CLI toodeploy en virtuell Azure-dator från hello [Oracle marketplace-avbildning i galleriet](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) ordning toocreate en Oracle 12 c-databas.</span><span class="sxs-lookup"><span data-stu-id="20c09-104">This guide details using hello Azure CLI toodeploy an Azure virtual machine from hello [Oracle marketplace gallery image](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) in order toocreate an Oracle 12c database.</span></span> <span data-ttu-id="20c09-105">När hello server har distribuerats, ansluter du via SSH i ordning tooconfigure hello Oracle-databas.</span><span class="sxs-lookup"><span data-stu-id="20c09-105">Once hello server is deployed, you will connect via SSH in order tooconfigure hello Oracle database.</span></span> 

<span data-ttu-id="20c09-106">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="20c09-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="20c09-107">Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="20c09-107">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="20c09-108">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="20c09-108">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="20c09-109">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="20c09-109">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="20c09-110">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="20c09-110">Create a resource group</span></span>

<span data-ttu-id="20c09-111">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="20c09-111">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="20c09-112">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="20c09-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="20c09-113">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.</span><span class="sxs-lookup"><span data-stu-id="20c09-113">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
## <a name="create-virtual-machine"></a><span data-ttu-id="20c09-114">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="20c09-114">Create virtual machine</span></span>

<span data-ttu-id="20c09-115">toocreate en virtuell dator (VM), Använd hello [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="20c09-115">toocreate a virtual machine (VM), use hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="20c09-116">hello följande exempel skapas en virtuell dator med namnet `myVM`.</span><span class="sxs-lookup"><span data-stu-id="20c09-116">hello following example creates a VM named `myVM`.</span></span> <span data-ttu-id="20c09-117">Det skapar också SSH-nycklar, om de inte redan finns på standardplatsen nyckel.</span><span class="sxs-lookup"><span data-stu-id="20c09-117">It also creates SSH keys, if they do not already exist in a default key location.</span></span> <span data-ttu-id="20c09-118">toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.</span><span class="sxs-lookup"><span data-stu-id="20c09-118">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="20c09-119">När du har skapat hello VM visar Azure CLI information liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="20c09-119">After you create hello VM, Azure CLI displays information similar toohello following example.</span></span> <span data-ttu-id="20c09-120">Observera hello värde för `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="20c09-120">Note hello value for `publicIpAddress`.</span></span> <span data-ttu-id="20c09-121">Du kan använda den här adressen tooaccess hello VM.</span><span class="sxs-lookup"><span data-stu-id="20c09-121">You use this address tooaccess hello VM.</span></span>

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/{snip}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="connect-toohello-vm"></a><span data-ttu-id="20c09-122">Ansluta toohello VM</span><span class="sxs-lookup"><span data-stu-id="20c09-122">Connect toohello VM</span></span>

<span data-ttu-id="20c09-123">toocreate en SSH-session med hello VM, använda hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="20c09-123">toocreate an SSH session with hello VM, use hello following command.</span></span> <span data-ttu-id="20c09-124">Ersätt hello IP-adress med hello `publicIpAddress` värde för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="20c09-124">Replace hello IP address with hello `publicIpAddress` value for your VM.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="create-hello-database"></a><span data-ttu-id="20c09-125">Skapa hello-databas</span><span class="sxs-lookup"><span data-stu-id="20c09-125">Create hello database</span></span>

<span data-ttu-id="20c09-126">hello Oracle-programvara är installerad på hello Marketplace-avbildning.</span><span class="sxs-lookup"><span data-stu-id="20c09-126">hello Oracle software is already installed on hello Marketplace image.</span></span> <span data-ttu-id="20c09-127">Skapa en exempeldatabas på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="20c09-127">Create a sample database as follows.</span></span> 

1.  <span data-ttu-id="20c09-128">Växla toohello *oracle* superanvändare och sedan initiera hello-lyssnare för loggning:</span><span class="sxs-lookup"><span data-stu-id="20c09-128">Switch toohello *oracle* superuser, then initialize hello listener for logging:</span></span>

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    ```

    <span data-ttu-id="20c09-129">hello-utdata är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="20c09-129">hello output is similar toohello following:</span></span>

    ```bash
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

2.  <span data-ttu-id="20c09-130">Skapa databas för hello:</span><span class="sxs-lookup"><span data-stu-id="20c09-130">Create hello database:</span></span>

    ```bash
    dbca -silent \
           -createDatabase \
           -templateName General_Purpose.dbc \
           -gdbname cdb1 \
           -sid cdb1 \
           -responseFile NO_VALUE \
           -characterSet AL32UTF8 \
           -sysPassword OraPasswd1 \
           -systemPassword OraPasswd1 \
           -createAsContainerDatabase true \
           -numberOfPDBs 1 \
           -pdbName pdb1 \
           -pdbAdminPassword OraPasswd1 \
           -databaseType MULTIPURPOSE \
           -automaticMemoryManagement false \
           -storageType FS \
           -ignorePreReqs
    ```

    <span data-ttu-id="20c09-131">Det tar några minuter toocreate hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="20c09-131">It takes a few minutes toocreate hello database.</span></span>

3. <span data-ttu-id="20c09-132">Ange variabler för Oracle</span><span class="sxs-lookup"><span data-stu-id="20c09-132">Set Oracle variables</span></span>

<span data-ttu-id="20c09-133">Innan du ansluter du behöver tooset två miljövariabler: *ORACLE_HOME* och *ORACLE_SID*.</span><span class="sxs-lookup"><span data-stu-id="20c09-133">Before you connect, you need tooset two environment variables: *ORACLE_HOME* and *ORACLE_SID*.</span></span>

```bash
ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
ORACLE_SID=cdb1; export ORACLE_SID
```
<span data-ttu-id="20c09-134">Du kan också lägga till ORACLE_HOME och ORACLE_SID variabler toohello .bashrc fil.</span><span class="sxs-lookup"><span data-stu-id="20c09-134">You also can add ORACLE_HOME and ORACLE_SID variables toohello .bashrc file.</span></span> <span data-ttu-id="20c09-135">Detta skulle spara hello miljövariabler för framtida inloggningar. Bekräfta hello följande instruktioner har lagts till toohello `~/.bashrc` fil med valfri redigerare.</span><span class="sxs-lookup"><span data-stu-id="20c09-135">This would save hello environment variables for future sign-ins. Confirm hello following statements have been added toohello `~/.bashrc` file using editor of your choice.</span></span>

```bash
# Add ORACLE_HOME. 
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1 
# Add ORACLE_SID. 
export ORACLE_SID=cdb1 
```

## <a name="oracle-em-express-connectivity"></a><span data-ttu-id="20c09-136">Oracle EM snabb anslutning</span><span class="sxs-lookup"><span data-stu-id="20c09-136">Oracle EM Express connectivity</span></span>

<span data-ttu-id="20c09-137">För ett GUI-hanteringsverktyg som du kan använda tooexplore hello databas, ställa in Oracle EM Express.</span><span class="sxs-lookup"><span data-stu-id="20c09-137">For a GUI management tool that you can use tooexplore hello database, set up Oracle EM Express.</span></span> <span data-ttu-id="20c09-138">tooconnect tooOracle EM Express, du måste först konfigurera hello port i Oracle.</span><span class="sxs-lookup"><span data-stu-id="20c09-138">tooconnect tooOracle EM Express, you must first set up hello port in Oracle.</span></span> 

1. <span data-ttu-id="20c09-139">Anslut tooyour databasen med hjälp av sqlplus:</span><span class="sxs-lookup"><span data-stu-id="20c09-139">Connect tooyour database using sqlplus:</span></span>

    ```bash
    sqlplus / as sysdba
    ```

2. <span data-ttu-id="20c09-140">När du är ansluten, ange hello port 5502 för EM Express</span><span class="sxs-lookup"><span data-stu-id="20c09-140">Once connected, set hello port 5502 for EM Express</span></span>

    ```bash
    exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);
    ```

3. <span data-ttu-id="20c09-141">Öppna hello behållaren PDB1 om inte redan öppnad men första hello statusen:</span><span class="sxs-lookup"><span data-stu-id="20c09-141">Open hello container PDB1 if not already opened, but first check hello status:</span></span>

    ```bash
    select con_id, name, open_mode from v$pdbs;
    ```

    <span data-ttu-id="20c09-142">hello-utdata är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="20c09-142">hello output is similar toohello following:</span></span>

    ```bash
      CON_ID NAME                           OPEN_MODE 
      ----------- ------------------------- ---------- 
      2           PDB$SEED                  READ ONLY 
      3           PDB1                      MOUNT
    ```

4. <span data-ttu-id="20c09-143">Om hello OPEN_MODE för `PDB1` är inte läsa skriva, köra hello följande kommandon tooopen PDB1:</span><span class="sxs-lookup"><span data-stu-id="20c09-143">If hello OPEN_MODE for `PDB1` is not READ WRITE, then run hello followings commands tooopen PDB1:</span></span>

   ```bash
    alter session set container=pdb1;
    alter database open;
   ```

<span data-ttu-id="20c09-144">Du behöver tootype `quit` tooend hello sqlplus session och skriv `exit` toologout av hello oracle-användare.</span><span class="sxs-lookup"><span data-stu-id="20c09-144">You need tootype `quit` tooend hello sqlplus session and type `exit` toologout of hello oracle user.</span></span>

## <a name="automate-database-startup-and-shutdown"></a><span data-ttu-id="20c09-145">Automatisera databasen start och stopp</span><span class="sxs-lookup"><span data-stu-id="20c09-145">Automate database startup and shutdown</span></span>

<span data-ttu-id="20c09-146">hello Oracle-databas som standard startas inte automatiskt när du startar om hello VM.</span><span class="sxs-lookup"><span data-stu-id="20c09-146">hello Oracle database by default doesn't automatically start when you restart hello VM.</span></span> <span data-ttu-id="20c09-147">tooset in hello Oracle-databasen toostart automatiskt, först logga in som rot.</span><span class="sxs-lookup"><span data-stu-id="20c09-147">tooset up hello Oracle database toostart automatically, first sign in as root.</span></span> <span data-ttu-id="20c09-148">Sedan skapa och uppdatera vissa systemfiler.</span><span class="sxs-lookup"><span data-stu-id="20c09-148">Then, create and update some system files.</span></span>

1. <span data-ttu-id="20c09-149">Logga in som rot</span><span class="sxs-lookup"><span data-stu-id="20c09-149">Sign on as root</span></span>
    ```bash
    sudo su -
    ```

2.  <span data-ttu-id="20c09-150">Med redigeringsprogram favorit redigerar hello filen `/etc/oratab` och ändra hello standard `N` för`Y`:</span><span class="sxs-lookup"><span data-stu-id="20c09-150">Using your favorite editor, edit hello file `/etc/oratab` and change hello default `N` too`Y`:</span></span>

    ```bash
    cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y
    ```

3.  <span data-ttu-id="20c09-151">Skapa en fil med namnet `/etc/init.d/dbora` och klistra in hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="20c09-151">Create a file named `/etc/init.d/dbora` and paste hello following contents:</span></span>

    ```
    #!/bin/sh
    # chkconfig: 345 99 10
    # Description: Oracle auto start-stop script.
    #
    # Set ORA_HOME toobe equivalent too$ORACLE_HOME.
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle

    case "$1" in
    'start')
        # Start hello Oracle databases:
        # hello following command assumes that hello Oracle sign-in
        # will not prompt hello user for any values.
        # Remove "&" if you don't want startup as a background process.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" &
        touch /var/lock/subsys/dbora
        ;;

    'stop')
        # Stop hello Oracle databases:
        # hello following command assumes that hello Oracle sign-in
        # will not prompt hello user for any values.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" &
        rm -f /var/lock/subsys/dbora
        ;;
    esac
    ```

4.  <span data-ttu-id="20c09-152">Ändra behörigheter för filer med *chmod* på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="20c09-152">Change permissions on files with *chmod* as follows:</span></span>

    ```bash
    chgrp dba /etc/init.d/dbora
    chmod 750 /etc/init.d/dbora
    ```

5.  <span data-ttu-id="20c09-153">Skapa symboliska länkar för start och stopp enligt följande:</span><span class="sxs-lookup"><span data-stu-id="20c09-153">Create symbolic links for startup and shutdown as follows:</span></span>

    ```bash
    ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  <span data-ttu-id="20c09-154">tootest ändringarna, starta om hello VM:</span><span class="sxs-lookup"><span data-stu-id="20c09-154">tootest your changes, restart hello VM:</span></span>

    ```bash
    reboot
    ```

## <a name="open-ports-for-connectivity"></a><span data-ttu-id="20c09-155">Öppna portar för anslutning</span><span class="sxs-lookup"><span data-stu-id="20c09-155">Open ports for connectivity</span></span>

<span data-ttu-id="20c09-156">hello sista aktivitet är tooconfigure vissa externa slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="20c09-156">hello final task is tooconfigure some external endpoints.</span></span> <span data-ttu-id="20c09-157">tooset in hello Azure Nätverkssäkerhetsgruppen som skyddar hello VM, avsluta SSH-sessionen i hello VM (bör ha har dessutom out-of-SSH när omstart i föregående steg).</span><span class="sxs-lookup"><span data-stu-id="20c09-157">tooset up hello Azure Network Security Group that protects hello VM, first exit your SSH session in hello VM (should have been kicked out of SSH when rebooting in previous step).</span></span> 

1.  <span data-ttu-id="20c09-158">tooopen hello endpoint att du använder tooaccess hello Oracle-databas, skapar en Nätverkssäkerhetsgrupp regel med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="20c09-158">tooopen hello endpoint that you use tooaccess hello Oracle database remotely, create a Network Security Group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) as follows:</span></span> 

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup\
        --nsg-name myVmNSG \
        --name allow-oracle \
        --protocol tcp \
        --priority 1001 \
        --destination-port-range 1521
    ```

2.  <span data-ttu-id="20c09-159">tooopen hello endpoint att du använder tooaccess Oracle EM Express via fjärranslutning, skapar en Nätverkssäkerhetsgrupp regel med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="20c09-159">tooopen hello endpoint that you use tooaccess Oracle EM Express remotely, create a Network Security Group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) as follows:</span></span>

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup \
        --nsg-name myVmNSG \
        --name allow-oracle-EM \
        --protocol tcp \
        --priority 1002 \
        --destination-port-range 5502
    ```

3. <span data-ttu-id="20c09-160">Om det behövs, hämta hello offentliga IP-adressen för den virtuella datorn igen med [az nätverket offentliga ip-visa](/cli/azure/network/public-ip#show) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="20c09-160">If needed, obtain hello public IP address of your VM again with [az network public-ip show](/cli/azure/network/public-ip#show) as follows:</span></span>

    ```azurecli-interactive
    az network public-ip show \
        --resource-group myResourceGroup \
        --name myVMPublicIP \
        --query [ipAddress] \
        --output tsv
    ```

4.  <span data-ttu-id="20c09-161">Ansluta EM Express från din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="20c09-161">Connect EM Express from your browser.</span></span> <span data-ttu-id="20c09-162">Kontrollera att din webbläsare är kompatibelt med EM Express (Flash installation krävs):</span><span class="sxs-lookup"><span data-stu-id="20c09-162">Make sure your browser is compatible with EM Express (Flash install is required):</span></span> 

    ```
    https://<VM ip address or hostname>:5502/em
    ```

<span data-ttu-id="20c09-163">Du kan logga in med hjälp av hello **SYS** konto och kontrollera hello **som sysdba** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="20c09-163">You can log in by using hello **SYS** account, and check hello **as sysdba** checkbox.</span></span> <span data-ttu-id="20c09-164">Använd hello lösenord **OraPasswd1** som du anger under installationen.</span><span class="sxs-lookup"><span data-stu-id="20c09-164">Use hello password **OraPasswd1** that you set during installation.</span></span> 

![Skärmbild av inloggningssidan för hello Oracle OEM Express](./media/oracle-quick-start/oracle_oem_express_login.png)

## <a name="clean-up-resources"></a><span data-ttu-id="20c09-166">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="20c09-166">Clean up resources</span></span>

<span data-ttu-id="20c09-167">När du är klar med att utforska din första Oracle-databas på Azure och hello VM inte längre behövs, kan du använda hello [ta bort grupp az](/cli/azure/group#delete) kommandot tooremove hello resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="20c09-167">Once you have finished exploring your first Oracle database on Azure and hello VM is no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="20c09-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="20c09-168">Next steps</span></span>

<span data-ttu-id="20c09-169">Lär dig mer om andra [Oracle-lösningar på Azure](oracle-considerations.md).</span><span class="sxs-lookup"><span data-stu-id="20c09-169">Learn about other [Oracle solutions on Azure](oracle-considerations.md).</span></span> 

<span data-ttu-id="20c09-170">Försök hello [installera och konfigurera Oracle Automated lagringshantering](configure-oracle-asm.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="20c09-170">Try hello [Installing and Configuring Oracle Automated Storage Management](configure-oracle-asm.md) tutorial.</span></span>
