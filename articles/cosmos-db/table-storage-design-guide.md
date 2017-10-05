---
title: "Designguide för Azure Storage-tabellen | Microsoft Docs"
description: Design skalbar och Performant tabeller i Azure-tabellagring
services: cosmos-db
documentationcenter: na
author: mimig1
manager: tadb
editor: tysonn
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 02/28/2017
ms.author: mimig
ms.openlocfilehash: fd34fb135c76eed4041c29e00e98dde330dfe3f3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-table-design-guide-designing-scalable-and-performant-tables"></a><span data-ttu-id="3c099-103">Designguide för Azure Storage-tabellen: Utforma skalbar och Performant tabeller</span><span class="sxs-lookup"><span data-stu-id="3c099-103">Azure Storage Table Design Guide: Designing Scalable and Performant Tables</span></span>
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

<span data-ttu-id="3c099-104">Design skalbar och performant tabeller måste du tänka på ett antal faktorer, till exempel prestanda, skalbarhet och kostnader.</span><span class="sxs-lookup"><span data-stu-id="3c099-104">To design scalable and performant tables you must consider a number of factors such as performance, scalability, and cost.</span></span> <span data-ttu-id="3c099-105">Om du har tidigare designat scheman för relationsdatabaser, dessa överväganden känna till dig, men det finns vissa likheter mellan Azure Table storage modell och relationella modeller, men det finns också många viktiga skillnader.</span><span class="sxs-lookup"><span data-stu-id="3c099-105">If you have previously designed schemas for relational databases, these considerations will be familiar to you, but while there are some similarities between the Azure Table service storage model and relational models, there are also many important differences.</span></span> <span data-ttu-id="3c099-106">Dessa skillnader vanligtvis leda till väldigt annorlunda design som kan se krånglig eller felaktig till någon bekant med relationsdatabaser, men som gör bra uppfattning om du designar för en NoSQL nyckel/värde-arkivet, t.ex Azure Table-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-106">These differences typically lead to very different designs that may look counter-intuitive or wrong to someone familiar with relational databases, but which do make good sense if you are designing for a NoSQL key/value store such as the Azure Table service.</span></span> <span data-ttu-id="3c099-107">Många av din design skillnader motsvarar det faktum att tabelltjänsten är utformad för att stödja skalbar molnlagring program som kan innehålla miljarder entiteter (rader i en relationsdatabas terminologi) av data eller för datauppsättningar som måste ha stöd för hög transaktionsvolymer: därför måste du tänka på olika sätt på hur du lagrar data och förstå hur tabelltjänsten fungerar.</span><span class="sxs-lookup"><span data-stu-id="3c099-107">Many of your design differences will reflect the fact that the Table service is designed to support cloud-scale applications that can contain billions of entities (rows in relational database terminology) of data or for datasets that must support very high transaction volumes: therefore, you need to think differently about how you store your data and understand how the Table service works.</span></span> <span data-ttu-id="3c099-108">En väl utformad NoSQL-databas kan aktivera din lösning för att skala mycket mer (och till en lägre kostnad) än en lösning som använder en relationsdatabas.</span><span class="sxs-lookup"><span data-stu-id="3c099-108">A well designed NoSQL data store can enable your solution to scale much further (and at a lower cost) than a solution that uses a relational database.</span></span> <span data-ttu-id="3c099-109">Den här guiden hjälper dig med dessa frågor.</span><span class="sxs-lookup"><span data-stu-id="3c099-109">This guide helps you with these topics.</span></span>  

## <a name="about-the-azure-table-service"></a><span data-ttu-id="3c099-110">Om tjänsten Azure Table</span><span class="sxs-lookup"><span data-stu-id="3c099-110">About the Azure Table service</span></span>
<span data-ttu-id="3c099-111">Det här avsnittet beskrivs några av de viktigaste funktionerna i tabelltjänsten som är särskilt relevanta till utformning för prestanda och skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="3c099-111">This section highlights some of the key features of the Table service that are especially relevant to designing for performance and scalability.</span></span> <span data-ttu-id="3c099-112">Om du har använt Azure Storage- och tabelltjänsten, läser du först [introduktion till Microsoft Azure Storage](../storage/common/storage-introduction.md) och [Kom igång med Azure Table Storage med hjälp av .NET](table-storage-how-to-use-dotnet.md) innan du läser resten av den här artikeln .</span><span class="sxs-lookup"><span data-stu-id="3c099-112">If you are new to Azure Storage and the Table service, first read [Introduction to Microsoft Azure Storage](../storage/common/storage-introduction.md) and [Get started with Azure Table Storage using .NET](table-storage-how-to-use-dotnet.md) before reading the remainder of this article.</span></span> <span data-ttu-id="3c099-113">Även om den här guiden fokuserar på tabelltjänsten, innehåller en beskrivning av Azure Queue och Blob-tjänster och hur du kan använda dem tillsammans med tjänsten tabellen i en lösning.</span><span class="sxs-lookup"><span data-stu-id="3c099-113">Although the focus of this guide is on the Table service, it will include some discussion of the Azure Queue and Blob services, and how you might use them along with the Table service in a solution.</span></span>  

<span data-ttu-id="3c099-114">Vad är tabelltjänsten?</span><span class="sxs-lookup"><span data-stu-id="3c099-114">What is the Table service?</span></span> <span data-ttu-id="3c099-115">Som du kan förvänta sig från namnet använder tabelltjänsten tabellformat för att lagra data.</span><span class="sxs-lookup"><span data-stu-id="3c099-115">As you might expect from the name, the Table service uses a tabular format to store data.</span></span> <span data-ttu-id="3c099-116">Varje rad i tabellen representerar en entitet i standard terminologi och kolumnerna lagra olika egenskaper för enheten.</span><span class="sxs-lookup"><span data-stu-id="3c099-116">In the standard terminology, each row of the table represents an entity, and the columns store the various properties of that entity.</span></span> <span data-ttu-id="3c099-117">Varje entitet har ett par nycklar för att identifiera den och en tidsstämpelkolumn som tabelltjänsten använder för att spåra när entiteten senast uppdaterade (detta sker automatiskt och du kan inte skriva över tidsstämpeln manuellt med ett godtyckligt värde).</span><span class="sxs-lookup"><span data-stu-id="3c099-117">Every entity has a pair of keys to uniquely identify it, and a timestamp column that the Table service uses to track when the entity was last updated (this happens automatically and you cannot manually overwrite the timestamp with an arbitrary value).</span></span> <span data-ttu-id="3c099-118">Tabelltjänsten använder den här last-modified-tidsstämpel (LMT) för att hantera Optimistisk samtidighet.</span><span class="sxs-lookup"><span data-stu-id="3c099-118">The Table service uses this last-modified timestamp (LMT) to manage optimistic concurrency.</span></span>  

> [!NOTE]
> <span data-ttu-id="3c099-119">REST API för tabellen tjänståtgärderna också returnera en **ETag** värde som den härleds från last-modified-tidsstämpeln (LMT).</span><span class="sxs-lookup"><span data-stu-id="3c099-119">The Table service REST API operations also return an **ETag** value that it derives from the last-modified timestamp (LMT).</span></span> <span data-ttu-id="3c099-120">I det här dokumentet ska vi använda villkoren ETag och LMT synonymt eftersom de refererar till samma underliggande data.</span><span class="sxs-lookup"><span data-stu-id="3c099-120">In this document we will use the terms ETag and LMT interchangeably because they refer to the same underlying data.</span></span>  
> 
> 

<span data-ttu-id="3c099-121">I följande exempel visas ett enkelt tabelldesign att lagra medarbetare och avdelning entiteter.</span><span class="sxs-lookup"><span data-stu-id="3c099-121">The following example shows a simple table design to store employee and department entities.</span></span> <span data-ttu-id="3c099-122">Många av exemplen senare i den här handboken är baserade på den här enkla designen.</span><span class="sxs-lookup"><span data-stu-id="3c099-122">Many of the examples shown later in this guide are based on this simple design.</span></span>  

<table>
<tr>
<th><span data-ttu-id="3c099-123">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="3c099-123">PartitionKey</span></span></th>
<th><span data-ttu-id="3c099-124">RowKey</span><span class="sxs-lookup"><span data-stu-id="3c099-124">RowKey</span></span></th>
<th><span data-ttu-id="3c099-125">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="3c099-125">Timestamp</span></span></th>
<th></th>
</tr>
<tr>
<td><span data-ttu-id="3c099-126">Marknadsföring</span><span class="sxs-lookup"><span data-stu-id="3c099-126">Marketing</span></span></td>
<td><span data-ttu-id="3c099-127">00001</span><span class="sxs-lookup"><span data-stu-id="3c099-127">00001</span></span></td>
<td><span data-ttu-id="3c099-128">2014-08-22T00:50:32Z</span><span class="sxs-lookup"><span data-stu-id="3c099-128">2014-08-22T00:50:32Z</span></span></td>
<td>
<table>
<tr>
<th><span data-ttu-id="3c099-129">Förnamn</span><span class="sxs-lookup"><span data-stu-id="3c099-129">FirstName</span></span></th>
<th><span data-ttu-id="3c099-130">Efternamn</span><span class="sxs-lookup"><span data-stu-id="3c099-130">LastName</span></span></th>
<th><span data-ttu-id="3c099-131">Ålder</span><span class="sxs-lookup"><span data-stu-id="3c099-131">Age</span></span></th>
<th><span data-ttu-id="3c099-132">E-post</span><span class="sxs-lookup"><span data-stu-id="3c099-132">Email</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="3c099-133">Ta</span><span class="sxs-lookup"><span data-stu-id="3c099-133">Don</span></span></td>
<td><span data-ttu-id="3c099-134">Hall</span><span class="sxs-lookup"><span data-stu-id="3c099-134">Hall</span></span></td>
<td><span data-ttu-id="3c099-135">34</span><span class="sxs-lookup"><span data-stu-id="3c099-135">34</span></span></td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td><span data-ttu-id="3c099-136">Marknadsföring</span><span class="sxs-lookup"><span data-stu-id="3c099-136">Marketing</span></span></td>
<td><span data-ttu-id="3c099-137">00002</span><span class="sxs-lookup"><span data-stu-id="3c099-137">00002</span></span></td>
<td><span data-ttu-id="3c099-138">2014-08-22T00:50:34Z</span><span class="sxs-lookup"><span data-stu-id="3c099-138">2014-08-22T00:50:34Z</span></span></td>
<td>
<table>
<tr>
<th><span data-ttu-id="3c099-139">Förnamn</span><span class="sxs-lookup"><span data-stu-id="3c099-139">FirstName</span></span></th>
<th><span data-ttu-id="3c099-140">Efternamn</span><span class="sxs-lookup"><span data-stu-id="3c099-140">LastName</span></span></th>
<th><span data-ttu-id="3c099-141">Ålder</span><span class="sxs-lookup"><span data-stu-id="3c099-141">Age</span></span></th>
<th><span data-ttu-id="3c099-142">E-post</span><span class="sxs-lookup"><span data-stu-id="3c099-142">Email</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="3c099-143">Jun</span><span class="sxs-lookup"><span data-stu-id="3c099-143">Jun</span></span></td>
<td><span data-ttu-id="3c099-144">CaO</span><span class="sxs-lookup"><span data-stu-id="3c099-144">Cao</span></span></td>
<td><span data-ttu-id="3c099-145">47</span><span class="sxs-lookup"><span data-stu-id="3c099-145">47</span></span></td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td><span data-ttu-id="3c099-146">Marknadsföring</span><span class="sxs-lookup"><span data-stu-id="3c099-146">Marketing</span></span></td>
<td><span data-ttu-id="3c099-147">Avdelning</span><span class="sxs-lookup"><span data-stu-id="3c099-147">Department</span></span></td>
<td><span data-ttu-id="3c099-148">2014-08-22T00:50:30Z</span><span class="sxs-lookup"><span data-stu-id="3c099-148">2014-08-22T00:50:30Z</span></span></td>
<td>
<table>
<tr>
<th><span data-ttu-id="3c099-149">DepartmentName</span><span class="sxs-lookup"><span data-stu-id="3c099-149">DepartmentName</span></span></th>
<th><span data-ttu-id="3c099-150">EmployeeCount</span><span class="sxs-lookup"><span data-stu-id="3c099-150">EmployeeCount</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="3c099-151">Marknadsföring</span><span class="sxs-lookup"><span data-stu-id="3c099-151">Marketing</span></span></td>
<td><span data-ttu-id="3c099-152">153</span><span class="sxs-lookup"><span data-stu-id="3c099-152">153</span></span></td>
</tr>
</table>
</td>
</tr>
<tr>
<td><span data-ttu-id="3c099-153">Försäljning</span><span class="sxs-lookup"><span data-stu-id="3c099-153">Sales</span></span></td>
<td><span data-ttu-id="3c099-154">00010</span><span class="sxs-lookup"><span data-stu-id="3c099-154">00010</span></span></td>
<td><span data-ttu-id="3c099-155">2014-08-22T00:50:44Z</span><span class="sxs-lookup"><span data-stu-id="3c099-155">2014-08-22T00:50:44Z</span></span></td>
<td>
<table>
<tr>
<th><span data-ttu-id="3c099-156">Förnamn</span><span class="sxs-lookup"><span data-stu-id="3c099-156">FirstName</span></span></th>
<th><span data-ttu-id="3c099-157">Efternamn</span><span class="sxs-lookup"><span data-stu-id="3c099-157">LastName</span></span></th>
<th><span data-ttu-id="3c099-158">Ålder</span><span class="sxs-lookup"><span data-stu-id="3c099-158">Age</span></span></th>
<th><span data-ttu-id="3c099-159">E-post</span><span class="sxs-lookup"><span data-stu-id="3c099-159">Email</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="3c099-160">Ken</span><span class="sxs-lookup"><span data-stu-id="3c099-160">Ken</span></span></td>
<td><span data-ttu-id="3c099-161">Kwok</span><span class="sxs-lookup"><span data-stu-id="3c099-161">Kwok</span></span></td>
<td><span data-ttu-id="3c099-162">23</span><span class="sxs-lookup"><span data-stu-id="3c099-162">23</span></span></td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


<span data-ttu-id="3c099-163">Hittills har liknar detta en tabell i en relationsdatabas med de viktigaste skillnaderna är de obligatoriska kolumnerna och möjlighet att lagra flera typer av enheter i samma tabell.</span><span class="sxs-lookup"><span data-stu-id="3c099-163">So far, this looks very similar to a table in a relational database with the key differences being the mandatory columns, and the ability to store multiple entity types in the same table.</span></span> <span data-ttu-id="3c099-164">Dessutom var och en av de användardefinierade egenskaperna som **Förnamn** eller **ålder** har en datatyp som heltal eller en sträng, precis som en kolumn i en relationsdatabas.</span><span class="sxs-lookup"><span data-stu-id="3c099-164">In addition, each of the user-defined properties such as **FirstName** or **Age** has a data type, such as integer or string, just like a column in a relational database.</span></span> <span data-ttu-id="3c099-165">Men till skillnad från i en relationsdatabas tabelltjänsten schemat mindre uppbyggnad innebär att en egenskap måste ha samma datatyp för varje entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-165">Although unlike in a relational database, the schema-less nature of the Table service means that a property need not have the same data type on each entity.</span></span> <span data-ttu-id="3c099-166">Om du vill lagra komplexa datatyper i en enskild egenskap, måste du använda ett serialiserat format, till exempel JSON eller XML.</span><span class="sxs-lookup"><span data-stu-id="3c099-166">To store complex data types in a single property, you must use a serialized format such as JSON or XML.</span></span> <span data-ttu-id="3c099-167">Mer information om datatyper i tabellen service som stöds, stöds datumintervall, namngivningsregler och storlek restriktioner finns [förstå den tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c099-167">For more information about the table service such as supported data types, supported date ranges, naming rules, and size constraints, see [Understanding the Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="3c099-168">Eftersom du kommer att se ditt val av **PartitionKey** och **RowKey** är grundläggande för bra tabelldesign.</span><span class="sxs-lookup"><span data-stu-id="3c099-168">As you will see, your choice of **PartitionKey** and **RowKey** is fundamental to good table design.</span></span> <span data-ttu-id="3c099-169">Varje entitet som lagras i en tabell måste ha en unik kombination av **PartitionKey** och **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="3c099-169">Every entity stored in a table must have a unique combination of **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="3c099-170">Precis som med nycklar i en relationsdatabas tabell den **PartitionKey** och **RowKey** värden indexeras om du vill skapa ett grupperat index som möjliggör snabb look-ups; men tabelltjänsten skapar inte någon sekundärindex så att de bara två indexerade egenskaper (vissa av de mönster som beskrivs senare visar hur du kan undvika den här tydligt begränsningen).</span><span class="sxs-lookup"><span data-stu-id="3c099-170">As with keys in a relational database table, the **PartitionKey** and **RowKey** values are indexed to create a clustered index that enables fast look-ups; however, the Table service does not create any secondary indexes so these are the only two indexed properties (some of the patterns described later show how you can work around this apparent limitation).</span></span>  

<span data-ttu-id="3c099-171">En tabell består av en eller flera partitioner och som du kommer att se många designbeslut som du kommer att runt att välja en lämplig **PartitionKey** och **RowKey** att optimera din lösning.</span><span class="sxs-lookup"><span data-stu-id="3c099-171">A table is made up of one or more partitions, and as you will see, many of the design decisions you make will be around choosing a suitable **PartitionKey** and **RowKey** to optimize your solution.</span></span> <span data-ttu-id="3c099-172">En lösning kan bestå av bara en enda tabell som innehåller alla entiteter indelade i partitioner, men vanligtvis en lösning har flera tabeller.</span><span class="sxs-lookup"><span data-stu-id="3c099-172">A solution could consist of just a single table that contains all your entities organized into partitions, but typically a solution will have multiple tables.</span></span> <span data-ttu-id="3c099-173">Tabeller hjälper dig att ordna dina enheter, hjälper dig att hantera åtkomst till data med åtkomstkontrollistor logiskt och du kan släppa en hel tabell med hjälp av en enda lagring-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="3c099-173">Tables help you to logically organize your entities, help you manage access to the data using access control lists, and you can drop an entire table using a single storage operation.</span></span>  

### <a name="table-partitions"></a><span data-ttu-id="3c099-174">Tabellpartitioner</span><span class="sxs-lookup"><span data-stu-id="3c099-174">Table partitions</span></span>
<span data-ttu-id="3c099-175">Kontonamn, tabellnamnet och **PartitionKey** identifierar tillsammans partitionen i lagringstjänsten där tabelltjänsten lagrar entiteten.</span><span class="sxs-lookup"><span data-stu-id="3c099-175">The account name, table name and **PartitionKey** together identify the partition within the storage service where the table service stores the entity.</span></span> <span data-ttu-id="3c099-176">Samt som en del av adresseringsschema för entiteter partitioner för att definiera ett omfång för transaktioner (se [entitet gruppera transaktioner](#entity-group-transactions) nedan), och utgör grunden för hur tabelltjänsten skalas.</span><span class="sxs-lookup"><span data-stu-id="3c099-176">As well as being part of the addressing scheme for entities, partitions define a scope for transactions (see [Entity Group Transactions](#entity-group-transactions) below), and form the basis of how the table service scales.</span></span> <span data-ttu-id="3c099-177">Läs mer på partitioner [Azure Storage skalbarhets- och prestandamål](../storage/common/storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="3c099-177">For more information on partitions see [Azure Storage Scalability and Performance Targets](../storage/common/storage-scalability-targets.md).</span></span>  

<span data-ttu-id="3c099-178">I tabellen service är en enskild nod tjänsterna eller mer Slutför partitioner och tjänsten skalar av dynamiskt nätverksbelastning partitioner mellan noder.</span><span class="sxs-lookup"><span data-stu-id="3c099-178">In the Table service, an individual node services one or more complete partitions and the service scales by dynamically load-balancing partitions across nodes.</span></span> <span data-ttu-id="3c099-179">Om en nod under belastning kan tabelltjänsten *dela* antal partitioner som underhålls av noden på olika noder; när trafik subsides tjänsten kan *merge* partition intervall från tyst noder tillbaka till en enda nod.</span><span class="sxs-lookup"><span data-stu-id="3c099-179">If a node is under load, the table service can *split* the range of partitions serviced by that node onto different nodes; when traffic subsides, the service can *merge* the partition ranges from quiet nodes back onto a single node.</span></span>  

<span data-ttu-id="3c099-180">Mer information om internt information för tabelltjänsten, särskilt hur tjänsten hanterar partitioner, finns i dokumentet [Microsoft Azure Storage: A hög Available Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c099-180">For more information about the internal details of the Table service, and in particular how the service manages partitions, see the paper [Microsoft Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).</span></span>  

### <a name="entity-group-transactions"></a><span data-ttu-id="3c099-181">Entiteten gruppera transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-181">Entity Group Transactions</span></span>
<span data-ttu-id="3c099-182">I tjänsten tabell är entitet grupptransaktioner (EGTs) den inbyggda mekanismen för att utföra atomiska uppdateringar över flera enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-182">In the Table service, Entity Group Transactions (EGTs) are the only built-in mechanism for performing atomic updates across multiple entities.</span></span> <span data-ttu-id="3c099-183">EGTs också kallas *batch transaktioner* i viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="3c099-183">EGTs are also referred to as *batch transactions* in some documentation.</span></span> <span data-ttu-id="3c099-184">EGTs fungerar bara med entiteter som lagras i samma partition (dela samma partitionsnyckel i en given tabell), så vilja ha atomiska transaktionella beteende över flera enheter måste du se till att dessa enheter finns i samma partition.</span><span class="sxs-lookup"><span data-stu-id="3c099-184">EGTs can only operate on entities stored in the same partition (share the same partition key in a given table), so anytime you need atomic transactional behavior across multiple entities you need to ensure that those entities are in the same partition.</span></span> <span data-ttu-id="3c099-185">Det här är ofta en orsak till att ha flera typer av enheter i samma tabell (och partition) och inte använder flera tabeller för olika enhetstyper.</span><span class="sxs-lookup"><span data-stu-id="3c099-185">This is often a reason for keeping multiple entity types in the same table (and partition) and not using multiple tables for different entity types.</span></span> <span data-ttu-id="3c099-186">En enda EGT kan tillämpas på högst 100 entiteter.</span><span class="sxs-lookup"><span data-stu-id="3c099-186">A single EGT can operate on at most 100 entities.</span></span>  <span data-ttu-id="3c099-187">Om du skickar flera samtidiga EGTs för bearbetning är det viktigt att se till att dessa EGTs inte fungerar på enheter som är gemensamma mellan EGTs som annars bearbetning kan vara fördröjd.</span><span class="sxs-lookup"><span data-stu-id="3c099-187">If you submit multiple concurrent EGTs for processing it is important to ensure  those EGTs do not operate on entities that are common across EGTs as otherwise processing can be delayed.</span></span>

<span data-ttu-id="3c099-188">EGTs också introducera en potentiell kompromiss där du kan utvärdera i din design: använda fler partitioner ökar skalbarheten för ditt program eftersom Azure har flera möjligheter för belastningsutjämning begäranden mellan noder, men detta kan begränsa möjligheten för ditt program att utföra atomiska transaktioner och underhålla stark konsekvens för dina data.</span><span class="sxs-lookup"><span data-stu-id="3c099-188">EGTs also introduce a potential trade-off for you to evaluate in your design: using more partitions will increase the scalability of your application because Azure has more opportunities for load balancing requests across nodes, but this might limit the ability of your application to perform atomic transactions and maintain strong consistency for your data.</span></span> <span data-ttu-id="3c099-189">Dessutom finns specifika skalbarhetsmål på nivån för en partition som kan begränsa genomflödet av transaktioner som du kan förvänta dig för en enskild nod: Läs mer om skalbarhetsmål för Azure storage-konton och tabelltjänsten [Azure Storage skalbarhets- och prestandamål](../storage/common/storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="3c099-189">Furthermore, there are specific scalability targets at the level of a partition that might limit the throughput of transactions you can expect for a single node: for more information about the scalability targets for Azure storage accounts and the table service, see [Azure Storage Scalability and Performance Targets](../storage/common/storage-scalability-targets.md).</span></span> <span data-ttu-id="3c099-190">Senare avsnitt i den här handboken beskrivs olika design strategier som kan hjälpa dig att hantera avvägningarna som den här och diskutera hur du bäst för att välja din partitionsnyckel baserat på de särskilda kraven i ditt klientprogram.</span><span class="sxs-lookup"><span data-stu-id="3c099-190">Later sections of this guide discuss various design strategies that help you manage trade-offs such as this one, and discuss how best to choose your partition key based on the specific requirements of your client application.</span></span>  

### <a name="capacity-considerations"></a><span data-ttu-id="3c099-191">Överväganden för kapacitetsplanering</span><span class="sxs-lookup"><span data-stu-id="3c099-191">Capacity considerations</span></span>
<span data-ttu-id="3c099-192">Följande tabell innehåller några nyckelvärden för att vara medveten om när du designar en lösning för tabell:</span><span class="sxs-lookup"><span data-stu-id="3c099-192">The following table includes some of the key values to be aware of when you are designing a Table service solution:</span></span>  

| <span data-ttu-id="3c099-193">Total kapacitet för ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="3c099-193">Total capacity of an Azure storage account</span></span> | <span data-ttu-id="3c099-194">500 TB</span><span class="sxs-lookup"><span data-stu-id="3c099-194">500 TB</span></span> |
| --- | --- |
| <span data-ttu-id="3c099-195">Antalet tabeller i ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="3c099-195">Number of tables in an Azure storage account</span></span> |<span data-ttu-id="3c099-196">Begränsas bara av kapaciteten för lagringskontot</span><span class="sxs-lookup"><span data-stu-id="3c099-196">Limited only by the capacity of the storage account</span></span> |
| <span data-ttu-id="3c099-197">Antalet partitioner i en tabell</span><span class="sxs-lookup"><span data-stu-id="3c099-197">Number of partitions in a table</span></span> |<span data-ttu-id="3c099-198">Begränsas bara av kapaciteten för lagringskontot</span><span class="sxs-lookup"><span data-stu-id="3c099-198">Limited only by the capacity of the storage account</span></span> |
| <span data-ttu-id="3c099-199">Antal entiteter i en partition</span><span class="sxs-lookup"><span data-stu-id="3c099-199">Number of entities in a partition</span></span> |<span data-ttu-id="3c099-200">Begränsas bara av kapaciteten för lagringskontot</span><span class="sxs-lookup"><span data-stu-id="3c099-200">Limited only by the capacity of the storage account</span></span> |
| <span data-ttu-id="3c099-201">Storleken på en enskild entitet</span><span class="sxs-lookup"><span data-stu-id="3c099-201">Size of an individual entity</span></span> |<span data-ttu-id="3c099-202">Upp till 1 MB med högst 255 egenskaper (inklusive den **PartitionKey**, **RowKey**, och **tidsstämpel**)</span><span class="sxs-lookup"><span data-stu-id="3c099-202">Up to 1 MB with a maximum of 255 properties (including the **PartitionKey**, **RowKey**, and **Timestamp**)</span></span> |
| <span data-ttu-id="3c099-203">Storleken på den **PartitionKey**</span><span class="sxs-lookup"><span data-stu-id="3c099-203">Size of the **PartitionKey**</span></span> |<span data-ttu-id="3c099-204">En sträng i storlek på upp till 1 KB</span><span class="sxs-lookup"><span data-stu-id="3c099-204">A string up to 1 KB in size</span></span> |
| <span data-ttu-id="3c099-205">Storleken på den **RowKey**</span><span class="sxs-lookup"><span data-stu-id="3c099-205">Size of the **RowKey**</span></span> |<span data-ttu-id="3c099-206">En sträng i storlek på upp till 1 KB</span><span class="sxs-lookup"><span data-stu-id="3c099-206">A string up to 1 KB in size</span></span> |
| <span data-ttu-id="3c099-207">Storleken på en entitet grupp-transaktion</span><span class="sxs-lookup"><span data-stu-id="3c099-207">Size of an Entity Group Transaction</span></span> |<span data-ttu-id="3c099-208">En transaktion får innehålla högst 100 entiteter och nyttolasten måste vara mindre än 4 MB i storlek.</span><span class="sxs-lookup"><span data-stu-id="3c099-208">A transaction can include at most 100 entities and the payload must be less than 4 MB in size.</span></span> <span data-ttu-id="3c099-209">En entitet kan bara uppdatera en gång i en EGT.</span><span class="sxs-lookup"><span data-stu-id="3c099-209">An EGT can only update an entity once.</span></span> |

<span data-ttu-id="3c099-210">Mer information finns i [förstå den tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c099-210">For more information, see [Understanding the Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>  

### <a name="cost-considerations"></a><span data-ttu-id="3c099-211">Kostnad överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-211">Cost considerations</span></span>
<span data-ttu-id="3c099-212">Table storage är relativt billig, men du bör innehålla kostnaderna för både kapacitetsförbrukning och antalet transaktioner som en del av din utvärderingsversion av någon lösning som använder tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-212">Table storage is relatively inexpensive, but you should include cost estimates for both capacity usage and the quantity of transactions as part of your evaluation of any solution that uses the Table service.</span></span> <span data-ttu-id="3c099-213">I många scenarier för datalagring Avnormaliserade eller dubbletter för att förbättra är prestanda och skalbarhet för din lösning dock en giltig metod för att vidta.</span><span class="sxs-lookup"><span data-stu-id="3c099-213">However, in many scenarios storing denormalized or duplicate data in order to improve the performance or scalability of your solution is a valid approach to take.</span></span> <span data-ttu-id="3c099-214">Mer information om priser finns [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/).</span><span class="sxs-lookup"><span data-stu-id="3c099-214">For more information about pricing, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/).</span></span>  

## <a name="guidelines-for-table-design"></a><span data-ttu-id="3c099-215">Riktlinjer för tabelldesign</span><span class="sxs-lookup"><span data-stu-id="3c099-215">Guidelines for table design</span></span>
<span data-ttu-id="3c099-216">De här listorna sammanfattas några av de viktiga riktlinjer som du bör tänka på när du utformar dina tabeller och den här guiden att behandla dem i detalj senare i.</span><span class="sxs-lookup"><span data-stu-id="3c099-216">These lists summarize some of the key guidelines you should keep in mind when you are designing your tables, and this guide will address them all in more detail later in.</span></span> <span data-ttu-id="3c099-217">Dessa riktlinjer är bland annat de riktlinjer som du vanligtvis utför för relationsdatabas design.</span><span class="sxs-lookup"><span data-stu-id="3c099-217">These guidelines are very different from the guidelines you would typically follow for relational database design.</span></span>  

<span data-ttu-id="3c099-218">Utforma din lösning för tabellen ska *läsa* effektivt:</span><span class="sxs-lookup"><span data-stu-id="3c099-218">Designing your Table service solution to be *read* efficient:</span></span>

* <span data-ttu-id="3c099-219">***Design för frågor i Läs-aktiverat program.***</span><span class="sxs-lookup"><span data-stu-id="3c099-219">***Design for querying in read-heavy applications.***</span></span> <span data-ttu-id="3c099-220">När du utformar dina tabeller tänka frågor (särskilt svarstid känsliga de) som du ska köra innan du tycker om hur du ska uppdatera-enheterna.</span><span class="sxs-lookup"><span data-stu-id="3c099-220">When you are designing your tables, think about the queries (especially the latency sensitive ones) that you will execute before you think about how you will update your entities.</span></span> <span data-ttu-id="3c099-221">Detta ger vanligtvis en effektiv och performant lösning.</span><span class="sxs-lookup"><span data-stu-id="3c099-221">This typically results in an efficient and performant solution.</span></span>  
* <span data-ttu-id="3c099-222">***Ange både PartitionKey och RowKey i dina frågor.***</span><span class="sxs-lookup"><span data-stu-id="3c099-222">***Specify both PartitionKey and RowKey in your queries.***</span></span> <span data-ttu-id="3c099-223">*Peka frågor* som dessa är de mest effektiva tabellen service frågorna.</span><span class="sxs-lookup"><span data-stu-id="3c099-223">*Point queries* such as these are the most efficient table service queries.</span></span>  
* <span data-ttu-id="3c099-224">***Överväg att lagra dubbletter av enheter.***</span><span class="sxs-lookup"><span data-stu-id="3c099-224">***Consider storing duplicate copies of entities.***</span></span> <span data-ttu-id="3c099-225">Table storage är billiga så du överväga att spara samma entitet flera gånger (med olika nycklar) om du vill aktivera effektivare frågor.</span><span class="sxs-lookup"><span data-stu-id="3c099-225">Table storage is cheap so consider storing the same entity multiple times (with different keys) to enable more efficient queries.</span></span>  
* <span data-ttu-id="3c099-226">***Överväg att denormalizing dina data.***</span><span class="sxs-lookup"><span data-stu-id="3c099-226">***Consider denormalizing your data.***</span></span> <span data-ttu-id="3c099-227">Table storage är billiga så Överväg denormalizing dina data.</span><span class="sxs-lookup"><span data-stu-id="3c099-227">Table storage is cheap so consider denormalizing your data.</span></span> <span data-ttu-id="3c099-228">Lagra exempelvis sammanfattning enheter så att frågor för att samla in data bara behöver åtkomst till en enda entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-228">For example, store summary entities so that queries for aggregate data only need to access a single entity.</span></span>  
* <span data-ttu-id="3c099-229">***Använd sammansatt nyckelvärden.***</span><span class="sxs-lookup"><span data-stu-id="3c099-229">***Use compound key values.***</span></span> <span data-ttu-id="3c099-230">Är bara nycklar som du har **PartitionKey** och **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="3c099-230">The only keys you have are **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="3c099-231">Till exempel använda sammansatta nyckelvärden för att aktivera alternativ nycklad åtkomst sökvägar till enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-231">For example, use compound key values to enable alternate keyed access paths to entities.</span></span>  
* <span data-ttu-id="3c099-232">***Använd fråga projektion.***</span><span class="sxs-lookup"><span data-stu-id="3c099-232">***Use query projection.***</span></span> <span data-ttu-id="3c099-233">Du kan minska mängden data som du överför via nätverket med hjälp av frågor som väljer de fält du behöver.</span><span class="sxs-lookup"><span data-stu-id="3c099-233">You can reduce the amount of data that you transfer over the network by using queries that select just the fields you need.</span></span>  

<span data-ttu-id="3c099-234">Utforma din lösning för tabellen ska *skriva* effektivt:</span><span class="sxs-lookup"><span data-stu-id="3c099-234">Designing your Table service solution to be *write* efficient:</span></span>  

* <span data-ttu-id="3c099-235">***Skapa inte varm partitioner.***</span><span class="sxs-lookup"><span data-stu-id="3c099-235">***Do not create hot partitions.***</span></span> <span data-ttu-id="3c099-236">Välj nycklar att sprida dina begäranden över flera partitioner vid någon tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="3c099-236">Choose keys that enable you to spread your requests across multiple partitions at any point of time.</span></span>  
* <span data-ttu-id="3c099-237">***Undvika toppar i trafiken.***</span><span class="sxs-lookup"><span data-stu-id="3c099-237">***Avoid spikes in traffic.***</span></span> <span data-ttu-id="3c099-238">Utjämna trafiken över en rimlig tid och undvika toppar i trafiken.</span><span class="sxs-lookup"><span data-stu-id="3c099-238">Smooth the traffic over a reasonable period of time and avoid spikes in traffic.</span></span>
* <span data-ttu-id="3c099-239">***Inte nödvändigtvis att skapa en separat tabell för varje typ av enhet.***</span><span class="sxs-lookup"><span data-stu-id="3c099-239">***Don't necessarily create a separate table for each type of entity.***</span></span> <span data-ttu-id="3c099-240">När du behöver atomiska transaktioner över typer av enheter kan lagra du dessa flera typer av enheter i samma partition i samma tabell.</span><span class="sxs-lookup"><span data-stu-id="3c099-240">When you require atomic transactions across entity types, you can store these multiple entity types in the same partition in the same table.</span></span>
* <span data-ttu-id="3c099-241">***Överväg att maximalt dataflöde du måste uppnå.***</span><span class="sxs-lookup"><span data-stu-id="3c099-241">***Consider the maximum throughput you must achieve.***</span></span> <span data-ttu-id="3c099-242">Du måste vara medveten om skalbarhetsmål för tabelltjänsten och se till att designen inte ska orsaka du överskrida dem.</span><span class="sxs-lookup"><span data-stu-id="3c099-242">You must be aware of the scalability targets for the Table service and ensure that your design will not cause you to exceed them.</span></span>  

<span data-ttu-id="3c099-243">När du läser handboken visas exempel placera alla dessa principer i praktiken.</span><span class="sxs-lookup"><span data-stu-id="3c099-243">As you read this guide, you will see examples that put all of these principles into practice.</span></span>  

## <a name="design-for-querying"></a><span data-ttu-id="3c099-244">Design för frågor</span><span class="sxs-lookup"><span data-stu-id="3c099-244">Design for querying</span></span>
<span data-ttu-id="3c099-245">Tabell tjänstelösningar får läsas intensiva, Skriv intensiva eller en blandning av båda.</span><span class="sxs-lookup"><span data-stu-id="3c099-245">Table service solutions may be read intensive, write intensive, or a mix of the two.</span></span> <span data-ttu-id="3c099-246">Det här avsnittet handlar om saker att ha i åtanke när du utformar din tabell-tjänsten för att stödja läsåtgärder effektivt.</span><span class="sxs-lookup"><span data-stu-id="3c099-246">This section focuses on the things to bear in mind when you are designing your Table service to support read operations efficiently.</span></span> <span data-ttu-id="3c099-247">Vanligtvis är en design som stöder läsåtgärder effektivt också effektivt för skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="3c099-247">Typically, a design that supports read operations efficiently is also efficient for write operations.</span></span> <span data-ttu-id="3c099-248">Men det finns ytterligare överväganden att ha i åtanke när du skapar för att stödja skrivåtgärder som beskrivs i nästa avsnitt, [Design för dataändring](#design-for-data-modification).</span><span class="sxs-lookup"><span data-stu-id="3c099-248">However, there are additional considerations to bear in mind when designing to support write operations, discussed in the next section, [Design for data modification](#design-for-data-modification).</span></span>

<span data-ttu-id="3c099-249">En bra utgångspunkt för att utforma din lösning för tabell så att du kan läsa data på ett effektivt sätt är att be ”vilka frågor mitt program måste köras för att hämta data som behövs från tabelltjänsten”?</span><span class="sxs-lookup"><span data-stu-id="3c099-249">A good starting point for designing your Table service solution to enable you to read data efficiently is to ask "What queries will my application need to execute to retrieve the data it needs from the Table service?"</span></span>  

> [!NOTE]
> <span data-ttu-id="3c099-250">Det är viktigt att hämta designen rätt direkt eftersom det är svåra och dyra att ändra dem senare med tabell-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-250">With the Table service, it's important to get the design correct up front because it's difficult and expensive to change it later.</span></span> <span data-ttu-id="3c099-251">Till exempel i en relationsdatabas är det ofta går att åtgärda problem med prestanda genom att lägga till index i en befintlig databas: Detta är inte ett alternativ med tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-251">For example, in a relational database it's often possible to address performance issues simply by adding indexes to an existing database: this is not an option with the Table service.</span></span>  
> 
> 

<span data-ttu-id="3c099-252">Det här avsnittet fokuserar på viktiga problem som du måste ta när du designar tabeller för frågor.</span><span class="sxs-lookup"><span data-stu-id="3c099-252">This section focuses on the key issues you must address when you design your tables for querying.</span></span> <span data-ttu-id="3c099-253">I det här avsnittet behandlar:</span><span class="sxs-lookup"><span data-stu-id="3c099-253">The topics covered in this section include:</span></span>

* [<span data-ttu-id="3c099-254">Hur du väljer PartitionKey och RowKey påverkar prestanda för frågor</span><span class="sxs-lookup"><span data-stu-id="3c099-254">How your choice of PartitionKey and RowKey impacts query performance</span></span>](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [<span data-ttu-id="3c099-255">Att välja en lämplig PartitionKey</span><span class="sxs-lookup"><span data-stu-id="3c099-255">Choosing an appropriate PartitionKey</span></span>](#choosing-an-appropriate-partitionkey)
* [<span data-ttu-id="3c099-256">Optimera frågor för tabelltjänsten</span><span class="sxs-lookup"><span data-stu-id="3c099-256">Optimizing queries for the Table service</span></span>](#optimizing-queries-for-the-table-service)
* [<span data-ttu-id="3c099-257">Sortera data i tabelltjänsten</span><span class="sxs-lookup"><span data-stu-id="3c099-257">Sorting data in the Table service</span></span>](#sorting-data-in-the-table-service)

### <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a><span data-ttu-id="3c099-258">Hur du väljer PartitionKey och RowKey påverkar prestanda för frågor</span><span class="sxs-lookup"><span data-stu-id="3c099-258">How your choice of PartitionKey and RowKey impacts query performance</span></span>
<span data-ttu-id="3c099-259">Följande exempel förutsätter tabelltjänsten lagrar medarbetare entiteter med följande struktur (de flesta av exempel utelämna den **tidsstämpel** egenskapen för tydlighetens skull):</span><span class="sxs-lookup"><span data-stu-id="3c099-259">The following examples assume the table service is storing employee entities with the following structure (most of the examples omit the **Timestamp** property for clarity):</span></span>  

| <span data-ttu-id="3c099-260">*Kolumnnamn*</span><span class="sxs-lookup"><span data-stu-id="3c099-260">*Column name*</span></span> | <span data-ttu-id="3c099-261">*Datatyp*</span><span class="sxs-lookup"><span data-stu-id="3c099-261">*Data type*</span></span> |
| --- | --- |
| <span data-ttu-id="3c099-262">**PartitionKey** (avdelningsnamn)</span><span class="sxs-lookup"><span data-stu-id="3c099-262">**PartitionKey** (Department Name)</span></span> |<span data-ttu-id="3c099-263">Sträng</span><span class="sxs-lookup"><span data-stu-id="3c099-263">String</span></span> |
| <span data-ttu-id="3c099-264">**RowKey** (anställnings-Id)</span><span class="sxs-lookup"><span data-stu-id="3c099-264">**RowKey** (Employee Id)</span></span> |<span data-ttu-id="3c099-265">Sträng</span><span class="sxs-lookup"><span data-stu-id="3c099-265">String</span></span> |
| <span data-ttu-id="3c099-266">**Förnamn**</span><span class="sxs-lookup"><span data-stu-id="3c099-266">**FirstName**</span></span> |<span data-ttu-id="3c099-267">Sträng</span><span class="sxs-lookup"><span data-stu-id="3c099-267">String</span></span> |
| <span data-ttu-id="3c099-268">**Efternamn**</span><span class="sxs-lookup"><span data-stu-id="3c099-268">**LastName**</span></span> |<span data-ttu-id="3c099-269">Sträng</span><span class="sxs-lookup"><span data-stu-id="3c099-269">String</span></span> |
| <span data-ttu-id="3c099-270">**Ålder**</span><span class="sxs-lookup"><span data-stu-id="3c099-270">**Age**</span></span> |<span data-ttu-id="3c099-271">Integer</span><span class="sxs-lookup"><span data-stu-id="3c099-271">Integer</span></span> |
| <span data-ttu-id="3c099-272">**E-postadress**</span><span class="sxs-lookup"><span data-stu-id="3c099-272">**EmailAddress**</span></span> |<span data-ttu-id="3c099-273">Sträng</span><span class="sxs-lookup"><span data-stu-id="3c099-273">String</span></span> |

<span data-ttu-id="3c099-274">Avsnittet tidigare [översikt över Azure Table](#overview) beskrivs några av de viktigaste funktionerna i Azure Table-tjänsten som har en direkt inverkan på utformning för frågan.</span><span class="sxs-lookup"><span data-stu-id="3c099-274">The earlier section [Azure Table service overview](#overview) describes some of the key features of the Azure Table service that have a direct influence on designing for query.</span></span> <span data-ttu-id="3c099-275">Dessa resultera i följande allmänna riktlinjer för att utforma tabellen service frågor.</span><span class="sxs-lookup"><span data-stu-id="3c099-275">These result in the following general guidelines for designing Table service queries.</span></span> <span data-ttu-id="3c099-276">Observera att filtersyntaxen som används i exemplen nedan är från tabelltjänsten REST API för mer information finns i [fråga entiteter](http://msdn.microsoft.com/library/azure/dd179421.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c099-276">Note that the filter syntax used in the examples below is from the Table service REST API, for more information see [Query Entities](http://msdn.microsoft.com/library/azure/dd179421.aspx).</span></span>  

* <span data-ttu-id="3c099-277">En ***punkt frågan*** är den mest effektiva sökningen att använda och rekommenderas som ska användas för stora volymer sökningar eller sökningar som kräver lägsta svarstid.</span><span class="sxs-lookup"><span data-stu-id="3c099-277">A ***Point Query*** is the most efficient lookup to use and is recommended to be used for high-volume lookups or lookups requiring lowest latency.</span></span> <span data-ttu-id="3c099-278">Sådan fråga kan använda index för att hitta en enskild entitet mycket effektivt genom att ange både den **PartitionKey** och **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-278">Such a query can use the indexes to locate an individual entity very efficiently by specifying both the **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="3c099-279">Till exempel: $filter = (PartitionKey eq 'Sales') och (RowKey eq '2')</span><span class="sxs-lookup"><span data-stu-id="3c099-279">For example: $filter=(PartitionKey eq 'Sales') and (RowKey eq '2')</span></span>  
* <span data-ttu-id="3c099-280">Andra bäst är en ***intervallet frågan*** som använder den **PartitionKey** och filter på en mängd **RowKey** värden för att returnera mer än en entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-280">Second best is a ***Range Query*** that uses the **PartitionKey** and filters on a range of **RowKey** values to return more than one entity.</span></span> <span data-ttu-id="3c099-281">Den **PartitionKey** värdet identifierar en specifik partition och **RowKey** identifierar en delmängd av entiteter i partitionen.</span><span class="sxs-lookup"><span data-stu-id="3c099-281">The **PartitionKey** value identifies a specific partition, and the **RowKey** values identify a subset of the entities in that partition.</span></span> <span data-ttu-id="3c099-282">Till exempel: $filter = PartitionKey eq 'Försäljning och RowKey ge' och RowKey lt t '</span><span class="sxs-lookup"><span data-stu-id="3c099-282">For example: $filter=PartitionKey eq 'Sales' and RowKey ge 'S' and RowKey lt 'T'</span></span>  
* <span data-ttu-id="3c099-283">Tredje bästa är en ***Partition skanna*** som använder den **PartitionKey** och filter på en annan icke-nyckelegenskapen och som kan returnera flera enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-283">Third best is a ***Partition Scan*** that uses the **PartitionKey** and filters on another non-key property and that may return more than one entity.</span></span> <span data-ttu-id="3c099-284">Den **PartitionKey** värdet identifierar en specifik partition och egenskapen värden väljer för en delmängd av entiteter i partitionen.</span><span class="sxs-lookup"><span data-stu-id="3c099-284">The **PartitionKey** value identifies a specific partition, and the property values select for a subset of the entities in that partition.</span></span> <span data-ttu-id="3c099-285">Till exempel: $filter = PartitionKey eq 'Försäljning' och efternamn eq ”Smith”</span><span class="sxs-lookup"><span data-stu-id="3c099-285">For example: $filter=PartitionKey eq 'Sales' and LastName eq 'Smith'</span></span>  
* <span data-ttu-id="3c099-286">En ***tabell skanna*** innehåller inte den **PartitionKey** och är mycket ineffektiv eftersom den söker igenom alla partitioner som utgör din tabell i sin tur för alla matchande entiteter.</span><span class="sxs-lookup"><span data-stu-id="3c099-286">A ***Table Scan*** does not include the **PartitionKey** and is very inefficient because it searches all of the partitions that make up your table in turn for any matching entities.</span></span> <span data-ttu-id="3c099-287">Utför en tabellgenomsökning oavsett om huruvida filtret använder den **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="3c099-287">It will perform a table scan regardless of whether or not your filter uses the **RowKey**.</span></span> <span data-ttu-id="3c099-288">Till exempel: $filter = efternamn eq 'Karlsson'</span><span class="sxs-lookup"><span data-stu-id="3c099-288">For example: $filter=LastName eq 'Jones'</span></span>  
* <span data-ttu-id="3c099-289">Frågor som returnerar flera entiteter tillbaka dem i **PartitionKey** och **RowKey** ordning.</span><span class="sxs-lookup"><span data-stu-id="3c099-289">Queries that return multiple entities return them sorted in **PartitionKey** and **RowKey** order.</span></span> <span data-ttu-id="3c099-290">För att undvika sortera entiteter i klienten kan välja en **RowKey** som definierar de vanligaste sorteringsordning.</span><span class="sxs-lookup"><span data-stu-id="3c099-290">To avoid resorting the entities in the client, choose a **RowKey** that defines the most common sort order.</span></span>  

<span data-ttu-id="3c099-291">Observera att använda en ”**eller**” att ange ett filter baserat på **RowKey** värden resulterar i en partition genomsökning och inte behandlas som en fråga för intervallet.</span><span class="sxs-lookup"><span data-stu-id="3c099-291">Note that using an "**or**" to specify a filter based on **RowKey** values results in a partition scan and is not treated as a range query.</span></span> <span data-ttu-id="3c099-292">Därför bör du undvika frågor som använder filter exempelvis: $filter = PartitionKey eq 'Sales' och (RowKey eq '121' eller RowKey eq '322')</span><span class="sxs-lookup"><span data-stu-id="3c099-292">Therefore, you should avoid queries that use filters such as: $filter=PartitionKey eq 'Sales' and (RowKey eq '121' or RowKey eq '322')</span></span>  

<span data-ttu-id="3c099-293">Exempel på klientsidan kod som använder Storage-klientbiblioteket för att köra effektiva frågor finns:</span><span class="sxs-lookup"><span data-stu-id="3c099-293">For examples of client-side code that use the Storage Client Library to execute efficient queries, see:</span></span>  

* [<span data-ttu-id="3c099-294">Köra en punkt-fråga med Storage-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="3c099-294">Executing a point query using the Storage Client Library</span></span>](#executing-a-point-query-using-the-storage-client-library)
* [<span data-ttu-id="3c099-295">Hämtning av flera enheter med hjälp av LINQ</span><span class="sxs-lookup"><span data-stu-id="3c099-295">Retrieving multiple entities using LINQ</span></span>](#retrieving-multiple-entities-using-linq)
* [<span data-ttu-id="3c099-296">Projektion av serversidan</span><span class="sxs-lookup"><span data-stu-id="3c099-296">Server-side projection</span></span>](#server-side-projection)  

<span data-ttu-id="3c099-297">För exempel på klientsidan kod som kan hantera flera enhetstyper lagras i samma tabell, se:</span><span class="sxs-lookup"><span data-stu-id="3c099-297">For examples of client-side code that can handle multiple entity types stored in the same table, see:</span></span>  

* [<span data-ttu-id="3c099-298">Arbeta med heterogena entitetstyper</span><span class="sxs-lookup"><span data-stu-id="3c099-298">Working with heterogeneous entity types</span></span>](#working-with-heterogeneous-entity-types)  

### <a name="choosing-an-appropriate-partitionkey"></a><span data-ttu-id="3c099-299">Att välja en lämplig PartitionKey</span><span class="sxs-lookup"><span data-stu-id="3c099-299">Choosing an appropriate PartitionKey</span></span>
<span data-ttu-id="3c099-300">Ditt val av **PartitionKey** ska utjämna behöver kan du använda EGTs (för att säkerställa konsekvens) mot kravet att distribuera din entiteter över flera partitioner (för att säkerställa en skalbar lösning).</span><span class="sxs-lookup"><span data-stu-id="3c099-300">Your choice of **PartitionKey** should balance the need to enables the use of EGTs (to ensure consistency) against the requirement to distribute your entities across multiple partitions (to ensure a scalable solution).</span></span>  

<span data-ttu-id="3c099-301">Vid en extreme kan du lagra alla entiteter i en enda partition, men detta kan begränsa skalbarhet i din lösning och tabelltjänsten skulle förhindra att kunna belastningsutjämna förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="3c099-301">At one extreme, you could store all your entities in a single partition, but this may limit the scalability of your solution and would prevent the table service from being able to load-balance requests.</span></span> <span data-ttu-id="3c099-302">På det andra extremt kan du lagra en entitet per partition, vilket är mycket skalbart och som gör att tabelltjänsten för att belastningsutjämna förfrågningar, men som skulle kunna hindra att du använder enheten grupptransaktioner.</span><span class="sxs-lookup"><span data-stu-id="3c099-302">At the other extreme, you could store one entity per partition, which would be highly scalable and which enables the table service to load-balance requests, but which would prevent you from using entity group transactions.</span></span>  

<span data-ttu-id="3c099-303">Perfekt **PartitionKey** är en som gör att du kan använda effektiva frågor och som har tillräcklig partitioner för att se till att din lösning är skalbart.</span><span class="sxs-lookup"><span data-stu-id="3c099-303">An ideal **PartitionKey** is one that enables you to use efficient queries and that has sufficient partitions to ensure your solution is scalable.</span></span> <span data-ttu-id="3c099-304">Du hittar normalt att-enheterna kommer att ha en lämplig egenskap som distribuerar entiteter i tillräcklig partitioner.</span><span class="sxs-lookup"><span data-stu-id="3c099-304">Typically, you will find that your entities will have a suitable property that distributes your entities across sufficient partitions.</span></span>

> [!NOTE]
> <span data-ttu-id="3c099-305">I ett system som lagrar information om användare eller anställda kanske användar-ID för en bra PartitionKey.</span><span class="sxs-lookup"><span data-stu-id="3c099-305">For example, in a system that stores information about users or employees, UserID may be a good PartitionKey.</span></span> <span data-ttu-id="3c099-306">Du kan ha flera enheter som använder en viss användar-ID som partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="3c099-306">You may have several entities that use a given UserID as the partition key.</span></span> <span data-ttu-id="3c099-307">Varje entitet som lagrar data om en användare är grupperade i en enda partition och så dessa enheter är tillgängliga via entitet grupptransaktioner, samtidigt som de skalbara.</span><span class="sxs-lookup"><span data-stu-id="3c099-307">Each entity that stores data about a user is grouped into a single partition, and so these entities are accessible via entity group transactions, while still being highly scalable.</span></span>
> 
> 

<span data-ttu-id="3c099-308">Det finns fler överväganden i valfri **PartitionKey** som relaterar till hur du ska infoga, uppdatera och ta bort entiteter: finns i avsnittet [Design för dataändring](#design-for-data-modification) nedan.</span><span class="sxs-lookup"><span data-stu-id="3c099-308">There are additional considerations in your choice of **PartitionKey** that relate to how you will insert, update, and delete entities: see the section [Design for data modification](#design-for-data-modification) below.</span></span>  

### <a name="optimizing-queries-for-the-table-service"></a><span data-ttu-id="3c099-309">Optimera frågor för tabelltjänsten</span><span class="sxs-lookup"><span data-stu-id="3c099-309">Optimizing queries for the Table service</span></span>
<span data-ttu-id="3c099-310">Tabelltjänsten indexerar automatiskt dina enheter med hjälp av den **PartitionKey** och **RowKey** värden i ett enda grupperat index, därför orsaken till att peka frågor är det mest effektiva sättet att använda.</span><span class="sxs-lookup"><span data-stu-id="3c099-310">The Table service automatically indexes your entities using the **PartitionKey** and **RowKey** values in a single clustered index, hence the reason that point queries are the most efficient to use.</span></span> <span data-ttu-id="3c099-311">Det finns dock ingen index än det som i det grupperade indexet på den **PartitionKey** och **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="3c099-311">However, there are no indexes other than that on the clustered index on the **PartitionKey** and **RowKey**.</span></span>

<span data-ttu-id="3c099-312">Många-Designer måste uppfylla kraven för att aktivera sökning efter enheter baserat på flera villkor.</span><span class="sxs-lookup"><span data-stu-id="3c099-312">Many designs must meet requirements to enable lookup of entities based on multiple criteria.</span></span> <span data-ttu-id="3c099-313">Hitta medarbetare enheter baserat på e-post, till exempel anställnings-id eller efternamn.</span><span class="sxs-lookup"><span data-stu-id="3c099-313">For example, locating employee entities based on email, employee id, or last name.</span></span> <span data-ttu-id="3c099-314">Följande mönster i avsnittet [tabell designmönster](#table-design-patterns) dessa typer av krav och beskriver fungerar runt det faktum att tabelltjänsten inte ger sekundärindex på olika sätt:</span><span class="sxs-lookup"><span data-stu-id="3c099-314">The following patterns in the section [Table Design Patterns](#table-design-patterns) address these types of requirement and describe ways of working around the fact that the Table service does not provide secondary indexes:</span></span>  

* <span data-ttu-id="3c099-315">[Intra-partition sekundärt index mönster](#intra-partition-secondary-index-pattern) -lagra flera kopior av varje enhet med hjälp av olika **RowKey** värden (i samma partition) för att aktivera snabb och effektiv sökningar och alternativa sorteringsordningen genom att använda olika **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-315">[Intra-partition secondary index pattern](#intra-partition-secondary-index-pattern) - Store multiple copies of each entity using different **RowKey** values (in the same partition) to enable fast and efficient lookups and alternate sort orders by using different **RowKey** values.</span></span>  
* <span data-ttu-id="3c099-316">[Mellan sekundära Partitionsindex mönster](#inter-partition-secondary-index-pattern) – lagra flera kopior av varje entitet som använder olika RowKey värden i olika partitioner eller i separata tabeller för att aktivera snabb och effektiv sökningar och alternativa sorteringen sorterar med hjälp av olika **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-316">[Inter-partition secondary index pattern](#inter-partition-secondary-index-pattern) - Store multiple copies of each entity using different RowKey values in separate partitions or in separate tables to enable fast and efficient lookups and alternate sort orders by using different **RowKey** values.</span></span>  
* <span data-ttu-id="3c099-317">[Index entiteter mönster](#index-entities-pattern) -Underhåll index enheter om du vill aktivera effektiva sökningar som returnerar en lista över enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-317">[Index Entities Pattern](#index-entities-pattern) - Maintain index entities to enable efficient searches that return lists of entities.</span></span>  

### <a name="sorting-data-in-the-table-service"></a><span data-ttu-id="3c099-318">Sortera data i tabelltjänsten</span><span class="sxs-lookup"><span data-stu-id="3c099-318">Sorting data in the Table service</span></span>
<span data-ttu-id="3c099-319">Tabelltjänsten returnerar entiteter sorterade i stigande ordning utifrån **PartitionKey** och sedan efter **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="3c099-319">The Table service returns entities sorted in ascending order based on **PartitionKey** and then by **RowKey**.</span></span> <span data-ttu-id="3c099-320">Nycklarna är strängvärden och för att säkerställa att numeriska värden sorteras korrekt, bör du konvertera dem till en fast längd och Fyll ut dem med nollor.</span><span class="sxs-lookup"><span data-stu-id="3c099-320">These keys are string values and to ensure that numeric values sort correctly, you should convert them to a fixed length and pad them with zeroes.</span></span> <span data-ttu-id="3c099-321">Om medarbetaren ID-värde som du använder som till exempel den **RowKey** är ett heltal, bör du konvertera anställnings-id **123** till **00000123**.</span><span class="sxs-lookup"><span data-stu-id="3c099-321">For example, if the employee id value you use as the **RowKey** is an integer value, you should convert employee id **123** to **00000123**.</span></span>  

<span data-ttu-id="3c099-322">Många program har krav för att använda data sorteras i olika ordning: till exempel sortera anställda efter namn eller ansluta till datum.</span><span class="sxs-lookup"><span data-stu-id="3c099-322">Many applications have requirements to use data sorted in different orders: for example, sorting employees by name, or by joining date.</span></span> <span data-ttu-id="3c099-323">Följande mönster i avsnittet [tabell designmönster](#table-design-patterns) adress så alternativa sorteringsordningar för dina enheter:</span><span class="sxs-lookup"><span data-stu-id="3c099-323">The following patterns in the section [Table Design Patterns](#table-design-patterns) address how to alternate sort orders for your entities:</span></span>  

* <span data-ttu-id="3c099-324">[Intra-partition sekundärt index mönster](#intra-partition-secondary-index-pattern) – lagra flera kopior av varje entitet som använder olika RowKey värden (i samma partition) för att aktivera snabb och effektiv sökningar och alternativa sorteringen sorterar med hjälp av olika RowKey värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-324">[Intra-partition secondary index pattern](#intra-partition-secondary-index-pattern) - Store multiple copies of each entity using different RowKey values (in the same partition) to enable fast and efficient lookups and alternate sort orders by using different RowKey values.</span></span>  
* <span data-ttu-id="3c099-325">[Mellan sekundära Partitionsindex mönster](#inter-partition-secondary-index-pattern) – lagra flera kopior av varje entitet med olika RowKey värden för olika partitioner i separata tabeller för att aktivera snabb och effektiv sökningar och alternativa sorteringen sorterar med hjälp av olika RowKey värden .</span><span class="sxs-lookup"><span data-stu-id="3c099-325">[Inter-partition secondary index pattern](#inter-partition-secondary-index-pattern) - Store multiple copies of each entity using different RowKey values in separate partitions in separate tables to enable fast and efficient lookups and alternate sort orders by using different RowKey values.</span></span>
* <span data-ttu-id="3c099-326">[Loggen pilslut mönster](#log-tail-pattern) -hämta den  *n*  entiteter som senast lades till en partition med hjälp av en **RowKey** värde som sorterar i omvänd datum och tid ordning.</span><span class="sxs-lookup"><span data-stu-id="3c099-326">[Log tail pattern](#log-tail-pattern) - Retrieve the *n* entities most recently added to a partition by using a **RowKey** value that sorts in reverse date and time order.</span></span>  

## <a name="design-for-data-modification"></a><span data-ttu-id="3c099-327">Design för dataändring</span><span class="sxs-lookup"><span data-stu-id="3c099-327">Design for data modification</span></span>
<span data-ttu-id="3c099-328">Det här avsnittet fokuserar på designöverväganden för att optimera infogningar, uppdateringar, och tar bort.</span><span class="sxs-lookup"><span data-stu-id="3c099-328">This section focuses on the design considerations for optimizing inserts, updates, and deletes.</span></span> <span data-ttu-id="3c099-329">I vissa fall behöver du utvärdera kompromissen mellan Designer optimerar för frågor mot Designer optimerar för dataändring precis som i Designer för relationsdatabaser (även om metoder för att hantera design avvägningarna är olika i en relationsdatabas).</span><span class="sxs-lookup"><span data-stu-id="3c099-329">In some cases, you will need to evaluate the trade-off between designs that optimize for querying against designs that optimize for data modification just as you do in designs for relational databases (although the techniques for managing the design trade-offs are different in a relational database).</span></span> <span data-ttu-id="3c099-330">Avsnittet [tabell designmönster](#table-design-patterns) beskriver vissa detaljerad designmönster för tabelltjänsten och beskrivs några dessa avvägningarna.</span><span class="sxs-lookup"><span data-stu-id="3c099-330">The section [Table Design Patterns](#table-design-patterns) describes some detailed design patterns for the Table service and highlights some these trade-offs.</span></span> <span data-ttu-id="3c099-331">Du hittar många Designer som optimerats för att fråga entiteter också fungerar bra för att ändra entiteter i praktiken.</span><span class="sxs-lookup"><span data-stu-id="3c099-331">In practice, you will find that many designs optimized for querying entities also work well for modifying entities.</span></span>  

### <a name="optimizing-the-performance-of-insert-update-and-delete-operations"></a><span data-ttu-id="3c099-332">Optimera prestanda för insert-, update och delete-åtgärder</span><span class="sxs-lookup"><span data-stu-id="3c099-332">Optimizing the performance of insert, update, and delete operations</span></span>
<span data-ttu-id="3c099-333">Om du vill uppdatera eller ta bort en entitet, du måste kunna identifieras med hjälp av den **PartitionKey** och **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-333">To update or delete an entity, you must be able to identify it by using the **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="3c099-334">I detta avseende valet av **PartitionKey** och **RowKey** för ändra entiteter bör följa liknande kriterier till ditt val att stödja punkt frågor eftersom du vill identifiera enheter som effektiv som möjligt.</span><span class="sxs-lookup"><span data-stu-id="3c099-334">In this respect, your choice of **PartitionKey** and **RowKey** for modifying entities should follow similar criteria to your choice to support point queries because you want to identify entities as efficiently as possible.</span></span> <span data-ttu-id="3c099-335">Du inte vill använda en ineffektiv partition eller tabell sökning för att hitta en enhet för att identifiera den **PartitionKey** och **RowKey** värden som du behöver uppdatera eller ta bort den.</span><span class="sxs-lookup"><span data-stu-id="3c099-335">You do not want to use an inefficient partition or table scan to locate an entity in order to discover the **PartitionKey** and **RowKey** values you need to update or delete it.</span></span>  

<span data-ttu-id="3c099-336">Följande mönster i avsnittet [tabell designmönster](#table-design-patterns) adress optimera prestanda eller din insert, update och delete-åtgärder:</span><span class="sxs-lookup"><span data-stu-id="3c099-336">The following patterns in the section [Table Design Patterns](#table-design-patterns) address optimizing the performance or your insert, update, and delete operations:</span></span>  

* <span data-ttu-id="3c099-337">[Hög volym ta bort mönster](#high-volume-delete-pattern) -Aktivera borttagning av ett stort antal enheter genom att lagra alla entiteter för samtidiga borttagning i sina egna separata tabeller; ta bort entiteterna genom att ta bort tabellen.</span><span class="sxs-lookup"><span data-stu-id="3c099-337">[High volume delete pattern](#high-volume-delete-pattern) - Enable the deletion of a high volume of entities by storing all the entities for simultaneous deletion in their own separate table; you delete the entities by deleting the table.</span></span>  
* <span data-ttu-id="3c099-338">[Serien datamönster](#data-series-pattern) -Store fullständig dataserier i en enda enhet för att minimera antalet begäranden som du gör.</span><span class="sxs-lookup"><span data-stu-id="3c099-338">[Data series pattern](#data-series-pattern) - Store complete data series in a single entity to minimize the number of requests you make.</span></span>  
* <span data-ttu-id="3c099-339">[Wide entiteter mönster](#wide-entities-pattern) -använda flera fysiska enheter för att lagra logiska entiteter med mer än 252 egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3c099-339">[Wide entities pattern](#wide-entities-pattern) - Use multiple physical entities to store logical entities with more than 252 properties.</span></span>  
* <span data-ttu-id="3c099-340">[Mönster för stora entiteter](#large-entities-pattern) -använda blob storage för att lagra stora egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="3c099-340">[Large entities pattern](#large-entities-pattern) - Use blob storage to store large property values.</span></span>  

### <a name="ensuring-consistency-in-your-stored-entities"></a><span data-ttu-id="3c099-341">Säkerställa konsekvens i lagrade entiteter</span><span class="sxs-lookup"><span data-stu-id="3c099-341">Ensuring consistency in your stored entities</span></span>
<span data-ttu-id="3c099-342">Den viktiga faktor som påverkar ditt val av nycklar för att optimera dataändringar är att säkerställa konsekvens med hjälp av atomiska transaktioner.</span><span class="sxs-lookup"><span data-stu-id="3c099-342">The other key factor that influences your choice of keys for optimizing data modifications is how to ensure consistency by using atomic transactions.</span></span> <span data-ttu-id="3c099-343">Du kan bara använda en EGT ska fungera på entiteter som lagras i samma partition.</span><span class="sxs-lookup"><span data-stu-id="3c099-343">You can only use an EGT to operate on entities stored in the same partition.</span></span>  

<span data-ttu-id="3c099-344">Följande mönster i avsnittet [tabell designmönster](#table-design-patterns) adress hantera konsekvens:</span><span class="sxs-lookup"><span data-stu-id="3c099-344">The following patterns in the section [Table Design Patterns](#table-design-patterns) address managing consistency:</span></span>  

* <span data-ttu-id="3c099-345">[Intra-partition sekundärt index mönster](#intra-partition-secondary-index-pattern) -lagra flera kopior av varje enhet med hjälp av olika **RowKey** värden (i samma partition) för att aktivera snabb och effektiv sökningar och alternativa sorteringsordningen genom att använda olika **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-345">[Intra-partition secondary index pattern](#intra-partition-secondary-index-pattern) - Store multiple copies of each entity using different **RowKey** values (in the same partition) to enable fast and efficient lookups and alternate sort orders by using different **RowKey** values.</span></span>  
* <span data-ttu-id="3c099-346">[Mellan sekundära Partitionsindex mönster](#inter-partition-secondary-index-pattern) – lagra flera kopior av varje entitet som använder olika RowKey värden i olika partitioner eller i separata tabeller för att aktivera snabb och effektiv sökningar och alternativa sorteringen sorterar med hjälp av olika **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-346">[Inter-partition secondary index pattern](#inter-partition-secondary-index-pattern) - Store multiple copies of each entity using different RowKey values in separate partitions or in separate tables to enable fast and efficient lookups and alternate sort orders by using different **RowKey** values.</span></span>  
* <span data-ttu-id="3c099-347">[Överensstämmelse transaktioner mönster](#eventually-consistent-transactions-pattern) -aktivera överensstämmelse beteende mellan partitionsgränser eller lagring system gränser med hjälp av Azure köer.</span><span class="sxs-lookup"><span data-stu-id="3c099-347">[Eventually consistent transactions pattern](#eventually-consistent-transactions-pattern) - Enable eventually consistent behavior across partition boundaries or storage system boundaries by using Azure queues.</span></span>
* <span data-ttu-id="3c099-348">[Index entiteter mönster](#index-entities-pattern) -Underhåll index enheter om du vill aktivera effektiva sökningar som returnerar en lista över enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-348">[Index Entities Pattern](#index-entities-pattern) - Maintain index entities to enable efficient searches that return lists of entities.</span></span>  
* <span data-ttu-id="3c099-349">[Denormalization mönster](#denormalization-pattern) -kombinera relaterade data tillsammans i en enda enhet så att du kan hämta alla data som du behöver med en enda fråga.</span><span class="sxs-lookup"><span data-stu-id="3c099-349">[Denormalization pattern](#denormalization-pattern) - Combine related data together in a single entity to enable you to retrieve all the data you need with a single point query.</span></span>  
* <span data-ttu-id="3c099-350">[Serien datamönster](#data-series-pattern) -Store fullständig dataserier i en enda enhet för att minimera antalet begäranden som du gör.</span><span class="sxs-lookup"><span data-stu-id="3c099-350">[Data series pattern](#data-series-pattern) - Store complete data series in a single entity to minimize the number of requests you make.</span></span>  

<span data-ttu-id="3c099-351">Information om entiteten grupptransaktioner finns i avsnittet [entitet gruppera transaktioner](#entity-group-transactions).</span><span class="sxs-lookup"><span data-stu-id="3c099-351">For information about entity group transactions, see the section [Entity Group Transactions](#entity-group-transactions).</span></span>  

### <a name="ensuring-your-design-for-efficient-modifications-facilitates-efficient-queries"></a><span data-ttu-id="3c099-352">Säkerställa en design för effektiv ändringar underlättar effektiv frågor</span><span class="sxs-lookup"><span data-stu-id="3c099-352">Ensuring your design for efficient modifications facilitates efficient queries</span></span>
<span data-ttu-id="3c099-353">I många fall bör alltid en design för effektiva frågor ger effektiv ändringar, men du utvärdera om så är fallet för din situation.</span><span class="sxs-lookup"><span data-stu-id="3c099-353">In many cases, a design for efficient querying results in efficient modifications, but you should always evaluate whether this is the case for your specific scenario.</span></span> <span data-ttu-id="3c099-354">Några av mönster i avsnittet [tabell designmönster](#table-design-patterns) explicit utvärdera avvägningarna mellan fråga och ändra entiteter och du bör alltid beakta numret för varje typ av åtgärd.</span><span class="sxs-lookup"><span data-stu-id="3c099-354">Some of the patterns in the section [Table Design Patterns](#table-design-patterns) explicitly evaluate trade-offs between querying and modifying entities, and you should always take into account the number of each type of operation.</span></span>  

<span data-ttu-id="3c099-355">Följande mönster i avsnittet [tabell designmönster](#table-design-patterns) adressen avvägningarna mellan utformning för effektiva frågor och designar för effektiv dataändring:</span><span class="sxs-lookup"><span data-stu-id="3c099-355">The following patterns in the section [Table Design Patterns](#table-design-patterns) address trade-offs between designing for efficient queries and designing for efficient data modification:</span></span>  

* <span data-ttu-id="3c099-356">[Sammansatt nyckel mönster](#compound-key-pattern) -Använd sammansatta **RowKey** värden att aktivera en klient att söka efter relaterade data med en enda fråga.</span><span class="sxs-lookup"><span data-stu-id="3c099-356">[Compound key pattern](#compound-key-pattern) - Use compound **RowKey** values to enable a client to lookup related data with a single point query.</span></span>  
* <span data-ttu-id="3c099-357">[Loggen pilslut mönster](#log-tail-pattern) -hämta den  *n*  entiteter som senast lades till en partition med hjälp av en **RowKey** värde som sorterar i omvänd datum och tid ordning.</span><span class="sxs-lookup"><span data-stu-id="3c099-357">[Log tail pattern](#log-tail-pattern) - Retrieve the *n* entities most recently added to a partition by using a **RowKey** value that sorts in reverse date and time order.</span></span>  

## <a name="encrypting-table-data"></a><span data-ttu-id="3c099-358">Kryptera tabelldata</span><span class="sxs-lookup"><span data-stu-id="3c099-358">Encrypting Table Data</span></span>
<span data-ttu-id="3c099-359">.NET Azure Storage-klientbibliotek har stöd för kryptering av strängen Entitetsegenskaper för insert och ersätt-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3c099-359">The .NET Azure Storage Client Library supports encryption of string entity properties for insert and replace operations.</span></span> <span data-ttu-id="3c099-360">Krypterade strängar lagras på tjänsten som binära egenskaper och de konverteras till strängar efter dekrypteringen.</span><span class="sxs-lookup"><span data-stu-id="3c099-360">The encrypted strings are stored on the service as binary properties, and they are converted back to strings after decryption.</span></span>    

<span data-ttu-id="3c099-361">För tabeller, förutom krypteringsprincipen, måste användare ange egenskaper som ska krypteras.</span><span class="sxs-lookup"><span data-stu-id="3c099-361">For tables, in addition to the encryption policy, users must specify the properties to be encrypted.</span></span> <span data-ttu-id="3c099-362">Detta kan göras genom att antingen att ange attributet [EncryptProperty] (för POCO entiteter som är härledda från TableEntity) eller en kryptering matcharen begäran alternativ.</span><span class="sxs-lookup"><span data-stu-id="3c099-362">This can be done by either specifying an [EncryptProperty] attribute (for POCO entities that derive from TableEntity) or an encryption resolver in request options.</span></span> <span data-ttu-id="3c099-363">En kryptering Konfliktlösaren är en delegat som tar en partitionsnyckel, radnyckel och egenskapsnamn och returnerar ett booleskt värde som anger om egenskapen ska krypteras.</span><span class="sxs-lookup"><span data-stu-id="3c099-363">An encryption resolver is a delegate that takes a partition key, row key, and property name and returns a Boolean that indicates whether that property should be encrypted.</span></span> <span data-ttu-id="3c099-364">Under krypteringen använder klientbiblioteket den här informationen för att avgöra om en egenskap ska krypteras vid skrivning till kabeln.</span><span class="sxs-lookup"><span data-stu-id="3c099-364">During encryption, the client library will use this information to decide whether a property should be encrypted while writing to the wire.</span></span> <span data-ttu-id="3c099-365">Delegaten ger också möjlighet att logik runt hur egenskaperna är krypterade.</span><span class="sxs-lookup"><span data-stu-id="3c099-365">The delegate also provides for the possibility of logic around how properties are encrypted.</span></span> <span data-ttu-id="3c099-366">(Till exempel om X, sedan kryptera egenskapen A; annars kryptera egenskaper A och b) Observera att det inte nödvändigt att ange den här informationen vid läsning eller fråga entiteter.</span><span class="sxs-lookup"><span data-stu-id="3c099-366">(For example, if X, then encrypt property A; otherwise encrypt properties A and B.) Note that it is not necessary to provide this information while reading or querying entities.</span></span>

<span data-ttu-id="3c099-367">Observera att koppling inte stöds för närvarande.</span><span class="sxs-lookup"><span data-stu-id="3c099-367">Note that merge is not currently supported.</span></span> <span data-ttu-id="3c099-368">Eftersom en delmängd av egenskaper kan har krypterats tidigare med hjälp av en annan nyckel leder bara sammanslagning nya egenskaper och uppdaterar metadata till dataförlust.</span><span class="sxs-lookup"><span data-stu-id="3c099-368">Since a subset of properties may have been encrypted previously using a different key, simply merging the new properties and updating the metadata will result in data loss.</span></span> <span data-ttu-id="3c099-369">Sammanslagning av antingen kräver extra service-anrop för att läsa den befintliga enheten från tjänsten eller med en ny nyckel för varje egenskap som lämpar sig inte av prestandaskäl.</span><span class="sxs-lookup"><span data-stu-id="3c099-369">Merging either requires making extra service calls to read the pre-existing entity from the service, or using a new key per property, both of which are not suitable for performance reasons.</span></span>     

<span data-ttu-id="3c099-370">Information om hur du krypterar tabelldata finns [kryptering på klientsidan och Azure Key Vault för Microsoft Azure Storage](../storage/common/storage-client-side-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="3c099-370">For information about encrypting table data, see [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](../storage/common/storage-client-side-encryption.md).</span></span>  

## <a name="modelling-relationships"></a><span data-ttu-id="3c099-371">Modellering relationer</span><span class="sxs-lookup"><span data-stu-id="3c099-371">Modelling relationships</span></span>
<span data-ttu-id="3c099-372">Skapa domänmodeller är ett viktigt steg i utformningen av komplexa system.</span><span class="sxs-lookup"><span data-stu-id="3c099-372">Building domain models is a key step in the design of complex systems.</span></span> <span data-ttu-id="3c099-373">Normalt använder du modellering processen för att identifiera enheter och relationer mellan dem som ett sätt att förstå business-domänen och informera designen av systemet.</span><span class="sxs-lookup"><span data-stu-id="3c099-373">Typically, you use the modelling process to identify entities and the relationships between them as a way to understand the business domain and inform the design of your system.</span></span> <span data-ttu-id="3c099-374">Det här avsnittet fokuserar på hur du kan översätta några av de vanliga relation hittades i domänmodeller för Designer för tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-374">This section focuses on how you can translate some of the common relationship types found in domain models to designs for the Table service.</span></span> <span data-ttu-id="3c099-375">Processen för mappning från en logisk datamodell till en fysisk NoSQL baserad-datamodell är väldigt annorlunda än den som används när du skapar en relationsdatabas.</span><span class="sxs-lookup"><span data-stu-id="3c099-375">The process of mapping from a logical data-model to a physical NoSQL based data-model is very different from that used when designing a relational database.</span></span> <span data-ttu-id="3c099-376">Relationsdatabaser design förutsätter vanligtvis en process för normalisering som har optimerats för att minimera redundans – och en deklarativ frågar funktion som avlägsnar hur implementeringen av hur databasen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3c099-376">Relational databases design typically assumes a data normalization process optimized for minimizing redundancy – and a declarative querying capability that abstracts how the implementation of how the database works.</span></span>  

### <a name="one-to-many-relationships"></a><span data-ttu-id="3c099-377">En-till-många-relationer</span><span class="sxs-lookup"><span data-stu-id="3c099-377">One-to-many relationships</span></span>
<span data-ttu-id="3c099-378">En-till-många-relationer mellan företag domänobjekt inträffa ofta: till exempel en avdelning har många anställda.</span><span class="sxs-lookup"><span data-stu-id="3c099-378">One-to-many relationships between business domain objects occur very frequently: for example, one department has many employees.</span></span> <span data-ttu-id="3c099-379">Det finns flera sätt att implementera en-till-många-relationer i tabelltjänsten varje med för- och nackdelar som är relevanta för ett visst scenario.</span><span class="sxs-lookup"><span data-stu-id="3c099-379">There are several ways to implement one-to-many relationships in the Table service each with pros and cons that may be relevant to the particular scenario.</span></span>  

<span data-ttu-id="3c099-380">Studera exemplet i flera nationella stora företag med tusentals avdelningar och medarbetare enheter där varje avdelning har många anställda och varje medarbetare som tillhör en viss avdelning.</span><span class="sxs-lookup"><span data-stu-id="3c099-380">Consider the example of a large multi-national corporation with tens of thousands of departments and employee entities where every department has many employees and each employee as associated with one specific department.</span></span> <span data-ttu-id="3c099-381">En metod är att lagra separat avdelning och medarbetare entiteter som dessa:</span><span class="sxs-lookup"><span data-stu-id="3c099-381">One approach is to store separate department and employee entities such as these:</span></span>  

![][1]

<span data-ttu-id="3c099-382">Det här exemplet illustrerar en implicit en-till-många-relation mellan de typer som är baserat på den **PartitionKey** värde.</span><span class="sxs-lookup"><span data-stu-id="3c099-382">This example shows an implicit one-to-many relationship between the types based on the **PartitionKey** value.</span></span> <span data-ttu-id="3c099-383">Varje avdelning kan ha många anställda.</span><span class="sxs-lookup"><span data-stu-id="3c099-383">Each department can have many employees.</span></span>  

<span data-ttu-id="3c099-384">Det här exemplet visar också en avdelning entiteten och dess relaterade medarbetare entiteter i samma partition.</span><span class="sxs-lookup"><span data-stu-id="3c099-384">This example also shows a department entity and its related employee entities in the same partition.</span></span> <span data-ttu-id="3c099-385">Du kan välja att använda olika partitioner, tabeller och även lagringskonton för olika enhetstyper.</span><span class="sxs-lookup"><span data-stu-id="3c099-385">You could choose to use different partitions, tables, or even storage accounts for the different entity types.</span></span>  

<span data-ttu-id="3c099-386">En annan metod är att denormalize dina data och lagrar bara medarbetare entiteter med Avnormaliserade avdelningsdata som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="3c099-386">An alternative approach is to denormalize your data and store only employee entities with denormalized department data as shown in the following example.</span></span> <span data-ttu-id="3c099-387">I det här scenariot kanske kan då Avnormaliserade inte bäst om du har ett krav för att kunna ändra information om en hanterare för avdelning eftersom om du vill göra detta måste du uppdatera alla medarbetare i avdelningen.</span><span class="sxs-lookup"><span data-stu-id="3c099-387">In this particular scenario, this denormalized approach may not be the best if you have a requirement to be able to change the details of a department manager because to do this you need to update every employee in the department.</span></span>  

![][2]

<span data-ttu-id="3c099-388">Mer information finns i [Denormalization mönster](#denormalization-pattern) senare i den här guiden.</span><span class="sxs-lookup"><span data-stu-id="3c099-388">For more information, see the [Denormalization pattern](#denormalization-pattern) later in this guide.</span></span>  

<span data-ttu-id="3c099-389">I följande tabell sammanfattas för- och nackdelarna med vart och ett av de metoder som beskrivs ovan för att lagra medarbetare och avdelningen enheter som har en en-till-många-relation.</span><span class="sxs-lookup"><span data-stu-id="3c099-389">The following table summarizes the pros and cons of each of the approaches outlined above for storing employee and department entities that have a one-to-many relationship.</span></span> <span data-ttu-id="3c099-390">Det bör också övervägas hur ofta du förväntar dig att utföra olika åtgärder: kan det vara acceptabelt att ha en design som innehåller en kostsam åtgärd om åtgärden endast inträffar sällan.</span><span class="sxs-lookup"><span data-stu-id="3c099-390">You should also consider how often you expect to perform various operations: it may be acceptable to have a design that includes an expensive operation if that operation only happens infrequently.</span></span>  

<table>
<tr>
<th><span data-ttu-id="3c099-391">Metoden</span><span class="sxs-lookup"><span data-stu-id="3c099-391">Approach</span></span></th>
<th><span data-ttu-id="3c099-392">Tekniker</span><span class="sxs-lookup"><span data-stu-id="3c099-392">Pros</span></span></th>
<th><span data-ttu-id="3c099-393">Nackdelar</span><span class="sxs-lookup"><span data-stu-id="3c099-393">Cons</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="3c099-394">Separata entitetstyper, samma partition i samma tabell</span><span class="sxs-lookup"><span data-stu-id="3c099-394">Separate entity types, same partition, same table</span></span></td>
<td>
<ul>
<li><span data-ttu-id="3c099-395">Du kan uppdatera en avdelning entitet med en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="3c099-395">You can update a department entity with a single operation.</span></span></li>
<li><span data-ttu-id="3c099-396">Du kan använda en EGT för att upprätthålla enhetliga om du har ett krav för att ändra en avdelning enhet när du uppdatering/insert/ta bort en medarbetare entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-396">You can use an EGT to maintain consistency if you have a requirement to modify a department entity whenever you update/insert/delete an employee entity.</span></span> <span data-ttu-id="3c099-397">Till exempel om du har ett antal avdelningens anställda för varje avdelning.</span><span class="sxs-lookup"><span data-stu-id="3c099-397">For example if you maintain a departmental employee count for each department.</span></span></li>
</ul>
</td>
<td>
<ul>
<li><span data-ttu-id="3c099-398">Du kan behöva hämta både anställda och en avdelning entitet för vissa aktiviteter för klienten.</span><span class="sxs-lookup"><span data-stu-id="3c099-398">You may need to retrieve both an employee and a department entity for some client activities.</span></span></li>
<li><span data-ttu-id="3c099-399">Storage-åtgärder sker i samma partition.</span><span class="sxs-lookup"><span data-stu-id="3c099-399">Storage operations happen in the same partition.</span></span> <span data-ttu-id="3c099-400">Vid höga transaktionsvolymer kan detta resultera i en hotspot.</span><span class="sxs-lookup"><span data-stu-id="3c099-400">At high transaction volumes, this may result in a hotspot.</span></span></li>
<li><span data-ttu-id="3c099-401">Du kan inte flytta en medarbetare till en ny avdelning med hjälp av en EGT.</span><span class="sxs-lookup"><span data-stu-id="3c099-401">You cannot move an employee to a new department using an EGT.</span></span></li>
</ul>
</td>
</tr>
<tr>
<td><span data-ttu-id="3c099-402">Separata entitetstyper, olika partitioner eller tabeller storage-konton</span><span class="sxs-lookup"><span data-stu-id="3c099-402">Separate entity types, different partitions or tables or storage accounts</span></span></td>
<td>
<ul>
<li><span data-ttu-id="3c099-403">Du kan uppdatera en avdelning entitet eller medarbetare enhet med en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="3c099-403">You can update a department entity or employee entity with a single operation.</span></span></li>
<li><span data-ttu-id="3c099-404">Vid höga transaktionsvolymer att detta sprida belastningen över flera partitioner.</span><span class="sxs-lookup"><span data-stu-id="3c099-404">At high transaction volumes, this may help spread the load across more partitions.</span></span></li>
</ul>
</td>
<td>
<ul>
<li><span data-ttu-id="3c099-405">Du kan behöva hämta både anställda och en avdelning entitet för vissa aktiviteter för klienten.</span><span class="sxs-lookup"><span data-stu-id="3c099-405">You may need to retrieve both an employee and a department entity for some client activities.</span></span></li>
<li><span data-ttu-id="3c099-406">Du kan inte använda EGTs för att underhålla konsekvensen när du uppdatering/insert/ta bort en medarbetare och uppdatera en avdelning.</span><span class="sxs-lookup"><span data-stu-id="3c099-406">You cannot use EGTs to maintain consistency when you update/insert/delete an employee and update a department.</span></span> <span data-ttu-id="3c099-407">Till exempel uppdaterar ett antal anställda i en avdelning entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-407">For example, updating an employee count in a department entity.</span></span></li>
<li><span data-ttu-id="3c099-408">Du kan inte flytta en medarbetare till en ny avdelning med hjälp av en EGT.</span><span class="sxs-lookup"><span data-stu-id="3c099-408">You cannot move an employee to a new department using an EGT.</span></span></li>
</ul>
</td>
</tr>
<tr>
<td><span data-ttu-id="3c099-409">Denormalize till samma enhetstyp</span><span class="sxs-lookup"><span data-stu-id="3c099-409">Denormalize into single entity type</span></span></td>
<td>
<ul>
<li><span data-ttu-id="3c099-410">Du kan hämta all information du behöver med en enskild begäran.</span><span class="sxs-lookup"><span data-stu-id="3c099-410">You can retrieve all the information you need with a single request.</span></span></li>
</ul>
</td>
<td>
<ul>
<li><span data-ttu-id="3c099-411">Det kan vara dyra att upprätthålla enhetliga om du behöver uppdatera avdelning information (det här kräver du uppdatera alla anställda i en avdelning).</span><span class="sxs-lookup"><span data-stu-id="3c099-411">It may be expensive to maintain consistency if you need to update department information (this would require you to update all the employees in a department).</span></span></li>
</ul>
</td>
</tr>
</table>

<span data-ttu-id="3c099-412">Hur du väljer mellan dessa alternativ och vilka av för- och nackdelar som är störst, beror på dina specifika Programscenarier.</span><span class="sxs-lookup"><span data-stu-id="3c099-412">How you choose between these options, and which of the pros and cons are most significant, depends on your specific application scenarios.</span></span> <span data-ttu-id="3c099-413">Till exempel hur ofta du ändrar avdelning entiteter; behöver alla medarbetare frågor kan ytterligare information som avdelningsnivå; hur nära kommer du skalbarhetsgränser på partitioner eller storage-konto?</span><span class="sxs-lookup"><span data-stu-id="3c099-413">For example, how often do you modify department entities; do all your employee queries need the additional departmental information; how close are you to the scalability limits on your partitions or your storage account?</span></span>  

### <a name="one-to-one-relationships"></a><span data-ttu-id="3c099-414">1: 1-relationer</span><span class="sxs-lookup"><span data-stu-id="3c099-414">One-to-one relationships</span></span>
<span data-ttu-id="3c099-415">Domänmodeller kan innehålla 1: 1-relationer mellan entiteter.</span><span class="sxs-lookup"><span data-stu-id="3c099-415">Domain models may include one-to-one relationships between entities.</span></span> <span data-ttu-id="3c099-416">Om du behöver implementera en-till-en relation i tabelltjänsten måste du också välja hur du länkar två relaterade entiteter när du behöver hämta dem båda.</span><span class="sxs-lookup"><span data-stu-id="3c099-416">If you need to implement a one-to-one relationship in the Table service, you must also choose how to link the two related entities when you need to retrieve them both.</span></span> <span data-ttu-id="3c099-417">Den här länken kan vara implicit, baserat på en konvention i nyckelvärdena eller explicit genom att lagra en länk i form av **PartitionKey** och **RowKey** värdena i varje entitet till dess relaterade entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-417">This link can be either implicit, based on a convention in the key values, or explicit by storing a link in the form of **PartitionKey** and **RowKey** values in each entity to its related entity.</span></span> <span data-ttu-id="3c099-418">Mer information om huruvida du bör lagra relaterade entiteter i samma partition, finns i avsnittet [en-till-många-relationer](#one-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="3c099-418">For a discussion of whether you should store the related entities in the same partition, see the section [One-to-many relationships](#one-to-many-relationships).</span></span>  

<span data-ttu-id="3c099-419">Observera att det finns också implementering som kan leda dig att implementera 1: 1-relationer i tabelltjänsten:</span><span class="sxs-lookup"><span data-stu-id="3c099-419">Note that there are also implementation considerations that might lead you to implement one-to-one relationships in the Table service:</span></span>  

* <span data-ttu-id="3c099-420">Hantering av stora entiteter (Mer information finns i [stora entiteter mönster](#large-entities-pattern)).</span><span class="sxs-lookup"><span data-stu-id="3c099-420">Handling large entities (for more information, see [Large Entities Pattern](#large-entities-pattern)).</span></span>  
* <span data-ttu-id="3c099-421">Implementera åtkomstkontroller (Mer information finns i [Kontrollera åtkomst med signaturer för delad åtkomst](#controlling-access-with-shared-access-signatures)).</span><span class="sxs-lookup"><span data-stu-id="3c099-421">Implementing access controls (for more information, see [Controlling access with Shared Access Signatures](#controlling-access-with-shared-access-signatures)).</span></span>  

### <a name="join-in-the-client"></a><span data-ttu-id="3c099-422">Delta i klient</span><span class="sxs-lookup"><span data-stu-id="3c099-422">Join in the client</span></span>
<span data-ttu-id="3c099-423">Även om det finns olika sätt att modellen relationer i tabelltjänsten, ska du inte glömma att två huvudsakliga skälen för att använda tabelltjänsten finns skalbarhet och prestanda.</span><span class="sxs-lookup"><span data-stu-id="3c099-423">Although there are ways to model relationships in the Table service, you should not forget that the two prime reasons for using the Table service are scalability and performance.</span></span> <span data-ttu-id="3c099-424">Om du hittar du modellering många relationer som kan påverka prestanda och skalbarhet i lösningen, bör du fråga dig själv om det är nödvändigt att skapa alla relationer som data i tabelldesign.</span><span class="sxs-lookup"><span data-stu-id="3c099-424">If you find you are modelling many relationships that compromise the performance and scalability of your solution, you should ask yourself if it is necessary to build all the data relationships into your table design.</span></span> <span data-ttu-id="3c099-425">Du kan förenkla designen och förbättra skalbarhet och prestanda i lösningen om du låta ditt klientprogram som utför alla nödvändiga kopplingar.</span><span class="sxs-lookup"><span data-stu-id="3c099-425">You may be able to simplify the design and improve the scalability and performance of your solution if you let your client application perform any necessary joins.</span></span>  

<span data-ttu-id="3c099-426">Till exempel om du har liten tabeller som innehåller data som inte ändras ofta kan sedan du hämta dessa data en gång och cachelagras på klienten.</span><span class="sxs-lookup"><span data-stu-id="3c099-426">For example, if you have small tables that contain data that does not change very often, then you can retrieve this data once and cache it on the client.</span></span> <span data-ttu-id="3c099-427">Detta kan undvika upprepade görs för att hämta samma data.</span><span class="sxs-lookup"><span data-stu-id="3c099-427">This can avoid repeated roundtrips to retrieve the same data.</span></span> <span data-ttu-id="3c099-428">I exemplen som vi har tittat på i den här handboken sannolikt uppsättning avdelningar i en liten organisation att små och ändra sällan att göra det en bra kandidat för data som klientprogram kan ladda ned en gång och cache som slå upp data.</span><span class="sxs-lookup"><span data-stu-id="3c099-428">In the examples we have looked at in this guide, the set of departments in a small organization is likely to be small and change infrequently making it a good candidate for data that client application can download once and cache as look up data.</span></span>  

### <a name="inheritance-relationships"></a><span data-ttu-id="3c099-429">Arvsrelationer</span><span class="sxs-lookup"><span data-stu-id="3c099-429">Inheritance relationships</span></span>
<span data-ttu-id="3c099-430">Om klientprogrammet använder en uppsättning klasser som ingår i en arvsrelation för att representera affärsobjekt, kan du enkelt kvarstår dessa enheter i tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-430">If your client application uses a set of classes that form part of an inheritance relationship to represent business entities, you can easily persist those entities in the Table service.</span></span> <span data-ttu-id="3c099-431">Du kan till exempel ha följande uppsättning klasser som definieras i ditt klientprogram där **Person** är en abstrakt klass.</span><span class="sxs-lookup"><span data-stu-id="3c099-431">For example, you might have the following set of classes defined in your client application where **Person** is an abstract class.</span></span>

![][3]

<span data-ttu-id="3c099-432">Du kan spara instanser av två konkreta klasser i tjänsten tabellen med hjälp av en enskild Person tabell med entiteter i den ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="3c099-432">You can persist instances of the two concrete classes in the Table service using a single Person table using entities in that look like this:</span></span>  

![][4]

<span data-ttu-id="3c099-433">Mer information om hur du arbetar med flera typer av enheter i samma tabell i klientkod finns i avsnittet [arbeta med heterogena entitetstyper](#working-with-heterogeneous-entity-types) senare i den här guiden.</span><span class="sxs-lookup"><span data-stu-id="3c099-433">For more information about working with multiple entity types in the same table in client code, see the section [Working with heterogeneous entity types](#working-with-heterogeneous-entity-types) later in this guide.</span></span> <span data-ttu-id="3c099-434">Detta ger exempel på hur du identifierar entitetstypen i klientkod.</span><span class="sxs-lookup"><span data-stu-id="3c099-434">This provides examples of how to recognize the entity type in client code.</span></span>  

## <a name="table-design-patterns"></a><span data-ttu-id="3c099-435">Designmönster för tabellen</span><span class="sxs-lookup"><span data-stu-id="3c099-435">Table Design Patterns</span></span>
<span data-ttu-id="3c099-436">Du har sett detaljerad diskussioner om hur du optimerar dina tabelldesign för både hämtades entitetsdata med hjälp av frågor och infoga, uppdatera och ta bort entitetsdata i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3c099-436">In previous sections, you have seen some detailed discussions about how to optimize your table design for both retrieving entity data using queries and for inserting, updating, and deleting entity data.</span></span> <span data-ttu-id="3c099-437">Det här avsnittet beskrivs vissa mönster som är lämpliga för användning med tjänstelösningar för tabellen.</span><span class="sxs-lookup"><span data-stu-id="3c099-437">This section describes some patterns appropriate for use with Table service solutions.</span></span> <span data-ttu-id="3c099-438">Dessutom visas hur du praktiskt taget kan lösa vissa problem och avvägningarna aktiveras tidigare i den här guiden.</span><span class="sxs-lookup"><span data-stu-id="3c099-438">In addition, you will see how you can practically address some of the issues and trade-offs raised previously in this guide.</span></span> <span data-ttu-id="3c099-439">I följande diagram visas relationerna mellan de olika mönster:</span><span class="sxs-lookup"><span data-stu-id="3c099-439">The following diagram summarizes the relationships between the different patterns:</span></span>  

![][5]

<span data-ttu-id="3c099-440">Mönstret kartan ovan visar relationer mellan mönster (blå) och ett mönster (orange) som finns dokumenterade i handboken.</span><span class="sxs-lookup"><span data-stu-id="3c099-440">The pattern map above highlights some relationships between patterns (blue) and anti-patterns (orange) that are documented in this guide.</span></span> <span data-ttu-id="3c099-441">Det är naturligtvis många mönster som är värda att ta hänsyn till.</span><span class="sxs-lookup"><span data-stu-id="3c099-441">There are of course many other patterns that are worth considering.</span></span> <span data-ttu-id="3c099-442">Till exempel ett viktiga scenarier för Tabelltjänsten är att använda den [Materialiserade vyn mönster](https://msdn.microsoft.com/library/azure/dn589782.aspx) från den [kommandot frågan ansvar ansvarsfördelning (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) mönster.</span><span class="sxs-lookup"><span data-stu-id="3c099-442">For example, one of the key scenarios for Table Service is to use the [Materialized View Pattern](https://msdn.microsoft.com/library/azure/dn589782.aspx) from the [Command Query Responsibility Segregation (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) pattern.</span></span>  

### <a name="intra-partition-secondary-index-pattern"></a><span data-ttu-id="3c099-443">Intra-partition sekundärt index mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-443">Intra-partition secondary index pattern</span></span>
<span data-ttu-id="3c099-444">Lagra flera kopior av varje enhet med hjälp av olika **RowKey** värden (i samma partition) att aktivera snabb och effektiv sökningar och alternativa sorteringsordningen genom att använda olika **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-444">Store multiple copies of each entity using different **RowKey** values (in the same partition) to enable fast and efficient lookups and alternate sort orders by using different **RowKey** values.</span></span> <span data-ttu-id="3c099-445">Uppdateringar mellan kopior kan vara konsekvent med EGT'S.</span><span class="sxs-lookup"><span data-stu-id="3c099-445">Updates between copies can be kept consistent using EGT's.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="3c099-446">Kontexten och problem</span><span class="sxs-lookup"><span data-stu-id="3c099-446">Context and problem</span></span>
<span data-ttu-id="3c099-447">Tabelltjänsten indexerar automatiskt enheter med hjälp av den **PartitionKey** och **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-447">The Table service automatically indexes entities using the **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="3c099-448">Detta gör att ett klientprogram att hämta en entitet som effektivt använder dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-448">This enables a client application to retrieve an entity efficiently using these values.</span></span> <span data-ttu-id="3c099-449">Till exempel använder tabellstrukturen som visas nedan, ett klientprogram kan använda en punkt-fråga för att hämta en enskild medarbetare entitet med id och ett avdelningsnamn (den **PartitionKey** och **RowKey**  värden).</span><span class="sxs-lookup"><span data-stu-id="3c099-449">For example, using the table structure shown below, a client application can use a point query to retrieve an individual employee entity by using the department name and the employee id (the **PartitionKey** and **RowKey** values).</span></span> <span data-ttu-id="3c099-450">En klient kan också hämta entiteter sorterade efter anställnings-id inom varje avdelning.</span><span class="sxs-lookup"><span data-stu-id="3c099-450">A client can also retrieve entities sorted by employee id within each department.</span></span>

![][6]

<span data-ttu-id="3c099-451">Om du vill kunna hitta en medarbetare entitet baserat på värdet för en annan egenskap, t.ex e-postadress, måste du använda en mindre effektiv partition sökning för att hitta en matchande.</span><span class="sxs-lookup"><span data-stu-id="3c099-451">If you also want to be able to find an employee entity based on the value of another property, such as email address, you must use a less efficient partition scan to find a match.</span></span> <span data-ttu-id="3c099-452">Det beror på att tabelltjänsten inte ger sekundärindex.</span><span class="sxs-lookup"><span data-stu-id="3c099-452">This is because the table service does not provide secondary indexes.</span></span> <span data-ttu-id="3c099-453">Dessutom kan det går inte att begära en lista över anställda sorterade i en annan ordning än **RowKey** ordning.</span><span class="sxs-lookup"><span data-stu-id="3c099-453">In addition, there is no option to request a list of employees sorted in a different order than **RowKey** order.</span></span>  

#### <a name="solution"></a><span data-ttu-id="3c099-454">Lösning</span><span class="sxs-lookup"><span data-stu-id="3c099-454">Solution</span></span>
<span data-ttu-id="3c099-455">Undvik bristande sekundärindex, kan du lagra flera kopior av varje entitet med varje kopia med ett annat **RowKey** värde.</span><span class="sxs-lookup"><span data-stu-id="3c099-455">To work around the lack of secondary indexes, you can store multiple copies of each entity with each copy using a different **RowKey** value.</span></span> <span data-ttu-id="3c099-456">Om du sparar en entitet med strukturer som visas nedan, kan du effektivt hämta medarbetare enheter baserat på e-postadress eller medarbetare id. Prefixet värden för den **RowKey**, ”empid_” och ”email_” kan du fråga efter en medarbetare eller ett intervall med anställda med hjälp av en mängd e-postadresser eller medarbetare-ID: n.</span><span class="sxs-lookup"><span data-stu-id="3c099-456">If you store an entity with the structures shown below, you can efficiently retrieve employee entities based on email address or employee id. The prefix values for the **RowKey**, "empid_" and "email_" enable you to query for a single employee or a range of employees by using a range of email addresses or employee ids.</span></span>  

![][7]

<span data-ttu-id="3c099-457">Följande två filtervillkoren (en slå upp av anställnings-id och en sökning efter av e-postadress) ange både punkt frågor:</span><span class="sxs-lookup"><span data-stu-id="3c099-457">The following two filter criteria (one looking up by employee id and one looking up by email address) both specify point queries:</span></span>  

* <span data-ttu-id="3c099-458">$filter = (PartitionKey eq 'Sales') och (RowKey eq 'empid_000223')</span><span class="sxs-lookup"><span data-stu-id="3c099-458">$filter=(PartitionKey eq 'Sales') and (RowKey eq 'empid_000223')</span></span>  
* <span data-ttu-id="3c099-459">$filter = (PartitionKey eq 'Sales') och (RowKey eq 'email_jonesj@contoso.com')</span><span class="sxs-lookup"><span data-stu-id="3c099-459">$filter=(PartitionKey eq 'Sales') and (RowKey eq 'email_jonesj@contoso.com')</span></span>  

<span data-ttu-id="3c099-460">Om du frågar efter ett intervall med anställdas enheter du kan ange ett intervall i medarbetare id ordning eller ett intervall som är sorterad i e-postadress ordning genom att fråga om entiteter med ett prefix i den **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="3c099-460">If you query for a range of employee entities, you can specify a range sorted in employee id order, or a range sorted in email address order by querying for entities with the appropriate prefix in the **RowKey**.</span></span>  

* <span data-ttu-id="3c099-461">Hitta alla anställda på försäljningsavdelningen med medarbetare id i intervallet 000100 000199 användning: $filter = (PartitionKey eq 'Sales') och (RowKey ge 'empid_000100') och (RowKey le 'empid_000199')</span><span class="sxs-lookup"><span data-stu-id="3c099-461">To find all the employees in the Sales department with an employee id in the range 000100 to 000199 use: $filter=(PartitionKey eq 'Sales') and (RowKey ge 'empid_000100') and (RowKey le 'empid_000199')</span></span>  
* <span data-ttu-id="3c099-462">Hitta alla anställda på försäljningsavdelningen med en e-postadress som börjar med bokstaven ”a” Använd: $filter = (PartitionKey eq 'Sales') och (RowKey ge 'email_a') och (RowKey lt 'email_b')</span><span class="sxs-lookup"><span data-stu-id="3c099-462">To find all the employees in the Sales department with an email address starting with the letter 'a' use: $filter=(PartitionKey eq 'Sales') and (RowKey ge 'email_a') and (RowKey lt 'email_b')</span></span>  
  
  <span data-ttu-id="3c099-463">Observera att filtersyntaxen som används i exemplen ovan är från tabelltjänsten REST API för mer information finns i [fråga entiteter](http://msdn.microsoft.com/library/azure/dd179421.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c099-463">Note that the filter syntax used in the examples above is from the Table service REST API, for more information see [Query Entities](http://msdn.microsoft.com/library/azure/dd179421.aspx).</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="3c099-464">Problem och överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-464">Issues and considerations</span></span>
<span data-ttu-id="3c099-465">Tänk på följande när du bestämmer hur du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-465">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="3c099-466">Table storage är relativt billig använder så overhead kostnaden för lagring av duplicerade data inte får vara ett större problem.</span><span class="sxs-lookup"><span data-stu-id="3c099-466">Table storage is relatively cheap to use so the cost overhead of storing duplicate data should not be a major concern.</span></span> <span data-ttu-id="3c099-467">Du bör dock alltid utvärdera kostnaden för din design utifrån din förväntade lagringsbehov och bara lägga till dubbla entiteter för att stödja frågorna client-program körs.</span><span class="sxs-lookup"><span data-stu-id="3c099-467">However, you should always evaluate the cost of your design based on your anticipated storage requirements and only add duplicate entities to support the queries your client application will execute.</span></span>  
* <span data-ttu-id="3c099-468">Eftersom entiteterna sekundärt index lagras i samma partition som de ursprungliga enheterna, bör du kontrollera att du inte överskrider skalbarhetsmål för en enskild partition.</span><span class="sxs-lookup"><span data-stu-id="3c099-468">Because the secondary index entities are stored in the same partition as the original entities, you should ensure that you do not exceed the scalability targets for an individual partition.</span></span>  
* <span data-ttu-id="3c099-469">Du kan behålla-dubbla enheterna överensstämmer med varandra med hjälp av EGTs att automatiskt uppdatera två kopior av entiteten.</span><span class="sxs-lookup"><span data-stu-id="3c099-469">You can keep your duplicate entities consistent with each other by using EGTs to update the two copies of the entity atomically.</span></span> <span data-ttu-id="3c099-470">Detta innebär att du bör lagra alla kopior av en entitet i samma partition.</span><span class="sxs-lookup"><span data-stu-id="3c099-470">This implies that you should store all copies of an entity in the same partition.</span></span> <span data-ttu-id="3c099-471">Mer information finns i avsnittet [med hjälp av entiteten gruppera transaktioner](#entity-group-transactions).</span><span class="sxs-lookup"><span data-stu-id="3c099-471">For more information, see the section [Using Entity Group Transactions](#entity-group-transactions).</span></span>  
* <span data-ttu-id="3c099-472">Det värde som används för den **RowKey** måste vara unikt för varje entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-472">The value used for the **RowKey** must be unique for each entity.</span></span> <span data-ttu-id="3c099-473">Överväg att använda sammansatta nyckelvärden.</span><span class="sxs-lookup"><span data-stu-id="3c099-473">Consider using compound key values.</span></span>  
* <span data-ttu-id="3c099-474">Utfyllnad numeriska värden i den **RowKey** (exempelvis anställnings-id 000223), aktiverar korrigera sortera och filtrera baserat på övre och nedre gränser.</span><span class="sxs-lookup"><span data-stu-id="3c099-474">Padding numeric values in the **RowKey** (for example, the employee id 000223), enables correct sorting and filtering based on upper and lower bounds.</span></span>  
* <span data-ttu-id="3c099-475">Du behöver inte nödvändigtvis att kopiera alla egenskaper för entiteten.</span><span class="sxs-lookup"><span data-stu-id="3c099-475">You do not necessarily need to duplicate all the properties of your entity.</span></span> <span data-ttu-id="3c099-476">Till exempel om frågorna som sökning entiteter med den e-posten-adressen i den **RowKey** behöver aldrig medarbetarens ålder, dessa enheter kan ha följande struktur:</span><span class="sxs-lookup"><span data-stu-id="3c099-476">For example, if the queries that lookup the entities using the email address in the **RowKey** never need the employee's age, these entities could have the following structure:</span></span>

![][8]

* <span data-ttu-id="3c099-477">Det är vanligtvis bättre att lagra duplicerade data och se till att du kan hämta alla data som du behöver med en enskild fråga än att använda en fråga för att hitta en enhet och en annan för att söka efter data som krävs.</span><span class="sxs-lookup"><span data-stu-id="3c099-477">It is typically better to store duplicate data and ensure that you can retrieve all the data you need with a single query, than to use one query to locate an entity and another to lookup the required data.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="3c099-478">När du ska använda det här mönstret</span><span class="sxs-lookup"><span data-stu-id="3c099-478">When to use this pattern</span></span>
<span data-ttu-id="3c099-479">Använd det här mönstret när klientprogrammet måste hämta entiteter med olika nycklar olika när klienten behöver hämta entiteter i olika sorteringsordningar, och där du kan identifiera varje entitet med olika unika värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-479">Use this pattern when your client application needs to retrieve entities using a variety of different keys, when your client needs to retrieve entities in different sort orders, and where you can identify each entity using a variety of unique values.</span></span> <span data-ttu-id="3c099-480">Dock bör du se till att du inte överskrider skalbarhetsgränser partition när du utför entitet sökningar med hjälp av de olika **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-480">However, you should be sure that you do not exceed the partition scalability limits when you are performing entity lookups using the different **RowKey** values.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="3c099-481">Vägledning och relaterade mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-481">Related patterns and guidance</span></span>
<span data-ttu-id="3c099-482">Följande mönster och guider kan även vara relevanta när du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-482">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="3c099-483">Mellan sekundära Partitionsindex mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-483">Inter-partition secondary index pattern</span></span>](#inter-partition-secondary-index-pattern)
* [<span data-ttu-id="3c099-484">Sammansatt nyckel mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-484">Compound key pattern</span></span>](#compound-key-pattern)
* [<span data-ttu-id="3c099-485">Entiteten gruppera transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-485">Entity Group Transactions</span></span>](#entity-group-transactions)
* [<span data-ttu-id="3c099-486">Arbeta med heterogena entitetstyper</span><span class="sxs-lookup"><span data-stu-id="3c099-486">Working with heterogeneous entity types</span></span>](#working-with-heterogeneous-entity-types)

### <a name="inter-partition-secondary-index-pattern"></a><span data-ttu-id="3c099-487">Mellan sekundära Partitionsindex mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-487">Inter-partition secondary index pattern</span></span>
<span data-ttu-id="3c099-488">Lagra flera kopior av varje enhet med hjälp av olika **RowKey** värden i separata partitioner eller i separata tabeller för att aktivera snabb och effektiv sökningar och alternativa sorteringsordningen genom att använda olika **RowKey**värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-488">Store multiple copies of each entity using different **RowKey** values in separate partitions or in separate tables to enable fast and efficient lookups and alternate sort orders by using different **RowKey** values.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="3c099-489">Kontexten och problem</span><span class="sxs-lookup"><span data-stu-id="3c099-489">Context and problem</span></span>
<span data-ttu-id="3c099-490">Tabelltjänsten indexerar automatiskt enheter med hjälp av den **PartitionKey** och **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-490">The Table service automatically indexes entities using the **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="3c099-491">Detta gör att ett klientprogram att hämta en entitet som effektivt använder dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-491">This enables a client application to retrieve an entity efficiently using these values.</span></span> <span data-ttu-id="3c099-492">Till exempel använder tabellstrukturen som visas nedan, ett klientprogram kan använda en punkt-fråga för att hämta en enskild medarbetare entitet med id och ett avdelningsnamn (den **PartitionKey** och **RowKey**  värden).</span><span class="sxs-lookup"><span data-stu-id="3c099-492">For example, using the table structure shown below, a client application can use a point query to retrieve an individual employee entity by using the department name and the employee id (the **PartitionKey** and **RowKey** values).</span></span> <span data-ttu-id="3c099-493">En klient kan också hämta entiteter sorterade efter anställnings-id inom varje avdelning.</span><span class="sxs-lookup"><span data-stu-id="3c099-493">A client can also retrieve entities sorted by employee id within each department.</span></span>  

![][9]

<span data-ttu-id="3c099-494">Om du vill kunna hitta en medarbetare entitet baserat på värdet för en annan egenskap, t.ex e-postadress, måste du använda en mindre effektiv partition sökning för att hitta en matchande.</span><span class="sxs-lookup"><span data-stu-id="3c099-494">If you also want to be able to find an employee entity based on the value of another property, such as email address, you must use a less efficient partition scan to find a match.</span></span> <span data-ttu-id="3c099-495">Det beror på att tabelltjänsten inte ger sekundärindex.</span><span class="sxs-lookup"><span data-stu-id="3c099-495">This is because the table service does not provide secondary indexes.</span></span> <span data-ttu-id="3c099-496">Dessutom kan det går inte att begära en lista över anställda sorterade i en annan ordning än **RowKey** ordning.</span><span class="sxs-lookup"><span data-stu-id="3c099-496">In addition, there is no option to request a list of employees sorted in a different order than **RowKey** order.</span></span>  

<span data-ttu-id="3c099-497">Du förutse en mycket stor volym med transaktioner mot dessa enheter och vill minska risken för tabelltjänsten begränsning av klienten.</span><span class="sxs-lookup"><span data-stu-id="3c099-497">You are anticipating a very high volume of transactions against these entities and want to minimize the risk of the Table service throttling your client.</span></span>  

#### <a name="solution"></a><span data-ttu-id="3c099-498">Lösning</span><span class="sxs-lookup"><span data-stu-id="3c099-498">Solution</span></span>
<span data-ttu-id="3c099-499">Undvik bristande sekundärindex, kan du lagra flera kopior av varje entitet med varje kopia med hjälp av olika **PartitionKey** och **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-499">To work around the lack of secondary indexes, you can store multiple copies of each entity with each copy using different **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="3c099-500">Om du sparar en entitet med strukturer som visas nedan, kan du effektivt hämta medarbetare enheter baserat på e-postadress eller medarbetare id. Prefixet värden för den **PartitionKey**, ”empid_” och ”email_” kan du identifiera vilka index som du vill använda för en fråga.</span><span class="sxs-lookup"><span data-stu-id="3c099-500">If you store an entity with the structures shown below, you can efficiently retrieve employee entities based on email address or employee id. The prefix values for the **PartitionKey**, "empid_" and "email_" enable you to identify which index you want to use for a query.</span></span>  

![][10]

<span data-ttu-id="3c099-501">Följande två filtervillkoren (en slå upp av anställnings-id och en sökning efter av e-postadress) ange både punkt frågor:</span><span class="sxs-lookup"><span data-stu-id="3c099-501">The following two filter criteria (one looking up by employee id and one looking up by email address) both specify point queries:</span></span>  

* <span data-ttu-id="3c099-502">$filter = (PartitionKey eq ' empid_Sales') och (RowKey eq '000223')</span><span class="sxs-lookup"><span data-stu-id="3c099-502">$filter=(PartitionKey eq 'empid_Sales') and (RowKey eq '000223')</span></span>
* <span data-ttu-id="3c099-503">$filter = (PartitionKey eq ' email_Sales') och (RowKey eq 'jonesj@contoso.com')</span><span class="sxs-lookup"><span data-stu-id="3c099-503">$filter=(PartitionKey eq 'email_Sales') and (RowKey eq 'jonesj@contoso.com')</span></span>  

<span data-ttu-id="3c099-504">Om du frågar efter ett intervall med anställdas enheter du kan ange ett intervall i medarbetare id ordning eller ett intervall som är sorterad i e-postadress ordning genom att fråga om entiteter med ett prefix i den **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="3c099-504">If you query for a range of employee entities, you can specify a range sorted in employee id order, or a range sorted in email address order by querying for entities with the appropriate prefix in the **RowKey**.</span></span>  

* <span data-ttu-id="3c099-505">Hitta alla anställda på försäljningsavdelningen med medarbetare id i intervallet **000100** till **000199** sorteras medarbetare id ordning används: $filter = (PartitionKey eq ' empid_Sales') och (RowKey ge '000100') och (RowKey le '000199')</span><span class="sxs-lookup"><span data-stu-id="3c099-505">To find all the employees in the Sales department with an employee id in the range **000100** to **000199** sorted in employee id order use: $filter=(PartitionKey eq 'empid_Sales') and (RowKey ge '000100') and (RowKey le '000199')</span></span>  
* <span data-ttu-id="3c099-506">Hitta alla anställda på försäljningsavdelningen med en e-postadress som börjar med ”a” i e-postadress ordning användning: $filter = (PartitionKey eq ' email_Sales') och (RowKey ge ”a”) och (RowKey lt ”b”)</span><span class="sxs-lookup"><span data-stu-id="3c099-506">To find all the employees in the Sales department with an email address that starts with 'a' sorted in email address order use: $filter=(PartitionKey eq 'email_Sales') and (RowKey ge 'a') and (RowKey lt 'b')</span></span>  

<span data-ttu-id="3c099-507">Observera att filtersyntaxen som används i exemplen ovan är från tabelltjänsten REST API för mer information finns i [fråga entiteter](http://msdn.microsoft.com/library/azure/dd179421.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c099-507">Note that the filter syntax used in the examples above is from the Table service REST API, for more information see [Query Entities](http://msdn.microsoft.com/library/azure/dd179421.aspx).</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="3c099-508">Problem och överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-508">Issues and considerations</span></span>
<span data-ttu-id="3c099-509">Tänk på följande när du bestämmer hur du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-509">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="3c099-510">Du kan behålla din dubbla entiteter överensstämmelse med varandra med hjälp av den [överensstämmelse transaktioner mönster](#eventually-consistent-transactions-pattern) att underhålla de primära och sekundära index entiteterna.</span><span class="sxs-lookup"><span data-stu-id="3c099-510">You can keep your duplicate entities eventually consistent with each other by using the [Eventually consistent transactions pattern](#eventually-consistent-transactions-pattern) to maintain the primary and secondary index entities.</span></span>  
* <span data-ttu-id="3c099-511">Table storage är relativt billig använder så overhead kostnaden för lagring av duplicerade data inte får vara ett större problem.</span><span class="sxs-lookup"><span data-stu-id="3c099-511">Table storage is relatively cheap to use so the cost overhead of storing duplicate data should not be a major concern.</span></span> <span data-ttu-id="3c099-512">Du bör dock alltid utvärdera kostnaden för din design utifrån din förväntade lagringsbehov och bara lägga till dubbla entiteter för att stödja frågorna client-program körs.</span><span class="sxs-lookup"><span data-stu-id="3c099-512">However, you should always evaluate the cost of your design based on your anticipated storage requirements and only add duplicate entities to support the queries your client application will execute.</span></span>  
* <span data-ttu-id="3c099-513">Det värde som används för den **RowKey** måste vara unikt för varje entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-513">The value used for the **RowKey** must be unique for each entity.</span></span> <span data-ttu-id="3c099-514">Överväg att använda sammansatta nyckelvärden.</span><span class="sxs-lookup"><span data-stu-id="3c099-514">Consider using compound key values.</span></span>  
* <span data-ttu-id="3c099-515">Utfyllnad numeriska värden i den **RowKey** (exempelvis anställnings-id 000223), aktiverar korrigera sortera och filtrera baserat på övre och nedre gränser.</span><span class="sxs-lookup"><span data-stu-id="3c099-515">Padding numeric values in the **RowKey** (for example, the employee id 000223), enables correct sorting and filtering based on upper and lower bounds.</span></span>  
* <span data-ttu-id="3c099-516">Du behöver inte nödvändigtvis att kopiera alla egenskaper för entiteten.</span><span class="sxs-lookup"><span data-stu-id="3c099-516">You do not necessarily need to duplicate all the properties of your entity.</span></span> <span data-ttu-id="3c099-517">Till exempel om frågorna som sökning entiteter med den e-posten-adressen i den **RowKey** behöver aldrig medarbetarens ålder, dessa enheter kan ha följande struktur:</span><span class="sxs-lookup"><span data-stu-id="3c099-517">For example, if the queries that lookup the entities using the email address in the **RowKey** never need the employee's age, these entities could have the following structure:</span></span>
  
  ![][11]
* <span data-ttu-id="3c099-518">Det är vanligtvis bättre att lagra duplicerade data och se till att du kan hämta alla data som du behöver med en enskild fråga att använda en fråga för att hitta en entitet med sekundärt index och en annan att söka efter data som krävs i det primära indexet än.</span><span class="sxs-lookup"><span data-stu-id="3c099-518">It is typically better to store duplicate data and ensure that you can retrieve all the data you need with a single query than to use one query to locate an entity using the secondary index and another to lookup the required data in the primary index.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="3c099-519">När du ska använda det här mönstret</span><span class="sxs-lookup"><span data-stu-id="3c099-519">When to use this pattern</span></span>
<span data-ttu-id="3c099-520">Använd det här mönstret när klientprogrammet måste hämta entiteter med olika nycklar olika när klienten behöver hämta entiteter i olika sorteringsordningar, och där du kan identifiera varje entitet med olika unika värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-520">Use this pattern when your client application needs to retrieve entities using a variety of different keys, when your client needs to retrieve entities in different sort orders, and where you can identify each entity using a variety of unique values.</span></span> <span data-ttu-id="3c099-521">Använd det här mönstret när du vill undvika överstiger skalbarhetsgränser partition när du utför entitet sökningar med hjälp av de olika **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-521">Use this pattern when you want to avoid exceeding the partition scalability limits when you are performing entity lookups using the different **RowKey** values.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="3c099-522">Vägledning och relaterade mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-522">Related patterns and guidance</span></span>
<span data-ttu-id="3c099-523">Följande mönster och guider kan även vara relevanta när du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-523">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="3c099-524">Mönster för överensstämmelse transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-524">Eventually consistent transactions pattern</span></span>](#eventually-consistent-transactions-pattern)  
* [<span data-ttu-id="3c099-525">Intra-partition sekundärt index mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-525">Intra-partition secondary index pattern</span></span>](#intra-partition-secondary-index-pattern)  
* [<span data-ttu-id="3c099-526">Sammansatt nyckel mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-526">Compound key pattern</span></span>](#compound-key-pattern)  
* [<span data-ttu-id="3c099-527">Entiteten gruppera transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-527">Entity Group Transactions</span></span>](#entity-group-transactions)  
* [<span data-ttu-id="3c099-528">Arbeta med heterogena entitetstyper</span><span class="sxs-lookup"><span data-stu-id="3c099-528">Working with heterogeneous entity types</span></span>](#working-with-heterogeneous-entity-types)  

### <a name="eventually-consistent-transactions-pattern"></a><span data-ttu-id="3c099-529">Mönster för överensstämmelse transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-529">Eventually consistent transactions pattern</span></span>
<span data-ttu-id="3c099-530">Aktivera överensstämmelse beteende mellan partitionsgränser eller lagring system gränser med hjälp av Azure köer.</span><span class="sxs-lookup"><span data-stu-id="3c099-530">Enable eventually consistent behavior across partition boundaries or storage system boundaries by using Azure queues.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="3c099-531">Kontexten och problem</span><span class="sxs-lookup"><span data-stu-id="3c099-531">Context and problem</span></span>
<span data-ttu-id="3c099-532">EGTs aktivera atomiska transaktioner mellan flera enheter som delar samma partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="3c099-532">EGTs enable atomic transactions across multiple entities that share the same partition key.</span></span> <span data-ttu-id="3c099-533">För bättre prestanda och skalbarhet som du kan välja att lagra entiteter som har konsekvenskontroll krav i separata partitioner eller i ett separat lagringssystem: i ett sådant scenario, du kan inte använda EGTs för att upprätthålla enhetliga.</span><span class="sxs-lookup"><span data-stu-id="3c099-533">For performance and scalability reasons, you might decide to store entities that have consistency requirements in separate partitions or in a separate storage system: in such a scenario, you cannot use EGTs to maintain consistency.</span></span> <span data-ttu-id="3c099-534">Du kan till exempel ha ett krav att underhålla slutliga konsekvensen mellan:</span><span class="sxs-lookup"><span data-stu-id="3c099-534">For example, you might have a requirement to maintain eventual consistency between:</span></span>  

* <span data-ttu-id="3c099-535">Enheter som lagras i två olika partitioner i samma tabell, i olika tabeller i i olika lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="3c099-535">Entities stored in two different partitions in the same table, in different tables, in in different storage accounts.</span></span>  
* <span data-ttu-id="3c099-536">En entitet som lagras i tabelltjänsten och en blob som lagras i Blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-536">An entity stored in the Table service and a blob stored in the Blob service.</span></span>  
* <span data-ttu-id="3c099-537">En entitet som lagras i tabelltjänsten och en fil i ett filsystem.</span><span class="sxs-lookup"><span data-stu-id="3c099-537">An entity stored in the Table service and a file in a file system.</span></span>  
* <span data-ttu-id="3c099-538">En entitet arkivet i tabelltjänsten indexerat ännu med Azure Search-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-538">An entity store in the Table service yet indexed using the Azure Search service.</span></span>  

#### <a name="solution"></a><span data-ttu-id="3c099-539">Lösning</span><span class="sxs-lookup"><span data-stu-id="3c099-539">Solution</span></span>
<span data-ttu-id="3c099-540">Du kan implementera en lösning som ger slutliga konsekvensen mellan två eller flera partitioner lagringssystem med hjälp av Azure köer.</span><span class="sxs-lookup"><span data-stu-id="3c099-540">By using Azure queues, you can implement a solution that delivers eventual consistency across two or more partitions or storage systems.</span></span>
<span data-ttu-id="3c099-541">För att illustrera den här metoden förutsätter att du har ett krav för att kunna arkivera gamla medarbetare enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-541">To illustrate this approach, assume you have a requirement to be able to archive old employee entities.</span></span> <span data-ttu-id="3c099-542">Den gamla medarbetare enheter frågas sällan och bör undantas från alla aktiviteter som handlar om aktuella anställda.</span><span class="sxs-lookup"><span data-stu-id="3c099-542">Old employee entities are rarely queried and should be excluded from any activities that deal with current employees.</span></span> <span data-ttu-id="3c099-543">För att implementera det här kravet du lagrar aktiva medarbetare i den **aktuella** tabell och tidigare anställda i den **Arkiv** tabell.</span><span class="sxs-lookup"><span data-stu-id="3c099-543">To implement this requirement you store active employees in the **Current** table and old employees in the **Archive** table.</span></span> <span data-ttu-id="3c099-544">Arkivering av en medarbetare måste du ta bort entiteten från den **aktuella** tabell och lägga till enheten som den **Arkiv** , men du kan inte använda en EGT för att utföra dessa två åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3c099-544">Archiving an employee requires you to delete the entity from the **Current** table and add the entity to the **Archive** table, but you cannot use an EGT to perform these two operations.</span></span> <span data-ttu-id="3c099-545">Arkivåtgärden måste vara överensstämmelse för att undvika risken för att ett fel som orsakar en entitet som ska visas i båda eller inget tabeller.</span><span class="sxs-lookup"><span data-stu-id="3c099-545">To avoid the risk that a failure causes an entity to appear in both or neither tables, the archive operation must be eventually consistent.</span></span> <span data-ttu-id="3c099-546">Följande sekvensdiagram beskrivs stegen i den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="3c099-546">The following sequence diagram outlines the steps in this operation.</span></span> <span data-ttu-id="3c099-547">Mer information ges för undantag sökvägar i följande text.</span><span class="sxs-lookup"><span data-stu-id="3c099-547">More detail is provided for exception paths in the text following.</span></span>  

![][12]

<span data-ttu-id="3c099-548">En klient initierar Arkiv igen genom att placera ett meddelande på en Azure-kö, i det här exemplet att arkivera medarbetare #456.</span><span class="sxs-lookup"><span data-stu-id="3c099-548">A client initiates the archive operation by placing a message on an Azure queue, in this example to archive employee #456.</span></span> <span data-ttu-id="3c099-549">En arbetsroll avsöker kön för nya meddelanden. När den hittar en läser meddelandet och en dold kopia finns kvar i kön.</span><span class="sxs-lookup"><span data-stu-id="3c099-549">A worker role polls the queue for new messages; when it finds one, it reads the message and leaves a hidden copy on the queue.</span></span> <span data-ttu-id="3c099-550">Arbetsrollen nästa hämtar en kopia av entiteten från den **aktuella** tabell, infogar en kopia i den **Arkiv** table och tar sedan bort ursprungligt från den **aktuella** tabell.</span><span class="sxs-lookup"><span data-stu-id="3c099-550">The worker role next fetches a copy of the entity from the **Current** table, inserts a copy in the **Archive** table, and then deletes the original from the **Current** table.</span></span> <span data-ttu-id="3c099-551">Slutligen, om det inte fanns några fel från föregående steg, arbetsrollen tar bort dolda meddelandet från kön.</span><span class="sxs-lookup"><span data-stu-id="3c099-551">Finally, if there were no errors from the previous steps, the worker role deletes the hidden message from the queue.</span></span>  

<span data-ttu-id="3c099-552">I det här exemplet steg 4 infogar medarbetaren till den **Arkiv** tabell.</span><span class="sxs-lookup"><span data-stu-id="3c099-552">In this example, step 4 inserts the employee into the **Archive** table.</span></span> <span data-ttu-id="3c099-553">Det gick att lägga till anställde till en blobb i Blob-tjänsten eller en fil i ett filsystem.</span><span class="sxs-lookup"><span data-stu-id="3c099-553">It could add the employee to a blob in the Blob service or a file in a file system.</span></span>  

#### <a name="recovering-from-failures"></a><span data-ttu-id="3c099-554">Återställa från fel</span><span class="sxs-lookup"><span data-stu-id="3c099-554">Recovering from failures</span></span>
<span data-ttu-id="3c099-555">Det är viktigt som åtgärder i steg **4** och **5** måste vara *idempotent* ifall arbetsrollen behöver starta om arkivåtgärden.</span><span class="sxs-lookup"><span data-stu-id="3c099-555">It is important that the operations in steps **4** and **5** must be *idempotent* in case the worker role needs to restart the archive operation.</span></span> <span data-ttu-id="3c099-556">Om du använder tjänsten tabellen för steget **4** bör du använda en ”infoga eller ersätta” åtgärd; för steg **5** bör du använda en ”ta bort om finns” åtgärden i klientbiblioteket som du använder.</span><span class="sxs-lookup"><span data-stu-id="3c099-556">If you are using the Table service, for step **4** you should use an "insert or replace" operation; for step **5** you should use a "delete if exists" operation in the client library you are using.</span></span> <span data-ttu-id="3c099-557">Om du använder en annan lagringssystemet, måste du använda en lämplig idempotent-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="3c099-557">If you are using another storage system, you must use an appropriate idempotent operation.</span></span>  

<span data-ttu-id="3c099-558">Om arbetsrollen slutförs aldrig steg **6**, och sedan efter en tidsgräns meddelandet visas igen på kön för arbetsrollen att försöka bearbeta den.</span><span class="sxs-lookup"><span data-stu-id="3c099-558">If the worker role never completes step **6**, then after a timeout the message reappears on the queue ready for the worker role to try to reprocess it.</span></span> <span data-ttu-id="3c099-559">Worker-rollen kan kontrollera hur många gånger meddelandet i kön har läsa och, om det behövs Flagga det är ett ”skadligt” meddelande för undersökningen genom att skicka den till en särskild kö.</span><span class="sxs-lookup"><span data-stu-id="3c099-559">The worker role can check how many times a message on the queue has been read and, if necessary, flag it is a "poison" message for investigation by sending it to a separate queue.</span></span> <span data-ttu-id="3c099-560">Läs mer om läsa meddelanden i kön och kontrollera antalet dequeue [få meddelanden](https://msdn.microsoft.com/library/azure/dd179474.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c099-560">For more information about reading queue messages and checking the dequeue count, see [Get Messages](https://msdn.microsoft.com/library/azure/dd179474.aspx).</span></span>  

<span data-ttu-id="3c099-561">Fel från tabellen och kön tjänsterna är tillfälligt fel och ditt klientprogram ska innehålla lämplig logik för att hantera dem.</span><span class="sxs-lookup"><span data-stu-id="3c099-561">Some errors from the Table and Queue services are transient errors, and your client application should include suitable retry logic to handle them.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="3c099-562">Problem och överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-562">Issues and considerations</span></span>
<span data-ttu-id="3c099-563">Tänk på följande när du bestämmer hur du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-563">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="3c099-564">Den här lösningen ger inte för transaktionsisoleringen.</span><span class="sxs-lookup"><span data-stu-id="3c099-564">This solution does not provide for transaction isolation.</span></span> <span data-ttu-id="3c099-565">Till exempel en klient kan läsa den **aktuella** och **Arkiv** tabeller när arbetsrollen mellan stegen **4** och **5**, och se en Inkonsekvent visning av data.</span><span class="sxs-lookup"><span data-stu-id="3c099-565">For example, a client could read the **Current** and **Archive** tables when the worker role was between steps **4** and **5**, and see an inconsistent view of the data.</span></span> <span data-ttu-id="3c099-566">Observera att data är konsekventa förr eller senare.</span><span class="sxs-lookup"><span data-stu-id="3c099-566">Note that the data will be consistent eventually.</span></span>  
* <span data-ttu-id="3c099-567">Du måste vara säker på att steg 4 och 5 är idempotent för att säkerställa slutliga konsekvensen.</span><span class="sxs-lookup"><span data-stu-id="3c099-567">You must be sure that steps 4 and 5 are idempotent in order to ensure eventual consistency.</span></span>  
* <span data-ttu-id="3c099-568">Du kan skala lösningen genom att använda flera köer och worker rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="3c099-568">You can scale the solution by using multiple queues and worker role instances.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="3c099-569">När du ska använda det här mönstret</span><span class="sxs-lookup"><span data-stu-id="3c099-569">When to use this pattern</span></span>
<span data-ttu-id="3c099-570">Använd det här mönstret när du vill garantera slutliga konsekvensen mellan enheter som finns i olika partitioner eller tabeller.</span><span class="sxs-lookup"><span data-stu-id="3c099-570">Use this pattern when you want to guarantee eventual consistency between entities that exist in different partitions or tables.</span></span> <span data-ttu-id="3c099-571">Du kan utöka detta mönster för att säkerställa slutliga konsekvensen för åtgärder i tabelltjänsten och Blob-tjänsten och andra Azure Storage-datakällor, till exempel databasen eller filsystemet.</span><span class="sxs-lookup"><span data-stu-id="3c099-571">You can extend this pattern to ensure eventual consistency for operations across the Table service and the Blob service and other non-Azure Storage data sources such as database or the file system.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="3c099-572">Vägledning och relaterade mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-572">Related patterns and guidance</span></span>
<span data-ttu-id="3c099-573">Följande mönster och guider kan även vara relevanta när du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-573">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="3c099-574">Entiteten gruppera transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-574">Entity Group Transactions</span></span>](#entity-group-transactions)  
* [<span data-ttu-id="3c099-575">Merge eller ersätta</span><span class="sxs-lookup"><span data-stu-id="3c099-575">Merge or replace</span></span>](#merge-or-replace)  

> [!NOTE]
> <span data-ttu-id="3c099-576">Om transaktionsisoleringen är viktigt att din lösning bör du designar tabellerna så att du kan använda EGTs.</span><span class="sxs-lookup"><span data-stu-id="3c099-576">If transaction isolation is important to your solution, you should consider redesigning your tables to enable you to use EGTs.</span></span>  
> 
> 

### <a name="index-entities-pattern"></a><span data-ttu-id="3c099-577">Index entiteter mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-577">Index Entities Pattern</span></span>
<span data-ttu-id="3c099-578">Underhålla index enheter om du vill aktivera effektiva sökningar som returnerar en lista över enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-578">Maintain index entities to enable efficient searches that return lists of entities.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="3c099-579">Kontexten och problem</span><span class="sxs-lookup"><span data-stu-id="3c099-579">Context and problem</span></span>
<span data-ttu-id="3c099-580">Tabelltjänsten indexerar automatiskt enheter med hjälp av den **PartitionKey** och **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-580">The Table service automatically indexes entities using the **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="3c099-581">Detta gör att ett klientprogram att hämta en entitet som effektivt med en punkt-fråga.</span><span class="sxs-lookup"><span data-stu-id="3c099-581">This enables a client application to retrieve an entity efficiently using a point query.</span></span> <span data-ttu-id="3c099-582">Till exempel använder tabellstrukturen som visas nedan, ett klientprogram enkelt kan hämta en enskild medarbetare entitet med id och ett avdelningsnamn (den **PartitionKey** och **RowKey**).</span><span class="sxs-lookup"><span data-stu-id="3c099-582">For example, using the table structure shown below, a client application can efficiently retrieve an individual employee entity by using the department name and the employee id (the **PartitionKey** and **RowKey**).</span></span>  

![][13]

<span data-ttu-id="3c099-583">Om du vill kunna hämta en lista över anställdas enheter baserat på värdet för en annan icke-unikt egenskap, till exempel efternamn, måste du använda en mindre effektiv partition-sökning för att hitta matchningar i stället för att använda ett index för att leta upp dem direkt.</span><span class="sxs-lookup"><span data-stu-id="3c099-583">If you also want to be able to retrieve a list of employee entities based on the value of another non-unique property, such as their last name, you must use a less efficient partition scan to find matches rather than using an index to look them up directly.</span></span> <span data-ttu-id="3c099-584">Det beror på att tabelltjänsten inte ger sekundärindex.</span><span class="sxs-lookup"><span data-stu-id="3c099-584">This is because the table service does not provide secondary indexes.</span></span>  

#### <a name="solution"></a><span data-ttu-id="3c099-585">Lösning</span><span class="sxs-lookup"><span data-stu-id="3c099-585">Solution</span></span>
<span data-ttu-id="3c099-586">Om du vill aktivera sökning efter efternamn med entiteten struktur som visas ovan, måste du upprätthålla en lista över medarbetare-ID: n.</span><span class="sxs-lookup"><span data-stu-id="3c099-586">To enable lookup by last name with the entity structure shown above, you must maintain lists of employee ids.</span></span> <span data-ttu-id="3c099-587">Om du vill hämta medarbetare entiteter med ett visst efternamn, till exempel Karlsson, måste du först lokalisera listan över medarbetare-ID: n för anställda med Karlsson som efternamn och hämta anställdas enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-587">If you want to retrieve the employee entities with a particular last name, such as Jones, you must first locate the list of employee ids for employees with Jones as their last name, and then retrieve those employee entities.</span></span> <span data-ttu-id="3c099-588">Det finns tre huvudsakliga alternativ för att lagra listor över medarbetare-ID: n:</span><span class="sxs-lookup"><span data-stu-id="3c099-588">There are three main options for storing the lists of employee ids:</span></span>  

* <span data-ttu-id="3c099-589">Använda blob storage.</span><span class="sxs-lookup"><span data-stu-id="3c099-589">Use blob storage.</span></span>  
* <span data-ttu-id="3c099-590">Skapa index entiteter i samma partition som anställdas enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-590">Create index entities in the same partition as the employee entities.</span></span>  
* <span data-ttu-id="3c099-591">Skapa index entiteter i en separat partition eller tabellen.</span><span class="sxs-lookup"><span data-stu-id="3c099-591">Create index entities in a separate partition or table.</span></span>  

<span data-ttu-id="3c099-592"><u>Alternativ #1: Använda blob storage</u></span><span class="sxs-lookup"><span data-stu-id="3c099-592"><u>Option #1: Use blob storage</u></span></span>  

<span data-ttu-id="3c099-593">För det första alternativet, skapar du en blob för varje unik efternamn och i varje blobstore en lista över de **PartitionKey** (avdelning) och **RowKey** värden (anställnings-id) för anställda som har det senaste namnet.</span><span class="sxs-lookup"><span data-stu-id="3c099-593">For the first option, you create a blob for every unique last name, and in each blob store a list of the **PartitionKey** (department) and **RowKey** (employee id) values for employees that have that last name.</span></span> <span data-ttu-id="3c099-594">När du lägger till eller ta bort en medarbetare bör du kontrollera att innehållet i den relevanta blobben är överensstämmelse med anställdas enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-594">When you add or delete an employee you should ensure that the content of the relevant blob is eventually consistent with the employee entities.</span></span>  

<span data-ttu-id="3c099-595"><u>Alternativ #2:</u> skapa index entiteter i samma partition</span><span class="sxs-lookup"><span data-stu-id="3c099-595"><u>Option #2:</u> Create index entities in the same partition</span></span>  

<span data-ttu-id="3c099-596">För det andra alternativet, använder du index entiteter som lagrar följande information:</span><span class="sxs-lookup"><span data-stu-id="3c099-596">For the second option, use index entities that store the following data:</span></span>  

![][14]

<span data-ttu-id="3c099-597">Den **EmployeeIDs** egenskapen innehåller en lista över medarbetare-ID för anställda med efternamn som lagras i den **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="3c099-597">The **EmployeeIDs** property contains a list of employee ids for employees with the last name stored in the **RowKey**.</span></span>  

<span data-ttu-id="3c099-598">Följande steg beskriver hur du bör följa när du lägger till en ny medarbetare om du använder det andra alternativet.</span><span class="sxs-lookup"><span data-stu-id="3c099-598">The following steps outline the process you should follow when you are adding a new employee if you are using the second option.</span></span> <span data-ttu-id="3c099-599">I det här exemplet lägger vi till en anställd med Id 000152 och efternamn Karlsson på försäljningsavdelningen:</span><span class="sxs-lookup"><span data-stu-id="3c099-599">In this example, we are adding an employee with Id 000152 and a last name Jones in the Sales department:</span></span>  

1. <span data-ttu-id="3c099-600">Hämta entiteten index med en **PartitionKey** värdet ”Försäljning” och **RowKey** värdet ”Jansson”.</span><span class="sxs-lookup"><span data-stu-id="3c099-600">Retrieve the index entity with a **PartitionKey** value "Sales" and the **RowKey** value "Jones."</span></span> <span data-ttu-id="3c099-601">Spara ETag för den här entiteten i steg 2.</span><span class="sxs-lookup"><span data-stu-id="3c099-601">Save the ETag of this entity to use in step 2.</span></span>  
2. <span data-ttu-id="3c099-602">Skapa entitet grupp transaktion (det vill säga en batchåtgärd) som infogar entiteten medarbetare (**PartitionKey** värdet ”Försäljning” och **RowKey** värdet ”000152”), och uppdaterar entiteten index (**PartitionKey** värdet ”Försäljning” och **RowKey** värdet ”Jansson”) genom att lägga till den nya medarbetare-id i listan i fältet EmployeeIDs.</span><span class="sxs-lookup"><span data-stu-id="3c099-602">Create an entity group transaction (that is, a batch operation) that inserts the new employee entity (**PartitionKey** value "Sales" and **RowKey** value "000152"), and updates the index entity (**PartitionKey** value "Sales" and **RowKey** value "Jones") by adding the new employee id to the list in the EmployeeIDs field.</span></span> <span data-ttu-id="3c099-603">Mer information om entiteten grupptransaktioner finns [entitet gruppera transaktioner](#entity-group-transactions).</span><span class="sxs-lookup"><span data-stu-id="3c099-603">For more information about entity group transactions, see [Entity Group Transactions](#entity-group-transactions).</span></span>  
3. <span data-ttu-id="3c099-604">Om entiteten grupp transaktionen misslyckas på grund av ett fel för Optimistisk samtidighet (någon annan har bara ändrade entiteten index), måste du börja om från steg 1 igen.</span><span class="sxs-lookup"><span data-stu-id="3c099-604">If the entity group transaction fails because of an optimistic concurrency error (someone else has just modified the index entity), then you need to start over at step 1 again.</span></span>  

<span data-ttu-id="3c099-605">Du kan använda ett liknande sätt att ta bort en medarbetare om du använder andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="3c099-605">You can use a similar approach to deleting an employee if you are using the second option.</span></span> <span data-ttu-id="3c099-606">Ändra en anställds efternamn är lite mer komplext eftersom du behöver köra en entitet grupp transaktion som uppdaterar tre enheter: medarbetare entiteten och entiteten index för det gamla efternamnet entiteten index för det nya efternamnet.</span><span class="sxs-lookup"><span data-stu-id="3c099-606">Changing an employee's last name is slightly more complex because you will need to execute an entity group transaction that updates three entities: the employee entity, the index entity for the old last name, and the index entity for the new last name.</span></span> <span data-ttu-id="3c099-607">Innan du gör några ändringar för att hämta ETag-värden som du sedan kan använda för att utföra uppdateringar med hjälp av Optimistisk samtidighet måste du hämta varje entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-607">You must retrieve each entity before making any changes in order to retrieve the ETag values that you can then use to perform the updates using optimistic concurrency.</span></span>  

<span data-ttu-id="3c099-608">Följande steg beskriver processen bör du följa när du behöver leta upp alla anställda med ett visst efternamn på en avdelning om du använder andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="3c099-608">The following steps outline the process you should follow when you need to look up all the employees with a given last name in a department if you are using the second option.</span></span> <span data-ttu-id="3c099-609">Det här exemplet letar vi upp alla anställda med efternamn Karlsson på försäljningsavdelningen:</span><span class="sxs-lookup"><span data-stu-id="3c099-609">In this example, we are looking up all the employees with last name Jones in the Sales department:</span></span>  

1. <span data-ttu-id="3c099-610">Hämta entiteten index med en **PartitionKey** värdet ”Försäljning” och **RowKey** värdet ”Jansson”.</span><span class="sxs-lookup"><span data-stu-id="3c099-610">Retrieve the index entity with a **PartitionKey** value "Sales" and the **RowKey** value "Jones."</span></span>  
2. <span data-ttu-id="3c099-611">Parsa listan över anställnings-ID i fältet EmployeeIDs.</span><span class="sxs-lookup"><span data-stu-id="3c099-611">Parse the list of employee Ids in the EmployeeIDs field.</span></span>  
3. <span data-ttu-id="3c099-612">Om du behöver ytterligare information om var och en av dessa anställda (till exempel sina e-postadresser) hämtar var och en av anställdas enheter med hjälp av **PartitionKey** värdet ”Försäljning” och **RowKey** värden från den lista över anställda som du hämtade i steg 2.</span><span class="sxs-lookup"><span data-stu-id="3c099-612">If you need additional information about each of these employees (such as their email addresses), retrieve each of the employee entities using **PartitionKey** value "Sales" and **RowKey** values from the list of employees you obtained in step 2.</span></span>  

<span data-ttu-id="3c099-613"><u>Alternativet #3:</u> skapa index entiteter i en separat partition eller tabell</span><span class="sxs-lookup"><span data-stu-id="3c099-613"><u>Option #3:</u> Create index entities in a separate partition or table</span></span>  

<span data-ttu-id="3c099-614">Det tredje alternativet Använd i index entiteter som lagrar följande information:</span><span class="sxs-lookup"><span data-stu-id="3c099-614">For the third option, use index entities that store the following data:</span></span>  

![][15]

<span data-ttu-id="3c099-615">Den **EmployeeIDs** egenskapen innehåller en lista över medarbetare-ID för anställda med efternamn som lagras i den **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="3c099-615">The **EmployeeIDs** property contains a list of employee ids for employees with the last name stored in the **RowKey**.</span></span>  

<span data-ttu-id="3c099-616">Du kan inte använda EGTs med det tredje alternativet för att upprätthålla enhetliga eftersom indexet entiteter i en separat partition från medarbetare entiteter.</span><span class="sxs-lookup"><span data-stu-id="3c099-616">With the third option, you cannot use EGTs to maintain consistency because the index entities are in a separate partition from the employee entities.</span></span> <span data-ttu-id="3c099-617">Du bör kontrollera att index entiteter är överensstämmelse med medarbetare entiteter.</span><span class="sxs-lookup"><span data-stu-id="3c099-617">You should ensure that the index entities are eventually consistent with the employee entities.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="3c099-618">Problem och överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-618">Issues and considerations</span></span>
<span data-ttu-id="3c099-619">Tänk på följande när du bestämmer hur du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-619">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="3c099-620">Denna lösning kräver minst två frågor för att hämta matchande entiteter: en för att fråga index entiteter för att hämta listan över **RowKey** värden och frågor för att hämta varje entitet i listan.</span><span class="sxs-lookup"><span data-stu-id="3c099-620">This solution requires at least two queries to retrieve matching entities: one to query the index entities to obtain the list of **RowKey** values, and then queries to retrieve each entity in the list.</span></span>  
* <span data-ttu-id="3c099-621">Med hänsyn till att en enskild entitet har en maximal storlek på 1 MB, förutsätter #2 och &#3; i lösningen att lista över medarbetare-ID för alla angivna efternamn aldrig är större än 1 MB.</span><span class="sxs-lookup"><span data-stu-id="3c099-621">Given that an individual entity has a maximum size of 1 MB, option #2 and option #3 in the solution assume that the list of employee ids for any given last name is never greater than 1 MB.</span></span> <span data-ttu-id="3c099-622">Om listan över medarbetare-ID: n är sannolikt måste vara större än 1 MB i storlek, Använd alternativet #1 och lagra indexinformationen i blob storage.</span><span class="sxs-lookup"><span data-stu-id="3c099-622">If the list of employee ids is likely to be greater than 1 MB in size, use option #1 and store the index data in blob storage.</span></span>  
* <span data-ttu-id="3c099-623">Om du använder alternativet #2 måste (med EGTs som hanterar att lägga till och ta bort anställda och ändra en anställds efternamn) du utvärdera om volymen av transaktioner, kommer hanterar skalbarhetsgränser i en given partition.</span><span class="sxs-lookup"><span data-stu-id="3c099-623">If you use option #2 (using EGTs to handle adding and deleting employees, and changing an employee's last name) you must evaluate if the volume of transactions will approach the scalability limits in a given partition.</span></span> <span data-ttu-id="3c099-624">Om så är fallet bör du överväga en överensstämmelse lösning (#1 eller #3) som använder köer för att hantera begäranden om uppdateringar och gör det möjligt att lagra indexet-entiteter i en separat partition från medarbetare entiteter.</span><span class="sxs-lookup"><span data-stu-id="3c099-624">If this is the case, you should consider an eventually consistent solution (option #1 or option #3) that uses queues to handle the update requests and enables you to store your index entities in a separate partition from the employee entities.</span></span>  
* <span data-ttu-id="3c099-625">Alternativet #2 i den här lösningen förutsätter att du vill leta upp efter efternamn inom en avdelning: till exempel du vill hämta en lista över anställda med efternamn Karlsson på försäljningsavdelningen.</span><span class="sxs-lookup"><span data-stu-id="3c099-625">Option #2 in this solution assumes that you want to look up by last name within a department: for example, you want to retrieve a list of employees with a last name Jones in the Sales department.</span></span> <span data-ttu-id="3c099-626">Om du vill kunna leta upp alla anställda med efternamn Karlsson i hela organisationen använda alternativet #1 eller alternativet #3.</span><span class="sxs-lookup"><span data-stu-id="3c099-626">If you want to be able to look up all the employees with a last name Jones across the whole organization, use either option #1 or option #3.</span></span>
* <span data-ttu-id="3c099-627">Du kan implementera en lösning med kön som levererar slutliga konsekvensen (finns i [överensstämmelse transaktioner mönster](#eventually-consistent-transactions-pattern) för mer information).</span><span class="sxs-lookup"><span data-stu-id="3c099-627">You can implement a queue-based solution that delivers eventual consistency (see the [Eventually consistent transactions pattern](#eventually-consistent-transactions-pattern) for more details).</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="3c099-628">När du ska använda det här mönstret</span><span class="sxs-lookup"><span data-stu-id="3c099-628">When to use this pattern</span></span>
<span data-ttu-id="3c099-629">Använd det här mönstret när du vill söka efter en mängd av entiteter med samma vanliga egenskapsvärde, till exempel alla anställda med efternamn Karlsson.</span><span class="sxs-lookup"><span data-stu-id="3c099-629">Use this pattern when you want to lookup a set of entities that all share a common property value, such as all employees with the last name Jones.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="3c099-630">Vägledning och relaterade mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-630">Related patterns and guidance</span></span>
<span data-ttu-id="3c099-631">Följande mönster och guider kan även vara relevanta när du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-631">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="3c099-632">Sammansatt nyckel mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-632">Compound key pattern</span></span>](#compound-key-pattern)  
* [<span data-ttu-id="3c099-633">Mönster för överensstämmelse transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-633">Eventually consistent transactions pattern</span></span>](#eventually-consistent-transactions-pattern)  
* [<span data-ttu-id="3c099-634">Entiteten gruppera transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-634">Entity Group Transactions</span></span>](#entity-group-transactions)  
* [<span data-ttu-id="3c099-635">Arbeta med heterogena entitetstyper</span><span class="sxs-lookup"><span data-stu-id="3c099-635">Working with heterogeneous entity types</span></span>](#working-with-heterogeneous-entity-types)  

### <a name="denormalization-pattern"></a><span data-ttu-id="3c099-636">Denormalization mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-636">Denormalization pattern</span></span>
<span data-ttu-id="3c099-637">Kombinera relaterade data tillsammans i en enda enhet så att du kan hämta alla data som du behöver med en enda fråga.</span><span class="sxs-lookup"><span data-stu-id="3c099-637">Combine related data together in a single entity to enable you to retrieve all the data you need with a single point query.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="3c099-638">Kontexten och problem</span><span class="sxs-lookup"><span data-stu-id="3c099-638">Context and problem</span></span>
<span data-ttu-id="3c099-639">I en relationsdatabas normalisera du normalt data för att ta bort duplicering, vilket resulterar i frågor som hämtar data från flera tabeller.</span><span class="sxs-lookup"><span data-stu-id="3c099-639">In a relational database, you typically normalize data to remove duplication resulting in queries that retrieve data from multiple tables.</span></span> <span data-ttu-id="3c099-640">Om du normalisera dina data i Azure-tabeller, måste du se flera sändningar fram och tillbaka från klienten till servern för att hämta relaterade data.</span><span class="sxs-lookup"><span data-stu-id="3c099-640">If you normalize your data in Azure tables, you must make multiple round trips from the client to the server to retrieve your related data.</span></span> <span data-ttu-id="3c099-641">Till exempel med tabellstrukturen nedan om du behöver två sändningar att hämta information för en avdelning: en för att hämta entiteten avdelningen som innehåller managerns-id och sedan en annan begäran om att hämta information om den manager i en medarbetare entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-641">For example, with the table structure shown below you need two round trips to retrieve the details for a department: one to fetch the department entity that includes the manager's id, and then another request to fetch the manager's details in an employee entity.</span></span>  

![][16]

#### <a name="solution"></a><span data-ttu-id="3c099-642">Lösning</span><span class="sxs-lookup"><span data-stu-id="3c099-642">Solution</span></span>
<span data-ttu-id="3c099-643">I stället för att lagra data i två separata entiteterna, denormalize data och behålla en kopia av den manager information i entiteten avdelning.</span><span class="sxs-lookup"><span data-stu-id="3c099-643">Instead of storing the data in two separate entities, denormalize the data and keep a copy of the manager's details in the department entity.</span></span> <span data-ttu-id="3c099-644">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3c099-644">For example:</span></span>  

![][17]

<span data-ttu-id="3c099-645">Du kan nu hämta allt du behöver om en avdelning med en punkt-fråga med avdelning entiteter som lagras med dessa egenskaper finns.</span><span class="sxs-lookup"><span data-stu-id="3c099-645">With department entities stored with these properties, you can now retrieve all the details you need about a department using a point query.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="3c099-646">Problem och överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-646">Issues and considerations</span></span>
<span data-ttu-id="3c099-647">Tänk på följande när du bestämmer hur du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-647">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="3c099-648">Det finns en kostnad som associeras med att lagra vissa data två gånger.</span><span class="sxs-lookup"><span data-stu-id="3c099-648">There is some cost overhead associated with storing some data twice.</span></span> <span data-ttu-id="3c099-649">Prestandafördelarna (följd färre begäranden till lagringstjänsten) vanligtvis uppväger marginell ökningen lagringskostnader (och kostnaden är delvis förskjuten genom en minskning av antal transaktioner som du behöver för att hämta information om en avdelning ).</span><span class="sxs-lookup"><span data-stu-id="3c099-649">The performance benefit (resulting from fewer requests to the storage service) typically outweighs the marginal increase in storage costs (and this cost is partially offset by a reduction in the number of transactions you require to fetch the details of a department).</span></span>  
* <span data-ttu-id="3c099-650">Du måste upprätthålla konsekvensen för de två entiteter som lagrar information om chefer.</span><span class="sxs-lookup"><span data-stu-id="3c099-650">You must maintain the consistency of the two entities that store information about managers.</span></span> <span data-ttu-id="3c099-651">Du kan hantera konsekvenskontroll problemet med hjälp av EGTs för att uppdatera flera entiteter i en atomisk transaktion: i det här fallet avdelning entiteten och medarbetare entiteten för avdelning manager lagras i samma partition.</span><span class="sxs-lookup"><span data-stu-id="3c099-651">You can handle the consistency issue by using EGTs to update multiple entities in a single atomic transaction: in this case, the department entity, and the employee entity for the department manager are stored in the same partition.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="3c099-652">När du ska använda det här mönstret</span><span class="sxs-lookup"><span data-stu-id="3c099-652">When to use this pattern</span></span>
<span data-ttu-id="3c099-653">Använd det här mönstret när du ofta behöver leta upp relaterad information.</span><span class="sxs-lookup"><span data-stu-id="3c099-653">Use this pattern when you frequently need to look up related information.</span></span> <span data-ttu-id="3c099-654">Det här mönstret minskar antalet frågor som klienten måste se till att hämta de data som krävs.</span><span class="sxs-lookup"><span data-stu-id="3c099-654">This pattern reduces the number of queries your client must make to retrieve the data it requires.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="3c099-655">Vägledning och relaterade mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-655">Related patterns and guidance</span></span>
<span data-ttu-id="3c099-656">Följande mönster och guider kan även vara relevanta när du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-656">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="3c099-657">Sammansatt nyckel mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-657">Compound key pattern</span></span>](#compound-key-pattern)  
* [<span data-ttu-id="3c099-658">Entiteten gruppera transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-658">Entity Group Transactions</span></span>](#entity-group-transactions)  
* [<span data-ttu-id="3c099-659">Arbeta med heterogena entitetstyper</span><span class="sxs-lookup"><span data-stu-id="3c099-659">Working with heterogeneous entity types</span></span>](#working-with-heterogeneous-entity-types)

### <a name="compound-key-pattern"></a><span data-ttu-id="3c099-660">Sammansatt nyckel mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-660">Compound key pattern</span></span>
<span data-ttu-id="3c099-661">Använd sammansatta **RowKey** värden att aktivera en klient att söka efter relaterade data med en enda fråga.</span><span class="sxs-lookup"><span data-stu-id="3c099-661">Use compound **RowKey** values to enable a client to lookup related data with a single point query.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="3c099-662">Kontexten och problem</span><span class="sxs-lookup"><span data-stu-id="3c099-662">Context and problem</span></span>
<span data-ttu-id="3c099-663">I en relationsdatabas är det ganska naturlig använda kopplingar i frågor för att returnera relaterade delar av data till klienten i en enskild fråga.</span><span class="sxs-lookup"><span data-stu-id="3c099-663">In a relational database, it is quite natural to use joins in queries to return related pieces of data to the client in a single query.</span></span> <span data-ttu-id="3c099-664">Du kan till exempel använda anställnings-id för att leta upp en lista över relaterade entiteter som innehåller prestanda och granska data för denna medarbetare.</span><span class="sxs-lookup"><span data-stu-id="3c099-664">For example, you might use the employee id to look up a list of related entities that contain performance and review data for that employee.</span></span>  

<span data-ttu-id="3c099-665">Anta att du lagrar medarbetare entiteter i tabelltjänsten med följande struktur:</span><span class="sxs-lookup"><span data-stu-id="3c099-665">Assume you are storing employee entities in the Table service using the following structure:</span></span>  

![][18]

<span data-ttu-id="3c099-666">Du måste också att lagra historiska data som rör omdömen och prestanda för varje år medarbetaren har arbetat för din organisation och du behöver kunna komma åt informationen per år.</span><span class="sxs-lookup"><span data-stu-id="3c099-666">You also need to store historical data relating to reviews and performance for each year the employee has worked for your organization and you need to be able to access this information by year.</span></span> <span data-ttu-id="3c099-667">Ett alternativ är att skapa en annan tabell som innehåller entiteter med följande struktur:</span><span class="sxs-lookup"><span data-stu-id="3c099-667">One option is to create another table that stores entities with the following structure:</span></span>  

![][19]

<span data-ttu-id="3c099-668">Observera att med den här metoden kan du välja att duplicera viss information (till exempel förnamn och efternamn) i den nya enheten så att du kan hämta dina data med en enskild begäran.</span><span class="sxs-lookup"><span data-stu-id="3c099-668">Notice that with this approach you may decide to duplicate some information (such as first name and last name) in the new entity to enable you to retrieve your data with a single request.</span></span> <span data-ttu-id="3c099-669">Du kan dock behålla stark konsekvens eftersom du inte kan använda en EGT att automatiskt uppdatera två entiteter.</span><span class="sxs-lookup"><span data-stu-id="3c099-669">However, you cannot maintain strong consistency because you cannot use an EGT to update the two entities atomically.</span></span>  

#### <a name="solution"></a><span data-ttu-id="3c099-670">Lösning</span><span class="sxs-lookup"><span data-stu-id="3c099-670">Solution</span></span>
<span data-ttu-id="3c099-671">Lagra nya entitetstypen i den ursprungliga tabellen med hjälp av entiteter med följande struktur:</span><span class="sxs-lookup"><span data-stu-id="3c099-671">Store a new entity type in your original table using entities with the following structure:</span></span>  

![][20]

<span data-ttu-id="3c099-672">Observera hur **RowKey** är nu en sammansatt nyckel består av anställnings-id och år granska data som gör att du kan hämta den anställde prestanda och granska data med en begäran för en enda entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-672">Notice how the **RowKey** is now a compound key made up of the employee id and the year of the review data that enables you to retrieve the employee's performance and review data with a single request for a single entity.</span></span>  

<span data-ttu-id="3c099-673">I följande exempel beskrivs hur du kan hämta alla data för granskning för en viss medarbetare (till exempel medarbetare 000123 på försäljningsavdelningen):</span><span class="sxs-lookup"><span data-stu-id="3c099-673">The following example outlines how you can retrieve all the review data for a particular employee (such as employee 000123 in the Sales department):</span></span>  

<span data-ttu-id="3c099-674">$filter = (PartitionKey eq 'Sales') och (RowKey ge 'empid_000123') och (RowKey lt 'empid_000124') & $select = RowKey, Arbetsledarens, peer-klassificering, kommentarer</span><span class="sxs-lookup"><span data-stu-id="3c099-674">$filter=(PartitionKey eq 'Sales') and (RowKey ge 'empid_000123') and (RowKey lt 'empid_000124')&$select=RowKey,Manager Rating,Peer Rating,Comments</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="3c099-675">Problem och överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-675">Issues and considerations</span></span>
<span data-ttu-id="3c099-676">Tänk på följande när du bestämmer hur du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-676">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="3c099-677">Du bör använda ett lämpligt avgränsningstecknet som gör det enkelt att parsa den **RowKey** värde: till exempel **000123_2012**.</span><span class="sxs-lookup"><span data-stu-id="3c099-677">You should use a suitable separator character that makes it easy to parse the **RowKey** value: for example, **000123_2012**.</span></span>  
* <span data-ttu-id="3c099-678">Den här entiteten lagras också i samma partition som andra entiteter som innehåller relaterade data för samma medarbetare, vilket innebär att du kan använda EGTs för att underhålla stark konsekvens.</span><span class="sxs-lookup"><span data-stu-id="3c099-678">You are also storing this entity in the same partition as other entities that contain related data for the same employee, which means you can use EGTs to maintain strong consistency.</span></span>
* <span data-ttu-id="3c099-679">Du bör överväga hur ofta du ska fråga efter data om det här mönstret är rätt.</span><span class="sxs-lookup"><span data-stu-id="3c099-679">You should consider how frequently you will query the data to determine whether this pattern is appropriate.</span></span>  <span data-ttu-id="3c099-680">Till exempel om du kommer åt data granska sällan och huvudsakliga medarbetardata ofta bör du behålla dem som separata entiteterna.</span><span class="sxs-lookup"><span data-stu-id="3c099-680">For example, if you will access the review data infrequently and the main employee data often you should keep them as separate entities.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="3c099-681">När du ska använda det här mönstret</span><span class="sxs-lookup"><span data-stu-id="3c099-681">When to use this pattern</span></span>
<span data-ttu-id="3c099-682">Använd det här mönstret när du behöver lagra en eller flera relaterade entiteter som du frågar ofta.</span><span class="sxs-lookup"><span data-stu-id="3c099-682">Use this pattern when you need to store one or more related entities that you query frequently.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="3c099-683">Vägledning och relaterade mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-683">Related patterns and guidance</span></span>
<span data-ttu-id="3c099-684">Följande mönster och guider kan även vara relevanta när du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-684">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="3c099-685">Entiteten gruppera transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-685">Entity Group Transactions</span></span>](#entity-group-transactions)  
* [<span data-ttu-id="3c099-686">Arbeta med heterogena entitetstyper</span><span class="sxs-lookup"><span data-stu-id="3c099-686">Working with heterogeneous entity types</span></span>](#working-with-heterogeneous-entity-types)  
* [<span data-ttu-id="3c099-687">Mönster för överensstämmelse transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-687">Eventually consistent transactions pattern</span></span>](#eventually-consistent-transactions-pattern)  

### <a name="log-tail-pattern"></a><span data-ttu-id="3c099-688">Loggen pilslut mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-688">Log tail pattern</span></span>
<span data-ttu-id="3c099-689">Hämta den  *n*  entiteter som senast lades till en partition med hjälp av en **RowKey** värde som sorterar i omvänd datum och tid ordning.</span><span class="sxs-lookup"><span data-stu-id="3c099-689">Retrieve the *n* entities most recently added to a partition by using a **RowKey** value that sorts in reverse date and time order.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="3c099-690">Kontexten och problem</span><span class="sxs-lookup"><span data-stu-id="3c099-690">Context and problem</span></span>
<span data-ttu-id="3c099-691">Ett vanligt krav är att kunna hämta de nyligen skapade enheterna, till exempel de senaste tio utgifter ansökningar som görs av en medarbetare.</span><span class="sxs-lookup"><span data-stu-id="3c099-691">A common requirement is be able to retrieve the most recently created entities, for example the ten most recent expense claims submitted by an employee.</span></span> <span data-ttu-id="3c099-692">Tabell frågar stöd för en **$top** frågeåtgärden att returnera först  *n*  entiteter från en uppsättning: Ingen åtgärd har motsvarande frågan att returnera de sista n entiteterna i en mängd.</span><span class="sxs-lookup"><span data-stu-id="3c099-692">Table queries support a **$top** query operation to return the first *n* entities from a set: there is no equivalent query operation to return the last n entities in a set.</span></span>  

#### <a name="solution"></a><span data-ttu-id="3c099-693">Lösning</span><span class="sxs-lookup"><span data-stu-id="3c099-693">Solution</span></span>
<span data-ttu-id="3c099-694">Lagra enheter med hjälp av en **RowKey** att naturligt sorterar i tidsvärdet i omvänd ordning med hjälp av det senaste posten är alltid den första i tabellen.</span><span class="sxs-lookup"><span data-stu-id="3c099-694">Store the entities using a **RowKey** that naturally sorts in reverse date/time order by using so the most recent entry is always the first one in the table.</span></span>  

<span data-ttu-id="3c099-695">Du kan till exempel använda en omvänd tick-värde som härletts från aktuellt datum och tid för att kunna hämta tio senaste utgifter anspråk skickas av en medarbetare.</span><span class="sxs-lookup"><span data-stu-id="3c099-695">For example, to be able to retrieve the ten most recent expense claims submitted by an employee, you can use a reverse tick value derived from the current date/time.</span></span> <span data-ttu-id="3c099-696">Följande C# kodexemplet visar ett sätt att skapa ett lämpligt ”inverterad tick” värde för en **RowKey** som sorterar från den senaste äldsta:</span><span class="sxs-lookup"><span data-stu-id="3c099-696">The following C# code sample shows one way to create a suitable "inverted ticks" value for a **RowKey** that sorts from the most recent to the oldest:</span></span>  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

<span data-ttu-id="3c099-697">Du kan komma tillbaka till datum tid-värden med hjälp av följande kod:</span><span class="sxs-lookup"><span data-stu-id="3c099-697">You can get back to the date time value using the following code:</span></span>  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

<span data-ttu-id="3c099-698">Frågan för tabellen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="3c099-698">The table query looks like this:</span></span>  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

#### <a name="issues-and-considerations"></a><span data-ttu-id="3c099-699">Problem och överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-699">Issues and considerations</span></span>
<span data-ttu-id="3c099-700">Tänk på följande när du bestämmer hur du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-700">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="3c099-701">Du måste fylla värdet omvänd skalstreck med inledande nollor så strängvärdet sorterar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="3c099-701">You must pad the reverse tick value with leading zeroes to ensure the string value sorts as expected.</span></span>  
* <span data-ttu-id="3c099-702">Du måste vara medveten om skalbarhetsmål på nivån för en partition.</span><span class="sxs-lookup"><span data-stu-id="3c099-702">You must be aware of the scalability targets at the level of a partition.</span></span> <span data-ttu-id="3c099-703">Var noga med att inte skapa hotspot-partitioner.</span><span class="sxs-lookup"><span data-stu-id="3c099-703">Be careful not create hot spot partitions.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="3c099-704">När du ska använda det här mönstret</span><span class="sxs-lookup"><span data-stu-id="3c099-704">When to use this pattern</span></span>
<span data-ttu-id="3c099-705">Använd det här mönstret när du behöver åtkomst till entiteter i tidsvärdet i omvänd ordning eller när du behöver åtkomst till de nyligen tillagda entiteterna.</span><span class="sxs-lookup"><span data-stu-id="3c099-705">Use this pattern when you need to access entities in reverse date/time order or when you need to access the most recently added entities.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="3c099-706">Vägledning och relaterade mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-706">Related patterns and guidance</span></span>
<span data-ttu-id="3c099-707">Följande mönster och guider kan även vara relevanta när du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-707">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="3c099-708">Lägga / lägga till ett mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-708">Prepend / append anti-pattern</span></span>](#prepend-append-anti-pattern)  
* [<span data-ttu-id="3c099-709">Hämta entiteter</span><span class="sxs-lookup"><span data-stu-id="3c099-709">Retrieving entities</span></span>](#retrieving-entities)  

### <a name="high-volume-delete-pattern"></a><span data-ttu-id="3c099-710">Ta bort mönster för hög volym</span><span class="sxs-lookup"><span data-stu-id="3c099-710">High volume delete pattern</span></span>
<span data-ttu-id="3c099-711">Aktivera borttagning av ett stort antal enheter genom att lagra alla entiteter för samtidiga borttagning i sina egna separata tabell. du tar bort entiteterna genom att ta bort tabellen.</span><span class="sxs-lookup"><span data-stu-id="3c099-711">Enable the deletion of a high volume of entities by storing all the entities for simultaneous deletion in their own separate table; you delete the entities by deleting the table.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="3c099-712">Kontexten och problem</span><span class="sxs-lookup"><span data-stu-id="3c099-712">Context and problem</span></span>
<span data-ttu-id="3c099-713">Många program att ta bort gamla data som inte längre behöver vara tillgängliga för ett klientprogram, eller som programmet har arkiverats till ett annat lagringsmedium.</span><span class="sxs-lookup"><span data-stu-id="3c099-713">Many applications delete old data which no longer needs to be available to a client application, or that the application has archived to another storage medium.</span></span> <span data-ttu-id="3c099-714">Dessa data identifieras vanligtvis med ett datum: du till exempel har ett krav för att ta bort poster för alla inloggningsbegäranden som är mer än 60 dagar.</span><span class="sxs-lookup"><span data-stu-id="3c099-714">You typically identify such data by a date: for example, you have a requirement to delete records of all login requests that are more than 60 days old.</span></span>  

<span data-ttu-id="3c099-715">Ett möjligt design är att använda datum och tid för inloggningsbegäran i den **RowKey**:</span><span class="sxs-lookup"><span data-stu-id="3c099-715">One possible design is to use the date and time of the login request in the **RowKey**:</span></span>  

![][21]

<span data-ttu-id="3c099-716">Det här sättet undviker partition surfpunkter eftersom programmet kan infoga och ta bort inloggningen entiteter för varje användare i en separat partition.</span><span class="sxs-lookup"><span data-stu-id="3c099-716">This approach avoids partition hotspots because the application can insert and delete login entities for each user in a separate partition.</span></span> <span data-ttu-id="3c099-717">Den här metoden kan dock dyrt och tidskrävande om du har ett stort antal entiteter eftersom du måste först utför en tabellgenomsökning för att identifiera alla enheter att ta bort och du måste ta bort varje gamla entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-717">However, this approach may be costly and time consuming if you have a large number of entities because first you need to perform a table scan in order to identify all the entities to delete, and then you must delete each old entity.</span></span> <span data-ttu-id="3c099-718">Observera att du kan minska antalet sändningar till servern som krävs för att ta bort de gamla enheterna av batchbearbetning flera delete-begäranden till EGTs.</span><span class="sxs-lookup"><span data-stu-id="3c099-718">Note that you can reduce the number of round trips to the server required to delete the old entities by batching multiple delete requests into EGTs.</span></span>  

#### <a name="solution"></a><span data-ttu-id="3c099-719">Lösning</span><span class="sxs-lookup"><span data-stu-id="3c099-719">Solution</span></span>
<span data-ttu-id="3c099-720">Använd en separat tabell för varje dag på inloggningsförsök.</span><span class="sxs-lookup"><span data-stu-id="3c099-720">Use a separate table for each day of login attempts.</span></span> <span data-ttu-id="3c099-721">Du kan använda entiteten designen ovan för att undvika surfpunkter när du infogar entiteter och ta bort gamla enheter är nu bara en fråga om du tar bort en tabell varje dag (en enda Lagringsåtgärden) i stället för att hitta och ta bort hundratals och tusentals person inloggningen enheter varje dag.</span><span class="sxs-lookup"><span data-stu-id="3c099-721">You can use the entity design above to avoid hotspots when you are inserting entities, and deleting old entities is now simply a question of deleting one table every day (a single storage operation) instead of finding and deleting hundreds and thousands of individual login entities every day.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="3c099-722">Problem och överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-722">Issues and considerations</span></span>
<span data-ttu-id="3c099-723">Tänk på följande när du bestämmer hur du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-723">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="3c099-724">Stöder din design andra sätt som programmet ska använda data, till exempel söka efter specifika enheter som länkar till andra data eller genererar samlar in information?</span><span class="sxs-lookup"><span data-stu-id="3c099-724">Does your design support other ways your application will use the data such as looking up specific entities, linking with other data, or generating aggregate information?</span></span>  
* <span data-ttu-id="3c099-725">Din design undvika aktiva punkter vid infogning av nya enheter?</span><span class="sxs-lookup"><span data-stu-id="3c099-725">Does your design avoid hot spots when you are inserting new entities?</span></span>  
* <span data-ttu-id="3c099-726">Förvänta dig en fördröjning om du vill återanvända samma namn när du tar bort den.</span><span class="sxs-lookup"><span data-stu-id="3c099-726">Expect a delay if you want to reuse the same table name after deleting it.</span></span> <span data-ttu-id="3c099-727">Det är bättre att alltid använda unikt tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="3c099-727">It's better to always use unique table names.</span></span>  
* <span data-ttu-id="3c099-728">Räkna med vissa begränsningar när du börjar använda en ny tabell medan tabelltjänsten lär sig åtkomstmönster och distribuerar partitionerna mellan noder.</span><span class="sxs-lookup"><span data-stu-id="3c099-728">Expect some throttling when you first use a new table while the Table service learns the access patterns and distributes the partitions across nodes.</span></span> <span data-ttu-id="3c099-729">Du bör överväga hur ofta du behöver skapa nya tabeller.</span><span class="sxs-lookup"><span data-stu-id="3c099-729">You should consider how frequently you need to create new tables.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="3c099-730">När du ska använda det här mönstret</span><span class="sxs-lookup"><span data-stu-id="3c099-730">When to use this pattern</span></span>
<span data-ttu-id="3c099-731">Använd det här mönstret när du har en stor volym med enheter som du måste ta bort samtidigt.</span><span class="sxs-lookup"><span data-stu-id="3c099-731">Use this pattern when you have a high volume of entities that you must delete at the same time.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="3c099-732">Vägledning och relaterade mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-732">Related patterns and guidance</span></span>
<span data-ttu-id="3c099-733">Följande mönster och guider kan även vara relevanta när du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-733">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="3c099-734">Entiteten gruppera transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-734">Entity Group Transactions</span></span>](#entity-group-transactions)
* [<span data-ttu-id="3c099-735">Ändra entiteter</span><span class="sxs-lookup"><span data-stu-id="3c099-735">Modifying entities</span></span>](#modifying-entities)  

### <a name="data-series-pattern"></a><span data-ttu-id="3c099-736">Serien datamönster</span><span class="sxs-lookup"><span data-stu-id="3c099-736">Data series pattern</span></span>
<span data-ttu-id="3c099-737">Store fullständig dataserier i en enda enhet för att minimera antalet begäranden som du gör.</span><span class="sxs-lookup"><span data-stu-id="3c099-737">Store complete data series in a single entity to minimize the number of requests you make.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="3c099-738">Kontexten och problem</span><span class="sxs-lookup"><span data-stu-id="3c099-738">Context and problem</span></span>
<span data-ttu-id="3c099-739">Ett vanligt scenario är för ett program att lagra en serie av data som normalt behöver hämta allt samtidigt.</span><span class="sxs-lookup"><span data-stu-id="3c099-739">A common scenario is for an application to store a series of data that it typically needs to retrieve all at once.</span></span> <span data-ttu-id="3c099-740">Programmet kan till exempel registrera hur många Snabbmeddelanden meddelanden varje anställd skickar varje timme och sedan använda informationen för att rita ut hur många meddelanden varje användare som skickas över de föregående 24 timmarna.</span><span class="sxs-lookup"><span data-stu-id="3c099-740">For example, your application might record how many IM messages each employee sends every hour, and then use this information to plot how many messages each user sent over the preceding 24 hours.</span></span> <span data-ttu-id="3c099-741">En kan vara att lagra 24 entiteter för varje medarbetare:</span><span class="sxs-lookup"><span data-stu-id="3c099-741">One design might be to store 24 entities for each employee:</span></span>  

![][22]

<span data-ttu-id="3c099-742">Med den här designen kan du lätt hitta och uppdatera entitet för varje medarbetare uppdateras när programmet måste uppdatera värdet för antal meddelande.</span><span class="sxs-lookup"><span data-stu-id="3c099-742">With this design, you can easily locate and update the entity to update for each employee whenever the application needs to update the message count value.</span></span> <span data-ttu-id="3c099-743">Om du vill hämta information för att rita ett diagram för aktiviteten under de föregående 24 timmarna, måste du hämta 24 entiteter.</span><span class="sxs-lookup"><span data-stu-id="3c099-743">However, to retrieve the information to plot a chart of the activity for the preceding 24 hours, you must retrieve 24 entities.</span></span>  

#### <a name="solution"></a><span data-ttu-id="3c099-744">Lösning</span><span class="sxs-lookup"><span data-stu-id="3c099-744">Solution</span></span>
<span data-ttu-id="3c099-745">Använd följande design med en separat egenskap för att lagra antalet meddelanden för varje timme:</span><span class="sxs-lookup"><span data-stu-id="3c099-745">Use the following design with a separate property to store the message count for each hour:</span></span>  

![][23]

<span data-ttu-id="3c099-746">Du kan använda en sammanfogning med den här designen för att uppdatera antalet meddelanden för en medarbetare för en given timme.</span><span class="sxs-lookup"><span data-stu-id="3c099-746">With this design, you can use a merge operation to update the message count for an employee for a specific hour.</span></span> <span data-ttu-id="3c099-747">Du kan nu hämta all information som du behöver att rita diagram med en begäran för en enda entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-747">Now, you can retrieve all the information you need to plot the chart using a request for a single entity.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="3c099-748">Problem och överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-748">Issues and considerations</span></span>
<span data-ttu-id="3c099-749">Tänk på följande när du bestämmer hur du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-749">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="3c099-750">Om fullständig dataserierna inte får plats i en enda entitet (en entitet kan ha upp till 252 egenskaper), kan du använda ett alternativt datalager, till exempel en blob.</span><span class="sxs-lookup"><span data-stu-id="3c099-750">If your complete data series does not fit into a single entity (an entity can have up to 252 properties), use an alternative data store such as a blob.</span></span>  
* <span data-ttu-id="3c099-751">Om du har flera klienter som uppdaterar en entitet samtidigt, du behöver använda den **ETag** att implementera Optimistisk samtidighet.</span><span class="sxs-lookup"><span data-stu-id="3c099-751">If you have multiple clients updating an entity simultaneously, you will need to use the **ETag** to implement optimistic concurrency.</span></span> <span data-ttu-id="3c099-752">Om du har många klienter kan uppstå det hög konkurrens.</span><span class="sxs-lookup"><span data-stu-id="3c099-752">If you have many clients, you may experience high contention.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="3c099-753">När du ska använda det här mönstret</span><span class="sxs-lookup"><span data-stu-id="3c099-753">When to use this pattern</span></span>
<span data-ttu-id="3c099-754">Använd det här mönstret när du behöver uppdatera och hämta en serie som är associerade med en enskild entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-754">Use this pattern when you need to update and retrieve a data series associated with an individual entity.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="3c099-755">Vägledning och relaterade mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-755">Related patterns and guidance</span></span>
<span data-ttu-id="3c099-756">Följande mönster och guider kan även vara relevanta när du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-756">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="3c099-757">Mönster för stora entiteter</span><span class="sxs-lookup"><span data-stu-id="3c099-757">Large entities pattern</span></span>](#large-entities-pattern)  
* [<span data-ttu-id="3c099-758">Merge eller ersätta</span><span class="sxs-lookup"><span data-stu-id="3c099-758">Merge or replace</span></span>](#merge-or-replace)  
* <span data-ttu-id="3c099-759">[Överensstämmelse transaktioner mönster](#eventually-consistent-transactions-pattern) (om du lagrar dataserien i en blob)</span><span class="sxs-lookup"><span data-stu-id="3c099-759">[Eventually consistent transactions pattern](#eventually-consistent-transactions-pattern) (if you are storing the data series in a blob)</span></span>  

### <a name="wide-entities-pattern"></a><span data-ttu-id="3c099-760">Wide entiteter mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-760">Wide entities pattern</span></span>
<span data-ttu-id="3c099-761">Använda flera fysiska enheter för att lagra logiska entiteter med mer än 252 egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3c099-761">Use multiple physical entities to store logical entities with more than 252 properties.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="3c099-762">Kontexten och problem</span><span class="sxs-lookup"><span data-stu-id="3c099-762">Context and problem</span></span>
<span data-ttu-id="3c099-763">En enskild entitet kan ha högst 252 egenskaper (exklusive obligatoriska Systemegenskaper) och kan inte lagra mer än 1 MB data totalt.</span><span class="sxs-lookup"><span data-stu-id="3c099-763">An individual entity can have no more than 252 properties (excluding the mandatory system properties) and cannot store more than 1 MB of data in total.</span></span> <span data-ttu-id="3c099-764">I en relationsdatabas, skulle du normalt får avrunda några gränser för storleken på en rad att lägga till en ny tabell och framtvinga en 1-till-1-relation mellan dem.</span><span class="sxs-lookup"><span data-stu-id="3c099-764">In a relational database, you would typically get round any limits on the size of a row by adding a new table and enforcing a 1-to-1 relationship between them.</span></span>  

#### <a name="solution"></a><span data-ttu-id="3c099-765">Lösning</span><span class="sxs-lookup"><span data-stu-id="3c099-765">Solution</span></span>
<span data-ttu-id="3c099-766">Du kan använda tabelltjänsten för att lagra flera entiteter som representerar ett enda stort företag objekt med mer än 252 egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3c099-766">Using the Table service, you can store multiple entities to represent a single large business object with more than 252 properties.</span></span> <span data-ttu-id="3c099-767">Om du vill spara en uppräkning av antalet IM-meddelanden som skickas av medarbetaren för de senaste 365 dagarna kan du använda följande designen som använder två entiteter med olika scheman:</span><span class="sxs-lookup"><span data-stu-id="3c099-767">For example, if you want to store a count of the number of IM messages sent by each employee for the last 365 days, you could use the following design that uses two entities with different schemas:</span></span>  

![][24]

<span data-ttu-id="3c099-768">Du kan använda en EGT om du behöver göra en ändring som kräver uppdatering båda entiteter för att hålla dem synkroniserade med varandra.</span><span class="sxs-lookup"><span data-stu-id="3c099-768">If you need to make a change that requires updating both entities to keep them synchronized with each other you can use an EGT.</span></span> <span data-ttu-id="3c099-769">Annars kan du använda en enda merge-operation för att uppdatera antalet meddelanden för en viss dag.</span><span class="sxs-lookup"><span data-stu-id="3c099-769">Otherwise, you can use a single merge operation to update the message count for a specific day.</span></span> <span data-ttu-id="3c099-770">För att hämta alla data för en enskild medarbetare måste du hämta båda enheter som du kan göra med två effektivt begäranden som använder både ett **PartitionKey** och en **RowKey** värde.</span><span class="sxs-lookup"><span data-stu-id="3c099-770">To retrieve all the data for an individual employee you must retrieve both entities, which you can do with two efficient requests that use both a **PartitionKey** and a **RowKey** value.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="3c099-771">Problem och överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-771">Issues and considerations</span></span>
<span data-ttu-id="3c099-772">Tänk på följande när du bestämmer hur du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-772">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="3c099-773">Hämtar en fullständig logisk entitet omfattar minst två lagringstransaktioner: en för att hämta varje fysisk entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-773">Retrieving a complete logical entity involves at least two storage transactions: one to retrieve each physical entity.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="3c099-774">När du ska använda det här mönstret</span><span class="sxs-lookup"><span data-stu-id="3c099-774">When to use this pattern</span></span>
<span data-ttu-id="3c099-775">Använd det här mönstret när behöver lagra enheter vars storlek eller ett antal egenskaper som överskrider gränserna för en enskild entitet i tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-775">Use this pattern when  need to store entities whose size or number of properties exceeds the limits for an individual entity in the Table service.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="3c099-776">Vägledning och relaterade mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-776">Related patterns and guidance</span></span>
<span data-ttu-id="3c099-777">Följande mönster och guider kan även vara relevanta när du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-777">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="3c099-778">Entiteten gruppera transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-778">Entity Group Transactions</span></span>](#entity-group-transactions)
* [<span data-ttu-id="3c099-779">Merge eller ersätta</span><span class="sxs-lookup"><span data-stu-id="3c099-779">Merge or replace</span></span>](#merge-or-replace)

### <a name="large-entities-pattern"></a><span data-ttu-id="3c099-780">Mönster för stora entiteter</span><span class="sxs-lookup"><span data-stu-id="3c099-780">Large entities pattern</span></span>
<span data-ttu-id="3c099-781">Använda blob storage för att lagra stora egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="3c099-781">Use blob storage to store large property values.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="3c099-782">Kontexten och problem</span><span class="sxs-lookup"><span data-stu-id="3c099-782">Context and problem</span></span>
<span data-ttu-id="3c099-783">En enskild entitet kan inte lagra mer än 1 MB data totalt.</span><span class="sxs-lookup"><span data-stu-id="3c099-783">An individual entity cannot store more than 1 MB of data in total.</span></span> <span data-ttu-id="3c099-784">Om en eller flera av dina egenskaper lagrar värden som orsakar den totala storleken på din enhet överskrider detta värde kan lagra du inte hela entiteten i tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-784">If one or several of your properties store values that cause the total size of your entity to exceed this value, you cannot store the entire entity in the Table service.</span></span>  

#### <a name="solution"></a><span data-ttu-id="3c099-785">Lösning</span><span class="sxs-lookup"><span data-stu-id="3c099-785">Solution</span></span>
<span data-ttu-id="3c099-786">Om entiteten överskrider 1 MB i storlek eftersom en eller flera egenskaper innehåller stora mängder data, du lagra data i Blob-tjänsten och sedan lagra den blob-adressen i en egenskap i entiteten.</span><span class="sxs-lookup"><span data-stu-id="3c099-786">If your entity exceeds 1 MB in size because one or more properties contain a large amount of data, you can store data in the Blob service and then store the address of the blob in a property in the entity.</span></span> <span data-ttu-id="3c099-787">Du kan till exempel lagra foto av en medarbetare i blob storage och lagra en länk till bilden i den **foto** egenskap för dina medarbetare entitet:</span><span class="sxs-lookup"><span data-stu-id="3c099-787">For example, you can store the photo of an employee in blob storage and store a link to the photo in the **Photo** property of your employee entity:</span></span>  

![][25]

#### <a name="issues-and-considerations"></a><span data-ttu-id="3c099-788">Problem och överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-788">Issues and considerations</span></span>
<span data-ttu-id="3c099-789">Tänk på följande när du bestämmer hur du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-789">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="3c099-790">Använda eventuell enhetliga mellan entiteten i tabelltjänsten och data i Blob-tjänsten på [överensstämmelse transaktioner mönster](#eventually-consistent-transactions-pattern) att underhålla din entiteter.</span><span class="sxs-lookup"><span data-stu-id="3c099-790">To maintain eventual consistency between the entity in the Table service and the data in the Blob service, use the [Eventually consistent transactions pattern](#eventually-consistent-transactions-pattern) to maintain your entities.</span></span>
* <span data-ttu-id="3c099-791">Hämtar en fullständig entitet omfattar minst två lagringstransaktioner: en för att hämta entiteten och en för att hämta blob-data.</span><span class="sxs-lookup"><span data-stu-id="3c099-791">Retrieving a complete entity involves at least two storage transactions: one to retrieve the entity and one to retrieve the blob data.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="3c099-792">När du ska använda det här mönstret</span><span class="sxs-lookup"><span data-stu-id="3c099-792">When to use this pattern</span></span>
<span data-ttu-id="3c099-793">Använd det här mönstret när du behöver lagra enheter vars storlek överskrider gränserna för en enskild entitet i tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-793">Use this pattern when you need to store entities whose size exceeds the limits for an individual entity in the Table service.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="3c099-794">Vägledning och relaterade mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-794">Related patterns and guidance</span></span>
<span data-ttu-id="3c099-795">Följande mönster och guider kan även vara relevanta när du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-795">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="3c099-796">Mönster för överensstämmelse transaktioner</span><span class="sxs-lookup"><span data-stu-id="3c099-796">Eventually consistent transactions pattern</span></span>](#eventually-consistent-transactions-pattern)  
* [<span data-ttu-id="3c099-797">Wide entiteter mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-797">Wide entities pattern</span></span>](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>

### <a name="prependappend-anti-pattern"></a><span data-ttu-id="3c099-798">Lägga/lägga till ett mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-798">Prepend/append anti-pattern</span></span>
<span data-ttu-id="3c099-799">Öka skalbarheten när du har en stor volym med infogningar genom att sprida infogningarna över flera partitioner.</span><span class="sxs-lookup"><span data-stu-id="3c099-799">Increase scalability when you have a high volume of inserts by spreading the inserts across multiple partitions.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="3c099-800">Kontexten och problem</span><span class="sxs-lookup"><span data-stu-id="3c099-800">Context and problem</span></span>
<span data-ttu-id="3c099-801">Tillagt eller lägger till enheter till din lagrade entiteter vanligtvis resulterar i programmet att lägga till nya entiteter till den första eller sista partitionen i en sekvens av partitioner.</span><span class="sxs-lookup"><span data-stu-id="3c099-801">Prepending or appending entities to your stored entities typically results in the application adding new entities to the first or last partition of a sequence of partitions.</span></span> <span data-ttu-id="3c099-802">I så fall måste äger alla infogningar vid något tillfälle rum i samma partition, skapar en hotspot som förhindrar tabelltjänsten från belastningen belastningsutjämning infogningar över flera noder och vilket kan orsaka att träffa skalbarhetsmål för programmet partition.</span><span class="sxs-lookup"><span data-stu-id="3c099-802">In this case, all of the inserts at any given time are taking place in the same partition, creating a hotspot that prevents the table service from load balancing inserts across multiple nodes, and possibly causing your application to hit the scalability targets for partition.</span></span> <span data-ttu-id="3c099-803">Till exempel om du har ett program som loggar nätverks- och åtkomst av anställda, sedan en entitet struktur som visas nedan kan resultera i den aktuella timman partition blir en hotspot om mängden transaktioner når skalbarhet mål för en enskilda partition:</span><span class="sxs-lookup"><span data-stu-id="3c099-803">For example, if you have an application that logs network and resource access by employees, then an entity structure as shown below could result in the current hour's partition becoming a hotspot if the volume of transactions reaches the scalability target for an individual partition:</span></span>  

![][26]

#### <a name="solution"></a><span data-ttu-id="3c099-804">Lösning</span><span class="sxs-lookup"><span data-stu-id="3c099-804">Solution</span></span>
<span data-ttu-id="3c099-805">Följande alternativ entitet struktur förhindrar en hotspot på en viss partition som programmet loggar händelser:</span><span class="sxs-lookup"><span data-stu-id="3c099-805">The following alternative entity structure avoids a hotspot on any particular partition as the application logs events:</span></span>  

![][27]

<span data-ttu-id="3c099-806">Meddelande med det här exemplet hur både den **PartitionKey** och **RowKey** är sammansatta nycklar.</span><span class="sxs-lookup"><span data-stu-id="3c099-806">Notice with this example how both the **PartitionKey** and **RowKey** are compound keys.</span></span> <span data-ttu-id="3c099-807">Den **PartitionKey** använder både avdelning och medarbetare id för att distribuera loggning över flera partitioner.</span><span class="sxs-lookup"><span data-stu-id="3c099-807">The **PartitionKey** uses both the department and employee id to distribute the logging across multiple partitions.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="3c099-808">Problem och överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-808">Issues and considerations</span></span>
<span data-ttu-id="3c099-809">Tänk på följande när du bestämmer hur du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-809">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="3c099-810">Stöder den alternativa nyckelstruktur som undviker att skapa varm partitioner på infogningar effektivt frågorna klientprogrammet gör?</span><span class="sxs-lookup"><span data-stu-id="3c099-810">Does the alternative key structure that avoids creating hot partitions on inserts efficiently support the queries your client application makes?</span></span>  
* <span data-ttu-id="3c099-811">Innebär förväntade volymen av transaktioner som troligen kommer att nå skalbarhetsmål för en enskild partition och begränsas av lagringstjänsten?</span><span class="sxs-lookup"><span data-stu-id="3c099-811">Does your anticipated volume of transactions mean that you are likely to reach the scalability targets for an individual partition and be throttled by the storage service?</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="3c099-812">När du ska använda det här mönstret</span><span class="sxs-lookup"><span data-stu-id="3c099-812">When to use this pattern</span></span>
<span data-ttu-id="3c099-813">Undvik prepend/lägga till ett mönster när volymen av transaktioner kan leda till begränsning av lagringstjänsten när du använder en varm partition.</span><span class="sxs-lookup"><span data-stu-id="3c099-813">Avoid the prepend/append anti-pattern when your volume of transactions is likely to result in throttling by the storage service when you access a hot partition.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="3c099-814">Vägledning och relaterade mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-814">Related patterns and guidance</span></span>
<span data-ttu-id="3c099-815">Följande mönster och guider kan även vara relevanta när du implementerar det här mönstret:</span><span class="sxs-lookup"><span data-stu-id="3c099-815">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="3c099-816">Sammansatt nyckel mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-816">Compound key pattern</span></span>](#compound-key-pattern)  
* [<span data-ttu-id="3c099-817">Loggen pilslut mönster</span><span class="sxs-lookup"><span data-stu-id="3c099-817">Log tail pattern</span></span>](#log-tail-pattern)  
* [<span data-ttu-id="3c099-818">Ändra entiteter</span><span class="sxs-lookup"><span data-stu-id="3c099-818">Modifying entities</span></span>](#modifying-entities)  

### <a name="log-data-anti-pattern"></a><span data-ttu-id="3c099-819">Loggen anti datamönster</span><span class="sxs-lookup"><span data-stu-id="3c099-819">Log data anti-pattern</span></span>
<span data-ttu-id="3c099-820">Normalt ska du använda Blob-tjänsten i stället för tabelltjänsten för att lagra loggdata.</span><span class="sxs-lookup"><span data-stu-id="3c099-820">Typically, you should use the Blob service instead of the Table service to store log data.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="3c099-821">Kontexten och problem</span><span class="sxs-lookup"><span data-stu-id="3c099-821">Context and problem</span></span>
<span data-ttu-id="3c099-822">Ett vanligt användningsfall för loggdata är att hämta en uppsättning loggposter för ett visst datum/tid-intervall: till exempel du vill hitta alla fel och kritiska meddelanden som tillämpningsprogrammet loggade mellan 15:04 och 15:06 på ett visst datum.</span><span class="sxs-lookup"><span data-stu-id="3c099-822">A common use case for log data is to retrieve a selection of log entries for a specific date/time range: for example, you want to find all the error and critical messages that your application logged between 15:04 and 15:06 on a specific date.</span></span> <span data-ttu-id="3c099-823">Du inte vill använda datum och tid för loggmeddelandet är för att avgöra partitionen du sparar logg entiteter till: som resulterar i en varm partition eftersom samtidigt, kommer alla entiteter i loggen delar samma **PartitionKey** värde (finns i avsnittet [Prepend/lägga till ett mönster](#prepend-append-anti-pattern)).</span><span class="sxs-lookup"><span data-stu-id="3c099-823">You do not want to use the date and time of the log message to determine the partition you save log entities to: that results in a hot partition because at any given time, all the log entities will share the same **PartitionKey** value (see the section [Prepend/append anti-pattern](#prepend-append-anti-pattern)).</span></span> <span data-ttu-id="3c099-824">Följande entitet schemat för ett loggmeddelande resulterar i en varm partition eftersom programmet skriver alla loggmeddelanden till partitionen för den aktuella datum och tid:</span><span class="sxs-lookup"><span data-stu-id="3c099-824">For example, the following entity schema for a log message results in a hot partition because the application writes all log messages to the partition for the current date and hour:</span></span>  

![][28]

<span data-ttu-id="3c099-825">I det här exemplet i **RowKey** innehåller datum och tid för loggmeddelande så att loggmeddelanden lagras i tidsvärdet ordning och innehåller ett meddelande-id om flera loggmeddelanden delar samma datum och tid.</span><span class="sxs-lookup"><span data-stu-id="3c099-825">In this example, the **RowKey** includes the date and time of the log message to ensure that log messages are stored sorted in date/time order, and includes a message id in case multiple log messages share the same date and time.</span></span>  

<span data-ttu-id="3c099-826">En annan metod är att använda en **PartitionKey** som säkerställer att programmet skriver meddelanden över ett antal partitioner.</span><span class="sxs-lookup"><span data-stu-id="3c099-826">Another approach is to use a **PartitionKey** that ensures that the application writes messages across a range of partitions.</span></span> <span data-ttu-id="3c099-827">Om källan för loggmeddelandet erbjuder ett sätt att distribuera meddelanden mellan många partitioner, kan du använda följande entitet schema:</span><span class="sxs-lookup"><span data-stu-id="3c099-827">For example, if the source of the log message provides a way to distribute messages across many partitions, you could use the following entity schema:</span></span>  

![][29]

<span data-ttu-id="3c099-828">Problem med det här schemat är dock att om du vill hämta alla loggmeddelanden för ett visst tidsintervall måste du söka varje partition i tabellen.</span><span class="sxs-lookup"><span data-stu-id="3c099-828">However, the problem with this schema is that to retrieve all the log messages for a specific time span you must search every partition in the table.</span></span>

#### <a name="solution"></a><span data-ttu-id="3c099-829">Lösning</span><span class="sxs-lookup"><span data-stu-id="3c099-829">Solution</span></span>
<span data-ttu-id="3c099-830">I föregående avsnitt markerade problemet med försöker använda tabelltjänsten för att lagra loggposter och föreslagna två otillräckliga, Designer.</span><span class="sxs-lookup"><span data-stu-id="3c099-830">The previous section highlighted the problem of trying to use the Table service to store log entries and suggested two, unsatisfactory, designs.</span></span> <span data-ttu-id="3c099-831">En lösning som ledde till en varm partition med risk för prestandaproblem skriva loggmeddelanden; andra lösningen resulterade i dåliga prestanda på grund av kravet att söka igenom varje partition i tabellen för att hämta loggmeddelanden för ett visst tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="3c099-831">One solution led to a hot partition with the risk of poor performance writing log messages; the other solution resulted in poor query performance because of the requirement to scan every partition in the table to retrieve log messages for a specific time span.</span></span> <span data-ttu-id="3c099-832">BLOB storage erbjuder en bättre lösning för den här typen av scenario och detta är hur Azure Storage Analytics lagrar loggdata som samlas in.</span><span class="sxs-lookup"><span data-stu-id="3c099-832">Blob storage offers a better solution for this type of scenario and this is how Azure Storage Analytics stores the log data it collects.</span></span>  

<span data-ttu-id="3c099-833">Det här avsnittet beskrivs hur Storage Analytics lagrar loggdata i blob storage som en illustration av den här metoden för att lagra data som du vanligtvis fråga med intervallet.</span><span class="sxs-lookup"><span data-stu-id="3c099-833">This section outlines how Storage Analytics stores log data in blob storage as an illustration of this approach to storing data that you typically query by range.</span></span>  

<span data-ttu-id="3c099-834">Storage Analytics lagrar meddelanden i en avgränsad format i flera blobbar.</span><span class="sxs-lookup"><span data-stu-id="3c099-834">Storage Analytics stores log messages in a delimited format in multiple blobs.</span></span> <span data-ttu-id="3c099-835">Avgränsad format gör det enkelt för ett klientprogram att tolka data i loggmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="3c099-835">The delimited format makes it easy for a client application to parse the data in the log message.</span></span>  

<span data-ttu-id="3c099-836">Storage Analytics använder en namngivningskonvention för blobbar som gör det möjligt för dig att hitta blob (eller BLOB) som innehåller loggmeddelanden som du söker.</span><span class="sxs-lookup"><span data-stu-id="3c099-836">Storage Analytics uses a naming convention for blobs that enables you to locate the blob (or blobs) that contain the log messages for which you are searching.</span></span> <span data-ttu-id="3c099-837">Till exempel innehåller en blob med namnet ”queue/2014/07/31/1800/000001.log” loggmeddelanden som relaterar till Kötjänsten för timme som börjar vid 18:00 31 juli 2014.</span><span class="sxs-lookup"><span data-stu-id="3c099-837">For example, a blob named "queue/2014/07/31/1800/000001.log" contains log messages that relate to the queue service for the hour starting at 18:00 on 31 July 2014.</span></span> <span data-ttu-id="3c099-838">”000001” anger att detta är den första loggfilen för den här perioden.</span><span class="sxs-lookup"><span data-stu-id="3c099-838">The "000001" indicates that this is the first log file for this period.</span></span> <span data-ttu-id="3c099-839">Tidsstämplar i de första och sista loggmeddelanden som lagras i filen som en del av den blobbens metadata även information om Storage Analytics.</span><span class="sxs-lookup"><span data-stu-id="3c099-839">Storage Analytics also records the timestamps of the first and last log messages stored in the file as part of the blob's metadata.</span></span> <span data-ttu-id="3c099-840">API för blob storage kan du hitta blobbar i en behållare baserat på ett namnprefix: Om du vill hitta alla blobbar som innehåller kön loggdata för timme som börjar vid 18:00, du kan använda prefixet ”kö/2014/07/31/1800”.</span><span class="sxs-lookup"><span data-stu-id="3c099-840">The API for blob storage enables you locate blobs in a container based on a name prefix: to locate all the blobs that contain queue log data for the hour starting at 18:00, you can use the prefix "queue/2014/07/31/1800."</span></span>  

<span data-ttu-id="3c099-841">Storage Analytics buffertar loggmeddelanden internt och regelbundet uppdaterar lämpliga blob eller skapar en ny loggposter senaste batchen.</span><span class="sxs-lookup"><span data-stu-id="3c099-841">Storage Analytics buffers log messages internally and then periodically updates the appropriate blob or creates a new one with the latest batch of log entries.</span></span> <span data-ttu-id="3c099-842">Detta minskar antalet skrivningar som måste utföras på blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-842">This reduces the number of writes it must perform to the blob service.</span></span>  

<span data-ttu-id="3c099-843">Om du implementerar en liknande lösning i ditt eget program, måste du hantera kompromissen mellan tillförlitlighet (skriver varje loggpost till blob-lagring när den uppstår) och kostnad och skalbarhet (buffring uppdateringar i programmet och skrivning dem till blob storage i batchar).</span><span class="sxs-lookup"><span data-stu-id="3c099-843">If you are implementing a similar solution in your own application, you must consider how to manage the trade-off between reliability (writing every log entry to blob storage as it happens) and cost and scalability (buffering updates in your application and writing them to blob storage in batches).</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="3c099-844">Problem och överväganden</span><span class="sxs-lookup"><span data-stu-id="3c099-844">Issues and considerations</span></span>
<span data-ttu-id="3c099-845">Tänk på följande när du bestämmer hur du lagrar loggdata:</span><span class="sxs-lookup"><span data-stu-id="3c099-845">Consider the following points when deciding how to store log data:</span></span>  

* <span data-ttu-id="3c099-846">Om du skapar en tabelldesign som undviker potentiella hot partitioner kan hända att du inte kommer åt din loggdata effektivt.</span><span class="sxs-lookup"><span data-stu-id="3c099-846">If you create a table design that avoids potential hot partitions, you may find that you cannot access your log data efficiently.</span></span>  
* <span data-ttu-id="3c099-847">Om du vill bearbeta loggdata måste ofta en klient att läsa in många poster.</span><span class="sxs-lookup"><span data-stu-id="3c099-847">To process log data, a client often needs to load many records.</span></span>  
* <span data-ttu-id="3c099-848">Även om loggdata är ofta strukturerad, kan blob-lagring vara en bättre lösning.</span><span class="sxs-lookup"><span data-stu-id="3c099-848">Although log data is often structured, blob storage may be a better solution.</span></span>  

### <a name="implementation-considerations"></a><span data-ttu-id="3c099-849">Implementering</span><span class="sxs-lookup"><span data-stu-id="3c099-849">Implementation considerations</span></span>
<span data-ttu-id="3c099-850">Det här avsnittet beskrivs några av övervägandena att ha i åtanke när du implementerar de mönster som beskrivs i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3c099-850">This section discusses some of the considerations to bear in mind when you implement the patterns described in the previous sections.</span></span> <span data-ttu-id="3c099-851">De flesta av det här avsnittet använder exempel skrivna i C# som använder Storage-klientbiblioteket (version 4.3.0 vid tidpunkten för skrivning).</span><span class="sxs-lookup"><span data-stu-id="3c099-851">Most of this section uses examples written in C# that use the Storage Client Library (version 4.3.0 at the time of writing).</span></span>  

### <a name="retrieving-entities"></a><span data-ttu-id="3c099-852">Hämta entiteter</span><span class="sxs-lookup"><span data-stu-id="3c099-852">Retrieving entities</span></span>
<span data-ttu-id="3c099-853">Enligt beskrivningen i avsnittet [Design för frågor](#design-for-querying), de mest effektiva frågan är en punkt-fråga.</span><span class="sxs-lookup"><span data-stu-id="3c099-853">As discussed in the section [Design for querying](#design-for-querying), the most efficient query is a point query.</span></span> <span data-ttu-id="3c099-854">I vissa fall kan du dock behöva hämta flera entiteter.</span><span class="sxs-lookup"><span data-stu-id="3c099-854">However, in some scenarios you may need to retrieve multiple entities.</span></span> <span data-ttu-id="3c099-855">Det här avsnittet beskrivs några vanliga sätt att hämta entiteter med Storage-klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="3c099-855">This section describes some common approaches to retrieving entities using the Storage Client Library.</span></span>  

#### <a name="executing-a-point-query-using-the-storage-client-library"></a><span data-ttu-id="3c099-856">Köra en punkt-fråga med Storage-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="3c099-856">Executing a point query using the Storage Client Library</span></span>
<span data-ttu-id="3c099-857">Det enklaste sättet att köra en fråga om platsen är att använda den **hämta** tabell åtgärden som visas i följande C# kodavsnitt som hämtar en entitet med en **PartitionKey** värdets ”försäljning” och en  **RowKey** värdets ”212”:</span><span class="sxs-lookup"><span data-stu-id="3c099-857">The easiest way to execute a point query is to use the **Retrieve** table operation as shown in the following C# code snippet that retrieves an entity with a **PartitionKey** of value "Sales" and a **RowKey** of value "212":</span></span>  

```csharp
TableOperation retrieveOperation = TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
var retrieveResult = employeeTable.Execute(retrieveOperation);
if (retrieveResult.Result != null)
{
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
}  
```

<span data-ttu-id="3c099-858">Observera hur entiteten förväntar att det här exemplet hämtas för att vara av typen **EmployeeEntity**.</span><span class="sxs-lookup"><span data-stu-id="3c099-858">Notice how this example expects the entity it retrieves to be of type **EmployeeEntity**.</span></span>  

#### <a name="retrieving-multiple-entities-using-linq"></a><span data-ttu-id="3c099-859">Hämtning av flera enheter med hjälp av LINQ</span><span class="sxs-lookup"><span data-stu-id="3c099-859">Retrieving multiple entities using LINQ</span></span>
<span data-ttu-id="3c099-860">Du kan hämta flera entiteter med hjälp av LINQ med Storage-klientbiblioteket och ange en fråga med en **där** satsen.</span><span class="sxs-lookup"><span data-stu-id="3c099-860">You can retrieve multiple entities by using LINQ with Storage Client Library and specifying a query with a **where** clause.</span></span> <span data-ttu-id="3c099-861">För att undvika en tabellgenomsökning, bör du alltid skriva den **PartitionKey** värde i where-satsen, och om möjligt den **RowKey** värdet för att undvika tabell och partition genomsökningar.</span><span class="sxs-lookup"><span data-stu-id="3c099-861">To avoid a table scan, you should always include the **PartitionKey** value in the where clause, and if possible the **RowKey** value to avoid table and partition scans.</span></span> <span data-ttu-id="3c099-862">Tabelltjänsten stöder en begränsad uppsättning jämförelseoperatorer (större än, större än eller lika med, mindre än, mindre än eller lika med, lika och inte lika) ska användas i where-satsen.</span><span class="sxs-lookup"><span data-stu-id="3c099-862">The table service supports a limited set of comparison operators (greater than, greater than or equal, less than, less than or equal, equal, and not equal) to use in the where clause.</span></span> <span data-ttu-id="3c099-863">Följande C# kodfragment hittar alla anställda vars efternamn börjar med ”B” (förutsatt att den **RowKey** lagrar efternamn) på försäljningsavdelningen (förutsatt att den **PartitionKey** lagrar den avdelningsnamn):</span><span class="sxs-lookup"><span data-stu-id="3c099-863">The following C# code snippet finds all the employees whose last name starts with "B" (assuming that the **RowKey** stores the last name) in the sales department (assuming the **PartitionKey** stores the department name):</span></span>  

```csharp
TableQuery<EmployeeEntity> employeeQuery = employeeTable.CreateQuery<EmployeeEntity>();
var query = (from employee in employeeQuery
            where employee.PartitionKey == "Sales" &&
            employee.RowKey.CompareTo("B") >= 0 &&
            employee.RowKey.CompareTo("C") < 0
            select employee).AsTableQuery();
var employees = query.Execute();  
```

<span data-ttu-id="3c099-864">Observera hur frågan anger både en **RowKey** och en **PartitionKey** att säkerställa bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="3c099-864">Notice how the query specifies both a **RowKey** and a **PartitionKey** to ensure better performance.</span></span>  

<span data-ttu-id="3c099-865">Följande kodexempel visar hur motsvarande fungerar med flytande API (Mer information om flytande API: er i allmänhet finns [bästa praxis för att designa en flytande API](http://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):</span><span class="sxs-lookup"><span data-stu-id="3c099-865">The following code sample shows equivalent functionality using the fluent API (for more information about fluent APIs in general, see [Best Practices for Designing a Fluent API](http://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):</span></span>  

```csharp
TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(
    TableQuery.CombineFilters(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition(
    "PartitionKey", QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.GenerateFilterCondition(
    "RowKey", QueryComparisons.GreaterThanOrEqual, "B")
),
TableOperators.And,
TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "C")
    )
);
var employees = employeeTable.ExecuteQuery(employeeQuery);  
```

> [!NOTE]
> <span data-ttu-id="3c099-866">Exemplet omsluter flera **CombineFilters** metoder för att inkludera tre filtervillkor.</span><span class="sxs-lookup"><span data-stu-id="3c099-866">The sample nests multiple **CombineFilters** methods to include the three filter conditions.</span></span>  
> 
> 

#### <a name="retrieving-large-numbers-of-entities-from-a-query"></a><span data-ttu-id="3c099-867">Hämtar stort antal enheter från en fråga</span><span class="sxs-lookup"><span data-stu-id="3c099-867">Retrieving large numbers of entities from a query</span></span>
<span data-ttu-id="3c099-868">En optimal frågan returnerar en enskild entitet baserat på en **PartitionKey** värde och ett **RowKey** värde.</span><span class="sxs-lookup"><span data-stu-id="3c099-868">An optimal query returns an individual entity based on a **PartitionKey** value and a **RowKey** value.</span></span> <span data-ttu-id="3c099-869">Du kan dock ha ett krav för att returnera många entiteter från samma partition eller även många partitioner i vissa scenarier.</span><span class="sxs-lookup"><span data-stu-id="3c099-869">However, in some scenarios you may have a requirement to return many entities from the same partition or even from many partitions.</span></span>  

<span data-ttu-id="3c099-870">Alltid fullständigt bör du testa prestanda för ditt program i dessa scenarier.</span><span class="sxs-lookup"><span data-stu-id="3c099-870">You should always fully test the performance of your application in such scenarios.</span></span>  

<span data-ttu-id="3c099-871">En fråga mot tabelltjänsten kan returnera högst 1 000 enheter på en gång och kan utföra för högst fem sekunder.</span><span class="sxs-lookup"><span data-stu-id="3c099-871">A query against the table service may return a maximum of 1,000 entities at one time and may execute for a maximum of five seconds.</span></span> <span data-ttu-id="3c099-872">Om resultatet innehåller fler än 1 000 enheter om frågan inte slutfördes inom fem sekunder, eller om frågan överskrider gränsen partition, returnerar tabelltjänsten en fortsättningstoken om du vill aktivera klientprogrammet begär en uppsättning enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-872">If the result set contains more than 1,000 entities, if the query did not complete within five seconds, or if the query crosses the partition boundary, the Table service returns a continuation token to enable the client application to request the next set of entities.</span></span> <span data-ttu-id="3c099-873">Mer information om hur fortsättning tokens arbete finns [Timeout för fråga och sidbrytning](http://msdn.microsoft.com/library/azure/dd135718.aspx).</span><span class="sxs-lookup"><span data-stu-id="3c099-873">For more information about how continuation tokens work, see [Query Timeout and Pagination](http://msdn.microsoft.com/library/azure/dd135718.aspx).</span></span>  

<span data-ttu-id="3c099-874">Om du använder Storage-klientbiblioteket hantera den automatiskt fortsättning token för dig som returnerar entiteter från tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-874">If you are using the Storage Client Library, it can automatically handle continuation tokens for you as it returns entities from the Table service.</span></span> <span data-ttu-id="3c099-875">Följande C# kodexempel använder Storage-klientbiblioteket automatiskt hanterar fortsättning token om tabelltjänsten returnerar dem i ett svar:</span><span class="sxs-lookup"><span data-stu-id="3c099-875">The following C# code sample using the Storage Client Library automatically handles continuation tokens if the table service returns them in a response:</span></span>  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

var employees = employeeTable.ExecuteQuery(employeeQuery);
foreach (var emp in employees)
{
        ...
}  
```

<span data-ttu-id="3c099-876">Följande C#-kod hanterar uttryckligen fortsättning token:</span><span class="sxs-lookup"><span data-stu-id="3c099-876">The following C# code handles continuation tokens explicitly:</span></span>  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

TableContinuationToken continuationToken = null;

do
{
        var employees = employeeTable.ExecuteQuerySegmented(
        employeeQuery, continuationToken);
    foreach (var emp in employees)
    {
    ...
    }
    continuationToken = employees.ContinuationToken;
} while (continuationToken != null);  
```

<span data-ttu-id="3c099-877">Du kan styra när programmet hämtar nästa segment av data med hjälp av fortsättning token explicit.</span><span class="sxs-lookup"><span data-stu-id="3c099-877">By using continuation tokens explicitly, you can control when your application retrieves the next segment of data.</span></span> <span data-ttu-id="3c099-878">Till exempel om ditt klientprogram kan du bläddra igenom de entiteter som lagras i en tabell, en användare behöver inte bläddra igenom alla entiteter som hämtas av frågan, så att programmet bara använda en fortsättningstoken för att hämta nästa segment när användaren hade slutförts bläddring genom alla entiteter i det aktuella segmentet.</span><span class="sxs-lookup"><span data-stu-id="3c099-878">For example, if your client application enables users to page through the entities stored in a table, a user may decide not to page through all the entities retrieved by the query so your application would only use a continuation token to retrieve the next segment when the user had finished paging through all the entities in the current segment.</span></span> <span data-ttu-id="3c099-879">Den här metoden har flera fördelar:</span><span class="sxs-lookup"><span data-stu-id="3c099-879">This approach has several benefits:</span></span>  

* <span data-ttu-id="3c099-880">Du kan begränsa mängden data som ska hämtas från tabelltjänsten och att du flyttar över nätverket.</span><span class="sxs-lookup"><span data-stu-id="3c099-880">It enables you to limit the amount of data to retrieve from the Table service and that you move over the network.</span></span>  
* <span data-ttu-id="3c099-881">Det kan du utföra asynkrona i/o i .NET.</span><span class="sxs-lookup"><span data-stu-id="3c099-881">It enables you to perform asynchronous IO in .NET.</span></span>  
* <span data-ttu-id="3c099-882">Det gör att du kan serialisera fortsättningstoken till permanent lagringsutrymme så att du kan fortsätta om ett program kraschar.</span><span class="sxs-lookup"><span data-stu-id="3c099-882">It enables you to serialize the continuation token to persistent storage so you can continue in the event of an application crash.</span></span>  

> [!NOTE]
> <span data-ttu-id="3c099-883">En fortsättningstoken returnerar vanligtvis ett segment som innehåller 1 000 enheter, även om det kan vara färre.</span><span class="sxs-lookup"><span data-stu-id="3c099-883">A continuation token typically returns a segment containing 1,000 entities, although it may be fewer.</span></span> <span data-ttu-id="3c099-884">Detta gäller även om du begränsar antalet poster som en fråga som returnerar med hjälp av **ta** att returnera de första n entiteter som matchar sökvillkoren sökning: tabelltjänsten kan returnera ett segment som innehåller färre än n entiteter tillsammans med en fortsättningstoken så att du kan hämta de återstående enheterna.</span><span class="sxs-lookup"><span data-stu-id="3c099-884">This is also the case if you limit the number of entries a query returns by using **Take** to return the first n entities that match your lookup criteria: the table service may return a segment containing fewer than n entities along with a continuation token to enable you to retrieve the remaining entities.</span></span>  
> 
> 

<span data-ttu-id="3c099-885">Följande C#-kod visar hur du ändrar antal entiteter som returneras i ett segment:</span><span class="sxs-lookup"><span data-stu-id="3c099-885">The following C# code shows how to modify the number of entities returned inside a segment:</span></span>  

```csharp
employeeQuery.TakeCount = 50;  
```

#### <a name="server-side-projection"></a><span data-ttu-id="3c099-886">Projektion av serversidan</span><span class="sxs-lookup"><span data-stu-id="3c099-886">Server-side projection</span></span>
<span data-ttu-id="3c099-887">En enda entitet kan ha upp till 255 egenskaper och upp till 1 MB i storlek.</span><span class="sxs-lookup"><span data-stu-id="3c099-887">A single entity can have up to 255 properties and be up to 1 MB in size.</span></span> <span data-ttu-id="3c099-888">När du frågar tabellen och hämta entiteter kanske inte behöver alla egenskaper och kan undvika att överföra data i onödan (om du vill minska svarstiden och kostnaden).</span><span class="sxs-lookup"><span data-stu-id="3c099-888">When you query the table and retrieve entities, you may not need all the properties and can avoid transferring data unnecessarily (to help reduce latency and cost).</span></span> <span data-ttu-id="3c099-889">Du kan använda serversidan projektion Överför bara de egenskaper som du behöver.</span><span class="sxs-lookup"><span data-stu-id="3c099-889">You can use server-side projection to transfer just the properties you need.</span></span> <span data-ttu-id="3c099-890">Följande exempel är hämtar bara den **e-post** egenskapen (tillsammans med **PartitionKey**, **RowKey**, **tidsstämpel**, och **ETag**) från de enheter som väljs av frågan.</span><span class="sxs-lookup"><span data-stu-id="3c099-890">The following example is retrieves just the **Email** property (along with **PartitionKey**, **RowKey**, **Timestamp**, and **ETag**) from the entities selected by the query.</span></span>  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
List<string> columns = new List<string>() { "Email" };
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter).Select(columns);

var entities = employeeTable.ExecuteQuery(employeeQuery);
foreach (var e in entities)
{
        Console.WriteLine("RowKey: {0}, EmployeeEmail: {1}", e.RowKey, e.Email);
}  
```

<span data-ttu-id="3c099-891">Observera hur **RowKey** värdet är tillgängligt även om den inte ingår i listan över egenskaper som hämtas.</span><span class="sxs-lookup"><span data-stu-id="3c099-891">Notice how the **RowKey** value is available even though it was not included in the list of properties to retrieve.</span></span>  

### <a name="modifying-entities"></a><span data-ttu-id="3c099-892">Ändra entiteter</span><span class="sxs-lookup"><span data-stu-id="3c099-892">Modifying entities</span></span>
<span data-ttu-id="3c099-893">Storage-klientbiblioteket kan du ändra din entiteter som lagras i tabelltjänsten genom att lägga till, ta bort och uppdatera entiteter.</span><span class="sxs-lookup"><span data-stu-id="3c099-893">The Storage Client Library enables you to modify your entities stored in the table service by inserting, deleting, and updating entities.</span></span> <span data-ttu-id="3c099-894">Du kan använda EGTs batch flera insert-, update- och delete-åtgärder tillsammans för att minska antalet sändningar som krävs och förbättra prestandan för din lösning.</span><span class="sxs-lookup"><span data-stu-id="3c099-894">You can use EGTs to batch multiple insert, update, and delete operations together to reduce the number of round trips required and improve the performance of your solution.</span></span>  

<span data-ttu-id="3c099-895">Observera att undantag när Storage-klientbiblioteket utför en EGT vanligtvis innehåller index för den entitet som orsakade batch misslyckas.</span><span class="sxs-lookup"><span data-stu-id="3c099-895">Note that exceptions thrown when the Storage Client Library executes an EGT typically include the index of the entity that caused the batch to fail.</span></span> <span data-ttu-id="3c099-896">Detta är användbart när du felsöker kod som använder EGTs.</span><span class="sxs-lookup"><span data-stu-id="3c099-896">This is helpful when you are debugging code that uses EGTs.</span></span>  

<span data-ttu-id="3c099-897">Du bör också övervägas hur din design påverkar hur ditt klientprogram hanterar samtidighet och update-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3c099-897">You should also consider how your design affects how your client application handles concurrency and update operations.</span></span>  

#### <a name="managing-concurrency"></a><span data-ttu-id="3c099-898">Hantera samtidighet</span><span class="sxs-lookup"><span data-stu-id="3c099-898">Managing concurrency</span></span>
<span data-ttu-id="3c099-899">Som standard tabelltjänsten implementerar Optimistisk samtidighet kontrollerar på nivån för enskilda entiteter för **infoga**, **sammanfoga**, och **ta bort** åtgärder, även om den är möjligt för en klient att tvinga tabelltjänsten att kringgå de här kontrollerna.</span><span class="sxs-lookup"><span data-stu-id="3c099-899">By default, the table service implements optimistic concurrency checks at the level of individual entities for **Insert**, **Merge**, and **Delete** operations, although it is possible for a client to force the table service to bypass these checks.</span></span> <span data-ttu-id="3c099-900">Mer information om hur tabelltjänsten hanterar samtidighet finns [hantera samtidighet i Microsoft Azure Storage](../storage/common/storage-concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="3c099-900">For more information about how the table service manages concurrency, see  [Managing Concurrency in Microsoft Azure Storage](../storage/common/storage-concurrency.md).</span></span>  

#### <a name="merge-or-replace"></a><span data-ttu-id="3c099-901">Merge eller ersätta</span><span class="sxs-lookup"><span data-stu-id="3c099-901">Merge or replace</span></span>
<span data-ttu-id="3c099-902">Den **ersätta** metod för den **TableOperation** klassen ersätter alltid fullständiga entiteten i tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-902">The **Replace** method of the **TableOperation** class always replaces the complete entity in the Table service.</span></span> <span data-ttu-id="3c099-903">Om du inte använder en egenskap i begäran när egenskapen finns i entiteten lagrade bort begäran egenskapen från den lagrade entiteten.</span><span class="sxs-lookup"><span data-stu-id="3c099-903">If you do not include a property in the request when that property exists in the stored entity, the request removes that property from the stored entity.</span></span> <span data-ttu-id="3c099-904">Om du inte vill ta bort en egenskap från en lagrad entitet explicit, måste du inkludera varje egenskap i begäran.</span><span class="sxs-lookup"><span data-stu-id="3c099-904">Unless you want to remove a property explicitly from a stored entity, you must include every property in the request.</span></span>  

<span data-ttu-id="3c099-905">Du kan använda den **sammanfoga** metod för den **TableOperation** klassen för att minska mängden data som du skickar till tabelltjänsten när du vill uppdatera en entitet.</span><span class="sxs-lookup"><span data-stu-id="3c099-905">You can use the **Merge** method of the **TableOperation** class to reduce the amount of data that you send to the Table service when you want to update an entity.</span></span> <span data-ttu-id="3c099-906">Den **sammanfoga** metoden ersätter alla egenskaper i entiteten lagrade med egenskapsvärden från det entitet som ingår i denna begäran, men lämnar kvar alla egenskaper i entiteten lagrade som inte ingår i begäran.</span><span class="sxs-lookup"><span data-stu-id="3c099-906">The **Merge** method replaces any properties in the stored entity with property values from the entity included in the request, but leaves intact any properties in the stored entity that are not included in the request.</span></span> <span data-ttu-id="3c099-907">Detta är användbart om du har stora entiteter och behöver bara uppdatera ett litet antal egenskaper i en begäran.</span><span class="sxs-lookup"><span data-stu-id="3c099-907">This is useful if you have large entities and only need to update a small number of properties in a request.</span></span>  

> [!NOTE]
> <span data-ttu-id="3c099-908">Den **ersätta** och **sammanfoga** metoder misslyckas om entiteten inte finns.</span><span class="sxs-lookup"><span data-stu-id="3c099-908">The **Replace** and **Merge** methods fail if the entity does not exist.</span></span> <span data-ttu-id="3c099-909">Alternativt kan du använda den **InsertOrReplace** och **InsertOrMerge** metoder som skapar en ny entitet om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="3c099-909">As an alternative, you can use the **InsertOrReplace** and **InsertOrMerge** methods that create a new entity if it doesn't exist.</span></span>  
> 
> 

### <a name="working-with-heterogeneous-entity-types"></a><span data-ttu-id="3c099-910">Arbeta med heterogena entitetstyper</span><span class="sxs-lookup"><span data-stu-id="3c099-910">Working with heterogeneous entity types</span></span>
<span data-ttu-id="3c099-911">Tabelltjänsten är en *schemat mindre* tabell butik som innebär att en enskild tabell kan lagra entiteter av flera typer som ger stor flexibilitet i din design.</span><span class="sxs-lookup"><span data-stu-id="3c099-911">The Table service is a *schema-less* table store that means that a single table can store entities of multiple types providing great flexibility in your design.</span></span> <span data-ttu-id="3c099-912">I följande exempel visas en tabell som lagrar både anställda och avdelningen enheter:</span><span class="sxs-lookup"><span data-stu-id="3c099-912">The following example illustrates a table storing both employee and department entities:</span></span>  

<table>
<tr>
<th><span data-ttu-id="3c099-913">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="3c099-913">PartitionKey</span></span></th>
<th><span data-ttu-id="3c099-914">RowKey</span><span class="sxs-lookup"><span data-stu-id="3c099-914">RowKey</span></span></th>
<th><span data-ttu-id="3c099-915">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="3c099-915">Timestamp</span></span></th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="3c099-916">Förnamn</span><span class="sxs-lookup"><span data-stu-id="3c099-916">FirstName</span></span></th>
<th><span data-ttu-id="3c099-917">Efternamn</span><span class="sxs-lookup"><span data-stu-id="3c099-917">LastName</span></span></th>
<th><span data-ttu-id="3c099-918">Ålder</span><span class="sxs-lookup"><span data-stu-id="3c099-918">Age</span></span></th>
<th><span data-ttu-id="3c099-919">E-post</span><span class="sxs-lookup"><span data-stu-id="3c099-919">Email</span></span></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="3c099-920">Förnamn</span><span class="sxs-lookup"><span data-stu-id="3c099-920">FirstName</span></span></th>
<th><span data-ttu-id="3c099-921">Efternamn</span><span class="sxs-lookup"><span data-stu-id="3c099-921">LastName</span></span></th>
<th><span data-ttu-id="3c099-922">Ålder</span><span class="sxs-lookup"><span data-stu-id="3c099-922">Age</span></span></th>
<th><span data-ttu-id="3c099-923">E-post</span><span class="sxs-lookup"><span data-stu-id="3c099-923">Email</span></span></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="3c099-924">DepartmentName</span><span class="sxs-lookup"><span data-stu-id="3c099-924">DepartmentName</span></span></th>
<th><span data-ttu-id="3c099-925">EmployeeCount</span><span class="sxs-lookup"><span data-stu-id="3c099-925">EmployeeCount</span></span></th>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="3c099-926">Förnamn</span><span class="sxs-lookup"><span data-stu-id="3c099-926">FirstName</span></span></th>
<th><span data-ttu-id="3c099-927">Efternamn</span><span class="sxs-lookup"><span data-stu-id="3c099-927">LastName</span></span></th>
<th><span data-ttu-id="3c099-928">Ålder</span><span class="sxs-lookup"><span data-stu-id="3c099-928">Age</span></span></th>
<th><span data-ttu-id="3c099-929">E-post</span><span class="sxs-lookup"><span data-stu-id="3c099-929">Email</span></span></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

<span data-ttu-id="3c099-930">Observera att varje entitet måste dock ha **PartitionKey**, **RowKey**, och **tidsstämpel** värden, men kan ha en uppsättning egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3c099-930">Note that each entity must still have **PartitionKey**, **RowKey**, and **Timestamp** values, but may have any set of properties.</span></span> <span data-ttu-id="3c099-931">Det finns inget att ange vilken typ av en enhet om du väljer att lagra informationen någonstans.</span><span class="sxs-lookup"><span data-stu-id="3c099-931">Furthermore, there is nothing to indicate the type of an entity unless you choose to store that information somewhere.</span></span> <span data-ttu-id="3c099-932">Det finns två alternativ för att identifiera enhetstyp:</span><span class="sxs-lookup"><span data-stu-id="3c099-932">There are two options for identifying the entity type:</span></span>  

* <span data-ttu-id="3c099-933">Lägga till typen i **RowKey** (eller eventuellt den **PartitionKey**).</span><span class="sxs-lookup"><span data-stu-id="3c099-933">Prepend the entity type to the **RowKey** (or possibly the **PartitionKey**).</span></span> <span data-ttu-id="3c099-934">Till exempel **EMPLOYEE_000123** eller **DEPARTMENT_SALES** som **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-934">For example, **EMPLOYEE_000123** or **DEPARTMENT_SALES** as **RowKey** values.</span></span>  
* <span data-ttu-id="3c099-935">Använda en separat egenskap för att registrera entitetstypen som visas i tabellen nedan.</span><span class="sxs-lookup"><span data-stu-id="3c099-935">Use a separate property to record the entity type as shown in the table below.</span></span>  

<table>
<tr>
<th><span data-ttu-id="3c099-936">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="3c099-936">PartitionKey</span></span></th>
<th><span data-ttu-id="3c099-937">RowKey</span><span class="sxs-lookup"><span data-stu-id="3c099-937">RowKey</span></span></th>
<th><span data-ttu-id="3c099-938">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="3c099-938">Timestamp</span></span></th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="3c099-939">EntityType</span><span class="sxs-lookup"><span data-stu-id="3c099-939">EntityType</span></span></th>
<th><span data-ttu-id="3c099-940">Förnamn</span><span class="sxs-lookup"><span data-stu-id="3c099-940">FirstName</span></span></th>
<th><span data-ttu-id="3c099-941">Efternamn</span><span class="sxs-lookup"><span data-stu-id="3c099-941">LastName</span></span></th>
<th><span data-ttu-id="3c099-942">Ålder</span><span class="sxs-lookup"><span data-stu-id="3c099-942">Age</span></span></th>
<th><span data-ttu-id="3c099-943">E-post</span><span class="sxs-lookup"><span data-stu-id="3c099-943">Email</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="3c099-944">Medarbetare</span><span class="sxs-lookup"><span data-stu-id="3c099-944">Employee</span></span></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="3c099-945">EntityType</span><span class="sxs-lookup"><span data-stu-id="3c099-945">EntityType</span></span></th>
<th><span data-ttu-id="3c099-946">Förnamn</span><span class="sxs-lookup"><span data-stu-id="3c099-946">FirstName</span></span></th>
<th><span data-ttu-id="3c099-947">Efternamn</span><span class="sxs-lookup"><span data-stu-id="3c099-947">LastName</span></span></th>
<th><span data-ttu-id="3c099-948">Ålder</span><span class="sxs-lookup"><span data-stu-id="3c099-948">Age</span></span></th>
<th><span data-ttu-id="3c099-949">E-post</span><span class="sxs-lookup"><span data-stu-id="3c099-949">Email</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="3c099-950">Medarbetare</span><span class="sxs-lookup"><span data-stu-id="3c099-950">Employee</span></span></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="3c099-951">EntityType</span><span class="sxs-lookup"><span data-stu-id="3c099-951">EntityType</span></span></th>
<th><span data-ttu-id="3c099-952">DepartmentName</span><span class="sxs-lookup"><span data-stu-id="3c099-952">DepartmentName</span></span></th>
<th><span data-ttu-id="3c099-953">EmployeeCount</span><span class="sxs-lookup"><span data-stu-id="3c099-953">EmployeeCount</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="3c099-954">Avdelning</span><span class="sxs-lookup"><span data-stu-id="3c099-954">Department</span></span></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="3c099-955">EntityType</span><span class="sxs-lookup"><span data-stu-id="3c099-955">EntityType</span></span></th>
<th><span data-ttu-id="3c099-956">Förnamn</span><span class="sxs-lookup"><span data-stu-id="3c099-956">FirstName</span></span></th>
<th><span data-ttu-id="3c099-957">Efternamn</span><span class="sxs-lookup"><span data-stu-id="3c099-957">LastName</span></span></th>
<th><span data-ttu-id="3c099-958">Ålder</span><span class="sxs-lookup"><span data-stu-id="3c099-958">Age</span></span></th>
<th><span data-ttu-id="3c099-959">E-post</span><span class="sxs-lookup"><span data-stu-id="3c099-959">Email</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="3c099-960">Medarbetare</span><span class="sxs-lookup"><span data-stu-id="3c099-960">Employee</span></span></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

<span data-ttu-id="3c099-961">Det första alternativet, prepending entiteten typ till den **RowKey**, är användbart om det finns en risk att två entiteter med olika typer kan ha samma nyckelvärde.</span><span class="sxs-lookup"><span data-stu-id="3c099-961">The first option, prepending the entity type to the **RowKey**, is useful if there is a possibility that two entities of different types might have the same key value.</span></span> <span data-ttu-id="3c099-962">Dessutom grupperas entiteter av samma typ samman i partitionen.</span><span class="sxs-lookup"><span data-stu-id="3c099-962">It also groups entities of the same type together in the partition.</span></span>  

<span data-ttu-id="3c099-963">De metoder som beskrivs i det här avsnittet är särskilt relevanta till diskussionen [arvsrelationer](#inheritance-relationships) i den här guiden i avsnittet [modellering relationer](#modelling-relationships).</span><span class="sxs-lookup"><span data-stu-id="3c099-963">The techniques discussed in this section are especially relevant to the discussion [Inheritance relationships](#inheritance-relationships) earlier in this guide in the section [Modelling relationships](#modelling-relationships).</span></span>  

> [!NOTE]
> <span data-ttu-id="3c099-964">Du bör överväga att använda ett versionsnummer i entiteten TYPVÄRDE att klientprogram att utvecklas POCO objekt och arbeta med olika versioner.</span><span class="sxs-lookup"><span data-stu-id="3c099-964">You should consider including a version number in the entity type value to enable client applications to evolve POCO objects and work with different versions.</span></span>  
> 
> 

<span data-ttu-id="3c099-965">Resten av det här avsnittet beskrivs några av de funktioner i Storage-klientbiblioteket som underlättar arbetet med flera typer av enheter i samma tabell.</span><span class="sxs-lookup"><span data-stu-id="3c099-965">The remainder of this section describes some of the features in the Storage Client Library that facilitate working with multiple entity types in the same table.</span></span>  

#### <a name="retrieving-heterogeneous-entity-types"></a><span data-ttu-id="3c099-966">Hämta heterogena entitetstyper</span><span class="sxs-lookup"><span data-stu-id="3c099-966">Retrieving heterogeneous entity types</span></span>
<span data-ttu-id="3c099-967">Om du använder Storage-klientbiblioteket finns det tre alternativ för att arbeta med flera typer av enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-967">If you are using the Storage Client Library, you have three options for working with multiple entity types.</span></span>  

<span data-ttu-id="3c099-968">Om du vet vilken typ av enhet som lagras med ett visst **RowKey** och **PartitionKey** värden, sedan kan du ange entitetstypen när du hämta entiteten som visas i de föregående två exemplen som Hämta entiteter av typen **EmployeeEntity**: [körs en punkt-fråga med Storage-klientbiblioteket](#executing-a-point-query-using-the-storage-client-library) och [hämtning av flera enheter med hjälp av LINQ](#retrieving-multiple-entities-using-linq).</span><span class="sxs-lookup"><span data-stu-id="3c099-968">If you know the type of the entity stored with a specific **RowKey** and **PartitionKey** values, then you can specify the entity type when you retrieve the entity as shown in the previous two examples that retrieve entities of type **EmployeeEntity**: [Executing a point query using the Storage Client Library](#executing-a-point-query-using-the-storage-client-library) and [Retrieving multiple entities using LINQ](#retrieving-multiple-entities-using-linq).</span></span>  

<span data-ttu-id="3c099-969">Det andra alternativet är att använda den **DynamicTableEntity** typ (en egenskapsuppsättning) i stället för en konkret POCO entitetstyp (det här alternativet kan också förbättra prestanda eftersom det finns inget behov av att serialisera och deserialisera entiteten till .NET-typer).</span><span class="sxs-lookup"><span data-stu-id="3c099-969">The second option is to use the **DynamicTableEntity** type (a property bag) instead of a concrete POCO entity type (this option may also improve performance because there is no need to serialize and deserialize the entity to .NET types).</span></span> <span data-ttu-id="3c099-970">Följande C#-kod potentiellt hämtar flera entiteter med olika typer från tabellen, men returnerar alla entiteter som **DynamicTableEntity** instanser.</span><span class="sxs-lookup"><span data-stu-id="3c099-970">The following C# code potentially retrieves multiple entities of different types from the table, but returns all entities as **DynamicTableEntity** instances.</span></span> <span data-ttu-id="3c099-971">Därefter använder den **EntityType** egenskapen fastställa typen av varje entitet:</span><span class="sxs-lookup"><span data-stu-id="3c099-971">It then uses the **EntityType** property to determine the type of each entity:</span></span>  

```csharp
string filter = TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("PartitionKey",
    QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("RowKey",
                    QueryComparisons.GreaterThanOrEqual, "B"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey",
        QueryComparisons.LessThan, "F")
    )
);
TableQuery<DynamicTableEntity> entityQuery =
    new TableQuery<DynamicTableEntity>().Where(filter);
var employees = employeeTable.ExecuteQuery(entityQuery);

IEnumerable<DynamicTableEntity> entities = employeeTable.ExecuteQuery(entityQuery);
foreach (var e in entities)
{
EntityProperty entityTypeProperty;
if (e.Properties.TryGetValue("EntityType", out entityTypeProperty))
{
    if (entityTypeProperty.StringValue == "Employee")
    {
        // Use entityTypeProperty, RowKey, PartitionKey, Etag, and Timestamp
        }
    }
}  
```

<span data-ttu-id="3c099-972">Observera att om du vill hämta andra egenskaper måste du använda den **TryGetValue** -metoden i den **egenskaper** -egenskapen för den **DynamicTableEntity** klass.</span><span class="sxs-lookup"><span data-stu-id="3c099-972">Note that to retrieve other properties you must use the **TryGetValue** method on the **Properties** property of the **DynamicTableEntity** class.</span></span>  

<span data-ttu-id="3c099-973">Ett tredje alternativ är att kombinera med hjälp av den **DynamicTableEntity** typ och en **EntityResolver** instans.</span><span class="sxs-lookup"><span data-stu-id="3c099-973">A third option is to combine using the **DynamicTableEntity** type and an **EntityResolver** instance.</span></span> <span data-ttu-id="3c099-974">På så sätt kan du matcha flera POCO-typer i samma fråga.</span><span class="sxs-lookup"><span data-stu-id="3c099-974">This enables you to resolve to multiple POCO types in the same query.</span></span> <span data-ttu-id="3c099-975">I det här exemplet i **EntityResolver** ombud använder den **EntityType** egenskapen att skilja mellan de två typerna av entitet som frågan returnerar.</span><span class="sxs-lookup"><span data-stu-id="3c099-975">In this example, the **EntityResolver** delegate is using the **EntityType** property to distinguish between the two types of entity that the query returns.</span></span> <span data-ttu-id="3c099-976">Den **lösa** metoden använder den **matcharen** ombud för att lösa **DynamicTableEntity** instanser till **TableEntity** instanser.</span><span class="sxs-lookup"><span data-stu-id="3c099-976">The **Resolve** method uses the **resolver** delegate to resolve **DynamicTableEntity** instances to **TableEntity** instances.</span></span>  

```csharp
EntityResolver<TableEntity> resolver = (pk, rk, ts, props, etag) =>
{

        TableEntity resolvedEntity = null;
        if (props["EntityType"].StringValue == "Department")
        {
        resolvedEntity = new DepartmentEntity();
        }
        else if (props["EntityType"].StringValue == "Employee")
        {
        resolvedEntity = new EmployeeEntity();
        }
        else throw new ArgumentException("Unrecognized entity", "props");

        resolvedEntity.PartitionKey = pk;
        resolvedEntity.RowKey = rk;
        resolvedEntity.Timestamp = ts;
        resolvedEntity.ETag = etag;
        resolvedEntity.ReadEntity(props, null);
        return resolvedEntity;
};

string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<DynamicTableEntity> entityQuery =
        new TableQuery<DynamicTableEntity>().Where(filter);

var entities = employeeTable.ExecuteQuery(entityQuery, resolver);
foreach (var e in entities)
{
        if (e is DepartmentEntity)
        {
    ...
        }
        if (e is EmployeeEntity)
        {
    ...
        }
}  
```

#### <a name="modifying-heterogeneous-entity-types"></a><span data-ttu-id="3c099-977">Ändra heterogena entitetstyper</span><span class="sxs-lookup"><span data-stu-id="3c099-977">Modifying heterogeneous entity types</span></span>
<span data-ttu-id="3c099-978">Du behöver inte vet vilken typ av en entitet att ta bort den och du vet alltid vilken typ av en enhet när du infogar den.</span><span class="sxs-lookup"><span data-stu-id="3c099-978">You do not need to know the type of an entity to delete it, and you always know the type of an entity when you insert it.</span></span> <span data-ttu-id="3c099-979">Du kan dock använda **DynamicTableEntity** typen för att uppdatera en entitet utan att känna till dess typ och utan att använda en POCO-entitetsklass.</span><span class="sxs-lookup"><span data-stu-id="3c099-979">However, you can use **DynamicTableEntity** type to update an entity without knowing its type and without using a POCO entity class.</span></span> <span data-ttu-id="3c099-980">Följande kodexempel hämtas en enda entitet och kontrollerar de **EmployeeCount** egenskapen finns innan den uppdateras.</span><span class="sxs-lookup"><span data-stu-id="3c099-980">The following code sample retrieves a single entity, and checks the **EmployeeCount** property exists before updating it.</span></span>  

```csharp
TableResult result =
        employeeTable.Execute(TableOperation.Retrieve(partitionKey, rowKey));
DynamicTableEntity department = (DynamicTableEntity)result.Result;

EntityProperty countProperty;

if (!department.Properties.TryGetValue("EmployeeCount", out countProperty))
{
        throw new
        InvalidOperationException("Invalid entity, EmployeeCount property not found.");
}
countProperty.Int32Value += 1;
employeeTable.Execute(TableOperation.Merge(department));  
```

### <a name="controlling-access-with-shared-access-signatures"></a><span data-ttu-id="3c099-981">Kontrollera åtkomst med signaturer för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="3c099-981">Controlling access with Shared Access Signatures</span></span>
<span data-ttu-id="3c099-982">Du kan använda delade signatur åtkomst (SAS)-tokens för att aktivera klientprogram att ändra (och fråga) tabellentiteter direkt utan att behöva autentisera direkt med tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-982">You can use Shared Access Signature (SAS) tokens to enable client applications to modify (and query) table entities directly without the need to authenticate directly with the table service.</span></span> <span data-ttu-id="3c099-983">Det finns vanligtvis tre huvudsakliga fördelar med SAS i ditt program:</span><span class="sxs-lookup"><span data-stu-id="3c099-983">Typically, there are three main benefits to using SAS in your application:</span></span>  

* <span data-ttu-id="3c099-984">Du behöver inte distribuera din lagringskontonyckel till en osäker plattform (till exempel en mobil enhet) för att tillåta enheten för att komma åt och ändra entiteter i tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-984">You do not need to distribute your storage account key to an insecure platform (such as a mobile device) in order to allow that device to access and modify entities in the Table service.</span></span>  
* <span data-ttu-id="3c099-985">Du kan avlasta del av arbetet som webb-och arbetsroller utför när du ska hantera dina enheter till klientenheter, till exempel slutanvändarnas datorer och mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-985">You can offload some of the work that web and worker roles perform in managing your entities to client devices such as end-user computers and mobile devices.</span></span>  
* <span data-ttu-id="3c099-986">Du kan tilldela en begränsad och tid begränsad uppsättning behörigheter till en klient (till exempel tillåta att skrivskyddad åtkomst till specifika resurser).</span><span class="sxs-lookup"><span data-stu-id="3c099-986">You can assign a constrained and time limited set of permissions to a client (such as allowing read-only access to specific resources).</span></span>  

<span data-ttu-id="3c099-987">Läs mer om hur du använder SAS-token med tabelltjänsten [med delad åtkomst signaturer (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="3c099-987">For more information about using SAS tokens with the Table service, see [Using Shared Access Signatures (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span></span>  

<span data-ttu-id="3c099-988">Du måste dock fortfarande skapa SAS-token som ger ett klientprogram för entiteter i tabelltjänsten: du bör göra detta i en miljö som har säker åtkomst till dina nycklar för lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3c099-988">However, you must still generate the SAS tokens that grant a client application to the entities in the table service: you should do this in an environment that has secure access to your storage account keys.</span></span> <span data-ttu-id="3c099-989">Normalt använder du en webb- eller arbetarroll roll för att generera SAS-token och skicka dem till klientprogram som behöver åtkomst till dina enheter.</span><span class="sxs-lookup"><span data-stu-id="3c099-989">Typically, you use a web or worker role to generate the SAS tokens and deliver them to the client applications that need access to your entities.</span></span> <span data-ttu-id="3c099-990">Eftersom det finns fortfarande en arbetet med genererar och leverera SAS-token till klienter, bör du hur du bäst för att minska den här kostnader, särskilt i stora mängder.</span><span class="sxs-lookup"><span data-stu-id="3c099-990">Because there is still an overhead involved in generating and delivering SAS tokens to clients, you should consider how best to reduce this overhead, especially in high-volume scenarios.</span></span>  

<span data-ttu-id="3c099-991">Det är möjligt att generera en SAS-token som ger åtkomst till en delmängd av entiteter i en tabell.</span><span class="sxs-lookup"><span data-stu-id="3c099-991">It is possible to generate a SAS token that grants access to a subset of the entities in a table.</span></span> <span data-ttu-id="3c099-992">Som standard kan du skapa en SAS-token för en hel tabell, men det är också möjligt att ange att SAS-token som ger åtkomst till antingen en mängd **PartitionKey** värden eller en uppsättning **PartitionKey** och **RowKey** värden.</span><span class="sxs-lookup"><span data-stu-id="3c099-992">By default, you create a SAS token for an entire table, but it is also possible to specify that the SAS token grant access to either a range of **PartitionKey** values, or a range of **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="3c099-993">Du kan välja att generera SAS-token för enskilda användare av systemet så att varje användares SAS-token endast ger dem åtkomst till sina egna enheter i tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="3c099-993">You might choose to generate SAS tokens for individual users of your system such that each user's SAS token only allows them access to their own entities in the table service.</span></span>  

### <a name="asynchronous-and-parallel-operations"></a><span data-ttu-id="3c099-994">Asynkron och parallella åtgärder</span><span class="sxs-lookup"><span data-stu-id="3c099-994">Asynchronous and parallel operations</span></span>
<span data-ttu-id="3c099-995">Om dina begäranden sprids över flera partitioner, kan du förbättra genomflöde och klienten svarstider med hjälp av asynkron eller parallella frågor.</span><span class="sxs-lookup"><span data-stu-id="3c099-995">Provided you are spreading your requests across multiple partitions, you can improve throughput and client responsiveness by using asynchronous or parallel queries.</span></span>
<span data-ttu-id="3c099-996">Du kan till exempel ha två eller flera worker rollinstanser åtkomst till dina tabeller parallellt.</span><span class="sxs-lookup"><span data-stu-id="3c099-996">For example, you might have two or more worker role instances accessing your tables in parallel.</span></span> <span data-ttu-id="3c099-997">Du kan har enskilda arbetsroller ansvarar för viss uppsättningar av partitioner eller helt enkelt ha flera worker rollinstanser, varje tillgång till alla partitioner i en tabell.</span><span class="sxs-lookup"><span data-stu-id="3c099-997">You could have individual worker roles responsible for particular sets of partitions, or simply have multiple worker role instances, each able to access all the partitions in a table.</span></span>  

<span data-ttu-id="3c099-998">Inom en klientinstans, kan du förbättra genomflöde genom att köra lagringsåtgärder asynkront.</span><span class="sxs-lookup"><span data-stu-id="3c099-998">Within a client instance, you can improve throughput by executing storage operations asynchronously.</span></span> <span data-ttu-id="3c099-999">Storage-klientbiblioteket gör det enkelt att skriva asynkrona frågor och ändringar.</span><span class="sxs-lookup"><span data-stu-id="3c099-999">The Storage Client Library makes it easy to write asynchronous queries and modifications.</span></span> <span data-ttu-id="3c099-1000">Du kan till exempel starta med synkron metod som hämtar alla entiteter i en partition som visas i följande C#-kod:</span><span class="sxs-lookup"><span data-stu-id="3c099-1000">For example, you might start with the synchronous method that retrieves all the entities in a partition as shown in the following C# code:</span></span>  

```csharp
private static void ManyEntitiesQuery(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

        TableContinuationToken continuationToken = null;

        do
        {
        var employees = employeeTable.ExecuteQuerySegmented(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
    {
        ...
    }
        continuationToken = employees.ContinuationToken;
        } while (continuationToken != null);
}  
```

<span data-ttu-id="3c099-1001">Du kan enkelt ändra den här koden så att frågan körs asynkront på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="3c099-1001">You can easily modify this code so that the query runs asynchronously as follows:</span></span>  

```csharp
private static async Task ManyEntitiesQueryAsync(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);
        TableContinuationToken continuationToken = null;

        do
        {
        var employees = await employeeTable.ExecuteQuerySegmentedAsync(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
        {
            ...
        }
        continuationToken = employees.ContinuationToken;
            } while (continuationToken != null);
}  
```

<span data-ttu-id="3c099-1002">I det här asynkron exemplet ser du följande ändringar från synkron version:</span><span class="sxs-lookup"><span data-stu-id="3c099-1002">In this asynchronous example, you can see the following changes from the synchronous version:</span></span>  

* <span data-ttu-id="3c099-1003">Metodsignaturen innehåller nu den **asynkrona** modifierare och returnerar ett **aktivitet** instans.</span><span class="sxs-lookup"><span data-stu-id="3c099-1003">The method signature now includes the **async** modifier and returns a **Task** instance.</span></span>  
* <span data-ttu-id="3c099-1004">I stället för att anropa den **ExecuteSegmented** metod för att hämta resultat, metoden nu anropar den **ExecuteSegmentedAsync** metod och använder den **await** modifierare till Hämta resultat asynkront.</span><span class="sxs-lookup"><span data-stu-id="3c099-1004">Instead of calling the **ExecuteSegmented** method to retrieve results, the method now calls the **ExecuteSegmentedAsync** method and uses the **await** modifier to retrieve results asynchronously.</span></span>  

<span data-ttu-id="3c099-1005">Klientprogrammet kan anropa metoden flera gånger (med olika värden för den **avdelning** parametern), och varje fråga som körs på en separat tråd.</span><span class="sxs-lookup"><span data-stu-id="3c099-1005">The client application can call this method multiple times (with different values for the **department** parameter), and each query will run on a separate thread.</span></span>  

<span data-ttu-id="3c099-1006">Observera att det finns inga asynkrona versionen av den **kör** metod i den **TableQuery** klassen eftersom den **IEnumerable** gränssnittet stöder inte asynkron uppräkning.</span><span class="sxs-lookup"><span data-stu-id="3c099-1006">Note that there is no asynchronous version of the **Execute** method in the **TableQuery** class because the **IEnumerable** interface does not support asynchronous enumeration.</span></span>  

<span data-ttu-id="3c099-1007">Du kan infoga, uppdatera och ta bort entiteter asynkront.</span><span class="sxs-lookup"><span data-stu-id="3c099-1007">You can also insert, update, and delete entities asynchronously.</span></span> <span data-ttu-id="3c099-1008">Följande C#-exempel visar en enkel, synkron metod för att infoga eller ersätta en medarbetare entitet:</span><span class="sxs-lookup"><span data-stu-id="3c099-1008">The following C# example shows a simple, synchronous method to insert or replace an employee entity:</span></span>  

```csharp
private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = employeeTable
        .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

<span data-ttu-id="3c099-1009">Du kan enkelt ändra den här koden så att uppdateringen körs asynkront på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="3c099-1009">You can easily modify this code so that the update runs asynchronously as follows:</span></span>  

```csharp
private static async Task SimpleEmployeeUpsertAsync(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = await employeeTable
        .ExecuteAsync(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

<span data-ttu-id="3c099-1010">I det här asynkron exemplet ser du följande ändringar från synkron version:</span><span class="sxs-lookup"><span data-stu-id="3c099-1010">In this asynchronous example, you can see the following changes from the synchronous version:</span></span>  

* <span data-ttu-id="3c099-1011">Metodsignaturen innehåller nu den **asynkrona** modifierare och returnerar ett **aktivitet** instans.</span><span class="sxs-lookup"><span data-stu-id="3c099-1011">The method signature now includes the **async** modifier and returns a **Task** instance.</span></span>  
* <span data-ttu-id="3c099-1012">I stället för att anropa den **kör** metod för att uppdatera entiteten metoden nu anropar den **ExecuteAsync** metoden och använder den **await** modifieraren att hämta resultat asynkront.</span><span class="sxs-lookup"><span data-stu-id="3c099-1012">Instead of calling the **Execute** method to update the entity, the method now calls the **ExecuteAsync** method and uses the **await** modifier to retrieve results asynchronously.</span></span>  

<span data-ttu-id="3c099-1013">Klientprogrammet kan anropa flera asynkrona metoder som det här och varje metodanropet körs på en separat tråd.</span><span class="sxs-lookup"><span data-stu-id="3c099-1013">The client application can call multiple asynchronous methods like this one, and each method invocation will run on a separate thread.</span></span>  

### <a name="credits"></a><span data-ttu-id="3c099-1014">Krediter</span><span class="sxs-lookup"><span data-stu-id="3c099-1014">Credits</span></span>
<span data-ttu-id="3c099-1015">Vi vill tacka följande medlemmar i Azure-teamet för deras bidrag: Dominic Betts, Jason Hogg, Jean Ghanem, Jai Haridas, Jeff Irwin, Vamshidhar Kommineni, Vinay Shah och Serdar Ozler samt Tom Hollander från Microsoft DX.</span><span class="sxs-lookup"><span data-stu-id="3c099-1015">We would like to thank the following members of the Azure team for their contributions: Dominic Betts, Jason Hogg, Jean Ghanem, Jai Haridas, Jeff Irwin, Vamshidhar Kommineni, Vinay Shah and Serdar Ozler as well as  Tom Hollander from Microsoft DX.</span></span> 

<span data-ttu-id="3c099-1016">Vi skulle också vilja Tack på följande Microsoft MVPS för deras feedback om värdefulla vid granskning tillfällen: Igor Papirov och Edward Bakker.</span><span class="sxs-lookup"><span data-stu-id="3c099-1016">We would also like to thank the following Microsoft MVP's for their valuable feedback during review cycles: Igor Papirov and Edward Bakker.</span></span>

[1]: ./media/storage-table-design-guide/storage-table-design-IMAGE01.png
[2]: ./media/storage-table-design-guide/storage-table-design-IMAGE02.png
[3]: ./media/storage-table-design-guide/storage-table-design-IMAGE03.png
[4]: ./media/storage-table-design-guide/storage-table-design-IMAGE04.png
[5]: ./media/storage-table-design-guide/storage-table-design-IMAGE05.png
[6]: ./media/storage-table-design-guide/storage-table-design-IMAGE06.png
[7]: ./media/storage-table-design-guide/storage-table-design-IMAGE07.png
[8]: ./media/storage-table-design-guide/storage-table-design-IMAGE08.png
[9]: ./media/storage-table-design-guide/storage-table-design-IMAGE09.png
[10]: ./media/storage-table-design-guide/storage-table-design-IMAGE10.png
[11]: ./media/storage-table-design-guide/storage-table-design-IMAGE11.png
[12]: ./media/storage-table-design-guide/storage-table-design-IMAGE12.png
[13]: ./media/storage-table-design-guide/storage-table-design-IMAGE13.png
[14]: ./media/storage-table-design-guide/storage-table-design-IMAGE14.png
[15]: ./media/storage-table-design-guide/storage-table-design-IMAGE15.png
[16]: ./media/storage-table-design-guide/storage-table-design-IMAGE16.png
[17]: ./media/storage-table-design-guide/storage-table-design-IMAGE17.png
[18]: ./media/storage-table-design-guide/storage-table-design-IMAGE18.png
[19]: ./media/storage-table-design-guide/storage-table-design-IMAGE19.png
[20]: ./media/storage-table-design-guide/storage-table-design-IMAGE20.png
[21]: ./media/storage-table-design-guide/storage-table-design-IMAGE21.png
[22]: ./media/storage-table-design-guide/storage-table-design-IMAGE22.png
[23]: ./media/storage-table-design-guide/storage-table-design-IMAGE23.png
[24]: ./media/storage-table-design-guide/storage-table-design-IMAGE24.png
[25]: ./media/storage-table-design-guide/storage-table-design-IMAGE25.png
[26]: ./media/storage-table-design-guide/storage-table-design-IMAGE26.png
[27]: ./media/storage-table-design-guide/storage-table-design-IMAGE27.png
[28]: ./media/storage-table-design-guide/storage-table-design-IMAGE28.png
[29]: ./media/storage-table-design-guide/storage-table-design-IMAGE29.png

