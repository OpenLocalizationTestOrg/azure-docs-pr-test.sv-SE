---
title: 'Ansluta Intel modern (nod) till Azure IoT - lektionen 2: Azure-verktyg (macOS) | Microsoft Docs'
description: "Installera Python och Azure-kommandoradsgränssnittet (Azure CLI) på macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure cli, iot-Molntjänsten, arduino moln"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 8a2a8031-b1a6-4219-b17d-2825550c35e1
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 16705d54e90ee1482822986ca8c581a192e8702f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-macos-1010"></a>Hämta Azure-verktyg (macOS 10.10)
> [!div class="op_single_selector"]
> * [Windows 7 och senare][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Vad du ska göra
Installera Azure-kommandoradsgränssnittet (Azure CLI). Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Så här installerar du Azure CLI.
* Hur du lägger till en IoT-undergrupp av Azure CLI.

## <a name="what-you-need"></a>Vad du behöver
* En Mac med en Internet-anslutning.
* En aktiv Azure-prenumeration. Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.

## <a name="install-python"></a>Installera Python
Även om macOS medföljer Python 2.7 direkt, rekommenderar vi att du installerar Python via Homebrew. Se [installerar Python på macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).

Installera Python och pip genom att köra följande kommando:

```bash
brew install python
```

## <a name="install-the-azure-cli"></a>Installera Azure CLI
Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure. Du arbetar direkt från kommandoraden för att etablera och hantera resurser. 

Följ dessa steg om du vill installera den senaste Azure CLI:

1. Kör följande kommandon i ett terminalfönster. Det kan ta fem minuter att installera Azure CLI.

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. Verifiera installationen genom att köra följande kommando:

   ```bash
   az iot -h
   ```

Du bör se följande utdata om installationen har slutförts.

![Utdata som indikerar att det lyckades](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a>Sammanfattning
Du har installerat Azure CLI. Nästa uppgift är att skapa din Azure IoT hub- och enhetsidentitet med Azure CLI.

## <a name="next-steps"></a>Nästa steg
[Skapa din IoT-hubb och registrera Intel modern][create-your-iot-hub-and-register-intel-edison]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-mac.md
