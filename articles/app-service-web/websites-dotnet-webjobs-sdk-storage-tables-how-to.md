---
title: "Använd Azure-tabellagring med WebJobs SDK:n"
description: "Lär dig mer om att använda Azure table storage med WebJobs SDK. Skapa tabeller, lägga till enheter i tabeller och läsa befintliga tabeller."
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
ms.openlocfilehash: 13cfc788c14d714df7022ce003d34691cf73d121
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-table-storage-with-the-webjobs-sdk"></a><span data-ttu-id="8fc94-104">Använd Azure-tabellagring med WebJobs SDK:n</span><span class="sxs-lookup"><span data-stu-id="8fc94-104">How to use Azure table storage with the WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="8fc94-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="8fc94-105">Overview</span></span>
<span data-ttu-id="8fc94-106">Den här guiden innehåller C#-kodexempel som visar hur du läser och skriver Azure storage-tabeller med hjälp av [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span><span class="sxs-lookup"><span data-stu-id="8fc94-106">This guide provides C# code samples that show how to read and write Azure storage tables by using [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="8fc94-107">Handboken förutsätter att du vet [hur du skapar ett Webbjobb-projekt i Visual Studio med anslutning strängar som pekar på ditt lagringskonto](websites-dotnet-webjobs-sdk-get-started.md) eller [flera lagringskonton](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="8fc94-107">The guide assumes you know [how to create a WebJob project in Visual Studio with connection strings that point to your storage account](websites-dotnet-webjobs-sdk-get-started.md) or to [multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

<span data-ttu-id="8fc94-108">Vissa av koden kodavsnitt visas den `Table` attribut som används i funktioner som är [kallas manuellt](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), det vill säga inte genom att använda ett av attributen för utlösare.</span><span class="sxs-lookup"><span data-stu-id="8fc94-108">Some of the code snippets show the `Table` attribute used in functions that are [called manually](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), that is, not by using one of the trigger attributes.</span></span> 

## <span data-ttu-id="8fc94-109"><a id="ingress"></a>Lägga till entiteter i en tabell</span><span class="sxs-lookup"><span data-stu-id="8fc94-109"><a id="ingress"></a> How to add entities to a table</span></span>
<span data-ttu-id="8fc94-110">Om du vill lägga till enheter i en tabell, använder den `Table` attribut med ett `ICollector<T>` eller `IAsyncCollector<T>` parametern där `T` anger schemat för de enheter som du vill lägga till.</span><span class="sxs-lookup"><span data-stu-id="8fc94-110">To add entities to a table, use the `Table` attribute with an `ICollector<T>` or `IAsyncCollector<T>` parameter where `T` specifies the schema of the entities you want to add.</span></span> <span data-ttu-id="8fc94-111">Attributkonstruktorn använder en strängparameter som anger namnet på tabellen.</span><span class="sxs-lookup"><span data-stu-id="8fc94-111">The attribute constructor takes a string parameter that specifies the name of the table.</span></span> 

<span data-ttu-id="8fc94-112">Följande kodexempel lägger till `Person` enheter till en tabell med namnet *ingång*.</span><span class="sxs-lookup"><span data-stu-id="8fc94-112">The following code sample adds `Person` entities to a table named *Ingress*.</span></span>

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

<span data-ttu-id="8fc94-113">Typen du använder vanligtvis med `ICollector` härleds från `TableEntity` eller implementerar `ITableEntity`, men det behöver inte.</span><span class="sxs-lookup"><span data-stu-id="8fc94-113">Typically the type you use with `ICollector` derives from `TableEntity` or implements `ITableEntity`, but it doesn't have to.</span></span> <span data-ttu-id="8fc94-114">Något av följande `Person` klasserna arbete med koden som visas i den föregående `Ingress` metod.</span><span class="sxs-lookup"><span data-stu-id="8fc94-114">Either of the following `Person` classes work with the code shown in the preceding `Ingress` method.</span></span>

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

<span data-ttu-id="8fc94-115">Om du vill arbeta direkt med Azure storage API, kan du lägga till en `CloudStorageAccount` parameter till Metodsignaturen.</span><span class="sxs-lookup"><span data-stu-id="8fc94-115">If you want to work directly with the Azure storage API, you can add a `CloudStorageAccount` parameter to the method signature.</span></span>

## <span data-ttu-id="8fc94-116"><a id="monitor"></a>Realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="8fc94-116"><a id="monitor"></a> Real-time monitoring</span></span>
<span data-ttu-id="8fc94-117">Eftersom dataåtkomstfunktioner ofta bearbetar stora mängder data, innehåller WebJobs SDK instrumentpanelen övervakning realtidsdata.</span><span class="sxs-lookup"><span data-stu-id="8fc94-117">Because data ingress functions often process large volumes of data, the WebJobs SDK dashboard provides real-time monitoring data.</span></span> <span data-ttu-id="8fc94-118">Den **anrop loggen** avsnittet berättar om funktionen fortfarande körs.</span><span class="sxs-lookup"><span data-stu-id="8fc94-118">The **Invocation Log** section tells you if the function is still running.</span></span>

![Ingång funktionen körs](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressrunning.png)

<span data-ttu-id="8fc94-120">Den **anrop information** sidan rapporterar funktionens status (antal entiteter som skrivs) medan den körs och ger dig en möjlighet att avbryta den.</span><span class="sxs-lookup"><span data-stu-id="8fc94-120">The **Invocation Details** page reports the function's progress (number of entities written) while it's running and gives you an opportunity to abort it.</span></span> 

![Ingång funktionen körs](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressprogress.png)

<span data-ttu-id="8fc94-122">När funktionen har slutförts i **anrop information** sidan rapporterar antalet rader som har skrivits.</span><span class="sxs-lookup"><span data-stu-id="8fc94-122">When the function finishes, the **Invocation Details** page reports the number of rows written.</span></span>

![Ingång funktionen klar](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingresssuccess.png)

## <span data-ttu-id="8fc94-124"><a id="multiple"></a>Läsa flera enheter från en tabell</span><span class="sxs-lookup"><span data-stu-id="8fc94-124"><a id="multiple"></a> How to read multiple entities from a table</span></span>
<span data-ttu-id="8fc94-125">Läs en tabell genom att använda den `Table` attribut med ett `IQueryable<T>` parametern skriver `T` härleds från `TableEntity` eller implementerar `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="8fc94-125">To read a table, use the `Table` attribute with an `IQueryable<T>` parameter where type `T` derives from `TableEntity` or implements `ITableEntity`.</span></span>

<span data-ttu-id="8fc94-126">Följande kodexempel läser och loggar alla rader från den `Ingress` tabellen:</span><span class="sxs-lookup"><span data-stu-id="8fc94-126">The following code sample reads and logs all rows from the `Ingress` table:</span></span>

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

### <span data-ttu-id="8fc94-127"><a id="readone"></a>Läsa en enda entitet från en tabell</span><span class="sxs-lookup"><span data-stu-id="8fc94-127"><a id="readone"></a> How to read a single entity from a table</span></span>
<span data-ttu-id="8fc94-128">Det finns en `Table` Attributkonstruktorn med två ytterligare parametrar som kan du ange partitionsnyckel och radnyckel när du vill binda till en enskild tabell entitet.</span><span class="sxs-lookup"><span data-stu-id="8fc94-128">There is a `Table` attribute constructor with two additional parameters that let you specify the partition key and row key when you want to bind to a single table entity.</span></span>

<span data-ttu-id="8fc94-129">Följande kodexempel läser en tabellrad för en `Person` entitet baserat på partitionen nyckel- och nyckelvärden togs emot i ett kömeddelande:</span><span class="sxs-lookup"><span data-stu-id="8fc94-129">The following code sample reads a table row for a `Person` entity based on partition key and row key values received in a queue message:</span></span>  

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


<span data-ttu-id="8fc94-130">Den `Person` klassen i det här exemplet behöver inte implementera `ITableEntity`.</span><span class="sxs-lookup"><span data-stu-id="8fc94-130">The `Person` class in this example does not have to implement `ITableEntity`.</span></span>

## <span data-ttu-id="8fc94-131"><a id="storageapi"></a>Hur du använder .NET Storage API direkt för att arbeta med en tabell</span><span class="sxs-lookup"><span data-stu-id="8fc94-131"><a id="storageapi"></a> How to use the .NET Storage API directly to work with a table</span></span>
<span data-ttu-id="8fc94-132">Du kan också använda den `Table` attribut med ett `CloudTable` objekt för större flexibilitet när du arbetar med en tabell.</span><span class="sxs-lookup"><span data-stu-id="8fc94-132">You can also use the `Table` attribute with a `CloudTable` object for more flexibility in working with a table.</span></span>

<span data-ttu-id="8fc94-133">I följande kod exempel används en `CloudTable` objekt att lägga till en enda entitet till den *ingång* tabell.</span><span class="sxs-lookup"><span data-stu-id="8fc94-133">The following code sample uses a `CloudTable` object to add a single entity to the *Ingress* table.</span></span> 

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

<span data-ttu-id="8fc94-134">Mer information om hur du använder den `CloudTable` objekt, se [använda Table Storage från .NET](../cosmos-db/table-storage-how-to-use-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="8fc94-134">For more information about how to use the `CloudTable` object, see [How to use Table Storage from .NET](../cosmos-db/table-storage-how-to-use-dotnet.md).</span></span> 

## <span data-ttu-id="8fc94-135"><a id="queues"></a>Närliggande ämnen som omfattas av den här artikeln köer</span><span class="sxs-lookup"><span data-stu-id="8fc94-135"><a id="queues"></a>Related topics covered by the queues how-to article</span></span>
<span data-ttu-id="8fc94-136">Information om hur du hanterar tabell bearbetning som utlöses av ett kömeddelande eller WebJobs-SDK-scenarier som inte är specifika för bearbetning av tabellen finns [använda Azure queue storage med WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="8fc94-136">For information about how to handle table processing triggered by a queue message, or for WebJobs SDK scenarios not specific to table processing, see [How to use Azure queue storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="8fc94-137">Följande: avsnitt beskrivs i artikeln</span><span class="sxs-lookup"><span data-stu-id="8fc94-137">Topics covered in that article include the following:</span></span>

* <span data-ttu-id="8fc94-138">Async-funktion</span><span class="sxs-lookup"><span data-stu-id="8fc94-138">Async functions</span></span>
* <span data-ttu-id="8fc94-139">Flera instanser</span><span class="sxs-lookup"><span data-stu-id="8fc94-139">Multiple instances</span></span>
* <span data-ttu-id="8fc94-140">Korrekt avslutning</span><span class="sxs-lookup"><span data-stu-id="8fc94-140">Graceful shutdown</span></span>
* <span data-ttu-id="8fc94-141">Använda WebJobs-SDK-attribut i brödtexten för en funktion</span><span class="sxs-lookup"><span data-stu-id="8fc94-141">Use WebJobs SDK attributes in the body of a function</span></span>
* <span data-ttu-id="8fc94-142">Ange anslutningssträngar SDK i kod</span><span class="sxs-lookup"><span data-stu-id="8fc94-142">Set the SDK connection strings in code</span></span>
* <span data-ttu-id="8fc94-143">Ange värden för WebJobs SDK konstruktorparametrarna i koden</span><span class="sxs-lookup"><span data-stu-id="8fc94-143">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="8fc94-144">Utlös en funktion manuellt</span><span class="sxs-lookup"><span data-stu-id="8fc94-144">Trigger a function manually</span></span>
* <span data-ttu-id="8fc94-145">Skriva loggar</span><span class="sxs-lookup"><span data-stu-id="8fc94-145">Write logs</span></span>

## <span data-ttu-id="8fc94-146"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8fc94-146"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="8fc94-147">Den här guiden tillhandahåller kodexempel som visar hur du hanterar vanliga scenarier för att arbeta med Azure-tabeller.</span><span class="sxs-lookup"><span data-stu-id="8fc94-147">This guide has provided code samples that show how to handle common scenarios for working with Azure tables.</span></span> <span data-ttu-id="8fc94-148">Mer information om hur du använder Azure WebJobs och WebJobs-SDK finns [Azure WebJobs rekommenderas resurser](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="8fc94-148">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

