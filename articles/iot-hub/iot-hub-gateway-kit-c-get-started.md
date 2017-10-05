---
title: "SensorTag enhet & Azure IoT-Gateway - komma igång | Microsoft Docs"
description: "Kom igång med startpaket för IoT-Gateway, skapa din Azure IoT-hubb och ansluta SensorTag och Gateway för IoT-hubb"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure iot-hubb, iot-gateway, komma igång med internet saker, iot toolkit"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 56d05f4e-f2c1-4b22-8701-f01e14deead6
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 624bdc7877d5048da08897f868272fd8e8f3f7b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-sensortag"></a>Kom igång med IoT Gateway Startpaket med en SensorTag

> [!div class="op_single_selector"]
> * [SensorTag](iot-hub-gateway-kit-c-get-started.md)
> * [Simulerade enheten](iot-hub-gateway-kit-c-sim-get-started.md)

I den här självstudiekursen, börjar du med att lära dig grunderna i att arbeta med [IoT Gateway startpaket](https://aka.ms/gateway-kit). Du kommer att arbeta med Intel NUC som kör Linux vind floden och [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main). Du får lära dig hur seamleesly ansluta enheter till molnet med hjälp av Azure IoT Hub.

***
**Har ett kit än?:** klickar du på [här](https://aka.ms/gateway-kit). **Har inte en SensorTag?:** [börja med en simulerad enhet](iot-hub-gateway-kit-c-sim-get-started.md) eller [köpa en SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)
***

## <a name="lesson-1-configure-your-nuc"></a>Lektion 1: Konfigurera din NUC
![Diagram för Lesson1 slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-lesson1.png)

Installerar Azure IoT kanten på NUC nu bör du ställa in Intel NUC (nästa enhet för datorer) i Kit som en Azure IoT-gateway och kör en exempelapp för att verifiera gateway-funktionerna.

*Uppskattad tidsåtgång: 15 minuter*

Gå till [konfigurera Intel NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)

## <a name="lesson-2-create-your-iot-hub"></a>Lektion 2: Skapa din IoT-hubb
![Diagram för Lesson2 slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-lesson2.png)

Nu bör installera du de verktyg och program på värddatorn. Sedan du skapar din kostnadsfria Azure-konto, etablera Azure IoT-hubb och skapa din första enhet i IoT-hubben.

Slutför lektionen 1 innan du börjar övningen.

### <a name="get-the-tools"></a>Hämta verktygen
Installera de verktyg och program på värddatorn.

*Uppskattad tidsåtgång: 20 minuter*

Gå till [skaffa verktygen](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)

### <a name="create-an-iot-hub-and-register-your-device"></a>Skapa en IoT-hubb och registrera din enhet
Skapa en resursgrupp, etablera din första Azure IoT-hubb och Lägg till din första enhet IoT-hubben med hjälp av Azure CLI.

*Uppskattad tidsåtgång: 10 minuter*

Gå till [skapar en IoT-hubb och registrerar din enhet](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="lesson-3-receive-messages-from-sensortag-and-read-messages-from-your-iot-hub"></a>Lektionen 3: Ta emot meddelanden från SensorTag och läsa meddelanden från din IoT-hubb
I kursen ska du använda skript för att automatisera konfigurationen och körningen av ett exempelprogram TIVERA i din gateway. Sådana program använder en samling av moduler för aggregering och transformera data, bearbeta kommandon eller utföra ett antal olika relaterade uppgifter. Moduler som kommunicerar med varandra via en koordinator för meddelandet. Exempelprogrammet har en tabell modul och en IoT-hubb-modul. Modulen TIVERA tar emot data från TIVERA SensorTag. Modulen IoT-hubb paket mottagna data och skickar den till din IoT-hubb via gateway-ramverk i Azure IoT kant.

![Diagram över lektionen 3-slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-lesson3.png)

### <a name="configure-and-run-the-ble-sample-app"></a>Konfigurera och köra TIVERA sample-appen
Konfigurera anslutningen mellan SensorTag och din gateway. Sedan slutföra konfigurationen och köra exempelprogrammet tabell.

*Uppskattad tidsåtgång: 15 minuter*

Gå till [konfigurera och köra TIVERA sample-appen](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

### <a name="read-messages-from-your-iot-hub"></a>Läsa meddelanden från din IoT-hubb
Köra exempelkod på värddatorn för att läsa meddelanden från din IoT-hubb.

*Uppskattad tidsåtgång: 15 minuter*

Gå till [läsa meddelanden från din IoT-hubb](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)

## <a name="lesson-4-save-messages-to-azure-table-storage"></a>Lektion 4: Spara meddelanden i Azure Table Storage
Skapa en funktionsapp i Azure-som hämtar inkommande meddelanden från din IoT-hubb och skriver dem till Azure Table storage.

![Lektionen 4 slutpunkt till slutpunkt-diagram](media/iot-hub-gateway-kit-lessons/e2e-lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Skapa en Azure funktionsapp och Azure Storage-konto
Använd en mall för Azure Resource Manager för att skapa en funktionsapp i Azure-och ett Azure Storage-konto.

*Uppskattad tidsåtgång: 10 minuter*

Gå till [skapa en Azure funktionsapp och Azure Storage-konto](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)

### <a name="read-messages-persisted-in-azure-table-storage"></a>Läs meddelandena kvar i Azure Table storage
Övervaka gateway-to-cloud-meddelanden som de skrivs till Azure Table storage.

*Uppskattad tidsåtgång: 5 minuter*

Gå till [lästa meddelanden kvar i Azure Table storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).

## <a name="troubleshooting"></a>Felsökning
Om det uppstår problem under erfarenheter söka efter lösningar i den [felsökning](iot-hub-gateway-kit-c-troubleshooting.md) artikel.

## <a name="explore-more"></a>Utforska mer
Besök den [Intel IoT Gateway Kit developer zonen](http://software.intel.com/iot/microsoft-azure) vill veta mer.