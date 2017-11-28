---
title: Migrera dina data till SQL Data Warehouse | Microsoft Docs
description: "Tips för att migrera dina data till Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: d78f954a-f54c-4aa4-9040-919bc6414887
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/29/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: dbdf1696cd169aa7e5e23f116027a1170347f4ea
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="migrate-your-data"></a><span data-ttu-id="88489-103">Migrera dina Data</span><span class="sxs-lookup"><span data-stu-id="88489-103">Migrate Your Data</span></span>
<span data-ttu-id="88489-104">Data kan flyttas från olika källor till din SQL Data Warehouse med en olika verktyg.</span><span class="sxs-lookup"><span data-stu-id="88489-104">Data can be moved from different sources into your SQL Data Warehouse with a variety tools.</span></span>  <span data-ttu-id="88489-105">ADF kopiera, SSIS och bcp kan alla användas för att uppnå det här målet.</span><span class="sxs-lookup"><span data-stu-id="88489-105">ADF Copy, SSIS, and bcp can all be used to achieve this goal.</span></span> <span data-ttu-id="88489-106">Som mängden data ökar bör du dock tänka bryta ned migrering av data i steg.</span><span class="sxs-lookup"><span data-stu-id="88489-106">However, as the amount of data increases you should think about breaking down the data migration process into steps.</span></span> <span data-ttu-id="88489-107">Detta ger möjlighet att optimera varje steg både för prestanda och återhämtning att säkerställa en smidig datamigrering.</span><span class="sxs-lookup"><span data-stu-id="88489-107">This affords you the opportunity to optimize each step both for performance and for resilience to ensure a smooth data migration.</span></span>

<span data-ttu-id="88489-108">Den här artikeln beskrivs först enkla Migreringsscenarier ADF kopia, SSIS och bcp.</span><span class="sxs-lookup"><span data-stu-id="88489-108">This article first discusses the simple migration scenarios of ADF Copy, SSIS, and bcp.</span></span> <span data-ttu-id="88489-109">Den sedan studera lite djupare hur migreringen kan optimeras.</span><span class="sxs-lookup"><span data-stu-id="88489-109">It then look a little deeper into how the migration can be optimized.</span></span>

## <a name="azure-data-factory-adf-copy"></a><span data-ttu-id="88489-110">Azure Data Factory (ADM) kopiera</span><span class="sxs-lookup"><span data-stu-id="88489-110">Azure Data Factory (ADF) copy</span></span>
<span data-ttu-id="88489-111">[Kopiera ADF] [ ADF Copy] är en del av [Azure Data Factory][Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="88489-111">[ADF Copy][ADF Copy] is part of [Azure Data Factory][Azure Data Factory].</span></span> <span data-ttu-id="88489-112">Du kan använda ADF kopiera för att exportera data till flat-filer som finns på lokal lagring till fjärranslutna flat-filer som lagras i Azure blob storage eller direkt till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="88489-112">You can use ADF Copy to export your data to flat files residing on local storage, to remote flat files held in Azure blob storage or directly into SQL Data Warehouse.</span></span>

<span data-ttu-id="88489-113">Om dina data startar i flat-filer, så behöver du först överföra den till Azure storage blob innan du påbörjar en belastning till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="88489-113">If your data starts in flat files, then you will first need to transfer it to Azure storage blob before initiating a load it into SQL Data Warehouse.</span></span> <span data-ttu-id="88489-114">När data överförs till Azure blob storage kan du använda [ADF kopiera] [ ADF Copy] igen för att skicka data till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="88489-114">Once the data is transferred into Azure blob storage you can choose to use [ADF Copy][ADF Copy] again to push the data into SQL Data Warehouse.</span></span>

<span data-ttu-id="88489-115">PolyBase ger också ett alternativ för högpresterande för inläsning av data.</span><span class="sxs-lookup"><span data-stu-id="88489-115">PolyBase also provides a high-performance option for loading the data.</span></span> <span data-ttu-id="88489-116">Dock innebär som använder två verktyg i stället för en.</span><span class="sxs-lookup"><span data-stu-id="88489-116">However, that does mean using two tools instead of one.</span></span> <span data-ttu-id="88489-117">Om du behöver för att uppnå bästa prestanda och Använd sedan PolyBase.</span><span class="sxs-lookup"><span data-stu-id="88489-117">If you need the best performance then use PolyBase.</span></span> <span data-ttu-id="88489-118">Om du vill använda en enda verktyg upplevelse (och data är inte massiv) är ADF svaret.</span><span class="sxs-lookup"><span data-stu-id="88489-118">If you want a single tool experience (and the data is not massive) then ADF is your answer.</span></span>


> 
> 

<span data-ttu-id="88489-119">HEAD via följande artikel för vissa bra [ADF prover][ADF samples].</span><span class="sxs-lookup"><span data-stu-id="88489-119">Head over to the following article for some great [ADF samples][ADF samples].</span></span>

## <a name="integration-services"></a><span data-ttu-id="88489-120">Integrationstjänster</span><span class="sxs-lookup"><span data-stu-id="88489-120">Integration Services</span></span>
<span data-ttu-id="88489-121">Integration Services (SSIS) är ett kraftfullt och flexibelt extrahera Transform och Load (ETL) som stöder komplexa arbetsflöden, DTS och flera alternativ för inläsning av data.</span><span class="sxs-lookup"><span data-stu-id="88489-121">Integration Services (SSIS) is a powerful and flexible Extract Transform and Load (ETL) tool that supports complex workflows, data transformation, and several data loading options.</span></span> <span data-ttu-id="88489-122">Använda SSIS att helt enkelt överföra data till Azure eller som en del av en bredare migrering.</span><span class="sxs-lookup"><span data-stu-id="88489-122">Use SSIS to simply transfer data to Azure or as part of a broader migration.</span></span>

> [!NOTE]
> <span data-ttu-id="88489-123">SSIS exportera till UTF-8 utan byte-ordningsmarkering i filen.</span><span class="sxs-lookup"><span data-stu-id="88489-123">SSIS can export to UTF-8 without the byte order mark in the file.</span></span> <span data-ttu-id="88489-124">Om du vill konfigurera detta måste du först använda komponenten härledd kolumn för att konvertera teckendata i dataflöde för att använda 65001 UTF-8-teckentabellen.</span><span class="sxs-lookup"><span data-stu-id="88489-124">To configure this you must first use the derived column component to convert the character data in the data flow to use the 65001 UTF-8 code page.</span></span> <span data-ttu-id="88489-125">När kolumnerna som har konverterats skriva data till flat fil-målnätverkskort säkerställer att 65001 också har markerats som en teckentabell för filen.</span><span class="sxs-lookup"><span data-stu-id="88489-125">Once the columns have been converted, write the data to the flat file destination adapter ensuring that 65001 has also been selected as the code page for the file.</span></span>
> 
> 

<span data-ttu-id="88489-126">SSIS ansluter till SQL Data Warehouse precis som den ska ansluta till en SQL Server-distributionen.</span><span class="sxs-lookup"><span data-stu-id="88489-126">SSIS connects to SQL Data Warehouse just as it would connect to a SQL Server deployment.</span></span> <span data-ttu-id="88489-127">Dock behöver dina anslutningar använder en ADO.NET Anslutningshanteraren.</span><span class="sxs-lookup"><span data-stu-id="88489-127">However, your connections will need to be using an ADO.NET connection manager.</span></span> <span data-ttu-id="88489-128">Du bör också var noga med för att konfigurera den ”Använd massinfogning när det är tillgängligt” inställningen för att maximera genomströmningen.</span><span class="sxs-lookup"><span data-stu-id="88489-128">You should also take care to configure the "Use bulk insert when available" setting to maximize throughput.</span></span> <span data-ttu-id="88489-129">Mer information finns i [ADO.NET-målnätverkskort] [ ADO.NET destination adapter] artikeln om du vill veta mer om den här egenskapen</span><span class="sxs-lookup"><span data-stu-id="88489-129">Please refer to the [ADO.NET destination adapter][ADO.NET destination adapter] article to learn more about this property</span></span>

> [!NOTE]
> <span data-ttu-id="88489-130">Ansluter till Azure SQL Data Warehouse med hjälp av OLEDB stöds inte.</span><span class="sxs-lookup"><span data-stu-id="88489-130">Connecting to Azure SQL Data Warehouse by using OLEDB is not supported.</span></span>
> 
> 

<span data-ttu-id="88489-131">Det finns dessutom alltid möjligheten att ett paket kan misslyckas på grund av att problem med begränsning eller nätverk.</span><span class="sxs-lookup"><span data-stu-id="88489-131">In addition, there is always the possibility that a package might fail due to throttling or network issues.</span></span> <span data-ttu-id="88489-132">Design paket så att de kan återupptas vid fel, utan göra om fungerar som slutförda före felet.</span><span class="sxs-lookup"><span data-stu-id="88489-132">Design packages so they can be resumed at the point of failure, without redoing work that completed before the failure.</span></span>

<span data-ttu-id="88489-133">Mer information finns i [SSIS-dokumentationen][SSIS documentation].</span><span class="sxs-lookup"><span data-stu-id="88489-133">For more information consult the [SSIS documentation][SSIS documentation].</span></span>

## <a name="bcp"></a><span data-ttu-id="88489-134">bcp</span><span class="sxs-lookup"><span data-stu-id="88489-134">bcp</span></span>
<span data-ttu-id="88489-135">BCP är ett kommandoradsverktyg som har utformats för flat fil dataimport och export.</span><span class="sxs-lookup"><span data-stu-id="88489-135">bcp is a command-line utility that is designed for flat file data import and export.</span></span> <span data-ttu-id="88489-136">Vissa omvandling kan ske under export av data.</span><span class="sxs-lookup"><span data-stu-id="88489-136">Some transformation can take place during data export.</span></span> <span data-ttu-id="88489-137">Om du vill utföra Använd enkla transformationer en fråga för att välja och transformera data.</span><span class="sxs-lookup"><span data-stu-id="88489-137">To perform simple transformations use a query to select and transform the data.</span></span> <span data-ttu-id="88489-138">När exporterat, kan flat-filer sedan laddas direkt i målet SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="88489-138">Once exported, the flat files can then be loaded directly into the target the SQL Data Warehouse database.</span></span>

> [!NOTE]
> <span data-ttu-id="88489-139">Det är ofta en bra idé att kapsla in omvandlingarna används under export av data i en vy i källsystemet.</span><span class="sxs-lookup"><span data-stu-id="88489-139">It is often a good idea to encapsulate the transformations used during data export in a view on the source system.</span></span> <span data-ttu-id="88489-140">Detta säkerställer att logiken sparas och processen repeterbara.</span><span class="sxs-lookup"><span data-stu-id="88489-140">This ensures that the logic is retained and the process is repeatable.</span></span>
> 
> 

<span data-ttu-id="88489-141">Fördelarna med bcp är:</span><span class="sxs-lookup"><span data-stu-id="88489-141">Advantages of bcp are:</span></span>

* <span data-ttu-id="88489-142">Enkelhet.</span><span class="sxs-lookup"><span data-stu-id="88489-142">Simplicity.</span></span> <span data-ttu-id="88489-143">BCP kommandon är enkla att skapa och köra</span><span class="sxs-lookup"><span data-stu-id="88489-143">bcp commands are simple to build and execute</span></span>
* <span data-ttu-id="88489-144">Nytt starta inläsningen.</span><span class="sxs-lookup"><span data-stu-id="88489-144">Re-startable load process.</span></span> <span data-ttu-id="88489-145">En gång exporterade belastningen kan vara körs många gånger</span><span class="sxs-lookup"><span data-stu-id="88489-145">Once exported the load can be executed any number of times</span></span>

<span data-ttu-id="88489-146">Begränsningar av bcp är:</span><span class="sxs-lookup"><span data-stu-id="88489-146">Limitations of bcp are:</span></span>

* <span data-ttu-id="88489-147">BCP fungerar med endast tabellform flata filer.</span><span class="sxs-lookup"><span data-stu-id="88489-147">bcp works with tabulated flat files only.</span></span> <span data-ttu-id="88489-148">Det fungerar inte med filer som XML- eller JSON</span><span class="sxs-lookup"><span data-stu-id="88489-148">It does not work with files such as xml or JSON</span></span>
* <span data-ttu-id="88489-149">Data transformation funktioner är begränsade till fasen export och är enkla till sin natur</span><span class="sxs-lookup"><span data-stu-id="88489-149">Data transformation capabilities are limited to the export stage only and are simple in nature</span></span>
* <span data-ttu-id="88489-150">BCP har anpassats till att vara robust vid inläsning av data via internet.</span><span class="sxs-lookup"><span data-stu-id="88489-150">bcp has not been adapted to be robust when loading data over the internet.</span></span> <span data-ttu-id="88489-151">Instabilitet nätverket kan orsaka en Inläsningsfel.</span><span class="sxs-lookup"><span data-stu-id="88489-151">Any network instability may cause a load error.</span></span>
* <span data-ttu-id="88489-152">BCP är beroende av schemat finns i måldatabasen innan belastningen</span><span class="sxs-lookup"><span data-stu-id="88489-152">bcp relies on the schema being present in the target database prior to the load</span></span>

<span data-ttu-id="88489-153">Mer information finns i [Använd bcp för att läsa in data till SQL Data Warehouse][Use bcp to load data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="88489-153">For more information, see [Use bcp to load data into SQL Data Warehouse][Use bcp to load data into SQL Data Warehouse].</span></span>

## <a name="optimizing-data-migration"></a><span data-ttu-id="88489-154">Optimera datamigrering</span><span class="sxs-lookup"><span data-stu-id="88489-154">Optimizing data migration</span></span>
<span data-ttu-id="88489-155">Migrerar en SQLDW data kan delas effektivt upp till tre separata steg:</span><span class="sxs-lookup"><span data-stu-id="88489-155">A SQLDW data migration process can be effectively broken down into three discrete steps:</span></span>

1. <span data-ttu-id="88489-156">Export av källdata</span><span class="sxs-lookup"><span data-stu-id="88489-156">Export of source data</span></span>
2. <span data-ttu-id="88489-157">Överföring av data till Azure</span><span class="sxs-lookup"><span data-stu-id="88489-157">Transfer of data to Azure</span></span>
3. <span data-ttu-id="88489-158">Läs in till måldatabasen SQLDW</span><span class="sxs-lookup"><span data-stu-id="88489-158">Load into the target SQLDW database</span></span>

<span data-ttu-id="88489-159">Varje steg kan optimeras individuellt för att skapa en robust, starta om och flexibel migreringsprocessen som maximerar prestandan i varje steg.</span><span class="sxs-lookup"><span data-stu-id="88489-159">Each step can be individually optimized to create a robust, re-startable and resilient migration process that maximizes performance at each step.</span></span>

## <a name="optimizing-data-load"></a><span data-ttu-id="88489-160">Optimera datainläsning</span><span class="sxs-lookup"><span data-stu-id="88489-160">Optimizing data load</span></span>
<span data-ttu-id="88489-161">Titta på dessa i omvänd ordning för ett ögonblick; Det är det snabbaste sättet att läsa in data via PolyBase.</span><span class="sxs-lookup"><span data-stu-id="88489-161">Looking at these in reverse order for a moment; the fastest way to load data is via PolyBase.</span></span> <span data-ttu-id="88489-162">Optimera för en PolyBase-inläsningen placeras krav på ovanstående steg så att det är bäst att förstå detta summa.</span><span class="sxs-lookup"><span data-stu-id="88489-162">Optimizing for a PolyBase load process places prerequisites on the preceding steps so it's best to understand this upfront.</span></span> <span data-ttu-id="88489-163">De är:</span><span class="sxs-lookup"><span data-stu-id="88489-163">They are:</span></span>

1. <span data-ttu-id="88489-164">Kodning av datafiler</span><span class="sxs-lookup"><span data-stu-id="88489-164">Encoding of data files</span></span>
2. <span data-ttu-id="88489-165">Formatet för datafiler</span><span class="sxs-lookup"><span data-stu-id="88489-165">Format of data files</span></span>
3. <span data-ttu-id="88489-166">Platsen för datafiler</span><span class="sxs-lookup"><span data-stu-id="88489-166">Location of data files</span></span>

### <a name="encoding"></a><span data-ttu-id="88489-167">Encoding</span><span class="sxs-lookup"><span data-stu-id="88489-167">Encoding</span></span>
<span data-ttu-id="88489-168">PolyBase kräver datafiler vara UTF-8- eller UTF-16FE.</span><span class="sxs-lookup"><span data-stu-id="88489-168">PolyBase requires data files to be UTF-8 or UTF-16FE.</span></span> 



### <a name="format-of-data-files"></a><span data-ttu-id="88489-169">Formatet för datafiler</span><span class="sxs-lookup"><span data-stu-id="88489-169">Format of data files</span></span>
<span data-ttu-id="88489-170">PolyBase kräver en fast radbegränsaren \n eller ny rad.</span><span class="sxs-lookup"><span data-stu-id="88489-170">PolyBase mandates a fixed row terminator of \n or newline.</span></span> <span data-ttu-id="88489-171">Datafiler måste följa standarden.</span><span class="sxs-lookup"><span data-stu-id="88489-171">Your data files must conform to this standard.</span></span> <span data-ttu-id="88489-172">Det inte finns några begränsningar på avslutarna sträng eller en kolumn.</span><span class="sxs-lookup"><span data-stu-id="88489-172">There aren't any restrictions on string or column terminators.</span></span>

<span data-ttu-id="88489-173">Du måste definiera alla kolumner i filen som en del av en extern tabell i PolyBase.</span><span class="sxs-lookup"><span data-stu-id="88489-173">You will have to define every column in the file as part of your external table in PolyBase.</span></span> <span data-ttu-id="88489-174">Se till att alla exporterade kolumnerna är nödvändiga och att typerna som överensstämmer med kraven.</span><span class="sxs-lookup"><span data-stu-id="88489-174">Make sure that all exported columns are required and that the types conform to the required standards.</span></span>

<span data-ttu-id="88489-175">Kontrollera refererar tillbaka till den [migrera schemat] artikeln för information om vilka datatyper.</span><span class="sxs-lookup"><span data-stu-id="88489-175">Please refer back to the [migrate your schema] article for detail on supported data types.</span></span>

### <a name="location-of-data-files"></a><span data-ttu-id="88489-176">Platsen för datafiler</span><span class="sxs-lookup"><span data-stu-id="88489-176">Location of data files</span></span>
<span data-ttu-id="88489-177">SQL Data Warehouse använder PolyBase för att läsa in data från Azure Blob Storage exklusivt.</span><span class="sxs-lookup"><span data-stu-id="88489-177">SQL Data Warehouse uses PolyBase to load data from Azure Blob Storage exclusively.</span></span> <span data-ttu-id="88489-178">Följaktligen måste data ha först överförts till blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="88489-178">Consequently, the data must have been first transferred into blob storage.</span></span>

## <a name="optimizing-data-transfer"></a><span data-ttu-id="88489-179">Optimera dataöverföring</span><span class="sxs-lookup"><span data-stu-id="88489-179">Optimizing data transfer</span></span>
<span data-ttu-id="88489-180">En av de långsammaste delarna av datamigrering är överföring av data till Azure.</span><span class="sxs-lookup"><span data-stu-id="88489-180">One of the slowest parts of data migration is the transfer of the data to Azure.</span></span> <span data-ttu-id="88489-181">Inte bara nätverkets bandbredd kan bli ett problem utan också nätverket tillförlitlighet kan försvåra pågår.</span><span class="sxs-lookup"><span data-stu-id="88489-181">Not only can network bandwidth be an issue but also network reliability can seriously hamper progress.</span></span> <span data-ttu-id="88489-182">Är på Internet så att risken för överföringen eventuella fel som inträffar är rimligen kan som standard migrera data till Azure.</span><span class="sxs-lookup"><span data-stu-id="88489-182">By default migrating data to Azure is over the internet so the chances of transfer errors occurring are reasonably likely.</span></span> <span data-ttu-id="88489-183">Dessa fel kan dock kräver att data skickas på nytt helt eller delvis.</span><span class="sxs-lookup"><span data-stu-id="88489-183">However, these errors may require data to be re-sent either in whole or in part.</span></span>

<span data-ttu-id="88489-184">Lyckligtvis har du flera alternativ för att förbättra hastighet och återhämtning i den här processen:</span><span class="sxs-lookup"><span data-stu-id="88489-184">Fortunately you have several options to improve the speed and resilience of this process:</span></span>

### <a name="expressrouteexpressroute"></a><span data-ttu-id="88489-185">[ExpressRoute][ExpressRoute]</span><span class="sxs-lookup"><span data-stu-id="88489-185">[ExpressRoute][ExpressRoute]</span></span>
<span data-ttu-id="88489-186">Du kanske vill använda [ExpressRoute] [ ExpressRoute] påskynda överföringen.</span><span class="sxs-lookup"><span data-stu-id="88489-186">You may want to consider using [ExpressRoute][ExpressRoute] to speed up the transfer.</span></span> <span data-ttu-id="88489-187">[ExpressRoute] [ ExpressRoute] innehåller en privat upprättad anslutning till Azure så att anslutningen inte går via det offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="88489-187">[ExpressRoute][ExpressRoute] provides you with an established private connection to Azure so the connection does not go over the public internet.</span></span> <span data-ttu-id="88489-188">Under inga omständigheter är ett obligatoriskt steg.</span><span class="sxs-lookup"><span data-stu-id="88489-188">This is by no means a mandatory step.</span></span> <span data-ttu-id="88489-189">Dock förbättras dataflöde när skicka data till Azure från en lokal eller samplacering.</span><span class="sxs-lookup"><span data-stu-id="88489-189">However, it will improve throughput when pushing data to Azure from an on-premises or co-location facility.</span></span>

<span data-ttu-id="88489-190">Fördelarna med att använda [ExpressRoute] [ ExpressRoute] är:</span><span class="sxs-lookup"><span data-stu-id="88489-190">The benefits of using [ExpressRoute][ExpressRoute] are:</span></span>

1. <span data-ttu-id="88489-191">Ökad tillförlitlighet</span><span class="sxs-lookup"><span data-stu-id="88489-191">Increased reliability</span></span>
2. <span data-ttu-id="88489-192">Snabbare nätverkshastigheten</span><span class="sxs-lookup"><span data-stu-id="88489-192">Faster network speed</span></span>
3. <span data-ttu-id="88489-193">Lägre nätverksfördröjning</span><span class="sxs-lookup"><span data-stu-id="88489-193">Lower network latency</span></span>
4. <span data-ttu-id="88489-194">högre nätverkssäkerhet</span><span class="sxs-lookup"><span data-stu-id="88489-194">higher network security</span></span>

<span data-ttu-id="88489-195">[ExpressRoute] [ ExpressRoute] är bra för ett antal scenarier, inte bara migreringen.</span><span class="sxs-lookup"><span data-stu-id="88489-195">[ExpressRoute][ExpressRoute] is beneficial for a number of scenarios; not just the migration.</span></span>

<span data-ttu-id="88489-196">Är du intresserad?</span><span class="sxs-lookup"><span data-stu-id="88489-196">Interested?</span></span> <span data-ttu-id="88489-197">Mer information och ange priser finns i [ExpressRoute dokumentation][ExpressRoute documentation].</span><span class="sxs-lookup"><span data-stu-id="88489-197">For more information and pricing please visit the [ExpressRoute documentation][ExpressRoute documentation].</span></span>

### <a name="azure-import-and-export-service"></a><span data-ttu-id="88489-198">Azure Import och Export Service</span><span class="sxs-lookup"><span data-stu-id="88489-198">Azure Import and Export Service</span></span>
<span data-ttu-id="88489-199">Azure Import och Export Service är en process för överföring av data utformad för stora (GB ++) på massiv (TB ++) överföring av data till Azure.</span><span class="sxs-lookup"><span data-stu-id="88489-199">The Azure Import and Export Service is a data transfer process designed for large (GB++) to massive (TB++) transfers of data into Azure.</span></span> <span data-ttu-id="88489-200">Det innebär att skriva data till diskar och leverera dem till ett Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="88489-200">It involves writing your data to disks and shipping them to an Azure data center.</span></span> <span data-ttu-id="88489-201">Diskinnehållet läses sedan in i Azure Storage Blobs å dina vägnar.</span><span class="sxs-lookup"><span data-stu-id="88489-201">The disk contents will then be loaded into Azure Storage Blobs on your behalf.</span></span>

<span data-ttu-id="88489-202">En översikt över för export importen är följande:</span><span class="sxs-lookup"><span data-stu-id="88489-202">A high-level view of the import export process is as follows:</span></span>

1. <span data-ttu-id="88489-203">Konfigurera en Azure Blob Storage-behållare för att ta emot data</span><span class="sxs-lookup"><span data-stu-id="88489-203">Configure an Azure Blob Storage container to receive the data</span></span>
2. <span data-ttu-id="88489-204">Exportera data till lokal lagring</span><span class="sxs-lookup"><span data-stu-id="88489-204">Export your data to local storage</span></span>
3. <span data-ttu-id="88489-205">Kopiera data till 3,5-tums SATA II/III hårddiskar verktyget [Azure Import/Export]</span><span class="sxs-lookup"><span data-stu-id="88489-205">Copy the data to 3.5 inch SATA II/III hard disk drives using the [Azure Import/Export Tool]</span></span>
4. <span data-ttu-id="88489-206">Skapa en Import-jobb med hjälp av Azure Import och Export-tjänst som tillhandahåller journalfiler som produceras av [Azure Import/Export verktyg]</span><span class="sxs-lookup"><span data-stu-id="88489-206">Create an Import Job using the Azure Import and Export Service providing the journal files produced by the [Azure Import/Export Tool]</span></span>
5. <span data-ttu-id="88489-207">Leverera diskarna utsedda Azure datacentret</span><span class="sxs-lookup"><span data-stu-id="88489-207">Ship the disks your nominated Azure data center</span></span>
6. <span data-ttu-id="88489-208">Dina data överförs till Azure Blob Storage-behållare</span><span class="sxs-lookup"><span data-stu-id="88489-208">Your data is transferred to your Azure Blob Storage container</span></span>
7. <span data-ttu-id="88489-209">Läsa in data i SQLDW med PolyBase</span><span class="sxs-lookup"><span data-stu-id="88489-209">Load the data into SQLDW using PolyBase</span></span>

### <a name="azcopyazcopy-utility"></a><span data-ttu-id="88489-210">[AZCopy][AZCopy] verktyget</span><span class="sxs-lookup"><span data-stu-id="88489-210">[AZCopy][AZCopy] utility</span></span>
<span data-ttu-id="88489-211">Den [AZCopy][AZCopy] verktyget är ett bra verktyg för att hämta dina data som överförs till Azure Storage-Blobbar.</span><span class="sxs-lookup"><span data-stu-id="88489-211">The [AZCopy][AZCopy] utility is a great tool for getting your data transferred into Azure Storage Blobs.</span></span> <span data-ttu-id="88489-212">Den är utformad för små (MB ++) till mycket stora (GB ++) överföring av data.</span><span class="sxs-lookup"><span data-stu-id="88489-212">It is designed for small (MB++) to very large (GB++) data transfers.</span></span> <span data-ttu-id="88489-213">[AZCopy] har också utformats för att ge bra flexibel dataflöde vid överföring av data till Azure och det är ett bra alternativ för steget data transfer.</span><span class="sxs-lookup"><span data-stu-id="88489-213">[AZCopy] has also been designed to provide good resilient throughput when transferring data to Azure and so is a great choice for the data transfer step.</span></span> <span data-ttu-id="88489-214">En gång överförda kan du läsa in data med PolyBase i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="88489-214">Once transferred you can load the data using PolyBase into SQL Data Warehouse.</span></span> <span data-ttu-id="88489-215">Du kan även inkludera AZCopy i din SSIS-paket med hjälp av en uppgift ”köra processen”.</span><span class="sxs-lookup"><span data-stu-id="88489-215">You can also incorporate AZCopy into your SSIS packages using an "Execute Process" task.</span></span>

<span data-ttu-id="88489-216">Om du vill använda AZCopy måste du först hämta och installera den.</span><span class="sxs-lookup"><span data-stu-id="88489-216">To use AZCopy you will first need to download and install it.</span></span> <span data-ttu-id="88489-217">Det finns en [produktionsversion] [ production version] och en [förhandsversionen] [ preview version] tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="88489-217">There is a [production version][production version] and a [preview version][preview version] available.</span></span>

<span data-ttu-id="88489-218">Om du vill överföra en fil från din dator behöver du ett kommando som i exemplet nedan:</span><span class="sxs-lookup"><span data-stu-id="88489-218">To upload a file from your file system you will need a command like the one below:</span></span>

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="88489-219">En sammanfattning av övergripande processen kan vara:</span><span class="sxs-lookup"><span data-stu-id="88489-219">A high-level process summary could be:</span></span>

1. <span data-ttu-id="88489-220">Konfigurera en Azure storage blob-behållare för att ta emot data</span><span class="sxs-lookup"><span data-stu-id="88489-220">Configure an Azure storage blob container to receive the data</span></span>
2. <span data-ttu-id="88489-221">Exportera data till lokal lagring</span><span class="sxs-lookup"><span data-stu-id="88489-221">Export your data to local storage</span></span>
3. <span data-ttu-id="88489-222">AZCopy dina data i Azure Blob Storage-behållare</span><span class="sxs-lookup"><span data-stu-id="88489-222">AZCopy your data in the Azure Blob Storage container</span></span>
4. <span data-ttu-id="88489-223">Läsa in data till SQL Data Warehouse med PolyBase</span><span class="sxs-lookup"><span data-stu-id="88489-223">Load the data into SQL Data Warehouse using PolyBase</span></span>

<span data-ttu-id="88489-224">Fullständig dokumentation tillgänglig: [AZCopy][AZCopy].</span><span class="sxs-lookup"><span data-stu-id="88489-224">Full documentation available: [AZCopy][AZCopy].</span></span>

## <a name="optimizing-data-export"></a><span data-ttu-id="88489-225">Optimera export av data</span><span class="sxs-lookup"><span data-stu-id="88489-225">Optimizing data export</span></span>
<span data-ttu-id="88489-226">Förutom att säkerställa att exporten överensstämmer med kraven av PolyBase begära du att optimera export av data till förbättra processen ytterligare.</span><span class="sxs-lookup"><span data-stu-id="88489-226">In addition to ensuring that the export conforms to the requirements laid out by PolyBase you can also seek to optimize the export of the data to improve the process further.</span></span>



### <a name="data-compression"></a><span data-ttu-id="88489-227">Datakomprimering</span><span class="sxs-lookup"><span data-stu-id="88489-227">Data compression</span></span>
<span data-ttu-id="88489-228">PolyBase kan läsa gzip komprimerade data.</span><span class="sxs-lookup"><span data-stu-id="88489-228">PolyBase can read gzip compressed data.</span></span> <span data-ttu-id="88489-229">Om du ska kunna komprimera data till gzip-filer kan du minimera mängden data som skickas över nätverket.</span><span class="sxs-lookup"><span data-stu-id="88489-229">If you are able to compress your data to gzip files then you will minimize the amount of data being pushed over the network.</span></span>

### <a name="multiple-files"></a><span data-ttu-id="88489-230">Flera filer</span><span class="sxs-lookup"><span data-stu-id="88489-230">Multiple files</span></span>
<span data-ttu-id="88489-231">Dela upp stora tabeller i flera filer hjälper inte bara att exportera snabbare, hjälper också med överföring re-startability och den övergripande hanteringen av data en gång i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="88489-231">Breaking up large tables into several files not only helps to improve export speed, it also helps with transfer re-startability, and the overall manageability of the data once in the Azure blob storage.</span></span> <span data-ttu-id="88489-232">En av de många bra funktionerna för PolyBase är att den ska läsa alla filer i en mapp och behandla det som en tabell.</span><span class="sxs-lookup"><span data-stu-id="88489-232">One of the many nice features of PolyBase is that it will read all the files inside a folder and treat it as one table.</span></span> <span data-ttu-id="88489-233">Det är därför en bra idé att isolera filer för varje tabell till en egen mapp.</span><span class="sxs-lookup"><span data-stu-id="88489-233">It is therefore a good idea to isolate the files for each table into its own folder.</span></span>

<span data-ttu-id="88489-234">PolyBase stöder också en funktion som kallas ”rekursiv mappen traversal”.</span><span class="sxs-lookup"><span data-stu-id="88489-234">PolyBase also supports a feature known as "recursive folder traversal".</span></span> <span data-ttu-id="88489-235">Du kan använda den här funktionen för att ytterligare förbättra organisationen för att förbättra din datahantering exporterade data.</span><span class="sxs-lookup"><span data-stu-id="88489-235">You can use this feature to further enhance the organization of your exported data to improve your data management.</span></span>

<span data-ttu-id="88489-236">Mer information om hur du läser data med PolyBase finns [Använd PolyBase för att läsa in data till SQL Data Warehouse][Use PolyBase to load data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="88489-236">To learn more about loading data with PolyBase, see [Use PolyBase to load data into SQL Data Warehouse][Use PolyBase to load data into SQL Data Warehouse].</span></span>

## <a name="next-steps"></a><span data-ttu-id="88489-237">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="88489-237">Next steps</span></span>
<span data-ttu-id="88489-238">Mer information om migrering finns i [migrera lösningen till SQL Data Warehouse][Migrate your solution to SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="88489-238">For more about migration, see [Migrate your solution to SQL Data Warehouse][Migrate your solution to SQL Data Warehouse].</span></span>
<span data-ttu-id="88489-239">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="88489-239">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[AZCopy]:../storage/common/storage-use-azcopy.md
[ADF Copy]: ../data-factory/data-factory-data-movement-activities.md 
[ADF samples]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[development overview]: sql-data-warehouse-overview-develop.md
[Migrate your solution to SQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Use bcp to load data into SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[Use PolyBase to load data into SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute documentation]: http://azure.microsoft.com/documentation/services/expressroute/

[production version]: http://aka.ms/downloadazcopy/
[preview version]: http://aka.ms/downloadazcopypr/
[ADO.NET destination adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS documentation]: https://msdn.microsoft.com/library/ms141026.aspx
