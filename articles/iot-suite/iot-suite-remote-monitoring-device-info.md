---
title: "Enhetens information metadata i fjärranslutna övervakningslösning | Microsoft Docs"
description: "En beskrivning av den förkonfigurerade fjärrövervakningslösningen i Azure IoT och dess arkitektur."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1b334769-103b-4eb0-a293-184f3d1ba9a3
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: f8fd452806a0a0b98cf8e434c9bd55700083a6c5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="8d9a0-103">Enhetens information metadata i fjärråtkomst övervakning förkonfigurerade lösningen</span><span class="sxs-lookup"><span data-stu-id="8d9a0-103">Device information metadata in the remote monitoring preconfigured solution</span></span>

<span data-ttu-id="8d9a0-104">Azure IoT Suite remote förkonfigurerade övervakningslösning visar en metod för att hantera enhetsmetadata.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-104">The Azure IoT Suite remote monitoring preconfigured solution demonstrates an approach for managing device metadata.</span></span> <span data-ttu-id="8d9a0-105">Den här artikeln beskrivs en metod som den här lösningen använder att du ska förstå:</span><span class="sxs-lookup"><span data-stu-id="8d9a0-105">This article outlines the approach this solution takes to enable you to understand:</span></span>

* <span data-ttu-id="8d9a0-106">Vilka enhetens metadata lagras för lösningen.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-106">What device metadata the solution stores.</span></span>
* <span data-ttu-id="8d9a0-107">Hur hanterar lösningen enhetens metadata.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-107">How the solution manages the device metadata.</span></span>

## <a name="context"></a><span data-ttu-id="8d9a0-108">Kontext</span><span class="sxs-lookup"><span data-stu-id="8d9a0-108">Context</span></span>

<span data-ttu-id="8d9a0-109">Fjärråtkomst övervakning förkonfigurerade lösningen använder [Azure IoT Hub] [ lnk-iot-hub] att enheterna att skicka data till molnet.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-109">The remote monitoring preconfigured solution uses [Azure IoT Hub][lnk-iot-hub] to enable your devices to send data to the cloud.</span></span> <span data-ttu-id="8d9a0-110">Lösningen lagrar information om enheter på tre olika platser:</span><span class="sxs-lookup"><span data-stu-id="8d9a0-110">The solution stores information about devices in three different locations:</span></span>

| <span data-ttu-id="8d9a0-111">Plats</span><span class="sxs-lookup"><span data-stu-id="8d9a0-111">Location</span></span> | <span data-ttu-id="8d9a0-112">Information som lagras</span><span class="sxs-lookup"><span data-stu-id="8d9a0-112">Information stored</span></span> | <span data-ttu-id="8d9a0-113">Implementering</span><span class="sxs-lookup"><span data-stu-id="8d9a0-113">Implementation</span></span> |
| -------- | ------------------ | -------------- |
| <span data-ttu-id="8d9a0-114">Identitetsregistret</span><span class="sxs-lookup"><span data-stu-id="8d9a0-114">Identity registry</span></span> | <span data-ttu-id="8d9a0-115">Enhets-id, autentiseringsnycklar, aktiverade tillstånd</span><span class="sxs-lookup"><span data-stu-id="8d9a0-115">Device id, authentication keys, enabled state</span></span> | <span data-ttu-id="8d9a0-116">Inbyggd i IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="8d9a0-116">Built in to IoT Hub</span></span> |
| <span data-ttu-id="8d9a0-117">Enheten twins</span><span class="sxs-lookup"><span data-stu-id="8d9a0-117">Device twins</span></span> | <span data-ttu-id="8d9a0-118">Metadata: rapporterade egenskaper, önskade egenskaper, taggar</span><span class="sxs-lookup"><span data-stu-id="8d9a0-118">Metadata: reported properties, desired properties, tags</span></span> | <span data-ttu-id="8d9a0-119">Inbyggd i IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="8d9a0-119">Built in to IoT Hub</span></span> |
| <span data-ttu-id="8d9a0-120">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8d9a0-120">Cosmos DB</span></span> | <span data-ttu-id="8d9a0-121">Kommandot och metoden historik</span><span class="sxs-lookup"><span data-stu-id="8d9a0-121">Command and method history</span></span> | <span data-ttu-id="8d9a0-122">Anpassad för lösning</span><span class="sxs-lookup"><span data-stu-id="8d9a0-122">Custom for solution</span></span> |

<span data-ttu-id="8d9a0-123">IoT-hubb innehåller en [enhetsidentitetsregistret] [ lnk-identity-registry] att hantera åtkomst till en IoT-hubb och använder [enhet twins] [ lnk-device-twin] att hantera enhetsmetadata.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-123">IoT Hub includes a [device identity registry][lnk-identity-registry] to manage access to an IoT hub and uses [device twins][lnk-device-twin] to manage device metadata.</span></span> <span data-ttu-id="8d9a0-124">Det finns också en fjärransluten övervakning Lösningsspecifika *enhetsregistret* som lagrar kommandot och metoden historik.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-124">There is also a remote monitoring solution-specific *device registry* that stores command and method history.</span></span> <span data-ttu-id="8d9a0-125">Fjärråtkomst övervakning lösningen använder en [Cosmos DB] [ lnk-docdb] databasen för att implementera ett eget Arkiv för kommandot och metoden historik.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-125">The remote monitoring solution uses a [Cosmos DB][lnk-docdb] database to implement a custom store for command and method history.</span></span>

> [!NOTE]
> <span data-ttu-id="8d9a0-126">Fjärråtkomst övervakning förkonfigurerade lösningen behåller enhetsidentitetsregistret synkroniserad med informationen i Cosmos-DB-databas.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-126">The remote monitoring preconfigured solution keeps the device identity registry in sync with the information in the Cosmos DB database.</span></span> <span data-ttu-id="8d9a0-127">Både kan använda samma enhets-id för att unikt identifiera varje enhet som är ansluten till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-127">Both use the same device id to uniquely identify each device connected to your IoT hub.</span></span>

## <a name="device-metadata"></a><span data-ttu-id="8d9a0-128">Enhetens metadata</span><span class="sxs-lookup"><span data-stu-id="8d9a0-128">Device metadata</span></span>

<span data-ttu-id="8d9a0-129">IoT-hubb upprätthåller en [enheten dubbla] [ lnk-device-twin] för varje simulerade och fysisk enhet som är ansluten till en fjärransluten övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-129">IoT Hub maintains a [device twin][lnk-device-twin] for each simulated and physical device connected to a remote monitoring solution.</span></span> <span data-ttu-id="8d9a0-130">Lösningen använder enheten twins för att hantera metadata som associeras med enheter.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-130">The solution uses device twins to manage the metadata associated with devices.</span></span> <span data-ttu-id="8d9a0-131">Lösningen använder API: et för IoT-hubb kan interagera med enheten twins dubbla en enhet är en JSON-dokumentet som underhålls av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-131">A device twin is a JSON document maintained by IoT Hub, and the solution uses the IoT Hub API to interact with device twins.</span></span>

<span data-ttu-id="8d9a0-132">En enhet dubbla lagrar tre typer av metadata:</span><span class="sxs-lookup"><span data-stu-id="8d9a0-132">A device twin stores three types of metadata:</span></span>

- <span data-ttu-id="8d9a0-133">*Rapporterade egenskaper* skickas till en IoT-hubb av en enhet.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-133">*Reported properties* are sent to an IoT hub by a device.</span></span> <span data-ttu-id="8d9a0-134">I den fjärranslutna övervakningslösning simulerade enheterna skickar rapporterade egenskaper vid start och som svar på **ändra enhetsstatus** kommandon och metoder.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-134">In the remote monitoring solution, simulated devices send reported properties at start-up and in response to **Change device state** commands and methods.</span></span> <span data-ttu-id="8d9a0-135">Du kan visa rapporterade egenskaper i den **enhetslistan** och **enhetsinformation** i lösningen portal.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-135">You can view reported properties in the **Device list** and **Device details** in the solution portal.</span></span> <span data-ttu-id="8d9a0-136">Rapporterat egenskaper är skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-136">Reported properties are read only.</span></span>
- <span data-ttu-id="8d9a0-137">*Egenskaper för Desired* hämtas från IoT-hubben av enheter.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-137">*Desired properties* are retrieved from the IoT hub by devices.</span></span> <span data-ttu-id="8d9a0-138">Ansvarar för enheten för att ge alla nödvändiga konfigurationsändringen på enheten.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-138">It is the responsibility of the device to make any necessary configuration change on the device.</span></span> <span data-ttu-id="8d9a0-139">Det är också ansvar för enheten att rapportera ändringen till hubben som rapporterades egenskap.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-139">It is also the responsibility of the device to report the change back to the hub as a reported property.</span></span> <span data-ttu-id="8d9a0-140">Du kan ange en önskad egenskapsvärdet via portalen lösning.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-140">You can set a desired property value through the solution portal.</span></span>
- <span data-ttu-id="8d9a0-141">*Taggar* finns bara i enheten dubbla och aldrig synkroniseras med en enhet.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-141">*Tags* only exist in the device twin and are never synchronized with a device.</span></span> <span data-ttu-id="8d9a0-142">Du kan ange värden i lösningen portal och använda dem när du filtrerar listan över enheter.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-142">You can set tag values in the solution portal and use them when you filter the list of devices.</span></span> <span data-ttu-id="8d9a0-143">Lösningen använder också en tagg för att identifiera ikonen som ska visas för en enhet i lösningen portal.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-143">The solution also uses a tag to identify the icon to display for a device in the solution portal.</span></span>

<span data-ttu-id="8d9a0-144">Exempel rapporterade egenskaper från de simulerade enheterna innehåller tillverkare, modellnummer, latitud och longitud.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-144">Example reported properties from the simulated devices include manufacturer, model number, latitude, and longitude.</span></span> <span data-ttu-id="8d9a0-145">Simulerade enheter returnerar listan över metoder som stöds som en rapporterade egenskap.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-145">Simulated devices also return the list of supported methods as a reported property.</span></span>

> [!NOTE]
> <span data-ttu-id="8d9a0-146">I koden för den simulerade enheten är det bara de önskade egenskaperna **Desired.Config.TemperatureMeanValue** och **Desired.Config.TelemetryInterval** som används till att uppdatera de rapporterade egenskaper som skickas tillbaka till IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-146">The simulated device code only uses the **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties to update the reported properties sent back to IoT Hub.</span></span> <span data-ttu-id="8d9a0-147">Alla andra ändringsbegäranden för önskad egenskapen ignoreras.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-147">All other desired property change requests are ignored.</span></span>

<span data-ttu-id="8d9a0-148">En enhet information metadata JSON-dokumentet lagras i enheten registret Cosmos-DB-databasen har följande struktur:</span><span class="sxs-lookup"><span data-stu-id="8d9a0-148">A device information metadata JSON document stored in the device registry Cosmos DB database has the following structure:</span></span>

```json
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

> [!NOTE]
> <span data-ttu-id="8d9a0-149">Information om en enhet kan också innehålla metadata för att beskriva telemetri enheten skickar till IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-149">Device information can also include metadata to describe the telemetry the device sends to IoT Hub.</span></span> <span data-ttu-id="8d9a0-150">Fjärråtkomst övervakningslösning använder telemetri metadata för att anpassa hur instrumentpanelen visar [dynamiska telemetri][lnk-dynamic-telemetry].</span><span class="sxs-lookup"><span data-stu-id="8d9a0-150">The remote monitoring solution uses this telemetry metadata to customize how the dashboard displays [dynamic telemetry][lnk-dynamic-telemetry].</span></span>

## <a name="lifecycle"></a><span data-ttu-id="8d9a0-151">Livscykel</span><span class="sxs-lookup"><span data-stu-id="8d9a0-151">Lifecycle</span></span>

<span data-ttu-id="8d9a0-152">När du först skapa en enhet i lösningen portal skapas en post i Cosmos-DB-databas för lagring av kommandot och metoden historik i lösningen.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-152">When you first create a device in the solution portal, the solution creates an entry in the Cosmos DB database to store command and method history.</span></span> <span data-ttu-id="8d9a0-153">Lösningen skapas nu också en post för enheten i enhetsidentitetsregistret, vilket genererar nycklar som enheten använder för att autentisera med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-153">At this point, the solution also creates an entry for the device in the device identity registry, which generates the keys the device uses to authenticate with IoT Hub.</span></span> <span data-ttu-id="8d9a0-154">Dessutom skapas en delad enhet.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-154">It also creates a device twin.</span></span>

<span data-ttu-id="8d9a0-155">När en enhet först ansluter till lösningen skickar rapporterade egenskaper och informationsmeddelande för enheten.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-155">When a device first connects to the solution, it sends reported properties and a device information message.</span></span> <span data-ttu-id="8d9a0-156">Rapporterat egenskapsvärden sparas automatiskt i dubbla för enheten.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-156">The reported property values are automatically saved in the device twin.</span></span> <span data-ttu-id="8d9a0-157">Rapporterat egenskaper innehåller enhetens tillverkare, modellnummer, serienummer och en lista över metoder som stöds.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-157">The reported properties include the device manufacturer, model number, serial number, and a list of supported methods.</span></span> <span data-ttu-id="8d9a0-158">Enheten information meddelandet innehåller listan med kommandon som enheten stöder inklusive information om några parametrar.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-158">The device information message includes the list of the commands the device supports including information about any command parameters.</span></span> <span data-ttu-id="8d9a0-159">När lösningen får det här meddelandet, uppdaterar enhetsinformation i Cosmos-DB-databasen.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-159">When the solution receives this message, it updates the device information in the Cosmos DB database.</span></span>

### <a name="view-and-edit-device-information-in-the-solution-portal"></a><span data-ttu-id="8d9a0-160">Visa och redigera enhetsinformation i lösningen portal</span><span class="sxs-lookup"><span data-stu-id="8d9a0-160">View and edit device information in the solution portal</span></span>

<span data-ttu-id="8d9a0-161">Listan över enheter i lösningen portal visas följande egenskaper för enheter som kolumner som standard: **Status**, **DeviceId**, **tillverkare**, **modellnummer**, **serienummer**, **Firmware**, **plattform**, **Processor**, och **installerade RAM**.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-161">The device list in the solution portal displays the following device properties as columns by default: **Status**, **DeviceId**, **Manufacturer**, **Model Number**, **Serial Number**, **Firmware**, **Platform**, **Processor**, and **Installed RAM**.</span></span> <span data-ttu-id="8d9a0-162">Du kan anpassa kolumnerna genom att klicka på **kolumnen editor**.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-162">You can customize the columns by clicking **Column editor**.</span></span> <span data-ttu-id="8d9a0-163">Egenskaper för enhet **latitud** och **longitud** enhet på plats i Bing Map på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-163">The device properties **Latitude** and **Longitude** drive the location in the Bing Map on the dashboard.</span></span>

![Kolumnen redigeraren i listan över enheter][img-device-list]

<span data-ttu-id="8d9a0-165">I den **enhetsinformation** rutan i portalen lösning kan du redigera egenskaper och taggar (rapporterat egenskaper är skrivskyddade).</span><span class="sxs-lookup"><span data-stu-id="8d9a0-165">In the **Device Details** pane in the solution portal, you can edit desired properties and tags (reported properties are read only).</span></span>

![Informationspenelen][img-device-edit]

<span data-ttu-id="8d9a0-167">Du kan använda portalen lösning för att ta bort en enhet från din lösning.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-167">You can use the solution portal to remove a device from your solution.</span></span> <span data-ttu-id="8d9a0-168">När du tar bort en enhet lösningen enhet-posten tas bort från identitetsregistret och sedan tar bort enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-168">When you remove a device, the solution removes the device entry from identity registry and then deletes the device twin.</span></span> <span data-ttu-id="8d9a0-169">Lösningen tar också bort information som rör enheten från Cosmos-DB-databasen.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-169">The solution also removes information related to the device from the Cosmos DB database.</span></span> <span data-ttu-id="8d9a0-170">Innan du tar bort en enhet, måste du inaktivera den.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-170">Before you can remove a device, you must disable it.</span></span>

![Ta bort enheten][img-device-remove]

## <a name="device-information-message-processing"></a><span data-ttu-id="8d9a0-172">Enheten information meddelandebehandling</span><span class="sxs-lookup"><span data-stu-id="8d9a0-172">Device information message processing</span></span>

<span data-ttu-id="8d9a0-173">Enheten informationsmeddelanden som skickats av en enhet skiljer sig från telemetri meddelanden.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-173">Device information messages sent by a device are distinct from telemetry messages.</span></span> <span data-ttu-id="8d9a0-174">Enheten informationsmeddelanden innehåller de kommandon som en enhet kan svara på och eventuella tidigare kommandon.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-174">Device information messages include the commands a device can respond to, and any command history.</span></span> <span data-ttu-id="8d9a0-175">IoT-hubb själva känner inte till metadata i ett meddelande för enheten information och bearbetar meddelandet på samma sätt som den bearbetar alla meddelanden enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-175">IoT Hub itself has no knowledge of the metadata contained in a device information message and processes the message in the same way it processes any device-to-cloud message.</span></span> <span data-ttu-id="8d9a0-176">I den fjärranslutna övervakningslösning en [Azure Stream Analytics] [ lnk-stream-analytics] (ASA) jobbet läser meddelanden från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-176">In the remote monitoring solution, an [Azure Stream Analytics][lnk-stream-analytics] (ASA) job reads the messages from IoT Hub.</span></span> <span data-ttu-id="8d9a0-177">Den **DeviceInfo** stream analytics-jobbet filter för meddelanden som innehåller **”ObjectType”: ”DeviceInfo”** och vidarebefordrar dem till den **EventProcessorHost** värdinstans som körs i ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-177">The **DeviceInfo** stream analytics job filters for messages that contain **"ObjectType": "DeviceInfo"** and forwards them to the **EventProcessorHost** host instance that runs in a web job.</span></span> <span data-ttu-id="8d9a0-178">Logiken i den **EventProcessorHost** instans använder enhets-id för att hitta Cosmos-DB-post för enheten och uppdatera posten.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-178">Logic in the **EventProcessorHost** instance uses the device id to find the Cosmos DB record for the specific device and update the record.</span></span>

> [!NOTE]
> <span data-ttu-id="8d9a0-179">En enhet information meddelandet är en standard enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-179">A device information message is a standard device-to-cloud message.</span></span> <span data-ttu-id="8d9a0-180">Lösningen skiljer enheten informationsmeddelanden och telemetri meddelanden med hjälp av ASA frågor.</span><span class="sxs-lookup"><span data-stu-id="8d9a0-180">The solution distinguishes between device information messages and telemetry messages by using ASA queries.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d9a0-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d9a0-181">Next steps</span></span>

<span data-ttu-id="8d9a0-182">Du kan utforska några av de andra funktionerna och funktioner i IoT Suite förkonfigurerade lösningar nu du är klar med att lära dig hur du kan anpassa förkonfigurerade lösningar:</span><span class="sxs-lookup"><span data-stu-id="8d9a0-182">Now you've finished learning how you can customize the preconfigured solutions, you can explore some of the other features and capabilities of the IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="8d9a0-183">[Förutsägande Underhåll förkonfigurerade lösning: översikt][lnk-predictive-overview]</span><span class="sxs-lookup"><span data-stu-id="8d9a0-183">[Predictive maintenance preconfigured solution overview][lnk-predictive-overview]</span></span>
* <span data-ttu-id="8d9a0-184">[Vanliga frågor och svar om IoT Suite][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="8d9a0-184">[Frequently asked questions for IoT Suite][lnk-faq]</span></span>
* <span data-ttu-id="8d9a0-185">[IoT-säkerhet från grunden][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="8d9a0-185">[IoT security from the ground up][lnk-security-groundup]</span></span>

<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
