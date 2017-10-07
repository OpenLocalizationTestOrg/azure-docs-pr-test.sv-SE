---
title: aaaSelect Linux VM bilder med hello Azure CLI | Microsoft Docs
description: "Lär dig hur toouse hello Azure CLI toodetermine hello utgivare, erbjudande, SKU och version för Marketplace VM-avbildningar."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7a858e38-4f17-4e8e-a28a-c7f801101721
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/24/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0b115b8654bc156b5bfadba53a6b002a105acb68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-linux-vm-images-in-hello-azure-marketplace-with-hello-azure-cli"></a>Hur toofind Linux VM bilder i hello Azure Marketplace med hello Azure CLI
Det här avsnittet beskrivs hur toouse hello Azure CLI 2.0 toofind VM-avbildningar i hello Azure Marketplace. Använd den här informationen toospecify Marketplace-avbildning när du skapar en Linux VM.

Kontrollera att du har installerat hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och loggas i tooan Azure-konto (`az login`).

## <a name="terminology"></a>Terminologi

Marketplace-bilder identifieras i hello CLI och andra Azure-verktyg enligt tooa hierarki:

* **Publisher** -hello organisationen som skapade hello avbildningen. Exempel: kanoniska
* **Erbjuder** – en grupp av relaterade bilder som har skapats av en utgivare. Exempel: Ubuntu Server
* **SKU** - en instans av ett erbjudande, till exempel en högre version av en distributionsplats. Exempel: 16.04-LTS
* **Version** -hello versionsnumret för en bild SKU. När du anger hello bild, kan du ersätta hello versionsnummer med ”senaste”, som väljer hello senaste versionen av hello-distribution.

toospecify Marketplace-avbildning du vanligtvis använda hello avbildning *URN*. hello URN kombinerar dessa värden, avgränsade med kolon (:) hello: *Publisher*:*erbjuder*:*Sku*:*Version*. 


## <a name="list-popular-images"></a>Lista över populära bilder

Kör hello [az vm bildlista](/cli/azure/vm/image#list) kommandot, utan hello `--all` alternativet, toosee en lista över populära VM bilder i hello Azure Marketplace. Till exempel köra följande kommando toodisplay hello en cachelagrad lista över populära bilder i tabellformat:

```azurecli
az vm image list --output table
```

hello utdata innehåller hello URN (hello värdet i hello *Urn* kolumn), som du använder toospecify hello bild. När du skapar en virtuell dator med någon av dessa populära Marketplace-avbildningar, kan du alternativt ange hello URN alias som *UbuntuLTS*.

```
You are viewing an offline list of images, use --all tooretrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
...
```

## <a name="find-specific-images"></a>Söka efter specifika avbildningar

toofind en specifik VM-avbildning i hello Marketplace, använda hello `az vm image list` med hello `--all` alternativet. Den här versionen av hello kommandot tar några tid toocomplete och kan returnera långa utdata så att du vanligtvis filtrera hello listan efter `--publisher` eller en annan parameter. 

Exempelvis hello följande kommando visar alla Debian erbjudanden (Kom ihåg att utan hello `--all` växla, söks endast hello lokalt cacheminne gemensamma nodbilder):

```azurecli
az vm image list --offer Debian --all --output table 

```

Partiella utdata: 
```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  --------------
Debian   credativ     7                  credativ:Debian:7:7.0.201602010                  7.0.201602010
Debian   credativ     7                  credativ:Debian:7:7.0.201603020                  7.0.201603020
Debian   credativ     7                  credativ:Debian:7:7.0.201604050                  7.0.201604050
Debian   credativ     7                  credativ:Debian:7:7.0.201604200                  7.0.201604200
Debian   credativ     7                  credativ:Debian:7:7.0.201606280                  7.0.201606280
Debian   credativ     7                  credativ:Debian:7:7.0.201609120                  7.0.201609120
Debian   credativ     7                  credativ:Debian:7:7.0.201611020                  7.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
Debian   credativ     8                  credativ:Debian:8:8.0.201708040                  8.0.201708040
...
```

Liknande filter med hello `--location`, `--publisher`, och `--sku` alternativ. Du kan även utföra delmatchningar på ett filter, till exempel söker efter `--offer Deb` toofind alla Debian bilder.

Om du inte anger en viss plats med hello `--location` alternativ, hello värden för `westus` returneras som standard. (Ange en annan standardsökväg genom att köra `az configure --defaults location=<location>`.)

Till exempel hello följande kommando visar alla Debian 8 SKU: er i `westeurope`:

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

Partiella utdata:

```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  -------------
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
...
```

## <a name="navigate-hello-images"></a>Navigera hello bilder 
Ett annat sätt toofind en avbildning på en plats är toorun hello [az vm avbildningen lista-utgivare](/cli/azure/vm/image#list-publishers), [az vm avbildningen lista-erbjudanden](/cli/azure/vm/image#list-offers), och [az vm avbildningen lista-SKU: er](/cli/azure/vm/image#list-skus) kommandon i sekvens. Med dessa kommandon bestämma dessa värden:

1. Lista hello avbildningen utgivare.
2. Visa en lista över erbjudanden från en viss utgivare.
3. Visa en lista över SKU:er för ett visst erbjudande.


Till exempel hello följande kommando visar hello avbildningen utgivare i hello västra USA plats:

```azurecli
az vm image list-publishers --location westus --output table
```

Partiella utdata:

```
Location    Name
----------  ----------------------------------------------------
westus      1e
westus      4psa
westus      7isolutions
westus      a10networks
westus      abiquo
westus      accellion
westus      Acronis
westus      Acronis.Backup
westus      actian_matrix
westus      actifio
westus      activeeon
westus      adatao
...
```
Använd den här informationen toofind erbjuder från en viss utgivare. Till exempel om Canonical är en avbildningens utgivare i hello västra USA plats kan hitta sina erbjudanden genom att köra `azure vm image list-offers`. Skicka hello plats och hello utgivare som i följande exempel hello:

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

Resultat:

```
Location    Name
----------  -------------------------
westus      Ubuntu15.04Snappy
westus      Ubuntu15.04SnappyDocker
westus      UbunturollingSnappy
westus      UbuntuServer
westus      Ubuntu_Core
westus      Ubuntu_Snappy_Core
westus      Ubuntu_Snappy_Core_Docker
```
Du ser att Canonical i hello västra USA region, publicerar hello **UbuntuServer** erbjudande på Azure. Men vad SKU: er? tooget dessa värden kör `azure vm image list-skus` och ange hello plats och utgivare som du har identifierat:

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

Resultat:

```
Location    Name
----------  -----------------
westus      12.04.3-LTS
westus      12.04.4-LTS
westus      12.04.5-DAILY-LTS
westus      12.04.5-LTS
westus      12.10
westus      14.04.0-LTS
westus      14.04.1-LTS
westus      14.04.2-LTS
westus      14.04.3-LTS
westus      14.04.4-LTS
westus      14.04.5-DAILY-LTS
westus      14.04.5-LTS
westus      16.04-beta
westus      16.04-DAILY-LTS
westus      16.04-LTS
westus      16.04.0-LTS
westus      16.10
westus      16.10-DAILY
westus      17.04
westus      17.04-DAILY
westus      17.10-DAILY
```

Använd slutligen hello `az vm image list` kommandot toofind en viss version av hello SKU som du vill **16.04 LTS**:

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --all --output table
```

Resultat:

```
Offer         Publisher    Sku        Urn                                               Version
------------  -----------  ---------  ------------------------------------------------  ---------------
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611220  16.04.201611220
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611300  16.04.201611300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612050  16.04.201612050
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612140  16.04.201612140
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612210  16.04.201612210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201701130  16.04.201701130
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702020  16.04.201702020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702200  16.04.201702200
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702210  16.04.201702210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702240  16.04.201702240
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703020  16.04.201703020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703030  16.04.201703030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703070  16.04.201703070
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703270  16.04.201703270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703280  16.04.201703280
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703300  16.04.201703300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705080  16.04.201705080
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705160  16.04.201705160
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706100  16.04.201706100
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706191  16.04.201706191
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707210  16.04.201707210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707270  16.04.201707270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708030  16.04.201708030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708110  16.04.201708110
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708151  16.04.201708151
```
## <a name="next-steps"></a>Nästa steg
Nu kan du välja exakt hello bild vill du ha toouse genom att anteckna hello URN värde. Skicka det här värdet med hello `--image` parameter när du skapar en virtuell dator med hello [az vm skapa](/cli/azure/vm#create) kommando. Kom ihåg att du kan ersätta hello versionsnumret i hello URN med ”senaste”. Den här versionen är alltid hello senaste versionen av hello-distribution. toocreate en virtuell dator snabbt med hjälp av hello URN information, se [skapa och hantera virtuella Linux-datorer med hello Azure CLI](tutorial-manage-vm.md).
