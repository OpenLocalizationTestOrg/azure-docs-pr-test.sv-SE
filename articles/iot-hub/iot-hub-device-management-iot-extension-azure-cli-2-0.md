---
title: "Azure IoT-enhetshantering med IoT-tillägg för Azure CLI 2.0 | Microsoft Docs"
description: "Använd IoT-tillägg för Azure CLI 2.0-verktyget för Azure IoT Hub-enhetshantering med direkta metoder och de två alternativ för egenskaper."
services: iot-hub
documentationcenter: 
author: chrissie926
manager: timlt
tags: 
keywords: "Azure iot enhetshantering, azure iot-hubb enhetshantering, device management iot, enhetshantering för iot-hubb"
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2018
ms.author: menchi
ms.openlocfilehash: 760a6a30513308aa59c5e253e3b91e28cf9e3241
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/18/2018
---
# <a name="use-the-iot-extension-for-azure-cli-20-for-azure-iot-hub-device-management"></a>Använd IoT-tillägg för Azure CLI 2.0 för hantering av Azure IoT Hub-enheter

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[IoT-tillägget för Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension) är en ny öppen källkod IoT-tillägg som lägger till funktioner i [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) innehåller kommandon för att interagera med Azure resource manager och hantering av slutpunkter. Azure CLI 2.0 innehåller kommandon för att interagera med Azure resource manager och hantering av slutpunkter. Du kan exempelvis använda Azure CLI 2.0 för att skapa en virtuell Azure-dator eller en IoT-hubb. En CLI-tillägget kan en Azure-tjänst att utöka Azure CLI ger dig åtkomst till ytterligare tjänstspecifika funktioner. IoT-tillägg ger utvecklare IoT kommandoraden åtkomst till alla IoT-hubb IoT kant och IoT-hubb enheten Etableringstjänsten funktioner.

| Hanteringsalternativ          | Aktivitet                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Direkta metoder             | Gör en enhet som agerar som startar eller stoppar skickar meddelanden eller starta om enheten.                                        |
| Dubbla önskade egenskaper    | Lägga till en enhet i vissa tillstånd, till exempel ange en Indikator till grönt eller inställningen telemetri skicka intervall till 30 minuter.         |
| Dubbla rapporterade egenskaper   | Hämta rapporterat status för en enhet. Till exempel rapporterar enheten Indikatorn blinkar nu.                                    |
| Dubbla taggar                  | Lagra enhetsspecifika metadata i molnet. Till exempel distributionsplatsen för en Varuautomat.                         |
| Enheten dubbla frågor        | Fråga alla enheten twins för att hämta dem med valfri villkor, till exempel identifiera de enheter som är tillgängliga för användning. |

Mer detaljerad förklaring om skillnaderna och vägledning om hur du använder dessa alternativ finns [enhet till moln kommunikation vägledning](iot-hub-devguide-d2c-guidance.md) och [moln till enhet kommunikation vägledning](iot-hub-devguide-c2d-guidance.md).

> [!NOTE]
> Enhetstvillingar är JSON-dokument som lagrar information om enhetstillstånd (metadata, konfigurationer och villkor). IoT-hubb kvarstår en enhet dubbla för varje enhet som ansluter till den. Läs mer om enheten twins [Kom igång med enheten twins](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Detta får du får lära dig

Du lär dig med olika alternativ på utvecklingsdatorn IoT-tillägg för Azure CLI 2.0.

## <a name="what-you-do"></a>Vad du gör

Kör Azure CLI 2.0 och IoT-tillägget för Azure CLI 2.0 med olika hanteringsalternativ för.

## <a name="what-you-need"></a>Vad du behöver

- Kursen [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md) slutförts som omfattar följande krav:
  - En aktiv Azure-prenumeration.
  - En Azure IoT-hubb i din prenumeration.
  - Ett klientprogram som skickar meddelanden till din Azure IoT-hubb.

- Kontrollera att enheten kör med klientprogrammet under den här kursen.

- [Python 2.7x eller Python 3.x](https://www.python.org/downloads/)

- Installera Azure CLI 2.0. Ett enkelt sätt att installera på Windows är att hämta och installera den [MSI](https://aka.ms/InstallAzureCliWindows). Du kan också följa installationsanvisningarna [Microsoft Docs](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) att konfigurera Azure CLI 2.0 i din miljö. Minst Azure CLI 2.0 måste du ha 2.0.24 eller senare. Använd `az –version` att validera. 

- Installera tillägget IoT. Det enklaste sättet är att köra `az extension add --name azure-cli-iot-ext`. [IoT-tillägget viktigt](https://github.com/Azure/azure-iot-cli-extension/blob/master/README.md) beskrivs flera olika sätt att installera tillägget.


## <a name="login-to-your-azure-account"></a>Logga in på ditt Azure-konto

Logga in på ditt Azure-konto genom att köra följande kommando:

```bash
az login
```

## <a name="use-the-iot-extension-for-azure-cli-20-with-direct-methods"></a>Använd IoT-tillägg för Azure CLI 2.0 med direkta metoder

```bash
az iot hub invoke-device-method --device-id <your device id> --hub-name <your hub name> --method-name <the method name> --method-payload <the method payload>
```

## <a name="use-the-iot-extension-for-azure-cli-20-with-twins-desired-properties"></a>Använd IoT-tillägg för Azure CLI 2.0 med dubblas egenskaper

Ange ett intervall önskade egenskapen = 3000 genom att köra följande kommando:

```bash
az iot hub device-twin update -n <your hub name> -d <your device id> --set properties.desired.interval = 3000
```

Den här egenskapen kan läsas från enheten.

## <a name="use-the-iot-extension-for-azure-cli-20-with-twins-reported-properties"></a>Använd IoT-tillägg för Azure CLI 2.0 med dubblas rapporterade egenskaper

Hämta rapporterade egenskaperna för enheten genom att köra följande kommando:

```bash
az iot hub device-twin update -n <your hub name> -d <your device id> --set properties.reported.interval = 3000
```

En av egenskaperna är $metadata. $lastUpdated som visar senast enheten skickar och tar emot ett meddelande.

## <a name="use-the-iot-extension-for-azure-cli-20-with-twins-tags"></a>Använd IoT-tillägg för Azure CLI 2.0 med dubblas taggar

Visa taggar och egenskaper för enheten genom att köra följande kommando:

```bash
az iot hub device-twin show --hub-name <your hub name> --device-id <your device id>
```

Lägga till en roll för fältet = temperatur & fuktighet till enheten genom att köra följande kommando:

```bash
az iot hub device-twin update --hub-name <your hub name> --device-id <your device id> --set tags = '{"role":"temperature&humidity"}}'
```

## <a name="use-the-iot-extension-for-azure-cli-20-with-device-twins-queries"></a>Välj IoT-tillägg för Azure CLI 2.0 med enheten twins frågor

Fråga enheter med en tagg av rollen = 'temperatur- och fuktighetskonsekvens' genom att köra följande kommando:

```bash
az iot hub query --hub-name <your hub name> --query-command "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Fråga alla enheter utom de som har en tagg av rollen = 'temperatur- och fuktighetskonsekvens' genom att köra följande kommando:

```bash
az iot hub query --hub-name <your hub name> --query-command "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Nästa steg

Du har lärt dig hur du övervakar meddelanden från enhet till moln och skicka meddelanden mellan din IoT-enhet och Azure IoT-hubb för moln till enhet.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]