---
title: "aaaImplement Oracle guld Gate på en Azure Linux-dator | Microsoft Docs"
description: "Snabbt en Oracle guld Gate upp och körs i Azure-miljön."
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
ms.date: 05/19/2017
ms.author: rclaus
ms.openlocfilehash: 320cafd5d23ee472f0af9f92577bc6f432f65778
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="implement-oracle-golden-gate-on-an-azure-linux-vm"></a><span data-ttu-id="ca6ee-103">Implementera Oracle guld Gate på en Azure Linux-dator</span><span class="sxs-lookup"><span data-stu-id="ca6ee-103">Implement Oracle Golden Gate on an Azure Linux VM</span></span> 

<span data-ttu-id="ca6ee-104">hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="ca6ee-105">Det här guiden beskriver hur toouse hello Azure CLI toodeploy en Oracle 12c-databas från hello Azure Marketplace-galleriet avbildning.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-105">This guide details how toouse hello Azure CLI toodeploy an Oracle 12c database from hello Azure Marketplace gallery image.</span></span> 

<span data-ttu-id="ca6ee-106">Det här dokumentet vägleder dig hur toocreate, installera och konfigurera Oracle guld Gate på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-106">This document shows you step-by-step how toocreate, install, and configure Oracle Golden Gate on an Azure VM.</span></span>

<span data-ttu-id="ca6ee-107">Innan du börjar bör du kontrollera om den hello Azure CLI har installerats.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-107">Before you start, make sure that hello Azure CLI has been installed.</span></span> <span data-ttu-id="ca6ee-108">Mer information finns i [installationsguiden för Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ca6ee-108">For more information, see [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-hello-environment"></a><span data-ttu-id="ca6ee-109">Förbered hello-miljön</span><span class="sxs-lookup"><span data-stu-id="ca6ee-109">Prepare hello environment</span></span>

<span data-ttu-id="ca6ee-110">tooperform hello Oracle guld Gate installationen måste toocreate två virtuella Azure-datorer på hello samma tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-110">tooperform hello Oracle Golden Gate installation, you need toocreate two Azure VMs on hello same availability set.</span></span> <span data-ttu-id="ca6ee-111">hello Marketplace-avbildning som du använder toocreate hello virtuella datorer är **Oracle: Oracle-databasen-Ee:12.1.0.2:latest**.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-111">hello Marketplace image you use toocreate hello VMs is **Oracle:Oracle-Database-Ee:12.1.0.2:latest**.</span></span>

<span data-ttu-id="ca6ee-112">Du behöver toobe bekant med Unix editor vi och har en grundläggande förståelse för x11 (Windows X).</span><span class="sxs-lookup"><span data-stu-id="ca6ee-112">You also need toobe familiar with Unix editor vi and have a basic understanding of x11 (X Windows).</span></span>

<span data-ttu-id="ca6ee-113">hello nedan följer en sammanfattning av konfigurationen för hello-miljö:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-113">hello following is a summary of hello environment configuration:</span></span>
> 
> |  | <span data-ttu-id="ca6ee-114">**Primär plats**</span><span class="sxs-lookup"><span data-stu-id="ca6ee-114">**Primary site**</span></span> | <span data-ttu-id="ca6ee-115">**Replikera plats**</span><span class="sxs-lookup"><span data-stu-id="ca6ee-115">**Replicate site**</span></span> |
> | --- | --- | --- |
> | <span data-ttu-id="ca6ee-116">**Oracle-versionen**</span><span class="sxs-lookup"><span data-stu-id="ca6ee-116">**Oracle release**</span></span> |<span data-ttu-id="ca6ee-117">Oracle 12c version 2 – (12.1.0.2)</span><span class="sxs-lookup"><span data-stu-id="ca6ee-117">Oracle 12c Release 2 – (12.1.0.2)</span></span> |<span data-ttu-id="ca6ee-118">Oracle 12c version 2 – (12.1.0.2)</span><span class="sxs-lookup"><span data-stu-id="ca6ee-118">Oracle 12c Release 2 – (12.1.0.2)</span></span>|
> | <span data-ttu-id="ca6ee-119">**Namnet på datorn**</span><span class="sxs-lookup"><span data-stu-id="ca6ee-119">**Machine name**</span></span> |<span data-ttu-id="ca6ee-120">myVM1</span><span class="sxs-lookup"><span data-stu-id="ca6ee-120">myVM1</span></span> |<span data-ttu-id="ca6ee-121">myVM2</span><span class="sxs-lookup"><span data-stu-id="ca6ee-121">myVM2</span></span> |
> | <span data-ttu-id="ca6ee-122">**Operativsystem**</span><span class="sxs-lookup"><span data-stu-id="ca6ee-122">**Operating system**</span></span> |<span data-ttu-id="ca6ee-123">Oracle Linux 6.x</span><span class="sxs-lookup"><span data-stu-id="ca6ee-123">Oracle Linux 6.x</span></span> |<span data-ttu-id="ca6ee-124">Oracle Linux 6.x</span><span class="sxs-lookup"><span data-stu-id="ca6ee-124">Oracle Linux 6.x</span></span> |
> | <span data-ttu-id="ca6ee-125">**Oracle SID**</span><span class="sxs-lookup"><span data-stu-id="ca6ee-125">**Oracle SID**</span></span> |<span data-ttu-id="ca6ee-126">CDB1</span><span class="sxs-lookup"><span data-stu-id="ca6ee-126">CDB1</span></span> |<span data-ttu-id="ca6ee-127">CDB1</span><span class="sxs-lookup"><span data-stu-id="ca6ee-127">CDB1</span></span> |
> | <span data-ttu-id="ca6ee-128">**Schemat för replikering**</span><span class="sxs-lookup"><span data-stu-id="ca6ee-128">**Replication schema**</span></span> |<span data-ttu-id="ca6ee-129">TEST</span><span class="sxs-lookup"><span data-stu-id="ca6ee-129">TEST</span></span>|<span data-ttu-id="ca6ee-130">TEST</span><span class="sxs-lookup"><span data-stu-id="ca6ee-130">TEST</span></span> |
> | <span data-ttu-id="ca6ee-131">**Guld Gate ägare/replikera**</span><span class="sxs-lookup"><span data-stu-id="ca6ee-131">**Golden Gate owner/replicate**</span></span> |<span data-ttu-id="ca6ee-132">C ##GGADMIN</span><span class="sxs-lookup"><span data-stu-id="ca6ee-132">C##GGADMIN</span></span> |<span data-ttu-id="ca6ee-133">REPUSER</span><span class="sxs-lookup"><span data-stu-id="ca6ee-133">REPUSER</span></span> |
> | <span data-ttu-id="ca6ee-134">**Guld Gate-processen**</span><span class="sxs-lookup"><span data-stu-id="ca6ee-134">**Golden Gate process**</span></span> |<span data-ttu-id="ca6ee-135">EXTORA</span><span class="sxs-lookup"><span data-stu-id="ca6ee-135">EXTORA</span></span> |<span data-ttu-id="ca6ee-136">REPORA</span><span class="sxs-lookup"><span data-stu-id="ca6ee-136">REPORA</span></span>|


### <a name="sign-in-tooazure"></a><span data-ttu-id="ca6ee-137">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="ca6ee-137">Sign in tooAzure</span></span> 

<span data-ttu-id="ca6ee-138">Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-138">Sign in tooyour Azure subscription with hello [az login](/cli/azure/#login) command.</span></span> <span data-ttu-id="ca6ee-139">Följ hello på skärmen anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-139">Then follow hello on-screen directions.</span></span>

```azurecli
az login
```

### <a name="create-a-resource-group"></a><span data-ttu-id="ca6ee-140">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="ca6ee-140">Create a resource group</span></span>

<span data-ttu-id="ca6ee-141">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-141">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="ca6ee-142">En Azure-resursgrupp är en logisk behållare i vilka Azure-resurser har distribuerats och från vilken de kan hanteras.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-142">An Azure resource group is a logical container into which Azure resources are deployed and from which they can be managed.</span></span> 

<span data-ttu-id="ca6ee-143">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westus` plats.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-143">hello following example creates a resource group named `myResourceGroup` in hello `westus` location.</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a><span data-ttu-id="ca6ee-144">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="ca6ee-144">Create an availability set</span></span>

<span data-ttu-id="ca6ee-145">hello följande steg är valfritt men rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-145">hello following step is optional but recommended.</span></span> <span data-ttu-id="ca6ee-146">Mer information finns i [Azure tillgänglighetsuppsättningar guiden](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span><span class="sxs-lookup"><span data-stu-id="ca6ee-146">For more information, see [Azure availability sets guide](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a><span data-ttu-id="ca6ee-147">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ca6ee-147">Create a virtual machine</span></span>

<span data-ttu-id="ca6ee-148">Skapa en virtuell dator med hello [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-148">Create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="ca6ee-149">hello följande exempel skapar två virtuella datorer med namnet `myVM1` och `myVM2`.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-149">hello following example creates two VMs named `myVM1` and `myVM2`.</span></span> <span data-ttu-id="ca6ee-150">Skapa SSH-nycklar om de inte redan finns på standardplatsen nyckel.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-150">Create SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="ca6ee-151">toouse för en specifik uppsättning nycklar använder hello `--ssh-key-value` alternativet.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-151">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>

#### <a name="create-myvm1-primary"></a><span data-ttu-id="ca6ee-152">Skapa myVM1 (primära):</span><span class="sxs-lookup"><span data-stu-id="ca6ee-152">Create myVM1 (primary):</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

<span data-ttu-id="ca6ee-153">Efter hello VM har skapats, visar hello Azure CLI information liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-153">After hello VM has been created, hello Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="ca6ee-154">(Anteckna hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-154">(Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="ca6ee-155">Den här adressen är används tooaccess hello VM.)</span><span class="sxs-lookup"><span data-stu-id="ca6ee-155">This address is used tooaccess hello VM.)</span></span>

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

#### <a name="create-myvm2-replicate"></a><span data-ttu-id="ca6ee-156">Skapa myVM2 (replikera):</span><span class="sxs-lookup"><span data-stu-id="ca6ee-156">Create myVM2 (replicate):</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

<span data-ttu-id="ca6ee-157">Anteckna hello `publicIpAddress` samt när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-157">Take note of hello `publicIpAddress` as well after it has been created.</span></span>

### <a name="open-hello-tcp-port-for-connectivity"></a><span data-ttu-id="ca6ee-158">Öppna hello TCP-port för anslutning</span><span class="sxs-lookup"><span data-stu-id="ca6ee-158">Open hello TCP port for connectivity</span></span>

<span data-ttu-id="ca6ee-159">hello nästa steg är tooconfigure externa slutpunkter, som gör att du tooaccess hello Oracle-databas från en fjärrdator.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-159">hello next step is tooconfigure external endpoints,  which enable you tooaccess hello Oracle database remotely.</span></span> <span data-ttu-id="ca6ee-160">tooconfigure hello externa slutpunkter genom att köra följande kommandon hello.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-160">tooconfigure hello external endpoints, run hello following commands.</span></span>

#### <a name="open-hello-port-for-myvm1"></a><span data-ttu-id="ca6ee-161">Öppna hello port för myVM1:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-161">Open hello port for myVM1:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

<span data-ttu-id="ca6ee-162">hello resultat bör se ut ungefär toohello efter svar:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-162">hello results should look similar toohello following response:</span></span>

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

#### <a name="open-hello-port-for-myvm2"></a><span data-ttu-id="ca6ee-163">Öppna hello port för myVM2:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-163">Open hello port for myVM2:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-toohello-virtual-machine"></a><span data-ttu-id="ca6ee-164">Ansluta toohello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ca6ee-164">Connect toohello virtual machine</span></span>

<span data-ttu-id="ca6ee-165">Använd hello följande kommando toocreate en SSH-session med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-165">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="ca6ee-166">Ersätt hello IP-adress med hello `publicIpAddress` i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-166">Replace hello IP address with hello `publicIpAddress` of your virtual machine.</span></span>

```bash 
ssh <publicIpAddress>
```

### <a name="create-hello-database-on-myvm1-primary"></a><span data-ttu-id="ca6ee-167">Skapa hello-databas på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="ca6ee-167">Create hello database on myVM1 (primary)</span></span>

<span data-ttu-id="ca6ee-168">hello Oracle-programvara är installerad på hello Marketplace-avbildning, så hello nästa steg är tooinstall hello-databas.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-168">hello Oracle software is already installed on hello Marketplace image, so hello next step is tooinstall hello database.</span></span> 

<span data-ttu-id="ca6ee-169">Kör hello programvara som hello 'oracle-superanvändare:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-169">Run hello software as hello 'oracle' superuser:</span></span>

```bash
sudo su - oracle
```

<span data-ttu-id="ca6ee-170">Skapa databas för hello:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-170">Create hello database:</span></span>

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
<span data-ttu-id="ca6ee-171">Utdata ska se ut ungefär toohello efter svar:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-171">Outputs should look similar toohello following response:</span></span>

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
Look at hello log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for more details.
```

<span data-ttu-id="ca6ee-172">Ange hello ORACLE_SID och ORACLE_HOME variabler.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-172">Set hello ORACLE_SID and ORACLE_HOME variables.</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=gg1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

<span data-ttu-id="ca6ee-173">Alternativt kan du lägga till ORACLE_HOME och ORACLE_SID toohello .bashrc filen, så att de här inställningarna sparas för framtida inloggningar:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-173">Optionally, you can add ORACLE_HOME and ORACLE_SID toohello .bashrc file, so that these settings are saved for future sign-ins:</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=gg1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a><span data-ttu-id="ca6ee-174">Starta Oracle-lyssnare</span><span class="sxs-lookup"><span data-stu-id="ca6ee-174">Start Oracle listener</span></span>
```bash
$ sudo su - oracle
$ lsnrctl start
```

### <a name="create-hello-database-on-myvm2-replicate"></a><span data-ttu-id="ca6ee-175">Skapa hello-databas på myVM2 (replikera)</span><span class="sxs-lookup"><span data-stu-id="ca6ee-175">Create hello database on myVM2 (replicate)</span></span>

```bash
sudo su - oracle
```
<span data-ttu-id="ca6ee-176">Skapa databas för hello:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-176">Create hello database:</span></span>

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
<span data-ttu-id="ca6ee-177">Ange hello ORACLE_SID och ORACLE_HOME variabler.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-177">Set hello ORACLE_SID and ORACLE_HOME variables.</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

<span data-ttu-id="ca6ee-178">Du kan eventuellt ORACLE_HOME och ORACLE_SID toohello .bashrc filen som läggs till, så att de här inställningarna sparas för framtida inloggningar.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-178">Optionally, you can added ORACLE_HOME and ORACLE_SID toohello .bashrc file, so that these settings are saved for future sign-ins.</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a><span data-ttu-id="ca6ee-179">Starta Oracle-lyssnare</span><span class="sxs-lookup"><span data-stu-id="ca6ee-179">Start Oracle listener</span></span>
```bash
$ sudo su - oracle
$ lsnrctl start
```

## <a name="configure-golden-gate"></a><span data-ttu-id="ca6ee-180">Konfigurera guld Gate</span><span class="sxs-lookup"><span data-stu-id="ca6ee-180">Configure Golden Gate</span></span> 
<span data-ttu-id="ca6ee-181">tooconfigure guld Gate ta hello stegen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-181">tooconfigure Golden Gate, take hello steps in this section.</span></span>

### <a name="enable-archive-log-mode-on-myvm1-primary"></a><span data-ttu-id="ca6ee-182">Aktivera archive log-läge på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="ca6ee-182">Enable archive log mode on myVM1 (primary)</span></span>

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
<span data-ttu-id="ca6ee-183">Aktivera Tvingad loggning och kontrollera att det finns minst en loggfil.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-183">Enable force logging, and make sure at least one log file is present.</span></span>

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
SQL> ALTER SYSTEM set enable_goldengate_replication=true;
SQL> ALTER PLUGGABLE DATABASE PDB1 OPEN;
SQL> ALTER SESSION SET CONTAINER=PDB1;
SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
SQL> EXIT;
```

### <a name="download-golden-gate-software"></a><span data-ttu-id="ca6ee-184">Hämta guld Gate-programvara</span><span class="sxs-lookup"><span data-stu-id="ca6ee-184">Download Golden Gate software</span></span>
<span data-ttu-id="ca6ee-185">toodownload och förbereda hello Oracle guld Gate programvara, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-185">toodownload and prepare hello Oracle Golden Gate software, complete hello following steps:</span></span>

1. <span data-ttu-id="ca6ee-186">Hämta hello **fbo_ggs_Linux_x64_shiphome.zip** filen från hello [hämtningssidan för Oracle guld Gate](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="ca6ee-186">Download hello **fbo_ggs_Linux_x64_shiphome.zip** file from hello [Oracle Golden Gate download page](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html).</span></span> <span data-ttu-id="ca6ee-187">Hämta rubrik under hello **Oracle GoldenGate 12.x.x.x för Oracle Linux x86-64**, bör det finnas en uppsättning toodownload för ZIP-filer.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-187">Under hello download title **Oracle GoldenGate 12.x.x.x for Oracle Linux x86-64**, there should be a set of .zip files toodownload.</span></span>

2. <span data-ttu-id="ca6ee-188">När du har hämtat hello .zip filer tooyour klientdatorn Använd Secure kopiera Protocol (SCP) toocopy hello filer tooyour VM:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-188">After you download hello .zip files tooyour client computer, use Secure Copy Protocol (SCP) toocopy hello files tooyour VM:</span></span>

  ```bash
  $ scp fbo_ggs_Linux_x64_shiphome.zip <publicIpAddress>:<folder>
  ```

3. <span data-ttu-id="ca6ee-189">Flytta hello .zip filer toohello **/ opt** mapp.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-189">Move hello .zip files toohello **/opt** folder.</span></span> <span data-ttu-id="ca6ee-190">Ändra hello ägare av hello filer på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-190">Then change hello owner of hello files as follows:</span></span>

  ```bash
  $ sudo su -
  # mv <folder>/*.zip /opt
  ```

4. <span data-ttu-id="ca6ee-191">Packa upp hello-filer (installera hello Linux packa verktyget om den inte redan är installerat):</span><span class="sxs-lookup"><span data-stu-id="ca6ee-191">Unzip hello files (install hello Linux unzip utility if it's not already installed):</span></span>

  ```bash
  # yum install unzip
  # cd /opt
  # unzip fbo_ggs_Linux_x64_shiphome.zip
  ```

5. <span data-ttu-id="ca6ee-192">Ändra behörigheter:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-192">Change permission:</span></span>

  ```bash
  # chown -R oracle:oinstall /opt/fbo_ggs_Linux_x64_shiphome
  ```

### <a name="prepare-hello-client-and-vm-toorun-x11-for-windows-clients-only"></a><span data-ttu-id="ca6ee-193">Förbereda hello klienten och VM-toorun x11 (för Windows-klienter)</span><span class="sxs-lookup"><span data-stu-id="ca6ee-193">Prepare hello client and VM toorun x11 (for Windows clients only)</span></span>
<span data-ttu-id="ca6ee-194">Det här är ett valfritt steg.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-194">This is an optional step.</span></span> <span data-ttu-id="ca6ee-195">Du kan hoppa över det här steget om du använder en Linux-klient eller redan har x11 installationen.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-195">You can skip this step if you are using a Linux client or already have x11 setup.</span></span>

1. <span data-ttu-id="ca6ee-196">Hämta PuTTY och Xming tooyour Windows-dator:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-196">Download PuTTY and Xming tooyour Windows computer:</span></span>

  * [<span data-ttu-id="ca6ee-197">Hämta PuTTY</span><span class="sxs-lookup"><span data-stu-id="ca6ee-197">Download PuTTY</span></span>](http://www.putty.org/)
  * [<span data-ttu-id="ca6ee-198">Hämta Xming</span><span class="sxs-lookup"><span data-stu-id="ca6ee-198">Download Xming</span></span>](https://xming.en.softonic.com/)

2.  <span data-ttu-id="ca6ee-199">När du har installerat PuTTY i hello PuTTY mappen (till exempel C:\Program Files\PuTTY) genom att köra puttygen.exe (PuTTY nyckel Generator).</span><span class="sxs-lookup"><span data-stu-id="ca6ee-199">After you install PuTTY, in hello PuTTY folder (for example, C:\Program Files\PuTTY), run puttygen.exe (PuTTY Key Generator).</span></span>

3.  <span data-ttu-id="ca6ee-200">I PuTTY Nyckelgenerator:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-200">In PuTTY Key Generator:</span></span>

  - <span data-ttu-id="ca6ee-201">toogenerate en nyckel, Välj hello **generera** knappen.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-201">toogenerate a key, select hello **Generate** button.</span></span>
  - <span data-ttu-id="ca6ee-202">Kopiera hello innehållet på hello nyckel (**Ctrl + C**).</span><span class="sxs-lookup"><span data-stu-id="ca6ee-202">Copy hello contents of hello key (**Ctrl+C**).</span></span>
  - <span data-ttu-id="ca6ee-203">Välj hello **Spara privat nyckel** knappen.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-203">Select hello **Save private key** button.</span></span>
  - <span data-ttu-id="ca6ee-204">Ignorera hello varningen som visas och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-204">Ignore hello warning that appears, and then select **OK**.</span></span>

    ![Skärmbild av hello PuTTY nyckelgenerator sidan](./media/oracle-golden-gate/puttykeygen.png)

4.  <span data-ttu-id="ca6ee-206">I den virtuella datorn köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-206">In your VM, run these commands:</span></span>

  ```bash
  # sudo su - oracle
  $ mkdir .ssh (if not already created)
  $ cd .ssh
  ```

5. <span data-ttu-id="ca6ee-207">Skapa en fil med namnet **authorized_keys**.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-207">Create a file named **authorized_keys**.</span></span> <span data-ttu-id="ca6ee-208">Klistra in hello innehållet i hello nyckel i filen och spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-208">Paste hello contents of hello key in this file, and then save hello file.</span></span>

  > [!NOTE]
  > <span data-ttu-id="ca6ee-209">hello nyckel måste innehålla hello sträng `ssh-rsa`.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-209">hello key must contain hello string `ssh-rsa`.</span></span> <span data-ttu-id="ca6ee-210">Hello innehållet i hello nyckel måste också vara en textrad.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-210">Also, hello contents of hello key must be a single line of text.</span></span>
  >  

6. <span data-ttu-id="ca6ee-211">Starta PuTTY.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-211">Start PuTTY.</span></span> <span data-ttu-id="ca6ee-212">I hello **kategori** väljer **anslutning** > **SSH** > **Auth**. I hello **fil för privat nyckel för autentisering** rutan, bläddra toohello nyckel som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-212">In hello **Category** pane, select **Connection** > **SSH** > **Auth**. In hello **Private key file for authentication** box, browse toohello key that you generated earlier.</span></span>

  ![Skärmbild av sidan för hello ange privat nyckel](./media/oracle-golden-gate/setprivatekey.png)

7. <span data-ttu-id="ca6ee-214">I hello **kategori** väljer **anslutning** > **SSH** > **X11**.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-214">In hello **Category** pane, select **Connection** > **SSH** > **X11**.</span></span> <span data-ttu-id="ca6ee-215">Välj hello **aktivera X11 vidarebefordran** rutan.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-215">Then select hello **Enable X11 forwarding** box.</span></span>

  ![Skärmbild av sidan för hello aktivera X11](./media/oracle-golden-gate/enablex11.png)

8. <span data-ttu-id="ca6ee-217">I hello **kategori** rutan Gå för**Session**.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-217">In hello **Category** pane, go too**Session**.</span></span> <span data-ttu-id="ca6ee-218">Ange information om hello värden och välj sedan **öppna**.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-218">Enter hello host information, and then select **Open**.</span></span>

  ![Skärmbild av hello-sida](./media/oracle-golden-gate/puttysession.png)

### <a name="install-golden-gate-software"></a><span data-ttu-id="ca6ee-220">Installera guld Gate-programvara</span><span class="sxs-lookup"><span data-stu-id="ca6ee-220">Install Golden Gate software</span></span>

<span data-ttu-id="ca6ee-221">tooinstall Oracle guld Gate, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-221">tooinstall Oracle Golden Gate, complete hello following steps:</span></span>

1. <span data-ttu-id="ca6ee-222">Logga in som oracle.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-222">Sign in as oracle.</span></span> <span data-ttu-id="ca6ee-223">(Du ska kunna toosign i utan att ange ett lösenord.) Se till att Xming körs innan du påbörjar installationen hello.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-223">(You should be able toosign in without being prompted for a password.) Make sure that Xming is running before you begin hello installation.</span></span>
 
  ```bash
  $ cd /opt/fbo_ggs_Linux_x64_shiphome/Disk1
  $ ./runInstaller
  ```
2. <span data-ttu-id="ca6ee-224">Välj 'Oracle GoldenGate för Oracle-databas 12c'.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-224">Select 'Oracle GoldenGate for Oracle Database 12c'.</span></span> <span data-ttu-id="ca6ee-225">Välj sedan **nästa** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-225">Then select **Next** toocontinue.</span></span>

  ![Skärmbild av hello installer Välj installationssidan](./media/oracle-golden-gate/golden_gate_install_01.png)

3. <span data-ttu-id="ca6ee-227">Ändra platsen för hello-programvara.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-227">Change hello software location.</span></span> <span data-ttu-id="ca6ee-228">Välj hello **Starthanteraren** och anger hello databasplatsen.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-228">Then select  hello **Start Manager** box and enter hello database location.</span></span> <span data-ttu-id="ca6ee-229">Välj **nästa** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-229">Select **Next** toocontinue.</span></span>

  ![Skärmbild av hello Välj installationssidan](./media/oracle-golden-gate/golden_gate_install_02.png)

4. <span data-ttu-id="ca6ee-231">Ändra hello inventering katalogen och välj sedan **nästa** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-231">Change hello inventory directory, and then select **Next** toocontinue.</span></span>

  ![Skärmbild av hello Välj installationssidan](./media/oracle-golden-gate/golden_gate_install_03.png)

5. <span data-ttu-id="ca6ee-233">På hello **sammanfattning** väljer **installera** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-233">On hello **Summary** screen, select **Install** toocontinue.</span></span>

  ![Skärmbild av hello installer Välj installationssidan](./media/oracle-golden-gate/golden_gate_install_04.png)

6. <span data-ttu-id="ca6ee-235">Du kanske ange toorun ett skript som 'rot'.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-235">You might be prompted toorun a script as 'root'.</span></span> <span data-ttu-id="ca6ee-236">I så fall, öppna en egen session ssh toohello VM, sudo tooroot och kör sedan hello skript.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-236">If so, open a separate session, ssh toohello VM, sudo tooroot, and then run hello script.</span></span> <span data-ttu-id="ca6ee-237">Välj **OK** fortsätta.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-237">Select **OK** continue.</span></span>

  ![Skärmbild av hello Välj installationssidan](./media/oracle-golden-gate/golden_gate_install_05.png)

7. <span data-ttu-id="ca6ee-239">När hello installationen är klar väljer du **Stäng** toocomplete hello processen.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-239">When hello installation has finished, select **Close** toocomplete hello process.</span></span>

  ![Skärmbild av hello Välj installationssidan](./media/oracle-golden-gate/golden_gate_install_06.png)

### <a name="set-up-service-on-myvm1-primary"></a><span data-ttu-id="ca6ee-241">Ställ in tjänsten på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="ca6ee-241">Set up service on myVM1 (primary)</span></span>

1. <span data-ttu-id="ca6ee-242">Skapa eller uppdatera hello tnsnames.ora fil:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-242">Create or update hello tnsnames.ora file:</span></span>

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. <span data-ttu-id="ca6ee-243">Skapa hello guld Gate ägar- och konton.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-243">Create hello Golden Gate owner and user accounts.</span></span>

  > [!NOTE]
  > <span data-ttu-id="ca6ee-244">hello ägare kontot måste ha C ## prefix.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-244">hello owner account must have C## prefix.</span></span>
  >

    ```bash
    $ sqlplus / as sysdba
    SQL> CREATE USER C##GGADMIN identified by ggadmin;
    SQL> EXEC dbms_goldengate_auth.grant_admin_privilege('C##GGADMIN',container=>'ALL');
    SQL> GRANT DBA tooC##GGADMIN container=all;
    SQL> connect C##GGADMIN/ggadmin
    SQL> ALTER SESSION SET CONTAINER=PDB1;
    SQL> EXIT;
    ```

3. <span data-ttu-id="ca6ee-245">Skapa hello guld Gate testanvändarkonto:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-245">Create hello Golden Gate test user account:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba tootest;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> @demo_ora_insert
  SQL> EXIT;
  ```

4. <span data-ttu-id="ca6ee-246">Konfigurera hello extrahera parameterfil.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-246">Configure hello extract parameter file.</span></span>

 <span data-ttu-id="ca6ee-247">Starta hello gyllene gate-kommandoradsgränssnittet (ggsci):</span><span class="sxs-lookup"><span data-stu-id="ca6ee-247">Start hello Golden gate command-line interface (ggsci):</span></span>

  ```bash
  $ sudo su - oracle
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> DBLOGIN USERID test@pdb1, PASSWORD test
  Successfully logged into database  pdb1
  GGSCI>  ADD SCHEMATRANDATA pdb1.test
  2017-05-23 15:44:25  INFO    OGG-01788  SCHEMATRANDATA has been added on schema test.
  2017-05-23 15:44:25  INFO    OGG-01976  SCHEMATRANDATA for scheduling columns has been added on schema test.

  GGSCI> EDIT PARAMS EXTORA
  ```
5. <span data-ttu-id="ca6ee-248">Lägg till hello följande toohello EXTRAHERA parametern (med hjälp av vi kommandon).</span><span class="sxs-lookup"><span data-stu-id="ca6ee-248">Add hello following toohello EXTRACT parameter file (by using vi commands).</span></span> <span data-ttu-id="ca6ee-249">Tryck på Esc-tangenten ': wq!'</span><span class="sxs-lookup"><span data-stu-id="ca6ee-249">Press Esc key, ':wq!'</span></span> <span data-ttu-id="ca6ee-250">toosave-fil.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-250">toosave file.</span></span> 

  ```bash
  EXTRACT EXTORA
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTRAIL ./dirdat/rt  
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT 
  LOGALLSUPCOLS
  UPDATERECORDFORMAT COMPACT
  TABLE pdb1.test.TCUSTMER;
  TABLE pdb1.test.TCUSTORD;
  ```
6. <span data-ttu-id="ca6ee-251">Registrera extrahera--integrerad extrahera:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-251">Register extract--integrated extract:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci

  GGSCI> dblogin userid C##GGADMIN, password ggadmin
  Successfully logged into database CDB$ROOT.

  GGSCI> REGISTER EXTRACT EXTORA DATABASE CONTAINER(pdb1)

  2017-05-23 15:58:34  INFO    OGG-02003  Extract EXTORA successfully registered with database at SCN 1821260.

  GGSCI> exit
  ```
7. <span data-ttu-id="ca6ee-252">Konfigurera extrahera kontrollpunkter och börja realtid extrahera:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-252">Set up extract checkpoints and start real-time extract:</span></span>

  ```bash
  $ ./ggsci
  GGSCI>  ADD EXTRACT EXTORA, INTEGRATED TRANLOG, BEGIN NOW
  EXTRACT (Integrated) added.

  GGSCI>  ADD RMTTRAIL ./dirdat/rt, EXTRACT EXTORA, MEGABYTES 10
  RMTTRAIL added.

  GGSCI>  START EXTRACT EXTORA

  Sending START request tooMANAGER ...
  EXTRACT EXTORA starting

  GGSCI > info all

  Program     Status      Group       Lag at Chkpt  Time Since Chkpt

  MANAGER     RUNNING
  EXTRACT     RUNNING     EXTORA      00:00:11      00:00:04
  ```
<span data-ttu-id="ca6ee-253">I det här steget kan hitta du hello startar Tillståndsändringsavisering som kommer att användas senare i ett annat avsnitt:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-253">In this step, you find hello starting SCN, which will be used later, in a different section:</span></span>

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> SELECT current_scn from v$database;
  CURRENT_SCN
  -----------
      1857887
  SQL> EXIT;
  ```

  ```bash
  $ ./ggsci
  GGSCI> EDIT PARAMS INITEXT
  ```

  ```bash
  EXTRACT INITEXT
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTASK REPLICAT, GROUP INITREP
  TABLE pdb1.test.*, SQLPREDICATE 'AS OF SCN 1857887'; 
  ```

  ```bash
  GGSCI> ADD EXTRACT INITEXT, SOURCEISTABLE
  ```

### <a name="set-up-service-on-myvm2-replicate"></a><span data-ttu-id="ca6ee-254">Ställ in tjänsten på myVM2 (replikera)</span><span class="sxs-lookup"><span data-stu-id="ca6ee-254">Set up service on myVM2 (replicate)</span></span>


1. <span data-ttu-id="ca6ee-255">Skapa eller uppdatera hello tnsnames.ora fil:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-255">Create or update hello tnsnames.ora file:</span></span>

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. <span data-ttu-id="ca6ee-256">Skapa ett replikera konto:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-256">Create a replicate account:</span></span>

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> create user repuser identified by rep_pass container=current;
  SQL> grant dba toorepuser;
  SQL> exec dbms_goldengate_auth.grant_admin_privilege('REPUSER',container=>'PDB1');
  SQL> connect repuser/rep_pass@pdb1 
  SQL> EXIT;
  ```

3. <span data-ttu-id="ca6ee-257">Skapa ett användarkonto för Guld Gate-test:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-257">Create a Golden Gate test user account:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba tootest;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> EXIT;
  ```

4. <span data-ttu-id="ca6ee-258">REPLICAT parametern tooreplicate filändringar:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-258">REPLICAT parameter file tooreplicate changes:</span></span> 

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS REPORA  
  ```
  <span data-ttu-id="ca6ee-259">Innehållet i filen som REPORA parameter:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-259">Content of REPORA parameter file:</span></span>

  ```bash
  REPLICAT REPORA
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/repora.dsc, PURGE, MEGABYTES 100
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT
  DBOPTIONS INTEGRATEDPARAMS(parallelism 6)
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;
  ```

5. <span data-ttu-id="ca6ee-260">Skapa en kontrollpunkt replicat:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-260">Set up a replicat checkpoint:</span></span>

  ```bash
  GGSCI> ADD REPLICAT REPORA, INTEGRATED, EXTTRAIL ./dirdat/rt
  GGSCI> EDIT PARAMS INITREP

  ```

  ```bash
  REPLICAT INITREP
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/tcustmer.dsc, APPEND
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;   
  ```

  ```bash
  GGSCI> ADD REPLICAT INITREP, SPECIALRUN
  ```

### <a name="set-up-hello-replication-myvm1-and-myvm2"></a><span data-ttu-id="ca6ee-261">Konfigurera hello replikering (myVM1 och myVM2)</span><span class="sxs-lookup"><span data-stu-id="ca6ee-261">Set up hello replication (myVM1 and myVM2)</span></span>

#### <a name="1-set-up-hello-replication-on-myvm2-replicate"></a><span data-ttu-id="ca6ee-262">1. Ställ in hello replikering på myVM2 (replikera)</span><span class="sxs-lookup"><span data-stu-id="ca6ee-262">1. Set up hello replication on myVM2 (replicate)</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS MGR
  ```
<span data-ttu-id="ca6ee-263">Uppdatera hello-filen med hello följande:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-263">Update hello file with hello following:</span></span>

  ```bash
  PORT 7809
  ACCESSRULE, PROG *, IPADDR *, ALLOW
  ```
<span data-ttu-id="ca6ee-264">Starta om hello Manager-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-264">Then restart hello Manager service:</span></span>

  ```bash
  GGSCI> STOP MGR
  GGSCI> START MGR
  GGSCI> EXIT
  ```

#### <a name="2-set-up-hello-replication-on-myvm1-primary"></a><span data-ttu-id="ca6ee-265">2. Ställ in hello replikering på myVM1 (primär)</span><span class="sxs-lookup"><span data-stu-id="ca6ee-265">2. Set up hello replication on myVM1 (primary)</span></span>

<span data-ttu-id="ca6ee-266">Starta hello första last och Sök efter fel:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-266">Start hello initial load and check for errors:</span></span>

```bash
$ cd /u01/app/oracle/product/12.1.0/oggcore_1
$ ./ggsci
GGSCI> START EXTRACT INITEXT
GGSCI> VIEW REPORT INITEXT
```
#### <a name="3-set-up-hello-replication-on-myvm2-replicate"></a><span data-ttu-id="ca6ee-267">3. Ställ in hello replikering på myVM2 (replikera)</span><span class="sxs-lookup"><span data-stu-id="ca6ee-267">3. Set up hello replication on myVM2 (replicate)</span></span>

<span data-ttu-id="ca6ee-268">Ändra hello Tillståndsändringsavisering tal med hello tal som du erhållit tidigare:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-268">Change hello SCN number with hello number you obtained before:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  START REPLICAT REPORA, AFTERCSN 1857887
  ```
<span data-ttu-id="ca6ee-269">hello replikering har startat och du kan testa den genom att lägga till nya poster tooTEST tabeller.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-269">hello replication has begun, and you can test it by inserting new records tooTEST tables.</span></span>


### <a name="view-job-status-and-troubleshooting"></a><span data-ttu-id="ca6ee-270">Visa jobbstatus och felsökning</span><span class="sxs-lookup"><span data-stu-id="ca6ee-270">View job status and troubleshooting</span></span>

#### <a name="view-reports"></a><span data-ttu-id="ca6ee-271">Visa rapporter</span><span class="sxs-lookup"><span data-stu-id="ca6ee-271">View reports</span></span>
<span data-ttu-id="ca6ee-272">tooview rapporter om myVM1, kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-272">tooview reports on myVM1, run hello following commands:</span></span>

  ```bash
  GGSCI> VIEW REPORT EXTORA 
  ```
 
<span data-ttu-id="ca6ee-273">tooview rapporter om myVM2, kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-273">tooview reports on myVM2, run hello following commands:</span></span>

  ```bash
  GGSCI> VIEW REPORT REPORA
  ```

#### <a name="view-status-and-history"></a><span data-ttu-id="ca6ee-274">Visa status och historik</span><span class="sxs-lookup"><span data-stu-id="ca6ee-274">View status and history</span></span>
<span data-ttu-id="ca6ee-275">tooview status och historik i myVM1, kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-275">tooview status and history on myVM1, run hello following commands:</span></span>

  ```bash
  GGSCI> dblogin userid c##ggadmin, password ggadmin 
  GGSCI> INFO EXTRACT EXTORA, DETAIL
  ```

<span data-ttu-id="ca6ee-276">tooview status och historik i myVM2, kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ca6ee-276">tooview status and history on myVM2, run hello following commands:</span></span>

  ```bash
  GGSCI> dblogin userid repuser@pdb1 password rep_pass 
  GGSCI> INFO REP REPORA, DETAIL
  ```
<span data-ttu-id="ca6ee-277">Detta avslutar hello installationen och konfigurationen av guld Gate på Oracle linux.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-277">This completes hello installation and configuration of Golden Gate on Oracle linux.</span></span>


## <a name="delete-hello-virtual-machine"></a><span data-ttu-id="ca6ee-278">Ta bort hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ca6ee-278">Delete hello virtual machine</span></span>

<span data-ttu-id="ca6ee-279">När den inte längre behövs, kan det vara hello följande kommando används tooremove hello resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="ca6ee-279">When it's no longer needed, hello following command can be used tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="ca6ee-280">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ca6ee-280">Next steps</span></span>

[<span data-ttu-id="ca6ee-281">Självstudie: Skapa virtuella datorer med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="ca6ee-281">Create highly available virtual machines tutorial</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="ca6ee-282">Utforska exempel på distribution av virtuella datorer med CLI</span><span class="sxs-lookup"><span data-stu-id="ca6ee-282">Explore VM deployment CLI samples</span></span>](../../linux/cli-samples.md)
