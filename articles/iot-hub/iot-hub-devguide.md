---
title: "aaaDeveloper guide för Azure IoT-hubb | Microsoft Docs"
description: "Utvecklarhandbok för hello Azure IoT Hub innehåller diskussioner slutpunkter, säkerhet, hello identitetsregistret, hantering av enheter, direkt metoder, enheten twins, filöverföringar, jobb, hello frågespråk för IoT-hubb och meddelanden."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: d534ff9d-2de5-4995-bb2d-84a02693cb2e
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 2d3f18399e4cef6f9c4850a5caeb170a2d091a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-developer-guide"></a><span data-ttu-id="369dc-103">Utvecklarhandbok för Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="369dc-103">Azure IoT Hub developer guide</span></span>

<span data-ttu-id="369dc-104">Azure IoT Hub är en helt hanterad tjänst som hjälper till att aktivera tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning tillbaka avslutas.</span><span class="sxs-lookup"><span data-stu-id="369dc-104">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span>

<span data-ttu-id="369dc-105">Azure IoT-hubb ger dig:</span><span class="sxs-lookup"><span data-stu-id="369dc-105">Azure IoT Hub provides you with:</span></span>

* <span data-ttu-id="369dc-106">Säker kommunikation med hjälp av autentiseringsuppgifterna per enhet och åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="369dc-106">Secure communications by using per-device security credentials and access control.</span></span>
* <span data-ttu-id="369dc-107">Flera enhet till moln och moln till enhet storskaliga kommunikationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="369dc-107">Multiple device-to-cloud and cloud-to-device hyper-scale communication options.</span></span>
* <span data-ttu-id="369dc-108">Frågbar lagring av information om tillstånd per enhet och metadata.</span><span class="sxs-lookup"><span data-stu-id="369dc-108">Queryable storage of per-device state information and meta-data.</span></span>
* <span data-ttu-id="369dc-109">Enkelt enhetsanslutning med enhetsbibliotek för hello mest populära språk och plattformar.</span><span class="sxs-lookup"><span data-stu-id="369dc-109">Easy device connectivity with device libraries for hello most popular languages and platforms.</span></span>

<span data-ttu-id="369dc-110">Den här IoT-hubb Utvecklarhandbok innehåller hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="369dc-110">This IoT Hub developer guide includes hello following articles:</span></span>

* <span data-ttu-id="369dc-111">[Enhet till moln kommunikation vägledning] [ lnk-d2c-guidance] kan du välja mellan meddelanden från enhet till moln, enheten dubbla rapporterade egenskaper och ladda upp filen.</span><span class="sxs-lookup"><span data-stu-id="369dc-111">[Device-to-cloud communication guidance][lnk-d2c-guidance] helps you choose between device-to-cloud messages, device twin's reported properties, and file upload.</span></span>
* <span data-ttu-id="369dc-112">[Moln till enhet kommunikation vägledning] [ lnk-c2d-guidance] kan du välja mellan direkt metoder, enheten dubbla egenskaper och moln till enhet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="369dc-112">[Cloud-to-device communication guidance][lnk-c2d-guidance] helps you choose between direct methods, device twin's desired properties, and cloud-to-device messages.</span></span>
* <span data-ttu-id="369dc-113">[Enhet till moln och moln till enhet meddelanden med IoT-hubben] [ devguide-messaging] beskriver hello meddelandefunktioner (enhet till moln och moln till enhet) som visar IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="369dc-113">[Device-to-cloud and cloud-to-device messaging with IoT Hub][devguide-messaging] describes hello messaging features (device-to-cloud and cloud-to-device) that IoT Hub exposes.</span></span>
  * <span data-ttu-id="369dc-114">[Skicka meddelanden från enhet till moln tooIoT hubb][devguide-messages-d2c].</span><span class="sxs-lookup"><span data-stu-id="369dc-114">[Send device-to-cloud messages tooIoT Hub][devguide-messages-d2c].</span></span>
  * <span data-ttu-id="369dc-115">[Läsa meddelanden från enhet till moln från hello inbyggd slutpunkt][devguide-builtin].</span><span class="sxs-lookup"><span data-stu-id="369dc-115">[Read device-to-cloud messages from hello built-in endpoint][devguide-builtin].</span></span>
  * <span data-ttu-id="369dc-116">[Använd anpassade slutpunkter och routningsregler för meddelanden från enhet till moln][devguide-custom].</span><span class="sxs-lookup"><span data-stu-id="369dc-116">[Use custom endpoints and routing rules for device-to-cloud messages][devguide-custom].</span></span>
  * <span data-ttu-id="369dc-117">[Skicka meddelanden moln till enhet från IoT-hubb][devguide-messages-c2d].</span><span class="sxs-lookup"><span data-stu-id="369dc-117">[Send cloud-to-device messages from IoT Hub][devguide-messages-c2d].</span></span>
  * <span data-ttu-id="369dc-118">[Skapa och läsa IoT-hubb][devguide-format].</span><span class="sxs-lookup"><span data-stu-id="369dc-118">[Create and read IoT Hub messages][devguide-format].</span></span>
* <span data-ttu-id="369dc-119">[Överföra filer från en enhet] [ devguide-upload] beskriver hur du kan ladda upp filer från en enhet.</span><span class="sxs-lookup"><span data-stu-id="369dc-119">[Upload files from a device][devguide-upload] describes how you can upload files from a device.</span></span> <span data-ttu-id="369dc-120">hello artikeln innehåller också information om exempelvis hello kan skicka meddelanden hello överföringen.</span><span class="sxs-lookup"><span data-stu-id="369dc-120">hello article also includes information about topics such as hello notifications hello upload process can send.</span></span>
* <span data-ttu-id="369dc-121">[Hantera identiteter för enheten i IoT-hubb] [ devguide-identities] beskriver vilken information varje IoT-hubb registret identitetslagringar och hur du kan komma åt och ändra den.</span><span class="sxs-lookup"><span data-stu-id="369dc-121">[Manage device identities in IoT Hub][devguide-identities] describes what information each IoT hub's identity registry stores, and how you can access and modify it.</span></span>
* <span data-ttu-id="369dc-122">[Kontrollera åtkomst tooIoT hubb] [ devguide-security] beskrivs hello säkerhet används toogrant åtkomst tooIoT hubb funktioner för både enheter och komponenter i molnet.</span><span class="sxs-lookup"><span data-stu-id="369dc-122">[Control access tooIoT Hub][devguide-security] describes hello security model used toogrant access tooIoT Hub functionality for both devices and cloud components.</span></span> <span data-ttu-id="369dc-123">hello artikeln innehåller information om hur du använder token och X.509-certifikat och information om hello behörigheter som du kan ge.</span><span class="sxs-lookup"><span data-stu-id="369dc-123">hello article includes information about using tokens and X.509 certificates, and details of hello permissions you can grant.</span></span>
* <span data-ttu-id="369dc-124">[Använda enhetstillstånd twins toosynchronize och konfigurationer] [ devguide-device-twins] beskriver hello *enheten dubbla* begrepp och hello funktioner som det visar till exempel synkronisera en enhet med sin enhet dubbla.</span><span class="sxs-lookup"><span data-stu-id="369dc-124">[Use device twins toosynchronize state and configurations][devguide-device-twins] describes hello *device twin* concept and hello functionality it exposes such as synchronizing a device with its device twin.</span></span> <span data-ttu-id="369dc-125">hello artikeln innehåller information om hello data som lagras i en delad enhet.</span><span class="sxs-lookup"><span data-stu-id="369dc-125">hello article includes information about hello data stored in a device twin.</span></span>
* <span data-ttu-id="369dc-126">[Anropa en metod som är direkt på en enhet] [ devguide-directmethods] beskriver hello livscykeln för en direkt metod, information om hur tooinvoke metoder på en enhet från din backend-app och referensen hello direkt metod på enheten.</span><span class="sxs-lookup"><span data-stu-id="369dc-126">[Invoke a direct method on a device][devguide-directmethods] describes hello lifecycle of a direct method, information about how tooinvoke methods on a device from your back-end app and handle hello direct method on your device.</span></span>
* <span data-ttu-id="369dc-127">[Schemalägga jobb på flera enheter] [ devguide-jobs] beskriver hur du schemalägger jobb på flera enheter.</span><span class="sxs-lookup"><span data-stu-id="369dc-127">[Schedule jobs on multiple devices][devguide-jobs] describes how you can schedule jobs on multiple devices.</span></span> <span data-ttu-id="369dc-128">hello artikeln beskriver hur toosubmit jobb som utför uppgifter som att köra en direkt metod, uppdaterar en enhet med en delad enhet.</span><span class="sxs-lookup"><span data-stu-id="369dc-128">hello article describes how toosubmit jobs that perform tasks as executing a direct method, updating a device using a device twin.</span></span> <span data-ttu-id="369dc-129">Här beskrivs också hur tooquery hello status för ett jobb.</span><span class="sxs-lookup"><span data-stu-id="369dc-129">It also describes how tooquery hello status of a job.</span></span>
* <span data-ttu-id="369dc-130">[Referera - Välj ett kommunikationsprotokoll] [ devguide-protocol] beskriver hello kommunikationsprotokoll att IoT-hubb som har stöd för kommunikation mellan och visar hello portar som ska vara öppen.</span><span class="sxs-lookup"><span data-stu-id="369dc-130">[Reference - choose a communication protocol][devguide-protocol] describes hello communication protocols that IoT Hub supports for device communication and lists hello ports that should be open.</span></span>
* <span data-ttu-id="369dc-131">[Referens - IoT-hubbslutpunkter] [ devguide-endpoints] beskriver hello olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="369dc-131">[Reference - IoT Hub endpoints][devguide-endpoints] describes hello various endpoints that each IoT hub exposes for runtime and management operations.</span></span> <span data-ttu-id="369dc-132">hello beskrivs också hur du kan skapa ytterligare slutpunkter i din IoT-hubb och hur toouse en fältet gateway tooenable enheter anslutningen tooyour IoT-hubb slutpunkter i scenarier som inte är standard.</span><span class="sxs-lookup"><span data-stu-id="369dc-132">hello article also describes how you can create additional endpoints in your IoT hub, and how toouse a field gateway tooenable devices connectivity tooyour IoT Hub endpoints in non-standard scenarios.</span></span>
* <span data-ttu-id="369dc-133">[Referens - frågespråket för IoT-hubb för enheten twins, jobb och meddelanderoutning] [ devguide-query] beskriver den IoT-hubb frågespråk som möjliggör tooretrieve information från din hubb om enheten twins och jobb.</span><span class="sxs-lookup"><span data-stu-id="369dc-133">[Reference - IoT Hub query language for device twins, jobs, and message routing][devguide-query] describes that IoT Hub query language that enables you tooretrieve information from your hub about your device twins and jobs.</span></span>
* <span data-ttu-id="369dc-134">[Referens - kvoter och begränsning] [ devguide-quotas] sammanfattar hello kvoter som anges i hello IoT-hubb-tjänsten och hello begränsning beteende som du kan förvänta dig toosee när en kvot.</span><span class="sxs-lookup"><span data-stu-id="369dc-134">[Reference - quotas and throttling][devguide-quotas] summarizes hello quotas set in hello IoT Hub service and hello throttling behavior you can expect toosee when you exceed a quota.</span></span>
* <span data-ttu-id="369dc-135">[Referera - priser] [ devguide-pricing] ger allmän information om olika SKU: er och priser för IoT-hubb och information om hur olika funktioner i IoT-hubben är avgiftsbelagda som hello meddelanden i IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="369dc-135">[Reference - pricing][devguide-pricing] provides general information on different SKUs and pricing for IoT Hub and details on how hello various IoT Hub functionalities are metered as messages by IoT Hub.</span></span>
* <span data-ttu-id="369dc-136">[Referens - enheten och tjänsten SDK] [ devguide-sdks] visar hello Azure IoT-SDK: er kan du använda toodevelop både enheten och tjänsten appar som samverkar med din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="369dc-136">[Reference - Device and service SDKs][devguide-sdks] lists hello Azure IoT SDKs you can use toodevelop both device and service apps that interact with your IoT hub.</span></span> <span data-ttu-id="369dc-137">hello artikeln innehåller länkar tooonline API-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="369dc-137">hello article includes links tooonline API documentation.</span></span>
* <span data-ttu-id="369dc-138">[Referens - stöd för IoT-hubb MQTT] [ devguide-mqtt] innehåller detaljerad information om hur IoT-hubb stöder hello MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="369dc-138">[Reference - IoT Hub MQTT support][devguide-mqtt] provides detailed information about how IoT Hub supports hello MQTT protocol.</span></span> <span data-ttu-id="369dc-139">hello artikeln beskriver hello stöd för hello MQTT protokollet inbyggda toohello Azure IoT-SDK: er och innehåller information om hur du använder hello MQTT protokollet direkt.</span><span class="sxs-lookup"><span data-stu-id="369dc-139">hello article describes hello support for hello MQTT protocol built-in toohello Azure IoT SDKs and provides information about using hello MQTT protocol directly.</span></span>
* <span data-ttu-id="369dc-140">[Ordlista] [ devguide-glossary] en lista över vanliga IoT-hubb-relaterade termer.</span><span class="sxs-lookup"><span data-stu-id="369dc-140">[Glossary][devguide-glossary] a list of common IoT Hub-related terms.</span></span>

[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md
[devguide-pricing]: iot-hub-devguide-pricing.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[devguide-messages-d2c]: iot-hub-devguide-messages-d2c.md
[devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[devguide-custom]: iot-hub-devguide-messages-read-custom.md
[devguide-messages-c2d]: iot-hub-devguide-messages-c2d.md
[devguide-format]: iot-hub-devguide-messages-construct.md
[devguide-protocol]: iot-hub-devguide-protocols.md
