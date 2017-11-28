---
title: "Programmeringsguide för aaaSCP.NET | Microsoft Docs"
description: "Lär dig hur toouse SCP.NET toocreate. NET-baserade Storm-topologier för att använda med Storm på HDInsight."
services: hdinsight
documentationcenter: 
author: raviperi
manager: jhubbard
editor: cgronlun
ms.assetid: 34192ed0-b1d1-4cf7-a3d4-5466301cf307
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/16/2016
ms.author: raviperi
ms.openlocfilehash: a57f4217b07e0e82a3f36650308695fbb45d9128
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scp-programming-guide"></a><span data-ttu-id="35ef0-103">Programmeringsguide för SCP</span><span class="sxs-lookup"><span data-stu-id="35ef0-103">SCP programming guide</span></span>
<span data-ttu-id="35ef0-104">SCP är en plattform toobuild realtid, tillförlitliga och konsekventa hög prestanda databearbetning program.</span><span class="sxs-lookup"><span data-stu-id="35ef0-104">SCP is a platform toobuild real time, reliable, consistent and high performance data processing application.</span></span> <span data-ttu-id="35ef0-105">Det är byggt ovanpå [Apache Storm](http://storm.incubator.apache.org/) – en dataström som bearbetar system utformas av hello OSS communities.</span><span class="sxs-lookup"><span data-stu-id="35ef0-105">It is built on top of [Apache Storm](http://storm.incubator.apache.org/) -- a stream processing system designed by hello OSS communities.</span></span> <span data-ttu-id="35ef0-106">Storm är utformad med Nathan Marz och öppen källkod med Twitter.</span><span class="sxs-lookup"><span data-stu-id="35ef0-106">Storm is designed by Nathan Marz and open sourced by Twitter.</span></span> <span data-ttu-id="35ef0-107">Den använder [Apache ZooKeeper](http://zookeeper.apache.org/), en annan Apache projektet tooenable mycket pålitlig distribuerade samordning och tillståndshantering.</span><span class="sxs-lookup"><span data-stu-id="35ef0-107">It leverages [Apache ZooKeeper](http://zookeeper.apache.org/), another Apache project tooenable highly reliable distributed coordination and state management.</span></span> 

<span data-ttu-id="35ef0-108">Inte bara hello SCP projektet portar Storm på Windows utan också hello-projektet till tillägg och anpassning av hello Windows-ekosystemet.</span><span class="sxs-lookup"><span data-stu-id="35ef0-108">Not only hello SCP project ported Storm on Windows but also hello project added extensions and customization for hello Windows ecosystem.</span></span> <span data-ttu-id="35ef0-109">hello tillägg inkluderar .NET developer upplevelse och bibliotek, hello anpassning innehåller Windows-baserad distribution.</span><span class="sxs-lookup"><span data-stu-id="35ef0-109">hello extensions include .NET developer experience, and libraries, hello customization includes Windows-based deployment.</span></span> 

<span data-ttu-id="35ef0-110">hello-tillägget och anpassning görs så att vi behöver inte toofork hello OSS projekt och vi kan utnyttja härledda ekosystem byggda på Storm.</span><span class="sxs-lookup"><span data-stu-id="35ef0-110">hello extension and customization is done in such a way that we do not need toofork hello OSS projects and we could leverage derived ecosystems built on top of Storm.</span></span>

## <a name="processing-model"></a><span data-ttu-id="35ef0-111">Processmodell</span><span class="sxs-lookup"><span data-stu-id="35ef0-111">Processing model</span></span>
<span data-ttu-id="35ef0-112">hello data i SCP modelleras som kontinuerliga strömmar av tupplar.</span><span class="sxs-lookup"><span data-stu-id="35ef0-112">hello data in SCP is modeled as continuous streams of tuples.</span></span> <span data-ttu-id="35ef0-113">Vanligtvis hello tupplar flöda till vissa kö först och sedan tas upp och transformeras av affärslogik som finns inuti en Storm-topologi, slutligen hello utdata kan skickas som tupplar tooanother SCP system eller vara allokerat toostores som distribuerat filsystem eller databaser som SQL Server.</span><span class="sxs-lookup"><span data-stu-id="35ef0-113">Typically hello tuples flow into some queue first, then picked up, and transformed by business logic hosted inside a Storm topology, finally hello output could be piped as tuples tooanother SCP system, or be committed toostores like distributed file system or databases like SQL Server.</span></span>

![Ett diagram av en kö mata data tooprocessing som ett datalager-flöden](media/hdinsight-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

<span data-ttu-id="35ef0-115">I Storm definierar en Programtopologi ett diagram över beräkning.</span><span class="sxs-lookup"><span data-stu-id="35ef0-115">In Storm, an application topology defines a graph of computation.</span></span> <span data-ttu-id="35ef0-116">Varje nod i en topologi innehåller logik för bearbetning och länkar mellan noder ange dataflöde.</span><span class="sxs-lookup"><span data-stu-id="35ef0-116">Each node in a topology contains processing logic, and links between nodes indicate data flow.</span></span> <span data-ttu-id="35ef0-117">hello noder tooinject indata till hello topologi kallas kanaler, vilket kan vara används toosequence hello data.</span><span class="sxs-lookup"><span data-stu-id="35ef0-117">hello nodes tooinject input data into hello topology are called Spouts, which can be used toosequence hello data.</span></span> <span data-ttu-id="35ef0-118">hello indata kan finnas i filen loggar, transaktionella databas, system-prestandaräknaren etc. hello noder med både inkommande och utgående dataflöden kallas bultar, vilket hello faktiska data filtrering och val och aggregering.</span><span class="sxs-lookup"><span data-stu-id="35ef0-118">hello input data could reside in file logs, transactional database, system performance counter etc. hello nodes with both input and output data flows are called Bolts, which do hello actual data filtering and selections and aggregation.</span></span>

<span data-ttu-id="35ef0-119">Tjänstanslutningspunkten stöder bästa för på-minst en gång och exakt-behandling av data en gång.</span><span class="sxs-lookup"><span data-stu-id="35ef0-119">SCP supports best efforts, at-least-once and exactly-once data processing.</span></span> <span data-ttu-id="35ef0-120">I ett distribuerat strömmande bearbetning-program kan olika fel inträffa under bearbetning av data, till exempel nätverksavbrott, maskinvarufel eller användarfel kod osv. På-minst en gång process säkerställer att alla data bearbetas minst en gång genom att spela upp automatiskt hello samma data när fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="35ef0-120">In a distributed streaming processing application, various errors may happen during data processing, such as network outage, machine failure, or user code error etc. At-least-once processing ensures all data will be processed at least once by replaying automatically hello same data when error happens.</span></span> <span data-ttu-id="35ef0-121">På-minst en gång bearbetning är enkel och tillförlitlig och passar bra i många program.</span><span class="sxs-lookup"><span data-stu-id="35ef0-121">At-least-once processing is simple and reliable and suits well in many applications.</span></span> <span data-ttu-id="35ef0-122">Men när programmet hello kräver exakt inventering, räcker till exempel på-minst en gång bearbetning inte eftersom hello samma data kan potentiellt spelas upp i hello programmet topologi.</span><span class="sxs-lookup"><span data-stu-id="35ef0-122">However, when hello application requires exact counting, for example, at-least-once processing is insufficient since hello same data could potentially be played in hello application topology.</span></span> <span data-ttu-id="35ef0-123">I så fall, exakt-när bearbetningen är utformat toomake att hello resultatet är korrekt när hello data kan spelas och behandlas flera gånger.</span><span class="sxs-lookup"><span data-stu-id="35ef0-123">In that case, exactly-once processing is designed toomake sure hello result is correct even when hello data may be replayed and processed multiple times.</span></span>

<span data-ttu-id="35ef0-124">SCP ger .NET-utvecklare toodevelop realtid processen program utnyttjar hello Java Virtual Machine (JVM) baserat Storm under hello skydd.</span><span class="sxs-lookup"><span data-stu-id="35ef0-124">SCP enables .NET developers toodevelop real time data process applications while leverage hello Java Virtual Machine (JVM) based Storm under hello cover.</span></span> <span data-ttu-id="35ef0-125">hello .NET och JVM kommunicerar via TCP lokal socket.</span><span class="sxs-lookup"><span data-stu-id="35ef0-125">hello .NET and JVM communicate via TCP local socket.</span></span> <span data-ttu-id="35ef0-126">I praktiken är varje kanal/bult paret .net/Java processen där hello användaren logik körs i .net-process som ett plugin-program.</span><span class="sxs-lookup"><span data-stu-id="35ef0-126">Basically each Spout/Bolt is a .Net/Java process pair, where hello user logic runs in .Net process as a plugin.</span></span>

<span data-ttu-id="35ef0-127">toobuild en databearbetningen program ovanpå SCP, flera steg behövs:</span><span class="sxs-lookup"><span data-stu-id="35ef0-127">toobuild a data processing application on top of SCP, several steps are needed:</span></span>

* <span data-ttu-id="35ef0-128">Utforma och implementera hello kanaler toopull i data från kön.</span><span class="sxs-lookup"><span data-stu-id="35ef0-128">Design and implement hello Spouts toopull in data from queue.</span></span>
* <span data-ttu-id="35ef0-129">Utforma och implementera bultar tooprocess hello indata och spara tooexternal datalager, till exempel databasen.</span><span class="sxs-lookup"><span data-stu-id="35ef0-129">Design and implement Bolts tooprocess hello input data, and save data tooexternal stores such as Database.</span></span>
* <span data-ttu-id="35ef0-130">Utforma hello topologi, skicka och kör hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="35ef0-130">Design hello topology, then submit and run hello topology.</span></span> <span data-ttu-id="35ef0-131">hello topologi definierar brytpunkter och hello data flödar mellan hello noder.</span><span class="sxs-lookup"><span data-stu-id="35ef0-131">hello topology defines vertexes and hello data flows between hello vertexes.</span></span> <span data-ttu-id="35ef0-132">SCP tar hello topologi-specifikationen och distribuera den på ett Storm-kluster där varje vertex körs på en logisk nod.</span><span class="sxs-lookup"><span data-stu-id="35ef0-132">SCP will take hello topology specification and deploy it on a Storm cluster, where each vertex runs on one logical node.</span></span> <span data-ttu-id="35ef0-133">hello redundans och skalning kommer typen av hello Storm-Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="35ef0-133">hello failover and scaling will be taken care of by hello Storm task scheduler.</span></span>

<span data-ttu-id="35ef0-134">Det här dokumentet kommer att använda några enkla exempel toowalk igenom hur toobuild databearbetning program med SCP.</span><span class="sxs-lookup"><span data-stu-id="35ef0-134">This document will use some simple examples toowalk through how toobuild data processing application with SCP.</span></span>

## <a name="scp-plugin-interface"></a><span data-ttu-id="35ef0-135">SCP-Plugin-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="35ef0-135">SCP Plugin Interface</span></span>
<span data-ttu-id="35ef0-136">SCP-plugin-program (eller program) är fristående EXEs som kan både körs i Visual Studio under hello utvecklingsfasen och anslutas till hello Storm pipeline efter distributionen i produktion.</span><span class="sxs-lookup"><span data-stu-id="35ef0-136">SCP plugins (or applications) are standalone EXEs that can both run inside Visual Studio during hello development phase, and be plugged into hello Storm pipeline after deployment in production.</span></span> <span data-ttu-id="35ef0-137">Skriva hello SCP-plugin-programmet är bara hello samma som att skriva andra standard Windows konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="35ef0-137">Writing hello SCP plugin is just hello same as writing any other standard Windows console applications.</span></span> <span data-ttu-id="35ef0-138">SCP.NET plattform deklarerar vissa gränssnitt för kanal-/ bult och hello användarkod plugin-programmet ska implementera dessa gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="35ef0-138">SCP.NET platform declares some interface for spout/bolt, and hello user plugin code should implement these interfaces.</span></span> <span data-ttu-id="35ef0-139">hello Huvudsyftet med den här designen är hello användaren kan fokusera på sina egna business logics och låta andra saker toobe som hanteras av SCP.NET plattform.</span><span class="sxs-lookup"><span data-stu-id="35ef0-139">hello main purpose of this design is that hello user can focus on their own business logics, and leaving other things toobe handled by SCP.NET platform.</span></span>

<span data-ttu-id="35ef0-140">hello användarkod plugin-programmet ska implementera ett av hello följande gränssnitt, beror på om hello topologin är transaktionell eller icke-transaktionell och om hello komponenten är kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="35ef0-140">hello user plugin code should implement one of hello followings interfaces, depends on whether hello topology is transactional or non-transactional, and whether hello component is spout or bolt.</span></span>

* <span data-ttu-id="35ef0-141">ISCPSpout</span><span class="sxs-lookup"><span data-stu-id="35ef0-141">ISCPSpout</span></span>
* <span data-ttu-id="35ef0-142">ISCPBolt</span><span class="sxs-lookup"><span data-stu-id="35ef0-142">ISCPBolt</span></span>
* <span data-ttu-id="35ef0-143">ISCPTxSpout</span><span class="sxs-lookup"><span data-stu-id="35ef0-143">ISCPTxSpout</span></span>
* <span data-ttu-id="35ef0-144">ISCPBatchBolt</span><span class="sxs-lookup"><span data-stu-id="35ef0-144">ISCPBatchBolt</span></span>

### <a name="iscpplugin"></a><span data-ttu-id="35ef0-145">ISCPPlugin</span><span class="sxs-lookup"><span data-stu-id="35ef0-145">ISCPPlugin</span></span>
<span data-ttu-id="35ef0-146">ISCPPlugin är hello gemensamt gränssnitt för alla typer av plugin-program.</span><span class="sxs-lookup"><span data-stu-id="35ef0-146">ISCPPlugin is hello common interface for all kinds of plugins.</span></span> <span data-ttu-id="35ef0-147">Det är för närvarande ett dummy gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="35ef0-147">Currently, it is a dummy interface.</span></span>

    public interface ISCPPlugin 
    {
    }

### <a name="iscpspout"></a><span data-ttu-id="35ef0-148">ISCPSpout</span><span class="sxs-lookup"><span data-stu-id="35ef0-148">ISCPSpout</span></span>
<span data-ttu-id="35ef0-149">ISCPSpout är hello gränssnitt för icke-transaktionell kanal.</span><span class="sxs-lookup"><span data-stu-id="35ef0-149">ISCPSpout is hello interface for non-transactional spout.</span></span>

     public interface ISCPSpout : ISCPPlugin                    
     {
         void NextTuple(Dictionary<string, Object> parms);         
         void Ack(long seqId, Dictionary<string, Object> parms);   
         void Fail(long seqId, Dictionary<string, Object> parms);  
     }

<span data-ttu-id="35ef0-150">När `NextTuple()` anropas, hello C\# användarkod kan generera en eller flera tupplar.</span><span class="sxs-lookup"><span data-stu-id="35ef0-150">When `NextTuple()` is called, hello C\# user code can emit one or more tuples.</span></span> <span data-ttu-id="35ef0-151">Om det inte finns något tooemit, som den här metoden ska returnera utan avger något.</span><span class="sxs-lookup"><span data-stu-id="35ef0-151">If there is nothing tooemit, this method should return without emitting anything.</span></span> <span data-ttu-id="35ef0-152">Det bör noteras som `NextTuple()`, `Ack()`, och `Fail()` kallas i en tät loop i en enskild tråd i C\# process.</span><span class="sxs-lookup"><span data-stu-id="35ef0-152">It should be noted that `NextTuple()`, `Ack()`, and `Fail()` are all called in a tight loop in a single thread in C\# process.</span></span> <span data-ttu-id="35ef0-153">När det finns inga tupplar tooemit, är Välkommen företagspolicy toohave NextTuple strömsparläge för kort tid (till exempel 10 millisekunder) som inte toowaste för mycket CPU.</span><span class="sxs-lookup"><span data-stu-id="35ef0-153">When there are no tuples tooemit, it is courteous toohave NextTuple sleep for a short amount of time (such as 10 milliseconds) so as not toowaste too much CPU.</span></span>

<span data-ttu-id="35ef0-154">`Ack()`och `Fail()` ska bara anropas om ack mekanism är aktiverat i spec filen.</span><span class="sxs-lookup"><span data-stu-id="35ef0-154">`Ack()` and `Fail()` will be called only when ack mechanism is enabled in spec file.</span></span> <span data-ttu-id="35ef0-155">Hej `seqId` är används tooidentify hello tuppel som ADE eller misslyckades.</span><span class="sxs-lookup"><span data-stu-id="35ef0-155">hello `seqId` is used tooidentify hello tuple which is acked or failed.</span></span> <span data-ttu-id="35ef0-156">Så om ack är aktiverad i icke-transaktionell topologi ska hello följande begär funktionen användas i kanal:</span><span class="sxs-lookup"><span data-stu-id="35ef0-156">So if ack is enabled in non-transactional topology, hello following emit function should be used in Spout:</span></span>

    public abstract void Emit(string streamId, List<object> values, long seqId); 

<span data-ttu-id="35ef0-157">Om ack inte stöds i icke-transaktionell topologi, hello `Ack()` och `Fail()` kan lämnas tomt funktion.</span><span class="sxs-lookup"><span data-stu-id="35ef0-157">If ack is not supported in non-transactional topology, hello `Ack()` and `Fail()` can be left as empty function.</span></span>

<span data-ttu-id="35ef0-158">Hej `parms` indataparametrar i dessa funktioner är bara tom ordlista, de är reserverad för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="35ef0-158">hello `parms` input parameters in these functions are just empty Dictionary, they are reserved for future use.</span></span>

### <a name="iscpbolt"></a><span data-ttu-id="35ef0-159">ISCPBolt</span><span class="sxs-lookup"><span data-stu-id="35ef0-159">ISCPBolt</span></span>
<span data-ttu-id="35ef0-160">ISCPBolt är hello gränssnitt för icke-transaktionell bulten.</span><span class="sxs-lookup"><span data-stu-id="35ef0-160">ISCPBolt is hello interface for non-transactional bolt.</span></span>

    public interface ISCPBolt : ISCPPlugin 
    {
    void Execute(SCPTuple tuple);           
    }

<span data-ttu-id="35ef0-161">När det finns nya tuppel hello `Execute()` funktionen anropas tooprocess den.</span><span class="sxs-lookup"><span data-stu-id="35ef0-161">When new tuple is available, hello `Execute()` function will be called tooprocess it.</span></span>

### <a name="iscptxspout"></a><span data-ttu-id="35ef0-162">ISCPTxSpout</span><span class="sxs-lookup"><span data-stu-id="35ef0-162">ISCPTxSpout</span></span>
<span data-ttu-id="35ef0-163">ISCPTxSpout är hello gränssnitt för transaktionell kanal.</span><span class="sxs-lookup"><span data-stu-id="35ef0-163">ISCPTxSpout is hello interface for transactional spout.</span></span>

    public interface ISCPTxSpout : ISCPPlugin
    {
        void NextTx(out long seqId, Dictionary<string, Object> parms);  
        void Ack(long seqId, Dictionary<string, Object> parms);         
        void Fail(long seqId, Dictionary<string, Object> parms);        
    }

<span data-ttu-id="35ef0-164">Precis som i sin icke-transaktionell motparterna del `NextTx()`, `Ack()`, och `Fail()` kallas i en tät loop i en enskild tråd i C\# process.</span><span class="sxs-lookup"><span data-stu-id="35ef0-164">Just like their non-transactional counter-part, `NextTx()`, `Ack()`, and `Fail()` are all called in a tight loop in a single thread in C\# process.</span></span> <span data-ttu-id="35ef0-165">När det finns inga data tooemit, är det Välkommen företagspolicy toohave `NextTx` viloläge under en kort tidsperiod (10 millisekunder) som inte toowaste för mycket CPU.</span><span class="sxs-lookup"><span data-stu-id="35ef0-165">When there are no data tooemit, it is courteous toohave `NextTx` sleep for a short amount of time (10 milliseconds) so as not toowaste too much CPU.</span></span>

<span data-ttu-id="35ef0-166">`NextTx()`kallas toostart en ny transaktion hello out-parameter `seqId` är används tooidentify hello transaktion, som också används i `Ack()` och `Fail()`.</span><span class="sxs-lookup"><span data-stu-id="35ef0-166">`NextTx()` is called toostart a new transaction, hello out parameter `seqId` is used tooidentify hello transaction, which is also used in `Ack()` and `Fail()`.</span></span> <span data-ttu-id="35ef0-167">I `NextTx()`, användaren kan generera data tooJava sida.</span><span class="sxs-lookup"><span data-stu-id="35ef0-167">In `NextTx()`, user can emit data tooJava side.</span></span> <span data-ttu-id="35ef0-168">hello data lagras i ZooKeeper toosupport repetitionsattacker.</span><span class="sxs-lookup"><span data-stu-id="35ef0-168">hello data will be stored in ZooKeeper toosupport replay.</span></span> <span data-ttu-id="35ef0-169">Eftersom hello kapacitet ZooKeeper är mycket begränsad bör endast användare genererar metadata, inte stora mängder data i transaktionella kanal.</span><span class="sxs-lookup"><span data-stu-id="35ef0-169">Because hello capacity of ZooKeeper is very limited, user should only emit metadata, not bulk data in transactional spout.</span></span>

<span data-ttu-id="35ef0-170">Storm ska spelas upp en transaktion automatiskt om den misslyckas så `Fail()` ska inte anropas i vanliga fall.</span><span class="sxs-lookup"><span data-stu-id="35ef0-170">Storm will replay a transaction automatically if it fails, so `Fail()` should not be called in normal case.</span></span> <span data-ttu-id="35ef0-171">Men om SCP kan kontrollera hello metadata som sänds av transaktionella kanal, kan det anropa `Fail()` när hello metadata är ogiltiga.</span><span class="sxs-lookup"><span data-stu-id="35ef0-171">But if SCP can check hello metadata emitted by transactional spout, it can call `Fail()` when hello metadata is invalid.</span></span>

<span data-ttu-id="35ef0-172">Hej `parms` indataparametrar i dessa funktioner är bara tom ordlista, de är reserverad för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="35ef0-172">hello `parms` input parameters in these functions are just empty Dictionary, they are reserved for future use.</span></span>

### <a name="iscpbatchbolt"></a><span data-ttu-id="35ef0-173">ISCPBatchBolt</span><span class="sxs-lookup"><span data-stu-id="35ef0-173">ISCPBatchBolt</span></span>
<span data-ttu-id="35ef0-174">ISCPBatchBolt är hello gränssnitt för transaktionell bulten.</span><span class="sxs-lookup"><span data-stu-id="35ef0-174">ISCPBatchBolt is hello interface for transactional bolt.</span></span>

    public interface ISCPBatchBolt : ISCPPlugin           
    {
        void Execute(SCPTuple tuple);
        void FinishBatch(Dictionary<string, Object> parms);  
    }

<span data-ttu-id="35ef0-175">`Execute()`anropas när det finns nya tuppel anländer till hello bulten.</span><span class="sxs-lookup"><span data-stu-id="35ef0-175">`Execute()` is called when there is new tuple arriving at hello bolt.</span></span> <span data-ttu-id="35ef0-176">`FinishBatch()`anropas när transaktionen avslutas.</span><span class="sxs-lookup"><span data-stu-id="35ef0-176">`FinishBatch()` is called when this transaction is ended.</span></span> <span data-ttu-id="35ef0-177">Hej `parms` indataparameter är reserverad för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="35ef0-177">hello `parms` input parameter is reserved for future use.</span></span>

<span data-ttu-id="35ef0-178">För transaktionell topologi, är ett viktigt begrepp – `StormTxAttempt`.</span><span class="sxs-lookup"><span data-stu-id="35ef0-178">For transactional topology, there is an important concept – `StormTxAttempt`.</span></span> <span data-ttu-id="35ef0-179">Den har två fält `TxId` och `AttemptId`.</span><span class="sxs-lookup"><span data-stu-id="35ef0-179">It has two fields, `TxId` and `AttemptId`.</span></span> <span data-ttu-id="35ef0-180">`TxId`är används tooidentify en särskild transaktion och för en given transaktion, det kan finnas flera försök om hello transaktionen misslyckas och spelas.</span><span class="sxs-lookup"><span data-stu-id="35ef0-180">`TxId` is used tooidentify a specific transaction, and for a given transaction, there may be multiple attempt if hello transaction fails and is replayed.</span></span> <span data-ttu-id="35ef0-181">SCP.NET kommer nya en annan ISCPBatchBolt objektet tooprocess varje `StormTxAttempt`, precis som vilken vill Storm Java-sida.</span><span class="sxs-lookup"><span data-stu-id="35ef0-181">SCP.NET will new a different ISCPBatchBolt object tooprocess each `StormTxAttempt`, just like what Storm do in Java side.</span></span> <span data-ttu-id="35ef0-182">hello syftet med den här designen är toosupport parallella transaktioner bearbetning.</span><span class="sxs-lookup"><span data-stu-id="35ef0-182">hello purpose of this design is toosupport parallel transactions processing.</span></span> <span data-ttu-id="35ef0-183">Användaren bör Tänk det som om transaktionen försök är klar, kommer att raderas hello motsvarande ISCPBatchBolt objekt och skräpinsamlats.</span><span class="sxs-lookup"><span data-stu-id="35ef0-183">User should keep it in mind that if transaction attempt is finished, hello corresponding ISCPBatchBolt object will be destroyed and garbage collected.</span></span>

## <a name="object-model"></a><span data-ttu-id="35ef0-184">Objektmodell</span><span class="sxs-lookup"><span data-stu-id="35ef0-184">Object Model</span></span>
<span data-ttu-id="35ef0-185">SCP.NET innehåller också en enkel uppsättning objekt nycklar för utvecklare tooprogram med.</span><span class="sxs-lookup"><span data-stu-id="35ef0-185">SCP.NET also provides a simple set of key objects for developers tooprogram with.</span></span> <span data-ttu-id="35ef0-186">De är **kontexten**, **StateStore**, och **SCPRuntime**.</span><span class="sxs-lookup"><span data-stu-id="35ef0-186">They are **Context**, **StateStore**, and **SCPRuntime**.</span></span> <span data-ttu-id="35ef0-187">De kommer att diskuteras i hello rest-delen av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="35ef0-187">They will be discussed in hello rest part of this section.</span></span>

### <a name="context"></a><span data-ttu-id="35ef0-188">Kontext</span><span class="sxs-lookup"><span data-stu-id="35ef0-188">Context</span></span>
<span data-ttu-id="35ef0-189">Kontexten innehåller ett program som körs miljö toohello.</span><span class="sxs-lookup"><span data-stu-id="35ef0-189">Context provides a running environment toohello application.</span></span> <span data-ttu-id="35ef0-190">Varje ISCPPlugin-instans (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) har en motsvarande kontext-instans.</span><span class="sxs-lookup"><span data-stu-id="35ef0-190">Each ISCPPlugin instance (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) has a corresponding Context instance.</span></span> <span data-ttu-id="35ef0-191">hello-funktionalitet som tillhandahålls av sammanhanget kan delas in i två delar: (1) hello statiska del som är tillgängliga i hello hela C\# bearbeta, (2) hello dynamiska del som endast är tillgänglig för hello specifika kontexten instans.</span><span class="sxs-lookup"><span data-stu-id="35ef0-191">hello functionality provided by Context can be divided into two parts: (1) hello static part which is available in hello whole C\# process, (2) hello dynamic part which is only available for hello specific Context instance.</span></span>

### <a name="static-part"></a><span data-ttu-id="35ef0-192">Statiska del</span><span class="sxs-lookup"><span data-stu-id="35ef0-192">Static Part</span></span>
    public static ILogger Logger = null;
    public static SCPPluginType pluginType;                      
    public static Config Config { get; set; }                    
    public static TopologyContext TopologyContext { get; set; }  

<span data-ttu-id="35ef0-193">`Logger`finns i loggen syfte.</span><span class="sxs-lookup"><span data-stu-id="35ef0-193">`Logger` is provided for log purpose.</span></span>

<span data-ttu-id="35ef0-194">`pluginType`är används tooindicate hello plugin-programmet för typ av hello C\# process.</span><span class="sxs-lookup"><span data-stu-id="35ef0-194">`pluginType` is used tooindicate hello plugin type of hello C\# process.</span></span> <span data-ttu-id="35ef0-195">Om hello C\# processen körs i lokala testläge (utan Java), hello plugin-programmet är `SCP_NET_LOCAL`.</span><span class="sxs-lookup"><span data-stu-id="35ef0-195">If hello C\# process is run in local test mode (without Java), hello plugin type is `SCP_NET_LOCAL`.</span></span>

    public enum SCPPluginType 
    {
        SCP_NET_LOCAL = 0,       
        SCP_NET_SPOUT = 1,       
        SCP_NET_BOLT = 2,        
        SCP_NET_TX_SPOUT = 3,   
        SCP_NET_BATCH_BOLT = 4  
    }

<span data-ttu-id="35ef0-196">`Config`tillhandahålls tooget konfigurationsparametrar från Java-sida.</span><span class="sxs-lookup"><span data-stu-id="35ef0-196">`Config` is provided tooget configuration parameters from Java side.</span></span> <span data-ttu-id="35ef0-197">hello parametrarna som skickas från Java sida när C\# plugin-programmet har initierats.</span><span class="sxs-lookup"><span data-stu-id="35ef0-197">hello parameters are passed from Java side when C\# plugin is initialized.</span></span> <span data-ttu-id="35ef0-198">Hej `Config` parametrar är uppdelat i två delar: `stormConf` och `pluginConf`.</span><span class="sxs-lookup"><span data-stu-id="35ef0-198">hello `Config` parameters are divided into two parts: `stormConf` and `pluginConf`.</span></span>

    public Dictionary<string, Object> stormConf { get; set; }  
    public Dictionary<string, Object> pluginConf { get; set; }  

<span data-ttu-id="35ef0-199">`stormConf`är parametrar som definierats av Storm och `pluginConf` är hello parametrar som definierats av SCP.</span><span class="sxs-lookup"><span data-stu-id="35ef0-199">`stormConf` is parameters defined by Storm and `pluginConf` is hello parameters defined by SCP.</span></span> <span data-ttu-id="35ef0-200">Exempel:</span><span class="sxs-lookup"><span data-stu-id="35ef0-200">For example:</span></span>

    public class Constants
    {
        … …

        // constant string for pluginConf
        public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

        // constant string for stormConf
        public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";           
        public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";                 
    }

<span data-ttu-id="35ef0-201">`TopologyContext`tillhandahålls tooget hello topologi kontext är den mest användbart för komponenter med flera parallellitet.</span><span class="sxs-lookup"><span data-stu-id="35ef0-201">`TopologyContext` is provided tooget hello topology context, it is most useful for components with multiple parallelism.</span></span> <span data-ttu-id="35ef0-202">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="35ef0-202">Here is an example:</span></span>

    //demo how tooget TopologyContext info
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)                      
    {
        Context.Logger.Info("TopologyContext info:");
        TopologyContext topologyContext = Context.TopologyContext;                    
        Context.Logger.Info("taskId: {0}", topologyContext.GetThisTaskId());          
        taskIndex = topologyContext.GetThisTaskIndex();
        Context.Logger.Info("taskIndex: {0}", taskIndex);
        string componentId = topologyContext.GetThisComponentId();                    
        Context.Logger.Info("componentId: {0}", componentId);
        List<int> componentTasks = topologyContext.GetComponentTasks(componentId);  
        Context.Logger.Info("taskNum: {0}", componentTasks.Count);                    
    }

### <a name="dynamic-part"></a><span data-ttu-id="35ef0-203">Dynamiska del</span><span class="sxs-lookup"><span data-stu-id="35ef0-203">Dynamic Part</span></span>
<span data-ttu-id="35ef0-204">Hej följande gränssnitt är relevanta tooa vissa kontextinstansen.</span><span class="sxs-lookup"><span data-stu-id="35ef0-204">hello following interfaces are pertinent tooa certain Context instance.</span></span> <span data-ttu-id="35ef0-205">Hej kontextinstansen skapas av SCP.NET plattform och skickades toohello användarkod:</span><span class="sxs-lookup"><span data-stu-id="35ef0-205">hello Context instance is created by SCP.NET platform and passed toohello user code:</span></span>

    // Declare hello Output and Input Stream Schemas

    public void DeclareComponentSchema(ComponentStreamSchema schema);   

    // Emit tuple toodefault stream.
    public abstract void Emit(List<object> values);                   

    // Emit tuple toohello specific stream.
    public abstract void Emit(string streamId, List<object> values);  

<span data-ttu-id="35ef0-206">För icke-transaktionell kanal som stöder ack tillhandahålls hello följande metod:</span><span class="sxs-lookup"><span data-stu-id="35ef0-206">For non-transactional spout supporting ack, hello following method is provided:</span></span>

    // for non-transactional Spout which supports ack
    public abstract void Emit(string streamId, List<object> values, long seqId);  

<span data-ttu-id="35ef0-207">För icke-transaktionell bult stöder ack, bör det explicit `Ack()` eller `Fail()` hello tuppel togs emot.</span><span class="sxs-lookup"><span data-stu-id="35ef0-207">For non-transactional bolt supporting ack, it should explicitly `Ack()` or `Fail()` hello tuple it received.</span></span> <span data-ttu-id="35ef0-208">Och när sändning nya tuppel, det måste även ange hello ankare för hello nya tuppel.</span><span class="sxs-lookup"><span data-stu-id="35ef0-208">And when emitting new tuple, it must also specify hello anchors of hello new tuple.</span></span> <span data-ttu-id="35ef0-209">hello följande metoder som tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="35ef0-209">hello following methods are provided.</span></span>

    public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values); 
    public abstract void Ack(SCPTuple tuple);
    public abstract void Fail(SCPTuple tuple);

### <a name="statestore"></a><span data-ttu-id="35ef0-210">StateStore</span><span class="sxs-lookup"><span data-stu-id="35ef0-210">StateStore</span></span>
<span data-ttu-id="35ef0-211">`StateStore`innehåller metadatatjänster, monotonisk sekvensgenerering och vänta utan samordning.</span><span class="sxs-lookup"><span data-stu-id="35ef0-211">`StateStore` provides metadata services, monotonic sequence generation, and wait-free coordination.</span></span> <span data-ttu-id="35ef0-212">Abstraktioner på högre nivåer distribuerade samtidighet kan baseras på `StateStore`, inklusive distribuerade lås, distribuerade köer, barriärer och Transaktionstjänster.</span><span class="sxs-lookup"><span data-stu-id="35ef0-212">Higher-level distributed concurrency abstractions can be built on `StateStore`, including distributed locks, distributed queues, barriers, and transaction services.</span></span>

<span data-ttu-id="35ef0-213">SCP-program kan använda hello `State` objekt toopersist information i ZooKeeper, särskilt för transaktionell topologi.</span><span class="sxs-lookup"><span data-stu-id="35ef0-213">SCP applications may use hello `State` object toopersist some information in ZooKeeper, especially for transactional topology.</span></span> <span data-ttu-id="35ef0-214">Om du gör det transaktionella kanal kraschar och startar om, kan den hämta hello nödvändig information från ZooKeeper och starta om hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="35ef0-214">Doing so, if transactional spout crashes and restart, it can retrieve hello necessary information from ZooKeeper and restart hello pipeline.</span></span>

<span data-ttu-id="35ef0-215">Hej `StateStore` -objektet har huvudsakligen dessa metoder:</span><span class="sxs-lookup"><span data-stu-id="35ef0-215">hello `StateStore` object mainly has these methods:</span></span>

    /// <summary>
    /// Static method tooretrieve a state store of hello given path and connStr 
    /// </summary>
    /// <param name="storePath">StateStore Path</param>
    /// <param name="connStr">StateStore Address</param>
    /// <returns>Instance of StateStore</returns>
    public static StateStore Get(string storePath, string connStr);

    /// <summary>
    /// Create a new state object in this state store instance
    /// </summary>
    /// <returns>State from StateStore</returns>
    public State Create();

    /// <summary>
    /// Retrieve all states that were previously uncommitted, excluding all aborted states 
    /// </summary>
    /// <returns>Uncommited States</returns>
    public IEnumerable<State> GetUnCommitted();

    /// <summary>
    /// Get all hello States in hello StateStore
    /// </summary>
    /// <returns>All hello States</returns>
    public IEnumerable<State> States();

    /// <summary>
    /// Get state or registry object
    /// </summary>
    /// <param name="info">Registry Name(Registry only)</param>
    /// <typeparam name="T">Type, Registry or State</typeparam>
    /// <returns>Return Registry or State</returns>
    public T Get<T>(string info = null);

    /// <summary>
    /// List all hello committed states
    /// </summary>
    /// <returns>Registries contain hello Committed State </returns> 
    public IEnumerable<Registry> Commited();

    /// <summary>
    /// List all hello Aborted State in hello StateStore
    /// </summary>
    /// <returns>Registries contain hello Aborted State</returns>
    public IEnumerable<Registry> Aborted();

    /// <summary>
    /// Retrieve an existing state object from this state store instance 
    /// </summary>
    /// <returns>State from StateStore</returns>
    /// <typeparam name="T">stateId, id of hello State</typeparam>
    public State GetState(long stateId)

<span data-ttu-id="35ef0-216">Hej `State` -objektet har huvudsakligen dessa metoder:</span><span class="sxs-lookup"><span data-stu-id="35ef0-216">hello `State` object mainly has these methods:</span></span>

    /// <summary>
    /// Set hello status of hello state object toocommit 
    /// </summary>
    public void Commit(bool simpleMode = true); 

    /// <summary>
    /// Set hello status of hello state object tooabort 
    /// </summary>
    public void Abort();

    /// <summary>
    /// Put an attribute value under hello give key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <param name="attribute">State Attribute</param> 
    public void PutAttribute<T>(string key, T attribute); 

    /// <summary>
    /// Get hello attribute value associated with hello given key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <returns>State Attribute</returns>               
    public T GetAttribute<T>(string key);                    

<span data-ttu-id="35ef0-217">För hello `Commit()` metod när simpleMode anges tootrue, helt enkelt bort hello motsvarande ZNode i ZooKeeper.</span><span class="sxs-lookup"><span data-stu-id="35ef0-217">For hello `Commit()` method, when simpleMode is set tootrue, it will simply delete hello corresponding ZNode in ZooKeeper.</span></span> <span data-ttu-id="35ef0-218">I annat fall raderas hello aktuella ZNode och lägga till en ny nod i hello GENOMFÖRD\_sökväg.</span><span class="sxs-lookup"><span data-stu-id="35ef0-218">Otherwise, it will delete hello current ZNode, and adding a new node in hello COMMITTED\_PATH.</span></span>

### <a name="scpruntime"></a><span data-ttu-id="35ef0-219">SCPRuntime</span><span class="sxs-lookup"><span data-stu-id="35ef0-219">SCPRuntime</span></span>
<span data-ttu-id="35ef0-220">SCPRuntime ger hello följande två metoder.</span><span class="sxs-lookup"><span data-stu-id="35ef0-220">SCPRuntime provides hello following two methods.</span></span>

    public static void Initialize();

    public static void LaunchPlugin(newSCPPlugin createDelegate);  

<span data-ttu-id="35ef0-221">`Initialize()`är används tooinitialize hello SCP-körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="35ef0-221">`Initialize()` is used tooinitialize hello SCP runtime environment.</span></span> <span data-ttu-id="35ef0-222">I den här metoden hello C\# processen ansluter toohello Java-sida, och hämtar konfigurationsparametrar och topologi-kontexten.</span><span class="sxs-lookup"><span data-stu-id="35ef0-222">In this method, hello C\# process will connect toohello Java side, and gets configuration parameters and topology context.</span></span>

<span data-ttu-id="35ef0-223">`LaunchPlugin()`använda tookick av hello-meddelande bearbetar loop.</span><span class="sxs-lookup"><span data-stu-id="35ef0-223">`LaunchPlugin()` is used tookick off hello message processing loop.</span></span> <span data-ttu-id="35ef0-224">I den här loop hello C\# plugin-program får meddelanden formuläret Java-sida (inklusive tupplar och kontroll signaler) och sedan processen hälsningsmeddelande kanske anropar hello gränssnittsmetod ge genom en användarkod hello.</span><span class="sxs-lookup"><span data-stu-id="35ef0-224">In this loop, hello C\# plugin will receive messages form Java side (including tuples and control signals), and then process hello messages, perhaps calling hello interface method provide by hello user code.</span></span> <span data-ttu-id="35ef0-225">hello Indataparametern för metoden `LaunchPlugin()` är en delegat som kan returnera ett objekt som implementerar ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="35ef0-225">hello input parameter for method `LaunchPlugin()` is a delegate that can return an object that implement ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt interface.</span></span>

    public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary\<string, Object\> parms); 

<span data-ttu-id="35ef0-226">För ISCPBatchBolt, kan vi hämta `StormTxAttempt` från `parms`, och använda den toojudge om det är ett uppspelat försök.</span><span class="sxs-lookup"><span data-stu-id="35ef0-226">For ISCPBatchBolt, we can get `StormTxAttempt` from `parms`, and use it toojudge whether it is a replayed attempt.</span></span> <span data-ttu-id="35ef0-227">Detta görs vanligtvis på hello commit bult och det visas i hello `HelloWorldTx` exempel.</span><span class="sxs-lookup"><span data-stu-id="35ef0-227">This is usually done at hello commit bolt, and it is demonstrated in hello `HelloWorldTx` example.</span></span>

<span data-ttu-id="35ef0-228">Generellt sett kan hello SCP-plugin-program köras i två lägen här:</span><span class="sxs-lookup"><span data-stu-id="35ef0-228">Generally speaking, hello SCP plugins may run in two modes here:</span></span>

1. <span data-ttu-id="35ef0-229">Lokala testläge: I det här läget hello SCP-plugin-program (hello C\# användarkod) körs i Visual Studio under hello utvecklingsfasen.</span><span class="sxs-lookup"><span data-stu-id="35ef0-229">Local Test Mode: In this mode, hello SCP plugins (hello C\# user code) run inside Visual Studio during hello development phase.</span></span> <span data-ttu-id="35ef0-230">`LocalContext`kan användas i det här läget, vilket ger metoden tooserialize hello orsakat tupplar toolocal filer och läsa dem tillbaka toomemory.</span><span class="sxs-lookup"><span data-stu-id="35ef0-230">`LocalContext` can be used in this mode, which provides method tooserialize hello emitted tuples toolocal files, and read them back toomemory.</span></span>
   
        public interface ILocalContext
        {
            List\<SCPTuple\> RecvFromMsgQueue();
            void WriteMsgQueueToFile(string filepath, bool append = false);  
            void ReadFromFileToMsgQueue(string filepath);                    
        }
2. <span data-ttu-id="35ef0-231">Standardläget: I det här läget hello SCP-plugin-program startas av storm java-process.</span><span class="sxs-lookup"><span data-stu-id="35ef0-231">Regular Mode: In this mode, hello SCP plugins are launched by storm java process.</span></span>
   
    <span data-ttu-id="35ef0-232">Här är ett exempel på Starta SCP-plugin-programmet:</span><span class="sxs-lookup"><span data-stu-id="35ef0-232">Here is an example of launching SCP plugin:</span></span>
   
        namespace Scp.App.HelloWorld
        {
        public class Generator : ISCPSpout
        {
            … …
            public static Generator Get(Context ctx, Dictionary<string, Object> parms)
            {
            return new Generator(ctx);
            }
        }
   
        class HelloWorld
        {
            static void Main(string[] args)
            {
            /* Setting hello environment variable here can change hello log file name */
            System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "HelloWorld");
   
            SCPRuntime.Initialize();
            SCPRuntime.LaunchPlugin(new newSCPPlugin(Generator.Get));
            }
        }
        }

## <a name="topology-specification-language"></a><span data-ttu-id="35ef0-233">Topologi specifikationsspråk</span><span class="sxs-lookup"><span data-stu-id="35ef0-233">Topology Specification Language</span></span>
<span data-ttu-id="35ef0-234">SCP är-topologi ett specifikt språk domän för att beskriva och konfigurera SCP topologier.</span><span class="sxs-lookup"><span data-stu-id="35ef0-234">SCP Topology Specification is a domain specific language for describing and configuring SCP topologies.</span></span> <span data-ttu-id="35ef0-235">Den är baserad på Storm's Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) och utökas med SCP.</span><span class="sxs-lookup"><span data-stu-id="35ef0-235">It is based on Storm’s Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) and is extended by SCP.</span></span>

<span data-ttu-id="35ef0-236">Topologi specifikationer som kan skickas direkt toostorm kluster för körning via hello ***runspec*** kommando.</span><span class="sxs-lookup"><span data-stu-id="35ef0-236">Topology specifications can be submitted directly toostorm cluster for execution via hello ***runspec*** command.</span></span>

<span data-ttu-id="35ef0-237">SCP.NET har Lägg till följande funktioner toodefine hello transaktionella topologi:</span><span class="sxs-lookup"><span data-stu-id="35ef0-237">SCP.NET has add follow functions toodefine hello Transactional Topology:</span></span>

| <span data-ttu-id="35ef0-238">**Nya funktioner**</span><span class="sxs-lookup"><span data-stu-id="35ef0-238">**New Functions**</span></span> | <span data-ttu-id="35ef0-239">**Parametrar**</span><span class="sxs-lookup"><span data-stu-id="35ef0-239">**Parameters**</span></span> | <span data-ttu-id="35ef0-240">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="35ef0-240">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="35ef0-241">**TX topolopy**</span><span class="sxs-lookup"><span data-stu-id="35ef0-241">**tx-topolopy**</span></span> |<span data-ttu-id="35ef0-242">topologi namn</span><span class="sxs-lookup"><span data-stu-id="35ef0-242">topology-name</span></span><br /><span data-ttu-id="35ef0-243">kanal-karta</span><span class="sxs-lookup"><span data-stu-id="35ef0-243">spout-map</span></span><br /><span data-ttu-id="35ef0-244">bult-karta</span><span class="sxs-lookup"><span data-stu-id="35ef0-244">bolt-map</span></span> |<span data-ttu-id="35ef0-245">Definiera en transaktionell topologi med hello topologi namnet &nbsp;spouts definitionen mappning och hello bultar definitionen mappning</span><span class="sxs-lookup"><span data-stu-id="35ef0-245">Define a transactional topology with hello topology name, &nbsp;spouts definition map and hello bolts definition map</span></span> |
| <span data-ttu-id="35ef0-246">**SCP-tx-kanal**</span><span class="sxs-lookup"><span data-stu-id="35ef0-246">**scp-tx-spout**</span></span> |<span data-ttu-id="35ef0-247">Exec-namn</span><span class="sxs-lookup"><span data-stu-id="35ef0-247">exec-name</span></span><br /><span data-ttu-id="35ef0-248">argument</span><span class="sxs-lookup"><span data-stu-id="35ef0-248">args</span></span><br /><span data-ttu-id="35ef0-249">Fält</span><span class="sxs-lookup"><span data-stu-id="35ef0-249">fields</span></span> |<span data-ttu-id="35ef0-250">Definiera en transaktionell kanal.</span><span class="sxs-lookup"><span data-stu-id="35ef0-250">Define a transactional spout.</span></span> <span data-ttu-id="35ef0-251">Det körs programmet hello med ***exec-name*** med ***argument***.</span><span class="sxs-lookup"><span data-stu-id="35ef0-251">It will run hello application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="35ef0-252">Hej ***fält*** är hello utdatafält för kanal</span><span class="sxs-lookup"><span data-stu-id="35ef0-252">hello ***fields*** is hello Output Fields for spout</span></span> |
| <span data-ttu-id="35ef0-253">**SCP-tx-batch-bult**</span><span class="sxs-lookup"><span data-stu-id="35ef0-253">**scp-tx-batch-bolt**</span></span> |<span data-ttu-id="35ef0-254">Exec-namn</span><span class="sxs-lookup"><span data-stu-id="35ef0-254">exec-name</span></span><br /><span data-ttu-id="35ef0-255">argument</span><span class="sxs-lookup"><span data-stu-id="35ef0-255">args</span></span><br /><span data-ttu-id="35ef0-256">Fält</span><span class="sxs-lookup"><span data-stu-id="35ef0-256">fields</span></span> |<span data-ttu-id="35ef0-257">Definiera en transaktionell Batch bulten.</span><span class="sxs-lookup"><span data-stu-id="35ef0-257">Define a transactional Batch Bolt.</span></span> <span data-ttu-id="35ef0-258">Det körs programmet hello med ***exec-name*** med ***argument.***</span><span class="sxs-lookup"><span data-stu-id="35ef0-258">It will run hello application with ***exec-name*** using ***args.***</span></span><br /><br /><span data-ttu-id="35ef0-259">hello är fält hello utdatafält för bulten.</span><span class="sxs-lookup"><span data-stu-id="35ef0-259">hello Fields is hello Output Fields for bolt.</span></span> |
| <span data-ttu-id="35ef0-260">**SCP-tx-commit-bult**</span><span class="sxs-lookup"><span data-stu-id="35ef0-260">**scp-tx-commit-bolt**</span></span> |<span data-ttu-id="35ef0-261">Exec-namn</span><span class="sxs-lookup"><span data-stu-id="35ef0-261">exec-name</span></span><br /><span data-ttu-id="35ef0-262">argument</span><span class="sxs-lookup"><span data-stu-id="35ef0-262">args</span></span><br /><span data-ttu-id="35ef0-263">Fält</span><span class="sxs-lookup"><span data-stu-id="35ef0-263">fields</span></span> |<span data-ttu-id="35ef0-264">Definiera en transaktionell Committer bulten.</span><span class="sxs-lookup"><span data-stu-id="35ef0-264">Define a transactional Committer Bolt.</span></span> <span data-ttu-id="35ef0-265">Det körs programmet hello med ***exec-name*** med ***argument***.</span><span class="sxs-lookup"><span data-stu-id="35ef0-265">It will run hello application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="35ef0-266">Hej ***fält*** är hello utdatafält för bult</span><span class="sxs-lookup"><span data-stu-id="35ef0-266">hello ***fields*** is hello Output Fields for bolt</span></span> |
| <span data-ttu-id="35ef0-267">**nontx topolopy**</span><span class="sxs-lookup"><span data-stu-id="35ef0-267">**nontx-topolopy**</span></span> |<span data-ttu-id="35ef0-268">topologi namn</span><span class="sxs-lookup"><span data-stu-id="35ef0-268">topology-name</span></span><br /><span data-ttu-id="35ef0-269">kanal-karta</span><span class="sxs-lookup"><span data-stu-id="35ef0-269">spout-map</span></span><br /><span data-ttu-id="35ef0-270">bult-karta</span><span class="sxs-lookup"><span data-stu-id="35ef0-270">bolt-map</span></span> |<span data-ttu-id="35ef0-271">Definiera en icke-transaktionell topologi med hello topologi namnet&nbsp; spouts definitionen mappning och hello bultar definitionen mappning</span><span class="sxs-lookup"><span data-stu-id="35ef0-271">Define a nontransactional topology with hello topology name,&nbsp; spouts definition map and hello bolts definition map</span></span> |
| <span data-ttu-id="35ef0-272">**SCP-kanal**</span><span class="sxs-lookup"><span data-stu-id="35ef0-272">**scp-spout**</span></span> |<span data-ttu-id="35ef0-273">Exec-namn</span><span class="sxs-lookup"><span data-stu-id="35ef0-273">exec-name</span></span><br /><span data-ttu-id="35ef0-274">argument</span><span class="sxs-lookup"><span data-stu-id="35ef0-274">args</span></span><br /><span data-ttu-id="35ef0-275">Fält</span><span class="sxs-lookup"><span data-stu-id="35ef0-275">fields</span></span><br /><span data-ttu-id="35ef0-276">parameters</span><span class="sxs-lookup"><span data-stu-id="35ef0-276">parameters</span></span> |<span data-ttu-id="35ef0-277">Definiera en icke-transaktionell kanal.</span><span class="sxs-lookup"><span data-stu-id="35ef0-277">Define a nontransactional spout.</span></span> <span data-ttu-id="35ef0-278">Det körs programmet hello med ***exec-name*** med ***argument***.</span><span class="sxs-lookup"><span data-stu-id="35ef0-278">It will run hello application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="35ef0-279">Hej ***fält*** är hello utdatafält för kanal</span><span class="sxs-lookup"><span data-stu-id="35ef0-279">hello ***fields*** is hello Output Fields for spout</span></span><br /><br /><span data-ttu-id="35ef0-280">Hej ***parametrar*** är valfritt, använder den toospecify vissa parametrar, till exempel ”nontransactional.ack.enabled”.</span><span class="sxs-lookup"><span data-stu-id="35ef0-280">hello ***parameters*** is optional, using it toospecify some parameters such as "nontransactional.ack.enabled".</span></span> |
| <span data-ttu-id="35ef0-281">**SCP-bult**</span><span class="sxs-lookup"><span data-stu-id="35ef0-281">**scp-bolt**</span></span> |<span data-ttu-id="35ef0-282">Exec-namn</span><span class="sxs-lookup"><span data-stu-id="35ef0-282">exec-name</span></span><br /><span data-ttu-id="35ef0-283">argument</span><span class="sxs-lookup"><span data-stu-id="35ef0-283">args</span></span><br /><span data-ttu-id="35ef0-284">Fält</span><span class="sxs-lookup"><span data-stu-id="35ef0-284">fields</span></span><br /><span data-ttu-id="35ef0-285">parameters</span><span class="sxs-lookup"><span data-stu-id="35ef0-285">parameters</span></span> |<span data-ttu-id="35ef0-286">Definiera en icke-transaktionell bulten.</span><span class="sxs-lookup"><span data-stu-id="35ef0-286">Define a nontransactional Bolt.</span></span> <span data-ttu-id="35ef0-287">Det körs programmet hello med ***exec-name*** med ***argument***.</span><span class="sxs-lookup"><span data-stu-id="35ef0-287">It will run hello application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="35ef0-288">Hej ***fält*** är hello utdatafält för bult</span><span class="sxs-lookup"><span data-stu-id="35ef0-288">hello ***fields*** is hello Output Fields for bolt</span></span><br /><br /><span data-ttu-id="35ef0-289">Hej ***parametrar*** är valfritt, använder den toospecify vissa parametrar, till exempel ”nontransactional.ack.enabled”.</span><span class="sxs-lookup"><span data-stu-id="35ef0-289">hello ***parameters*** is optional, using it toospecify some parameters such as "nontransactional.ack.enabled".</span></span> |

<span data-ttu-id="35ef0-290">SCP.NET har Följ nycklar ord som definierats:</span><span class="sxs-lookup"><span data-stu-id="35ef0-290">SCP.NET has follow keys words defined:</span></span>

| <span data-ttu-id="35ef0-291">**Nyckelord**</span><span class="sxs-lookup"><span data-stu-id="35ef0-291">**Key Words**</span></span> | <span data-ttu-id="35ef0-292">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="35ef0-292">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="35ef0-293">**: namn**</span><span class="sxs-lookup"><span data-stu-id="35ef0-293">**:name**</span></span> |<span data-ttu-id="35ef0-294">Definiera hello topologi namn</span><span class="sxs-lookup"><span data-stu-id="35ef0-294">Define hello Topology Name</span></span> |
| <span data-ttu-id="35ef0-295">**: topologi**</span><span class="sxs-lookup"><span data-stu-id="35ef0-295">**:topology**</span></span> |<span data-ttu-id="35ef0-296">Definiera hello topologi med hello ovan funktioner och skapa i viktiga.</span><span class="sxs-lookup"><span data-stu-id="35ef0-296">Define hello Topology using hello above functions and build in ones.</span></span> |
| <span data-ttu-id="35ef0-297">**: p**</span><span class="sxs-lookup"><span data-stu-id="35ef0-297">**:p**</span></span> |<span data-ttu-id="35ef0-298">Definiera hello parallellitet tips för varje kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="35ef0-298">Define hello parallelism hint for each spout or bolt.</span></span> |
| <span data-ttu-id="35ef0-299">**: config**</span><span class="sxs-lookup"><span data-stu-id="35ef0-299">**:config**</span></span> |<span data-ttu-id="35ef0-300">Definiera konfigurera parametern eller uppdatera hello befintliga</span><span class="sxs-lookup"><span data-stu-id="35ef0-300">Define configure parameter or update hello existing ones</span></span> |
| <span data-ttu-id="35ef0-301">**: schemat**</span><span class="sxs-lookup"><span data-stu-id="35ef0-301">**:schema**</span></span> |<span data-ttu-id="35ef0-302">Definiera hello schemat för dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="35ef0-302">Define hello Schema of Stream.</span></span> |

<span data-ttu-id="35ef0-303">Och vanliga parametrar:</span><span class="sxs-lookup"><span data-stu-id="35ef0-303">And frequently-used parameters:</span></span>

| <span data-ttu-id="35ef0-304">**Parametern**</span><span class="sxs-lookup"><span data-stu-id="35ef0-304">**Parameter**</span></span> | <span data-ttu-id="35ef0-305">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="35ef0-305">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="35ef0-306">**”plugin.name”**</span><span class="sxs-lookup"><span data-stu-id="35ef0-306">**"plugin.name"**</span></span> |<span data-ttu-id="35ef0-307">namnet på hello C#-plugin-programmet exe-filen</span><span class="sxs-lookup"><span data-stu-id="35ef0-307">exe file name of hello C# plugin</span></span> |
| <span data-ttu-id="35ef0-308">**”plugin.args”**</span><span class="sxs-lookup"><span data-stu-id="35ef0-308">**"plugin.args"**</span></span> |<span data-ttu-id="35ef0-309">plugin-argument</span><span class="sxs-lookup"><span data-stu-id="35ef0-309">plugin args</span></span> |
| <span data-ttu-id="35ef0-310">**”output.schema”**</span><span class="sxs-lookup"><span data-stu-id="35ef0-310">**"output.schema"**</span></span> |<span data-ttu-id="35ef0-311">Utdataschemat</span><span class="sxs-lookup"><span data-stu-id="35ef0-311">Output schema</span></span> |
| <span data-ttu-id="35ef0-312">**”nontransactional.ack.enabled”**</span><span class="sxs-lookup"><span data-stu-id="35ef0-312">**"nontransactional.ack.enabled"**</span></span> |<span data-ttu-id="35ef0-313">Om ack har aktiverats för icke-transaktionell topologi</span><span class="sxs-lookup"><span data-stu-id="35ef0-313">Whether ack is enabled for nontransactional topology</span></span> |

<span data-ttu-id="35ef0-314">hello runspec kommandot kommer att distribueras tillsammans med hello bits, hello användning är t.ex.:</span><span class="sxs-lookup"><span data-stu-id="35ef0-314">hello runspec command will be deployed together with hello bits, hello usage is like:</span></span>

    .\bin\runSpec.cmd
    usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
    ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target

<span data-ttu-id="35ef0-315">Hej ***resurs dir*** parametern är valfri, måste du toospecify den när du vill tooplug en C\# programmet och den här katalogen innehåller hello programmet hello beroenden och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="35ef0-315">hello ***resource-dir*** parameter is optional, you need toospecify it when you want tooplug a C\# application, and this directory will contain hello application, hello dependencies and configurations.</span></span>

<span data-ttu-id="35ef0-316">Hej ***klassökvägen*** parametern också är valfri.</span><span class="sxs-lookup"><span data-stu-id="35ef0-316">hello ***classpath*** parameter is also optional.</span></span> <span data-ttu-id="35ef0-317">Det är används toospecify hello Java klassökvägen om spec hello-filen innehåller Java-kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="35ef0-317">It is used toospecify hello Java classpath if hello spec file contains Java Spout or Bolt.</span></span>

## <a name="miscellaneous-features"></a><span data-ttu-id="35ef0-318">Diverse funktioner</span><span class="sxs-lookup"><span data-stu-id="35ef0-318">Miscellaneous Features</span></span>
### <a name="input-and-output-schema-declaration"></a><span data-ttu-id="35ef0-319">Indata och utdata Schema-deklaration</span><span class="sxs-lookup"><span data-stu-id="35ef0-319">Input and Output Schema Declaration</span></span>
<span data-ttu-id="35ef0-320">hello användaren kan generera tuppel i C\# bearbeta, hello plattform måste tooserialize hello tuppel i byte [], överföring tooJava sida och Storm överför det här tuppeln toohello mål.</span><span class="sxs-lookup"><span data-stu-id="35ef0-320">hello user can emit tuple in C\# process, hello platform needs tooserialize hello tuple into byte[], transfer tooJava side, and Storm will transfer this tuple toohello targets.</span></span> <span data-ttu-id="35ef0-321">Under tiden hello C i underordnade komponent\# processen kommer ta emot tuppel tillbaka från java sida och konvertera toohello ursprungliga typerna av plattform, dessa åtgärder är dolt enligt hello plattform.</span><span class="sxs-lookup"><span data-stu-id="35ef0-321">Meanwhile in downstream component, hello C\# process will receive tuple back from java side, and convert it toohello original types by platform, all these operations are hidden by hello Platform.</span></span>

<span data-ttu-id="35ef0-322">toosupport hello serialisering och deserialisering måste användarkod toodeclare hello schemat för hello indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="35ef0-322">toosupport hello serialization and deserialization, user code needs toodeclare hello schema of hello inputs and outputs.</span></span>

<span data-ttu-id="35ef0-323">hello i/o-dataströmmen schemat är definierad som en ordlista hello nyckeln är hello StreamId och hello är hello typer av hello kolumner.</span><span class="sxs-lookup"><span data-stu-id="35ef0-323">hello input/output stream schema is defined as a dictionary, hello key is hello StreamId and hello value is hello Types of hello columns.</span></span> <span data-ttu-id="35ef0-324">hello komponent kan ha flera strömmar som deklarerats.</span><span class="sxs-lookup"><span data-stu-id="35ef0-324">hello component can have multi-streams declared.</span></span>

    public class ComponentStreamSchema
    {
        public Dictionary<string, List<Type>> InputStreamSchema { get; set; }
        public Dictionary<string, List<Type>> OutputStreamSchema { get; set; }
        public ComponentStreamSchema(Dictionary<string, List<Type>> input, Dictionary<string, List<Type>> output)
        {
            InputStreamSchema = input;
            OutputStreamSchema = output;
        }
    }


<span data-ttu-id="35ef0-325">Vi har hello följande API lagts till i Context-objektet:</span><span class="sxs-lookup"><span data-stu-id="35ef0-325">In Context object, we have hello following API added:</span></span>

    public void DeclareComponentSchema(ComponentStreamSchema schema)

<span data-ttu-id="35ef0-326">Användarkod måste kontrollera hello tupplar som orsakat lyder under hello-schema som definierats för dataströmmen eller hello system genereras ett undantagsfel för körning.</span><span class="sxs-lookup"><span data-stu-id="35ef0-326">User code must make sure hello tuples emitted obey hello schema defined for that stream, or hello system will throw a runtime exception.</span></span>

### <a name="multi-stream-support"></a><span data-ttu-id="35ef0-327">Stöd för flera dataström</span><span class="sxs-lookup"><span data-stu-id="35ef0-327">Multi-Stream Support</span></span>
<span data-ttu-id="35ef0-328">SCP stöder användaren code tooemit eller ta emot från flera olika dataströmmar på hello samma tid.</span><span class="sxs-lookup"><span data-stu-id="35ef0-328">SCP supports user code tooemit or receive from multiple distinct streams at hello same time.</span></span> <span data-ttu-id="35ef0-329">stöd för hello återspeglar i hello Context-objektet som hello begär metoden tar en valfri dataströmmen ID-parametern.</span><span class="sxs-lookup"><span data-stu-id="35ef0-329">hello support reflects in hello Context object as hello Emit method takes an optional stream ID parameter.</span></span>

<span data-ttu-id="35ef0-330">Två metoder i hello SCP.NET Context-objektet har lagts till.</span><span class="sxs-lookup"><span data-stu-id="35ef0-330">Two methods in hello SCP.NET Context object have been added.</span></span> <span data-ttu-id="35ef0-331">De är används tooemit tuppeln eller Tupplar toospecify StreamId.</span><span class="sxs-lookup"><span data-stu-id="35ef0-331">They are used tooemit Tuple or Tuples toospecify StreamId.</span></span> <span data-ttu-id="35ef0-332">Hej StreamId är en sträng och den måste toobe konsekvent i båda C\# och hello topologi Definition-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="35ef0-332">hello StreamId is a string and it needs toobe consistent in both C\# and hello Topology Definition Spec.</span></span>

        /* Emit tuple toohello specific stream. */
        public abstract void Emit(string streamId, List<object> values);

        /* for non-transactional Spout only */
        public abstract void Emit(string streamId, List<object> values, long seqId);

<span data-ttu-id="35ef0-333">hello ljusavgivande tooa obefintligt dataströmmen kommer runtime-undantag.</span><span class="sxs-lookup"><span data-stu-id="35ef0-333">hello emitting tooa non-existing stream will cause runtime exceptions.</span></span>

### <a name="fields-grouping"></a><span data-ttu-id="35ef0-334">Fält gruppering</span><span class="sxs-lookup"><span data-stu-id="35ef0-334">Fields Grouping</span></span>
<span data-ttu-id="35ef0-335">hello fungerar inbyggda fält gruppering i Strom inte korrekt i SCP.NET.</span><span class="sxs-lookup"><span data-stu-id="35ef0-335">hello build-in Fields Grouping in Strom is not working properly in SCP.NET.</span></span> <span data-ttu-id="35ef0-336">Hello fält gruppering används hello byte [] objekt hash-kod tooperform hello gruppering i hello Java Proxy sida, alla datatyper i hello-fält är faktiskt byte [].</span><span class="sxs-lookup"><span data-stu-id="35ef0-336">On hello Java Proxy side, all hello fields data types are actually byte[], and hello fields grouping uses hello byte[] object hash code tooperform hello grouping.</span></span> <span data-ttu-id="35ef0-337">hello byte [] hash-kod är hello-adressen för det här objektet i minnet.</span><span class="sxs-lookup"><span data-stu-id="35ef0-337">hello byte[] object hash code is hello address of this object in memory.</span></span> <span data-ttu-id="35ef0-338">Så hello gruppering är fel för två byte objekt [] hello att dela samma innehåll men inte hello samma adress.</span><span class="sxs-lookup"><span data-stu-id="35ef0-338">So hello grouping will be wrong for two byte[] objects that share hello same content but not hello same address.</span></span>

<span data-ttu-id="35ef0-339">SCP.NET lägger till en metod för anpassad gruppering och använder hello innehållet i hello byte [] toodo hello gruppering.</span><span class="sxs-lookup"><span data-stu-id="35ef0-339">SCP.NET adds a customized grouping method, and it will use hello content of hello byte[] toodo hello grouping.</span></span> <span data-ttu-id="35ef0-340">I **SPEC** filen hello syntax är t.ex.:</span><span class="sxs-lookup"><span data-stu-id="35ef0-340">In **SPEC** file, hello syntax is like:</span></span>

    (bolt-spec
        {
            "spout_test" (scp-field-group :non-tx [0,1])
        }
        …
    )


<span data-ttu-id="35ef0-341">Här</span><span class="sxs-lookup"><span data-stu-id="35ef0-341">Here,</span></span>

1. <span data-ttu-id="35ef0-342">”scp-fältet-grupp”: ”anpassad fältet gruppering implementeras av SCP”.</span><span class="sxs-lookup"><span data-stu-id="35ef0-342">"scp-field-group" means "Customized field grouping implemented by SCP".</span></span>
2. <span data-ttu-id="35ef0-343">”: tx” eller ”: icke-tx” innebär att om det är transaktionell topologi.</span><span class="sxs-lookup"><span data-stu-id="35ef0-343">":tx" or ":non-tx" means if it’s transactional topology.</span></span> <span data-ttu-id="35ef0-344">Vi behöver informationen eftersom hello startar index skiljer sig åt i tx kontra-tx topologier.</span><span class="sxs-lookup"><span data-stu-id="35ef0-344">We need this information since hello starting index is different in tx vs. non-tx topologies.</span></span>
3. <span data-ttu-id="35ef0-345">[0,1] innebär en hashset av fältet ID, början på 0.</span><span class="sxs-lookup"><span data-stu-id="35ef0-345">[0,1] means a hashset of field Ids, starting from 0.</span></span>

### <a name="hybrid-topology"></a><span data-ttu-id="35ef0-346">Hybrid-topologi</span><span class="sxs-lookup"><span data-stu-id="35ef0-346">Hybrid topology</span></span>
<span data-ttu-id="35ef0-347">hello intern Storm är skriven i Java.</span><span class="sxs-lookup"><span data-stu-id="35ef0-347">hello native Storm is written in Java.</span></span> <span data-ttu-id="35ef0-348">Och SCP.Net har förbättrad den tooenable våra tull toowrite C\# code toohandle sina affärslogik.</span><span class="sxs-lookup"><span data-stu-id="35ef0-348">And SCP.Net has enhanced it tooenable our customs toowrite C\# code toohandle their business logic.</span></span> <span data-ttu-id="35ef0-349">Men vi stöder också hybridtopologier, som innehåller inte bara C\# kanaler/bultar men Java kanal/bultar.</span><span class="sxs-lookup"><span data-stu-id="35ef0-349">But we also support hybrid topologies, which contains not only C\# spouts/bolts, but also Java Spout/Bolts.</span></span>

### <a name="specify-java-spoutbolt-in-spec-file"></a><span data-ttu-id="35ef0-350">Ange kanal/bult för Java i spec fil</span><span class="sxs-lookup"><span data-stu-id="35ef0-350">Specify Java Spout/Bolt in spec file</span></span>
<span data-ttu-id="35ef0-351">”Scp-kanal” och ”scp-bult” kan också vara används toospecify Java Spouts och bultar i spec fil, här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="35ef0-351">In spec file, "scp-spout" and "scp-bolt" can also be used toospecify Java Spouts and Bolts, here is an example:</span></span>

    (spout-spec 
      (microsoft.scp.example.HybridTopology.Generator.)           
      :p 1)

<span data-ttu-id="35ef0-352">Här `microsoft.scp.example.HybridTopology.Generator` är hello namnet på hello prata Java-klass.</span><span class="sxs-lookup"><span data-stu-id="35ef0-352">Here `microsoft.scp.example.HybridTopology.Generator` is hello name of hello Java Spout class.</span></span>

### <a name="specify-java-classpath-in-runspec-command"></a><span data-ttu-id="35ef0-353">Ange Java-klassökvägen i runSpec kommando</span><span class="sxs-lookup"><span data-stu-id="35ef0-353">Specify Java Classpath in runSpec Command</span></span>
<span data-ttu-id="35ef0-354">Om du vill toosubmit topologi som innehåller Java Spouts eller bultar behöver toofirst kompilera hello Java Spouts eller bultar och hämta hello Jar-filer.</span><span class="sxs-lookup"><span data-stu-id="35ef0-354">If you want toosubmit topology containing Java Spouts or Bolts, you need toofirst compile hello Java Spouts or Bolts and get hello Jar files.</span></span> <span data-ttu-id="35ef0-355">Du bör ange hello java-klassökvägen som innehåller hello Jar-filer när du skickar in topologin.</span><span class="sxs-lookup"><span data-stu-id="35ef0-355">Then you should specify hello java classpath that contains hello Jar files when submitting topology.</span></span> <span data-ttu-id="35ef0-356">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="35ef0-356">Here is an example:</span></span>

    bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*

<span data-ttu-id="35ef0-357">Här **exempel\\HybridTopology\\java\\mål\\**  är hello mappen som innehåller hello Java kanal/bult Jar-fil.</span><span class="sxs-lookup"><span data-stu-id="35ef0-357">Here **examples\\HybridTopology\\java\\target\\** is hello folder containing hello Java Spout/Bolt Jar file.</span></span>

### <a name="serialization-and-deserialization-between-java-and-c"></a><span data-ttu-id="35ef0-358">Serialisering och deserialisering mellan Java och C\\</span><span class="sxs-lookup"><span data-stu-id="35ef0-358">Serialization and Deserialization between Java and C\\</span></span>
<span data-ttu-id="35ef0-359">Vår SCP-komponenten innehåller Java sida och C\# sida.</span><span class="sxs-lookup"><span data-stu-id="35ef0-359">Our SCP component includes Java side and C\# side.</span></span> <span data-ttu-id="35ef0-360">I ordning toointeract med inbyggda Java kanaler/bultar serialisering/deserialisering utföras mellan Java sida och C\# sida, enligt beskrivningen i följande diagram hello.</span><span class="sxs-lookup"><span data-stu-id="35ef0-360">In order toointeract with native Java Spouts/Bolts, Serialization/Deserialization must be carried out between Java side and C\# side, as illustrated in hello following graph.</span></span>

![diagram över java-komponent som skickar tooSCP komponenten skickar tooJava komponent](media/hdinsight-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

1. <span data-ttu-id="35ef0-362">**Serialisering i Java-sida och deserialisering i C\# sida**</span><span class="sxs-lookup"><span data-stu-id="35ef0-362">**Serialization in Java side and Deserialization in C\# side**</span></span>
   
   <span data-ttu-id="35ef0-363">Första vi tillhandahåller standardimplementering för serialisering i Java-sida och deserialisering i C\# sida.</span><span class="sxs-lookup"><span data-stu-id="35ef0-363">First we provide default implementation for serialization in Java side and deserialization in C\# side.</span></span> <span data-ttu-id="35ef0-364">Hej serialiseringsmetod i Java-sida kan anges i SPEC filen:</span><span class="sxs-lookup"><span data-stu-id="35ef0-364">hello serialization method in Java side can be specified in SPEC file:</span></span>
   
       (scp-bolt
           {
               "plugin.name" "HybridTopology.exe"
               "plugin.args" ["displayer"]
               "output.schema" {}
               "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
           })
   
   <span data-ttu-id="35ef0-365">Hej deserialisering metod i C\# sida måste anges i C\# användarkod:</span><span class="sxs-lookup"><span data-stu-id="35ef0-365">hello deserialization method in C\# side should be specified in C\# user code:</span></span>
   
       Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
       inputSchema.Add("default", new List<Type>() { typeof(Person) });
       this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
       this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());            
   
   <span data-ttu-id="35ef0-366">Den här standardimplementering ska hantera de flesta fall om hello datatypen inte är för komplex.</span><span class="sxs-lookup"><span data-stu-id="35ef0-366">This default implementation should handle most cases if hello data type is not too complex.</span></span> <span data-ttu-id="35ef0-367">I vissa fall, antingen eftersom hello användardatatypen är för komplex eller för att hello prestanda för våra standardimplementering inte uppfyller hello användarens kravet användaren kan plugin-programmet sina egna implementering.</span><span class="sxs-lookup"><span data-stu-id="35ef0-367">For certain cases, either because hello user data type is too complex, or because hello performance of our default implementation does not meet hello user's requirement, user can plug-in their own implementation.</span></span>
   
   <span data-ttu-id="35ef0-368">hello serialisera gränssnittet i java sida definieras som:</span><span class="sxs-lookup"><span data-stu-id="35ef0-368">hello serialize interface in java side is defined as:</span></span>
   
       public interface ICustomizedInteropJavaSerializer {
           public void prepare(String[] args);
           public List<ByteBuffer> serialize(List<Object> objectList);
       }
   
   <span data-ttu-id="35ef0-369">hello deserialisera gränssnitt i C\# sida definieras som:</span><span class="sxs-lookup"><span data-stu-id="35ef0-369">hello deserialize interface in C\# side is defined as:</span></span>
   
   <span data-ttu-id="35ef0-370">offentliga gränssnittet ICustomizedInteropCSharpDeserializer</span><span class="sxs-lookup"><span data-stu-id="35ef0-370">public interface ICustomizedInteropCSharpDeserializer</span></span>
   
       public interface ICustomizedInteropCSharpDeserializer
       {
           List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
       }
2. <span data-ttu-id="35ef0-371">**Serialisering i C\# sida och deserialisering i Java sida sida**</span><span class="sxs-lookup"><span data-stu-id="35ef0-371">**Serialization in C\# side and Deserialization in Java side side**</span></span>
   
   <span data-ttu-id="35ef0-372">Hej serialiseringsmetod i C\# sida måste anges i C\# användarkod:</span><span class="sxs-lookup"><span data-stu-id="35ef0-372">hello serialization method in C\# side should be specified in C\# user code:</span></span>
   
       this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
   
   <span data-ttu-id="35ef0-373">hello deserialisering metod i Java-sida måste anges i SPEC fil:</span><span class="sxs-lookup"><span data-stu-id="35ef0-373">hello Deserialization method in Java side should be specified in SPEC file:</span></span>
   
     <span data-ttu-id="35ef0-374">(scp-kanal</span><span class="sxs-lookup"><span data-stu-id="35ef0-374">(scp-spout</span></span>
   
       {
         "plugin.name" "HybridTopology.exe"
         "plugin.args" ["generator"]
         "output.schema" {"default" ["person"]}
         "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
       })
   
   <span data-ttu-id="35ef0-375">Är här ”microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer” hello namnet på funktionen för avserialisering och ”microsoft.scp.example.HybridTopology.Person” är hello måldata klassen hello avserialiseras till.</span><span class="sxs-lookup"><span data-stu-id="35ef0-375">Here "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" is hello name of Deserializer, and "microsoft.scp.example.HybridTopology.Person" is hello target class hello data is deserialized to.</span></span>
   
   <span data-ttu-id="35ef0-376">Användaren kan också plugin-programmet sina egna implementering av C\# serialiseraren och Java funktionen för avserialisering.</span><span class="sxs-lookup"><span data-stu-id="35ef0-376">User can also plug-in their own implementation of C\# serializer and Java Deserializer.</span></span> <span data-ttu-id="35ef0-377">Detta är hello gränssnitt för C\# serialiseraren:</span><span class="sxs-lookup"><span data-stu-id="35ef0-377">This is hello interface for C\# serializer:</span></span>
   
       public interface ICustomizedInteropCSharpSerializer
       {
           List<byte[]> Serialize(List<object> dataList);
       }
   
   <span data-ttu-id="35ef0-378">Detta är hello gränssnitt för Java funktionen för avserialisering:</span><span class="sxs-lookup"><span data-stu-id="35ef0-378">This is hello interface for Java Deserializer:</span></span>
   
       public interface ICustomizedInteropJavaDeserializer {
           public void prepare(String[] targetClassNames);
           public List<Object> Deserialize(List<ByteBuffer> dataList);
       }

## <a name="scp-host-mode"></a><span data-ttu-id="35ef0-379">Värden för SCP-läge</span><span class="sxs-lookup"><span data-stu-id="35ef0-379">SCP Host Mode</span></span>
<span data-ttu-id="35ef0-380">I det här läget kan användaren kompilera sina koder tooDLL och använda SCPHost.exe som tillhandahålls av SCP toosubmit topologi.</span><span class="sxs-lookup"><span data-stu-id="35ef0-380">In this mode, user can compile their codes tooDLL, and use SCPHost.exe provided by SCP toosubmit topology.</span></span> <span data-ttu-id="35ef0-381">hello spec filen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="35ef0-381">hello spec file looks like this:</span></span>

    (scp-spout
      {
        "plugin.name" "SCPHost.exe"
        "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
        "output.schema" {"default" ["sentence"]}
      })

<span data-ttu-id="35ef0-382">Här, `plugin.name` har angetts som `SCPHost.exe` som tillhandahålls av SCP SDK.</span><span class="sxs-lookup"><span data-stu-id="35ef0-382">Here, `plugin.name` is specified as `SCPHost.exe` provided by SCP SDK.</span></span> <span data-ttu-id="35ef0-383">SCPHost.exe som accepterar exakt tre parametrar:</span><span class="sxs-lookup"><span data-stu-id="35ef0-383">SCPHost.exe which accepts exactly three parameters:</span></span>

1. <span data-ttu-id="35ef0-384">hello först en är hello DLL-namn som är `"HelloWorld.dll"` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="35ef0-384">hello first one is hello DLL name, which is `"HelloWorld.dll"` in this example.</span></span>
2. <span data-ttu-id="35ef0-385">hello andra är hello klassnamnet, som är `"Scp.App.HelloWorld.Generator"` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="35ef0-385">hello second one is hello Class name, which is `"Scp.App.HelloWorld.Generator"` in this example.</span></span>
3. <span data-ttu-id="35ef0-386">hello tredje är en hello namnet på en offentlig statisk metod som kan vara anropade tooget en instans av ISCPPlugin.</span><span class="sxs-lookup"><span data-stu-id="35ef0-386">hello third one is hello name of a public static method, which can be invoked tooget an instance of ISCPPlugin.</span></span>

<span data-ttu-id="35ef0-387">Användarkod i värden läge är kompilerad som DLL-filen och anropas av SCP-plattformen.</span><span class="sxs-lookup"><span data-stu-id="35ef0-387">In host mode, user code is compiled as DLL, and is invoked by SCP platform.</span></span> <span data-ttu-id="35ef0-388">SCP-plattformen kan så få fullständig kontroll över hello hela standardbearbetningslogiken.</span><span class="sxs-lookup"><span data-stu-id="35ef0-388">So SCP platform can get full control of hello whole processing logic.</span></span> <span data-ttu-id="35ef0-389">Så vi rekommenderar våra kunder toosubmit topologi i SCP värden läge eftersom den kan förenkla hello utveckling och sätta oss större flexibilitet och bättre bakåtkompatibilitet för samt senare version.</span><span class="sxs-lookup"><span data-stu-id="35ef0-389">So we recommend our customers toosubmit topology in SCP host mode since it can simplify hello development experience and bring us more flexibility and better backward compatibility for later release as well.</span></span>

## <a name="scp-programming-examples"></a><span data-ttu-id="35ef0-390">Exempel för SCP-programmering</span><span class="sxs-lookup"><span data-stu-id="35ef0-390">SCP Programming Examples</span></span>
### <a name="helloworld"></a><span data-ttu-id="35ef0-391">HelloWorld</span><span class="sxs-lookup"><span data-stu-id="35ef0-391">HelloWorld</span></span>
<span data-ttu-id="35ef0-392">**HelloWorld** är ett väldigt enkelt exempel tooshow uppleva SCP.Net.</span><span class="sxs-lookup"><span data-stu-id="35ef0-392">**HelloWorld** is a very simple example tooshow a taste of SCP.Net.</span></span> <span data-ttu-id="35ef0-393">Den använder en icke-transaktionell topologi med en kanal som kallas **generator**, och två bultar kallas **delningslisten** och **räknaren**.</span><span class="sxs-lookup"><span data-stu-id="35ef0-393">It uses a non-transactional topology, with a spout called **generator**, and two bolts called **splitter** and **counter**.</span></span> <span data-ttu-id="35ef0-394">hello kanal **generator** att slumpmässigt generera vissa meningar och generera dessa meningar för**delningslisten**.</span><span class="sxs-lookup"><span data-stu-id="35ef0-394">hello spout **generator** will randomly generate some sentences, and emit these sentences too**splitter**.</span></span> <span data-ttu-id="35ef0-395">hello bult **delningslisten** ska dela hello meningar toowords och generera dessa ord för**räknaren** bulten.</span><span class="sxs-lookup"><span data-stu-id="35ef0-395">hello bolt **splitter** will split hello sentences toowords and emit these words too**counter** bolt.</span></span> <span data-ttu-id="35ef0-396">räknaren ”hello bult” används en ordlista toorecord hello förekomstantal varje ord.</span><span class="sxs-lookup"><span data-stu-id="35ef0-396">hello bolt "counter" uses a dictionary toorecord hello occurrence number of each word.</span></span>

<span data-ttu-id="35ef0-397">Det finns två spec filer **HelloWorld.spec** och **HelloWorld\_EnableAck.spec** för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="35ef0-397">There are two spec files, **HelloWorld.spec** and **HelloWorld\_EnableAck.spec** for this example.</span></span> <span data-ttu-id="35ef0-398">I hello C\# kod, det kan ta reda på om ack aktiveras genom att hämta hello pluginConf från Java-sida.</span><span class="sxs-lookup"><span data-stu-id="35ef0-398">In hello C\# code, it can find out whether ack is enabled by getting hello pluginConf from Java side.</span></span>

    /* demo how tooget pluginConf info */
    if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
    {
        enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
    }
    Context.Logger.Info("enableAck: {0}", enableAck);

<span data-ttu-id="35ef0-399">I hello kanal är en ordlista används toocache hello tupplar som inte är ADE om ack är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="35ef0-399">In hello spout, if ack is enabled, a dictionary is used toocache hello tuples that have not been acked.</span></span> <span data-ttu-id="35ef0-400">Om Fail() anropas ska hello misslyckade tuppel återupprepas:</span><span class="sxs-lookup"><span data-stu-id="35ef0-400">If Fail() is called, hello failed tuple will be replayed:</span></span>

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        Context.Logger.Info("Fail, seqId: {0}", seqId);
        if (cachedTuples.ContainsKey(seqId))
        {
            /* get hello cached tuple */
            string sentence = cachedTuples[seqId];

            /* replay hello failed tuple */
            Context.Logger.Info("Re-Emit: {0}, seqId: {1}", sentence, seqId);
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), seqId);
        }
        else
        {
            Context.Logger.Warn("Fail(), can't find cached tuple for seqId {0}!", seqId);
        }
    }

### <a name="helloworldtx"></a><span data-ttu-id="35ef0-401">HelloWorldTx</span><span class="sxs-lookup"><span data-stu-id="35ef0-401">HelloWorldTx</span></span>
<span data-ttu-id="35ef0-402">Hej **HelloWorldTx** exemplet visar hur tooimplement transaktionella topologi.</span><span class="sxs-lookup"><span data-stu-id="35ef0-402">hello **HelloWorldTx** example demonstrates how tooimplement transactional topology.</span></span> <span data-ttu-id="35ef0-403">Den har en kanal som kallas **generator**en batch bultar kallas **partiell antal**, och en bult commit anropas **summan antal**.</span><span class="sxs-lookup"><span data-stu-id="35ef0-403">It have one spout called **generator**, a batch bolts called **partial-count**, and a commit bolt called **count-sum**.</span></span> <span data-ttu-id="35ef0-404">Det finns tre förskapade txt-filer: **DataSource0.txt**, **DataSource1.txt** och **DataSource2.txt**.</span><span class="sxs-lookup"><span data-stu-id="35ef0-404">There are also three pre-created txt files: **DataSource0.txt**, **DataSource1.txt** and **DataSource2.txt**.</span></span>

<span data-ttu-id="35ef0-405">I varje transaktion, hello kanal **generator** slumpmässigt väljer två filer från hello förskapade tre filer, och genererar hello två filen namn toohello **partiell antal** bulten.</span><span class="sxs-lookup"><span data-stu-id="35ef0-405">In each transaction, hello spout **generator** will randomly choose two files from hello pre-created three files, and emit hello two file names toohello **partial-count** bolt.</span></span> <span data-ttu-id="35ef0-406">hello bult **partiell antal** först hämta hello filnamnet från hello emot tuppel och sedan öppna hello fil- och antal hello antalet ord i den här filen och slutligen genererar hello word nummer toohello **count-summan**bulten.</span><span class="sxs-lookup"><span data-stu-id="35ef0-406">hello bolt **partial-count** will first get hello file name from hello received tuple, then open hello file and count hello number of words in this file, and finally emit hello word number toohello **count-sum** bolt.</span></span> <span data-ttu-id="35ef0-407">Hej **antal summan** bult sammanfattas hello totala antalet.</span><span class="sxs-lookup"><span data-stu-id="35ef0-407">hello **count-sum** bolt will summarize hello total count.</span></span>

<span data-ttu-id="35ef0-408">tooachieve **exakt en gång** semantik, hello commit bult **summan antal** behöver toojudge oavsett om det är en upprepat transaktion.</span><span class="sxs-lookup"><span data-stu-id="35ef0-408">tooachieve **exactly once** semantics, hello commit bolt **count-sum** need toojudge whether it is a replayed transaction.</span></span> <span data-ttu-id="35ef0-409">I det här exemplet har den en statisk medlemsvariabel:</span><span class="sxs-lookup"><span data-stu-id="35ef0-409">In this example, it has a static member variable:</span></span>

    public static long lastCommittedTxId = -1; 

<span data-ttu-id="35ef0-410">När en instans av ISCPBatchBolt skapas får hello `txAttempt` från indataparametrar:</span><span class="sxs-lookup"><span data-stu-id="35ef0-410">When an ISCPBatchBolt instance is created, it will get hello `txAttempt` from input parameters:</span></span>

    public static CountSum Get(Context ctx, Dictionary<string, Object> parms)
    {
        /* for transactional topology, we can get txAttempt from hello input parms */
        if (parms.ContainsKey(Constants.STORM_TX_ATTEMPT))
        {
            StormTxAttempt txAttempt = (StormTxAttempt)parms[Constants.STORM_TX_ATTEMPT];
            return new CountSum(ctx, txAttempt);
        }
        else
        {
            throw new Exception("null txAttempt");
        }
    }

<span data-ttu-id="35ef0-411">När `FinishBatch()` anropas, hello `lastCommittedTxId` kommer att uppdateringen om den inte är en upprepat transaktion.</span><span class="sxs-lookup"><span data-stu-id="35ef0-411">When `FinishBatch()` is called, hello `lastCommittedTxId` will be update if it is not a replayed transaction.</span></span>

    public void FinishBatch(Dictionary<string, Object> parms)
    {
        /* judge whether it is a replayed transaction? */
        bool replay = (this.txAttempt.TxId <= lastCommittedTxId);

        if (!replay)
        {
            /* If it is not replayed, update hello toalCount and lastCommittedTxId vaule */
            totalCount = totalCount + this.count;
            lastCommittedTxId = this.txAttempt.TxId;
        }
        … …
    }


### <a name="hybridtopology"></a><span data-ttu-id="35ef0-412">HybridTopology</span><span class="sxs-lookup"><span data-stu-id="35ef0-412">HybridTopology</span></span>
<span data-ttu-id="35ef0-413">Den här topologin innehåller en Java-kanalen och en C\# bulten.</span><span class="sxs-lookup"><span data-stu-id="35ef0-413">This topology contains a Java Spout and a C\# Bolt.</span></span> <span data-ttu-id="35ef0-414">Den använder hello serialisering och deserialisering standardimplementering tillhandahålls av SCP-plattformen.</span><span class="sxs-lookup"><span data-stu-id="35ef0-414">It uses hello default serialization and deserialization implementation provided by SCP platform.</span></span> <span data-ttu-id="35ef0-415">Ange ref hello **HybridTopology.spec** i **exempel\\HybridTopology** mapp för hello spec filinformation, och **SubmitTopology.bat** för toospecify Java-klassökvägen.</span><span class="sxs-lookup"><span data-stu-id="35ef0-415">Please ref hello **HybridTopology.spec** in **examples\\HybridTopology** folder for hello spec file details, and **SubmitTopology.bat** for how toospecify Java classpath.</span></span>

### <a name="scphostdemo"></a><span data-ttu-id="35ef0-416">SCPHostDemo</span><span class="sxs-lookup"><span data-stu-id="35ef0-416">SCPHostDemo</span></span>
<span data-ttu-id="35ef0-417">Det här exemplet är i princip hello samma som HelloWorld.</span><span class="sxs-lookup"><span data-stu-id="35ef0-417">This example is hello same as HelloWorld in essence.</span></span> <span data-ttu-id="35ef0-418">hello endast skillnaden är att hello användarkod har kompilerats som DLL-filen och hello topologi skickas med hjälp av SCPHost.exe.</span><span class="sxs-lookup"><span data-stu-id="35ef0-418">hello only difference is that hello user code is compiled as DLL and hello topology is submitted by using SCPHost.exe.</span></span> <span data-ttu-id="35ef0-419">Ta ref hello avsnittet ”SCP värden läget” mer detaljerad förklaring.</span><span class="sxs-lookup"><span data-stu-id="35ef0-419">Please ref hello section "SCP Host Mode" for more detailed explanation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="35ef0-420">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="35ef0-420">Next Steps</span></span>
<span data-ttu-id="35ef0-421">Exempel på Storm-topologier som skapats med SCP finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="35ef0-421">For examples of Storm topologies created using SCP, see hello following:</span></span>

* [<span data-ttu-id="35ef0-422">Utveckla C#-topologier för Apache Storm på HDInsight med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35ef0-422">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="35ef0-423">Bearbeta händelser från Azure Event Hubs med Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="35ef0-423">Process events from Azure Event Hubs with Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-event-hub-topology.md)
* [<span data-ttu-id="35ef0-424">Skapa flera dataströmmar i en C# Storm-topologi</span><span class="sxs-lookup"><span data-stu-id="35ef0-424">Create multiple data streams in a C# Storm topology</span></span>](hdinsight-storm-twitter-trending.md)
* [<span data-ttu-id="35ef0-425">Använd Power Bi toovisualize data från en Storm-topologi</span><span class="sxs-lookup"><span data-stu-id="35ef0-425">Use Power Bi toovisualize data from a Storm topology</span></span>](hdinsight-storm-power-bi-topology.md)
* [<span data-ttu-id="35ef0-426">Bearbeta vehicle sensordata från Händelsehubbar med Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="35ef0-426">Process vehicle sensor data from Event Hubs using Storm on HDInsight</span></span>](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [<span data-ttu-id="35ef0-427">Extrahering, transformering och inläsning (ETL) från Azure Event Hubs tooHBase</span><span class="sxs-lookup"><span data-stu-id="35ef0-427">Extract, Transform, and Load (ETL) from Azure Event Hubs tooHBase</span></span>](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
* [<span data-ttu-id="35ef0-428">Samordna händelser med Storm och HBase på HDInsight</span><span class="sxs-lookup"><span data-stu-id="35ef0-428">Correlate events using Storm and HBase on HDInsight</span></span>](hdinsight-storm-correlation-topology.md)

