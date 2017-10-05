---
title: "Genomgång av förebyggande underhåll | Microsoft Docs"
description: "En genomgång av den förkonfigurerade lösningen för förebyggande underhåll i Azure IoT."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3c48a716-b805-4c99-8177-414cc4bec3de
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: a68a8fdc3976ade0d1036d5ed58c8b2eb6d32a5d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a><span data-ttu-id="cadc8-103">Genomgång av den förkonfigurerade lösningen för förebyggande underhåll</span><span class="sxs-lookup"><span data-stu-id="cadc8-103">Predictive maintenance preconfigured solution walkthrough</span></span>

<span data-ttu-id="cadc8-104">Den förkonfigurerade lösningen för förutsägande underhåll är en lösning från slutpunkt till slutpunkt för ett affärsscenario som beräknar den punkt då det är troligt att ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="cadc8-104">The predictive maintenance preconfigured solution is an end-to-end solution for a business scenario that predicts the point at which a failure is likely to occur.</span></span> <span data-ttu-id="cadc8-105">Du kan använda denna förkonfigurerade lösning proaktivt för aktiviteter, till exempel för att optimera underhåll.</span><span class="sxs-lookup"><span data-stu-id="cadc8-105">You can use this preconfigured solution proactively for activities such as optimizing maintenance.</span></span> <span data-ttu-id="cadc8-106">Lösningen kombinerar viktiga tjänster i Azure IoT Suite, till exempel IoT Hub, Stream analytics och en [Azure Machine Learning][lnk-machine-learning]-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="cadc8-106">The solution combines key Azure IoT Suite services, such as IoT Hub, Stream analytics, and an [Azure Machine Learning][lnk-machine-learning] workspace.</span></span> <span data-ttu-id="cadc8-107">Den här arbetsytan innehåller en modell, baserad på en offentlig provdatauppsättning, för att förutsäga återstående driftstid (RUL) för en flygplansmotor.</span><span class="sxs-lookup"><span data-stu-id="cadc8-107">This workspace contains a model, based on a public sample data set, to predict the Remaining Useful Life (RUL) of an aircraft engine.</span></span> <span data-ttu-id="cadc8-108">Lösningen implementerar IoT-affärsscenariot som startpunkt när du planerar och implementerar en lösning som uppfyller dina specifika affärskrav.</span><span class="sxs-lookup"><span data-stu-id="cadc8-108">The solution fully implements the IoT business scenario as a starting point for you to plan and implement a solution that meets your own specific business requirements.</span></span>

## <a name="logical-architecture"></a><span data-ttu-id="cadc8-109">Logisk arkitektur</span><span class="sxs-lookup"><span data-stu-id="cadc8-109">Logical architecture</span></span>

<span data-ttu-id="cadc8-110">Följande diagram illustrerar de logiska komponenterna i den förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="cadc8-110">The following diagram outlines the logical components of the preconfigured solution:</span></span>

![][img-architecture]

<span data-ttu-id="cadc8-111">De blå objekten är Azure-tjänster som är etablerade i den region där du etablerade den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="cadc8-111">The blue items are Azure services provisioned in the region where you deployed the preconfigured solution.</span></span> <span data-ttu-id="cadc8-112">På [etableringssidan][lnk-azureiotsuite] finns en lista över de regioner där du kan distribuera den förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="cadc8-112">The list of regions where you can deploy the preconfigured solution displays on the [provisioning page][lnk-azureiotsuite].</span></span>

<span data-ttu-id="cadc8-113">Det gröna objektet är en simulerad enhet som representerar en flygplansmotor.</span><span class="sxs-lookup"><span data-stu-id="cadc8-113">The green item is a simulated device that represents an aircraft engine.</span></span> <span data-ttu-id="cadc8-114">Mer information om dessa simulerade enheter finns i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cadc8-114">You can learn more about these simulated devices in the following section.</span></span>

<span data-ttu-id="cadc8-115">De grå objekten representerar komponenter som implementerar funktioner för *enhetshantering*.</span><span class="sxs-lookup"><span data-stu-id="cadc8-115">The gray items represent components that implement *device management* capabilities.</span></span> <span data-ttu-id="cadc8-116">Den aktuella versionen av den förkonfigurerade lösningen för förebyggande underhåll etablerar inte dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="cadc8-116">The current release of the predictive maintenance preconfigured solution does not provision these resources.</span></span> <span data-ttu-id="cadc8-117">Mer information om enhetsadministration finns i dokumentationen för [den förkonfigurerade lösningen för fjärrövervakning][lnk-remote-monitoring].</span><span class="sxs-lookup"><span data-stu-id="cadc8-117">To learn more about device management, refer to the [remote monitoring pre-configured solution][lnk-remote-monitoring].</span></span>

## <a name="simulated-devices"></a><span data-ttu-id="cadc8-118">Simulerade enheter</span><span class="sxs-lookup"><span data-stu-id="cadc8-118">Simulated devices</span></span>

<span data-ttu-id="cadc8-119">I den förkonfigurerade lösningen representerar en simulerad enhet en flygplansmotor.</span><span class="sxs-lookup"><span data-stu-id="cadc8-119">In the preconfigured solution, a simulated device represents an aircraft engine.</span></span> <span data-ttu-id="cadc8-120">Lösningen etableras med två motorer som mappar till ett enda flygplan.</span><span class="sxs-lookup"><span data-stu-id="cadc8-120">The solution is provisioned with two engines that map to a single aircraft.</span></span> <span data-ttu-id="cadc8-121">Varje motor sänder fyra typer av telemetri: Sensor 9, Sensor 11, Sensor 14 och Sensor 15 tillhandahåller de data som behövs för att Machine Learning-modellen ska kunna beräkna motorns återstående driftstid.</span><span class="sxs-lookup"><span data-stu-id="cadc8-121">Each engine emits four types of telemetry: Sensor 9, Sensor 11, Sensor 14, and Sensor 15 provide the data necessary for the Machine Learning model to calculate the RUL for the engine.</span></span> <span data-ttu-id="cadc8-122">Varje simulerad enhet skickar följande telemetrimeddelanden till IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="cadc8-122">Each simulated device sends the following telemetry messages to IoT Hub:</span></span>

<span data-ttu-id="cadc8-123">*Antal cykler*.</span><span class="sxs-lookup"><span data-stu-id="cadc8-123">*Cycle count*.</span></span> <span data-ttu-id="cadc8-124">En cykel representerar en slutförd flygning med varierande längd på mellan 2–10 timmar.</span><span class="sxs-lookup"><span data-stu-id="cadc8-124">A cycle represents a completed flight with a duration between two and ten hours.</span></span> <span data-ttu-id="cadc8-125">Telemetridata samlas in varje halvtimme under flygningens längd.</span><span class="sxs-lookup"><span data-stu-id="cadc8-125">During the flight, telemetry data is captured every half hour.</span></span>

<span data-ttu-id="cadc8-126">*Telemetri*.</span><span class="sxs-lookup"><span data-stu-id="cadc8-126">*Telemetry*.</span></span> <span data-ttu-id="cadc8-127">Det finns fyra sensorer som representerar motorattribut.</span><span class="sxs-lookup"><span data-stu-id="cadc8-127">There are four sensors that represent engine attributes.</span></span> <span data-ttu-id="cadc8-128">Sensorerna har allmänna beteckningar: Sensor 9, Sensor 11, Sensor 14 och Sensor 15.</span><span class="sxs-lookup"><span data-stu-id="cadc8-128">The sensors are generically labeled Sensor 9, Sensor 11, Sensor 14, and Sensor 15.</span></span> <span data-ttu-id="cadc8-129">Dessa fyra sensorer representerar den mängd telemetri som krävs för att få fram användbara resultat från RUL-modellen.</span><span class="sxs-lookup"><span data-stu-id="cadc8-129">These four sensors represent telemetry sufficient to obtain useful results from the RUL model.</span></span> <span data-ttu-id="cadc8-130">Modellen som används i den förkonfigurerade lösningen har skapats från en offentlig datauppsättning som innehåller verkliga data från motorsensorer.</span><span class="sxs-lookup"><span data-stu-id="cadc8-130">The model used in the preconfigured solution is created from a public data set that includes real engine sensor data.</span></span> <span data-ttu-id="cadc8-131">Mer information om hur modellen har skapats från den ursprungliga datauppsättningen finns i [mallen för förebyggande underhåll i Cortana Intelligence-galleriet][lnk-cortana-analytics].</span><span class="sxs-lookup"><span data-stu-id="cadc8-131">For more information on how the model was created from the original data set, see the [Cortana Intelligence Gallery Predictive Maintenance Template][lnk-cortana-analytics].</span></span>

<span data-ttu-id="cadc8-132">De simulerade enheterna kan hantera följande kommandon som skickas från IoT Hub i lösningen:</span><span class="sxs-lookup"><span data-stu-id="cadc8-132">The simulated devices can handle the following commands sent from the IoT hub in the solution:</span></span>

| <span data-ttu-id="cadc8-133">Kommando</span><span class="sxs-lookup"><span data-stu-id="cadc8-133">Command</span></span> | <span data-ttu-id="cadc8-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cadc8-134">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cadc8-135">StartTelemetry</span><span class="sxs-lookup"><span data-stu-id="cadc8-135">StartTelemetry</span></span> |<span data-ttu-id="cadc8-136">Kontrollerar tillståndet för simuleringen.</span><span class="sxs-lookup"><span data-stu-id="cadc8-136">Controls the state of the simulation.</span></span><br/><span data-ttu-id="cadc8-137">Startar enheten som skickar telemetri</span><span class="sxs-lookup"><span data-stu-id="cadc8-137">Starts the device sending telemetry</span></span> |
| <span data-ttu-id="cadc8-138">StopTelemetry</span><span class="sxs-lookup"><span data-stu-id="cadc8-138">StopTelemetry</span></span> |<span data-ttu-id="cadc8-139">Kontrollerar tillståndet för simuleringen.</span><span class="sxs-lookup"><span data-stu-id="cadc8-139">Controls the state of the simulation.</span></span><br/><span data-ttu-id="cadc8-140">Stoppar enheten som skickar telemetri</span><span class="sxs-lookup"><span data-stu-id="cadc8-140">Stops the device sending telemetry</span></span> |

<span data-ttu-id="cadc8-141">IoT Hub bekräftar enhetskommandona.</span><span class="sxs-lookup"><span data-stu-id="cadc8-141">IoT Hub provides device command acknowledgment.</span></span>

## <a name="azure-stream-analytics-job"></a><span data-ttu-id="cadc8-142">Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="cadc8-142">Azure Stream Analytics job</span></span>

<span data-ttu-id="cadc8-143">**Jobb: Telemetri** körs på den inkommande telemetriströmmen för enheten med hjälp av två uttryck:</span><span class="sxs-lookup"><span data-stu-id="cadc8-143">**Job: Telemetry** operates on the incoming device telemetry stream using two statements:</span></span>

* <span data-ttu-id="cadc8-144">Det första uttrycket väljer all telemetri från enheterna och skickar dessa data till Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="cadc8-144">The first selects all telemetry from the devices and sends this data to blob storage.</span></span> <span data-ttu-id="cadc8-145">Härifrån kan de sedan visualiseras i webbappen.</span><span class="sxs-lookup"><span data-stu-id="cadc8-145">From here, it is visualized in the web app.</span></span>
* <span data-ttu-id="cadc8-146">Den andra instruktionen beräknar genomsnittliga sensorvärden under en glidande tvåminutersperiod och skickar dem via händelsehubben till en **händelseprocessor**.</span><span class="sxs-lookup"><span data-stu-id="cadc8-146">The second computes average sensor values over a two-minute sliding window and sends this data through the Event hub to an **event processor**.</span></span>

## <a name="event-processor"></a><span data-ttu-id="cadc8-147">Händelseprocessor</span><span class="sxs-lookup"><span data-stu-id="cadc8-147">Event processor</span></span>
<span data-ttu-id="cadc8-148">**Händelseprocessorvärden** körs i ett Azure-webbjobb.</span><span class="sxs-lookup"><span data-stu-id="cadc8-148">The **event processor host** runs in an Azure Web Job.</span></span> <span data-ttu-id="cadc8-149">**Händelseprocessorn** tar de genomsnittliga sensorvärdena från en slutförd cykel.</span><span class="sxs-lookup"><span data-stu-id="cadc8-149">The **event processor** takes the average sensor values for a completed cycle.</span></span> <span data-ttu-id="cadc8-150">Den skickar dessa värden till en API som får den tränade modellen att beräkna RUL för en motor.</span><span class="sxs-lookup"><span data-stu-id="cadc8-150">It then passes those values to an API that exposes trained model to calculate the RUL for an engine.</span></span> <span data-ttu-id="cadc8-151">API: et exponeras av en Machine Learning-arbetsyta som har etablerats som en del av lösningen.</span><span class="sxs-lookup"><span data-stu-id="cadc8-151">The API is exposed by a Machine Learning workspace that is provisioned as part of the solution.</span></span>

## <a name="machine-learning"></a><span data-ttu-id="cadc8-152">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="cadc8-152">Machine Learning</span></span>
<span data-ttu-id="cadc8-153">Machine Learning-komponenten använder en modell som härletts från de data som samlats in från verkliga luftfartygsmotorer.</span><span class="sxs-lookup"><span data-stu-id="cadc8-153">The Machine Learning component uses a model derived from data collected from real aircraft engines.</span></span> <span data-ttu-id="cadc8-154">Du kan navigera till Machine Learning-arbetsytan från ikonen på sidan [azureiotsuite.com][lnk-azureiotsuite] för din etablerade lösning.</span><span class="sxs-lookup"><span data-stu-id="cadc8-154">You can navigate to the Machine Learning workspace from the tile on the [azureiotsuite.com][lnk-azureiotsuite] page for your provisioned solution.</span></span> <span data-ttu-id="cadc8-155">Panelen är tillgänglig när lösningen har statusen **Redo**.</span><span class="sxs-lookup"><span data-stu-id="cadc8-155">The tile is available when the solution is in the **Ready** state.</span></span>


## <a name="next-steps"></a><span data-ttu-id="cadc8-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cadc8-156">Next steps</span></span>
<span data-ttu-id="cadc8-157">Nu när du har sett huvudkomponenterna i den förkonfigurerade lösningen för förebyggande underhåll kanske du vill anpassa den.</span><span class="sxs-lookup"><span data-stu-id="cadc8-157">Now you've seen the key components of the predictive maintenance preconfigured solution, you may want to customize it.</span></span> <span data-ttu-id="cadc8-158">Se [Vägledning för anpassning av förkonfigurerade lösningar][lnk-customize].</span><span class="sxs-lookup"><span data-stu-id="cadc8-158">See [Guidance on customizing preconfigured solutions][lnk-customize].</span></span>

<span data-ttu-id="cadc8-159">Du kan även utforska några andra funktioner och möjligheter i de förkonfigurerade lösningarna i IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="cadc8-159">You can also explore some of the other features and capabilities of the IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="cadc8-160">[Vanliga frågor och svar om IoT Suite][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="cadc8-160">[Frequently asked questions for IoT Suite][lnk-faq]</span></span>
* <span data-ttu-id="cadc8-161">[IoT-säkerhet från grunden][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="cadc8-161">[IoT security from the ground up][lnk-security-groundup]</span></span>

[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png

[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/