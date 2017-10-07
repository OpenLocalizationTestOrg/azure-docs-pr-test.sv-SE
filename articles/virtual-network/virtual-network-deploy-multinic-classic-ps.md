---
title: "aaaCreate en virtuell dator (klassisk) med flera nätverkskort – Azure PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en virtuell dator (klassisk) med flera nätverkskort med hjälp av PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 6e50f39a-2497-4845-a5d4-7332dbc203c5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 90c967929bb418042c3fb7079e0f69246faac53c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics-using-powershell"></a>Skapa en virtuell dator (klassisk) med flera nätverkskort med hjälp av PowerShell

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Du kan skapa virtuella datorer (VM) i Azure och bifoga flera nätverk gränssnitt (NIC) tooeach för dina virtuella datorer. Flera nätverkskort aktivera uppdelning av trafiktyper mellan nätverkskort. Till exempel anslutna en NIC kan kommunicera med hello Internet, medan en annan inte kommunicerar endast med interna resurser toohello Internet. hello möjlighet tooseparate nätverkstrafik på flera nätverkskort krävs för många virtuella nätverksenheter, till exempel leverans av program och optimering av WAN-lösningar.

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Lär dig hur tooperform dessa steg med hello [Resource Manager-distributionsmodellen](virtual-network-deploy-multinic-arm-ps.md).

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

hello så här använder du en resursgrupp med namnet *IaaSStory* för hello webbservrar och en resursgrupp med namnet *IaaSStory BackEnd* för hello DB-servrar.

## <a name="prerequisites"></a>Krav

Innan du kan skapa hello DB-servrar, behöver du toocreate hello *IaaSStory* resursgrupp med alla hello nödvändiga resurser för det här scenariot. toocreate dessa resurser, fullständig hello steg som följer. Skapa ett virtuellt nätverk genom att följa stegen hello i hello [skapa ett virtuellt nätverk](virtual-networks-create-vnet-classic-netcfg-ps.md) artikel.

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-back-end-vms"></a>Skapa hello backend-virtuella datorer
hello beror backend-VMs på hello skapande av hello följande resurser:

* **Backend-undernät**. hello databasservrar ska ingå i ett separat undernät, toosegregate trafik. hello-skriptet nedan kommer det här undernätet tooexist i ett vnet med namnet *WTestVnet*.
* **Storage-konto för datadiskar**. För bättre prestanda använder hello datadiskar på hello databasservrar Solid-State-hårddisk (SSD)-teknik som kräver ett premiumlagringskonto. Se till att hello Azure-plats som du distribuerar toosupport premium-lagring.
* **Tillgänglighetsuppsättningen**. Alla databasservrar läggs tooa enda tillgänglighetsuppsättning, tooensure minst en av hello virtuella datorer är igång och körs under underhåll.

### <a name="step-1---start-your-script"></a>Steg 1 – starta skriptet
Du kan hämta hello fullständig PowerShell-skript används [här](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Följ hello steg nedan toochange hello skriptet toowork i din miljö.

1. Ändra hello värdena för hello variabler nedan baserat på en befintlig resursgrupp distribuerade ovan i [krav](#Prerequisites).

    ```powershell
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    ```

2. Ändra hello värden hello variabler nedan baserat på hello värden som du vill toouse för backend-distribution.

    ```powershell
    $backendCSName         = "IaaSStory-Backend"
    $prmStorageAccountName = "iaasstoryprmstorage"
    $avSetName             = "ASDB"
    $vmSize                = "Standard_DS3"
    $diskSize              = 127
    $vmNamePrefix          = "DB"
    $dataDiskSuffix        = "datadisk"
    $ipAddressPrefix       = "192.168.2."
    $numberOfVMs           = 2
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Steg 2 – skapa nödvändiga resurser för din virtuella datorer
Du måste toocreate en ny molntjänst och en lagringsplats för kontot för hello datadiskar för alla virtuella datorer. Du måste också toospecify en bild och ett lokalt administratörskonto för hello virtuella datorer. toocreate dessa resurser, Slutför hello följande steg:

1. Skapa en ny molntjänst.

    ```powershell
    New-AzureService -ServiceName $backendCSName -Location $location
    ```

2. Skapa ett nytt premium-lagringskonto.

    ```powershell
    New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
    -Location $location -Type Premium_LRS
    ```
3. Ange hello lagringskonto skapade ovan som hello aktuella lagringskontot för din prenumeration.

    ```powershell
    $subscription = Get-AzureSubscription | where {$_.IsCurrent -eq $true}  
    Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
    -CurrentStorageAccountName $prmStorageAccountName
    ```

4. Välj en bild för hello VM.

    ```powershell
    $image = Get-AzureVMImage `
    | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
    | sort PublishedDate -Descending `
    | select -ExpandProperty ImageName -First 1
    ```

5. Ange autentiseringsuppgifter för hello lokala administratörskontot.

    ```powershell
    $cred = Get-Credential -Message "Enter username and password for local admin account"
    ```

### <a name="step-3---create-vms"></a>Steg 3 – skapa virtuella datorer
Du måste toouse toocreate en loop som många virtuella datorer som du vill och skapa hello nödvändiga nätverkskort och virtuella datorer i hello loop. toocreate hello nätverkskort och virtuella datorer kan du köra hello följande steg.

1. Starta en `for` loop toorepeat hello kommandon toocreate en virtuell dator och två nätverkskort så många gånger som behövs, beroende på hello värde hello `$numberOfVMs` variabeln.

    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. Skapa en `VMConfig` -objekt som anger hello bild, storlek och tillgänglighetsuppsättning för hello VM.

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureVMConfig -Name $vmName `
        -ImageName $image `
        -InstanceSize $vmSize `
        -AvailabilitySetName $avSetName
    ```

3. Etablera hello VM som en Windows-dator.

    ```powershell
    Add-AzureProvisioningConfig -VM $vmConfig -Windows `
        -AdminUsername $cred.UserName `
        -Password $cred.GetNetworkCredential().Password
    ```

4. Ange hello standard nätverkskort och tilldela den en statisk IP-adress.

    ```powershell
    Set-AzureSubnet         -SubnetNames $backendSubnetName -VM $vmConfig
    Set-AzureStaticVNetIP   -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig
    ```

5. Lägga till ett andra nätverkskort för varje virtuell dator.

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
    -SubnetName $backendSubnetName `
    -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
    -VM $vmConfig
    ```

6. Skapa toodata diskar för varje virtuell dator.

    ```powershell
    $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk1Name `
    -LUN 0

    $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk2Name `
    -LUN 1
    ```

7. Skapa varje virtuell dator och avsluta hello loop.

    ```powershell
    New-AzureVM -VM $vmConfig `
    -ServiceName $backendCSName `
    -Location $location `
    -VNetName $vnetName
    }
    ```

### <a name="step-4---run-hello-script"></a>Steg 4 – kör hello skript
Nu när du hämtade och ändrade utifrån hello skript dina behov, runt han skript toocreate hello serverdel databasen virtuella datorer med flera nätverkskort.

1. Spara skriptet och kör det från hello **PowerShell** Kommandotolken eller **PowerShell ISE**. Hello inledande utdata visas enligt nedan.

        OperationDescription    OperationId                          OperationStatus

        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        
        WARNING: No deployment found in service: 'IaaSStory-Backend'.
2. Fyll i hello information som behövs i hello autentiseringsuppgifter efterfrågas och klicka på **OK**. hello utdata nedan returneras.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
