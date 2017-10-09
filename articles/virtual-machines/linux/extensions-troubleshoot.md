---
title: "aaaTroubleshooting Linux VM tillägget fel | Microsoft Docs"
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
ms.openlocfilehash: 29a0ca34207421e0014380000a313d3c44e7e594
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-linux-vm-extension-failures"></a><span data-ttu-id="392e4-103">Felsöka Azure Linux VM-tillägget fel</span><span class="sxs-lookup"><span data-stu-id="392e4-103">Troubleshooting Azure Linux VM extension failures</span></span>
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a><span data-ttu-id="392e4-104">Visa Tilläggsstatus för</span><span class="sxs-lookup"><span data-stu-id="392e4-104">Viewing extension status</span></span>
<span data-ttu-id="392e4-105">Azure Resource Manager-mallar kan köras från hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="392e4-105">Azure Resource Manager templates can be executed from hello  Azure CLI.</span></span> <span data-ttu-id="392e4-106">När hello mallen körs visas hello tillståndets status från Azure Resursläsaren eller hello kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="392e4-106">Once hello template is executed, hello extension status can be viewed from Azure Resource Explorer or hello command line tools.</span></span>

<span data-ttu-id="392e4-107">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="392e4-107">Here is an example:</span></span>

<span data-ttu-id="392e4-108">Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="392e4-108">Azure CLI:</span></span>

      azure vm get-instance-view


<span data-ttu-id="392e4-109">Här är exempel på utdata från hello:</span><span class="sxs-lookup"><span data-stu-id="392e4-109">Here is hello sample output:</span></span>

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
  <span data-ttu-id="392e4-110">]</span><span class="sxs-lookup"><span data-stu-id="392e4-110">]</span></span>

## <a name="troubleshooting-extenson-failures"></a><span data-ttu-id="392e4-111">Felsöka Extenson fel:</span><span class="sxs-lookup"><span data-stu-id="392e4-111">Troubleshooting Extenson failures:</span></span>
### <a name="re-running-hello-extension-on-hello-vm"></a><span data-ttu-id="392e4-112">Köra hello tillägg på hello VM</span><span class="sxs-lookup"><span data-stu-id="392e4-112">Re-running hello extension on hello VM</span></span>
<span data-ttu-id="392e4-113">Om du kör skript på hello VM som använder tillägget för anpassat skript kan du ibland stöter på ett fel där VM skapades men hello skriptet har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="392e4-113">If you are running scripts on hello VM using Custom Script Extension, you could sometimes run into an error where VM was created successfully but hello script has failed.</span></span> <span data-ttu-id="392e4-114">Under dessa villkor hello rekommenderat sätt toorecover från det här felet är tooremove hello tillägg och kör hello mallen igen.</span><span class="sxs-lookup"><span data-stu-id="392e4-114">Under these conditons, hello recommended way toorecover from this error is tooremove hello extension and rerun hello template again.</span></span>
<span data-ttu-id="392e4-115">Obs: I framtiden kan den här funktionen är utökad tooremove hello behöver för att avinstallera hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="392e4-115">Note: In future, this functionality would be enhanced tooremove hello need for uninstalling hello extension.</span></span>

#### <a name="remove-hello-extension-from-azure-cli"></a><span data-ttu-id="392e4-116">Hello tillägget tas bort från Azure CLI</span><span class="sxs-lookup"><span data-stu-id="392e4-116">Remove hello extension from Azure CLI</span></span>
      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

<span data-ttu-id="392e4-117">Där ”publsher-name” motsvarar toohello tilläggstypen från hello utdata ”azure vm get-instans-vyn” och hello namn hello tillägget resurs från hello mall</span><span class="sxs-lookup"><span data-stu-id="392e4-117">Where "publsher-name" corresponds toohello extension type from hello output of "azure vm get-instance-view" and name is hello name of hello extension resource from hello template</span></span>

<span data-ttu-id="392e4-118">När hello tillägget har tagits bort, kan hello mallen vara gång toorun hello skript på hello VM.</span><span class="sxs-lookup"><span data-stu-id="392e4-118">Once hello extension has been removed, hello template can be re-executed toorun hello scripts on hello VM.</span></span>

