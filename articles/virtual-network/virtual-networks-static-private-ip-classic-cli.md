---
title: "aaaConfigure privata IP-adresser för virtuella datorer (klassisk) - Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur tooconfigure privata IP-adresser för virtuella datorer (klassisk) med hello Azure-kommandoradsgränssnittet (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17386acf-c708-4103-9b22-ff9bf04b778d
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 417a57181bcf5c2e6101bf3bdf63fc94ebc99df5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-hello-azure-cli-10"></a>Konfigurera privat IP-adresser för en virtuell dator (klassisk) med hello Azure CLI 1.0

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Den här artikeln beskriver hello klassiska distributionsmodellen. Du kan också [hantera en statisk privat IP-adress i hello Resource Manager-distributionsmodellen](virtual-networks-static-private-ip-arm-cli.md).

hello exempel Azure CLI-kommandona nedan förväntar sig en enkel miljö som redan har skapats. Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö som beskrivs i [skapa ett vnet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a>Hur toospecify en statisk privat IP-adress när du skapar en virtuell dator
toocreate en ny virtuell dator med namnet *DNS01* i en ny molntjänst med namnet *TestService* baserat på hello scenariot ovan, gör du följande:

1. Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.
2. Kör hello **azure-tjänsten skapar** kommandot toocreate hello-Molntjänsten.
   
        azure service create TestService --location uscentral
   
    Förväntad utdata:
   
        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
3. Kör hello **azure skapa vm** kommandot toocreate hello VM. Observera hello-värdet för en statisk privat IP-adress. hello-listan som visas efter hello utdata förklarar hello parametrar som används.
   
        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd
   
    Förväntad utdata:
   
        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting too"Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK
   
   * **-l (eller --location)**. Azure-region där hello VM kommer att skapas. I vårt scenario, *centralus*.
   * **-n (eller--vm-namn)**. Namnet på hello VM toobe skapas.
   * **-w (eller--virtuella nätverksnamnet)**. Namnet på hello VNet där hello VM kommer att skapas. 
   * **-S (eller--statisk ip)**. Statisk privat IP-adress för hello VM.
   * **TestService**. Namnet på hello molnbaserad tjänst där hello VM kommer att skapas.
   * **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**. Bild som används för toocreate hello VM.
   * **adminuser**. Lokal administratör för hello Windows VM.
   * **AdminP@ssw0rd**. Lokala administratörslösenordet för hello Windows VM.

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a>Hur tooretrieve statiska privata IP-information för en virtuell dator
tooview hello statiska privata IP-information för hello skapas den virtuella datorn med hello skriptet ovan, kör följande Azure CLI kommando hello och se hello värde för *nätverk StaticIP*:

    azure vm static-ip show DNS01

Förväntad utdata:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a>Hur tooremove en statisk privat IP-adress från en virtuell dator
tooremove hello statisk privat IP-adress läggs toohello VM i hello skriptet ovan, kör hello följande Azure CLI-kommando:

    azure vm static-ip remove DNS01

Förväntad utdata:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-tooadd-a-static-private-ip-tooan-existing-vm"></a>Hur tooadd en statisk privat IP-tooan befintlig virtuell dator
tooadd en statisk privat IP-adress toohello VM som skapats med hjälp av hello skriptet ovan runt han följande kommando:

    azure vm static-ip set DNS01 192.168.1.101

Förväntad utdata:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [reserverade offentliga IP-Adressen](virtual-networks-reserved-public-ip.md) adresser.
* Lär dig mer om [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) adresser.
* Kontakta hello [reserverade IP-REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx).

