---
title: "aaaExporting Azure-resursgrupper som innehåller VM-tillägg | Microsoft Docs"
description: "Exportera Resource Manager-mallar som inkluderar tillägg för virtuell dator."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7f4e2ca6-f1c7-4f59-a2cc-8f63132de279
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: nepeters
ms.openlocfilehash: cdbc2030988a19fe68429e8733dc60536c264abf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exporting-resource-groups-that-contain-vm-extensions"></a><span data-ttu-id="74c35-103">Exportera resursgrupper som innehåller VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="74c35-103">Exporting Resource Groups that contain VM extensions</span></span>

<span data-ttu-id="74c35-104">Azure resursgrupper kan exporteras till en ny Resource Manager-mall som sedan kan distribueras på nytt.</span><span class="sxs-lookup"><span data-stu-id="74c35-104">Azure Resource Groups can be exported into a new Resource Manager template that can then be redeployed.</span></span> <span data-ttu-id="74c35-105">hello exportprocessen tolkar befintliga resurser och skapar en Resource Manager-mall som när den distribuerats resulterar i en liknande resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="74c35-105">hello export process interprets existing resources, and creates a Resource Manager template that when deployed results in a similar Resource Group.</span></span> <span data-ttu-id="74c35-106">När du använder hello resursgruppen exportalternativ mot en resursgrupp som innehåller tillägg för virtuell dator, flera objekt behöver toobe betraktas som tillägget kompatibilitet och skyddade inställningar.</span><span class="sxs-lookup"><span data-stu-id="74c35-106">When using hello Resource Group export option against a Resource Group containing Virtual Machine extensions, several items need toobe considered such as extension compatibility and protected settings.</span></span>

<span data-ttu-id="74c35-107">Det här dokumentet beskriver hur hello resursgruppen exportprocessen fungerar om tillägg för virtuell dator, inklusive en lista över tillägg som stöds och information om hantering av skyddade data.</span><span class="sxs-lookup"><span data-stu-id="74c35-107">This document details how hello Resource Group export process works regarding virtual machine extensions, including a list of supported extensions, and details on handling secured data.</span></span>

## <a name="supported-virtual-machine-extensions"></a><span data-ttu-id="74c35-108">Tillägg för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="74c35-108">Supported Virtual Machine Extensions</span></span>

<span data-ttu-id="74c35-109">Det finns många tillägg för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="74c35-109">Many Virtual Machine extensions are available.</span></span> <span data-ttu-id="74c35-110">Inte alla tillägg kan exporteras till en Resource Manager-mall med hjälp av funktionen för hello ”Automatiseringsskriptet”.</span><span class="sxs-lookup"><span data-stu-id="74c35-110">Not all extensions can be exported into a Resource Manager template using hello “Automation Script” feature.</span></span> <span data-ttu-id="74c35-111">Om ett tillägg för virtuell dator inte stöds, måste det toobe placeras manuellt i hello exporterad mall.</span><span class="sxs-lookup"><span data-stu-id="74c35-111">If a virtual machine extension is not supported, it needs toobe manually placed back into hello exported template.</span></span>

<span data-ttu-id="74c35-112">hello kan följande tillägg exporteras med hello automation skript-funktionen.</span><span class="sxs-lookup"><span data-stu-id="74c35-112">hello following extensions can be exported with hello automation script feature.</span></span>

| <span data-ttu-id="74c35-113">Anknytning</span><span class="sxs-lookup"><span data-stu-id="74c35-113">Extension</span></span> ||||
|---|---|---|---|
| <span data-ttu-id="74c35-114">Acronis säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="74c35-114">Acronis Backup</span></span> | <span data-ttu-id="74c35-115">Datadog Windows-agenten</span><span class="sxs-lookup"><span data-stu-id="74c35-115">Datadog Windows Agent</span></span> | <span data-ttu-id="74c35-116">OS-korrigering för Linux</span><span class="sxs-lookup"><span data-stu-id="74c35-116">OS Patching For Linux</span></span> | <span data-ttu-id="74c35-117">VM-ögonblicksbild Linux</span><span class="sxs-lookup"><span data-stu-id="74c35-117">VM Snapshot Linux</span></span>
| <span data-ttu-id="74c35-118">Acronis säkerhetskopiering Linux</span><span class="sxs-lookup"><span data-stu-id="74c35-118">Acronis Backup Linux</span></span> | <span data-ttu-id="74c35-119">Docker-tillägg</span><span class="sxs-lookup"><span data-stu-id="74c35-119">Docker Extension</span></span> | <span data-ttu-id="74c35-120">Puppet-Agent</span><span class="sxs-lookup"><span data-stu-id="74c35-120">Puppet Agent</span></span> |
| <span data-ttu-id="74c35-121">BG Info</span><span class="sxs-lookup"><span data-stu-id="74c35-121">Bg Info</span></span> | <span data-ttu-id="74c35-122">DSC-tillägg</span><span class="sxs-lookup"><span data-stu-id="74c35-122">DSC Extension</span></span> | <span data-ttu-id="74c35-123">Plats 24 x 7 Apm Insight</span><span class="sxs-lookup"><span data-stu-id="74c35-123">Site 24x7 Apm Insight</span></span> |
| <span data-ttu-id="74c35-124">BMC CTM Agent Linux</span><span class="sxs-lookup"><span data-stu-id="74c35-124">BMC CTM Agent Linux</span></span> | <span data-ttu-id="74c35-125">Dynatrace Linux</span><span class="sxs-lookup"><span data-stu-id="74c35-125">Dynatrace Linux</span></span> | <span data-ttu-id="74c35-126">Linux platsservern 24 x 7</span><span class="sxs-lookup"><span data-stu-id="74c35-126">Site 24x7 Linux Server</span></span> |
| <span data-ttu-id="74c35-127">BMC CTM Agent Windows</span><span class="sxs-lookup"><span data-stu-id="74c35-127">BMC CTM Agent Windows</span></span> | <span data-ttu-id="74c35-128">Dynatrace Windows</span><span class="sxs-lookup"><span data-stu-id="74c35-128">Dynatrace Windows</span></span> | <span data-ttu-id="74c35-129">Windows platsservern 24 x 7</span><span class="sxs-lookup"><span data-stu-id="74c35-129">Site 24x7 Windows Server</span></span> |
| <span data-ttu-id="74c35-130">Chef klienten</span><span class="sxs-lookup"><span data-stu-id="74c35-130">Chef Client</span></span> | <span data-ttu-id="74c35-131">HPE säkerhet programmet Defender</span><span class="sxs-lookup"><span data-stu-id="74c35-131">HPE Security Application Defender</span></span> | <span data-ttu-id="74c35-132">Trend Micro DSA</span><span class="sxs-lookup"><span data-stu-id="74c35-132">Trend Micro DSA</span></span> |
| <span data-ttu-id="74c35-133">Anpassat skript</span><span class="sxs-lookup"><span data-stu-id="74c35-133">Custom Script</span></span> | <span data-ttu-id="74c35-134">IaaS program mot skadlig kod</span><span class="sxs-lookup"><span data-stu-id="74c35-134">IaaS Antimalware</span></span> | <span data-ttu-id="74c35-135">Trend Micro DSA Linux</span><span class="sxs-lookup"><span data-stu-id="74c35-135">Trend Micro DSA Linux</span></span> |
| <span data-ttu-id="74c35-136">Anpassat skripttillägg</span><span class="sxs-lookup"><span data-stu-id="74c35-136">Custom Script Extension</span></span> | <span data-ttu-id="74c35-137">IaaS-diagnostik</span><span class="sxs-lookup"><span data-stu-id="74c35-137">IaaS Diagnostics</span></span> | <span data-ttu-id="74c35-138">VM-åtkomst för Linux</span><span class="sxs-lookup"><span data-stu-id="74c35-138">VM Access For Linux</span></span> |
| <span data-ttu-id="74c35-139">Anpassat skript för Linux</span><span class="sxs-lookup"><span data-stu-id="74c35-139">Custom Script for Linux</span></span> | <span data-ttu-id="74c35-140">Klienten för Linux-Chef</span><span class="sxs-lookup"><span data-stu-id="74c35-140">Linux Chef Client</span></span> | <span data-ttu-id="74c35-141">VM-åtkomst för Linux</span><span class="sxs-lookup"><span data-stu-id="74c35-141">VM Access For Linux</span></span> |
| <span data-ttu-id="74c35-142">Datadog Linux-Agent</span><span class="sxs-lookup"><span data-stu-id="74c35-142">Datadog Linux Agent</span></span> | <span data-ttu-id="74c35-143">Linux-diagnostik</span><span class="sxs-lookup"><span data-stu-id="74c35-143">Linux Diagnostic</span></span> | <span data-ttu-id="74c35-144">VM-ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="74c35-144">VM Snapshot</span></span> |

## <a name="export-hello-resource-group"></a><span data-ttu-id="74c35-145">Exportera hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="74c35-145">Export hello Resource Group</span></span>

<span data-ttu-id="74c35-146">tooexport en resursgrupp till en återanvändbara mall klar hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="74c35-146">tooexport a Resource Group into a reusable template, complete hello following steps:</span></span>

1. <span data-ttu-id="74c35-147">Logga in toohello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="74c35-147">Sign in toohello Azure portal</span></span>
2. <span data-ttu-id="74c35-148">Hello Hubbmenyn, klicka på resursgrupper</span><span class="sxs-lookup"><span data-stu-id="74c35-148">On hello Hub Menu, click Resource Groups</span></span>
3. <span data-ttu-id="74c35-149">Välj hello målresursgruppen hello listan</span><span class="sxs-lookup"><span data-stu-id="74c35-149">Select hello target resource group from hello list</span></span>
4. <span data-ttu-id="74c35-150">I hello resursgrupp-bladet, klickar du på Automatiseringsskriptet</span><span class="sxs-lookup"><span data-stu-id="74c35-150">In hello Resource Group blade, click Automation Script</span></span>

![Exportera mall](./media/extensions-export-templates/template-export.png)

<span data-ttu-id="74c35-152">hello Azure Resource Manager automatiseringar skriptet skapar en Resource Manager-mall, en fil med parametrar och flera exempel på distribution av skript, till exempel PowerShell och Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="74c35-152">hello Azure Resource Manager automations script produces a Resource Manager template, a parameters file, and several sample deployment scripts such as PowerShell and Azure CLI.</span></span> <span data-ttu-id="74c35-153">Nu hello exporterad mall kan hämtas med hjälp av hello hämtningsknappen läggas till som en ny mall toohello mallbibliotek eller omdistribueras med hello knappen distribuera.</span><span class="sxs-lookup"><span data-stu-id="74c35-153">At this point, hello exported template can be downloaded using hello download button, added as a new template toohello template library, or redeployed using hello deploy button.</span></span>

## <a name="configure-protected-settings"></a><span data-ttu-id="74c35-154">Konfigurera inställningar för skyddade</span><span class="sxs-lookup"><span data-stu-id="74c35-154">Configure protected settings</span></span>

<span data-ttu-id="74c35-155">Många tillägg för virtuella Azure-datorn innehåller en skyddad Inställningskonfiguration som krypterar känsliga data, till exempel autentiseringsuppgifter och konfigurationssträngar.</span><span class="sxs-lookup"><span data-stu-id="74c35-155">Many Azure virtual machine extensions include a protected settings configuration, that encrypts sensitive data such as credentials and configuration strings.</span></span> <span data-ttu-id="74c35-156">Skyddade inställningarna exporteras inte med hello automation-skript.</span><span class="sxs-lookup"><span data-stu-id="74c35-156">Protected settings are not exported with hello automation script.</span></span> <span data-ttu-id="74c35-157">Om nödvändigt, skyddade inställningarna måste toobe gränsvärdet i hello exporteras utan mall.</span><span class="sxs-lookup"><span data-stu-id="74c35-157">If necessary, protected settings need toobe reinserted into hello exported templated.</span></span>

### <a name="step-1---remove-template-parameter"></a><span data-ttu-id="74c35-158">Steg 1 – ta bort mallparameter</span><span class="sxs-lookup"><span data-stu-id="74c35-158">Step 1 - Remove template parameter</span></span>

<span data-ttu-id="74c35-159">När hello resursgruppen exporteras, en enda mallparameter skapas tooprovide exporteras en värdet toohello skyddade inställningar.</span><span class="sxs-lookup"><span data-stu-id="74c35-159">When hello Resource Group is exported, a single template parameter is created tooprovide a value toohello exported protected settings.</span></span> <span data-ttu-id="74c35-160">Den här parametern kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="74c35-160">This parameter can be removed.</span></span> <span data-ttu-id="74c35-161">tooremove hello parametern titta igenom hello parameterlista och ta bort hello-parameter som ser ut ungefär toothis JSON-exemplet.</span><span class="sxs-lookup"><span data-stu-id="74c35-161">tooremove hello parameter, look through hello parameter list and delete hello parameter that looks similar toothis JSON example.</span></span>

```json
"extensions_extensionname_protectedSettings": {
    "defaultValue": null,
    "type": "SecureObject"
}
```

### <a name="step-2---get-protected-settings-properties"></a><span data-ttu-id="74c35-162">Steg 2 – Get skyddade egenskaper</span><span class="sxs-lookup"><span data-stu-id="74c35-162">Step 2 - Get protected settings properties</span></span>

<span data-ttu-id="74c35-163">En lista över de här egenskaperna måste toobe samlas in eftersom varje skyddad inställning har en uppsättning egenskaper som krävs.</span><span class="sxs-lookup"><span data-stu-id="74c35-163">Because each protected setting has a set of required properties, a list of these properties need toobe gathered.</span></span> <span data-ttu-id="74c35-164">Varje parameter för hello skyddade inställningarna konfiguration finns i hello [Azure Resource Manager schemat på GitHub](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json).</span><span class="sxs-lookup"><span data-stu-id="74c35-164">Each parameter of hello protected settings configuration can be found in hello [Azure Resource Manager schema on GitHub](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json).</span></span> <span data-ttu-id="74c35-165">Det här schemat innehåller endast hello parameteruppsättningar för hello-tillägg som nämns i hello översikt över i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="74c35-165">This schema only includes hello parameter sets for hello extensions listed in hello overview section of this document.</span></span> 

<span data-ttu-id="74c35-166">Från inom hello schema-lagringsplatsen, söka efter hello önskad tillägget i det här exemplet `IaaSDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="74c35-166">From within hello schema repository, search for hello desired extension, for this example `IaaSDiagnostics`.</span></span> <span data-ttu-id="74c35-167">En gång hello tillägg `protectedSettings` objekt har hittats, anteckna varje parameter.</span><span class="sxs-lookup"><span data-stu-id="74c35-167">Once hello extensions `protectedSettings` object has been located, take note of each parameter.</span></span> <span data-ttu-id="74c35-168">I exemplet hello av hello `IaasDiagnostic` tillägg, hello kräver parametrar är `storageAccountName`, `storageAccountKey`, och `storageAccountEndPoint`.</span><span class="sxs-lookup"><span data-stu-id="74c35-168">In hello example of hello `IaasDiagnostic` extension, hello require parameters are `storageAccountName`, `storageAccountKey`, and `storageAccountEndPoint`.</span></span>

```json
"protectedSettings": {
    "type": "object",
    "properties": {
        "storageAccountName": {
            "type": "string"
        },
        "storageAccountKey": {
            "type": "string"
        },
        "storageAccountEndPoint": {
            "type": "string"
        }
    },
    "required": [
        "storageAccountName",
        "storageAccountKey",
        "storageAccountEndPoint"
    ]
}
```

### <a name="step-3---re-create-hello-protected-configuration"></a><span data-ttu-id="74c35-169">Steg 3 – återskapa hello skyddade konfiguration</span><span class="sxs-lookup"><span data-stu-id="74c35-169">Step 3 - Re-create hello protected configuration</span></span>

<span data-ttu-id="74c35-170">Hej exporterad mall, söka efter på `protectedSettings` och Ersätt hello exporterade skyddade inställningsobjekt med en ny som innehåller hello krävs Tilläggsparametrarna och ett värde för varje kriterium.</span><span class="sxs-lookup"><span data-stu-id="74c35-170">On hello exported template, search for `protectedSettings` and replace hello exported protected setting object with a new one that includes hello required extension parameters and a value for each one.</span></span>

<span data-ttu-id="74c35-171">I exemplet hello av hello `IaasDiagnostic` tillägg, hello nya skyddade inställningen konfigurationen ser ut hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="74c35-171">In hello example of hello `IaasDiagnostic` extension, hello new protected setting configuration would look like hello following example:</span></span>

```json
"protectedSettings": {
    "storageAccountName": "[parameters('storageAccountName')]",
    "storageAccountKey": "[parameters('storageAccountKey')]",
    "storageAccountEndPoint": "https://core.windows.net"
}
```

<span data-ttu-id="74c35-172">hello Final = extension resource ser ut ungefär toohello följande JSON-exempel:</span><span class="sxs-lookup"><span data-stu-id="74c35-172">hello final extension resource looks similar toohello following JSON example:</span></span>

```json
{
    "name": "Microsoft.Insights.VMDiagnosticsSettings",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "[variables('apiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "tags": {
        "displayName": "AzureDiagnostics"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Diagnostics",
        "type": "IaaSDiagnostics",
        "typeHandlerVersion": "1.5",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
        },
        "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]",
            "storageAccountEndPoint": "https://core.windows.net"
        }
    }
}
```

<span data-ttu-id="74c35-173">Om du använder mallen parametrar tooprovide egenskapsvärden, måste dessa toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="74c35-173">If using template parameters tooprovide property values, these need toobe created.</span></span> <span data-ttu-id="74c35-174">När du skapar mallparametrar för skyddade inställningsvärden måste du toouse hello `SecureString` parametertypen så att känslig värden är skyddade.</span><span class="sxs-lookup"><span data-stu-id="74c35-174">When creating template parameters for protected setting values, make sure toouse hello `SecureString` parameter type so that sensitive values are secured.</span></span> <span data-ttu-id="74c35-175">Mer information om hur du använder parametrar finns [redigera Azure Resource Manager-mallar](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="74c35-175">For more information on using parameters, see [Authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="74c35-176">I exemplet hello av hello `IaasDiagnostic` tillägg, hello följande parametrar skulle skapas under hello parametrar i hello Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="74c35-176">In hello example of hello `IaasDiagnostic` extension, hello following parameters would be created in hello parameters section of hello Resource Manager template.</span></span>

```json
"storageAccountName": {
    "defaultValue": null,
    "type": "SecureString"
},
"storageAccountKey": {
    "defaultValue": null,
    "type": "SecureString"
}
```

<span data-ttu-id="74c35-177">Nu kan hello mallen distribueras med valfri metod för distribution av mallen.</span><span class="sxs-lookup"><span data-stu-id="74c35-177">At this point, hello template can be deployed using any template deployment method.</span></span>
