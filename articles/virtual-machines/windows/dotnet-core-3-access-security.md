---
title: "aaaAccess och säkerhet i Azure-mallar för virtuella Windows-datorer | Microsoft Docs"
description: "Virtuella Azure-datorn DotNet grundläggande självstudierna"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: e671fc45-5e4d-40fd-aac5-290892713cc0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4b8227ae745b3b0a22d136e98d18479f8b1504c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-and-security-in-azure-resource-manager-templates-for-windows-vms"></a>Åtkomst och säkerhet i Azure Resource Manager-mallar för virtuella Windows-datorer

Program finns i Azure förmodligen behöver toobe åtkomst via hello internet eller ett VPN / Expressroute-anslutningen med Azure. Med hello musik Store programmet exempel hello webbplats görs tillgängliga på hello internet med en offentlig IP-adress. Med åtkomst upprätta ska anslutningar toohello program- och toohello virtuella datorresurser själva skyddas. Den här åtkomstsäkerhet ingår i en Nätverkssäkerhetsgrupp. 

Det här dokumentet beskriver hur hello musik Store-programmet skyddas i Azure Resource Manager för hello exempelmall. Alla beroenden och unika konfigurationer är markerade. Distribuera en instans av hello lösning tooyour Azure-prenumeration och fungerar tillsammans med hello Azure Resource Manager-mall för hello bästa möjliga resultat. hello fullständig mallen hittar du här – [musik Store distributionen på Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="public-ip-address"></a>Offentlig IP-adress
tooprovide offentlig åtkomst tooan Azure-resurs, en offentlig IP-adressresurs kan användas. Offentlig IP-adress kan konfigureras med en statisk eller dynamisk IP-adress. Om en dynamisk adress används och hello virtuella datorn stoppas och frigöra, hello adresser tas bort. När hello dator startas igen, kan den tilldelas en annan offentlig IP-adress. tooprevent en IP-adress från ändrar, en reserverad IP-adress kan användas. 

Tooan Azure Resource Manager-mallen med hjälp av hello Visual Studio guiden Lägg till ny resurs, kan läggas till en offentlig IP-adress eller genom att infoga giltig JSON i en mall. 

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [offentliga IP-adressen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L110).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicIpAddressName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "public-ip"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

En offentlig IP-adress kan associeras med ett virtuellt nätverkskort eller en belastningsutjämnare. I det här exemplet är hello offentliga IP-adressen bifogade toohello belastningsutjämnare eftersom hello musik Store webbplats belastningsutjämnas mellan flera virtuella datorer.

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [kopplingen offentlig IP-adress till belastningsutjämnaren](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).

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

hello sett offentliga IP-adress som från hello Azure-portalen. Observera att hello offentliga IP-adressen är kopplad tooa belastningsutjämnare och inte en virtuell dator. Belastningsutjämning för nätverk beskrivs i nästa hello-dokument av den här serien.

![Offentlig IP-adress](./media/dotnet-core-3-access-security/pubip-win.png)

Mer information om Azure offentliga IP-adresser finns [IP-adresser i Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Nätverkssäkerhetsgrupp
När åtkomst har etablerat tooAzure resurser, begränsas åtkomst. För virtuella Azure-datorer möjliggörs säker åtkomst med en nätverkssäkerhetsgrupp. Med hello musik Store programmet exempel begränsas alla åtkomst toohello virtuella datorn utom för via port 80 för HTTP-åtkomst och port 3389 för RDP-åtkomst. En Nätverkssäkerhetsgrupp kan läggas till tooan Azure Resource Manager-mallen med hjälp av hello Visual Studio guiden Lägg till ny resurs, eller genom att infoga giltig JSON i en mall.

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Nätverkssäkerhetsgruppen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L57).

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('networkSecurityGroup')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-security-group"
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
},
```

I det här exemplet är hello nätverkssäkerhetsgruppen associeras med hello undernätsobjekt som deklarerats i hello virtuella nätverksresurs. 

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Nätverkssäkerhetsgruppen association med ett virtuellt nätverk](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L143).

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
]
```

Det här är vad hello nätverkssäkerhetsgruppen ser ut som från hello Azure-portalen. Observera att en NSG som kan associeras med ett undernät och / eller nätverket gränssnitt. I det här fallet är hello NSG associerade tooa undernät. I den här konfigurationen hello inkommande regler gäller tooall virtuella datorer anslutna toohello undernät.

![Nätverkssäkerhetsgrupp](./media/dotnet-core-3-access-security/nsg-win.png)

Detaljerad information om Nätverkssäkerhetsgrupper finns [vad är en Nätverkssäkerhetsgrupp](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Nästa steg
<hr>

[Steg 3 – tillgänglighet och skala i Azure Resource Manager-mallar](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

