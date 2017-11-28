---
title: "aaaNetworking för skalningsuppsättningar i virtuella Azure-datorn | Microsoft Docs"
description: "Konfigurationsnätverksegenskaper för skalningsuppsättningar för virtuella Azure-datorer."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: guybo
ms.openlocfilehash: ef3f0cfe648d2195c051a73987e654f0e15d13bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="networking-for-azure-virtual-machine-scale-sets"></a><span data-ttu-id="7787d-103">Nätverk för skalningsuppsättningar för virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="7787d-103">Networking for Azure virtual machine scale sets</span></span>

<span data-ttu-id="7787d-104">När du distribuerar en virtuell dator i Azure-skala anges via hello portal vissa egenskaper för nätverk som är standard, till exempel en Azure belastningsutjämnare med inkommande NAT-regler.</span><span class="sxs-lookup"><span data-stu-id="7787d-104">When you deploy an Azure virtual machine scale set through hello portal, certain network properties are defaulted, for example an Azure Load Balancer with inbound NAT rules.</span></span> <span data-ttu-id="7787d-105">Den här artikeln beskriver hur toouse vissa hello mer avancerade funktioner som du kan konfigurera med en skala anger.</span><span class="sxs-lookup"><span data-stu-id="7787d-105">This article describes how toouse some of hello more advanced networking features that you can configure with scale sets.</span></span>

<span data-ttu-id="7787d-106">Du kan konfigurera alla hello-funktioner som beskrivs i den här artikeln med hjälp av Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="7787d-106">You can configure all of hello features covered in this article using Azure Resource Manager templates.</span></span> <span data-ttu-id="7787d-107">Azure CLI- och PowerShell-exempel ingår också i de valda funktionerna.</span><span class="sxs-lookup"><span data-stu-id="7787d-107">Azure CLI and PowerShell examples are also included for selected features.</span></span> <span data-ttu-id="7787d-108">Använd CLI 2.10 och PowerShell 4.2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7787d-108">Use CLI 2.10, and PowerShell 4.2.0 or later.</span></span>

## <a name="accelerated-networking"></a><span data-ttu-id="7787d-109">Accelererat nätverk</span><span class="sxs-lookup"><span data-stu-id="7787d-109">Accelerated Networking</span></span>
<span data-ttu-id="7787d-110">Azure [snabbare nätverk](../virtual-network/virtual-network-create-vm-accelerated-networking.md) förbättrar nätverkets prestanda genom att aktivera single-root I/O virtualization (SR-IOV) tooa virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7787d-110">Azure [Accelerated Networking](../virtual-network/virtual-network-create-vm-accelerated-networking.md) improves  network performance by enabling single root I/O virtualization (SR-IOV) tooa virtual machine.</span></span> <span data-ttu-id="7787d-111">toouse snabbare nätverk med skaluppsättningar genom att ange enableAcceleratedNetworking för**SANT** i inställningarna för din skaluppsättning networkInterfaceConfigurations.</span><span class="sxs-lookup"><span data-stu-id="7787d-111">toouse accelerated networking with scale sets, set enableAcceleratedNetworking too**true** in your scale set's networkInterfaceConfigurations settings.</span></span> <span data-ttu-id="7787d-112">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7787d-112">For example:</span></span>
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
      "name": "niconfig1",
      "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
          ...
        ]
      }
    }
   ]
}
```

## <a name="create-a-scale-set-that-references-an-existing-azure-load-balancer"></a><span data-ttu-id="7787d-113">Skapa en skalningsuppsättning som refererar till en befintlig Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="7787d-113">Create a scale set that references an existing Azure Load Balancer</span></span>
<span data-ttu-id="7787d-114">När en skaluppsättning skapas med hjälp av hello Azure-portalen, skapas en ny belastningsutjämnare för de flesta konfigurationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="7787d-114">When a scale set is created using hello Azure portal, a new load balancer is created for most configuration options.</span></span> <span data-ttu-id="7787d-115">Om du skapar en skalningsuppsättning som måste tooreference en befintlig belastningsutjämnare, kan du göra detta med hjälp av CLI.</span><span class="sxs-lookup"><span data-stu-id="7787d-115">If you create a scale set, which needs tooreference an existing load balancer, you can do this using CLI.</span></span> <span data-ttu-id="7787d-116">Följande exempelskript hello skapar en belastningsutjämnare och skapar sedan en skalningsuppsättning som refererar till den:</span><span class="sxs-lookup"><span data-stu-id="7787d-116">hello following example script creates a load balancer and then creates a scale set, which references it:</span></span>
```bash
az network lb create -g lbtest -n mylb --vnet-name myvnet --subnet mysubnet --public-ip-address-allocation Static --backend-pool-name mybackendpool

az vmss create -g lbtest -n myvmss --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username negat --ssh-key-value /home/myuser/.ssh/id_rsa.pub --upgrade-policy-mode Automatic --instance-count 3 --vnet-name myvnet --subnet mysubnet --lb mylb --backend-pool-name mybackendpool

```

## <a name="configurable-dns-settings"></a><span data-ttu-id="7787d-117">Konfigurera DNS-inställningar</span><span class="sxs-lookup"><span data-stu-id="7787d-117">Configurable DNS Settings</span></span>
<span data-ttu-id="7787d-118">Som standard utför skalningsuppsättningar på hello specifika DNS-inställningarna för hello VNET och undernät som de skapades i.</span><span class="sxs-lookup"><span data-stu-id="7787d-118">By default, scale sets take on hello specific DNS settings of hello VNET and subnet they were created in.</span></span> <span data-ttu-id="7787d-119">Du kan dock konfigurera hello DNS-inställningarna för en skala som anges direkt.</span><span class="sxs-lookup"><span data-stu-id="7787d-119">You can however, configure hello DNS settings for a scale set directly.</span></span>
~
### <a name="creating-a-scale-set-with-configurable-dns-servers"></a><span data-ttu-id="7787d-120">Skapa en skalningsuppsättning med konfigurerbara DNS-servrar</span><span class="sxs-lookup"><span data-stu-id="7787d-120">Creating a scale set with configurable DNS servers</span></span>
<span data-ttu-id="7787d-121">toocreate en skala som anges med en anpassad DNS-konfiguration med hjälp av CLI 2.0, lägga till hello **--dns-servrar** argumentet toohello **vmss skapa** följt av utrymme avgränsade ip-adresser.</span><span class="sxs-lookup"><span data-stu-id="7787d-121">toocreate a scale set with a custom DNS configuration using CLI 2.0, add hello **--dns-servers** argument toohello **vmss create** command, followed by space separated server ip addresses.</span></span> <span data-ttu-id="7787d-122">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7787d-122">For example:</span></span>
```bash
--dns-servers 10.0.0.6 10.0.0.5
```
<span data-ttu-id="7787d-123">tooconfigure anpassade DNS-servrar i en Azure-mall, lägga till en dnsSettings egenskapen toohello skala networkInterfaceConfigurations avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7787d-123">tooconfigure custom DNS servers in an Azure template, add a dnsSettings property toohello scale set networkInterfaceConfigurations section.</span></span> <span data-ttu-id="7787d-124">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7787d-124">For example:</span></span>
```json
"dnsSettings":{
    "dnsServers":["10.0.0.6", "10.0.0.5"]
}
```

### <a name="creating-a-scale-set-with-configurable-virtual-machine-domain-names"></a><span data-ttu-id="7787d-125">Skapa en skalningsuppsättning med konfigurerbara domännamn för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="7787d-125">Creating a scale set with configurable virtual machine domain names</span></span>
<span data-ttu-id="7787d-126">toocreate skaluppsättning med en anpassad DNS-namnet för virtuella datorer med hjälp av CLI 2.0, lägga till hello **--vm domännamn** argumentet toohello **vmss skapa** följt av en sträng som representerar hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="7787d-126">toocreate a scale set with a custom DNS name for virtual machines using CLI 2.0, add hello **--vm-domain-name** argument toohello **vmss create** command, followed by a string representing hello domain name.</span></span>

<span data-ttu-id="7787d-127">tooset hello-domännamnet i en Azure-mall, lägga till en **dnsSettings** skaluppsättning för egenskapen toohello **networkInterfaceConfigurations** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7787d-127">tooset hello domain name in an Azure template, add a **dnsSettings** property toohello scale set **networkInterfaceConfigurations** section.</span></span> <span data-ttu-id="7787d-128">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7787d-128">For example:</span></span>

```json
"networkProfile": {
  "networkInterfaceConfigurations": [
    {
    "name": "nic1",
    "properties": {
      "primary": "true",
      "ipConfigurations": [
      {
        "name": "ip1",
        "properties": {
          "subnet": {
            "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
          },
          "publicIPAddressconfiguration": {
            "name": "publicip",
            "properties": {
            "idleTimeoutInMinutes": 10,
              "dnsSettings": {
                "domainNameLabel": "[parameters('vmssDnsName')]"
              }
            }
          }
        }
      }
    ]
    }
}
```

<span data-ttu-id="7787d-129">hello utdata för en enskild virtuell dator dns-namn är i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="7787d-129">hello output, for an individual virtual machine dns name would be in hello following form:</span></span> 
```
<vm><vmindex>.<specifiedVmssDomainNameLabel>
```

## <a name="public-ipv4-per-virtual-machine"></a><span data-ttu-id="7787d-130">Offentlig IPv4 per virtuell dator</span><span class="sxs-lookup"><span data-stu-id="7787d-130">Public IPv4 per virtual machine</span></span>
<span data-ttu-id="7787d-131">I allmänhet kräver inte skalningsuppsättningar för virtuella Azure-datorer sina egna offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="7787d-131">In general, Azure scale set virtual machines do not require their own public IP addresses.</span></span> <span data-ttu-id="7787d-132">För de flesta fall är det mer ekonomiska och säker tooassociate en offentlig IP-adress tooa belastningen belastningsutjämnare eller tooan enskild virtuell dator (även kallat en jumpbox) som sedan vidarebefordrar inkommande anslutningar tooscale uppsättning virtuella datorer efter behov (till exempel via inkommande NAT-regler).</span><span class="sxs-lookup"><span data-stu-id="7787d-132">For most scenarios, it is more economical and secure tooassociate a public IP address tooa load balancer or tooan individual virtual machine (aka a jumpbox), which then routes incoming connections tooscale set virtual machines as needed (for example, through inbound NAT rules).</span></span>

<span data-ttu-id="7787d-133">Dock vissa scenarier kräver skaluppsättning för virtuella datorer toohave sina egna offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="7787d-133">However, some scenarios do require scale set virtual machines toohave their own public IP addresses.</span></span> <span data-ttu-id="7787d-134">Ett exempel spel, där en konsol måste toomake en direktanslutning tooa moln virtuella datorn, vilket gör spelet fysik bearbetning.</span><span class="sxs-lookup"><span data-stu-id="7787d-134">An example is gaming, where a console needs toomake a direct connection tooa cloud virtual machine, which is doing game physics processing.</span></span> <span data-ttu-id="7787d-135">Ett annat exempel är där virtuella datorer måste toomake externa anslutningar tooone tvärs över regioner i en distribuerad databas.</span><span class="sxs-lookup"><span data-stu-id="7787d-135">Another example is where virtual machines need toomake external connections tooone another across regions in a distributed database.</span></span>

### <a name="creating-a-scale-set-with-public-ip-per-virtual-machine"></a><span data-ttu-id="7787d-136">Skapa en skalningsuppsättning med en offentlig IP per virtuell dator</span><span class="sxs-lookup"><span data-stu-id="7787d-136">Creating a scale set with public IP per virtual machine</span></span>
<span data-ttu-id="7787d-137">toocreate en skalningsuppsättning som tilldelar en offentlig IP-adress tooeach virtuell dator med CLI 2.0, lägga till hello **--offentlig ip per vm** parametern toohello **vmss skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="7787d-137">toocreate a scale set that assigns a public IP address tooeach virtual machine with CLI 2.0, add hello **--public-ip-per-vm** parameter toohello **vmss create** command.</span></span> 

<span data-ttu-id="7787d-138">toocreate en skala som anges med en Azure-mall, kontrollera hello API-versionen av hello Microsoft.Compute/virtualMachineScaleSets resursen är minst **2017-03-30**, och Lägg till en **publicIpAddressConfiguration**JSON egenskapen toohello skaluppsättning ipConfigurations avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7787d-138">toocreate a scale set using an Azure template, make sure hello API version of hello Microsoft.Compute/virtualMachineScaleSets resource is at least **2017-03-30**, and add a **publicIpAddressConfiguration** JSON property toohello scale set ipConfigurations section.</span></span> <span data-ttu-id="7787d-139">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7787d-139">For example:</span></span>

```json
"publicIpAddressConfiguration": {
    "name": "pub1",
    "properties": {
      "idleTimeoutInMinutes": 15
    }
}
```
<span data-ttu-id="7787d-140">Exempelmall: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)</span><span class="sxs-lookup"><span data-stu-id="7787d-140">Example template: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)</span></span>

### <a name="querying-hello-public-ip-addresses-of-hello-virtual-machines-in-a-scale-set"></a><span data-ttu-id="7787d-141">Frågar hello offentliga IP-adresser för hello virtuella datorer på en skala ange</span><span class="sxs-lookup"><span data-stu-id="7787d-141">Querying hello public IP addresses of hello virtual machines in a scale set</span></span>
<span data-ttu-id="7787d-142">toolist hello offentliga IP-adresser tilldelas tooscale uppsättning virtuella datorer med hjälp av CLI 2.0 använder hello **az vmss lista-instans-offentliga-IP-adresser** kommando.</span><span class="sxs-lookup"><span data-stu-id="7787d-142">toolist hello public IP addresses assigned tooscale set virtual machines using CLI 2.0, use hello **az vmss list-instance-public-ips** command.</span></span>

<span data-ttu-id="7787d-143">toolist skaluppsättning för den offentliga IP-adresser med hjälp av PowerShell använder hello _Get-AzureRmPublicIpAddress_ kommando.</span><span class="sxs-lookup"><span data-stu-id="7787d-143">toolist scale set public IP addresses using PowerShell, use hello _Get-AzureRmPublicIpAddress_ command.</span></span> <span data-ttu-id="7787d-144">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7787d-144">For example:</span></span>
```PowerShell
PS C:\> Get-AzureRmPublicIpAddress -ResourceGroupName myrg -VirtualMachineScaleSetName myvmss
```

<span data-ttu-id="7787d-145">Du kan också fråga hello offentliga IP-adresser genom att referera hello resurs-id för hello offentliga IP-adresskonfigurationen direkt.</span><span class="sxs-lookup"><span data-stu-id="7787d-145">You can also query hello public IP addresses by referencing hello resource id of hello public IP address configuration directly.</span></span> <span data-ttu-id="7787d-146">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7787d-146">For example:</span></span>
```PowerShell
PS C:\> Get-AzureRmPublicIpAddress -ResourceGroupName myrg -Name myvmsspip
```

<span data-ttu-id="7787d-147">tooquery hello offentliga IP-adresser tilldelas tooscale uppsättning virtuella datorer med hello [resursutforskaren Azure](https://resources.azure.com), eller hello Azure REST-API med version **2017-03-30** eller högre.</span><span class="sxs-lookup"><span data-stu-id="7787d-147">tooquery hello public IP addresses assigned tooscale set virtual machines using hello [Azure Resource Explorer](https://resources.azure.com), or hello Azure REST API with version **2017-03-30** or higher.</span></span>

<span data-ttu-id="7787d-148">tooview offentliga IP-adresser för en skala som anges med hello resursutforskaren, titta på hello **publicipaddresses** avsnitt under din skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="7787d-148">tooview public IP addresses for a scale set using hello Resource Explorer, look at hello **publicipaddresses** section under your scale set.</span></span> <span data-ttu-id="7787d-149">Till exempel: https://resources.azure.com/subscriptions/_your_sub_id_/resourceGroups/_your_rg_/providers/Microsoft.Compute/virtualMachineScaleSets/_your_vmss_/publicipaddresses</span><span class="sxs-lookup"><span data-stu-id="7787d-149">For example: https://resources.azure.com/subscriptions/_your_sub_id_/resourceGroups/_your_rg_/providers/Microsoft.Compute/virtualMachineScaleSets/_your_vmss_/publicipaddresses</span></span>

```
GET https://management.azure.com/subscriptions/{your sub ID}/resourceGroups/{RG name}/providers/Microsoft.Compute/virtualMachineScaleSets/{scale set name}/publicipaddresses?api-version=2017-03-30
```

<span data-ttu-id="7787d-150">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="7787d-150">Example output:</span></span>
```json
{
  "value": [
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/pipvmss/virtualMachines/0/networkInterfaces/pipvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"a64060d5-4dea-4379-a11d-b23cd49a3c8d\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "ee8cb20f-af8e-4cd6-892f-441ae2bf701f",
        "ipAddress": "13.84.190.11",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/0/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    },
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"5f6ff30c-a24c-4818-883c-61ebd5f9eee8\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "036ce266-403f-41bd-8578-d446d7397c2f",
        "ipAddress": "13.84.159.176",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    }
```

## <a name="multiple-ip-addresses-per-nic"></a><span data-ttu-id="7787d-151">Flera IP-adresser per nätverkskort</span><span class="sxs-lookup"><span data-stu-id="7787d-151">Multiple IP addresses per NIC</span></span>
<span data-ttu-id="7787d-152">Varje nätverkskort anslutet tooa virtuell dator i en skaluppsättning kan ha en eller flera IP-konfigurationer som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="7787d-152">Every NIC attached tooa VM in a scale set can have one or more IP configurations associated with it.</span></span> <span data-ttu-id="7787d-153">Varje konfiguration tilldelas en privat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="7787d-153">Each configuration is assigned one private IP address.</span></span> <span data-ttu-id="7787d-154">Varje konfiguration kan också ha en associerad offentlig IP-adressresurs.</span><span class="sxs-lookup"><span data-stu-id="7787d-154">Each configuration may also have one public IP address resource associated with it.</span></span> <span data-ttu-id="7787d-155">toounderstand hur många IP-adresser kan tilldelas tooa NIC, och hur många offentliga IP-adresser som du kan använda i en Azure-prenumeration finns för[Azure gränser](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).</span><span class="sxs-lookup"><span data-stu-id="7787d-155">toounderstand how many IP addresses can be assigned tooa NIC, and how many public IP addresses you can use in an Azure subscription, refer too[Azure limits](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).</span></span>

## <a name="multiple-nics-per-virtual-machine"></a><span data-ttu-id="7787d-156">Flera nätverkskort per virtuell dator</span><span class="sxs-lookup"><span data-stu-id="7787d-156">Multiple NICs per virtual machine</span></span>
<span data-ttu-id="7787d-157">Du kan ha upp too8 nätverkskort per virtuell dator, beroende på storleken på datorn.</span><span class="sxs-lookup"><span data-stu-id="7787d-157">You can have up too8 NICs per virtual machine, depending on machine size.</span></span> <span data-ttu-id="7787d-158">hello maximalt antal nätverkskort per dator är tillgängliga i hello [VM storlek artikel](../virtual-machines/windows/sizes.md).</span><span class="sxs-lookup"><span data-stu-id="7787d-158">hello maximum number of NICs per machine is available in hello [VM size article](../virtual-machines/windows/sizes.md).</span></span> <span data-ttu-id="7787d-159">hello följande exempel är en skaluppsättning för nätverksprofil visar flera poster för NIC och flera offentliga IP-adresser per virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="7787d-159">hello following example is a scale set network profile showing multiple NIC entries, and multiple public IPs per virtual machine:</span></span>
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
        "name": "nic1",
        "properties": {
            "primary": "true",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        },
        {
        "name": "nic2",
        "properties": {
            "primary": "false",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        }
    ]
}
```

## <a name="nsg-per-scale-set"></a><span data-ttu-id="7787d-160">Nätverkssäkerhetsgrupp per skalningsuppsättning</span><span class="sxs-lookup"><span data-stu-id="7787d-160">NSG per scale set</span></span>
<span data-ttu-id="7787d-161">Nätverkssäkerhetsgrupper kan tillämpas direkt tooa skaluppsättning genom att lägga till en referens toohello network interface konfigurationsavsnittet hello skalan ange egenskaper för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7787d-161">Network Security Groups can be applied directly tooa scale set, by adding a reference toohello network interface configuration section of hello scale set virtual machine properties.</span></span>

<span data-ttu-id="7787d-162">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7787d-162">For example:</span></span> 
```
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a><span data-ttu-id="7787d-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7787d-163">Next steps</span></span>
<span data-ttu-id="7787d-164">Mer information om virtuella Azure-nätverk finns för[denna dokumentation](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7787d-164">For more information about Azure virtual networks, refer too[this documentation](../virtual-network/virtual-networks-overview.md).</span></span>
