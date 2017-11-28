---
title: "aaaConfigure privata IP-adresser för virtuella datorer – Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur tooconfigure privata IP-adresser för virtuella datorer med hello Azure-kommandoradsgränssnittet (CLI) 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 40b03a1a-ea00-454c-b716-7574cea49ac0
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0e278e6ac63c0cda061cf70ab0edfaff5491c03b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-cli-20"></a><span data-ttu-id="c75c9-103">Konfigurera privat IP-adresser för en virtuell dator med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c75c9-103">Configure private IP addresses for a virtual machine using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="c75c9-104">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="c75c9-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="c75c9-105">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="c75c9-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="c75c9-106">[Azure CLI 1.0](virtual-networks-static-private-ip-cli-nodejs.md) – våra CLI för hello klassisk och resurs management distributionsmodeller</span><span class="sxs-lookup"><span data-stu-id="c75c9-106">[Azure CLI 1.0](virtual-networks-static-private-ip-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="c75c9-107">[Azure CLI 2.0](#specify-a-static-private-ip-address-when-creating-a-vm) -vår nästa generations CLI för hello management resursdistributionsmodell (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="c75c9-107">[Azure CLI 2.0](#specify-a-static-private-ip-address-when-creating-a-vm) - our next generation CLI for hello resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="c75c9-108">Den här artikeln beskriver hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="c75c9-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="c75c9-109">Du kan också [hantera statisk privat IP-adress i hello klassiska distributionsmodellen](virtual-networks-static-private-ip-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c75c9-109">You can also [manage static private IP address in hello classic deployment model](virtual-networks-static-private-ip-classic-cli.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

> [!NOTE]
> <span data-ttu-id="c75c9-110">hello Azure CLI 2.0 exempelkommandon nedan förväntar sig en enkel miljö som redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="c75c9-110">hello sample Azure CLI 2.0 commands below expect a simple environment already created.</span></span> <span data-ttu-id="c75c9-111">Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö som beskrivs i [skapa ett vnet](virtual-networks-create-vnet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c75c9-111">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-arm-cli.md).</span></span>

## <a name="specify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="c75c9-112">Ange en statisk privat IP-adress när du skapar en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c75c9-112">Specify a static private IP address when creating a VM</span></span>

<span data-ttu-id="c75c9-113">toocreate en virtuell dator med namnet *DNS01* i hello *klientdel* undernätet i ett VNet med namnet *TestVNet* med en statisk privat IP-adress för *192.168.1.101*, Följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="c75c9-113">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow hello steps below:</span></span>

1. <span data-ttu-id="c75c9-114">Om du inte har gjort det ännu, installerar och konfigurerar hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="c75c9-114">If you haven't yet, install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span> 

2. <span data-ttu-id="c75c9-115">Skapa en offentlig IP-adress för hello VM med hello [az nätverket offentliga IP-skapa](/cli/azure/network/public-ip#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="c75c9-115">Create a public IP for hello VM with hello [az network public-ip create](/cli/azure/network/public-ip#create) command.</span></span> <span data-ttu-id="c75c9-116">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="c75c9-116">hello list shown after hello output explains hello parameters used.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c75c9-117">Om du vill eller behöver toouse olika värden för att argumenten i den här och efterföljande steg beroende på din miljö.</span><span class="sxs-lookup"><span data-stu-id="c75c9-117">You may want or need toouse different values for your arguments in this and subsequent steps, depending upon your environment.</span></span>
   
    ```azurecli
    az network public-ip create \
    --name TestPIP \
    --resource-group TestRG \
    --location centralus \
    --allocation-method Static
    ```

    <span data-ttu-id="c75c9-118">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="c75c9-118">Expected output:</span></span>
   
   ```json
   {
        "publicIp": {
            "idleTimeoutInMinutes": 4,
            "ipAddress": "52.176.43.167",
            "provisioningState": "Succeeded",
            "publicIPAllocationMethod": "Static",
            "resourceGuid": "79e8baa3-33ce-466a-846c-37af3c721ce1"
        }
    }
    ```

   * <span data-ttu-id="c75c9-119">`--resource-group`: Namnet på resursgruppen för hello toocreate hello offentliga IP.</span><span class="sxs-lookup"><span data-stu-id="c75c9-119">`--resource-group`: Name of hello resource group in which toocreate hello public IP.</span></span>
   * <span data-ttu-id="c75c9-120">`--name`: Namnet på hello offentlig IP.</span><span class="sxs-lookup"><span data-stu-id="c75c9-120">`--name`: Name of hello public IP.</span></span>
   * <span data-ttu-id="c75c9-121">`--location`: Azure-region i vilken toocreate hello offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="c75c9-121">`--location`: Azure region in which toocreate hello public IP.</span></span>

3. <span data-ttu-id="c75c9-122">Kör hello [az nätverket nic skapa](/cli/azure/network/nic#create) kommandot toocreate ett nätverkskort med en statisk privat IP.</span><span class="sxs-lookup"><span data-stu-id="c75c9-122">Run hello [az network nic create](/cli/azure/network/nic#create) command toocreate a NIC with a static private IP.</span></span> <span data-ttu-id="c75c9-123">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="c75c9-123">hello list shown after hello output explains hello parameters used.</span></span> 
   
    ```azurecli
    az network nic create \
    --resource-group TestRG \
    --name TestNIC \
    --location centralus \
    --subnet FrontEnd \
    --private-ip-address 192.168.1.101 \
    --vnet-name TestVNet
    ```

    <span data-ttu-id="c75c9-124">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="c75c9-124">Expected output:</span></span>
   
    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.101",
                "privateIPAllocationMethod": "Static",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>"
        }
    }
    ```
    
    <span data-ttu-id="c75c9-125">Parametrar:</span><span class="sxs-lookup"><span data-stu-id="c75c9-125">Parameters:</span></span>

    * <span data-ttu-id="c75c9-126">`--private-ip-address`: Statisk privat IP-adress för hello NIC.</span><span class="sxs-lookup"><span data-stu-id="c75c9-126">`--private-ip-address`: Static private IP address for hello NIC.</span></span>
    * <span data-ttu-id="c75c9-127">`--vnet-name`: Namnet på hello virtuella nätverk i vilka toocreate hello NIC.</span><span class="sxs-lookup"><span data-stu-id="c75c9-127">`--vnet-name`: Name of hello VNet in wihch toocreate hello NIC.</span></span>
    * <span data-ttu-id="c75c9-128">`--subnet`: Namnet på hello undernät i vilka toocreate hello NIC.</span><span class="sxs-lookup"><span data-stu-id="c75c9-128">`--subnet`: Name of hello subnet in which toocreate hello NIC.</span></span>

4. <span data-ttu-id="c75c9-129">Kör hello [azure vm skapa](/cli/azure/vm/nic#create) kommandot toocreate hello skapas den virtuella datorn med hjälp av hello offentlig IP-adress och NIC ovan.</span><span class="sxs-lookup"><span data-stu-id="c75c9-129">Run hello [azure vm create](/cli/azure/vm/nic#create) command toocreate hello VM using hello public IP and NIC created above.</span></span> <span data-ttu-id="c75c9-130">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="c75c9-130">hello list shown after hello output explains hello parameters used.</span></span>
   
    ```azurecli
    az vm create \
    --resource-group TestRG \
    --name DNS01 \
    --location centralus \
    --image Debian \
    --admin-username adminuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics TestNIC
    ```

    <span data-ttu-id="c75c9-131">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="c75c9-131">Expected output:</span></span>
   
    ```json
    {
        "fqdns": "",
        "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01",
        "location": "centralus",
        "macAddress": "00-0D-3A-92-C1-66",
        "powerState": "VM running",
        "privateIpAddress": "192.168.1.101",
        "publicIpAddress": "",
        "resourceGroup": "TestRG"
    }
    ```
   
   <span data-ttu-id="c75c9-132">Andra parametrar än grundläggande hello [az vm skapa](/cli/azure/vm#create) parametrar.</span><span class="sxs-lookup"><span data-stu-id="c75c9-132">Parameters other than hello basic [az vm create](/cli/azure/vm#create) parameters.</span></span>

   * <span data-ttu-id="c75c9-133">`--nics`: Namnet på hello NIC toowhich hello VM är ansluten.</span><span class="sxs-lookup"><span data-stu-id="c75c9-133">`--nics`: Name of hello NIC toowhich hello VM is attached.</span></span>
   

## <a name="retrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="c75c9-134">Hämta statisk privat IP-adressinformation för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c75c9-134">Retrieve static private IP address information for a VM</span></span>

<span data-ttu-id="c75c9-135">tooview hello statisk privat IP-adress som du har skapat kör hello följande Azure CLI-kommando och Observera hello värden för *privat IP allokeringsenhets-metoden* och *privata IP-adressen*:</span><span class="sxs-lookup"><span data-stu-id="c75c9-135">tooview hello static private IP address that you created, run hello following Azure CLI command and observe hello values for *Private IP alloc-method* and *Private IP address*:</span></span>

```azurecli
az vm show -g TestRG -n DNS01 --show-details --query 'privateIps'
```

<span data-ttu-id="c75c9-136">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="c75c9-136">Expected output:</span></span>

```json
"192.168.1.101"
```

<span data-ttu-id="c75c9-137">toodisplay hello specifika IP-information för hello nätverkskort för den virtuella datorn, fråga hello NIC specifikt:</span><span class="sxs-lookup"><span data-stu-id="c75c9-137">toodisplay hello specific IP information of hello NIC for that VM, query hello NIC specifically:</span></span>

```azurecli
az network nic show \
-g testrg \
-n testnic \
--query 'ipConfigurations[0].{PrivateAddress:privateIpAddress,IPVer:privateIpAddressVersion,IpAllocMethod:p
rivateIpAllocationMethod,PublicAddress:publicIpAddress}'
```

<span data-ttu-id="c75c9-138">hello-utdata är ungefär:</span><span class="sxs-lookup"><span data-stu-id="c75c9-138">hello output is something like:</span></span>

```json
{
    "IPVer": "IPv4",
    "IpAllocMethod": "Static",
    "PrivateAddress": "192.168.1.101",
    "PublicAddress": null
}
```

## <a name="remove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="c75c9-139">Ta bort en statisk privat IP-adress från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c75c9-139">Remove a static private IP address from a VM</span></span>

<span data-ttu-id="c75c9-140">Du kan inte ta bort en statisk privat IP-adress från ett nätverkskort i Azure CLI för resource manager distributioner.</span><span class="sxs-lookup"><span data-stu-id="c75c9-140">You cannot remove a static private IP address from a NIC in Azure CLI for resource manager deployments.</span></span> <span data-ttu-id="c75c9-141">Du måste:</span><span class="sxs-lookup"><span data-stu-id="c75c9-141">You must:</span></span>
- <span data-ttu-id="c75c9-142">Skapa ett nytt nätverkskort som använder en dynamisk IP-adress</span><span class="sxs-lookup"><span data-stu-id="c75c9-142">Create a new NIC that uses a dynamic IP</span></span>
- <span data-ttu-id="c75c9-143">Ange hello NIC på hello VM hello nyskapad NIC.</span><span class="sxs-lookup"><span data-stu-id="c75c9-143">Set hello NIC on hello VM do hello newly created NIC.</span></span> 

<span data-ttu-id="c75c9-144">toochange hello NIC för hello VM används i hello kommandona ovan, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="c75c9-144">toochange hello NIC for hello VM used in hello commands above, follow hello steps below.</span></span>

1. <span data-ttu-id="c75c9-145">Kör hello **azure-nätverk nic skapa** kommandot toocreate ett nytt nätverkskort med hjälp av dynamisk IP-adressallokering med en ny IP-adress.</span><span class="sxs-lookup"><span data-stu-id="c75c9-145">Run hello **azure network nic create** command toocreate a new NIC using dynamic IP allocation with a new IP address.</span></span> <span data-ttu-id="c75c9-146">Observera att eftersom ingen IP-adress har angetts är hello allokeringsmetod **dynamiska**.</span><span class="sxs-lookup"><span data-stu-id="c75c9-146">Note that because no IP address is specified, hello allocation method is **Dynamic**.</span></span>

    ```azurecli
    az network nic create     \
    --resource-group TestRG     \
    --name TestNIC2     \
    --location centralus     \
    --subnet FrontEnd    \
    --vnet-name TestVNet
    ```        
   
    <span data-ttu-id="c75c9-147">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="c75c9-147">Expected output:</span></span>

    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.4",
                "privateIPAllocationMethod": "Dynamic",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "0808a61c-476f-4d08-98ee-0fa83671b010"
        }
    }
    ```

2. <span data-ttu-id="c75c9-148">Kör hello **azure vm set** kommandot toochange hello NIC som används av hello VM.</span><span class="sxs-lookup"><span data-stu-id="c75c9-148">Run hello **azure vm set** command toochange hello NIC used by hello VM.</span></span>
   
    ```azurecli
    azure vm set -g TestRG -n DNS01 -N TestNIC2
    ```

    <span data-ttu-id="c75c9-149">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="c75c9-149">Expected output:</span></span>
   
    ```json
    [
        {
            "id": "/subscriptions/0e220bf6-5caa-4e9f-8383-51f16b6c109f/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC3",
            "primary": true,
            "resourceGroup": "TestRG"
        }
    ]
    ```

    > [!NOTE]
    > <span data-ttu-id="c75c9-150">Om hello VM är tillräckligt stor toohave mer än ett nätverkskort, kör hello **ta bort azure-nätverk nic** kommandot toodelete hello gamla NIC.</span><span class="sxs-lookup"><span data-stu-id="c75c9-150">If hello VM is large enough toohave more than one NIC, run hello **azure network nic delete** command toodelete hello old NIC.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="c75c9-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c75c9-151">Next steps</span></span>
* <span data-ttu-id="c75c9-152">Lär dig mer om [reserverade offentliga IP-Adressen](virtual-networks-reserved-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="c75c9-152">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="c75c9-153">Lär dig mer om [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="c75c9-153">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="c75c9-154">Kontakta hello [reserverade IP-REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="c75c9-154">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

