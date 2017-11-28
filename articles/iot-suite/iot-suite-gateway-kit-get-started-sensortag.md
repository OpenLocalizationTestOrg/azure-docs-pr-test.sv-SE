---
title: aaaConnect gateway-tooAzure IoT Suite med en Intel NUC | Microsoft Docs
description: "Använd hello Microsoft IoT kommersiella Gateway Kit och hello remote förkonfigurerade övervakningslösning. Använd hello Azure IoT Edge gateway tooenable en SensorTag enhet tooconnect toohello remote övervakningslösning, skicka telemetri toohello molnet och åtgärda toomethods anropas från hello lösning instrumentpanelen."
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
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: 6f98ee3c1e2311a8644da9d72d40e671e7cbcf00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-toohello-remote-monitoring-preconfigured-solution-and-send-telemetry-from-a-sensortag"></a><span data-ttu-id="a9061-104">Anslut dina Azure IoT Edge gateway toohello remote förkonfigurerade övervakningslösning och skicka telemetri från en SensorTag</span><span class="sxs-lookup"><span data-stu-id="a9061-104">Connect your Azure IoT Edge gateway toohello remote monitoring preconfigured solution and send telemetry from a SensorTag</span></span>

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

<span data-ttu-id="a9061-105">Den här kursen visar hur toouse Azure IoT kant toosend temperatur- och fuktighetskonsekvens data från SensorTag toohello remote enhetsövervakning förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="a9061-105">This tutorial shows you how toouse Azure IoT Edge toosend temperature and humidity data from SensorTag device toohello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="a9061-106">Hej SensorTag ansluter toohello Intel NUC gateway med hjälp av Bluetooth.</span><span class="sxs-lookup"><span data-stu-id="a9061-106">hello SensorTag connects toohello Intel NUC gateway using Bluetooth.</span></span> <span data-ttu-id="a9061-107">hello självstudiekursen används:</span><span class="sxs-lookup"><span data-stu-id="a9061-107">hello tutorial uses:</span></span>

- <span data-ttu-id="a9061-108">Azure IoT kant tooimplement en exempel-gateway.</span><span class="sxs-lookup"><span data-stu-id="a9061-108">Azure IoT Edge tooimplement a sample gateway.</span></span>
- <span data-ttu-id="a9061-109">Hej IoT Suite fjärrövervaknings förkonfigurerade lösning som hello molnbaserade serverdel.</span><span class="sxs-lookup"><span data-stu-id="a9061-109">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="a9061-110">Översikt</span><span class="sxs-lookup"><span data-stu-id="a9061-110">Overview</span></span>

<span data-ttu-id="a9061-111">Du har slutfört hello följa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="a9061-111">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="a9061-112">Distribuera en instans av enligt förkonfigurerade lösningen tooyour i hello fjärråtkomst övervakning Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a9061-112">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="a9061-113">Det här steget kan du automatiskt distribuerar och konfigurerar Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="a9061-113">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="a9061-114">Ställ in Intel NUC gateway-enhet toocommunicate med datorn och hello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="a9061-114">Set up your Intel NUC gateway device toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="a9061-115">Konfigurera din Intel NUC gateway tooreceive telemetri från en SensorTag-enhet och skicka den toohello fjärråtkomst övervakning instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="a9061-115">Set up your Intel NUC gateway tooreceive telemetry from a SensorTag device and send it toohello remote monitoring dashboard.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

<span data-ttu-id="a9061-116">[Texas Instruments TIVERA SensorTag][lnk-sensortag].</span><span class="sxs-lookup"><span data-stu-id="a9061-116">[Texas Instruments BLE SensorTag][lnk-sensortag].</span></span> <span data-ttu-id="a9061-117">Den här självstudiekursen hämtar telemetridata via Bluetooth från hello SensorTag enhet.</span><span class="sxs-lookup"><span data-stu-id="a9061-117">This tutorial retrieves telemetry data over Bluetooth from hello SensorTag device.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="a9061-118">hello fjärråtkomst övervakning lösning tillhandahåller en uppsättning Azure-tjänster i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a9061-118">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="a9061-119">hello distribution visar en verklig enterprise-arkitektur.</span><span class="sxs-lookup"><span data-stu-id="a9061-119">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="a9061-120">tooavoid onödiga Azure-förbrukningen kostnader, ta bort din instans av hello förkonfigurerade lösningen i azureiotsuite.com när du är klar med den.</span><span class="sxs-lookup"><span data-stu-id="a9061-120">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="a9061-121">Om du behöver hello förkonfigurerade lösningen igen, kan du enkelt återskapa den.</span><span class="sxs-lookup"><span data-stu-id="a9061-121">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="a9061-122">Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="a9061-122">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

## <a name="configure-bluetooth-connectivity"></a><span data-ttu-id="a9061-123">Konfigurera Bluetooth-anslutning</span><span class="sxs-lookup"><span data-stu-id="a9061-123">Configure Bluetooth connectivity</span></span>

<span data-ttu-id="a9061-124">Konfigurerar Bluetooth på hello Intel NUC tooenable hello SensorTag enhet tooconnect och skicka telemetri.</span><span class="sxs-lookup"><span data-stu-id="a9061-124">Configure Bluetooth on hello Intel NUC tooenable hello SensorTag device tooconnect and send telemetry.</span></span>

### <a name="find-hello-mac-address-of-hello-sensortag"></a><span data-ttu-id="a9061-125">Hitta hello MAC-adressen för hello SensorTag</span><span class="sxs-lookup"><span data-stu-id="a9061-125">Find hello MAC address of hello SensorTag</span></span>

1. <span data-ttu-id="a9061-126">Hello shell på hello Intel NUC, kör hello efter kommandot toounblock hello Bluetooth-tjänst:</span><span class="sxs-lookup"><span data-stu-id="a9061-126">In hello shell on hello Intel NUC, run hello following command toounblock hello Bluetooth service:</span></span>

    ```bash
    sudo rfkill unblock bluetooth
    ```

1. <span data-ttu-id="a9061-127">Kör hello följande kommandon toostart hello Bluetooth-tjänsten på hello Intel NUC och ange hello Bluetooth shell:</span><span class="sxs-lookup"><span data-stu-id="a9061-127">Run hello following commands toostart hello Bluetooth service on hello Intel NUC and enter hello Bluetooth shell:</span></span>

    ```bash
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. <span data-ttu-id="a9061-128">Kör hello efter kommandot toopower på hello Bluetooth domänkontrollant:</span><span class="sxs-lookup"><span data-stu-id="a9061-128">Run hello following command toopower on hello Bluetooth controller:</span></span>

    ```bash
    power on
    ```

    <span data-ttu-id="a9061-129">När hello domänkontrollant är aktiverat kan du se ett meddelande **ändra power på lyckades**.</span><span class="sxs-lookup"><span data-stu-id="a9061-129">When hello controller is on, you see a message **Changing power on succeeded**.</span></span>

1. <span data-ttu-id="a9061-130">Kör hello efter kommandot tooscan för närliggande Bluetooth-enheter:</span><span class="sxs-lookup"><span data-stu-id="a9061-130">Run hello following command tooscan for nearby Bluetooth devices:</span></span>

    ```bash
    scan on
    ```

1. <span data-ttu-id="a9061-131">Tryck på hello power-knappen på hello SensorTag toomake den kan identifieras.</span><span class="sxs-lookup"><span data-stu-id="a9061-131">Press hello power button on hello SensorTag toomake it discoverable.</span></span> <span data-ttu-id="a9061-132">hello grön Indikator blinkar.</span><span class="sxs-lookup"><span data-stu-id="a9061-132">hello green LED flashes.</span></span>

1. <span data-ttu-id="a9061-133">När du ser ett meddelande i hello shell hello styrenheten har identifierat hello SensorTag, anteckna hello hello enhetens MAC-adress.</span><span class="sxs-lookup"><span data-stu-id="a9061-133">When you see a message in hello shell that hello controller has discovered hello SensorTag, make a note of hello MAC address of hello device.</span></span> <span data-ttu-id="a9061-134">hello MAC-adress som ser ut som **A0:E6:F8:B5:F6:00**.</span><span class="sxs-lookup"><span data-stu-id="a9061-134">hello MAC address looks like **A0:E6:F8:B5:F6:00**.</span></span> <span data-ttu-id="a9061-135">Behöver du hello MAC-adress senare i självstudiekursen hello när du konfigurerar hello-gateway.</span><span class="sxs-lookup"><span data-stu-id="a9061-135">You need hello MAC address later in hello tutorial when you configure hello gateway.</span></span>

1. <span data-ttu-id="a9061-136">Kör hello efter kommandot tooturn av Bluetooth-sökning:</span><span class="sxs-lookup"><span data-stu-id="a9061-136">Run hello following command tooturn off Bluetooth scanning:</span></span>

    ```bash
    scan off
    ```

1. <span data-ttu-id="a9061-137">Kör hello efter kommandot tooverify att du kan ansluta toohello SensorTag-enheter:</span><span class="sxs-lookup"><span data-stu-id="a9061-137">Run hello following command tooverify that you can connect toohello SensorTag device:</span></span>

    ```bash
    connect <SensorTag MAC address>
    ```

    <span data-ttu-id="a9061-138">Om du ansluter har hello shell visar hello-meddelande **lyckad anslutning** och skriver ut information om hello SensorTag enhet.</span><span class="sxs-lookup"><span data-stu-id="a9061-138">If you connect successfully, hello shell shows hello message **Connection successful** and prints information about hello SensorTag device.</span></span> <span data-ttu-id="a9061-139">Om du inte kan ansluta Kontrollera hello SensorTag fortfarande är påslagen.</span><span class="sxs-lookup"><span data-stu-id="a9061-139">If you cannot connect, check hello SensorTag is still powered on.</span></span>

1. <span data-ttu-id="a9061-140">Du kan nu koppla från hello SensorTag och avsluta hello Bluetooth-gränssnittet genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="a9061-140">You can now disconnect from hello SensorTag and exit hello Bluetooth shell by running hello following commands:</span></span>

    ```bash
    disconnect
    exit
    ```

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-hello-custom-iot-edge-module"></a><span data-ttu-id="a9061-141">Skapa hello anpassad IoT kant-modul</span><span class="sxs-lookup"><span data-stu-id="a9061-141">Build hello custom IoT Edge module</span></span>

<span data-ttu-id="a9061-142">Du kan nu skapa hello anpassad IoT kant-modul som möjliggör hello gateway toosend meddelanden toohello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="a9061-142">You can now build hello custom IoT Edge module that enables hello gateway toosend messages toohello remote monitoring solution.</span></span> <span data-ttu-id="a9061-143">Mer information om hur du konfigurerar en gateway- och IoT-Edge moduler finns [Azure IoT kant begrepp][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="a9061-143">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

<span data-ttu-id="a9061-144">Hämta hello källkoden för hello anpassade IoT kant moduler från GitHub använder hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="a9061-144">Download hello source code for hello custom IoT Edge modules from GitHub using hello following commands:</span></span>

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

<span data-ttu-id="a9061-145">Skapa hello anpassade IoT kant-modul med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="a9061-145">Build hello custom IoT Edge module using hello following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

<span data-ttu-id="a9061-146">hello build-skript placerar hello libsensor2remotemonitoring.so anpassad IoT kant-modul i hello build-mappen.</span><span class="sxs-lookup"><span data-stu-id="a9061-146">hello build script places hello libsensor2remotemonitoring.so custom IoT Edge module in hello build folder.</span></span>

## <a name="configure-and-run-hello-iot-edge-gateway"></a><span data-ttu-id="a9061-147">Konfigurera och köra hello IoT gräns-gatewayen</span><span class="sxs-lookup"><span data-stu-id="a9061-147">Configure and run hello IoT Edge gateway</span></span>

<span data-ttu-id="a9061-148">Du kan nu konfigurera hello IoT Edge gateway toosend telemetri från dina SensorTag enhet tooyour fjärråtkomst övervakning instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="a9061-148">You can now configure hello IoT Edge gateway toosend telemetry from your SensorTag device tooyour remote monitoring dashboard.</span></span> <span data-ttu-id="a9061-149">Mer information om hur du konfigurerar en gateway- och IoT-Edge moduler finns [Azure IoT kant begrepp][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="a9061-149">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

> [!TIP]
> <span data-ttu-id="a9061-150">I den här kursen använder du hello standard `vi` textredigerare på hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="a9061-150">In this tutorial, you use hello standard `vi` text editor on hello Intel NUC.</span></span> <span data-ttu-id="a9061-151">Om du inte har använt `vi` innan, bör du genomföra en inledande vägledning som [Unix - hello vi Editor kursen] [ lnk-vi-tutorial] toofamiliarize dig med den här redigeraren.</span><span class="sxs-lookup"><span data-stu-id="a9061-151">If you have not used `vi` before, you should complete an introductory tutorial, such as [Unix - hello vi Editor Tutorial][lnk-vi-tutorial] toofamiliarize yourself with this editor.</span></span> <span data-ttu-id="a9061-152">Du kan också installera hello mer användarvänlig [nano](https://www.nano-editor.org/) editor hello kommandot `smart install nano -y`.</span><span class="sxs-lookup"><span data-stu-id="a9061-152">Alternatively, you can install hello more user-friendly [nano](https://www.nano-editor.org/) editor using hello command `smart install nano -y`.</span></span>

<span data-ttu-id="a9061-153">Öppna hello exempelkonfigurationsfilen i hello **vi** redigeraren med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a9061-153">Open hello sample configuration file in hello **vi** editor using hello following command:</span></span>

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic/remote_monitoring.json
```

<span data-ttu-id="a9061-154">Leta upp följande rader i hello konfiguration för hello IoTHub modulen hello:</span><span class="sxs-lookup"><span data-stu-id="a9061-154">Locate hello following lines in hello configuration for hello IoTHub module:</span></span>

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

<span data-ttu-id="a9061-155">Ersätt hello platshållare för värden med hello IoT-hubb information du skapat och sparat på hello början av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="a9061-155">Replace hello placeholder values with hello IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="a9061-156">hello-värdet för IoTHubName ser ut som **yourrmsolution37e08**, och hello värdet för IoTSuffix är vanligtvis **azure devices.net**.</span><span class="sxs-lookup"><span data-stu-id="a9061-156">hello value for IoTHubName looks like **yourrmsolution37e08**, and hello value for IoTSuffix is typically **azure-devices.net**.</span></span>

<span data-ttu-id="a9061-157">Leta upp följande rader i hello konfiguration för hello mappningsmodul hello:</span><span class="sxs-lookup"><span data-stu-id="a9061-157">Locate hello following lines in hello configuration for hello mapping module:</span></span>

```json
args": [
  {
    "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

<span data-ttu-id="a9061-158">Ersätt hello **macAddress** med hello MAC-adressen för din SensorTag som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a9061-158">Replace hello **macAddress** placeholder with hello MAC address of your SensorTag you noted previously.</span></span> <span data-ttu-id="a9061-159">Ersätt hello **deviceID** och **deviceKey** platshållarna med hello-ID: N och nycklarna för hello två enheter som du skapade tidigare i hello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="a9061-159">Replace hello **deviceID** and **deviceKey** placeholders with hello IDs and keys for hello two devices you created in hello remote monitoring solution previously.</span></span>

<span data-ttu-id="a9061-160">Leta upp följande rader i hello konfiguration för hello SensorTag modulen hello:</span><span class="sxs-lookup"><span data-stu-id="a9061-160">Locate hello following lines in hello configuration for hello SensorTag module:</span></span>

```json
"args": {
  "controller_index": 0,
  "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
  ...
}
```

<span data-ttu-id="a9061-161">Ersätt hello **enhet\_mac\_adress** med hello MAC-adressen för din SensorTag som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a9061-161">Replace hello **device\_mac\_address** placeholder  with hello MAC address of your SensorTag you noted previously.</span></span>

<span data-ttu-id="a9061-162">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="a9061-162">Save your changes.</span></span>

<span data-ttu-id="a9061-163">Du kan nu köra hello gateway med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="a9061-163">You can now run hello gateway using hello following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
/usr/share/azureiotgatewaysdk/samples/ble_gateway/ble_gateway remote_monitoring.json
```

<span data-ttu-id="a9061-164">Hej IoT gräns-gatewayen på hello Intel NUC påbörjas och skickar telemetri från hello SensorTag toohello remote övervakningslösning:</span><span class="sxs-lookup"><span data-stu-id="a9061-164">hello IoT Edge gateway starts on hello Intel NUC and sends telemetry from hello SensorTag toohello remote monitoring solution:</span></span>

![IoT-gräns-gatewayen skickar telemetri från hello SensorTag][img-telemetry]

<span data-ttu-id="a9061-166">Tryck på **Ctrl-C** tooexit hello program när som helst.</span><span class="sxs-lookup"><span data-stu-id="a9061-166">Press **Ctrl-C** tooexit hello program at any time.</span></span>

## <a name="view-hello-telemetry"></a><span data-ttu-id="a9061-167">Visa hello telemetri</span><span class="sxs-lookup"><span data-stu-id="a9061-167">View hello telemetry</span></span>

<span data-ttu-id="a9061-168">hello-gateway nu skicka telemetri från hello SensorTag enhet toohello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="a9061-168">hello gateway is now sending telemetry from hello SensorTag device toohello remote monitoring solution.</span></span> <span data-ttu-id="a9061-169">Du kan visa hello telemetri på instrumentpanelen för hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="a9061-169">You can view hello telemetry on hello solution dashboard.</span></span> <span data-ttu-id="a9061-170">Du kan också skicka kommandon tooyour SensorTag enheten via hello gateway hello lösning instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="a9061-170">You can also send commands tooyour SensorTag device through hello gateway from hello solution dashboard.</span></span>

- <span data-ttu-id="a9061-171">Navigera toohello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="a9061-171">Navigate toohello solution dashboard.</span></span>
- <span data-ttu-id="a9061-172">Välj hello-enhet som du konfigurerade i hello-gateway som representerar hello SensorTag i hello **enhet tooView** listrutan.</span><span class="sxs-lookup"><span data-stu-id="a9061-172">Select hello device you configured in hello gateway that represents hello SensorTag in hello **Device tooView** dropdown.</span></span>
- <span data-ttu-id="a9061-173">hello telemetri från hello SensorTag enhet visar hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="a9061-173">hello telemetry from hello SensorTag device displays on hello dashboard.</span></span>

![Visa telemetri från hello SensorTag-enheter][img-telemetry-display]

> [!WARNING]
> <span data-ttu-id="a9061-175">Om du lämnar hello remote övervakningslösning som körs i ditt Azure-konto, debiteras du för hello gång den körs.</span><span class="sxs-lookup"><span data-stu-id="a9061-175">If you leave hello remote monitoring solution running in your Azure account, you are billed for hello time it runs.</span></span> <span data-ttu-id="a9061-176">Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="a9061-176">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="a9061-177">Ta bort hello förkonfigurerade lösningen från ditt Azure-konto när du är klar.</span><span class="sxs-lookup"><span data-stu-id="a9061-177">Delete hello preconfigured solution from your Azure account when you have finished using it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a9061-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9061-178">Next steps</span></span>

<span data-ttu-id="a9061-179">Besök hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="a9061-179">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-telemetry]: ./media/iot-suite-gateway-kit-get-started-sensortag/appoutput.png


[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-sensortag/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-sensortag]: http://www.ti.com/ww/en/wireless_connectivity/sensortag/
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started