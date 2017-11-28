---
title: aaaConnect gateway-tooAzure IoT Suite med en Intel NUC | Microsoft Docs
description: "Använd hello Microsoft IoT kommersiella Gateway Kit och hello remote förkonfigurerade övervakningslösning. Använd hello Azure IoT Edge gateway tooconnect toohello remote övervakningslösning, skicka simulerade telemetri toohello molnet och besvara toomethods anropas från hello lösning instrumentpanelen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 46b545fc21b054191c8f78ace20fc628f839a819
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-toohello-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a><span data-ttu-id="435a2-104">Anslut dina Azure IoT Edge gateway toohello remote förkonfigurerade övervakningslösning och skicka telemetri om simulerad</span><span class="sxs-lookup"><span data-stu-id="435a2-104">Connect your Azure IoT Edge gateway toohello remote monitoring preconfigured solution and send simulated telemetry</span></span>

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

<span data-ttu-id="435a2-105">Den här kursen visar hur toouse Azure IoT kant toosimulate temperatur- och fuktighetskonsekvens data toosend toohello fjärrövervaknings förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="435a2-105">This tutorial shows you how toouse Azure IoT Edge toosimulate temperature and humidity data toosend toohello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="435a2-106">hello självstudiekursen används:</span><span class="sxs-lookup"><span data-stu-id="435a2-106">hello tutorial uses:</span></span>

- <span data-ttu-id="435a2-107">Azure IoT kant tooimplement en exempel-gateway.</span><span class="sxs-lookup"><span data-stu-id="435a2-107">Azure IoT Edge tooimplement a sample gateway.</span></span>
- <span data-ttu-id="435a2-108">Hej IoT Suite fjärrövervaknings förkonfigurerade lösning som hello molnbaserade serverdel.</span><span class="sxs-lookup"><span data-stu-id="435a2-108">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="435a2-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="435a2-109">Overview</span></span>

<span data-ttu-id="435a2-110">Du har slutfört hello följa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="435a2-110">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="435a2-111">Distribuera en instans av enligt förkonfigurerade lösningen tooyour i hello fjärråtkomst övervakning Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="435a2-111">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="435a2-112">Det här steget kan du automatiskt distribuerar och konfigurerar Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="435a2-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="435a2-113">Ställ in Intel NUC gateway-enhet toocommunicate med datorn och hello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="435a2-113">Set up your Intel NUC gateway device toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="435a2-114">Konfigurera hello IoT Edge gateway toosend simulerade telemetri som kan visas på instrumentpanelen för hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="435a2-114">Configure hello IoT Edge gateway toosend simulated telemetry that you can view on hello solution dashboard.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="435a2-115">hello fjärråtkomst övervakning lösning tillhandahåller en uppsättning Azure-tjänster i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="435a2-115">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="435a2-116">hello distribution visar en verklig enterprise-arkitektur.</span><span class="sxs-lookup"><span data-stu-id="435a2-116">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="435a2-117">tooavoid onödiga Azure-förbrukningen kostnader, ta bort din instans av hello förkonfigurerade lösningen i azureiotsuite.com när du är klar med den.</span><span class="sxs-lookup"><span data-stu-id="435a2-117">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="435a2-118">Om du behöver hello förkonfigurerade lösningen igen, kan du enkelt återskapa den.</span><span class="sxs-lookup"><span data-stu-id="435a2-118">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="435a2-119">Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="435a2-119">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

<span data-ttu-id="435a2-120">Upprepa hello föregående steg tooadd en andra enhet med enhets-ID, till exempel **device02**.</span><span class="sxs-lookup"><span data-stu-id="435a2-120">Repeat hello previous steps tooadd a second device using a Device ID such as **device02**.</span></span> <span data-ttu-id="435a2-121">hello exemplet skickar data från två simulerade enheter i hello gateway toohello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="435a2-121">hello sample sends data from two simulated devices in hello gateway toohello remote monitoring solution.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-hello-custom-iot-edge-module"></a><span data-ttu-id="435a2-122">Skapa hello anpassad IoT kant-modul</span><span class="sxs-lookup"><span data-stu-id="435a2-122">Build hello custom IoT Edge module</span></span>

<span data-ttu-id="435a2-123">Du kan nu skapa hello anpassad IoT kant-modul som möjliggör hello gateway toosend meddelanden toohello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="435a2-123">You can now build hello custom IoT Edge module that enables hello gateway toosend messages toohello remote monitoring solution.</span></span> <span data-ttu-id="435a2-124">Mer information om hur du konfigurerar en gateway- och IoT-Edge moduler finns [Azure IoT kant begrepp][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="435a2-124">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

<span data-ttu-id="435a2-125">Hämta hello källkoden för hello anpassade IoT kant moduler från GitHub använder hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="435a2-125">Download hello source code for hello custom IoT Edge modules from GitHub using hello following commands:</span></span>

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

<span data-ttu-id="435a2-126">Skapa hello anpassade IoT kant-modul med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="435a2-126">Build hello custom IoT Edge module using hello following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

<span data-ttu-id="435a2-127">hello build-skript placerar hello libsimulator.so anpassad IoT kant-modul i hello build-mappen.</span><span class="sxs-lookup"><span data-stu-id="435a2-127">hello build script places hello libsimulator.so custom IoT Edge module in hello build folder.</span></span>

## <a name="configure-and-run-hello-iot-edge-gateway"></a><span data-ttu-id="435a2-128">Konfigurera och köra hello IoT gräns-gatewayen</span><span class="sxs-lookup"><span data-stu-id="435a2-128">Configure and run hello IoT Edge gateway</span></span>

<span data-ttu-id="435a2-129">Du kan nu konfigurera hello IoT Edge gateway toosend simulerade telemetri tooyour remote instrumentpanelen för övervakning.</span><span class="sxs-lookup"><span data-stu-id="435a2-129">You can now configure hello IoT Edge gateway toosend simulated telemetry tooyour remote monitoring dashboard.</span></span> <span data-ttu-id="435a2-130">Mer information om hur du konfigurerar en gateway- och IoT-Edge moduler finns [Azure IoT kant begrepp][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="435a2-130">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

> [!TIP]
> <span data-ttu-id="435a2-131">I den här kursen använder du hello standard `vi` textredigerare på hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="435a2-131">In this tutorial, you use hello standard `vi` text editor on hello Intel NUC.</span></span> <span data-ttu-id="435a2-132">Om du inte har använt `vi` innan, bör du genomföra en inledande vägledning som [Unix - hello vi Editor kursen] [ lnk-vi-tutorial] toofamiliarize dig med den här redigeraren.</span><span class="sxs-lookup"><span data-stu-id="435a2-132">If you have not used `vi` before, you should complete an introductory tutorial, such as [Unix - hello vi Editor Tutorial][lnk-vi-tutorial] toofamiliarize yourself with this editor.</span></span> <span data-ttu-id="435a2-133">Du kan också installera hello mer användarvänlig [nano](https://www.nano-editor.org/) editor hello kommandot `smart install nano -y`.</span><span class="sxs-lookup"><span data-stu-id="435a2-133">Alternatively, you can install hello more user-friendly [nano](https://www.nano-editor.org/) editor using hello command `smart install nano -y`.</span></span>

<span data-ttu-id="435a2-134">Öppna hello exempelkonfigurationsfilen i hello **vi** redigeraren med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="435a2-134">Open hello sample configuration file in hello **vi** editor using hello following command:</span></span>

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
```

<span data-ttu-id="435a2-135">Leta upp följande rader i hello konfiguration för hello IoTHub modulen hello:</span><span class="sxs-lookup"><span data-stu-id="435a2-135">Locate hello following lines in hello configuration for hello IoTHub module:</span></span>

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

<span data-ttu-id="435a2-136">Ersätt hello platshållare för värden med hello IoT-hubb information du skapat och sparat på hello början av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="435a2-136">Replace hello placeholder values with hello IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="435a2-137">hello-värdet för IoTHubName ser ut som **yourrmsolution37e08**, och hello värdet för IoTSuffix är vanligtvis **azure devices.net**.</span><span class="sxs-lookup"><span data-stu-id="435a2-137">hello value for IoTHubName looks like **yourrmsolution37e08**, and hello value for IoTSuffix is typically **azure-devices.net**.</span></span>

<span data-ttu-id="435a2-138">Leta upp följande rader i hello konfiguration för hello mappningsmodul hello:</span><span class="sxs-lookup"><span data-stu-id="435a2-138">Locate hello following lines in hello configuration for hello mapping module:</span></span>

```json
args": [
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  },
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

<span data-ttu-id="435a2-139">Ersätt hello **deviceID** och **deviceKey** platshållarna med hello-ID: N och nycklarna för hello två enheter som du skapade tidigare i hello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="435a2-139">Replace hello **deviceID** and **deviceKey** placeholders with hello IDs and keys for hello two devices you created in hello remote monitoring solution previously.</span></span>

<span data-ttu-id="435a2-140">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="435a2-140">Save your changes.</span></span>

<span data-ttu-id="435a2-141">Du kan nu köra hello IoT gräns-gatewayen med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="435a2-141">You can now run hello IoT Edge gateway using hello following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

<span data-ttu-id="435a2-142">hello gateway startar på hello Intel NUC och skickar simulerade telemetri toohello remote övervakningslösning:</span><span class="sxs-lookup"><span data-stu-id="435a2-142">hello gateway starts on hello Intel NUC and sends simulated telemetry toohello remote monitoring solution:</span></span>

![IoT-gräns-gatewayen genererar simulerade telemetri][img-simulated telemetry]

<span data-ttu-id="435a2-144">Tryck på **Ctrl-C** tooexit hello program när som helst.</span><span class="sxs-lookup"><span data-stu-id="435a2-144">Press **Ctrl-C** tooexit hello program at any time.</span></span>

## <a name="view-hello-telemetry"></a><span data-ttu-id="435a2-145">Visa hello telemetri</span><span class="sxs-lookup"><span data-stu-id="435a2-145">View hello telemetry</span></span>

<span data-ttu-id="435a2-146">Hej IoT gräns-gatewayen nu skickar simulerade telemetri toohello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="435a2-146">hello IoT Edge gateway is now sending simulated telemetry toohello remote monitoring solution.</span></span> <span data-ttu-id="435a2-147">Du kan visa hello telemetri på instrumentpanelen för hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="435a2-147">You can view hello telemetry on hello solution dashboard.</span></span>

- <span data-ttu-id="435a2-148">Navigera toohello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="435a2-148">Navigate toohello solution dashboard.</span></span>
- <span data-ttu-id="435a2-149">Välj en av hello två enheter som du konfigurerade i hello gateway i hello **enhet tooView** listrutan.</span><span class="sxs-lookup"><span data-stu-id="435a2-149">Select one of hello two devices you configured in hello gateway in hello **Device tooView** dropdown.</span></span>
- <span data-ttu-id="435a2-150">hello telemetri från hello gatewayenheter visar hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="435a2-150">hello telemetry from hello gateway devices displays on hello dashboard.</span></span>

![Visa telemetri från hello simulerade gateway-enheter][img-telemetry-display]

> [!WARNING]
> <span data-ttu-id="435a2-152">Om du lämnar hello remote övervakningslösning som körs i ditt Azure-konto, debiteras du för hello gång den körs.</span><span class="sxs-lookup"><span data-stu-id="435a2-152">If you leave hello remote monitoring solution running in your Azure account, you are billed for hello time it runs.</span></span> <span data-ttu-id="435a2-153">Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="435a2-153">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="435a2-154">Ta bort hello förkonfigurerade lösningen från ditt Azure-konto när du är klar.</span><span class="sxs-lookup"><span data-stu-id="435a2-154">Delete hello preconfigured solution from your Azure account when you have finished using it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="435a2-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="435a2-155">Next steps</span></span>

<span data-ttu-id="435a2-156">Besök hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="435a2-156">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-simulated telemetry]: ./media/iot-suite-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started