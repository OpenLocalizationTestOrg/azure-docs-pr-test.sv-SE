---
title: Hantera databaser i Azure SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: b14d0aad5a1f50c225391dbab27ec6240423a65a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-databases-in-azure-sql-data-warehouse"></a><span data-ttu-id="2dc3e-104">Hantera databaser i Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2dc3e-104">Manage databases in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="2dc3e-105">SQL Data Warehouse automatiserar många aspekter av att hantera dina databaser.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-105">SQL Data Warehouse automates many aspects of managing your databases.</span></span> <span data-ttu-id="2dc3e-106">Om du vill skala prestanda behöver du bara att justera och betala för rätt nivå av beräkningsresurser och därefter låta SQL Data Warehouse gör allt arbete med skala ut och skala tillbaka.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-106">For example, to scale performance you only need to adjust and pay for the right level of compute resources, and then let SQL Data Warehouse do all the work of scaling out and scaling back.</span></span>

<span data-ttu-id="2dc3e-107">Säkerligen kommer du vilja övervaka din arbetsbelastning för att identifiera dina prestandabehov samt felsöka tidskrävande frågor.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-107">You will undoubtedly want to monitor your workload to identify your performance needs as well as troubleshoot long-running queries.</span></span> <span data-ttu-id="2dc3e-108">Du måste också utföra några säkerhetsaktiviteter för att hantera behörigheter för användare och inloggningar.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-108">You will also need to perform a few security tasks to manage permissions for users and logins.</span></span>

<span data-ttu-id="2dc3e-109">Den här översikten beskrivs dessa aspekter av att hantera SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-109">This overview covers these aspects of managing SQL Data Warehouse.</span></span>

* <span data-ttu-id="2dc3e-110">Hanteringsverktyg</span><span class="sxs-lookup"><span data-stu-id="2dc3e-110">Management tools</span></span>
* <span data-ttu-id="2dc3e-111">Skala bearbetning</span><span class="sxs-lookup"><span data-stu-id="2dc3e-111">Scale Compute</span></span>
* <span data-ttu-id="2dc3e-112">Pausa och återuppta</span><span class="sxs-lookup"><span data-stu-id="2dc3e-112">Pause and Resume</span></span>
* <span data-ttu-id="2dc3e-113">Bästa praxis för prestanda</span><span class="sxs-lookup"><span data-stu-id="2dc3e-113">Performance Best Practices</span></span>
* <span data-ttu-id="2dc3e-114">Övervakning av frågan</span><span class="sxs-lookup"><span data-stu-id="2dc3e-114">Query Monitoring</span></span>
* <span data-ttu-id="2dc3e-115">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="2dc3e-115">Security</span></span>
* <span data-ttu-id="2dc3e-116">Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="2dc3e-116">Backup and restore</span></span>

## <a name="management-tools"></a><span data-ttu-id="2dc3e-117">Hanteringsverktyg</span><span class="sxs-lookup"><span data-stu-id="2dc3e-117">Management tools</span></span>
<span data-ttu-id="2dc3e-118">Du kan använda en mängd olika verktyg för att hantera databaser i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-118">You can use a variety of tools to manage databases in SQL Data Warehouse.</span></span> <span data-ttu-id="2dc3e-119">När du hanterar databaser utvecklar du inställningar för varje typ av uppgift som du behöver utföra.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-119">As you manage databases, you will develop tool preferences for each type of task you need to perform.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="2dc3e-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2dc3e-120">Azure portal</span></span>
<span data-ttu-id="2dc3e-121">Den [Azure-portalen] [ Azure portal] är en webbaserad portal där du kan skapa, uppdatera, och ta bort databaser och övervaka databasresurser.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-121">The [Azure portal][Azure portal] is a web-based portal where you can create, update, and delete databases and monitor database resources.</span></span> <span data-ttu-id="2dc3e-122">Det här verktyget är bra om du precis har börjat med Azure, hantera ett litet antal databaser för datalager, eller gör så snabbt.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-122">This tool is great is if you're just getting started with Azure, managing a small number of data warehouse databases, or need to quickly do something.</span></span>

<span data-ttu-id="2dc3e-123">Om du vill komma igång med Azure-portalen, se [skapa ett SQL Data Warehouse (Azure portal)][Create a SQL Data Warehouse (Azure portal)].</span><span class="sxs-lookup"><span data-stu-id="2dc3e-123">To get started with the Azure portal, see [Create a SQL Data Warehouse (Azure portal)][Create a SQL Data Warehouse (Azure portal)].</span></span>

### <a name="sql-server-data-tools-in-visual-studio"></a><span data-ttu-id="2dc3e-124">SQL Server Data Tools i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2dc3e-124">SQL Server Data Tools in Visual Studio</span></span>
<span data-ttu-id="2dc3e-125">[SQL Server Data Tools] [ SQL Server Data Tools] (SSDT) i Visual Studio kan du ansluta till, hantera och utveckla dina databaser.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-125">[SQL Server Data Tools][SQL Server Data Tools] (SSDT) in Visual Studio allows you to connect to, manage, and develop your databases.</span></span> <span data-ttu-id="2dc3e-126">Om du är programutvecklare som är bekant med Visual Studio eller andra integrerad utvecklingsmiljöer (IDEs) kan du använda SSDT i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-126">If you're an application developer familiar with Visual Studio or other integrated development environments (IDEs), try using SSDT in Visual Studio.</span></span>

<span data-ttu-id="2dc3e-127">SSDT innehåller SQL Server Object Explorer, som gör det möjligt att visualisera, ansluta och köra skript mot SQL Data Warehouse-databaser.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-127">SSDT includes the SQL Server Object Explorer which enables you to visualize, connect, and execute scripts against SQL Data Warehouse databases.</span></span> <span data-ttu-id="2dc3e-128">För att snabbt ansluta till SQL Data Warehouse, klickar du bara den **öppna i Visual Studio** knapp i kommandofältet när Visa databasen information i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-128">To quickly connect to SQL Data Warehouse, you can simply click the **Open in Visual Studio** button in the command bar when viewing the database details in the Azure Classic Portal.</span></span>  

<span data-ttu-id="2dc3e-129">Om du vill komma igång med SSDT i Visual Studio, se [frågan Azure SQL Data Warehouse med Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="2dc3e-129">To get started with SSDT in Visual Studio, see [Query Azure SQL Data Warehouse with Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].</span></span>

### <a name="command-line-tools"></a><span data-ttu-id="2dc3e-130">Kommandoradsverktyg</span><span class="sxs-lookup"><span data-stu-id="2dc3e-130">Command-line tools</span></span>
<span data-ttu-id="2dc3e-131">Kommandoradsverktyg lämpar sig för att automatisera dina arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-131">Command line tools are ideal for automating your workloads.</span></span>  <span data-ttu-id="2dc3e-132">PowerShell och sqlcmd finns två bra sätt att automatisera dina processer.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-132">PowerShell and sqlcmd are two great ways to automate your processes.</span></span>  <span data-ttu-id="2dc3e-133">Vi rekommenderar att dessa verktyg för att hantera ett stort antal logiska servrar och distribuera resursändringar i en produktionsmiljö som nödvändiga åtgärder kan användas av skriptet och sedan automatiseras.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-133">We recommend these tools for managing a large number of logical servers and deploying resource changes in a production environment as the tasks necessary can be scripted and then automated.</span></span>

### <a name="dynamic-management-views"></a><span data-ttu-id="2dc3e-134">Dynamiska hanteringsvyer</span><span class="sxs-lookup"><span data-stu-id="2dc3e-134">Dynamic management views</span></span>
<span data-ttu-id="2dc3e-135">Av DMV: er är bröd och smör för att hantera SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-135">DMVs are the bread and butter of managing SQL Data Warehouse.</span></span> <span data-ttu-id="2dc3e-136">Nästan alla information som hämtar i portalen är beroende av DMV: er.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-136">Almost all information that surfaces in the portal relies on DMVs.</span></span> <span data-ttu-id="2dc3e-137">Om du vill se en lista över SQL Data Warehouse av DMV: er Se [SQL Data Warehouse systemvyer][SQL Data Warehouse system views].</span><span class="sxs-lookup"><span data-stu-id="2dc3e-137">To see a list of SQL Data Warehouse DMVs, see [SQL Data Warehouse system views][SQL Data Warehouse system views].</span></span>

<span data-ttu-id="2dc3e-138">Om du vill komma igång finns [Anslut och fråga med sqlcmd][Connect and query with sqlcmd], och [skapa en databas (PowerShell)][Create a database (PowerShell)].</span><span class="sxs-lookup"><span data-stu-id="2dc3e-138">To get started, see [Connect and query with sqlcmd][Connect and query with sqlcmd], and [Create a database (PowerShell)][Create a database (PowerShell)].</span></span>

## <a name="scale-compute"></a><span data-ttu-id="2dc3e-139">Skala bearbetning</span><span class="sxs-lookup"><span data-stu-id="2dc3e-139">Scale compute</span></span>
<span data-ttu-id="2dc3e-140">Du kan snabbt skala ut eller igen genom att öka eller minska beräkningsresurser av CPU, minne och i/o-bandbredd i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-140">In SQL Data Warehouse, you can quickly scale performance out or back by increasing or decreasing compute resources of CPU, memory, and I/O bandwidth.</span></span> <span data-ttu-id="2dc3e-141">Om du vill skala prestanda är allt du behöver göra justera antalet informationslagerenheter (dwu: er) som SQL Data Warehouse allokerar till din databas.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-141">To scale performance, all you need to do is adjust the number of data warehouse units (DWUs) that SQL Data Warehouse allocates to your database.</span></span> <span data-ttu-id="2dc3e-142">SQL Data Warehouse snabbt gör ändringen och hanterar alla underliggande ändringar av maskinvara eller programvara.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-142">SQL Data Warehouse quickly makes the change and handles all the underlying changes to hardware or software.</span></span>

<span data-ttu-id="2dc3e-143">Mer information om att skala dwu: er finns [skala prestanda].</span><span class="sxs-lookup"><span data-stu-id="2dc3e-143">To learn more about scaling DWUs, see [Scale performance].</span></span>

## <a name="pause-and-resume"></a><span data-ttu-id="2dc3e-144">Pausa och återuppta</span><span class="sxs-lookup"><span data-stu-id="2dc3e-144">Pause and resume</span></span>
<span data-ttu-id="2dc3e-145">Du kan pausa och återuppta beräkning resurser på begäran för att spara kostnader.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-145">To save costs, you can pause and resume compute resources on-demand.</span></span> <span data-ttu-id="2dc3e-146">Till exempel om du inte använder databasen under natten och helger, kan du pausa under dessa tider och återuppta under dagen.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-146">For example, if you won't be using the database during the night and on weekends, you can pause it during those times, and resume it during the day.</span></span> <span data-ttu-id="2dc3e-147">Du kommer inte att debiteras för dwu: er när databasen har pausats.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-147">You won't be charged for DWUs while the database is paused.</span></span>

<span data-ttu-id="2dc3e-148">Mer information finns i [pausa beräkning][Pause compute], och [återuppta databearbetning][Resume compute].</span><span class="sxs-lookup"><span data-stu-id="2dc3e-148">For more information, see [Pause compute][Pause compute], and [Resume compute][Resume compute].</span></span>

## <a name="performance-best-practices"></a><span data-ttu-id="2dc3e-149">Bästa praxis för prestanda</span><span class="sxs-lookup"><span data-stu-id="2dc3e-149">Performance Best Practices</span></span>
<span data-ttu-id="2dc3e-150">När du börjar arbeta med en ny teknik, kan identifiera tips och råd som fungerar bäst direkt från början spara mycket tid.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-150">When getting started with a new technology, discovering the tips and tricks that work best right from the start can save you lots of time.</span></span>  <span data-ttu-id="2dc3e-151">Du hittar bästa praxis i många av våra artiklar.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-151">You will find best practices throughout many of our topics.</span></span>

<span data-ttu-id="2dc3e-152">Många en sammanfattning av de viktigaste överväganden när du utvecklar din arbetsbelastning finns [Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="2dc3e-152">To see many a summary of the most important considerations when developing your workload, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

## <a name="query-monitoring"></a><span data-ttu-id="2dc3e-153">Övervakning av frågan</span><span class="sxs-lookup"><span data-stu-id="2dc3e-153">Query Monitoring</span></span>
<span data-ttu-id="2dc3e-154">En fråga körs ibland för länge, men du är inte säker på vilket som är sällan.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-154">Sometimes a query is running too long, but you aren't sure of which one is the culprit.</span></span> <span data-ttu-id="2dc3e-155">SQL Data Warehouse har dynamiska hanteringsvyer (av DMV: er) som du kan använda för att ta reda vilka frågan tar för lång.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-155">SQL Data Warehouse has dynamic management views (DMVs) that you can use to figure out which query is taking too long.</span></span>

<span data-ttu-id="2dc3e-156">Du hittar tidskrävande frågor [övervaka din arbetsbelastning med av DMV: er][Monitor your workload using DMVs].</span><span class="sxs-lookup"><span data-stu-id="2dc3e-156">To find long-running queries, see [Monitor your workload using DMVs][Monitor your workload using DMVs].</span></span>

## <a name="security"></a><span data-ttu-id="2dc3e-157">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="2dc3e-157">Security</span></span>
<span data-ttu-id="2dc3e-158">För att underhålla en säker, måste du vara på aviseringen och skydda datorn mot alla typer av obehörig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-158">To maintain a secure system, you must be on the alert and guard against any type of unauthorized access.</span></span> <span data-ttu-id="2dc3e-159">Ett säkerhetssystem måste se till att brandväggsreglerna på plats så att endast auktoriserade IP-adresser kan ansluta.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-159">A security system needs to make sure firewall rules are in place so only authorized IP addresses can connect.</span></span> <span data-ttu-id="2dc3e-160">Den måste korrekt autentisering av användarautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-160">It needs proper authentication of user credentials.</span></span> <span data-ttu-id="2dc3e-161">När en användare har anslutit till databasen kan bör användaren endast behörighet att utföra ett minimalt antal åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-161">After a user has connected to the database, the user should only have permissions to perform a minimal number of actions.</span></span> <span data-ttu-id="2dc3e-162">Du kan använda kryptering för att skydda data.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-162">To secure data, you can use encryption.</span></span> <span data-ttu-id="2dc3e-163">Det är också viktigt att granska och spåra så att du kan gå händelser om det inte finns några misstänkt aktivitet.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-163">It's also important to have auditing and tracking so you can retrace events if there is any suspicious activity.</span></span>

<span data-ttu-id="2dc3e-164">Mer information om att hantera säkerhet, gå till den [Säkerhetsöversikt][Security overview].</span><span class="sxs-lookup"><span data-stu-id="2dc3e-164">To learn about managing security, head over to the [Security overview][Security overview].</span></span>

## <a name="backup-and-restore"></a><span data-ttu-id="2dc3e-165">Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="2dc3e-165">Backup and restore</span></span>
<span data-ttu-id="2dc3e-166">Med tillförlitlig backps av dina data är en viktig del av en produktionsdatabas.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-166">Having reliable backps of your data is an essential part of any production database.</span></span> <span data-ttu-id="2dc3e-167">SQL Data Warehouse hålls data säker genom att säkerhetskopiera active databaserna automatiskt med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-167">SQL Data Warehouse keeps your data safe by automatically backing up your active databases at regular intervals.</span></span> <span data-ttu-id="2dc3e-168">Dessa säkerhetskopior kan du återställa från scenarier där du har skadade data eller av misstag bort data eller databasen.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-168">These backups allow you to recover from scenarios where you've corrupted your data or accidentally dropped your data or database.</span></span>  <span data-ttu-id="2dc3e-169">Data schemat för säkerhetskopiering, bevarandeprincip och hur du återställer en databas, finns i [återställning från ögonblicksbild][Restore from snapshot].</span><span class="sxs-lookup"><span data-stu-id="2dc3e-169">For the data backup schedule, retention policy and how to restore a database, see [Restore from snapshot][Restore from snapshot].</span></span>

## <a name="next-steps"></a><span data-ttu-id="2dc3e-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2dc3e-170">Next steps</span></span>
<span data-ttu-id="2dc3e-171">Med bra design principer gör det enklare att hantera dina databaser i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2dc3e-171">Using good database design principles will make it easier to manage your databases in SQL Data Warehouse.</span></span> <span data-ttu-id="2dc3e-172">Mer information, gå till den [utvecklingsöversikt][Development overview].</span><span class="sxs-lookup"><span data-stu-id="2dc3e-172">To learn more, head over to the [Development overview][Development overview].</span></span>

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
<span data-ttu-id="2dc3e-173">[skala prestanda]: sql-data-warehouse-manage-compute-overview.md#scale-compute</span><span class="sxs-lookup"><span data-stu-id="2dc3e-173">[Scale performance]: sql-data-warehouse-manage-compute-overview.md#scale-compute</span></span>
[Security overview]: sql-data-warehouse-overview-manage-security.md
[SQL Data Warehouse Best Practices]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse system views]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portal]: http://portal.azure.com/
