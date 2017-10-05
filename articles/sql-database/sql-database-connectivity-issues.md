---
title: "Åtgärda ett fel vid anslutning till SQL, ett tillfälligt fel | Microsoft Docs"
description: "Lär dig hur du felsöker, diagnostisera och förhindra att ett fel vid anslutning till SQL eller ett tillfälligt fel i Azure SQL Database. "
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
ms.openlocfilehash: ae081fc0432e36bf9f4d4f06f289386ddce37990
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a><span data-ttu-id="a0597-104">Felsöka, diagnostisera och förhindra SQL-anslutningsfel och tillfälliga fel för SQL Database</span><span class="sxs-lookup"><span data-stu-id="a0597-104">Troubleshoot, diagnose, and prevent SQL connection errors and transient errors for SQL Database</span></span>
<span data-ttu-id="a0597-105">Den här artikeln beskriver hur du förhindra, Felsök, diagnostisera och minska anslutningsfel och tillfälliga fel som ditt klientprogram påträffar när det interagerar med Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a0597-105">This article describes how to prevent, troubleshoot, diagnose, and mitigate connection errors and transient errors that your client application encounters when it interacts with Azure SQL Database.</span></span> <span data-ttu-id="a0597-106">Lär dig hur du konfigurerar logik för omprövning, anslutningssträngen och justera andra anslutningsinställningar för.</span><span class="sxs-lookup"><span data-stu-id="a0597-106">Learn how to configure retry logic, build the connection string, and adjust other connection settings.</span></span>

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a><span data-ttu-id="a0597-107">Tillfälliga fel tillfälligt</span><span class="sxs-lookup"><span data-stu-id="a0597-107">Transient errors (transient faults)</span></span>
<span data-ttu-id="a0597-108">Ett tillfälligt fel - också tillfälliga fel – har en underliggande orsaken som kommer snart att lösas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="a0597-108">A transient error - also, transient fault - has an underlying cause that will soon resolve itself.</span></span> <span data-ttu-id="a0597-109">En tillfällig tillfälliga fel beror på när systemets Azure snabbt hamnar maskinvaruresurser bättre belastningsutjämning olika arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="a0597-109">An occasional cause of transient errors is when the Azure system quickly shifts hardware resources to better load-balance various workloads.</span></span> <span data-ttu-id="a0597-110">De flesta av dessa händelser för omkonfiguration Slutför ofta i mindre än 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="a0597-110">Most of these reconfiguration events often complete in less than 60 seconds.</span></span> <span data-ttu-id="a0597-111">Du kan ha problem med nätverksanslutningen till Azure SQL Database under det angivna tidsintervallet omkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="a0597-111">During this reconfiguration time span, you may have connectivity issues to Azure SQL Database.</span></span> <span data-ttu-id="a0597-112">Program som ansluter till Azure SQL Database ska byggas räknar dessa tillfälliga fel, hantera dem genom att implementera logik i koden i stället för att visa dem till användare som programfel.</span><span class="sxs-lookup"><span data-stu-id="a0597-112">Applications connecting to Azure SQL Database should be built to expect these transient errors, handle them by implementing retry logic in their code instead of surfacing them to users as application errors.</span></span>

<span data-ttu-id="a0597-113">Om klientprogrammet använder ADO.NET, programmet meddelade om tillfälligt fel av throw av en **SqlException**.</span><span class="sxs-lookup"><span data-stu-id="a0597-113">If your client program is using ADO.NET, your program is told about the transient error by the throw of an **SqlException**.</span></span> <span data-ttu-id="a0597-114">Den **nummer** egenskapen kan jämföras med listan över tillfälliga fel längst upp i avsnittet: [SQL-felkoder för SQL Database-klientprogram](sql-database-develop-error-messages.md).</span><span class="sxs-lookup"><span data-stu-id="a0597-114">The **Number** property can be compared against the list of transient errors near the top of the topic: [SQL error codes for SQL Database client applications](sql-database-develop-error-messages.md).</span></span>

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a><span data-ttu-id="a0597-115">Anslutningen jämfört med kommandot</span><span class="sxs-lookup"><span data-stu-id="a0597-115">Connection versus command</span></span>
<span data-ttu-id="a0597-116">Du kommer försök SQL-anslutning eller upprätta igen, beroende på följande:</span><span class="sxs-lookup"><span data-stu-id="a0597-116">You'll retry the SQL connection or establish it again, depending on the following:</span></span>

* <span data-ttu-id="a0597-117">**Ett tillfälligt fel inträffar under ett anslutningsförsök**: anslutningen ska göras efter att förskjuta för några sekunder.</span><span class="sxs-lookup"><span data-stu-id="a0597-117">**A transient error occurs during a connection try**: The connection should be retried after delaying for several seconds.</span></span>
* <span data-ttu-id="a0597-118">**Ett tillfälligt fel inträffar under en SQL-kommandot query**: kommandot bör inte göras omedelbart.</span><span class="sxs-lookup"><span data-stu-id="a0597-118">**A transient error occurs during a SQL query command**: The command should not be immediately retried.</span></span> <span data-ttu-id="a0597-119">I stället efter en fördröjning i anslutningen bör nyligen upprättas.</span><span class="sxs-lookup"><span data-stu-id="a0597-119">Instead, after a delay, the connection should be freshly established.</span></span> <span data-ttu-id="a0597-120">Sedan kan kommandot göras.</span><span class="sxs-lookup"><span data-stu-id="a0597-120">Then the command can be retried.</span></span>

<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a><span data-ttu-id="a0597-121">Logik för omprövning av tillfälliga fel</span><span class="sxs-lookup"><span data-stu-id="a0597-121">Retry logic for transient errors</span></span>
<span data-ttu-id="a0597-122">Klientprogram som kan uppstår ett tillfälligt fel är mer robusta när de innehåller logik för omprövning.</span><span class="sxs-lookup"><span data-stu-id="a0597-122">Client programs that occasionally encounter a transient error are more robust when they contain retry logic.</span></span>

<span data-ttu-id="a0597-123">När ditt program kommunicerar med Azure SQL Database via en 3 part mellanprogram, fråga med leverantören om mellanprogram innehåller logik för omprövning av tillfälliga fel.</span><span class="sxs-lookup"><span data-stu-id="a0597-123">When your program communicates with Azure SQL Database through a 3rd party middleware, inquire with the vendor whether the middleware contains retry logic for transient errors.</span></span>

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a><span data-ttu-id="a0597-124">Principer för nya försök</span><span class="sxs-lookup"><span data-stu-id="a0597-124">Principles for retry</span></span>
* <span data-ttu-id="a0597-125">Ett försök att öppna en anslutning ska göras om felet är tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="a0597-125">An attempt to open a connection should be retried if the error is transient.</span></span>
* <span data-ttu-id="a0597-126">En SQL SELECT-instruktion som misslyckas med ett tillfälligt fel bör inte göras direkt.</span><span class="sxs-lookup"><span data-stu-id="a0597-126">A SQL SELECT statement that fails with a transient error should not be retried directly.</span></span>
  
  * <span data-ttu-id="a0597-127">I stället skapa en ny anslutning och försök sedan Välj.</span><span class="sxs-lookup"><span data-stu-id="a0597-127">Instead, establish a fresh connection, and then retry the SELECT.</span></span>
* <span data-ttu-id="a0597-128">När en UPDATE SQL-instruktion misslyckas med ett tillfälligt fel, ska en ny anslutning upprättas innan UPPDATERINGEN igen.</span><span class="sxs-lookup"><span data-stu-id="a0597-128">When a SQL UPDATE statement fails with a transient error, a fresh connection should be established before the UPDATE is retried.</span></span>
  
  * <span data-ttu-id="a0597-129">Logik för omprövning måste se till att hela Databastransaktionen slutförts, eller som hela transaktionen återställs.</span><span class="sxs-lookup"><span data-stu-id="a0597-129">The retry logic must ensure that either the entire database transaction completed, or that the entire transaction is rolled back.</span></span>

#### <a name="other-considerations-for-retry"></a><span data-ttu-id="a0597-130">Andra överväganden för nya försök</span><span class="sxs-lookup"><span data-stu-id="a0597-130">Other considerations for retry</span></span>
* <span data-ttu-id="a0597-131">En kommandofil som startas automatiskt efter arbetstid och som kommer att slutföras innan morgon har råd att mycket tålamod med lång tidsintervall mellan dess nya försök.</span><span class="sxs-lookup"><span data-stu-id="a0597-131">A batch program that is automatically started after work hours, and which will complete before morning, can afford to very patient with long time intervals between its retry attempts.</span></span>
* <span data-ttu-id="a0597-132">Ett gränssnitt användarprogram bör ta hänsyn till mänsklig tendens att avstå från efter för lång väntan.</span><span class="sxs-lookup"><span data-stu-id="a0597-132">A user interface program should account for the human tendency to give up after too long a wait.</span></span>
  
  * <span data-ttu-id="a0597-133">Lösningen måste dock inte att försöka några sekunders eftersom principen kan översvämma system med begäranden.</span><span class="sxs-lookup"><span data-stu-id="a0597-133">However, the solution must not be to retry every few seconds, because that policy can flood the system with requests.</span></span>

#### <a name="interval-increase-between-retries"></a><span data-ttu-id="a0597-134">Öka intervall mellan försök</span><span class="sxs-lookup"><span data-stu-id="a0597-134">Interval increase between retries</span></span>
<span data-ttu-id="a0597-135">Vi rekommenderar att du fördröjning för 5 sekunder innan din första försök.</span><span class="sxs-lookup"><span data-stu-id="a0597-135">We recommend that you delay for 5 seconds before your first retry.</span></span> <span data-ttu-id="a0597-136">Försöker igen efter en kortare än 5 sekunder risker överbelasta Molntjänsten fördröjning.</span><span class="sxs-lookup"><span data-stu-id="a0597-136">Retrying after a delay shorter than 5 seconds risks overwhelming the cloud service.</span></span> <span data-ttu-id="a0597-137">För varje efterföljande försök fördröjningen ska växa exponentiellt, upp till högst 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="a0597-137">For each subsequent retry the delay should grow exponentially, up to a maximum of 60 seconds.</span></span>

<span data-ttu-id="a0597-138">En beskrivning av den *blockerande period* för klienter som använder ADO.NET finns i [SQL Server anslutning poolning (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).</span><span class="sxs-lookup"><span data-stu-id="a0597-138">A discussion of the *blocking period* for clients that use ADO.NET is available in [SQL Server Connection Pooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).</span></span>

<span data-ttu-id="a0597-139">Du kanske också vill ange ett maximalt antal försök innan programmet avslutar själva.</span><span class="sxs-lookup"><span data-stu-id="a0597-139">You might also want to set a maximum number of retries before the program self-terminates.</span></span>

#### <a name="code-samples-with-retry-logic"></a><span data-ttu-id="a0597-140">Kodexempel med logik</span><span class="sxs-lookup"><span data-stu-id="a0597-140">Code samples with retry logic</span></span>
<span data-ttu-id="a0597-141">Kodexempel med logik för omprövning, på en mängd olika programmeringsspråk, finns på:</span><span class="sxs-lookup"><span data-stu-id="a0597-141">Code samples with retry logic, in a variety of programming languages, are available at:</span></span>

* [<span data-ttu-id="a0597-142">Anslutningsbibliotek för SQL Database och SQL Server</span><span class="sxs-lookup"><span data-stu-id="a0597-142">Connection libraries for SQL Database and SQL Server</span></span>](sql-database-libraries.md)

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a><span data-ttu-id="a0597-143">Testa din logik</span><span class="sxs-lookup"><span data-stu-id="a0597-143">Test your retry logic</span></span>
<span data-ttu-id="a0597-144">Om du vill testa logik för omprövning, måste du simulera eller orsaka fel än kan korrigeras medan programmet körs.</span><span class="sxs-lookup"><span data-stu-id="a0597-144">To test your retry logic, you must simulate or cause an error than can be corrected while your program is still running.</span></span>

##### <a name="test-by-disconnecting-from-the-network"></a><span data-ttu-id="a0597-145">Testa genom frånkoppling från nätverket</span><span class="sxs-lookup"><span data-stu-id="a0597-145">Test by disconnecting from the network</span></span>
<span data-ttu-id="a0597-146">Ett sätt som du kan testa din logik är att koppla från din klientdator från nätverket medan programmet körs.</span><span class="sxs-lookup"><span data-stu-id="a0597-146">One way you can test your retry logic is to disconnect your client computer from the network while the program is running.</span></span> <span data-ttu-id="a0597-147">Felet är:</span><span class="sxs-lookup"><span data-stu-id="a0597-147">The error will be:</span></span>

* <span data-ttu-id="a0597-148">**SqlException.Number** = 11001</span><span class="sxs-lookup"><span data-stu-id="a0597-148">**SqlException.Number** = 11001</span></span>
* <span data-ttu-id="a0597-149">Meddelandet: ”värden är inte känd”</span><span class="sxs-lookup"><span data-stu-id="a0597-149">Message: "No such host is known"</span></span>

<span data-ttu-id="a0597-150">Som en del av det första nytt försöket, programmet korrigera felstavningen och försök sedan ansluta.</span><span class="sxs-lookup"><span data-stu-id="a0597-150">As part of the first retry attempt, your program can correct the misspelling, and then attempt to connect.</span></span>

<span data-ttu-id="a0597-151">För att göra detta praktiska kan du koppla från datorn från nätverket innan du startar programmet.</span><span class="sxs-lookup"><span data-stu-id="a0597-151">To make this practical, you unplug your computer from the network before you start your program.</span></span> <span data-ttu-id="a0597-152">Sedan identifierar programmet en körning-parameter som innebär att:</span><span class="sxs-lookup"><span data-stu-id="a0597-152">Then your program recognizes a run time parameter that causes the program to:</span></span>

1. <span data-ttu-id="a0597-153">Lägg till 11001 tillfälligt till i listan med fel ska vara tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="a0597-153">Temporarily add 11001 to its list of errors to consider as transient.</span></span>
2. <span data-ttu-id="a0597-154">Försök den första anslutningen som vanligt.</span><span class="sxs-lookup"><span data-stu-id="a0597-154">Attempt its first connection as usual.</span></span>
3. <span data-ttu-id="a0597-155">Ta bort 11001 i listan efter fel har inträffat.</span><span class="sxs-lookup"><span data-stu-id="a0597-155">After the error is caught, remove 11001 from the list.</span></span>
4. <span data-ttu-id="a0597-156">Visa ett meddelande som uppmanar användaren att ansluta datorn till nätverket.</span><span class="sxs-lookup"><span data-stu-id="a0597-156">Display a message telling the user to plug the computer into the network.</span></span>
   * <span data-ttu-id="a0597-157">Pausa körning med hjälp av antingen den **Console.ReadLine** metod eller en dialogruta med en OK-knapp.</span><span class="sxs-lookup"><span data-stu-id="a0597-157">Pause further execution by using either the **Console.ReadLine** method or a dialog with an OK button.</span></span> <span data-ttu-id="a0597-158">Användaren trycker på RETUR när datorn är ansluten till nätverket.</span><span class="sxs-lookup"><span data-stu-id="a0597-158">The user presses the Enter key after the computer plugged into the network.</span></span>
5. <span data-ttu-id="a0597-159">Försök igen att ansluta, förväntas lyckades.</span><span class="sxs-lookup"><span data-stu-id="a0597-159">Attempt again to connect, expecting success.</span></span>

##### <a name="test-by-misspelling-the-database-name-when-connecting"></a><span data-ttu-id="a0597-160">Testa genom skriver namnet på databasen vid anslutning</span><span class="sxs-lookup"><span data-stu-id="a0597-160">Test by misspelling the database name when connecting</span></span>
<span data-ttu-id="a0597-161">Programmet kan ändamålsenligt fel användarnamn innan det första anslutningsförsöket.</span><span class="sxs-lookup"><span data-stu-id="a0597-161">Your program can purposely misspell the user name before the first connection attempt.</span></span> <span data-ttu-id="a0597-162">Felet är:</span><span class="sxs-lookup"><span data-stu-id="a0597-162">The error will be:</span></span>

* <span data-ttu-id="a0597-163">**SqlException.Number** = 18456</span><span class="sxs-lookup"><span data-stu-id="a0597-163">**SqlException.Number** = 18456</span></span>
* <span data-ttu-id="a0597-164">Meddelandet: ”inloggning misslyckades för användaren 'WRONG_MyUserName'”.</span><span class="sxs-lookup"><span data-stu-id="a0597-164">Message: "Login failed for user 'WRONG_MyUserName'."</span></span>

<span data-ttu-id="a0597-165">Som en del av det första nytt försöket, programmet korrigera felstavningen och försök sedan ansluta.</span><span class="sxs-lookup"><span data-stu-id="a0597-165">As part of the first retry attempt, your program can correct the misspelling, and then attempt to connect.</span></span>

<span data-ttu-id="a0597-166">För att göra det praktiska kände programmet igen en körning-parameter som innebär att:</span><span class="sxs-lookup"><span data-stu-id="a0597-166">To make this practical, your program could recognize a run time parameter that causes the program to:</span></span>

1. <span data-ttu-id="a0597-167">Lägg till 18456 tillfälligt till i listan med fel ska vara tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="a0597-167">Temporarily add 18456 to its list of errors to consider as transient.</span></span>
2. <span data-ttu-id="a0597-168">Lägg till 'WRONG_' ändamålsenligt i användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="a0597-168">Purposely add 'WRONG_' to the user name.</span></span>
3. <span data-ttu-id="a0597-169">Ta bort 18456 i listan efter fel har inträffat.</span><span class="sxs-lookup"><span data-stu-id="a0597-169">After the error is caught, remove 18456 from the list.</span></span>
4. <span data-ttu-id="a0597-170">Ta bort 'WRONG_' från användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="a0597-170">Remove 'WRONG_' from the user name.</span></span>
5. <span data-ttu-id="a0597-171">Försök igen att ansluta, förväntas lyckades.</span><span class="sxs-lookup"><span data-stu-id="a0597-171">Attempt again to connect, expecting success.</span></span>

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a><span data-ttu-id="a0597-172">.NET SqlConnection parametrar för anslutningen försök igen</span><span class="sxs-lookup"><span data-stu-id="a0597-172">.NET SqlConnection parameters for connection retry</span></span>
<span data-ttu-id="a0597-173">Om klientprogrammet ansluter till Azure SQL Database med hjälp av .NET Framework-klassen **System.Data.SqlClient.SqlConnection**, bör du använda .NET 4.6.1 eller senare (eller .NET Core) så att du kan utnyttja funktionen anslutning försök igen.</span><span class="sxs-lookup"><span data-stu-id="a0597-173">If your client program connects to to Azure SQL Database by using the .NET Framework class **System.Data.SqlClient.SqlConnection**, you should use .NET 4.6.1 or later (or .NET Core) so you can leverage its connection retry feature.</span></span> <span data-ttu-id="a0597-174">Information om funktionen [här](http://go.microsoft.com/fwlink/?linkid=393996).</span><span class="sxs-lookup"><span data-stu-id="a0597-174">Details of the feature are [here](http://go.microsoft.com/fwlink/?linkid=393996).</span></span>

<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


<span data-ttu-id="a0597-175">När du skapar den [anslutningssträngen](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) för din **SqlConnection** objekt, bör du samordna värdena bland följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="a0597-175">When you build the [connection string](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) for your **SqlConnection** object, you should coordinate the values among the following parameters:</span></span>

* <span data-ttu-id="a0597-176">ConnectRetryCount &nbsp; &nbsp; *(standardvärdet är 1. Intervallet är 0 och 255.)*</span><span class="sxs-lookup"><span data-stu-id="a0597-176">ConnectRetryCount &nbsp;&nbsp;*(Default is 1. Range is 0 through 255.)*</span></span>
* <span data-ttu-id="a0597-177">ConnectRetryInterval &nbsp; &nbsp; *(standard är 1 sekund. Intervall är 1 till 60).*</span><span class="sxs-lookup"><span data-stu-id="a0597-177">ConnectRetryInterval &nbsp;&nbsp;*(Default is 1 second. Range is 1 through 60.)*</span></span>
* <span data-ttu-id="a0597-178">Timeout för anslutningen &nbsp; &nbsp; *(standardvärdet är 15 sekunder. Intervallet är 0 och 2147483647)*</span><span class="sxs-lookup"><span data-stu-id="a0597-178">Connection Timeout &nbsp;&nbsp;*(Default is 15 seconds. Range is 0 through 2147483647)*</span></span>

<span data-ttu-id="a0597-179">Mer specifikt bör valda värdena Se följande likheten SANT:</span><span class="sxs-lookup"><span data-stu-id="a0597-179">Specifically, your chosen values should make the following equality true:</span></span>

* <span data-ttu-id="a0597-180">Timeout för anslutningen = ConnectRetryCount * ConnectionRetryInterval</span><span class="sxs-lookup"><span data-stu-id="a0597-180">Connection Timeout = ConnectRetryCount * ConnectionRetryInterval</span></span>

<span data-ttu-id="a0597-181">Till exempel om antalet = 3 och intervall = 10 sekunder timeout för bara 29 sekunder inte skulle helt ge systemet tillräckligt med tid för dess 3 och slutgiltig försök vid anslutning: 29 < 3 * 10.</span><span class="sxs-lookup"><span data-stu-id="a0597-181">For example, if the count = 3, and interval = 10 seconds, a timeout of only 29 seconds would not quite give the system enough time for its 3rd and final retry at connecting: 29 < 3 * 10.</span></span>

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a><span data-ttu-id="a0597-182">Anslutningen jämfört med kommandot</span><span class="sxs-lookup"><span data-stu-id="a0597-182">Connection versus command</span></span>
<span data-ttu-id="a0597-183">Den **ConnectRetryCount** och **ConnectRetryInterval** -parametrar kan din **SqlConnection** objekt försök ansluta utan uppmanar eller stör din program, till exempel kontrollen återgår till programmet.</span><span class="sxs-lookup"><span data-stu-id="a0597-183">The **ConnectRetryCount** and **ConnectRetryInterval** parameters let your **SqlConnection** object retry the connect operation without telling or bothering your program, such as returning control to your program.</span></span> <span data-ttu-id="a0597-184">Återförsöken kan inträffa i följande situationer:</span><span class="sxs-lookup"><span data-stu-id="a0597-184">The retries can occur in the following situations:</span></span>

* <span data-ttu-id="a0597-185">mySqlConnection.Open metodanrop</span><span class="sxs-lookup"><span data-stu-id="a0597-185">mySqlConnection.Open method call</span></span>
* <span data-ttu-id="a0597-186">mySqlConnection.Execute metodanrop</span><span class="sxs-lookup"><span data-stu-id="a0597-186">mySqlConnection.Execute method call</span></span>

<span data-ttu-id="a0597-187">Det finns en liten finess.</span><span class="sxs-lookup"><span data-stu-id="a0597-187">There is a subtlety.</span></span> <span data-ttu-id="a0597-188">Om ett tillfälligt fel uppstår när din *frågan* körs din **SqlConnection** objekt inte försök ansluta igen och den verkligen inte köra frågan igen.</span><span class="sxs-lookup"><span data-stu-id="a0597-188">If a transient error occurs while your *query* is being executed, your **SqlConnection** object does not retry the connect operation, and it certainly does not retry your query.</span></span> <span data-ttu-id="a0597-189">Dock **SqlConnection** snabbt kontrollerar anslutningen innan du skickar frågan för körning.</span><span class="sxs-lookup"><span data-stu-id="a0597-189">However, **SqlConnection** very quickly checks the connection before sending your query for execution.</span></span> <span data-ttu-id="a0597-190">Om snabb kontrollen identifierar ett anslutningsproblem **SqlConnection** försöker ansluta igen.</span><span class="sxs-lookup"><span data-stu-id="a0597-190">If the quick check detects a connection problem, **SqlConnection** retries the connect operation.</span></span> <span data-ttu-id="a0597-191">Om det nya försöket lyckas, skickas frågan för du för körning.</span><span class="sxs-lookup"><span data-stu-id="a0597-191">If the retry succeeds, you query is sent for execution.</span></span>

#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a><span data-ttu-id="a0597-192">ConnectRetryCount kombineras med logik för omprövning av programmet?</span><span class="sxs-lookup"><span data-stu-id="a0597-192">Should ConnectRetryCount be combined with application retry logic?</span></span>
<span data-ttu-id="a0597-193">Anta att programmet har robust anpassade logik.</span><span class="sxs-lookup"><span data-stu-id="a0597-193">Suppose your application has robust custom retry logic.</span></span> <span data-ttu-id="a0597-194">Det kan och försök ansluta igen 4 gånger.</span><span class="sxs-lookup"><span data-stu-id="a0597-194">It might retry the connect operation 4 times.</span></span> <span data-ttu-id="a0597-195">Om du lägger till **ConnectRetryInterval** och **ConnectRetryCount** = 3 till anslutningssträngen, ökar antalet försök att 4 * 3 = 12 återförsök.</span><span class="sxs-lookup"><span data-stu-id="a0597-195">If you add **ConnectRetryInterval** and **ConnectRetryCount** =3 to your connection string, you will increase the retry count to 4 * 3 = 12 retries.</span></span> <span data-ttu-id="a0597-196">Du kan inte avser sådana ett stort antal återförsök.</span><span class="sxs-lookup"><span data-stu-id="a0597-196">You might not intend such a high number of retries.</span></span>

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a><span data-ttu-id="a0597-197">Anslutningar till Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="a0597-197">Connections to Azure SQL Database</span></span>
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a><span data-ttu-id="a0597-198">Anslutningen: Anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="a0597-198">Connection: Connection string</span></span>
<span data-ttu-id="a0597-199">Anslutningssträngen som är nödvändiga för att ansluta till Azure SQL Database är skiljer sig från strängen för att ansluta till Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a0597-199">The connection string necessary for connecting to Azure SQL Database is slightly different from the string for connecting to Microsoft SQL Server.</span></span> <span data-ttu-id="a0597-200">Du kan kopiera anslutningssträngen för databasen från den [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a0597-200">You can copy the connection string for your database from the [Azure portal](https://portal.azure.com/).</span></span>

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a><span data-ttu-id="a0597-201">Anslutning: IP-adress för</span><span class="sxs-lookup"><span data-stu-id="a0597-201">Connection: IP address</span></span>
<span data-ttu-id="a0597-202">Du måste konfigurera SQL Database-server för att acceptera kommunikation från IP-adress på den dator som är värd för dina klientprogram.</span><span class="sxs-lookup"><span data-stu-id="a0597-202">You must configure the SQL Database server to accept communication from the IP address of the computer that hosts your client program.</span></span> <span data-ttu-id="a0597-203">Du kan göra detta genom att redigera brandväggsinställningar via den [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a0597-203">You do this by editing the firewall settings through the [Azure portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="a0597-204">Om du glömmer att konfigurera IP-adressen misslyckas programmet med en praktisk felmeddelande nödvändiga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="a0597-204">If you forget to configure the IP address, your program will fail with a handy error message that states the necessary IP address.</span></span>

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

<span data-ttu-id="a0597-205">Mer information finns: [så här: konfigurera brandväggsinställningar på SQL-databas](sql-database-configure-firewall-settings.md)</span><span class="sxs-lookup"><span data-stu-id="a0597-205">For more information, see: [How to: Configure firewall settings on SQL Database](sql-database-configure-firewall-settings.md)</span></span>

<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a><span data-ttu-id="a0597-206">Anslutning: portar</span><span class="sxs-lookup"><span data-stu-id="a0597-206">Connection: Ports</span></span>
<span data-ttu-id="a0597-207">Vanligtvis behöver du bara kontrollera att port 1433 är öppen för utgående kommunikation på datorn som är värd-klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a0597-207">Typically you only need to ensure that port 1433 is open for outbound communication, on the computer that hosts you client program.</span></span>

<span data-ttu-id="a0597-208">Till exempel när klientprogrammet finns på en Windows-dator, kan Windows-brandväggen på värden du öppna port 1433:</span><span class="sxs-lookup"><span data-stu-id="a0597-208">For example, when your client program is hosted on a Windows computer, the Windows Firewall on the host enables you to open port 1433:</span></span>

1. <span data-ttu-id="a0597-209">Öppna Kontrollpanelen</span><span class="sxs-lookup"><span data-stu-id="a0597-209">Open the Control Panel</span></span>
2. <span data-ttu-id="a0597-210">&gt;Alla objekt på Kontrollpanelen</span><span class="sxs-lookup"><span data-stu-id="a0597-210">&gt; All Control Panel Items</span></span>
3. <span data-ttu-id="a0597-211">&gt;Windows-brandväggen</span><span class="sxs-lookup"><span data-stu-id="a0597-211">&gt; Windows Firewall</span></span>
4. <span data-ttu-id="a0597-212">&gt;Avancerade inställningar</span><span class="sxs-lookup"><span data-stu-id="a0597-212">&gt; Advanced Settings</span></span>
5. <span data-ttu-id="a0597-213">&gt;Utgående regler</span><span class="sxs-lookup"><span data-stu-id="a0597-213">&gt; Outbound Rules</span></span>
6. <span data-ttu-id="a0597-214">&gt;Åtgärder</span><span class="sxs-lookup"><span data-stu-id="a0597-214">&gt; Actions</span></span>
7. <span data-ttu-id="a0597-215">&gt;Ny regel</span><span class="sxs-lookup"><span data-stu-id="a0597-215">&gt; New Rule</span></span>

<span data-ttu-id="a0597-216">Om klientprogrammet finns på en Azure-dator (VM), bör du läsa:</span><span class="sxs-lookup"><span data-stu-id="a0597-216">If your client program is hosted on an Azure virtual machine (VM), you should read:</span></span><br/><span data-ttu-id="a0597-217">[Portar utöver 1433 för ADO.NET 4.5 och SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span><span class="sxs-lookup"><span data-stu-id="a0597-217">[Ports beyond 1433 for ADO.NET 4.5 and SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span></span>

<span data-ttu-id="a0597-218">Mer bakgrundsinformation om cofiguration portar och IP-adress, se: [Azure SQL Database-brandvägg](sql-database-firewall-configure.md)</span><span class="sxs-lookup"><span data-stu-id="a0597-218">For background information about cofiguration of ports and IP address, see: [Azure SQL Database firewall](sql-database-firewall-configure.md)</span></span>

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a><span data-ttu-id="a0597-219">Anslutning: ADO.NET 4.6.1</span><span class="sxs-lookup"><span data-stu-id="a0597-219">Connection: ADO.NET 4.6.1</span></span>
<span data-ttu-id="a0597-220">Om programmet använder ADO.NET-klasser som **System.Data.SqlClient.SqlConnection** för att ansluta till Azure SQL Database, rekommenderar vi att du använder .NET Framework version 4.6.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a0597-220">If your program uses ADO.NET classes like **System.Data.SqlClient.SqlConnection** to connect to Azure SQL Database, we recommend that you use .NET Framework version 4.6.1 or higher.</span></span>

<span data-ttu-id="a0597-221">ADO.NET 4.6.1:</span><span class="sxs-lookup"><span data-stu-id="a0597-221">ADO.NET 4.6.1:</span></span>

* <span data-ttu-id="a0597-222">För Azure SQL-databas finns förbättrad tillförlitlighet när du öppnar en anslutning med hjälp av den **SqlConnection.Open** metod.</span><span class="sxs-lookup"><span data-stu-id="a0597-222">For Azure SQL Database, there is improved reliability when you open a connection by using the **SqlConnection.Open** method.</span></span> <span data-ttu-id="a0597-223">Den **öppna** metoden omfattar nu bästa prestanda försök mekanismer som svar på tillfälliga problem för vissa fel inom tidsgränsen för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="a0597-223">The **Open** method now incorporates best effort retry mechanisms in response to transient faults, for certain errors within the Connection Timeout period.</span></span>
* <span data-ttu-id="a0597-224">Stöder anslutningspooler.</span><span class="sxs-lookup"><span data-stu-id="a0597-224">Supports connection pooling.</span></span> <span data-ttu-id="a0597-225">Detta inkluderar en effektiv kontroll av att det anslutningsobjekt som den ger programmet fungerar.</span><span class="sxs-lookup"><span data-stu-id="a0597-225">This includes an efficient verification that the connection object it gives your program is functioning.</span></span>

<span data-ttu-id="a0597-226">När du använder ett anslutningsobjekt från en anslutningspool, rekommenderar vi att programmet tillfälligt stänga anslutningen när du använder den inte omedelbart.</span><span class="sxs-lookup"><span data-stu-id="a0597-226">When you use a connection object from a connection pool, we recommend that your program temporarily close the connection when not immediately using it.</span></span> <span data-ttu-id="a0597-227">Öppna en anslutning är inte dyr sättet att skapa en ny anslutning.</span><span class="sxs-lookup"><span data-stu-id="a0597-227">Re-opening a connection is not expensive the way creating a new connection is.</span></span>

<span data-ttu-id="a0597-228">Om du använder ADO.NET 4.0 eller tidigare, rekommenderar vi att du uppgraderar till den senaste ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="a0597-228">If you are using ADO.NET 4.0 or earlier, we recommend that you upgrade to the latest ADO.NET.</span></span>

* <span data-ttu-id="a0597-229">Från och med November 2015, kan du [hämta ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).</span><span class="sxs-lookup"><span data-stu-id="a0597-229">As of November 2015, you can [download ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).</span></span>

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a><span data-ttu-id="a0597-230">Diagnostik</span><span class="sxs-lookup"><span data-stu-id="a0597-230">Diagnostics</span></span>
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a><span data-ttu-id="a0597-231">Diagnostik: Testa om verktyg kan ansluta</span><span class="sxs-lookup"><span data-stu-id="a0597-231">Diagnostics: Test whether utilities can connect</span></span>
<span data-ttu-id="a0597-232">Om programmet inte kan ansluta till Azure SQL Database, kan diagnostiska du prova att ansluta till ett program.</span><span class="sxs-lookup"><span data-stu-id="a0597-232">If your program is failing to connect to Azure SQL Database, one diagnostic option is to try to connect with a utility program.</span></span> <span data-ttu-id="a0597-233">Verktyget skulle helst ansluta genom att använda samma bibliotek som används i programmet.</span><span class="sxs-lookup"><span data-stu-id="a0597-233">Ideally the utility would connect by using the same library that your program uses.</span></span>

<span data-ttu-id="a0597-234">Du kan försöka verktygen på alla Windows-datorer:</span><span class="sxs-lookup"><span data-stu-id="a0597-234">On any Windows computer, you can try these utilities:</span></span>

* <span data-ttu-id="a0597-235">SQL Server Management Studio (ssms.exe) som ansluter med hjälp av ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="a0597-235">SQL Server Management Studio (ssms.exe), which connects by using ADO.NET.</span></span>
* <span data-ttu-id="a0597-236">SQLCMD.exe som ansluter med hjälp av [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).</span><span class="sxs-lookup"><span data-stu-id="a0597-236">sqlcmd.exe, which connects by using [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).</span></span>

<span data-ttu-id="a0597-237">När du är ansluten, testa om ett kort SQL SELECT-frågan fungerar.</span><span class="sxs-lookup"><span data-stu-id="a0597-237">Once connected, test whether a short SQL SELECT query works.</span></span>

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a><span data-ttu-id="a0597-238">Diagnostik: Kontrollera öppna portar</span><span class="sxs-lookup"><span data-stu-id="a0597-238">Diagnostics: Check the open ports</span></span>
<span data-ttu-id="a0597-239">Anta att du misstänker att anslutningsförsök misslyckas på grund av problem med port.</span><span class="sxs-lookup"><span data-stu-id="a0597-239">Suppose you suspect that connection attempts are failing due to port issues.</span></span> <span data-ttu-id="a0597-240">Du kan köra ett verktyg som rapporter om portkonfigurationerna som på datorn.</span><span class="sxs-lookup"><span data-stu-id="a0597-240">On your computer you can run a utility that reports on the port configurations.</span></span>

<span data-ttu-id="a0597-241">Följande verktyg kan vara användbart i Linux:</span><span class="sxs-lookup"><span data-stu-id="a0597-241">On Linux the following utilities might be helpful:</span></span>

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * <span data-ttu-id="a0597-242">(Ändra exempelvärde ska vara din IP-adress).</span><span class="sxs-lookup"><span data-stu-id="a0597-242">(Change the example value to be your IP address.)</span></span>

<span data-ttu-id="a0597-243">I Windows på [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) verktyg kan vara till hjälp.</span><span class="sxs-lookup"><span data-stu-id="a0597-243">On Windows the [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) utility might be helpful.</span></span> <span data-ttu-id="a0597-244">Här är ett exempel körning som efterfrågas situationen port på en Azure SQL Database-server och som kördes på en bärbar dator:</span><span class="sxs-lookup"><span data-stu-id="a0597-244">Here is an example execution that queried the port situation on an Azure SQL Database server, and which was run on a laptop computer:</span></span>

```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a><span data-ttu-id="a0597-245">Diagnostik: Logga din fel</span><span class="sxs-lookup"><span data-stu-id="a0597-245">Diagnostics: Log your errors</span></span>
<span data-ttu-id="a0597-246">Ett tillfälligt problem diagnostiserats ibland bäst av identifiering av ett allmänt mönster över dagar eller veckor.</span><span class="sxs-lookup"><span data-stu-id="a0597-246">An intermittent problem is sometimes best diagnosed by detection of a general pattern over days or weeks.</span></span>

<span data-ttu-id="a0597-247">Klienten kan hjälpa en diagnos genom att logga alla fel som påträffas.</span><span class="sxs-lookup"><span data-stu-id="a0597-247">Your client can assist in a diagnosis by logging all errors it encounters.</span></span> <span data-ttu-id="a0597-248">Du kanske kan korrelera loggposterna med Feldata Azure SQL Database loggar själva internt.</span><span class="sxs-lookup"><span data-stu-id="a0597-248">You might be able to correlate the log entries with error data that Azure SQL Database logs itself internally.</span></span>

<span data-ttu-id="a0597-249">Enterprise-biblioteket 6 (EntLib60) ger hanterad klasser för att hjälpa till med loggning:</span><span class="sxs-lookup"><span data-stu-id="a0597-249">Enterprise Library 6 (EntLib60) offers .NET managed classes to assist with logging:</span></span>

* [<span data-ttu-id="a0597-250">5 - lika enkelt som att falla en logg: använda loggning programmet Block</span><span class="sxs-lookup"><span data-stu-id="a0597-250">5 - As Easy As Falling Off a Log: Using the Logging Application Block</span></span>](http://msdn.microsoft.com/library/dn440731.aspx)

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a><span data-ttu-id="a0597-251">Diagnostik: Granska systemloggar för fel</span><span class="sxs-lookup"><span data-stu-id="a0597-251">Diagnostics: Examine system logs for errors</span></span>
<span data-ttu-id="a0597-252">Här följer några Transact-SQL SELECT-satser som frågeloggar av fel och annan information.</span><span class="sxs-lookup"><span data-stu-id="a0597-252">Here are some Transact-SQL SELECT statements that query logs of error and other information.</span></span>

| <span data-ttu-id="a0597-253">Frågan för logg</span><span class="sxs-lookup"><span data-stu-id="a0597-253">Query of log</span></span> | <span data-ttu-id="a0597-254">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a0597-254">Description</span></span> |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |<span data-ttu-id="a0597-255">Den [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) vyn innehåller information om hur enskilda händelser, inklusive några som kan orsaka tillfälliga fel eller anslutningsfel.</span><span class="sxs-lookup"><span data-stu-id="a0597-255">The [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) view offers information about individual events, including some that can cause transient errors or connectivity failures.</span></span><br/><br/><span data-ttu-id="a0597-256">Förhoppningsvis kan du jämföra den **starttid** eller **end_time** värden med information om när dina klientprogram stötte på problem.</span><span class="sxs-lookup"><span data-stu-id="a0597-256">Ideally you can correlate the **start_time** or **end_time** values with information about when your client program experienced problems.</span></span><br/><br/><span data-ttu-id="a0597-257">**Tips:** måste du ansluta till den **master** databasen för att köra det här.</span><span class="sxs-lookup"><span data-stu-id="a0597-257">**TIP:** You must connect to the **master** database to run this.</span></span> |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |<span data-ttu-id="a0597-258">Den [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) visa erbjuder aggregerade antal händelsetyper för ytterligare diagnostik.</span><span class="sxs-lookup"><span data-stu-id="a0597-258">The [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) view offers aggregated counts of event types, for additional diagnostics.</span></span><br/><br/><span data-ttu-id="a0597-259">**Tips:** måste du ansluta till den **master** databasen för att köra det här.</span><span class="sxs-lookup"><span data-stu-id="a0597-259">**TIP:** You must connect to the **master** database to run this.</span></span> |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a><span data-ttu-id="a0597-260">Diagnostik: Sök efter problem händelser i loggen för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="a0597-260">Diagnostics: Search for problem events in the SQL Database log</span></span>
<span data-ttu-id="a0597-261">Du kan söka efter poster om problemet händelser i loggen för Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a0597-261">You can search for entries about problem events in the log of Azure SQL Database.</span></span> <span data-ttu-id="a0597-262">Försök följande Transact-SQL SELECT-uttryck i den **master** databasen:</span><span class="sxs-lookup"><span data-stu-id="a0597-262">Try the following Transact-SQL SELECT statement in the **master** database:</span></span>

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


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a><span data-ttu-id="a0597-263">Några returnerade rader från sys.fn_xe_telemetry_blob_target_read_file</span><span class="sxs-lookup"><span data-stu-id="a0597-263">A few returned rows from sys.fn_xe_telemetry_blob_target_read_file</span></span>
<span data-ttu-id="a0597-264">Nästa är en returnerad rad kan se ut.</span><span class="sxs-lookup"><span data-stu-id="a0597-264">Next is what a returned row might look like.</span></span> <span data-ttu-id="a0597-265">Null-värden visas är ofta inte null i andra rader.</span><span class="sxs-lookup"><span data-stu-id="a0597-265">The null values shown are often not null in other rows.</span></span>

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a><span data-ttu-id="a0597-266">Enterprise Library 6</span><span class="sxs-lookup"><span data-stu-id="a0597-266">Enterprise Library 6</span></span>
<span data-ttu-id="a0597-267">Enterprise-biblioteket 6 (EntLib60) är ett ramverk för .NET-klasser som hjälper dig att implementera robust klienter för molntjänster, varav en är Azure SQL Database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a0597-267">Enterprise Library 6 (EntLib60) is a framework of .NET classes that helps you implement robust clients of cloud services, one of which is the Azure SQL Database service.</span></span> <span data-ttu-id="a0597-268">Du kan hitta avsnitt som är dedikerad till varje område där EntLib60 kan hjälpa genom att besöka första:</span><span class="sxs-lookup"><span data-stu-id="a0597-268">You can locate topics dedicated to each area in which EntLib60 can assist by first visiting:</span></span>

* [<span data-ttu-id="a0597-269">Enterprise Library 6 – April 2013</span><span class="sxs-lookup"><span data-stu-id="a0597-269">Enterprise Library 6 - April 2013</span></span>](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)

<span data-ttu-id="a0597-270">Logik för att hantera tillfälliga fel är en plats där EntLib60 kan hjälpa.</span><span class="sxs-lookup"><span data-stu-id="a0597-270">Retry logic for handling transient errors is one area in which EntLib60 can assist:</span></span>

* [<span data-ttu-id="a0597-271">4 - perseverance, hemligheten för alla framgångar: använder tillfälligt fel hantering programmet Block</span><span class="sxs-lookup"><span data-stu-id="a0597-271">4 - Perseverance, Secret of All Triumphs: Using the Transient Fault Handling Application Block</span></span>](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)

> [!NOTE]
> <span data-ttu-id="a0597-272">Källkoden för EntLib60 är tillgängligt för allmänheten [hämta](http://go.microsoft.com/fwlink/p/?LinkID=290898).</span><span class="sxs-lookup"><span data-stu-id="a0597-272">The source code for EntLib60 is available for public [download](http://go.microsoft.com/fwlink/p/?LinkID=290898).</span></span> <span data-ttu-id="a0597-273">Microsoft har inga planer på att göra ytterligare funktionsuppdateringar eller uppdateringar för underhåll EntLib.</span><span class="sxs-lookup"><span data-stu-id="a0597-273">Microsoft has no plans to make further feature updates or maintenance updates to EntLib.</span></span>
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a><span data-ttu-id="a0597-274">EntLib60 klasser för tillfälliga fel och försök igen</span><span class="sxs-lookup"><span data-stu-id="a0597-274">EntLib60 classes for transient errors and retry</span></span>
<span data-ttu-id="a0597-275">Följande EntLib60 klasser är särskilt användbara för logik.</span><span class="sxs-lookup"><span data-stu-id="a0597-275">The following EntLib60 classes are particularly useful for retry logic.</span></span> <span data-ttu-id="a0597-276">Alla dessa, eller ytterligare under namnområdet **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:</span><span class="sxs-lookup"><span data-stu-id="a0597-276">All these  are in, or are further under, the namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:</span></span>

<span data-ttu-id="a0597-277">*I namnområdet **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*</span><span class="sxs-lookup"><span data-stu-id="a0597-277">*In the namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*</span></span>

* <span data-ttu-id="a0597-278">**RetryPolicy** klass</span><span class="sxs-lookup"><span data-stu-id="a0597-278">**RetryPolicy** class</span></span>
  
  * <span data-ttu-id="a0597-279">**ExecuteAction** metod</span><span class="sxs-lookup"><span data-stu-id="a0597-279">**ExecuteAction** method</span></span>
* <span data-ttu-id="a0597-280">**ExponentialBackoff** klass</span><span class="sxs-lookup"><span data-stu-id="a0597-280">**ExponentialBackoff** class</span></span>
* <span data-ttu-id="a0597-281">**SqlDatabaseTransientErrorDetectionStrategy** klass</span><span class="sxs-lookup"><span data-stu-id="a0597-281">**SqlDatabaseTransientErrorDetectionStrategy** class</span></span>
* <span data-ttu-id="a0597-282">**ReliableSqlConnection** klass</span><span class="sxs-lookup"><span data-stu-id="a0597-282">**ReliableSqlConnection** class</span></span>
  
  * <span data-ttu-id="a0597-283">**ExecuteCommand** metod</span><span class="sxs-lookup"><span data-stu-id="a0597-283">**ExecuteCommand** method</span></span>

<span data-ttu-id="a0597-284">I namnområdet **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:</span><span class="sxs-lookup"><span data-stu-id="a0597-284">In the namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:</span></span>

* <span data-ttu-id="a0597-285">**AlwaysTransientErrorDetectionStrategy** klass</span><span class="sxs-lookup"><span data-stu-id="a0597-285">**AlwaysTransientErrorDetectionStrategy** class</span></span>
* <span data-ttu-id="a0597-286">**NeverTransientErrorDetectionStrategy** klass</span><span class="sxs-lookup"><span data-stu-id="a0597-286">**NeverTransientErrorDetectionStrategy** class</span></span>

<span data-ttu-id="a0597-287">Här är länkar till information om EntLib60:</span><span class="sxs-lookup"><span data-stu-id="a0597-287">Here are links to information about EntLib60:</span></span>

* <span data-ttu-id="a0597-288">Ledigt [bok Download: Utvecklarhandbok Microsoft Enterprise-biblioteket, 2 Edition](http://www.microsoft.com/download/details.aspx?id=41145)</span><span class="sxs-lookup"><span data-stu-id="a0597-288">Free [Book Download: Developer's Guide to Microsoft Enterprise Library, 2nd Edition](http://www.microsoft.com/download/details.aspx?id=41145)</span></span>
* <span data-ttu-id="a0597-289">Bästa praxis: [försök allmänna riktlinjer](../best-practices-retry-general.md) har en utmärkt detaljerad beskrivning av logik.</span><span class="sxs-lookup"><span data-stu-id="a0597-289">Best practices: [Retry general guidance](../best-practices-retry-general.md) has an excellent in-depth discussion of retry logic.</span></span>
* <span data-ttu-id="a0597-290">Hämta NuGet [Enterprise Library - hantering av tillfälliga fel program block 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)</span><span class="sxs-lookup"><span data-stu-id="a0597-290">NuGet download of [Enterprise Library - Transient Fault Handling application block 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)</span></span>

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a><span data-ttu-id="a0597-291">EntLib60: Loggning block</span><span class="sxs-lookup"><span data-stu-id="a0597-291">EntLib60: The logging block</span></span>
* <span data-ttu-id="a0597-292">Loggning blocket är en mycket flexibelt och konfigurerbara lösning som hjälper dig att:</span><span class="sxs-lookup"><span data-stu-id="a0597-292">The Logging block is a highly flexible and configurable solution that allows you to:</span></span>
  
  * <span data-ttu-id="a0597-293">Skapa och lagra meddelanden i en mängd olika platser.</span><span class="sxs-lookup"><span data-stu-id="a0597-293">Create and store log messages in a wide variety of locations.</span></span>
  * <span data-ttu-id="a0597-294">Kategorisera och filtrera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a0597-294">Categorize and filter messages.</span></span>
  * <span data-ttu-id="a0597-295">Samla in relevant information som är användbart för felsökning och spårning och för granskning och allmänna loggning krav.</span><span class="sxs-lookup"><span data-stu-id="a0597-295">Collect contextual information that is useful for debugging and tracing, as well as for auditing and general logging requirements.</span></span>
* <span data-ttu-id="a0597-296">Loggning blocket avlägsnar loggningsfunktionen från loggmålet så att programkoden är konsekvent, oavsett plats och typ för arkivet mål loggning.</span><span class="sxs-lookup"><span data-stu-id="a0597-296">The Logging block abstracts the logging functionality from the log destination so that the application code is consistent, irrespective of the location and type of the target logging store.</span></span>

<span data-ttu-id="a0597-297">Mer information finns: [5 – som enkelt som som omfattas av av en logg: med blocket loggning program](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="a0597-297">For details see: [5 - As Easy As Falling Off a Log: Using the Logging Application Block](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)</span></span>

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a><span data-ttu-id="a0597-298">Källkoden för EntLib60 IsTransient metod</span><span class="sxs-lookup"><span data-stu-id="a0597-298">EntLib60 IsTransient method source code</span></span>
<span data-ttu-id="a0597-299">Nästa från den **SqlDatabaseTransientErrorDetectionStrategy** klassen, är den C#-källkoden för den **IsTransient** metod.</span><span class="sxs-lookup"><span data-stu-id="a0597-299">Next, from the **SqlDatabaseTransientErrorDetectionStrategy** class, is the C# source code for the **IsTransient** method.</span></span> <span data-ttu-id="a0597-300">Källkoden klargör vilka fel ansågs vara tillfälligt och worthy på försök igen, från och med April 2013.</span><span class="sxs-lookup"><span data-stu-id="a0597-300">The source code clarifies which errors were considered to be transient and worthy of retry, as of April 2013.</span></span>

<span data-ttu-id="a0597-301">Ett stort antal **//comment** rader har tagits bort från den här kopian att betona läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="a0597-301">Numerous **//comment** lines have been removed from this copy to emphasize readability.</span></span>

```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
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
            // The instance of SQL Server you attempted to connect to
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


## <a name="next-steps"></a><span data-ttu-id="a0597-302">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0597-302">Next steps</span></span>
* <span data-ttu-id="a0597-303">Felsöka andra vanliga problem med Azure SQL Database anslutningen finns [felsökning av problem med anslutningen till Azure SQL Database](sql-database-troubleshoot-common-connection-issues.md).</span><span class="sxs-lookup"><span data-stu-id="a0597-303">For troubleshooting other common Azure SQL Database connection issues, visit [Troubleshoot connection issues to Azure SQL Database](sql-database-troubleshoot-common-connection-issues.md).</span></span>
* [<span data-ttu-id="a0597-304">SQL Server anslutningspoolen (ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="a0597-304">SQL Server Connection Pooling (ADO.NET)</span></span>](http://msdn.microsoft.com/library/8xx3tyca.aspx)
* [<span data-ttu-id="a0597-305">*Du försöker* är ett Apache 2.0 licensierad allmänna du försöker biblioteket som skrivits i **Python**, för att förenkla uppgiften att lägga till försök beteende till bara om något.</span><span class="sxs-lookup"><span data-stu-id="a0597-305">*Retrying* is an Apache 2.0 licensed general-purpose retrying library, written in **Python**, to simplify the task of adding retry behavior to just about anything.</span></span>](https://pypi.python.org/pypi/retrying)

