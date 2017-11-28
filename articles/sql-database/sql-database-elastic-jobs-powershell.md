---
title: "aaaCreate och hantera elastiska jobb med hjälp av PowerShell | Microsoft Docs"
description: "PowerShell används toomanage Azure SQL Database-pooler"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 737d8d13-5632-4e18-9cb0-4d3b8a19e495
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: f6c18aecfa7e8c0b102a3b7cd2f266f5542ae400
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-sql-database-elastic-jobs-using-powershell-preview"></a><span data-ttu-id="89c59-103">Skapa och hantera SQL Database: elastiska jobb med hjälp av PowerShell (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="89c59-103">Create and manage SQL Database elastic jobs using PowerShell (preview)</span></span>

<span data-ttu-id="89c59-104">hello PowerShell APIs för **elastisk databas jobb** (under förhandsgranskning) gör att du kan definiera en grupp databaser mot vilken skript körs.</span><span class="sxs-lookup"><span data-stu-id="89c59-104">hello PowerShell APIs for **Elastic Database jobs** (in preview), let you define a group of databases against which scripts will execute.</span></span> <span data-ttu-id="89c59-105">Den här artikeln visar hur toocreate och hantera **elastisk databas jobb** med PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="89c59-105">This article shows how toocreate and manage **Elastic Database jobs** using PowerShell cmdlets.</span></span> <span data-ttu-id="89c59-106">Se [elastiska jobb översikt](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="89c59-106">See [Elastic jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="89c59-107">Krav</span><span class="sxs-lookup"><span data-stu-id="89c59-107">Prerequisites</span></span>
* <span data-ttu-id="89c59-108">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="89c59-108">An Azure subscription.</span></span> <span data-ttu-id="89c59-109">För en kostnadsfri utvärderingsversion finns [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="89c59-109">For a free trial, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="89c59-110">En uppsättning databaser som skapats med hello elastisk Databasverktyg.</span><span class="sxs-lookup"><span data-stu-id="89c59-110">A set of databases created with hello Elastic Database tools.</span></span> <span data-ttu-id="89c59-111">Se [Kom igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="89c59-111">See [Get started with Elastic Database tools](sql-database-elastic-scale-get-started.md).</span></span>
* <span data-ttu-id="89c59-112">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="89c59-112">Azure PowerShell.</span></span> <span data-ttu-id="89c59-113">Detaljerad information finns i [hur tooinstall och konfigurera Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="89c59-113">For detailed information, see [How tooinstall and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span>
* <span data-ttu-id="89c59-114">**Den elastiska databasen jobb** PowerShell paket: finns [jobb installerar elastisk databas](sql-database-elastic-jobs-service-installation.md)</span><span class="sxs-lookup"><span data-stu-id="89c59-114">**Elastic Database jobs** PowerShell package: See [Installing Elastic Database jobs](sql-database-elastic-jobs-service-installation.md)</span></span>

### <a name="select-your-azure-subscription"></a><span data-ttu-id="89c59-115">Välj din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="89c59-115">Select your Azure subscription</span></span>
<span data-ttu-id="89c59-116">tooselect hello prenumeration som du behöver ditt prenumerations-Id (**- SubscriptionId**) eller prenumerationsnamn (**- SubscriptionName**).</span><span class="sxs-lookup"><span data-stu-id="89c59-116">tooselect hello subscription you need your subscription Id (**-SubscriptionId**) or subscription name (**-SubscriptionName**).</span></span> <span data-ttu-id="89c59-117">Om du har flera prenumerationer kan du köra hello **Get-AzureRmSubscription** cmdlet och kopiera hello önskad prenumerationsinformation hello resultatmängden.</span><span class="sxs-lookup"><span data-stu-id="89c59-117">If you have multiple subscriptions you can run hello **Get-AzureRmSubscription** cmdlet and copy hello desired subscription information from hello result set.</span></span> <span data-ttu-id="89c59-118">När du har din prenumerationsinformation kör följande cmdlet tooset hello prenumerationen som hello standard, nämligen hello mål för att skapa och hantera jobb:</span><span class="sxs-lookup"><span data-stu-id="89c59-118">Once you have your subscription information, run hello following commandlet tooset this subscription as hello default, namely hello target for creating and managing jobs:</span></span>

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

<span data-ttu-id="89c59-119">Hej [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) rekommenderas för användning toodevelop och köra PowerShell-skript mot hello elastisk databas jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-119">hello [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) is recommended for usage toodevelop and execute PowerShell scripts against hello Elastic Database jobs.</span></span>

## <a name="elastic-database-jobs-objects"></a><span data-ttu-id="89c59-120">Elastiska jobb databasobjekt</span><span class="sxs-lookup"><span data-stu-id="89c59-120">Elastic Database jobs objects</span></span>
<span data-ttu-id="89c59-121">Hej följande tabell visar ut alla hello objekttyper för **elastisk databas jobb** tillsammans med dess beskrivning och relevanta PowerShell APIs.</span><span class="sxs-lookup"><span data-stu-id="89c59-121">hello following table lists out all hello object types of **Elastic Database jobs** along with its description and relevant PowerShell APIs.</span></span>

<table style="width:100%">
  <tr>
    <th><span data-ttu-id="89c59-122">Objekttyp</span><span class="sxs-lookup"><span data-stu-id="89c59-122">Object Type</span></span></th>
    <th><span data-ttu-id="89c59-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="89c59-123">Description</span></span></th>
    <th><span data-ttu-id="89c59-124">Relaterade PowerShell API: er</span><span class="sxs-lookup"><span data-stu-id="89c59-124">Related PowerShell APIs</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="89c59-125">Autentiseringsuppgift</span><span class="sxs-lookup"><span data-stu-id="89c59-125">Credential</span></span></td>
    <td><span data-ttu-id="89c59-126">Användarnamn och lösenord toouse när du ansluter toodatabases för körning av skript eller ett program av DACPACs.</span><span class="sxs-lookup"><span data-stu-id="89c59-126">Username and password toouse when connecting toodatabases for execution of scripts or application of DACPACs.</span></span> <p><span data-ttu-id="89c59-127">hello lösenord krypteras innan det skickas tooand lagra i hello elastiska databasen jobb databas.</span><span class="sxs-lookup"><span data-stu-id="89c59-127">hello password is encrypted before sending tooand storing in hello Elastic Database Jobs database.</span></span>  <span data-ttu-id="89c59-128">hello lösenord dekrypteras av hello elastiska databasen jobb tjänsten via hello autentiseringsuppgifter skapas och som överförts från hello installationsskript.</span><span class="sxs-lookup"><span data-stu-id="89c59-128">hello password is decrypted by hello Elastic Database Jobs service via hello credential created and uploaded from hello installation script.</span></span></td>
    <td><p><span data-ttu-id="89c59-129">Get-AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="89c59-129">Get-AzureSqlJobCredential</span></span></p>
    <p><span data-ttu-id="89c59-130">Ny AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="89c59-130">New-AzureSqlJobCredential</span></span></p><p><span data-ttu-id="89c59-131">Ange AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="89c59-131">Set-AzureSqlJobCredential</span></span></p></td></td>
  </tr>

  <tr>
    <td><span data-ttu-id="89c59-132">Skript</span><span class="sxs-lookup"><span data-stu-id="89c59-132">Script</span></span></td>
    <td><span data-ttu-id="89c59-133">Transact-SQL-skript toobe används för att köras över databaser.</span><span class="sxs-lookup"><span data-stu-id="89c59-133">Transact-SQL script toobe used for execution across databases.</span></span>  <span data-ttu-id="89c59-134">hello skript ska vara idempotent skapade toobe eftersom hello tjänsten kommer att försöka köra hello skriptet vid fel.</span><span class="sxs-lookup"><span data-stu-id="89c59-134">hello script should be authored toobe idempotent since hello service will retry execution of hello script upon failures.</span></span>
    </td>
    <td>
    <p><span data-ttu-id="89c59-135">Get-AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="89c59-135">Get-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="89c59-136">Get-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="89c59-136">Get-AzureSqlJobContentDefinition</span></span></p>
    <p><span data-ttu-id="89c59-137">Ny AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="89c59-137">New-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="89c59-138">Ange AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="89c59-138">Set-AzureSqlJobContentDefinition</span></span></p>
    </td>
  </tr>

  <tr>
    <td><span data-ttu-id="89c59-139">DACPAC</span><span class="sxs-lookup"><span data-stu-id="89c59-139">DACPAC</span></span></td>
    <td><span data-ttu-id="89c59-140"><a href="https://msdn.microsoft.com/library/ee210546.aspx">Programmet på datanivå </a> paketet toobe används över flera databaser.</span><span class="sxs-lookup"><span data-stu-id="89c59-140"><a href="https://msdn.microsoft.com/library/ee210546.aspx">Data-tier application </a> package toobe applied across databases.</span></span>

    </td>
    <td>
    <p><span data-ttu-id="89c59-141">Get-AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="89c59-141">Get-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="89c59-142">Ny AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="89c59-142">New-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="89c59-143">Ange AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="89c59-143">Set-AzureSqlJobContentDefinition</span></span></p>
    </td>
  </tr>
  <tr>
    <td><span data-ttu-id="89c59-144">Databasen mål</span><span class="sxs-lookup"><span data-stu-id="89c59-144">Database Target</span></span></td>
    <td><span data-ttu-id="89c59-145">Databas- och name peka tooan Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="89c59-145">Database and server name pointing tooan Azure SQL Database.</span></span>

    </td>
    <td>
    <p><span data-ttu-id="89c59-146">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="89c59-146">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="89c59-147">Ny AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="89c59-147">New-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
  <tr>
    <td><span data-ttu-id="89c59-148">Fragmentera kartan mål</span><span class="sxs-lookup"><span data-stu-id="89c59-148">Shard Map Target</span></span></td>
    <td><span data-ttu-id="89c59-149">Kombinationen av target databas och en autentiseringsuppgift toobe används toodetermine information som lagras i en elastisk databas Fragmentera mappning.</span><span class="sxs-lookup"><span data-stu-id="89c59-149">Combination of a database target and a credential toobe used toodetermine information stored within an Elastic Database shard map.</span></span>
    </td>
    <td>
    <p><span data-ttu-id="89c59-150">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="89c59-150">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="89c59-151">Ny AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="89c59-151">New-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="89c59-152">Ange AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="89c59-152">Set-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
<tr>
    <td><span data-ttu-id="89c59-153">Mål för anpassad insamling</span><span class="sxs-lookup"><span data-stu-id="89c59-153">Custom Collection Target</span></span></td>
    <td><span data-ttu-id="89c59-154">Angiven grupp databaser toocollectively använda för körning.</span><span class="sxs-lookup"><span data-stu-id="89c59-154">Defined group of databases toocollectively use for execution.</span></span></td>
    <td>
    <p><span data-ttu-id="89c59-155">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="89c59-155">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="89c59-156">Ny AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="89c59-156">New-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
<tr>
    <td><span data-ttu-id="89c59-157">Anpassad samling underordnade mål</span><span class="sxs-lookup"><span data-stu-id="89c59-157">Custom Collection Child Target</span></span></td>
    <td><span data-ttu-id="89c59-158">Mål för databasen som refereras från en anpassad samling.</span><span class="sxs-lookup"><span data-stu-id="89c59-158">Database target that is referenced from a custom collection.</span></span></td>
    <td>
    <p><span data-ttu-id="89c59-159">Lägg till AzureSqlJobChildTarget</span><span class="sxs-lookup"><span data-stu-id="89c59-159">Add-AzureSqlJobChildTarget</span></span></p>
    <p><span data-ttu-id="89c59-160">Ta bort AzureSqlJobChildTarget</span><span class="sxs-lookup"><span data-stu-id="89c59-160">Remove-AzureSqlJobChildTarget</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="89c59-161">Jobb</span><span class="sxs-lookup"><span data-stu-id="89c59-161">Job</span></span></td>
    <td>
    <p><span data-ttu-id="89c59-162">Definition av parametrar för ett jobb som kan använda tootrigger körning eller toofulfill ett schema.</span><span class="sxs-lookup"><span data-stu-id="89c59-162">Definition of parameters for a job that can be used tootrigger execution or toofulfill a schedule.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="89c59-163">Get-AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="89c59-163">Get-AzureSqlJob</span></span></p>
    <p><span data-ttu-id="89c59-164">Ny AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="89c59-164">New-AzureSqlJob</span></span></p>
    <p><span data-ttu-id="89c59-165">Ange AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="89c59-165">Set-AzureSqlJob</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="89c59-166">Jobbkörningen</span><span class="sxs-lookup"><span data-stu-id="89c59-166">Job Execution</span></span></td>
    <td>
    <p><span data-ttu-id="89c59-167">Behållare för aktiviteter nödvändiga toofulfill antingen köra ett skript eller använda DACPAC tooa mål med autentiseringsuppgifter för databasanslutningar med fel hanteras i enlighet tooan körningsprincipen.</span><span class="sxs-lookup"><span data-stu-id="89c59-167">Container of tasks necessary toofulfill either executing a script or applying a DACPAC tooa target using credentials for database connections with failures handled in accordance tooan execution policy.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="89c59-168">Get-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="89c59-168">Get-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="89c59-169">Start-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="89c59-169">Start-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="89c59-170">Stoppa AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="89c59-170">Stop-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="89c59-171">Vänta AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="89c59-171">Wait-AzureSqlJobExecution</span></span></p>
  </tr>

<tr>
    <td><span data-ttu-id="89c59-172">Jobb för körning av aktiviteten</span><span class="sxs-lookup"><span data-stu-id="89c59-172">Job Task Execution</span></span></td>
    <td>
    <p><span data-ttu-id="89c59-173">Enhet för arbete toofulfill ett jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-173">Single unit of work toofulfill a job.</span></span></p>
    <p><span data-ttu-id="89c59-174">Om en projektaktivitet inte kan toosuccessfully köra, hello resulterande Undantagsmeddelande loggas och en ny matchande projektaktivitet skapas och körs i enlighet toohello angetts körningsprincipen.</span><span class="sxs-lookup"><span data-stu-id="89c59-174">If a job task is not able toosuccessfully execute, hello resulting exception message will be logged and a new matching job task will be created and executed in accordance toohello specified execution policy.</span></span></p></p>
    </td>
    <td>
    <p><span data-ttu-id="89c59-175">Get-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="89c59-175">Get-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="89c59-176">Start-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="89c59-176">Start-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="89c59-177">Stoppa AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="89c59-177">Stop-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="89c59-178">Vänta AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="89c59-178">Wait-AzureSqlJobExecution</span></span></p>
  </tr>

<tr>
    <td><span data-ttu-id="89c59-179">Jobbet körningsprincipen</span><span class="sxs-lookup"><span data-stu-id="89c59-179">Job Execution Policy</span></span></td>
    <td>
    <p><span data-ttu-id="89c59-180">Kontroller jobbet körning timeout, försök gränser och intervall mellan försök.</span><span class="sxs-lookup"><span data-stu-id="89c59-180">Controls job execution timeouts, retry limits and intervals between retries.</span></span></p>
    <p><span data-ttu-id="89c59-181">Den elastiska databasen jobb innehåller en körningsprincip för standard-jobb som orsakar i princip obegränsat återförsök uppgiften jobbfel med exponentiell backoff intervall mellan varje försök.</span><span class="sxs-lookup"><span data-stu-id="89c59-181">Elastic Database jobs includes a default job execution policy which cause essentially infinite retries of job task failures with exponential backoff of intervals between each retry.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="89c59-182">Get-AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="89c59-182">Get-AzureSqlJobExecutionPolicy</span></span></p>
    <p><span data-ttu-id="89c59-183">Ny AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="89c59-183">New-AzureSqlJobExecutionPolicy</span></span></p>
    <p><span data-ttu-id="89c59-184">Ange AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="89c59-184">Set-AzureSqlJobExecutionPolicy</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="89c59-185">Schema</span><span class="sxs-lookup"><span data-stu-id="89c59-185">Schedule</span></span></td>
    <td>
    <p><span data-ttu-id="89c59-186">Tid baserat specifikationen för körning av tootake plats på ett reoccurring intervall eller på en gång.</span><span class="sxs-lookup"><span data-stu-id="89c59-186">Time based specification for execution tootake place either on a reoccurring interval or at a single time.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="89c59-187">Get-AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="89c59-187">Get-AzureSqlJobSchedule</span></span></p>
    <p><span data-ttu-id="89c59-188">Ny AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="89c59-188">New-AzureSqlJobSchedule</span></span></p>
    <p><span data-ttu-id="89c59-189">Ange AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="89c59-189">Set-AzureSqlJobSchedule</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="89c59-190">Jobbet utlöser</span><span class="sxs-lookup"><span data-stu-id="89c59-190">Job Triggers</span></span></td>
    <td>
    <p><span data-ttu-id="89c59-191">En mappning mellan ett jobb och ett schema tootrigger jobbkörningen enligt toohello schema.</span><span class="sxs-lookup"><span data-stu-id="89c59-191">A mapping between a job and a schedule tootrigger job execution according toohello schedule.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="89c59-192">Ny AzureSqlJobTrigger</span><span class="sxs-lookup"><span data-stu-id="89c59-192">New-AzureSqlJobTrigger</span></span></p>
    <p><span data-ttu-id="89c59-193">Ta bort AzureSqlJobTrigger</span><span class="sxs-lookup"><span data-stu-id="89c59-193">Remove-AzureSqlJobTrigger</span></span></p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a><span data-ttu-id="89c59-194">Stöds för elastisk databas jobb gruppera typer</span><span class="sxs-lookup"><span data-stu-id="89c59-194">Supported Elastic Database jobs group types</span></span>
<span data-ttu-id="89c59-195">hello jobbet kör Transact-SQL (T-SQL)-skript eller ett program av DACPACs över flera databaser.</span><span class="sxs-lookup"><span data-stu-id="89c59-195">hello job executes Transact-SQL (T-SQL) scripts or application of DACPACs across a group of databases.</span></span> <span data-ttu-id="89c59-196">När ett jobb är skickade toobe utförs över begärde en grupp av databaser, hello jobbet ”expanderas” Hej till underordnade jobb där varje utför hello körning mot en enskild databas i hello grupp.</span><span class="sxs-lookup"><span data-stu-id="89c59-196">When a job is submitted toobe executed across a group of databases, hello job “expands” hello into child jobs where each performs hello requested execution against a single database in hello group.</span></span> 

<span data-ttu-id="89c59-197">Det finns två typer av grupper som du kan skapa:</span><span class="sxs-lookup"><span data-stu-id="89c59-197">There are two types of groups that you can create:</span></span> 

* <span data-ttu-id="89c59-198">[Fragmentera kartan](sql-database-elastic-scale-shard-map-management.md) grupp: när ett jobb är skickade tootarget en Fragmentera karta, hello jobbet frågar hello Fragmentera kartan toodetermine aktuell uppsättning shards och skapar sedan underordnade jobb för varje Fragmentera hello Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="89c59-198">[Shard Map](sql-database-elastic-scale-shard-map-management.md) group: When a job is submitted tootarget a shard map, hello job queries hello shard map toodetermine its current set of shards, and then creates child jobs for each shard in hello shard map.</span></span>
* <span data-ttu-id="89c59-199">Samlingsgruppen för anpassade: en anpassad definierad uppsättning databaser.</span><span class="sxs-lookup"><span data-stu-id="89c59-199">Custom Collection group: A custom defined set of databases.</span></span> <span data-ttu-id="89c59-200">När ett jobb riktar sig till en anpassad samling, skapar den underordnade jobb för varje databas för närvarande i hello anpassad samling.</span><span class="sxs-lookup"><span data-stu-id="89c59-200">When a job targets a custom collection, it creates child jobs for each database currently in hello custom collection.</span></span>

## <a name="tooset-hello-elastic-database-jobs-connection"></a><span data-ttu-id="89c59-201">tooset hello elastiska jobb databasanslutning</span><span class="sxs-lookup"><span data-stu-id="89c59-201">tooset hello Elastic Database jobs connection</span></span>
<span data-ttu-id="89c59-202">En anslutning måste toobe set toohello jobb *databasen* tidigare toousing hello jobb API: er.</span><span class="sxs-lookup"><span data-stu-id="89c59-202">A connection needs toobe set toohello jobs *control database* prior toousing hello jobs APIs.</span></span> <span data-ttu-id="89c59-203">Kör denna cmdlet utlöser en autentiseringsuppgift fönstret toopop in begär hello användarnamn och lösenord som skapas när du installerar elastisk databas jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-203">Running this cmdlet triggers a credential window toopop up requesting hello user name and password created when installing Elastic Database jobs.</span></span> <span data-ttu-id="89c59-204">Alla exemplen i det här avsnittet förutsätter att det här första steget redan har utförts.</span><span class="sxs-lookup"><span data-stu-id="89c59-204">All examples provided within this topic assume that this first step has already been performed.</span></span>

<span data-ttu-id="89c59-205">Öppna en anslutning toohello elastisk databas jobb:</span><span class="sxs-lookup"><span data-stu-id="89c59-205">Open a connection toohello Elastic Database jobs:</span></span>

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-hello-elastic-database-jobs"></a><span data-ttu-id="89c59-206">Krypterade autentiseringsuppgifter i jobb för hello elastisk databas</span><span class="sxs-lookup"><span data-stu-id="89c59-206">Encrypted credentials within hello Elastic Database jobs</span></span>
<span data-ttu-id="89c59-207">Databasautentiseringsuppgifter kan infogas i hello jobb *databasen* med dess lösenord krypteras.</span><span class="sxs-lookup"><span data-stu-id="89c59-207">Database credentials can be inserted into hello jobs *control database*  with its password encrypted.</span></span> <span data-ttu-id="89c59-208">Det är nödvändigt toostore autentiseringsuppgifter tooenable jobb toobe körs vid ett senare tillfälle (med jobbscheman).</span><span class="sxs-lookup"><span data-stu-id="89c59-208">It is necessary toostore credentials tooenable jobs toobe executed at a later time, (using job schedules).</span></span>

<span data-ttu-id="89c59-209">Kryptering fungerar via ett certifikat som skapats som en del av hello installationsskript.</span><span class="sxs-lookup"><span data-stu-id="89c59-209">Encryption works through a certificate created as part of hello installation script.</span></span> <span data-ttu-id="89c59-210">hello installationsskriptet skapar och överföringar hello certifikatet till hello Azure Cloud Service för dekryptering av hello lagras krypterade lösenord.</span><span class="sxs-lookup"><span data-stu-id="89c59-210">hello installation script creates and uploads hello certificate into hello Azure Cloud Service for decryption of hello stored encrypted passwords.</span></span> <span data-ttu-id="89c59-211">hello Azure Cloud Service senare lagrar hello offentlig nyckel i hello jobb *databasen* som aktiverar hello PowerShell API eller klassiska Azure-portalen gränssnittet tooencrypt angivna lösenord utan hello certifikat toobe har installerats lokalt.</span><span class="sxs-lookup"><span data-stu-id="89c59-211">hello Azure Cloud Service later stores hello public key within hello jobs *control database* which enables hello PowerShell API or Azure Classic Portal interface tooencrypt a provided password without requiring hello certificate toobe locally installed.</span></span>

<span data-ttu-id="89c59-212">hello autentiseringsuppgifter lösenord är krypterad och säker från användare med läsåtkomst tooElastic jobb databasobjekt.</span><span class="sxs-lookup"><span data-stu-id="89c59-212">hello credential passwords are encrypted and secure from users with read-only access tooElastic Database jobs objects.</span></span> <span data-ttu-id="89c59-213">Men det är möjligt för en obehörig användare med läs-/ skrivåtkomst tooElastic databasen jobb objekt tooextract ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="89c59-213">But it is possible for a malicious user with read-write access tooElastic Database Jobs objects tooextract a password.</span></span> <span data-ttu-id="89c59-214">Autentiseringsuppgifterna är utformad toobe återanvänds över jobb körningar.</span><span class="sxs-lookup"><span data-stu-id="89c59-214">Credentials are designed toobe reused across job executions.</span></span> <span data-ttu-id="89c59-215">Autentiseringsuppgifter skickas tootarget databaser när anslutningar upprättas.</span><span class="sxs-lookup"><span data-stu-id="89c59-215">Credentials are passed tootarget databases when establishing connections.</span></span> <span data-ttu-id="89c59-216">Det finns för närvarande inga begränsningar för hello måldatabaserna används för varje autentiseringsuppgifter, obehörig användare kan lägga till ett mål för databasen för en databas under hello angripare kontroll.</span><span class="sxs-lookup"><span data-stu-id="89c59-216">There are currently no restrictions on hello target databases used for each credential, malicious user could add a database target for a database under hello malicious user's control.</span></span> <span data-ttu-id="89c59-217">hello användare kan sedan starta ett jobb som målobjekt för den här databasen toogain hello-referensens lösenord.</span><span class="sxs-lookup"><span data-stu-id="89c59-217">hello user could subsequently start a job targeting this database toogain hello credential's password.</span></span>

<span data-ttu-id="89c59-218">Rekommenderade säkerhetsmetoder för elastisk databas jobb är:</span><span class="sxs-lookup"><span data-stu-id="89c59-218">Security best practices for Elastic Database jobs include:</span></span>

* <span data-ttu-id="89c59-219">Begränsa användning av hello-API: er tootrusted enskilda användare.</span><span class="sxs-lookup"><span data-stu-id="89c59-219">Limit usage of hello APIs tootrusted individuals.</span></span>
* <span data-ttu-id="89c59-220">Autentiseringsuppgifter ska ha hello minst privilegier krävs tooperform hello projektaktivitet.</span><span class="sxs-lookup"><span data-stu-id="89c59-220">Credentials should have hello least privileges necessary tooperform hello job task.</span></span>  <span data-ttu-id="89c59-221">Mer information kan ses i detta [auktoriserings- och behörigheter](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN-artikel.</span><span class="sxs-lookup"><span data-stu-id="89c59-221">More information can be seen within this [Authorization and Permissions](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN article.</span></span>

### <a name="toocreate-an-encrypted-credential-for-job-execution-across-databases"></a><span data-ttu-id="89c59-222">toocreate en krypterade autentiseringsuppgifter för jobbkörningen över databaser</span><span class="sxs-lookup"><span data-stu-id="89c59-222">toocreate an encrypted credential for job execution across databases</span></span>
<span data-ttu-id="89c59-223">toocreate en ny krypterade autentiseringsuppgifter, hello [ **cmdlet Get-Credential** ](https://technet.microsoft.com/library/hh849815.aspx) uppmanas att ange ett användarnamn och lösenord som kan skickas toohello [ **ny AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).</span><span class="sxs-lookup"><span data-stu-id="89c59-223">toocreate a new encrypted credential, hello [**Get-Credential cmdlet**](https://technet.microsoft.com/library/hh849815.aspx) prompts for a user name and password that can be passed toohello [**New-AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).</span></span>

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="tooupdate-credentials"></a><span data-ttu-id="89c59-224">tooupdate autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="89c59-224">tooupdate credentials</span></span>
<span data-ttu-id="89c59-225">När du ändrar lösenord, använda hello [ **cmdlet Set-AzureSqlJobCredential** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) och ange hello **CredentialName** parameter.</span><span class="sxs-lookup"><span data-stu-id="89c59-225">When passwords change, use hello [**Set-AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) and set hello **CredentialName** parameter.</span></span>

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="toodefine-an-elastic-database-shard-map-target"></a><span data-ttu-id="89c59-226">toodefine ett mål för elastisk databas Fragmentera karta</span><span class="sxs-lookup"><span data-stu-id="89c59-226">toodefine an Elastic Database shard map target</span></span>
<span data-ttu-id="89c59-227">tooexecute ett jobb för alla databaser i en Fragmentera (skapat med hjälp av [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md)), Använd en Fragmentera som mål för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="89c59-227">tooexecute a job against all databases in a shard set (created using [Elastic Database client library](sql-database-elastic-database-client-library.md)), use a shard map as hello database target.</span></span> <span data-ttu-id="89c59-228">Det här exemplet kräver ett delat program som skapats med hjälp av hello klientbibliotek för elastisk databas.</span><span class="sxs-lookup"><span data-stu-id="89c59-228">This example requires a sharded application created using hello Elastic Database client library.</span></span> <span data-ttu-id="89c59-229">Se [komma igång med elastisk databas verktyg exempel](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="89c59-229">See [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

<span data-ttu-id="89c59-230">hello Fragmentera kartan manager-databasen måste anges som ett mål för databasen och sedan hello specifika Fragmentera mappning måste anges som mål.</span><span class="sxs-lookup"><span data-stu-id="89c59-230">hello shard map manager database must be set as a database target and then hello specific shard map must be specified as a target.</span></span>

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="89c59-231">Skapa ett T-SQL-skript för körning på databaser</span><span class="sxs-lookup"><span data-stu-id="89c59-231">Create a T-SQL Script for execution across databases</span></span>
<span data-ttu-id="89c59-232">När du skapar T-SQL-skript för körning, rekommenderas toobuild dem toobe [idempotent](https://en.wikipedia.org/wiki/Idempotence) och motståndskraftiga mot fel.</span><span class="sxs-lookup"><span data-stu-id="89c59-232">When creating T-SQL scripts for execution, it is highly recommended toobuild them toobe [idempotent](https://en.wikipedia.org/wiki/Idempotence) and resilient against failures.</span></span> <span data-ttu-id="89c59-233">Den elastiska databasen jobb kommer att försöka körningen av ett skript när körningen påträffar ett fel, oavsett hello klassificering av hello-fel.</span><span class="sxs-lookup"><span data-stu-id="89c59-233">Elastic Database jobs will retry execution of a script whenever execution encounters a failure, regardless of hello classification of hello failure.</span></span>

<span data-ttu-id="89c59-234">Använd hello [ **cmdlet New-AzureSqlJobContent** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) toocreate och sparat ett skript för körning och ange hello **- ContentName** och **- CommandText**parametrar.</span><span class="sxs-lookup"><span data-stu-id="89c59-234">Use hello [**New-AzureSqlJobContent cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) toocreate and save a script for execution and set hello **-ContentName** and **-CommandText** parameters.</span></span>

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a><span data-ttu-id="89c59-235">Skapa ett nytt skript från en fil</span><span class="sxs-lookup"><span data-stu-id="89c59-235">Create a new script from a file</span></span>
<span data-ttu-id="89c59-236">Om hello T-SQL-skript har definierats i en fil, använder du skriptet tooimport hello:</span><span class="sxs-lookup"><span data-stu-id="89c59-236">If hello T-SQL script is defined within a file, use this tooimport hello script:</span></span>

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path tooSQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="tooupdate-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="89c59-237">tooupdate T-SQL-skript för körning över databaser</span><span class="sxs-lookup"><span data-stu-id="89c59-237">tooupdate a T-SQL script for execution across databases</span></span>
<span data-ttu-id="89c59-238">Den här PowerShell-skript uppdateringar hello T-SQL kommandotexten för ett befintligt skript.</span><span class="sxs-lookup"><span data-stu-id="89c59-238">This PowerShell script updates hello T-SQL command text for an existing script.</span></span>

<span data-ttu-id="89c59-239">Ange hello följande variabler tooreflect hello önskad skript definition toobe set:</span><span class="sxs-lookup"><span data-stu-id="89c59-239">Set hello following variables tooreflect hello desired script definition toobe set:</span></span>

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column tooTestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="tooupdate-hello-definition-tooan-existing-script"></a><span data-ttu-id="89c59-240">tooupdate hello definition tooan befintligt skript</span><span class="sxs-lookup"><span data-stu-id="89c59-240">tooupdate hello definition tooan existing script</span></span>
    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="toocreate-a-job-tooexecute-a-script-across-a-shard-map"></a><span data-ttu-id="89c59-241">toocreate jobbet-tooexecute ett skript i en Fragmentera karta</span><span class="sxs-lookup"><span data-stu-id="89c59-241">toocreate a job tooexecute a script across a shard map</span></span>
<span data-ttu-id="89c59-242">Detta PowerShell-skript startar ett jobb för körning av ett skript på varje Fragmentera i en elastisk skalbarhet Fragmentera mappning.</span><span class="sxs-lookup"><span data-stu-id="89c59-242">This PowerShell script starts a job for execution of a script across each shard in an Elastic Scale shard map.</span></span>

<span data-ttu-id="89c59-243">Ange hello följande variabler tooreflect hello önskad skript och mål:</span><span class="sxs-lookup"><span data-stu-id="89c59-243">Set hello following variables tooreflect hello desired script and target:</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="tooexecute-a-job"></a><span data-ttu-id="89c59-244">tooexecute ett jobb</span><span class="sxs-lookup"><span data-stu-id="89c59-244">tooexecute a job</span></span>
<span data-ttu-id="89c59-245">Det här PowerShell-skriptet körs ett befintligt jobb:</span><span class="sxs-lookup"><span data-stu-id="89c59-245">This PowerShell script executes an existing job:</span></span>

<span data-ttu-id="89c59-246">Uppdatera hello variabeln tooreflect hello önskad jobbet namn toohave utförs följande:</span><span class="sxs-lookup"><span data-stu-id="89c59-246">Update hello following variable tooreflect hello desired job name toohave executed:</span></span>

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution

## <a name="tooretrieve-hello-state-of-a-single-job-execution"></a><span data-ttu-id="89c59-247">tooretrieve hello tillståndet för en enskild jobbkörningen</span><span class="sxs-lookup"><span data-stu-id="89c59-247">tooretrieve hello state of a single job execution</span></span>
<span data-ttu-id="89c59-248">Använd hello [ **cmdlet Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) och ange hello **JobExecutionId** parametern tooview hello tillståndet för jobbkörningen.</span><span class="sxs-lookup"><span data-stu-id="89c59-248">Use hello [**Get-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) and set hello **JobExecutionId** parameter tooview hello state of job execution.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

<span data-ttu-id="89c59-249">Använd hello samma **Get-AzureSqlJobExecution** med hello **IncludeChildren** parametern tooview hello tillstånd för underordnad jobbet körningar, nämligen hello specifikt tillstånd för varje jobbkörningen mot varje databasen som mål för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-249">Use hello same **Get-AzureSqlJobExecution** cmdlet with hello **IncludeChildren** parameter tooview hello state of child job executions, namely hello specific state for each job execution against each database targeted by hello job.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="tooview-hello-state-across-multiple-job-executions"></a><span data-ttu-id="89c59-250">tooview hello tillstånd över flera jobb körningar</span><span class="sxs-lookup"><span data-stu-id="89c59-250">tooview hello state across multiple job executions</span></span>
<span data-ttu-id="89c59-251">Hej [ **cmdlet Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) har flera valfria parametrar som kan använda toodisplay flera jobb körningar, filtreras via hello angivna parametrar.</span><span class="sxs-lookup"><span data-stu-id="89c59-251">hello [**Get-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljob) has multiple optional parameters that can be used toodisplay multiple job executions, filtered through hello provided parameters.</span></span> <span data-ttu-id="89c59-252">hello nedan visar några av hello möjliga sätt toouse Get-AzureSqlJobExecution:</span><span class="sxs-lookup"><span data-stu-id="89c59-252">hello following demonstrates some of hello possible ways toouse Get-AzureSqlJobExecution:</span></span>

<span data-ttu-id="89c59-253">Hämta alla aktiva översta nivå jobbet körningar:</span><span class="sxs-lookup"><span data-stu-id="89c59-253">Retrieve all active top level job executions:</span></span>

    Get-AzureSqlJobExecution

<span data-ttu-id="89c59-254">Hämta alla jobb för övre nivå körningar, inklusive inaktiva jobb körningar:</span><span class="sxs-lookup"><span data-stu-id="89c59-254">Retrieve all top level job executions, including inactive job executions:</span></span>

    Get-AzureSqlJobExecution -IncludeInactive

<span data-ttu-id="89c59-255">Hämta alla underordnade jobbet körningar av en tillhandahållna jobbet körnings-ID, inklusive inaktiva jobb körningar:</span><span class="sxs-lookup"><span data-stu-id="89c59-255">Retrieve all child job executions of a provided job execution ID, including inactive job executions:</span></span>

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren

<span data-ttu-id="89c59-256">Hämta alla jobb körningar som skapats med hjälp av ett schema / jobb tillsammans med inaktiva jobb:</span><span class="sxs-lookup"><span data-stu-id="89c59-256">Retrieve all job executions created using a schedule / job combination, including inactive jobs:</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

<span data-ttu-id="89c59-257">Hämta alla jobb som mål för en angiven Fragmentera karta, inklusive inaktiva jobb:</span><span class="sxs-lookup"><span data-stu-id="89c59-257">Retrieve all jobs targeting a specified shard map, including inactive jobs:</span></span>

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

<span data-ttu-id="89c59-258">Hämta alla jobb som mål för en angiven anpassad samling, inklusive inaktiva jobb:</span><span class="sxs-lookup"><span data-stu-id="89c59-258">Retrieve all jobs targeting a specified custom collection, including inactive jobs:</span></span>

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

<span data-ttu-id="89c59-259">Hämta hello lista över jobb uppgiften körningar inom en specifik jobbkörningen:</span><span class="sxs-lookup"><span data-stu-id="89c59-259">Retrieve hello list of job task executions within a specific job execution:</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

<span data-ttu-id="89c59-260">Hämta jobb aktivitetsinformation körning:</span><span class="sxs-lookup"><span data-stu-id="89c59-260">Retrieve job task execution details:</span></span>

<span data-ttu-id="89c59-261">hello följande PowerShell-skript kan vara används tooview hello information om ett jobb för körning av aktiviteten, vilket är särskilt användbart när körningen felsökningsändamål.</span><span class="sxs-lookup"><span data-stu-id="89c59-261">hello following PowerShell script can be used tooview hello details of a job task execution, which is particularly useful when debugging execution failures.</span></span>

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="tooretrieve-failures-within-job-task-executions"></a><span data-ttu-id="89c59-262">tooretrieve fel i jobb uppgiften körningar</span><span class="sxs-lookup"><span data-stu-id="89c59-262">tooretrieve failures within job task executions</span></span>
<span data-ttu-id="89c59-263">Hej **JobTaskExecution objekt** innehåller en egenskap för hello livscykeln för hello uppgiften tillsammans med en meddelandeegenskap.</span><span class="sxs-lookup"><span data-stu-id="89c59-263">hello **JobTaskExecution object** includes a property for hello lifecycle of hello task along with a message property.</span></span> <span data-ttu-id="89c59-264">Om ett jobb för körning av aktiviteten misslyckades hello livscykel kommer att ange egenskapen för*misslyckades* och kommer att anges hello meddelandeegenskap toohello resulterande Undantagsmeddelande och dess stacken.</span><span class="sxs-lookup"><span data-stu-id="89c59-264">If a job task execution failed, hello lifecycle property will be set too*Failed* and hello message property will be set toohello resulting exception message and its stack.</span></span> <span data-ttu-id="89c59-265">Om ett jobb misslyckades, är det viktigt tooview hello information om jobbuppgifter som misslyckades för ett visst jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-265">If a job did not succeed, it is important tooview hello details of job tasks that did not succeed for a given job.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="toowait-for-a-job-execution-toocomplete"></a><span data-ttu-id="89c59-266">toowait för en toocomplete för körning av jobbet</span><span class="sxs-lookup"><span data-stu-id="89c59-266">toowait for a job execution toocomplete</span></span>
<span data-ttu-id="89c59-267">hello följande PowerShell-skript kan vara används toowait för ett jobb uppgiften toocomplete:</span><span class="sxs-lookup"><span data-stu-id="89c59-267">hello following PowerShell script can be used toowait for a job task toocomplete:</span></span>

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a><span data-ttu-id="89c59-268">Skapa en princip för anpassad körning</span><span class="sxs-lookup"><span data-stu-id="89c59-268">Create a custom execution policy</span></span>
<span data-ttu-id="89c59-269">Den elastiska databasen jobb kan du skapa anpassade körningsprinciper som kan användas när du startar jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-269">Elastic Database jobs supports creating custom execution policies that can be applied when starting jobs.</span></span>

<span data-ttu-id="89c59-270">Körningsprinciper tillåter för närvarande för att definiera:</span><span class="sxs-lookup"><span data-stu-id="89c59-270">Execution policies currently allow for defining:</span></span>

* <span data-ttu-id="89c59-271">Namn: Identifierare för hello körningsprincipen.</span><span class="sxs-lookup"><span data-stu-id="89c59-271">Name: Identifier for hello execution policy.</span></span>
* <span data-ttu-id="89c59-272">Tidsgräns för jobb: Total tid innan ett jobb kommer att avbrytas av elastiska databasen jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-272">Job Timeout: Total time before a job will be canceled by Elastic Database Jobs.</span></span>
* <span data-ttu-id="89c59-273">Inledande återförsöksintervall: Intervall toowait innan nytt försök görs.</span><span class="sxs-lookup"><span data-stu-id="89c59-273">Initial Retry Interval: Interval toowait before first retry.</span></span>
* <span data-ttu-id="89c59-274">Maximal återförsöksintervall: Locket försök intervall toouse.</span><span class="sxs-lookup"><span data-stu-id="89c59-274">Maximum Retry Interval: Cap of retry intervals toouse.</span></span>
* <span data-ttu-id="89c59-275">Försök intervall Backoff värde: Värde används toocalculate hello nästa intervall mellan försök.</span><span class="sxs-lookup"><span data-stu-id="89c59-275">Retry Interval Backoff Coefficient: Coefficient used toocalculate hello next interval between retries.</span></span>  <span data-ttu-id="89c59-276">hello används följande formel: (första försök intervall) * Math.pow ((intervall Backoff värde) (antal nya försök) - 2).</span><span class="sxs-lookup"><span data-stu-id="89c59-276">hello following formula is used: (Initial Retry Interval) * Math.pow((Interval Backoff Coefficient), (Number of Retries) - 2).</span></span> 
* <span data-ttu-id="89c59-277">Maximalt antal försök: hello maximalt antal försök försök tooperform inom ett jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-277">Maximum Attempts: hello maximum number of retry attempts tooperform within a job.</span></span>

<span data-ttu-id="89c59-278">hello standard körningsprincipen använder hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="89c59-278">hello default execution policy uses hello following values:</span></span>

* <span data-ttu-id="89c59-279">Namn: Standardprincipen för körning</span><span class="sxs-lookup"><span data-stu-id="89c59-279">Name: Default execution policy</span></span>
* <span data-ttu-id="89c59-280">Tidsgräns för jobb: 1 vecka</span><span class="sxs-lookup"><span data-stu-id="89c59-280">Job Timeout: 1 week</span></span>
* <span data-ttu-id="89c59-281">Inledande återförsöksintervall: 100 millisekunder</span><span class="sxs-lookup"><span data-stu-id="89c59-281">Initial Retry Interval:  100 milliseconds</span></span>
* <span data-ttu-id="89c59-282">Maximal återförsöksintervall: 30 minuter</span><span class="sxs-lookup"><span data-stu-id="89c59-282">Maximum Retry Interval: 30 minutes</span></span>
* <span data-ttu-id="89c59-283">Försök intervall värde: 2</span><span class="sxs-lookup"><span data-stu-id="89c59-283">Retry Interval Coefficient: 2</span></span>
* <span data-ttu-id="89c59-284">Maximalt antal försök: 2 147 483 647</span><span class="sxs-lookup"><span data-stu-id="89c59-284">Maximum Attempts: 2,147,483,647</span></span>

<span data-ttu-id="89c59-285">Skapa hello önskad körningsprincipen:</span><span class="sxs-lookup"><span data-stu-id="89c59-285">Create hello desired execution policy:</span></span>

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a><span data-ttu-id="89c59-286">Uppdatera en anpassad körningsprincip</span><span class="sxs-lookup"><span data-stu-id="89c59-286">Update a custom execution policy</span></span>
<span data-ttu-id="89c59-287">Uppdatera hello önskad körning princip tooupdate:</span><span class="sxs-lookup"><span data-stu-id="89c59-287">Update hello desired execution policy tooupdate:</span></span>

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy

## <a name="cancel-a-job"></a><span data-ttu-id="89c59-288">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="89c59-288">Cancel a job</span></span>
<span data-ttu-id="89c59-289">Den elastiska databasen jobb stöder förfrågningar om annullering av jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-289">Elastic Database Jobs supports cancellation requests of jobs.</span></span>  <span data-ttu-id="89c59-290">Om den elastiska databasen jobb upptäcker en begäran om att avbryta ett jobb som körs, försöker den toostop hello jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-290">If Elastic Database Jobs detects a cancellation request for a job currently being executed, it will attempt toostop hello job.</span></span>

<span data-ttu-id="89c59-291">Det finns två olika sätt att elastiska databasen jobb kan utföra en annullering:</span><span class="sxs-lookup"><span data-stu-id="89c59-291">There are two different ways that Elastic Database Jobs can perform a cancellation:</span></span>

1. <span data-ttu-id="89c59-292">Avbryt aktiviteter som körs: om en annullering identifieras när en aktivitet körs för närvarande en annullering görs inom hello aspekt av hello aktivitet som körs.</span><span class="sxs-lookup"><span data-stu-id="89c59-292">Cancel currently executing tasks: If a cancellation is detected while a task is currently running, a cancellation will be attempted within hello currently executing aspect of hello task.</span></span>  <span data-ttu-id="89c59-293">Exempel: om det finns en tidskrävande fråga som för närvarande utförs när en annullering görs, är en försök toocancel hello-fråga.</span><span class="sxs-lookup"><span data-stu-id="89c59-293">For example: If there is a long running query currently being performed when a cancellation is attempted, there will be an attempt toocancel hello query.</span></span>
2. <span data-ttu-id="89c59-294">Om du avbryter återförsök för aktiviteten: om en annullering identifieras av hello kontrollen tråd innan en uppgift startas för körning hello kontrollen tråd ska undvika starta hello aktivitet och deklarera hello begäran som avbruten.</span><span class="sxs-lookup"><span data-stu-id="89c59-294">Canceling task retries: If a cancellation is detected by hello control thread before a task is launched for execution, hello control thread will avoid launching hello task and declare hello request as canceled.</span></span>

<span data-ttu-id="89c59-295">Om det krävs en annullering av jobbet för överordnade jobb respekteras hello avbrottsbegäran för hello överordnade jobb och alla dess underordnade jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-295">If a job cancellation is requested for a parent job, hello cancellation request will be honored for hello parent job and for all of its child jobs.</span></span>

<span data-ttu-id="89c59-296">toosubmit en begäran om att avbryta, använda hello [ **cmdlet Stop AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) och ange hello **JobExecutionId** parameter.</span><span class="sxs-lookup"><span data-stu-id="89c59-296">toosubmit a cancellation request, use hello [**Stop-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) and set hello **JobExecutionId** parameter.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="toodelete-a-job-and-job-history-asynchronously"></a><span data-ttu-id="89c59-297">toodelete ett jobb och Jobbhistorik asynkront</span><span class="sxs-lookup"><span data-stu-id="89c59-297">toodelete a job and job history asynchronously</span></span>
<span data-ttu-id="89c59-298">Den elastiska databasen jobb stöder asynkrona borttagning av jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-298">Elastic Database jobs supports asynchronous deletion of jobs.</span></span> <span data-ttu-id="89c59-299">Ett jobb kan vara markerad för borttagning och hello tas bort hello jobb och alla dess jobbhistorik när alla jobb körningar har slutförts för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-299">A job can be marked for deletion and hello system will delete hello job and all its job history after all job executions have completed for hello job.</span></span> <span data-ttu-id="89c59-300">hello system Avbryt inte automatiskt aktiva jobb körningar.</span><span class="sxs-lookup"><span data-stu-id="89c59-300">hello system will not automatically cancel active job executions.</span></span>  

<span data-ttu-id="89c59-301">Anropa [ **stoppa AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) toocancel aktiva jobb körningar.</span><span class="sxs-lookup"><span data-stu-id="89c59-301">Invoke [**Stop-AzureSqlJobExecution**](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) toocancel active job executions.</span></span>

<span data-ttu-id="89c59-302">tootrigger jobbet borttagning, Använd hello [ **cmdlet Remove-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/remove-azuresqljob) och ange hello **jobbnamn** parameter.</span><span class="sxs-lookup"><span data-stu-id="89c59-302">tootrigger job deletion, use hello [**Remove-AzureSqlJob cmdlet**](/powershell/module/elasticdatabasejobs/remove-azuresqljob) and set hello **JobName** parameter.</span></span>

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName

## <a name="toocreate-a-custom-database-target"></a><span data-ttu-id="89c59-303">toocreate mål för en anpassad databas</span><span class="sxs-lookup"><span data-stu-id="89c59-303">toocreate a custom database target</span></span>
<span data-ttu-id="89c59-304">Du kan definiera anpassade databasen mål för att köra en direkt eller som ska ingå i en anpassad databas-grupp.</span><span class="sxs-lookup"><span data-stu-id="89c59-304">You can define custom database targets either for direct execution or for inclusion within a custom database group.</span></span> <span data-ttu-id="89c59-305">Till exempel eftersom **elastiska pooler** är stöds inte än direkt med hjälp av PowerShell APIs, kan du skapa en anpassad databas mål och anpassad databas samling mål som omfattar alla hello-databaser i hello pool.</span><span class="sxs-lookup"><span data-stu-id="89c59-305">For example, because **elastic pools** are not yet directly supported using PowerShell APIs, you can create a custom database target and custom database collection target which encompasses all hello databases in hello pool.</span></span>

<span data-ttu-id="89c59-306">Ange följande variabler tooreflect hello önskad databasinformation hello:</span><span class="sxs-lookup"><span data-stu-id="89c59-306">Set hello following variables tooreflect hello desired database information:</span></span>

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="toocreate-a-custom-database-collection-target"></a><span data-ttu-id="89c59-307">toocreate en anpassad databas samling mål</span><span class="sxs-lookup"><span data-stu-id="89c59-307">toocreate a custom database collection target</span></span>
<span data-ttu-id="89c59-308">Använd hello [ **ny AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet toodefine en anpassad samling mål tooenable databaskörning över flera definierade databasen mål.</span><span class="sxs-lookup"><span data-stu-id="89c59-308">Use hello [**New-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet toodefine a custom database collection target tooenable execution across multiple defined database targets.</span></span> <span data-ttu-id="89c59-309">När du har skapat en databasgrupp kan databaser vara associerat med hello anpassad samling mål.</span><span class="sxs-lookup"><span data-stu-id="89c59-309">After creating a database group, databases can be associated with hello custom collection target.</span></span>

<span data-ttu-id="89c59-310">Ange hello följande variabler tooreflect hello önskade anpassade samlingar mål konfiguration:</span><span class="sxs-lookup"><span data-stu-id="89c59-310">Set hello following variables tooreflect hello desired custom collection target configuration:</span></span>

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="tooadd-databases-tooa-custom-database-collection-target"></a><span data-ttu-id="89c59-311">tooadd databaser tooa anpassad databas samling mål</span><span class="sxs-lookup"><span data-stu-id="89c59-311">tooadd databases tooa custom database collection target</span></span>
<span data-ttu-id="89c59-312">tooadd en databas tooa specifika anpassad samling använda hello [ **Lägg till AzureSqlJobChildTarget** ](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="89c59-312">tooadd a database tooa specific custom collection use hello [**Add-AzureSqlJobChildTarget**](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) cmdlet.</span></span>

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-hello-databases-within-a-custom-database-collection-target"></a><span data-ttu-id="89c59-313">Granska hello databaser i en anpassad databas samling målet</span><span class="sxs-lookup"><span data-stu-id="89c59-313">Review hello databases within a custom database collection target</span></span>
<span data-ttu-id="89c59-314">Använd hello [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet tooretrieve hello underordnade databaser i en anpassad databas samling målet.</span><span class="sxs-lookup"><span data-stu-id="89c59-314">Use hello [**Get-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet tooretrieve hello child databases within a custom database collection target.</span></span> 

    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-tooexecute-a-script-across-a-custom-database-collection-target"></a><span data-ttu-id="89c59-315">Skapa ett skript i ett jobb tooexecute över en anpassad databas samling mål</span><span class="sxs-lookup"><span data-stu-id="89c59-315">Create a job tooexecute a script across a custom database collection target</span></span>
<span data-ttu-id="89c59-316">Använd hello [ **ny AzureSqlJob** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) cmdlet toocreate ett jobb mot en grupp databaser som definieras av en anpassad databas samling mål.</span><span class="sxs-lookup"><span data-stu-id="89c59-316">Use hello [**New-AzureSqlJob**](/powershell/module/elasticdatabasejobs/new-azuresqljob) cmdlet toocreate a job against a group of databases defined by a custom database collection target.</span></span> <span data-ttu-id="89c59-317">Den elastiska databasen jobb kommer att expandera hello jobbet till flera underordnade jobb varje motsvarande tooa-databas som är associerade med hello anpassad databas samling mål och se till att hello skriptet körs mot varje databas.</span><span class="sxs-lookup"><span data-stu-id="89c59-317">Elastic Database jobs will expand hello job into multiple child jobs each corresponding tooa database associated with hello custom database collection target and ensure that hello script is executed against each database.</span></span> <span data-ttu-id="89c59-318">Igen, är det viktigt att skripten är idempotent toobe flexibel tooretries.</span><span class="sxs-lookup"><span data-stu-id="89c59-318">Again, it is important that scripts are idempotent toobe resilient tooretries.</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a><span data-ttu-id="89c59-319">Insamling av data över databaser</span><span class="sxs-lookup"><span data-stu-id="89c59-319">Data collection across databases</span></span>
<span data-ttu-id="89c59-320">Du kan använda en jobbet tooexecute en fråga i en grupp med databaser och skicka hello tooa specifika resultattabellen.</span><span class="sxs-lookup"><span data-stu-id="89c59-320">You can use a job tooexecute a query across a group of databases and send hello results tooa specific table.</span></span> <span data-ttu-id="89c59-321">hello tabellen kan efterfrågas efter hello fakta toosee hello frågans resultat från varje databas.</span><span class="sxs-lookup"><span data-stu-id="89c59-321">hello table can be queried after hello fact toosee hello query’s results from each database.</span></span> <span data-ttu-id="89c59-322">Detta ger en asynkron metod tooexecute en fråga över flera databaser.</span><span class="sxs-lookup"><span data-stu-id="89c59-322">This provides an asynchronous method tooexecute a query across many databases.</span></span> <span data-ttu-id="89c59-323">Misslyckade försök hanteras automatiskt via återförsök.</span><span class="sxs-lookup"><span data-stu-id="89c59-323">Failed attempts are handled automatically via retries.</span></span>

<span data-ttu-id="89c59-324">hello angivna måltabellen skapas automatiskt om det inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="89c59-324">hello specified destination table will be automatically created if it does not yet exist.</span></span> <span data-ttu-id="89c59-325">hello ny tabell matchar hello schemat för hello returnerade en resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="89c59-325">hello new table matches hello schema of hello returned result set.</span></span> <span data-ttu-id="89c59-326">Om ett skript som returnerar flera resultatmängder, skickas endast elastisk databas jobb hello första toohello måltabellen.</span><span class="sxs-lookup"><span data-stu-id="89c59-326">If a script returns multiple result sets, Elastic Database jobs will only send hello first toohello destination table.</span></span>

<span data-ttu-id="89c59-327">hello följande PowerShell-skript körs ett skript och samlar in resultaten i en angiven tabell.</span><span class="sxs-lookup"><span data-stu-id="89c59-327">hello following PowerShell script executes a script and collects its results into a specified table.</span></span> <span data-ttu-id="89c59-328">Det här skriptet förutsätter att ett T-SQL-skript har skapats som matar ut en enda resultatmängd och ett mål för insamling av anpassad databas har skapats.</span><span class="sxs-lookup"><span data-stu-id="89c59-328">This script assumes that a T-SQL script has been created which outputs a single result set and that a custom database collection target has been created.</span></span>

<span data-ttu-id="89c59-329">Det här skriptet använder hello [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="89c59-329">This script uses hello [**Get-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet.</span></span> <span data-ttu-id="89c59-330">Ange hello parametrar för skriptet, autentiseringsuppgifter och körning av mål:</span><span class="sxs-lookup"><span data-stu-id="89c59-330">Set hello parameters for script, credentials, and execution target:</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="toocreate-and-start-a-job-for-data-collection-scenarios"></a><span data-ttu-id="89c59-331">toocreate och starta ett jobb för scenarion för insamling av data</span><span class="sxs-lookup"><span data-stu-id="89c59-331">toocreate and start a job for data collection scenarios</span></span>
<span data-ttu-id="89c59-332">Det här skriptet använder hello [ **Start AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="89c59-332">This script uses hello [**Start-AzureSqlJobExecution**](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) cmdlet.</span></span>

    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="tooschedule-a-job-execution-trigger"></a><span data-ttu-id="89c59-333">tooschedule en utlösare för körning av jobbet</span><span class="sxs-lookup"><span data-stu-id="89c59-333">tooschedule a job execution trigger</span></span>
<span data-ttu-id="89c59-334">hello följande PowerShell-skript kan vara används toocreate ett återkommande schema.</span><span class="sxs-lookup"><span data-stu-id="89c59-334">hello following PowerShell script can be used toocreate a recurring schedule.</span></span> <span data-ttu-id="89c59-335">Det här skriptet använder ett minuters intervall, men [ **ny AzureSqlJobSchedule** ](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) stöder också - DayInterval, - HourInterval, - MonthInterval, och WeekInterval - parametrar.</span><span class="sxs-lookup"><span data-stu-id="89c59-335">This script uses a minute interval, but [**New-AzureSqlJobSchedule**](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) also supports -DayInterval, -HourInterval, -MonthInterval, and -WeekInterval parameters.</span></span> <span data-ttu-id="89c59-336">Scheman som kör en gång kan skapas med skicka - görs.</span><span class="sxs-lookup"><span data-stu-id="89c59-336">Schedules that execute only once can be created by passing -OneTime.</span></span>

<span data-ttu-id="89c59-337">Skapa ett nytt schema:</span><span class="sxs-lookup"><span data-stu-id="89c59-337">Create a new schedule:</span></span>

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="tootrigger-a-job-executed-on-a-time-schedule"></a><span data-ttu-id="89c59-338">tootrigger ett jobb som körs på en tidsplan</span><span class="sxs-lookup"><span data-stu-id="89c59-338">tootrigger a job executed on a time schedule</span></span>
<span data-ttu-id="89c59-339">En utlösare för jobb kan vara definierade toohave ett jobb körs bl.a tooa tid schema.</span><span class="sxs-lookup"><span data-stu-id="89c59-339">A job trigger can be defined toohave a job executed according tooa time schedule.</span></span> <span data-ttu-id="89c59-340">hello följande PowerShell-skript kan vara används toocreate en utlösare för jobbet.</span><span class="sxs-lookup"><span data-stu-id="89c59-340">hello following PowerShell script can be used toocreate a job trigger.</span></span>

<span data-ttu-id="89c59-341">Använd [ny AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) och ange hello följande variabler toocorrespond toohello önskade jobbet och schema:</span><span class="sxs-lookup"><span data-stu-id="89c59-341">Use [New-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) and set hello following variables toocorrespond toohello desired job and schedule:</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    -JobName $jobName
    Write-Output $jobTrigger

### <a name="tooremove-a-scheduled-association-toostop-job-from-executing-on-schedule"></a><span data-ttu-id="89c59-342">tooremove ett schemalagda association toostop jobb körs enligt schema</span><span class="sxs-lookup"><span data-stu-id="89c59-342">tooremove a scheduled association toostop job from executing on schedule</span></span>
<span data-ttu-id="89c59-343">toodiscontinue igen jobbkörningen via jobb utlösare hello jobbet utlösare kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="89c59-343">toodiscontinue reoccurring job execution through a job trigger, hello job trigger can be removed.</span></span> <span data-ttu-id="89c59-344">Ta bort ett jobb utlösaren toostop ett jobb från utförs bl.a tooa schema med hello [ **cmdlet Remove-AzureSqlJobTrigger**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).</span><span class="sxs-lookup"><span data-stu-id="89c59-344">Remove a job trigger toostop a job from being executed according tooa schedule using hello [**Remove-AzureSqlJobTrigger cmdlet**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-tooa-time-schedule"></a><span data-ttu-id="89c59-345">Hämta jobbschemat utlösare bundna tooa tid</span><span class="sxs-lookup"><span data-stu-id="89c59-345">Retrieve job triggers bound tooa time schedule</span></span>
<span data-ttu-id="89c59-346">hello följande PowerShell-skript kan använda tooobtain och visa hello utlösare registrerade tooa viss tid jobbschemat.</span><span class="sxs-lookup"><span data-stu-id="89c59-346">hello following PowerShell script can be used tooobtain and display hello job triggers registered tooa particular time schedule.</span></span>

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="tooretrieve-job-triggers-bound-tooa-job"></a><span data-ttu-id="89c59-347">tooretrieve jobb utlösare bundna tooa jobb</span><span class="sxs-lookup"><span data-stu-id="89c59-347">tooretrieve job triggers bound tooa job</span></span>
<span data-ttu-id="89c59-348">Använd [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) tooobtain och visa scheman som innehåller ett registrerade jobb.</span><span class="sxs-lookup"><span data-stu-id="89c59-348">Use [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) tooobtain and display schedules containing a registered job.</span></span>

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="toocreate-a-data-tier-application-dacpac-for-execution-across-databases"></a><span data-ttu-id="89c59-349">toocreate datanivå-program (DACPAC) för körning över databaser</span><span class="sxs-lookup"><span data-stu-id="89c59-349">toocreate a data-tier application (DACPAC) for execution across databases</span></span>
<span data-ttu-id="89c59-350">toocreate en DACPAC finns [-datanivåprogram](https://msdn.microsoft.com/library/ee210546.aspx).</span><span class="sxs-lookup"><span data-stu-id="89c59-350">toocreate a DACPAC, see [Data-Tier applications](https://msdn.microsoft.com/library/ee210546.aspx).</span></span> <span data-ttu-id="89c59-351">toodeploy DACPAC, använda hello [cmdlet New-AzureSqlJobContent](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent).</span><span class="sxs-lookup"><span data-stu-id="89c59-351">toodeploy a DACPAC, use hello [New-AzureSqlJobContent cmdlet](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent).</span></span> <span data-ttu-id="89c59-352">Hej DACPAC måste vara tillgänglig toohello tjänst.</span><span class="sxs-lookup"><span data-stu-id="89c59-352">hello DACPAC must be accessible toohello service.</span></span> <span data-ttu-id="89c59-353">Det är rekommenderat tooupload en skapade DACPAC tooAzure lagring och skapa en [signatur för delad åtkomst](../storage/common/storage-dotnet-shared-access-signature-part-1.md) för hello DACPAC.</span><span class="sxs-lookup"><span data-stu-id="89c59-353">It is recommended tooupload a created DACPAC tooAzure Storage and create a [Shared Access Signature](../storage/common/storage-dotnet-shared-access-signature-part-1.md) for hello DACPAC.</span></span>

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="tooupdate-a-data-tier-application-dacpac-for-execution-across-databases"></a><span data-ttu-id="89c59-354">tooupdate datanivå-program (DACPAC) för körning över databaser</span><span class="sxs-lookup"><span data-stu-id="89c59-354">tooupdate a data-tier application (DACPAC) for execution across databases</span></span>
<span data-ttu-id="89c59-355">Befintliga DACPACs som registrerats i den elastiska databasen jobb kan vara uppdaterade toopoint toonew URI: er.</span><span class="sxs-lookup"><span data-stu-id="89c59-355">Existing DACPACs registered within Elastic Database Jobs can be updated toopoint toonew URIs.</span></span> <span data-ttu-id="89c59-356">Använd hello [ **cmdlet Set-AzureSqlJobContentDefinition** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) tooupdate hello DACPAC URI på en befintlig registrerad DACPAC:</span><span class="sxs-lookup"><span data-stu-id="89c59-356">Use hello [**Set-AzureSqlJobContentDefinition cmdlet**](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) tooupdate hello DACPAC URI on an existing registered DACPAC:</span></span>

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="toocreate-a-job-tooapply-a-data-tier-application-dacpac-across-databases"></a><span data-ttu-id="89c59-357">toocreate jobbet-tooapply datanivå-program (DACPAC) över databaser</span><span class="sxs-lookup"><span data-stu-id="89c59-357">toocreate a job tooapply a data-tier application (DACPAC) across databases</span></span>
<span data-ttu-id="89c59-358">När en DACPAC har skapats i den elastiska databasen jobb, kan ett jobb skapas tooapply hello DACPAC över flera databaser.</span><span class="sxs-lookup"><span data-stu-id="89c59-358">After a DACPAC has been created within Elastic Database Jobs, a job can be created tooapply hello DACPAC across a group of databases.</span></span> <span data-ttu-id="89c59-359">hello följande PowerShell-skript kan vara används toocreate ett DACPAC-jobb över en anpassad samling med databaser:</span><span class="sxs-lookup"><span data-stu-id="89c59-359">hello following PowerShell script can be used toocreate a DACPAC job across a custom collection of databases:</span></span>

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
