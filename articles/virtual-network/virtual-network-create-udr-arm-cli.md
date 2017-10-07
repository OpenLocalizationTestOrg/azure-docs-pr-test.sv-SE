---
title: "aaaControl Routning och virtuella installationer med hjälp av hello Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur toocontrol Routning och virtuella installationer med hjälp av hello Azure CLI 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5452a0b8-21a6-4699-8d6a-e2d8faf32c25
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/12/2017
ms.author: jdial
ms.openlocfilehash: 79b908848932a4365dab1b7497b6a0dbf44bbaf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-hello-azure-cli-20"></a><span data-ttu-id="2655e-103">Skapa användardefinierade vägar (UDR) med hjälp av hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2655e-103">Create User-Defined Routes (UDR) using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2655e-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2655e-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="2655e-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2655e-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="2655e-106">Mall</span><span class="sxs-lookup"><span data-stu-id="2655e-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="2655e-107">PowerShell (klassisk distribution)</span><span class="sxs-lookup"><span data-stu-id="2655e-107">PowerShell (Classic deployment)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="2655e-108">CLI (klassisk distribution)</span><span class="sxs-lookup"><span data-stu-id="2655e-108">CLI (Classic deployment)</span></span>](virtual-network-create-udr-classic-cli.md)

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="2655e-109">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="2655e-109">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="2655e-110">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="2655e-110">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="2655e-111">[Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – våra CLI för hello klassisk och resurs management distributionsmodeller</span><span class="sxs-lookup"><span data-stu-id="2655e-111">[Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="2655e-112">[Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) -vår nästa generations CLI för hello management resursdistributionsmodell (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="2655e-112">[Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) - our next generation CLI for hello resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="2655e-113">hello exempel Azure CLI-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="2655e-113">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="2655e-114">Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö genom att distribuera [mallen](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klickar du på **distribuera tooAzure**, Ersätt hello Standardparametervärden Om nödvändigt och hello anvisningar i hello portal.</span><span class="sxs-lookup"><span data-stu-id="2655e-114">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>


## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="2655e-115">Skapa hello UDR för hello frontend undernät</span><span class="sxs-lookup"><span data-stu-id="2655e-115">Create hello UDR for hello front-end subnet</span></span>
<span data-ttu-id="2655e-116">routningstabellen för toocreate hello och väg som behövs för hello klientdelen undernät baserat på hello scenariot ovan, följer du hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="2655e-116">toocreate hello route table and route needed for hello front end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="2655e-117">Skapa en routingtabell för hello frontend undernät med hello [az nätverket routningstabellen skapa](/cli/azure/network/route-table#create) kommando:</span><span class="sxs-lookup"><span data-stu-id="2655e-117">Create a route table for hello front-end subnet with hello [az network route-table create](/cli/azure/network/route-table#create) command:</span></span>

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --location centralus \
    --name UDR-FrontEnd
    ```

    <span data-ttu-id="2655e-118">Resultat:</span><span class="sxs-lookup"><span data-stu-id="2655e-118">Output:</span></span>

    ```json
    {
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
    "location": "centralus",
    "name": "UDR-FrontEnd",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "routes": [],
    "subnets": null,
    "tags": null,
    "type": "Microsoft.Network/routeTables"
    }
    ```

2. <span data-ttu-id="2655e-119">Skapa en väg som skickar all trafik toohello backend-undernät (192.168.2.0/24) toohello **FW1** VM (192.168.0.4) med hjälp av hello [az nätverksväg routningstabellen skapa](/cli/azure/network/route-table/route#create) kommando:</span><span class="sxs-lookup"><span data-stu-id="2655e-119">Create a route that sends all traffic destined toohello back-end subnet (192.168.2.0/24) toohello **FW1** VM (192.168.0.4) using hello [az network route-table route create](/cli/azure/network/route-table/route#create) command:</span></span>

    ```azurecli 
    az network route-table route create \
    --resource-group testrg \
    --name RouteToBackEnd \
    --route-table-name UDR-FrontEnd \
    --address-prefix 192.168.2.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

    <span data-ttu-id="2655e-120">Resultat:</span><span class="sxs-lookup"><span data-stu-id="2655e-120">Output:</span></span>

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd/routes/RouteToBackEnd",
    "name": "RouteToBackEnd",
    "nextHopIpAddress": "192.168.0.4",
    "nextHopType": "VirtualAppliance",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg"
    }
    ```
    <span data-ttu-id="2655e-121">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="2655e-121">Parameters:</span></span>

    * <span data-ttu-id="2655e-122">**--routningstabellnamnet-**.</span><span class="sxs-lookup"><span data-stu-id="2655e-122">**--route-table-name**.</span></span> <span data-ttu-id="2655e-123">Namnet på hello routningstabellen där hello flödet kommer att läggas till.</span><span class="sxs-lookup"><span data-stu-id="2655e-123">Name of hello route table where hello route will be added.</span></span> <span data-ttu-id="2655e-124">I vårt scenario, *UDR klientdel*.</span><span class="sxs-lookup"><span data-stu-id="2655e-124">For our scenario, *UDR-FrontEnd*.</span></span>
    * <span data-ttu-id="2655e-125">**--address-prefix**.</span><span class="sxs-lookup"><span data-stu-id="2655e-125">**--address-prefix**.</span></span> <span data-ttu-id="2655e-126">Adressprefixet för hello undernät där paket som är avsedda att.</span><span class="sxs-lookup"><span data-stu-id="2655e-126">Address prefix for hello subnet where packets are destined to.</span></span> <span data-ttu-id="2655e-127">I vårt scenario, *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="2655e-127">For our scenario, *192.168.2.0/24*.</span></span>
    * <span data-ttu-id="2655e-128">**--Nästa hopptyp**.</span><span class="sxs-lookup"><span data-stu-id="2655e-128">**--next-hop-type**.</span></span> <span data-ttu-id="2655e-129">Typ av objekt trafik skickas till.</span><span class="sxs-lookup"><span data-stu-id="2655e-129">Type of object traffic will be sent to.</span></span> <span data-ttu-id="2655e-130">Möjliga värden är *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, eller *ingen*.</span><span class="sxs-lookup"><span data-stu-id="2655e-130">Possible values are *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, or *None*.</span></span>
    * <span data-ttu-id="2655e-131">**--Nästa-hopp-ip-adress**.</span><span class="sxs-lookup"><span data-stu-id="2655e-131">**--next-hop-ip-address**.</span></span> <span data-ttu-id="2655e-132">IP-adressen för nästa hopp.</span><span class="sxs-lookup"><span data-stu-id="2655e-132">IP address for next hop.</span></span> <span data-ttu-id="2655e-133">I vårt scenario, *192.168.0.4*.</span><span class="sxs-lookup"><span data-stu-id="2655e-133">For our scenario, *192.168.0.4*.</span></span>

3. <span data-ttu-id="2655e-134">Kör hello [az network vnet undernät uppdatering](/cli/azure/network/vnet/subnet#update) routningstabellen för kommandot tooassociate hello skapade ovan med hello **klientdel** undernät:</span><span class="sxs-lookup"><span data-stu-id="2655e-134">Run hello [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) command tooassociate hello route table created above with hello **FrontEnd** subnet:</span></span>

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name FrontEnd \
    --route-table UDR-FrontEnd
    ```

    <span data-ttu-id="2655e-135">Resultat:</span><span class="sxs-lookup"><span data-stu-id="2655e-135">Output:</span></span>

    ```json
    {
    "addressPrefix": "192.168.1.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/FrontEnd",
    "ipConfigurations": null,
    "name": "FrontEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": {
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
        "location": null,
        "name": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "routes": null,
        "subnets": null,
        "tags": null,
        "type": null
        }
    }
    ```

    <span data-ttu-id="2655e-136">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="2655e-136">Parameters:</span></span>
    
    * <span data-ttu-id="2655e-137">**--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="2655e-137">**--vnet-name**.</span></span> <span data-ttu-id="2655e-138">Namnet på hello VNet där undernätet hello finns.</span><span class="sxs-lookup"><span data-stu-id="2655e-138">Name of hello VNet where hello subnet is located.</span></span> <span data-ttu-id="2655e-139">I vårt scenario, *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="2655e-139">For our scenario, *TestVNet*.</span></span>

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="2655e-140">Skapa hello UDR för hello backend-undernät</span><span class="sxs-lookup"><span data-stu-id="2655e-140">Create hello UDR for hello back-end subnet</span></span>

<span data-ttu-id="2655e-141">toocreate hello routningstabellen och dirigera behövs för hello backend-undernät baserat på hello scenariot ovan fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2655e-141">toocreate hello route table and route needed for hello back-end subnet based on hello scenario above, complete hello following steps:</span></span>

1. <span data-ttu-id="2655e-142">Kör följande kommando toocreate hello en routningstabell för hello backend-undernät:</span><span class="sxs-lookup"><span data-stu-id="2655e-142">Run hello following command toocreate a route table for hello back-end subnet:</span></span>

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --name UDR-BackEnd \
    --location centralus
    ```

2. <span data-ttu-id="2655e-143">Kör hello efter kommandot toocreate en väg i hello flödet tabellen toosend all trafik toohello frontend undernät (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="2655e-143">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    az network route-table route create \
    --resource-group testrg \
    --name RouteToFrontEnd \
    --route-table-name UDR-BackEnd \
    --address-prefix 192.168.1.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

3. <span data-ttu-id="2655e-144">Kör hello följande kommando tooassociate hello routningstabellen med hello **BackEnd** undernät:</span><span class="sxs-lookup"><span data-stu-id="2655e-144">Run hello following command tooassociate hello route table with hello **BackEnd** subnet:</span></span>

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name BackEnd \
    --route-table UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="2655e-145">Aktivera IP-vidarebefordring på FW1</span><span class="sxs-lookup"><span data-stu-id="2655e-145">Enable IP forwarding on FW1</span></span>

<span data-ttu-id="2655e-146">tooenable IP-vidarebefordran i hello NIC som används av **FW1**, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2655e-146">tooenable IP forwarding in hello NIC used by **FW1**, complete hello following steps:</span></span>

1. <span data-ttu-id="2655e-147">Kör hello [az nätverket nic visa](/cli/azure/network/nic#show) med en JMESPATH filter toodisplay hello aktuella **aktivera IP-vidarebefordring** värde för **aktivera IP-vidarebefordring**.</span><span class="sxs-lookup"><span data-stu-id="2655e-147">Run hello [az network nic show](/cli/azure/network/nic#show) command with a JMESPATH filter toodisplay hello current **enable-ip-forwarding** value for **Enable IP forwarding**.</span></span> <span data-ttu-id="2655e-148">Det ska ställas in för*FALSKT*.</span><span class="sxs-lookup"><span data-stu-id="2655e-148">It should be set too*false*.</span></span>

    ```azurecli
    az network nic show \
    --resource-group testrg \
    --nname nicfw1 \
    --query 'enableIpForwarding' -o tsv
    ```

    <span data-ttu-id="2655e-149">Resultat:</span><span class="sxs-lookup"><span data-stu-id="2655e-149">Output:</span></span>

        false

2. <span data-ttu-id="2655e-150">Kör följande kommando tooenable IP-vidarebefordring hello:</span><span class="sxs-lookup"><span data-stu-id="2655e-150">Run hello following command tooenable IP forwarding:</span></span>

    ```azurecli
    az network nic update \
    --resource-group testrg \
    --name nicfw1 \
    --ip-forwarding true
    ```

    <span data-ttu-id="2655e-151">Du kan granska hello utdata direktuppspelat toohello konsolen eller bara testa om för specifika hello **enableIpForwarding** värde:</span><span class="sxs-lookup"><span data-stu-id="2655e-151">You can examine hello output streamed toohello console, or just retest for hello specific **enableIpForwarding** value:</span></span>

    ```azurecli
    az network nic show -g testrg -n nicfw1 --query 'enableIpForwarding' -o tsv
    ```

    <span data-ttu-id="2655e-152">Resultat:</span><span class="sxs-lookup"><span data-stu-id="2655e-152">Output:</span></span>

        true

    <span data-ttu-id="2655e-153">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="2655e-153">Parameters:</span></span>

    <span data-ttu-id="2655e-154">**– ip-vidarebefordran**: *SANT* eller *FALSKT*.</span><span class="sxs-lookup"><span data-stu-id="2655e-154">**--ip-forwarding**: *true* or *false*.</span></span>

