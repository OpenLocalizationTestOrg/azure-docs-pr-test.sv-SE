---
title: "Stream Analytics: Rotera logga in autentiseringsuppgifterna för in- och utdataenheter | Microsoft Docs"
description: "Lär dig mer om att uppdatera autentiseringsuppgifterna för Stream Analytics-in- och utdataenheter."
keywords: "autentiseringsuppgifter för inloggning"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 42ae83e1-cd33-49bb-a455-a39a7c151ea4
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 2cb995a3969a8cb025f371ed0ab160cd04b0454d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a><span data-ttu-id="8d365-104">Rotera inloggningsuppgifterna för in- och utdataenheter i Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="8d365-104">Rotate login credentials for inputs and outputs in Stream Analytics Jobs</span></span>
## <a name="abstract"></a><span data-ttu-id="8d365-105">Abstrakt</span><span class="sxs-lookup"><span data-stu-id="8d365-105">Abstract</span></span>
<span data-ttu-id="8d365-106">Azure Stream Analytics idag tillåter inte att ersätta autentiseringsuppgifter på en in-/ utdata när jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="8d365-106">Azure Stream Analytics today doesn’t allow replacing the credentials on an input/output while the job is running.</span></span>

<span data-ttu-id="8d365-107">Medan Azure Stream Analytics stöder återuppta ett jobb från senaste utdata, vill vi dela hela processen för att minimera fördröjning mellan stoppas och starta jobbet och rotera inloggningsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="8d365-107">While Azure Stream Analytics does support resuming a job from last output, we wanted to share the entire process for minimizing the lag between the stopping and starting of the job and rotating the login credentials.</span></span>

## <a name="part-1---prepare-the-new-set-of-credentials"></a><span data-ttu-id="8d365-108">Del 1 – förbereda den nya uppsättningen autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="8d365-108">Part 1 - Prepare the new set of credentials:</span></span>
<span data-ttu-id="8d365-109">Den här delen gäller följande indata/utdata:</span><span class="sxs-lookup"><span data-stu-id="8d365-109">This part is applicable to the following inputs/outputs:</span></span>

* <span data-ttu-id="8d365-110">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="8d365-110">Blob Storage</span></span>
* <span data-ttu-id="8d365-111">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="8d365-111">Event Hubs</span></span>
* <span data-ttu-id="8d365-112">SQL Database</span><span class="sxs-lookup"><span data-stu-id="8d365-112">SQL Database</span></span>
* <span data-ttu-id="8d365-113">Table Storage</span><span class="sxs-lookup"><span data-stu-id="8d365-113">Table Storage</span></span>

<span data-ttu-id="8d365-114">Andra indata/utdata, fortsätter du med del 2.</span><span class="sxs-lookup"><span data-stu-id="8d365-114">For other inputs/outputs, proceed with Part 2.</span></span>

### <a name="blob-storagetable-storage"></a><span data-ttu-id="8d365-115">BLOB storage/tabellagring</span><span class="sxs-lookup"><span data-stu-id="8d365-115">Blob storage/Table storage</span></span>
1. <span data-ttu-id="8d365-116">Gå till lagring-tillägget på Azure-hanteringsportalen:</span><span class="sxs-lookup"><span data-stu-id="8d365-116">Go to the Storage extention on the Azure Management portal:</span></span>  
   ![graphic1][graphic1]
2. <span data-ttu-id="8d365-118">Leta upp den lagring som används av ditt jobb och gå till den:</span><span class="sxs-lookup"><span data-stu-id="8d365-118">Locate the storage used by your job and go into it:</span></span>  
   ![graphic2][graphic2]
3. <span data-ttu-id="8d365-120">Klicka på Hantera åtkomstnycklar-kommando:</span><span class="sxs-lookup"><span data-stu-id="8d365-120">Click the Manage Access Keys command:</span></span>  
   ![graphic3][graphic3]
4. <span data-ttu-id="8d365-122">Mellan den primära åtkomstnyckeln och den sekundära åtkomstnyckeln **Välj den som inte används av ditt jobb**.</span><span class="sxs-lookup"><span data-stu-id="8d365-122">Between the Primary Access Key and the Secondary Access Key, **pick the one not used by your job**.</span></span>
5. <span data-ttu-id="8d365-123">Klicka på Generera:</span><span class="sxs-lookup"><span data-stu-id="8d365-123">Hit regenerate:</span></span>  
   ![graphic4][graphic4]
6. <span data-ttu-id="8d365-125">Kopiera den nyligen skapade nyckeln:</span><span class="sxs-lookup"><span data-stu-id="8d365-125">Copy the newly generated key:</span></span>  
   ![graphic5][graphic5]
7. <span data-ttu-id="8d365-127">Fortsätta att del 2.</span><span class="sxs-lookup"><span data-stu-id="8d365-127">Continue to Part 2.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="8d365-128">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="8d365-128">Event hubs</span></span>
1. <span data-ttu-id="8d365-129">Gå till Service Bus-tillägget på Azure-hanteringsportalen:</span><span class="sxs-lookup"><span data-stu-id="8d365-129">Go to the Service Bus extension on the Azure Management portal:</span></span>  
   ![graphic6][graphic6]
2. <span data-ttu-id="8d365-131">Leta upp Service Bus Namespace som används av ditt jobb och gå till den:</span><span class="sxs-lookup"><span data-stu-id="8d365-131">Locate the Service Bus Namespace used by your job and go into it:</span></span>  
   ![graphic7][graphic7]
3. <span data-ttu-id="8d365-133">Om jobbet använder en princip för delad åtkomst på Service Bus-Namespace, hoppa till steg 6</span><span class="sxs-lookup"><span data-stu-id="8d365-133">If your job uses a shared access policy on the Service Bus Namespace, jump to step 6</span></span>  
4. <span data-ttu-id="8d365-134">Gå till fliken Händelsehubbar:</span><span class="sxs-lookup"><span data-stu-id="8d365-134">Go to the Event Hubs tab:</span></span>  
   ![graphic8][graphic8]
5. <span data-ttu-id="8d365-136">Hitta Händelsehubben som används av ditt jobb och gå till den:</span><span class="sxs-lookup"><span data-stu-id="8d365-136">Locate the Event Hub used by your job and go into it:</span></span>  
   ![graphic9][graphic9]
6. <span data-ttu-id="8d365-138">Gå till fliken Konfigurera:</span><span class="sxs-lookup"><span data-stu-id="8d365-138">Go to the Configure Tab:</span></span>  
   ![graphic10][graphic10]
7. <span data-ttu-id="8d365-140">Leta upp den princip för delad åtkomst som används av ditt jobb på principnamn listrutan:</span><span class="sxs-lookup"><span data-stu-id="8d365-140">On the Policy Name drop-down, locate the shared access policy used by your job:</span></span>  
   ![graphic11][graphic11]
8. <span data-ttu-id="8d365-142">Mellan primärnyckeln och sekundärnyckeln, **Välj den som inte används av ditt jobb**.</span><span class="sxs-lookup"><span data-stu-id="8d365-142">Between the Primary Key and the Secondary Key, **pick the one not used by your job**.</span></span>  
9. <span data-ttu-id="8d365-143">Klicka på Generera:</span><span class="sxs-lookup"><span data-stu-id="8d365-143">Hit regenerate:</span></span>  
   ![graphic12][graphic12]
10. <span data-ttu-id="8d365-145">Kopiera den nyligen skapade nyckeln:</span><span class="sxs-lookup"><span data-stu-id="8d365-145">Copy the newly generated key:</span></span>  
   ![graphic13][graphic13]
11. <span data-ttu-id="8d365-147">Fortsätta att del 2.</span><span class="sxs-lookup"><span data-stu-id="8d365-147">Continue to Part 2.</span></span>  

### <a name="sql-database"></a><span data-ttu-id="8d365-148">SQL Database</span><span class="sxs-lookup"><span data-stu-id="8d365-148">SQL Database</span></span>
> [!NOTE]
> <span data-ttu-id="8d365-149">Obs: du behöver ansluta till tjänsten SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="8d365-149">Note: you will need to connect to the SQL Database Service.</span></span> <span data-ttu-id="8d365-150">Vi ska visa hur du gör detta med hjälp av hanteringen av på Azure-hanteringsportalen, men du kan välja att använda vissa klientsidan verktyg som SQL Server Management Studio samt.</span><span class="sxs-lookup"><span data-stu-id="8d365-150">We are going to show how to do this using the management experience on the Azure Management portal but you may choose to use some client-side tool such as SQL Server Management Studio as well.</span></span>
>
> 

1. <span data-ttu-id="8d365-151">Gå till tillägget SQL-databaser på Azure-hanteringsportalen:</span><span class="sxs-lookup"><span data-stu-id="8d365-151">Go to the SQL Databases extension on the Azure Management portal:</span></span>  
   ![graphic14][graphic14]
2. <span data-ttu-id="8d365-153">Leta reda på SQL-databas som används av ditt jobb och **klickar du på servern** länk på samma rad:</span><span class="sxs-lookup"><span data-stu-id="8d365-153">Locate the SQL Database used by your job and **click on the server** link on the same line:</span></span>  
   <span data-ttu-id="8d365-154">![graphic15][graphic15]</span><span class="sxs-lookup"><span data-stu-id="8d365-154">![graphic15][graphic15]</span></span>
3. <span data-ttu-id="8d365-155">Klicka på kommandot hantera:</span><span class="sxs-lookup"><span data-stu-id="8d365-155">Click the Manage command:</span></span>  
   ![graphic16][graphic16]
4. <span data-ttu-id="8d365-157">Typen databasen Master:</span><span class="sxs-lookup"><span data-stu-id="8d365-157">Type Database Master:</span></span>  
   ![graphic17][graphic17]
5. <span data-ttu-id="8d365-159">Ange ditt användarnamn, lösenord och klicka på Logga in:</span><span class="sxs-lookup"><span data-stu-id="8d365-159">Type in your User Name, Password, and click Log on:</span></span>  
   ![graphic18][graphic18]
6. <span data-ttu-id="8d365-161">Klicka på ny fråga:</span><span class="sxs-lookup"><span data-stu-id="8d365-161">Click New Query:</span></span>  
   ![graphic19][graphic19]
7. <span data-ttu-id="8d365-163">Typen i följande fråga ersätta < login_name > med ditt användarnamn och ersätta <enterStrongPasswordHere> med ditt nya lösenord:</span><span class="sxs-lookup"><span data-stu-id="8d365-163">Type in the following query replacing <login_name> with your User Name and replacing <enterStrongPasswordHere> with your new password:</span></span>  
   `CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8. <span data-ttu-id="8d365-164">Klicka på Kör:</span><span class="sxs-lookup"><span data-stu-id="8d365-164">Click Run:</span></span>  
   ![graphic20][graphic20]
9. <span data-ttu-id="8d365-166">Gå tillbaka till steg 2 och den här gången klickar du på databasen:</span><span class="sxs-lookup"><span data-stu-id="8d365-166">Go back to step 2 and this time click the database:</span></span>  
   ![graphic21][graphic21]
10. <span data-ttu-id="8d365-168">Klicka på kommandot hantera:</span><span class="sxs-lookup"><span data-stu-id="8d365-168">Click the Manage command:</span></span>  
   ![graphic22][graphic22]
11. <span data-ttu-id="8d365-170">Ange ditt användarnamn, lösenord och klicka på Logga in:</span><span class="sxs-lookup"><span data-stu-id="8d365-170">type in your User Name, Password, and click Logon:</span></span>  
   ![graphic23][graphic23]
12. <span data-ttu-id="8d365-172">Klicka på ny fråga:</span><span class="sxs-lookup"><span data-stu-id="8d365-172">Click New Query:</span></span>  
   ![graphic24][graphic24]
13. <span data-ttu-id="8d365-174">Skriv i följande fråga ersätta < användarnamn > med ett namn som du vill använda att identifiera den här inloggningen i kontexten för den här databasen (du kan ange samma värde som du gav för < login_name >, till exempel) och ersätter < login_name > med din nya användarnamn:</span><span class="sxs-lookup"><span data-stu-id="8d365-174">Type in the following query replacing <user_name> with a name by which you want to identify this login in the context of this database (you can provide the same value you gave for <login_name>, for example) and replacing <login_name> with your new user name:</span></span>  
   `CREATE USER <user_name> FROM LOGIN <login_name>`
14. <span data-ttu-id="8d365-175">Klicka på Kör:</span><span class="sxs-lookup"><span data-stu-id="8d365-175">Click Run:</span></span>  
   ![graphic25][graphic25]
15. <span data-ttu-id="8d365-177">Nu bör du ge din nya användare med samma roller och behörigheter som den ursprungliga användaren hade.</span><span class="sxs-lookup"><span data-stu-id="8d365-177">You should now provide your new user with the same roles and privileges your original user had.</span></span>
16. <span data-ttu-id="8d365-178">Fortsätta att del 2.</span><span class="sxs-lookup"><span data-stu-id="8d365-178">Continue to Part 2.</span></span>

## <a name="part-2-stopping-the-stream-analytics-job"></a><span data-ttu-id="8d365-179">Del 2: Stoppar Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="8d365-179">Part 2: Stopping the Stream Analytics Job</span></span>
1. <span data-ttu-id="8d365-180">Gå till Stream Analytics-tillägget på Azure-hanteringsportalen:</span><span class="sxs-lookup"><span data-stu-id="8d365-180">Go to the Stream Analytics extension on the Azure Management portal:</span></span>  
   ![graphic26][graphic26]
2. <span data-ttu-id="8d365-182">Leta upp ditt jobb och gå till den:</span><span class="sxs-lookup"><span data-stu-id="8d365-182">Locate your job and go into it:</span></span>  
   ![graphic27][graphic27]
3. <span data-ttu-id="8d365-184">Gå till fliken för indata eller utdata baserat på om du rotera autentiseringsuppgifter på indata eller utdata.</span><span class="sxs-lookup"><span data-stu-id="8d365-184">Go to the Inputs tab or the Outputs tab based on whether you are rotating the credentials on an Input or on an Output.</span></span>  
   ![graphic28][graphic28]
4. <span data-ttu-id="8d365-186">Klicka på kommandot Stop och bekräfta att jobbet har stoppats:</span><span class="sxs-lookup"><span data-stu-id="8d365-186">Click the Stop command and confirm the job has stopped:</span></span>  
   <span data-ttu-id="8d365-187">![graphic29][graphic29] väntar du tills jobbet att stoppa.</span><span class="sxs-lookup"><span data-stu-id="8d365-187">![graphic29][graphic29] Wait for the job to stop.</span></span>
5. <span data-ttu-id="8d365-188">Leta upp indata/utdata som du vill rotera autentiseringsuppgifter på och gå till den:</span><span class="sxs-lookup"><span data-stu-id="8d365-188">Locate the input/output you want to rotate credentials on and go into it:</span></span>  
   ![graphic30][graphic30]
6. <span data-ttu-id="8d365-190">Gå vidare till del 3.</span><span class="sxs-lookup"><span data-stu-id="8d365-190">Proceed to Part 3.</span></span>

## <a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a><span data-ttu-id="8d365-191">Del 3: Redigera autentiseringsuppgifter på Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="8d365-191">Part 3: Editing the credentials on the Stream Analytics Job</span></span>
### <a name="blob-storagetable-storage"></a><span data-ttu-id="8d365-192">BLOB storage/tabellagring</span><span class="sxs-lookup"><span data-stu-id="8d365-192">Blob storage/Table storage</span></span>
1. <span data-ttu-id="8d365-193">Leta upp fältet Lagringskontonyckel och klistra in den nyligen skapade nyckeln till den:</span><span class="sxs-lookup"><span data-stu-id="8d365-193">Find the Storage Account Key field and paste your newly generated key into it:</span></span>  
   ![graphic31][graphic31]
2. <span data-ttu-id="8d365-195">Klicka på kommandot Spara och bekräfta att spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="8d365-195">Click the Save command and confirm saving your changes:</span></span>  
   ![graphic32][graphic32]
3. <span data-ttu-id="8d365-197">En anslutningstest startar automatiskt när du sparar ändringarna, se till att har har passerat.</span><span class="sxs-lookup"><span data-stu-id="8d365-197">A connection test will automatically start when you save your changes, make sure that is has successfully passed.</span></span>
4. <span data-ttu-id="8d365-198">Gå vidare till del 4.</span><span class="sxs-lookup"><span data-stu-id="8d365-198">Proceed to Part 4.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="8d365-199">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="8d365-199">Event hubs</span></span>
1. <span data-ttu-id="8d365-200">Hitta nyckelfältet Event Hub princip och klistra in den nyligen skapade nyckeln i den:</span><span class="sxs-lookup"><span data-stu-id="8d365-200">Find the Event Hub Policy Key field and paste your newly generated key into it:</span></span>  
   ![graphic33][graphic33]
2. <span data-ttu-id="8d365-202">Klicka på kommandot Spara och bekräfta att spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="8d365-202">Click the Save command and confirm saving your changes:</span></span>  
   ![graphic34][graphic34]
3. <span data-ttu-id="8d365-204">En anslutningstest startar automatiskt när du sparar ändringarna, se till att den har passerat.</span><span class="sxs-lookup"><span data-stu-id="8d365-204">A connection test will automatically start when you save your changes, make sure that it has successfully passed.</span></span>
4. <span data-ttu-id="8d365-205">Gå vidare till del 4.</span><span class="sxs-lookup"><span data-stu-id="8d365-205">Proceed to Part 4.</span></span>

### <a name="power-bi"></a><span data-ttu-id="8d365-206">Power BI</span><span class="sxs-lookup"><span data-stu-id="8d365-206">Power BI</span></span>
1. <span data-ttu-id="8d365-207">Klicka på förnya auktoriseringen:</span><span class="sxs-lookup"><span data-stu-id="8d365-207">Click the Renew authorization:</span></span>  

   ![graphic35][graphic35]
2. <span data-ttu-id="8d365-209">Du får följande bekräftelse:</span><span class="sxs-lookup"><span data-stu-id="8d365-209">You will get the following confirmation:</span></span>  

   ![graphic36][graphic36]
3. <span data-ttu-id="8d365-211">Klicka på kommandot Spara och bekräfta att spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="8d365-211">Click the Save command and confirm saving your changes:</span></span>  
   ![graphic37][graphic37]
4. <span data-ttu-id="8d365-213">En anslutningstest startar automatiskt när du sparar ändringarna, se till att den passerat har.</span><span class="sxs-lookup"><span data-stu-id="8d365-213">A connection test will automatically start when you save your changes, make sure it has successfully passed.</span></span>
5. <span data-ttu-id="8d365-214">Gå vidare till del 4.</span><span class="sxs-lookup"><span data-stu-id="8d365-214">Proceed to Part 4.</span></span>

### <a name="sql-database"></a><span data-ttu-id="8d365-215">SQL Database</span><span class="sxs-lookup"><span data-stu-id="8d365-215">SQL Database</span></span>
1. <span data-ttu-id="8d365-216">Hitta fälten användarnamn och lösenord och klistra in den nyligen skapade uppsättningen autentiseringsuppgifter i dem:</span><span class="sxs-lookup"><span data-stu-id="8d365-216">Find the User Name and Password fields and paste your newly created set of credentials into them:</span></span>  
   ![graphic38][graphic38]
2. <span data-ttu-id="8d365-218">Klicka på kommandot Spara och bekräfta att spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="8d365-218">Click the Save command and confirm saving your changes:</span></span>  
   ![graphic39][graphic39]
3. <span data-ttu-id="8d365-220">En anslutningstest startar automatiskt när du sparar ändringarna, se till att den har passerat.</span><span class="sxs-lookup"><span data-stu-id="8d365-220">A connection test will automatically start when you save your changes, make sure that it has successfully passed.</span></span>  
4. <span data-ttu-id="8d365-221">Gå vidare till del 4.</span><span class="sxs-lookup"><span data-stu-id="8d365-221">Proceed to Part 4.</span></span>

## <a name="part-4-starting-your-job-from-last-stopped-time"></a><span data-ttu-id="8d365-222">Del 4: Från och med jobbet senast stoppad</span><span class="sxs-lookup"><span data-stu-id="8d365-222">Part 4: Starting your job from last stopped time</span></span>
1. <span data-ttu-id="8d365-223">Navigera bort från in-/ utdata:</span><span class="sxs-lookup"><span data-stu-id="8d365-223">Navigate away from the Input/Output:</span></span>  
   ![graphic40][graphic40]
2. <span data-ttu-id="8d365-225">Klicka på Start-kommando:</span><span class="sxs-lookup"><span data-stu-id="8d365-225">Click the Start command:</span></span>  
   ![graphic41][graphic41]
3. <span data-ttu-id="8d365-227">Välj senast stoppad och klicka på OK:</span><span class="sxs-lookup"><span data-stu-id="8d365-227">Pick the Last Stopped Time and click OK:</span></span>  
   ![graphic42][graphic42]
4. <span data-ttu-id="8d365-229">Gå vidare till del 5.</span><span class="sxs-lookup"><span data-stu-id="8d365-229">Proceed to Part 5.</span></span>  

## <a name="part-5-removing-the-old-set-of-credentials"></a><span data-ttu-id="8d365-230">Del 5: Ta bort den gamla uppsättningen autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="8d365-230">Part 5: Removing the old set of credentials</span></span>
<span data-ttu-id="8d365-231">Den här delen gäller följande indata/utdata:</span><span class="sxs-lookup"><span data-stu-id="8d365-231">This part is applicable to the following inputs/outputs:</span></span>

* <span data-ttu-id="8d365-232">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="8d365-232">Blob Storage</span></span>
* <span data-ttu-id="8d365-233">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="8d365-233">Event Hubs</span></span>
* <span data-ttu-id="8d365-234">SQL Database</span><span class="sxs-lookup"><span data-stu-id="8d365-234">SQL Database</span></span>
* <span data-ttu-id="8d365-235">Table Storage</span><span class="sxs-lookup"><span data-stu-id="8d365-235">Table Storage</span></span>

### <a name="blob-storagetable-storage"></a><span data-ttu-id="8d365-236">BLOB storage/tabellagring</span><span class="sxs-lookup"><span data-stu-id="8d365-236">Blob storage/Table storage</span></span>
<span data-ttu-id="8d365-237">Upprepa del 1 för åtkomstnyckel som tidigare användes av ditt jobb för att förnya nu oanvända snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="8d365-237">Repeat Part 1 for the Access Key that was previously used by your job to renew the now unused Access Key.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="8d365-238">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="8d365-238">Event hubs</span></span>
<span data-ttu-id="8d365-239">Upprepa del 1 för den nyckel som tidigare användes av ditt jobb för att förnya nu oanvända nyckeln.</span><span class="sxs-lookup"><span data-stu-id="8d365-239">Repeat Part 1 for the Key that was previously used by your job to renew the now unused Key.</span></span>

### <a name="sql-database"></a><span data-ttu-id="8d365-240">SQL Database</span><span class="sxs-lookup"><span data-stu-id="8d365-240">SQL Database</span></span>
1. <span data-ttu-id="8d365-241">Gå tillbaka till frågefönstret från del 1 steg 7 och anger följande fråga ersätta < previous_login_name > med det användarnamn som tidigare användes av ditt jobb:</span><span class="sxs-lookup"><span data-stu-id="8d365-241">Go back to the query window from Part 1 Step 7 and type in the following query, replacing <previous_login_name> with the User Name that was previously used by your job:</span></span>  
   `DROP LOGIN <previous_login_name>`  
2. <span data-ttu-id="8d365-242">Klicka på Kör:</span><span class="sxs-lookup"><span data-stu-id="8d365-242">Click Run:</span></span>  
   ![graphic43][graphic43]  

<span data-ttu-id="8d365-244">Du bör få följande bekräftelse:</span><span class="sxs-lookup"><span data-stu-id="8d365-244">You should get the following confirmation:</span></span> 

    Command(s) completed successfully.

## <a name="get-help"></a><span data-ttu-id="8d365-245">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="8d365-245">Get help</span></span>
<span data-ttu-id="8d365-246">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="8d365-246">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d365-247">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d365-247">Next steps</span></span>
* [<span data-ttu-id="8d365-248">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8d365-248">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="8d365-249">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8d365-249">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="8d365-250">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="8d365-250">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="8d365-251">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="8d365-251">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="8d365-252">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="8d365-252">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png

