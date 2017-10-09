---
title: aaaManage databaser i Azure SQL Data Warehouse | Microsoft Docs
description: "Översikt över hantering av SQL Data Warehouse-databaser. Innehåller hanteringsverktyg, dwu: er och skalbar prestanda, felsökning frågeprestanda, etablera bra säkerhetsprinciper och återställa en databas från skadade data eller från ett regionalt strömavbrott."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 8576fdb3-71fe-4b3b-a4e0-5e8a7f148acf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: caec6572c4ab395107c3b095adc69a53eae8bd88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-databases-in-azure-sql-data-warehouse"></a><span data-ttu-id="01d3d-104">Hantera databaser i Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="01d3d-104">Manage databases in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="01d3d-105">SQL Data Warehouse automatiserar många aspekter av att hantera dina databaser.</span><span class="sxs-lookup"><span data-stu-id="01d3d-105">SQL Data Warehouse automates many aspects of managing your databases.</span></span> <span data-ttu-id="01d3d-106">Till exempel göra tooscale prestanda du behöver bara tooadjust betala för hello rätt nivå av beräkningsresurser och därefter låta SQL Data Warehouse alla hello arbetet med skala ut och skalning tillbaka.</span><span class="sxs-lookup"><span data-stu-id="01d3d-106">For example, tooscale performance you only need tooadjust and pay for hello right level of compute resources, and then let SQL Data Warehouse do all hello work of scaling out and scaling back.</span></span>

<span data-ttu-id="01d3d-107">Otvivelaktigt vill toomonitor din arbetsbelastning tooidentify prestandan måste samt felsöka tidskrävande frågor.</span><span class="sxs-lookup"><span data-stu-id="01d3d-107">You will undoubtedly want toomonitor your workload tooidentify your performance needs as well as troubleshoot long-running queries.</span></span> <span data-ttu-id="01d3d-108">Du måste också tooperform några uppgifter toomanage behörigheter för användare och inloggningar.</span><span class="sxs-lookup"><span data-stu-id="01d3d-108">You will also need tooperform a few security tasks toomanage permissions for users and logins.</span></span>

<span data-ttu-id="01d3d-109">Den här översikten beskrivs dessa aspekter av att hantera SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="01d3d-109">This overview covers these aspects of managing SQL Data Warehouse.</span></span>

* <span data-ttu-id="01d3d-110">Hanteringsverktyg</span><span class="sxs-lookup"><span data-stu-id="01d3d-110">Management tools</span></span>
* <span data-ttu-id="01d3d-111">Skala bearbetning</span><span class="sxs-lookup"><span data-stu-id="01d3d-111">Scale Compute</span></span>
* <span data-ttu-id="01d3d-112">Pausa och återuppta</span><span class="sxs-lookup"><span data-stu-id="01d3d-112">Pause and Resume</span></span>
* <span data-ttu-id="01d3d-113">Bästa praxis för prestanda</span><span class="sxs-lookup"><span data-stu-id="01d3d-113">Performance Best Practices</span></span>
* <span data-ttu-id="01d3d-114">Övervakning av frågan</span><span class="sxs-lookup"><span data-stu-id="01d3d-114">Query Monitoring</span></span>
* <span data-ttu-id="01d3d-115">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="01d3d-115">Security</span></span>
* <span data-ttu-id="01d3d-116">Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="01d3d-116">Backup and restore</span></span>

## <a name="management-tools"></a><span data-ttu-id="01d3d-117">Hanteringsverktyg</span><span class="sxs-lookup"><span data-stu-id="01d3d-117">Management tools</span></span>
<span data-ttu-id="01d3d-118">Du kan använda flera olika verktyg toomanage databaser i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="01d3d-118">You can use a variety of tools toomanage databases in SQL Data Warehouse.</span></span> <span data-ttu-id="01d3d-119">När du hanterar databaser du utvecklar inställningar för varje typ av uppgift som du behöver tooperform.</span><span class="sxs-lookup"><span data-stu-id="01d3d-119">As you manage databases, you will develop tool preferences for each type of task you need tooperform.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="01d3d-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="01d3d-120">Azure portal</span></span>
<span data-ttu-id="01d3d-121">Hej [Azure-portalen] [ Azure portal] är en webbaserad portal där du kan skapa, uppdatera, och ta bort databaser och övervaka databasresurser.</span><span class="sxs-lookup"><span data-stu-id="01d3d-121">hello [Azure portal][Azure portal] is a web-based portal where you can create, update, and delete databases and monitor database resources.</span></span> <span data-ttu-id="01d3d-122">Det här verktyget är bra om du precis har börjat med Azure, hantera ett litet antal datalagerdatabaserna eller behöver tooquickly göra något.</span><span class="sxs-lookup"><span data-stu-id="01d3d-122">This tool is great is if you're just getting started with Azure, managing a small number of data warehouse databases, or need tooquickly do something.</span></span>

<span data-ttu-id="01d3d-123">tooget igång med hello Azure-portalen finns [skapa ett SQL Data Warehouse (Azure portal)][Create a SQL Data Warehouse (Azure portal)].</span><span class="sxs-lookup"><span data-stu-id="01d3d-123">tooget started with hello Azure portal, see [Create a SQL Data Warehouse (Azure portal)][Create a SQL Data Warehouse (Azure portal)].</span></span>

### <a name="sql-server-data-tools-in-visual-studio"></a><span data-ttu-id="01d3d-124">SQL Server Data Tools i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01d3d-124">SQL Server Data Tools in Visual Studio</span></span>
<span data-ttu-id="01d3d-125">[SQL Server Data Tools] [ SQL Server Data Tools] (SSDT) i Visual Studio kan du tooconnect att hantera och utveckla dina databaser.</span><span class="sxs-lookup"><span data-stu-id="01d3d-125">[SQL Server Data Tools][SQL Server Data Tools] (SSDT) in Visual Studio allows you tooconnect to, manage, and develop your databases.</span></span> <span data-ttu-id="01d3d-126">Om du är programutvecklare som är bekant med Visual Studio eller andra integrerad utvecklingsmiljöer (IDEs) kan du använda SSDT i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01d3d-126">If you're an application developer familiar with Visual Studio or other integrated development environments (IDEs), try using SSDT in Visual Studio.</span></span>

<span data-ttu-id="01d3d-127">SSDT innehåller hello SQL Server Object Explorer, vilket gör att du toovisualize, ansluta och köra skript mot SQL Data Warehouse-databaser.</span><span class="sxs-lookup"><span data-stu-id="01d3d-127">SSDT includes hello SQL Server Object Explorer which enables you toovisualize, connect, and execute scripts against SQL Data Warehouse databases.</span></span> <span data-ttu-id="01d3d-128">tooquickly ansluta tooSQL Data Warehouse, klickar du bara hello **öppna i Visual Studio** knappen i kommandofältet hello när du visar hello databasinformation i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="01d3d-128">tooquickly connect tooSQL Data Warehouse, you can simply click hello **Open in Visual Studio** button in hello command bar when viewing hello database details in hello Azure Classic Portal.</span></span>  

<span data-ttu-id="01d3d-129">tooget igång med SSDT i Visual Studio finns [frågan Azure SQL Data Warehouse med Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="01d3d-129">tooget started with SSDT in Visual Studio, see [Query Azure SQL Data Warehouse with Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].</span></span>

### <a name="command-line-tools"></a><span data-ttu-id="01d3d-130">Kommandoradsverktyg</span><span class="sxs-lookup"><span data-stu-id="01d3d-130">Command-line tools</span></span>
<span data-ttu-id="01d3d-131">Kommandoradsverktyg lämpar sig för att automatisera dina arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="01d3d-131">Command line tools are ideal for automating your workloads.</span></span>  <span data-ttu-id="01d3d-132">PowerShell och sqlcmd är två bra sätt tooautomate dina processer.</span><span class="sxs-lookup"><span data-stu-id="01d3d-132">PowerShell and sqlcmd are two great ways tooautomate your processes.</span></span>  <span data-ttu-id="01d3d-133">Vi rekommenderar att dessa verktyg för att hantera ett stort antal logiska servrar och distribuera resursändringar i en produktionsmiljö som hello nödvändiga åtgärder kan användas av skriptet och sedan automatiseras.</span><span class="sxs-lookup"><span data-stu-id="01d3d-133">We recommend these tools for managing a large number of logical servers and deploying resource changes in a production environment as hello tasks necessary can be scripted and then automated.</span></span>

### <a name="dynamic-management-views"></a><span data-ttu-id="01d3d-134">Dynamiska hanteringsvyer</span><span class="sxs-lookup"><span data-stu-id="01d3d-134">Dynamic management views</span></span>
<span data-ttu-id="01d3d-135">Av DMV: er är hello bröd och smör för att hantera SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="01d3d-135">DMVs are hello bread and butter of managing SQL Data Warehouse.</span></span> <span data-ttu-id="01d3d-136">Nästan alla information som hämtar hello-portalen är beroende av DMV: er.</span><span class="sxs-lookup"><span data-stu-id="01d3d-136">Almost all information that surfaces in hello portal relies on DMVs.</span></span> <span data-ttu-id="01d3d-137">toosee en lista över SQL Data Warehouse av DMV: er, se [SQL Data Warehouse systemvyer][SQL Data Warehouse system views].</span><span class="sxs-lookup"><span data-stu-id="01d3d-137">toosee a list of SQL Data Warehouse DMVs, see [SQL Data Warehouse system views][SQL Data Warehouse system views].</span></span>

<span data-ttu-id="01d3d-138">tooget igång, se [Anslut och fråga med sqlcmd][Connect and query with sqlcmd], och [skapa en databas (PowerShell)][Create a database (PowerShell)].</span><span class="sxs-lookup"><span data-stu-id="01d3d-138">tooget started, see [Connect and query with sqlcmd][Connect and query with sqlcmd], and [Create a database (PowerShell)][Create a database (PowerShell)].</span></span>

## <a name="scale-compute"></a><span data-ttu-id="01d3d-139">Skala bearbetning</span><span class="sxs-lookup"><span data-stu-id="01d3d-139">Scale compute</span></span>
<span data-ttu-id="01d3d-140">Du kan snabbt skala ut eller igen genom att öka eller minska beräkningsresurser av CPU, minne och i/o-bandbredd i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="01d3d-140">In SQL Data Warehouse, you can quickly scale performance out or back by increasing or decreasing compute resources of CPU, memory, and I/O bandwidth.</span></span> <span data-ttu-id="01d3d-141">tooscale prestanda, allt du behöver toodo är justera hello antalet informationslagerenheter (dwu: er) som SQL Data Warehouse allokerar tooyour databas.</span><span class="sxs-lookup"><span data-stu-id="01d3d-141">tooscale performance, all you need toodo is adjust hello number of data warehouse units (DWUs) that SQL Data Warehouse allocates tooyour database.</span></span> <span data-ttu-id="01d3d-142">SQL Data Warehouse gör hello ändra snabbt och hanterar alla hello underliggande ändringar toohardware eller programvara.</span><span class="sxs-lookup"><span data-stu-id="01d3d-142">SQL Data Warehouse quickly makes hello change and handles all hello underlying changes toohardware or software.</span></span>

<span data-ttu-id="01d3d-143">toolearn mer om att skala dwu: er, se [skala prestanda].</span><span class="sxs-lookup"><span data-stu-id="01d3d-143">toolearn more about scaling DWUs, see [Scale performance].</span></span>

## <a name="pause-and-resume"></a><span data-ttu-id="01d3d-144">Pausa och återuppta</span><span class="sxs-lookup"><span data-stu-id="01d3d-144">Pause and resume</span></span>
<span data-ttu-id="01d3d-145">toosave kostnader, du kan pausa och återuppta beräkna resurser på begäran.</span><span class="sxs-lookup"><span data-stu-id="01d3d-145">toosave costs, you can pause and resume compute resources on-demand.</span></span> <span data-ttu-id="01d3d-146">Till exempel om du inte använder hello databasen under hello natten och helger, kan du pausa under dessa tider och återuppta under hello dag.</span><span class="sxs-lookup"><span data-stu-id="01d3d-146">For example, if you won't be using hello database during hello night and on weekends, you can pause it during those times, and resume it during hello day.</span></span> <span data-ttu-id="01d3d-147">Du kommer inte att debiteras för dwu: er medan hello databasen har pausats.</span><span class="sxs-lookup"><span data-stu-id="01d3d-147">You won't be charged for DWUs while hello database is paused.</span></span>

<span data-ttu-id="01d3d-148">Mer information finns i [pausa beräkning][Pause compute], och [återuppta databearbetning][Resume compute].</span><span class="sxs-lookup"><span data-stu-id="01d3d-148">For more information, see [Pause compute][Pause compute], and [Resume compute][Resume compute].</span></span>

## <a name="performance-best-practices"></a><span data-ttu-id="01d3d-149">Bästa praxis för prestanda</span><span class="sxs-lookup"><span data-stu-id="01d3d-149">Performance Best Practices</span></span>
<span data-ttu-id="01d3d-150">När du börjar arbeta med en ny teknik, kan identifiera hello tips och råd som fungerar bäst direkt från hello start spara mycket tid.</span><span class="sxs-lookup"><span data-stu-id="01d3d-150">When getting started with a new technology, discovering hello tips and tricks that work best right from hello start can save you lots of time.</span></span>  <span data-ttu-id="01d3d-151">Du hittar bästa praxis i många av våra artiklar.</span><span class="sxs-lookup"><span data-stu-id="01d3d-151">You will find best practices throughout many of our topics.</span></span>

<span data-ttu-id="01d3d-152">toosee många en sammanfattning av hello viktigaste att tänka på när du utvecklar din arbetsbelastning finns [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="01d3d-152">toosee many a summary of hello most important considerations when developing your workload, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

## <a name="query-monitoring"></a><span data-ttu-id="01d3d-153">Övervakning av frågan</span><span class="sxs-lookup"><span data-stu-id="01d3d-153">Query Monitoring</span></span>
<span data-ttu-id="01d3d-154">En fråga körs ibland för länge, men du är inte säker på vilken som är hello sällan.</span><span class="sxs-lookup"><span data-stu-id="01d3d-154">Sometimes a query is running too long, but you aren't sure of which one is hello culprit.</span></span> <span data-ttu-id="01d3d-155">SQL Data Warehouse har dynamiska hanteringsvyer (av DMV: er) som du kan använda toofigure reda på vilka frågan tar för lång.</span><span class="sxs-lookup"><span data-stu-id="01d3d-155">SQL Data Warehouse has dynamic management views (DMVs) that you can use toofigure out which query is taking too long.</span></span>

<span data-ttu-id="01d3d-156">toofind tidskrävande frågor finns [övervaka din arbetsbelastning med av DMV: er][Monitor your workload using DMVs].</span><span class="sxs-lookup"><span data-stu-id="01d3d-156">toofind long-running queries, see [Monitor your workload using DMVs][Monitor your workload using DMVs].</span></span>

## <a name="security"></a><span data-ttu-id="01d3d-157">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="01d3d-157">Security</span></span>
<span data-ttu-id="01d3d-158">toomaintain ett säkerhetssystem som du måste vara på hello avisering och skydda datorn mot alla typer av obehörig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="01d3d-158">toomaintain a secure system, you must be on hello alert and guard against any type of unauthorized access.</span></span> <span data-ttu-id="01d3d-159">Ett säkerhetssystem måste toomake till brandväggsregler som finns på plats så att endast auktoriserade IP-adresser kan ansluta.</span><span class="sxs-lookup"><span data-stu-id="01d3d-159">A security system needs toomake sure firewall rules are in place so only authorized IP addresses can connect.</span></span> <span data-ttu-id="01d3d-160">Den måste korrekt autentisering av användarautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="01d3d-160">It needs proper authentication of user credentials.</span></span> <span data-ttu-id="01d3d-161">När en användare har anslutits toohello databasen hello användare ska bara ha behörigheter tooperform ett minimalt antal åtgärder.</span><span class="sxs-lookup"><span data-stu-id="01d3d-161">After a user has connected toohello database, hello user should only have permissions tooperform a minimal number of actions.</span></span> <span data-ttu-id="01d3d-162">toosecure data, kan du använda kryptering.</span><span class="sxs-lookup"><span data-stu-id="01d3d-162">toosecure data, you can use encryption.</span></span> <span data-ttu-id="01d3d-163">Det är också viktigt toohave granskning och spåra så att du kan gå händelser om det inte finns några misstänkt aktivitet.</span><span class="sxs-lookup"><span data-stu-id="01d3d-163">It's also important toohave auditing and tracking so you can retrace events if there is any suspicious activity.</span></span>

<span data-ttu-id="01d3d-164">toolearn om att hantera säkerhet, head över toohello [Säkerhetsöversikt][Security overview].</span><span class="sxs-lookup"><span data-stu-id="01d3d-164">toolearn about managing security, head over toohello [Security overview][Security overview].</span></span>

## <a name="backup-and-restore"></a><span data-ttu-id="01d3d-165">Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="01d3d-165">Backup and restore</span></span>
<span data-ttu-id="01d3d-166">Med tillförlitlig backps av dina data är en viktig del av en produktionsdatabas.</span><span class="sxs-lookup"><span data-stu-id="01d3d-166">Having reliable backps of your data is an essential part of any production database.</span></span> <span data-ttu-id="01d3d-167">SQL Data Warehouse hålls data säker genom att säkerhetskopiera active databaserna automatiskt med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="01d3d-167">SQL Data Warehouse keeps your data safe by automatically backing up your active databases at regular intervals.</span></span> <span data-ttu-id="01d3d-168">Dessa säkerhetskopior kan du toorecover från scenarier där du har skadade data eller av misstag bort data eller databasen.</span><span class="sxs-lookup"><span data-stu-id="01d3d-168">These backups allow you toorecover from scenarios where you've corrupted your data or accidentally dropped your data or database.</span></span>  <span data-ttu-id="01d3d-169">För hello data schemat för säkerhetskopiering, bevarandeprincip och hur toorestore en databas, se [återställning från ögonblicksbild][Restore from snapshot].</span><span class="sxs-lookup"><span data-stu-id="01d3d-169">For hello data backup schedule, retention policy and how toorestore a database, see [Restore from snapshot][Restore from snapshot].</span></span>

## <a name="next-steps"></a><span data-ttu-id="01d3d-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="01d3d-170">Next steps</span></span>
<span data-ttu-id="01d3d-171">Med bra databas principer gör det enklare toomanage dina databaser i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="01d3d-171">Using good database design principles will make it easier toomanage your databases in SQL Data Warehouse.</span></span> <span data-ttu-id="01d3d-172">fler toolearn head över toohello [utvecklingsöversikt][Development overview].</span><span class="sxs-lookup"><span data-stu-id="01d3d-172">toolearn more, head over toohello [Development overview][Development overview].</span></span>

<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse (Azure Portal)]: sql-data-warehouse-get-started-provision.md
[Create a database (PowerShell)]: sql-data-warehouse-get-started-provision-powershell.md
[connection]: sql-data-warehouse-develop-connections.md
[Query Azure SQL Data Warehouse with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Connect and query with sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Development overview]: sql-data-warehouse-overview-develop.md
[Monitor your workload using DMVs]: sql-data-warehouse-manage-monitor.md
[Pause compute]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Restore from snapshot]: sql-data-warehouse-restore-database-overview.md
[Resume compute]: sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[skala prestanda]: sql-data-warehouse-manage-compute-overview.md#scale-compute
[Security overview]: sql-data-warehouse-overview-manage-security.md
[SQL Data Warehouse Best Practices]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse system views]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portal]: http://portal.azure.com/
