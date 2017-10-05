---
title: "Tillgänglighet och skala i Azure Resource Manager-mallar | Microsoft Docs"
description: "Virtuella Azure-datorn DotNet grundläggande självstudierna"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8fcfea79-f017-4658-8c51-74242fcfb7f6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0c0250b8152ed31b9a5d8b42ae139c9b38da0984
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="availability-and-scale-in-azure-resource-manager-templates-for-linux-vms"></a><span data-ttu-id="a97d0-103">Tillgänglighet och skala i Azure Resource Manager-mallar för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="a97d0-103">Availability and scale in Azure Resource Manager templates for Linux VMs</span></span>

<span data-ttu-id="a97d0-104">Tillgänglighet och skala finns drifttid och möjligheten att uppfylla begäran.</span><span class="sxs-lookup"><span data-stu-id="a97d0-104">Availability and scale refer to uptime and the ability to meet demand.</span></span> <span data-ttu-id="a97d0-105">Om ett program måste vara in 99,9% av tiden, måste den ha en arkitektur som gör det möjligt för flera samtidiga beräkningsresurser.</span><span class="sxs-lookup"><span data-stu-id="a97d0-105">If an application must be up 99.9% of the time, it needs to have an architecture that allows for multiple concurrent compute resources.</span></span> <span data-ttu-id="a97d0-106">I stället för med en webbplats, innehåller en konfiguration med en högre säkerhetsnivå för tillgänglighet till exempel flera instanser av samma plats, med belastningsutjämning teknik framför dem.</span><span class="sxs-lookup"><span data-stu-id="a97d0-106">For instance, rather than having a single website, a configuration with a higher level of availability includes multiple instances of the same site, with balancing technology in front of them.</span></span> <span data-ttu-id="a97d0-107">I den här konfigurationen är kan en instans av programmet stängas av för underhåll, medan de återstående fortsätter att fungera.</span><span class="sxs-lookup"><span data-stu-id="a97d0-107">In this configuration, one instance of the application can be taken down for maintenance, while the remaining continue to function.</span></span> <span data-ttu-id="a97d0-108">Skala refererar å andra sidan till ett program möjlighet att hantera begäran.</span><span class="sxs-lookup"><span data-stu-id="a97d0-108">Scale on the other hand refers to an applications ability to serve demand.</span></span> <span data-ttu-id="a97d0-109">Med belastning kan belastningsutjämnade program, lägga till eller ta bort instanser från poolen programmet att skala för att uppfylla begäran.</span><span class="sxs-lookup"><span data-stu-id="a97d0-109">With a load balanced application, adding or removing instances from the pool allows an application to scale to meet demand.</span></span>

<span data-ttu-id="a97d0-110">Det här dokumentet beskriver hur exempeldistribution musik Store har konfigurerats för tillgänglighet och skala.</span><span class="sxs-lookup"><span data-stu-id="a97d0-110">This document details how the Music Store sample deployment is configured for availability and scale.</span></span> <span data-ttu-id="a97d0-111">Alla beroenden och unika konfigurationer är markerade.</span><span class="sxs-lookup"><span data-stu-id="a97d0-111">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="a97d0-112">Distribuera en instans av lösningen till din Azure-prenumeration och fungerar tillsammans med Azure Resource Manager-mall för bästa resultat.</span><span class="sxs-lookup"><span data-stu-id="a97d0-112">For the best experience, pre-deploy an instance of the solution to your Azure subscription and work along with the Azure Resource Manager template.</span></span> <span data-ttu-id="a97d0-113">Fullständig mallen hittar du här – [musik Store distributionen på Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="a97d0-113">The complete template can be found here – [Music Store Deployment on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span>

## <a name="availability-set"></a><span data-ttu-id="a97d0-114">Tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="a97d0-114">Availability Set</span></span>
<span data-ttu-id="a97d0-115">En Tillgänglighetsuppsättning täcker logiskt Azure Virtual Machines fysiska värdar och andra infrastrukturella komponenter, till exempel strömkällor och maskinvara för fysiska nätverk.</span><span class="sxs-lookup"><span data-stu-id="a97d0-115">An Availability Set logically spans Azure Virtual Machines across physical hosts and other infrastructural components such as power supplies and physical networking hardware.</span></span> <span data-ttu-id="a97d0-116">Tillgänglighetsuppsättningar se till att inte alla virtuella datorer sker under underhåll, enhet eller annan stillestånd.</span><span class="sxs-lookup"><span data-stu-id="a97d0-116">Availability sets ensure that during maintenance, device failure, or other down time, not all virtual machines are effected.</span></span> <span data-ttu-id="a97d0-117">En Tillgänglighetsuppsättning kan läggas till en Azure Resource Manager-mall med hjälp av Visual Studio guiden Lägg till ny resurs eller infoga giltig JSON i en mall.</span><span class="sxs-lookup"><span data-stu-id="a97d0-117">An Availability Set can be added to an Azure Resource Manager template using the Visual Studio Add New Resource Wizard, or inserting valid JSON into a template.</span></span>

<span data-ttu-id="a97d0-118">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Tillgänglighetsuppsättning](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387).</span><span class="sxs-lookup"><span data-stu-id="a97d0-118">Follow this link to see the JSON sample within the Resource Manager template – [Availability Set](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "avalibility-set"
  },
  "properties": {
    "platformUpdateDomainCount": 5,
    "platformFaultDomainCount": 3
  }
}
```

<span data-ttu-id="a97d0-119">En Tillgänglighetsuppsättning har deklarerats som en egenskap för en virtuell datorresurs.</span><span class="sxs-lookup"><span data-stu-id="a97d0-119">An Availability Set is declared as a property of a Virtual Machine resource.</span></span> 

<span data-ttu-id="a97d0-120">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Tillgänglighetsuppsättning association med den virtuella datorn](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313).</span><span class="sxs-lookup"><span data-stu-id="a97d0-120">Follow this link to see the JSON sample within the Resource Manager template – [Availability Set association with Virtual Machine](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313).</span></span>

```json
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
<span data-ttu-id="a97d0-121">Tillgänglighetsuppsättning sett från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a97d0-121">The availability set as seen from the Azure portal.</span></span> <span data-ttu-id="a97d0-122">Varje virtuell dator och information om konfigurationen beskrivs här.</span><span class="sxs-lookup"><span data-stu-id="a97d0-122">Each virtual machine and details about the configuration are detailed here.</span></span>

![Tillgänglighetsuppsättning](./media/dotnet-core-4-availability-scale/aset.png)

<span data-ttu-id="a97d0-124">Detaljerad information om Tillgänglighetsuppsättningar finns [hantera tillgängligheten för virtuella datorer](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a97d0-124">For in-depth information on Availability Sets, see [Manage availability of virtual machines](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

## <a name="network-load-balancer"></a><span data-ttu-id="a97d0-125">Utjämning av nätverksbelastning</span><span class="sxs-lookup"><span data-stu-id="a97d0-125">Network Load Balancer</span></span>
<span data-ttu-id="a97d0-126">Medan en tillgänglighetsuppsättning ger programmet feltolerans, tillgängliggör belastningsutjämning många instanser av programmet på en enda nätverksadress.</span><span class="sxs-lookup"><span data-stu-id="a97d0-126">Whereas an availability set provides application fault tolerance, a load balancer makes many instances of the application available on a single network address.</span></span> <span data-ttu-id="a97d0-127">Flera instanser av ett program kan finnas på många virtuella datorer, var och en ansluten till en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="a97d0-127">Multiple instances of an application can be hosted on many virtual machines, each one connected to a load balancer.</span></span> <span data-ttu-id="a97d0-128">Eftersom programmet används dirigerar inkommande begäran över de anslutna medlemmarna i belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="a97d0-128">As the application is accessed, the load balancer routes the incoming request across the attached members.</span></span> <span data-ttu-id="a97d0-129">En belastningsutjämnare kan läggas till med hjälp av Visual Studio guiden Lägg till ny resurs, eller genom att infoga korrekt formaterad JSON-resursen till Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="a97d0-129">A Load Balancer can be added using the Visual Studio Add New Resource Wizard, or by inserting properly formatted JSON resource into the Azure Resource Manager template.</span></span>

<span data-ttu-id="a97d0-130">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Utjämning av nätverksbelastning](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).</span><span class="sxs-lookup"><span data-stu-id="a97d0-130">Follow this link to see the JSON sample within the Resource Manager template – [Network Load Balancer](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-front"
  },
  ........<truncated>
}
```

<span data-ttu-id="a97d0-131">Eftersom exempelprogrammet exponeras mot internet med en offentlig IP-adress och är den här adressen kopplad till belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="a97d0-131">Because the sample application is exposed to the internet with a public IP address, this address is associated with the load balancer.</span></span> 

<span data-ttu-id="a97d0-132">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Utjämning av nätverksbelastning kopplingen till offentliga IP-adressen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).</span><span class="sxs-lookup"><span data-stu-id="a97d0-132">Follow this link to see the JSON sample within the Resource Manager template – [Network Load Balancer association with Public IP Address](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).</span></span>

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

<span data-ttu-id="a97d0-133">I belastningsutjämnaren översikt över Utjämning visar kopplingen till den offentliga IP-adressen från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a97d0-133">From the Azure portal, the network load balancer overview shows the association with the public IP address.</span></span>

![Utjämning av nätverksbelastning](./media/dotnet-core-4-availability-scale/nlb.png)

## <a name="load-balancer-rule"></a><span data-ttu-id="a97d0-135">Regel för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="a97d0-135">Load Balancer Rule</span></span>
<span data-ttu-id="a97d0-136">När du använder en belastningsutjämnare, konfigureras regler som styr hur trafik fördelas över de avsedda resurserna.</span><span class="sxs-lookup"><span data-stu-id="a97d0-136">When using a load balancer, rules are configured that control how traffic is balanced across the intended resources.</span></span> <span data-ttu-id="a97d0-137">Med musik Store exempelprogrammet trafik anländer på port 80 på den offentliga IP-adressen och distribueras till port 80 på alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a97d0-137">With the sample Music Store application, traffic arrives on port 80 of the public IP address and is distributed across port 80 of all virtual machines.</span></span> 

<span data-ttu-id="a97d0-138">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Belastningsutjämningsregeln](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).</span><span class="sxs-lookup"><span data-stu-id="a97d0-138">Follow this link to see the JSON sample within the Resource Manager template – [Load Balancer Rule](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).</span></span>

```json
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

<span data-ttu-id="a97d0-139">En vy över nätverket belastningsutjämningsregeln från portalen.</span><span class="sxs-lookup"><span data-stu-id="a97d0-139">A view of the network load balancer rule from the portal.</span></span>

![Nätverk-regel för belastningsutjämnare](./media/dotnet-core-4-availability-scale/lbrule.png)

## <a name="load-balancer-probe"></a><span data-ttu-id="a97d0-141">Belastningsutjämningsavsökning</span><span class="sxs-lookup"><span data-stu-id="a97d0-141">Load Balancer Probe</span></span>
<span data-ttu-id="a97d0-142">Belastningsutjämnaren måste också övervaka varje virtuell dator så att begäranden skickas endast till kör.</span><span class="sxs-lookup"><span data-stu-id="a97d0-142">The load balancer also needs to monitor each virtual machine so that requests are served only to running systems.</span></span> <span data-ttu-id="a97d0-143">Denna övervakning sker genom konstant sökning i en fördefinierad port.</span><span class="sxs-lookup"><span data-stu-id="a97d0-143">This monitoring takes place by constant probing of a pre-defined port.</span></span> <span data-ttu-id="a97d0-144">Musik Store-distribution är konfigurerad för att avsökning port 80 på alla virtuella datorer som ingår.</span><span class="sxs-lookup"><span data-stu-id="a97d0-144">The Music Store deployment is configured to probe port 80 on all included virtual machines.</span></span> 

<span data-ttu-id="a97d0-145">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [belastningen belastningsutjämnaren avsökning](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257).</span><span class="sxs-lookup"><span data-stu-id="a97d0-145">Follow this link to see the JSON sample within the Resource Manager template – [Load Balancer Probe](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257).</span></span>

```json
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

<span data-ttu-id="a97d0-146">Belastningsutjämningsavsökning sett från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a97d0-146">The load balancer probe seen from the Azure portal.</span></span>

![Nätverk Belastningsutjämningsavsökning](./media/dotnet-core-4-availability-scale/lbprobe.png)

## <a name="inbound-nat-rules"></a><span data-ttu-id="a97d0-148">Regler för ingående NAT</span><span class="sxs-lookup"><span data-stu-id="a97d0-148">Inbound NAT Rules</span></span>
<span data-ttu-id="a97d0-149">När du använder en belastningsutjämnare måste regler införas som ger icke belastningen belastningsutjämnade åtkomst till varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a97d0-149">When using a Load Balancer, rules need to be put into place that provide non-load balanced access to each Virtual Machine.</span></span> <span data-ttu-id="a97d0-150">Exempelvis när du skapar en SSH-anslutning med varje virtuell dator kan den här trafiken inte bör vara belastningsutjämnad, snarare en förbestämd sökväg ska konfigureras.</span><span class="sxs-lookup"><span data-stu-id="a97d0-150">For instance, when creating an SSH connection with each virtual machine, this traffic should not be load balanced, rather a pre-determined path should be configured.</span></span> <span data-ttu-id="a97d0-151">förinställt sökvägar konfigureras med hjälp av en resurs för inkommande NAT-regeln.</span><span class="sxs-lookup"><span data-stu-id="a97d0-151">pre-determined paths are configured using an Inbound NAT Rule resource.</span></span> <span data-ttu-id="a97d0-152">Med den här resursen mappas inkommande kommunikation till enskilda virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a97d0-152">Using this resource, inbound communication can be mapped to individual Virtual Machines.</span></span> 

<span data-ttu-id="a97d0-153">En port som börjar vid 5000 är mappad till port 22 på varje virtuell dator för SSH-åtkomst till musik Store-programmet.</span><span class="sxs-lookup"><span data-stu-id="a97d0-153">With the Music Store application, a port starting at 5000 is mapped to port 22 on each Virtual Machine for SSH access.</span></span> <span data-ttu-id="a97d0-154">Den `copyindex()` funktionen används för att öka den inkommande porten, så att den andra virtuella datorn tar emot ett inkommande port 5001, tredje 5002 och så vidare.</span><span class="sxs-lookup"><span data-stu-id="a97d0-154">The `copyindex()` function is used to increment the incoming port, such that the second Virtual Machine receives an incoming port of 5001, the third 5002, and so on.</span></span> 

<span data-ttu-id="a97d0-155">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [inkommande NAT-regler](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).</span><span class="sxs-lookup"><span data-stu-id="a97d0-155">Follow this link to see the JSON sample within the Resource Manager template – [Inbound NAT Rules](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).</span></span> 

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'SSH-VM', copyIndex())]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 22,
    "enableFloatingIP": false
  }
}
```

<span data-ttu-id="a97d0-156">Ett exempel inkommande NAT-regel som visas i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a97d0-156">One example inbound NAT rule as seen in the Azure portal.</span></span> <span data-ttu-id="a97d0-157">En SSH NAT-regel skapas för varje virtuell dator i distributionen.</span><span class="sxs-lookup"><span data-stu-id="a97d0-157">An SSH NAT rule is created for each virtual machine in the deployment.</span></span>

![Inkommande NAT-regel](./media/dotnet-core-4-availability-scale/natrule.png)

<span data-ttu-id="a97d0-159">Detaljerad information om belastningsutjämning för Azure-nätverk finns i [belastningsutjämning för Azure infrastrukturtjänster](../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a97d0-159">For in-depth information on the Azure Network Load Balancer, see [Load balancing for Azure infrastructure services](../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="deploy-multiple-vms"></a><span data-ttu-id="a97d0-160">Distribuera flera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a97d0-160">Deploy multiple VMs</span></span>
<span data-ttu-id="a97d0-161">Slutligen för en Tillgänglighetsuppsättning eller belastningsutjämnare ska fungera effektivt kan krävs flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a97d0-161">Finally, for an Availability Set or Load Balancer to effectively function, multiple virtual machines are required.</span></span> <span data-ttu-id="a97d0-162">Flera virtuella datorer kan distribueras med hjälp av Azure Resource Manager-mall för kopieringsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="a97d0-162">Multiple VMs can be deployed using the Azure Resource Manager template copy function.</span></span> <span data-ttu-id="a97d0-163">Använda kopieringsfunktionen behöver inte definiera ett begränsat antal virtuella datorer, i stället det här värdet kan erbjudas dynamiskt vid tidpunkten för distribution.</span><span class="sxs-lookup"><span data-stu-id="a97d0-163">Using the copy function, it is not necessary to define a finite number of Virtual Machines, rather this value can be dynamically provided at the time of deployment.</span></span> <span data-ttu-id="a97d0-164">Kopieringsfunktionen förbrukar antalet instanser som skapats och referenser som distribuerar rätt antal virtuella datorer och associerade resurser.</span><span class="sxs-lookup"><span data-stu-id="a97d0-164">The copy function consumes the number of instances to created and handles deploying the proper number of virtual machines and associated resources.</span></span>

<span data-ttu-id="a97d0-165">I mallen musik Store exempel definieras en parameter i ett instansantal som tar.</span><span class="sxs-lookup"><span data-stu-id="a97d0-165">In the Music Store Sample template, a parameter is defined that takes in an instance count.</span></span> <span data-ttu-id="a97d0-166">Numret används i mallen när du skapar virtuella datorer och relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="a97d0-166">This number is used throughout the template when creating virtual machines and related resources.</span></span>

```json
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 1,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
}
```

<span data-ttu-id="a97d0-167">På den virtuella datorresursen ges kopiera loop ett namn och antalet instanser parameter som används för att styra hur många resulterande kopior.</span><span class="sxs-lookup"><span data-stu-id="a97d0-167">On the Virtual Machine resource, the copy loop is given a name and the number of instances parameter used to control the number of resulting copies.</span></span>

<span data-ttu-id="a97d0-168">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [virtuella kopieringsfunktionen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300).</span><span class="sxs-lookup"><span data-stu-id="a97d0-168">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Machine Copy Function](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300).</span></span> 

```json
"apiVersion": "2015-06-15",
"type": "Microsoft.Compute/virtualMachines",
"name": "[concat(variables('vmName'),copyindex())]",
"location": "[resourceGroup().location]",
"copy": {
  "name": "virtualMachineLoop",
  "count": "[parameters('numberOfInstances')]"
}
```

<span data-ttu-id="a97d0-169">Den aktuella iterationen av kopieringsfunktionen kan användas med den `copyIndex()` funktion.</span><span class="sxs-lookup"><span data-stu-id="a97d0-169">The current iteration of the copy function can be accessed with the `copyIndex()` function.</span></span> <span data-ttu-id="a97d0-170">Värdet för kopieringsfunktionen för index kan användas för att namnge virtuella datorer och andra resurser.</span><span class="sxs-lookup"><span data-stu-id="a97d0-170">The value of the copy index function can be used to name virtual machines and other resources.</span></span> <span data-ttu-id="a97d0-171">Till exempel måste två instanser av en virtuell dator distribueras, de olika namn.</span><span class="sxs-lookup"><span data-stu-id="a97d0-171">For instance, if two instances of a virtual machine are deployed, they need different names.</span></span> <span data-ttu-id="a97d0-172">Den `copyIndex()` funktionen kan användas som en del av namnet på virtuella datorn för att skapa ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="a97d0-172">The `copyIndex()` function can be used as part of the virtual machine name to create a unique name.</span></span> <span data-ttu-id="a97d0-173">Ett exempel på den `copyindex()` funktion som används för namngivning kan ses i den virtuella datorresursen.</span><span class="sxs-lookup"><span data-stu-id="a97d0-173">An example of the `copyindex()` function used for naming purposes can be seen in the Virtual Machine resource.</span></span> <span data-ttu-id="a97d0-174">Här är namnet på datorn är en sammansättning av den `vmName` parameter, och `copyIndex()` funktion.</span><span class="sxs-lookup"><span data-stu-id="a97d0-174">Here, the computer name is a concatenation of the `vmName` parameter, and the `copyIndex()` function.</span></span> 

<span data-ttu-id="a97d0-175">Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Index kopieringsfunktionen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319).</span><span class="sxs-lookup"><span data-stu-id="a97d0-175">Follow this link to see the JSON sample within the Resource Manager template – [Copy Index Function](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319).</span></span> 

```json
"osProfile": {
  "computerName": "[concat(parameters('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "linuxConfiguration": {
    "disablePasswordAuthentication": "true",
    "ssh": {
      "publicKeys": [
        {
          "path": "[variables('sshKeyPath')]",
          "keyData": "[parameters('sshKeyData')]"
        }
      ]
    }
  }
}
```

<span data-ttu-id="a97d0-176">Den `copyIndex` funktionen används flera gånger i exempelmall musik Store.</span><span class="sxs-lookup"><span data-stu-id="a97d0-176">The `copyIndex` function is used several times in the Music Store sample template.</span></span> <span data-ttu-id="a97d0-177">Resurser och funktioner som använder `copyIndex` innehåller allt som en enda instans av den virtuella datorn till exempel nätverksgränssnitt, regler för inläsning av belastningsutjämning, och alla beroende funktioner.</span><span class="sxs-lookup"><span data-stu-id="a97d0-177">Resources and functions utilizing `copyIndex` include anything specific to a single instance of the virtual machine such as network interface, load balancer rules, and any depends on functions.</span></span> 

<span data-ttu-id="a97d0-178">Mer information om kopieringsfunktionen finns [skapa flera instanser av resurser i Azure Resource Manager](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="a97d0-178">For more information on the copy function, see [Create multiple instances of resources in Azure Resource Manager](../../resource-group-create-multiple.md).</span></span>

## <a name="next-step"></a><span data-ttu-id="a97d0-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a97d0-179">Next step</span></span>
<hr>

[<span data-ttu-id="a97d0-180">Steg 4 – programdistribution med Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="a97d0-180">Step 4 - Application Deployment with Azure Resource Manager Templates</span></span>](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

