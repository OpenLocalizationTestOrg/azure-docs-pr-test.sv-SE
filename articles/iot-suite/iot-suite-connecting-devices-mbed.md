---
title: "aaaConnect en enhet med hjälp av C på mbed | Microsoft Docs"
description: "Beskriver hur tooconnect en enhet toohello Azure IoT Suite förkonfigurerade remote övervakningslösning som använder ett program som skrivits i C som körs på en mbed enhet."
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
ms.openlocfilehash: dcd1e74635e8dec678a59bff060a73f7cfabd124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-mbed"></a><span data-ttu-id="bc9d2-103">Ansluta din enhet toohello remote förkonfigurerade övervakningslösning (mbed)</span><span class="sxs-lookup"><span data-stu-id="bc9d2-103">Connect your device toohello remote monitoring preconfigured solution (mbed)</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="bc9d2-104">Scenarioöversikt</span><span class="sxs-lookup"><span data-stu-id="bc9d2-104">Scenario overview</span></span>
<span data-ttu-id="bc9d2-105">I det här scenariot skapar du en enhet som skickar hello följande telemetri toohello fjärrövervaknings [förkonfigurerade lösningen][lnk-what-are-preconfig-solutions]:</span><span class="sxs-lookup"><span data-stu-id="bc9d2-105">In this scenario, you create a device that sends hello following telemetry toohello remote monitoring [preconfigured solution][lnk-what-are-preconfig-solutions]:</span></span>

* <span data-ttu-id="bc9d2-106">Extern temperatur</span><span class="sxs-lookup"><span data-stu-id="bc9d2-106">External temperature</span></span>
* <span data-ttu-id="bc9d2-107">Intern temperatur</span><span class="sxs-lookup"><span data-stu-id="bc9d2-107">Internal temperature</span></span>
* <span data-ttu-id="bc9d2-108">Fuktighet</span><span class="sxs-lookup"><span data-stu-id="bc9d2-108">Humidity</span></span>

<span data-ttu-id="bc9d2-109">För enkelhetens skull hello koden på hello enhet genererar exempelvärden, men vi rekommenderar att du tooextend hello exemplet genom att ansluta verkliga sensorer tooyour enheten och skicka verkliga telemetri.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-109">For simplicity, hello code on hello device generates sample values, but we encourage you tooextend hello sample by connecting real sensors tooyour device and sending real telemetry.</span></span>

<span data-ttu-id="bc9d2-110">hello-enheten är också kan toorespond toomethods anropas från hello lösning instrumentpanelen och önskade egenskapsvärden i hello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-110">hello device is also able toorespond toomethods invoked from hello solution dashboard and desired property values set in hello solution dashboard.</span></span>

<span data-ttu-id="bc9d2-111">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-111">toocomplete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="bc9d2-112">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="bc9d2-113">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="bc9d2-113">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="bc9d2-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="bc9d2-114">Before you start</span></span>
<span data-ttu-id="bc9d2-115">Innan du kan skriva kod för enheten måste du etablera din förkonfigurerade lösning för fjärrövervakning och etablera en ny anpassad enhet i lösningen.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-115">Before you write any code for your device, you must provision your remote monitoring preconfigured solution and provision a new custom device in that solution.</span></span>

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="bc9d2-116">Etablera din förkonfigurerade lösning för fjärrövervakning</span><span class="sxs-lookup"><span data-stu-id="bc9d2-116">Provision your remote monitoring preconfigured solution</span></span>
<span data-ttu-id="bc9d2-117">hello-enhet som du skapar i den här självstudiekursen skickar data tooan instans av hello [fjärrövervaknings] [ lnk-remote-monitoring] förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-117">hello device you create in this tutorial sends data tooan instance of hello [remote monitoring][lnk-remote-monitoring] preconfigured solution.</span></span> <span data-ttu-id="bc9d2-118">Om du inte redan har etablerats hello fjärråtkomst övervakning förkonfigurerade lösning i ditt Azure-konto, använder du hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bc9d2-118">If you haven't already provisioned hello remote monitoring preconfigured solution in your Azure account, use hello following steps:</span></span>

1. <span data-ttu-id="bc9d2-119">På hello <https://www.azureiotsuite.com/> klickar du på  **+**  toocreate en lösning.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-119">On hello <https://www.azureiotsuite.com/> page, click **+** toocreate a solution.</span></span>
2. <span data-ttu-id="bc9d2-120">Klicka på **Välj** på hello **fjärrövervaknings** panelen toocreate din lösning.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-120">Click **Select** on hello **Remote monitoring** panel toocreate your solution.</span></span>
3. <span data-ttu-id="bc9d2-121">På hello **skapa Remote övervakningslösning** anger en **lösningsnamn** du själv väljer, Välj hello **Region** toodeploy till, och markera hello Azure prenumerationen toowant toouse.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-121">On hello **Create Remote monitoring solution** page, enter a **Solution name** of your choice, select hello **Region** you want toodeploy to, and select hello Azure subscription toowant toouse.</span></span> <span data-ttu-id="bc9d2-122">Klicka på **Skapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-122">Then click **Create solution**.</span></span>
4. <span data-ttu-id="bc9d2-123">Vänta tills hello etableringsprocessen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-123">Wait until hello provisioning process completes.</span></span>

> [!WARNING]
> <span data-ttu-id="bc9d2-124">hello förkonfigurerade lösningar använder fakturerbar Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-124">hello preconfigured solutions use billable Azure services.</span></span> <span data-ttu-id="bc9d2-125">Glöm inte tooremove hello förkonfigurerade lösningen från prenumerationen när du är klar med den tooavoid eventuella onödiga kostnader.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-125">Be sure tooremove hello preconfigured solution from your subscription when you are done with it tooavoid any unnecessary charges.</span></span> <span data-ttu-id="bc9d2-126">Du kan helt ta bort en förkonfigurerade lösning från prenumerationen genom att besöka hello <https://www.azureiotsuite.com/> sidan.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-126">You can completely remove a preconfigured solution from your subscription by visiting hello <https://www.azureiotsuite.com/> page.</span></span>
> 
> 

<span data-ttu-id="bc9d2-127">När hello etableringsprocessen för hello remote övervakningslösning är klar klickar du på **starta** tooopen hello lösning instrumentpanel i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-127">When hello provisioning process for hello remote monitoring solution finishes, click **Launch** tooopen hello solution dashboard in your browser.</span></span>

![Instrumentpanel för lösningen][img-dashboard]

### <a name="provision-your-device-in-hello-remote-monitoring-solution"></a><span data-ttu-id="bc9d2-129">Etablera din enhet i hello remote övervakningslösning</span><span class="sxs-lookup"><span data-stu-id="bc9d2-129">Provision your device in hello remote monitoring solution</span></span>
> [!NOTE]
> <span data-ttu-id="bc9d2-130">Om du redan har etablerat en enhet i din lösning kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-130">If you have already provisioned a device in your solution, you can skip this step.</span></span> <span data-ttu-id="bc9d2-131">När du skapar hello klientprogrammet behöver du tooknow hello enheten autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-131">You need tooknow hello device credentials when you create hello client application.</span></span>
> 
> 

<span data-ttu-id="bc9d2-132">För en enhet tooconnect toohello förkonfigurerade lösning, den måste identifiera sig själv tooIoT hubb med giltiga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-132">For a device tooconnect toohello preconfigured solution, it must identify itself tooIoT Hub using valid credentials.</span></span> <span data-ttu-id="bc9d2-133">Du kan hämta hello enheten autentiseringsuppgifter från hello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-133">You can retrieve hello device credentials from hello solution dashboard.</span></span> <span data-ttu-id="bc9d2-134">Du inkludera hello enheten autentiseringsuppgifter i ditt klientprogram senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-134">You include hello device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="bc9d2-135">tooadd en enhet tooyour remote övervakningslösning fullständig hello följa stegen i hello lösning instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="bc9d2-135">tooadd a device tooyour remote monitoring solution, complete hello following steps in hello solution dashboard:</span></span>

1. <span data-ttu-id="bc9d2-136">I hello nedre vänstra hörnet av hello instrumentpanelen, klickar du på **lägger till en enhet**.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-136">In hello lower left-hand corner of hello dashboard, click **Add a device**.</span></span>
   
   ![Lägg till en enhet][1]
2. <span data-ttu-id="bc9d2-138">I hello **anpassad enhet** klickar du på **Lägg till ny**.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-138">In hello **Custom Device** panel, click **Add new**.</span></span>
   
   ![Lägg till en anpassad enhet][2]
3. <span data-ttu-id="bc9d2-140">Välj **Låt mig ange mitt eget enhets-ID**.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-140">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="bc9d2-141">Ange ett enhets-ID som **mydevice**, klickar du på **kontrollera ID** tooverify namnet inte redan används och klicka sedan på **skapa** tooprovision hello enhet.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-141">Enter a Device ID such as **mydevice**, click **Check ID** tooverify that name isn't already in use, and then click **Create** tooprovision hello device.</span></span>
   
   ![Lägg till enhets-ID][3]
4. <span data-ttu-id="bc9d2-143">Gör en anteckning hello enheten autentiseringsuppgifter (enhets-ID, IoT Hub-värdnamnet och nyckeln enheten).</span><span class="sxs-lookup"><span data-stu-id="bc9d2-143">Make a note hello device credentials (Device ID, IoT Hub Hostname, and Device Key).</span></span> <span data-ttu-id="bc9d2-144">Klientprogrammet måste dessa värden tooconnect toohello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-144">Your client application needs these values tooconnect toohello remote monitoring solution.</span></span> <span data-ttu-id="bc9d2-145">Klicka sedan på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-145">Then click **Done**.</span></span>
   
    ![Visa enhetsautentiseringsuppgifter][4]
5. <span data-ttu-id="bc9d2-147">Välj din enhet i listan över enheter hello hello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-147">Select your device in hello device list in hello solution dashboard.</span></span> <span data-ttu-id="bc9d2-148">Sedan hello **enhetsinformation** klickar du på **Aktivera enhet**.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-148">Then, in hello **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="bc9d2-149">hello statusen för din enhet är nu **kör**.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-149">hello status of your device is now **Running**.</span></span> <span data-ttu-id="bc9d2-150">hello remote övervakningslösning kan nu ta emot telemetri från enheten och anropa metoder i hello enhet.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-150">hello remote monitoring solution can now receive telemetry from your device and invoke methods on hello device.</span></span>

[img-dashboard]: ./media/iot-suite-connecting-devices-mbed/dashboard.png
[1]: ./media/iot-suite-connecting-devices-mbed/suite0.png
[2]: ./media/iot-suite-connecting-devices-mbed/suite1.png
[3]: ./media/iot-suite-connecting-devices-mbed/suite2.png
[4]: ./media/iot-suite-connecting-devices-mbed/suite3.png

[lnk-what-are-preconfig-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

## <a name="build-and-run-hello-c-sample-solution"></a><span data-ttu-id="bc9d2-151">Skapa och köra hello C exempellösning</span><span class="sxs-lookup"><span data-stu-id="bc9d2-151">Build and run hello C sample solution</span></span>

<span data-ttu-id="bc9d2-152">hello följande anvisningar beskrivs hello åtgärder för att ansluta en [mbed-aktiverade Freescale FRDM-K64F] [ lnk-mbed-home] enhet toohello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-152">hello following instructions describe hello steps for connecting an [mbed-enabled Freescale FRDM-K64F][lnk-mbed-home] device toohello remote monitoring solution.</span></span>

### <a name="connect-hello-mbed-device-tooyour-network-and-desktop-machine"></a><span data-ttu-id="bc9d2-153">Ansluta hello mbed enhet tooyour nätverks- och stationära datorer</span><span class="sxs-lookup"><span data-stu-id="bc9d2-153">Connect hello mbed device tooyour network and desktop machine</span></span>

1. <span data-ttu-id="bc9d2-154">Ansluta hello mbed enhet tooyour nätverk med hjälp av en Ethernet-kabel.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-154">Connect hello mbed device tooyour network using an Ethernet cable.</span></span> <span data-ttu-id="bc9d2-155">Det här steget är nödvändigt eftersom hello exempelprogrammet kräver tillgång till internet.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-155">This step is necessary because hello sample application requires internet access.</span></span>

1. <span data-ttu-id="bc9d2-156">Se [komma igång med mbed] [ lnk-mbed-getstarted] tooconnect din mbed enhet tooyour stationär dator.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-156">See [Getting Started with mbed][lnk-mbed-getstarted] tooconnect your mbed device tooyour desktop PC.</span></span>

1. <span data-ttu-id="bc9d2-157">Om din dator kör Windows, se [Datorkonfiguration] [ lnk-mbed-pcconnect] tooconfigure serieport tooyour mbed enhet.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-157">If your desktop PC is running Windows, see [PC Configuration][lnk-mbed-pcconnect] tooconfigure serial port access tooyour mbed device.</span></span>

### <a name="create-an-mbed-project-and-import-hello-sample-code"></a><span data-ttu-id="bc9d2-158">Skapa ett mbed projekt och importera hello exempelkod</span><span class="sxs-lookup"><span data-stu-id="bc9d2-158">Create an mbed project and import hello sample code</span></span>

<span data-ttu-id="bc9d2-159">Följ dessa steg tooadd vissa kod tooan mbed exempelprojektet.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-159">Follow these steps tooadd some sample code tooan mbed project.</span></span> <span data-ttu-id="bc9d2-160">Du importerar hello fjärråtkomst övervakning starter-projekt och ändra sedan hello projektet toouse hello MQTT protokollet i stället för hello AMQP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-160">You import hello remote monitoring starter project and then change hello project toouse hello MQTT protocol instead of hello AMQP protocol.</span></span> <span data-ttu-id="bc9d2-161">För närvarande måste toouse hello MQTT protokollet toouse hello enhetshanteringsfunktioner för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-161">Currently, you need toouse hello MQTT protocol toouse hello device management features of IoT Hub.</span></span>

1. <span data-ttu-id="bc9d2-162">I webbläsaren, går toohello mbed.org [developer plats](https://developer.mbed.org/).</span><span class="sxs-lookup"><span data-stu-id="bc9d2-162">In your web browser, go toohello mbed.org [developer site](https://developer.mbed.org/).</span></span> <span data-ttu-id="bc9d2-163">Om du inte har registrerat dig kan se du en alternativet toocreate ett konto (som är ledigt).</span><span class="sxs-lookup"><span data-stu-id="bc9d2-163">If you haven't signed up, you see an option toocreate an account (it's free).</span></span> <span data-ttu-id="bc9d2-164">Annars kan logga in med autentiseringsuppgifterna för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-164">Otherwise, log in with your account credentials.</span></span> <span data-ttu-id="bc9d2-165">Klicka på **kompileraren** i hello övre högra hörnet av hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-165">Then click **Compiler** in hello upper right-hand corner of hello page.</span></span> <span data-ttu-id="bc9d2-166">Den här åtgärden ger dig toohello *arbetsytan* gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-166">This action brings you toohello *Workspace* interface.</span></span>

1. <span data-ttu-id="bc9d2-167">Kontrollera hello maskinvaruplattform du använder visas i hello övre högra hörnet av hello-fönstret eller klicka på hello ikon i hello höger tooselect plattform för maskinvaran.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-167">Make sure hello hardware platform you're using appears in hello upper right-hand corner of hello window, or click hello icon in hello right-hand corner tooselect your hardware platform.</span></span>

1. <span data-ttu-id="bc9d2-168">Klicka på **importera** på hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-168">Click **Import** on hello main menu.</span></span> <span data-ttu-id="bc9d2-169">Klicka på **Klicka här tooimport från URL**.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-169">Then click **Click here tooimport from URL**.</span></span>
   
    ![Starta import toombed arbetsytan][6]

1. <span data-ttu-id="bc9d2-171">Ange hello länk för hello exempel kod https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ i hello popup-fönster, och klicka sedan på **importera**.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-171">In hello pop-up window, enter hello link for hello sample code https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ then click **Import**.</span></span>
   
    ![Importera exempel kod toombed arbetsytan][7]

1. <span data-ttu-id="bc9d2-173">Du kan se i hello mbed kompileraren-fönstret att importera det här projektet också importerar olika bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-173">You can see in hello mbed compiler window that importing this project also imports various libraries.</span></span> <span data-ttu-id="bc9d2-174">Vissa tillhandahålls och underhålls av hello Azure IoT-teamet ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), medan andra är tillgängliga i hello mbed bibliotek katalog från tredje part-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-174">Some are provided and maintained by hello Azure IoT team ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), while others are third-party libraries available in hello mbed libraries catalog.</span></span>
   
    ![Visa mbed projekt][8]

1. <span data-ttu-id="bc9d2-176">I hello **programmet arbetsytan**, högerklicka på hello **iothub\_amqp\_transport** bibliotek, klickar du på **ta bort**, och klicka sedan på **OK** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-176">In hello **Program Workspace**, right-click hello **iothub\_amqp\_transport** library, click **Delete**, and then click **OK** tooconfirm.</span></span>

1. <span data-ttu-id="bc9d2-177">I hello **programmet arbetsytan**, högerklicka på hello **azure\_amqp\_c** bibliotek, klickar du på **ta bort**, och klicka sedan på **OK**  tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-177">In hello **Program Workspace**, right-click hello **azure\_amqp\_c** library, click **Delete**, and then click **OK** tooconfirm.</span></span>

1. <span data-ttu-id="bc9d2-178">Högerklicka på hello **remote_monitoring** projekt i hello **programmet arbetsytan**väljer **importera bibliotek**och välj **från URL: en**.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-178">Right-click hello **remote_monitoring** project in hello **Program Workspace**, select **Import Library**, then select **From URL**.</span></span>
   
    ![Starta biblioteket Importera toombed arbetsytan][6]

1. <span data-ttu-id="bc9d2-180">I hello popup-fönster, ange hello länk hello MQTT transport biblioteket https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_transport / Klicka **importera**.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-180">In hello pop-up window, enter hello link for hello MQTT transport library https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_transport/ then click **Import**.</span></span>
   
    ![Importera bibliotek toombed arbetsytan][12]

1. <span data-ttu-id="bc9d2-182">Hello Upprepa föregående steg tooadd hello MQTT bibliotek från https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c /.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-182">Repeat hello previous step tooadd hello MQTT library from https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c/.</span></span>

1. <span data-ttu-id="bc9d2-183">Arbetsytan nu ser ut som följande hello:</span><span class="sxs-lookup"><span data-stu-id="bc9d2-183">Your workspace now looks like hello following:</span></span>

    ![Visa mbed arbetsyta][13]

1. <span data-ttu-id="bc9d2-185">Öppna hello remote\_monitoring\remote_monitoring.c fil- och Ersätt hello befintliga `#include` instruktioner med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="bc9d2-185">Open hello remote\_monitoring\remote_monitoring.c file and replace hello existing `#include` statements with hello following code:</span></span>

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
1. <span data-ttu-id="bc9d2-186">Ta bort alla hello återstående koden i hello remote\_monitoring\remote\_monitoring.c fil.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-186">Delete all hello remaining code in hello remote\_monitoring\remote\_monitoring.c file.</span></span>

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-hello-sample"></a><span data-ttu-id="bc9d2-187">Skapa och köra hello-exempel</span><span class="sxs-lookup"><span data-stu-id="bc9d2-187">Build and run hello sample</span></span>

<span data-ttu-id="bc9d2-188">Lägg till kod tooinvoke hello **remote\_övervakning\_kör** fungera och sedan skapa och köra hello enhetsprogram.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-188">Add code tooinvoke hello **remote\_monitoring\_run** function and then build and run hello device application.</span></span>

1. <span data-ttu-id="bc9d2-189">Lägg till en **huvudsakliga** funktionen med följande kod hello slutet av hello remote\_monitoring.c filen tooinvoke hello **remote\_övervakning\_kör** funktionen:</span><span class="sxs-lookup"><span data-stu-id="bc9d2-189">Add a **main** function with following code at hello end of hello remote\_monitoring.c file tooinvoke hello **remote\_monitoring\_run** function:</span></span>
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. <span data-ttu-id="bc9d2-190">Klicka på **Kompilera** toobuild hello program.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-190">Click **Compile** toobuild hello program.</span></span> <span data-ttu-id="bc9d2-191">Du kan på ett säkert sätt ignoreras alla varningar, men om hello build genererar fel, åtgärda dem innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-191">You can safely ignore any warnings, but if hello build generates errors, fix them before proceeding.</span></span>

1. <span data-ttu-id="bc9d2-192">Om det lyckas hello build hello mbed kompileraren webbplats genererar en .bin-filen med hello namnet på ditt projekt och hämtar den tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-192">If hello build is successful, hello mbed compiler website generates a .bin file with hello name of your project and downloads it tooyour local machine.</span></span> <span data-ttu-id="bc9d2-193">Kopiera hello .bin-filen toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-193">Copy hello .bin file toohello device.</span></span> <span data-ttu-id="bc9d2-194">Spara hello .bin-filen toohello enhet gör hello enheten toorestart och kör hello program som finns i hello .bin-filen.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-194">Saving hello .bin file toohello device causes hello device toorestart and run hello program contained in hello .bin file.</span></span> <span data-ttu-id="bc9d2-195">Du kan starta om programmet hello manuellt när som helst genom att trycka på hello Återställ-knappen på hello mbed enhet.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-195">You can manually restart hello program at any time by pressing hello reset button on hello mbed device.</span></span>

1. <span data-ttu-id="bc9d2-196">Ansluta toohello enhet med hjälp av en SSH-klientprogram, till exempel PuTTY.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-196">Connect toohello device using an SSH client application, such as PuTTY.</span></span> <span data-ttu-id="bc9d2-197">Du kan fastställa hello serieport enheten använder genom att kontrollera i Enhetshanteraren Windows.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-197">You can determine hello serial port your device uses by checking Windows Device Manager.</span></span>
   
    ![][11]

1. <span data-ttu-id="bc9d2-198">Klicka på hello i PuTTY, **seriella** anslutningstypen.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-198">In PuTTY, click hello **Serial** connection type.</span></span> <span data-ttu-id="bc9d2-199">hello enheten ansluts vanligtvis på 9 600 baud, så ange 9600 i hello **hastighet** rutan.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-199">hello device typically connects at 9600 baud, so enter 9600 in hello **Speed** box.</span></span> <span data-ttu-id="bc9d2-200">Klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-200">Then click **Open**.</span></span>

1. <span data-ttu-id="bc9d2-201">hello postprogram körs.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-201">hello program starts executing.</span></span> <span data-ttu-id="bc9d2-202">Du kan ha tooreset hello board (tryck på CTRL + Break eller tryck på hello board Återställ-knapp) om hello inte startar automatiskt när du ansluter.</span><span class="sxs-lookup"><span data-stu-id="bc9d2-202">You may have tooreset hello board (press CTRL+Break or press hello board's reset button) if hello program does not start automatically when you connect.</span></span>
   
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
