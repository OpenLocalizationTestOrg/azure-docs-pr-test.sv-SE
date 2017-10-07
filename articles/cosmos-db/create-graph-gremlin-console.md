---
title: "Självstudier Azure Cosmos DB: CrSkapa, fråga och passera i Apache TinkerPops Gremlin-konsol | Microsoft Docs"
description: "Ett Azure Cosmos DB quickstart toocreates formhörnen, kanter och frågor med hello Azure Cosmos DB Graph API."
services: cosmos-db
author: dennyglee
manager: jhubbard
editor: monicar
ms.assetid: bf08e031-718a-4a2a-89d6-91e12ff8797d
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: terminal
ms.topic: hero-article
ms.date: 07/27/2017
ms.author: denlee
ms.openlocfilehash: 9de64c97fec89c45cecba9e14214db472ec76f57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-query-and-traverse-a-graph-in-hello-gremlin-console"></a><span data-ttu-id="647d8-103">Azure Cosmos DB: Skapa, fråga, och bläddra i ett diagram i hello Gremlin-konsolen</span><span class="sxs-lookup"><span data-stu-id="647d8-103">Azure Cosmos DB: Create, query, and traverse a graph in hello Gremlin console</span></span>

<span data-ttu-id="647d8-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="647d8-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="647d8-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="647d8-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="647d8-106">Den här snabbstartsguide visar hur toocreate Azure DB som Cosmos-kontot, databas och diagram (behållaren) med hjälp av hello Azure-portalen och Använd hello [Gremlin konsolen](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) från [Apache TinkerPop](http://tinkerpop.apache.org) toowork med Diagramdata API (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="647d8-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, database, and graph (container) using hello Azure portal and then use hello [Gremlin Console](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) from  [Apache TinkerPop](http://tinkerpop.apache.org) toowork with Graph API (preview) data.</span></span> <span data-ttu-id="647d8-107">I kursen får du skapa och läsa formhörnen och kanter, uppdaterar en vertex-egenskap, fråga formhörnen passerar hello diagram och ta bort en nod.</span><span class="sxs-lookup"><span data-stu-id="647d8-107">In this tutorial, you create and query vertices and edges, updating a vertex property, query vertices, traverse hello graph, and drop a vertex.</span></span>

![Azure Cosmos-DB hello Apache Gremlin konsolen](./media/create-graph-gremlin-console/gremlin-console.png)

<span data-ttu-id="647d8-109">Hej Gremlin konsolen är Groovy/Java-baserad och körs på Linux, Mac och Windows.</span><span class="sxs-lookup"><span data-stu-id="647d8-109">hello Gremlin console is Groovy/Java based and runs on Linux, Mac, and Windows.</span></span> <span data-ttu-id="647d8-110">Du kan ladda ned det från hello [Apache TinkerPop plats](https://www.apache.org/dyn/closer.lua/tinkerpop/3.2.5/apache-tinkerpop-gremlin-console-3.2.5-bin.zip).</span><span class="sxs-lookup"><span data-stu-id="647d8-110">You can download it from hello [Apache TinkerPop site](https://www.apache.org/dyn/closer.lua/tinkerpop/3.2.5/apache-tinkerpop-gremlin-console-3.2.5-bin.zip).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="647d8-111">Krav</span><span class="sxs-lookup"><span data-stu-id="647d8-111">Prerequisites</span></span>

<span data-ttu-id="647d8-112">Du behöver toohave en Azure-prenumeration toocreate ett Azure DB som Cosmos-konto för denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="647d8-112">You need toohave an Azure subscription toocreate an Azure Cosmos DB account for this quickstart.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<span data-ttu-id="647d8-113">Du måste också tooinstall hello [Gremlin konsolen](http://tinkerpop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="647d8-113">You also need tooinstall hello [Gremlin Console](http://tinkerpop.apache.org/).</span></span> <span data-ttu-id="647d8-114">Använd version 3.2.5 eller senare.</span><span class="sxs-lookup"><span data-stu-id="647d8-114">Use version 3.2.5 or above.</span></span>

## <a name="create-a-database-account"></a><span data-ttu-id="647d8-115">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="647d8-115">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a><span data-ttu-id="647d8-116">Lägga till en graf</span><span class="sxs-lookup"><span data-stu-id="647d8-116">Add a graph</span></span>

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <span data-ttu-id="647d8-117"><a id="ConnectAppService"></a>Ansluta tooyour apptjänst</span><span class="sxs-lookup"><span data-stu-id="647d8-117"><a id="ConnectAppService"></a>Connect tooyour app service</span></span>
1. <span data-ttu-id="647d8-118">Hej Gremlin konsolen innan du startar, skapa eller ändra hello remote secure.yaml konfigurationsfilen i hello apache-tinkerpop-gremlin-console-3.2.5/conf directory.</span><span class="sxs-lookup"><span data-stu-id="647d8-118">Before starting hello Gremlin Console, create or modify hello remote-secure.yaml configuration file in hello apache-tinkerpop-gremlin-console-3.2.5/conf directory.</span></span>
2. <span data-ttu-id="647d8-119">Fyll i din *värd*, *port*, *användarnamn*, *lösenord*, *connectionPool* och *serialiserarens* konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="647d8-119">Fill in your *host*, *port*, *username*, *password*, *connectionPool*, and *serializer* configurations:</span></span>

    <span data-ttu-id="647d8-120">Inställning</span><span class="sxs-lookup"><span data-stu-id="647d8-120">Setting</span></span>|<span data-ttu-id="647d8-121">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="647d8-121">Suggested value</span></span>|<span data-ttu-id="647d8-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="647d8-122">Description</span></span>
    ---|---|---
    <span data-ttu-id="647d8-123">värdar</span><span class="sxs-lookup"><span data-stu-id="647d8-123">hosts</span></span>|<span data-ttu-id="647d8-124">[***.graphs.azure.com]</span><span class="sxs-lookup"><span data-stu-id="647d8-124">[***.graphs.azure.com]</span></span>|<span data-ttu-id="647d8-125">Se skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="647d8-125">See screenshot below.</span></span> <span data-ttu-id="647d8-126">Detta är hello Gremlin URI-värde på översiktssidan för hello av hello Azure-portalen inom hakparenteser med hello avslutande: 443 / tas bort.</span><span class="sxs-lookup"><span data-stu-id="647d8-126">This is hello Gremlin URI value on hello Overview page of hello Azure portal, in square brackets, with hello trailing :443/ removed.</span></span><br><br><span data-ttu-id="647d8-127">Det här värdet kan också hämtas från hello nycklar på fliken med hello URI-värde genom att ta bort https://, ändra dokument toographs och tar bort avslutande hello: 443 /.</span><span class="sxs-lookup"><span data-stu-id="647d8-127">This value can also be retrieved from hello Keys tab, using hello URI value by removing https://, changing documents toographs, and removing hello trailing :443/.</span></span>
    <span data-ttu-id="647d8-128">port</span><span class="sxs-lookup"><span data-stu-id="647d8-128">port</span></span>|<span data-ttu-id="647d8-129">443</span><span class="sxs-lookup"><span data-stu-id="647d8-129">443</span></span>|<span data-ttu-id="647d8-130">Ange too443.</span><span class="sxs-lookup"><span data-stu-id="647d8-130">Set too443.</span></span>
    <span data-ttu-id="647d8-131">användarnamn</span><span class="sxs-lookup"><span data-stu-id="647d8-131">username</span></span>|<span data-ttu-id="647d8-132">*Ditt användarnamn*</span><span class="sxs-lookup"><span data-stu-id="647d8-132">*Your username*</span></span>|<span data-ttu-id="647d8-133">Hej resurs hello formatet `/dbs/<db>/colls/<coll>` där `<db>` är databasnamnet och `<coll>` är samlingens namn.</span><span class="sxs-lookup"><span data-stu-id="647d8-133">hello resource of hello form `/dbs/<db>/colls/<coll>` where `<db>` is your database name and `<coll>` is your collection name.</span></span>
    <span data-ttu-id="647d8-134">lösenord</span><span class="sxs-lookup"><span data-stu-id="647d8-134">password</span></span>|<span data-ttu-id="647d8-135">*Din primärnyckel*</span><span class="sxs-lookup"><span data-stu-id="647d8-135">*Your primary key*</span></span>| <span data-ttu-id="647d8-136">Se andra skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="647d8-136">See second screenshot below.</span></span> <span data-ttu-id="647d8-137">Det här är ditt primära nyckel som du kan hämta hello nycklar sidan för hello Azure-portalen hello primärnyckel i rutan.</span><span class="sxs-lookup"><span data-stu-id="647d8-137">This is your primary key, which you can retrieve from hello Keys page of hello Azure portal, in hello Primary Key box.</span></span> <span data-ttu-id="647d8-138">Använd kopieringsknappen hello hello vänster på hello toocopy hello värde.</span><span class="sxs-lookup"><span data-stu-id="647d8-138">Use hello copy button on hello left side of hello box toocopy hello value.</span></span>
    <span data-ttu-id="647d8-139">ConnectionPool</span><span class="sxs-lookup"><span data-stu-id="647d8-139">connectionPool</span></span>|<span data-ttu-id="647d8-140">{enableSsl: true}</span><span class="sxs-lookup"><span data-stu-id="647d8-140">{enableSsl: true}</span></span>|<span data-ttu-id="647d8-141">Din anslutningspoolinställning för SSL.</span><span class="sxs-lookup"><span data-stu-id="647d8-141">Your connection pool setting for SSL.</span></span>
    <span data-ttu-id="647d8-142">Serialiserare</span><span class="sxs-lookup"><span data-stu-id="647d8-142">serializer</span></span>|<span data-ttu-id="647d8-143">{ className: org.apache.tinkerpop.gremlin.</span><span class="sxs-lookup"><span data-stu-id="647d8-143">{ className: org.apache.tinkerpop.gremlin.</span></span><br><span data-ttu-id="647d8-144">driver.ser.GraphSONMessageSerializerV1d0,</span><span class="sxs-lookup"><span data-stu-id="647d8-144">driver.ser.GraphSONMessageSerializerV1d0,</span></span><br> <span data-ttu-id="647d8-145">config: { serializeResultToString: true }}</span><span class="sxs-lookup"><span data-stu-id="647d8-145">config: { serializeResultToString: true }}</span></span>|<span data-ttu-id="647d8-146">Värdet för toothis och ta bort de `\n` radbrytningar när den klistras in i hello-värdet.</span><span class="sxs-lookup"><span data-stu-id="647d8-146">Set toothis value and delete any `\n` line breaks when pasting in hello value.</span></span>

    <span data-ttu-id="647d8-147">Kopiera hello för hello värdar värdet **Gremlin URI** värde från hello **översikt** sida: ![visa och kopiera hello Gremlin URI-värde på översiktssidan för hello i hello Azure-portalen](./media/create-graph-gremlin-console/gremlin-uri.png)</span><span class="sxs-lookup"><span data-stu-id="647d8-147">For hello hosts value, copy hello **Gremlin URI** value from hello **Overview** page: ![View and copy hello Gremlin URI value on hello Overview page in hello Azure portal](./media/create-graph-gremlin-console/gremlin-uri.png)</span></span>

    <span data-ttu-id="647d8-148">Kopiera hello för hello lösenordsvärdet **primärnyckel** från hello **nycklar** sida: ![visa och kopiera primärnyckeln i hello Azure-portalen nycklar sida](./media/create-graph-gremlin-console/keys.png)</span><span class="sxs-lookup"><span data-stu-id="647d8-148">For hello password value, copy hello **Primary key** from hello **Keys** page: ![View and copy your primary key in hello Azure portal, Keys page](./media/create-graph-gremlin-console/keys.png)</span></span>


3. <span data-ttu-id="647d8-149">I terminalen kör `bin/gremlin.bat` eller `bin/gremlin.sh` toostart hello [Gremlin konsolen](http://tinkerpop.apache.org/docs/3.2.5/tutorials/getting-started/).</span><span class="sxs-lookup"><span data-stu-id="647d8-149">In your terminal, run `bin/gremlin.bat` or `bin/gremlin.sh` toostart hello [Gremlin Console](http://tinkerpop.apache.org/docs/3.2.5/tutorials/getting-started/).</span></span>
4. <span data-ttu-id="647d8-150">I terminalen kör `:remote connect tinkerpop.server conf/remote-secure.yaml` tooconnect tooyour apptjänst.</span><span class="sxs-lookup"><span data-stu-id="647d8-150">In your terminal, run `:remote connect tinkerpop.server conf/remote-secure.yaml` tooconnect tooyour app service.</span></span>

    > [!TIP]
    > <span data-ttu-id="647d8-151">Om felmeddelandet hello `No appenders could be found for logger` se till att uppdateras hello serialiseraren värdet i hello remote secure.yaml filen enligt beskrivningen i steg 2.</span><span class="sxs-lookup"><span data-stu-id="647d8-151">If you receive hello error `No appenders could be found for logger` ensure that you updated hello serializer value in hello remote-secure.yaml file as described in step 2.</span></span> 

<span data-ttu-id="647d8-152">Bra!</span><span class="sxs-lookup"><span data-stu-id="647d8-152">Great!</span></span> <span data-ttu-id="647d8-153">Nu när vi avslutat hello inställningarna Låt oss börja med vissa kommandon.</span><span class="sxs-lookup"><span data-stu-id="647d8-153">Now that we finished hello setup, let's start running some console commands.</span></span>

<span data-ttu-id="647d8-154">Prova med ett enkelt count()-kommando.</span><span class="sxs-lookup"><span data-stu-id="647d8-154">Let's try a simple count() command.</span></span> <span data-ttu-id="647d8-155">Skriv följande hello i hello konsolen hello i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="647d8-155">Type hello following into hello console at hello prompt:</span></span>
```
:> g.V().count()
```

> [!TIP]
> <span data-ttu-id="647d8-156">Meddelande hello `:>` som föregår hello `g.V().count()` text?</span><span class="sxs-lookup"><span data-stu-id="647d8-156">Notice hello `:>` that precedes hello `g.V().count()` text?</span></span> 
>
> <span data-ttu-id="647d8-157">Detta är en del av hello-kommando som du behöver tootype.</span><span class="sxs-lookup"><span data-stu-id="647d8-157">This is part of hello command you need tootype.</span></span> <span data-ttu-id="647d8-158">Det är viktigt när du använder hello Gremlin konsolen med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="647d8-158">It is important when using hello Gremlin console, with Azure Cosmos DB.</span></span>  
>
> <span data-ttu-id="647d8-159">Om du utesluter detta `:>` prefix instruerar hello konsolen tooexecute hello kommando lokalt, ofta mot en InMemory-diagram.</span><span class="sxs-lookup"><span data-stu-id="647d8-159">Omitting this `:>` prefix instructs hello console tooexecute hello command locally, often against an in-memory graph.</span></span>
> <span data-ttu-id="647d8-160">Med den här `:>` talar om hello konsolen tooexecute fjärrkommandot, i det här fallet mot Cosmos DB (antingen localhost emulatorn hello eller en > Azure-instans).</span><span class="sxs-lookup"><span data-stu-id="647d8-160">Using this `:>` tells hello console tooexecute a remote command, in this case against Cosmos DB (either hello localhost emulator, or an > Azure instance).</span></span>


## <a name="create-vertices-and-edges"></a><span data-ttu-id="647d8-161">Skapa hörn och gränser</span><span class="sxs-lookup"><span data-stu-id="647d8-161">Create vertices and edges</span></span>

<span data-ttu-id="647d8-162">Vi börjar med att lägga till fem personhörn för *Thomas*, *Mary Kay*, *Robin*, *Ben* och *Jack*.</span><span class="sxs-lookup"><span data-stu-id="647d8-162">Let's begin by adding five person vertices for *Thomas*, *Mary Kay*, *Robin*, *Ben*, and *Jack*.</span></span>

<span data-ttu-id="647d8-163">Indata (Thomas):</span><span class="sxs-lookup"><span data-stu-id="647d8-163">Input (Thomas):</span></span>

```
:> g.addV('person').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44).property('userid', 1)
```

<span data-ttu-id="647d8-164">Resultat:</span><span class="sxs-lookup"><span data-stu-id="647d8-164">Output:</span></span>

```
==>[id:796cdccc-2acd-4e58-a324-91d6f6f5ed6d,label:person,type:vertex,properties:[firstName:[[id:f02a749f-b67c-4016-850e-910242d68953,value:Thomas]],lastName:[[id:f5fa3126-8818-4fda-88b0-9bb55145ce5c,value:Andersen]],age:[[id:f6390f9c-e563-433e-acbf-25627628016e,value:44]],userid:[[id:796cdccc-2acd-4e58-a324-91d6f6f5ed6d|userid,value:1]]]]
```
<span data-ttu-id="647d8-165">Indata (Mary Kay):</span><span class="sxs-lookup"><span data-stu-id="647d8-165">Input (Mary Kay):</span></span>

```
:> g.addV('person').property('firstName', 'Mary Kay').property('lastName', 'Andersen').property('age', 39).property('userid', 2)

```

<span data-ttu-id="647d8-166">Resultat:</span><span class="sxs-lookup"><span data-stu-id="647d8-166">Output:</span></span>

```
==>[id:0ac9be25-a476-4a30-8da8-e79f0119ea5e,label:person,type:vertex,properties:[firstName:[[id:ea0604f8-14ee-4513-a48a-1734a1f28dc0,value:Mary Kay]],lastName:[[id:86d3bba5-fd60-4856-9396-c195ef7d7f4b,value:Andersen]],age:[[id:bc81b78d-30c4-4e03-8f40-50f72eb5f6da,value:39]],userid:[[id:0ac9be25-a476-4a30-8da8-e79f0119ea5e|userid,value:2]]]]

```

<span data-ttu-id="647d8-167">Indata (Robin):</span><span class="sxs-lookup"><span data-stu-id="647d8-167">Input (Robin):</span></span>

```
:> g.addV('person').property('firstName', 'Robin').property('lastName', 'Wakefield').property('userid', 3)
```

<span data-ttu-id="647d8-168">Resultat:</span><span class="sxs-lookup"><span data-stu-id="647d8-168">Output:</span></span>

```
==>[id:8dc14d6a-8683-4a54-8d74-7eef1fb43a3e,label:person,type:vertex,properties:[firstName:[[id:ec65f078-7a43-4cbe-bc06-e50f2640dc4e,value:Robin]],lastName:[[id:a3937d07-0e88-45d3-a442-26fcdfb042ce,value:Wakefield]],userid:[[id:8dc14d6a-8683-4a54-8d74-7eef1fb43a3e|userid,value:3]]]]
```

<span data-ttu-id="647d8-169">Indata (Ben):</span><span class="sxs-lookup"><span data-stu-id="647d8-169">Input (Ben):</span></span>

```
:> g.addV('person').property('firstName', 'Ben').property('lastName', 'Miller').property('userid', 4)

```

<span data-ttu-id="647d8-170">Resultat:</span><span class="sxs-lookup"><span data-stu-id="647d8-170">Output:</span></span>

```
==>[id:ee86b670-4d24-4966-9a39-30529284b66f,label:person,type:vertex,properties:[firstName:[[id:a632469b-30fc-4157-840c-b80260871e9a,value:Ben]],lastName:[[id:4a08d307-0719-47c6-84ae-1b0b06630928,value:Miller]],userid:[[id:ee86b670-4d24-4966-9a39-30529284b66f|userid,value:4]]]]
```

<span data-ttu-id="647d8-171">Indata (Jack):</span><span class="sxs-lookup"><span data-stu-id="647d8-171">Input (Jack):</span></span>

```
:> g.addV('person').property('firstName', 'Jack').property('lastName', 'Connor').property('userid', 5)
```

<span data-ttu-id="647d8-172">Resultat:</span><span class="sxs-lookup"><span data-stu-id="647d8-172">Output:</span></span>

```
==>[id:4c835f2a-ea5b-43bb-9b6b-215488ad8469,label:person,type:vertex,properties:[firstName:[[id:4250824e-4b72-417f-af98-8034aa15559f,value:Jack]],lastName:[[id:44c1d5e1-a831-480a-bf94-5167d133549e,value:Connor]],userid:[[id:4c835f2a-ea5b-43bb-9b6b-215488ad8469|userid,value:5]]]]
```


<span data-ttu-id="647d8-173">Nu ska vi lägga till gränser för personernas relationer.</span><span class="sxs-lookup"><span data-stu-id="647d8-173">Next, let's add edges for relationships between our people.</span></span>

<span data-ttu-id="647d8-174">Indata (Thomas -> Mary Kay):</span><span class="sxs-lookup"><span data-stu-id="647d8-174">Input (Thomas -> Mary Kay):</span></span>

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Mary Kay'))
```

<span data-ttu-id="647d8-175">Resultat:</span><span class="sxs-lookup"><span data-stu-id="647d8-175">Output:</span></span>

```
==>[id:c12bf9fb-96a1-4cb7-a3f8-431e196e702f,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:0d1fa428-780c-49a5-bd3a-a68d96391d5c,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

<span data-ttu-id="647d8-176">Indata (Thomas -> Robin):</span><span class="sxs-lookup"><span data-stu-id="647d8-176">Input (Thomas -> Robin):</span></span>

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Robin'))
```

<span data-ttu-id="647d8-177">Resultat:</span><span class="sxs-lookup"><span data-stu-id="647d8-177">Output:</span></span>

```
==>[id:58319bdd-1d3e-4f17-a106-0ddf18719d15,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:3e324073-ccfc-4ae1-8675-d450858ca116,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

<span data-ttu-id="647d8-178">Indata (Robin -> Ben):</span><span class="sxs-lookup"><span data-stu-id="647d8-178">Input (Robin -> Ben):</span></span>

```
:> g.V().hasLabel('person').has('firstName', 'Robin').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Ben'))
```

<span data-ttu-id="647d8-179">Resultat:</span><span class="sxs-lookup"><span data-stu-id="647d8-179">Output:</span></span>

```
==>[id:889c4d3c-549e-4d35-bc21-a3d1bfa11e00,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:40fd641d-546e-412a-abcc-58fe53891aab,outV:3e324073-ccfc-4ae1-8675-d450858ca116]
```

## <a name="update-a-vertex"></a><span data-ttu-id="647d8-180">Uppdatera ett hörn</span><span class="sxs-lookup"><span data-stu-id="647d8-180">Update a vertex</span></span>

<span data-ttu-id="647d8-181">Vi uppdaterar hello *Thomas* hörn med nya tidens *45*.</span><span class="sxs-lookup"><span data-stu-id="647d8-181">Let's update hello *Thomas* vertex with a new age of *45*.</span></span>

<span data-ttu-id="647d8-182">Indata:</span><span class="sxs-lookup"><span data-stu-id="647d8-182">Input:</span></span>
```
:> g.V().hasLabel('person').has('firstName', 'Thomas').property('age', 45)
```
<span data-ttu-id="647d8-183">Resultat:</span><span class="sxs-lookup"><span data-stu-id="647d8-183">Output:</span></span>

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

## <a name="query-your-graph"></a><span data-ttu-id="647d8-184">Fråga diagrammet</span><span class="sxs-lookup"><span data-stu-id="647d8-184">Query your graph</span></span>

<span data-ttu-id="647d8-185">Nu ska vi köra en mängd olika frågor mot grafen.</span><span class="sxs-lookup"><span data-stu-id="647d8-185">Now, let's run a variety of queries against your graph.</span></span>

<span data-ttu-id="647d8-186">Först prova med en fråga med ett filter tooreturn endast personer som är äldre än 40 år gammal.</span><span class="sxs-lookup"><span data-stu-id="647d8-186">First, let's try a query with a filter tooreturn only people who are older than 40 years old.</span></span>

<span data-ttu-id="647d8-187">Indata (filterfråga):</span><span class="sxs-lookup"><span data-stu-id="647d8-187">Input (filter query):</span></span>

```
:> g.V().hasLabel('person').has('age', gt(40))
```

<span data-ttu-id="647d8-188">Resultat:</span><span class="sxs-lookup"><span data-stu-id="647d8-188">Output:</span></span>

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

<span data-ttu-id="647d8-189">Därefter går vi project hello förnamn för hello människor som är äldre än 40 år gammal.</span><span class="sxs-lookup"><span data-stu-id="647d8-189">Next, let's project hello first name for hello people who are older than 40 years old.</span></span>

<span data-ttu-id="647d8-190">Indata (filter + projektionsfråga):</span><span class="sxs-lookup"><span data-stu-id="647d8-190">Input (filter + projection query):</span></span>

```
:> g.V().hasLabel('person').has('age', gt(40)).values('firstName')
```

<span data-ttu-id="647d8-191">Resultat:</span><span class="sxs-lookup"><span data-stu-id="647d8-191">Output:</span></span>

```
==>Thomas
```

## <a name="traverse-your-graph"></a><span data-ttu-id="647d8-192">Bläddra i grafen</span><span class="sxs-lookup"><span data-stu-id="647d8-192">Traverse your graph</span></span>

<span data-ttu-id="647d8-193">Vi passerar hello diagram tooreturn alla Thomass vänner.</span><span class="sxs-lookup"><span data-stu-id="647d8-193">Let's traverse hello graph tooreturn all of Thomas's friends.</span></span>

<span data-ttu-id="647d8-194">Indata (Thomas vänner):</span><span class="sxs-lookup"><span data-stu-id="647d8-194">Input (friends of Thomas):</span></span>

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person')
```

<span data-ttu-id="647d8-195">Resultat:</span><span class="sxs-lookup"><span data-stu-id="647d8-195">Output:</span></span> 

```
==>[id:f04bc00b-cb56-46c4-a3bb-a5870c42f7ff,label:person,type:vertex,properties:[firstName:[[id:14feedec-b070-444e-b544-62be15c7167c,value:Mary Kay]],lastName:[[id:107ab421-7208-45d4-b969-bbc54481992a,value:Andersen]],age:[[id:4b08d6e4-58f5-45df-8e69-6b790b692e0a,value:39]]]]
==>[id:91605c63-4988-4b60-9a30-5144719ae326,label:person,type:vertex,properties:[firstName:[[id:f760e0e6-652a-481a-92b0-1767d9bf372e,value:Robin]],lastName:[[id:352a4caa-bad6-47e3-a7dc-90ff342cf870,value:Wakefield]]]]
```

<span data-ttu-id="647d8-196">Nu ska vi gå hello nästa säkerhetslager formhörnen.</span><span class="sxs-lookup"><span data-stu-id="647d8-196">Next, let's get hello next layer of vertices.</span></span> <span data-ttu-id="647d8-197">Passerar hello diagram tooreturn alla hello vänners Thomass vänner.</span><span class="sxs-lookup"><span data-stu-id="647d8-197">Traverse hello graph tooreturn all hello friends of Thomas's friends.</span></span>

<span data-ttu-id="647d8-198">Indata (Thomas vänners vänner):</span><span class="sxs-lookup"><span data-stu-id="647d8-198">Input (friends of friends of Thomas):</span></span>

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```
<span data-ttu-id="647d8-199">Resultat:</span><span class="sxs-lookup"><span data-stu-id="647d8-199">Output:</span></span>

```
==>[id:a801a0cb-ee85-44ee-a502-271685ef212e,label:person,type:vertex,properties:[firstName:[[id:b9489902-d29a-4673-8c09-c2b3fe7f8b94,value:Ben]],lastName:[[id:e084f933-9a4b-4dbc-8273-f0171265cf1d,value:Miller]]]]
```

## <a name="drop-a-vertex"></a><span data-ttu-id="647d8-200">Släppa ett hörn</span><span class="sxs-lookup"><span data-stu-id="647d8-200">Drop a vertex</span></span>

<span data-ttu-id="647d8-201">Vi bort en nod från hello graph-databasen.</span><span class="sxs-lookup"><span data-stu-id="647d8-201">Let's now delete a vertex from hello graph database.</span></span>

<span data-ttu-id="647d8-202">Indata (drop Jack vertex):</span><span class="sxs-lookup"><span data-stu-id="647d8-202">Input (drop Jack vertex):</span></span>

```
:> g.V().hasLabel('person').has('firstName', 'Jack').drop()
```

## <a name="clear-your-graph"></a><span data-ttu-id="647d8-203">Rensa diagrammet</span><span class="sxs-lookup"><span data-stu-id="647d8-203">Clear your graph</span></span>

<span data-ttu-id="647d8-204">Slutligen ska vi Rensa hello databasen alla hörn och kanter.</span><span class="sxs-lookup"><span data-stu-id="647d8-204">Finally, let's clear hello database of all vertices and edges.</span></span>

<span data-ttu-id="647d8-205">Indata:</span><span class="sxs-lookup"><span data-stu-id="647d8-205">Input:</span></span>

```
:> g.E().drop()
:> g.V().drop()
```

<span data-ttu-id="647d8-206">Grattis!</span><span class="sxs-lookup"><span data-stu-id="647d8-206">Congratulations!</span></span> <span data-ttu-id="647d8-207">Du har slutfört den här självstudien om Azure Cosmos DB: Graph API!</span><span class="sxs-lookup"><span data-stu-id="647d8-207">You've completed this Azure Cosmos DB: Graph API tutorial!</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="647d8-208">Granska SLA: er i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="647d8-208">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="647d8-209">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="647d8-209">Clean up resources</span></span>

<span data-ttu-id="647d8-210">Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av denna Snabbstart i hello Azure-portalen med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="647d8-210">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>  

1. <span data-ttu-id="647d8-211">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="647d8-211">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="647d8-212">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="647d8-212">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="647d8-213">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="647d8-213">Next steps</span></span>

<span data-ttu-id="647d8-214">Du har lärt dig hur toocreate ett Azure DB som Cosmos-konto, skapa diagram med hello Data Explorer, skapa hörn och kanter och passerar diagrammet med hello Gremlin konsolen i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="647d8-214">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a graph using hello Data Explorer, create vertices and edges, and traverse your graph using hello Gremlin console.</span></span> <span data-ttu-id="647d8-215">Nu kan du skapa mer komplexa frågor och implementera kraftfull logik för grafbläddring med Gremlin.</span><span class="sxs-lookup"><span data-stu-id="647d8-215">You can now build more complex queries and implement powerful graph traversal logic using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="647d8-216">Fråga med hjälp av Gremlin</span><span class="sxs-lookup"><span data-stu-id="647d8-216">Query using Gremlin</span></span>](tutorial-query-graph.md)
