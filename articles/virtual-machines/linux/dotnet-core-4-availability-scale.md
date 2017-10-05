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
# <a name="availability-and-scale-in-azure-resource-manager-templates-for-linux-vms"></a>Tillgänglighet och skala i Azure Resource Manager-mallar för virtuella Linux-datorer

Tillgänglighet och skala finns drifttid och möjligheten att uppfylla begäran. Om ett program måste vara in 99,9% av tiden, måste den ha en arkitektur som gör det möjligt för flera samtidiga beräkningsresurser. I stället för med en webbplats, innehåller en konfiguration med en högre säkerhetsnivå för tillgänglighet till exempel flera instanser av samma plats, med belastningsutjämning teknik framför dem. I den här konfigurationen är kan en instans av programmet stängas av för underhåll, medan de återstående fortsätter att fungera. Skala refererar å andra sidan till ett program möjlighet att hantera begäran. Med belastning kan belastningsutjämnade program, lägga till eller ta bort instanser från poolen programmet att skala för att uppfylla begäran.

Det här dokumentet beskriver hur exempeldistribution musik Store har konfigurerats för tillgänglighet och skala. Alla beroenden och unika konfigurationer är markerade. Distribuera en instans av lösningen till din Azure-prenumeration och fungerar tillsammans med Azure Resource Manager-mall för bästa resultat. Fullständig mallen hittar du här – [musik Store distributionen på Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="availability-set"></a>Tillgänglighetsuppsättning
En Tillgänglighetsuppsättning täcker logiskt Azure Virtual Machines fysiska värdar och andra infrastrukturella komponenter, till exempel strömkällor och maskinvara för fysiska nätverk. Tillgänglighetsuppsättningar se till att inte alla virtuella datorer sker under underhåll, enhet eller annan stillestånd. En Tillgänglighetsuppsättning kan läggas till en Azure Resource Manager-mall med hjälp av Visual Studio guiden Lägg till ny resurs eller infoga giltig JSON i en mall.

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Tillgänglighetsuppsättning](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387).

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

En Tillgänglighetsuppsättning har deklarerats som en egenskap för en virtuell datorresurs. 

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Tillgänglighetsuppsättning association med den virtuella datorn](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313).

```json
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
Tillgänglighetsuppsättning sett från Azure-portalen. Varje virtuell dator och information om konfigurationen beskrivs här.

![Tillgänglighetsuppsättning](./media/dotnet-core-4-availability-scale/aset.png)

Detaljerad information om Tillgänglighetsuppsättningar finns [hantera tillgängligheten för virtuella datorer](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

## <a name="network-load-balancer"></a>Utjämning av nätverksbelastning
Medan en tillgänglighetsuppsättning ger programmet feltolerans, tillgängliggör belastningsutjämning många instanser av programmet på en enda nätverksadress. Flera instanser av ett program kan finnas på många virtuella datorer, var och en ansluten till en belastningsutjämnare. Eftersom programmet används dirigerar inkommande begäran över de anslutna medlemmarna i belastningsutjämnaren. En belastningsutjämnare kan läggas till med hjälp av Visual Studio guiden Lägg till ny resurs, eller genom att infoga korrekt formaterad JSON-resursen till Azure Resource Manager-mall.

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Utjämning av nätverksbelastning](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

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

Eftersom exempelprogrammet exponeras mot internet med en offentlig IP-adress och är den här adressen kopplad till belastningsutjämnaren. 

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Utjämning av nätverksbelastning kopplingen till offentliga IP-adressen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).

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

I belastningsutjämnaren översikt över Utjämning visar kopplingen till den offentliga IP-adressen från Azure-portalen.

![Utjämning av nätverksbelastning](./media/dotnet-core-4-availability-scale/nlb.png)

## <a name="load-balancer-rule"></a>Regel för belastningsutjämnare
När du använder en belastningsutjämnare, konfigureras regler som styr hur trafik fördelas över de avsedda resurserna. Med musik Store exempelprogrammet trafik anländer på port 80 på den offentliga IP-adressen och distribueras till port 80 på alla virtuella datorer. 

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Belastningsutjämningsregeln](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).

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

En vy över nätverket belastningsutjämningsregeln från portalen.

![Nätverk-regel för belastningsutjämnare](./media/dotnet-core-4-availability-scale/lbrule.png)

## <a name="load-balancer-probe"></a>Belastningsutjämningsavsökning
Belastningsutjämnaren måste också övervaka varje virtuell dator så att begäranden skickas endast till kör. Denna övervakning sker genom konstant sökning i en fördefinierad port. Musik Store-distribution är konfigurerad för att avsökning port 80 på alla virtuella datorer som ingår. 

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [belastningen belastningsutjämnaren avsökning](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257).

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

Belastningsutjämningsavsökning sett från Azure-portalen.

![Nätverk Belastningsutjämningsavsökning](./media/dotnet-core-4-availability-scale/lbprobe.png)

## <a name="inbound-nat-rules"></a>Regler för ingående NAT
När du använder en belastningsutjämnare måste regler införas som ger icke belastningen belastningsutjämnade åtkomst till varje virtuell dator. Exempelvis när du skapar en SSH-anslutning med varje virtuell dator kan den här trafiken inte bör vara belastningsutjämnad, snarare en förbestämd sökväg ska konfigureras. förinställt sökvägar konfigureras med hjälp av en resurs för inkommande NAT-regeln. Med den här resursen mappas inkommande kommunikation till enskilda virtuella datorer. 

En port som börjar vid 5000 är mappad till port 22 på varje virtuell dator för SSH-åtkomst till musik Store-programmet. Den `copyindex()` funktionen används för att öka den inkommande porten, så att den andra virtuella datorn tar emot ett inkommande port 5001, tredje 5002 och så vidare. 

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [inkommande NAT-regler](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270). 

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

Ett exempel inkommande NAT-regel som visas i Azure-portalen. En SSH NAT-regel skapas för varje virtuell dator i distributionen.

![Inkommande NAT-regel](./media/dotnet-core-4-availability-scale/natrule.png)

Detaljerad information om belastningsutjämning för Azure-nätverk finns i [belastningsutjämning för Azure infrastrukturtjänster](../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="deploy-multiple-vms"></a>Distribuera flera virtuella datorer
Slutligen för en Tillgänglighetsuppsättning eller belastningsutjämnare ska fungera effektivt kan krävs flera virtuella datorer. Flera virtuella datorer kan distribueras med hjälp av Azure Resource Manager-mall för kopieringsfunktionen. Använda kopieringsfunktionen behöver inte definiera ett begränsat antal virtuella datorer, i stället det här värdet kan erbjudas dynamiskt vid tidpunkten för distribution. Kopieringsfunktionen förbrukar antalet instanser som skapats och referenser som distribuerar rätt antal virtuella datorer och associerade resurser.

I mallen musik Store exempel definieras en parameter i ett instansantal som tar. Numret används i mallen när du skapar virtuella datorer och relaterade resurser.

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

På den virtuella datorresursen ges kopiera loop ett namn och antalet instanser parameter som används för att styra hur många resulterande kopior.

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [virtuella kopieringsfunktionen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300). 

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

Den aktuella iterationen av kopieringsfunktionen kan användas med den `copyIndex()` funktion. Värdet för kopieringsfunktionen för index kan användas för att namnge virtuella datorer och andra resurser. Till exempel måste två instanser av en virtuell dator distribueras, de olika namn. Den `copyIndex()` funktionen kan användas som en del av namnet på virtuella datorn för att skapa ett unikt namn. Ett exempel på den `copyindex()` funktion som används för namngivning kan ses i den virtuella datorresursen. Här är namnet på datorn är en sammansättning av den `vmName` parameter, och `copyIndex()` funktion. 

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Index kopieringsfunktionen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319). 

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

Den `copyIndex` funktionen används flera gånger i exempelmall musik Store. Resurser och funktioner som använder `copyIndex` innehåller allt som en enda instans av den virtuella datorn till exempel nätverksgränssnitt, regler för inläsning av belastningsutjämning, och alla beroende funktioner. 

Mer information om kopieringsfunktionen finns [skapa flera instanser av resurser i Azure Resource Manager](../../resource-group-create-multiple.md).

## <a name="next-step"></a>Nästa steg
<hr>

[Steg 4 – programdistribution med Azure Resource Manager-mallar](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

