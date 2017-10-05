---
title: "Åtkomst och säkerhet i Azure-mallar för virtuella Windows-datorer | Microsoft Docs"
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
ms.openlocfilehash: ad1b5c4763cf56f681a50bb1bccc825311bbfdf5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="access-and-security-in-azure-resource-manager-templates-for-windows-vms"></a>Åtkomst och säkerhet i Azure Resource Manager-mallar för virtuella Windows-datorer

Program som finns i Azure sannolikt måste vara åtkomst via internet eller ett VPN / Expressroute-anslutningen med Azure. Med hjälp av programmet exempel musik Store görs webbplatsen tillgänglig på internet med en offentlig IP-adress. Med åtkomst upprätta skyddas anslutningar till programmet och åtkomst till de virtuella datorresurser själva. Den här åtkomstsäkerhet ingår i en Nätverkssäkerhetsgrupp. 

Det här dokumentet beskriver hur musik Store-programmet skyddas i Azure Resource Manager exempelmall. Alla beroenden och unika konfigurationer är markerade. Distribuera en instans av lösningen till din Azure-prenumeration och fungerar tillsammans med Azure Resource Manager-mall för bästa resultat. Fullständig mallen hittar du här – [musik Store distributionen på Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="public-ip-address"></a>Offentlig IP-adress
Om du vill ge offentlig åtkomst till en Azure-resurs, kan du använda en offentlig IP-adressresurs. Offentlig IP-adress kan konfigureras med en statisk eller dynamisk IP-adress. Om en dynamisk adress används, och den virtuella datorn stoppas och frigöra, adresser tas bort. När datorn startas igen, kan den tilldelas en annan offentlig IP-adress. En reserverad IP-adress kan användas för att förhindra att en IP-adress ändras. 

En offentlig IP-adress kan läggas till en Azure Resource Manager-mallen med hjälp av Visual Studio guiden Lägg till ny resurs eller genom att infoga giltig JSON i en mall. 

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [offentliga IP-adressen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L110).

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

En offentlig IP-adress kan associeras med ett virtuellt nätverkskort eller en belastningsutjämnare. I det här exemplet eftersom webbplatsen musik Store belastningsutjämnas mellan flera virtuella datorer, är den offentliga IP-adressen kopplad till de belastningsutjämnare.

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [kopplingen offentlig IP-adress till belastningsutjämnaren](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).

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

Offentliga IP-adressen som visas i Azure Portal. Observera att den offentliga IP-adressen är kopplad till en belastningsutjämnare och inte en virtuell dator. Belastningsutjämning för nätverk beskrivs i nästa dokument av den här serien.

![Offentlig IP-adress](./media/dotnet-core-3-access-security/pubip-win.png)

Mer information om Azure offentliga IP-adresser finns [IP-adresser i Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Nätverkssäkerhetsgrupp
När åtkomst har upprättats till Azure-resurser, begränsas åtkomst. För virtuella Azure-datorer möjliggörs säker åtkomst med en nätverkssäkerhetsgrupp. Med hjälp av programmet exempel musik Store begränsas all åtkomst till den virtuella datorn utom för via port 80 för HTTP-åtkomst och port 3389 för RDP-åtkomst. En Nätverkssäkerhetsgrupp kan läggas till en Azure Resource Manager-mallen med hjälp av Visual Studio guiden Lägg till ny resurs eller genom att infoga giltig JSON i en mall.

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Nätverkssäkerhetsgruppen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L57).

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

I det här exemplet är nätverkssäkerhetsgruppen associeras med undernätverksobjektet som deklarerats i resursen virtuellt nätverk. 

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Nätverkssäkerhetsgruppen association med ett virtuellt nätverk](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L143).

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

Här är nätverkssäkerhetsgruppen ser ut från Azure-portalen. Observera att en NSG som kan associeras med ett undernät och / eller nätverket gränssnitt. I det här fallet är NSG: N kopplad till ett undernät. I den här konfigurationen inkommande reglerna som gäller för alla virtuella datorer som är anslutna till undernätet.

![Nätverkssäkerhetsgrupp](./media/dotnet-core-3-access-security/nsg-win.png)

Detaljerad information om Nätverkssäkerhetsgrupper finns [vad är en Nätverkssäkerhetsgrupp](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Nästa steg
<hr>

[Steg 3 – tillgänglighet och skala i Azure Resource Manager-mallar](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

