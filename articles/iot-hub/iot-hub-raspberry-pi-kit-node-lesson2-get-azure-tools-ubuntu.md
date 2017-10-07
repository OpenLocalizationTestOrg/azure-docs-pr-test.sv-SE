---
title: "Ansluta hallon Pi (nod) tooAzure IoT - lektionen 2: Hämta verktyg (Ubuntu) | Microsoft Docs"
description: "Installera Python och hello Azure-kommandoradsgränssnittet (Azure CLI) på Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT cloud service, azure cli
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 2f98923a-5274-4220-87d4-77ac8beb4d0f
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0adf91bef41f502e1333fdcc75cfb2fe912364c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a>Hämta Azure-verktyg (Ubuntu 16.04)
> [!div class="op_single_selector"]
> * [Windows 7 och senare](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a>Vad du ska göra
Installera hello Azure-kommandoradsgränssnittet (Azure CLI). Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Hur tooinstall hello Azure CLI.
* Hur tooadd en IoT-undergrupp till hello Azure CLI.

## <a name="what-you-need"></a>Vad du behöver
* En Ubuntu-dator med en Internet-anslutning.
* En aktiv Azure-prenumeration. Om du inte har ett konto kan du skapa en [ledigt utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.

## <a name="install-hello-azure-cli"></a>Installera hello Azure CLI
hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure, vilket gör att du toowork direkt från din kommandoradsverktyget tooprovision och hantera resurser.

tooinstall Hej senaste Azure CLI, gör du följande:

1. Kör följande kommandon i ett terminalfönster hello. Det kan ta fem minuter tooinstall hello Azure CLI.

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. Kontrollera hello installationen genom att köra följande kommando hello:

   ```bash
   az iot -h
   ```

Du bör se hello följande utdata om hello-installationen har slutförts.

![Utdata som indikerar att det lyckades](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a>Sammanfattning
Du har installerat hello Azure CLI. Nästa uppgift är toocreate dina Azure IoT-hubb och enheter identitet med hjälp av hello Azure CLI.

## <a name="next-steps"></a>Nästa steg
[Skapa din IoT-hubb och registrera hallon Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

