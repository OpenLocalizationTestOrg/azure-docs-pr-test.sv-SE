---
title: "aaaGet igång med förkonfigurerade lösningar | Microsoft Docs"
description: "Följ den här självstudiekursen toolearn hur toodeploy ett Azure IoT Suite förkonfigurerade lösningen."
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
ms.openlocfilehash: a7f46023d26b08de2e8ed48c34c5066a43e3fa38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-preconfigured-solutions"></a><span data-ttu-id="7bf4e-103">Kom igång med hello förkonfigurerade lösningar</span><span class="sxs-lookup"><span data-stu-id="7bf4e-103">Get started with hello preconfigured solutions</span></span>

<span data-ttu-id="7bf4e-104">Azure IoT Suite [förkonfigurerade lösningar] [ lnk-preconfigured-solutions] kombinera flera Azure IoT services toodeliver slutpunkt till slutpunkt-lösningar som implementerar vanliga scenarier för IoT-företag.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-104">Azure IoT Suite [preconfigured solutions][lnk-preconfigured-solutions] combine multiple Azure IoT services toodeliver end-to-end solutions that implement common IoT business scenarios.</span></span> <span data-ttu-id="7bf4e-105">Hej *fjärrövervaknings* förkonfigurerade lösningen ansluter tooand övervakar dina enheter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-105">hello *remote monitoring* preconfigured solution connects tooand monitors your devices.</span></span> <span data-ttu-id="7bf4e-106">Du kan använda hello lösning tooanalyze hello dataström från dina enheter och tooimprove affärsresultatet genom att göra processer svara automatiskt toothat dataström.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-106">You can use hello solution tooanalyze hello stream of data from your devices and tooimprove business outcomes by making processes respond automatically toothat stream of data.</span></span>

<span data-ttu-id="7bf4e-107">Den här kursen visar hur tooprovision hello fjärrövervaknings förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-107">This tutorial shows you how tooprovision hello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="7bf4e-108">Även vägleder dig genom hello grundläggande funktioner i hello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-108">It also walks you through hello basic features of hello preconfigured solution.</span></span> <span data-ttu-id="7bf4e-109">Du kan komma åt många av dessa funktioner från hello lösning *instrumentpanelen* som distribuerar som en del av hello förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-109">You can access many of these features from hello solution *dashboard* that deploys as part of hello preconfigured solution:</span></span>

![Instrumentpanel för den förkonfigurerade fjärrövervakningslösningen][img-dashboard]

<span data-ttu-id="7bf4e-111">toocomplete den här självstudiekursen kommer du behöver en aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-111">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="7bf4e-112">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="7bf4e-113">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="7bf4e-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="scenario-overview"></a><span data-ttu-id="7bf4e-114">Scenarioöversikt</span><span class="sxs-lookup"><span data-stu-id="7bf4e-114">Scenario overview</span></span>

<span data-ttu-id="7bf4e-115">När du distribuerar hello remote förkonfigurerade övervakningslösning förinställd den med resurser som gör att toostep via ett vanligt scenario för fjärråtkomst övervakning.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-115">When you deploy hello remote monitoring preconfigured solution, it is prepopulated with resources that enable you toostep through a common remote monitoring scenario.</span></span> <span data-ttu-id="7bf4e-116">I det här scenariot lösning för flera enheter anslutna toohello rapporterar oväntat temperatur värden.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-116">In this scenario, several devices connected toohello solution are reporting unexpected temperature values.</span></span> <span data-ttu-id="7bf4e-117">hello följande avsnitt visar hur till:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-117">hello following sections show you how to:</span></span>

* <span data-ttu-id="7bf4e-118">Identifiera hello enheter skickar oväntat temperatur värden.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-118">Identify hello devices sending unexpected temperature values.</span></span>
* <span data-ttu-id="7bf4e-119">Konfigurera de här enheterna toosend mer detaljerad telemetri.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-119">Configure these devices toosend more detailed telemetry.</span></span>
* <span data-ttu-id="7bf4e-120">Korrigera hello problemet genom att uppdatera hello inbyggd programvara på enheterna.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-120">Fix hello problem by updating hello firmware on these devices.</span></span>
* <span data-ttu-id="7bf4e-121">Kontrollera att åtgärden har löst problemet hello.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-121">Verify that your action has resolved hello issue.</span></span>

<span data-ttu-id="7bf4e-122">En nyckelfunktion i det här scenariot är att du kan utföra dessa åtgärder via fjärranslutning från hello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-122">A key feature of this scenario is that you can perform all these actions remotely from hello solution dashboard.</span></span> <span data-ttu-id="7bf4e-123">Du behöver inte fysisk åtkomst toohello enheter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-123">You do not need physical access toohello devices.</span></span>

## <a name="view-hello-solution-dashboard"></a><span data-ttu-id="7bf4e-124">Visa hello lösning instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="7bf4e-124">View hello solution dashboard</span></span>

<span data-ttu-id="7bf4e-125">hello lösning instrumentpanelen kan du toomanage hello distribueras lösning.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-125">hello solution dashboard enables you toomanage hello deployed solution.</span></span> <span data-ttu-id="7bf4e-126">Du kan till exempel visa telemetri, lägga till enheter och konfigurera regler.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-126">For example, you can view telemetry, add devices, and configure rules.</span></span>

1. <span data-ttu-id="7bf4e-127">När hello etableringen har slutförts och hello panelen för din förkonfigurerade lösning anger **klar**, Välj **starta** tooopen fjärråtkomst övervakning lösning portalen på en ny flik.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-127">When hello provisioning is complete and hello tile for your preconfigured solution indicates **Ready**, choose **Launch** tooopen your remote monitoring solution portal in a new tab.</span></span>

    ![Starta hello förkonfigurerade lösningen][img-launch-solution]

1. <span data-ttu-id="7bf4e-129">Som standard visar hello lösning portal hello *instrumentpanelen*.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-129">By default, hello solution portal shows hello *dashboard*.</span></span> <span data-ttu-id="7bf4e-130">Du kan navigera tooother områden i hello lösning portal med hello menyn hello vänster hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-130">You can navigate tooother areas of hello solution portal using hello menu on hello left-hand side of hello page.</span></span>

    ![Instrumentpanel för den förkonfigurerade fjärrövervakningslösningen][img-menu]

<span data-ttu-id="7bf4e-132">hello instrumentpanelen visar hello följande information:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-132">hello dashboard displays hello following information:</span></span>

* <span data-ttu-id="7bf4e-133">En karta som visar hello platsen för varje enhet ansluten toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-133">A map that displays hello location of each device connected toohello solution.</span></span> <span data-ttu-id="7bf4e-134">När du först kör hello lösning finns 25 simulerade enheter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-134">When you first run hello solution, there are 25 simulated devices.</span></span> <span data-ttu-id="7bf4e-135">hello simulerade enheterna implementeras som Azure WebJobs och hello lösningen använder hello Bing Maps API tooplot information på hello karta.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-135">hello simulated devices are implemented as Azure WebJobs, and hello solution uses hello Bing Maps API tooplot information on hello map.</span></span> <span data-ttu-id="7bf4e-136">Se hello [vanliga frågor och svar] [ lnk-faq] toolearn hur toomake hello kartan dynamiska.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-136">See hello [FAQ][lnk-faq] toolearn how toomake hello map dynamic.</span></span>
* <span data-ttu-id="7bf4e-137">Panelen **Telemetrihistorik** ritar upp fuktighets- och temperaturtelemetri från en vald enhet nästan i realtid och visar aggregerade data, till exempel högsta, lägsta och genomsnittlig fuktighet.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-137">A **Telemetry History** panel that plots humidity and temperature telemetry from a selected device in near real time and displays aggregate data such as maximum, minimum, and average humidity.</span></span>
* <span data-ttu-id="7bf4e-138">Panelen **Larmhistorik** visar de senaste larmhändelserna när ett telemetrivärde har överskridit ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-138">An **Alarm History** panel that shows recent alarm events when a telemetry value has exceeded a threshold.</span></span> <span data-ttu-id="7bf4e-139">Du kan definiera dina egna larm i tillägg toohello exempel som skapats av hello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-139">You can define your own alarms in addition toohello examples created by hello preconfigured solution.</span></span>
* <span data-ttu-id="7bf4e-140">På panelen **Jobb** visas information om schemalagda jobb.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-140">A **Jobs** panel that displays information about scheduled jobs.</span></span> <span data-ttu-id="7bf4e-141">Du kan schemalägga egna jobb på sidan **Hanteringsjobb**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-141">You can schedule your own jobs on **Management jobs** page.</span></span>

## <a name="view-alarms"></a><span data-ttu-id="7bf4e-142">Visa larm</span><span class="sxs-lookup"><span data-stu-id="7bf4e-142">View alarms</span></span>

<span data-ttu-id="7bf4e-143">panelen för hello larm händelser visar att fem enheter rapporterar högre än förväntade telemetri värden.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-143">hello alarm history panel shows you that five devices are reporting higher than expected telemetry values.</span></span>

![TODO larm historik på instrumentpanelen för hello-lösning][img-alarms]

> [!NOTE]
> <span data-ttu-id="7bf4e-145">Dessa larm genereras av en regel som ingår i hello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-145">These alarms are generated by a rule that is included in hello preconfigured solution.</span></span> <span data-ttu-id="7bf4e-146">Den här regeln genererar en avisering när hello temperatur värdet som skickas av en enhet överstiger 60.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-146">This rule generates an alert when hello temperature value sent by a device exceeds 60.</span></span> <span data-ttu-id="7bf4e-147">Du kan definiera dina egna regler och åtgärder genom att välja [regler](#add-a-rule) och [åtgärder](#add-an-action) hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-147">You can define your own rules and actions by choosing [Rules](#add-a-rule) and [Actions](#add-an-action) in hello left-hand menu.</span></span>

## <a name="view-devices"></a><span data-ttu-id="7bf4e-148">Visa enheter</span><span class="sxs-lookup"><span data-stu-id="7bf4e-148">View devices</span></span>

<span data-ttu-id="7bf4e-149">Hej *enheter* listan visas alla hello registrerade enheter i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-149">hello *devices* list shows all hello registered devices in hello solution.</span></span> <span data-ttu-id="7bf4e-150">Lägg till eller ta bort enheter och anropa metoder i enheter hello enheten listan som du kan visa och redigera enhetens metadata.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-150">From hello device list you can view and edit device metadata, add or remove devices, and invoke methods on devices.</span></span> <span data-ttu-id="7bf4e-151">Du kan filtrera och sortera hello lista över enheter i listan över hello-enheter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-151">You can filter and sort hello list of devices in hello device list.</span></span> <span data-ttu-id="7bf4e-152">Du kan också anpassa hello kolumner som visas i listan över hello-enheter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-152">You can also customize hello columns shown in hello device list.</span></span>

1. <span data-ttu-id="7bf4e-153">Välj **enheter** tooshow hello listan över enheter för den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-153">Choose **Devices** tooshow hello device list for this solution.</span></span>

   ![Visa hello listan över enheter i hello lösning portal][img-devicelist]

1. <span data-ttu-id="7bf4e-155">hello enheten i listan visas först 25 simulerade enheter som skapats av hello etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-155">hello device list initially shows 25 simulated devices created by hello provisioning process.</span></span> <span data-ttu-id="7bf4e-156">Du kan lägga till ytterligare simulerade och fysiska enheter toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-156">You can add additional simulated and physical devices toohello solution.</span></span>

1. <span data-ttu-id="7bf4e-157">tooview hello information om en enhet, Välj en enhet i listan över hello-enheter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-157">tooview hello details of a device, choose a device in hello device list.</span></span>

   ![Visa information om hello enhet i hello lösning portal][img-devicedetails]

<span data-ttu-id="7bf4e-159">Hej **enhetsinformation** panelen innehåller sex avsnitt:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-159">hello **Device Details** panel contains six sections:</span></span>

* <span data-ttu-id="7bf4e-160">En samling länkar som aktiverar du toocustomize hello ikon, inaktivera hello enheten, lägga till en regel, anropa en metod eller skicka ett kommando.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-160">A collection of links that enable you toocustomize hello device icon, disable hello device, add a rule, invoke a method, or send a command.</span></span> <span data-ttu-id="7bf4e-161">En jämförelse av kommandon (meddelanden från enheten till molnet) och metoder (direkta metoder) finns i [Cloud-to-device communications guidance][lnk-c2d-guidance] (Vägledning för kommunikation från moln till enhet).</span><span class="sxs-lookup"><span data-stu-id="7bf4e-161">For a comparison of commands (device-to-cloud messages) and methods (direct methods), see [Cloud-to-device communications guidance][lnk-c2d-guidance].</span></span>
* <span data-ttu-id="7bf4e-162">Hej **enheten dubbla - taggar** avsnittet kan du tooedit taggvärden för hello enhet.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-162">hello **Device Twin - Tags** section enables you tooedit tag values for hello device.</span></span> <span data-ttu-id="7bf4e-163">Du kan visa värden i listan över enheter hello och använda taggen värden toofilter hello listan över enheter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-163">You can display tag values in hello device list and use tag values toofilter hello device list.</span></span>
* <span data-ttu-id="7bf4e-164">Hej **enheten dubbla - önskade egenskaper** avsnittet kan du tooset egenskapen värden toobe skickas toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-164">hello **Device Twin - Desired Properties** section enables you tooset property values toobe sent toohello device.</span></span>
* <span data-ttu-id="7bf4e-165">Hej **enheten dubbla - rapporterade egenskaper** avsnittet visar egenskapsvärden som skickas från hello enheten.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-165">hello **Device Twin - Reported Properties** section shows property values sent from hello device.</span></span>
* <span data-ttu-id="7bf4e-166">Hej **enhetsegenskaper** avsnittet visar information från hello identitetsregistret till exempel hello enhet id- och autentiseringsnycklarna.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-166">hello **Device Properties** section shows information from hello identity registry such as hello device id and authentication keys.</span></span>
* <span data-ttu-id="7bf4e-167">Hej **senaste jobb** avsnittet visar information om alla jobb som har nyligen mål för den här enheten.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-167">hello **Recent Jobs** section shows information about any jobs that have recently targeted this device.</span></span>

## <a name="filter-hello-device-list"></a><span data-ttu-id="7bf4e-168">Filtrera listan över hello-enheter</span><span class="sxs-lookup"><span data-stu-id="7bf4e-168">Filter hello device list</span></span>

<span data-ttu-id="7bf4e-169">Du kan använda ett filter toodisplay endast de enheter som skickar oväntat temperatur värden.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-169">You can use a filter toodisplay only those devices that are sending unexpected temperature values.</span></span> <span data-ttu-id="7bf4e-170">hello fjärråtkomst övervakning förkonfigurerade lösningen innehåller hello **ohälsosamt enheter** filtrera tooshow enheter med ett medelvärde temperatur-värde som är större än 60.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-170">hello remote monitoring preconfigured solution includes hello **Unhealthy devices** filter tooshow devices with a mean temperature value greater than 60.</span></span> <span data-ttu-id="7bf4e-171">Du kan även [skapa egna filter](#add-a-filter).</span><span class="sxs-lookup"><span data-stu-id="7bf4e-171">You can also [create your own filters](#add-a-filter).</span></span>

1. <span data-ttu-id="7bf4e-172">Välj **öppna sparade filter** toodisplay en lista över tillgängliga filtren.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-172">Choose **Open saved filter** toodisplay a list of available filters.</span></span> <span data-ttu-id="7bf4e-173">Välj **ohälsosamt enheter** tooapply hello filter:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-173">Then choose **Unhealthy devices** tooapply hello filter:</span></span>

    ![Visa hello listan över filter][img-unhealthy-filter]

1. <span data-ttu-id="7bf4e-175">listan över hello-enheter visas nu endast enheter med ett medelvärde temperatur värde större än 60.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-175">hello device list now shows only devices with a mean temperature value greater than 60.</span></span>

    ![Visa hello filtreras listan över enheter visar ohälsosamt][img-filtered-unhealthy-list]

## <a name="update-desired-properties"></a><span data-ttu-id="7bf4e-177">Uppdatera önskade egenskaper</span><span class="sxs-lookup"><span data-stu-id="7bf4e-177">Update desired properties</span></span>

<span data-ttu-id="7bf4e-178">Nu har du identifierat en uppsättning enheter som kan behöva repareras.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-178">You have now identified a set of devices that may need remediation.</span></span> <span data-ttu-id="7bf4e-179">Men du bestämmer dig för hello data frekvens på 15 sekunder inte är tillräcklig för en tydlig diagnos av hello problemet.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-179">However, you decide that hello data frequency of 15 seconds is not sufficient for a clear diagnosis of hello issue.</span></span> <span data-ttu-id="7bf4e-180">Ändra hello telemetri frekvens toofive sekunder tooprovide diagnostisera du med mer datapunkter toobetter hello problemet.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-180">Changing hello telemetry frequency toofive seconds tooprovide you with more data points toobetter diagnose hello issue.</span></span> <span data-ttu-id="7bf4e-181">Du kan skicka den här konfigurationen ändras tooyour fjärranslutna enheter från hello lösning portal.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-181">You can push this configuration change tooyour remote devices from hello solution portal.</span></span> <span data-ttu-id="7bf4e-182">Du kan göra hello ändra en gång, utvärdera hello inverkan och sedan vidta åtgärder för hello resultat.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-182">You can make hello change once, evaluate hello impact, and then act on hello results.</span></span>

<span data-ttu-id="7bf4e-183">Följ dessa steg toorun ett jobb som ändrar hello **TelemetryInterval** önskad egenskap för hello berörda enheter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-183">Follow these steps toorun a job that changes hello **TelemetryInterval** desired property for hello affected devices.</span></span> <span data-ttu-id="7bf4e-184">När hello enheter får nya hello **TelemetryInterval** egenskapsvärde, de ändra sin konfiguration toosend telemetri varje fem sekunder i stället för var 15: e sekund:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-184">When hello devices receive hello new **TelemetryInterval** property value, they change their configuration toosend telemetry every five seconds instead of every 15 seconds:</span></span>

1. <span data-ttu-id="7bf4e-185">När du visar hello lista över enheter som ohälsosamt i listan över enheter hello väljer **jobbschemat**, sedan **redigera enheten dubbla**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-185">While you are showing hello list of unhealthy devices in hello device list, choose **Job Scheduler**, then **Edit Device Twin**.</span></span>

1. <span data-ttu-id="7bf4e-186">Anropa hello jobbet **intervall mellan lösenordsbyte telemetri**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-186">Call hello job **Change telemetry interval**.</span></span>

1. <span data-ttu-id="7bf4e-187">Ändra hello värdet på hello **önskade egenskapen** namn **önskade. Config.TelemetryInterval** toofive sekunder.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-187">Change hello value of hello **Desired Property** name **desired.Config.TelemetryInterval** toofive seconds.</span></span>

1. <span data-ttu-id="7bf4e-188">Välj **Schema**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-188">Choose **Schedule**.</span></span>

    ![Ändra hello TelemetryInterval egenskapen toofive sekunder][img-change-interval]

1. <span data-ttu-id="7bf4e-190">Du kan övervaka hello hello jobb på hello **Management jobb** sidan hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-190">You can monitor hello progress of hello job on hello **Management Jobs** page in hello portal.</span></span>

> [!NOTE]
> <span data-ttu-id="7bf4e-191">Om du vill toochange önskade egenskapsvärde för en enskild enhet använder hello **egenskaper** avsnitt i hello **enhetsinformation** panelen i stället för att köra ett jobb.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-191">If you want toochange a desired property value for an individual device, use hello **Desired Properties** section in hello **Device Details** panel instead of running a job.</span></span>

<span data-ttu-id="7bf4e-192">Det här jobbet anger hello-värdet för hello **TelemetryInterval** önskad egenskap i hello enheten identiska för alla hello enheter som valts av hello filter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-192">This job sets hello value of hello **TelemetryInterval** desired property in hello device twin for all hello devices selected by hello filter.</span></span> <span data-ttu-id="7bf4e-193">hello enheter hämtar värdet från hello enheten dubbla och uppdatera deras beteende.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-193">hello devices retrieve this value from hello device twin and update their behavior.</span></span> <span data-ttu-id="7bf4e-194">När en enhet hämtar och bearbetar en önskad egenskap från en enhet dubbla, anger hello motsvarande rapporterade value-egenskap.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-194">When a device retrieves and processes a desired property from a device twin, it sets hello corresponding reported value property.</span></span>

## <a name="invoke-methods"></a><span data-ttu-id="7bf4e-195">Anropningsmetoder</span><span class="sxs-lookup"><span data-stu-id="7bf4e-195">Invoke methods</span></span>

<span data-ttu-id="7bf4e-196">När hello jobbet körs du upptäcker i hello lista över enheter som ohälsosamt att alla dessa enheter har gamla (mindre än version 1.6) programvaran versioner.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-196">While hello job runs, you notice in hello list of unhealthy devices that all these devices have old (less than version 1.6) firmware versions.</span></span>

![Visa hello rapporterade version på inbyggd programvara för hello ohälsosamt enheter][img-old-firmware]

<span data-ttu-id="7bf4e-198">Den här versionen av inbyggd programvara kan vara hello grundorsaken till hello oväntat temperatur värden eftersom du vet att andra felfri enheter har nyligen uppdaterat tooversion 2.0.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-198">This firmware version may be hello root cause of hello unexpected temperature values because you know that other healthy devices were recently updated tooversion 2.0.</span></span> <span data-ttu-id="7bf4e-199">Du kan använda inbyggda hello **gamla firmware enheter** filtrera tooidentify alla enheter med äldre versioner av inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-199">You can use hello built-in **Old firmware devices** filter tooidentify any devices with old firmware versions.</span></span> <span data-ttu-id="7bf4e-200">Från hello-portalen kan du via fjärranslutning uppdatera alla hello-enheter som fortfarande kör äldre versioner av inbyggd programvara:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-200">From hello portal, you can then remotely update all hello devices still running old firmware versions:</span></span>

1. <span data-ttu-id="7bf4e-201">Välj **öppna sparade filter** toodisplay en lista över tillgängliga filtren.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-201">Choose **Open saved filter** toodisplay a list of available filters.</span></span> <span data-ttu-id="7bf4e-202">Välj **gamla firmware enheter** tooapply hello filter:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-202">Then choose **Old firmware devices** tooapply hello filter:</span></span>

    ![Visa hello listan över filter][img-old-filter]

1. <span data-ttu-id="7bf4e-204">listan över hello-enheter visas nu endast enheter med äldre versioner av inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-204">hello device list now shows only devices with old firmware versions.</span></span> <span data-ttu-id="7bf4e-205">Den här listan innehåller hello fem enheter som identifieras av hello **ohälsosamt enheter** filter och tre ytterligare enheter:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-205">This list includes hello five devices identified by hello **Unhealthy devices** filter and three additional devices:</span></span>

    ![Visa hello filtreras listan över enheter visar gamla enheter][img-filtered-old-list]

1. <span data-ttu-id="7bf4e-207">Välj **Jobbschema**, sedan **Anropa metod**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-207">Choose **Job Scheduler**, then **Invoke Method**.</span></span>

1. <span data-ttu-id="7bf4e-208">Ange **jobbnamn** för**Firmware-uppdatering tooversion 2.0**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-208">Set **Job Name** too**Firmware update tooversion 2.0**.</span></span>

1. <span data-ttu-id="7bf4e-209">Välj **InitiateFirmwareUpdate** som hello **metoden**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-209">Choose **InitiateFirmwareUpdate** as hello **Method**.</span></span>

1. <span data-ttu-id="7bf4e-210">Ange hello **FwPackageUri** parameter för**https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-210">Set hello **FwPackageUri** parameter too**https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin**.</span></span>

1. <span data-ttu-id="7bf4e-211">Välj **Schema**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-211">Choose **Schedule**.</span></span> <span data-ttu-id="7bf4e-212">hello standardvärdet är nu för hello jobbet toorun.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-212">hello default is for hello job toorun now.</span></span>

    ![Skapa jobbet tooupdate hello inbyggda hello markerade enheter][img-method-update]

> [!NOTE]
> <span data-ttu-id="7bf4e-214">Om du vill tooinvoke en metod i en enskild enhet väljer **metoder** i hello **enhetsinformation** panelen i stället för att köra ett jobb.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-214">If you want tooinvoke a method on an individual device, choose **Methods** in hello **Device Details** panel instead of running a job.</span></span>

<span data-ttu-id="7bf4e-215">Det här jobbet anropar hello **InitiateFirmwareUpdate** direkt metod på alla hello-enheter som valts av hello filter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-215">This job invokes hello **InitiateFirmwareUpdate** direct method on all hello devices selected by hello filter.</span></span> <span data-ttu-id="7bf4e-216">Enheter svara direkt tooIoT hubb och startar sedan hello firmware uppdateringsprocessen asynkront.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-216">Devices respond immediately tooIoT Hub and then initiate hello firmware update process asynchronously.</span></span> <span data-ttu-id="7bf4e-217">hello enheter visar statusinformation om hello firmware uppdateringsprocessen via rapporterade egenskapsvärden som visas i följande skärmdumpar hello.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-217">hello devices provide status information about hello firmware update process through reported property values, as shown in hello following screenshots.</span></span> <span data-ttu-id="7bf4e-218">Välj hello **uppdatera** ikonen tooupdate hello information enheten och jobbet i hello:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-218">Choose hello **Refresh** icon tooupdate hello information in hello device and job lists:</span></span>

<span data-ttu-id="7bf4e-219">![Jobblistan som visar hello firmware uppdatera listan körs][img-update-1]
![listan över enheter visar status för uppdateringar av inbyggd programvara][img-update-2]
![lista visar hello firmware update jobblistan klar][img-update-3]</span><span class="sxs-lookup"><span data-stu-id="7bf4e-219">![Job list showing hello firmware update list running][img-update-1]
![Device list showing firmware update status][img-update-2]
![Job list showing hello firmware update list complete][img-update-3]</span></span>

> [!NOTE]
> <span data-ttu-id="7bf4e-220">I en produktionsmiljö kan du schemalägga jobb toorun under en angiven underhållsperiod.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-220">In a production environment, you can schedule jobs toorun during a designated maintenance window.</span></span>

## <a name="scenario-review"></a><span data-ttu-id="7bf4e-221">Scenariogranskning</span><span class="sxs-lookup"><span data-stu-id="7bf4e-221">Scenario review</span></span>

<span data-ttu-id="7bf4e-222">I det här scenariot identifiera potentiella problem med en del av din fjärranslutna enheter med hello larm historik på hello instrumentpanelen och ett filter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-222">In this scenario, you identified a potential issue with some of your remote devices using hello alarm history on hello dashboard and a filter.</span></span> <span data-ttu-id="7bf4e-223">Konfigurera hello enheter tooprovide du använt hello filter och sedan ett jobb tooremotely mer information toohelp diagnostisera hello problemet.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-223">You then used hello filter and a job tooremotely configure hello devices tooprovide more information toohelp diagnose hello issue.</span></span> <span data-ttu-id="7bf4e-224">Slutligen använde du ett filter och ett jobb tooschedule Underhåll på hello berörda enheter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-224">Finally, you used a filter and a job tooschedule maintenance on hello affected devices.</span></span> <span data-ttu-id="7bf4e-225">Om du återgår toohello instrumentpanelen kan kontrollera du att det inte längre några larm som kommer från enheter i din lösning.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-225">If you return toohello dashboard, you can check that there are no longer any alarms coming from devices in your solution.</span></span> <span data-ttu-id="7bf4e-226">Du kan använda ett filter tooverify som hello inbyggd programvara är uppdaterad på alla hello enheter i din lösning och att det finns inga fler ohälsosamt enheter:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-226">You can use a filter tooverify that hello firmware is up-to-date on all hello devices in your solution and that there are no more unhealthy devices:</span></span>

![Filter som visar att alla enheter som har uppdaterad inbyggd programvara][img-updated]

![Filter som visar att alla enheter är felfria][img-healthy]

## <a name="other-features"></a><span data-ttu-id="7bf4e-229">Andra funktioner</span><span class="sxs-lookup"><span data-stu-id="7bf4e-229">Other features</span></span>

<span data-ttu-id="7bf4e-230">hello beskrivs följande avsnitt några ytterligare funktioner hello remote förkonfigurerade övervakningslösning som inte beskrivs som en del av hello föregående scenario.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-230">hello following sections describe some additional features of hello remote monitoring preconfigured solution that are not described as part of hello previous scenario.</span></span>

### <a name="customize-columns"></a><span data-ttu-id="7bf4e-231">Anpassa kolumner</span><span class="sxs-lookup"><span data-stu-id="7bf4e-231">Customize columns</span></span>

<span data-ttu-id="7bf4e-232">Du kan anpassa hello informationen som visas i listan över hello-enheter genom att välja **kolumnen editor**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-232">You can customize hello information shown in hello device list by choosing **Column editor**.</span></span> <span data-ttu-id="7bf4e-233">Du kan lägga till och ta bort kolumner som visar rapporterade egenskap- och taggvärden.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-233">You can add and remove columns that display reported property and tag values.</span></span> <span data-ttu-id="7bf4e-234">Du kan även ändra ordningen och byta namn på kolumner:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-234">You can also reorder and rename columns:</span></span>

   ![Kolumnen redigeraren med hello listan över enheter][img-columneditor]

### <a name="customize-hello-device-icon"></a><span data-ttu-id="7bf4e-236">Anpassa hello ikon</span><span class="sxs-lookup"><span data-stu-id="7bf4e-236">Customize hello device icon</span></span>

<span data-ttu-id="7bf4e-237">Du kan anpassa hello ikon visas i listan över enheter hello från hello **enhetsinformation** panelen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-237">You can customize hello device icon displayed in hello device list from hello **Device Details** panel as follows:</span></span>

1. <span data-ttu-id="7bf4e-238">Välj hello penna ikonen tooopen hello **Redigera bild** för en enhet:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-238">Choose hello pencil icon tooopen hello **Edit image** panel for a device:</span></span>

   ![Öppna redigeringsprogrammet för enhetsavbildningar][img-startimageedit]

1. <span data-ttu-id="7bf4e-240">Ladda upp en ny avbildning eller använda en befintlig hello-avbildningar och väljer sedan **spara**:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-240">Either upload a new image or use one of hello existing images and then choose **Save**:</span></span>

   ![Redigera redigeringsprogrammet för enhetsavbildningar][img-imageedit]

1. <span data-ttu-id="7bf4e-242">hello valda nu visas i hello **ikonen** kolumnen för hello enhet.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-242">hello image you selected now displays in hello **Icon** column for hello device.</span></span>

> [!NOTE]
> <span data-ttu-id="7bf4e-243">hello avbildningen lagras i blob storage.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-243">hello image is stored in blob storage.</span></span> <span data-ttu-id="7bf4e-244">En tagg i hello enheten dubbla innehåller en länk toohello avbildning i blob storage.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-244">A tag in hello device twin contains a link toohello image in blob storage.</span></span>

### <a name="add-a-device"></a><span data-ttu-id="7bf4e-245">Lägg till en enhet</span><span class="sxs-lookup"><span data-stu-id="7bf4e-245">Add a device</span></span>

<span data-ttu-id="7bf4e-246">När du distribuerar hello förkonfigurerade lösningen kan etablera du automatiskt 25 exempel enheter som du kan se i listan över hello-enheter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-246">When you deploy hello preconfigured solution, you automatically provision 25 sample devices that you can see in hello device list.</span></span> <span data-ttu-id="7bf4e-247">Dessa enheter är *simulerade enheter* som körs i ett Azure-webbjobb.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-247">These devices are *simulated devices* running in an Azure WebJob.</span></span> <span data-ttu-id="7bf4e-248">Simulerade enheter gör det enkelt för dig tooexperiment med hello förkonfigurerade lösningen utan hello måste toodeploy verkliga fysiska enheter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-248">Simulated devices make it easy for you tooexperiment with hello preconfigured solution without hello need toodeploy real, physical devices.</span></span> <span data-ttu-id="7bf4e-249">Om du vill tooconnect en lösning för verklig enheter toohello, se hello [ansluta din enhet toohello remote förkonfigurerade övervakningslösning] [ lnk-connect-rm] kursen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-249">If you do want tooconnect a real device toohello solution, see hello [Connect your device toohello remote monitoring preconfigured solution][lnk-connect-rm] tutorial.</span></span>

<span data-ttu-id="7bf4e-250">hello följande steg visar hur tooadd en simulerad enhet toohello lösning:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-250">hello following steps show you how tooadd a simulated device toohello solution:</span></span>

1. <span data-ttu-id="7bf4e-251">Gå tillbaka toohello enhetslistan.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-251">Navigate back toohello device list.</span></span>

1. <span data-ttu-id="7bf4e-252">tooadd en enhet, Välj **+ Lägg till en enhet** i hello nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-252">tooadd a device, choose **+ Add A Device** in hello bottom left corner.</span></span>

   ![Lägg till en enhet toohello förkonfigurerade lösning][img-adddevice]

1. <span data-ttu-id="7bf4e-254">Välj **Lägg till ny** på hello **simulerade enheten** panelen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-254">Choose **Add New** on hello **Simulated Device** tile.</span></span>

   ![Ange information om den nya enheten på instrumentpanelen][img-addnew]

   <span data-ttu-id="7bf4e-256">Tillägg toocreating en ny simulerade enhet, du kan också lägga till en fysisk enhet om du väljer toocreate en **anpassad enhet**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-256">In addition toocreating a new simulated device, you can also add a physical device if you choose toocreate a **Custom Device**.</span></span> <span data-ttu-id="7bf4e-257">toolearn mer information om hur du ansluter fysiska enheter toohello lösning finns [ansluta din enhet toohello IoT Suite remote förkonfigurerade övervakningslösning][lnk-connect-rm].</span><span class="sxs-lookup"><span data-stu-id="7bf4e-257">toolearn more about connecting physical devices toohello solution, see [Connect your device toohello IoT Suite remote monitoring preconfigured solution][lnk-connect-rm].</span></span>

1. <span data-ttu-id="7bf4e-258">Välj **Låt mig ange mitt eget enhets-ID** och ange ett unikt enhets-ID, t.ex. **mydevice_01**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-258">Select **Let me define my own Device ID**, and enter a unique device ID name such as **mydevice_01**.</span></span>

1. <span data-ttu-id="7bf4e-259">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-259">Choose **Create**.</span></span>

   ![Spara den nya enheten][img-definedevice]

1. <span data-ttu-id="7bf4e-261">I steg 3 i **lägger till en simulerad enhet**, Välj **klar** tooreturn toohello enhetslistan.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-261">In step 3 of **Add a simulated device**, choose **Done** tooreturn toohello device list.</span></span>

1. <span data-ttu-id="7bf4e-262">Du kan visa din enhet **kör** i listan över hello-enheter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-262">You can view your device **Running** in hello device list.</span></span>

    ![Visa den nya enheten i enhetslistan][img-runningnew]

1. <span data-ttu-id="7bf4e-264">Du kan också visa hello simulerade telemetri från den nya enheten på hello instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-264">You can also view hello simulated telemetry from your new device on hello dashboard:</span></span>

    ![Visa telemetri från den nya enheten][img-runningnew-2]

### <a name="disable-and-delete-a-device"></a><span data-ttu-id="7bf4e-266">Inaktivera och ta bort en enhet</span><span class="sxs-lookup"><span data-stu-id="7bf4e-266">Disable and delete a device</span></span>

<span data-ttu-id="7bf4e-267">Du kan inaktivera en enhet och när den har inaktiverats kan du ta bort den:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-267">You can disable a device, and after it is disabled you can remove it:</span></span>

![Inaktivera och ta bort en enhet][img-disable]

### <a name="add-a-rule"></a><span data-ttu-id="7bf4e-269">Lägg till en regel</span><span class="sxs-lookup"><span data-stu-id="7bf4e-269">Add a rule</span></span>

<span data-ttu-id="7bf4e-270">Det finns inga regler för hello ny enhet som du just lagt till.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-270">There are no rules for hello new device you just added.</span></span> <span data-ttu-id="7bf4e-271">I det här avsnittet kan du lägga till en regel som utlöser ett larm när hello temperatur som rapporterats av hello ny enhet överskrider 47 grader.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-271">In this section, you add a rule that triggers an alarm when hello temperature reported by hello new device exceeds 47 degrees.</span></span> <span data-ttu-id="7bf4e-272">Innan du börjar, Lägg märke till att hello telemetri historik för hello ny enhet på hello instrumentpanelen visar hello enheten temperatur aldrig överskrider 45 grader.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-272">Before you start, notice that hello telemetry history for hello new device on hello dashboard shows hello device temperature never exceeds 45 degrees.</span></span>

1. <span data-ttu-id="7bf4e-273">Gå tillbaka toohello enhetslistan.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-273">Navigate back toohello device list.</span></span>

1. <span data-ttu-id="7bf4e-274">tooadd en regel för hello enheten, Välj den nya enheten i hello **enhetslistan**, och välj sedan **Lägg till regel**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-274">tooadd a rule for hello device, select your new device in hello **Devices List**, and then choose **Add rule**.</span></span>

1. <span data-ttu-id="7bf4e-275">Skapa en regel som använder **temperatur** som hello datafält och använder **AlarmTemp** som hello utdata när hello temperatur överskrider 47 grader:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-275">Create a rule that uses **Temperature** as hello data field and uses **AlarmTemp** as hello output when hello temperature exceeds 47 degrees:</span></span>

    ![Lägga till en enhetsregel][img-adddevicerule]

1. <span data-ttu-id="7bf4e-277">toosave ändringarna, Välj **spara och visa regler**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-277">toosave your changes, choose **Save and View Rules**.</span></span>

1. <span data-ttu-id="7bf4e-278">Välj **kommandon** i hello informationspenelen för hello ny enhet.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-278">Choose **Commands** in hello device details pane for hello new device.</span></span>

    ![Lägga till en enhetsregel][img-adddevicerule2]

1. <span data-ttu-id="7bf4e-280">Välj **ChangeSetPointTemp** hello Kommandolistan och ange **SetPointTemp** too45.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-280">Select **ChangeSetPointTemp** from hello command list and set **SetPointTemp** too45.</span></span> <span data-ttu-id="7bf4e-281">Välj sedan **kommandot Skicka**:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-281">Then choose **Send Command**:</span></span>

    ![Lägga till en enhetsregel][img-adddevicerule3]

1. <span data-ttu-id="7bf4e-283">Gå tillbaka toohello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-283">Navigate back toohello dashboard.</span></span> <span data-ttu-id="7bf4e-284">Efter en kort tid ser en ny post i hello **larm historik** när hello temperatur som rapporterats av den nya enheten fönster överskrider hello 47 graders tröskeln:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-284">After a short time, you will see a new entry in hello **Alarm History** pane when hello temperature reported by your new device exceeds hello 47-degree threshold:</span></span>

    ![Lägga till en enhetsregel][img-adddevicerule4]

1. <span data-ttu-id="7bf4e-286">Du kan granska och redigera alla regler på hello **regler** sidan hello instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-286">You can review and edit all your rules on hello **Rules** page of hello dashboard:</span></span>

    ![Visa en lista över enhetsregler][img-rules]

1. <span data-ttu-id="7bf4e-288">Du kan granska och redigera alla hello-åtgärder som kan vidtas i svaret tooa regel på hello **åtgärder** sidan hello instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-288">You can review and edit all hello actions that can be taken in response tooa rule on hello **Actions** page of hello dashboard:</span></span>

    ![Visa en lista över enhetsåtgärder][img-actions]

> [!NOTE]
> <span data-ttu-id="7bf4e-290">Det är möjligt toodefine åtgärder som kan skicka ett e-postmeddelande eller SMS i svaret tooa regel eller integrera med en line-of-business-systemet via en [Logikapp][lnk-logic-apps].</span><span class="sxs-lookup"><span data-stu-id="7bf4e-290">It is possible toodefine actions that can send an email message or SMS in response tooa rule or integrate with a line-of-business system through a [Logic App][lnk-logic-apps].</span></span> <span data-ttu-id="7bf4e-291">Mer information finns i hello [ansluta Logikapp tooyour Azure IoT Suite Fjärrövervaknings förkonfigurerade lösningen][lnk-logicapptutorial].</span><span class="sxs-lookup"><span data-stu-id="7bf4e-291">For more information, see hello [Connect Logic App tooyour Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logicapptutorial].</span></span>

### <a name="manage-filters"></a><span data-ttu-id="7bf4e-292">Hantera filter</span><span class="sxs-lookup"><span data-stu-id="7bf4e-292">Manage filters</span></span>

<span data-ttu-id="7bf4e-293">I listan över enheter hello kan du skapa, spara och Läs in filter toodisplay en egen lista över enheter anslutna tooyour hubb.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-293">In hello device list, you can create, save, and reload filters toodisplay a customized list of devices connected tooyour hub.</span></span> <span data-ttu-id="7bf4e-294">toocreate ett filter:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-294">toocreate a filter:</span></span>

1. <span data-ttu-id="7bf4e-295">Välj hello redigera filterikonen ovan hello lista över enheter:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-295">Choose hello edit filter icon above hello list of devices:</span></span>

    ![Öppna hello filter redigeraren][img-editfiltericon]

1. <span data-ttu-id="7bf4e-297">I hello **Filter editor**, lägga till hello fält, operatorer och värden toofilter hello enhetslistan.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-297">In hello **Filter editor**, add hello fields, operators, and values toofilter hello device list.</span></span> <span data-ttu-id="7bf4e-298">Du kan lägga till flera satser toorefine filtret.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-298">You can add multiple clauses toorefine your filter.</span></span> <span data-ttu-id="7bf4e-299">Välj **Filter** tooapply hello filter:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-299">Choose **Filter** tooapply hello filter:</span></span>

    ![Skapa ett filter][img-filtereditor]

1. <span data-ttu-id="7bf4e-301">I det här exemplet filtreras hello listan efter tillverkare och modell tal:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-301">In this example, hello list is filtered by manufacturer and model number:</span></span>

    ![Filtrerad lista][img-filterelist]

1. <span data-ttu-id="7bf4e-303">toosave filtret med ett anpassat namn väljer hello **Spara som** ikon:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-303">toosave your filter with a custom name, choose hello **Save as** icon:</span></span>

    ![Spara ett filter][img-savefilter]

1. <span data-ttu-id="7bf4e-305">tooreapply ett filter som du sparade tidigare, Välj hello **öppna sparade filter** ikon:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-305">tooreapply a filter you saved previously, choose hello **Open saved filter** icon:</span></span>

    ![Öppna ett filter][img-openfilter]

<span data-ttu-id="7bf4e-307">Du kan skapa filter som baseras på enhets-id, enhetstillstånd, önskade egenskaper, rapporterade egenskaper och taggar.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-307">You can create filters based on device id, device state, desired properties, reported properties, and tags.</span></span> <span data-ttu-id="7bf4e-308">Du lägger till dina egna anpassade taggar tooa enhet i hello **taggar** avsnitt i hello **enhetsinformation** Kontrollpanelen eller köra ett jobb tooupdate taggar på flera enheter.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-308">You add your own custom tags tooa device in hello **Tags** section of hello **Device Details** panel, or run a job tooupdate tags on multiple devices.</span></span>

> [!NOTE]
> <span data-ttu-id="7bf4e-309">I hello **Filter editor**, du kan använda hello **Avancerat läge** tooedit hello frågetexten direkt.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-309">In hello **Filter editor**, you can use hello **Advanced view** tooedit hello query text directly.</span></span>

### <a name="commands"></a><span data-ttu-id="7bf4e-310">Kommandon</span><span class="sxs-lookup"><span data-stu-id="7bf4e-310">Commands</span></span>

<span data-ttu-id="7bf4e-311">Från hello **enhetsinformation** Kontrollpanelen, kan du skicka kommandon toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-311">From hello **Device Details** panel, you can send commands toohello device.</span></span> <span data-ttu-id="7bf4e-312">När en enhet startar, skickar information om hello kommandon stöder toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-312">When a device first starts, it sends information about hello commands it supports toohello solution.</span></span> <span data-ttu-id="7bf4e-313">En beskrivning av hello skillnaderna mellan kommandon och metoder finns [alternativ för Azure IoT Hub-moln till enhet][lnk-c2d-guidance].</span><span class="sxs-lookup"><span data-stu-id="7bf4e-313">For a discussion of hello differences between commands and methods, see [Azure IoT Hub cloud-to-device options][lnk-c2d-guidance].</span></span>

1. <span data-ttu-id="7bf4e-314">Välj **kommandon** i hello **enhetsinformation** panelen för hello vald enhet:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-314">Choose **Commands** in hello **Device Details** panel for hello selected device:</span></span>

   ![Enhetskommandon på instrumentpanelen][img-devicecommands]

1. <span data-ttu-id="7bf4e-316">Välj **PingDevice** hello kommandot listan.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-316">Select **PingDevice** from hello command list.</span></span>

1. <span data-ttu-id="7bf4e-317">Klicka på **Skicka kommando**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-317">Choose **Send Command**.</span></span>

1. <span data-ttu-id="7bf4e-318">Du kan se hello status för hello-kommandot i historiken för hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-318">You can see hello status of hello command in hello command history.</span></span>

   ![Kommandostatus på instrumentpanelen][img-pingcommand]

<span data-ttu-id="7bf4e-320">hello lösning spårar hello status för varje kommando som skickas.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-320">hello solution tracks hello status of each command it sends.</span></span> <span data-ttu-id="7bf4e-321">Ursprungligen hello resultatet är **väntande**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-321">Initially hello result is **Pending**.</span></span> <span data-ttu-id="7bf4e-322">När hello enheten rapporterar att den har körts hello-kommandot, hello är resultatet för**lyckade**.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-322">When hello device reports that it has executed hello command, hello result is set too**Success**.</span></span>

## <a name="behind-hello-scenes"></a><span data-ttu-id="7bf4e-323">Hello bakgrunden</span><span class="sxs-lookup"><span data-stu-id="7bf4e-323">Behind hello scenes</span></span>

<span data-ttu-id="7bf4e-324">När du distribuerar en förkonfigurerade lösning skapar hello distributionsprocessen flera resurser i hello Azure-prenumeration du valt.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-324">When you deploy a preconfigured solution, hello deployment process creates multiple resources in hello Azure subscription you selected.</span></span> <span data-ttu-id="7bf4e-325">Du kan visa dessa resurser i hello Azure [portal][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="7bf4e-325">You can view these resources in hello Azure [portal][lnk-portal].</span></span> <span data-ttu-id="7bf4e-326">hello distributionsprocessen skapar en **resursgruppen** med ett namn baserat på hello namn du väljer för din förkonfigurerade lösning:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-326">hello deployment process creates a **resource group** with a name based on hello name you choose for your preconfigured solution:</span></span>

![Förkonfigurerade lösning i hello Azure-portalen][img-portal]

<span data-ttu-id="7bf4e-328">Du kan visa hello inställningarna för varje resurs som väljs i hello listan över resurser i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-328">You can view hello settings of each resource by selecting it in hello list of resources in hello resource group.</span></span>

<span data-ttu-id="7bf4e-329">Du kan också visa hello källkoden för hello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-329">You can also view hello source code for hello preconfigured solution.</span></span> <span data-ttu-id="7bf4e-330">fjärråtkomst övervakning förkonfigurerade lösningen källkoden hello är i hello [azure iot-fjärr-övervakning] [ lnk-rmgithub] GitHub-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-330">hello remote monitoring preconfigured solution source code is in hello [azure-iot-remote-monitoring][lnk-rmgithub] GitHub repository:</span></span>

* <span data-ttu-id="7bf4e-331">Hej **DeviceAdministration** mappen innehåller hello källkoden för hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-331">hello **DeviceAdministration** folder contains hello source code for hello dashboard.</span></span>
* <span data-ttu-id="7bf4e-332">Hej **Simulator** mappen innehåller hello källkoden för hello simulerade enheten.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-332">hello **Simulator** folder contains hello source code for hello simulated device.</span></span>
* <span data-ttu-id="7bf4e-333">Hej **EventProcessor** mappen innehåller hello källkoden för hello backend-processen som hanterar hello inkommande telemetri.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-333">hello **EventProcessor** folder contains hello source code for hello back-end process that handles hello incoming telemetry.</span></span>

<span data-ttu-id="7bf4e-334">När du är klar kan du ta bort hello förkonfigurerade lösningen från din Azure-prenumeration på hello [azureiotsuite.com] [ lnk-azureiotsuite] plats.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-334">When you are done, you can delete hello preconfigured solution from your Azure subscription on hello [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="7bf4e-335">Den här webbplatsen kan tooeasily ta bort alla hello resurser som etablerades när du skapade hello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-335">This site enables you tooeasily delete all hello resources that were provisioned when you created hello preconfigured solution.</span></span>

> [!NOTE]
> <span data-ttu-id="7bf4e-336">tooensure att du tar bort allt relaterade toohello förkonfigurerade lösningen kan ta bort den på hello [azureiotsuite.com] [ lnk-azureiotsuite] plats och ta inte bort hello resursgrupp i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="7bf4e-336">tooensure that you delete everything related toohello preconfigured solution, delete it on hello [azureiotsuite.com][lnk-azureiotsuite] site and do not delete hello resource group in hello portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7bf4e-337">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7bf4e-337">Next Steps</span></span>

<span data-ttu-id="7bf4e-338">Nu när du har distribuerat en fungerande förkonfigurerade lösning kan fortsätta du komma igång med IoT Suite genom att läsa hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="7bf4e-338">Now that you’ve deployed a working preconfigured solution, you can continue getting started with IoT Suite by reading hello following articles:</span></span>

* <span data-ttu-id="7bf4e-339">[Genomgång av den förkonfigurerade lösningen för fjärrövervakning][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="7bf4e-339">[Remote monitoring preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="7bf4e-340">[Ansluta din enhet toohello remote förkonfigurerade övervakningslösning][lnk-connect-rm]</span><span class="sxs-lookup"><span data-stu-id="7bf4e-340">[Connect your device toohello remote monitoring preconfigured solution][lnk-connect-rm]</span></span>
* <span data-ttu-id="7bf4e-341">[Behörigheter för hello azureiotsuite.com plats][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="7bf4e-341">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>

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