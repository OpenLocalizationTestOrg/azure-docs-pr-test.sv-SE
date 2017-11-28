---
title: "Komma igång med förkonfigurerade lösningar | Microsoft Docs"
description: "I den här självstudiekursen lär du dig hur du distribuerar en förkonfigurerad Azure IoT Suite-lösning."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 6ab38d1a-b564-469e-8a87-e597aa51d0f7
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 466825ab78a5ac9773d8beff69cca90ff9db6c01
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-the-preconfigured-solutions"></a><span data-ttu-id="761f5-103">Kom igång med förkonfigurerade lösningar</span><span class="sxs-lookup"><span data-stu-id="761f5-103">Get started with the preconfigured solutions</span></span>

<span data-ttu-id="761f5-104">Azure IoT Suites [förkonfigurerade lösningar][lnk-preconfigured-solutions] kombinerar flera Azure IoT-tjänster för att leverera lösningar från slutpunkt till slutpunkt som implementerar vanliga IoT-företagsscenarier.</span><span class="sxs-lookup"><span data-stu-id="761f5-104">Azure IoT Suite [preconfigured solutions][lnk-preconfigured-solutions] combine multiple Azure IoT services to deliver end-to-end solutions that implement common IoT business scenarios.</span></span> <span data-ttu-id="761f5-105">Med den förkonfigurerade *fjärrövervakningslösningen* kan du ansluta till och övervaka dina enheter.</span><span class="sxs-lookup"><span data-stu-id="761f5-105">The *remote monitoring* preconfigured solution connects to and monitors your devices.</span></span> <span data-ttu-id="761f5-106">Du kan använda lösningen för att analysera dataströmmen från dina enheter och för att förbättra affärsresultat genom att konfigurera processer så att de svarar automatiskt på dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="761f5-106">You can use the solution to analyze the stream of data from your devices and to improve business outcomes by making processes respond automatically to that stream of data.</span></span>

<span data-ttu-id="761f5-107">I den här självstudiekursen lär du dig hur du etablerar den förkonfigurerade lösningen för fjärrövervakning.</span><span class="sxs-lookup"><span data-stu-id="761f5-107">This tutorial shows you how to provision the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="761f5-108">Vi går också igenom de grundläggande funktionerna i den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="761f5-108">It also walks you through the basic features of the preconfigured solution.</span></span> <span data-ttu-id="761f5-109">Du kan komma åt många av dessa funktioner från *instrumentpanelen* för lösningen som distribueras som en del av den förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="761f5-109">You can access many of these features from the solution *dashboard* that deploys as part of the preconfigured solution:</span></span>

![Instrumentpanel för den förkonfigurerade fjärrövervakningslösningen][img-dashboard]

<span data-ttu-id="761f5-111">Du behöver en aktiv Azure-prenumeration för att kunna utföra stegen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="761f5-111">To complete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="761f5-112">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="761f5-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="761f5-113">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="761f5-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="scenario-overview"></a><span data-ttu-id="761f5-114">Scenarioöversikt</span><span class="sxs-lookup"><span data-stu-id="761f5-114">Scenario overview</span></span>

<span data-ttu-id="761f5-115">När du distribuerar den fjärranslutna förinställda övervakningslösningen innehåller den redan resurser som hjälper dig att gå igenom ett vanligt scenario för fjärransluten övervakning.</span><span class="sxs-lookup"><span data-stu-id="761f5-115">When you deploy the remote monitoring preconfigured solution, it is prepopulated with resources that enable you to step through a common remote monitoring scenario.</span></span> <span data-ttu-id="761f5-116">I det här scenariot rapporterar flera enheter som är anslutna till lösningen oväntade temperaturvärden.</span><span class="sxs-lookup"><span data-stu-id="761f5-116">In this scenario, several devices connected to the solution are reporting unexpected temperature values.</span></span> <span data-ttu-id="761f5-117">I avsnitten nedan får du information om följande:</span><span class="sxs-lookup"><span data-stu-id="761f5-117">The following sections show you how to:</span></span>

* <span data-ttu-id="761f5-118">Identifiera de enheter som skickar oväntade temperaturvärden.</span><span class="sxs-lookup"><span data-stu-id="761f5-118">Identify the devices sending unexpected temperature values.</span></span>
* <span data-ttu-id="761f5-119">Konfigurera dessa enheter för att få fram mer detaljerad telemetri.</span><span class="sxs-lookup"><span data-stu-id="761f5-119">Configure these devices to send more detailed telemetry.</span></span>
* <span data-ttu-id="761f5-120">Åtgärda problemet genom att uppdatera den inbyggda programvaran på dessa enheter.</span><span class="sxs-lookup"><span data-stu-id="761f5-120">Fix the problem by updating the firmware on these devices.</span></span>
* <span data-ttu-id="761f5-121">Kontrollera att åtgärden har löst problemet.</span><span class="sxs-lookup"><span data-stu-id="761f5-121">Verify that your action has resolved the issue.</span></span>

<span data-ttu-id="761f5-122">En viktig egenskap i detta scenario är att du kan utföra alla dessa åtgärder via fjärranslutning från instrumentpanelen med lösningar.</span><span class="sxs-lookup"><span data-stu-id="761f5-122">A key feature of this scenario is that you can perform all these actions remotely from the solution dashboard.</span></span> <span data-ttu-id="761f5-123">Du behöver inte fysisk åtkomst till enheterna.</span><span class="sxs-lookup"><span data-stu-id="761f5-123">You do not need physical access to the devices.</span></span>

## <a name="view-the-solution-dashboard"></a><span data-ttu-id="761f5-124">Visa instrumentpanelen för lösningen</span><span class="sxs-lookup"><span data-stu-id="761f5-124">View the solution dashboard</span></span>

<span data-ttu-id="761f5-125">På instrumentpanelen för lösningen kan du hantera den distribuerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="761f5-125">The solution dashboard enables you to manage the deployed solution.</span></span> <span data-ttu-id="761f5-126">Du kan till exempel visa telemetri, lägga till enheter och konfigurera regler.</span><span class="sxs-lookup"><span data-stu-id="761f5-126">For example, you can view telemetry, add devices, and configure rules.</span></span>

1. <span data-ttu-id="761f5-127">När etableringen har slutförts och panelen för din förkonfigurerade lösning visar statusen **Klar** klickar du på **Starta** så öppnas portalen för fjärrövervakningslösningen på en ny flik.</span><span class="sxs-lookup"><span data-stu-id="761f5-127">When the provisioning is complete and the tile for your preconfigured solution indicates **Ready**, choose **Launch** to open your remote monitoring solution portal in a new tab.</span></span>

    ![Starta den förkonfigurerade lösningen][img-launch-solution]

1. <span data-ttu-id="761f5-129">Som standard visar lösningsportalen *instrumentpanelen*.</span><span class="sxs-lookup"><span data-stu-id="761f5-129">By default, the solution portal shows the *dashboard*.</span></span> <span data-ttu-id="761f5-130">Du kan navigera till andra delar av lösningsportalen via menyn till vänster på sidan.</span><span class="sxs-lookup"><span data-stu-id="761f5-130">You can navigate to other areas of the solution portal using the menu on the left-hand side of the page.</span></span>

    ![Instrumentpanel för den förkonfigurerade fjärrövervakningslösningen][img-menu]

<span data-ttu-id="761f5-132">Följande information visas på instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="761f5-132">The dashboard displays the following information:</span></span>

* <span data-ttu-id="761f5-133">En karta visar platsen för alla enheter som är kopplade till lösningen.</span><span class="sxs-lookup"><span data-stu-id="761f5-133">A map that displays the location of each device connected to the solution.</span></span> <span data-ttu-id="761f5-134">Första gången du kör lösningen finns det 25 simulerade enheter.</span><span class="sxs-lookup"><span data-stu-id="761f5-134">When you first run the solution, there are 25 simulated devices.</span></span> <span data-ttu-id="761f5-135">De simulerade enheterna implementeras som Azure WebJobs och lösningen använder Bing Maps-API:et för att rita information på kartan.</span><span class="sxs-lookup"><span data-stu-id="761f5-135">The simulated devices are implemented as Azure WebJobs, and the solution uses the Bing Maps API to plot information on the map.</span></span> <span data-ttu-id="761f5-136">Se [vanliga frågor och svar][lnk-faq] för information om hur du gör kartan dynamisk.</span><span class="sxs-lookup"><span data-stu-id="761f5-136">See the [FAQ][lnk-faq] to learn how to make the map dynamic.</span></span>
* <span data-ttu-id="761f5-137">Panelen **Telemetrihistorik** ritar upp fuktighets- och temperaturtelemetri från en vald enhet nästan i realtid och visar aggregerade data, till exempel högsta, lägsta och genomsnittlig fuktighet.</span><span class="sxs-lookup"><span data-stu-id="761f5-137">A **Telemetry History** panel that plots humidity and temperature telemetry from a selected device in near real time and displays aggregate data such as maximum, minimum, and average humidity.</span></span>
* <span data-ttu-id="761f5-138">Panelen **Larmhistorik** visar de senaste larmhändelserna när ett telemetrivärde har överskridit ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="761f5-138">An **Alarm History** panel that shows recent alarm events when a telemetry value has exceeded a threshold.</span></span> <span data-ttu-id="761f5-139">Du kan definiera dina egna larm förutom exemplen som skapas med den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="761f5-139">You can define your own alarms in addition to the examples created by the preconfigured solution.</span></span>
* <span data-ttu-id="761f5-140">På panelen **Jobb** visas information om schemalagda jobb.</span><span class="sxs-lookup"><span data-stu-id="761f5-140">A **Jobs** panel that displays information about scheduled jobs.</span></span> <span data-ttu-id="761f5-141">Du kan schemalägga egna jobb på sidan **Hanteringsjobb**.</span><span class="sxs-lookup"><span data-stu-id="761f5-141">You can schedule your own jobs on **Management jobs** page.</span></span>

## <a name="view-alarms"></a><span data-ttu-id="761f5-142">Visa larm</span><span class="sxs-lookup"><span data-stu-id="761f5-142">View alarms</span></span>

<span data-ttu-id="761f5-143">Panelen med larmhistorik visar att fem enheter rapporterar högre telemetrivärden än förväntat.</span><span class="sxs-lookup"><span data-stu-id="761f5-143">The alarm history panel shows you that five devices are reporting higher than expected telemetry values.</span></span>

![TODO-larmhistorik på instrumentpanelen för lösningen][img-alarms]

> [!NOTE]
> <span data-ttu-id="761f5-145">Dessa larm genereras av en regel som ingår i den förinställda lösningen.</span><span class="sxs-lookup"><span data-stu-id="761f5-145">These alarms are generated by a rule that is included in the preconfigured solution.</span></span> <span data-ttu-id="761f5-146">Den här regeln genererar en avisering när temperaturvärdet som skickas av en enhet överstiger 60.</span><span class="sxs-lookup"><span data-stu-id="761f5-146">This rule generates an alert when the temperature value sent by a device exceeds 60.</span></span> <span data-ttu-id="761f5-147">Du kan definiera egna regler och åtgärder genom att välja [Regler](#add-a-rule) och [Åtgärder](#add-an-action) i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="761f5-147">You can define your own rules and actions by choosing [Rules](#add-a-rule) and [Actions](#add-an-action) in the left-hand menu.</span></span>

## <a name="view-devices"></a><span data-ttu-id="761f5-148">Visa enheter</span><span class="sxs-lookup"><span data-stu-id="761f5-148">View devices</span></span>

<span data-ttu-id="761f5-149">I listan *Enheter* visas alla registrerade enheter i lösningen.</span><span class="sxs-lookup"><span data-stu-id="761f5-149">The *devices* list shows all the registered devices in the solution.</span></span> <span data-ttu-id="761f5-150">Från enhetslistan kan du se och redigera enhetsmetadata, lägga till eller ta bort enheter och anropa metoder på enheter.</span><span class="sxs-lookup"><span data-stu-id="761f5-150">From the device list you can view and edit device metadata, add or remove devices, and invoke methods on devices.</span></span> <span data-ttu-id="761f5-151">Du kan filtrera och sortera listan över enheter i enhetslistan.</span><span class="sxs-lookup"><span data-stu-id="761f5-151">You can filter and sort the list of devices in the device list.</span></span> <span data-ttu-id="761f5-152">Du kan också anpassa de kolumner som visas i enhetslistan.</span><span class="sxs-lookup"><span data-stu-id="761f5-152">You can also customize the columns shown in the device list.</span></span>

1. <span data-ttu-id="761f5-153">Välj **Enheter** om du vill visa listan över enheter för den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="761f5-153">Choose **Devices** to show the device list for this solution.</span></span>

   ![Visa enhetslistan i lösningsportalen][img-devicelist]

1. <span data-ttu-id="761f5-155">I enhetslistan visas inledningsvis 25 simulerade enheter som har skapats vid etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="761f5-155">The device list initially shows 25 simulated devices created by the provisioning process.</span></span> <span data-ttu-id="761f5-156">Du kan lägga till ytterligare simulerade och fysiska enheter till lösningen.</span><span class="sxs-lookup"><span data-stu-id="761f5-156">You can add additional simulated and physical devices to the solution.</span></span>

1. <span data-ttu-id="761f5-157">Välj en enhet i enhetslistan om du vill visa information om enheten.</span><span class="sxs-lookup"><span data-stu-id="761f5-157">To view the details of a device, choose a device in the device list.</span></span>

   ![Visa information om enheten i lösningsportalen][img-devicedetails]

<span data-ttu-id="761f5-159">Panelen **Enhetsinformation** innehåller sex delar:</span><span class="sxs-lookup"><span data-stu-id="761f5-159">The **Device Details** panel contains six sections:</span></span>

* <span data-ttu-id="761f5-160">En uppsättning länkar som gör att du kan anpassa på enhetsikonen, inaktivera enheten, lägga till en regel, anropa en metod eller skicka ett kommando.</span><span class="sxs-lookup"><span data-stu-id="761f5-160">A collection of links that enable you to customize the device icon, disable the device, add a rule, invoke a method, or send a command.</span></span> <span data-ttu-id="761f5-161">En jämförelse av kommandon (meddelanden från enheten till molnet) och metoder (direkta metoder) finns i [Cloud-to-device communications guidance][lnk-c2d-guidance] (Vägledning för kommunikation från moln till enhet).</span><span class="sxs-lookup"><span data-stu-id="761f5-161">For a comparison of commands (device-to-cloud messages) and methods (direct methods), see [Cloud-to-device communications guidance][lnk-c2d-guidance].</span></span>
* <span data-ttu-id="761f5-162">Avsnittet **Enhetstvilling – taggar** gör det möjligt att redigera taggvärden för enheten.</span><span class="sxs-lookup"><span data-stu-id="761f5-162">The **Device Twin - Tags** section enables you to edit tag values for the device.</span></span> <span data-ttu-id="761f5-163">Du kan visa taggvärden i enhetslistan och använda taggvärden för att filtrera enhetslistan.</span><span class="sxs-lookup"><span data-stu-id="761f5-163">You can display tag values in the device list and use tag values to filter the device list.</span></span>
* <span data-ttu-id="761f5-164">Avsnittet **Enhetstvilling – Önskade egenskaper** gör det möjligt för dig att ange egenskapsvärden som ska skickas till enheten.</span><span class="sxs-lookup"><span data-stu-id="761f5-164">The **Device Twin - Desired Properties** section enables you to set property values to be sent to the device.</span></span>
* <span data-ttu-id="761f5-165">Avsnittet **Enhetstvilling – Rapporterade egenskaper** visar egenskapsvärden som skickas från enheten.</span><span class="sxs-lookup"><span data-stu-id="761f5-165">The **Device Twin - Reported Properties** section shows property values sent from the device.</span></span>
* <span data-ttu-id="761f5-166">Avsnittet **Enhetsegenskaper** visar information från identitetsregistret, till exempel enhetens ID och autentiseringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="761f5-166">The **Device Properties** section shows information from the identity registry such as the device id and authentication keys.</span></span>
* <span data-ttu-id="761f5-167">Avsnittet **Senaste jobb** visar information om alla jobb som nyligen haft den här enheten som mål.</span><span class="sxs-lookup"><span data-stu-id="761f5-167">The **Recent Jobs** section shows information about any jobs that have recently targeted this device.</span></span>

## <a name="filter-the-device-list"></a><span data-ttu-id="761f5-168">Filtrera enhetslistan</span><span class="sxs-lookup"><span data-stu-id="761f5-168">Filter the device list</span></span>

<span data-ttu-id="761f5-169">Du kan använda ett filter om du vill visa enbart de enheter som skickar oväntade temperaturvärden.</span><span class="sxs-lookup"><span data-stu-id="761f5-169">You can use a filter to display only those devices that are sending unexpected temperature values.</span></span> <span data-ttu-id="761f5-170">Den fjärranslutna förinställda övervakningslösningen innehåller filtret **Ej felfria enheter** som används för att visa enheter med ett medeltemperaturvärde högre än 60.</span><span class="sxs-lookup"><span data-stu-id="761f5-170">The remote monitoring preconfigured solution includes the **Unhealthy devices** filter to show devices with a mean temperature value greater than 60.</span></span> <span data-ttu-id="761f5-171">Du kan även [skapa egna filter](#add-a-filter).</span><span class="sxs-lookup"><span data-stu-id="761f5-171">You can also [create your own filters](#add-a-filter).</span></span>

1. <span data-ttu-id="761f5-172">Välj **Öppna sparat filter** så visas en lista över tillgängliga filter.</span><span class="sxs-lookup"><span data-stu-id="761f5-172">Choose **Open saved filter** to display a list of available filters.</span></span> <span data-ttu-id="761f5-173">Välj sedan **Ej felfria enheter** för att tillämpa filtret:</span><span class="sxs-lookup"><span data-stu-id="761f5-173">Then choose **Unhealthy devices** to apply the filter:</span></span>

    ![Visa listan över filter][img-unhealthy-filter]

1. <span data-ttu-id="761f5-175">I enhetslistan visas nu endast enheter med ett medeltemperaturvärde högre än 60.</span><span class="sxs-lookup"><span data-stu-id="761f5-175">The device list now shows only devices with a mean temperature value greater than 60.</span></span>

    ![Visa listan med filtrerade enheter, där enheter med feltillstånd visas][img-filtered-unhealthy-list]

## <a name="update-desired-properties"></a><span data-ttu-id="761f5-177">Uppdatera önskade egenskaper</span><span class="sxs-lookup"><span data-stu-id="761f5-177">Update desired properties</span></span>

<span data-ttu-id="761f5-178">Nu har du identifierat en uppsättning enheter som kan behöva repareras.</span><span class="sxs-lookup"><span data-stu-id="761f5-178">You have now identified a set of devices that may need remediation.</span></span> <span data-ttu-id="761f5-179">Du anser dock att datafrekvensen på 15 sekunder inte räcker för en tydlig diagnos av problemet.</span><span class="sxs-lookup"><span data-stu-id="761f5-179">However, you decide that the data frequency of 15 seconds is not sufficient for a clear diagnosis of the issue.</span></span> <span data-ttu-id="761f5-180">Du vill ändra telemetrifrekvensen till fem sekunder så att du får fler datapunkter för att kunna diagnostisera problemet på ett bättre sätt.</span><span class="sxs-lookup"><span data-stu-id="761f5-180">Changing the telemetry frequency to five seconds to provide you with more data points to better diagnose the issue.</span></span> <span data-ttu-id="761f5-181">Du kan skicka konfigurationsändringen till din fjärranslutna enheter från lösningsportalen.</span><span class="sxs-lookup"><span data-stu-id="761f5-181">You can push this configuration change to your remote devices from the solution portal.</span></span> <span data-ttu-id="761f5-182">Du kan göra ändringen en gång, utvärdera effekterna och sedan agera utifrån resultaten.</span><span class="sxs-lookup"><span data-stu-id="761f5-182">You can make the change once, evaluate the impact, and then act on the results.</span></span>

<span data-ttu-id="761f5-183">Följ stegen nedan för att köra ett jobb som förändrar den önskade **TelemetryInterval**-egenskapen för de berörda enheterna.</span><span class="sxs-lookup"><span data-stu-id="761f5-183">Follow these steps to run a job that changes the **TelemetryInterval** desired property for the affected devices.</span></span> <span data-ttu-id="761f5-184">När enheterna får det nya **TelemetryInterval**-egenskapsvärdet ändrar de konfigurationen så att telemetri skickas var femte sekund i stället för var 15:e sekund:</span><span class="sxs-lookup"><span data-stu-id="761f5-184">When the devices receive the new **TelemetryInterval** property value, they change their configuration to send telemetry every five seconds instead of every 15 seconds:</span></span>

1. <span data-ttu-id="761f5-185">När du visar listan över enheter med feltillstånd i enhetslistan väljer du **Jobbschema**, och sedan **Redigera enhetstvilling**.</span><span class="sxs-lookup"><span data-stu-id="761f5-185">While you are showing the list of unhealthy devices in the device list, choose **Job Scheduler**, then **Edit Device Twin**.</span></span>

1. <span data-ttu-id="761f5-186">Anropa jobbet **Ändra telemetriintervall**.</span><span class="sxs-lookup"><span data-stu-id="761f5-186">Call the job **Change telemetry interval**.</span></span>

1. <span data-ttu-id="761f5-187">Ändra värdet för namnet **Önskad egenskap** **desired.Config.TelemetryInterval** till fem sekunder.</span><span class="sxs-lookup"><span data-stu-id="761f5-187">Change the value of the **Desired Property** name **desired.Config.TelemetryInterval** to five seconds.</span></span>

1. <span data-ttu-id="761f5-188">Välj **Schema**.</span><span class="sxs-lookup"><span data-stu-id="761f5-188">Choose **Schedule**.</span></span>

    ![Ändra egenskapen TelemetryInterval till fem sekunder][img-change-interval]

1. <span data-ttu-id="761f5-190">Du kan övervaka status för det jobb som du schemalagt på sidan **Hanteringsjobb** i portalen.</span><span class="sxs-lookup"><span data-stu-id="761f5-190">You can monitor the progress of the job on the **Management Jobs** page in the portal.</span></span>

> [!NOTE]
> <span data-ttu-id="761f5-191">Om du vill ändra ett önskat egenskapsvärde för en enskild enhet använder du avsnittet **Önskade egenskaper** på panelen **Enhetsinformation** i stället för att köra ett jobb.</span><span class="sxs-lookup"><span data-stu-id="761f5-191">If you want to change a desired property value for an individual device, use the **Desired Properties** section in the **Device Details** panel instead of running a job.</span></span>

<span data-ttu-id="761f5-192">Det här jobbet anger värdet för den önskade **TelemetryInterval**-egenskapen hos enhetstvillingen för alla enheter som valts med hjälp av filtret.</span><span class="sxs-lookup"><span data-stu-id="761f5-192">This job sets the value of the **TelemetryInterval** desired property in the device twin for all the devices selected by the filter.</span></span> <span data-ttu-id="761f5-193">Enheterna hämtar det här värdet från enhetstvillingen och uppdaterar beteendet.</span><span class="sxs-lookup"><span data-stu-id="761f5-193">The devices retrieve this value from the device twin and update their behavior.</span></span> <span data-ttu-id="761f5-194">När en enhet hämtar och bearbetar en önskad egenskap från en enhetstvilling anger den motsvarande rapporterade värdeegenskap.</span><span class="sxs-lookup"><span data-stu-id="761f5-194">When a device retrieves and processes a desired property from a device twin, it sets the corresponding reported value property.</span></span>

## <a name="invoke-methods"></a><span data-ttu-id="761f5-195">Anropningsmetoder</span><span class="sxs-lookup"><span data-stu-id="761f5-195">Invoke methods</span></span>

<span data-ttu-id="761f5-196">När jobbet körs du ser i listan över enheter med feltillstånd att alla dessa enheter har gamla (tidigare än version 1.6) versioner av inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="761f5-196">While the job runs, you notice in the list of unhealthy devices that all these devices have old (less than version 1.6) firmware versions.</span></span>

![Visa den rapporterade versionen av den inbyggda programvaran för enheter med feltillstånd][img-old-firmware]

<span data-ttu-id="761f5-198">Denna version av inbyggd programvara kan vara grundorsaken till oväntade temperaturvärden eftersom du vet att andra felfria enheter nyligen har uppdaterats till version 2.0.</span><span class="sxs-lookup"><span data-stu-id="761f5-198">This firmware version may be the root cause of the unexpected temperature values because you know that other healthy devices were recently updated to version 2.0.</span></span> <span data-ttu-id="761f5-199">Du kan använda det inbyggda filtret **Old firmware devices** (Enheter med föråldrad inbyggd programvara) för att identifiera alla enheter med äldre versioner av inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="761f5-199">You can use the built-in **Old firmware devices** filter to identify any devices with old firmware versions.</span></span> <span data-ttu-id="761f5-200">Från portalen kan du sedan fjärruppdatera alla enheter som fortfarande kör äldre versioner av inbyggd programvara:</span><span class="sxs-lookup"><span data-stu-id="761f5-200">From the portal, you can then remotely update all the devices still running old firmware versions:</span></span>

1. <span data-ttu-id="761f5-201">Välj **Öppna sparat filter** så visas en lista över tillgängliga filter.</span><span class="sxs-lookup"><span data-stu-id="761f5-201">Choose **Open saved filter** to display a list of available filters.</span></span> <span data-ttu-id="761f5-202">Välj sedan **Old firmware devices** (Enheter med föråldrad inbyggd programvara) och tillämpa filtret:</span><span class="sxs-lookup"><span data-stu-id="761f5-202">Then choose **Old firmware devices** to apply the filter:</span></span>

    ![Visa listan över filter][img-old-filter]

1. <span data-ttu-id="761f5-204">I enhetslistan visas nu endast enheter med äldre versioner av inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="761f5-204">The device list now shows only devices with old firmware versions.</span></span> <span data-ttu-id="761f5-205">Listan innehåller fem enheter som har identifieras av filtret **Ej felfria enheter** och tre ytterligare enheter:</span><span class="sxs-lookup"><span data-stu-id="761f5-205">This list includes the five devices identified by the **Unhealthy devices** filter and three additional devices:</span></span>

    ![Visa listan över filtrerade enheter där gamla enheter visas][img-filtered-old-list]

1. <span data-ttu-id="761f5-207">Välj **Jobbschema**, sedan **Anropa metod**.</span><span class="sxs-lookup"><span data-stu-id="761f5-207">Choose **Job Scheduler**, then **Invoke Method**.</span></span>

1. <span data-ttu-id="761f5-208">Ange **Jobbnamn** till **Firmware update to version 2.0** (Uppdatera inbyggd programvara till version 2.0).</span><span class="sxs-lookup"><span data-stu-id="761f5-208">Set **Job Name** to **Firmware update to version 2.0**.</span></span>

1. <span data-ttu-id="761f5-209">Välj **InitiateFirmwareUpdate** som **Metod**.</span><span class="sxs-lookup"><span data-stu-id="761f5-209">Choose **InitiateFirmwareUpdate** as the **Method**.</span></span>

1. <span data-ttu-id="761f5-210">Ange parametern **FwPackageUri** till **https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.</span><span class="sxs-lookup"><span data-stu-id="761f5-210">Set the **FwPackageUri** parameter to **https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.</span></span>

1. <span data-ttu-id="761f5-211">Välj **Schema**.</span><span class="sxs-lookup"><span data-stu-id="761f5-211">Choose **Schedule**.</span></span> <span data-ttu-id="761f5-212">Standardvärdet är att jobbet ska köras nu.</span><span class="sxs-lookup"><span data-stu-id="761f5-212">The default is for the job to run now.</span></span>

    ![Skapa jobb för att uppdatera den inbyggda programvaran för de valda enheterna][img-method-update]

> [!NOTE]
> <span data-ttu-id="761f5-214">Om du vill anropa en metod i en enskild enhet väljer du **Metoder** på panelen **Enhetsinformation** i stället för att köra ett jobb.</span><span class="sxs-lookup"><span data-stu-id="761f5-214">If you want to invoke a method on an individual device, choose **Methods** in the **Device Details** panel instead of running a job.</span></span>

<span data-ttu-id="761f5-215">Det här jobbet anropar direktmetoden **InitiateFirmwareUpdate** på alla enheter som har valts med filtret.</span><span class="sxs-lookup"><span data-stu-id="761f5-215">This job invokes the **InitiateFirmwareUpdate** direct method on all the devices selected by the filter.</span></span> <span data-ttu-id="761f5-216">Enheterna svarar omedelbart till IoT Hub och startar sedan uppdateringsprocessen för den fasta programvaran asynkront.</span><span class="sxs-lookup"><span data-stu-id="761f5-216">Devices respond immediately to IoT Hub and then initiate the firmware update process asynchronously.</span></span> <span data-ttu-id="761f5-217">Enheterna visar statusinformation om uppdateringen av den fasta programvaran genom de rapporterade egenskapsvärdena, som visas på följande skärmdumpar.</span><span class="sxs-lookup"><span data-stu-id="761f5-217">The devices provide status information about the firmware update process through reported property values, as shown in the following screenshots.</span></span> <span data-ttu-id="761f5-218">Välj ikonen **Uppdatera** för att uppdatera information i enhets- och jobblistorna:</span><span class="sxs-lookup"><span data-stu-id="761f5-218">Choose the **Refresh** icon to update the information in the device and job lists:</span></span>

<span data-ttu-id="761f5-219">![Jobblista där en lista för uppdatering av inbyggd programvara körs][img-update-1]
![Lista över enheter där uppdateringsstatus för inbyggd programvara visas][img-update-2]
![Jobblista som visar att listan över uppdatering av inbyggd programvara har slutförts][img-update-3]</span><span class="sxs-lookup"><span data-stu-id="761f5-219">![Job list showing the firmware update list running][img-update-1]
![Device list showing firmware update status][img-update-2]
![Job list showing the firmware update list complete][img-update-3]</span></span>

> [!NOTE]
> <span data-ttu-id="761f5-220">Du kan schemalägga jobb att köras under en angiven underhållsperiod i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="761f5-220">In a production environment, you can schedule jobs to run during a designated maintenance window.</span></span>

## <a name="scenario-review"></a><span data-ttu-id="761f5-221">Scenariogranskning</span><span class="sxs-lookup"><span data-stu-id="761f5-221">Scenario review</span></span>

<span data-ttu-id="761f5-222">I det här scenariot har du identifierat ett potentiellt problem med vissa fjärranslutna enheter m ed hjälp av larmhistoriken på instrumentpanelen och ett filter.</span><span class="sxs-lookup"><span data-stu-id="761f5-222">In this scenario, you identified a potential issue with some of your remote devices using the alarm history on the dashboard and a filter.</span></span> <span data-ttu-id="761f5-223">Du använde sedan filtret och ett jobb för att fjärrkonfigurera enheterna och få fram mer information för att diagnostisera problemet.</span><span class="sxs-lookup"><span data-stu-id="761f5-223">You then used the filter and a job to remotely configure the devices to provide more information to help diagnose the issue.</span></span> <span data-ttu-id="761f5-224">Slutligen kan använde du ett filter och ett jobb för att schemalägga underhåll av de berörda enheterna.</span><span class="sxs-lookup"><span data-stu-id="761f5-224">Finally, you used a filter and a job to schedule maintenance on the affected devices.</span></span> <span data-ttu-id="761f5-225">Om du går tillbaka till instrumentpanelen kan du kontrollera att det inte längre finns några larm som kommer från enheter i din lösning.</span><span class="sxs-lookup"><span data-stu-id="761f5-225">If you return to the dashboard, you can check that there are no longer any alarms coming from devices in your solution.</span></span> <span data-ttu-id="761f5-226">Du kan använda ett filter för att kontrollera att den inbyggda programvaran är uppdaterad på alla enheter i din lösning och att det inte finns några enheter med fel:</span><span class="sxs-lookup"><span data-stu-id="761f5-226">You can use a filter to verify that the firmware is up-to-date on all the devices in your solution and that there are no more unhealthy devices:</span></span>

![Filter som visar att alla enheter som har uppdaterad inbyggd programvara][img-updated]

![Filter som visar att alla enheter är felfria][img-healthy]

## <a name="other-features"></a><span data-ttu-id="761f5-229">Andra funktioner</span><span class="sxs-lookup"><span data-stu-id="761f5-229">Other features</span></span>

<span data-ttu-id="761f5-230">I följande avsnitt beskrivs vissa ytterligare funktioner för den fjärranslutna förinställda övervakningslösning som inte beskrivs som en del av scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="761f5-230">The following sections describe some additional features of the remote monitoring preconfigured solution that are not described as part of the previous scenario.</span></span>

### <a name="customize-columns"></a><span data-ttu-id="761f5-231">Anpassa kolumner</span><span class="sxs-lookup"><span data-stu-id="761f5-231">Customize columns</span></span>

<span data-ttu-id="761f5-232">Du kan anpassa informationen som visas i enhetslistan genom att klicka på **Kolumnredigerare**.</span><span class="sxs-lookup"><span data-stu-id="761f5-232">You can customize the information shown in the device list by choosing **Column editor**.</span></span> <span data-ttu-id="761f5-233">Du kan lägga till och ta bort kolumner som visar rapporterade egenskap- och taggvärden.</span><span class="sxs-lookup"><span data-stu-id="761f5-233">You can add and remove columns that display reported property and tag values.</span></span> <span data-ttu-id="761f5-234">Du kan även ändra ordningen och byta namn på kolumner:</span><span class="sxs-lookup"><span data-stu-id="761f5-234">You can also reorder and rename columns:</span></span>

   ![Ikonen för kolumnredigeraren med listan över enheter][img-columneditor]

### <a name="customize-the-device-icon"></a><span data-ttu-id="761f5-236">Anpassa enhetsikonen</span><span class="sxs-lookup"><span data-stu-id="761f5-236">Customize the device icon</span></span>

<span data-ttu-id="761f5-237">Du kan anpassa enhetsikonen som visas i enhetslistan från panelen **Enhetsinformation** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="761f5-237">You can customize the device icon displayed in the device list from the **Device Details** panel as follows:</span></span>

1. <span data-ttu-id="761f5-238">Klicka på pennikonen så öppnas panelen **Redigera avbildning** för en enhet:</span><span class="sxs-lookup"><span data-stu-id="761f5-238">Choose the pencil icon to open the **Edit image** panel for a device:</span></span>

   ![Öppna redigeringsprogrammet för enhetsavbildningar][img-startimageedit]

1. <span data-ttu-id="761f5-240">Ladda upp en ny avbildning eller använd en befintlig avbildning och klicka sedan på **Spara**:</span><span class="sxs-lookup"><span data-stu-id="761f5-240">Either upload a new image or use one of the existing images and then choose **Save**:</span></span>

   ![Redigera redigeringsprogrammet för enhetsavbildningar][img-imageedit]

1. <span data-ttu-id="761f5-242">Den valda avbildningen visas nu i kolumnen **Ikon** för enheten.</span><span class="sxs-lookup"><span data-stu-id="761f5-242">The image you selected now displays in the **Icon** column for the device.</span></span>

> [!NOTE]
> <span data-ttu-id="761f5-243">Avbildningen lagras i Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="761f5-243">The image is stored in blob storage.</span></span> <span data-ttu-id="761f5-244">En tagg i enhetstvillingen innehåller en länk till avbildning i Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="761f5-244">A tag in the device twin contains a link to the image in blob storage.</span></span>

### <a name="add-a-device"></a><span data-ttu-id="761f5-245">Lägg till en enhet</span><span class="sxs-lookup"><span data-stu-id="761f5-245">Add a device</span></span>

<span data-ttu-id="761f5-246">När du distribuerar den förkonfigurerade lösningen etablerar du automatiskt 25 exempelenheter som du ser i enhetslistan.</span><span class="sxs-lookup"><span data-stu-id="761f5-246">When you deploy the preconfigured solution, you automatically provision 25 sample devices that you can see in the device list.</span></span> <span data-ttu-id="761f5-247">Dessa enheter är *simulerade enheter* som körs i ett Azure-webbjobb.</span><span class="sxs-lookup"><span data-stu-id="761f5-247">These devices are *simulated devices* running in an Azure WebJob.</span></span> <span data-ttu-id="761f5-248">Simulerade enheter gör det lätt att experimentera med den förkonfigurerade lösningen utan att behöva distribuera verkliga fysiska enheter.</span><span class="sxs-lookup"><span data-stu-id="761f5-248">Simulated devices make it easy for you to experiment with the preconfigured solution without the need to deploy real, physical devices.</span></span> <span data-ttu-id="761f5-249">Om du vill ansluta en verklig enhet till lösningen går du självstudiekursen [Ansluta enheten till den förkonfigurerade fjärrövervakningslösningen][lnk-connect-rm].</span><span class="sxs-lookup"><span data-stu-id="761f5-249">If you do want to connect a real device to the solution, see the [Connect your device to the remote monitoring preconfigured solution][lnk-connect-rm] tutorial.</span></span>

<span data-ttu-id="761f5-250">Följande steg beskriver hur du lägger till en simulerad enhet i lösningen:</span><span class="sxs-lookup"><span data-stu-id="761f5-250">The following steps show you how to add a simulated device to the solution:</span></span>

1. <span data-ttu-id="761f5-251">Gå tillbaka till enhetslistan.</span><span class="sxs-lookup"><span data-stu-id="761f5-251">Navigate back to the device list.</span></span>

1. <span data-ttu-id="761f5-252">Klicka på **+ Lägg till en enhet** i det nedre vänstra hörnet för att lägga till en enhet.</span><span class="sxs-lookup"><span data-stu-id="761f5-252">To add a device, choose **+ Add A Device** in the bottom left corner.</span></span>

   ![Lägga till en enhet i den förkonfigurerade lösningen][img-adddevice]

1. <span data-ttu-id="761f5-254">Klicka på **Lägg till ny** på panelen **Simulerad enhet**.</span><span class="sxs-lookup"><span data-stu-id="761f5-254">Choose **Add New** on the **Simulated Device** tile.</span></span>

   ![Ange information om den nya enheten på instrumentpanelen][img-addnew]

   <span data-ttu-id="761f5-256">Förutom att skapa en ny simulerad enhet kan du också lägga till en fysisk enhet om du väljer att skapa en **anpassad enhet**.</span><span class="sxs-lookup"><span data-stu-id="761f5-256">In addition to creating a new simulated device, you can also add a physical device if you choose to create a **Custom Device**.</span></span> <span data-ttu-id="761f5-257">Mer information om hur du ansluter fysiska enheter till lösningen finns i [Ansluta enheten till den förkonfigurerade övervakningslösningen i IoT Suite][lnk-connect-rm].</span><span class="sxs-lookup"><span data-stu-id="761f5-257">To learn more about connecting physical devices to the solution, see [Connect your device to the IoT Suite remote monitoring preconfigured solution][lnk-connect-rm].</span></span>

1. <span data-ttu-id="761f5-258">Välj **Låt mig ange mitt eget enhets-ID** och ange ett unikt enhets-ID, t.ex. **mydevice_01**.</span><span class="sxs-lookup"><span data-stu-id="761f5-258">Select **Let me define my own Device ID**, and enter a unique device ID name such as **mydevice_01**.</span></span>

1. <span data-ttu-id="761f5-259">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="761f5-259">Choose **Create**.</span></span>

   ![Spara den nya enheten][img-definedevice]

1. <span data-ttu-id="761f5-261">I steg 3 under **Lägg till en simulerad enhet** klickar du på **Klar** för att gå tillbaka till enhetslistan.</span><span class="sxs-lookup"><span data-stu-id="761f5-261">In step 3 of **Add a simulated device**, choose **Done** to return to the device list.</span></span>

1. <span data-ttu-id="761f5-262">Du kan se att din enhet **körs** i listan över enheter.</span><span class="sxs-lookup"><span data-stu-id="761f5-262">You can view your device **Running** in the device list.</span></span>

    ![Visa den nya enheten i enhetslistan][img-runningnew]

1. <span data-ttu-id="761f5-264">Du kan också visa den simulerade telemetrin från den nya enheten på instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="761f5-264">You can also view the simulated telemetry from your new device on the dashboard:</span></span>

    ![Visa telemetri från den nya enheten][img-runningnew-2]

### <a name="disable-and-delete-a-device"></a><span data-ttu-id="761f5-266">Inaktivera och ta bort en enhet</span><span class="sxs-lookup"><span data-stu-id="761f5-266">Disable and delete a device</span></span>

<span data-ttu-id="761f5-267">Du kan inaktivera en enhet och när den har inaktiverats kan du ta bort den:</span><span class="sxs-lookup"><span data-stu-id="761f5-267">You can disable a device, and after it is disabled you can remove it:</span></span>

![Inaktivera och ta bort en enhet][img-disable]

### <a name="add-a-rule"></a><span data-ttu-id="761f5-269">Lägg till en regel</span><span class="sxs-lookup"><span data-stu-id="761f5-269">Add a rule</span></span>

<span data-ttu-id="761f5-270">Det finns inga regler för den nya enheten som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="761f5-270">There are no rules for the new device you just added.</span></span> <span data-ttu-id="761f5-271">I det här avsnittet ska du lägga till en regel som utlöser ett larm när temperaturen som rapporteras av den nya enheten överstiger 47 grader.</span><span class="sxs-lookup"><span data-stu-id="761f5-271">In this section, you add a rule that triggers an alarm when the temperature reported by the new device exceeds 47 degrees.</span></span> <span data-ttu-id="761f5-272">Notera innan du börjar att telemetrihistoriken för den nya enheten på instrumentpanelen visar att enhetens temperatur aldrig överstiger 45 grader.</span><span class="sxs-lookup"><span data-stu-id="761f5-272">Before you start, notice that the telemetry history for the new device on the dashboard shows the device temperature never exceeds 45 degrees.</span></span>

1. <span data-ttu-id="761f5-273">Gå tillbaka till enhetslistan.</span><span class="sxs-lookup"><span data-stu-id="761f5-273">Navigate back to the device list.</span></span>

1. <span data-ttu-id="761f5-274">Om du vill lägga till en regel för enheten väljer du den nya enheten i **enhetslistan** och klickar sedan på **Lägg till regel**.</span><span class="sxs-lookup"><span data-stu-id="761f5-274">To add a rule for the device, select your new device in the **Devices List**, and then choose **Add rule**.</span></span>

1. <span data-ttu-id="761f5-275">Skapa en regel som använder **Temperatur** som datafält och **AlarmTemp** som utdata när temperaturen överstiger 47 grader:</span><span class="sxs-lookup"><span data-stu-id="761f5-275">Create a rule that uses **Temperature** as the data field and uses **AlarmTemp** as the output when the temperature exceeds 47 degrees:</span></span>

    ![Lägga till en enhetsregel][img-adddevicerule]

1. <span data-ttu-id="761f5-277">Spara ändringarna genom att klicka på **Spara och visa regler**.</span><span class="sxs-lookup"><span data-stu-id="761f5-277">To save your changes, choose **Save and View Rules**.</span></span>

1. <span data-ttu-id="761f5-278">Klicka på **Kommandon** i rutan med enhetsinformation för den nya enheten.</span><span class="sxs-lookup"><span data-stu-id="761f5-278">Choose **Commands** in the device details pane for the new device.</span></span>

    ![Lägga till en enhetsregel][img-adddevicerule2]

1. <span data-ttu-id="761f5-280">Välj **ChangeSetPointTemp** från kommandolistan och ange **SetPointTemp** till 45.</span><span class="sxs-lookup"><span data-stu-id="761f5-280">Select **ChangeSetPointTemp** from the command list and set **SetPointTemp** to 45.</span></span> <span data-ttu-id="761f5-281">Välj sedan **kommandot Skicka**:</span><span class="sxs-lookup"><span data-stu-id="761f5-281">Then choose **Send Command**:</span></span>

    ![Lägga till en enhetsregel][img-adddevicerule3]

1. <span data-ttu-id="761f5-283">Gå tillbaka till instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="761f5-283">Navigate back to the dashboard.</span></span> <span data-ttu-id="761f5-284">Efter en kort stund visas en ny post i rutan **Larmhistorik** när temperaturen som rapporteras av den nya enheten överstiger tröskelvärdet på 47 grader:</span><span class="sxs-lookup"><span data-stu-id="761f5-284">After a short time, you will see a new entry in the **Alarm History** pane when the temperature reported by your new device exceeds the 47-degree threshold:</span></span>

    ![Lägga till en enhetsregel][img-adddevicerule4]

1. <span data-ttu-id="761f5-286">Du kan granska och redigera alla regler på sidan **Regler** på instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="761f5-286">You can review and edit all your rules on the **Rules** page of the dashboard:</span></span>

    ![Visa en lista över enhetsregler][img-rules]

1. <span data-ttu-id="761f5-288">Du kan granska och redigera alla åtgärder som kan vidtas som svar på en regel på sidan **Åtgärder** på instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="761f5-288">You can review and edit all the actions that can be taken in response to a rule on the **Actions** page of the dashboard:</span></span>

    ![Visa en lista över enhetsåtgärder][img-actions]

> [!NOTE]
> <span data-ttu-id="761f5-290">Du kan definiera åtgärder som kan skicka ett e-postmeddelande eller ett SMS som svar på en regel eller integrera med ett affärssystem via en [logikapp][lnk-logic-apps].</span><span class="sxs-lookup"><span data-stu-id="761f5-290">It is possible to define actions that can send an email message or SMS in response to a rule or integrate with a line-of-business system through a [Logic App][lnk-logic-apps].</span></span> <span data-ttu-id="761f5-291">Mer information finns i [Ansluta logikapp till den förkonfigurerade fjärrövervakningslösningen i Azure IoT Suite][lnk-logicapptutorial].</span><span class="sxs-lookup"><span data-stu-id="761f5-291">For more information, see the [Connect Logic App to your Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logicapptutorial].</span></span>

### <a name="manage-filters"></a><span data-ttu-id="761f5-292">Hantera filter</span><span class="sxs-lookup"><span data-stu-id="761f5-292">Manage filters</span></span>

<span data-ttu-id="761f5-293">I enhetslistan kan du skapa, spara och ladda filter för att visa en anpassad enhetslista som är ansluten till hubben.</span><span class="sxs-lookup"><span data-stu-id="761f5-293">In the device list, you can create, save, and reload filters to display a customized list of devices connected to your hub.</span></span> <span data-ttu-id="761f5-294">Skapa ett filter:</span><span class="sxs-lookup"><span data-stu-id="761f5-294">To create a filter:</span></span>

1. <span data-ttu-id="761f5-295">Klicka på filterredigeringsikonen ovanför listan med enheter:</span><span class="sxs-lookup"><span data-stu-id="761f5-295">Choose the edit filter icon above the list of devices:</span></span>

    ![Öppna filterredigeraren][img-editfiltericon]

1. <span data-ttu-id="761f5-297">Lägg till fält, operatorer och värden för att filtrera enhetslistan i **Filterredigeraren**.</span><span class="sxs-lookup"><span data-stu-id="761f5-297">In the **Filter editor**, add the fields, operators, and values to filter the device list.</span></span> <span data-ttu-id="761f5-298">Du kan lägga till fler villkor för att förfina filtreringen.</span><span class="sxs-lookup"><span data-stu-id="761f5-298">You can add multiple clauses to refine your filter.</span></span> <span data-ttu-id="761f5-299">Klicka på **Filtrera** för att tillämpa filtret:</span><span class="sxs-lookup"><span data-stu-id="761f5-299">Choose **Filter** to apply the filter:</span></span>

    ![Skapa ett filter][img-filtereditor]

1. <span data-ttu-id="761f5-301">I det här exemplet filtreras listan efter tillverkare och modellnummer:</span><span class="sxs-lookup"><span data-stu-id="761f5-301">In this example, the list is filtered by manufacturer and model number:</span></span>

    ![Filtrerad lista][img-filterelist]

1. <span data-ttu-id="761f5-303">Om du vill spara filtret med ett anpassat namn klickar du på ikonen **Spara som**:</span><span class="sxs-lookup"><span data-stu-id="761f5-303">To save your filter with a custom name, choose the **Save as** icon:</span></span>

    ![Spara ett filter][img-savefilter]

1. <span data-ttu-id="761f5-305">Om du vill återanvända ett filter som du har sparat tidigare klickar du på ikonen **Öppna sparat filter**:</span><span class="sxs-lookup"><span data-stu-id="761f5-305">To reapply a filter you saved previously, choose the **Open saved filter** icon:</span></span>

    ![Öppna ett filter][img-openfilter]

<span data-ttu-id="761f5-307">Du kan skapa filter som baseras på enhets-id, enhetstillstånd, önskade egenskaper, rapporterade egenskaper och taggar.</span><span class="sxs-lookup"><span data-stu-id="761f5-307">You can create filters based on device id, device state, desired properties, reported properties, and tags.</span></span> <span data-ttu-id="761f5-308">Du kan lägga till egna anpassade taggar för en enhet i avsnittet **Taggar** på panelen **Enhetsinformation** eller köra ett jobb för att uppdatera taggar på flera enheter.</span><span class="sxs-lookup"><span data-stu-id="761f5-308">You add your own custom tags to a device in the **Tags** section of the **Device Details** panel, or run a job to update tags on multiple devices.</span></span>

> [!NOTE]
> <span data-ttu-id="761f5-309">I **Filterredigeraren** kan du använda **Avancerad vy** för att redigera frågetexten direkt.</span><span class="sxs-lookup"><span data-stu-id="761f5-309">In the **Filter editor**, you can use the **Advanced view** to edit the query text directly.</span></span>

### <a name="commands"></a><span data-ttu-id="761f5-310">Kommandon</span><span class="sxs-lookup"><span data-stu-id="761f5-310">Commands</span></span>

<span data-ttu-id="761f5-311">Du kan skicka kommandon till enheten från panelen **Enhetsinformation**.</span><span class="sxs-lookup"><span data-stu-id="761f5-311">From the **Device Details** panel, you can send commands to the device.</span></span> <span data-ttu-id="761f5-312">Första gången en enhet startar skickar den information om de kommandon som den stöder till lösningen.</span><span class="sxs-lookup"><span data-stu-id="761f5-312">When a device first starts, it sends information about the commands it supports to the solution.</span></span> <span data-ttu-id="761f5-313">En beskrivning av skillnaderna mellan kommandon och metoder finns i [Azure IoT Hub cloud-to-device options][lnk-c2d-guidance] (Alternativ för moln-till-enhet i Azure IoT Hub).</span><span class="sxs-lookup"><span data-stu-id="761f5-313">For a discussion of the differences between commands and methods, see [Azure IoT Hub cloud-to-device options][lnk-c2d-guidance].</span></span>

1. <span data-ttu-id="761f5-314">Klicka på **Kommandon** på panelen **Enhetsinformation** för den valda enheten:</span><span class="sxs-lookup"><span data-stu-id="761f5-314">Choose **Commands** in the **Device Details** panel for the selected device:</span></span>

   ![Enhetskommandon på instrumentpanelen][img-devicecommands]

1. <span data-ttu-id="761f5-316">Välj **PingDevice** i kommandolistan.</span><span class="sxs-lookup"><span data-stu-id="761f5-316">Select **PingDevice** from the command list.</span></span>

1. <span data-ttu-id="761f5-317">Klicka på **Skicka kommando**.</span><span class="sxs-lookup"><span data-stu-id="761f5-317">Choose **Send Command**.</span></span>

1. <span data-ttu-id="761f5-318">Du kan se statusen för kommandot i kommandohistoriken.</span><span class="sxs-lookup"><span data-stu-id="761f5-318">You can see the status of the command in the command history.</span></span>

   ![Kommandostatus på instrumentpanelen][img-pingcommand]

<span data-ttu-id="761f5-320">Lösningen spårar statusen för varje kommando som skickas.</span><span class="sxs-lookup"><span data-stu-id="761f5-320">The solution tracks the status of each command it sends.</span></span> <span data-ttu-id="761f5-321">Resultatet är till en början **Väntande**.</span><span class="sxs-lookup"><span data-stu-id="761f5-321">Initially the result is **Pending**.</span></span> <span data-ttu-id="761f5-322">När enheten rapporterar att den har kört kommandot ändras resultatet till **Lyckades**.</span><span class="sxs-lookup"><span data-stu-id="761f5-322">When the device reports that it has executed the command, the result is set to **Success**.</span></span>

## <a name="behind-the-scenes"></a><span data-ttu-id="761f5-323">I bakgrunden</span><span class="sxs-lookup"><span data-stu-id="761f5-323">Behind the scenes</span></span>

<span data-ttu-id="761f5-324">När du distribuerar en förkonfigurerad lösning skapar distributionsprocessen flera resurser i Azure-prenumerationen som du valt.</span><span class="sxs-lookup"><span data-stu-id="761f5-324">When you deploy a preconfigured solution, the deployment process creates multiple resources in the Azure subscription you selected.</span></span> <span data-ttu-id="761f5-325">Du kan visa dessa resurser på Azure-[portalen][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="761f5-325">You can view these resources in the Azure [portal][lnk-portal].</span></span> <span data-ttu-id="761f5-326">Under distributionsprocessen skapas en **resursgrupp** med ett namn baserat på det namn som du valde för den förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="761f5-326">The deployment process creates a **resource group** with a name based on the name you choose for your preconfigured solution:</span></span>

![Förkonfigurerad lösning på Azure-portalen][img-portal]

<span data-ttu-id="761f5-328">Du kan visa inställningarna för varje resurs genom att välja resursen i listan över resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="761f5-328">You can view the settings of each resource by selecting it in the list of resources in the resource group.</span></span>

<span data-ttu-id="761f5-329">Du kan också visa källkoden för den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="761f5-329">You can also view the source code for the preconfigured solution.</span></span> <span data-ttu-id="761f5-330">Källkoden för den förkonfigurerade fjärrövervakningslösningen finns i [azure-iot-remote-monitoring][lnk-rmgithub]-databasen på GitHub:</span><span class="sxs-lookup"><span data-stu-id="761f5-330">The remote monitoring preconfigured solution source code is in the [azure-iot-remote-monitoring][lnk-rmgithub] GitHub repository:</span></span>

* <span data-ttu-id="761f5-331">Mappen **DeviceAdministration** innehåller källkoden för instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="761f5-331">The **DeviceAdministration** folder contains the source code for the dashboard.</span></span>
* <span data-ttu-id="761f5-332">Mappen **Simulator** innehåller källkoden för den simulerade enheten.</span><span class="sxs-lookup"><span data-stu-id="761f5-332">The **Simulator** folder contains the source code for the simulated device.</span></span>
* <span data-ttu-id="761f5-333">Mappen **EventProcessor** innehåller källkoden för backend-processen som hanterar den inkommande telemetrin.</span><span class="sxs-lookup"><span data-stu-id="761f5-333">The **EventProcessor** folder contains the source code for the back-end process that handles the incoming telemetry.</span></span>

<span data-ttu-id="761f5-334">När du är klar kan du ta bort den förkonfigurerade lösningen från Azure-prenumerationen på webbplatsen [azureiotsuite.com][lnk-azureiotsuite].</span><span class="sxs-lookup"><span data-stu-id="761f5-334">When you are done, you can delete the preconfigured solution from your Azure subscription on the [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="761f5-335">På den här webbplatsen kan du enkelt ta bort alla resurser som etablerades när du skapade den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="761f5-335">This site enables you to easily delete all the resources that were provisioned when you created the preconfigured solution.</span></span>

> [!NOTE]
> <span data-ttu-id="761f5-336">För att vara säker på att du tar bort allt relaterat till den förkonfigurerade lösningen tar du bort den från [azureiotsuite.com][lnk-azureiotsuite] i stället för att ta bort resursgruppen på portalen.</span><span class="sxs-lookup"><span data-stu-id="761f5-336">To ensure that you delete everything related to the preconfigured solution, delete it on the [azureiotsuite.com][lnk-azureiotsuite] site and do not delete the resource group in the portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="761f5-337">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="761f5-337">Next Steps</span></span>

<span data-ttu-id="761f5-338">Nu när du har distribuerat en fungerande förkonfigurerad lösning kan du fortsätta och lära dig mer om IoT Suite genom att läsa följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="761f5-338">Now that you’ve deployed a working preconfigured solution, you can continue getting started with IoT Suite by reading the following articles:</span></span>

* <span data-ttu-id="761f5-339">[Genomgång av den förkonfigurerade lösningen för fjärrövervakning][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="761f5-339">[Remote monitoring preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="761f5-340">[Ansluta enheten till den förkonfigurerade fjärrövervakningslösningen][lnk-connect-rm]</span><span class="sxs-lookup"><span data-stu-id="761f5-340">[Connect your device to the remote monitoring preconfigured solution][lnk-connect-rm]</span></span>
* <span data-ttu-id="761f5-341">[Behörigheter på webbplatsen azureiotsuite.com][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="761f5-341">[Permissions on the azureiotsuite.com site][lnk-permissions]</span></span>

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-menu]: media/iot-suite-getstarted-preconfigured-solutions/menu.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-alarms]: media/iot-suite-getstarted-preconfigured-solutions/alarms.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png
[img-columneditor]: media/iot-suite-getstarted-preconfigured-solutions/columneditor.png
[img-startimageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit1.png
[img-imageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit2.png
[img-editfiltericon]: media/iot-suite-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-getstarted-preconfigured-solutions/openfilter.png
[img-unhealthy-filter]: media/iot-suite-getstarted-preconfigured-solutions/unhealthyfilter.png
[img-filtered-unhealthy-list]: media/iot-suite-getstarted-preconfigured-solutions/unhealthylist.png
[img-change-interval]: media/iot-suite-getstarted-preconfigured-solutions/changeinterval.png
[img-old-firmware]: media/iot-suite-getstarted-preconfigured-solutions/noticeold.png
[img-old-filter]: media/iot-suite-getstarted-preconfigured-solutions/oldfilter.png
[img-filtered-old-list]: media/iot-suite-getstarted-preconfigured-solutions/oldlist.png
[img-method-update]: media/iot-suite-getstarted-preconfigured-solutions/methodupdate.png
[img-update-1]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate1.png
[img-update-2]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate2.png
[img-update-3]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate3.png
[img-updated]: media/iot-suite-getstarted-preconfigured-solutions/updated.png
[img-healthy]: media/iot-suite-getstarted-preconfigured-solutions/healthy.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-faq]: iot-suite-faq.md