---
title: "Skapa och hantera elastiska jobb med hjälp av PowerShell | Microsoft Docs"
description: "PowerShell som används för att hantera Azure SQL Database-pooler"
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
ms.openlocfilehash: b4c97e8f51581f9a3f7c5a8d8e82562255fe7b48
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-sql-database-elastic-jobs-using-powershell-preview"></a><span data-ttu-id="4eb8c-103">Skapa och hantera SQL Database: elastiska jobb med hjälp av PowerShell (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="4eb8c-103">Create and manage SQL Database elastic jobs using PowerShell (preview)</span></span>

<span data-ttu-id="4eb8c-104">PowerShell-APIs för **elastisk databas jobb** (under förhandsgranskning) gör att du kan definiera en grupp databaser mot vilken skript körs.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-104">The PowerShell APIs for **Elastic Database jobs** (in preview), let you define a group of databases against which scripts will execute.</span></span> <span data-ttu-id="4eb8c-105">Den här artikeln visar hur du skapar och hanterar **elastisk databas jobb** med PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-105">This article shows how to create and manage **Elastic Database jobs** using PowerShell cmdlets.</span></span> <span data-ttu-id="4eb8c-106">Se [elastiska jobb översikt](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4eb8c-106">See [Elastic jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="4eb8c-107">Krav</span><span class="sxs-lookup"><span data-stu-id="4eb8c-107">Prerequisites</span></span>
* <span data-ttu-id="4eb8c-108">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-108">An Azure subscription.</span></span> <span data-ttu-id="4eb8c-109">För en kostnadsfri utvärderingsversion finns [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4eb8c-109">For a free trial, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="4eb8c-110">En uppsättning databaser som skapats med elastiska Databasverktyg.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-110">A set of databases created with the Elastic Database tools.</span></span> <span data-ttu-id="4eb8c-111">Se [Kom igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4eb8c-111">See [Get started with Elastic Database tools](sql-database-elastic-scale-get-started.md).</span></span>
* <span data-ttu-id="4eb8c-112">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-112">Azure PowerShell.</span></span> <span data-ttu-id="4eb8c-113">Mer information finns i [Så här installerar och konfigurerar du Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4eb8c-113">For detailed information, see [How to install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span>
* <span data-ttu-id="4eb8c-114">**Den elastiska databasen jobb** PowerShell paket: finns [jobb installerar elastisk databas](sql-database-elastic-jobs-service-installation.md)</span><span class="sxs-lookup"><span data-stu-id="4eb8c-114">**Elastic Database jobs** PowerShell package: See [Installing Elastic Database jobs](sql-database-elastic-jobs-service-installation.md)</span></span>

### <a name="select-your-azure-subscription"></a><span data-ttu-id="4eb8c-115">Välj din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4eb8c-115">Select your Azure subscription</span></span>
<span data-ttu-id="4eb8c-116">Välj prenumerationen du behöver ditt prenumerations-Id (**- SubscriptionId**) eller prenumerationsnamn (**- SubscriptionName**).</span><span class="sxs-lookup"><span data-stu-id="4eb8c-116">To select the subscription you need your subscription Id (**-SubscriptionId**) or subscription name (**-SubscriptionName**).</span></span> <span data-ttu-id="4eb8c-117">Om du har flera prenumerationer kan du köra den **Get-AzureRmSubscription** cmdlet och kopiera önskade prenumerationsinformation från resultatet anges.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-117">If you have multiple subscriptions you can run the **Get-AzureRmSubscription** cmdlet and copy the desired subscription information from the result set.</span></span> <span data-ttu-id="4eb8c-118">När du har din prenumerationsinformation kör du följande cmdlet för att ange den här prenumerationen som standard, nämligen mål för att skapa och hantera jobb:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-118">Once you have your subscription information, run the following commandlet to set this subscription as the default, namely the target for creating and managing jobs:</span></span>

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

<span data-ttu-id="4eb8c-119">Den [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) rekommenderas att utveckla och köra PowerShell-skript mot jobb för elastisk databas.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-119">The [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) is recommended for usage to develop and execute PowerShell scripts against the Elastic Database jobs.</span></span>

## <a name="elastic-database-jobs-objects"></a><span data-ttu-id="4eb8c-120">Elastiska jobb databasobjekt</span><span class="sxs-lookup"><span data-stu-id="4eb8c-120">Elastic Database jobs objects</span></span>
<span data-ttu-id="4eb8c-121">I följande tabell visas ut alla objekttyper för **elastisk databas jobb** tillsammans med dess beskrivning och relevanta PowerShell APIs.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-121">The following table lists out all the object types of **Elastic Database jobs** along with its description and relevant PowerShell APIs.</span></span>

<table style="width:100%">
  <tr>
    <th><span data-ttu-id="4eb8c-122">Objekttyp</span><span class="sxs-lookup"><span data-stu-id="4eb8c-122">Object Type</span></span></th>
    <th><span data-ttu-id="4eb8c-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4eb8c-123">Description</span></span></th>
    <th><span data-ttu-id="4eb8c-124">Relaterade PowerShell API: er</span><span class="sxs-lookup"><span data-stu-id="4eb8c-124">Related PowerShell APIs</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="4eb8c-125">Autentiseringsuppgift</span><span class="sxs-lookup"><span data-stu-id="4eb8c-125">Credential</span></span></td>
    <td><span data-ttu-id="4eb8c-126">Användarnamn och lösenord för att ansluta till databaser för körning av skript eller ett program av DACPACs.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-126">Username and password to use when connecting to databases for execution of scripts or application of DACPACs.</span></span> <p><span data-ttu-id="4eb8c-127">Lösenordet krypteras innan du skickar till och lagra i databasen för den elastiska databasen jobb.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-127">The password is encrypted before sending to and storing in the Elastic Database Jobs database.</span></span>  <span data-ttu-id="4eb8c-128">Lösenordet dekrypteras av tjänsten elastiska databasen jobb via autentiseringsuppgifter skapas och som överförts från ett installationsskript.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-128">The password is decrypted by the Elastic Database Jobs service via the credential created and uploaded from the installation script.</span></span></td>
    <td><p><span data-ttu-id="4eb8c-129">Get-AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="4eb8c-129">Get-AzureSqlJobCredential</span></span></p>
    <p><span data-ttu-id="4eb8c-130">Ny AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="4eb8c-130">New-AzureSqlJobCredential</span></span></p><p><span data-ttu-id="4eb8c-131">Ange AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="4eb8c-131">Set-AzureSqlJobCredential</span></span></p></td></td>
  </tr>

  <tr>
    <td><span data-ttu-id="4eb8c-132">Skript</span><span class="sxs-lookup"><span data-stu-id="4eb8c-132">Script</span></span></td>
    <td><span data-ttu-id="4eb8c-133">Transact-SQL-skript som ska användas för körning över databaser.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-133">Transact-SQL script to be used for execution across databases.</span></span>  <span data-ttu-id="4eb8c-134">Skriptet ska skapas för att vara idempotent eftersom tjänsten kommer att försöka körningen av skriptet vid fel.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-134">The script should be authored to be idempotent since the service will retry execution of the script upon failures.</span></span>
    </td>
    <td>
    <p><span data-ttu-id="4eb8c-135">Get-AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="4eb8c-135">Get-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="4eb8c-136">Get-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="4eb8c-136">Get-AzureSqlJobContentDefinition</span></span></p>
    <p><span data-ttu-id="4eb8c-137">Ny AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="4eb8c-137">New-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="4eb8c-138">Ange AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="4eb8c-138">Set-AzureSqlJobContentDefinition</span></span></p>
    </td>
  </tr>

  <tr>
    <td><span data-ttu-id="4eb8c-139">DACPAC</span><span class="sxs-lookup"><span data-stu-id="4eb8c-139">DACPAC</span></span></td>
    <td><span data-ttu-id="4eb8c-140"><a href="https://msdn.microsoft.com/library/ee210546.aspx">Programmet på datanivå </a> paket som ska tillämpas över databaser.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-140"><a href="https://msdn.microsoft.com/library/ee210546.aspx">Data-tier application </a> package to be applied across databases.</span></span>

    </td>
    <td>
    <p><span data-ttu-id="4eb8c-141">Get-AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="4eb8c-141">Get-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="4eb8c-142">Ny AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="4eb8c-142">New-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="4eb8c-143">Ange AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="4eb8c-143">Set-AzureSqlJobContentDefinition</span></span></p>
    </td>
  </tr>
  <tr>
    <td><span data-ttu-id="4eb8c-144">Databasen mål</span><span class="sxs-lookup"><span data-stu-id="4eb8c-144">Database Target</span></span></td>
    <td><span data-ttu-id="4eb8c-145">Namn för databasen och pekar på en Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-145">Database and server name pointing to an Azure SQL Database.</span></span>

    </td>
    <td>
    <p><span data-ttu-id="4eb8c-146">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="4eb8c-146">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="4eb8c-147">Ny AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="4eb8c-147">New-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
  <tr>
    <td><span data-ttu-id="4eb8c-148">Fragmentera kartan mål</span><span class="sxs-lookup"><span data-stu-id="4eb8c-148">Shard Map Target</span></span></td>
    <td><span data-ttu-id="4eb8c-149">Kombinationen av target databas och autentiseringsuppgifter som används för att avgöra information som lagras i en elastisk databas Fragmentera mappning.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-149">Combination of a database target and a credential to be used to determine information stored within an Elastic Database shard map.</span></span>
    </td>
    <td>
    <p><span data-ttu-id="4eb8c-150">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="4eb8c-150">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="4eb8c-151">Ny AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="4eb8c-151">New-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="4eb8c-152">Ange AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="4eb8c-152">Set-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
<tr>
    <td><span data-ttu-id="4eb8c-153">Mål för anpassad insamling</span><span class="sxs-lookup"><span data-stu-id="4eb8c-153">Custom Collection Target</span></span></td>
    <td><span data-ttu-id="4eb8c-154">Angiven grupp databaser som ska användas tillsammans för att köras.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-154">Defined group of databases to collectively use for execution.</span></span></td>
    <td>
    <p><span data-ttu-id="4eb8c-155">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="4eb8c-155">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="4eb8c-156">Ny AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="4eb8c-156">New-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
<tr>
    <td><span data-ttu-id="4eb8c-157">Anpassad samling underordnade mål</span><span class="sxs-lookup"><span data-stu-id="4eb8c-157">Custom Collection Child Target</span></span></td>
    <td><span data-ttu-id="4eb8c-158">Mål för databasen som refereras från en anpassad samling.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-158">Database target that is referenced from a custom collection.</span></span></td>
    <td>
    <p><span data-ttu-id="4eb8c-159">Lägg till AzureSqlJobChildTarget</span><span class="sxs-lookup"><span data-stu-id="4eb8c-159">Add-AzureSqlJobChildTarget</span></span></p>
    <p><span data-ttu-id="4eb8c-160">Ta bort AzureSqlJobChildTarget</span><span class="sxs-lookup"><span data-stu-id="4eb8c-160">Remove-AzureSqlJobChildTarget</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="4eb8c-161">Jobb</span><span class="sxs-lookup"><span data-stu-id="4eb8c-161">Job</span></span></td>
    <td>
    <p><span data-ttu-id="4eb8c-162">Definition av parametrar för ett jobb som kan användas för att starta körningen eller för att uppfylla ett schema.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-162">Definition of parameters for a job that can be used to trigger execution or to fulfill a schedule.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="4eb8c-163">Get-AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="4eb8c-163">Get-AzureSqlJob</span></span></p>
    <p><span data-ttu-id="4eb8c-164">Ny AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="4eb8c-164">New-AzureSqlJob</span></span></p>
    <p><span data-ttu-id="4eb8c-165">Ange AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="4eb8c-165">Set-AzureSqlJob</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="4eb8c-166">Jobbkörningen</span><span class="sxs-lookup"><span data-stu-id="4eb8c-166">Job Execution</span></span></td>
    <td>
    <p><span data-ttu-id="4eb8c-167">Behållare för aktiviteter som krävs för att uppfylla antingen köra ett skript eller använda en DACPAC till ett mål med autentiseringsuppgifter för databasanslutningar med fel hanteras i enlighet med en körningsprincip.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-167">Container of tasks necessary to fulfill either executing a script or applying a DACPAC to a target using credentials for database connections with failures handled in accordance to an execution policy.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="4eb8c-168">Get-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="4eb8c-168">Get-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="4eb8c-169">Start-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="4eb8c-169">Start-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="4eb8c-170">Stoppa AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="4eb8c-170">Stop-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="4eb8c-171">Vänta AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="4eb8c-171">Wait-AzureSqlJobExecution</span></span></p>
  </tr>

<tr>
    <td><span data-ttu-id="4eb8c-172">Jobb för körning av aktiviteten</span><span class="sxs-lookup"><span data-stu-id="4eb8c-172">Job Task Execution</span></span></td>
    <td>
    <p><span data-ttu-id="4eb8c-173">Arbetsenhet att slutföra ett jobb.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-173">Single unit of work to fulfill a job.</span></span></p>
    <p><span data-ttu-id="4eb8c-174">Om en projektaktivitet inte kan har köra, resulterande Undantagsmeddelande loggas och en ny matchande projektaktivitet skapas och körs i enlighet med angiven körningsprincipen.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-174">If a job task is not able to successfully execute, the resulting exception message will be logged and a new matching job task will be created and executed in accordance to the specified execution policy.</span></span></p></p>
    </td>
    <td>
    <p><span data-ttu-id="4eb8c-175">Get-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="4eb8c-175">Get-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="4eb8c-176">Start-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="4eb8c-176">Start-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="4eb8c-177">Stoppa AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="4eb8c-177">Stop-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="4eb8c-178">Vänta AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="4eb8c-178">Wait-AzureSqlJobExecution</span></span></p>
  </tr>

<tr>
    <td><span data-ttu-id="4eb8c-179">Jobbet körningsprincipen</span><span class="sxs-lookup"><span data-stu-id="4eb8c-179">Job Execution Policy</span></span></td>
    <td>
    <p><span data-ttu-id="4eb8c-180">Kontroller jobbet körning timeout, försök gränser och intervall mellan försök.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-180">Controls job execution timeouts, retry limits and intervals between retries.</span></span></p>
    <p><span data-ttu-id="4eb8c-181">Den elastiska databasen jobb innehåller en körningsprincip för standard-jobb som orsakar i princip obegränsat återförsök uppgiften jobbfel med exponentiell backoff intervall mellan varje försök.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-181">Elastic Database jobs includes a default job execution policy which cause essentially infinite retries of job task failures with exponential backoff of intervals between each retry.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="4eb8c-182">Get-AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="4eb8c-182">Get-AzureSqlJobExecutionPolicy</span></span></p>
    <p><span data-ttu-id="4eb8c-183">Ny AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="4eb8c-183">New-AzureSqlJobExecutionPolicy</span></span></p>
    <p><span data-ttu-id="4eb8c-184">Ange AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="4eb8c-184">Set-AzureSqlJobExecutionPolicy</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="4eb8c-185">Schema</span><span class="sxs-lookup"><span data-stu-id="4eb8c-185">Schedule</span></span></td>
    <td>
    <p><span data-ttu-id="4eb8c-186">Tid baserat specifikationen för körning ska ske på ett reoccurring intervall eller på en gång.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-186">Time based specification for execution to take place either on a reoccurring interval or at a single time.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="4eb8c-187">Get-AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="4eb8c-187">Get-AzureSqlJobSchedule</span></span></p>
    <p><span data-ttu-id="4eb8c-188">Ny AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="4eb8c-188">New-AzureSqlJobSchedule</span></span></p>
    <p><span data-ttu-id="4eb8c-189">Ange AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="4eb8c-189">Set-AzureSqlJobSchedule</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="4eb8c-190">Jobbet utlöser</span><span class="sxs-lookup"><span data-stu-id="4eb8c-190">Job Triggers</span></span></td>
    <td>
    <p><span data-ttu-id="4eb8c-191">En mappning mellan ett jobb och ett schema för att utlösaren jobb enligt schemat.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-191">A mapping between a job and a schedule to trigger job execution according to the schedule.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="4eb8c-192">Ny AzureSqlJobTrigger</span><span class="sxs-lookup"><span data-stu-id="4eb8c-192">New-AzureSqlJobTrigger</span></span></p>
    <p><span data-ttu-id="4eb8c-193">Ta bort AzureSqlJobTrigger</span><span class="sxs-lookup"><span data-stu-id="4eb8c-193">Remove-AzureSqlJobTrigger</span></span></p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a><span data-ttu-id="4eb8c-194">Stöds för elastisk databas jobb gruppera typer</span><span class="sxs-lookup"><span data-stu-id="4eb8c-194">Supported Elastic Database jobs group types</span></span>
<span data-ttu-id="4eb8c-195">Jobbet körs Transact-SQL (T-SQL)-skript eller ett program av DACPACs över flera databaser.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-195">The job executes Transact-SQL (T-SQL) scripts or application of DACPACs across a group of databases.</span></span> <span data-ttu-id="4eb8c-196">När ett jobb skickas för att köra över en grupp databaser jobbet ”utökar” den till underordnade jobb där varje utför begärda körning mot en enskild databas i gruppen.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-196">When a job is submitted to be executed across a group of databases, the job “expands” the into child jobs where each performs the requested execution against a single database in the group.</span></span> 

<span data-ttu-id="4eb8c-197">Det finns två typer av grupper som du kan skapa:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-197">There are two types of groups that you can create:</span></span> 

* <span data-ttu-id="4eb8c-198">[Fragmentera kartan](sql-database-elastic-scale-shard-map-management.md) grupp: när ett jobb skickas för att rikta en Fragmentera karta jobbet frågar Fragmentera kartan för att fastställa den aktuella mängden shards och skapar sedan underordnade jobb för varje Fragmentera i kartan Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-198">[Shard Map](sql-database-elastic-scale-shard-map-management.md) group: When a job is submitted to target a shard map, the job queries the shard map to determine its current set of shards, and then creates child jobs for each shard in the shard map.</span></span>
* <span data-ttu-id="4eb8c-199">Samlingsgruppen för anpassade: en anpassad definierad uppsättning databaser.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-199">Custom Collection group: A custom defined set of databases.</span></span> <span data-ttu-id="4eb8c-200">När ett jobb riktar sig till en anpassad samling, skapar den underordnade jobb för varje databas för närvarande i anpassade samlingar.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-200">When a job targets a custom collection, it creates child jobs for each database currently in the custom collection.</span></span>

## <a name="to-set-the-elastic-database-jobs-connection"></a><span data-ttu-id="4eb8c-201">Om du vill ange den elastiska databasen jobb anslutning</span><span class="sxs-lookup"><span data-stu-id="4eb8c-201">To set the Elastic Database jobs connection</span></span>
<span data-ttu-id="4eb8c-202">En anslutning måste anges till jobben *databasen* innan du använder jobb API: er.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-202">A connection needs to be set to the jobs *control database* prior to using the jobs APIs.</span></span> <span data-ttu-id="4eb8c-203">Kör denna cmdlet utlöser ett fönster för autentiseringsuppgifter att popup-begär användarnamn och lösenord som skapas när du installerar elastisk databas jobb.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-203">Running this cmdlet triggers a credential window to pop up requesting the user name and password created when installing Elastic Database jobs.</span></span> <span data-ttu-id="4eb8c-204">Alla exemplen i det här avsnittet förutsätter att det här första steget redan har utförts.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-204">All examples provided within this topic assume that this first step has already been performed.</span></span>

<span data-ttu-id="4eb8c-205">Öppna en anslutning till elastisk databas jobb:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-205">Open a connection to the Elastic Database jobs:</span></span>

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a><span data-ttu-id="4eb8c-206">Krypterade autentiseringsuppgifter i jobb för elastisk databas</span><span class="sxs-lookup"><span data-stu-id="4eb8c-206">Encrypted credentials within the Elastic Database jobs</span></span>
<span data-ttu-id="4eb8c-207">Databasautentiseringsuppgifter kan infogas i jobben *databasen* med dess lösenord krypteras.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-207">Database credentials can be inserted into the jobs *control database*  with its password encrypted.</span></span> <span data-ttu-id="4eb8c-208">Det är nödvändigt att lagra autentiseringsuppgifter för att aktivera jobb som ska utföras vid ett senare tillfälle (med jobbscheman).</span><span class="sxs-lookup"><span data-stu-id="4eb8c-208">It is necessary to store credentials to enable jobs to be executed at a later time, (using job schedules).</span></span>

<span data-ttu-id="4eb8c-209">Kryptering fungerar via ett certifikat som skapas som en del av installationen.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-209">Encryption works through a certificate created as part of the installation script.</span></span> <span data-ttu-id="4eb8c-210">Installationsskriptet skapar och laddar upp certifikatet i Azure Cloud Service för dekryptering av lagrade krypterade lösenord.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-210">The installation script creates and uploads the certificate into the Azure Cloud Service for decryption of the stored encrypted passwords.</span></span> <span data-ttu-id="4eb8c-211">Azure Cloud Service senare lagrar den offentliga nyckeln i jobben *databasen* som gör det möjligt för gränssnittet PowerShell API eller klassiska Azure-portalen att kryptera en lösenordet utan att certifikatet ska vara lokalt installerad.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-211">The Azure Cloud Service later stores the public key within the jobs *control database* which enables the PowerShell API or Azure Classic Portal interface to encrypt a provided password without requiring the certificate to be locally installed.</span></span>

<span data-ttu-id="4eb8c-212">Autentiseringsuppgifter lösenord är krypterad och säkra från användare med läsåtkomst till elastiska jobb databasobjekt.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-212">The credential passwords are encrypted and secure from users with read-only access to Elastic Database jobs objects.</span></span> <span data-ttu-id="4eb8c-213">Men det är möjligt för en obehörig användare med skrivskyddad åtkomst till den elastiska databasen jobb objekt att extrahera ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-213">But it is possible for a malicious user with read-write access to Elastic Database Jobs objects to extract a password.</span></span> <span data-ttu-id="4eb8c-214">Autentiseringsuppgifterna är avsedda att återanvändas i jobbet körningar.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-214">Credentials are designed to be reused across job executions.</span></span> <span data-ttu-id="4eb8c-215">Autentiseringsuppgifter skickas till måldatabaserna när anslutningar upprättas.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-215">Credentials are passed to target databases when establishing connections.</span></span> <span data-ttu-id="4eb8c-216">Det finns för närvarande inga begränsningar för måldatabaserna används för varje autentiseringsuppgifter, obehörig användare kan lägga till databasen mål för en databas med skadliga användarkontrollen.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-216">There are currently no restrictions on the target databases used for each credential, malicious user could add a database target for a database under the malicious user's control.</span></span> <span data-ttu-id="4eb8c-217">Användaren kan sedan starta ett jobb som den här databasen för att få lösenord för de autentiseringsuppgifter som mål.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-217">The user could subsequently start a job targeting this database to gain the credential's password.</span></span>

<span data-ttu-id="4eb8c-218">Rekommenderade säkerhetsmetoder för elastisk databas jobb är:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-218">Security best practices for Elastic Database jobs include:</span></span>

* <span data-ttu-id="4eb8c-219">Begränsa användning av API: er till betrodda personer.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-219">Limit usage of the APIs to trusted individuals.</span></span>
* <span data-ttu-id="4eb8c-220">Autentiseringsuppgifter bör ha minst behörighet att utföra åtgärden för jobbet.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-220">Credentials should have the least privileges necessary to perform the job task.</span></span>  <span data-ttu-id="4eb8c-221">Mer information kan ses i detta [auktoriserings- och behörigheter](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN-artikel.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-221">More information can be seen within this [Authorization and Permissions](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN article.</span></span>

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a><span data-ttu-id="4eb8c-222">Så här skapar du en krypterade autentiseringsuppgifter för jobbkörningen över databaser</span><span class="sxs-lookup"><span data-stu-id="4eb8c-222">To create an encrypted credential for job execution across databases</span></span>
<span data-ttu-id="4eb8c-223">Så här skapar du en ny krypterade autentiseringsuppgifter i [ **cmdlet Get-Credential** ](https://technet.microsoft.com/library/hh849815.aspx) uppmanas att ange ett användarnamn och lösenord som kan skickas till den [ **cmdlet New-AzureSqlJobCredential** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).</span><span class="sxs-lookup"><span data-stu-id="4eb8c-223">To create a new encrypted credential, the [**Get-Credential cmdlet**](https://technet.microsoft.com/library/hh849815.aspx) prompts for a user name and password that can be passed to the [**New-AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).</span></span>

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a><span data-ttu-id="4eb8c-224">Uppdatera autentiseringsuppgifterna</span><span class="sxs-lookup"><span data-stu-id="4eb8c-224">To update credentials</span></span>
<span data-ttu-id="4eb8c-225">När du ändrar lösenord, använda den [ **cmdlet Set-AzureSqlJobCredential** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) och ange den **CredentialName** parameter.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-225">When passwords change, use the [**Set-AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) and set the **CredentialName** parameter.</span></span>

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a><span data-ttu-id="4eb8c-226">Definiera ett mål för elastisk databas Fragmentera karta</span><span class="sxs-lookup"><span data-stu-id="4eb8c-226">To define an Elastic Database shard map target</span></span>
<span data-ttu-id="4eb8c-227">Att köra ett jobb för alla databaser i en Fragmentera (skapat med hjälp av [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md)), Använd en Fragmentera som mål för databasen.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-227">To execute a job against all databases in a shard set (created using [Elastic Database client library](sql-database-elastic-database-client-library.md)), use a shard map as the database target.</span></span> <span data-ttu-id="4eb8c-228">Det här exemplet kräver ett delat program som skapats med hjälp av klientbiblioteket för elastisk databas.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-228">This example requires a sharded application created using the Elastic Database client library.</span></span> <span data-ttu-id="4eb8c-229">Se [komma igång med elastisk databas verktyg exempel](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4eb8c-229">See [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

<span data-ttu-id="4eb8c-230">Fragmentera kartan manager-databasen måste anges som ett mål för databasen och sedan kartan specifika Fragmentera måste anges som mål.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-230">The shard map manager database must be set as a database target and then the specific shard map must be specified as a target.</span></span>

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="4eb8c-231">Skapa ett T-SQL-skript för körning på databaser</span><span class="sxs-lookup"><span data-stu-id="4eb8c-231">Create a T-SQL Script for execution across databases</span></span>
<span data-ttu-id="4eb8c-232">När du skapar T-SQL-skript för körning, rekommenderas att skapa dem för att vara [idempotent](https://en.wikipedia.org/wiki/Idempotence) och motståndskraftiga mot fel.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-232">When creating T-SQL scripts for execution, it is highly recommended to build them to be [idempotent](https://en.wikipedia.org/wiki/Idempotence) and resilient against failures.</span></span> <span data-ttu-id="4eb8c-233">Den elastiska databasen jobb kommer att försöka körningen av ett skript när körningen påträffar ett fel, oavsett klassificeringen av felet.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-233">Elastic Database jobs will retry execution of a script whenever execution encounters a failure, regardless of the classification of the failure.</span></span>

<span data-ttu-id="4eb8c-234">Använd den [ **cmdlet New-AzureSqlJobContent** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) att skapa och spara ett skript för körning och ange den **- ContentName** och **- CommandText** parametrar.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-234">Use the [**New-AzureSqlJobContent cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) to create and save a script for execution and set the **-ContentName** and **-CommandText** parameters.</span></span>

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

### <a name="create-a-new-script-from-a-file"></a><span data-ttu-id="4eb8c-235">Skapa ett nytt skript från en fil</span><span class="sxs-lookup"><span data-stu-id="4eb8c-235">Create a new script from a file</span></span>
<span data-ttu-id="4eb8c-236">Om T-SQL-skript har definierats i en fil, använder du detta för att importera skriptet:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-236">If the T-SQL script is defined within a file, use this to import the script:</span></span>

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="4eb8c-237">Uppdatera ett T-SQL-skript för körning över databaser</span><span class="sxs-lookup"><span data-stu-id="4eb8c-237">To update a T-SQL script for execution across databases</span></span>
<span data-ttu-id="4eb8c-238">Det här PowerShell-skriptet uppdaterar T-SQL-kommandotexten för ett befintligt skript.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-238">This PowerShell script updates the T-SQL command text for an existing script.</span></span>

<span data-ttu-id="4eb8c-239">Ange följande variabler återspeglar önskade skript definition anges:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-239">Set the following variables to reflect the desired script definition to be set:</span></span>

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
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

### <a name="to-update-the-definition-to-an-existing-script"></a><span data-ttu-id="4eb8c-240">Uppdatera definitionen till ett befintligt skript</span><span class="sxs-lookup"><span data-stu-id="4eb8c-240">To update the definition to an existing script</span></span>
    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a><span data-ttu-id="4eb8c-241">Att skapa ett jobb för att köra ett skript på en Fragmentera karta</span><span class="sxs-lookup"><span data-stu-id="4eb8c-241">To create a job to execute a script across a shard map</span></span>
<span data-ttu-id="4eb8c-242">Detta PowerShell-skript startar ett jobb för körning av ett skript på varje Fragmentera i en elastisk skalbarhet Fragmentera mappning.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-242">This PowerShell script starts a job for execution of a script across each shard in an Elastic Scale shard map.</span></span>

<span data-ttu-id="4eb8c-243">Ange följande variabler återspeglar önskade skript och mål:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-243">Set the following variables to reflect the desired script and target:</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a><span data-ttu-id="4eb8c-244">Att köra ett jobb</span><span class="sxs-lookup"><span data-stu-id="4eb8c-244">To execute a job</span></span>
<span data-ttu-id="4eb8c-245">Det här PowerShell-skriptet körs ett befintligt jobb:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-245">This PowerShell script executes an existing job:</span></span>

<span data-ttu-id="4eb8c-246">Uppdatera följande variabel för att återspegla önskade Jobbnamnet som har köras:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-246">Update the following variable to reflect the desired job name to have executed:</span></span>

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution

## <a name="to-retrieve-the-state-of-a-single-job-execution"></a><span data-ttu-id="4eb8c-247">Att hämta tillståndet för en enskild jobbkörningen</span><span class="sxs-lookup"><span data-stu-id="4eb8c-247">To retrieve the state of a single job execution</span></span>
<span data-ttu-id="4eb8c-248">Använd den [ **cmdlet Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) och ange den **JobExecutionId** parametern för att visa status för jobbkörningen.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-248">Use the [**Get-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) and set the **JobExecutionId** parameter to view the state of job execution.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

<span data-ttu-id="4eb8c-249">Använda samma **Get-AzureSqlJobExecution** med den **IncludeChildren** parametern för att visa status för underordnade jobbet körningar, nämligen ett visst tillstånd för varje jobbkörningen mot varje databas som mål för jobbet.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-249">Use the same **Get-AzureSqlJobExecution** cmdlet with the **IncludeChildren** parameter to view the state of child job executions, namely the specific state for each job execution against each database targeted by the job.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a><span data-ttu-id="4eb8c-250">Visa status över flera jobb körningar</span><span class="sxs-lookup"><span data-stu-id="4eb8c-250">To view the state across multiple job executions</span></span>
<span data-ttu-id="4eb8c-251">Den [ **cmdlet Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) flera valfria parametrar som kan användas för att visa flera jobb körningar, filtreras via de angivna parametrarna.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-251">The [**Get-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljob) has multiple optional parameters that can be used to display multiple job executions, filtered through the provided parameters.</span></span> <span data-ttu-id="4eb8c-252">Följande visar några av de möjliga sätt att använda Get-AzureSqlJobExecution:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-252">The following demonstrates some of the possible ways to use Get-AzureSqlJobExecution:</span></span>

<span data-ttu-id="4eb8c-253">Hämta alla aktiva översta nivå jobbet körningar:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-253">Retrieve all active top level job executions:</span></span>

    Get-AzureSqlJobExecution

<span data-ttu-id="4eb8c-254">Hämta alla jobb för övre nivå körningar, inklusive inaktiva jobb körningar:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-254">Retrieve all top level job executions, including inactive job executions:</span></span>

    Get-AzureSqlJobExecution -IncludeInactive

<span data-ttu-id="4eb8c-255">Hämta alla underordnade jobbet körningar av en tillhandahållna jobbet körnings-ID, inklusive inaktiva jobb körningar:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-255">Retrieve all child job executions of a provided job execution ID, including inactive job executions:</span></span>

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren

<span data-ttu-id="4eb8c-256">Hämta alla jobb körningar som skapats med hjälp av ett schema / jobb tillsammans med inaktiva jobb:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-256">Retrieve all job executions created using a schedule / job combination, including inactive jobs:</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

<span data-ttu-id="4eb8c-257">Hämta alla jobb som mål för en angiven Fragmentera karta, inklusive inaktiva jobb:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-257">Retrieve all jobs targeting a specified shard map, including inactive jobs:</span></span>

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

<span data-ttu-id="4eb8c-258">Hämta alla jobb som mål för en angiven anpassad samling, inklusive inaktiva jobb:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-258">Retrieve all jobs targeting a specified custom collection, including inactive jobs:</span></span>

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

<span data-ttu-id="4eb8c-259">Hämta listan över jobb uppgiften körningar inom en specifik jobbkörningen:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-259">Retrieve the list of job task executions within a specific job execution:</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

<span data-ttu-id="4eb8c-260">Hämta jobb aktivitetsinformation körning:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-260">Retrieve job task execution details:</span></span>

<span data-ttu-id="4eb8c-261">Följande PowerShell-skript kan användas för att visa information om ett jobb för körning av aktiviteten, vilket är särskilt användbart när körningen felsökningsändamål.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-261">The following PowerShell script can be used to view the details of a job task execution, which is particularly useful when debugging execution failures.</span></span>

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a><span data-ttu-id="4eb8c-262">Att hämta fel i jobb uppgiften körningar</span><span class="sxs-lookup"><span data-stu-id="4eb8c-262">To retrieve failures within job task executions</span></span>
<span data-ttu-id="4eb8c-263">Den **JobTaskExecution objekt** innehåller en egenskap för livscykeln för uppgiften tillsammans med en meddelandeegenskap.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-263">The **JobTaskExecution object** includes a property for the lifecycle of the task along with a message property.</span></span> <span data-ttu-id="4eb8c-264">Om ett jobb för körning av aktiviteten misslyckades livscykel egenskap anges *misslyckades* och meddelandeegenskapen kommer att anges till det resulterande Undantagsmeddelandet och dess stacken.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-264">If a job task execution failed, the lifecycle property will be set to *Failed* and the message property will be set to the resulting exception message and its stack.</span></span> <span data-ttu-id="4eb8c-265">Om ett jobb misslyckades, är det viktigt att visa information om jobbuppgifter som misslyckades för ett visst jobb.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-265">If a job did not succeed, it is important to view the details of job tasks that did not succeed for a given job.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a><span data-ttu-id="4eb8c-266">Vänta på ett jobbkörning ska slutföras</span><span class="sxs-lookup"><span data-stu-id="4eb8c-266">To wait for a job execution to complete</span></span>
<span data-ttu-id="4eb8c-267">Följande PowerShell-skript kan användas för att vänta på en projektaktivitet att slutföra:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-267">The following PowerShell script can be used to wait for a job task to complete:</span></span>

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a><span data-ttu-id="4eb8c-268">Skapa en princip för anpassad körning</span><span class="sxs-lookup"><span data-stu-id="4eb8c-268">Create a custom execution policy</span></span>
<span data-ttu-id="4eb8c-269">Den elastiska databasen jobb kan du skapa anpassade körningsprinciper som kan användas när du startar jobb.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-269">Elastic Database jobs supports creating custom execution policies that can be applied when starting jobs.</span></span>

<span data-ttu-id="4eb8c-270">Körningsprinciper tillåter för närvarande för att definiera:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-270">Execution policies currently allow for defining:</span></span>

* <span data-ttu-id="4eb8c-271">Namn: Identifierare för körningsprincipen.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-271">Name: Identifier for the execution policy.</span></span>
* <span data-ttu-id="4eb8c-272">Tidsgräns för jobb: Total tid innan ett jobb kommer att avbrytas av elastiska databasen jobb.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-272">Job Timeout: Total time before a job will be canceled by Elastic Database Jobs.</span></span>
* <span data-ttu-id="4eb8c-273">Inledande återförsöksintervall: Intervall ska vänta innan nytt försök görs.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-273">Initial Retry Interval: Interval to wait before first retry.</span></span>
* <span data-ttu-id="4eb8c-274">Maximal återförsöksintervall: Locket återförsöksintervall ska användas.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-274">Maximum Retry Interval: Cap of retry intervals to use.</span></span>
* <span data-ttu-id="4eb8c-275">Försök intervall Backoff värde: Värde används för att beräkna nästa intervall mellan försök.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-275">Retry Interval Backoff Coefficient: Coefficient used to calculate the next interval between retries.</span></span>  <span data-ttu-id="4eb8c-276">Följande formel används: (första försök intervall) * Math.pow ((intervall Backoff värde) (antal nya försök) - 2).</span><span class="sxs-lookup"><span data-stu-id="4eb8c-276">The following formula is used: (Initial Retry Interval) * Math.pow((Interval Backoff Coefficient), (Number of Retries) - 2).</span></span> 
* <span data-ttu-id="4eb8c-277">Maximalt antal försök: Maximalt antal försök försöker utföra inom ett jobb.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-277">Maximum Attempts: The maximum number of retry attempts to perform within a job.</span></span>

<span data-ttu-id="4eb8c-278">Standard-körningsprincipen används följande värden:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-278">The default execution policy uses the following values:</span></span>

* <span data-ttu-id="4eb8c-279">Namn: Standardprincipen för körning</span><span class="sxs-lookup"><span data-stu-id="4eb8c-279">Name: Default execution policy</span></span>
* <span data-ttu-id="4eb8c-280">Tidsgräns för jobb: 1 vecka</span><span class="sxs-lookup"><span data-stu-id="4eb8c-280">Job Timeout: 1 week</span></span>
* <span data-ttu-id="4eb8c-281">Inledande återförsöksintervall: 100 millisekunder</span><span class="sxs-lookup"><span data-stu-id="4eb8c-281">Initial Retry Interval:  100 milliseconds</span></span>
* <span data-ttu-id="4eb8c-282">Maximal återförsöksintervall: 30 minuter</span><span class="sxs-lookup"><span data-stu-id="4eb8c-282">Maximum Retry Interval: 30 minutes</span></span>
* <span data-ttu-id="4eb8c-283">Försök intervall värde: 2</span><span class="sxs-lookup"><span data-stu-id="4eb8c-283">Retry Interval Coefficient: 2</span></span>
* <span data-ttu-id="4eb8c-284">Maximalt antal försök: 2 147 483 647</span><span class="sxs-lookup"><span data-stu-id="4eb8c-284">Maximum Attempts: 2,147,483,647</span></span>

<span data-ttu-id="4eb8c-285">Skapa önskade körningsprincipen:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-285">Create the desired execution policy:</span></span>

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a><span data-ttu-id="4eb8c-286">Uppdatera en anpassad körningsprincip</span><span class="sxs-lookup"><span data-stu-id="4eb8c-286">Update a custom execution policy</span></span>
<span data-ttu-id="4eb8c-287">Uppdatera önskade körningsprincipen att uppdatera:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-287">Update the desired execution policy to update:</span></span>

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy

## <a name="cancel-a-job"></a><span data-ttu-id="4eb8c-288">Avbryta ett jobb</span><span class="sxs-lookup"><span data-stu-id="4eb8c-288">Cancel a job</span></span>
<span data-ttu-id="4eb8c-289">Den elastiska databasen jobb stöder förfrågningar om annullering av jobb.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-289">Elastic Database Jobs supports cancellation requests of jobs.</span></span>  <span data-ttu-id="4eb8c-290">Om den elastiska databasen jobb upptäcker en begäran om att avbryta ett jobb som körs, försöker den att stoppa jobbet.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-290">If Elastic Database Jobs detects a cancellation request for a job currently being executed, it will attempt to stop the job.</span></span>

<span data-ttu-id="4eb8c-291">Det finns två olika sätt att elastiska databasen jobb kan utföra en annullering:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-291">There are two different ways that Elastic Database Jobs can perform a cancellation:</span></span>

1. <span data-ttu-id="4eb8c-292">Avbryt aktiviteter som körs: om en annullering identifieras när en aktivitet körs för närvarande en annullering görs inom körs aspekt av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-292">Cancel currently executing tasks: If a cancellation is detected while a task is currently running, a cancellation will be attempted within the currently executing aspect of the task.</span></span>  <span data-ttu-id="4eb8c-293">Exempel: om det finns en tidskrävande fråga som för närvarande utförs när en annullering görs, är ett försök att avbryta frågan.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-293">For example: If there is a long running query currently being performed when a cancellation is attempted, there will be an attempt to cancel the query.</span></span>
2. <span data-ttu-id="4eb8c-294">Om du avbryter återförsök för aktiviteten: om en annullering identifieras av kontrollen tråd innan en uppgift startas för körning tråden kontrollen ska undvika starta uppgiften och deklarera begäran som avbruten.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-294">Canceling task retries: If a cancellation is detected by the control thread before a task is launched for execution, the control thread will avoid launching the task and declare the request as canceled.</span></span>

<span data-ttu-id="4eb8c-295">Om det krävs en annullering av jobbet för överordnade jobb respekteras begäran om att avbryta för överordnade jobb och alla dess underordnade jobb.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-295">If a job cancellation is requested for a parent job, the cancellation request will be honored for the parent job and for all of its child jobs.</span></span>

<span data-ttu-id="4eb8c-296">För att skicka en begäran om att avbryta, Använd den [ **cmdlet Stop AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) och ange den **JobExecutionId** parameter.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-296">To submit a cancellation request, use the [**Stop-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) and set the **JobExecutionId** parameter.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a><span data-ttu-id="4eb8c-297">Ta bort ett jobb och Jobbhistorik asynkront</span><span class="sxs-lookup"><span data-stu-id="4eb8c-297">To delete a job and job history asynchronously</span></span>
<span data-ttu-id="4eb8c-298">Den elastiska databasen jobb stöder asynkrona borttagning av jobb.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-298">Elastic Database jobs supports asynchronous deletion of jobs.</span></span> <span data-ttu-id="4eb8c-299">Ett jobb kan vara markerad för borttagning och tas bort jobbet och alla dess jobbhistorik när alla jobb körningar har slutförts för jobbet.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-299">A job can be marked for deletion and the system will delete the job and all its job history after all job executions have completed for the job.</span></span> <span data-ttu-id="4eb8c-300">Systemet Avbryt inte automatiskt aktiva jobb körningar.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-300">The system will not automatically cancel active job executions.</span></span>  

<span data-ttu-id="4eb8c-301">Anropa [ **stoppa AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) att avbryta aktiva jobb körningar.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-301">Invoke [**Stop-AzureSqlJobExecution**](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) to cancel active job executions.</span></span>

<span data-ttu-id="4eb8c-302">Utlös borttagning av jobbet genom att använda den [ **cmdlet Remove-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/remove-azuresqljob) och ange den **jobbnamn** parameter.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-302">To trigger job deletion, use the [**Remove-AzureSqlJob cmdlet**](/powershell/module/elasticdatabasejobs/remove-azuresqljob) and set the **JobName** parameter.</span></span>

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName

## <a name="to-create-a-custom-database-target"></a><span data-ttu-id="4eb8c-303">Så här skapar du en anpassad databas mål</span><span class="sxs-lookup"><span data-stu-id="4eb8c-303">To create a custom database target</span></span>
<span data-ttu-id="4eb8c-304">Du kan definiera anpassade databasen mål för att köra en direkt eller som ska ingå i en anpassad databas-grupp.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-304">You can define custom database targets either for direct execution or for inclusion within a custom database group.</span></span> <span data-ttu-id="4eb8c-305">Till exempel eftersom **elastiska pooler** är stöds inte än direkt med hjälp av PowerShell APIs, kan du skapa en anpassad databas mål och anpassad databas samling mål som omfattar alla databaser i poolen.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-305">For example, because **elastic pools** are not yet directly supported using PowerShell APIs, you can create a custom database target and custom database collection target which encompasses all the databases in the pool.</span></span>

<span data-ttu-id="4eb8c-306">Ange följande variabler återspeglar den önskade databasinformationen:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-306">Set the following variables to reflect the desired database information:</span></span>

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a><span data-ttu-id="4eb8c-307">Så här skapar du en anpassad databas samling mål</span><span class="sxs-lookup"><span data-stu-id="4eb8c-307">To create a custom database collection target</span></span>
<span data-ttu-id="4eb8c-308">Använd den [ **ny AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) för att definiera en anpassad databas samling mål för att aktivera körningen över flera definierade databasen mål.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-308">Use the [**New-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet to define a custom database collection target to enable execution across multiple defined database targets.</span></span> <span data-ttu-id="4eb8c-309">När du har skapat en databasgrupp vara databaser kopplat till målet för anpassad insamling.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-309">After creating a database group, databases can be associated with the custom collection target.</span></span>

<span data-ttu-id="4eb8c-310">Ange följande variabler önskade anpassade samlingar mål konfiguration:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-310">Set the following variables to reflect the desired custom collection target configuration:</span></span>

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a><span data-ttu-id="4eb8c-311">Att lägga till databaser till en anpassad databas samling mål</span><span class="sxs-lookup"><span data-stu-id="4eb8c-311">To add databases to a custom database collection target</span></span>
<span data-ttu-id="4eb8c-312">Att lägga till en databas till en specifik egen samling använder den [ **Lägg till AzureSqlJobChildTarget** ](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-312">To add a database to a specific custom collection use the [**Add-AzureSqlJobChildTarget**](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) cmdlet.</span></span>

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a><span data-ttu-id="4eb8c-313">Granska databaser i en anpassad databas samling målet</span><span class="sxs-lookup"><span data-stu-id="4eb8c-313">Review the databases within a custom database collection target</span></span>
<span data-ttu-id="4eb8c-314">Använd den [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) för att hämta underordnade databaser i en anpassad databas samling målet.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-314">Use the [**Get-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet to retrieve the child databases within a custom database collection target.</span></span> 

    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a><span data-ttu-id="4eb8c-315">Skapa ett jobb för att köra ett skript i en anpassad databas samling mål</span><span class="sxs-lookup"><span data-stu-id="4eb8c-315">Create a job to execute a script across a custom database collection target</span></span>
<span data-ttu-id="4eb8c-316">Använd den [ **ny AzureSqlJob** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) för att skapa ett jobb mot en grupp databaser som definieras av en anpassad databas samling mål.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-316">Use the [**New-AzureSqlJob**](/powershell/module/elasticdatabasejobs/new-azuresqljob) cmdlet to create a job against a group of databases defined by a custom database collection target.</span></span> <span data-ttu-id="4eb8c-317">Elastiska databasen jobb kommer Expandera jobbet till flera underordnade jobb varje motsvarar en databas som är associerade med samlingen målet anpassad databas och se till att skriptet körs mot varje databas.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-317">Elastic Database jobs will expand the job into multiple child jobs each corresponding to a database associated with the custom database collection target and ensure that the script is executed against each database.</span></span> <span data-ttu-id="4eb8c-318">Igen, är det viktigt att skripten har idempotent för att hantera att återförsök.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-318">Again, it is important that scripts are idempotent to be resilient to retries.</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a><span data-ttu-id="4eb8c-319">Insamling av data över databaser</span><span class="sxs-lookup"><span data-stu-id="4eb8c-319">Data collection across databases</span></span>
<span data-ttu-id="4eb8c-320">Du kan använda ett jobb för att köra en fråga i en grupp med databaser och skicka resultaten till en viss tabell.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-320">You can use a job to execute a query across a group of databases and send the results to a specific table.</span></span> <span data-ttu-id="4eb8c-321">Tabellen kan efterfrågas efter faktumet att se resultatet av frågan från varje databas.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-321">The table can be queried after the fact to see the query’s results from each database.</span></span> <span data-ttu-id="4eb8c-322">Detta ger en asynkron metod för att köra en fråga över flera databaser.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-322">This provides an asynchronous method to execute a query across many databases.</span></span> <span data-ttu-id="4eb8c-323">Misslyckade försök hanteras automatiskt via återförsök.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-323">Failed attempts are handled automatically via retries.</span></span>

<span data-ttu-id="4eb8c-324">Den angivna tabellen skapas automatiskt om det inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-324">The specified destination table will be automatically created if it does not yet exist.</span></span> <span data-ttu-id="4eb8c-325">Den nya tabellen matchar schemat för den returnerade resultatuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-325">The new table matches the schema of the returned result set.</span></span> <span data-ttu-id="4eb8c-326">Om ett skript som returnerar flera resultatmängder elastisk databas jobb bara att skicka först till måltabellen.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-326">If a script returns multiple result sets, Elastic Database jobs will only send the first to the destination table.</span></span>

<span data-ttu-id="4eb8c-327">Följande PowerShell-skript körs ett skript och samlar in resultaten i en angiven tabell.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-327">The following PowerShell script executes a script and collects its results into a specified table.</span></span> <span data-ttu-id="4eb8c-328">Det här skriptet förutsätter att ett T-SQL-skript har skapats som matar ut en enda resultatmängd och ett mål för insamling av anpassad databas har skapats.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-328">This script assumes that a T-SQL script has been created which outputs a single result set and that a custom database collection target has been created.</span></span>

<span data-ttu-id="4eb8c-329">Det här skriptet använder den [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-329">This script uses the [**Get-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet.</span></span> <span data-ttu-id="4eb8c-330">Ange parametrar för skriptet, autentiseringsuppgifter och körning av mål:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-330">Set the parameters for script, credentials, and execution target:</span></span>

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

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a><span data-ttu-id="4eb8c-331">Skapa och starta ett jobb för scenarion för insamling av data</span><span class="sxs-lookup"><span data-stu-id="4eb8c-331">To create and start a job for data collection scenarios</span></span>
<span data-ttu-id="4eb8c-332">Det här skriptet använder den [ **Start AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-332">This script uses the [**Start-AzureSqlJobExecution**](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) cmdlet.</span></span>

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

## <a name="to-schedule-a-job-execution-trigger"></a><span data-ttu-id="4eb8c-333">Så här schemalägger du en utlösare för körning av jobbet</span><span class="sxs-lookup"><span data-stu-id="4eb8c-333">To schedule a job execution trigger</span></span>
<span data-ttu-id="4eb8c-334">Följande PowerShell-skript kan användas för att skapa ett återkommande schema.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-334">The following PowerShell script can be used to create a recurring schedule.</span></span> <span data-ttu-id="4eb8c-335">Det här skriptet använder ett minuters intervall, men [ **ny AzureSqlJobSchedule** ](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) stöder också - DayInterval, - HourInterval, - MonthInterval, och WeekInterval - parametrar.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-335">This script uses a minute interval, but [**New-AzureSqlJobSchedule**](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) also supports -DayInterval, -HourInterval, -MonthInterval, and -WeekInterval parameters.</span></span> <span data-ttu-id="4eb8c-336">Scheman som kör en gång kan skapas med skicka - görs.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-336">Schedules that execute only once can be created by passing -OneTime.</span></span>

<span data-ttu-id="4eb8c-337">Skapa ett nytt schema:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-337">Create a new schedule:</span></span>

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a><span data-ttu-id="4eb8c-338">Att utlösa ett jobb som körs på en tidsplan</span><span class="sxs-lookup"><span data-stu-id="4eb8c-338">To trigger a job executed on a time schedule</span></span>
<span data-ttu-id="4eb8c-339">En utlösare för jobbet kan definieras om du vill att ett jobb som körs enligt ett schema med tiden.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-339">A job trigger can be defined to have a job executed according to a time schedule.</span></span> <span data-ttu-id="4eb8c-340">Följande PowerShell-skript kan användas för att skapa en utlösare för jobbet.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-340">The following PowerShell script can be used to create a job trigger.</span></span>

<span data-ttu-id="4eb8c-341">Använd [ny AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) och ange följande variabler som motsvarar den önskade jobbet och schema:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-341">Use [New-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) and set the following variables to correspond to the desired job and schedule:</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    -JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a><span data-ttu-id="4eb8c-342">Ta bort en schemalagd association att avbryta jobbet körs enligt schema</span><span class="sxs-lookup"><span data-stu-id="4eb8c-342">To remove a scheduled association to stop job from executing on schedule</span></span>
<span data-ttu-id="4eb8c-343">Jobbet utlösaren kan tas bort för att avbryta igen jobbkörningen via en utlösare för jobbet.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-343">To discontinue reoccurring job execution through a job trigger, the job trigger can be removed.</span></span> <span data-ttu-id="4eb8c-344">Ta bort utlösaren jobbet om du vill stoppa ett jobb från utförs enligt ett schema som använder den [ **cmdlet Remove-AzureSqlJobTrigger**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).</span><span class="sxs-lookup"><span data-stu-id="4eb8c-344">Remove a job trigger to stop a job from being executed according to a schedule using the [**Remove-AzureSqlJobTrigger cmdlet**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a><span data-ttu-id="4eb8c-345">Hämta jobb utlösare som är bunden till en tidsplan</span><span class="sxs-lookup"><span data-stu-id="4eb8c-345">Retrieve job triggers bound to a time schedule</span></span>
<span data-ttu-id="4eb8c-346">Följande PowerShell-skript kan användas för att hämta och visa jobb utlösare som har registrerats för en viss tidsplan.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-346">The following PowerShell script can be used to obtain and display the job triggers registered to a particular time schedule.</span></span>

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a><span data-ttu-id="4eb8c-347">Om du vill hämta jobb utlösare som är bunden till ett jobb</span><span class="sxs-lookup"><span data-stu-id="4eb8c-347">To retrieve job triggers bound to a job</span></span>
<span data-ttu-id="4eb8c-348">Använd [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) att hämta och visa scheman som innehåller ett registrerade jobb.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-348">Use [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) to obtain and display schedules containing a registered job.</span></span>

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a><span data-ttu-id="4eb8c-349">Skapa ett program för datanivå (DACPAC) för körning över databaser</span><span class="sxs-lookup"><span data-stu-id="4eb8c-349">To create a data-tier application (DACPAC) for execution across databases</span></span>
<span data-ttu-id="4eb8c-350">Om du vill skapa en DACPAC finns [-datanivåprogram](https://msdn.microsoft.com/library/ee210546.aspx).</span><span class="sxs-lookup"><span data-stu-id="4eb8c-350">To create a DACPAC, see [Data-Tier applications](https://msdn.microsoft.com/library/ee210546.aspx).</span></span> <span data-ttu-id="4eb8c-351">Om du vill distribuera en DACPAC använder den [cmdlet New-AzureSqlJobContent](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent).</span><span class="sxs-lookup"><span data-stu-id="4eb8c-351">To deploy a DACPAC, use the [New-AzureSqlJobContent cmdlet](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent).</span></span> <span data-ttu-id="4eb8c-352">DACPAC måste vara tillgänglig för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-352">The DACPAC must be accessible to the service.</span></span> <span data-ttu-id="4eb8c-353">Det rekommenderas att överföra en skapade DACPAC till Azure Storage och skapa en [signatur för delad åtkomst](../storage/common/storage-dotnet-shared-access-signature-part-1.md) för DACPAC.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-353">It is recommended to upload a created DACPAC to Azure Storage and create a [Shared Access Signature](../storage/common/storage-dotnet-shared-access-signature-part-1.md) for the DACPAC.</span></span>

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a><span data-ttu-id="4eb8c-354">Uppdatera data-tier application (DACPAC) för körning över databaser</span><span class="sxs-lookup"><span data-stu-id="4eb8c-354">To update a data-tier application (DACPAC) for execution across databases</span></span>
<span data-ttu-id="4eb8c-355">Befintliga DACPACs som registrerats i den elastiska databasen jobb kan uppdateras för att peka mot nya URI: er.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-355">Existing DACPACs registered within Elastic Database Jobs can be updated to point to new URIs.</span></span> <span data-ttu-id="4eb8c-356">Använd den [ **cmdlet Set-AzureSqlJobContentDefinition** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) uppdatera DACPAC URI på en befintlig registrerade DACPAC:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-356">Use the [**Set-AzureSqlJobContentDefinition cmdlet**](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) to update the DACPAC URI on an existing registered DACPAC:</span></span>

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a><span data-ttu-id="4eb8c-357">Att skapa ett jobb för att tillämpa en dataskiktsprogrammet (DACPAC) över databaser</span><span class="sxs-lookup"><span data-stu-id="4eb8c-357">To create a job to apply a data-tier application (DACPAC) across databases</span></span>
<span data-ttu-id="4eb8c-358">När en DACPAC har skapats i den elastiska databasen jobb, kan du skapa ett jobb för att tillämpa DACPAC över flera databaser.</span><span class="sxs-lookup"><span data-stu-id="4eb8c-358">After a DACPAC has been created within Elastic Database Jobs, a job can be created to apply the DACPAC across a group of databases.</span></span> <span data-ttu-id="4eb8c-359">Följande PowerShell-skript kan användas för att skapa ett DACPAC-jobb över en anpassad samling med databaser:</span><span class="sxs-lookup"><span data-stu-id="4eb8c-359">The following PowerShell script can be used to create a DACPAC job across a custom collection of databases:</span></span>

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
