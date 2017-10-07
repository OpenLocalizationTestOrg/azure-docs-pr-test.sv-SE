---
title: "aaaDeploying Linux beräkna resurser med Azure Resource Manager-mallar | Microsoft Docs"
description: "Virtuella Azure-datorn DotNet grundläggande självstudierna"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 1c4d419e-ba0e-45e4-a9dd-7ee9975a86f9
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0bc26805860fed47923d46fc84f357060f68a951
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-architecture-with-azure-resource-manager-templates-for-linux-vms"></a>Programarkitektur med Azure Resource Manager-mallar för virtuella Linux-datorer

När du utvecklar en Azure Resource Manager-distribution måste behov toobe mappas tooAzure resurser och tjänster. Om ett program består av flera http-slutpunkter, en databas och en data cachelagring service, hello Azure-resurser som är värdar för var och en av dessa komponenter måste toobe rationaliserad. Hello musik Store exempelprogrammet innehåller exempelvis ett webbprogram som är värd för en virtuell dator och en SQL-databas, som finns i Azure SQL-databas. 

Det här dokumentet beskriver hur hello musik Store beräkningsresurser konfigureras i hello Azure Resource Manager exempelmall. Alla beroenden och unika konfigurationer är markerade. Distribuera en instans av hello lösning tooyour Azure-prenumeration och fungerar tillsammans med hello Azure Resource Manager-mall för hello bästa möjliga resultat. hello fullständig mallen hittar du här – [musik Store distributionen på Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux). 

## <a name="virtual-machine"></a>Virtuell dator
hello musik Store-programmet innehåller ett webbprogram där kunder kan bläddra bland och köpa musik. Det finns flera Azure-tjänster som kan vara värd för webbprogram i det här exemplet används en virtuell dator. Använder hello exempelmall musik Store, en virtuell dator distribueras, installera en webbserver och hello musik Store webbplats installeras och konfigureras. För hello ut i den här artikeln beskrivs bara distributionen av hello virtuella datorer. hello konfigurationen av hello webbservern och hello program beskrivs i en senare artikel.

En virtuell dator kan läggas till tooa mallen med hjälp av hello Visual Studio Lägg till ny resurs guiden, eller genom att infoga giltig JSON i hello Distributionsmall. När du distribuerar en virtuell dator krävs också flera relaterade resurser. Om du använder Visual Studio toocreate hello mall skapas dessa resurser. Om manuellt konstruera hello mall, måste dessa resurser toobe infogas och konfigurerats.

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [virtuella JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L295).

```json
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

När har distribuerats, kan hello egenskaper för virtuell dator ses i hello Azure-portalen.

![Virtuell dator](./media/dotnet-core-2-architecture/vm.png)

## <a name="storage-account"></a>Lagringskonto
Storage-konton har många lagringsalternativ och funktioner. Hello-kontexten av Azure virtuella datorer innehåller ett lagringskonto hello virtuella hårddiskar för hello virtuell dator och eventuella ytterligare hårddiskar. hello musik Store exempel innehåller en lagring konto toohold hello virtuella hårddisken på varje virtuell dator i hello-distribution. 

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Lagringskonto](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L109).

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

Ett lagringskonto är associerat med en virtuell dator i hello Resource Manager-mall deklarationen av hello virtuell dator. 

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [virtuella datorn och Storage-konto associationen](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L341).

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

Efter distributionen kan hello storage-konto visas i hello Azure-portalen.

![Lagringskonto](./media/dotnet-core-2-architecture/storacct.png)

Klicka i hello lagringsblobbehållare för kontot visas hello virtuell hårddiskfil för varje virtuell dator distribueras med hello mallen.

![Virtuella hårddiskar](./media/dotnet-core-2-architecture/vhd.png)

Mer information om Azure Storage finns [Azure Storage-dokumentationen](https://azure.microsoft.com/documentation/services/storage/).

## <a name="virtual-network"></a>Virtual Network
Om en virtuell dator kräver interna nätverk, till exempel hello möjlighet toocommunicate med andra virtuella datorer och Azure-resurser, krävs ett Azure Virtual Network.  Ett virtuellt nätverk gör inte hello virtuella datorn nås över hello internet. Anslutning för offentliga kräver en offentlig IP-adress som beskrivs senare i den här serien.

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [virtuella nätverk och undernät](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L136).

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

Från hello Azure-portalen, hello virtuellt nätverk ser ut som hello följande bild. Observera att alla virtuella datorer som distribueras med hello mallen är anslutna toohello virtuellt nätverk.

![Virtual Network](./media/dotnet-core-2-architecture/vnet.png)

## <a name="network-interface"></a>Nätverksgränssnitt
 Ett nätverksgränssnitt ansluter en virtuell dator tooa virtuellt nätverk, mer specifikt tooa undernät som har definierats i hello virtuellt nätverk. 

 Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [nätverksgränssnittet](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L166).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceNamePrefix'), copyindex())]",
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
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'SSH-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig1",
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
              "id": "[concat(variables('lbID'),'/inboundNatRules/SSH-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

Varje virtuell datorresurs innehåller en nätverksprofil. hello nätverksgränssnittet är kopplat till hello virtuell dator i den här profilen.  

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [virtuella nätverksprofil](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L350).

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceNamePrefix'), copyindex()))]"
    }
  ]
}
```

Från hello Azure-portalen, hello nätverksgränssnittet ser ut som hello följande bild. hello interna IP-adress och hello virtuella koppling kan ses på hello nätverksresurs gränssnitt.

![Nätverksgränssnitt](./media/dotnet-core-2-architecture/nic.png)

Mer information om virtuella Azure-nätverk finns [dokumentation för Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>Azure SQL Database
Dessutom är tooa virtuella datorn är värd för hello musik Store webbplats, Azure SQL-databas distribuerade toohost hello musik Store-databas. hello fördelen med att använda Azure SQL Database här är att en annan uppsättning virtuella datorer är inte obligatoriska och tillgänglighet och skala är inbyggd i hello-tjänsten.

En Azure SQL database kan läggas till med hello Visual Studio Lägg till ny resurs guiden, eller genom att infoga giltig JSON i en mall. hello SQL Server-resurs innehåller ett användarnamn och lösenord som har beviljats administratörsbehörighet på hello SQL-instansen. Dessutom läggs en resurs för SQL-brandväggen. Som standard är program som finns i Azure kan tooconnect med hello SQL-instans. tooallow externt program sådana en SQL Server Management studio tooconnect toohello SQL-instans, hello brandvägg måste toobe konfigurerats. Hello standardkonfigurationen är bra för hello musik Store demo hello ut. 

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L401).

```json
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicStoreSqlName')]",
  "location": "[resourceGroup().location]",

  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('sqlAdminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicStoreSqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

En vy över hello SQLServer- och MusicStore som visas i hello Azure-portalen.

![SQL Server](./media/dotnet-core-2-architecture/sql.png)

Mer information om hur du distribuerar Azure SQL Database finns [dokumentation för Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Nästa steg
<hr>

[Steg 2 – åtkomst och säkerhet i Azure Resource Manager-mallar](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

