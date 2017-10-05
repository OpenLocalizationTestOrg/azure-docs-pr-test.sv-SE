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
# <a name="troubleshooting-azure-linux-vm-extension-failures"></a>Felsöka Azure Linux VM-tillägget fel
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Visa Tilläggsstatus för
Azure Resource Manager-mallar kan köras från Azure CLI. När mallen körs visas tilläggets status från Azure Resursläsaren eller kommandoradsverktyg.

Här är ett exempel:

Azure CLI:

      azure vm get-instance-view


Här är exempel på utdata:

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
### <a name="re-running-the-extension-on-the-vm"></a>Köra tillägget på den virtuella datorn
Om du kör skript på den virtuella datorn med hjälp av tillägget för anpassat skript, kan du ibland stöter på ett fel där VM skapades men inte det gick att skriptet. Under dessa villkor är det rekommenderade sättet att åtgärda felet för att tillägget tas bort och kör mallen igen.
Obs: I framtiden kommer den här funktionen skulle förbättras för att ta bort behovet av att avinstallera tillägget.

#### <a name="remove-the-extension-from-azure-cli"></a>Ta bort tillägget från Azure CLI
      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

Där ”publsher-name” motsvarar tilläggstypen från utdata från ”azure vm get-instans view” och namn är namnet på resursen tillägg från mallen

När tillägget har tagits bort går kan mallen köras igen att köra skript på den virtuella datorn.

