---
title: aaaTransactions i SQL Data Warehouse | Microsoft Docs
description: "Tips för transaktioner i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: ae621788-e575-41f5-8bfe-fa04dc4b0b53
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 7c541648553238443b407666612561918096eb61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transactions-in-sql-data-warehouse"></a><span data-ttu-id="c18b5-103">Transaktioner i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c18b5-103">Transactions in SQL Data Warehouse</span></span>
<span data-ttu-id="c18b5-104">Som förväntat, stöder transaktioner som en del av hello arbetsbelastningen för informationslager i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c18b5-104">As you would expect, SQL Data Warehouse supports transactions as part of hello data warehouse workload.</span></span> <span data-ttu-id="c18b5-105">Dock underhålls tooensure hello prestanda för SQL Data Warehouse i större skala vissa funktioner är begränsade när jämfört med tooSQL Server.</span><span class="sxs-lookup"><span data-stu-id="c18b5-105">However, tooensure hello performance of SQL Data Warehouse is maintained at scale some features are limited when compared tooSQL Server.</span></span> <span data-ttu-id="c18b5-106">Den här artikeln visar hello skillnader och visar hello andra.</span><span class="sxs-lookup"><span data-stu-id="c18b5-106">This article highlights hello differences and lists hello others.</span></span> 

## <a name="transaction-isolation-levels"></a><span data-ttu-id="c18b5-107">Transaktionsisoleringsnivåer</span><span class="sxs-lookup"><span data-stu-id="c18b5-107">Transaction isolation levels</span></span>
<span data-ttu-id="c18b5-108">SQL Data Warehouse implementerar ACID-transaktioner.</span><span class="sxs-lookup"><span data-stu-id="c18b5-108">SQL Data Warehouse implements ACID transactions.</span></span> <span data-ttu-id="c18b5-109">Hello isolering av hello transaktionellt stöd är dock begränsad för`READ UNCOMMITTED` och detta kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="c18b5-109">However, hello Isolation of hello transactional support is limited too`READ UNCOMMITTED` and this cannot be changed.</span></span> <span data-ttu-id="c18b5-110">Du kan implementera ett antal kodning metoder tooprevent dirty läser data om det här är ett problem för dig.</span><span class="sxs-lookup"><span data-stu-id="c18b5-110">You can implement a number of coding methods tooprevent dirty reads of data if this is a concern for you.</span></span> <span data-ttu-id="c18b5-111">hello populäraste metoder utnyttja CTAS och tabellen partition växling (ofta kallas hello glidande fönstret mönster) tooprevent användare från datafrågor som förbereds fortfarande.</span><span class="sxs-lookup"><span data-stu-id="c18b5-111">hello most popular methods leverage both CTAS and table partition switching (often known as hello sliding window pattern) tooprevent users from querying data that is still being prepared.</span></span> <span data-ttu-id="c18b5-112">Vyer som före filtrera hello data är också en populär metod.</span><span class="sxs-lookup"><span data-stu-id="c18b5-112">Views that pre-filter hello data is also a popular approach.</span></span>  

## <a name="transaction-size"></a><span data-ttu-id="c18b5-113">Transaktionsstorlek</span><span class="sxs-lookup"><span data-stu-id="c18b5-113">Transaction size</span></span>
<span data-ttu-id="c18b5-114">En enda data ändras transaktion har begränsad storlek.</span><span class="sxs-lookup"><span data-stu-id="c18b5-114">A single data modification transaction is limited in size.</span></span> <span data-ttu-id="c18b5-115">hello gränsen tillämpas idag ”per distribution”.</span><span class="sxs-lookup"><span data-stu-id="c18b5-115">hello limit today is applied "per distribution".</span></span> <span data-ttu-id="c18b5-116">Därför kan hello totala fördelningen beräknas genom att multiplicera hello gränsen med hello distribution count.</span><span class="sxs-lookup"><span data-stu-id="c18b5-116">Therefore, hello total allocation can be calculated by multiplying hello limit by hello distribution count.</span></span> <span data-ttu-id="c18b5-117">tooapproximate hello maximalt antal rader i hello transaktion delar hello distribution cap med hello totala storleken på varje rad.</span><span class="sxs-lookup"><span data-stu-id="c18b5-117">tooapproximate hello maximum number of rows in hello transaction divide hello distribution cap by hello total size of each row.</span></span> <span data-ttu-id="c18b5-118">Överväg att tar en genomsnittlig kolumnlängden i stället hello maxstorleken för kolumner med variabel längd.</span><span class="sxs-lookup"><span data-stu-id="c18b5-118">For variable length columns consider taking an average column length rather than using hello maximum size.</span></span>

<span data-ttu-id="c18b5-119">Följande antaganden har gjorts i hello tabellen nedan hello:</span><span class="sxs-lookup"><span data-stu-id="c18b5-119">In hello table below hello following assumptions have been made:</span></span>

* <span data-ttu-id="c18b5-120">Det uppstod en jämn fördelning av data</span><span class="sxs-lookup"><span data-stu-id="c18b5-120">An even distribution of data has occurred</span></span> 
* <span data-ttu-id="c18b5-121">hello genomsnittlig radlängden är 250 byte</span><span class="sxs-lookup"><span data-stu-id="c18b5-121">hello average row length is 250 bytes</span></span>

| <span data-ttu-id="c18b5-122">[DWU][DWU]</span><span class="sxs-lookup"><span data-stu-id="c18b5-122">[DWU][DWU]</span></span> | <span data-ttu-id="c18b5-123">CAP per distribution (GiB)</span><span class="sxs-lookup"><span data-stu-id="c18b5-123">Cap per distribution (GiB)</span></span> | <span data-ttu-id="c18b5-124">Antal distributioner</span><span class="sxs-lookup"><span data-stu-id="c18b5-124">Number of Distributions</span></span> | <span data-ttu-id="c18b5-125">Maxstorlek för transaktionen (GiB)</span><span class="sxs-lookup"><span data-stu-id="c18b5-125">MAX transaction size (GiB)</span></span> | <span data-ttu-id="c18b5-126"># Rader per distribution</span><span class="sxs-lookup"><span data-stu-id="c18b5-126"># Rows per distribution</span></span> | <span data-ttu-id="c18b5-127">Maximalt antal rader per transaktion</span><span class="sxs-lookup"><span data-stu-id="c18b5-127">Max Rows per transaction</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="c18b5-128">DW100</span><span class="sxs-lookup"><span data-stu-id="c18b5-128">DW100</span></span> |<span data-ttu-id="c18b5-129">1</span><span class="sxs-lookup"><span data-stu-id="c18b5-129">1</span></span> |<span data-ttu-id="c18b5-130">60</span><span class="sxs-lookup"><span data-stu-id="c18b5-130">60</span></span> |<span data-ttu-id="c18b5-131">60</span><span class="sxs-lookup"><span data-stu-id="c18b5-131">60</span></span> |<span data-ttu-id="c18b5-132">4,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-132">4,000,000</span></span> |<span data-ttu-id="c18b5-133">240,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-133">240,000,000</span></span> |
| <span data-ttu-id="c18b5-134">DW200</span><span class="sxs-lookup"><span data-stu-id="c18b5-134">DW200</span></span> |<span data-ttu-id="c18b5-135">1.5</span><span class="sxs-lookup"><span data-stu-id="c18b5-135">1.5</span></span> |<span data-ttu-id="c18b5-136">60</span><span class="sxs-lookup"><span data-stu-id="c18b5-136">60</span></span> |<span data-ttu-id="c18b5-137">90</span><span class="sxs-lookup"><span data-stu-id="c18b5-137">90</span></span> |<span data-ttu-id="c18b5-138">6,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-138">6,000,000</span></span> |<span data-ttu-id="c18b5-139">360,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-139">360,000,000</span></span> |
| <span data-ttu-id="c18b5-140">DW300</span><span class="sxs-lookup"><span data-stu-id="c18b5-140">DW300</span></span> |<span data-ttu-id="c18b5-141">2.25</span><span class="sxs-lookup"><span data-stu-id="c18b5-141">2.25</span></span> |<span data-ttu-id="c18b5-142">60</span><span class="sxs-lookup"><span data-stu-id="c18b5-142">60</span></span> |<span data-ttu-id="c18b5-143">135</span><span class="sxs-lookup"><span data-stu-id="c18b5-143">135</span></span> |<span data-ttu-id="c18b5-144">9,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-144">9,000,000</span></span> |<span data-ttu-id="c18b5-145">540,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-145">540,000,000</span></span> |
| <span data-ttu-id="c18b5-146">DW400</span><span class="sxs-lookup"><span data-stu-id="c18b5-146">DW400</span></span> |<span data-ttu-id="c18b5-147">3</span><span class="sxs-lookup"><span data-stu-id="c18b5-147">3</span></span> |<span data-ttu-id="c18b5-148">60</span><span class="sxs-lookup"><span data-stu-id="c18b5-148">60</span></span> |<span data-ttu-id="c18b5-149">180</span><span class="sxs-lookup"><span data-stu-id="c18b5-149">180</span></span> |<span data-ttu-id="c18b5-150">12,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-150">12,000,000</span></span> |<span data-ttu-id="c18b5-151">720,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-151">720,000,000</span></span> |
| <span data-ttu-id="c18b5-152">DW500</span><span class="sxs-lookup"><span data-stu-id="c18b5-152">DW500</span></span> |<span data-ttu-id="c18b5-153">3.75</span><span class="sxs-lookup"><span data-stu-id="c18b5-153">3.75</span></span> |<span data-ttu-id="c18b5-154">60</span><span class="sxs-lookup"><span data-stu-id="c18b5-154">60</span></span> |<span data-ttu-id="c18b5-155">225</span><span class="sxs-lookup"><span data-stu-id="c18b5-155">225</span></span> |<span data-ttu-id="c18b5-156">15,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-156">15,000,000</span></span> |<span data-ttu-id="c18b5-157">900,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-157">900,000,000</span></span> |
| <span data-ttu-id="c18b5-158">DW600</span><span class="sxs-lookup"><span data-stu-id="c18b5-158">DW600</span></span> |<span data-ttu-id="c18b5-159">4.5</span><span class="sxs-lookup"><span data-stu-id="c18b5-159">4.5</span></span> |<span data-ttu-id="c18b5-160">60</span><span class="sxs-lookup"><span data-stu-id="c18b5-160">60</span></span> |<span data-ttu-id="c18b5-161">270</span><span class="sxs-lookup"><span data-stu-id="c18b5-161">270</span></span> |<span data-ttu-id="c18b5-162">18,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-162">18,000,000</span></span> |<span data-ttu-id="c18b5-163">1,080,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-163">1,080,000,000</span></span> |
| <span data-ttu-id="c18b5-164">DW1000</span><span class="sxs-lookup"><span data-stu-id="c18b5-164">DW1000</span></span> |<span data-ttu-id="c18b5-165">7.5</span><span class="sxs-lookup"><span data-stu-id="c18b5-165">7.5</span></span> |<span data-ttu-id="c18b5-166">60</span><span class="sxs-lookup"><span data-stu-id="c18b5-166">60</span></span> |<span data-ttu-id="c18b5-167">450</span><span class="sxs-lookup"><span data-stu-id="c18b5-167">450</span></span> |<span data-ttu-id="c18b5-168">30,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-168">30,000,000</span></span> |<span data-ttu-id="c18b5-169">1,800,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-169">1,800,000,000</span></span> |
| <span data-ttu-id="c18b5-170">DW1200</span><span class="sxs-lookup"><span data-stu-id="c18b5-170">DW1200</span></span> |<span data-ttu-id="c18b5-171">9</span><span class="sxs-lookup"><span data-stu-id="c18b5-171">9</span></span> |<span data-ttu-id="c18b5-172">60</span><span class="sxs-lookup"><span data-stu-id="c18b5-172">60</span></span> |<span data-ttu-id="c18b5-173">540</span><span class="sxs-lookup"><span data-stu-id="c18b5-173">540</span></span> |<span data-ttu-id="c18b5-174">36,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-174">36,000,000</span></span> |<span data-ttu-id="c18b5-175">2,160,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-175">2,160,000,000</span></span> |
| <span data-ttu-id="c18b5-176">DW1500</span><span class="sxs-lookup"><span data-stu-id="c18b5-176">DW1500</span></span> |<span data-ttu-id="c18b5-177">11.25</span><span class="sxs-lookup"><span data-stu-id="c18b5-177">11.25</span></span> |<span data-ttu-id="c18b5-178">60</span><span class="sxs-lookup"><span data-stu-id="c18b5-178">60</span></span> |<span data-ttu-id="c18b5-179">675</span><span class="sxs-lookup"><span data-stu-id="c18b5-179">675</span></span> |<span data-ttu-id="c18b5-180">45,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-180">45,000,000</span></span> |<span data-ttu-id="c18b5-181">2,700,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-181">2,700,000,000</span></span> |
| <span data-ttu-id="c18b5-182">DW2000</span><span class="sxs-lookup"><span data-stu-id="c18b5-182">DW2000</span></span> |<span data-ttu-id="c18b5-183">15</span><span class="sxs-lookup"><span data-stu-id="c18b5-183">15</span></span> |<span data-ttu-id="c18b5-184">60</span><span class="sxs-lookup"><span data-stu-id="c18b5-184">60</span></span> |<span data-ttu-id="c18b5-185">900</span><span class="sxs-lookup"><span data-stu-id="c18b5-185">900</span></span> |<span data-ttu-id="c18b5-186">60,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-186">60,000,000</span></span> |<span data-ttu-id="c18b5-187">3,600,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-187">3,600,000,000</span></span> |
| <span data-ttu-id="c18b5-188">DW3000</span><span class="sxs-lookup"><span data-stu-id="c18b5-188">DW3000</span></span> |<span data-ttu-id="c18b5-189">22.5</span><span class="sxs-lookup"><span data-stu-id="c18b5-189">22.5</span></span> |<span data-ttu-id="c18b5-190">60</span><span class="sxs-lookup"><span data-stu-id="c18b5-190">60</span></span> |<span data-ttu-id="c18b5-191">1,350</span><span class="sxs-lookup"><span data-stu-id="c18b5-191">1,350</span></span> |<span data-ttu-id="c18b5-192">90,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-192">90,000,000</span></span> |<span data-ttu-id="c18b5-193">5,400,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-193">5,400,000,000</span></span> |
| <span data-ttu-id="c18b5-194">DW6000</span><span class="sxs-lookup"><span data-stu-id="c18b5-194">DW6000</span></span> |<span data-ttu-id="c18b5-195">45</span><span class="sxs-lookup"><span data-stu-id="c18b5-195">45</span></span> |<span data-ttu-id="c18b5-196">60</span><span class="sxs-lookup"><span data-stu-id="c18b5-196">60</span></span> |<span data-ttu-id="c18b5-197">2,700</span><span class="sxs-lookup"><span data-stu-id="c18b5-197">2,700</span></span> |<span data-ttu-id="c18b5-198">180,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-198">180,000,000</span></span> |<span data-ttu-id="c18b5-199">10,800,000,000</span><span class="sxs-lookup"><span data-stu-id="c18b5-199">10,800,000,000</span></span> |

<span data-ttu-id="c18b5-200">storleksgräns för hello transaktion tillämpas per transaktion eller åtgärden.</span><span class="sxs-lookup"><span data-stu-id="c18b5-200">hello transaction size limit is applied per transaction or operation.</span></span> <span data-ttu-id="c18b5-201">Det har inte tillämpats över alla samtidiga transaktioner.</span><span class="sxs-lookup"><span data-stu-id="c18b5-201">It is not applied across all concurrent transactions.</span></span> <span data-ttu-id="c18b5-202">Varje transaktion är därför tillåts toowrite den här mängden data toohello loggen.</span><span class="sxs-lookup"><span data-stu-id="c18b5-202">Therefore each transaction is permitted toowrite this amount of data toohello log.</span></span> 

<span data-ttu-id="c18b5-203">toooptimize och minimera hello mängden data som skrivs toohello loggen finns toohello [transaktioner metodtips] [ Transactions best practices] artikel.</span><span class="sxs-lookup"><span data-stu-id="c18b5-203">toooptimize and minimize hello amount of data written toohello log please refer toohello [Transactions best practices][Transactions best practices] article.</span></span>

> [!WARNING]
> <span data-ttu-id="c18b5-204">hello maximala transaktionsstorlek kan bara genomföras för HASH eller ROUND_ROBIN distribuerade tabeller där hello sprida sig på hello data är jämnt.</span><span class="sxs-lookup"><span data-stu-id="c18b5-204">hello maximum transaction size can only be achieved for HASH or ROUND_ROBIN distributed tables where hello spread of hello data is even.</span></span> <span data-ttu-id="c18b5-205">Om hello transaktion skriver data i ett skeva sätt toohello distributioner är hello gränsen sannolikt toobe nått tidigare toohello transaktion maximala storlek.</span><span class="sxs-lookup"><span data-stu-id="c18b5-205">If hello transaction is writing data in a skewed fashion toohello distributions then hello limit is likely toobe reached prior toohello maximum transaction size.</span></span>
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a><span data-ttu-id="c18b5-206">Transaktionstillstånd</span><span class="sxs-lookup"><span data-stu-id="c18b5-206">Transaction state</span></span>
<span data-ttu-id="c18b5-207">SQL Data Warehouse använder hello XACT_STATE() funktionen tooreport en misslyckad transaktion med värdet hello -2.</span><span class="sxs-lookup"><span data-stu-id="c18b5-207">SQL Data Warehouse uses hello XACT_STATE() function tooreport a failed transaction using hello value -2.</span></span> <span data-ttu-id="c18b5-208">Detta innebär att hello transaktionen misslyckades och har markerats för återställning endast</span><span class="sxs-lookup"><span data-stu-id="c18b5-208">This means that hello transaction has failed and is marked for rollback only</span></span>

> [!NOTE]
> <span data-ttu-id="c18b5-209">hello använda 2 genom hello XACT_STATE funktionen toodenote en misslyckade transaktionen representerar olika beteenden tooSQL Server.</span><span class="sxs-lookup"><span data-stu-id="c18b5-209">hello use of -2 by hello XACT_STATE function toodenote a failed transaction represents different behavior tooSQL Server.</span></span> <span data-ttu-id="c18b5-210">SQL Server använder hello värdet -1 toorepresent en som transaktion.</span><span class="sxs-lookup"><span data-stu-id="c18b5-210">SQL Server uses hello value -1 toorepresent an un-committable transaction.</span></span> <span data-ttu-id="c18b5-211">SQL Server kan tolerera fel inuti en transaktion utan att den har markerats som som toobe.</span><span class="sxs-lookup"><span data-stu-id="c18b5-211">SQL Server can tolerate some errors inside a transaction without it having toobe marked as un-committable.</span></span> <span data-ttu-id="c18b5-212">Till exempel `SELECT 1/0` skulle orsaka fel men inte att tvinga fram en transaktion till ett som tillstånd.</span><span class="sxs-lookup"><span data-stu-id="c18b5-212">For example `SELECT 1/0` would cause an error but not force a transaction into an un-committable state.</span></span> <span data-ttu-id="c18b5-213">SQL Server tillåter också läsningar i hello som transaktion.</span><span class="sxs-lookup"><span data-stu-id="c18b5-213">SQL Server also permits reads in hello un-committable transaction.</span></span> <span data-ttu-id="c18b5-214">Dock kan SQL Data Warehouse du inte göra detta.</span><span class="sxs-lookup"><span data-stu-id="c18b5-214">However, SQL Data Warehouse does not let you do this.</span></span> <span data-ttu-id="c18b5-215">Om ett fel uppstår i en transaktion i SQL Data Warehouse hello -2 tillstånd anges automatiskt och du kommer inte att kunna toomake Välj någon ytterligare instruktioner förrän hello-instruktionen har återställts.</span><span class="sxs-lookup"><span data-stu-id="c18b5-215">If an error occurs inside a SQL Data Warehouse transaction it will automatically enter hello -2 state and you will not be able toomake any further select statements until hello statement has been rolled back.</span></span> <span data-ttu-id="c18b5-216">Det är därför viktigt toocheck att ditt program kod toosee om XACT_STATE() används som du kanske måste toomake kod ändringar.</span><span class="sxs-lookup"><span data-stu-id="c18b5-216">It is therefore important toocheck that your application code toosee if it uses  XACT_STATE() as you may need toomake code modifications.</span></span>
> 
> 

<span data-ttu-id="c18b5-217">Till exempel i SQL Server kan du se en transaktion som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="c18b5-217">For example, in SQL Server you might see a transaction that looks like this:</span></span>

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

<span data-ttu-id="c18b5-218">Om du lämnar din kod som ovan kommer du få hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="c18b5-218">If you leave your code as it is above then you will get hello following error message:</span></span>

<span data-ttu-id="c18b5-219">Meddelande 111233, nivå 16, tillstånd 1, rad 1 111233; hello aktuella transaktionen har avbrutits och eventuella väntande ändringar har återställts.</span><span class="sxs-lookup"><span data-stu-id="c18b5-219">Msg 111233, Level 16, State 1, Line 1 111233;hello current transaction has aborted, and any pending changes have been rolled back.</span></span> <span data-ttu-id="c18b5-220">Orsak: En transaktion endast återställning har inte uttryckligen återställts innan DDL-, DML- eller SELECT-instruktion.</span><span class="sxs-lookup"><span data-stu-id="c18b5-220">Cause: A transaction in a rollback-only state was not explicitly rolled back before a DDL, DML or SELECT statement.</span></span>

<span data-ttu-id="c18b5-221">Du får också inte hello utdata från hello ERROR_ * funktioner.</span><span class="sxs-lookup"><span data-stu-id="c18b5-221">You will also not get hello output of hello ERROR_* functions.</span></span>

<span data-ttu-id="c18b5-222">I SQL Data Warehouse måste hello toobe något ändras:</span><span class="sxs-lookup"><span data-stu-id="c18b5-222">In SQL Data Warehouse hello code needs toobe slightly altered:</span></span>

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

<span data-ttu-id="c18b5-223">hello förväntat beteende observeras nu.</span><span class="sxs-lookup"><span data-stu-id="c18b5-223">hello expected behavior is now observed.</span></span> <span data-ttu-id="c18b5-224">hello-fel i hello transaktion hanteras och hello ERROR_ * funktioner ange värden som förväntat.</span><span class="sxs-lookup"><span data-stu-id="c18b5-224">hello error in hello transaction is managed and hello ERROR_* functions provide values as expected.</span></span>

<span data-ttu-id="c18b5-225">Alla som har ändrats är den hello `ROLLBACK` av hello transaktionen hade toohappen innan hello läser av hello felinformation i hello `CATCH` block.</span><span class="sxs-lookup"><span data-stu-id="c18b5-225">All that has changed is that hello `ROLLBACK` of hello transaction had toohappen before hello read of hello error information in hello `CATCH` block.</span></span>

## <a name="errorline-function"></a><span data-ttu-id="c18b5-226">Error_Line() funktion</span><span class="sxs-lookup"><span data-stu-id="c18b5-226">Error_Line() function</span></span>
<span data-ttu-id="c18b5-227">Det är också värt att nämna att SQL Data Warehouse inte implementera eller stöder hello ERROR_LINE() funktion.</span><span class="sxs-lookup"><span data-stu-id="c18b5-227">It is also worth noting that SQL Data Warehouse does not implement or support hello ERROR_LINE() function.</span></span> <span data-ttu-id="c18b5-228">Om du har detta i din kod måste tooremove den toobe som är kompatibla med SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c18b5-228">If you have this in your code you will need tooremove it toobe compliant with SQL Data Warehouse.</span></span> <span data-ttu-id="c18b5-229">Använd fråga etiketter i koden i stället tooimplement motsvarande funktioner.</span><span class="sxs-lookup"><span data-stu-id="c18b5-229">Use query labels in your code instead tooimplement equivalent functionality.</span></span> <span data-ttu-id="c18b5-230">Se toohello [etikett] [ LABEL] mer information om den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c18b5-230">Please refer toohello [LABEL][LABEL] article for more details on this feature.</span></span>

## <a name="using-throw-and-raiserror"></a><span data-ttu-id="c18b5-231">Med hjälp av THROW och RAISERROR</span><span class="sxs-lookup"><span data-stu-id="c18b5-231">Using THROW and RAISERROR</span></span>
<span data-ttu-id="c18b5-232">THROW är hello mer modern implementering för att öka undantag i SQL Data Warehouse men stöds även RAISERROR.</span><span class="sxs-lookup"><span data-stu-id="c18b5-232">THROW is hello more modern implementation for raising exceptions in SQL Data Warehouse but RAISERROR is also supported.</span></span> <span data-ttu-id="c18b5-233">Det finns några skillnader som är värt uppmärksamhet toohowever.</span><span class="sxs-lookup"><span data-stu-id="c18b5-233">There are a few differences that are worth paying attention toohowever.</span></span>

* <span data-ttu-id="c18b5-234">Användardefinierade felmeddelanden siffror inte får finnas i hello 150 100 000 000 intervallet för THROW</span><span class="sxs-lookup"><span data-stu-id="c18b5-234">User defined error messages numbers cannot be in hello 100,000 - 150,000 range for THROW</span></span>
* <span data-ttu-id="c18b5-235">RAISERROR felmeddelanden korrigeras på 50 000</span><span class="sxs-lookup"><span data-stu-id="c18b5-235">RAISERROR error messages are fixed at 50,000</span></span>
* <span data-ttu-id="c18b5-236">Användning av sys.messages stöds inte</span><span class="sxs-lookup"><span data-stu-id="c18b5-236">Use of sys.messages is not supported</span></span>

## <a name="limitiations"></a><span data-ttu-id="c18b5-237">Limitiations</span><span class="sxs-lookup"><span data-stu-id="c18b5-237">Limitiations</span></span>
<span data-ttu-id="c18b5-238">SQL Data Warehouse har några andra begränsningar som handlar om tootransactions.</span><span class="sxs-lookup"><span data-stu-id="c18b5-238">SQL Data Warehouse does have a few other restrictions that relate tootransactions.</span></span>

<span data-ttu-id="c18b5-239">De är följande:</span><span class="sxs-lookup"><span data-stu-id="c18b5-239">They are as follows:</span></span>

* <span data-ttu-id="c18b5-240">Inga distribuerade transaktioner</span><span class="sxs-lookup"><span data-stu-id="c18b5-240">No distributed transactions</span></span>
* <span data-ttu-id="c18b5-241">Inga kapslade transaktioner tillåtna</span><span class="sxs-lookup"><span data-stu-id="c18b5-241">No nested transactions permitted</span></span>
* <span data-ttu-id="c18b5-242">Spara punkter tillåts inte</span><span class="sxs-lookup"><span data-stu-id="c18b5-242">No save points allowed</span></span>
* <span data-ttu-id="c18b5-243">Inga namngivna transaktioner</span><span class="sxs-lookup"><span data-stu-id="c18b5-243">No named transactions</span></span>
* <span data-ttu-id="c18b5-244">Inga markerade transaktioner</span><span class="sxs-lookup"><span data-stu-id="c18b5-244">No marked transactions</span></span>
* <span data-ttu-id="c18b5-245">Inget stöd för DDL som `CREATE TABLE` definieras inuti en transaktion</span><span class="sxs-lookup"><span data-stu-id="c18b5-245">No support for DDL such as `CREATE TABLE` inside a user defined transaction</span></span>

## <a name="next-steps"></a><span data-ttu-id="c18b5-246">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c18b5-246">Next steps</span></span>
<span data-ttu-id="c18b5-247">toolearn mer information om hur du optimerar transaktioner, se [transaktioner metodtips][Transactions best practices].</span><span class="sxs-lookup"><span data-stu-id="c18b5-247">toolearn more about optimizing transactions, see [Transactions best practices][Transactions best practices].</span></span>  <span data-ttu-id="c18b5-248">toolearn om andra Metodtips för SQL Data Warehouse, se [SQL Data Warehouse metodtips][SQL Data Warehouse best practices].</span><span class="sxs-lookup"><span data-stu-id="c18b5-248">toolearn about other SQL Data Warehouse best practices, see [SQL Data Warehouse best practices][SQL Data Warehouse best practices].</span></span>

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Transactions best practices]: ./sql-data-warehouse-develop-best-practices-transactions.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
