---
title: "Realtidsdata visualisering av sensordata från din Azure IoT-hubb – Web Apps | Microsoft Docs"
description: "Använd funktionen Web Apps i Microsoft Azure App Service visualisera temperatur- och fuktighetskonsekvens data som samlas in från sensorn och skickas till din Iot-hubb."
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
ms.openlocfilehash: e037f5c29cabf8e5d0d3e7ded187280a0652d5c3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-the-web-apps-feature-of-azure-app-service"></a><span data-ttu-id="c097a-104">Visualisera sensordata i realtid från Azure IoT-hubben med hjälp av funktionen Web Apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c097a-104">Visualize real-time sensor data from your Azure IoT hub by using the Web Apps feature of Azure App Service</span></span>

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/5.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="c097a-106">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="c097a-106">What you learn</span></span>

<span data-ttu-id="c097a-107">I kursen får lära du att visualisera sensordata i realtid som din IoT-hubb som tar emot genom att köra ett program som finns på en webbapp.</span><span class="sxs-lookup"><span data-stu-id="c097a-107">In this tutorial, you learn how to visualize real-time sensor data that your IoT hub receives by running a web application that is hosted on a web app.</span></span> <span data-ttu-id="c097a-108">Om du vill försöka visualisera data i din IoT-hubb med hjälp av Power BI, se [Använd Power BI att visualisera sensordata i realtid från Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="c097a-108">If you want to try to visualize the data in your IoT hub by using Power BI, see [Use Power BI to visualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="c097a-109">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="c097a-109">What you do</span></span>

- <span data-ttu-id="c097a-110">Skapa en webbapp i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c097a-110">Create a web app in the Azure portal.</span></span>
- <span data-ttu-id="c097a-111">Förbereda din IoT-hubb för åtkomst till data genom att lägga till en konsumentgrupp.</span><span class="sxs-lookup"><span data-stu-id="c097a-111">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="c097a-112">Konfigurera webbappen för att läsa sensordata från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c097a-112">Configure the web app to read sensor data from your IoT hub.</span></span>
- <span data-ttu-id="c097a-113">Ladda upp ett webbprogram kan hanteras på webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="c097a-113">Upload a web application to be hosted by the web app.</span></span>
- <span data-ttu-id="c097a-114">Öppna webbapp om du vill se temperatur- och fuktighetskonsekvens realtidsdata från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c097a-114">Open the web app to see real-time temperature and humidity data from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="c097a-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="c097a-115">What you need</span></span>

- <span data-ttu-id="c097a-116">[Konfigurera din enhet](iot-hub-raspberry-pi-kit-node-get-started.md), som omfattar följande krav:</span><span class="sxs-lookup"><span data-stu-id="c097a-116">[Set up your device](iot-hub-raspberry-pi-kit-node-get-started.md), which covers the following requirements:</span></span>
  - <span data-ttu-id="c097a-117">En aktiv Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c097a-117">An active Azure subscription</span></span>
  - <span data-ttu-id="c097a-118">En Iot-hubb i din prenumeration</span><span class="sxs-lookup"><span data-stu-id="c097a-118">An Iot hub under your subscription</span></span>
  - <span data-ttu-id="c097a-119">Ett klientprogram som skickar meddelanden till din Iot-hubb</span><span class="sxs-lookup"><span data-stu-id="c097a-119">A client application that sends messages to your Iot hub</span></span>
- [<span data-ttu-id="c097a-120">Hämta Git</span><span class="sxs-lookup"><span data-stu-id="c097a-120">Download Git</span></span>](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a><span data-ttu-id="c097a-121">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="c097a-121">Create a web app</span></span>

1. <span data-ttu-id="c097a-122">I den [Azure-portalen](https://ms.portal.azure.com/), klickar du på **ny** > **webb + mobilt** > **Web App**.</span><span class="sxs-lookup"><span data-stu-id="c097a-122">In the [Azure portal](https://ms.portal.azure.com/), click **New** > **Web + Mobile** > **Web App**.</span></span>
2. <span data-ttu-id="c097a-123">Ange ett unikt jobbnamn, kontrollera prenumerationen, ange en resursgrupp och en plats, väljer **fäst på instrumentpanelen**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c097a-123">Enter a unique job name, verify the subscription, specify a resource group and a location, select **Pin to dashboard**, and then click **Create**.</span></span>

   <span data-ttu-id="c097a-124">Vi rekommenderar att du väljer på samma plats som resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c097a-124">We recommend that you select the same location as that of your resource group.</span></span> <span data-ttu-id="c097a-125">Gör detta hjälper till med bearbetningshastigheten och minskar kostnaden för dataöverföring.</span><span class="sxs-lookup"><span data-stu-id="c097a-125">Doing so assists with processing speed and reduces the cost of data transfer.</span></span>

   ![Skapa en webbapp](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-the-web-app-to-read-data-from-your-iot-hub"></a><span data-ttu-id="c097a-127">Konfigurera webbappen för att läsa data från IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="c097a-127">Configure the web app to read data from your IoT hub</span></span>

1. <span data-ttu-id="c097a-128">Öppna webbapp som du precis har etablerats.</span><span class="sxs-lookup"><span data-stu-id="c097a-128">Open the web app you’ve just provisioned.</span></span>
2. <span data-ttu-id="c097a-129">Klicka på **programinställningar**, och under **appinställningar**, Lägg till följande nyckel/värde-par:</span><span class="sxs-lookup"><span data-stu-id="c097a-129">Click **Application settings**, and then, under **App settings**, add the following key/value pairs:</span></span>

   | <span data-ttu-id="c097a-130">Nyckel</span><span class="sxs-lookup"><span data-stu-id="c097a-130">Key</span></span>                                   | <span data-ttu-id="c097a-131">Värde</span><span class="sxs-lookup"><span data-stu-id="c097a-131">Value</span></span>                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | <span data-ttu-id="c097a-132">Azure.IoT.IoTHub.ConnectionString</span><span class="sxs-lookup"><span data-stu-id="c097a-132">Azure.IoT.IoTHub.ConnectionString</span></span>     | <span data-ttu-id="c097a-133">Hämtas från iothub explorer</span><span class="sxs-lookup"><span data-stu-id="c097a-133">Obtained from iothub-explorer</span></span>                                |
   | <span data-ttu-id="c097a-134">Azure.IoT.IoTHub.ConsumerGroup</span><span class="sxs-lookup"><span data-stu-id="c097a-134">Azure.IoT.IoTHub.ConsumerGroup</span></span>        | <span data-ttu-id="c097a-135">Namnet på konsumentgrupp som du lägger till din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="c097a-135">The name of the consumer group that you add to your IoT hub</span></span>  |

   ![Lägger till inställningarna i ditt webbprogram med nyckel/värde-par](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

3. <span data-ttu-id="c097a-137">Klicka på **programinställningar**under **allmänna inställningar**, växla den **Web sockets** alternativ och klickar sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c097a-137">Click **Application settings**, under **General settings**, toggle the **Web sockets** option, and then click **Save**.</span></span>

   ![Växla alternativet sockets](media/iot-hub-live-data-visualization-in-web-apps/10_toggle_web_sockets.png)

## <a name="upload-a-web-application-to-be-hosted-by-the-web-app"></a><span data-ttu-id="c097a-139">Ladda upp ett webbprogram kan hanteras på webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="c097a-139">Upload a web application to be hosted by the web app</span></span>

<span data-ttu-id="c097a-140">På GitHub, har vi gjort tillgängliga ett webbprogram som visar sensordata i realtid från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c097a-140">On GitHub, we've made available a web application that displays real-time sensor data from your IoT hub.</span></span> <span data-ttu-id="c097a-141">Allt du behöver göra är att konfigurera webbprogram för att arbeta med Git-lagringsplatsen, hämta webbprogrammet från GitHub och överföra det till Azure för webbprogrammet till värden.</span><span class="sxs-lookup"><span data-stu-id="c097a-141">All you need to do is configure the web app to work with a Git repository, download the web application from GitHub, and then upload it to Azure for the web app to host.</span></span>

1. <span data-ttu-id="c097a-142">I webbappen, klickar du på **distributionsalternativ** > **Välj källa** > **lokal Git-lagringsplats**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c097a-142">In the web app, click **Deployment Options** > **Choose Source** > **Local Git Repository**, and then click **OK**.</span></span>

   ![Konfigurera web app-distribution för att använda lokal Git-lagringsplats](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. <span data-ttu-id="c097a-144">Klicka på **Distributionsbehörigheterna**, skapa ett användarnamn och lösenord som ska användas för att ansluta till Git-lagringsplats i Azure och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c097a-144">Click **Deployment Credentials**, create a user name and password to use to connect to the Git repository in Azure, and then click **Save**.</span></span>

3. <span data-ttu-id="c097a-145">Klicka på **översikt**, och anteckna värdet för **url för Git-klon**.</span><span class="sxs-lookup"><span data-stu-id="c097a-145">Click **Overview**, and note the value of **Git clone url**.</span></span>

   ![Hämta URL för Git-klon av ditt webbprogram](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

4. <span data-ttu-id="c097a-147">Öppna ett kommando eller ett terminalfönster på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="c097a-147">Open a command or terminal window on your local computer.</span></span>

5. <span data-ttu-id="c097a-148">Hämta webbprogrammet från GitHub och överföra det till Azure för webbprogrammet till värden.</span><span class="sxs-lookup"><span data-stu-id="c097a-148">Download the web app from GitHub, and upload it to Azure for the web app to host.</span></span> <span data-ttu-id="c097a-149">Det gör du genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="c097a-149">To do so, run the following commands:</span></span>

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > <span data-ttu-id="c097a-150">\<URL för Git-klon\> är URL för Git-lagringsplats som hittades på den **översikt** sidan i webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="c097a-150">\<Git clone URL\> is the URL of the Git repository found on the **Overview** page of the web app.</span></span>

## <a name="open-the-web-app-to-see-real-time-temperature-and-humidity-data-from-your-iot-hub"></a><span data-ttu-id="c097a-151">Öppna webbapp om du vill se temperatur- och fuktighetskonsekvens realtidsdata från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="c097a-151">Open the web app to see real-time temperature and humidity data from your IoT hub</span></span>

<span data-ttu-id="c097a-152">På den **översikt** sidan av ditt webbprogram, klicka på Webbadressen för att öppna webbapp.</span><span class="sxs-lookup"><span data-stu-id="c097a-152">On the **Overview** page of your web app, click the URL to open the web app.</span></span>

![Hämta URL för ditt webbprogram](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

<span data-ttu-id="c097a-154">Du bör se temperatur- och fuktighetskonsekvens realtidsdata från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c097a-154">You should see the real-time temperature and humidity data from your IoT hub.</span></span>

![Appen webbsidan visar realtid temperatur- och fuktighetskonsekvens](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> <span data-ttu-id="c097a-156">Kontrollera exempelprogrammet som körs på enheten.</span><span class="sxs-lookup"><span data-stu-id="c097a-156">Ensure the sample application is running on your device.</span></span> <span data-ttu-id="c097a-157">Om inte, får du ett tomt diagram, kan du referera till självstudier under [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c097a-157">If not, you will get a blank chart, you can refer to the tutorials under [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c097a-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c097a-158">Next steps</span></span>
<span data-ttu-id="c097a-159">Du har har använt ditt webbprogram för att visualisera sensordata i realtid från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c097a-159">You've successfully used your web app to visualize real-time sensor data from your IoT hub.</span></span>

<span data-ttu-id="c097a-160">Ett annat sätt att visualisera data från Azure IoT Hub, se [Använd Power BI att visualisera sensordata i realtid från din IoT-hubb](iot-hub-live-data-visualization-in-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="c097a-160">For an alternative way to visualize data from Azure IoT Hub, see [Use Power BI to visualize real-time sensor data from your IoT hub](iot-hub-live-data-visualization-in-power-bi.md).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
