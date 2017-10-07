---
title: aaaData Factory funktioner och systemvariabler | Microsoft Docs
description: "Visar en lista över Azure Data Factory-funktioner och systemvariabler"
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
services: data-factory
ms.assetid: b6b3c2ae-b0e8-4e28-90d8-daf20421660d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: d2936c2821797947bb37d9775226a6c19c4b8ab9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---functions-and-system-variables"></a><span data-ttu-id="39882-103">Azure Data Factory - funktioner och systemvariabler</span><span class="sxs-lookup"><span data-stu-id="39882-103">Azure Data Factory - Functions and System Variables</span></span>
<span data-ttu-id="39882-104">Den här artikeln innehåller information om funktioner och variabler som stöds av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="39882-104">This article provides information about functions and variables supported by Azure Data Factory.</span></span>

## <a name="data-factory-system-variables"></a><span data-ttu-id="39882-105">Data Factory systemvariabler</span><span class="sxs-lookup"><span data-stu-id="39882-105">Data Factory system variables</span></span>
| <span data-ttu-id="39882-106">Variabelnamn</span><span class="sxs-lookup"><span data-stu-id="39882-106">Variable Name</span></span> | <span data-ttu-id="39882-107">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="39882-107">Description</span></span> | <span data-ttu-id="39882-108">Objektets Scope</span><span class="sxs-lookup"><span data-stu-id="39882-108">Object Scope</span></span> | <span data-ttu-id="39882-109">JSON-Scope och användningsområden</span><span class="sxs-lookup"><span data-stu-id="39882-109">JSON Scope and Use Cases</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="39882-110">WindowStart</span><span class="sxs-lookup"><span data-stu-id="39882-110">WindowStart</span></span> |<span data-ttu-id="39882-111">Början av tidsintervall för aktuell aktivitet som kör Windows</span><span class="sxs-lookup"><span data-stu-id="39882-111">Start of time interval for current activity run window</span></span> |<span data-ttu-id="39882-112">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="39882-112">activity</span></span> |<ol><li><span data-ttu-id="39882-113">Ange datafrågor val.</span><span class="sxs-lookup"><span data-stu-id="39882-113">Specify data selection queries.</span></span> <span data-ttu-id="39882-114">Se connector artiklar som refereras i hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="39882-114">See connector articles referenced in hello [Data Movement Activities](data-factory-data-movement-activities.md) article.</span></span></li> |
| <span data-ttu-id="39882-115">WindowEnd</span><span class="sxs-lookup"><span data-stu-id="39882-115">WindowEnd</span></span> |<span data-ttu-id="39882-116">Slutet av tidsintervall för aktuell aktivitet som kör Windows</span><span class="sxs-lookup"><span data-stu-id="39882-116">End of time interval for current activity run window</span></span> |<span data-ttu-id="39882-117">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="39882-117">activity</span></span> |<span data-ttu-id="39882-118">samma som WindowStart.</span><span class="sxs-lookup"><span data-stu-id="39882-118">same as WindowStart.</span></span> |
| <span data-ttu-id="39882-119">SliceStart</span><span class="sxs-lookup"><span data-stu-id="39882-119">SliceStart</span></span> |<span data-ttu-id="39882-120">Början av tidsintervall för datasektor som skapas</span><span class="sxs-lookup"><span data-stu-id="39882-120">Start of time interval for data  slice being produced</span></span> |<span data-ttu-id="39882-121">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="39882-121">activity</span></span><br/><span data-ttu-id="39882-122">DataSet</span><span class="sxs-lookup"><span data-stu-id="39882-122">dataset</span></span> |<ol><li><span data-ttu-id="39882-123">Ange dynamiska sökvägar och filnamn när du arbetar med [Azure Blob](data-factory-azure-blob-connector.md) och [filsystemet datauppsättningar](data-factory-onprem-file-system-connector.md).</span><span class="sxs-lookup"><span data-stu-id="39882-123">Specify dynamic folder paths and file names while working with [Azure Blob](data-factory-azure-blob-connector.md) and [File System datasets](data-factory-onprem-file-system-connector.md).</span></span></li><li><span data-ttu-id="39882-124">Ange indata beroenden med data factory-funktioner i aktiviteten indata samling.</span><span class="sxs-lookup"><span data-stu-id="39882-124">Specify input dependencies with data factory functions in activity inputs collection.</span></span></li></ol> |
| <span data-ttu-id="39882-125">SliceEnd</span><span class="sxs-lookup"><span data-stu-id="39882-125">SliceEnd</span></span> |<span data-ttu-id="39882-126">Slut på tidsintervall för aktuella datasektorn.</span><span class="sxs-lookup"><span data-stu-id="39882-126">End of time interval for current data slice.</span></span> |<span data-ttu-id="39882-127">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="39882-127">activity</span></span><br/><span data-ttu-id="39882-128">DataSet</span><span class="sxs-lookup"><span data-stu-id="39882-128">dataset</span></span> |<span data-ttu-id="39882-129">samma som SliceStart.</span><span class="sxs-lookup"><span data-stu-id="39882-129">same as SliceStart.</span></span> |

> [!NOTE]
> <span data-ttu-id="39882-130">För närvarande datafabriken kräver att hello schemalägga anges i hello aktivitet matchar exakt hello schema som angetts i tillgängligheten för utdatauppsättningen hello.</span><span class="sxs-lookup"><span data-stu-id="39882-130">Currently data factory requires that hello schedule specified in hello activity exactly matches hello schedule specified in availability of hello output dataset.</span></span> <span data-ttu-id="39882-131">Därför WindowStart, WindowEnd, och SliceStart och SliceEnd mappas alltid toohello samma tid period och ett enda utflöde segment.</span><span class="sxs-lookup"><span data-stu-id="39882-131">Therefore, WindowStart, WindowEnd, and SliceStart and SliceEnd always map toohello same time period and a single output slice.</span></span>
> 

### <a name="example-for-using-a-system-variable"></a><span data-ttu-id="39882-132">Exempel för att använda en systemvariabel</span><span class="sxs-lookup"><span data-stu-id="39882-132">Example for using a system variable</span></span>
<span data-ttu-id="39882-133">I följande exempel, år, månad, dag och tid för hello **SliceStart** extraheras till olika variabler som används av **folderPath** och **fileName** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="39882-133">In hello following example, year, month, day, and time of **SliceStart** are extracted into separate variables that are used by **folderPath** and **fileName** properties.</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

## <a name="data-factory-functions"></a><span data-ttu-id="39882-134">Data Factory-funktioner</span><span class="sxs-lookup"><span data-stu-id="39882-134">Data Factory functions</span></span>
<span data-ttu-id="39882-135">Du kan använda funktionerna i data factory tillsammans med systemvariabler för hello följande syften:</span><span class="sxs-lookup"><span data-stu-id="39882-135">You can use functions in data factory along with system variables for hello following purposes:</span></span>

1. <span data-ttu-id="39882-136">Anger att markeringen datafrågor (se connector artiklar som refereras av hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="39882-136">Specifying data selection queries (see connector articles referenced by hello [Data Movement Activities](data-factory-data-movement-activities.md) article.</span></span>
   
   <span data-ttu-id="39882-137">Hej syntax tooinvoke är en funktion som data factory:  **$$ <function>**  för val av datafrågor och andra egenskaper hello och datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="39882-137">hello syntax tooinvoke a data factory function is: **$$<function>** for data selection queries and other properties in hello activity and datasets.</span></span>  
2. <span data-ttu-id="39882-138">Ange indata beroenden med data factory-funktioner i aktivitetssamling indata.</span><span class="sxs-lookup"><span data-stu-id="39882-138">Specifying input dependencies with data factory functions in activity inputs collection.</span></span>
   
    <span data-ttu-id="39882-139">$$ behövs inte för att ange indata beroende uttryck.</span><span class="sxs-lookup"><span data-stu-id="39882-139">$$ is not needed for specifying input dependency expressions.</span></span>     

<span data-ttu-id="39882-140">I följande exempel, hello **sqlReaderQuery** egenskap i en JSON-fil tilldelas tooa värdet som returneras av hello `Text.Format` funktion.</span><span class="sxs-lookup"><span data-stu-id="39882-140">In hello following sample, **sqlReaderQuery** property in a JSON file is assigned tooa value returned by hello `Text.Format` function.</span></span> <span data-ttu-id="39882-141">Det här exemplet använder också en systemvariabeln med namnet **WindowStart**, som representerar hello starttiden för hello-aktivitet som kör Windows.</span><span class="sxs-lookup"><span data-stu-id="39882-141">This sample also uses a system variable named **WindowStart**, which represents hello start time of hello activity run window.</span></span>

```json
{
    "Type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
}
```

<span data-ttu-id="39882-142">Se [anpassade datum och tid formatsträngar](https://msdn.microsoft.com/library/8kb3ddd4.aspx) avsnitt som beskriver olika formateringsalternativ som du kan använda (till exempel: Visa kontra åååå).</span><span class="sxs-lookup"><span data-stu-id="39882-142">See [Custom Date and Time Format Strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx) topic that describes different formatting options you can use (for example: ay vs. yyyy).</span></span> 

### <a name="functions"></a><span data-ttu-id="39882-143">Funktioner</span><span class="sxs-lookup"><span data-stu-id="39882-143">Functions</span></span>
<span data-ttu-id="39882-144">hello följande tabeller visar alla hello funktioner i Azure Data Factory:</span><span class="sxs-lookup"><span data-stu-id="39882-144">hello following tables list all hello functions in Azure Data Factory:</span></span>

| <span data-ttu-id="39882-145">Kategori</span><span class="sxs-lookup"><span data-stu-id="39882-145">Category</span></span> | <span data-ttu-id="39882-146">Funktionen</span><span class="sxs-lookup"><span data-stu-id="39882-146">Function</span></span> | <span data-ttu-id="39882-147">Parametrar</span><span class="sxs-lookup"><span data-stu-id="39882-147">Parameters</span></span> | <span data-ttu-id="39882-148">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="39882-148">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="39882-149">Tid</span><span class="sxs-lookup"><span data-stu-id="39882-149">Time</span></span> |<span data-ttu-id="39882-150">AddHours(X,Y)</span><span class="sxs-lookup"><span data-stu-id="39882-150">AddHours(X,Y)</span></span> |<span data-ttu-id="39882-151">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-151">X: DateTime</span></span> <br/><br/><span data-ttu-id="39882-152">Y: int</span><span class="sxs-lookup"><span data-stu-id="39882-152">Y: int</span></span> |<span data-ttu-id="39882-153">Lägger till Y timmar toohello angivna tid X.</span><span class="sxs-lookup"><span data-stu-id="39882-153">Adds Y hours toohello given time X.</span></span> <br/><br/><span data-ttu-id="39882-154">Exempel:`9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="39882-154">Example: `9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM`</span></span> |
| <span data-ttu-id="39882-155">Tid</span><span class="sxs-lookup"><span data-stu-id="39882-155">Time</span></span> |<span data-ttu-id="39882-156">AddMinutes(X,Y)</span><span class="sxs-lookup"><span data-stu-id="39882-156">AddMinutes(X,Y)</span></span> |<span data-ttu-id="39882-157">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-157">X: DateTime</span></span> <br/><br/><span data-ttu-id="39882-158">Y: int</span><span class="sxs-lookup"><span data-stu-id="39882-158">Y: int</span></span> |<span data-ttu-id="39882-159">Lägger till Y minuter tooX.</span><span class="sxs-lookup"><span data-stu-id="39882-159">Adds Y minutes tooX.</span></span><br/><br/><span data-ttu-id="39882-160">Exempel:`9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM`</span><span class="sxs-lookup"><span data-stu-id="39882-160">Example: `9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM`</span></span> |
| <span data-ttu-id="39882-161">Tid</span><span class="sxs-lookup"><span data-stu-id="39882-161">Time</span></span> |<span data-ttu-id="39882-162">StartOfHour(X)</span><span class="sxs-lookup"><span data-stu-id="39882-162">StartOfHour(X)</span></span> |<span data-ttu-id="39882-163">X: Datetime</span><span class="sxs-lookup"><span data-stu-id="39882-163">X: Datetime</span></span> |<span data-ttu-id="39882-164">Hämtar hello starttid för hello timme som representeras av hello timkomponenten för X.</span><span class="sxs-lookup"><span data-stu-id="39882-164">Gets hello starting time for hello hour represented by hello hour component of X.</span></span> <br/><br/><span data-ttu-id="39882-165">Exempel:`StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="39882-165">Example: `StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM`</span></span> |
| <span data-ttu-id="39882-166">Date</span><span class="sxs-lookup"><span data-stu-id="39882-166">Date</span></span> |<span data-ttu-id="39882-167">AddDays(X,Y)</span><span class="sxs-lookup"><span data-stu-id="39882-167">AddDays(X,Y)</span></span> |<span data-ttu-id="39882-168">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-168">X: DateTime</span></span><br/><br/><span data-ttu-id="39882-169">Y: int</span><span class="sxs-lookup"><span data-stu-id="39882-169">Y: int</span></span> |<span data-ttu-id="39882-170">Lägger till Y dagar tooX.</span><span class="sxs-lookup"><span data-stu-id="39882-170">Adds Y days tooX.</span></span> <br/><br/><span data-ttu-id="39882-171">Exempel: 9/15/2013 12:00:00 PM + 2 dagar = 9/17/2013 12:00:00 PM.</span><span class="sxs-lookup"><span data-stu-id="39882-171">Example: 9/15/2013 12:00:00 PM + 2 days = 9/17/2013 12:00:00 PM.</span></span><br/><br/><span data-ttu-id="39882-172">Du kan ta bort dagar för genom att ange Y som ett negativt tal.</span><span class="sxs-lookup"><span data-stu-id="39882-172">You can subtract days too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="39882-173">Exempel: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="39882-173">Example: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`.</span></span> |
| <span data-ttu-id="39882-174">Date</span><span class="sxs-lookup"><span data-stu-id="39882-174">Date</span></span> |<span data-ttu-id="39882-175">AddMonths(X,Y)</span><span class="sxs-lookup"><span data-stu-id="39882-175">AddMonths(X,Y)</span></span> |<span data-ttu-id="39882-176">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-176">X: DateTime</span></span><br/><br/><span data-ttu-id="39882-177">Y: int</span><span class="sxs-lookup"><span data-stu-id="39882-177">Y: int</span></span> |<span data-ttu-id="39882-178">Lägger till Y månader tooX.</span><span class="sxs-lookup"><span data-stu-id="39882-178">Adds Y months tooX.</span></span><br/><br/><span data-ttu-id="39882-179">`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="39882-179">`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.</span></span><br/><br/><span data-ttu-id="39882-180">Du kan ta bort månader för genom att ange Y som ett negativt tal.</span><span class="sxs-lookup"><span data-stu-id="39882-180">You can subtract months too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="39882-181">Exempel: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="39882-181">Example: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.</span></span>|
| <span data-ttu-id="39882-182">Date</span><span class="sxs-lookup"><span data-stu-id="39882-182">Date</span></span> |<span data-ttu-id="39882-183">AddQuarters(X,Y)</span><span class="sxs-lookup"><span data-stu-id="39882-183">AddQuarters(X,Y)</span></span> |<span data-ttu-id="39882-184">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-184">X: DateTime</span></span> <br/><br/><span data-ttu-id="39882-185">Y: int</span><span class="sxs-lookup"><span data-stu-id="39882-185">Y: int</span></span> |<span data-ttu-id="39882-186">Lägger till Y * tooX för tre månader.</span><span class="sxs-lookup"><span data-stu-id="39882-186">Adds Y * 3 months tooX.</span></span><br/><br/><span data-ttu-id="39882-187">Exempel:`9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="39882-187">Example: `9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM`</span></span> |
| <span data-ttu-id="39882-188">Date</span><span class="sxs-lookup"><span data-stu-id="39882-188">Date</span></span> |<span data-ttu-id="39882-189">AddWeeks(X,Y)</span><span class="sxs-lookup"><span data-stu-id="39882-189">AddWeeks(X,Y)</span></span> |<span data-ttu-id="39882-190">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-190">X: DateTime</span></span><br/><br/><span data-ttu-id="39882-191">Y: int</span><span class="sxs-lookup"><span data-stu-id="39882-191">Y: int</span></span> |<span data-ttu-id="39882-192">Lägger till Y * tooX 7 dagar</span><span class="sxs-lookup"><span data-stu-id="39882-192">Adds Y * 7 days tooX</span></span><br/><br/><span data-ttu-id="39882-193">Exempel: 9/15/2013 12:00:00 PM + 1 vecka = 9/22/2013 12:00:00 PM</span><span class="sxs-lookup"><span data-stu-id="39882-193">Example: 9/15/2013 12:00:00 PM + 1 week = 9/22/2013 12:00:00 PM</span></span><br/><br/><span data-ttu-id="39882-194">Du kan ta bort veckor för genom att ange Y som ett negativt tal.</span><span class="sxs-lookup"><span data-stu-id="39882-194">You can subtract weeks too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="39882-195">Exempel: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="39882-195">Example: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`.</span></span> |
| <span data-ttu-id="39882-196">Date</span><span class="sxs-lookup"><span data-stu-id="39882-196">Date</span></span> |<span data-ttu-id="39882-197">AddYears(X,Y)</span><span class="sxs-lookup"><span data-stu-id="39882-197">AddYears(X,Y)</span></span> |<span data-ttu-id="39882-198">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-198">X: DateTime</span></span><br/><br/><span data-ttu-id="39882-199">Y: int</span><span class="sxs-lookup"><span data-stu-id="39882-199">Y: int</span></span> |<span data-ttu-id="39882-200">Lägger till Y år tooX.</span><span class="sxs-lookup"><span data-stu-id="39882-200">Adds Y years tooX.</span></span><br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 year = 9/15/2014 12:00:00 PM`<br/><br/><span data-ttu-id="39882-201">Du kan ta bort år för genom att ange Y som ett negativt tal.</span><span class="sxs-lookup"><span data-stu-id="39882-201">You can subtract years too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="39882-202">Exempel: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="39882-202">Example: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`.</span></span> |
| <span data-ttu-id="39882-203">Date</span><span class="sxs-lookup"><span data-stu-id="39882-203">Date</span></span> |<span data-ttu-id="39882-204">Day(X)</span><span class="sxs-lookup"><span data-stu-id="39882-204">Day(X)</span></span> |<span data-ttu-id="39882-205">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-205">X: DateTime</span></span> |<span data-ttu-id="39882-206">Hämtar hello dagkomponenten för X.</span><span class="sxs-lookup"><span data-stu-id="39882-206">Gets hello day component of X.</span></span><br/><br/><span data-ttu-id="39882-207">Exempel: `Day of 9/15/2013 12:00:00 PM is 9`.</span><span class="sxs-lookup"><span data-stu-id="39882-207">Example: `Day of 9/15/2013 12:00:00 PM is 9`.</span></span> |
| <span data-ttu-id="39882-208">Date</span><span class="sxs-lookup"><span data-stu-id="39882-208">Date</span></span> |<span data-ttu-id="39882-209">DayOfWeek(X)</span><span class="sxs-lookup"><span data-stu-id="39882-209">DayOfWeek(X)</span></span> |<span data-ttu-id="39882-210">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-210">X: DateTime</span></span> |<span data-ttu-id="39882-211">Hämtar hello dagen i veckodagskomponenten för X.</span><span class="sxs-lookup"><span data-stu-id="39882-211">Gets hello day of week component of X.</span></span><br/><br/><span data-ttu-id="39882-212">Exempel: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`.</span><span class="sxs-lookup"><span data-stu-id="39882-212">Example: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`.</span></span> |
| <span data-ttu-id="39882-213">Date</span><span class="sxs-lookup"><span data-stu-id="39882-213">Date</span></span> |<span data-ttu-id="39882-214">DayOfYear(X)</span><span class="sxs-lookup"><span data-stu-id="39882-214">DayOfYear(X)</span></span> |<span data-ttu-id="39882-215">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-215">X: DateTime</span></span> |<span data-ttu-id="39882-216">Hämtar hello dag hello år som representeras av hello årskomponenten för X.</span><span class="sxs-lookup"><span data-stu-id="39882-216">Gets hello day in hello year represented by hello year component of X.</span></span><br/><br/><span data-ttu-id="39882-217">Exempel:</span><span class="sxs-lookup"><span data-stu-id="39882-217">Examples:</span></span><br/>`12/1/2015: day 335 of 2015`<br/>`12/31/2015: day 365 of 2015`<br/>`12/31/2016: day 366 of 2016 (Leap Year)` |
| <span data-ttu-id="39882-218">Date</span><span class="sxs-lookup"><span data-stu-id="39882-218">Date</span></span> |<span data-ttu-id="39882-219">DaysInMonth(X)</span><span class="sxs-lookup"><span data-stu-id="39882-219">DaysInMonth(X)</span></span> |<span data-ttu-id="39882-220">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-220">X: DateTime</span></span> |<span data-ttu-id="39882-221">Hämtar hello dagar i hello månad som representeras av hello månadskomponenten för parametern X.</span><span class="sxs-lookup"><span data-stu-id="39882-221">Gets hello days in hello month represented by hello month component of parameter X.</span></span><br/><br/><span data-ttu-id="39882-222">Exempel: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in hello September month`.</span><span class="sxs-lookup"><span data-stu-id="39882-222">Example: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in hello September month`.</span></span> |
| <span data-ttu-id="39882-223">Date</span><span class="sxs-lookup"><span data-stu-id="39882-223">Date</span></span> |<span data-ttu-id="39882-224">EndOfDay(X)</span><span class="sxs-lookup"><span data-stu-id="39882-224">EndOfDay(X)</span></span> |<span data-ttu-id="39882-225">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-225">X: DateTime</span></span> |<span data-ttu-id="39882-226">Hämtar hello tid som representerar hello slutet av hello dag (dagkomponenten) för X.</span><span class="sxs-lookup"><span data-stu-id="39882-226">Gets hello date-time that represents hello end of hello day (day component) of X.</span></span><br/><br/><span data-ttu-id="39882-227">Exempel: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`.</span><span class="sxs-lookup"><span data-stu-id="39882-227">Example: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`.</span></span> |
| <span data-ttu-id="39882-228">Date</span><span class="sxs-lookup"><span data-stu-id="39882-228">Date</span></span> |<span data-ttu-id="39882-229">EndOfMonth(X)</span><span class="sxs-lookup"><span data-stu-id="39882-229">EndOfMonth(X)</span></span> |<span data-ttu-id="39882-230">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-230">X: DateTime</span></span> |<span data-ttu-id="39882-231">Hämtar hello månadsslut hello representeras av månadskomponenten för parametern X.</span><span class="sxs-lookup"><span data-stu-id="39882-231">Gets hello end of hello month represented by month component of parameter X.</span></span> <br/><br/><span data-ttu-id="39882-232">Exempel: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (datum som representerar hello slutet av September månad)</span><span class="sxs-lookup"><span data-stu-id="39882-232">Example: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (date time that represents hello end of September month)</span></span> |
| <span data-ttu-id="39882-233">Date</span><span class="sxs-lookup"><span data-stu-id="39882-233">Date</span></span> |<span data-ttu-id="39882-234">StartOfDay(X)</span><span class="sxs-lookup"><span data-stu-id="39882-234">StartOfDay(X)</span></span> |<span data-ttu-id="39882-235">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-235">X: DateTime</span></span> |<span data-ttu-id="39882-236">Hämtar hello början av hello dag som representeras av hello dagkomponenten för parametern X.</span><span class="sxs-lookup"><span data-stu-id="39882-236">Gets hello start of hello day represented by hello day component of parameter X.</span></span><br/><br/><span data-ttu-id="39882-237">Exempel: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`.</span><span class="sxs-lookup"><span data-stu-id="39882-237">Example: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`.</span></span> |
| <span data-ttu-id="39882-238">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="39882-238">DateTime</span></span> |<span data-ttu-id="39882-239">FROM(X)</span><span class="sxs-lookup"><span data-stu-id="39882-239">From(X)</span></span> |<span data-ttu-id="39882-240">X: sträng</span><span class="sxs-lookup"><span data-stu-id="39882-240">X: String</span></span> |<span data-ttu-id="39882-241">Parsa strängen X tooa datum och tid.</span><span class="sxs-lookup"><span data-stu-id="39882-241">Parse string X tooa date time.</span></span> |
| <span data-ttu-id="39882-242">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="39882-242">DateTime</span></span> |<span data-ttu-id="39882-243">Ticks(X)</span><span class="sxs-lookup"><span data-stu-id="39882-243">Ticks(X)</span></span> |<span data-ttu-id="39882-244">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="39882-244">X: DateTime</span></span> |<span data-ttu-id="39882-245">Hämtar hello tick-egenskapen för hello parametern X. En skalstreck är lika med 100 nanosekunder.</span><span class="sxs-lookup"><span data-stu-id="39882-245">Gets hello ticks property of hello parameter X. One tick equals 100 nanoseconds.</span></span> <span data-ttu-id="39882-246">hello värdet på egenskapen representerar hello antalet skalstreck som har förflutit sedan 12:00:00 midnatt den 1 januari 0001.</span><span class="sxs-lookup"><span data-stu-id="39882-246">hello value of this property represents hello number of ticks that have elapsed since 12:00:00 midnight, January 1, 0001.</span></span> |
| <span data-ttu-id="39882-247">Text</span><span class="sxs-lookup"><span data-stu-id="39882-247">Text</span></span> |<span data-ttu-id="39882-248">Format(X)</span><span class="sxs-lookup"><span data-stu-id="39882-248">Format(X)</span></span> |<span data-ttu-id="39882-249">X: string-variabel</span><span class="sxs-lookup"><span data-stu-id="39882-249">X: String variable</span></span> |<span data-ttu-id="39882-250">Format hello text (Använd `\\'` kombination tooescape `'` tecken).</span><span class="sxs-lookup"><span data-stu-id="39882-250">Formats hello text (use `\\'` combination tooescape `'` character).</span></span>|

> [!IMPORTANT]
> <span data-ttu-id="39882-251">När du använder en funktion i en annan funktion behöver du inte toouse  **$$**  prefix för inre hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="39882-251">When using a function within another function, you do not need toouse **$$** prefix for hello inner function.</span></span> <span data-ttu-id="39882-252">Till exempel: $$Text.Format ('PartitionKey eq \\' my_pkey_filter_value\\' och ge RowKey \\' {0: ÅÅÅÅ-MM-dd: mm: ss}\\'', Time.AddHours (SliceStart -6)).</span><span class="sxs-lookup"><span data-stu-id="39882-252">For example: $$Text.Format('PartitionKey eq \\'my_pkey_filter_value\\' and RowKey ge \\'{0: yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(SliceStart, -6)).</span></span> <span data-ttu-id="39882-253">Observera att i det här exemplet  **$$**  prefix används inte för hello **Time.AddHours** funktion.</span><span class="sxs-lookup"><span data-stu-id="39882-253">In this example, notice that **$$** prefix is not used for hello **Time.AddHours** function.</span></span> 

#### <a name="example"></a><span data-ttu-id="39882-254">Exempel</span><span class="sxs-lookup"><span data-stu-id="39882-254">Example</span></span>
<span data-ttu-id="39882-255">I hello följande parametrar för hello Hive aktivitet som exempel, ingående och utgående bestäms genom att använda hello `Text.Format` funktion och SliceStart systemvariabeln.</span><span class="sxs-lookup"><span data-stu-id="39882-255">In hello following example, input and output parameters for hello Hive activity are determined by using hello `Text.Format` function and SliceStart system variable.</span></span> 

```json  
{
    "name": "HiveActivitySamplePipeline",
        "properties": {
    "activities": [
            {
            "name": "HiveActivitySample",
            "type": "HDInsightHive",
            "inputs": [
                    {
                    "name": "HiveSampleIn"
                    }
            ],
            "outputs": [
                    {
                    "name": "HiveSampleOut"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                }
            }
            }
    ]
    }
}
```

### <a name="example-2"></a><span data-ttu-id="39882-256">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="39882-256">Example 2</span></span>

<span data-ttu-id="39882-257">I följande exempel hello, hello hello DateTime-parametern för hello lagrade Proceduraktiviteten bestäms med hjälp av Text.</span><span class="sxs-lookup"><span data-stu-id="39882-257">In hello following example, hello DateTime parameter for hello Stored Procedure Activity is determined by using hello Text.</span></span> <span data-ttu-id="39882-258">Formatera funktionen och hello SliceStart variabeln.</span><span class="sxs-lookup"><span data-stu-id="39882-258">Format function and hello SliceStart variable.</span></span> 

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
            "start": "2016-08-02T00:00:00Z",
            "end": "2016-08-02T05:00:00Z",
        "isPaused": false
    }
}
```

### <a name="example-3"></a><span data-ttu-id="39882-259">Exempel 3</span><span class="sxs-lookup"><span data-stu-id="39882-259">Example 3</span></span>
<span data-ttu-id="39882-260">tooread data från föregående dag i stället för dag som representeras av hello SliceStart, funktionen hello AddDays som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="39882-260">tooread data from previous day instead of day represented by hello SliceStart, use hello AddDays function as shown in hello following example:</span></span> 

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-01-01T08:00:00",
        "end": "2017-01-01T11:00:00",
        "description": "hive activity",
        "activities": [
            {
                "name": "SampleHiveActivity",
                "inputs": [
                    {
                        "name": "MyAzureBlobInput",
                        "startTime": "Date.AddDays(SliceStart, -1)",
                        "endTime": "Date.AddDays(SliceEnd, -1)"
                    }
                ],
                "outputs": [
                    {
                        "name": "MyAzureBlobOutput"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adftutorial\\hivequery.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Year": "$$Text.Format('{0:yyyy}',WindowsStart)",
                        "Month": "$$Text.Format('{0:MM}',WindowStart)",
                        "Day": "$$Text.Format('{0:dd}',WindowStart)"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 2,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

<span data-ttu-id="39882-261">Se [anpassade datum och tid formatsträngar](https://msdn.microsoft.com/library/8kb3ddd4.aspx) avsnitt som beskriver olika formateringsalternativ som du kan använda (till exempel: åå kontra åååå).</span><span class="sxs-lookup"><span data-stu-id="39882-261">See [Custom Date and Time Format Strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx) topic that describes different formatting options you can use (for example: yy vs. yyyy).</span></span> 

