---
title: Virtuella datorer i en Azure Resource Manager-mall | Microsoft Azure
description: "Läs mer om hur den virtuella datorresursen har definierats i en Azure Resource Manager-mall."
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
ms.openlocfilehash: d9b9121bc5e38396ba4def6c17f9b373c2b48056
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="virtual-machines-in-an-azure-resource-manager-template"></a><span data-ttu-id="298d6-103">Virtuella datorer i en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="298d6-103">Virtual machines in an Azure Resource Manager template</span></span>

<span data-ttu-id="298d6-104">Den här artikeln beskriver aspekter av en Azure Resource Manager-mall som gäller för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="298d6-104">This article describes aspects of an Azure Resource Manager template that apply to virtual machines.</span></span> <span data-ttu-id="298d6-105">Den här artikeln beskriver inte en fullständig mall för att skapa en virtuell dator; för att du behöver resursdefinitionerna för storage-konton, nätverksgränssnitt, offentliga IP-adresser och virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="298d6-105">This article doesn’t describe a complete template for creating a virtual machine; for that you need resource definitions for storage accounts, network interfaces, public IP addresses, and virtual networks.</span></span> <span data-ttu-id="298d6-106">Mer information om hur dessa resurser kan definieras tillsammans finns i [genomgång av Resource Manager-mall](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="298d6-106">For more information about how these resources can be defined together, see the [Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="298d6-107">Det finns många [mallar i galleriet](https://azure.microsoft.com/documentation/templates/?term=VM) som innehåller den Virtuella datorresursen.</span><span class="sxs-lookup"><span data-stu-id="298d6-107">There are many [templates in the gallery](https://azure.microsoft.com/documentation/templates/?term=VM) that include the VM resource.</span></span> <span data-ttu-id="298d6-108">Här beskrivs inte alla element som kan ingå i en mall.</span><span class="sxs-lookup"><span data-stu-id="298d6-108">Not all elements that can be included in a template are described here.</span></span>

<span data-ttu-id="298d6-109">Det här exemplet illustrerar en typisk resursavsnitt i en mall för att skapa ett angivet antal virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="298d6-109">This example shows a typical resource section of a template for creating a specified number of VMs:</span></span>

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
><span data-ttu-id="298d6-110">Det här exemplet bygger på ett lagringskonto som skapades tidigare.</span><span class="sxs-lookup"><span data-stu-id="298d6-110">This example relies on a storage account that was previously created.</span></span> <span data-ttu-id="298d6-111">Du kan skapa lagringskontot genom att distribuera den från mallen.</span><span class="sxs-lookup"><span data-stu-id="298d6-111">You could create the storage account by deploying it from the template.</span></span> <span data-ttu-id="298d6-112">Exemplet använder också ett nätverksgränssnitt och dess beroende resurser som skulle definieras i mallen.</span><span class="sxs-lookup"><span data-stu-id="298d6-112">The example also relies on a network interface and its dependent resources that would be defined in the template.</span></span> <span data-ttu-id="298d6-113">Dessa resurser visas inte i exemplet.</span><span class="sxs-lookup"><span data-stu-id="298d6-113">These resources are not shown in the example.</span></span>
>
>

## <a name="api-version"></a><span data-ttu-id="298d6-114">API-Version</span><span class="sxs-lookup"><span data-stu-id="298d6-114">API Version</span></span>

<span data-ttu-id="298d6-115">Du måste ange en version av API för att använda när du distribuerar resurser med hjälp av en mall.</span><span class="sxs-lookup"><span data-stu-id="298d6-115">When you deploy resources using a template, you have to specify a version of the API to use.</span></span> <span data-ttu-id="298d6-116">Exemplet visar den virtuella datorresursen med den här apiVersion-element:</span><span class="sxs-lookup"><span data-stu-id="298d6-116">The example shows the virtual machine resource using this apiVersion element:</span></span>

```
"apiVersion": "2016-04-30-preview",
```

<span data-ttu-id="298d6-117">Versionen av API: N som du anger i mallen påverkar vilka egenskaper som du kan definiera i mallen.</span><span class="sxs-lookup"><span data-stu-id="298d6-117">The version of the API you specify in your template affects which properties you can define in the template.</span></span> <span data-ttu-id="298d6-118">I allmänhet bör du välja den senaste API-versionen när du skapar mallar.</span><span class="sxs-lookup"><span data-stu-id="298d6-118">In general, you should select the most recent API version when creating templates.</span></span> <span data-ttu-id="298d6-119">Du kan bestämma om du vill fortsätta med en tidigare API-version eller uppdatera mallen för den senaste versionen dra nytta av nya funktioner för befintliga mallar.</span><span class="sxs-lookup"><span data-stu-id="298d6-119">For existing templates, you can decide whether you want to continue using an earlier API version, or update your template for the latest version to take advantage of new features.</span></span>

<span data-ttu-id="298d6-120">Använd dessa möjligheter för att hämta de senaste API-versionerna:</span><span class="sxs-lookup"><span data-stu-id="298d6-120">Use these opportunities for getting the latest API versions:</span></span>

- <span data-ttu-id="298d6-121">REST API - [lista över alla resursproviders](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)</span><span class="sxs-lookup"><span data-stu-id="298d6-121">REST API - [List all resource providers](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)</span></span>
- <span data-ttu-id="298d6-122">PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)</span><span class="sxs-lookup"><span data-stu-id="298d6-122">PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)</span></span>
- <span data-ttu-id="298d6-123">Azure CLI 2.0 - [az provider show](https://docs.microsoft.com/cli/azure/provider#show)</span><span class="sxs-lookup"><span data-stu-id="298d6-123">Azure CLI 2.0 - [az provider show](https://docs.microsoft.com/cli/azure/provider#show)</span></span>

## <a name="parameters-and-variables"></a><span data-ttu-id="298d6-124">Parametrar och variabler</span><span class="sxs-lookup"><span data-stu-id="298d6-124">Parameters and variables</span></span>

<span data-ttu-id="298d6-125">[Parametrarna](../../resource-group-authoring-templates.md) gör det lättare för dig att ange värden för mallen när den körs.</span><span class="sxs-lookup"><span data-stu-id="298d6-125">[Parameters](../../resource-group-authoring-templates.md) make it easy for you to specify values for the template when you run it.</span></span> <span data-ttu-id="298d6-126">Det här avsnittet parametrar används i exemplet:</span><span class="sxs-lookup"><span data-stu-id="298d6-126">This parameters section is used in the example:</span></span>

```        
"parameters": {
  "adminUsername": { "type": "string" },
  "adminPassword": { "type": "securestring" },
  "numberOfInstances": { "type": "int" }
},
```

<span data-ttu-id="298d6-127">När du distribuerar mallen exempel kan ange du värden för namn och lösenord för administratörskontot för varje virtuell dator och antalet virtuella datorer för att skapa.</span><span class="sxs-lookup"><span data-stu-id="298d6-127">When you deploy the example template, you enter values for the name and password of the administrator account on each VM and the number of VMs to create.</span></span> <span data-ttu-id="298d6-128">Du har möjlighet att ange parametervärden i en separat fil som hanteras med hjälp av mallen eller tillhandahåller värden när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="298d6-128">You have the option of specifying parameter values in a separate file that's managed with the template, or providing values when prompted.</span></span>

<span data-ttu-id="298d6-129">[Variabler](../../resource-group-authoring-templates.md) gör det lättare för dig att ange värden i mallen som används flera gånger i den eller som kan ändras med tiden.</span><span class="sxs-lookup"><span data-stu-id="298d6-129">[Variables](../../resource-group-authoring-templates.md) make it easy for you to set up values in the template that are used repeatedly throughout it or that can change over time.</span></span> <span data-ttu-id="298d6-130">Det här avsnittet för variabler som används i exemplet:</span><span class="sxs-lookup"><span data-stu-id="298d6-130">This variables section is used in the example:</span></span>

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

<span data-ttu-id="298d6-131">När du distribuerar mallen exempel används variabelvärden för namn och ID: t för det tidigare skapade lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="298d6-131">When you deploy the example template, variable values are used for the name and identifier of the previously created storage account.</span></span> <span data-ttu-id="298d6-132">Variabler används också för att ange inställningar för diagnostik tillägget.</span><span class="sxs-lookup"><span data-stu-id="298d6-132">Variables are also used to provide the settings for the diagnostic extension.</span></span> <span data-ttu-id="298d6-133">Använd den [bästa praxis för att skapa mallar för Azure Resource Manager](../../resource-manager-template-best-practices.md) som hjälper dig att bestämma hur du vill att strukturera parametrar och variabler i mallen.</span><span class="sxs-lookup"><span data-stu-id="298d6-133">Use the [best practices for creating Azure Resource Manager templates](../../resource-manager-template-best-practices.md) to help you decide how you want to structure the parameters and variables in your template.</span></span>

## <a name="resource-loops"></a><span data-ttu-id="298d6-134">Resursen slingor</span><span class="sxs-lookup"><span data-stu-id="298d6-134">Resource loops</span></span>

<span data-ttu-id="298d6-135">När du behöver mer än en virtuell dator för programmet, kan du använda en kopia-element i en mall.</span><span class="sxs-lookup"><span data-stu-id="298d6-135">When you need more than one virtual machine for your application, you can use a copy element in a template.</span></span> <span data-ttu-id="298d6-136">Det här valfria elementet loop genom att skapa antal virtuella datorer som du angav som en parameter:</span><span class="sxs-lookup"><span data-stu-id="298d6-136">This optional element loops through creating the number of VMs that you specified as a parameter:</span></span>

```
"copy": {
  "name": "virtualMachineLoop", 
  "count": "[parameters('numberOfInstances')]"
},
```

<span data-ttu-id="298d6-137">Observera också i exempel loopindexet används när du anger en del av värden för resursen.</span><span class="sxs-lookup"><span data-stu-id="298d6-137">Also, notice in the example that the loop index is used when specifying some of the values for the resource.</span></span> <span data-ttu-id="298d6-138">Om du har angett ett instansantal på tre är till exempel namnen på operativsystemet diskar myOSDisk1, myOSDisk2 och myOSDisk3:</span><span class="sxs-lookup"><span data-stu-id="298d6-138">For example, if you entered an instance count of three, the names of the operating system disks are myOSDisk1, myOSDisk2, and myOSDisk3:</span></span>

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
}
```

> [!NOTE] 
><span data-ttu-id="298d6-139">Det här exemplet använder hanterade diskar för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="298d6-139">This example uses managed disks for the virtual machines.</span></span>
>
>

<span data-ttu-id="298d6-140">Tänk på att skapa en loop för en resurs i mallen kan kräva att använda loop när du skapar eller få åtkomst till andra resurser.</span><span class="sxs-lookup"><span data-stu-id="298d6-140">Keep in mind that creating a loop for one resource in the template may require you to use the loop when creating or accessing other resources.</span></span> <span data-ttu-id="298d6-141">Flera virtuella datorer kan inte använda samma nätverksgränssnitt, så om din mall loop genom att skapa tre virtuella datorer måste den också gå igenom skapa tre nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="298d6-141">For example, multiple VMs can’t use the same network interface, so if your template loops through creating three VMs it must also loop through creating three network interfaces.</span></span> <span data-ttu-id="298d6-142">När du tilldelar en virtuell dator ett nätverksgränssnitt används loopindexet för att identifiera den:</span><span class="sxs-lookup"><span data-stu-id="298d6-142">When assigning a network interface to a VM, the loop index is used to identify it:</span></span>

```
"networkInterfaces": [ { 
  "id": "[resourceId('Microsoft.Network/networkInterfaces',
    concat('myNIC', copyindex()))]" 
} ]
```

## <a name="dependencies"></a><span data-ttu-id="298d6-143">Beroenden</span><span class="sxs-lookup"><span data-stu-id="298d6-143">Dependencies</span></span>

<span data-ttu-id="298d6-144">De flesta resurser är beroende av andra resurser ska fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="298d6-144">Most resources depend on other resources to work correctly.</span></span> <span data-ttu-id="298d6-145">Virtuella datorer måste vara associerad med ett virtuellt nätverk och gör att den behöver ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="298d6-145">Virtual machines must be associated with a virtual network and to do that it needs a network interface.</span></span> <span data-ttu-id="298d6-146">Den [dependsOn](../../resource-group-define-dependencies.md) element som används för att kontrollera att nätverkskortet är redo att användas innan de virtuella datorerna har skapats:</span><span class="sxs-lookup"><span data-stu-id="298d6-146">The [dependsOn](../../resource-group-define-dependencies.md) element is used to make sure that the network interface is ready to be used before the VMs are created:</span></span>

```
"dependsOn": [
  "[concat('Microsoft.Network/networkInterfaces/', 'myNIC', copyindex())]" 
],
```

<span data-ttu-id="298d6-147">Hanteraren för filserverresurser distribuerar parallellt alla resurser som inte är beroende av en annan resurs som ska distribueras.</span><span class="sxs-lookup"><span data-stu-id="298d6-147">Resource Manager deploys in parallel any resources that are not dependent on another resource being deployed.</span></span> <span data-ttu-id="298d6-148">Var försiktig när du ställer in beroenden eftersom du kan oavsiktligt göra distributionen genom att ange onödiga beroenden.</span><span class="sxs-lookup"><span data-stu-id="298d6-148">Be careful when setting dependencies because you can inadvertently slow your deployment by specifying unnecessary dependencies.</span></span> <span data-ttu-id="298d6-149">Beroenden kan kedja genom flera resurser.</span><span class="sxs-lookup"><span data-stu-id="298d6-149">Dependencies can chain through multiple resources.</span></span> <span data-ttu-id="298d6-150">Till exempel beror nätverksgränssnittet på den offentliga IP-adressen och nätverksresurser på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="298d6-150">For example, the network interface depends on the public IP address and virtual network resources.</span></span>

<span data-ttu-id="298d6-151">Hur vet du om det krävs ett beroende?</span><span class="sxs-lookup"><span data-stu-id="298d6-151">How do you know if a dependency is required?</span></span> <span data-ttu-id="298d6-152">Titta på de värden som du anger i mallen.</span><span class="sxs-lookup"><span data-stu-id="298d6-152">Look at the values you set in the template.</span></span> <span data-ttu-id="298d6-153">Om ett element i den virtuella resource definition pekar på en annan resurs som distribueras i samma mall måste ett beroende.</span><span class="sxs-lookup"><span data-stu-id="298d6-153">If an element in the virtual machine resource definition points to another resource that is deployed in the same template, you need a dependency.</span></span> <span data-ttu-id="298d6-154">Till exempel definierar den virtuella datorn exempel en nätverksprofil:</span><span class="sxs-lookup"><span data-stu-id="298d6-154">For example, your example virtual machine defines a network profile:</span></span>

```
"networkProfile": { 
  "networkInterfaces": [ { 
    "id": "[resourceId('Microsoft.Network/networkInterfaces',
      concat('myNIC', copyindex())]" 
  } ] 
},
```

<span data-ttu-id="298d6-155">Nätverksgränssnittet måste finnas för att ange den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="298d6-155">To set this property, the network interface must exist.</span></span> <span data-ttu-id="298d6-156">Därför måste ett beroende.</span><span class="sxs-lookup"><span data-stu-id="298d6-156">Therefore, you need a dependency.</span></span> <span data-ttu-id="298d6-157">Du måste ange ett beroende när en resurs (en underordnad) definieras inom en annan resurs (överordnad).</span><span class="sxs-lookup"><span data-stu-id="298d6-157">You also need to set a dependency when one resource (a child) is defined within another resource (a parent).</span></span> <span data-ttu-id="298d6-158">Till exempel är diagnostikinställningar och tillägg för anpassat skript definierade som underordnade resurser till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="298d6-158">For example, the diagnostic settings and custom script extensions are both defined as child resources of the virtual machine.</span></span> <span data-ttu-id="298d6-159">De kan inte skapas förrän den virtuella datorn finns.</span><span class="sxs-lookup"><span data-stu-id="298d6-159">They cannot be created until the virtual machine exists.</span></span> <span data-ttu-id="298d6-160">Därför markeras båda resurserna som är beroende av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="298d6-160">Therefore, both resources are marked as dependent on the virtual machine.</span></span>

## <a name="profiles"></a><span data-ttu-id="298d6-161">Profiler</span><span class="sxs-lookup"><span data-stu-id="298d6-161">Profiles</span></span>

<span data-ttu-id="298d6-162">Flera profil element används när du definierar en virtuell datorresurs.</span><span class="sxs-lookup"><span data-stu-id="298d6-162">Several profile elements are used when defining a virtual machine resource.</span></span> <span data-ttu-id="298d6-163">Vissa är obligatoriska och vissa är valfria.</span><span class="sxs-lookup"><span data-stu-id="298d6-163">Some are required and some are optional.</span></span> <span data-ttu-id="298d6-164">Till exempel hardwareProfile, osProfile, storageProfile och networkProfile-element krävs, men diagnosticsProfile är valfritt.</span><span class="sxs-lookup"><span data-stu-id="298d6-164">For example, the hardwareProfile, osProfile, storageProfile, and networkProfile elements are required, but the diagnosticsProfile is optional.</span></span> <span data-ttu-id="298d6-165">De här profilerna definiera inställningar som:</span><span class="sxs-lookup"><span data-stu-id="298d6-165">These profiles define settings such as:</span></span>
   
- [<span data-ttu-id="298d6-166">storlek</span><span class="sxs-lookup"><span data-stu-id="298d6-166">size</span></span>](sizes.md)
- <span data-ttu-id="298d6-167">[namnet](/architecture/best-practices/naming-conventions) och autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="298d6-167">[name](/architecture/best-practices/naming-conventions) and credentials</span></span>
- <span data-ttu-id="298d6-168">disk och [inställningar för operativsystem](cli-ps-findimage.md)</span><span class="sxs-lookup"><span data-stu-id="298d6-168">disk and [operating system settings](cli-ps-findimage.md)</span></span>
- [<span data-ttu-id="298d6-169">nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="298d6-169">network interface</span></span>](../../virtual-network/virtual-networks-multiple-nics.md) 
- <span data-ttu-id="298d6-170">Startdiagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="298d6-170">boot diagnostics</span></span>

## <a name="disks-and-images"></a><span data-ttu-id="298d6-171">Diskar och bilder</span><span class="sxs-lookup"><span data-stu-id="298d6-171">Disks and images</span></span>
   
<span data-ttu-id="298d6-172">I Azure, vhd-filer kan representera [diskar eller bilder](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="298d6-172">In Azure, vhd files can represent [disks or images](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="298d6-173">När operativsystemet i en vhd-fil är specialanpassat ska vara en specifik VM, kallas det en disk.</span><span class="sxs-lookup"><span data-stu-id="298d6-173">When the operating system in a vhd file is specialized to be a specific VM, it is referred to as a disk.</span></span> <span data-ttu-id="298d6-174">När operativsystemet i en vhd-fil är generaliserad som används för att skapa många virtuella datorer, kallas det en bild.</span><span class="sxs-lookup"><span data-stu-id="298d6-174">When the operating system in a vhd file is generalized to be used to create many VMs, it is referred to as an image.</span></span>   
    
### <a name="create-new-virtual-machines-and-new-disks-from-a-platform-image"></a><span data-ttu-id="298d6-175">Skapa nya virtuella datorer och nya diskar från en plattformsavbildning</span><span class="sxs-lookup"><span data-stu-id="298d6-175">Create new virtual machines and new disks from a platform image</span></span>

<span data-ttu-id="298d6-176">När du skapar en virtuell dator, måste du bestämma vilket operativsystem du använder.</span><span class="sxs-lookup"><span data-stu-id="298d6-176">When you create a VM, you must decide what operating system to use.</span></span> <span data-ttu-id="298d6-177">Elementet imageReference används för att definiera operativsystemet på en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="298d6-177">The imageReference element is used to define the operating system of a new VM.</span></span> <span data-ttu-id="298d6-178">Exemplet visar en definition för ett Windows Server-operativsystem:</span><span class="sxs-lookup"><span data-stu-id="298d6-178">The example shows a definition for a Windows Server operating system:</span></span>

```
"imageReference": { 
  "publisher": "MicrosoftWindowsServer", 
  "offer": "WindowsServer", 
  "sku": "2012-R2-Datacenter", 
  "version": "latest" 
},
```

<span data-ttu-id="298d6-179">Du kan använda den här definitionen om du vill skapa ett Linux-operativsystem:</span><span class="sxs-lookup"><span data-stu-id="298d6-179">If you want to create a Linux operating system, you might use this definition:</span></span>

```
"imageReference": {
  "publisher": "Canonical",
  "offer": "UbuntuServer",
  "sku": "14.04.2-LTS",
  "version": "latest"
},
```

<span data-ttu-id="298d6-180">Konfigurationsinställningar för operativsystemets disk tilldelas med elementet osDisk.</span><span class="sxs-lookup"><span data-stu-id="298d6-180">Configuration settings for the operating system disk are assigned with the osDisk element.</span></span> <span data-ttu-id="298d6-181">I exempel definierar en ny hanterade disk med cachelagring inställd på **ReadWrite** och att disken skapas från en [plattformsavbildning](cli-ps-findimage.md):</span><span class="sxs-lookup"><span data-stu-id="298d6-181">The example defines a new managed disk with the caching mode set to **ReadWrite** and that the disk is being created from a [platform image](cli-ps-findimage.md):</span></span>

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
},
```

### <a name="create-new-virtual-machines-from-existing-managed-disks"></a><span data-ttu-id="298d6-182">Skapa nya virtuella datorer från befintliga hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="298d6-182">Create new virtual machines from existing managed disks</span></span>

<span data-ttu-id="298d6-183">Om du vill skapa virtuella datorer från befintliga diskar, ta bort imageReference och osProfile-element och definiera diskinställningarna:</span><span class="sxs-lookup"><span data-stu-id="298d6-183">If you want to create virtual machines from existing disks, remove the imageReference and the osProfile elements and define these disk settings:</span></span>

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

### <a name="create-new-virtual-machines-from-a-managed-image"></a><span data-ttu-id="298d6-184">Skapa nya virtuella datorer från en hanterad avbildning</span><span class="sxs-lookup"><span data-stu-id="298d6-184">Create new virtual machines from a managed image</span></span>

<span data-ttu-id="298d6-185">Om du vill skapa en virtuell dator från en hanterad avbildning ändra elementet imageReference och definiera diskinställningarna:</span><span class="sxs-lookup"><span data-stu-id="298d6-185">If you want to create a virtual machine from a managed image, change the imageReference element and define these disk settings:</span></span>

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

### <a name="attach-data-disks"></a><span data-ttu-id="298d6-186">Bifoga datadiskar</span><span class="sxs-lookup"><span data-stu-id="298d6-186">Attach data disks</span></span>

<span data-ttu-id="298d6-187">Du kan lägga till datadiskar till de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="298d6-187">You can optionally add data disks to the VMs.</span></span> <span data-ttu-id="298d6-188">Den [antal diskar](sizes.md) beror på storleken på disken för operativsystemet som du använder.</span><span class="sxs-lookup"><span data-stu-id="298d6-188">The [number of disks](sizes.md) depends on the size of operating system disk that you use.</span></span> <span data-ttu-id="298d6-189">Med storleken på de virtuella datorerna som angetts till Standard_DS1_v2 är maximala antalet datadiskar som kan läggas till dem två.</span><span class="sxs-lookup"><span data-stu-id="298d6-189">With the size of the VMs set to Standard_DS1_v2, the maximum number of data disks that could be added to the them is two.</span></span> <span data-ttu-id="298d6-190">I exemplet är läggs en hanterad datadisk till varje virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="298d6-190">In the example, one managed data disk is being added to each VM:</span></span>

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

## <a name="extensions"></a><span data-ttu-id="298d6-191">Tillägg</span><span class="sxs-lookup"><span data-stu-id="298d6-191">Extensions</span></span>

<span data-ttu-id="298d6-192">Även om [tillägg](extensions-features.md) är en separat resurs, de är beroende av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="298d6-192">Although [extensions](extensions-features.md) are a separate resource, they are closely tied to VMs.</span></span> <span data-ttu-id="298d6-193">Tillägg kan läggas till som en underordnad resurs för den virtuella datorn eller som en separat resurs.</span><span class="sxs-lookup"><span data-stu-id="298d6-193">Extensions can be added as a child resource of the VM or as a separate resource.</span></span> <span data-ttu-id="298d6-194">Exempel visar den [diagnostik tillägget](extensions-diagnostics-template.md) läggs till i de virtuella datorerna:</span><span class="sxs-lookup"><span data-stu-id="298d6-194">The example shows the [Diagnostics Extension](extensions-diagnostics-template.md) being added to the VMs:</span></span>

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

<span data-ttu-id="298d6-195">Tillägget resursen använder variabeln storageName och variablerna diagnostik för att ge värden.</span><span class="sxs-lookup"><span data-stu-id="298d6-195">This extension resource uses the storageName variable and the diagnostic variables to provide values.</span></span> <span data-ttu-id="298d6-196">Om du vill ändra de data som samlas in med det här tillägget kan du lägga till flera prestandaräknare wadperfcounters variabeln.</span><span class="sxs-lookup"><span data-stu-id="298d6-196">If you want to change the data that is collected by this extension, you can add more performance counters to the wadperfcounters variable.</span></span> <span data-ttu-id="298d6-197">Du kan också välja att placera diagnostikdata i ett annat lagringskonto än där VM-diskarna lagras.</span><span class="sxs-lookup"><span data-stu-id="298d6-197">You could also choose to put the diagnostics data into a different storage account than where the VM disks are stored.</span></span>

<span data-ttu-id="298d6-198">Det finns många tillägg som du kan installera på en virtuell dator, men den mest användbara är antagligen det [tillägget för anpassat skript](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="298d6-198">There are many extensions that you can install on a VM, but the most useful is probably the [Custom Script Extension](extensions-customscript.md).</span></span> <span data-ttu-id="298d6-199">I det här exemplet körs ett PowerShell.skript som heter start.ps1 på varje virtuell dator när den startas:</span><span class="sxs-lookup"><span data-stu-id="298d6-199">In the example, a PowerShell script named start.ps1 runs on each VM when it first starts:</span></span>

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

<span data-ttu-id="298d6-200">Skriptet start.ps1 kan utföra många konfigurationsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="298d6-200">The start.ps1 script can accomplish many configuration tasks.</span></span> <span data-ttu-id="298d6-201">Till exempel har datadiskar som läggs till de virtuella datorerna i exemplet inte initierats; Du kan använda ett anpassat skript till initiering.</span><span class="sxs-lookup"><span data-stu-id="298d6-201">For example, the data disks that are added to the VMs in the example are not initialized; you can use a custom script to initialize them.</span></span> <span data-ttu-id="298d6-202">Om du har flera Start uppgifter om du vill kan använda du filen start.ps1 för att anropa andra PowerShell-skript i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="298d6-202">If you have multiple startup tasks to do, you can use the start.ps1 file to call other PowerShell scripts in Azure storage.</span></span> <span data-ttu-id="298d6-203">I exemplet används PowerShell, men du kan använda valfri metod för skript som är tillgänglig på det operativsystem som du använder.</span><span class="sxs-lookup"><span data-stu-id="298d6-203">The example uses PowerShell, but you can use any scripting method that is available on the operating system that you are using.</span></span>

<span data-ttu-id="298d6-204">Du kan se status för de installerade tilläggen från tillägg-inställningarna i portalen:</span><span class="sxs-lookup"><span data-stu-id="298d6-204">You can see the status of the installed extensions from the Extensions settings in the portal:</span></span>

![Hämta Åtgärdsstatus för tillägg](./media/template-description/virtual-machines-show-extensions.png)

<span data-ttu-id="298d6-206">Du kan också få tillägget information med hjälp av den **Get-AzureRmVMExtension** PowerShell-kommando på **vm-tillägget get** Azure CLI 2.0 kommando eller **hämta information om tillägg** REST-API.</span><span class="sxs-lookup"><span data-stu-id="298d6-206">You can also get extension information by using the **Get-AzureRmVMExtension** PowerShell command, the **vm extension get** Azure CLI 2.0 command, or the **Get extension information** REST API.</span></span>

## <a name="deployments"></a><span data-ttu-id="298d6-207">Distributioner</span><span class="sxs-lookup"><span data-stu-id="298d6-207">Deployments</span></span>

<span data-ttu-id="298d6-208">När du distribuerar en mall spårar resurser som du distribuerat som en grupp och tilldelar automatiskt ett namn för den här distribuerad grupp i Azure.</span><span class="sxs-lookup"><span data-stu-id="298d6-208">When you deploy a template, Azure tracks the resources that you deployed as a group and automatically assigns a name to this deployed group.</span></span> <span data-ttu-id="298d6-209">Namnet på distributionen är samma som namnet på mallen.</span><span class="sxs-lookup"><span data-stu-id="298d6-209">The name of the deployment is the same as the name of the template.</span></span>

<span data-ttu-id="298d6-210">Om du är nyfiken på status för resurser i distributionen kan använda du resursgrupp-bladet i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="298d6-210">If you are curious about the status of resources in the deployment, you can use the Resource Group blade in the Azure portal:</span></span>

![Hämta information om distribution](./media/template-description/virtual-machines-deployment-info.png)
    
<span data-ttu-id="298d6-212">Det är inte ett problem ska använda samma mall att skapa resurser eller uppdaterar befintliga resurser.</span><span class="sxs-lookup"><span data-stu-id="298d6-212">It’s not a problem to use the same template to create resources or to update existing resources.</span></span> <span data-ttu-id="298d6-213">När du distribuera mallar med hjälp av kommandon har möjlighet att säga som [läge](../../resource-group-template-deploy.md) du vill använda.</span><span class="sxs-lookup"><span data-stu-id="298d6-213">When you use commands to deploy templates, you have the opportunity to say which [mode](../../resource-group-template-deploy.md) you want to use.</span></span> <span data-ttu-id="298d6-214">Läget kan vara inställd på antingen **Slutför** eller **stegvis**.</span><span class="sxs-lookup"><span data-stu-id="298d6-214">The mode can be set to either **Complete** or **Incremental**.</span></span> <span data-ttu-id="298d6-215">Standardvärdet är att göra inkrementella uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="298d6-215">The default is to do incremental updates.</span></span> <span data-ttu-id="298d6-216">Var försiktig när du använder den **Slutför** läge eftersom av misstag kan du ta bort resurser.</span><span class="sxs-lookup"><span data-stu-id="298d6-216">Be careful when using the **Complete** mode because you may accidentally delete resources.</span></span> <span data-ttu-id="298d6-217">När du anger läget till **Slutför**, Resource Manager tar du bort alla resurser i resursgruppen som inte ingår i mallen.</span><span class="sxs-lookup"><span data-stu-id="298d6-217">When you set the mode to **Complete**, Resource Manager deletes any resources in the resource group that are not in the template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="298d6-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="298d6-218">Next Steps</span></span>

- <span data-ttu-id="298d6-219">Skapa en egen mall med hjälp av [redigera Azure Resource Manager-mallar](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="298d6-219">Create your own template using [Authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span>
- <span data-ttu-id="298d6-220">Distribuera mallen som du skapat med [skapa en Windows-dator med en Resource Manager-mall](ps-template.md).</span><span class="sxs-lookup"><span data-stu-id="298d6-220">Deploy the template that you created using [Create a Windows virtual machine with a Resource Manager template](ps-template.md).</span></span>
- <span data-ttu-id="298d6-221">Lär dig hur du hanterar de virtuella datorerna som du skapade genom att granska [skapa och hantera virtuella Windows-datorer med Azure PowerShell-modulen](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="298d6-221">Learn how to manage the VMs that you created by reviewing [Create and manage Windows VMs with the Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
