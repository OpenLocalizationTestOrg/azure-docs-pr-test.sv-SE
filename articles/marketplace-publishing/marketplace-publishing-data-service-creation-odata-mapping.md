---
title: "aaaGuide toocreating en datatjänst för hello Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar hur toocreate, certifiera och distribuera en Data-tjänst för att köpa på hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3a632825-db5b-49ec-98bd-887138798bc4
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: deb2e52dd03f5beb2ad6a927bd2d03e47d20b691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mapping-an-existing-web-service-tooodata-through-csdl"></a><span data-ttu-id="f42e7-103">Mappning av en befintlig web service tooOData via CSDL</span><span class="sxs-lookup"><span data-stu-id="f42e7-103">Mapping an existing web service tooOData through CSDL</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f42e7-104">**Just nu vi inte längre onboarding några nya Data Service-utgivare. Nya dataservices kommer inte godkännas för lista.**</span><span class="sxs-lookup"><span data-stu-id="f42e7-104">**At this time we are no longer onboarding any new Data Service publishers. New dataservices will not get approved for listing.**</span></span> <span data-ttu-id="f42e7-105">Om du har ett SaaS-affärsprogram som toopublish på AppSource hittar du mer information [här](https://appsource.microsoft.com/partners).</span><span class="sxs-lookup"><span data-stu-id="f42e7-105">If you have a SaaS business application you would like toopublish on AppSource you can find more information [here](https://appsource.microsoft.com/partners).</span></span> <span data-ttu-id="f42e7-106">Om du har en IaaS-program eller utvecklare tjänst du skulle t.ex toopublish på Azure Marketplace hittar du mer information [här](https://azure.microsoft.com/marketplace/programs/certified/).</span><span class="sxs-lookup"><span data-stu-id="f42e7-106">If you have an IaaS applications or developer service you would like toopublish on Azure Marketplace you can find more information [here](https://azure.microsoft.com/marketplace/programs/certified/).</span></span>
> 
> 

<span data-ttu-id="f42e7-107">Den här artikeln ger en översikt över om hur toouse CSDL-toomap en befintlig service tooan kompatibel OData-tjänst.</span><span class="sxs-lookup"><span data-stu-id="f42e7-107">This article gives an overview on how toouse a CSDL toomap an existing service tooan OData compatible service.</span></span> <span data-ttu-id="f42e7-108">Den förklarar hur toocreate hello mappning dokumentet (CSDL) som omvandlar hello inkommande begäran från hello klient via en samtalet och hello utdata (data) tillbaka toohello klienten via en kompatibel OData-feed.</span><span class="sxs-lookup"><span data-stu-id="f42e7-108">It explains how toocreate hello mapping document (CSDL) that transforms hello input request from hello client via a service call and hello output (data) back toohello client via an OData compatible feed.</span></span> <span data-ttu-id="f42e7-109">Microsoft Azure Marketplace visar services toohello slutanvändare med hjälp av hello OData-protokollet.</span><span class="sxs-lookup"><span data-stu-id="f42e7-109">Microsoft’s Azure Marketplace exposes services toohello end-users by using hello OData protocol.</span></span> <span data-ttu-id="f42e7-110">Tjänster som exponeras av innehållsleverantörer (dataägare) visas i flera olika former REST, SOAP, t.ex.</span><span class="sxs-lookup"><span data-stu-id="f42e7-110">Services that are exposed by content providers (Data Owners) are exposed in a variety of forms, such as REST, SOAP, etc.</span></span>

## <a name="what-is-a-csdl-and-its-structure"></a><span data-ttu-id="f42e7-111">Vad är en CSDL och dess struktur?</span><span class="sxs-lookup"><span data-stu-id="f42e7-111">What is a CSDL and its structure?</span></span>
<span data-ttu-id="f42e7-112">En CSDL (konceptuellt Schema Definition Language) är en specifikation som definierar hur toodescribe webbtjänst eller databas tjänsten gemensamma XML ordflöde toohello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f42e7-112">A CSDL (Conceptual Schema Definition Language) is a specification defining how toodescribe web service or database service in common XML verbiage toohello Azure Marketplace.</span></span>

<span data-ttu-id="f42e7-113">Enkel översikt över hello **flödet för begäran:**</span><span class="sxs-lookup"><span data-stu-id="f42e7-113">Simple overview of hello **request flow:**</span></span>

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

<span data-ttu-id="f42e7-114">Hej **dataflöde** i hello motsatt riktning:</span><span class="sxs-lookup"><span data-stu-id="f42e7-114">hello **data flow** is in hello opposite direction:</span></span>

  `Client <- Azure Marketplace <- Content Provider’s WebService`

<span data-ttu-id="f42e7-115">**Bild 1** diagram en klient skulle hur hämta data från en innehållsleverantör (din tjänst) genom att gå via hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f42e7-115">**Figure 1** diagrams how a client would obtain data from a content provider (your service) by going through hello Azure Marketplace.</span></span>  <span data-ttu-id="f42e7-116">Hej CSDL används av hello mappning-transformering komponenten toohandle hello begäran och data som skickas mellan hello innehållsleverantören tjänster och hello begär klienten.</span><span class="sxs-lookup"><span data-stu-id="f42e7-116">hello CSDL is used by hello Mapping/Transformation component toohandle hello request and data pass between hello content provider’s service(s) and hello requesting client.</span></span>

<span data-ttu-id="f42e7-117">*Bild 1: Detaljerad flödar från begär klienten toocontent provider via Azure Marketplace*</span><span class="sxs-lookup"><span data-stu-id="f42e7-117">*Figure 1: Detailed flow from requesting client toocontent provider via Azure Marketplace*</span></span>

  ![Rita](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

<span data-ttu-id="f42e7-119">Bakgrundsinformation om Atom och Atom Pub hello OData-protokollet vid vilka hello Azure Marketplace tillägg build granska: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)</span><span class="sxs-lookup"><span data-stu-id="f42e7-119">For background on Atom, Atom Pub, and hello OData protocol upon which hello Azure Marketplace extensions build, please review: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)</span></span>

<span data-ttu-id="f42e7-120">Utdrag ovan länk: *”hello hello Open Data protocol (som anges nedan tooas OData) syftar tooprovide en REST-baserat protokoll för CRUD-format åtgärder (skapa, läsa, uppdatera och ta bort) mot resurser visas som data services. En ”datatjänst” är en slutpunkt har angett namn / värde-par där det finns data exponeras från en eller flera ”samlingar” varje med noll eller flera ”poster”, som består av. OData har publicerats av Microsoft under OASIS (organisation för hello framflyttning för strukturerade Information Standards) standarder så att vem som helst som vill toocan skapa servrar, klienter eller verktyg utan royalty eller begränsningar ”.*</span><span class="sxs-lookup"><span data-stu-id="f42e7-120">Excerpt from above link: *“hello purpose of hello Open Data protocol (hereafter referred tooas OData) is tooprovide a REST-based protocol for CRUD-style operations (Create, Read, Update and Delete) against resources exposed as data services. A “data service” is an endpoint where there is data exposed from one or more “collections” each with zero or more “entries”, which consist of typed named-value pairs. OData is published by Microsoft under OASIS (Organization for hello Advancement of Structured Information Standards) Standards so that anyone that wants toocan build servers, clients or tools without royalties or restrictions.”*</span></span>

### <a name="three-critical-pieces-that-have-toobe-defined-by-hello-csdl-are"></a><span data-ttu-id="f42e7-121">Tre viktiga uppgifter som har toobe som definieras av hello CSDL är:</span><span class="sxs-lookup"><span data-stu-id="f42e7-121">Three Critical Pieces that have toobe defined by hello CSDL are:</span></span>
* <span data-ttu-id="f42e7-122">Hej **endpoint** hello Web adress (URI) av hello Service av hello-leverantör</span><span class="sxs-lookup"><span data-stu-id="f42e7-122">hello **endpoint** of hello Service Provider hello Web Address (URI) of hello Service</span></span>
* <span data-ttu-id="f42e7-123">Hej **dataparametrar** som skickas som indata toohello tjänstleverantör hello definitioner av hello parametrar skickas toohello innehållsleverantören service ned toohello datatyp.</span><span class="sxs-lookup"><span data-stu-id="f42e7-123">hello **data parameters** being passed as input toohello Service Provider hello definitions of hello parameters being sent toohello Content Provider’s service down toohello data type.</span></span>
* <span data-ttu-id="f42e7-124">**Schemat** hello data som returneras toohello begär Service hello schemat för hello som levereras av hello innehållsleverantören tjänsten, inklusive behållare, samlingar-tabeller, variabler eller kolumner och datatyper.</span><span class="sxs-lookup"><span data-stu-id="f42e7-124">**Schema** of hello data being returned toohello Requesting Service hello schema of hello data being delivered by hello Content Provider’s service, including Container, collections/tables, variables/columns, and data types.</span></span>

<span data-ttu-id="f42e7-125">hello följande diagram visar en översikt över hello flödet, från där hello-klienten anger hello OData-uttryck (anropet toohello Innehållsleverantörens webbtjänst) toogetting hello/Resultatdata tillbaka.</span><span class="sxs-lookup"><span data-stu-id="f42e7-125">hello following diagram shows an overview of hello flow from where hello client enters hello OData statement (call toohello content provider’s web service) toogetting hello results/data back.</span></span>

  ![Rita](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a><span data-ttu-id="f42e7-127">Så här:</span><span class="sxs-lookup"><span data-stu-id="f42e7-127">Steps:</span></span>
1. <span data-ttu-id="f42e7-128">Klienten skickar begäran via tjänsten anropet slutfördes med indataparametrar som definierats i XML-toohello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="f42e7-128">Client sends request via Service call complete with Input Parameters defined in XML toohello Azure Marketplace</span></span>
2. <span data-ttu-id="f42e7-129">CSDL är används toovalidate hello anropet till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f42e7-129">CSDL is used toovalidate hello Service call.</span></span>
   * <span data-ttu-id="f42e7-130">hello skickas formaterad Service anropa sedan toohello innehåll Providers Service av hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="f42e7-130">hello Formatted Service Call is then sent toohello Content Providers Service by hello Azure Marketplace</span></span>
3. <span data-ttu-id="f42e7-131">hello Webservice körs och preforms hello åtgärden hello Http-Verb (d.v.s. får) hello data returneras tooAzure Marketplace där hello begärde data (eventuella) är visar i XML-Format toohello klienten med hjälp av hello mappning som definierats i hello CSDL.</span><span class="sxs-lookup"><span data-stu-id="f42e7-131">hello Webservice executes and preforms hello action of hello Http Verb (i.e. GET) hello data is returned tooAzure Marketplace where hello requested data (if any) is exposes in XML Format toohello Client using hello Mapping defined in hello CSDL.</span></span>
4. <span data-ttu-id="f42e7-132">hello klienten skickas hello data (om sådan finns) i XML- eller JSON-format</span><span class="sxs-lookup"><span data-stu-id="f42e7-132">hello Client is sent hello data (if any) in XML or JSON format</span></span>

## <a name="definitions"></a><span data-ttu-id="f42e7-133">Definitioner</span><span class="sxs-lookup"><span data-stu-id="f42e7-133">Definitions</span></span>
### <a name="odata-atom-pub"></a><span data-ttu-id="f42e7-134">OData-ATOM pub</span><span class="sxs-lookup"><span data-stu-id="f42e7-134">OData ATOM pub</span></span>
<span data-ttu-id="f42e7-135">Ange ett tillägg toohello ATOM pub där varje post representerar en rad med ett resultat.</span><span class="sxs-lookup"><span data-stu-id="f42e7-135">An extension toohello ATOM pub where each entry represents one row of a result set.</span></span> <span data-ttu-id="f42e7-136">hello innehåll en del av hello transaktionen är förbättrad toocontain hello värden för hello rad – som nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="f42e7-136">hello content part of hello entry is enhanced toocontain hello values of hello row – as key value pairs.</span></span> <span data-ttu-id="f42e7-137">Mer information finns här: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)</span><span class="sxs-lookup"><span data-stu-id="f42e7-137">More information is found here: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)</span></span>

### <a name="csdl---conceptual-schema-definition-language"></a><span data-ttu-id="f42e7-138">CSDL - Conceptual Schema Definition Language</span><span class="sxs-lookup"><span data-stu-id="f42e7-138">CSDL - Conceptual Schema Definition Language</span></span>
<span data-ttu-id="f42e7-139">Här kan du definiera funktioner (SPROCs) och de entiteter som är tillgängliga via en databas.</span><span class="sxs-lookup"><span data-stu-id="f42e7-139">Allows defining functions (SPROCs) and entities that are exposed through a database.</span></span> <span data-ttu-id="f42e7-140">Mer information finns här: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)</span><span class="sxs-lookup"><span data-stu-id="f42e7-140">More information found here: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)</span></span>  

> [!TIP]
> <span data-ttu-id="f42e7-141">Klicka på hello **andra versioner** listrutan och väljer en version om du inte ser hello artikel.</span><span class="sxs-lookup"><span data-stu-id="f42e7-141">Click hello **other versions** dropdown and select a version if you don’t see hello article.</span></span>
> 
> 

### <a name="edm---entry-data-model"></a><span data-ttu-id="f42e7-142">EDM - post-datamodell</span><span class="sxs-lookup"><span data-stu-id="f42e7-142">EDM - Entry Data Model</span></span>
* <span data-ttu-id="f42e7-143">Översikt: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink]</span><span class="sxs-lookup"><span data-stu-id="f42e7-143">Overview: [http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx][OverviewLink]</span></span>

[OverviewLink]:http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx
* <span data-ttu-id="f42e7-144">Förhandsversion: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink]</span><span class="sxs-lookup"><span data-stu-id="f42e7-144">Preview: [http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx][PreviewLink]</span></span>

[PreviewLink]:http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx
* <span data-ttu-id="f42e7-145">Datatyper: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink]</span><span class="sxs-lookup"><span data-stu-id="f42e7-145">Data types: [http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx][DataTypesLink]</span></span>

[DataTypesLink]:http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx

<span data-ttu-id="f42e7-146">hello följande visar hello detaljerad vänstra tooRight flödar från där hello-klienten anger hello OData-uttryck (anropet toohello Innehållsleverantörens webbtjänst) toogetting hello/Resultatdata tillbaka:</span><span class="sxs-lookup"><span data-stu-id="f42e7-146">hello following shows hello detailed Left tooRight flow from where hello client enters hello OData statement (call toohello content provider’s web service) toogetting hello results/data back:</span></span>

  ![Rita](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)

## <a name="csdl-basics"></a><span data-ttu-id="f42e7-148">CSDL-grunderna</span><span class="sxs-lookup"><span data-stu-id="f42e7-148">CSDL Basics</span></span>
<span data-ttu-id="f42e7-149">En CSDL (konceptuellt Schema Definition Language) är en specifikation som definierar hur toodescribe webbtjänst eller databas tjänsten gemensamma XML ordflöde toohello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f42e7-149">A CSDL (Conceptual Schema Definition Language) is a specification defining how toodescribe web service or database service in common XML verbiage toohello Azure Marketplace.</span></span> <span data-ttu-id="f42e7-150">CSDL beskriver hello kritiska delar som **gör det möjligt hello överföring av data från hello datakällan toohello Azure Marketplace.**</span><span class="sxs-lookup"><span data-stu-id="f42e7-150">CSDL describes hello critical pieces that **makes hello passing of data from hello Data Source toohello Azure Marketplace possible.**</span></span> <span data-ttu-id="f42e7-151">Här beskrivs hello viktigaste delarna:</span><span class="sxs-lookup"><span data-stu-id="f42e7-151">hello main pieces are described here:</span></span>

* <span data-ttu-id="f42e7-152">Gränssnittsinformation som beskriver alla offentligt tillgängliga funktioner (FunctionImport nod)</span><span class="sxs-lookup"><span data-stu-id="f42e7-152">Interface information describing all publicly available functions (FunctionImport Node)</span></span>
* <span data-ttu-id="f42e7-153">Information om datatyp för alla requests(input) meddelandet och meddelandet responses(outputs) (EntityType-EntityContainer/EntitySet noder)</span><span class="sxs-lookup"><span data-stu-id="f42e7-153">Data type information for all message requests(input) and message responses(outputs) (EntityContainer/EntitySet/EntityType Nodes)</span></span>
* <span data-ttu-id="f42e7-154">Bindningsinformation om hello transport protocol toobe används (sidhuvud nod)</span><span class="sxs-lookup"><span data-stu-id="f42e7-154">Binding information about hello transport protocol toobe used (Header Node)</span></span>
* <span data-ttu-id="f42e7-155">Information om för att hitta hello anges service (BaseURI attribut)</span><span class="sxs-lookup"><span data-stu-id="f42e7-155">Address information for locating hello specified service (BaseURI attribute)</span></span>

<span data-ttu-id="f42e7-156">I kort sagt kan representerar hello CSDL ett kontrakt och språk-plattformsoberoende mellan hello service begärande och hello-leverantör.</span><span class="sxs-lookup"><span data-stu-id="f42e7-156">In a nutshell, hello CSDL represents a platform- and language-independent contract between hello service requestor and hello service provider.</span></span> <span data-ttu-id="f42e7-157">Med hello CSDL kan en klient Leta upp en webbtjänst för service-databasen och anropa en av dess offentligt tillgängliga funktioner.</span><span class="sxs-lookup"><span data-stu-id="f42e7-157">Using hello CSDL, a client can locate a web service/database service and invoke any of its publicly available functions.</span></span>

### <a name="relating-a-csdl-tooa-database-or-a-collection"></a><span data-ttu-id="f42e7-158">Om en CSDL-tooa databas eller en samling</span><span class="sxs-lookup"><span data-stu-id="f42e7-158">Relating a CSDL tooa Database or a Collection</span></span>
<span data-ttu-id="f42e7-159">**Hej CSDL-specifikationen**</span><span class="sxs-lookup"><span data-stu-id="f42e7-159">**hello CSDL Specification**</span></span>

<span data-ttu-id="f42e7-160">CSDL är XML-grammatiken för att beskriva en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="f42e7-160">CSDL is XML grammar for describing a web service.</span></span> <span data-ttu-id="f42e7-161">hello-specifikationen själva är uppdelad i 4 viktiga element: EntitySet, FunctionImport; Namnområdet och EntityType.</span><span class="sxs-lookup"><span data-stu-id="f42e7-161">hello specification itself is divided into 4 major elements:  EntitySet, FunctionImport; NameSpace, and EntityType.</span></span>

<span data-ttu-id="f42e7-162">toomake detta enklare toounderstand abstraction kan relatera en CSDL tooa tabell.</span><span class="sxs-lookup"><span data-stu-id="f42e7-162">toomake this abstraction easier toounderstand lets relate a CSDL tooa table.</span></span>

<span data-ttu-id="f42e7-163">Kom ihåg;</span><span class="sxs-lookup"><span data-stu-id="f42e7-163">Remember;</span></span>

  <span data-ttu-id="f42e7-164">CSDL representerar en plattform och språkoberoende avtal mellan hello **service begärande** och hello **tjänstleverantör**.</span><span class="sxs-lookup"><span data-stu-id="f42e7-164">CSDL represents a platform- and language-independent contract between hello **service requestor** and hello **service provider**.</span></span> <span data-ttu-id="f42e7-165">Med hjälp av CSDL, en **klienten** kan hitta en **webbtjänsten tjänstdatabasen/** och anropa en av dess offentligt tillgängliga **funktioner.**</span><span class="sxs-lookup"><span data-stu-id="f42e7-165">Using CSDL, a **client** can locate a **web service/database service** and invoke any of its publicly available **functions.**</span></span>

<span data-ttu-id="f42e7-166">För en datatjänst hello kan fyra delar av en CSDL betraktas som en databas, tabeller, kolumner och Store-procedur.</span><span class="sxs-lookup"><span data-stu-id="f42e7-166">For a Data Service hello four parts of a CSDL can be thought of in terms of a Database, Table, Column, and Store Procedure.</span></span>

<span data-ttu-id="f42e7-167">Om dessa på följande sätt för en datatjänst:</span><span class="sxs-lookup"><span data-stu-id="f42e7-167">Relating these as follows for a Data Service:</span></span>

* <span data-ttu-id="f42e7-168">EntityContainer ~ = databas</span><span class="sxs-lookup"><span data-stu-id="f42e7-168">EntityContainer  ~=  Database</span></span>
* <span data-ttu-id="f42e7-169">EntitySet ~ = tabell</span><span class="sxs-lookup"><span data-stu-id="f42e7-169">EntitySet  ~=  Table</span></span>
* <span data-ttu-id="f42e7-170">EntityType ~ = kolumner</span><span class="sxs-lookup"><span data-stu-id="f42e7-170">EntityType  ~= Columns</span></span>
* <span data-ttu-id="f42e7-171">FunctionImport ~ = lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="f42e7-171">FunctionImport  ~=  Stored Procedure</span></span>

<span data-ttu-id="f42e7-172">**HTTP-verb tillåts**</span><span class="sxs-lookup"><span data-stu-id="f42e7-172">**HTTP Verbs allowed**</span></span>

* <span data-ttu-id="f42e7-173">Hämta – returnerar värden från hello db (returnerar en mängd)</span><span class="sxs-lookup"><span data-stu-id="f42e7-173">GET – returns values from hello db (returns a Collection)</span></span>
* <span data-ttu-id="f42e7-174">POST – används toopass data tooand valfria returvärden från hello db (skapa en ny post i hello samling, return-id eller-URI)</span><span class="sxs-lookup"><span data-stu-id="f42e7-174">POST – used toopass data tooand optional return values from hello db (Create a new entry in hello collection, return id/URI)</span></span>
* <span data-ttu-id="f42e7-175">Ta bort – tar bort data från hello DB (tar bort en samling)</span><span class="sxs-lookup"><span data-stu-id="f42e7-175">DELETE – Deletes data from hello DB (Deletes a collection)</span></span>
* <span data-ttu-id="f42e7-176">PLACERA – uppdatera data i en DB (ersätta en samling eller skapa ett)</span><span class="sxs-lookup"><span data-stu-id="f42e7-176">PUT – Update data into a DB (replace a collection or create one)</span></span>

## <a name="metadatamapping-document"></a><span data-ttu-id="f42e7-177">Metadata-mappning dokumentet</span><span class="sxs-lookup"><span data-stu-id="f42e7-177">Metadata/Mapping Document</span></span>
<span data-ttu-id="f42e7-178">hello metadata-mappning dokumentet är används toomap en innehållsleverantör befintliga webbtjänster så att den kan visas som en OData-webbtjänst hello Azure Marketplace-systemet.</span><span class="sxs-lookup"><span data-stu-id="f42e7-178">hello metadata/mapping document is used toomap a content provider’s existing web services so that it can be exposed as an OData web service by hello Azure Marketplace system.</span></span> <span data-ttu-id="f42e7-179">Den är baserad på CSDL och implementerar några tillägg tooCSDL tooaccommodate hello behov REST baserat webbtjänsterna som exponeras via Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f42e7-179">It is based on CSDL and implements a few extensions tooCSDL tooaccommodate hello needs of REST based web services exposed through Azure Marketplace.</span></span> <span data-ttu-id="f42e7-180">hello-tillägg finns i hello [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) namnområde.</span><span class="sxs-lookup"><span data-stu-id="f42e7-180">hello extensions are found in hello [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) namespace.</span></span>

<span data-ttu-id="f42e7-181">Ett exempel på hello CSDL visas nedan: (kopiera och klistra in hello nedan exempel CSDL till en XML-redigerare och ändra toomatch din tjänst.</span><span class="sxs-lookup"><span data-stu-id="f42e7-181">An example of hello CSDL follows:  (Copy and paste hello below example CSDL into an XML editor and change toomatch your Service.</span></span>  <span data-ttu-id="f42e7-182">Klistra in i CSDL mappning DataService fliken när du skapar din tjänst i hello [Azure Marketplace Publishing Portal](https://publish.windowsazure.com)).</span><span class="sxs-lookup"><span data-stu-id="f42e7-182">Then paste into CSDL Mapping under DataService tab when creating your service in hello  [Azure Marketplace Publishing Portal](https://publish.windowsazure.com)).</span></span>

<span data-ttu-id="f42e7-183">**Villkor:** hänför sig hello CSDL villkor toohello [Publiceringsportal](https://publish.windowsazure.com) Användargränssnittet (PPUI) villkor.</span><span class="sxs-lookup"><span data-stu-id="f42e7-183">**Terms:** Relating hello CSDL terms toohello [Publishing Portal](https://publish.windowsazure.com) UI (PPUI) terms.</span></span>

* <span data-ttu-id="f42e7-184">Erbjuder ”Title” i hello PPUI relaterar tooMyWebOffer</span><span class="sxs-lookup"><span data-stu-id="f42e7-184">Offer “Title” in hello PPUI relates tooMyWebOffer</span></span>
* <span data-ttu-id="f42e7-185">Mitt företag i hello PPUI gäller för**Publisher visningsnamn** i hello [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) UI</span><span class="sxs-lookup"><span data-stu-id="f42e7-185">MyCompany in hello PPUI relates too**Publisher Display Name** in hello [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) UI</span></span>
* <span data-ttu-id="f42e7-186">Din API relaterad tooa webb- eller Data-tjänsten (en Plan i hello PPUI)</span><span class="sxs-lookup"><span data-stu-id="f42e7-186">Your API relates tooa Web or Data Service (a Plan in hello PPUI)</span></span>

<span data-ttu-id="f42e7-187">**Hierarki:** ett företag (innehållsleverantören) äger erbjudanden som har plan eller de planer, nämligen service(s) vilken rad upp med ett API.</span><span class="sxs-lookup"><span data-stu-id="f42e7-187">**Hierarchy:** A Company (Content Provider) owns Offer(s) which have Plan(s), namely service(s), which line up with an API.</span></span>

### <a name="webservice-csdl-example"></a><span data-ttu-id="f42e7-188">Webbtjänsten CSDL-exempel</span><span class="sxs-lookup"><span data-stu-id="f42e7-188">WebService CSDL Example</span></span>
<span data-ttu-id="f42e7-189">Ansluter tooa tjänst som visar en web application slutpunkt (t.ex. en C#-program)</span><span class="sxs-lookup"><span data-stu-id="f42e7-189">Connects tooa service that is exposing an web application endpoint (like a C# application)</span></span>

        <?xml version="1.0" encoding="utf-8"?>
        <!-- hello namespace attribute below is used by our system toogenerate C#. You can change “MyCompany.MyOffer” toosomething that makes sense for you, but change “MyOffer” consistently throughout hello document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all hello web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in hello customer facing UI as name of hello Service.
        @EntityType is used toopoint at hello type definition near hello bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is hello entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines hello service call, replace & with hello encode value (&amp;).
        In hello input name value pairs {param} represents passed in value.
        Or hello value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows hello CSDL toospecifically specify hello verb of hello service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes hello parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how hello web service call is exposed toomarketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, hello {...} point toohello parameters defined below.
        Marketplace will read hello parameters from this URITemplate and fill hello values into hello corresponding parameters of hello BaseUri or RequestBody (below) during calls tooyour service.  
        It is okay for @d:BaseUri tooinclude information only for Marketplace consumption, it will not be exposed tooend users. i.e. hello hardcoded AccountKey in hello above BaseURI does not show up in hello client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of hello RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders tooinsert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how toopass userid and password via hello header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed tooend users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with hello value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited tooappearing as hello value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used toospecify values tooinsert into hello ATOM feed returned toohello end user.  You can specify hello element toocontain a fixed message by providing a value for hello element (this is hello default value).  @d:Map is an XPath expression that points into hello response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay tooalso add a d:Map attribute toooverride this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of hello web service call:
        @Name should match exactly (case sensitive) hello {…} placeholders in hello @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is hello EDM.SimpleType of hello parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether hello parameter is required.
        @d:Regex - optional, attribute toodescribe hello string, limiting unwanted input at hello entry of hello system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in hello Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating hello service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in hello order listed.
        @d:Match is an Xpath query on hello service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is hello error code tooreturn if an response matches hello error.
        @d:errorMessage is hello user friendly message tooreturn when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect toohello service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- hello EntityContainer defines hello output data schema -->
        </EntityContainer>
        <!-- hello EntityType @d:Map defines hello repeating node (an XPath query) in hello response (output data schema). -->
        <!-- If these nodes are outside a namespace, add hello prefix in hello xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in hello ATOM feed, so comply with hello XML element naming restrictions (no spaces or other illegal characters).
        @Type is hello EDM.SimpleType of hello parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query toopoint at hello location tooextract hello content from your services response.
        hello "." is relative toohello repeating node in hello EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID"    d:IsPrimaryKey="True" Type="Int32"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount"    Type="Double"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"    Type="String"    Nullable="false" d:Map="./City"/>
        <Property Name="State"    Type="String"    Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime"    Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [!TIP]
> <span data-ttu-id="f42e7-190">Visa fler CSDL Web Service-exempel i hello artikel [exempel för att matcha en befintlig web service tooOData via CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)</span><span class="sxs-lookup"><span data-stu-id="f42e7-190">View more CSDL Web Service examples in hello article [Examples of mapping an existing web service tooOData through CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)</span></span>
> 
> 

### <a name="dataservice-csdl-example"></a><span data-ttu-id="f42e7-191">DataService CSDL-exempel</span><span class="sxs-lookup"><span data-stu-id="f42e7-191">DataService CSDL Example</span></span>
<span data-ttu-id="f42e7-192">Ansluter tooa tjänst som Exponerar en databastabell eller vy som en slutpunkt som exemplet nedan visar två API: er för databas baserat API CSDL (kan använda vyer i stället för tabeller).</span><span class="sxs-lookup"><span data-stu-id="f42e7-192">Connects tooa service that is exposing a database table or view as an endpoint Below example shows two APIs for Data base based API CSDL (can use views rather than tables).</span></span>

        <?xml version="1.0"?>
        <!-- hello namespace attribute below is used by our system toogenerate C#. You can change “MyCompany.MyOffer” toosomething that makes sense for you, but change “MyOffer” consistently throughout hello document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all hello data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of hello EntitySet as a Service
        @Name is used in hello customer facing UI as name of hello Service.
        @EntityType is used toopoint at hello type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); hello table (or view) and columns toobe returned by hello data service.)
            Map is hello schema.tabel or schema.view
            dals.TableName is hello table Name
            Name is hello name identifier for hello EntityType and hello Name of hello service exposed toohello client via hello UI.
            dals:IsExposed determines if hello table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is hello schema name of hello table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines hello column properties and hello output of hello service.
            dals:ColumnName is hello name of hello column in hello table /view.
            Type is hello emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is hello maximum length of hello output value
            Name is hello name of hello Property and is exposed toohello client facing UI
            dals:IsReturned is hello Boolean that determines if hello Service exposes this value toohello client.
            IsQueryable is hello Boolean that determines if hello column can be used in a database query
            (For data Services: tooimprove Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is hello numerical position x in hello table or hello View, where x is from 1 toohello number of columns in hello table.
            dals:DatabaseDataType is hello data type of hello column in hello database, i.e. SQL data type dals:IsPrimaryKey indicates if hello column is hello Primary key in hello table/view.  (hello columns marked ISPrimaryKey are used in hello Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a><span data-ttu-id="f42e7-193">Se även</span><span class="sxs-lookup"><span data-stu-id="f42e7-193">See Also</span></span>
* <span data-ttu-id="f42e7-194">Om du är intresserad av utbildning och förstå hello specifika noder och deras parametrar kan den här artikeln [mappa Data Service OData-noder](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definitioner och förklaringar, exempel och Använd case kontext.</span><span class="sxs-lookup"><span data-stu-id="f42e7-194">If you are interested in learning and understanding hello specific nodes and their parameters, read this article [Data Service OData Mapping Nodes](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) for definitions and explanations, examples, and use case context.</span></span>
* <span data-ttu-id="f42e7-195">Om du vill granska exempel den här artikeln [mappa Data Service OData-exempel](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee exempelkoden och förstå kodsyntax och kontext.</span><span class="sxs-lookup"><span data-stu-id="f42e7-195">If you are interested in reviewing examples, read this article [Data Service OData Mapping Examples](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee sample code and understand code syntax and context.</span></span>
* <span data-ttu-id="f42e7-196">tooreturn toohello föreskrivs sökväg för att publicera en datatjänst toohello Azure Marketplace som den här artikeln [Data Service publiceringsguide](marketplace-publishing-data-service-creation.md).</span><span class="sxs-lookup"><span data-stu-id="f42e7-196">tooreturn toohello prescribed path for publishing a Data Service toohello Azure Marketplace, read this article [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).</span></span>

