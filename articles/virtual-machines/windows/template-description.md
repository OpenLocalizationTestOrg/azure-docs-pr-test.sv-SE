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
# <a name="virtual-machines-in-an-azure-resource-manager-template"></a><span data-ttu-id="629dd-103">Virtuella datorer i en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="629dd-103">Virtual machines in an Azure Resource Manager template</span></span>

<span data-ttu-id="629dd-104">Den här artikeln beskriver aspekter av en Azure Resource Manager-mall som gäller toovirtual datorer.</span><span class="sxs-lookup"><span data-stu-id="629dd-104">This article describes aspects of an Azure Resource Manager template that apply toovirtual machines.</span></span> <span data-ttu-id="629dd-105">Den här artikeln beskriver inte en fullständig mall för att skapa en virtuell dator; för att du behöver resursdefinitionerna för storage-konton, nätverksgränssnitt, offentliga IP-adresser och virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="629dd-105">This article doesn’t describe a complete template for creating a virtual machine; for that you need resource definitions for storage accounts, network interfaces, public IP addresses, and virtual networks.</span></span> <span data-ttu-id="629dd-106">Mer information om hur dessa resurser kan definieras tillsammans finns hello [genomgång av Resource Manager-mall](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="629dd-106">For more information about how these resources can be defined together, see hello [Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="629dd-107">Det finns många [mallar i hello galleriet](https://azure.microsoft.com/documentation/templates/?term=VM) som inkluderar hello Virtuella datorresursen.</span><span class="sxs-lookup"><span data-stu-id="629dd-107">There are many [templates in hello gallery](https://azure.microsoft.com/documentation/templates/?term=VM) that include hello VM resource.</span></span> <span data-ttu-id="629dd-108">Här beskrivs inte alla element som kan ingå i en mall.</span><span class="sxs-lookup"><span data-stu-id="629dd-108">Not all elements that can be included in a template are described here.</span></span>

<span data-ttu-id="629dd-109">Det här exemplet illustrerar en typisk resursavsnitt i en mall för att skapa ett angivet antal virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="629dd-109">This example shows a typical resource section of a template for creating a specified number of VMs:</span></span>

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
><span data-ttu-id="629dd-110">Det här exemplet bygger på ett lagringskonto som skapades tidigare.</span><span class="sxs-lookup"><span data-stu-id="629dd-110">This example relies on a storage account that was previously created.</span></span> <span data-ttu-id="629dd-111">Du kan skapa hello storage-konto genom att distribuera den från hello mall.</span><span class="sxs-lookup"><span data-stu-id="629dd-111">You could create hello storage account by deploying it from hello template.</span></span> <span data-ttu-id="629dd-112">hello exempel använder också ett nätverksgränssnitt och dess beroende resurser som skulle definieras i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="629dd-112">hello example also relies on a network interface and its dependent resources that would be defined in hello template.</span></span> <span data-ttu-id="629dd-113">Dessa resurser visas inte i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="629dd-113">These resources are not shown in hello example.</span></span>
>
>

## <a name="api-version"></a><span data-ttu-id="629dd-114">API-Version</span><span class="sxs-lookup"><span data-stu-id="629dd-114">API Version</span></span>

<span data-ttu-id="629dd-115">När du distribuerar resurser med hjälp av en mall har toospecify en version av hello API toouse.</span><span class="sxs-lookup"><span data-stu-id="629dd-115">When you deploy resources using a template, you have toospecify a version of hello API toouse.</span></span> <span data-ttu-id="629dd-116">hello exempel visar hello virtuell datorresurs med den här apiVersion-element:</span><span class="sxs-lookup"><span data-stu-id="629dd-116">hello example shows hello virtual machine resource using this apiVersion element:</span></span>

```
"apiVersion": "2016-04-30-preview",
```

<span data-ttu-id="629dd-117">hello-versionen av hello API som du anger i mallen påverkar vilka egenskaper som du kan definiera i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="629dd-117">hello version of hello API you specify in your template affects which properties you can define in hello template.</span></span> <span data-ttu-id="629dd-118">I allmänhet bör du välja hello senaste API-versionen när du skapar mallar.</span><span class="sxs-lookup"><span data-stu-id="629dd-118">In general, you should select hello most recent API version when creating templates.</span></span> <span data-ttu-id="629dd-119">För befintliga mallar, kan du bestämma om du vill toocontinue med en tidigare API-version eller uppdatera mallen för hello senaste version tootake nytta av nya funktioner.</span><span class="sxs-lookup"><span data-stu-id="629dd-119">For existing templates, you can decide whether you want toocontinue using an earlier API version, or update your template for hello latest version tootake advantage of new features.</span></span>

<span data-ttu-id="629dd-120">Använd dessa möjligheter för att hämta hello senaste API-versioner:</span><span class="sxs-lookup"><span data-stu-id="629dd-120">Use these opportunities for getting hello latest API versions:</span></span>

- <span data-ttu-id="629dd-121">REST API - [lista över alla resursproviders](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)</span><span class="sxs-lookup"><span data-stu-id="629dd-121">REST API - [List all resource providers](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)</span></span>
- <span data-ttu-id="629dd-122">PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)</span><span class="sxs-lookup"><span data-stu-id="629dd-122">PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)</span></span>
- <span data-ttu-id="629dd-123">Azure CLI 2.0 - [az provider show](https://docs.microsoft.com/cli/azure/provider#show)</span><span class="sxs-lookup"><span data-stu-id="629dd-123">Azure CLI 2.0 - [az provider show](https://docs.microsoft.com/cli/azure/provider#show)</span></span>

## <a name="parameters-and-variables"></a><span data-ttu-id="629dd-124">Parametrar och variabler</span><span class="sxs-lookup"><span data-stu-id="629dd-124">Parameters and variables</span></span>

<span data-ttu-id="629dd-125">[Parametrarna](../../resource-group-authoring-templates.md) gör det lättare för dig toospecify värden för hello mallen när den körs.</span><span class="sxs-lookup"><span data-stu-id="629dd-125">[Parameters](../../resource-group-authoring-templates.md) make it easy for you toospecify values for hello template when you run it.</span></span> <span data-ttu-id="629dd-126">Det här avsnittet parametrar används i hello exemplet:</span><span class="sxs-lookup"><span data-stu-id="629dd-126">This parameters section is used in hello example:</span></span>

```        
"parameters": {
  "adminUsername": { "type": "string" },
  "adminPassword": { "type": "securestring" },
  "numberOfInstances": { "type": "int" }
},
```

<span data-ttu-id="629dd-127">När du distribuerar hello exempelmall ange värden för hello namn och lösenord för administratörskontot för hello på varje virtuell dator och hello antal virtuella datorer toocreate.</span><span class="sxs-lookup"><span data-stu-id="629dd-127">When you deploy hello example template, you enter values for hello name and password of hello administrator account on each VM and hello number of VMs toocreate.</span></span> <span data-ttu-id="629dd-128">Du har hello alternativet för att ange parametervärden i en separat fil som hanteras med hello mall eller tillhandahåller värden när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="629dd-128">You have hello option of specifying parameter values in a separate file that's managed with hello template, or providing values when prompted.</span></span>

<span data-ttu-id="629dd-129">[Variabler](../../resource-group-authoring-templates.md) gör det lättare för dig tooset värden i hello mallen som används flera gånger i den eller som kan ändras med tiden.</span><span class="sxs-lookup"><span data-stu-id="629dd-129">[Variables](../../resource-group-authoring-templates.md) make it easy for you tooset up values in hello template that are used repeatedly throughout it or that can change over time.</span></span> <span data-ttu-id="629dd-130">Det här avsnittet för variabler som används i hello exemplet:</span><span class="sxs-lookup"><span data-stu-id="629dd-130">This variables section is used in hello example:</span></span>

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

<span data-ttu-id="629dd-131">När du distribuerar hello exempelmall används variabelvärden för hello namn och en identifierare för hello tidigare skapade lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="629dd-131">When you deploy hello example template, variable values are used for hello name and identifier of hello previously created storage account.</span></span> <span data-ttu-id="629dd-132">Variabler är också används tooprovide hello inställningar för diagnostik hello-tillägg.</span><span class="sxs-lookup"><span data-stu-id="629dd-132">Variables are also used tooprovide hello settings for hello diagnostic extension.</span></span> <span data-ttu-id="629dd-133">Använd hello [bästa praxis för att skapa mallar för Azure Resource Manager](../../resource-manager-template-best-practices.md) toohelp du bestämma hur du vill toostructure hello parametrar och variabler i mallen.</span><span class="sxs-lookup"><span data-stu-id="629dd-133">Use hello [best practices for creating Azure Resource Manager templates](../../resource-manager-template-best-practices.md) toohelp you decide how you want toostructure hello parameters and variables in your template.</span></span>

## <a name="resource-loops"></a><span data-ttu-id="629dd-134">Resursen slingor</span><span class="sxs-lookup"><span data-stu-id="629dd-134">Resource loops</span></span>

<span data-ttu-id="629dd-135">När du behöver mer än en virtuell dator för programmet, kan du använda en kopia-element i en mall.</span><span class="sxs-lookup"><span data-stu-id="629dd-135">When you need more than one virtual machine for your application, you can use a copy element in a template.</span></span> <span data-ttu-id="629dd-136">Det här valfria elementet loop genom att skapa hello antal virtuella datorer som du angav som en parameter:</span><span class="sxs-lookup"><span data-stu-id="629dd-136">This optional element loops through creating hello number of VMs that you specified as a parameter:</span></span>

```
"copy": {
  "name": "virtualMachineLoop", 
  "count": "[parameters('numberOfInstances')]"
},
```

<span data-ttu-id="629dd-137">I hello-exemplet som hello loopindexet används också när du anger några av hello värden för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="629dd-137">Also, notice in hello example that hello loop index is used when specifying some of hello values for hello resource.</span></span> <span data-ttu-id="629dd-138">Till exempel är om du har angett ett instansantal på tre, hello namnen på hello operativsystemet diskar myOSDisk1, myOSDisk2 och myOSDisk3:</span><span class="sxs-lookup"><span data-stu-id="629dd-138">For example, if you entered an instance count of three, hello names of hello operating system disks are myOSDisk1, myOSDisk2, and myOSDisk3:</span></span>

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
}
```

> [!NOTE] 
><span data-ttu-id="629dd-139">Det här exemplet använder hanterade diskar för hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="629dd-139">This example uses managed disks for hello virtual machines.</span></span>
>
>

<span data-ttu-id="629dd-140">Tänk på att skapa en loop för en resurs i mallen för hello kan kräva att du toouse hello loop när du skapar eller få åtkomst till andra resurser.</span><span class="sxs-lookup"><span data-stu-id="629dd-140">Keep in mind that creating a loop for one resource in hello template may require you toouse hello loop when creating or accessing other resources.</span></span> <span data-ttu-id="629dd-141">Flera virtuella datorer kan till exempel inte använda hello samma nätverksgränssnittet, så om din mall loop genom att skapa tre virtuella datorer det måste också gå igenom hur du skapar tre nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="629dd-141">For example, multiple VMs can’t use hello same network interface, so if your template loops through creating three VMs it must also loop through creating three network interfaces.</span></span> <span data-ttu-id="629dd-142">När du tilldelar en network interface tooa VM hello loopindexet är används tooidentify den:</span><span class="sxs-lookup"><span data-stu-id="629dd-142">When assigning a network interface tooa VM, hello loop index is used tooidentify it:</span></span>

```
"networkInterfaces": [ { 
  "id": "[resourceId('Microsoft.Network/networkInterfaces',
    concat('myNIC', copyindex()))]" 
} ]
```

## <a name="dependencies"></a><span data-ttu-id="629dd-143">Beroenden</span><span class="sxs-lookup"><span data-stu-id="629dd-143">Dependencies</span></span>

<span data-ttu-id="629dd-144">De flesta resurser beroende av andra resurser toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="629dd-144">Most resources depend on other resources toowork correctly.</span></span> <span data-ttu-id="629dd-145">Virtuella datorer måste vara kopplad till ett virtuellt nätverk och toodo måste ett nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="629dd-145">Virtual machines must be associated with a virtual network and toodo that it needs a network interface.</span></span> <span data-ttu-id="629dd-146">Hej [dependsOn](../../resource-group-define-dependencies.md) elementet har använt toomake att hello nätverksgränssnittet är klar toobe används innan hello virtuella datorer skapas:</span><span class="sxs-lookup"><span data-stu-id="629dd-146">hello [dependsOn](../../resource-group-define-dependencies.md) element is used toomake sure that hello network interface is ready toobe used before hello VMs are created:</span></span>

```
"dependsOn": [
  "[concat('Microsoft.Network/networkInterfaces/', 'myNIC', copyindex())]" 
],
```

<span data-ttu-id="629dd-147">Hanteraren för filserverresurser distribuerar parallellt alla resurser som inte är beroende av en annan resurs som ska distribueras.</span><span class="sxs-lookup"><span data-stu-id="629dd-147">Resource Manager deploys in parallel any resources that are not dependent on another resource being deployed.</span></span> <span data-ttu-id="629dd-148">Var försiktig när du ställer in beroenden eftersom du kan oavsiktligt göra distributionen genom att ange onödiga beroenden.</span><span class="sxs-lookup"><span data-stu-id="629dd-148">Be careful when setting dependencies because you can inadvertently slow your deployment by specifying unnecessary dependencies.</span></span> <span data-ttu-id="629dd-149">Beroenden kan kedja genom flera resurser.</span><span class="sxs-lookup"><span data-stu-id="629dd-149">Dependencies can chain through multiple resources.</span></span> <span data-ttu-id="629dd-150">Till exempel beroende hello nätverksgränssnittet hello offentlig IP-adress och nätverksresurser på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="629dd-150">For example, hello network interface depends on hello public IP address and virtual network resources.</span></span>

<span data-ttu-id="629dd-151">Hur vet du om det krävs ett beroende?</span><span class="sxs-lookup"><span data-stu-id="629dd-151">How do you know if a dependency is required?</span></span> <span data-ttu-id="629dd-152">Titta på hello värdena som du angett i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="629dd-152">Look at hello values you set in hello template.</span></span> <span data-ttu-id="629dd-153">Om ett element i hello virtuella resursdefinitionen pekar tooanother resurs som har distribuerats i hello samma mall du behöver ett beroende.</span><span class="sxs-lookup"><span data-stu-id="629dd-153">If an element in hello virtual machine resource definition points tooanother resource that is deployed in hello same template, you need a dependency.</span></span> <span data-ttu-id="629dd-154">Till exempel definierar den virtuella datorn exempel en nätverksprofil:</span><span class="sxs-lookup"><span data-stu-id="629dd-154">For example, your example virtual machine defines a network profile:</span></span>

```
"networkProfile": { 
  "networkInterfaces": [ { 
    "id": "[resourceId('Microsoft.Network/networkInterfaces',
      concat('myNIC', copyindex())]" 
  } ] 
},
```

<span data-ttu-id="629dd-155">tooset den här egenskapen hello nätverksgränssnitt måste finnas.</span><span class="sxs-lookup"><span data-stu-id="629dd-155">tooset this property, hello network interface must exist.</span></span> <span data-ttu-id="629dd-156">Därför måste ett beroende.</span><span class="sxs-lookup"><span data-stu-id="629dd-156">Therefore, you need a dependency.</span></span> <span data-ttu-id="629dd-157">Du måste också tooset ett beroende när en resurs (en underordnad) definieras inom en annan resurs (överordnad).</span><span class="sxs-lookup"><span data-stu-id="629dd-157">You also need tooset a dependency when one resource (a child) is defined within another resource (a parent).</span></span> <span data-ttu-id="629dd-158">Till exempel definieras hello diagnostikinställningarna-tillägg för anpassat skript för både som underordnade resurser till hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="629dd-158">For example, hello diagnostic settings and custom script extensions are both defined as child resources of hello virtual machine.</span></span> <span data-ttu-id="629dd-159">De kan inte skapas förrän hello virtuella datorn finns.</span><span class="sxs-lookup"><span data-stu-id="629dd-159">They cannot be created until hello virtual machine exists.</span></span> <span data-ttu-id="629dd-160">Därför är båda resurserna markerad som en beroende hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="629dd-160">Therefore, both resources are marked as dependent on hello virtual machine.</span></span>

## <a name="profiles"></a><span data-ttu-id="629dd-161">Profiler</span><span class="sxs-lookup"><span data-stu-id="629dd-161">Profiles</span></span>

<span data-ttu-id="629dd-162">Flera profil element används när du definierar en virtuell datorresurs.</span><span class="sxs-lookup"><span data-stu-id="629dd-162">Several profile elements are used when defining a virtual machine resource.</span></span> <span data-ttu-id="629dd-163">Vissa är obligatoriska och vissa är valfria.</span><span class="sxs-lookup"><span data-stu-id="629dd-163">Some are required and some are optional.</span></span> <span data-ttu-id="629dd-164">Till exempel hello hardwareProfile, osProfile, storageProfile och networkProfile-element krävs, men hello diagnosticsProfile är valfritt.</span><span class="sxs-lookup"><span data-stu-id="629dd-164">For example, hello hardwareProfile, osProfile, storageProfile, and networkProfile elements are required, but hello diagnosticsProfile is optional.</span></span> <span data-ttu-id="629dd-165">De här profilerna definiera inställningar som:</span><span class="sxs-lookup"><span data-stu-id="629dd-165">These profiles define settings such as:</span></span>
   
- [<span data-ttu-id="629dd-166">storlek</span><span class="sxs-lookup"><span data-stu-id="629dd-166">size</span></span>](sizes.md)
- <span data-ttu-id="629dd-167">[namnet](/architecture/best-practices/naming-conventions) och autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="629dd-167">[name](/architecture/best-practices/naming-conventions) and credentials</span></span>
- <span data-ttu-id="629dd-168">disk och [inställningar för operativsystem](cli-ps-findimage.md)</span><span class="sxs-lookup"><span data-stu-id="629dd-168">disk and [operating system settings](cli-ps-findimage.md)</span></span>
- [<span data-ttu-id="629dd-169">nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="629dd-169">network interface</span></span>](../../virtual-network/virtual-networks-multiple-nics.md) 
- <span data-ttu-id="629dd-170">Startdiagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="629dd-170">boot diagnostics</span></span>

## <a name="disks-and-images"></a><span data-ttu-id="629dd-171">Diskar och bilder</span><span class="sxs-lookup"><span data-stu-id="629dd-171">Disks and images</span></span>
   
<span data-ttu-id="629dd-172">I Azure, vhd-filer kan representera [diskar eller bilder](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="629dd-172">In Azure, vhd files can represent [disks or images](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="629dd-173">När hello operativsystem i en vhd-fil är särskilda toobe en specifik VM, är det hänvisade tooas en disk.</span><span class="sxs-lookup"><span data-stu-id="629dd-173">When hello operating system in a vhd file is specialized toobe a specific VM, it is referred tooas a disk.</span></span> <span data-ttu-id="629dd-174">När hello operativsystem i en vhd-fil är generaliserad toobe används toocreate många virtuella datorer, är det hänvisade tooas en bild.</span><span class="sxs-lookup"><span data-stu-id="629dd-174">When hello operating system in a vhd file is generalized toobe used toocreate many VMs, it is referred tooas an image.</span></span>   
    
### <a name="create-new-virtual-machines-and-new-disks-from-a-platform-image"></a><span data-ttu-id="629dd-175">Skapa nya virtuella datorer och nya diskar från en plattformsavbildning</span><span class="sxs-lookup"><span data-stu-id="629dd-175">Create new virtual machines and new disks from a platform image</span></span>

<span data-ttu-id="629dd-176">När du skapar en virtuell dator, måste du bestämma vilka operativsystem toouse.</span><span class="sxs-lookup"><span data-stu-id="629dd-176">When you create a VM, you must decide what operating system toouse.</span></span> <span data-ttu-id="629dd-177">Hej imageReference elementet har använt toodefine hello operativsystemet på en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="629dd-177">hello imageReference element is used toodefine hello operating system of a new VM.</span></span> <span data-ttu-id="629dd-178">hello exempel visas en definition för ett Windows Server-operativsystem:</span><span class="sxs-lookup"><span data-stu-id="629dd-178">hello example shows a definition for a Windows Server operating system:</span></span>

```
"imageReference": { 
  "publisher": "MicrosoftWindowsServer", 
  "offer": "WindowsServer", 
  "sku": "2012-R2-Datacenter", 
  "version": "latest" 
},
```

<span data-ttu-id="629dd-179">Om du vill toocreate en Linux-operativsystem, kan du använda den här definitionen:</span><span class="sxs-lookup"><span data-stu-id="629dd-179">If you want toocreate a Linux operating system, you might use this definition:</span></span>

```
"imageReference": {
  "publisher": "Canonical",
  "offer": "UbuntuServer",
  "sku": "14.04.2-LTS",
  "version": "latest"
},
```

<span data-ttu-id="629dd-180">Konfigurationsinställningar för hello operativsystemdisken tilldelas med hello osDisk element.</span><span class="sxs-lookup"><span data-stu-id="629dd-180">Configuration settings for hello operating system disk are assigned with hello osDisk element.</span></span> <span data-ttu-id="629dd-181">hello exempel definierar en ny hanterade disk med hello cachelagring läge har angetts för**ReadWrite** och hello disken skapas från en [plattformsavbildning](cli-ps-findimage.md):</span><span class="sxs-lookup"><span data-stu-id="629dd-181">hello example defines a new managed disk with hello caching mode set too**ReadWrite** and that hello disk is being created from a [platform image](cli-ps-findimage.md):</span></span>

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
},
```

### <a name="create-new-virtual-machines-from-existing-managed-disks"></a><span data-ttu-id="629dd-182">Skapa nya virtuella datorer från befintliga hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="629dd-182">Create new virtual machines from existing managed disks</span></span>

<span data-ttu-id="629dd-183">Om du vill toocreate virtuella datorer från befintliga diskar, ta bort hello imageReference och hello osProfile element och definiera diskinställningarna:</span><span class="sxs-lookup"><span data-stu-id="629dd-183">If you want toocreate virtual machines from existing disks, remove hello imageReference and hello osProfile elements and define these disk settings:</span></span>

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

### <a name="create-new-virtual-machines-from-a-managed-image"></a><span data-ttu-id="629dd-184">Skapa nya virtuella datorer från en hanterad avbildning</span><span class="sxs-lookup"><span data-stu-id="629dd-184">Create new virtual machines from a managed image</span></span>

<span data-ttu-id="629dd-185">Om du vill toocreate en virtuell dator från en hanterad avbildning, ändra hello imageReference element och definiera diskinställningarna:</span><span class="sxs-lookup"><span data-stu-id="629dd-185">If you want toocreate a virtual machine from a managed image, change hello imageReference element and define these disk settings:</span></span>

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

### <a name="attach-data-disks"></a><span data-ttu-id="629dd-186">Bifoga datadiskar</span><span class="sxs-lookup"><span data-stu-id="629dd-186">Attach data disks</span></span>

<span data-ttu-id="629dd-187">Du kan lägga till data diskar toohello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="629dd-187">You can optionally add data disks toohello VMs.</span></span> <span data-ttu-id="629dd-188">Hej [antal diskar](sizes.md) beror på hello storleken på operativsystemdisken som du använder.</span><span class="sxs-lookup"><span data-stu-id="629dd-188">hello [number of disks](sizes.md) depends on hello size of operating system disk that you use.</span></span> <span data-ttu-id="629dd-189">Med hello ange storleken på virtuella datorer hello tooStandard_DS1_v2 hello maximala antalet datadiskar som kunde läggas till toohello dem är två.</span><span class="sxs-lookup"><span data-stu-id="629dd-189">With hello size of hello VMs set tooStandard_DS1_v2, hello maximum number of data disks that could be added toohello them is two.</span></span> <span data-ttu-id="629dd-190">I exemplet hello läggs en hanterad datadisk tooeach VM:</span><span class="sxs-lookup"><span data-stu-id="629dd-190">In hello example, one managed data disk is being added tooeach VM:</span></span>

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

## <a name="extensions"></a><span data-ttu-id="629dd-191">Tillägg</span><span class="sxs-lookup"><span data-stu-id="629dd-191">Extensions</span></span>

<span data-ttu-id="629dd-192">Även om [tillägg](extensions-features.md) är en separat resurs, de är nära bundet tooVMs.</span><span class="sxs-lookup"><span data-stu-id="629dd-192">Although [extensions](extensions-features.md) are a separate resource, they are closely tied tooVMs.</span></span> <span data-ttu-id="629dd-193">Tillägg kan läggas till som en underordnad resurs av hello VM eller som en separat resurs.</span><span class="sxs-lookup"><span data-stu-id="629dd-193">Extensions can be added as a child resource of hello VM or as a separate resource.</span></span> <span data-ttu-id="629dd-194">hello exemplet visar hello [diagnostik tillägget](extensions-diagnostics-template.md) läggs toohello virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="629dd-194">hello example shows hello [Diagnostics Extension](extensions-diagnostics-template.md) being added toohello VMs:</span></span>

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

<span data-ttu-id="629dd-195">Tillägget resursen använder hello storageName variabeln och hello diagnostiska tooprovide variabelvärden.</span><span class="sxs-lookup"><span data-stu-id="629dd-195">This extension resource uses hello storageName variable and hello diagnostic variables tooprovide values.</span></span> <span data-ttu-id="629dd-196">Om du vill toochange hello data som samlas in med det här tillägget kan du lägga till flera prestandaräknare toohello wadperfcounters variabeln.</span><span class="sxs-lookup"><span data-stu-id="629dd-196">If you want toochange hello data that is collected by this extension, you can add more performance counters toohello wadperfcounters variable.</span></span> <span data-ttu-id="629dd-197">Du kan också välja tooput hello diagnostikdata till ett annat lagringskonto än där hello Virtuella diskar lagras.</span><span class="sxs-lookup"><span data-stu-id="629dd-197">You could also choose tooput hello diagnostics data into a different storage account than where hello VM disks are stored.</span></span>

<span data-ttu-id="629dd-198">Det finns många tillägg som du kan installera på en virtuell dator, men hello mest användbara är förmodligen hello [tillägget för anpassat skript](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="629dd-198">There are many extensions that you can install on a VM, but hello most useful is probably hello [Custom Script Extension](extensions-customscript.md).</span></span> <span data-ttu-id="629dd-199">Hello exempelvis körs ett PowerShell.skript som heter start.ps1 på varje virtuell dator när den startas:</span><span class="sxs-lookup"><span data-stu-id="629dd-199">In hello example, a PowerShell script named start.ps1 runs on each VM when it first starts:</span></span>

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

<span data-ttu-id="629dd-200">Hej start.ps1 skriptet kan utföra många konfigurationsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="629dd-200">hello start.ps1 script can accomplish many configuration tasks.</span></span> <span data-ttu-id="629dd-201">Till exempel har hello datadiskar som läggs toohello virtuella datorer i hello exempel inte initierats; Du kan använda ett anpassat skript tooinitialize dem.</span><span class="sxs-lookup"><span data-stu-id="629dd-201">For example, hello data disks that are added toohello VMs in hello example are not initialized; you can use a custom script tooinitialize them.</span></span> <span data-ttu-id="629dd-202">Om du har flera Start uppgifter toodo kan använda du hello start.ps1 filen toocall andra PowerShell-skript i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="629dd-202">If you have multiple startup tasks toodo, you can use hello start.ps1 file toocall other PowerShell scripts in Azure storage.</span></span> <span data-ttu-id="629dd-203">hello exempel använder PowerShell, men du kan använda valfri metod för skript som är tillgänglig på hello-operativsystemet som du använder.</span><span class="sxs-lookup"><span data-stu-id="629dd-203">hello example uses PowerShell, but you can use any scripting method that is available on hello operating system that you are using.</span></span>

<span data-ttu-id="629dd-204">Du kan se status för hello av hello installerat tillägg från hello tillägg inställningar i hello-portalen:</span><span class="sxs-lookup"><span data-stu-id="629dd-204">You can see hello status of hello installed extensions from hello Extensions settings in hello portal:</span></span>

![Hämta Åtgärdsstatus för tillägg](./media/template-description/virtual-machines-show-extensions.png)

<span data-ttu-id="629dd-206">Du kan också få tillägget information med hjälp av hello **Get-AzureRmVMExtension** PowerShell kommandot hello **vm-tillägget get** Azure CLI 2.0 kommando eller hello **hämta information om tillägg**  REST API.</span><span class="sxs-lookup"><span data-stu-id="629dd-206">You can also get extension information by using hello **Get-AzureRmVMExtension** PowerShell command, hello **vm extension get** Azure CLI 2.0 command, or hello **Get extension information** REST API.</span></span>

## <a name="deployments"></a><span data-ttu-id="629dd-207">Distributioner</span><span class="sxs-lookup"><span data-stu-id="629dd-207">Deployments</span></span>

<span data-ttu-id="629dd-208">När du distribuerar en mall, Azure spårar hello resurser som du har distribuerat som en grupp och automatiskt tilldelas en gruppnamnet toothis distribueras.</span><span class="sxs-lookup"><span data-stu-id="629dd-208">When you deploy a template, Azure tracks hello resources that you deployed as a group and automatically assigns a name toothis deployed group.</span></span> <span data-ttu-id="629dd-209">hello namnet på hello distribution är hello samma som hello hello mallens namn.</span><span class="sxs-lookup"><span data-stu-id="629dd-209">hello name of hello deployment is hello same as hello name of hello template.</span></span>

<span data-ttu-id="629dd-210">Om du är nyfiken hello status för resurser i hello distribution kan använda du hello resursgrupp-bladet i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="629dd-210">If you are curious about hello status of resources in hello deployment, you can use hello Resource Group blade in hello Azure portal:</span></span>

![Hämta information om distribution](./media/template-description/virtual-machines-deployment-info.png)
    
<span data-ttu-id="629dd-212">Det är inte ett problem toouse hello samma mall toocreate eller tooupdate befintliga resurser.</span><span class="sxs-lookup"><span data-stu-id="629dd-212">It’s not a problem toouse hello same template toocreate resources or tooupdate existing resources.</span></span> <span data-ttu-id="629dd-213">När du använder kommandona toodeploy mallar du har hello möjlighet toosay som [läge](../../resource-group-template-deploy.md) du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="629dd-213">When you use commands toodeploy templates, you have hello opportunity toosay which [mode](../../resource-group-template-deploy.md) you want toouse.</span></span> <span data-ttu-id="629dd-214">hello-läge kan anges tooeither **Slutför** eller **stegvis**.</span><span class="sxs-lookup"><span data-stu-id="629dd-214">hello mode can be set tooeither **Complete** or **Incremental**.</span></span> <span data-ttu-id="629dd-215">hello standardvärdet är toodo inkrementella uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="629dd-215">hello default is toodo incremental updates.</span></span> <span data-ttu-id="629dd-216">Var försiktig när du använder hello **Slutför** läge eftersom av misstag kan du ta bort resurser.</span><span class="sxs-lookup"><span data-stu-id="629dd-216">Be careful when using hello **Complete** mode because you may accidentally delete resources.</span></span> <span data-ttu-id="629dd-217">När du anger hello läge för**Slutför**, Resource Manager tar du bort alla resurser i hello resursgrupp som inte ingår i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="629dd-217">When you set hello mode too**Complete**, Resource Manager deletes any resources in hello resource group that are not in hello template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="629dd-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="629dd-218">Next Steps</span></span>

- <span data-ttu-id="629dd-219">Skapa en egen mall med hjälp av [redigera Azure Resource Manager-mallar](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="629dd-219">Create your own template using [Authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span>
- <span data-ttu-id="629dd-220">Distribuera hello-mallen som du skapat med [skapa en Windows-dator med en Resource Manager-mall](ps-template.md).</span><span class="sxs-lookup"><span data-stu-id="629dd-220">Deploy hello template that you created using [Create a Windows virtual machine with a Resource Manager template](ps-template.md).</span></span>
- <span data-ttu-id="629dd-221">Lär dig hur toomanage hello virtuella datorer som du skapade genom att granska [skapa och hantera virtuella Windows-datorer med hello Azure PowerShell-modulen](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="629dd-221">Learn how toomanage hello VMs that you created by reviewing [Create and manage Windows VMs with hello Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
