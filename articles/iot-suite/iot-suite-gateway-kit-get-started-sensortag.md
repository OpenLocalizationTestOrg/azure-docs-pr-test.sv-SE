---
title: "Ansluta en gateway för Azure IoT Suite med en Intel NUC | Microsoft Docs"
description: "Använd Microsoft IoT kommersiella Gateway Kit och fjärråtkomst övervakning förkonfigurerade lösningen. Använd Azure IoT gräns-gatewayen för att aktivera en SensorTag-enhet för att ansluta till den fjärranslutna övervakningslösning skicka telemetri till molnet och svara på metoderna som anropas från instrumentpanelen lösning."
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
ms.openlocfilehash: bda16be1094276fcecef1e708f9d7db307d94a89
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-telemetry-from-a-sensortag"></a><span data-ttu-id="f640f-104">Anslut dina Azure IoT gräns-gatewayen till fjärråtkomst övervakning förkonfigurerade lösningen och skicka telemetri från en SensorTag</span><span class="sxs-lookup"><span data-stu-id="f640f-104">Connect your Azure IoT Edge gateway to the remote monitoring preconfigured solution and send telemetry from a SensorTag</span></span>

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

<span data-ttu-id="f640f-105">Den här kursen visar hur du använder Azure IoT Edge för att skicka data för temperatur- och fuktighetskonsekvens från SensorTag enhet till fjärråtkomst övervakning förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="f640f-105">This tutorial shows you how to use Azure IoT Edge to send temperature and humidity data from SensorTag device to the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="f640f-106">SensorTag ansluter till Intel NUC gatewayen med Bluetooth.</span><span class="sxs-lookup"><span data-stu-id="f640f-106">The SensorTag connects to the Intel NUC gateway using Bluetooth.</span></span> <span data-ttu-id="f640f-107">I självstudiekursen används:</span><span class="sxs-lookup"><span data-stu-id="f640f-107">The tutorial uses:</span></span>

- <span data-ttu-id="f640f-108">Azure IoT-Edge att implementera en exempel-gateway.</span><span class="sxs-lookup"><span data-stu-id="f640f-108">Azure IoT Edge to implement a sample gateway.</span></span>
- <span data-ttu-id="f640f-109">IoT Suite fjärråtkomst övervakning förkonfigurerade lösning som molnbaserade serverdelen.</span><span class="sxs-lookup"><span data-stu-id="f640f-109">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="f640f-110">Översikt</span><span class="sxs-lookup"><span data-stu-id="f640f-110">Overview</span></span>

<span data-ttu-id="f640f-111">I den här kursen kan du utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="f640f-111">In this tutorial, you complete the following steps:</span></span>

- <span data-ttu-id="f640f-112">Distribuera en instans av fjärråtkomst övervakning förkonfigurerade lösningen till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f640f-112">Deploy an instance of the remote monitoring preconfigured solution to your Azure subscription.</span></span> <span data-ttu-id="f640f-113">Det här steget kan du automatiskt distribuerar och konfigurerar Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="f640f-113">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="f640f-114">Ställ in Intel NUC-gateway-enheten att kommunicera med datorn och den fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="f640f-114">Set up your Intel NUC gateway device to communicate with your computer and the remote monitoring solution.</span></span>
- <span data-ttu-id="f640f-115">Ställ in din Intel NUC gateway att ta emot telemetri från en SensorTag-enhet och skicka den till fjärranslutna instrumentpanelen för övervakning.</span><span class="sxs-lookup"><span data-stu-id="f640f-115">Set up your Intel NUC gateway to receive telemetry from a SensorTag device and send it to the remote monitoring dashboard.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

<span data-ttu-id="f640f-116">[Texas Instruments TIVERA SensorTag][lnk-sensortag].</span><span class="sxs-lookup"><span data-stu-id="f640f-116">[Texas Instruments BLE SensorTag][lnk-sensortag].</span></span> <span data-ttu-id="f640f-117">Den här självstudiekursen hämtar telemetridata via Bluetooth från SensorTag-enheter.</span><span class="sxs-lookup"><span data-stu-id="f640f-117">This tutorial retrieves telemetry data over Bluetooth from the SensorTag device.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="f640f-118">Fjärråtkomst övervakningslösning etablerar en mängd olika Azure-tjänster i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f640f-118">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="f640f-119">Distributionen visar en verklig enterprise-arkitektur.</span><span class="sxs-lookup"><span data-stu-id="f640f-119">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="f640f-120">Ta bort din instans av förkonfigurerade lösningen vid azureiotsuite.com för att undvika onödiga Azure-förbrukningen avgifter, när du är klar med den.</span><span class="sxs-lookup"><span data-stu-id="f640f-120">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="f640f-121">Om du behöver den förkonfigurerade lösningen, kan du enkelt återskapa den.</span><span class="sxs-lookup"><span data-stu-id="f640f-121">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="f640f-122">Mer information om hur du minskar användning när den fjärranslutna övervakningslösning körs finns [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="f640f-122">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

## <a name="configure-bluetooth-connectivity"></a><span data-ttu-id="f640f-123">Konfigurera Bluetooth-anslutning</span><span class="sxs-lookup"><span data-stu-id="f640f-123">Configure Bluetooth connectivity</span></span>

<span data-ttu-id="f640f-124">Konfigurera Bluetooth på Intel NUC så att SensorTag-enheten att ansluta och skicka telemetri.</span><span class="sxs-lookup"><span data-stu-id="f640f-124">Configure Bluetooth on the Intel NUC to enable the SensorTag device to connect and send telemetry.</span></span>

### <a name="find-the-mac-address-of-the-sensortag"></a><span data-ttu-id="f640f-125">Hitta MAC-adressen för SensorTag</span><span class="sxs-lookup"><span data-stu-id="f640f-125">Find the MAC address of the SensorTag</span></span>

1. <span data-ttu-id="f640f-126">Shell på Intel NUC, kör du följande kommando för att avblockera Bluetooth-tjänst:</span><span class="sxs-lookup"><span data-stu-id="f640f-126">In the shell on the Intel NUC, run the following command to unblock the Bluetooth service:</span></span>

    ```bash
    sudo rfkill unblock bluetooth
    ```

1. <span data-ttu-id="f640f-127">Kör följande kommandon för att starta tjänsten Bluetooth på Intel NUC och ange Bluetooth-gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="f640f-127">Run the following commands to start the Bluetooth service on the Intel NUC and enter the Bluetooth shell:</span></span>

    ```bash
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. <span data-ttu-id="f640f-128">Kör följande kommando för att stänga på Bluetooth-domänkontrollant:</span><span class="sxs-lookup"><span data-stu-id="f640f-128">Run the following command to power on the Bluetooth controller:</span></span>

    ```bash
    power on
    ```

    <span data-ttu-id="f640f-129">När styrenheten är aktiverat kan du se ett meddelande **ändra power på lyckades**.</span><span class="sxs-lookup"><span data-stu-id="f640f-129">When the controller is on, you see a message **Changing power on succeeded**.</span></span>

1. <span data-ttu-id="f640f-130">Kör följande kommando för att söka efter enheter i närheten Bluetooth:</span><span class="sxs-lookup"><span data-stu-id="f640f-130">Run the following command to scan for nearby Bluetooth devices:</span></span>

    ```bash
    scan on
    ```

1. <span data-ttu-id="f640f-131">Tryck på strömknappen på SensorTag så att den kan identifieras.</span><span class="sxs-lookup"><span data-stu-id="f640f-131">Press the power button on the SensorTag to make it discoverable.</span></span> <span data-ttu-id="f640f-132">Grön Indikator blinkar.</span><span class="sxs-lookup"><span data-stu-id="f640f-132">The green LED flashes.</span></span>

1. <span data-ttu-id="f640f-133">När du ser ett meddelande i gränssnittet att kontrollanten har identifierats i SensorTag anteckna MAC-adressen till enheten.</span><span class="sxs-lookup"><span data-stu-id="f640f-133">When you see a message in the shell that the controller has discovered the SensorTag, make a note of the MAC address of the device.</span></span> <span data-ttu-id="f640f-134">MAC-adressen ser ut som **A0:E6:F8:B5:F6:00**.</span><span class="sxs-lookup"><span data-stu-id="f640f-134">The MAC address looks like **A0:E6:F8:B5:F6:00**.</span></span> <span data-ttu-id="f640f-135">Du behöver MAC-adressen senare under kursen när du konfigurerar en gateway.</span><span class="sxs-lookup"><span data-stu-id="f640f-135">You need the MAC address later in the tutorial when you configure the gateway.</span></span>

1. <span data-ttu-id="f640f-136">Kör följande kommando för att inaktivera Bluetooth-sökning:</span><span class="sxs-lookup"><span data-stu-id="f640f-136">Run the following command to turn off Bluetooth scanning:</span></span>

    ```bash
    scan off
    ```

1. <span data-ttu-id="f640f-137">Kör följande kommando för att kontrollera att du kan ansluta till SensorTag-enhet:</span><span class="sxs-lookup"><span data-stu-id="f640f-137">Run the following command to verify that you can connect to the SensorTag device:</span></span>

    ```bash
    connect <SensorTag MAC address>
    ```

    <span data-ttu-id="f640f-138">Om du ansluter har gränssnittet visas meddelandet **lyckad anslutning** och skriver ut information om SensorTag-enhet.</span><span class="sxs-lookup"><span data-stu-id="f640f-138">If you connect successfully, the shell shows the message **Connection successful** and prints information about the SensorTag device.</span></span> <span data-ttu-id="f640f-139">Om du inte kan ansluta kontrollerar du SensorTag fortfarande är påslagen.</span><span class="sxs-lookup"><span data-stu-id="f640f-139">If you cannot connect, check the SensorTag is still powered on.</span></span>

1. <span data-ttu-id="f640f-140">Du kan nu koppla från SensorTag och lämna Bluetooth-gränssnittet genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="f640f-140">You can now disconnect from the SensorTag and exit the Bluetooth shell by running the following commands:</span></span>

    ```bash
    disconnect
    exit
    ```

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-the-custom-iot-edge-module"></a><span data-ttu-id="f640f-141">Skapa anpassad IoT kant-modul</span><span class="sxs-lookup"><span data-stu-id="f640f-141">Build the custom IoT Edge module</span></span>

<span data-ttu-id="f640f-142">Du kan nu skapa anpassade IoT kant-modulen som gör att en gateway att skicka meddelanden till den fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="f640f-142">You can now build the custom IoT Edge module that enables the gateway to send messages to the remote monitoring solution.</span></span> <span data-ttu-id="f640f-143">Mer information om hur du konfigurerar en gateway- och IoT-Edge moduler finns [Azure IoT kant begrepp][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="f640f-143">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

<span data-ttu-id="f640f-144">Ladda ned källkoden för de anpassade IoT kant-modulerna från GitHub med hjälp av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="f640f-144">Download the source code for the custom IoT Edge modules from GitHub using the following commands:</span></span>

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

<span data-ttu-id="f640f-145">Skapa anpassade IoT kant modulen med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="f640f-145">Build the custom IoT Edge module using the following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

<span data-ttu-id="f640f-146">Build-skript placerar libsensor2remotemonitoring.so anpassad IoT kant-modul i build-mappen.</span><span class="sxs-lookup"><span data-stu-id="f640f-146">The build script places the libsensor2remotemonitoring.so custom IoT Edge module in the build folder.</span></span>

## <a name="configure-and-run-the-iot-edge-gateway"></a><span data-ttu-id="f640f-147">Konfigurera och köra IoT gräns-gatewayen</span><span class="sxs-lookup"><span data-stu-id="f640f-147">Configure and run the IoT Edge gateway</span></span>

<span data-ttu-id="f640f-148">Du kan nu konfigurera IoT gräns-gatewayen för att skicka telemetri från enheten SensorTag till instrumentpanelen för fjärråtkomst övervakning.</span><span class="sxs-lookup"><span data-stu-id="f640f-148">You can now configure the IoT Edge gateway to send telemetry from your SensorTag device to your remote monitoring dashboard.</span></span> <span data-ttu-id="f640f-149">Mer information om hur du konfigurerar en gateway- och IoT-Edge moduler finns [Azure IoT kant begrepp][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="f640f-149">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

> [!TIP]
> <span data-ttu-id="f640f-150">I kursen får du använder standard `vi` textredigerare på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="f640f-150">In this tutorial, you use the standard `vi` text editor on the Intel NUC.</span></span> <span data-ttu-id="f640f-151">Om du inte har använt `vi` innan, bör du genomföra en inledande vägledning som [Unix - vi Editor kursen] [ lnk-vi-tutorial] att bekanta dig med den här redigeraren.</span><span class="sxs-lookup"><span data-stu-id="f640f-151">If you have not used `vi` before, you should complete an introductory tutorial, such as [Unix - The vi Editor Tutorial][lnk-vi-tutorial] to familiarize yourself with this editor.</span></span> <span data-ttu-id="f640f-152">Du kan också installera mer användarvänlig [nano](https://www.nano-editor.org/) redigeraren med hjälp av kommandot `smart install nano -y`.</span><span class="sxs-lookup"><span data-stu-id="f640f-152">Alternatively, you can install the more user-friendly [nano](https://www.nano-editor.org/) editor using the command `smart install nano -y`.</span></span>

<span data-ttu-id="f640f-153">Öppna exempelkonfigurationsfilen i den **vi** redigeraren med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f640f-153">Open the sample configuration file in the **vi** editor using the following command:</span></span>

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic/remote_monitoring.json
```

<span data-ttu-id="f640f-154">Leta upp följande rader i konfigurationen för IoTHub-modulen:</span><span class="sxs-lookup"><span data-stu-id="f640f-154">Locate the following lines in the configuration for the IoTHub module:</span></span>

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

<span data-ttu-id="f640f-155">Vill du ersätta platshållarvärdena med IoT-hubb-information som du skapade och sparade i början av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="f640f-155">Replace the placeholder values with the IoT Hub information you created and saved at the start of this tutorial.</span></span> <span data-ttu-id="f640f-156">Värdet för IoTHubName ser ut som **yourrmsolution37e08**, och värdet för IoTSuffix är vanligtvis **azure devices.net**.</span><span class="sxs-lookup"><span data-stu-id="f640f-156">The value for IoTHubName looks like **yourrmsolution37e08**, and the value for IoTSuffix is typically **azure-devices.net**.</span></span>

<span data-ttu-id="f640f-157">Leta upp följande rader i konfigurationen för modulen mappning:</span><span class="sxs-lookup"><span data-stu-id="f640f-157">Locate the following lines in the configuration for the mapping module:</span></span>

```json
args": [
  {
    "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

<span data-ttu-id="f640f-158">Ersätt den **macAddress** med MAC-adressen för din SensorTag som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="f640f-158">Replace the **macAddress** placeholder with the MAC address of your SensorTag you noted previously.</span></span> <span data-ttu-id="f640f-159">Ersätt den **deviceID** och **deviceKey** platshållarna med ID: N och nycklarna för de två enheter som du skapade tidigare i den fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="f640f-159">Replace the **deviceID** and **deviceKey** placeholders with the IDs and keys for the two devices you created in the remote monitoring solution previously.</span></span>

<span data-ttu-id="f640f-160">Leta upp följande rader i konfigurationen för SensorTag-modulen:</span><span class="sxs-lookup"><span data-stu-id="f640f-160">Locate the following lines in the configuration for the SensorTag module:</span></span>

```json
"args": {
  "controller_index": 0,
  "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
  ...
}
```

<span data-ttu-id="f640f-161">Ersätt den **enhet\_mac\_adress** med MAC-adressen för din SensorTag som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="f640f-161">Replace the **device\_mac\_address** placeholder  with the MAC address of your SensorTag you noted previously.</span></span>

<span data-ttu-id="f640f-162">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="f640f-162">Save your changes.</span></span>

<span data-ttu-id="f640f-163">Du kan nu köra gatewayen med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="f640f-163">You can now run the gateway using the following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
/usr/share/azureiotgatewaysdk/samples/ble_gateway/ble_gateway remote_monitoring.json
```

<span data-ttu-id="f640f-164">IoT-gräns-gatewayen på Intel NUC påbörjas och skickar telemetri från SensorTag till fjärranslutna övervakningslösning:</span><span class="sxs-lookup"><span data-stu-id="f640f-164">The IoT Edge gateway starts on the Intel NUC and sends telemetry from the SensorTag to the remote monitoring solution:</span></span>

![IoT-gräns-gatewayen skickar telemetri från SensorTag][img-telemetry]

<span data-ttu-id="f640f-166">Tryck på **Ctrl-C** avsluta programmet när som helst.</span><span class="sxs-lookup"><span data-stu-id="f640f-166">Press **Ctrl-C** to exit the program at any time.</span></span>

## <a name="view-the-telemetry"></a><span data-ttu-id="f640f-167">Visa telemetrin</span><span class="sxs-lookup"><span data-stu-id="f640f-167">View the telemetry</span></span>

<span data-ttu-id="f640f-168">Gatewayen är nu skicka telemetri från SensorTag-enhet till fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="f640f-168">The gateway is now sending telemetry from the SensorTag device to the remote monitoring solution.</span></span> <span data-ttu-id="f640f-169">Du kan visa telemetrin på instrumentpanelen för lösningen.</span><span class="sxs-lookup"><span data-stu-id="f640f-169">You can view the telemetry on the solution dashboard.</span></span> <span data-ttu-id="f640f-170">Du kan även skicka kommandon till enheten SensorTag via gatewayen från instrumentpanelen lösning.</span><span class="sxs-lookup"><span data-stu-id="f640f-170">You can also send commands to your SensorTag device through the gateway from the solution dashboard.</span></span>

- <span data-ttu-id="f640f-171">Gå till instrumentpanelen lösning.</span><span class="sxs-lookup"><span data-stu-id="f640f-171">Navigate to the solution dashboard.</span></span>
- <span data-ttu-id="f640f-172">Välj den enhet som du konfigurerade i den gateway som representerar SensorTag i den **enhet för att visa** listrutan.</span><span class="sxs-lookup"><span data-stu-id="f640f-172">Select the device you configured in the gateway that represents the SensorTag in the **Device to View** dropdown.</span></span>
- <span data-ttu-id="f640f-173">Telemetri från SensorTag-enheten visas på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f640f-173">The telemetry from the SensorTag device displays on the dashboard.</span></span>

![Visa telemetri från SensorTag-enheter][img-telemetry-display]

> [!WARNING]
> <span data-ttu-id="f640f-175">Om du lämnar den fjärranslutna övervakningslösning som körs i ditt Azure-konto, debiteras du för den tid som den körs.</span><span class="sxs-lookup"><span data-stu-id="f640f-175">If you leave the remote monitoring solution running in your Azure account, you are billed for the time it runs.</span></span> <span data-ttu-id="f640f-176">Mer information om hur du minskar användning när den fjärranslutna övervakningslösning körs finns [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="f640f-176">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="f640f-177">Ta bort den förkonfigurerade lösningen från ditt Azure-konto när du är klar.</span><span class="sxs-lookup"><span data-stu-id="f640f-177">Delete the preconfigured solution from your Azure account when you have finished using it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f640f-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f640f-178">Next steps</span></span>

<span data-ttu-id="f640f-179">Besök den [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="f640f-179">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-telemetry]: ./media/iot-suite-gateway-kit-get-started-sensortag/appoutput.png


[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-sensortag/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-sensortag]: http://www.ti.com/ww/en/wireless_connectivity/sensortag/
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started