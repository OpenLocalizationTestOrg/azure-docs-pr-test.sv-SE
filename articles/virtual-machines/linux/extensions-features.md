---
title: "Tillägg för virtuell dator och funktioner för Linux | Microsoft Docs"
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
ms.openlocfilehash: 8a5b39351f665c51ae7d83f755329e54ff3cf786
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a><span data-ttu-id="1adf7-103">Tillägg för virtuell dator och funktioner för Linux</span><span class="sxs-lookup"><span data-stu-id="1adf7-103">Virtual machine extensions and features for Linux</span></span>

<span data-ttu-id="1adf7-104">Tillägg för virtuell dator i Azure är små program som innehåller efter distributionen konfiguration och automatisering av uppgifter på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="1adf7-104">Azure virtual machine extensions are small applications that provide post-deployment configuration and automation tasks on Azure virtual machines.</span></span> <span data-ttu-id="1adf7-105">Om en virtuell dator kräver installation av programvara, anti-virusskydd eller Docker-konfiguration, kan en VM-tillägget användas utföra dessa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="1adf7-105">For example, if a virtual machine requires software installation, anti-virus protection, or Docker configuration, a VM extension can be used to complete these tasks.</span></span> <span data-ttu-id="1adf7-106">Azure VM-tillägg kan köras med hjälp av Azure CLI, PowerShell, Azure Resource Manager-mallar och Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1adf7-106">Azure VM extensions can be run using the Azure CLI, PowerShell, Azure Resource Manager templates, and the Azure portal.</span></span> <span data-ttu-id="1adf7-107">Tillägg kan levereras tillsammans med en ny distribution av virtuella datorer eller köra mot alla befintliga system.</span><span class="sxs-lookup"><span data-stu-id="1adf7-107">Extensions can be bundled with a new virtual machine deployment, or run against any existing system.</span></span>

<span data-ttu-id="1adf7-108">Det här dokumentet innehåller en översikt över krav för att använda Azure VM-tillägg på VM-tillägg och anvisningar om hur du identifierar, hantera och ta bort VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="1adf7-108">This document provides an overview of VM extensions, prerequisites for using Azure VM extensions, and guidance on how to detect, manage, and remove VM extensions.</span></span> <span data-ttu-id="1adf7-109">Det här dokumentet innehåller allmänna informationen eftersom många VM-tillägg är tillgängligt, var och en med en potentiellt unik konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1adf7-109">This document provides generalized information because many VM extensions are available, each with a potentially unique configuration.</span></span> <span data-ttu-id="1adf7-110">Tillägget-specifik information finns i varje dokument specifika för enskilda tillägg.</span><span class="sxs-lookup"><span data-stu-id="1adf7-110">Extension-specific details can be found in each document specific to the individual extension.</span></span>

## <a name="use-cases-and-samples"></a><span data-ttu-id="1adf7-111">Användningsfall och exempel</span><span class="sxs-lookup"><span data-stu-id="1adf7-111">Use cases and samples</span></span>

<span data-ttu-id="1adf7-112">Det finns flera olika Virtuella Azure-tillägg, var och en med en specifik användningsfall.</span><span class="sxs-lookup"><span data-stu-id="1adf7-112">Several different Azure VM extensions are available, each with a specific use case.</span></span> <span data-ttu-id="1adf7-113">Några exempel är:</span><span class="sxs-lookup"><span data-stu-id="1adf7-113">Some examples are:</span></span>

- <span data-ttu-id="1adf7-114">Använd PowerShell önskade tillstånd konfigurationer till en virtuell dator med hjälp av DSC-tillägg för Linux.</span><span class="sxs-lookup"><span data-stu-id="1adf7-114">Apply PowerShell Desired State configurations to a virtual machine using the DSC extension for Linux.</span></span> <span data-ttu-id="1adf7-115">Mer information finns i [tillägg för konfiguration av Azure önskade tillstånd](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).</span><span class="sxs-lookup"><span data-stu-id="1adf7-115">For more information, see [Azure Desired State configuration extension](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).</span></span>
- <span data-ttu-id="1adf7-116">Konfigurera övervakning av en virtuell dator med Microsoft Monitoring Agent VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="1adf7-116">Configure monitoring of a virtual machine with the Microsoft Monitoring Agent VM extension.</span></span> <span data-ttu-id="1adf7-117">Mer information finns i [hur du övervakar en Linux VM](tutorial-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="1adf7-117">For more information, see [How to monitor a Linux VM](tutorial-monitoring.md).</span></span>
- <span data-ttu-id="1adf7-118">Konfigurera övervakning av Azure-infrastrukturen med filnamnstillägget Datadog.</span><span class="sxs-lookup"><span data-stu-id="1adf7-118">Configure monitoring of your Azure infrastructure with the Datadog extension.</span></span> <span data-ttu-id="1adf7-119">Mer information finns i [Datadog blogg](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span><span class="sxs-lookup"><span data-stu-id="1adf7-119">For more information, see the [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span></span>
- <span data-ttu-id="1adf7-120">Konfigurera en Docker-värd på en virtuell Azure-dator med hjälp av Docker VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="1adf7-120">Configure a Docker host on an Azure virtual machine using the Docker VM extension.</span></span> <span data-ttu-id="1adf7-121">Mer information finns i [Docker VM-tillägget](dockerextension.md).</span><span class="sxs-lookup"><span data-stu-id="1adf7-121">For more information, see [Docker VM extension](dockerextension.md).</span></span>

<span data-ttu-id="1adf7-122">Förutom processpecifika tillägg är ett tillägg för anpassat skript tillgängligt för både Windows och Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1adf7-122">In addition to process-specific extensions, a Custom Script extension is available for both Windows and Linux virtual machines.</span></span> <span data-ttu-id="1adf7-123">Tillägget för anpassat skript för Linux kan alla Bash-skript ska köras på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1adf7-123">The Custom Script extension for Linux allows any Bash script to be run on a virtual machine.</span></span> <span data-ttu-id="1adf7-124">Anpassade skript är användbara för att utforma Azure-distributioner som kräver konfiguration efter vilken interna Azure verktygsuppsättning kan ge.</span><span class="sxs-lookup"><span data-stu-id="1adf7-124">Custom scripts are useful for designing Azure deployments that require configuration beyond what native Azure tooling can provide.</span></span> <span data-ttu-id="1adf7-125">Mer information finns i [tillägget för anpassat skript för Linux Virtuella](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="1adf7-125">For more information, see [Linux VM Custom Script extension](extensions-customscript.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="1adf7-126">Krav</span><span class="sxs-lookup"><span data-stu-id="1adf7-126">Prerequisites</span></span>

<span data-ttu-id="1adf7-127">Varje tillägg för virtuell dator kan ha en egen uppsättning krav.</span><span class="sxs-lookup"><span data-stu-id="1adf7-127">Each virtual machine extension might have its own set of prerequisites.</span></span> <span data-ttu-id="1adf7-128">Docker VM-tillägget har till exempel en förutsättning för en Linux-fördelning som stöds.</span><span class="sxs-lookup"><span data-stu-id="1adf7-128">For instance, the Docker VM extension has a prerequisite of a supported Linux distribution.</span></span> <span data-ttu-id="1adf7-129">Krav för enskilda tillägg beskrivs i dokumentationen för tillägget-specifika.</span><span class="sxs-lookup"><span data-stu-id="1adf7-129">Requirements of individual extensions are detailed in the extension-specific documentation.</span></span>

### <a name="azure-vm-agent"></a><span data-ttu-id="1adf7-130">Virtuell Azure-datoragent</span><span class="sxs-lookup"><span data-stu-id="1adf7-130">Azure VM agent</span></span>

<span data-ttu-id="1adf7-131">Virtuella Azure-agenten hanterar samverkan mellan en virtuell Azure-dator och Azure-infrastrukturkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="1adf7-131">The Azure VM agent manages interactions between an Azure virtual machine and the Azure fabric controller.</span></span> <span data-ttu-id="1adf7-132">Den Virtuella datoragenten är ansvarig för många funktioner för att distribuera och hantera virtuella Azure-datorer, inklusive kör VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="1adf7-132">The VM agent is responsible for many functional aspects of deploying and managing Azure virtual machines, including running VM extensions.</span></span> <span data-ttu-id="1adf7-133">Virtuella Azure-agenten är förinstallerat på Azure Marketplace-bilder och kan installeras manuellt på operativsystem som stöds.</span><span class="sxs-lookup"><span data-stu-id="1adf7-133">The Azure VM agent is preinstalled on Azure Marketplace images and can be installed manually on supported operating systems.</span></span>

<span data-ttu-id="1adf7-134">Information om operativsystem som stöds och installationsanvisningar finns [virtuella Azure-datorn agent](../windows/classic/agents-and-extensions.md).</span><span class="sxs-lookup"><span data-stu-id="1adf7-134">For information on supported operating systems and installation instructions, see [Azure virtual machine agent](../windows/classic/agents-and-extensions.md).</span></span>

## <a name="discover-vm-extensions"></a><span data-ttu-id="1adf7-135">Identifiera VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="1adf7-135">Discover VM extensions</span></span>

<span data-ttu-id="1adf7-136">Det finns många olika VM-tillägg för användning med virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="1adf7-136">Many different VM extensions are available for use with Azure virtual machines.</span></span> <span data-ttu-id="1adf7-137">Kör följande kommando om du vill se en fullständig lista med Azure CLI, ersätter Exempelplats med önskad plats.</span><span class="sxs-lookup"><span data-stu-id="1adf7-137">To see a complete list, run the following command with the Azure CLI, replacing the example location with the location of your choice.</span></span>

```azurecli
az vm extension image list --location westus -o table
```

## <a name="run-vm-extensions"></a><span data-ttu-id="1adf7-138">Kör VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="1adf7-138">Run VM extensions</span></span>

<span data-ttu-id="1adf7-139">Tillägg för virtuell Azure-dator kan köras på befintliga virtuella datorer som är användbara när du behöver göra konfigurationsändringar eller återställa anslutningen på en redan distribuerad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1adf7-139">Azure virtual machine extensions can be run on existing virtual machines, which are useful when you need to make configuration changes or recover connectivity on an already deployed VM.</span></span> <span data-ttu-id="1adf7-140">VM-tillägg kan också tillsammans med Azure Resource Manager mall-distributioner.</span><span class="sxs-lookup"><span data-stu-id="1adf7-140">VM extensions can also be bundled with Azure Resource Manager template deployments.</span></span> <span data-ttu-id="1adf7-141">Genom att använda tillägg med Resource Manager-mallar, virtuella Azure-datorer distribuerats och konfigurerats utan åtgärder från efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="1adf7-141">By using extensions with Resource Manager templates, Azure virtual machines can be deployed and configured without post-deployment intervention.</span></span>

<span data-ttu-id="1adf7-142">Följande metoder kan användas för att köra ett tillägg mot en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1adf7-142">The following methods can be used to run an extension against an existing virtual machine.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="1adf7-143">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1adf7-143">Azure CLI</span></span>

<span data-ttu-id="1adf7-144">Tillägg för virtuell Azure-dator kan köras mot en befintlig virtuell dator med hjälp av den `az vm extension set` kommando.</span><span class="sxs-lookup"><span data-stu-id="1adf7-144">Azure virtual machine extensions can be run against an existing virtual machine by using the `az vm extension set` command.</span></span> <span data-ttu-id="1adf7-145">Det här exemplet körs tillägget för anpassat skript mot en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1adf7-145">This example runs the custom script extension against a virtual machine.</span></span>

```azurecli
az vm extension set `
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

<span data-ttu-id="1adf7-146">Skriptet genererar utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="1adf7-146">The script produces output similar to the following text:</span></span>

```azurecli
info:    Executing command vm extension set
+ Looking up the VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a><span data-ttu-id="1adf7-147">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1adf7-147">Azure portal</span></span>

<span data-ttu-id="1adf7-148">VM-tillägg kan tillämpas på en befintlig virtuell dator via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1adf7-148">VM extensions can be applied to an existing virtual machine through the Azure portal.</span></span> <span data-ttu-id="1adf7-149">Om du vill göra det, markerar du den virtuella datorn, väljer **tillägg**, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1adf7-149">To do so, select the virtual machine, choose **Extensions**, and click **Add**.</span></span> <span data-ttu-id="1adf7-150">Välj de tillägg du vill använda från listan över tillgängliga tillägg och följ instruktionerna i guiden.</span><span class="sxs-lookup"><span data-stu-id="1adf7-150">Select the extension you want from the list of available extensions and follow the instructions in the wizard.</span></span>

<span data-ttu-id="1adf7-151">Följande bild visar installationen av tillägget för anpassat skript för Linux från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1adf7-151">The following image shows the installation of the Linux Custom Script extension from the Azure portal.</span></span>

![Installera tillägget för anpassat skript](./media/extensions-features/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="1adf7-153">Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="1adf7-153">Azure Resource Manager templates</span></span>

<span data-ttu-id="1adf7-154">VM-tillägg kan läggas till i en Azure Resource Manager-mall och utförs med distributionen av mallen.</span><span class="sxs-lookup"><span data-stu-id="1adf7-154">VM extensions can be added to an Azure Resource Manager template and executed with the deployment of the template.</span></span> <span data-ttu-id="1adf7-155">När du distribuerar ett tillägg med en mall kan skapa du helt konfigurerade Azure-distributioner.</span><span class="sxs-lookup"><span data-stu-id="1adf7-155">When you deploy an extension with a template, you can create fully configured Azure deployments.</span></span> <span data-ttu-id="1adf7-156">Till exempel hämtas följande JSON från en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="1adf7-156">For example, the following JSON is taken from a Resource Manager template.</span></span> <span data-ttu-id="1adf7-157">Mallen distribuerar en uppsättning belastningsutjämnade virtuella datorer och en Azure SQL database och installerar ett program med .NET Core på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1adf7-157">The template deploys a set of load-balanced virtual machines and an Azure SQL database, and then installs a .NET Core application on each VM.</span></span> <span data-ttu-id="1adf7-158">Tillägg för virtuell dator hand tar om installationen av programmet.</span><span class="sxs-lookup"><span data-stu-id="1adf7-158">The VM extension takes care of the software installation.</span></span>

<span data-ttu-id="1adf7-159">Mer information finns i hela [Resource Manager-mall](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="1adf7-159">For more information, see the full [Resource Manager template](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span>

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

<span data-ttu-id="1adf7-160">Mer information finns i [redigera Azure Resource Manager-mallar](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span><span class="sxs-lookup"><span data-stu-id="1adf7-160">For more information, see [Authoring Azure Resource Manager templates](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span></span>

## <a name="secure-vm-extension-data"></a><span data-ttu-id="1adf7-161">Säkra data för VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="1adf7-161">Secure VM extension data</span></span>

<span data-ttu-id="1adf7-162">När du kör en VM-tillägget måste vara det nödvändigt att inkludera känslig information, till exempel autentiseringsuppgifter, lagringskontonamn och åtkomst för lagringskontonycklar.</span><span class="sxs-lookup"><span data-stu-id="1adf7-162">When you're running a VM extension, it may be necessary to include sensitive information such as credentials, storage account names, and storage account access keys.</span></span> <span data-ttu-id="1adf7-163">Många VM-tillägg innehåller en skyddad konfiguration som krypterar data och dekrypterar dem bara inuti den virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="1adf7-163">Many VM extensions include a protected configuration that encrypts data and only decrypts it inside the target virtual machine.</span></span> <span data-ttu-id="1adf7-164">Varje tillägg innehåller en specifik skyddade konfigurationsschema och varje beskrivs i dokumentationen för specifika tillägg.</span><span class="sxs-lookup"><span data-stu-id="1adf7-164">Each extension has a specific protected configuration schema, and each is detailed in extension-specific documentation.</span></span>

<span data-ttu-id="1adf7-165">I följande exempel visas en instans av tillägget för anpassat skript för Linux.</span><span class="sxs-lookup"><span data-stu-id="1adf7-165">The following example shows an instance of the Custom Script extension for Linux.</span></span> <span data-ttu-id="1adf7-166">Observera att kommandot ska köras innehåller en uppsättning autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1adf7-166">Notice that the command to execute includes a set of credentials.</span></span> <span data-ttu-id="1adf7-167">I det här exemplet krypteras inte kommandot att köras.</span><span class="sxs-lookup"><span data-stu-id="1adf7-167">In this example, the command to execute will not be encrypted.</span></span>


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

<span data-ttu-id="1adf7-168">Flytta den **kommando att köra** egenskapen till den **skyddade** konfigurationen skyddar körning av strängen.</span><span class="sxs-lookup"><span data-stu-id="1adf7-168">Moving the **command to execute** property to the **protected** configuration secures the execution string.</span></span>

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

## <a name="troubleshoot-vm-extensions"></a><span data-ttu-id="1adf7-169">Felsökning av VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="1adf7-169">Troubleshoot VM extensions</span></span>

<span data-ttu-id="1adf7-170">Varje VM-tillägg kan ha felsökningssteg specifika tillägget.</span><span class="sxs-lookup"><span data-stu-id="1adf7-170">Each VM extension may have troubleshooting steps specific to the extension.</span></span> <span data-ttu-id="1adf7-171">Till exempel när du använder tillägget för anpassat skript finns körning skriptdetaljer lokalt på den virtuella datorn där tillägget kördes.</span><span class="sxs-lookup"><span data-stu-id="1adf7-171">For example, when you're using the Custom Script extension, script execution details can be found locally on the virtual machine on which the extension was run.</span></span> <span data-ttu-id="1adf7-172">Tillägget-specifika felsökning beskrivs i dokumentationen för tillägget-specifika.</span><span class="sxs-lookup"><span data-stu-id="1adf7-172">Any extension-specific troubleshooting steps are detailed in extension-specific documentation.</span></span>

<span data-ttu-id="1adf7-173">Följande felsökningssteg gäller för alla tillägg för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1adf7-173">The following troubleshooting steps apply to all virtual machine extensions.</span></span>

### <a name="view-extension-status"></a><span data-ttu-id="1adf7-174">Visa status för tillägg</span><span class="sxs-lookup"><span data-stu-id="1adf7-174">View extension status</span></span>

<span data-ttu-id="1adf7-175">När en virtuell dator tillägget har körts mot en virtuell dator, använder du följande kommando i Azure CLI Tilläggsstatus ska returneras.</span><span class="sxs-lookup"><span data-stu-id="1adf7-175">After a virtual machine extension has been run against a virtual machine, use the following Azure CLI command to return extension status.</span></span> <span data-ttu-id="1adf7-176">Ersätt exempel parameternamn med egna värden.</span><span class="sxs-lookup"><span data-stu-id="1adf7-176">Replace example parameter names with your own values.</span></span>

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

<span data-ttu-id="1adf7-177">Det ser ut som följande utdata:</span><span class="sxs-lookup"><span data-stu-id="1adf7-177">The output looks like the following text:</span></span>

```azurecli
AutoUpgradeMinorVersion    Location    Name          ProvisioningState    Publisher                   ResourceGroup      TypeHandlerVersion  VirtualMachineExtensionType
-------------------------  ----------  ------------  -------------------  --------------------------  ---------------  --------------------  -----------------------------
True                       westus      customScript  Succeeded            Microsoft.Azure.Extensions  exttest                             2  customScript
```

<span data-ttu-id="1adf7-178">Tillståndets status är körningen kan också finnas i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1adf7-178">Extension execution status can also be found in the Azure portal.</span></span> <span data-ttu-id="1adf7-179">Om du vill visa status för ett tillägg markerar du den virtuella datorn, väljer **tillägg**, och välj önskad tillägget.</span><span class="sxs-lookup"><span data-stu-id="1adf7-179">To view the status of an extension, select the virtual machine, choose **Extensions**, and select the desired extension.</span></span>

### <a name="rerun-a-vm-extension"></a><span data-ttu-id="1adf7-180">Kör en VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="1adf7-180">Rerun a VM extension</span></span>

<span data-ttu-id="1adf7-181">Det kan finnas fall där tillägget för virtuell dator behöver köras igen.</span><span class="sxs-lookup"><span data-stu-id="1adf7-181">There may be cases in which a virtual machine extension needs to be rerun.</span></span> <span data-ttu-id="1adf7-182">Du kan köra ett tillägg genom att ta bort den och sedan köra tillägget med en körning metod du föredrar.</span><span class="sxs-lookup"><span data-stu-id="1adf7-182">You can rerun an extension by removing it, and then rerunning the extension with an execution method of your choice.</span></span> <span data-ttu-id="1adf7-183">Kör följande kommando för att ta bort ett tillägg med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="1adf7-183">To remove an extension, run the following command with the Azure CLI.</span></span> <span data-ttu-id="1adf7-184">Ersätt exempel parameternamn med egna värden.</span><span class="sxs-lookup"><span data-stu-id="1adf7-184">Replace example parameter names with your own values.</span></span>

```azurecli
az vm extension delete --name customScript --resource-group myResourceGroup --vm-name myVM
```

<span data-ttu-id="1adf7-185">Du kan ta bort ett tillägg med hjälp av följande steg i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="1adf7-185">You can remove an extension by using the following steps in the Azure portal:</span></span>

1. <span data-ttu-id="1adf7-186">Välj en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1adf7-186">Select a virtual machine.</span></span>
2. <span data-ttu-id="1adf7-187">Välj **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="1adf7-187">Choose **Extensions**.</span></span>
3. <span data-ttu-id="1adf7-188">Välj önskad tillägget.</span><span class="sxs-lookup"><span data-stu-id="1adf7-188">Select the desired extension.</span></span>
4. <span data-ttu-id="1adf7-189">Välj **avinstallera**.</span><span class="sxs-lookup"><span data-stu-id="1adf7-189">Choose **Uninstall**.</span></span>

## <a name="common-vm-extension-reference"></a><span data-ttu-id="1adf7-190">Benämningen för VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="1adf7-190">Common VM extension reference</span></span>
| <span data-ttu-id="1adf7-191">Tilläggsnamn</span><span class="sxs-lookup"><span data-stu-id="1adf7-191">Extension name</span></span> | <span data-ttu-id="1adf7-192">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1adf7-192">Description</span></span> | <span data-ttu-id="1adf7-193">Mer information</span><span class="sxs-lookup"><span data-stu-id="1adf7-193">More information</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1adf7-194">Tillägget för anpassat skript för Linux</span><span class="sxs-lookup"><span data-stu-id="1adf7-194">Custom Script extension for Linux</span></span> |<span data-ttu-id="1adf7-195">Kör skript mot en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="1adf7-195">Run scripts against an Azure virtual machine</span></span> |[<span data-ttu-id="1adf7-196">Tillägget för anpassat skript för Linux</span><span class="sxs-lookup"><span data-stu-id="1adf7-196">Custom Script extension for Linux</span></span>](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| <span data-ttu-id="1adf7-197">Docker-tillägg</span><span class="sxs-lookup"><span data-stu-id="1adf7-197">Docker extension</span></span> |<span data-ttu-id="1adf7-198">Installera Docker-daemon för att stödja Docker fjärrkommandon.</span><span class="sxs-lookup"><span data-stu-id="1adf7-198">Install the Docker daemon to support remote Docker commands.</span></span> |[<span data-ttu-id="1adf7-199">Docker VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="1adf7-199">Docker VM extension</span></span>](dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| <span data-ttu-id="1adf7-200">VM Access-tillägg</span><span class="sxs-lookup"><span data-stu-id="1adf7-200">VM Access extension</span></span> |<span data-ttu-id="1adf7-201">Få åtkomst till en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="1adf7-201">Regain access to an Azure virtual machine</span></span> |[<span data-ttu-id="1adf7-202">VM Access-tillägg</span><span class="sxs-lookup"><span data-stu-id="1adf7-202">VM Access extension</span></span>](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| <span data-ttu-id="1adf7-203">Azure Diagnostics-tillägget</span><span class="sxs-lookup"><span data-stu-id="1adf7-203">Azure Diagnostics extension</span></span> |<span data-ttu-id="1adf7-204">Hantera Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="1adf7-204">Manage Azure Diagnostics</span></span> |[<span data-ttu-id="1adf7-205">Azure Diagnostics-tillägget</span><span class="sxs-lookup"><span data-stu-id="1adf7-205">Azure Diagnostics extension</span></span>](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| <span data-ttu-id="1adf7-206">Azure VM Access-tillägg</span><span class="sxs-lookup"><span data-stu-id="1adf7-206">Azure VM Access extension</span></span> |<span data-ttu-id="1adf7-207">Hantera användare och autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="1adf7-207">Manage users and credentials</span></span> |[<span data-ttu-id="1adf7-208">Tillägget för virtuell dator åtkomst för Linux</span><span class="sxs-lookup"><span data-stu-id="1adf7-208">VM Access extension for Linux</span></span>](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
