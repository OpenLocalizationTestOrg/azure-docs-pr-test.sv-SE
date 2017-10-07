---
title: "aaaLearn hur toosecure åt toodata i Azure Cosmos DB | Microsoft Docs"
description: "Lär dig mer om access control begrepp i Azure Cosmos-databasen, inklusive huvudnycklar, skrivskyddade nycklar, användare och behörigheter."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 8641225d-e839-4ba6-a6fd-d6314ae3a51c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.openlocfilehash: fef7f8e14b488f6ceab0f2aa279a1e99d4416f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-access-tooazure-cosmos-db-data"></a><span data-ttu-id="04c9c-103">Skydda åtkomst tooAzure Cosmos DB data</span><span class="sxs-lookup"><span data-stu-id="04c9c-103">Securing access tooAzure Cosmos DB data</span></span>
<span data-ttu-id="04c9c-104">Den här artikeln innehåller en översikt över att skydda åtkomst toodata lagras i [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="04c9c-104">This article provides an overview of securing access toodata stored in [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span>

<span data-ttu-id="04c9c-105">Azure Cosmos-DB använder två typer av nycklar tooauthenticate användare och ger åtkomst till tooits data och resurser.</span><span class="sxs-lookup"><span data-stu-id="04c9c-105">Azure Cosmos DB uses two types of keys tooauthenticate users and provide access tooits data and resources.</span></span> 

|<span data-ttu-id="04c9c-106">Nyckeltyp</span><span class="sxs-lookup"><span data-stu-id="04c9c-106">Key type</span></span>|<span data-ttu-id="04c9c-107">Resurser</span><span class="sxs-lookup"><span data-stu-id="04c9c-107">Resources</span></span>|
|---|---|
|[<span data-ttu-id="04c9c-108">Huvudnycklar</span><span class="sxs-lookup"><span data-stu-id="04c9c-108">Master keys</span></span>](#master-keys) |<span data-ttu-id="04c9c-109">Används för administrativa resurser: databasen konton, databaser, användare och behörigheter</span><span class="sxs-lookup"><span data-stu-id="04c9c-109">Used for administrative resources: database accounts, databases, users, and permissions</span></span>|
|[<span data-ttu-id="04c9c-110">Resurs-token</span><span class="sxs-lookup"><span data-stu-id="04c9c-110">Resource tokens</span></span>](#resource-tokens)|<span data-ttu-id="04c9c-111">Används för programresurser: samlingar, dokument, bifogade filer, lagrade procedurer, utlösare och UDF: er</span><span class="sxs-lookup"><span data-stu-id="04c9c-111">Used for application resources: collections, documents, attachments, stored procedures, triggers, and UDFs</span></span>|

<a id="master-keys"></a>

## <a name="master-keys"></a><span data-ttu-id="04c9c-112">Huvudnycklar</span><span class="sxs-lookup"><span data-stu-id="04c9c-112">Master keys</span></span> 

<span data-ttu-id="04c9c-113">Huvudnycklar ange åtkomst toohello alla hello administrativa resurser för hello konto.</span><span class="sxs-lookup"><span data-stu-id="04c9c-113">Master keys provide access toohello all hello administrative resources for hello database account.</span></span> <span data-ttu-id="04c9c-114">Huvudnycklar:</span><span class="sxs-lookup"><span data-stu-id="04c9c-114">Master keys:</span></span>  
- <span data-ttu-id="04c9c-115">Ge åtkomst tooaccounts, databaser, användare och behörigheter.</span><span class="sxs-lookup"><span data-stu-id="04c9c-115">Provide access tooaccounts, databases, users, and permissions.</span></span> 
- <span data-ttu-id="04c9c-116">Det går inte att vara används tooprovide detaljerade åtkomst toocollections och dokument.</span><span class="sxs-lookup"><span data-stu-id="04c9c-116">Cannot be used tooprovide granular access toocollections and documents.</span></span>
- <span data-ttu-id="04c9c-117">Skapas under hello skapandet av ett konto.</span><span class="sxs-lookup"><span data-stu-id="04c9c-117">Are created during hello creation of an account.</span></span>
- <span data-ttu-id="04c9c-118">Genereras när som helst.</span><span class="sxs-lookup"><span data-stu-id="04c9c-118">Can be regenerated at any time.</span></span>

<span data-ttu-id="04c9c-119">Varje konto består av två huvudnycklar: en primärnyckel och sekundärnyckel.</span><span class="sxs-lookup"><span data-stu-id="04c9c-119">Each account consists of two Master keys: a primary key and secondary key.</span></span> <span data-ttu-id="04c9c-120">hello syftet med dubbla nycklar är så att du kan återskapa eller återställa nycklar, vilket ger kontinuerlig tooyour åtkomstkonto och data.</span><span class="sxs-lookup"><span data-stu-id="04c9c-120">hello purpose of dual keys is so that you can regenerate, or roll keys, providing continuous access tooyour account and data.</span></span> 

<span data-ttu-id="04c9c-121">Det finns två skrivskyddade nycklar i tillägg toohello två huvudnycklar för hello Cosmos-DB-kontot.</span><span class="sxs-lookup"><span data-stu-id="04c9c-121">In addition toohello two master keys for hello Cosmos DB account, there are two read-only keys.</span></span> <span data-ttu-id="04c9c-122">Nycklarna skrivskyddat tillåter endast läsåtgärder på hello-konto.</span><span class="sxs-lookup"><span data-stu-id="04c9c-122">These read-only keys only allow read operations on hello account.</span></span> <span data-ttu-id="04c9c-123">Skrivskyddade nycklar ger inte tillgång tooread behörigheter resurser.</span><span class="sxs-lookup"><span data-stu-id="04c9c-123">Read-only keys do not provide access tooread permissions resources.</span></span>

<span data-ttu-id="04c9c-124">Primära, sekundära, skrivskyddad och skrivskyddad huvudnycklar kan hämtas och återskapas med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="04c9c-124">Primary, secondary, read only, and read-write master keys can be retrieved and regenerated using hello Azure portal.</span></span> <span data-ttu-id="04c9c-125">Instruktioner finns i [visa, kopiera och generera åtkomstnycklar](manage-account.md#keys).</span><span class="sxs-lookup"><span data-stu-id="04c9c-125">For instructions, see [View, copy, and regenerate access keys](manage-account.md#keys).</span></span>

![Åtkomstkontroll (IAM) i hello Azure portal – visar NoSQL database-säkerhet](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

<span data-ttu-id="04c9c-127">hello är rotera din huvudnyckeln enkel.</span><span class="sxs-lookup"><span data-stu-id="04c9c-127">hello process of rotating your master key is simple.</span></span> <span data-ttu-id="04c9c-128">Navigera toohello Azure portal tooretrieve sekundärnyckeln, och sedan ersätta primärnyckel med din sekundärnyckeln i ditt program och rotera hello primärnyckel i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="04c9c-128">Navigate toohello Azure portal tooretrieve your secondary key, then replace your primary key with your secondary key in your application, then rotate hello primary key in hello Azure portal.</span></span>

![Huvudnyckeln rotation i hello Azure portal – visar NoSQL database-säkerhet](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-toouse-a-master-key"></a><span data-ttu-id="04c9c-130">Code exempel toouse en huvudnyckel</span><span class="sxs-lookup"><span data-stu-id="04c9c-130">Code sample toouse a master key</span></span>

<span data-ttu-id="04c9c-131">hello illustrerar följande kodexempel hur toouse en Cosmos-DB kontot slutpunkt och huvudnyckeln tooinstantiate en DocumentClient och skapa en databas.</span><span class="sxs-lookup"><span data-stu-id="04c9c-131">hello following code sample illustrates how toouse a Cosmos DB account endpoint and master key tooinstantiate a DocumentClient and create a database.</span></span> 

```csharp
//Read hello Azure Cosmos DB endpointUrl and authorization keys from config.
//These values are available from hello Azure portal on hello Azure Cosmos DB account blade under "Keys".
//NB > Keep these values in a safe and secure location. Together they provide Administrative access tooyour DocDB account.

private static readonly string endpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
private static readonly SecureString authorizationKey = ToSecureString(ConfigurationManager.AppSettings["AuthorizationKey"]);

client = new DocumentClient(new Uri(endpointUrl), authorizationKey);

// Create Database
Database database = await client.CreateDatabaseAsync(
    new Database
    {
        Id = databaseName
    });
```

<a id="resource-tokens"></a>

## <a name="resource-tokens"></a><span data-ttu-id="04c9c-132">Resurs-token</span><span class="sxs-lookup"><span data-stu-id="04c9c-132">Resource tokens</span></span>

<span data-ttu-id="04c9c-133">Token för resursen ger åtkomst toohello resurser i en databas.</span><span class="sxs-lookup"><span data-stu-id="04c9c-133">Resource tokens provide access toohello application resources within a database.</span></span> <span data-ttu-id="04c9c-134">Resurs-token:</span><span class="sxs-lookup"><span data-stu-id="04c9c-134">Resource tokens:</span></span>
- <span data-ttu-id="04c9c-135">Ge åtkomst toospecific samlingar, partitionsnycklar, dokument, bifogade filer, lagrade procedurer, utlösare och UDF: er.</span><span class="sxs-lookup"><span data-stu-id="04c9c-135">Provide access toospecific collections, partition keys, documents, attachments, stored procedures, triggers, and UDFs.</span></span>
- <span data-ttu-id="04c9c-136">Skapas när en [användare](#users) beviljas [behörigheter](#permissions) tooa viss resurs.</span><span class="sxs-lookup"><span data-stu-id="04c9c-136">Are created when a [user](#users) is granted [permissions](#permissions) tooa specific resource.</span></span>
- <span data-ttu-id="04c9c-137">Återskapas när en resurs behörighet är efterlevts på av POST, hämta eller skicka anrop.</span><span class="sxs-lookup"><span data-stu-id="04c9c-137">Are recreated when a permission resource is acted upon on by POST, GET, or PUT call.</span></span>
- <span data-ttu-id="04c9c-138">Använd en hash-resurs token speciellt konstruerade för hello användare, resurser och behörighet.</span><span class="sxs-lookup"><span data-stu-id="04c9c-138">Use a hash resource token specifically constructed for hello user, resource, and permission.</span></span>
- <span data-ttu-id="04c9c-139">Är Tidsbundna med en anpassningsbara giltighetsperiod.</span><span class="sxs-lookup"><span data-stu-id="04c9c-139">Are time bound with a customizable validity period.</span></span> <span data-ttu-id="04c9c-140">hello standard giltigt timespan är en timme.</span><span class="sxs-lookup"><span data-stu-id="04c9c-140">hello default valid timespan is one hour.</span></span> <span data-ttu-id="04c9c-141">Livslängd för token, men kan uttryckligen anges, in tooa maximalt fem timmar.</span><span class="sxs-lookup"><span data-stu-id="04c9c-141">Token lifetime, however, may be explicitly specified, up tooa maximum of five hours.</span></span>
- <span data-ttu-id="04c9c-142">Ange en säker alternativa toogiving ut hello huvudnyckel.</span><span class="sxs-lookup"><span data-stu-id="04c9c-142">Provide a safe alternative toogiving out hello master key.</span></span> 
- <span data-ttu-id="04c9c-143">Aktivera klienter tooread, skriva och ta bort resurser i hello Cosmos DB konto enligt toohello behörighet de tilldelats.</span><span class="sxs-lookup"><span data-stu-id="04c9c-143">Enable clients tooread, write, and delete resources in hello Cosmos DB account according toohello permissions they've been granted.</span></span>

<span data-ttu-id="04c9c-144">Du kan använda en resurs-token (genom att skapa Cosmos-DB-användare och behörigheter) när du vill tooprovide åtkomst tooresources i Cosmos-DB kontot tooa klient som inte är betrott med hello huvudnyckel.</span><span class="sxs-lookup"><span data-stu-id="04c9c-144">You can use a resource token (by creating Cosmos DB users and permissions) when you want tooprovide access tooresources in your Cosmos DB account tooa client that cannot be trusted with hello master key.</span></span>  

<span data-ttu-id="04c9c-145">Cosmos DB resurs token är en säker alternativ som gör att klienter tooread, skriva och ta bort resurser i Cosmos-DB-kontot enligt toohello behörigheter som du har beviljat och utan behov av antingen master eller läsa endast nyckel.</span><span class="sxs-lookup"><span data-stu-id="04c9c-145">Cosmos DB resource tokens provide a safe alternative that enables clients tooread, write, and delete resources in your Cosmos DB account according toohello permissions you've granted, and without need for either a master or read only key.</span></span>

<span data-ttu-id="04c9c-146">Här är en typisk designmönstret där resurs token kan begärs, skapas och levereras tooclients:</span><span class="sxs-lookup"><span data-stu-id="04c9c-146">Here is a typical design pattern whereby resource tokens may be requested, generated, and delivered tooclients:</span></span>

1. <span data-ttu-id="04c9c-147">En tjänst halva nivå ställs in tooserve en mobilprogram tooshare Användarfoton.</span><span class="sxs-lookup"><span data-stu-id="04c9c-147">A mid-tier service is set up tooserve a mobile application tooshare user photos.</span></span> 
2. <span data-ttu-id="04c9c-148">tjänst för hello halva nivå har hello huvudnyckeln för hello Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="04c9c-148">hello mid-tier service possesses hello master key of hello Cosmos DB account.</span></span>
3. <span data-ttu-id="04c9c-149">hello foto-app är installerad på slutanvändarens mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="04c9c-149">hello photo app is installed on end-user mobile devices.</span></span> 
4. <span data-ttu-id="04c9c-150">Du loggar in upprättar hello foto-app hello identitet hello användare med tjänst för hello halva nivå.</span><span class="sxs-lookup"><span data-stu-id="04c9c-150">On login, hello photo app establishes hello identity of hello user with hello mid-tier service.</span></span> <span data-ttu-id="04c9c-151">Den här mekanismen för identitet etablering är rent upp toohello program.</span><span class="sxs-lookup"><span data-stu-id="04c9c-151">This mechanism of identity establishment is purely up toohello application.</span></span>
5. <span data-ttu-id="04c9c-152">När hello identitet har upprättats tjänstbegäranden hello halva nivå behörigheter baserat på hello identitet.</span><span class="sxs-lookup"><span data-stu-id="04c9c-152">Once hello identity is established, hello mid-tier service requests permissions based on hello identity.</span></span>
6. <span data-ttu-id="04c9c-153">hello halva nivå tjänsten skickar en resurs token tillbaka toohello phone app.</span><span class="sxs-lookup"><span data-stu-id="04c9c-153">hello mid-tier service sends a resource token back toohello phone app.</span></span>
7. <span data-ttu-id="04c9c-154">hello phone-app fortsätta toouse hello resurs token toodirectly åtkomst Cosmos DB resurser med hello behörigheter har definierats av hello resurs token och för hello-intervall som tillåts av hello resurs token.</span><span class="sxs-lookup"><span data-stu-id="04c9c-154">hello phone app can continue toouse hello resource token toodirectly access Cosmos DB resources with hello permissions defined by hello resource token and for hello interval allowed by hello resource token.</span></span> 
8. <span data-ttu-id="04c9c-155">När hello resurs token upphör att gälla, får ett 401 obehörig undantag efterföljande förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="04c9c-155">When hello resource token expires, subsequent requests receive a 401 unauthorized exception.</span></span>  <span data-ttu-id="04c9c-156">Nu hello telefonapp återupprättar hello identitet och begär en ny resurs-token.</span><span class="sxs-lookup"><span data-stu-id="04c9c-156">At this point, hello phone app re-establishes hello identity and requests a new resource token.</span></span>

    ![Azure DB Cosmos-resurs tokens arbetsflöde](./media/secure-access-to-data/resourcekeyworkflow.png)

<span data-ttu-id="04c9c-158">Resurs-token generering och hantering hanteras av hello interna Cosmos DB klientbibliotek; Om du använder REST måste du skapa hello begäran/autentiseringshuvuden.</span><span class="sxs-lookup"><span data-stu-id="04c9c-158">Resource token generation and management is handled by hello native Cosmos DB client libraries; however, if you use REST you must construct hello request/authentication headers.</span></span> <span data-ttu-id="04c9c-159">Mer information om hur du skapar autentiseringshuvuden för REST finns [åtkomstkontroll på Cosmos DB resurser](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) eller hello [källkoden för våra SDK](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).</span><span class="sxs-lookup"><span data-stu-id="04c9c-159">For more information on creating authentication headers for REST, see [Access Control on Cosmos DB Resources](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) or hello [source code for our SDKs](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).</span></span>

<span data-ttu-id="04c9c-160">Ett exempel på en tjänst på mellannivå används toogenerate eller broker resurs-token, finns hello [ResourceTokenBroker app](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).</span><span class="sxs-lookup"><span data-stu-id="04c9c-160">For an example of a middle tier service used toogenerate or broker resource tokens, see hello [ResourceTokenBroker app](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).</span></span>

<a id="users"></a>

## <a name="users"></a><span data-ttu-id="04c9c-161">Användare</span><span class="sxs-lookup"><span data-stu-id="04c9c-161">Users</span></span>
<span data-ttu-id="04c9c-162">Cosmos DB användare har associerats med en Cosmos-DB-databas.</span><span class="sxs-lookup"><span data-stu-id="04c9c-162">Cosmos DB users are associated with a Cosmos DB database.</span></span>  <span data-ttu-id="04c9c-163">Varje databas kan innehålla noll eller flera Cosmos-DB-användare.</span><span class="sxs-lookup"><span data-stu-id="04c9c-163">Each database can contain zero or more Cosmos DB users.</span></span>  <span data-ttu-id="04c9c-164">Hej följande exempel visas hur toocreate en Cosmos-DB användarresurs.</span><span class="sxs-lookup"><span data-stu-id="04c9c-164">hello following code sample shows how toocreate a Cosmos DB user resource.</span></span>

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> <span data-ttu-id="04c9c-165">Varje Cosmos-DB-användare har en PermissionsLink-egenskap som kan vara används tooretrieve hello lista över [behörigheter](#permissions) som är associerade med hello användare.</span><span class="sxs-lookup"><span data-stu-id="04c9c-165">Each Cosmos DB user has a PermissionsLink property that can be used tooretrieve hello list of [permissions](#permissions) associated with hello user.</span></span>
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a><span data-ttu-id="04c9c-166">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="04c9c-166">Permissions</span></span>
<span data-ttu-id="04c9c-167">En Cosmos-DB behörighet resurs är associerad med en Cosmos-DB-användare.</span><span class="sxs-lookup"><span data-stu-id="04c9c-167">A Cosmos DB permission resource is associated with a Cosmos DB user.</span></span>  <span data-ttu-id="04c9c-168">Varje användare kan innehålla noll eller flera Cosmos-DB-behörigheter.</span><span class="sxs-lookup"><span data-stu-id="04c9c-168">Each user may contain zero or more Cosmos DB permissions.</span></span>  <span data-ttu-id="04c9c-169">En behörighet resurs ger åtkomst tooa säkerhetstoken som hello användarnas behov när du försöker tooaccess en resurs för specifika program.</span><span class="sxs-lookup"><span data-stu-id="04c9c-169">A permission resource provides access tooa security token that hello user needs when trying tooaccess a specific application resource.</span></span>
<span data-ttu-id="04c9c-170">Det finns två tillgängliga åtkomstnivåer som kan tillhandahållas av en behörighet:</span><span class="sxs-lookup"><span data-stu-id="04c9c-170">There are two available access levels that may be provided by a permission resource:</span></span>

* <span data-ttu-id="04c9c-171">Alla: hello användaren har fullständig behörighet för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="04c9c-171">All: hello user has full permission on hello resource.</span></span>
* <span data-ttu-id="04c9c-172">Läs: hello användare kan endast läsa hello innehållet i hello resurs men kan inte utföra skrivåtgärder-, update- eller delete-åtgärder på hello resursen.</span><span class="sxs-lookup"><span data-stu-id="04c9c-172">Read: hello user can only read hello contents of hello resource but cannot perform write, update, or delete operations on hello resource.</span></span>

> [!NOTE]
> <span data-ttu-id="04c9c-173">Användaren i ordning toorun Cosmos DB lagrade procedurer hello måste ha hello alla behörighet på hello samling i vilka hello lagrade proceduren körs.</span><span class="sxs-lookup"><span data-stu-id="04c9c-173">In order toorun Cosmos DB stored procedures hello user must have hello All permission on hello collection in which hello stored procedure will be run.</span></span>
> 
> 

### <a name="code-sample-toocreate-permission"></a><span data-ttu-id="04c9c-174">Koden exempel toocreate behörighet</span><span class="sxs-lookup"><span data-stu-id="04c9c-174">Code sample toocreate permission</span></span>

<span data-ttu-id="04c9c-175">hello följande kodexempel visar hur toocreate en behörighet resurs läsa hello resurs token av hello behörighet för resursen och koppla hello behörigheter till hello [användaren](#users) skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="04c9c-175">hello following code sample shows how toocreate a permission resource, read hello resource token of hello permission resource, and associate hello permissions with hello [user](#users) created above.</span></span>

```csharp
// Create a permission.
Permission docPermission = new Permission
{
    PermissionMode = PermissionMode.Read,
    ResourceLink = documentCollection.SelfLink,
    Id = "readperm"
};
  
docPermission = await client.CreatePermissionAsync(UriFactory.CreateUserUri("db", "user"), docPermission);
Console.WriteLine(docPermission.Id + " has token of: " + docPermission.Token);
```

<span data-ttu-id="04c9c-176">Om du har angett en partitionsnyckel för samlingen, sedan hello-behörighet för insamling, dokument och bilaga resurser måste också innehålla hello ResourcePartitionKey i tillägget toohello ResourceLink.</span><span class="sxs-lookup"><span data-stu-id="04c9c-176">If you have specified a partition key for your collection, then hello permission for collection, document, and attachment resources must also include hello ResourcePartitionKey in addition toohello ResourceLink.</span></span>

### <a name="code-sample-tooread-permissions-for-user"></a><span data-ttu-id="04c9c-177">Koden exempel tooread behörigheter för användare</span><span class="sxs-lookup"><span data-stu-id="04c9c-177">Code sample tooread permissions for user</span></span>

<span data-ttu-id="04c9c-178">tooeasily hämta alla behörighet resurser som är associerade med en viss användare, Cosmos DB tillgängliggör en behörighet för varje användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="04c9c-178">tooeasily obtain all permission resources associated with a particular user, Cosmos DB makes available a permission feed for each user object.</span></span>  <span data-ttu-id="04c9c-179">hello följande kodavsnitt visar hur tooretrieve hello behörigheten som är associerad med hello användaren som skapade ovan, skapa en behörighetslista och initiera en ny Dokumentklient hello användarens räkning.</span><span class="sxs-lookup"><span data-stu-id="04c9c-179">hello following code snippet shows how tooretrieve hello permission associated with hello user created above, construct a permission list, and instantiate a new DocumentClient on behalf of hello user.</span></span>

```csharp
//Read a permission feed.
FeedResponse<Permission> permFeed = await client.ReadPermissionFeedAsync(
  UriFactory.CreateUserUri("db", "myUser"));
 List<Permission> permList = new List<Permission>();

foreach (Permission perm in permFeed)
{
    permList.Add(perm);
}

DocumentClient userClient = new DocumentClient(new Uri(endpointUrl), permList);
```

## <a name="next-steps"></a><span data-ttu-id="04c9c-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="04c9c-180">Next steps</span></span>
* <span data-ttu-id="04c9c-181">toolearn mer om Cosmos DB databassäkerhet finns [Cosmos DB: databasen säkerhet](database-security.md).</span><span class="sxs-lookup"><span data-stu-id="04c9c-181">toolearn more about Cosmos DB database security, see [Cosmos DB: Database security](database-security.md).</span></span>
* <span data-ttu-id="04c9c-182">toolearn om hur du hanterar master och skrivskyddade nycklar, se [hur toomanage ett konto i Azure Cosmos DB](manage-account.md#keys).</span><span class="sxs-lookup"><span data-stu-id="04c9c-182">toolearn about managing master and read-only keys, see [How toomanage an Azure Cosmos DB account](manage-account.md#keys).</span></span>
* <span data-ttu-id="04c9c-183">toolearn hur tooconstruct Azure Cosmos DB auktorisering token Se [åtkomstkontroll på Azure Cosmos DB resurser](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).</span><span class="sxs-lookup"><span data-stu-id="04c9c-183">toolearn how tooconstruct Azure Cosmos DB authorization tokens, see [Access Control on Azure Cosmos DB Resources](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).</span></span>
