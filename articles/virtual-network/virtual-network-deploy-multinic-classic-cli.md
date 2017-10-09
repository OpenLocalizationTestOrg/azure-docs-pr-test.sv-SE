---
title: "aaaCreate en virtuell dator (klassisk) med flera nätverkskort – Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur toocreate en virtuell dator (klassisk) med flera nätverkskort med hello Azure-kommandoradsgränssnittet (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: b436e41e-866c-439f-a7c7-7b4b041725ef
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 181bfb28027caff33410ca94744e79206a2a0d0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics-using-hello-azure-cli-10"></a>Skapa en virtuell dator (klassisk) med flera nätverkskort med hello Azure CLI 1.0

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Du kan skapa virtuella datorer (VM) i Azure och bifoga flera nätverk gränssnitt (NIC) tooeach för dina virtuella datorer. Flera nätverkskort aktivera uppdelning av trafiktyper mellan nätverkskort. Till exempel anslutna en NIC kan kommunicera med hello Internet, medan en annan inte kommunicerar endast med interna resurser toohello Internet. hello möjlighet tooseparate nätverkstrafik på flera nätverkskort krävs för många virtuella nätverksenheter, till exempel leverans av program och optimering av WAN-lösningar.

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Lär dig hur tooperform dessa steg med hello [Resource Manager-distributionsmodellen](virtual-network-deploy-multinic-arm-cli.md).

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

hello så här använder du en resursgrupp med namnet *IaaSStory* för hello webbservrar och en resursgrupp med namnet *IaaSStory BackEnd* för hello DB-servrar.

## <a name="prerequisites"></a>Krav
Innan du kan skapa hello DB-servrar, behöver du toocreate hello *IaaSStory* resursgrupp med alla hello nödvändiga resurser för det här scenariot. toocreate dessa resurser, fullständig hello steg som följer. Skapa ett virtuellt nätverk genom att följa stegen hello i hello [skapa ett virtuellt nätverk](virtual-networks-create-vnet-classic-cli.md) artikel.

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-hello-back-end-vms"></a>Distribuera hello backend-virtuella datorer
hello beror backend-VMs på hello skapande av hello följande resurser:

* **Storage-konto för datadiskar**. För bättre prestanda använder hello datadiskar på hello databasservrar Solid-State-hårddisk (SSD)-teknik som kräver ett premiumlagringskonto. Se till att hello Azure-plats som du distribuerar toosupport premium-lagring.
* **Nätverkskort**. Varje virtuell dator har två nätverkskort, ett för åtkomst till databasen, och en för hantering.
* **Tillgänglighetsuppsättningen**. Alla databasservrar läggs tooa enda tillgänglighetsuppsättning, tooensure minst en av hello virtuella datorer är igång och körs under underhåll.

### <a name="step-1---start-your-script"></a>Steg 1 – starta skriptet
Du kan hämta hello fullständig bash-skript används [här](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Slutför hello följande steg toochange hello skriptet toowork i din miljö:

1. Ändra hello värdena för hello variabler nedan baserat på en befintlig resursgrupp distribuerade ovan i [krav](#Prerequisites).

    ```azurecli
    location="useast2"
    vnetName="WTestVNet"
    backendSubnetName="BackEnd"
    ```
2. Ändra hello värden hello variabler nedan baserat på hello värden som du vill toouse för backend-distribution.

    ```azurecli
    backendCSName="IaaSStory-Backend"
    prmStorageAccountName="iaasstoryprmstorage"
    image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
    avSetName="ASDB"
    vmSize="Standard_DS3"
    diskSize=127
    vmNamePrefix="DB"
    osDiskName="osdiskdb"
    dataDiskPrefix="db"
    dataDiskName="datadisk"
    ipAddressPrefix="192.168.2."
    username='adminuser'
    password='adminP@ssw0rd'
    numberOfVMs=2
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Steg 2 – skapa nödvändiga resurser för din virtuella datorer
1. Skapa en ny molntjänst för alla virtuella datorer i serverdelen. Meddelande hello användning av hello `$backendCSName` variabel för hello resursgruppens namn, och `$location` för hello Azure-region.

    ```azurecli
    azure service create --serviceName $backendCSName \
        --location $location
    ```

2. Skapa ett premiumlagringskonto för hello OS och data diskar toobe används av dina virtuella datorer.

    ```azurecli
    azure storage account create $prmStorageAccountName \
        --location $location \
        --type PLRS
    ```

### <a name="step-3---create-vms-with-multiple-nics"></a>Steg 3 – skapa virtuella datorer med flera nätverkskort
1. Starta en loop toocreate flera virtuella datorer, baserat på hello `numberOfVMs` variabler.

    ```azurecli
    for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
    do
    ```

2. Ange hello namn och IP-adressen för varje hello två nätverkskort för varje virtuell dator.

    ```azurecli
    nic1Name=$vmNamePrefix$suffixNumber-DA
    x=$((suffixNumber+3))
    ipAddress1=$ipAddressPrefix$x

    nic2Name=$vmNamePrefix$suffixNumber-RA
    x=$((suffixNumber+53))
    ipAddress2=$ipAddressPrefix$x
    ```

3. Skapa hello VM. Lägg märke till hello användning av hello `--nic-config` parametern, som innehåller en lista över alla nätverkskort med namnet, undernät och IP-adress.

    ```azurecli
    azure vm create $backendCSName $image $username $password \
        --connect $backendCSName \
        --vm-name $vmNamePrefix$suffixNumber \
        --vm-size $vmSize \
        --availability-set $avSetName \
        --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
        --virtual-network-name $vnetName \
        --subnet-names $backendSubnetName \
        --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::
    ```

4. Skapa två hårddiskar för varje virtuell dator.

    ```azurecli
    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
    done
    ```

### <a name="step-4---run-hello-script"></a>Steg 4 – kör hello skript
Nu när du hämtade och ändrade hello skript baserat på dina behov, kör hello skriptet toocreate hello tillbaka avslutas databasen virtuella datorer med flera nätverkskort.

1. Spara skriptet och kör det från din **Bash** terminal. Hello inledande utdata visas enligt nedan.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. Hello körningen avslutas efter några minuter och du ser hello resten av hello utdata som visas nedan.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
