---
title: "aaaThe Azure IoT-enhet SDK för C | Microsoft Docs"
description: "Komma igång med hello Azure IoT-enhet SDK för C och lära dig hur toocreate appar för enheter som kommunicerar med IoT-hubben."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: e448b061-6bdd-470a-a527-15ec03cca7b9
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: obloch
ms.openlocfilehash: 9e20742e6ea513c124bfaf28f02f6fba86170daf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-device-sdk-for-c"></a><span data-ttu-id="609ab-103">Azure IoT-enhet SDK för C</span><span class="sxs-lookup"><span data-stu-id="609ab-103">Azure IoT device SDK for C</span></span>

<span data-ttu-id="609ab-104">Hej **Azure IoT-enhet SDK** är en uppsättning bibliotek utformats toosimplify hello för att skicka meddelanden tooand ta emot meddelanden från hello **Azure IoT Hub** service.</span><span class="sxs-lookup"><span data-stu-id="609ab-104">hello **Azure IoT device SDK** is a set of libraries designed toosimplify hello process of sending messages tooand receiving messages from hello **Azure IoT Hub** service.</span></span> <span data-ttu-id="609ab-105">Det finns olika varianter av hello SDK för riktad en viss plattform, men den här artikeln beskriver hello **Azure IoT-enhet SDK för C**.</span><span class="sxs-lookup"><span data-stu-id="609ab-105">There are different variations of hello SDK, each targeting a specific platform, but this article describes hello **Azure IoT device SDK for C**.</span></span>

<span data-ttu-id="609ab-106">hello Azure IoT-enhet SDK för C sparas i ANSI C (C99) toomaximize portability.</span><span class="sxs-lookup"><span data-stu-id="609ab-106">hello Azure IoT device SDK for C is written in ANSI C (C99) toomaximize portability.</span></span> <span data-ttu-id="609ab-107">Den här funktionen gör hello bibliotek väl lämpade toooperate på flera plattformar och enheter, särskilt om minimera disk och minneskrav är en prioritet.</span><span class="sxs-lookup"><span data-stu-id="609ab-107">This feature makes hello libraries well-suited toooperate on multiple platforms and devices, especially where minimizing disk and memory footprint is a priority.</span></span>

<span data-ttu-id="609ab-108">Det finns en mängd olika plattformar på vilka hello SDK har testats (se hello [Azure certifierad för IoT-enhet katalogen](https://catalog.azureiotsuite.com/) information).</span><span class="sxs-lookup"><span data-stu-id="609ab-108">There are a broad range of platforms on which hello SDK has been tested (see hello [Azure Certified for IoT device catalog](https://catalog.azureiotsuite.com/) for details).</span></span> <span data-ttu-id="609ab-109">Även om den här artikeln innehåller genomgångar av exempelkod som körs på Windows hello-plattformen, är hello koden beskrivs i den här artikeln identiska över hello mängd plattformar som stöds.</span><span class="sxs-lookup"><span data-stu-id="609ab-109">Although this article includes walkthroughs of sample code running on hello Windows platform, hello code described in this article is identical across hello range of supported platforms.</span></span>

<span data-ttu-id="609ab-110">Den här artikeln ger en introduktion hello Azure IoT-enhet SDK toohello arkitektur för C. Den visar hur tooinitialize hello enheten biblioteket skicka data tooIoT hubb och ta emot meddelanden från den.</span><span class="sxs-lookup"><span data-stu-id="609ab-110">This article introduces you toohello architecture of hello Azure IoT device SDK for C. It demonstrates how tooinitialize hello device library, send data tooIoT Hub, and receive messages from it.</span></span> <span data-ttu-id="609ab-111">hello informationen i den här artikeln bör vara tillräckligt med tooget igång med hello SDK, men innehåller även pekare tooadditional information om hello-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="609ab-111">hello information in this article should be enough tooget started using hello SDK, but also provides pointers tooadditional information about hello libraries.</span></span>

## <a name="sdk-architecture"></a><span data-ttu-id="609ab-112">SDK-arkitektur</span><span class="sxs-lookup"><span data-stu-id="609ab-112">SDK architecture</span></span>

<span data-ttu-id="609ab-113">Du kan hitta hello [ **Azure IoT-enhet SDK för C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-lagringsplatsen och visa information om hello API i hello [C API-referens för](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="609ab-113">You can find hello [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of hello API in hello [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

<span data-ttu-id="609ab-114">hello senaste versionen av hello-bibliotek finns i hello **master** grenen av hello databasen:</span><span class="sxs-lookup"><span data-stu-id="609ab-114">hello latest version of hello libraries can be found in hello **master** branch of hello repository:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

* <span data-ttu-id="609ab-115">hello core implementering av hello SDK är i hello **iothub\_klienten** mapp som innehåller hello implementering av hello lägsta API-lagret i hello SDK: hello **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="609ab-115">hello core implementation of hello SDK is in hello **iothub\_client** folder that contains hello implementation of hello lowest API layer in hello SDK: hello **IoTHubClient** library.</span></span> <span data-ttu-id="609ab-116">Hej **IoTHubClient** biblioteket innehåller API: er som implementerar raw-meddelanden för att skicka meddelanden tooIoT hubb och ta emot meddelanden från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="609ab-116">hello **IoTHubClient** library contains APIs implementing raw messaging for sending messages tooIoT Hub and receiving messages from IoT Hub.</span></span> <span data-ttu-id="609ab-117">När du använder det här biblioteket kan du ansvarar för att implementera meddelandet serialisering, men andra detaljer för kommunikation med IoT-hubb hanteras åt dig.</span><span class="sxs-lookup"><span data-stu-id="609ab-117">When using this library, you are responsible for implementing message serialization, but other details of communicating with IoT Hub are handled for you.</span></span>
* <span data-ttu-id="609ab-118">Hej **serialiseraren** mappen innehåller hjälpfunktioner och exempel som visar hur tooserialize data innan du skickar tooAzure IoT-hubb med hello klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="609ab-118">hello **serializer** folder contains helper functions and samples that show you how tooserialize data before sending tooAzure IoT Hub using hello client library.</span></span> <span data-ttu-id="609ab-119">hello användning av hello serialiseraren är inte obligatoriskt och tillhandahålls bekvämlighets skull.</span><span class="sxs-lookup"><span data-stu-id="609ab-119">hello use of hello serializer is not mandatory and is provided as a convenience.</span></span> <span data-ttu-id="609ab-120">toouse hello **serialiseraren** bibliotek, definierar du en modell som anger data toosend tooIoT NAV- och hello hälsningsmeddelande du förväntar dig tooreceive från den.</span><span class="sxs-lookup"><span data-stu-id="609ab-120">toouse hello **serializer** library, you define a model that specifies hello data toosend tooIoT Hub and hello messages you expect tooreceive from it.</span></span> <span data-ttu-id="609ab-121">När hello modellen har definierats hello SDK ger dig en API-yta som gör att du tooeasily arbete med enhet till moln och moln till enhet meddelanden utan att oroa hello serialisering information.</span><span class="sxs-lookup"><span data-stu-id="609ab-121">Once hello model is defined, hello SDK provides you with an API surface that enables you tooeasily work with device-to-cloud and cloud-to-device messages without worrying about hello serialization details.</span></span> <span data-ttu-id="609ab-122">hello-biblioteket är beroende av andra bibliotek med öppen källkod som implementerar transport med hjälp av protokoll som MQTT och AMQP.</span><span class="sxs-lookup"><span data-stu-id="609ab-122">hello library depends on other open source libraries that implement transport using protocols such as MQTT and AMQP.</span></span>
* <span data-ttu-id="609ab-123">Hej **IoTHubClient** biblioteket är beroende av andra bibliotek med öppen källkod:</span><span class="sxs-lookup"><span data-stu-id="609ab-123">hello **IoTHubClient** library depends on other open source libraries:</span></span>
  * <span data-ttu-id="609ab-124">Hej [Azure C delade verktyget](https://github.com/Azure/azure-c-shared-utility) biblioteket, som innehåller vanliga funktioner för grundläggande uppgifter (till exempel strängar, lista bearbetning och -i/o) som krävs för över flera Azure-relaterade C SDK: er.</span><span class="sxs-lookup"><span data-stu-id="609ab-124">hello [Azure C shared utility](https://github.com/Azure/azure-c-shared-utility) library, which provides common functionality for basic tasks (such as strings, list manipulation, and IO) needed across several Azure-related C SDKs.</span></span>
  * <span data-ttu-id="609ab-125">Hej [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) biblioteket, som är en implementering för klientsidan av AMQP som är optimerade för resursen begränsad enheter.</span><span class="sxs-lookup"><span data-stu-id="609ab-125">hello [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) library, which is a client-side implementation of AMQP optimized for resource constrained devices.</span></span>
  * <span data-ttu-id="609ab-126">Hej [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) biblioteket, vilket är ett allmänna implementering hello MQTT protokollet-bibliotek och optimeras för resursen begränsad enheter.</span><span class="sxs-lookup"><span data-stu-id="609ab-126">hello [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) library, which is a general-purpose library implementing hello MQTT protocol and optimized for resource constrained devices.</span></span>

<span data-ttu-id="609ab-127">Användning av dessa bibliotek är enklare toounderstand genom att titta på exempelkod.</span><span class="sxs-lookup"><span data-stu-id="609ab-127">Use of these libraries is easier toounderstand by looking at example code.</span></span> <span data-ttu-id="609ab-128">hello följande avsnitt vägleder dig genom flera hello exempelprogram som ingår i hello SDK.</span><span class="sxs-lookup"><span data-stu-id="609ab-128">hello following sections walk you through several of hello sample applications that are included in hello SDK.</span></span> <span data-ttu-id="609ab-129">Den här genomgången bör ge dig en bra anser för hello olika funktioner i hello arkitektur lager med hello SDK och en introduktion toohow hello-API: er fungerar.</span><span class="sxs-lookup"><span data-stu-id="609ab-129">This walkthrough should give you a good feel for hello various capabilities of hello architectural layers of hello SDK and an introduction toohow hello APIs work.</span></span>

## <a name="before-you-run-hello-samples"></a><span data-ttu-id="609ab-130">Innan du kör hello-exempel</span><span class="sxs-lookup"><span data-stu-id="609ab-130">Before you run hello samples</span></span>

<span data-ttu-id="609ab-131">Innan du kan köra hello prover i hello Azure IoT-enhet SDK för C, måste du [skapa en instans av hello IoT-hubb service](iot-hub-create-through-portal.md) i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="609ab-131">Before you can run hello samples in hello Azure IoT device SDK for C, you must [create an instance of hello IoT Hub service](iot-hub-create-through-portal.md) in your Azure subscription.</span></span> <span data-ttu-id="609ab-132">Slutför hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="609ab-132">Then complete hello following tasks:</span></span>

* <span data-ttu-id="609ab-133">Förbereda utvecklingsmiljön</span><span class="sxs-lookup"><span data-stu-id="609ab-133">Prepare your development environment</span></span>
* <span data-ttu-id="609ab-134">Hämta autentiseringsuppgifter för enheten.</span><span class="sxs-lookup"><span data-stu-id="609ab-134">Obtain device credentials.</span></span>

### <a name="prepare-your-development-environment"></a><span data-ttu-id="609ab-135">Förbereda utvecklingsmiljön</span><span class="sxs-lookup"><span data-stu-id="609ab-135">Prepare your development environment</span></span>

<span data-ttu-id="609ab-136">Paket har angetts för vanliga plattformar (till exempel NuGet för Windows eller apt_get för Debian och Ubuntu) och hello exempel använder dessa paket när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="609ab-136">Packages are provided for common platforms (such as NuGet for Windows or apt_get for Debian and Ubuntu) and hello samples use these packages when available.</span></span> <span data-ttu-id="609ab-137">I vissa fall behöver du toocompile hello SDK för eller på din enhet.</span><span class="sxs-lookup"><span data-stu-id="609ab-137">In some cases, you need toocompile hello SDK for or on your device.</span></span> <span data-ttu-id="609ab-138">Om du behöver toocompile hello SDK, se [förbereda din utvecklingsmiljö](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) i hello GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="609ab-138">If you need toocompile hello SDK, see [Prepare your development environment](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) in hello GitHub repository.</span></span>

<span data-ttu-id="609ab-139">tooobtain hello programmet exempelkod, hämta en kopia av hello SDK från GitHub.</span><span class="sxs-lookup"><span data-stu-id="609ab-139">tooobtain hello sample application code, download a copy of hello SDK from GitHub.</span></span> <span data-ttu-id="609ab-140">Hämta ditt exemplar av hello källa från hello **master** grenen av hello [GitHub-lagringsplatsen](https://github.com/Azure/azure-iot-sdk-c).</span><span class="sxs-lookup"><span data-stu-id="609ab-140">Get your copy of hello source from hello **master** branch of hello [GitHub repository](https://github.com/Azure/azure-iot-sdk-c).</span></span>


### <a name="obtain-hello-device-credentials"></a><span data-ttu-id="609ab-141">Hämta hello enheten autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="609ab-141">Obtain hello device credentials</span></span>

<span data-ttu-id="609ab-142">Nu när du har hello exempelkod för datakällan är hello nästa sak toodo tooget en uppsättning autentiseringsuppgifter för enheten.</span><span class="sxs-lookup"><span data-stu-id="609ab-142">Now that you have hello sample source code, hello next thing toodo is tooget a set of device credentials.</span></span> <span data-ttu-id="609ab-143">För en enhet toobe kan tooaccess en IoT-hubb, måste du först lägga till hello enheten toohello IoT-hubb identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="609ab-143">For a device toobe able tooaccess an IoT hub, you must first add hello device toohello IoT Hub identity registry.</span></span> <span data-ttu-id="609ab-144">När du lägger till en enhet kan du hämta en uppsättning autentiseringsuppgifter för enheten som du behöver för hello enheten toobe kan tooconnect toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="609ab-144">When you add your device, you get a set of device credentials that you need for hello device toobe able tooconnect toohello IoT hub.</span></span> <span data-ttu-id="609ab-145">hello exempelprogram som beskrivs i nästa avsnitt om hello räknar autentiseringsuppgifterna i hello form av en **enheten anslutningssträngen**.</span><span class="sxs-lookup"><span data-stu-id="609ab-145">hello sample applications discussed in hello next section expect these credentials in hello form of a **device connection string**.</span></span>

<span data-ttu-id="609ab-146">Det finns flera toohelp för öppen källkod verktyg du hanterar din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="609ab-146">There are several open source tools toohelp you manage your IoT hub.</span></span>

* <span data-ttu-id="609ab-147">Ett Windows-program som kallas [enheten explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span><span class="sxs-lookup"><span data-stu-id="609ab-147">A Windows application called [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span></span>
* <span data-ttu-id="609ab-148">Ett plattformsoberoende node.js CLI-verktyg kallas [iothub explorer](https://github.com/azure/iothub-explorer).</span><span class="sxs-lookup"><span data-stu-id="609ab-148">A cross-platform node.js CLI tool called [iothub-explorer](https://github.com/azure/iothub-explorer).</span></span>

<span data-ttu-id="609ab-149">Den här kursen använder hello grafiska *enheten explorer* verktyget.</span><span class="sxs-lookup"><span data-stu-id="609ab-149">This tutorial uses hello graphical *device explorer* tool.</span></span> <span data-ttu-id="609ab-150">Du kan också använda hello *iothub explorer* verktyget om du föredrar toouse ett CLI-verktyg.</span><span class="sxs-lookup"><span data-stu-id="609ab-150">You can also use hello *iothub-explorer* tool if you prefer toouse a CLI tool.</span></span>

<span data-ttu-id="609ab-151">hello enheten explorer verktyget använder hello Azure IoT-tjänsten bibliotek tooperform olika funktioner på IoT Hub, inklusive lägga till enheter.</span><span class="sxs-lookup"><span data-stu-id="609ab-151">hello device explorer tool uses hello Azure IoT service libraries tooperform various functions on IoT Hub, including adding devices.</span></span> <span data-ttu-id="609ab-152">Om du använder hello enheten explorer verktyget tooadd en enhet får du en anslutningssträng för enheten.</span><span class="sxs-lookup"><span data-stu-id="609ab-152">If you use hello device explorer tool tooadd a device, you get a connection string for your device.</span></span> <span data-ttu-id="609ab-153">Du behöver den här anslutningen sträng toorun hello exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="609ab-153">You need this connection string toorun hello sample applications.</span></span>

<span data-ttu-id="609ab-154">Om du inte är bekant med hello enheten explorer verktyget hello följande procedur beskriver hur toouse den tooadd en enhet och få en anslutningssträng för enheten.</span><span class="sxs-lookup"><span data-stu-id="609ab-154">If you're not familiar with hello device explorer tool, hello following procedure describes how toouse it tooadd a device and obtain a device connection string.</span></span>

<span data-ttu-id="609ab-155">tooinstall hello enheten explorer verktyget, se [hur toouse hello enheten Explorer för IoT Hub-enheter](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span><span class="sxs-lookup"><span data-stu-id="609ab-155">tooinstall hello device explorer tool, see [How toouse hello Device Explorer for IoT Hub devices](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).</span></span>

<span data-ttu-id="609ab-156">När du kör programmet hello finns det här gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="609ab-156">When you run hello program, you see this interface:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

<span data-ttu-id="609ab-157">Ange din **IoT-hubb anslutningssträngen** i hello först fältet och klickar på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="609ab-157">Enter your **IoT Hub Connection String** in hello first field and click **Update**.</span></span> <span data-ttu-id="609ab-158">Det här steget konfigurerar hello verktyget så att den kan kommunicera med IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="609ab-158">This step configures hello tool so that it can communicate with IoT Hub.</span></span>

<span data-ttu-id="609ab-159">När hello IoT-hubb anslutningssträngen har konfigurerats klickar du på hello **Management** fliken:</span><span class="sxs-lookup"><span data-stu-id="609ab-159">When hello IoT Hub connection string is configured, click hello **Management** tab:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

<span data-ttu-id="609ab-160">Den här fliken är där du hanterar hello enheter registrerade i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="609ab-160">This tab is where you manage hello devices registered in your IoT hub.</span></span>

<span data-ttu-id="609ab-161">Du skapar en enhet genom att klicka på hello **skapa** knappen.</span><span class="sxs-lookup"><span data-stu-id="609ab-161">You create a device by clicking hello **Create** button.</span></span> <span data-ttu-id="609ab-162">En dialogruta visas med en uppsättning i förväg nycklar (primär eller sekundär).</span><span class="sxs-lookup"><span data-stu-id="609ab-162">A dialog displays with a set of pre-populated keys (primary and secondary).</span></span> <span data-ttu-id="609ab-163">Ange en **enhets-ID** och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="609ab-163">Enter a **Device ID** and then click **Create**.</span></span>

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

<span data-ttu-id="609ab-164">När hello enhet skapas listas hello enheter uppdateringar med alla hello registrerade enheter, inklusive hello som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="609ab-164">When hello device is created, hello Devices list updates with all hello registered devices, including hello one you just created.</span></span> <span data-ttu-id="609ab-165">Om du högerklickar på den nya enheten visas den här menyn:</span><span class="sxs-lookup"><span data-stu-id="609ab-165">If you right-click your new device, you see this menu:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

<span data-ttu-id="609ab-166">Om du väljer **kopiera anslutningssträngen för vald enhet**, hello enheten anslutningssträngen är kopierade toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="609ab-166">If you choose **Copy connection string for selected device**, hello device connection string is copied toohello clipboard.</span></span> <span data-ttu-id="609ab-167">Spara en kopia av hello anslutningssträngen för enheten.</span><span class="sxs-lookup"><span data-stu-id="609ab-167">Keep a copy of hello device connection string.</span></span> <span data-ttu-id="609ab-168">Du behöver den när du kör hello exempelprogram som beskrivs i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="609ab-168">You need it when running hello sample applications described in hello following sections.</span></span>

<span data-ttu-id="609ab-169">När du har slutfört hello stegen ovan, är du redo toostart kör kod.</span><span class="sxs-lookup"><span data-stu-id="609ab-169">When you've completed hello steps above, you're ready toostart running some code.</span></span> <span data-ttu-id="609ab-170">Båda exempel har en konstant hello överst i hello huvudsakliga källfilen som möjliggör tooenter en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="609ab-170">Both samples have a constant at hello top of hello main source file that enables you tooenter a connection string.</span></span> <span data-ttu-id="609ab-171">Till exempel hello motsvarande rad från hello **iothub\_klienten\_exempel\_mqtt** programmet visas på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="609ab-171">For example, hello corresponding line from hello **iothub\_client\_sample\_mqtt** application appears as follows.</span></span>

```c
static const char* connectionString = "[device connection string]";
```

## <a name="use-hello-iothubclient-library"></a><span data-ttu-id="609ab-172">Använd hello IoTHubClient biblioteket</span><span class="sxs-lookup"><span data-stu-id="609ab-172">Use hello IoTHubClient library</span></span>

<span data-ttu-id="609ab-173">Inom hello **iothub\_klienten** mapp i hello [azure-iot-sdk-c](https://github.com/azure/azure-iot-sdk-c) databasen, det finns en **exempel** mapp som innehåller ett program kallas **iothub\_klienten\_exempel\_mqtt**.</span><span class="sxs-lookup"><span data-stu-id="609ab-173">Within hello **iothub\_client** folder in hello [azure-iot-sdk-c](https://github.com/azure/azure-iot-sdk-c) repository, there is a **samples** folder that contains an application called **iothub\_client\_sample\_mqtt**.</span></span>

<span data-ttu-id="609ab-174">hello Windows-versionen av hello **iothub\_klienten\_exempel\_mqtt** program innehåller hello följande Visual Studio-lösning:</span><span class="sxs-lookup"><span data-stu-id="609ab-174">hello Windows version of hello **iothub\_client\_sample\_mqtt** application includes hello following Visual Studio solution:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-mqtt.PNG)

> [!NOTE]
> <span data-ttu-id="609ab-175">Om du öppnar det här projektet i Visual Studio 2017 acceptera hello prompter tooretarget hello projektet toohello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="609ab-175">If you open this project in Visual Studio 2017, accept hello prompts tooretarget hello project toohello latest version.</span></span>

<span data-ttu-id="609ab-176">Den här lösningen innehåller ett projekt.</span><span class="sxs-lookup"><span data-stu-id="609ab-176">This solution contains a single project.</span></span> <span data-ttu-id="609ab-177">Det finns fyra NuGet-paket i den här lösningen:</span><span class="sxs-lookup"><span data-stu-id="609ab-177">There are four NuGet packages installed in this solution:</span></span>

* <span data-ttu-id="609ab-178">Microsoft.Azure.C.SharedUtility</span><span class="sxs-lookup"><span data-stu-id="609ab-178">Microsoft.Azure.C.SharedUtility</span></span>
* <span data-ttu-id="609ab-179">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="609ab-179">Microsoft.Azure.IoTHub.MqttTransport</span></span>
* <span data-ttu-id="609ab-180">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="609ab-180">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
* <span data-ttu-id="609ab-181">Microsoft.Azure.umqtt</span><span class="sxs-lookup"><span data-stu-id="609ab-181">Microsoft.Azure.umqtt</span></span>

<span data-ttu-id="609ab-182">Du måste alltid hello **Microsoft.Azure.C.SharedUtility** när du arbetar med hello SDK-paketet.</span><span class="sxs-lookup"><span data-stu-id="609ab-182">You always need hello **Microsoft.Azure.C.SharedUtility** package when you are working with hello SDK.</span></span> <span data-ttu-id="609ab-183">Det här exemplet använder hello MQTT protokoll, därför måste du inkludera hello **Microsoft.Azure.umqtt** och **Microsoft.Azure.IoTHub.MqttTransport** (det finns motsvarande paket för AMQP och HTTP-paket ).</span><span class="sxs-lookup"><span data-stu-id="609ab-183">This sample uses hello MQTT protocol, therefore you must include hello **Microsoft.Azure.umqtt** and **Microsoft.Azure.IoTHub.MqttTransport** packages (there are equivalent packages for AMQP and HTTP).</span></span> <span data-ttu-id="609ab-184">Eftersom hello används hello **IoTHubClient** bibliotek, måste du också inkludera hello **Microsoft.Azure.IoTHub.IoTHubClient** paketet i din lösning.</span><span class="sxs-lookup"><span data-stu-id="609ab-184">Because hello sample uses hello **IoTHubClient** library, you must also include hello **Microsoft.Azure.IoTHub.IoTHubClient** package in your solution.</span></span>

<span data-ttu-id="609ab-185">Du hittar hello implementering för hello exempelprogrammet i hello **iothub\_klienten\_exempel\_mqtt.c** källfilen.</span><span class="sxs-lookup"><span data-stu-id="609ab-185">You can find hello implementation for hello sample application in hello **iothub\_client\_sample\_mqtt.c** source file.</span></span>

<span data-ttu-id="609ab-186">hello följande använder det här exemplet programmet toowalk du vad krävs toouse hello **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="609ab-186">hello following steps use this sample application toowalk you through what's required toouse hello **IoTHubClient** library.</span></span>

### <a name="initialize-hello-library"></a><span data-ttu-id="609ab-187">Initiera hello-bibliotek</span><span class="sxs-lookup"><span data-stu-id="609ab-187">Initialize hello library</span></span>

> [!NOTE]
> <span data-ttu-id="609ab-188">Innan du börjar arbeta med hello bibliotek måste kanske du måste tooperform vissa plattformsspecifika initiering.</span><span class="sxs-lookup"><span data-stu-id="609ab-188">Before you start working with hello libraries, you may need tooperform some platform-specific initialization.</span></span> <span data-ttu-id="609ab-189">Om du tänker toouse AMQP Linux måste du initiera hello OpenSSL-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="609ab-189">For example, if you plan toouse AMQP on Linux you must initialize hello OpenSSL library.</span></span> <span data-ttu-id="609ab-190">Hej prover i hello [GitHub-lagringsplatsen](https://github.com/Azure/azure-iot-sdk-c) anropa hello verktygsfunktionen **plattform\_init** när hello klienten startar och anropa hello **plattform\_deinit**  funktionen innan du avslutar.</span><span class="sxs-lookup"><span data-stu-id="609ab-190">hello samples in hello [GitHub repository](https://github.com/Azure/azure-iot-sdk-c) call hello utility function **platform\_init** when hello client starts and call hello **platform\_deinit** function before exiting.</span></span> <span data-ttu-id="609ab-191">Dessa funktioner är deklarerad i hello platform.h huvudfilen.</span><span class="sxs-lookup"><span data-stu-id="609ab-191">These functions are declared in hello platform.h header file.</span></span> <span data-ttu-id="609ab-192">Granska hello definitioner av dessa funktioner för målplattformen i hello [databasen](https://github.com/Azure/azure-iot-sdk-c) toodetermine om du behöver tooinclude någon plattformsspecifika initieringskod i din klient.</span><span class="sxs-lookup"><span data-stu-id="609ab-192">Examine hello definitions of these functions for your target platform in hello [repository](https://github.com/Azure/azure-iot-sdk-c) toodetermine whether you need tooinclude any platform-specific initialization code in your client.</span></span>

<span data-ttu-id="609ab-193">Arbeta med hello bibliotek toostart allokera först en referens för IoT-hubb-klienten:</span><span class="sxs-lookup"><span data-stu-id="609ab-193">toostart working with hello libraries, first allocate an IoT Hub client handle:</span></span>

```c
if ((iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol)) == NULL)
{
    (void)printf("ERROR: iotHubClientHandle is NULL!\r\n");
}
else
{
    ...
```

<span data-ttu-id="609ab-194">Du skickar en kopia av hello enheten anslutningssträngen som du fick från hello explorer verktyget toothis enhetsfunktion.</span><span class="sxs-lookup"><span data-stu-id="609ab-194">You pass a copy of hello device connection string you obtained from hello device explorer tool toothis function.</span></span> <span data-ttu-id="609ab-195">Du kan också ange hello kommunikation protokollet toouse.</span><span class="sxs-lookup"><span data-stu-id="609ab-195">You also designate hello communications protocol toouse.</span></span> <span data-ttu-id="609ab-196">Det här exemplet används MQTT men AMQP och HTTP finns också alternativ.</span><span class="sxs-lookup"><span data-stu-id="609ab-196">This example uses MQTT, but AMQP and HTTP are also options.</span></span>

<span data-ttu-id="609ab-197">När du har en giltig **IOTHUB\_klienten\_hantera**, du kan starta anropar hello-API: er toosend och ta emot meddelanden tooand från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="609ab-197">When you have a valid **IOTHUB\_CLIENT\_HANDLE**, you can start calling hello APIs toosend and receive messages tooand from IoT Hub.</span></span>

### <a name="send-messages"></a><span data-ttu-id="609ab-198">Skicka meddelanden</span><span class="sxs-lookup"><span data-stu-id="609ab-198">Send messages</span></span>

<span data-ttu-id="609ab-199">hello exempelprogrammet ställer in en loop toosend meddelanden tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="609ab-199">hello sample application sets up a loop toosend messages tooyour IoT hub.</span></span> <span data-ttu-id="609ab-200">Hej följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="609ab-200">hello following snippet:</span></span>

- <span data-ttu-id="609ab-201">Skapar ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="609ab-201">Creates a message.</span></span>
- <span data-ttu-id="609ab-202">Lägger till en message-egenskap toohello.</span><span class="sxs-lookup"><span data-stu-id="609ab-202">Adds a property toohello message.</span></span>
- <span data-ttu-id="609ab-203">Skickar ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="609ab-203">Sends a message.</span></span>

<span data-ttu-id="609ab-204">Skapa först ett meddelande:</span><span class="sxs-lookup"><span data-stu-id="609ab-204">First, create a message:</span></span>

```c
size_t iterator = 0;
do
{
    if (iterator < MESSAGE_COUNT)
    {
        sprintf_s(msgText, sizeof(msgText), "{\"deviceId\":\"myFirstDevice\",\"windSpeed\":%.2f}", avgWindSpeed + (rand() % 4 + 2));
        if ((messages[iterator].messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText))) == NULL)
        {
            (void)printf("ERROR: iotHubMessageHandle is NULL!\r\n");
        }
        else
        {
            messages[iterator].messageTrackingId = iterator;
            MAP_HANDLE propMap = IoTHubMessage_Properties(messages[iterator].messageHandle);
            (void)sprintf_s(propText, sizeof(propText), "PropMsg_%zu", iterator);
            if (Map_AddOrUpdate(propMap, "PropName", propText) != MAP_OK)
            {
                (void)printf("ERROR: Map_AddOrUpdate Failed!\r\n");
            }

            if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messages[iterator].messageHandle, SendConfirmationCallback, &messages[iterator]) != IOTHUB_CLIENT_OK)
            {
                (void)printf("ERROR: IoTHubClient_LL_SendEventAsync..........FAILED!\r\n");
            }
            else
            {
                (void)printf("IoTHubClient_LL_SendEventAsync accepted message [%d] for transmission tooIoT Hub.\r\n", (int)iterator);
            }
        }
    }
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1);

    iterator++;
} while (g_continueRunning);
```

<span data-ttu-id="609ab-205">Varje gång du skickar ett meddelande kan ange du en referens tooa callback-funktion som anropas när hello data skickas.</span><span class="sxs-lookup"><span data-stu-id="609ab-205">Every time you send a message, you specify a reference tooa callback function that's invoked when hello data is sent.</span></span> <span data-ttu-id="609ab-206">I det här exemplet hello Återanropsfunktionen kallas **SendConfirmationCallback**.</span><span class="sxs-lookup"><span data-stu-id="609ab-206">In this example, hello callback function is called **SendConfirmationCallback**.</span></span> <span data-ttu-id="609ab-207">hello följande fragment visar Återanropsfunktionen:</span><span class="sxs-lookup"><span data-stu-id="609ab-207">hello following snippet shows this callback function:</span></span>

```c
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %zu with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

<span data-ttu-id="609ab-208">Observera hello anropet toohello **IoTHubMessage\_förstöra** fungera när du är klar med hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="609ab-208">Note hello call toohello **IoTHubMessage\_Destroy** function when you're done with hello message.</span></span> <span data-ttu-id="609ab-209">Funktionen frigör hello resurser allokeras när du har skapat hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="609ab-209">This function frees hello resources allocated when you created hello message.</span></span>

### <a name="receive-messages"></a><span data-ttu-id="609ab-210">Ta emot meddelanden</span><span class="sxs-lookup"><span data-stu-id="609ab-210">Receive messages</span></span>

<span data-ttu-id="609ab-211">Ett meddelande är en asynkron åtgärd.</span><span class="sxs-lookup"><span data-stu-id="609ab-211">Receiving a message is an asynchronous operation.</span></span> <span data-ttu-id="609ab-212">Först registrera hello återanrop tooinvoke när hello enheten tar emot ett meddelande:</span><span class="sxs-lookup"><span data-stu-id="609ab-212">First, you register hello callback tooinvoke when hello device receives a message:</span></span>

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext) != IOTHUB_CLIENT_OK)
{
    (void)printf("ERROR: IoTHubClient_LL_SetMessageCallback..........FAILED!\r\n");
}
else
{
    (void)printf("IoTHubClient_LL_SetMessageCallback...successful.\r\n");
...
```

<span data-ttu-id="609ab-213">hello den sista parametern är en ogiltig pekare toowhatever som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="609ab-213">hello last parameter is a void pointer toowhatever you want.</span></span> <span data-ttu-id="609ab-214">I exemplet hello är det en pekare tooan heltal, men det kan vara en pekare tooa mer komplexa datastruktur.</span><span class="sxs-lookup"><span data-stu-id="609ab-214">In hello sample, it's a pointer tooan integer but it could be a pointer tooa more complex data structure.</span></span> <span data-ttu-id="609ab-215">Den här parametern aktiverar hello återanrop funktionen toooperate på delade tillståndet med hello anroparen av den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="609ab-215">This parameter enables hello callback function toooperate on shared state with hello caller of this function.</span></span>

<span data-ttu-id="609ab-216">När hello enheten tar emot ett meddelande, anropas hello registrerade Återanropsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="609ab-216">When hello device receives a message, hello registered callback function is invoked.</span></span> <span data-ttu-id="609ab-217">Den här Återanropsfunktionen hämtar:</span><span class="sxs-lookup"><span data-stu-id="609ab-217">This callback function retrieves:</span></span>

* <span data-ttu-id="609ab-218">hello-meddelande-id och Korrelations-id från hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="609ab-218">hello message id and correlation id from hello message.</span></span>
* <span data-ttu-id="609ab-219">hello meddelandeinnehåll.</span><span class="sxs-lookup"><span data-stu-id="609ab-219">hello message content.</span></span>
* <span data-ttu-id="609ab-220">Eventuella anpassade egenskaper från hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="609ab-220">Any custom properties from hello message.</span></span>

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    MAP_HANDLE mapProperties;
    const char* messageId;
    const char* correlationId;

    // Message properties
    if ((messageId = IoTHubMessage_GetMessageId(message)) == NULL)
    {
        messageId = "<null>";
    }

    if ((correlationId = IoTHubMessage_GetCorrelationId(message)) == NULL)
    {
        correlationId = "<null>";
    }

    // Message content
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        (void)printf("unable tooretrieve hello message data\r\n");
    }
    else
    {
        (void)printf("Received Message [%d]\r\n Message ID: %s\r\n Correlation ID: %s\r\n Data: <<<%.*s>>> & Size=%d\r\n", *counter, messageId, correlationId, (int)size, buffer, (int)size);
        // If we receive hello work 'quit' then we stop running
        if (size == (strlen("quit") * sizeof(char)) && memcmp(buffer, "quit", size) == 0)
        {
            g_continueRunning = false;
        }
    }

    // Retrieve properties from hello message
    mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                size_t index;

                printf(" Message Properties:\r\n");
                for (index = 0; index < propertyCount; index++)
                {
                    (void)printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                (void)printf("\r\n");
            }
        }
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

<span data-ttu-id="609ab-221">Använd hello **IoTHubMessage\_GetByteArray** funktionen tooretrieve hello-meddelande, som i det här exemplet är en sträng.</span><span class="sxs-lookup"><span data-stu-id="609ab-221">Use hello **IoTHubMessage\_GetByteArray** function tooretrieve hello message, which in this example is a string.</span></span>

### <a name="uninitialize-hello-library"></a><span data-ttu-id="609ab-222">Uninitialize hello-bibliotek</span><span class="sxs-lookup"><span data-stu-id="609ab-222">Uninitialize hello library</span></span>

<span data-ttu-id="609ab-223">När du är klar skickar händelser och ta emot meddelanden, kan du uninitialize hello IoT-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="609ab-223">When you're done sending events and receiving messages, you can uninitialize hello IoT library.</span></span> <span data-ttu-id="609ab-224">toodo utfärda så hello följande funktionsanrop:</span><span class="sxs-lookup"><span data-stu-id="609ab-224">toodo so, issue hello following function call:</span></span>

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

<span data-ttu-id="609ab-225">Det här anropet Frigör hello resurser som tidigare har allokerats av hello **IoTHubClient\_CreateFromConnectionString** funktion.</span><span class="sxs-lookup"><span data-stu-id="609ab-225">This call frees up hello resources previously allocated by hello **IoTHubClient\_CreateFromConnectionString** function.</span></span>

<span data-ttu-id="609ab-226">Som du ser det är enkelt toosend och ta emot meddelanden med hello **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="609ab-226">As you can see, it's easy toosend and receive messages with hello **IoTHubClient** library.</span></span> <span data-ttu-id="609ab-227">hello biblioteket hanterar hello information om kommunikation med IoT-hubb, inklusive vilka protokoll toouse (från hello perspektiv hello utvecklare, detta är ett enkelt konfigurationsalternativ).</span><span class="sxs-lookup"><span data-stu-id="609ab-227">hello library handles hello details of communicating with IoT Hub, including which protocol toouse (from hello perspective of hello developer, this is a simple configuration option).</span></span>

<span data-ttu-id="609ab-228">Hej **IoTHubClient** biblioteket innehåller också exakt kontroll över hur tooserialize hello data enheten skickar tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="609ab-228">hello **IoTHubClient** library also provides precise control over how tooserialize hello data your device sends tooIoT Hub.</span></span> <span data-ttu-id="609ab-229">I vissa fall denna behörighet är en fördel, men det är att du inte vill toobe bekymrad i andra.</span><span class="sxs-lookup"><span data-stu-id="609ab-229">In some cases this level of control is an advantage, but in others it is an implementation detail that you don't want toobe concerned with.</span></span> <span data-ttu-id="609ab-230">Om så är fallet hello kan du använda hello **serialiseraren** biblioteket, som beskrivs i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="609ab-230">If that's hello case, you might consider using hello **serializer** library, which is described in hello next section.</span></span>

## <a name="use-hello-serializer-library"></a><span data-ttu-id="609ab-231">Använd hello serialiserare-biblioteket</span><span class="sxs-lookup"><span data-stu-id="609ab-231">Use hello serializer library</span></span>

<span data-ttu-id="609ab-232">Begreppsmässigt hello **serialiseraren** biblioteket placeras ovanpå hello **IoTHubClient** bibliotek i hello SDK.</span><span class="sxs-lookup"><span data-stu-id="609ab-232">Conceptually hello **serializer** library sits on top of hello **IoTHubClient** library in hello SDK.</span></span> <span data-ttu-id="609ab-233">Den använder hello **IoTHubClient** -biblioteket för kommunikation med IoT-hubb, men den underliggande hello lägger till modellering funktioner som tar bort hello belastningen för att behandla meddelande serialisering från hello-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="609ab-233">It uses hello **IoTHubClient** library for hello underlying communication with IoT Hub, but it adds modeling capabilities that remove hello burden of dealing with message serialization from hello developer.</span></span> <span data-ttu-id="609ab-234">Hur framgår fungerar det här biblioteket bästa av ett exempel.</span><span class="sxs-lookup"><span data-stu-id="609ab-234">How this library works is best demonstrated by an example.</span></span>

<span data-ttu-id="609ab-235">I hello **serialiseraren** mapp i hello [azure-iot-sdk-c databasen](https://github.com/Azure/azure-iot-sdk-c), är en **exempel** mapp som innehåller ett program kallas **simplesample \_mqtt**.</span><span class="sxs-lookup"><span data-stu-id="609ab-235">Inside hello **serializer** folder in hello [azure-iot-sdk-c repository](https://github.com/Azure/azure-iot-sdk-c), is a **samples** folder that contains an application called **simplesample\_mqtt**.</span></span> <span data-ttu-id="609ab-236">hello Windows-versionen av det här exemplet innehåller hello följande Visual Studio-lösning:</span><span class="sxs-lookup"><span data-stu-id="609ab-236">hello Windows version of this sample includes hello following Visual Studio solution:</span></span>

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_mqtt.PNG)

> [!NOTE]
> <span data-ttu-id="609ab-237">Om du öppnar det här projektet i Visual Studio 2017 acceptera hello prompter tooretarget hello projektet toohello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="609ab-237">If you open this project in Visual Studio 2017, accept hello prompts tooretarget hello project toohello latest version.</span></span>

<span data-ttu-id="609ab-238">Precis som med hello föregående exempel, innehåller den här flera NuGet-paket:</span><span class="sxs-lookup"><span data-stu-id="609ab-238">As with hello previous sample, this one includes several NuGet packages:</span></span>

* <span data-ttu-id="609ab-239">Microsoft.Azure.C.SharedUtility</span><span class="sxs-lookup"><span data-stu-id="609ab-239">Microsoft.Azure.C.SharedUtility</span></span>
* <span data-ttu-id="609ab-240">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="609ab-240">Microsoft.Azure.IoTHub.MqttTransport</span></span>
* <span data-ttu-id="609ab-241">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="609ab-241">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
* <span data-ttu-id="609ab-242">Microsoft.Azure.IoTHub.Serializer</span><span class="sxs-lookup"><span data-stu-id="609ab-242">Microsoft.Azure.IoTHub.Serializer</span></span>
* <span data-ttu-id="609ab-243">Microsoft.Azure.umqtt</span><span class="sxs-lookup"><span data-stu-id="609ab-243">Microsoft.Azure.umqtt</span></span>

<span data-ttu-id="609ab-244">Du har sett de flesta av dessa paket i hello föregående exempel, men **Microsoft.Azure.IoTHub.Serializer** är ny.</span><span class="sxs-lookup"><span data-stu-id="609ab-244">You've seen most of these packages in hello previous sample, but **Microsoft.Azure.IoTHub.Serializer** is new.</span></span> <span data-ttu-id="609ab-245">Det här paketet måste anges när du använder hello **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="609ab-245">This package is required when you use hello **serializer** library.</span></span>

<span data-ttu-id="609ab-246">Du hittar hello implementering av hello exempelprogrammet i hello **simplesample\_mqtt.c** fil.</span><span class="sxs-lookup"><span data-stu-id="609ab-246">You can find hello implementation of hello sample application in hello **simplesample\_mqtt.c** file.</span></span>

<span data-ttu-id="609ab-247">hello vägleder följande avsnitt dig genom hello viktiga delar av det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="609ab-247">hello following sections walk you through hello key parts of this sample.</span></span>

### <a name="initialize-hello-library"></a><span data-ttu-id="609ab-248">Initiera hello-bibliotek</span><span class="sxs-lookup"><span data-stu-id="609ab-248">Initialize hello library</span></span>

<span data-ttu-id="609ab-249">Arbeta med hello toostart **serialiseraren** bibliotek, anrop hello initieringen API: er:</span><span class="sxs-lookup"><span data-stu-id="609ab-249">toostart working with hello **serializer** library, call hello initialization APIs:</span></span>

```c
if (serializer_init(NULL) != SERIALIZER_OK)
{
    (void)printf("Failed on serializer_init\r\n");
}
else
{
    IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol);
    srand((unsigned int)time(NULL));
    int avgWindSpeed = 10;

    if (iotHubClientHandle == NULL)
    {
        (void)printf("Failed on IoTHubClient_LL_Create\r\n");
    }
    else
    {
        ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
        if (myWeather == NULL)
        {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
        }
        else
        {
...
```

<span data-ttu-id="609ab-250">hello anropet toohello **serialiseraren\_init** funktionen är en enstaka anrop och initierar hello underliggande bibliotek.</span><span class="sxs-lookup"><span data-stu-id="609ab-250">hello call toohello **serializer\_init** function is a one-time call and initializes hello underlying library.</span></span> <span data-ttu-id="609ab-251">Anropa sedan hello **IoTHubClient\_lla\_CreateFromConnectionString** funktion, vilket är hello samma API som hello **IoTHubClient** exempel.</span><span class="sxs-lookup"><span data-stu-id="609ab-251">Then, you call hello **IoTHubClient\_LL\_CreateFromConnectionString** function, which is hello same API as in hello **IoTHubClient** sample.</span></span> <span data-ttu-id="609ab-252">Det här anropet anger anslutningssträngen enhet (det här anropet är också där du väljer hello-protokollet du vill toouse).</span><span class="sxs-lookup"><span data-stu-id="609ab-252">This call sets your device connection string (this call is also where you choose hello protocol you want toouse).</span></span> <span data-ttu-id="609ab-253">Det här exemplet används MQTT som hello transport, men kan använda AMQP eller HTTP.</span><span class="sxs-lookup"><span data-stu-id="609ab-253">This sample uses MQTT as hello transport, but could use AMQP or HTTP.</span></span>

<span data-ttu-id="609ab-254">Slutligen anropa hello **skapa\_MODELLEN\_instans** funktion.</span><span class="sxs-lookup"><span data-stu-id="609ab-254">Finally, call hello **CREATE\_MODEL\_INSTANCE** function.</span></span> <span data-ttu-id="609ab-255">**WeatherStation** hello namnområde för hello modellen och **ContosoAnemometer** är hello namnet på hello modell.</span><span class="sxs-lookup"><span data-stu-id="609ab-255">**WeatherStation** is hello namespace of hello model and **ContosoAnemometer** is hello name of hello model.</span></span> <span data-ttu-id="609ab-256">När du har skapat hello modellen instans kan du använda den toostart skicka och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="609ab-256">Once hello model instance is created, you can use it toostart sending and receiving messages.</span></span> <span data-ttu-id="609ab-257">Det är dock viktigt toounderstand vilka modellen.</span><span class="sxs-lookup"><span data-stu-id="609ab-257">However, it's important toounderstand what a model is.</span></span>

### <a name="define-hello-model"></a><span data-ttu-id="609ab-258">Definiera hello modellen</span><span class="sxs-lookup"><span data-stu-id="609ab-258">Define hello model</span></span>

<span data-ttu-id="609ab-259">En modell i hello **serialiseraren** biblioteket definierar hälsningsmeddelande som enheten kan skicka tooIoT NAV- och hello meddelanden, som kallas *åtgärder* i hello modeling språk, vilket kan ta emot.</span><span class="sxs-lookup"><span data-stu-id="609ab-259">A model in hello **serializer** library defines hello messages that your device can send tooIoT Hub and hello messages, called *actions* in hello modeling language, which it can receive.</span></span> <span data-ttu-id="609ab-260">Du definierar en modell med en uppsättning C makron som hello **simplesample\_mqtt** exempelprogrammet:</span><span class="sxs-lookup"><span data-stu-id="609ab-260">You define a model using a set of C macros as in hello **simplesample\_mqtt** sample application:</span></span>

```c
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(int, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

<span data-ttu-id="609ab-261">Hej **börjar\_namnområde** och **END\_namnområde** makron båda tar hello namnområdet för hello modellen som ett argument.</span><span class="sxs-lookup"><span data-stu-id="609ab-261">hello **BEGIN\_NAMESPACE** and **END\_NAMESPACE** macros both take hello namespace of hello model as an argument.</span></span> <span data-ttu-id="609ab-262">Det förväntas att något mellan dessa makron är hello definition av din modell eller modeller och hello datastrukturer som använder hello modeller.</span><span class="sxs-lookup"><span data-stu-id="609ab-262">It's expected that anything between these macros is hello definition of your model or models, and hello data structures that hello models use.</span></span>

<span data-ttu-id="609ab-263">I det här exemplet är en modell kallas **ContosoAnemometer**.</span><span class="sxs-lookup"><span data-stu-id="609ab-263">In this example, there is a single model called **ContosoAnemometer**.</span></span> <span data-ttu-id="609ab-264">Den här modellen definierar två datadelar att enheten kan skicka tooIoT hubb: **DeviceId** och **vindhastigheten**.</span><span class="sxs-lookup"><span data-stu-id="609ab-264">This model defines two pieces of data that your device can send tooIoT Hub: **DeviceId** and **WindSpeed**.</span></span> <span data-ttu-id="609ab-265">Den definierar även tre åtgärder (meddelanden) som kan ta emot din enhet: **TurnFanOn**, **TurnFanOff**, och **SetAirResistance**.</span><span class="sxs-lookup"><span data-stu-id="609ab-265">It also defines three actions (messages) that your device can receive: **TurnFanOn**, **TurnFanOff**, and **SetAirResistance**.</span></span> <span data-ttu-id="609ab-266">Alla dataposter har en typ och varje åtgärd har ett namn (och eventuellt en uppsättning parametrar).</span><span class="sxs-lookup"><span data-stu-id="609ab-266">Each data element has a type, and each action has a name (and optionally a set of parameters).</span></span>

<span data-ttu-id="609ab-267">Definiera en API-yta som du kan använda toosend meddelanden tooIoT hubb och svara toomessages skickas toohello enheten hello data och åtgärder som definierats i hello modellen.</span><span class="sxs-lookup"><span data-stu-id="609ab-267">hello data and actions defined in hello model define an API surface that you can use toosend messages tooIoT Hub, and respond toomessages sent toohello device.</span></span> <span data-ttu-id="609ab-268">Användning av den här modellen är bäst att förstå genom ett exempel.</span><span class="sxs-lookup"><span data-stu-id="609ab-268">Use of this model is best understood through an example.</span></span>

### <a name="send-messages"></a><span data-ttu-id="609ab-269">Skicka meddelanden</span><span class="sxs-lookup"><span data-stu-id="609ab-269">Send messages</span></span>

<span data-ttu-id="609ab-270">hello modellen definierar hello data som du kan skicka tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="609ab-270">hello model defines hello data you can send tooIoT Hub.</span></span> <span data-ttu-id="609ab-271">I det här exemplet som innebär att en av hello två dataobjekt som definieras med hjälp av hello **WITH_DATA** makro.</span><span class="sxs-lookup"><span data-stu-id="609ab-271">In this example, that means one of hello two data items defined using hello **WITH_DATA** macro.</span></span> <span data-ttu-id="609ab-272">Det finns flera steg krävs toosend **DeviceId** och **vindhastigheten** värden tooan IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="609ab-272">There are several steps required toosend **DeviceId** and **WindSpeed** values tooan IoT hub.</span></span> <span data-ttu-id="609ab-273">hello är först tooset hello data som du vill toosend:</span><span class="sxs-lookup"><span data-stu-id="609ab-273">hello first is tooset hello data you want toosend:</span></span>

```c
myWeather->DeviceId = "myFirstDevice";
myWeather->WindSpeed = avgWindSpeed + (rand() % 4 + 2);
```

<span data-ttu-id="609ab-274">hello modell som du angav tidigare kan du tooset hello värden genom att ange medlemmar i en **struct**.</span><span class="sxs-lookup"><span data-stu-id="609ab-274">hello model you defined earlier enables you tooset hello values by setting members of a **struct**.</span></span> <span data-ttu-id="609ab-275">Därefter serialisera hello-meddelande som du vill toosend:</span><span class="sxs-lookup"><span data-stu-id="609ab-275">Next, serialize hello message you want toosend:</span></span>

```c
unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, myWeather->DeviceId, myWeather->WindSpeed) != CODEFIRST_OK)
{
    (void)printf("Failed tooserialize\r\n");
}
else
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
    free(destination);
}
```

<span data-ttu-id="609ab-276">Den här koden Serialiserar hello enhet till moln tooa buffert (refereras av **mål**).</span><span class="sxs-lookup"><span data-stu-id="609ab-276">This code serializes hello device-to-cloud tooa buffer (referenced by **destination**).</span></span> <span data-ttu-id="609ab-277">hello koden anropar sedan hello **sendMessage** fungerar toosend hello meddelandet tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="609ab-277">hello code then invokes hello **sendMessage** function toosend hello message tooIoT Hub:</span></span>

```c
static void sendMessage(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle == NULL)
    {
        printf("unable toocreate a new IoTHubMessage\r\n");
    }
    else
    {
        if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed toohand over hello message tooIoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted hello message for delivery\r\n");
        }
        IoTHubMessage_Destroy(messageHandle);
    }
    messageTrackingId++;
}
```


<span data-ttu-id="609ab-278">Hej andra toolast-parametern för **IoTHubClient\_lla\_SendEventAsync** är en referens tooa callback-funktion som anropas när hello data har skickat.</span><span class="sxs-lookup"><span data-stu-id="609ab-278">hello second toolast parameter of **IoTHubClient\_LL\_SendEventAsync** is a reference tooa callback function that's called when hello data is successfully sent.</span></span> <span data-ttu-id="609ab-279">Här är hello Återanropsfunktionen i hello exemplet:</span><span class="sxs-lookup"><span data-stu-id="609ab-279">Here's hello callback function in hello sample:</span></span>

```c
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    unsigned int messageTrackingId = (unsigned int)(uintptr_t)userContextCallback;

    (void)printf("Message Id: %u Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

<span data-ttu-id="609ab-280">hello andra parametern är en pekare toouser kontext. hello samma pekaren som skickades för**IoTHubClient\_lla\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="609ab-280">hello second parameter is a pointer toouser context; hello same pointer passed too**IoTHubClient\_LL\_SendEventAsync**.</span></span> <span data-ttu-id="609ab-281">I det här fallet hello kontext är en enkel räknare, men det kan vara vad du vill.</span><span class="sxs-lookup"><span data-stu-id="609ab-281">In this case, hello context is a simple counter, but it can be anything you want.</span></span>

<span data-ttu-id="609ab-282">Det är allt finns toosending meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="609ab-282">That's all there is toosending device-to-cloud messages.</span></span> <span data-ttu-id="609ab-283">hello endast återstår toocover är hur tooreceive meddelanden.</span><span class="sxs-lookup"><span data-stu-id="609ab-283">hello only thing left toocover is how tooreceive messages.</span></span>

### <a name="receive-messages"></a><span data-ttu-id="609ab-284">Ta emot meddelanden</span><span class="sxs-lookup"><span data-stu-id="609ab-284">Receive messages</span></span>

<span data-ttu-id="609ab-285">Ett meddelande fungerar på liknande sätt toohello arbetsmetoder meddelanden i hello **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="609ab-285">Receiving a message works similarly toohello way messages work in hello **IoTHubClient** library.</span></span> <span data-ttu-id="609ab-286">Först måste registrera du ett meddelande Återanropsfunktionen:</span><span class="sxs-lookup"><span data-stu-id="609ab-286">First, you register a message callback function:</span></span>

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather) != IOTHUB_CLIENT_OK)
{
    printf("unable tooIoTHubClient_SetMessageCallback\r\n");
}
else
{
...
```

<span data-ttu-id="609ab-287">Sedan kan skriva du hello återanropsfunktion som anropas när ett meddelande tas emot:</span><span class="sxs-lookup"><span data-stu-id="609ab-287">Then, you write hello callback function that's invoked when a message is received:</span></span>

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable tooIoTHubMessage_GetByteArray\r\n");
        result = IOTHUBMESSAGE_ABANDONED;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed toomalloc\r\n");
            result = IOTHUBMESSAGE_ABANDONED;
        }
        else
        {
            (void)memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

<span data-ttu-id="609ab-288">Den här koden är formaterad – det har hello samma för alla lösningar.</span><span class="sxs-lookup"><span data-stu-id="609ab-288">This code is boilerplate -- it's hello same for any solution.</span></span> <span data-ttu-id="609ab-289">Den här funktionen tar emot hello-meddelande och tar hand om vidarebefordring toohello önskad funktion via hello anrop för**kör\_kommandot**.</span><span class="sxs-lookup"><span data-stu-id="609ab-289">This function receives hello message and takes care of routing it toohello appropriate function through hello call too**EXECUTE\_COMMAND**.</span></span> <span data-ttu-id="609ab-290">hello-funktionen anropas beroende nu hello definition av hello åtgärder i din modell.</span><span class="sxs-lookup"><span data-stu-id="609ab-290">hello function called at this point depends on hello definition of hello actions in your model.</span></span>

<span data-ttu-id="609ab-291">När du definierar en åtgärd i din modell kan du är obligatoriska tooimplement en funktion som anropas när enheten tar emot motsvarande hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="609ab-291">When you define an action in your model, you're required tooimplement a function that's called when your device receives hello corresponding message.</span></span> <span data-ttu-id="609ab-292">Om till exempel din modell definierar den här åtgärden:</span><span class="sxs-lookup"><span data-stu-id="609ab-292">For example, if your model defines this action:</span></span>

```c
WITH_ACTION(SetAirResistance, int, Position)
```

<span data-ttu-id="609ab-293">Definiera en funktion med den här signaturen:</span><span class="sxs-lookup"><span data-stu-id="609ab-293">Define a function with this signature:</span></span>

```c
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position too%d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

<span data-ttu-id="609ab-294">Observera hur hello funktionen hello namn matchar hello namnet på hello åtgärd i hello modellen och att hello hello-funktionen matchar hello parametrar angavs för hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="609ab-294">Note how hello name of hello function matches hello name of hello action in hello model and that hello parameters of hello function match hello parameters specified for hello action.</span></span> <span data-ttu-id="609ab-295">hello första parametern krävs alltid och innehåller en pekare toohello instans av din modell.</span><span class="sxs-lookup"><span data-stu-id="609ab-295">hello first parameter is always required and contains a pointer toohello instance of your model.</span></span>

<span data-ttu-id="609ab-296">När hello enheten tar emot ett meddelande som matchar denna signatur, anropas motsvarande hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="609ab-296">When hello device receives a message that matches this signature, hello corresponding function is called.</span></span> <span data-ttu-id="609ab-297">Därför utöver med tooinclude hello formaterad exempelkod från **IoTHubMessage**, ta emot meddelanden är bara en fråga för att definiera en enkel funktion för varje åtgärd som definierats i din modell.</span><span class="sxs-lookup"><span data-stu-id="609ab-297">Therefore, aside from having tooinclude hello boilerplate code from **IoTHubMessage**, receiving messages is just a matter of defining a simple function for each action defined in your model.</span></span>

### <a name="uninitialize-hello-library"></a><span data-ttu-id="609ab-298">Uninitialize hello-bibliotek</span><span class="sxs-lookup"><span data-stu-id="609ab-298">Uninitialize hello library</span></span>

<span data-ttu-id="609ab-299">När du är klar skickar data och ta emot meddelanden, kan du uninitialize hello IoT-biblioteket:</span><span class="sxs-lookup"><span data-stu-id="609ab-299">When you're done sending data and receiving messages, you can uninitialize hello IoT library:</span></span>

```c
...
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_LL_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

<span data-ttu-id="609ab-300">Dessa tre funktioner justerar hello tre initieringen funktioner som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="609ab-300">Each of these three functions aligns with hello three initialization functions described previously.</span></span> <span data-ttu-id="609ab-301">Anropar API: erna säkerställer du frigöra tidigare allokerade resurser.</span><span class="sxs-lookup"><span data-stu-id="609ab-301">Calling these APIs ensures that you free previously allocated resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="609ab-302">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="609ab-302">Next Steps</span></span>

<span data-ttu-id="609ab-303">Den här artikeln beskrivs hello grunderna i hello-biblioteken i hello **Azure IoT-enhet SDK för C**. Det medföljer tillräckligt med information toounderstand vad som ingår i hello SDK, dess arkitektur och hur tooget igång med hello Windows prover.</span><span class="sxs-lookup"><span data-stu-id="609ab-303">This article covered hello basics of using hello libraries in hello **Azure IoT device SDK for C**. It provided you with enough information toounderstand what's included in hello SDK, its architecture, and how tooget started working with hello Windows samples.</span></span> <span data-ttu-id="609ab-304">hello nästa artikel fortsätter hello beskrivning av hello SDK genom att förklara [mer om hello IoTHubClient biblioteket](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="609ab-304">hello next article continues hello description of hello SDK by explaining [more about hello IoTHubClient library](iot-hub-device-sdk-c-iothubclient.md).</span></span>

<span data-ttu-id="609ab-305">toolearn mer information om hur du utvecklar för IoT-hubb finns hello [Azure IoT SDK][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="609ab-305">toolearn more about developing for IoT Hub, see hello [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="609ab-306">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="609ab-306">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="609ab-307">[Simulera en enhet med Azure IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="609ab-307">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
