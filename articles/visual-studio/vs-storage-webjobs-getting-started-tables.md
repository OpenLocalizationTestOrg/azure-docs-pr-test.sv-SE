---
title: "aaaGetting igång med Azure storage och Visual Studio anslutna tjänster (Webbjobb projekt)"
description: "Hur tooget igång med Azure Table storage i ett Azure WebJobs-projekt i Visual Studio när ansluter tooa lagringskonto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 061a6c46-0592-4e5d-aced-ab7498481cde
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 80d9f8d8b493ce612623dfed7e89325fb154a1c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-storage-azure-webjob-projects"></a><span data-ttu-id="c0322-103">Komma igång med Azure Storage (Azure Webjobs-projekt)</span><span class="sxs-lookup"><span data-stu-id="c0322-103">Getting Started with Azure Storage (Azure WebJob Projects)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="c0322-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c0322-104">Overview</span></span>
<span data-ttu-id="c0322-105">Den här artikeln innehåller C#-kodexempel som visar visa hur toouse hello Azure WebJobs SDK version 1.x med hello Azure table storage-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c0322-105">This article provides C# code samples that show show how toouse hello Azure WebJobs SDK version 1.x with hello Azure table storage service.</span></span> <span data-ttu-id="c0322-106">hello kodexempel använda hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span><span class="sxs-lookup"><span data-stu-id="c0322-106">hello code samples use hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="c0322-107">hello Azure Table storage-tjänst kan du toostore stora mängder strukturerade data.</span><span class="sxs-lookup"><span data-stu-id="c0322-107">hello Azure Table storage service enables you toostore large amounts of structured data.</span></span> <span data-ttu-id="c0322-108">hello-tjänsten är en NoSQL-datalager som tar emot autentiserade anrop inuti och utanför hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="c0322-108">hello service is a NoSQL datastore that accepts authenticated calls from inside and outside hello Azure cloud.</span></span> <span data-ttu-id="c0322-109">Azure-tabeller passar utmärkt för att lagra strukturerade, icke-relationella data.</span><span class="sxs-lookup"><span data-stu-id="c0322-109">Azure tables are ideal for storing structured, non-relational data.</span></span>  <span data-ttu-id="c0322-110">Se [komma igång med Azure Table storage med hjälp av .NET](../cosmos-db/table-storage-how-to-use-dotnet.md#create-a-table) för mer information.</span><span class="sxs-lookup"><span data-stu-id="c0322-110">See [Get started with Azure Table storage using .NET](../cosmos-db/table-storage-how-to-use-dotnet.md#create-a-table) for more information.</span></span>

<span data-ttu-id="c0322-111">Vissa hello kodstycken som visar hello **tabell** attribut som används i funktioner som kallas manuellt, det vill säga inte med någon av hello utlösaren attribut.</span><span class="sxs-lookup"><span data-stu-id="c0322-111">Some of hello code snippets show hello **Table** attribute used in functions that are called manually, that is, not by using one of hello trigger attributes.</span></span>

## <a name="how-tooadd-entities-tooa-table"></a><span data-ttu-id="c0322-112">Hur tooadd entiteter tooa tabell</span><span class="sxs-lookup"><span data-stu-id="c0322-112">How tooadd entities tooa table</span></span>
<span data-ttu-id="c0322-113">tooadd entiteter tooa tabell, Använd hello **tabell** attribut med ett **ICollector<T> ** eller **IAsyncCollector<T> ** parametern där **T** anger hello schemat hello entiteter som du vill tooadd.</span><span class="sxs-lookup"><span data-stu-id="c0322-113">tooadd entities tooa table, use hello **Table** attribute with an **ICollector<T>** or **IAsyncCollector<T>** parameter where **T** specifies hello schema of hello entities you want tooadd.</span></span> <span data-ttu-id="c0322-114">hello Attributkonstruktorn använder en strängparameter som anger hello namnet på hello tabell.</span><span class="sxs-lookup"><span data-stu-id="c0322-114">hello attribute constructor takes a string parameter that specifies hello name of hello table.</span></span>

<span data-ttu-id="c0322-115">hello följande kodexempel lägger till **Person** entiteter tooa tabell med namnet *ingång*.</span><span class="sxs-lookup"><span data-stu-id="c0322-115">hello following code sample adds **Person** entities tooa table named *Ingress*.</span></span>

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

<span data-ttu-id="c0322-116">Vanligtvis hello typ som du använder med **ICollector** härleds från **TableEntity** eller implementerar **ITableEntity**, men det behöver inte.</span><span class="sxs-lookup"><span data-stu-id="c0322-116">Typically hello type you use with **ICollector** derives from **TableEntity** or implements **ITableEntity**, but it doesn't have to.</span></span> <span data-ttu-id="c0322-117">Något av följande hello **Person** klasserna arbete med hello koden som visas i föregående hello **ingång** metod.</span><span class="sxs-lookup"><span data-stu-id="c0322-117">Either of hello following **Person** classes work with hello code shown in hello preceding **Ingress** method.</span></span>

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

<span data-ttu-id="c0322-118">Om du vill toowork direkt med hello Azure storage-API, kan du lägga till en **CloudStorageAccount** Metodsignaturen för parametern toohello.</span><span class="sxs-lookup"><span data-stu-id="c0322-118">If you want toowork directly with hello Azure storage API, you can add a **CloudStorageAccount** parameter toohello method signature.</span></span>

## <a name="real-time-monitoring"></a><span data-ttu-id="c0322-119">Realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="c0322-119">Real-time monitoring</span></span>
<span data-ttu-id="c0322-120">Eftersom dataåtkomstfunktioner ofta bearbetar stora mängder data, innehåller hello WebJobs SDK instrumentpanelen övervakning realtidsdata.</span><span class="sxs-lookup"><span data-stu-id="c0322-120">Because data ingress functions often process large volumes of data, hello WebJobs SDK dashboard provides real-time monitoring data.</span></span> <span data-ttu-id="c0322-121">Hej **anrop loggen** avsnittet berättar om hello funktionen fortfarande körs.</span><span class="sxs-lookup"><span data-stu-id="c0322-121">hello **Invocation Log** section tells you if hello function is still running.</span></span>

![Ingång funktionen körs](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

<span data-ttu-id="c0322-123">Hej **anrop information** sidan rapporterar hello funktionens förlopp (antal entiteter som skrivs) medan den körs och ger dig en möjlighet tooabort den.</span><span class="sxs-lookup"><span data-stu-id="c0322-123">hello **Invocation Details** page reports hello function's progress (number of entities written) while it's running and gives you an opportunity tooabort it.</span></span>

![Ingång funktionen körs](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

<span data-ttu-id="c0322-125">När funktionen hello är klar, hello **anrop information** sidan rapporterar hello antalet rader som har skrivits.</span><span class="sxs-lookup"><span data-stu-id="c0322-125">When hello function finishes, hello **Invocation Details** page reports hello number of rows written.</span></span>

![Ingång funktionen klar](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## <a name="how-tooread-multiple-entities-from-a-table"></a><span data-ttu-id="c0322-127">Hur tooread flera enheter från en tabell</span><span class="sxs-lookup"><span data-stu-id="c0322-127">How tooread multiple entities from a table</span></span>
<span data-ttu-id="c0322-128">tooread en tabell, använder hello **tabell** attribut med ett **IQueryable<T> ** parametern skriver **T** härleds från **TableEntity**eller implementerar **ITableEntity**.</span><span class="sxs-lookup"><span data-stu-id="c0322-128">tooread a table, use hello **Table** attribute with an **IQueryable<T>** parameter where type **T** derives from **TableEntity** or implements **ITableEntity**.</span></span>

<span data-ttu-id="c0322-129">hello följande kodexempel läser och loggar alla rader från hello **ingång** tabell:</span><span class="sxs-lookup"><span data-stu-id="c0322-129">hello following code sample reads and logs all rows from hello **Ingress** table:</span></span>

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

### <a name="how-tooread-a-single-entity-from-a-table"></a><span data-ttu-id="c0322-130">Hur tooread en enda entitet från en tabell</span><span class="sxs-lookup"><span data-stu-id="c0322-130">How tooread a single entity from a table</span></span>
<span data-ttu-id="c0322-131">Det finns en **tabell** Attributkonstruktorn med två ytterligare parametrar som kan du ange hello partitionsnyckel och radnyckel när du vill toobind tooa tabell entitet.</span><span class="sxs-lookup"><span data-stu-id="c0322-131">There is a **Table** attribute constructor with two additional parameters that let you specify hello partition key and row key when you want toobind tooa single table entity.</span></span>

<span data-ttu-id="c0322-132">hello följande kodexempel läser en tabellrad för en **Person** entitet baserat på partitionen nyckel- och nyckelvärden togs emot i ett kömeddelande:</span><span class="sxs-lookup"><span data-stu-id="c0322-132">hello following code sample reads a table row for a **Person** entity based on partition key and row key values received in a queue message:</span></span>  

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


<span data-ttu-id="c0322-133">Hej **Person** klassen i det här exemplet har inte tooimplement **ITableEntity**.</span><span class="sxs-lookup"><span data-stu-id="c0322-133">hello **Person** class in this example does not have tooimplement **ITableEntity**.</span></span>

## <a name="how-toouse-hello-net-storage-api-directly-toowork-with-a-table"></a><span data-ttu-id="c0322-134">Hur toouse hello .NET Storage API direkt toowork med en tabell</span><span class="sxs-lookup"><span data-stu-id="c0322-134">How toouse hello .NET Storage API directly toowork with a table</span></span>
<span data-ttu-id="c0322-135">Du kan också använda hello **tabell** attribut med en **CloudTable** objekt för större flexibilitet när du arbetar med en tabell.</span><span class="sxs-lookup"><span data-stu-id="c0322-135">You can also use hello **Table** attribute with a **CloudTable** object for more flexibility in working with a table.</span></span>

<span data-ttu-id="c0322-136">hello följande kod exempel används en **CloudTable** objekt tooadd en enda entitet toohello *ingång* tabell.</span><span class="sxs-lookup"><span data-stu-id="c0322-136">hello following code sample uses a **CloudTable** object tooadd a single entity toohello *Ingress* table.</span></span>

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

<span data-ttu-id="c0322-137">Mer information om hur toouse hello **CloudTable** objekt, se [komma igång med Azure Table storage med hjälp av .NET](../storage/storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="c0322-137">For more information about how toouse hello **CloudTable** object, see [Get started with Azure Table storage using .NET](../storage/storage-dotnet-how-to-use-tables.md).</span></span>

## <a name="related-topics-covered-by-hello-queues-how-tooarticle"></a><span data-ttu-id="c0322-138">Närliggande ämnen som omfattas av hello köer hur-tooarticle</span><span class="sxs-lookup"><span data-stu-id="c0322-138">Related topics covered by hello queues how-tooarticle</span></span>
<span data-ttu-id="c0322-139">Information om hur toohandle tabell bearbetning utlöses av ett kömeddelande eller för WebJobs SDK scenarier inte specifika tootable bearbetning, se [komma igång med Azure Queue storage och Visual Studio anslutna tjänster (Webbjobb projekt) ](../storage/vs-storage-webjobs-getting-started-queues.md).</span><span class="sxs-lookup"><span data-stu-id="c0322-139">For information about how toohandle table processing triggered by a queue message, or for WebJobs SDK scenarios not specific tootable processing, see [Getting started with Azure Queue storage and Visual Studio connected services (WebJob Projects)](../storage/vs-storage-webjobs-getting-started-queues.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0322-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c0322-140">Next steps</span></span>
<span data-ttu-id="c0322-141">Den här artikeln har angett koden exempel som visar hur toohandle vanliga scenarier för att arbeta med Azure-tabeller.</span><span class="sxs-lookup"><span data-stu-id="c0322-141">This article has provided code samples that show how toohandle common scenarios for working with Azure tables.</span></span> <span data-ttu-id="c0322-142">Mer information om hur toouse Azure WebJobs och hello WebJobs SDK, se [Azure WebJobs-dokumentation](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="c0322-142">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

