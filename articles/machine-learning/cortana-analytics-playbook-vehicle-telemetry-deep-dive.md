---
title: "aaaDeep fördjupa dig i förutsäga vehicle hälsotillstånd och andra vanor - Azure | Microsoft Docs"
description: "Använda hello funktionerna i Cortana Intelligence toogain i realtid och förutsägbara insikter på vehicle hälsotillstånd och andra vanor."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: ba1448a5081762292561f904d9ec54617c9a5330
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-hello-solution"></a><span data-ttu-id="0694a-103">Vehicle telemetri analytics lösning playbook: ingående i hello lösning</span><span class="sxs-lookup"><span data-stu-id="0694a-103">Vehicle telemetry analytics solution playbook: deep dive into hello solution</span></span>
<span data-ttu-id="0694a-104">Detta **menyn** länkar toohello avsnitt i den här playbook:</span><span class="sxs-lookup"><span data-stu-id="0694a-104">This **menu** links toohello sections of this playbook:</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

<span data-ttu-id="0694a-105">Det här avsnittet flyttar ned till de enskilda hello stegen som beskrivs i hello lösningsarkitektur med instruktioner och pekare för anpassning.</span><span class="sxs-lookup"><span data-stu-id="0694a-105">This section drills down into each of hello stages depicted in hello Solution Architecture with instructions and pointers for customization.</span></span> 

## <a name="data-sources"></a><span data-ttu-id="0694a-106">Datakällor</span><span class="sxs-lookup"><span data-stu-id="0694a-106">Data Sources</span></span>
<span data-ttu-id="0694a-107">hello lösningen använder två olika datakällor:</span><span class="sxs-lookup"><span data-stu-id="0694a-107">hello solution uses two different data sources:</span></span>

* <span data-ttu-id="0694a-108">**simulerade vehicle signaler och diagnostik dataset** och</span><span class="sxs-lookup"><span data-stu-id="0694a-108">**simulated vehicle signals and diagnostic dataset** and</span></span> 
* <span data-ttu-id="0694a-109">**vehicle katalog**</span><span class="sxs-lookup"><span data-stu-id="0694a-109">**vehicle catalog**</span></span>

<span data-ttu-id="0694a-110">Vehicle telematik simulator ingår som en del av den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="0694a-110">A vehicle telematics simulator is included as part of this solution.</span></span> <span data-ttu-id="0694a-111">Den genererar diagnostisk information och signalerar till motsvarande toohello tillståndet för hello vehicle och toohello körning mönster vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="0694a-111">It emits diagnostic information and signals corresponding toohello state of hello vehicle and toohello driving pattern at a given point in time.</span></span> <span data-ttu-id="0694a-112">Klicka på [Vehicle telematik Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) toodownload hello **Vehicle telematik Simulator lösning i Visual Studio** anpassningar baserat på dina krav.</span><span class="sxs-lookup"><span data-stu-id="0694a-112">Click [Vehicle Telematics Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) toodownload hello **Vehicle Telematics Simulator Visual Studio Solution** for customizations based on your requirements.</span></span> <span data-ttu-id="0694a-113">hello vehicle katalogen innehåller en referens datamängd med en VIN toomodel mappning.</span><span class="sxs-lookup"><span data-stu-id="0694a-113">hello vehicle catalog contains a reference dataset with a VIN toomodel mapping.</span></span>

![Vehicle telematik simulator](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)

<span data-ttu-id="0694a-115">*Bild 1 – Vehicle telematik Simulator*</span><span class="sxs-lookup"><span data-stu-id="0694a-115">*Figure 1 – Vehicle Telematics Simulator*</span></span>

<span data-ttu-id="0694a-116">Det här är en JSON-formaterad datamängd som innehåller hello följer schemat.</span><span class="sxs-lookup"><span data-stu-id="0694a-116">This is a JSON-formatted dataset that contains hello following schema.</span></span>

| <span data-ttu-id="0694a-117">Kolumn</span><span class="sxs-lookup"><span data-stu-id="0694a-117">Column</span></span> | <span data-ttu-id="0694a-118">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0694a-118">Description</span></span> | <span data-ttu-id="0694a-119">Värden</span><span class="sxs-lookup"><span data-stu-id="0694a-119">Values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0694a-120">VIN</span><span class="sxs-lookup"><span data-stu-id="0694a-120">VIN</span></span> |<span data-ttu-id="0694a-121">Slumpmässigt genererat Vehicle ID-nummer</span><span class="sxs-lookup"><span data-stu-id="0694a-121">Randomly generated Vehicle Identification Number</span></span> |<span data-ttu-id="0694a-122">Detta hämtas från en övergripande lista med 10 000 slumpmässigt genererat vehicle ID-nummer.</span><span class="sxs-lookup"><span data-stu-id="0694a-122">This is obtained from a master list of 10,000 randomly generated vehicle identification numbers.</span></span> |
| <span data-ttu-id="0694a-123">Utanför temperatur</span><span class="sxs-lookup"><span data-stu-id="0694a-123">Outside temperature</span></span> |<span data-ttu-id="0694a-124">hello utanför temperatur där hello vehicle kör</span><span class="sxs-lookup"><span data-stu-id="0694a-124">hello outside temperature where hello vehicle is driving</span></span> |<span data-ttu-id="0694a-125">Slumptal mellan 0-100</span><span class="sxs-lookup"><span data-stu-id="0694a-125">Randomly generated number from 0-100</span></span> |
| <span data-ttu-id="0694a-126">Motorn temperatur</span><span class="sxs-lookup"><span data-stu-id="0694a-126">Engine temperature</span></span> |<span data-ttu-id="0694a-127">hello motorn temperatur hello vehicle</span><span class="sxs-lookup"><span data-stu-id="0694a-127">hello engine temperature of hello vehicle</span></span> |<span data-ttu-id="0694a-128">Slumpmässigt genererat tal mellan 0 och 500</span><span class="sxs-lookup"><span data-stu-id="0694a-128">Randomly generated number from 0-500</span></span> |
| <span data-ttu-id="0694a-129">Hastighet</span><span class="sxs-lookup"><span data-stu-id="0694a-129">Speed</span></span> |<span data-ttu-id="0694a-130">hello hastighet på vilka hello vehicle körning</span><span class="sxs-lookup"><span data-stu-id="0694a-130">hello engine speed at which hello vehicle is driving</span></span> |<span data-ttu-id="0694a-131">Slumptal mellan 0-100</span><span class="sxs-lookup"><span data-stu-id="0694a-131">Randomly generated number from 0-100</span></span> |
| <span data-ttu-id="0694a-132">Bränsle</span><span class="sxs-lookup"><span data-stu-id="0694a-132">Fuel</span></span> |<span data-ttu-id="0694a-133">hello bränslenivå hello fordon</span><span class="sxs-lookup"><span data-stu-id="0694a-133">hello fuel level of hello vehicle</span></span> |<span data-ttu-id="0694a-134">Slumptal mellan 0-100 (anger bränsle nivån procent)</span><span class="sxs-lookup"><span data-stu-id="0694a-134">Randomly generated number from 0-100 (indicates fuel level percentage)</span></span> |
| <span data-ttu-id="0694a-135">EngineOil</span><span class="sxs-lookup"><span data-stu-id="0694a-135">EngineOil</span></span> |<span data-ttu-id="0694a-136">hello motorn olja andelen hello vehicle</span><span class="sxs-lookup"><span data-stu-id="0694a-136">hello engine oil level of hello vehicle</span></span> |<span data-ttu-id="0694a-137">Slumptal mellan 0-100 (anger motorn olja nivån procent)</span><span class="sxs-lookup"><span data-stu-id="0694a-137">Randomly generated number from 0-100 (indicates engine oil level percentage)</span></span> |
| <span data-ttu-id="0694a-138">Däck hög belastning</span><span class="sxs-lookup"><span data-stu-id="0694a-138">Tire pressure</span></span> |<span data-ttu-id="0694a-139">hello däck trycket hello vehicle</span><span class="sxs-lookup"><span data-stu-id="0694a-139">hello tire pressure of hello vehicle</span></span> |<span data-ttu-id="0694a-140">Slumpmässigt tal mellan 0-50 (anger däck trycket nivån procent)</span><span class="sxs-lookup"><span data-stu-id="0694a-140">Randomly generated number from 0-50 (indicates tire pressure level percentage)</span></span> |
| <span data-ttu-id="0694a-141">Mätarställning</span><span class="sxs-lookup"><span data-stu-id="0694a-141">Odometer</span></span> |<span data-ttu-id="0694a-142">hello Mätarställning hello fordon</span><span class="sxs-lookup"><span data-stu-id="0694a-142">hello odometer reading of hello vehicle</span></span> |<span data-ttu-id="0694a-143">Slumpmässigt genererat nummer från 0 200000</span><span class="sxs-lookup"><span data-stu-id="0694a-143">Randomly generated number from 0-200000</span></span> |
| <span data-ttu-id="0694a-144">Accelerator_pedal_position</span><span class="sxs-lookup"><span data-stu-id="0694a-144">Accelerator_pedal_position</span></span> |<span data-ttu-id="0694a-145">hello accelerator cyklar positionen för hello vehicle</span><span class="sxs-lookup"><span data-stu-id="0694a-145">hello accelerator pedal position of hello vehicle</span></span> |<span data-ttu-id="0694a-146">Slumptal mellan 0-100 (anger accelerator nivån procent)</span><span class="sxs-lookup"><span data-stu-id="0694a-146">Randomly generated number from 0-100 (indicates accelerator level percentage)</span></span> |
| <span data-ttu-id="0694a-147">Parking_brake_status</span><span class="sxs-lookup"><span data-stu-id="0694a-147">Parking_brake_status</span></span> |<span data-ttu-id="0694a-148">Anger om hello vehicle parkerade eller inte</span><span class="sxs-lookup"><span data-stu-id="0694a-148">Indicates whether hello vehicle is parked or not</span></span> |<span data-ttu-id="0694a-149">True eller False</span><span class="sxs-lookup"><span data-stu-id="0694a-149">True or False</span></span> |
| <span data-ttu-id="0694a-150">Headlamp_status</span><span class="sxs-lookup"><span data-stu-id="0694a-150">Headlamp_status</span></span> |<span data-ttu-id="0694a-151">Anger om hello strålkastaren är på eller inte</span><span class="sxs-lookup"><span data-stu-id="0694a-151">Indicates where hello headlamp is on or not</span></span> |<span data-ttu-id="0694a-152">True eller False</span><span class="sxs-lookup"><span data-stu-id="0694a-152">True or False</span></span> |
| <span data-ttu-id="0694a-153">Brake_pedal_status</span><span class="sxs-lookup"><span data-stu-id="0694a-153">Brake_pedal_status</span></span> |<span data-ttu-id="0694a-154">Anger om hello bromspedal är nedtryckt eller inte</span><span class="sxs-lookup"><span data-stu-id="0694a-154">Indicates whether hello brake pedal is pressed or not</span></span> |<span data-ttu-id="0694a-155">True eller False</span><span class="sxs-lookup"><span data-stu-id="0694a-155">True or False</span></span> |
| <span data-ttu-id="0694a-156">Transmission_gear_position</span><span class="sxs-lookup"><span data-stu-id="0694a-156">Transmission_gear_position</span></span> |<span data-ttu-id="0694a-157">hello överföring växeln positionen för hello vehicle</span><span class="sxs-lookup"><span data-stu-id="0694a-157">hello transmission gear position of hello vehicle</span></span> |<span data-ttu-id="0694a-158">Tillstånd: första, andra, tredje, fjärde, femte, sjätte, sjunde, åttonde</span><span class="sxs-lookup"><span data-stu-id="0694a-158">States: first, second, third, fourth, fifth, sixth, seventh, eighth</span></span> |
| <span data-ttu-id="0694a-159">Ignition_status</span><span class="sxs-lookup"><span data-stu-id="0694a-159">Ignition_status</span></span> |<span data-ttu-id="0694a-160">Anger om hello vehicle är igång eller Stoppad</span><span class="sxs-lookup"><span data-stu-id="0694a-160">Indicates whether hello vehicle is running or stopped</span></span> |<span data-ttu-id="0694a-161">True eller False</span><span class="sxs-lookup"><span data-stu-id="0694a-161">True or False</span></span> |
| <span data-ttu-id="0694a-162">Windshield_wiper_status</span><span class="sxs-lookup"><span data-stu-id="0694a-162">Windshield_wiper_status</span></span> |<span data-ttu-id="0694a-163">Anger om hello vindrutan vindrutetorkare är aktiverat eller inte</span><span class="sxs-lookup"><span data-stu-id="0694a-163">Indicates whether hello windshield wiper is turned or not</span></span> |<span data-ttu-id="0694a-164">True eller False</span><span class="sxs-lookup"><span data-stu-id="0694a-164">True or False</span></span> |
| <span data-ttu-id="0694a-165">ABS</span><span class="sxs-lookup"><span data-stu-id="0694a-165">ABS</span></span> |<span data-ttu-id="0694a-166">Anger om ABS används eller inte</span><span class="sxs-lookup"><span data-stu-id="0694a-166">Indicates whether ABS is engaged or not</span></span> |<span data-ttu-id="0694a-167">True eller False</span><span class="sxs-lookup"><span data-stu-id="0694a-167">True or False</span></span> |
| <span data-ttu-id="0694a-168">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="0694a-168">Timestamp</span></span> |<span data-ttu-id="0694a-169">hello tidsstämpel när hello datapunkt har skapats</span><span class="sxs-lookup"><span data-stu-id="0694a-169">hello timestamp when hello data point is created</span></span> |<span data-ttu-id="0694a-170">Date</span><span class="sxs-lookup"><span data-stu-id="0694a-170">Date</span></span> |
| <span data-ttu-id="0694a-171">Ort</span><span class="sxs-lookup"><span data-stu-id="0694a-171">City</span></span> |<span data-ttu-id="0694a-172">hello platsen för hello vehicle</span><span class="sxs-lookup"><span data-stu-id="0694a-172">hello location of hello vehicle</span></span> |<span data-ttu-id="0694a-173">4 orter i den här lösningen: Bellevue, Redmond, Sammamish, Seattle</span><span class="sxs-lookup"><span data-stu-id="0694a-173">4 cities in this solution: Bellevue, Redmond, Sammamish, Seattle</span></span> |

<span data-ttu-id="0694a-174">referensdatauppsättningen för hello vehicle modellen innehåller VIN toohello Modellmappning.</span><span class="sxs-lookup"><span data-stu-id="0694a-174">hello vehicle model reference dataset contains VIN toohello model mapping.</span></span> 

| <span data-ttu-id="0694a-175">VIN</span><span class="sxs-lookup"><span data-stu-id="0694a-175">VIN</span></span> | <span data-ttu-id="0694a-176">Modellen</span><span class="sxs-lookup"><span data-stu-id="0694a-176">Model</span></span> |
| --- | --- |
| <span data-ttu-id="0694a-177">FHL3O1SA4IEHB4WU1</span><span class="sxs-lookup"><span data-stu-id="0694a-177">FHL3O1SA4IEHB4WU1</span></span> |<span data-ttu-id="0694a-178">Sedan</span><span class="sxs-lookup"><span data-stu-id="0694a-178">Sedan</span></span> |
| <span data-ttu-id="0694a-179">8J0U8XCPRGW4Z3NQE</span><span class="sxs-lookup"><span data-stu-id="0694a-179">8J0U8XCPRGW4Z3NQE</span></span> |<span data-ttu-id="0694a-180">Hybrid</span><span class="sxs-lookup"><span data-stu-id="0694a-180">Hybrid</span></span> |
| <span data-ttu-id="0694a-181">WORG68Z2PLTNZDBI7</span><span class="sxs-lookup"><span data-stu-id="0694a-181">WORG68Z2PLTNZDBI7</span></span> |<span data-ttu-id="0694a-182">Family sedan</span><span class="sxs-lookup"><span data-stu-id="0694a-182">Family Saloon</span></span> |
| <span data-ttu-id="0694a-183">JTHMYHQTEPP4WBMRN</span><span class="sxs-lookup"><span data-stu-id="0694a-183">JTHMYHQTEPP4WBMRN</span></span> |<span data-ttu-id="0694a-184">Sedan</span><span class="sxs-lookup"><span data-stu-id="0694a-184">Sedan</span></span> |
| <span data-ttu-id="0694a-185">W9FTHG27LZN1YWO0Y</span><span class="sxs-lookup"><span data-stu-id="0694a-185">W9FTHG27LZN1YWO0Y</span></span> |<span data-ttu-id="0694a-186">Hybrid</span><span class="sxs-lookup"><span data-stu-id="0694a-186">Hybrid</span></span> |
| <span data-ttu-id="0694a-187">MHTP9N792PHK08WJM</span><span class="sxs-lookup"><span data-stu-id="0694a-187">MHTP9N792PHK08WJM</span></span> |<span data-ttu-id="0694a-188">Family sedan</span><span class="sxs-lookup"><span data-stu-id="0694a-188">Family Saloon</span></span> |
| <span data-ttu-id="0694a-189">EI4QXI2AXVQQING4I</span><span class="sxs-lookup"><span data-stu-id="0694a-189">EI4QXI2AXVQQING4I</span></span> |<span data-ttu-id="0694a-190">Sedan</span><span class="sxs-lookup"><span data-stu-id="0694a-190">Sedan</span></span> |
| <span data-ttu-id="0694a-191">5KKR2VB4WHQH97PF8</span><span class="sxs-lookup"><span data-stu-id="0694a-191">5KKR2VB4WHQH97PF8</span></span> |<span data-ttu-id="0694a-192">Hybrid</span><span class="sxs-lookup"><span data-stu-id="0694a-192">Hybrid</span></span> |
| <span data-ttu-id="0694a-193">W9NSZ423XZHAONYXB</span><span class="sxs-lookup"><span data-stu-id="0694a-193">W9NSZ423XZHAONYXB</span></span> |<span data-ttu-id="0694a-194">Family sedan</span><span class="sxs-lookup"><span data-stu-id="0694a-194">Family Saloon</span></span> |
| <span data-ttu-id="0694a-195">26WJSGHX4MA5ROHNL</span><span class="sxs-lookup"><span data-stu-id="0694a-195">26WJSGHX4MA5ROHNL</span></span> |<span data-ttu-id="0694a-196">Kan konverteras</span><span class="sxs-lookup"><span data-stu-id="0694a-196">Convertible</span></span> |
| <span data-ttu-id="0694a-197">GHLUB6ONKMOSI7E77</span><span class="sxs-lookup"><span data-stu-id="0694a-197">GHLUB6ONKMOSI7E77</span></span> |<span data-ttu-id="0694a-198">Station vagn</span><span class="sxs-lookup"><span data-stu-id="0694a-198">Station Wagon</span></span> |
| <span data-ttu-id="0694a-199">9C2RHVRVLMEJDBXLP</span><span class="sxs-lookup"><span data-stu-id="0694a-199">9C2RHVRVLMEJDBXLP</span></span> |<span data-ttu-id="0694a-200">Bil</span><span class="sxs-lookup"><span data-stu-id="0694a-200">Compact Car</span></span> |
| <span data-ttu-id="0694a-201">BRNHVMZOUJ6EOCP32</span><span class="sxs-lookup"><span data-stu-id="0694a-201">BRNHVMZOUJ6EOCP32</span></span> |<span data-ttu-id="0694a-202">Liten Stadsjeep</span><span class="sxs-lookup"><span data-stu-id="0694a-202">Small SUV</span></span> |
| <span data-ttu-id="0694a-203">VCYVW0WUZNBTM594J</span><span class="sxs-lookup"><span data-stu-id="0694a-203">VCYVW0WUZNBTM594J</span></span> |<span data-ttu-id="0694a-204">Sport bil</span><span class="sxs-lookup"><span data-stu-id="0694a-204">Sports Car</span></span> |
| <span data-ttu-id="0694a-205">HNVCE6YFZSA5M82NY</span><span class="sxs-lookup"><span data-stu-id="0694a-205">HNVCE6YFZSA5M82NY</span></span> |<span data-ttu-id="0694a-206">Medelhög Stadsjeep</span><span class="sxs-lookup"><span data-stu-id="0694a-206">Medium SUV</span></span> |
| <span data-ttu-id="0694a-207">4R30FOR7NUOBL05GJ</span><span class="sxs-lookup"><span data-stu-id="0694a-207">4R30FOR7NUOBL05GJ</span></span> |<span data-ttu-id="0694a-208">Station vagn</span><span class="sxs-lookup"><span data-stu-id="0694a-208">Station Wagon</span></span> |
| <span data-ttu-id="0694a-209">WYNIIY42VKV6OQS1J</span><span class="sxs-lookup"><span data-stu-id="0694a-209">WYNIIY42VKV6OQS1J</span></span> |<span data-ttu-id="0694a-210">Stora Stadsjeep</span><span class="sxs-lookup"><span data-stu-id="0694a-210">Large SUV</span></span> |
| <span data-ttu-id="0694a-211">8Y5QKG27QET1RBK7I</span><span class="sxs-lookup"><span data-stu-id="0694a-211">8Y5QKG27QET1RBK7I</span></span> |<span data-ttu-id="0694a-212">Stora Stadsjeep</span><span class="sxs-lookup"><span data-stu-id="0694a-212">Large SUV</span></span> |
| <span data-ttu-id="0694a-213">DF6OX2WSRA6511BVG</span><span class="sxs-lookup"><span data-stu-id="0694a-213">DF6OX2WSRA6511BVG</span></span> |<span data-ttu-id="0694a-214">Coupe</span><span class="sxs-lookup"><span data-stu-id="0694a-214">Coupe</span></span> |
| <span data-ttu-id="0694a-215">Z2EOZWZBXAEW3E60T</span><span class="sxs-lookup"><span data-stu-id="0694a-215">Z2EOZWZBXAEW3E60T</span></span> |<span data-ttu-id="0694a-216">Sedan</span><span class="sxs-lookup"><span data-stu-id="0694a-216">Sedan</span></span> |
| <span data-ttu-id="0694a-217">M4TV6IEALD5QDS3IR</span><span class="sxs-lookup"><span data-stu-id="0694a-217">M4TV6IEALD5QDS3IR</span></span> |<span data-ttu-id="0694a-218">Hybrid</span><span class="sxs-lookup"><span data-stu-id="0694a-218">Hybrid</span></span> |
| <span data-ttu-id="0694a-219">VHRA1Y2TGTA84F00H</span><span class="sxs-lookup"><span data-stu-id="0694a-219">VHRA1Y2TGTA84F00H</span></span> |<span data-ttu-id="0694a-220">Family sedan</span><span class="sxs-lookup"><span data-stu-id="0694a-220">Family Saloon</span></span> |
| <span data-ttu-id="0694a-221">R0JAUHT1L1R3BIKI0</span><span class="sxs-lookup"><span data-stu-id="0694a-221">R0JAUHT1L1R3BIKI0</span></span> |<span data-ttu-id="0694a-222">Sedan</span><span class="sxs-lookup"><span data-stu-id="0694a-222">Sedan</span></span> |
| <span data-ttu-id="0694a-223">9230C202Z60XX84AU</span><span class="sxs-lookup"><span data-stu-id="0694a-223">9230C202Z60XX84AU</span></span> |<span data-ttu-id="0694a-224">Hybrid</span><span class="sxs-lookup"><span data-stu-id="0694a-224">Hybrid</span></span> |
| <span data-ttu-id="0694a-225">T8DNDN5UDCWL7M72H</span><span class="sxs-lookup"><span data-stu-id="0694a-225">T8DNDN5UDCWL7M72H</span></span> |<span data-ttu-id="0694a-226">Family sedan</span><span class="sxs-lookup"><span data-stu-id="0694a-226">Family Saloon</span></span> |
| <span data-ttu-id="0694a-227">4WPYRUZII5YV7YA42</span><span class="sxs-lookup"><span data-stu-id="0694a-227">4WPYRUZII5YV7YA42</span></span> |<span data-ttu-id="0694a-228">Sedan</span><span class="sxs-lookup"><span data-stu-id="0694a-228">Sedan</span></span> |
| <span data-ttu-id="0694a-229">D1ZVY26UV2BFGHZNO</span><span class="sxs-lookup"><span data-stu-id="0694a-229">D1ZVY26UV2BFGHZNO</span></span> |<span data-ttu-id="0694a-230">Hybrid</span><span class="sxs-lookup"><span data-stu-id="0694a-230">Hybrid</span></span> |
| <span data-ttu-id="0694a-231">XUF99EW9OIQOMV7Q7</span><span class="sxs-lookup"><span data-stu-id="0694a-231">XUF99EW9OIQOMV7Q7</span></span> |<span data-ttu-id="0694a-232">Family sedan</span><span class="sxs-lookup"><span data-stu-id="0694a-232">Family Saloon</span></span> |
| <span data-ttu-id="0694a-233">8OMCL3LGI7XNCC21U</span><span class="sxs-lookup"><span data-stu-id="0694a-233">8OMCL3LGI7XNCC21U</span></span> |<span data-ttu-id="0694a-234">Kan konverteras</span><span class="sxs-lookup"><span data-stu-id="0694a-234">Convertible</span></span> |
| <span data-ttu-id="0694a-235">…….</span><span class="sxs-lookup"><span data-stu-id="0694a-235">…….</span></span> | |

### <a name="references"></a><span data-ttu-id="0694a-236">Referenser</span><span class="sxs-lookup"><span data-stu-id="0694a-236">References</span></span>
[<span data-ttu-id="0694a-237">Vehicle telematik Simulator Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="0694a-237">Vehicle Telematics Simulator Visual Studio Solution</span></span>](http://go.microsoft.com/fwlink/?LinkId=717075) 

[<span data-ttu-id="0694a-238">Azure Event Hub</span><span class="sxs-lookup"><span data-stu-id="0694a-238">Azure Event Hub</span></span>](https://azure.microsoft.com/services/event-hubs/)

[<span data-ttu-id="0694a-239">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="0694a-239">Azure Data Factory</span></span>](https://azure.microsoft.com/documentation/learning-paths/data-factory/)

## <a name="ingestion"></a><span data-ttu-id="0694a-240">Införandet</span><span class="sxs-lookup"><span data-stu-id="0694a-240">Ingestion</span></span>
<span data-ttu-id="0694a-241">Kombinationer av Händelsehubbar i Azure Stream Analytics och Data Factory är balanserad tooingest hello vehicle signaler, hello diagnostiska händelser, och realtid och batch-analytics.</span><span class="sxs-lookup"><span data-stu-id="0694a-241">Combinations of Azure Event Hubs, Stream Analytics, and Data Factory are leveraged tooingest hello vehicle signals, hello diagnostic events, and real-time and batch analytics.</span></span> <span data-ttu-id="0694a-242">Alla dessa komponenter skapas och konfigureras som en del av hello lösningsdistribution.</span><span class="sxs-lookup"><span data-stu-id="0694a-242">All these components are created and configured as part of hello solution deployment.</span></span> 

### <a name="real-time-analysis"></a><span data-ttu-id="0694a-243">Analys i realtid</span><span class="sxs-lookup"><span data-stu-id="0694a-243">Real-time analysis</span></span>
<span data-ttu-id="0694a-244">hello händelser som genererats av hello Vehicle telematik Simulator publiceras toohello Event Hub med hello Event Hub SDK.</span><span class="sxs-lookup"><span data-stu-id="0694a-244">hello events generated by hello Vehicle Telematics Simulator are published toohello Event Hub using hello Event Hub SDK.</span></span> <span data-ttu-id="0694a-245">hello Stream Analytics-jobbet en dessa händelser från hello Event Hub och processer hello data i realtid tooanalyze hello vehicle hälsa.</span><span class="sxs-lookup"><span data-stu-id="0694a-245">hello Stream Analytics job ingests these events from hello Event Hub and processes hello data in real time tooanalyze hello vehicle health.</span></span> 

![Event hub instrumentpanelen](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

<span data-ttu-id="0694a-247">*Bild 4 - Event Hub-instrumentpanelen*</span><span class="sxs-lookup"><span data-stu-id="0694a-247">*Figure 4 - Event Hub dashboard*</span></span>

![Strömma Analytics-jobbet databearbetning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 

<span data-ttu-id="0694a-249">*Bild 5 - Stream Analytics-jobbet bearbetning av data*</span><span class="sxs-lookup"><span data-stu-id="0694a-249">*Figure 5 - Stream Analytics job processing data*</span></span>

<span data-ttu-id="0694a-250">hello Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="0694a-250">hello Stream Analytics job;</span></span>

* <span data-ttu-id="0694a-251">en data från hello Event Hub</span><span class="sxs-lookup"><span data-stu-id="0694a-251">ingests data from hello Event Hub</span></span> 
* <span data-ttu-id="0694a-252">Utför en koppling med hello referens toomap hello vehicle VIN toohello motsvarande datamodell</span><span class="sxs-lookup"><span data-stu-id="0694a-252">performs a join with hello reference data toomap hello vehicle VIN toohello corresponding model</span></span> 
* <span data-ttu-id="0694a-253">sparar dem i Azure blob-lagring för omfattande batch analytics.</span><span class="sxs-lookup"><span data-stu-id="0694a-253">persists them into Azure blob storage for rich batch analytics.</span></span> 

<span data-ttu-id="0694a-254">hello följande Stream Analytics-fråga är används toopersist hello data till Azure-blobblagring.</span><span class="sxs-lookup"><span data-stu-id="0694a-254">hello following Stream Analytics query is used toopersist hello data into Azure blob storage.</span></span> 

![Stream Analytics-jobbet frågan för datapåfyllning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

<span data-ttu-id="0694a-256">*Bild 6 - Stream Analytics-jobbet frågan för datapåfyllning*</span><span class="sxs-lookup"><span data-stu-id="0694a-256">*Figure 6 - Stream Analytics job query for data ingestion*</span></span>

### <a name="batch-analysis"></a><span data-ttu-id="0694a-257">Batchanalys</span><span class="sxs-lookup"><span data-stu-id="0694a-257">Batch analysis</span></span>
<span data-ttu-id="0694a-258">Vi också genererar en ytterligare volym av simulerade vehicle signaler och diagnostik datamängden för bättre batch analytics.</span><span class="sxs-lookup"><span data-stu-id="0694a-258">We are also generating an additional volume of simulated vehicle signals and diagnostic dataset for richer batch analytics.</span></span> <span data-ttu-id="0694a-259">Detta är obligatorisk tooensure en bra representativt datavolym för batch-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="0694a-259">This is required tooensure a good representative data volume for batch processing.</span></span> <span data-ttu-id="0694a-260">För detta ändamål använder du en pipeline med namnet ”PrepareSampleDataPipeline” i hello Azure Data Factory arbetsflöde toogenerate ett år kan du se simulerade vehicle signaler och diagnostik dataset.</span><span class="sxs-lookup"><span data-stu-id="0694a-260">For this purpose, we are using a pipeline named "PrepareSampleDataPipeline" in hello Azure Data Factory workflow toogenerate one year's worth of simulated vehicle signals and diagnostic dataset.</span></span> <span data-ttu-id="0694a-261">Klicka på [Data Factory anpassad aktivitet](http://go.microsoft.com/fwlink/?LinkId=717077) toodownload hello Data Factory anpassad DotNet aktivitet Visual Studio-lösning för anpassningar baserat på dina krav.</span><span class="sxs-lookup"><span data-stu-id="0694a-261">Click [Data Factory custom activity](http://go.microsoft.com/fwlink/?LinkId=717077) toodownload hello Data Factory custom DotNet activity Visual Studio solution for customizations based on your requirements.</span></span> 

![Förbereda exempeldata för batchbearbetning arbetsflöde](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

<span data-ttu-id="0694a-263">*Figur 7 – förbereda exempeldata för arbetsflöde för batch-bearbetning*</span><span class="sxs-lookup"><span data-stu-id="0694a-263">*Figure 7 - Prepare Sample data for batch processing workflow*</span></span>

<span data-ttu-id="0694a-264">hello försäljningsförlopp består av en anpassad ADF .net aktivitet, visas här:</span><span class="sxs-lookup"><span data-stu-id="0694a-264">hello pipeline consists of a custom ADF .Net Activity, show here:</span></span>

![PrepareSampleDataPipeline aktivitet](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

<span data-ttu-id="0694a-266">*Figur 8 - PrepareSampleDataPipeline*</span><span class="sxs-lookup"><span data-stu-id="0694a-266">*Figure 8 - PrepareSampleDataPipeline*</span></span>

<span data-ttu-id="0694a-267">När hello pipelinen körs korrekt och ”RawCarEventsTable” dataset markeras ”klar” års värt simulerade vehicle signaler och diagnostiska som data produceras.</span><span class="sxs-lookup"><span data-stu-id="0694a-267">Once hello pipeline executes successfully and "RawCarEventsTable" dataset is marked "Ready", one-year worth of simulated vehicle signals and diagnostic data are produced.</span></span> <span data-ttu-id="0694a-268">Du ser hello följande mapp och fil som skapats i ditt lagringskonto i hello ”connectedcar”-behållaren:</span><span class="sxs-lookup"><span data-stu-id="0694a-268">You see hello following folder and file created in your storage account under hello "connectedcar" container:</span></span>

![PrepareSampleDataPipeline utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

<span data-ttu-id="0694a-270">*Bild 9 - PrepareSampleDataPipeline utdata*</span><span class="sxs-lookup"><span data-stu-id="0694a-270">*Figure 9 - PrepareSampleDataPipeline Output*</span></span>

### <a name="references"></a><span data-ttu-id="0694a-271">Referenser</span><span class="sxs-lookup"><span data-stu-id="0694a-271">References</span></span>
[<span data-ttu-id="0694a-272">Azure Event Hub-SDK för inhämtning av dataströmmar</span><span class="sxs-lookup"><span data-stu-id="0694a-272">Azure Event Hub SDK for stream ingestion</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

<span data-ttu-id="0694a-273">[Azure Data Factory data movement funktioner](../data-factory/data-factory-data-movement-activities.md)
[DotNet-aktivitet för Azure Data Factory](../data-factory/data-factory-use-custom-activities.md)</span><span class="sxs-lookup"><span data-stu-id="0694a-273">[Azure Data Factory data movement capabilities](../data-factory/data-factory-data-movement-activities.md)
[Azure Data Factory DotNet Activity](../data-factory/data-factory-use-custom-activities.md)</span></span>

[<span data-ttu-id="0694a-274">Azure Data Factory DotNet aktivitet visual studio-lösning för att förbereda exempeldata</span><span class="sxs-lookup"><span data-stu-id="0694a-274">Azure Data Factory DotNet activity visual studio solution for preparing sample data</span></span>](http://go.microsoft.com/fwlink/?LinkId=717077) 

## <a name="partition-hello-dataset"></a><span data-ttu-id="0694a-275">Partitionen hello dataset</span><span class="sxs-lookup"><span data-stu-id="0694a-275">Partition hello dataset</span></span>
<span data-ttu-id="0694a-276">hello rådata halvstrukturerade vehicle signaler och diagnostik dataset partitioneras i hello data förberedelsen till formatet år/månad.</span><span class="sxs-lookup"><span data-stu-id="0694a-276">hello raw semi-structured vehicle signals and diagnostic dataset are partitioned in hello data preparation step into a YEAR/MONTH format.</span></span> <span data-ttu-id="0694a-277">Den här partitionering befordrar mer effektiva frågor och skalbar långsiktig lagring genom att aktivera fel-over från en blob konto toohello bredvid hello första kontot fylls.</span><span class="sxs-lookup"><span data-stu-id="0694a-277">This partitioning promotes more efficient querying and scalable long-term storage by enabling fault-over from one blob account toohello next as hello first account fills up.</span></span> 

>[!NOTE] 
><span data-ttu-id="0694a-278">Det här steget i hello lösningen är tillämpliga endast toobatch bearbetning.</span><span class="sxs-lookup"><span data-stu-id="0694a-278">This step in hello solution is applicable only toobatch processing.</span></span>

<span data-ttu-id="0694a-279">Indata och utdata hantering av data:</span><span class="sxs-lookup"><span data-stu-id="0694a-279">Input and output data data management:</span></span>

* <span data-ttu-id="0694a-280">Hej **utdata** (märkta *PartitionedCarEventsTable*) toobe lagras under en längre tidsperiod som hello grundläggande / ”rawest” form av data i hello kundens ”Data Lake”.</span><span class="sxs-lookup"><span data-stu-id="0694a-280">hello **output data** (labeled *PartitionedCarEventsTable*) is toobe kept for a long period of time as hello foundational/"rawest" form of data in hello customer's "Data Lake".</span></span> 
* <span data-ttu-id="0694a-281">Hej **indata** toothis pipeline skulle normalt ignoreras eftersom hello utdata har fullständig återgivning toohello indata - lagras bara (partitionerad) bättre för senare användning.</span><span class="sxs-lookup"><span data-stu-id="0694a-281">hello **input data** toothis pipeline would typically be discarded as hello output data has full fidelity toohello input - it's just stored (partitioned) better for subsequent use.</span></span>

![Partitionen bil händelser arbetsflöde](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)

<span data-ttu-id="0694a-283">*Figur 10 – Partition bil händelser arbetsflöde*</span><span class="sxs-lookup"><span data-stu-id="0694a-283">*Figure 10 – Partition Car Events workflow*</span></span>

<span data-ttu-id="0694a-284">hello rådata är partitionerad med hjälp av en HDInsight Hive-aktivitet i ”PartitionCarEventsPipeline”.</span><span class="sxs-lookup"><span data-stu-id="0694a-284">hello raw data is partitioned using a Hive HDInsight activity in "PartitionCarEventsPipeline".</span></span> <span data-ttu-id="0694a-285">hello exempeldata genereras i steg 1 för ett år har partitionerats med år/månad.</span><span class="sxs-lookup"><span data-stu-id="0694a-285">hello sample data generated in step 1 for a year is partitioned by YEAR/MONTH.</span></span> <span data-ttu-id="0694a-286">hello partitioner är används toogenerate vehicle signaler och diagnostikdata för varje månad (totalt 12 partitioner) för ett år.</span><span class="sxs-lookup"><span data-stu-id="0694a-286">hello partitions are used toogenerate vehicle signals and diagnostic data for each month (total 12 partitions) of a year.</span></span> 

![PartitionCarEventsPipeline aktivitet](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)

<span data-ttu-id="0694a-288">*Figur 11 - PartitionCarEventsPipeline*</span><span class="sxs-lookup"><span data-stu-id="0694a-288">*Figure 11 - PartitionCarEventsPipeline*</span></span>

<span data-ttu-id="0694a-289">***PartitionConnectedCarEvents Hive-skript***</span><span class="sxs-lookup"><span data-stu-id="0694a-289">***PartitionConnectedCarEvents Hive Script***</span></span>

<span data-ttu-id="0694a-290">hello följande Hive-skript med namnet ”partitioncarevents.hql” används för partitionering och finns i hello ”\demo\src\connectedcar\scripts” mapp för hello hämtade zip.</span><span class="sxs-lookup"><span data-stu-id="0694a-290">hello following Hive script, named "partitioncarevents.hql", is used for partitioning and is located in hello "\demo\src\connectedcar\scripts" folder of hello downloaded zip.</span></span> 
    
    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string,
                YearNo                             int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

<span data-ttu-id="0694a-291">När hello pipeline har körts, finns hello följande partitioner som skapas i ditt lagringskonto i hello ”connectedcar”-behållaren.</span><span class="sxs-lookup"><span data-stu-id="0694a-291">Once hello pipeline is executed successfully, you see hello following partitions generated in your storage account under hello "connectedcar" container.</span></span>

![Partitionerade utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

<span data-ttu-id="0694a-293">*Figur 12 - partitionerade utdata*</span><span class="sxs-lookup"><span data-stu-id="0694a-293">*Figure 12 - Partitioned Output*</span></span>

<span data-ttu-id="0694a-294">hello data optimeras nu, hantera och redo för vidare bearbetning toogain omfattande batch insikter.</span><span class="sxs-lookup"><span data-stu-id="0694a-294">hello data is now optimized, is more manageable and ready for further processing toogain rich batch insights.</span></span> 

## <a name="data-analysis"></a><span data-ttu-id="0694a-295">Dataanalys</span><span class="sxs-lookup"><span data-stu-id="0694a-295">Data Analysis</span></span>
<span data-ttu-id="0694a-296">I det här avsnittet visas hur toocombine Azure Stream Analytics, Azure Machine Learning, Azure Data Factory och Azure HDInsight för omfattande avancerade analyser på vehicle hälsa och köra vanor.</span><span class="sxs-lookup"><span data-stu-id="0694a-296">In this section, you see how toocombine Azure Stream Analytics, Azure Machine Learning, Azure Data Factory, and Azure HDInsight for rich advanced analytics on vehicle health and driving habits.</span></span> <span data-ttu-id="0694a-297">Det finns tre underavsnitt här:</span><span class="sxs-lookup"><span data-stu-id="0694a-297">There are three subsections here:</span></span>

1. <span data-ttu-id="0694a-298">**Maskininlärning**: detta avsnitt innehåller information om hello avvikelseidentifiering identifiering experiment som används i den här lösningen toopredict fordon kräver Underhåll underhåll och fordon som kräver återställning av på grund av problem med toosafety.</span><span class="sxs-lookup"><span data-stu-id="0694a-298">**Machine Learning**: This subsection contains information on hello anomaly detection experiment that we have used in this solution toopredict vehicles requiring servicing maintenance and vehicles requiring recalls due toosafety issues.</span></span>
2. <span data-ttu-id="0694a-299">**Realtid analysis**: detta avsnitt innehåller information om hello analys i realtid med hello Stream Analytics-frågespråket och operationalizing hello maskininlärning experiment i realtid med ett anpassat program.</span><span class="sxs-lookup"><span data-stu-id="0694a-299">**Real-time analysis**: This subsection contains information regarding hello real-time analytics using hello Stream Analytics Query Language and operationalizing hello machine learning experiment in real time using a custom application.</span></span>
3. <span data-ttu-id="0694a-300">**Batch-analys**: detta avsnitt innehåller information om hello omvandla och bearbetning av hello batch data med Azure HDInsight och Azure Machine Learning operationalized av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0694a-300">**Batch analysis**: This subsection contains information regarding hello transforming and processing of hello batch data using Azure HDInsight and Azure Machine Learning operationalized by Azure Data Factory.</span></span>

### <a name="machine-learning"></a><span data-ttu-id="0694a-301">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="0694a-301">Machine Learning</span></span>
<span data-ttu-id="0694a-302">Vårt mål här är toopredict hello fordon som kräver underhåll eller återkallas baserat på vissa hed statistik.</span><span class="sxs-lookup"><span data-stu-id="0694a-302">Our goal here is toopredict hello vehicles that require maintenance or recall based on certain heath statistics.</span></span> <span data-ttu-id="0694a-303">Vi göra hello följande antaganden</span><span class="sxs-lookup"><span data-stu-id="0694a-303">We make hello following assumptions</span></span>

* <span data-ttu-id="0694a-304">Om en av hello följande tre villkor är uppfyllda, hello fordon kräver **servicing Underhåll**:</span><span class="sxs-lookup"><span data-stu-id="0694a-304">If one of hello following three conditions are true, hello vehicles require **servicing maintenance**:</span></span>
  
  * <span data-ttu-id="0694a-305">Däck trycket är låg</span><span class="sxs-lookup"><span data-stu-id="0694a-305">Tire pressure is low</span></span>
  * <span data-ttu-id="0694a-306">Motorn olja nivån är låg</span><span class="sxs-lookup"><span data-stu-id="0694a-306">Engine oil level is low</span></span>
  * <span data-ttu-id="0694a-307">Motorn är hög</span><span class="sxs-lookup"><span data-stu-id="0694a-307">Engine temperature is high</span></span>
* <span data-ttu-id="0694a-308">Om en av hello följande villkor är uppfyllda, hello fordon kan ha en **säkerhet problemet** och kräver **återkallning**:</span><span class="sxs-lookup"><span data-stu-id="0694a-308">If one of hello following conditions are true, hello vehicles may have a **safety issue** and require **recall**:</span></span>
  
  * <span data-ttu-id="0694a-309">Motorn temperatur är hög men utanför temperatur är låg</span><span class="sxs-lookup"><span data-stu-id="0694a-309">Engine temperature is high but outside temperature is low</span></span>
  * <span data-ttu-id="0694a-310">Motorn temperatur är låg men utanför temperatur är hög</span><span class="sxs-lookup"><span data-stu-id="0694a-310">Engine temperature is low but outside temperature is high</span></span>

<span data-ttu-id="0694a-311">Baserat på hello tidigare krav har vi skapat två separata modeller toodetect avvikelser, en för identifiering av vehicle underhåll och en för vehicle återkallning identifiering.</span><span class="sxs-lookup"><span data-stu-id="0694a-311">Based on hello previous requirements, we have created two separate models toodetect anomalies, one for vehicle maintenance detection, and one for vehicle recall detection.</span></span> <span data-ttu-id="0694a-312">I båda dessa modeller används hello inbyggda huvudnamn komponenten analys (PCA) algoritm för avvikelseidentifiering.</span><span class="sxs-lookup"><span data-stu-id="0694a-312">In both these models, hello built-in Principal Component Analysis (PCA) algorithm is used for anomaly detection.</span></span> 

<span data-ttu-id="0694a-313">**Underhåll identifiering av modellen**</span><span class="sxs-lookup"><span data-stu-id="0694a-313">**Maintenance detection model**</span></span>

<span data-ttu-id="0694a-314">Om en av tre indikatorer däck trycket, motorolja eller motorn temperatur - uppfyller sitt respektive tillstånd, rapporterar avvikelser hello Underhåll identifiering av modellen.</span><span class="sxs-lookup"><span data-stu-id="0694a-314">If one of three indicators - tire pressure, engine oil, or engine temperature - satisfies its respective condition, hello maintenance detection model reports an anomaly.</span></span> <span data-ttu-id="0694a-315">Därför kan behöver vi bara tooconsider dessa tre variabler i att skapa hello modellen.</span><span class="sxs-lookup"><span data-stu-id="0694a-315">As a result, we only need tooconsider these three variables in building hello model.</span></span> <span data-ttu-id="0694a-316">I vårt experiment i Azure Machine Learning vi först använda en **Välj kolumner i datauppsättning** modulen tooextract dessa tre variabler.</span><span class="sxs-lookup"><span data-stu-id="0694a-316">In our experiment in Azure Machine Learning, we first use a **Select Columns in Dataset** module tooextract these three variables.</span></span> <span data-ttu-id="0694a-317">Vi använder bredvid hello PCA-baserad avvikelseidentifiering identifiering modulen toobuild hello avvikelseidentifiering identifiering modell.</span><span class="sxs-lookup"><span data-stu-id="0694a-317">Next we use hello PCA-based anomaly detection module toobuild hello anomaly detection model.</span></span> 

<span data-ttu-id="0694a-318">Huvudnamn komponenten analys (PCA) är en etablerad teknik i machine learning som kan vara tillämpade toofeature markeringen, klassificering och avvikelseidentifiering identifiering.</span><span class="sxs-lookup"><span data-stu-id="0694a-318">Principal Component Analysis (PCA) is an established technique in machine learning that can be applied toofeature selection, classification, and anomaly detection.</span></span> <span data-ttu-id="0694a-319">PCA konverterar en uppsättning fallet med eventuellt korrelerade variabler i en uppsättning värden som kallas huvudkomponenter.</span><span class="sxs-lookup"><span data-stu-id="0694a-319">PCA converts a set of case containing possibly correlated variables, into a set of values called principal components.</span></span> <span data-ttu-id="0694a-320">hello viktiga uppfattning om PCA-baserad modellering är tooproject data till ett lägre endimensionell utrymme så att funktioner och avvikelser mer lätt kan identifieras.</span><span class="sxs-lookup"><span data-stu-id="0694a-320">hello key idea of PCA-based modeling is tooproject data onto a lower-dimensional space so that features and anomalies can be more easily identified.</span></span>

<span data-ttu-id="0694a-321">För varje ny indata hello för identifiering av modellen, hello avvikelseidentifiering detektor först beräknar dess projektion på hello eigenvectors och sedan beräknar hello normaliserade återuppbyggnad fel.</span><span class="sxs-lookup"><span data-stu-id="0694a-321">For each new input too hello detection model, hello anomaly detector first computes its projection on hello eigenvectors, and then computes hello normalized reconstruction error.</span></span> <span data-ttu-id="0694a-322">Felet normaliserade är hello avvikelseidentifiering poäng.</span><span class="sxs-lookup"><span data-stu-id="0694a-322">This normalized error is hello anomaly score.</span></span> <span data-ttu-id="0694a-323">hello högre hello fel hello mer avvikande hello-instansen är.</span><span class="sxs-lookup"><span data-stu-id="0694a-323">hello higher hello error, hello more anomalous hello instance is.</span></span> 

<span data-ttu-id="0694a-324">Hello Underhåll identifiering problem kan varje post ses som en punkt i 3-dimensionell kan definieras av däck tryck, motorolja och motorn temperatur koordinater.</span><span class="sxs-lookup"><span data-stu-id="0694a-324">In hello maintenance detection problem, each record can be considered as a point in a 3-dimensional space defined by tire pressure, engine oil, and engine temperature coordinates.</span></span> <span data-ttu-id="0694a-325">toocapture dessa avvikelser kan vi projicera hello ursprungliga data i hello 3-dimensionell utrymme på 2-dimensionell kan använda PCA.</span><span class="sxs-lookup"><span data-stu-id="0694a-325">toocapture these anomalies, we can project hello original data in hello 3-dimensional space onto a 2-dimensional space using PCA.</span></span> <span data-ttu-id="0694a-326">Därför som vi hello parametern antal komponenter toouse i PCA toobe 2.</span><span class="sxs-lookup"><span data-stu-id="0694a-326">Thus, we set hello parameter Number of components toouse in PCA toobe 2.</span></span> <span data-ttu-id="0694a-327">Den här parametern spelar en viktig roll vid tillämpning av PCA-baserad avvikelseidentifiering.</span><span class="sxs-lookup"><span data-stu-id="0694a-327">This parameter plays an important role in applying PCA-based anomaly detection.</span></span> <span data-ttu-id="0694a-328">Efter projicera data med hjälp av PCA, kan vi identifiera dessa avvikelser enklare.</span><span class="sxs-lookup"><span data-stu-id="0694a-328">After projecting data using PCA, we can identify these anomalies more easily.</span></span>

<span data-ttu-id="0694a-329">**Återkalla avvikelseidentifiering identifiering av modellen** i hello återkallning avvikelseidentifiering identifiering av modellen vi använda hello Välj kolumner i datauppsättning och PCA-baserad avvikelseidentifiering identifiering moduler på ett liknande sätt.</span><span class="sxs-lookup"><span data-stu-id="0694a-329">**Recall anomaly detection model** In hello recall anomaly detection model, we use hello Select Columns in Dataset and PCA-based anomaly detection modules in a similar way.</span></span> <span data-ttu-id="0694a-330">Mer specifikt vi först extrahera tre variabler - motorn temperatur, utanför temperatur och hastighet - med hello **Välj kolumner i datauppsättning** modul.</span><span class="sxs-lookup"><span data-stu-id="0694a-330">Specifically, we first extract three variables - engine temperature, outside temperature, and speed - using hello **Select Columns in Dataset** module.</span></span> <span data-ttu-id="0694a-331">Vi också innehålla hello hastighet variabeln eftersom hello motorn temperatur är vanligtvis korrelerade toohello hastighet.</span><span class="sxs-lookup"><span data-stu-id="0694a-331">We also include hello speed variable since hello engine temperature typically is correlated toohello speed.</span></span> <span data-ttu-id="0694a-332">Vi använder bredvid PCA-baserad avvikelseidentifiering modulen tooproject hello avkänningsdata från hello 3-dimensionell utrymme till en 2-dimensionell.</span><span class="sxs-lookup"><span data-stu-id="0694a-332">Next we use PCA-based anomaly detection module tooproject hello data from hello 3-dimensional space onto a 2-dimensional space.</span></span> <span data-ttu-id="0694a-333">hello återkallning villkor är uppfyllda och hello vehicle kräver återkallning när motorn temperatur- och utanför temperatur hög negativt korrelerade.</span><span class="sxs-lookup"><span data-stu-id="0694a-333">hello recall criteria are satisfied and so hello vehicle requires recall when engine temperature and outside temperature are highly negatively correlated.</span></span> <span data-ttu-id="0694a-334">Vi använder PCA-baserad algoritm för avvikelseidentifiering kan du avbilda hello avvikelser när du utför PCA.</span><span class="sxs-lookup"><span data-stu-id="0694a-334">Using PCA-based anomaly detection algorithm, we can capture hello anomalies after performing PCA.</span></span> 

<span data-ttu-id="0694a-335">Vid inlärning antingen modellen måste toouse normal data, som inte kräver underhåll eller återkallas som hello indata tootrain hello PCA-baserad avvikelseidentifiering identifiering av modellen.</span><span class="sxs-lookup"><span data-stu-id="0694a-335">When training either model, we need toouse normal data, which does not require maintenance or recall as hello input data tootrain hello PCA-based anomaly detection model.</span></span> <span data-ttu-id="0694a-336">I hello bedömningen experiment, använder vi hello tränats avvikelseidentifiering identifiering av modellen toodetect huruvida hello vehicle kräver underhåll eller återkallas.</span><span class="sxs-lookup"><span data-stu-id="0694a-336">In hello scoring experiment, we use hello trained anomaly detection model toodetect whether or not hello vehicle requires maintenance or recall.</span></span> 

### <a name="real-time-analysis"></a><span data-ttu-id="0694a-337">Analys i realtid</span><span class="sxs-lookup"><span data-stu-id="0694a-337">Real-time analysis</span></span>
<span data-ttu-id="0694a-338">följande SQL-frågan i Stream Analytics hello används tooget hello medelvärdet av alla hello viktiga vehicle parametrar som hastigheten, bränslenivå, motorn temperatur, mätarställning, däck tryck, motorn olja nivå och andra.</span><span class="sxs-lookup"><span data-stu-id="0694a-338">hello following Stream Analytics SQL Query is used tooget hello average of all hello important vehicle parameters like vehicle speed, fuel level, engine temperature, odometer reading, tire pressure, engine oil level, and others.</span></span> <span data-ttu-id="0694a-339">hello medelvärden är används toodetect avvikelser utfärda aviseringar och fastställa hello övergripande hälsa villkor fordon drivas på ett visst område och sedan korrelera toodemographics.</span><span class="sxs-lookup"><span data-stu-id="0694a-339">hello averages are used toodetect anomalies, issue alerts, and determine hello overall health conditions of vehicles operated in specific region and then correlate it toodemographics.</span></span> 

![Stream Analytics-fråga för realtidsbearbetning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

<span data-ttu-id="0694a-341">*Figur 13-Stream Analytics-fråga för realtidsbearbetning*</span><span class="sxs-lookup"><span data-stu-id="0694a-341">*Figure 13 – Stream Analytics query for real-time processing*</span></span>

<span data-ttu-id="0694a-342">Alla hello medelvärden beräknas under en TumblingWindow 3 sekunder.</span><span class="sxs-lookup"><span data-stu-id="0694a-342">All hello averages are calculated over a 3-second TumblingWindow.</span></span> <span data-ttu-id="0694a-343">Vi använder TubmlingWindow i det här fallet eftersom vi kräver inte överlappar och sammanhängande intervall.</span><span class="sxs-lookup"><span data-stu-id="0694a-343">We are using TubmlingWindow in this case since we require non-overlapping and contiguous time intervals.</span></span> 

<span data-ttu-id="0694a-344">toolearn mer om alla hello ”fönsterhantering” funktioner i Azure Stream Analytics, klickar du på [fönsterhantering (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).</span><span class="sxs-lookup"><span data-stu-id="0694a-344">toolearn more about all hello "Windowing" capabilities in Azure Stream Analytics, click [Windowing (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).</span></span>

<span data-ttu-id="0694a-345">**Realtid förutsägelse**</span><span class="sxs-lookup"><span data-stu-id="0694a-345">**Real-time prediction**</span></span>

<span data-ttu-id="0694a-346">Ett program ingår som en del av hello lösning toooperationalize hello machine learning-modellen i verkliga tid.</span><span class="sxs-lookup"><span data-stu-id="0694a-346">An application is included as part of hello solution toooperationalize hello machine learning model in real time.</span></span> <span data-ttu-id="0694a-347">Det här programmet som kallas ”RealTimeDashboardApp” har skapats och konfigurerats som en del av hello lösningsdistribution.</span><span class="sxs-lookup"><span data-stu-id="0694a-347">This application called “RealTimeDashboardApp” is created and configured as part of hello solution deployment.</span></span> <span data-ttu-id="0694a-348">hello program utför hello följande:</span><span class="sxs-lookup"><span data-stu-id="0694a-348">hello application performs hello following:</span></span>

1. <span data-ttu-id="0694a-349">Lyssnar tooan Event Hub-instans där Stream Analytics publicerar hello händelser i ett mönster kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="0694a-349">Listens tooan Event Hub instance where Stream Analytics is publishing hello events in a pattern continuously.</span></span> <span data-ttu-id="0694a-350">![Stream Analytics-fråga för att publicera hello data](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *figur 14 – Stream Analytics-fråga för att publicera hello data tooan utdata Event Hub-instans*</span><span class="sxs-lookup"><span data-stu-id="0694a-350">![Stream Analytics query for publishing hello data](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *Figure 14 – Stream Analytics query for publishing hello data tooan output Event Hub instance*</span></span> 
2. <span data-ttu-id="0694a-351">För varje händelse som tar emot det här programmet:</span><span class="sxs-lookup"><span data-stu-id="0694a-351">For every event that this application receives:</span></span> 
   
   * <span data-ttu-id="0694a-352">Processer hello data med hjälp av Machine Learning-svar på begäranden bedömningen (RR) slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="0694a-352">Processes hello data using Machine Learning Request-Response Scoring (RRS) endpoint.</span></span> <span data-ttu-id="0694a-353">hello Resursposter endpoint publiceras automatiskt som en del av hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="0694a-353">hello RRS endpoint is automatically published as part of hello deployment.</span></span>
   * <span data-ttu-id="0694a-354">hello Resursposter utdata är publicerade tooa Power BI dataset med hello push API: er.</span><span class="sxs-lookup"><span data-stu-id="0694a-354">hello RRS output is published tooa Power BI dataset using hello push APIs.</span></span>

<span data-ttu-id="0694a-355">Det här mönstret är också tillämpliga tooscenarios som du vill toointegrate ett Line of Business (LoB)-program med hello analys i realtid flödet för scenarier, till exempel aviseringar, meddelanden och meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0694a-355">This pattern is also applicable tooscenarios in which you want toointegrate a Line of Business (LoB) application with hello real-time analytics flow, for scenarios such as alerts, notifications, and messaging.</span></span>

<span data-ttu-id="0694a-356">Klicka på [RealtimeDashboardApp download](http://go.microsoft.com/fwlink/?LinkId=717078) toodownload hello RealtimeDashboardApp Visual Studio-lösning för anpassningar.</span><span class="sxs-lookup"><span data-stu-id="0694a-356">Click [RealtimeDashboardApp download](http://go.microsoft.com/fwlink/?LinkId=717078) toodownload hello RealtimeDashboardApp Visual Studio solution for customizations.</span></span> 

<span data-ttu-id="0694a-357">**tooexecute hello realtid instrumentpanelen för program**</span><span class="sxs-lookup"><span data-stu-id="0694a-357">**tooexecute hello Real-time Dashboard Application**</span></span>
1. <span data-ttu-id="0694a-358">Extrahera och spara lokalt ![RealtimeDashboardApp mappen](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *bild 16 – RealtimeDashboardApp mapp*</span><span class="sxs-lookup"><span data-stu-id="0694a-358">Extract and save locally ![RealtimeDashboardApp folder](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *Figure 16 – RealtimeDashboardApp folder*</span></span>  
2. <span data-ttu-id="0694a-359">Köra programmet hello RealtimeDashboardApp.exe</span><span class="sxs-lookup"><span data-stu-id="0694a-359">Execute hello application RealtimeDashboardApp.exe</span></span>
3. <span data-ttu-id="0694a-360">Ange giltiga autentiseringsuppgifter för Power BI, logga in och klicka på Acceptera</span><span class="sxs-lookup"><span data-stu-id="0694a-360">Provide valid Power BI credentials, sign in and click Accept</span></span> ![Realtid instrumentpanelen app inloggning tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) ![Realtid instrumentpanelsapp Slutför inloggningen tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

<span data-ttu-id="0694a-363">*Bild 17 – RealtimeDashboardApp: Logga in tooPower BI*</span><span class="sxs-lookup"><span data-stu-id="0694a-363">*Figure 17 – RealtimeDashboardApp: Sign-in tooPower BI*</span></span>

>[!NOTE] 
><span data-ttu-id="0694a-364">Om du vill tooflush hello Power BI dataset, kör du hello RealtimeDashboardApp med hello ”flushdata”-parametern:</span><span class="sxs-lookup"><span data-stu-id="0694a-364">If you want tooflush hello Power BI dataset, execute hello RealtimeDashboardApp with hello "flushdata" parameter:</span></span> 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a><span data-ttu-id="0694a-365">Batchanalys</span><span class="sxs-lookup"><span data-stu-id="0694a-365">Batch analysis</span></span>
<span data-ttu-id="0694a-366">hello målet är tooshow hur Contoso motorer använder hello Azure compute funktioner tooharness stordata toogain omfattande insikter om körning mönster, användning beteende och vehicle hälsa.</span><span class="sxs-lookup"><span data-stu-id="0694a-366">hello goal here is tooshow how Contoso Motors utilizes hello Azure compute capabilities tooharness big data toogain rich insights on driving pattern, usage behavior, and vehicle health.</span></span> <span data-ttu-id="0694a-367">Detta gör det möjligt att:</span><span class="sxs-lookup"><span data-stu-id="0694a-367">This makes it possible to:</span></span>

* <span data-ttu-id="0694a-368">Förbättra kundupplevelsen hello och gör den billigare genom att ge insikter på Driver vanor och bränsle effektivt intresseväckande beteenden</span><span class="sxs-lookup"><span data-stu-id="0694a-368">Improve hello customer experience and make it cheaper by providing insights on driving habits and fuel efficient driving behaviors</span></span>
* <span data-ttu-id="0694a-369">Lär dig att kunder och deras intresseväckande patters toogovern affärsbeslut och ange hello bäst i klassen produkter och tjänster</span><span class="sxs-lookup"><span data-stu-id="0694a-369">Learn proactively about customers and their driving patters toogovern business decisions and provide hello best in class products & services</span></span>

<span data-ttu-id="0694a-370">I den här lösningen utvecklar vi hello följande mått:</span><span class="sxs-lookup"><span data-stu-id="0694a-370">In this solution, we are targeting hello following metrics:</span></span>

1. <span data-ttu-id="0694a-371">**Styr beteendet aggressivt**: identifierar hello trend hello modeller, platser, intresseväckande villkor och tiden för hello år toogain insikter om aggressivt intresseväckande mönster.</span><span class="sxs-lookup"><span data-stu-id="0694a-371">**Aggressive driving behavior**: Identifies hello trend of hello models, locations, driving conditions, and time of hello year toogain insights on aggressive driving patterns.</span></span> <span data-ttu-id="0694a-372">Contoso motorer kan använda dessa insikter för marknadsföringskampanjer, driver nya anpassade funktioner och användningsbaserad insurance.</span><span class="sxs-lookup"><span data-stu-id="0694a-372">Contoso Motors can use these insights for marketing campaigns, driving new personalized features and usage-based insurance.</span></span>
2. <span data-ttu-id="0694a-373">**Bränsle effektivt intresseväckande beteende**: identifierar hello trend hello modeller, platser, intresseväckande villkor och tiden för hello år toogain insikter om bränsle effektivt intresseväckande mönster.</span><span class="sxs-lookup"><span data-stu-id="0694a-373">**Fuel efficient driving behavior**: Identifies hello trend of hello models, locations, driving conditions, and time of hello year toogain insights on fuel efficient driving patterns.</span></span> <span data-ttu-id="0694a-374">Contoso motorer kan använda dessa insikter för marknadsföringskampanjer, köra nya funktioner och proaktiv reporting toohello drivrutiner för effektiv och miljö eget intresseväckande vanor.</span><span class="sxs-lookup"><span data-stu-id="0694a-374">Contoso Motors can use these insights for marketing campaigns, driving new features and proactive reporting toohello drivers for cost effective and environment friendly driving habits.</span></span> 
3. <span data-ttu-id="0694a-375">**Återkalla modeller**: identifierar modeller som kräver återställning av operationalizing hello avvikelseidentifiering identifiering machine learning-experiment</span><span class="sxs-lookup"><span data-stu-id="0694a-375">**Recall models**: Identifies models requiring recalls by operationalizing hello anomaly detection machine learning experiment</span></span>

<span data-ttu-id="0694a-376">Nu ska vi titta hello detaljer om var och en av de här måtten</span><span class="sxs-lookup"><span data-stu-id="0694a-376">Let's look into hello details of each of these metrics,</span></span>

<span data-ttu-id="0694a-377">**Aggressiv intresseväckande mönster**</span><span class="sxs-lookup"><span data-stu-id="0694a-377">**Aggressive driving pattern**</span></span>

<span data-ttu-id="0694a-378">hello partitioneras vehicle signaler och diagnostikdata bearbetas i pipeline-hello med namnet ”AggresiveDrivingPatternPipeline” med hjälp av Hive toodetermine hello modeller, plats, vehicle, körning villkor och andra parametrar som uppvisar aggressivt intresseväckande mönster.</span><span class="sxs-lookup"><span data-stu-id="0694a-378">hello partitioned vehicle signals and diagnostic data are processed in hello pipeline named "AggresiveDrivingPatternPipeline" using Hive toodetermine hello models, location, vehicle, driving conditions, and other parameters that exhibits aggressive driving pattern.</span></span>

<span data-ttu-id="0694a-379">![Aggressiv intresseväckande mönster arbetsflöde](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*bild 18 – aggressiv intresseväckande mönster-arbetsflöde*</span><span class="sxs-lookup"><span data-stu-id="0694a-379">![Aggressive driving pattern workflow](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*Figure 18 – Aggressive driving pattern workflow*</span></span>


<span data-ttu-id="0694a-380">***Aggressiv intresseväckande mönster Hive-fråga***</span><span class="sxs-lookup"><span data-stu-id="0694a-380">***Aggressive driving pattern Hive query***</span></span>

<span data-ttu-id="0694a-381">hello Hive-skript som heter ”aggresivedriving.hql” används för att analysera aggressivt intresseväckande villkoret mönster finns på ”\demo\src\connectedcar\scripts” hello hämtade zip mapp.</span><span class="sxs-lookup"><span data-stu-id="0694a-381">hello Hive script named "aggresivedriving.hql" used for analyzing aggressive driving condition pattern is located at "\demo\src\connectedcar\scripts" folder of hello downloaded zip.</span></span> 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                   vin                         string, 
                model                        string,
                timestamp                    string,
                city                        string,
                speed                          string,
                transmission_gear_position    string,
                brake_pedal_status            string,
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'


<span data-ttu-id="0694a-382">Den använder hello kombination av fordon överföring växeln position, bromsar cyklar status och hastighet toodetect reckless/aggressivt körning beteenden, baserat på bromsar mönster med hög hastighet.</span><span class="sxs-lookup"><span data-stu-id="0694a-382">It uses hello combination of vehicle's transmission gear position, brake pedal status, and speed toodetect reckless/aggressive driving behavior based on braking pattern at high speed.</span></span> 

<span data-ttu-id="0694a-383">När hello pipeline har körts, finns hello följande partitioner som skapas i ditt lagringskonto i hello ”connectedcar”-behållaren.</span><span class="sxs-lookup"><span data-stu-id="0694a-383">Once hello pipeline is executed successfully, you see hello following partitions generated in your storage account under hello "connectedcar" container.</span></span>

![AggressiveDrivingPatternPipeline utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 

<span data-ttu-id="0694a-385">*Bild 19 – AggressiveDrivingPatternPipeline utdata*</span><span class="sxs-lookup"><span data-stu-id="0694a-385">*Figure 19 – AggressiveDrivingPatternPipeline output*</span></span>

<span data-ttu-id="0694a-386">**Bränsle effektivt intresseväckande mönster**</span><span class="sxs-lookup"><span data-stu-id="0694a-386">**Fuel efficient driving pattern**</span></span>

<span data-ttu-id="0694a-387">hello partitionerad vehicle signaler och diagnostikdata bearbetas i pipeline-hello med namnet ”FuelEfficientDrivingPatternPipeline”.</span><span class="sxs-lookup"><span data-stu-id="0694a-387">hello partitioned vehicle signals and diagnostic data are processed in hello pipeline named "FuelEfficientDrivingPatternPipeline".</span></span> <span data-ttu-id="0694a-388">Hive är används toodetermine hello modeller, plats, vehicle, intresseväckande villkor och andra egenskaper som uppvisar bränsle effektivt intresseväckande mönster.</span><span class="sxs-lookup"><span data-stu-id="0694a-388">Hive is used toodetermine hello models, location, vehicle, driving conditions, and other properties that exhibit fuel efficient driving pattern.</span></span>

![Bränsleeffektiva intresseväckande mönster](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

<span data-ttu-id="0694a-390">*Figur 20 – bränsleeffektiva intresseväckande mönster-arbetsflöde*</span><span class="sxs-lookup"><span data-stu-id="0694a-390">*Figure 20 – Fuel-efficient driving pattern workflow*</span></span>

<span data-ttu-id="0694a-391">***Bränsle effektivt intresseväckande mönster Hive-fråga***</span><span class="sxs-lookup"><span data-stu-id="0694a-391">***Fuel efficient driving pattern Hive query***</span></span>

<span data-ttu-id="0694a-392">hello Hive-skript som heter ”fuelefficientdriving.hql” används för att analysera aggressivt intresseväckande villkoret mönster finns på ”\demo\src\connectedcar\scripts” hello hämtade zip mapp.</span><span class="sxs-lookup"><span data-stu-id="0694a-392">hello Hive script named "fuelefficientdriving.hql" used for analyzing aggressive driving condition pattern is located at "\demo\src\connectedcar\scripts" folder of hello downloaded zip.</span></span> 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                   vin                         string, 
                model                        string,
                   city                        string,
                speed                          string,
                transmission_gear_position    string,                
                brake_pedal_status            string,            
                accelerator_pedal_position    string,                             
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


<span data-ttu-id="0694a-393">Den använder hello kombination av fordon överföring växeln position, bromsar cyklar status, hastighet och accelerator cyklar position toodetect bränsle effektivt intresseväckande beteende baserat på acceleration, bromsar och processorhastighet mönster.</span><span class="sxs-lookup"><span data-stu-id="0694a-393">It uses hello combination of vehicle's transmission gear position, brake pedal status, speed, and accelerator pedal position toodetect fuel efficient driving behavior based on acceleration, braking, and speed patterns.</span></span> 

<span data-ttu-id="0694a-394">När hello pipeline har körts, finns hello följande partitioner som skapas i ditt lagringskonto i hello ”connectedcar”-behållaren.</span><span class="sxs-lookup"><span data-stu-id="0694a-394">Once hello pipeline is executed successfully, you see hello following partitions generated in your storage account under hello "connectedcar" container.</span></span>

![FuelEfficientDrivingPatternPipeline utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

<span data-ttu-id="0694a-396">*Figur 21 – FuelEfficientDrivingPatternPipeline utdata*</span><span class="sxs-lookup"><span data-stu-id="0694a-396">*Figure 21 – FuelEfficientDrivingPatternPipeline output*</span></span>

<span data-ttu-id="0694a-397">**Återkalla förutsägelser**</span><span class="sxs-lookup"><span data-stu-id="0694a-397">**Recall Predictions**</span></span>

<span data-ttu-id="0694a-398">Hej maskininlärningsexperiment etablerats och publiceras som en webbtjänst som en del av hello lösningsdistribution.</span><span class="sxs-lookup"><span data-stu-id="0694a-398">hello machine learning experiment is provisioned and published as a web service as part of hello solution deployment.</span></span> <span data-ttu-id="0694a-399">i det här arbetsflödet, registrerats som data factory länkad tjänst och operationalized med hjälp av data factory-batchbedömningsaktivitet utnyttjas hello batchbedömningsjobbet slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="0694a-399">hello batch scoring end point is leveraged in this workflow, registered as a data factory linked service and operationalized using data factory batch scoring activity.</span></span>

![Machine Learning-slutpunkt](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

<span data-ttu-id="0694a-401">*Figur 22 – Machine learning-slutpunkt som är registrerat som en länkad tjänst i data factory*</span><span class="sxs-lookup"><span data-stu-id="0694a-401">*Figure 22 – Machine learning endpoint registered as a linked service in data factory*</span></span>

<span data-ttu-id="0694a-402">hello används registrerade länkade tjänsten i hello DetectAnomalyPipeline tooscore hello data med hjälp av hello avvikelseidentifiering identifiering av modellen.</span><span class="sxs-lookup"><span data-stu-id="0694a-402">hello registered linked service is used in hello DetectAnomalyPipeline tooscore hello data using hello anomaly detection model.</span></span> 

![Datorn Learning batchbedömningsaktivitet i data factory](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png) 

<span data-ttu-id="0694a-404">*Figur 23 – Azure Machine Learning-Batchbedömningen aktivitet i data factory*</span><span class="sxs-lookup"><span data-stu-id="0694a-404">*Figure 23 – Azure Machine Learning Batch Scoring activity in data factory*</span></span> 

<span data-ttu-id="0694a-405">Det finns några steg utförs i den här pipelinen för förberedelse av data så att den kan operationalized med hello batchbedömningsjobbet webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="0694a-405">There are few steps performed in this pipeline for data preparation so that it can be operationalized with hello batch scoring web service.</span></span> 

![DetectAnomalyPipeline för att förutsäga fordon som kräver återställning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png) 

<span data-ttu-id="0694a-407">*Figur 24 – DetectAnomalyPipeline för att förutsäga fordon som kräver återställning*</span><span class="sxs-lookup"><span data-stu-id="0694a-407">*Figure 24 – DetectAnomalyPipeline for predicting vehicles requiring recalls*</span></span> 

<span data-ttu-id="0694a-408">***Avvikelseidentifiering identifiering Hive-fråga***</span><span class="sxs-lookup"><span data-stu-id="0694a-408">***Anomaly detection Hive query***</span></span>

<span data-ttu-id="0694a-409">När hello bedömningen är klar, är en HDInsight-aktivitet används tooprocess och sammanställd hello data som är kategoriserade som avvikelser av hello modell med en sannolikhet poäng 0,60 eller högre.</span><span class="sxs-lookup"><span data-stu-id="0694a-409">Once hello scoring is completed, an HDInsight activity is used tooprocess and aggregate hello data that are categorized as anomalies by hello model with a probability score of 0.60 or higher.</span></span>

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                            string,
                model                        string,
                gendate                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                fuel                        string,
                engineoil                    string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position    string,
                parking_brake_status        string,
                headlamp_status                string,
                brake_pedal_status            string,
                transmission_gear_position    string,
                ignition_status                string,
                windshield_wiper_status        string,
                abs                          string,
                maintenanceLabel            string,
                maintenanceProbability        string,
                RecallLabel                    string,
                RecallProbability            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                            string,
                model                        string,
                city                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                Year                        string,
                Month                        string                

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


<span data-ttu-id="0694a-410">När hello pipeline har körts, finns hello följande partitioner som skapas i ditt lagringskonto i hello ”connectedcar”-behållaren.</span><span class="sxs-lookup"><span data-stu-id="0694a-410">Once hello pipeline is executed successfully, you see hello following partitions generated in your storage account under hello "connectedcar" container.</span></span>

![DetectAnomalyPipeline utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

<span data-ttu-id="0694a-412">*Bild 25 – DetectAnomalyPipeline utdata*</span><span class="sxs-lookup"><span data-stu-id="0694a-412">*Figure 25 – DetectAnomalyPipeline output*</span></span>

## <a name="publish"></a><span data-ttu-id="0694a-413">Publicera</span><span class="sxs-lookup"><span data-stu-id="0694a-413">Publish</span></span>

### <a name="real-time-analysis"></a><span data-ttu-id="0694a-414">Analys i realtid</span><span class="sxs-lookup"><span data-stu-id="0694a-414">Real-time analysis</span></span>
<span data-ttu-id="0694a-415">En av hello frågor i hello Stream Analytics-jobbet publicerar hello händelser tooan utdata Event Hub-instans.</span><span class="sxs-lookup"><span data-stu-id="0694a-415">One of hello queries in hello Stream Analytics job publishes hello events tooan output Event Hub instance.</span></span> 

![Stream Analytics-jobbet publicerar tooan utdata Event Hub-instans](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

<span data-ttu-id="0694a-417">*Bild 26 – Stream Analytics-jobbet publicerar tooan utdata Event Hub-instans*</span><span class="sxs-lookup"><span data-stu-id="0694a-417">*Figure 26 – Stream Analytics job publishes tooan output Event Hub instance*</span></span>

![Stream Analytics query toopublish toohello utdata Event Hub-instans](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

<span data-ttu-id="0694a-419">*Bild 27 – Stream Analytics query toopublish toohello utdata Event Hub-instans*</span><span class="sxs-lookup"><span data-stu-id="0694a-419">*Figure 27 – Stream Analytics query toopublish toohello output Event Hub instance*</span></span>

<span data-ttu-id="0694a-420">Den här dataströmmen av händelser som förbrukas av hello RealTimeDashboardApp ingår i hello lösningen.</span><span class="sxs-lookup"><span data-stu-id="0694a-420">This stream of events is consumed by hello RealTimeDashboardApp included in hello solution.</span></span> <span data-ttu-id="0694a-421">Det här programmet utnyttjar hello Machine Learning begäran och svar webbtjänst för realtid poäng och publicerar hello resulterande data tooa Power BI dataset för användning.</span><span class="sxs-lookup"><span data-stu-id="0694a-421">This application leverages hello Machine Learning Request-Response web service for real-time scoring and publishes hello resultant data tooa Power BI dataset for consumption.</span></span> 

### <a name="batch-analysis"></a><span data-ttu-id="0694a-422">Batchanalys</span><span class="sxs-lookup"><span data-stu-id="0694a-422">Batch analysis</span></span>
<span data-ttu-id="0694a-423">hello resultatet av hello batch och realtidsbearbetning är publicerade toohello Azure SQL Database-tabeller för användning.</span><span class="sxs-lookup"><span data-stu-id="0694a-423">hello results of hello batch and real-time processing are published toohello Azure SQL Database tables for consumption.</span></span> <span data-ttu-id="0694a-424">hello Azure SQL Server, databas och hello tabeller skapas automatiskt som en del av hello installationsskriptet.</span><span class="sxs-lookup"><span data-stu-id="0694a-424">hello Azure SQL Server, Database, and hello tables are created automatically as part of hello setup script.</span></span> 

![Resultat för batch-bearbetning kopiera toodata mart arbetsflöde](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

<span data-ttu-id="0694a-426">*Bild 28 – batchbearbetning resultat kopiera toodata mart arbetsflöde*</span><span class="sxs-lookup"><span data-stu-id="0694a-426">*Figure 28 – Batch processing results copy toodata mart workflow*</span></span>

![Stream Analytics-jobbet publicerar toodata mart](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

<span data-ttu-id="0694a-428">*Bild 29 – Stream Analytics-jobbet publicerar toodata mart*</span><span class="sxs-lookup"><span data-stu-id="0694a-428">*Figure 29 – Stream Analytics job publishes toodata mart*</span></span>

![Data mart-inställningen i Stream Analytics-jobbet](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

<span data-ttu-id="0694a-430">*Bild 30 – dataarkiv i Stream Analytics-jobbet*</span><span class="sxs-lookup"><span data-stu-id="0694a-430">*Figure 30 – Data mart setting in Stream Analytics job*</span></span>

## <a name="consume"></a><span data-ttu-id="0694a-431">Förbruka</span><span class="sxs-lookup"><span data-stu-id="0694a-431">Consume</span></span>
<span data-ttu-id="0694a-432">Powerbi ger den här lösningen en omfattande instrumentpanel för data i realtid och förutsägelseanalys visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="0694a-432">Power BI gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span> 

<span data-ttu-id="0694a-433">Klicka här för detaljerade anvisningar om hur du konfigurerar hello Power BI-rapporter och hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="0694a-433">Click here for detailed instructions on setting up hello Power BI reports and hello dashboard.</span></span> <span data-ttu-id="0694a-434">hello slutliga instrumentpanelen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="0694a-434">hello final dashboard looks like this:</span></span>

![Power BI-instrumentpanel](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

<span data-ttu-id="0694a-436">*Bild 31 - Power BI-instrumentpanel*</span><span class="sxs-lookup"><span data-stu-id="0694a-436">*Figure 31 - Power BI Dashboard*</span></span>

## <a name="summary"></a><span data-ttu-id="0694a-437">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0694a-437">Summary</span></span>
<span data-ttu-id="0694a-438">Det här dokumentet innehåller en detaljerad nedåt i hello Vehicle telemetri Analytics lösning.</span><span class="sxs-lookup"><span data-stu-id="0694a-438">This document contains a detailed drill-down of hello Vehicle Telemetry Analytics Solution.</span></span> <span data-ttu-id="0694a-439">Detta prov på ett lambda-arkitektur mönster för realtid och batch-analytics med förutsägelser och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0694a-439">This showcases a lambda architecture pattern for real-time and batch analytics with predictions and actions.</span></span> <span data-ttu-id="0694a-440">Det här mönstret gäller tooa mängd användningsområden som kräver varm sökväg (realtid) och kalla sökväg (batch) analyser.</span><span class="sxs-lookup"><span data-stu-id="0694a-440">This pattern applies tooa wide range of use cases that require hot path (real-time) and cold path (batch) analytics.</span></span> 

