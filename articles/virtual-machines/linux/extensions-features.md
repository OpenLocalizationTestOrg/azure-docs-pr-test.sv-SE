---
title: "aaaVirtual datorn tillägg och funktioner för Linux | Microsoft Docs"
description: "Lär dig vilka tillägg som är tillgängliga för virtuella Azure-datorer, grupperade efter vad de tillhandahålla och förbättra."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: e0d2ce794c76815ccc6743e8788ee5d9d931e9a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a><span data-ttu-id="7d7dd-103">Tillägg för virtuell dator och funktioner för Linux</span><span class="sxs-lookup"><span data-stu-id="7d7dd-103">Virtual machine extensions and features for Linux</span></span>

<span data-ttu-id="7d7dd-104">Tillägg för virtuell dator i Azure är små program som innehåller efter distributionen konfiguration och automatisering av uppgifter på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-104">Azure virtual machine extensions are small applications that provide post-deployment configuration and automation tasks on Azure virtual machines.</span></span> <span data-ttu-id="7d7dd-105">Om en virtuell dator kräver installation av programvara, anti-virusskydd eller Docker-konfiguration, VM-tillägget kan exempelvis vara används toocomplete dessa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-105">For example, if a virtual machine requires software installation, anti-virus protection, or Docker configuration, a VM extension can be used toocomplete these tasks.</span></span> <span data-ttu-id="7d7dd-106">Azure VM-tillägg kan köras med hello Azure CLI, PowerShell, Azure Resource Manager-mallar och hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-106">Azure VM extensions can be run using hello Azure CLI, PowerShell, Azure Resource Manager templates, and hello Azure portal.</span></span> <span data-ttu-id="7d7dd-107">Tillägg kan levereras tillsammans med en ny distribution av virtuella datorer eller köra mot alla befintliga system.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-107">Extensions can be bundled with a new virtual machine deployment, or run against any existing system.</span></span>

<span data-ttu-id="7d7dd-108">Det här dokumentet innehåller en översikt över krav för att använda Azure VM-tillägg och vägledning om hur toodetect, hantera och ta bort VM-tillägg på VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-108">This document provides an overview of VM extensions, prerequisites for using Azure VM extensions, and guidance on how toodetect, manage, and remove VM extensions.</span></span> <span data-ttu-id="7d7dd-109">Det här dokumentet innehåller allmänna informationen eftersom många VM-tillägg är tillgängligt, var och en med en potentiellt unik konfiguration.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-109">This document provides generalized information because many VM extensions are available, each with a potentially unique configuration.</span></span> <span data-ttu-id="7d7dd-110">Tillägget-specifik information finns i varje dokument specifika toohello enskilda tillägg.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-110">Extension-specific details can be found in each document specific toohello individual extension.</span></span>

## <a name="use-cases-and-samples"></a><span data-ttu-id="7d7dd-111">Användningsfall och exempel</span><span class="sxs-lookup"><span data-stu-id="7d7dd-111">Use cases and samples</span></span>

<span data-ttu-id="7d7dd-112">Det finns flera olika Virtuella Azure-tillägg, var och en med en specifik användningsfall.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-112">Several different Azure VM extensions are available, each with a specific use case.</span></span> <span data-ttu-id="7d7dd-113">Några exempel är:</span><span class="sxs-lookup"><span data-stu-id="7d7dd-113">Some examples are:</span></span>

- <span data-ttu-id="7d7dd-114">Använd PowerShell önskade konfigurationer tooa virtuella datorn med hjälp av hello DSC-tillägg för Linux.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-114">Apply PowerShell Desired State configurations tooa virtual machine using hello DSC extension for Linux.</span></span> <span data-ttu-id="7d7dd-115">Mer information finns i [tillägg för konfiguration av Azure önskade tillstånd](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).</span><span class="sxs-lookup"><span data-stu-id="7d7dd-115">For more information, see [Azure Desired State configuration extension](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).</span></span>
- <span data-ttu-id="7d7dd-116">Konfigurera övervakning av en virtuell dator med hello Microsoft Monitoring Agent VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-116">Configure monitoring of a virtual machine with hello Microsoft Monitoring Agent VM extension.</span></span> <span data-ttu-id="7d7dd-117">Mer information finns i [hur toomonitor en Linux VM](tutorial-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="7d7dd-117">For more information, see [How toomonitor a Linux VM](tutorial-monitoring.md).</span></span>
- <span data-ttu-id="7d7dd-118">Konfigurera övervakning av Azure-infrastrukturen med hello Datadog tillägg.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-118">Configure monitoring of your Azure infrastructure with hello Datadog extension.</span></span> <span data-ttu-id="7d7dd-119">Mer information finns i hello [Datadog blogg](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span><span class="sxs-lookup"><span data-stu-id="7d7dd-119">For more information, see hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span></span>
- <span data-ttu-id="7d7dd-120">Konfigurera en Docker-värd på en virtuell Azure-dator med hjälp av hello Docker VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-120">Configure a Docker host on an Azure virtual machine using hello Docker VM extension.</span></span> <span data-ttu-id="7d7dd-121">Mer information finns i [Docker VM-tillägget](dockerextension.md).</span><span class="sxs-lookup"><span data-stu-id="7d7dd-121">For more information, see [Docker VM extension](dockerextension.md).</span></span>

<span data-ttu-id="7d7dd-122">Dessutom tooprocess-specifika tillägg, ett anpassat skript tillägg är tillgängligt för både Windows och Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-122">In addition tooprocess-specific extensions, a Custom Script extension is available for both Windows and Linux virtual machines.</span></span> <span data-ttu-id="7d7dd-123">hello tillägget för anpassat skript för Linux kan alla Bash-skript toobe som körs på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-123">hello Custom Script extension for Linux allows any Bash script toobe run on a virtual machine.</span></span> <span data-ttu-id="7d7dd-124">Anpassade skript är användbara för att utforma Azure-distributioner som kräver konfiguration efter vilken interna Azure verktygsuppsättning kan ge.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-124">Custom scripts are useful for designing Azure deployments that require configuration beyond what native Azure tooling can provide.</span></span> <span data-ttu-id="7d7dd-125">Mer information finns i [tillägget för anpassat skript för Linux Virtuella](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="7d7dd-125">For more information, see [Linux VM Custom Script extension](extensions-customscript.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="7d7dd-126">Krav</span><span class="sxs-lookup"><span data-stu-id="7d7dd-126">Prerequisites</span></span>

<span data-ttu-id="7d7dd-127">Varje tillägg för virtuell dator kan ha en egen uppsättning krav.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-127">Each virtual machine extension might have its own set of prerequisites.</span></span> <span data-ttu-id="7d7dd-128">Hej Docker VM-tillägget har till exempel en förutsättning för en Linux-fördelning som stöds.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-128">For instance, hello Docker VM extension has a prerequisite of a supported Linux distribution.</span></span> <span data-ttu-id="7d7dd-129">Krav av enskilda tillägg beskrivs i dokumentationen för hello-tillägg-specifika.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-129">Requirements of individual extensions are detailed in hello extension-specific documentation.</span></span>

### <a name="azure-vm-agent"></a><span data-ttu-id="7d7dd-130">Virtuell Azure-datoragent</span><span class="sxs-lookup"><span data-stu-id="7d7dd-130">Azure VM agent</span></span>

<span data-ttu-id="7d7dd-131">hello Azure VM-agenten hanterar samverkan mellan en virtuell Azure-dator och hello Azure-infrastrukturkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-131">hello Azure VM agent manages interactions between an Azure virtual machine and hello Azure fabric controller.</span></span> <span data-ttu-id="7d7dd-132">hello VM-agenten är ansvarig för många funktioner för att distribuera och hantera virtuella Azure-datorer, inklusive kör VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-132">hello VM agent is responsible for many functional aspects of deploying and managing Azure virtual machines, including running VM extensions.</span></span> <span data-ttu-id="7d7dd-133">hello Azure VM-agenten är förinstallerat på Azure Marketplace-bilder och kan installeras manuellt på operativsystem som stöds.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-133">hello Azure VM agent is preinstalled on Azure Marketplace images and can be installed manually on supported operating systems.</span></span>

<span data-ttu-id="7d7dd-134">Information om operativsystem som stöds och installationsanvisningar finns [virtuella Azure-datorn agent](../windows/classic/agents-and-extensions.md).</span><span class="sxs-lookup"><span data-stu-id="7d7dd-134">For information on supported operating systems and installation instructions, see [Azure virtual machine agent](../windows/classic/agents-and-extensions.md).</span></span>

## <a name="discover-vm-extensions"></a><span data-ttu-id="7d7dd-135">Identifiera VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="7d7dd-135">Discover VM extensions</span></span>

<span data-ttu-id="7d7dd-136">Det finns många olika VM-tillägg för användning med virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-136">Many different VM extensions are available for use with Azure virtual machines.</span></span> <span data-ttu-id="7d7dd-137">toosee en fullständig lista, kör hello följande kommando med hello Azure CLI, ersätta hello Exempelplats med hello plats.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-137">toosee a complete list, run hello following command with hello Azure CLI, replacing hello example location with hello location of your choice.</span></span>

```azurecli
az vm extension image list --location westus -o table
```

## <a name="run-vm-extensions"></a><span data-ttu-id="7d7dd-138">Kör VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="7d7dd-138">Run VM extensions</span></span>

<span data-ttu-id="7d7dd-139">Tillägg för virtuell Azure-dator kan köras på befintliga virtuella datorer som är användbara när du behöver toomake konfigurationsändringar eller återställa anslutningen på en redan distribuerad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-139">Azure virtual machine extensions can be run on existing virtual machines, which are useful when you need toomake configuration changes or recover connectivity on an already deployed VM.</span></span> <span data-ttu-id="7d7dd-140">VM-tillägg kan också tillsammans med Azure Resource Manager mall-distributioner.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-140">VM extensions can also be bundled with Azure Resource Manager template deployments.</span></span> <span data-ttu-id="7d7dd-141">Genom att använda tillägg med Resource Manager-mallar, virtuella Azure-datorer distribuerats och konfigurerats utan åtgärder från efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-141">By using extensions with Resource Manager templates, Azure virtual machines can be deployed and configured without post-deployment intervention.</span></span>

<span data-ttu-id="7d7dd-142">hello följande metoder kan vara används toorun filnamnstillägget mot en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-142">hello following methods can be used toorun an extension against an existing virtual machine.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="7d7dd-143">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7d7dd-143">Azure CLI</span></span>

<span data-ttu-id="7d7dd-144">Tillägg för virtuell Azure-dator kan köras mot en befintlig virtuell dator med hjälp av hello `az vm extension set` kommando.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-144">Azure virtual machine extensions can be run against an existing virtual machine by using hello `az vm extension set` command.</span></span> <span data-ttu-id="7d7dd-145">Det här exemplet körs hello tillägget för anpassat skript mot en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-145">This example runs hello custom script extension against a virtual machine.</span></span>

```azurecli
az vm extension set `
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

<span data-ttu-id="7d7dd-146">hello skriptet ger utdata liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="7d7dd-146">hello script produces output similar toohello following text:</span></span>

```azurecli
info:    Executing command vm extension set
+ Looking up hello VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a><span data-ttu-id="7d7dd-147">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7d7dd-147">Azure portal</span></span>

<span data-ttu-id="7d7dd-148">VM-tillägg kan vara tillämpade tooan befintliga virtuella datorn via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-148">VM extensions can be applied tooan existing virtual machine through hello Azure portal.</span></span> <span data-ttu-id="7d7dd-149">toodo Välj därför hello virtuell dator, Välj **tillägg**, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-149">toodo so, select hello virtual machine, choose **Extensions**, and click **Add**.</span></span> <span data-ttu-id="7d7dd-150">Välj hello tillägg du vill använda hello listan över tillgängliga tillägg och följ instruktionerna hello i hello guiden.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-150">Select hello extension you want from hello list of available extensions and follow hello instructions in hello wizard.</span></span>

<span data-ttu-id="7d7dd-151">hello visar följande bild hello installation av hello Linux anpassade skripttillägg från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-151">hello following image shows hello installation of hello Linux Custom Script extension from hello Azure portal.</span></span>

![Installera tillägget för anpassat skript](./media/extensions-features/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="7d7dd-153">Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="7d7dd-153">Azure Resource Manager templates</span></span>

<span data-ttu-id="7d7dd-154">VM-tillägg kan vara tillagda tooan Azure Resource Manager-mall och utförs med hello distribution av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-154">VM extensions can be added tooan Azure Resource Manager template and executed with hello deployment of hello template.</span></span> <span data-ttu-id="7d7dd-155">När du distribuerar ett tillägg med en mall kan skapa du helt konfigurerade Azure-distributioner.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-155">When you deploy an extension with a template, you can create fully configured Azure deployments.</span></span> <span data-ttu-id="7d7dd-156">Till exempel hello följande JSON hämtas från en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-156">For example, hello following JSON is taken from a Resource Manager template.</span></span> <span data-ttu-id="7d7dd-157">hello mallen distribuerar en uppsättning belastningsutjämnade virtuella datorer och en Azure SQL database och installerar sedan en .NET Core-programmet på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-157">hello template deploys a set of load-balanced virtual machines and an Azure SQL database, and then installs a .NET Core application on each VM.</span></span> <span data-ttu-id="7d7dd-158">hello VM-tillägget hand tar om hello Programvaruinstallation.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-158">hello VM extension takes care of hello software installation.</span></span>

<span data-ttu-id="7d7dd-159">Mer information finns i hello fullständig [Resource Manager-mall](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="7d7dd-159">For more information, see hello full [Resource Manager template](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span>

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
    }
}
```

<span data-ttu-id="7d7dd-160">Mer information finns i [redigera Azure Resource Manager-mallar](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span><span class="sxs-lookup"><span data-stu-id="7d7dd-160">For more information, see [Authoring Azure Resource Manager templates](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span></span>

## <a name="secure-vm-extension-data"></a><span data-ttu-id="7d7dd-161">Säkra data för VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="7d7dd-161">Secure VM extension data</span></span>

<span data-ttu-id="7d7dd-162">När du kör en VM-tillägget, kan det vara nödvändigt tooinclude känslig information, till exempel autentiseringsuppgifter, lagringskontonamn och åtkomst för lagringskontonycklar.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-162">When you're running a VM extension, it may be necessary tooinclude sensitive information such as credentials, storage account names, and storage account access keys.</span></span> <span data-ttu-id="7d7dd-163">Många VM-tillägg innehåller en skyddad konfiguration som krypterar data och dekrypterar dem bara inuti hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-163">Many VM extensions include a protected configuration that encrypts data and only decrypts it inside hello target virtual machine.</span></span> <span data-ttu-id="7d7dd-164">Varje tillägg innehåller en specifik skyddade konfigurationsschema och varje beskrivs i dokumentationen för specifika tillägg.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-164">Each extension has a specific protected configuration schema, and each is detailed in extension-specific documentation.</span></span>

<span data-ttu-id="7d7dd-165">hello som följande exempel visar en instans av hello tillägget för anpassat skript för Linux.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-165">hello following example shows an instance of hello Custom Script extension for Linux.</span></span> <span data-ttu-id="7d7dd-166">Observera att hello kommandot tooexecute innehåller en uppsättning autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-166">Notice that hello command tooexecute includes a set of credentials.</span></span> <span data-ttu-id="7d7dd-167">I det här exemplet kommer hello kommandot tooexecute inte krypteras.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-167">In this example, hello command tooexecute will not be encrypted.</span></span>


```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ],
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

<span data-ttu-id="7d7dd-168">Flytta hello **kommandot tooexecute** egenskapen toohello **skyddade** konfigurationen skyddar hello körning sträng.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-168">Moving hello **command tooexecute** property toohello **protected** configuration secures hello execution string.</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

## <a name="troubleshoot-vm-extensions"></a><span data-ttu-id="7d7dd-169">Felsökning av VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="7d7dd-169">Troubleshoot VM extensions</span></span>

<span data-ttu-id="7d7dd-170">Varje VM-tillägg kan ha felsökning steg specifika toohello tillägg.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-170">Each VM extension may have troubleshooting steps specific toohello extension.</span></span> <span data-ttu-id="7d7dd-171">Till exempel när du använder hello tillägget för anpassat skript finns körning skriptdetaljer lokalt på hello virtuell dator där hello tillägget kördes.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-171">For example, when you're using hello Custom Script extension, script execution details can be found locally on hello virtual machine on which hello extension was run.</span></span> <span data-ttu-id="7d7dd-172">Tillägget-specifika felsökning beskrivs i dokumentationen för tillägget-specifika.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-172">Any extension-specific troubleshooting steps are detailed in extension-specific documentation.</span></span>

<span data-ttu-id="7d7dd-173">hello följande felsökningssteg gäller tooall tillägg för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-173">hello following troubleshooting steps apply tooall virtual machine extensions.</span></span>

### <a name="view-extension-status"></a><span data-ttu-id="7d7dd-174">Visa status för tillägg</span><span class="sxs-lookup"><span data-stu-id="7d7dd-174">View extension status</span></span>

<span data-ttu-id="7d7dd-175">När ett tillägg för virtuell dator har körts mot en virtuell dator, kan du använda hello följande Azure CLI kommandot tooreturn tillståndets status.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-175">After a virtual machine extension has been run against a virtual machine, use hello following Azure CLI command tooreturn extension status.</span></span> <span data-ttu-id="7d7dd-176">Ersätt exempel parameternamn med egna värden.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-176">Replace example parameter names with your own values.</span></span>

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

<span data-ttu-id="7d7dd-177">hello utdata ser ut så hello följande text:</span><span class="sxs-lookup"><span data-stu-id="7d7dd-177">hello output looks like hello following text:</span></span>

```azurecli
AutoUpgradeMinorVersion    Location    Name          ProvisioningState    Publisher                   ResourceGroup      TypeHandlerVersion  VirtualMachineExtensionType
-------------------------  ----------  ------------  -------------------  --------------------------  ---------------  --------------------  -----------------------------
True                       westus      customScript  Succeeded            Microsoft.Azure.Extensions  exttest                             2  customScript
```

<span data-ttu-id="7d7dd-178">Tillståndets status är körningen kan också finnas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-178">Extension execution status can also be found in hello Azure portal.</span></span> <span data-ttu-id="7d7dd-179">tooview hello status för ett tillägg markerar hello virtuell dator, Välj **tillägg**, och välj hello önskade tillägg.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-179">tooview hello status of an extension, select hello virtual machine, choose **Extensions**, and select hello desired extension.</span></span>

### <a name="rerun-a-vm-extension"></a><span data-ttu-id="7d7dd-180">Kör en VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="7d7dd-180">Rerun a VM extension</span></span>

<span data-ttu-id="7d7dd-181">Det kan finnas fall där tillägget för virtuell dator måste toobe kör igen.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-181">There may be cases in which a virtual machine extension needs toobe rerun.</span></span> <span data-ttu-id="7d7dd-182">Du kan köra ett tillägg genom att ta bort den och sedan köra hello-tillägget med en körning metod du föredrar.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-182">You can rerun an extension by removing it, and then rerunning hello extension with an execution method of your choice.</span></span> <span data-ttu-id="7d7dd-183">tooremove ett tillägg, kör följande kommando med hello Azure CLI hello.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-183">tooremove an extension, run hello following command with hello Azure CLI.</span></span> <span data-ttu-id="7d7dd-184">Ersätt exempel parameternamn med egna värden.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-184">Replace example parameter names with your own values.</span></span>

```azurecli
az vm extension delete --name customScript --resource-group myResourceGroup --vm-name myVM
```

<span data-ttu-id="7d7dd-185">Du kan ta bort ett tillägg genom att använda hello följa stegen i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="7d7dd-185">You can remove an extension by using hello following steps in hello Azure portal:</span></span>

1. <span data-ttu-id="7d7dd-186">Välj en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-186">Select a virtual machine.</span></span>
2. <span data-ttu-id="7d7dd-187">Välj **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-187">Choose **Extensions**.</span></span>
3. <span data-ttu-id="7d7dd-188">Välj önskad hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-188">Select hello desired extension.</span></span>
4. <span data-ttu-id="7d7dd-189">Välj **avinstallera**.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-189">Choose **Uninstall**.</span></span>

## <a name="common-vm-extension-reference"></a><span data-ttu-id="7d7dd-190">Benämningen för VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="7d7dd-190">Common VM extension reference</span></span>
| <span data-ttu-id="7d7dd-191">Tilläggsnamn</span><span class="sxs-lookup"><span data-stu-id="7d7dd-191">Extension name</span></span> | <span data-ttu-id="7d7dd-192">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7d7dd-192">Description</span></span> | <span data-ttu-id="7d7dd-193">Mer information</span><span class="sxs-lookup"><span data-stu-id="7d7dd-193">More information</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7d7dd-194">Tillägget för anpassat skript för Linux</span><span class="sxs-lookup"><span data-stu-id="7d7dd-194">Custom Script extension for Linux</span></span> |<span data-ttu-id="7d7dd-195">Kör skript mot en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="7d7dd-195">Run scripts against an Azure virtual machine</span></span> |[<span data-ttu-id="7d7dd-196">Tillägget för anpassat skript för Linux</span><span class="sxs-lookup"><span data-stu-id="7d7dd-196">Custom Script extension for Linux</span></span>](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| <span data-ttu-id="7d7dd-197">Docker-tillägg</span><span class="sxs-lookup"><span data-stu-id="7d7dd-197">Docker extension</span></span> |<span data-ttu-id="7d7dd-198">Installera hello Docker daemon toosupport Docker fjärrkommandon.</span><span class="sxs-lookup"><span data-stu-id="7d7dd-198">Install hello Docker daemon toosupport remote Docker commands.</span></span> |[<span data-ttu-id="7d7dd-199">Docker VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="7d7dd-199">Docker VM extension</span></span>](dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| <span data-ttu-id="7d7dd-200">VM Access-tillägg</span><span class="sxs-lookup"><span data-stu-id="7d7dd-200">VM Access extension</span></span> |<span data-ttu-id="7d7dd-201">Återfå åtkomst tooan virtuella Azure-datorn</span><span class="sxs-lookup"><span data-stu-id="7d7dd-201">Regain access tooan Azure virtual machine</span></span> |[<span data-ttu-id="7d7dd-202">VM Access-tillägg</span><span class="sxs-lookup"><span data-stu-id="7d7dd-202">VM Access extension</span></span>](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| <span data-ttu-id="7d7dd-203">Azure Diagnostics-tillägget</span><span class="sxs-lookup"><span data-stu-id="7d7dd-203">Azure Diagnostics extension</span></span> |<span data-ttu-id="7d7dd-204">Hantera Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="7d7dd-204">Manage Azure Diagnostics</span></span> |[<span data-ttu-id="7d7dd-205">Azure Diagnostics-tillägget</span><span class="sxs-lookup"><span data-stu-id="7d7dd-205">Azure Diagnostics extension</span></span>](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| <span data-ttu-id="7d7dd-206">Azure VM Access-tillägg</span><span class="sxs-lookup"><span data-stu-id="7d7dd-206">Azure VM Access extension</span></span> |<span data-ttu-id="7d7dd-207">Hantera användare och autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="7d7dd-207">Manage users and credentials</span></span> |[<span data-ttu-id="7d7dd-208">Tillägget för virtuell dator åtkomst för Linux</span><span class="sxs-lookup"><span data-stu-id="7d7dd-208">VM Access extension for Linux</span></span>](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
