---
title: "Genomgång av den förkonfigurerade lösningen för fjärrövervakning | Microsoft Docs"
description: "En beskrivning av den förkonfigurerade fjärrövervakningslösningen i Azure IoT och dess arkitektur."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: b28105f300723b542fa6d1aebc569439d5c73dc4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a><span data-ttu-id="9b2c3-103">Genomgång av den förkonfigurerade lösningen för fjärrövervakning</span><span class="sxs-lookup"><span data-stu-id="9b2c3-103">Remote monitoring preconfigured solution walkthrough</span></span>

<span data-ttu-id="9b2c3-104">Den [förkonfigurerade fjärrövervakningslösningen][lnk-preconfigured-solutions] i IoT Suite är en implementering av en övervakningslösning från slutpunkt till slutpunkt för flera datorer som körs på fjärrplatser.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-104">The IoT Suite remote monitoring [preconfigured solution][lnk-preconfigured-solutions] is an implementation of an end-to-end monitoring solution for multiple machines running in remote locations.</span></span> <span data-ttu-id="9b2c3-105">I lösningen kombineras viktiga Azure-tjänster till en allmän implementering av affärsscenariot.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-105">The solution combines key Azure services to provide a generic implementation of the business scenario.</span></span> <span data-ttu-id="9b2c3-106">Du kan använda lösningen som startpunkt för en egen implementering och [anpassa][lnk-customize] den efter dina egna affärsbehov.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-106">You can use the solution as a starting point for your own implementation and [customize][lnk-customize] it to meet your own specific business requirements.</span></span>

<span data-ttu-id="9b2c3-107">Den här artikeln beskriver några av de viktigaste elementen i fjärrövervakningslösningen så att du förstår hur den fungerar.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-107">This article walks you through some of the key elements of the remote monitoring solution to enable you to understand how it works.</span></span> <span data-ttu-id="9b2c3-108">Med den här kunskapen kan du sedan:</span><span class="sxs-lookup"><span data-stu-id="9b2c3-108">This knowledge helps you to:</span></span>

* <span data-ttu-id="9b2c3-109">Felsöka problem i lösningen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-109">Troubleshoot issues in the solution.</span></span>
* <span data-ttu-id="9b2c3-110">Planera hur lösningen kan anpassas för att uppfylla dina behov.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-110">Plan how to customize to the solution to meet your own specific requirements.</span></span> 
* <span data-ttu-id="9b2c3-111">Utforma en egen IoT-lösning som använder Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-111">Design your own IoT solution that uses Azure services.</span></span>

## <a name="logical-architecture"></a><span data-ttu-id="9b2c3-112">Logisk arkitektur</span><span class="sxs-lookup"><span data-stu-id="9b2c3-112">Logical architecture</span></span>

<span data-ttu-id="9b2c3-113">Följande diagram illustrerar de logiska komponenterna i den förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="9b2c3-113">The following diagram outlines the logical components of the preconfigured solution:</span></span>

![Logisk arkitektur](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="simulated-devices"></a><span data-ttu-id="9b2c3-115">Simulerade enheter</span><span class="sxs-lookup"><span data-stu-id="9b2c3-115">Simulated devices</span></span>

<span data-ttu-id="9b2c3-116">I den förkonfigurerade lösningen representerar den simulerade enheten en kylningsenhet (till exempel en ventilations- eller luftkonditioneringsapparat i en byggnad eller lokal).</span><span class="sxs-lookup"><span data-stu-id="9b2c3-116">In the preconfigured solution, the simulated device represents a cooling device (such as a building air conditioner or facility air handling unit).</span></span> <span data-ttu-id="9b2c3-117">När du distribuerar den förkonfigurerade lösningen etablerar du också automatiskt fyra simulerade enheter som körs i ett [Azure-webbjobb][lnk-webjobs].</span><span class="sxs-lookup"><span data-stu-id="9b2c3-117">When you deploy the preconfigured solution, you also automatically provision four simulated devices that run in an [Azure WebJob][lnk-webjobs].</span></span> <span data-ttu-id="9b2c3-118">De simulerade enheterna gör det enkelt att utforska lösningens beteende utan att du behöver distribuera några fysiska enheter.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-118">The simulated devices make it easy for you to explore the behavior of the solution without the need to deploy any physical devices.</span></span> <span data-ttu-id="9b2c3-119">Om du vill distribuera en riktig fysisk enhet går du självstudiekursen [Ansluta enheten till den förkonfigurerade fjärrövervakningslösningen][lnk-connect-rm].</span><span class="sxs-lookup"><span data-stu-id="9b2c3-119">To deploy a real physical device, see the [Connect your device to the remote monitoring preconfigured solution][lnk-connect-rm] tutorial.</span></span>

### <a name="device-to-cloud-messages"></a><span data-ttu-id="9b2c3-120">Meddelanden från enheten till molnet</span><span class="sxs-lookup"><span data-stu-id="9b2c3-120">Device-to-cloud messages</span></span>

<span data-ttu-id="9b2c3-121">Varje simulerad enhet kan skicka följande typer av meddelanden till IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="9b2c3-121">Each simulated device can send the following message types to IoT Hub:</span></span>

| <span data-ttu-id="9b2c3-122">Meddelande</span><span class="sxs-lookup"><span data-stu-id="9b2c3-122">Message</span></span> | <span data-ttu-id="9b2c3-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9b2c3-123">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9b2c3-124">Start</span><span class="sxs-lookup"><span data-stu-id="9b2c3-124">Startup</span></span> |<span data-ttu-id="9b2c3-125">När enheten startas skickar den ett **enhetsinformationsmeddelande** som innehåller information om enheten till backend-servern.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-125">When the device starts, it sends a **device-info** message containing information about itself to the back end.</span></span> <span data-ttu-id="9b2c3-126">Den här informationen är bland annat enhets-ID:t och en lista med kommandon och metoder som enheten har stöd för.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-126">This data includes the device id and a list of the commands and methods the device supports.</span></span> |
| <span data-ttu-id="9b2c3-127">Närvaro</span><span class="sxs-lookup"><span data-stu-id="9b2c3-127">Presence</span></span> |<span data-ttu-id="9b2c3-128">En enhet skickar regelbundet ett **närvaromeddelande** för att rapportera om den kan känna av närvaron av en sensor.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-128">A device periodically sends a **presence** message to report whether the device can sense the presence of a sensor.</span></span> |
| <span data-ttu-id="9b2c3-129">Telemetri</span><span class="sxs-lookup"><span data-stu-id="9b2c3-129">Telemetry</span></span> |<span data-ttu-id="9b2c3-130">En enhet skickar regelbundet ett **telemetrimeddelande** för att rapporterar simulerade värden för temperaturen och fuktigheten som samlats in från enhetens simulerade sensorer.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-130">A device periodically sends a **telemetry** message that reports simulated values for the temperature and humidity collected from the device's simulated sensors.</span></span> |

> [!NOTE]
> <span data-ttu-id="9b2c3-131">Listan med kommandon som stöds av enheten lagras i en Cosmos DB-databas och inte i enhetstvillingen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-131">The solution stores the list of commands supported by the device in a Cosmos DB database and not in the device twin.</span></span>

### <a name="properties-and-device-twins"></a><span data-ttu-id="9b2c3-132">Egenskaper och enhetstvillingar</span><span class="sxs-lookup"><span data-stu-id="9b2c3-132">Properties and device twins</span></span>

<span data-ttu-id="9b2c3-133">De simulerade enheterna skickar följande enhetsegenskaper till [tvillingen][lnk-device-twins] i IoT Hub som *rapporterade egenskaper*.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-133">The simulated devices send the following device properties to the [twin][lnk-device-twins] in the IoT hub as *reported properties*.</span></span> <span data-ttu-id="9b2c3-134">Enheten skickar rapporterade egenskaper vid start och som svar på ett kommando eller en metod om att **ändra enhetens tillstånd**.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-134">The device sends reported properties at startup and in response to a **Change Device State** command or method.</span></span>

| <span data-ttu-id="9b2c3-135">Egenskap</span><span class="sxs-lookup"><span data-stu-id="9b2c3-135">Property</span></span> | <span data-ttu-id="9b2c3-136">Syfte</span><span class="sxs-lookup"><span data-stu-id="9b2c3-136">Purpose</span></span> |
| --- | --- |
| <span data-ttu-id="9b2c3-137">Config.TelemetryInterval</span><span class="sxs-lookup"><span data-stu-id="9b2c3-137">Config.TelemetryInterval</span></span> | <span data-ttu-id="9b2c3-138">Den frekvens (i sekunder) som enheten skickar telemetri med</span><span class="sxs-lookup"><span data-stu-id="9b2c3-138">Frequency (seconds) the device sends telemetry</span></span> |
| <span data-ttu-id="9b2c3-139">Config.TemperatureMeanValue</span><span class="sxs-lookup"><span data-stu-id="9b2c3-139">Config.TemperatureMeanValue</span></span> | <span data-ttu-id="9b2c3-140">Anger medelvärdet för telemetrin för simulerad temperatur</span><span class="sxs-lookup"><span data-stu-id="9b2c3-140">Specifies the mean value for the simulated temperature telemetry</span></span> |
| <span data-ttu-id="9b2c3-141">Device.DeviceID</span><span class="sxs-lookup"><span data-stu-id="9b2c3-141">Device.DeviceID</span></span> |<span data-ttu-id="9b2c3-142">Ett ID som antingen anges eller tilldelas när en enhet skapas i lösningen</span><span class="sxs-lookup"><span data-stu-id="9b2c3-142">Id that is either provided or assigned when a device is created in the solution</span></span> |
| <span data-ttu-id="9b2c3-143">Device.DeviceState</span><span class="sxs-lookup"><span data-stu-id="9b2c3-143">Device.DeviceState</span></span> | <span data-ttu-id="9b2c3-144">Tillståndet som rapporteras av enheten</span><span class="sxs-lookup"><span data-stu-id="9b2c3-144">State reported by the device</span></span> |
| <span data-ttu-id="9b2c3-145">Device.CreatedTime</span><span class="sxs-lookup"><span data-stu-id="9b2c3-145">Device.CreatedTime</span></span> |<span data-ttu-id="9b2c3-146">Tiden då enheten skapades i lösningen</span><span class="sxs-lookup"><span data-stu-id="9b2c3-146">Time the device was created in the solution</span></span> |
| <span data-ttu-id="9b2c3-147">Device.StartupTime</span><span class="sxs-lookup"><span data-stu-id="9b2c3-147">Device.StartupTime</span></span> |<span data-ttu-id="9b2c3-148">Tidpunkten då enheten startades</span><span class="sxs-lookup"><span data-stu-id="9b2c3-148">Time the device was started</span></span> |
| <span data-ttu-id="9b2c3-149">Device.LastDesiredPropertyChange</span><span class="sxs-lookup"><span data-stu-id="9b2c3-149">Device.LastDesiredPropertyChange</span></span> |<span data-ttu-id="9b2c3-150">Versionsnumret för den senast önskade egenskapsändringen</span><span class="sxs-lookup"><span data-stu-id="9b2c3-150">The version number of the last desired property change</span></span> |
| <span data-ttu-id="9b2c3-151">Device.Location.Latitude</span><span class="sxs-lookup"><span data-stu-id="9b2c3-151">Device.Location.Latitude</span></span> |<span data-ttu-id="9b2c3-152">Enhetens latitud</span><span class="sxs-lookup"><span data-stu-id="9b2c3-152">Latitude location of the device</span></span> |
| <span data-ttu-id="9b2c3-153">Device.Location.Longitude</span><span class="sxs-lookup"><span data-stu-id="9b2c3-153">Device.Location.Longitude</span></span> |<span data-ttu-id="9b2c3-154">Enhetens longitud</span><span class="sxs-lookup"><span data-stu-id="9b2c3-154">Longitude location of the device</span></span> |
| <span data-ttu-id="9b2c3-155">System.Manufacturer</span><span class="sxs-lookup"><span data-stu-id="9b2c3-155">System.Manufacturer</span></span> |<span data-ttu-id="9b2c3-156">Enhetstillverkare</span><span class="sxs-lookup"><span data-stu-id="9b2c3-156">Device manufacturer</span></span> |
| <span data-ttu-id="9b2c3-157">System.ModelNumber</span><span class="sxs-lookup"><span data-stu-id="9b2c3-157">System.ModelNumber</span></span> |<span data-ttu-id="9b2c3-158">Enhetens modellnummer</span><span class="sxs-lookup"><span data-stu-id="9b2c3-158">Model number of the device</span></span> |
| <span data-ttu-id="9b2c3-159">System.SerialNumber</span><span class="sxs-lookup"><span data-stu-id="9b2c3-159">System.SerialNumber</span></span> |<span data-ttu-id="9b2c3-160">Enhetens serienummer</span><span class="sxs-lookup"><span data-stu-id="9b2c3-160">Serial number of the device</span></span> |
| <span data-ttu-id="9b2c3-161">System.FirmwareVersion</span><span class="sxs-lookup"><span data-stu-id="9b2c3-161">System.FirmwareVersion</span></span> |<span data-ttu-id="9b2c3-162">Aktuell version av enhetens inbyggda programvara</span><span class="sxs-lookup"><span data-stu-id="9b2c3-162">Current version of firmware on the device</span></span> |
| <span data-ttu-id="9b2c3-163">System.Platform</span><span class="sxs-lookup"><span data-stu-id="9b2c3-163">System.Platform</span></span> |<span data-ttu-id="9b2c3-164">Enhetens plattformsarkitektur</span><span class="sxs-lookup"><span data-stu-id="9b2c3-164">Platform architecture of the device</span></span> |
| <span data-ttu-id="9b2c3-165">System.Processor</span><span class="sxs-lookup"><span data-stu-id="9b2c3-165">System.Processor</span></span> |<span data-ttu-id="9b2c3-166">Processorn som kör enheten</span><span class="sxs-lookup"><span data-stu-id="9b2c3-166">Processor running the device</span></span> |
| <span data-ttu-id="9b2c3-167">System.InstalledRAM</span><span class="sxs-lookup"><span data-stu-id="9b2c3-167">System.InstalledRAM</span></span> |<span data-ttu-id="9b2c3-168">Mängden RAM-minne som är installerat på enheten</span><span class="sxs-lookup"><span data-stu-id="9b2c3-168">Amount of RAM installed on the device</span></span> |

<span data-ttu-id="9b2c3-169">Simulatorn lägger till dessa egenskaper på simulerade enheter med exempelvärden.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-169">The simulator seeds these properties in simulated devices with sample values.</span></span> <span data-ttu-id="9b2c3-170">Varje gång simulatorn initierar en simulerad enhet rapporterar enheten fördefinierade metadata till IoT Hub som rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-170">Each time the simulator initializes a simulated device, the device reports the pre-defined metadata to IoT Hub as reported properties.</span></span> <span data-ttu-id="9b2c3-171">Det är bara enheten som kan uppdatera rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-171">Reported properties can only be updated by the device.</span></span> <span data-ttu-id="9b2c3-172">Om du vill ändra en rapporterad egenskap anger du den önskade egenskapen i lösningsportalen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-172">To change a reported property, you set a desired property in solution portal.</span></span> <span data-ttu-id="9b2c3-173">Enheten ansvarar för följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="9b2c3-173">It is the responsibility of the device to:</span></span>

1. <span data-ttu-id="9b2c3-174">Att med jämna mellanrum hämta önskade egenskaper från IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-174">Periodically retrieve desired properties from the IoT hub.</span></span>
2. <span data-ttu-id="9b2c3-175">Att uppdatera konfigurationen med önskade egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-175">Update its configuration with the desired property value.</span></span>
3. <span data-ttu-id="9b2c3-176">Att skicka tillbaka det nya värdet till hubben som en rapporterad egenskap.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-176">Send the new value back to the hub as a reported property.</span></span>

<span data-ttu-id="9b2c3-177">Från instrumentpanelen i lösningen kan du använda *önskade egenskaper* till att ange egenskaper för en enhet med hjälp av [enhetstvillingen][lnk-device-twins].</span><span class="sxs-lookup"><span data-stu-id="9b2c3-177">From the solution dashboard, you can use *desired properties* to set properties on a device by using the [device twin][lnk-device-twins].</span></span> <span data-ttu-id="9b2c3-178">En enhet läser vanligtvis av ett önskat egenskapsvärde från hubben, uppdaterar det interna tillståndet och rapporterar ändringen som en rapporterad egenskap.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-178">Typically, a device reads a desired property value from the hub to update its internal state and report the change back as a reported property.</span></span>

> [!NOTE]
> <span data-ttu-id="9b2c3-179">I koden för den simulerade enheten är det bara de önskade egenskaperna **Desired.Config.TemperatureMeanValue** och **Desired.Config.TelemetryInterval** som används till att uppdatera de rapporterade egenskaper som skickas tillbaka till IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-179">The simulated device code only uses the **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties to update the reported properties sent back to IoT Hub.</span></span> <span data-ttu-id="9b2c3-180">Alla andra förfrågningar om att ändra önskade egenskaper ignoreras i den simulerade enheten.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-180">All other desired property change requests are ignored in the simulated device.</span></span>

### <a name="methods"></a><span data-ttu-id="9b2c3-181">Metoder</span><span class="sxs-lookup"><span data-stu-id="9b2c3-181">Methods</span></span>

<span data-ttu-id="9b2c3-182">De simulerade enheterna kan hantera följande metoder ([direkta metoder][lnk-direct-methods]) som anropas från lösningsportalen via IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="9b2c3-182">The simulated devices can handle the following methods ([direct methods][lnk-direct-methods]) invoked from the solution portal through the IoT hub:</span></span>

| <span data-ttu-id="9b2c3-183">Metod</span><span class="sxs-lookup"><span data-stu-id="9b2c3-183">Method</span></span> | <span data-ttu-id="9b2c3-184">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9b2c3-184">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9b2c3-185">InitiateFirmwareUpdate</span><span class="sxs-lookup"><span data-stu-id="9b2c3-185">InitiateFirmwareUpdate</span></span> |<span data-ttu-id="9b2c3-186">Anger att enheten ska uppdatera den inbyggda programvaran</span><span class="sxs-lookup"><span data-stu-id="9b2c3-186">Instructs the device to perform a firmware update</span></span> |
| <span data-ttu-id="9b2c3-187">Starta om</span><span class="sxs-lookup"><span data-stu-id="9b2c3-187">Reboot</span></span> |<span data-ttu-id="9b2c3-188">Anger att enheten ska startas om</span><span class="sxs-lookup"><span data-stu-id="9b2c3-188">Instructs the device to reboot</span></span> |
| <span data-ttu-id="9b2c3-189">FactoryReset</span><span class="sxs-lookup"><span data-stu-id="9b2c3-189">FactoryReset</span></span> |<span data-ttu-id="9b2c3-190">Anger att enheten ska fabriksåterställas</span><span class="sxs-lookup"><span data-stu-id="9b2c3-190">Instructs the device to perform a factory reset</span></span> |

<span data-ttu-id="9b2c3-191">I vissa metoder används rapporterade egenskaper till att informera om förloppet.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-191">Some methods use reported properties to report on progress.</span></span> <span data-ttu-id="9b2c3-192">I metoden **InitiateFirmwareUpdate** simuleras till exempel att uppdateringen körs asynkront på enheten.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-192">For example, the **InitiateFirmwareUpdate** method simulates running the update asynchronously on the device.</span></span> <span data-ttu-id="9b2c3-193">Metoden returnerar omedelbart ett resultat på enheten medan den asynkrona uppgiften fortsätter att skicka statusuppdateringar till lösningens instrumentpanel med hjälp av rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-193">The method returns immediately on the device, while the asynchronous task continues to send status updates back to the solution dashboard using reported properties.</span></span>

### <a name="commands"></a><span data-ttu-id="9b2c3-194">Kommandon</span><span class="sxs-lookup"><span data-stu-id="9b2c3-194">Commands</span></span>

<span data-ttu-id="9b2c3-195">De simulerade enheterna kan hantera följande kommandon (meddelanden från molnet till enheten) som skickas från lösningsportalen via IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="9b2c3-195">The simulated devices can handle the following commands (cloud-to-device messages) sent from the solution portal through the IoT hub:</span></span>

| <span data-ttu-id="9b2c3-196">Kommando</span><span class="sxs-lookup"><span data-stu-id="9b2c3-196">Command</span></span> | <span data-ttu-id="9b2c3-197">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9b2c3-197">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9b2c3-198">PingDevice</span><span class="sxs-lookup"><span data-stu-id="9b2c3-198">PingDevice</span></span> |<span data-ttu-id="9b2c3-199">Skickar en *ping* till enheten för att kontrollera att den är aktiv</span><span class="sxs-lookup"><span data-stu-id="9b2c3-199">Sends a *ping* to the device to check it is alive</span></span> |
| <span data-ttu-id="9b2c3-200">StartTelemetry</span><span class="sxs-lookup"><span data-stu-id="9b2c3-200">StartTelemetry</span></span> |<span data-ttu-id="9b2c3-201">Startar enheten som skickar telemetri</span><span class="sxs-lookup"><span data-stu-id="9b2c3-201">Starts the device sending telemetry</span></span> |
| <span data-ttu-id="9b2c3-202">StopTelemetry</span><span class="sxs-lookup"><span data-stu-id="9b2c3-202">StopTelemetry</span></span> |<span data-ttu-id="9b2c3-203">Stoppar enheten så att den inte skickar mer telemetri</span><span class="sxs-lookup"><span data-stu-id="9b2c3-203">Stops the device from sending telemetry</span></span> |
| <span data-ttu-id="9b2c3-204">ChangeSetPointTemp</span><span class="sxs-lookup"><span data-stu-id="9b2c3-204">ChangeSetPointTemp</span></span> |<span data-ttu-id="9b2c3-205">Ändrar värdet för den angivna punkten som slumpmässiga data skapas kring</span><span class="sxs-lookup"><span data-stu-id="9b2c3-205">Changes the set point value around which the random data is generated</span></span> |
| <span data-ttu-id="9b2c3-206">DiagnosticTelemetry</span><span class="sxs-lookup"><span data-stu-id="9b2c3-206">DiagnosticTelemetry</span></span> |<span data-ttu-id="9b2c3-207">Utlöser enhetssimulatorn för att skicka ytterligare ett telemetrivärde (externalTemp)</span><span class="sxs-lookup"><span data-stu-id="9b2c3-207">Triggers the device simulator to send an additional telemetry value (externalTemp)</span></span> |
| <span data-ttu-id="9b2c3-208">ChangeDeviceState</span><span class="sxs-lookup"><span data-stu-id="9b2c3-208">ChangeDeviceState</span></span> |<span data-ttu-id="9b2c3-209">Ändrar en egenskap för utökat läge för enheten och skickar meddelandet med enhetsinformation från enheten</span><span class="sxs-lookup"><span data-stu-id="9b2c3-209">Changes an extended state property for the device and sends the device info message from the device</span></span> |

> [!NOTE]
> <span data-ttu-id="9b2c3-210">En jämförelse av dessa kommandon (meddelanden från molnet till enheten) och metoder (direkta metoder) finns i artikeln om [kommunikation mellan moln och enheter][lnk-c2d-guidance].</span><span class="sxs-lookup"><span data-stu-id="9b2c3-210">For a comparison of these commands (cloud-to-device messages) and methods (direct methods), see [Cloud-to-device communications guidance][lnk-c2d-guidance].</span></span>

## <a name="iot-hub"></a><span data-ttu-id="9b2c3-211">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="9b2c3-211">IoT Hub</span></span>

<span data-ttu-id="9b2c3-212">[IoT Hub][lnk-iothub] matar in data som skickas från enheterna till molnet och gör dem tillgängliga för ASA-jobben (Azure Stream Analytics).</span><span class="sxs-lookup"><span data-stu-id="9b2c3-212">The [IoT hub][lnk-iothub] ingests data sent from the devices into the cloud and makes it available to the Azure Stream Analytics (ASA) jobs.</span></span> <span data-ttu-id="9b2c3-213">Varje ASA-jobb använder en separat IoT Hub-konsumentgrupp för att läsa strömmen av meddelanden från dina enheter.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-213">Each stream ASA job uses a separate IoT Hub consumer group to read the stream of messages from your devices.</span></span>

<span data-ttu-id="9b2c3-214">IoT Hub ansvarar även för följande uppgifter i lösningen:</span><span class="sxs-lookup"><span data-stu-id="9b2c3-214">The IoT hub in the solution also:</span></span>

- <span data-ttu-id="9b2c3-215">Att underhålla ett ID-register där ID:n och autentiseringsnycklar lagras för alla enheter som har behörighet att ansluta till portalen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-215">Maintains an identity registry that stores the ids and authentication keys of all the devices permitted to connect to the portal.</span></span> <span data-ttu-id="9b2c3-216">Du kan aktivera och inaktivera enheter via ID-registret.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-216">You can enable and disable devices through the identity registry.</span></span>
- <span data-ttu-id="9b2c3-217">Att skicka kommandon till dina enheter för lösningsportalens räkning.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-217">Sends commands to your devices on behalf of the solution portal.</span></span>
- <span data-ttu-id="9b2c3-218">Att anropa metoder på dina enheter för lösningsportalens räkning.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-218">Invokes methods on your devices on behalf of the solution portal.</span></span>
- <span data-ttu-id="9b2c3-219">Att underhålla enhetstvillingar för alla registrerade enheter.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-219">Maintains device twins for all registered devices.</span></span> <span data-ttu-id="9b2c3-220">De egenskapsvärden som rapporteras av en enhet lagras i enhetstvillingen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-220">A device twin stores the property values reported by a device.</span></span> <span data-ttu-id="9b2c3-221">De önskade egenskaper som anges i lösningsportalen och som enheten ska hämta vid nästa anslutning lagras också där.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-221">A device twin also stores desired properties, set in the solution portal, for the device to retrieve when it next connects.</span></span>
- <span data-ttu-id="9b2c3-222">Att schemalägga jobb där egenskaper ska anges för flera enheter eller där metoder ska anropas på flera enheter.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-222">Schedules jobs to set properties for multiple devices or invoke methods on multiple devices.</span></span>

## <a name="azure-stream-analytics"></a><span data-ttu-id="9b2c3-223">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9b2c3-223">Azure Stream Analytics</span></span>

<span data-ttu-id="9b2c3-224">I fjärrövervakningslösningen skickar [Azure Stream Analytics][lnk-asa] (ASA) meddelanden som tas emot av IoT Hub till andra serverkomponenter för bearbetning eller lagring.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-224">In the remote monitoring solution, [Azure Stream Analytics][lnk-asa] (ASA) dispatches device messages received by the IoT hub to other back-end components for processing or storage.</span></span> <span data-ttu-id="9b2c3-225">Olika ASA-jobb utför specifika funktioner baserat på innehållet i meddelandena.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-225">Different ASA jobs perform specific functions based on the content of the messages.</span></span>

<span data-ttu-id="9b2c3-226">**Jobb 1: Enhetsinformation** filtrerar meddelandena med enhetsinformation från den inkommande meddelandeströmmen och skickar dem till slutpunkten för en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-226">**Job 1: Device Info** filters device information messages from the incoming message stream and sends them to an Event Hub endpoint.</span></span> <span data-ttu-id="9b2c3-227">En enhet skickar meddelanden med enhetsinformation vid starten och som svar på ett **SendDeviceInfo**-kommando.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-227">A device sends device information messages at startup and in response to a **SendDeviceInfo** command.</span></span> <span data-ttu-id="9b2c3-228">Det här jobbet använder följande frågedefinition för att identifiera **device-info**-meddelanden:</span><span class="sxs-lookup"><span data-stu-id="9b2c3-228">This job uses the following query definition to identify **device-info** messages:</span></span>

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

<span data-ttu-id="9b2c3-229">Det här jobbet skickar sina utdata till en händelsehubb för vidare bearbetning.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-229">This job sends its output to an Event Hub for further processing.</span></span>

<span data-ttu-id="9b2c3-230">**Jobb 2: Regler** utvärderar inkommande telemetrivärden för temperatur och fuktighet mot tröskelvärdena för varje enhet.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-230">**Job 2: Rules** evaluates incoming temperature and humidity telemetry values against per-device thresholds.</span></span> <span data-ttu-id="9b2c3-231">Tröskelvärdena anges i regelredigeraren som finns i lösningsportalen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-231">Threshold values are set in the rules editor available in the solution portal.</span></span> <span data-ttu-id="9b2c3-232">Varje enhet/värde-par lagras med en tidsstämpel i en blobb som Stream Analytics läser in som **referensdata**.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-232">Each device/value pair is stored by timestamp in a blob which Stream Analytics reads in as **Reference Data**.</span></span> <span data-ttu-id="9b2c3-233">Jobbet jämför icke-tomma värden mot det angivna tröskelvärdet för enheten.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-233">The job compares any non-empty value against the set threshold for the device.</span></span> <span data-ttu-id="9b2c3-234">Om det överskrider ” >”-villkoret returnerar jobbet en **larmhändelse** som anger att tröskelvärdet överskridits och som visar enheten, värdet och tidstämpeln.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-234">If it exceeds the '>' condition, the job outputs an **alarm** event that indicates that the threshold is exceeded and provides the device, value, and timestamp values.</span></span> <span data-ttu-id="9b2c3-235">Det här jobbet använder följande frågedefinition för att identifiera telemetrimeddelanden som ska utlösa ett larm:</span><span class="sxs-lookup"><span data-stu-id="9b2c3-235">This job uses the following query definition to identify telemetry messages that should trigger an alarm:</span></span>

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
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

<span data-ttu-id="9b2c3-236">Jobbet skickar sina utdata till en händelsehubb för vidare bearbetning och sparar information om varje avisering i Blob Storage där lösningsportalen kan läsa aviseringarna.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-236">The job sends its output to an Event Hub for further processing and saves details of each alert to blob storage from where the solution portal can read the alert information.</span></span>

<span data-ttu-id="9b2c3-237">**Jobb 3: Telemetri** används på inkommande telemetriströmmar för enheten på två sätt.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-237">**Job 3: Telemetry** operates on the incoming device telemetry stream in two ways.</span></span> <span data-ttu-id="9b2c3-238">Med det första skickas alla telemetrimeddelanden från enheterna till permanent blobblagring för långsiktig lagring.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-238">The first sends all telemetry messages from the devices to persistent blob storage for long-term storage.</span></span> <span data-ttu-id="9b2c3-239">Med det andra beräknas värden för genomsnittlig, minsta och högsta fuktighet under en glidande femminutersperiod. Dessa data skickas sedan till blobblagring.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-239">The second computes average, minimum, and maximum humidity values over a five-minute sliding window and sends this data to blob storage.</span></span> <span data-ttu-id="9b2c3-240">Lösningsportalen läser av telemetridata från Blob Storage och fyller i diagrammen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-240">The solution portal reads the telemetry data from blob storage to populate the charts.</span></span> <span data-ttu-id="9b2c3-241">Det här jobbet använder följande frågedefinition:</span><span class="sxs-lookup"><span data-stu-id="9b2c3-241">This job uses the following query definition:</span></span>

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a><span data-ttu-id="9b2c3-242">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="9b2c3-242">Event Hubs</span></span>

<span data-ttu-id="9b2c3-243">ASA-jobben för **enhetsinformation** och **regler** skickar sina data till Event Hubs för bearbetning i **händelseprocessorn** som körs i webbjobbet.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-243">The **device info** and **rules** ASA jobs output their data to Event Hubs to reliably forward on to the **Event Processor** running in the WebJob.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="9b2c3-244">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="9b2c3-244">Azure storage</span></span>

<span data-ttu-id="9b2c3-245">Lösningen använder Azure-blobblagring för att bevara alla rådata och sammanfattade telemetridata från enheterna i lösningen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-245">The solution uses Azure blob storage to persist all the raw and summarized telemetry data from the devices in the solution.</span></span> <span data-ttu-id="9b2c3-246">Portalen läser av telemetridata från Blob Storage och fyller i diagrammen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-246">The portal reads the telemetry data from blob storage to populate the charts.</span></span> <span data-ttu-id="9b2c3-247">När aviseringar ska visas läser lösningsportalen av data från Blob Storage som registreras när telemetrivärden överskrider de konfigurerade tröskelvärdena.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-247">To display alerts, the solution portal reads the data from blob storage that records when telemetry values exceeded the configured threshold values.</span></span> <span data-ttu-id="9b2c3-248">I lösningen används också Blob Storage till att registrera de tröskelvärden du anger i lösningsportalen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-248">The solution also uses blob storage to record the threshold values you set in the solution portal.</span></span>

## <a name="webjobs"></a><span data-ttu-id="9b2c3-249">Webbjobb</span><span class="sxs-lookup"><span data-stu-id="9b2c3-249">WebJobs</span></span>

<span data-ttu-id="9b2c3-250">Förutom att fungera som värdar för enhetssimulatorerna fungerar WebJobs i lösningen även som värdar för **händelseprocessorn** som körs i ett Azure WebJob som hanterar svar från kommandon.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-250">In addition to hosting the device simulators, the WebJobs in the solution also host the **Event Processor** running in an Azure WebJob that handles command responses.</span></span> <span data-ttu-id="9b2c3-251">Svarsmeddelanden från kommandon används för att uppdatera enhetens kommandohistorik (lagras i Cosmos DB-databasen).</span><span class="sxs-lookup"><span data-stu-id="9b2c3-251">It uses command response messages to update the device command history (stored in the Cosmos DB database).</span></span>

## <a name="cosmos-db"></a><span data-ttu-id="9b2c3-252">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9b2c3-252">Cosmos DB</span></span>

<span data-ttu-id="9b2c3-253">Lösningen använder en Cosmos DB-databas för att lagra information om de enheter som är anslutna till lösningen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-253">The solution uses a Cosmos DB database to store information about the devices connected to the solution.</span></span> <span data-ttu-id="9b2c3-254">I den här informationen ingår historiken för de kommandon som skickas till enheter från lösningsportalen och för de metoder som anropas från lösningsportalen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-254">This information includes the history of commands sent to devices from the solution portal and of methods invoked from the solution portal.</span></span>

## <a name="solution-portal"></a><span data-ttu-id="9b2c3-255">Lösningsportal</span><span class="sxs-lookup"><span data-stu-id="9b2c3-255">Solution portal</span></span>

<span data-ttu-id="9b2c3-256">Lösningsportalen är en webbapp som ingår i distributionen av den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-256">The solution portal is a web app deployed as part of the preconfigured solution.</span></span> <span data-ttu-id="9b2c3-257">Några viktiga sidor i lösningsportalen är instrumentpanelen och enhetslistan.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-257">The key pages in the solution portal are the dashboard and the device list.</span></span>

### <a name="dashboard"></a><span data-ttu-id="9b2c3-258">Instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="9b2c3-258">Dashboard</span></span>

<span data-ttu-id="9b2c3-259">På den här sidan i webbappen används javascript-baserade PowerBI-kontroller (se [PowerBI-visuals-databasen](https://www.github.com/Microsoft/PowerBI-visuals)) till att visualisera telemetridata från enheterna.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-259">This page in the web app uses PowerBI javascript controls (See [PowerBI-visuals repo](https://www.github.com/Microsoft/PowerBI-visuals)) to visualize the telemetry data from the devices.</span></span> <span data-ttu-id="9b2c3-260">Lösningen använder ASA-telemetrijobbet för att skriva telemetridata till blobblagring.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-260">The solution uses the ASA telemetry job to write the telemetry data to blob storage.</span></span>

### <a name="device-list"></a><span data-ttu-id="9b2c3-261">Enhetslista</span><span class="sxs-lookup"><span data-stu-id="9b2c3-261">Device list</span></span>

<span data-ttu-id="9b2c3-262">På den här sidan i lösningsportalen kan du göra följande:</span><span class="sxs-lookup"><span data-stu-id="9b2c3-262">From this page in the solution portal you can:</span></span>

* <span data-ttu-id="9b2c3-263">Etablera en ny enhet.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-263">Provision a new device.</span></span> <span data-ttu-id="9b2c3-264">Den här åtgärden anger det unika enhets-ID:t och genererar autentiseringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-264">This action sets the unique device id and generates the authentication key.</span></span> <span data-ttu-id="9b2c3-265">Den skriver information om enheten till både IoT Hub-identitetsregistret och den lösningsspecifika Cosmos DB-databasen.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-265">It writes information about the device to both the IoT Hub identity registry and the solution-specific Cosmos DB database.</span></span>
* <span data-ttu-id="9b2c3-266">Hantera enhetsegenskaper.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-266">Manage device properties.</span></span> <span data-ttu-id="9b2c3-267">Den här åtgärden används för att visa befintliga egenskaper och uppdatera dem med nya egenskaper.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-267">This action includes viewing existing properties and updating with new properties.</span></span>
* <span data-ttu-id="9b2c3-268">Skicka kommandon till en enhet.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-268">Send commands to a device.</span></span>
* <span data-ttu-id="9b2c3-269">Visa kommandohistoriken för en enhet.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-269">View the command history for a device.</span></span>
* <span data-ttu-id="9b2c3-270">Aktivera och inaktivera enheter.</span><span class="sxs-lookup"><span data-stu-id="9b2c3-270">Enable and disable devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9b2c3-271">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9b2c3-271">Next steps</span></span>

<span data-ttu-id="9b2c3-272">Följande blogginlägg på TechNet innehåller mer information om den förkonfigurerade lösningen för fjärrövervakning:</span><span class="sxs-lookup"><span data-stu-id="9b2c3-272">The following TechNet blog posts provide more detail about the remote monitoring preconfigured solution:</span></span>

* [<span data-ttu-id="9b2c3-273">IoT Suite - Under The Hood - Remote Monitoring</span><span class="sxs-lookup"><span data-stu-id="9b2c3-273">IoT Suite - Under The Hood - Remote Monitoring</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
* [<span data-ttu-id="9b2c3-274">IoT Suite - Remote Monitoring - Adding Live and Simulated Devices</span><span class="sxs-lookup"><span data-stu-id="9b2c3-274">IoT Suite - Remote Monitoring - Adding Live and Simulated Devices</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

<span data-ttu-id="9b2c3-275">Läs följande artiklar om du vill fortsätta och lära dig mer om IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="9b2c3-275">You can continue getting started with IoT Suite by reading the following articles:</span></span>

* <span data-ttu-id="9b2c3-276">[Ansluta enheten till den förkonfigurerade fjärrövervakningslösningen][lnk-connect-rm]</span><span class="sxs-lookup"><span data-stu-id="9b2c3-276">[Connect your device to the remote monitoring preconfigured solution][lnk-connect-rm]</span></span>
* <span data-ttu-id="9b2c3-277">[Behörigheter på webbplatsen azureiotsuite.com][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="9b2c3-277">[Permissions on the azureiotsuite.com site][lnk-permissions]</span></span>

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twins]:  ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
