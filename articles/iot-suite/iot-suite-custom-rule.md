---
title: aaaCreate en anpassad regel i Azure IoT Suite | Microsoft Docs
description: "Hur toocreate en anpassad regel i en IoT Suite förkonfigurerade lösningen."
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
ms.openlocfilehash: 6c5bb2ca54f3f17b99ad482e727c8e9fa28d7fe5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-rule-in-hello-remote-monitoring-preconfigured-solution"></a>Skapa en anpassad regel i hello remote förkonfigurerade övervakningslösning

## <a name="introduction"></a>Introduktion

Hello förkonfigurerade lösningar du kan konfigurera [regler som utlöses när en telemetri värdet för en enhet når ett visst tröskelvärde][lnk-builtin-rule]. [Använda dynamisk telemetri med hello remote förkonfigurerade övervakningslösning] [ lnk-dynamic-telemetry] beskriver hur du lägger till anpassad telemetri värden som *ExternalTemperature* tooyour lösning. Den här artikeln visar hur toocreate anpassad regel för dynamiskt telemetri typer i din lösning.

Den här kursen använder en enkel Node.js simulerade enheten toogenerate dynamiska telemetri toosend toohello förkonfigurerade lösningens serverdel. Lägg till anpassade regler sedan i hello **RemoteMonitoring** Visual Studio-lösning och distribuera den här anpassade serverdel tooyour Azure-prenumeration.

toocomplete den här kursen behöver du:

* En aktiv Azure-prenumeration. Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].
* [Node.js] [ lnk-node] version 0.12.x eller senare toocreate en simulerad enhet.
* Visual Studio 2015 eller Visual Studio 2017 toomodify hello förkonfigurerade lösningen avslutas med din nya regler.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

Anteckna hello lösningens namn som du har valt för din distribution. Du behöver den här lösningsnamn senare i den här kursen.

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

Du kan stoppa hello Node.js-konsolprogram när du har verifierat att den skickar **ExternalTemperature** telemetri toohello förkonfigurerade lösningen. Behåll hello konsolfönstret öppna eftersom du kör den här Node.js-konsolprogram igen när du har lagt till hello anpassad regel toohello lösning.

## <a name="rule-storage-locations"></a>Lagringsplatser för regeln

Information om regler sparas på två platser:

* **DeviceRulesNormalizedTable** tabellen – den här tabellen lagras en normaliserade referera toohello regler som definierats av hello lösning portalen. När hello lösning portal visas enheten regler, frågar den här tabellen för hello definitioner.
* **DeviceRules** blob – den här blob lagrar alla hello regler som definierats för alla registrerade enheter och har definierats som en referens inkommande toohello Azure Stream Analytics-jobb.
 
När du uppdaterar en befintlig regel eller definiera en ny regel i hello lösning portal är hello tabell- och blobbdata uppdaterade tooreflect hello ändringar. hello regeln definition visas i hello portal kommer från hello tabell store och hello regeln definition som refereras av hello Stream Analytics-jobb kommer från hello-blob. 

## <a name="update-hello-remotemonitoring-visual-studio-solution"></a>Uppdatera hello RemoteMonitoring Visual Studio-lösning

hello följande steg visar hur hello toomodify RemoteMonitoring Visual Studio-lösning tooinclude en ny regel som använder hello **ExternalTemperature** telemetri som skickas från hello simulerade enheten:

1. Om du inte redan har gjort det klonade hello **azure iot-fjärr-övervakning** lämplig plats på den lokala datorn med hjälp av följande kommando i Git hello tooa i databasen:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. I Visual Studio öppnas hello RemoteMonitoring.sln från den lokala kopian av hello **azure iot-fjärr-övervakning** databasen.

3. Öppna filen hello Infrastructure\Models\DeviceRuleBlobEntity.cs och Lägg till en **ExternalTemperature** egenskapen på följande sätt:

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. Hej samma fil, lägga till i en **ExternalTemperatureRuleOutput** egenskapen på följande sätt:

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. Öppna filen hello Infrastructure\Models\DeviceRuleDataFields.cs och Lägg till följande hello **ExternalTemperature** egenskapen efter hello befintliga **fuktighet** egenskapen:

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. I Hej samma fil, uppdatera hello **_availableDataFields** metoden tooinclude **ExternalTemperature** på följande sätt:

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. Öppna filen hello Infrastructure\Repository\DeviceRulesRepository.cs och ändra hello **BuildBlobEntityListFromTableRows** metoden på följande sätt:

    ```csharp
    else if (rule.DataField == DeviceRuleDataFields.Humidity)
    {
        entity.Humidity = rule.Threshold;
        entity.HumidityRuleOutput = rule.RuleOutput;
    }
    else if (rule.DataField == DeviceRuleDataFields.ExternalTemperature)
    {
      entity.ExternalTemperature = rule.Threshold;
      entity.ExternalTemperatureRuleOutput = rule.RuleOutput;
    }
    ```

## <a name="rebuild-and-redeploy-hello-solution"></a>Återskapa och omdistribuera hello lösning.

Du kan nu distribuera hello uppdateras lösning tooyour Azure-prenumeration.

1. Öppna en upphöjd kommandotolk och navigera toohello roten för den lokala kopian av databasen för hello azure iot-fjärr-övervakning.

2. toodeploy uppdaterade lösningen, kör följande kommando ersätter hello **{distributionsnamnet}** med hello namnet på din förkonfigurerade lösningsdistribution som du antecknade tidigare:

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-hello-stream-analytics-job"></a>Uppdatera hello Stream Analytics-jobbet

När hello distributionen är klar, kan du uppdatera hello Stream Analytics-jobbet toouse hello nya definitioner.

1. Navigera toohello resursgruppen som innehåller din förkonfigurerade lösning resurser i hello Azure-portalen. Den här resursgruppen har samma namn du angav för hello hello lösningen under hello distributionen.

2. Navigera toohello {distributionsnamnet}-regler Stream Analytics-jobbet. 

3. Klicka på **stoppa** toostop hello Stream Analytics-jobbet körs. (Du måste vänta tills hello streaming job toostop innan du kan redigera hello fråga).

4. Klicka på **frågan**. Redigera hello frågan tooinclude hello **Välj** -uttrycket för **ExternalTemperature**. hello följande exempel visar hello fullständig fråga med hello nya **Välj** instruktionen:

    ```
    WITH AlarmsData AS 
    (
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Temperature' as ReadingType,
         Stream.Temperature as Reading,
         Ref.Temperature as Threshold,
         Ref.TemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Humidity' as ReadingType,
         Stream.Humidity as Reading,
         Ref.Humidity as Threshold,
         Ref.HumidityRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'ExternalTemperature' as ReadingType,
         Stream.ExternalTemperature as Reading,
         Ref.ExternalTemperature as Threshold,
         Ref.ExternalTemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.ExternalTemperature IS NOT null AND Stream.ExternalTemperature > Ref.ExternalTemperature
    )
     
    SELECT *
    INTO DeviceRulesMonitoring
    FROM AlarmsData
     
    SELECT *
    INTO DeviceRulesHub
    FROM AlarmsData
    ```

5. Klicka på **spara** toochange hello uppdatera regel för frågan.

6. Klicka på **starta** toostart hello Stream Analytics-jobbet körs igen.

## <a name="add-your-new-rule-in-hello-dashboard"></a>Lägg till din nya regel hello instrumentpanelen

Nu kan du lägga till hello **ExternalTemperature** regeln tooa enhet i hello lösning instrumentpanelen.

1. Navigera toohello lösning portal.

2. Navigera toohello **enheter** panelen.

3. Leta upp hello anpassade du skapade som skickar **ExternalTemperature** telemetri och på hello **enhetsinformation** klickar du på **Lägg till regel**.

4. Välj **ExternalTemperature** i **datafält**.

5. Ange **tröskelvärdet** too56. Klicka på **spara och visa regler**.

6. Returnera toohello instrumentpanelen tooview hello larm historik.

7. Hello konsolfönstret du lämnat öppna börja hello Node.js konsolen app toobegin skicka **ExternalTemperature** telemetridata.

8. Observera att hello **larm historik** tabell visas nya larm när hello ny regel utlöses.
 
## <a name="additional-information"></a>Ytterligare information

Ändra hello operatorn  **>**  är mer komplexa och utöver hello steg som beskrivs i den här kursen. Du kan ändra hello Stream Analytics-jobbet toouse oavsett operator som du vill, avspeglar den operatorn i hello lösning portal är en mer komplicerad uppgift. 

## <a name="next-steps"></a>Nästa steg
Nu när du har sett hur toocreate anpassade regler, du kan lära dig mer om hello förkonfigurerade lösningar:

- [Ansluta Logikapp tooyour Azure IoT Suite Fjärrövervaknings förkonfigurerade lösningen][lnk-logic-app]
- [Enhetens information metadata i hello fjärrövervaknings förkonfigurerade lösningen][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-logic-apps-tutorial.md