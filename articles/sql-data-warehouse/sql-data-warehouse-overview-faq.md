---
title: "aaaAzure SQL Data Warehouse vanliga frågor och svar | Microsoft Docs"
description: "Den här artikeln innehåller ut vanliga frågor och svar om Azure SQL Data Warehouse från kunder och utvecklare"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: 812CA525-3BF3-49DF-8DF3-FB4342464F4F
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 3/1/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: 09fd3f65d9507b09fcb8f477742c7d020add2755
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-frequently-asked-questions"></a><span data-ttu-id="4c8fd-103">SQL Data Warehouse vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="4c8fd-103">SQL Data Warehouse Frequently asked questions</span></span>

## <a name="general"></a><span data-ttu-id="4c8fd-104">Allmänt</span><span class="sxs-lookup"><span data-stu-id="4c8fd-104">General</span></span>

<span data-ttu-id="4c8fd-105">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-105">Q.</span></span> <span data-ttu-id="4c8fd-106">Vad erbjuder SQL DW för datasäkerhet?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-106">What does SQL DW offer for data security?</span></span>

<span data-ttu-id="4c8fd-107">A.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-107">A.</span></span> <span data-ttu-id="4c8fd-108">SQL DW erbjuder flera lösningar för att skydda data, till exempel TDE och granskning.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-108">SQL DW offers several solutions for protecting data such as TDE and auditing.</span></span> <span data-ttu-id="4c8fd-109">Mer information finns i [säkerhet].</span><span class="sxs-lookup"><span data-stu-id="4c8fd-109">For more information, see [Security].</span></span>

<span data-ttu-id="4c8fd-110">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-110">Q.</span></span> <span data-ttu-id="4c8fd-111">Var hittar jag reda på vilka juridiska eller affärsmässiga standarder SQL DW som är kompatibla med?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-111">Where can I find out what legal or business standards is SQL DW compliant with?</span></span>

<span data-ttu-id="4c8fd-112">A.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-112">A.</span></span> <span data-ttu-id="4c8fd-113">Besök hello [Microsoft Compliance] sidan för olika erbjudanden för efterlevnad av produkten som SOC och ISO.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-113">Visit hello [Microsoft Compliance] page for various compliance offerings by product such as SOC and ISO.</span></span> <span data-ttu-id="4c8fd-114">Först välja genom efterlevnad titel och sedan expandera Azure under hello Microsoft i omfånget cloud services hello höger i hello sidan toosee vilka tjänster som är Azure-tjänster är kompatibla.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-114">First choose by Compliance title, then expand Azure in hello Microsoft in-scope cloud services section on hello right side of hello page toosee what services are Azure services are compliant.</span></span>

<span data-ttu-id="4c8fd-115">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-115">Q.</span></span> <span data-ttu-id="4c8fd-116">Kan jag ansluta PowerBI?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-116">Can I connect PowerBI?</span></span>

<span data-ttu-id="4c8fd-117">A.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-117">A.</span></span> <span data-ttu-id="4c8fd-118">Visst!</span><span class="sxs-lookup"><span data-stu-id="4c8fd-118">Yes!</span></span> <span data-ttu-id="4c8fd-119">Om PowerBI stöder direkt fråga med SQL DW, är den inte avsedd för stort antal användare eller realtidsdata.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-119">Though PowerBI supports direct query with SQL DW, it’s not intended for large number of users or real-time data.</span></span> <span data-ttu-id="4c8fd-120">För produktion av PowerBI bör du använda PowerBI utöver Azure Analysis Services och Analysis Service IaaS.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-120">For production use of PowerBI, we recommend using PowerBI on top of Azure Analysis Services or Analysis Service IaaS.</span></span> 

<span data-ttu-id="4c8fd-121">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-121">Q.</span></span> <span data-ttu-id="4c8fd-122">Vad är SQL Data Warehouse kapacitetsbegränsningar?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-122">What are SQL Data Warehouse Capacity Limits?</span></span>

<span data-ttu-id="4c8fd-123">A.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-123">A.</span></span> <span data-ttu-id="4c8fd-124">Se vår aktuella [kapacitetsbegränsningar] sidan.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-124">See our current [capacity limits] page.</span></span> 

<span data-ttu-id="4c8fd-125">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-125">Q.</span></span> <span data-ttu-id="4c8fd-126">Varför är min skala / / pausa tar så lång tid?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-126">Why is my Scale/Pause/Resume taking so long?</span></span>

<span data-ttu-id="4c8fd-127">A.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-127">A.</span></span> <span data-ttu-id="4c8fd-128">En mängd olika faktorer kan påverka hello tid för beräkning hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-128">A variety of factors can influence hello time for compute management operations.</span></span> <span data-ttu-id="4c8fd-129">En gemensam fall för långvariga åtgärder är transaktionell återställning.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-129">A common case for  long running operations is transactional rollback.</span></span> <span data-ttu-id="4c8fd-130">När en åtgärd för skala eller pausa initieras, blockeras alla inkommande sessioner och frågor är tar slut.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-130">When a scale or pause operation is initiated, all incoming sessions are blocked and queries are drained.</span></span> <span data-ttu-id="4c8fd-131">I ordning tooleave hello system i ett stabilt tillstånd föras transaktioner tillbaka innan en åtgärd kan påbörjas.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-131">In order tooleave hello system in a stable state, transactions must be rolled back before an operation can commence.</span></span> <span data-ttu-id="4c8fd-132">Hej fler hello och större hello loggfilsstorlek av transaktioner, kommer stoppats hello längre hello åtgärden återställer hello system tooa stabilt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-132">hello greater hello number and larger hello log size of transactions, hello longer hello operation will be stalled restoring hello system tooa stable state.</span></span>

## <a name="user-support"></a><span data-ttu-id="4c8fd-133">Stöd för användare</span><span class="sxs-lookup"><span data-stu-id="4c8fd-133">User support</span></span>

<span data-ttu-id="4c8fd-134">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-134">Q.</span></span> <span data-ttu-id="4c8fd-135">Jag har en i funktionsbegäran var skickar jag det?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-135">I have a feature request, where do I submit it?</span></span>

<span data-ttu-id="4c8fd-136">A.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-136">A.</span></span> <span data-ttu-id="4c8fd-137">Om du har en funktionsbegäran kan du skicka den på vår [UserVoice] sidan</span><span class="sxs-lookup"><span data-stu-id="4c8fd-137">If you have a feature request, submit it on our [UserVoice] page</span></span>

<span data-ttu-id="4c8fd-138">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-138">Q.</span></span> <span data-ttu-id="4c8fd-139">Hur gör jag x?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-139">How can I do x?</span></span>

<span data-ttu-id="4c8fd-140">A.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-140">A.</span></span> <span data-ttu-id="4c8fd-141">Hjälp med att utveckla med SQL Data Warehouse, du kan ställa frågor på vår [Stack Overflow] sidan.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-141">For help in developing with SQL Data Warehouse, you can ask questions on our [Stack Overflow] page.</span></span> 

<span data-ttu-id="4c8fd-142">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-142">Q.</span></span> <span data-ttu-id="4c8fd-143">Hur skickar jag ett supportärende</span><span class="sxs-lookup"><span data-stu-id="4c8fd-143">How do I submit a support ticket?</span></span>

<span data-ttu-id="4c8fd-144">A.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-144">A.</span></span> <span data-ttu-id="4c8fd-145">[Supportärenden] kan registreras via Azure portal.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-145">[Support Tickets] can be filed through Azure portal.</span></span>

## <a name="sql-languagefeature-support"></a><span data-ttu-id="4c8fd-146">Stöd för SQL-språk-funktionen</span><span class="sxs-lookup"><span data-stu-id="4c8fd-146">SQL language/feature support</span></span> 

<span data-ttu-id="4c8fd-147">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-147">Q.</span></span> <span data-ttu-id="4c8fd-148">Vilka datatyper stöder SQL Data Warehouse?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-148">What datatypes does SQL Data Warehouse support?</span></span>

<span data-ttu-id="4c8fd-149">A.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-149">A.</span></span> <span data-ttu-id="4c8fd-150">Se SQL Data Warehouse [datatyper].</span><span class="sxs-lookup"><span data-stu-id="4c8fd-150">See SQL Data Warehouse [data types].</span></span>

<span data-ttu-id="4c8fd-151">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-151">Q.</span></span> <span data-ttu-id="4c8fd-152">Vilka table-funktioner stöder?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-152">What table features do you support?</span></span>

<span data-ttu-id="4c8fd-153">A.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-153">A.</span></span> <span data-ttu-id="4c8fd-154">Medan SQL Data Warehouse stöder många funktioner, vissa stöds inte och dokumenteras i [funktioner som inte stöds tabellen].</span><span class="sxs-lookup"><span data-stu-id="4c8fd-154">While SQL Data Warehouse supports many features, some are not supported and are documented in [Unsupported Table Features].</span></span>

## <a name="tooling-and-administration"></a><span data-ttu-id="4c8fd-155">Verktygsuppsättning och administration</span><span class="sxs-lookup"><span data-stu-id="4c8fd-155">Tooling and administration</span></span>

<span data-ttu-id="4c8fd-156">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-156">Q.</span></span> <span data-ttu-id="4c8fd-157">Stöder du databasprojekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-157">Do you support Database projects in Visual Studio.</span></span>

<span data-ttu-id="4c8fd-158">A.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-158">A.</span></span> <span data-ttu-id="4c8fd-159">Vi stöder för närvarande inte databasprojekt i Visual Studio för SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-159">We currently do not support Database projects in Visual Studio for SQL Data Warehouse.</span></span> <span data-ttu-id="4c8fd-160">Om du vill att toocast en omröstning tooget denna funktion, finns våra User Voice [Database projects funktion begäran].</span><span class="sxs-lookup"><span data-stu-id="4c8fd-160">If you'd like toocast a vote tooget this feature, visit our User Voice [Database projects feature request].</span></span>

<span data-ttu-id="4c8fd-161">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-161">Q.</span></span> <span data-ttu-id="4c8fd-162">Stöder SQL Data Warehouse REST API: er?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-162">Does SQL Data Warehouse support REST APIs?</span></span>

<span data-ttu-id="4c8fd-163">A.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-163">A.</span></span> <span data-ttu-id="4c8fd-164">Ja.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-164">Yes.</span></span> <span data-ttu-id="4c8fd-165">De flesta REST-funktioner som kan användas med SQL-databas är också tillgänglig i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-165">Most REST functionality that can be used with SQL Database is also available with SQL Data Warehouse.</span></span> <span data-ttu-id="4c8fd-166">Du API information i övriga sidor med dokumentation eller [MSDN].</span><span class="sxs-lookup"><span data-stu-id="4c8fd-166">You can find API information within REST documentation pages or [MSDN].</span></span>


## <a name="loading"></a><span data-ttu-id="4c8fd-167">Läser in</span><span class="sxs-lookup"><span data-stu-id="4c8fd-167">Loading</span></span>

<span data-ttu-id="4c8fd-168">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-168">Q.</span></span> <span data-ttu-id="4c8fd-169">Vilka klientdrivrutiner som stöder?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-169">What client drivers do you support?</span></span>

<span data-ttu-id="4c8fd-170">A.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-170">A.</span></span> <span data-ttu-id="4c8fd-171">Stöd för drivrutiner för DW kan hittas på hello [anslutningssträngar] sidan</span><span class="sxs-lookup"><span data-stu-id="4c8fd-171">Driver support for DW can be found on hello [Connection Strings] page</span></span>

<span data-ttu-id="4c8fd-172">F: vad filformat som stöds av PolyBase med SQL Data Warehouse?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-172">Q: What file formats are supported by PolyBase with SQL Data Warehouse?</span></span>

<span data-ttu-id="4c8fd-173">S: Orc, RC, parkettgolv och flat avgränsad text</span><span class="sxs-lookup"><span data-stu-id="4c8fd-173">A: Orc, RC, Parquet, and flat delimited text</span></span>

<span data-ttu-id="4c8fd-174">F: Vad kan jag ansluta toofrom SQL DW med PolyBase?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-174">Q: What can I connect toofrom SQL DW using PolyBase?</span></span> 

<span data-ttu-id="4c8fd-175">S: [Azure Data Lake Store] och [Azure Storage-Blobbar]</span><span class="sxs-lookup"><span data-stu-id="4c8fd-175">A: [Azure Data Lake Store] and [Azure Storage Blobs]</span></span>

<span data-ttu-id="4c8fd-176">F: går beräkning pushdown när du ansluter tooAzure Storage-Blobbar eller ADLS?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-176">Q: Is computation pushdown possible  when connecting tooAzure Storage Blobs or ADLS?</span></span> 

<span data-ttu-id="4c8fd-177">A: SQL DW PolyBase interagerar Nej, endast hello lagringskomponenter.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-177">A: No, SQL DW PolyBase only interacts hello storage components.</span></span> 

<span data-ttu-id="4c8fd-178">F: kan jag ansluta tooHDI?</span><span class="sxs-lookup"><span data-stu-id="4c8fd-178">Q: Can I connect tooHDI?</span></span>

<span data-ttu-id="4c8fd-179">S: HDI kan använda ADLS eller WASB som hello HDFS lager.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-179">A: HDI can use either ADLS or WASB as hello HDFS layer.</span></span> <span data-ttu-id="4c8fd-180">Om du har antingen som HDFS-lager kan du läsa in data till SQL DW.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-180">If you have either as your HDFS layer, then you can load that data into SQL DW.</span></span> <span data-ttu-id="4c8fd-181">Men kan inte du generera pushdown beräkning toohello HDI-instans.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-181">However, you cannot generate pushdown computation toohello HDI instance.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="4c8fd-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4c8fd-182">Next steps</span></span>
<span data-ttu-id="4c8fd-183">Mer information om SQL Data Warehouse som helhet finns i vår [översikt] sidan.</span><span class="sxs-lookup"><span data-stu-id="4c8fd-183">For more information on SQL Data Warehouse as a whole, see our [Overview] page.</span></span>


<!-- Article references -->
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[anslutningssträngar]: ./sql-data-warehouse-connection-strings.md
[Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Supportärenden]: ./sql-data-warehouse-get-started-create-support-ticket.md
[säkerhet]: ./sql-data-warehouse-overview-manage-security.md
[Microsoft Compliance]: https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings
[kapacitetsbegränsningar]: ./sql-data-warehouse-service-capacity-limits.md
[datatyper]: ./sql-data-warehouse-tables-data-types.md
[funktioner som inte stöds tabellen]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Azure Data Lake Store]: ./sql-data-warehouse-load-from-azure-data-lake-store.md
[Azure Storage-Blobbar]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Database projects funktion begäran]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
[MSDN]: https://msdn.microsoft.com/en-us/library/azure/mt163685.aspx
[översikt]: ./sql-data-warehouse-overview-faq.md