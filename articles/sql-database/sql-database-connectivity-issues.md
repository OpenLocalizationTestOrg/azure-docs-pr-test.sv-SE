---
title: "aaaFix SQL-anslutningsfel tillfälligt fel | Microsoft Docs"
description: "Lär dig hur tootroubleshoot, diagnostisera och förhindra att ett fel vid anslutning till SQL eller ett tillfälligt fel i Azure SQL Database. "
keywords: "SQL-anslutning, anslutningssträngen, problem med nätverksanslutningen, tillfälligt fel, anslutningsfel"
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: efb35451-3fed-4264-bf86-72b350f67d50
ms.service: sql-database
ms.custom: develop apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: d225e610b9e88170ab53ca16d615bd07220603cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a><span data-ttu-id="ec88d-104">Felsöka, diagnostisera och förhindra SQL-anslutningsfel och tillfälliga fel för SQL Database</span><span class="sxs-lookup"><span data-stu-id="ec88d-104">Troubleshoot, diagnose, and prevent SQL connection errors and transient errors for SQL Database</span></span>
<span data-ttu-id="ec88d-105">Den här artikeln beskriver hur tooprevent, Felsök, diagnostisera och minska anslutningsfel och tillfälliga fel som ditt klientprogram påträffar när det interagerar med Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ec88d-105">This article describes how tooprevent, troubleshoot, diagnose, and mitigate connection errors and transient errors that your client application encounters when it interacts with Azure SQL Database.</span></span> <span data-ttu-id="ec88d-106">Lär dig hur tooconfigure försök logik, skapa hello anslutningssträngen och justera andra inställningar.</span><span class="sxs-lookup"><span data-stu-id="ec88d-106">Learn how tooconfigure retry logic, build hello connection string, and adjust other connection settings.</span></span>

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a><span data-ttu-id="ec88d-107">Tillfälliga fel tillfälligt</span><span class="sxs-lookup"><span data-stu-id="ec88d-107">Transient errors (transient faults)</span></span>
<span data-ttu-id="ec88d-108">Ett tillfälligt fel - också tillfälliga fel – har en underliggande orsaken som kommer snart att lösas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ec88d-108">A transient error - also, transient fault - has an underlying cause that will soon resolve itself.</span></span> <span data-ttu-id="ec88d-109">En tillfällig tillfälliga fel beror på när hello Azure system snabbt skiftar maskinvara resurser toobetter belastningsutjämna olika arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="ec88d-109">An occasional cause of transient errors is when hello Azure system quickly shifts hardware resources toobetter load-balance various workloads.</span></span> <span data-ttu-id="ec88d-110">De flesta av dessa händelser för omkonfiguration Slutför ofta i mindre än 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="ec88d-110">Most of these reconfiguration events often complete in less than 60 seconds.</span></span> <span data-ttu-id="ec88d-111">Under det angivna tidsintervallet omkonfiguration kanske anslutningen utfärdar tooAzure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="ec88d-111">During this reconfiguration time span, you may have connectivity issues tooAzure SQL Database.</span></span> <span data-ttu-id="ec88d-112">Program som ansluter tooAzure SQL-databas ska vara inbyggda tooexpect felen tillfälligt hantera dem genom att implementera försök logiken i koden i stället för att visa dem toousers som programfel.</span><span class="sxs-lookup"><span data-stu-id="ec88d-112">Applications connecting tooAzure SQL Database should be built tooexpect these transient errors, handle them by implementing retry logic in their code instead of surfacing them toousers as application errors.</span></span>

<span data-ttu-id="ec88d-113">Om klientprogrammet använder ADO.NET, programmet meddelade om hello tillfälligt fel av hello throw av en **SqlException**.</span><span class="sxs-lookup"><span data-stu-id="ec88d-113">If your client program is using ADO.NET, your program is told about hello transient error by hello throw of an **SqlException**.</span></span> <span data-ttu-id="ec88d-114">Hej **nummer** egenskapen kan jämföras mot hello lista över tillfälliga fel hello övre delen av avsnittet hello: [SQL-felkoder för SQL Database-klientprogram](sql-database-develop-error-messages.md).</span><span class="sxs-lookup"><span data-stu-id="ec88d-114">hello **Number** property can be compared against hello list of transient errors near hello top of hello topic: [SQL error codes for SQL Database client applications](sql-database-develop-error-messages.md).</span></span>

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a><span data-ttu-id="ec88d-115">Anslutningen jämfört med kommandot</span><span class="sxs-lookup"><span data-stu-id="ec88d-115">Connection versus command</span></span>
<span data-ttu-id="ec88d-116">Du kommer försök hello SQL-anslutning eller upprätta igen, beroende på hello följande:</span><span class="sxs-lookup"><span data-stu-id="ec88d-116">You'll retry hello SQL connection or establish it again, depending on hello following:</span></span>

* <span data-ttu-id="ec88d-117">**Ett tillfälligt fel inträffar under ett anslutningsförsök**: hello anslutningen ska göras efter att förskjuta för några sekunder.</span><span class="sxs-lookup"><span data-stu-id="ec88d-117">**A transient error occurs during a connection try**: hello connection should be retried after delaying for several seconds.</span></span>
* <span data-ttu-id="ec88d-118">**Ett tillfälligt fel inträffar under en SQL-kommandot query**: hello kommandot bör inte göras omedelbart.</span><span class="sxs-lookup"><span data-stu-id="ec88d-118">**A transient error occurs during a SQL query command**: hello command should not be immediately retried.</span></span> <span data-ttu-id="ec88d-119">I stället efter en stund hello anslutningen bör nyligen upprättas.</span><span class="sxs-lookup"><span data-stu-id="ec88d-119">Instead, after a delay, hello connection should be freshly established.</span></span> <span data-ttu-id="ec88d-120">Sedan kan hello-kommandot göras.</span><span class="sxs-lookup"><span data-stu-id="ec88d-120">Then hello command can be retried.</span></span>

<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a><span data-ttu-id="ec88d-121">Logik för omprövning av tillfälliga fel</span><span class="sxs-lookup"><span data-stu-id="ec88d-121">Retry logic for transient errors</span></span>
<span data-ttu-id="ec88d-122">Klientprogram som kan uppstår ett tillfälligt fel är mer robusta när de innehåller logik för omprövning.</span><span class="sxs-lookup"><span data-stu-id="ec88d-122">Client programs that occasionally encounter a transient error are more robust when they contain retry logic.</span></span>

<span data-ttu-id="ec88d-123">När ditt program kommunicerar med Azure SQL Database via en 3 part mellanprogram, fråga med hello leverantör om hello mellanprogram innehåller logik för omprövning av tillfälliga fel.</span><span class="sxs-lookup"><span data-stu-id="ec88d-123">When your program communicates with Azure SQL Database through a 3rd party middleware, inquire with hello vendor whether hello middleware contains retry logic for transient errors.</span></span>

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a><span data-ttu-id="ec88d-124">Principer för nya försök</span><span class="sxs-lookup"><span data-stu-id="ec88d-124">Principles for retry</span></span>
* <span data-ttu-id="ec88d-125">Ett försök tooopen en anslutning ska göras om hello felet är tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="ec88d-125">An attempt tooopen a connection should be retried if hello error is transient.</span></span>
* <span data-ttu-id="ec88d-126">En SQL SELECT-instruktion som misslyckas med ett tillfälligt fel bör inte göras direkt.</span><span class="sxs-lookup"><span data-stu-id="ec88d-126">A SQL SELECT statement that fails with a transient error should not be retried directly.</span></span>
  
  * <span data-ttu-id="ec88d-127">I stället skapa en ny anslutning och försök sedan hello väljer.</span><span class="sxs-lookup"><span data-stu-id="ec88d-127">Instead, establish a fresh connection, and then retry hello SELECT.</span></span>
* <span data-ttu-id="ec88d-128">När en UPDATE SQL-instruktion misslyckas med ett tillfälligt fel bör en ny anslutning upprättas innan hello UPPDATERINGEN igen.</span><span class="sxs-lookup"><span data-stu-id="ec88d-128">When a SQL UPDATE statement fails with a transient error, a fresh connection should be established before hello UPDATE is retried.</span></span>
  
  * <span data-ttu-id="ec88d-129">logik för omprövning av hello måste se till att hello hela Databastransaktionen har slutförts eller hello hela transaktionen återställs.</span><span class="sxs-lookup"><span data-stu-id="ec88d-129">hello retry logic must ensure that either hello entire database transaction completed, or that hello entire transaction is rolled back.</span></span>

#### <a name="other-considerations-for-retry"></a><span data-ttu-id="ec88d-130">Andra överväganden för nya försök</span><span class="sxs-lookup"><span data-stu-id="ec88d-130">Other considerations for retry</span></span>
* <span data-ttu-id="ec88d-131">En kommandofil som startas automatiskt efter arbetstid och som kommer att slutföras innan morgon har råd toovery tålamod med lång tidsintervall mellan dess nya försök.</span><span class="sxs-lookup"><span data-stu-id="ec88d-131">A batch program that is automatically started after work hours, and which will complete before morning, can afford toovery patient with long time intervals between its retry attempts.</span></span>
* <span data-ttu-id="ec88d-132">Ett gränssnitt användarprogram bör hänsyn till hello mänsklig tendens toogive upp efter för lång väntan.</span><span class="sxs-lookup"><span data-stu-id="ec88d-132">A user interface program should account for hello human tendency toogive up after too long a wait.</span></span>
  
  * <span data-ttu-id="ec88d-133">Dock får hello lösning inte vara tooretry några sekunders eftersom principen kan översvämma hello system med förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="ec88d-133">However, hello solution must not be tooretry every few seconds, because that policy can flood hello system with requests.</span></span>

#### <a name="interval-increase-between-retries"></a><span data-ttu-id="ec88d-134">Öka intervall mellan försök</span><span class="sxs-lookup"><span data-stu-id="ec88d-134">Interval increase between retries</span></span>
<span data-ttu-id="ec88d-135">Vi rekommenderar att du fördröjning för 5 sekunder innan din första försök.</span><span class="sxs-lookup"><span data-stu-id="ec88d-135">We recommend that you delay for 5 seconds before your first retry.</span></span> <span data-ttu-id="ec88d-136">Försöker igen efter en kortare än 5 sekunder fördröjning risker överväldigande hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ec88d-136">Retrying after a delay shorter than 5 seconds risks overwhelming hello cloud service.</span></span> <span data-ttu-id="ec88d-137">För varje efterföljande försök ska hello fördröjning växa exponentiellt, in tooa högst 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="ec88d-137">For each subsequent retry hello delay should grow exponentially, up tooa maximum of 60 seconds.</span></span>

<span data-ttu-id="ec88d-138">En beskrivning av hello *blockerande period* för klienter som använder ADO.NET finns i [SQL Server anslutning poolning (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec88d-138">A discussion of hello *blocking period* for clients that use ADO.NET is available in [SQL Server Connection Pooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).</span></span>

<span data-ttu-id="ec88d-139">Du kan också tooset ett maximalt antal försök innan programmet hello själv avslutas.</span><span class="sxs-lookup"><span data-stu-id="ec88d-139">You might also want tooset a maximum number of retries before hello program self-terminates.</span></span>

#### <a name="code-samples-with-retry-logic"></a><span data-ttu-id="ec88d-140">Kodexempel med logik</span><span class="sxs-lookup"><span data-stu-id="ec88d-140">Code samples with retry logic</span></span>
<span data-ttu-id="ec88d-141">Kodexempel med logik för omprövning, på en mängd olika programmeringsspråk, finns på:</span><span class="sxs-lookup"><span data-stu-id="ec88d-141">Code samples with retry logic, in a variety of programming languages, are available at:</span></span>

* [<span data-ttu-id="ec88d-142">Anslutningsbibliotek för SQL Database och SQL Server</span><span class="sxs-lookup"><span data-stu-id="ec88d-142">Connection libraries for SQL Database and SQL Server</span></span>](sql-database-libraries.md)

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a><span data-ttu-id="ec88d-143">Testa din logik</span><span class="sxs-lookup"><span data-stu-id="ec88d-143">Test your retry logic</span></span>
<span data-ttu-id="ec88d-144">tootest logik för omprövning måste du simulera eller orsaka fel än kan korrigeras medan programmet körs.</span><span class="sxs-lookup"><span data-stu-id="ec88d-144">tootest your retry logic, you must simulate or cause an error than can be corrected while your program is still running.</span></span>

##### <a name="test-by-disconnecting-from-hello-network"></a><span data-ttu-id="ec88d-145">Testa genom att koppla från hello nätverk</span><span class="sxs-lookup"><span data-stu-id="ec88d-145">Test by disconnecting from hello network</span></span>
<span data-ttu-id="ec88d-146">Ett sätt som du kan testa din logik är toodisconnect klientdatorn från hello nätverk när hello programmet körs.</span><span class="sxs-lookup"><span data-stu-id="ec88d-146">One way you can test your retry logic is toodisconnect your client computer from hello network while hello program is running.</span></span> <span data-ttu-id="ec88d-147">hello-felet är:</span><span class="sxs-lookup"><span data-stu-id="ec88d-147">hello error will be:</span></span>

* <span data-ttu-id="ec88d-148">**SqlException.Number** = 11001</span><span class="sxs-lookup"><span data-stu-id="ec88d-148">**SqlException.Number** = 11001</span></span>
* <span data-ttu-id="ec88d-149">Meddelandet: ”värden är inte känd”</span><span class="sxs-lookup"><span data-stu-id="ec88d-149">Message: "No such host is known"</span></span>

<span data-ttu-id="ec88d-150">Som en del av hello första återförsöket försök, kan programmet korrigera hello felstavat och försök sedan tooconnect.</span><span class="sxs-lookup"><span data-stu-id="ec88d-150">As part of hello first retry attempt, your program can correct hello misspelling, and then attempt tooconnect.</span></span>

<span data-ttu-id="ec88d-151">Det här praktiska toomake, du koppla från datorn från nätverket hello innan du startar programmet.</span><span class="sxs-lookup"><span data-stu-id="ec88d-151">toomake this practical, you unplug your computer from hello network before you start your program.</span></span> <span data-ttu-id="ec88d-152">Sedan identifierar programmet en parameter för körning som gör att hello program:</span><span class="sxs-lookup"><span data-stu-id="ec88d-152">Then your program recognizes a run time parameter that causes hello program to:</span></span>

1. <span data-ttu-id="ec88d-153">Lägg till 11001 tooits lista över fel tooconsider tillfälligt som tillfällig.</span><span class="sxs-lookup"><span data-stu-id="ec88d-153">Temporarily add 11001 tooits list of errors tooconsider as transient.</span></span>
2. <span data-ttu-id="ec88d-154">Försök den första anslutningen som vanligt.</span><span class="sxs-lookup"><span data-stu-id="ec88d-154">Attempt its first connection as usual.</span></span>
3. <span data-ttu-id="ec88d-155">Ta bort 11001 hello listan när hello fel har inträffat.</span><span class="sxs-lookup"><span data-stu-id="ec88d-155">After hello error is caught, remove 11001 from hello list.</span></span>
4. <span data-ttu-id="ec88d-156">Visa ett meddelande om hello tooplug hello dator i hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="ec88d-156">Display a message telling hello user tooplug hello computer into hello network.</span></span>
   * <span data-ttu-id="ec88d-157">Pausa körning med hjälp av antingen hello **Console.ReadLine** metod eller en dialogruta med en OK-knapp.</span><span class="sxs-lookup"><span data-stu-id="ec88d-157">Pause further execution by using either hello **Console.ReadLine** method or a dialog with an OK button.</span></span> <span data-ttu-id="ec88d-158">hello användaren trycker på RETUR hello när hello datorn ansluten till nätverket hello.</span><span class="sxs-lookup"><span data-stu-id="ec88d-158">hello user presses hello Enter key after hello computer plugged into hello network.</span></span>
5. <span data-ttu-id="ec88d-159">Försök igen tooconnect, förväntas lyckades.</span><span class="sxs-lookup"><span data-stu-id="ec88d-159">Attempt again tooconnect, expecting success.</span></span>

##### <a name="test-by-misspelling-hello-database-name-when-connecting"></a><span data-ttu-id="ec88d-160">Testa genom felstavat hello databasnamnet vid anslutning</span><span class="sxs-lookup"><span data-stu-id="ec88d-160">Test by misspelling hello database name when connecting</span></span>
<span data-ttu-id="ec88d-161">Programmet kan ändamålsenligt fel hello användarnamn innan hello första anslutningsförsöket.</span><span class="sxs-lookup"><span data-stu-id="ec88d-161">Your program can purposely misspell hello user name before hello first connection attempt.</span></span> <span data-ttu-id="ec88d-162">hello-felet är:</span><span class="sxs-lookup"><span data-stu-id="ec88d-162">hello error will be:</span></span>

* <span data-ttu-id="ec88d-163">**SqlException.Number** = 18456</span><span class="sxs-lookup"><span data-stu-id="ec88d-163">**SqlException.Number** = 18456</span></span>
* <span data-ttu-id="ec88d-164">Meddelandet: ”inloggning misslyckades för användaren 'WRONG_MyUserName'”.</span><span class="sxs-lookup"><span data-stu-id="ec88d-164">Message: "Login failed for user 'WRONG_MyUserName'."</span></span>

<span data-ttu-id="ec88d-165">Som en del av hello första återförsöket försök, kan programmet korrigera hello felstavat och försök sedan tooconnect.</span><span class="sxs-lookup"><span data-stu-id="ec88d-165">As part of hello first retry attempt, your program can correct hello misspelling, and then attempt tooconnect.</span></span>

<span data-ttu-id="ec88d-166">Det här praktiska toomake, programmet kände igen en parameter för körning som gör att hello program:</span><span class="sxs-lookup"><span data-stu-id="ec88d-166">toomake this practical, your program could recognize a run time parameter that causes hello program to:</span></span>

1. <span data-ttu-id="ec88d-167">Lägg till 18456 tooits lista över fel tooconsider tillfälligt som tillfällig.</span><span class="sxs-lookup"><span data-stu-id="ec88d-167">Temporarily add 18456 tooits list of errors tooconsider as transient.</span></span>
2. <span data-ttu-id="ec88d-168">Lägg till 'WRONG_' toohello användarnamn ändamålsenligt.</span><span class="sxs-lookup"><span data-stu-id="ec88d-168">Purposely add 'WRONG_' toohello user name.</span></span>
3. <span data-ttu-id="ec88d-169">Ta bort 18456 hello listan när hello fel har inträffat.</span><span class="sxs-lookup"><span data-stu-id="ec88d-169">After hello error is caught, remove 18456 from hello list.</span></span>
4. <span data-ttu-id="ec88d-170">Ta bort 'WRONG_' från hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="ec88d-170">Remove 'WRONG_' from hello user name.</span></span>
5. <span data-ttu-id="ec88d-171">Försök igen tooconnect, förväntas lyckades.</span><span class="sxs-lookup"><span data-stu-id="ec88d-171">Attempt again tooconnect, expecting success.</span></span>

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a><span data-ttu-id="ec88d-172">.NET SqlConnection parametrar för anslutningen försök igen</span><span class="sxs-lookup"><span data-stu-id="ec88d-172">.NET SqlConnection parameters for connection retry</span></span>
<span data-ttu-id="ec88d-173">Om klientprogrammet ansluter tootooAzure SQL-databas med hjälp av .NET Framework-klass för hello **System.Data.SqlClient.SqlConnection**, bör du använda .NET 4.6.1 eller senare (eller .NET Core) så att du kan utnyttja funktionen anslutning försök igen.</span><span class="sxs-lookup"><span data-stu-id="ec88d-173">If your client program connects tootooAzure SQL Database by using hello .NET Framework class **System.Data.SqlClient.SqlConnection**, you should use .NET 4.6.1 or later (or .NET Core) so you can leverage its connection retry feature.</span></span> <span data-ttu-id="ec88d-174">Information om hello funktionen [här](http://go.microsoft.com/fwlink/?linkid=393996).</span><span class="sxs-lookup"><span data-stu-id="ec88d-174">Details of hello feature are [here](http://go.microsoft.com/fwlink/?linkid=393996).</span></span>

<!--
2015-11-30, FwLink 393996 points toodn632678.aspx, which links tooa downloadable .docx related tooSqlClient and SQL Server 2014.
-->


<span data-ttu-id="ec88d-175">När du skapar hello [anslutningssträngen](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) för din **SqlConnection** objekt, bör du samordna hello värden mellan hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ec88d-175">When you build hello [connection string](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) for your **SqlConnection** object, you should coordinate hello values among hello following parameters:</span></span>

* <span data-ttu-id="ec88d-176">ConnectRetryCount &nbsp; &nbsp; *(standardvärdet är 1. Intervallet är 0 och 255.)*</span><span class="sxs-lookup"><span data-stu-id="ec88d-176">ConnectRetryCount &nbsp;&nbsp;*(Default is 1. Range is 0 through 255.)*</span></span>
* <span data-ttu-id="ec88d-177">ConnectRetryInterval &nbsp; &nbsp; *(standard är 1 sekund. Intervall är 1 till 60).*</span><span class="sxs-lookup"><span data-stu-id="ec88d-177">ConnectRetryInterval &nbsp;&nbsp;*(Default is 1 second. Range is 1 through 60.)*</span></span>
* <span data-ttu-id="ec88d-178">Timeout för anslutningen &nbsp; &nbsp; *(standardvärdet är 15 sekunder. Intervallet är 0 och 2147483647)*</span><span class="sxs-lookup"><span data-stu-id="ec88d-178">Connection Timeout &nbsp;&nbsp;*(Default is 15 seconds. Range is 0 through 2147483647)*</span></span>

<span data-ttu-id="ec88d-179">Mer specifikt bör valda värdena Se hello efter likhet SANT:</span><span class="sxs-lookup"><span data-stu-id="ec88d-179">Specifically, your chosen values should make hello following equality true:</span></span>

* <span data-ttu-id="ec88d-180">Timeout för anslutningen = ConnectRetryCount * ConnectionRetryInterval</span><span class="sxs-lookup"><span data-stu-id="ec88d-180">Connection Timeout = ConnectRetryCount * ConnectionRetryInterval</span></span>

<span data-ttu-id="ec88d-181">Exempel: om hello antal = 3 och intervall = 10 sekunder timeout för bara 29 sekunder inte skulle helt ger hello system tillräckligt med tid för dess 3 och slutgiltig försök vid anslutning: 29 < 3 * 10.</span><span class="sxs-lookup"><span data-stu-id="ec88d-181">For example, if hello count = 3, and interval = 10 seconds, a timeout of only 29 seconds would not quite give hello system enough time for its 3rd and final retry at connecting: 29 < 3 * 10.</span></span>

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a><span data-ttu-id="ec88d-182">Anslutningen jämfört med kommandot</span><span class="sxs-lookup"><span data-stu-id="ec88d-182">Connection versus command</span></span>
<span data-ttu-id="ec88d-183">Hej **ConnectRetryCount** och **ConnectRetryInterval** -parametrar kan din **SqlConnection** objekt försök hello anslutningsåtgärden utan uppmanar eller stör din program, till exempel returnera kontrollen tooyour program.</span><span class="sxs-lookup"><span data-stu-id="ec88d-183">hello **ConnectRetryCount** and **ConnectRetryInterval** parameters let your **SqlConnection** object retry hello connect operation without telling or bothering your program, such as returning control tooyour program.</span></span> <span data-ttu-id="ec88d-184">hello återförsök kan inträffa i följande situationer hello:</span><span class="sxs-lookup"><span data-stu-id="ec88d-184">hello retries can occur in hello following situations:</span></span>

* <span data-ttu-id="ec88d-185">mySqlConnection.Open metodanrop</span><span class="sxs-lookup"><span data-stu-id="ec88d-185">mySqlConnection.Open method call</span></span>
* <span data-ttu-id="ec88d-186">mySqlConnection.Execute metodanrop</span><span class="sxs-lookup"><span data-stu-id="ec88d-186">mySqlConnection.Execute method call</span></span>

<span data-ttu-id="ec88d-187">Det finns en liten finess.</span><span class="sxs-lookup"><span data-stu-id="ec88d-187">There is a subtlety.</span></span> <span data-ttu-id="ec88d-188">Om ett tillfälligt fel uppstår när din *frågan* körs din **SqlConnection** objekt har inte försök hello åtgärden med anslutning och den verkligen inte köra frågan igen.</span><span class="sxs-lookup"><span data-stu-id="ec88d-188">If a transient error occurs while your *query* is being executed, your **SqlConnection** object does not retry hello connect operation, and it certainly does not retry your query.</span></span> <span data-ttu-id="ec88d-189">Dock **SqlConnection** snabbt kontrollerar hello anslutningen innan du skickar frågan för körning.</span><span class="sxs-lookup"><span data-stu-id="ec88d-189">However, **SqlConnection** very quickly checks hello connection before sending your query for execution.</span></span> <span data-ttu-id="ec88d-190">Om hello Snabbkontrollen upptäcker ett anslutningsproblem, **SqlConnection** hello försök ansluta igen.</span><span class="sxs-lookup"><span data-stu-id="ec88d-190">If hello quick check detects a connection problem, **SqlConnection** retries hello connect operation.</span></span> <span data-ttu-id="ec88d-191">Om hello försök lyckas, skickas frågan för du för körning.</span><span class="sxs-lookup"><span data-stu-id="ec88d-191">If hello retry succeeds, you query is sent for execution.</span></span>

#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a><span data-ttu-id="ec88d-192">ConnectRetryCount kombineras med logik för omprövning av programmet?</span><span class="sxs-lookup"><span data-stu-id="ec88d-192">Should ConnectRetryCount be combined with application retry logic?</span></span>
<span data-ttu-id="ec88d-193">Anta att programmet har robust anpassade logik.</span><span class="sxs-lookup"><span data-stu-id="ec88d-193">Suppose your application has robust custom retry logic.</span></span> <span data-ttu-id="ec88d-194">Det kan försök hello anslutningsåtgärden 4 gånger.</span><span class="sxs-lookup"><span data-stu-id="ec88d-194">It might retry hello connect operation 4 times.</span></span> <span data-ttu-id="ec88d-195">Om du lägger till **ConnectRetryInterval** och **ConnectRetryCount** = 3 tooyour anslutningssträngen, ökar hello försök antal too4 * 3 = 12 återförsök.</span><span class="sxs-lookup"><span data-stu-id="ec88d-195">If you add **ConnectRetryInterval** and **ConnectRetryCount** =3 tooyour connection string, you will increase hello retry count too4 * 3 = 12 retries.</span></span> <span data-ttu-id="ec88d-196">Du kan inte avser sådana ett stort antal återförsök.</span><span class="sxs-lookup"><span data-stu-id="ec88d-196">You might not intend such a high number of retries.</span></span>

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-tooazure-sql-database"></a><span data-ttu-id="ec88d-197">Anslutningar tooAzure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="ec88d-197">Connections tooAzure SQL Database</span></span>
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a><span data-ttu-id="ec88d-198">Anslutningen: Anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="ec88d-198">Connection: Connection string</span></span>
<span data-ttu-id="ec88d-199">hello anslutningssträngen som är nödvändiga för att ansluta tooAzure SQL-databas är skiljer sig från hello sträng för att ansluta tooMicrosoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ec88d-199">hello connection string necessary for connecting tooAzure SQL Database is slightly different from hello string for connecting tooMicrosoft SQL Server.</span></span> <span data-ttu-id="ec88d-200">Du kan kopiera hello anslutningssträngen för databasen från hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ec88d-200">You can copy hello connection string for your database from hello [Azure portal](https://portal.azure.com/).</span></span>

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a><span data-ttu-id="ec88d-201">Anslutning: IP-adress för</span><span class="sxs-lookup"><span data-stu-id="ec88d-201">Connection: IP address</span></span>
<span data-ttu-id="ec88d-202">Du måste konfigurera hello SQL Database server tooaccept kommunikation från hello IP-adress för hello-dator som är värd för dina klientprogram.</span><span class="sxs-lookup"><span data-stu-id="ec88d-202">You must configure hello SQL Database server tooaccept communication from hello IP address of hello computer that hosts your client program.</span></span> <span data-ttu-id="ec88d-203">Du kan göra detta genom att redigera hello brandväggsinställningar via hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ec88d-203">You do this by editing hello firewall settings through hello [Azure portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="ec88d-204">Om du glömmer tooconfigure hello IP-adress kan misslyckas programmet med en praktisk felmeddelande hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="ec88d-204">If you forget tooconfigure hello IP address, your program will fail with a handy error message that states hello necessary IP address.</span></span>

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

<span data-ttu-id="ec88d-205">Mer information finns: [så här: konfigurera brandväggsinställningar på SQL-databas](sql-database-configure-firewall-settings.md)</span><span class="sxs-lookup"><span data-stu-id="ec88d-205">For more information, see: [How to: Configure firewall settings on SQL Database](sql-database-configure-firewall-settings.md)</span></span>

<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a><span data-ttu-id="ec88d-206">Anslutning: portar</span><span class="sxs-lookup"><span data-stu-id="ec88d-206">Connection: Ports</span></span>
<span data-ttu-id="ec88d-207">Vanligtvis behöver du bara tooensure att port 1433 är öppen för utgående kommunikation på hello-dator som värd-klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="ec88d-207">Typically you only need tooensure that port 1433 is open for outbound communication, on hello computer that hosts you client program.</span></span>

<span data-ttu-id="ec88d-208">Till exempel när klientprogrammet finns på en dator med Windows kan hello Windows-brandväggen på hello värd du tooopen port 1433:</span><span class="sxs-lookup"><span data-stu-id="ec88d-208">For example, when your client program is hosted on a Windows computer, hello Windows Firewall on hello host enables you tooopen port 1433:</span></span>

1. <span data-ttu-id="ec88d-209">Öppna hello Kontrollpanelen</span><span class="sxs-lookup"><span data-stu-id="ec88d-209">Open hello Control Panel</span></span>
2. <span data-ttu-id="ec88d-210">&gt;Alla objekt på Kontrollpanelen</span><span class="sxs-lookup"><span data-stu-id="ec88d-210">&gt; All Control Panel Items</span></span>
3. <span data-ttu-id="ec88d-211">&gt;Windows-brandväggen</span><span class="sxs-lookup"><span data-stu-id="ec88d-211">&gt; Windows Firewall</span></span>
4. <span data-ttu-id="ec88d-212">&gt;Avancerade inställningar</span><span class="sxs-lookup"><span data-stu-id="ec88d-212">&gt; Advanced Settings</span></span>
5. <span data-ttu-id="ec88d-213">&gt;Utgående regler</span><span class="sxs-lookup"><span data-stu-id="ec88d-213">&gt; Outbound Rules</span></span>
6. <span data-ttu-id="ec88d-214">&gt;Åtgärder</span><span class="sxs-lookup"><span data-stu-id="ec88d-214">&gt; Actions</span></span>
7. <span data-ttu-id="ec88d-215">&gt;Ny regel</span><span class="sxs-lookup"><span data-stu-id="ec88d-215">&gt; New Rule</span></span>

<span data-ttu-id="ec88d-216">Om klientprogrammet finns på en Azure-dator (VM), bör du läsa:</span><span class="sxs-lookup"><span data-stu-id="ec88d-216">If your client program is hosted on an Azure virtual machine (VM), you should read:</span></span><br/><span data-ttu-id="ec88d-217">[Portar utöver 1433 för ADO.NET 4.5 och SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span><span class="sxs-lookup"><span data-stu-id="ec88d-217">[Ports beyond 1433 for ADO.NET 4.5 and SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span></span>

<span data-ttu-id="ec88d-218">Mer bakgrundsinformation om cofiguration portar och IP-adress, se: [Azure SQL Database-brandvägg](sql-database-firewall-configure.md)</span><span class="sxs-lookup"><span data-stu-id="ec88d-218">For background information about cofiguration of ports and IP address, see: [Azure SQL Database firewall](sql-database-firewall-configure.md)</span></span>

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a><span data-ttu-id="ec88d-219">Anslutning: ADO.NET 4.6.1</span><span class="sxs-lookup"><span data-stu-id="ec88d-219">Connection: ADO.NET 4.6.1</span></span>
<span data-ttu-id="ec88d-220">Om programmet använder ADO.NET-klasser som **System.Data.SqlClient.SqlConnection** tooconnect tooAzure SQL-databas, rekommenderar vi att du använder .NET Framework version 4.6.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ec88d-220">If your program uses ADO.NET classes like **System.Data.SqlClient.SqlConnection** tooconnect tooAzure SQL Database, we recommend that you use .NET Framework version 4.6.1 or higher.</span></span>

<span data-ttu-id="ec88d-221">ADO.NET 4.6.1:</span><span class="sxs-lookup"><span data-stu-id="ec88d-221">ADO.NET 4.6.1:</span></span>

* <span data-ttu-id="ec88d-222">För Azure SQL-databas finns förbättrad tillförlitlighet när du öppnar en anslutning med hjälp av hello **SqlConnection.Open** metod.</span><span class="sxs-lookup"><span data-stu-id="ec88d-222">For Azure SQL Database, there is improved reliability when you open a connection by using hello **SqlConnection.Open** method.</span></span> <span data-ttu-id="ec88d-223">Hej **öppna** metoden omfattar nu bästa prestanda försök autentiseringsmekanismer i svaret tootransient fel, för vissa fel inom tidsgränsen för hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="ec88d-223">hello **Open** method now incorporates best effort retry mechanisms in response tootransient faults, for certain errors within hello Connection Timeout period.</span></span>
* <span data-ttu-id="ec88d-224">Stöder anslutningspooler.</span><span class="sxs-lookup"><span data-stu-id="ec88d-224">Supports connection pooling.</span></span> <span data-ttu-id="ec88d-225">Detta inkluderar en effektiv kontroll hello anslutning objekt som den ger programmet fungerar.</span><span class="sxs-lookup"><span data-stu-id="ec88d-225">This includes an efficient verification that hello connection object it gives your program is functioning.</span></span>

<span data-ttu-id="ec88d-226">Vi rekommenderar att programmet tillfälligt stänger hello-anslutning när du använder den inte omedelbart när du använder ett anslutningsobjekt från en anslutningspool.</span><span class="sxs-lookup"><span data-stu-id="ec88d-226">When you use a connection object from a connection pool, we recommend that your program temporarily close hello connection when not immediately using it.</span></span> <span data-ttu-id="ec88d-227">Öppna en anslutning inte är dyr hello sättet att skapa en ny anslutning.</span><span class="sxs-lookup"><span data-stu-id="ec88d-227">Re-opening a connection is not expensive hello way creating a new connection is.</span></span>

<span data-ttu-id="ec88d-228">Om du använder ADO.NET 4.0 eller tidigare, rekommenderar vi att du uppgraderar toohello senaste ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="ec88d-228">If you are using ADO.NET 4.0 or earlier, we recommend that you upgrade toohello latest ADO.NET.</span></span>

* <span data-ttu-id="ec88d-229">Från och med November 2015, kan du [hämta ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec88d-229">As of November 2015, you can [download ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).</span></span>

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a><span data-ttu-id="ec88d-230">Diagnostik</span><span class="sxs-lookup"><span data-stu-id="ec88d-230">Diagnostics</span></span>
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a><span data-ttu-id="ec88d-231">Diagnostik: Testa om verktyg kan ansluta</span><span class="sxs-lookup"><span data-stu-id="ec88d-231">Diagnostics: Test whether utilities can connect</span></span>
<span data-ttu-id="ec88d-232">Om programmet misslyckas tooconnect tooAzure SQL-databas, kan en diagnostik tootry tooconnect med ett program.</span><span class="sxs-lookup"><span data-stu-id="ec88d-232">If your program is failing tooconnect tooAzure SQL Database, one diagnostic option is tootry tooconnect with a utility program.</span></span> <span data-ttu-id="ec88d-233">Helst hello verktyget skulle ansluta med hjälp av hello samma bibliotek som används i programmet.</span><span class="sxs-lookup"><span data-stu-id="ec88d-233">Ideally hello utility would connect by using hello same library that your program uses.</span></span>

<span data-ttu-id="ec88d-234">Du kan försöka verktygen på alla Windows-datorer:</span><span class="sxs-lookup"><span data-stu-id="ec88d-234">On any Windows computer, you can try these utilities:</span></span>

* <span data-ttu-id="ec88d-235">SQL Server Management Studio (ssms.exe) som ansluter med hjälp av ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="ec88d-235">SQL Server Management Studio (ssms.exe), which connects by using ADO.NET.</span></span>
* <span data-ttu-id="ec88d-236">SQLCMD.exe som ansluter med hjälp av [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec88d-236">sqlcmd.exe, which connects by using [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).</span></span>

<span data-ttu-id="ec88d-237">När du är ansluten, testa om ett kort SQL SELECT-frågan fungerar.</span><span class="sxs-lookup"><span data-stu-id="ec88d-237">Once connected, test whether a short SQL SELECT query works.</span></span>

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-hello-open-ports"></a><span data-ttu-id="ec88d-238">Diagnostik: Kontrollera hello öppna portar</span><span class="sxs-lookup"><span data-stu-id="ec88d-238">Diagnostics: Check hello open ports</span></span>
<span data-ttu-id="ec88d-239">Anta att du misstänker att anslutningsförsök misslyckas på grund av problem med tooport.</span><span class="sxs-lookup"><span data-stu-id="ec88d-239">Suppose you suspect that connection attempts are failing due tooport issues.</span></span> <span data-ttu-id="ec88d-240">Du kan köra ett verktyg som rapporterar hello portkonfigurationer på datorn.</span><span class="sxs-lookup"><span data-stu-id="ec88d-240">On your computer you can run a utility that reports on hello port configurations.</span></span>

<span data-ttu-id="ec88d-241">Följande verktyg kan vara användbart på Linux hello:</span><span class="sxs-lookup"><span data-stu-id="ec88d-241">On Linux hello following utilities might be helpful:</span></span>

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * <span data-ttu-id="ec88d-242">(Ändra hello exempel värdet toobe IP-adress.)</span><span class="sxs-lookup"><span data-stu-id="ec88d-242">(Change hello example value toobe your IP address.)</span></span>

<span data-ttu-id="ec88d-243">På Windows hello [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) verktyg kan vara till hjälp.</span><span class="sxs-lookup"><span data-stu-id="ec88d-243">On Windows hello [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) utility might be helpful.</span></span> <span data-ttu-id="ec88d-244">Här är ett exempel körning som efterfrågas hello port situationen på en Azure SQL Database-server och som kördes på en bärbar dator:</span><span class="sxs-lookup"><span data-stu-id="ec88d-244">Here is an example execution that queried hello port situation on an Azure SQL Database server, and which was run on a laptop computer:</span></span>

```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting tooresolve name tooIP address...
Name resolved too23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a><span data-ttu-id="ec88d-245">Diagnostik: Logga din fel</span><span class="sxs-lookup"><span data-stu-id="ec88d-245">Diagnostics: Log your errors</span></span>
<span data-ttu-id="ec88d-246">Ett tillfälligt problem diagnostiserats ibland bäst av identifiering av ett allmänt mönster över dagar eller veckor.</span><span class="sxs-lookup"><span data-stu-id="ec88d-246">An intermittent problem is sometimes best diagnosed by detection of a general pattern over days or weeks.</span></span>

<span data-ttu-id="ec88d-247">Klienten kan hjälpa en diagnos genom att logga alla fel som påträffas.</span><span class="sxs-lookup"><span data-stu-id="ec88d-247">Your client can assist in a diagnosis by logging all errors it encounters.</span></span> <span data-ttu-id="ec88d-248">Du kanske kan toocorrelate hello loggposter med Feldata som Azure SQL Database loggar själva internt.</span><span class="sxs-lookup"><span data-stu-id="ec88d-248">You might be able toocorrelate hello log entries with error data that Azure SQL Database logs itself internally.</span></span>

<span data-ttu-id="ec88d-249">Enterprise-biblioteket 6 (EntLib60) ger hanterad klasser tooassist med loggning:</span><span class="sxs-lookup"><span data-stu-id="ec88d-249">Enterprise Library 6 (EntLib60) offers .NET managed classes tooassist with logging:</span></span>

* [<span data-ttu-id="ec88d-250">5 - som enkelt som som omfattas av av en logg: med hello loggning programmet Block</span><span class="sxs-lookup"><span data-stu-id="ec88d-250">5 - As Easy As Falling Off a Log: Using hello Logging Application Block</span></span>](http://msdn.microsoft.com/library/dn440731.aspx)

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a><span data-ttu-id="ec88d-251">Diagnostik: Granska systemloggar för fel</span><span class="sxs-lookup"><span data-stu-id="ec88d-251">Diagnostics: Examine system logs for errors</span></span>
<span data-ttu-id="ec88d-252">Här följer några Transact-SQL SELECT-satser som frågeloggar av fel och annan information.</span><span class="sxs-lookup"><span data-stu-id="ec88d-252">Here are some Transact-SQL SELECT statements that query logs of error and other information.</span></span>

| <span data-ttu-id="ec88d-253">Frågan för logg</span><span class="sxs-lookup"><span data-stu-id="ec88d-253">Query of log</span></span> | <span data-ttu-id="ec88d-254">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ec88d-254">Description</span></span> |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |<span data-ttu-id="ec88d-255">Hej [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) vyn innehåller information om hur enskilda händelser, inklusive några som kan orsaka tillfälliga fel eller anslutningsfel.</span><span class="sxs-lookup"><span data-stu-id="ec88d-255">hello [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) view offers information about individual events, including some that can cause transient errors or connectivity failures.</span></span><br/><br/><span data-ttu-id="ec88d-256">Helst korrelera hello **starttid** eller **end_time** värden med information om när dina klientprogram stötte på problem.</span><span class="sxs-lookup"><span data-stu-id="ec88d-256">Ideally you can correlate hello **start_time** or **end_time** values with information about when your client program experienced problems.</span></span><br/><br/><span data-ttu-id="ec88d-257">**Tips:** måste du ansluta toohello **master** databasen toorun detta.</span><span class="sxs-lookup"><span data-stu-id="ec88d-257">**TIP:** You must connect toohello **master** database toorun this.</span></span> |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |<span data-ttu-id="ec88d-258">Hej [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) visa erbjuder aggregerade antal händelsetyper för ytterligare diagnostik.</span><span class="sxs-lookup"><span data-stu-id="ec88d-258">hello [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) view offers aggregated counts of event types, for additional diagnostics.</span></span><br/><br/><span data-ttu-id="ec88d-259">**Tips:** måste du ansluta toohello **master** databasen toorun detta.</span><span class="sxs-lookup"><span data-stu-id="ec88d-259">**TIP:** You must connect toohello **master** database toorun this.</span></span> |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-hello-sql-database-log"></a><span data-ttu-id="ec88d-260">Diagnostik: Sök efter problem händelser i loggen för hello SQL-databas</span><span class="sxs-lookup"><span data-stu-id="ec88d-260">Diagnostics: Search for problem events in hello SQL Database log</span></span>
<span data-ttu-id="ec88d-261">Du kan söka efter poster om problemet händelser i loggen för hello i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ec88d-261">You can search for entries about problem events in hello log of Azure SQL Database.</span></span> <span data-ttu-id="ec88d-262">Försök följande Transact-SQL SELECT-uttryck i hello hello **master** databasen:</span><span class="sxs-lookup"><span data-stu-id="ec88d-262">Try hello following Transact-SQL SELECT statement in hello **master** database:</span></span>

```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a><span data-ttu-id="ec88d-263">Några returnerade rader från sys.fn_xe_telemetry_blob_target_read_file</span><span class="sxs-lookup"><span data-stu-id="ec88d-263">A few returned rows from sys.fn_xe_telemetry_blob_target_read_file</span></span>
<span data-ttu-id="ec88d-264">Nästa är en returnerad rad kan se ut.</span><span class="sxs-lookup"><span data-stu-id="ec88d-264">Next is what a returned row might look like.</span></span> <span data-ttu-id="ec88d-265">hello null-värden som visas är ofta inte null i andra rader.</span><span class="sxs-lookup"><span data-stu-id="ec88d-265">hello null values shown are often not null in other rows.</span></span>

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a><span data-ttu-id="ec88d-266">Enterprise Library 6</span><span class="sxs-lookup"><span data-stu-id="ec88d-266">Enterprise Library 6</span></span>
<span data-ttu-id="ec88d-267">Enterprise-biblioteket 6 (EntLib60) är ett ramverk för .NET-klasser som hjälper dig att implementera robust klienter för molntjänster, varav en är hello Azure SQL Database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ec88d-267">Enterprise Library 6 (EntLib60) is a framework of .NET classes that helps you implement robust clients of cloud services, one of which is hello Azure SQL Database service.</span></span> <span data-ttu-id="ec88d-268">Du kan hitta avsnitt dedikerade tooeach område där EntLib60 kan hjälpa genom att besöka första:</span><span class="sxs-lookup"><span data-stu-id="ec88d-268">You can locate topics dedicated tooeach area in which EntLib60 can assist by first visiting:</span></span>

* [<span data-ttu-id="ec88d-269">Enterprise Library 6 – April 2013</span><span class="sxs-lookup"><span data-stu-id="ec88d-269">Enterprise Library 6 - April 2013</span></span>](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)

<span data-ttu-id="ec88d-270">Logik för att hantera tillfälliga fel är en plats där EntLib60 kan hjälpa.</span><span class="sxs-lookup"><span data-stu-id="ec88d-270">Retry logic for handling transient errors is one area in which EntLib60 can assist:</span></span>

* [<span data-ttu-id="ec88d-271">4 - perseverance, hemligheten för alla framgångar: med hello tillfälliga fel hanterar programmet Block</span><span class="sxs-lookup"><span data-stu-id="ec88d-271">4 - Perseverance, Secret of All Triumphs: Using hello Transient Fault Handling Application Block</span></span>](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)

> [!NOTE]
> <span data-ttu-id="ec88d-272">hello källkoden för EntLib60 är tillgängligt för allmänheten [hämta](http://go.microsoft.com/fwlink/p/?LinkID=290898).</span><span class="sxs-lookup"><span data-stu-id="ec88d-272">hello source code for EntLib60 is available for public [download](http://go.microsoft.com/fwlink/p/?LinkID=290898).</span></span> <span data-ttu-id="ec88d-273">Microsoft har inga planer toomake ytterligare uppdateringar och underhåll uppdaterar tooEntLib.</span><span class="sxs-lookup"><span data-stu-id="ec88d-273">Microsoft has no plans toomake further feature updates or maintenance updates tooEntLib.</span></span>
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a><span data-ttu-id="ec88d-274">EntLib60 klasser för tillfälliga fel och försök igen</span><span class="sxs-lookup"><span data-stu-id="ec88d-274">EntLib60 classes for transient errors and retry</span></span>
<span data-ttu-id="ec88d-275">hello följande EntLib60 klasser är mycket användbart för logik.</span><span class="sxs-lookup"><span data-stu-id="ec88d-275">hello following EntLib60 classes are particularly useful for retry logic.</span></span> <span data-ttu-id="ec88d-276">Alla dessa, eller ytterligare under hello namnområde **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:</span><span class="sxs-lookup"><span data-stu-id="ec88d-276">All these  are in, or are further under, hello namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:</span></span>

<span data-ttu-id="ec88d-277">*I hello namnområdet **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*</span><span class="sxs-lookup"><span data-stu-id="ec88d-277">*In hello namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*</span></span>

* <span data-ttu-id="ec88d-278">**RetryPolicy** klass</span><span class="sxs-lookup"><span data-stu-id="ec88d-278">**RetryPolicy** class</span></span>
  
  * <span data-ttu-id="ec88d-279">**ExecuteAction** metod</span><span class="sxs-lookup"><span data-stu-id="ec88d-279">**ExecuteAction** method</span></span>
* <span data-ttu-id="ec88d-280">**ExponentialBackoff** klass</span><span class="sxs-lookup"><span data-stu-id="ec88d-280">**ExponentialBackoff** class</span></span>
* <span data-ttu-id="ec88d-281">**SqlDatabaseTransientErrorDetectionStrategy** klass</span><span class="sxs-lookup"><span data-stu-id="ec88d-281">**SqlDatabaseTransientErrorDetectionStrategy** class</span></span>
* <span data-ttu-id="ec88d-282">**ReliableSqlConnection** klass</span><span class="sxs-lookup"><span data-stu-id="ec88d-282">**ReliableSqlConnection** class</span></span>
  
  * <span data-ttu-id="ec88d-283">**ExecuteCommand** metod</span><span class="sxs-lookup"><span data-stu-id="ec88d-283">**ExecuteCommand** method</span></span>

<span data-ttu-id="ec88d-284">I hello namnområdet **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:</span><span class="sxs-lookup"><span data-stu-id="ec88d-284">In hello namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:</span></span>

* <span data-ttu-id="ec88d-285">**AlwaysTransientErrorDetectionStrategy** klass</span><span class="sxs-lookup"><span data-stu-id="ec88d-285">**AlwaysTransientErrorDetectionStrategy** class</span></span>
* <span data-ttu-id="ec88d-286">**NeverTransientErrorDetectionStrategy** klass</span><span class="sxs-lookup"><span data-stu-id="ec88d-286">**NeverTransientErrorDetectionStrategy** class</span></span>

<span data-ttu-id="ec88d-287">Här är länkar tooinformation om EntLib60:</span><span class="sxs-lookup"><span data-stu-id="ec88d-287">Here are links tooinformation about EntLib60:</span></span>

* <span data-ttu-id="ec88d-288">Ledigt [Book hämta: Developer's Guide tooMicrosoft Enterprise Library 2 Edition](http://www.microsoft.com/download/details.aspx?id=41145)</span><span class="sxs-lookup"><span data-stu-id="ec88d-288">Free [Book Download: Developer's Guide tooMicrosoft Enterprise Library, 2nd Edition](http://www.microsoft.com/download/details.aspx?id=41145)</span></span>
* <span data-ttu-id="ec88d-289">Bästa praxis: [försök allmänna riktlinjer](../best-practices-retry-general.md) har en utmärkt detaljerad beskrivning av logik.</span><span class="sxs-lookup"><span data-stu-id="ec88d-289">Best practices: [Retry general guidance](../best-practices-retry-general.md) has an excellent in-depth discussion of retry logic.</span></span>
* <span data-ttu-id="ec88d-290">Hämta NuGet [Enterprise Library - hantering av tillfälliga fel program block 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)</span><span class="sxs-lookup"><span data-stu-id="ec88d-290">NuGet download of [Enterprise Library - Transient Fault Handling application block 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)</span></span>

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-hello-logging-block"></a><span data-ttu-id="ec88d-291">EntLib60: hello loggning block</span><span class="sxs-lookup"><span data-stu-id="ec88d-291">EntLib60: hello logging block</span></span>
* <span data-ttu-id="ec88d-292">hello loggning block är en mycket flexibelt och konfigurerbara lösning som hjälper dig att:</span><span class="sxs-lookup"><span data-stu-id="ec88d-292">hello Logging block is a highly flexible and configurable solution that allows you to:</span></span>
  
  * <span data-ttu-id="ec88d-293">Skapa och lagra meddelanden i en mängd olika platser.</span><span class="sxs-lookup"><span data-stu-id="ec88d-293">Create and store log messages in a wide variety of locations.</span></span>
  * <span data-ttu-id="ec88d-294">Kategorisera och filtrera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ec88d-294">Categorize and filter messages.</span></span>
  * <span data-ttu-id="ec88d-295">Samla in relevant information som är användbart för felsökning och spårning och för granskning och allmänna loggning krav.</span><span class="sxs-lookup"><span data-stu-id="ec88d-295">Collect contextual information that is useful for debugging and tracing, as well as for auditing and general logging requirements.</span></span>
* <span data-ttu-id="ec88d-296">hello loggning block avlägsnar hello loggning funktioner från hello loggmålet så att hello programkoden är konsekvent, oavsett hello plats och typ för hello mål loggning store.</span><span class="sxs-lookup"><span data-stu-id="ec88d-296">hello Logging block abstracts hello logging functionality from hello log destination so that hello application code is consistent, irrespective of hello location and type of hello target logging store.</span></span>

<span data-ttu-id="ec88d-297">Mer information finns: [5 – som enkelt som som omfattas av av en logg: med hello loggning programmet Block](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="ec88d-297">For details see: [5 - As Easy As Falling Off a Log: Using hello Logging Application Block](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)</span></span>

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a><span data-ttu-id="ec88d-298">Källkoden för EntLib60 IsTransient metod</span><span class="sxs-lookup"><span data-stu-id="ec88d-298">EntLib60 IsTransient method source code</span></span>
<span data-ttu-id="ec88d-299">Nästa från hello **SqlDatabaseTransientErrorDetectionStrategy** klassen är hello C#-källkod för hello **IsTransient** metod.</span><span class="sxs-lookup"><span data-stu-id="ec88d-299">Next, from hello **SqlDatabaseTransientErrorDetectionStrategy** class, is hello C# source code for hello **IsTransient** method.</span></span> <span data-ttu-id="ec88d-300">hello källkoden klargör vilka fel ansågs toobe tillfälligt och worthy på försök igen, från och med April 2013.</span><span class="sxs-lookup"><span data-stu-id="ec88d-300">hello source code clarifies which errors were considered toobe transient and worthy of retry, as of April 2013.</span></span>

<span data-ttu-id="ec88d-301">Ett stort antal **//comment** rader har tagits bort från den här kopian tooemphasize läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="ec88d-301">Numerous **//comment** lines have been removed from this copy tooemphasize readability.</span></span>

```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in hello exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // hello service is currently busy. Retry hello request after 10 seconds.
            // Code: (reason code toobe decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode hello reason code from hello error message to
            // determine hello grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach hello decoded values as additional attributes to
            // hello original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // hello instance of SQL Server you attempted tooconnect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a><span data-ttu-id="ec88d-302">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ec88d-302">Next steps</span></span>
* <span data-ttu-id="ec88d-303">Felsöka andra vanliga problem med Azure SQL Database anslutningen finns [felsöka anslutningen utfärdar tooAzure SQL-databas](sql-database-troubleshoot-common-connection-issues.md).</span><span class="sxs-lookup"><span data-stu-id="ec88d-303">For troubleshooting other common Azure SQL Database connection issues, visit [Troubleshoot connection issues tooAzure SQL Database](sql-database-troubleshoot-common-connection-issues.md).</span></span>
* [<span data-ttu-id="ec88d-304">SQL Server anslutningspoolen (ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="ec88d-304">SQL Server Connection Pooling (ADO.NET)</span></span>](http://msdn.microsoft.com/library/8xx3tyca.aspx)
* [<span data-ttu-id="ec88d-305">*Du försöker* är ett Apache 2.0 licensierad allmänna du försöker biblioteket som skrivits i **Python**, toosimplify hello uppgiften att lägga till försök beteende toojust om något.</span><span class="sxs-lookup"><span data-stu-id="ec88d-305">*Retrying* is an Apache 2.0 licensed general-purpose retrying library, written in **Python**, toosimplify hello task of adding retry behavior toojust about anything.</span></span>](https://pypi.python.org/pypi/retrying)

