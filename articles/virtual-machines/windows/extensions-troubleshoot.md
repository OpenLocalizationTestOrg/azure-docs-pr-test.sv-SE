---
title: "Felsöka Windows VM-tillägget fel | Microsoft Docs"
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
ms.openlocfilehash: 4dba196e1b838f2092b30972fb070514bd2a4b25
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a><span data-ttu-id="06819-103">Felsöka Azure Windows VM-tillägget fel</span><span class="sxs-lookup"><span data-stu-id="06819-103">Troubleshooting Azure Windows VM extension failures</span></span>
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a><span data-ttu-id="06819-104">Visa Tilläggsstatus för</span><span class="sxs-lookup"><span data-stu-id="06819-104">Viewing extension status</span></span>
<span data-ttu-id="06819-105">Azure Resource Manager-mallar kan köras från Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="06819-105">Azure Resource Manager templates can be executed from Azure PowerShell.</span></span> <span data-ttu-id="06819-106">När mallen körs visas tilläggets status från Azure Resursläsaren eller kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="06819-106">Once the template is executed, the extension status can be viewed from Azure Resource Explorer or the command line tools.</span></span>

<span data-ttu-id="06819-107">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="06819-107">Here is an example:</span></span>

<span data-ttu-id="06819-108">Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="06819-108">Azure PowerShell:</span></span>

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

<span data-ttu-id="06819-109">Här är exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="06819-109">Here is the sample output:</span></span>

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
  <span data-ttu-id="06819-110">]</span><span class="sxs-lookup"><span data-stu-id="06819-110">]</span></span>

## <a name="troubleshooting-extension-failures"></a><span data-ttu-id="06819-111">Felsökning av tillägget fel</span><span class="sxs-lookup"><span data-stu-id="06819-111">Troubleshooting extension failures</span></span>
### <a name="re-running-the-extension-on-the-vm"></a><span data-ttu-id="06819-112">Köra tillägget på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="06819-112">Re-running the extension on the VM</span></span>
<span data-ttu-id="06819-113">Om du kör skript på den virtuella datorn med hjälp av tillägget för anpassat skript, kan du ibland stöter på ett fel där VM skapades men inte det gick att skriptet.</span><span class="sxs-lookup"><span data-stu-id="06819-113">If you are running scripts on the VM using Custom Script Extension, you could sometimes run into an error where VM was created successfully but the script has failed.</span></span> <span data-ttu-id="06819-114">Under dessa villkor är det rekommenderade sättet att åtgärda felet för att tillägget tas bort och kör mallen igen.</span><span class="sxs-lookup"><span data-stu-id="06819-114">Under these conditons, the recommended way to recover from this error is to remove the extension and rerun the template again.</span></span>
<span data-ttu-id="06819-115">Obs: I framtiden kommer den här funktionen skulle förbättras för att ta bort behovet av att avinstallera tillägget.</span><span class="sxs-lookup"><span data-stu-id="06819-115">Note: In future, this functionality would be enhanced to remove the need for uninstalling the extension.</span></span>

#### <a name="remove-the-extension-from-azure-powershell"></a><span data-ttu-id="06819-116">Ta bort tillägget från Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="06819-116">Remove the extension from Azure PowerShell</span></span>
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

<span data-ttu-id="06819-117">När tillägget har tagits bort går kan mallen köras igen att köra skript på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="06819-117">Once the extension has been removed, the template can be re-executed to run the scripts on the VM.</span></span>

