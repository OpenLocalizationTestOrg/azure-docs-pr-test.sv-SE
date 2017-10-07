---
title: "Stream Analytics: Rotera logga in autentiseringsuppgifterna för in- och utdataenheter | Microsoft Docs"
description: "Lär dig hur tooupdate hello autentiseringsuppgifter för Stream Analytics-in- och utdataenheter."
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
ms.openlocfilehash: ac2374c539012b66ab390656c5750024e02f6bdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a><span data-ttu-id="8f6ed-104">Rotera inloggningsuppgifterna för in- och utdataenheter i Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="8f6ed-104">Rotate login credentials for inputs and outputs in Stream Analytics Jobs</span></span>
## <a name="abstract"></a><span data-ttu-id="8f6ed-105">Abstrakt</span><span class="sxs-lookup"><span data-stu-id="8f6ed-105">Abstract</span></span>
<span data-ttu-id="8f6ed-106">Azure Stream Analytics idag tillåter inte att ersätta hello autentiseringsuppgifter på en in-/ utdata när hello jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-106">Azure Stream Analytics today doesn’t allow replacing hello credentials on an input/output while hello job is running.</span></span>

<span data-ttu-id="8f6ed-107">Medan Azure Stream Analytics stöder återuppta ett jobb från senaste utdata, vill vi tooshare hello hela processen för att minimera hello fördröjning mellan hello stoppar och startar för hello jobb och rotera hello inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-107">While Azure Stream Analytics does support resuming a job from last output, we wanted tooshare hello entire process for minimizing hello lag between hello stopping and starting of hello job and rotating hello login credentials.</span></span>

## <a name="part-1---prepare-hello-new-set-of-credentials"></a><span data-ttu-id="8f6ed-108">Del 1 – förbereda hello ny uppsättning autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-108">Part 1 - Prepare hello new set of credentials:</span></span>
<span data-ttu-id="8f6ed-109">Den här delen är tillämpliga toohello följande indata/utdata:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-109">This part is applicable toohello following inputs/outputs:</span></span>

* <span data-ttu-id="8f6ed-110">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="8f6ed-110">Blob Storage</span></span>
* <span data-ttu-id="8f6ed-111">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="8f6ed-111">Event Hubs</span></span>
* <span data-ttu-id="8f6ed-112">SQL Database</span><span class="sxs-lookup"><span data-stu-id="8f6ed-112">SQL Database</span></span>
* <span data-ttu-id="8f6ed-113">Table Storage</span><span class="sxs-lookup"><span data-stu-id="8f6ed-113">Table Storage</span></span>

<span data-ttu-id="8f6ed-114">Andra indata/utdata, fortsätter du med del 2.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-114">For other inputs/outputs, proceed with Part 2.</span></span>

### <a name="blob-storagetable-storage"></a><span data-ttu-id="8f6ed-115">BLOB storage/tabellagring</span><span class="sxs-lookup"><span data-stu-id="8f6ed-115">Blob storage/Table storage</span></span>
1. <span data-ttu-id="8f6ed-116">Gå toohello lagring-tillägget i hello Azure Management portal:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-116">Go toohello Storage extention on hello Azure Management portal:</span></span>  
   ![graphic1][graphic1]
2. <span data-ttu-id="8f6ed-118">Leta upp hello lagringsutrymme som används av ditt jobb och gå till den:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-118">Locate hello storage used by your job and go into it:</span></span>  
   ![graphic2][graphic2]
3. <span data-ttu-id="8f6ed-120">Klicka på hello hantera åtkomstnycklar kommando:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-120">Click hello Manage Access Keys command:</span></span>  
   ![graphic3][graphic3]
4. <span data-ttu-id="8f6ed-122">Mellan hello primära åtkomstnyckeln och hello sekundära åtkomstnyckeln **Välj hello en inte används av ditt jobb**.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-122">Between hello Primary Access Key and hello Secondary Access Key, **pick hello one not used by your job**.</span></span>
5. <span data-ttu-id="8f6ed-123">Klicka på Generera:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-123">Hit regenerate:</span></span>  
   ![graphic4][graphic4]
6. <span data-ttu-id="8f6ed-125">Kopiera hello nya nyckeln:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-125">Copy hello newly generated key:</span></span>  
   ![graphic5][graphic5]
7. <span data-ttu-id="8f6ed-127">Fortsätt tooPart 2.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-127">Continue tooPart 2.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="8f6ed-128">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="8f6ed-128">Event hubs</span></span>
1. <span data-ttu-id="8f6ed-129">Gå toohello Service Bus-tillägget i hello Azure Management portal:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-129">Go toohello Service Bus extension on hello Azure Management portal:</span></span>  
   ![graphic6][graphic6]
2. <span data-ttu-id="8f6ed-131">Leta upp hello Service Bus-Namespace som används av ditt jobb och gå till den:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-131">Locate hello Service Bus Namespace used by your job and go into it:</span></span>  
   ![graphic7][graphic7]
3. <span data-ttu-id="8f6ed-133">Om jobbet använder en princip för delad åtkomst på hello Service Bus Namespace, hoppa toostep 6</span><span class="sxs-lookup"><span data-stu-id="8f6ed-133">If your job uses a shared access policy on hello Service Bus Namespace, jump toostep 6</span></span>  
4. <span data-ttu-id="8f6ed-134">Gå toohello Händelsehubbar fliken:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-134">Go toohello Event Hubs tab:</span></span>  
   ![graphic8][graphic8]
5. <span data-ttu-id="8f6ed-136">Leta upp hello Event Hub som används av ditt jobb och gå till den:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-136">Locate hello Event Hub used by your job and go into it:</span></span>  
   ![graphic9][graphic9]
6. <span data-ttu-id="8f6ed-138">Gå toohello konfigurera fliken:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-138">Go toohello Configure Tab:</span></span>  
   ![graphic10][graphic10]
7. <span data-ttu-id="8f6ed-140">Leta upp hello delad åtkomstprincip som används av ditt jobb på hello principnamn listrutan:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-140">On hello Policy Name drop-down, locate hello shared access policy used by your job:</span></span>  
   ![graphic11][graphic11]
8. <span data-ttu-id="8f6ed-142">Mellan hello primärnyckel och hello sekundärnyckeln, **Välj hello en inte används av ditt jobb**.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-142">Between hello Primary Key and hello Secondary Key, **pick hello one not used by your job**.</span></span>  
9. <span data-ttu-id="8f6ed-143">Klicka på Generera:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-143">Hit regenerate:</span></span>  
   ![graphic12][graphic12]
10. <span data-ttu-id="8f6ed-145">Kopiera hello nya nyckeln:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-145">Copy hello newly generated key:</span></span>  
   ![graphic13][graphic13]
11. <span data-ttu-id="8f6ed-147">Fortsätt tooPart 2.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-147">Continue tooPart 2.</span></span>  

### <a name="sql-database"></a><span data-ttu-id="8f6ed-148">SQL Database</span><span class="sxs-lookup"><span data-stu-id="8f6ed-148">SQL Database</span></span>
> [!NOTE]
> <span data-ttu-id="8f6ed-149">Obs: du behöver tooconnect toohello SQL Database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-149">Note: you will need tooconnect toohello SQL Database Service.</span></span> <span data-ttu-id="8f6ed-150">Vi tooshow hur toodo detta med hjälp av hello hanteringen på hello Azure-hanteringsportalen, men du toouse vissa klientsidan verktyg som till exempel SQL Server Management Studio samt.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-150">We are going tooshow how toodo this using hello management experience on hello Azure Management portal but you may choose toouse some client-side tool such as SQL Server Management Studio as well.</span></span>
>
> 

1. <span data-ttu-id="8f6ed-151">Gå toohello SQL-databaser tillägg på hello Azure Management portal:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-151">Go toohello SQL Databases extension on hello Azure Management portal:</span></span>  
   ![graphic14][graphic14]
2. <span data-ttu-id="8f6ed-153">Leta upp hello SQL-databas som används av ditt jobb och **klickar du på hello server** länka hello samma rad:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-153">Locate hello SQL Database used by your job and **click on hello server** link on hello same line:</span></span>  
   <span data-ttu-id="8f6ed-154">![graphic15][graphic15]</span><span class="sxs-lookup"><span data-stu-id="8f6ed-154">![graphic15][graphic15]</span></span>
3. <span data-ttu-id="8f6ed-155">Klicka på hello hantera kommando:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-155">Click hello Manage command:</span></span>  
   ![graphic16][graphic16]
4. <span data-ttu-id="8f6ed-157">Typen databasen Master:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-157">Type Database Master:</span></span>  
   ![graphic17][graphic17]
5. <span data-ttu-id="8f6ed-159">Ange ditt användarnamn, lösenord och klicka på Logga in:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-159">Type in your User Name, Password, and click Log on:</span></span>  
   ![graphic18][graphic18]
6. <span data-ttu-id="8f6ed-161">Klicka på ny fråga:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-161">Click New Query:</span></span>  
   ![graphic19][graphic19]
7. <span data-ttu-id="8f6ed-163">Typen i hello följande fråga ersätter < login_name > med ditt användarnamn och ersätta <enterStrongPasswordHere> med ditt nya lösenord:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-163">Type in hello following query replacing <login_name> with your User Name and replacing <enterStrongPasswordHere> with your new password:</span></span>  
   `CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8. <span data-ttu-id="8f6ed-164">Klicka på Kör:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-164">Click Run:</span></span>  
   ![graphic20][graphic20]
9. <span data-ttu-id="8f6ed-166">Gå tillbaka toostep 2 och nu på hello databasen:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-166">Go back toostep 2 and this time click hello database:</span></span>  
   ![graphic21][graphic21]
10. <span data-ttu-id="8f6ed-168">Klicka på hello hantera kommando:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-168">Click hello Manage command:</span></span>  
   ![graphic22][graphic22]
11. <span data-ttu-id="8f6ed-170">Ange ditt användarnamn, lösenord och klicka på Logga in:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-170">type in your User Name, Password, and click Logon:</span></span>  
   ![graphic23][graphic23]
12. <span data-ttu-id="8f6ed-172">Klicka på ny fråga:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-172">Click New Query:</span></span>  
   ![graphic24][graphic24]
13. <span data-ttu-id="8f6ed-174">Skriv i följande fråga ersätter < användarnamn > med ett namn som du vill använda tooidentify hello inloggningen i hello kontexten för den här databasen (du kan ange samma värde som du gav för < login_name >, till exempel hello) och ersätter < login_name > med nytt användarnamn:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-174">Type in hello following query replacing <user_name> with a name by which you want tooidentify this login in hello context of this database (you can provide hello same value you gave for <login_name>, for example) and replacing <login_name> with your new user name:</span></span>  
   `CREATE USER <user_name> FROM LOGIN <login_name>`
14. <span data-ttu-id="8f6ed-175">Klicka på Kör:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-175">Click Run:</span></span>  
   ![graphic25][graphic25]
15. <span data-ttu-id="8f6ed-177">Nu bör du ge din nya användare hello samma roller och privilegier som den ursprungliga användaren hade.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-177">You should now provide your new user with hello same roles and privileges your original user had.</span></span>
16. <span data-ttu-id="8f6ed-178">Fortsätt tooPart 2.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-178">Continue tooPart 2.</span></span>

## <a name="part-2-stopping-hello-stream-analytics-job"></a><span data-ttu-id="8f6ed-179">Del 2: Stoppar hello Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="8f6ed-179">Part 2: Stopping hello Stream Analytics Job</span></span>
1. <span data-ttu-id="8f6ed-180">Gå toohello Stream Analytics-tillägget i hello Azure Management portal:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-180">Go toohello Stream Analytics extension on hello Azure Management portal:</span></span>  
   ![graphic26][graphic26]
2. <span data-ttu-id="8f6ed-182">Leta upp ditt jobb och gå till den:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-182">Locate your job and go into it:</span></span>  
   ![graphic27][graphic27]
3. <span data-ttu-id="8f6ed-184">Gå toohello indata fliken eller hello utdata baserat på om du rotera hello autentiseringsuppgifter på indata eller utdata.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-184">Go toohello Inputs tab or hello Outputs tab based on whether you are rotating hello credentials on an Input or on an Output.</span></span>  
   ![graphic28][graphic28]
4. <span data-ttu-id="8f6ed-186">Klicka på kommandot för hello stoppa och bekräfta hello jobbet har stoppats:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-186">Click hello Stop command and confirm hello job has stopped:</span></span>  
   <span data-ttu-id="8f6ed-187">![graphic29][graphic29] vänta hello jobbet toostop.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-187">![graphic29][graphic29] Wait for hello job toostop.</span></span>
5. <span data-ttu-id="8f6ed-188">Leta upp hello indata/utdata som du vill att toorotate autentiseringsuppgifter på och gå till den:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-188">Locate hello input/output you want toorotate credentials on and go into it:</span></span>  
   ![graphic30][graphic30]
6. <span data-ttu-id="8f6ed-190">Fortsätt tooPart 3.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-190">Proceed tooPart 3.</span></span>

## <a name="part-3-editing-hello-credentials-on-hello-stream-analytics-job"></a><span data-ttu-id="8f6ed-191">Del 3: Redigera hello autentiseringsuppgifter på hello Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="8f6ed-191">Part 3: Editing hello credentials on hello Stream Analytics Job</span></span>
### <a name="blob-storagetable-storage"></a><span data-ttu-id="8f6ed-192">BLOB storage/tabellagring</span><span class="sxs-lookup"><span data-stu-id="8f6ed-192">Blob storage/Table storage</span></span>
1. <span data-ttu-id="8f6ed-193">Hitta hello Lagringskontonyckel fältet och klistra in den nyligen skapade nyckeln till den:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-193">Find hello Storage Account Key field and paste your newly generated key into it:</span></span>  
   ![graphic31][graphic31]
2. <span data-ttu-id="8f6ed-195">Klicka på Spara hello-kommando och Bekräfta spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-195">Click hello Save command and confirm saving your changes:</span></span>  
   ![graphic32][graphic32]
3. <span data-ttu-id="8f6ed-197">En anslutningstest startar automatiskt när du sparar ändringarna, se till att har har passerat.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-197">A connection test will automatically start when you save your changes, make sure that is has successfully passed.</span></span>
4. <span data-ttu-id="8f6ed-198">Fortsätt tooPart 4.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-198">Proceed tooPart 4.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="8f6ed-199">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="8f6ed-199">Event hubs</span></span>
1. <span data-ttu-id="8f6ed-200">Hitta hello Event Hub princip nyckelfältet och klistra in den nyligen skapade nyckeln i den:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-200">Find hello Event Hub Policy Key field and paste your newly generated key into it:</span></span>  
   ![graphic33][graphic33]
2. <span data-ttu-id="8f6ed-202">Klicka på Spara hello-kommando och Bekräfta spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-202">Click hello Save command and confirm saving your changes:</span></span>  
   ![graphic34][graphic34]
3. <span data-ttu-id="8f6ed-204">En anslutningstest startar automatiskt när du sparar ändringarna, se till att den har passerat.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-204">A connection test will automatically start when you save your changes, make sure that it has successfully passed.</span></span>
4. <span data-ttu-id="8f6ed-205">Fortsätt tooPart 4.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-205">Proceed tooPart 4.</span></span>

### <a name="power-bi"></a><span data-ttu-id="8f6ed-206">Power BI</span><span class="sxs-lookup"><span data-stu-id="8f6ed-206">Power BI</span></span>
1. <span data-ttu-id="8f6ed-207">Klicka på hello förnya tillstånd:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-207">Click hello Renew authorization:</span></span>  

   ![graphic35][graphic35]
2. <span data-ttu-id="8f6ed-209">Du får följande bekräftelse hello:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-209">You will get hello following confirmation:</span></span>  

   ![graphic36][graphic36]
3. <span data-ttu-id="8f6ed-211">Klicka på Spara hello-kommando och Bekräfta spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-211">Click hello Save command and confirm saving your changes:</span></span>  
   ![graphic37][graphic37]
4. <span data-ttu-id="8f6ed-213">En anslutningstest startar automatiskt när du sparar ändringarna, se till att den passerat har.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-213">A connection test will automatically start when you save your changes, make sure it has successfully passed.</span></span>
5. <span data-ttu-id="8f6ed-214">Fortsätt tooPart 4.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-214">Proceed tooPart 4.</span></span>

### <a name="sql-database"></a><span data-ttu-id="8f6ed-215">SQL Database</span><span class="sxs-lookup"><span data-stu-id="8f6ed-215">SQL Database</span></span>
1. <span data-ttu-id="8f6ed-216">Hitta hello fälten användarnamn och lösenord och klistra in den nyligen skapade uppsättningen autentiseringsuppgifter i dem:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-216">Find hello User Name and Password fields and paste your newly created set of credentials into them:</span></span>  
   ![graphic38][graphic38]
2. <span data-ttu-id="8f6ed-218">Klicka på Spara hello-kommando och Bekräfta spara dina ändringar:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-218">Click hello Save command and confirm saving your changes:</span></span>  
   ![graphic39][graphic39]
3. <span data-ttu-id="8f6ed-220">En anslutningstest startar automatiskt när du sparar ändringarna, se till att den har passerat.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-220">A connection test will automatically start when you save your changes, make sure that it has successfully passed.</span></span>  
4. <span data-ttu-id="8f6ed-221">Fortsätt tooPart 4.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-221">Proceed tooPart 4.</span></span>

## <a name="part-4-starting-your-job-from-last-stopped-time"></a><span data-ttu-id="8f6ed-222">Del 4: Från och med jobbet senast stoppad</span><span class="sxs-lookup"><span data-stu-id="8f6ed-222">Part 4: Starting your job from last stopped time</span></span>
1. <span data-ttu-id="8f6ed-223">Navigera bort från hello in-/ utdata:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-223">Navigate away from hello Input/Output:</span></span>  
   ![graphic40][graphic40]
2. <span data-ttu-id="8f6ed-225">Klicka på hello Start-kommando:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-225">Click hello Start command:</span></span>  
   ![graphic41][graphic41]
3. <span data-ttu-id="8f6ed-227">Välj hello senast stoppad och klicka på OK:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-227">Pick hello Last Stopped Time and click OK:</span></span>  
   ![graphic42][graphic42]
4. <span data-ttu-id="8f6ed-229">Fortsätt tooPart 5.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-229">Proceed tooPart 5.</span></span>  

## <a name="part-5-removing-hello-old-set-of-credentials"></a><span data-ttu-id="8f6ed-230">Del 5: Ta bort hello gamla uppsättning autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="8f6ed-230">Part 5: Removing hello old set of credentials</span></span>
<span data-ttu-id="8f6ed-231">Den här delen är tillämpliga toohello följande indata/utdata:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-231">This part is applicable toohello following inputs/outputs:</span></span>

* <span data-ttu-id="8f6ed-232">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="8f6ed-232">Blob Storage</span></span>
* <span data-ttu-id="8f6ed-233">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="8f6ed-233">Event Hubs</span></span>
* <span data-ttu-id="8f6ed-234">SQL Database</span><span class="sxs-lookup"><span data-stu-id="8f6ed-234">SQL Database</span></span>
* <span data-ttu-id="8f6ed-235">Table Storage</span><span class="sxs-lookup"><span data-stu-id="8f6ed-235">Table Storage</span></span>

### <a name="blob-storagetable-storage"></a><span data-ttu-id="8f6ed-236">BLOB storage/tabellagring</span><span class="sxs-lookup"><span data-stu-id="8f6ed-236">Blob storage/Table storage</span></span>
<span data-ttu-id="8f6ed-237">Upprepa del 1 för hello åtkomstnyckel som tidigare användes av ditt jobb toorenew hello nu oanvända snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-237">Repeat Part 1 for hello Access Key that was previously used by your job toorenew hello now unused Access Key.</span></span>

### <a name="event-hubs"></a><span data-ttu-id="8f6ed-238">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="8f6ed-238">Event hubs</span></span>
<span data-ttu-id="8f6ed-239">Upprepa del 1 för hello nyckel som tidigare användes av ditt jobb toorenew hello nu oanvända nyckel.</span><span class="sxs-lookup"><span data-stu-id="8f6ed-239">Repeat Part 1 for hello Key that was previously used by your job toorenew hello now unused Key.</span></span>

### <a name="sql-database"></a><span data-ttu-id="8f6ed-240">SQL Database</span><span class="sxs-lookup"><span data-stu-id="8f6ed-240">SQL Database</span></span>
1. <span data-ttu-id="8f6ed-241">Gå tillbaka toohello frågefönstret från del 1 steg 7 och anger hello följande fråga, ersätta < previous_login_name > med hello användarnamn som tidigare användes av ditt jobb:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-241">Go back toohello query window from Part 1 Step 7 and type in hello following query, replacing <previous_login_name> with hello User Name that was previously used by your job:</span></span>  
   `DROP LOGIN <previous_login_name>`  
2. <span data-ttu-id="8f6ed-242">Klicka på Kör:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-242">Click Run:</span></span>  
   ![graphic43][graphic43]  

<span data-ttu-id="8f6ed-244">Du bör få hello efter bekräftelse:</span><span class="sxs-lookup"><span data-stu-id="8f6ed-244">You should get hello following confirmation:</span></span> 

    Command(s) completed successfully.

## <a name="get-help"></a><span data-ttu-id="8f6ed-245">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="8f6ed-245">Get help</span></span>
<span data-ttu-id="8f6ed-246">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="8f6ed-246">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f6ed-247">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f6ed-247">Next steps</span></span>
* [<span data-ttu-id="8f6ed-248">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8f6ed-248">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="8f6ed-249">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8f6ed-249">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="8f6ed-250">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="8f6ed-250">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="8f6ed-251">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="8f6ed-251">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="8f6ed-252">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="8f6ed-252">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

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

