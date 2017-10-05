---
title: "Ingående till förutsäga vehicle hälsotillstånd och andra vanor - Azure | Microsoft Docs"
description: "Använda funktionerna i Cortana Intelligence och få insikter om i realtid och förutsägbara på vehicle hälsa och köra vanor."
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
ms.openlocfilehash: 0a4dba58445cf0fd9fd8f51d443576bacd92251b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a><span data-ttu-id="5bb43-103">Fordonstelemetrianalys, lösning, playbook: djupdykning i lösningen</span><span class="sxs-lookup"><span data-stu-id="5bb43-103">Vehicle telemetry analytics solution playbook: deep dive into the solution</span></span>
<span data-ttu-id="5bb43-104">Detta **menyn** länkar till avsnitt i den här playbook:</span><span class="sxs-lookup"><span data-stu-id="5bb43-104">This **menu** links to the sections of this playbook:</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

<span data-ttu-id="5bb43-105">Det här avsnittet flyttar ned till var och en av de stegen som beskrivs i lösningsarkitektur med instruktioner och pekare för anpassning.</span><span class="sxs-lookup"><span data-stu-id="5bb43-105">This section drills down into each of the stages depicted in the Solution Architecture with instructions and pointers for customization.</span></span> 

## <a name="data-sources"></a><span data-ttu-id="5bb43-106">Datakällor</span><span class="sxs-lookup"><span data-stu-id="5bb43-106">Data Sources</span></span>
<span data-ttu-id="5bb43-107">Lösningen använder två olika datakällor:</span><span class="sxs-lookup"><span data-stu-id="5bb43-107">The solution uses two different data sources:</span></span>

* <span data-ttu-id="5bb43-108">**simulerade vehicle signaler och diagnostik dataset** och</span><span class="sxs-lookup"><span data-stu-id="5bb43-108">**simulated vehicle signals and diagnostic dataset** and</span></span> 
* <span data-ttu-id="5bb43-109">**vehicle katalog**</span><span class="sxs-lookup"><span data-stu-id="5bb43-109">**vehicle catalog**</span></span>

<span data-ttu-id="5bb43-110">Vehicle telematik simulator ingår som en del av den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="5bb43-110">A vehicle telematics simulator is included as part of this solution.</span></span> <span data-ttu-id="5bb43-111">Den genererar diagnostisk information och signalerar till motsvarande tillstånd för programuppdatering och intresseväckande mönstret vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="5bb43-111">It emits diagnostic information and signals corresponding to the state of the vehicle and to the driving pattern at a given point in time.</span></span> <span data-ttu-id="5bb43-112">Klicka på [Vehicle telematik simulatorn](http://go.microsoft.com/fwlink/?LinkId=717075) att hämta den **Vehicle telematik Simulator lösning i Visual Studio** anpassningar baserat på dina krav.</span><span class="sxs-lookup"><span data-stu-id="5bb43-112">Click [Vehicle Telematics Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) to download the **Vehicle Telematics Simulator Visual Studio Solution** for customizations based on your requirements.</span></span> <span data-ttu-id="5bb43-113">Vehicle katalogen innehåller en referens datamängd med Registreringsnumret för Modellmappning.</span><span class="sxs-lookup"><span data-stu-id="5bb43-113">The vehicle catalog contains a reference dataset with a VIN to model mapping.</span></span>

![Vehicle telematik simulator](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)

<span data-ttu-id="5bb43-115">*Bild 1 – Vehicle telematik Simulator*</span><span class="sxs-lookup"><span data-stu-id="5bb43-115">*Figure 1 – Vehicle Telematics Simulator*</span></span>

<span data-ttu-id="5bb43-116">Det här är en JSON-formaterad datamängd som innehåller följande schema.</span><span class="sxs-lookup"><span data-stu-id="5bb43-116">This is a JSON-formatted dataset that contains the following schema.</span></span>

| <span data-ttu-id="5bb43-117">Kolumn</span><span class="sxs-lookup"><span data-stu-id="5bb43-117">Column</span></span> | <span data-ttu-id="5bb43-118">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5bb43-118">Description</span></span> | <span data-ttu-id="5bb43-119">Värden</span><span class="sxs-lookup"><span data-stu-id="5bb43-119">Values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5bb43-120">VIN</span><span class="sxs-lookup"><span data-stu-id="5bb43-120">VIN</span></span> |<span data-ttu-id="5bb43-121">Slumpmässigt genererat Vehicle ID-nummer</span><span class="sxs-lookup"><span data-stu-id="5bb43-121">Randomly generated Vehicle Identification Number</span></span> |<span data-ttu-id="5bb43-122">Detta hämtas från en övergripande lista med 10 000 slumpmässigt genererat vehicle ID-nummer.</span><span class="sxs-lookup"><span data-stu-id="5bb43-122">This is obtained from a master list of 10,000 randomly generated vehicle identification numbers.</span></span> |
| <span data-ttu-id="5bb43-123">Utanför temperatur</span><span class="sxs-lookup"><span data-stu-id="5bb43-123">Outside temperature</span></span> |<span data-ttu-id="5bb43-124">Utanför temperaturen där vehicle kör</span><span class="sxs-lookup"><span data-stu-id="5bb43-124">The outside temperature where the vehicle is driving</span></span> |<span data-ttu-id="5bb43-125">Slumptal mellan 0-100</span><span class="sxs-lookup"><span data-stu-id="5bb43-125">Randomly generated number from 0-100</span></span> |
| <span data-ttu-id="5bb43-126">Motorn temperatur</span><span class="sxs-lookup"><span data-stu-id="5bb43-126">Engine temperature</span></span> |<span data-ttu-id="5bb43-127">Motorn temperatur för programuppdatering</span><span class="sxs-lookup"><span data-stu-id="5bb43-127">The engine temperature of the vehicle</span></span> |<span data-ttu-id="5bb43-128">Slumpmässigt genererat tal mellan 0 och 500</span><span class="sxs-lookup"><span data-stu-id="5bb43-128">Randomly generated number from 0-500</span></span> |
| <span data-ttu-id="5bb43-129">Hastighet</span><span class="sxs-lookup"><span data-stu-id="5bb43-129">Speed</span></span> |<span data-ttu-id="5bb43-130">Motorns hastighet som som aktiverar för programuppdatering</span><span class="sxs-lookup"><span data-stu-id="5bb43-130">The engine speed at which the vehicle is driving</span></span> |<span data-ttu-id="5bb43-131">Slumptal mellan 0-100</span><span class="sxs-lookup"><span data-stu-id="5bb43-131">Randomly generated number from 0-100</span></span> |
| <span data-ttu-id="5bb43-132">Bränsle</span><span class="sxs-lookup"><span data-stu-id="5bb43-132">Fuel</span></span> |<span data-ttu-id="5bb43-133">Bränslenivå för för programuppdatering</span><span class="sxs-lookup"><span data-stu-id="5bb43-133">The fuel level of the vehicle</span></span> |<span data-ttu-id="5bb43-134">Slumptal mellan 0-100 (anger bränsle nivån procent)</span><span class="sxs-lookup"><span data-stu-id="5bb43-134">Randomly generated number from 0-100 (indicates fuel level percentage)</span></span> |
| <span data-ttu-id="5bb43-135">EngineOil</span><span class="sxs-lookup"><span data-stu-id="5bb43-135">EngineOil</span></span> |<span data-ttu-id="5bb43-136">Motorn olja andelen för programuppdatering</span><span class="sxs-lookup"><span data-stu-id="5bb43-136">The engine oil level of the vehicle</span></span> |<span data-ttu-id="5bb43-137">Slumptal mellan 0-100 (anger motorn olja nivån procent)</span><span class="sxs-lookup"><span data-stu-id="5bb43-137">Randomly generated number from 0-100 (indicates engine oil level percentage)</span></span> |
| <span data-ttu-id="5bb43-138">Däck hög belastning</span><span class="sxs-lookup"><span data-stu-id="5bb43-138">Tire pressure</span></span> |<span data-ttu-id="5bb43-139">Däck tryck för programuppdatering</span><span class="sxs-lookup"><span data-stu-id="5bb43-139">The tire pressure of the vehicle</span></span> |<span data-ttu-id="5bb43-140">Slumpmässigt tal mellan 0-50 (anger däck trycket nivån procent)</span><span class="sxs-lookup"><span data-stu-id="5bb43-140">Randomly generated number from 0-50 (indicates tire pressure level percentage)</span></span> |
| <span data-ttu-id="5bb43-141">Mätarställning</span><span class="sxs-lookup"><span data-stu-id="5bb43-141">Odometer</span></span> |<span data-ttu-id="5bb43-142">Mätarställning för för programuppdatering</span><span class="sxs-lookup"><span data-stu-id="5bb43-142">The odometer reading of the vehicle</span></span> |<span data-ttu-id="5bb43-143">Slumpmässigt genererat nummer från 0 200000</span><span class="sxs-lookup"><span data-stu-id="5bb43-143">Randomly generated number from 0-200000</span></span> |
| <span data-ttu-id="5bb43-144">Accelerator_pedal_position</span><span class="sxs-lookup"><span data-stu-id="5bb43-144">Accelerator_pedal_position</span></span> |<span data-ttu-id="5bb43-145">Accelerator cyklar placering för programuppdatering</span><span class="sxs-lookup"><span data-stu-id="5bb43-145">The accelerator pedal position of the vehicle</span></span> |<span data-ttu-id="5bb43-146">Slumptal mellan 0-100 (anger accelerator nivån procent)</span><span class="sxs-lookup"><span data-stu-id="5bb43-146">Randomly generated number from 0-100 (indicates accelerator level percentage)</span></span> |
| <span data-ttu-id="5bb43-147">Parking_brake_status</span><span class="sxs-lookup"><span data-stu-id="5bb43-147">Parking_brake_status</span></span> |<span data-ttu-id="5bb43-148">Anger om vehicle parkerade eller inte</span><span class="sxs-lookup"><span data-stu-id="5bb43-148">Indicates whether the vehicle is parked or not</span></span> |<span data-ttu-id="5bb43-149">True eller False</span><span class="sxs-lookup"><span data-stu-id="5bb43-149">True or False</span></span> |
| <span data-ttu-id="5bb43-150">Headlamp_status</span><span class="sxs-lookup"><span data-stu-id="5bb43-150">Headlamp_status</span></span> |<span data-ttu-id="5bb43-151">Anger om strålkastaren är på eller inte</span><span class="sxs-lookup"><span data-stu-id="5bb43-151">Indicates where the headlamp is on or not</span></span> |<span data-ttu-id="5bb43-152">True eller False</span><span class="sxs-lookup"><span data-stu-id="5bb43-152">True or False</span></span> |
| <span data-ttu-id="5bb43-153">Brake_pedal_status</span><span class="sxs-lookup"><span data-stu-id="5bb43-153">Brake_pedal_status</span></span> |<span data-ttu-id="5bb43-154">Anger om bromspedal är nedtryckt eller inte</span><span class="sxs-lookup"><span data-stu-id="5bb43-154">Indicates whether the brake pedal is pressed or not</span></span> |<span data-ttu-id="5bb43-155">True eller False</span><span class="sxs-lookup"><span data-stu-id="5bb43-155">True or False</span></span> |
| <span data-ttu-id="5bb43-156">Transmission_gear_position</span><span class="sxs-lookup"><span data-stu-id="5bb43-156">Transmission_gear_position</span></span> |<span data-ttu-id="5bb43-157">Överföring växeln placering för programuppdatering</span><span class="sxs-lookup"><span data-stu-id="5bb43-157">The transmission gear position of the vehicle</span></span> |<span data-ttu-id="5bb43-158">Tillstånd: första, andra, tredje, fjärde, femte, sjätte, sjunde, åttonde</span><span class="sxs-lookup"><span data-stu-id="5bb43-158">States: first, second, third, fourth, fifth, sixth, seventh, eighth</span></span> |
| <span data-ttu-id="5bb43-159">Ignition_status</span><span class="sxs-lookup"><span data-stu-id="5bb43-159">Ignition_status</span></span> |<span data-ttu-id="5bb43-160">Anger om är igång eller Stoppad</span><span class="sxs-lookup"><span data-stu-id="5bb43-160">Indicates whether the vehicle is running or stopped</span></span> |<span data-ttu-id="5bb43-161">True eller False</span><span class="sxs-lookup"><span data-stu-id="5bb43-161">True or False</span></span> |
| <span data-ttu-id="5bb43-162">Windshield_wiper_status</span><span class="sxs-lookup"><span data-stu-id="5bb43-162">Windshield_wiper_status</span></span> |<span data-ttu-id="5bb43-163">Anger om vindrutan vindrutetorkare är aktiverat eller inte</span><span class="sxs-lookup"><span data-stu-id="5bb43-163">Indicates whether the windshield wiper is turned or not</span></span> |<span data-ttu-id="5bb43-164">True eller False</span><span class="sxs-lookup"><span data-stu-id="5bb43-164">True or False</span></span> |
| <span data-ttu-id="5bb43-165">ABS</span><span class="sxs-lookup"><span data-stu-id="5bb43-165">ABS</span></span> |<span data-ttu-id="5bb43-166">Anger om ABS används eller inte</span><span class="sxs-lookup"><span data-stu-id="5bb43-166">Indicates whether ABS is engaged or not</span></span> |<span data-ttu-id="5bb43-167">True eller False</span><span class="sxs-lookup"><span data-stu-id="5bb43-167">True or False</span></span> |
| <span data-ttu-id="5bb43-168">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="5bb43-168">Timestamp</span></span> |<span data-ttu-id="5bb43-169">Tidsstämpel när datapunkten som har skapats</span><span class="sxs-lookup"><span data-stu-id="5bb43-169">The timestamp when the data point is created</span></span> |<span data-ttu-id="5bb43-170">Date</span><span class="sxs-lookup"><span data-stu-id="5bb43-170">Date</span></span> |
| <span data-ttu-id="5bb43-171">Ort</span><span class="sxs-lookup"><span data-stu-id="5bb43-171">City</span></span> |<span data-ttu-id="5bb43-172">Platsen för programuppdatering</span><span class="sxs-lookup"><span data-stu-id="5bb43-172">The location of the vehicle</span></span> |<span data-ttu-id="5bb43-173">4 orter i den här lösningen: Bellevue, Redmond, Sammamish, Seattle</span><span class="sxs-lookup"><span data-stu-id="5bb43-173">4 cities in this solution: Bellevue, Redmond, Sammamish, Seattle</span></span> |

<span data-ttu-id="5bb43-174">Referensdatauppsättningen vehicle modellen innehåller VIN till modellen mappningen.</span><span class="sxs-lookup"><span data-stu-id="5bb43-174">The vehicle model reference dataset contains VIN to the model mapping.</span></span> 

| <span data-ttu-id="5bb43-175">VIN</span><span class="sxs-lookup"><span data-stu-id="5bb43-175">VIN</span></span> | <span data-ttu-id="5bb43-176">Modellen</span><span class="sxs-lookup"><span data-stu-id="5bb43-176">Model</span></span> |
| --- | --- |
| <span data-ttu-id="5bb43-177">FHL3O1SA4IEHB4WU1</span><span class="sxs-lookup"><span data-stu-id="5bb43-177">FHL3O1SA4IEHB4WU1</span></span> |<span data-ttu-id="5bb43-178">Sedan</span><span class="sxs-lookup"><span data-stu-id="5bb43-178">Sedan</span></span> |
| <span data-ttu-id="5bb43-179">8J0U8XCPRGW4Z3NQE</span><span class="sxs-lookup"><span data-stu-id="5bb43-179">8J0U8XCPRGW4Z3NQE</span></span> |<span data-ttu-id="5bb43-180">Hybrid</span><span class="sxs-lookup"><span data-stu-id="5bb43-180">Hybrid</span></span> |
| <span data-ttu-id="5bb43-181">WORG68Z2PLTNZDBI7</span><span class="sxs-lookup"><span data-stu-id="5bb43-181">WORG68Z2PLTNZDBI7</span></span> |<span data-ttu-id="5bb43-182">Family sedan</span><span class="sxs-lookup"><span data-stu-id="5bb43-182">Family Saloon</span></span> |
| <span data-ttu-id="5bb43-183">JTHMYHQTEPP4WBMRN</span><span class="sxs-lookup"><span data-stu-id="5bb43-183">JTHMYHQTEPP4WBMRN</span></span> |<span data-ttu-id="5bb43-184">Sedan</span><span class="sxs-lookup"><span data-stu-id="5bb43-184">Sedan</span></span> |
| <span data-ttu-id="5bb43-185">W9FTHG27LZN1YWO0Y</span><span class="sxs-lookup"><span data-stu-id="5bb43-185">W9FTHG27LZN1YWO0Y</span></span> |<span data-ttu-id="5bb43-186">Hybrid</span><span class="sxs-lookup"><span data-stu-id="5bb43-186">Hybrid</span></span> |
| <span data-ttu-id="5bb43-187">MHTP9N792PHK08WJM</span><span class="sxs-lookup"><span data-stu-id="5bb43-187">MHTP9N792PHK08WJM</span></span> |<span data-ttu-id="5bb43-188">Family sedan</span><span class="sxs-lookup"><span data-stu-id="5bb43-188">Family Saloon</span></span> |
| <span data-ttu-id="5bb43-189">EI4QXI2AXVQQING4I</span><span class="sxs-lookup"><span data-stu-id="5bb43-189">EI4QXI2AXVQQING4I</span></span> |<span data-ttu-id="5bb43-190">Sedan</span><span class="sxs-lookup"><span data-stu-id="5bb43-190">Sedan</span></span> |
| <span data-ttu-id="5bb43-191">5KKR2VB4WHQH97PF8</span><span class="sxs-lookup"><span data-stu-id="5bb43-191">5KKR2VB4WHQH97PF8</span></span> |<span data-ttu-id="5bb43-192">Hybrid</span><span class="sxs-lookup"><span data-stu-id="5bb43-192">Hybrid</span></span> |
| <span data-ttu-id="5bb43-193">W9NSZ423XZHAONYXB</span><span class="sxs-lookup"><span data-stu-id="5bb43-193">W9NSZ423XZHAONYXB</span></span> |<span data-ttu-id="5bb43-194">Family sedan</span><span class="sxs-lookup"><span data-stu-id="5bb43-194">Family Saloon</span></span> |
| <span data-ttu-id="5bb43-195">26WJSGHX4MA5ROHNL</span><span class="sxs-lookup"><span data-stu-id="5bb43-195">26WJSGHX4MA5ROHNL</span></span> |<span data-ttu-id="5bb43-196">Kan konverteras</span><span class="sxs-lookup"><span data-stu-id="5bb43-196">Convertible</span></span> |
| <span data-ttu-id="5bb43-197">GHLUB6ONKMOSI7E77</span><span class="sxs-lookup"><span data-stu-id="5bb43-197">GHLUB6ONKMOSI7E77</span></span> |<span data-ttu-id="5bb43-198">Station vagn</span><span class="sxs-lookup"><span data-stu-id="5bb43-198">Station Wagon</span></span> |
| <span data-ttu-id="5bb43-199">9C2RHVRVLMEJDBXLP</span><span class="sxs-lookup"><span data-stu-id="5bb43-199">9C2RHVRVLMEJDBXLP</span></span> |<span data-ttu-id="5bb43-200">Bil</span><span class="sxs-lookup"><span data-stu-id="5bb43-200">Compact Car</span></span> |
| <span data-ttu-id="5bb43-201">BRNHVMZOUJ6EOCP32</span><span class="sxs-lookup"><span data-stu-id="5bb43-201">BRNHVMZOUJ6EOCP32</span></span> |<span data-ttu-id="5bb43-202">Liten Stadsjeep</span><span class="sxs-lookup"><span data-stu-id="5bb43-202">Small SUV</span></span> |
| <span data-ttu-id="5bb43-203">VCYVW0WUZNBTM594J</span><span class="sxs-lookup"><span data-stu-id="5bb43-203">VCYVW0WUZNBTM594J</span></span> |<span data-ttu-id="5bb43-204">Sport bil</span><span class="sxs-lookup"><span data-stu-id="5bb43-204">Sports Car</span></span> |
| <span data-ttu-id="5bb43-205">HNVCE6YFZSA5M82NY</span><span class="sxs-lookup"><span data-stu-id="5bb43-205">HNVCE6YFZSA5M82NY</span></span> |<span data-ttu-id="5bb43-206">Medelhög Stadsjeep</span><span class="sxs-lookup"><span data-stu-id="5bb43-206">Medium SUV</span></span> |
| <span data-ttu-id="5bb43-207">4R30FOR7NUOBL05GJ</span><span class="sxs-lookup"><span data-stu-id="5bb43-207">4R30FOR7NUOBL05GJ</span></span> |<span data-ttu-id="5bb43-208">Station vagn</span><span class="sxs-lookup"><span data-stu-id="5bb43-208">Station Wagon</span></span> |
| <span data-ttu-id="5bb43-209">WYNIIY42VKV6OQS1J</span><span class="sxs-lookup"><span data-stu-id="5bb43-209">WYNIIY42VKV6OQS1J</span></span> |<span data-ttu-id="5bb43-210">Stora Stadsjeep</span><span class="sxs-lookup"><span data-stu-id="5bb43-210">Large SUV</span></span> |
| <span data-ttu-id="5bb43-211">8Y5QKG27QET1RBK7I</span><span class="sxs-lookup"><span data-stu-id="5bb43-211">8Y5QKG27QET1RBK7I</span></span> |<span data-ttu-id="5bb43-212">Stora Stadsjeep</span><span class="sxs-lookup"><span data-stu-id="5bb43-212">Large SUV</span></span> |
| <span data-ttu-id="5bb43-213">DF6OX2WSRA6511BVG</span><span class="sxs-lookup"><span data-stu-id="5bb43-213">DF6OX2WSRA6511BVG</span></span> |<span data-ttu-id="5bb43-214">Coupe</span><span class="sxs-lookup"><span data-stu-id="5bb43-214">Coupe</span></span> |
| <span data-ttu-id="5bb43-215">Z2EOZWZBXAEW3E60T</span><span class="sxs-lookup"><span data-stu-id="5bb43-215">Z2EOZWZBXAEW3E60T</span></span> |<span data-ttu-id="5bb43-216">Sedan</span><span class="sxs-lookup"><span data-stu-id="5bb43-216">Sedan</span></span> |
| <span data-ttu-id="5bb43-217">M4TV6IEALD5QDS3IR</span><span class="sxs-lookup"><span data-stu-id="5bb43-217">M4TV6IEALD5QDS3IR</span></span> |<span data-ttu-id="5bb43-218">Hybrid</span><span class="sxs-lookup"><span data-stu-id="5bb43-218">Hybrid</span></span> |
| <span data-ttu-id="5bb43-219">VHRA1Y2TGTA84F00H</span><span class="sxs-lookup"><span data-stu-id="5bb43-219">VHRA1Y2TGTA84F00H</span></span> |<span data-ttu-id="5bb43-220">Family sedan</span><span class="sxs-lookup"><span data-stu-id="5bb43-220">Family Saloon</span></span> |
| <span data-ttu-id="5bb43-221">R0JAUHT1L1R3BIKI0</span><span class="sxs-lookup"><span data-stu-id="5bb43-221">R0JAUHT1L1R3BIKI0</span></span> |<span data-ttu-id="5bb43-222">Sedan</span><span class="sxs-lookup"><span data-stu-id="5bb43-222">Sedan</span></span> |
| <span data-ttu-id="5bb43-223">9230C202Z60XX84AU</span><span class="sxs-lookup"><span data-stu-id="5bb43-223">9230C202Z60XX84AU</span></span> |<span data-ttu-id="5bb43-224">Hybrid</span><span class="sxs-lookup"><span data-stu-id="5bb43-224">Hybrid</span></span> |
| <span data-ttu-id="5bb43-225">T8DNDN5UDCWL7M72H</span><span class="sxs-lookup"><span data-stu-id="5bb43-225">T8DNDN5UDCWL7M72H</span></span> |<span data-ttu-id="5bb43-226">Family sedan</span><span class="sxs-lookup"><span data-stu-id="5bb43-226">Family Saloon</span></span> |
| <span data-ttu-id="5bb43-227">4WPYRUZII5YV7YA42</span><span class="sxs-lookup"><span data-stu-id="5bb43-227">4WPYRUZII5YV7YA42</span></span> |<span data-ttu-id="5bb43-228">Sedan</span><span class="sxs-lookup"><span data-stu-id="5bb43-228">Sedan</span></span> |
| <span data-ttu-id="5bb43-229">D1ZVY26UV2BFGHZNO</span><span class="sxs-lookup"><span data-stu-id="5bb43-229">D1ZVY26UV2BFGHZNO</span></span> |<span data-ttu-id="5bb43-230">Hybrid</span><span class="sxs-lookup"><span data-stu-id="5bb43-230">Hybrid</span></span> |
| <span data-ttu-id="5bb43-231">XUF99EW9OIQOMV7Q7</span><span class="sxs-lookup"><span data-stu-id="5bb43-231">XUF99EW9OIQOMV7Q7</span></span> |<span data-ttu-id="5bb43-232">Family sedan</span><span class="sxs-lookup"><span data-stu-id="5bb43-232">Family Saloon</span></span> |
| <span data-ttu-id="5bb43-233">8OMCL3LGI7XNCC21U</span><span class="sxs-lookup"><span data-stu-id="5bb43-233">8OMCL3LGI7XNCC21U</span></span> |<span data-ttu-id="5bb43-234">Kan konverteras</span><span class="sxs-lookup"><span data-stu-id="5bb43-234">Convertible</span></span> |
| <span data-ttu-id="5bb43-235">…….</span><span class="sxs-lookup"><span data-stu-id="5bb43-235">…….</span></span> | |

### <a name="references"></a><span data-ttu-id="5bb43-236">Referenser</span><span class="sxs-lookup"><span data-stu-id="5bb43-236">References</span></span>
[<span data-ttu-id="5bb43-237">Vehicle telematik Simulator Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="5bb43-237">Vehicle Telematics Simulator Visual Studio Solution</span></span>](http://go.microsoft.com/fwlink/?LinkId=717075) 

[<span data-ttu-id="5bb43-238">Azure Event Hub</span><span class="sxs-lookup"><span data-stu-id="5bb43-238">Azure Event Hub</span></span>](https://azure.microsoft.com/services/event-hubs/)

[<span data-ttu-id="5bb43-239">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="5bb43-239">Azure Data Factory</span></span>](https://azure.microsoft.com/documentation/learning-paths/data-factory/)

## <a name="ingestion"></a><span data-ttu-id="5bb43-240">Införandet</span><span class="sxs-lookup"><span data-stu-id="5bb43-240">Ingestion</span></span>
<span data-ttu-id="5bb43-241">Kombinationer av Händelsehubbar i Azure Stream Analytics och Data Factory utnyttjas för att mata in vehicle signaler, diagnostiska händelser och realtid och batch-analytics.</span><span class="sxs-lookup"><span data-stu-id="5bb43-241">Combinations of Azure Event Hubs, Stream Analytics, and Data Factory are leveraged to ingest the vehicle signals, the diagnostic events, and real-time and batch analytics.</span></span> <span data-ttu-id="5bb43-242">Alla dessa komponenter skapas och konfigureras som en del av distributionen av lösningen.</span><span class="sxs-lookup"><span data-stu-id="5bb43-242">All these components are created and configured as part of the solution deployment.</span></span> 

### <a name="real-time-analysis"></a><span data-ttu-id="5bb43-243">Analys i realtid</span><span class="sxs-lookup"><span data-stu-id="5bb43-243">Real-time analysis</span></span>
<span data-ttu-id="5bb43-244">Händelser som genererats av Vehicle telematik Simulator publiceras till Händelsehubben med hjälp av Event Hub SDK.</span><span class="sxs-lookup"><span data-stu-id="5bb43-244">The events generated by the Vehicle Telematics Simulator are published to the Event Hub using the Event Hub SDK.</span></span> <span data-ttu-id="5bb43-245">Stream Analytics-jobbet en dessa händelser från Event Hub och bearbetar data i realtid för att analysera vehicle hälsa.</span><span class="sxs-lookup"><span data-stu-id="5bb43-245">The Stream Analytics job ingests these events from the Event Hub and processes the data in real time to analyze the vehicle health.</span></span> 

![Event hub instrumentpanelen](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

<span data-ttu-id="5bb43-247">*Bild 4 - Event Hub-instrumentpanelen*</span><span class="sxs-lookup"><span data-stu-id="5bb43-247">*Figure 4 - Event Hub dashboard*</span></span>

![Strömma Analytics-jobbet databearbetning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 

<span data-ttu-id="5bb43-249">*Bild 5 - Stream Analytics-jobbet bearbetning av data*</span><span class="sxs-lookup"><span data-stu-id="5bb43-249">*Figure 5 - Stream Analytics job processing data*</span></span>

<span data-ttu-id="5bb43-250">Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="5bb43-250">The Stream Analytics job;</span></span>

* <span data-ttu-id="5bb43-251">en data från Event Hub</span><span class="sxs-lookup"><span data-stu-id="5bb43-251">ingests data from the Event Hub</span></span> 
* <span data-ttu-id="5bb43-252">Utför en koppling med referensdata att mappa vehicle VIN till motsvarande modellen</span><span class="sxs-lookup"><span data-stu-id="5bb43-252">performs a join with the reference data to map the vehicle VIN to the corresponding model</span></span> 
* <span data-ttu-id="5bb43-253">sparar dem i Azure blob-lagring för omfattande batch analytics.</span><span class="sxs-lookup"><span data-stu-id="5bb43-253">persists them into Azure blob storage for rich batch analytics.</span></span> 

<span data-ttu-id="5bb43-254">Följande Stream Analytics-fråga används för att spara data i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="5bb43-254">The following Stream Analytics query is used to persist the data into Azure blob storage.</span></span> 

![Stream Analytics-jobbet frågan för datapåfyllning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

<span data-ttu-id="5bb43-256">*Bild 6 - Stream Analytics-jobbet frågan för datapåfyllning*</span><span class="sxs-lookup"><span data-stu-id="5bb43-256">*Figure 6 - Stream Analytics job query for data ingestion*</span></span>

### <a name="batch-analysis"></a><span data-ttu-id="5bb43-257">Batchanalys</span><span class="sxs-lookup"><span data-stu-id="5bb43-257">Batch analysis</span></span>
<span data-ttu-id="5bb43-258">Vi också genererar en ytterligare volym av simulerade vehicle signaler och diagnostik datamängden för bättre batch analytics.</span><span class="sxs-lookup"><span data-stu-id="5bb43-258">We are also generating an additional volume of simulated vehicle signals and diagnostic dataset for richer batch analytics.</span></span> <span data-ttu-id="5bb43-259">Detta krävs för att säkerställa en god representativt datavolym för batch-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="5bb43-259">This is required to ensure a good representative data volume for batch processing.</span></span> <span data-ttu-id="5bb43-260">För detta ändamål använder en pipeline med namnet ”PrepareSampleDataPipeline” i Azure Data Factory-arbetsflödet för att generera ett år kan du se simulerade vehicle signaler och diagnostik dataset.</span><span class="sxs-lookup"><span data-stu-id="5bb43-260">For this purpose, we are using a pipeline named "PrepareSampleDataPipeline" in the Azure Data Factory workflow to generate one year's worth of simulated vehicle signals and diagnostic dataset.</span></span> <span data-ttu-id="5bb43-261">Klicka på [Data Factory anpassad aktivitet](http://go.microsoft.com/fwlink/?LinkId=717077) att hämta Data Factory anpassad DotNet aktivitet Visual Studio-lösning för anpassningar baserat på dina krav.</span><span class="sxs-lookup"><span data-stu-id="5bb43-261">Click [Data Factory custom activity](http://go.microsoft.com/fwlink/?LinkId=717077) to download the Data Factory custom DotNet activity Visual Studio solution for customizations based on your requirements.</span></span> 

![Förbereda exempeldata för batchbearbetning arbetsflöde](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

<span data-ttu-id="5bb43-263">*Figur 7 – förbereda exempeldata för arbetsflöde för batch-bearbetning*</span><span class="sxs-lookup"><span data-stu-id="5bb43-263">*Figure 7 - Prepare Sample data for batch processing workflow*</span></span>

<span data-ttu-id="5bb43-264">Pipelinen består av en anpassad ADF .net aktivitet, visas här:</span><span class="sxs-lookup"><span data-stu-id="5bb43-264">The pipeline consists of a custom ADF .Net Activity, show here:</span></span>

![PrepareSampleDataPipeline aktivitet](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

<span data-ttu-id="5bb43-266">*Figur 8 - PrepareSampleDataPipeline*</span><span class="sxs-lookup"><span data-stu-id="5bb43-266">*Figure 8 - PrepareSampleDataPipeline*</span></span>

<span data-ttu-id="5bb43-267">När pipelinen körs korrekt och ”RawCarEventsTable” dataset markeras ”klar” års värt simulerade vehicle signaler och diagnostiska som data produceras.</span><span class="sxs-lookup"><span data-stu-id="5bb43-267">Once the pipeline executes successfully and "RawCarEventsTable" dataset is marked "Ready", one-year worth of simulated vehicle signals and diagnostic data are produced.</span></span> <span data-ttu-id="5bb43-268">Du kan se på följande mapp och fil som skapats i ditt lagringskonto i ”connectedcar”-behållaren:</span><span class="sxs-lookup"><span data-stu-id="5bb43-268">You see the following folder and file created in your storage account under the "connectedcar" container:</span></span>

![PrepareSampleDataPipeline utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

<span data-ttu-id="5bb43-270">*Bild 9 - PrepareSampleDataPipeline utdata*</span><span class="sxs-lookup"><span data-stu-id="5bb43-270">*Figure 9 - PrepareSampleDataPipeline Output*</span></span>

### <a name="references"></a><span data-ttu-id="5bb43-271">Referenser</span><span class="sxs-lookup"><span data-stu-id="5bb43-271">References</span></span>
[<span data-ttu-id="5bb43-272">Azure Event Hub-SDK för inhämtning av dataströmmar</span><span class="sxs-lookup"><span data-stu-id="5bb43-272">Azure Event Hub SDK for stream ingestion</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

<span data-ttu-id="5bb43-273">[Azure Data Factory data movement funktioner](../data-factory/data-factory-data-movement-activities.md)
[DotNet-aktivitet för Azure Data Factory](../data-factory/data-factory-use-custom-activities.md)</span><span class="sxs-lookup"><span data-stu-id="5bb43-273">[Azure Data Factory data movement capabilities](../data-factory/data-factory-data-movement-activities.md)
[Azure Data Factory DotNet Activity](../data-factory/data-factory-use-custom-activities.md)</span></span>

[<span data-ttu-id="5bb43-274">Azure Data Factory DotNet aktivitet visual studio-lösning för att förbereda exempeldata</span><span class="sxs-lookup"><span data-stu-id="5bb43-274">Azure Data Factory DotNet activity visual studio solution for preparing sample data</span></span>](http://go.microsoft.com/fwlink/?LinkId=717077) 

## <a name="partition-the-dataset"></a><span data-ttu-id="5bb43-275">Partitionen datauppsättningen</span><span class="sxs-lookup"><span data-stu-id="5bb43-275">Partition the dataset</span></span>
<span data-ttu-id="5bb43-276">Rådata halvstrukturerade vehicle signaler och diagnostik datamängd partitioneras i förberedelsen av data till formatet år/månad.</span><span class="sxs-lookup"><span data-stu-id="5bb43-276">The raw semi-structured vehicle signals and diagnostic dataset are partitioned in the data preparation step into a YEAR/MONTH format.</span></span> <span data-ttu-id="5bb43-277">Den här partitionering befordrar effektivare fråga och skalbar långsiktig lagring genom att aktivera fel-över från en blob-konto till en annan när det första kontot fylls.</span><span class="sxs-lookup"><span data-stu-id="5bb43-277">This partitioning promotes more efficient querying and scalable long-term storage by enabling fault-over from one blob account to the next as the first account fills up.</span></span> 

>[!NOTE] 
><span data-ttu-id="5bb43-278">Det här steget i lösningen gäller endast för batch-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="5bb43-278">This step in the solution is applicable only to batch processing.</span></span>

<span data-ttu-id="5bb43-279">Indata och utdata hantering av data:</span><span class="sxs-lookup"><span data-stu-id="5bb43-279">Input and output data data management:</span></span>

* <span data-ttu-id="5bb43-280">Den **utdata** (märkta *PartitionedCarEventsTable*) ska bevaras under en längre tidsperiod som grundläggande / ”rawest” form av data i kundens ”Data Lake”.</span><span class="sxs-lookup"><span data-stu-id="5bb43-280">The **output data** (labeled *PartitionedCarEventsTable*) is to be kept for a long period of time as the foundational/"rawest" form of data in the customer's "Data Lake".</span></span> 
* <span data-ttu-id="5bb43-281">Den **indata** till den här pipelinen skulle normalt ignoreras som utdata har fullständig återgivning inkommande - lagras bara (partitionerad) bättre för senare användning.</span><span class="sxs-lookup"><span data-stu-id="5bb43-281">The **input data** to this pipeline would typically be discarded as the output data has full fidelity to the input - it's just stored (partitioned) better for subsequent use.</span></span>

![Partitionen bil händelser arbetsflöde](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)

<span data-ttu-id="5bb43-283">*Figur 10 – Partition bil händelser arbetsflöde*</span><span class="sxs-lookup"><span data-stu-id="5bb43-283">*Figure 10 – Partition Car Events workflow*</span></span>

<span data-ttu-id="5bb43-284">Rådata är partitionerad med hjälp av en HDInsight Hive-aktivitet i ”PartitionCarEventsPipeline”.</span><span class="sxs-lookup"><span data-stu-id="5bb43-284">The raw data is partitioned using a Hive HDInsight activity in "PartitionCarEventsPipeline".</span></span> <span data-ttu-id="5bb43-285">Exempeldata genereras i steg 1 för ett år har partitionerats med år/månad.</span><span class="sxs-lookup"><span data-stu-id="5bb43-285">The sample data generated in step 1 for a year is partitioned by YEAR/MONTH.</span></span> <span data-ttu-id="5bb43-286">Partitionerna som används för att generera vehicle signaler och diagnostikdata för varje månad (totalt 12 partitioner) för ett år.</span><span class="sxs-lookup"><span data-stu-id="5bb43-286">The partitions are used to generate vehicle signals and diagnostic data for each month (total 12 partitions) of a year.</span></span> 

![PartitionCarEventsPipeline aktivitet](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)

<span data-ttu-id="5bb43-288">*Figur 11 - PartitionCarEventsPipeline*</span><span class="sxs-lookup"><span data-stu-id="5bb43-288">*Figure 11 - PartitionCarEventsPipeline*</span></span>

<span data-ttu-id="5bb43-289">***PartitionConnectedCarEvents Hive-skript***</span><span class="sxs-lookup"><span data-stu-id="5bb43-289">***PartitionConnectedCarEvents Hive Script***</span></span>

<span data-ttu-id="5bb43-290">Följande Hive-skript, med namnet ”partitioncarevents.hql” används för partitionering och finns i mappen ”\demo\src\connectedcar\scripts” i den hämtade zip.</span><span class="sxs-lookup"><span data-stu-id="5bb43-290">The following Hive script, named "partitioncarevents.hql", is used for partitioning and is located in the "\demo\src\connectedcar\scripts" folder of the downloaded zip.</span></span> 
    
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

<span data-ttu-id="5bb43-291">När pipelinen har körts, finns följande partitioner som skapas i ditt lagringskonto i ”connectedcar”-behållaren.</span><span class="sxs-lookup"><span data-stu-id="5bb43-291">Once the pipeline is executed successfully, you see the following partitions generated in your storage account under the "connectedcar" container.</span></span>

![Partitionerade utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

<span data-ttu-id="5bb43-293">*Figur 12 - partitionerade utdata*</span><span class="sxs-lookup"><span data-stu-id="5bb43-293">*Figure 12 - Partitioned Output*</span></span>

<span data-ttu-id="5bb43-294">Data optimeras nu, hantera och redo för ytterligare bearbetning för att få omfattande batch insikter.</span><span class="sxs-lookup"><span data-stu-id="5bb43-294">The data is now optimized, is more manageable and ready for further processing to gain rich batch insights.</span></span> 

## <a name="data-analysis"></a><span data-ttu-id="5bb43-295">Dataanalys</span><span class="sxs-lookup"><span data-stu-id="5bb43-295">Data Analysis</span></span>
<span data-ttu-id="5bb43-296">I det här avsnittet lär du dig att kombinera Azure Stream Analytics, Azure Machine Learning, Azure Data Factory och Azure HDInsight för omfattande avancerade analyser på vehicle hälsotillstånd och andra vanor.</span><span class="sxs-lookup"><span data-stu-id="5bb43-296">In this section, you see how to combine Azure Stream Analytics, Azure Machine Learning, Azure Data Factory, and Azure HDInsight for rich advanced analytics on vehicle health and driving habits.</span></span> <span data-ttu-id="5bb43-297">Det finns tre underavsnitt här:</span><span class="sxs-lookup"><span data-stu-id="5bb43-297">There are three subsections here:</span></span>

1. <span data-ttu-id="5bb43-298">**Maskininlärning**: detta avsnitt innehåller information om avvikelseidentifiering identifiering experiment som vi använde i den här lösningen för att förutsäga fordon kräver Underhåll underhåll och fordon som kräver återställning av på grund av problem med säkerheten.</span><span class="sxs-lookup"><span data-stu-id="5bb43-298">**Machine Learning**: This subsection contains information on the anomaly detection experiment that we have used in this solution to predict vehicles requiring servicing maintenance and vehicles requiring recalls due to safety issues.</span></span>
2. <span data-ttu-id="5bb43-299">**Realtid analysis**: detta avsnitt innehåller information om realtidsanalys med Stream Analytics Query Language och operationalizing machine learning-experiment i realtid med ett anpassat program.</span><span class="sxs-lookup"><span data-stu-id="5bb43-299">**Real-time analysis**: This subsection contains information regarding the real-time analytics using the Stream Analytics Query Language and operationalizing the machine learning experiment in real time using a custom application.</span></span>
3. <span data-ttu-id="5bb43-300">**Batch-analys**: detta avsnitt innehåller information om den Omforma och bearbetning av batch-data med Azure HDInsight och Azure Machine Learning operationalized av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5bb43-300">**Batch analysis**: This subsection contains information regarding the transforming and processing of the batch data using Azure HDInsight and Azure Machine Learning operationalized by Azure Data Factory.</span></span>

### <a name="machine-learning"></a><span data-ttu-id="5bb43-301">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5bb43-301">Machine Learning</span></span>
<span data-ttu-id="5bb43-302">Vår avsikten är att förutsäga fordon som kräver underhåll eller återkallas baserat på vissa hed statistik.</span><span class="sxs-lookup"><span data-stu-id="5bb43-302">Our goal here is to predict the vehicles that require maintenance or recall based on certain heath statistics.</span></span> <span data-ttu-id="5bb43-303">Vi göra följande antaganden</span><span class="sxs-lookup"><span data-stu-id="5bb43-303">We make the following assumptions</span></span>

* <span data-ttu-id="5bb43-304">Om något av följande tre villkor är uppfyllda, fordon kräver **servicing Underhåll**:</span><span class="sxs-lookup"><span data-stu-id="5bb43-304">If one of the following three conditions are true, the vehicles require **servicing maintenance**:</span></span>
  
  * <span data-ttu-id="5bb43-305">Däck trycket är låg</span><span class="sxs-lookup"><span data-stu-id="5bb43-305">Tire pressure is low</span></span>
  * <span data-ttu-id="5bb43-306">Motorn olja nivån är låg</span><span class="sxs-lookup"><span data-stu-id="5bb43-306">Engine oil level is low</span></span>
  * <span data-ttu-id="5bb43-307">Motorn är hög</span><span class="sxs-lookup"><span data-stu-id="5bb43-307">Engine temperature is high</span></span>
* <span data-ttu-id="5bb43-308">Om något av följande villkor är uppfyllda, fordon kan ha en **säkerhet problemet** och kräver **återkallning**:</span><span class="sxs-lookup"><span data-stu-id="5bb43-308">If one of the following conditions are true, the vehicles may have a **safety issue** and require **recall**:</span></span>
  
  * <span data-ttu-id="5bb43-309">Motorn temperatur är hög men utanför temperatur är låg</span><span class="sxs-lookup"><span data-stu-id="5bb43-309">Engine temperature is high but outside temperature is low</span></span>
  * <span data-ttu-id="5bb43-310">Motorn temperatur är låg men utanför temperatur är hög</span><span class="sxs-lookup"><span data-stu-id="5bb43-310">Engine temperature is low but outside temperature is high</span></span>

<span data-ttu-id="5bb43-311">Vi har skapat två separata modeller för att identifiera avvikelser, en för identifiering av vehicle underhåll och en för identifiering av vehicle återkallning baserat på de tidigare krav.</span><span class="sxs-lookup"><span data-stu-id="5bb43-311">Based on the previous requirements, we have created two separate models to detect anomalies, one for vehicle maintenance detection, and one for vehicle recall detection.</span></span> <span data-ttu-id="5bb43-312">I båda dessa modeller används algoritmen inbyggda huvudnamn komponenten analys (PCA) för identifiering av avvikelse.</span><span class="sxs-lookup"><span data-stu-id="5bb43-312">In both these models, the built-in Principal Component Analysis (PCA) algorithm is used for anomaly detection.</span></span> 

<span data-ttu-id="5bb43-313">**Underhåll identifiering av modellen**</span><span class="sxs-lookup"><span data-stu-id="5bb43-313">**Maintenance detection model**</span></span>

<span data-ttu-id="5bb43-314">Om något av tre indikatorer däck trycket, motorolja eller motorn temperatur - uppfyller sitt respektive tillstånd, rapporterar Underhåll identifiering av modellen ett fel.</span><span class="sxs-lookup"><span data-stu-id="5bb43-314">If one of three indicators - tire pressure, engine oil, or engine temperature - satisfies its respective condition, the maintenance detection model reports an anomaly.</span></span> <span data-ttu-id="5bb43-315">Därför behöver vi bara att tänka på de här tre variablerna i att skapa modellen.</span><span class="sxs-lookup"><span data-stu-id="5bb43-315">As a result, we only need to consider these three variables in building the model.</span></span> <span data-ttu-id="5bb43-316">I vårt experiment i Azure Machine Learning vi först använda en **Välj kolumner i datauppsättning** modulen att extrahera de här tre variablerna.</span><span class="sxs-lookup"><span data-stu-id="5bb43-316">In our experiment in Azure Machine Learning, we first use a **Select Columns in Dataset** module to extract these three variables.</span></span> <span data-ttu-id="5bb43-317">Vi använder bredvid modulen PCA-baserad avvikelseidentifiering identifiering för att skapa avvikelseidentifiering identifiering modell.</span><span class="sxs-lookup"><span data-stu-id="5bb43-317">Next we use the PCA-based anomaly detection module to build the anomaly detection model.</span></span> 

<span data-ttu-id="5bb43-318">Huvudnamn komponenten analys (PCA) är en etablerad teknik i machine learning som kan tillämpas på val av funktioner, klassificering och avvikelseidentifiering.</span><span class="sxs-lookup"><span data-stu-id="5bb43-318">Principal Component Analysis (PCA) is an established technique in machine learning that can be applied to feature selection, classification, and anomaly detection.</span></span> <span data-ttu-id="5bb43-319">PCA konverterar en uppsättning fallet med eventuellt korrelerade variabler i en uppsättning värden som kallas huvudkomponenter.</span><span class="sxs-lookup"><span data-stu-id="5bb43-319">PCA converts a set of case containing possibly correlated variables, into a set of values called principal components.</span></span> <span data-ttu-id="5bb43-320">Viktiga uppfattning om PCA-baserad modellering är att projektdata till ett lägre endimensionell utrymme så att funktioner och avvikelser mer lätt kan identifieras.</span><span class="sxs-lookup"><span data-stu-id="5bb43-320">The key idea of PCA-based modeling is to project data onto a lower-dimensional space so that features and anomalies can be more easily identified.</span></span>

<span data-ttu-id="5bb43-321">För varje ny indata för identifiering av modellen avvikelseidentifiering detektorn först beräknar dess projektion på eigenvectors och beräknar normaliserade återuppbyggnad-fel.</span><span class="sxs-lookup"><span data-stu-id="5bb43-321">For each new input to  the detection model, the anomaly detector first computes its projection on the eigenvectors, and then computes the normalized reconstruction error.</span></span> <span data-ttu-id="5bb43-322">Felet normaliserade är avvikelseidentifiering poäng.</span><span class="sxs-lookup"><span data-stu-id="5bb43-322">This normalized error is the anomaly score.</span></span> <span data-ttu-id="5bb43-323">Ju högre fel, mer avvikande instansen är.</span><span class="sxs-lookup"><span data-stu-id="5bb43-323">The higher the error, the more anomalous the instance is.</span></span> 

<span data-ttu-id="5bb43-324">I Underhåll identifiering problemet, kan varje post ses som en punkt i 3-dimensionell kan definieras av däck tryck, motorolja och motorn temperatur koordinater.</span><span class="sxs-lookup"><span data-stu-id="5bb43-324">In the maintenance detection problem, each record can be considered as a point in a 3-dimensional space defined by tire pressure, engine oil, and engine temperature coordinates.</span></span> <span data-ttu-id="5bb43-325">För att samla in dessa avvikelser kan vi projicera ursprungliga data i området 3-dimensionell till en 2-dimensionell utrymme med PCA.</span><span class="sxs-lookup"><span data-stu-id="5bb43-325">To capture these anomalies, we can project the original data in the 3-dimensional space onto a 2-dimensional space using PCA.</span></span> <span data-ttu-id="5bb43-326">Därför ange vi parametern antal komponenter som ska användas i PCA 2.</span><span class="sxs-lookup"><span data-stu-id="5bb43-326">Thus, we set the parameter Number of components to use in PCA to be 2.</span></span> <span data-ttu-id="5bb43-327">Den här parametern spelar en viktig roll vid tillämpning av PCA-baserad avvikelseidentifiering.</span><span class="sxs-lookup"><span data-stu-id="5bb43-327">This parameter plays an important role in applying PCA-based anomaly detection.</span></span> <span data-ttu-id="5bb43-328">Efter projicera data med hjälp av PCA, kan vi identifiera dessa avvikelser enklare.</span><span class="sxs-lookup"><span data-stu-id="5bb43-328">After projecting data using PCA, we can identify these anomalies more easily.</span></span>

<span data-ttu-id="5bb43-329">**Återkalla avvikelseidentifiering identifiering av modellen** i modellen återkallning avvikelseidentifiering identifiering vi använda Välj kolumner i datauppsättning och PCA-baserad avvikelseidentifiering identifiering moduler på ett liknande sätt.</span><span class="sxs-lookup"><span data-stu-id="5bb43-329">**Recall anomaly detection model** In the recall anomaly detection model, we use the Select Columns in Dataset and PCA-based anomaly detection modules in a similar way.</span></span> <span data-ttu-id="5bb43-330">Mer specifikt vi först extrahera tre variabler - motorn temperatur utanför temperatur- och hastighet - med hjälp av den **Välj kolumner i datauppsättning** modul.</span><span class="sxs-lookup"><span data-stu-id="5bb43-330">Specifically, we first extract three variables - engine temperature, outside temperature, and speed - using the **Select Columns in Dataset** module.</span></span> <span data-ttu-id="5bb43-331">Vi kan också innehålla hastighet variabeln eftersom motorn temperaturen normalt korreleras till hastighet.</span><span class="sxs-lookup"><span data-stu-id="5bb43-331">We also include the speed variable since the engine temperature typically is correlated to the speed.</span></span> <span data-ttu-id="5bb43-332">Vi använder bredvid PCA-baserad avvikelseidentifiering modulen för villkorsidentifiering för att projicera data från 3-dimensionell utrymme på en 2-dimensionell utrymme.</span><span class="sxs-lookup"><span data-stu-id="5bb43-332">Next we use PCA-based anomaly detection module to project the data from the 3-dimensional space onto a 2-dimensional space.</span></span> <span data-ttu-id="5bb43-333">Återkalla villkoren är uppfyllda och för programuppdatering kräver återkallning när motorn temperatur- och utanför temperatur hög negativt korrelerade.</span><span class="sxs-lookup"><span data-stu-id="5bb43-333">The recall criteria are satisfied and so the vehicle requires recall when engine temperature and outside temperature are highly negatively correlated.</span></span> <span data-ttu-id="5bb43-334">Vi kan samla in avvikelser när du utför PCA med PCA-baserad algoritm för avvikelseidentifiering.</span><span class="sxs-lookup"><span data-stu-id="5bb43-334">Using PCA-based anomaly detection algorithm, we can capture the anomalies after performing PCA.</span></span> 

<span data-ttu-id="5bb43-335">När utbildning antingen modell, som vi behöver använda vanliga data, som inte kräver underhåll eller återkallas som indata för att träna modellen PCA-baserad avvikelseidentifiering identifiering.</span><span class="sxs-lookup"><span data-stu-id="5bb43-335">When training either model, we need to use normal data, which does not require maintenance or recall as the input data to train the PCA-based anomaly detection model.</span></span> <span data-ttu-id="5bb43-336">I bedömningsprofil experiment använda vi utbildade avvikelseidentifiering identifiering av modellen för att identifiera huruvida för programuppdatering kräver underhåll eller återkallas.</span><span class="sxs-lookup"><span data-stu-id="5bb43-336">In the scoring experiment, we use the trained anomaly detection model to detect whether or not the vehicle requires maintenance or recall.</span></span> 

### <a name="real-time-analysis"></a><span data-ttu-id="5bb43-337">Analys i realtid</span><span class="sxs-lookup"><span data-stu-id="5bb43-337">Real-time analysis</span></span>
<span data-ttu-id="5bb43-338">Följande Stream Analytics SQL-fråga används för att hämta medelvärdet av alla viktig vehicle parametrar som hastigheten, bränslenivå, motorn temperatur, mätarställning, däck trycket, motorn olja nivå och andra.</span><span class="sxs-lookup"><span data-stu-id="5bb43-338">The following Stream Analytics SQL Query is used to get the average of all the important vehicle parameters like vehicle speed, fuel level, engine temperature, odometer reading, tire pressure, engine oil level, and others.</span></span> <span data-ttu-id="5bb43-339">Medelvärden används för att identifiera avvikelser, utfärda aviseringar, och bestämma övergripande hälsa villkoren för fordon drivas på ett visst område och korrelera den till demografi.</span><span class="sxs-lookup"><span data-stu-id="5bb43-339">The averages are used to detect anomalies, issue alerts, and determine the overall health conditions of vehicles operated in specific region and then correlate it to demographics.</span></span> 

![Stream Analytics-fråga för realtidsbearbetning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

<span data-ttu-id="5bb43-341">*Figur 13-Stream Analytics-fråga för realtidsbearbetning*</span><span class="sxs-lookup"><span data-stu-id="5bb43-341">*Figure 13 – Stream Analytics query for real-time processing*</span></span>

<span data-ttu-id="5bb43-342">Alla medelvärden beräknas under en TumblingWindow 3 sekunder.</span><span class="sxs-lookup"><span data-stu-id="5bb43-342">All the averages are calculated over a 3-second TumblingWindow.</span></span> <span data-ttu-id="5bb43-343">Vi använder TubmlingWindow i det här fallet eftersom vi kräver inte överlappar och sammanhängande intervall.</span><span class="sxs-lookup"><span data-stu-id="5bb43-343">We are using TubmlingWindow in this case since we require non-overlapping and contiguous time intervals.</span></span> 

<span data-ttu-id="5bb43-344">Mer information om alla funktioner för ”fönsterhantering” i Azure Stream Analytics klickar du på [fönsterhantering (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).</span><span class="sxs-lookup"><span data-stu-id="5bb43-344">To learn more about all the "Windowing" capabilities in Azure Stream Analytics, click [Windowing (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).</span></span>

<span data-ttu-id="5bb43-345">**Realtid förutsägelse**</span><span class="sxs-lookup"><span data-stu-id="5bb43-345">**Real-time prediction**</span></span>

<span data-ttu-id="5bb43-346">Ett program ingår som en del av lösningen för att operationalisera maskininlärning modellen i realtid.</span><span class="sxs-lookup"><span data-stu-id="5bb43-346">An application is included as part of the solution to operationalize the machine learning model in real time.</span></span> <span data-ttu-id="5bb43-347">Det här programmet som kallas ”RealTimeDashboardApp” har skapats och konfigurerats som en del av distributionen av lösningen.</span><span class="sxs-lookup"><span data-stu-id="5bb43-347">This application called “RealTimeDashboardApp” is created and configured as part of the solution deployment.</span></span> <span data-ttu-id="5bb43-348">Programmet gör följande:</span><span class="sxs-lookup"><span data-stu-id="5bb43-348">The application performs the following:</span></span>

1. <span data-ttu-id="5bb43-349">Lyssnar på en Event Hub-instans där Stream Analytics publicerar händelser i ett mönster kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="5bb43-349">Listens to an Event Hub instance where Stream Analytics is publishing the events in a pattern continuously.</span></span> <span data-ttu-id="5bb43-350">![Stream Analytics-fråga för att publicera data](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *figur 14 – Stream Analytics-fråga för att publicera data till ett utgående Event Hub-instans*</span><span class="sxs-lookup"><span data-stu-id="5bb43-350">![Stream Analytics query for publishing the data](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *Figure 14 – Stream Analytics query for publishing the data to an output Event Hub instance*</span></span> 
2. <span data-ttu-id="5bb43-351">För varje händelse som tar emot det här programmet:</span><span class="sxs-lookup"><span data-stu-id="5bb43-351">For every event that this application receives:</span></span> 
   
   * <span data-ttu-id="5bb43-352">Bearbetar data med Machine Learning-svar på begäranden bedömningen (RR) slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="5bb43-352">Processes the data using Machine Learning Request-Response Scoring (RRS) endpoint.</span></span> <span data-ttu-id="5bb43-353">RR-slutpunkten publiceras automatiskt som en del av distributionen.</span><span class="sxs-lookup"><span data-stu-id="5bb43-353">The RRS endpoint is automatically published as part of the deployment.</span></span>
   * <span data-ttu-id="5bb43-354">RR-utdata har publicerats till en Power BI-datamängd med push-API: er.</span><span class="sxs-lookup"><span data-stu-id="5bb43-354">The RRS output is published to a Power BI dataset using the push APIs.</span></span>

<span data-ttu-id="5bb43-355">Det här mönstret gäller även för scenarier där du vill integrera en Line of Business (LoB)-program med realtidsanalys-flödet för scenarier, till exempel aviseringar, meddelanden och meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5bb43-355">This pattern is also applicable to scenarios in which you want to integrate a Line of Business (LoB) application with the real-time analytics flow, for scenarios such as alerts, notifications, and messaging.</span></span>

<span data-ttu-id="5bb43-356">Klicka på [RealtimeDashboardApp download](http://go.microsoft.com/fwlink/?LinkId=717078) att hämta RealtimeDashboardApp Visual Studio-lösning för anpassningar.</span><span class="sxs-lookup"><span data-stu-id="5bb43-356">Click [RealtimeDashboardApp download](http://go.microsoft.com/fwlink/?LinkId=717078) to download the RealtimeDashboardApp Visual Studio solution for customizations.</span></span> 

<span data-ttu-id="5bb43-357">**Att köra programmet realtid instrumentpanelen**</span><span class="sxs-lookup"><span data-stu-id="5bb43-357">**To execute the Real-time Dashboard Application**</span></span>
1. <span data-ttu-id="5bb43-358">Extrahera och spara lokalt ![RealtimeDashboardApp mappen](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *bild 16 – RealtimeDashboardApp mapp*</span><span class="sxs-lookup"><span data-stu-id="5bb43-358">Extract and save locally ![RealtimeDashboardApp folder](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *Figure 16 – RealtimeDashboardApp folder*</span></span>  
2. <span data-ttu-id="5bb43-359">Köra programmet RealtimeDashboardApp.exe</span><span class="sxs-lookup"><span data-stu-id="5bb43-359">Execute the application RealtimeDashboardApp.exe</span></span>
3. <span data-ttu-id="5bb43-360">Ange giltiga autentiseringsuppgifter för Power BI, logga in och klicka på Acceptera</span><span class="sxs-lookup"><span data-stu-id="5bb43-360">Provide valid Power BI credentials, sign in and click Accept</span></span> ![Realtid instrumentpanelen app logga in på Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) ![Realtid instrumentpanelsapp Slutför logga in till Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

<span data-ttu-id="5bb43-363">*Bild 17 – RealtimeDashboardApp: Logga in på Powerbi*</span><span class="sxs-lookup"><span data-stu-id="5bb43-363">*Figure 17 – RealtimeDashboardApp: Sign-in to Power BI*</span></span>

>[!NOTE] 
><span data-ttu-id="5bb43-364">Om du vill rensa Power BI-dataset kör RealtimeDashboardApp med parametern ”flushdata”:</span><span class="sxs-lookup"><span data-stu-id="5bb43-364">If you want to flush the Power BI dataset, execute the RealtimeDashboardApp with the "flushdata" parameter:</span></span> 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a><span data-ttu-id="5bb43-365">Batchanalys</span><span class="sxs-lookup"><span data-stu-id="5bb43-365">Batch analysis</span></span>
<span data-ttu-id="5bb43-366">Avsikten är att visa hur Contoso motorer använder Azure compute-funktioner för att utnyttja stordata för att få detaljerad information på körning mönster, användning beteende och vehicle hälsa.</span><span class="sxs-lookup"><span data-stu-id="5bb43-366">The goal here is to show how Contoso Motors utilizes the Azure compute capabilities to harness big data to gain rich insights on driving pattern, usage behavior, and vehicle health.</span></span> <span data-ttu-id="5bb43-367">Detta gör det möjligt att:</span><span class="sxs-lookup"><span data-stu-id="5bb43-367">This makes it possible to:</span></span>

* <span data-ttu-id="5bb43-368">Förbättra kundupplevelsen och gör den billigare genom att ge insikter på Driver vanor och bränsle effektivt intresseväckande beteenden</span><span class="sxs-lookup"><span data-stu-id="5bb43-368">Improve the customer experience and make it cheaper by providing insights on driving habits and fuel efficient driving behaviors</span></span>
* <span data-ttu-id="5bb43-369">Lär dig att kunder och deras intresseväckande patters att styra affärsbeslut och tillhandahålla bästa i klassen produkter och tjänster</span><span class="sxs-lookup"><span data-stu-id="5bb43-369">Learn proactively about customers and their driving patters to govern business decisions and provide the best in class products & services</span></span>

<span data-ttu-id="5bb43-370">I den här lösningen utvecklar vi följande mått:</span><span class="sxs-lookup"><span data-stu-id="5bb43-370">In this solution, we are targeting the following metrics:</span></span>

1. <span data-ttu-id="5bb43-371">**Styr beteendet aggressivt**: identifierar trend i modeller, platser, intresseväckande villkor och tid för år och få insikter om aggressivt intresseväckande mönster.</span><span class="sxs-lookup"><span data-stu-id="5bb43-371">**Aggressive driving behavior**: Identifies the trend of the models, locations, driving conditions, and time of the year to gain insights on aggressive driving patterns.</span></span> <span data-ttu-id="5bb43-372">Contoso motorer kan använda dessa insikter för marknadsföringskampanjer, driver nya anpassade funktioner och användningsbaserad insurance.</span><span class="sxs-lookup"><span data-stu-id="5bb43-372">Contoso Motors can use these insights for marketing campaigns, driving new personalized features and usage-based insurance.</span></span>
2. <span data-ttu-id="5bb43-373">**Bränsle effektivt intresseväckande beteende**: identifierar trend i modeller, platser, intresseväckande villkor och tid för år och få insikter på bränsle effektivt intresseväckande mönster.</span><span class="sxs-lookup"><span data-stu-id="5bb43-373">**Fuel efficient driving behavior**: Identifies the trend of the models, locations, driving conditions, and time of the year to gain insights on fuel efficient driving patterns.</span></span> <span data-ttu-id="5bb43-374">Contoso motorer kan använda dessa insikter för marknadsföringskampanjer, köra nya funktioner och proaktiv rapporterar till drivrutiner för effektiv och miljö eget intresseväckande vanor.</span><span class="sxs-lookup"><span data-stu-id="5bb43-374">Contoso Motors can use these insights for marketing campaigns, driving new features and proactive reporting to the drivers for cost effective and environment friendly driving habits.</span></span> 
3. <span data-ttu-id="5bb43-375">**Återkalla modeller**: identifierar modeller som kräver återställning av operationalizing den avvikelseidentifiering identifiering maskininlärningsexperiment</span><span class="sxs-lookup"><span data-stu-id="5bb43-375">**Recall models**: Identifies models requiring recalls by operationalizing the anomaly detection machine learning experiment</span></span>

<span data-ttu-id="5bb43-376">Nu ska vi titta i informationen för var och en av de här måtten</span><span class="sxs-lookup"><span data-stu-id="5bb43-376">Let's look into the details of each of these metrics,</span></span>

<span data-ttu-id="5bb43-377">**Aggressiv intresseväckande mönster**</span><span class="sxs-lookup"><span data-stu-id="5bb43-377">**Aggressive driving pattern**</span></span>

<span data-ttu-id="5bb43-378">Partitionerade vehicle signaler och diagnostiska data bearbetas i pipeline med namnet ”AggresiveDrivingPatternPipeline” med Hive för att avgöra modeller, plats, vehicle, intresseväckande villkor och andra parametrar som uppvisar aggressivt körning mönster.</span><span class="sxs-lookup"><span data-stu-id="5bb43-378">The partitioned vehicle signals and diagnostic data are processed in the pipeline named "AggresiveDrivingPatternPipeline" using Hive to determine the models, location, vehicle, driving conditions, and other parameters that exhibits aggressive driving pattern.</span></span>

<span data-ttu-id="5bb43-379">![Aggressiv intresseväckande mönster arbetsflöde](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*bild 18 – aggressiv intresseväckande mönster-arbetsflöde*</span><span class="sxs-lookup"><span data-stu-id="5bb43-379">![Aggressive driving pattern workflow](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*Figure 18 – Aggressive driving pattern workflow*</span></span>


<span data-ttu-id="5bb43-380">***Aggressiv intresseväckande mönster Hive-fråga***</span><span class="sxs-lookup"><span data-stu-id="5bb43-380">***Aggressive driving pattern Hive query***</span></span>

<span data-ttu-id="5bb43-381">Hive-skript med namnet ”aggresivedriving.hql” används för att analysera aggressivt intresseväckande villkoret mönster finns i ”\demo\src\connectedcar\scripts” mappar för hämtade zip.</span><span class="sxs-lookup"><span data-stu-id="5bb43-381">The Hive script named "aggresivedriving.hql" used for analyzing aggressive driving condition pattern is located at "\demo\src\connectedcar\scripts" folder of the downloaded zip.</span></span> 

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


<span data-ttu-id="5bb43-382">Kombinationen av fordon överföring växeln position bromsar cyklar status och hastighet används för att identifiera reckless/aggressivt intresseväckande beteende baserat på bromsar mönster med hög hastighet.</span><span class="sxs-lookup"><span data-stu-id="5bb43-382">It uses the combination of vehicle's transmission gear position, brake pedal status, and speed to detect reckless/aggressive driving behavior based on braking pattern at high speed.</span></span> 

<span data-ttu-id="5bb43-383">När pipelinen har körts, finns följande partitioner som skapas i ditt lagringskonto i ”connectedcar”-behållaren.</span><span class="sxs-lookup"><span data-stu-id="5bb43-383">Once the pipeline is executed successfully, you see the following partitions generated in your storage account under the "connectedcar" container.</span></span>

![AggressiveDrivingPatternPipeline utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 

<span data-ttu-id="5bb43-385">*Bild 19 – AggressiveDrivingPatternPipeline utdata*</span><span class="sxs-lookup"><span data-stu-id="5bb43-385">*Figure 19 – AggressiveDrivingPatternPipeline output*</span></span>

<span data-ttu-id="5bb43-386">**Bränsle effektivt intresseväckande mönster**</span><span class="sxs-lookup"><span data-stu-id="5bb43-386">**Fuel efficient driving pattern**</span></span>

<span data-ttu-id="5bb43-387">Partitionerade vehicle signaler och diagnostiska data bearbetas i pipeline med namnet ”FuelEfficientDrivingPatternPipeline”.</span><span class="sxs-lookup"><span data-stu-id="5bb43-387">The partitioned vehicle signals and diagnostic data are processed in the pipeline named "FuelEfficientDrivingPatternPipeline".</span></span> <span data-ttu-id="5bb43-388">Hive används för att fastställa modeller, plats, vehicle, intresseväckande villkor och andra egenskaper som uppvisar bränsle effektivt intresseväckande mönster.</span><span class="sxs-lookup"><span data-stu-id="5bb43-388">Hive is used to determine the models, location, vehicle, driving conditions, and other properties that exhibit fuel efficient driving pattern.</span></span>

![Bränsleeffektiva intresseväckande mönster](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

<span data-ttu-id="5bb43-390">*Figur 20 – bränsleeffektiva intresseväckande mönster-arbetsflöde*</span><span class="sxs-lookup"><span data-stu-id="5bb43-390">*Figure 20 – Fuel-efficient driving pattern workflow*</span></span>

<span data-ttu-id="5bb43-391">***Bränsle effektivt intresseväckande mönster Hive-fråga***</span><span class="sxs-lookup"><span data-stu-id="5bb43-391">***Fuel efficient driving pattern Hive query***</span></span>

<span data-ttu-id="5bb43-392">Hive-skript med namnet ”fuelefficientdriving.hql” används för att analysera aggressivt intresseväckande villkoret mönster finns i ”\demo\src\connectedcar\scripts” mappar för hämtade zip.</span><span class="sxs-lookup"><span data-stu-id="5bb43-392">The Hive script named "fuelefficientdriving.hql" used for analyzing aggressive driving condition pattern is located at "\demo\src\connectedcar\scripts" folder of the downloaded zip.</span></span> 

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


<span data-ttu-id="5bb43-393">Det används en kombination av fordon överföring växeln position, bromsar cyklar status, hastighet och accelerator pedal möjlighet att identifiera bränsle effektivt intresseväckande beteende baserat på acceleration, bromsar och processorhastighet mönster.</span><span class="sxs-lookup"><span data-stu-id="5bb43-393">It uses the combination of vehicle's transmission gear position, brake pedal status, speed, and accelerator pedal position to detect fuel efficient driving behavior based on acceleration, braking, and speed patterns.</span></span> 

<span data-ttu-id="5bb43-394">När pipelinen har körts, finns följande partitioner som skapas i ditt lagringskonto i ”connectedcar”-behållaren.</span><span class="sxs-lookup"><span data-stu-id="5bb43-394">Once the pipeline is executed successfully, you see the following partitions generated in your storage account under the "connectedcar" container.</span></span>

![FuelEfficientDrivingPatternPipeline utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

<span data-ttu-id="5bb43-396">*Figur 21 – FuelEfficientDrivingPatternPipeline utdata*</span><span class="sxs-lookup"><span data-stu-id="5bb43-396">*Figure 21 – FuelEfficientDrivingPatternPipeline output*</span></span>

<span data-ttu-id="5bb43-397">**Återkalla förutsägelser**</span><span class="sxs-lookup"><span data-stu-id="5bb43-397">**Recall Predictions**</span></span>

<span data-ttu-id="5bb43-398">Den maskininlärningsexperiment etablerad och publiceras som en webbtjänst som en del av distributionen av lösningen.</span><span class="sxs-lookup"><span data-stu-id="5bb43-398">The machine learning experiment is provisioned and published as a web service as part of the solution deployment.</span></span> <span data-ttu-id="5bb43-399">Slutpunkten för batchbedömningsjobbet utnyttjas i det här arbetsflödet, registrerats som data factory länkad tjänst och operationalized med hjälp av data factory-batchbedömningsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="5bb43-399">The batch scoring end point is leveraged in this workflow, registered as a data factory linked service and operationalized using data factory batch scoring activity.</span></span>

![Machine Learning-slutpunkt](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

<span data-ttu-id="5bb43-401">*Figur 22 – Machine learning-slutpunkt som är registrerat som en länkad tjänst i data factory*</span><span class="sxs-lookup"><span data-stu-id="5bb43-401">*Figure 22 – Machine learning endpoint registered as a linked service in data factory*</span></span>

<span data-ttu-id="5bb43-402">Den registrerade länkade tjänsten används i DetectAnomalyPipeline för att samla in data med hjälp av avvikelseidentifiering identifiering av modellen.</span><span class="sxs-lookup"><span data-stu-id="5bb43-402">The registered linked service is used in the DetectAnomalyPipeline to score the data using the anomaly detection model.</span></span> 

![Datorn Learning batchbedömningsaktivitet i data factory](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png) 

<span data-ttu-id="5bb43-404">*Figur 23 – Azure Machine Learning-Batchbedömningen aktivitet i data factory*</span><span class="sxs-lookup"><span data-stu-id="5bb43-404">*Figure 23 – Azure Machine Learning Batch Scoring activity in data factory*</span></span> 

<span data-ttu-id="5bb43-405">Det finns några steg utförs i den här pipelinen för förberedelse av data så att den kan operationalized med av webbtjänsten för batchbedömningsjobbet.</span><span class="sxs-lookup"><span data-stu-id="5bb43-405">There are few steps performed in this pipeline for data preparation so that it can be operationalized with the batch scoring web service.</span></span> 

![DetectAnomalyPipeline för att förutsäga fordon som kräver återställning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png) 

<span data-ttu-id="5bb43-407">*Figur 24 – DetectAnomalyPipeline för att förutsäga fordon som kräver återställning*</span><span class="sxs-lookup"><span data-stu-id="5bb43-407">*Figure 24 – DetectAnomalyPipeline for predicting vehicles requiring recalls*</span></span> 

<span data-ttu-id="5bb43-408">***Avvikelseidentifiering identifiering Hive-fråga***</span><span class="sxs-lookup"><span data-stu-id="5bb43-408">***Anomaly detection Hive query***</span></span>

<span data-ttu-id="5bb43-409">När den bedömningen har slutförts för aktiviteten HDInsight att bearbeta och aggregera data som är kategoriserade som avvikelser av modellen med en sannolikhet poängen för 0,60 eller högre.</span><span class="sxs-lookup"><span data-stu-id="5bb43-409">Once the scoring is completed, an HDInsight activity is used to process and aggregate the data that are categorized as anomalies by the model with a probability score of 0.60 or higher.</span></span>

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


<span data-ttu-id="5bb43-410">När pipelinen har körts, finns följande partitioner som skapas i ditt lagringskonto i ”connectedcar”-behållaren.</span><span class="sxs-lookup"><span data-stu-id="5bb43-410">Once the pipeline is executed successfully, you see the following partitions generated in your storage account under the "connectedcar" container.</span></span>

![DetectAnomalyPipeline utdata](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

<span data-ttu-id="5bb43-412">*Bild 25 – DetectAnomalyPipeline utdata*</span><span class="sxs-lookup"><span data-stu-id="5bb43-412">*Figure 25 – DetectAnomalyPipeline output*</span></span>

## <a name="publish"></a><span data-ttu-id="5bb43-413">Publicera</span><span class="sxs-lookup"><span data-stu-id="5bb43-413">Publish</span></span>

### <a name="real-time-analysis"></a><span data-ttu-id="5bb43-414">Analys i realtid</span><span class="sxs-lookup"><span data-stu-id="5bb43-414">Real-time analysis</span></span>
<span data-ttu-id="5bb43-415">En av frågorna i Stream Analytics-jobbet publicerar händelser till utdata Event Hub-instans.</span><span class="sxs-lookup"><span data-stu-id="5bb43-415">One of the queries in the Stream Analytics job publishes the events to an output Event Hub instance.</span></span> 

![Stream Analytics-jobbet publicerar till utdata Event Hub-instans](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

<span data-ttu-id="5bb43-417">*Bild 26 – Stream Analytics-jobbet publicerar till utdata Event Hub-instans*</span><span class="sxs-lookup"><span data-stu-id="5bb43-417">*Figure 26 – Stream Analytics job publishes to an output Event Hub instance*</span></span>

![Stream Analytics-fråga för att publicera till utdata Event Hub-instans](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

<span data-ttu-id="5bb43-419">*Bild 27 – Stream Analytics-fråga för att publicera till utdata Event Hub-instans*</span><span class="sxs-lookup"><span data-stu-id="5bb43-419">*Figure 27 – Stream Analytics query to publish to the output Event Hub instance*</span></span>

<span data-ttu-id="5bb43-420">Den här dataströmmen av händelser som förbrukas av RealTimeDashboardApp som ingår i lösningen.</span><span class="sxs-lookup"><span data-stu-id="5bb43-420">This stream of events is consumed by the RealTimeDashboardApp included in the solution.</span></span> <span data-ttu-id="5bb43-421">Det här programmet utnyttjar webbtjänsten för Machine Learning begäran och svar för realtid poäng och publicerar den resulterande data till en Power BI-datamängd för användning.</span><span class="sxs-lookup"><span data-stu-id="5bb43-421">This application leverages the Machine Learning Request-Response web service for real-time scoring and publishes the resultant data to a Power BI dataset for consumption.</span></span> 

### <a name="batch-analysis"></a><span data-ttu-id="5bb43-422">Batchanalys</span><span class="sxs-lookup"><span data-stu-id="5bb43-422">Batch analysis</span></span>
<span data-ttu-id="5bb43-423">Resultatet av batch- och realtidsbearbetning publiceras till Azure SQL Database-tabeller för användning.</span><span class="sxs-lookup"><span data-stu-id="5bb43-423">The results of the batch and real-time processing are published to the Azure SQL Database tables for consumption.</span></span> <span data-ttu-id="5bb43-424">Azure SQL Server-databasen och tabeller skapas automatiskt som en del av installationsskriptet.</span><span class="sxs-lookup"><span data-stu-id="5bb43-424">The Azure SQL Server, Database, and the tables are created automatically as part of the setup script.</span></span> 

![Kopiera resultaten till arbetsflödet för data mart-batchbearbetning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

<span data-ttu-id="5bb43-426">*Bild 28 – kopiera resultaten till arbetsflödet för data mart-batchbearbetning*</span><span class="sxs-lookup"><span data-stu-id="5bb43-426">*Figure 28 – Batch processing results copy to data mart workflow*</span></span>

![Stream Analytics-jobbet publicerar till dataarkiv](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

<span data-ttu-id="5bb43-428">*Bild 29 – Stream Analytics-jobbet publicerar till dataarkiv*</span><span class="sxs-lookup"><span data-stu-id="5bb43-428">*Figure 29 – Stream Analytics job publishes to data mart*</span></span>

![Data mart-inställningen i Stream Analytics-jobbet](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

<span data-ttu-id="5bb43-430">*Bild 30 – dataarkiv i Stream Analytics-jobbet*</span><span class="sxs-lookup"><span data-stu-id="5bb43-430">*Figure 30 – Data mart setting in Stream Analytics job*</span></span>

## <a name="consume"></a><span data-ttu-id="5bb43-431">Förbruka</span><span class="sxs-lookup"><span data-stu-id="5bb43-431">Consume</span></span>
<span data-ttu-id="5bb43-432">Powerbi ger den här lösningen en omfattande instrumentpanel för data i realtid och förutsägelseanalys visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="5bb43-432">Power BI gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span> 

<span data-ttu-id="5bb43-433">Klicka här för detaljerade anvisningar om hur du skapar Power BI-rapporter och på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="5bb43-433">Click here for detailed instructions on setting up the Power BI reports and the dashboard.</span></span> <span data-ttu-id="5bb43-434">Den slutliga instrumentpanelen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="5bb43-434">The final dashboard looks like this:</span></span>

![Power BI-instrumentpanel](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

<span data-ttu-id="5bb43-436">*Bild 31 - Power BI-instrumentpanel*</span><span class="sxs-lookup"><span data-stu-id="5bb43-436">*Figure 31 - Power BI Dashboard*</span></span>

## <a name="summary"></a><span data-ttu-id="5bb43-437">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5bb43-437">Summary</span></span>
<span data-ttu-id="5bb43-438">Det här dokumentet innehåller en detaljerad nedåt Vehicle telemetri Analytics lösning.</span><span class="sxs-lookup"><span data-stu-id="5bb43-438">This document contains a detailed drill-down of the Vehicle Telemetry Analytics Solution.</span></span> <span data-ttu-id="5bb43-439">Detta prov på ett lambda-arkitektur mönster för realtid och batch-analytics med förutsägelser och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="5bb43-439">This showcases a lambda architecture pattern for real-time and batch analytics with predictions and actions.</span></span> <span data-ttu-id="5bb43-440">Det här mönstret som gäller för en mängd olika användningsområden som kräver varm sökväg (realtid) och kalla sökväg (batch) analyser.</span><span class="sxs-lookup"><span data-stu-id="5bb43-440">This pattern applies to a wide range of use cases that require hot path (real-time) and cold path (batch) analytics.</span></span> 

