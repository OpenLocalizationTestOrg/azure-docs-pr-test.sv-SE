---
title: "Ansluter en enhet med hjälp av C på mbed | Microsoft Docs"
description: "Beskriver hur du ansluter en enhet till Azure IoT Suite förkonfigurerade fjärråtkomst övervakning lösningen med hjälp av ett program som skrivits i C som körs på en mbed enhet."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 9551075e-dcf9-488f-943e-d0eb0e6260be
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: dobett
ROBOTS: NOINDEX
ms.openlocfilehash: ef7b78f85a787f8fbe22c0e26aa34f0cd1685d58
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a><span data-ttu-id="56a3a-103">Ansluta enheten till den fjärranslutna förkonfigurerade övervakningslösning (mbed)</span><span class="sxs-lookup"><span data-stu-id="56a3a-103">Connect your device to the remote monitoring preconfigured solution (mbed)</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="56a3a-104">Scenarioöversikt</span><span class="sxs-lookup"><span data-stu-id="56a3a-104">Scenario overview</span></span>
<span data-ttu-id="56a3a-105">I det här scenariot skapar du en enhet som skickar följande telemetri till den [förkonfigurerade lösningen][lnk-what-are-preconfig-solutions] för fjärrövervakning:</span><span class="sxs-lookup"><span data-stu-id="56a3a-105">In this scenario, you create a device that sends the following telemetry to the remote monitoring [preconfigured solution][lnk-what-are-preconfig-solutions]:</span></span>

* <span data-ttu-id="56a3a-106">Extern temperatur</span><span class="sxs-lookup"><span data-stu-id="56a3a-106">External temperature</span></span>
* <span data-ttu-id="56a3a-107">Intern temperatur</span><span class="sxs-lookup"><span data-stu-id="56a3a-107">Internal temperature</span></span>
* <span data-ttu-id="56a3a-108">Fuktighet</span><span class="sxs-lookup"><span data-stu-id="56a3a-108">Humidity</span></span>

<span data-ttu-id="56a3a-109">För enkelhetens skull genererar koden på enheten exempelvärden men vi rekommenderar att du utökar exemplet genom att ansluta riktiga sensorer till enheten och skicka riktig telemetri.</span><span class="sxs-lookup"><span data-stu-id="56a3a-109">For simplicity, the code on the device generates sample values, but we encourage you to extend the sample by connecting real sensors to your device and sending real telemetry.</span></span>

<span data-ttu-id="56a3a-110">Enheten kan även svara på metoderna som anropas från lösningens instrumentpanel och de önskade egenskapsvärden som är angivna i lösningens instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="56a3a-110">The device is also able to respond to methods invoked from the solution dashboard and desired property values set in the solution dashboard.</span></span>

<span data-ttu-id="56a3a-111">Du behöver ett Azure-konto för att slutföra den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="56a3a-111">To complete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="56a3a-112">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="56a3a-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="56a3a-113">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="56a3a-113">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="56a3a-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="56a3a-114">Before you start</span></span>
<span data-ttu-id="56a3a-115">Innan du kan skriva kod för enheten måste du etablera din förkonfigurerade lösning för fjärrövervakning och etablera en ny anpassad enhet i lösningen.</span><span class="sxs-lookup"><span data-stu-id="56a3a-115">Before you write any code for your device, you must provision your remote monitoring preconfigured solution and provision a new custom device in that solution.</span></span>

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="56a3a-116">Etablera din förkonfigurerade lösning för fjärrövervakning</span><span class="sxs-lookup"><span data-stu-id="56a3a-116">Provision your remote monitoring preconfigured solution</span></span>
<span data-ttu-id="56a3a-117">Enheten som du skapar i den här självstudien skickar data till en instans av den förkonfigurerade lösningen för [fjärrövervakning][lnk-remote-monitoring].</span><span class="sxs-lookup"><span data-stu-id="56a3a-117">The device you create in this tutorial sends data to an instance of the [remote monitoring][lnk-remote-monitoring] preconfigured solution.</span></span> <span data-ttu-id="56a3a-118">Om du inte redan har etablerat den förkonfigurerade lösningen för fjärrövervakning i ditt Azure-konto använder du följande steg:</span><span class="sxs-lookup"><span data-stu-id="56a3a-118">If you haven't already provisioned the remote monitoring preconfigured solution in your Azure account, use the following steps:</span></span>

1. <span data-ttu-id="56a3a-119">Gå till <https://www.azureiotsuite.com/> och klicka på **+** för att skapa en lösning.</span><span class="sxs-lookup"><span data-stu-id="56a3a-119">On the <https://www.azureiotsuite.com/> page, click **+** to create a solution.</span></span>
2. <span data-ttu-id="56a3a-120">Klicka på **Välj** på panelen **Fjärrövervakning** för att skapa din lösning.</span><span class="sxs-lookup"><span data-stu-id="56a3a-120">Click **Select** on the **Remote monitoring** panel to create your solution.</span></span>
3. <span data-ttu-id="56a3a-121">På sidan **Create Remote monitoring solution** (Skapa fjärrövervakningslösning) anger du ett **Lösningsnamn** och väljer den **Region** du vill distribuera till samt väljer den Azure-prenumerationen som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="56a3a-121">On the **Create Remote monitoring solution** page, enter a **Solution name** of your choice, select the **Region** you want to deploy to, and select the Azure subscription to want to use.</span></span> <span data-ttu-id="56a3a-122">Klicka på **Skapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="56a3a-122">Then click **Create solution**.</span></span>
4. <span data-ttu-id="56a3a-123">Vänta tills etableringsprocessen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="56a3a-123">Wait until the provisioning process completes.</span></span>

> [!WARNING]
> <span data-ttu-id="56a3a-124">De förkonfigurerade lösningarna använder fakturerbara Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="56a3a-124">The preconfigured solutions use billable Azure services.</span></span> <span data-ttu-id="56a3a-125">Se till att ta bort den förkonfigurerade lösningen från prenumerationen när du är färdig med den för att undvika onödiga kostnader.</span><span class="sxs-lookup"><span data-stu-id="56a3a-125">Be sure to remove the preconfigured solution from your subscription when you are done with it to avoid any unnecessary charges.</span></span> <span data-ttu-id="56a3a-126">Du kan ta bort en förkonfigurerad lösning helt från din prenumeration genom att besöka sidan <https://www.azureiotsuite.com/>.</span><span class="sxs-lookup"><span data-stu-id="56a3a-126">You can completely remove a preconfigured solution from your subscription by visiting the <https://www.azureiotsuite.com/> page.</span></span>
> 
> 

<span data-ttu-id="56a3a-127">När etableringen av fjärrövervakningslösningen är klar klickar du på **Starta** för att öppna lösningens instrumentpanel i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="56a3a-127">When the provisioning process for the remote monitoring solution finishes, click **Launch** to open the solution dashboard in your browser.</span></span>

![Instrumentpanel för lösningen][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a><span data-ttu-id="56a3a-129">Etablera enheten i fjärrövervakningslösningen</span><span class="sxs-lookup"><span data-stu-id="56a3a-129">Provision your device in the remote monitoring solution</span></span>
> [!NOTE]
> <span data-ttu-id="56a3a-130">Om du redan har etablerat en enhet i din lösning kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="56a3a-130">If you have already provisioned a device in your solution, you can skip this step.</span></span> <span data-ttu-id="56a3a-131">Du behöver känna till enhetens autentiseringsuppgifter när du skapar klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="56a3a-131">You need to know the device credentials when you create the client application.</span></span>
> 
> 

<span data-ttu-id="56a3a-132">För att en enhet ska kunna ansluta till den förkonfigurerade lösningen måste den identifiera sig för IoT Hub med giltiga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="56a3a-132">For a device to connect to the preconfigured solution, it must identify itself to IoT Hub using valid credentials.</span></span> <span data-ttu-id="56a3a-133">Du kan hämta enhetens autentiseringsuppgifter från lösningens instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="56a3a-133">You can retrieve the device credentials from the solution dashboard.</span></span> <span data-ttu-id="56a3a-134">Du kan inkludera enhetsautentiseringsuppgifterna i klientprogrammet senare i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="56a3a-134">You include the device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="56a3a-135">Om du vill lägga till en enhet till din fjärrövervakningslösning utför du följande steg i lösningens instrumentpanel:</span><span class="sxs-lookup"><span data-stu-id="56a3a-135">To add a device to your remote monitoring solution, complete the following steps in the solution dashboard:</span></span>

1. <span data-ttu-id="56a3a-136">Klicka på **Lägg till en enhet** i det nedre vänstra hörnet på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="56a3a-136">In the lower left-hand corner of the dashboard, click **Add a device**.</span></span>
   
   ![Lägg till en enhet][1]
2. <span data-ttu-id="56a3a-138">I panelen **Anpassad enhet** klickar du på **Lägg till ny**.</span><span class="sxs-lookup"><span data-stu-id="56a3a-138">In the **Custom Device** panel, click **Add new**.</span></span>
   
   ![Lägg till en anpassad enhet][2]
3. <span data-ttu-id="56a3a-140">Välj **Låt mig ange mitt eget enhets-ID**.</span><span class="sxs-lookup"><span data-stu-id="56a3a-140">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="56a3a-141">Ange ett enhets-ID, t.ex. **mydevice** och klicka på **Kontrollera ID** för att kontrollera att namnet inte redan används. Klicka sedan på **Skapa** för att etablera enheten.</span><span class="sxs-lookup"><span data-stu-id="56a3a-141">Enter a Device ID such as **mydevice**, click **Check ID** to verify that name isn't already in use, and then click **Create** to provision the device.</span></span>
   
   ![Lägg till enhets-ID][3]
4. <span data-ttu-id="56a3a-143">Notera enhetsautentiseringsuppgifterna (Enhets-ID, IoT Hub-värdnamn och Enhetsnyckel).</span><span class="sxs-lookup"><span data-stu-id="56a3a-143">Make a note the device credentials (Device ID, IoT Hub Hostname, and Device Key).</span></span> <span data-ttu-id="56a3a-144">Klientprogrammet behöver dessa värden för att ansluta till fjärrövervakningslösningen.</span><span class="sxs-lookup"><span data-stu-id="56a3a-144">Your client application needs these values to connect to the remote monitoring solution.</span></span> <span data-ttu-id="56a3a-145">Klicka sedan på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="56a3a-145">Then click **Done**.</span></span>
   
    ![Visa enhetsautentiseringsuppgifter][4]
5. <span data-ttu-id="56a3a-147">Välj enheten i enhetslistan i lösningens instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="56a3a-147">Select your device in the device list in the solution dashboard.</span></span> <span data-ttu-id="56a3a-148">Klicka sedan på **Aktivera enhet** i panelen **Enhetsinformation**.</span><span class="sxs-lookup"><span data-stu-id="56a3a-148">Then, in the **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="56a3a-149">Statusen för din enhet är nu **Körs**.</span><span class="sxs-lookup"><span data-stu-id="56a3a-149">The status of your device is now **Running**.</span></span> <span data-ttu-id="56a3a-150">Fjärrövervakningslösningen kan nu ta emot telemetri från enheten och anropa metoder på enheten.</span><span class="sxs-lookup"><span data-stu-id="56a3a-150">The remote monitoring solution can now receive telemetry from your device and invoke methods on the device.</span></span>

[img-dashboard]: ./media/iot-suite-connecting-devices-mbed/dashboard.png
[1]: ./media/iot-suite-connecting-devices-mbed/suite0.png
[2]: ./media/iot-suite-connecting-devices-mbed/suite1.png
[3]: ./media/iot-suite-connecting-devices-mbed/suite2.png
[4]: ./media/iot-suite-connecting-devices-mbed/suite3.png

[lnk-what-are-preconfig-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

## <a name="build-and-run-the-c-sample-solution"></a><span data-ttu-id="56a3a-151">Skapa och köra exempellösning C</span><span class="sxs-lookup"><span data-stu-id="56a3a-151">Build and run the C sample solution</span></span>

<span data-ttu-id="56a3a-152">I följande anvisningar beskrivs stegen för att ansluta en [mbed-aktiverade Freescale FRDM-K64F] [ lnk-mbed-home] enheten till den fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="56a3a-152">The following instructions describe the steps for connecting an [mbed-enabled Freescale FRDM-K64F][lnk-mbed-home] device to the remote monitoring solution.</span></span>

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a><span data-ttu-id="56a3a-153">Anslut mbed enheten till din dator för Nätverks- och fjärrskrivbordsanslutning</span><span class="sxs-lookup"><span data-stu-id="56a3a-153">Connect the mbed device to your network and desktop machine</span></span>

1. <span data-ttu-id="56a3a-154">Anslut mbed enheten till nätverket med hjälp av en Ethernet-kabel.</span><span class="sxs-lookup"><span data-stu-id="56a3a-154">Connect the mbed device to your network using an Ethernet cable.</span></span> <span data-ttu-id="56a3a-155">Det här steget är nödvändigt eftersom exempelprogrammet kräver tillgång till internet.</span><span class="sxs-lookup"><span data-stu-id="56a3a-155">This step is necessary because the sample application requires internet access.</span></span>

1. <span data-ttu-id="56a3a-156">Se [komma igång med mbed] [ lnk-mbed-getstarted] ansluta enheten mbed till din dator.</span><span class="sxs-lookup"><span data-stu-id="56a3a-156">See [Getting Started with mbed][lnk-mbed-getstarted] to connect your mbed device to your desktop PC.</span></span>

1. <span data-ttu-id="56a3a-157">Om din dator kör Windows, se [Datorkonfiguration] [ lnk-mbed-pcconnect] att konfigurera serieport åtkomst till enheten mbed.</span><span class="sxs-lookup"><span data-stu-id="56a3a-157">If your desktop PC is running Windows, see [PC Configuration][lnk-mbed-pcconnect] to configure serial port access to your mbed device.</span></span>

### <a name="create-an-mbed-project-and-import-the-sample-code"></a><span data-ttu-id="56a3a-158">Skapa ett mbed projekt och importera exempelkoden</span><span class="sxs-lookup"><span data-stu-id="56a3a-158">Create an mbed project and import the sample code</span></span>

<span data-ttu-id="56a3a-159">Följ dessa steg för att lägga till vissa exempelkod till en mbed-projekt.</span><span class="sxs-lookup"><span data-stu-id="56a3a-159">Follow these steps to add some sample code to an mbed project.</span></span> <span data-ttu-id="56a3a-160">Du importerar fjärråtkomst övervakning starter-projektet och ändra sedan projektet om du vill använda protokollet MQTT i stället för AMQP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="56a3a-160">You import the remote monitoring starter project and then change the project to use the MQTT protocol instead of the AMQP protocol.</span></span> <span data-ttu-id="56a3a-161">För närvarande kan behöver du använda MQTT-protokollet för att använda hanteringsfunktioner för enheter i IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="56a3a-161">Currently, you need to use the MQTT protocol to use the device management features of IoT Hub.</span></span>

1. <span data-ttu-id="56a3a-162">I webbläsaren, går du till mbed.org [developer plats](https://developer.mbed.org/).</span><span class="sxs-lookup"><span data-stu-id="56a3a-162">In your web browser, go to the mbed.org [developer site](https://developer.mbed.org/).</span></span> <span data-ttu-id="56a3a-163">Om du inte har registrerat dig kan se du ett alternativ för att skapa ett konto (som är ledigt).</span><span class="sxs-lookup"><span data-stu-id="56a3a-163">If you haven't signed up, you see an option to create an account (it's free).</span></span> <span data-ttu-id="56a3a-164">Annars kan logga in med autentiseringsuppgifterna för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="56a3a-164">Otherwise, log in with your account credentials.</span></span> <span data-ttu-id="56a3a-165">Klicka på **kompileraren** i det övre högra hörnet på sidan.</span><span class="sxs-lookup"><span data-stu-id="56a3a-165">Then click **Compiler** in the upper right-hand corner of the page.</span></span> <span data-ttu-id="56a3a-166">Den här åtgärden ger dig den *arbetsytan* gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="56a3a-166">This action brings you to the *Workspace* interface.</span></span>

1. <span data-ttu-id="56a3a-167">Kontrollera att maskinvaruplattform som du använder som visas i det övre högra hörnet i fönstret eller klicka på ikonen i det högra hörnet och välj din maskinvaruplattform.</span><span class="sxs-lookup"><span data-stu-id="56a3a-167">Make sure the hardware platform you're using appears in the upper right-hand corner of the window, or click the icon in the right-hand corner to select your hardware platform.</span></span>

1. <span data-ttu-id="56a3a-168">Klicka på **importera** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="56a3a-168">Click **Import** on the main menu.</span></span> <span data-ttu-id="56a3a-169">Klicka på **Klicka här om du vill importera från URL: en**.</span><span class="sxs-lookup"><span data-stu-id="56a3a-169">Then click **Click here to import from URL**.</span></span>
   
    ![Starta import till mbed arbetsyta][6]

1. <span data-ttu-id="56a3a-171">Ange länken för exemplet kod https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ i popup-fönstret och klicka sedan på **importera**.</span><span class="sxs-lookup"><span data-stu-id="56a3a-171">In the pop-up window, enter the link for the sample code https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ then click **Import**.</span></span>
   
    ![Importera exempelkod till mbed arbetsytan][7]

1. <span data-ttu-id="56a3a-173">Du kan se i fönstret mbed kompilatorn att importera det här projektet också importerar olika bibliotek.</span><span class="sxs-lookup"><span data-stu-id="56a3a-173">You can see in the mbed compiler window that importing this project also imports various libraries.</span></span> <span data-ttu-id="56a3a-174">Vissa tillhandahålls och underhålls av Azure IoT-teamet ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), medan andra är tredjeparts-bibliotek finns i katalogen mbed bibliotek.</span><span class="sxs-lookup"><span data-stu-id="56a3a-174">Some are provided and maintained by the Azure IoT team ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), while others are third-party libraries available in the mbed libraries catalog.</span></span>
   
    ![Visa mbed projekt][8]

1. <span data-ttu-id="56a3a-176">I den **programmet arbetsytan**, högerklicka på den **iothub\_amqp\_transport** bibliotek, klickar du på **ta bort**, och klicka sedan på  **OK** att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="56a3a-176">In the **Program Workspace**, right-click the **iothub\_amqp\_transport** library, click **Delete**, and then click **OK** to confirm.</span></span>

1. <span data-ttu-id="56a3a-177">I den **programmet arbetsytan**, högerklicka på den **azure\_amqp\_c** bibliotek, klickar du på **ta bort**, och klicka sedan på **OK** att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="56a3a-177">In the **Program Workspace**, right-click the **azure\_amqp\_c** library, click **Delete**, and then click **OK** to confirm.</span></span>

1. <span data-ttu-id="56a3a-178">Högerklicka på den **remote_monitoring** projektet i den **programmet arbetsytan**väljer **importera bibliotek**och välj **från URL: en**.</span><span class="sxs-lookup"><span data-stu-id="56a3a-178">Right-click the **remote_monitoring** project in the **Program Workspace**, select **Import Library**, then select **From URL**.</span></span>
   
    ![Starta import av typbibliotek till mbed arbetsyta][6]

1. <span data-ttu-id="56a3a-180">Ange länken i popup-fönstret för MQTT transport biblioteket https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_transport / Klicka **importera**.</span><span class="sxs-lookup"><span data-stu-id="56a3a-180">In the pop-up window, enter the link for the MQTT transport library https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_transport/ then click **Import**.</span></span>
   
    ![Importera bibliotek till mbed arbetsyta][12]

1. <span data-ttu-id="56a3a-182">Upprepa det föregående steget för att lägga till biblioteket MQTT från https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c /.</span><span class="sxs-lookup"><span data-stu-id="56a3a-182">Repeat the previous step to add the MQTT library from https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c/.</span></span>

1. <span data-ttu-id="56a3a-183">Arbetsytan nu ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="56a3a-183">Your workspace now looks like the following:</span></span>

    ![Visa mbed arbetsyta][13]

1. <span data-ttu-id="56a3a-185">Öppna fjärransluten\_monitoring\remote_monitoring.c fil- och Ersätt den befintliga `#include` instruktioner med följande kod:</span><span class="sxs-lookup"><span data-stu-id="56a3a-185">Open the remote\_monitoring\remote_monitoring.c file and replace the existing `#include` statements with the following code:</span></span>

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"

    #ifdef MBED_BUILD_TIMESTAMP
    #include "certs.h"
    #endif // MBED_BUILD_TIMESTAMP
    ```
1. <span data-ttu-id="56a3a-186">Ta bort den återstående koden i fjärransluten\_monitoring\remote\_monitoring.c fil.</span><span class="sxs-lookup"><span data-stu-id="56a3a-186">Delete all the remaining code in the remote\_monitoring\remote\_monitoring.c file.</span></span>

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a><span data-ttu-id="56a3a-187">Skapa och köra exemplet</span><span class="sxs-lookup"><span data-stu-id="56a3a-187">Build and run the sample</span></span>

<span data-ttu-id="56a3a-188">Lägg till kod för att anropa den **remote\_övervakning\_kör** fungera och sedan skapa och köra programmet för enheten.</span><span class="sxs-lookup"><span data-stu-id="56a3a-188">Add code to invoke the **remote\_monitoring\_run** function and then build and run the device application.</span></span>

1. <span data-ttu-id="56a3a-189">Lägg till en **huvudsakliga** funktionen med följande kod i slutet av fjärransluten\_monitoring.c fil att anropa den **remote\_övervakning\_kör** funktionen:</span><span class="sxs-lookup"><span data-stu-id="56a3a-189">Add a **main** function with following code at the end of the remote\_monitoring.c file to invoke the **remote\_monitoring\_run** function:</span></span>
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. <span data-ttu-id="56a3a-190">Klicka på **Kompilera** att skapa programmet.</span><span class="sxs-lookup"><span data-stu-id="56a3a-190">Click **Compile** to build the program.</span></span> <span data-ttu-id="56a3a-191">Du kan på ett säkert sätt ignoreras alla varningar, men om bygga genererar fel, åtgärda dem innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="56a3a-191">You can safely ignore any warnings, but if the build generates errors, fix them before proceeding.</span></span>

1. <span data-ttu-id="56a3a-192">Om det lyckas bygga mbed kompileraren webbplatsen genererar en .bin-filen med namnet på ditt projekt och hämtas till din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="56a3a-192">If the build is successful, the mbed compiler website generates a .bin file with the name of your project and downloads it to your local machine.</span></span> <span data-ttu-id="56a3a-193">Kopiera .bin-filen till enheten.</span><span class="sxs-lookup"><span data-stu-id="56a3a-193">Copy the .bin file to the device.</span></span> <span data-ttu-id="56a3a-194">Spara .bin-filen till enheten gör att enheten att starta om och kör programmet i .bin-filen.</span><span class="sxs-lookup"><span data-stu-id="56a3a-194">Saving the .bin file to the device causes the device to restart and run the program contained in the .bin file.</span></span> <span data-ttu-id="56a3a-195">Du kan starta om programmet manuellt när som helst genom att trycka på återställningsknappen på mbed enheten.</span><span class="sxs-lookup"><span data-stu-id="56a3a-195">You can manually restart the program at any time by pressing the reset button on the mbed device.</span></span>

1. <span data-ttu-id="56a3a-196">Ansluta till den enhet som använder en SSH-klientprogram, till exempel PuTTY.</span><span class="sxs-lookup"><span data-stu-id="56a3a-196">Connect to the device using an SSH client application, such as PuTTY.</span></span> <span data-ttu-id="56a3a-197">Du kan fastställa serieporten enheten använder genom att kontrollera i Enhetshanteraren Windows.</span><span class="sxs-lookup"><span data-stu-id="56a3a-197">You can determine the serial port your device uses by checking Windows Device Manager.</span></span>
   
    ![][11]

1. <span data-ttu-id="56a3a-198">I PuTTY, klickar du på den **seriella** anslutningstypen.</span><span class="sxs-lookup"><span data-stu-id="56a3a-198">In PuTTY, click the **Serial** connection type.</span></span> <span data-ttu-id="56a3a-199">Enheten ansluts vanligtvis på 9 600 baud, så ange 9600 i den **hastighet** rutan.</span><span class="sxs-lookup"><span data-stu-id="56a3a-199">The device typically connects at 9600 baud, so enter 9600 in the **Speed** box.</span></span> <span data-ttu-id="56a3a-200">Klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="56a3a-200">Then click **Open**.</span></span>

1. <span data-ttu-id="56a3a-201">Programmet startar körs.</span><span class="sxs-lookup"><span data-stu-id="56a3a-201">The program starts executing.</span></span> <span data-ttu-id="56a3a-202">Du kan behöva återställa kortet (tryck på CTRL + Break eller tryck på den board Återställ-knapp) om programmet inte startar automatiskt när du ansluter.</span><span class="sxs-lookup"><span data-stu-id="56a3a-202">You may have to reset the board (press CTRL+Break or press the board's reset button) if the program does not start automatically when you connect.</span></span>
   
    ![][10]

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png
[12]: ./media/iot-suite-connecting-devices-mbed/mbed7.png
[13]: ./media/iot-suite-connecting-devices-mbed/mbed8.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
