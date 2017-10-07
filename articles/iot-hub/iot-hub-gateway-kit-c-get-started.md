---
title: "SensorTag enhet & Azure IoT-Gateway - komma igång | Microsoft Docs"
description: "Kom igång med startpaket för IoT-Gateway, skapa din Azure IoT-hubb och ansluta SensorTag och Gateway toohello IoT-hubb"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure iot-hubb, iot-gateway, komma igång med hello internet saker, iot toolkit"
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
ms.openlocfilehash: d8e4057e7774e43c069dd3f2f2e03f098c1ac844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-sensortag"></a>Kom igång med IoT Gateway Startpaket med en SensorTag

> [!div class="op_single_selector"]
> * [SensorTag](iot-hub-gateway-kit-c-get-started.md)
> * [Simulerade enheten](iot-hub-gateway-kit-c-sim-get-started.md)

I den här självstudiekursen börjar du med att lära sig hello grunderna i att arbeta med [IoT Gateway startpaket](https://aka.ms/gateway-kit). Du kommer att arbeta med Intel NUC som kör Linux vind floden och hello [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main). Får du lära dig hur tooseamleesly ansluta enheter toohello molnet med hjälp av Azure IoT Hub.

***
**Har ett kit än?:** klickar du på [här](https://aka.ms/gateway-kit). **Har inte en SensorTag?:** [börja med en simulerad enhet](iot-hub-gateway-kit-c-sim-get-started.md) eller [köpa en SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)
***

## <a name="lesson-1-configure-your-nuc"></a>Lektion 1: Konfigurera din NUC
![Diagram för Lesson1 slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-lesson1.png)

Installerar hello Azure IoT kanten på NUC nu bör du ställa in Intel NUC (nästa enhet för datorer) i hello Kit som en Azure IoT-gateway och kör ett exempel app tooverify hello gateway-funktioner.

*Uppskattad tid toocomplete: 15 minuter*

Gå för[konfigurera Intel NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)

## <a name="lesson-2-create-your-iot-hub"></a>Lektion 2: Skapa din IoT-hubb
![Diagram för Lesson2 slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-lesson2.png)

Nu bör installera du hello verktyg och program på värddatorn. Sedan du skapar din kostnadsfria Azure-konto, etablera Azure IoT-hubb och skapa din första enhet i hello IoT-hubb.

Slutför lektionen 1 innan du börjar övningen.

### <a name="get-hello-tools"></a>Hämta hello-verktyg
Installera hello verktyg och programvara på värddatorn.

*Uppskattad tid toocomplete: 20 minuter*

Gå för[hämta hello-verktyg](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)

### <a name="create-an-iot-hub-and-register-your-device"></a>Skapa en IoT-hubb och registrera din enhet
Skapa en resursgrupp, etablera din första Azure IoT-hubb och Lägg till din första enhet toohello IoT-hubb med hello Azure CLI.

*Uppskattad tid toocomplete: 10 minuter*

Gå för[skapar en IoT-hubb och registrerar din enhet](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="lesson-3-receive-messages-from-sensortag-and-read-messages-from-your-iot-hub"></a>Lektionen 3: Ta emot meddelanden från SensorTag och läsa meddelanden från din IoT-hubb
Nu bör ska du använda skript tooautomate hello konfiguration och körningen av ett exempelprogram TIVERA i din gateway. Sådana program använder en samling moduler tooaggregate och transformera data, bearbeta kommandon eller utföra ett antal olika relaterade uppgifter. Moduler som kommunicerar med varandra via en koordinator för meddelandet. hello exempelprogrammet har en tabell modul och en IoT-hubb-modul. hello TIVERA modulen tar emot data från TIVERA SensorTag. Hej IoT-hubb modulen paket hello data tas emot och skickar den tooyour IoT-hubb via hello gateway ramverk i Azure IoT kant.

![Diagram över lektionen 3-slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-lesson3.png)

### <a name="configure-and-run-hello-ble-sample-app"></a>Konfigurera och köra hello TIVERA sample-appen
Ställ in hello anslutningen mellan SensorTag och din gateway. Sedan slutföra hello konfigurationen och köra hello TIVERA exempelprogrammet.

*Uppskattad tid toocomplete: 15 minuter*

Gå för[konfigurera och köra hello TIVERA sample-appen](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

### <a name="read-messages-from-your-iot-hub"></a>Läsa meddelanden från din IoT-hubb
Köra exempelkod på din värd datorn tooread meddelanden från din IoT-hubb.

*Uppskattad tid toocomplete: 15 minuter*

Gå för[läsa meddelanden från din IoT-hubb](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)

## <a name="lesson-4-save-messages-tooazure-table-storage"></a>Lektionen 4: Spara meddelanden tooAzure Table storage
Skapa en funktionsapp i Azure-som hämtar inkommande meddelanden från din IoT-hubb och skriver dem tooAzure Table storage.

![Lektionen 4 slutpunkt till slutpunkt-diagram](media/iot-hub-gateway-kit-lessons/e2e-lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Skapa en Azure funktionsapp och Azure Storage-konto
Använda en Azure Resource Manager-mall toocreate en funktionsapp i Azure-och ett Azure Storage-konto.

*Uppskattad tid toocomplete: 10 minuter*

Gå för[skapa en Azure funktionsapp och Azure Storage-konto](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)

### <a name="read-messages-persisted-in-azure-table-storage"></a>Läs meddelandena kvar i Azure Table storage
Övervaka hälsningsmeddelande för gateway-to-cloud medan de skrivs tooAzure Table storage.

*Uppskattad tid toocomplete: 5 minuter*

Gå för[lästa meddelanden kvar i Azure Table storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).

## <a name="troubleshooting"></a>Felsökning
Om det uppstår problem under hello erfarenheter söka efter lösningar i hello [felsökning](iot-hub-gateway-kit-c-troubleshooting.md) artikel.

## <a name="explore-more"></a>Utforska mer
Besök hello [Intel IoT Gateway Kit developer zonen](http://software.intel.com/iot/microsoft-azure) toolearn mer.
