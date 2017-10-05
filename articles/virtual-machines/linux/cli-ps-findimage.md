---
title: "Välj Linux VM-avbildningar med Azure CLI | Microsoft Docs"
description: "Lär dig hur du använder Azure CLI för att fastställa utgivare, erbjudande, SKU och version för Marketplace VM-avbildningar."
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
ms.openlocfilehash: e0c27a7ee9e9a7ab1a3b004e070fa556b56a36a5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-find-linux-vm-images-in-the-azure-marketplace-with-the-azure-cli"></a><span data-ttu-id="faa55-103">Hur du hittar Linux VM avbildningar i Azure Marketplace med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="faa55-103">How to find Linux VM images in the Azure Marketplace with the Azure CLI</span></span>
<span data-ttu-id="faa55-104">Det här avsnittet beskriver hur du använder Azure CLI 2.0 för att hitta VM-avbildningar i Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="faa55-104">This topic describes how to use the Azure CLI 2.0 to find VM images in the Azure Marketplace.</span></span> <span data-ttu-id="faa55-105">Använd informationen för att ange en Marketplace-avbildning när du skapar en Linux VM.</span><span class="sxs-lookup"><span data-stu-id="faa55-105">Use this information to specify a Marketplace image when you create a Linux VM.</span></span>

<span data-ttu-id="faa55-106">Kontrollera att du har installerat senast [Azure CLI 2.0](/cli/azure/install-az-cli2) och är inloggad på ett Azure-konto (`az login`).</span><span class="sxs-lookup"><span data-stu-id="faa55-106">Make sure that you installed the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and are logged in to an Azure account (`az login`).</span></span>

## <a name="terminology"></a><span data-ttu-id="faa55-107">Terminologi</span><span class="sxs-lookup"><span data-stu-id="faa55-107">Terminology</span></span>

<span data-ttu-id="faa55-108">Marketplace-bilder identifieras i CLI och andra Azure-verktyg enligt en hierarki:</span><span class="sxs-lookup"><span data-stu-id="faa55-108">Marketplace images are identified in the CLI and other Azure tools according to a hierarchy:</span></span>

* <span data-ttu-id="faa55-109">**Publisher** -organisationen som skapade avbildningen.</span><span class="sxs-lookup"><span data-stu-id="faa55-109">**Publisher** - The organization that created the image.</span></span> <span data-ttu-id="faa55-110">Exempel: kanoniska</span><span class="sxs-lookup"><span data-stu-id="faa55-110">Example: Canonical</span></span>
* <span data-ttu-id="faa55-111">**Erbjuder** – en grupp av relaterade bilder som har skapats av en utgivare.</span><span class="sxs-lookup"><span data-stu-id="faa55-111">**Offer** - A group of related images created by a publisher.</span></span> <span data-ttu-id="faa55-112">Exempel: Ubuntu Server</span><span class="sxs-lookup"><span data-stu-id="faa55-112">Example: Ubuntu Server</span></span>
* <span data-ttu-id="faa55-113">**SKU** - en instans av ett erbjudande, till exempel en högre version av en distributionsplats.</span><span class="sxs-lookup"><span data-stu-id="faa55-113">**SKU** - An instance of an offer, such as a major release of a distribution.</span></span> <span data-ttu-id="faa55-114">Exempel: 16.04-LTS</span><span class="sxs-lookup"><span data-stu-id="faa55-114">Example: 16.04-LTS</span></span>
* <span data-ttu-id="faa55-115">**Version** -versionsnumret för en bild SKU.</span><span class="sxs-lookup"><span data-stu-id="faa55-115">**Version** - The version number of an image SKU.</span></span> <span data-ttu-id="faa55-116">När du anger avbildningen, kan du ersätta versionsnumret med ”senaste”, som väljs den senaste versionen av distributionen.</span><span class="sxs-lookup"><span data-stu-id="faa55-116">When specifying the image, you can replace the version number with "latest", which selects the latest version of the distribution.</span></span>

<span data-ttu-id="faa55-117">Om du vill ange en Marketplace-avbildning, använder du vanligtvis bilden *URN*.</span><span class="sxs-lookup"><span data-stu-id="faa55-117">To specify a Marketplace image, you typically use the image *URN*.</span></span> <span data-ttu-id="faa55-118">URN kombinerar dessa värden, avgränsade med kolon (:): *Publisher*:*erbjuder*:*Sku*:*Version*.</span><span class="sxs-lookup"><span data-stu-id="faa55-118">The URN combines these values, separated by the colon (:) character: *Publisher*:*Offer*:*Sku*:*Version*.</span></span> 


## <a name="list-popular-images"></a><span data-ttu-id="faa55-119">Lista över populära bilder</span><span class="sxs-lookup"><span data-stu-id="faa55-119">List popular images</span></span>

<span data-ttu-id="faa55-120">Kör den [az vm bildlista](/cli/azure/vm/image#list) kommandot, utan de `--all` alternativet om du vill se en lista över populära VM-avbildningar i Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="faa55-120">Run the [az vm image list](/cli/azure/vm/image#list) command, without the `--all` option, to see a list of popular VM images in the Azure Marketplace.</span></span> <span data-ttu-id="faa55-121">Till exempel köra följande kommando för att visa en cachelagrad lista över populära bilder i tabellformat:</span><span class="sxs-lookup"><span data-stu-id="faa55-121">For example, run the following command to display a cached list of popular images in table format:</span></span>

```azurecli
az vm image list --output table
```

<span data-ttu-id="faa55-122">Utdata innehåller URN (värdet i den *Urn* kolumn), där du anger du vilken avbildning.</span><span class="sxs-lookup"><span data-stu-id="faa55-122">The output includes the URN (the value in the *Urn* column), which you use to specify the image.</span></span> <span data-ttu-id="faa55-123">När du skapar en virtuell dator med någon av dessa populära Marketplace-avbildningar, kan du alternativt ange URN-alias som *UbuntuLTS*.</span><span class="sxs-lookup"><span data-stu-id="faa55-123">When creating a VM with one of these popular Marketplace images, you can alternatively specify the URN alias, such as *UbuntuLTS*.</span></span>

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
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

## <a name="find-specific-images"></a><span data-ttu-id="faa55-124">Söka efter specifika avbildningar</span><span class="sxs-lookup"><span data-stu-id="faa55-124">Find specific images</span></span>

<span data-ttu-id="faa55-125">Använd för att hitta en specifik VM-avbildning i Marketplace den `az vm image list` kommandot med de `--all` alternativet.</span><span class="sxs-lookup"><span data-stu-id="faa55-125">To find a specific VM image in the Marketplace, use the `az vm image list` command with the `--all` option.</span></span> <span data-ttu-id="faa55-126">Den här versionen av kommandot tar en stund att slutföra och kan returnera långa utdata så att du vanligtvis filtrera listan efter `--publisher` eller en annan parameter.</span><span class="sxs-lookup"><span data-stu-id="faa55-126">This version of the command takes some time to complete and can return lengthy output, so you usually filter the list by `--publisher` or another parameter.</span></span> 

<span data-ttu-id="faa55-127">Till exempel följande kommando visar alla Debian erbjudanden (Kom ihåg att utan den `--all` växla, söks endast den lokala cachen gemensamma nodbilder):</span><span class="sxs-lookup"><span data-stu-id="faa55-127">For example, the following command displays all Debian offers (remember that without the `--all` switch, it only searches the local cache of common images):</span></span>

```azurecli
az vm image list --offer Debian --all --output table 

```

<span data-ttu-id="faa55-128">Partiella utdata:</span><span class="sxs-lookup"><span data-stu-id="faa55-128">Partial output:</span></span> 
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

<span data-ttu-id="faa55-129">Liknande filter med det `--location`, `--publisher`, och `--sku` alternativ.</span><span class="sxs-lookup"><span data-stu-id="faa55-129">Apply similar filters with the `--location`, `--publisher`, and `--sku` options.</span></span> <span data-ttu-id="faa55-130">Du kan även utföra delmatchningar på ett filter, till exempel söker efter `--offer Deb` att hitta alla Debian bilder.</span><span class="sxs-lookup"><span data-stu-id="faa55-130">You can even perform partial matches on a filter, such as searching for `--offer Deb` to find all Debian images.</span></span>

<span data-ttu-id="faa55-131">Om du inte anger en viss plats med den `--location` alternativ, värdena för `westus` returneras som standard.</span><span class="sxs-lookup"><span data-stu-id="faa55-131">If you don't specify a particular location with the `--location` option, the values for `westus` are returned by default.</span></span> <span data-ttu-id="faa55-132">(Ange en annan standardsökväg genom att köra `az configure --defaults location=<location>`.)</span><span class="sxs-lookup"><span data-stu-id="faa55-132">(Set a different default location by running `az configure --defaults location=<location>`.)</span></span>

<span data-ttu-id="faa55-133">Till exempel följande kommando visar alla Debian 8 SKU: er i `westeurope`:</span><span class="sxs-lookup"><span data-stu-id="faa55-133">For example, the following command lists all Debian 8 SKUs in `westeurope`:</span></span>

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

<span data-ttu-id="faa55-134">Partiella utdata:</span><span class="sxs-lookup"><span data-stu-id="faa55-134">Partial output:</span></span>

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

## <a name="navigate-the-images"></a><span data-ttu-id="faa55-135">Navigera avbildningar</span><span class="sxs-lookup"><span data-stu-id="faa55-135">Navigate the images</span></span> 
<span data-ttu-id="faa55-136">Ett annat sätt att hitta en bild på en plats är att köra den [az vm avbildningen lista-utgivare](/cli/azure/vm/image#list-publishers), [az vm avbildningen lista-erbjudanden](/cli/azure/vm/image#list-offers), och [az vm avbildningen lista-SKU: er](/cli/azure/vm/image#list-skus) kommandon i sekvens.</span><span class="sxs-lookup"><span data-stu-id="faa55-136">Another way to find an image in a location is to run the [az vm image list-publishers](/cli/azure/vm/image#list-publishers), [az vm image list-offers](/cli/azure/vm/image#list-offers), and [az vm image list-skus](/cli/azure/vm/image#list-skus) commands in sequence.</span></span> <span data-ttu-id="faa55-137">Med dessa kommandon bestämma dessa värden:</span><span class="sxs-lookup"><span data-stu-id="faa55-137">With these commands, you determine these values:</span></span>

1. <span data-ttu-id="faa55-138">Visa en lista över avbildningsutgivare.</span><span class="sxs-lookup"><span data-stu-id="faa55-138">List the image publishers.</span></span>
2. <span data-ttu-id="faa55-139">Visa en lista över erbjudanden från en viss utgivare.</span><span class="sxs-lookup"><span data-stu-id="faa55-139">For a given publisher, list their offers.</span></span>
3. <span data-ttu-id="faa55-140">Visa en lista över SKU:er för ett visst erbjudande.</span><span class="sxs-lookup"><span data-stu-id="faa55-140">For a given offer, list their SKUs.</span></span>


<span data-ttu-id="faa55-141">Till exempel visar följande kommando bilden utgivare i USA, västra plats:</span><span class="sxs-lookup"><span data-stu-id="faa55-141">For example, the following command lists the image publishers in the West US location:</span></span>

```azurecli
az vm image list-publishers --location westus --output table
```

<span data-ttu-id="faa55-142">Partiella utdata:</span><span class="sxs-lookup"><span data-stu-id="faa55-142">Partial output:</span></span>

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
<span data-ttu-id="faa55-143">Använd den här informationen kan hitta från en viss utgivare.</span><span class="sxs-lookup"><span data-stu-id="faa55-143">Use this information to find offers from a specific publisher.</span></span> <span data-ttu-id="faa55-144">Till exempel om Canonical är en avbildningens utgivare i USA, västra platsen kan hitta sina erbjudanden genom att köra `azure vm image list-offers`.</span><span class="sxs-lookup"><span data-stu-id="faa55-144">For example, if Canonical is an image publisher in the West US location, find their offers by running `azure vm image list-offers`.</span></span> <span data-ttu-id="faa55-145">Ange platsen och utgivare som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="faa55-145">Pass the location and the publisher as in the following example:</span></span>

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

<span data-ttu-id="faa55-146">Resultat:</span><span class="sxs-lookup"><span data-stu-id="faa55-146">Output:</span></span>

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
<span data-ttu-id="faa55-147">Du ser att i regionen USA, västra Canonical publicerar den **UbuntuServer** erbjudande på Azure.</span><span class="sxs-lookup"><span data-stu-id="faa55-147">You see that in the West US region, Canonical publishes the **UbuntuServer** offer on Azure.</span></span> <span data-ttu-id="faa55-148">Men vad finns det för SKU:er?</span><span class="sxs-lookup"><span data-stu-id="faa55-148">But what SKUs?</span></span> <span data-ttu-id="faa55-149">För att få dessa värden, köra `azure vm image list-skus` och ange den plats och utgivare som du har identifierat:</span><span class="sxs-lookup"><span data-stu-id="faa55-149">To get those values, run `azure vm image list-skus` and set the location, publisher, and offer that you have discovered:</span></span>

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

<span data-ttu-id="faa55-150">Resultat:</span><span class="sxs-lookup"><span data-stu-id="faa55-150">Output:</span></span>

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

<span data-ttu-id="faa55-151">Använd slutligen den `az vm image list` kommando som söker efter en viss version av SKU: N som du vill **16.04 LTS**:</span><span class="sxs-lookup"><span data-stu-id="faa55-151">Finally, use the `az vm image list` command to find a specific version of the SKU you want, for example, **16.04-LTS**:</span></span>

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --all --output table
```

<span data-ttu-id="faa55-152">Resultat:</span><span class="sxs-lookup"><span data-stu-id="faa55-152">Output:</span></span>

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
## <a name="next-steps"></a><span data-ttu-id="faa55-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="faa55-153">Next steps</span></span>
<span data-ttu-id="faa55-154">Du kan nu välja exakt den avbildning som du vill använda genom att anteckna värdet för URN.</span><span class="sxs-lookup"><span data-stu-id="faa55-154">Now you can choose precisely the image you want to use by taking note of the URN value.</span></span> <span data-ttu-id="faa55-155">Skicka detta värde med den `--image` parameter när du skapar en virtuell dator med den [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="faa55-155">Pass this value with the `--image` parameter when you create a VM with the [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="faa55-156">Kom ihåg att du kan ersätta versionsnumret i URN med ”senaste”.</span><span class="sxs-lookup"><span data-stu-id="faa55-156">Remember that you can optionally replace the version number in the URN with "latest".</span></span> <span data-ttu-id="faa55-157">Den här versionen är alltid den senaste versionen av distributionen.</span><span class="sxs-lookup"><span data-stu-id="faa55-157">This version is always the latest version of the distribution.</span></span> <span data-ttu-id="faa55-158">För att snabbt skapa en virtuell dator med hjälp av URN information, se [skapa och hantera virtuella Linux-datorer med Azure CLI](tutorial-manage-vm.md).</span><span class="sxs-lookup"><span data-stu-id="faa55-158">To create a virtual machine quickly by using the URN information, see [Create and Manage Linux VMs with the Azure CLI](tutorial-manage-vm.md).</span></span>
