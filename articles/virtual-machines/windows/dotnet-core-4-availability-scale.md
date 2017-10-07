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
# <a name="availability-and-scale-in-azure-resource-manager-templates-for-windows-vms"></a>Tillgänglighet och skala i Azure Resource Manager-mallar för virtuella Windows-datorer

Tillgänglighet och skala finns toouptime och hello möjlighet toomeet efterfrågan. Om ett program måste vara in 99,9% av hello tid, måste det toohave en arkitektur som gör det möjligt för flera samtidiga beräkningsresurser. I stället för med en webbplats, innehåller en konfiguration med en högre säkerhetsnivå för tillgänglighet till exempel flera instanser av samma plats, med belastningsutjämning teknik framför dem hello. I den här konfigurationen är kan en instans av programmet hello stängas av för underhåll, medan hello återstående fortsätter toofunction. Skala på hello andra sidan refererar tooan program möjlighet tooserve begäran. Med belastning kan belastningsutjämnade program, lägga till eller ta bort instanser från hello pool ett program tooscale toomeet begäran.

Det här dokumentet beskriver hur hello musik Store exempeldistribution har konfigurerats för tillgänglighet och skala. Alla beroenden och unika konfigurationer är markerade. Distribuera en instans av hello lösning tooyour Azure-prenumeration och fungerar tillsammans med hello Azure Resource Manager-mall för hello bästa möjliga resultat. hello fullständig mallen hittar du här – [musik Store distributionen på Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="availability-set"></a>Tillgänglighetsuppsättning
En Tillgänglighetsuppsättning täcker logiskt Azure Virtual Machines fysiska värdar och andra infrastrukturella komponenter, till exempel strömkällor och maskinvara för fysiska nätverk. Tillgänglighetsuppsättningar se till att inte alla virtuella datorer sker under underhåll, enhet eller annan stillestånd. En Tillgänglighetsuppsättning kan läggas tooan Azure Resource Manager-mallen använder hello Visual Studio guiden Lägg till ny resurs, eller lägga till giltig JSON i en mall.

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Tillgänglighetsuppsättning](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L368).

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

En Tillgänglighetsuppsättning har deklarerats som en egenskap för en virtuell datorresurs. 

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Tillgänglighetsuppsättning association med den virtuella datorn](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L302).

```json
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
Hej tillgänglighetsuppsättning sett från hello Azure-portalen. Varje virtuell dator och information om hello konfiguration beskrivs här.

![Tillgänglighetsuppsättning](./media/dotnet-core-4-availability-scale/ase-win.png)

Detaljerad information om Tillgänglighetsuppsättningar finns [hantera tillgängligheten för virtuella datorer](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="network-load-balancer"></a>Utjämning av nätverksbelastning
Medan en tillgänglighetsuppsättning ger programmet feltolerans, gör belastningsutjämning många instanser av programmet hello tillgängliga i en enda nätverksadress. Flera instanser av ett program kan finnas på många virtuella datorer, var och en ansluten tooa belastningsutjämnaren. Eftersom programmet hello används hello hello belastningen belastningsutjämnaren vägar inkommande begäran över hello kopplade medlemmar. En belastningsutjämnare kan läggas till med hello Visual Studio guiden Lägg till ny resurs, eller genom att infoga korrekt formaterad JSON-resurs i hello Azure Resource Manager-mall.

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Utjämning av nätverksbelastning](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L198).

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

Eftersom hello exempelprogrammet är utsatta toohello internet med en offentlig IP-adress, den här adressen är kopplad till hello belastningsutjämnaren. 

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Utjämning av nätverksbelastning kopplingen till offentliga IP-adressen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).

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

Från hello Azure-portalen visar hello översikt över Utjämning belastningsutjämnaren hello association med hello offentlig IP-adress.

![Utjämning av nätverksbelastning](./media/dotnet-core-4-availability-scale/nlb-win.png)

## <a name="load-balancer-rule"></a>Regel för belastningsutjämnare
När du använder en belastningsutjämnare, konfigureras regler som styr hur trafik fördelas över hello avsedda resurserna. Med hello för musik Store exempelprogrammet trafik anländer på port 80 på hello offentlig IP-adress och distribueras till port 80 på alla virtuella datorer. 

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Belastningsutjämningsregeln](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L226).

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

En vy över nätverket hello belastningsutjämningsregeln från hello-portalen.

![Nätverk-regel för belastningsutjämnare](./media/dotnet-core-4-availability-scale/lbrule-win.png)

## <a name="load-balancer-probe"></a>Belastningsutjämningsavsökning
hello belastningsutjämnaren måste också toomonitor varje virtuell dator så att begäranden hanteras endast toorunning system. Denna övervakning sker genom konstant sökning i en fördefinierad port. hello är musik Store konfigurerade tooprobe port 80 på alla med virtuella datorer. 

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [belastningen belastningsutjämnaren avsökning](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L247).

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

Hej belastningsutjämningsavsökning sett från hello Azure-portalen.

![Nätverk Belastningsutjämningsavsökning](./media/dotnet-core-4-availability-scale/lbprobe-win.png)

## <a name="inbound-nat-rules"></a>Regler för ingående NAT
När du använder en belastningsutjämnare regler måste toobe inrätta som tillhandahåller icke belastningen belastningsutjämnade åtkomst tooeach virtuella datorn. Exempelvis när du skapar en RDP-anslutning med varje virtuell dator kan den här trafiken inte bör vara belastningsutjämnad, snarare en förbestämd sökväg ska konfigureras. förinställt sökvägar konfigureras med hjälp av en resurs för inkommande NAT-regeln. Med den här resursen vara inkommande kommunikation mappade tooindividual virtuella datorer. 

Med hello musik Store-programmet är en port som börjar vid 5000 mappade tooport 3389 på varje virtuell dator för RDP-åtkomst. Hej `copyindex()` funktionen är används tooincrement Hej inkommande port, så att hello andra virtuell dator tar emot ett inkommande port 5001 hello tredje 5002 och så vidare.

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [inkommande NAT-regler](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L260). 

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

Ett exempel inkommande NAT-regel som visas i hello Azure-portalen. En RDP-NAT-regel skapas för varje virtuell dator i hello-distribution.

![Inkommande NAT-regel](./media/dotnet-core-4-availability-scale/natrule-win.png)

Detaljerad information om hello Azure Utjämning av nätverksbelastning finns [belastningsutjämning för Azure infrastrukturtjänster](load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="deploy-multiple-vms"></a>Distribuera flera virtuella datorer
Slutligen för en Tillgänglighetsuppsättning eller belastningsutjämnare tooeffectively funktion krävs flera virtuella datorer. Flera virtuella datorer kan distribueras med hjälp av hello Azure Resource Manager-mall för kopieringsfunktionen. Använder hello kopieringsfunktionen, är det inte nödvändigt toodefine ett begränsat antal virtuella datorer, i stället det här värdet kan erbjudas dynamiskt vid hello tiden för distributionen. hello kopieringsfunktionen förbrukar hello antalet instanser toobe skapas och referenser som distribuerar hello rätt antal virtuella datorer och associerade resurser.

I hello musik Store exempelmall definieras en parameter som tar i instansantal. Numret används i hela hello mallen när du skapar virtuella datorer och relaterade resurser.

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

Hello kopiera loop ges ett namn på hello virtuell datorresurs, och hello antal instanser parametern används toocontrol hello antal resulterande kopior.

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [virtuella kopieringsfunktionen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L290). 

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

hello aktuella iteration av hello kopieringsfunktionen kan nås med hello `copyIndex()` funktion. hello-värdet för hello kopieringsfunktionen indexet kan vara används tooname virtuella datorer och andra resurser. Till exempel måste två instanser av en virtuell dator distribueras, de olika namn. Hej `copyIndex()` funktionen kan användas som en del av hello virtuella namn toocreate ett unikt namn. Ett exempel på hello `copyindex()` funktion som används för namngivning kan ses i hello virtuella datorresurser. Här hello datornamnet är en sammansättning av hello `vmName` parametern och hello `copyIndex()` funktion. 

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Index kopieringsfunktionen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L309). 

```json
"osProfile": {
  "computerName": "[concat(variables('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "adminPassword": "[parameters('adminPassword')]"
}
```

Hej `copyIndex` funktionen används flera gånger i hello musik Store exempelmall. Resurser och funktioner som använder `copyIndex` innehåller något annat specifika tooa instans av hello virtuella sådana och nätverksgränssnitt, regler för inläsning av belastningsutjämning, och alla beroende funktioner. 

Mer information om hello kopieringsfunktionen finns [skapa flera instanser av resurser i Azure Resource Manager](../../resource-group-create-multiple.md).

## <a name="next-step"></a>Nästa steg
<hr>

[Steg 4 – programdistribution med Azure Resource Manager-mallar](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

