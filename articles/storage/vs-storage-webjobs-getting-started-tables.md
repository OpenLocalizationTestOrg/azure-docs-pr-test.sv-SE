---
title: "Komma igång med Azure storage och Visual Studio anslutna tjänster (Webbjobb projekt)"
description: "Hur du kommer igång med Azure Table storage i Azure WebJobs-projekt i Visual Studio efter anslutning till ett lagringskonto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 061a6c46-0592-4e5d-aced-ab7498481cde
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 79fba102377cdc6b681f6798699767961040a7e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-storage-azure-webjob-projects"></a><span data-ttu-id="1ea56-103">Komma igång med Azure Storage (Azure Webjobs-projekt)</span><span class="sxs-lookup"><span data-stu-id="1ea56-103">Getting Started with Azure Storage (Azure WebJob Projects)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="1ea56-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1ea56-104">Overview</span></span>
<span data-ttu-id="1ea56-105">Den här artikeln innehåller C#-kodexempel som visar visar hur du använder Azure WebJobs SDK version 1.x med Azure table storage-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1ea56-105">This article provides C# code samples that show show how to use the Azure WebJobs SDK version 1.x with the Azure table storage service.</span></span> <span data-ttu-id="1ea56-106">Koden exempel användning av [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span><span class="sxs-lookup"><span data-stu-id="1ea56-106">The code samples use the [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="1ea56-107">Tjänsten Azure Table storage kan du lagra stora mängder strukturerade data.</span><span class="sxs-lookup"><span data-stu-id="1ea56-107">The Azure Table storage service enables you to store large amounts of structured data.</span></span> <span data-ttu-id="1ea56-108">Tjänsten är en NoSQL-datalager som tar emot autentiserade anrop inuti och utanför Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="1ea56-108">The service is a NoSQL datastore that accepts authenticated calls from inside and outside the Azure cloud.</span></span> <span data-ttu-id="1ea56-109">Azure-tabeller passar utmärkt för att lagra strukturerade, icke-relationella data.</span><span class="sxs-lookup"><span data-stu-id="1ea56-109">Azure tables are ideal for storing structured, non-relational data.</span></span>  <span data-ttu-id="1ea56-110">Se [komma igång med Azure Table storage med hjälp av .NET](storage-dotnet-how-to-use-tables.md#create-a-table) för mer information.</span><span class="sxs-lookup"><span data-stu-id="1ea56-110">See [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md#create-a-table) for more information.</span></span>

<span data-ttu-id="1ea56-111">Vissa av koden kodavsnitt visas den **tabell** attribut som används i funktioner som kallas manuellt, det vill säga inte genom att använda ett av attributen för utlösare.</span><span class="sxs-lookup"><span data-stu-id="1ea56-111">Some of the code snippets show the **Table** attribute used in functions that are called manually, that is, not by using one of the trigger attributes.</span></span>

## <a name="how-to-add-entities-to-a-table"></a><span data-ttu-id="1ea56-112">Lägga till entiteter i en tabell</span><span class="sxs-lookup"><span data-stu-id="1ea56-112">How to add entities to a table</span></span>
<span data-ttu-id="1ea56-113">Om du vill lägga till enheter i en tabell, använder den **tabell** attribut med ett **ICollector<T>**  eller **IAsyncCollector<T>**  parametern där **T** anger schemat för de enheter som du vill lägga till.</span><span class="sxs-lookup"><span data-stu-id="1ea56-113">To add entities to a table, use the **Table** attribute with an **ICollector<T>** or **IAsyncCollector<T>** parameter where **T** specifies the schema of the entities you want to add.</span></span> <span data-ttu-id="1ea56-114">Attributkonstruktorn använder en strängparameter som anger namnet på tabellen.</span><span class="sxs-lookup"><span data-stu-id="1ea56-114">The attribute constructor takes a string parameter that specifies the name of the table.</span></span>

<span data-ttu-id="1ea56-115">Följande kodexempel lägger till **Person** enheter till en tabell med namnet *ingång*.</span><span class="sxs-lookup"><span data-stu-id="1ea56-115">The following code sample adds **Person** entities to a table named *Ingress*.</span></span>

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

<span data-ttu-id="1ea56-116">Typen du använder vanligtvis med **ICollector** härleds från **TableEntity** eller implementerar **ITableEntity**, men det behöver inte.</span><span class="sxs-lookup"><span data-stu-id="1ea56-116">Typically the type you use with **ICollector** derives from **TableEntity** or implements **ITableEntity**, but it doesn't have to.</span></span> <span data-ttu-id="1ea56-117">Något av följande **Person** klasserna arbete med koden som visas i den föregående **ingång** metod.</span><span class="sxs-lookup"><span data-stu-id="1ea56-117">Either of the following **Person** classes work with the code shown in the preceding **Ingress** method.</span></span>

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

<span data-ttu-id="1ea56-118">Om du vill arbeta direkt med Azure storage API, kan du lägga till en **CloudStorageAccount** parameter till Metodsignaturen.</span><span class="sxs-lookup"><span data-stu-id="1ea56-118">If you want to work directly with the Azure storage API, you can add a **CloudStorageAccount** parameter to the method signature.</span></span>

## <a name="real-time-monitoring"></a><span data-ttu-id="1ea56-119">Realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="1ea56-119">Real-time monitoring</span></span>
<span data-ttu-id="1ea56-120">Eftersom dataåtkomstfunktioner ofta bearbetar stora mängder data, innehåller WebJobs SDK instrumentpanelen övervakning realtidsdata.</span><span class="sxs-lookup"><span data-stu-id="1ea56-120">Because data ingress functions often process large volumes of data, the WebJobs SDK dashboard provides real-time monitoring data.</span></span> <span data-ttu-id="1ea56-121">Den **anrop loggen** avsnittet berättar om funktionen fortfarande körs.</span><span class="sxs-lookup"><span data-stu-id="1ea56-121">The **Invocation Log** section tells you if the function is still running.</span></span>

![Ingång funktionen körs](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

<span data-ttu-id="1ea56-123">Den **anrop information** sidan rapporterar funktionens status (antal entiteter som skrivs) medan den körs och ger dig en möjlighet att avbryta den.</span><span class="sxs-lookup"><span data-stu-id="1ea56-123">The **Invocation Details** page reports the function's progress (number of entities written) while it's running and gives you an opportunity to abort it.</span></span>

![Ingång funktionen körs](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

<span data-ttu-id="1ea56-125">När funktionen har slutförts i **anrop information** sidan rapporterar antalet rader som har skrivits.</span><span class="sxs-lookup"><span data-stu-id="1ea56-125">When the function finishes, the **Invocation Details** page reports the number of rows written.</span></span>

![Ingång funktionen klar](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## <a name="how-to-read-multiple-entities-from-a-table"></a><span data-ttu-id="1ea56-127">Läsa flera enheter från en tabell</span><span class="sxs-lookup"><span data-stu-id="1ea56-127">How to read multiple entities from a table</span></span>
<span data-ttu-id="1ea56-128">Läs en tabell genom att använda den **tabell** attribut med ett **IQueryable<T>**  parametern skriver **T** härleds från **TableEntity**eller implementerar **ITableEntity**.</span><span class="sxs-lookup"><span data-stu-id="1ea56-128">To read a table, use the **Table** attribute with an **IQueryable<T>** parameter where type **T** derives from **TableEntity** or implements **ITableEntity**.</span></span>

<span data-ttu-id="1ea56-129">Följande kodexempel läser och loggar alla rader från den **ingång** tabell:</span><span class="sxs-lookup"><span data-stu-id="1ea56-129">The following code sample reads and logs all rows from the **Ingress** table:</span></span>

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

### <a name="how-to-read-a-single-entity-from-a-table"></a><span data-ttu-id="1ea56-130">Läsa en enda entitet från en tabell</span><span class="sxs-lookup"><span data-stu-id="1ea56-130">How to read a single entity from a table</span></span>
<span data-ttu-id="1ea56-131">Det finns en **tabell** Attributkonstruktorn med två ytterligare parametrar som kan du ange partitionsnyckel och radnyckel när du vill binda till en enskild tabell entitet.</span><span class="sxs-lookup"><span data-stu-id="1ea56-131">There is a **Table** attribute constructor with two additional parameters that let you specify the partition key and row key when you want to bind to a single table entity.</span></span>

<span data-ttu-id="1ea56-132">Följande kodexempel läser en tabellrad för en **Person** entitet baserat på partitionen nyckel- och nyckelvärden togs emot i ett kömeddelande:</span><span class="sxs-lookup"><span data-stu-id="1ea56-132">The following code sample reads a table row for a **Person** entity based on partition key and row key values received in a queue message:</span></span>  

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


<span data-ttu-id="1ea56-133">Den **Person** klassen i det här exemplet behöver inte implementera **ITableEntity**.</span><span class="sxs-lookup"><span data-stu-id="1ea56-133">The **Person** class in this example does not have to implement **ITableEntity**.</span></span>

## <a name="how-to-use-the-net-storage-api-directly-to-work-with-a-table"></a><span data-ttu-id="1ea56-134">Hur du använder .NET Storage API direkt för att arbeta med en tabell</span><span class="sxs-lookup"><span data-stu-id="1ea56-134">How to use the .NET Storage API directly to work with a table</span></span>
<span data-ttu-id="1ea56-135">Du kan också använda den **tabell** attribut med en **CloudTable** objekt för större flexibilitet när du arbetar med en tabell.</span><span class="sxs-lookup"><span data-stu-id="1ea56-135">You can also use the **Table** attribute with a **CloudTable** object for more flexibility in working with a table.</span></span>

<span data-ttu-id="1ea56-136">I följande kod exempel används en **CloudTable** objekt att lägga till en enda entitet till den *ingång* tabell.</span><span class="sxs-lookup"><span data-stu-id="1ea56-136">The following code sample uses a **CloudTable** object to add a single entity to the *Ingress* table.</span></span>

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

<span data-ttu-id="1ea56-137">Mer information om hur du använder den **CloudTable** objekt, se [komma igång med Azure Table storage med hjälp av .NET](storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="1ea56-137">For more information about how to use the **CloudTable** object, see [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md).</span></span>

## <a name="related-topics-covered-by-the-queues-how-to-article"></a><span data-ttu-id="1ea56-138">Närliggande ämnen som omfattas av den här artikeln köer</span><span class="sxs-lookup"><span data-stu-id="1ea56-138">Related topics covered by the queues how-to article</span></span>
<span data-ttu-id="1ea56-139">Information om hur du hanterar tabell bearbetning som utlöses av ett kömeddelande eller WebJobs-SDK-scenarier som inte är specifika för bearbetning av tabellen finns [komma igång med Azure Queue storage och Visual Studio anslutna tjänster (Webbjobb projekt) ](vs-storage-webjobs-getting-started-queues.md).</span><span class="sxs-lookup"><span data-stu-id="1ea56-139">For information about how to handle table processing triggered by a queue message, or for WebJobs SDK scenarios not specific to table processing, see [Getting started with Azure Queue storage and Visual Studio connected services (WebJob Projects)](vs-storage-webjobs-getting-started-queues.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ea56-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1ea56-140">Next steps</span></span>
<span data-ttu-id="1ea56-141">Den här artikeln har lämnat kodexempel som visar hur du hanterar vanliga scenarier för att arbeta med Azure-tabeller.</span><span class="sxs-lookup"><span data-stu-id="1ea56-141">This article has provided code samples that show how to handle common scenarios for working with Azure tables.</span></span> <span data-ttu-id="1ea56-142">Mer information om hur du använder Azure WebJobs och WebJobs-SDK finns [Azure WebJobs-dokumentation](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="1ea56-142">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

