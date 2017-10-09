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
# <a name="customize-a-preconfigured-solution"></a><span data-ttu-id="5dca4-103">Anpassa en förkonfigurerad lösning</span><span class="sxs-lookup"><span data-stu-id="5dca4-103">Customize a preconfigured solution</span></span>

<span data-ttu-id="5dca4-104">hello förkonfigurerade lösningar som tillhandahålls med hello Azure IoT Suite visar hello tjänster inom hello suite arbetar tillsammans toodeliver en slutpunkt till slutpunkt-lösning.</span><span class="sxs-lookup"><span data-stu-id="5dca4-104">hello preconfigured solutions provided with hello Azure IoT Suite demonstrate hello services within hello suite working together toodeliver an end-to-end solution.</span></span> <span data-ttu-id="5dca4-105">Det finns olika ställen där du kan utöka och anpassa hello lösning för specifika scenarier från den här startpunkten.</span><span class="sxs-lookup"><span data-stu-id="5dca4-105">From this starting point, there are various places in which you can extend and customize hello solution for specific scenarios.</span></span> <span data-ttu-id="5dca4-106">hello följande avsnitt beskrivs dessa vanliga anpassningspunkter.</span><span class="sxs-lookup"><span data-stu-id="5dca4-106">hello following sections describe these common customization points.</span></span>

## <a name="find-hello-source-code"></a><span data-ttu-id="5dca4-107">Hitta hello källkod</span><span class="sxs-lookup"><span data-stu-id="5dca4-107">Find hello source code</span></span>

<span data-ttu-id="5dca4-108">hello källkoden för hello förkonfigurerade lösningar finns tillgänglig på GitHub hello följande databaser:</span><span class="sxs-lookup"><span data-stu-id="5dca4-108">hello source code for hello preconfigured solutions is available on GitHub in hello following repositories:</span></span>

* <span data-ttu-id="5dca4-109">Fjärråtkomst övervakning: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)</span><span class="sxs-lookup"><span data-stu-id="5dca4-109">Remote Monitoring: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)</span></span>
* <span data-ttu-id="5dca4-110">Förutsägande Underhåll: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)</span><span class="sxs-lookup"><span data-stu-id="5dca4-110">Predictive Maintenance: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)</span></span>
* <span data-ttu-id="5dca4-111">Anslutna fabriken: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)</span><span class="sxs-lookup"><span data-stu-id="5dca4-111">Connected factory: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)</span></span>

<span data-ttu-id="5dca4-112">hello källkoden för hello förkonfigurerade lösningar tillhandahålls toodemonstrate hello mönster och praxis tooimplement hello slutpunkt till slutpunkt-funktionen på en IoT-lösning med hjälp av Azure IoT Suite har använts.</span><span class="sxs-lookup"><span data-stu-id="5dca4-112">hello source code for hello preconfigured solutions is provided toodemonstrate hello patterns and practices used tooimplement hello end-to-end functionality of an IoT solution using Azure IoT Suite.</span></span> <span data-ttu-id="5dca4-113">Du hittar mer information om hur toobuild och distribuera hello lösningar i hello GitHub-lagringsplatser.</span><span class="sxs-lookup"><span data-stu-id="5dca4-113">You can find more information about how toobuild and deploy hello solutions in hello GitHub repositories.</span></span>

## <a name="change-hello-preconfigured-rules"></a><span data-ttu-id="5dca4-114">Ändra hello fördefinierade regler</span><span class="sxs-lookup"><span data-stu-id="5dca4-114">Change hello preconfigured rules</span></span>

<span data-ttu-id="5dca4-115">hello remote övervakningslösning innehåller tre [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) toohandle enhetsinformation, telemetri och regler logiken i hello lösning.</span><span class="sxs-lookup"><span data-stu-id="5dca4-115">hello remote monitoring solution includes three [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) jobs toohandle device information, telemetry, and rules logic in hello solution.</span></span>

<span data-ttu-id="5dca4-116">hello tre strömma analytics-jobb och deras syntax som beskrivs i mer detalj i hello [fjärrövervaknings förkonfigurerade lösningen genomgången](iot-suite-remote-monitoring-sample-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="5dca4-116">hello three stream analytics jobs and their syntax are described in depth in hello [Remote monitoring preconfigured solution walkthrough](iot-suite-remote-monitoring-sample-walkthrough.md).</span></span> 

<span data-ttu-id="5dca4-117">Du kan redigera dessa jobb direkt tooalter hello logik eller Lägg till logik specifika tooyour scenario.</span><span class="sxs-lookup"><span data-stu-id="5dca4-117">You can edit these jobs directly tooalter hello logic, or add logic specific tooyour scenario.</span></span> <span data-ttu-id="5dca4-118">Du kan hitta hello Stream Analytics-jobb på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5dca4-118">You can find hello Stream Analytics jobs as follows:</span></span>

1. <span data-ttu-id="5dca4-119">Gå för[Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5dca4-119">Go too[Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="5dca4-120">Navigera toohello resursgrupp med samma namn som IoT-lösningen hello.</span><span class="sxs-lookup"><span data-stu-id="5dca4-120">Navigate toohello resource group with hello same name as your IoT solution.</span></span> 
3. <span data-ttu-id="5dca4-121">Välj hello Azure Stream Analytics-jobbet som toomodify.</span><span class="sxs-lookup"><span data-stu-id="5dca4-121">Select hello Azure Stream Analytics job you'd like toomodify.</span></span> 
4. <span data-ttu-id="5dca4-122">Stoppa hello jobbet genom att välja **stoppa** i hello uppsättning kommandon.</span><span class="sxs-lookup"><span data-stu-id="5dca4-122">Stop hello job by selecting **Stop** in hello set of commands.</span></span> 
5. <span data-ttu-id="5dca4-123">Redigera hello indata-, fråge- och utdata.</span><span class="sxs-lookup"><span data-stu-id="5dca4-123">Edit hello inputs, query, and outputs.</span></span>
   
    <span data-ttu-id="5dca4-124">En enkel ändring är toochange hello fråga efter Hej **regler** jobbet toouse en **”<”** i stället för en **”>”**.</span><span class="sxs-lookup"><span data-stu-id="5dca4-124">A simple modification is toochange hello query for hello **Rules** job toouse a **"<"** instead of a **">"**.</span></span> <span data-ttu-id="5dca4-125">hello lösning portal visas fortfarande **”>”** när du redigerar du en regel, men Observera hur hello beteende vändas på grund av toohello ändring i hello underliggande jobb.</span><span class="sxs-lookup"><span data-stu-id="5dca4-125">hello solution portal still shows **">"** when you edit a rule, but notice how hello behavior is flipped due toohello change in hello underlying job.</span></span>
6. <span data-ttu-id="5dca4-126">Starta hello jobbet</span><span class="sxs-lookup"><span data-stu-id="5dca4-126">Start hello job</span></span>

> [!NOTE]
> <span data-ttu-id="5dca4-127">hello instrumentpanelen för övervakning beror på specifika data så att ändringar av hello jobb kan orsaka hello instrumentpanelen toofail.</span><span class="sxs-lookup"><span data-stu-id="5dca4-127">hello remote monitoring dashboard depends on specific data, so altering hello jobs can cause hello dashboard toofail.</span></span>

## <a name="add-your-own-rules"></a><span data-ttu-id="5dca4-128">Lägg till dina egna regler</span><span class="sxs-lookup"><span data-stu-id="5dca4-128">Add your own rules</span></span>

<span data-ttu-id="5dca4-129">Dessutom toochanging hello förkonfigurerade Azure Stream Analytics-jobb kan du använda hello Azure portal tooadd nya jobb eller lägga till nya frågor tooexisting jobb.</span><span class="sxs-lookup"><span data-stu-id="5dca4-129">In addition toochanging hello preconfigured Azure Stream Analytics jobs, you can use hello Azure portal tooadd new jobs or add new queries tooexisting jobs.</span></span>

## <a name="customize-devices"></a><span data-ttu-id="5dca4-130">Anpassa enheter</span><span class="sxs-lookup"><span data-stu-id="5dca4-130">Customize devices</span></span>

<span data-ttu-id="5dca4-131">En av de vanligaste tillägget aktiviteter för hello fungerar med enheter specifika tooyour scenario.</span><span class="sxs-lookup"><span data-stu-id="5dca4-131">One of hello most common extension activities is working with devices specific tooyour scenario.</span></span> <span data-ttu-id="5dca4-132">Det finns flera metoder för att arbeta med enheter.</span><span class="sxs-lookup"><span data-stu-id="5dca4-132">There are several methods for working with devices.</span></span> <span data-ttu-id="5dca4-133">De här metoderna omfattar och ändra en simulerad enhet toomatch ditt scenario eller med hjälp av hello [IoT-enhet SDK] [ IoT Device SDK] tooconnect din toohello lösning för fysiska enheter.</span><span class="sxs-lookup"><span data-stu-id="5dca4-133">These methods include altering a simulated device toomatch your scenario, or using hello [IoT Device SDK][IoT Device SDK] tooconnect your physical device toohello solution.</span></span>

<span data-ttu-id="5dca4-134">För en stegvis guide tooadding enheter finns hello [Iot Suite ansluta enheter](iot-suite-connecting-devices.md) artikeln och hello [remote övervakning C SDK-exempel](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring).</span><span class="sxs-lookup"><span data-stu-id="5dca4-134">For a step-by-step guide tooadding devices, see hello [Iot Suite Connecting Devices](iot-suite-connecting-devices.md) article and hello [remote monitoring C SDK Sample](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring).</span></span> <span data-ttu-id="5dca4-135">Det här exemplet är utformad toowork med hello remote förkonfigurerade övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="5dca4-135">This sample is designed toowork with hello remote monitoring preconfigured solution.</span></span>

### <a name="create-your-own-simulated-device"></a><span data-ttu-id="5dca4-136">Skapa simulerade enheten</span><span class="sxs-lookup"><span data-stu-id="5dca4-136">Create your own simulated device</span></span>

<span data-ttu-id="5dca4-137">Ingår i hello [fjärråtkomst övervakning lösning källkoden](https://github.com/Azure/azure-iot-remote-monitoring), är en .NET-simulatorn.</span><span class="sxs-lookup"><span data-stu-id="5dca4-137">Included in hello [remote monitoring solution source code](https://github.com/Azure/azure-iot-remote-monitoring), is a .NET simulator.</span></span> <span data-ttu-id="5dca4-138">Den här simulator är hello en etablerats som en del av hello lösningen och du kan ändra den toosend olika metadata, telemetri och svara toodifferent kommandon och metoder.</span><span class="sxs-lookup"><span data-stu-id="5dca4-138">This simulator is hello one provisioned as part of hello solution and you can alter it toosend different metadata, telemetry, and respond toodifferent commands and methods.</span></span>

<span data-ttu-id="5dca4-139">hello förkonfigurerade simulator i hello remote förkonfigurerade övervakningslösning simulerar en kyligare enhet som skickar temperatur- och fuktighetskonsekvens telemetri.</span><span class="sxs-lookup"><span data-stu-id="5dca4-139">hello preconfigured simulator in hello remote monitoring preconfigured solution simulates a cooler device that emits temperature and humidity telemetry.</span></span> <span data-ttu-id="5dca4-140">Du kan ändra hello simulator i hello [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) projektet när du har forked hello GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="5dca4-140">You can modify hello simulator in hello [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) project when you've forked hello GitHub repository.</span></span>

### <a name="available-locations-for-simulated-devices"></a><span data-ttu-id="5dca4-141">Tillgängliga platser för simulerad enheter</span><span class="sxs-lookup"><span data-stu-id="5dca4-141">Available locations for simulated devices</span></span>

<span data-ttu-id="5dca4-142">hello standarduppsättning av platser finns i Seattle/Redmond, Washington, USA.</span><span class="sxs-lookup"><span data-stu-id="5dca4-142">hello default set of locations is in Seattle/Redmond, Washington, United States of America.</span></span> <span data-ttu-id="5dca4-143">Du kan ändra dessa platser i [SampleDeviceFactory.cs][lnk-sample-device-factory].</span><span class="sxs-lookup"><span data-stu-id="5dca4-143">You can change these locations in [SampleDeviceFactory.cs][lnk-sample-device-factory].</span></span>

### <a name="add-a-desired-property-update-handler-toohello-simulator"></a><span data-ttu-id="5dca4-144">Lägg till önskade egenskapen update hanteraren toohello simulator</span><span class="sxs-lookup"><span data-stu-id="5dca4-144">Add a desired property update handler toohello simulator</span></span>

<span data-ttu-id="5dca4-145">Du kan ange ett värde för en önskad egenskap för en enhet i hello lösning portal.</span><span class="sxs-lookup"><span data-stu-id="5dca4-145">You can set a value for a desired property for a device in hello solution portal.</span></span> <span data-ttu-id="5dca4-146">Det är hello ansvar hello toohandle hello egenskapen ändringsbegäran när hello enhet hämtar hello önskad egenskapsvärde.</span><span class="sxs-lookup"><span data-stu-id="5dca4-146">It is hello responsibility of hello device toohandle hello property change request when hello device retrieves hello desired property value.</span></span> <span data-ttu-id="5dca4-147">tooadd stöd för en egenskap ändras via en önskad egenskap måste tooadd hanterare toohello simulator.</span><span class="sxs-lookup"><span data-stu-id="5dca4-147">tooadd support for a property value change through a desired property, you need tooadd a handler toohello simulator.</span></span>

<span data-ttu-id="5dca4-148">hello simulator innehåller hanterare för hello **SetPointTemp** och **TelemetryInterval** egenskaper som du kan uppdatera genom att ange önskade värden i hello lösning portal.</span><span class="sxs-lookup"><span data-stu-id="5dca4-148">hello simulator contains handlers for hello **SetPointTemp** and **TelemetryInterval** properties that you can update by setting desired values in hello solution portal.</span></span>

<span data-ttu-id="5dca4-149">hello följande exempel visar hello hanterare för hello **SetPointTemp** önskad egenskap i hello **CoolerDevice** klass:</span><span class="sxs-lookup"><span data-stu-id="5dca4-149">hello following example shows hello handler for hello **SetPointTemp** desired property in hello **CoolerDevice** class:</span></span>

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

<span data-ttu-id="5dca4-150">Den här metoden uppdaterar hello telemetri punkt temperatur och rapporter hello ändra tillbaka tooIoT Hub genom att ange en rapporterade egenskap.</span><span class="sxs-lookup"><span data-stu-id="5dca4-150">This method updates hello telemetry point temperature and then reports hello change back tooIoT Hub by setting a reported property.</span></span>

<span data-ttu-id="5dca4-151">Du kan lägga till egna hanterare för egna egenskaper genom följande hello mönster i föregående exempel hello.</span><span class="sxs-lookup"><span data-stu-id="5dca4-151">You can add your own handlers for your own properties by following hello pattern in hello preceding example.</span></span>

<span data-ttu-id="5dca4-152">Du måste också binda hello önskade egenskapen toohello hanterare som visas i följande exempel från hello hello **CoolerDevice** konstruktorn:</span><span class="sxs-lookup"><span data-stu-id="5dca4-152">You must also bind hello desired property toohello handler as shown in hello following example from hello **CoolerDevice** constructor:</span></span>

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

<span data-ttu-id="5dca4-153">Observera att **SetPointTempPropertyName** är en konstant som definierats som ”Config.SetPointTemp”.</span><span class="sxs-lookup"><span data-stu-id="5dca4-153">Note that **SetPointTempPropertyName** is a constant defined as "Config.SetPointTemp".</span></span>

### <a name="add-support-for-a-new-method-toohello-simulator"></a><span data-ttu-id="5dca4-154">Lägga till stöd för en ny metod toohello simulator</span><span class="sxs-lookup"><span data-stu-id="5dca4-154">Add support for a new method toohello simulator</span></span>

<span data-ttu-id="5dca4-155">Du kan anpassa hello simulator tooadd stöd för en ny [metod (direkt metod)][lnk-direct-methods].</span><span class="sxs-lookup"><span data-stu-id="5dca4-155">You can customize hello simulator tooadd support for a new [method (direct method)][lnk-direct-methods].</span></span> <span data-ttu-id="5dca4-156">Det finns två viktiga steg som krävs:</span><span class="sxs-lookup"><span data-stu-id="5dca4-156">There are two key steps required:</span></span>

- <span data-ttu-id="5dca4-157">hello simulator måste meddela hello IoT-hubb i hello förkonfigurerade lösningen med information om hello-metoden.</span><span class="sxs-lookup"><span data-stu-id="5dca4-157">hello simulator must notify hello IoT hub in hello preconfigured solution with details of hello method.</span></span>
- <span data-ttu-id="5dca4-158">hello simulator måste innehålla kod toohandle hello metodanrop när du anropar den från hello **enhetsinformation** panelen i hello solution explorer eller via ett jobb.</span><span class="sxs-lookup"><span data-stu-id="5dca4-158">hello simulator must include code toohandle hello method call when you invoke it from hello **Device details** panel in hello solution explorer or through a job.</span></span>

<span data-ttu-id="5dca4-159">Hej fjärrövervaknings förkonfigurerade lösningen använder *rapporterade egenskaper* toosend information om metoder som stöds tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="5dca4-159">hello remote monitoring preconfigured solution uses *reported properties* toosend details of supported methods tooIoT hub.</span></span> <span data-ttu-id="5dca4-160">hello lösningens serverdel upprätthåller en lista över alla hello-metoder som stöds av varje enhet tillsammans med en historik över anrop av metoden.</span><span class="sxs-lookup"><span data-stu-id="5dca4-160">hello solution back end maintains a list of all hello methods supported by each device along with a history of method invocations.</span></span> <span data-ttu-id="5dca4-161">Du kan visa den här informationen om enheter och anropa metoder i hello lösning portal.</span><span class="sxs-lookup"><span data-stu-id="5dca4-161">You can view this information about devices and invoke methods in hello solution portal.</span></span>

<span data-ttu-id="5dca4-162">toonotify hello IoT-hubb att en enhet har stöd för en metod, hello enheten måste lägga till information om hello metoden toohello **SupportedMethods** nod i hello rapporterade egenskaper:</span><span class="sxs-lookup"><span data-stu-id="5dca4-162">toonotify hello IoT hub that a device supports a method, hello device must add details of hello method toohello **SupportedMethods** node in hello reported properties:</span></span>

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

<span data-ttu-id="5dca4-163">hello Metodsignaturen har hello följande format: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`.</span><span class="sxs-lookup"><span data-stu-id="5dca4-163">hello method signature has hello following format: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`.</span></span> <span data-ttu-id="5dca4-164">Till exempel toospecify hello **InitiateFirmwareUpdate** metoden förväntar sig en strängparameter med namnet **FwPackageURI**, Använd följande Metodsignaturen hello:</span><span class="sxs-lookup"><span data-stu-id="5dca4-164">For example, toospecify hello **InitiateFirmwareUpdate** method expects a string parameter named **FwPackageURI**, use hello following method signature:</span></span>

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

<span data-ttu-id="5dca4-165">En lista över stöds parametertyper finns hello **CommandTypes** klass i projektet för hello-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="5dca4-165">For a list of supported parameter types, see hello **CommandTypes** class in hello Infrastructure project.</span></span>

<span data-ttu-id="5dca4-166">toodelete en metod, ange hello Metodsignaturen för`null` i hello rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="5dca4-166">toodelete a method, set hello method signature too`null` in hello reported properties.</span></span>

> [!NOTE]
> <span data-ttu-id="5dca4-167">hello lösningens serverdel uppdateras bara information om metoder som stöds när den tar emot en *enhetsinformation* meddelande från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="5dca4-167">hello solution back end only updates information about supported methods when it receives a *device information* message from hello device.</span></span>

<span data-ttu-id="5dca4-168">Hej följande kodexempel från hello **SampleDeviceFactory** klassen i hello gemensamma projektet visar hur tooadd en lista med toohello av **SupportedMethods** i hello rapporterade egenskaper som skickats av hello enhet:</span><span class="sxs-lookup"><span data-stu-id="5dca4-168">hello following code sample from hello **SampleDeviceFactory** class in hello Common project shows how tooadd a method toohello list of **SupportedMethods** in hello reported properties sent by hello device:</span></span>

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' toospecifiy hello URI of hello firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

<span data-ttu-id="5dca4-169">Det här kodstycket lägger till information om hello **InitiateFirmwareUpdate** metoden inklusive text toodisplay i hello lösning portal och information om hello krävs metodparametrar.</span><span class="sxs-lookup"><span data-stu-id="5dca4-169">This code snippet adds details of hello **InitiateFirmwareUpdate** method including text toodisplay in hello solution portal and details of hello required method parameters.</span></span>

<span data-ttu-id="5dca4-170">hello simulator skickar rapporterade egenskaper, inklusive hello lista över metoder som stöds, tooIoT hubb när hello simulator startar.</span><span class="sxs-lookup"><span data-stu-id="5dca4-170">hello simulator sends reported properties, including hello list of supported methods, tooIoT Hub when hello simulator starts.</span></span>

<span data-ttu-id="5dca4-171">Lägg till en hanterare toohello simulator kod för varje metod stöder.</span><span class="sxs-lookup"><span data-stu-id="5dca4-171">Add a handler toohello simulator code for each method it supports.</span></span> <span data-ttu-id="5dca4-172">Du kan se hello befintliga hanterare i hello **CoolerDevice** klassen i hello Simulator.WebJob projekt.</span><span class="sxs-lookup"><span data-stu-id="5dca4-172">You can see hello existing handlers in hello **CoolerDevice** class in hello Simulator.WebJob project.</span></span> <span data-ttu-id="5dca4-173">hello följande exempel visar hello hanterare för **InitiateFirmwareUpdate** metod:</span><span class="sxs-lookup"><span data-stu-id="5dca4-173">hello following example shows hello handler for **InitiateFirmwareUpdate** method:</span></span>

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

<span data-ttu-id="5dca4-174">Metoden hanterare namn måste börja med `On` följt av namnet på hello hello-metod.</span><span class="sxs-lookup"><span data-stu-id="5dca4-174">Method handler names must start with `On` followed by hello name of hello method.</span></span> <span data-ttu-id="5dca4-175">Hej **methodRequest** parametern innehåller alla parametrar hello metodanropet från hello lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="5dca4-175">hello **methodRequest** parameter contains any parameters passed with hello method invocation from hello solution back end.</span></span> <span data-ttu-id="5dca4-176">hello returnerade värdet måste vara av typen **aktivitet&lt;MethodResponse&gt;**.</span><span class="sxs-lookup"><span data-stu-id="5dca4-176">hello return value must be of type **Task&lt;MethodResponse&gt;**.</span></span> <span data-ttu-id="5dca4-177">Hej **BuildMethodResponse** verktyget metoden kan du skapa hello returvärde.</span><span class="sxs-lookup"><span data-stu-id="5dca4-177">hello **BuildMethodResponse** utility method helps you create hello return value.</span></span>

<span data-ttu-id="5dca4-178">Inuti hello metoden hanterare kan du:</span><span class="sxs-lookup"><span data-stu-id="5dca4-178">Inside hello method handler, you could:</span></span>

- <span data-ttu-id="5dca4-179">Starta en asynkron åtgärd.</span><span class="sxs-lookup"><span data-stu-id="5dca4-179">Start an asynchronous task.</span></span>
- <span data-ttu-id="5dca4-180">Hämta egenskaper från hello *enheten dubbla* i IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5dca4-180">Retrieve desired properties from hello *device twin* in IoT Hub.</span></span>
- <span data-ttu-id="5dca4-181">Uppdatera en enskild rapporterade egenskap med hello **SetReportedPropertyAsync** metod i hello **CoolerDevice** klass.</span><span class="sxs-lookup"><span data-stu-id="5dca4-181">Update a single reported property using hello **SetReportedPropertyAsync** method in hello **CoolerDevice** class.</span></span>
- <span data-ttu-id="5dca4-182">Uppdatera flera rapporterade egenskaper genom att skapa en **TwinCollection** -instans och anropa hello **Transport.UpdateReportedPropertiesAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="5dca4-182">Update multiple reported properties by creating a **TwinCollection** instance and calling hello **Transport.UpdateReportedPropertiesAsync** method.</span></span>

<span data-ttu-id="5dca4-183">hello utför exemplet för firmware-uppdatering hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5dca4-183">hello preceding firmware update example performs hello following steps:</span></span>

- <span data-ttu-id="5dca4-184">Kontrollerar hello enheten är begäran om uppdatering kan tooaccept hello inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="5dca4-184">Checks hello device is able tooaccept hello firmware update request.</span></span>
- <span data-ttu-id="5dca4-185">Asynkront initierar hello uppdateringsåtgärden för inbyggd programvara och återställer hello telemetri när hello-åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="5dca4-185">Asynchronously initiates hello firmware update operation and resets hello telemetry when hello operation is complete.</span></span>
- <span data-ttu-id="5dca4-186">Omedelbart returnerar hello ”FirmwareUpdate accepteras” accepterades meddelande tooindicate hello förfrågan av hello-enheten.</span><span class="sxs-lookup"><span data-stu-id="5dca4-186">Immediately returns hello "FirmwareUpdate accepted" message tooindicate hello request was accepted by hello device.</span></span>

### <a name="build-and-use-your-own-physical-device"></a><span data-ttu-id="5dca4-187">Skapa och använda enheten (fysiska)</span><span class="sxs-lookup"><span data-stu-id="5dca4-187">Build and use your own (physical) device</span></span>

<span data-ttu-id="5dca4-188">Hej [Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) ange bibliotek för att ansluta flera typer av enheter (språk och operativsystem) i IoT-lösningar.</span><span class="sxs-lookup"><span data-stu-id="5dca4-188">hello [Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) provide libraries for connecting numerous device types (languages and operating systems) into IoT solutions.</span></span>

## <a name="modify-dashboard-limits"></a><span data-ttu-id="5dca4-189">Ändra gränserna för instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="5dca4-189">Modify dashboard limits</span></span>

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a><span data-ttu-id="5dca4-190">Antalet enheter som visas i instrumentpanelen listrutan</span><span class="sxs-lookup"><span data-stu-id="5dca4-190">Number of devices displayed in dashboard dropdown</span></span>

<span data-ttu-id="5dca4-191">hello standardinställningen är 200.</span><span class="sxs-lookup"><span data-stu-id="5dca4-191">hello default is 200.</span></span> <span data-ttu-id="5dca4-192">Du kan ändra det här numret i [DashboardController.cs][lnk-dashboard-controller].</span><span class="sxs-lookup"><span data-stu-id="5dca4-192">You can change this number in [DashboardController.cs][lnk-dashboard-controller].</span></span>

### <a name="number-of-pins-toodisplay-in-bing-map-control"></a><span data-ttu-id="5dca4-193">Antal PIN-koder toodisplay Bing Map-kontrollen</span><span class="sxs-lookup"><span data-stu-id="5dca4-193">Number of pins toodisplay in Bing Map control</span></span>

<span data-ttu-id="5dca4-194">hello standardinställningen är 200.</span><span class="sxs-lookup"><span data-stu-id="5dca4-194">hello default is 200.</span></span> <span data-ttu-id="5dca4-195">Du kan ändra det här numret i [TelemetryApiController.cs][lnk-telemetry-api-controller-01].</span><span class="sxs-lookup"><span data-stu-id="5dca4-195">You can change this number in [TelemetryApiController.cs][lnk-telemetry-api-controller-01].</span></span>

### <a name="time-period-of-telemetry-graph"></a><span data-ttu-id="5dca4-196">Tidsperiod telemetri diagrammet</span><span class="sxs-lookup"><span data-stu-id="5dca4-196">Time period of telemetry graph</span></span>

<span data-ttu-id="5dca4-197">hello standardvärdet är 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="5dca4-197">hello default is 10 minutes.</span></span> <span data-ttu-id="5dca4-198">Du kan ändra det här värdet i [TelmetryApiController.cs][lnk-telemetry-api-controller-02].</span><span class="sxs-lookup"><span data-stu-id="5dca4-198">You can change this value in [TelmetryApiController.cs][lnk-telemetry-api-controller-02].</span></span>

## <a name="manually-set-up-application-roles"></a><span data-ttu-id="5dca4-199">Manuellt konfigurera roller för programmet</span><span class="sxs-lookup"><span data-stu-id="5dca4-199">Manually set up application roles</span></span>

<span data-ttu-id="5dca4-200">hello nedan beskrivs hur tooadd **Admin** och **ReadOnly** programmet roller tooa förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="5dca4-200">hello following procedure describes how tooadd **Admin** and **ReadOnly** application roles tooa preconfigured solution.</span></span> <span data-ttu-id="5dca4-201">Observera att förkonfigurerade lösningar som etablerats från hello azureiotsuite.com plats redan innehåller hello **Admin** och **ReadOnly** roller.</span><span class="sxs-lookup"><span data-stu-id="5dca4-201">Note that preconfigured solutions provisioned from hello azureiotsuite.com site already include hello **Admin** and **ReadOnly** roles.</span></span>

<span data-ttu-id="5dca4-202">Medlemmar i hello **ReadOnly** rollen kan se hello instrumentpanel och hello enhetslistan, men är inte tillåtna tooadd enheter, ändra attributen eller skicka kommandon.</span><span class="sxs-lookup"><span data-stu-id="5dca4-202">Members of hello **ReadOnly** role can see hello dashboard and hello device list, but are not allowed tooadd devices, change device attributes, or send commands.</span></span>  <span data-ttu-id="5dca4-203">Medlemmar i hello **Admin** roll har fullständig åtkomst tooall hello funktioner i hello lösning.</span><span class="sxs-lookup"><span data-stu-id="5dca4-203">Members of hello **Admin** role have full access tooall hello functionality in hello solution.</span></span>

1. <span data-ttu-id="5dca4-204">Gå toohello [klassiska Azure-portalen][lnk-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="5dca4-204">Go toohello [Azure classic portal][lnk-classic-portal].</span></span>
2. <span data-ttu-id="5dca4-205">Välj **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5dca4-205">Select **Active Directory**.</span></span>
3. <span data-ttu-id="5dca4-206">Klicka på hello namnet på hello AAD-klient som du använde när du har etablerat din lösning.</span><span class="sxs-lookup"><span data-stu-id="5dca4-206">Click hello name of hello AAD tenant you used when you provisioned your solution.</span></span>
4. <span data-ttu-id="5dca4-207">Klicka på **program**.</span><span class="sxs-lookup"><span data-stu-id="5dca4-207">Click **Applications**.</span></span>
5. <span data-ttu-id="5dca4-208">Klicka på hello namnet på hello-program som matchar din förkonfigurerade lösningsnamn.</span><span class="sxs-lookup"><span data-stu-id="5dca4-208">Click hello name of hello application that matches your preconfigured solution name.</span></span> <span data-ttu-id="5dca4-209">Om du inte ser ditt program i hello listan, Välj **program som företaget äger** i hello **visa** listrutan och klicka på hello är markerat.</span><span class="sxs-lookup"><span data-stu-id="5dca4-209">If you don't see your application in hello list, select **Applications my company owns** in hello **Show** dropdown and click hello check mark.</span></span>
6. <span data-ttu-id="5dca4-210">Hello längst hello-sidan, klickar du på **hantera Manifest** och sedan **hämta Manifest**.</span><span class="sxs-lookup"><span data-stu-id="5dca4-210">At hello bottom of hello page, click **Manage Manifest** and then **Download Manifest**.</span></span>
7. <span data-ttu-id="5dca4-211">Den här proceduren hämtar en JSON-fil tooyour lokala dator.</span><span class="sxs-lookup"><span data-stu-id="5dca4-211">This procedure downloads a .json file tooyour local machine.</span></span> <span data-ttu-id="5dca4-212">Öppna filen för redigering i en textredigerare som du väljer.</span><span class="sxs-lookup"><span data-stu-id="5dca4-212">Open this file for editing in a text editor of your choice.</span></span>
8. <span data-ttu-id="5dca4-213">Du kan se på hello tredje raden av hello JSON-fil:</span><span class="sxs-lookup"><span data-stu-id="5dca4-213">On hello third line of hello .json file, you can see:</span></span>

   ```json
   "appRoles" : [],
   ```
   <span data-ttu-id="5dca4-214">Ersätt den här raden med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="5dca4-214">Replace this line with hello following code:</span></span>

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

9. <span data-ttu-id="5dca4-215">Spara hello uppdaterade JSON-fil (du kan skriva över befintliga hello-filen).</span><span class="sxs-lookup"><span data-stu-id="5dca4-215">Save hello updated .json file (you can overwrite hello existing file).</span></span>
10. <span data-ttu-id="5dca4-216">I hello klassiska Azure-portalen längst hello hello sidan Välj **hantera Manifest** sedan **överför Manifest** tooupload hello JSON-fil som du sparade i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="5dca4-216">In hello Azure classic portal, at hello bottom of hello page, select **Manage Manifest** then **Upload Manifest** tooupload hello .json file you saved in hello previous step.</span></span>
11. <span data-ttu-id="5dca4-217">Du har nu lagt till hello **Admin** och **ReadOnly** roller tooyour program.</span><span class="sxs-lookup"><span data-stu-id="5dca4-217">You have now added hello **Admin** and **ReadOnly** roles tooyour application.</span></span>
12. <span data-ttu-id="5dca4-218">tooassign något av dessa roller tooa användare i din katalog finns [behörigheter på hello azureiotsuite.com platsen][lnk-permissions].</span><span class="sxs-lookup"><span data-stu-id="5dca4-218">tooassign one of these roles tooa user in your directory, see [Permissions on hello azureiotsuite.com site][lnk-permissions].</span></span>

## <a name="feedback"></a><span data-ttu-id="5dca4-219">Feedback</span><span class="sxs-lookup"><span data-stu-id="5dca4-219">Feedback</span></span>

<span data-ttu-id="5dca4-220">Har du en anpassning som beskrivs i det här dokumentet toosee?</span><span class="sxs-lookup"><span data-stu-id="5dca4-220">Do you have a customization you'd like toosee covered in this document?</span></span> <span data-ttu-id="5dca4-221">Lägg till förslag på funktioner för[User Voice](https://feedback.azure.com/forums/321918-azure-iot), eller en kommentar för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5dca4-221">Add feature suggestions too[User Voice](https://feedback.azure.com/forums/321918-azure-iot), or comment on this article.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="5dca4-222">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5dca4-222">Next steps</span></span>

<span data-ttu-id="5dca4-223">toolearn mer om hello alternativ för att anpassa hello förkonfigurerade lösningar, se:</span><span class="sxs-lookup"><span data-stu-id="5dca4-223">toolearn more about hello options for customizing hello preconfigured solutions, see:</span></span>

* <span data-ttu-id="5dca4-224">[Ansluta Logikapp tooyour Azure IoT Suite Fjärrövervaknings förkonfigurerade lösningen][lnk-logicapp]</span><span class="sxs-lookup"><span data-stu-id="5dca4-224">[Connect Logic App tooyour Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logicapp]</span></span>
* <span data-ttu-id="5dca4-225">[Använda dynamisk telemetri med hello remote förkonfigurerade övervakningslösning][lnk-dynamic]</span><span class="sxs-lookup"><span data-stu-id="5dca4-225">[Use dynamic telemetry with hello remote monitoring preconfigured solution][lnk-dynamic]</span></span>
* <span data-ttu-id="5dca4-226">[Enhetens information metadata i hello remote förkonfigurerade övervakningslösning][lnk-devinfo]</span><span class="sxs-lookup"><span data-stu-id="5dca4-226">[Device information metadata in hello remote monitoring preconfigured solution][lnk-devinfo]</span></span>
* <span data-ttu-id="5dca4-227">[Anpassa hur hello anslutna fabriksinställningarna lösning visar data från dina OPC UA-servrar][lnk-cf-customize]</span><span class="sxs-lookup"><span data-stu-id="5dca4-227">[Customize how hello connected factory solution displays data from your OPC UA servers][lnk-cf-customize]</span></span>

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