---
title: "aaaImplement Oracle Data Guard på en virtuell Azure Linux-dator | Microsoft Docs"
description: "Snabbt Oracle Data Guard upp och körs i Azure-miljön."
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
ms.openlocfilehash: 6bb530098737e3ca7dd8bab3f4306ecbb620f3f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="implement-oracle-data-guard-on-an-azure-linux-virtual-machine"></a><span data-ttu-id="a9e57-103">Implementera Oracle Data Guard på en virtuell Azure Linux-dator</span><span class="sxs-lookup"><span data-stu-id="a9e57-103">Implement Oracle Data Guard on an Azure Linux virtual machine</span></span> 

<span data-ttu-id="a9e57-104">Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="a9e57-104">Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="a9e57-105">Den här artikeln beskriver hur toouse Azure CLI toodeploy en Oracle-databas 12c databasen från hello Azure Marketplace-avbildning.</span><span class="sxs-lookup"><span data-stu-id="a9e57-105">This article describes how toouse Azure CLI toodeploy an Oracle Database 12c database from hello Azure Marketplace image.</span></span> <span data-ttu-id="a9e57-106">Den här artikeln sedan visar du steg för steg hur tooinstall och konfigurera Data Guard på en Azure-dator (VM).</span><span class="sxs-lookup"><span data-stu-id="a9e57-106">This article then shows you, step by step, how tooinstall and configure Data Guard on an Azure virtual machine (VM).</span></span>

<span data-ttu-id="a9e57-107">Innan du börjar bör du kontrollera att Azure CLI är installerad.</span><span class="sxs-lookup"><span data-stu-id="a9e57-107">Before you start, make sure that Azure CLI is installed.</span></span> <span data-ttu-id="a9e57-108">Mer information finns i hello [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a9e57-108">For more information, see hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-hello-environment"></a><span data-ttu-id="a9e57-109">Förbered hello-miljön</span><span class="sxs-lookup"><span data-stu-id="a9e57-109">Prepare hello environment</span></span>
### <a name="assumptions"></a><span data-ttu-id="a9e57-110">Antaganden</span><span class="sxs-lookup"><span data-stu-id="a9e57-110">Assumptions</span></span>

<span data-ttu-id="a9e57-111">tooinstall Oracle Data Guard måste toocreate två virtuella Azure-datorer på hello samma tillgänglighetsuppsättning:</span><span class="sxs-lookup"><span data-stu-id="a9e57-111">tooinstall Oracle Data Guard, you need toocreate two Azure VMs on hello same availability set:</span></span>

- <span data-ttu-id="a9e57-112">hello har primära virtuella datorn (myVM1) en Oracle-instans som körs.</span><span class="sxs-lookup"><span data-stu-id="a9e57-112">hello primary VM (myVM1) has a running Oracle instance.</span></span>
- <span data-ttu-id="a9e57-113">hello vänteläge VM (myVM2) har hello Oracle programvaran endast.</span><span class="sxs-lookup"><span data-stu-id="a9e57-113">hello standby VM (myVM2) has hello Oracle software installed only.</span></span>

<span data-ttu-id="a9e57-114">hello Marketplace-avbildning som du använder toocreate hello virtuella datorer är Oracle: Oracle-databasen-Ee:12.1.0.2:latest.</span><span class="sxs-lookup"><span data-stu-id="a9e57-114">hello Marketplace image that you use toocreate hello VMs is Oracle:Oracle-Database-Ee:12.1.0.2:latest.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="a9e57-115">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="a9e57-115">Sign in tooAzure</span></span> 

<span data-ttu-id="a9e57-116">Logga in på tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="a9e57-116">Sign in tooyour Azure subscription by using hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

```azurecli
az login
```

### <a name="create-a-resource-group"></a><span data-ttu-id="a9e57-117">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a9e57-117">Create a resource group</span></span>

<span data-ttu-id="a9e57-118">Skapa en resursgrupp med hjälp av hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="a9e57-118">Create a resource group by using hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="a9e57-119">En Azure-resursgrupp är en logisk behållare i vilka Azure resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="a9e57-119">An Azure resource group is a logical container in which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="a9e57-120">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westus` plats:</span><span class="sxs-lookup"><span data-stu-id="a9e57-120">hello following example creates a resource group named `myResourceGroup` in hello `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a><span data-ttu-id="a9e57-121">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="a9e57-121">Create an availability set</span></span>

<span data-ttu-id="a9e57-122">Skapa en tillgänglighetsuppsättning är valfritt men rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="a9e57-122">Creating an availability set is optional, but we recommend it.</span></span> <span data-ttu-id="a9e57-123">Mer information finns i [Azure tillgänglighetsuppsättningar riktlinjer](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span><span class="sxs-lookup"><span data-stu-id="a9e57-123">For more information, see [Azure availability sets guidelines](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a><span data-ttu-id="a9e57-124">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a9e57-124">Create a virtual machine</span></span>

<span data-ttu-id="a9e57-125">Skapa en virtuell dator med hjälp av hello [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="a9e57-125">Create a VM by using hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="a9e57-126">hello följande exempel skapar två virtuella datorer med namnet `myVM1` och `myVM2`.</span><span class="sxs-lookup"><span data-stu-id="a9e57-126">hello following example creates two VMs named `myVM1` and `myVM2`.</span></span> <span data-ttu-id="a9e57-127">Det skapar också SSH-nycklar, om de inte redan finns på standardplatsen nyckel.</span><span class="sxs-lookup"><span data-stu-id="a9e57-127">It also creates SSH keys, if they do not already exist in a default key location.</span></span> <span data-ttu-id="a9e57-128">toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.</span><span class="sxs-lookup"><span data-stu-id="a9e57-128">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>

<span data-ttu-id="a9e57-129">Skapa myVM1 (primära):</span><span class="sxs-lookup"><span data-stu-id="a9e57-129">Create myVM1 (primary):</span></span>
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

<span data-ttu-id="a9e57-130">När du har skapat hello VM visar Azure CLI information liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="a9e57-130">After you create hello VM, Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="a9e57-131">Observera hello värdet för `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="a9e57-131">Note hello value of `publicIpAddress`.</span></span> <span data-ttu-id="a9e57-132">Du kan använda den här adressen tooaccess hello VM.</span><span class="sxs-lookup"><span data-stu-id="a9e57-132">You use this address tooaccess hello VM.</span></span>

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

<span data-ttu-id="a9e57-133">Skapa myVM2 (standby):</span><span class="sxs-lookup"><span data-stu-id="a9e57-133">Create myVM2 (standby):</span></span>
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

<span data-ttu-id="a9e57-134">Observera hello värdet för `publicIpAddress` när du har skapat myVM2.</span><span class="sxs-lookup"><span data-stu-id="a9e57-134">Note hello value of `publicIpAddress` after you create myVM2.</span></span>

### <a name="open-hello-tcp-port-for-connectivity"></a><span data-ttu-id="a9e57-135">Öppna hello TCP-port för anslutning</span><span class="sxs-lookup"><span data-stu-id="a9e57-135">Open hello TCP port for connectivity</span></span>

<span data-ttu-id="a9e57-136">Det här steget konfigurerar externa slutpunkter som tillåter fjärråtkomst toohello Oracle-databas.</span><span class="sxs-lookup"><span data-stu-id="a9e57-136">This step configures external endpoints, which allow remote access toohello Oracle database.</span></span>

<span data-ttu-id="a9e57-137">Öppna hello port för myVM1:</span><span class="sxs-lookup"><span data-stu-id="a9e57-137">Open hello port for myVM1:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVM1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

<span data-ttu-id="a9e57-138">hello resultatet bör se ut ungefär toohello efter svar:</span><span class="sxs-lookup"><span data-stu-id="a9e57-138">hello result should look similar toohello following response:</span></span>

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

<span data-ttu-id="a9e57-139">Öppna hello port för myVM2:</span><span class="sxs-lookup"><span data-stu-id="a9e57-139">Open hello port for myVM2:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVM2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-toohello-virtual-machine"></a><span data-ttu-id="a9e57-140">Ansluta toohello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a9e57-140">Connect toohello virtual machine</span></span>

<span data-ttu-id="a9e57-141">Använd hello följande kommando toocreate en SSH-session med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a9e57-141">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="a9e57-142">Ersätt hello IP-adress med hello `publicIpAddress` värde för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a9e57-142">Replace hello IP address with hello `publicIpAddress` value for your virtual machine.</span></span>

```bash 
$ ssh azureuser@<publicIpAddress>
```

### <a name="create-hello-database-on-myvm1-primary"></a><span data-ttu-id="a9e57-143">Skapa hello-databas på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="a9e57-143">Create hello database on myVM1 (primary)</span></span>

<span data-ttu-id="a9e57-144">hello Oracle-programvara är installerad på hello Marketplace-avbildning, så hello nästa steg är tooinstall hello-databas.</span><span class="sxs-lookup"><span data-stu-id="a9e57-144">hello Oracle software is already installed on hello Marketplace image, so hello next step is tooinstall hello database.</span></span> 

<span data-ttu-id="a9e57-145">Växla toohello Oracle superanvändare:</span><span class="sxs-lookup"><span data-stu-id="a9e57-145">Switch toohello Oracle superuser:</span></span>

```bash
$ sudo su - oracle
```

<span data-ttu-id="a9e57-146">Skapa databas för hello:</span><span class="sxs-lookup"><span data-stu-id="a9e57-146">Create hello database:</span></span>

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
<span data-ttu-id="a9e57-147">Utdata ska se ut ungefär toohello efter svar:</span><span class="sxs-lookup"><span data-stu-id="a9e57-147">Outputs should look similar toohello following response:</span></span>

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

<span data-ttu-id="a9e57-148">Ange hello ORACLE_SID och ORACLE_HOME variabler:</span><span class="sxs-lookup"><span data-stu-id="a9e57-148">Set hello ORACLE_SID and ORACLE_HOME variables:</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
```

<span data-ttu-id="a9e57-149">Alternativt kan du lägga till ORACLE_HOME och ORACLE_SID toohello /home/oracle/.bashrc filen, så att de här inställningarna sparas för framtida inloggningar:</span><span class="sxs-lookup"><span data-stu-id="a9e57-149">Optionally, you can add ORACLE_HOME and ORACLE_SID toohello /home/oracle/.bashrc file, so that these settings are saved for future logins:</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

## <a name="configure-data-guard"></a><span data-ttu-id="a9e57-150">Konfigurera Data Guard</span><span class="sxs-lookup"><span data-stu-id="a9e57-150">Configure Data Guard</span></span>

### <a name="enable-archive-log-mode-on-myvm1-primary"></a><span data-ttu-id="a9e57-151">Aktivera archive log-läge på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="a9e57-151">Enable archive log mode on myVM1 (primary)</span></span>

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
<span data-ttu-id="a9e57-152">Aktivera loggning för kraft och se till att minst en loggfil finns:</span><span class="sxs-lookup"><span data-stu-id="a9e57-152">Enable force logging, and make sure at least one log file is present:</span></span>

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
```

<span data-ttu-id="a9e57-153">Skapa vänteläge gör om-loggar:</span><span class="sxs-lookup"><span data-stu-id="a9e57-153">Create standby redo logs:</span></span>

```bash
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo01.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo02.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo03.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo04.log') SIZE 50M;
```

<span data-ttu-id="a9e57-154">Aktivera Flashback (vilket gör återställningen mycket enklare) och ange VÄNTELÄGE\_filen\_MANAGEMENT tooauto.</span><span class="sxs-lookup"><span data-stu-id="a9e57-154">Turn on Flashback (which makes recovery a lot easier) and set STANDBY\_FILE\_MANAGEMENT tooauto.</span></span> <span data-ttu-id="a9e57-155">Avsluta SQL * Plus efter.</span><span class="sxs-lookup"><span data-stu-id="a9e57-155">Exit SQL*Plus after that.</span></span>

```bash
SQL> ALTER DATABASE FLASHBACK ON;
SQL> ALTER SYSTEM SET STANDBY_FILE_MANAGEMENT=AUTO;
SQL> EXIT;
```

### <a name="set-up-service-on-myvm1-primary"></a><span data-ttu-id="a9e57-156">Ställ in tjänsten på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="a9e57-156">Set up service on myVM1 (primary)</span></span>

<span data-ttu-id="a9e57-157">Redigera eller skapa hello tnsnames.ora-filen i hello $ORACLE_HOME\network\admin mapp.</span><span class="sxs-lookup"><span data-stu-id="a9e57-157">Edit or create hello tnsnames.ora file, which is in hello $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="a9e57-158">Lägg till följande poster hello:</span><span class="sxs-lookup"><span data-stu-id="a9e57-158">Add hello following entries:</span></span>

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

<span data-ttu-id="a9e57-159">Redigera eller skapa hello listener.ora-filen i hello $ORACLE_HOME\network\admin mapp.</span><span class="sxs-lookup"><span data-stu-id="a9e57-159">Edit or create hello listener.ora file, which is in hello $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="a9e57-160">Lägg till följande poster hello:</span><span class="sxs-lookup"><span data-stu-id="a9e57-160">Add hello following entries:</span></span>

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

<span data-ttu-id="a9e57-161">Aktivera Data Guard Broker:</span><span class="sxs-lookup"><span data-stu-id="a9e57-161">Enable Data Guard Broker:</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```
<span data-ttu-id="a9e57-162">Starta hello lyssnare:</span><span class="sxs-lookup"><span data-stu-id="a9e57-162">Start hello listener:</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```

### <a name="set-up-service-on-myvm2-standby"></a><span data-ttu-id="a9e57-163">Ställ in tjänsten på myVM2 (standby)</span><span class="sxs-lookup"><span data-stu-id="a9e57-163">Set up service on myVM2 (standby)</span></span>

<span data-ttu-id="a9e57-164">SSH-toomyVM2:</span><span class="sxs-lookup"><span data-stu-id="a9e57-164">SSH toomyVM2:</span></span>

```bash 
$ ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="a9e57-165">Logga in som Oracle:</span><span class="sxs-lookup"><span data-stu-id="a9e57-165">Log in as Oracle:</span></span>

```bash
$ sudo su - oracle
```

<span data-ttu-id="a9e57-166">Redigera eller skapa hello tnsnames.ora-filen i hello $ORACLE_HOME\network\admin mapp.</span><span class="sxs-lookup"><span data-stu-id="a9e57-166">Edit or create hello tnsnames.ora file, which is in hello $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="a9e57-167">Lägg till följande poster hello:</span><span class="sxs-lookup"><span data-stu-id="a9e57-167">Add hello following entries:</span></span>

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

<span data-ttu-id="a9e57-168">Redigera eller skapa hello listener.ora-filen i hello $ORACLE_HOME\network\admin mapp.</span><span class="sxs-lookup"><span data-stu-id="a9e57-168">Edit or create hello listener.ora file, which is in hello $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="a9e57-169">Lägg till följande poster hello:</span><span class="sxs-lookup"><span data-stu-id="a9e57-169">Add hello following entries:</span></span>

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

<span data-ttu-id="a9e57-170">Starta hello lyssnare:</span><span class="sxs-lookup"><span data-stu-id="a9e57-170">Start hello listener:</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```


### <a name="restore-hello-database-toomyvm2-standby"></a><span data-ttu-id="a9e57-171">Återställa hello databasen toomyVM2 (standby)</span><span class="sxs-lookup"><span data-stu-id="a9e57-171">Restore hello database toomyVM2 (standby)</span></span>

<span data-ttu-id="a9e57-172">Skapa hello parametern filen /tmp/initcdb1_stby.ora med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="a9e57-172">Create hello parameter file /tmp/initcdb1_stby.ora with hello following contents:</span></span>
```bash
*.db_name='cdb1'
```

<span data-ttu-id="a9e57-173">Skapa mappar:</span><span class="sxs-lookup"><span data-stu-id="a9e57-173">Create folders:</span></span>

```bash
mkdir -p /u01/app/oracle/oradata/cdb1/pdbseed
mkdir -p /u01/app/oracle/oradata/cdb1/pdb1
mkdir -p /u01/app/oracle/fast_recovery_area/cdb1
mkdir -p /u01/app/oracle/admin/cdb1/adump
```

<span data-ttu-id="a9e57-174">Skapa en lösenordsfil:</span><span class="sxs-lookup"><span data-stu-id="a9e57-174">Create a password file:</span></span>

```bash
$ orapwd file=/u01/app/oracle/product/12.1.0/dbhome_1/dbs/orapwcdb1 password=OraPasswd1 entries=10
```
<span data-ttu-id="a9e57-175">Starta hello databasen på myVM2:</span><span class="sxs-lookup"><span data-stu-id="a9e57-175">Start hello database on myVM2:</span></span>

```bash
$ export ORACLE_SID=cdb1
$ sqlplus / as sysdba

SQL> STARTUP NOMOUNT PFILE='/tmp/initcdb1_stby.ora';
SQL> EXIT;
```

<span data-ttu-id="a9e57-176">Återställa hello-databasen med hjälp av hello RMAN verktyget:</span><span class="sxs-lookup"><span data-stu-id="a9e57-176">Restore hello database by using hello RMAN tool:</span></span>

```bash
$ rman TARGET sys/OraPasswd1@cdb1 AUXILIARY sys/OraPasswd1@cdb1_stby
```

<span data-ttu-id="a9e57-177">Kör följande kommandon i RMAN hello:</span><span class="sxs-lookup"><span data-stu-id="a9e57-177">Run hello following commands in RMAN:</span></span>
```bash
DUPLICATE TARGET DATABASE
  FOR STANDBY
  FROM ACTIVE DATABASE
  DORECOVER
  SPFILE
    SET db_unique_name='CDB1_STBY' COMMENT 'Is standby'
  NOFILENAMECHECK;
```

<span data-ttu-id="a9e57-178">Du bör se meddelanden liknande toohello följande när hello-kommandot har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a9e57-178">You should see messages similar toohello following when hello command is completed.</span></span> <span data-ttu-id="a9e57-179">Avsluta RMAN.</span><span class="sxs-lookup"><span data-stu-id="a9e57-179">Exit RMAN.</span></span>
```bash
media recovery complete, elapsed time: 00:00:00
Finished recover at 29-JUN-17
Finished Duplicate Db at 29-JUN-17

RMAN> EXIT;
```

<span data-ttu-id="a9e57-180">Alternativt kan du lägga till ORACLE_HOME och ORACLE_SID toohello /home/oracle/.bashrc filen, så att de här inställningarna sparas för framtida inloggningar:</span><span class="sxs-lookup"><span data-stu-id="a9e57-180">Optionally, you can add ORACLE_HOME and ORACLE_SID toohello /home/oracle/.bashrc file, so that these settings are saved for future logins:</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

<span data-ttu-id="a9e57-181">Aktivera Data Guard Broker:</span><span class="sxs-lookup"><span data-stu-id="a9e57-181">Enable Data Guard Broker:</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="configure-data-guard-broker-on-myvm1-primary"></a><span data-ttu-id="a9e57-182">Konfigurera Data Guard Broker på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="a9e57-182">Configure Data Guard Broker on myVM1 (primary)</span></span>

<span data-ttu-id="a9e57-183">Starta Data Guard Manager och logga in med hjälp av SYS och ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="a9e57-183">Start Data Guard Manager and log in by using SYS and a password.</span></span> <span data-ttu-id="a9e57-184">(Använd inte OS-autentisering.) Utför hello följande:</span><span class="sxs-lookup"><span data-stu-id="a9e57-184">(Do not use OS authentication.) Perform hello following:</span></span>

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

<span data-ttu-id="a9e57-185">Granska hello konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="a9e57-185">Review hello configuration:</span></span>
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

<span data-ttu-id="a9e57-186">Du har slutfört installationen av hello Oracle Data Guard.</span><span class="sxs-lookup"><span data-stu-id="a9e57-186">You've completed hello Oracle Data Guard setup.</span></span> <span data-ttu-id="a9e57-187">hello nästa avsnitt visar hur tootest hello anslutning och växla.</span><span class="sxs-lookup"><span data-stu-id="a9e57-187">hello next section shows you how tootest hello connectivity and switch over.</span></span>

### <a name="connect-hello-database-from-hello-client-machine"></a><span data-ttu-id="a9e57-188">Ansluta hello databasen från hello-klientdatorn</span><span class="sxs-lookup"><span data-stu-id="a9e57-188">Connect hello database from hello client machine</span></span>

<span data-ttu-id="a9e57-189">Uppdatera eller skapa hello tnsnames.ora fil på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="a9e57-189">Update or create hello tnsnames.ora file on your client machine.</span></span> <span data-ttu-id="a9e57-190">Den här filen har vanligtvis $ORACLE_HOME\network\admin.</span><span class="sxs-lookup"><span data-stu-id="a9e57-190">This file is usually in $ORACLE_HOME\network\admin.</span></span>

<span data-ttu-id="a9e57-191">Ersätt hello IP-adresser med din `publicIpAddress` värden för myVM1 och myVM2:</span><span class="sxs-lookup"><span data-stu-id="a9e57-191">Replace hello IP addresses with your `publicIpAddress` values for myVM1 and myVM2:</span></span>

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

<span data-ttu-id="a9e57-192">Starta SQL * Plus:</span><span class="sxs-lookup"><span data-stu-id="a9e57-192">Start SQL*Plus:</span></span>

```bash
$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```
## <a name="test-hello-data-guard-configuration"></a><span data-ttu-id="a9e57-193">Testkonfiguration hello Data Guard</span><span class="sxs-lookup"><span data-stu-id="a9e57-193">Test hello Data Guard configuration</span></span>

### <a name="switch-over-hello-database-on-myvm1-primary"></a><span data-ttu-id="a9e57-194">Växla över hello-databas på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="a9e57-194">Switch over hello database on myVM1 (primary)</span></span>

<span data-ttu-id="a9e57-195">tooswitch från primära toostandby (cdb1 toocdb1_stby):</span><span class="sxs-lookup"><span data-stu-id="a9e57-195">tooswitch from primary toostandby (cdb1 toocdb1_stby):</span></span>

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

<span data-ttu-id="a9e57-196">Nu kan du ansluta toohello standby-databas.</span><span class="sxs-lookup"><span data-stu-id="a9e57-196">You can now connect toohello standby database.</span></span>

<span data-ttu-id="a9e57-197">Starta SQL * Plus:</span><span class="sxs-lookup"><span data-stu-id="a9e57-197">Start SQL*Plus:</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1_stby
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

### <a name="switch-over-hello-database-on-myvm2-standby"></a><span data-ttu-id="a9e57-198">Växla över hello-databas på myVM2 (standby)</span><span class="sxs-lookup"><span data-stu-id="a9e57-198">Switch over hello database on myVM2 (standby)</span></span>

<span data-ttu-id="a9e57-199">tooswitch över, kör myVM2 hello följande:</span><span class="sxs-lookup"><span data-stu-id="a9e57-199">tooswitch over, run hello following on myVM2:</span></span>
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

<span data-ttu-id="a9e57-200">Igen nu bör du kunna tooconnect toohello primära databasen.</span><span class="sxs-lookup"><span data-stu-id="a9e57-200">Once again, you should now be able tooconnect toohello primary database.</span></span>

<span data-ttu-id="a9e57-201">Starta SQL * Plus:</span><span class="sxs-lookup"><span data-stu-id="a9e57-201">Start SQL*Plus:</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

<span data-ttu-id="a9e57-202">Du är klar hello installationen och konfigurationen av Data Guard på Oracle Linux.</span><span class="sxs-lookup"><span data-stu-id="a9e57-202">You've finished hello installation and configuration of Data Guard on Oracle Linux.</span></span>


## <a name="delete-hello-virtual-machine"></a><span data-ttu-id="a9e57-203">Ta bort hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a9e57-203">Delete hello virtual machine</span></span>

<span data-ttu-id="a9e57-204">När du behöver inte längre hello VM, kan du använda hello efter kommandot tooremove hello resursgrupp, VM och alla relaterade resurser:</span><span class="sxs-lookup"><span data-stu-id="a9e57-204">When you no longer need hello VM, you can use hello following command tooremove hello resource group, VM, and all related resources:</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="a9e57-205">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9e57-205">Next steps</span></span>

[<span data-ttu-id="a9e57-206">Självstudier: Skapa virtuella datorer med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="a9e57-206">Tutorial: Create highly available virtual machines</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="a9e57-207">Utforska VM distribution Azure CLI-exempel</span><span class="sxs-lookup"><span data-stu-id="a9e57-207">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
