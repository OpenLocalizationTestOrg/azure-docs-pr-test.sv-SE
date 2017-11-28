---
title: "aaaAccess och säkerhet i Azure-mallar för virtuella Linux-datorer | Microsoft Docs"
description: "Virtuella Azure-datorn DotNet grundläggande självstudierna"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 07e47189-680e-4102-a8d4-5a8eb9c00213
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 88fedc8287c1f8ab8397a03ddefe1e60a686815e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-and-security-in-azure-resource-manager-templates-for-linux-vms"></a><span data-ttu-id="e5bec-103">Åtkomst och säkerhet i Azure Resource Manager-mallar för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="e5bec-103">Access and security in Azure Resource Manager templates for Linux VMs</span></span>

<span data-ttu-id="e5bec-104">Program finns i Azure förmodligen behöver toobe åtkomst via hello internet eller ett VPN / Expressroute-anslutningen med Azure.</span><span class="sxs-lookup"><span data-stu-id="e5bec-104">Applications hosted in Azure likely need toobe access over hello internet or a VPN / Express Route connection with Azure.</span></span> <span data-ttu-id="e5bec-105">Med hello musik Store programmet exempel hello webbplats görs tillgängliga på hello internet med en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e5bec-105">With hello Music Store application sample, hello web site is made available on hello internet with a public IP address.</span></span> <span data-ttu-id="e5bec-106">Med åtkomst upprätta ska anslutningar toohello program- och toohello virtuella datorresurser själva skyddas.</span><span class="sxs-lookup"><span data-stu-id="e5bec-106">With access established, connections toohello application and access toohello virtual machine resources themselves should be secured.</span></span> <span data-ttu-id="e5bec-107">Den här åtkomstsäkerhet ingår i en Nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="e5bec-107">This access security is provided with a Network Security Group.</span></span> 

<span data-ttu-id="e5bec-108">Det här dokumentet beskriver hur hello musik Store-programmet skyddas i Azure Resource Manager för hello exempelmall.</span><span class="sxs-lookup"><span data-stu-id="e5bec-108">This document details how hello Music Store application is secured in hello sample Azure Resource Manager template.</span></span> <span data-ttu-id="e5bec-109">Alla beroenden och unika konfigurationer är markerade.</span><span class="sxs-lookup"><span data-stu-id="e5bec-109">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="e5bec-110">Distribuera en instans av hello lösning tooyour Azure-prenumeration och fungerar tillsammans med hello Azure Resource Manager-mall för hello bästa möjliga resultat.</span><span class="sxs-lookup"><span data-stu-id="e5bec-110">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="e5bec-111">hello fullständig mallen hittar du här – [musik Store distributionen på Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="e5bec-111">hello complete template can be found here – [Music Store Deployment on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span> 

## <a name="public-ip-address"></a><span data-ttu-id="e5bec-112">Offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="e5bec-112">Public IP Address</span></span>
<span data-ttu-id="e5bec-113">tooprovide offentlig åtkomst tooan Azure-resurs, en offentlig IP-adressresurs kan användas.</span><span class="sxs-lookup"><span data-stu-id="e5bec-113">tooprovide public access tooan Azure resource, a public IP address resource can be used.</span></span> <span data-ttu-id="e5bec-114">Offentlig IP-adress kan konfigureras med en statisk eller dynamisk IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e5bec-114">Public IP address can be configured with a static or dynamic IP address.</span></span> <span data-ttu-id="e5bec-115">Om en dynamisk adress används och hello virtuella datorn stoppas och frigöra, hello adresser tas bort.</span><span class="sxs-lookup"><span data-stu-id="e5bec-115">If a dynamic address is used, and hello virtual machine is stopped and deallocated, hello addresses is removed.</span></span> <span data-ttu-id="e5bec-116">När hello dator startas igen, kan den tilldelas en annan offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e5bec-116">When hello machine is started again, it may be assigned a different public IP address.</span></span> <span data-ttu-id="e5bec-117">tooprevent en IP-adress från ändrar, en reserverad IP-adress kan användas.</span><span class="sxs-lookup"><span data-stu-id="e5bec-117">tooprevent an IP address from changing, a reserved IP address can be used.</span></span> 

<span data-ttu-id="e5bec-118">Tooan Azure Resource Manager-mallen med hjälp av hello Visual Studio guiden Lägg till ny resurs, kan läggas till en offentlig IP-adress eller genom att infoga giltig JSON i en mall.</span><span class="sxs-lookup"><span data-stu-id="e5bec-118">A Public IP Address can be added tooan Azure Resource Manager template using hello Visual Studio Add New Resource Wizard, or by inserting valid JSON into a template.</span></span> 

<span data-ttu-id="e5bec-119">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [offentliga IP-adressen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).</span><span class="sxs-lookup"><span data-stu-id="e5bec-119">Follow this link toosee hello JSON sample within hello Resource Manager template – [Public IP Address](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicipaddressName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "public-ip-front"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

<span data-ttu-id="e5bec-120">En offentlig IP-adress kan associeras med ett virtuellt nätverkskort eller en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="e5bec-120">A Public IP Address can be associated with a Virtual Network Adapter, or a Load Balancer.</span></span> <span data-ttu-id="e5bec-121">I det här exemplet är hello offentliga IP-adressen bifogade toohello belastningsutjämnare eftersom hello musik Store webbplats belastningsutjämnas mellan flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e5bec-121">In this example, because hello Music Store website is load balanced across several virtual machines, hello Public IP Address is attached toohello Load Balancer.</span></span>

<span data-ttu-id="e5bec-122">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [kopplingen offentlig IP-adress till belastningsutjämnaren](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).</span><span class="sxs-lookup"><span data-stu-id="e5bec-122">Follow this link toosee hello JSON sample within hello Resource Manager template – [Public IP Address association with Load Balancer](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).</span></span>

```json
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

<span data-ttu-id="e5bec-123">hello sett offentliga IP-adress som från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e5bec-123">hello public IP Address as seen from hello Azure portal.</span></span> <span data-ttu-id="e5bec-124">Observera att hello offentliga IP-adressen är kopplad tooa belastningsutjämnare och inte en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5bec-124">Notice that hello public IP address is associated tooa load balancer and not a virtual machine.</span></span> <span data-ttu-id="e5bec-125">Belastningsutjämning för nätverk beskrivs i nästa hello-dokument av den här serien.</span><span class="sxs-lookup"><span data-stu-id="e5bec-125">Network load balancers are detailed in hello next document of this series.</span></span>

![Offentlig IP-adress](./media/dotnet-core-3-access-security/pubip.png)

<span data-ttu-id="e5bec-127">Mer information om Azure offentliga IP-adresser finns [IP-adresser i Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span><span class="sxs-lookup"><span data-stu-id="e5bec-127">For more information on Azure Public IP Addresses, see [IP addresses in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span></span>

## <a name="network-security-group"></a><span data-ttu-id="e5bec-128">Nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="e5bec-128">Network Security Group</span></span>
<span data-ttu-id="e5bec-129">När åtkomst har etablerat tooAzure resurser, begränsas åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e5bec-129">Once access has been established tooAzure resources, this access should be limited.</span></span> <span data-ttu-id="e5bec-130">För virtuella Azure-datorer möjliggörs säker åtkomst med en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="e5bec-130">For Azure virtual machines, secure access is accomplished using a network security group.</span></span> <span data-ttu-id="e5bec-131">Med hello musik Store programmet exempel begränsas alla åtkomst toohello virtuella datorn utom för via port 80 för HTTP-åtkomst och port 22 för SSH-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e5bec-131">With hello Music Store application sample, all access toohello virtual machine is restricted except for over port 80 for http access, and port 22 for SSH access.</span></span> <span data-ttu-id="e5bec-132">En Nätverkssäkerhetsgrupp kan läggas till tooan Azure Resource Manager-mallen med hjälp av hello Visual Studio guiden Lägg till ny resurs, eller genom att infoga giltig JSON i en mall.</span><span class="sxs-lookup"><span data-stu-id="e5bec-132">A Network Security Group can be added tooan Azure Resource Manager template using hello Visual Studio Add New Resource Wizard, or by inserting valid JSON into a template.</span></span>

<span data-ttu-id="e5bec-133">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Nätverkssäkerhetsgruppen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).</span><span class="sxs-lookup"><span data-stu-id="e5bec-133">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Security Group](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).</span></span>

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('nsgfront')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "nsg-front"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
}
```

<span data-ttu-id="e5bec-134">I det här exemplet är hello nätverkssäkerhetsgruppen associeras med hello undernätsobjekt som deklarerats i hello virtuella nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="e5bec-134">In this example, hello network security group is associate with hello subnet object declared in hello Virtual Network resource.</span></span> 

<span data-ttu-id="e5bec-135">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Nätverkssäkerhetsgruppen association med ett virtuellt nätverk](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).</span><span class="sxs-lookup"><span data-stu-id="e5bec-135">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Security Group association with Virtual Network](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).</span></span>

```json
"subnets": [
  {
    "name": "[variables('subnetName')]",
    "properties": {
      "addressPrefix": "10.0.0.0/24",
      "networkSecurityGroup": {
        "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
      }
    }
  }
```

<span data-ttu-id="e5bec-136">Det här är vad hello nätverkssäkerhetsgruppen ser ut som från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e5bec-136">Here is what hello network security group looks like from hello Azure portal.</span></span> <span data-ttu-id="e5bec-137">Observera att en NSG som kan associeras med ett undernät och / eller nätverket gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="e5bec-137">Notice that an NSG can be associate with a subnet and / or network interface.</span></span> <span data-ttu-id="e5bec-138">I det här fallet är hello NSG associerade tooa undernät.</span><span class="sxs-lookup"><span data-stu-id="e5bec-138">In this case, hello NSG is associated tooa subnet.</span></span> <span data-ttu-id="e5bec-139">I den här konfigurationen hello inkommande regler gäller tooall virtuella datorer anslutna toohello undernät.</span><span class="sxs-lookup"><span data-stu-id="e5bec-139">In this configuration, hello inbound rules apply tooall virtual machines connected toohello subnet.</span></span>

![Nätverkssäkerhetsgrupp](./media/dotnet-core-3-access-security/nsg.png)

<span data-ttu-id="e5bec-141">Detaljerad information om Nätverkssäkerhetsgrupper finns [vad är en Nätverkssäkerhetsgrupp](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).</span><span class="sxs-lookup"><span data-stu-id="e5bec-141">For in-depth information on Network Security Groups, see [What is a Network Security Group](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).</span></span>

## <a name="next-step"></a><span data-ttu-id="e5bec-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5bec-142">Next step</span></span>
<hr>

[<span data-ttu-id="e5bec-143">Steg 3 – tillgänglighet och skala i Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="e5bec-143">Step 3 - Availability and Scale in Azure Resource Manager Templates</span></span>](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

