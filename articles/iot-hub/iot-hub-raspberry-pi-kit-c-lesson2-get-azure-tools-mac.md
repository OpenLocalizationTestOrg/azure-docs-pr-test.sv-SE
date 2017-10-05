---
title: 'Connect Raspberry PI (C) till Azure IoT - lektionen 2: Azure-verktyg (macOS) | Microsoft Docs'
description: "Installera Python och Azure-kommandoradsgränssnittet (Azure CLI) på macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: IOT cloud service, azure cli
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: f2d7d584-7734-401c-976c-81788a7282a3
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 3810990f4a27270fa45709f4d9dbb36a8f4369a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-macos-1010"></a>Hämta Azure-verktyg (macOS 10.10)
> [!div class="op_single_selector"]
> * [Windows 7 och senare](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a>Vad du ska göra
Installera Azure-kommandoradsgränssnittet (Azure CLI). Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

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

![Utdata som indikerar att det lyckades](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a>Sammanfattning
Du har installerat Azure CLI. Nästa uppgift är att skapa din Azure IoT hub- och enhetsidentitet med Azure CLI.

## <a name="next-steps"></a>Nästa steg
[Skapa din IoT-hubb och registrera hallon Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

