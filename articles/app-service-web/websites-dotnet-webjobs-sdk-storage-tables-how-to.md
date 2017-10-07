---
title: aaaHow toouse Azure table storage med hello WebJobs SDK
description: "Lär dig hur toouse Azure table storage med hello WebJobs-SDK. Skapa tabeller, lägga till entiteter tootables och läsa befintliga tabeller."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 451432cc-c780-4310-85d3-84f44fe48afe
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 8e28c69df4a934646add9e50c6de28e76dca1636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-with-hello-webjobs-sdk"></a><span data-ttu-id="bf488-104">Hur toouse Azure table storage med hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="bf488-104">How toouse Azure table storage with hello WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="bf488-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="bf488-105">Overview</span></span>
<span data-ttu-id="bf488-106">Den här guiden innehåller C#-kodexempel som visar hur tooread och skriva Azure storage-tabeller med hjälp av [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span><span class="sxs-lookup"><span data-stu-id="bf488-106">This guide provides C# code samples that show how tooread and write Azure storage tables by using [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="bf488-107">hello handboken förutsätts att du vet [hur toocreate ett Webbjobb projekt i Visual Studio med anslutning strängar som punkt tooyour lagringskonto](websites-dotnet-webjobs-sdk-get-started.md) eller för[flera lagringskonton](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="bf488-107">hello guide assumes you know [how toocreate a WebJob project in Visual Studio with connection strings that point tooyour storage account](websites-dotnet-webjobs-sdk-get-started.md) or too[multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

<span data-ttu-id="bf488-108">Vissa hello kodstycken som visar hello `Table` attribut som används i funktioner som är [kallas manuellt](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), det vill säga inte med någon av hello utlösaren attribut.</span><span class="sxs-lookup"><span data-stu-id="bf488-108">Some of hello code snippets show hello `Table` attribute used in functions that are [called manually](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), that is, not by using one of hello trigger attributes.</span></span> 

## <span data-ttu-id="bf488-109"><a id="ingress"></a>Hur tooadd entiteter tooa tabell</span><span class="sxs-lookup"><span data-stu-id="bf488-109"><a id="ingress"></a> How tooadd entities tooa table</span></span>
<span data-ttu-id="bf488-110">tooadd entiteter tooa tabell, Använd hello `Table` attribut med ett `ICollector<T>` eller `IAsyncCollector<T>` parametern där `T` anger hello schemat hello entiteter som du vill tooadd.</span><span class="sxs-lookup"><span data-stu-id="bf488-110">tooadd entities tooa table, use hello `Table` attribute with an `ICollector<T>` or `IAsyncCollector<T>` parameter where `T` specifies hello schema of hello entities you want tooadd.</span></span> <span data-ttu-id="bf488-111">hello Attributkonstruktorn använder en strängparameter som anger hello namnet på hello tabell.</span><span class="sxs-lookup"><span data-stu-id="bf488-111">hello attribute constructor takes a string parameter that specifies hello name of hello table.</span></span> 

<span data-ttu-id="bf488-112">hello följande kodexempel lägger till `Person` entiteter tooa tabell med namnet *ingång*.</span><span class="sxs-lookup"><span data-stu-id="bf488-112">hello following code sample adds `Person` entities tooa table named *Ingress*.</span></span>

        [NoAutomaticTrigger]
        public static void IngressDemo(
            [Table("Ingress")] ICollector<Person> tableBinding)
        {
            for (int i = 0; i < 100000; i++)
            {
                tableBinding.Add(
                    new Person() { 
                        PartitionKey = "Test", 
                        RowKey = i.ToString(), 
                        Name = "Name" }
                    );
            }
        }

<span data-ttu-id="bf488-113">Vanligtvis hello typ som du använder med `ICollector` härleds från `TableEntity` eller implementerar `ITableEntity`, men det behöver inte.</span><span class="sxs-lookup"><span data-stu-id="bf488-113">Typically hello type you use with `ICollector` derives from `TableEntity` or implements `ITableEntity`, but it doesn't have to.</span></span> <span data-ttu-id="bf488-114">Något av följande hello `Person` klasserna arbete med hello koden som visas i föregående hello `Ingress` metod.</span><span class="sxs-lookup"><span data-stu-id="bf488-114">Either of hello following `Person` classes work with hello code shown in hello preceding `Ingress` method.</span></span>

        public class Person : TableEntity
        {
            public string Name { get; set; }
        }

        public class Person
        {
            public string PartitionKey { get; set; }
            public string RowKey { get; set; }
            public string Name { get; set; }
        }

<span data-ttu-id="bf488-115">Om du vill toowork direkt med hello Azure storage-API, kan du lägga till en `CloudStorageAccount` Metodsignaturen för parametern toohello.</span><span class="sxs-lookup"><span data-stu-id="bf488-115">If you want toowork directly with hello Azure storage API, you can add a `CloudStorageAccount` parameter toohello method signature.</span></span>

## <span data-ttu-id="bf488-116"><a id="monitor"></a>Realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="bf488-116"><a id="monitor"></a> Real-time monitoring</span></span>
<span data-ttu-id="bf488-117">Eftersom dataåtkomstfunktioner ofta bearbetar stora mängder data, innehåller hello WebJobs SDK instrumentpanelen övervakning realtidsdata.</span><span class="sxs-lookup"><span data-stu-id="bf488-117">Because data ingress functions often process large volumes of data, hello WebJobs SDK dashboard provides real-time monitoring data.</span></span> <span data-ttu-id="bf488-118">Hej **anrop loggen** avsnittet berättar om hello funktionen fortfarande körs.</span><span class="sxs-lookup"><span data-stu-id="bf488-118">hello **Invocation Log** section tells you if hello function is still running.</span></span>

![Ingång funktionen körs](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressrunning.png)

<span data-ttu-id="bf488-120">Hej **anrop information** sidan rapporterar hello funktionens förlopp (antal entiteter som skrivs) medan den körs och ger dig en möjlighet tooabort den.</span><span class="sxs-lookup"><span data-stu-id="bf488-120">hello **Invocation Details** page reports hello function's progress (number of entities written) while it's running and gives you an opportunity tooabort it.</span></span> 

![Ingång funktionen körs](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressprogress.png)

<span data-ttu-id="bf488-122">När funktionen hello är klar, hello **anrop information** sidan rapporterar hello antalet rader som har skrivits.</span><span class="sxs-lookup"><span data-stu-id="bf488-122">When hello function finishes, hello **Invocation Details** page reports hello number of rows written.</span></span>

![Ingång funktionen klar](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingresssuccess.png)

## <span data-ttu-id="bf488-124"><a id="multiple"></a>Hur tooread flera enheter från en tabell</span><span class="sxs-lookup"><span data-stu-id="bf488-124"><a id="multiple"></a> How tooread multiple entities from a table</span></span>
<span data-ttu-id="bf488-125">tooread en tabell, använder hello `Table` attribut med ett `IQueryable<T>` parametern skriver `T` härleds från `TableEntity` eller implementerar `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="bf488-125">tooread a table, use hello `Table` attribute with an `IQueryable<T>` parameter where type `T` derives from `TableEntity` or implements `ITableEntity`.</span></span>

<span data-ttu-id="bf488-126">hello följande kodexempel läser och loggar alla rader från hello `Ingress` tabell:</span><span class="sxs-lookup"><span data-stu-id="bf488-126">hello following code sample reads and logs all rows from hello `Ingress` table:</span></span>

        public static void ReadTable(
            [Table("Ingress")] IQueryable<Person> tableBinding,
            TextWriter logger)
        {
            var query = from p in tableBinding select p;
            foreach (Person person in query)
            {
                logger.WriteLine("PK:{0}, RK:{1}, Name:{2}", 
                    person.PartitionKey, person.RowKey, person.Name);
            }
        }

### <span data-ttu-id="bf488-127"><a id="readone"></a>Hur tooread en enda entitet från en tabell</span><span class="sxs-lookup"><span data-stu-id="bf488-127"><a id="readone"></a> How tooread a single entity from a table</span></span>
<span data-ttu-id="bf488-128">Det finns en `Table` Attributkonstruktorn med två ytterligare parametrar som kan du ange hello partitionsnyckel och radnyckel när du vill toobind tooa tabell entitet.</span><span class="sxs-lookup"><span data-stu-id="bf488-128">There is a `Table` attribute constructor with two additional parameters that let you specify hello partition key and row key when you want toobind tooa single table entity.</span></span>

<span data-ttu-id="bf488-129">hello följande kodexempel läser en tabellrad för en `Person` entitet baserat på partitionen nyckel- och nyckelvärden togs emot i ett kömeddelande:</span><span class="sxs-lookup"><span data-stu-id="bf488-129">hello following code sample reads a table row for a `Person` entity based on partition key and row key values received in a queue message:</span></span>  

        public static void ReadTableEntity(
            [QueueTrigger("inputqueue")] Person personInQueue,
            [Table("persontable","{PartitionKey}", "{RowKey}")] Person personInTable,
            TextWriter logger)
        {
            if (personInTable == null)
            {
                logger.WriteLine("Person not found: PK:{0}, RK:{1}",
                        personInQueue.PartitionKey, personInQueue.RowKey);
            }
            else
            {
                logger.WriteLine("Person found: PK:{0}, RK:{1}, Name:{2}",
                        personInTable.PartitionKey, personInTable.RowKey, personInTable.Name);
            }
        }


<span data-ttu-id="bf488-130">Hej `Person` klassen i det här exemplet har inte tooimplement `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="bf488-130">hello `Person` class in this example does not have tooimplement `ITableEntity`.</span></span>

## <span data-ttu-id="bf488-131"><a id="storageapi"></a>Hur toouse hello .NET Storage API direkt toowork med en tabell</span><span class="sxs-lookup"><span data-stu-id="bf488-131"><a id="storageapi"></a> How toouse hello .NET Storage API directly toowork with a table</span></span>
<span data-ttu-id="bf488-132">Du kan också använda hello `Table` attribut med ett `CloudTable` objekt för större flexibilitet när du arbetar med en tabell.</span><span class="sxs-lookup"><span data-stu-id="bf488-132">You can also use hello `Table` attribute with a `CloudTable` object for more flexibility in working with a table.</span></span>

<span data-ttu-id="bf488-133">hello följande kod exempel används en `CloudTable` objekt tooadd en enda entitet toohello *ingång* tabell.</span><span class="sxs-lookup"><span data-stu-id="bf488-133">hello following code sample uses a `CloudTable` object tooadd a single entity toohello *Ingress* table.</span></span> 

        public static void UseStorageAPI(
            [Table("Ingress")] CloudTable tableBinding,
            TextWriter logger)
        {
            var person = new Person()
                {
                    PartitionKey = "Test",
                    RowKey = "100",
                    Name = "Name"
                };
            TableOperation insertOperation = TableOperation.Insert(person);
            tableBinding.Execute(insertOperation);
        }

<span data-ttu-id="bf488-134">Mer information om hur toouse hello `CloudTable` objekt, se [hur toouse Table Storage från .NET](../cosmos-db/table-storage-how-to-use-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="bf488-134">For more information about how toouse hello `CloudTable` object, see [How toouse Table Storage from .NET](../cosmos-db/table-storage-how-to-use-dotnet.md).</span></span> 

## <span data-ttu-id="bf488-135"><a id="queues"></a>Närliggande ämnen som omfattas av hello köer hur-tooarticle</span><span class="sxs-lookup"><span data-stu-id="bf488-135"><a id="queues"></a>Related topics covered by hello queues how-tooarticle</span></span>
<span data-ttu-id="bf488-136">Information om hur toohandle tabell bearbetning utlöses av ett kömeddelande eller för WebJobs SDK scenarier inte specifika tootable bearbetning, se [hur toouse Azure kö lagring med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="bf488-136">For information about how toohandle table processing triggered by a queue message, or for WebJobs SDK scenarios not specific tootable processing, see [How toouse Azure queue storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="bf488-137">Avsnitt som beskrivs i artikeln inkludera hello följande:</span><span class="sxs-lookup"><span data-stu-id="bf488-137">Topics covered in that article include hello following:</span></span>

* <span data-ttu-id="bf488-138">Async-funktion</span><span class="sxs-lookup"><span data-stu-id="bf488-138">Async functions</span></span>
* <span data-ttu-id="bf488-139">Flera instanser</span><span class="sxs-lookup"><span data-stu-id="bf488-139">Multiple instances</span></span>
* <span data-ttu-id="bf488-140">Korrekt avslutning</span><span class="sxs-lookup"><span data-stu-id="bf488-140">Graceful shutdown</span></span>
* <span data-ttu-id="bf488-141">Använda WebJobs-SDK-attribut i hello brödtext</span><span class="sxs-lookup"><span data-stu-id="bf488-141">Use WebJobs SDK attributes in hello body of a function</span></span>
* <span data-ttu-id="bf488-142">Ange hello SDK-anslutningssträngar i kod</span><span class="sxs-lookup"><span data-stu-id="bf488-142">Set hello SDK connection strings in code</span></span>
* <span data-ttu-id="bf488-143">Ange värden för WebJobs SDK konstruktorparametrarna i koden</span><span class="sxs-lookup"><span data-stu-id="bf488-143">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="bf488-144">Utlös en funktion manuellt</span><span class="sxs-lookup"><span data-stu-id="bf488-144">Trigger a function manually</span></span>
* <span data-ttu-id="bf488-145">Skriva loggar</span><span class="sxs-lookup"><span data-stu-id="bf488-145">Write logs</span></span>

## <span data-ttu-id="bf488-146"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bf488-146"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="bf488-147">Den här guiden har angett koden exempel som visar hur toohandle vanliga scenarier för att arbeta med Azure-tabeller.</span><span class="sxs-lookup"><span data-stu-id="bf488-147">This guide has provided code samples that show how toohandle common scenarios for working with Azure tables.</span></span> <span data-ttu-id="bf488-148">Mer information om hur toouse Azure WebJobs och hello WebJobs SDK, se [Azure WebJobs rekommenderas resurser](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="bf488-148">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

