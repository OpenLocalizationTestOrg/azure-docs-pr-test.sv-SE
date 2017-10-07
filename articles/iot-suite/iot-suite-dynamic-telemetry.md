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
# <a name="use-dynamic-telemetry-with-hello-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="a500c-103">Använda dynamisk telemetri med hello remote förkonfigurerade övervakningslösning</span><span class="sxs-lookup"><span data-stu-id="a500c-103">Use dynamic telemetry with hello remote monitoring preconfigured solution</span></span>

<span data-ttu-id="a500c-104">Dynamisk telemetri gör du toovisualize alla telemetri skickas toohello remote förkonfigurerade övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="a500c-104">Dynamic telemetry enables you toovisualize any telemetry sent toohello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="a500c-105">hello simulerade enheter som distribueras med hello förkonfigurerade lösningen skickar temperatur- och fuktighetskonsekvens telemetri som du visualisera på hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="a500c-105">hello simulated devices that deploy with hello preconfigured solution send temperature and humidity telemetry, which you can visualize on hello dashboard.</span></span> <span data-ttu-id="a500c-106">Om du anpassar befintliga simulerade enheter, skapa nya simulerade enheter eller ansluta fysiska enheter toohello förkonfigurerade lösningen kan du skicka andra telemetri värden som hello externa temperatur, RPM eller vindhastigheten.</span><span class="sxs-lookup"><span data-stu-id="a500c-106">If you customize existing simulated devices, create new simulated devices, or connect physical devices toohello preconfigured solution you can send other telemetry values such as hello external temperature, RPM, or windspeed.</span></span> <span data-ttu-id="a500c-107">Sedan kan du visualisera detta ytterligare telemetri för hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="a500c-107">You can then visualize this additional telemetry on hello dashboard.</span></span>

<span data-ttu-id="a500c-108">Den här kursen använder en enkel simulerade Node.js-enhet som du kan enkelt ändra tooexperiment med dynamiska telemetri.</span><span class="sxs-lookup"><span data-stu-id="a500c-108">This tutorial uses a simple Node.js simulated device that you can easily modify tooexperiment with dynamic telemetry.</span></span>

<span data-ttu-id="a500c-109">toocomplete den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="a500c-109">toocomplete this tutorial, you’ll need:</span></span>

* <span data-ttu-id="a500c-110">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a500c-110">An active Azure subscription.</span></span> <span data-ttu-id="a500c-111">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="a500c-111">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="a500c-112">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="a500c-112">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
* <span data-ttu-id="a500c-113">[Node.js] [ lnk-node] version 0.12.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="a500c-113">[Node.js][lnk-node] version 0.12.x or later.</span></span>

<span data-ttu-id="a500c-114">Du kan slutföra den här självstudiekursen på alla operativsystem, till exempel Windows eller Linux, där du kan installera Node.js.</span><span class="sxs-lookup"><span data-stu-id="a500c-114">You can complete this tutorial on any operating system, such as Windows or Linux, where you can install Node.js.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a><span data-ttu-id="a500c-115">Lägga till en telemetri typ.</span><span class="sxs-lookup"><span data-stu-id="a500c-115">Add a telemetry type</span></span>

<span data-ttu-id="a500c-116">hello nästa steg är tooreplace hello telemetri som genererats av hello Node.js simulerade enhet med en ny uppsättning värden:</span><span class="sxs-lookup"><span data-stu-id="a500c-116">hello next step is tooreplace hello telemetry generated by hello Node.js simulated device with a new set of values:</span></span>

1. <span data-ttu-id="a500c-117">Stoppa hello Node.js simulerade enheten genom att skriva **Ctrl + C** i Kommandotolken eller shell.</span><span class="sxs-lookup"><span data-stu-id="a500c-117">Stop hello Node.js simulated device by typing **Ctrl+C** in your command prompt or shell.</span></span>
2. <span data-ttu-id="a500c-118">Du kan se hello grunddata värden för hello befintliga temperatur, fuktighet och externa temperatur telemetri i hello remote_monitoring.js-filen.</span><span class="sxs-lookup"><span data-stu-id="a500c-118">In hello remote_monitoring.js file, you can see hello base data values for hello existing temperature, humidity, and external temperature telemetry.</span></span> <span data-ttu-id="a500c-119">Lägg till ett värde för basdata för **rpm** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a500c-119">Add a base data value for **rpm** as follows:</span></span>

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. <span data-ttu-id="a500c-120">Hej Node.js simulerade enhet använder hello **generateRandomIncrement** fungera i hello remote_monitoring.js filen tooadd en slumpmässig ökning toohello grunddata värden.</span><span class="sxs-lookup"><span data-stu-id="a500c-120">hello Node.js simulated device uses hello **generateRandomIncrement** function in hello remote_monitoring.js file tooadd a random increment toohello base data values.</span></span> <span data-ttu-id="a500c-121">Slumpa hello **rpm** värde genom att lägga till en rad med kod efter hello befintliga randomizations på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a500c-121">Randomize hello **rpm** value by adding a line of code after hello existing randomizations as follows:</span></span>

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. <span data-ttu-id="a500c-122">Lägg till hello nya rpm värdet toohello JSON-nyttolast hello enheten skickar tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="a500c-122">Add hello new rpm value toohello JSON payload hello device sends tooIoT Hub:</span></span>

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. <span data-ttu-id="a500c-123">Kör hello Node.js simulerade enhet med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a500c-123">Run hello Node.js simulated device using hello following command:</span></span>

    `node remote_monitoring.js`

6. <span data-ttu-id="a500c-124">Observera hello ny RPM telemetri typ som visas på hello diagram i hello instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="a500c-124">Observe hello new RPM telemetry type that displays on hello chart in hello dashboard:</span></span>

![Lägga till RPM toohello instrumentpanel][image3]

> [!NOTE]
> <span data-ttu-id="a500c-126">Du kanske behöver toodisable och sedan aktivera hello Node.js-enheten på hello **enheter** sida i hello instrumentpanelen toosee hello ändra omedelbart.</span><span class="sxs-lookup"><span data-stu-id="a500c-126">You may need toodisable and then enable hello Node.js device on hello **Devices** page in hello dashboard toosee hello change immediately.</span></span>

## <a name="customize-hello-dashboard-display"></a><span data-ttu-id="a500c-127">Anpassa hello instrumentpanelsvy</span><span class="sxs-lookup"><span data-stu-id="a500c-127">Customize hello dashboard display</span></span>

<span data-ttu-id="a500c-128">Hej **enhetsinformation** meddelandet kan innehålla metadata om hello telemetri hello enheten kan skicka tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="a500c-128">hello **Device-Info** message can include metadata about hello telemetry hello device can send tooIoT Hub.</span></span> <span data-ttu-id="a500c-129">Dessa metadata kan ange hello telemetri typer hello enheten skickar.</span><span class="sxs-lookup"><span data-stu-id="a500c-129">This metadata can specify hello telemetry types hello device sends.</span></span> <span data-ttu-id="a500c-130">Ändra hello **deviceMetaData** värdet i hello remote_monitoring.js filen tooinclude en **telemetri** definition följande hello **kommandon** definition.</span><span class="sxs-lookup"><span data-stu-id="a500c-130">Modify hello **deviceMetaData** value in hello remote_monitoring.js file tooinclude a **Telemetry** definition following hello **Commands** definition.</span></span> <span data-ttu-id="a500c-131">hello följande kodavsnitt visar hello **kommandon** definition (vara säker på att tooadd en `,` efter hello **kommandon** definition):</span><span class="sxs-lookup"><span data-stu-id="a500c-131">hello following code snippet shows hello **Commands** definition (be sure tooadd a `,` after hello **Commands** definition):</span></span>

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
> <span data-ttu-id="a500c-132">hello remote övervakningslösning använder en icke-skiftlägeskänsliga matchningen toocompare hello metadatadefinition med data i hello telemetri dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="a500c-132">hello remote monitoring solution uses a case-insensitive match toocompare hello metadata definition with data in hello telemetry stream.</span></span>


<span data-ttu-id="a500c-133">Lägga till en **telemetri** definition enligt hello föregående kodfragment inte ändra hello beteende hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="a500c-133">Adding a **Telemetry** definition as shown in hello preceding code snippet does not change hello behavior of hello dashboard.</span></span> <span data-ttu-id="a500c-134">Dock hello metadata kan även inkludera ett **DisplayName** attributet toocustomize hello visas i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="a500c-134">However, hello metadata can also include a **DisplayName** attribute toocustomize hello display in hello dashboard.</span></span> <span data-ttu-id="a500c-135">Uppdatera hello **telemetri** metadatadefinitionen som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="a500c-135">Update hello **Telemetry** metadata definition as shown in hello following snippet:</span></span>

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

<span data-ttu-id="a500c-136">hello följande skärmbild visar hur den här ändringen ändrar hello diagramförklaringen på hello instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="a500c-136">hello following screenshot shows how this change modifies hello chart legend on hello dashboard:</span></span>

![Anpassa hello diagrammets förklaring][image4]

> [!NOTE]
> <span data-ttu-id="a500c-138">Du kanske behöver toodisable och sedan aktivera hello Node.js-enheten på hello **enheter** sida i hello instrumentpanelen toosee hello ändra omedelbart.</span><span class="sxs-lookup"><span data-stu-id="a500c-138">You may need toodisable and then enable hello Node.js device on hello **Devices** page in hello dashboard toosee hello change immediately.</span></span>

## <a name="filter-hello-telemetry-types"></a><span data-ttu-id="a500c-139">Filtrera hello telemetri typer</span><span class="sxs-lookup"><span data-stu-id="a500c-139">Filter hello telemetry types</span></span>

<span data-ttu-id="a500c-140">Som standard visar hello diagram på instrumentpanelen för hello varje dataserie i hello telemetri dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="a500c-140">By default, hello chart on hello dashboard shows every data series in hello telemetry stream.</span></span> <span data-ttu-id="a500c-141">Du kan använda hello **enhetsinformation** metadata toosuppress hello visningen av specifika telemetri typer i hello diagram.</span><span class="sxs-lookup"><span data-stu-id="a500c-141">You can use hello **Device-Info** metadata toosuppress hello display of specific telemetry types on hello chart.</span></span> 

<span data-ttu-id="a500c-142">toomake hello diagram visar endast temperatur- och Fuktighetskonsekvens telemetri utelämna **ExternalTemperature** från hello **enhetsinformation** **telemetri** metadata på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a500c-142">toomake hello chart show only Temperature and Humidity telemetry, omit **ExternalTemperature** from hello **Device-Info** **Telemetry** metadata as follows:</span></span>

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

<span data-ttu-id="a500c-143">Hej **utomhus temperatur** visas inte längre i hello diagram:</span><span class="sxs-lookup"><span data-stu-id="a500c-143">hello **Outdoor Temperature** no longer displays on hello chart:</span></span>

![Filtrera hello telemetri för hello instrumentpanelen][image5]

<span data-ttu-id="a500c-145">Den här ändringen påverkar bara hello diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="a500c-145">This change only affects hello chart display.</span></span> <span data-ttu-id="a500c-146">Hej **ExternalTemperature** datavärden fortfarande lagras och blir tillgänglig för alla backend-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="a500c-146">hello **ExternalTemperature** data values are still stored and made available for any backend processing.</span></span>

> [!NOTE]
> <span data-ttu-id="a500c-147">Du kanske behöver toodisable och sedan aktivera hello Node.js-enheten på hello **enheter** sida i hello instrumentpanelen toosee hello ändra omedelbart.</span><span class="sxs-lookup"><span data-stu-id="a500c-147">You may need toodisable and then enable hello Node.js device on hello **Devices** page in hello dashboard toosee hello change immediately.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="a500c-148">Hantera fel</span><span class="sxs-lookup"><span data-stu-id="a500c-148">Handle errors</span></span>

<span data-ttu-id="a500c-149">För en data-dataströmmen toodisplay av hello diagram, dess **typen** i hello **enhetsinformation** metadata måste matcha hello datatyp hello telemetri värden.</span><span class="sxs-lookup"><span data-stu-id="a500c-149">For a data stream toodisplay on hello chart, its **Type** in hello **Device-Info** metadata must match hello data type of hello telemetry values.</span></span> <span data-ttu-id="a500c-150">Till exempel om hello metadata anger att hello **typen** för fuktighet data är **int** och en **dubbla** hittas i hello telemetri dataströmmen sedan hello fuktighet telemetri har Visa inte i hello schemat.</span><span class="sxs-lookup"><span data-stu-id="a500c-150">For example, if hello metadata specifies that hello **Type** of humidity data is **int** and a **double** is found in hello telemetry stream then hello humidity telemetry does not display on hello chart.</span></span> <span data-ttu-id="a500c-151">Dock hello **fuktighet** värden fortfarande lagras och blir tillgänglig för alla backend-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="a500c-151">However, hello **Humidity** values are still stored and made available for any back-end processing.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a500c-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a500c-152">Next steps</span></span>

<span data-ttu-id="a500c-153">Nu när du har sett hur toouse dynamiska telemetri, kan du lära dig mer om hur hello förkonfigurerade lösningar använder enhetsinformation: [enheten information metadata i hello fjärrövervaknings förkonfigurerade lösningen] [ lnk-devinfo].</span><span class="sxs-lookup"><span data-stu-id="a500c-153">Now that you've seen how toouse dynamic telemetry, you can learn more about how hello preconfigured solutions use device information: [Device information metadata in hello remote monitoring preconfigured solution][lnk-devinfo].</span></span>

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
