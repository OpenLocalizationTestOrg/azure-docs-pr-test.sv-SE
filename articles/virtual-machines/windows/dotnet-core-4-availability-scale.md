---
title: aaaAvailability och skala i Azure Resource Manager-mallar | Microsoft Docs
description: "Virtuella Azure-datorn DotNet grundläggande självstudierna"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 494724b5-06af-4dea-a774-ba580cf27527
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a122d8e9536ea5fc2dc9c3f84042ed5c5179d783
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-scale-in-azure-resource-manager-templates-for-windows-vms"></a><span data-ttu-id="dcebd-103">Tillgänglighet och skala i Azure Resource Manager-mallar för virtuella Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="dcebd-103">Availability and scale in Azure Resource Manager templates for Windows VMs</span></span>

<span data-ttu-id="dcebd-104">Tillgänglighet och skala finns toouptime och hello möjlighet toomeet efterfrågan.</span><span class="sxs-lookup"><span data-stu-id="dcebd-104">Availability and scale refer toouptime and hello ability toomeet demand.</span></span> <span data-ttu-id="dcebd-105">Om ett program måste vara in 99,9% av hello tid, måste det toohave en arkitektur som gör det möjligt för flera samtidiga beräkningsresurser.</span><span class="sxs-lookup"><span data-stu-id="dcebd-105">If an application must be up 99.9% of hello time, it needs toohave an architecture that allows for multiple concurrent compute resources.</span></span> <span data-ttu-id="dcebd-106">I stället för med en webbplats, innehåller en konfiguration med en högre säkerhetsnivå för tillgänglighet till exempel flera instanser av samma plats, med belastningsutjämning teknik framför dem hello.</span><span class="sxs-lookup"><span data-stu-id="dcebd-106">For instance, rather than having a single website, a configuration with a higher level of availability includes multiple instances of hello same site, with balancing technology in front of them.</span></span> <span data-ttu-id="dcebd-107">I den här konfigurationen är kan en instans av programmet hello stängas av för underhåll, medan hello återstående fortsätter toofunction.</span><span class="sxs-lookup"><span data-stu-id="dcebd-107">In this configuration, one instance of hello application can be taken down for maintenance, while hello remaining continue toofunction.</span></span> <span data-ttu-id="dcebd-108">Skala på hello andra sidan refererar tooan program möjlighet tooserve begäran.</span><span class="sxs-lookup"><span data-stu-id="dcebd-108">Scale on hello other hand refers tooan applications ability tooserve demand.</span></span> <span data-ttu-id="dcebd-109">Med belastning kan belastningsutjämnade program, lägga till eller ta bort instanser från hello pool ett program tooscale toomeet begäran.</span><span class="sxs-lookup"><span data-stu-id="dcebd-109">With a load balanced application, adding or removing instances from hello pool allows an application tooscale toomeet demand.</span></span>

<span data-ttu-id="dcebd-110">Det här dokumentet beskriver hur hello musik Store exempeldistribution har konfigurerats för tillgänglighet och skala.</span><span class="sxs-lookup"><span data-stu-id="dcebd-110">This document details how hello Music Store sample deployment is configured for availability and scale.</span></span> <span data-ttu-id="dcebd-111">Alla beroenden och unika konfigurationer är markerade.</span><span class="sxs-lookup"><span data-stu-id="dcebd-111">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="dcebd-112">Distribuera en instans av hello lösning tooyour Azure-prenumeration och fungerar tillsammans med hello Azure Resource Manager-mall för hello bästa möjliga resultat.</span><span class="sxs-lookup"><span data-stu-id="dcebd-112">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="dcebd-113">hello fullständig mallen hittar du här – [musik Store distributionen på Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="dcebd-113">hello complete template can be found here – [Music Store Deployment on Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="availability-set"></a><span data-ttu-id="dcebd-114">Tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="dcebd-114">Availability Set</span></span>
<span data-ttu-id="dcebd-115">En Tillgänglighetsuppsättning täcker logiskt Azure Virtual Machines fysiska värdar och andra infrastrukturella komponenter, till exempel strömkällor och maskinvara för fysiska nätverk.</span><span class="sxs-lookup"><span data-stu-id="dcebd-115">An Availability Set logically spans Azure Virtual Machines across physical hosts and other infrastructural components such as power supplies and physical networking hardware.</span></span> <span data-ttu-id="dcebd-116">Tillgänglighetsuppsättningar se till att inte alla virtuella datorer sker under underhåll, enhet eller annan stillestånd.</span><span class="sxs-lookup"><span data-stu-id="dcebd-116">Availability sets ensure that during maintenance, device failure, or other down time, not all virtual machines are effected.</span></span> <span data-ttu-id="dcebd-117">En Tillgänglighetsuppsättning kan läggas tooan Azure Resource Manager-mallen använder hello Visual Studio guiden Lägg till ny resurs, eller lägga till giltig JSON i en mall.</span><span class="sxs-lookup"><span data-stu-id="dcebd-117">An Availability Set can be added tooan Azure Resource Manager template using hello Visual Studio Add New Resource Wizard, or inserting valid JSON into a template.</span></span>

<span data-ttu-id="dcebd-118">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Tillgänglighetsuppsättning](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L368).</span><span class="sxs-lookup"><span data-stu-id="dcebd-118">Follow this link toosee hello JSON sample within hello Resource Manager template – [Availability Set](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L368).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "availability-set"
  },
  "properties": {
  }
}
```

<span data-ttu-id="dcebd-119">En Tillgänglighetsuppsättning har deklarerats som en egenskap för en virtuell datorresurs.</span><span class="sxs-lookup"><span data-stu-id="dcebd-119">An Availability Set is declared as a property of a Virtual Machine resource.</span></span> 

<span data-ttu-id="dcebd-120">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Tillgänglighetsuppsättning association med den virtuella datorn](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L302).</span><span class="sxs-lookup"><span data-stu-id="dcebd-120">Follow this link toosee hello JSON sample within hello Resource Manager template – [Availability Set association with Virtual Machine](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L302).</span></span>

```json
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
<span data-ttu-id="dcebd-121">Hej tillgänglighetsuppsättning sett från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dcebd-121">hello availability set as seen from hello Azure portal.</span></span> <span data-ttu-id="dcebd-122">Varje virtuell dator och information om hello konfiguration beskrivs här.</span><span class="sxs-lookup"><span data-stu-id="dcebd-122">Each virtual machine and details about hello configuration are detailed here.</span></span>

![Tillgänglighetsuppsättning](./media/dotnet-core-4-availability-scale/ase-win.png)

<span data-ttu-id="dcebd-124">Detaljerad information om Tillgänglighetsuppsättningar finns [hantera tillgängligheten för virtuella datorer](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dcebd-124">For in-depth information on Availability Sets, see [Manage availability of virtual machines](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

## <a name="network-load-balancer"></a><span data-ttu-id="dcebd-125">Utjämning av nätverksbelastning</span><span class="sxs-lookup"><span data-stu-id="dcebd-125">Network Load Balancer</span></span>
<span data-ttu-id="dcebd-126">Medan en tillgänglighetsuppsättning ger programmet feltolerans, gör belastningsutjämning många instanser av programmet hello tillgängliga i en enda nätverksadress.</span><span class="sxs-lookup"><span data-stu-id="dcebd-126">Whereas an availability set provides application fault tolerance, a load balancer makes many instances of hello application available on a single network address.</span></span> <span data-ttu-id="dcebd-127">Flera instanser av ett program kan finnas på många virtuella datorer, var och en ansluten tooa belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="dcebd-127">Multiple instances of an application can be hosted on many virtual machines, each one connected tooa load balancer.</span></span> <span data-ttu-id="dcebd-128">Eftersom programmet hello används hello hello belastningen belastningsutjämnaren vägar inkommande begäran över hello kopplade medlemmar.</span><span class="sxs-lookup"><span data-stu-id="dcebd-128">As hello application is accessed, hello load balancer routes hello incoming request across hello attached members.</span></span> <span data-ttu-id="dcebd-129">En belastningsutjämnare kan läggas till med hello Visual Studio guiden Lägg till ny resurs, eller genom att infoga korrekt formaterad JSON-resurs i hello Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="dcebd-129">A Load Balancer can be added using hello Visual Studio Add New Resource Wizard, or by inserting properly formatted JSON resource into hello Azure Resource Manager template.</span></span>

<span data-ttu-id="dcebd-130">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Utjämning av nätverksbelastning](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L198).</span><span class="sxs-lookup"><span data-stu-id="dcebd-130">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Load Balancer](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L198).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer"
  },
  ........<truncated>
}
```

<span data-ttu-id="dcebd-131">Eftersom hello exempelprogrammet är utsatta toohello internet med en offentlig IP-adress, den här adressen är kopplad till hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="dcebd-131">Because hello sample application is exposed toohello internet with a public IP address, this address is associated with hello load balancer.</span></span> 

<span data-ttu-id="dcebd-132">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Utjämning av nätverksbelastning kopplingen till offentliga IP-adressen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).</span><span class="sxs-lookup"><span data-stu-id="dcebd-132">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Load Balancer association with Public IP Address](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).</span></span>

```json
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIpAddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

<span data-ttu-id="dcebd-133">Från hello Azure-portalen visar hello översikt över Utjämning belastningsutjämnaren hello association med hello offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="dcebd-133">From hello Azure portal, hello network load balancer overview shows hello association with hello public IP address.</span></span>

![Utjämning av nätverksbelastning](./media/dotnet-core-4-availability-scale/nlb-win.png)

## <a name="load-balancer-rule"></a><span data-ttu-id="dcebd-135">Regel för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="dcebd-135">Load Balancer Rule</span></span>
<span data-ttu-id="dcebd-136">När du använder en belastningsutjämnare, konfigureras regler som styr hur trafik fördelas över hello avsedda resurserna.</span><span class="sxs-lookup"><span data-stu-id="dcebd-136">When using a load balancer, rules are configured that control how traffic is balanced across hello intended resources.</span></span> <span data-ttu-id="dcebd-137">Med hello för musik Store exempelprogrammet trafik anländer på port 80 på hello offentlig IP-adress och distribueras till port 80 på alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="dcebd-137">With hello sample Music Store application, traffic arrives on port 80 of hello public IP address and is distributed across port 80 of all virtual machines.</span></span> 

<span data-ttu-id="dcebd-138">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Belastningsutjämningsregeln](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L226).</span><span class="sxs-lookup"><span data-stu-id="dcebd-138">Follow this link toosee hello JSON sample within hello Resource Manager template – [Load Balancer Rule](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L226).</span></span>

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

<span data-ttu-id="dcebd-139">En vy över nätverket hello belastningsutjämningsregeln från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="dcebd-139">A view of hello network load balancer rule from hello portal.</span></span>

![Nätverk-regel för belastningsutjämnare](./media/dotnet-core-4-availability-scale/lbrule-win.png)

## <a name="load-balancer-probe"></a><span data-ttu-id="dcebd-141">Belastningsutjämningsavsökning</span><span class="sxs-lookup"><span data-stu-id="dcebd-141">Load Balancer Probe</span></span>
<span data-ttu-id="dcebd-142">hello belastningsutjämnaren måste också toomonitor varje virtuell dator så att begäranden hanteras endast toorunning system.</span><span class="sxs-lookup"><span data-stu-id="dcebd-142">hello load balancer also needs toomonitor each virtual machine so that requests are served only toorunning systems.</span></span> <span data-ttu-id="dcebd-143">Denna övervakning sker genom konstant sökning i en fördefinierad port.</span><span class="sxs-lookup"><span data-stu-id="dcebd-143">This monitoring takes place by constant probing of a pre-defined port.</span></span> <span data-ttu-id="dcebd-144">hello är musik Store konfigurerade tooprobe port 80 på alla med virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="dcebd-144">hello Music Store deployment is configured tooprobe port 80 on all included virtual machines.</span></span> 

<span data-ttu-id="dcebd-145">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [belastningen belastningsutjämnaren avsökning](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L247).</span><span class="sxs-lookup"><span data-stu-id="dcebd-145">Follow this link toosee hello JSON sample within hello Resource Manager template – [Load Balancer Probe](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L247).</span></span>

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

<span data-ttu-id="dcebd-146">Hej belastningsutjämningsavsökning sett från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dcebd-146">hello load balancer probe seen from hello Azure portal.</span></span>

![Nätverk Belastningsutjämningsavsökning](./media/dotnet-core-4-availability-scale/lbprobe-win.png)

## <a name="inbound-nat-rules"></a><span data-ttu-id="dcebd-148">Regler för ingående NAT</span><span class="sxs-lookup"><span data-stu-id="dcebd-148">Inbound NAT Rules</span></span>
<span data-ttu-id="dcebd-149">När du använder en belastningsutjämnare regler måste toobe inrätta som tillhandahåller icke belastningen belastningsutjämnade åtkomst tooeach virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="dcebd-149">When using a Load Balancer, rules need toobe put into place that provide non-load balanced access tooeach Virtual Machine.</span></span> <span data-ttu-id="dcebd-150">Exempelvis när du skapar en RDP-anslutning med varje virtuell dator kan den här trafiken inte bör vara belastningsutjämnad, snarare en förbestämd sökväg ska konfigureras.</span><span class="sxs-lookup"><span data-stu-id="dcebd-150">For instance, when creating an RDP connection with each virtual machine, this traffic should not be load balanced, rather a pre-determined path should be configured.</span></span> <span data-ttu-id="dcebd-151">förinställt sökvägar konfigureras med hjälp av en resurs för inkommande NAT-regeln.</span><span class="sxs-lookup"><span data-stu-id="dcebd-151">Pre-determined paths are configured using an Inbound NAT Rule resource.</span></span> <span data-ttu-id="dcebd-152">Med den här resursen vara inkommande kommunikation mappade tooindividual virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="dcebd-152">Using this resource, inbound communication can be mapped tooindividual Virtual Machines.</span></span> 

<span data-ttu-id="dcebd-153">Med hello musik Store-programmet är en port som börjar vid 5000 mappade tooport 3389 på varje virtuell dator för RDP-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="dcebd-153">With hello Music Store application, a port starting at 5000 is mapped tooport 3389 on each Virtual Machine for RDP access.</span></span> <span data-ttu-id="dcebd-154">Hej `copyindex()` funktionen är används tooincrement Hej inkommande port, så att hello andra virtuell dator tar emot ett inkommande port 5001 hello tredje 5002 och så vidare.</span><span class="sxs-lookup"><span data-stu-id="dcebd-154">hello `copyindex()` function is used tooincrement hello incoming port, such that hello second Virtual Machine receives an incoming port of 5001, hello third 5002, and so on.</span></span>

<span data-ttu-id="dcebd-155">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [inkommande NAT-regler](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L260).</span><span class="sxs-lookup"><span data-stu-id="dcebd-155">Follow this link toosee hello JSON sample within hello Resource Manager template – [Inbound NAT Rules](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L260).</span></span> 

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'RDP-VM', copyIndex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
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
    "backendPort": 3389,
    "enableFloatingIP": false
  }
}
```

<span data-ttu-id="dcebd-156">Ett exempel inkommande NAT-regel som visas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dcebd-156">One example inbound NAT rule as seen in hello Azure portal.</span></span> <span data-ttu-id="dcebd-157">En RDP-NAT-regel skapas för varje virtuell dator i hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="dcebd-157">An RDP NAT rule is created for each virtual machine in hello deployment.</span></span>

![Inkommande NAT-regel](./media/dotnet-core-4-availability-scale/natrule-win.png)

<span data-ttu-id="dcebd-159">Detaljerad information om hello Azure Utjämning av nätverksbelastning finns [belastningsutjämning för Azure infrastrukturtjänster](load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dcebd-159">For in-depth information on hello Azure Network Load Balancer, see [Load balancing for Azure infrastructure services](load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="deploy-multiple-vms"></a><span data-ttu-id="dcebd-160">Distribuera flera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="dcebd-160">Deploy multiple VMs</span></span>
<span data-ttu-id="dcebd-161">Slutligen för en Tillgänglighetsuppsättning eller belastningsutjämnare tooeffectively funktion krävs flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="dcebd-161">Finally, for an Availability Set or Load Balancer tooeffectively function, multiple virtual machines are required.</span></span> <span data-ttu-id="dcebd-162">Flera virtuella datorer kan distribueras med hjälp av hello Azure Resource Manager-mall för kopieringsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="dcebd-162">Multiple VMs can be deployed using hello Azure Resource Manager template copy function.</span></span> <span data-ttu-id="dcebd-163">Använder hello kopieringsfunktionen, är det inte nödvändigt toodefine ett begränsat antal virtuella datorer, i stället det här värdet kan erbjudas dynamiskt vid hello tiden för distributionen.</span><span class="sxs-lookup"><span data-stu-id="dcebd-163">Using hello copy function, it is not necessary toodefine a finite number of Virtual Machines, rather this value can be dynamically provided at hello time of deployment.</span></span> <span data-ttu-id="dcebd-164">hello kopieringsfunktionen förbrukar hello antalet instanser toobe skapas och referenser som distribuerar hello rätt antal virtuella datorer och associerade resurser.</span><span class="sxs-lookup"><span data-stu-id="dcebd-164">hello copy function consumes hello number of instances toobe created and handles deploying hello proper number of virtual machines and associated resources.</span></span>

<span data-ttu-id="dcebd-165">I hello musik Store exempelmall definieras en parameter som tar i instansantal.</span><span class="sxs-lookup"><span data-stu-id="dcebd-165">In hello Music Store Sample template, a parameter is defined that takes in an instance count.</span></span> <span data-ttu-id="dcebd-166">Numret används i hela hello mallen när du skapar virtuella datorer och relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="dcebd-166">This number is used throughout hello template when creating virtual machines and related resources.</span></span>

```json
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 2,
  "metadata": {
    "description": "Number of VM instances toobe created behind load balancer."
  }
},
```

<span data-ttu-id="dcebd-167">Hello kopiera loop ges ett namn på hello virtuell datorresurs, och hello antal instanser parametern används toocontrol hello antal resulterande kopior.</span><span class="sxs-lookup"><span data-stu-id="dcebd-167">On hello Virtual Machine resource, hello copy loop is given a name and hello number of instances parameter used toocontrol hello number of resulting copies.</span></span>

<span data-ttu-id="dcebd-168">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [virtuella kopieringsfunktionen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L290).</span><span class="sxs-lookup"><span data-stu-id="dcebd-168">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Machine Copy Function](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L290).</span></span> 

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  }
```

<span data-ttu-id="dcebd-169">hello aktuella iteration av hello kopieringsfunktionen kan nås med hello `copyIndex()` funktion.</span><span class="sxs-lookup"><span data-stu-id="dcebd-169">hello current iteration of hello copy function can be accessed with hello `copyIndex()` function.</span></span> <span data-ttu-id="dcebd-170">hello-värdet för hello kopieringsfunktionen indexet kan vara används tooname virtuella datorer och andra resurser.</span><span class="sxs-lookup"><span data-stu-id="dcebd-170">hello value of hello copy index function can be used tooname virtual machines and other resources.</span></span> <span data-ttu-id="dcebd-171">Till exempel måste två instanser av en virtuell dator distribueras, de olika namn.</span><span class="sxs-lookup"><span data-stu-id="dcebd-171">For instance, if two instances of a virtual machine are deployed, they need different names.</span></span> <span data-ttu-id="dcebd-172">Hej `copyIndex()` funktionen kan användas som en del av hello virtuella namn toocreate ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="dcebd-172">hello `copyIndex()` function can be used as part of hello virtual machine name toocreate a unique name.</span></span> <span data-ttu-id="dcebd-173">Ett exempel på hello `copyindex()` funktion som används för namngivning kan ses i hello virtuella datorresurser.</span><span class="sxs-lookup"><span data-stu-id="dcebd-173">An example of hello `copyindex()` function used for naming purposes can be seen in hello Virtual Machine resource.</span></span> <span data-ttu-id="dcebd-174">Här hello datornamnet är en sammansättning av hello `vmName` parametern och hello `copyIndex()` funktion.</span><span class="sxs-lookup"><span data-stu-id="dcebd-174">Here, hello computer name is a concatenation of hello `vmName` parameter, and hello `copyIndex()` function.</span></span> 

<span data-ttu-id="dcebd-175">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Index kopieringsfunktionen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L309).</span><span class="sxs-lookup"><span data-stu-id="dcebd-175">Follow this link toosee hello JSON sample within hello Resource Manager template – [Copy Index Function](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L309).</span></span> 

```json
"osProfile": {
  "computerName": "[concat(variables('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "adminPassword": "[parameters('adminPassword')]"
}
```

<span data-ttu-id="dcebd-176">Hej `copyIndex` funktionen används flera gånger i hello musik Store exempelmall.</span><span class="sxs-lookup"><span data-stu-id="dcebd-176">hello `copyIndex` function is used several times in hello Music Store sample template.</span></span> <span data-ttu-id="dcebd-177">Resurser och funktioner som använder `copyIndex` innehåller något annat specifika tooa instans av hello virtuella sådana och nätverksgränssnitt, regler för inläsning av belastningsutjämning, och alla beroende funktioner.</span><span class="sxs-lookup"><span data-stu-id="dcebd-177">Resources and functions utilizing `copyIndex` include anything specific tooa single instance of hello virtual machine such and network interface, load balancer rules, and any depends on functions.</span></span> 

<span data-ttu-id="dcebd-178">Mer information om hello kopieringsfunktionen finns [skapa flera instanser av resurser i Azure Resource Manager](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="dcebd-178">For more information on hello copy function, see [Create multiple instances of resources in Azure Resource Manager](../../resource-group-create-multiple.md).</span></span>

## <a name="next-step"></a><span data-ttu-id="dcebd-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dcebd-179">Next step</span></span>
<hr>

[<span data-ttu-id="dcebd-180">Steg 4 – programdistribution med Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="dcebd-180">Step 4 - Application Deployment with Azure Resource Manager Templates</span></span>](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

