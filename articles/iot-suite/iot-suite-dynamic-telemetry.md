---
title: aaaUse dynamiska telemetri | Microsoft Docs
description: "Följ den här självstudiekursen toolearn hur toouse dynamiska telemetri med hello Azure IoT Suite fjärrövervaknings förkonfigurerade lösningen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 06cb2a370b67b4950efdfa4c7d906ac92106f4a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-dynamic-telemetry-with-hello-remote-monitoring-preconfigured-solution"></a>Använda dynamisk telemetri med hello remote förkonfigurerade övervakningslösning

Dynamisk telemetri gör du toovisualize alla telemetri skickas toohello remote förkonfigurerade övervakningslösning. hello simulerade enheter som distribueras med hello förkonfigurerade lösningen skickar temperatur- och fuktighetskonsekvens telemetri som du visualisera på hello instrumentpanel. Om du anpassar befintliga simulerade enheter, skapa nya simulerade enheter eller ansluta fysiska enheter toohello förkonfigurerade lösningen kan du skicka andra telemetri värden som hello externa temperatur, RPM eller vindhastigheten. Sedan kan du visualisera detta ytterligare telemetri för hello instrumentpanelen.

Den här kursen använder en enkel simulerade Node.js-enhet som du kan enkelt ändra tooexperiment med dynamiska telemetri.

toocomplete den här kursen behöver du:

* En aktiv Azure-prenumeration. Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].
* [Node.js] [ lnk-node] version 0.12.x eller senare.

Du kan slutföra den här självstudiekursen på alla operativsystem, till exempel Windows eller Linux, där du kan installera Node.js.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a>Lägga till en telemetri typ.

hello nästa steg är tooreplace hello telemetri som genererats av hello Node.js simulerade enhet med en ny uppsättning värden:

1. Stoppa hello Node.js simulerade enheten genom att skriva **Ctrl + C** i Kommandotolken eller shell.
2. Du kan se hello grunddata värden för hello befintliga temperatur, fuktighet och externa temperatur telemetri i hello remote_monitoring.js-filen. Lägg till ett värde för basdata för **rpm** på följande sätt:

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Hej Node.js simulerade enhet använder hello **generateRandomIncrement** fungera i hello remote_monitoring.js filen tooadd en slumpmässig ökning toohello grunddata värden. Slumpa hello **rpm** värde genom att lägga till en rad med kod efter hello befintliga randomizations på följande sätt:

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Lägg till hello nya rpm värdet toohello JSON-nyttolast hello enheten skickar tooIoT hubb:

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Kör hello Node.js simulerade enhet med hjälp av hello följande kommando:

    `node remote_monitoring.js`

6. Observera hello ny RPM telemetri typ som visas på hello diagram i hello instrumentpanelen:

![Lägga till RPM toohello instrumentpanel][image3]

> [!NOTE]
> Du kanske behöver toodisable och sedan aktivera hello Node.js-enheten på hello **enheter** sida i hello instrumentpanelen toosee hello ändra omedelbart.

## <a name="customize-hello-dashboard-display"></a>Anpassa hello instrumentpanelsvy

Hej **enhetsinformation** meddelandet kan innehålla metadata om hello telemetri hello enheten kan skicka tooIoT hubb. Dessa metadata kan ange hello telemetri typer hello enheten skickar. Ändra hello **deviceMetaData** värdet i hello remote_monitoring.js filen tooinclude en **telemetri** definition följande hello **kommandon** definition. hello följande kodavsnitt visar hello **kommandon** definition (vara säker på att tooadd en `,` efter hello **kommandon** definition):

```nodejs
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [!NOTE]
> hello remote övervakningslösning använder en icke-skiftlägeskänsliga matchningen toocompare hello metadatadefinition med data i hello telemetri dataströmmen.


Lägga till en **telemetri** definition enligt hello föregående kodfragment inte ändra hello beteende hello instrumentpanelen. Dock hello metadata kan även inkludera ett **DisplayName** attributet toocustomize hello visas i hello instrumentpanelen. Uppdatera hello **telemetri** metadatadefinitionen som visas i följande fragment hello:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

hello följande skärmbild visar hur den här ändringen ändrar hello diagramförklaringen på hello instrumentpanelen:

![Anpassa hello diagrammets förklaring][image4]

> [!NOTE]
> Du kanske behöver toodisable och sedan aktivera hello Node.js-enheten på hello **enheter** sida i hello instrumentpanelen toosee hello ändra omedelbart.

## <a name="filter-hello-telemetry-types"></a>Filtrera hello telemetri typer

Som standard visar hello diagram på instrumentpanelen för hello varje dataserie i hello telemetri dataströmmen. Du kan använda hello **enhetsinformation** metadata toosuppress hello visningen av specifika telemetri typer i hello diagram. 

toomake hello diagram visar endast temperatur- och Fuktighetskonsekvens telemetri utelämna **ExternalTemperature** från hello **enhetsinformation** **telemetri** metadata på följande sätt:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

Hej **utomhus temperatur** visas inte längre i hello diagram:

![Filtrera hello telemetri för hello instrumentpanelen][image5]

Den här ändringen påverkar bara hello diagramvyn. Hej **ExternalTemperature** datavärden fortfarande lagras och blir tillgänglig för alla backend-bearbetning.

> [!NOTE]
> Du kanske behöver toodisable och sedan aktivera hello Node.js-enheten på hello **enheter** sida i hello instrumentpanelen toosee hello ändra omedelbart.

## <a name="handle-errors"></a>Hantera fel

För en data-dataströmmen toodisplay av hello diagram, dess **typen** i hello **enhetsinformation** metadata måste matcha hello datatyp hello telemetri värden. Till exempel om hello metadata anger att hello **typen** för fuktighet data är **int** och en **dubbla** hittas i hello telemetri dataströmmen sedan hello fuktighet telemetri har Visa inte i hello schemat. Dock hello **fuktighet** värden fortfarande lagras och blir tillgänglig för alla backend-bearbetning.

## <a name="next-steps"></a>Nästa steg

Nu när du har sett hur toouse dynamiska telemetri, kan du lära dig mer om hur hello förkonfigurerade lösningar använder enhetsinformation: [enheten information metadata i hello fjärrövervaknings förkonfigurerade lösningen] [ lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
