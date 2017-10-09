---
title: "aaaCustomizing förkonfigurerade lösningar | Microsoft Docs"
description: "Innehåller råd om hur toocustomize hello Azure IoT Suite förkonfigurerade lösningar."
services: 
suite: iot-suite
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4653ae53-4110-4a10-bd6c-7dc034c293a8
ms.service: iot-suite
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: 1a8573f5ac6ed944c44459df495446f15174d513
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-a-preconfigured-solution"></a>Anpassa en förkonfigurerad lösning

hello förkonfigurerade lösningar som tillhandahålls med hello Azure IoT Suite visar hello tjänster inom hello suite arbetar tillsammans toodeliver en slutpunkt till slutpunkt-lösning. Det finns olika ställen där du kan utöka och anpassa hello lösning för specifika scenarier från den här startpunkten. hello följande avsnitt beskrivs dessa vanliga anpassningspunkter.

## <a name="find-hello-source-code"></a>Hitta hello källkod

hello källkoden för hello förkonfigurerade lösningar finns tillgänglig på GitHub hello följande databaser:

* Fjärråtkomst övervakning: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
* Förutsägande Underhåll: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)
* Anslutna fabriken: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)

hello källkoden för hello förkonfigurerade lösningar tillhandahålls toodemonstrate hello mönster och praxis tooimplement hello slutpunkt till slutpunkt-funktionen på en IoT-lösning med hjälp av Azure IoT Suite har använts. Du hittar mer information om hur toobuild och distribuera hello lösningar i hello GitHub-lagringsplatser.

## <a name="change-hello-preconfigured-rules"></a>Ändra hello fördefinierade regler

hello remote övervakningslösning innehåller tre [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) toohandle enhetsinformation, telemetri och regler logiken i hello lösning.

hello tre strömma analytics-jobb och deras syntax som beskrivs i mer detalj i hello [fjärrövervaknings förkonfigurerade lösningen genomgången](iot-suite-remote-monitoring-sample-walkthrough.md). 

Du kan redigera dessa jobb direkt tooalter hello logik eller Lägg till logik specifika tooyour scenario. Du kan hitta hello Stream Analytics-jobb på följande sätt:

1. Gå för[Azure-portalen](https://portal.azure.com).
2. Navigera toohello resursgrupp med samma namn som IoT-lösningen hello. 
3. Välj hello Azure Stream Analytics-jobbet som toomodify. 
4. Stoppa hello jobbet genom att välja **stoppa** i hello uppsättning kommandon. 
5. Redigera hello indata-, fråge- och utdata.
   
    En enkel ändring är toochange hello fråga efter Hej **regler** jobbet toouse en **”<”** i stället för en **”>”**. hello lösning portal visas fortfarande **”>”** när du redigerar du en regel, men Observera hur hello beteende vändas på grund av toohello ändring i hello underliggande jobb.
6. Starta hello jobbet

> [!NOTE]
> hello instrumentpanelen för övervakning beror på specifika data så att ändringar av hello jobb kan orsaka hello instrumentpanelen toofail.

## <a name="add-your-own-rules"></a>Lägg till dina egna regler

Dessutom toochanging hello förkonfigurerade Azure Stream Analytics-jobb kan du använda hello Azure portal tooadd nya jobb eller lägga till nya frågor tooexisting jobb.

## <a name="customize-devices"></a>Anpassa enheter

En av de vanligaste tillägget aktiviteter för hello fungerar med enheter specifika tooyour scenario. Det finns flera metoder för att arbeta med enheter. De här metoderna omfattar och ändra en simulerad enhet toomatch ditt scenario eller med hjälp av hello [IoT-enhet SDK] [ IoT Device SDK] tooconnect din toohello lösning för fysiska enheter.

För en stegvis guide tooadding enheter finns hello [Iot Suite ansluta enheter](iot-suite-connecting-devices.md) artikeln och hello [remote övervakning C SDK-exempel](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring). Det här exemplet är utformad toowork med hello remote förkonfigurerade övervakningslösning.

### <a name="create-your-own-simulated-device"></a>Skapa simulerade enheten

Ingår i hello [fjärråtkomst övervakning lösning källkoden](https://github.com/Azure/azure-iot-remote-monitoring), är en .NET-simulatorn. Den här simulator är hello en etablerats som en del av hello lösningen och du kan ändra den toosend olika metadata, telemetri och svara toodifferent kommandon och metoder.

hello förkonfigurerade simulator i hello remote förkonfigurerade övervakningslösning simulerar en kyligare enhet som skickar temperatur- och fuktighetskonsekvens telemetri. Du kan ändra hello simulator i hello [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) projektet när du har forked hello GitHub-lagringsplatsen.

### <a name="available-locations-for-simulated-devices"></a>Tillgängliga platser för simulerad enheter

hello standarduppsättning av platser finns i Seattle/Redmond, Washington, USA. Du kan ändra dessa platser i [SampleDeviceFactory.cs][lnk-sample-device-factory].

### <a name="add-a-desired-property-update-handler-toohello-simulator"></a>Lägg till önskade egenskapen update hanteraren toohello simulator

Du kan ange ett värde för en önskad egenskap för en enhet i hello lösning portal. Det är hello ansvar hello toohandle hello egenskapen ändringsbegäran när hello enhet hämtar hello önskad egenskapsvärde. tooadd stöd för en egenskap ändras via en önskad egenskap måste tooadd hanterare toohello simulator.

hello simulator innehåller hanterare för hello **SetPointTemp** och **TelemetryInterval** egenskaper som du kan uppdatera genom att ange önskade värden i hello lösning portal.

hello följande exempel visar hello hanterare för hello **SetPointTemp** önskad egenskap i hello **CoolerDevice** klass:

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

Den här metoden uppdaterar hello telemetri punkt temperatur och rapporter hello ändra tillbaka tooIoT Hub genom att ange en rapporterade egenskap.

Du kan lägga till egna hanterare för egna egenskaper genom följande hello mönster i föregående exempel hello.

Du måste också binda hello önskade egenskapen toohello hanterare som visas i följande exempel från hello hello **CoolerDevice** konstruktorn:

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

Observera att **SetPointTempPropertyName** är en konstant som definierats som ”Config.SetPointTemp”.

### <a name="add-support-for-a-new-method-toohello-simulator"></a>Lägga till stöd för en ny metod toohello simulator

Du kan anpassa hello simulator tooadd stöd för en ny [metod (direkt metod)][lnk-direct-methods]. Det finns två viktiga steg som krävs:

- hello simulator måste meddela hello IoT-hubb i hello förkonfigurerade lösningen med information om hello-metoden.
- hello simulator måste innehålla kod toohandle hello metodanrop när du anropar den från hello **enhetsinformation** panelen i hello solution explorer eller via ett jobb.

Hej fjärrövervaknings förkonfigurerade lösningen använder *rapporterade egenskaper* toosend information om metoder som stöds tooIoT hubb. hello lösningens serverdel upprätthåller en lista över alla hello-metoder som stöds av varje enhet tillsammans med en historik över anrop av metoden. Du kan visa den här informationen om enheter och anropa metoder i hello lösning portal.

toonotify hello IoT-hubb att en enhet har stöd för en metod, hello enheten måste lägga till information om hello metoden toohello **SupportedMethods** nod i hello rapporterade egenskaper:

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

hello Metodsignaturen har hello följande format: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`. Till exempel toospecify hello **InitiateFirmwareUpdate** metoden förväntar sig en strängparameter med namnet **FwPackageURI**, Använd följande Metodsignaturen hello:

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

En lista över stöds parametertyper finns hello **CommandTypes** klass i projektet för hello-infrastruktur.

toodelete en metod, ange hello Metodsignaturen för`null` i hello rapporterade egenskaper.

> [!NOTE]
> hello lösningens serverdel uppdateras bara information om metoder som stöds när den tar emot en *enhetsinformation* meddelande från hello enhet.

Hej följande kodexempel från hello **SampleDeviceFactory** klassen i hello gemensamma projektet visar hur tooadd en lista med toohello av **SupportedMethods** i hello rapporterade egenskaper som skickats av hello enhet:

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' toospecifiy hello URI of hello firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

Det här kodstycket lägger till information om hello **InitiateFirmwareUpdate** metoden inklusive text toodisplay i hello lösning portal och information om hello krävs metodparametrar.

hello simulator skickar rapporterade egenskaper, inklusive hello lista över metoder som stöds, tooIoT hubb när hello simulator startar.

Lägg till en hanterare toohello simulator kod för varje metod stöder. Du kan se hello befintliga hanterare i hello **CoolerDevice** klassen i hello Simulator.WebJob projekt. hello följande exempel visar hello hanterare för **InitiateFirmwareUpdate** metod:

```csharp
public async Task<MethodResponse> OnInitiateFirmwareUpdate(MethodRequest methodRequest, object userContext)
{
    if (_deviceManagementTask != null && !_deviceManagementTask.IsCompleted)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "Device is busy"
        }, 409));
    }

    try
    {
        var operation = new FirmwareUpdate(methodRequest);
        _deviceManagementTask = operation.Run(Transport).ContinueWith(async task =>
        {
            // after firmware completed, we reset telemetry
            var telemetry = _telemetryController as ITelemetryWithTemperatureMeanValue;
            if (telemetry != null)
            {
                telemetry.TemperatureMeanValue = 34.5;
            }

            await UpdateReportedTemperatureMeanValue();
        });

        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "FirmwareUpdate accepted",
            Uri = operation.Uri
        }));
    }
    catch (Exception ex)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = ex.Message
        }, 400));
    }
}
```

Metoden hanterare namn måste börja med `On` följt av namnet på hello hello-metod. Hej **methodRequest** parametern innehåller alla parametrar hello metodanropet från hello lösningens serverdel. hello returnerade värdet måste vara av typen **aktivitet&lt;MethodResponse&gt;**. Hej **BuildMethodResponse** verktyget metoden kan du skapa hello returvärde.

Inuti hello metoden hanterare kan du:

- Starta en asynkron åtgärd.
- Hämta egenskaper från hello *enheten dubbla* i IoT-hubb.
- Uppdatera en enskild rapporterade egenskap med hello **SetReportedPropertyAsync** metod i hello **CoolerDevice** klass.
- Uppdatera flera rapporterade egenskaper genom att skapa en **TwinCollection** -instans och anropa hello **Transport.UpdateReportedPropertiesAsync** metod.

hello utför exemplet för firmware-uppdatering hello följande steg:

- Kontrollerar hello enheten är begäran om uppdatering kan tooaccept hello inbyggd programvara.
- Asynkront initierar hello uppdateringsåtgärden för inbyggd programvara och återställer hello telemetri när hello-åtgärden har slutförts.
- Omedelbart returnerar hello ”FirmwareUpdate accepteras” accepterades meddelande tooindicate hello förfrågan av hello-enheten.

### <a name="build-and-use-your-own-physical-device"></a>Skapa och använda enheten (fysiska)

Hej [Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) ange bibliotek för att ansluta flera typer av enheter (språk och operativsystem) i IoT-lösningar.

## <a name="modify-dashboard-limits"></a>Ändra gränserna för instrumentpanelen

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Antalet enheter som visas i instrumentpanelen listrutan

hello standardinställningen är 200. Du kan ändra det här numret i [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-toodisplay-in-bing-map-control"></a>Antal PIN-koder toodisplay Bing Map-kontrollen

hello standardinställningen är 200. Du kan ändra det här numret i [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Tidsperiod telemetri diagrammet

hello standardvärdet är 10 minuter. Du kan ändra det här värdet i [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-set-up-application-roles"></a>Manuellt konfigurera roller för programmet

hello nedan beskrivs hur tooadd **Admin** och **ReadOnly** programmet roller tooa förkonfigurerade lösningen. Observera att förkonfigurerade lösningar som etablerats från hello azureiotsuite.com plats redan innehåller hello **Admin** och **ReadOnly** roller.

Medlemmar i hello **ReadOnly** rollen kan se hello instrumentpanel och hello enhetslistan, men är inte tillåtna tooadd enheter, ändra attributen eller skicka kommandon.  Medlemmar i hello **Admin** roll har fullständig åtkomst tooall hello funktioner i hello lösning.

1. Gå toohello [klassiska Azure-portalen][lnk-classic-portal].
2. Välj **Active Directory**.
3. Klicka på hello namnet på hello AAD-klient som du använde när du har etablerat din lösning.
4. Klicka på **program**.
5. Klicka på hello namnet på hello-program som matchar din förkonfigurerade lösningsnamn. Om du inte ser ditt program i hello listan, Välj **program som företaget äger** i hello **visa** listrutan och klicka på hello är markerat.
6. Hello längst hello-sidan, klickar du på **hantera Manifest** och sedan **hämta Manifest**.
7. Den här proceduren hämtar en JSON-fil tooyour lokala dator. Öppna filen för redigering i en textredigerare som du väljer.
8. Du kan se på hello tredje raden av hello JSON-fil:

   ```json
   "appRoles" : [],
   ```
   Ersätt den här raden med hello följande kod:

   ```json
   "appRoles": [
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Administrator access toohello application",
   "displayName": "Admin",
   "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
   "isEnabled": true,
   "value": "Admin"
   },
   {
   "allowedMemberTypes": [
   "User"
   ],
   "description": "Read only access toodevice information",
   "displayName": "Read Only",
   "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
   "isEnabled": true,
   "value": "ReadOnly"
   } ],
   ```

9. Spara hello uppdaterade JSON-fil (du kan skriva över befintliga hello-filen).
10. I hello klassiska Azure-portalen längst hello hello sidan Välj **hantera Manifest** sedan **överför Manifest** tooupload hello JSON-fil som du sparade i hello föregående steg.
11. Du har nu lagt till hello **Admin** och **ReadOnly** roller tooyour program.
12. tooassign något av dessa roller tooa användare i din katalog finns [behörigheter på hello azureiotsuite.com platsen][lnk-permissions].

## <a name="feedback"></a>Feedback

Har du en anpassning som beskrivs i det här dokumentet toosee? Lägg till förslag på funktioner för[User Voice](https://feedback.azure.com/forums/321918-azure-iot), eller en kommentar för den här artikeln. 

## <a name="next-steps"></a>Nästa steg

toolearn mer om hello alternativ för att anpassa hello förkonfigurerade lösningar, se:

* [Ansluta Logikapp tooyour Azure IoT Suite Fjärrövervaknings förkonfigurerade lösningen][lnk-logicapp]
* [Använda dynamisk telemetri med hello remote förkonfigurerade övervakningslösning][lnk-dynamic]
* [Enhetens information metadata i hello remote förkonfigurerade övervakningslösning][lnk-devinfo]
* [Anpassa hur hello anslutna fabriksinställningarna lösning visar data från dina OPC UA-servrar][lnk-cf-customize]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[IoT Device SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-cf-customize]: iot-suite-connected-factory-customize.md