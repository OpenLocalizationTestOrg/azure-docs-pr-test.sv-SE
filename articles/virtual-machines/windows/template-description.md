---
title: aaaVirtual datorer i en Azure Resource Manager-mall | Microsoft Azure
description: "Läs mer om hur hello virtuell datorresurs har definierats i en Azure Resource Manager-mall."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f63ab5cc-45b8-43aa-a4e7-69dc42adbb99
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: davidmu
ms.openlocfilehash: 94adcbe5bf44be72ffc1b920461aed15c4fc025f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machines-in-an-azure-resource-manager-template"></a>Virtuella datorer i en Azure Resource Manager-mall

Den här artikeln beskriver aspekter av en Azure Resource Manager-mall som gäller toovirtual datorer. Den här artikeln beskriver inte en fullständig mall för att skapa en virtuell dator; för att du behöver resursdefinitionerna för storage-konton, nätverksgränssnitt, offentliga IP-adresser och virtuella nätverk. Mer information om hur dessa resurser kan definieras tillsammans finns hello [genomgång av Resource Manager-mall](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Det finns många [mallar i hello galleriet](https://azure.microsoft.com/documentation/templates/?term=VM) som inkluderar hello Virtuella datorresursen. Här beskrivs inte alla element som kan ingå i en mall.

Det här exemplet illustrerar en typisk resursavsnitt i en mall för att skapa ett angivet antal virtuella datorer:

```json
"resources": [
  { 
    "apiVersion": "2016-04-30-preview", 
    "type": "Microsoft.Compute/virtualMachines", 
    "name": "[concat('myVM', copyindex())]", 
    "location": "[resourceGroup().location]",
    "copy": {
      "name": "virtualMachineLoop", 
      "count": "[parameters('numberOfInstances')]"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/myNIC', copyindex())]" 
    ], 
    "properties": { 
      "hardwareProfile": { 
        "vmSize": "Standard_DS1" 
      }, 
      "osProfile": { 
        "computername": "[concat('myVM', copyindex())]", 
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
          "name": "[concat('myOSDisk', copyindex())]",
          "caching": "ReadWrite", 
          "createOption": "FromImage" 
        },
        "dataDisks": [
          {
            "name": "[concat('myDataDisk', copyindex())]",
            "diskSizeGB": "100",
            "lun": 0,
            "createOption": "Empty"
          }
        ] 
      }, 
      "networkProfile": { 
        "networkInterfaces": [ 
          { 
            "id": "[resourceId('Microsoft.Network/networkInterfaces',
              concat('myNIC', copyindex()))]" 
          } 
        ] 
      },
      "diagnosticsProfile": {
        "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('https://', variables('storageName'), '.blob.core.windows.net')]"
        }
      } 
    },
    "resources": [ 
      { 
        "name": "Microsoft.Insights.VMDiagnosticsSettings", 
        "type": "extensions", 
        "location": "[resourceGroup().location]", 
        "apiVersion": "2016-03-30", 
        "dependsOn": [ 
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
        ], 
        "properties": { 
          "publisher": "Microsoft.Azure.Diagnostics", 
          "type": "IaaSDiagnostics", 
          "typeHandlerVersion": "1.5", 
          "autoUpgradeMinorVersion": true, 
          "settings": { 
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
            variables('wadmetricsresourceid'), 
            concat('myVM', copyindex()),
            variables('wadcfgxend')))]", 
            "storageAccount": "[variables('storageName')]" 
          }, 
          "protectedSettings": { 
            "storageAccountName": "[variables('storageName')]", 
            "storageAccountKey": "[listkeys(variables('accountid'), 
              '2015-06-15').key1]", 
            "storageAccountEndPoint": "https://core.windows.net" 
          } 
        } 
      },
      {
        "name": "MyCustomScriptExtension",
        "type": "extensions",
        "apiVersion": "2016-03-30",
        "location": "[resourceGroup().location]",
        "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]"
        ],
        "properties": {
          "publisher": "Microsoft.Compute",
          "type": "CustomScriptExtension",
          "typeHandlerVersion": "1.7",
          "autoUpgradeMinorVersion": true,
          "settings": {
            "fileUris": [
              "[concat('https://', variables('storageName'),
                '.blob.core.windows.net/customscripts/start.ps1')]" 
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
          }
        }
      } 
    ]
  } 
]
``` 

> [!NOTE] 
>Det här exemplet bygger på ett lagringskonto som skapades tidigare. Du kan skapa hello storage-konto genom att distribuera den från hello mall. hello exempel använder också ett nätverksgränssnitt och dess beroende resurser som skulle definieras i hello mallen. Dessa resurser visas inte i hello exempel.
>
>

## <a name="api-version"></a>API-Version

När du distribuerar resurser med hjälp av en mall har toospecify en version av hello API toouse. hello exempel visar hello virtuell datorresurs med den här apiVersion-element:

```
"apiVersion": "2016-04-30-preview",
```

hello-versionen av hello API som du anger i mallen påverkar vilka egenskaper som du kan definiera i hello mallen. I allmänhet bör du välja hello senaste API-versionen när du skapar mallar. För befintliga mallar, kan du bestämma om du vill toocontinue med en tidigare API-version eller uppdatera mallen för hello senaste version tootake nytta av nya funktioner.

Använd dessa möjligheter för att hämta hello senaste API-versioner:

- REST API - [lista över alla resursproviders](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)
- PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)
- Azure CLI 2.0 - [az provider show](https://docs.microsoft.com/cli/azure/provider#show)

## <a name="parameters-and-variables"></a>Parametrar och variabler

[Parametrarna](../../resource-group-authoring-templates.md) gör det lättare för dig toospecify värden för hello mallen när den körs. Det här avsnittet parametrar används i hello exemplet:

```        
"parameters": {
  "adminUsername": { "type": "string" },
  "adminPassword": { "type": "securestring" },
  "numberOfInstances": { "type": "int" }
},
```

När du distribuerar hello exempelmall ange värden för hello namn och lösenord för administratörskontot för hello på varje virtuell dator och hello antal virtuella datorer toocreate. Du har hello alternativet för att ange parametervärden i en separat fil som hanteras med hello mall eller tillhandahåller värden när du uppmanas.

[Variabler](../../resource-group-authoring-templates.md) gör det lättare för dig tooset värden i hello mallen som används flera gånger i den eller som kan ändras med tiden. Det här avsnittet för variabler som används i hello exemplet:

```
"variables": { 
  "storageName": "mystore1",
  "accountid": "[concat('/subscriptions/', subscription().subscriptionId, 
    '/resourceGroups/', resourceGroup().name,
  '/providers/','Microsoft.Storage/storageAccounts/', variables('storageName'))]", 
  "wadlogs": "<WadCfg> 
    <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> 
      <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> 
      <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > 
        <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> 
        <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> 
        <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" />
      </WindowsEventLog>", 
  "wadperfcounters": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\">
      <PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\">
        <annotation displayName=\"Threads\" locale=\"en-us\"/>
      </PerformanceCounterConfiguration>
    </PerformanceCounters>", 
  "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters'), 
    '<Metrics resourceId=\"')]", 
  "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, 
    '/resourceGroups/', resourceGroup().name , 
    '/providers/', 'Microsoft.Compute/virtualMachines/')]", 
  "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/>
    <MetricAggregation scheduledTransferPeriod=\"PT1M\"/>
    </Metrics></DiagnosticMonitorConfiguration>
    </WadCfg>"
}, 
```

När du distribuerar hello exempelmall används variabelvärden för hello namn och en identifierare för hello tidigare skapade lagringskontot. Variabler är också används tooprovide hello inställningar för diagnostik hello-tillägg. Använd hello [bästa praxis för att skapa mallar för Azure Resource Manager](../../resource-manager-template-best-practices.md) toohelp du bestämma hur du vill toostructure hello parametrar och variabler i mallen.

## <a name="resource-loops"></a>Resursen slingor

När du behöver mer än en virtuell dator för programmet, kan du använda en kopia-element i en mall. Det här valfria elementet loop genom att skapa hello antal virtuella datorer som du angav som en parameter:

```
"copy": {
  "name": "virtualMachineLoop", 
  "count": "[parameters('numberOfInstances')]"
},
```

I hello-exemplet som hello loopindexet används också när du anger några av hello värden för hello resurs. Till exempel är om du har angett ett instansantal på tre, hello namnen på hello operativsystemet diskar myOSDisk1, myOSDisk2 och myOSDisk3:

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
}
```

> [!NOTE] 
>Det här exemplet använder hanterade diskar för hello virtuella datorer.
>
>

Tänk på att skapa en loop för en resurs i mallen för hello kan kräva att du toouse hello loop när du skapar eller få åtkomst till andra resurser. Flera virtuella datorer kan till exempel inte använda hello samma nätverksgränssnittet, så om din mall loop genom att skapa tre virtuella datorer det måste också gå igenom hur du skapar tre nätverksgränssnitt. När du tilldelar en network interface tooa VM hello loopindexet är används tooidentify den:

```
"networkInterfaces": [ { 
  "id": "[resourceId('Microsoft.Network/networkInterfaces',
    concat('myNIC', copyindex()))]" 
} ]
```

## <a name="dependencies"></a>Beroenden

De flesta resurser beroende av andra resurser toowork korrekt. Virtuella datorer måste vara kopplad till ett virtuellt nätverk och toodo måste ett nätverksgränssnitt. Hej [dependsOn](../../resource-group-define-dependencies.md) elementet har använt toomake att hello nätverksgränssnittet är klar toobe används innan hello virtuella datorer skapas:

```
"dependsOn": [
  "[concat('Microsoft.Network/networkInterfaces/', 'myNIC', copyindex())]" 
],
```

Hanteraren för filserverresurser distribuerar parallellt alla resurser som inte är beroende av en annan resurs som ska distribueras. Var försiktig när du ställer in beroenden eftersom du kan oavsiktligt göra distributionen genom att ange onödiga beroenden. Beroenden kan kedja genom flera resurser. Till exempel beroende hello nätverksgränssnittet hello offentlig IP-adress och nätverksresurser på virtuella datorer.

Hur vet du om det krävs ett beroende? Titta på hello värdena som du angett i hello mallen. Om ett element i hello virtuella resursdefinitionen pekar tooanother resurs som har distribuerats i hello samma mall du behöver ett beroende. Till exempel definierar den virtuella datorn exempel en nätverksprofil:

```
"networkProfile": { 
  "networkInterfaces": [ { 
    "id": "[resourceId('Microsoft.Network/networkInterfaces',
      concat('myNIC', copyindex())]" 
  } ] 
},
```

tooset den här egenskapen hello nätverksgränssnitt måste finnas. Därför måste ett beroende. Du måste också tooset ett beroende när en resurs (en underordnad) definieras inom en annan resurs (överordnad). Till exempel definieras hello diagnostikinställningarna-tillägg för anpassat skript för både som underordnade resurser till hello virtuell dator. De kan inte skapas förrän hello virtuella datorn finns. Därför är båda resurserna markerad som en beroende hello virtuell dator.

## <a name="profiles"></a>Profiler

Flera profil element används när du definierar en virtuell datorresurs. Vissa är obligatoriska och vissa är valfria. Till exempel hello hardwareProfile, osProfile, storageProfile och networkProfile-element krävs, men hello diagnosticsProfile är valfritt. De här profilerna definiera inställningar som:
   
- [storlek](sizes.md)
- [namnet](/architecture/best-practices/naming-conventions) och autentiseringsuppgifter
- disk och [inställningar för operativsystem](cli-ps-findimage.md)
- [nätverksgränssnitt](../../virtual-network/virtual-networks-multiple-nics.md) 
- Startdiagnostikinställningar

## <a name="disks-and-images"></a>Diskar och bilder
   
I Azure, vhd-filer kan representera [diskar eller bilder](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). När hello operativsystem i en vhd-fil är särskilda toobe en specifik VM, är det hänvisade tooas en disk. När hello operativsystem i en vhd-fil är generaliserad toobe används toocreate många virtuella datorer, är det hänvisade tooas en bild.   
    
### <a name="create-new-virtual-machines-and-new-disks-from-a-platform-image"></a>Skapa nya virtuella datorer och nya diskar från en plattformsavbildning

När du skapar en virtuell dator, måste du bestämma vilka operativsystem toouse. Hej imageReference elementet har använt toodefine hello operativsystemet på en ny virtuell dator. hello exempel visas en definition för ett Windows Server-operativsystem:

```
"imageReference": { 
  "publisher": "MicrosoftWindowsServer", 
  "offer": "WindowsServer", 
  "sku": "2012-R2-Datacenter", 
  "version": "latest" 
},
```

Om du vill toocreate en Linux-operativsystem, kan du använda den här definitionen:

```
"imageReference": {
  "publisher": "Canonical",
  "offer": "UbuntuServer",
  "sku": "14.04.2-LTS",
  "version": "latest"
},
```

Konfigurationsinställningar för hello operativsystemdisken tilldelas med hello osDisk element. hello exempel definierar en ny hanterade disk med hello cachelagring läge har angetts för**ReadWrite** och hello disken skapas från en [plattformsavbildning](cli-ps-findimage.md):

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
},
```

### <a name="create-new-virtual-machines-from-existing-managed-disks"></a>Skapa nya virtuella datorer från befintliga hanterade diskar

Om du vill toocreate virtuella datorer från befintliga diskar, ta bort hello imageReference och hello osProfile element och definiera diskinställningarna:

```
"osDisk": { 
  "osType": "Windows",
  "managedDisk": { 
    "id": "[resourceId('Microsoft.Compute/disks', [concat('myOSDisk', copyindex())])]" 
  }, 
  "caching": "ReadWrite",
  "createOption": "Attach" 
},
```

### <a name="create-new-virtual-machines-from-a-managed-image"></a>Skapa nya virtuella datorer från en hanterad avbildning

Om du vill toocreate en virtuell dator från en hanterad avbildning, ändra hello imageReference element och definiera diskinställningarna:

```
"storageProfile": { 
  "imageReference": {
    "id": "[resourceId('Microsoft.Compute/images', 'myImage')]"
  },
  "osDisk": { 
    "name": "[concat('myOSDisk', copyindex())]",
    "osType": "Windows",
    "caching": "ReadWrite", 
    "createOption": "FromImage" 
  }
},
```

### <a name="attach-data-disks"></a>Bifoga datadiskar

Du kan lägga till data diskar toohello virtuella datorer. Hej [antal diskar](sizes.md) beror på hello storleken på operativsystemdisken som du använder. Med hello ange storleken på virtuella datorer hello tooStandard_DS1_v2 hello maximala antalet datadiskar som kunde läggas till toohello dem är två. I exemplet hello läggs en hanterad datadisk tooeach VM:

```
"dataDisks": [
  {
    "name": "[concat('myDataDisk', copyindex())]",
    "diskSizeGB": "100",
    "lun": 0, 
    "caching": "ReadWrite",
    "createOption": "Empty"
  }
],
```

## <a name="extensions"></a>Tillägg

Även om [tillägg](extensions-features.md) är en separat resurs, de är nära bundet tooVMs. Tillägg kan läggas till som en underordnad resurs av hello VM eller som en separat resurs. hello exemplet visar hello [diagnostik tillägget](extensions-diagnostics-template.md) läggs toohello virtuella datorer:

```
{ 
  "name": "Microsoft.Insights.VMDiagnosticsSettings", 
  "type": "extensions", 
  "location": "[resourceGroup().location]", 
  "apiVersion": "2016-03-30", 
  "dependsOn": [ 
    "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
  ], 
  "properties": { 
    "publisher": "Microsoft.Azure.Diagnostics", 
    "type": "IaaSDiagnostics", 
    "typeHandlerVersion": "1.5", 
    "autoUpgradeMinorVersion": true, 
    "settings": { 
      "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
      variables('wadmetricsresourceid'), 
      concat('myVM', copyindex()),
      variables('wadcfgxend')))]", 
      "storageAccount": "[variables('storageName')]" 
    }, 
    "protectedSettings": { 
      "storageAccountName": "[variables('storageName')]", 
      "storageAccountKey": "[listkeys(variables('accountid'), 
        '2015-06-15').key1]", 
      "storageAccountEndPoint": "https://core.windows.net" 
    } 
  } 
},
```

Tillägget resursen använder hello storageName variabeln och hello diagnostiska tooprovide variabelvärden. Om du vill toochange hello data som samlas in med det här tillägget kan du lägga till flera prestandaräknare toohello wadperfcounters variabeln. Du kan också välja tooput hello diagnostikdata till ett annat lagringskonto än där hello Virtuella diskar lagras.

Det finns många tillägg som du kan installera på en virtuell dator, men hello mest användbara är förmodligen hello [tillägget för anpassat skript](extensions-customscript.md). Hello exempelvis körs ett PowerShell.skript som heter start.ps1 på varje virtuell dator när den startas:

```
{
  "name": "MyCustomScriptExtension",
  "type": "extensions",
  "apiVersion": "2016-03-30",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]"
  ],
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.7",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "[concat('https://', variables('storageName'),
          '.blob.core.windows.net/customscripts/start.ps1')]" 
      ],
      "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
    }
  }
}
```

Hej start.ps1 skriptet kan utföra många konfigurationsåtgärder. Till exempel har hello datadiskar som läggs toohello virtuella datorer i hello exempel inte initierats; Du kan använda ett anpassat skript tooinitialize dem. Om du har flera Start uppgifter toodo kan använda du hello start.ps1 filen toocall andra PowerShell-skript i Azure-lagring. hello exempel använder PowerShell, men du kan använda valfri metod för skript som är tillgänglig på hello-operativsystemet som du använder.

Du kan se status för hello av hello installerat tillägg från hello tillägg inställningar i hello-portalen:

![Hämta Åtgärdsstatus för tillägg](./media/template-description/virtual-machines-show-extensions.png)

Du kan också få tillägget information med hjälp av hello **Get-AzureRmVMExtension** PowerShell kommandot hello **vm-tillägget get** Azure CLI 2.0 kommando eller hello **hämta information om tillägg**  REST API.

## <a name="deployments"></a>Distributioner

När du distribuerar en mall, Azure spårar hello resurser som du har distribuerat som en grupp och automatiskt tilldelas en gruppnamnet toothis distribueras. hello namnet på hello distribution är hello samma som hello hello mallens namn.

Om du är nyfiken hello status för resurser i hello distribution kan använda du hello resursgrupp-bladet i hello Azure-portalen:

![Hämta information om distribution](./media/template-description/virtual-machines-deployment-info.png)
    
Det är inte ett problem toouse hello samma mall toocreate eller tooupdate befintliga resurser. När du använder kommandona toodeploy mallar du har hello möjlighet toosay som [läge](../../resource-group-template-deploy.md) du vill toouse. hello-läge kan anges tooeither **Slutför** eller **stegvis**. hello standardvärdet är toodo inkrementella uppdateringar. Var försiktig när du använder hello **Slutför** läge eftersom av misstag kan du ta bort resurser. När du anger hello läge för**Slutför**, Resource Manager tar du bort alla resurser i hello resursgrupp som inte ingår i hello mallen.

## <a name="next-steps"></a>Nästa steg

- Skapa en egen mall med hjälp av [redigera Azure Resource Manager-mallar](../../resource-group-authoring-templates.md).
- Distribuera hello-mallen som du skapat med [skapa en Windows-dator med en Resource Manager-mall](ps-template.md).
- Lär dig hur toomanage hello virtuella datorer som du skapade genom att granska [skapa och hantera virtuella Windows-datorer med hello Azure PowerShell-modulen](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
