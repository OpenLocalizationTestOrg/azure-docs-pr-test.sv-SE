---
title: aaaMigrate dina data tooSQL Data Warehouse | Microsoft Docs
description: "Tips för att migrera dina data tooAzure SQL Data Warehouse för utveckling av lösningar."
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
ms.openlocfilehash: fe4c6b7e82094c59c45e06be6da225fee1b707ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-data"></a><span data-ttu-id="afc8f-103">Migrera dina Data</span><span class="sxs-lookup"><span data-stu-id="afc8f-103">Migrate Your Data</span></span>
<span data-ttu-id="afc8f-104">Data kan flyttas från olika källor till din SQL Data Warehouse med en olika verktyg.</span><span class="sxs-lookup"><span data-stu-id="afc8f-104">Data can be moved from different sources into your SQL Data Warehouse with a variety tools.</span></span>  <span data-ttu-id="afc8f-105">ADF kopiera, SSIS och bcp kan alla vara används tooachieve det här målet.</span><span class="sxs-lookup"><span data-stu-id="afc8f-105">ADF Copy, SSIS, and bcp can all be used tooachieve this goal.</span></span> <span data-ttu-id="afc8f-106">När hello mängden data ökar bör du tänka på bryta ned hello migrering av data i steg.</span><span class="sxs-lookup"><span data-stu-id="afc8f-106">However, as hello amount of data increases you should think about breaking down hello data migration process into steps.</span></span> <span data-ttu-id="afc8f-107">Detta en hello möjlighet toooptimize varje steg både för prestanda och återhämtning tooensure smooth datamigrering.</span><span class="sxs-lookup"><span data-stu-id="afc8f-107">This affords you hello opportunity toooptimize each step both for performance and for resilience tooensure a smooth data migration.</span></span>

<span data-ttu-id="afc8f-108">Den här artikeln beskrivs först hello enkla Migreringsscenarier ADF kopia, SSIS och bcp.</span><span class="sxs-lookup"><span data-stu-id="afc8f-108">This article first discusses hello simple migration scenarios of ADF Copy, SSIS, and bcp.</span></span> <span data-ttu-id="afc8f-109">Den sedan studera lite djupare hur hello migrering kan optimeras.</span><span class="sxs-lookup"><span data-stu-id="afc8f-109">It then look a little deeper into how hello migration can be optimized.</span></span>

## <a name="azure-data-factory-adf-copy"></a><span data-ttu-id="afc8f-110">Azure Data Factory (ADM) kopiera</span><span class="sxs-lookup"><span data-stu-id="afc8f-110">Azure Data Factory (ADF) copy</span></span>
<span data-ttu-id="afc8f-111">[Kopiera ADF] [ ADF Copy] är en del av [Azure Data Factory][Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="afc8f-111">[ADF Copy][ADF Copy] is part of [Azure Data Factory][Azure Data Factory].</span></span> <span data-ttu-id="afc8f-112">Du kan använda ADF kopiera tooexport dina tooflat filer som finns på lokal lagring tooremote flat-filer som lagras i Azure blob storage eller direkt till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="afc8f-112">You can use ADF Copy tooexport your data tooflat files residing on local storage, tooremote flat files held in Azure blob storage or directly into SQL Data Warehouse.</span></span>

<span data-ttu-id="afc8f-113">Om dina data startar i flat-filer, så måste du först tootransfer den tooAzure lagringsblob innan du påbörjar en inläsning till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="afc8f-113">If your data starts in flat files, then you will first need tootransfer it tooAzure storage blob before initiating a load it into SQL Data Warehouse.</span></span> <span data-ttu-id="afc8f-114">När hello data överförs till Azure blob storage kan du välja toouse [ADF kopiera] [ ADF Copy] igen toopush hello data till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="afc8f-114">Once hello data is transferred into Azure blob storage you can choose toouse [ADF Copy][ADF Copy] again toopush hello data into SQL Data Warehouse.</span></span>

<span data-ttu-id="afc8f-115">PolyBase ger också ett alternativ för högpresterande för hello data läses in.</span><span class="sxs-lookup"><span data-stu-id="afc8f-115">PolyBase also provides a high-performance option for loading hello data.</span></span> <span data-ttu-id="afc8f-116">Dock innebär som använder två verktyg i stället för en.</span><span class="sxs-lookup"><span data-stu-id="afc8f-116">However, that does mean using two tools instead of one.</span></span> <span data-ttu-id="afc8f-117">Om du behöver hello bästa prestanda och Använd sedan PolyBase.</span><span class="sxs-lookup"><span data-stu-id="afc8f-117">If you need hello best performance then use PolyBase.</span></span> <span data-ttu-id="afc8f-118">Om du vill använda en enda verktyg upplevelse (inte och hello data massiv) är ADF svaret.</span><span class="sxs-lookup"><span data-stu-id="afc8f-118">If you want a single tool experience (and hello data is not massive) then ADF is your answer.</span></span>


> 
> 

<span data-ttu-id="afc8f-119">HEAD över toohello följande artikel för vissa bra [ADF prover][ADF samples].</span><span class="sxs-lookup"><span data-stu-id="afc8f-119">Head over toohello following article for some great [ADF samples][ADF samples].</span></span>

## <a name="integration-services"></a><span data-ttu-id="afc8f-120">Integrationstjänster</span><span class="sxs-lookup"><span data-stu-id="afc8f-120">Integration Services</span></span>
<span data-ttu-id="afc8f-121">Integration Services (SSIS) är ett kraftfullt och flexibelt extrahera Transform och Load (ETL) som stöder komplexa arbetsflöden, DTS och flera alternativ för inläsning av data.</span><span class="sxs-lookup"><span data-stu-id="afc8f-121">Integration Services (SSIS) is a powerful and flexible Extract Transform and Load (ETL) tool that supports complex workflows, data transformation, and several data loading options.</span></span> <span data-ttu-id="afc8f-122">Använd SSIS toosimply överför data tooAzure eller som en del av en bredare migrering.</span><span class="sxs-lookup"><span data-stu-id="afc8f-122">Use SSIS toosimply transfer data tooAzure or as part of a broader migration.</span></span>

> [!NOTE]
> <span data-ttu-id="afc8f-123">SSIS kan exportera tooUTF 8 utan hello byte-ordningsmarkering i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="afc8f-123">SSIS can export tooUTF-8 without hello byte order mark in hello file.</span></span> <span data-ttu-id="afc8f-124">Detta måste du först använda hello härledda teckendata för kolumnen komponenten tooconvert hello i tooconfigure hello data flödet toouse hello 65001 UTF-8-teckentabellen.</span><span class="sxs-lookup"><span data-stu-id="afc8f-124">tooconfigure this you must first use hello derived column component tooconvert hello character data in hello data flow toouse hello 65001 UTF-8 code page.</span></span> <span data-ttu-id="afc8f-125">När hello kolumner har konverterats skriva hello data toohello flat fil målet nätverkskort att säkerställa att 65001 också har markerats som hello teckentabell för hello-filen.</span><span class="sxs-lookup"><span data-stu-id="afc8f-125">Once hello columns have been converted, write hello data toohello flat file destination adapter ensuring that 65001 has also been selected as hello code page for hello file.</span></span>
> 
> 

<span data-ttu-id="afc8f-126">SSIS ansluter tooSQL datalagret precis som den ska ansluta tooa SQL Server-distributionen.</span><span class="sxs-lookup"><span data-stu-id="afc8f-126">SSIS connects tooSQL Data Warehouse just as it would connect tooa SQL Server deployment.</span></span> <span data-ttu-id="afc8f-127">Anslutningarna behöver toobe som använder en ADO.NET Anslutningshanteraren.</span><span class="sxs-lookup"><span data-stu-id="afc8f-127">However, your connections will need toobe using an ADO.NET connection manager.</span></span> <span data-ttu-id="afc8f-128">Också noga tooconfigure hello ”Använd massinfogning när det är tillgängligt” inställningen toomaximize genomflöde.</span><span class="sxs-lookup"><span data-stu-id="afc8f-128">You should also take care tooconfigure hello "Use bulk insert when available" setting toomaximize throughput.</span></span> <span data-ttu-id="afc8f-129">Se toohello [ADO.NET-målnätverkskort] [ ADO.NET destination adapter] artikel toolearn mer om den här egenskapen</span><span class="sxs-lookup"><span data-stu-id="afc8f-129">Please refer toohello [ADO.NET destination adapter][ADO.NET destination adapter] article toolearn more about this property</span></span>

> [!NOTE]
> <span data-ttu-id="afc8f-130">Det går inte att ansluta tooAzure SQL Data Warehouse med hjälp av OLEDB.</span><span class="sxs-lookup"><span data-stu-id="afc8f-130">Connecting tooAzure SQL Data Warehouse by using OLEDB is not supported.</span></span>
> 
> 

<span data-ttu-id="afc8f-131">Dessutom finns alltid hello möjligheten att ett paket kan misslyckas på grund av problem med toothrottling eller nätverket.</span><span class="sxs-lookup"><span data-stu-id="afc8f-131">In addition, there is always hello possibility that a package might fail due toothrottling or network issues.</span></span> <span data-ttu-id="afc8f-132">Design paket så att de kan återupptas vid hello felpunkt utan göra om fungerar som slutförts innan hello misslyckades.</span><span class="sxs-lookup"><span data-stu-id="afc8f-132">Design packages so they can be resumed at hello point of failure, without redoing work that completed before hello failure.</span></span>

<span data-ttu-id="afc8f-133">Mer information finns i hello [SSIS-dokumentationen][SSIS documentation].</span><span class="sxs-lookup"><span data-stu-id="afc8f-133">For more information consult hello [SSIS documentation][SSIS documentation].</span></span>

## <a name="bcp"></a><span data-ttu-id="afc8f-134">bcp</span><span class="sxs-lookup"><span data-stu-id="afc8f-134">bcp</span></span>
<span data-ttu-id="afc8f-135">BCP är ett kommandoradsverktyg som har utformats för flat fil dataimport och export.</span><span class="sxs-lookup"><span data-stu-id="afc8f-135">bcp is a command-line utility that is designed for flat file data import and export.</span></span> <span data-ttu-id="afc8f-136">Vissa omvandling kan ske under export av data.</span><span class="sxs-lookup"><span data-stu-id="afc8f-136">Some transformation can take place during data export.</span></span> <span data-ttu-id="afc8f-137">tooperform enkel transformationer använder en fråga tooselect och transformera hello data.</span><span class="sxs-lookup"><span data-stu-id="afc8f-137">tooperform simple transformations use a query tooselect and transform hello data.</span></span> <span data-ttu-id="afc8f-138">När exporteras, kan sedan hello flata filer laddas direkt i hello måldatabasen hello SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="afc8f-138">Once exported, hello flat files can then be loaded directly into hello target hello SQL Data Warehouse database.</span></span>

> [!NOTE]
> <span data-ttu-id="afc8f-139">Det är ofta en bra idé tooencapsulate hello transformationer som används under data exportera i en vy på hello källsystemet.</span><span class="sxs-lookup"><span data-stu-id="afc8f-139">It is often a good idea tooencapsulate hello transformations used during data export in a view on hello source system.</span></span> <span data-ttu-id="afc8f-140">Detta säkerställer att hello logik sparas och hello processen repeterbara.</span><span class="sxs-lookup"><span data-stu-id="afc8f-140">This ensures that hello logic is retained and hello process is repeatable.</span></span>
> 
> 

<span data-ttu-id="afc8f-141">Fördelarna med bcp är:</span><span class="sxs-lookup"><span data-stu-id="afc8f-141">Advantages of bcp are:</span></span>

* <span data-ttu-id="afc8f-142">Enkelhet.</span><span class="sxs-lookup"><span data-stu-id="afc8f-142">Simplicity.</span></span> <span data-ttu-id="afc8f-143">BCP kommandon är enkla toobuild och köra</span><span class="sxs-lookup"><span data-stu-id="afc8f-143">bcp commands are simple toobuild and execute</span></span>
* <span data-ttu-id="afc8f-144">Nytt starta inläsningen.</span><span class="sxs-lookup"><span data-stu-id="afc8f-144">Re-startable load process.</span></span> <span data-ttu-id="afc8f-145">En gång exporterade hello inläsningen kan vara körs många gånger</span><span class="sxs-lookup"><span data-stu-id="afc8f-145">Once exported hello load can be executed any number of times</span></span>

<span data-ttu-id="afc8f-146">Begränsningar av bcp är:</span><span class="sxs-lookup"><span data-stu-id="afc8f-146">Limitations of bcp are:</span></span>

* <span data-ttu-id="afc8f-147">BCP fungerar med endast tabellform flata filer.</span><span class="sxs-lookup"><span data-stu-id="afc8f-147">bcp works with tabulated flat files only.</span></span> <span data-ttu-id="afc8f-148">Det fungerar inte med filer som XML- eller JSON</span><span class="sxs-lookup"><span data-stu-id="afc8f-148">It does not work with files such as xml or JSON</span></span>
* <span data-ttu-id="afc8f-149">Data transformation funktioner är begränsade toohello export endast mellanlagring och är enkel till sin natur</span><span class="sxs-lookup"><span data-stu-id="afc8f-149">Data transformation capabilities are limited toohello export stage only and are simple in nature</span></span>
* <span data-ttu-id="afc8f-150">BCP har inte anpassats toobe robust vid läsning av data över hello internet.</span><span class="sxs-lookup"><span data-stu-id="afc8f-150">bcp has not been adapted toobe robust when loading data over hello internet.</span></span> <span data-ttu-id="afc8f-151">Instabilitet nätverket kan orsaka en Inläsningsfel.</span><span class="sxs-lookup"><span data-stu-id="afc8f-151">Any network instability may cause a load error.</span></span>
* <span data-ttu-id="afc8f-152">BCP är beroende av hello schemat finns i hello mål databasen tidigare toohello belastning</span><span class="sxs-lookup"><span data-stu-id="afc8f-152">bcp relies on hello schema being present in hello target database prior toohello load</span></span>

<span data-ttu-id="afc8f-153">Mer information finns i [använda bcp tooload data till SQL Data Warehouse][Use bcp tooload data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="afc8f-153">For more information, see [Use bcp tooload data into SQL Data Warehouse][Use bcp tooload data into SQL Data Warehouse].</span></span>

## <a name="optimizing-data-migration"></a><span data-ttu-id="afc8f-154">Optimera datamigrering</span><span class="sxs-lookup"><span data-stu-id="afc8f-154">Optimizing data migration</span></span>
<span data-ttu-id="afc8f-155">Migrerar en SQLDW data kan delas effektivt upp till tre separata steg:</span><span class="sxs-lookup"><span data-stu-id="afc8f-155">A SQLDW data migration process can be effectively broken down into three discrete steps:</span></span>

1. <span data-ttu-id="afc8f-156">Export av källdata</span><span class="sxs-lookup"><span data-stu-id="afc8f-156">Export of source data</span></span>
2. <span data-ttu-id="afc8f-157">Överföring av data tooAzure</span><span class="sxs-lookup"><span data-stu-id="afc8f-157">Transfer of data tooAzure</span></span>
3. <span data-ttu-id="afc8f-158">Läs in till hello måldatabasen SQLDW</span><span class="sxs-lookup"><span data-stu-id="afc8f-158">Load into hello target SQLDW database</span></span>

<span data-ttu-id="afc8f-159">Varje steg kan vara individuellt optimerade toocreate en robust, starta om och flexibel migreringsprocessen som maximerar prestandan i varje steg.</span><span class="sxs-lookup"><span data-stu-id="afc8f-159">Each step can be individually optimized toocreate a robust, re-startable and resilient migration process that maximizes performance at each step.</span></span>

## <a name="optimizing-data-load"></a><span data-ttu-id="afc8f-160">Optimera datainläsning</span><span class="sxs-lookup"><span data-stu-id="afc8f-160">Optimizing data load</span></span>
<span data-ttu-id="afc8f-161">Titta på dessa i omvänd ordning för ett ögonblick; hello snabbaste sättet tooload data är via PolyBase.</span><span class="sxs-lookup"><span data-stu-id="afc8f-161">Looking at these in reverse order for a moment; hello fastest way tooload data is via PolyBase.</span></span> <span data-ttu-id="afc8f-162">Optimera för en PolyBase-inläsningen placeras krav hello föregående steg så att det är bästa toounderstand detta summa.</span><span class="sxs-lookup"><span data-stu-id="afc8f-162">Optimizing for a PolyBase load process places prerequisites on hello preceding steps so it's best toounderstand this upfront.</span></span> <span data-ttu-id="afc8f-163">De är:</span><span class="sxs-lookup"><span data-stu-id="afc8f-163">They are:</span></span>

1. <span data-ttu-id="afc8f-164">Kodning av datafiler</span><span class="sxs-lookup"><span data-stu-id="afc8f-164">Encoding of data files</span></span>
2. <span data-ttu-id="afc8f-165">Formatet för datafiler</span><span class="sxs-lookup"><span data-stu-id="afc8f-165">Format of data files</span></span>
3. <span data-ttu-id="afc8f-166">Platsen för datafiler</span><span class="sxs-lookup"><span data-stu-id="afc8f-166">Location of data files</span></span>

### <a name="encoding"></a><span data-ttu-id="afc8f-167">Encoding</span><span class="sxs-lookup"><span data-stu-id="afc8f-167">Encoding</span></span>
<span data-ttu-id="afc8f-168">PolyBase kräver data filer toobe UTF-8- eller UTF-16FE.</span><span class="sxs-lookup"><span data-stu-id="afc8f-168">PolyBase requires data files toobe UTF-8 or UTF-16FE.</span></span> 



### <a name="format-of-data-files"></a><span data-ttu-id="afc8f-169">Formatet för datafiler</span><span class="sxs-lookup"><span data-stu-id="afc8f-169">Format of data files</span></span>
<span data-ttu-id="afc8f-170">PolyBase kräver en fast radbegränsaren \n eller ny rad.</span><span class="sxs-lookup"><span data-stu-id="afc8f-170">PolyBase mandates a fixed row terminator of \n or newline.</span></span> <span data-ttu-id="afc8f-171">Datafiler måste följa toothis standard.</span><span class="sxs-lookup"><span data-stu-id="afc8f-171">Your data files must conform toothis standard.</span></span> <span data-ttu-id="afc8f-172">Det inte finns några begränsningar på avslutarna sträng eller en kolumn.</span><span class="sxs-lookup"><span data-stu-id="afc8f-172">There aren't any restrictions on string or column terminators.</span></span>

<span data-ttu-id="afc8f-173">Du måste toodefine alla kolumner i hello-filen som en del av en extern tabell i PolyBase.</span><span class="sxs-lookup"><span data-stu-id="afc8f-173">You will have toodefine every column in hello file as part of your external table in PolyBase.</span></span> <span data-ttu-id="afc8f-174">Se till att alla exporterade kolumnerna är nödvändiga och att hello typer överensstämmer toohello krävs standarder.</span><span class="sxs-lookup"><span data-stu-id="afc8f-174">Make sure that all exported columns are required and that hello types conform toohello required standards.</span></span>

<span data-ttu-id="afc8f-175">Se tillbaka toohello [migrera schemat] artikeln för information om vilka datatyper.</span><span class="sxs-lookup"><span data-stu-id="afc8f-175">Please refer back toohello [migrate your schema] article for detail on supported data types.</span></span>

### <a name="location-of-data-files"></a><span data-ttu-id="afc8f-176">Platsen för datafiler</span><span class="sxs-lookup"><span data-stu-id="afc8f-176">Location of data files</span></span>
<span data-ttu-id="afc8f-177">SQL Data Warehouse använder uteslutande PolyBase tooload data från Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="afc8f-177">SQL Data Warehouse uses PolyBase tooload data from Azure Blob Storage exclusively.</span></span> <span data-ttu-id="afc8f-178">Följaktligen måste hello data ha först överförts till blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="afc8f-178">Consequently, hello data must have been first transferred into blob storage.</span></span>

## <a name="optimizing-data-transfer"></a><span data-ttu-id="afc8f-179">Optimera dataöverföring</span><span class="sxs-lookup"><span data-stu-id="afc8f-179">Optimizing data transfer</span></span>
<span data-ttu-id="afc8f-180">En av hello långsammaste delar av datamigrering är hello överföringen av hello data tooAzure.</span><span class="sxs-lookup"><span data-stu-id="afc8f-180">One of hello slowest parts of data migration is hello transfer of hello data tooAzure.</span></span> <span data-ttu-id="afc8f-181">Inte bara nätverkets bandbredd kan bli ett problem utan också nätverket tillförlitlighet kan försvåra pågår.</span><span class="sxs-lookup"><span data-stu-id="afc8f-181">Not only can network bandwidth be an issue but also network reliability can seriously hamper progress.</span></span> <span data-ttu-id="afc8f-182">Migrera data tooAzure är över hello internet så hello risken för överföringen eventuella fel som inträffar rimligen kan som standard.</span><span class="sxs-lookup"><span data-stu-id="afc8f-182">By default migrating data tooAzure is over hello internet so hello chances of transfer errors occurring are reasonably likely.</span></span> <span data-ttu-id="afc8f-183">Dessa fel kan dock kräva data toobe skickas på nytt helt eller delvis.</span><span class="sxs-lookup"><span data-stu-id="afc8f-183">However, these errors may require data toobe re-sent either in whole or in part.</span></span>

<span data-ttu-id="afc8f-184">Lyckligtvis har du flera alternativ tooimprove hello hastighet och återhämtning i den här processen:</span><span class="sxs-lookup"><span data-stu-id="afc8f-184">Fortunately you have several options tooimprove hello speed and resilience of this process:</span></span>

### <a name="expressrouteexpressroute"></a><span data-ttu-id="afc8f-185">[ExpressRoute][ExpressRoute]</span><span class="sxs-lookup"><span data-stu-id="afc8f-185">[ExpressRoute][ExpressRoute]</span></span>
<span data-ttu-id="afc8f-186">Du kanske använder tooconsider [ExpressRoute] [ ExpressRoute] toospeed in hello-överföring.</span><span class="sxs-lookup"><span data-stu-id="afc8f-186">You may want tooconsider using [ExpressRoute][ExpressRoute] toospeed up hello transfer.</span></span> <span data-ttu-id="afc8f-187">[ExpressRoute] [ ExpressRoute] ger dig med en privat upprättad anslutning tooAzure så hello anslutning inte går hello offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="afc8f-187">[ExpressRoute][ExpressRoute] provides you with an established private connection tooAzure so hello connection does not go over hello public internet.</span></span> <span data-ttu-id="afc8f-188">Under inga omständigheter är ett obligatoriskt steg.</span><span class="sxs-lookup"><span data-stu-id="afc8f-188">This is by no means a mandatory step.</span></span> <span data-ttu-id="afc8f-189">Dock förbättras dataflöde vid push-installation tooAzure data från en lokal eller samplacering.</span><span class="sxs-lookup"><span data-stu-id="afc8f-189">However, it will improve throughput when pushing data tooAzure from an on-premises or co-location facility.</span></span>

<span data-ttu-id="afc8f-190">Hej fördelarna med att använda [ExpressRoute] [ ExpressRoute] är:</span><span class="sxs-lookup"><span data-stu-id="afc8f-190">hello benefits of using [ExpressRoute][ExpressRoute] are:</span></span>

1. <span data-ttu-id="afc8f-191">Ökad tillförlitlighet</span><span class="sxs-lookup"><span data-stu-id="afc8f-191">Increased reliability</span></span>
2. <span data-ttu-id="afc8f-192">Snabbare nätverkshastigheten</span><span class="sxs-lookup"><span data-stu-id="afc8f-192">Faster network speed</span></span>
3. <span data-ttu-id="afc8f-193">Lägre nätverksfördröjning</span><span class="sxs-lookup"><span data-stu-id="afc8f-193">Lower network latency</span></span>
4. <span data-ttu-id="afc8f-194">högre nätverkssäkerhet</span><span class="sxs-lookup"><span data-stu-id="afc8f-194">higher network security</span></span>

<span data-ttu-id="afc8f-195">[ExpressRoute] [ ExpressRoute] är bra för ett antal scenarier, inte bara hello migrering.</span><span class="sxs-lookup"><span data-stu-id="afc8f-195">[ExpressRoute][ExpressRoute] is beneficial for a number of scenarios; not just hello migration.</span></span>

<span data-ttu-id="afc8f-196">Är du intresserad?</span><span class="sxs-lookup"><span data-stu-id="afc8f-196">Interested?</span></span> <span data-ttu-id="afc8f-197">Mer information och priser du besöker hello [ExpressRoute dokumentation][ExpressRoute documentation].</span><span class="sxs-lookup"><span data-stu-id="afc8f-197">For more information and pricing please visit hello [ExpressRoute documentation][ExpressRoute documentation].</span></span>

### <a name="azure-import-and-export-service"></a><span data-ttu-id="afc8f-198">Azure Import och Export Service</span><span class="sxs-lookup"><span data-stu-id="afc8f-198">Azure Import and Export Service</span></span>
<span data-ttu-id="afc8f-199">hello är Azure importera och exportera Service en process för överföring av data utformad för stora (GB ++) toomassive (TB ++) överföringar av data till Azure.</span><span class="sxs-lookup"><span data-stu-id="afc8f-199">hello Azure Import and Export Service is a data transfer process designed for large (GB++) toomassive (TB++) transfers of data into Azure.</span></span> <span data-ttu-id="afc8f-200">Det innebär att skriva data-toodisks och leverera dem tooan Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="afc8f-200">It involves writing your data toodisks and shipping them tooan Azure data center.</span></span> <span data-ttu-id="afc8f-201">hello diskinnehållet läses sedan in i Azure Storage Blobs å dina vägnar.</span><span class="sxs-lookup"><span data-stu-id="afc8f-201">hello disk contents will then be loaded into Azure Storage Blobs on your behalf.</span></span>

<span data-ttu-id="afc8f-202">En översikt över för export av hello importen är följande:</span><span class="sxs-lookup"><span data-stu-id="afc8f-202">A high-level view of hello import export process is as follows:</span></span>

1. <span data-ttu-id="afc8f-203">Konfigurera en Azure Blob Storage-behållare tooreceive hello data</span><span class="sxs-lookup"><span data-stu-id="afc8f-203">Configure an Azure Blob Storage container tooreceive hello data</span></span>
2. <span data-ttu-id="afc8f-204">Exportera toolocal datalagringen</span><span class="sxs-lookup"><span data-stu-id="afc8f-204">Export your data toolocal storage</span></span>
3. <span data-ttu-id="afc8f-205">Kopiera hello data too3.5 tums SATA II/III hårddiskar med hello [Azure Import/Export verktyg]</span><span class="sxs-lookup"><span data-stu-id="afc8f-205">Copy hello data too3.5 inch SATA II/III hard disk drives using hello [Azure Import/Export Tool]</span></span>
4. <span data-ttu-id="afc8f-206">Skapa ett importjobb med hello Azure importera och exportera tjänst som tillhandahåller hello journalfiler produceras av hello [Azure Import/Export verktyg]</span><span class="sxs-lookup"><span data-stu-id="afc8f-206">Create an Import Job using hello Azure Import and Export Service providing hello journal files produced by hello [Azure Import/Export Tool]</span></span>
5. <span data-ttu-id="afc8f-207">Leverera hello diskar utsedda Azure datacentret</span><span class="sxs-lookup"><span data-stu-id="afc8f-207">Ship hello disks your nominated Azure data center</span></span>
6. <span data-ttu-id="afc8f-208">Dina data är överförda tooyour Azure Blob Storage-behållare</span><span class="sxs-lookup"><span data-stu-id="afc8f-208">Your data is transferred tooyour Azure Blob Storage container</span></span>
7. <span data-ttu-id="afc8f-209">Läs in hello data till SQLDW med PolyBase</span><span class="sxs-lookup"><span data-stu-id="afc8f-209">Load hello data into SQLDW using PolyBase</span></span>

### <a name="azcopyazcopy-utility"></a><span data-ttu-id="afc8f-210">[AZCopy][AZCopy] verktyget</span><span class="sxs-lookup"><span data-stu-id="afc8f-210">[AZCopy][AZCopy] utility</span></span>
<span data-ttu-id="afc8f-211">Hej [AZCopy][AZCopy] verktyget är ett bra verktyg för att hämta dina data som överförs till Azure Storage-Blobbar.</span><span class="sxs-lookup"><span data-stu-id="afc8f-211">hello [AZCopy][AZCopy] utility is a great tool for getting your data transferred into Azure Storage Blobs.</span></span> <span data-ttu-id="afc8f-212">Den är utformad för mindre (MB ++) toovery stora (GB ++) överföring av data.</span><span class="sxs-lookup"><span data-stu-id="afc8f-212">It is designed for small (MB++) toovery large (GB++) data transfers.</span></span> <span data-ttu-id="afc8f-213">[AZCopy] har också utformad tooprovide bra flexibel dataflöde vid överföring av data tooAzure och det är ett bra alternativ för steget hello-överföring.</span><span class="sxs-lookup"><span data-stu-id="afc8f-213">[AZCopy] has also been designed tooprovide good resilient throughput when transferring data tooAzure and so is a great choice for hello data transfer step.</span></span> <span data-ttu-id="afc8f-214">En gång överförda kan du läsa in hello-data med PolyBase i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="afc8f-214">Once transferred you can load hello data using PolyBase into SQL Data Warehouse.</span></span> <span data-ttu-id="afc8f-215">Du kan även inkludera AZCopy i din SSIS-paket med hjälp av en uppgift ”köra processen”.</span><span class="sxs-lookup"><span data-stu-id="afc8f-215">You can also incorporate AZCopy into your SSIS packages using an "Execute Process" task.</span></span>

<span data-ttu-id="afc8f-216">toouse AZCopy du först måste toodownload och installera den.</span><span class="sxs-lookup"><span data-stu-id="afc8f-216">toouse AZCopy you will first need toodownload and install it.</span></span> <span data-ttu-id="afc8f-217">Det finns en [produktionsversion] [ production version] och en [förhandsversionen] [ preview version] tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="afc8f-217">There is a [production version][production version] and a [preview version][preview version] available.</span></span>

<span data-ttu-id="afc8f-218">tooupload en fil från din dator behöver du ett kommando som hello en nedan:</span><span class="sxs-lookup"><span data-stu-id="afc8f-218">tooupload a file from your file system you will need a command like hello one below:</span></span>

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="afc8f-219">En sammanfattning av övergripande processen kan vara:</span><span class="sxs-lookup"><span data-stu-id="afc8f-219">A high-level process summary could be:</span></span>

1. <span data-ttu-id="afc8f-220">Konfigurera ett Azure storage blob-behållaren tooreceive hello-data</span><span class="sxs-lookup"><span data-stu-id="afc8f-220">Configure an Azure storage blob container tooreceive hello data</span></span>
2. <span data-ttu-id="afc8f-221">Exportera toolocal datalagringen</span><span class="sxs-lookup"><span data-stu-id="afc8f-221">Export your data toolocal storage</span></span>
3. <span data-ttu-id="afc8f-222">AZCopy dina data i hello Azure Blob Storage-behållare</span><span class="sxs-lookup"><span data-stu-id="afc8f-222">AZCopy your data in hello Azure Blob Storage container</span></span>
4. <span data-ttu-id="afc8f-223">Läs in hello data till SQL Data Warehouse med PolyBase</span><span class="sxs-lookup"><span data-stu-id="afc8f-223">Load hello data into SQL Data Warehouse using PolyBase</span></span>

<span data-ttu-id="afc8f-224">Fullständig dokumentation tillgänglig: [AZCopy][AZCopy].</span><span class="sxs-lookup"><span data-stu-id="afc8f-224">Full documentation available: [AZCopy][AZCopy].</span></span>

## <a name="optimizing-data-export"></a><span data-ttu-id="afc8f-225">Optimera export av data</span><span class="sxs-lookup"><span data-stu-id="afc8f-225">Optimizing data export</span></span>
<span data-ttu-id="afc8f-226">Dessutom kan tooensuring att hello export överensstämmer toohello kraven av PolyBase du också söka toooptimize hello export av hello tooimprove hello av data ytterligare.</span><span class="sxs-lookup"><span data-stu-id="afc8f-226">In addition tooensuring that hello export conforms toohello requirements laid out by PolyBase you can also seek toooptimize hello export of hello data tooimprove hello process further.</span></span>



### <a name="data-compression"></a><span data-ttu-id="afc8f-227">Datakomprimering</span><span class="sxs-lookup"><span data-stu-id="afc8f-227">Data compression</span></span>
<span data-ttu-id="afc8f-228">PolyBase kan läsa gzip komprimerade data.</span><span class="sxs-lookup"><span data-stu-id="afc8f-228">PolyBase can read gzip compressed data.</span></span> <span data-ttu-id="afc8f-229">Om du kan toocompress kommer toogzip datafiler du minimera hello mängden data som flyttas över hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="afc8f-229">If you are able toocompress your data toogzip files then you will minimize hello amount of data being pushed over hello network.</span></span>

### <a name="multiple-files"></a><span data-ttu-id="afc8f-230">Flera filer</span><span class="sxs-lookup"><span data-stu-id="afc8f-230">Multiple files</span></span>
<span data-ttu-id="afc8f-231">Dela upp stora tabeller i flera filer hjälper inte bara tooimprove exportera hastighet, hjälper också med överföra re-startability och hello övergripande hanterbarhet hello data en gång i hello Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="afc8f-231">Breaking up large tables into several files not only helps tooimprove export speed, it also helps with transfer re-startability, and hello overall manageability of hello data once in hello Azure blob storage.</span></span> <span data-ttu-id="afc8f-232">En av hello många bra funktioner i PolyBase är att den ska läsa alla hello-filer i en mapp och behandla det som en tabell.</span><span class="sxs-lookup"><span data-stu-id="afc8f-232">One of hello many nice features of PolyBase is that it will read all hello files inside a folder and treat it as one table.</span></span> <span data-ttu-id="afc8f-233">Därför är det en bra idé tooisolate hello filer för varje tabell till en egen mapp.</span><span class="sxs-lookup"><span data-stu-id="afc8f-233">It is therefore a good idea tooisolate hello files for each table into its own folder.</span></span>

<span data-ttu-id="afc8f-234">PolyBase stöder också en funktion som kallas ”rekursiv mappen traversal”.</span><span class="sxs-lookup"><span data-stu-id="afc8f-234">PolyBase also supports a feature known as "recursive folder traversal".</span></span> <span data-ttu-id="afc8f-235">Du kan använda den här funktionen toofurther förbättra hello organisationen av din exporterade data tooimprove din datahantering.</span><span class="sxs-lookup"><span data-stu-id="afc8f-235">You can use this feature toofurther enhance hello organization of your exported data tooimprove your data management.</span></span>

<span data-ttu-id="afc8f-236">toolearn mer information om hur du läser in data med PolyBase, se [Använd PolyBase tooload data till SQL Data Warehouse][Use PolyBase tooload data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="afc8f-236">toolearn more about loading data with PolyBase, see [Use PolyBase tooload data into SQL Data Warehouse][Use PolyBase tooload data into SQL Data Warehouse].</span></span>

## <a name="next-steps"></a><span data-ttu-id="afc8f-237">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="afc8f-237">Next steps</span></span>
<span data-ttu-id="afc8f-238">Mer information om migrering finns i [migrera din lösning tooSQL datalagret][Migrate your solution tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="afc8f-238">For more about migration, see [Migrate your solution tooSQL Data Warehouse][Migrate your solution tooSQL Data Warehouse].</span></span>
<span data-ttu-id="afc8f-239">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="afc8f-239">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[AZCopy]:../storage/common/storage-use-azcopy.md
[ADF Copy]: ../data-factory/data-factory-data-movement-activities.md 
[ADF samples]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[development overview]: sql-data-warehouse-overview-develop.md
[Migrate your solution tooSQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Use bcp tooload data into SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[Use PolyBase tooload data into SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute documentation]: http://azure.microsoft.com/documentation/services/expressroute/

[production version]: http://aka.ms/downloadazcopy/
[preview version]: http://aka.ms/downloadazcopypr/
[ADO.NET destination adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS documentation]: https://msdn.microsoft.com/library/ms141026.aspx
