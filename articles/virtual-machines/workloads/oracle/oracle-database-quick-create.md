---
title: Skapa en Oracle-databas i en Azure VM | Microsoft Docs
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
ms.openlocfilehash: 8683b016c4db2c66fb1dd994405b70c3d137a7fc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-oracle-database-in-an-azure-vm"></a><span data-ttu-id="c8381-103">Skapa en Oracle-databas i en virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="c8381-103">Create an Oracle Database in an Azure VM</span></span>

<span data-ttu-id="c8381-104">Den här guiden information som använder Azure CLI för att distribuera en virtuell Azure-dator från den [Oracle marketplace-avbildning i galleriet](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) för att skapa en Oracle 12 c-databas.</span><span class="sxs-lookup"><span data-stu-id="c8381-104">This guide details using the Azure CLI to deploy an Azure virtual machine from the [Oracle marketplace gallery image](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) in order to create an Oracle 12c database.</span></span> <span data-ttu-id="c8381-105">När servern har distribuerats, ansluter du via SSH för att kunna konfigurera Oracle-databasen.</span><span class="sxs-lookup"><span data-stu-id="c8381-105">Once the server is deployed, you will connect via SSH in order to configure the Oracle database.</span></span> 

<span data-ttu-id="c8381-106">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="c8381-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c8381-107">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="c8381-107">If you choose to install and use the CLI locally, this quickstart requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="c8381-108">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="c8381-108">Run `az --version` to find the version.</span></span> <span data-ttu-id="c8381-109">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c8381-109">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="c8381-110">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="c8381-110">Create a resource group</span></span>

<span data-ttu-id="c8381-111">Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="c8381-111">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="c8381-112">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="c8381-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="c8381-113">I följande exempel skapas en resursgrupp med namnet *myResourceGroup* på platsen *eastus*.</span><span class="sxs-lookup"><span data-stu-id="c8381-113">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
## <a name="create-virtual-machine"></a><span data-ttu-id="c8381-114">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c8381-114">Create virtual machine</span></span>

<span data-ttu-id="c8381-115">Du kan skapa en virtuell dator (VM) med den [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="c8381-115">To create a virtual machine (VM), use the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="c8381-116">Följande exempel skapar en virtuell dator med namnet `myVM`.</span><span class="sxs-lookup"><span data-stu-id="c8381-116">The following example creates a VM named `myVM`.</span></span> <span data-ttu-id="c8381-117">Det skapar också SSH-nycklar, om de inte redan finns på standardplatsen nyckel.</span><span class="sxs-lookup"><span data-stu-id="c8381-117">It also creates SSH keys, if they do not already exist in a default key location.</span></span> <span data-ttu-id="c8381-118">Om du vill använda en specifik uppsättning nycklar använder du alternativet `--ssh-key-value`.</span><span class="sxs-lookup"><span data-stu-id="c8381-118">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="c8381-119">När du har skapat den virtuella datorn, visar Azure CLI information som liknar följande exempel.</span><span class="sxs-lookup"><span data-stu-id="c8381-119">After you create the VM, Azure CLI displays information similar to the following example.</span></span> <span data-ttu-id="c8381-120">Anteckna värdet för `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="c8381-120">Note the value for `publicIpAddress`.</span></span> <span data-ttu-id="c8381-121">Du kan använda den här adressen för att få åtkomst till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c8381-121">You use this address to access the VM.</span></span>

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

## <a name="connect-to-the-vm"></a><span data-ttu-id="c8381-122">Anslut till VM:en</span><span class="sxs-lookup"><span data-stu-id="c8381-122">Connect to the VM</span></span>

<span data-ttu-id="c8381-123">Använd följande kommando för att skapa en SSH-session med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c8381-123">To create an SSH session with the VM, use the following command.</span></span> <span data-ttu-id="c8381-124">Ersätt IP-adressen med den `publicIpAddress` värde för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c8381-124">Replace the IP address with the `publicIpAddress` value for your VM.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="create-the-database"></a><span data-ttu-id="c8381-125">Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="c8381-125">Create the database</span></span>

<span data-ttu-id="c8381-126">Oracle-programvara är installerad på Marketplace-avbildning.</span><span class="sxs-lookup"><span data-stu-id="c8381-126">The Oracle software is already installed on the Marketplace image.</span></span> <span data-ttu-id="c8381-127">Skapa en exempeldatabas på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="c8381-127">Create a sample database as follows.</span></span> 

1.  <span data-ttu-id="c8381-128">Växla till den *oracle* superanvändare och sedan initiera lyssnaren för loggning:</span><span class="sxs-lookup"><span data-stu-id="c8381-128">Switch to the *oracle* superuser, then initialize the listener for logging:</span></span>

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    ```

    <span data-ttu-id="c8381-129">De utdata som genereras liknar följande:</span><span class="sxs-lookup"><span data-stu-id="c8381-129">The output is similar to the following:</span></span>

    ```bash
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

2.  <span data-ttu-id="c8381-130">skapa databasen:</span><span class="sxs-lookup"><span data-stu-id="c8381-130">Create the database:</span></span>

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

    <span data-ttu-id="c8381-131">Det tar några minuter att skapa databasen.</span><span class="sxs-lookup"><span data-stu-id="c8381-131">It takes a few minutes to create the database.</span></span>

3. <span data-ttu-id="c8381-132">Ange variabler för Oracle</span><span class="sxs-lookup"><span data-stu-id="c8381-132">Set Oracle variables</span></span>

<span data-ttu-id="c8381-133">Innan du ansluter måste du ställa in två miljövariabler: *ORACLE_HOME* och *ORACLE_SID*.</span><span class="sxs-lookup"><span data-stu-id="c8381-133">Before you connect, you need to set two environment variables: *ORACLE_HOME* and *ORACLE_SID*.</span></span>

```bash
ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
ORACLE_SID=cdb1; export ORACLE_SID
```
<span data-ttu-id="c8381-134">Du kan också lägga till ORACLE_HOME och ORACLE_SID variabler i filen .bashrc.</span><span class="sxs-lookup"><span data-stu-id="c8381-134">You also can add ORACLE_HOME and ORACLE_SID variables to the .bashrc file.</span></span> <span data-ttu-id="c8381-135">Detta skulle spara miljövariabler för framtida inloggningar.</span><span class="sxs-lookup"><span data-stu-id="c8381-135">This would save the environment variables for future sign-ins.</span></span> <span data-ttu-id="c8381-136">Bekräfta följande påståenden har lagts till i `~/.bashrc` fil med valfri redigerare.</span><span class="sxs-lookup"><span data-stu-id="c8381-136">Confirm the following statements have been added to the `~/.bashrc` file using editor of your choice.</span></span>

```bash
# Add ORACLE_HOME. 
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1 
# Add ORACLE_SID. 
export ORACLE_SID=cdb1 
```

## <a name="oracle-em-express-connectivity"></a><span data-ttu-id="c8381-137">Oracle EM snabb anslutning</span><span class="sxs-lookup"><span data-stu-id="c8381-137">Oracle EM Express connectivity</span></span>

<span data-ttu-id="c8381-138">För ett GUI-hanteringsverktyg som du kan använda för att utforska databasen, ställa in Oracle EM Express.</span><span class="sxs-lookup"><span data-stu-id="c8381-138">For a GUI management tool that you can use to explore the database, set up Oracle EM Express.</span></span> <span data-ttu-id="c8381-139">Om du vill ansluta till Oracle EM Express, måste du först ställa in port i Oracle.</span><span class="sxs-lookup"><span data-stu-id="c8381-139">To connect to Oracle EM Express, you must first set up the port in Oracle.</span></span> 

1. <span data-ttu-id="c8381-140">Anslut till databasen med sqlplus:</span><span class="sxs-lookup"><span data-stu-id="c8381-140">Connect to your database using sqlplus:</span></span>

    ```bash
    sqlplus / as sysdba
    ```

2. <span data-ttu-id="c8381-141">När du är ansluten, anger du porten 5502 för EM Express</span><span class="sxs-lookup"><span data-stu-id="c8381-141">Once connected, set the port 5502 for EM Express</span></span>

    ```bash
    exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);
    ```

3. <span data-ttu-id="c8381-142">Öppna den behållaren PDB1 om den inte redan öppnad men första Kontrollera status:</span><span class="sxs-lookup"><span data-stu-id="c8381-142">Open the container PDB1 if not already opened, but first check the status:</span></span>

    ```bash
    select con_id, name, open_mode from v$pdbs;
    ```

    <span data-ttu-id="c8381-143">De utdata som genereras liknar följande:</span><span class="sxs-lookup"><span data-stu-id="c8381-143">The output is similar to the following:</span></span>

    ```bash
      CON_ID NAME                           OPEN_MODE 
      ----------- ------------------------- ---------- 
      2           PDB$SEED                  READ ONLY 
      3           PDB1                      MOUNT
    ```

4. <span data-ttu-id="c8381-144">Om OPEN_MODE för `PDB1` är inte läsa skriva, kör du följande kommandon för att öppna PDB1:</span><span class="sxs-lookup"><span data-stu-id="c8381-144">If the OPEN_MODE for `PDB1` is not READ WRITE, then run the followings commands to open PDB1:</span></span>

   ```bash
    alter session set container=pdb1;
    alter database open;
   ```

<span data-ttu-id="c8381-145">Du måste ange `quit` att avsluta sessionen sqlplus och typen `exit` logga ut för oracle-användare.</span><span class="sxs-lookup"><span data-stu-id="c8381-145">You need to type `quit` to end the sqlplus session and type `exit` to logout of the oracle user.</span></span>

## <a name="automate-database-startup-and-shutdown"></a><span data-ttu-id="c8381-146">Automatisera databasen start och stopp</span><span class="sxs-lookup"><span data-stu-id="c8381-146">Automate database startup and shutdown</span></span>

<span data-ttu-id="c8381-147">Oracle-databasen som standard startas inte automatiskt när du startar om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c8381-147">The Oracle database by default doesn't automatically start when you restart the VM.</span></span> <span data-ttu-id="c8381-148">Om du vill ställa in Oracle-databasen för att starta automatiskt, först logga in som rot.</span><span class="sxs-lookup"><span data-stu-id="c8381-148">To set up the Oracle database to start automatically, first sign in as root.</span></span> <span data-ttu-id="c8381-149">Sedan skapa och uppdatera vissa systemfiler.</span><span class="sxs-lookup"><span data-stu-id="c8381-149">Then, create and update some system files.</span></span>

1. <span data-ttu-id="c8381-150">Logga in som rot</span><span class="sxs-lookup"><span data-stu-id="c8381-150">Sign on as root</span></span>
    ```bash
    sudo su -
    ```

2.  <span data-ttu-id="c8381-151">Med din favorit redigerare, redigerar du filen `/etc/oratab` och ändra standard `N` till `Y`:</span><span class="sxs-lookup"><span data-stu-id="c8381-151">Using your favorite editor, edit the file `/etc/oratab` and change the default `N` to `Y`:</span></span>

    ```bash
    cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y
    ```

3.  <span data-ttu-id="c8381-152">Skapa en fil med namnet `/etc/init.d/dbora` och klistra in följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="c8381-152">Create a file named `/etc/init.d/dbora` and paste the following contents:</span></span>

    ```
    #!/bin/sh
    # chkconfig: 345 99 10
    # Description: Oracle auto start-stop script.
    #
    # Set ORA_HOME to be equivalent to $ORACLE_HOME.
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle

    case "$1" in
    'start')
        # Start the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        # Remove "&" if you don't want startup as a background process.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" &
        touch /var/lock/subsys/dbora
        ;;

    'stop')
        # Stop the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" &
        rm -f /var/lock/subsys/dbora
        ;;
    esac
    ```

4.  <span data-ttu-id="c8381-153">Ändra behörigheter för filer med *chmod* på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c8381-153">Change permissions on files with *chmod* as follows:</span></span>

    ```bash
    chgrp dba /etc/init.d/dbora
    chmod 750 /etc/init.d/dbora
    ```

5.  <span data-ttu-id="c8381-154">Skapa symboliska länkar för start och stopp enligt följande:</span><span class="sxs-lookup"><span data-stu-id="c8381-154">Create symbolic links for startup and shutdown as follows:</span></span>

    ```bash
    ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  <span data-ttu-id="c8381-155">Testa dina ändringar genom att starta om den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="c8381-155">To test your changes, restart the VM:</span></span>

    ```bash
    reboot
    ```

## <a name="open-ports-for-connectivity"></a><span data-ttu-id="c8381-156">Öppna portar för anslutning</span><span class="sxs-lookup"><span data-stu-id="c8381-156">Open ports for connectivity</span></span>

<span data-ttu-id="c8381-157">Det sista steget är att konfigurera vissa externa slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="c8381-157">The final task is to configure some external endpoints.</span></span> <span data-ttu-id="c8381-158">Avsluta SSH-sessionen på den virtuella datorn (bör ha har dessutom out-of-SSH när omstart i föregående steg) för att ställa in Azure Nätverkssäkerhetsgruppen som skyddar den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c8381-158">To set up the Azure Network Security Group that protects the VM, first exit your SSH session in the VM (should have been kicked out of SSH when rebooting in previous step).</span></span> 

1.  <span data-ttu-id="c8381-159">Om du vill öppna den slutpunkt som du använder för att få fjärråtkomst till Oracle-databasen, skapar du en Nätverkssäkerhetsgrupp regel med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c8381-159">To open the endpoint that you use to access the Oracle database remotely, create a Network Security Group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) as follows:</span></span> 

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup\
        --nsg-name myVmNSG \
        --name allow-oracle \
        --protocol tcp \
        --priority 1001 \
        --destination-port-range 1521
    ```

2.  <span data-ttu-id="c8381-160">Om du vill öppna den slutpunkt som du använder för att fjärransluta till Oracle EM Express, skapar du en Nätverkssäkerhetsgrupp regel med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c8381-160">To open the endpoint that you use to access Oracle EM Express remotely, create a Network Security Group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) as follows:</span></span>

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup \
        --nsg-name myVmNSG \
        --name allow-oracle-EM \
        --protocol tcp \
        --priority 1002 \
        --destination-port-range 5502
    ```

3. <span data-ttu-id="c8381-161">Om det behövs kan hämta den offentliga IP-adressen på den virtuella datorn igen med [az nätverket offentliga ip-visa](/cli/azure/network/public-ip#show) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c8381-161">If needed, obtain the public IP address of your VM again with [az network public-ip show](/cli/azure/network/public-ip#show) as follows:</span></span>

    ```azurecli-interactive
    az network public-ip show \
        --resource-group myResourceGroup \
        --name myVMPublicIP \
        --query [ipAddress] \
        --output tsv
    ```

4.  <span data-ttu-id="c8381-162">Ansluta EM Express från din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="c8381-162">Connect EM Express from your browser.</span></span> <span data-ttu-id="c8381-163">Kontrollera att din webbläsare är kompatibelt med EM Express (Flash installation krävs):</span><span class="sxs-lookup"><span data-stu-id="c8381-163">Make sure your browser is compatible with EM Express (Flash install is required):</span></span> 

    ```
    https://<VM ip address or hostname>:5502/em
    ```

<span data-ttu-id="c8381-164">Du kan logga in med hjälp av den **SYS** konto och kontrollera den **som sysdba** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="c8381-164">You can log in by using the **SYS** account, and check the **as sysdba** checkbox.</span></span> <span data-ttu-id="c8381-165">Använd lösenord **OraPasswd1** som du anger under installationen.</span><span class="sxs-lookup"><span data-stu-id="c8381-165">Use the password **OraPasswd1** that you set during installation.</span></span> 

![Skärmbild av inloggningssidan Oracle OEM Express](./media/oracle-quick-start/oracle_oem_express_login.png)

## <a name="clean-up-resources"></a><span data-ttu-id="c8381-167">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="c8381-167">Clean up resources</span></span>

<span data-ttu-id="c8381-168">När du är klar med att utforska din första Oracle-databas på Azure och den virtuella datorn inte längre behövs kan du använda den [ta bort grupp az](/cli/azure/group#delete) kommandot för att ta bort resursgruppen VM, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="c8381-168">Once you have finished exploring your first Oracle database on Azure and the VM is no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="c8381-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c8381-169">Next steps</span></span>

<span data-ttu-id="c8381-170">Lär dig mer om andra [Oracle-lösningar på Azure](oracle-considerations.md).</span><span class="sxs-lookup"><span data-stu-id="c8381-170">Learn about other [Oracle solutions on Azure](oracle-considerations.md).</span></span> 

<span data-ttu-id="c8381-171">Försök i [installera och konfigurera Oracle Automated lagringshantering](configure-oracle-asm.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="c8381-171">Try the [Installing and Configuring Oracle Automated Storage Management](configure-oracle-asm.md) tutorial.</span></span>