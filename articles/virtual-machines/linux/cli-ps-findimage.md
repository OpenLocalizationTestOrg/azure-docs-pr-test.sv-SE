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
# <a name="how-toofind-linux-vm-images-in-hello-azure-marketplace-with-hello-azure-cli"></a><span data-ttu-id="7da84-103">Hur toofind Linux VM bilder i hello Azure Marketplace med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7da84-103">How toofind Linux VM images in hello Azure Marketplace with hello Azure CLI</span></span>
<span data-ttu-id="7da84-104">Det här avsnittet beskrivs hur toouse hello Azure CLI 2.0 toofind VM-avbildningar i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="7da84-104">This topic describes how toouse hello Azure CLI 2.0 toofind VM images in hello Azure Marketplace.</span></span> <span data-ttu-id="7da84-105">Använd den här informationen toospecify Marketplace-avbildning när du skapar en Linux VM.</span><span class="sxs-lookup"><span data-stu-id="7da84-105">Use this information toospecify a Marketplace image when you create a Linux VM.</span></span>

<span data-ttu-id="7da84-106">Kontrollera att du har installerat hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och loggas i tooan Azure-konto (`az login`).</span><span class="sxs-lookup"><span data-stu-id="7da84-106">Make sure that you installed hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and are logged in tooan Azure account (`az login`).</span></span>

## <a name="terminology"></a><span data-ttu-id="7da84-107">Terminologi</span><span class="sxs-lookup"><span data-stu-id="7da84-107">Terminology</span></span>

<span data-ttu-id="7da84-108">Marketplace-bilder identifieras i hello CLI och andra Azure-verktyg enligt tooa hierarki:</span><span class="sxs-lookup"><span data-stu-id="7da84-108">Marketplace images are identified in hello CLI and other Azure tools according tooa hierarchy:</span></span>

* <span data-ttu-id="7da84-109">**Publisher** -hello organisationen som skapade hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="7da84-109">**Publisher** - hello organization that created hello image.</span></span> <span data-ttu-id="7da84-110">Exempel: kanoniska</span><span class="sxs-lookup"><span data-stu-id="7da84-110">Example: Canonical</span></span>
* <span data-ttu-id="7da84-111">**Erbjuder** – en grupp av relaterade bilder som har skapats av en utgivare.</span><span class="sxs-lookup"><span data-stu-id="7da84-111">**Offer** - A group of related images created by a publisher.</span></span> <span data-ttu-id="7da84-112">Exempel: Ubuntu Server</span><span class="sxs-lookup"><span data-stu-id="7da84-112">Example: Ubuntu Server</span></span>
* <span data-ttu-id="7da84-113">**SKU** - en instans av ett erbjudande, till exempel en högre version av en distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="7da84-113">**SKU** - An instance of an offer, such as a major release of a distribution.</span></span> <span data-ttu-id="7da84-114">Exempel: 16.04-LTS</span><span class="sxs-lookup"><span data-stu-id="7da84-114">Example: 16.04-LTS</span></span>
* <span data-ttu-id="7da84-115">**Version** -hello versionsnumret för en bild SKU.</span><span class="sxs-lookup"><span data-stu-id="7da84-115">**Version** - hello version number of an image SKU.</span></span> <span data-ttu-id="7da84-116">När du anger hello bild, kan du ersätta hello versionsnummer med ”senaste”, som väljer hello senaste versionen av hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="7da84-116">When specifying hello image, you can replace hello version number with "latest", which selects hello latest version of hello distribution.</span></span>

<span data-ttu-id="7da84-117">toospecify Marketplace-avbildning du vanligtvis använda hello avbildning *URN*.</span><span class="sxs-lookup"><span data-stu-id="7da84-117">toospecify a Marketplace image, you typically use hello image *URN*.</span></span> <span data-ttu-id="7da84-118">hello URN kombinerar dessa värden, avgränsade med kolon (:) hello: *Publisher*:*erbjuder*:*Sku*:*Version*.</span><span class="sxs-lookup"><span data-stu-id="7da84-118">hello URN combines these values, separated by hello colon (:) character: *Publisher*:*Offer*:*Sku*:*Version*.</span></span> 


## <a name="list-popular-images"></a><span data-ttu-id="7da84-119">Lista över populära bilder</span><span class="sxs-lookup"><span data-stu-id="7da84-119">List popular images</span></span>

<span data-ttu-id="7da84-120">Kör hello [az vm bildlista](/cli/azure/vm/image#list) kommandot, utan hello `--all` alternativet, toosee en lista över populära VM bilder i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="7da84-120">Run hello [az vm image list](/cli/azure/vm/image#list) command, without hello `--all` option, toosee a list of popular VM images in hello Azure Marketplace.</span></span> <span data-ttu-id="7da84-121">Till exempel köra följande kommando toodisplay hello en cachelagrad lista över populära bilder i tabellformat:</span><span class="sxs-lookup"><span data-stu-id="7da84-121">For example, run hello following command toodisplay a cached list of popular images in table format:</span></span>

```azurecli
az vm image list --output table
```

<span data-ttu-id="7da84-122">hello utdata innehåller hello URN (hello värdet i hello *Urn* kolumn), som du använder toospecify hello bild.</span><span class="sxs-lookup"><span data-stu-id="7da84-122">hello output includes hello URN (hello value in hello *Urn* column), which you use toospecify hello image.</span></span> <span data-ttu-id="7da84-123">När du skapar en virtuell dator med någon av dessa populära Marketplace-avbildningar, kan du alternativt ange hello URN alias som *UbuntuLTS*.</span><span class="sxs-lookup"><span data-stu-id="7da84-123">When creating a VM with one of these popular Marketplace images, you can alternatively specify hello URN alias, such as *UbuntuLTS*.</span></span>

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

## <a name="find-specific-images"></a><span data-ttu-id="7da84-124">Söka efter specifika avbildningar</span><span class="sxs-lookup"><span data-stu-id="7da84-124">Find specific images</span></span>

<span data-ttu-id="7da84-125">toofind en specifik VM-avbildning i hello Marketplace, använda hello `az vm image list` med hello `--all` alternativet.</span><span class="sxs-lookup"><span data-stu-id="7da84-125">toofind a specific VM image in hello Marketplace, use hello `az vm image list` command with hello `--all` option.</span></span> <span data-ttu-id="7da84-126">Den här versionen av hello kommandot tar några tid toocomplete och kan returnera långa utdata så att du vanligtvis filtrera hello listan efter `--publisher` eller en annan parameter.</span><span class="sxs-lookup"><span data-stu-id="7da84-126">This version of hello command takes some time toocomplete and can return lengthy output, so you usually filter hello list by `--publisher` or another parameter.</span></span> 

<span data-ttu-id="7da84-127">Exempelvis hello följande kommando visar alla Debian erbjudanden (Kom ihåg att utan hello `--all` växla, söks endast hello lokalt cacheminne gemensamma nodbilder):</span><span class="sxs-lookup"><span data-stu-id="7da84-127">For example, hello following command displays all Debian offers (remember that without hello `--all` switch, it only searches hello local cache of common images):</span></span>

```azurecli
az vm image list --offer Debian --all --output table 

```

<span data-ttu-id="7da84-128">Partiella utdata:</span><span class="sxs-lookup"><span data-stu-id="7da84-128">Partial output:</span></span> 
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

<span data-ttu-id="7da84-129">Liknande filter med hello `--location`, `--publisher`, och `--sku` alternativ.</span><span class="sxs-lookup"><span data-stu-id="7da84-129">Apply similar filters with hello `--location`, `--publisher`, and `--sku` options.</span></span> <span data-ttu-id="7da84-130">Du kan även utföra delmatchningar på ett filter, till exempel söker efter `--offer Deb` toofind alla Debian bilder.</span><span class="sxs-lookup"><span data-stu-id="7da84-130">You can even perform partial matches on a filter, such as searching for `--offer Deb` toofind all Debian images.</span></span>

<span data-ttu-id="7da84-131">Om du inte anger en viss plats med hello `--location` alternativ, hello värden för `westus` returneras som standard.</span><span class="sxs-lookup"><span data-stu-id="7da84-131">If you don't specify a particular location with hello `--location` option, hello values for `westus` are returned by default.</span></span> <span data-ttu-id="7da84-132">(Ange en annan standardsökväg genom att köra `az configure --defaults location=<location>`.)</span><span class="sxs-lookup"><span data-stu-id="7da84-132">(Set a different default location by running `az configure --defaults location=<location>`.)</span></span>

<span data-ttu-id="7da84-133">Till exempel hello följande kommando visar alla Debian 8 SKU: er i `westeurope`:</span><span class="sxs-lookup"><span data-stu-id="7da84-133">For example, hello following command lists all Debian 8 SKUs in `westeurope`:</span></span>

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

<span data-ttu-id="7da84-134">Partiella utdata:</span><span class="sxs-lookup"><span data-stu-id="7da84-134">Partial output:</span></span>

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

## <a name="navigate-hello-images"></a><span data-ttu-id="7da84-135">Navigera hello bilder</span><span class="sxs-lookup"><span data-stu-id="7da84-135">Navigate hello images</span></span> 
<span data-ttu-id="7da84-136">Ett annat sätt toofind en avbildning på en plats är toorun hello [az vm avbildningen lista-utgivare](/cli/azure/vm/image#list-publishers), [az vm avbildningen lista-erbjudanden](/cli/azure/vm/image#list-offers), och [az vm avbildningen lista-SKU: er](/cli/azure/vm/image#list-skus) kommandon i sekvens.</span><span class="sxs-lookup"><span data-stu-id="7da84-136">Another way toofind an image in a location is toorun hello [az vm image list-publishers](/cli/azure/vm/image#list-publishers), [az vm image list-offers](/cli/azure/vm/image#list-offers), and [az vm image list-skus](/cli/azure/vm/image#list-skus) commands in sequence.</span></span> <span data-ttu-id="7da84-137">Med dessa kommandon bestämma dessa värden:</span><span class="sxs-lookup"><span data-stu-id="7da84-137">With these commands, you determine these values:</span></span>

1. <span data-ttu-id="7da84-138">Lista hello avbildningen utgivare.</span><span class="sxs-lookup"><span data-stu-id="7da84-138">List hello image publishers.</span></span>
2. <span data-ttu-id="7da84-139">Visa en lista över erbjudanden från en viss utgivare.</span><span class="sxs-lookup"><span data-stu-id="7da84-139">For a given publisher, list their offers.</span></span>
3. <span data-ttu-id="7da84-140">Visa en lista över SKU:er för ett visst erbjudande.</span><span class="sxs-lookup"><span data-stu-id="7da84-140">For a given offer, list their SKUs.</span></span>


<span data-ttu-id="7da84-141">Till exempel hello följande kommando visar hello avbildningen utgivare i hello västra USA plats:</span><span class="sxs-lookup"><span data-stu-id="7da84-141">For example, hello following command lists hello image publishers in hello West US location:</span></span>

```azurecli
az vm image list-publishers --location westus --output table
```

<span data-ttu-id="7da84-142">Partiella utdata:</span><span class="sxs-lookup"><span data-stu-id="7da84-142">Partial output:</span></span>

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
<span data-ttu-id="7da84-143">Använd den här informationen toofind erbjuder från en viss utgivare.</span><span class="sxs-lookup"><span data-stu-id="7da84-143">Use this information toofind offers from a specific publisher.</span></span> <span data-ttu-id="7da84-144">Till exempel om Canonical är en avbildningens utgivare i hello västra USA plats kan hitta sina erbjudanden genom att köra `azure vm image list-offers`.</span><span class="sxs-lookup"><span data-stu-id="7da84-144">For example, if Canonical is an image publisher in hello West US location, find their offers by running `azure vm image list-offers`.</span></span> <span data-ttu-id="7da84-145">Skicka hello plats och hello utgivare som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="7da84-145">Pass hello location and hello publisher as in hello following example:</span></span>

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

<span data-ttu-id="7da84-146">Resultat:</span><span class="sxs-lookup"><span data-stu-id="7da84-146">Output:</span></span>

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
<span data-ttu-id="7da84-147">Du ser att Canonical i hello västra USA region, publicerar hello **UbuntuServer** erbjudande på Azure.</span><span class="sxs-lookup"><span data-stu-id="7da84-147">You see that in hello West US region, Canonical publishes hello **UbuntuServer** offer on Azure.</span></span> <span data-ttu-id="7da84-148">Men vad SKU: er? tooget dessa värden kör `azure vm image list-skus` och ange hello plats och utgivare som du har identifierat:</span><span class="sxs-lookup"><span data-stu-id="7da84-148">But what SKUs? tooget those values, run `azure vm image list-skus` and set hello location, publisher, and offer that you have discovered:</span></span>

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

<span data-ttu-id="7da84-149">Resultat:</span><span class="sxs-lookup"><span data-stu-id="7da84-149">Output:</span></span>

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

<span data-ttu-id="7da84-150">Använd slutligen hello `az vm image list` kommandot toofind en viss version av hello SKU som du vill **16.04 LTS**:</span><span class="sxs-lookup"><span data-stu-id="7da84-150">Finally, use hello `az vm image list` command toofind a specific version of hello SKU you want, for example, **16.04-LTS**:</span></span>

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --all --output table
```

<span data-ttu-id="7da84-151">Resultat:</span><span class="sxs-lookup"><span data-stu-id="7da84-151">Output:</span></span>

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
## <a name="next-steps"></a><span data-ttu-id="7da84-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7da84-152">Next steps</span></span>
<span data-ttu-id="7da84-153">Nu kan du välja exakt hello bild vill du ha toouse genom att anteckna hello URN värde.</span><span class="sxs-lookup"><span data-stu-id="7da84-153">Now you can choose precisely hello image you want toouse by taking note of hello URN value.</span></span> <span data-ttu-id="7da84-154">Skicka det här värdet med hello `--image` parameter när du skapar en virtuell dator med hello [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="7da84-154">Pass this value with hello `--image` parameter when you create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="7da84-155">Kom ihåg att du kan ersätta hello versionsnumret i hello URN med ”senaste”.</span><span class="sxs-lookup"><span data-stu-id="7da84-155">Remember that you can optionally replace hello version number in hello URN with "latest".</span></span> <span data-ttu-id="7da84-156">Den här versionen är alltid hello senaste versionen av hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="7da84-156">This version is always hello latest version of hello distribution.</span></span> <span data-ttu-id="7da84-157">toocreate en virtuell dator snabbt med hjälp av hello URN information, se [skapa och hantera virtuella Linux-datorer med hello Azure CLI](tutorial-manage-vm.md).</span><span class="sxs-lookup"><span data-stu-id="7da84-157">toocreate a virtual machine quickly by using hello URN information, see [Create and Manage Linux VMs with hello Azure CLI](tutorial-manage-vm.md).</span></span>
