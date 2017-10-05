---
title: "Felsökning av Linux VM-tillägget fel | Microsoft Docs"
description: "Lär dig mer om hur du felsöker Azure Linux VM-tillägget fel"
services: virtual-machines-linux
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: top-support-issue,azure-resource-manager
ms.assetid: f05d93f3-42fc-4a09-9798-d92f7929c417
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: 589890de379d0b729de1f1ba9e604e0ec0496f50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-azure-linux-vm-extension-failures"></a><span data-ttu-id="faa2e-103">Felsöka Azure Linux VM-tillägget fel</span><span class="sxs-lookup"><span data-stu-id="faa2e-103">Troubleshooting Azure Linux VM extension failures</span></span>
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a><span data-ttu-id="faa2e-104">Visa Tilläggsstatus för</span><span class="sxs-lookup"><span data-stu-id="faa2e-104">Viewing extension status</span></span>
<span data-ttu-id="faa2e-105">Azure Resource Manager-mallar kan köras från Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="faa2e-105">Azure Resource Manager templates can be executed from the  Azure CLI.</span></span> <span data-ttu-id="faa2e-106">När mallen körs visas tilläggets status från Azure Resursläsaren eller kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="faa2e-106">Once the template is executed, the extension status can be viewed from Azure Resource Explorer or the command line tools.</span></span>

<span data-ttu-id="faa2e-107">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="faa2e-107">Here is an example:</span></span>

<span data-ttu-id="faa2e-108">Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="faa2e-108">Azure CLI:</span></span>

      azure vm get-instance-view


<span data-ttu-id="faa2e-109">Här är exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="faa2e-109">Here is the sample output:</span></span>

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
  <span data-ttu-id="faa2e-110">]</span><span class="sxs-lookup"><span data-stu-id="faa2e-110">]</span></span>

## <a name="troubleshooting-extenson-failures"></a><span data-ttu-id="faa2e-111">Felsöka Extenson fel:</span><span class="sxs-lookup"><span data-stu-id="faa2e-111">Troubleshooting Extenson failures:</span></span>
### <a name="re-running-the-extension-on-the-vm"></a><span data-ttu-id="faa2e-112">Köra tillägget på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="faa2e-112">Re-running the extension on the VM</span></span>
<span data-ttu-id="faa2e-113">Om du kör skript på den virtuella datorn med hjälp av tillägget för anpassat skript, kan du ibland stöter på ett fel där VM skapades men inte det gick att skriptet.</span><span class="sxs-lookup"><span data-stu-id="faa2e-113">If you are running scripts on the VM using Custom Script Extension, you could sometimes run into an error where VM was created successfully but the script has failed.</span></span> <span data-ttu-id="faa2e-114">Under dessa villkor är det rekommenderade sättet att åtgärda felet för att tillägget tas bort och kör mallen igen.</span><span class="sxs-lookup"><span data-stu-id="faa2e-114">Under these conditons, the recommended way to recover from this error is to remove the extension and rerun the template again.</span></span>
<span data-ttu-id="faa2e-115">Obs: I framtiden kommer den här funktionen skulle förbättras för att ta bort behovet av att avinstallera tillägget.</span><span class="sxs-lookup"><span data-stu-id="faa2e-115">Note: In future, this functionality would be enhanced to remove the need for uninstalling the extension.</span></span>

#### <a name="remove-the-extension-from-azure-cli"></a><span data-ttu-id="faa2e-116">Ta bort tillägget från Azure CLI</span><span class="sxs-lookup"><span data-stu-id="faa2e-116">Remove the extension from Azure CLI</span></span>
      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

<span data-ttu-id="faa2e-117">Där ”publsher-name” motsvarar tilläggstypen från utdata från ”azure vm get-instans view” och namn är namnet på resursen tillägg från mallen</span><span class="sxs-lookup"><span data-stu-id="faa2e-117">Where "publsher-name" corresponds to the extension type from the output of "azure vm get-instance-view" and name is the name of the extension resource from the template</span></span>

<span data-ttu-id="faa2e-118">När tillägget har tagits bort går kan mallen köras igen att köra skript på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="faa2e-118">Once the extension has been removed, the template can be re-executed to run the scripts on the VM.</span></span>

