---
title: "Distribuera Windows beräkningsresurser med Azure Resource Manager-mallar | Microsoft Docs"
description: "Virtuella Azure-datorn DotNet grundläggande självstudierna"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: b026fe81-1bc1-4899-ac32-886091671498
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8a8b888195e52ea9669922a6a00a873025f3c375
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="application-architecture-with-azure-resource-manager-templates-for-windows-vms"></a>Programarkitektur med Azure Resource Manager-mallar för virtuella Windows-datorer

När du utvecklar en Azure Resource Manager-distribution för att beräkna krav måste mappas till Azure-resurser och tjänster. Om ett program består av flera http-slutpunkter, en databas och en data cachelagring service, Azure-resurser som är värdar för varje för dessa komponenter måste vara rationaliserad. Musik Store exempelprogrammet innehåller exempelvis ett webbprogram som är värd för en virtuell dator och en SQL-databas, som finns i Azure SQL-databas. 

Det här dokumentet beskriver hur beräkningsresurser musik Store har konfigurerats i Azure Resource Manager exempelmall. Alla beroenden och unika konfigurationer är markerade. Distribuera en instans av lösningen till din Azure-prenumeration och fungerar tillsammans med Azure Resource Manager-mall för bästa resultat. Fullständig mallen hittar du här – [musik Store distributionen på Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="virtual-machine"></a>Virtuell dator
Musik Store-programmet innehåller ett webbprogram där kunder kan bläddra bland och köpa musik. Det finns flera Azure-tjänster som kan vara värd för webbprogram i det här exemplet används en virtuell dator. Med mallen musik Store exempel kan en virtuell dator distribueras, installera en webbserver och musik Store webbplats installeras och konfigureras. För den här artikeln beskrivs bara distributionen av virtuella datorer. Konfigurationen av webbservern och programmet beskrivs i en senare artikel.

En virtuell dator kan läggas till en mall med hjälp av Visual Studio Lägg till ny resurs-guiden eller genom att infoga giltig JSON i mallen för distribution. När du distribuerar en virtuell dator krävs också flera relaterade resurser. Om du använder Visual Studio för att skapa mallen, är dessa resurser skapas. Om hur du manuellt skapar mallen, måste dessa resurser infogas och konfigureras.

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [virtuella JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).

```json
{
  {
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
  ........<truncated>  
}
```

Distribution kan kan egenskaperna för virtuella datorn ses i Azure-portalen.

![Virtuell dator](./media/dotnet-core-2-architecture/vm-win.png)

## <a name="storage-account"></a>Storage-konto
Storage-konton har många lagringsalternativ och funktioner. Ett lagringskonto innehåller virtuella hårddiskar på den virtuella datorn och eventuella ytterligare hårddiskar i kontexten av Azure virtuella datorer. Musik Store-exempel innehåller ett lagringskonto för att lagra den virtuella hårddisken på varje virtuell dator i distributionen. 

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Lagringskonto](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

Ett lagringskonto är associerat med en virtuell dator i Resource Manager mallen deklarationen av den virtuella datorn. 

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [virtuella datorn och Storage-konto associationen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).

```json
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

Efter distributionen kan storage-konto visas i Azure-portalen.

![Storage-konto](./media/dotnet-core-2-architecture/storacct-win.png)

Klicka i lagringsblobbehållare för kontot visas virtuell hårddiskfil för varje virtuell dator distribueras med hjälp av mallen.

![Virtuella hårddiskar](./media/dotnet-core-2-architecture/vhd-win.png)

Mer information om Azure Storage finns [Azure Storage-dokumentationen](https://azure.microsoft.com/documentation/services/storage/).

## <a name="virtual-network"></a>Virtual Network
Om en virtuell dator kräver interna nätverk, till exempel möjligheten att kommunicera med andra virtuella datorer och Azure-resurser, krävs ett Azure Virtual Network.  Ett virtuellt nätverk gör inte den virtuella datorn tillgänglig via internet. Anslutning för offentliga kräver en offentlig IP-adress som beskrivs senare i den här serien.

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [virtuella nätverk och undernät](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
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
  }
}
```

Från Azure-portalen virtuellt nätverk ser ut som följande bild. Observera att alla virtuella datorer som distribueras med hjälp av mallen är anslutna till det virtuella nätverket.

![Virtual Network](./media/dotnet-core-2-architecture/vnet-win.png)

## <a name="network-interface"></a>Nätverksgränssnitt
 Ett nätverksgränssnitt ansluter en virtuell dator till ett virtuellt nätverk, mer specifikt till ett undernät som har definierats i det virtuella nätverket. 

 Följ länken för att se JSON-exemplet i Resource Manager-mallen – [nätverksgränssnittet](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceName'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'RDP-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[variables('subnetRef')]"
          },
          "loadBalancerBackendAddressPools": [
            {
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

Varje virtuell datorresurs innehåller en nätverksprofil. Nätverksgränssnittet är kopplat till den virtuella datorn i den här profilen.  

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [virtuella nätverksprofil](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330).

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

Från Azure-portalen nätverksgränssnittet ser ut som följande bild. Den interna IP-adressen och virtuella associationen kan ses på gränssnittet nätverksresurs.

![Nätverksgränssnitt](./media/dotnet-core-2-architecture/nic-win.png)

Mer information om virtuella Azure-nätverk finns [dokumentation för Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>Azure SQL Database
Förutom en virtuell dator som värd för webbplatsen musik Store distribueras en Azure SQL Database som värd för musik Store-databasen. Fördelen med att använda Azure SQL Database här är att en annan uppsättning virtuella datorer är inte obligatoriska och tillgänglighet och skala är inbyggd i tjänsten.

En Azure SQL database kan läggas till med Visual Studio lägga till nya resursen guiden, eller genom att infoga giltig JSON i en mall. SQL Server-resurs innehåller ett användarnamn och lösenord som har beviljats administratörsbehörighet på SQL-instansen. Dessutom läggs en resurs för SQL-brandväggen. Som standard är program som finns i Azure kan ansluta till SQL-instansen. Sådana en SQL Server Management studio att ansluta till SQL-instansen brandväggen måste konfigureras för att tillåta externa program. Standardkonfigurationen är bra för enkelhetens musik Store-demonstrationen. 

Följ länken för att se JSON-exemplet i Resource Manager-mallen – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).

```json
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicstoresqlName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('adminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicstoresqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

En vy av SQLServer och MusicStore databas som visas i Azure-portalen.

![SQL Server](./media/dotnet-core-2-architecture/sql-win.png)

Mer information om hur du distribuerar Azure SQL Database finns [dokumentation för Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Nästa steg
<hr>

[Steg 2 – åtkomst och säkerhet i Azure Resource Manager-mallar](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

