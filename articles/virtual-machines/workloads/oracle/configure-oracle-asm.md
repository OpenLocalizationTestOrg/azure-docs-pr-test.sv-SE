---
title: "aaaSet in Oracle ASM på en virtuell Azure Linux-dator | Microsoft Docs"
description: "Snabbt Oracle ASM upp och körs i Azure-miljön."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/19/2017
ms.author: rclaus
ms.openlocfilehash: d6a7046638e919876477d46943faabcb1872acac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-oracle-asm-on-an-azure-linux-virtual-machine"></a><span data-ttu-id="04727-103">Ställa in Oracle ASM på en virtuell Azure Linux-dator</span><span class="sxs-lookup"><span data-stu-id="04727-103">Set up Oracle ASM on an Azure Linux virtual machine</span></span>  

<span data-ttu-id="04727-104">Virtuella datorer i Azure ger en fullständigt konfigurerbara och flexibel datormiljö.</span><span class="sxs-lookup"><span data-stu-id="04727-104">Azure virtual machines provide a fully configurable and flexible computing environment.</span></span> <span data-ttu-id="04727-105">Den här självstudiekursen beskriver distributionen av grundläggande Azure virtuella datorer i kombination med hello installation och konfiguration av Oracle Automated Storage Management (ASM).</span><span class="sxs-lookup"><span data-stu-id="04727-105">This tutorial covers basic Azure virtual machine deployment combined with hello installation and configuration of Oracle Automated Storage Management (ASM).</span></span>  <span data-ttu-id="04727-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="04727-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="04727-107">Skapa och ansluta tooan VM för Oracle-databas</span><span class="sxs-lookup"><span data-stu-id="04727-107">Create and connect tooan Oracle Database VM</span></span>
> * <span data-ttu-id="04727-108">Installera och konfigurera automatisk lagringshantering för Oracle</span><span class="sxs-lookup"><span data-stu-id="04727-108">Install and configure Oracle Automated Storage Management</span></span>
> * <span data-ttu-id="04727-109">Installera och konfigurera infrastrukturen för Oracle rutnätet</span><span class="sxs-lookup"><span data-stu-id="04727-109">Install and configure Oracle Grid infrastructure</span></span>
> * <span data-ttu-id="04727-110">Initiera en Oracle ASM-installation</span><span class="sxs-lookup"><span data-stu-id="04727-110">Initialize an Oracle ASM installation</span></span>
> * <span data-ttu-id="04727-111">Skapa en Oracle-databas som hanteras av ASM</span><span class="sxs-lookup"><span data-stu-id="04727-111">Create an Oracle DB managed by ASM</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="04727-112">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="04727-112">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="04727-113">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="04727-113">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="04727-114">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="04727-114">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prepare-hello-environment"></a><span data-ttu-id="04727-115">Förbered hello-miljön</span><span class="sxs-lookup"><span data-stu-id="04727-115">Prepare hello environment</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="04727-116">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="04727-116">Create a resource group</span></span>

<span data-ttu-id="04727-117">toocreate en resursgrupp, använda hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="04727-117">toocreate a resource group, use hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="04727-118">En Azure-resursgrupp är en logisk behållare i vilka Azure resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="04727-118">An Azure resource group is a logical container in which Azure resources are deployed and managed.</span></span> <span data-ttu-id="04727-119">I det här exemplet en resursgrupp med namnet *myResourceGroup* i hello *eastus* region.</span><span class="sxs-lookup"><span data-stu-id="04727-119">In this example, a resource group named *myResourceGroup* in hello *eastus* region.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-vm"></a><span data-ttu-id="04727-120">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="04727-120">Create a VM</span></span>

<span data-ttu-id="04727-121">toocreate en virtuell dator baserat på hello Oracle-databas avbildningen och konfigurera den toouse Oracle ASM använder hello [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="04727-121">toocreate a virtual machine based on hello Oracle Database image and configure it toouse Oracle ASM, use hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="04727-122">hello skapas följande exempel en virtuell dator med namnet myVM som har en Standard_DS2_v2 storlek med fyra bifogade datadiskar på 50 GB.</span><span class="sxs-lookup"><span data-stu-id="04727-122">hello following example creates a VM named myVM that is a Standard_DS2_v2 size with four attached data disks of 50 GB each.</span></span> <span data-ttu-id="04727-123">Om de inte redan finns i hello standardplatsen för nyckeln, skapas också SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="04727-123">If they do not already exist in hello default key location, it also creates SSH keys.</span></span>  <span data-ttu-id="04727-124">toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.</span><span class="sxs-lookup"><span data-stu-id="04727-124">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

   ```azurecli-interactive
   az vm create --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50 50 50 50
   ```

<span data-ttu-id="04727-125">När du har skapat hello VM visar Azure CLI information liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="04727-125">After you create hello VM, Azure CLI displays information similar toohello following example.</span></span> <span data-ttu-id="04727-126">Observera hello värde för `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="04727-126">Note hello value for `publicIpAddress`.</span></span> <span data-ttu-id="04727-127">Du kan använda den här adressen tooaccess hello VM.</span><span class="sxs-lookup"><span data-stu-id="04727-127">You use this address tooaccess hello VM.</span></span>

   ```azurecli
   {
     "fqdns": "",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
     "location": "eastus",
     "macAddress": "00-0D-3A-36-2F-56",
     "powerState": "VM running",
     "privateIpAddress": "10.0.0.4",
     "publicIpAddress": "13.64.104.241",
     "resourceGroup": "myResourceGroup"
   }
   ```

### <a name="connect-toohello-vm"></a><span data-ttu-id="04727-128">Ansluta toohello VM</span><span class="sxs-lookup"><span data-stu-id="04727-128">Connect toohello VM</span></span>

<span data-ttu-id="04727-129">toocreate en SSH-session med hello VM och konfigurera ytterligare inställningar kan använda följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="04727-129">toocreate an SSH session with hello VM and configure additional settings, use hello following command.</span></span> <span data-ttu-id="04727-130">Ersätt hello IP-adress med hello `publicIpAddress` värde för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="04727-130">Replace hello IP address with hello `publicIpAddress` value for your VM.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="install-oracle-asm"></a><span data-ttu-id="04727-131">Installera Oracle ASM</span><span class="sxs-lookup"><span data-stu-id="04727-131">Install Oracle ASM</span></span>

<span data-ttu-id="04727-132">tooinstall Oracle ASM, fullständig hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="04727-132">tooinstall Oracle ASM, complete hello following steps.</span></span> 

<span data-ttu-id="04727-133">Mer information om hur du installerar Oracle ASM finns [Oracle ASMLib laddar ned för Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html).</span><span class="sxs-lookup"><span data-stu-id="04727-133">For more information about installing Oracle ASM, see [Oracle ASMLib Downloads for Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html).</span></span>  

1. <span data-ttu-id="04727-134">Du behöver toologin som rot i ordning toocontinue med ASM installation:</span><span class="sxs-lookup"><span data-stu-id="04727-134">You need toologin as root in order toocontinue with ASM installation:</span></span>

   ```bash
   sudo su -
   ```
   
2. <span data-ttu-id="04727-135">Kör kommandona ytterligare tooinstall Oracle ASM komponenter:</span><span class="sxs-lookup"><span data-stu-id="04727-135">Run these additional commands tooinstall Oracle ASM components:</span></span>

   ```bash
    yum list | grep oracleasm 
    yum -y install kmod-oracleasm.x86_64 
    yum -y install oracleasm-support.x86_64 
    wget http://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    yum -y install oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    rm -f oracleasmlib-2.0.12-1.el6.x86_64.rpm
   ```

3. <span data-ttu-id="04727-136">Kontrollera att Oracle ASM är installerad:</span><span class="sxs-lookup"><span data-stu-id="04727-136">Verify that Oracle ASM is installed:</span></span>

   ```bash
   rpm -qa |grep oracleasm
   ```

    <span data-ttu-id="04727-137">hello utdata från kommandot ska visa en lista över hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="04727-137">hello output of this command should list hello following components:</span></span>

    ```bash
   oracleasm-support-2.1.10-4.el6.x86_64
   kmod-oracleasm-2.0.8-15.el6_9.x86_64
   oracleasmlib-2.0.12-1.el6.x86_64
    ```

4. <span data-ttu-id="04727-138">ASM kräver särskilda användare och roller i ordning toofunction korrekt.</span><span class="sxs-lookup"><span data-stu-id="04727-138">ASM requires specific users and roles in order toofunction correctly.</span></span> <span data-ttu-id="04727-139">hello följande kommandon skapar hello krävs användarkonton och grupper:</span><span class="sxs-lookup"><span data-stu-id="04727-139">hello following commands create hello pre-requisite user accounts and groups:</span></span> 

   ```bash
    groupadd -g 54345 asmadmin 
    groupadd -g 54346 asmdba 
    groupadd -g 54347 asmoper 
    useradd -u 3000 -g oinstall -G dba,asmadmin,asmdba,asmoper grid 
    usermod -g oinstall -G dba,asmdba,asmadmin oracle
   ```

5. <span data-ttu-id="04727-140">Kontrollera att användare och grupper har skapats på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="04727-140">Verify users and groups were created correctly:</span></span>

   ```bash
   id grid
   ```

    <span data-ttu-id="04727-141">hello utdata från kommandot ska visa en lista över hello följande användare och grupper:</span><span class="sxs-lookup"><span data-stu-id="04727-141">hello output of this command should list hello following users and groups:</span></span>

    ```bash
    uid=3000(grid) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54345(asmadmin),54346(asmdba),54347(asmoper)
    ```
 
6. <span data-ttu-id="04727-142">Skapa en mapp för användaren *rutnätet* och ändra hello ägare:</span><span class="sxs-lookup"><span data-stu-id="04727-142">Create a folder for user *grid* and change hello owner:</span></span>

   ```bash
   mkdir /u01/app/grid 
   chown grid:oinstall /u01/app/grid
   ```

## <a name="set-up-oracle-asm"></a><span data-ttu-id="04727-143">Ställ in Oracle ASM</span><span class="sxs-lookup"><span data-stu-id="04727-143">Set up Oracle ASM</span></span>

<span data-ttu-id="04727-144">I den här självstudien är hello standardanvändaren *rutnätet* och hello standardgruppen är *asmadmin*.</span><span class="sxs-lookup"><span data-stu-id="04727-144">For this tutorial, hello default user is *grid* and hello default group is *asmadmin*.</span></span> <span data-ttu-id="04727-145">Se till att hello *oracle* användaren är en del av hello asmadmin grupp.</span><span class="sxs-lookup"><span data-stu-id="04727-145">Ensure that hello *oracle* user is part of hello asmadmin group.</span></span> <span data-ttu-id="04727-146">tooset in Oracle ASM-installation, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="04727-146">tooset up your Oracle ASM installation, complete hello following steps:</span></span>

1. <span data-ttu-id="04727-147">Konfigurera hello Oracle ASM biblioteket drivrutinen innebär att definiera hello standardanvändaren (rutnätet) och standardgruppen (asmadmin) samt konfigurera hello enhet toostart på Start (Välj y) och tooscan diskar på Start (Välj y).</span><span class="sxs-lookup"><span data-stu-id="04727-147">Setting up hello Oracle ASM library driver involves defining hello default user (grid) and default group (asmadmin) as well as configuring hello drive toostart on boot (choose y) and tooscan for disks on boot (choose y).</span></span> <span data-ttu-id="04727-148">Du behöver tooanswer hello anvisningarna från hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="04727-148">You need tooanswer hello prompts from hello following command:</span></span>

   ```bash
   /usr/sbin/oracleasm configure -i
   ```

   <span data-ttu-id="04727-149">hello utdata från kommandot ska se ut liknande toohello efter, stoppar med prompter toobe besvaras.</span><span class="sxs-lookup"><span data-stu-id="04727-149">hello output of this command should look similar toohello following, stopping with prompts toobe answered.</span></span>

    ```bash
   Configuring hello Oracle ASM library driver.

   This will configure hello on-boot properties of hello Oracle ASM library
   driver. hello following questions will determine whether hello driver is
   loaded on boot and what permissions it will have. hello current values
   will be shown in brackets ('[]'). Hitting <ENTER> without typing an
   answer will keep that current value. Ctrl-C will abort.

   Default user tooown hello driver interface []: grid
   Default group tooown hello driver interface []: asmadmin
   Start Oracle ASM library driver on boot (y/n) [n]: y
   Scan for Oracle ASM disks on boot (y/n) [y]: y
   Writing Oracle ASM library driver configuration: done
   ```

2. <span data-ttu-id="04727-150">Visa hello diskkonfiguration:</span><span class="sxs-lookup"><span data-stu-id="04727-150">View hello disk configuration:</span></span>
   ```bash
   cat /proc/partitions
   ```

   <span data-ttu-id="04727-151">hello utdata från kommandot ska se ut ungefär toohello följande lista över tillgängliga diskar</span><span class="sxs-lookup"><span data-stu-id="04727-151">hello output of this command should look similar toohello following listing of available disks</span></span>

   ```bash
   8       16   14680064 sdb
   8       17   14678976 sdb1
   8        0   52428800 sda
   8        1     512000 sda1
   8        2   51915776 sda2
   8       48   52428800 sdd
   8       64   52428800 sde
   8       80   52428800 sdf
   8       32   52428800 sdc
   11       0       1152 sr0
   ```

3. <span data-ttu-id="04727-152">Formatera disk */dev/sdc* genom att köra hello följande kommando och svara på hello prompter med:</span><span class="sxs-lookup"><span data-stu-id="04727-152">Format disk */dev/sdc* by running hello following command and answering hello prompts with:</span></span>
   - <span data-ttu-id="04727-153">*n*för nya partitionen</span><span class="sxs-lookup"><span data-stu-id="04727-153">*n* for new partition</span></span>
   - <span data-ttu-id="04727-154">*p* för primär partition</span><span class="sxs-lookup"><span data-stu-id="04727-154">*p* for primary partition</span></span>
   - <span data-ttu-id="04727-155">*1* tooselect hello första partitionen</span><span class="sxs-lookup"><span data-stu-id="04727-155">*1* tooselect hello first partition</span></span>
   - <span data-ttu-id="04727-156">Tryck på `enter` för hello standard första cylinder</span><span class="sxs-lookup"><span data-stu-id="04727-156">press `enter` for hello default first cylinder</span></span>
   - <span data-ttu-id="04727-157">Tryck på `enter` för hello standard senaste cylinder</span><span class="sxs-lookup"><span data-stu-id="04727-157">press `enter` for hello default last cylinder</span></span>
   - <span data-ttu-id="04727-158">Tryck på *w* toowrite hello ändringar toohello partitionstabellen</span><span class="sxs-lookup"><span data-stu-id="04727-158">press *w* toowrite hello changes toohello partition table</span></span>  

   ```bash
   fdisk /dev/sdc
   ```
   
   <span data-ttu-id="04727-159">Använder hello-svar som anges ovan, ska hello utdata för hello fdisk kommandot se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="04727-159">Using hello answers provided above, hello output for hello fdisk command should look like hello following:</span></span>

   ```bash
   Device contains not a valid DOS partition table, or Sun, SGI or OSF disklabel
   Building a new DOS disklabel with disk identifier 0xf865c6ca.
   Changes will remain in memory only, until you decide toowrite them.
   After that, of course, hello previous content won't be recoverable.

   Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

   hello device presents a logical sector size that is smaller than
   hello physical sector size. Aligning tooa physical sector (or optimal
   I/O) size boundary is recommended, or performance may be impacted.

   WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
           switch off hello mode (command 'c') and change display units to
           sectors (command 'u').

   Command (m for help): n
   Command action
     e   extended
     p   primary partition (1-4)
   p
   Partition number (1-4): 1
   First cylinder (1-6527, default 1):
   Using default value 1
   Last cylinder, +cylinders or +size{K,M,G} (1-6527, default 6527):
   Using default value 6527

   Command (m for help): w
   hello partition table has been altered!

   Calling ioctl() toore-read partition table.
   Syncing disks.
   ```

4. <span data-ttu-id="04727-160">Upprepa hello föregående fdisk kommandot för `/dev/sdd`, `/dev/sde`, och `/dev/sdf`.</span><span class="sxs-lookup"><span data-stu-id="04727-160">Repeat hello preceding fdisk command for `/dev/sdd`, `/dev/sde`, and `/dev/sdf`.</span></span>

5. <span data-ttu-id="04727-161">Kontrollera diskkonfigurationen hello:</span><span class="sxs-lookup"><span data-stu-id="04727-161">Check hello disk configuration:</span></span>

   ```bash
   cat /proc/partitions
   ```

   <span data-ttu-id="04727-162">hello hello kommandots utdata ska se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="04727-162">hello output of hello command should look like hello following:</span></span>

   ```bash
   major minor  #blocks  name

     8       16   14680064 sdb
     8       17   14678976 sdb1
     8       32   52428800 sdc
     8       33   52428096 sdc1
     8       48   52428800 sdd
     8       49   52428096 sdd1
     8       64   52428800 sde
     8       65   52428096 sde1
     8       80   52428800 sdf
     8       81   52428096 sdf1
     8        0   52428800 sda
     8        1     512000 sda1
     8        2   51915776 sda2
     11       0    1048575 sr0
   ```

6. <span data-ttu-id="04727-163">Kontrollera status för hello Oracle ASM-tjänsten och starta hello Oracle ASM-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="04727-163">Check hello Oracle ASM service status and start hello Oracle ASM service:</span></span>

   ```bash
   service oracleasm status 
   service oracleasm start
   ```

   <span data-ttu-id="04727-164">hello hello kommandots utdata ska se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="04727-164">hello output of hello command should look like hello following:</span></span>
   
   ```bash
   Checking if ASM is loaded: no
   Checking if /dev/oracleasm is mounted: no
   Initializing hello Oracle ASMLib driver:                     [  OK  ]
   Scanning hello system for Oracle ASMLib disks:               [  OK  ]
   ```

7. <span data-ttu-id="04727-165">Skapa Oracle ASM diskar:</span><span class="sxs-lookup"><span data-stu-id="04727-165">Create Oracle ASM disks:</span></span>

   ```bash
   service oracleasm createdisk ASMSP /dev/sdc1 
   service oracleasm createdisk DATA /dev/sdd1 
   service oracleasm createdisk DATA1 /dev/sde1 
   service oracleasm createdisk FRA /dev/sdf1
   ```    

   <span data-ttu-id="04727-166">hello hello kommandots utdata ska se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="04727-166">hello output of hello command should look like hello following:</span></span>

   ```bash
   Marking disk "ASMSP" as an ASM disk:                       [  OK  ]
   Marking disk "DATA" as an ASM disk:                        [  OK  ]
   Marking disk "DATA1" as an ASM disk:                       [  OK  ]
   Marking disk "FRA" as an ASM disk:                         [  OK  ]
   ```

8. <span data-ttu-id="04727-167">Visa en lista med Oracle ASM diskar:</span><span class="sxs-lookup"><span data-stu-id="04727-167">List Oracle ASM disks:</span></span>

   ```bash
   service oracleasm listdisks
   ```   

   <span data-ttu-id="04727-168">hello utdatan hello kommandot lista av hello följande Oracle ASM diskar:</span><span class="sxs-lookup"><span data-stu-id="04727-168">hello output of hello command should list off hello following Oracle ASM disks:</span></span>

   ```bash
    ASMSP
    DATA
    DATA1
    FRA
   ```

9. <span data-ttu-id="04727-169">Ändra hello lösenord för hello rot, oracle och rutnätet användare.</span><span class="sxs-lookup"><span data-stu-id="04727-169">Change hello passwords for hello root, oracle, and grid users.</span></span> <span data-ttu-id="04727-170">**Anteckna dessa nya lösenord** som du använder dem senare under hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="04727-170">**Make note of these new passwords** as you are using them later during hello installation.</span></span>

   ```bash
   passwd oracle 
   passwd grid 
   passwd root
   ```

10. <span data-ttu-id="04727-171">Ändra behörigheten för hello-mapp:</span><span class="sxs-lookup"><span data-stu-id="04727-171">Change hello folder permission:</span></span>

   ```bash
   chmod -R 775 /opt 
   chown grid:oinstall /opt 
   chown oracle:oinstall /dev/sdc1 
   chown oracle:oinstall /dev/sdd1 
   chown oracle:oinstall /dev/sde1 
   chown oracle:oinstall /dev/sdf1 
   chmod 600 /dev/sdc1 
   chmod 600 /dev/sdd1 
   chmod 600 /dev/sde1 
   chmod 600 /dev/sdf1
   ```

## <a name="download-and-prepare-oracle-grid-infrastructure"></a><span data-ttu-id="04727-172">Hämta och förbereda infrastrukturen för Oracle-rutnätet</span><span class="sxs-lookup"><span data-stu-id="04727-172">Download and prepare Oracle Grid Infrastructure</span></span>

<span data-ttu-id="04727-173">toodownload och förbereda hello Oracle rutnätet infrastruktur programvara, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="04727-173">toodownload and prepare hello Oracle Grid Infrastructure software, complete hello following steps:</span></span>

1. <span data-ttu-id="04727-174">Hämta Oracle rutnätet infrastruktur från hello [hämtningssidan för Oracle ASM](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html).</span><span class="sxs-lookup"><span data-stu-id="04727-174">Download Oracle Grid Infrastructure from hello [Oracle ASM download page](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html).</span></span> 

   <span data-ttu-id="04727-175">Under hello hämta med titeln **Oracle-databas 12c Versionsinfrastruktur 1 rutnätet (12.1.0.2.0) för Linux x86-64**, hämta hello två ZIP-filer.</span><span class="sxs-lookup"><span data-stu-id="04727-175">Under hello download titled **Oracle Database 12c Release 1 Grid Infrastructure (12.1.0.2.0) for Linux x86-64**, download hello two .zip files.</span></span>

2. <span data-ttu-id="04727-176">Du kan använda säkra kopia Protocol (SCP) toocopy hello filer tooyour VM när du har hämtat hello .zip filer tooyour klientdatorn:</span><span class="sxs-lookup"><span data-stu-id="04727-176">After you download hello .zip files tooyour client computer, you can use Secure Copy Protocol (SCP) toocopy hello files tooyour VM:</span></span>

   ```bash
   scp *.zip <publicIpAddress>:.
   ```

3. <span data-ttu-id="04727-177">SSH tillbaka till din Oracle VM i Azure i ordning toomove hello ZIP-filer till hello eller välja mappen.</span><span class="sxs-lookup"><span data-stu-id="04727-177">SSH back into your Oracle VM in Azure in order toomove hello .zip files into hello /opt folder.</span></span> <span data-ttu-id="04727-178">Ändra hello ägare av hello filer:</span><span class="sxs-lookup"><span data-stu-id="04727-178">Then, change hello owner of hello files:</span></span>

   ```bash
   ssh <publicIPAddress>
   sudo mv ./*.zip /opt
   cd /opt
   sudo chown grid:oinstall linuxamd64_12102_grid_1of2.zip
   sudo chown grid:oinstall linuxamd64_12102_grid_2of2.zip
   ```

4. <span data-ttu-id="04727-179">Packa upp hello-filer.</span><span class="sxs-lookup"><span data-stu-id="04727-179">Unzip hello files.</span></span> <span data-ttu-id="04727-180">(Installera hello Linux packa upp verktyget om den inte redan är installerad.)</span><span class="sxs-lookup"><span data-stu-id="04727-180">(Install hello Linux unzip tool if it's not already installed.)</span></span>
   
   ```bash
   sudo yum install unzip
   sudo unzip linuxamd64_12102_grid_1of2.zip
   sudo unzip linuxamd64_12102_grid_2of2.zip
   ```

5. <span data-ttu-id="04727-181">Ändra behörigheter:</span><span class="sxs-lookup"><span data-stu-id="04727-181">Change permission:</span></span>
   
   ```bash
   sudo chown -R grid:oinstall /opt/grid
   ```

6. <span data-ttu-id="04727-182">Uppdatera konfigurerats växlingsutrymme.</span><span class="sxs-lookup"><span data-stu-id="04727-182">Update configured swap space.</span></span> <span data-ttu-id="04727-183">Oracle rutnätet komponenter måste minst 6,8 GB växlingen utrymme tooinstall rutnätet.</span><span class="sxs-lookup"><span data-stu-id="04727-183">Oracle Grid components need at least 6.8 GB of swap space tooinstall Grid.</span></span> <span data-ttu-id="04727-184">standard hello växlingen filstorleken för Oracle Linux-avbildningar i Azure är endast 2 048 MB.</span><span class="sxs-lookup"><span data-stu-id="04727-184">hello default swap file size for Oracle Linux images in Azure is only 2048MB.</span></span> <span data-ttu-id="04727-185">Du behöver tooincrease `ResourceDisk.SwapSizeMB` i hello `/etc/waagent.conf` filen och starta om tjänsten för hello WALinuxAgent för hello uppdaterade inställningar tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="04727-185">You need tooincrease `ResourceDisk.SwapSizeMB` in hello `/etc/waagent.conf` file and restart hello WALinuxAgent service in order for hello updated settings tootake effect.</span></span> <span data-ttu-id="04727-186">Eftersom det är en skrivskyddad fil, måste toochange filen behörigheter tooenable skrivåtkomst.</span><span class="sxs-lookup"><span data-stu-id="04727-186">Because it is a read-only file, you need toochange file permissions tooenable write access.</span></span>

   ```bash
   sudo chmod 777 /etc/waagent.conf  
   vi /etc/waagent.conf
   ```
   
   <span data-ttu-id="04727-187">Sök efter `ResourceDisk.SwapSizeMB` och ändra hello värdet för**8192**.</span><span class="sxs-lookup"><span data-stu-id="04727-187">Search for `ResourceDisk.SwapSizeMB` and change hello value too**8192**.</span></span> <span data-ttu-id="04727-188">Du behöver toopress `insert` typ i hello värde i infogningsläge tooenter **8192** och tryck sedan på `esc` tooreturn toocommand läge.</span><span class="sxs-lookup"><span data-stu-id="04727-188">You will need toopress `insert` tooenter insert mode, type in hello value of **8192** and then press `esc` tooreturn toocommand mode.</span></span> <span data-ttu-id="04727-189">toowrite hello ändringar och avsluta hello filtyp `:wq` och tryck på `enter`.</span><span class="sxs-lookup"><span data-stu-id="04727-189">toowrite hello changes and quit hello file, type `:wq` and press `enter`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="04727-190">Vi rekommenderar starkt att du alltid använder `WALinuxAgent` tooconfigure växlingsutrymme så att den alltid har skapats på hello tillfälliga hårddisk (tillfällig disk) för bästa prestanda.</span><span class="sxs-lookup"><span data-stu-id="04727-190">We highly recommend that you always use `WALinuxAgent` tooconfigure swap space so that it's always created on hello local ephemeral disk (temporary disk) for best performance.</span></span> <span data-ttu-id="04727-191">Mer information om finns [hur tooadd en växling filen i virtuella Linux Azure-datorer](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="04727-191">For more information on, see [How tooadd a swap file in Linux Azure virtual machines](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines).</span></span>

## <a name="prepare-your-local-client-and-vm-toorun-x11"></a><span data-ttu-id="04727-192">Förbereda din lokala klient och VM-toorun x11</span><span class="sxs-lookup"><span data-stu-id="04727-192">Prepare your local client and VM toorun x11</span></span>
<span data-ttu-id="04727-193">Konfigurera Oracle ASM kräver ett grafiskt gränssnitt toocomplete hello installation och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="04727-193">Configuring Oracle ASM requires a graphical interface toocomplete hello install and configuration.</span></span> <span data-ttu-id="04727-194">Vi använder hello x11 protokollet toofacilitate installationen.</span><span class="sxs-lookup"><span data-stu-id="04727-194">We are using hello x11 protocol toofacilitate this installation.</span></span> <span data-ttu-id="04727-195">Om du använder ett klientsystem (Mac eller Linux) som redan har X11 funktioner aktiverat och konfigurerat – du kan hoppa över den här konfigurationen och konfigurera exklusiv tooWindows datorer.</span><span class="sxs-lookup"><span data-stu-id="04727-195">If you are using a client system (Mac or Linux) that already has X11 capabilities enabled and configured - you can skip this configuration and setup exclusive tooWindows machines.</span></span> 

1. <span data-ttu-id="04727-196">[Hämta PuTTY](http://www.putty.org/) och [hämta Xming](https://xming.en.softonic.com/) tooyour Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="04727-196">[Download PuTTY](http://www.putty.org/) and [download Xming](https://xming.en.softonic.com/) tooyour Windows computer.</span></span> <span data-ttu-id="04727-197">Du behöver toocomplete hello installation av båda dessa program med hello standardvärden innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="04727-197">You will need toocomplete hello installation of both of these applications with hello default values before proceeding.</span></span>

2. <span data-ttu-id="04727-198">När du har installerat PuTTY öppna Kommandotolken, ändra till hello PuTTY mappen (till exempel C:\Program Files\PuTTY) och kör `puttygen.exe` ordning toogenerate en nyckel.</span><span class="sxs-lookup"><span data-stu-id="04727-198">After you install PuTTY, open a command prompt, change into hello PuTTY folder (for example, C:\Program Files\PuTTY), and run `puttygen.exe` in order toogenerate a key.</span></span>

3. <span data-ttu-id="04727-199">I PuTTY Nyckelgenerator:</span><span class="sxs-lookup"><span data-stu-id="04727-199">In PuTTY Key Generator:</span></span>
   
   1. <span data-ttu-id="04727-200">Generera en nyckel genom att välja hello `Generate` knappen.</span><span class="sxs-lookup"><span data-stu-id="04727-200">Generate a key by selecting hello `Generate` button.</span></span>
   2. <span data-ttu-id="04727-201">Kopiera hello innehållet på hello nyckel (Ctrl + C).</span><span class="sxs-lookup"><span data-stu-id="04727-201">Copy hello contents of hello key (Ctrl+C).</span></span>
   3. <span data-ttu-id="04727-202">Välj hello `Save private key` knappen.</span><span class="sxs-lookup"><span data-stu-id="04727-202">Select hello `Save private key` button.</span></span>
   4. <span data-ttu-id="04727-203">Ignorera hello varningen om att skydda hello nyckel med en lösenfras och välj sedan `OK`.</span><span class="sxs-lookup"><span data-stu-id="04727-203">Ignore hello warning about securing hello key with a passphrase, and then select `OK`.</span></span>

   ![Skärmbild av PuTTY Nyckelgenerator](./media/oracle-asm/puttykeygen.png)

4. <span data-ttu-id="04727-205">I den virtuella datorn köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="04727-205">In your VM, run these commands:</span></span>

   ```bash
   sudo su - grid
   mkdir .ssh 
   cd .ssh
   ```

5. <span data-ttu-id="04727-206">Skapa en fil med namnet `authorized_keys`.</span><span class="sxs-lookup"><span data-stu-id="04727-206">Create a file named `authorized_keys`.</span></span> <span data-ttu-id="04727-207">Klistra in hello innehållet i hello nyckel i filen och spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="04727-207">Paste hello contents of hello key in this file, and then save hello file.</span></span>

   > [!NOTE]
   > <span data-ttu-id="04727-208">hello nyckel måste innehålla hello sträng `ssh-rsa`.</span><span class="sxs-lookup"><span data-stu-id="04727-208">hello key must contain hello string `ssh-rsa`.</span></span> <span data-ttu-id="04727-209">Hello innehållet i hello nyckel måste också vara en textrad.</span><span class="sxs-lookup"><span data-stu-id="04727-209">Also, hello contents of hello key must be a single line of text.</span></span>
   >  

6. <span data-ttu-id="04727-210">Starta PuTTY på din klientsystemet.</span><span class="sxs-lookup"><span data-stu-id="04727-210">On your client system, start PuTTY.</span></span> <span data-ttu-id="04727-211">I hello **kategori** rutan Gå för**anslutning** > **SSH** > **Auth**. I hello **fil för privat nyckel för autentisering** rutan, bläddra toohello nyckel som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="04727-211">In hello **Category** pane, go too**Connection** > **SSH** > **Auth**. In hello **Private key file for authentication** box, browse toohello key that you generated earlier.</span></span>

   ![Skärmbild av alternativ för hello SSH-autentisering](./media/oracle-asm/setprivatekey.png)

7. <span data-ttu-id="04727-213">I hello **kategori** rutan Gå för**anslutning** > **SSH** > **X11**.</span><span class="sxs-lookup"><span data-stu-id="04727-213">In hello **Category** pane, go too**Connection** > **SSH** > **X11**.</span></span> <span data-ttu-id="04727-214">Välj hello **aktivera X11 vidarebefordran** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="04727-214">Select hello **Enable X11 forwarding** check box.</span></span>

   ![Skärmbild av hello SSH X11 vidarebefordran alternativ](./media/oracle-asm/enablex11.png)

8. <span data-ttu-id="04727-216">I hello **kategori** rutan Gå för**Session**.</span><span class="sxs-lookup"><span data-stu-id="04727-216">In hello **Category** pane, go too**Session**.</span></span> <span data-ttu-id="04727-217">Ange den virtuella datorn ASM Oracle `<publicIPaddress>` i dialogrutan hello värd namn, fyller du i en ny `Saved Session` namn och klicka sedan på `Save`.</span><span class="sxs-lookup"><span data-stu-id="04727-217">Enter your Oracle ASM VM `<publicIPaddress>` in hello host name dialog box, fill in a new `Saved Session` name and then click on `Save`.</span></span>  <span data-ttu-id="04727-218">När du sparat klickar du på `open` tooconnect tooyour Oracle ASM virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="04727-218">Once saved, click on `open` tooconnect tooyour Oracle ASM virtual machine.</span></span>  <span data-ttu-id="04727-219">hello första gången du ansluter du varnas hello fjärrsystemet inte cachelagras i registret.</span><span class="sxs-lookup"><span data-stu-id="04727-219">hello first time you connect you are warned  hello remote system is not cached in your registry.</span></span> <span data-ttu-id="04727-220">Klicka på `yes` tooadd det och fortsätta.</span><span class="sxs-lookup"><span data-stu-id="04727-220">Click on `yes` tooadd it and continue.</span></span>

   ![Skärmbild av hello PuTTY-sessionsalternativ](./media/oracle-asm/puttysession.png)

## <a name="install-oracle-grid-infrastructure"></a><span data-ttu-id="04727-222">Installera infrastrukturen för Oracle-rutnätet</span><span class="sxs-lookup"><span data-stu-id="04727-222">Install Oracle Grid Infrastructure</span></span>

<span data-ttu-id="04727-223">tooinstall Oracle rutnätet infrastruktur, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="04727-223">tooinstall Oracle Grid Infrastructure, complete hello following steps:</span></span>

1. <span data-ttu-id="04727-224">Logga in som **rutnätet**.</span><span class="sxs-lookup"><span data-stu-id="04727-224">Sign in as **grid**.</span></span> <span data-ttu-id="04727-225">(Du ska kunna toosign i utan att ange ett lösenord.)</span><span class="sxs-lookup"><span data-stu-id="04727-225">(You should be able toosign in without being prompted for a password.)</span></span> 

   > [!NOTE]
   > <span data-ttu-id="04727-226">Om du kör Windows Kontrollera att du har startat Xming innan du påbörjar installationen hello.</span><span class="sxs-lookup"><span data-stu-id="04727-226">If you are running Windows, make sure you have started Xming before you begin hello installation.</span></span>

   ```bash
   cd /opt/grid
   ./runInstaller
   ```

   <span data-ttu-id="04727-227">Oracle rutnätet infrastruktur 12c version 1 Installer öppnas.</span><span class="sxs-lookup"><span data-stu-id="04727-227">Oracle Grid Infrastructure 12c Release 1 Installer opens.</span></span> <span data-ttu-id="04727-228">(Det kan ta några minuter för hello installer toostart.)</span><span class="sxs-lookup"><span data-stu-id="04727-228">(It might take a few minutes for hello  installer toostart.)</span></span>

2. <span data-ttu-id="04727-229">På hello **väljer installationsalternativet** väljer **installera och konfigurera infrastrukturen för Oracle rutnät för en fristående Server**.</span><span class="sxs-lookup"><span data-stu-id="04727-229">On hello **Select Installation Option** page, select **Install and Configure Oracle Grid Infrastructure for a Standalone Server**.</span></span>

   ![Skärmbild av hello installer väljer installationsalternativet sida](./media/oracle-asm/install01.png)

3. <span data-ttu-id="04727-231">På hello **Välj språk för produkten** Kontrollera **engelska** eller hello språk har valts.</span><span class="sxs-lookup"><span data-stu-id="04727-231">On hello **Select Product Languages** page, ensure **English** or hello language that you want is selected.</span></span>  <span data-ttu-id="04727-232">Klicka på `next`.</span><span class="sxs-lookup"><span data-stu-id="04727-232">Click `next`.</span></span>

4. <span data-ttu-id="04727-233">På hello **skapa ASM diskgruppen** sidan:</span><span class="sxs-lookup"><span data-stu-id="04727-233">On hello **Create ASM Disk Group** page:</span></span>
   - <span data-ttu-id="04727-234">Ange ett namn för hello diskgruppen.</span><span class="sxs-lookup"><span data-stu-id="04727-234">Enter a name for hello disk group.</span></span>
   - <span data-ttu-id="04727-235">Under **redundans**väljer **externa**.</span><span class="sxs-lookup"><span data-stu-id="04727-235">Under **Redundancy**, select **External**.</span></span>
   - <span data-ttu-id="04727-236">Under **storlek på allokeringsenhet**väljer **4**.</span><span class="sxs-lookup"><span data-stu-id="04727-236">Under **Allocation Unit Size**, select **4**.</span></span>
   - <span data-ttu-id="04727-237">Under **Lägg till diskar**väljer **ORCLASMSP**.</span><span class="sxs-lookup"><span data-stu-id="04727-237">Under **Add Disks**, select **ORCLASMSP**.</span></span>
   - <span data-ttu-id="04727-238">Klicka på `next`.</span><span class="sxs-lookup"><span data-stu-id="04727-238">Click `next`.</span></span>

5. <span data-ttu-id="04727-239">På hello **ange ASM lösenord** sidan, Välj hello **använda samma lösenord för dessa konton** alternativet och ange ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="04727-239">On hello **Specify ASM Password** page, select hello **Use same passwords for these accounts** option, and enter a password.</span></span>

   ![Skärmbild av lösenordssidan för hello installer ange ASM](./media/oracle-asm/install04.png)

6. <span data-ttu-id="04727-241">På hello **ange alternativ för hantering av** kan du ha hello alternativet tooconfigure EM molnet kontroll.</span><span class="sxs-lookup"><span data-stu-id="04727-241">On hello **Specify Management Options** page, you have hello option tooconfigure EM Cloud Control.</span></span> <span data-ttu-id="04727-242">Vi vill hoppa över det här alternativet – Klicka på `next` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="04727-242">We are skipping this option - click `next` toocontinue.</span></span> 

7. <span data-ttu-id="04727-243">På hello **Privilegierade Operativsystemgrupper** använder hello standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="04727-243">On hello **Privileged Operating System Groups** page, use hello default settings.</span></span> <span data-ttu-id="04727-244">Klicka på `next` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="04727-244">Click `next` toocontinue.</span></span>

8. <span data-ttu-id="04727-245">På hello **ange installationsplatsen** använder hello standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="04727-245">On hello **Specify Installation Location** page, use hello default settings.</span></span> <span data-ttu-id="04727-246">Klicka på `next` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="04727-246">Click `next` toocontinue.</span></span>

9. <span data-ttu-id="04727-247">På hello **skapa** ändrar hello inventering Directory för`/u01/app/grid/oraInventory`.</span><span class="sxs-lookup"><span data-stu-id="04727-247">On hello **Create Inventory** page, change hello Inventory Directory too`/u01/app/grid/oraInventory`.</span></span> <span data-ttu-id="04727-248">Klicka på `next` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="04727-248">Click `next` toocontinue.</span></span>

   ![Skärmbild av hello installer Skapa sida](./media/oracle-asm/install08.png)

10. <span data-ttu-id="04727-250">På hello **rot skript körning configuration** sidan, Välj hello **kör automatiskt konfigurationsskript** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="04727-250">On hello **Root script execution configuration** page, select hello **Automatically run configuration scripts** check box.</span></span> <span data-ttu-id="04727-251">Markera hello **använder ”rot” användarens autentiseringsuppgifter** alternativet och ange hello rot användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="04727-251">Then, select hello **Use "root" user credential** option, and enter hello root user password.</span></span>

    ![Skärmbild av konfigurationssidan för hello installer rot skript för körning](./media/oracle-asm/install09.png)

11. <span data-ttu-id="04727-253">På hello **utföra nödvändiga kontrollerar** sidan hello nuvarande konfiguration kommer att misslyckas med fel.</span><span class="sxs-lookup"><span data-stu-id="04727-253">On hello **Perform Prerequisite Checks** page, hello current setup will fail with errors.</span></span> <span data-ttu-id="04727-254">Det här är ett förväntat beteende.</span><span class="sxs-lookup"><span data-stu-id="04727-254">This is an expected behavior.</span></span> <span data-ttu-id="04727-255">Välj `Fix & Check Again`.</span><span class="sxs-lookup"><span data-stu-id="04727-255">Select `Fix & Check Again`.</span></span>

12. <span data-ttu-id="04727-256">I hello **korrigering skriptet** dialogrutan klickar du på `OK`.</span><span class="sxs-lookup"><span data-stu-id="04727-256">In hello **Fixup Script** dialog box, click `OK`.</span></span>

13. <span data-ttu-id="04727-257">På hello **sammanfattning** sidan Granska de angivna inställningarna och klicka sedan på `Install`.</span><span class="sxs-lookup"><span data-stu-id="04727-257">On hello **Summary** page, review your selected settings, and then click `Install`.</span></span>

    ![Skärmbild av hello installer sammanfattningssida](./media/oracle-asm/install12.png)

14. <span data-ttu-id="04727-259">Ett varningsmeddelande visas om du konfigurationsskript måste toobe körs som en Privilegierade användare.</span><span class="sxs-lookup"><span data-stu-id="04727-259">A warning dialog box appears informing you configuration scripts need toobe run as a privileged user.</span></span> <span data-ttu-id="04727-260">Klicka på `Yes` toocontinue.</span><span class="sxs-lookup"><span data-stu-id="04727-260">Click `Yes` toocontinue.</span></span>

15. <span data-ttu-id="04727-261">På hello **Slutför** klickar du på `Close` toofinish hello installation.</span><span class="sxs-lookup"><span data-stu-id="04727-261">On hello **Finish** page, click `Close` toofinish hello installation.</span></span>

## <a name="set-up-your-oracle-asm-installation"></a><span data-ttu-id="04727-262">Ställ in Oracle ASM-installation</span><span class="sxs-lookup"><span data-stu-id="04727-262">Set up your Oracle ASM installation</span></span>

<span data-ttu-id="04727-263">tooset in Oracle ASM-installation, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="04727-263">tooset up your Oracle ASM installation, complete hello following steps:</span></span>

1. <span data-ttu-id="04727-264">Se till att du fortfarande är inloggad som **rutnätet**, från din X11 session.</span><span class="sxs-lookup"><span data-stu-id="04727-264">Ensure you are still signed in as **grid**, from your X11 session.</span></span> <span data-ttu-id="04727-265">Du kan behöva toohit `enter` toorevive hello terminal.</span><span class="sxs-lookup"><span data-stu-id="04727-265">You might need toohit `enter` toorevive hello terminal.</span></span> <span data-ttu-id="04727-266">Starta hello Oracle Automated Storage Management Configuration Assistant:</span><span class="sxs-lookup"><span data-stu-id="04727-266">Then launch hello Oracle Automated Storage Management Configuration Assistant:</span></span>

   ```bash
   cd /u01/app/grid/product/12.1.0/grid/bin
   ./asmca
   ```

   <span data-ttu-id="04727-267">Oracle ASM Configuration Assistant öppnas.</span><span class="sxs-lookup"><span data-stu-id="04727-267">Oracle ASM Configuration Assistant opens.</span></span>

2. <span data-ttu-id="04727-268">I hello **konfigurera ASM: diskgrupper** dialogrutan klickar du på hello `Create` knappen och klicka sedan på `Show Advanced Options`.</span><span class="sxs-lookup"><span data-stu-id="04727-268">In hello **Configure ASM: Disk Groups** dialog box, click hello `Create` button, and then click `Show Advanced Options`.</span></span>

3. <span data-ttu-id="04727-269">I hello **skapa diskgruppen** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="04727-269">In hello **Create Disk Group** dialog box:</span></span>

   - <span data-ttu-id="04727-270">Ange gruppnamn för hello disk **DATA**.</span><span class="sxs-lookup"><span data-stu-id="04727-270">Enter hello disk group name **DATA**.</span></span>
   - <span data-ttu-id="04727-271">Under **Välj medlem diskar**väljer **ORCL_DATA** och **ORCL_DATA1**.</span><span class="sxs-lookup"><span data-stu-id="04727-271">Under **Select Member Disks**, select **ORCL_DATA** and **ORCL_DATA1**.</span></span>
   - <span data-ttu-id="04727-272">Under **storlek på allokeringsenhet**väljer **4**.</span><span class="sxs-lookup"><span data-stu-id="04727-272">Under **Allocation Unit Size**, select **4**.</span></span>
   - <span data-ttu-id="04727-273">Klicka på `ok` toocreate hello diskgruppen.</span><span class="sxs-lookup"><span data-stu-id="04727-273">Click `ok` toocreate hello disk group.</span></span>
   - <span data-ttu-id="04727-274">Klicka på `ok` tooclose hello bekräftelsefönstret.</span><span class="sxs-lookup"><span data-stu-id="04727-274">Click `ok` tooclose hello confirmation window.</span></span>

   ![Skärmbild av dialogrutan för hello skapa diskgruppen](./media/oracle-asm/asm02.png)

4. <span data-ttu-id="04727-276">I hello **konfigurera ASM: diskgrupper** dialogrutan klickar du på hello `Create` knappen och klicka sedan på `Show Advanced Options`.</span><span class="sxs-lookup"><span data-stu-id="04727-276">In hello **Configure ASM: Disk Groups** dialog box, click hello `Create` button, and then click `Show Advanced Options`.</span></span>

5. <span data-ttu-id="04727-277">I hello **skapa diskgruppen** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="04727-277">In hello **Create Disk Group** dialog box:</span></span>

   - <span data-ttu-id="04727-278">Ange gruppnamn för hello disk **FRA**.</span><span class="sxs-lookup"><span data-stu-id="04727-278">Enter hello disk group name **FRA**.</span></span>
   - <span data-ttu-id="04727-279">Under **redundans**väljer **externt (ingen)**.</span><span class="sxs-lookup"><span data-stu-id="04727-279">Under **Redundancy**, select **External (none)**.</span></span>
   - <span data-ttu-id="04727-280">Under **Välj medlem diskar**väljer **ORCL_FRA**.</span><span class="sxs-lookup"><span data-stu-id="04727-280">Under **Select Member Disks**, select **ORCL_FRA**.</span></span>
   - <span data-ttu-id="04727-281">Under **storlek på allokeringsenhet**väljer **4**.</span><span class="sxs-lookup"><span data-stu-id="04727-281">Under **Allocation Unit Size**, select **4**.</span></span>
   - <span data-ttu-id="04727-282">Klicka på `ok` toocreate hello diskgruppen.</span><span class="sxs-lookup"><span data-stu-id="04727-282">Click `ok` toocreate hello disk group.</span></span>
   - <span data-ttu-id="04727-283">Klicka på `ok` tooclose hello bekräftelsefönstret.</span><span class="sxs-lookup"><span data-stu-id="04727-283">Click `ok` tooclose hello confirmation window.</span></span>

   ![Skärmbild av dialogrutan för hello skapa diskgruppen](./media/oracle-asm/asm04.png)

6. <span data-ttu-id="04727-285">Välj **avsluta** tooclose ASM Configuration Installationsassistenten.</span><span class="sxs-lookup"><span data-stu-id="04727-285">Select **Exit** tooclose ASM Configuration Assistant.</span></span>

   ![Skärmbild av hello konfigurera ASM: Disk-grupper i dialogrutan Avsluta](./media/oracle-asm/asm05.png)

## <a name="create-hello-database"></a><span data-ttu-id="04727-287">Skapa hello-databas</span><span class="sxs-lookup"><span data-stu-id="04727-287">Create hello database</span></span>

<span data-ttu-id="04727-288">hello Oracle-databas som är redan installerad på hello Azure Marketplace-avbildning.</span><span class="sxs-lookup"><span data-stu-id="04727-288">hello Oracle database software is already installed on hello Azure Marketplace image.</span></span> <span data-ttu-id="04727-289">toocreate en databas, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="04727-289">toocreate a database, complete hello following steps:</span></span>

1. <span data-ttu-id="04727-290">Växla användare toohello Oracle superanvändare och sedan initiera hello-lyssnaren för loggning:</span><span class="sxs-lookup"><span data-stu-id="04727-290">Switch users toohello Oracle superuser, and then initialize hello listener for logging:</span></span>

   ```bash
   su - oracle
   cd /u01/app/oracle/product/12.1.0/dbhome_1/bin
   ./dbca
   ```
   <span data-ttu-id="04727-291">Databasen Configuration Assistant öppnas.</span><span class="sxs-lookup"><span data-stu-id="04727-291">Database Configuration Assistant opens.</span></span>

2. <span data-ttu-id="04727-292">På hello **Databasåtgärden** klickar du på `Create Database`.</span><span class="sxs-lookup"><span data-stu-id="04727-292">On hello **Database Operation** page, click `Create Database`.</span></span>

3. <span data-ttu-id="04727-293">På hello **läget Skapa** sidan:</span><span class="sxs-lookup"><span data-stu-id="04727-293">On hello **Creation Mode** page:</span></span>

   - <span data-ttu-id="04727-294">Ange ett namn för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="04727-294">Enter a name for hello database.</span></span>
   - <span data-ttu-id="04727-295">För **lagringstyp**, se till att **automatisk hantering av lagring (ASM)** är markerad.</span><span class="sxs-lookup"><span data-stu-id="04727-295">For **Storage Type**, ensure **Automatic Storage Management (ASM)** is selected.</span></span>
   - <span data-ttu-id="04727-296">För **filer databasplatsen**, Använd hello standard ASM förslag på plats.</span><span class="sxs-lookup"><span data-stu-id="04727-296">For **Database Files Location**, use hello default ASM suggested location.</span></span>
   - <span data-ttu-id="04727-297">För **snabb återställning området**, Använd hello standard ASM förslag på plats.</span><span class="sxs-lookup"><span data-stu-id="04727-297">For **Fast Recovery Area**, use hello default ASM suggested location.</span></span>
   - <span data-ttu-id="04727-298">Ange en **administratörslösenord** och **Bekräfta lösenord**.</span><span class="sxs-lookup"><span data-stu-id="04727-298">type in an **Administrative Password** and **confirm password**.</span></span>
   - <span data-ttu-id="04727-299">Se till att `create as container database` är markerad.</span><span class="sxs-lookup"><span data-stu-id="04727-299">ensure `create as container database` is selected.</span></span>
   - <span data-ttu-id="04727-300">Ange en `pluggable database name` värde.</span><span class="sxs-lookup"><span data-stu-id="04727-300">type in a `pluggable database name` value.</span></span>

4. <span data-ttu-id="04727-301">På hello **sammanfattning** sidan Granska de angivna inställningarna och klicka sedan på `Finish` toocreate hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="04727-301">On hello **Summary** page, review your selected settings, and then click `Finish` toocreate hello database.</span></span>

   ![Skärmbild av hello sammanfattningssida](./media/oracle-asm/createdb03.png)

5. <span data-ttu-id="04727-303">hello databas har skapats.</span><span class="sxs-lookup"><span data-stu-id="04727-303">hello Database has been created.</span></span> <span data-ttu-id="04727-304">På hello **Slutför** , har hello alternativet toounlock ytterligare konton toouse databasen och ändra hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="04727-304">On hello **Finish** page, you have hello option toounlock additional accounts toouse this database and change hello passwords.</span></span> <span data-ttu-id="04727-305">Om du vill toodo så kan du välja **lösenordshantering** -Annars klickar du på `close`.</span><span class="sxs-lookup"><span data-stu-id="04727-305">If you wish toodo so, select **Password Management** - otherwise click on `close`.</span></span>

## <a name="delete-hello-vm"></a><span data-ttu-id="04727-306">Ta bort hello VM</span><span class="sxs-lookup"><span data-stu-id="04727-306">Delete hello VM</span></span>

<span data-ttu-id="04727-307">Du har konfigurerat Oracle Automated lagringshantering på hello Oracle DB-avbildning från hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="04727-307">You have successfully configured Oracle Automated Storage Management on hello Oracle DB image from hello Azure Marketplace.</span></span>  <span data-ttu-id="04727-308">När du inte längre behöver den här virtuella datorn kan du använda hello efter kommandot tooremove hello resursgrupp, VM och alla relaterade resurser:</span><span class="sxs-lookup"><span data-stu-id="04727-308">When you no longer need this VM, you can use hello following command tooremove hello resource group, VM, and all related resources:</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="04727-309">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="04727-309">Next steps</span></span>

[<span data-ttu-id="04727-310">Självstudier: Konfigurera Oracle DataGuard</span><span class="sxs-lookup"><span data-stu-id="04727-310">Tutorial: Configure Oracle DataGuard</span></span>](configure-oracle-dataguard.md)

[<span data-ttu-id="04727-311">Självstudier: Konfigurera Oracle GoldenGate</span><span class="sxs-lookup"><span data-stu-id="04727-311">Tutorial: Configure Oracle GoldenGate</span></span>](Configure-oracle-golden-gate.md)

<span data-ttu-id="04727-312">Granska [skapa en Oracle-databas](oracle-design.md)</span><span class="sxs-lookup"><span data-stu-id="04727-312">Review [Architect an Oracle DB](oracle-design.md)</span></span>
