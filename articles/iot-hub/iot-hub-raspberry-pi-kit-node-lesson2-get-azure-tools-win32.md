---
title: "Ansluta hallon Pi (nod) tooAzure IoT - lektionen 2: Hämta verktyg (Windows) | Microsoft Docs"
description: "Installera Python och hello Azure-kommandoradsgränssnittet (Azure CLI) för Windows 7 och senare versioner."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT cloud service, azure cli
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: acfa13e3-6d2c-4e68-9a77-1cbc2cf3f9c1
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 25b50214322137ea32861fd1131c474e2fc7ebb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a>Hämta Azure-verktyg (Windows 7 och senare)
> [!div class="op_single_selector"]
> * [Windows 7 och senare](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a>Vad du ska göra
Installera Python och hello Azure-kommandoradsgränssnittet (Azure CLI). Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Hur tooinstall Python.
* Hur tooinstall hello Azure CLI.

## <a name="what-you-need"></a>Vad du behöver
* En Windows-dator med en Internet-anslutning.
* En aktiv Azure-prenumeration. Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.

## <a name="install-python"></a>Installera Python
[Installera Python](https://www.python.org/downloads/) på din Windows-dator. Du kan installera Python 2.7, 3.4 eller 3.5. Den här kursen är baserad på Python 2.7. Om du redan har installerat Python gå toohello nästa avsnitt och installera hello Azure CLI.

Du måste också tooadd hello sökvägen hello mappar där python.exe och pip.exe är installerade toohello system `PATH` miljövariabeln. Som standard installeras python.exe i `C:\Python27` och pip.exe installeras i `C:\Python27\Scripts`.

## <a name="install-hello-azure-cli"></a>Installera hello Azure CLI
hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure. Du arbetar direkt från din kommandoradsverktyget tooprovision och hantera resurser.

tooinstall hello Azure CLI, Följ dessa steg:

1. Öppna ett kommandotolksfönster som administratör.
2. Kör följande kommandon hello:

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. Kontrollera hello installationen genom att köra följande kommando hello:

   ```bash
   az iot -h
   ```

Du kan se hello följande utdata om hello-installationen har slutförts.

![Utdata som indikerar att det lyckades](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a>Sammanfattning
Du har installerat hello Azure CLI. Nästa uppgift toocreate dina Azure IoT hub- och enhetsidentitet med hjälp av hello Azure CLI.

## <a name="next-steps"></a>Nästa steg
[Skapa din IoT-hubb och registrera hallon Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

