---
title: Data Factory-funktioner och systemvariabler | Microsoft Docs
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
ms.openlocfilehash: 72a966bdc271f86b9568d3310d2e22d83b447594
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-data-factory---functions-and-system-variables"></a><span data-ttu-id="3e4f8-103">Azure Data Factory - funktioner och systemvariabler</span><span class="sxs-lookup"><span data-stu-id="3e4f8-103">Azure Data Factory - Functions and System Variables</span></span>
<span data-ttu-id="3e4f8-104">Den här artikeln innehåller information om funktioner och variabler som stöds av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-104">This article provides information about functions and variables supported by Azure Data Factory.</span></span>

## <a name="data-factory-system-variables"></a><span data-ttu-id="3e4f8-105">Data Factory systemvariabler</span><span class="sxs-lookup"><span data-stu-id="3e4f8-105">Data Factory system variables</span></span>
| <span data-ttu-id="3e4f8-106">Variabelnamn</span><span class="sxs-lookup"><span data-stu-id="3e4f8-106">Variable Name</span></span> | <span data-ttu-id="3e4f8-107">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3e4f8-107">Description</span></span> | <span data-ttu-id="3e4f8-108">Objektets Scope</span><span class="sxs-lookup"><span data-stu-id="3e4f8-108">Object Scope</span></span> | <span data-ttu-id="3e4f8-109">JSON-Scope och användningsområden</span><span class="sxs-lookup"><span data-stu-id="3e4f8-109">JSON Scope and Use Cases</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e4f8-110">WindowStart</span><span class="sxs-lookup"><span data-stu-id="3e4f8-110">WindowStart</span></span> |<span data-ttu-id="3e4f8-111">Början av tidsintervall för aktuell aktivitet som kör Windows</span><span class="sxs-lookup"><span data-stu-id="3e4f8-111">Start of time interval for current activity run window</span></span> |<span data-ttu-id="3e4f8-112">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="3e4f8-112">activity</span></span> |<ol><li><span data-ttu-id="3e4f8-113">Ange datafrågor val.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-113">Specify data selection queries.</span></span> <span data-ttu-id="3e4f8-114">Se connector artiklar som refereras i den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-114">See connector articles referenced in the [Data Movement Activities](data-factory-data-movement-activities.md) article.</span></span></li> |
| <span data-ttu-id="3e4f8-115">WindowEnd</span><span class="sxs-lookup"><span data-stu-id="3e4f8-115">WindowEnd</span></span> |<span data-ttu-id="3e4f8-116">Slutet av tidsintervall för aktuell aktivitet som kör Windows</span><span class="sxs-lookup"><span data-stu-id="3e4f8-116">End of time interval for current activity run window</span></span> |<span data-ttu-id="3e4f8-117">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="3e4f8-117">activity</span></span> |<span data-ttu-id="3e4f8-118">samma som WindowStart.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-118">same as WindowStart.</span></span> |
| <span data-ttu-id="3e4f8-119">SliceStart</span><span class="sxs-lookup"><span data-stu-id="3e4f8-119">SliceStart</span></span> |<span data-ttu-id="3e4f8-120">Början av tidsintervall för datasektor som skapas</span><span class="sxs-lookup"><span data-stu-id="3e4f8-120">Start of time interval for data  slice being produced</span></span> |<span data-ttu-id="3e4f8-121">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="3e4f8-121">activity</span></span><br/><span data-ttu-id="3e4f8-122">DataSet</span><span class="sxs-lookup"><span data-stu-id="3e4f8-122">dataset</span></span> |<ol><li><span data-ttu-id="3e4f8-123">Ange dynamiska sökvägar och filnamn när du arbetar med [Azure Blob](data-factory-azure-blob-connector.md) och [filsystemet datauppsättningar](data-factory-onprem-file-system-connector.md).</span><span class="sxs-lookup"><span data-stu-id="3e4f8-123">Specify dynamic folder paths and file names while working with [Azure Blob](data-factory-azure-blob-connector.md) and [File System datasets](data-factory-onprem-file-system-connector.md).</span></span></li><li><span data-ttu-id="3e4f8-124">Ange indata beroenden med data factory-funktioner i aktiviteten indata samling.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-124">Specify input dependencies with data factory functions in activity inputs collection.</span></span></li></ol> |
| <span data-ttu-id="3e4f8-125">SliceEnd</span><span class="sxs-lookup"><span data-stu-id="3e4f8-125">SliceEnd</span></span> |<span data-ttu-id="3e4f8-126">Slut på tidsintervall för aktuella datasektorn.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-126">End of time interval for current data slice.</span></span> |<span data-ttu-id="3e4f8-127">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="3e4f8-127">activity</span></span><br/><span data-ttu-id="3e4f8-128">DataSet</span><span class="sxs-lookup"><span data-stu-id="3e4f8-128">dataset</span></span> |<span data-ttu-id="3e4f8-129">samma som SliceStart.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-129">same as SliceStart.</span></span> |

> [!NOTE]
> <span data-ttu-id="3e4f8-130">För närvarande kräver datafabriken att schemat som anges i aktiviteten exakt matchar det schema som angetts i tillgängligheten för datamängd för utdata.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-130">Currently data factory requires that the schedule specified in the activity exactly matches the schedule specified in availability of the output dataset.</span></span> <span data-ttu-id="3e4f8-131">WindowStart, WindowEnd, och SliceStart och SliceEnd mappas därför alltid till samma tidsperiod och ett enda utflöde segment.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-131">Therefore, WindowStart, WindowEnd, and SliceStart and SliceEnd always map to the same time period and a single output slice.</span></span>
> 

### <a name="example-for-using-a-system-variable"></a><span data-ttu-id="3e4f8-132">Exempel för att använda en systemvariabel</span><span class="sxs-lookup"><span data-stu-id="3e4f8-132">Example for using a system variable</span></span>
<span data-ttu-id="3e4f8-133">I följande exempel, år, månad, dag och tid för **SliceStart** extraheras till olika variabler som används av **folderPath** och **fileName** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-133">In the following example, year, month, day, and time of **SliceStart** are extracted into separate variables that are used by **folderPath** and **fileName** properties.</span></span>

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

## <a name="data-factory-functions"></a><span data-ttu-id="3e4f8-134">Data Factory-funktioner</span><span class="sxs-lookup"><span data-stu-id="3e4f8-134">Data Factory functions</span></span>
<span data-ttu-id="3e4f8-135">Du kan använda funktionerna i data factory tillsammans med systemvariabler för följande ändamål:</span><span class="sxs-lookup"><span data-stu-id="3e4f8-135">You can use functions in data factory along with system variables for the following purposes:</span></span>

1. <span data-ttu-id="3e4f8-136">Anger att markeringen datafrågor (finns connector artiklar som refererar till den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-136">Specifying data selection queries (see connector articles referenced by the [Data Movement Activities](data-factory-data-movement-activities.md) article.</span></span>
   
   <span data-ttu-id="3e4f8-137">Syntax för att anropa en funktion som data factory är:  **$$ <function>**  för val av datafrågor och andra egenskaper i aktiviteten och datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-137">The syntax to invoke a data factory function is: **$$<function>** for data selection queries and other properties in the activity and datasets.</span></span>  
2. <span data-ttu-id="3e4f8-138">Ange indata beroenden med data factory-funktioner i aktivitetssamling indata.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-138">Specifying input dependencies with data factory functions in activity inputs collection.</span></span>
   
    <span data-ttu-id="3e4f8-139">$$ behövs inte för att ange indata beroende uttryck.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-139">$$ is not needed for specifying input dependency expressions.</span></span>     

<span data-ttu-id="3e4f8-140">I följande exempel **sqlReaderQuery** egenskap i en JSON-fil som har tilldelats ett värde som returneras av den `Text.Format` funktion.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-140">In the following sample, **sqlReaderQuery** property in a JSON file is assigned to a value returned by the `Text.Format` function.</span></span> <span data-ttu-id="3e4f8-141">Det här exemplet använder också en systemvariabeln med namnet **WindowStart**, som representerar starttiden för kör-Aktivitetsfönstret.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-141">This sample also uses a system variable named **WindowStart**, which represents the start time of the activity run window.</span></span>

```json
{
    "Type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
}
```

<span data-ttu-id="3e4f8-142">Se [anpassade datum och tid formatsträngar](https://msdn.microsoft.com/library/8kb3ddd4.aspx) avsnitt som beskriver olika formateringsalternativ som du kan använda (till exempel: Visa kontra åååå).</span><span class="sxs-lookup"><span data-stu-id="3e4f8-142">See [Custom Date and Time Format Strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx) topic that describes different formatting options you can use (for example: ay vs. yyyy).</span></span> 

### <a name="functions"></a><span data-ttu-id="3e4f8-143">Funktioner</span><span class="sxs-lookup"><span data-stu-id="3e4f8-143">Functions</span></span>
<span data-ttu-id="3e4f8-144">I tabellerna nedan listas funktionerna i Azure Data Factory:</span><span class="sxs-lookup"><span data-stu-id="3e4f8-144">The following tables list all the functions in Azure Data Factory:</span></span>

| <span data-ttu-id="3e4f8-145">Kategori</span><span class="sxs-lookup"><span data-stu-id="3e4f8-145">Category</span></span> | <span data-ttu-id="3e4f8-146">Funktionen</span><span class="sxs-lookup"><span data-stu-id="3e4f8-146">Function</span></span> | <span data-ttu-id="3e4f8-147">Parametrar</span><span class="sxs-lookup"><span data-stu-id="3e4f8-147">Parameters</span></span> | <span data-ttu-id="3e4f8-148">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3e4f8-148">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e4f8-149">Tid</span><span class="sxs-lookup"><span data-stu-id="3e4f8-149">Time</span></span> |<span data-ttu-id="3e4f8-150">AddHours(X,Y)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-150">AddHours(X,Y)</span></span> |<span data-ttu-id="3e4f8-151">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-151">X: DateTime</span></span> <br/><br/><span data-ttu-id="3e4f8-152">Y: int</span><span class="sxs-lookup"><span data-stu-id="3e4f8-152">Y: int</span></span> |<span data-ttu-id="3e4f8-153">Lägger till Y timmar angiven tid X.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-153">Adds Y hours to the given time X.</span></span> <br/><br/><span data-ttu-id="3e4f8-154">Exempel:`9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="3e4f8-154">Example: `9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM`</span></span> |
| <span data-ttu-id="3e4f8-155">Tid</span><span class="sxs-lookup"><span data-stu-id="3e4f8-155">Time</span></span> |<span data-ttu-id="3e4f8-156">AddMinutes(X,Y)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-156">AddMinutes(X,Y)</span></span> |<span data-ttu-id="3e4f8-157">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-157">X: DateTime</span></span> <br/><br/><span data-ttu-id="3e4f8-158">Y: int</span><span class="sxs-lookup"><span data-stu-id="3e4f8-158">Y: int</span></span> |<span data-ttu-id="3e4f8-159">Lägger till Y minuter X.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-159">Adds Y minutes to X.</span></span><br/><br/><span data-ttu-id="3e4f8-160">Exempel:`9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM`</span><span class="sxs-lookup"><span data-stu-id="3e4f8-160">Example: `9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM`</span></span> |
| <span data-ttu-id="3e4f8-161">Tid</span><span class="sxs-lookup"><span data-stu-id="3e4f8-161">Time</span></span> |<span data-ttu-id="3e4f8-162">StartOfHour(X)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-162">StartOfHour(X)</span></span> |<span data-ttu-id="3e4f8-163">X: Datetime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-163">X: Datetime</span></span> |<span data-ttu-id="3e4f8-164">Hämtar starttiden för timme som representeras av timkomponenten för X.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-164">Gets the starting time for the hour represented by the hour component of X.</span></span> <br/><br/><span data-ttu-id="3e4f8-165">Exempel:`StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="3e4f8-165">Example: `StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM`</span></span> |
| <span data-ttu-id="3e4f8-166">Date</span><span class="sxs-lookup"><span data-stu-id="3e4f8-166">Date</span></span> |<span data-ttu-id="3e4f8-167">AddDays(X,Y)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-167">AddDays(X,Y)</span></span> |<span data-ttu-id="3e4f8-168">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-168">X: DateTime</span></span><br/><br/><span data-ttu-id="3e4f8-169">Y: int</span><span class="sxs-lookup"><span data-stu-id="3e4f8-169">Y: int</span></span> |<span data-ttu-id="3e4f8-170">Lägger till Y dagar X.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-170">Adds Y days to X.</span></span> <br/><br/><span data-ttu-id="3e4f8-171">Exempel: 9/15/2013 12:00:00 PM + 2 dagar = 9/17/2013 12:00:00 PM.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-171">Example: 9/15/2013 12:00:00 PM + 2 days = 9/17/2013 12:00:00 PM.</span></span><br/><br/><span data-ttu-id="3e4f8-172">Du kan ta bort dagar för genom att ange Y som ett negativt tal.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-172">You can subtract days too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="3e4f8-173">Exempel: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-173">Example: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`.</span></span> |
| <span data-ttu-id="3e4f8-174">Date</span><span class="sxs-lookup"><span data-stu-id="3e4f8-174">Date</span></span> |<span data-ttu-id="3e4f8-175">AddMonths(X,Y)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-175">AddMonths(X,Y)</span></span> |<span data-ttu-id="3e4f8-176">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-176">X: DateTime</span></span><br/><br/><span data-ttu-id="3e4f8-177">Y: int</span><span class="sxs-lookup"><span data-stu-id="3e4f8-177">Y: int</span></span> |<span data-ttu-id="3e4f8-178">Lägger till Y månader X.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-178">Adds Y months to X.</span></span><br/><br/><span data-ttu-id="3e4f8-179">`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-179">`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.</span></span><br/><br/><span data-ttu-id="3e4f8-180">Du kan ta bort månader för genom att ange Y som ett negativt tal.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-180">You can subtract months too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="3e4f8-181">Exempel: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-181">Example: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.</span></span>|
| <span data-ttu-id="3e4f8-182">Date</span><span class="sxs-lookup"><span data-stu-id="3e4f8-182">Date</span></span> |<span data-ttu-id="3e4f8-183">AddQuarters(X,Y)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-183">AddQuarters(X,Y)</span></span> |<span data-ttu-id="3e4f8-184">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-184">X: DateTime</span></span> <br/><br/><span data-ttu-id="3e4f8-185">Y: int</span><span class="sxs-lookup"><span data-stu-id="3e4f8-185">Y: int</span></span> |<span data-ttu-id="3e4f8-186">Lägger till Y * tre månader x.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-186">Adds Y * 3 months to X.</span></span><br/><br/><span data-ttu-id="3e4f8-187">Exempel:`9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM`</span><span class="sxs-lookup"><span data-stu-id="3e4f8-187">Example: `9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM`</span></span> |
| <span data-ttu-id="3e4f8-188">Date</span><span class="sxs-lookup"><span data-stu-id="3e4f8-188">Date</span></span> |<span data-ttu-id="3e4f8-189">AddWeeks(X,Y)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-189">AddWeeks(X,Y)</span></span> |<span data-ttu-id="3e4f8-190">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-190">X: DateTime</span></span><br/><br/><span data-ttu-id="3e4f8-191">Y: int</span><span class="sxs-lookup"><span data-stu-id="3e4f8-191">Y: int</span></span> |<span data-ttu-id="3e4f8-192">Lägger till Y * 7 dagar x</span><span class="sxs-lookup"><span data-stu-id="3e4f8-192">Adds Y * 7 days to X</span></span><br/><br/><span data-ttu-id="3e4f8-193">Exempel: 9/15/2013 12:00:00 PM + 1 vecka = 9/22/2013 12:00:00 PM</span><span class="sxs-lookup"><span data-stu-id="3e4f8-193">Example: 9/15/2013 12:00:00 PM + 1 week = 9/22/2013 12:00:00 PM</span></span><br/><br/><span data-ttu-id="3e4f8-194">Du kan ta bort veckor för genom att ange Y som ett negativt tal.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-194">You can subtract weeks too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="3e4f8-195">Exempel: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-195">Example: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`.</span></span> |
| <span data-ttu-id="3e4f8-196">Date</span><span class="sxs-lookup"><span data-stu-id="3e4f8-196">Date</span></span> |<span data-ttu-id="3e4f8-197">AddYears(X,Y)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-197">AddYears(X,Y)</span></span> |<span data-ttu-id="3e4f8-198">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-198">X: DateTime</span></span><br/><br/><span data-ttu-id="3e4f8-199">Y: int</span><span class="sxs-lookup"><span data-stu-id="3e4f8-199">Y: int</span></span> |<span data-ttu-id="3e4f8-200">Lägger till Y år X.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-200">Adds Y years to X.</span></span><br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 year = 9/15/2014 12:00:00 PM`<br/><br/><span data-ttu-id="3e4f8-201">Du kan ta bort år för genom att ange Y som ett negativt tal.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-201">You can subtract years too by specifying Y as a negative number.</span></span><br/><br/><span data-ttu-id="3e4f8-202">Exempel: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-202">Example: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`.</span></span> |
| <span data-ttu-id="3e4f8-203">Date</span><span class="sxs-lookup"><span data-stu-id="3e4f8-203">Date</span></span> |<span data-ttu-id="3e4f8-204">Day(X)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-204">Day(X)</span></span> |<span data-ttu-id="3e4f8-205">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-205">X: DateTime</span></span> |<span data-ttu-id="3e4f8-206">Hämtar dagkomponenten för X.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-206">Gets the day component of X.</span></span><br/><br/><span data-ttu-id="3e4f8-207">Exempel: `Day of 9/15/2013 12:00:00 PM is 9`.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-207">Example: `Day of 9/15/2013 12:00:00 PM is 9`.</span></span> |
| <span data-ttu-id="3e4f8-208">Date</span><span class="sxs-lookup"><span data-stu-id="3e4f8-208">Date</span></span> |<span data-ttu-id="3e4f8-209">DayOfWeek(X)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-209">DayOfWeek(X)</span></span> |<span data-ttu-id="3e4f8-210">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-210">X: DateTime</span></span> |<span data-ttu-id="3e4f8-211">Hämtar dagen i veckodagskomponenten för X.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-211">Gets the day of week component of X.</span></span><br/><br/><span data-ttu-id="3e4f8-212">Exempel: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-212">Example: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`.</span></span> |
| <span data-ttu-id="3e4f8-213">Date</span><span class="sxs-lookup"><span data-stu-id="3e4f8-213">Date</span></span> |<span data-ttu-id="3e4f8-214">DayOfYear(X)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-214">DayOfYear(X)</span></span> |<span data-ttu-id="3e4f8-215">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-215">X: DateTime</span></span> |<span data-ttu-id="3e4f8-216">Hämtar dagen i representeras av årskomponenten för X.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-216">Gets the day in the year represented by the year component of X.</span></span><br/><br/><span data-ttu-id="3e4f8-217">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3e4f8-217">Examples:</span></span><br/>`12/1/2015: day 335 of 2015`<br/>`12/31/2015: day 365 of 2015`<br/>`12/31/2016: day 366 of 2016 (Leap Year)` |
| <span data-ttu-id="3e4f8-218">Date</span><span class="sxs-lookup"><span data-stu-id="3e4f8-218">Date</span></span> |<span data-ttu-id="3e4f8-219">DaysInMonth(X)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-219">DaysInMonth(X)</span></span> |<span data-ttu-id="3e4f8-220">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-220">X: DateTime</span></span> |<span data-ttu-id="3e4f8-221">Hämtar dagar i månaden som månadskomponenten för parametern X.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-221">Gets the days in the month represented by the month component of parameter X.</span></span><br/><br/><span data-ttu-id="3e4f8-222">Exempel: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in the September month`.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-222">Example: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in the September month`.</span></span> |
| <span data-ttu-id="3e4f8-223">Date</span><span class="sxs-lookup"><span data-stu-id="3e4f8-223">Date</span></span> |<span data-ttu-id="3e4f8-224">EndOfDay(X)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-224">EndOfDay(X)</span></span> |<span data-ttu-id="3e4f8-225">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-225">X: DateTime</span></span> |<span data-ttu-id="3e4f8-226">Hämtar den tid som representerar dagen (dagkomponenten) för X.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-226">Gets the date-time that represents the end of the day (day component) of X.</span></span><br/><br/><span data-ttu-id="3e4f8-227">Exempel: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-227">Example: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`.</span></span> |
| <span data-ttu-id="3e4f8-228">Date</span><span class="sxs-lookup"><span data-stu-id="3e4f8-228">Date</span></span> |<span data-ttu-id="3e4f8-229">EndOfMonth(X)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-229">EndOfMonth(X)</span></span> |<span data-ttu-id="3e4f8-230">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-230">X: DateTime</span></span> |<span data-ttu-id="3e4f8-231">Hämtar slutet på månaden som representeras av månadskomponenten för parametern X.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-231">Gets the end of the month represented by month component of parameter X.</span></span> <br/><br/><span data-ttu-id="3e4f8-232">Exempel: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (datum tid som motsvarar slutet av September månad)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-232">Example: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (date time that represents the end of September month)</span></span> |
| <span data-ttu-id="3e4f8-233">Date</span><span class="sxs-lookup"><span data-stu-id="3e4f8-233">Date</span></span> |<span data-ttu-id="3e4f8-234">StartOfDay(X)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-234">StartOfDay(X)</span></span> |<span data-ttu-id="3e4f8-235">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-235">X: DateTime</span></span> |<span data-ttu-id="3e4f8-236">Hämtar början på dagen som representeras av dagkomponenten i parametern X.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-236">Gets the start of the day represented by the day component of parameter X.</span></span><br/><br/><span data-ttu-id="3e4f8-237">Exempel: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-237">Example: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`.</span></span> |
| <span data-ttu-id="3e4f8-238">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="3e4f8-238">DateTime</span></span> |<span data-ttu-id="3e4f8-239">FROM(X)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-239">From(X)</span></span> |<span data-ttu-id="3e4f8-240">X: sträng</span><span class="sxs-lookup"><span data-stu-id="3e4f8-240">X: String</span></span> |<span data-ttu-id="3e4f8-241">Parsa strängen X till en datum-tid.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-241">Parse string X to a date time.</span></span> |
| <span data-ttu-id="3e4f8-242">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="3e4f8-242">DateTime</span></span> |<span data-ttu-id="3e4f8-243">Ticks(X)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-243">Ticks(X)</span></span> |<span data-ttu-id="3e4f8-244">X: DateTime</span><span class="sxs-lookup"><span data-stu-id="3e4f8-244">X: DateTime</span></span> |<span data-ttu-id="3e4f8-245">Hämtar skalstrecken-egenskapen för parametern X. En skalstreck är lika med 100 nanosekunder.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-245">Gets the ticks property of the parameter X. One tick equals 100 nanoseconds.</span></span> <span data-ttu-id="3e4f8-246">Värdet för den här egenskapen representerar antalet intervall som har förflutit sedan 12:00:00 midnatt den 1 januari 0001.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-246">The value of this property represents the number of ticks that have elapsed since 12:00:00 midnight, January 1, 0001.</span></span> |
| <span data-ttu-id="3e4f8-247">Text</span><span class="sxs-lookup"><span data-stu-id="3e4f8-247">Text</span></span> |<span data-ttu-id="3e4f8-248">Format(X)</span><span class="sxs-lookup"><span data-stu-id="3e4f8-248">Format(X)</span></span> |<span data-ttu-id="3e4f8-249">X: string-variabel</span><span class="sxs-lookup"><span data-stu-id="3e4f8-249">X: String variable</span></span> |<span data-ttu-id="3e4f8-250">Formaterar texten (Använd `\\'` kombination för att undvika `'` tecken).</span><span class="sxs-lookup"><span data-stu-id="3e4f8-250">Formats the text (use `\\'` combination to escape `'` character).</span></span>|

> [!IMPORTANT]
> <span data-ttu-id="3e4f8-251">När du använder en funktion i en annan funktion behöver du inte använda  **$$**  prefix för inre funktionen.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-251">When using a function within another function, you do not need to use **$$** prefix for the inner function.</span></span> <span data-ttu-id="3e4f8-252">Till exempel: $$Text.Format ('PartitionKey eq \\' my_pkey_filter_value\\' och ge RowKey \\' {0: ÅÅÅÅ-MM-dd: mm: ss}\\'', Time.AddHours (SliceStart -6)).</span><span class="sxs-lookup"><span data-stu-id="3e4f8-252">For example: $$Text.Format('PartitionKey eq \\'my_pkey_filter_value\\' and RowKey ge \\'{0: yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(SliceStart, -6)).</span></span> <span data-ttu-id="3e4f8-253">Observera att i det här exemplet  **$$**  prefix används inte för den **Time.AddHours** funktion.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-253">In this example, notice that **$$** prefix is not used for the **Time.AddHours** function.</span></span> 

#### <a name="example"></a><span data-ttu-id="3e4f8-254">Exempel</span><span class="sxs-lookup"><span data-stu-id="3e4f8-254">Example</span></span>
<span data-ttu-id="3e4f8-255">I följande exempel inkommande och utgående parametrar för aktiviteten Hive bestäms med hjälp av den `Text.Format` funktionen och SliceStart systemvariabeln.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-255">In the following example, input and output parameters for the Hive activity are determined by using the `Text.Format` function and SliceStart system variable.</span></span> 

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

### <a name="example-2"></a><span data-ttu-id="3e4f8-256">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="3e4f8-256">Example 2</span></span>

<span data-ttu-id="3e4f8-257">I följande exempel bestäms DateTime-parametern för aktiviteten lagrad procedur med hjälp av texten.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-257">In the following example, the DateTime parameter for the Stored Procedure Activity is determined by using the Text.</span></span> <span data-ttu-id="3e4f8-258">Format-funktionen och SliceStart-variabeln.</span><span class="sxs-lookup"><span data-stu-id="3e4f8-258">Format function and the SliceStart variable.</span></span> 

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

### <a name="example-3"></a><span data-ttu-id="3e4f8-259">Exempel 3</span><span class="sxs-lookup"><span data-stu-id="3e4f8-259">Example 3</span></span>
<span data-ttu-id="3e4f8-260">Om du vill läsa data från föregående dag i stället för dag som representeras av SliceStart funktionen AddDays som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="3e4f8-260">To read data from previous day instead of day represented by the SliceStart, use the AddDays function as shown in the following example:</span></span> 

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

<span data-ttu-id="3e4f8-261">Se [anpassade datum och tid formatsträngar](https://msdn.microsoft.com/library/8kb3ddd4.aspx) avsnitt som beskriver olika formateringsalternativ som du kan använda (till exempel: åå kontra åååå).</span><span class="sxs-lookup"><span data-stu-id="3e4f8-261">See [Custom Date and Time Format Strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx) topic that describes different formatting options you can use (for example: yy vs. yyyy).</span></span> 

