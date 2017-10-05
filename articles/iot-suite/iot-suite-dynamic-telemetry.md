---
title: "Använd dynamiska telemetri | Microsoft Docs"
description: "Den här kursen om du vill veta hur du använder dynamiska telemetri med Azure IoT Suite remote förkonfigurerade övervakningslösning."
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
ms.openlocfilehash: 0114f27f9b8ae76e1170d04ddf66e2c4bf20686a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="ee9ad-103">Använd dynamiska telemetri med fjärråtkomst övervakning förkonfigurerade lösningen</span><span class="sxs-lookup"><span data-stu-id="ee9ad-103">Use dynamic telemetry with the remote monitoring preconfigured solution</span></span>

<span data-ttu-id="ee9ad-104">Dynamisk telemetri kan du visualisera alla telemetri som skickas till den fjärranslutna förkonfigurerade övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-104">Dynamic telemetry enables you to visualize any telemetry sent to the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="ee9ad-105">Simulerade enheter som distribueras med förkonfigurerade lösningen skicka temperatur- och fuktighetskonsekvens telemetri som du kan visualisera på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-105">The simulated devices that deploy with the preconfigured solution send temperature and humidity telemetry, which you can visualize on the dashboard.</span></span> <span data-ttu-id="ee9ad-106">Om du anpassa befintliga simulerade enheter, skapa nya simulerade enheter eller ansluta fysiska enheter till den förkonfigurerade lösningen kan du skicka andra telemetri värden som externa temperatur, RPM eller vindhastigheten.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-106">If you customize existing simulated devices, create new simulated devices, or connect physical devices to the preconfigured solution you can send other telemetry values such as the external temperature, RPM, or windspeed.</span></span> <span data-ttu-id="ee9ad-107">Sedan kan du visualisera detta ytterligare telemetri på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-107">You can then visualize this additional telemetry on the dashboard.</span></span>

<span data-ttu-id="ee9ad-108">Den här kursen använder en enkel Node.js simulerade enhet som du kan enkelt ändra om du vill experimentera med dynamiska telemetri.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-108">This tutorial uses a simple Node.js simulated device that you can easily modify to experiment with dynamic telemetry.</span></span>

<span data-ttu-id="ee9ad-109">Den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="ee9ad-109">To complete this tutorial, you’ll need:</span></span>

* <span data-ttu-id="ee9ad-110">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-110">An active Azure subscription.</span></span> <span data-ttu-id="ee9ad-111">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-111">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ee9ad-112">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="ee9ad-112">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
* <span data-ttu-id="ee9ad-113">[Node.js] [ lnk-node] version 0.12.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-113">[Node.js][lnk-node] version 0.12.x or later.</span></span>

<span data-ttu-id="ee9ad-114">Du kan slutföra den här självstudiekursen på alla operativsystem, till exempel Windows eller Linux, där du kan installera Node.js.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-114">You can complete this tutorial on any operating system, such as Windows or Linux, where you can install Node.js.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a><span data-ttu-id="ee9ad-115">Lägga till en telemetri typ.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-115">Add a telemetry type</span></span>

<span data-ttu-id="ee9ad-116">Nästa steg är att ersätta telemetri som genererats av Node.js simulerade enheten med en ny uppsättning värden:</span><span class="sxs-lookup"><span data-stu-id="ee9ad-116">The next step is to replace the telemetry generated by the Node.js simulated device with a new set of values:</span></span>

1. <span data-ttu-id="ee9ad-117">Stoppa Node.js simulerade enheten genom att skriva **Ctrl + C** i Kommandotolken eller shell.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-117">Stop the Node.js simulated device by typing **Ctrl+C** in your command prompt or shell.</span></span>
2. <span data-ttu-id="ee9ad-118">Du kan se grunddata värdena för befintliga temperatur, fuktighet och externa temperatur telemetri i filen remote_monitoring.js.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-118">In the remote_monitoring.js file, you can see the base data values for the existing temperature, humidity, and external temperature telemetry.</span></span> <span data-ttu-id="ee9ad-119">Lägg till ett värde för basdata för **rpm** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ee9ad-119">Add a base data value for **rpm** as follows:</span></span>

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. <span data-ttu-id="ee9ad-120">Node.js simulerade enheten använder den **generateRandomIncrement** funktion i filen remote_monitoring.js att lägga till en slumpmässig ökning i grunddata-värden.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-120">The Node.js simulated device uses the **generateRandomIncrement** function in the remote_monitoring.js file to add a random increment to the base data values.</span></span> <span data-ttu-id="ee9ad-121">Slumpa den **rpm** värde genom att lägga till en rad med kod efter befintliga randomizations på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ee9ad-121">Randomize the **rpm** value by adding a line of code after the existing randomizations as follows:</span></span>

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. <span data-ttu-id="ee9ad-122">Lägg till det nya rpm-värdet i JSON-nyttolast enheten skickar till IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="ee9ad-122">Add the new rpm value to the JSON payload the device sends to IoT Hub:</span></span>

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. <span data-ttu-id="ee9ad-123">Kör Node.js simulerade enheten med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ee9ad-123">Run the Node.js simulated device using the following command:</span></span>

    `node remote_monitoring.js`

6. <span data-ttu-id="ee9ad-124">Se den nya typen av RPM telemetri som visas i diagram på instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="ee9ad-124">Observe the new RPM telemetry type that displays on the chart in the dashboard:</span></span>

![Lägg till RPM på instrumentpanelen][image3]

> [!NOTE]
> <span data-ttu-id="ee9ad-126">Du kan behöva inaktivera och aktivera sedan Node.js-enhet på den **enheter** sida i instrumentpanelen för att se ändringen direkt.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-126">You may need to disable and then enable the Node.js device on the **Devices** page in the dashboard to see the change immediately.</span></span>

## <a name="customize-the-dashboard-display"></a><span data-ttu-id="ee9ad-127">Anpassa instrumentpanelsvy</span><span class="sxs-lookup"><span data-stu-id="ee9ad-127">Customize the dashboard display</span></span>

<span data-ttu-id="ee9ad-128">Den **enhetsinformation** meddelandet kan innehålla metadata om den telemetriska enheten kan skicka till IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-128">The **Device-Info** message can include metadata about the telemetry the device can send to IoT Hub.</span></span> <span data-ttu-id="ee9ad-129">Dessa metadata kan ange vilka telemetri enheten skickar.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-129">This metadata can specify the telemetry types the device sends.</span></span> <span data-ttu-id="ee9ad-130">Ändra den **deviceMetaData** värdet i filen remote_monitoring.js för att inkludera en **telemetri** definition följande den **kommandon** definition.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-130">Modify the **deviceMetaData** value in the remote_monitoring.js file to include a **Telemetry** definition following the **Commands** definition.</span></span> <span data-ttu-id="ee9ad-131">I följande kod fragment visas den **kommandon** definition (se till att lägga till en `,` när den **kommandon** definition):</span><span class="sxs-lookup"><span data-stu-id="ee9ad-131">The following code snippet shows the **Commands** definition (be sure to add a `,` after the **Commands** definition):</span></span>

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
> <span data-ttu-id="ee9ad-132">Fjärråtkomst övervakningslösning använder en skiftlägeskänslig matchning för att jämföra metadatadefinitionen med data i dataströmmen telemetri.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-132">The remote monitoring solution uses a case-insensitive match to compare the metadata definition with data in the telemetry stream.</span></span>


<span data-ttu-id="ee9ad-133">Lägga till en **telemetri** definition som visas i föregående kodfragment ändrar inte beteendet för instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-133">Adding a **Telemetry** definition as shown in the preceding code snippet does not change the behavior of the dashboard.</span></span> <span data-ttu-id="ee9ad-134">Metadata kan dock också inkludera en **DisplayName** attribut för att anpassa visningen i instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-134">However, the metadata can also include a **DisplayName** attribute to customize the display in the dashboard.</span></span> <span data-ttu-id="ee9ad-135">Uppdatering av **telemetri** metadatadefinitionen som visas i följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="ee9ad-135">Update the **Telemetry** metadata definition as shown in the following snippet:</span></span>

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

<span data-ttu-id="ee9ad-136">Följande skärmbild visar hur den här ändringen ändrar diagramförklaringen på instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="ee9ad-136">The following screenshot shows how this change modifies the chart legend on the dashboard:</span></span>

![Anpassa diagrammets förklaring][image4]

> [!NOTE]
> <span data-ttu-id="ee9ad-138">Du kan behöva inaktivera och aktivera sedan Node.js-enhet på den **enheter** sida i instrumentpanelen för att se ändringen direkt.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-138">You may need to disable and then enable the Node.js device on the **Devices** page in the dashboard to see the change immediately.</span></span>

## <a name="filter-the-telemetry-types"></a><span data-ttu-id="ee9ad-139">Filtrera telemetri-typer</span><span class="sxs-lookup"><span data-stu-id="ee9ad-139">Filter the telemetry types</span></span>

<span data-ttu-id="ee9ad-140">Som standard visar diagram på instrumentpanelen varje dataserie i telemetri dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-140">By default, the chart on the dashboard shows every data series in the telemetry stream.</span></span> <span data-ttu-id="ee9ad-141">Du kan använda den **enhetsinformation** metadata för visningen av specifika telemetri typer i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-141">You can use the **Device-Info** metadata to suppress the display of specific telemetry types on the chart.</span></span> 

<span data-ttu-id="ee9ad-142">Att visa endast temperatur- och Fuktighetskonsekvens telemetri, utelämna **ExternalTemperature** från den **enhetsinformation** **telemetri** metadata på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ee9ad-142">To make the chart show only Temperature and Humidity telemetry, omit **ExternalTemperature** from the **Device-Info** **Telemetry** metadata as follows:</span></span>

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

<span data-ttu-id="ee9ad-143">Den **utomhus temperatur** inte längre visas i diagrammet:</span><span class="sxs-lookup"><span data-stu-id="ee9ad-143">The **Outdoor Temperature** no longer displays on the chart:</span></span>

![Filtrera telemetri på instrumentpanelen][image5]

<span data-ttu-id="ee9ad-145">Den här ändringen påverkar bara diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-145">This change only affects the chart display.</span></span> <span data-ttu-id="ee9ad-146">Den **ExternalTemperature** datavärden fortfarande lagras och blir tillgänglig för alla backend-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-146">The **ExternalTemperature** data values are still stored and made available for any backend processing.</span></span>

> [!NOTE]
> <span data-ttu-id="ee9ad-147">Du kan behöva inaktivera och aktivera sedan Node.js-enhet på den **enheter** sida i instrumentpanelen för att se ändringen direkt.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-147">You may need to disable and then enable the Node.js device on the **Devices** page in the dashboard to see the change immediately.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="ee9ad-148">Hantera fel</span><span class="sxs-lookup"><span data-stu-id="ee9ad-148">Handle errors</span></span>

<span data-ttu-id="ee9ad-149">För en dataström som ska visas i diagrammet, dess **typen** i den **enhetsinformation** metadata måste matcha värdena telemetri datatyp.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-149">For a data stream to display on the chart, its **Type** in the **Device-Info** metadata must match the data type of the telemetry values.</span></span> <span data-ttu-id="ee9ad-150">Till exempel om metadata som anger att den **typen** för fuktighet data är **int** och en **dubbla** hittas i dataströmmen telemetri och sedan fuktighet telemetri inte visas i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-150">For example, if the metadata specifies that the **Type** of humidity data is **int** and a **double** is found in the telemetry stream then the humidity telemetry does not display on the chart.</span></span> <span data-ttu-id="ee9ad-151">Men den **fuktighet** värden fortfarande lagras och blir tillgänglig för alla backend-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="ee9ad-151">However, the **Humidity** values are still stored and made available for any back-end processing.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee9ad-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ee9ad-152">Next steps</span></span>

<span data-ttu-id="ee9ad-153">Nu när du har lärt dig hur du använder dynamiska telemetri, kan du läsa mer om hur förkonfigurerade lösningar använder enhetsinformation: [enhetens information metadata för fjärranslutna övervakningen förkonfigurerade lösningen] [ lnk-devinfo].</span><span class="sxs-lookup"><span data-stu-id="ee9ad-153">Now that you've seen how to use dynamic telemetry, you can learn more about how the preconfigured solutions use device information: [Device information metadata in the remote monitoring preconfigured solution][lnk-devinfo].</span></span>

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
