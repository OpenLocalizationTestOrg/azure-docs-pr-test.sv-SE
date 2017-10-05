---
title: 'Connect Intel EDISON (C) till Azure IoT - lektionen 2: Azure-verktyg (Windows) | Microsoft Docs'
description: "Installera Python och Azure-kommandoradsgränssnittet (Azure CLI) för Windows 7 och senare versioner."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure cli, iot-Molntjänsten, arduino moln"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 1035760e-cdd1-4d99-8003-067e98b29762
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 90ef113ae84ccc8cb3cbdbe8c245e138320a93c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a>Hämta Azure-verktyg (Windows 7 och senare)
> [!div class="op_single_selector"]
> * [Windows 7 och senare][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Vad du ska göra
Installera Python och Azure-kommandoradsgränssnittet (Azure CLI). Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Hur du installerar Python.
* Så här installerar du Azure CLI.

## <a name="what-you-need"></a>Vad du behöver
* En Windows-dator med en Internet-anslutning.
* En aktiv Azure-prenumeration. Om du inte har ett Azure-konto kan du skapa en [kostnadsfria Azure utvärderingskonto](http://azure.microsoft.com/pricing/free-trial/) i bara några minuter.

## <a name="install-python"></a>Installera Python
[Installera Python](https://www.python.org/downloads/) på din Windows-dator. Du kan installera Python 2.7, 3.4 eller 3.5. Den här kursen är baserad på Python 2.7. Om du redan har installerat Python, gå till nästa avsnitt och installera Azure CLI.

Du måste också lägga till sökvägen till de mappar där python.exe och pip.exe är installerade i systemet `PATH` miljövariabeln. Som standard installeras python.exe i `C:\Python27` och pip.exe installeras i `C:\Python27\Scripts`.

## <a name="install-the-azure-cli"></a>Installera Azure CLI
Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure. Du arbetar direkt från kommandoraden för att etablera och hantera resurser.

Så här installerar du Azure CLI:

1. Öppna ett kommandotolksfönster som administratör.
2. Kör följande kommandon:

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. Verifiera installationen genom att köra följande kommando:

   ```cmd
   az iot -h
   ```

Du ser i följande utdata om installationen har slutförts.

![Utdata som indikerar att det lyckades](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a>Sammanfattning
Du har installerat Azure CLI. Nästa uppgift att skapa din Azure IoT hub- och enhetsidentitet med Azure CLI.

## <a name="next-steps"></a>Nästa steg
[Skapa din IoT-hubb och registrera Intel modern][create-your-iot-hub-and-register-intel-edison]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md
