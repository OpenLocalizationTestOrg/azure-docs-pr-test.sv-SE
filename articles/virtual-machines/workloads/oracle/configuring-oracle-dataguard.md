---
title: "aaaImplement Oracle Data Guard på virtuella Azure Linux-datorer | Microsoft Docs"
description: "Snabbt en Oracle Data Guard upp och körs i Azure-miljön."
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
ms.date: 05/10/2017
ms.author: rclaus
ms.openlocfilehash: 101196b2f50dfca64d3eb1b4be56ff0c108693e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="implement-oracle-data-guard-on-azure-linux-vm"></a><span data-ttu-id="542cd-103">Implementera Oracle Data Guard på Azure virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="542cd-103">Implement Oracle Data Guard on Azure Linux VM</span></span> 

<span data-ttu-id="542cd-104">hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="542cd-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="542cd-105">Den här guiden information med hjälp av hello Azure CLI toodeploy en Oracle 12c databasen från galleriet hello Marketplace-avbildning.</span><span class="sxs-lookup"><span data-stu-id="542cd-105">This guide details using hello Azure CLI toodeploy an Oracle 12c Database from hello Marketplace gallery image.</span></span> <span data-ttu-id="542cd-106">När hello Oracle-databas har skapats kan det här dokumentet visar du steg för steg hur tooinstall och konfigurera Data Guard på Azure VM.</span><span class="sxs-lookup"><span data-stu-id="542cd-106">Once hello Oracle database is created, this document shows you step-by-step how tooinstall and configure Data Guard on Azure VM.</span></span>

<span data-ttu-id="542cd-107">Innan du börjar bör du kontrollera om den hello Azure CLI har installerats.</span><span class="sxs-lookup"><span data-stu-id="542cd-107">Before you start, make sure that hello Azure CLI has been installed.</span></span> <span data-ttu-id="542cd-108">Mer information finns i [installationsguiden för Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="542cd-108">For more information, see [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-hello-environment"></a><span data-ttu-id="542cd-109">Förbered hello-miljön</span><span class="sxs-lookup"><span data-stu-id="542cd-109">Prepare hello environment</span></span>
### <a name="assumptions"></a><span data-ttu-id="542cd-110">Antaganden</span><span class="sxs-lookup"><span data-stu-id="542cd-110">Assumptions</span></span>

<span data-ttu-id="542cd-111">tooperform hello Oracle Data Guard installera, behöver du toocreate två virtuella Azure-datorer på hello samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="542cd-111">tooperform hello Oracle Data Guard install, you need toocreate two Azure VMs on hello same availability set.</span></span> <span data-ttu-id="542cd-112">hello Marketplace-avbildning som du använder toocreate hello virtuella datorer är ”Oracle: Oracle-databasen-Ee:12.1.0.2:latest”.</span><span class="sxs-lookup"><span data-stu-id="542cd-112">hello Marketplace image you use toocreate hello VMs is "Oracle:Oracle-Database-Ee:12.1.0.2:latest".</span></span>

<span data-ttu-id="542cd-113">hello har primära virtuella datorn (myVM1) en Oracle-instans som körs.</span><span class="sxs-lookup"><span data-stu-id="542cd-113">hello primary VM (myVM1) has a running Oracle instance.</span></span>

<span data-ttu-id="542cd-114">hello vänteläge VM (myVM2) har hello Oracle programvaran endast.</span><span class="sxs-lookup"><span data-stu-id="542cd-114">hello standby VM (myVM2) has hello Oracle software installed only.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="542cd-115">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="542cd-115">Log in tooAzure</span></span> 

<span data-ttu-id="542cd-116">Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="542cd-116">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

```azurecli
az login
```

### <a name="create-a-resource-group"></a><span data-ttu-id="542cd-117">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="542cd-117">Create a resource group</span></span>

<span data-ttu-id="542cd-118">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="542cd-118">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="542cd-119">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="542cd-119">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="542cd-120">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westus` plats.</span><span class="sxs-lookup"><span data-stu-id="542cd-120">hello following example creates a resource group named `myResourceGroup` in hello `westus` location.</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-availability-set"></a><span data-ttu-id="542cd-121">Skapa tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="542cd-121">Create availability set</span></span>

<span data-ttu-id="542cd-122">Det här steget är valfritt men rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="542cd-122">This step is optional, but is recommended.</span></span> <span data-ttu-id="542cd-123">Se [Azure tillgänglighetsuppsättningar guide](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) för mer information.</span><span class="sxs-lookup"><span data-stu-id="542cd-123">see [Azure availability sets guide](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) for more information.</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-virtual-machine"></a><span data-ttu-id="542cd-124">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="542cd-124">Create virtual machine</span></span>

<span data-ttu-id="542cd-125">Skapa en virtuell dator med hello [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="542cd-125">Create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="542cd-126">hello följande exempel skapar 2 virtuella datorer med namnet `myVM1` och `myVM2`.</span><span class="sxs-lookup"><span data-stu-id="542cd-126">hello following example creates 2 VMs named `myVM1` and `myVM2`.</span></span> <span data-ttu-id="542cd-127">Skapar SSH-nycklar om de inte redan finns på standardplatsen nyckel.</span><span class="sxs-lookup"><span data-stu-id="542cd-127">Creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="542cd-128">toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.</span><span class="sxs-lookup"><span data-stu-id="542cd-128">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>

<span data-ttu-id="542cd-129">Skapa myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="542cd-129">Create myVM1 (primary)</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --admin-username azureuser \
     --generate-ssh-keys \
```

<span data-ttu-id="542cd-130">En gång hello VM har skapats, hello Azure CLI visar information liknande toohello följande exempel: Notera hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="542cd-130">Once hello VM has been created, hello Azure CLI shows information similar toohello following example: Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="542cd-131">Den här adressen är används tooaccess hello VM.</span><span class="sxs-lookup"><span data-stu-id="542cd-131">This address is used tooaccess hello VM.</span></span>

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

<span data-ttu-id="542cd-132">Skapa myVM2 (standby)</span><span class="sxs-lookup"><span data-stu-id="542cd-132">Create myVM2 (standby)</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --admin-username azureuser \
     --generate-ssh-keys \
```

<span data-ttu-id="542cd-133">Anteckna hello `publicIpAddress` samt när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="542cd-133">Take note of hello `publicIpAddress` as well once it created.</span></span>

### <a name="open-hello-tcp-port-for-connectivity"></a><span data-ttu-id="542cd-134">Öppna hello TCP-port för anslutning</span><span class="sxs-lookup"><span data-stu-id="542cd-134">Open hello TCP port for connectivity</span></span>

<span data-ttu-id="542cd-135">hello steg är tooconfigure externa slutpunkter, som tillåter fjärråtkomst hello Oracle-databas måste du köra följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="542cd-135">hello step is tooconfigure external endpoints, which allows accessing hello Oracle DB remotely, you execute hello following command.</span></span>

<span data-ttu-id="542cd-136">Öppna porten för myVM1</span><span class="sxs-lookup"><span data-stu-id="542cd-136">Open port for myVM1</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVmN1SG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

<span data-ttu-id="542cd-137">Resultatet bör se ut ungefär toohello efter svar:</span><span class="sxs-lookup"><span data-stu-id="542cd-137">Result should look similar toohello following response:</span></span>

```bash
{
  "access": "Allow",
  "description": null,
  "destinationAddressPrefix": "*",
  "destinationPortRange": "1521",
  "direction": "Inbound",
  "etag": "W/\"bd77dcae-e5fd-4bd6-a632-26045b646414\"",
  "id": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myVmNSG/securityRules/allow-oracle",
  "name": "allow-oracle",
  "priority": 999,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sourceAddressPrefix": "*",
  "sourcePortRange": "*"
}
```

<span data-ttu-id="542cd-138">Öppna porten för myVM2</span><span class="sxs-lookup"><span data-stu-id="542cd-138">Open port for myVM2</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2N1SG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-toovirtual-machine"></a><span data-ttu-id="542cd-139">Ansluta toovirtual datorn</span><span class="sxs-lookup"><span data-stu-id="542cd-139">Connect toovirtual machine</span></span>

<span data-ttu-id="542cd-140">Använd hello följande kommando toocreate en SSH-session med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="542cd-140">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="542cd-141">Ersätt hello IP-adress med hello `publicIpAddress` i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="542cd-141">Replace hello IP address with hello `publicIpAddress` of your virtual machine.</span></span>

```bash 
ssh azureuser@<publicIpAddress>
```

### <a name="create-database-on-myvm1-primary"></a><span data-ttu-id="542cd-142">Skapa databas på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="542cd-142">Create Database on myVM1 (Primary)</span></span>

<span data-ttu-id="542cd-143">hello Oracle-programvara är installerad på hello Marketplace-avbildning, så hello nästa steg är tooinstall hello-databas.</span><span class="sxs-lookup"><span data-stu-id="542cd-143">hello Oracle software is already installed on hello Marketplace image, so hello next step is tooinstall hello database.</span></span> <span data-ttu-id="542cd-144">hello första steget körs som hello 'oracle-superanvändare.</span><span class="sxs-lookup"><span data-stu-id="542cd-144">hello first step is running as hello 'oracle' superuser.</span></span>

```bash
sudo su - oracle
```

<span data-ttu-id="542cd-145">Skapa databas för hello:</span><span class="sxs-lookup"><span data-stu-id="542cd-145">create hello database:</span></span>

```bash
$ dbca -silent \
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
<span data-ttu-id="542cd-146">Utdata ska se ut ungefär toohello efter svar:</span><span class="sxs-lookup"><span data-stu-id="542cd-146">Outputs should look similar toohello following response:</span></span>

```bash
Copying database files
1% complete
2% complete
8% complete
13% complete
19% complete
27% complete
Creating and starting Oracle instance
29% complete
32% complete
33% complete
34% complete
38% complete
42% complete
43% complete
45% complete
Completing Database Creation
48% complete
51% complete
53% complete
62% complete
70% complete
72% complete
Creating Pluggable Databases
78% complete
100% complete
Look at hello log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for further details.
```

<span data-ttu-id="542cd-147">Ange hello ORACLE_SID och ORACLE_HOME variabler</span><span class="sxs-lookup"><span data-stu-id="542cd-147">Set hello ORACLE_SID and ORACLE_HOME variables</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
```

<span data-ttu-id="542cd-148">Du kan eventuellt ORACLE_HOME och ORACLE_SID toohello .bashrc filen som läggs till, så att inställningarna sparas för framtida inloggningar.</span><span class="sxs-lookup"><span data-stu-id="542cd-148">Optionally, You can added ORACLE_HOME and ORACLE_SID toohello .bashrc file, so that these settings are saved for future logins.</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

## <a name="data-guard-configurations"></a><span data-ttu-id="542cd-149">Data Guard konfigurationer</span><span class="sxs-lookup"><span data-stu-id="542cd-149">Data Guard Configurations</span></span>

### <a name="enable-archive-log-mode-on-myvm1-primary"></a><span data-ttu-id="542cd-150">Aktivera Archive log-läge på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="542cd-150">Enable Archive log mode on myVM1 (primary)</span></span>

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
```
<span data-ttu-id="542cd-151">Aktivera Tvingad loggning och kontrollera att det finns minst en loggfil.</span><span class="sxs-lookup"><span data-stu-id="542cd-151">Enable force logging, and make sure at least one logfile is present.</span></span>

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
```

<span data-ttu-id="542cd-152">Skapa vänteläge gör om-loggar</span><span class="sxs-lookup"><span data-stu-id="542cd-152">Create standby redo logs</span></span>

```bash
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo01.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo02.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo03.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo04.log') SIZE 50M;
```

<span data-ttu-id="542cd-153">Aktivera Flashback (som gjorts hello recovery mycket tidigare) och ange STANDBY_FILE_MANAGEMENT tooauto</span><span class="sxs-lookup"><span data-stu-id="542cd-153">Turn on Flashback (which made hello recovery a lot earlier) and set STANDBY_FILE_MANAGEMENT tooauto</span></span>

```bash
SQL> ALTER DATABASE FLASHBACK ON;
SQL> ALTER SYSTEM SET STANDBY_FILE_MANAGEMENT=AUTO;
```

### <a name="service-setup-on-myvm1-primary"></a><span data-ttu-id="542cd-154">Installation av Service på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="542cd-154">Service setup on myVM1 (primary)</span></span>

<span data-ttu-id="542cd-155">Redigera eller skapa hello tnsnames.ora fil, som finns på $ORACLE_HOME\network\admin mapp</span><span class="sxs-lookup"><span data-stu-id="542cd-155">Edit or create hello tnsnames.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="542cd-156">Lägg till följande poster hello</span><span class="sxs-lookup"><span data-stu-id="542cd-156">Add hello following entries</span></span>

```bash
cdb1 =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )

cdb1_stby =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )
```

<span data-ttu-id="542cd-157">Redigera eller skapa hello listener.ora fil, som finns på $ORACLE_HOME\network\admin mapp</span><span class="sxs-lookup"><span data-stu-id="542cd-157">Edit or create hello listener.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="542cd-158">Lägg till följande poster hello</span><span class="sxs-lookup"><span data-stu-id="542cd-158">Add hello following entries</span></span>

```bash
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = cdb1_DGMGRL)
      (ORACLE_HOME = /u01/app/oracle/product/12.1.0/dbhome_1)
      (SID_NAME = cdb1)
    )
  )

ADR_BASE_LISTENER = /u01/app/oracle
```

<span data-ttu-id="542cd-159">Starta hello-lyssnare</span><span class="sxs-lookup"><span data-stu-id="542cd-159">Start hello listener</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```

### <a name="service-setup-on-myvm2-standby"></a><span data-ttu-id="542cd-160">Installation av Service på myVM2 (Standby)</span><span class="sxs-lookup"><span data-stu-id="542cd-160">Service setup on myVM2 (Standby)</span></span>

<span data-ttu-id="542cd-161">Redigera eller skapa hello tnsnames.ora fil, som finns på $ORACLE_HOME\network\admin mapp</span><span class="sxs-lookup"><span data-stu-id="542cd-161">Edit or create hello tnsnames.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="542cd-162">Lägg till följande poster hello</span><span class="sxs-lookup"><span data-stu-id="542cd-162">Add hello following entries</span></span>

```bash
cdb1 =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )

cdb1_stby =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )
```

<span data-ttu-id="542cd-163">Redigera eller skapa hello listener.ora fil, som finns på $ORACLE_HOME\network\admin mapp</span><span class="sxs-lookup"><span data-stu-id="542cd-163">Edit or create hello listener.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="542cd-164">Lägg till följande poster hello</span><span class="sxs-lookup"><span data-stu-id="542cd-164">Add hello following entries</span></span>

```bash
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = cdb1_DGMGRL)
      (ORACLE_HOME = /u01/app/oracle/product/12.1.0/dbhome_1)
      (SID_NAME = cdb1)
    )
  )

ADR_BASE_LISTENER = /u01/app/oracle
```

<span data-ttu-id="542cd-165">Starta hello-lyssnare</span><span class="sxs-lookup"><span data-stu-id="542cd-165">Start hello listener</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```

<span data-ttu-id="542cd-166">Aktivera Data Guard Broker</span><span class="sxs-lookup"><span data-stu-id="542cd-166">Enable Data Guard Broker</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="restore-database-toomyvm2-standby"></a><span data-ttu-id="542cd-167">Återställa databasen toomyVM2 (Standby)</span><span class="sxs-lookup"><span data-stu-id="542cd-167">Restore database toomyVM2 (Standby)</span></span>

<span data-ttu-id="542cd-168">Skapa en parameterfil ' / tmp/initcdb1_stby.ora' med hello följande innehåll</span><span class="sxs-lookup"><span data-stu-id="542cd-168">Create a parameter file '/tmp/initcdb1_stby.ora' with hello followings contents</span></span>
```bash
*.db_name='cdb1'
```

<span data-ttu-id="542cd-169">Skapa mappar</span><span class="sxs-lookup"><span data-stu-id="542cd-169">Create folders</span></span>

```bash
mkdir -p /u01/app/oracle/oradata/cdb1/pdbseed
mkdir -p /u01/app/oracle/oradata/cdb1/pdb1
mkdir -p /u01/app/oracle/fast_recovery_area/cdb1
mkdir -p /u01/app/oracle/admin/cdb1/adump
```

<span data-ttu-id="542cd-170">Skapa lösenordsfilen</span><span class="sxs-lookup"><span data-stu-id="542cd-170">Create password file</span></span>

```bash
$ orapwd file=/u01/app/oracle/product/12.1.0.2/db_1/dbs/orapwcdb1 password=OraPasswd1 entries=10
```
<span data-ttu-id="542cd-171">Starta databasen på myVM2</span><span class="sxs-lookup"><span data-stu-id="542cd-171">Start up database on myVM2</span></span>

```bash
$ export ORACLE_SID=cdb1
$ sqlplus / as sysdba

SQL> STARTUP NOMOUNT PFILE='/tmp/initcdb1_stby.ora';
SQL> EXIT;
```

<span data-ttu-id="542cd-172">Återställa databasen med verktyg för RMAN</span><span class="sxs-lookup"><span data-stu-id="542cd-172">Restore database using RMAN utility</span></span>

```bash
$ rman TARGET sys/OraPasswd1@cdb1 AUXILIARY sys/OraPasswd1@cdb1_stby
```

<span data-ttu-id="542cd-173">Kör följande kommandon i RMAN hello</span><span class="sxs-lookup"><span data-stu-id="542cd-173">Execute hello following commands in RMAN</span></span>
```bash
DUPLICATE TARGET DATABASE
  FOR STANDBY
  FROM ACTIVE DATABASE
  DORECOVER
  SPFILE
    SET db_unique_name='CDB1_STBY' COMMENT 'Is standby'
  NOFILENAMECHECK;
```

<span data-ttu-id="542cd-174">Aktivera Data Guard Broker</span><span class="sxs-lookup"><span data-stu-id="542cd-174">Enable Data Guard Broker</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="configure-data-guard-broker-on-myvm1-primary"></a><span data-ttu-id="542cd-175">Konfigurera Data Guard broker på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="542cd-175">Configure Data Guard broker on myVM1 (primary)</span></span>

<span data-ttu-id="542cd-176">Starta hello Data Guard manager och logga in med SYS och lösenord (gör inte använder OS-autentisering).</span><span class="sxs-lookup"><span data-stu-id="542cd-176">Start hello Data Guard manager and login using SYS and password (do not using OS authentication).</span></span> <span data-ttu-id="542cd-177">Utföra hello i:</span><span class="sxs-lookup"><span data-stu-id="542cd-177">Perform hello followings</span></span>

```bash
$ dgmgrl sys/OraPasswd1@cdb1
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome tooDGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> CREATE CONFIGURATION my_dg_config AS PRIMARY DATABASE IS cdb1 CONNECT IDENTIFIER IS cdb1;
Configuration "my_dg_config" created with primary database "cdb1"
DGMGRL> ADD DATABASE cdb1_stby AS CONNECT IDENTIFIER IS cdb1_stby MAINTAINED AS PHYSICAL;
Database "cdb1_stby" added
DGMGRL> ENABLE CONFIGURATION;
Enabled.
```

<span data-ttu-id="542cd-178">Granska hello konfigurationen</span><span class="sxs-lookup"><span data-stu-id="542cd-178">Review hello configuration</span></span>
```bash
DGMGRL> SHOW CONFIGURATION;

Configuration - my_dg_config

  Protection Mode: MaxPerformance
  Members:
  cdb1      - Primary database
    cdb1_stby - Physical standby database

Fast-Start Failover: DISABLED

Configuration Status:
SUCCESS   (status updated 26 seconds ago)
```

<span data-ttu-id="542cd-179">Hello Oracle Data Guard installationen klar.</span><span class="sxs-lookup"><span data-stu-id="542cd-179">This completed hello Oracle Data Guard setup.</span></span> <span data-ttu-id="542cd-180">hello nästa avsnitt visar hur tootest hello anslutningar och växlar</span><span class="sxs-lookup"><span data-stu-id="542cd-180">hello next section shows you how tootest hello connectivity and switching over</span></span>

### <a name="connect-database-from-client-machine"></a><span data-ttu-id="542cd-181">Ansluta databasen från klientdatorn</span><span class="sxs-lookup"><span data-stu-id="542cd-181">Connect database from client machine</span></span>

<span data-ttu-id="542cd-182">Uppdatera eller skapa hello tnsnames.ora fil på din klientdator som vanligtvis finns i $ORACLE_HOME\network\admin.</span><span class="sxs-lookup"><span data-stu-id="542cd-182">Update or create hello tnsnames.ora file on your client machine which usually is located at $ORACLE_HOME\network\admin.</span></span>

<span data-ttu-id="542cd-183">Ersätt hello IP med din `publicIpAddress` för myVM1 och myVM2</span><span class="sxs-lookup"><span data-stu-id="542cd-183">Replace hello IP with your `publicIpAddress` for myVM1 and myVM2</span></span>

```bash
cdb1=
  (DESCRIPTION=
    (ADDRESS=
      (PROTOCOL=TCP)
      (HOST=<myVM1 IP address>)
      (PORT=1521)
    )
    (CONNECT_DATA=
      (SERVER=dedicated)
      (SERVICE_NAME=cdb1)
    )
  )

cdb1_stby=
  (DESCRIPTION=
    (ADDRESS=
      (PROTOCOL=TCP)
      (HOST=<myVM2 IP address>)
      (PORT=1521)
    )
    (CONNECT_DATA=
      (SERVER=dedicated)
      (SERVICE_NAME=cdb1_stby)
    )
  )
```

<span data-ttu-id="542cd-184">Starta sqlplus</span><span class="sxs-lookup"><span data-stu-id="542cd-184">Start sqlplus</span></span>

```bash
$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```
## <a name="test-data-guard-configuration"></a><span data-ttu-id="542cd-185">Data Guard testkonfiguration</span><span class="sxs-lookup"><span data-stu-id="542cd-185">Test Data Guard configuration</span></span>

### <a name="database-switchover-on-myvm1-primary"></a><span data-ttu-id="542cd-186">Databasen övergången på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="542cd-186">Database switchover on myVM1 (primary)</span></span>

<span data-ttu-id="542cd-187">tooswitch från primära toostandby (cdb1 toocdb1_stby)</span><span class="sxs-lookup"><span data-stu-id="542cd-187">tooswitch from primary toostandby (cdb1 toocdb1_stby)</span></span>

```bash
$ dgmgrl sys/OraPasswd1@cdb1
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome tooDGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> SWITCHOVER toocdb1_stby;
Performing switchover NOW, please wait...
Operation requires a connection tooinstance "cdb1" on database "cdb1_stby"
Connecting tooinstance "cdb1"...
Connected as SYSDBA.
New primary database "cdb1_stby" is opening...
Operation requires start up of instance "cdb1" on database "cdb1"
Starting instance "cdb1"...
ORACLE instance started.
Database mounted.
Switchover succeeded, new primary is "cdb1_stby"
DGMGRL>
```

<span data-ttu-id="542cd-188">Nu bör du kunna tooconnect toohello standby-databas</span><span class="sxs-lookup"><span data-stu-id="542cd-188">You should now be able tooconnect toohello standby database</span></span>

<span data-ttu-id="542cd-189">Starta sqlplus</span><span class="sxs-lookup"><span data-stu-id="542cd-189">Start sqlplus</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1_stby
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

### <a name="database-switch-back-on-myvm2-standby"></a><span data-ttu-id="542cd-190">Databasen växeln tillbaka på myVM2 (standby)</span><span class="sxs-lookup"><span data-stu-id="542cd-190">Database switch back on myVM2 (standby)</span></span>

<span data-ttu-id="542cd-191">tooswitch tillbaka, kör i: hello på myVM2</span><span class="sxs-lookup"><span data-stu-id="542cd-191">tooswitch back, run hello followings on myVM2</span></span>
```bash
$ dgmgrl sys/OraPasswd1@cdb1_stby
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome tooDGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> SWITCHOVER toocdb1;
Performing switchover NOW, please wait...
Operation requires a connection tooinstance "cdb1" on database "cdb1"
Connecting tooinstance "cdb1"...
Connected as SYSDBA.
New primary database "cdb1" is opening...
Operation requires start up of instance "cdb1" on database "cdb1_stby"
Starting instance "cdb1"...
ORACLE instance started.
Database mounted.
Switchover succeeded, new primary is "cdb1"
```

<span data-ttu-id="542cd-192">Igen nu bör du kunna tooconnect toohello primära databasen</span><span class="sxs-lookup"><span data-stu-id="542cd-192">Once again, You should now be able tooconnect toohello primary database</span></span>

<span data-ttu-id="542cd-193">Starta sqlplus</span><span class="sxs-lookup"><span data-stu-id="542cd-193">Start sqlplus</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

<span data-ttu-id="542cd-194">Detta slutfört hello installationen och konfigurationen av Data Guard på Oracle linux.</span><span class="sxs-lookup"><span data-stu-id="542cd-194">This completed hello installation and configuration of Data Guard on Oracle linux.</span></span>


## <a name="delete-virtual-machine"></a><span data-ttu-id="542cd-195">Ta bort en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="542cd-195">Delete virtual machine</span></span>

<span data-ttu-id="542cd-196">När det inte längre behövs hello följande kommando kan vara används tooremove hello resursgrupp, en virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="542cd-196">When no longer needed, hello following command can be used tooremove hello Resource Group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="542cd-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="542cd-197">Next steps</span></span>

[<span data-ttu-id="542cd-198">Självstudie: Skapa virtuella datorer med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="542cd-198">Create highly available virtual machines tutorial</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="542cd-199">Utforska exempel på distribution av virtuella datorer med CLI</span><span class="sxs-lookup"><span data-stu-id="542cd-199">Explore VM deployment CLI samples</span></span>](../../linux/cli-samples.md)
