---
title: "aaaReal tidsdata visualisering av sensordata från din Azure IoT-hubb – Web Apps | Microsoft Docs"
description: "Med funktionen hello webbprogram med Microsoft Azure App Service toovisualize temperatur- och fuktighetskonsekvens data som samlas in från hello sensor och skickas tooyour Iot-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: realtid datavisualisering, realtidsdata visualiseringen sensor datavisualisering
ms.assetid: e42b07a8-ddd4-476e-9bfb-903d6b033e91
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 72f2dffee1c2f975948820eee9f2e287c3f77255
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-hello-web-apps-feature-of-azure-app-service"></a><span data-ttu-id="d619a-104">Visualisera sensordata i realtid från Azure IoT-hubben med hjälp av funktionen för hello Web Apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d619a-104">Visualize real-time sensor data from your Azure IoT hub by using hello Web Apps feature of Azure App Service</span></span>

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/5.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="d619a-106">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="d619a-106">What you learn</span></span>

<span data-ttu-id="d619a-107">I kursen får du lära dig hur toovisualize sensordata i realtid som din IoT-hubb som tar emot genom att köra ett webbprogram som är värd för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d619a-107">In this tutorial, you learn how toovisualize real-time sensor data that your IoT hub receives by running a web application that is hosted on a web app.</span></span> <span data-ttu-id="d619a-108">Om du vill tootry toovisualize hello data i din IoT-hubb med hjälp av Power BI, se [Använd Power BI toovisualize realtid sensordata från Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="d619a-108">If you want tootry toovisualize hello data in your IoT hub by using Power BI, see [Use Power BI toovisualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="d619a-109">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="d619a-109">What you do</span></span>

- <span data-ttu-id="d619a-110">Skapa en webbapp i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d619a-110">Create a web app in hello Azure portal.</span></span>
- <span data-ttu-id="d619a-111">Förbereda din IoT-hubb för åtkomst till data genom att lägga till en konsumentgrupp.</span><span class="sxs-lookup"><span data-stu-id="d619a-111">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="d619a-112">Konfigurera hello web app tooread sensordata från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d619a-112">Configure hello web app tooread sensor data from your IoT hub.</span></span>
- <span data-ttu-id="d619a-113">Överför en web application toobe hello webbprogram som värd.</span><span class="sxs-lookup"><span data-stu-id="d619a-113">Upload a web application toobe hosted by hello web app.</span></span>
- <span data-ttu-id="d619a-114">Öppna hello web app toosee temperatur- och fuktighetskonsekvens realtidsdata från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d619a-114">Open hello web app toosee real-time temperature and humidity data from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="d619a-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="d619a-115">What you need</span></span>

- <span data-ttu-id="d619a-116">[Konfigurera din enhet](iot-hub-raspberry-pi-kit-node-get-started.md), som omfattar hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="d619a-116">[Set up your device](iot-hub-raspberry-pi-kit-node-get-started.md), which covers hello following requirements:</span></span>
  - <span data-ttu-id="d619a-117">En aktiv Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d619a-117">An active Azure subscription</span></span>
  - <span data-ttu-id="d619a-118">En Iot-hubb i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="d619a-118">An Iot hub under your subscription</span></span>
  - <span data-ttu-id="d619a-119">Ett klientprogram som skickar meddelanden tooyour Iot-hubb</span><span class="sxs-lookup"><span data-stu-id="d619a-119">A client application that sends messages tooyour Iot hub</span></span>
- [<span data-ttu-id="d619a-120">Hämta Git</span><span class="sxs-lookup"><span data-stu-id="d619a-120">Download Git</span></span>](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a><span data-ttu-id="d619a-121">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="d619a-121">Create a web app</span></span>

1. <span data-ttu-id="d619a-122">I hello [Azure-portalen](https://ms.portal.azure.com/), klickar du på **ny** > **webb + mobilt** > **Web App**.</span><span class="sxs-lookup"><span data-stu-id="d619a-122">In hello [Azure portal](https://ms.portal.azure.com/), click **New** > **Web + Mobile** > **Web App**.</span></span>
2. <span data-ttu-id="d619a-123">Ange ett unikt jobbnamn verifierar hello prenumeration, ange en resursgrupp och en plats, väljer **PIN-kod toodashboard**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="d619a-123">Enter a unique job name, verify hello subscription, specify a resource group and a location, select **Pin toodashboard**, and then click **Create**.</span></span>

   <span data-ttu-id="d619a-124">Vi rekommenderar att du väljer hello samma plats som resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d619a-124">We recommend that you select hello same location as that of your resource group.</span></span> <span data-ttu-id="d619a-125">Detta hjälper till med bearbetningshastigheten och minskar hello kostnaden för dataöverföring.</span><span class="sxs-lookup"><span data-stu-id="d619a-125">Doing so assists with processing speed and reduces hello cost of data transfer.</span></span>

   ![Skapa en webbapp](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-hello-web-app-tooread-data-from-your-iot-hub"></a><span data-ttu-id="d619a-127">Konfigurera hello web app tooread data från IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="d619a-127">Configure hello web app tooread data from your IoT hub</span></span>

1. <span data-ttu-id="d619a-128">Öppna hello webbapp som du precis har etablerats.</span><span class="sxs-lookup"><span data-stu-id="d619a-128">Open hello web app you’ve just provisioned.</span></span>
2. <span data-ttu-id="d619a-129">Klicka på **programinställningar**, och under **appinställningar**, Lägg till följande nyckel/värde-par hello:</span><span class="sxs-lookup"><span data-stu-id="d619a-129">Click **Application settings**, and then, under **App settings**, add hello following key/value pairs:</span></span>

   | <span data-ttu-id="d619a-130">Nyckel</span><span class="sxs-lookup"><span data-stu-id="d619a-130">Key</span></span>                                   | <span data-ttu-id="d619a-131">Värde</span><span class="sxs-lookup"><span data-stu-id="d619a-131">Value</span></span>                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | <span data-ttu-id="d619a-132">Azure.IoT.IoTHub.ConnectionString</span><span class="sxs-lookup"><span data-stu-id="d619a-132">Azure.IoT.IoTHub.ConnectionString</span></span>     | <span data-ttu-id="d619a-133">Hämtas från iothub explorer</span><span class="sxs-lookup"><span data-stu-id="d619a-133">Obtained from iothub-explorer</span></span>                                |
   | <span data-ttu-id="d619a-134">Azure.IoT.IoTHub.ConsumerGroup</span><span class="sxs-lookup"><span data-stu-id="d619a-134">Azure.IoT.IoTHub.ConsumerGroup</span></span>        | <span data-ttu-id="d619a-135">hello namnet på hello konsumentgrupp som du lägger till tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="d619a-135">hello name of hello consumer group that you add tooyour IoT hub</span></span>  |

   ![Lägga till inställningarna tooyour webbprogrammet med nyckel/värde-par](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

3. <span data-ttu-id="d619a-137">Klicka på **programinställningar**under **allmänna inställningar**, växla hello **Web sockets** alternativ och klickar sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="d619a-137">Click **Application settings**, under **General settings**, toggle hello **Web sockets** option, and then click **Save**.</span></span>

   ![Växla hello sockets webbinställningar](media/iot-hub-live-data-visualization-in-web-apps/10_toggle_web_sockets.png)

## <a name="upload-a-web-application-toobe-hosted-by-hello-web-app"></a><span data-ttu-id="d619a-139">Överför en web application toobe hos hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="d619a-139">Upload a web application toobe hosted by hello web app</span></span>

<span data-ttu-id="d619a-140">På GitHub, har vi gjort tillgängliga ett webbprogram som visar sensordata i realtid från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d619a-140">On GitHub, we've made available a web application that displays real-time sensor data from your IoT hub.</span></span> <span data-ttu-id="d619a-141">Allt du behöver toodo är konfigurera hello web app toowork med en Git-lagringsplats, hämta hello webbprogrammet från GitHub och sedan ladda upp den tooAzure för hello web app toohost.</span><span class="sxs-lookup"><span data-stu-id="d619a-141">All you need toodo is configure hello web app toowork with a Git repository, download hello web application from GitHub, and then upload it tooAzure for hello web app toohost.</span></span>

1. <span data-ttu-id="d619a-142">I hello webbapp klickar du på **distributionsalternativ** > **Välj källa** > **lokal Git-lagringsplats**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d619a-142">In hello web app, click **Deployment Options** > **Choose Source** > **Local Git Repository**, and then click **OK**.</span></span>

   ![Konfigurera din web app distribution toouse hello lokal Git-lagringsplats](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. <span data-ttu-id="d619a-144">Klicka på **Distributionsbehörigheterna**, skapa en användare och lösenord toouse tooconnect toohello Git-lagringsplats i Azure och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="d619a-144">Click **Deployment Credentials**, create a user name and password toouse tooconnect toohello Git repository in Azure, and then click **Save**.</span></span>

3. <span data-ttu-id="d619a-145">Klicka på **översikt**, och anteckna värdet för hello av **url för Git-klon**.</span><span class="sxs-lookup"><span data-stu-id="d619a-145">Click **Overview**, and note hello value of **Git clone url**.</span></span>

   ![Hämta hello Git klon-URL för ditt webbprogram](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

4. <span data-ttu-id="d619a-147">Öppna ett kommando eller ett terminalfönster på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="d619a-147">Open a command or terminal window on your local computer.</span></span>

5. <span data-ttu-id="d619a-148">Hämta hello webbprogrammet från GitHub och överför den tooAzure för hello web app toohost.</span><span class="sxs-lookup"><span data-stu-id="d619a-148">Download hello web app from GitHub, and upload it tooAzure for hello web app toohost.</span></span> <span data-ttu-id="d619a-149">toodo kör så hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="d619a-149">toodo so, run hello following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > <span data-ttu-id="d619a-150">\<URL för Git-klon\> är hello URL för hello Git-lagringsplatsen finns på hello **översikt** sidan av hello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d619a-150">\<Git clone URL\> is hello URL of hello Git repository found on hello **Overview** page of hello web app.</span></span>

## <a name="open-hello-web-app-toosee-real-time-temperature-and-humidity-data-from-your-iot-hub"></a><span data-ttu-id="d619a-151">Öppna hello web app toosee temperatur- och fuktighetskonsekvens realtidsdata från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="d619a-151">Open hello web app toosee real-time temperature and humidity data from your IoT hub</span></span>

<span data-ttu-id="d619a-152">På hello **översikt** sidan av ditt webbprogram, klickar du på hello URL tooopen hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d619a-152">On hello **Overview** page of your web app, click hello URL tooopen hello web app.</span></span>

![Hämta hello URL för ditt webbprogram](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

<span data-ttu-id="d619a-154">Du bör se hello realtid temperatur- och fuktighetskonsekvens data från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d619a-154">You should see hello real-time temperature and humidity data from your IoT hub.</span></span>

![Appen webbsidan visar realtid temperatur- och fuktighetskonsekvens](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> <span data-ttu-id="d619a-156">Kontrollera hello exempelprogrammet körs på enheten.</span><span class="sxs-lookup"><span data-stu-id="d619a-156">Ensure hello sample application is running on your device.</span></span> <span data-ttu-id="d619a-157">Om inte, får du ett tomt diagram, kan du läsa toohello självstudier under [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d619a-157">If not, you will get a blank chart, you can refer toohello tutorials under [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d619a-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d619a-158">Next steps</span></span>
<span data-ttu-id="d619a-159">Du har använt sensordata i realtid din web app toovisualize från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d619a-159">You've successfully used your web app toovisualize real-time sensor data from your IoT hub.</span></span>

<span data-ttu-id="d619a-160">Ett annat sätt toovisualize data från Azure IoT Hub, se [Använd Power BI toovisualize realtid sensordata från IoT-hubb](iot-hub-live-data-visualization-in-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="d619a-160">For an alternative way toovisualize data from Azure IoT Hub, see [Use Power BI toovisualize real-time sensor data from your IoT hub](iot-hub-live-data-visualization-in-power-bi.md).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
