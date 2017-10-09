---
title: aaaCustomize Azure IoT Suite anslutna factory | Microsoft Docs
description: "En beskrivning av hur toocustomize hello funktionssätt hello anslutna factory förkonfigurerade lösningen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: c#
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 53f2fef7a76b5d8e6ad023945a7812dc7fabd12c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-how-hello-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a><span data-ttu-id="1a271-103">Anpassa hur hello anslutna fabriksinställningarna lösning visar data från dina OPC UA-servrar</span><span class="sxs-lookup"><span data-stu-id="1a271-103">Customize how hello connected factory solution displays data from your OPC UA servers</span></span>

## <a name="introduction"></a><span data-ttu-id="1a271-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="1a271-104">Introduction</span></span>

<span data-ttu-id="1a271-105">hello anslutna factory lösning sammanställer och visar data från hello OPC UA servrar anslutna toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-105">hello connected factory solution aggregates and displays data from hello OPC UA servers connected toohello solution.</span></span> <span data-ttu-id="1a271-106">Du kan bläddra och skicka kommandon toohello OPC UA servrar i din lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-106">You can browse and send commands toohello OPC UA servers in your solution.</span></span> <span data-ttu-id="1a271-107">Mer information om OPC UA finns hello [anslutna factory vanliga frågor och svar](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="1a271-107">For more information about OPC UA, see hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>

<span data-ttu-id="1a271-108">Exempel på sammanställda data i hello lösningen är hello övergripande utrustning effektivitet (OEE) och nyckeln (nyckeltal) som du kan visa i hello instrumentpanelen på hello fabriken, rad och station nivåer.</span><span class="sxs-lookup"><span data-stu-id="1a271-108">Examples of aggregated data in hello solution include hello Overall Equipment Efficiency (OEE) and Key Performance Indicators (KPIs) that you can view in hello dashboard at hello factory, line, and station levels.</span></span> <span data-ttu-id="1a271-109">hello följande skärmbild visar hello OEE och KPI-värden för hello **sammansättningen** station på **produktion rad 1**, i hello **München** fabriken:</span><span class="sxs-lookup"><span data-stu-id="1a271-109">hello following screenshot shows hello OEE and KPI values for hello **Assembly** station, on **Production line 1**, in hello **Munich** factory:</span></span>

![Exempel på OEE och KPI-värden i hello-lösning][img-oee-kpi]

<span data-ttu-id="1a271-111">hello lösning aktiverar du tooview detaljerad information från specifika dataobjekt från hello OPC UA servrar, kallade *stationer*.</span><span class="sxs-lookup"><span data-stu-id="1a271-111">hello solution enables you tooview detailed information from specific data items from hello OPC UA servers, called *stations*.</span></span> <span data-ttu-id="1a271-112">hello visar följande skärmbild områden hello antalet tillverkade artiklar från en specifik station:</span><span class="sxs-lookup"><span data-stu-id="1a271-112">hello following screenshot shows plots of hello number of manufactured items from a specific station:</span></span>

![Områden av antal tillverkade artiklar][img-manufactured-items]

<span data-ttu-id="1a271-114">Om du klickar på en av hello diagram, kan du utforska hello data ytterligare med tiden serien insikter (TSD):</span><span class="sxs-lookup"><span data-stu-id="1a271-114">If you click one of hello graphs, you can explore hello data further using Time Series Insights (TSI):</span></span>

![Utforska data med hjälp av tid serien insikter][img-tsi]

<span data-ttu-id="1a271-116">Den här artikeln beskrivs:</span><span class="sxs-lookup"><span data-stu-id="1a271-116">This article describes:</span></span>

- <span data-ttu-id="1a271-117">Hur görs hello data tillgängliga toohello olika vyer i hello lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-117">How hello data is made available toohello various views in hello solution.</span></span>
- <span data-ttu-id="1a271-118">Hur du kan anpassa hello sätt hello lösning visar hello data.</span><span class="sxs-lookup"><span data-stu-id="1a271-118">How you can customize hello way hello solution displays hello data.</span></span>

## <a name="data-sources"></a><span data-ttu-id="1a271-119">Datakällor</span><span class="sxs-lookup"><span data-stu-id="1a271-119">Data sources</span></span>

<span data-ttu-id="1a271-120">Hej anslutna fabriksinställningarna lösning visar data från hello OPC UA anslutna servrar toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-120">hello connected factory solution displays data from hello OPC UA servers connected toohello solution.</span></span> <span data-ttu-id="1a271-121">hello standardinstallationen omfattar flera OPC UA-servrar som kör en fabrik simulering.</span><span class="sxs-lookup"><span data-stu-id="1a271-121">hello default installation includes several OPC UA servers running a factory simulation.</span></span> <span data-ttu-id="1a271-122">Du kan lägga till egna OPC UA-servrar som [ansluta via en gateway] [ lnk-connect-cf] tooyour lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-122">You can add your own OPC UA servers that [connect through a gateway][lnk-connect-cf] tooyour solution.</span></span>

<span data-ttu-id="1a271-123">Du kan bläddra hello dataobjekt att en anslutna OPC UA-servern kan skicka tooyour lösning i hello instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="1a271-123">You can browse hello data items that a connected OPC UA server can send tooyour solution in hello dashboard:</span></span>

1. <span data-ttu-id="1a271-124">Navigera toohello **väljer du en OPC UA server** vy:</span><span class="sxs-lookup"><span data-stu-id="1a271-124">Navigate toohello **Select an OPC UA server** view:</span></span>

    ![Navigera toohello Välj vyn OPC UA server][img-select-server]

1. <span data-ttu-id="1a271-126">Välj en server och klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="1a271-126">Select a server and click **Connect**.</span></span> <span data-ttu-id="1a271-127">Klicka på **Fortsätt** när hello säkerhetsvarning visas.</span><span class="sxs-lookup"><span data-stu-id="1a271-127">Click **Proceed** when hello security warning appears.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1a271-128">Den här varningen visas en gång för varje server endast och upprättar en förtroenderelation mellan hello lösning instrumentpanelen och hello-servern.</span><span class="sxs-lookup"><span data-stu-id="1a271-128">This warning only appears once for each server and establishes a trust relationship between hello solution dashboard and hello server.</span></span>

1. <span data-ttu-id="1a271-129">Nu kan du bläddra hello dataobjekt som hello servern kan skicka toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-129">You can now browse hello data items that hello server can send toohello solution.</span></span> <span data-ttu-id="1a271-130">Objekt som skickas toohello lösning har en grön bock:</span><span class="sxs-lookup"><span data-stu-id="1a271-130">Items that are being sent toohello solution have a green check mark:</span></span>

    ![Publicerade objekt][img-published]

1. <span data-ttu-id="1a271-132">Om du är en *administratör* i hello-lösning kan du välja toopublish toomake en data-objekt den tillgängliga i hello ansluten factory lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-132">If you are an *Administrator* in hello solution, you can choose toopublish a data item toomake it available in hello connected factory solution.</span></span> <span data-ttu-id="1a271-133">Som administratör kan du också ändra hello värdet på dataobjekt och anropa metoder i hello OPC UA server.</span><span class="sxs-lookup"><span data-stu-id="1a271-133">As an Administrator, you can also change hello value of data items and call methods in hello OPC UA server.</span></span>

## <a name="map-hello-data"></a><span data-ttu-id="1a271-134">Mappa hello data</span><span class="sxs-lookup"><span data-stu-id="1a271-134">Map hello data</span></span>

<span data-ttu-id="1a271-135">hello anslutna factory lösning maps och mängder hello publicerade dataobjekt från hello OPC UA server toohello olika vyer i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-135">hello connected factory solution maps and aggregates hello published data items from hello OPC UA server toohello various views in hello solution.</span></span> <span data-ttu-id="1a271-136">hello distribuerar anslutna factory lösning tooyour Azure-konto när du etablerar hello lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-136">hello connected factory solution deploys tooyour Azure account when you provision hello solution.</span></span> <span data-ttu-id="1a271-137">Mappningsinformationen lagras i en JSON-fil i hello Visual Studio anslutna factory-lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-137">A JSON file in hello Visual Studio connected factory solution stores this mapping information.</span></span> <span data-ttu-id="1a271-138">Du kan visa och ändra den här JSON-konfigurationsfil i hello anslutna factory Visual Studio-lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-138">You can view and modify this JSON configuration file in hello connected factory Visual Studio solution.</span></span> <span data-ttu-id="1a271-139">Du kan distribuera hello lösning när du har gjort en ändring.</span><span class="sxs-lookup"><span data-stu-id="1a271-139">You can redeploy hello solution after you make a change.</span></span>

<span data-ttu-id="1a271-140">Du kan använda hello-konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="1a271-140">You can use hello configuration file to:</span></span>

- <span data-ttu-id="1a271-141">Redigera hello befintliga simulerade fabriker och produktion rader stationer.</span><span class="sxs-lookup"><span data-stu-id="1a271-141">Edit hello existing simulated factories, production lines, and stations.</span></span>
- <span data-ttu-id="1a271-142">Mappa data från verkliga OPC UA-servrar som du ansluter toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-142">Map data from real OPC UA servers that you connect toohello solution.</span></span>

<span data-ttu-id="1a271-143">tooclone en kopia av hello ansluten factory Visual Studio-lösning, Använd hello följande git-kommando:</span><span class="sxs-lookup"><span data-stu-id="1a271-143">tooclone a copy of hello connected factory Visual Studio solution, use hello following git command:</span></span>

`git clone https://github.com/Azure/azure-iot-connected-factory.git`

<span data-ttu-id="1a271-144">hello filen **ContosoTopologyDescription.json** definierar hello mappning från hello OPC UA serverdata objekt toohello vyer i hello anslutna factory lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="1a271-144">hello file **ContosoTopologyDescription.json** defines hello mapping from hello OPC UA server data items toohello views in hello connected factory solution dashboard.</span></span> <span data-ttu-id="1a271-145">Du hittar den här konfigurationsfilen i hello **Contoso\Topology** mapp i hello **WebApp** projekt i hello Visual Studio-lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-145">You can find this configuration file in hello **Contoso\Topology** folder in hello **WebApp** project in hello Visual Studio solution.</span></span>

<span data-ttu-id="1a271-146">hello innehållet hello JSON-filen har ordnats som en hierarki av fabriken, produktionen och station noder.</span><span class="sxs-lookup"><span data-stu-id="1a271-146">hello content of hello JSON file is organized as a hierarchy of factory, production line, and station nodes.</span></span> <span data-ttu-id="1a271-147">Den här hierarkin definierar hello Navigeringshierarki i hello anslutna factory instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="1a271-147">This hierarchy defines hello navigation hierarchy in hello connected factory dashboard.</span></span> <span data-ttu-id="1a271-148">Värden på varje nod i hierarkin hello avgör hello informationen som visas i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="1a271-148">Values at each node of hello hierarchy determine hello information displayed in hello dashboard.</span></span> <span data-ttu-id="1a271-149">Hello JSON-fil innehåller till exempel följande värden för hello München factory hello:</span><span class="sxs-lookup"><span data-stu-id="1a271-149">For example, hello JSON file contains hello following values for hello Munich factory:</span></span>

```json
"Guid": "73B534AE-7C7E-4877-B826-F1C0EA339F65",
"Name": "Munich",
"Description": "Braking system",
"Location": {
    "City": "Munich",
    "Country": "Germany",
    "Latitude": 48.13641,
    "Longitude": 11.57754
},
"Image": "munich.jpg"
```

<span data-ttu-id="1a271-150">hello namn, beskrivning och plats visas på den här vyn i instrumentpanelen för hello:</span><span class="sxs-lookup"><span data-stu-id="1a271-150">hello name, description, and location appear on this view in hello dashboard:</span></span>

![München data i hello instrumentpanelen][img-munich]

<span data-ttu-id="1a271-152">Varje fabriken, produktionen och station har ett image-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="1a271-152">Each factory, production line, and station have an image property.</span></span> <span data-ttu-id="1a271-153">Du hittar dessa JPEG-filer i hello **Content\img** mapp i hello **WebApp** projekt.</span><span class="sxs-lookup"><span data-stu-id="1a271-153">You can find these JPEG files in hello **Content\img** folder in hello **WebApp** project.</span></span> <span data-ttu-id="1a271-154">Dessa bildfiler visas hello anslutna factory instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="1a271-154">These image files display in hello connected factory dashboard.</span></span>

<span data-ttu-id="1a271-155">Varje station innehåller flera detaljerade egenskaper som definierar hello mappning från hello OPC UA dataobjekt.</span><span class="sxs-lookup"><span data-stu-id="1a271-155">Each station includes several detailed properties that define hello mapping from hello OPC UA data items.</span></span> <span data-ttu-id="1a271-156">De här egenskaperna beskrivs i följande avsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="1a271-156">These properties are described in hello following sections:</span></span>

### <a name="opcuri"></a><span data-ttu-id="1a271-157">OpcUri</span><span class="sxs-lookup"><span data-stu-id="1a271-157">OpcUri</span></span>

<span data-ttu-id="1a271-158">Hej **OpcUri** värdet är hello OPC UA programmet URI som unikt identifierar hello OPC UA server.</span><span class="sxs-lookup"><span data-stu-id="1a271-158">hello **OpcUri** value is hello OPC UA Application URI that uniquely identifies hello OPC UA server.</span></span> <span data-ttu-id="1a271-159">Till exempel hello **OpcUri** värde hello sammansättningen station på produktion rad 1 i München ser ut som följande hello: **urn: scada2194:ua:munich:productionline0:assemblystation**.</span><span class="sxs-lookup"><span data-stu-id="1a271-159">For example, hello **OpcUri** value for hello assembly station on production line 1 in Munich looks like hello following: **urn:scada2194:ua:munich:productionline0:assemblystation**.</span></span>

<span data-ttu-id="1a271-160">Du kan visa hello URI: er hello anslutna OPC UA servrar i instrumentpanelen för hello-lösningen:</span><span class="sxs-lookup"><span data-stu-id="1a271-160">You can view hello URIs of hello connected OPC UA servers in hello solution dashboard:</span></span>

![Visa OPC UA server URI: er][img-server-uris]

### <a name="simulation"></a><span data-ttu-id="1a271-162">Simulering</span><span class="sxs-lookup"><span data-stu-id="1a271-162">Simulation</span></span>

<span data-ttu-id="1a271-163">Hej informationen i hello **simuleringen** noden är särskilda toohello OPC UA simuleringen som körs i hello OPC UA servrar som tillhandahålls som standard.</span><span class="sxs-lookup"><span data-stu-id="1a271-163">hello information in hello **Simulation** node is specific toohello OPC UA simulation that runs in hello OPC UA servers that are provisioned by default.</span></span> <span data-ttu-id="1a271-164">Den används inte för en verklig OPC UA-server.</span><span class="sxs-lookup"><span data-stu-id="1a271-164">It is not used for a real OPC UA server.</span></span>

### <a name="kpi1-and-kpi2"></a><span data-ttu-id="1a271-165">Kpi1 och Kpi2</span><span class="sxs-lookup"><span data-stu-id="1a271-165">Kpi1 and Kpi2</span></span>

<span data-ttu-id="1a271-166">Dessa noder beskrivs hur data från hello station bidrar toohello två KPI-värden i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="1a271-166">These nodes describe how data from hello station contributes toohello two KPI values in hello dashboard.</span></span> <span data-ttu-id="1a271-167">Dessa KPI-värden stöds i en standarddistribution, enheter per timme och kWh per timme.</span><span class="sxs-lookup"><span data-stu-id="1a271-167">In a default deployment, these KPI values are units per hour and kWh per hour.</span></span> <span data-ttu-id="1a271-168">hello lösning beräknas KPI vales på en station hello nivå och sammanställer dem på fabriken nivå och hello produktionsrad.</span><span class="sxs-lookup"><span data-stu-id="1a271-168">hello solution calculates KPI vales at hello level of a station and aggregates them at hello production line and factory levels.</span></span>

<span data-ttu-id="1a271-169">Varje KPI har en lägsta, högsta och målvärdet.</span><span class="sxs-lookup"><span data-stu-id="1a271-169">Each KPI has a minimum, maximum, and target value.</span></span> <span data-ttu-id="1a271-170">Varje KPI-värdet kan också definiera aviseringsåtgärder för hello anslutna factory lösning tooperform.</span><span class="sxs-lookup"><span data-stu-id="1a271-170">Each KPI value can also define alert actions for hello connected factory solution tooperform.</span></span> <span data-ttu-id="1a271-171">hello följande utdrag visar hello KPI definitioner för hello sammansättningen station produktion rad 1 i München:</span><span class="sxs-lookup"><span data-stu-id="1a271-171">hello following snippet shows hello KPI definitions for hello assembly station on production line 1 in Munich:</span></span>

```json
"Kpi1": {
  "Minimum": 150,
  "Target": 300,
  "Maximum": 600
},
"Kpi2": {
  "Minimum": 50,
  "Target": 100,
  "Maximum": 200,
  "MinimumAlertActions": [
    {
      "Type": "None"
    }
  ]
}
```

<span data-ttu-id="1a271-172">hello följande skärmbild visar hello KPI data i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="1a271-172">hello following screenshot shows hello KPI data in hello dashboard.</span></span>

![KPI-information i hello-instrumentpanelen][lnk-kpi]

### <a name="opcnodes"></a><span data-ttu-id="1a271-174">OpcNodes</span><span class="sxs-lookup"><span data-stu-id="1a271-174">OpcNodes</span></span>

<span data-ttu-id="1a271-175">Hej **OpcNodes** noder identifiera hello publicerade dataobjekt från hello OPC UA server och ange hur tooprocess data.</span><span class="sxs-lookup"><span data-stu-id="1a271-175">hello **OpcNodes** nodes identify hello published data items from hello OPC UA server and specify how tooprocess that data.</span></span>

<span data-ttu-id="1a271-176">Hej **NodeId** värdet identifierar hello specifika OPC UA NodeID från hello OPC UA server.</span><span class="sxs-lookup"><span data-stu-id="1a271-176">hello **NodeId** value identifies hello specific OPC UA NodeID from hello OPC UA server.</span></span> <span data-ttu-id="1a271-177">hello första noden i hello sammansättningen station för produktion rad 1 i München har värdet **ns = 2, i = 385**.</span><span class="sxs-lookup"><span data-stu-id="1a271-177">hello first node in hello assembly station for production line 1 in Munich has a value **ns=2;i=385**.</span></span> <span data-ttu-id="1a271-178">En **NodeId** värdet anger hello data objektet tooread från hello OPC UA server och hello **SymbolicName** innehåller ett användarvänligt namn toouse hello instrumentpanelen för dessa data.</span><span class="sxs-lookup"><span data-stu-id="1a271-178">A **NodeId** value specifies hello data item tooread from hello OPC UA server, and hello **SymbolicName** provides a user-friendly name toouse in hello dashboard for that data.</span></span>

<span data-ttu-id="1a271-179">Andra värden som är kopplade till varje nod sammanfattas i följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="1a271-179">Other values associated with each node are summarized in hello following table:</span></span>

| <span data-ttu-id="1a271-180">Värde</span><span class="sxs-lookup"><span data-stu-id="1a271-180">Value</span></span> | <span data-ttu-id="1a271-181">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1a271-181">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="1a271-182">Relevans</span><span class="sxs-lookup"><span data-stu-id="1a271-182">Relevance</span></span>  | <span data-ttu-id="1a271-183">hello KPI och OEE värden informationen bidrar till.</span><span class="sxs-lookup"><span data-stu-id="1a271-183">hello KPI and OEE values this data contributes to.</span></span> |
| <span data-ttu-id="1a271-184">OpCode</span><span class="sxs-lookup"><span data-stu-id="1a271-184">OpCode</span></span>     | <span data-ttu-id="1a271-185">Hur hello informationen sammanställs.</span><span class="sxs-lookup"><span data-stu-id="1a271-185">How hello data is aggregated.</span></span> |
| <span data-ttu-id="1a271-186">Enheter</span><span class="sxs-lookup"><span data-stu-id="1a271-186">Units</span></span>      | <span data-ttu-id="1a271-187">hello enheter toouse hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="1a271-187">hello units toouse in hello dashboard.</span></span>  |
| <span data-ttu-id="1a271-188">Synliga</span><span class="sxs-lookup"><span data-stu-id="1a271-188">Visible</span></span>    | <span data-ttu-id="1a271-189">Om toodisplay detta värde i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="1a271-189">Whether toodisplay this value in hello dashboard.</span></span> <span data-ttu-id="1a271-190">Vissa värden beräkningar men visas inte.</span><span class="sxs-lookup"><span data-stu-id="1a271-190">Some values are used in calculations but not displayed.</span></span>  |
| <span data-ttu-id="1a271-191">Maximalt</span><span class="sxs-lookup"><span data-stu-id="1a271-191">Maximum</span></span>    | <span data-ttu-id="1a271-192">hello maximalt värde som utlöser en avisering i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="1a271-192">hello maximum value that triggers an alert in hello dashboard.</span></span> |
| <span data-ttu-id="1a271-193">MaximumAlertActions</span><span class="sxs-lookup"><span data-stu-id="1a271-193">MaximumAlertActions</span></span> | <span data-ttu-id="1a271-194">En åtgärd tootake i svaret tooan avisering.</span><span class="sxs-lookup"><span data-stu-id="1a271-194">An action tootake in response tooan alert.</span></span> <span data-ttu-id="1a271-195">Till exempel skicka en kommandot tooa station.</span><span class="sxs-lookup"><span data-stu-id="1a271-195">For example, send a command tooa station.</span></span> |
| <span data-ttu-id="1a271-196">ConstValue</span><span class="sxs-lookup"><span data-stu-id="1a271-196">ConstValue</span></span> | <span data-ttu-id="1a271-197">Ett konstantvärde som används i en beräkning.</span><span class="sxs-lookup"><span data-stu-id="1a271-197">A constant value used in a calculation.</span></span> |

## <a name="deploy-hello-changes"></a><span data-ttu-id="1a271-198">Distribuera hello ändringar</span><span class="sxs-lookup"><span data-stu-id="1a271-198">Deploy hello changes</span></span>

<span data-ttu-id="1a271-199">När du är klar med ändringarna toohello **ContosoTopologyDescription.json** -fil, måste du distribuera hello anslutna factory lösning tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="1a271-199">When you have finished making changes toohello **ContosoTopologyDescription.json** file, you must redeploy hello connected factory solution tooyour Azure account.</span></span>

<span data-ttu-id="1a271-200">Hej **azure iot-ansluten-fabrik** databasen innehåller en **build.ps1** PowerShell-skript som du kan använda toorebuild och distribuera hello lösning.</span><span class="sxs-lookup"><span data-stu-id="1a271-200">hello **azure-iot-connected-factory** repository includes a **build.ps1** PowerShell script you can use toorebuild and deploy hello solution.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a271-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a271-201">Next Steps</span></span>

<span data-ttu-id="1a271-202">Läs mer om hello anslutna factory förkonfigurerade lösningen genom att läsa hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="1a271-202">Learn more about hello connected factory preconfigured solution by reading hello following articles:</span></span>

* <span data-ttu-id="1a271-203">[Genomgång av den förkonfigurerade lösningen Ansluten fabrik][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="1a271-203">[Connected factory preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="1a271-204">[Distribuera en gateway för anslutna factory][lnk-connect-cf]</span><span class="sxs-lookup"><span data-stu-id="1a271-204">[Deploy a gateway for connected factory][lnk-connect-cf]</span></span>
* <span data-ttu-id="1a271-205">[Behörigheter för hello azureiotsuite.com plats][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="1a271-205">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>
* [<span data-ttu-id="1a271-206">Vanliga frågor och svar om ansluten fabrik</span><span class="sxs-lookup"><span data-stu-id="1a271-206">Connected factory FAQ</span></span>](iot-suite-faq-cf.md)
* <span data-ttu-id="1a271-207">[VANLIGA FRÅGOR OCH SVAR][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="1a271-207">[FAQ][lnk-faq]</span></span>


[img-oee-kpi]: ./media/iot-suite-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-suite-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-suite-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-suite-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-suite-connected-factory-customize/published.png
[img-munich]: ./media/iot-suite-connected-factory-customize/munich.png
[img-server-uris]: ./media/iot-suite-connected-factory-customize/serveruris.png
[lnk-kpi]: ./media/iot-suite-connected-factory-customize/kpidisplay.png

[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md