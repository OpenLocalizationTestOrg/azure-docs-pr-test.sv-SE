---
title: "aaaAutoscale Windows Virtual Machine-Skalningsuppsättningar | Microsoft Docs"
description: "Ställa in autoskalning för en Windows Skalningsuppsättning virtuell dator med hjälp av Azure PowerShell"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: 67cf1c5063ceba4fc076dc270090defdbddbcfe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Skala automatiskt datorer i en skaluppsättning för virtuell dator
Skaluppsättningar för den virtuella datorn gör det lättare för dig toodeploy och hantera identiska virtuella datorer som en uppsättning. Skaluppsättningar ger en skalbar och anpassningsbara beräkningslager för storskaliga program och stöd för avbildningar för Windows-plattformen, Linux-plattformen bilder, anpassade avbildningar och tillägg. Läs mer om skaluppsättningar [Skalningsuppsättningarna för virtuella](virtual-machine-scale-sets-overview.md).

Den här kursen visar hur toocreate en skaluppsättning för virtuella Windows-datorer och automatiskt skala hello datorer i hello anges. Du kan skapa hello skala och skalning genom att skapa en Azure Resource Manager-mall och distribuera den med hjälp av Azure PowerShell. Mer information om mallar finns i [Redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md). toolearn mer om automatisk skalning av skalningsuppsättningar, se [automatisk skalning och Skalningsuppsättningarna för virtuella](virtual-machine-scale-sets-autoscale-overview.md).

I den här artikeln får du distribuera hello följande resurser och tillägg:

* Microsoft.Storage/storageAccounts
* Microsoft.Network/virtualNetworks
* Microsoft.Network/publicIPAddresses
* Microsoft.Network/loadBalancers
* Microsoft.Network/networkInterfaces
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.Insights.VMDiagnosticsSettings
* Microsoft.Insights/autoscaleSettings

Mer information om Resource Manager-resurser finns [Azure Resource Manager och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="step-1-install-azure-powershell"></a>Steg 1: Installera Azure PowerShell
Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) information om installation hello senaste versionen av Azure PowerShell, väljer din prenumeration och loggar in tooAzure.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Steg 2: Skapa en resursgrupp och ett lagringskonto
1. **Skapa en resursgrupp** – alla resurser måste vara distribuerad tooa resursgruppen. Använd [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) toocreate en resursgrupp med namnet **vmsstestrg1**.
2. **Skapa ett lagringskonto** – det här lagringskontot sparas om hello mallen. Använd [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) toocreate ett lagringskonto med namnet **vmsstestsa**.

## <a name="step-3-create-hello-template"></a>Steg 3: Skapa hello mall
En mall för Azure Resource Manager gör det möjligt för du toodeploy och hantera Azure-resurser tillsammans med hjälp av en JSON-beskrivning av hello resurser och associerade distributionen parametrar.

1. Skapa hello fil C:\VMSSTemplate.json i din favorit-redigeraren och lägga till hello inledande JSON strukturen toosupport hello mallen.

    ```json
    {
      "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      },
      "variables": {
      },
      "resources": [
      ]
    }
    ```

2. Parametrar krävs inte alltid, men de ger ett sätt tooinput som värden när hello mallen distribueras. Lägg till de här parametrarna under hello parametrar överordnade element som du lagt till toohello mall.

    ```json   
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
    * Ange ett namn för hello separat virtuell dator som används tooaccess hello datorer i hello skala.
    * hello namnet på hello lagringskonto där hello mall ska lagras.
    * hello antalet virtuella datorer tooinitially skapa i hello skaluppsättning.
    * hello namnet och lösenordet för administratörskontot för hello på hello virtuella datorer.
    * Ange ett namnprefix för hello-resurser som har skapats toosupport hello skala.

3. Variabler kan användas i en mall toospecify värden som ändras ofta eller värden som behöver toobe som skapats från en kombination av parametervärden. Lägg till dessa variabler under hello variabler överordnade element som du lagt till toohello mall.

    ```json
    "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
    "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
    "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
    "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
    "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
    "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
    "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
    "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
      "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```
   
    * DNS-namn som används av hello nätverksgränssnitt.

        * hello IP-adressnamn och prefix för hello virtuella nätverk och undernät.
        * hello namn och identifierare för hello virtuellt nätverk, läsa in belastningsutjämning och nätverksgränssnitt.
        * Lagringskontonamn för hello konton som är associerade med hello datorer i hello skaluppsättning.
        * Inställningar för hello diagnostik-tillägg som är installerad på hello virtuella datorer. Läs mer om hello diagnostik tillägget [skapa en Windows virtuell dator med övervakning och diagnostik med Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

4. Lägg till hello konto lagringsresurs under hello resurser överordnade element som du lagt till toohello mall. Den här mallen använder en loop toocreate hello rekommenderas fem storage-konton där hello operativsystemet diskar och diagnostiska data lagras. Denna uppsättning konton kan stödja av too100 virtuella datorer i en skaluppsättning hello aktuella högsta. Varje lagringskonto heter med en bokstav enhetsbeteckning som definierades i hello variabler i kombination med hello prefix du anger i hello parametrar för hello mall.

    ```json
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
      "apiVersion": "2015-06-15",
      "copy": {
        "name": "storageLoop",
        "count": 5
      },
      "location": "[resourceGroup().location]",
      "properties": { "accountType": "Standard_LRS" }
    },
    ```

5. Lägg till resurs för hello virtuellt nätverk. Mer information finns i [Nätverksresursprovidern](../virtual-network/resource-groups-networking.md).

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
        "subnets": [
          {
            "name": "subnet1",
            "properties": { "addressPrefix": "10.0.0.0/24" }
          }
        ]
      }
    },
    ```

6. Lägg till hello offentliga IP-adress-resurser som används av hello läsa in belastningsutjämning och nätverksgränssnittet.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP1')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName1')]"
        }
      }
    },
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP2')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName2')]"
        }
      }
    },
    ```

7. Lägg till hello belastningsutjämningsresurs som används av hello skaluppsättning. Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnaren](../load-balancer/load-balancer-arm.md).

    ```json   
    {
      "apiVersion": "2015-06-15",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
              }
            }
          }
        ],
        "backendAddressPools": [ { "name": "bepool1" } ],
        "inboundNatPools": [
          {
            "name": "natpool1",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPortRangeStart": 50000,
              "frontendPortRangeEnd": 50500,
              "backendPort": 3389
            }
          }
        ]
      }
    },
    ```

8. Lägg till hello gränssnittet nätverksresurs som används av hello separat virtuell dator. Eftersom datorer i en skaluppsättning inte är tillgängligt via en offentlig IP-adress, en separat virtuell dator skapas i hello samma virtuella nätverk tooremotely åtkomst hello datorer.

    ```json  
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
              },
              "subnet": {
                "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
              }
            }
          }
        ]
      }
    },
    ```

9. Lägg till hello separat virtuell dator i samma nätverk som hello skaluppsättning hello.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": { "vmSize": "Standard_A1" },
        "osProfile": {
          "computername": "[parameters('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2012-R2-Datacenter",
            "version": "latest"
          },
          "osDisk": {
            "name": "[concat(parameters('resourcePrefix'), 'os1')]",
            "vhd": {
              "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"        
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        }
      }
    },
    ```

10. Lägga till hello virtuella skala resursen och ange hello diagnostik tillägg som är installerad på alla virtuella datorer i hello skaluppsättning. Många av hello inställningar för den här resursen är liknande med hello virtuella datorresurser. hello viktigaste skillnaderna är hello kapacitet element som anger hello antalet virtuella datorer i hello skaluppsättning och upgradePolicy som anger hur uppdateringar görs toovirtual datorer. hello skaluppsättning inte skapas förrän alla hello storage-konton skapas som anges med hello dependsOn-element.

    ```json
    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "apiVersion": "2016-03-30",
      "name": "[parameters('vmSSName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "sku": {
        "name": "Standard_A1",
        "tier": "Standard",
        "capacity": "[parameters('instanceCount')]"
      },
      "properties": {
        "upgradePolicy": {
          "mode": "Manual"
        },
        "virtualMachineProfile": {
          "storageProfile": {
            "osDisk": {
              "vhdContainers": [
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "MicrosoftWindowsServer",
              "offer": "WindowsServer",
              "sku": "2012-R2-Datacenter",
              "version": "latest"
            }
          },
          "osProfile": {
            "computerNamePrefix": "[parameters('vmSSName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
          },
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "networkconfig1",
                "properties": {
                  "primary": "true",
                  "ipConfigurations": [
                    {
                      "name": "ip1",
                      "properties": {
                        "subnet": {
                          "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          },
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Insights.VMDiagnosticsSettings",
                "properties": {
                  "publisher": "Microsoft.Azure.Diagnostics",
                  "type": "IaaSDiagnostics",
                  "typeHandlerVersion": "1.5",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint": "https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. Lägga till hello autoscaleSettings resurs som definierar hur hello skaluppsättning justerar baserat på hello processoranvändning på hello datorer i hello skaluppsättning.

    ```json
    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Processor(_Total)\\% Processor Time",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 50.0
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      }
    }
    ```

    Dessa värden är viktiga för den här självstudiekursen:
    
    * **metricName**  
    Det här värdet är hello samma som hello prestandaräknare som vi har definierat i hello wadperfcounter variabeln. Använda den variabeln hello diagnostik tillägget samlar in hello **Processor(_Total)\% processortid** räknaren.
    
    * **metricResourceUri**  
    Det här värdet är hello resursidentifieraren för hello skaluppsättning för virtuell dator.
    
    * **Tidskorn**  
    Det här värdet är hello Granulariteten för hello mått som har samlats in. I den här mallen anges tooone minut.
    
    * **statistik**  
    Det här värdet avgör hur hello är kombinerade tooaccommodate hello autoskalning åtgärd. hello möjliga värden är: medel, Min, Max. Hello genomsnittlig total CPU-användning av hello virtuella datorer samlas in i den här mallen.
    
    * **värdet timeWindow**  
    Det här värdet är hello tidsintervall som samlas in instansdata. Det måste vara mellan 5 minuter och 12 timmar.
    
    * **timeAggregation**  
    hans värdet avgör hur hello data som samlas in ska kombineras med tiden. hello standardvärdet är medelvärdet. hello möjliga värden är:, lägsta, högsta, senaste, totalt antal, räknas.
    
    * **operatorn**  
    Det här värdet är hello operator som används toocompare hello mått data och hello tröskelvärdet. hello möjliga värden är: är lika med, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    
    * **Tröskelvärde**  
    Det här värdet är hello-värde som utlöser hello skalningsåtgärd. I den här mallen läggs datorer toohello skaluppsättningen när hello genomsnittliga processoranvändningen mellan datorer i hello uppsättningen är över 50%.
    
    * **riktning**  
    Det här värdet fastställer hello vad som händer när hello tröskelvärde uppnås. hello möjliga värden är ökning eller minskning. I den här mallen ökas hello antalet virtuella datorer i hello skaluppsättning om hello tröskelvärdet över 50% under hello definierade tidsperioden.
    
    * **typ**  
    Det här värdet är hello typ av åtgärd som ska ske och tooChangeCount måste anges.
    
    * **värdet**  
    Det här värdet är hello antalet virtuella datorer som läggs till eller tas bort från hello skaluppsättning. Det här värdet måste vara 1 eller högre. hello standardvärdet är 1. I den här mallen hello antalet datorer i hello skala in ökar med 1 när hello tröskelvärdet har uppnåtts.
    
    * **cooldown**  
    Det här värdet är hello mängden tid toowait sedan hello senaste skalning åtgärd innan hello nästa åtgärd sker. Det här värdet måste vara mellan en minut och en vecka.

12. Spara filen med hello mallen.    

## <a name="step-4-upload-hello-template-toostorage"></a>Steg 4: Överför hello mall toostorage
hello mall kan överföras så länge som du känner hello namn och primärnyckel för hello storage-konto som du skapade i steg 1.

1. Ange en variabel som anger hello namn hello storage-konto som du skapade i steg 1 i hello Microsoft Azure PowerShell-fönster.
   
    ```powershell
    $storageAccountName = "vmstestsa"
    ```

2. Ange en variabel som anger hello primärnyckel hello storage-konto.
   
    ```powershell
    $storageAccountKey = "<primary-account-key>"
    ```
   
   Du kan hämta den här nyckeln genom att klicka på nyckelikonen hello när du visar hello konto lagringsresurs i hello Azure-portalen.
3. Skapa hello storage-konto context-objektet som används toovalidate åtgärder med hello storage-konto.
   
    ```powershell
    $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
    ```

4. Skapa hello behållare för att lagra hello mall.

    ```powershell
    $containerName = "templates"
    New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob
    ```

5. Överför hello mallen filen toohello ny behållare.

    ```powershell   
    $blobName = "VMSSTemplate.json"
    $fileName = "C:\" + $BlobName
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx
    ```

## <a name="step-5-deploy-hello-template"></a>Steg 5: Distribuera hello mall
Nu när du har skapat hello mallen börja du distribuera hello resurser. Använd den här processen med kommandot toostart hello:

```powershell
New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"
```

När du trycker på Ange är du tillfrågas tooprovide värden för hello variabler som du tilldelat. Ange dessa värden:

```powershell
vmName: vmsstestvm1
  vmSSName: vmsstest1
  instanceCount: 5
  adminUserName: vmadmin1
  adminPassword: VMpass1
  resourcePrefix: vmsstest
```

Det bör ta ungefär 15 minuter för alla hello resurser toosuccessfully distribueras.

> [!NOTE]
> Du kan också använda hello portal möjlighet toodeploy hello resurser. Använd den här länken ”: https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template>”
> 
> 

## <a name="step-6-monitor-resources"></a>Steg 6: Övervaka resurser
Du kan få information om skalningsuppsättningar i virtuella datorer på följande sätt:

* hello Azure-portalen – du får för närvarande en begränsad mängd information med hjälp av hello-portalen.
* Hej [resursutforskaren Azure](https://resources.azure.com/) -verktyget är hello bästa för att utforska hello aktuell status för din skaluppsättning. Följ den här sökvägen och du bör se hello instansvyn för hello skala ange som du skapade:
  
    prenumerationer > {din prenumeration} > resursgrupper > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

* Azure PowerShell - och använda det här kommandot tooget viss information:

  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  ```
  
  Eller
  
  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView
  ```

* Ansluta toohello separat virtuella datorn precis som du skulle någon annan dator och sedan kan du fjärransluta till hello virtuella datorer i hello scale set toomonitor enskilda processer.

> [!NOTE]
> En fullständig REST-API för att få information om skaluppsättningar finns i [Skalningsuppsättningarna för virtuella datorer](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-hello-resources"></a>Steg 7: Ta bort hello resurser
Eftersom du debiteras för de resurser som används i Azure, men det är alltid en bra idé toodelete resurser som inte längre behövs. Du behöver inte toodelete varje resurs separat från en resursgrupp. Du kan ta bort hello resursgruppen och alla dess resurser tas bort automatiskt.

  ```powershell
  Remove-AzureRmResourceGroup -Name vmsstestrg1
  ```

Om du vill tookeep resursgruppen kan du ta bort hello skala endast.

  ```powershell
  Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"
  ```

## <a name="next-steps"></a>Nästa steg
* Hantera hello skaluppsättning som du precis skapat med hjälp av hello informationen i [hantera virtuella datorer i en virtuell dator Skaluppsättning](virtual-machine-scale-sets-windows-manage.md).
* Läs mer om vertikal skalning i [Vertikal automatisk skalning med skaluppsättningar för virtuella datorer](virtual-machine-scale-sets-vertical-scale-reprovision.md)
* Hitta exempel på Azure-Monitor övervakningsfunktionerna i [Azure-Monitor PowerShell snabb start prover](../monitoring-and-diagnostics/insights-powershell-samples.md)
* Lär dig mer om meddelandefunktioner i [använda automatiska åtgärder toosend e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
* Lär dig hur för[Använd granskningsloggar toosend e-post och webhook aviseringar i Azure-Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)

