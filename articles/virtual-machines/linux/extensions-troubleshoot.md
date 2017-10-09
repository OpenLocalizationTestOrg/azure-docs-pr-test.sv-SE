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
# <a name="troubleshooting-azure-linux-vm-extension-failures"></a>Felsöka Azure Linux VM-tillägget fel
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Visa Tilläggsstatus för
Azure Resource Manager-mallar kan köras från hello Azure CLI. När hello mallen körs visas hello tillståndets status från Azure Resursläsaren eller hello kommandoradsverktyg.

Här är ett exempel:

Azure CLI:

      azure vm get-instance-view


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

## <a name="troubleshooting-extenson-failures"></a>Felsöka Extenson fel:
### <a name="re-running-hello-extension-on-hello-vm"></a>Köra hello tillägg på hello VM
Om du kör skript på hello VM som använder tillägget för anpassat skript kan du ibland stöter på ett fel där VM skapades men hello skriptet har misslyckats. Under dessa villkor hello rekommenderat sätt toorecover från det här felet är tooremove hello tillägg och kör hello mallen igen.
Obs: I framtiden kan den här funktionen är utökad tooremove hello behöver för att avinstallera hello-tillägget.

#### <a name="remove-hello-extension-from-azure-cli"></a>Hello tillägget tas bort från Azure CLI
      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

Där ”publsher-name” motsvarar toohello tilläggstypen från hello utdata ”azure vm get-instans-vyn” och hello namn hello tillägget resurs från hello mall

När hello tillägget har tagits bort, kan hello mallen vara gång toorun hello skript på hello VM.

