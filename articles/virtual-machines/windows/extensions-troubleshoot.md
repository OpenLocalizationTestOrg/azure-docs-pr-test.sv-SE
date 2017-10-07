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
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Felsöka Azure Windows VM-tillägget fel
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Visa Tilläggsstatus för
Azure Resource Manager-mallar kan köras från Azure PowerShell. När hello mallen körs visas hello tillståndets status från Azure Resursläsaren eller hello kommandoradsverktyg.

Här är ett exempel:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Här är exempel på utdata från hello:

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
  ]

## <a name="troubleshooting-extension-failures"></a>Felsökning av tillägget fel
### <a name="re-running-hello-extension-on-hello-vm"></a>Köra hello tillägg på hello VM
Om du kör skript på hello VM som använder tillägget för anpassat skript kan du ibland stöter på ett fel där VM skapades men hello skriptet har misslyckats. Under dessa villkor hello rekommenderat sätt toorecover från det här felet är tooremove hello tillägg och kör hello mallen igen.
Obs: I framtiden kan den här funktionen är utökad tooremove hello behöver för att avinstallera hello-tillägget.

#### <a name="remove-hello-extension-from-azure-powershell"></a>Hello tillägget tas bort från Azure PowerShell
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

När hello tillägget har tagits bort, kan hello mallen vara gång toorun hello skript på hello VM.

