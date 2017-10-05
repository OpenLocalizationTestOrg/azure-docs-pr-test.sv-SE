---
title: "Programmeringsguide för SCP.NET | Microsoft Docs"
description: "Lär dig hur du skapar med hjälp av SCP.NET. NET-baserade Storm-topologier för att använda med Storm på HDInsight."
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
ms.openlocfilehash: 3d76aebd2a1fd729c8e0639e6afcbde4c3fb752b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scp-programming-guide"></a><span data-ttu-id="0d5a1-103">Programmeringsguide för SCP</span><span class="sxs-lookup"><span data-stu-id="0d5a1-103">SCP programming guide</span></span>
<span data-ttu-id="0d5a1-104">SCP är en plattform för att skapa realtid, tillförlitliga och konsekventa hög prestanda databearbetning program.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-104">SCP is a platform to build real time, reliable, consistent and high performance data processing application.</span></span> <span data-ttu-id="0d5a1-105">Det är byggt ovanpå [Apache Storm](http://storm.incubator.apache.org/) – en dataström som bearbetar system utformas av OSS communities.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-105">It is built on top of [Apache Storm](http://storm.incubator.apache.org/) -- a stream processing system designed by the OSS communities.</span></span> <span data-ttu-id="0d5a1-106">Storm är utformad med Nathan Marz och öppen källkod med Twitter.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-106">Storm is designed by Nathan Marz and open sourced by Twitter.</span></span> <span data-ttu-id="0d5a1-107">Den använder [Apache ZooKeeper](http://zookeeper.apache.org/), ett annat Apache-projekt att aktivera hög tillförlitliga distribuerade samordning och tillstånd.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-107">It leverages [Apache ZooKeeper](http://zookeeper.apache.org/), another Apache project to enable highly reliable distributed coordination and state management.</span></span> 

<span data-ttu-id="0d5a1-108">Inte bara SCP-projektet portar Storm på Windows, utan också projektet till tillägg och anpassning av Windows-ekosystemet.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-108">Not only the SCP project ported Storm on Windows but also the project added extensions and customization for the Windows ecosystem.</span></span> <span data-ttu-id="0d5a1-109">Tilläggen innehåller .NET-utvecklare upplevelse och bibliotek, anpassning av innehåller Windows-baserad distribution.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-109">The extensions include .NET developer experience, and libraries, the customization includes Windows-based deployment.</span></span> 

<span data-ttu-id="0d5a1-110">Tillägget och anpassning görs så att vi inte behöver duplicera projekt för OSS och vi kan utnyttja härledda ekosystem byggda på Storm.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-110">The extension and customization is done in such a way that we do not need to fork the OSS projects and we could leverage derived ecosystems built on top of Storm.</span></span>

## <a name="processing-model"></a><span data-ttu-id="0d5a1-111">Processmodell</span><span class="sxs-lookup"><span data-stu-id="0d5a1-111">Processing model</span></span>
<span data-ttu-id="0d5a1-112">Data i SCP modelleras som kontinuerliga strömmar av tupplar.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-112">The data in SCP is modeled as continuous streams of tuples.</span></span> <span data-ttu-id="0d5a1-113">Vanligtvis trafikflödet tuppeln i vissa kö först, sedan tas upp och transformeras av affärslogik som finns inuti en Storm-topologi slutligen utdata kan skickas som tupplar till ett annat SCP-system eller allokeras till butiker som distribuerat filsystem eller databaser som SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-113">Typically the tuples flow into some queue first, then picked up, and transformed by business logic hosted inside a Storm topology, finally the output could be piped as tuples to another SCP system, or be committed to stores like distributed file system or databases like SQL Server.</span></span>

![Ett diagram av en kö mata data för bearbetning, som ett datalager-flöden](media/hdinsight-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

<span data-ttu-id="0d5a1-115">I Storm definierar en Programtopologi ett diagram över beräkning.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-115">In Storm, an application topology defines a graph of computation.</span></span> <span data-ttu-id="0d5a1-116">Varje nod i en topologi innehåller logik för bearbetning och länkar mellan noder ange dataflöde.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-116">Each node in a topology contains processing logic, and links between nodes indicate data flow.</span></span> <span data-ttu-id="0d5a1-117">Noder att mata in indata i topologin kallas kanaler, som kan användas för att ordna data.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-117">The nodes to inject input data into the topology are called Spouts, which can be used to sequence the data.</span></span> <span data-ttu-id="0d5a1-118">Indata kan finnas i filen loggar, transaktionella databas, system-prestandaräknaren osv. Noder med både inkommande och utgående dataflöden kallas bultar, vilket gör det faktiska data filtrering och val och aggregering.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-118">The input data could reside in file logs, transactional database, system performance counter etc. The nodes with both input and output data flows are called Bolts, which do the actual data filtering and selections and aggregation.</span></span>

<span data-ttu-id="0d5a1-119">Tjänstanslutningspunkten stöder bästa för på-minst en gång och exakt-behandling av data en gång.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-119">SCP supports best efforts, at-least-once and exactly-once data processing.</span></span> <span data-ttu-id="0d5a1-120">I ett distribuerat strömmande bearbetning-program kan olika fel inträffa under bearbetning av data, till exempel nätverksavbrott, maskinvarufel eller användarfel kod osv. På-minst en gång process säkerställer att alla data bearbetas minst en gång genom att spela upp automatiskt samma data när fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-120">In a distributed streaming processing application, various errors may happen during data processing, such as network outage, machine failure, or user code error etc. At-least-once processing ensures all data will be processed at least once by replaying automatically the same data when error happens.</span></span> <span data-ttu-id="0d5a1-121">På-minst en gång bearbetning är enkel och tillförlitlig och passar bra i många program.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-121">At-least-once processing is simple and reliable and suits well in many applications.</span></span> <span data-ttu-id="0d5a1-122">När programmet kräver exakt inventering, till exempel är på-minst en gång bearbetning dock inte tillräckligt eftersom samma data kan vara att spela upp i Programtopologi.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-122">However, when the application requires exact counting, for example, at-least-once processing is insufficient since the same data could potentially be played in the application topology.</span></span> <span data-ttu-id="0d5a1-123">I så fall, exakt-när bearbetningen är utformat för att kontrollera att resultatet är korrekt även när data kan spelas och behandlas flera gånger.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-123">In that case, exactly-once processing is designed to make sure the result is correct even when the data may be replayed and processed multiple times.</span></span>

<span data-ttu-id="0d5a1-124">SCP gör att .NET-utvecklare kan utveckla program i realtid data processen medan utnyttjar Java Virtual Machine (JVM) baserat Storm under skyddet.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-124">SCP enables .NET developers to develop real time data process applications while leverage the Java Virtual Machine (JVM) based Storm under the cover.</span></span> <span data-ttu-id="0d5a1-125">.NET och JVM kommunicera via TCP lokal socket.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-125">The .NET and JVM communicate via TCP local socket.</span></span> <span data-ttu-id="0d5a1-126">I praktiken är varje kanal/bult paret .net/Java processen där användaren-logik körs i .net-process som ett plugin-program.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-126">Basically each Spout/Bolt is a .Net/Java process pair, where the user logic runs in .Net process as a plugin.</span></span>

<span data-ttu-id="0d5a1-127">Flera steg krävs för att skapa ett program för databearbetning ovanpå SCP:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-127">To build a data processing application on top of SCP, several steps are needed:</span></span>

* <span data-ttu-id="0d5a1-128">Utforma och implementera kanaler att dra in data från kön.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-128">Design and implement the Spouts to pull in data from queue.</span></span>
* <span data-ttu-id="0d5a1-129">Utforma och implementera bultar för att behandla indata och spara data till externa butiker som databas.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-129">Design and implement Bolts to process the input data, and save data to external stores such as Database.</span></span>
* <span data-ttu-id="0d5a1-130">Utforma topologi, skicka och kör topologin.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-130">Design the topology, then submit and run the topology.</span></span> <span data-ttu-id="0d5a1-131">Topologin definierar brytpunkter och data som flödar mellan dess noder.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-131">The topology defines vertexes and the data flows between the vertexes.</span></span> <span data-ttu-id="0d5a1-132">SCP tar specifikationen topologi och distribuera den på ett Storm-kluster där varje vertex körs på en logisk nod.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-132">SCP will take the topology specification and deploy it on a Storm cluster, where each vertex runs on one logical node.</span></span> <span data-ttu-id="0d5a1-133">Redundans och skalning kommer typen av Schemaläggaren Storm.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-133">The failover and scaling will be taken care of by the Storm task scheduler.</span></span>

<span data-ttu-id="0d5a1-134">Det här dokumentet använder några enkla exempel för att gå igenom hur du skapar program för databearbetning med SCP.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-134">This document will use some simple examples to walk through how to build data processing application with SCP.</span></span>

## <a name="scp-plugin-interface"></a><span data-ttu-id="0d5a1-135">SCP-Plugin-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="0d5a1-135">SCP Plugin Interface</span></span>
<span data-ttu-id="0d5a1-136">SCP-plugin-program (eller program) är fristående EXEs som kan både körs i Visual Studio under utvecklingsfasen och anslutas till Storm-pipeline efter distributionen i produktion.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-136">SCP plugins (or applications) are standalone EXEs that can both run inside Visual Studio during the development phase, and be plugged into the Storm pipeline after deployment in production.</span></span> <span data-ttu-id="0d5a1-137">Skriva SCP plugin-programmet är på samma sätt som att skriva andra standard Windows konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-137">Writing the SCP plugin is just the same as writing any other standard Windows console applications.</span></span> <span data-ttu-id="0d5a1-138">SCP.NET plattform deklarerar vissa gränssnitt för kanal-/ bult och användarkod för plugin-programmet ska implementera dessa gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-138">SCP.NET platform declares some interface for spout/bolt, and the user plugin code should implement these interfaces.</span></span> <span data-ttu-id="0d5a1-139">Det huvudsakliga syftet med den här designen är att användaren kan fokusera på sina egna business logics och låta andra saker som ska hanteras av SCP.NET plattform.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-139">The main purpose of this design is that the user can focus on their own business logics, and leaving other things to be handled by SCP.NET platform.</span></span>

<span data-ttu-id="0d5a1-140">Plugin-programmet användarkod ska implementera ett av följande gränssnitt, beror på om topologin är transaktionell eller icke-transaktionell och om komponenten är kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-140">The user plugin code should implement one of the followings interfaces, depends on whether the topology is transactional or non-transactional, and whether the component is spout or bolt.</span></span>

* <span data-ttu-id="0d5a1-141">ISCPSpout</span><span class="sxs-lookup"><span data-stu-id="0d5a1-141">ISCPSpout</span></span>
* <span data-ttu-id="0d5a1-142">ISCPBolt</span><span class="sxs-lookup"><span data-stu-id="0d5a1-142">ISCPBolt</span></span>
* <span data-ttu-id="0d5a1-143">ISCPTxSpout</span><span class="sxs-lookup"><span data-stu-id="0d5a1-143">ISCPTxSpout</span></span>
* <span data-ttu-id="0d5a1-144">ISCPBatchBolt</span><span class="sxs-lookup"><span data-stu-id="0d5a1-144">ISCPBatchBolt</span></span>

### <a name="iscpplugin"></a><span data-ttu-id="0d5a1-145">ISCPPlugin</span><span class="sxs-lookup"><span data-stu-id="0d5a1-145">ISCPPlugin</span></span>
<span data-ttu-id="0d5a1-146">ISCPPlugin är gemensamt gränssnitt för alla typer av plugin-program.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-146">ISCPPlugin is the common interface for all kinds of plugins.</span></span> <span data-ttu-id="0d5a1-147">Det är för närvarande ett dummy gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-147">Currently, it is a dummy interface.</span></span>

    public interface ISCPPlugin 
    {
    }

### <a name="iscpspout"></a><span data-ttu-id="0d5a1-148">ISCPSpout</span><span class="sxs-lookup"><span data-stu-id="0d5a1-148">ISCPSpout</span></span>
<span data-ttu-id="0d5a1-149">ISCPSpout är gränssnittet för icke-transaktionell kanal.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-149">ISCPSpout is the interface for non-transactional spout.</span></span>

     public interface ISCPSpout : ISCPPlugin                    
     {
         void NextTuple(Dictionary<string, Object> parms);         
         void Ack(long seqId, Dictionary<string, Object> parms);   
         void Fail(long seqId, Dictionary<string, Object> parms);  
     }

<span data-ttu-id="0d5a1-150">När `NextTuple()` anropas, C\# användarkod kan generera en eller flera tupplar.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-150">When `NextTuple()` is called, the C\# user code can emit one or more tuples.</span></span> <span data-ttu-id="0d5a1-151">Om det finns inget att generera ska den här metoden returnera utan avger något.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-151">If there is nothing to emit, this method should return without emitting anything.</span></span> <span data-ttu-id="0d5a1-152">Det bör noteras som `NextTuple()`, `Ack()`, och `Fail()` kallas i en tät loop i en enskild tråd i C\# process.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-152">It should be noted that `NextTuple()`, `Ack()`, and `Fail()` are all called in a tight loop in a single thread in C\# process.</span></span> <span data-ttu-id="0d5a1-153">När det finns inga tupplar att generera, är det Välkommen företagspolicy ha NextTuple strömsparläge under en kort tidsperiod (till exempel 10 millisekunder) utan att avfallshantering för mycket CPU.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-153">When there are no tuples to emit, it is courteous to have NextTuple sleep for a short amount of time (such as 10 milliseconds) so as not to waste too much CPU.</span></span>

<span data-ttu-id="0d5a1-154">`Ack()`och `Fail()` ska bara anropas om ack mekanism är aktiverat i spec filen.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-154">`Ack()` and `Fail()` will be called only when ack mechanism is enabled in spec file.</span></span> <span data-ttu-id="0d5a1-155">Den `seqId` används för att identifiera den tuppel som ADE eller misslyckades.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-155">The `seqId` is used to identify the tuple which is acked or failed.</span></span> <span data-ttu-id="0d5a1-156">Så om ack är aktiverad i icke-transaktionell topologi, bör följande begär funktion användas i kanal:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-156">So if ack is enabled in non-transactional topology, the following emit function should be used in Spout:</span></span>

    public abstract void Emit(string streamId, List<object> values, long seqId); 

<span data-ttu-id="0d5a1-157">Om ack inte stöds i icke-transaktionell topologi, den `Ack()` och `Fail()` kan lämnas tomt funktion.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-157">If ack is not supported in non-transactional topology, the `Ack()` and `Fail()` can be left as empty function.</span></span>

<span data-ttu-id="0d5a1-158">Den `parms` indataparametrar i dessa funktioner är bara tom ordlista, de är reserverad för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-158">The `parms` input parameters in these functions are just empty Dictionary, they are reserved for future use.</span></span>

### <a name="iscpbolt"></a><span data-ttu-id="0d5a1-159">ISCPBolt</span><span class="sxs-lookup"><span data-stu-id="0d5a1-159">ISCPBolt</span></span>
<span data-ttu-id="0d5a1-160">ISCPBolt är gränssnittet för icke-transaktionell bulten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-160">ISCPBolt is the interface for non-transactional bolt.</span></span>

    public interface ISCPBolt : ISCPPlugin 
    {
    void Execute(SCPTuple tuple);           
    }

<span data-ttu-id="0d5a1-161">När nya tuppel är tillgänglig, den `Execute()` funktionen anropas för att bearbeta den.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-161">When new tuple is available, the `Execute()` function will be called to process it.</span></span>

### <a name="iscptxspout"></a><span data-ttu-id="0d5a1-162">ISCPTxSpout</span><span class="sxs-lookup"><span data-stu-id="0d5a1-162">ISCPTxSpout</span></span>
<span data-ttu-id="0d5a1-163">ISCPTxSpout är gränssnittet för transaktionell kanal.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-163">ISCPTxSpout is the interface for transactional spout.</span></span>

    public interface ISCPTxSpout : ISCPPlugin
    {
        void NextTx(out long seqId, Dictionary<string, Object> parms);  
        void Ack(long seqId, Dictionary<string, Object> parms);         
        void Fail(long seqId, Dictionary<string, Object> parms);        
    }

<span data-ttu-id="0d5a1-164">Precis som i sin icke-transaktionell motparterna del `NextTx()`, `Ack()`, och `Fail()` kallas i en tät loop i en enskild tråd i C\# process.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-164">Just like their non-transactional counter-part, `NextTx()`, `Ack()`, and `Fail()` are all called in a tight loop in a single thread in C\# process.</span></span> <span data-ttu-id="0d5a1-165">När det finns inga data att generera, är det Välkommen företagspolicy ha `NextTx` strömsparläge under en kort tidsperiod (10 millisekunder) utan att avfallshantering för mycket CPU.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-165">When there are no data to emit, it is courteous to have `NextTx` sleep for a short amount of time (10 milliseconds) so as not to waste too much CPU.</span></span>

<span data-ttu-id="0d5a1-166">`NextTx()`för att starta en ny transaktion Utdataparametern `seqId` används för att identifiera den transaktion som också används i `Ack()` och `Fail()`.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-166">`NextTx()` is called to start a new transaction, the out parameter `seqId` is used to identify the transaction, which is also used in `Ack()` and `Fail()`.</span></span> <span data-ttu-id="0d5a1-167">I `NextTx()`, användare som kan sända data till Java-sida.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-167">In `NextTx()`, user can emit data to Java side.</span></span> <span data-ttu-id="0d5a1-168">Data lagras i ZooKeeper att stödja repetitionsattacker.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-168">The data will be stored in ZooKeeper to support replay.</span></span> <span data-ttu-id="0d5a1-169">Eftersom kapaciteten för ZooKeeper är mycket begränsad användare genererar endast metadata, inte masskopiera data i transaktionella kanal.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-169">Because the capacity of ZooKeeper is very limited, user should only emit metadata, not bulk data in transactional spout.</span></span>

<span data-ttu-id="0d5a1-170">Storm ska spelas upp en transaktion automatiskt om den misslyckas så `Fail()` ska inte anropas i vanliga fall.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-170">Storm will replay a transaction automatically if it fails, so `Fail()` should not be called in normal case.</span></span> <span data-ttu-id="0d5a1-171">Men om SCP kan kontrollera metadata som sänds av transaktionella kanal, kan det anropa `Fail()` när metadata är ogiltiga.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-171">But if SCP can check the metadata emitted by transactional spout, it can call `Fail()` when the metadata is invalid.</span></span>

<span data-ttu-id="0d5a1-172">Den `parms` indataparametrar i dessa funktioner är bara tom ordlista, de är reserverad för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-172">The `parms` input parameters in these functions are just empty Dictionary, they are reserved for future use.</span></span>

### <a name="iscpbatchbolt"></a><span data-ttu-id="0d5a1-173">ISCPBatchBolt</span><span class="sxs-lookup"><span data-stu-id="0d5a1-173">ISCPBatchBolt</span></span>
<span data-ttu-id="0d5a1-174">ISCPBatchBolt är gränssnittet för transaktionell bulten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-174">ISCPBatchBolt is the interface for transactional bolt.</span></span>

    public interface ISCPBatchBolt : ISCPPlugin           
    {
        void Execute(SCPTuple tuple);
        void FinishBatch(Dictionary<string, Object> parms);  
    }

<span data-ttu-id="0d5a1-175">`Execute()`anropas när det finns nya tuppel anländer till bulten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-175">`Execute()` is called when there is new tuple arriving at the bolt.</span></span> <span data-ttu-id="0d5a1-176">`FinishBatch()`anropas när transaktionen avslutas.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-176">`FinishBatch()` is called when this transaction is ended.</span></span> <span data-ttu-id="0d5a1-177">Den `parms` indataparameter är reserverad för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-177">The `parms` input parameter is reserved for future use.</span></span>

<span data-ttu-id="0d5a1-178">För transaktionell topologi, är ett viktigt begrepp – `StormTxAttempt`.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-178">For transactional topology, there is an important concept – `StormTxAttempt`.</span></span> <span data-ttu-id="0d5a1-179">Den har två fält `TxId` och `AttemptId`.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-179">It has two fields, `TxId` and `AttemptId`.</span></span> <span data-ttu-id="0d5a1-180">`TxId`används för att identifiera en viss transaktion och för en given transaktion, det kan finnas flera försök om transaktionen misslyckas och spelas.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-180">`TxId` is used to identify a specific transaction, and for a given transaction, there may be multiple attempt if the transaction fails and is replayed.</span></span> <span data-ttu-id="0d5a1-181">SCP.NET nya kommer ett annat ISCPBatchBolt objekt från varje `StormTxAttempt`, precis som vilken vill Storm Java-sida.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-181">SCP.NET will new a different ISCPBatchBolt object to process each `StormTxAttempt`, just like what Storm do in Java side.</span></span> <span data-ttu-id="0d5a1-182">Syftet med den här designen är att stödja parallella transaktioner bearbetning.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-182">The purpose of this design is to support parallel transactions processing.</span></span> <span data-ttu-id="0d5a1-183">Användaren bör ha det i åtanke att om transaktionen försök är slutförd, motsvarande ISCPBatchBolt objektet ska förstöras och skräpinsamlats.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-183">User should keep it in mind that if transaction attempt is finished, the corresponding ISCPBatchBolt object will be destroyed and garbage collected.</span></span>

## <a name="object-model"></a><span data-ttu-id="0d5a1-184">Objektmodell</span><span class="sxs-lookup"><span data-stu-id="0d5a1-184">Object Model</span></span>
<span data-ttu-id="0d5a1-185">SCP.NET innehåller också en enkel uppsättning objekt för utvecklare att programmera med nycklar.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-185">SCP.NET also provides a simple set of key objects for developers to program with.</span></span> <span data-ttu-id="0d5a1-186">De är **kontexten**, **StateStore**, och **SCPRuntime**.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-186">They are **Context**, **StateStore**, and **SCPRuntime**.</span></span> <span data-ttu-id="0d5a1-187">De kommer att diskuteras i rest-delen av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-187">They will be discussed in the rest part of this section.</span></span>

### <a name="context"></a><span data-ttu-id="0d5a1-188">Kontext</span><span class="sxs-lookup"><span data-stu-id="0d5a1-188">Context</span></span>
<span data-ttu-id="0d5a1-189">Kontexten är en aktiv miljö till programmet.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-189">Context provides a running environment to the application.</span></span> <span data-ttu-id="0d5a1-190">Varje ISCPPlugin-instans (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) har en motsvarande kontext-instans.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-190">Each ISCPPlugin instance (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) has a corresponding Context instance.</span></span> <span data-ttu-id="0d5a1-191">Funktionerna i kontexten kan delas in i två delar: (1) menyns statiska del som är tillgängliga i hela C\# bearbeta, (2) den dynamiska delen som endast är tillgänglig för den specifika instansen i kontexten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-191">The functionality provided by Context can be divided into two parts: (1) the static part which is available in the whole C\# process, (2) the dynamic part which is only available for the specific Context instance.</span></span>

### <a name="static-part"></a><span data-ttu-id="0d5a1-192">Statiska del</span><span class="sxs-lookup"><span data-stu-id="0d5a1-192">Static Part</span></span>
    public static ILogger Logger = null;
    public static SCPPluginType pluginType;                      
    public static Config Config { get; set; }                    
    public static TopologyContext TopologyContext { get; set; }  

<span data-ttu-id="0d5a1-193">`Logger`finns i loggen syfte.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-193">`Logger` is provided for log purpose.</span></span>

<span data-ttu-id="0d5a1-194">`pluginType`används för att ange plugin-typ av C\# process.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-194">`pluginType` is used to indicate the plugin type of the C\# process.</span></span> <span data-ttu-id="0d5a1-195">Om C\# processen körs i lokala testläge (utan Java), plugin-typen är `SCP_NET_LOCAL`.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-195">If the C\# process is run in local test mode (without Java), the plugin type is `SCP_NET_LOCAL`.</span></span>

    public enum SCPPluginType 
    {
        SCP_NET_LOCAL = 0,       
        SCP_NET_SPOUT = 1,       
        SCP_NET_BOLT = 2,        
        SCP_NET_TX_SPOUT = 3,   
        SCP_NET_BATCH_BOLT = 4  
    }

<span data-ttu-id="0d5a1-196">`Config`finns för att hämta konfigurationsparametrar från Java-sida.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-196">`Config` is provided to get configuration parameters from Java side.</span></span> <span data-ttu-id="0d5a1-197">Parametrarna som skickas från Java sida när C\# plugin-programmet har initierats.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-197">The parameters are passed from Java side when C\# plugin is initialized.</span></span> <span data-ttu-id="0d5a1-198">Den `Config` parametrar är uppdelat i två delar: `stormConf` och `pluginConf`.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-198">The `Config` parameters are divided into two parts: `stormConf` and `pluginConf`.</span></span>

    public Dictionary<string, Object> stormConf { get; set; }  
    public Dictionary<string, Object> pluginConf { get; set; }  

<span data-ttu-id="0d5a1-199">`stormConf`är parametrar som definierats av Storm och `pluginConf` är de parametrar som definierats av SCP.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-199">`stormConf` is parameters defined by Storm and `pluginConf` is the parameters defined by SCP.</span></span> <span data-ttu-id="0d5a1-200">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-200">For example:</span></span>

    public class Constants
    {
        … …

        // constant string for pluginConf
        public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

        // constant string for stormConf
        public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";           
        public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";                 
    }

<span data-ttu-id="0d5a1-201">`TopologyContext`har angetts för att få kontexten topologi, är det mest användbar för komponenter med flera parallellitet.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-201">`TopologyContext` is provided to get the topology context, it is most useful for components with multiple parallelism.</span></span> <span data-ttu-id="0d5a1-202">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-202">Here is an example:</span></span>

    //demo how to get TopologyContext info
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

### <a name="dynamic-part"></a><span data-ttu-id="0d5a1-203">Dynamiska del</span><span class="sxs-lookup"><span data-stu-id="0d5a1-203">Dynamic Part</span></span>
<span data-ttu-id="0d5a1-204">Följande gränssnitt är relevant för en viss instans i kontexten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-204">The following interfaces are pertinent to a certain Context instance.</span></span> <span data-ttu-id="0d5a1-205">Kontexten instans skapas av SCP.NET plattform och skickas till användarkoden:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-205">The Context instance is created by SCP.NET platform and passed to the user code:</span></span>

    // Declare the Output and Input Stream Schemas

    public void DeclareComponentSchema(ComponentStreamSchema schema);   

    // Emit tuple to default stream.
    public abstract void Emit(List<object> values);                   

    // Emit tuple to the specific stream.
    public abstract void Emit(string streamId, List<object> values);  

<span data-ttu-id="0d5a1-206">För icke-transaktionell kanal stöder ack, finns följande metod:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-206">For non-transactional spout supporting ack, the following method is provided:</span></span>

    // for non-transactional Spout which supports ack
    public abstract void Emit(string streamId, List<object> values, long seqId);  

<span data-ttu-id="0d5a1-207">För icke-transaktionell bult stöder ack, bör det explicit `Ack()` eller `Fail()` tuppeln togs emot.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-207">For non-transactional bolt supporting ack, it should explicitly `Ack()` or `Fail()` the tuple it received.</span></span> <span data-ttu-id="0d5a1-208">Och när sändning nya tuppel, det måste även ange ankare nya parets.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-208">And when emitting new tuple, it must also specify the anchors of the new tuple.</span></span> <span data-ttu-id="0d5a1-209">Följande metoder tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-209">The following methods are provided.</span></span>

    public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values); 
    public abstract void Ack(SCPTuple tuple);
    public abstract void Fail(SCPTuple tuple);

### <a name="statestore"></a><span data-ttu-id="0d5a1-210">StateStore</span><span class="sxs-lookup"><span data-stu-id="0d5a1-210">StateStore</span></span>
<span data-ttu-id="0d5a1-211">`StateStore`innehåller metadatatjänster, monotonisk sekvensgenerering och vänta utan samordning.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-211">`StateStore` provides metadata services, monotonic sequence generation, and wait-free coordination.</span></span> <span data-ttu-id="0d5a1-212">Abstraktioner på högre nivåer distribuerade samtidighet kan baseras på `StateStore`, inklusive distribuerade lås, distribuerade köer, barriärer och Transaktionstjänster.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-212">Higher-level distributed concurrency abstractions can be built on `StateStore`, including distributed locks, distributed queues, barriers, and transaction services.</span></span>

<span data-ttu-id="0d5a1-213">SCP-program kan använda den `State` objekt att spara information i ZooKeeper, särskilt för transaktionell topologi.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-213">SCP applications may use the `State` object to persist some information in ZooKeeper, especially for transactional topology.</span></span> <span data-ttu-id="0d5a1-214">Om du gör det transaktionella kanal kraschar och startar om, den kan hämta nödvändig information från ZooKeeper och starta om pipeline.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-214">Doing so, if transactional spout crashes and restart, it can retrieve the necessary information from ZooKeeper and restart the pipeline.</span></span>

<span data-ttu-id="0d5a1-215">Den `StateStore` -objektet har huvudsakligen dessa metoder:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-215">The `StateStore` object mainly has these methods:</span></span>

    /// <summary>
    /// Static method to retrieve a state store of the given path and connStr 
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
    /// Get all the States in the StateStore
    /// </summary>
    /// <returns>All the States</returns>
    public IEnumerable<State> States();

    /// <summary>
    /// Get state or registry object
    /// </summary>
    /// <param name="info">Registry Name(Registry only)</param>
    /// <typeparam name="T">Type, Registry or State</typeparam>
    /// <returns>Return Registry or State</returns>
    public T Get<T>(string info = null);

    /// <summary>
    /// List all the committed states
    /// </summary>
    /// <returns>Registries contain the Committed State </returns> 
    public IEnumerable<Registry> Commited();

    /// <summary>
    /// List all the Aborted State in the StateStore
    /// </summary>
    /// <returns>Registries contain the Aborted State</returns>
    public IEnumerable<Registry> Aborted();

    /// <summary>
    /// Retrieve an existing state object from this state store instance 
    /// </summary>
    /// <returns>State from StateStore</returns>
    /// <typeparam name="T">stateId, id of the State</typeparam>
    public State GetState(long stateId)

<span data-ttu-id="0d5a1-216">Den `State` -objektet har huvudsakligen dessa metoder:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-216">The `State` object mainly has these methods:</span></span>

    /// <summary>
    /// Set the status of the state object to commit 
    /// </summary>
    public void Commit(bool simpleMode = true); 

    /// <summary>
    /// Set the status of the state object to abort 
    /// </summary>
    public void Abort();

    /// <summary>
    /// Put an attribute value under the give key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <param name="attribute">State Attribute</param> 
    public void PutAttribute<T>(string key, T attribute); 

    /// <summary>
    /// Get the attribute value associated with the given key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <returns>State Attribute</returns>               
    public T GetAttribute<T>(string key);                    

<span data-ttu-id="0d5a1-217">För den `Commit()` metoden när simpleMode har angetts till true, helt enkelt bort motsvarande ZNode i ZooKeeper.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-217">For the `Commit()` method, when simpleMode is set to true, it will simply delete the corresponding ZNode in ZooKeeper.</span></span> <span data-ttu-id="0d5a1-218">I annat fall tas bort aktuella ZNode och lägga till en ny nod i GENOMFÖRD\_sökväg.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-218">Otherwise, it will delete the current ZNode, and adding a new node in the COMMITTED\_PATH.</span></span>

### <a name="scpruntime"></a><span data-ttu-id="0d5a1-219">SCPRuntime</span><span class="sxs-lookup"><span data-stu-id="0d5a1-219">SCPRuntime</span></span>
<span data-ttu-id="0d5a1-220">SCPRuntime innehåller följande två metoder.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-220">SCPRuntime provides the following two methods.</span></span>

    public static void Initialize();

    public static void LaunchPlugin(newSCPPlugin createDelegate);  

<span data-ttu-id="0d5a1-221">`Initialize()`används för att initiera SCP-körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-221">`Initialize()` is used to initialize the SCP runtime environment.</span></span> <span data-ttu-id="0d5a1-222">Den här metoden C\# processen ska ansluta till Java-sida, och hämtar konfigurationsparametrar och topologi-kontexten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-222">In this method, the C\# process will connect to the Java side, and gets configuration parameters and topology context.</span></span>

<span data-ttu-id="0d5a1-223">`LaunchPlugin()`används för att startar bearbetning meddelandeloop.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-223">`LaunchPlugin()` is used to kick off the message processing loop.</span></span> <span data-ttu-id="0d5a1-224">I den här loop C\# plugin-program får meddelanden formuläret Java-sida (inklusive tupplar och kontroll signaler) och sedan bearbeta meddelanden, kanske anropar gränssnittsmetoden ange av användarkoden.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-224">In this loop, the C\# plugin will receive messages form Java side (including tuples and control signals), and then process the messages, perhaps calling the interface method provide by the user code.</span></span> <span data-ttu-id="0d5a1-225">Indataparametern för metoden `LaunchPlugin()` är en delegat som kan returnera ett objekt som implementerar ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-225">The input parameter for method `LaunchPlugin()` is a delegate that can return an object that implement ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt interface.</span></span>

    public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary\<string, Object\> parms); 

<span data-ttu-id="0d5a1-226">För ISCPBatchBolt, kan vi hämta `StormTxAttempt` från `parms`, och använda den för att bedöma om det är ett uppspelat försök.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-226">For ISCPBatchBolt, we can get `StormTxAttempt` from `parms`, and use it to judge whether it is a replayed attempt.</span></span> <span data-ttu-id="0d5a1-227">Detta görs vanligtvis på bulten genomförande och det visas i den `HelloWorldTx` exempel.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-227">This is usually done at the commit bolt, and it is demonstrated in the `HelloWorldTx` example.</span></span>

<span data-ttu-id="0d5a1-228">Generellt sett kan SCP-plugin-program köras i två lägen här:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-228">Generally speaking, the SCP plugins may run in two modes here:</span></span>

1. <span data-ttu-id="0d5a1-229">Lokala testläge: I det här läget SCP-plugin-program (C\# användarkod) körs i Visual Studio under utvecklingsfasen.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-229">Local Test Mode: In this mode, the SCP plugins (the C\# user code) run inside Visual Studio during the development phase.</span></span> <span data-ttu-id="0d5a1-230">`LocalContext`kan användas i det här läget, vilket ger en metod för att serialisera skickade tupplar lokala filer och läsa dem tillbaka till minne.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-230">`LocalContext` can be used in this mode, which provides method to serialize the emitted tuples to local files, and read them back to memory.</span></span>
   
        public interface ILocalContext
        {
            List\<SCPTuple\> RecvFromMsgQueue();
            void WriteMsgQueueToFile(string filepath, bool append = false);  
            void ReadFromFileToMsgQueue(string filepath);                    
        }
2. <span data-ttu-id="0d5a1-231">Standardläget: I det här läget SCP-plugin-program startas av storm java-process.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-231">Regular Mode: In this mode, the SCP plugins are launched by storm java process.</span></span>
   
    <span data-ttu-id="0d5a1-232">Här är ett exempel på Starta SCP-plugin-programmet:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-232">Here is an example of launching SCP plugin:</span></span>
   
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
            /* Setting the environment variable here can change the log file name */
            System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "HelloWorld");
   
            SCPRuntime.Initialize();
            SCPRuntime.LaunchPlugin(new newSCPPlugin(Generator.Get));
            }
        }
        }

## <a name="topology-specification-language"></a><span data-ttu-id="0d5a1-233">Topologi specifikationsspråk</span><span class="sxs-lookup"><span data-stu-id="0d5a1-233">Topology Specification Language</span></span>
<span data-ttu-id="0d5a1-234">SCP är-topologi ett specifikt språk domän för att beskriva och konfigurera SCP topologier.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-234">SCP Topology Specification is a domain specific language for describing and configuring SCP topologies.</span></span> <span data-ttu-id="0d5a1-235">Den är baserad på Storm's Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) och utökas med SCP.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-235">It is based on Storm’s Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) and is extended by SCP.</span></span>

<span data-ttu-id="0d5a1-236">Topologi specifikationer kan skickas direkt till storm-kluster för körning via den ***runspec*** kommando.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-236">Topology specifications can be submitted directly to storm cluster for execution via the ***runspec*** command.</span></span>

<span data-ttu-id="0d5a1-237">SCP.NET har Lägg till följande funktioner att definiera den transaktionella topologin:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-237">SCP.NET has add follow functions to define the Transactional Topology:</span></span>

| <span data-ttu-id="0d5a1-238">**Nya funktioner**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-238">**New Functions**</span></span> | <span data-ttu-id="0d5a1-239">**Parametrar**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-239">**Parameters**</span></span> | <span data-ttu-id="0d5a1-240">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-240">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0d5a1-241">**TX topolopy**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-241">**tx-topolopy**</span></span> |<span data-ttu-id="0d5a1-242">topologi namn</span><span class="sxs-lookup"><span data-stu-id="0d5a1-242">topology-name</span></span><br /><span data-ttu-id="0d5a1-243">kanal-karta</span><span class="sxs-lookup"><span data-stu-id="0d5a1-243">spout-map</span></span><br /><span data-ttu-id="0d5a1-244">bult-karta</span><span class="sxs-lookup"><span data-stu-id="0d5a1-244">bolt-map</span></span> |<span data-ttu-id="0d5a1-245">Definiera en transaktionell topologi med namnet topologi &nbsp;spouts definitionen mappning och bultar definitionen mappning</span><span class="sxs-lookup"><span data-stu-id="0d5a1-245">Define a transactional topology with the topology name, &nbsp;spouts definition map and the bolts definition map</span></span> |
| <span data-ttu-id="0d5a1-246">**SCP-tx-kanal**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-246">**scp-tx-spout**</span></span> |<span data-ttu-id="0d5a1-247">Exec-namn</span><span class="sxs-lookup"><span data-stu-id="0d5a1-247">exec-name</span></span><br /><span data-ttu-id="0d5a1-248">argument</span><span class="sxs-lookup"><span data-stu-id="0d5a1-248">args</span></span><br /><span data-ttu-id="0d5a1-249">Fält</span><span class="sxs-lookup"><span data-stu-id="0d5a1-249">fields</span></span> |<span data-ttu-id="0d5a1-250">Definiera en transaktionell kanal.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-250">Define a transactional spout.</span></span> <span data-ttu-id="0d5a1-251">Den kommer att köra programmet med ***exec-name*** med ***argument***.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-251">It will run the application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="0d5a1-252">Den ***fält*** är utdatafält för kanal</span><span class="sxs-lookup"><span data-stu-id="0d5a1-252">The ***fields*** is the Output Fields for spout</span></span> |
| <span data-ttu-id="0d5a1-253">**SCP-tx-batch-bult**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-253">**scp-tx-batch-bolt**</span></span> |<span data-ttu-id="0d5a1-254">Exec-namn</span><span class="sxs-lookup"><span data-stu-id="0d5a1-254">exec-name</span></span><br /><span data-ttu-id="0d5a1-255">argument</span><span class="sxs-lookup"><span data-stu-id="0d5a1-255">args</span></span><br /><span data-ttu-id="0d5a1-256">Fält</span><span class="sxs-lookup"><span data-stu-id="0d5a1-256">fields</span></span> |<span data-ttu-id="0d5a1-257">Definiera en transaktionell Batch bulten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-257">Define a transactional Batch Bolt.</span></span> <span data-ttu-id="0d5a1-258">Den kommer att köra programmet med ***exec-name*** med ***argument.***</span><span class="sxs-lookup"><span data-stu-id="0d5a1-258">It will run the application with ***exec-name*** using ***args.***</span></span><br /><br /><span data-ttu-id="0d5a1-259">Fälten är utdatafält för bulten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-259">The Fields is the Output Fields for bolt.</span></span> |
| <span data-ttu-id="0d5a1-260">**SCP-tx-commit-bult**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-260">**scp-tx-commit-bolt**</span></span> |<span data-ttu-id="0d5a1-261">Exec-namn</span><span class="sxs-lookup"><span data-stu-id="0d5a1-261">exec-name</span></span><br /><span data-ttu-id="0d5a1-262">argument</span><span class="sxs-lookup"><span data-stu-id="0d5a1-262">args</span></span><br /><span data-ttu-id="0d5a1-263">Fält</span><span class="sxs-lookup"><span data-stu-id="0d5a1-263">fields</span></span> |<span data-ttu-id="0d5a1-264">Definiera en transaktionell Committer bulten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-264">Define a transactional Committer Bolt.</span></span> <span data-ttu-id="0d5a1-265">Den kommer att köra programmet med ***exec-name*** med ***argument***.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-265">It will run the application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="0d5a1-266">Den ***fält*** är utdatafält för bult</span><span class="sxs-lookup"><span data-stu-id="0d5a1-266">The ***fields*** is the Output Fields for bolt</span></span> |
| <span data-ttu-id="0d5a1-267">**nontx topolopy**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-267">**nontx-topolopy**</span></span> |<span data-ttu-id="0d5a1-268">topologi namn</span><span class="sxs-lookup"><span data-stu-id="0d5a1-268">topology-name</span></span><br /><span data-ttu-id="0d5a1-269">kanal-karta</span><span class="sxs-lookup"><span data-stu-id="0d5a1-269">spout-map</span></span><br /><span data-ttu-id="0d5a1-270">bult-karta</span><span class="sxs-lookup"><span data-stu-id="0d5a1-270">bolt-map</span></span> |<span data-ttu-id="0d5a1-271">Definiera en icke-transaktionell topologi med namnet topologi&nbsp; spouts definitionen mappning och bultar definitionen mappning</span><span class="sxs-lookup"><span data-stu-id="0d5a1-271">Define a nontransactional topology with the topology name,&nbsp; spouts definition map and the bolts definition map</span></span> |
| <span data-ttu-id="0d5a1-272">**SCP-kanal**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-272">**scp-spout**</span></span> |<span data-ttu-id="0d5a1-273">Exec-namn</span><span class="sxs-lookup"><span data-stu-id="0d5a1-273">exec-name</span></span><br /><span data-ttu-id="0d5a1-274">argument</span><span class="sxs-lookup"><span data-stu-id="0d5a1-274">args</span></span><br /><span data-ttu-id="0d5a1-275">Fält</span><span class="sxs-lookup"><span data-stu-id="0d5a1-275">fields</span></span><br /><span data-ttu-id="0d5a1-276">Parametrar</span><span class="sxs-lookup"><span data-stu-id="0d5a1-276">parameters</span></span> |<span data-ttu-id="0d5a1-277">Definiera en icke-transaktionell kanal.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-277">Define a nontransactional spout.</span></span> <span data-ttu-id="0d5a1-278">Den kommer att köra programmet med ***exec-name*** med ***argument***.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-278">It will run the application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="0d5a1-279">Den ***fält*** är utdatafält för kanal</span><span class="sxs-lookup"><span data-stu-id="0d5a1-279">The ***fields*** is the Output Fields for spout</span></span><br /><br /><span data-ttu-id="0d5a1-280">Den ***parametrar*** är valfritt, använder den för att ange vissa parametrar, till exempel ”nontransactional.ack.enabled”.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-280">The ***parameters*** is optional, using it to specify some parameters such as "nontransactional.ack.enabled".</span></span> |
| <span data-ttu-id="0d5a1-281">**SCP-bult**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-281">**scp-bolt**</span></span> |<span data-ttu-id="0d5a1-282">Exec-namn</span><span class="sxs-lookup"><span data-stu-id="0d5a1-282">exec-name</span></span><br /><span data-ttu-id="0d5a1-283">argument</span><span class="sxs-lookup"><span data-stu-id="0d5a1-283">args</span></span><br /><span data-ttu-id="0d5a1-284">Fält</span><span class="sxs-lookup"><span data-stu-id="0d5a1-284">fields</span></span><br /><span data-ttu-id="0d5a1-285">Parametrar</span><span class="sxs-lookup"><span data-stu-id="0d5a1-285">parameters</span></span> |<span data-ttu-id="0d5a1-286">Definiera en icke-transaktionell bulten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-286">Define a nontransactional Bolt.</span></span> <span data-ttu-id="0d5a1-287">Den kommer att köra programmet med ***exec-name*** med ***argument***.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-287">It will run the application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="0d5a1-288">Den ***fält*** är utdatafält för bult</span><span class="sxs-lookup"><span data-stu-id="0d5a1-288">The ***fields*** is the Output Fields for bolt</span></span><br /><br /><span data-ttu-id="0d5a1-289">Den ***parametrar*** är valfritt, använder den för att ange vissa parametrar, till exempel ”nontransactional.ack.enabled”.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-289">The ***parameters*** is optional, using it to specify some parameters such as "nontransactional.ack.enabled".</span></span> |

<span data-ttu-id="0d5a1-290">SCP.NET har Följ nycklar ord som definierats:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-290">SCP.NET has follow keys words defined:</span></span>

| <span data-ttu-id="0d5a1-291">**Nyckelord**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-291">**Key Words**</span></span> | <span data-ttu-id="0d5a1-292">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-292">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="0d5a1-293">**: namn**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-293">**:name**</span></span> |<span data-ttu-id="0d5a1-294">Definiera namnet topologi</span><span class="sxs-lookup"><span data-stu-id="0d5a1-294">Define the Topology Name</span></span> |
| <span data-ttu-id="0d5a1-295">**: topologi**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-295">**:topology**</span></span> |<span data-ttu-id="0d5a1-296">Definiera topologin med hjälp av dessa funktioner och skapa i viktiga.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-296">Define the Topology using the above functions and build in ones.</span></span> |
| <span data-ttu-id="0d5a1-297">**: p**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-297">**:p**</span></span> |<span data-ttu-id="0d5a1-298">Definiera parallellitet-tips för varje kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-298">Define the parallelism hint for each spout or bolt.</span></span> |
| <span data-ttu-id="0d5a1-299">**: config**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-299">**:config**</span></span> |<span data-ttu-id="0d5a1-300">Definiera konfigurera parameter eller uppdatera befintliga</span><span class="sxs-lookup"><span data-stu-id="0d5a1-300">Define configure parameter or update the existing ones</span></span> |
| <span data-ttu-id="0d5a1-301">**: schemat**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-301">**:schema**</span></span> |<span data-ttu-id="0d5a1-302">Definiera schemat för dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-302">Define the Schema of Stream.</span></span> |

<span data-ttu-id="0d5a1-303">Och vanliga parametrar:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-303">And frequently-used parameters:</span></span>

| <span data-ttu-id="0d5a1-304">**Parametern**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-304">**Parameter**</span></span> | <span data-ttu-id="0d5a1-305">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-305">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="0d5a1-306">**”plugin.name”**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-306">**"plugin.name"**</span></span> |<span data-ttu-id="0d5a1-307">namnet på C# plugin-programmet exe-filen</span><span class="sxs-lookup"><span data-stu-id="0d5a1-307">exe file name of the C# plugin</span></span> |
| <span data-ttu-id="0d5a1-308">**”plugin.args”**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-308">**"plugin.args"**</span></span> |<span data-ttu-id="0d5a1-309">plugin-argument</span><span class="sxs-lookup"><span data-stu-id="0d5a1-309">plugin args</span></span> |
| <span data-ttu-id="0d5a1-310">**”output.schema”**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-310">**"output.schema"**</span></span> |<span data-ttu-id="0d5a1-311">Utdataschemat</span><span class="sxs-lookup"><span data-stu-id="0d5a1-311">Output schema</span></span> |
| <span data-ttu-id="0d5a1-312">**”nontransactional.ack.enabled”**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-312">**"nontransactional.ack.enabled"**</span></span> |<span data-ttu-id="0d5a1-313">Om ack har aktiverats för icke-transaktionell topologi</span><span class="sxs-lookup"><span data-stu-id="0d5a1-313">Whether ack is enabled for nontransactional topology</span></span> |

<span data-ttu-id="0d5a1-314">Kommandot runspec kommer att distribueras tillsammans med bits, användningen är t.ex.:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-314">The runspec command will be deployed together with the bits, the usage is like:</span></span>

    .\bin\runSpec.cmd
    usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
    ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target

<span data-ttu-id="0d5a1-315">Den ***resurs dir*** -parametern är valfri, måste du ange den när du vill ansluta en C\# programmet och den här katalogen ska innehålla programmet, beroenden och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-315">The ***resource-dir*** parameter is optional, you need to specify it when you want to plug a C\# application, and this directory will contain the application, the dependencies and configurations.</span></span>

<span data-ttu-id="0d5a1-316">Den ***klassökvägen*** parametern också är valfri.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-316">The ***classpath*** parameter is also optional.</span></span> <span data-ttu-id="0d5a1-317">Den används för att ange klassökväg Java om spec filen innehåller Java-kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-317">It is used to specify the Java classpath if the spec file contains Java Spout or Bolt.</span></span>

## <a name="miscellaneous-features"></a><span data-ttu-id="0d5a1-318">Diverse funktioner</span><span class="sxs-lookup"><span data-stu-id="0d5a1-318">Miscellaneous Features</span></span>
### <a name="input-and-output-schema-declaration"></a><span data-ttu-id="0d5a1-319">Indata och utdata Schema-deklaration</span><span class="sxs-lookup"><span data-stu-id="0d5a1-319">Input and Output Schema Declaration</span></span>
<span data-ttu-id="0d5a1-320">Användaren kan generera tuppel i C\# process, plattformen måste serialisera tuppeln till byte [], överföring till Java-sida och Storm överför det här tuppeln till mål.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-320">The user can emit tuple in C\# process, the platform needs to serialize the tuple into byte[], transfer to Java side, and Storm will transfer this tuple to the targets.</span></span> <span data-ttu-id="0d5a1-321">Under tiden i underordnade komponent C\# processen kommer ta emot tuppel tillbaka från java sida och konvertera den till de ursprungliga typerna av plattform, dessa åtgärder är dolda av plattformen.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-321">Meanwhile in downstream component, the C\# process will receive tuple back from java side, and convert it to the original types by platform, all these operations are hidden by the Platform.</span></span>

<span data-ttu-id="0d5a1-322">Användarkod måste deklarera schemat för indata och utdata för att stödja serialisering och deserialisering.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-322">To support the serialization and deserialization, user code needs to declare the schema of the inputs and outputs.</span></span>

<span data-ttu-id="0d5a1-323">Schemat för i/o-dataströmmen har definierats som en ordlista, nyckeln är StreamId och värdet är typerna av kolumnerna.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-323">The input/output stream schema is defined as a dictionary, the key is the StreamId and the value is the Types of the columns.</span></span> <span data-ttu-id="0d5a1-324">Komponenten kan ha flera strömmar som deklarerats.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-324">The component can have multi-streams declared.</span></span>

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


<span data-ttu-id="0d5a1-325">Vi har lagt till följande API i Context-objektet:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-325">In Context object, we have the following API added:</span></span>

    public void DeclareComponentSchema(ComponentStreamSchema schema)

<span data-ttu-id="0d5a1-326">Användarkod måste kontrollera att tupplar som orsakat följa det schema som definierats för dataströmmen eller systemet genereras ett undantagsfel för körning.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-326">User code must make sure the tuples emitted obey the schema defined for that stream, or the system will throw a runtime exception.</span></span>

### <a name="multi-stream-support"></a><span data-ttu-id="0d5a1-327">Stöd för flera dataström</span><span class="sxs-lookup"><span data-stu-id="0d5a1-327">Multi-Stream Support</span></span>
<span data-ttu-id="0d5a1-328">Tjänstanslutningspunkten stöder användarkod för att generera eller ta emot från flera olika dataströmmar på samma gång.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-328">SCP supports user code to emit or receive from multiple distinct streams at the same time.</span></span> <span data-ttu-id="0d5a1-329">Stödet visar i Context-objektet som begär metoden tar en valfri dataströmmen ID-parametern.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-329">The support reflects in the Context object as the Emit method takes an optional stream ID parameter.</span></span>

<span data-ttu-id="0d5a1-330">Två metoder i SCP.NET Context-objektet har lagts till.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-330">Two methods in the SCP.NET Context object have been added.</span></span> <span data-ttu-id="0d5a1-331">De används för att generera tuppeln eller Tupplar om du vill ange StreamId.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-331">They are used to emit Tuple or Tuples to specify StreamId.</span></span> <span data-ttu-id="0d5a1-332">StreamId är en sträng och den måste vara konsekvent i båda C\# och topologi Definition-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-332">The StreamId is a string and it needs to be consistent in both C\# and the Topology Definition Spec.</span></span>

        /* Emit tuple to the specific stream. */
        public abstract void Emit(string streamId, List<object> values);

        /* for non-transactional Spout only */
        public abstract void Emit(string streamId, List<object> values, long seqId);

<span data-ttu-id="0d5a1-333">Sändning till en dataström som inte finns kommer att orsaka körningsfel undantag.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-333">The emitting to a non-existing stream will cause runtime exceptions.</span></span>

### <a name="fields-grouping"></a><span data-ttu-id="0d5a1-334">Fält gruppering</span><span class="sxs-lookup"><span data-stu-id="0d5a1-334">Fields Grouping</span></span>
<span data-ttu-id="0d5a1-335">Den inbyggda fält gruppering i Strom fungerar inte korrekt i SCP.NET.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-335">The build-in Fields Grouping in Strom is not working properly in SCP.NET.</span></span> <span data-ttu-id="0d5a1-336">Alla fält datatyper är faktiskt byte [] på Java-Proxy-sida och fälten Gruppera använder byte [] objekt hash-kod för att utföra grupperingen.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-336">On the Java Proxy side, all the fields data types are actually byte[], and the fields grouping uses the byte[] object hash code to perform the grouping.</span></span> <span data-ttu-id="0d5a1-337">Byte [] objekt hash-koden är adressen till det här objektet i minnet.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-337">The byte[] object hash code is the address of this object in memory.</span></span> <span data-ttu-id="0d5a1-338">Grupperingen kommer därför att fel för två byte [] objekt som delar samma innehåll, men inte samma adress.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-338">So the grouping will be wrong for two byte[] objects that share the same content but not the same address.</span></span>

<span data-ttu-id="0d5a1-339">SCP.NET lägger till en metod för anpassad gruppering och innehållet i byte [] används för att göra grupperingen.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-339">SCP.NET adds a customized grouping method, and it will use the content of the byte[] to do the grouping.</span></span> <span data-ttu-id="0d5a1-340">I **SPEC** filen syntax är t.ex.:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-340">In **SPEC** file, the syntax is like:</span></span>

    (bolt-spec
        {
            "spout_test" (scp-field-group :non-tx [0,1])
        }
        …
    )


<span data-ttu-id="0d5a1-341">Här</span><span class="sxs-lookup"><span data-stu-id="0d5a1-341">Here,</span></span>

1. <span data-ttu-id="0d5a1-342">”scp-fältet-grupp”: ”anpassad fältet gruppering implementeras av SCP”.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-342">"scp-field-group" means "Customized field grouping implemented by SCP".</span></span>
2. <span data-ttu-id="0d5a1-343">”: tx” eller ”: icke-tx” innebär att om det är transaktionell topologi.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-343">":tx" or ":non-tx" means if it’s transactional topology.</span></span> <span data-ttu-id="0d5a1-344">Vi behöver informationen eftersom startindexet skiljer sig åt i tx kontra-tx topologier.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-344">We need this information since the starting index is different in tx vs. non-tx topologies.</span></span>
3. <span data-ttu-id="0d5a1-345">[0,1] innebär en hashset av fältet ID, början på 0.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-345">[0,1] means a hashset of field Ids, starting from 0.</span></span>

### <a name="hybrid-topology"></a><span data-ttu-id="0d5a1-346">Hybrid-topologi</span><span class="sxs-lookup"><span data-stu-id="0d5a1-346">Hybrid topology</span></span>
<span data-ttu-id="0d5a1-347">Den interna Storm är skriven i Java.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-347">The native Storm is written in Java.</span></span> <span data-ttu-id="0d5a1-348">Och SCP.Net har förbättrad den, så att våra tull att skriva C\# kod för att hantera sina affärslogik.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-348">And SCP.Net has enhanced it to enable our customs to write C\# code to handle their business logic.</span></span> <span data-ttu-id="0d5a1-349">Men vi stöder också hybridtopologier, som innehåller inte bara C\# kanaler/bultar men Java kanal/bultar.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-349">But we also support hybrid topologies, which contains not only C\# spouts/bolts, but also Java Spout/Bolts.</span></span>

### <a name="specify-java-spoutbolt-in-spec-file"></a><span data-ttu-id="0d5a1-350">Ange kanal/bult för Java i spec fil</span><span class="sxs-lookup"><span data-stu-id="0d5a1-350">Specify Java Spout/Bolt in spec file</span></span>
<span data-ttu-id="0d5a1-351">”Scp-kanal” och ”scp-bult” kan också användas för att ange Java Spouts och bultar i spec fil, här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-351">In spec file, "scp-spout" and "scp-bolt" can also be used to specify Java Spouts and Bolts, here is an example:</span></span>

    (spout-spec 
      (microsoft.scp.example.HybridTopology.Generator.)           
      :p 1)

<span data-ttu-id="0d5a1-352">Här `microsoft.scp.example.HybridTopology.Generator` är namnet på klassen Java-kanalen.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-352">Here `microsoft.scp.example.HybridTopology.Generator` is the name of the Java Spout class.</span></span>

### <a name="specify-java-classpath-in-runspec-command"></a><span data-ttu-id="0d5a1-353">Ange Java-klassökvägen i runSpec kommando</span><span class="sxs-lookup"><span data-stu-id="0d5a1-353">Specify Java Classpath in runSpec Command</span></span>
<span data-ttu-id="0d5a1-354">Om du vill skicka topologi som innehåller Java Spouts eller bultar måste du först kompilera Java Spouts eller bultar och hämta Jar-filerna.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-354">If you want to submit topology containing Java Spouts or Bolts, you need to first compile the Java Spouts or Bolts and get the Jar files.</span></span> <span data-ttu-id="0d5a1-355">Du bör ange java-klassökvägen som innehåller Jar-filer när du skickar in topologin.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-355">Then you should specify the java classpath that contains the Jar files when submitting topology.</span></span> <span data-ttu-id="0d5a1-356">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-356">Here is an example:</span></span>

    bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*

<span data-ttu-id="0d5a1-357">Här **exempel\\HybridTopology\\java\\mål\\**  är den mapp som innehåller Java kanal/bult Jar-filen.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-357">Here **examples\\HybridTopology\\java\\target\\** is the folder containing the Java Spout/Bolt Jar file.</span></span>

### <a name="serialization-and-deserialization-between-java-and-c"></a><span data-ttu-id="0d5a1-358">Serialisering och deserialisering mellan Java och C\\</span><span class="sxs-lookup"><span data-stu-id="0d5a1-358">Serialization and Deserialization between Java and C\\</span></span>
<span data-ttu-id="0d5a1-359">Vår SCP-komponenten innehåller Java sida och C\# sida.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-359">Our SCP component includes Java side and C\# side.</span></span> <span data-ttu-id="0d5a1-360">För att kunna interagera med inbyggda Java kanaler/bultar serialisering/deserialisering utföras mellan Java sida och C\# sida, enligt beskrivningen i följande diagram.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-360">In order to interact with native Java Spouts/Bolts, Serialization/Deserialization must be carried out between Java side and C\# side, as illustrated in the following graph.</span></span>

![diagram över java-komponent som skickas till SCP-komponent som skickas till Java-komponent](media/hdinsight-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

1. <span data-ttu-id="0d5a1-362">**Serialisering i Java-sida och deserialisering i C\# sida**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-362">**Serialization in Java side and Deserialization in C\# side**</span></span>
   
   <span data-ttu-id="0d5a1-363">Första vi tillhandahåller standardimplementering för serialisering i Java-sida och deserialisering i C\# sida.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-363">First we provide default implementation for serialization in Java side and deserialization in C\# side.</span></span> <span data-ttu-id="0d5a1-364">Serialiseringsmetod i Java-sida kan anges i SPEC filen:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-364">The serialization method in Java side can be specified in SPEC file:</span></span>
   
       (scp-bolt
           {
               "plugin.name" "HybridTopology.exe"
               "plugin.args" ["displayer"]
               "output.schema" {}
               "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
           })
   
   <span data-ttu-id="0d5a1-365">Metoden deserialisering i C\# sida måste anges i C\# användarkod:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-365">The deserialization method in C\# side should be specified in C\# user code:</span></span>
   
       Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
       inputSchema.Add("default", new List<Type>() { typeof(Person) });
       this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
       this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());            
   
   <span data-ttu-id="0d5a1-366">Den här standardimplementering ska hantera de flesta fall om datatypen inte är för komplex.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-366">This default implementation should handle most cases if the data type is not too complex.</span></span> <span data-ttu-id="0d5a1-367">För vissa fall, eftersom användaren datatyp är för komplex eller eftersom prestanda för våra standardimplementering inte uppfyller kraven för användarens, användaren kan plugin-programmet sina egna implementering.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-367">For certain cases, either because the user data type is too complex, or because the performance of our default implementation does not meet the user's requirement, user can plug-in their own implementation.</span></span>
   
   <span data-ttu-id="0d5a1-368">Serialisera gränssnittet i java-sida definieras som:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-368">The serialize interface in java side is defined as:</span></span>
   
       public interface ICustomizedInteropJavaSerializer {
           public void prepare(String[] args);
           public List<ByteBuffer> serialize(List<Object> objectList);
       }
   
   <span data-ttu-id="0d5a1-369">Gränssnittet deserialize i C\# sida definieras som:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-369">The deserialize interface in C\# side is defined as:</span></span>
   
   <span data-ttu-id="0d5a1-370">offentliga gränssnittet ICustomizedInteropCSharpDeserializer</span><span class="sxs-lookup"><span data-stu-id="0d5a1-370">public interface ICustomizedInteropCSharpDeserializer</span></span>
   
       public interface ICustomizedInteropCSharpDeserializer
       {
           List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
       }
2. <span data-ttu-id="0d5a1-371">**Serialisering i C\# sida och deserialisering i Java sida sida**</span><span class="sxs-lookup"><span data-stu-id="0d5a1-371">**Serialization in C\# side and Deserialization in Java side side**</span></span>
   
   <span data-ttu-id="0d5a1-372">Serialiseringsmetod i C\# sida måste anges i C\# användarkod:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-372">The serialization method in C\# side should be specified in C\# user code:</span></span>
   
       this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
   
   <span data-ttu-id="0d5a1-373">Metoden deserialisering i Java-sida måste anges i SPEC fil:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-373">The Deserialization method in Java side should be specified in SPEC file:</span></span>
   
     <span data-ttu-id="0d5a1-374">(scp-kanal</span><span class="sxs-lookup"><span data-stu-id="0d5a1-374">(scp-spout</span></span>
   
       {
         "plugin.name" "HybridTopology.exe"
         "plugin.args" ["generator"]
         "output.schema" {"default" ["person"]}
         "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
       })
   
   <span data-ttu-id="0d5a1-375">”Microsoft.scp.example.HybridTopology.Person” är målklassen data avserialiseras till ”microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer” är här namnet på funktionen för avserialisering.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-375">Here "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" is the name of Deserializer, and "microsoft.scp.example.HybridTopology.Person" is the target class the data is deserialized to.</span></span>
   
   <span data-ttu-id="0d5a1-376">Användaren kan också plugin-programmet sina egna implementering av C\# serialiseraren och Java funktionen för avserialisering.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-376">User can also plug-in their own implementation of C\# serializer and Java Deserializer.</span></span> <span data-ttu-id="0d5a1-377">Det här är gränssnittet för C\# serialiseraren:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-377">This is the interface for C\# serializer:</span></span>
   
       public interface ICustomizedInteropCSharpSerializer
       {
           List<byte[]> Serialize(List<object> dataList);
       }
   
   <span data-ttu-id="0d5a1-378">Det här är gränssnittet för Java funktionen för avserialisering:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-378">This is the interface for Java Deserializer:</span></span>
   
       public interface ICustomizedInteropJavaDeserializer {
           public void prepare(String[] targetClassNames);
           public List<Object> Deserialize(List<ByteBuffer> dataList);
       }

## <a name="scp-host-mode"></a><span data-ttu-id="0d5a1-379">Värden för SCP-läge</span><span class="sxs-lookup"><span data-stu-id="0d5a1-379">SCP Host Mode</span></span>
<span data-ttu-id="0d5a1-380">I det här läget kan användaren kompilera koderna till DLL-filen och använda SCPHost.exe som tillhandahålls av SCP för att skicka topologi.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-380">In this mode, user can compile their codes to DLL, and use SCPHost.exe provided by SCP to submit topology.</span></span> <span data-ttu-id="0d5a1-381">Klientfilsspecifik filen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-381">The spec file looks like this:</span></span>

    (scp-spout
      {
        "plugin.name" "SCPHost.exe"
        "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
        "output.schema" {"default" ["sentence"]}
      })

<span data-ttu-id="0d5a1-382">Här, `plugin.name` har angetts som `SCPHost.exe` som tillhandahålls av SCP SDK.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-382">Here, `plugin.name` is specified as `SCPHost.exe` provided by SCP SDK.</span></span> <span data-ttu-id="0d5a1-383">SCPHost.exe som accepterar exakt tre parametrar:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-383">SCPHost.exe which accepts exactly three parameters:</span></span>

1. <span data-ttu-id="0d5a1-384">Den första är DLL-namn, som är `"HelloWorld.dll"` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-384">The first one is the DLL name, which is `"HelloWorld.dll"` in this example.</span></span>
2. <span data-ttu-id="0d5a1-385">Den andra är klassnamnet, som är `"Scp.App.HelloWorld.Generator"` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-385">The second one is the Class name, which is `"Scp.App.HelloWorld.Generator"` in this example.</span></span>
3. <span data-ttu-id="0d5a1-386">Det tredje är namnet på en offentlig statisk metod som kan anropas för att hämta en instans av ISCPPlugin.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-386">The third one is the name of a public static method, which can be invoked to get an instance of ISCPPlugin.</span></span>

<span data-ttu-id="0d5a1-387">Användarkod i värden läge är kompilerad som DLL-filen och anropas av SCP-plattformen.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-387">In host mode, user code is compiled as DLL, and is invoked by SCP platform.</span></span> <span data-ttu-id="0d5a1-388">SCP-plattformen kan så få fullständig kontroll över hela standardbearbetningslogiken.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-388">So SCP platform can get full control of the whole processing logic.</span></span> <span data-ttu-id="0d5a1-389">Så rekommenderar vi våra kunder att skicka topologi i SCP värden läge eftersom den kan förenkla utvecklingen och sätta oss större flexibilitet och bättre bakåtkompatibilitet för samt senare version.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-389">So we recommend our customers to submit topology in SCP host mode since it can simplify the development experience and bring us more flexibility and better backward compatibility for later release as well.</span></span>

## <a name="scp-programming-examples"></a><span data-ttu-id="0d5a1-390">Exempel för SCP-programmering</span><span class="sxs-lookup"><span data-stu-id="0d5a1-390">SCP Programming Examples</span></span>
### <a name="helloworld"></a><span data-ttu-id="0d5a1-391">HelloWorld</span><span class="sxs-lookup"><span data-stu-id="0d5a1-391">HelloWorld</span></span>
<span data-ttu-id="0d5a1-392">**HelloWorld** är ett väldigt enkelt exempel för att visa uppleva SCP.Net.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-392">**HelloWorld** is a very simple example to show a taste of SCP.Net.</span></span> <span data-ttu-id="0d5a1-393">Den använder en icke-transaktionell topologi med en kanal som kallas **generator**, och två bultar kallas **delningslisten** och **räknaren**.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-393">It uses a non-transactional topology, with a spout called **generator**, and two bolts called **splitter** and **counter**.</span></span> <span data-ttu-id="0d5a1-394">Kanal **generator** att slumpmässigt generera vissa meningar och genererar meningar för att **delningslisten**.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-394">The spout **generator** will randomly generate some sentences, and emit these sentences to **splitter**.</span></span> <span data-ttu-id="0d5a1-395">Bulten **delningslisten** ska dela meningar ord och generera dessa ord att **räknaren** bulten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-395">The bolt **splitter** will split the sentences to words and emit these words to **counter** bolt.</span></span> <span data-ttu-id="0d5a1-396">Bult ”räknaren” använder en ordlista för att registrera förekomsten av varje ord.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-396">The bolt "counter" uses a dictionary to record the occurrence number of each word.</span></span>

<span data-ttu-id="0d5a1-397">Det finns två spec filer **HelloWorld.spec** och **HelloWorld\_EnableAck.spec** för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-397">There are two spec files, **HelloWorld.spec** and **HelloWorld\_EnableAck.spec** for this example.</span></span> <span data-ttu-id="0d5a1-398">I C\# kod, det kan ta reda på om ack aktiveras genom att hämta pluginConf från Java-sida.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-398">In the C\# code, it can find out whether ack is enabled by getting the pluginConf from Java side.</span></span>

    /* demo how to get pluginConf info */
    if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
    {
        enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
    }
    Context.Logger.Info("enableAck: {0}", enableAck);

<span data-ttu-id="0d5a1-399">I kanal om ack aktiveras används en ordlista till att cachelagra tupplar som inte är ADE.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-399">In the spout, if ack is enabled, a dictionary is used to cache the tuples that have not been acked.</span></span> <span data-ttu-id="0d5a1-400">Om Fail() anropas, kommer misslyckade tuppeln återupprepas:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-400">If Fail() is called, the failed tuple will be replayed:</span></span>

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        Context.Logger.Info("Fail, seqId: {0}", seqId);
        if (cachedTuples.ContainsKey(seqId))
        {
            /* get the cached tuple */
            string sentence = cachedTuples[seqId];

            /* replay the failed tuple */
            Context.Logger.Info("Re-Emit: {0}, seqId: {1}", sentence, seqId);
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), seqId);
        }
        else
        {
            Context.Logger.Warn("Fail(), can't find cached tuple for seqId {0}!", seqId);
        }
    }

### <a name="helloworldtx"></a><span data-ttu-id="0d5a1-401">HelloWorldTx</span><span class="sxs-lookup"><span data-stu-id="0d5a1-401">HelloWorldTx</span></span>
<span data-ttu-id="0d5a1-402">Den **HelloWorldTx** exemplet visar hur du implementerar transaktionella topologi.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-402">The **HelloWorldTx** example demonstrates how to implement transactional topology.</span></span> <span data-ttu-id="0d5a1-403">Den har en kanal som kallas **generator**en batch bultar kallas **partiell antal**, och en bult commit anropas **summan antal**.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-403">It have one spout called **generator**, a batch bolts called **partial-count**, and a commit bolt called **count-sum**.</span></span> <span data-ttu-id="0d5a1-404">Det finns tre förskapade txt-filer: **DataSource0.txt**, **DataSource1.txt** och **DataSource2.txt**.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-404">There are also three pre-created txt files: **DataSource0.txt**, **DataSource1.txt** and **DataSource2.txt**.</span></span>

<span data-ttu-id="0d5a1-405">I varje transaktion, kanal **generator** kommer slumpmässigt väljer två filer i förväg skapade tre filer och generera två filnamnen till den **partiell antal** bulten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-405">In each transaction, the spout **generator** will randomly choose two files from the pre-created three files, and emit the two file names to the **partial-count** bolt.</span></span> <span data-ttu-id="0d5a1-406">Bulten **partiell antal** kommer först hämta filnamnet från mottagna tuppeln och sedan öppna filen räkna antalet ord i den här filen och slutligen genererar word numret till den **antal summan** bulten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-406">The bolt **partial-count** will first get the file name from the received tuple, then open the file and count the number of words in this file, and finally emit the word number to the **count-sum** bolt.</span></span> <span data-ttu-id="0d5a1-407">Den **antal summan** bult sammanfattas det totala antalet.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-407">The **count-sum** bolt will summarize the total count.</span></span>

<span data-ttu-id="0d5a1-408">Att uppnå **exakt en gång** semantik, commit bulten **summan antal** behöver för att bedöma om det är en upprepat transaktion.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-408">To achieve **exactly once** semantics, the commit bolt **count-sum** need to judge whether it is a replayed transaction.</span></span> <span data-ttu-id="0d5a1-409">I det här exemplet har den en statisk medlemsvariabel:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-409">In this example, it has a static member variable:</span></span>

    public static long lastCommittedTxId = -1; 

<span data-ttu-id="0d5a1-410">När en instans av ISCPBatchBolt skapas, får den `txAttempt` från indataparametrar:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-410">When an ISCPBatchBolt instance is created, it will get the `txAttempt` from input parameters:</span></span>

    public static CountSum Get(Context ctx, Dictionary<string, Object> parms)
    {
        /* for transactional topology, we can get txAttempt from the input parms */
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

<span data-ttu-id="0d5a1-411">När `FinishBatch()` anropas, den `lastCommittedTxId` kommer att uppdateringen om den inte är en upprepat transaktion.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-411">When `FinishBatch()` is called, the `lastCommittedTxId` will be update if it is not a replayed transaction.</span></span>

    public void FinishBatch(Dictionary<string, Object> parms)
    {
        /* judge whether it is a replayed transaction? */
        bool replay = (this.txAttempt.TxId <= lastCommittedTxId);

        if (!replay)
        {
            /* If it is not replayed, update the toalCount and lastCommittedTxId vaule */
            totalCount = totalCount + this.count;
            lastCommittedTxId = this.txAttempt.TxId;
        }
        … …
    }


### <a name="hybridtopology"></a><span data-ttu-id="0d5a1-412">HybridTopology</span><span class="sxs-lookup"><span data-stu-id="0d5a1-412">HybridTopology</span></span>
<span data-ttu-id="0d5a1-413">Den här topologin innehåller en Java-kanalen och en C\# bulten.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-413">This topology contains a Java Spout and a C\# Bolt.</span></span> <span data-ttu-id="0d5a1-414">Den använder serialisering och deserialisering standardimplementering tillhandahålls av SCP-plattformen.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-414">It uses the default serialization and deserialization implementation provided by SCP platform.</span></span> <span data-ttu-id="0d5a1-415">Ange ref den **HybridTopology.spec** i **exempel\\HybridTopology** mapp för spec Filinformation och **SubmitTopology.bat** för hur du anger klassökvägen Java.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-415">Please ref the **HybridTopology.spec** in **examples\\HybridTopology** folder for the spec file details, and **SubmitTopology.bat** for how to specify Java classpath.</span></span>

### <a name="scphostdemo"></a><span data-ttu-id="0d5a1-416">SCPHostDemo</span><span class="sxs-lookup"><span data-stu-id="0d5a1-416">SCPHostDemo</span></span>
<span data-ttu-id="0d5a1-417">Det här exemplet är samma som HelloWorld i princip.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-417">This example is the same as HelloWorld in essence.</span></span> <span data-ttu-id="0d5a1-418">Den enda skillnaden är att användarkod kompileras som DLL-filen och topologin skickas med hjälp av SCPHost.exe.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-418">The only difference is that the user code is compiled as DLL and the topology is submitted by using SCPHost.exe.</span></span> <span data-ttu-id="0d5a1-419">Ta ref i avsnittet ”SCP värden läget” mer detaljerad förklaring.</span><span class="sxs-lookup"><span data-stu-id="0d5a1-419">Please ref the section "SCP Host Mode" for more detailed explanation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d5a1-420">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0d5a1-420">Next Steps</span></span>
<span data-ttu-id="0d5a1-421">Exempel på Storm-topologier som skapats med SCP finns i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="0d5a1-421">For examples of Storm topologies created using SCP, see the following:</span></span>

* [<span data-ttu-id="0d5a1-422">Utveckla C#-topologier för Apache Storm på HDInsight med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d5a1-422">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="0d5a1-423">Bearbeta händelser från Azure Event Hubs med Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="0d5a1-423">Process events from Azure Event Hubs with Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-event-hub-topology.md)
* [<span data-ttu-id="0d5a1-424">Skapa flera dataströmmar i en C# Storm-topologi</span><span class="sxs-lookup"><span data-stu-id="0d5a1-424">Create multiple data streams in a C# Storm topology</span></span>](hdinsight-storm-twitter-trending.md)
* [<span data-ttu-id="0d5a1-425">Använd Power Bi för att visualisera data från en Storm-topologi</span><span class="sxs-lookup"><span data-stu-id="0d5a1-425">Use Power Bi to visualize data from a Storm topology</span></span>](hdinsight-storm-power-bi-topology.md)
* [<span data-ttu-id="0d5a1-426">Bearbeta vehicle sensordata från Händelsehubbar med Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="0d5a1-426">Process vehicle sensor data from Event Hubs using Storm on HDInsight</span></span>](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [<span data-ttu-id="0d5a1-427">Extrahera, transformering och laddning (ETL) från Azure Event Hubs till HBase</span><span class="sxs-lookup"><span data-stu-id="0d5a1-427">Extract, Transform, and Load (ETL) from Azure Event Hubs to HBase</span></span>](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
* [<span data-ttu-id="0d5a1-428">Samordna händelser med Storm och HBase på HDInsight</span><span class="sxs-lookup"><span data-stu-id="0d5a1-428">Correlate events using Storm and HBase on HDInsight</span></span>](hdinsight-storm-correlation-topology.md)

