---
title: "aaaRemote övervakning förkonfigurerade lösningen genomgången | Microsoft Docs"
description: "En beskrivning av hello Azure IoT förkonfigurerade lösningen fjärråtkomst övervakning och dess arkitektur."
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
ms.openlocfilehash: 57a336bd94938c2b9ee5d3456ea8e45446cf3d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a><span data-ttu-id="dfad2-103">Genomgång av den förkonfigurerade lösningen för fjärrövervakning</span><span class="sxs-lookup"><span data-stu-id="dfad2-103">Remote monitoring preconfigured solution walkthrough</span></span>

<span data-ttu-id="dfad2-104">Hej IoT Suite fjärrövervaknings [förkonfigurerade lösningen] [ lnk-preconfigured-solutions] är en implementering av en slutpunkt till slutpunkt övervakningslösning för flera datorer som körs på fjärrplatser.</span><span class="sxs-lookup"><span data-stu-id="dfad2-104">hello IoT Suite remote monitoring [preconfigured solution][lnk-preconfigured-solutions] is an implementation of an end-to-end monitoring solution for multiple machines running in remote locations.</span></span> <span data-ttu-id="dfad2-105">hello lösningen kombinerar Azure nyckeltjänster tooprovide en allmänna implementering av hello affärsscenario.</span><span class="sxs-lookup"><span data-stu-id="dfad2-105">hello solution combines key Azure services tooprovide a generic implementation of hello business scenario.</span></span> <span data-ttu-id="dfad2-106">Du kan använda hello lösning som en startpunkt för din egen implementering och [anpassa] [ lnk-customize] den toomeet egna specifika krav i företaget.</span><span class="sxs-lookup"><span data-stu-id="dfad2-106">You can use hello solution as a starting point for your own implementation and [customize][lnk-customize] it toomeet your own specific business requirements.</span></span>

<span data-ttu-id="dfad2-107">Den här artikeln guidar dig igenom några av hello viktiga delar i hello fjärråtkomst övervakning lösning tooenable toounderstand hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="dfad2-107">This article walks you through some of hello key elements of hello remote monitoring solution tooenable you toounderstand how it works.</span></span> <span data-ttu-id="dfad2-108">Med den här kunskapen kan du sedan:</span><span class="sxs-lookup"><span data-stu-id="dfad2-108">This knowledge helps you to:</span></span>

* <span data-ttu-id="dfad2-109">Felsöka problem i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="dfad2-109">Troubleshoot issues in hello solution.</span></span>
* <span data-ttu-id="dfad2-110">Planera hur toocustomize toohello lösning toomeet egna specifika krav.</span><span class="sxs-lookup"><span data-stu-id="dfad2-110">Plan how toocustomize toohello solution toomeet your own specific requirements.</span></span> 
* <span data-ttu-id="dfad2-111">Utforma en egen IoT-lösning som använder Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="dfad2-111">Design your own IoT solution that uses Azure services.</span></span>

## <a name="logical-architecture"></a><span data-ttu-id="dfad2-112">Logisk arkitektur</span><span class="sxs-lookup"><span data-stu-id="dfad2-112">Logical architecture</span></span>

<span data-ttu-id="dfad2-113">följande diagram hello beskrivs hello logiska komponenter av hello förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="dfad2-113">hello following diagram outlines hello logical components of hello preconfigured solution:</span></span>

![Logisk arkitektur](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="simulated-devices"></a><span data-ttu-id="dfad2-115">Simulerade enheter</span><span class="sxs-lookup"><span data-stu-id="dfad2-115">Simulated devices</span></span>

<span data-ttu-id="dfad2-116">I hello förkonfigurerade lösningen representerar hello simulerade enhet en kylningsenhet (till exempel en byggnad luftkonditionering eller anläggning luften hanteringsenhet).</span><span class="sxs-lookup"><span data-stu-id="dfad2-116">In hello preconfigured solution, hello simulated device represents a cooling device (such as a building air conditioner or facility air handling unit).</span></span> <span data-ttu-id="dfad2-117">När du distribuerar hello förkonfigurerade lösningen kan du också automatiskt etablera fyra simulerade enheter som kör i en [Azure Webjobs][lnk-webjobs].</span><span class="sxs-lookup"><span data-stu-id="dfad2-117">When you deploy hello preconfigured solution, you also automatically provision four simulated devices that run in an [Azure WebJob][lnk-webjobs].</span></span> <span data-ttu-id="dfad2-118">hello simulerade enheter gör det lättare för du tooexplore hello beteendet för hello lösning utan hello måste toodeploy alla fysiska enheter.</span><span class="sxs-lookup"><span data-stu-id="dfad2-118">hello simulated devices make it easy for you tooexplore hello behavior of hello solution without hello need toodeploy any physical devices.</span></span> <span data-ttu-id="dfad2-119">toodeploy en riktigt fysisk enhet finns hello [ansluta din enhet toohello remote förkonfigurerade övervakningslösning] [ lnk-connect-rm] kursen.</span><span class="sxs-lookup"><span data-stu-id="dfad2-119">toodeploy a real physical device, see hello [Connect your device toohello remote monitoring preconfigured solution][lnk-connect-rm] tutorial.</span></span>

### <a name="device-to-cloud-messages"></a><span data-ttu-id="dfad2-120">Meddelanden från enheten till molnet</span><span class="sxs-lookup"><span data-stu-id="dfad2-120">Device-to-cloud messages</span></span>

<span data-ttu-id="dfad2-121">Varje simulerade enhet kan skicka hello följande meddelande typer tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="dfad2-121">Each simulated device can send hello following message types tooIoT Hub:</span></span>

| <span data-ttu-id="dfad2-122">Meddelande</span><span class="sxs-lookup"><span data-stu-id="dfad2-122">Message</span></span> | <span data-ttu-id="dfad2-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dfad2-123">Description</span></span> |
| --- | --- |
| <span data-ttu-id="dfad2-124">Start</span><span class="sxs-lookup"><span data-stu-id="dfad2-124">Startup</span></span> |<span data-ttu-id="dfad2-125">När hello enheten startar skickar den ett **enhetsinformation** meddelande som innehåller information om själva toohello serverdel.</span><span class="sxs-lookup"><span data-stu-id="dfad2-125">When hello device starts, it sends a **device-info** message containing information about itself toohello back end.</span></span> <span data-ttu-id="dfad2-126">Dessa data innehåller hello enhets-id och en lista över hello kommandon och metoder hello enhet stöder.</span><span class="sxs-lookup"><span data-stu-id="dfad2-126">This data includes hello device id and a list of hello commands and methods hello device supports.</span></span> |
| <span data-ttu-id="dfad2-127">Närvaro</span><span class="sxs-lookup"><span data-stu-id="dfad2-127">Presence</span></span> |<span data-ttu-id="dfad2-128">En enhet skickar regelbundet en **förekomst** meddelande tooreport om hello förekomsten av en sensor känner av hello enhet.</span><span class="sxs-lookup"><span data-stu-id="dfad2-128">A device periodically sends a **presence** message tooreport whether hello device can sense hello presence of a sensor.</span></span> |
| <span data-ttu-id="dfad2-129">Telemetri</span><span class="sxs-lookup"><span data-stu-id="dfad2-129">Telemetry</span></span> |<span data-ttu-id="dfad2-130">En enhet skickar regelbundet en **telemetri** meddelande som rapporterar simulerade värden för hello temperatur- och fuktighetskonsekvens samlas in från hello enhet har simulerade sensorer.</span><span class="sxs-lookup"><span data-stu-id="dfad2-130">A device periodically sends a **telemetry** message that reports simulated values for hello temperature and humidity collected from hello device's simulated sensors.</span></span> |

> [!NOTE]
> <span data-ttu-id="dfad2-131">hello lösningen lagrar hello lista med kommandon som stöds av hello-enhet i en Cosmos-DB-databas och inte i hello enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="dfad2-131">hello solution stores hello list of commands supported by hello device in a Cosmos DB database and not in hello device twin.</span></span>

### <a name="properties-and-device-twins"></a><span data-ttu-id="dfad2-132">Egenskaper och enhetstvillingar</span><span class="sxs-lookup"><span data-stu-id="dfad2-132">Properties and device twins</span></span>

<span data-ttu-id="dfad2-133">hello simulerade enheterna skickar hello följande enhet egenskaper toohello [dubbla] [ lnk-device-twins] i hello IoT-hubb som *rapporterade egenskaper*.</span><span class="sxs-lookup"><span data-stu-id="dfad2-133">hello simulated devices send hello following device properties toohello [twin][lnk-device-twins] in hello IoT hub as *reported properties*.</span></span> <span data-ttu-id="dfad2-134">hello enheten skickar rapporterade egenskaper vid start och svar tooa **ändra enhetens tillstånd** kommando eller metod.</span><span class="sxs-lookup"><span data-stu-id="dfad2-134">hello device sends reported properties at startup and in response tooa **Change Device State** command or method.</span></span>

| <span data-ttu-id="dfad2-135">Egenskap</span><span class="sxs-lookup"><span data-stu-id="dfad2-135">Property</span></span> | <span data-ttu-id="dfad2-136">Syfte</span><span class="sxs-lookup"><span data-stu-id="dfad2-136">Purpose</span></span> |
| --- | --- |
| <span data-ttu-id="dfad2-137">Config.TelemetryInterval</span><span class="sxs-lookup"><span data-stu-id="dfad2-137">Config.TelemetryInterval</span></span> | <span data-ttu-id="dfad2-138">Frekvens (sekunder) hello enheten skickar telemetri</span><span class="sxs-lookup"><span data-stu-id="dfad2-138">Frequency (seconds) hello device sends telemetry</span></span> |
| <span data-ttu-id="dfad2-139">Config.TemperatureMeanValue</span><span class="sxs-lookup"><span data-stu-id="dfad2-139">Config.TemperatureMeanValue</span></span> | <span data-ttu-id="dfad2-140">Anger hello medelvärdet för hello simulerade temperatur telemetri</span><span class="sxs-lookup"><span data-stu-id="dfad2-140">Specifies hello mean value for hello simulated temperature telemetry</span></span> |
| <span data-ttu-id="dfad2-141">Device.DeviceID</span><span class="sxs-lookup"><span data-stu-id="dfad2-141">Device.DeviceID</span></span> |<span data-ttu-id="dfad2-142">ID som är angivna eller tilldelas när en enhet skapas i hello-lösning</span><span class="sxs-lookup"><span data-stu-id="dfad2-142">Id that is either provided or assigned when a device is created in hello solution</span></span> |
| <span data-ttu-id="dfad2-143">Device.DeviceState</span><span class="sxs-lookup"><span data-stu-id="dfad2-143">Device.DeviceState</span></span> | <span data-ttu-id="dfad2-144">Tillstånd som rapporteras av hello-enhet</span><span class="sxs-lookup"><span data-stu-id="dfad2-144">State reported by hello device</span></span> |
| <span data-ttu-id="dfad2-145">Device.CreatedTime</span><span class="sxs-lookup"><span data-stu-id="dfad2-145">Device.CreatedTime</span></span> |<span data-ttu-id="dfad2-146">Tid hello enheten har skapats i hello-lösning</span><span class="sxs-lookup"><span data-stu-id="dfad2-146">Time hello device was created in hello solution</span></span> |
| <span data-ttu-id="dfad2-147">Device.StartupTime</span><span class="sxs-lookup"><span data-stu-id="dfad2-147">Device.StartupTime</span></span> |<span data-ttu-id="dfad2-148">Tid hello enheten startades</span><span class="sxs-lookup"><span data-stu-id="dfad2-148">Time hello device was started</span></span> |
| <span data-ttu-id="dfad2-149">Device.LastDesiredPropertyChange</span><span class="sxs-lookup"><span data-stu-id="dfad2-149">Device.LastDesiredPropertyChange</span></span> |<span data-ttu-id="dfad2-150">hello versionsnumret för senaste önskade hello-egenskapen ändras</span><span class="sxs-lookup"><span data-stu-id="dfad2-150">hello version number of hello last desired property change</span></span> |
| <span data-ttu-id="dfad2-151">Device.Location.Latitude</span><span class="sxs-lookup"><span data-stu-id="dfad2-151">Device.Location.Latitude</span></span> |<span data-ttu-id="dfad2-152">Latitud platsen för hello-enhet</span><span class="sxs-lookup"><span data-stu-id="dfad2-152">Latitude location of hello device</span></span> |
| <span data-ttu-id="dfad2-153">Device.Location.Longitude</span><span class="sxs-lookup"><span data-stu-id="dfad2-153">Device.Location.Longitude</span></span> |<span data-ttu-id="dfad2-154">Longitud platsen för hello-enhet</span><span class="sxs-lookup"><span data-stu-id="dfad2-154">Longitude location of hello device</span></span> |
| <span data-ttu-id="dfad2-155">System.Manufacturer</span><span class="sxs-lookup"><span data-stu-id="dfad2-155">System.Manufacturer</span></span> |<span data-ttu-id="dfad2-156">Enhetstillverkare</span><span class="sxs-lookup"><span data-stu-id="dfad2-156">Device manufacturer</span></span> |
| <span data-ttu-id="dfad2-157">System.ModelNumber</span><span class="sxs-lookup"><span data-stu-id="dfad2-157">System.ModelNumber</span></span> |<span data-ttu-id="dfad2-158">Modellnumret för hello-enhet</span><span class="sxs-lookup"><span data-stu-id="dfad2-158">Model number of hello device</span></span> |
| <span data-ttu-id="dfad2-159">System.SerialNumber</span><span class="sxs-lookup"><span data-stu-id="dfad2-159">System.SerialNumber</span></span> |<span data-ttu-id="dfad2-160">Serienumret för hello-enhet</span><span class="sxs-lookup"><span data-stu-id="dfad2-160">Serial number of hello device</span></span> |
| <span data-ttu-id="dfad2-161">System.FirmwareVersion</span><span class="sxs-lookup"><span data-stu-id="dfad2-161">System.FirmwareVersion</span></span> |<span data-ttu-id="dfad2-162">Aktuell version av den inbyggda programvaran på hello-enhet</span><span class="sxs-lookup"><span data-stu-id="dfad2-162">Current version of firmware on hello device</span></span> |
| <span data-ttu-id="dfad2-163">System.Platform</span><span class="sxs-lookup"><span data-stu-id="dfad2-163">System.Platform</span></span> |<span data-ttu-id="dfad2-164">Plattformsarkitektur av hello-enhet</span><span class="sxs-lookup"><span data-stu-id="dfad2-164">Platform architecture of hello device</span></span> |
| <span data-ttu-id="dfad2-165">System.Processor</span><span class="sxs-lookup"><span data-stu-id="dfad2-165">System.Processor</span></span> |<span data-ttu-id="dfad2-166">Processorn körs hello-enhet</span><span class="sxs-lookup"><span data-stu-id="dfad2-166">Processor running hello device</span></span> |
| <span data-ttu-id="dfad2-167">System.InstalledRAM</span><span class="sxs-lookup"><span data-stu-id="dfad2-167">System.InstalledRAM</span></span> |<span data-ttu-id="dfad2-168">Mängden RAM-minne på hello-enhet</span><span class="sxs-lookup"><span data-stu-id="dfad2-168">Amount of RAM installed on hello device</span></span> |

<span data-ttu-id="dfad2-169">hello simulator lägger dessa egenskaper i simulerade enheter med exempelvärden.</span><span class="sxs-lookup"><span data-stu-id="dfad2-169">hello simulator seeds these properties in simulated devices with sample values.</span></span> <span data-ttu-id="dfad2-170">Varje gång hello simulator initierar en simulerad enhet, hello enheten rapporterar hello fördefinierade metadata tooIoT hubb som rapporterades egenskaper.</span><span class="sxs-lookup"><span data-stu-id="dfad2-170">Each time hello simulator initializes a simulated device, hello device reports hello pre-defined metadata tooIoT Hub as reported properties.</span></span> <span data-ttu-id="dfad2-171">Rapporterat egenskaper kan bara uppdateras genom hello enhet.</span><span class="sxs-lookup"><span data-stu-id="dfad2-171">Reported properties can only be updated by hello device.</span></span> <span data-ttu-id="dfad2-172">toochange rapporterade egenskapen du egenskapen en önskad i lösningen portal.</span><span class="sxs-lookup"><span data-stu-id="dfad2-172">toochange a reported property, you set a desired property in solution portal.</span></span> <span data-ttu-id="dfad2-173">Det är hello ansvar hello enheten:</span><span class="sxs-lookup"><span data-stu-id="dfad2-173">It is hello responsibility of hello device to:</span></span>

1. <span data-ttu-id="dfad2-174">Med jämna mellanrum att hämta egenskaper från hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="dfad2-174">Periodically retrieve desired properties from hello IoT hub.</span></span>
2. <span data-ttu-id="dfad2-175">Uppdatera konfigurationen med hello önskad egenskapsvärde.</span><span class="sxs-lookup"><span data-stu-id="dfad2-175">Update its configuration with hello desired property value.</span></span>
3. <span data-ttu-id="dfad2-176">Skicka hello nya värdet tillbaka toohello navet som rapporterades egenskap.</span><span class="sxs-lookup"><span data-stu-id="dfad2-176">Send hello new value back toohello hub as a reported property.</span></span>

<span data-ttu-id="dfad2-177">Hello lösning instrumentpanel kan du använda *önskade egenskaper* tooset egenskaper på en enhet med hjälp av hello [enheten dubbla][lnk-device-twins].</span><span class="sxs-lookup"><span data-stu-id="dfad2-177">From hello solution dashboard, you can use *desired properties* tooset properties on a device by using hello [device twin][lnk-device-twins].</span></span> <span data-ttu-id="dfad2-178">En enhet läser vanligtvis ett önskade egenskapsvärde från hello hubb tooupdate dess interna tillståndet och rapporten hello ändra tillbaka som en egenskap för rapporterade.</span><span class="sxs-lookup"><span data-stu-id="dfad2-178">Typically, a device reads a desired property value from hello hub tooupdate its internal state and report hello change back as a reported property.</span></span>

> [!NOTE]
> <span data-ttu-id="dfad2-179">hello simulerade enheten koden endast använder hello **Desired.Config.TemperatureMeanValue** och **Desired.Config.TelemetryInterval** egenskaper tooupdate hello rapporterade egenskaper som skickas tillbaka tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="dfad2-179">hello simulated device code only uses hello **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties tooupdate hello reported properties sent back tooIoT Hub.</span></span> <span data-ttu-id="dfad2-180">Alla andra ändringsbegäranden för önskad egenskapen ignoreras i hello simulerade enheten.</span><span class="sxs-lookup"><span data-stu-id="dfad2-180">All other desired property change requests are ignored in hello simulated device.</span></span>

### <a name="methods"></a><span data-ttu-id="dfad2-181">Metoder</span><span class="sxs-lookup"><span data-stu-id="dfad2-181">Methods</span></span>

<span data-ttu-id="dfad2-182">hello simulerade enheter kan hantera hello följande metoder ([direkt metoder][lnk-direct-methods]) anropas från portalen för hello lösning hello IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="dfad2-182">hello simulated devices can handle hello following methods ([direct methods][lnk-direct-methods]) invoked from hello solution portal through hello IoT hub:</span></span>

| <span data-ttu-id="dfad2-183">Metod</span><span class="sxs-lookup"><span data-stu-id="dfad2-183">Method</span></span> | <span data-ttu-id="dfad2-184">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dfad2-184">Description</span></span> |
| --- | --- |
| <span data-ttu-id="dfad2-185">InitiateFirmwareUpdate</span><span class="sxs-lookup"><span data-stu-id="dfad2-185">InitiateFirmwareUpdate</span></span> |<span data-ttu-id="dfad2-186">Instruerar hello enheten tooperform en firmware-uppdatering</span><span class="sxs-lookup"><span data-stu-id="dfad2-186">Instructs hello device tooperform a firmware update</span></span> |
| <span data-ttu-id="dfad2-187">Starta om</span><span class="sxs-lookup"><span data-stu-id="dfad2-187">Reboot</span></span> |<span data-ttu-id="dfad2-188">Instruerar hello enheten tooreboot</span><span class="sxs-lookup"><span data-stu-id="dfad2-188">Instructs hello device tooreboot</span></span> |
| <span data-ttu-id="dfad2-189">FactoryReset</span><span class="sxs-lookup"><span data-stu-id="dfad2-189">FactoryReset</span></span> |<span data-ttu-id="dfad2-190">Instruerar hello enheten tooperform en fabriksåterställa</span><span class="sxs-lookup"><span data-stu-id="dfad2-190">Instructs hello device tooperform a factory reset</span></span> |

<span data-ttu-id="dfad2-191">Vissa metoder använda rapporterade egenskaper tooreport på pågår.</span><span class="sxs-lookup"><span data-stu-id="dfad2-191">Some methods use reported properties tooreport on progress.</span></span> <span data-ttu-id="dfad2-192">Till exempel hello **InitiateFirmwareUpdate** metoden simulerar körs hello update asynkront på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="dfad2-192">For example, hello **InitiateFirmwareUpdate** method simulates running hello update asynchronously on hello device.</span></span> <span data-ttu-id="dfad2-193">hello-metoden returnerar omedelbart på hello enhet medan hello asynkrona aktiviteten fortsätter toosend statusuppdateringar tillbaka toohello lösningen en instrumentpanel med hjälp av rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="dfad2-193">hello method returns immediately on hello device, while hello asynchronous task continues toosend status updates back toohello solution dashboard using reported properties.</span></span>

### <a name="commands"></a><span data-ttu-id="dfad2-194">Kommandon</span><span class="sxs-lookup"><span data-stu-id="dfad2-194">Commands</span></span>

<span data-ttu-id="dfad2-195">hello simulerade enheter kan hantera hello följande kommandon (moln till enhet meddelanden) som skickas från portalen för hello lösning hello IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="dfad2-195">hello simulated devices can handle hello following commands (cloud-to-device messages) sent from hello solution portal through hello IoT hub:</span></span>

| <span data-ttu-id="dfad2-196">Kommando</span><span class="sxs-lookup"><span data-stu-id="dfad2-196">Command</span></span> | <span data-ttu-id="dfad2-197">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dfad2-197">Description</span></span> |
| --- | --- |
| <span data-ttu-id="dfad2-198">PingDevice</span><span class="sxs-lookup"><span data-stu-id="dfad2-198">PingDevice</span></span> |<span data-ttu-id="dfad2-199">Skickar en *ping* toohello enhet toocheck den är aktiv</span><span class="sxs-lookup"><span data-stu-id="dfad2-199">Sends a *ping* toohello device toocheck it is alive</span></span> |
| <span data-ttu-id="dfad2-200">StartTelemetry</span><span class="sxs-lookup"><span data-stu-id="dfad2-200">StartTelemetry</span></span> |<span data-ttu-id="dfad2-201">Startar hello enhet som skickar telemetri</span><span class="sxs-lookup"><span data-stu-id="dfad2-201">Starts hello device sending telemetry</span></span> |
| <span data-ttu-id="dfad2-202">StopTelemetry</span><span class="sxs-lookup"><span data-stu-id="dfad2-202">StopTelemetry</span></span> |<span data-ttu-id="dfad2-203">Stoppar hello enheten från att skicka telemetri</span><span class="sxs-lookup"><span data-stu-id="dfad2-203">Stops hello device from sending telemetry</span></span> |
| <span data-ttu-id="dfad2-204">ChangeSetPointTemp</span><span class="sxs-lookup"><span data-stu-id="dfad2-204">ChangeSetPointTemp</span></span> |<span data-ttu-id="dfad2-205">Ändringar hello set punktvärdet runt vilken hello slumpmässiga data som genereras</span><span class="sxs-lookup"><span data-stu-id="dfad2-205">Changes hello set point value around which hello random data is generated</span></span> |
| <span data-ttu-id="dfad2-206">DiagnosticTelemetry</span><span class="sxs-lookup"><span data-stu-id="dfad2-206">DiagnosticTelemetry</span></span> |<span data-ttu-id="dfad2-207">Utlösare hello enheten simulatorn toosend ett ytterligare telemetri värde (externalTemp)</span><span class="sxs-lookup"><span data-stu-id="dfad2-207">Triggers hello device simulator toosend an additional telemetry value (externalTemp)</span></span> |
| <span data-ttu-id="dfad2-208">ChangeDeviceState</span><span class="sxs-lookup"><span data-stu-id="dfad2-208">ChangeDeviceState</span></span> |<span data-ttu-id="dfad2-209">Ändrar en egenskap för utökat läge för hello enhet och skickar hälsningsmeddelande enheten information från hello-enhet</span><span class="sxs-lookup"><span data-stu-id="dfad2-209">Changes an extended state property for hello device and sends hello device info message from hello device</span></span> |

> [!NOTE]
> <span data-ttu-id="dfad2-210">En jämförelse av dessa kommandon (meddelanden från molnet till enheten) och metoder (direkta metoder) finns i artikeln om [kommunikation mellan moln och enheter][lnk-c2d-guidance].</span><span class="sxs-lookup"><span data-stu-id="dfad2-210">For a comparison of these commands (cloud-to-device messages) and methods (direct methods), see [Cloud-to-device communications guidance][lnk-c2d-guidance].</span></span>

## <a name="iot-hub"></a><span data-ttu-id="dfad2-211">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="dfad2-211">IoT Hub</span></span>

<span data-ttu-id="dfad2-212">Hej [IoT-hubb] [ lnk-iothub] en data som skickas från hello enheter i hello moln och gör den tillgänglig toohello Azure Stream Analytics (ASA) jobb.</span><span class="sxs-lookup"><span data-stu-id="dfad2-212">hello [IoT hub][lnk-iothub] ingests data sent from hello devices into hello cloud and makes it available toohello Azure Stream Analytics (ASA) jobs.</span></span> <span data-ttu-id="dfad2-213">Varje stream ASA jobbet används en separat IoT-hubb konsumenten grupp tooread hello ström av meddelanden från dina enheter.</span><span class="sxs-lookup"><span data-stu-id="dfad2-213">Each stream ASA job uses a separate IoT Hub consumer group tooread hello stream of messages from your devices.</span></span>

<span data-ttu-id="dfad2-214">Hej IoT-hubb i hello-lösning också:</span><span class="sxs-lookup"><span data-stu-id="dfad2-214">hello IoT hub in hello solution also:</span></span>

- <span data-ttu-id="dfad2-215">Upprätthåller en identitetsregistret som lagrar hello-ID: n och autentiseringsnycklar för alla hello enheter tillåts tooconnect toohello portal.</span><span class="sxs-lookup"><span data-stu-id="dfad2-215">Maintains an identity registry that stores hello ids and authentication keys of all hello devices permitted tooconnect toohello portal.</span></span> <span data-ttu-id="dfad2-216">Du kan aktivera och inaktivera enheter via hello identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="dfad2-216">You can enable and disable devices through hello identity registry.</span></span>
- <span data-ttu-id="dfad2-217">Skickar kommandon tooyour enheter åt hello lösning portal.</span><span class="sxs-lookup"><span data-stu-id="dfad2-217">Sends commands tooyour devices on behalf of hello solution portal.</span></span>
- <span data-ttu-id="dfad2-218">Anropar metoder på dina enheter åt hello lösning portal.</span><span class="sxs-lookup"><span data-stu-id="dfad2-218">Invokes methods on your devices on behalf of hello solution portal.</span></span>
- <span data-ttu-id="dfad2-219">Att underhålla enhetstvillingar för alla registrerade enheter.</span><span class="sxs-lookup"><span data-stu-id="dfad2-219">Maintains device twins for all registered devices.</span></span> <span data-ttu-id="dfad2-220">En enhet dubbla lagrar hello egenskapsvärden som rapporterats av en enhet.</span><span class="sxs-lookup"><span data-stu-id="dfad2-220">A device twin stores hello property values reported by a device.</span></span> <span data-ttu-id="dfad2-221">En enhet dubbla lagrar också önskade egenskaper, ange hello lösning för i portalen, hello enheten tooretrieve vid nästa anslutning.</span><span class="sxs-lookup"><span data-stu-id="dfad2-221">A device twin also stores desired properties, set in hello solution portal, for hello device tooretrieve when it next connects.</span></span>
- <span data-ttu-id="dfad2-222">Scheman jobb tooset egenskaper för flera enheter eller anropa metoder i flera enheter.</span><span class="sxs-lookup"><span data-stu-id="dfad2-222">Schedules jobs tooset properties for multiple devices or invoke methods on multiple devices.</span></span>

## <a name="azure-stream-analytics"></a><span data-ttu-id="dfad2-223">Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="dfad2-223">Azure Stream Analytics</span></span>

<span data-ttu-id="dfad2-224">I hello remote övervakningslösning, [Azure Stream Analytics] [ lnk-asa] (ASA) skickar enheten har tagits emot av hello IoT-hubb tooother backend-komponenter för bearbetning eller lagring.</span><span class="sxs-lookup"><span data-stu-id="dfad2-224">In hello remote monitoring solution, [Azure Stream Analytics][lnk-asa] (ASA) dispatches device messages received by hello IoT hub tooother back-end components for processing or storage.</span></span> <span data-ttu-id="dfad2-225">Olika ASA jobb utför funktioner baserat på hälsningsmeddelande hello innehåll.</span><span class="sxs-lookup"><span data-stu-id="dfad2-225">Different ASA jobs perform specific functions based on hello content of hello messages.</span></span>

<span data-ttu-id="dfad2-226">**Jobbet 1: Enhetsinformationen** filtrerar informationsmeddelanden från hello inkommande meddelandeströmmen och skickar dem tooan Event Hub slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="dfad2-226">**Job 1: Device Info** filters device information messages from hello incoming message stream and sends them tooan Event Hub endpoint.</span></span> <span data-ttu-id="dfad2-227">En enhet skickar meddelanden med enheten vid start och svar tooa **SendDeviceInfo** kommando.</span><span class="sxs-lookup"><span data-stu-id="dfad2-227">A device sends device information messages at startup and in response tooa **SendDeviceInfo** command.</span></span> <span data-ttu-id="dfad2-228">Det här jobbet använder hello följande fråga definition tooidentify **enhetsinformation** meddelanden:</span><span class="sxs-lookup"><span data-stu-id="dfad2-228">This job uses hello following query definition tooidentify **device-info** messages:</span></span>

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

<span data-ttu-id="dfad2-229">Det här jobbet skickar dess utdata tooan Event Hub för vidare bearbetning.</span><span class="sxs-lookup"><span data-stu-id="dfad2-229">This job sends its output tooan Event Hub for further processing.</span></span>

<span data-ttu-id="dfad2-230">**Jobb 2: Regler** utvärderar inkommande telemetrivärden för temperatur och fuktighet mot tröskelvärdena för varje enhet.</span><span class="sxs-lookup"><span data-stu-id="dfad2-230">**Job 2: Rules** evaluates incoming temperature and humidity telemetry values against per-device thresholds.</span></span> <span data-ttu-id="dfad2-231">Tröskelvärdena är inställda i hello regler redigeraren i hello lösning användarportalen.</span><span class="sxs-lookup"><span data-stu-id="dfad2-231">Threshold values are set in hello rules editor available in hello solution portal.</span></span> <span data-ttu-id="dfad2-232">Varje enhet/värde-par lagras med en tidsstämpel i en blobb som Stream Analytics läser in som **referensdata**.</span><span class="sxs-lookup"><span data-stu-id="dfad2-232">Each device/value pair is stored by timestamp in a blob which Stream Analytics reads in as **Reference Data**.</span></span> <span data-ttu-id="dfad2-233">hello jobbet jämför ett tomt värde mot hello angiven tröskel för hello enhet.</span><span class="sxs-lookup"><span data-stu-id="dfad2-233">hello job compares any non-empty value against hello set threshold for hello device.</span></span> <span data-ttu-id="dfad2-234">Om den överskrider hello ' >' villkor hello jobbet matar ut en **larm** händelse som anger att hello tröskelvärde har överskridits och ger hello enhet, värde och tidsstämpelvärden.</span><span class="sxs-lookup"><span data-stu-id="dfad2-234">If it exceeds hello '>' condition, hello job outputs an **alarm** event that indicates that hello threshold is exceeded and provides hello device, value, and timestamp values.</span></span> <span data-ttu-id="dfad2-235">Det här jobbet använder följande definition tooidentify telemetri frågemeddelanden som ska utlösa ett larm hello:</span><span class="sxs-lookup"><span data-stu-id="dfad2-235">This job uses hello following query definition tooidentify telemetry messages that should trigger an alarm:</span></span>

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

<span data-ttu-id="dfad2-236">hello jobbet skickar dess utdata tooan Event Hub för vidare bearbetning och sparar informationen om varje avisering tooblob lagring från där hello lösning portal kan läsa hello aviseringsinformation.</span><span class="sxs-lookup"><span data-stu-id="dfad2-236">hello job sends its output tooan Event Hub for further processing and saves details of each alert tooblob storage from where hello solution portal can read hello alert information.</span></span>

<span data-ttu-id="dfad2-237">**Jobbet 3: Telemetri** fungerar på hello inkommande enheten telemetri ström på två sätt.</span><span class="sxs-lookup"><span data-stu-id="dfad2-237">**Job 3: Telemetry** operates on hello incoming device telemetry stream in two ways.</span></span> <span data-ttu-id="dfad2-238">hello skickar först alla telemetri meddelanden från hello enheter toopersistent blob storage för långsiktig lagring.</span><span class="sxs-lookup"><span data-stu-id="dfad2-238">hello first sends all telemetry messages from hello devices toopersistent blob storage for long-term storage.</span></span> <span data-ttu-id="dfad2-239">hello beräknar andra fuktighet genomsnittlig, lägsta och högsta värden över ett skjutfönster fem minuter långa och skickar den här tooblob datalagring.</span><span class="sxs-lookup"><span data-stu-id="dfad2-239">hello second computes average, minimum, and maximum humidity values over a five-minute sliding window and sends this data tooblob storage.</span></span> <span data-ttu-id="dfad2-240">hello lösning portal läser hello telemetridata från blob storage toopopulate hello diagram.</span><span class="sxs-lookup"><span data-stu-id="dfad2-240">hello solution portal reads hello telemetry data from blob storage toopopulate hello charts.</span></span> <span data-ttu-id="dfad2-241">Det här jobbet använder följande frågedefinitionen hello:</span><span class="sxs-lookup"><span data-stu-id="dfad2-241">This job uses hello following query definition:</span></span>

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

## <a name="event-hubs"></a><span data-ttu-id="dfad2-242">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="dfad2-242">Event Hubs</span></span>

<span data-ttu-id="dfad2-243">Hej **enhetsinformation** och **regler** ASA jobb utdata sina data tooEvent Hubs tooreliably framåt på toohello **Händelseprocessorn** körs i hello Webbjobb.</span><span class="sxs-lookup"><span data-stu-id="dfad2-243">hello **device info** and **rules** ASA jobs output their data tooEvent Hubs tooreliably forward on toohello **Event Processor** running in hello WebJob.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="dfad2-244">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="dfad2-244">Azure storage</span></span>

<span data-ttu-id="dfad2-245">hello lösningen använder Azure blob storage toopersist alla hello rådata och sammanfattade telemetridata från hello enheter i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="dfad2-245">hello solution uses Azure blob storage toopersist all hello raw and summarized telemetry data from hello devices in hello solution.</span></span> <span data-ttu-id="dfad2-246">hello portal läser hello telemetridata från blob storage toopopulate hello diagram.</span><span class="sxs-lookup"><span data-stu-id="dfad2-246">hello portal reads hello telemetry data from blob storage toopopulate hello charts.</span></span> <span data-ttu-id="dfad2-247">toodisplay aviseringar hello lösning portal läser hello data från blob storage som registrerar konfigurerades telemetri värden som överskrider hello tröskelvärden.</span><span class="sxs-lookup"><span data-stu-id="dfad2-247">toodisplay alerts, hello solution portal reads hello data from blob storage that records when telemetry values exceeded hello configured threshold values.</span></span> <span data-ttu-id="dfad2-248">hello lösningen använder också blob storage toorecord hello tröskelvärden du angett i hello lösning portal.</span><span class="sxs-lookup"><span data-stu-id="dfad2-248">hello solution also uses blob storage toorecord hello threshold values you set in hello solution portal.</span></span>

## <a name="webjobs"></a><span data-ttu-id="dfad2-249">Webbjobb</span><span class="sxs-lookup"><span data-stu-id="dfad2-249">WebJobs</span></span>

<span data-ttu-id="dfad2-250">Dessutom toohosting hello enheten simulatorer hello WebJobs i hello-lösning också värden hello **Händelseprocessorn** körs i en Azure-WebJob som hanterar svar för kommandot.</span><span class="sxs-lookup"><span data-stu-id="dfad2-250">In addition toohosting hello device simulators, hello WebJobs in hello solution also host hello **Event Processor** running in an Azure WebJob that handles command responses.</span></span> <span data-ttu-id="dfad2-251">Den använder kommandot svar meddelanden tooupdate hello kommandot enhetshistorik (som lagras i hello Cosmos-DB-databas).</span><span class="sxs-lookup"><span data-stu-id="dfad2-251">It uses command response messages tooupdate hello device command history (stored in hello Cosmos DB database).</span></span>

## <a name="cosmos-db"></a><span data-ttu-id="dfad2-252">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="dfad2-252">Cosmos DB</span></span>

<span data-ttu-id="dfad2-253">hello lösningen använder en Cosmos-DB toostore databasinformation om hello enheter anslutna toohello lösningen.</span><span class="sxs-lookup"><span data-stu-id="dfad2-253">hello solution uses a Cosmos DB database toostore information about hello devices connected toohello solution.</span></span> <span data-ttu-id="dfad2-254">Informationen omfattar hello historik av kommandon som skickas toodevices från hello lösning portal och metoder som anropas från hello lösning portal.</span><span class="sxs-lookup"><span data-stu-id="dfad2-254">This information includes hello history of commands sent toodevices from hello solution portal and of methods invoked from hello solution portal.</span></span>

## <a name="solution-portal"></a><span data-ttu-id="dfad2-255">Lösningsportal</span><span class="sxs-lookup"><span data-stu-id="dfad2-255">Solution portal</span></span>

<span data-ttu-id="dfad2-256">hello lösning portal är en webbapp som distribueras som en del av hello förkonfigurerade lösning.</span><span class="sxs-lookup"><span data-stu-id="dfad2-256">hello solution portal is a web app deployed as part of hello preconfigured solution.</span></span> <span data-ttu-id="dfad2-257">hello viktiga sidor i hello lösning portal är hello instrumentpanelen och hello enhetslistan.</span><span class="sxs-lookup"><span data-stu-id="dfad2-257">hello key pages in hello solution portal are hello dashboard and hello device list.</span></span>

### <a name="dashboard"></a><span data-ttu-id="dfad2-258">Instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="dfad2-258">Dashboard</span></span>

<span data-ttu-id="dfad2-259">Den här sidan i hello webbapp använder PowerBI javascript kontroller (se [PowerBI-visuell information lagringsplatsen](https://www.github.com/Microsoft/PowerBI-visuals)) toovisualize hello telemetridata från hello-enheter.</span><span class="sxs-lookup"><span data-stu-id="dfad2-259">This page in hello web app uses PowerBI javascript controls (See [PowerBI-visuals repo](https://www.github.com/Microsoft/PowerBI-visuals)) toovisualize hello telemetry data from hello devices.</span></span> <span data-ttu-id="dfad2-260">hello lösningen använder hello ASA telemetri jobbet toowrite hello telemetri tooblob datalagring.</span><span class="sxs-lookup"><span data-stu-id="dfad2-260">hello solution uses hello ASA telemetry job toowrite hello telemetry data tooblob storage.</span></span>

### <a name="device-list"></a><span data-ttu-id="dfad2-261">Enhetslista</span><span class="sxs-lookup"><span data-stu-id="dfad2-261">Device list</span></span>

<span data-ttu-id="dfad2-262">Den här sidan i hello lösning portal kan du:</span><span class="sxs-lookup"><span data-stu-id="dfad2-262">From this page in hello solution portal you can:</span></span>

* <span data-ttu-id="dfad2-263">Etablera en ny enhet.</span><span class="sxs-lookup"><span data-stu-id="dfad2-263">Provision a new device.</span></span> <span data-ttu-id="dfad2-264">Den här åtgärden anger hello unikt enhets-id och genererar hello autentiseringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="dfad2-264">This action sets hello unique device id and generates hello authentication key.</span></span> <span data-ttu-id="dfad2-265">Skriver information om hello enheten tooboth hello IoT-hubb identitetsregistret och hello Lösningsspecifika Cosmos-DB-databas.</span><span class="sxs-lookup"><span data-stu-id="dfad2-265">It writes information about hello device tooboth hello IoT Hub identity registry and hello solution-specific Cosmos DB database.</span></span>
* <span data-ttu-id="dfad2-266">Hantera enhetsegenskaper.</span><span class="sxs-lookup"><span data-stu-id="dfad2-266">Manage device properties.</span></span> <span data-ttu-id="dfad2-267">Den här åtgärden används för att visa befintliga egenskaper och uppdatera dem med nya egenskaper.</span><span class="sxs-lookup"><span data-stu-id="dfad2-267">This action includes viewing existing properties and updating with new properties.</span></span>
* <span data-ttu-id="dfad2-268">Skicka kommandon tooa enhet.</span><span class="sxs-lookup"><span data-stu-id="dfad2-268">Send commands tooa device.</span></span>
* <span data-ttu-id="dfad2-269">Visa historiken för hello-kommandot för en enhet.</span><span class="sxs-lookup"><span data-stu-id="dfad2-269">View hello command history for a device.</span></span>
* <span data-ttu-id="dfad2-270">Aktivera och inaktivera enheter.</span><span class="sxs-lookup"><span data-stu-id="dfad2-270">Enable and disable devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dfad2-271">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dfad2-271">Next steps</span></span>

<span data-ttu-id="dfad2-272">hello innehåller följande TechNet blogginlägg mer information om hello fjärråtkomst övervakning förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="dfad2-272">hello following TechNet blog posts provide more detail about hello remote monitoring preconfigured solution:</span></span>

* [<span data-ttu-id="dfad2-273">IoT Suite - Under hello huven - Fjärrövervaknings</span><span class="sxs-lookup"><span data-stu-id="dfad2-273">IoT Suite - Under hello Hood - Remote Monitoring</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
* [<span data-ttu-id="dfad2-274">IoT Suite - Remote Monitoring - Adding Live and Simulated Devices</span><span class="sxs-lookup"><span data-stu-id="dfad2-274">IoT Suite - Remote Monitoring - Adding Live and Simulated Devices</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

<span data-ttu-id="dfad2-275">Du kan fortsätta komma igång med IoT Suite genom att läsa hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="dfad2-275">You can continue getting started with IoT Suite by reading hello following articles:</span></span>

* <span data-ttu-id="dfad2-276">[Ansluta din enhet toohello remote förkonfigurerade övervakningslösning][lnk-connect-rm]</span><span class="sxs-lookup"><span data-stu-id="dfad2-276">[Connect your device toohello remote monitoring preconfigured solution][lnk-connect-rm]</span></span>
* <span data-ttu-id="dfad2-277">[Behörigheter för hello azureiotsuite.com plats][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="dfad2-277">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>

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
