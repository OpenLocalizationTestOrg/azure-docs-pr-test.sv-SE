---
title: "aaaDevice information metadata i hello remote övervakningslösning | Microsoft Docs"
description: "En beskrivning av hello Azure IoT förkonfigurerade lösningen fjärråtkomst övervakning och dess arkitektur."
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
ms.openlocfilehash: 8387b98b8b2ae4934b0c900bc4df37dc17337c60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="device-information-metadata-in-hello-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="0b8c0-103">Enhetens information metadata i hello remote förkonfigurerade övervakningslösning</span><span class="sxs-lookup"><span data-stu-id="0b8c0-103">Device information metadata in hello remote monitoring preconfigured solution</span></span>

<span data-ttu-id="0b8c0-104">hello Azure IoT Suite remote förkonfigurerade övervakningslösning visar en metod för att hantera enhetsmetadata.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-104">hello Azure IoT Suite remote monitoring preconfigured solution demonstrates an approach for managing device metadata.</span></span> <span data-ttu-id="0b8c0-105">Den här artikeln beskrivs hello-metoden den här lösningen tar tooenable du toounderstand:</span><span class="sxs-lookup"><span data-stu-id="0b8c0-105">This article outlines hello approach this solution takes tooenable you toounderstand:</span></span>

* <span data-ttu-id="0b8c0-106">Lösning för hello vilka enheter metadata lagras.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-106">What device metadata hello solution stores.</span></span>
* <span data-ttu-id="0b8c0-107">Hur hanterar hello lösning hello enhetens metadata.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-107">How hello solution manages hello device metadata.</span></span>

## <a name="context"></a><span data-ttu-id="0b8c0-108">Kontext</span><span class="sxs-lookup"><span data-stu-id="0b8c0-108">Context</span></span>

<span data-ttu-id="0b8c0-109">Hej fjärrövervaknings förkonfigurerade lösningen använder [Azure IoT Hub] [ lnk-iot-hub] tooenable dina enheter toosend data toohello molnet.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-109">hello remote monitoring preconfigured solution uses [Azure IoT Hub][lnk-iot-hub] tooenable your devices toosend data toohello cloud.</span></span> <span data-ttu-id="0b8c0-110">hello lösningen lagrar information om enheter på tre olika platser:</span><span class="sxs-lookup"><span data-stu-id="0b8c0-110">hello solution stores information about devices in three different locations:</span></span>

| <span data-ttu-id="0b8c0-111">Plats</span><span class="sxs-lookup"><span data-stu-id="0b8c0-111">Location</span></span> | <span data-ttu-id="0b8c0-112">Information som lagras</span><span class="sxs-lookup"><span data-stu-id="0b8c0-112">Information stored</span></span> | <span data-ttu-id="0b8c0-113">Implementering</span><span class="sxs-lookup"><span data-stu-id="0b8c0-113">Implementation</span></span> |
| -------- | ------------------ | -------------- |
| <span data-ttu-id="0b8c0-114">Identitetsregistret</span><span class="sxs-lookup"><span data-stu-id="0b8c0-114">Identity registry</span></span> | <span data-ttu-id="0b8c0-115">Enhets-id, autentiseringsnycklar, aktiverade tillstånd</span><span class="sxs-lookup"><span data-stu-id="0b8c0-115">Device id, authentication keys, enabled state</span></span> | <span data-ttu-id="0b8c0-116">Inbyggda tooIoT Hub</span><span class="sxs-lookup"><span data-stu-id="0b8c0-116">Built in tooIoT Hub</span></span> |
| <span data-ttu-id="0b8c0-117">Enheten twins</span><span class="sxs-lookup"><span data-stu-id="0b8c0-117">Device twins</span></span> | <span data-ttu-id="0b8c0-118">Metadata: rapporterade egenskaper, önskade egenskaper, taggar</span><span class="sxs-lookup"><span data-stu-id="0b8c0-118">Metadata: reported properties, desired properties, tags</span></span> | <span data-ttu-id="0b8c0-119">Inbyggda tooIoT Hub</span><span class="sxs-lookup"><span data-stu-id="0b8c0-119">Built in tooIoT Hub</span></span> |
| <span data-ttu-id="0b8c0-120">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0b8c0-120">Cosmos DB</span></span> | <span data-ttu-id="0b8c0-121">Kommandot och metoden historik</span><span class="sxs-lookup"><span data-stu-id="0b8c0-121">Command and method history</span></span> | <span data-ttu-id="0b8c0-122">Anpassad för lösning</span><span class="sxs-lookup"><span data-stu-id="0b8c0-122">Custom for solution</span></span> |

<span data-ttu-id="0b8c0-123">IoT-hubb innehåller en [enhetsidentitetsregistret] [ lnk-identity-registry] toomanage åtkomst till tooan IoT-hubb och använder [enhet twins] [ lnk-device-twin] toomanage enhetens metadata .</span><span class="sxs-lookup"><span data-stu-id="0b8c0-123">IoT Hub includes a [device identity registry][lnk-identity-registry] toomanage access tooan IoT hub and uses [device twins][lnk-device-twin] toomanage device metadata.</span></span> <span data-ttu-id="0b8c0-124">Det finns också en fjärransluten övervakning Lösningsspecifika *enhetsregistret* som lagrar kommandot och metoden historik.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-124">There is also a remote monitoring solution-specific *device registry* that stores command and method history.</span></span> <span data-ttu-id="0b8c0-125">hello fjärråtkomst övervakning lösningen använder en [Cosmos DB] [ lnk-docdb] databasen tooimplement ett eget Arkiv för kommandot och metoden historik.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-125">hello remote monitoring solution uses a [Cosmos DB][lnk-docdb] database tooimplement a custom store for command and method history.</span></span>

> [!NOTE]
> <span data-ttu-id="0b8c0-126">hello remote förkonfigurerade övervakningslösning behåller hello enhetsidentitetsregistret synkroniserad med hello information i hello Cosmos-DB-databas.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-126">hello remote monitoring preconfigured solution keeps hello device identity registry in sync with hello information in hello Cosmos DB database.</span></span> <span data-ttu-id="0b8c0-127">Båda använder samma enhet id toouniquely identifiera hello varje enhet ansluten tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-127">Both use hello same device id toouniquely identify each device connected tooyour IoT hub.</span></span>

## <a name="device-metadata"></a><span data-ttu-id="0b8c0-128">Enhetens metadata</span><span class="sxs-lookup"><span data-stu-id="0b8c0-128">Device metadata</span></span>

<span data-ttu-id="0b8c0-129">IoT-hubb upprätthåller en [enheten dubbla] [ lnk-device-twin] anslutna tooa remote övervakningslösning för varje simulerade och fysisk enhet.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-129">IoT Hub maintains a [device twin][lnk-device-twin] for each simulated and physical device connected tooa remote monitoring solution.</span></span> <span data-ttu-id="0b8c0-130">hello lösningen använder enheten twins toomanage hello metadata som associeras med enheter.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-130">hello solution uses device twins toomanage hello metadata associated with devices.</span></span> <span data-ttu-id="0b8c0-131">En enhet dubbla är en JSON-dokumentet som underhålls av IoT-hubb och hello lösningen använder hello IoT-hubb API toointeract med enheten twins.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-131">A device twin is a JSON document maintained by IoT Hub, and hello solution uses hello IoT Hub API toointeract with device twins.</span></span>

<span data-ttu-id="0b8c0-132">En enhet dubbla lagrar tre typer av metadata:</span><span class="sxs-lookup"><span data-stu-id="0b8c0-132">A device twin stores three types of metadata:</span></span>

- <span data-ttu-id="0b8c0-133">*Rapporterade egenskaper* tooan IoT-hubb skickas av en enhet.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-133">*Reported properties* are sent tooan IoT hub by a device.</span></span> <span data-ttu-id="0b8c0-134">I hello remote övervakningslösning, simulerade enheterna skickar rapporterade egenskaper vid start och svar för**ändra enhetsstatus** kommandon och metoder.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-134">In hello remote monitoring solution, simulated devices send reported properties at start-up and in response too**Change device state** commands and methods.</span></span> <span data-ttu-id="0b8c0-135">Du kan visa rapporterade egenskaper i hello **enhetslistan** och **enhetsinformation** i hello lösning portal.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-135">You can view reported properties in hello **Device list** and **Device details** in hello solution portal.</span></span> <span data-ttu-id="0b8c0-136">Rapporterat egenskaper är skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-136">Reported properties are read only.</span></span>
- <span data-ttu-id="0b8c0-137">*Egenskaper för Desired* hämtas från hello IoT-hubb av enheter.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-137">*Desired properties* are retrieved from hello IoT hub by devices.</span></span> <span data-ttu-id="0b8c0-138">Det är hello ansvar hello enheten toomake alla nödvändiga konfigurationsändring på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-138">It is hello responsibility of hello device toomake any necessary configuration change on hello device.</span></span> <span data-ttu-id="0b8c0-139">Det är också hello ansvar hello enheten tooreport hello ändra tillbaka toohello hubb som rapporterades egenskap.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-139">It is also hello responsibility of hello device tooreport hello change back toohello hub as a reported property.</span></span> <span data-ttu-id="0b8c0-140">Du kan ange en önskad egenskapsvärdet hello lösning-portalen.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-140">You can set a desired property value through hello solution portal.</span></span>
- <span data-ttu-id="0b8c0-141">*Taggar* finns bara i hello enheten dubbla och aldrig synkroniseras med en enhet.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-141">*Tags* only exist in hello device twin and are never synchronized with a device.</span></span> <span data-ttu-id="0b8c0-142">Du kan ange värden i hello lösning portal och använda dem när du filtrerar hello lista över enheter.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-142">You can set tag values in hello solution portal and use them when you filter hello list of devices.</span></span> <span data-ttu-id="0b8c0-143">hello lösningen använder också en tagg tooidentify hello ikonen toodisplay för en enhet i hello lösning portal.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-143">hello solution also uses a tag tooidentify hello icon toodisplay for a device in hello solution portal.</span></span>

<span data-ttu-id="0b8c0-144">Exempel rapporterade egenskaper från hello simulerade enheter innehåller tillverkare, modellnummer, latitud och longitud.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-144">Example reported properties from hello simulated devices include manufacturer, model number, latitude, and longitude.</span></span> <span data-ttu-id="0b8c0-145">Simulerade enheter returnerar hello listan över metoder som stöds som en rapporterade egenskap.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-145">Simulated devices also return hello list of supported methods as a reported property.</span></span>

> [!NOTE]
> <span data-ttu-id="0b8c0-146">hello simulerade enheten koden endast använder hello **Desired.Config.TemperatureMeanValue** och **Desired.Config.TelemetryInterval** egenskaper tooupdate hello rapporterade egenskaper som skickas tillbaka tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-146">hello simulated device code only uses hello **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties tooupdate hello reported properties sent back tooIoT Hub.</span></span> <span data-ttu-id="0b8c0-147">Alla andra ändringsbegäranden för önskad egenskapen ignoreras.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-147">All other desired property change requests are ignored.</span></span>

<span data-ttu-id="0b8c0-148">En enhet information metadata JSON-dokumentet lagras i hello enheten registret Cosmos-DB-databasen har hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="0b8c0-148">A device information metadata JSON document stored in hello device registry Cosmos DB database has hello following structure:</span></span>

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
> <span data-ttu-id="0b8c0-149">Information om en enhet kan även inkludera metadata toodescribe hello telemetri hello enheten skickar tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-149">Device information can also include metadata toodescribe hello telemetry hello device sends tooIoT Hub.</span></span> <span data-ttu-id="0b8c0-150">hello remote övervakningslösning använder den här telemetri metadata toocustomize hur hello instrumentpanelen visar [dynamiska telemetri][lnk-dynamic-telemetry].</span><span class="sxs-lookup"><span data-stu-id="0b8c0-150">hello remote monitoring solution uses this telemetry metadata toocustomize how hello dashboard displays [dynamic telemetry][lnk-dynamic-telemetry].</span></span>

## <a name="lifecycle"></a><span data-ttu-id="0b8c0-151">Livscykel</span><span class="sxs-lookup"><span data-stu-id="0b8c0-151">Lifecycle</span></span>

<span data-ttu-id="0b8c0-152">När du skapar en enhet i hello lösning portal, skapar hello lösningen en post i hello Cosmos DB databasen toostore kommandot och metoden historik.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-152">When you first create a device in hello solution portal, hello solution creates an entry in hello Cosmos DB database toostore command and method history.</span></span> <span data-ttu-id="0b8c0-153">Hello lösning skapar nu även en post för hello enheten i hello enhetsidentitetsregistret, vilket genererar hello nycklar hello enheten använder tooauthenticate med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-153">At this point, hello solution also creates an entry for hello device in hello device identity registry, which generates hello keys hello device uses tooauthenticate with IoT Hub.</span></span> <span data-ttu-id="0b8c0-154">Dessutom skapas en delad enhet.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-154">It also creates a device twin.</span></span>

<span data-ttu-id="0b8c0-155">När en enhet ansluter först toohello lösning, skickar den rapporterade egenskaper och informationsmeddelande för enheten.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-155">When a device first connects toohello solution, it sends reported properties and a device information message.</span></span> <span data-ttu-id="0b8c0-156">hello rapporterade egenskapsvärden sparas automatiskt i hello enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-156">hello reported property values are automatically saved in hello device twin.</span></span> <span data-ttu-id="0b8c0-157">hello rapporterade egenskaper innehåller hello enhetens tillverkare, modellnummer, serienummer och en lista över metoder som stöds.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-157">hello reported properties include hello device manufacturer, model number, serial number, and a list of supported methods.</span></span> <span data-ttu-id="0b8c0-158">hälsningsmeddelande enheten information innehåller hello lista över hello kommandon hello enheten stöder inklusive information om några parametrar.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-158">hello device information message includes hello list of hello commands hello device supports including information about any command parameters.</span></span> <span data-ttu-id="0b8c0-159">När hello lösning får det här meddelandet, uppdaterar hello enhetsinformation i hello Cosmos-DB-databasen.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-159">When hello solution receives this message, it updates hello device information in hello Cosmos DB database.</span></span>

### <a name="view-and-edit-device-information-in-hello-solution-portal"></a><span data-ttu-id="0b8c0-160">Visa och redigera enhetsinformation i hello lösning portal</span><span class="sxs-lookup"><span data-stu-id="0b8c0-160">View and edit device information in hello solution portal</span></span>

<span data-ttu-id="0b8c0-161">hello listan över enheter i hello lösning portal visar hello följande egenskaper för enhet som kolumner som standard: **Status**, **DeviceId**, **tillverkare**, **Modellnummer**, **serienummer**, **Firmware**, **plattform**, **Processor**, och  **Installerade RAM**.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-161">hello device list in hello solution portal displays hello following device properties as columns by default: **Status**, **DeviceId**, **Manufacturer**, **Model Number**, **Serial Number**, **Firmware**, **Platform**, **Processor**, and **Installed RAM**.</span></span> <span data-ttu-id="0b8c0-162">Du kan anpassa hello kolumner genom att klicka på **kolumnen editor**.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-162">You can customize hello columns by clicking **Column editor**.</span></span> <span data-ttu-id="0b8c0-163">Hej enhetsegenskaper **latitud** och **longitud** enhet hello plats i hello Bing Map på hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-163">hello device properties **Latitude** and **Longitude** drive hello location in hello Bing Map on hello dashboard.</span></span>

![Kolumnen redigeraren i listan över enheter][img-device-list]

<span data-ttu-id="0b8c0-165">I hello **enhetsinformation** rutan i hello lösning portalen kan du redigera egenskaper och taggar (rapporterat egenskaper är skrivskyddade).</span><span class="sxs-lookup"><span data-stu-id="0b8c0-165">In hello **Device Details** pane in hello solution portal, you can edit desired properties and tags (reported properties are read only).</span></span>

![Informationspenelen][img-device-edit]

<span data-ttu-id="0b8c0-167">Du kan använda hello lösning portal tooremove en enhet från din lösning.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-167">You can use hello solution portal tooremove a device from your solution.</span></span> <span data-ttu-id="0b8c0-168">När du tar bort en enhet tar bort hello enhetspost från identitetsregistret hello lösning och tar bort hello enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-168">When you remove a device, hello solution removes hello device entry from identity registry and then deletes hello device twin.</span></span> <span data-ttu-id="0b8c0-169">hello lösning tar också bort information relaterad toohello enheten från hello Cosmos-DB-databas.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-169">hello solution also removes information related toohello device from hello Cosmos DB database.</span></span> <span data-ttu-id="0b8c0-170">Innan du tar bort en enhet, måste du inaktivera den.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-170">Before you can remove a device, you must disable it.</span></span>

![Ta bort enheten][img-device-remove]

## <a name="device-information-message-processing"></a><span data-ttu-id="0b8c0-172">Enheten information meddelandebehandling</span><span class="sxs-lookup"><span data-stu-id="0b8c0-172">Device information message processing</span></span>

<span data-ttu-id="0b8c0-173">Enheten informationsmeddelanden som skickats av en enhet skiljer sig från telemetri meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-173">Device information messages sent by a device are distinct from telemetry messages.</span></span> <span data-ttu-id="0b8c0-174">Enheten informationsmeddelanden innehåller hello-kommandon som en enhet kan svara på och eventuella kommandohistoriken.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-174">Device information messages include hello commands a device can respond to, and any command history.</span></span> <span data-ttu-id="0b8c0-175">IoT-hubb själva känner inte till hello metadata i ett meddelande för enheten information och processer hello meddelandet i hello samma sätt som den bearbetar alla meddelanden enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-175">IoT Hub itself has no knowledge of hello metadata contained in a device information message and processes hello message in hello same way it processes any device-to-cloud message.</span></span> <span data-ttu-id="0b8c0-176">I hello remote övervakningslösning, en [Azure Stream Analytics] [ lnk-stream-analytics] (ASA) jobbet läser hälsningsmeddelande från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-176">In hello remote monitoring solution, an [Azure Stream Analytics][lnk-stream-analytics] (ASA) job reads hello messages from IoT Hub.</span></span> <span data-ttu-id="0b8c0-177">Hej **DeviceInfo** stream analytics-jobbet filter för meddelanden som innehåller **”ObjectType”: ”DeviceInfo”** och vidarebefordrar dem toohello **EventProcessorHost** värden -instans som körs i ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-177">hello **DeviceInfo** stream analytics job filters for messages that contain **"ObjectType": "DeviceInfo"** and forwards them toohello **EventProcessorHost** host instance that runs in a web job.</span></span> <span data-ttu-id="0b8c0-178">Logiken i hello **EventProcessorHost** instans använder hello id toofind hello Cosmos DB enhetspost för hello särskild enhet och uppdatera hello post.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-178">Logic in hello **EventProcessorHost** instance uses hello device id toofind hello Cosmos DB record for hello specific device and update hello record.</span></span>

> [!NOTE]
> <span data-ttu-id="0b8c0-179">En enhet information meddelandet är en standard enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-179">A device information message is a standard device-to-cloud message.</span></span> <span data-ttu-id="0b8c0-180">hello lösning skiljer enheten informationsmeddelanden och telemetri meddelanden med hjälp av ASA frågor.</span><span class="sxs-lookup"><span data-stu-id="0b8c0-180">hello solution distinguishes between device information messages and telemetry messages by using ASA queries.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b8c0-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0b8c0-181">Next steps</span></span>

<span data-ttu-id="0b8c0-182">Nu du är klar med att lära dig hur du kan anpassa hello förkonfigurerade lösningar kan du utforska några av hello andra funktioner och förmågor i hello IoT Suite förkonfigurerade lösningar:</span><span class="sxs-lookup"><span data-stu-id="0b8c0-182">Now you've finished learning how you can customize hello preconfigured solutions, you can explore some of hello other features and capabilities of hello IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="0b8c0-183">[Förutsägande Underhåll förkonfigurerade lösning: översikt][lnk-predictive-overview]</span><span class="sxs-lookup"><span data-stu-id="0b8c0-183">[Predictive maintenance preconfigured solution overview][lnk-predictive-overview]</span></span>
* <span data-ttu-id="0b8c0-184">[Vanliga frågor och svar om IoT Suite][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="0b8c0-184">[Frequently asked questions for IoT Suite][lnk-faq]</span></span>
* <span data-ttu-id="0b8c0-185">[IoT-säkerhet från hello bakgrund][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="0b8c0-185">[IoT security from hello ground up][lnk-security-groundup]</span></span>

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
