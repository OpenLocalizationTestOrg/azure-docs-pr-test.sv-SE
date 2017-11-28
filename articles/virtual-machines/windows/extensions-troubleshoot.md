---
title: "aaaTroubleshooting Windows VM-tillägget fel | Microsoft Docs"
description: "Lär dig mer om hur du felsöker Azure Windows VM-tillägget fel"
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: top-support-issue,azure-resource-manager
ms.assetid: 878ab9b6-c3e6-40be-82d4-d77fecd5030f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: d24544743d9e0cb1a68ec9ab7718716f918054f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a><span data-ttu-id="9b964-103">Felsöka Azure Windows VM-tillägget fel</span><span class="sxs-lookup"><span data-stu-id="9b964-103">Troubleshooting Azure Windows VM extension failures</span></span>
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a><span data-ttu-id="9b964-104">Visa Tilläggsstatus för</span><span class="sxs-lookup"><span data-stu-id="9b964-104">Viewing extension status</span></span>
<span data-ttu-id="9b964-105">Azure Resource Manager-mallar kan köras från Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9b964-105">Azure Resource Manager templates can be executed from Azure PowerShell.</span></span> <span data-ttu-id="9b964-106">När hello mallen körs visas hello tillståndets status från Azure Resursläsaren eller hello kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="9b964-106">Once hello template is executed, hello extension status can be viewed from Azure Resource Explorer or hello command line tools.</span></span>

<span data-ttu-id="9b964-107">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="9b964-107">Here is an example:</span></span>

<span data-ttu-id="9b964-108">Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="9b964-108">Azure PowerShell:</span></span>

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

<span data-ttu-id="9b964-109">Här är exempel på utdata från hello:</span><span class="sxs-lookup"><span data-stu-id="9b964-109">Here is hello sample output:</span></span>

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  <span data-ttu-id="9b964-110">]</span><span class="sxs-lookup"><span data-stu-id="9b964-110">]</span></span>

## <a name="troubleshooting-extension-failures"></a><span data-ttu-id="9b964-111">Felsökning av tillägget fel</span><span class="sxs-lookup"><span data-stu-id="9b964-111">Troubleshooting extension failures</span></span>
### <a name="re-running-hello-extension-on-hello-vm"></a><span data-ttu-id="9b964-112">Köra hello tillägg på hello VM</span><span class="sxs-lookup"><span data-stu-id="9b964-112">Re-running hello extension on hello VM</span></span>
<span data-ttu-id="9b964-113">Om du kör skript på hello VM som använder tillägget för anpassat skript kan du ibland stöter på ett fel där VM skapades men hello skriptet har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="9b964-113">If you are running scripts on hello VM using Custom Script Extension, you could sometimes run into an error where VM was created successfully but hello script has failed.</span></span> <span data-ttu-id="9b964-114">Under dessa villkor hello rekommenderat sätt toorecover från det här felet är tooremove hello tillägg och kör hello mallen igen.</span><span class="sxs-lookup"><span data-stu-id="9b964-114">Under these conditons, hello recommended way toorecover from this error is tooremove hello extension and rerun hello template again.</span></span>
<span data-ttu-id="9b964-115">Obs: I framtiden kan den här funktionen är utökad tooremove hello behöver för att avinstallera hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="9b964-115">Note: In future, this functionality would be enhanced tooremove hello need for uninstalling hello extension.</span></span>

#### <a name="remove-hello-extension-from-azure-powershell"></a><span data-ttu-id="9b964-116">Hello tillägget tas bort från Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9b964-116">Remove hello extension from Azure PowerShell</span></span>
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

<span data-ttu-id="9b964-117">När hello tillägget har tagits bort, kan hello mallen vara gång toorun hello skript på hello VM.</span><span class="sxs-lookup"><span data-stu-id="9b964-117">Once hello extension has been removed, hello template can be re-executed toorun hello scripts on hello VM.</span></span>

